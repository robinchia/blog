---
layout: post
title: "Java 7 的新特性一览表"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# Java 7 的新特性一览表

# Java 7 的新特性一览表

[红薯](http://my.oschina.net/javayou) 发布于： 2011年07月27日 ([82评]())

**分享到 **

[新浪微博]( "分享到新浪微博")[腾讯微博]( "分享到腾讯微博")
[收藏]( "收藏此新闻")+72
![]()

官方说是 7月28日 正式发布 Java 7 ，正常的话我们应该在 7月29日 看到这个版本。很快了，就两天时间。

发布之前让我们先来看看 Java 7 都有什么新特性吧。

Java 7 的架构图：

![]()

新特性一览表：

**Swing**

* 新增 [
JLayer
](http://download.oracle.com/javase/7/docs/api/javax/swing/JLayer.html) 类，是一个灵活而且功能强大的Swing组件修饰器，使用方法：[How to Decorate Components with JLayer](http://download.oracle.com/javase/tutorial/uiswing/misc/jlayer.html).
* [Nimbus Look and Feel](http://download.oracle.com/javase/tutorial/uiswing/lookandfeel/nimbus.html) 外观从

com.sun.java.swing
包移到

javax.swing
包中，详情：[
javax.swing.plaf.nimbus
](http://download.oracle.com/javase/7/docs/api/javax/swing/plaf/nimbus/package-summary.html)
* [更轻松的重量级和轻量级组件的混合](http://java.sun.com/developer/technicalArticles/GUI/mixing_components/index.html)
* 支持透明窗体以及非矩形窗体的图形界面，请看 [How to Create Translucent and Shaped Windows](http://download.oracle.com/javase/tutorial/uiswing/misc/trans_shaped_windows.html)
* [
JColorChooser
](http://download.oracle.com/javase/7/docs/api/javax/swing/JColorChooser.html) 类新增 HSV tab.

**网络**

* 
新增
[
URLClassLoader.close
](http://download.oracle.com/javase/7/docs/api/java/net/URLClassLoader.html#close%28%29) 方法，请看 [Closing a URLClassLoader](http://download.oracle.com/javase/7/docs/technotes/guides/net/ClassLoader.html).
* 支持 Sockets Direct Protocol (SDP) 提供高性能网络连接，详情请看 [Understanding the Sockets Direct Protocol](http://download.oracle.com/javase/tutorial/sdp/sockets/index.html).

**集合**

* 
新增
[
TransferQueue
](http://download.oracle.com/javase/7/docs/api/java/util/concurrent/TransferQueue.html) 接口，是 [
BlockingQueue
](http://download.oracle.com/javase/7/docs/api/java/util/concurrent/BlockingQueue.html) 的改进版，实现类为 [
LinkedTransferQueue
](http://download.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedTransferQueue.html)

**RIA/发布**

* 拖拽的小程序使用一个默认或者定制的标题进行修饰，详情：[Requesting and Customizing Applet Decoration in Draggable Applets](http://download.oracle.com/javase/tutorial/deployment/applet/draggableApplet.html#decoration).
* JNLP 文件做了如下方面的增强，详情请看 [JNLP File Syntax](http://download.oracle.com/javase/7/docs/technotes/guides/javaws/developersguide/syntax.html):

* The

os
attribute in the

information
and

resources
elements can now contain specific versions of Windows, such as Windows Vista or Windows 7.
* Applications can use the

install
attribute in the

shortcut
element to specify their their desire to be installed. Installed applications are not removed when the Java Web Start cache is cleared, but can be explicitly removed using the Java Control Panel.
* Java Web Start applications can be deployed without specifying the

codebase
attribute; see [Deploying Without Codebase](http://download.oracle.com/javase/tutorial/deployment/deploymentInDepth/deployingWithoutCodebase.html)
* 可直接在 HTML 中嵌入 JNLP 文件：[Embedding JNLP File in Applet Tag](http://download.oracle.com/javase/tutorial/deployment/deploymentInDepth/embeddingJNLPFileInWebPage.html).
* 可在 JavaScript 代码中检查 Applet 是否已经加载完成：[Handling Initialization Status With Event Handlers](http://download.oracle.com/javase/tutorial/deployment/applet/appletStatus.html).
* 可在 Applet 从快捷方式启动或者拖出浏览器时对窗口样式和标题进行控制：[Requesting and Customizing Applet Decoration](http://download.oracle.com/javase/tutorial/deployment/applet/draggableApplet.html#decoration) in [Developing Draggable Applets](http://download.oracle.com/javase/tutorial/deployment/applet/draggableApplet.html).

**XML**

* 包含 [Java API for XML Processing](http://jaxp.java.net/) (JAXP) 1.4.5, 支持 [Java Architecture for XML Binding](http://jaxb.java.net/) (JAXB) 2.2.3, 和 [Java API for XML Web Services](http://jax-ws.java.net/) (JAX-WS) 2.2.4.

**java.lang 包**

* 消除了在多线程环境下的非层次话类加载时导致的潜在死锁，详情：[Multithreaded Custom Class Loaders in Java SE 7](http://download.oracle.com/javase/7/docs/technotes/guides/lang/cl-mt.html).

**Java 虚拟机**

* [支持非 Java 语言](http://download.oracle.com/javase/7/docs/technotes/guides/vm/multiple-language-support.html): Java SE 7 引入一个新的 JVM 指令用于简化实现动态类型编程语言
* [Garbage-First Collector](http://download.oracle.com/javase/7/docs/technotes/guides/vm/G1.html) 是一个服务器端的垃圾收集器用于替换 Concurrent Mark-Sweep Collector (CMS).
* [提升了 Java HotSpot 虚拟机的性能](http://download.oracle.com/javase/7/docs/technotes/guides/vm/performance-enhancements-7.html)

**Java I/O**

[
java.nio.file
](http://download.oracle.com/javase/7/docs/api/java/nio/file/package-summary.html) 包以及相关的包 [
java.nio.file.attribute
](http://download.oracle.com/javase/7/docs/api/java/nio/file/attribute/package-summary.html) 提供对文件 I/O 以及访问文件系统的全面支持，请看 [File I/O (featuring NIO.2)](http://download.oracle.com/javase/tutorial/essential/io/fileio.html).

* 目录

*<Java home>*/sample/nio/chatserver/
包含使用 java.nio.file 包的演示程序
* 目录

*<Java home>*/demo/nio/zipfs/
包含 NIO.2 NFS 文件系统的演示程序

**安全性**

* 新的内置对多个基于 ECC 算法(ECDSA/ECDH)的支持，详情请看：[Sun PKCS#11 Provider's Supported Algorithms](http://download.oracle.com/javase/7/docs/technotes/guides/security/p11guide.html#ALG) in [Java PKCS#11 Reference Guide](http://download.oracle.com/javase/7/docs/technotes/guides/security/p11guide.html).
* 禁用了一些弱加密算法，详情请看 [Appendix D: Disabling Cryptographic Algorithms](http://download.oracle.com/javase/7/docs/technotes/guides/security/certpath/CertPathProgGuide.html#AppD) in [Java PKI Programmer's Guide](http://download.oracle.com/javase/7/docs/technotes/guides/security/certpath/CertPathProgGuide.html) and [Disabled Cryptographic Algorithms](http://download.oracle.com/javase/7/docs/technotes/guides/security/jsse/JSSERefGuide.html#DisabledAlgorithms) in [Java Secure Socket Extension (JSSE) Reference Guide](http://download.oracle.com/javase/7/docs/technotes/guides/security/jsse/JSSERefGuide.html).
* Java 安全套接字扩展中对 SSL/TLS 的增强

**并发**

* fork/join 框架，基于 [
ForkJoinPool
](http://download.oracle.com/javase/7/docs/api/java/util/concurrent/ForkJoinPool.html) 类，是 [
Executor
](http://download.oracle.com/javase/7/docs/api/java/util/concurrent/Executor.html) 接口的实现，设计它用来进行高效的运行大量任务；使用 work-stealing 技术用来保证大量的 worker 线程工作，特别适合多处理器环境，详情请看 [Fork/Join](http://download.oracle.com/javase/tutorial/essential/concurrency/forkjoin.html)

* 目录

*<Java home>*/sample/forkjoin/
包含了 fork/join 框架的演示程序
* [
ThreadLocalRandom
](http://download.oracle.com/javase/7/docs/api/java/util/concurrent/ThreadLocalRandom.html) 类class 消除了使用伪随机码线程的竞争，请看 [Concurrent Random Numbers](http://download.oracle.com/javase/tutorial/essential/concurrency/threadlocalrandom.html).
* [
Phaser
](http://download.oracle.com/javase/7/docs/api/java/util/concurrent/Phaser.html) 类是一个新的同步的屏障，与 [
CyclicBarrier
](http://download.oracle.com/javase/7/docs/api/java/util/concurrent/CyclicBarrier.html) 类似.

**Java 2D**

* 一个新的基于 XRender 的 Java 2D 渲染管道支持现在的 X11 桌面，改善了图形性能，请看 [System Properties for Java 2D Technology](http://download.oracle.com/javase/7/docs/technotes/guides/2d/flags.html#xrender) 中的

xrender
.
* JDK 可枚举并显示出已安装的 OpenType/CFF 字体，通过 [
GraphicsEnvironment.getAvailableFontFamilyNames
](http://download.oracle.com/javase/7/docs/api/java/awt/GraphicsEnvironment.html#getAvailableFontFamilyNames%28%29) 方法 See [Selecting a Font](http://download.oracle.com/javase/tutorial/2d/text/fonts.html).
* [
TextLayout
](http://download.oracle.com/javase/7/docs/api/java/awt/font/TextLayout.html) 类支持西藏语脚本
* 
libfontconfig
, 是一个字体配置 api ，see [Fontconfig](http://www.fontconfig.org/).

**国际化**

* 支持 [Unicode 6.0.0](http://www.unicode.org/versions/Unicode6.0.0) 

* 目录

*<Java home>*/demo/jfc/Font2DTest/
包含 Unicode 6.0 的演示程序
* Java SE 7 可容纳在 ISO 4217 中新的货币，详情请看 [
Currency
](http://download.oracle.com/javase/7/docs/api/java/util/Currency.html) 类.

**Java 编程语言特性**

* [二进制数字表达方式](http://download.oracle.com/javase/7/docs/technotes/guides/language/binary-literals.html)
* [使用下划线对数字进行分隔表达，例如 1_322_222](http://download.oracle.com/javase/7/docs/technotes/guides/language/underscores-literals.html)
* [switch 语句支持字符串变量](http://download.oracle.com/javase/7/docs/technotes/guides/language/strings-switch.html)
* [泛型实例创建的类型推断](http://download.oracle.com/javase/7/docs/technotes/guides/language/type-inference-generic-instance-creation.html)
* [使用可变参数时，提升编译器的警告和错误信息](http://download.oracle.com/javase/7/docs/technotes/guides/language/non-reifiable-varargs.html)
* [try-with-resources 语句
](http://download.oracle.com/javase/7/docs/technotes/guides/language/try-with-resources.html)
* [同时捕获多个异常处理](http://download.oracle.com/javase/7/docs/technotes/guides/language/catch-multiple.html)

**JDBC 4.1**

* 支持使用 [
try
-with-resources](http://download.oracle.com/javase/7/docs/technotes/guides/language/try-with-resources.html) 语句进行自动的资源释放，包括连接、语句和结果集
* 支持 RowSet 1.1

Java 的详细介绍：[请点这里](http://www.oschina.net/p/java)
Java 的下载地址：[请点这里](http://www.oschina.net/action/project/go?id=16962&p=download)
想通过手机客户端访问开源中国：[请点这里](http://www.oschina.net/app)
本站文章除注明转载外，均为本站原创或编译
欢迎任何形式的转载，但请务必注明出处，尊重他人劳动共创开源社区
转载请注明：文章转载自：**开源中国社区** [[http://www.oschina.net](http://www.oschina.net/)]
本文标题：Java 7 的新特性一览表
本文地址：[http://www.oschina.net/news/20119/new-features-of-java-7](http://www.oschina.net/news/20119/new-features-of-java-7)

**旧一篇：[Kohana 3.2 分支发布](http://www.oschina.net/news/20118/kohana-3-2) 2年前**
**新一篇：[程序员越老越优秀吗？](http://www.oschina.net/news/20120/are-older-people-better) 2年前**
**相关资讯**

* [Apache UIMA Java SDK 2.4.1 发布...](http://www.oschina.net/news/42676/apache-uima-java-sdk-2-4-1 "Apache UIMA Java SDK 2.4.1 发布")2天前
* [Java Web快速开发平台 WebBuilder 6...](http://www.oschina.net/news/42651 "Java Web快速开发平台 WebBuilder 6.8 发布")3天前
* [Spring AMQP 1.2.0 for Java 发布...](http://www.oschina.net/news/42295/spring-amqp-1-2-0-for-java "Spring AMQP 1.2.0 for Java 发布")15天前
* [fastjson 1.1.33 发布，Java 的 JS...](http://www.oschina.net/news/42225/fastjson-1-1-33 "fastjson 1.1.33 发布，Java 的 JSON 库")18天前
* [DEMUX Framework 0.6.1 发布，Java ...](http://www.oschina.net/news/42112/demux-framework-0-6-1 "DEMUX Framework 0.6.1 发布，Java 框架")22天前
* [Aspose.Email for Java 3.1.0 发布...](http://www.oschina.net/news/42092/aspose-email-for-java-3-1-0 "Aspose.Email for Java 3.1.0 发布")23天前
* [MariaDB Java Client 1.1.3 发布...](http://www.oschina.net/news/41946/mariadb-java-client-1-1-3 "MariaDB Java Client 1.1.3 发布")29天前
* [HTTL 1.0.10 版本发布，Java 模版引...](http://www.oschina.net/news/41926/httl-1-0-10 "HTTL 1.0.10 版本发布，Java 模版引擎")29天前
* [Beetl 1.24 发布，java模板引擎...](http://www.oschina.net/news/41916/beetl-1-24 "Beetl 1.24 发布，java模板引擎")29天前
* [FreeMarker 2.3.20 发布，Java 模板...](http://www.oschina.net/news/41863/freemarker-2-3-20 "FreeMarker 2.3.20 发布，Java 模板引擎")1个月前
 **相关讨论话题**

* [这样的状态不知道该不该留下来，求指教](http://www.oschina.net/question/1177024_120104 "这样的状态不知道该不该留下来，求指教")15小时前
* [快速排序，眉庄说这个思路是极好的，...](http://www.oschina.net/question/571225_120116 "快速排序，眉庄说这个思路是极好的，可惜就是死了个循环TUT（java）")14小时前
* [每个职业都是有天花板的](http://www.oschina.net/question/817133_119906 "每个职业都是有天花板的")2天前
* [eclipse怎么设置中文版？](http://www.oschina.net/question/1170472_120132 "eclipse怎么设置中文版？")13小时前
* [android开发界面有什么好用的拖拽工...](http://www.oschina.net/question/49614_90267 "android开发界面有什么好用的拖拽工具")5个月前
* [两张大表对比找出差集](http://www.oschina.net/question/944819_120014 "两张大表对比找出差集")昨天(20:05)
* [疯狂JAVA里这段代码为什么first.cou...](http://www.oschina.net/question/571225_119905 "疯狂JAVA里这段代码为什么first.count=3删除成功，为20就不成功了呢？")2天前
* [有没有像Highcharts,D3这样的可视化...](http://www.oschina.net/question/43796_120102 "有没有像Highcharts,D3这样的可视化java包")15小时前
* [java 操作ldap, 怎么修改objectcla...](http://www.oschina.net/question/137332_120098 "java 操作ldap, 怎么修改objectclass ， 或者定义自己的objectclass")15小时前
* [java计算大文件的md5，有什么快速方...](http://www.oschina.net/question/1050447_120054 "java计算大文件的md5，有什么快速方法吗")20小时前

**[X]()你也许会喜欢**

* [Apache UIMA Java SDK 2.4.1 发布](http://www.oschina.net/news/42676/apache-uima-java-sdk-2-4-1 "Apache UIMA Java SDK 2.4.1 发布")2天前
* [Java Web快速开发平台 WebBuilder 6.8 发布...](http://www.oschina.net/news/42651 "Java Web快速开发平台 WebBuilder 6.8 发布")3天前
* [Spring AMQP 1.2.0 for Java 发布](http://www.oschina.net/news/42295/spring-amqp-1-2-0-for-java "Spring AMQP 1.2.0 for Java 发布")15天前

## [回到顶部]()[发表评论]() 网友评论，共 82 条

* [![ddatsh]( "ddatsh")](http://my.oschina.net/zhangthe9) 1楼：**dd**发表于 2011-07-27 23:04 [回复此评论]()

目前不关心
* [![梅公子]( "梅公子")](http://my.oschina.net/seamanmei) 2楼：**幽幽** 发表于 2011-07-27 23:06 [回复此评论]()

我不想做小白鼠啊~
* [![haorizi]( "haorizi")](http://my.oschina.net/haorizi) 3楼：**haorizi**发表于 2011-07-27 23:08 [回复此评论]()

鼓掌！！！
* [![CheckStyle]( "CheckStyle")](http://my.oschina.net/WilliamShen) 4楼：**CheckStyle**发表于 2011-07-28 00:14 [回复此评论]()

有几项还是非常值得期待的
* [![穿着马甲的鸟]( "穿着马甲的鸟")](http://my.oschina.net/u/158986) 5楼：**穿着马甲的鸟** 发表于 2011-07-28 05:14 [回复此评论]()

期待做小白鼠。。。
* [![一刀]( "一刀")](http://my.oschina.net/yidao620c) 6楼：**一刀** 发表于 2011-07-28 06:47 [回复此评论]()

不错不错~~ JAVA加油
* [![景德真人]( "景德真人")](http://my.oschina.net/szpengvictor) 7楼：**景德真人** 发表于 2011-07-28 07:15 [回复此评论]()

又要努力学习了
* [![jazz]( "jazz")](http://my.oschina.net/jiahut) 8楼：**jazz**发表于 2011-07-28 08:42 [回复此评论]()

都是些语法糖，用工具类有时候做的可能比这更好
* [![heronote]( "heronote")](http://my.oschina.net/heronote) 9楼：**heronote**发表于 2011-07-28 08:46 [回复此评论]()

2000年以前Java走在其他语言之前（虚拟执行的概念以及隔离底层变化的机制）
2005年以前Java和追上来的语言一起前行
2005年以后Java......
2005年以前做过几个Java的项目，包括B/S和桌面应用，从语言机制上说Exception机制很烦人，从类库上说太一般了，尤其是UI库和IO库：So Stupid，从打包机制来说浪费了大量的时间，从配置来说.....
* [![douglarek]( "douglarek")](http://my.oschina.net/lcx2007) 10楼：**Java行者** 发表于 2011-07-28 10:03 [回复此评论]()

### 引用来自“heronote”的评论

2000年以前Java走在其他语言之前（虚拟执行的概念以及隔离底层变化的机制）
2005年以前Java和追上来的语言一起前行
2005年以后Java......
2005年以前做过几个Java的项目，包括B/S和桌面应用，从语言机制上说Exception机制很烦人，从类库上说太一般了，尤其是UI库和IO库：So Stupid，从打包机制来说浪费了大量的时间，从配置来说.....
哥们，貌似你Java很厉害阿，不过看到你最后一段的评论，我还是感觉你太肤浅了，你说的Exception很烦人，我想问你哪种主流编程语言没有这一项？你说UI的设计一般，你也做个跨平台的UI类库，你说I/O库一般，正说明你连Java还不入门，I/O库是Java实现的接近完美的一个类库！打包机制浪费了时间，别跟我说这些没用的，那还不是为了方便程序员阿？
* [![虫虫]( "虫虫")](http://my.oschina.net/zhlmmc) 11楼：**虫虫** 发表于 2011-07-28 11:56 [回复此评论]()

鼓掌！！！
* [![虫虫]( "虫虫")](http://my.oschina.net/zhlmmc) 12楼：**虫虫** 发表于 2011-07-28 12:05 [回复此评论]()

### 引用来自“Java行者”的评论

### 引用来自“heronote”的评论

2000年以前Java走在其他语言之前（虚拟执行的概念以及隔离底层变化的机制）
2005年以前Java和追上来的语言一起前行
2005年以后Java......
2005年以前做过几个Java的项目，包括B/S和桌面应用，从语言机制上说Exception机制很烦人，从类库上说太一般了，尤其是UI库和IO库：So Stupid，从打包机制来说浪费了大量的时间，从配置来说.....
哥们，貌似你Java很厉害阿，不过看到你最后一段的评论，我还是感觉你太肤浅了，你说的Exception很烦人，我想问你哪种主流编程语言没有这一项？你说UI的设计一般，你也做个跨平台的UI类库，你说I/O库一般，正说明你连Java还不入门，I/O库是Java实现的接近完美的一个类库！打包机制浪费了时间，别跟我说这些没用的，那还不是为了方便程序员阿？

这跟选老婆是一样的，各花入各眼。讨论技术还是不要扯到人身攻击上了……
* [![CheckStyle]( "CheckStyle")](http://my.oschina.net/WilliamShen) 13楼：**CheckStyle**发表于 2011-07-28 12:28 [回复此评论]()

### 引用来自“Java行者”的评论

### 引用来自“heronote”的评论

2000年以前Java走在其他语言之前（虚拟执行的概念以及隔离底层变化的机制）
2005年以前Java和追上来的语言一起前行
2005年以后Java......
2005年以前做过几个Java的项目，包括B/S和桌面应用，从语言机制上说Exception机制很烦人，从类库上说太一般了，尤其是UI库和IO库：So Stupid，从打包机制来说浪费了大量的时间，从配置来说.....
哥们，貌似你Java很厉害阿，不过看到你最后一段的评论，我还是感觉你太肤浅了，你说的Exception很烦人，我想问你哪种主流编程语言没有这一项？你说UI的设计一般，你也做个跨平台的UI类库，你说I/O库一般，正说明你连Java还不入门，I/O库是Java实现的接近完美的一个类库！打包机制浪费了时间，别跟我说这些没用的，那还不是为了方便程序员阿？

人家是指Checked Exception很烦人.
Checked Exception目前我所知道,Java是唯一使用的语言.而那些后续更先进的语言,都没加入这个feature. 我也认为这是Java的败笔.
* [![CheckStyle]( "CheckStyle")](http://my.oschina.net/WilliamShen) 14楼：**CheckStyle**发表于 2011-07-28 12:29 [回复此评论]()

### 引用来自“Java行者”的评论

### 引用来自“heronote”的评论

2000年以前Java走在其他语言之前（虚拟执行的概念以及隔离底层变化的机制）
2005年以前Java和追上来的语言一起前行
2005年以后Java......
2005年以前做过几个Java的项目，包括B/S和桌面应用，从语言机制上说Exception机制很烦人，从类库上说太一般了，尤其是UI库和IO库：So Stupid，从打包机制来说浪费了大量的时间，从配置来说.....
哥们，貌似你Java很厉害阿，不过看到你最后一段的评论，我还是感觉你太肤浅了，你说的Exception很烦人，我想问你哪种主流编程语言没有这一项？你说UI的设计一般，你也做个跨平台的UI类库，你说I/O库一般，正说明你连Java还不入门，I/O库是Java实现的接近完美的一个类库！打包机制浪费了时间，别跟我说这些没用的，那还不是为了方便程序员阿？

你的水平不如被你拍的人. I/O完美么? Windows平台的Java,连NIO都没真正实现, 你敢说完美?
* [![Gmail.com]( "Gmail.com")](http://my.oschina.net/zantesu) 15楼：**zantesu**发表于 2011-07-28 13:10 [回复此评论]()

我觉得Java的几个无法忍受的缺陷
不是纯面向对象的
不支持泛型
不支持属性
不支持匿名方法
不支持方法对象传递
不支持yield机制
不支持lambda表达式
不支持类型推断
还有就是上面提到的Checked Exception
-----
为什么Java的lambda表达式迟迟不能实装,估计就是因为Checked Exception的缘故
为什么总是有人说建议学习Python/Ruby,就是相比落后的Java,Python/Ruby的编程思维,生产力能得到极大的解放
* [![douglarek]( "douglarek")](http://my.oschina.net/lcx2007) 16楼：**Java行者** 发表于 2011-07-28 13:40 [回复此评论]()

### 引用来自“CheckStyle”的评论

### 引用来自“Java行者”的评论

### 引用来自“heronote”的评论

2000年以前Java走在其他语言之前（虚拟执行的概念以及隔离底层变化的机制）
2005年以前Java和追上来的语言一起前行
2005年以后Java......
2005年以前做过几个Java的项目，包括B/S和桌面应用，从语言机制上说Exception机制很烦人，从类库上说太一般了，尤其是UI库和IO库：So Stupid，从打包机制来说浪费了大量的时间，从配置来说.....
哥们，貌似你Java很厉害阿，不过看到你最后一段的评论，我还是感觉你太肤浅了，你说的Exception很烦人，我想问你哪种主流编程语言没有这一项？你说UI的设计一般，你也做个跨平台的UI类库，你说I/O库一般，正说明你连Java还不入门，I/O库是Java实现的接近完美的一个类库！打包机制浪费了时间，别跟我说这些没用的，那还不是为了方便程序员阿？

你的水平不如被你拍的人. I/O完美么? Windows平台的Java,连NIO都没真正实现, 你敢说完美?

"从JDK 1.4开始，Java的标准库中就包含了NIO，即所谓的“New IO”。其中最重要的功能就是提供了“非阻塞”的IO，当然包括了Socket。NonBlocking的IO就是对select(Unix平台下)以及 WaitForMultipleObjects(Windows平台)的封装，提供了高性能、易伸缩的服务架构。" 你自己看吧，别跟我说没用的，我平时不用windows的。
* [![douglarek]( "douglarek")](http://my.oschina.net/lcx2007) 17楼：**Java行者** 发表于 2011-07-28 13:48 [回复此评论]()

### 引用来自“CheckStyle”的评论

### 引用来自“Java行者”的评论

### 引用来自“heronote”的评论

2000年以前Java走在其他语言之前（虚拟执行的概念以及隔离底层变化的机制）
2005年以前Java和追上来的语言一起前行
2005年以后Java......
2005年以前做过几个Java的项目，包括B/S和桌面应用，从语言机制上说Exception机制很烦人，从类库上说太一般了，尤其是UI库和IO库：So Stupid，从打包机制来说浪费了大量的时间，从配置来说.....
哥们，貌似你Java很厉害阿，不过看到你最后一段的评论，我还是感觉你太肤浅了，你说的Exception很烦人，我想问你哪种主流编程语言没有这一项？你说UI的设计一般，你也做个跨平台的UI类库，你说I/O库一般，正说明你连Java还不入门，I/O库是Java实现的接近完美的一个类库！打包机制浪费了时间，别跟我说这些没用的，那还不是为了方便程序员阿？

人家是指Checked Exception很烦人.
Checked Exception目前我所知道,Java是唯一使用的语言.而那些后续更先进的语言,都没加入这个feature. 我也认为这是Java的败笔.

我想问一下，你怎么知道他指的是“Checked Exception”，你是他的马甲还是什么？至于你说的那些后续的语言都没有加进什么所谓的feature，那我想问你，Java发明之初，她能设计的那么齐全和周到吗？你可能会问，他为什么部把这个feature去掉，现在没法去了，兼容性是个大问题！还有那我问你，C语言没有运用面向对象的思想，是不是也是C语言的败笔？说了那么多，总而言之，言而总之就是：上面的那位哥们他明显的在评论语言的优劣，而我从没有说过哪种语言设计的非常好，我从没有比较过哪种语言谁好谁差，这种比较本身就是一种无知的表现！
* [![SudyX]( "SudyX")](http://my.oschina.net/free) 18楼：**SudyX**发表于 2011-07-28 14:43 [回复此评论]()

### 引用来自“zantesu”的评论

我觉得Java的几个无法忍受的缺陷
不是纯面向对象的
不支持泛型
不支持属性
不支持匿名方法
不支持方法对象传递
不支持yield机制
不支持lambda表达式
不支持类型推断
还有就是上面提到的Checked Exception
-----
为什么Java的lambda表达式迟迟不能实装,估计就是因为Checked Exception的缘故
为什么总是有人说建议学习Python/Ruby,就是相比落后的Java,Python/Ruby的编程思维,生产力能得到极大的解放
没用过就别发言...
* [![非会员用户]( "非会员用户")]( "yissyo") 19楼：**yissyo**发表于 2011-07-28 15:00 (非会员)

鼓掌围观高手
* [![Gmail.com]( "Gmail.com")](http://my.oschina.net/zantesu) 20楼：**zantesu**发表于 2011-07-28 15:10 [回复此评论]()

### 引用来自“SudyX”的评论

### 引用来自“zantesu”的评论

我觉得Java的几个无法忍受的缺陷
不是纯面向对象的
不支持泛型
不支持属性
不支持匿名方法
不支持方法对象传递
不支持yield机制
不支持lambda表达式
不支持类型推断
还有就是上面提到的Checked Exception
-----
为什么Java的lambda表达式迟迟不能实装,估计就是因为Checked Exception的缘故
为什么总是有人说建议学习Python/Ruby,就是相比落后的Java,Python/Ruby的编程思维,生产力能得到极大的解放
没用过就别发言...

敢问你来解释解释?还是就只有你用过Java么?

* [1](http://www.oschina.net/news/20119/?p=1)
* [2](http://www.oschina.net/news/20119/?p=2#comments)
* [3](http://www.oschina.net/news/20119/?p=3#comments)
* [4](http://www.oschina.net/news/20119/?p=4#comments)
* [5](http://www.oschina.net/news/20119/?p=5#comments)
* [>](http://www.oschina.net/news/20119/?p=2#comments)
[]()                            
                                     

     [](http://www.51idc.com/activit/sale_hulan.html "租服务器，就上51IDC")       

网名： (必填) 邮箱： (必填，不公开) 网址：
文明上网，理性发言

[]() []()[]()
![]()
