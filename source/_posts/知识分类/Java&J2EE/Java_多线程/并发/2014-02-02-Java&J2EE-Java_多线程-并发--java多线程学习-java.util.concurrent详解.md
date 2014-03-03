---
layout: post
title: "java多线程学习-java.util.concurrent详解"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_多线程
 - 并发
--- 

# java多线程学习-java.util.concurrent详解

### [Latch/Barrier](http://janeky.iteye.com/blog/769965)

   Java1.5提供了一个非常高效实用的多线程包：java.util.concurrent, 提供了大量高级工具，可以帮助开发者编写高效、易维护、结构清晰的Java多线程程序。从这篇blog起，我将跟大家一起共同学习这些新的Java多线程构件 
1. CountDownLatch 
    我们先来学习一下JDK1.5 API中关于这个类的详细介绍： 
“一个同步辅助类，在完成一组正在其他线程中执行的操作之前，它允许一个或多个线程一直等待。 用给定的计数 初始化 CountDownLatch。由于调用了 countDown() 方法，所以在当前计数到达零之前，await 方法会一直受阻塞。之后，会释放所有等待的线程，await 的所有后续调用都将立即返回。这种现象只出现一次——计数无法被重置。如果需要重置计数，请考虑使用 CyclicBarrier。” 
    这就是说，CountDownLatch可以用来管理一组相关的线程执行，只需在主线程中调用CountDownLatch 的await方法（一直阻塞），让各个线程调用countDown方法。当所有的线程都只需完countDown了，await也顺利返回，不再阻塞了。在这样情况下尤其适用：将一个任务分成若干线程执行，等到所有线程执行完，再进行汇总处理。 
    下面我举一个非常简单的例子。假设我们要打印1-100，最后再输出“Ok“。1-100的打印顺序不要求统一，只需保证“Ok“是在最后出现即可。 
    解决方案：我们定义一个CountDownLatch，然后开10个线程分别打印（n-1）*10+1至（n-1）*10+10。主线程中调用await方法等待所有线程的执行完毕，每个线程执行完毕后都调用countDown方法。最后再await返回后打印“Ok”。 
具体代码如下（本代码参考了JDK示例代码）： 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. import java.util.concurrent.CountDownLatch;  
1. /** 
1.  * 示例：CountDownLatch的使用举例 
1.  * Mail: ken@iamcoding.com 
1.  * @author janeky 
1.  */  
1. public class TestCountDownLatch {  
1.     private static final int N = 10;  
1.   
1.     public static void main(String[] args) throws InterruptedException {  
1.         CountDownLatch doneSignal = new CountDownLatch(N);  
1.         CountDownLatch startSignal = new CountDownLatch(1);//开始执行信号  
1.   
1.         for (int i = 1; i <= N; i++) {  
1.             new Thread(new Worker(i, doneSignal, startSignal)).start();//线程启动了  
1.         }  
1.         System.out.println("begin------------");  
1.         startSignal.countDown();//开始执行啦  
1.         doneSignal.await();//等待所有的线程执行完毕  
1.         System.out.println("Ok");  
1.   
1.     }  
1.   
1.     static class Worker implements Runnable {  
1.         private final CountDownLatch doneSignal;  
1.         private final CountDownLatch startSignal;  
1.         private int beginIndex;  
1.   
1.         Worker(int beginIndex, CountDownLatch doneSignal,  
1.                 CountDownLatch startSignal) {  
1.             this.startSignal = startSignal;  
1.             this.beginIndex = beginIndex;  
1.             this.doneSignal = doneSignal;  
1.         }  
1.   
1.         public void run() {  
1.             try {  
1.                 startSignal.await(); //等待开始执行信号的发布  
1.                 beginIndex = (beginIndex - 1) * 10 + 1;  
1.                 for (int i = beginIndex; i <= beginIndex + 10; i++) {  
1.                     System.out.println(i);  
1.                 }  
1.             } catch (InterruptedException e) {  
1.                 e.printStackTrace();  
1.             } finally {  
1.                 doneSignal.countDown();  
1.             }  
1.         }  
1.     }  
1. }  
    总结：CounDownLatch对于管理一组相关线程非常有用。上述示例代码中就形象地描述了两种使用情况。第一种是计算器为1，代表了两种状态，开关。第二种是计数器为N，代表等待N个操作完成。今后我们在编写多线程程序时，可以使用这个构件来管理一组独立线程的执行。 
