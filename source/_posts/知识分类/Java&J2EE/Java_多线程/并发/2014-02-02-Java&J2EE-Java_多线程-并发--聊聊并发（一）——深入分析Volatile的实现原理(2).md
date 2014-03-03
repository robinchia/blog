---
layout: post
title: "聊聊并发（一）——深入分析Volatile的实现原理 (2)"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_多线程
 - 并发
--- 

# 聊聊并发（一）——深入分析Volatile的实现原理 (2)

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

* 欢迎关注我们的：
* [![]()](http://e.weibo.com/infoqchina)
* [![]()](http://www.infoq.com/cn/news/2013/02/infoq-wechat)
* [![]()](http://www.infoq.com/cn/rss/rss.action?token=J6EBu2hcpyEuMs9msdo8GyNCZTcWG7pm)

# 聊聊并发（一）——深入分析Volatile的实现原理

作者 [方腾飞](http://www.infoq.com/cn/author/%E6%96%B9%E8%85%BE%E9%A3%9E) 发布于 二月 21, 2012 *|* [27 评论]()

* [新浪微博]( "分享到新浪微博") [腾讯微博]( "分享到腾讯微博") [豆瓣网]( "分享到豆瓣网") [Twitter]( "分享到Twitter") [Facebook]( "分享到Facebook") [linkedin]( "分享到linkedin") [邮件分享]( "分享到邮件分享") 更多 [21]( "累计分享21次")
* [稍后阅读]()
* [我的阅读清单](http://www.infoq.com/cn/showbookmarks.action)
## **引言**

在多线程并发编程中synchronized和Volatile都扮演着重要的角色，Volatile是**轻量级的synchronized**，它在多处理器开发中保证了共享变量的“可见性”。可见性的意思是当一个线程修改一个共享变量时，另外一个线程能读到这个修改的值。

它在某些情况下比synchronized的开销更小，本文将深入分析在硬件层面上Inter处理器是如何实现Volatile的，通过深入分析能帮助我们正确的使用Volatile变量。

## **术语定义**

术语
 
英文单词
 
描述 共享变量
   
在多个线程之间能够被共享的变量被称为共享变量。共享变量包括所有的实例变量，静态变量和数组元素。他们都被存放在堆内存中，Volatile只作用于共享变量。 内存屏障
 
Memory Barriers
 
是一组处理器指令，用于实现对内存操作的顺序限制。 缓冲行
 
Cache line
 
缓存中可以分配的最小存储单位。处理器填写缓存线时会加载整个缓存线，需要使用多个主内存读周期。 原子操作
 
Atomic operations
 
不可中断的一个或一系列操作。 缓存行填充
 
cache line fill
 
当处理器识别到从内存中读取操作数是可缓存的，处理器读取整个缓存行到适当的缓存（L1，L2，L3的或所有） 缓存命中
 
cache hit
 
如果进行高速缓存行填充操作的内存位置仍然是下次处理器访问的地址时，处理器从缓存中读取操作数，而不是从内存。 写命中
 
write hit
 
当处理器将操作数写回到一个内存缓存的区域时，它首先会检查这个缓存的内存地址是否在缓存行中，如果存在一个有效的缓存行，则处理器将这个操作数写回到缓存，而不是写回到内存，这个操作被称为写命中。 写缺失
 
write misses the cache
 
一个有效的缓存行被写入到不存在的内存区域。

## **Volatile的官方定义**

Java语言规范第三版中对volatile的定义如下： java编程语言允许线程访问共享变量，为了确保共享变量能被准确和一致的更新，线程应该确保通过排他锁单独获得这个变量。Java语言提供了volatile，在某些情况下比锁更加方便。如果一个字段被声明成volatile，java线程内存模型确保所有线程看到这个变量的值是一致的。

## **为什么要使用Volatile**

Volatile变量修饰符如果使用**恰当**的话，它比synchronized的**使用和执行成本会更低**，因为它不会引起线程上下文的切换和调度。

## **Volatile的实现原理**

那么Volatile是如何来保证可见性的呢？在x86处理器下通过工具获取JIT编译器生成的汇编指令来看看对Volatile进行写操作CPU会做什么事情。
Java代码：
 
instance = new Singleton();//instance是volatile变量 汇编代码：
 
0x01a3de1d: movb $0x0,0x1104800(%esi);

0x01a3de24: **lock** addl $0x0,(%esp);

有volatile变量修饰的共享变量进行写操作的时候会多第二行汇编代码，通过查IA-32架构软件开发者手册可知，lock前缀的指令在多核处理器下会引发了两件事情。

* 将当前处理器缓存行的数据会写回到系统内存。
* 这个写回内存的操作会引起在其他CPU里缓存了该内存地址的数据无效。

处理器为了提高处理速度，不直接和内存进行通讯，而是先将系统内存的数据读到内部缓存（L1,L2或其他）后再进行操作，但操作完之后不知道何时会写到内存，如果对声明了Volatile变量进行写操作，JVM就会向处理器发送一条Lock前缀的指令，将这个变量所在缓存行的数据写回到系统内存。但是就算写回到内存，如果其他处理器缓存的值还是旧的，再执行计算操作就会有问题，所以在多处理器下，为了保证各个处理器的缓存是一致的，就会实现缓存一致性协议，每个处理器通过嗅探在总线上传播的数据来检查自己缓存的值是不是过期了，当处理器发现自己缓存行对应的内存地址被修改，就会将当前处理器的缓存行设置成无效状态，当处理器要对这个数据进行修改操作的时候，会强制重新从系统内存里把数据读到处理器缓存里。

这两件事情在IA-32软件开发者架构手册的第三册的多处理器管理章节（第八章）中有详细阐述。

**Lock前缀指令会引起处理器缓存回写到内存**。Lock前缀指令导致在执行指令期间，声言处理器的 LOCK# 信号。在多处理器环境中，LOCK# 信号确保在声言该信号期间，处理器可以独占使用任何共享内存。（因为它会锁住总线，导致其他CPU不能访问总线，不能访问总线就意味着不能访问系统内存），但是在最近的处理器里，LOCK＃信号一般不锁总线，而是锁缓存，毕竟锁总线开销比较大。在8.1.4章节有详细说明锁定操作对处理器缓存的影响，对于Intel486和Pentium处理器，在锁操作时，总是在总线上声言LOCK#信号。但在P6和最近的处理器中，如果访问的内存区域已经缓存在处理器内部，则不会声言LOCK#信号。相反地，它会锁定这块内存区域的缓存并回写到内存，并使用缓存一致性机制来确保修改的原子性，此操作被称为“缓存锁定”，**缓存一致性机制会阻止同时修改被两个以上处理器缓存的内存区域数据**。

**一个处理器的缓存回写到内存会导致其他处理器的缓存无效**。IA-32处理器和Intel 64处理器使用MESI（修改，独占，共享，无效）控制协议去维护内部缓存和其他处理器缓存的一致性。在多核处理器系统中进行操作的时候，IA-32 和Intel 64处理器能嗅探其他处理器访问系统内存和它们的内部缓存。它们使用嗅探技术保证它的内部缓存，系统内存和其他处理器的缓存的数据在总线上保持一致。例如在Pentium和P6 family处理器中，如果通过嗅探一个处理器来检测其他处理器打算写内存地址，而这个地址当前处理共享状态，那么正在嗅探的处理器将无效它的缓存行，在下次访问相同内存地址时，强制执行缓存行填充。

## **Volatile的使用优化**

著名的Java并发编程大师Doug lea在JDK7的并发包里新增一个队列集合类LinkedTransferQueue，他在使用Volatile变量时，用一种追加字节的方式来优化队列出队和入队的性能。

追加字节能优化性能？这种方式看起来很神奇，但如果深入理解处理器架构就能理解其中的奥秘。让我们先来看看LinkedTransferQueue这个类，它使用一个内部类类型来定义队列的头队列（Head）和尾节点（tail），而这个内部类PaddedAtomicReference相对于父类AtomicReference只做了一件事情，就将共享变量追加到64字节。我们可以来计算下，一个对象的引用占4个字节，它追加了15个变量共占60个字节，再加上父类的Value变量，一共64个字节。
/** head of the queue */ private transient final PaddedAtomicReference < QNode > head; /** tail of the queue */ private transient final PaddedAtomicReference < QNode > tail; static final class PaddedAtomicReference < T > extends AtomicReference < T > { // enough padding for 64bytes with 4byte refs Object p0, p1, p2, p3, p4, p5, p6, p7, p8, p9, pa, pb, pc, pd, pe; PaddedAtomicReference(T r) { super(r); } } public class AtomicReference < V > implements java.io.Serializable { private volatile V value; //省略其他代码 ｝

**为什么追加64字节能够提高并发编程的效率呢**？ 因为对于英特尔酷睿i7，酷睿， Atom和NetBurst， Core Solo和Pentium M处理器的L1，L2或L3缓存的高速缓存行是64个字节宽，不支持部分填充缓存行，这意味着如果队列的头节点和尾节点都不足64字节的话，处理器会将它们都读到同一个高速缓存行中，在多处理器下每个处理器都会缓存同样的头尾节点，当一个处理器试图修改头接点时会将整个缓存行锁定，那么在缓存一致性机制的作用下，会导致其他处理器不能访问自己高速缓存中的尾节点，而队列的入队和出队操作是需要不停修改头接点和尾节点，所以在多处理器的情况下将会严重影响到队列的入队和出队效率。Doug lea使用追加到64字节的方式来填满高速缓冲区的缓存行，避免头接点和尾节点加载到同一个缓存行，使得头尾节点在修改时不会互相锁定。

那么是不是在使用Volatile变量时都应该追加到64字节呢？不是的。在两种场景下不应该使用这种方式。第一：**缓存行非64字节宽的处理器**，如P6系列和奔腾处理器，它们的L1和L2高速缓存行是32个字节宽。第二：**共享变量不会被频繁的写**。因为使用追加字节的方式需要处理器读取更多的字节到高速缓冲区，这本身就会带来一定的性能消耗，共享变量如果不被频繁写的话，锁的几率也非常小，就没必要通过追加字节的方式来避免相互锁定。

## **参考资料**

* [JVM执行篇：使用HSDIS插件分析JVM代码执行细节](http://www.infoq.com/cn/articles/zzm-java-hsdis-jvm)
* [内存屏障和并发](http://www.infoq.com/cn/articles/memory_barriers_jvm_concurrency)
* [Intel 64和IA-32架构软件开发人员手册](http://www.intel.com/products/processor/manuals/)

## 关于作者

方腾飞，阿里巴巴资深软件开发工程师，致力于高性能网络编程，目前在公司从事询盘管理和长连接服务器OpenComet的开发工作。博客地址：[http://ifeve.com](http://ifeve.com/)

感谢[郑柯](http://www.infoq.com/cn/bycategory.action?authorName=郑柯)对本文的审校。

给InfoQ中文站投稿或者参与内容翻译工作，请邮件至[editors@cn.infoq.com](mailto:editors@cn.infoq.com)。也欢迎大家通过新浪微博（[@InfoQ](http://www.weibo.com/infoqchina)）或者腾讯微博（[@InfoQ](http://t.qq.com/infoqchina)）关注我们，并与我们的编辑和其他读者朋友交流。
* [Sections]()
* [**语言 & 开发**](http://www.infoq.com/cn/development)
* [Topics]()
* [多线程](http://www.infoq.com/cn/Multi-threading)
* [并发](http://www.infoq.com/cn/concurrency)
* [Java](http://www.infoq.com/cn/java)

相关内容
### [聊聊并发（六）——ConcurrentLinkedQueue的实现原理分析](http://www.infoq.com/cn/articles/ConcurrentLinkedQueue?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（五）——原子操作的实现原理](http://www.infoq.com/cn/articles/atomic-operation?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（四）——深入分析ConcurrentHashMap](http://www.infoq.com/cn/articles/ConcurrentHashMap?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（三）——JAVA线程池的分析和使用](http://www.infoq.com/cn/articles/java-threadPool?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（七）——总结](http://www.infoq.com/cn/articles/java-memory-model-7?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（七）——总结](http://www.infoq.com/cn/articles/java-memory-model-7?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（六）——final](http://www.infoq.com/cn/articles/java-memory-model-6?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（五）——锁](http://www.infoq.com/cn/articles/java-memory-model-5?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（六）——ConcurrentLinkedQueue的实现原理分析](http://www.infoq.com/cn/articles/ConcurrentLinkedQueue?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（五）——原子操作的实现原理](http://www.infoq.com/cn/articles/atomic-operation?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)
## 您好，陌生人！

您需要 [注册一个InfoQ账号](http://www.infoq.com/reginit.action) 或者 [登录]() 才能进行评论。在您完成注册后还需要进行一些设置。
## 获得来自InfoQ的更多体验。[]()

允许的HTML标签: a,b,br,blockquote,i,li,pre,u,ul,p

当有人回复此评论时请E-mail通知我

社区评论 [Watch Thread]()

[**fantastic** by chuanmin zuo Posted 22/02/2012 01:19]()
[**Re: fantastic** by chuanmin zuo Posted 22/02/2012 01:21]()

[**Re: fantastic** by 方 腾飞 Posted 22/02/2012 01:55]()
[**I have a question to ask you** by chuanmin zuo Posted 22/02/2012 07:23]()

[**Re: I have a question to ask you** by chuanmin zuo Posted 22/02/2012 07:24]()
[**疑问** by Wang Frank Posted 24/02/2012 01:12]()

[**Re: 疑问** by 方 腾飞 Posted 26/02/2012 10:35]()
[**Re: 两个不同的线程看到某个成员变量的值是不相同的，怎么写个例子** by zhang qinghua Posted 08/05/2013 11:43]()

[**Re: 疑问** by 史 墨轩 Posted 19/12/2012 10:06]()
[**Re: 疑问** by 方 腾飞 Posted 27/12/2012 01:07]()

[**Re: 疑问** by 方 腾飞 Posted 31/12/2012 10:33]()
[**so wonderful** by hu depin Posted 24/02/2012 02:56]()

[**Re: so wonderful** by 方 腾飞 Posted 26/02/2012 10:41]()
[**汇编代码的角度去考虑** by Liu Xia Posted 28/02/2012 00:09]()

[**如何在实际的项目中应用** by www www Posted 28/02/2012 09:18]()
[**Re: 如何在实际的项目中应用** by 方 腾飞 Posted 28/02/2012 07:08]()

[**为什么volatile 变量不能保证写操作的原子性** by Haixia Chen Posted 11/08/2012 10:09]()
[**Re: 为什么volatile 变量不能保证写操作的原子性** by Haixia Chen Posted 11/08/2012 11:01]()

[**Re: 为什么volatile 变量不能保证写操作的原子性** by 方 腾飞 Posted 02/09/2012 07:36]()
[**好啊** by s zh Posted 31/12/2012 05:08]()

[**疑问** by zhao chiva Posted 17/02/2013 03:34]()
[**改字** by 吴 杰峰 Posted 07/03/2013 08:07]()

[**一个有效的缓存行被写入到不存在的内存区域** by 高 海军 Posted 02/04/2013 10:19]()
[**java中对象或者数组用volatile修饰有什么用?** by sc lv Posted 03/04/2013 03:00]()

[**请教一个悖论问题** by sun shanghai Posted 26/04/2013 10:43]()
[**Re: 请教一个悖论问题** by zhang qinghua Posted 08/05/2013 11:44]()

[**文章导出** by tony zarric Posted 12/05/2013 11:05]()
[]()

**fantastic** 22/02/2012 01:19 by chuanmin zuo

Thanks for your sharing,I think it's pretty wonderful technical article.
It's very useful for java developers to understand volatile.
But I also want to know the performance difference between volatile and synchronized,don't misunderstand,I mean precise,or to prove performance difference with some data.If you can add this,that's perfect,coz we pay close attention to that.Maybe that is the key point of your article.
thanks again.

* [回复]()
* [回到顶部]()

[]()

**Re: fantastic** 22/02/2012 01:21 by chuanmin zuo

Sorry,Maybe that is the key point of your article.-->Maybe that is not the key point of your article

* [回复]()
* [回到顶部]()
[]()

**Re: fantastic** 22/02/2012 01:55 by 方 腾飞

Thank you for your attention! i will add it to another article.

* [回复]()
* [回到顶部]()

[]()

**I have a question to ask you** 22/02/2012 07:23 by chuanmin zuo

After reading your article,I made a test like this:
I downloaded java language specification,and test a example from 8.3.1.4 volatile field in this document.
But I found even if a field(i and j in that example) is declared volatile,the value of j is still greater than that for i.Actually I know why,coz volatile just ensure other thread can see the latest value ,not ensure synchronization.But that document(I mean java langguage specification) tells me:"Therefore,the shared value for j is never greater than that of j",I can not understand,Have you tested that example.
I look forward to your reply.

* [回复]()
* [回到顶部]()
[]()

**Re: I have a question to ask you** 22/02/2012 07:24 by chuanmin zuo

"Therefore,the shared value for j is never greater than that of i",sorry,my mistake.

* [回复]()
* [回到顶部]()

[]()

**疑问** 24/02/2012 01:12 by Wang Frank

当处理器将操作数写回到一个内存缓存的区域时，它首先会检查这个缓存的内存地址是否在缓存行中，如果不存在一个有效的缓存行，则处理器将这个操作数写回到缓存，而不是写回到内存，这个操作被称为写命中。
“如果不存在一个有效的缓存行” 这里是否应该是“如果存在一个有效的缓存行”？

* [回复]()
* [回到顶部]()
[]()

**so wonderful** 24/02/2012 02:56 by hu depin

受益匪浅，感谢分享

* [回复]()
* [回到顶部]()

[]()

**Re: 疑问** 26/02/2012 10:35 by 方 腾飞

是的，感谢纠正。

* [回复]()
* [回到顶部]()
[]()

**Re: so wonderful** 26/02/2012 10:41 by 方 腾飞

谢谢关注。

* [回复]()
* [回到顶部]()

[]()

**汇编代码的角度去考虑** 28/02/2012 00:09 by Liu Xia

Java 语言实际上是有自己的内存模型的。这种内存模型实际上就是希望隐藏各种不同的处理器对于缓存一致性的处理。volatile 在 Java 的内存模型还真是没有太关注，在 CLR 的内存模型上是 volatile 的读具有 load acuqire 语义（这和一般的读不同），从而防止了一定程度上的代码调整，而保证在 load 完成后的一致性。
但是从汇编上那就复杂了，例如 IA-32 或者 Intel 64 架构的处理器上使用了 lock 或者，但是在 IA-64 的处理器上就需要一个 load acquire 的 inter-lock 操作（IA-64 上的这种操作指令还挺丰富）。

* [回复]()
* [回到顶部]()
[]()

**如何在实际的项目中应用** 28/02/2012 09:18 by www www

在单位的电脑上跑了一下 确实快很多。也确实证明了他的可用性。但问题是我们如何在实际的项目中应用这个东西。 或者说如何在现行的项目中确定false sharing产生的瓶颈，然后用这种方法来提高性能？

* [回复]()
* [回到顶部]()

[]()

**Re: 如何在实际的项目中应用** 28/02/2012 07:08 by 方 腾飞

文中有提到，应用场景是首先要确认该共享变量是否会被频繁的写？比如队列的入队和出队，需要不停的修改头节点和尾节点，所以这里建议使用Padding的方式进行优化，当然即使是频繁的写也不一定会成为性能的瓶颈。

* [回复]()
* [回到顶部]()
[]()

**为什么volatile 变量不能保证写操作的原子性** 11/08/2012 10:09 by Haixia Chen

大师，文章中提到volatile变量的写操作，处理器写完会更新到内存，其他处理器缓存会失效，既然这样，为什么多线程i++，100次的时候，最后i的值可能<100呢，i++的分析见：[wk.baidu.com/view/bc890df5f61fb7360b4c654b&...](http://wk.baidu.com/view/bc890df5f61fb7360b4c654b&?pcf=2)
Pad输入不容易，盼大师解惑

* [回复]()
* [回到顶部]()

[]()

**Re: 为什么volatile 变量不能保证写操作的原子性** 11/08/2012 11:01 by Haixia Chen

明白了，那篇文章已经讲的很清楚了

* [回复]()
* [回到顶部]()
[]()

**Re: 为什么volatile 变量不能保证写操作的原子性** 02/09/2012 07:36 by 方 腾飞

：）

* [回复]()
* [回到顶部]()

[]()

**Re: 疑问** 19/12/2012 10:06 by 史 墨轩

我刚看到的时候也有这个疑问，看了回复果然是翻译问题，希望作者做一下修改吧

* [回复]()
* [回到顶部]()
[]()

**Re: 疑问** 27/12/2012 01:07 by 方 腾飞

好的

* [回复]()
* [回到顶部]()

[]()

**Re: 疑问** 31/12/2012 10:33 by 方 腾飞

已经修改！

* [回复]()
* [回到顶部]()
[]()

**好啊** 31/12/2012 05:08 by s zh

感谢你的分享

* [回复]()
* [回到顶部]()

[]()

**疑问** 17/02/2013 03:34 by zhao chiva

JKD7里面LinkedTransferQueue的实现跟你说的不一样，head跟tail的内部实现类是Node.也不是采用移位实现的。
/** head of the queue; null until first enqueue */
transient volatile Node head;
/** tail of the queue; null until first append */
private transient volatile Node tail;

* [回复]()
* [回到顶部]()
[]()

**改字** 07/03/2013 08:07 by 吴 杰峰

例如在Pentium和P6 family处理器中，如果通过嗅探一个处理器来检测其他处理器打算写内存地址，而这个地址当前处理共享状态 "而这个地址当前处理共享状态"：应该是“而这个地址当前处于共享状态”

* [回复]()
* [回到顶部]()

[]()

**一个有效的缓存行被写入到不存在的内存区域** 02/04/2013 10:19 by 高 海军

write misses the cache 表示"一个有效的缓存行被写入到不存在的内存区域。" 这段话是是不是有点问题? 应该是写缓存未命中巴? 可以再解释一下吗?

* [回复]()
* [回到顶部]()
[]()

**java中对象或者数组用volatile修饰有什么用?** 03/04/2013 03:00 by sc lv

上文说到共享变量包括所有的实例变量，静态变量和数组元素，有看到其它资料说对于数组，volatile修饰的只是数组的引用，例如，java.io.BufferedInputStream类中protected volatile byte buf[]; 数组buf用volatile修饰；
java.io.FilterInputStream类中protected volatile InputStream in; in用volatile修饰。
jdk中这样修饰有什么好处呢？这两个地方如果不用volatile修饰会有什么影响？

* [回复]()
* [回到顶部]()

[]()

**请教一个悖论问题** 26/04/2013 10:43 by sun shanghai

import java.lang.*;
/**
* 我觉得如果不加volatile修饰符，则第1个线程是永远不会停止的，
* 但是实际上线程2把flag设置为true后，线程1就停止了
* 这个是什么原因呢？不明白。
*/
public class Counter {
//
private static Boolean flag = new Boolean(false);
// private volatile static Boolean flag = new Boolean(false);
public static void main(String[] args) {
// 线程1
new Thread() {
int i = 0;
public void run() {
while (!flag.booleanValue()) {
System.out.println(i++);
}
}
}.start();
// 线程2
new Thread() {
public void run() {
try {
Thread.sleep(1000);
flag = new Boolean(true);
} catch (InterruptedException e) {
e.printStackTrace();
}
}
}.start();
}
}

* [回复]()
* [回到顶部]()
[]()

**Re: 两个不同的线程看到某个成员变量的值是不相同的，怎么写个例子** 08/05/2013 11:43 by zhang qinghua

private int i = 0;
//a线程调用
public void foo1(){
try {
while (true) {
Thread.sleep(10);
i++;
}
} catch (InterruptedException e) {
//not to do;
}
}
//b线程调用
public void foo2(){
try {
while(true){
Thread.sleep(1000);
System.out.println("第二个："+i);
}
} catch (InterruptedException e) {
//not to do;
}
}
为什么foo2打印的i的值会随着foo1修改的值变化。。。。。。
请见凉，自从看了这篇文章，感觉以前写的代码都有线程缓存共享变量的问题，但实际没有出现过 两个不同的线程看到某个成员变量的值是不相同的。
请lz,给个例子，
两个不同的线程（通过线程缓存共享变量）看到某个成员变量的值是不相同的，怎么写个例子？
请楼主回答一下。

* [回复]()
* [回到顶部]()

[]()

**Re: 请教一个悖论问题** 08/05/2013 11:44 by zhang qinghua

你的困惑解决没，我的困惑和你一样。望回答一下，在你的楼下。

* [回复]()
* [回到顶部]()
[]()

**文章导出** 12/05/2013 11:05 by tony zarric

页面内容能提供导出功能，如：
聊聊并发（一）——深入分析Volatile的实现原理 能导出 保存为pdf或word文件

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
![]( "定义(粉红)")![]( "假设(蓝色)")![]( "分析(黄色)")![]( "结论(橘红)")![]( "优势(绿色)")![]( "缺陷(紫色)")![]( "注意(红色)")![]( "清除背景色")![]( "书签")![]( "设为目录项")![]( "在Google中搜索")![]( "查找解释")![]( "标注")![]( "链接到所选文件夹/标签/样式/文件")![]( "Wiz助手")![]( "稍后阅读(tag)")
