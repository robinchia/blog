---
layout: post
title: "聊聊并发（三）——JAVA线程池的分析和使用"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_多线程
 - 并发
--- 

# 聊聊并发（三）——JAVA线程池的分析和使用

### 分享到

* [一键分享]()
* [QQ空间]()
* [新浪微博]()
* [百度搜藏]()
* [人人网]()
* [腾讯微博]()
* [百度相册]()
* [开心网]()
* [腾讯朋友]()
* [百度贴吧]()
* [豆瓣网]()
* [搜狐微博]()
* [百度新首页]()
* [QQ好友]()
* [和讯微博]()
* [更多...]()

[百度分享]()

* [关于我们](http://www.infoq.com/cn/aboutus "关于我们")
* [让大家在InfoQ上听见你的声音](http://www.infoq.com/cn/contribute "让大家在InfoQ上听见你的声音")

* [![]()](http://e.weibo.com/infoqchina)
* [![]()](http://www.infoq.com/cn/news/2013/02/infoq-wechat)
* [![]()](http://www.infoq.com/cn/rss/rss.action?token=J6EBu2hcpyEuMs9msdo8GyNCZTcWG7pm)

# 聊聊并发（三）——JAVA线程池的分析和使用

作者 [方腾飞](http://www.infoq.com/cn/author/%E6%96%B9%E8%85%BE%E9%A3%9E) 发布于 十一月 15, 2012 *|* [14 评论]()

## 1. 引言

合理利用线程池能够带来三个好处。第一：降低资源消耗。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。第二：提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行。第三：提高线程的可管理性。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。但是要做到合理的利用线程池，必须对其原理了如指掌。

## 2. 线程池的使用

**线程池的创建**

我们可以通过ThreadPoolExecutor来创建一个线程池。
new ThreadPoolExecutor(corePoolSize, maximumPoolSize, keepAliveTime, milliseconds,runnableTaskQueue, handler);

创建一个线程池需要输入几个参数：

* corePoolSize（线程池的基本大小）：当提交一个任务到线程池时，线程池会创建一个线程来执行任务，即使其他空闲的基本线程能够执行新任务也会创建线程，等到需要执行的任务数大于线程池基本大小时就不再创建。如果调用了线程池的prestartAllCoreThreads方法，线程池会提前创建并启动所有基本线程。
* runnableTaskQueue（任务队列）：用于保存等待执行的任务的阻塞队列。 可以选择以下几个阻塞队列。

* ArrayBlockingQueue：是一个基于数组结构的有界阻塞队列，此队列按 FIFO（先进先出）原则对元素进行排序。
* LinkedBlockingQueue：一个基于链表结构的阻塞队列，此队列按FIFO （先进先出） 排序元素，吞吐量通常要高于ArrayBlockingQueue。静态工厂方法Executors.newFixedThreadPool()使用了这个队列。
* SynchronousQueue：一个不存储元素的阻塞队列。每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态，吞吐量通常要高于LinkedBlockingQueue，静态工厂方法Executors.newCachedThreadPool使用了这个队列。
* PriorityBlockingQueue：一个具有优先级的无限阻塞队列。
* maximumPoolSize（线程池最大大小）：线程池允许创建的最大线程数。如果队列满了，并且已创建的线程数小于最大线程数，则线程池会再创建新的线程执行任务。值得注意的是如果使用了无界的任务队列这个参数就没什么效果。
* ThreadFactory：用于设置创建线程的工厂，可以通过线程工厂给每个创建出来的线程设置更有意义的名字。
* RejectedExecutionHandler（饱和策略）：当队列和线程池都满了，说明线程池处于饱和状态，那么必须采取一种策略处理提交的新任务。这个策略默认情况下是AbortPolicy，表示无法处理新任务时抛出异常。以下是JDK1.5提供的四种策略。

* AbortPolicy：直接抛出异常。
* CallerRunsPolicy：只用调用者所在线程来运行任务。
* DiscardOldestPolicy：丢弃队列里最近的一个任务，并执行当前任务。
* DiscardPolicy：不处理，丢弃掉。
* 当然也可以根据应用场景需要来实现RejectedExecutionHandler接口自定义策略。如记录日志或持久化不能处理的任务。
* keepAliveTime（线程活动保持时间）：线程池的工作线程空闲后，保持存活的时间。所以如果任务很多，并且每个任务执行的时间比较短，可以调大这个时间，提高线程的利用率。
* TimeUnit（线程活动保持时间的单位）：可选的单位有天（DAYS），小时（HOURS），分钟（MINUTES），毫秒(MILLISECONDS)，微秒(MICROSECONDS, 千分之一毫秒)和毫微秒(NANOSECONDS, 千分之一微秒)。

**向线程池提交任务**

我们可以使用execute提交的任务，但是execute方法没有返回值，所以无法判断任务是否被线程池执行成功。通过以下代码可知execute方法输入的任务是一个Runnable类的实例。
threadsPool.execute(new Runnable() { @Override public void run() { // TODO Auto-generated method stub } });

我们也可以使用submit 方法来提交任务，它会返回一个future,那么我们可以通过这个future来判断任务是否执行成功，通过future的get方法来获取返回值，get方法会阻塞住直到任务完成，而使用get(long timeout, TimeUnit unit)方法则会阻塞一段时间后立即返回，这时有可能任务没有执行完。

Future<Object> future = executor.submit(harReturnValuetask); try { Object s = future.get(); } catch (InterruptedException e) { // 处理中断异常 } catch (ExecutionException e) { // 处理无法执行任务异常 } finally { // 关闭线程池 executor.shutdown(); }

**线程池的关闭**

我们可以通过调用线程池的shutdown或shutdownNow方法来关闭线程池，它们的原理是遍历线程池中的工作线程，然后逐个调用线程的interrupt方法来中断线程，所以无法响应中断的任务可能永远无法终止。但是它们存在一定的区别，shutdownNow首先将线程池的状态设置成STOP，然后尝试停止所有的正在执行或暂停任务的线程，并返回等待执行任务的列表，而shutdown只是将线程池的状态设置成SHUTDOWN状态，然后中断所有没有正在执行任务的线程。

只要调用了这两个关闭方法的其中一个，isShutdown方法就会返回true。当所有的任务都已关闭后,才表示线程池关闭成功，这时调用isTerminaed方法会返回true。至于我们应该调用哪一种方法来关闭线程池，应该由提交到线程池的任务特性决定，通常调用shutdown来关闭线程池，如果任务不一定要执行完，则可以调用shutdownNow。

## 3. 线程池的分析

流程分析：线程池的主要工作流程如下图：

![]()

从上图我们可以看出，当提交一个新任务到线程池时，线程池的处理流程如下：

1. 首先线程池判断**基本线程池**是否已满？没满，创建一个工作线程来执行任务。满了，则进入下个流程。
1. 其次线程池判断**工作队列**是否已满？没满，则将新提交的任务存储在工作队列里。满了，则进入下个流程。
1. 最后线程池判断**整个线程池**是否已满？没满，则创建一个新的工作线程来执行任务，满了，则交给饱和策略来处理这个任务。

**源码分析**。上面的流程分析让我们很直观的了解了线程池的工作原理，让我们再通过源代码来看看是如何实现的。线程池执行任务的方法如下：
public void execute(Runnable command) { if (command == null) throw new NullPointerException(); //如果线程数小于基本线程数，则创建线程并执行当前任务 if (poolSize >= corePoolSize || !addIfUnderCorePoolSize(command)) { //如线程数大于等于基本线程数或线程创建失败，则将当前任务放到工作队列中。 if (runState == RUNNING && workQueue.offer(command)) { if (runState != RUNNING || poolSize == 0) ensureQueuedTaskHandled(command); } //如果线程池不处于运行中或任务无法放入队列，并且当前线程数量小于最大允许的线程数量， 则创建一个线程执行任务。 else if (!addIfUnderMaximumPoolSize(command)) //抛出RejectedExecutionException异常 reject(command); // is shutdown or saturated } }

**工作线程**。线程池创建线程时，会将线程封装成工作线程Worker，Worker在执行完任务后，还会无限循环获取工作队列里的任务来执行。我们可以从Worker的run方法里看到这点：

public void run() { try { Runnable task = firstTask; firstTask = null; while (task != null || (task = getTask()) != null) { runTask(task); task = null; } } finally { workerDone(this); } }

## 4. 合理的配置线程池

要想合理的配置线程池，就必须首先分析任务特性，可以从以下几个角度来进行分析：

1. 任务的性质：CPU密集型任务，IO密集型任务和混合型任务。
1. 任务的优先级：高，中和低。
1. 任务的执行时间：长，中和短。
1. 任务的依赖性：是否依赖其他系统资源，如数据库连接。

任务性质不同的任务可以用不同规模的线程池分开处理。CPU密集型任务配置尽可能小的线程，如配置Ncpu+1个线程的线程池。IO密集型任务则由于线程并不是一直在执行任务，则配置尽可能多的线程，如2*Ncpu。混合型的任务，如果可以拆分，则将其拆分成一个CPU密集型任务和一个IO密集型任务，只要这两个任务执行的时间相差不是太大，那么分解后执行的吞吐率要高于串行执行的吞吐率，如果这两个任务执行时间相差太大，则没必要进行分解。我们可以通过Runtime.getRuntime().availableProcessors()方法获得当前设备的CPU个数。

优先级不同的任务可以使用优先级队列PriorityBlockingQueue来处理。它可以让优先级高的任务先得到执行，需要注意的是如果一直有优先级高的任务提交到队列里，那么优先级低的任务可能永远不能执行。

执行时间不同的任务可以交给不同规模的线程池来处理，或者也可以使用优先级队列，让执行时间短的任务先执行。

依赖数据库连接池的任务，因为线程提交SQL后需要等待数据库返回结果，如果等待的时间越长CPU空闲时间就越长，那么线程数应该设置越大，这样才能更好的利用CPU。

建议使用有界队列，有界队列能增加系统的稳定性和预警能力，可以根据需要设大一点，比如几千。有一次我们组使用的后台任务线程池的队列和线程池全满了，不断的抛出抛弃任务的异常，通过排查发现是数据库出现了问题，导致执行SQL变得非常缓慢，因为后台任务线程池里的任务全是需要向数据库查询和插入数据的，所以导致线程池里的工作线程全部阻塞住，任务积压在线程池里。如果当时我们设置成无界队列，线程池的队列就会越来越多，有可能会撑满内存，导致整个系统不可用，而不只是后台任务出现问题。当然我们的系统所有的任务是用的单独的服务器部署的，而我们使用不同规模的线程池跑不同类型的任务，但是出现这样问题时也会影响到其他任务。

## 5. 线程池的监控

通过线程池提供的参数进行监控。线程池里有一些属性在监控线程池的时候可以使用

* taskCount：线程池需要执行的任务数量。
* completedTaskCount：线程池在运行过程中已完成的任务数量。小于或等于taskCount。
* largestPoolSize：线程池曾经创建过的最大线程数量。通过这个数据可以知道线程池是否满过。如等于线程池的最大大小，则表示线程池曾经满了。
* getPoolSize:线程池的线程数量。如果线程池不销毁的话，池里的线程不会自动销毁，所以这个大小只增不+ getActiveCount：获取活动的线程数。

通过扩展线程池进行监控。通过继承线程池并重写线程池的beforeExecute，afterExecute和terminated方法，我们可以在任务执行前，执行后和线程池关闭前干一些事情。如监控任务的平均执行时间，最大执行时间和最小执行时间等。这几个方法在线程池里是空方法。如：
protected void beforeExecute(Thread t, Runnable r) { }

## 6. 参考资料

* Java并发编程实战。
* JDK1.6源码

## 作者介绍

**方腾飞**，花名清英，淘宝资深开发工程师，关注并发编程，目前在广告技术部从事无线广告联盟的开发和设计工作。个人博客：[http://ifeve.com](http://ifeve.com/) 微博：[http://weibo.com/kirals](http://weibo.com/kirals) 欢迎通过我的微博进行技术交流。

感谢[张龙](http://www.infoq.com/cn/bycategory.action?authorName=张龙)对本文的审校。

给InfoQ中文站投稿或者参与内容翻译工作，请邮件至[editors@cn.infoq.com](mailto:editors@cn.infoq.com)。也欢迎大家通过新浪微博（[@InfoQ](http://www.weibo.com/infoqchina)）或者腾讯微博（[@InfoQ](http://t.qq.com/infoqchina)）关注我们，并与我们的编辑和其他读者朋友交流。
* [Sections]()
* [**语言 & 开发**](http://www.infoq.com/cn/development)
* [Topics]()
* [线程技术](http://www.infoq.com/cn/threading)
* [多线程](http://www.infoq.com/cn/Multi-threading)
* [并发](http://www.infoq.com/cn/concurrency)
* [Java](http://www.infoq.com/cn/java)
* [线程池](http://www.infoq.com/cn/threadPool)
* [线程](http://www.infoq.com/cn/thread)

相关内容
### [深入理解Java内存模型（七）——总结](http://www.infoq.com/cn/articles/java-memory-model-7?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（六）——final](http://www.infoq.com/cn/articles/java-memory-model-6?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（五）——锁](http://www.infoq.com/cn/articles/java-memory-model-5?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（六）——ConcurrentLinkedQueue的实现原理分析](http://www.infoq.com/cn/articles/ConcurrentLinkedQueue?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（五）——原子操作的实现原理](http://www.infoq.com/cn/articles/atomic-operation?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（七）——总结](http://www.infoq.com/cn/articles/java-memory-model-7?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（六）——final](http://www.infoq.com/cn/articles/java-memory-model-6?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（五）——锁](http://www.infoq.com/cn/articles/java-memory-model-5?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型](http://www.infoq.com/cn/minibooks/java_memory_model?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（四）——volatile](http://www.infoq.com/cn/articles/java-memory-model-4?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)
## 您好，陌生人！

您需要 [注册一个InfoQ账号](http://www.infoq.com/reginit.action) 或者 [登录]() 才能进行评论。在您完成注册后还需要进行一些设置。
## 获得来自InfoQ的更多体验。[]()

允许的HTML标签: a,b,br,blockquote,i,li,pre,u,ul,p

当有人回复此评论时请E-mail通知我

社区评论 [Watch Thread]()

[**拜读，有一些困惑望解答** by 史 雨鑫 Posted 16/11/2012 03:19]()
[**Re: 拜读，有一些困惑望解答** by 方 腾飞 Posted 16/11/2012 05:06]()

[**Re: 拜读，有一些困惑望解答** by 萧 欢 Posted 18/11/2012 12:57]()
[**Re: 拜读，有一些困惑望解答** by 方 腾飞 Posted 18/11/2012 02:43]()

[**能谈谈thread pool和多核CPU的关系吗？ 怎样能保证thread pool的配置，让其能够最大化的压栈多核cpu** by biao jiang Posted 20/11/2012 11:02]()
[**Re: 能谈谈thread pool和多核CPU的关系吗？ 怎样能保证thread pool的配置，让其能够最大化的压栈多核cpu** by 萧 欢 Posted 20/11/2012 02:26]()

[**Re: 能谈谈thread pool和多核CPU的关系吗？ 怎样能保证thread pool的配置，让其能够最大化的压栈多核cpu** by 老 bull Posted 22/11/2012 11:48]()
[**Re: 能谈谈thread pool和多核CPU的关系吗？ 怎样能保证thread pool的配置，让其能够最大化的压栈多核cpu** by 方 腾飞 Posted 26/11/2012 00:00]()

[**使用的一些问题?** by Qiao stephen Posted 25/02/2013 02:23]()
[**Re: 使用的一些问题?** by vince vince Posted 16/04/2013 02:45]()

[**不知道是不是错误** by 高 鹏翔 Posted 29/03/2013 01:53]()
[**Re: 不知道是不是错误** by 方 腾飞 Posted 18/05/2013 01:25]()

[**昨晚再次阅读了该文，有几个问题需要解答** by 王 明军 Posted 27/05/2013 09:11]()
[**Re: 昨晚再次阅读了该文，有几个问题需要解答** by 方 腾飞 Posted 13/06/2013 03:38]()
[]()

**拜读，有一些困惑望解答** 16/11/2012 03:19 by 史 雨鑫

腾飞，线程池分析小节中，“队列是否满了”->”线程池是否满了“过程中，假设使用ArrayBlockingQueue队列，文中提到如果队列满了，并且线程池没满，则会新建线程处理此任务，那问题是此任务有可能会在队列任务被执行前执行，这样就不满足”先进先出“原则了吧？望解答。

* [回复]()
* [回到顶部]()

[]()

**Re: 拜读，有一些困惑望解答** 16/11/2012 05:06 by 方 腾飞

此任务是提交给线程池执行的新任务，不在队列里。队列里的任务存储的都是以前提交的任务，它们需要等待线程空闲时来执行。

* [回复]()
* [回到顶部]()
[]()

**Re: 拜读，有一些困惑望解答** 18/11/2012 12:57 by 萧 欢

你的理解是对的。如果在创建线程池的时候，指定的corePoolSize<maximumPoolsize是会出现你说的这种情况，这在线程池机制中称为“应急处理”。如果你的线程池模型用于任务有严格的先后顺序的情况下，指定corePoolSize=maximumPoolsize就不会有你说的问题了。通常用newFixedThreadPool这个工厂方法创建的线程池是corePoolSize=maximumPoolsize，遵循先进先出>

* [回复]()
* [回到顶部]()

[]()

**Re: 拜读，有一些困惑望解答** 18/11/2012 02:43 by 方 腾飞

是的。之前没理解你的问题。

* [回复]()
* [回到顶部]()
[]()

**能谈谈thread pool和多核CPU的关系吗？ 怎样能保证thread pool的配置，让其能够最大化的压栈多核cpu** 20/11/2012 11:02 by biao jiang

能谈谈thread pool和多核CPU的关系吗？ 怎样能保证thread pool的配置，让其能够最大化的压栈多核cpu

* [回复]()
* [回到顶部]()

[]()

**Re: 能谈谈thread pool和多核CPU的关系吗？ 怎样能保证thread pool的配置，让其能够最大化的压栈多核cpu** 20/11/2012 02:26 by 萧 欢

这个应该挺难的。只能说，理论上一个CPU分配一个线程是最优的情况，然而这只是理论上。线程池和CPU核数的关系也与线程池的任务模型息息相关，是CPU密集型还是I/O密集型。最简单的方式就是动态递增的增加线程池数目然后跑一段时间程序，观察系统吞吐率，寻找最适合你的那个高层点

* [回复]()
* [回到顶部]()
[]()

**Re: 能谈谈thread pool和多核CPU的关系吗？ 怎样能保证thread pool的配置，让其能够最大化的压栈多核cpu** 22/11/2012 11:48 by 老 bull

Java 1.2之后线程都是采用内核级线程（进程）实现的，一个JAVA用户线程最终会对应到一个内核线程上面托管，而内核线程在CPU上的调度是依赖操作系统的，这个问题可以从操作系统层面上解决或者寻求答案。

* [回复]()
* [回到顶部]()

[]()

**Re: 能谈谈thread pool和多核CPU的关系吗？ 怎样能保证thread pool的配置，让其能够最大化的压栈多核cpu** 26/11/2012 00:00 by 方 腾飞

文中有提到如果是CPU密集型任务，使用Ncpu+1个线程可以很好的压榨CPU。详细的可以参见“合理的配置线程池”章节。

* [回复]()
* [回到顶部]()
[]()

**使用的一些问题?** 25/02/2013 02:23 by Qiao stephen

如果我在一个应用中,存在多个小服务,每个服务都开启一个线程,请问那么是在整个应用中开启一个线程池还是每个小服务开启
一个线程池呢?如果是每个小服务开启一个,各个线程池之间又是怎么管理呢?
是不是使用一个线程池来管理上面的多个线程池呢?

* [回复]()
* [回到顶部]()

[]()

**不知道是不是错误** 29/03/2013 01:53 by 高 鹏翔

◦DiscardOldestPolicy：丢弃队列里最近的一个任务，并执行当前任务。
这个策略,我看的时候有点疑惑,进入代码里面看了下,应该是丢弃待执行的任务队列的对首,但是这个队列是FIFO的,所以我觉得应该是丢弃最远的一个任务,不知道是否理解的有误解,忘指正

* [回复]()
* [回到顶部]()
[]()

**Re: 使用的一些问题?** 16/04/2013 02:45 by vince vince

应该是在应用中建一个线程池，把每个服务要执行的方法体封装成Runnable对象提交到线程池里执行吧，线程池顾名思义，就是一个线程库，事先为你建好，要用的时候丢给它，它给你返回结果，至于优势嘛开头说得很清楚了~这么做可以简化客户端代码复杂度，让他只关心自己的线程体逻辑，而线程本身的生命周期等都不需要自己操心~

* [回复]()
* [回到顶部]()

[]()

**Re: 不知道是不是错误** 18/05/2013 01:25 by 方 腾飞

的确是丢弃最远的任务，源码如下
public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
if (!e.isShutdown()) {
e.getQueue().poll();
e.execute(r);
}
}

* [回复]()
* [回到顶部]()
[]()

**昨晚再次阅读了该文，有几个问题需要解答** 27/05/2013 09:11 by 王 明军

1、文中提到了线程池的关闭，那么线程池在什么情况下执行关闭呢？
2、什么是CPU密集型任务？
谢谢！有个问题通过阅读评论已经解决，是丢弃任务那个问题。

* [回复]()
* [回到顶部]()

[]()

**Re: 昨晚再次阅读了该文，有几个问题需要解答** 13/06/2013 03:38 by 方 腾飞

1：不需要使用的时候就关闭。比如服务器暂停使用。
2：CPU密集型任务，如压缩和解压缩，这种需要CPU不停的计算的任务。

* [回复]()
* [回到顶部]()

[关闭]()

### ****by

发布于

* [查看]()
* [回复]()
* [回到顶部]()
[关闭]() 主题  您的回复 [引用原消息]()

允许的HTML标签: a,b,br,blockquote,i,li,pre,u,ul,p
当有人回复此评论时请E-mail通知我

[关闭]() 主题  您的回复

允许的HTML标签: a,b,br,blockquote,i,li,pre,u,ul,p
当有人回复此评论时请E-mail通知我
[关闭]()
* 热点内容
* [本周]()
* [本月]()
* [近6个月]()
### [计算机科学中最重要的32个算法](http://www.infoq.com/cn/news/2012/08/32-most-important-algorithms?utm_source=infoq&utm_medium=popular_links_homepage)

### [ThoughtWorks技术雷达（2013年5月）](http://www.infoq.com/cn/articles/thoughtWorks-technology-radar?utm_source=infoq&utm_medium=popular_links_homepage)

### [让1.5亿移动端用户第一时间获取消息](http://www.infoq.com/cn/articles/iqiyi-cloud-push-practices?utm_source=infoq&utm_medium=popular_links_homepage)

### [ThoughtWorks全球CEO郭晓谈软件人才的招聘与培养](http://www.infoq.com/cn/news/2013/06/tw-guoxiao-on-talents?utm_source=infoq&utm_medium=popular_links_homepage)

### [影响可扩展性的十宗罪](http://www.infoq.com/cn/news/2013/06/10-sins-for-scalability?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（6月刊）](http://www.infoq.com/cn/minibooks/architect-jun-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [使用功能开关更好地实现持续部署](http://www.infoq.com/cn/articles/function-switch-realize-better-continuous-implementations?utm_source=infoq&utm_medium=popular_links_homepage)

### [Eclipse迁移到GitHub](http://www.infoq.com/cn/news/2013/06/eclipse-github?utm_source=infoq&utm_medium=popular_links_homepage)

### [Newegg.com大数据实践](http://www.infoq.com/cn/presentations/newegg-big-data-practice?utm_source=infoq&utm_medium=popular_links_homepage)

### [软件系统架构：使用视点和视角与利益相关者合作](http://www.infoq.com/cn/minibooks/software-sys-architect?utm_source=infoq&utm_medium=popular_links_homepage)

### [计算机科学中最重要的32个算法](http://www.infoq.com/cn/news/2012/08/32-most-important-algorithms?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（6月刊）](http://www.infoq.com/cn/minibooks/architect-jun-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [REST的缺点是什么？](http://www.infoq.com/cn/news/2013/06/rest-drawbacks?utm_source=infoq&utm_medium=popular_links_homepage)

### [Google 发布新一代Web UI库Polymer](http://www.infoq.com/cn/news/2013/06/webcomponents?utm_source=infoq&utm_medium=popular_links_homepage)

### [大众点评网域名劫持事件概述](http://www.infoq.com/cn/news/2013/06/dianping-xinnet-hacked?utm_source=infoq&utm_medium=popular_links_homepage)

### [基于Node.js的自动化构建工具Grunt.js](http://www.infoq.com/cn/articles/GruntJs?utm_source=infoq&utm_medium=popular_links_homepage)

### [深入浅出Node.js（一）：什么是Node.js](http://www.infoq.com/cn/articles/what-is-nodejs?utm_source=infoq&utm_medium=popular_links_homepage)

### [代码之殇·第二版](http://www.infoq.com/cn/minibooks/i-m-wrights-hard-code?utm_source=infoq&utm_medium=popular_links_homepage)

### [软件系统架构：使用视点和视角与利益相关者合作](http://www.infoq.com/cn/minibooks/software-sys-architect?utm_source=infoq&utm_medium=popular_links_homepage)

### [大家谈18岁的Java——朱鸿：开过跑车后再去开大巴车总是有点不爽的](http://www.infoq.com/cn/news/2013/06/zhuhong-on-java?utm_source=infoq&utm_medium=popular_links_homepage)
### [计算机科学中最重要的32个算法](http://www.infoq.com/cn/news/2012/08/32-most-important-algorithms?utm_source=infoq&utm_medium=popular_links_homepage)

### [深入理解Java内存模型（一）——基础](http://www.infoq.com/cn/articles/java-memory-model-1?utm_source=infoq&utm_medium=popular_links_homepage)

### [深入浅出Node.js（一）：什么是Node.js](http://www.infoq.com/cn/articles/what-is-nodejs?utm_source=infoq&utm_medium=popular_links_homepage)

### [深入浅出Node.js（二）：Node.js&NPM的安装与配置](http://www.infoq.com/cn/articles/nodejs-npm-install-config?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（3月刊）](http://www.infoq.com/cn/minibooks/architect-mar-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（1月刊）](http://www.infoq.com/cn/minibooks/architect-jan-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（4月刊）](http://www.infoq.com/cn/minibooks/architect-apr-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [聊聊并发（三）——JAVA线程池的分析和使用](http://www.infoq.com/cn/articles/java-threadPool?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（2月刊）](http://www.infoq.com/cn/minibooks/architect-feb-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [12306订票助手插件拖垮GitHub事件原因始末](http://www.infoq.com/cn/news/2013/01/12306-plugin-ddos-github?utm_source=infoq&utm_medium=popular_links_homepage)

### [计算机科学中最重要的32个算法](http://www.infoq.com/cn/news/2012/08/32-most-important-algorithms?utm_source=infoq&utm_medium=popular_links_homepage)

### [ThoughtWorks技术雷达（2013年5月）](http://www.infoq.com/cn/articles/thoughtWorks-technology-radar?utm_source=infoq&utm_medium=popular_links_homepage)

### [让1.5亿移动端用户第一时间获取消息](http://www.infoq.com/cn/articles/iqiyi-cloud-push-practices?utm_source=infoq&utm_medium=popular_links_homepage)

### [ThoughtWorks全球CEO郭晓谈软件人才的招聘与培养](http://www.infoq.com/cn/news/2013/06/tw-guoxiao-on-talents?utm_source=infoq&utm_medium=popular_links_homepage)

### [影响可扩展性的十宗罪](http://www.infoq.com/cn/news/2013/06/10-sins-for-scalability?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（6月刊）](http://www.infoq.com/cn/minibooks/architect-jun-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [使用功能开关更好地实现持续部署](http://www.infoq.com/cn/articles/function-switch-realize-better-continuous-implementations?utm_source=infoq&utm_medium=popular_links_homepage)

### [Eclipse迁移到GitHub](http://www.infoq.com/cn/news/2013/06/eclipse-github?utm_source=infoq&utm_medium=popular_links_homepage)

### [Newegg.com大数据实践](http://www.infoq.com/cn/presentations/newegg-big-data-practice?utm_source=infoq&utm_medium=popular_links_homepage)

### [软件系统架构：使用视点和视角与利益相关者合作](http://www.infoq.com/cn/minibooks/software-sys-architect?utm_source=infoq&utm_medium=popular_links_homepage)
### [计算机科学中最重要的32个算法](http://www.infoq.com/cn/news/2012/08/32-most-important-algorithms?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（6月刊）](http://www.infoq.com/cn/minibooks/architect-jun-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [REST的缺点是什么？](http://www.infoq.com/cn/news/2013/06/rest-drawbacks?utm_source=infoq&utm_medium=popular_links_homepage)

### [Google 发布新一代Web UI库Polymer](http://www.infoq.com/cn/news/2013/06/webcomponents?utm_source=infoq&utm_medium=popular_links_homepage)

### [大众点评网域名劫持事件概述](http://www.infoq.com/cn/news/2013/06/dianping-xinnet-hacked?utm_source=infoq&utm_medium=popular_links_homepage)

### [基于Node.js的自动化构建工具Grunt.js](http://www.infoq.com/cn/articles/GruntJs?utm_source=infoq&utm_medium=popular_links_homepage)

### [深入浅出Node.js（一）：什么是Node.js](http://www.infoq.com/cn/articles/what-is-nodejs?utm_source=infoq&utm_medium=popular_links_homepage)

### [代码之殇·第二版](http://www.infoq.com/cn/minibooks/i-m-wrights-hard-code?utm_source=infoq&utm_medium=popular_links_homepage)

### [软件系统架构：使用视点和视角与利益相关者合作](http://www.infoq.com/cn/minibooks/software-sys-architect?utm_source=infoq&utm_medium=popular_links_homepage)

### [大家谈18岁的Java——朱鸿：开过跑车后再去开大巴车总是有点不爽的](http://www.infoq.com/cn/news/2013/06/zhuhong-on-java?utm_source=infoq&utm_medium=popular_links_homepage)

### [计算机科学中最重要的32个算法](http://www.infoq.com/cn/news/2012/08/32-most-important-algorithms?utm_source=infoq&utm_medium=popular_links_homepage)

### [深入理解Java内存模型（一）——基础](http://www.infoq.com/cn/articles/java-memory-model-1?utm_source=infoq&utm_medium=popular_links_homepage)

### [深入浅出Node.js（一）：什么是Node.js](http://www.infoq.com/cn/articles/what-is-nodejs?utm_source=infoq&utm_medium=popular_links_homepage)

### [深入浅出Node.js（二）：Node.js&NPM的安装与配置](http://www.infoq.com/cn/articles/nodejs-npm-install-config?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（3月刊）](http://www.infoq.com/cn/minibooks/architect-mar-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（1月刊）](http://www.infoq.com/cn/minibooks/architect-jan-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（4月刊）](http://www.infoq.com/cn/minibooks/architect-apr-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [聊聊并发（三）——JAVA线程池的分析和使用](http://www.infoq.com/cn/articles/java-threadPool?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（2月刊）](http://www.infoq.com/cn/minibooks/architect-feb-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [12306订票助手插件拖垮GitHub事件原因始末](http://www.infoq.com/cn/news/2013/01/12306-plugin-ddos-github?utm_source=infoq&utm_medium=popular_links_homepage)

## 深度内容

* [全部]()
* [文章]()
* [演讲]()
* [访谈]()
* [迷你书]()
  

## [Juergen Fesslmeier谈端到端的JavaScript开发](http://www.infoq.com/cn/interviews/end-to-end-javascript "Juergen Fesslmeier谈端到端的JavaScript开发")

[Juergen Fesslmeier](http://www.infoq.com/cn/author/Juergen-Fesslmeier "Juergen Fesslmeier") 七月 02, 2013

[![]()](http://www.infoq.com/cn/interviews/end-to-end-javascript "Juergen Fesslmeier谈端到端的JavaScript开发")
## [设计模式自动化](http://www.infoq.com/cn/articles/Design-Pattern-Automation "设计模式自动化")

[Gael Fraiteur and Yan Cui](http://www.infoq.com/cn/author/Gael-Fraiteur-and-Yan-Cui "Gael Fraiteur and Yan Cui") 七月 01, 2013

[![]()](http://www.infoq.com/cn/articles/Design-Pattern-Automation "设计模式自动化")

## [设计指尖上的世界：移动用户界面一瞥](http://www.infoq.com/cn/articles/designing-a-world-at-your-fingertips-mobile-user-interfaces "设计指尖上的世界：移动用户界面一瞥")

[Forrest Shull](http://www.infoq.com/cn/author/Forrest-Shull "Forrest Shull") 六月 28, 2013

[![]()](http://www.infoq.com/cn/articles/designing-a-world-at-your-fingertips-mobile-user-interfaces "设计指尖上的世界：移动用户界面一瞥")
## [成功的根本—集成的ALM工具](http://www.infoq.com/cn/articles/Integrated-ALM "成功的根本—集成的ALM工具")

[Dave West](http://www.infoq.com/cn/author/Dave-West "Dave West") 六月 28, 2013

[![]()](http://www.infoq.com/cn/articles/Integrated-ALM "成功的根本—集成的ALM工具")

## [书评：验收测试驱动开发实践指南](http://www.infoq.com/cn/articles/atdd-by-example-book "书评：验收测试驱动开发实践指南")

[Manuel Pais](http://www.infoq.com/cn/author/Manuel-Pais "Manuel Pais") 六月 26, 2013

[![]()](http://www.infoq.com/cn/articles/atdd-by-example-book "书评：验收测试驱动开发实践指南")
## [跨终端的web](http://www.infoq.com/cn/presentations/across-device-web "跨终端的web")

[舒文亮](http://www.infoq.com/cn/author/%E8%88%92%E6%96%87%E4%BA%AE "舒文亮") 六月 26, 2013

[![]()](http://www.infoq.com/cn/presentations/across-device-web "跨终端的web")

* [更早的 >]()

语言 & 开发

[Juergen Fesslmeier谈端到端的JavaScript开发](http://www.infoq.com/cn/interviews/end-to-end-javascript "Juergen Fesslmeier谈端到端的JavaScript开发")

[MobileCloud for TFS支持测试Windows Phone,Android,iOS及BlackBerry应用](http://www.infoq.com/cn/news/2013/07/mobilecloud-tfs "MobileCloud for TFS支持测试Windows Phone,Android,iOS及BlackBerry应用")

[百度技术沙龙第39期回顾：前端快速开发实践（含资料下载）](http://www.infoq.com/cn/news/2013/07/baidu-salon39-summary "百度技术沙龙第39期回顾：前端快速开发实践（含资料下载）")

架构 & 设计

[内存与本机代码的性能](http://www.infoq.com/cn/news/2013/07/Native-Performance "内存与本机代码的性能")

[设计模式自动化](http://www.infoq.com/cn/articles/Design-Pattern-Automation "设计模式自动化")

[连接设备编程](http://www.infoq.com/cn/news/2013/07/WinRT-Devices "连接设备编程")
过程 & 实践

[成功的根本—集成的ALM工具](http://www.infoq.com/cn/articles/Integrated-ALM "成功的根本—集成的ALM工具")

[ThoughtWorks全球CEO郭晓谈软件人才的招聘与培养](http://www.infoq.com/cn/news/2013/06/tw-guoxiao-on-talents "ThoughtWorks全球CEO郭晓谈软件人才的招聘与培养")

[书评：验收测试驱动开发实践指南](http://www.infoq.com/cn/articles/atdd-by-example-book "书评：验收测试驱动开发实践指南")

运维 & 基础架构

[在传统企业中引入DevOps](http://www.infoq.com/cn/news/2013/06/devops-in-traditional-enterprise "在传统企业中引入DevOps")

[安全性——“DevOpS”中的S](http://www.infoq.com/cn/news/2013/06/dod-ams-day1-s-is-for-security "安全性——“DevOpS”中的S")

[书评：验收测试驱动开发实践指南](http://www.infoq.com/cn/articles/atdd-by-example-book "书评：验收测试驱动开发实践指南")
企业架构

[设计指尖上的世界：移动用户界面一瞥](http://www.infoq.com/cn/articles/designing-a-world-at-your-fingertips-mobile-user-interfaces "设计指尖上的世界：移动用户界面一瞥")

[Stratos 2.0已发布，支持所有运行时环境和30个IaaS](http://www.infoq.com/cn/news/2013/06/stratos-2 "Stratos 2.0已发布，支持所有运行时环境和30个IaaS")

[让1.5亿移动端用户第一时间获取消息](http://www.infoq.com/cn/articles/iqiyi-cloud-push-practices "让1.5亿移动端用户第一时间获取消息")

语言 & 开发

[Juergen Fesslmeier谈端到端的JavaScript开发](http://www.infoq.com/cn/interviews/end-to-end-javascript "Juergen Fesslmeier谈端到端的JavaScript开发")

[MobileCloud for TFS支持测试Windows Phone,Android,iOS及BlackBerry应用](http://www.infoq.com/cn/news/2013/07/mobilecloud-tfs "MobileCloud for TFS支持测试Windows Phone,Android,iOS及BlackBerry应用")

[百度技术沙龙第39期回顾：前端快速开发实践（含资料下载）](http://www.infoq.com/cn/news/2013/07/baidu-salon39-summary "百度技术沙龙第39期回顾：前端快速开发实践（含资料下载）")
架构 & 设计

[内存与本机代码的性能](http://www.infoq.com/cn/news/2013/07/Native-Performance "内存与本机代码的性能")

[设计模式自动化](http://www.infoq.com/cn/articles/Design-Pattern-Automation "设计模式自动化")

[连接设备编程](http://www.infoq.com/cn/news/2013/07/WinRT-Devices "连接设备编程")

过程 & 实践

[成功的根本—集成的ALM工具](http://www.infoq.com/cn/articles/Integrated-ALM "成功的根本—集成的ALM工具")

[ThoughtWorks全球CEO郭晓谈软件人才的招聘与培养](http://www.infoq.com/cn/news/2013/06/tw-guoxiao-on-talents "ThoughtWorks全球CEO郭晓谈软件人才的招聘与培养")

[书评：验收测试驱动开发实践指南](http://www.infoq.com/cn/articles/atdd-by-example-book "书评：验收测试驱动开发实践指南")
运维 & 基础架构

[在传统企业中引入DevOps](http://www.infoq.com/cn/news/2013/06/devops-in-traditional-enterprise "在传统企业中引入DevOps")

[安全性——“DevOpS”中的S](http://www.infoq.com/cn/news/2013/06/dod-ams-day1-s-is-for-security "安全性——“DevOpS”中的S")

[书评：验收测试驱动开发实践指南](http://www.infoq.com/cn/articles/atdd-by-example-book "书评：验收测试驱动开发实践指南")

企业架构

[设计指尖上的世界：移动用户界面一瞥](http://www.infoq.com/cn/articles/designing-a-world-at-your-fingertips-mobile-user-interfaces "设计指尖上的世界：移动用户界面一瞥")

[Stratos 2.0已发布，支持所有运行时环境和30个IaaS](http://www.infoq.com/cn/news/2013/06/stratos-2 "Stratos 2.0已发布，支持所有运行时环境和30个IaaS")

[让1.5亿移动端用户第一时间获取消息](http://www.infoq.com/cn/articles/iqiyi-cloud-push-practices "让1.5亿移动端用户第一时间获取消息")
[Close]() E-mail  密码

[使用Google账号登录](http://www.infoq.com/cn/social/googleLogin.action?fl=login "使用Google账号登录") [使用Microsoft账号登录](http://www.infoq.com/cn/social/liveLogin.action?fl=login "使用Microsoft账号登录")
[忘记密码？]()
 InfoQ账号使用的E-mail 发送邮件

[重新登录]()
重新发送激活信息 重新发送

[重新登录]()
[没有用户名？](http://www.infoq.com/cn/reginit.action)

[点击注册](http://www.infoq.com/cn/reginit.action)