2. CyclicBarrier 
    我们先来学习一下JDK1.5 API中关于这个类的详细介绍： 
    “一个同步辅助类，它允许一组线程互相等待，直到到达某个公共屏障点 (common barrier point)。在涉及一组固定大小的线程的程序中，这些线程必须不时地互相等待，此时 CyclicBarrier 很有用。因为该 barrier 在释放等待线程后可以重用，所以称它为循环 的 barrier。 
    CyclicBarrier 支持一个可选的 Runnable 命令，在一组线程中的最后一个线程到达之后（但在释放所有线程之前），该命令只在每个屏障点运行一次。若在继续所有参与线程之前更新共享状态，此屏障操作 很有用。 
    我们在学习CountDownLatch的时候就提到了CyclicBarrier。两者究竟有什么联系呢？引用[JCIP]中的描述“The key difference is that with a barrier, all the threads must come together at a barrier point at the same time in order to proceed. Latches are for waiting for events; barriers are for waiting for other threads。CyclicBarrier等待所有的线程一起完成后再执行某个动作。这个功能CountDownLatch也同样可以实现。但是CountDownLatch更多时候是在等待某个事件的发生。在CyclicBarrier中，所有的线程调用await方法，等待其他线程都执行完。 
    举一个很简单的例子，今天晚上我们哥们4个去Happy。就互相通知了一下：晚上八点准时到xx酒吧门前集合，不见不散！。有个哥们住的近，早早就到了。有的事务繁忙，刚好踩点到了。无论怎样，先来的都不能独自行动，只能等待所有人 
代码如下（参考了网上给的一些教程） 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. import java.util.Random;  
1. import java.util.concurrent.BrokenBarrierException;  
1. import java.util.concurrent.CyclicBarrier;  
1. import java.util.concurrent.ExecutorService;  
1. import java.util.concurrent.Executors;  
1.   
1. public class TestCyclicBarrier {  
1.   
1.     public static void main(String[] args) {  
1.       
1.         ExecutorService exec = Executors.newCachedThreadPool();       
1.         final Random random=new Random();  
1.           
1.         final CyclicBarrier barrier=new CyclicBarrier(4,new Runnable(){  
1.             @Override  
1.             public void run() {  
1.                 System.out.println("大家都到齐了，开始happy去");  
1.             }});  
1.           
1.         for(int i=0;i<4;i++){  
1.             exec.execute(new Runnable(){  
1.                 @Override  
1.                 public void run() {  
1.                     try {  
1.                         Thread.sleep(random.nextInt(1000));  
1.                     } catch (InterruptedException e) {  
1.                         e.printStackTrace();  
1.                     }  
1.                     System.out.println(Thread.currentThread().getName()+"到了，其他哥们呢");  
1.                     try {  
1.                         barrier.await();//等待其他哥们  
1.                     } catch (InterruptedException e) {  
1.                         e.printStackTrace();  
1.                     } catch (BrokenBarrierException e) {  
1.                         e.printStackTrace();  
1.                     }  
1.                 }});  
1.         }  
1.         exec.shutdown();  
1.     }  
1.   
1. }  
    关于await方法要特别注意一下，它有可能在阻塞的过程中由于某些原因被中断 
    总结：CyclicBarrier就是一个栅栏，等待所有线程到达后再执行相关的操作。barrier 在释放等待线程后可以重用。 

