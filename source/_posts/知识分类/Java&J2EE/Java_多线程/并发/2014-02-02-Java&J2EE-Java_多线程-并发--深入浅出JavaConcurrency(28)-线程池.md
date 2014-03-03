---
layout: post
title: "深入浅出 Java Concurrency (28)- 线程池"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_多线程
 - 并发
--- 

# 深入浅出 Java Concurrency (28): 线程池

[ 简介](http://www.blogjava.net/xylz/archive/2010/12/19/341098.html)

从这一节开始正式进入线程池的部分。其实整个体系已经拖了很长的时间，因此后面的章节会加快速度，甚至只是一个半成品或者简单化，以后有时间的慢慢补充、完善。

其实线程池是并发包里面很重要的一部分，在实际情况中也是使用很多的一个重要组件。

下图描述的是线程池API的一部分。广义上的完整线程池可能还包括Thread/Runnable、Timer/TimerTask等部分。这里只介绍主要的和高级的API以及架构和原理。

 

 
[![ThreadPool2]( "ThreadPool2")](http://www.blogjava.net/images/blogjava_net/xylz/Windows-Live-Writer/-part-1-_8E6F/ThreadPool2_2.png)

 

大多数并发应用程序是围绕执行任务（Task）进行管理的。所谓任务就是抽象、离散的工作单元（unit of work）。把一个应用程序的工作（work）分离到任务中，可以简化程序的管理；这种分离还在不同事物间划分了自然的分界线，可以方便程序在出现错误时进行恢复；同时这种分离还可以为并行工作提供一个自然的结构，有利于提高程序的并发性。[[1]](http://www.blogjava.net/xylz/archive/2010/12/19/341098.html#jcp)

 

并发执行任务的一个很重要前提是拆分任务。把一个大的过程或者任务拆分成很多小的工作单元，每一个工作单元可能相关、也可能无关，这些单元在一定程度上可以充分利用CPU的特性并发的执行，从而提高并发性（性能、响应时间、吞吐量等）。

所谓的任务拆分就是确定每一个执行任务（工作单元）的边界。理想情况下独立的工作单元有最大的吞吐量，这些工作单元不依赖于其它工作单元的状态、结果或者其他资源等。因此将任务尽可能的拆分成一个个独立的工作单元有利于提高程序的并发性。

对于有依赖关系以及资源竞争的工作单元就涉及到任务的调度和负载均衡。工作单元的状态、结果或者其他资源等有关联的工作单元就需要有一个总体的调度者来协调资源和执行顺序。同样在有限的资源情况下，大量的任务也需要一个协调各个工作单元的调度者。这就涉及到任务执行的策略问题。

任务的执行策略包括4W3H部分：

* 任务在什么（What）线程中执行
* 任务以什么（What）顺序执行（FIFO/LIFO/优先级等）
* 同时有多少个（How Many）任务并发执行
* 允许有多少个（How Many）个任务进入执行队列
* 系统过载时选择放弃哪一个（Which）任务，如何（How）通知应用程序这个动作
* 任务执行的开始、结束应该做什么（What）处理

在后面的章节中会详细分写这些策略是如何实现的。我们先来简单回答些如何满足上面的条件。

1. 首先明确一定是在Java里面可以供使用者调用的启动线程类是Thread。因此Runnable或者Timer/TimerTask等都是要依赖Thread来启动的，因此在ThreadPool里面同样也是靠Thread来启动多线程的。
1. 默认情况下Runnable接口执行完毕后是不能拿到执行结果的，因此在ThreadPool里就定义了一个Callable接口来处理执行结果。
1. 为了异步阻塞的获取结果，Future可以帮助调用线程获取执行结果。
1. Executor解决了向线程池提交任务的入口问题，同时ScheduledExecutorService解决了如何进行重复调用任务的问题。
1. CompletionService解决了如何按照执行完毕的顺序获取结果的问题，这在某些情况下可以提高任务执行的并发，调用线程不必在长时间任务上等待过多时间。
1. 显然线程的数量是有限的，而且也不宜过多，因此合适的任务队列是必不可少的，BlockingQueue的容量正好可以解决此问题。
1. 固定任务容量就意味着在容量满了以后需要一定的策略来处理过多的任务（新任务），RejectedExecutionHandler正好解决此问题。
1. 一定时间内阻塞就意味着有超时，因此TimeoutException就是为了描述这种现象。TimeUnit是为了描述超时时间方便的一个时间单元枚举类。
1. 有上述问题就意味了配置一个合适的线程池是很复杂的，因此Executors默认的一些线程池配置可以减少这个操作。

线程池的基本策略大致就这些，从下一节开始就从线程池的基本原理和执行方法开始描述。

 

[[1] Java Concurrency in Practice]()
来源： <[http://www.blogjava.net/xylz/archive/2010/12/19/341098.html](http://www.blogjava.net/xylz/archive/2010/12/19/341098.html)> [Executor 以及Executors](http://www.blogjava.net/xylz/archive/2010/12/21/341281.html)
Java里面线程池的顶级接口是Executor，但是严格意义上讲Executor并不是一个线程池，而只是一个执行线程的工具。真正的线程池接口是ExecutorService。

下面这张图完整描述了线程池的类体系结构。

[![Executor-class]( "Executor-class")](http://www.blogjava.net/images/blogjava_net/xylz/Windows-Live-Writer/-Java-Concurrency-28--part-1-_1302E/Executor-class_2.png)

首先Executor的execute方法只是执行一个Runnable的任务，当然了从某种角度上将最后的实现类也是在线程中启动此任务的。根据线程池的执行策略最后这个任务可能在新的线程中执行，或者线程池中的某个线程，甚至是调用者线程中执行（相当于直接运行Runnable的run方法）。这点在后面会详细说明。

ExecutorService在Executor的基础上增加了一些方法，其中有两个核心的方法：

* Future<?> submit(Runnable task)
* <T> Future<T> submit(Callable<T> task)

这两个方法都是向线程池中提交任务，它们的区别在于Runnable在执行完毕后没有结果，Callable执行完毕后有一个结果。这在多个线程中传递状态和结果是非常有用的。另外他们的相同点在于都返回一个Future对象。Future对象可以阻塞线程直到运行完毕（获取结果，如果有的话），也可以取消任务执行，当然也能够检测任务是否被取消或者是否执行完毕。

在没有Future之前我们检测一个线程是否执行完毕通常使用Thread.join()或者用一个死循环加状态位来描述线程执行完毕。现在有了更好的方法能够阻塞线程，检测任务执行完毕甚至取消执行中或者未开始执行的任务。

 

ScheduledExecutorService描述的功能和Timer/TimerTask类似，解决那些需要任务重复执行的问题。这包括延迟时间一次性执行、延迟时间周期性执行以及固定延迟时间周期性执行等。当然了继承ExecutorService的ScheduledExecutorService拥有ExecutorService的全部特性。

 

ThreadPoolExecutor是ExecutorService的默认实现，其中的配置、策略也是比较复杂的，在后面的章节中会有详细的分析。

ScheduledThreadPoolExecutor是继承ThreadPoolExecutor的ScheduledExecutorService接口实现，周期性任务调度的类实现，在后面的章节中会有详细的分析。

这里需要稍微提一下的是CompletionService接口，它是用于描述顺序获取执行结果的一个线程池包装器。它依赖一个具体的线程池调度，但是能够根据任务的执行先后顺序得到执行结果，这在某些情况下可能提高并发效率。

 

要配置一个线程池是比较复杂的，尤其是对于线程池的原理不是很清楚的情况下，很有可能配置的线程池不是较优的，因此在Executors类里面提供了一些静态工厂，生成一些常用的线程池。

* **newSingleThreadExecutor**：创建一个单线程的线程池。这个线程池只有一个线程在工作，也就是相当于单线程串行执行所有任务。如果这个唯一的线程因为异常结束，那么会有一个新的线程来替代它。此线程池保证所有任务的执行顺序按照任务的提交顺序执行。
* **newFixedThreadPool**：创建固定大小的线程池。每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。线程池的大小一旦达到最大值就会保持不变，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程。
* **newCachedThreadPool**：创建一个可缓存的线程池。如果线程池的大小超过了处理任务所需要的线程，那么就会回收部分空闲（60秒不执行任务）的线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。
* **newScheduledThreadPool**：创建一个大小无限的线程池。此线程池支持定时以及周期性执行任务的需求。
* **newSingleThreadScheduledExecutor**：创建一个单线程的线程池。此线程池支持定时以及周期性执行任务的需求。

在详细讲解ThreadPoolExecutor的时候会具体讨论上述参数配置后的意义和原理。

线程池是一个复杂的任务调度工具，因此它涉及到任务、线程池等的生命周期问题，在下一节中来探讨下这个问题。
来源： <[http://www.blogjava.net/xylz/archive/2010/12/21/341281.html](http://www.blogjava.net/xylz/archive/2010/12/21/341281.html)> [Executor 生命周期](http://www.blogjava.net/xylz/archive/2011/01/04/342316.html)

我们知道线程是有多种执行状态的，同样管理线程的线程池也有多种状态。JVM会在所有线程（非后台daemon线程）全部终止后才退出，为了节省资源和有效释放资源关闭一个线程池就显得很重要。有时候无法正确的关闭线程池，将会阻止JVM的结束。

线程池Executor是异步的执行任务，因此任何时刻不能够直接获取提交的任务的状态。这些任务有可能已经完成，也有可能正在执行或者还在排队等待执行。因此关闭线程池可能出现一下几种情况：

* 平缓关闭：已经启动的任务全部执行完毕，同时不再接受新的任务
* 立即关闭：取消所有正在执行和未执行的任务

另外关闭线程池后对于任务的状态应该有相应的反馈信息。

图1 描述了线程池的4种状态。

* 线程池在构造前（new操作）是初始状态，一旦构造完成线程池就进入了执行状态RUNNING。严格意义上讲线程池构造完成后并没有线程被立即启动，只有进行“预启动”或者接收到任务的时候才会启动线程。这个会后面线程池的原理会详细分析。但是线程池是出于运行状态，随时准备接受任务来执行。
* 线程池运行中可以通过shutdown()和shutdownNow()来改变运行状态。shutdown()是一个平缓的关闭过程，线程池停止接受新的任务，同时等待已经提交的任务执行完毕，包括那些进入队列还没有开始的任务，这时候线程池处于SHUTDOWN状态；shutdownNow()是一个立即关闭过程，线程池停止接受新的任务，同时线程池取消所有执行的任务和已经进入队列但是还没有执行的任务，这时候线程池处于STOP状态。
* 一旦shutdown()或者shutdownNow()执行完毕，线程池就进入TERMINATED状态，此时线程池就结束了。
* isTerminating()描述的是SHUTDOWN和STOP两种状态。
* isShutdown()描述的是非RUNNING状态，也就是SHUTDOWN/STOP/TERMINATED三种状态。

 

[![Executor-Lifecycle]( "Executor-Lifecycle")](http://www.blogjava.net/images/blogjava_net/xylz/Windows-Live-Writer/-part-3-Executor-_12486/Executor-Lifecycle_4.png)

图1

线程池的API如下：

[![ExecutorService-LifeCycle]( "ExecutorService-LifeCycle")](http://www.blogjava.net/images/blogjava_net/xylz/Windows-Live-Writer/-part-3-Executor-_12486/ExecutorService-LifeCycle_2.png)

图2

其中shutdownNow()会返回那些已经进入了队列但是还没有执行的任务列表。awaitTermination描述的是等待线程池关闭的时间，如果等待时间线程池还没有关闭将会抛出一个超时异常。

对于关闭线程池期间发生的任务提交情况就会触发一个拒绝执行的操作。这是java.util.concurrent.RejectedExecutionHandler描述的任务操作。下一个小结中将描述这些任务被拒绝后的操作。

 

总结下这个小节：

1. 线程池有运行、关闭、停止、结束四种状态，结束后就会释放所有资源
1. 平缓关闭线程池使用shutdown()
1. 立即关闭线程池使用shutdownNow()，同时得到未执行的任务列表
1. 检测线程池是否正处于关闭中，使用isShutdown()
1. 检测线程池是否已经关闭使用isTerminated()
1. 定时或者永久等待线程池关闭结束使用awaitTermination()操作

来源： <[http://www.blogjava.net/xylz/archive/2011/01/04/342316.html](http://www.blogjava.net/xylz/archive/2011/01/04/342316.html)>
 

**线程池数据结构与线程构造方法**

由于已经看到了ThreadPoolExecutor的源码，因此很容易就看到了ThreadPoolExecutor线程池的数据结构。图1描述了这种数据结构。

[![ThreadPoolExecutor]( "ThreadPoolExecutor")](http://www.blogjava.net/images/blogjava_net/xylz/Windows-Live-Writer/-Java-Concurrency-32--part-5-_72AF/ThreadPoolExecutor_2.png)

图1 ThreadPoolExecutor 数据结构

其实，即使没有上述图形描述ThreadPoolExecutor的数据结构，我们根据线程池的要求也很能够猜测出其数据结构出来。

* 线程池需要支持多个线程并发执行，因此有一个线程集合Collection<Thread>来执行线程任务；
* 涉及任务的异步执行，因此需要有一个集合来缓存任务队列Collection<Runnable>；
* 很显然在多个线程之间协调多个任务，那么就需要一个线程安全的任务集合，同时还需要支持阻塞、超时操作，那么BlockingQueue是必不可少的；
* 既然是线程池，出发点就是提高系统性能同时降低资源消耗，那么线程池的大小就有限制，因此需要有一个核心线程池大小（线程个数）和一个最大线程池大小（线程个数），有一个计数用来描述当前线程池大小；
* 如果是有限的线程池大小，那么长时间不使用的线程资源就应该销毁掉，这样就需要一个线程空闲时间的计数来描述线程何时被销毁；
* 前面描述过线程池也是有生命周期的，因此需要有一个状态来描述线程池当前的运行状态；
* 线程池的任务队列如果有边界，那么就需要有一个任务拒绝策略来处理过多的任务，同时在线程池的销毁阶段也需要有一个任务拒绝策略来处理新加入的任务；
* 上面种的线程池大小、线程空闲实际那、线程池运行状态等等状态改变都不是线程安全的，因此需要有一个全局的锁（mainLock）来协调这些竞争资源；
* 除了以上数据结构以外，ThreadPoolExecutor还有一些状态用来描述线程池的运行计数，例如线程池运行的任务数、曾经达到的最大线程数，主要用于调试和性能分析。

 

对于ThreadPoolExecutor而言，一个线程就是一个Worker对象，它与一个线程绑定，当Worker执行完毕就是线程执行完毕，这个在后面详细讨论线程池中线程的运行方式。

既然是线程池，那么就首先研究下线程的构造方法。
public interface ThreadFactory {
    Thread newThread(Runnable r);
}

 

ThreadPoolExecutor使用一个线程工厂来构造线程。线程池都是提交一个任务Runnable，然后在某一个线程Thread中执行，ThreadFactory 负责如何创建一个新线程。

在J.U.C中有一个通用的线程工厂java.util.concurrent.Executors.DefaultThreadFactory，它的构造方式如下：
static class DefaultThreadFactory implements ThreadFactory {
    static final AtomicInteger poolNumber = new AtomicInteger(1);
    final ThreadGroup group;
    final AtomicInteger threadNumber = new AtomicInteger(1);
    final String namePrefix;
    DefaultThreadFactory() {
        SecurityManager s = System.getSecurityManager();
        group = (s != null)? s.getThreadGroup() :
                             Thread.currentThread().getThreadGroup();
        namePrefix = "pool-" +
                      poolNumber.getAndIncrement() +
                     "-thread-";
    }
    public Thread newThread(Runnable r) {
        Thread t = new Thread(group, r,
                              namePrefix + threadNumber.getAndIncrement(),
                              0);
        if (t.isDaemon())
            t.setDaemon(false);
        if (t.getPriority() != Thread.NORM_PRIORITY)
            t.setPriority(Thread.NORM_PRIORITY);
        return t;
    }
}

 

在这个线程工厂中，同一个线程池的所有线程属于同一个线程组，也就是创建线程池的那个线程组，同时线程池的名称都是“pool-<poolNum>-thread-<threadNum>”，其中poolNum是线程池的数量序号，threadNum是此线程池中的线程数量序号。这样如果使用jstack的话很容易就看到了系统中线程池的数量和线程池中线程的数量。另外对于线程池中的所有线程默认都转换为非后台线程，这样主线程退出时不会直接退出JVM，而是等待线程池结束。还有一点就是默认将线程池中的所有线程都调为同一个级别，这样在操作系统角度来看所有系统都是公平的，不会导致竞争堆积。

**线程池中线程生命周期**

一个线程Worker被构造出来以后就开始处于运行状态。以下是一个线程执行的简版逻辑。
private final class Worker implements Runnable {
    private final ReentrantLock runLock = new ReentrantLock();
    private Runnable firstTask;
    Thread thread;
    Worker(Runnable firstTask) {
        this.firstTask = firstTask;
    }
    private void runTask(Runnable task) {
        final ReentrantLock runLock = this.runLock;
        runLock.lock();
        try {
           task.run();
        } finally {
            runLock.unlock();
        }
    }
    public void run() {
        try {
            Runnable task = firstTask;
            firstTask = null;
            while (task != null || (task = getTask()) != null) {
                runTask(task);
                task = null;
            }
        } finally {
            workerDone(this);
        }
    }
}

 

[![ThreadPoolExecutor-Worker]( "ThreadPoolExecutor-Worker")](http://www.blogjava.net/images/blogjava_net/xylz/Windows-Live-Writer/-Java-Concurrency-32--part-5-_72AF/ThreadPoolExecutor-Worker_2.png)

当提交一个任务时，如果需要创建一个线程（何时需要在下一节中探讨）时，就调用线程工厂创建一个线程，同时将线程绑定到Worker工作队列中。需要说明的是，Worker队列构造的时候带着一个任务Runnable，因此Worker创建时总是绑定着一个待执行任务。换句话说，创建线程的前提是有必要创建线程（任务数已经超出了线程或者强制创建新的线程，至于为何强制创建新的线程后面章节会具体分析），不会无缘无故创建一堆空闲线程等着任务。这是节省资源的一种方式。

一旦线程池启动线程后（调用线程run()）方法，那么线程工作队列Worker就从第1个任务开始执行（这时候发现构造Worker时传递一个任务的好处了），一旦第1个任务执行完毕，就从线程池的任务队列中取出下一个任务进行执行。循环如此，直到线程池被关闭或者任务抛出了一个RuntimeException。

由此可见，线程池的基本原理其实也很简单，无非预先启动一些线程，线程进入死循环状态，每次从任务队列中获取一个任务进行执行，直到线程池被关闭。如果某个线程因为执行某个任务发生异常而终止，那么重新创建一个新的线程而已。如此反复。

其实，线程池原理看起来简单，但是复杂的是各种策略，例如何时该启动一个线程，何时该终止、挂起、唤醒一个线程，任务队列的阻塞与超时，线程池的生命周期以及任务拒绝策略等等。下一节将研究这些策略问题。

 
来源： <[http://www.blogjava.net/xylz/archive/2011/01/18/343183.html](http://www.blogjava.net/xylz/archive/2011/01/18/343183.html)> 

**线程池任务执行流程**

我们从一个API开始接触Executor是如何处理任务队列的。

java.util.concurrent.Executor.execute(Runnable)
Executes the given task sometime in the future. The task may execute in a new thread or in an existing pooled thread. If the task cannot be submitted for execution, either because this executor has been shutdown or because its capacity has been reached, the task is handled by the current RejectedExecutionHandler.

线程池中所有任务执行都依赖于此接口。这段话有以下几个意思：

1. 任务可能在将来某个时刻被执行，有可能不是立即执行。为什么这里有两个“可能”？继续往下面看。
1. 任务可能在一个新的线程中执行或者线程池中存在的一个线程中执行。
1. 任务无法被提交执行有以下两个原因：线程池已经关闭或者线程池已经达到了容量限制。
1. 所有失败的任务都将被“当前”的任务拒绝策略RejectedExecutionHandler 处理。

回答上面两个“可能“。任务可能被执行，那不可能的情况就是上面说的情况3；可能不是立即执行，是因为任务可能还在队列中排队，因此还在等待分配线程执行。了解完了字面上的问题，我们再来看具体的实现。
public void execute(Runnable command) {
    if (command == null)
        throw new NullPointerException();
    if (poolSize >= corePoolSize || !addIfUnderCorePoolSize(command)) {
        if (runState == RUNNING && workQueue.offer(command)) {
            if (runState != RUNNING || poolSize == 0)
                ensureQueuedTaskHandled(command);
        }
        else if (!addIfUnderMaximumPoolSize(command))
            reject(command); // is shutdown or saturated
    }
}

这一段代码看起来挺简单的，其实这就是线程池最重要的一部分，如果能够完全理解这一块，线程池还是挺容易的。整个执行流程是这样的：

1. 如果任务command为空，则抛出空指针异常，返回。否则进行2。
1. 如果当前线程池大小 大于或等于 核心线程池大小，进行4。否则进行3。
1. 创建一个新工作队列（线程，参考上一节），成功直接返回，失败进行4。
1. 如果线程池正在运行并且任务加入线程池队列成功，进行5，否则进行7。
1. 如果线程池已经关闭或者线程池大小为0，进行6，否则直接返回。
1. 如果线程池已经关闭则执行拒绝策略返回，否则启动一个新线程来进行执行任务，返回。
1. 如果线程池大小 不大于 最大线程池数量，则启动新线程来进行执行，否则进行拒绝策略，结束。

文字描述步骤不够简单？下面图形详细表述了此过程。

[![Executor.execute]( "Executor.execute")](http://www.blogjava.net/images/blogjava_net/xylz/Windows-Live-Writer/-Java-Concurrency-34--part-7--2_BFAE/Executor.execute_8.png)

老实说这个图比上面步骤更难以理解，那么从何入手呢。

流程的入口很简单，我们就是要执行一个任务（Runnable command)，那么它的结束点在哪或者有哪几个？

根据左边这个图我们知道可能有以下几种出口：

（1）图中的P1、P7，我们根据这条路径可以看到，仅仅是将任务加入任务队列（offer(command)）了；

（2）图中的P3，这条路径不将任务加入任务队列，但是启动了一个新工作线程（Worker）进行扫尾操作，用户处理为空的任务队列；

（3）图中的P4，这条路径没有将任务加入任务队列，但是启动了一个新工作线程（Worker），并且工作现场的第一个任务就是当前任务；

（4）图中的P5、P6，这条路径没有将任务加入任务队列，也没有启动工作线程，仅仅是抛给了任务拒绝策略。P2是任务加入了任务队列却因为线程池已经关闭于是又从任务队列中删除，并且抛给了拒绝策略。

如果上面的解释还不清楚，可以去研究下面两段代码：
java.util.concurrent.ThreadPoolExecutor.addIfUnderCorePoolSize(Runnable)
java.util.concurrent.ThreadPoolExecutor.addIfUnderMaximumPoolSize(Runnable)
java.util.concurrent.ThreadPoolExecutor.ensureQueuedTaskHandled(Runnable)

那么什么时候一个任务被立即执行呢？

在线程池运行状态下，如果线程池大小 小于 核心线程池大小或者线程池已满（任务队列已满）并且线程池大小 小于 最大线程池大小（此时线程池大小 大于 核心线程池大小的），用程序描述为：
runState == RUNNING && ( poolSize < corePoolSize || poolSize < maxnumPoolSize && workQueue.isFull())

上面的条件就是一个任务能够被立即执行的条件。

有了execute的基础，我们看看ExecutorService中的几个submit方法的实现。
    public Future<?> submit(Runnable task) {
        if (task == null) throw new NullPointerException();
        RunnableFuture<Object> ftask = newTaskFor(task, null);
        execute(ftask);
        return ftask;
    }
    public <T> Future<T> submit(Runnable task, T result) {
        if (task == null) throw new NullPointerException();
        RunnableFuture<T> ftask = newTaskFor(task, result);
        execute(ftask);
        return ftask;
    }
    public <T> Future<T> submit(Callable<T> task) {
        if (task == null) throw new NullPointerException();
        RunnableFuture<T> ftask = newTaskFor(task);
        execute(ftask);
        return ftask;
    }

很简单，不是么？对于一个线程池来说复杂的地方也就在execute方法的执行流程。在下一节中我们来讨论下如何获取任务的执行结果，也就是Future类的使用和原理。

来源： <[http://www.blogjava.net/xylz/archive/2011/02/11/344091.html](http://www.blogjava.net/xylz/archive/2011/02/11/344091.html)> 

**线程池任务执行结果**

这一节来探讨下线程池中任务执行的结果以及如何阻塞线程、取消任务等等。
1 package info.imxylz.study.concurrency.future;
2 
3 public class SleepForResultDemo implements Runnable {
4 
5     static boolean result = false;
6 
7     static void sleepWhile(long ms) {
8         try {
9             Thread.sleep(ms);
10         } catch (Exception e) {}
11     }
12 
13     @Override
14     public void run() {
15         //do work
16         System.out.println("Hello, sleep a while.");
17         sleepWhile(2000L);
18         result = true;
19     }
20 
21     public static void main(String[] args) {
22         SleepForResultDemo demo = new SleepForResultDemo();
23         Thread t = new Thread(demo);
24         t.start();
25         sleepWhile(3000L);
26         System.out.println(result);
27     }
28 
29 }
30 

在没有线程池的时代里面，使用Thread.sleep(long)去获取线程执行完毕的场景很多。显然这种方式很笨拙，他需要你事先知道任务可能的执行时间，并且还会阻塞主线程，不管任务有没有执行完毕。

1 package info.imxylz.study.concurrency.future;
2 
3 public class SleepLoopForResultDemo implements Runnable {
4 
5     boolean result = false;
6 
7     volatile boolean finished = false;
8 
9     static void sleepWhile(long ms) {
10         try {
11             Thread.sleep(ms);
12         } catch (Exception e) {}
13     }
14 
15     @Override
16     public void run() {
17         //do work
18         try {
19             System.out.println("Hello, sleep a while.");
20             sleepWhile(2000L);
21             result = true;
22         } finally {
23             finished = true;
24         }
25     }
26 
27     public static void main(String[] args) {
28         SleepLoopForResultDemo demo = new SleepLoopForResultDemo();
29         Thread t = new Thread(demo);
30         t.start();
31         while (!demo.finished) {
32             sleepWhile(10L);
33         }
34         System.out.println(demo.result);
35     }
36 
37 }
38 

使用volatile与while死循环的好处就是等待的时间可以稍微小一点，但是依然有CPU负载高并且阻塞主线程的问题。最简单的降低CPU负载的方式就是使用Thread.join().

        SleepLoopForResultDemo demo = new SleepLoopForResultDemo();
        Thread t = new Thread(demo);
        t.start();
        t.join();
        System.out.println(demo.result);

显然这也是一种不错的方式，另外还有自己写锁使用wait/notify的方式。其实join()从本质上讲就是利用while和wait来实现的。

上面的方式中都存在一个问题，那就是会阻塞主线程并且任务不能被取消。为了解决这个问题，线程池中提供了一个Future接口。

[![ThreadPoolExecutor-Future]( "ThreadPoolExecutor-Future")](http://www.blogjava.net/images/blogjava_net/xylz/Windows-Live-Writer/-Java-Concurrency-35--part-8--2_BFEA/ThreadPoolExecutor-Future_2.png)

在Future接口中提供了5个方法。

* V get() throws InterruptedException, ExecutionException： 等待计算完成，然后获取其结果。
* V get(long timeout, TimeUnit unit) throws InterruptedException, ExecutionException, TimeoutException。最多等待为使计算完成所给定的时间之后，获取其结果（如果结果可用）。
* boolean cancel(boolean mayInterruptIfRunning)：试图取消对此任务的执行。
* boolean isCancelled()：如果在任务正常完成前将其取消，则返回 true。
* boolean isDone()：如果任务已完成，则返回 true。 可能由于正常终止、异常或取消而完成，在所有这些情况中，此方法都将返回 true。

API看起来容易，来研究下异常吧。get()请求获取一个结果会阻塞当前进程，并且可能抛出以下三种异常：

* InterruptedException：执行任务的线程被中断则会抛出此异常，此时不能知道任务是否执行完毕，因此其结果是无用的，必须处理此异常。
* ExecutionException：任务执行过程中(Runnable#run()）方法可能抛出RuntimeException，如果提交的是一个java.util.concurrent.Callable<V>接口任务，那么java.util.concurrent.Callable.call()方法有可能抛出任意异常。
* CancellationException：实际上get()方法还可能抛出一个CancellationException的RuntimeException，也就是任务被取消了但是依然去获取结果。

对于get(long timeout, TimeUnit unit)而言，除了get()方法的异常外，由于有超时机制，因此还可能得到一个TimeoutException。

boolean cancel(boolean mayInterruptIfRunning)方法比较复杂，各种情况比较多：

1. 如果任务已经执行完毕，那么返回false。
1. 如果任务已经取消，那么返回false。
1. 循环直到设置任务为取消状态，对于未启动的任务将永远不再执行，对于正在运行的任务，将根据mayInterruptIfRunning是否中断其运行，如果不中断那么任务将继续运行直到结束。
1. 此方法返回后任务要么处于运行结束状态，要么处于取消状态。isDone()将永远返回true，如果cancel()方法返回true，isCancelled()始终返回true。

来看看Future接口的实现类java.util.concurrent.FutureTask<V>具体是如何操作的。

在FutureTask中使用了一个[AQS](http://www.blogjava.net/xylz/archive/2010/07/06/325390.html)数据结构来完成各种状态以及加锁、阻塞的实现。

在此AQS类java.util.concurrent.FutureTask.Sync中一个任务用4中状态：

[![ThreadPoolExecutor-FutureTask-state]( "ThreadPoolExecutor-FutureTask-state")](http://www.blogjava.net/images/blogjava_net/xylz/Windows-Live-Writer/-Java-Concurrency-35--part-8--2_BFEA/ThreadPoolExecutor-FutureTask-state_2.png)

初始情况下任务状态state=0，任务执行(innerRun)后状态变为运行状态RUNNING(state=1)，执行完毕后变成运行结束状态RAN(state=2)。任务在初始状态或者执行状态被取消后就变为状态CANCELLED(state=4)。[AQS](http://www.blogjava.net/xylz/archive/2010/07/06/325390.html)最擅长无锁情况下处理几种简单的状态变更的。
        void innerRun() {
            if (!compareAndSetState(0, RUNNING))
                return;
            try {
                runner = Thread.currentThread();
                if (getState() == RUNNING) // recheck after setting thread
                    innerSet(callable.call());
                else
                    releaseShared(0); // cancel
            } catch (Throwable ex) {
                innerSetException(ex);
            }
        }

执行一个任务有四步：设置运行状态、设置当前线程（AQS需要）、执行任务(Runnable#run或者Callable#call）、设置执行结果。这里也可以看到，一个任务只能执行一次，因为执行完毕后它的状态不在为初始值0，要么为CANCELLED，要么为RAN。

取消一个任务(cancel)又是怎样进行的呢？对比下前面取消任务的描述是不是很简单，这里无非利用AQS的状态来改变任务的执行状态，最终达到放弃未启动或者正在执行的任务的目的。
boolean innerCancel(boolean mayInterruptIfRunning) {
    for (;;) {
        int s = getState();
        if (ranOrCancelled(s))
            return false;
        if (compareAndSetState(s, CANCELLED))
            break;
    }
    if (mayInterruptIfRunning) {
        Thread r = runner;
        if (r != null)
            r.interrupt();
    }
    releaseShared(0);
    done();
    return true;
}

到目前为止我们依然没有说明到底是如何阻塞获取一个结果的。下面四段代码描述了这个过程。

1     V innerGet() throws InterruptedException, ExecutionException {
2         acquireSharedInterruptibly(0);
3         if (getState() == CANCELLED)
4             throw new CancellationException();
5         if (exception != null)
6             throw new ExecutionException(exception);
7         return result;
8     }
9     //AQS#acquireSharedInterruptibly
10     public final void acquireSharedInterruptibly(int arg) throws InterruptedException {
11         if (Thread.interrupted())
12             throw new InterruptedException();
13         if (tryAcquireShared(arg) < 0)
14             doAcquireSharedInterruptibly(arg); //park current Thread for result
15     }
16     protected int tryAcquireShared(int ignore) {
17         return innerIsDone()? 1 : -1;
18     }
19 
20     boolean innerIsDone() {
21         return ranOrCancelled(getState()) && runner == null;
22     }

当调用Future#get()的时候尝试去获取一个共享变量。这就涉及到AQS的使用方式了。这里获取一个共享变量的状态是任务是否结束(innerIsDone())，也就是任务是否执行完毕或者被取消。如果不满足条件，那么在AQS中就会doAcquireSharedInterruptibly(arg)挂起当前线程，直到满足条件。AQS前面讲过，挂起线程使用的是LockSupport的park方式，因此性能消耗是很低的。

至于将Runnable接口转换成Callable接口，java.util.concurrent.Executors.callable(Runnable, T)也提供了一个简单实现。
    static final class RunnableAdapter<T> implements Callable<T> {
        final Runnable task;
        final T result;
        RunnableAdapter(Runnable  task, T result) {
            this.task = task;
            this.result = result;
        }
        public T call() {
            task.run();
            return result;
        }
    }

**延迟、周期性任务调度的实现**

java.util.concurrent.ScheduledThreadPoolExecutor是默认的延迟、周期性任务调度的实现。

有了整个线程池的实现，再回头来看延迟、周期性任务调度的实现应该就很简单了，因为所谓的延迟、周期性任务调度，无非添加一系列有序的任务队列，然后按照执行顺序的先后来处理整个任务队列。如果是周期性任务，那么在执行完毕的时候加入下一个时间点的任务即可。

由此可见，ScheduledThreadPoolExecutor和ThreadPoolExecutor的唯一区别在于任务是有序（按照执行时间顺序）的，并且需要到达时间点（临界点）才能执行，并不是任务队列中有任务就需要执行的。也就是说唯一不同的就是任务队列BlockingQueue<Runnable> workQueue不一样。ScheduledThreadPoolExecutor的任务队列是java.util.concurrent.ScheduledThreadPoolExecutor.DelayedWorkQueue，它是基于java.util.concurrent.DelayQueue<RunnableScheduledFuture>队列的实现。

DelayQueue是基于有序队列[PriorityQueue](http://www.blogjava.net/xylz/archive/2010/07/30/327582.html)实现的。[PriorityQueue](http://www.blogjava.net/xylz/archive/2010/07/30/327582.html) 也叫优先级队列，按照自然顺序对元素进行排序，类似于TreeMap/Collections.sort一样。

同样是有序队列，DelayQueue和[PriorityQueue](http://www.blogjava.net/xylz/archive/2010/07/30/327582.html)区别在什么地方？

由于DelayQueue在获取元素时需要检测元素是否“可用”，也就是任务是否达到“临界点”（指定时间点），因此加入元素和移除元素会有一些额外的操作。

典型的，移除元素需要检测元素是否达到“临界点”，增加元素的时候如果有一个元素比“头元素”更早达到临界点，那么就需要通知任务队列。因此这需要一个条件变量final Condition available 。

移除元素（出队列）的过程是这样的：

* 总是检测队列的头元素（顺序最小元素，也是最先达到临界点的元素）
* 检测头元素与当前时间的差，如果大于0，表示还未到底临界点，因此等待响应时间（使用条件变量available)
* 如果小于或者等于0，说明已经到底临界点或者已经过了临界点，那么就移除头元素，并且唤醒其它等待任务队列的线程。
    public E take() throws InterruptedException {
        final ReentrantLock lock = this.lock;
        lock.lockInterruptibly();
        try {
            for (;;) {
                E first = q.peek();
                if (first == null) {
                    available.await();
                } else {
                    long delay =  first.getDelay(TimeUnit.NANOSECONDS);
                    if (delay > 0) {
                        long tl = available.awaitNanos(delay);
                    } else {
                        E x = q.poll();
                        assert x != null;
                        if (q.size() != 0)
                            available.signalAll(); // wake up other takers
                        return x;
                    }
                }
            }
        } finally {
            lock.unlock();
        }
    }

同样加入元素也会有相应的条件变量操作。当前仅当队列为空或者要加入的元素比队列中的头元素还小的时候才需要唤醒“等待线程”去检测元素。因为头元素都没有唤醒那么比头元素更延迟的元素就更加不会唤醒。

    public boolean offer(E e) {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            E first = q.peek();
            q.offer(e);
            if (first == null || e.compareTo(first) < 0)
                available.signalAll();
            return true;
        } finally {
            lock.unlock();
        }
    }

有了任务队列后再来看Future在ScheduledThreadPoolExecutor中是如何操作的。

java.util.concurrent.ScheduledThreadPoolExecutor.ScheduledFutureTask<V>是继承java.util.concurrent.FutureTask<V>的，区别在于执行任务是否是周期性的。
        private void runPeriodic() {
            boolean ok = ScheduledFutureTask.super.runAndReset();
            boolean down = isShutdown();
            // Reschedule if not cancelled and not shutdown or policy allows
            if (ok && (!down ||
                       (getContinueExistingPeriodicTasksAfterShutdownPolicy() &&
                        !isStopped()))) {
                long p = period;
                if (p > 0)
                    time += p;
                else
                    time = now() - p;
                ScheduledThreadPoolExecutor.super.getQueue().add(this);
            }
            // This might have been the final executed delayed
            // task.  Wake up threads to check.
            else if (down)
                interruptIdleWorkers();
        }
        /**
         * Overrides FutureTask version so as to reset/requeue if periodic.
         */
        public void run() {
            if (isPeriodic())
                runPeriodic();
            else
                ScheduledFutureTask.super.run();
        }
    }

如果不是周期性任务调度，那么就和java.util.concurrent.FutureTask.Sync的调度方式是一样的。如果是周期性任务（isPeriodic()）那么就稍微有所不同的。

[![ScheduledThreadPoolExecutor-ScheduledFutureTask]( "ScheduledThreadPoolExecutor-ScheduledFutureTask")](http://www.blogjava.net/images/blogjava_net/xylz/Windows-Live-Writer/-Java-Concurrency-35--part-8--2_BFEA/ScheduledThreadPoolExecutor-ScheduledFutureTask_4.png)

先从功能/结构上分析下。第一种情况假设提交的任务每次执行花费10s，间隔（delay/period)为20s，对于scheduleAtFixedRate而言，每次执行开始时间20s，对于scheduleWithFixedDelay来说每次执行开始时间30s。第二种情况假设提交的任务每次执行时间花费20s，间隔（delay/period)为10s，对于scheduleAtFixedRate而言，每次执行开始时间10s，对于scheduleWithFixedDelay来说每次执行开始时间30s。（具体分析可以参考[这里](http://www.blogjava.net/xylz/archive/2011/01/10/342738.html)）

也就是说scheduleWithFixedDelay的执行开始时间为(delay+cost)，而对于scheduleAtFixedRate来说执行开始时间为max(period,cost)。

回头再来看上面源码runPeriodic()就很容易了。但特别要提醒的，如果任务的任何一个执行遇到异常，则后续执行都会被取消，这从runPeriodic()就能看出。要强调的第二点就是**同一个周期性任务不会被同时执行**。就比如说尽管上面第二种情况的scheduleAtFixedRate任务每隔10s执行到达一个时间点，但是由于每次执行时间花费为20s，因此每次执行间隔为20s，只不过执行的任务次数会多一点。但从本质上讲就是每隔20s执行一次，如果任务队列不取消的话。

为什么不会同时执行？

这是因为ScheduledFutureTask执行的时候会将任务从队列中移除来，执行完毕以后才会添加下一个同序列的任务，因此任务队列中其实最多只有同序列的任务的一份副本，所以永远不会同时执行（尽管要执行的时间在过去）。

 

ScheduledThreadPoolExecutor使用一个无界（容量无限，整数的最大值）的容器（DelayedWorkQueue队列），根据[ThreadPoolExecutor](http://www.blogjava.net/xylz/archive/2011/02/11/344091.html)的原理，只要当容器满的时候才会启动一个大于corePoolSize的线程数。因此实际上ScheduledThreadPoolExecutor是一个固定线程大小的线程池，固定大小为corePoolSize，构造函数里面的Integer.MAX_VALUE其实是不生效的（尽管[PriorityQueue](http://www.blogjava.net/xylz/archive/2010/07/30/327582.html)使用数组实现有[PriorityQueue](http://www.blogjava.net/xylz/archive/2010/07/30/327582.html)大小限制，如果你的任务数超过了2147483647就会导致OutOfMemoryError，这个参考[PriorityQueue](http://www.blogjava.net/xylz/archive/2010/07/30/327582.html)的grow方法）。

 

再回头看scheduleAtFixedRate等方法就容易多了。无非就是往任务队列中添加一个未来某一时刻的ScheduledFutureTask任务，如果是scheduleAtFixedRate那么period/delay就是正数，如果是scheduleWithFixedDelay那么period/delay就是一个负数，如果是0那么就是一次性任务。直接调用父类[ThreadPoolExecutor](http://www.blogjava.net/xylz/archive/2011/02/11/344091.html)的execute/submit等方法就相当于period/delay是0，并且initialDelay也是0。
    public ScheduledFuture<?> scheduleAtFixedRate(Runnable command,
                                                  long initialDelay,
                                                  long period,
                                                  TimeUnit unit) {
        if (command == null || unit == null)
            throw new NullPointerException();
        if (period <= 0)
            throw new IllegalArgumentException();
        if (initialDelay < 0) initialDelay = 0;
        long triggerTime = now() + unit.toNanos(initialDelay);
        RunnableScheduledFuture<?> t = decorateTask(command,
            new ScheduledFutureTask<Object>(command,
                                            null,
                                            triggerTime,
                                            unit.toNanos(period)));
        delayedExecute(t);
        return t;
    }

另外需要补充说明的一点，前面说过java.util.concurrent.FutureTask.Sync任务只能执行一次，那么在runPeriodic()里面怎么又将执行过的任务加入队列中呢？这是因为java.util.concurrent.FutureTask.Sync提供了一个innerRunAndReset()方法，此方法不仅执行任务还将任务的状态还原成0（初始状态）了，所以此任务就可以重复执行。这就是为什么runPeriodic()里面调用runAndRest()的缘故。

        boolean innerRunAndReset() {
            if (!compareAndSetState(0, RUNNING))
                return false;
            try {
                runner = Thread.currentThread();
                if (getState() == RUNNING)
                    callable.call(); // don't set result
                runner = null;
                return compareAndSetState(RUNNING, 0);
            } catch (Throwable ex) {
                innerSetException(ex);
                return false;
            }
        }

 

**后话**

整个并发实践原理和实现（源码）上的东西都讲完了，后面几个小节是一些总结和扫尾的工作，包括超时机制、异常处理等一些细节问题。也就是说大部分只需要搬出一些理论和最佳实践知识出来就好了，不会有大量费脑筋的算法分析和原理、思想探讨之类的。后面的章节也会加快一些进度。

老实说从刚开始的好奇到中间的兴奋，再到现在的彻悟，收获还是很多，个人觉得这是最认真、最努力也是自我最满意的一次技术研究和探讨，同时在这个过程中将很多技术细节都串联起来了，慢慢就有了那种技术相通的感觉。原来有了理论以后再去实践、再去分析问题、解决问题和那种纯解决问题得到的经验完全不一样。整个专辑下来不仅仅是并发包这一点点知识，设计到硬件、软件、操作系统、网络、安全、性能、算法、理论等等，总的来说这也算是一次比较成功的研究切入点，这比[Guice](http://www.blogjava.net/xylz/archive/2009/12/22/306955.html)那次探讨要深入和持久的多。
来源： <[http://www.blogjava.net/xylz/archive/2011/02/13/344207.html](http://www.blogjava.net/xylz/archive/2011/02/13/344207.html)> 

[并发操作异常体系](http://www.blogjava.net/xylz/archive/2011/07/12/354206.html) 

并发包引入的工具类很多方法都会抛出一定的异常，这些异常描述了任务在线程池中执行时发生的例外情况，而通常这些例外需要应用程序进行捕捉和处理。

例如在Future接口中有如下一个API：

 
java.util.concurrent.Future.get(long, TimeUnit) throws InterruptedException, ExecutionException, TimeoutException;

 

在[前面的章节](http://www.blogjava.net/xylz/archive/2011/02/13/344207.html)中描述了Future类的具体实现原理。这里不再讨论，但是比较好奇的抛出的三个异常。

这里有一篇文章（[Java 理论与实践: 处理 InterruptedException](http://www.ibm.com/developerworks/cn/java/j-jtp05236.html)）描述了InterruptedException的来源和处理方式。简单的说就是线程在执行的过程中被自己或者别人中断了。这时候为了响应中断就需要处理当前的异常。

对于java.lang.Thread而言，InterruptedException也是一个很诡异的问题。

中断一个线程Thread.**interrupt()**时会触发下面一种情况：
如果线程在调用 Object 类的 wait()、wait(long) 或 wait(long, int) 方法，或者该类的 join()、join(long)、join(long, int)、sleep(long) 或 sleep(long, int) 方法过程中受阻，则其中断状态将被清除，它还将收到一个 InterruptedException。

检测一个线程的中断状态描述是这样的Thread.**interrupted()：**

测试当前线程是否已经中断。线程的*中断状态* 由该方法清除。换句话说，如果连续两次调用该方法，则第二次调用将返回 false（在第一次调用已清除了其中断状态之后，且第二次调用检验完中断状态前，当前线程再次中断的情况除外）。 

也就是说如果检测到一个线程已经被中断了，那么线程的使用方（挂起、等待或者正在执行）都将应该得到一个中断异常，同时将会清除异常中断状态。

 
V innerGet(long nanosTimeout) throws InterruptedException, ExecutionException, TimeoutException {
    if (!tryAcquireSharedNanos(0, nanosTimeout))
        throw new TimeoutException();
    if (getState() == CANCELLED)
        throw new CancellationException();
    if (exception != null)
        throw new ExecutionException(exception);
    return result;
}

 

上面获取任务结果的方法实现中，将在获取锁的过程中得到一个中断异常。代码java.util.concurrent.locks.AbstractQueuedSynchronizer.tryAcquireSharedNanos(int, long)描述了这种情况：
    public final boolean tryAcquireSharedNanos(int arg, long nanosTimeout) throws InterruptedException {
    if (Thread.interrupted())
        throw new InterruptedException();
    return tryAcquireShared(arg) >= 0 ||
        doAcquireSharedNanos(arg, nanosTimeout);
    }

 

这里在获取锁的时候检测线程中断情况，如果被中断则清除中断位，同时抛出一个中断异常。为什么如此做？因为我们的线程在线程池中是被重复执行的，所以一旦线程被中断后并不会退出线程，而是设置中断位，等候任务队列自己处理线程，从而达到线程被重复利用的目的。有兴趣的可以参考代码java.util.concurrent.ThreadPoolExecutor.Worker.runTask(Runnable)。这里在关闭线程池时就会导致中断所有线程。

除了InterruptedException 异常我们还发现了一个全新的异常java.util.concurrent.TimeoutException，此异常是用来描述任务执行时间超过了期望等待时间，也许是一直没有获取到锁，也许是还没有执行完成。

在innerGet代码片段中我们看到，如果线程在指定的时间无法获取到锁，那么就会得到一个超时异常。这个很好理解，比如如果执行一个非常耗时的网络任务，我们不希望任务一直等待从而占用大量的资源，可能在一定时间后就会希望取消此操作。此时超时异常很好的描述了这种需求。

与此同时，如果取消了一个任务，那么再次从任务中获取执行结果，那么将会得到一个任务被取消的异常java.util.concurrent.CancellationException。

除了上述异常外，还将得到一个java.util.concurrent.ExecutionException异常，

这是因为我们的提交的任务java.util.concurrent.Callable在call()方法中允许抛出任何异常，另外常规的线程执行也可能抛出一个RuntimeException，所以这里简单包装了下所有异常，当作执行过程中发生的异常ExecutionException抛出。

以上就是整个异常体系，所有并发操作的异常都可以归结于上述几类。

很多情况下处理时间长度都是用**java.util.concurrent.TimeUnit**，这是一个枚举类型，用来描述时间长度。其中内置了一些长度的单位。其中包括纳秒、微秒、毫秒、秒、分、时、天。例如超时操作5秒，可以使用

Future.get(5,TimeUnit.SECONDS) 或者 Future.get(5000L,TimeUnit.MILLISECONDS)

当然一种单位的时间转换成另一种单位的时间也是非常方便的。另外还有线程的sleep/join以及对象的wait操作的便捷操作。
来源： <[http://www.blogjava.net/xylz/archive/2011/07/12/354206.html](http://www.blogjava.net/xylz/archive/2011/07/12/354206.html)> 
