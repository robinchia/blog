---
layout: post
title: "Java aio(异步网络IO)初探"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - NIO
--- 

# Java aio(异步网络IO)初探

[<]() [>]()  猎头职位: [上海:  Junior Product Manager](http://www.iteye.com/jobs/2509)

相关文章: [ ]( "关闭")

* [JDK7 AIO 初体验](http://www.iteye.com/topic/1113611 "JDK7 AIO 初体验")
* [JavaSE7新特性 异步非阻塞I/O 网络通信 AIO](http://www.iteye.com/topic/446298 "JavaSE7新特性 异步非阻塞I/O 网络通信 AIO")
* [JAVA NIO 简介](http://www.iteye.com/topic/834447 "JAVA NIO 简介")
推荐群组: [D语言](http://dlang.group.iteye.com/)
[更多相关推荐](http://www.iteye.com/wiki/topic/472333)
[企业应用](http://www.iteye.com/forums/tag/%E4%BC%81%E4%B8%9A%E5%BA%94%E7%94%A8)

    按照《Unix网络编程》的划分，IO模型可以分为：阻塞IO、非阻塞IO、IO复用、信号驱动IO和异步IO，按照POSIX标准来划分只分为两类：同步IO和异步IO。如何区分呢？首先一个IO操作其实分成了两个步骤：发起IO请求和实际的IO操作，同步IO和异步IO的区别就在于第二个步骤是否阻塞，如果实际的IO读写阻塞请求进程，那么就是同步IO，因此阻塞IO、非阻塞IO、IO服用、信号驱动IO都是同步IO，如果不阻塞，而是操作系统帮你做完IO操作再将结果返回给你，那么就是异步IO。阻塞IO和非阻塞IO的区别在于第一步，发起IO请求是否会被阻塞，如果阻塞直到完成那么就是传统的阻塞IO，如果不阻塞，那么就是非阻塞IO。
   Java nio 2.0的主要改进就是引入了异步IO（包括文件和网络），这里主要介绍下异步网络IO API的使用以及框架的设计，以TCP服务端为例。首先看下为了支持AIO引入的新的类和接口：
 **java.nio.channels.AsynchronousChannel**
       标记一个channel支持异步IO操作。
** java.nio.channels.AsynchronousServerSocketChannel**
       ServerSocket的aio版本，创建TCP服务端，绑定地址，监听端口等。
** java.nio.channels.AsynchronousSocketChannel**
       面向流的异步socket channel，表示一个连接。
** java.nio.channels.AsynchronousChannelGroup**
       异步channel的分组管理，目的是为了资源共享。一个AsynchronousChannelGroup绑定一个线程池，这个线程池执行两个任务：处理IO事件和派发CompletionHandler。AsynchronousServerSocketChannel创建的时候可以传入一个 AsynchronousChannelGroup，那么通过AsynchronousServerSocketChannel创建的 AsynchronousSocketChannel将同属于一个组，共享资源。
** java.nio.channels.CompletionHandler**
       异步IO操作结果的回调接口，用于定义在IO操作完成后所作的回调工作。AIO的API允许两种方式来处理异步操作的结果：返回的Future模式或者注册CompletionHandler，我更推荐用CompletionHandler的方式，这些handler的调用是由 AsynchronousChannelGroup的线程池派发的。显然，线程池的大小是性能的关键因素。AsynchronousChannelGroup允许绑定不同的线程池，通过三个静态方法来创建：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public static AsynchronousChannelGroup withFixedThreadPool(int nThreads,  
1.                                                               ThreadFactory threadFactory)  
1.        throws IOException  
1.   
1. public static AsynchronousChannelGroup withCachedThreadPool(ExecutorService executor,  
1.                                                                int initialSize)  
1.   
1. public static AsynchronousChannelGroup withThreadPool(ExecutorService executor)  
1.        throws IOException  

public static AsynchronousChannelGroup withFixedThreadPool(int nThreads,

                                                               ThreadFactory threadFactory)
        throws IOException

 
public static AsynchronousChannelGroup withCachedThreadPool(ExecutorService executor,

                                                                int initialSize)
 

public static AsynchronousChannelGroup withThreadPool(ExecutorService executor)
        throws IOException
 

     需要根据具体应用相应调整，从框架角度出发，需要暴露这样的配置选项给用户。
     在介绍完了aio引入的TCP的主要接口和类之后，我们来设想下一个aio框架应该怎么设计。参考非阻塞nio框架的设计，一般都是采用**Reactor**模式，Reacot负责事件的注册、select、事件的派发；相应地，异步IO有个**Proactor**模式，Proactor负责 CompletionHandler的派发，查看一个典型的IO写操作的流程来看两者的区别：
     Reactor:  send(msg) -> 消息队列是否为空，如果为空  -> 向Reactor注册OP_WRITE，然后返回 -> Reactor select -> 触发Writable，通知用户线程去处理 ->先注销Writable(很多人遇到的cpu 100%的问题就在于没有注销）,处理Writeable，如果没有完全写入，继续注册OP_WRITE。注意到，写入的工作还是用户线程在处理。
     Proactor: send(msg) -> 消息队列是否为空，如果为空,发起read异步调用，并注册CompletionHandler，然后返回。 -> 操作系统负责将你的消息写入，并返回结果（写入的字节数）给Proactor -> Proactor派发CompletionHandler。可见，写入的工作是操作系统在处理，无需用户线程参与。事实上在aio的API 中,**AsynchronousChannelGroup就扮演了Proactor的角色**。
    CompletionHandler有三个方法，分别对应于处理成功、失败、被取消（通过返回的Future)情况下的回调处理：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public interface CompletionHandler<V,A> {  
1.   
1.      void completed(V result, A attachment);  
1.   
1.     void failed(Throwable exc, A attachment);  
1.   
1.      
1.     void cancelled(A attachment);  
1. }  

public interface CompletionHandler<V,A> {

 
     void completed(V result, A attachment);

 
    void failed(Throwable exc, A attachment);

 
 

    void cancelled(A attachment);
}
 

    其中的泛型参数V表示IO调用的结果，而A是发起调用时传入的attchment。
    在初步介绍完aio引入的类和接口后，我们看看一个典型的tcp服务端是怎么启动的，怎么接受连接并处理读和写，这里引用的代码都是yanf4j 的aio分支中的代码，可以从svn checkout，svn地址: [http://yanf4j.googlecode.com/svn/branches/yanf4j-aio](http://yanf4j.googlecode.com/svn/branches/yanf4j-aio)
    第一步，创建一个AsynchronousServerSocketChannel，创建之前先创建一个 AsynchronousChannelGroup，上文提到AsynchronousServerSocketChannel可以绑定一个 AsynchronousChannelGroup，那么通过这个AsynchronousServerSocketChannel建立的连接都将同属于一个AsynchronousChannelGroup并共享资源：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. this.asynchronousChannelGroup = AsynchronousChannelGroup  
1.                     .withCachedThreadPool(Executors.newCachedThreadPool(),  
1.                             this.threadPoolSize);  

this.asynchronousChannelGroup = AsynchronousChannelGroup

                    .withCachedThreadPool(Executors.newCachedThreadPool(),
                            this.threadPoolSize);

     然后初始化一个AsynchronousServerSocketChannel，通过open方法：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. this.serverSocketChannel = AsynchronousServerSocketChannel  
1.                 .open(this.asynchronousChannelGroup);  

this.serverSocketChannel = AsynchronousServerSocketChannel

                .open(this.asynchronousChannelGroup);
 

    通过nio 2.0引入的SocketOption类设置一些TCP选项：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. this.serverSocketChannel  
1.                     .setOption(  
1.                             StandardSocketOption.SO_REUSEADDR,true);  
1. this.serverSocketChannel  
1.                     .setOption(  
1.                             StandardSocketOption.SO_RCVBUF,16*1024);  

this.serverSocketChannel

                    .setOption(
                            StandardSocketOption.SO_REUSEADDR,true);

this.serverSocketChannel
                    .setOption(

                            StandardSocketOption.SO_RCVBUF,16*1024);
 

    绑定本地地址：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. this.serverSocketChannel  
1.                     .bind(new InetSocketAddress("localhost",8080), 100);  

this.serverSocketChannel

                    .bind(new InetSocketAddress("localhost",8080), 100);
 

  
    其中的100用于指定等待连接的队列大小(backlog)。完了吗？还没有，最重要的**监听**工作还没开始，监听端口是为了等待连接上来以便accept产生一个AsynchronousSocketChannel来表示一个新建立的连接，因此需要发起一个accept调用，调用是异步的，操作系统将在连接建立后，将最后的结果——**AsynchronousSocketChannel**返回给你：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public void pendingAccept() {  
1.         if (this.started && this.serverSocketChannel.isOpen()) {  
1.             this.acceptFuture = this.serverSocketChannel.accept(null,  
1.                     new AcceptCompletionHandler());  
1.   
1.         } else {  
1.             throw new IllegalStateException("Controller has been closed");  
1.         }  
1.     }  

public void pendingAccept() {

        if (this.started && this.serverSocketChannel.isOpen()) {
            this.acceptFuture = this.serverSocketChannel.accept(null,

                    new AcceptCompletionHandler());
 

        } else {
            throw new IllegalStateException("Controller has been closed");

        }
    }
 

   注意，重复的accept调用将会抛出PendingAcceptException，后文提到的read和write也是如此。accept方法的第一个参数是你想传给CompletionHandler的attchment，第二个参数就是注册的用于回调的CompletionHandler，最后返回结果Future<AsynchronousSocketChannel>。你可以对future做处理，这里采用更推荐的方式就是注册一个CompletionHandler。那么accept的CompletionHandler中做些什么工作呢？显然一个赤裸裸的 AsynchronousSocketChannel是不够的，我们需要将它封装成session，一个session表示一个连接（mina里就叫 IoSession了），里面带了一个缓冲的消息队列以及一些其他资源等。在连接建立后，除非你的服务器只准备接受一个连接，不然你需要在后面**继续调用pendingAccept来发起另一个accept请求**：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. private final class AcceptCompletionHandler implements  
1.             CompletionHandler<AsynchronousSocketChannel, Object> {  
1.   
1.         @Override  
1.         public void cancelled(Object attachment) {  
1.             logger.warn("Accept operation was canceled");  
1.         }  
1.   
1.         @Override  
1.         public void completed(AsynchronousSocketChannel socketChannel,  
1.                 Object attachment) {  
1.             try {  
1.                 logger.debug("Accept connection from "  
1.                         + socketChannel.getRemoteAddress());  
1.                 configureChannel(socketChannel);  
1.                 AioSessionConfig sessionConfig = buildSessionConfig(socketChannel);  
1.                 Session session = new AioTCPSession(sessionConfig,  
1.                         AioTCPController.this.configuration  
1.                                 .getSessionReadBufferSize(),  
1.                         AioTCPController.this.sessionTimeout);  
1.                 session.start();  
1.                 registerSession(session);  
1.             } catch (Exception e) {  
1.                 e.printStackTrace();  
1.                 logger.error("Accept error", e);  
1.                 notifyException(e);  
1.             } finally {  
1.                 <strong>pendingAccept</strong>();  
1.             }  
1.         }  
1.   
1.         @Override  
1.         public void failed(Throwable exc, Object attachment) {  
1.             logger.error("Accept error", exc);  
1.             try {  
1.                 notifyException(exc);  
1.             } finally {  
1.                 <strong>pendingAccept</strong>();  
1.             }  
1.         }  
1.     }  

private final class AcceptCompletionHandler implements

            CompletionHandler<AsynchronousSocketChannel, Object> {
 

        @Override
        public void cancelled(Object attachment) {

            logger.warn("Accept operation was canceled");
        }

 
        @Override

        public void completed(AsynchronousSocketChannel socketChannel,
                Object attachment) {

            try {
                logger.debug("Accept connection from "

                        + socketChannel.getRemoteAddress());
                configureChannel(socketChannel);

                AioSessionConfig sessionConfig = buildSessionConfig(socketChannel);
                Session session = new AioTCPSession(sessionConfig,

                        AioTCPController.this.configuration
                                .getSessionReadBufferSize(),

                        AioTCPController.this.sessionTimeout);
                session.start();

                registerSession(session);
            } catch (Exception e) {

                e.printStackTrace();
                logger.error("Accept error", e);

                notifyException(e);
            } finally {

 
**pendingAccept**

();
            }

        }
 

        @Override
        public void failed(Throwable exc, Object attachment) {

            logger.error("Accept error", exc);
            try {

                notifyException(exc);
            } finally {

 
**pendingAccept**

();
            }

        }
    }
 

 
    注意到了吧，我们在failed和completed方法中在最后都调用了pendingAccept来继续发起accept调用，等待新的连接上来。有的同学可能要说了，这样搞是不是递归调用，会不会堆栈溢出？实际上不会，因为发起accept调用的线程与CompletionHandler回调的线程并非同一个，不是一个上下文中，两者之间没有耦合关系。要注意到，CompletionHandler的回调共用的是 AsynchronousChannelGroup绑定的线程池，因此**千万别在CompletionHandler回调方法中调用阻塞或者长时间的操作**，例如sleep，回调方法最好能支持超时，防止线程池耗尽。
    连接建立后，怎么读和写呢？回忆下在nonblocking nio框架中，连接建立后的第一件事是干什么？注册OP_READ事件等待socket可读。异步IO也同样如此，连接建立后马上发起一个异步read调用，等待socket可读，这个是Session.start方法中所做的事情：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public class AioTCPSession {  
1.     protected void start0() {  
1.         pendingRead();  
1.     }  
1.   
1.     protected final void pendingRead() {  
1.         if (!isClosed() && this.asynchronousSocketChannel.isOpen()) {  
1.             if (!this.readBuffer.hasRemaining()) {  
1.                 this.readBuffer = ByteBufferUtils  
1.                         .increaseBufferCapatity(this.readBuffer);  
1.             }  
1.             this.readFuture = this.asynchronousSocketChannel.read(  
1.                     this.readBuffer, this, this.readCompletionHandler);  
1.         } else {  
1.             throw new IllegalStateException(  
1.                     "Session Or Channel has been closed");  
1.         }  
1.     }  
1.      
1. }  

public class AioTCPSession {

    protected void start0() {
        pendingRead();

    }
 

    protected final void pendingRead() {
        if (!isClosed() && this.asynchronousSocketChannel.isOpen()) {

            if (!this.readBuffer.hasRemaining()) {
                this.readBuffer = ByteBufferUtils

                        .increaseBufferCapatity(this.readBuffer);
            }

            this.readFuture = this.asynchronousSocketChannel.read(
                    this.readBuffer, this, this.readCompletionHandler);

        } else {
            throw new IllegalStateException(

                    "Session Or Channel has been closed");
        }

    }
 

}
 

     AsynchronousSocketChannel的read调用与AsynchronousServerSocketChannel的accept调用类似，同样是非阻塞的，返回结果也是一个Future，但是写的结果是整数，表示写入了多少字节，因此read调用返回的是 **Future<Integer>**，方法的第一个参数是读的缓冲区，操作系统将IO读到数据拷贝到这个缓冲区，第二个参数是传递给 CompletionHandler的attchment，第三个参数就是注册的用于回调的CompletionHandler。这里保存了read的结果Future，这是为了在关闭连接的时候能够主动取消调用，accept也是如此。现在可以看看read的CompletionHandler的实现：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public final class ReadCompletionHandler implements  
1.         CompletionHandler<Integer, AbstractAioSession> {  
1.   
1.     private static final Logger log = LoggerFactory  
1.             .getLogger(ReadCompletionHandler.class);  
1.     protected final AioTCPController controller;  
1.   
1.     public ReadCompletionHandler(AioTCPController controller) {  
1.         this.controller = controller;  
1.     }  
1.   
1.     @Override  
1.     public void cancelled(AbstractAioSession session) {  
1.         log.warn("Session(" + session.getRemoteSocketAddress()  
1.                 + ") read operation was canceled");  
1.     }  
1.   
1.     @Override  
1.     public void completed(Integer result, AbstractAioSession session) {  
1.         if (log.isDebugEnabled())  
1.             log.debug("Session(" + session.getRemoteSocketAddress()  
1.                     + ") read +" + result + " bytes");  
1.         if (result < 0) {  
1.             session.close();  
1.             return;  
1.         }  
1.         try {  
1.             if (result > 0) {  
1.                 session.updateTimeStamp();  
1.                 session.getReadBuffer().flip();  
1.                 session.decode();  
1.                 session.getReadBuffer().compact();  
1.             }  
1.         } finally {  
1.             try {  
1.                 session.pendingRead();  
1.             } catch (IOException e) {  
1.                 session.onException(e);  
1.                 session.close();  
1.             }  
1.         }  
1.         controller.checkSessionTimeout();  
1.     }  
1.   
1.     @Override  
1.     public void failed(Throwable exc, AbstractAioSession session) {  
1.         log.error("Session read error", exc);  
1.         session.onException(exc);  
1.         session.close();  
1.     }  
1.   
1. }  

public final class ReadCompletionHandler implements

        CompletionHandler<Integer, AbstractAioSession> {
 

    private static final Logger log = LoggerFactory
            .getLogger(ReadCompletionHandler.class);

    protected final AioTCPController controller;
 

    public ReadCompletionHandler(AioTCPController controller) {
        this.controller = controller;

    }
 

    @Override
    public void cancelled(AbstractAioSession session) {

        log.warn("Session(" + session.getRemoteSocketAddress()
                + ") read operation was canceled");

    }
 

    @Override
    public void completed(Integer result, AbstractAioSession session) {

        if (log.isDebugEnabled())
            log.debug("Session(" + session.getRemoteSocketAddress()

                    + ") read +" + result + " bytes");
        if (result < 0) {

            session.close();
            return;

        }
        try {

            if (result > 0) {
                session.updateTimeStamp();

                session.getReadBuffer().flip();
                session.decode();

                session.getReadBuffer().compact();
            }

        } finally {
            try {

                session.pendingRead();
            } catch (IOException e) {

                session.onException(e);
                session.close();

            }
        }

        controller.checkSessionTimeout();
    }

 
    @Override

    public void failed(Throwable exc, AbstractAioSession session) {
        log.error("Session read error", exc);

        session.onException(exc);
        session.close();

    }
 

}
 

   如果IO读失败，会返回失败产生的异常，这种情况下我们就主动关闭连接，通过session.close()方法，这个方法干了两件事情：关闭channel和取消read调用：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. if (null != this.readFuture) {  
1.             this.readFuture.cancel(true);  
1.         }  
1. this.asynchronousSocketChannel.close();  

if (null != this.readFuture) {

            this.readFuture.cancel(true);
        }

this.asynchronousSocketChannel.close();
 

   在读成功的情况下，我们还需要判断结果result是否小于0，**如果小于0就表示对端关闭了**，这种情况下我们也主动关闭连接并返回。如果读到一定字节，也就是result大于0的情况下，我们就尝试从读缓冲区中decode出消息，并派发给业务处理器的回调方法，最终**通过pendingRead继续发起read调用等待socket的下一次可读**。可见，我们并不需要自己去调用channel来进行IO读，而是操作系统帮你直接读到了缓冲区，然后给你一个结果表示读入了多少字节，你处理这个结果即可。而nonblocking IO框架中，是reactor通知用户线程socket可读了，然后用户线程自己去调用read进行实际读操作。这里还有个需要注意的地方，就是decode出来的消息的派发给业务处理器工作最好交给一个线程池来处理，避免阻塞group绑定的线程池。
 
   IO写的操作与此类似，不过通常写的话我们会在session中关联一个缓冲队列来处理，没有完全写入或者等待写入的消息都存放在队列中，队列为空的情况下发起write调用：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. protected void write0(WriteMessage message) {  
1.       boolean needWrite = false;  
1.       synchronized (this.writeQueue) {  
1.           needWrite = this.writeQueue.isEmpty();  
1.           this.writeQueue.offer(message);  
1.       }  
1.       if (needWrite) {  
1.           pendingWrite(message);  
1.       }  
1.   }  
1.   
1.   protected final void pendingWrite(WriteMessage message) {  
1.       message = preprocessWriteMessage(message);  
1.       if (!isClosed() && this.asynchronousSocketChannel.isOpen()) {  
1.           this.asynchronousSocketChannel.write(message.getWriteBuffer(),  
1.                   this, this.writeCompletionHandler);  
1.       } else {  
1.           throw new IllegalStateException(  
1.                   "Session Or Channel has been closed");  
1.       }  
1.   }  

  protected void write0(WriteMessage message) {

        boolean needWrite = false;
        synchronized (this.writeQueue) {

            needWrite = this.writeQueue.isEmpty();
            this.writeQueue.offer(message);

        }
        if (needWrite) {

            pendingWrite(message);
        }

    }
 

    protected final void pendingWrite(WriteMessage message) {
        message = preprocessWriteMessage(message);

        if (!isClosed() && this.asynchronousSocketChannel.isOpen()) {
            this.asynchronousSocketChannel.write(message.getWriteBuffer(),

                    this, this.writeCompletionHandler);
        } else {

            throw new IllegalStateException(
                    "Session Or Channel has been closed");

        }
    }

 
 

    write调用返回的结果与read一样是一个Future<Integer>，而write的CompletionHandler处理的核心逻辑大概是这样：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. @Override  
1.     public void completed(Integer result, AbstractAioSession session) {  
1.         if (log.isDebugEnabled())  
1.             log.debug("Session(" + session.getRemoteSocketAddress()  
1.                     + ") writen " + result + " bytes");  
1.                   
1.         WriteMessage writeMessage;  
1.         Queue<WriteMessage> writeQueue = session.getWriteQueue();  
1.         synchronized (writeQueue) {  
1.             writeMessage = writeQueue.peek();  
1.             if (writeMessage.getWriteBuffer() == null  
1.                     || !writeMessage.getWriteBuffer().hasRemaining()) {  
1.                 writeQueue.remove();  
1.                 if (writeMessage.getWriteFuture() != null) {  
1.                     writeMessage.getWriteFuture().setResult(Boolean.TRUE);  
1.                 }  
1.                 try {  
1.                     session.getHandler().onMessageSent(session,  
1.                             writeMessage.getMessage());  
1.                 } catch (Exception e) {  
1.                     session.onException(e);  
1.                 }  
1.                 writeMessage = writeQueue.peek();  
1.             }  
1.         }  
1.         if (writeMessage != null) {  
1.             try {  
1.                 session.pendingWrite(writeMessage);  
1.             } catch (IOException e) {  
1.                 session.onException(e);  
1.                 session.close();  
1.             }  
1.         }  
1.     }  

@Override

    public void completed(Integer result, AbstractAioSession session) {
        if (log.isDebugEnabled())

            log.debug("Session(" + session.getRemoteSocketAddress()
                    + ") writen " + result + " bytes");

 
        WriteMessage writeMessage;

        Queue<WriteMessage> writeQueue = session.getWriteQueue();
        synchronized (writeQueue) {

            writeMessage = writeQueue.peek();
            if (writeMessage.getWriteBuffer() == null

                    || !writeMessage.getWriteBuffer().hasRemaining()) {
                writeQueue.remove();

                if (writeMessage.getWriteFuture() != null) {
                    writeMessage.getWriteFuture().setResult(Boolean.TRUE);

                }
                try {

                    session.getHandler().onMessageSent(session,
                            writeMessage.getMessage());

                } catch (Exception e) {
                    session.onException(e);

                }
                writeMessage = writeQueue.peek();

            }
        }

        if (writeMessage != null) {
            try {

                session.pendingWrite(writeMessage);
            } catch (IOException e) {

                session.onException(e);
                session.close();

            }
        }

    }
 

   compete方法中的result就是实际写入的字节数，然后我们判断消息的缓冲区是否还有剩余，如果没有就将消息从队列中移除，如果队列中还有消息，那么继续发起write调用。
   重复一下，这里引用的代码都是yanf4j aio分支中的源码，感兴趣的朋友可以直接check out出来看看: [http://yanf4j.googlecode.com/svn/branches/yanf4j-aio](http://yanf4j.googlecode.com/svn/branches/yanf4j-aio)。
   在引入了aio之后，java对于网络层的支持已经非常完善，该有的都有了，java也已经成为服务器开发的首选语言之一。java的弱项在于对内存的管理上，由于这一切都交给了GC，因此在高性能的网络服务器上还是Cpp的天下。java这种单一堆模型比之erlang的进程内堆模型还是有差距，很难做到高效的垃圾回收和细粒度的内存管理。
   这里仅仅是介绍了aio开发的核心流程，对于一个网络框架来说，还需要考虑超时的处理、缓冲buffer的处理、业务层和网络层的切分、可扩展性、性能的可调性以及一定的通用性要求。

 

刚看了一点，第一行有个错别字，不是IO服用，是复用，嘿嘿
老大终于开始介绍java NIO了。。
还有一点，看过一些源代码，对事件驱动还是理解不深，也请老大介绍下把。
rain2005 写道

刚看了一点，第一行有个错别字，不是IO服用，是复用，嘿嘿
老大终于开始介绍java NIO了。。
还有一点，看过一些源代码，对事件驱动还是理解不深，也请老大介绍下把。
多谢指正，关于事件机制，我会画个UML图可能比较清晰

上面提到几个类 怎么在jdk6中没发现那
是不是要下载第三方框架
正好想研究一下
收藏啦
xly_971223 写道

上面提到几个类 怎么在jdk6中没发现那
是不是要下载第三方框架
正好想研究一下
收藏啦
aio是在jdk7引入的，请下载JDK7 preview1版本，或者open jdk

dennis_zane 写道

xly_971223 写道

上面提到几个类 怎么在jdk6中没发现那
是不是要下载第三方框架
正好想研究一下
收藏啦
aio是在jdk7引入的，请下载JDK7 preview1版本，或者open jdk
是不是这个意思。
jdk7以前的nio是非阻塞IO,操作系统底层比方说linux,是用IO复用select实现的
jdk7用的是真正的异步IO,操作系统底层是用epoll实现的
是这样的吗？
rain2005 写道

dennis_zane 写道

xly_971223 写道

上面提到几个类 怎么在jdk6中没发现那
是不是要下载第三方框架
正好想研究一下
收藏啦
aio是在jdk7引入的，请下载JDK7 preview1版本，或者open jdk
是不是这个意思。
jdk7以前的nio是非阻塞IO,操作系统底层比方说linux,是用IO复用select实现的
jdk7用的是真正的异步IO,操作系统底层是用epoll实现的
是这样的吗？
epoll也不是异步IO啊。异步IO在linux上目前仅限于文件系统，并且还没有得到广泛应用，很多平台都没有这玩意。
java aio在windows上是利用iocp实现的，这是真正的异步IO。而在linux上，是通过epoll模拟的。

楼主，你好，我写的server是p2p的应用，恩，想请教一下，因为我对数据库这一块的操作并不是特别的多，基本是客户自己也有很多是服务器，不知道是否可以在事件响应的当前线程来对数据库操作呢？前提是把线程池子的数目设的大些？或者用那个JDK提供的可自己增加线程的池子？
因为如果在弄个池子来处理数据库的话，担心线程太多了，
你如果时间充分心情好的话，真希望你能讲解一下和别人公用一台服务器（主机）是怎么用的呢。。。
总之要谢谢你对这段代码的讲解，kang sang mi da
wujingsong 写道

楼主，你好，我写的server是p2p的应用，恩，想请教一下，因为我对数据库这一块的操作并不是特别的多，基本是客户自己也有很多是服务器，不知道是否可以在事件响应的当前线程来对数据库操作呢？前提是把线程池子的数目设的大些？或者用那个JDK提供的可自己增加线程的池子？
因为如果在弄个池子来处理数据库的话，担心线程太多了，
你如果时间充分心情好的话，真希望你能讲解一下和别人公用一台服务器（主机）是怎么用的呢。。。
总之要谢谢你对这段代码的讲解，kang sang mi da
按我的经验来说，类似数据库操作这样的IO操作，最好还是起个线程池来处理，防止阻塞框架内部的处理线程。如果这样的操作不是特别多，那么直接在响应线程处理也未尝不可，还是建议你自己搞两个版本性能对比一下。
wujingsong 写道

和别人公用一台是怎么用的呢？
我还真不明白什么意思，现在我们的应用基本都跑在虚拟机上了，几个应用跑在一个物理机上。虚拟化我不懂，就不乱弹了。

kang sa mi da,谢谢楼主的回复,确实是个很好的建议.
闲聊啊,今天无意看到Google 上一个音乐的图片链接,点进去后,看到了一个不大容易理解的词,说什么
            "在南中国常年保持高收听率的极有个性的节目",
费解.....广东那边是这么叫的吗?不大可能吧..
 