3. Semaphore 
    我们先来学习一下JDK1.5 API中关于这个类的详细介绍： 
“一个计数信号量。从概念上讲，信号量维护了一个许可集。如有必要，在许可可用前会阻塞每一个 acquire()，然后再获取该许可。每个 release() 添加一个许可，从而可能释放一个正在阻塞的获取者。但是，不使用实际的许可对象，Semaphore 只对可用许可的号码进行计数，并采取相应的行动。” 
    我们一般用它来控制某个对象的线程访问对象 
    例如，对于某个容器，我们规定，最多只能容纳n个线程同时操作 
使用信号量来模拟实现 
具体代码如下（参考 [JCIP]） 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. import java.util.Collections;  
1. import java.util.HashSet;  
1. import java.util.Set;  
1. import java.util.concurrent.ExecutorService;  
1. import java.util.concurrent.Executors;  
1. import java.util.concurrent.Semaphore;  
1.   
1. public class TestSemaphore {  
1.   
1.     public static void main(String[] args) {  
1.         ExecutorService exec = Executors.newCachedThreadPool();  
1.         TestSemaphore t = new TestSemaphore();  
1.         final BoundedHashSet<String> set = t.getSet();  
1.   
1.         for (int i = 0; i < 3; i++) {//三个线程同时操作add  
1.             exec.execute(new Runnable() {  
1.                 public void run() {  
1.                     try {  
1.                         set.add(Thread.currentThread().getName());  
1.                     } catch (InterruptedException e) {  
1.                         e.printStackTrace();  
1.                     }  
1.                 }  
1.             });  
1.         }  
1.   
1.         for (int j = 0; j < 3; j++) {//三个线程同时操作remove  
1.             exec.execute(new Runnable() {  
1.                 public void run() {  
1.                     set.remove(Thread.currentThread().getName());  
1.                 }  
1.             });  
1.         }  
1.         exec.shutdown();  
1.     }  
1.   
1.     public BoundedHashSet<String> getSet() {  
1.         return new BoundedHashSet<String>(2);//定义一个边界约束为2的线程  
1.     }  
1.   
1.     class BoundedHashSet<T> {  
1.         private final Set<T> set;  
1.         private final Semaphore semaphore;  
1.   
1.         public BoundedHashSet(int bound) {  
1.             this.set = Collections.synchronizedSet(new HashSet<T>());  
1.             this.semaphore = new Semaphore(bound, true);  
1.         }  
1.   
1.         public void add(T o) throws InterruptedException {  
1.             semaphore.acquire();//信号量控制可访问的线程数目  
1.             set.add(o);  
1.             System.out.printf("add:%s%n",o);  
1.         }  
1.   
1.         public void remove(T o) {  
1.             if (set.remove(o))  
1.                 semaphore.release();//释放掉信号量  
1.             System.out.printf("remove:%s%n",o);  
1.         }  
1.     }  
1. }  
    总结：Semaphore通常用于对象池的控制 
4．FutureTask 
    我们先来学习一下JDK1.5 API中关于这个类的详细介绍： 
    “取消的异步计算。利用开始和取消计算的方法、查询计算是否完成的方法和获取计算结果的方法，此类提供了对 Future 的基本实现。仅在计算完成时才能获取结果；如果计算尚未完成，则阻塞 get 方法。一旦计算完成，就不能再重新开始或取消计算。 
可使用 FutureTask 包装 Callable 或 Runnable 对象。因为 FutureTask 实现了 Runnable，所以可将 FutureTask 提交给 Executor 执行。 
除了作为一个独立的类外，此类还提供了 protected 功能，这在创建自定义任务类时可能很有用。 “ 
    应用举例：我们的算法中有一个很耗时的操作，在编程的是，我们希望将它独立成一个模块，调用的时候当做它是立刻返回的，并且可以随时取消的 
