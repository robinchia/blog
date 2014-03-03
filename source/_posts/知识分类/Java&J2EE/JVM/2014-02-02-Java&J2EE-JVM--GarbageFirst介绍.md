---
layout: post
title: "Garbage First介绍"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# Garbage First介绍

# [BlueDavy之技术Blog](http://www.blogjava.net/BlueDavy/)

理论不懂就实践，实践不会就学理论！

## [Garbage First介绍](http://www.blogjava.net/BlueDavy/archive/2009/03/11/259230.html)

本文摘自《构建高性能的大型分布式Java应用》一书，Garbage First简称G1，它的目标是要做到尽量减少GC所导致的应用暂停的时间，让应用达到准实时的效果，同时保持JVM堆空间的利用率，将作为CMS的替代者在JDK 7中闪亮登场，其最大的特色在于允许指定在某个时间段内GC所导致的应用暂停的时间最大为多少，例如在100秒内最多允许GC导致的应用暂停时间为1秒，这个特性对于准实时响应的系统而言非常的吸引人，这样就再也不用担心系统突然会暂停个两三秒了。

G1要做到这样的效果，也是有前提的，一方面是硬件环境的要求，必须是多核的CPU以及较大的内存（从规范来看，512M以上就满足条件了），另外一方面是需要接受吞吐量的稍微降低，对于实时性要求高的系统而言，这点应该是可以接受的。

为了能够达到这样的效果，G1在原有的各种GC策略上进行了吸收和改进，在G1中可以看到增量收集器和CMS的影子，但它不仅仅是吸收原有GC策略的优点，并在此基础上做出了很多的改进，简单来说，G1吸收了增量GC以及CMS的精髓，将整个jvm Heap划分为多个固定大小的region，扫描时采用Snapshot-at-the-beginning的并发marking算法（具体在后面内容详细解释）对整个heap中的region进行mark，回收时根据region中活跃对象的bytes进行排序，首先回收活跃对象bytes小以及回收耗时短（预估出来的时间）的region，回收的方法为将此region中的活跃对象复制到另外的region中，根据指定的GC所能占用的时间来估算能回收多少region，这点和以前版本的Full GC时得处理整个heap非常不同，这样就做到了能够尽量短时间的暂停应用，又能回收内存，由于这种策略在回收时首先回收的是垃圾对象所占空间最多的region，因此称为Garbage First。

看完上面对于G1策略的简短描述，并不能清楚的掌握G1，在继续详细看G1的步骤之前，必须先明白G1对于JVM Heap的改造，这些对于习惯了划分为new generation、old generation的大家来说都有不少的新意。

G1将Heap划分为多个固定大小的region，这也是G1能够实现控制GC导致的应用暂停时间的前提，region之间的对象引用通过remembered set来维护，每个region都有一个remembered set，remembered set中包含了引用当前region中对象的region的对象的pointer，由于同时应用也会造成这些region中对象的引用关系不断的发生改变，G1采用了Card Table来用于应用通知region修改remembered sets，Card Table由多个512字节的Card构成，这些Card在Card Table中以1个字节来标识，每个应用的线程都有一个关联的remembered set log，用于缓存和顺序化线程运行时造成的对于card的修改，另外，还有一个全局的filled RS buffers，当应用线程执行时修改了card后，如果造成的改变仅为同一region中的对象之间的关联，则不记录remembered set log，如造成的改变为跨region中的对象的关联，则记录到线程的remembered set log，如线程的remembered set log满了，则放入全局的filled RS buffers中，线程自身则重新创建一个新的remembered set log，remembered set本身也是一个由一堆cards构成的哈希表。

尽管G1将Heap划分为了多个region，但其默认采用的仍然是分代的方式，只是仅简单的划分为了年轻代（young）和非年轻代，这也是由于G1仍然坚信大多数新创建的对象都是不需要长的生命周期的，对于应用新创建的对象，G1将其放入标识为young的region中，对于这些region，并不记录remembered set logs，扫描时只需扫描活跃的对象，G1在分代的方式上还可更细的划分为：fully young或partially young，fully young方式暂停的时候仅处理young regions，partially同样处理所有的young regions，但它还会根据允许的GC的暂停时间来决定是否要加入其他的非young regions，G1是运行到fully-young方式还是partially young方式，外部是不能决定的，在启动时，G1采用的为fully-young方式，当G1完成一次Concurrent Marking后，则切换为partially young方式，随后G1跟踪每次回收的效率，如果回收fully-young中的regions已经可以满足内存需要的话，那么就切换回fully young方式，但当heap size的大小接近满的情况下，G1会切换到partially young方式，以保证能提供足够的内存空间给应用使用。

