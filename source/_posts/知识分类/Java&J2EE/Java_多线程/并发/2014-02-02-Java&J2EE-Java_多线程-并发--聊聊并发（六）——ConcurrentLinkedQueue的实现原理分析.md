---
layout: post
title: "聊聊并发（六）——ConcurrentLinkedQueue的实现原理分析"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_多线程
 - 并发
--- 

# 聊聊并发（六）——ConcurrentLinkedQueue的实现原理分析

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

# 聊聊并发（六）——ConcurrentLinkedQueue的实现原理分析

作者 [方腾飞](http://www.infoq.com/cn/author/%E6%96%B9%E8%85%BE%E9%A3%9E) 发布于 一月 09, 2013 *|* [5 评论]()

## 1. 引言

在并发编程中我们有时候需要使用线程安全的队列。如果我们要实现一个线程安全的队列有两种实现方式：一种是使用阻塞算法，另一种是使用非阻塞算法。使用阻塞算法的队列可以用一个锁（入队和出队用同一把锁）或两个锁（入队和出队用不同的锁）等方式来实现，而非阻塞的实现方式则可以使用循环CAS的方式来实现，本文让我们一起来研究下Doug Lea是如何使用非阻塞的方式来实现线程安全队列ConcurrentLinkedQueue的，相信从大师身上我们能学到不少并发编程的技巧。

## 2. ConcurrentLinkedQueue的介绍

ConcurrentLinkedQueue是一个基于链接节点的无界线程安全队列，它采用先进先出的规则对节点进行排序，当我们添加一个元素的时候，它会添加到队列的尾部，当我们获取一个元素时，它会返回队列头部的元素。它采用了“wait－free”算法来实现，该算法在Michael & Scott算法上进行了一些修改, Michael & Scott算法的详细信息可以参见[参考资料一](http://www.cs.rochester.edu/u/michael/PODC96.html)。

## 3. ConcurrentLinkedQueue的结构

我们通过ConcurrentLinkedQueue的类图来分析一下它的结构。

![]()

（图1）

ConcurrentLinkedQueue由head节点和tair节点组成，每个节点（Node）由节点元素（item）和指向下一个节点的引用(next)组成，节点与节点之间就是通过这个next关联起来，从而组成一张链表结构的队列。默认情况下head节点存储的元素为空，tair节点等于head节点。
private transient volatile Node tail = head;

## 4. 入队列

**入队列就是将入队节点添加到队列的尾部**。为了方便理解入队时队列的变化，以及head节点和tair节点的变化，每添加一个节点我就做了一个队列的快照图。

![]()

（图二）

* 第一步添加元素1。队列更新head节点的next节点为元素1节点。又因为tail节点默认情况下等于head节点，所以它们的next节点都指向元素1节点。
* 第二步添加元素2。队列首先设置元素1节点的next节点为元素2节点，然后更新tail节点指向元素2节点。
* 第三步添加元素3，设置tail节点的next节点为元素3节点。
* 第四步添加元素4，设置元素3的next节点为元素4节点，然后将tail节点指向元素4节点。

通过debug入队过程并观察head节点和tail节点的变化，发现入队主要做两件事情，第一是将入队节点设置成当前队列尾节点的下一个节点。第二是更新tail节点，如果tail节点的next节点不为空，则将入队节点设置成tail节点，如果tail节点的next节点为空，则将入队节点设置成tail的next节点，所以tail节点不总是尾节点，理解这一点对于我们研究源码会非常有帮助。

上面的分析让我们从单线程入队的角度来理解入队过程，但是多个线程同时进行入队情况就变得更加复杂，因为可能会出现其他线程插队的情况。如果有一个线程正在入队，那么它必须先获取尾节点，然后设置尾节点的下一个节点为入队节点，但这时可能有另外一个线程插队了，那么队列的尾节点就会发生变化，这时当前线程要暂停入队操作，然后重新获取尾节点。让我们再通过源码来详细分析下它是如何使用CAS算法来入队的。
public boolean offer(E e) { if (e == null) throw new NullPointerException(); //入队前，创建一个入队节点 Node<E> n = new Node<E>(e); retry: //死循环，入队不成功反复入队。 for (;;) { //创建一个指向tail节点的引用 Node<E> t = tail; //p用来表示队列的尾节点，默认情况下等于tail节点。 Node<E> p = t; for (int hops = 0; ; hops++) { //获得p节点的下一个节点。 Node<E> next = succ(p); //next节点不为空，说明p不是尾节点，需要更新p后在将它指向next节点 if (next != null) { //循环了两次及其以上，并且当前节点还是不等于尾节点 if (hops > HOPS && t != tail) continue retry; p = next; } //如果p是尾节点，则设置p节点的next节点为入队节点。 else if (p.casNext(null, n)) { //如果tail节点有大于等于1个next节点，则将入队节点设置成tair节点，更新失败了也 没关系，因为失败了表示有其他线程成功更新了tair节点。 if (hops >= HOPS) casTail(t, n); // 更新tail节点，允许失败 return true; } // p有next节点,表示p的next节点是尾节点，则重新设置p节点 else { p = succ(p); } } } }

**从源代码角度来看整个入队过程主要做二件事情**。第一是定位出尾节点，第二是使用CAS算法能将入队节点设置成尾节点的next节点，如不成功则重试。

**第一步定位尾节点**。tail节点并不总是尾节点，所以每次入队都必须先通过tail节点来找到尾节点，尾节点可能就是tail节点，也可能是tail节点的next节点。代码中循环体中的第一个if就是判断tail是否有next节点，有则表示next节点可能是尾节点。获取tail节点的next节点需要注意的是p节点等于p的next节点的情况，只有一种可能就是p节点和p的next节点都等于空，表示这个队列刚初始化，正准备添加第一次节点，所以需要返回head节点。获取p节点的next节点代码如下
final Node<E> succ(Node<E> p) { Node<E> next = p.getNext(); return (p == next) ? head : next; }

**第二步设置入队节点为尾节点**。p.casNext(null, n)方法用于将入队节点设置为当前队列尾节点的next节点，p如果是null表示p是当前队列的尾节点，如果不为null表示有其他线程更新了尾节点，则需要重新获取当前队列的尾节点。

**hops的设计意图**。上面分析过对于先进先出的队列入队所要做的事情就是将入队节点设置成尾节点，doug lea写的代码和逻辑还是稍微有点复杂。那么我用以下方式来实现行不行？
public boolean offer(E e) { if (e == null) throw new NullPointerException(); Node<E> n = new Node<E>(e); for (;;) { Node<E> t = tail; if (t.casNext(null, n) && casTail(t, n)) { return true; } } }

让tail节点永远作为队列的尾节点，这样实现代码量非常少，而且逻辑非常清楚和易懂。但是这么做有个缺点就是每次都需要使用循环CAS更新tail节点。如果能减少CAS更新tail节点的次数，就能提高入队的效率，所以doug lea使用hops变量来控制并减少tail节点的更新频率，并不是每次节点入队后都将 tail节点更新成尾节点，而是当 tail节点和尾节点的距离大于等于常量HOPS的值（默认等于1）时才更新tail节点，tail和尾节点的距离越长使用CAS更新tail节点的次数就会越少，但是距离越长带来的负面效果就是每次入队时定位尾节点的时间就越长，因为循环体需要多循环一次来定位出尾节点，但是这样仍然能提高入队的效率，因为从本质上来看它通过增加对volatile变量的读操作来减少了对volatile变量的写操作，而对volatile变量的写操作开销要远远大于读操作，所以入队效率会有所提升。

private static final int HOPS = 1;

还有一点需要注意的是入队方法永远返回true，所以不要通过返回值判断入队是否成功。

## 5. 出队列

出队列的就是从队列里返回一个节点元素，并清空该节点对元素的引用。让我们通过每个节点出队的快照来观察下head节点的变化。

![]()

从上图可知，并不是每次出队时都更新head节点，当head节点里有元素时，直接弹出head节点里的元素，而不会更新head节点。只有当head节点里没有元素时，出队操作才会更新head节点。这种做法也是通过hops变量来减少使用CAS更新head节点的消耗，从而提高出队效率。让我们再通过源码来深入分析下出队过程。
public E poll() { Node<E> h = head; // p表示头节点，需要出队的节点 Node<E> p = h; for (int hops = 0;; hops++) { // 获取p节点的元素 E item = p.getItem(); // 如果p节点的元素不为空，使用CAS设置p节点引用的元素为null,如果成功则返回p节点的元素。 if (item != null && p.casItem(item, null)) { if (hops >= HOPS) { //将p节点下一个节点设置成head节点 Node<E> q = p.getNext(); updateHead(h, (q != null) ? q : p); } return item; } // 如果头节点的元素为空或头节点发生了变化，这说明头节点已经被另外一个线程修改了。那么获取p节点的下一个节点 Node<> next = succ(p); // 如果p的下一个节点也为空，说明这个队列已经空了 if (next == null) { // 更新头节点。 updateHead(h, p); break; } // 如果下一个元素不为空，则将头节点的下一个节点设置成头节点 p = next; } return null; }

首先获取头节点的元素，然后判断头节点元素是否为空，如果为空，表示另外一个线程已经进行了一次出队操作将该节点的元素取走，如果不为空，则使用CAS的方式将头节点的引用设置成null，如果CAS成功，则直接返回头节点的元素，如果不成功，表示另外一个线程已经进行了一次出队操作更新了head节点，导致元素发生了变化，需要重新获取头节点。

## 6. 参考资料

* [简单，快速和实用的阻塞和非阻塞并发队列算法](http://www.cs.rochester.edu/u/scott/papers/1996_PODC_queues.pdf)。
* [非阻塞算法在容器里的实现](http://www.ibm.com/developerworks/cn/java/j-lo-concurrent/index.html)。
* JDK1.6中ConcurrentLinkedQueue源码和注释。

##

## 作者介绍

**方腾飞**，花名清英，淘宝资深开发工程师，关注并发编程，目前在广告技术部从事无线广告联盟的开发和设计工作。个人博客：[http://ifeve.com](http://ifeve.com/) 微博：[http://weibo.com/kirals](http://weibo.com/kirals)欢迎通过我的微博进行技术交流。

感谢[张龙](http://www.infoq.com/cn/bycategory.action?authorName=张龙)对本文的审校。

给InfoQ中文站投稿或者参与内容翻译工作，请邮件至[editors@cn.infoq.com](mailto:editors@cn.infoq.com)。也欢迎大家通过新浪微博（[@InfoQ](http://www.weibo.com/infoqchina)）或者腾讯微博（[@InfoQ](http://t.qq.com/infoqchina)）关注我们，并与我们的编辑和其他读者朋友交流。
* [Sections]()
* [**语言 & 开发**](http://www.infoq.com/cn/development)
* [Topics]()
* [多线程](http://www.infoq.com/cn/Multi-threading)
* [并发](http://www.infoq.com/cn/concurrency)
* [Java](http://www.infoq.com/cn/java)

相关内容
### [聊聊并发（五）——原子操作的实现原理](http://www.infoq.com/cn/articles/atomic-operation?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（四）——深入分析ConcurrentHashMap](http://www.infoq.com/cn/articles/ConcurrentHashMap?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（三）——JAVA线程池的分析和使用](http://www.infoq.com/cn/articles/java-threadPool?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（一）——深入分析Volatile的实现原理](http://www.infoq.com/cn/articles/ftf-java-volatile?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（七）——总结](http://www.infoq.com/cn/articles/java-memory-model-7?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（七）——总结](http://www.infoq.com/cn/articles/java-memory-model-7?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（六）——final](http://www.infoq.com/cn/articles/java-memory-model-6?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（五）——锁](http://www.infoq.com/cn/articles/java-memory-model-5?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（五）——原子操作的实现原理](http://www.infoq.com/cn/articles/atomic-operation?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [聊聊并发（四）——深入分析ConcurrentHashMap](http://www.infoq.com/cn/articles/ConcurrentHashMap?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)
## 您好，陌生人！

您需要 [注册一个InfoQ账号](http://www.infoq.com/reginit.action) 或者 [登录]() 才能进行评论。在您完成注册后还需要进行一些设置。
## 获得来自InfoQ的更多体验。[]()

允许的HTML标签: a,b,br,blockquote,i,li,pre,u,ul,p

当有人回复此评论时请E-mail通知我

社区评论 [Watch Thread]()

[**CAS 描述有错** by lin lirs Posted 10/01/2013 05:35]()
[**Re: CAS 描述有错** by 方 腾飞 Posted 11/01/2013 03:10]()

[**请教大师，有个地方不是很明白** by huang shuihua Posted 18/01/2013 02:58]()
[**Re: 请教大师，有个地方不是很明白** by 方 腾飞 Posted 18/01/2013 03:40]()

[**看着不过瘾，买了本书** by 杨 亮 Posted 04/03/2013 01:42]()
[]()

**CAS 描述有错** 10/01/2013 05:35 by lin lirs

“它采用了“wait－free”算法（即CAS算法）来实现”， CAS不是指算法，而是一个原子操作，wait-free不等同CAS。
[en.wikipedia.org/wiki/Compare-and-swap](http://en.wikipedia.org/wiki/Compare-and-swap)

* [回复]()
* [回到顶部]()

[]()

**Re: CAS 描述有错** 11/01/2013 03:10 by 方 腾飞

的确！CAS是一个原子操作指令，可以用来实现“wait－free”算法。这里用“既CAS算法”的确不太合适，我加上的初衷是希望读者能很好的理解“wait－free”算法，但这样会产生一些歧义，所以还是删掉比较合适，感谢您的纠正。

* [回复]()
* [回到顶部]()
[]()

**请教大师，有个地方不是很明白** 18/01/2013 02:58 by huang shuihua

(如果tail节点的next节点不为空，则将入队节点设置成tail节点，如果tail节点的next节点为空，则将入队节点设置成tail的next节点，所以tail节点不总是尾节点),这个还是不是很明白，能否有更加清晰一点的原理图show一下，谢谢!

* [回复]()
* [回到顶部]()

[]()

**Re: 请教大师，有个地方不是很明白** 18/01/2013 03:40 by 方 腾飞

tail节点并总是尾节点。入队的时候，入队节点要放在队列的尾部，那首先要定位尾节点是哪个节点？所以这里通过tail节点来定位尾节点。尾节点要么是tail节点，要么是tail的next节点。所以入队会有两种情况
情况1：tail节点的next节点不为空，那么插入后队列变成
旧的tail节点->旧tail节点的next节点->入队节点（新tail节点）
情况2：tail节点的next节点为空，那么插入后队列变成
tail节点->入队节点

* [回复]()
* [回到顶部]()
[]()

**看着不过瘾，买了本书** 04/03/2013 01:42 by 杨 亮

聊聊并发这个系列都看了，结合内存模型系列，但还是看着不过瘾，买了本书看。

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