具体代码如下（参考 [JCIP]） 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. import java.util.concurrent.Callable;  
1. import java.util.concurrent.ExecutionException;  
1. import java.util.concurrent.ExecutorService;  
1. import java.util.concurrent.Executors;  
1. import java.util.concurrent.FutureTask;  
1.   
1. public class TestFutureTask {  
1.   
1.     public static void main(String[] args) {  
1.         ExecutorService exec=Executors.newCachedThreadPool();  
1.           
1.         FutureTask<String> task=new FutureTask<String>(new Callable<String>(){//FutrueTask的构造参数是一个Callable接口  
1.             @Override  
1.             public String call() throws Exception {  
1.                 return Thread.currentThread().getName();//这里可以是一个异步操作  
1.             }});  
1.               
1.             try {  
1.                 exec.execute(task);//FutureTask实际上也是一个线程  
1.                 String result=task.get();//取得异步计算的结果，如果没有返回，就会一直阻塞等待  
1.                 System.out.printf("get:%s%n",result);  
1.             } catch (InterruptedException e) {  
1.                 e.printStackTrace();  
1.             } catch (ExecutionException e) {  
1.                 e.printStackTrace();  
1.             }  
1.     }  
1.   
1. }  
    总结：FutureTask其实就是新建了一个线程单独执行，使得线程有一个返回值，方便程序的编写 
5. Exchanger 
    我们先来学习一下JDK1.5 API中关于这个类的详细介绍： 
    “可以在pair中对元素进行配对和交换的线程的同步点。每个线程将条目上的某个方法呈现给 exchange 方法，与伙伴线程进行匹配，并且在返回时接收其伙伴的对象。Exchanger 可能被视为 SynchronousQueue 的双向形式。Exchanger 可能在应用程序（比如遗传算法和管道设计）中很有用。 “ 
    应用举例：有两个缓存区，两个线程分别向两个缓存区fill和take，当且仅当一个满了，两个缓存区交换 
    代码如下（参考了网上给的示例   http://hi.baidu.com/webidea/blog/item/2995e731e53ad5a55fdf0e7d.html） 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. import java.util.ArrayList;  
1. import java.util.concurrent.Exchanger;  
1.   
1. public class TestExchanger {  
1.   
1.     public static void main(String[] args) {  
1.         final Exchanger<ArrayList<Integer>> exchanger = new Exchanger<ArrayList<Integer>>();  
1.         final ArrayList<Integer> buff1 = new ArrayList<Integer>(10);  
1.         final ArrayList<Integer> buff2 = new ArrayList<Integer>(10);  
1.   
1.         new Thread(new Runnable() {  
1.             @Override  
1.             public void run() {  
1.                 ArrayList<Integer> buff = buff1;  
1.                 try {  
1.                     while (true) {  
1.                         if (buff.size() >= 10) {  
1.                             buff = exchanger.exchange(buff);//开始跟另外一个线程交互数据  
1.                             System.out.println("exchange buff1");  
1.                             buff.clear();  
1.                         }  
1.                         buff.add((int)(Math.random()*100));  
1.                         Thread.sleep((long)(Math.random()*1000));  
1.                     }  
1.                 } catch (InterruptedException e) {  
1.                     e.printStackTrace();  
1.                 }  
1.             }  
1.         }).start();  
1.           
1.         new Thread(new Runnable(){  
1.             @Override  
1.             public void run() {  
1.                 ArrayList<Integer> buff=buff2;  
1.                 while(true){  
1.                     try {  
1.                         for(Integer i:buff){  
1.                             System.out.println(i);  
1.                         }  
1.                         Thread.sleep(1000);  
1.                         buff=exchanger.exchange(buff);//开始跟另外一个线程交换数据  
1.                         System.out.println("exchange buff2");  
1.                     } catch (InterruptedException e) {  
1.                         e.printStackTrace();  
1.                     }  
1.                 }  
1.             }}).start();  
1.     }  
1. }  
    总结：Exchanger在特定的使用场景比较有用（两个伙伴线程之间的数据交互） 