除了分代方式的划分外，G1还支持另外一种pure G1的方式，也就是不进行代的划分，pure方式和分代方式的具体不同在下面的具体执行步骤中进行描述。

掌握了这些概念后，继续来看G1的具体执行步骤：

1.         Initial Marking

G1对于每个region都保存了两个标识用的bitmap，一个为previous marking bitmap，一个为next marking bitmap，bitmap中包含了一个bit的地址信息来指向对象的起始点。

开始Initial Marking之前，首先并发的清空next marking bitmap，然后停止所有应用线程，并扫描标识出每个region中root可直接访问到的对象，将region中top的值放入next top at mark start（TAMS）中，之后恢复所有应用线程。

触发这个步骤执行的条件为：

l  G1定义了一个JVM Heap大小的百分比的阀值，称为h，另外还有一个H，H的值为(1-h)*Heap Size，目前这个h的值是固定的，后续G1也许会将其改为动态的，根据jvm的运行情况来动态的调整，在分代方式下，G1还定义了一个u以及soft limit，soft limit的值为H-u*Heap Size，当Heap中使用的内存超过了soft limit值时，就会在一次clean up执行完毕后在应用允许的GC暂停时间范围内尽快的执行此步骤；

l  在pure方式下，G1将marking与clean up组成一个环，以便clean up能充分的使用marking的信息，当clean up开始回收时，首先回收能够带来最多内存空间的regions，当经过多次的clean up，回收到没多少空间的regions时，G1重新初始化一个新的marking与clean up构成的环。

2.         Concurrent Marking

按照之前Initial Marking扫描到的对象进行遍历，以识别这些对象的下层对象的活跃状态，对于在此期间应用线程并发修改的对象的以来关系则记录到remembered set logs中，新创建的对象则放入比top值更高的地址区间中，这些新创建的对象默认状态即为活跃的，同时修改top值。

3.         Final Marking Pause

当应用线程的remembered set logs未满时，是不会放入filled RS buffers中的，在这样的情况下，这些remebered set logs中记录的card的修改就会被更新了，因此需要这一步，这一步要做的就是把应用线程中存在的remembered set logs的内容进行处理，并相应的修改remembered sets，这一步需要暂停应用，并行的运行。

4.         Live Data Counting and Cleanup

值得注意的是，在G1中，并不是说Final Marking Pause执行完了，就肯定执行Cleanup这步的，由于这步需要暂停应用，G1为了能够达到准实时的要求，需要根据用户指定的最大的GC造成的暂停时间来合理的规划什么时候执行Cleanup，另外还有几种情况也是会触发这个步骤的执行的：

l  G1采用的是复制方法来进行收集，必须保证每次的”to space”的空间都是够的，因此G1采取的策略是当已经使用的内存空间达到了H时，就执行Cleanup这个步骤；

l  对于full-young和partially-young的分代模式的G1而言，则还有情况会触发Cleanup的执行，full-young模式下，G1根据应用可接受的暂停时间、回收young regions需要消耗的时间来估算出一个yound regions的数量值，当JVM中分配对象的young regions的数量达到此值时，Cleanup就会执行；partially-young模式下，则会尽量频繁的在应用可接受的暂停时间范围内执行Cleanup，并最大限度的去执行non-young regions的Cleanup。

这一步中GC线程并行的扫描所有region，计算每个region中低于next TAMS值中marked data的大小，然后根据应用所期望的GC的短延时以及G1对于region回收所需的耗时的预估，排序region，将其中活跃的对象复制到其他region中。

 
G1为了能够尽量的做到准实时的响应，例如估算暂停时间的算法、对于经常被引用的对象的特殊处理等，G1为了能够让GC既能够充分的回收内存，又能够尽量少的导致应用的暂停，可谓费尽心思，从G1的论文中的性能评测来看效果也是不错的，不过如果G1能允许开发人员在编写代码时指定哪些对象是不用mark的就更完美了，这对于有巨大缓存的应用而言，会有很大的帮助，G1将随JDK 6 Update 14 beta发布。

