---
layout: post
title: "JAVA线程池管理及分布式HADOOP调度框架搭建"
categories: hadoop
tags: 
 - hadoop
--- 

# JAVA线程池管理及分布式HADOOP调度框架搭建

平时的开发中线程是个少不了的东西，比如tomcat里的servlet就是线程，没有线程我们如何提供多用户访问呢？不过很多刚开始接触线程的开发攻城师却在这个上面吃了不少苦头。怎么做一套简便的线程开发模式框架让大家从单线程开发快速转入多线程开发，这确实是个比较难搞的工程。

那具体什么是线程呢？首先看看进程是什么，进程就是系统中执行的一个程序，这个程序可以使用内存、处理器、文件系统等相关资源。例如 QQ软件、eclipse、tomcat等就是一个exe程序，运行启动起来就是一个进程。为什么需要多线程？如果每个进程都是单独处理一件事情不能多个任务同时处理，比如我们打开qq只能和一个人聊天，我们用eclipse开发代码的时候不能编译代码，我们请求tomcat服务时只能服务一个用户请求，那我想我们还在原始社会。多线程的目的就是让一个进程能够同时处理多件事情或者请求。比如现在我们使用的QQ软件可以同时和多个人聊天，我们用eclipse开发代码时还可以编译代码，tomcat可以同时服务多个用户请求。