6. ScheduledThreadPoolExecutor 
    我们先来学习一下JDK1.5 API中关于这个类的详细介绍： 
    "可另行安排在给定的延迟后运行命令，或者定期执行命令。需要多个辅助线程时，或者要求 ThreadPoolExecutor 具有额外的灵活性或功能时，此类要优于 Timer。 
    一旦启用已延迟的任务就执行它，但是有关何时启用，启用后何时执行则没有任何实时保证。按照提交的先进先出 (FIFO) 顺序来启用那些被安排在同一执行时间的任务。 
    虽然此类继承自 ThreadPoolExecutor，但是几个继承的调整方法对此类并无作用。特别是，因为它作为一个使用 corePoolSize 线程和一个无界队列的固定大小的池，所以调整 maximumPoolSize 没有什么效果。" 
    在JDK1.5之前，我们关于定时/周期操作都是通过Timer来实现的。但是Timer有以下几种危险[JCIP] 
a. Timer是基于绝对时间的。容易受系统时钟的影响。 
b. Timer只新建了一个线程来执行所有的TimeTask。所有TimeTask可能会相关影响 
c. Timer不会捕获TimerTask的异常，只是简单地停止。这样势必会影响其他TimeTask的执行。 
    如果你是使用JDK1.5以上版本，建议用ScheduledThreadPoolExecutor代替Timer。它基本上解决了上述问题。它采用相对时间，用线程池来执行TimerTask，会出来TimerTask异常。 
    下面通过一个简单的实例来阐述ScheduledThreadPoolExecutor的使用。 
   
    我们定期让定时器抛异常 
    我们定期从控制台打印系统时间 
代码如下（参考了网上的一些代码，在此表示感谢） 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. import java.util.concurrent.ScheduledThreadPoolExecutor;  
1. import java.util.concurrent.TimeUnit;  
1.   
1.   
1. public class TestScheduledThreadPoolExecutor {  
1.       
1.     public static void main(String[] args) {  
1.         ScheduledThreadPoolExecutor exec=new ScheduledThreadPoolExecutor(1);  
1.           
1.         exec.scheduleAtFixedRate(new Runnable(){//每隔一段时间就触发异常  
1.             @Override  
1.             public void run() {  
1.                 throw new RuntimeException();  
1.             }}, 1000, 5000, TimeUnit.MILLISECONDS);  
1.           
1.         exec.scheduleAtFixedRate(new Runnable(){//每隔一段时间打印系统时间，证明两者是互不影响的  
1.             @Override  
1.             public void run() {  
1.                 System.out.println(System.nanoTime());  
1.             }}, 1000, 2000, TimeUnit.MILLISECONDS);  
1.     }  
1.   
1. }  
总结：是时候把你的定时器换成 ScheduledThreadPoolExecutor了 

7.BlockingQueue 
    “支持两个附加操作的 Queue，这两个操作是：获取元素时等待队列变为非空，以及存储元素时等待空间变得可用。“ 
    这里我们主要讨论BlockingQueue的最典型实现：LinkedBlockingQueue 和ArrayBlockingQueue。两者的不同是底层的数据结构不够，一个是链表，另外一个是数组。 
    
    后面将要单独解释其他类型的BlockingQueue和SynchronousQueue 
    BlockingQueue的经典用途是 生产者-消费者模式 
    代码如下： 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. import java.util.Random;  