posted on 2009-03-11 22:18 [BlueDavy](http://www.blogjava.net/BlueDavy/) 阅读(5465) [评论(3)]()  [编辑](http://www.blogjava.net/BlueDavy/admin/EditPosts.aspx?postid=259230)  [收藏](http://www.blogjava.net/BlueDavy/AddToFavorite.aspx?id=259230) 所属分类: [Java](http://www.blogjava.net/BlueDavy/category/1366.html)

 ![]()

[]() []()

[
### 评论

]()

### []()[#]( "permalink: re: Garbage First介绍") []()re: Garbage First介绍  2009-03-14 13:37  [phpxer](http://www.blogjava.net/phpxer/)

Mark  [回复]()  [更多评论](http://www.blogjava.net/comment?author=phpxer "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: Garbage First介绍") []()re: Garbage First介绍  2009-04-01 12:14  [中国兄弟连](http://www.owner0571.com/about.asp?c=CS)

给你踩踩哈!  [回复]()  [更多评论](http://www.blogjava.net/comment?author=%e4%b8%ad%e5%9b%bd%e5%85%84%e5%bc%9f%e8%bf%9e "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: Garbage First介绍") []()re: Garbage First介绍[]()  2013-03-04 23:52  [呵乐猫](http://www.helemao.com/)

学习了。  [回复]()  [更多评论](http://www.blogjava.net/comment?author=%e5%91%b5%e4%b9%90%e7%8c%ab "查看该作者发表过的评论") []()  []()
[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [推荐购买云服务器（15%返利+最高千元奖金）](http://www.aliyun.com/cps/channel?channel_id=1306&user=0&lv=1)   [博客园](http://www.cnblogs.com/)  [博问](http://q.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![](http://www.blogjava.net/Modules/CaptchaImage/JpegImage.aspx?cacheid=20130727130737) 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/BlueDavy/archive/2009/03/11/259230.html&SourceURL=/BlueDavy/archive/2009/03/11/259230.html)       [使用Ctrl+Enter键可以直接提交]      网站导航:

[博客园](http://www.cnblogs.com/ "程序员的网上家园")   [IT新闻](http://news.cnblogs.com/)   [知识库](http://kb.cnblogs.com/)   [C++博客](http://www.cppblog.com/)   [程序员招聘](http://job.cnblogs.com/)   [管理](http://www.blogjava.net/BlueDavy/archive/2009/03/11/259230.html?opt=admin) 相关文章:

* [关于《分布式Java应用：基础与实践》一书](http://www.blogjava.net/BlueDavy/archive/2010/05/25/321796.html)
* [杭州程序员圆桌交流第二期视频](http://www.blogjava.net/BlueDavy/archive/2010/04/30/319794.html)
* [第一次杭州程序员交流会总结](http://www.blogjava.net/BlueDavy/archive/2010/03/22/316148.html)
* [杭州程序员圆桌交流第一期–并发编程PPT](http://www.blogjava.net/BlueDavy/archive/2010/03/19/315989.html)
* [GCLogViewer(tool to visualize gc log) V0.2 Release](http://www.blogjava.net/BlueDavy/archive/2009/12/03/304597.html)
* [Simple Scala actor Vs java Thread Vs Kilim Test](http://www.blogjava.net/BlueDavy/archive/2009/11/25/303662.html)
* [《构建高性能的大型分布式Java应用》目录&试读样章](http://www.blogjava.net/BlueDavy/archive/2009/11/06/301448.html)
* [动态跟踪Java代码的执行状况工具--BTrace](http://www.blogjava.net/BlueDavy/archive/2009/10/10/297661.html)
* [GC策略的调优](http://www.blogjava.net/BlueDavy/archive/2009/10/09/297562.html)
* [Hessian 3.2.0的两个bug](http://www.blogjava.net/BlueDavy/archive/2009/08/06/290003.html)  
Powered by: [BlogJava](http://www.blogjava.net/) Copyright © BlueDavy
### 公告

[![]()](http://www.douban.com/subject/3843896/) 
[![]()](http://china.osgiusers.org/)
[![]()](http://feed.feedsky.com/bluedavy "BlogJava-BlueDavy之技术Blog") [![feedsky]()](http://feed.feedsky.com/bluedavy)
[![抓虾]()](http://www.zhuaxia.com/add_channel.php?url=http://feed.feedsky.com/bluedavy)
[![google reader]()](http://fusion.google.com/add?feedurl=http://feed.feedsky.com/bluedavy)
[![鲜果]()](http://www.xianguo.com/subscribe.php?url=http://feed.feedsky.com/bluedavy)

### 导航

* [BlogJava](http://www.blogjava.net/)
* [首页](http://www.blogjava.net/BlueDavy/)
* [新随笔](http://www.blogjava.net/BlueDavy/admin/EditPosts.aspx?opt=1)
* [联系](http://www.blogjava.net/BlueDavy/contact.aspx?id=1)
* [聚合](http://www.blogjava.net/BlueDavy/rss)[![]()](http://www.blogjava.net/BlueDavy/rss)
* [管理](http://www.blogjava.net/BlueDavy/admin/EditPosts.aspx)
[<]( "转到上一个月")2009年3月[>]( "转到下一个月")日一二三四五六222324252627281234[5](http://www.blogjava.net/BlueDavy/archive/2009/03/05.html)[6](http://www.blogjava.net/BlueDavy/archive/2009/03/06.html)78910[11](http://www.blogjava.net/BlueDavy/archive/2009/03/11.html)[12](http://www.blogjava.net/BlueDavy/archive/2009/03/12.html)1314151617181920212223242526[27](http://www.blogjava.net/BlueDavy/archive/2009/03/27.html)282930311234

### 统计

* 随笔 - 294
* 文章 - 2
* 评论 - 2068
* 引用 - 1

### 随笔分类

* [@RIAWork(10)](http://www.blogjava.net/BlueDavy/category/10609.html) [(rss)](http://www.blogjava.net/BlueDavy/category/10609.html/rss "Subscribe to @RIAWork(10)")
* [Internet(20)](http://www.blogjava.net/BlueDavy/category/32839.html) [(rss)](http://www.blogjava.net/BlueDavy/category/32839.html/rss "Subscribe to Internet(20)")
* [Java(82)](http://www.blogjava.net/BlueDavy/category/1366.html) [(rss)](http://www.blogjava.net/BlueDavy/category/1366.html/rss "Subscribe to Java(82)")
* [Javascript(7)](http://www.blogjava.net/BlueDavy/category/8127.html) [(rss)](http://www.blogjava.net/BlueDavy/category/8127.html/rss "Subscribe to Javascript(7)")
* [OSGi、SOA、SCA(81)](http://www.blogjava.net/BlueDavy/category/18356.html) [(rss)](http://www.blogjava.net/BlueDavy/category/18356.html/rss "Subscribe to OSGi、SOA、SCA(81)")
* [Plugin Architecture(10)](http://www.blogjava.net/BlueDavy/category/1514.html) [(rss)](http://www.blogjava.net/BlueDavy/category/1514.html/rss "Subscribe to Plugin Architecture(10)")
* [Workflow(4)](http://www.blogjava.net/BlueDavy/category/1513.html) [(rss)](http://www.blogjava.net/BlueDavy/category/1513.html/rss "Subscribe to Workflow(4)")
* [业界随想(28)](http://www.blogjava.net/BlueDavy/category/11559.html) [(rss)](http://www.blogjava.net/BlueDavy/category/11559.html/rss "Subscribe to 业界随想(28)")
* [数据集成(8)](http://www.blogjava.net/BlueDavy/category/22527.html) [(rss)](http://www.blogjava.net/BlueDavy/category/22527.html/rss "Subscribe to 数据集成(8)")
* [系统设计(38)](http://www.blogjava.net/BlueDavy/category/1911.html) [(rss)](http://www.blogjava.net/BlueDavy/category/1911.html/rss "Subscribe to 系统设计(38)")
* [软件工程(22)](http://www.blogjava.net/BlueDavy/category/1367.html) [(rss)](http://www.blogjava.net/BlueDavy/category/1367.html/rss "Subscribe to 软件工程(22)")

### 随笔档案

* [2010年5月 (3)](http://www.blogjava.net/BlueDavy/archive/2010/05.html)
* [2010年4月 (4)](http://www.blogjava.net/BlueDavy/archive/2010/04.html)
* [2010年3月 (3)](http://www.blogjava.net/BlueDavy/archive/2010/03.html)
* [2010年2月 (1)](http://www.blogjava.net/BlueDavy/archive/2010/02.html)
* [2010年1月 (2)](http://www.blogjava.net/BlueDavy/archive/2010/01.html)
* [2009年12月 (1)](http://www.blogjava.net/BlueDavy/archive/2009/12.html)
* [2009年11月 (3)](http://www.blogjava.net/BlueDavy/archive/2009/11.html)
* [2009年10月 (2)](http://www.blogjava.net/BlueDavy/archive/2009/10.html)
* [2009年9月 (2)](http://www.blogjava.net/BlueDavy/archive/2009/09.html)
* [2009年8月 (2)](http://www.blogjava.net/BlueDavy/archive/2009/08.html)
* [2009年7月 (3)](http://www.blogjava.net/BlueDavy/archive/2009/07.html)
* [2009年6月 (1)](http://www.blogjava.net/BlueDavy/archive/2009/06.html)
* [2009年5月 (4)](http://www.blogjava.net/BlueDavy/archive/2009/05.html)
* [2009年4月 (5)](http://www.blogjava.net/BlueDavy/archive/2009/04.html)
* [2009年3月 (5)](http://www.blogjava.net/BlueDavy/archive/2009/03.html)
* [2009年1月 (1)](http://www.blogjava.net/BlueDavy/archive/2009/01.html)
* [2008年11月 (3)](http://www.blogjava.net/BlueDavy/archive/2008/11.html)
* [2008年10月 (1)](http://www.blogjava.net/BlueDavy/archive/2008/10.html)
* [2008年9月 (2)](http://www.blogjava.net/BlueDavy/archive/2008/09.html)
* [2008年8月 (1)](http://www.blogjava.net/BlueDavy/archive/2008/08.html)
* [2008年7月 (4)](http://www.blogjava.net/BlueDavy/archive/2008/07.html)
* [2008年6月 (4)](http://www.blogjava.net/BlueDavy/archive/2008/06.html)
* [2008年5月 (3)](http://www.blogjava.net/BlueDavy/archive/2008/05.html)
* [2008年4月 (1)](http://www.blogjava.net/BlueDavy/archive/2008/04.html)
* [2008年3月 (3)](http://www.blogjava.net/BlueDavy/archive/2008/03.html)
* [2008年2月 (1)](http://www.blogjava.net/BlueDavy/archive/2008/02.html)
* [2008年1月 (10)](http://www.blogjava.net/BlueDavy/archive/2008/01.html)
* [2007年12月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/12.html)
* [2007年11月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/11.html)
* [2007年10月 (6)](http://www.blogjava.net/BlueDavy/archive/2007/10.html)
* [2007年9月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/09.html)
* [2007年8月 (4)](http://www.blogjava.net/BlueDavy/archive/2007/08.html)
* [2007年7月 (5)](http://www.blogjava.net/BlueDavy/archive/2007/07.html)
* [2007年6月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/06.html)
* [2007年5月 (4)](http://www.blogjava.net/BlueDavy/archive/2007/05.html)
* [2007年4月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/04.html)
* [2007年3月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/03.html)
* [2007年2月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/02.html)
* [2007年1月 (1)](http://www.blogjava.net/BlueDavy/archive/2007/01.html)
* [2006年12月 (6)](http://www.blogjava.net/BlueDavy/archive/2006/12.html)
* [2006年11月 (5)](http://www.blogjava.net/BlueDavy/archive/2006/11.html)
* [2006年10月 (8)](http://www.blogjava.net/BlueDavy/archive/2006/10.html)
* [2006年9月 (13)](http://www.blogjava.net/BlueDavy/archive/2006/09.html)
* [2006年8月 (15)](http://www.blogjava.net/BlueDavy/archive/2006/08.html)
* [2006年7月 (3)](http://www.blogjava.net/BlueDavy/archive/2006/07.html)
* [2006年6月 (7)](http://www.blogjava.net/BlueDavy/archive/2006/06.html)
* [2006年5月 (9)](http://www.blogjava.net/BlueDavy/archive/2006/05.html)
* [2006年4月 (12)](http://www.blogjava.net/BlueDavy/archive/2006/04.html)
* [2006年3月 (13)](http://www.blogjava.net/BlueDavy/archive/2006/03.html)
* [2006年2月 (9)](http://www.blogjava.net/BlueDavy/archive/2006/02.html)
* [2006年1月 (14)](http://www.blogjava.net/BlueDavy/archive/2006/01.html)
* [2005年12月 (11)](http://www.blogjava.net/BlueDavy/archive/2005/12.html)
* [2005年11月 (14)](http://www.blogjava.net/BlueDavy/archive/2005/11.html)
* [2005年10月 (9)](http://www.blogjava.net/BlueDavy/archive/2005/10.html)
* [2005年9月 (10)](http://www.blogjava.net/BlueDavy/archive/2005/09.html)
* [2005年8月 (5)](http://www.blogjava.net/BlueDavy/archive/2005/08.html)
* [2005年7月 (9)](http://www.blogjava.net/BlueDavy/archive/2005/07.html)
* [2005年6月 (9)](http://www.blogjava.net/BlueDavy/archive/2005/06.html)
* [2005年5月 (2)](http://www.blogjava.net/BlueDavy/archive/2005/05.html)
* [2005年2月 (1)](http://www.blogjava.net/BlueDavy/archive/2005/02.html)

### 文章档案

* [2005年5月 (2)](http://www.blogjava.net/BlueDavy/archives/2005/05.html)

### Blogger's

* [DBANotes](http://www.dbanotes.net/)
* 大名鼎鼎了，勿需多说
* [ESBZone](http://www.esbzone.net/) [(rss)](http://www.esbzone.net/feed "Subscribe to ESBZone")
* SOA实战者，强烈推荐
* [kenwu's blog[推荐]](http://kenwublog.com/) [(rss)](http://feed.kenwublog.com/ "Subscribe to kenwu's blog[推荐]")
* Java底层和JVM的很多文章，强烈推荐。
* [梦想风暴](http://dreamhead.blogbus.com/)
* [西湖边的穷秀才](http://www.blogjava.net/cenwenchu/)

### 搜索

*
*  

### 最新评论 [![]()](http://www.blogjava.net/BlueDavy/CommentsRSS.aspx)

* [1. re: 关于《分布式Java应用：基础与实践》一书](http://www.blogjava.net/BlueDavy/archive/2013/07/19/321796.html#401750)
* 博主 大神啊 这个博客很久没有更新了
* --1836567962@qq.com
* [2. re: 网站架构相关PPT、文章整理（更新于2009-7-15）](http://www.blogjava.net/BlueDavy/archive/2013/07/11/267970.html#401444)
* 这个文章很有用，收藏
* --度哥网
* [3. re: OSGi Opendoc & Book](http://www.blogjava.net/BlueDavy/archive/2013/05/28/267968.html#399877)
* 评论内容较长,点击标题查看
* --stono
* [4. re: OSGi Opendoc & Book](http://www.blogjava.net/BlueDavy/archive/2013/05/28/267968.html#399865)
* 我再看《OSGi》原理与最佳实践，可是源码没有办法下载呀；
有没有什么办法呢？
* --stono
* [5. re: 关于《分布式Java应用：基础与实践》一书](http://www.blogjava.net/BlueDavy/archive/2013/05/27/321796.html#399801)
* 在这篇blog中放置了我收集的一些网站架构相关的PPT和文章
* --色都

### 阅读排行榜

* [1. 大型网站架构演变和知识体系(57638)](http://www.blogjava.net/BlueDavy/archive/2008/09/03/226749.html)
* [2. 网站架构相关PPT、文章整理（更新于2009-7-15）(38732)](http://www.blogjava.net/BlueDavy/archive/2009/04/28/267970.html)
* [3. Hibernate实践(31780)](http://www.blogjava.net/BlueDavy/archive/2006/03/27/37582.html)
* [4. 系统设计说明书(架构、概要、详细)目录结构(24258)](http://www.blogjava.net/BlueDavy/archive/2005/06/13/6037.html)
* [5. 发布《OSGi实战》正式版(17725)](http://www.blogjava.net/BlueDavy/archive/2006/08/25/65741.html)

### 评论排行榜

* [1. 大型网站架构演变和知识体系(96)](http://www.blogjava.net/BlueDavy/archive/2008/09/03/226749.html)
* [2. 漫谈CMS(88)](http://www.blogjava.net/BlueDavy/archive/2005/09/07/12340.html)
* [3. 网站架构相关PPT、文章整理（更新于2009-7-15）(66)](http://www.blogjava.net/BlueDavy/archive/2009/04/28/267970.html)
* [4. 《OSGi进阶》预览版发布(59)](http://www.blogjava.net/BlueDavy/archive/2007/09/29/149631.html)
* [5. 发布《OSGi实战》正式版(54)](http://www.blogjava.net/BlueDavy/archive/2006/08/25/65741.html)
