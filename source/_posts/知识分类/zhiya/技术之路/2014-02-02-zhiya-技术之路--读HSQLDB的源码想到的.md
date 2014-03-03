---
layout: post
title: "读HSQLDB的源码想到的"
categories: zhiya
tags: 
 - zhiya
 - 技术之路
--- 

# 读HSQLDB的源码想到的

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

### [读HSQLDB的源码想到的]() **

**博客分类：**
* [IT技术](http://zwchen.iteye.com/category/9634)
[HSQLDB](http://www.iteye.com/blogs/tag/HSQLDB)[JDBC](http://www.iteye.com/blogs/tag/JDBC)[MySQL](http://www.iteye.com/blogs/tag/MySQL)[Socket](http://www.iteye.com/blogs/tag/Socket)[Tomcat](http://www.iteye.com/blogs/tag/Tomcat)

昨天在论坛看到一篇讨论嵌入式数据库HSQLDB([http://www.iteye.com/topic/79802](http://www.iteye.com/topic/79802))的帖子，想到自己曾经读过部分它的源码，有一种对某些技术豁然开朗的感觉。所以，也希望和朋友们一起分享，大家有什么好的感受，不如也分享一下吧。下面是我对那个帖子的冗余回复，我觉得有必要专门发一篇帖子重复一下：
说点题外话，建议大家读读HSQLDB的源码，特别是jdbc driver（**org/hsqldb/jdbc包**）那部分，写得清晰易懂。读了它的部分源码，我自认为对下面一些问题理解深入了：
1、JDBC规范和JDBC实现的关系：怎么自己去设计一个规范，一种架构？我是否自己可以为某种数据设计jdbc driver，如何设计？想想php里面各数据库的函数库各自为政对程序移植性的影响，就知道jdbc规范有多么重要了。
2、JDBC协议：JDBC是基于socket之上的，数据包格式（**org.hsqldb.Result**)（mysql数据包格式公开了）？那么JMS数据包呢？其实，这也可以延伸到分布式协议的设计原理，如RMI、SOAP。其实，这些数据包格式和JSON、YAML这些message格式没有本质的区别，只不过应用范围不一样。任何分布式协议，肯定有一种message格式。
3、JDBC over HTTP：这样我们对RMI over IIOP, soap over HTTP, http tunnel原理有更深入的理解。
4、什么是long connection（jdbc的socket)，什么是short connection（http)，具体怎么实现？
3和4这些在HSQLDB的**org.hsqldb.HTTPClientConnection**类里有实现。
5、Java客户端和服务器端的通讯实现：jdbc driver就可以认为是一个java客户端类库。那么JMS client呢？还有，像mysql有各种语言的driver，原理是什么。
6、sql这种command、描述型语言究竟在数据库里面是个什么地位：sql是怎么传入jdbc driver，最终和database交互的？我们是否可以设计出另外一种command，形成一种行业标准，它在服务器和客户端怎么实现的。
以上我的表达可能有些晦涩，我只想表达一点：大家有兴趣就多读读经典的源码，扩展一下自己的设计思路。可能很多人象我一样，总有忙不完的项目，那么抽几个小时就够了，不必深入。
有很多技术我们理解总是很模糊，当你深入到内部，忽然发现原来就这么回事。我们总觉得IoC很神秘，其实最简单的IoC容器，也许一个HashMap就够了。
分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[也说说项目成败、企业信息化](http://zwchen.iteye.com/blog/81109 "也说说项目成败、企业信息化") | [Web Services开发体会和项目教训](http://zwchen.iteye.com/blog/73047 "Web Services开发体会和项目教训")
* 2007-05-17 10:36
* 浏览 7050
* [评论(15)]()
* 论坛回复 / [浏览](http://zwchen.iteye.com/topic/80532) (13 / 10010)
* [相关推荐](http://www.iteye.com/wiki/blog/80532)

### 评论

[]()

15 楼 [zwchen](http://zwchen.iteye.com/ "zwchen") 2011-08-20  

realdah 写道

楼主除了java用过别的编程语言吗?是否熟悉TCP/IP等协议?
挺奇怪为啥有如此大的感想..
我曾经啃过那本《TCP/IP技术内幕》，也用sniffer探测过TCP、FTP等数据包，开发过BT服务器，但具体到语言实现的层面，还是看这些源码才恍然大悟的。
14 楼 [realdah](http://realdah.iteye.com/ "realdah") 2007-11-02  

楼主除了java用过别的编程语言吗?是否熟悉TCP/IP等协议?
挺奇怪为啥有如此大的感想..

13 楼 [zwchen](http://zwchen.iteye.com/ "zwchen") 2007-11-02  

javachs 写道

我看了你关于hsqldb的帖子，讲到hsqldb源码里有关于数据库long connection和short connection的实现，我看了下不太明白，数据库的long connection如何保持连接一直不太明白，能讲一下关于数据库长连接吗？
1、数据库的connection保持也就是TCP的连接保持，也就是TCP里面的Keepalive选项：在Socket编程里面的Keepalive。在Java的Socket类中就专门说到SO_KEEPALIVE：[http://download.java.net/jdk7/docs/api/java/net/SocketOptions.html](http://download.java.net/jdk7/docs/api/java/net/SocketOptions.html)，
顺便说一下，在TCP通信期间，是靠sequence number来维持的，它们由initial sequence number (ISN）递增，它是keepAlive的前提。
2、在hsqldb里面，是默认用Java socket的，自己本身并没有去实现long connection，也就是说这个连接默认是2小时，反正我也没有找到这个选项设置的地方。在其HTTPClientConnection里面，倒是每次execute sql后就close了，遵从http协议的short connection实现。附带说一下，在hsqldb里面，是通过sessionID来标识客户端的（（这个sessionID可以保证认证后的安全sql），就如同http里面的cookie中的JSESSIONID。
3、我以前做过即时通讯的开发，在OpenFire里面，keepAlive是自己实现的，也就是server端有个**demo**线程，默认每60s给所有客户端发送一个检测包，如org.jivesoftware.openfire.net包下的SocketConnection的**checkHealth**() 注释：
    /**
     * Returns true if the socket was closed due to a bad health. The socket is considered to
     * be in a bad state if a thread has been writing for a while and the write operation has
     * not finished in a long time or when the client has not sent a heartbeat for a long time.
     * In any of both cases the socket will be closed.
     *
     * @return true if the socket was closed due to a bad health.s
     */
顺便说一下，现在的ajax的push技术（comet）（譬如web版的gtalk、msn），也主要利用http的keepalive选项，还有个专门的协议扩展：[http://www.xmpp.org/extensions/xep-0124.html](http://www.xmpp.org/extensions/xep-0124.html)
如果你有兴趣，可以看看《TCP/IP Illustrated, Volume 1: The Protocols》的Chapter 23. TCP Keepalive Timer，文中说到Keepalive是TCP里面的一个可选项，一般由application层实现，并且给出了理由。
12 楼 [lihy70](http://lihy70.iteye.com/ "lihy70") 2007-07-02  

leebai 写道

楼主揭示出了学习编程的正正之道！
往上看，多是浮躁的、流行一时的东西；往下看，基本上都是沉淀的精华。
作为真正的开发者，研究和学习各种服务器和JDK的原代码，比学习各种框架、ORMaping、J2EE乱七八糟的API要有意义得多。
icefire 写道

有时候觉得，迷惑的时候，读源码比看API来得快！
**写源代码比看源代码容易，有时候**
*     －－接上一句，典型的程序员。*
读一辈子代码也不一定能看出个微积分，还是看高数吧
不知道那位虾客能抽出一段代码来摆个擂台？

11 楼 [hideto](http://hideto.iteye.com/ "hideto") 2007-07-02  

icefire 写道

有时候觉得，迷惑的时候，读源码比看API来得快！
hand!![]()
10 楼 [lando](http://lando.iteye.com/ "lando") 2007-07-02  

感同身受！

9 楼 [lordhong](http://lordhong.iteye.com/ "lordhong") 2007-05-17  

潜力精华贴...拜读...收藏...
8 楼 [icefire](http://icefire.iteye.com/ "icefire") 2007-05-17  

有时候觉得，迷惑的时候，读源码比看API来得快！

7 楼 [leebai](http://leebai.iteye.com/ "leebai") 2007-05-17  

楼主揭示出了学习编程的正正之道！
往上看，多是浮躁的、流行一时的东西；往下看，基本上都是沉淀的精华。
作为真正的开发者，研究和学习各种服务器和JDK的原代码，比学习各种框架、ORMaping、J2EE乱七八糟的API要有意义得多。
6 楼 [shaucle](http://shaucle.iteye.com/ "shaucle") 2007-05-17  

写得很不错
很多源码看了后阔然开朗．
而且很有意思，只是有的要多看几遍．
推荐一些较好的源码(有的正在看）
Seam
Web Work
Hibernate(8)
Pico Container
Lucene
TreeCache
Spring(12)
Hsqldb
db4o
jboss(N)
geronimo
activeMQ
Compass
Equinox
jboss-esb/mule
Asm
Jetty
Terracotta
Jencks

5 楼 [yatwql](http://yatwql.iteye.com/ "yatwql") 2007-05-17  

粗略扫过一些hsqldb和h2database的代码，觉得h2database写得更干净一些
4 楼 [dennis_zane](http://dennis-zane.iteye.com/ "dennis_zane") 2007-05-17  

zwchen 写道

dennis_zane 写道

我最近在读hibernate2.1的源码，spring的源码读了核心的IOC和AOP后就没去看了，听了你的介绍，也把HSQLDB列入计划：）
按照您的意见，最好是与jdbc规范一起解读？
jdbc只是告诉厂商去实现那些api，如Statement就够了，这个学的东西还是有限，因为sun并没有告诉我们怎么去实现一个driver，也就是jdbc api接口的实现。
不过，我觉得对照servlet规范，看一些最简单的servlet容器实现，如
[url]http://www.onjava.com/pub/a/onjava/2003/05/14/java_webserver.html [/url]  
[url]http://tomcat.apache.org/tomcat-5.5-doc/architecture/index.html [/url]
可能收获更大。
看jdbc驱动，先去看这本书吧，mysql的实现，特别是关于MySQL Client/Server Protocol那章：[http://forge.mysql.com/wiki/MySQL_Internals](http://forge.mysql.com/wiki/MySQL_Internals)
这本书看来难度相当大。对照serlvet规范，读简单的servlet容器的实现是很好的建议，谢谢。

3 楼 [zwchen](http://zwchen.iteye.com/ "zwchen") 2007-05-17  

dennis_zane 写道

我最近在读hibernate2.1的源码，spring的源码读了核心的IOC和AOP后就没去看了，听了你的介绍，也把HSQLDB列入计划：）
按照您的意见，最好是与jdbc规范一起解读？
jdbc只是告诉厂商去实现那些api，如Statement就够了，这个学的东西还是有限，因为sun并没有告诉我们怎么去实现一个driver，也就是jdbc api接口的实现。
不过，我觉得对照servlet规范，看一些最简单的servlet容器实现，如
[url]http://www.onjava.com/pub/a/onjava/2003/05/14/java_webserver.html [/url]  
[url]http://tomcat.apache.org/tomcat-5.5-doc/architecture/index.html [/url]
可能收获更大。
看jdbc驱动，先去看这本书吧，mysql的实现，特别是关于MySQL Client/Server Protocol那章：[http://forge.mysql.com/wiki/MySQL_Internals](http://forge.mysql.com/wiki/MySQL_Internals)
2 楼 [zwchen](http://zwchen.iteye.com/ "zwchen") 2007-05-17  

顺便说一下，现在我们项目组正在用Jive公司的Openfire＋spark做即时通讯方案，其实，研究那种XMPP（Jabber）协议，和jdbc、JMS并没有什么本质的区别。想想曾经看过部分Tomcat的源码，觉得也差不多，只是它走标准的http，也就是发http数据包让服务器处理，而即时通讯客户端，如gtalk，是发XMPP数据包给服务器处理。你看即时通讯的应用和我们的web程序有多少相似之处：处理库存查询：[http://www.activesoft.com.cn/pipeintrokc.asp](http://www.activesoft.com.cn/pipeintrokc.asp) 。网上那些msn、QQ机器人也就是这样的模式。

1 楼 [dennis_zane](http://dennis-zane.iteye.com/ "dennis_zane") 2007-05-17  

我最近在读hibernate2.1的源码，spring的源码读了核心的IOC和AOP后就没去看了，听了你的介绍，也把HSQLDB列入计划：）
按照您的意见，最好是与jdbc规范一起解读？
### 发表评论

[![]()](http://zwchen.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://zwchen.iteye.com/login)

[![zwchen的博客]( "zwchen的博客: zwchen的博客")](http://zwchen.iteye.com/)

zwchen

* 浏览: 411244 次
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
![](http://stat.iteye.com/?url=http%3A%2F%2Fzwchen.iteye.com%2Fblog%2F80532&referrer=http%3A%2F%2Fzwchen.iteye.com%2Fcategory%2F9634&user_id=)
