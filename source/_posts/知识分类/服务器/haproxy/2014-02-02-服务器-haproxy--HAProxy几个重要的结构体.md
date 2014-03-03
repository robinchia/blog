---
layout: post
title: "HAProxy几个重要的结构体"
categories: 服务器
tags: 
 - 服务器
 - haproxy
--- 

# HAProxy几个重要的结构体

# [HAProxy几个重要的结构体](http://tech.uc.cn/?p=1738)

上一篇文章《[HAProxy的event_accept函数源码分析](http://tech.uc.cn/?p=1523)》（以下简称上文）理顺了HAProxy接收客户端连接的主要流程，遗留下相关的基础设施和数据结构没有深入分析，这一篇先介绍几个重要的结构体。

另外要说明的是，本系列文章对HAProxy的分析都基于目前的稳定版本HAProxy 1.4，而目前的开发版本HAProxy 1.5和1.4相比，重构力度比较大，不少函数名称和结构体都发生变化，但是关键流程还是基本一致的，请各位读者留意。

# 1. session

上文讲的其实就是HAProxy怎样在客户端和服务端之间建立一个完整的链路，相关信息都保存到一个session结构体中。原本的session结构体有80行，可以说非常冗长。和上文思路一样，本着不追求旁枝末节，以较小的代价描述HAProxy原理的原则，我把它进行简化，去掉了HTTP处理相关的成员和一些实现额外功能的成员，只把必不可少的成员列出来：

1

2
3

4
5

6
7

8
9

10
11

12
13 struct session {

    struct list list;         /* position in global sessions list */
    struct task *task;        /* the task associated with this session */

    int conn_retries;         /* number of connect retries left */
    int flags;                /* some flags describing the session */

    struct buffer *req;       /* request buffer */
    struct buffer *rep;       /* response buffer */

    struct stream_interface si[2];     /* client and server stream interfaces */
    struct sockaddr_storage cli_addr;  /* the client address */

    struct sockaddr_in srv_addr;       /* the address to connect to */
    struct server *srv;       /* the server the session will be running or has been running on */

    struct server *prev_srv;  /* the server the was running on, after a redispatch, otherwise NULL */
};

其中，task、req、rep和si在上文已经有所提及，下面也会有进一步解释；conn_retries、flags、cli_addr和srv_addr甚至不用看注释就明白其意义；srv和prev_srv也很明显，一个指向将要执行或正在执行的后端服务器结构体，一个指向上一次曾经执行的后端服务器结构体。

list也是一个结构体，包含n和p两个指向struct list类型的指针，详见mini-clist.h。它在这里的作用是把各个session结构体串起来，形成 一个双向链表，如下图所示。这是包括HAProxy和Linux内核广泛使用的一种数据结构。我认为用在这里倒不是很必要，不过仍然是一种值得学习的技巧。

[![haproxy_sessions]()](http://tech.uc.cn/wp-content/uploads/2013/07/haproxy_sessions.png)

值得注意的是，这种链表是嵌入到各个结构体中使用，p和n两个指针成员各自指向前、后节点节点的list成员，而非前、后节点本身。

# 2. task

task结构体的主要成员如下：

1

2
3

4
5

6
7

8 struct task {

    struct eb32_node wq;  /* ebtree node used to hold the task in the wait queue */
    struct eb32_node rq;  /* ebtree node used to hold the task in the run queue */

    int state;            /* task state : bit field of TASK_* */
    int expire;           /* next expiration date for this task, in ticks */

    struct task * (*process)(struct task *t);  /* the function which processes the task */
    void *context;        /* the task's context */

};

其中，state和expire都是顾名思义；process是函数指针，根据task的类型会执行不同的处理函数，以后的文章再叙述；context指向与task关联的结构体，在event_accept函数中，context指向的就是session。

重点讲一下wq和rq，它们是两个使用32位键的ebtree节点（以下简称ebnode），分别代表该task与等待队列和运行队列的关系。在第一篇文章《[HAProxy的独门武器：ebtree](http://tech.uc.cn/?p=1031)》中，因为没有具体例子印证，有一个关于ebnode的特点没有交代：一个ebnode在ebtree中，当且仅当该ebnode的leaf_p不为空。这一点可以从ebnode的插入、删除操作中推导得出。

因此，判断一个task是否在运行队列中就是：

1

2
3

4 static inline int task_in_rq(struct task *t)

{
    return t->rq.node.leaf_p != NULL;

}

同样，判断一个task是否在等待队列中就是：

1

2
3

4 static inline int task_in_wq(struct task *t)

{
    return t->wq.node.leaf_p != NULL;

}

从运行队列中删除一个task：

1

2
3

4
5 static inline struct task *__task_unlink_rq(struct task *t)

{
    eb32_delete(&t->rq);

    return t;
}

从等待队列中删除一个task，这里有个优化，last_timer指向最后一个入队列的task（稍后再介绍）：

1

2
3

4
5

6
7 static inline struct task *__task_unlink_wq(struct task *t)

{
    eb32_delete(&t->wq);

    if (last_timer == &t->wq)
        last_timer = NULL;

    return t;
}

添加task到运行队列，也就是唤醒该task，上文的event_accept函数，最终也是会执行到这里：

1

2
3

4
5

6
7

8 extern struct task *__task_wakeup(struct task *t)

{
    t->rq.key = ++rqueue_ticks;

    // clear state flags at the same time
    t->state &= ~TASK_WOKEN_ANY;

    eb32_insert(&rqueue, &t->rq);
    return t;

}

添加task到等待队列，也就是让该task排队，乍看上去有点复杂，其实核心部分就只有两三句，关于last_timer的都是一些优化工作，使得HAProxy更快地找到插入的地方：

1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
17

18
19

20
21

22
23

24 extern void __task_queue(struct task *task)

{
    if (likely(task_in_wq(task)))

        __task_unlink_wq(task);
 

    /* the task is not in the queue now */
    task->wq.key = task->expire;

 
    if (likely(last_timer &&

           last_timer->node.bit < 0 &&
           last_timer->key == task->wq.key &&

           last_timer->node.node_p)) {
        eb_insert_dup(&last_timer->node, &task->wq.node);

        if (task->wq.node.bit < last_timer->node.bit)
            last_timer = &task->wq;

        return;
    }

    eb32_insert(&timers, &task->wq);
 

    /* Make sure we don't assign the last_timer to a node-less entry */
    if (task->wq.node.node_p && (!last_timer || (task->wq.node.bit < last_timer->node.bit)))

        last_timer = &task->wq;
    return;

}

由此可以看出，ebtree函数库在使用上，只负责ebnode的组织，并不负责对ebnode的分配和释放，否则wq和rq就是两个ebnode类型的指针，而不是两个ebnode。这样做是有道理的，利用以上ebnode的特点，一个task可以先后多次挂载到相应的以ebtree为具体设施的队列中，减少了内存分配和释放次数。

# 3. stream interface

stream interface结构体的主要成员如下：

1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
17

18
19

20
21 struct stream_interface {

    unsigned int state;       /* SI_ST* */
    unsigned int prev_state;  /* SI_ST*, copy of previous state */

    void *owner;              /* generally a (struct task*) */
    int fd;                   /* file descriptor for a stream driver when known */

    unsigned int flags;
    unsigned int exp;         /* wake up time for connect, queue, turn-around, ... */

    void (*update)(struct stream_interface *);     /* I/O update function */
    void (*shutr)(struct stream_interface *);      /* shutr function */

    void (*shutw)(struct stream_interface *);      /* shutw function */
    void (*chk_rcv)(struct stream_interface *);    /* chk_rcv function */

    void (*chk_snd)(struct stream_interface *);    /* chk_snd function */
    int (*connect)(struct stream_interface *, struct proxy *, struct server *,

       struct sockaddr *, struct sockaddr *);      /* connect function if any */
    void (*iohandler)(struct stream_interface *);  /* internal I/O handler when embedded */

    struct buffer *ib, *ob;   /* input and output buffers */
    unsigned int err_type;    /* first error detected, one of SI_ET_* */

    void *err_loc;            /* commonly the server, NULL when SI_ET_NONE */
    void *private;            /* may be used by any function above */

    unsigned int st0, st1;    /* may be used by any function above */
};

大部分成员可以从注释中了解其用途，值得注意的是几个函数指针。大致上，它们可以指向两组函数：默认一组是以stream_sock_开头，用于在socket与缓冲区之间传输数据，转发处理业务流；另一组是以stream_int_开头，负责拦截、统计和响应客户端的监控请求，因为HAProxy的业务请求与监控请求是发送到同一个端口，通过URI来区分的，而监控请求显然是不能转发到后端服务器的，所以必须在获得各计数器数据后返回客户端。

以下简述一下stream_sock_开头的那一组函数的作用。这一组函数在上文的event_accept函数中初始化。
update指向的函数（stream_sock_data_finish）用于更新stream interface的fd的读写状态和相关标志位。
shutr和shutw指向的函数（stream_sock_shutr和stream_sock_shutw）分别用于关闭stream interface上的读和写。
chk_rcv指向的函数（stream_sock_chk_rcv）由消费者调用，用于通知生产者：缓冲区可能有空间了。
chk_snd指向的函数（stream_sock_chk_snd）由生产者调用，用于通知消费者：缓冲区可能有数据了。
当si是客户端stream interface时，connect为空，因为客户端连接显然已经建立。
当si是服务端stream interface时，connect指向HAProxy与服务端建立连接的那个函数（tcpv4_connect_server），而它会在backend.c的connect_server执行时被调用。
iohandler总是为空。

stream_int_开头的那一组函数在stream_interface.c的stream_int_register_handler函数中初始化，而该函数只有在监控分支才会执行。最明显区别是这一组会设置iohandler，而且调用时会重新唤醒所属的task，调用后马上返回，不再向下处理，如下所示：

1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
17

18
19 struct task *process_session(struct task *t)

{
    ...

    /* Call the second stream interface's I/O handler if it's embedded.
     * Note that this one may wake the task up again.

     */
    if (s->req->cons->iohandler) {

        s->req->cons->iohandler(s->req->cons);
        if (task_in_rq(t)) {

            /* If we woke up, we don't want to requeue the
             * task to the wait queue, but rather requeue

             * it into the runqueue ASAP.
             */

            t->expire = TICK_ETERNITY;
            return t;

        }
    }

    ...
}

# 4. buffer

完整的buffer结构体如下：

1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
17

18
19

20
21

22
23 struct buffer {

    unsigned int flags;             /* BF_* */
    int rex;                        /* expiration date for a read, in ticks */

    int wex;                        /* expiration date for a write or connect, in ticks */
    int rto;                        /* read timeout, in ticks */

    int wto;                        /* write timeout, in ticks */
    int cto;                        /* connect timeout, in ticks */

    unsigned int l;                 /* data length */
    char *r, *w, *lr;               /* read ptr, write ptr, last read */

    unsigned int size;              /* buffer size in bytes */
    unsigned int send_max;          /* number of bytes the sender can consume om this buffer, <= l */

    unsigned int to_forward;        /* number of bytes to forward after send_max without a wake-up */
    unsigned int analysers;         /* bit field indicating what to do on the buffer */

    int analyse_exp;                /* expiration date for current analysers (if set) */
    void (*hijacker)(struct session *, struct buffer *); /* alternative content producer */

    unsigned char xfer_large;       /* number of consecutive large xfers */
    unsigned char xfer_small;       /* number of consecutive small xfers */

    unsigned long long total;       /* total data read */
    struct stream_interface *prod;  /* producer attached to this buffer */

    struct stream_interface *cons;  /* consumer attached to this buffer */
    struct pipe *pipe;              /* non-NULL only when data present */

    char data[0];                   /* <size> bytes */
};

可以看到，HAProxy的buffer结构体也由许多成员组成，常规buffer该有的一个都不少，例如size、data、l、r、w、lr等等，除此之外，还有具有HAProxy特有的生产者、消费者stream interface指针（prod和cons），还有众多表示读写过期时间、超时时间的成员，还有指示具体请求响应解析过程的标志位（analysers），还有回调函数hijacker（不过看起来没有使用），还有表示该buffer最多能向其消费者发送多少字节的send_max，还有在HTTP chunked格式转发时才有意义的to_forward，还有连续满负荷传输数据和连续低负荷传输数据的计数器（xfer_large和xfer_small），以触发判断是否使用splice系统调用。面对把这么多功能揽于一身的buffer，看你晕不晕！

下面只挑一些具有通用借鉴意义的成员提及一下。

最后的那个成员data[0]是一个“零长度数组”，不懂的google一下就知道了，很简单，不多说。

# 5. pipe

buffer的倒数第二个成员pipe指针，指向的是这么一个链表结构：

1

2
3

4
5

6 struct pipe {

    int data;   /* number of bytes present in the pipe  */
    int prod;   /* FD the producer must write to ; -1 if none */

    int cons;   /* FD the consumer must read from ; -1 if none */
    struct pipe *next;

};

因为splice系统调用要求输入和输出至少必须有一个描述符是管道符，所以，HAProxy准备了一对管道符（prod和cons）。当使用splice读数据时，HAProxy从源socket描述符（对于请求，就是连接客户端与HAProxy的socket描述符，对于响应，则是连接服务端与HAProxy的socket 描述符；下面的目的socket描述符刚好相反）splice到管道符prod；当使用splice写数据时，HAProxy从管道符cons splice到目的socket描述符。详见stream_socket.c的stream_sock_splice_in函数和stream_sock_write_loop函数：

1

2
3

4
5

6
7

8
9

10
11

12
13

14
15 static int stream_sock_splice_in(struct buffer *b, struct stream_interface *si)

{
    ...

    ret = splice(fd, NULL, b->pipe->prod, NULL, max,
            SPLICE_F_MOVE|SPLICE_F_NONBLOCK);

    ...
}

 
static int stream_sock_write_loop(struct stream_interface *si, struct buffer *b)

{
    ...

    ret = splice(b->pipe->cons, NULL, si->fd, NULL, b->pipe->data,
            SPLICE_F_MOVE|SPLICE_F_NONBLOCK);

    ...
}

这样一来，每个buffer就要创建一对管道符，从上文和上面的分析可知，每个session就有两个buffer，而每个session的生命周期是从客户端连接建立到客户端连接释放（这里之前没有提到，以后可能会讲一下），所以，如果并发量很大，这是一个巨大的消耗。为此，HAProxy用管道池来解决这一问题，就像内存池的使用那样，需要有一个next指针把pipe结构体串起来。具体流程是，读取数据之前，调用get_pipe函数获取一对管道符，全部数据写完之后（由data指示数据是否写完），调用put_pipe函数进行释放，这样管道只会在异步等待写的时候被占用，大大减少使用量，从而减少系统开销。详见上面两个函数的代码。

# 6. 小结

五个我认为比较重要的结构体就介绍到这里。当然，HAProxy还有许多结构体，例如proxy、server、listener等等，不过，这些结构体，要么比较容易看懂，要么网上已经有比较齐全的资料，要么可以陆续在后面的文章中单独说明。而session、task、stream interface、buffer和pipe这五个结构体，连同第一篇介绍的ebtree，向我们展现了HAProxy作为一个高性能代理服务器的底层数据组织和一些重要的处理细节。
### 您可能感兴趣的文章

* [HAProxy的event_accept函数源码分析](http://tech.uc.cn/?p=1523)
* [HAProxy的独门武器：ebtree](http://tech.uc.cn/?p=1031)
* [Nginx使用ETag功能的分析](http://tech.uc.cn/?p=965)

来源： <[http://tech.uc.cn/?p=1738](http://tech.uc.cn/?p=1738)>
 
