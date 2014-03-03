---
layout: post
title: "Eclipse调试常用技巧"
categories: eclipse
tags: 
 - eclipse
--- 

# Eclipse调试常用技巧

[您还未登录 !](http://www.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://www.javaeye.com/login) [注册](http://www.javaeye.com/signup)

[![JavaEye-最棒的软件开发交流社区]( "JavaEye-最棒的软件开发交流社区")](http://www.javaeye.com/)

[论坛首页](http://www.javaeye.com/forums) → [Java编程和Java企业应用版](http://www.javaeye.com/forums/board/Java) → [Java综合](http://www.javaeye.com/forums/tag/Java) →

# Eclipse调试常用技巧

[全部](http://www.javaeye.com/forums/board/Java) [Hibernate](http://www.javaeye.com/forums/tag/Hibernate) [Spring](http://www.javaeye.com/forums/tag/Spring) [Struts](http://www.javaeye.com/forums/tag/Struts) [iBATIS](http://www.javaeye.com/forums/tag/iBATIS) [企业应用](http://www.javaeye.com/forums/tag/J2EE) [设计模式](http://www.javaeye.com/forums/tag/Design-Pattern) [DAO](http://www.javaeye.com/forums/tag/DAO) [领域模型](http://www.javaeye.com/forums/tag/Object-Domain) [OO](http://www.javaeye.com/forums/tag/OO) [Tomcat](http://www.javaeye.com/forums/tag/Tomcat) [SOA](http://www.javaeye.com/forums/tag/SOA) [JBoss](http://www.javaeye.com/forums/tag/JBoss) [Swing](http://www.javaeye.com/forums/tag/Swing) [Java综合](http://www.javaeye.com/forums/tag/Java)
[](http://www.javaeye.com/forums/39/topics/633824/posts/new "发表回复")

« 上一页 1 [2](http://www.javaeye.com/topic/633824?page=2) [3](http://www.javaeye.com/topic/633824?page=3) … [9](http://www.javaeye.com/topic/633824?page=9) [10](http://www.javaeye.com/topic/633824?page=10) [下一页 »](http://www.javaeye.com/topic/633824?page=2)

浏览 22292 次
 [主题：Eclipse调试常用技巧](http://www.javaeye.com/topic/633824)

**该帖已经被评为精华帖** 作者 正文 * daimojingdeyu
* 等级: ![一星会员]( "一星会员")
* [![daimojingdeyu的博客]( "daimojingdeyu的博客: daimojingdeyu")](http://daimojingdeyu.javaeye.com/)
* 文章: 38
* 积分: 180
* 来自: 山东
* ![]()
    发表时间：2010-04-06   最后修改：2010-05-02

[<](http://www.javaeye.com/topic/633824#) [>](http://www.javaeye.com/topic/633824#) 猎头职位:

相关文章: [ ](http://www.javaeye.com/topic/633824# "关闭")

* [使用 Eclipse 平台进行调试](http://www.javaeye.com/topic/799 "使用 Eclipse 平台进行调试")
* [Eclipse中的断点](http://www.javaeye.com/topic/737692 "Eclipse中的断点")
* [Javascript Debug Toolkit使用说明](http://www.javaeye.com/topic/261463 "Javascript Debug Toolkit使用说明")
推荐圈子: [IBM WebSphere专区](http://ibm-websphere.group.javaeye.com/)
[更多相关推荐](http://www.javaeye.com/wiki/topic/633824)

**本文写给那些像几年前的我一样刚刚走出校门，及一些未使用过这些高级些的调试技巧的人。**

 

 

记得刚刚毕业的时候，自己连断点也不会打，当时还在用JCreate ，就连毕业设计也是用 System.out 找 Bug 的，想想真的很笨。开始工作后，一个星期过去了，在一个 1 、 2 百万行的系统中找 Bug ，我依然在用 System.out ，当时最痛苦的就是修改代码，每次找到疑似 Bug ，就输出一下，然后重启（那时也不知道代码热替换），直到有一天带我的导师发现了这样笨笨的调试 Bug ，才让我第一次认识了断点，也知道了代码修改完了可以进行热替换， 我这个中国教育的半牺牲品才算向美好生活迈进了一小步。

 

 

# 1、 条件断点

断点大家都比较熟悉，在Eclipse Java 编辑区的行头双击就会得到一个断点，代码会运行到此处时停止。

条件断点，顾名思义就是一个有一定条件的断点，只有满足了用户设置的条件，代码才会在运行到断点处时停止。

在断点处点击鼠标右键，选择最后一个"Breakpoint Properties"

![]()

断点的属性界面及各个选项的意思如下图，

![]()

# 2、 变量断点

断点不仅能打在语句上，变量也可以接受断点，

![]()

上图就是一个变量的打的断点，在变量的值初始化，或是变量值改变时可以停止，当然变量断点上也是可以加条件的，和上面的介绍的条件断点的设置是一样的。

# 3、 方法断点

 

方法断点就是将断点打在方法的入口处，

![]()

方法断点的特别之处在于它可以打在 JDK的源码里，由于 JDK 在编译时去掉了调试信息，所以普通断点是不能打到里面的，但是方法断点却可以，可以通过这种方法查看方法的调用栈。

# 4、 改变变量值

代码停在了断点处，但是传过来的值不正确，如何修改一下变量值保证代码继续走正确的流程，或是说有一个异常分支老是进不去，能不能调试时改一下条件，看一下异常分支代码是否正确？

在Debug 视图的 Variables 小窗口中，我们可以看到 mDestJarName 变量的值为 " F:\Study\eclipsepro\JarDir\jarHelp.jar "

![]()

我们可以在变量上右键，选择"Change Value..." 在弹出的对话框中修改变量的值，

![]()

 

或是在下面的值查看窗口中修改，保用Ctr+S 保存后，变量值就会变成修改后的新值了。

![]()

# 5、 重新调试

 

这种调试的回退不是万能的，只能在当前线程的栈帧中回退，也就说最多只能退回到当前线程的调用的开始处。

回退时，请在需要回退的线程方法上点右键，选择 "Drop to Frame"

![]()

# 6、 远程调试

用于调试不在本机上的程序，有两种方式，

1、本机作为客户端

2、本机作为服务端

使用远程调试的前提是服务器端和客户端的代码是一致的。

 

### 本机作为客户端

本机作客户端比较常用，需要在远端的服务器上的java程序在启动时打开远程调试开关，

服务器端需要加上虚拟机参数

1.5以前版本（1.5以后也可用）：【-Xdebug -Xrunjdwp:transport=dt_socket,server=y,address=8000 】

1.5及以上版本：【 -agentlib:jdwp=transport=dt_socket,server=y,address=8000】

F:\Study\eclipsepro\screensnap>java -Xdebug -Xrunjdwp:transport=dt_socket,server=y,address=8000 -jar screensnap3.jar

连接时远程服务器时，需要在Eclipse中新建一个远程调试程序

![]()

这里有一个小地方需注意，连接上的时候貌似不能自动切换到Debug视图，不要以为本机的调试程序没有连接到服务器端。

 

### 本机作为服务端

同本机作为客户端相比，只需要修改一下“Connection Type”

![]()

 

这时Eclipse会进入到等待连接的状态

![]()

连接程序使用如下参数即可连接本机服务器，IP地址请用实现IP替换~~

【-agentlib:jdwp=transport=dt_socket,suspend=y,address=127.0.0.1:8000】

F:\Study\eclipsepro\screensnap>java -agentlib:jdwp=transport=dt_socket,suspend=y,address=127.0.0.1:8000 -jar screensnap3.jar

 

远程调试时本地的代码修改可同步到远程，但不会写到远程的文件里，也就是说本地修改会在下次启动远程程序时就没有了，不会影响到下次使用时的远程代码。

 

有关远程调试更详细点的介绍请参考[【使用 Eclipse 远程调试 Java 应用程序】](http://www.ibm.com/developerworks/cn/opensource/os-eclipse-javadebug/)

 

 

好像漏了一个断点，异常断点，补一下。

# 7、异常断点

经常遇见一些异常，然后程序就退出来了，要找到异常发生的地方就比较难了，还好可以打一个异常断点，

![]()

上图中我们增加了一个NullPointException的异常断点，当异常发生时，代码会停在异常发生处，定位问题时应该比较有帮助。

 

 
* [![]( "点击查看原始大小图片")]()
* 大小: 25.2 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 7.6 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 9.7 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 16.7 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 14.8 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 15.7 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 34.8 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 49 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 13.9 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 16.2 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 12.6 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 32.7 KB

* [查看图片附件](http://www.javaeye.com/topic/633824#)

声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。
推荐链接

* [![adobe]( "adobe")](http://www.javaeye.com/clicks/373) [返回顶楼](http://www.javaeye.com/topic/633824#) [ ](http://daimojingdeyu.javaeye.com/ "浏览作者的博客") [ ](http://daimojingdeyu.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=daimojingdeyu "发送站内短信") [ ](http://daimojingdeyu.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=daimojingdeyu "关注作者")  * 劉罡
* 等级: 初级会员
* [![劉罡的博客]( "劉罡的博客: Never give up , Never too later , Anything is possible !")](http://liu19860404.javaeye.com/)
* 文章: 4
* 积分: 30
* 来自: 上海
* ![]()
    发表时间：2010-04-06  

原来断点调试还可以这样用，学到了，O(∩_∩)O~ [返回顶楼](http://www.javaeye.com/topic/633824#) [ ](http://liu19860404.javaeye.com/ "浏览作者的博客") [ ](http://liu19860404.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E5%8A%89%E7%BD%A1 "发送站内短信") [ ](http://liu19860404.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E5%8A%89%E7%BD%A1 "关注作者") [回帖地址](http://www.javaeye.com/topic/633824#1437983 "劉罡回帖:Eclipse调试常用技巧")

[0](http://www.javaeye.com/topic/633824# "好") [0](http://www.javaeye.com/topic/633824# "差") 请登录后投票  * andrewYe
* 等级: 初级会员
* [![andrewYe的博客]( "andrewYe的博客: andrew`s java blog")](http://andrewye.javaeye.com/)
* 文章: 15
* 积分: 40
* 来自: 成都
* ![]()
    发表时间：2010-04-06  

受教了，期待续 [返回顶楼](http://www.javaeye.com/topic/633824#) [ ](http://andrewye.javaeye.com/ "浏览作者的博客") [ ](http://andrewye.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=andrewYe "发送站内短信") [ ](http://andrewye.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=andrewYe "关注作者") [回帖地址](http://www.javaeye.com/topic/633824#1438760 "andrewYe回帖:Eclipse调试常用技巧")

[0](http://www.javaeye.com/topic/633824# "好") [0](http://www.javaeye.com/topic/633824# "差") 请登录后投票  * JE帐号
* 等级: 初级会员
* [![JE帐号的博客]( "JE帐号的博客: op")](http://oooooooooooooooooooooooooooo.javaeye.com/)
* 文章: 79
* 积分: 0
* 来自: 北京
* [![]()](http://www.javaeye.com/topic/744417 "我正在看《（十）用JAVA编写MP3解码器——逆量化和重排序》")
    发表时间：2010-04-06  

良好贴... [返回顶楼](http://www.javaeye.com/topic/633824#) [ ](http://oooooooooooooooooooooooooooo.javaeye.com/ "浏览作者的博客") [ ](http://oooooooooooooooooooooooooooo.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=JE%E5%B8%90%E5%8F%B7 "发送站内短信") [ ](http://oooooooooooooooooooooooooooo.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=JE%E5%B8%90%E5%8F%B7 "关注作者") [回帖地址](http://www.javaeye.com/topic/633824#1438766 "JE帐号回帖:Eclipse调试常用技巧")

[0](http://www.javaeye.com/topic/633824# "好") [0](http://www.javaeye.com/topic/633824# "差") 请登录后投票  * 喜羊羊与灰太狼
* 等级: 初级会员
* [![喜羊羊与灰太狼的博客]( "喜羊羊与灰太狼的博客: Mario.Copyright")](http://copyright.javaeye.com/)
* 文章: 43
* 积分: 30
* 来自: 成都
* ![]()
    发表时间：2010-04-06  

十分感谢，又长知识了 [返回顶楼](http://www.javaeye.com/topic/633824#) [ ](http://copyright.javaeye.com/ "浏览作者的博客") [ ](http://copyright.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E5%96%9C%E7%BE%8A%E7%BE%8A%E4%B8%8E%E7%81%B0%E5%A4%AA%E7%8B%BC "发送站内短信") [ ](http://copyright.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E5%96%9C%E7%BE%8A%E7%BE%8A%E4%B8%8E%E7%81%B0%E5%A4%AA%E7%8B%BC "关注作者") [回帖地址](http://www.javaeye.com/topic/633824#1438770 "喜羊羊与灰太狼回帖:Eclipse调试常用技巧")

[0](http://www.javaeye.com/topic/633824# "好") [0](http://www.javaeye.com/topic/633824# "差") 请登录后投票  * snail613
* 等级: 初级会员
* [![snail613的博客]( "snail613的博客: ")](http://snail613.javaeye.com/)
* 文章: 1
* 积分: 30
* 来自: 长沙
* ![]()
    发表时间：2010-04-06  

受教了 ，正在为循环语句的调试困惑，感谢 [返回顶楼](http://www.javaeye.com/topic/633824#) [ ](http://snail613.javaeye.com/ "浏览作者的博客") [ ](http://snail613.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=snail613 "发送站内短信") [ ](http://snail613.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=snail613 "关注作者") [回帖地址](http://www.javaeye.com/topic/633824#1438796 "snail613回帖:Eclipse调试常用技巧")

[0](http://www.javaeye.com/topic/633824# "好") [0](http://www.javaeye.com/topic/633824# "差") 请登录后投票  * 李释然
* 等级: 初级会员
* [![李释然的博客]( "李释然的博客: ")](http://hi-hehuan-foxmail-com.javaeye.com/)
* 文章: 2
* 积分: 30
* 来自: 桂阳
* ![]()
    发表时间：2010-04-06  

中国教育的半牺牲品...引人深思啊 [返回顶楼](http://www.javaeye.com/topic/633824#) [ ](http://hi-hehuan-foxmail-com.javaeye.com/ "浏览作者的博客") [ ](http://hi-hehuan-foxmail-com.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E6%9D%8E%E9%87%8A%E7%84%B6 "发送站内短信") [ ](http://hi-hehuan-foxmail-com.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E6%9D%8E%E9%87%8A%E7%84%B6 "关注作者") [回帖地址](http://www.javaeye.com/topic/633824#1438805 "李释然回帖:Eclipse调试常用技巧")

[0](http://www.javaeye.com/topic/633824# "好") [0](http://www.javaeye.com/topic/633824# "差") 请登录后投票  * peanut_sei
* 等级: 初级会员
* [![peanut_sei的博客]( "peanut_sei的博客: Man Month程序人生")](http://man-month.javaeye.com/)
* 文章: 11
* 积分: 0
* 来自: 昆明
* ![]()
    发表时间：2010-04-06  

调试也用了很久，但有些东西还真没太注意，谢谢。 [返回顶楼](http://www.javaeye.com/topic/633824#) [ ](http://man-month.javaeye.com/ "浏览作者的博客") [ ](http://man-month.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=peanut_sei "发送站内短信") [ ](http://man-month.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=peanut_sei "关注作者") [回帖地址](http://www.javaeye.com/topic/633824#1438821 "peanut_sei回帖:Eclipse调试常用技巧")

[0](http://www.javaeye.com/topic/633824# "好") [0](http://www.javaeye.com/topic/633824# "差") 请登录后投票  * constant
* 等级: 初级会员
* [![constant的博客]( "constant的博客: constant")](http://constantztk.javaeye.com/)
* 文章: 47
* 积分: 0
* 来自: 北京
* ![]()
    发表时间：2010-04-06  

远程调试不太了解，能详细讲讲嘛？ [返回顶楼](http://www.javaeye.com/topic/633824#) [ ](http://constantztk.javaeye.com/ "浏览作者的博客") [ ](http://constantztk.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=constant "发送站内短信") [ ](http://constantztk.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=constant "关注作者") [回帖地址](http://www.javaeye.com/topic/633824#1438840 "constant回帖:Eclipse调试常用技巧")

[0](http://www.javaeye.com/topic/633824# "好") [0](http://www.javaeye.com/topic/633824# "差") 请登录后投票  * heyjjay8
* 等级: 初级会员
* [![heyjjay8的博客]( "heyjjay8的博客: heyjjay8")](http://heyjjay8.javaeye.com/)
* 文章: 26
* 积分: 40
* 来自: ...
* ![]()
    发表时间：2010-04-06  

写的很好,谢谢lz [返回顶楼](http://www.javaeye.com/topic/633824#) [ ](http://heyjjay8.javaeye.com/ "浏览作者的博客") [ ](http://heyjjay8.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=heyjjay8 "发送站内短信") [ ](http://heyjjay8.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=heyjjay8 "关注作者") [回帖地址](http://www.javaeye.com/topic/633824#1438868 "heyjjay8回帖:Eclipse调试常用技巧")

[0](http://www.javaeye.com/topic/633824# "好") [0](http://www.javaeye.com/topic/633824# "差") 请登录后投票

[](http://www.javaeye.com/forums/39/topics/633824/posts/new "发表回复")

« 上一页 1 [2](http://www.javaeye.com/topic/633824?page=2) [3](http://www.javaeye.com/topic/633824?page=3) … [9](http://www.javaeye.com/topic/633824?page=9) [10](http://www.javaeye.com/topic/633824?page=10) [下一页 »](http://www.javaeye.com/topic/633824?page=2)
[论坛首页](http://www.javaeye.com/forums) → [Java编程和Java企业应用版](http://www.javaeye.com/forums/board/Java) → [Java综合](http://www.javaeye.com/forums/tag/Java)
跳转论坛:Java编程和Java企业应用 Web前端技术 移动编程和手机应用开发 C/C++编程 Ruby编程 Python编程 PHP编程 Flash编程和RIA Microsoft .Net 综合技术 软件开发和项目管理 行业应用 入门讨论 招聘求职 海阔天空

* [上海: 我的钢铁网诚聘Java高级开发工程师（互联网开](http://www.javaeye.com/jobs/863 "上海:我的钢铁网诚聘Java高级开发工程师（互联网开发方向）")
* [北京: 去哪儿旅游搜索引擎诚聘java开发工程师](http://www.javaeye.com/jobs/523 "北京:去哪儿旅游搜索引擎诚聘java开发工程师")
* [上海: 傲视堂诚聘高级Java程序员](http://www.javaeye.com/jobs/841 "上海:傲视堂诚聘高级Java程序员")
* [北京: Laszlo Systems 诚聘高级Java程序员](http://www.javaeye.com/jobs/817 "北京:Laszlo Systems 诚聘高级Java程序员")
* [广东: 穆迪moody's诚聘Java Developer](http://www.javaeye.com/jobs/794 "广东:穆迪moody's诚聘Java Developer")
* [北京: glodon诚聘Eclipse插件开发工程师](http://www.javaeye.com/jobs/839 "北京:glodon诚聘Eclipse插件开发工程师")
* [北京: chinacache诚聘高级Java开发工程师（后台应用](http://www.javaeye.com/jobs/726 "北京:chinacache诚聘高级Java开发工程师（后台应用）")
* [江苏: 方正国际诚聘【苏州、杭州】JAVA软件工程师（](http://www.javaeye.com/jobs/865 "江苏:方正国际诚聘【苏州、杭州】JAVA软件工程师（含高级）")
* [北京: 网易（NETEASE）诚聘高级Java开发工程师](http://www.javaeye.com/jobs/800 "北京:网易（NETEASE）诚聘高级Java开发工程师")
* [广东: 动网先锋诚聘Java开发工程师](http://www.javaeye.com/jobs/844 "广东:动网先锋诚聘Java开发工程师")
* [北京: 杰嘉迪诚聘高级Java程序员](http://www.javaeye.com/jobs/750 "北京:杰嘉迪诚聘高级Java程序员")
* [福建: 福州几维网络诚聘游戏服务端程序员(Java)](http://www.javaeye.com/jobs/804 "福建:福州几维网络诚聘游戏服务端程序员(Java)")
* [北京: chinacache诚聘Java应用架构师](http://www.javaeye.com/jobs/728 "北京:chinacache诚聘Java应用架构师")
* [北京: 老虎宝典诚聘服务器端软件开发（偏重工程)](http://www.javaeye.com/jobs/622 "北京:老虎宝典诚聘服务器端软件开发（偏重工程)")
* [上海: BShare诚聘资深Java Web开发工程师](http://www.javaeye.com/jobs/838 "上海:BShare诚聘资深Java Web开发工程师")
* [北京: JavaEye猎头诚聘Java搜索工程师](http://www.javaeye.com/jobs/712 "北京:JavaEye猎头诚聘Java搜索工程师")

* [首页](http://www.javaeye.com/)
* [新闻](http://www.javaeye.com/news)
* [论坛](http://www.javaeye.com/forums)
* [问答](http://www.javaeye.com/ask)
* [专栏](http://www.javaeye.com/wiki)
* [博客](http://www.javaeye.com/blogs)
* [圈子](http://www.javaeye.com/groups)
* [招聘](http://www.javaeye.com/job)
* [服务](http://www.javaeye.com/index/service)
* [搜索](http://www.javaeye.com/search)
* [Java](http://java.javaeye.com/)
* [Web](http://webapp.javaeye.com/)
* [Ruby](http://ruby.javaeye.com/)
* [Python](http://python.javaeye.com/)
* [敏捷](http://agile.javaeye.com/)
* [MySQL](http://mysql.javaeye.com/)
* [润亁报表](http://runqian.javaeye.com/)
* [图书](http://book.javaeye.com/)
* [广告服务](http://www.javaeye.com/index/service)
* [JavaEye黑板报](http://webmaster.javaeye.com/)
* [关于我们](http://www.javaeye.com/index/aboutus)
* [联系我们](http://www.javaeye.com/index/contactus)
* [友情链接](http://www.javaeye.com/index/friend_links)

© 2003-2010 JavaEye.com. 上海炯耐计算机软件有限公司版权所有 [ [沪ICP备05023328号](http://www.miibeian.gov.cn/) ]
