---
layout: post
title: "聊聊并发（五）——原子操作的实现原理"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_多线程
 - 并发
--- 

# 聊聊并发（五）——原子操作的实现原理

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

# 聊聊并发（五）——原子操作的实现原理

作者 [方腾飞](http://www.infoq.com/cn/author/%E6%96%B9%E8%85%BE%E9%A3%9E) 发布于 十一月 29, 2012 *|* [14 评论]()

## 1. 引言

原子（atom）本意是“不能被进一步分割的最小粒子”，而原子操作（atomic operation）意为"不可被中断的一个或一系列操作" 。在多处理器上实现原子操作就变得有点复杂。本文让我们一起来聊一聊在Intel处理器和Java里是如何实现原子操作的。

## 2. 术语定义

术语 英文 解释 缓存行 Cache line 缓存的最小操作单位 比较并交换 Compare and Swap CAS操作需要输入两个数值，一个旧值（期望操作前的值）和一个新值，在操作期间先比较下旧值有没有发生变化，如果没有发生变化，才交换成新值，发生了变化则不交换。 CPU流水线 CPU pipeline CPU流水线的工作方式就象工业生产上的装配流水线，在CPU中由5~6个不同功能的电路单元组成一条指令处理流水线，然后将一条X86指令分成5~6步后再由这些电路单元分别执行，这样就能实现在一个CPU时钟周期完成一条指令，因此提高CPU的运算速度。 内存顺序冲突 Memory order violation 内存顺序冲突一般是由假共享引起，假共享是指多个CPU同时修改同一个缓存行的不同部分而引起其中一个CPU的操作无效，当出现这个内存顺序冲突时，CPU必须清空流水线。

## 3. 处理器如何实现原子操作

32位IA-32处理器使用基于对缓存加锁或总线加锁的方式来实现多处理器之间的原子操作。

### 3.1 处理器自动保证基本内存操作的原子性

首先处理器会自动保证基本的内存操作的原子性。处理器保证从系统内存当中读取或者写入一个字节是原子的，意思是当一个处理器读取一个字节时，其他处理器不能访问这个字节的内存地址。奔腾6和最新的处理器能自动保证单处理器对同一个缓存行里进行16/32/64位的操作是原子的，但是复杂的内存操作处理器不能自动保证其原子性，比如跨总线宽度，跨多个缓存行，跨页表的访问。但是处理器提供总线锁定和缓存锁定两个机制来保证复杂内存操作的原子性。

### 3.2 使用总线锁保证原子性

第一个机制是通过总线锁保证原子性。如果多个处理器同时对共享变量进行读改写（i++就是经典的读改写操作）操作，那么共享变量就会被多个处理器同时进行操作，这样读改写操作就不是原子的，操作完之后共享变量的值会和期望的不一致，举个例子：如果i=1,我们进行两次i++操作，我们期望的结果是3，但是有可能结果是2。如下图

![]()

（例1）

原因是有可能多个处理器同时从各自的缓存中读取变量i，分别进行加一操作，然后分别写入系统内存当中。那么想要保证读改写共享变量的操作是原子的，就必须保证CPU1读改写共享变量的时候，CPU2不能操作缓存了该共享变量内存地址的缓存。

处理器使用总线锁就是来解决这个问题的。所谓总线锁就是使用处理器提供的一个LOCK＃信号，当一个处理器在总线上输出此信号时，其他处理器的请求将被阻塞住,那么该处理器可以独占使用共享内存。

### 3.3 使用缓存锁保证原子性

第二个机制是通过缓存锁定保证原子性。在同一时刻我们只需保证对某个内存地址的操作是原子性即可，但总线锁定把CPU和内存之间通信锁住了，这使得锁定期间，其他处理器不能操作其他内存地址的数据，所以总线锁定的开销比较大，最近的处理器在某些场合下使用缓存锁定代替总线锁定来进行优化。

频繁使用的内存会缓存在处理器的L1，L2和L3高速缓存里，那么原子操作就可以直接在处理器内部缓存中进行，并不需要声明总线锁，在奔腾6和最近的处理器中可以使用“缓存锁定”的方式来实现复杂的原子性。所谓“缓存锁定”就是如果缓存在处理器缓存行中内存区域在LOCK操作期间被锁定，当它执行锁操作回写内存时，处理器不在总线上声言LOCK＃信号，而是修改内部的内存地址，并允许它的缓存一致性机制来保证操作的原子性，因为缓存一致性机制会阻止同时修改被两个以上处理器缓存的内存区域数据，当其他处理器回写已被锁定的缓存行的数据时会起缓存行无效，在例1中，当CPU1修改缓存行中的i时使用缓存锁定，那么CPU2就不能同时缓存了i的缓存行。

但是有两种情况下处理器不会使用缓存锁定。第一种情况是：当操作的数据不能被缓存在处理器内部，或操作的数据跨多个缓存行（cache line），则处理器会调用总线锁定。第二种情况是：有些处理器不支持缓存锁定。对于Inter486和奔腾处理器,就算锁定的内存区域在处理器的缓存行中也会调用总线锁定。

以上两个机制我们可以通过Inter处理器提供了很多LOCK前缀的指令来实现。比如位测试和修改指令BTS，BTR，BTC，交换指令XADD，CMPXCHG和其他一些操作数和逻辑指令，比如ADD（加），OR（或）等，被这些指令操作的内存区域就会加锁，导致其他处理器不能同时访问它。

## 4. JAVA如何实现原子操作

在java中可以通过锁和循环CAS的方式来实现原子操作。

### 4.1 使用循环CAS实现原子操作

JVM中的CAS操作正是利用了上一节中提到的处理器提供的CMPXCHG指令实现的。自旋CAS实现的基本思路就是循环进行CAS操作直到成功为止，以下代码实现了一个基于CAS线程安全的计数器方法safeCount和一个非线程安全的计数器count。
public class Counter { private AtomicInteger atomicI = new AtomicInteger(0); private int i = 0; public static void main(String[] args) { final Counter cas = new Counter(); List<Thread> ts = new ArrayList<Thread>(600); long start = System.currentTimeMillis(); for (int j = 0; j < 100; j++) { Thread t = new Thread(new Runnable() { @Override public void run() { for (int i = 0; i < 10000; i++) { cas.count(); cas.safeCount(); } } }); ts.add(t); } for (Thread t : ts) { t.start(); } // 等待所有线程执行完成 for (Thread t : ts) { try { t.join(); } catch (InterruptedException e) { e.printStackTrace(); } } System.out.println(cas.i); System.out.println(cas.atomicI.get()); System.out.println(System.currentTimeMillis() - start); } /** * 使用CAS实现线程安全计数器 */ private void safeCount() { for (;;) { int i = atomicI.get(); boolean suc = atomicI.compareAndSet(i, ++i); if (suc) { break; } } } /** * 非线程安全计数器 */ private void count() { i++; } }

在java并发包中有一些并发框架也使用了自旋CAS的方式来实现原子操作，比如LinkedTransferQueue类的Xfer方法。CAS虽然很高效的解决原子操作，但是CAS仍然存在三大问题。ABA问题，循环时间长开销大和只能保证一个共享变量的原子操作。

1. ABA问题。因为CAS需要在操作值的时候检查下值有没有发生变化，如果没有发生变化则更新，但是如果一个值原来是A，变成了B，又变成了A，那么使用CAS进行检查时会发现它的值没有发生变化，但是实际上却变化了。ABA问题的解决思路就是使用版本号。在变量前面追加上版本号，每次变量更新的时候把版本号加一，那么A－B－A 就会变成1A-2B－3A。
从Java1.5开始JDK的atomic包里提供了一个类AtomicStampedReference来解决ABA问题。这个类的compareAndSet方法作用是首先检查当前引用是否等于预期引用，并且当前标志是否等于预期标志，如果全部相等，则以原子方式将该引用和该标志的值设置为给定的更新值。
public boolean compareAndSet (V expectedReference,//预期引用 V newReference,//更新后的引用 int expectedStamp, //预期标志 int newStamp) //更新后的标志
1. 
循环时间长开销大。自旋CAS如果长时间不成功，会给CPU带来非常大的执行开销。如果JVM能支持处理器提供的pause指令那么效率会有一定的提升，pause指令有两个作用，第一它可以延迟流水线执行指令（de-pipeline）,使CPU不会消耗过多的执行资源，延迟的时间取决于具体实现的版本，在一些处理器上延迟时间是零。第二它可以避免在退出循环的时候因内存顺序冲突（memory order violation）而引起CPU流水线被清空（CPU pipeline flush），从而提高CPU的执行效率。
1. 
只能保证一个共享变量的原子操作。当对一个共享变量执行操作时，我们可以使用循环CAS的方式来保证原子操作，但是对多个共享变量操作时，循环CAS就无法保证操作的原子性，这个时候就可以用锁，或者有一个取巧的办法，就是把多个共享变量合并成一个共享变量来操作。比如有两个共享变量i＝2,j=a，合并一下ij=2a，然后用CAS来操作ij。从Java1.5开始JDK提供了AtomicReference类来保证引用对象之间的原子性，你可以把多个变量放在一个对象里来进行CAS操作。

### 4.2 使用锁机制实现原子操作

锁机制保证了只有获得锁的线程能够操作锁定的内存区域。JVM内部实现了很多种锁机制，有偏向锁，轻量级锁和互斥锁，有意思的是除了偏向锁，JVM实现锁的方式都用到的循环CAS，当一个线程想进入同步块的时候使用循环CAS的方式来获取锁，当它退出同步块的时候使用循环CAS释放锁。详细说明可以参见文章Java SE1.6中的Synchronized。

## 5. 参考资料

1. [Java SE1.6中的Synchronized](http://www.infoq.com/cn/articles/java-se-16-synchronized)
1. [Intel 64和IA-32架构软件开发人员手册](http://www.intel.com/products/processor/manuals/)
1. [深入分析Volatile的实现原理](http://www.infoq.com/cn/articles/ftf-java-volatile)

## 作者介绍

**方腾飞**，花名清英，淘宝资深开发工程师，关注并发编程，目前在广告技术部从事无线广告联盟的开发和设计工作。个人博客：[http://ifeve.com](http://ifeve.com/) 微博：[http://weibo.com/kirals](http://weibo.com/kirals) 欢迎通过我的微博进行技术交流。

感谢[张龙](http://www.infoq.com/cn/bycategory.action?authorName=张龙)对本文的审校。

给InfoQ中文站投稿或者参与内容翻译工作，请邮件至[editors@cn.infoq.com](mailto:editors@cn.infoq.com)。也欢迎大家通过新浪微博（[@InfoQ](http://www.weibo.com/infoqchina)）或者腾讯微博（[@InfoQ](http://t.qq.com/infoqchina)）关注我们，并与我们的编辑和其他读者朋友交流。
* [Sections]()
* [**架构 & 设计**](http://www.infoq.com/cn/architecture-design)
* [**语言 & 开发**](http://www.infoq.com/cn/development)
* [Topics]()
* [云计算](http://www.infoq.com/cn/cloud-computing)
* [多线程](http://www.infoq.com/cn/Multi-threading)
* [原子操作](http://www.infoq.com/cn/atomic-operation)
* [安全](http://www.infoq.com/cn/Security)
* [并发](http://www.infoq.com/cn/concurrency)
* [SOA平台](http://www.infoq.com/cn/soa_platforms)
* [架构](http://www.infoq.com/cn/architecture)
* [Java](http://www.infoq.com/cn/java)
* [SOA](http://www.infoq.com/cn/soa)
* [企业架构](http://www.infoq.com/cn/enterprise-architecture)
* [Intel](http://www.infoq.com/cn/intel)

相关内容
### [聊聊并发（六）——ConcurrentLinkedQueue的实现原理分析](http://www.infoq.com/cn/articles/ConcurrentLinkedQueue?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（四）——深入分析ConcurrentHashMap](http://www.infoq.com/cn/articles/ConcurrentHashMap?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（三）——JAVA线程池的分析和使用](http://www.infoq.com/cn/articles/java-threadPool?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（一）——深入分析Volatile的实现原理](http://www.infoq.com/cn/articles/ftf-java-volatile?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [Intel发布JavaScript扩展以支持并行运算](http://www.infoq.com/cn/news/2012/02/javascript-parallel-processing?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)
## 您好，陌生人！

您需要 [注册一个InfoQ账号](http://www.infoq.com/reginit.action) 或者 [登录]() 才能进行评论。在您完成注册后还需要进行一些设置。
## 获得来自InfoQ的更多体验。[]()

允许的HTML标签: a,b,br,blockquote,i,li,pre,u,ul,p

当有人回复此评论时请E-mail通知我

社区评论 [Watch Thread]()

[**您好，有个地方不太明白** by 王 凯 Posted 30/11/2012 05:01]()
[**Re: 您好，有个地方不太明白** by 方 腾飞 Posted 30/11/2012 05:14]()

[**Re: 您好，有个地方不太明白** by 王 凯 Posted 30/11/2012 05:21]()
[**期待看到happens-before的深入解读** by freish freish Posted 05/12/2012 08:54]()

[**Re: 期待看到happens-before的深入解读** by 方 腾飞 Posted 05/12/2012 11:04]()
[**缓存锁定一处不太明白** by 陈 良柱 Posted 18/12/2012 08:20]()

[**Re: 缓存锁定一处不太明白** by 方 腾飞 Posted 27/12/2012 01:00]()
[**Re: 缓存锁定一处不太明白** by Zhao Yu Posted 27/12/2012 10:20]()

[**Re: 缓存锁定一处不太明白** by 方 腾飞 Posted 31/12/2012 10:28]()
[**Re: 缓存锁定一处不太明白** by 陈 良柱 Posted 31/12/2012 09:29]()

[**缓存锁定** by Zhao Yu Posted 25/12/2012 04:19]()
[**Re: 缓存锁定** by 方 腾飞 Posted 27/12/2012 00:59]()

[**关于总线锁的问题** by lee jw Posted 29/12/2012 12:54]()
[**Re: 关于总线锁的问题** by 方 腾飞 Posted 31/12/2012 10:30]()
[]()

**您好，有个地方不太明白** 30/11/2012 05:01 by 王 凯

safeCount()方法有些看不明白，直接让atomicI自增不可以吗，AtomicInteger本身不就是原子方式增加的吗，谢谢。

* [回复]()
* [回到顶部]()

[]()

**Re: 您好，有个地方不太明白** 30/11/2012 05:14 by 方 腾飞

atomicI.compareAndSet(i, ++i);本身是原子方式的增加，但是有可能会增加失败，所以需要不停atomicI.compareAndSet(i, ++i);

* [回复]()
* [回到顶部]()
[]()

**Re: 您好，有个地方不太明白** 30/11/2012 05:21 by 王 凯

哦，明白了，我看了下AtomicInteger的源码，incrementAndGet方法实现，呵呵

* [回复]()
* [回到顶部]()

[]()

**期待看到happens-before的深入解读** 05/12/2012 08:54 by freish freish

期待看到happens-before的深入解读

* [回复]()
* [回到顶部]()
[]()

**Re: 期待看到happens-before的深入解读** 05/12/2012 11:04 by 方 腾飞

好的，没问题，在JMM文章里我会深入解读下。

* [回复]()
* [回到顶部]()

[]()

**缓存锁定一处不太明白** 18/12/2012 08:20 by 陈 良柱

缓存失效是在内存写回之前还是之后？如何使得其他cpu的缓存失效？是说内存到缓存的映射是一共享区域且回写内存包含对其的检查吗？

* [回复]()
* [回到顶部]()
[]()

**缓存锁定** 25/12/2012 04:19 by Zhao Yu

“所谓“缓存锁定”就是如果缓存在处理器缓存行中内存区域在LOCK操作期间被锁定，当它执行锁操作回写内存时，处理器不在总线上声言LOCK＃信号，而是修改内部的内存地址，并允许它的缓存一致性机制来保证操作的原子性”
这句实在不理解，首先什么是Lock操作期间？不是不锁总线了嘛？为什么还Lock？其次，处理器不发Lock信号而是修改内部的内存地址，到底是怎么保障在一个处理器修改缓存并回写期间，其它处理器不会执行同样操作？？？望作者解答一下

* [回复]()
* [回到顶部]()

[]()

**Re: 缓存锁定** 27/12/2012 00:59 by 方 腾飞

Lock操作期间，是指Lock指令执行期间，不锁总线但是锁缓存。
最后一个问题，这个是由缓存一致性协议保证的，详细参见：[www.tektalk.org/2011/07/07/cache一致性与2种基本写策略1/](http://www.tektalk.org/2011/07/07/cache一致性与2种基本写策略1/)

* [回复]()
* [回到顶部]()
[]()

**Re: 缓存锁定一处不太明白** 27/12/2012 01:00 by 方 腾飞

参见缓存一致性协议：www.tektalk.org/2011/07/07/cache一致性与2种基本写策略1/

* [回复]()
* [回到顶部]()

[]()

**Re: 缓存锁定一处不太明白** 27/12/2012 10:20 by Zhao Yu

好的～多谢作者的答复，这块内容关注很久了，多线程并发开发程序，那门语言和平台都是学到最后的最为重要，也是最深的地方，希望您继续发表相关博文～

* [回复]()
* [回到顶部]()
[]()

**关于总线锁的问题** 29/12/2012 12:54 by lee jw

您好，我想请问CPU在什么情况下会发出LOCK#信号，是在冲突的时候发出，还是在读写内存的时候发出？看您的文章是说CPU1发出LOCK#信号后，CPU2就不能读写内存，我想请问一下这里面是否有优先级的判断？谢谢

* [回复]()
* [回到顶部]()

[]()

**Re: 缓存锁定一处不太明白** 31/12/2012 10:28 by 方 腾飞

多谢你的关注，定期会发表新文章的。或者你也可以关注我的小站[ifeve.com](http://ifeve.com/) 最近我在做国外并发编程文章的翻译，都是非常值得一看的文章，你也可以参与到我们的翻译当中来，一起促进并发编程的研究和推广 。

* [回复]()
* [回到顶部]()
[]()

**Re: 关于总线锁的问题** 31/12/2012 10:30 by 方 腾飞

指令触发的，比如使用lock前缀的指令。

* [回复]()
* [回到顶部]()

[]()

**Re: 缓存锁定一处不太明白** 31/12/2012 09:29 by 陈 良柱

你给的文章链接没有回答我的问题，不过write-invalidate和write-update两词很有用处。
找到一篇个人认为写得较为清楚的综述文章，在此分享：[www.docin.com/p-92508695.html。](http://www.docin.com/p-92508695.html。)

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
[Close]() E-mail  密码

[使用Google账号登录](http://www.infoq.com/cn/social/googleLogin.action?fl=login "使用Google账号登录") [使用Microsoft账号登录](http://www.infoq.com/cn/social/liveLogin.action?fl=login "使用Microsoft账号登录")
[忘记密码？]()
 InfoQ账号使用的E-mail 发送邮件

[重新登录]()
重新发送激活信息 重新发送

[重新登录]()
[没有用户名？](http://www.infoq.com/cn/reginit.action)

[点击注册](http://www.infoq.com/cn/reginit.action)
