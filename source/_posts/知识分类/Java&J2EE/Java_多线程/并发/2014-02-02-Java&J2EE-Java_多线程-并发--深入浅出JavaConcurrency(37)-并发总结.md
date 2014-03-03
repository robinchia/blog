---
layout: post
title: "深入浅出 Java Concurrency (37)- 并发总结"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_多线程
 - 并发
--- 

# 深入浅出 Java Concurrency (37): 并发总结

[深入浅出 Java Concurrency (37): 并发总结 part 1 死锁与活跃度](http://www.blogjava.net/xylz/archive/2011/12/29/365149.html)

# 死锁与活跃度

前面谈了很多并发的特性和工具，但是大部分都是和锁有关的。我们使用锁来保证线程安全，但是这也会引起一些问题。

* 锁顺序死锁(lock-ordering deadlock)：多个线程试图通过不同的顺序获得多个相同的资源，则发生的循环锁依赖现象。
* 动态的锁顺序死锁（Dynamic Lock Order Deadlocks）：多个线程通过传递不同的锁造成的锁顺序死锁问题。
* 资源死锁（Resource Deadlocks）：线程间相互等待对方持有的锁，并且谁都不会释放自己持有的锁发生的死锁。也就是说当现场持有和等待的目标成为资源，就有可能发生此死锁。这和锁顺序死锁不一样的地方是，竞争的资源之间并没有严格先后顺序，仅仅是相互依赖而已。

## 锁顺序死锁

最经典的锁顺序死锁就是LeftRightDeadLock.
![]()

public class LeftRightDeadLock {
    final Object left = new Object();
    final Object right = new Object();
    public void doLeftRight() {
        synchronized (left) {
            synchronized (right) {
                execute1();
            }
        }
    }
    public void doRightLeft() {
        synchronized (right) {
            synchronized (left) {
                execute2();
            }
        }
    }
    private void execute2() {
    }
    private void execute1() {
    }
}

这个例子很简单，当两个线程分别获取到left和right锁时，互相等待对方释放其对应的锁，很显然双方都陷入了绝境。

## 动态的锁顺序死锁

与锁顺序死锁不同的是动态的锁顺序死锁只是将静态的锁变成了动态锁。 一个比较生动的例子是这样的。

public void transferMoney(Account fromAccount,//
        Account toAccount,//
        int amount
        ) {
    synchronized (fromAccount) {
        synchronized (toAccount) {
            fromAccount.decr(amount);
            toAccount.add(amount);
        }
    }
}
当我们银行转账的时候，我们期望锁住双方的账户，这样保证是原子操作。 看起来很合理，可是如果双方同时在进行转账操作，那么就有可能发生死锁的可能性。

 

很显然，动态的锁顺序死锁的解决方案应该看起来和锁顺序死锁解决方案差不多。 但是一个比较特殊的解决方式是纠正这种顺序。 例如可以调整成这样：
Object lock = new Object();
public void transferMoney(Account fromAccount,//
        Account toAccount,//
        int amount
        ) {
    int order = fromAccount.name().compareTo(toAccount.name());
    Object lockFirst = order>0?toAccount:fromAccount;
    Object lockSecond = order>0?fromAccount:toAccount;
    if(order==0){
        synchronized(lock){
            synchronized(lockFirst){
                synchronized(lockSecond){
                    //do work
                }
            }
        }
    }else{
        synchronized(lockFirst){
            synchronized(lockSecond){
                //do work
            }
        }
    }
}

 

这个挺有意思的。比较两个账户的顺序，保证此两个账户之间的传递顺序总是按照某一种锁的顺序进行的， 即使多个线程同时发生，也会遵循一次操作完释放完锁才进行下一次操作的顺序，从而可以避免死锁的发生。

## 资源死锁

资源死锁比较容易理解，就是需要的资源远远大于已有的资源，这样就有可能线程间的资源竞争从而发生死锁。 一个简单的场景是，应用同时从两个连接池中获取资源，两个线程都在等待对方释放连接池的资源以便能够同时获取 到所需要的资源，从而发生死锁。

资源死锁除了这种资源之间的直接依赖死锁外，还有一种叫线程饥饿死锁（thread-starvation deadlock）。 严格意义上讲，这种死锁更像是活跃度问题。例如提交到线程池中的任务由于总是不能够抢到线程从而一直不被执行， 造成任务的“假死”状况。

除了上述几种问题外，还有协作对象间的死锁以及开发调用的问题。这个描述起来会比较困难，也不容易看出死锁来。

# 避免和解决死锁

通常发生死锁后程序难以自恢复。但也不是不能避免的。 有一些技巧和原则是可以降低死锁可能性的。

最简单的原则是尽可能的减少锁的范围。锁的范围越小，那么竞争的可能性也越小。 尽快释放锁也有助于避开锁顺序。如果一个线程每次最多只能够获取一个锁，那么就不会产生锁顺序死锁。尽管应用中比较困难，但是减少锁的边界有助于分析程序的设计和简化流程。 减少锁之间的依赖以及遵守获取锁的顺序是避免锁顺序死锁的有效途径。

另外尽可能的使用定时的锁有助于程序从死锁中自恢复。 例如对于上述顺序锁死锁中，使用定时锁很容易解决此问题。

public void doLeftRight() throws Exception {
    boolean over = false;
    while (!over) {
        if (left.tryLock(1, TimeUnit.SECONDS)) {
            try {
                if (right.tryLock(1, TimeUnit.SECONDS)) {
                    try {
                        execute1();
                    } finally {
                        right.unlock();
                        over = true;
                    }
                }
            } finally {
                left.unlock();
            }
        }
    }
}
public void doRightLeft() throws Exception {
    boolean over = false;
    while (!over) {
        if (right.tryLock(1, TimeUnit.SECONDS)) {
            try {
                if (left.tryLock(1, TimeUnit.SECONDS)) {
                    try {
                        execute2();
                    } finally {
                        left.unlock();
                        over = true;
                    }
                }
            } finally {
                right.unlock();
            }
        }
    }
}
看起来代码会比较复杂，但是这是避免死锁的有效方式。

 

# 活跃度

对于多线程来说，死锁是非常严重的系统问题，必须修正。除了死锁，遇到很多的就是活跃度问题了。 活跃度问题主要包括：饥饿，丢失信号，和活锁等。

## 饥饿

饥饿是指线程需要访问的资源被永久拒绝，以至于不能在继续进行。 比如说：某个权重比较低的线程可能一直不能够抢到CPU周期，从而一直不能够被执行。

也有一些场景是比较容易理解的。对于一个固定大小的连接池中，如果连接一直被用完，那么过多的任务可能由于一直无法抢占到连接从而不能够被执行。这也是饥饿的一种表现。

对于饥饿而言，就需要平衡资源的竞争，例如线程的优先级，任务的权重，执行的周期等等。总之，当空闲的资源较多的情况下，发生饥饿的可能性就越小。

## 弱响应性

弱响应是指，线程最终能够得到有效的执行，只是等待的响应时间较长。 最常见的莫过于GUI的“假死”了。很多时候GUI的响应只是为了等待后台数据的处理，如果线程协调不好，很有可能就会发生“失去响应”的现象。

另外，和饥饿很类似的情况。如果一个线程长时间独占一个锁，那么其它需要此锁的线程很有可能就会被迫等待。

## 活锁

活锁（Livelock）是指线程虽然没有被阻塞，但是由于某种条件不满足，一直尝试重试，却终是失败。

考虑一个场景，我们从队列中拿出一个任务来执行，如果任务执行失败，那么将任务重新加入队列，继续执行。假如任务总是执行失败，或者某种依赖的条件总是不满足，那么线程一直在繁忙却没有任何结果。

错误的循环引用和判断也有可能导致活锁。当某些条件总是不能满足的时候，可能陷入死循环的境地。

线程间的协同也有可能导致活锁。例如如果两个线程发生了某些条件的碰撞后重新执行，那么如果再次尝试后依然发生了碰撞，长此下去就有可能发生活锁。

解决活锁的一种方案是对重试机制引入一些随机性。例如如果检测到冲突，那么就暂停随机的一定时间进行重试。这回大大减少碰撞的可能性。

另外为了避免可能的死锁，适当加入一定的重试次数也是有效的解决办法。尽管这在业务上会引起一些复杂的逻辑处理。
来源： <[http://www.blogjava.net/xylz/archive/2011/12/29/365149.html](http://www.blogjava.net/xylz/archive/2011/12/29/365149.html)> 

# 常见的并发场景

## 线程池

并发最常见用于线程池，显然使用线程池可以有效的提高吞吐量。

最常见、比较复杂一个场景是Web容器的线程池。Web容器使用线程池同步或者异步处理HTTP请求，同时这也可以有效的复用HTTP连接，降低资源申请的开销。通常我们认为HTTP请求时非常昂贵的，并且也是比较耗费资源和性能的，所以线程池在这里就扮演了非常重要的角色。

在[线程池](http://www.blogjava.net/xylz/archive/2010/12/19/341098.html)的章节中非常详细的讨论了线程池的原理和使用，同时也提到了，线程池的配置和参数对性能的影响是巨大的。不尽如此，受限于资源（机器的性能、网络的带宽等等）、依赖的服务，客户端的响应速度等，线程池的威力也不会一直增长。达到了线程池的瓶颈后，性能和吞吐量都会大幅度降低。

一直增加机器的性能或者增大线程的个数，并不一定能有效的提高吞吐量。高并发的情况下，机器的负载会大幅提升，这时候机器的稳定性、服务的可靠性都会下降。

尽管如此，线程池依然是提高吞吐量的一个有效措施，配合合适的参数能够有效的充分利用资源，提高资源的利用率。

## 任务队列

除了线程池是比较发杂的并发场景外，[任务队列](http://www.blogjava.net/xylz/archive/2010/07/21/326723.html)也是一个不错的并发工具。JDK内部有大量的队列（Queue),这些工具不仅能够方便使用，提高生产力，也能够进行组合适应于不同的场景。即使线程池内部，也是用了任务队列来处理任务的积压，平衡资源的消耗。

安全的任务队列能够有效的平衡机器的复杂，抵消由于峰值和波动带来的不稳定，有效提高服务的可靠性。同时任务队列的处理也有助于统计和分析服务的状况。

任务队列也可以在多个线程之间传递数据，有助于并行处理任务。例如经典的“生产者-消费者”模型就可以有效的提高多个线程的并行处理能力。在IO延时比较大的服务中尤其有效。 我最喜欢的一个案例是导数据是，一个线程负责往固定大小的任务队列中压入大量的数据，队列满了以后就暂停，另外几个线程负责从任务队列中获取数据并消费。这将串行的“生产-消费”，变成了并行的“生产-消费”。实践证明极大的节省任务处理时间。

## 异步处理

线程池也是异步处理的一种表现形式，除此之外，使用异步处理的目的也是为了提高服务的处理速度。 例如AOP的一个例子就是使用切面来记录日志，如果说我们要远程收集日志，显然不希望由于收集日志而影响服务本身。这时候就将日志收集的过程进行异步处理。

如今大量的开源组件都喜欢使用异步处理来提高IO的效率，某些不需要同步返回的操作使用异步处理后能够有效的提高吞吐量。

当然，异步也不总是令人满意的，也会有相应的问题。例如引入异步设计后的复杂性，线程中断后的处理机制，失败后的处理策略，产生的消息比消费的还快时怎么办，关闭程序时如何关闭异步处理逻辑等等。这都会增加系统的复杂性。

尽管大量的服务、业务使用异步来处理，但是很显然需要有保障机制能够保证异步处理的逻辑正确性。如果认为异步处理的任务不是特别重要，或者说主业务不能因为附属业务的逻辑出错而崩溃，那么使用异步处理是正确的选择。

## 同步操作

并发操作的同时还需要维护数据的一致性，或多或少的会涉及到同步操作。正确的使用原子操作，合理的使用独占锁和读写锁也是一个很大的挑战。

线程间的协调与通信，尤其是状态的同步都是比较困难的。我们看到线程池[ThreadPoolExecutor](http://www.blogjava.net/xylz/archive/2011/01/18/343183.html)的实现为了解决各个线程的执行状态，引入的很多的同步操作。线程越来越多的情况下，同步的成本会越来越高，同时也有可能引入死锁的情况。

尽管如此，单个JVM内部的多线程同步还是比较容易控制的。JDK内部也提供了大量的工具来方便完成数据的同步。例如[Lock](http://www.blogjava.net/xylz/archive/2010/07/05/325274.html)/[Condition](http://www.blogjava.net/xylz/archive/2010/07/08/325540.html)/[CountDownLatch](http://www.blogjava.net/xylz/archive/2010/07/09/325612.html)/[CyclicBarrier](http://www.blogjava.net/xylz/archive/2010/07/12/325913.html)/[Semaphore](http://www.blogjava.net/xylz/archive/2010/07/13/326021.html)/[Exchanger](http://www.blogjava.net/xylz/archive/2010/11/22/338733.html)等等。

## 分布式锁

分布式的并发问题更难以处理，根据[CAP](http://en.wikipedia.org/wiki/CAP_theorem)的原理，基本上没有一个至善至美的方案。 分布式资源协调使用分布式锁是一个不错的选择。[Google的分布式锁](http://blog.nosqlfan.com/html/1038.html)（建立在BigTable之上），[Zookeeper的分布式锁](http://zookeeper.apache.org/doc/r3.3.2/zookeeperOver.html)，甚至简单的利用[memcache](http://memcached.org/)的add操作或者[redis](http://redis.io/)的setnx操作建立伪分布式锁也可以解决类似的问题。
来源： <[http://www.blogjava.net/xylz/archive/2011/12/29/367480.html](http://www.blogjava.net/xylz/archive/2011/12/29/367480.html)> 

# 常见的并发陷阱

## volatile

volatile只能强调数据的可见性，并不能保证原子操作和线程安全，因此volatile不是万能的。参考[指令重排序](http://www.blogjava.net/xylz/archive/2010/07/03/325168.html)

volatile最常见于下面两种场景。

a. 循环检测机制
volatile boolean done = false;
![]()
    while( ! done ){
        dosomething();
    }

b. 单例模型 （[http://www.blogjava.net/xylz/archive/2009/12/18/306622.html）
](http://www.blogjava.net/xylz/archive/2009/12/18/306622.html%ef%bc%89)

[public class DoubleLockSingleton {
    private static volatile DoubleLockSingleton instance = null;
    private DoubleLockSingleton() {
    }
    public static DoubleLockSingleton getInstance() {
        if (instance == null) {
            synchronized (DoubleLockSingleton.class) {
                if (instance == null) {
                    instance = new DoubleLockSingleton();
                }
            }
        }
        return instance;
    }
}](http://www.blogjava.net/xylz/archive/2009/12/18/306622.html%ef%bc%89)

 

## synchronized/Lock

看起来Lock有更好的性能以及更灵活的控制，是否完全可以替换synchronized？

在[锁的一些其它问题](http://www.blogjava.net/xylz/archive/2010/07/16/326246.html)中说过，synchronized的性能随着JDK版本的升级会越来越高，而Lock优化的空间受限于CPU的性能，很有限。另外JDK内部的工具（线程转储）对synchronized是有一些支持的（方便发现死锁等），而对Lock是没有任何支持的。

也就说简单的逻辑使用synchronized完全没有问题，随着机器的性能的提高，这点开销是可以忽略的。而且从代码结构上讲是更简单的。简单就是美。

对于复杂的逻辑，如果涉及到读写锁、条件变量、更高的吞吐量以及更灵活、动态的用法，那么就可以考虑使用Lock。当然这里尤其需要注意Lock的正确用法。
Lock lock = ![]()
lock.lock();
try{
    //do something
}finally{
    lock.unlock();
}

一定要将Lock的释放放入finally块中，否则一旦发生异常或者逻辑跳转，很有可能会导致锁没有释放，从而发生死锁。而且这种死锁是难以排查的。

如果需要synchronized无法做到的尝试锁机制，或者说担心发生死锁无法自恢复，那么使用tryLock()是一个比较明智的选择的。
Lock lock = ![]()
if(lock.tryLock()){
    try{
        //do something
    }finally{
        lock.unlock();
    }
}

 

甚至可以使用获取锁一段时间内超时的机制Lock.tryLock(long,TimeUnit)。 锁的使用可以参考前面文章的描述和建议。

## 锁的边界

一个流行的错误是这样的。
ConcurrentMap<String,String> map = new ConcurrentHashMap<String,String>();
if(!map.containsKey(key)){
    map.put(key,value);
}

看起来很合理的，对于一个线程安全的Map实现，要存取一个不重复的结果，先检测是否存在然后加入。 其实我们知道两个原子操作和在一起的指令序列不代表就是线程安全的。 割裂的多个原子操作放在一起在多线程的情况下就有可能发生错误。

实际上ConcurrentMap提供了putIfAbsent(K, V)的“原子操作”机制，这等价于下面的逻辑：
if(map.containsKey(key)){
    return map.get(key);
}else{
    return map.put(k,v);
}

除了putIfAbsent还有replace(K, V)以及replace(K, V, V)两种机制来完成组合的操作。

提到Map，这里有一篇谈[HashMap读写并发](http://www.blogjava.net/xylz/archive/2009/12/18/306602.html)的问题。

## 构造函数启动线程

下面的实例是在构造函数中启动一个线程。
public class Runner{
   int x,y;
   Thread thread;
   public Runner(){
      this.x=1;
      this.y=2;
      this.thread=new MyThread();
      this.thread.start();
   }
}

这里可能存在的陷阱是如果此类被继承，那么启动的线程可能无法正确读取子类的初始化操作。

因此一个简单的原则是，禁止在构造函数中启动线程，可以考虑但是提供一个方法来启动线程。如果非要这么做，最好将类设置为final，禁止继承。

## 丢失通知的问题

[这篇文章](http://www.blogjava.net/xylz/archive/2011/09/05/326988.html)里面提到过notify丢失通知的问题。

对于wait/notify/notifyAll以及await/singal/singalAll，如果不确定到底是否能够正确的收到消息，担心丢失通知，简单一点就是总是通知所有。

如果担心只收到一次消息，使用循环一直监听是不错的选择。

非常主用性能的系统，可能就需要区分到底是通知单个还是通知所有的挂起者。

## 线程数

并不是线程数越多越好，在下一篇文章里面会具体了解下性能和可伸缩性。 简单的说，线程数多少没有一个固定的结论，受限于CPU的内核数，IO的性能以及依赖的服务等等。因此选择一个合适的线程数有助于提高吞吐量。

对于CPU密集型应用，线程数和CPU的内核数一致有助于提高吞吐量，所有CPU都很繁忙，效率就很高。 对于IO密集型应用，线程数受限于IO的性能，某些时候单线程可能比多线程效率更高。但通常情况下适当提高线程数，有利于提高网络IO的效率，因为我们总是认为网络IO的效率比较低。

对于线程池而言，选择合适的线程数以及任务队列是提高线程池效率的手段。
public ThreadPoolExecutor(
    int corePoolSize,
    int maximumPoolSize,
    long keepAliveTime,
    TimeUnit unit,
    BlockingQueue<Runnable> workQueue,
    ThreadFactory threadFactory,
    RejectedExecutionHandler handler)

 

对于线程池来说，如果任务总是有积压，那么可以适当提高corePoolSize大小；如果机器负载较低，那么可以适当提高maximumPoolSize的大小；任务队列不长的情况下减小keepAliveTime的时间有助于降低负载；另外任务队列的长度以及任务队列的[拒绝策略](http://www.blogjava.net/xylz/archive/2011/01/18/343183.html)也会对任务的处理有一些影响。
来源： <[http://www.blogjava.net/xylz/archive/2011/12/30/367592.html](http://www.blogjava.net/xylz/archive/2011/12/30/367592.html)> 

# 性能与伸缩性

使用线程的一种说法是为了提高性能。多线程可以使程序充分利用闲置的资源，提高资源的利用率，同时能够并行处理任务，提高系统的响应性。 但是很显然，引入线程的同时也引入了系统的复杂性。另外系统的性能并不是总是随着线程数的增加而总是提高。

## 性能与伸缩性

性能的提升通常意味着可以用更少的资源做更多的事情。这里资源是包括我们常说的CPU周期、内存、网络带宽、磁盘IO、数据库、WEB服务等等。 引入多线程可以充分利用多核的优势，充分利用IO阻塞带来的延迟，也可以降低网络开销带来的影响，从而提高单位时间内的响应效率。

为了提高性能，需要有效的利用我们现有的处理资源，同时也要开拓新的可用资源。例如，对于CPU而言，理想状况下希望CPU能够满负荷工作。当然这里满负荷工作是指做有用的事情，而不是无谓的死循环或者等待。受限于CPU的计算能力，如果CPU达到了极限，那么很显然我们充分利用了计算能力。对于IO而言（内存、磁盘、网络等），如果达到了其对于的带宽，这些资源的利用率也就上去了。理想状况下所有资源的能力都被用完了，那么这个系统的性能达到了最大值。

为了衡量系统的性能，有一些指标用于定性、定量的分析。例如服务时间、等待时间、吞吐量、效率、可伸缩性、生成量等等。服务时间、等待时间等用于衡量系统的效率，即到底有多快。吞吐量、生成量等用于衡量系统的容量，即能够处理多少数据。除此之外，有效服务时间、中断时间等用于能力系统的可靠性和稳定性等。

可伸缩性的意思是指增加计算资源，吞吐量和生产量相应得到的改进。 从算法的角度讲，通常用复杂度来衡量其对应的性能。例如时间复杂度、空间复杂度等。

## Amdahl定律

并行的任务增加资源显然能够提高性能，但是如果是串行的任务，增加资源并不一定能够得到合理的性能提升。 [Amdahl定律](http://en.wikipedia.org/wiki/Amdahl%27s_law)描述的在一个系统中，增加处理器资源对系统行的提升比率。 假定在一个系统中，F是必须串行化执行的比重，N是处理器资源，那么随着N的增加最多增加的加速比：
![]()

理论上，当N趋近于无穷大时，加速比最大值无限趋近于1/F。 这意味着如果一个程序的串行化比重为50%，那么并行化后最大加速比为2倍。

加速比除了可以用于加速的比率外，也可以用于衡量CPU资源的利用率。如果每一个CPU的资源利用率为100%，那么CPU的资源每次翻倍时，加速比也应该翻倍。 事实上，在拥有10个处理器的系统中，程序如果有10%是串行化的，那么最多可以加速1/(0.1+(1-0.1)/10)=5.3倍，换句话说CPU的利用率只用5.3/10=53%。而如果处理器增加到100倍，那么加速比为9.2倍，也就是说CPU的利用率只有个9.3%。

显然增加CPU的数量并不能提高CPU的利用率。下图描述的是随着CPU的数量增加，不同串行化比重的系统的加速比。
![]()

很显然，串行比重越大，增加CPU资源的效果越不明显。

## 性能提升

性能的提升可以从以下几个方面入手。

### 系统平台的资源利用率

一个程序对系统平台的资源利用率是指某一个设备繁忙且服务于此程序的时间占所有时间的比率。从物理学的角度讲类似于有用功的比率。简单的说就是：资源利用率=有效繁忙时间/总耗费时间。

也就说尽可能的让设备做有用的功，同时榨取其最大值。无用的循环可能会导致CPU 100%的使用率，但不一定是有效的工作。有效性通常难以衡量，通常只能以主观来评估，或者通过被优化的程序的行为来判断是否提高了有效性。

### 延迟

延迟描述的是完成任务所耗费的时间。延迟有时候也成为响应时间。如果有多个并行的操作，那么延迟取决于耗费时间最大的任务。

### 多处理

多处理是指在单一系统上同时执行多个进程或者多个程序的能力。多处理能力的好处是可以提高吞吐量。多处理可以有效利用多核CPU的资源。

### 多线程

多线程描述的是同一个地址空间内同时执行多个线程的过程。这些线程都有不同的执行路径和不同的栈结构。我们说的并发性更多的是指针对线程。

### 并发性

同时执行多个程序或者任务称之为并发。单程序内的多任务处理或者多程序间的多任务处理都认为是并发。

### 吞吐量

吞吐量衡量系统在单位之间内可以完成的工作总量。对于硬件系统而言，吞吐量是物理介质的上限。在没有达到物理介质之前，提高系统的吞吐量也可以大幅度改进性能。同时吞吐量也是衡量性能的一个指标。

### 瓶颈

程序运行过程中性能最差的地方。通常而言，串行的IO、磁盘IO、内存单元分配、网络IO等都可能造成瓶颈。某些使用太频繁的算法也有可能成为瓶颈。

### 可扩展性

这里的可扩展性主要是指程序或系统通过增加可使用的资源而增加性能的能力。

## 线程开销

假设引入的多线程都用于计算，那么性能一定会有很大的提升么？ 其实引入多线程以后也会引入更多的开销。

### 切换上下文

如果可运行的线程数大于CPU的内核数，那么OS会根据一定的调度算法，强行切换正在运行的线程，从而使其它线程能够使用CPU周期。

切换线程会导致上下文切换。线程的调度会导致CPU需要在操作系统和进程间花费更多的时间片段，这样真正执行应用程序的时间就减少了。另外上下文切换也会导致缓存的频繁进出，对于一个刚被切换的线程来说，可能由于高速缓冲中没有数据而变得更慢，从而导致更多的IO开销。

### 内存同步

不同线程间要进行数据同步，synchronized以及volatile提供的可见性都会导致缓存失效。线程栈之间的数据要和主存进行同步，这些同步有一些小小的开销。如果线程间同时要进行数据同步，那么这些同步的线程可能都会受阻。

### 阻塞

当发生锁竞争时，失败的线程会导致阻塞。通常阻塞的线程可能在JVM内部进行自旋等待，或者被操作系统挂起。自旋等待可能会导致更多的CPU切片浪费，而操作系统挂起则会导致更多的上下文切换。

了解了性能的提升的几个方面，也了解性能的开销后，应用程序就要根据实际的场景进行取舍和评估。没有一劳永逸的优化方案，不断的进行小范围改进和调整是提高性能的有效手段。当前一些大的架构调整也会导致较大的性能的提升。

简单的原则是在保证逻辑正确的情况小，找到性能瓶颈，小步改进和优化。

## 参考资料

* Amdahl's law: [http://en.wikipedia.org/wiki/Amdahl%27s_law](http://en.wikipedia.org/wiki/Amdahl%27s_law)
* Gustafson's law: [http://en.wikipedia.org/wiki/Gustafson%27s_law](http://en.wikipedia.org/wiki/Gustafson%27s_law)
* Sun-Ni law: [http://en.wikipedia.org/wiki/Sun-Ni_law](http://en.wikipedia.org/wiki/Sun-Ni_law)
* 多核系统中三种典型锁竞争的加速比分析 [http://blog.csdn.net/drzhouweiming/article/details/1800319](http://blog.csdn.net/drzhouweiming/article/details/1800319)
* 阿姆达尔定律和Gustafson定律的等价性 [http://book.51cto.com/art/201004/197506.htm](http://book.51cto.com/art/201004/197506.htm)
来源： <[http://www.blogjava.net/xylz/archive/2011/12/31/367641.html](http://www.blogjava.net/xylz/archive/2011/12/31/367641.html)> 