线程这么多好处，怎么把单进程程序变成多线程程序呢？不同的语言有不同的实现，这里说下java语言的实现多线程的两种方式：扩展java.lang.Thread类、实现java.lang.Runnable接口。
先看个例子，假设有100个数据需要分发并且计算。看下单线程的处理速度：
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
24
25
26 package thread;
import java.util.Vector;
public class OneMain {
       public static void main([String](http://www.google.com/search?hl=en&q=allinurl%3Astring+java.sun.com&btnI=I%27m%20Feeling%20Lucky)[] args) throws [InterruptedException](http://www.google.com/search?hl=en&q=allinurl%3Ainterruptedexception+java.sun.com&btnI=I%27m%20Feeling%20Lucky) {
            Vector<Integer> list = new Vector<Integer>(100);
             for (int i = 0; i < 100; i++) {
                  list.add(i);
            }
             long start = [System](http://www.google.com/search?hl=en&q=allinurl%3Asystem+java.sun.com&btnI=I%27m%20Feeling%20Lucky).currentTimeMillis();
             while (list.size() > 0) {
                   int val = list.remove(0);
                  [Thread](http://www.google.com/search?hl=en&q=allinurl%3Athread+java.sun.com&btnI=I%27m%20Feeling%20Lucky). sleep(100);//模拟处理
                  [System](http://www.google.com/search?hl=en&q=allinurl%3Asystem+java.sun.com&btnI=I%27m%20Feeling%20Lucky). out.println(val);
            }
             long end = [System](http://www.google.com/search?hl=en&q=allinurl%3Asystem+java.sun.com&btnI=I%27m%20Feeling%20Lucky).currentTimeMillis();
            [System](http://www.google.com/search?hl=en&q=allinurl%3Asystem+java.sun.com&btnI=I%27m%20Feeling%20Lucky). out.println("消耗 " + (end - start) + " ms");
      }
       // 消耗 10063 ms
}

再看一下多线程的处理速度，采用了10个线程分别处理：

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
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48 package thread;
import java.util.Vector;
import java.util.concurrent.CountDownLatch;
public class MultiThread extends [Thread](http://www.google.com/search?hl=en&q=allinurl%3Athread+java.sun.com&btnI=I%27m%20Feeling%20Lucky) {
     static Vector<Integer> list = new Vector<Integer>(100);
     static CountDownLatch count = new CountDownLatch(10);
     public void run() {
          while (list.size() > 0) {
               try {
                    int val = list.remove(0);
                    [System](http://www.google.com/search?hl=en&q=allinurl%3Asystem+java.sun.com&btnI=I%27m%20Feeling%20Lucky).out.println(val);
                    [Thread](http://www.google.com/search?hl=en&q=allinurl%3Athread+java.sun.com&btnI=I%27m%20Feeling%20Lucky).sleep(100);//模拟处理
               } catch ([Exception](http://www.google.com/search?hl=en&q=allinurl%3Aexception+java.sun.com&btnI=I%27m%20Feeling%20Lucky) e) {
                    // 可能数组越界，这个地方只是为了说明问题，忽略错误
               }
          }
         
          count.countDown(); // 删除成功减一
     }
     public static void main([String](http://www.google.com/search?hl=en&q=allinurl%3Astring+java.sun.com&btnI=I%27m%20Feeling%20Lucky)[] args) throws [InterruptedException](http://www.google.com/search?hl=en&q=allinurl%3Ainterruptedexception+java.sun.com&btnI=I%27m%20Feeling%20Lucky) {
         
          for (int i = 0; i < 100; i++) {
               list.add(i);
          }
         
          long start = [System](http://www.google.com/search?hl=en&q=allinurl%3Asystem+java.sun.com&btnI=I%27m%20Feeling%20Lucky).currentTimeMillis();
          for (int i = 0; i < 10; i++) {
               new MultiThread().start();
          }
         
          count.await();
          long end = [System](http://www.google.com/search?hl=en&q=allinurl%3Asystem+java.sun.com&btnI=I%27m%20Feeling%20Lucky).currentTimeMillis();
          [System](http://www.google.com/search?hl=en&q=allinurl%3Asystem+java.sun.com&btnI=I%27m%20Feeling%20Lucky).out.println("消耗 " + (end - start) + " ms");
     }
     // 消耗 1001 ms
}

大家看到了线程的好处了吧！单线程需要10S，10个线程只需要1S。充分利用了系统资源实现并行计算。也许这里会产生一个误解，是不是增加的线程个数越多效率越高。线程越多处理性能越高这个是错误的，范式都要合适，过了就不好了。需要普及一下计算机硬件的一些知识。我们的cpu是个运算器，线程执行就需要这个运算器来运行。不过这个资源只有一个，大家就会争抢。一般通过以下几种算法实现争抢cpu的调度：

1、队列方式，先来先服务。不管是什么任务来了都要按照队列排队先来后到。
2、时间片轮转，这也是最古老的cpu调度算法。设定一个时间片，每个任务使用cpu的时间不能超过这个时间。如果超过了这个时间就把任务暂停保存状态，放到队列尾部继续等待执行。
3、优先级方式：给任务设定优先级，有优先级的先执行，没有优先级的就等待执行。

这三种算法都有优缺点，实际操作系统是结合多种算法，保证优先级的能够先处理，但是也不能一直处理优先级的任务。硬件方面为了提高效率也有多核cpu、多线程cpu等解决方案。目前看得出来线程增多了会带来cpu调度的负载增加，cpu需要调度大量的线程，包括创建线程、销毁线程、线程是否需要换出cpu、是否需要分配到cpu。这些都是需要消耗系统资源的，由此，我们需要一个机制来统一管理这一堆线程资源。线程池的理念提出解决了频繁创建、销毁线程的代价。线程池指预先创建好一定大小的线程等待随时服务用户的任务处理，不必等到用户需要的时候再去创建。特别是在java开发中，尽量减少垃圾回收机制的消耗就要减少对象的频繁创建和销毁。

之前我们都是自己实现的线程池，不过随之jdk1.5的推出，jdk自带了 java.util.concurrent并发开发框架，解决了我们大部分线程池框架的重复工作。可以使用Executors来建立线程池，列出以下大概的，后面再介绍。
newCachedThreadPool 建立具有缓存功能线程池
newFixedThreadPool 建立固定数量的线程
newScheduledThreadPool 建立具有时间调度的线程

有了线程池后有以下几个问题需要考虑：
1、线程怎么管理，比如新建任务线程。
2、线程如何停止、启动。
3、线程除了scheduled模式的间隔时间定时外能否实现精确时间启动。比如晚上1点启动。
4、线程如何监控，如果线程执行过程中死掉了，异常终止我们怎么知道。

考虑到这几点，我们需要把线程集中管理起来，用java.util.concurrent是做不到的。需要做以下几点：
1、将线程和业务分离，业务的配置单独做成一个表。
2、构建基于concurrent的线程调度框架，包括可以管理线程的状态、停止线程的接口、线程存活心跳机制、线程异常日志记录模块。
3、构建灵活的timer组件，添加quartz定时组件实现精准定时系统。
4、和业务配置信息结合构建线程池任务调度系统。可以通过配置管理、添加线程任务、监控、定时、管理等操作。
组件图为：
[![分布式调度框架-lanceyan.com]()](http://www.lanceyan.com/wp-content/uploads/2013/05/task1.png)

构建好线程调度框架是不是就可以应对大量计算的需求了呢?答案是否定的。因为一个机器的资源是有限的，上面也提到了cpu是时间周期的，任务一多了也会排队，就算增加cpu，一个机器能承载的cpu也是有限的。所以需要把整个线程池框架做成分布式的任务调度框架才能应对横向扩展，比如一个机器上的资源呢达到瓶颈了，马上增加一台机器部署调度框架和业务就可以增加计算能力了。好了，如何搭建？如下图：
[![分布式调度框架-lanceyan.com]()](http://www.lanceyan.com/wp-content/uploads/2013/05/task2.png)

基于jeeframework我们封装spring、ibatis、数据库等操作，并且可以调用业务方法完成业务处理。主要组件为：
1、任务集中存储到数据库服务器
2、控制中心负责管理集群中的节点状态，任务分发
3、线程池调度集群负责控制中心分发的任务执行
4、web服务器通过可视化操作任务的分派、管理、监控。

一般这个架构可以应对常用的分布式处理需求了，不过有个缺陷就是随着开发人员的增多和业务模型的增多，单线程的编程模型也会变得复杂。比如需要对1000w数据进行分词，如果这个放到一个线程里来执行，不算计算时间消耗光是查询数据库就需要耗费不少时间。有人说，那我把1000w数据打散放到不同机器去运算，然后再合并不就行了吗？因为这是个特例的模式，专为了这个需求去开发相应的程序没有问题，但是以后又有其他的海量需求如何办？比如把倒退3年的所有用户发的帖子中发帖子最多的粉丝转发的最高的用户作息时间取出来。又得编一套程序实现，太麻烦！分布式云计算架构要解决的就是这些问题，减少开发复杂度并且要高性能，大家会不会想到一个最近很热的一个框架，hadoop，没错就是这个玩意。hadoop解决的就是这个问题，把大的计算任务分解、计算、合并，这不就是我们要的东西吗？不过玩过这个的人都知道他是一个单独的进程。不是！他是一堆进程，怎么和我们的调度框架结合起来？看图说话：
[![task31]()](http://www.lanceyan.com/wp-content/uploads/2013/05/task31.png)

基本前面的分布式调度框架组件不变，增加如下组件和功能：
1、改造分布式调度框架，可以把本身线程任务变成mapreduce任务并提交到hadoop集群。
2、hadoop集群能够调用业务接口的spring、ibatis处理业务逻辑访问数据库。
3、hadoop需要的数据能够通过hive查询。
4、hadoop可以访问hdfs/hbase读写操作。
5、业务数据要及时加入hive仓库。
6、hive处理离线型数据、hbase处理经常更新的数据、hdfs是hive和hbase的底层结构也可以存放常规文件。

这样，整个改造基本完成。不过需要注意的是架构设计一定要减少开发程序的复杂度。这里虽然引入了hadoop模型，但是框架上开发者还是隐藏的。业务处理类既可以在单机模式下运行也可以在hadoop上运行，并且可以调用spring、ibatis。减少了开发的学习成本，在实战中慢慢体会就学会了一项新技能。

界面截图：
[![task4]()](http://www.lanceyan.com/wp-content/uploads/2013/05/task4.png)
来源： <[http://www.lanceyan.com/category/tech/hadoop](http://www.lanceyan.com/category/tech/hadoop)> 
