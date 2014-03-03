---
layout: post
title: "JVM知识点题目"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# JVM知识点题目

# [BlueDavy之技术Blog](http://www.blogjava.net/BlueDavy/)

理论不懂就实践，实践不会就学理论！

## [JVM知识点题目](http://www.blogjava.net/BlueDavy/archive/2009/03/27/262419.html)

JVM是Java程序的运行环境，因此对于JVM的掌握有助于理解Java程序的执行以及编写，尤其是运行时碰到的一些诡异问题，那么怎么样能考察自己对于JVM关键知识点的掌握情况，帮助学习JVM机制呢，在这篇blog中来探讨下。
对于Java程序而言，JVM的关键机制有：字节码的加载、方法的执行、对象内存的分配和回收、线程和锁机制，这几个机制涉及到的jvm的知识点远没有写这几个字这么简单，里面的复杂度还是非常高的。
字节码的加载
JVM通过ClassLoader来完成字节码的动态加载，这里面涉及到的主要是ClassLoader的双亲委派、ClassLoader的编写方法、Class是否被加载的唯一标识以及Class的加载过程。
在考察的时候我觉得可以以这么两道简单的题来考察：
1、写一段将目录中指定的.class文件加载到JVM的程序，并通过Class对象获取到完整类名等信息；
2、一段展示代码，里面包含一个全局静态整型变量，问如果用两个ClassLoader加载此对象，执行这个整型变量++操作后结果会是怎么样的？
方法的执行
JVM有自己的一套指令系统，字节码中即已经是指令了，需要大概掌握了JVM对static、interface、instance、构造器采用的不同的执行方法，另外就是JVM中反射的实现（可以以Sun JDK来举例）、动态代理的实现，最后相关的就是JVM执行字节码的方式（解释、JIT、Hotspot），以及什么时候触发编译成机器码，如何控制。
在考察的时候我觉得可以以这么三道题来考察：
1、A a=new A();a.execute();和IA a=new A();a.execute();执行有什么不同；
2、反射的性能低的原因是？
3、编写一段程序，动态的创建一个接口的实现，并加载到JVM中执行；（可以允许用BCEL等工具）
对象内存的分配和回收
这块涉及的知识点也是比较的多，例如JVM内存区域的划分、自然类型和引用类型的内存分配的不同、TLAB、GC的算法、Sun JDK对于GC的实现、GC触发的时机、GC的跟踪和分析的方法。
在考察的时候我觉得可以以这么三道题来考察：
1、经典的String比较程序题：
   String a="a";
   String b="b";
   String ab="ab";
   (a+b)==ab;  ??  (引深题，如何才能让(a+b)==ab）
   ("a"+"b")==ab; ?? 
2、写一段程序，让其OutOfMemory，或频繁执行Minor GC，但又不触发Full GC，又或频繁执行Full GC，但不执行minor GC，而且不OutOfMemory，甚至可以是控制几次Minor GC后发生一次Full GC；
3、详细讲解GC的实现，例如minor GC的时候导致是怎么回收对象内存的，Full GC的时候是怎么回收对象内存的。
线程和锁机制
这块涉及的知识点仍然是非常的多，例如线程中变量的操作机制、线程调度机制、线程的状态以及控制方法、线程的跟踪和分析方法、同步关键字、lock/unlock的原理等。
在考察的时候我觉得可以以这么几道题考察下：
1、i++的执行过程；
2、一个线程需要等待另外一个线程将某变量置为true才继续执行，如何编写这段程序，或者如何控制多个线程共同启动等；
3、控制线程状态的转换的方法，或者给几个thread dump，分析下哪个线程有问题，问题出在哪；
4、static属性加锁、全局变量属性加锁、方法加锁的不同点？

posted on 2009-03-27 14:30 [BlueDavy](http://www.blogjava.net/BlueDavy/) 阅读(7897) [评论(9)]()  [编辑](http://www.blogjava.net/BlueDavy/admin/EditPosts.aspx?postid=262419)  [收藏](http://www.blogjava.net/BlueDavy/AddToFavorite.aspx?id=262419) 所属分类: [Java](http://www.blogjava.net/BlueDavy/category/1366.html)

 ![]()

[]() []()

[
### 评论

]()

### []()[#]( "permalink: re: JVM知识点题目") []()re: JVM知识点题目  2009-03-27 15:28  [flyisland](http://flyisland.blogbus.com/)

希望不是你当面试官，哈  [回复]()  [更多评论](http://www.blogjava.net/comment?author=flyisland "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: JVM知识点题目") []()re: JVM知识点题目  2009-03-27 16:05  [云襄](http://blog.goodtiger.net/)

JVM的东西的确比较复杂  [回复]()  [更多评论](http://www.blogjava.net/comment?author=%e4%ba%91%e8%a5%84 "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: JVM知识点题目") []()re: JVM知识点题目  2009-03-27 19:27  [问问]()

都不是很懂，什么时候贴答案啊？  [回复]()  [更多评论](http://www.blogjava.net/comment?author=%e9%97%ae%e9%97%ae "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: JVM知识点题目[未登录]") []()re: JVM知识点题目[未登录]  2009-03-27 20:15  [逝水fox]()

看了问题 很想知道答案 看书去咯  [回复]()  [更多评论](http://www.blogjava.net/comment?author=%e9%80%9d%e6%b0%b4fox "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: JVM知识点题目[未登录]") []()re: JVM知识点题目[未登录]  2009-03-28 15:55  [kimi]()

关于ClassLoader的两个问题
第一个题目把文件系统中的.class文件读到一个bute数组中，使用ClassLoader的defineClass方法把这个byte数组传递给jvm，则返回一个Class对象，通过这个Class对象可以做关于一个对象的任何事情，但defineClass方法是protected的，所以要写一个ClassLoader的子类，也就是实现一个自己的ClassLoader。
第二个题目由于Java中ClassLoader本质上是定义了一个Class的集合，而且ClassLoader之间是项目隔离的，除非它们之间是父子关系，但即使这样，只能是子ClassLoader能看到父ClassLoader定义的Class，父ClassLoader看不到子ClassLoader定义的Class，所以使用一个ClassLoader加载的类的静态变量执行++操作，对另一个ClassLoader加载的对象的静态变量是没有影响的。
班门弄斧了，还请博主指正我的回答是否正确。  [回复]()  [更多评论](http://www.blogjava.net/comment?author=kimi "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: JVM知识点题目") []()re: JVM知识点题目  2009-05-24 01:51  [ttt]()

老大，给个答案啊，别变成炫耀贴了。  [回复]()  [更多评论](http://www.blogjava.net/comment?author=ttt "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: JVM知识点题目") []()re: JVM知识点题目  2011-12-31 14:33  [whwang]()

这题太狠了：
3、编写一段程序，动态的创建一个接口的实现，并加载到JVM中执行；（可以允许用BCEL等工具）
  [回复]()  [更多评论](http://www.blogjava.net/comment?author=whwang "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: JVM知识点题目") []()re: JVM知识点题目  2012-01-02 00:23  [斌斌]()

将目录里的*.class filter出来，getName一个个来  [回复]()  [更多评论](http://www.blogjava.net/comment?author=%e6%96%8c%e6%96%8c "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: JVM知识点题目") []()re: JVM知识点题目[]()  2012-08-10 17:04  [jb](http://none/)

1.1 findclass
1.2 不同classloader加载的class是不同的class，其实包名与文件名都相同
2.3用asm 框架 writeclass  [回复]()  [更多评论](http://www.blogjava.net/comment?author=jb "查看该作者发表过的评论") []()  []()
[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [推荐购买云服务器（15%返利+最高千元奖金）](http://www.aliyun.com/cps/channel?channel_id=1306&user=0&lv=1)   [博客园](http://www.cnblogs.com/)  [博问](http://q.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![](http://www.blogjava.net/Modules/CaptchaImage/JpegImage.aspx?cacheid=20130727130743) 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/BlueDavy/archive/2009/03/27/262419.html&SourceURL=/BlueDavy/archive/2009/03/27/262419.html)       [使用Ctrl+Enter键可以直接提交]      网站导航:

[博客园](http://www.cnblogs.com/ "程序员的网上家园")   [IT新闻](http://news.cnblogs.com/)   [知识库](http://kb.cnblogs.com/)   [C++博客](http://www.cppblog.com/)   [程序员招聘](http://job.cnblogs.com/)   [管理](http://www.blogjava.net/BlueDavy/archive/2009/03/27/262419.html?opt=admin) 相关文章:

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