1. import java.util.concurrent.BlockingQueue;  
1. import java.util.concurrent.LinkedBlockingQueue;  
1.   
1. public class TestBlockingQueue {  
1.   
1.     public static void main(String[] args) {  
1.         final BlockingQueue<Integer> queue=new LinkedBlockingQueue<Integer>(3);  
1.         final Random random=new Random();  
1.           
1.         class Producer implements Runnable{  
1.             @Override  
1.             public void run() {  
1.                 while(true){  
1.                     try {  
1.                         int i=random.nextInt(100);  
1.                         queue.put(i);//当队列达到容量时候，会自动阻塞的  
1.                         if(queue.size()==3)  
1.                         {  
1.                             System.out.println("full");  
1.                         }  
1.                     } catch (InterruptedException e) {  
1.                         e.printStackTrace();  
1.                     }  
1.                 }  
1.             }  
1.         }  
1.           
1.         class Consumer implements Runnable{  
1.             @Override  
1.             public void run() {  
1.                 while(true){  
1.                     try {  
1.                         queue.take();//当队列为空时，也会自动阻塞  
1.                         Thread.sleep(1000);  
1.                     } catch (InterruptedException e) {  
1.                         e.printStackTrace();  
1.                     }  
1.                 }  
1.             }  
1.         }  
1.           
1.         new Thread(new Producer()).start();  
1.         new Thread(new Consumer()).start();  
1.     }  
1.   
1. }  
    总结：BlockingQueue使用时候特别注意take 和 put 
8. DelayQueue 
我们先来学习一下JDK1.5 API中关于这个类的详细介绍： 
    “它是包含Delayed 元素的一个无界阻塞队列，只有在延迟期满时才能从中提取元素。该队列的头部 是延迟期满后保存时间最长的 Delayed 元素。如果延迟都还没有期满，则队列没有头部，并且 poll 将返回 null。当一个元素的 getDelay(TimeUnit.NANOSECONDS) 方法返回一个小于等于 0 的值时，将发生到期。即使无法使用 take 或 poll 移除未到期的元素，也不会将这些元素作为正常元素对待。例如，size 方法同时返回到期和未到期元素的计数。此队列不允许使用 null 元素。” 
    在现实生活中，很多DelayQueue的例子。就拿上海的SB会来说明，很多国家地区的开馆时间不同。你很早就来到园区，然后急急忙忙地跑到一些心仪的馆区，发现有些还没开，你吃了闭门羹。 
    仔细研究DelayQueue，你会发现它其实就是一个PriorityQueue的封装（按照delay时间排序），里面的元素都实现了Delayed接口，相关操作需要判断延时时间是否到了。 
    在实际应用中，有人拿它来管理跟实际相关的缓存、session等 
   下面我就通过 “上海SB会的例子来阐述DelayQueue的用法” 
代码如下： 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. import java.util.Random;  
1. import java.util.concurrent.DelayQueue;  
1. import java.util.concurrent.Delayed;  
1. import java.util.concurrent.TimeUnit;  
1.   
1. public class TestDelayQueue {  
1.   
1.     private class Stadium implements Delayed  
1.     {  
1.         long trigger;  
1.           
1.         public Stadium(long i){  
1.             trigger=System.currentTimeMillis()+i;  
1.         }  
1.           
1.         @Override  
1.         public long getDelay(TimeUnit arg0) {  
1.             long n=trigger-System.currentTimeMillis();  
1.             return n;  
1.         }  
1.   
1.         @Override  
1.         public int compareTo(Delayed arg0) {  
1.             return (int)(this.getDelay(TimeUnit.MILLISECONDS)-arg0.getDelay(TimeUnit.MILLISECONDS));  
1.         }  
1.           
1.         public long getTriggerTime(){  
1.             return trigger;  
1.         }  
1.           
1.     }  
1.     public static void main(String[] args)throws Exception {  
1.         Random random=new Random();  
1.         DelayQueue<Stadium> queue=new DelayQueue<Stadium>();  
1.         TestDelayQueue t=new TestDelayQueue();  
1.           
1.         for(int i=0;i<5;i++){  
1.             queue.add(t.new Stadium(random.nextInt(30000)));  
1.         }  
1.         Thread.sleep(2000);  
1.           
1.         while(true){  
1.             Stadium s=queue.take();//延时时间未到就一直等待  
1.             if(s!=null){  
1.                 System.out.println(System.currentTimeMillis()-s.getTriggerTime());//基本上是等于0  
1.             }  
1.             if(queue.size()==0)  
1.                 break;  
1.         }  
1.     }  
1. }  
    总结：适用于需要延时操作的队列管理 
