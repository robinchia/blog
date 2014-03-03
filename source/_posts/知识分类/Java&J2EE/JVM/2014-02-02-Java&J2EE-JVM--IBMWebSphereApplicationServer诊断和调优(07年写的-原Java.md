---
layout: post
title: "IBM WebSphere Application Server 诊断和调优(07年写的-原Java"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# IBM WebSphere Application Server 诊断和调优(07年写的,原JavaEye精华帖)

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼]()

[招聘](http://job.iteye.com/iteye) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://zwchen.iteye.com/login "登录") [登录](http://zwchen.iteye.com/login) [注册](http://zwchen.iteye.com/signup)

# [zwchen的博客](http://zwchen.iteye.com/)

* [**博客**](http://zwchen.iteye.com/)
* [微博](http://zwchen.iteye.com/weibo)
* [相册](http://zwchen.iteye.com/album)
* [收藏](http://zwchen.iteye.com/link)
* [留言](http://zwchen.iteye.com/blog/guest_book)
* [关于我](http://zwchen.iteye.com/blog/profile)

### [IBM WebSphere Application Server 诊断和调优(07年写的,原JavaEye精华帖)]() **

**博客分类：**
* [IT技术](http://zwchen.iteye.com/category/9634)
[IBM](http://www.iteye.com/blogs/tag/IBM)[Websphere](http://www.iteye.com/blogs/tag/Websphere)[AIX](http://www.iteye.com/blogs/tag/AIX)[JVM](http://www.iteye.com/blogs/tag/JVM)[CMS](http://www.iteye.com/blogs/tag/CMS)

这是[上篇文章](http://zwchen.iteye.com/blog/646063)的续篇，也是07年初发表于JavaEye，被评为精华帖，浏览近四万次，也被各大IT媒体转载（google可查）。基于同样的原因，被删除了）。
对WebSphere一线开发人员，这么珍贵的文章，特别是会员的评论，就被这个商业社会给和谐。
因为，寻找和阅读WebSphere诊断和调优的资料，非常困难，否则，它那高额的服务费哪里来？（IBM的WebSphere服务费：人民币10000元/天）。
![]( "点击查看原始大小图片")
--------------------------------------------------------------------
续写这篇文章，已经过去一个半月了。直到现在，系统一直运行平稳。
先说说我接手这项工作的经历吧：该项目大部分是06年10月就部署在客户那边了，到07年3月份，WAS宕机问题实在无法忍受，我才加入进来，前半年有另外一位同事断断续续处理，但对问题一直都无可奈何，而且项目负责人也没有引起足够的重视。可想而知，最后付出的代价是非常惨重的。在这近半年的时间内，服务器宕机63次。每次宕机时，WAS的JVM会dump出一个heapdump.phd文件(heap快照)，然后JVM就死掉了，当然，此时WAS也停止了响应。一般我们的做法是重启，最后是干脆AIX每天晚上定时重启。有时候一天还死多次。大家见附件的截图（all- GC.png）。这是我接手后，用IBM的分析工具得到的截图。对截图的分析，留给后面对应的部分吧。
服务器不稳定、宕机问题，拖延到最后，客户愤怒了，公司高层也害怕了，部门还专门成立了八人攻关组。当然了，我当时的压力也非常大，因为我是技术负责人，也就是实实在在干活、想主意的。
服务器诊断那段时间，从前到后，我们也是沿着一条线走下来，虽然最后发现很多路都走不通。现在就按这个思路，也就是时间先后一步步叙述吧。我想，大家如果也碰到类似应用服务器诊断，应该思路差不多。
术语说明：
IBM Websphere Application Server：WAS，WebSphere本身是一个平台，产品家族
OutOfMemoryError：OOM，内存泄漏，内存溢出
Gabage Collection：GC，自动垃圾回收
Content Management System：CMS，就是给新闻类门户网站编辑们用的系统
我们诊断大体上经历了以下几个阶段：
1、按Job调度线程池引起内存泄漏诊断：因为很多次OOM是发生在某个特定时候，譬如14：30、22：40左右。
2、按应用程序引起内存泄漏诊断：用JProfiler等工具探测：因为总是发生OOM。
3、分离WAS怀疑有OOM的应用：因为每个WAS应用太多，20来个，混一起没法定位。
4、用IBM官方heap、GC分析工具。以及和IBM技术支持联系。WAS、AIX参数优化。
5、隔离出WAS超级恶魔程序：一个CMS产品。
6、WAS、AIX参数优化、设置。
我们走到第5步时，才出现效果。计算一下，那时已经过去一个月了。服务器宕机、系统不稳定，在这个验收的时候，客户已经忍无可忍，以致后来的每一次行动都得胆战心惊得去做。
**一、按Job调度线程池导致内存泄漏诊断**
因为从我们WAS的日志(默认是native_stderr.log)来看，最近半年的宕机时间都有一个明显时间规律。见附件截图Job1-1.png。
我想，做过Java服务器性能调优的朋友，都知道在Web容器里面启线程池是个不太好的做法，因为Web容器本身有一个线程池，譬如Servlet线程池（Tomcat默认起25个），而自启的线程池很容易导致Servlet线程管理混乱，最终导致GC问题。我们的现象似乎和那很符合。如果我们沿着这个思路做下去，具体怎样实施呢？
我们的WAS上部署了20个左右的Web应用，譬如Lucene全文检索、B2B行业数据同步等，都是通过Quartz的Job调度做的，当然还有很多其它调度。当时，由我负责，通知相关负责人，将定时调度暂时去掉。观察了几天，后来发现问题依然存在，不过时间有点随机了。
不过，最后还是发现OOM不是由Job调度引起的。
也就是说，我们这个方案是失败的。而且，我们的很多想法都是臆测的，没有可靠的根据，也没有方向，再加上我是第一次处理这种问题，这导致后来查找问题的艰难。但是，仔细想想，我们又能拿什么做依据呢？出现OOM错误，我想大多数人想到的，除了JVM参数设置，就是内存泄漏。其实，OOM发生有很多种情况，在IBM、Sun、BEA等不同虚拟机上，因为GC机制不一样，所以原因一般都不同，容易定位难度也不一样。下文会谈到。
于是，我们干脆釜底抽薪分析问题吧：用JProfiler检测。
**二、按应用程序导致内存泄漏诊断，JProfiler检测**
如果遇到OOM问题，我想大家都会想到内存检测工具，现在最可靠的还是下面三种分析工具：Borland 的Optimizeit Suite，Quest的JProbe，ej-technologies的JProfiler。但面临三个问题：
1、三个都是商业产品，公司暂时没有买，必须自己下载，而且要找序列号。
2、工具必须支持AIX5.3＋JDK1.42＋WAS6.0，不是Windows平台。
3、工具必须在客户真实环境下部署，对客户的业务不能有冲击，也就是说部署测试工具前，必须做个大量测试，对工具非常熟练，遇到意外可以立即恢复现场。
Note:项目上线后，而不是测试或试运行阶段遇到此类问题，非常考验人；另外一个，就是性能和可伸缩性问题，很可能把整个项目给毁了。
当我决定要这么做后，就立即动手查阅这些工具的官方文档，用emule下载，最终都下载到了。试用后，最终选择了JProfiler4.03，比起其它工具，它界面美观、清晰、功能强大、集成度高(Heap,Memory,CPU,Thread都统一了)。另外，JProbe没有AIX版本，这也是放弃它的一个原因。
JVM的Profiler原理，都是通过JVM内置的的标准C语言Profiler 接口收集数据，然后通过Profiler工具的客户端展现。也就是说各厂商的Profiler工具，都有两个部分，一个部分是Profiler Agent，和JVM绑定，负责收集JVM内部数据，譬如方法调用次数、耗费时间等；另外一个部分就是Profiler front-end。通过Profiler工具的自定义local或remote协议传输到front-end，其实，我们最常用的JavaIDE的debug功能就是在此基础上的（JPDA）。（JProfiler的截图[http://www.ej-technologies.com/products/jprofiler/screenshots.html](http://www.ej-technologies.com/products/jprofiler/screenshots.html)）。
下面是Sun官方文档：
JDK1.42及以前是JVM PI：[http://java.sun.com/j2se/1.4.2/docs/guide/jvmpi/jvmpi.html](http://java.sun.com/j2se/1.4.2/docs/guide/jvmpi/jvmpi.html)
JDK1.5是JVM TI：[http://java.sun.com/j2se/1.5.0/docs/guide/jvmti/jvmti.html](http://java.sun.com/j2se/1.5.0/docs/guide/jvmti/jvmti.html)
具体到JProfiler的配置上，专门针对JDK1.4和1.5的JVM配置差别很大。
我用的JProfiler是4.31版本，透露给大家一个万能序列号吧(这东西不太好找)，对各版本应该都支持。深入了解Java，这类工具是不可少的：
Name: License for You
Lincese Code: A-G667#42616F-10kg1r134fq9m#2217
为了保证真实环境的检测成功，我做了大量的试验，譬如：
1、Windows系列的本地、远程测试。
2、AIX的远程测试。
3、Tomcat5.0、WebLogic8.14、WebSphere6.02，以及上述两种方式的组合测试，排列组合，场景不下10个。
当时也参阅了大量JVM文档，JProfiler官方几百页英文文档，辅助的JProbe对照。而且也制造过内存泄漏造成的OOM场景。
当然，要是在几个月前，在客户那边部署的测试环境时，就进行测试该多好啊。
在公司内部，我用 JProfiler测试了我们当时部署的几个应用，没有发现内存泄漏，所以，我们最怀疑的是就是CMS系统。因为出问题的那个WAS上它占去了90％的负荷（我们有多台AIX、WAS服务器）。该CMS超级庞大，感觉著名的赛迪网就用它，当时该CMS厂商给我们部署都花了快一个月。所以再重新部署一套测试环境也挺困难。另外，CMS提供商不给lisence。现在测试，客户早就对我们恼火了，当然不怎么配合，这对我们工作的开展就有很大的挑战。
在大致可以确定万无一失的情况下，我们最终决定在客户的真实环境下测试。也就是让JProfiler的agent端直接在WAS的JVM里面启动（北京IDC），然后远程（大连）监控。
本来该模式在另外几个应用的测试都通过了（因为北京IDC那边好几台AIX服务器）。但一部署上，客户的一些编辑用CMS时就感觉超级慢，尽管我们用了JProfiler的最小负载模式。半个小时后，客户实在无法忍受，打电话过来，又把我们部长和经理训了一顿，还要写书面报告。我们被迫马山中止测试，恢复现场。
虽然JProfiler也支持客户那边的环境，但还是有bug，至少负载一高就有严重的性能问题，几乎导致系统挂起，这是我当时没有料到的。JProfiler一启动，WAS的启动时间就由原来的3分钟降到10分钟，而且系统响应明显变慢，我们具体的环境如下（排列组合恐怕不下20种）：
1、AIX5.3，Power PC64位（不是32位）
2、WebSphere6.0
3、IBM JVM1.42
4、Remote 模式
我后来仔细读了一下JProfiler的changeLog，发现对上面的环境支持不够也很正常，因为官方在最近的4.3.x版本下陆续有对IBM JVM和Websphere6.0的features和bug fix：[http://www.ej-technologies.com/download/jprofiler/changelog.html](http://www.ej-technologies.com/download/jprofiler/changelog.html)
进行到这一步，我忽然觉得无计可施了 ，此时已经过了三周。
上面的策略，我认为是很正统的处理方法。谁怪我们当初项目上线时没有进行充分的测试呢？其实，这类测试没做也正常，OOM这类问题谁都无法预测。
到这个时候，我想肯定有人会问我？你知道导致JVM的OOM有几种情况吗？在当时，我想到了以下五种：
1、JVM的heap最小、最大值没有设，或不合理。
2、JVM的maxPermSize没有设置（这个IBM的JVM没有，一设置JVM就起不来）。
对于Sun和BEA的JVM，以上两种参数设置，可以排除90％以上的非程序内存溢出导致的OOM。
3、程序内存泄漏
4、有的JVM，当在80%的CPU时间内，不能GC出2%的heap时，也发生OOM（IBM的JVM就是，但我没有验证）
5、JVM本身内存泄漏（JVM有bug不是不可能）
现在的难题是，如果是那个可怕的CMS程序本身有内存溢出，在产品环境下，我们怎么去验证？我们自己开发的10来个web应用，测试并不是很难，在公司测试都可以。但是，我现在最想解决的，就是CMS测试的问题。而且，在我们那种环境下，该CMS产品供应商并没有透露成功案例。
其实，最后发现，并不是内存泄漏造成的，因为我们的heap走势都很平稳。纳闷的是，有1000M的heap，每次在heap只被占用400来M时就发生OOM，没有任何预兆。大家猜猜，会是什么原因 ？这个问题我到后面相关章节再说吧。
既然我们所有的矛头都指向那个可怕的CMS系统，现在就是考虑隔离的问题。如果我们验证这个问题是CMS造成的，那么大部分责任就应该由CMS厂商承担 。
既然CMS我们不敢移（费劲，而且客户在正式用），那我就移我们开发的10来个web系统吧。
**三、移出除CMS系统以外的所有应用
**说起来容易啊，做呢？ 隔离（移动）工作由我负责，具体涉及到10来个相关负责人。
转移工作，必须处理好很多问题，就说几个印象最深的吧：
1、某些应用，如Blog和BBS，都涉及到文件、图片上传目录和产品本身的环境，如 JDBC连接池、Cache位置。
2、目标服务器本身的环境，WAS安装环境、网络等。
3、移植时的先后顺序、调度，各应用内部本身的约束关系。
4、移植后的测试。
当然，还有一个最严峻的问题，客户允许我们这么做吗？对它们目前运行的系统有多大影响？风险如何评估？
这个工作持续了一天，已经完成了80％的工作，到第二天，客户又恼火了：WAS又宕机了。
为什么？这确实是WAS的一个bug：WAS的后台随便一操作，heap就会突然上升几百M，导致JVM内存不够。不过WAS撑住的话，过半小时后就会降下来，我估计是WAS后台对用户操作状态、文件都缓存到Session里面。你们可以检查类似这样的一个文件夹：d:\IBM\WebSphere\AppServer\profiles\AppSrv01\wstemp，我不知道为什么WAS不主动去清除它，它偷偷的就上升到几个G，系统硬盘可能不久就后就会空间不足，WAS莫名迟缓、最后死掉。听过WAS6.0以前这个目录更夸张。大家见我附件的截图WAS_Console.png那个尖峰。
咋办？经理也已经不敢让我们继续铤而走险了。这个方案最终又以失败告终 。
不过，最后我们还是发现问题出在CMS上。我们以前把这个问题向CMS技术支持反映，有大量依据和现象，并且把相关日志都给它们。过了两天，他们最后竟然只回了一句话“从给我的两个日志来看，没有找到任何与XXX有关的东西….”。TMD！我真的很生气 ，它们的产品都折磨我们半年之久了。不过，看他们对IBM的WAS和JVM也不懂，我也就不想再说什么了。下面是我的邮件，公司机密部分都隐去了：
引用
附件是我们这段时间服务器宕机的日志。我们用IBM Pattern Modeling and Analysis Tool for Java Garbage Collector Version 1.3.2分析了一下虚拟机日志，没有发现是内存泄漏导致；用IBM HeapAnalyzer Version 1.4.4 分析heap文件，也没有发现很可疑的内存泄漏。
我想以前你们也这样做过，现在我们分析错误日志，发现有一个现象，在宕机时，总是找不到文件，我看就是Websphere或是AIX IO资源不够，不知道是什么导致的。但是，我们自己的应用，基本上没有什么IO，除了一次load几个配置文件。不过，我觉得你们WCM的IO操作挺多的，不知道你对日志有什么新的发现。
客观的说，这几个月来，宕机那台服务器，除了你们的XXX，就以论坛和blog为主，而且他们都是开源的。在频繁宕机的06年11月份，我们的论坛和blog还没有上线。现在我们不得不每天晚上11点定时重启，但这也不是长久之计。
现在，我们进行分离遇到很大阻力，原来想把你们的XXX单独分离出来，在当前的环境下，不是很现实，如安装、测试（负载、定时服务），所以现在分离我们自己的应用，但当前在产品环境下，客户方阻力也很大。
希望尽快能够得到你们的问题建议和方案。
文中说到了IBM的两个分析工具，这也是我们后来的救星：我们就是需要这种离线分析工具，因为实时检测已经证明不现实。但我始终对该分析出来的结果抱怀疑态度，直到我去深入IBM的JVM以及和IBM的技术支持交流……
柳暗花明啊 ，至少看到了一点希望，不过最后我们还是失望而返。
**四、用IBM的HeapAnalyzer和GarbageCollector检测**
找到这两个工具，已经是够费劲了，因为以前找的IBM HeapRoot工具，让我对这类工具很失望。而且，这两个工具，只有在IBM的Techinical Support网站能够搜索到，但很不容易，因为那两个工具，并不是象IBM的Websphere产品那样宣传，它只在IBM Techinical Support文章的某些角落里出现。要知道，Techinical Support是IBM很重要的收入来源，这类文档，他们并不会让你很轻易就拿到，比起BEA WLS的支持网站dev2dev差远了。
具体诊断细节我就不详述了。我认为，IBM的WAS或JVM出了性能和OOM问题，这两个工具是最有效的，而且是离线分析工具，比起那些实时Profiler工具，某些场合有绝对的优势：譬如我们目前的产品环境，你只能分析宕机后的日志，实时分析前面已经验证是不可行的。
从日志分析，我们最终得出结论，我们购买的CMS系统有严重的碎片（大对象）问题，而该问题是OOM的罪魁祸首，而且IBM工程师也得出了同一结论。不过，在起先我们得出这一结论一周后，我还始终不相信heap碎片会导致OOM，直到IBM工程师总是向我强调。
我想很多人也是不太相信，因为大多数人用的都是Sun的JVM，譬如Windows、Solaris上的hotspot。而且，Sun JVM出问题，如果是配置的问题，一般通过配置heap最大最小值，以及maxPermSize都可以解决。Heap碎片导致的OOM，只有BEA的 JRockit和IBM JVM上发生，不过JRockit有专门文档说明，而且很容易找到（就在jdk的文档里面）。
配置heap最小最大值，我想大多数人都有经验。对于Sun的JVM来说，一般可以设置heap最大最小值一致，也是推荐的做法。因为它的GC策略默认是复制、分代算法。也就是说，它会将heap分成不同的几个区，譬如Solaris JVM中最上面有两个大小相等的区。GC时刻，将一个区的存活对象复制到另外一个对等区，垃圾对象就算遗弃了。这样在heap里面，就不存在碎片问题。另外，根据Java对象的存活期不同，将之转移到不同的区（Tenured区），存活最长的在最底部（火车算法），这也就是分代模型。具体请参考官方文档：[http://java.sun.com/docs/hotspot/gc1.4.2/](http://java.sun.com/docs/hotspot/gc1.4.2/)
对于maxPermSize（Permanent Generation），主要和那些加载到JVM里面的Java Class对象相关，它的空间不是在Java Heap里面分配。如果你当前的heap有1000M，permSize是200M，那么JVM至少占用1200M。
在这个空间内的对象的生存期和JVM是一样的，譬如JDK的核心类库，它们被System Classloader加载到JVM的Method Area（方法区）后，就不会被GC掉的，这些对象一般是Class对象，而不是普通的实例对象，也就是JVM的元数据。我们在用反射时经常用到它们。所以，对于现在象Spring、Hibernate这些框架经常通过反射创建实例，可能对maxPermSize要求就大了，缺省的64M很多时候是不够的，特别是对于应用服务器里的应用，象JSP就会产生和加载很多classes。不过，如果是它导致的OOM，一般会有类似 perm size提示。
但是，对于IBM的JVM，情况就完全不一样。它的默认GC策略并没有采取复制、分代。这个可以从GC日志分析出来。它不像Sun的JVM那样，有个单独的方法区，它的方法区就放在Java Heap里面。JVM规范里面并没有要求方法区的必须存放的位置，因为它只是一个JVM实现问题。
在IBM的JVM里面，这些对象一般分配在称为k-cluster和p-cluster里（cluster又是属于Heap），而后者一般是临时在heap里面申请。并且，这些cluster是不能GC，或是被移动重排的（Compact过程）。这就导致Java Heap里面就如同马蜂窝，但不同的蜂孔又不能合并，于是，当我们程序里面产生一个大对象，譬如2M的数组(数组必须分配在连续的内存区)时，就没有可分配空间了，于是就报告OOM。这些不能被移动的cluster就称为所谓的碎片。此时，JVM的Heap利用率可能不到50%。
当然，通过一定时期的GC日志，可以计算出cluster的合理大小（专门在Java Heap的底部），另外，还可以为这些大对象专门分配大对象区的（超过64k的对象）。
通过上面的理论介绍，我想大家一定知道了为什么IBM的JVM里面不推荐heap的最大最小值相同，因为这样碎片问题会非常严重：如果我们每次大对象申请内存时，heap都扩展5%，譬如50M，碎片问题很大程度上可以避开，程序性能也高些（寻找可用空隙和分配耗时，以及每次GC时间拉长）。
以上的具体阐述，请参考我在上文推荐的几个URL，另外再推荐三个宝贵的链接：
[http://www-1.ibm.com/support/docview.wss?rs=180&amp;context=SSEQTP&amp;q1=fragmentation&amp;uid=swg21176363&amp;loc=en_US&amp;cs=utf-8&amp;lang=en](http://www-1.ibm.com/support/docview.wss?rs=180&context=SSEQTP&q1=fragmentation&uid=swg21176363&loc=en_US&cs=utf-8&lang=en)
[http://www-900.ibm.com/cn/support/viewdoc/detail?DocId=2447476A10000](http://www-900.ibm.com/cn/support/viewdoc/detail?DocId=2447476A10000)（IBM 技术支持告诉我的，太重要了！）
[http://www-900.ibm.com/cn/support/viewdoc/detail?DocId=2847476B08000](http://www-900.ibm.com/cn/support/viewdoc/detail?DocId=2847476B08000)
我想大家应该会问：我怎么能够肯定我的OOM问题是heap碎片造成的呢？下面的方法可以验证。
在OOM发生时，JVM会产生一个heapdump文件。然后用GarbageCollector分析出该OOM发生时刻，JVM去申请的空间，譬如约235k。此时，你再用HeapAnalyzer去分析此时的heap快照里面的gap size大小（空隙大小）和各自的可用数目。你会发现，大于235k的空隙个数为0。这就是碎片导致OOM的证据。
另外，有人会问：我怀疑我的OOM是因为程序内存泄漏造成的，怎么去验证？
你可以用HeapAnalyzer分析发生OOM时刻的heap快照，工具会罗列出哪些对象怀疑有内存泄漏，譬如Cache对象都非常大（但你可以确定它不是内存泄漏）。另外，分析这次宕机（从这次虚拟机启动到宕机这段时间）的heap走势，如果曲线明显是向上倾斜，也就是那种典型的内存泄漏图，就有可能是内存泄漏。当然，还必须结合heap快照。
内存持续上升在JVM开始一段时间很正常，因为JVM对第一次访问到的Class 对象，譬如一个典型的Web应用，就有jdk的class、Spring或Hibernate的class对象，它们都会被缓存下来 (ClassLoader原理)，一般均不会被GC。当大多数class对象缓存差不多（当然还可能有一些Singleton对象，不过不怎么占分量），JVM的Heap就平稳了，呈一水平波浪或锯齿线。
如果可以用JProfiler这类工具实时监控，就更容易确诊了。
经过一番周折，我们终于看到了一线希望了 。
在一定的准备后，我们决定对WAS进行性能调优了。WAS的调优参数，可以分为两个部分：JVM级别和WAS级别：
JVM：主要是GC和Heap。
WAS：Thread Pool，JDBC DataSource。
当然要调节，你需要明白你的目标是什么，调节依据是什么，怎么计算，绝对不是凭空想象的，譬如heap最小值1024M，日志证明，该参数非常不适合我们的环境。具体细节，留给后文吧。
战战兢兢地，中午12:00，我们给产品环境下的WAS调节参数、重启，同时优化了AIX的IO相关参数。
我试着设置了一下JVM的k-cluster和p-cluster。下午15:00左右，WAS挂了，AIX也挂了。这下麻烦可大了。我们都慌了，马山客户的老总就来电话了，一阵哗哗啦啦。实在无奈，让客户那边工作人员通知机房（服务器托管处）工作人员重启AIX。我也不得不强行更改刚才的参数，立即设为另外一个值。
其实，我把那个两个cluster值确实设置太大了，我把它们设置为推荐值的5倍，譬如p-cluster是65k×110%×5。另外一个愚蠢的设置就是把最小heap设置为2048M(AIX有4G内存)。
后来我恢复到约正常的值，也就是去掉那个cluster的5，另外分配了一个30%的大对象区（如果1000M的heap，就是700M＋300M）。
就这样，系统持续正常运行了三天，以前可是一天一down。当在三天后再次宕机时，我们都没有自信了 。不得不通过AIX的cron，继续每天深夜11点的WAS定时重启。
不过，那次宕机，包括以后的几次宕机，再也没有出现OOM错误了，但系统依然不稳定。虽然我可以说OOM问题解决了，但领导和客户需要的并不是这种结果。
其实，在这个时候，我们已经发现我们系统的四大问题：
1、WAS和JVM参数：OOM问题
2、AIX的IO和Paging Spacing不足：AIX日志后来显示错误
3、AIX的WAS分区空间不够：WAS的日志膨胀一周就把那个opt分区塞满了。
4、应用程序的JDBC连接池：我们20来个应用，一个20 connections，DB2数据库有时被撑死。
也就是说，我们最初在客户那儿部署时，用的默认值根本不行。而且，部署涉及多人，人员之间出现断层。如果我们只是按OOM，无疑是走入死胡同，必须全局考虑！
但是，项目组实力薄弱，公司范围内就没有对AIX精通的。不过项目组原来有一个搞银行系统，在AIX下开发，就他熟悉些。我当时对AIX也比较陌生，你们从Linux转到AIX，你就知道它有多别扭了。命令都自定一套（也许因为是Unix元老吧），那个shell也超级别扭，而且参考书特少。不是自诩，我两年前负责一个高负载的Linux服务器管理一年多，也是玩得很转的。
就这样，他负责AIX的相关问题，我负责WAS相关的。
但是，现实环境，已经不允许我们再试验下去了 。我们必须找到一条绝对可靠的对策！
这就是下文的CMS系统大迁移，服务器再次优化。
**五、隔离CMS系统，服务器优化**
从前面的介绍，大家应该记得，我们开始是固定CMS，分离其它应用，但遭遇失败。现在是反过来，干脆把CMS系统赶出WAS平台 。
说实话，项目经理做这个决定，我认为已经是鼓出很大勇气了 。
当时我们想在一个备用AIX机上安装CMS产品测试，但最后还是没有做成：
CMS这类文章发布系统很难安装，也不好测试，又没有liscence，而且还有一堆准备工作。绝对没有著名的openCMS安装那么简单，当然功能远远比它复杂。而且，我们当时也低估了后来的工作，总觉得问题好解决。
在很遥远的06年中期，CMS厂商在客户那边一台AIX的Tomcat上部署了一套CMS产品。但当时客户执意要求将其跑在WAS上，也就是现在的情况。最开始，客户还要求我们必须用WAS的集群(我们买的就是WAS的ND版)，无奈该CMS不支持。要是集群，又是死伤一遍。其实，现在想想，我们当时太被动，CMS这种东西，就供公司的几十个编辑用，一个普通Tomcat就完全够用。而且，把它和面向公网的Internet应用混在一起，完全没有必要。也许，被动是因为我的实力造成的。
我们决定背水一战时，已经做过周密的计划：某年某月某日晚上8:00……
CMS产品负责人现场切换
xx（我）负责WAS相关参数调整
yy负责AIX参数。
zz负责应用的测试
…..
总之，该行动涉及到客户方、产品提供商、公司高层、项目组。每个人都密切关注，不下20人。每个人都守在电脑前，随时听候调遣，当天晚上，我们都没有准备回家睡觉，大家齐心协力。
真没想到，整个式切换工作，一个小时就顺利完成 ！第二天，客户编辑打开浏览器，她们一定想不到昨晚大家准备经历一场厮杀….
系统持续平稳地运行了一周，然后是漫长的五一，我回湖北黄冈老家休息了八天。回来时，一切依旧。
当天晚上，我们这边主要做了两项工作：
1、JVM的Heap参数，共五个。
2、AIX的IO、Paging Space等共六个。
当然还有其他人的工作，譬如测试、监控。
还有一个非常重要的方面：JDBC连接池。我们原来是在每个Web应用里面独立设置，这样20来个应用就有几百个DB连接，一不小心DB就给撑死。现在统一交由WAS内置DataSource处理，总共连接不到30个。其实，我们项目开始部署时，就是这样做的，但当时WAS内置的DataSource对JTA(XA)支持有bug （这个和IBM技术支持确认过，但他们没有给予很好的解决方案），不过Datasource还是配好的。
但是这个工作已经属于WAS性能优化的主题了，而且优化值必须持续观察一段时间，通过专门的分析工具来计算。
优化本身，是一项很考验人的工作，我就简单说一下最实用方法吧，也许是专门针对IBM的产品。
1、清理归零WAS日志。然后启动WAS，生成日志（-verbose:gc默认是开的）
2、让WAS持续运行约两周，让JVM Heap占用曲线平稳一段时间即可（用IBM的Garbage Collector分析观察）。
3、在AIX的shell里，产生heapdump.phd文件，也就是heap快照。命令：kill -3 pid (pid是WAS的PID，通过ps –ef查看)，观察heap当时的碎片情况，是否需要单独分大对象区（一般不需要设置），特别是那个方法区Class对象大小（p-cluster参数）。
4、通过GC工具，观察GC平均时间、Heap实际占用情况。Note: GC是一个Stop The World过程，也就是说GC时系统对外不响应，多CPU也不例外。看你的应用实际需求了，GC持续时间和频率是矛盾的，另外还有性能考虑。一般Web应用，我想让GC持续时间（Pause time）调节到合理值就ok了，譬如0.2到0.4s。
5、根据3可以算出k-cluster值，它是工具推荐值的110%。
6、Heap的最小值是程序刚启动不久的占用值，譬如320M。切记：IBM JVM初始值太大非常不好。
7、Heap的最大值是系统平稳后的100/70。也就是说如果最大值是1000M，那么应该平稳时是700M，还有30%的空余。IBM的JVM默认情下的碎片问题，WAS控制台下操作Heap猛增这种bug，你不得不堤防。Heap最大值不设，AIX下的WAS肯定OOM。
当然啦，我没有考虑到大对象区的计算（虽然我们的应用设置了专门的大对象区），包括IBM JVM支持的分代GC、并行GC，Heap每次expand百分比等。那些情况我们一般不常用，譬如，你的AIX平台一般不是16CPU吧？
一口气写到现在，我忽然觉得该收尾了。下面就说说我对这类工作的整体看法吧。
1、尽量在项目测试和试运行的时候就进行压力、性能测试，当正式投入使用后，如果发现类似问题，代价非常大。躲过算你运气好，一般来说，可能你们系统没多少人用，也不是核心业务系统，譬如一般的电子政务。
2、千万不要低估了技术风险，用IBM的系列产品尤其要慎重，出问题一定不要忘了技术支持。而且，查资料时，建议用google English，因为象WebSphere这类问题，很少有中文资料。
3、程序部署环境建立时，就要考虑到日后的正式环境，譬如AIX的Paging Space、IO、分区大小，默认值往往是不行的，而且在产品环境下改这些值，往往非常难。
4、在项目开发初期，就考虑到日志的问题，因为它分散到每个方法内，必须慎重定义好debug、info、warn、error级别，不要随便忽视异常（catch里面不记录），到真正程序出问题时，它就是我们的最重要的依据之一。当然这主要是功能性问题诊断。另外对于高负载网站，日志文件往往非常大，各级别日志千万不要混在一起，否则找问题就很困难了。
5、怎么说呢，别死扣技术，以为什么都可以通过技术解决。你看我们最大的问题就是把CMS给移到Tomcat下。你要是问我，为什么CMS产品会导致系统这么多的问题，我也不知道，到那时候，我确实也不想深入。我只要知道，赶出你这个应用，我的系统就好控制多了。而且，那个CMS系统，在Tomcat下，就是跑得服服帖帖的，非常稳定。难度是可恶的WAS？不过那CMS，据IBM工程师，包括我们二次开发，都觉得够烂了，每个jsp页面都打开、关闭DB connection（7年前的jsp开发模式），还有那么严重的大对象问题。
好了，以上总结的几点可能也不充分、深入，但如果你仔细读我这篇文章，应该有自己的想法。毕竟，只有经过思考的东西，才会属于自己。
————————————————-
**原文仅存的一点回复（google快照）**
也许有人会说，我看完全文，发现你们遇到的问题还是很基础啊。现在想想，也确实如此。不过，当时确实没有方向。另外，一个人不太可能有这么全面的知识。其实，最难的，还是那个IBM JVM的heap碎片问题，关键是你想不到，网上又很难搜到资料。大家要是用它后，出现OOM问题，一定要前车之鉴啊！
newold 写道
我感觉WAS调优没那么难吧,你们应该在测试系统上做下压力测试,不应该在生产系统上这么折腾.解决这种问题,首先考虑把WAS升级到最新版,然后再考虑是不是程序问题.如果WAS是正版的,IBM应该会帮你们分析日志的,我觉得你解决问题的方式不大对，把事情搞复杂了
1、我接手这个活的时候，系统已经在生产环境几个月了，这是我无法选择的。当初准备切换到正式环境时，没有人对这个问题引起重视，特别是领导，而且我当时也不在该小组。再说，谁会怀疑那个大名鼎鼎的CMS产品，中国恐怕是top 1。
2、把WAS升级到最新版是我在接手这个活的时候已经做了（6.0.0.0到6.0.2.0），所以文中我没有提及。另外，在诊断过程中也准备升级一次（从6.0.2.0到6.0.2.17），但考虑生产环境，风险太大。不过，我当时确实也总怀疑是否是这个原因，仔细读了读官方的bugfix（估计有几千个）。
3、“IBM应该会帮你们分析日志的”，当然了。当我去向IBM技术支持咨询时，他们向我要日志。不过，他们和我对日志的分析结果是一样的，只是他们让我更肯定了某些东西。主要就是文中那几个截图中的工具。另外，就是SystemOut.log日志（看是否有线程挂起）。
不过，对于IBM的技术支持，有点让人失望的是，我们解决了OOM后，但我们的系统还是不稳定，再向他们咨询时，那个WAS技术支持告诉我，他只提供IBM的WAS相关技术支持，如果你们告诉我WebLogic AS下很正常，我无法给你提供帮助，我们对其它公司的类似产品不了解。另外，如果你们怀疑是我们的AIX产品有问题，请咨询我们的AIX技术支持（当前前提是我们的AIX还在服务期，对于那个WAS的售后服务，我们是用公司另外一个项目组的license，因为我们的已经过期了）。
也就是说，IBM无法给你提供全局的技术支持！
4、一开始，我们根本就没有方向，应用系统又多（两个WAS上共30多个应用）。你仔细读文章也会知道，我们的现象和推测往往都是矛盾的。
什么东西，都是解决了，经历过了，再回转头来说，好简单啊，当初我怎么没想到？就像中国股市，要是两年前我都能够预测了现在，我早就暴发了。
而且，直到现在，我都可以坦率得说，不把那个CMS移出WAS，无论我们怎么优化AIX、WAS和JVM，都很难解决问题！
分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[回母校，用iPhone拍的几张照片](http://zwchen.iteye.com/blog/552019 "回母校，用iPhone拍的几张照片") | [又半年，技术的探险(2009.7)](http://zwchen.iteye.com/blog/551930 "又半年，技术的探险(2009.7)")
* 2009-12-19 11:07
* 浏览 3762
* [评论(5)]()
* 分类:[企业架构](http://www.iteye.com/blogs/category/architecture)
* [相关推荐](http://www.iteye.com/wiki/blog/551933)

### 评论

[]()

5 楼 [txyly998](http://txyly998.iteye.com/ "txyly998") 2011-08-05  

呵呵 谢谢你，我google下吧。
我们的系统就是数据量太大才导致性能问题频发的，最大的局点有一千几百万用户、一千几百万订购关系等等。
4 楼 [zwchen](http://zwchen.iteye.com/ "zwchen") 2011-08-05  

txyly998 写道

现在就想问你一下，你说的按通用的JVM设置方式调整参数，这个有没有具体点的步骤呢？
google Tomcat的一般参数设置方法，或是选择WAS一般的调优参数。
像你们这种政务系统，一般是因为加个WAS，项目合同可以翻倍，和WAS无关。而且，你们的负载应该是超低的(员工用)。
对于非分布式应用（无RMI、EJB），WAS就是垃圾。
我隐约记得(好几年没关注了)，IBM的JVM的GC和sun的不一样，所以最大和最小的heap可能是设置不同的。

3 楼 [txyly998](http://txyly998.iteye.com/ "txyly998") 2011-08-04  

首先非常感谢你的回复！谢谢！
我们的这个系统在全国有13个局点在用，有些局点基本没怎么出现（在我接触这个1年多的时间内），有些局点则1个月出现一两次，项目组都没多少人懂这类的问题，可能也没有引起足够的重视，最近出现的局点越来越多，而我是在公司支持现网的，现网有问题直接联系我这边，所以想自己试着解决，目前感觉有点无从下手。
对于你说的两周重启一次，这个不行的，现场没事不能随便重启环境的，有特殊情况要重启环境必须要向客户申请。另外，出现问题后，watchdog会自动重启系统或者切换双机的。出现重启或者切换，现场的人就要我们分析原因的。
现在就想问你一下，你说的按通用的JVM设置方式调整参数，这个有没有具体点的步骤呢？
2 楼 [zwchen](http://zwchen.iteye.com/ "zwchen") 2011-08-04  

txyly998 写道

我们一个系统down掉了，生成了javacore和heapdump文件，有IBM Thread and Monitor Dump Analyzer for Java和IBM HeapAnalyzer工具，但是不知道怎么分析这俩文件，能否指教下呢？多谢！
操作系统AIX5.3
JDK 1.5
华为的uniportal平台（Tomcat和Jboss结合到一起的）
如果只是down一次，没有出现规律性的宕机，比如一周一次，暂时不用太考虑，因为你的数据积累不够。
你可以：
1、如果系统一个月宕一两次，你可以两周重启一下服务
2、按通用的JVM设置方式，调整参数
我不太建议直接去分析headdump等问题，它必须对各种JVM的GC有很深入的了解，我当时也是自学过一两个月，摸索着调成功的概率非常低。
如果你们的WAS还在技术支持范围内，咨询IBM北京。
自己去解决这种极其有难度的问题，从公司的角度是很不值的。

1 楼 [txyly998](http://txyly998.iteye.com/ "txyly998") 2011-08-02  

我们一个系统down掉了，生成了javacore和heapdump文件，有IBM Thread and Monitor Dump Analyzer for Java和IBM HeapAnalyzer工具，但是不知道怎么分析这俩文件，能否指教下呢？多谢！
操作系统AIX5.3
JDK 1.5
华为的uniportal平台（Tomcat和Jboss结合到一起的）
### 发表评论

[![]()](http://zwchen.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://zwchen.iteye.com/login)

[![zwchen的博客]( "zwchen的博客: zwchen的博客")](http://zwchen.iteye.com/)

zwchen

* 浏览: 411240 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 成都
* ![]()
### 最近访客 [更多访客>>](http://zwchen.iteye.com/blog/user_visits)

[![dylinshi126的博客]( "dylinshi126的博客: ")](http://dylinshi126.iteye.com/)

[dylinshi126](http://dylinshi126.iteye.com/ "dylinshi126")

[![kglgmlldd的博客]( "kglgmlldd的博客: kglgmlldd")](http://kglgmlldd.iteye.com/)

[kglgmlldd](http://kglgmlldd.iteye.com/ "kglgmlldd")
[![frfgzzq的博客]( "frfgzzq的博客: ")](http://frfgzzq.iteye.com/)

[frfgzzq](http://frfgzzq.iteye.com/ "frfgzzq")

[![sungang_1120的博客]( "sungang_1120的博客: 50")](http://sungang-1120.iteye.com/)

[sungang_1120](http://sungang-1120.iteye.com/ "sungang_1120")

### 文章分类

* [全部博客 (113)](http://zwchen.iteye.com/)
* [IT技术 (25)](http://zwchen.iteye.com/category/9634)
* [互联网和电子商务 (13)](http://zwchen.iteye.com/category/107180)
* [杂谈 (24)](http://zwchen.iteye.com/category/24390)
* [管理和商业 (23)](http://zwchen.iteye.com/category/39194)
* [我的生活 (28)](http://zwchen.iteye.com/category/12717)
### 社区版块

* [我的资讯](http://zwchen.iteye.com/blog/news) (0)
* [我的论坛](http://zwchen.iteye.com/blog/post) (547)
* [我的问答](http://zwchen.iteye.com/blog/answered_problems) (0)

### 存档分类

* [2013-07](http://zwchen.iteye.com/blog/monthblog/2013-07) (1)
* [2012-11](http://zwchen.iteye.com/blog/monthblog/2012-11) (1)
* [2012-10](http://zwchen.iteye.com/blog/monthblog/2012-10) (1)
* [更多存档...](http://zwchen.iteye.com/blog/monthblog_more)
### 评论排行榜

* [长江三峡游轮之旅(宜昌到重庆)](http://zwchen.iteye.com/blog/1912647 "长江三峡游轮之旅(宜昌到重庆)")
* [四川燕子沟-雅加格-四姑娘山-巴郎山自驾 ...](http://zwchen.iteye.com/blog/1717111 "四川燕子沟-雅加格-四姑娘山-巴郎山自驾游")

### 最新评论

* [yangfuchao418](http://yangfuchao418.iteye.com/ "yangfuchao418")： zwchen 写道yangfuchao418 写道不错，只是那 ...
[长江三峡游轮之旅(宜昌到重庆)](http://zwchen.iteye.com/blog/1912647#bc2320017)
* [zwchen](http://zwchen.iteye.com/ "zwchen")： yangfuchao418 写道不错，只是那水怎么这么脏，总共 ...
[长江三峡游轮之旅(宜昌到重庆)](http://zwchen.iteye.com/blog/1912647#bc2319980)
* [zwchen](http://zwchen.iteye.com/ "zwchen")： yangfuchao418 写道不错，只是那水怎么这么脏，总共 ...
[长江三峡游轮之旅(宜昌到重庆)](http://zwchen.iteye.com/blog/1912647#bc2319978)
* [yangfuchao418](http://yangfuchao418.iteye.com/ "yangfuchao418")： 不错，只是那水怎么这么脏，总共花了多少银子？
[长江三峡游轮之旅(宜昌到重庆)](http://zwchen.iteye.com/blog/1912647#bc2319921)
* [shengfuqiang](http://fqsheng.iteye.com/ "shengfuqiang")： 每天读你的博客都有收获，都能给我带来不一样的心得。
[我的2011](http://zwchen.iteye.com/blog/1337350#bc2318900)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2012 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![](http://stat.iteye.com/?url=http%3A%2F%2Fzwchen.iteye.com%2Fblog%2F551933&referrer=http%3A%2F%2Fzwchen.iteye.com%2Fcategory%2F9634&user_id=)
