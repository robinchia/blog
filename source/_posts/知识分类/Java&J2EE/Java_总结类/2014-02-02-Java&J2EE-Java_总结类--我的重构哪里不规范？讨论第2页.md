---
layout: post
title: "我的重构哪里不规范？讨论第2页"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# 我的重构哪里不规范？讨论第2页

[您还未登录 !](http://www.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://www.javaeye.com/login) [注册](http://www.javaeye.com/signup)

[![JavaEye-最棒的软件开发交流社区]( "JavaEye-最棒的软件开发交流社区")](http://www.javaeye.com/)

[论坛首页](http://www.javaeye.com/forums) → [软件开发和项目管理版](http://www.javaeye.com/forums/board/develop) → [软件测试](http://www.javaeye.com/forums/tag/test) →

# 我的重构哪里不规范？

[全部](http://www.javaeye.com/forums/board/develop) [项目管理](http://www.javaeye.com/forums/tag/project-manage) [敏捷开发](http://www.javaeye.com/forums/tag/agile) [软件测试](http://www.javaeye.com/forums/tag/test) [配置管理](http://www.javaeye.com/forums/tag/configuration) [UseCase](http://www.javaeye.com/forums/tag/UseCase) [UML](http://www.javaeye.com/forums/tag/UML) [单元测试](http://www.javaeye.com/forums/tag/unitest) [XP](http://www.javaeye.com/forums/tag/XP) [TDD](http://www.javaeye.com/forums/tag/TDD) [UP](http://www.javaeye.com/forums/tag/UP) [CMM](http://www.javaeye.com/forums/tag/CMM)
[« 上一页](http://www.javaeye.com/topic/90347?page=1) [1](http://www.javaeye.com/topic/90347?page=1) 2 [3](http://www.javaeye.com/topic/90347?page=3) [4](http://www.javaeye.com/topic/90347?page=4) [5](http://www.javaeye.com/topic/90347?page=5) [下一页 »](http://www.javaeye.com/topic/90347?page=3)

浏览 17393 次 锁定老贴子 [主题：我的重构哪里不规范？](http://www.javaeye.com/topic/90347)

精华帖 (3) :: 良好帖 (0) :: 新手帖 (0) :: 隐藏帖 (1) 作者 正文 * gigix
* 等级: ![资深会员]( "资深会员")资深会员
* [![gigix的博客]( "gigix的博客: 透明之眼")](http://gigix.javaeye.com/)
* 文章: 4810
* 积分: 2991
* 来自: 北京
* ![]()
    发表时间：2007-06-15  

[<](http://www.javaeye.com/topic/90347?page=2#) [>](http://www.javaeye.com/topic/90347?page=2#)  猎头职位: [北京:  Java搜索工程师](http://www.javaeye.com/jobs/712)

ojava 写道
公司新加代码规范条目：所定义的方法尽量不要超过100行。
某一方面来说，可以避免这种流水账式的代码吧！
并且强制我们进行Xiaohanne所说的面向接口不要面向实现的编程。
TDD is THE solution of your problem:
you just can't write a test for a hundred-lines-long method.
Actually, in normal cases,
methods should not be longer than 10 lines.
(I'd say 5 lines indeed.) [返回顶楼](http://www.javaeye.com/topic/90347?page=2#) [ ](http://gigix.javaeye.com/ "浏览作者的博客") [ ](http://gigix.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=gigix "发送站内短信") [ ](http://gigix.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=gigix "关注作者") [回帖地址](http://www.javaeye.com/topic/90347?page=2#313291 "gigix回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347?page=2# "好") [0](http://www.javaeye.com/topic/90347?page=2# "差") 请登录后投票  * 诺铁
* 等级: ![四星会员]( "四星会员")
* [![诺铁的博客]( "诺铁的博客: 诺铁心斋")](http://notyy.javaeye.com/)
* 文章: 277
* 积分: 398
* ![]()
    发表时间：2007-06-15  

因为要后续使用而把局部变量提到成员变量是最差的作法。
你需要的不是重构，是重设计。 [返回顶楼](http://www.javaeye.com/topic/90347?page=2#) [ ](http://notyy.javaeye.com/ "浏览作者的博客") [ ](http://notyy.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E8%AF%BA%E9%93%81 "发送站内短信") [ ](http://notyy.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E8%AF%BA%E9%93%81 "关注作者") [回帖地址](http://www.javaeye.com/topic/90347?page=2#313301 "诺铁回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347?page=2# "好") [0](http://www.javaeye.com/topic/90347?page=2# "差") 请登录后投票  * gigix
* 等级: ![资深会员]( "资深会员")资深会员
* [![gigix的博客]( "gigix的博客: 透明之眼")](http://gigix.javaeye.com/)
* 文章: 4810
* 积分: 2991
* 来自: 北京
* ![]()
    发表时间：2007-06-15  

诺铁 写道

因为要后续使用而把局部变量提到成员变量是最差的作法。
你需要的不是重构，是重设计。
please man ...
sigh [返回顶楼](http://www.javaeye.com/topic/90347?page=2#) [ ](http://gigix.javaeye.com/ "浏览作者的博客") [ ](http://gigix.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=gigix "发送站内短信") [ ](http://gigix.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=gigix "关注作者") [回帖地址](http://www.javaeye.com/topic/90347?page=2#313312 "gigix回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347?page=2# "好") [0](http://www.javaeye.com/topic/90347?page=2# "差") 请登录后投票  * xly_971223
* 等级: ![五星会员]( "五星会员")
* [![xly_971223的博客]( "xly_971223的博客: 一生一火花")](http://xuliangyong.javaeye.com/)
* 文章: 1048
* 积分: 650
* 来自: 北京
* ![]()
    发表时间：2007-06-15  

ojava 写道

公司新加代码规范条目：所定义的方法尽量不要超过100行。
某一方面来说，可以避免这种流水账式的代码吧！
并且强制我们进行Xiaohanne所说的面向接口不要面向实现的编程。
100行？太长了吧
我一般情况保持在20行左右 [返回顶楼](http://www.javaeye.com/topic/90347?page=2#) [ ](http://xuliangyong.javaeye.com/ "浏览作者的博客") [ ](http://xuliangyong.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=xly_971223 "发送站内短信") [ ](http://xuliangyong.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=xly_971223 "关注作者") [回帖地址](http://www.javaeye.com/topic/90347?page=2#313402 "xly_971223回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347?page=2# "好") [0](http://www.javaeye.com/topic/90347?page=2# "差") 请登录后投票  * gigix
* 等级: ![资深会员]( "资深会员")资深会员
* [![gigix的博客]( "gigix的博客: 透明之眼")](http://gigix.javaeye.com/)
* 文章: 4810
* 积分: 2991
* 来自: 北京
* ![]()
    发表时间：2007-06-15  

xly_971223 写道

ojava 写道

公司新加代码规范条目：所定义的方法尽量不要超过100行。
某一方面来说，可以避免这种流水账式的代码吧！
并且强制我们进行Xiaohanne所说的面向接口不要面向实现的编程。
100行？太长了吧
我一般情况保持在20行左右
3.5 lines in average [返回顶楼](http://www.javaeye.com/topic/90347?page=2#) [ ](http://gigix.javaeye.com/ "浏览作者的博客") [ ](http://gigix.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=gigix "发送站内短信") [ ](http://gigix.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=gigix "关注作者") [回帖地址](http://www.javaeye.com/topic/90347?page=2#313404 "gigix回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347?page=2# "好") [0](http://www.javaeye.com/topic/90347?page=2# "差") 请登录后投票  * xly_971223
* 等级: ![五星会员]( "五星会员")
* [![xly_971223的博客]( "xly_971223的博客: 一生一火花")](http://xuliangyong.javaeye.com/)
* 文章: 1048
* 积分: 650
* 来自: 北京
* ![]()
    发表时间：2007-06-15  

诺铁 写道

因为要后续使用而把局部变量提到成员变量是最差的作法。
你需要的不是重构，是重设计。
重构跟重新设计是冲突的吗
在功能不变的情况下，我们可以通过重新设计完成重构 [返回顶楼](http://www.javaeye.com/topic/90347?page=2#) [ ](http://xuliangyong.javaeye.com/ "浏览作者的博客") [ ](http://xuliangyong.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=xly_971223 "发送站内短信") [ ](http://xuliangyong.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=xly_971223 "关注作者") [回帖地址](http://www.javaeye.com/topic/90347?page=2#313406 "xly_971223回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347?page=2# "好") [0](http://www.javaeye.com/topic/90347?page=2# "差") 请登录后投票  * xly_971223
* 等级: ![五星会员]( "五星会员")
* [![xly_971223的博客]( "xly_971223的博客: 一生一火花")](http://xuliangyong.javaeye.com/)
* 文章: 1048
* 积分: 650
* 来自: 北京
* ![]()
    发表时间：2007-06-15  

gigix 写道

xly_971223 写道

ojava 写道

公司新加代码规范条目：所定义的方法尽量不要超过100行。
某一方面来说，可以避免这种流水账式的代码吧！
并且强制我们进行Xiaohanne所说的面向接口不要面向实现的编程。
100行？太长了吧
我一般情况保持在20行左右
3.5 lines in average
这个。。。。
有点难度 。。。 [返回顶楼](http://www.javaeye.com/topic/90347?page=2#) [ ](http://xuliangyong.javaeye.com/ "浏览作者的博客") [ ](http://xuliangyong.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=xly_971223 "发送站内短信") [ ](http://xuliangyong.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=xly_971223 "关注作者") [回帖地址](http://www.javaeye.com/topic/90347?page=2#313412 "xly_971223回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347?page=2# "好") [0](http://www.javaeye.com/topic/90347?page=2# "差") 请登录后投票  * 抛出异常的爱
* 等级: ![五钻会员]( "五钻会员")
* [![抛出异常的爱的博客]( "抛出异常的爱的博客: 设计可以拯救国家")](http://loveexception.javaeye.com/)
* 文章: 12053
* 积分: 2782
* 来自: 北京
* ![]()
    发表时间：2007-06-15  

javastudy 写道

ojava 写道

公司新加代码规范条目：所定义的方法尽量不要超过100行。
某一方面来说，可以避免这种流水账式的代码吧！
并且强制我们进行Xiaohanne所说的面向接口不要面向实现的编程。
得在设计时就得想到拉
我只能说呸。。。。只有日本人能想的到。。。 [返回顶楼](http://www.javaeye.com/topic/90347?page=2#) [ ](http://loveexception.javaeye.com/ "浏览作者的博客") [ ](http://loveexception.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E6%8A%9B%E5%87%BA%E5%BC%82%E5%B8%B8%E7%9A%84%E7%88%B1 "发送站内短信") [ ](http://loveexception.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E6%8A%9B%E5%87%BA%E5%BC%82%E5%B8%B8%E7%9A%84%E7%88%B1 "关注作者") [回帖地址](http://www.javaeye.com/topic/90347?page=2#313519 "抛出异常的爱回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347?page=2# "好") [0](http://www.javaeye.com/topic/90347?page=2# "差") 请登录后投票  * sg552
* 等级: ![一星会员]( "一星会员")
* [![sg552的博客]( "sg552的博客: sg552")](http://sg552.javaeye.com/)
* 文章: 566
* 积分: 141
* 来自: 北京
* ![]()
    发表时间：2007-06-15  

重构—— 改善现有代码的设计。 看看这本书吧。LZ
另外，多测试，常测试，保证每个重构都是成功的。
至于具体细节，就要看经验了。 [返回顶楼](http://www.javaeye.com/topic/90347?page=2#) [ ](http://sg552.javaeye.com/ "浏览作者的博客") [ ](http://sg552.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=sg552 "发送站内短信") [ ](http://sg552.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=sg552 "关注作者") [回帖地址](http://www.javaeye.com/topic/90347?page=2#313526 "sg552回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347?page=2# "好") [0](http://www.javaeye.com/topic/90347?page=2# "差") 请登录后投票  * gigix
* 等级: ![资深会员]( "资深会员")资深会员
* [![gigix的博客]( "gigix的博客: 透明之眼")](http://gigix.javaeye.com/)
* 文章: 4810
* 积分: 2991
* 来自: 北京
* ![]()
    发表时间：2007-06-15  

javastudy 写道

gigix 写道

xly_971223 写道

ojava 写道

公司新加代码规范条目：所定义的方法尽量不要超过100行。
某一方面来说，可以避免这种流水账式的代码吧！
并且强制我们进行Xiaohanne所说的面向接口不要面向实现的编程。
100行？太长了吧
我一般情况保持在20行左右
3.5 lines in average
有点太短了吧
that's our stat in previous project
you can try to show me an example: why do you need a method longer than 5 lines?
(some complex algorithm implementations are exceptions.) [返回顶楼](http://www.javaeye.com/topic/90347?page=2#) [ ](http://gigix.javaeye.com/ "浏览作者的博客") [ ](http://gigix.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=gigix "发送站内短信") [ ](http://gigix.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=gigix "关注作者") [回帖地址](http://www.javaeye.com/topic/90347?page=2#313548 "gigix回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347?page=2# "好") [0](http://www.javaeye.com/topic/90347?page=2# "差") 请登录后投票

[« 上一页](http://www.javaeye.com/topic/90347?page=1) [1](http://www.javaeye.com/topic/90347?page=1) 2 [3](http://www.javaeye.com/topic/90347?page=3) [4](http://www.javaeye.com/topic/90347?page=4) [5](http://www.javaeye.com/topic/90347?page=5) [下一页 »](http://www.javaeye.com/topic/90347?page=3)
[论坛首页](http://www.javaeye.com/forums) → [软件开发和项目管理版](http://www.javaeye.com/forums/board/develop) → [软件测试](http://www.javaeye.com/forums/tag/test)
跳转论坛:Java编程和Java企业应用 Web前端技术 移动编程和手机应用开发 C/C++编程 Ruby编程 Python编程 PHP编程 Flash编程和RIA Microsoft .Net 综合技术 软件开发和项目管理 行业应用 入门讨论 招聘求职 海阔天空

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

© 2003-2010 JavaEye.com. 上海炯耐计算机软件有限公司版权所有 [ [沪ICP备05023328号](http://www.miibeian.gov.cn/) ] ![]()