9. SynchronousQueue 
    我们先来学习一下JDK1.5 API中关于这个类的详细介绍： 
    “一种阻塞队列，其中每个插入操作必须等待另一个线程的对应移除操作 ，反之亦然。同步队列没有任何内部容量，甚至连一个队列的容量都没有。不能在同步队列上进行 peek，因为仅在试图要移除元素时，该元素才存在；除非另一个线程试图移除某个元素，否则也不能（使用任何方法）插入元素；也不能迭代队列，因为其中没有元素可用于迭代。队列的头 是尝试添加到队列中的首个已排队插入线程的元素；如果没有这样的已排队线程，则没有可用于移除的元素并且 poll() 将会返回 null。对于其他 Collection 方法（例如 contains），SynchronousQueue 作为一个空 collection。此队列不允许 null 元素。 
    同步队列类似于 CSP 和 Ada 中使用的 rendezvous 信道。它非常适合于传递性设计，在这种设计中，在一个线程中运行的对象要将某些信息、事件或任务传递给在另一个线程中运行的对象，它就必须与该对象同步。 “ 
    看起来很有意思吧。队列竟然是没有内部容量的。这个队列其实是BlockingQueue的一种实现。每个插入操作必须等待另一个线程的对应移除操作，反之亦然。它给我们提供了在线程之间交换单一元素的极轻量级方法 
   应用举例：我们要在多个线程中传递一个变量。 
   代码如下（其实就是生产者消费者模式） 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. import java.util.Arrays;  
1. import java.util.List;  
1. import java.util.concurrent.BlockingQueue;  
1. import java.util.concurrent.SynchronousQueue;  
1.   
1. public class TestSynchronousQueue {  
1.   
1.     class Producer implements Runnable {  
1.         private BlockingQueue<String> queue;  
1.         List<String> objects = Arrays.asList("one", "two", "three");  
1.   
1.         public Producer(BlockingQueue<String> q) {  
1.             this.queue = q;  
1.         }  
1.   
1.         @Override  
1.         public void run() {  
1.             try {  
1.                 for (String s : objects) {  
1.                     queue.put(s);// 产生数据放入队列中  
1.                     System.out.printf("put:%s%n",s);  
1.                 }  
1.                 queue.put("Done");// 已完成的标志  
1.             } catch (InterruptedException e) {  
1.                 e.printStackTrace();  
1.             }  
1.         }  
1.     }  
1.   
1.     class Consumer implements Runnable {  
1.         private BlockingQueue<String> queue;  
1.   
1.         public Consumer(BlockingQueue<String> q) {  
1.             this.queue = q;  
1.         }  
1.   
1.         @Override  
1.         public void run() {  
1.             String obj = null;  
1.             try {  
1.                 while (!((obj = queue.take()).equals("Done"))) {  
1.                     System.out.println(obj);//从队列中读取对象  
1.                     Thread.sleep(3000);     //故意sleep，证明Producer是put不进去的  
1.                 }  
1.             } catch (InterruptedException e) {  
1.                 e.printStackTrace();  
1.             }  
1.         }  
1.     }  
1.   
1.     public static void main(String[] args) {  
1.         BlockingQueue<String> q=new SynchronousQueue<String>();  
1.         TestSynchronousQueue t=new TestSynchronousQueue();  
1.         new Thread(t.new Producer(q)).start();  
1.         new Thread(t.new Consumer(q)).start();  
1.     }  
1.   
1. }  
   总结：SynchronousQueue主要用于单个元素在多线程之间的传递 

 
 
 
 
 
