---
layout: post
title: "我的重构哪里不规范？ - 软件测试"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# 我的重构哪里不规范？ - 软件测试

[您还未登录 !](http://www.javaeye.com/login "登录")[我的应用](http://www.javaeye.com/all)[登录](http://www.javaeye.com/login)[注册](http://www.javaeye.com/signup)

[![JavaEye-最棒的软件开发交流社区]( "JavaEye-最棒的软件开发交流社区")](http://www.javaeye.com/)

[论坛首页](http://www.javaeye.com/forums) →[软件开发和项目管理版](http://www.javaeye.com/forums/board/develop) →[软件测试](http://www.javaeye.com/forums/tag/test) →

# 我的重构哪里不规范？

[全部](http://www.javaeye.com/forums/board/develop)[项目管理](http://www.javaeye.com/forums/tag/project-manage)[敏捷开发](http://www.javaeye.com/forums/tag/agile)[软件测试](http://www.javaeye.com/forums/tag/test)[配置管理](http://www.javaeye.com/forums/tag/configuration)[UseCase](http://www.javaeye.com/forums/tag/UseCase)[UML](http://www.javaeye.com/forums/tag/UML)[单元测试](http://www.javaeye.com/forums/tag/unitest)[XP](http://www.javaeye.com/forums/tag/XP)[TDD](http://www.javaeye.com/forums/tag/TDD)[UP](http://www.javaeye.com/forums/tag/UP)[CMM](http://www.javaeye.com/forums/tag/CMM)
« 上一页 1 [2](http://www.javaeye.com/topic/90347?page=2) [3](http://www.javaeye.com/topic/90347?page=3) [4](http://www.javaeye.com/topic/90347?page=4) [5](http://www.javaeye.com/topic/90347?page=5) [下一页 »](http://www.javaeye.com/topic/90347?page=2)

浏览 17393 次 锁定老贴子 [主题：我的重构哪里不规范？]()

精华帖 (3) :: 良好帖 (0) :: 新手帖 (0) :: 隐藏帖 (1) 作者 正文 * ojava
* 等级: 初级会员
* [![ojava的博客]( "ojava的博客: ojava")](http://ojava.javaeye.com/)
* 文章: 7
* 积分: 33
* 来自: 烟台
* ![]()
    发表时间：2007-06-15  

[<](http://www.javaeye.com/topic/90347#)[>](http://www.javaeye.com/topic/90347#)猎头职位:   [北京:  Java搜索工程师](http://www.javaeye.com/jobs/712)

相关文章: [ ](http://www.javaeye.com/topic/90347# "关闭")

* [你们的项目经常重构代码吗？](http://www.javaeye.com/topic/416297 "你们的项目经常重构代码吗？")
* [重构的几大重要特点](http://www.javaeye.com/topic/460125 "重构的几大重要特点")
* [讨论：重构的前提是不是 TDD](http://www.javaeye.com/topic/6426 "讨论：重构的前提是不是 TDD")
推荐圈子: [火星常驻JE办事处](http://mars.group.javaeye.com/)
[更多相关推荐](http://www.javaeye.com/wiki/topic/90347)
在项目中，由于没有经过大脑思考，结果产生了流水账形式的代码。
流水账代码：就是根据是详细设计书把整个业务的流程顺序完成到一个类的一个方法中，
而没有根据功能划分成若干个小的方法。
这种流水账式的代码非常不容易测试，因为详细设计中已经将设计细化到对字符串如何操作了，
所以从这样的设计书的高度看业务，简直就是乱七八糟！
所幸，还有重构这个工具，就重构，发现很多的局部变量，因为在多处改变值，而且后续还要使用，
所以只能把这种变量，提到类变量的高度，好多啊。
这样一来，
1。如果要用junit测试，还需要再给相应的提出来的变量加上set,get方法。
2。因为重构出来的方法都是private的，所以测试的时候还要用反射的方法。
上面这两种情况可以避免吗？这是一个问题。
还有一个对自己的警告：小心费力不讨好！
声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。

推荐链接

* [![adobe]( "adobe")](http://www.javaeye.com/clicks/373) [返回顶楼](http://www.javaeye.com/topic/90347#) [ ](http://ojava.javaeye.com/ "浏览作者的博客")[ ](http://ojava.javaeye.com/blog/profile "浏览作者资料")[ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=ojava "发送站内短信")[ ](http://ojava.javaeye.com/blog/guest_book "给作者留言")[ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=ojava "关注作者") * Xiaohanne
* 等级: ![四星会员]( "四星会员")
* [![Xiaohanne的博客]( "Xiaohanne的博客: Xiaohanne")](http://xiaohanne.javaeye.com/)
* 文章: 183
* 积分: 389
* ![]()
    发表时间：2007-06-15  

面向接口不要面向实现，谢谢 [返回顶楼](http://www.javaeye.com/topic/90347#) [ ](http://xiaohanne.javaeye.com/ "浏览作者的博客")[ ](http://xiaohanne.javaeye.com/blog/profile "浏览作者资料")[ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=Xiaohanne "发送站内短信")[ ](http://xiaohanne.javaeye.com/blog/guest_book "给作者留言")[ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=Xiaohanne "关注作者")[回帖地址](http://www.javaeye.com/topic/90347#312955 "Xiaohanne回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347# "好") [0](http://www.javaeye.com/topic/90347# "差") 请登录后投票 * 抛出异常的爱
* 等级: ![五钻会员]( "五钻会员")
* [![抛出异常的爱的博客]( "抛出异常的爱的博客: 设计可以拯救国家")](http://loveexception.javaeye.com/)
* 文章: 12050
* 积分: 2782
* 来自: 北京
* [![]()](http://www.javaeye.com/topic/743366 "我正在看《纪念coding之路.祝福所有的coder都可以实现自己的梦想》")
    发表时间：2007-06-15  

**ojava 写道：**
在项目中，由于没有经过大脑思考，结果产生了流水账形式的代码。
流水账代码：就是根据是详细设计书把整个业务的流程顺序完成到一个类的一个方法中，
而没有根据功能划分成若干个小的方法。
这种新式的代码非常不容易测试，因为详细设计中已经将设计细化到对字符串如何操作了，
所以从这样的设计书的高度看业务，简直就是乱七八糟！
所幸，还有重构这个工具，就重构，发现很多的局部变量，因为在多处改变值，而且后续还要使用，
所以只能把这种变量，提到类变量的高度，好多啊。
这样一来，
1。如果要用junit测试，还需要再给相应的提出来的变量加上set,get方法。
2。因为重构出来的方法都是private的，所以测试的时候还要用反射的方法。
上面这两种情况可以避免吗？这是一个问题。
还有一个对自己的警告：小心费力不讨好！
无设计，无定义（对每个类与方法），无规则，的重构。。。叶公好龙 [返回顶楼](http://www.javaeye.com/topic/90347#) [ ](http://loveexception.javaeye.com/ "浏览作者的博客")[ ](http://loveexception.javaeye.com/blog/profile "浏览作者资料")[ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E6%8A%9B%E5%87%BA%E5%BC%82%E5%B8%B8%E7%9A%84%E7%88%B1 "发送站内短信")[ ](http://loveexception.javaeye.com/blog/guest_book "给作者留言")[ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E6%8A%9B%E5%87%BA%E5%BC%82%E5%B8%B8%E7%9A%84%E7%88%B1 "关注作者")[回帖地址](http://www.javaeye.com/topic/90347#312962 "抛出异常的爱回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347# "好") [0](http://www.javaeye.com/topic/90347# "差") 请登录后投票 * ojava
* 等级: 初级会员
* [![ojava的博客]( "ojava的博客: ojava")](http://ojava.javaeye.com/)
* 文章: 7
* 积分: 33
* 来自: 烟台
* ![]()
    发表时间：2007-06-15  

无设计，无定义（对每个类与方法），无规则，的重构。。。叶公好龙
这句话，一针见血！！！
刚学习测试，准备在项目中实践，结果发现自己的代码根本没有办法测试，因为这个巨大的方法太大，看不清要实现的业务是什么！
头脑中就是因为缺少测试的思想，才导致出现这样的问题，果然没有思想是不行的，亡羊补牢，现在晚了吗？ [返回顶楼](http://www.javaeye.com/topic/90347#) [ ](http://ojava.javaeye.com/ "浏览作者的博客")[ ](http://ojava.javaeye.com/blog/profile "浏览作者资料")[ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=ojava "发送站内短信")[ ](http://ojava.javaeye.com/blog/guest_book "给作者留言")[ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=ojava "关注作者")[回帖地址](http://www.javaeye.com/topic/90347#312971 "ojava回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347# "好") [0](http://www.javaeye.com/topic/90347# "差") 请登录后投票 * gigix
* 等级: ![资深会员]( "资深会员")资深会员
* [![gigix的博客]( "gigix的博客: 透明之眼")](http://gigix.javaeye.com/)
* 文章: 4810
* 积分: 2991
* 来自: 北京
* ![]()
    发表时间：2007-06-15  

First of all you should have tests for you original method.
During your refactoring those test should always pass.
So you don't have to test new (and quite possible private) methods/fields you extract out. [返回顶楼](http://www.javaeye.com/topic/90347#) [ ](http://gigix.javaeye.com/ "浏览作者的博客")[ ](http://gigix.javaeye.com/blog/profile "浏览作者资料")[ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=gigix "发送站内短信")[ ](http://gigix.javaeye.com/blog/guest_book "给作者留言")[ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=gigix "关注作者")[回帖地址](http://www.javaeye.com/topic/90347#312973 "gigix回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347# "好") [1](http://www.javaeye.com/topic/90347# "差") 请登录后投票 * 抛出异常的爱
* 等级: ![五钻会员]( "五钻会员")
* [![抛出异常的爱的博客]( "抛出异常的爱的博客: 设计可以拯救国家")](http://loveexception.javaeye.com/)
* 文章: 12050
* 积分: 2782
* 来自: 北京
* [![]()](http://www.javaeye.com/topic/743366 "我正在看《纪念coding之路.祝福所有的coder都可以实现自己的梦想》")
    发表时间：2007-06-15  

gigix 写道

First of all you should have tests for you original method.
During your refactoring those test should always pass.
So you don't have to test new (and quite possible private) methods/fields you extract out.
是的，不用再加测试了，先写着，
不过对于拿不准的，写出来出错看不出来，找不到错的
还是要写中间测试帮助编码
PS:如果要测试中间的private方法，可以写在主函数中一个
public XXX (){ return 你要测试的方法 ；}
PS：天啊。。。变的够快。。。认不出来了 [返回顶楼](http://www.javaeye.com/topic/90347#) [ ](http://loveexception.javaeye.com/ "浏览作者的博客")[ ](http://loveexception.javaeye.com/blog/profile "浏览作者资料")[ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E6%8A%9B%E5%87%BA%E5%BC%82%E5%B8%B8%E7%9A%84%E7%88%B1 "发送站内短信")[ ](http://loveexception.javaeye.com/blog/guest_book "给作者留言")[ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E6%8A%9B%E5%87%BA%E5%BC%82%E5%B8%B8%E7%9A%84%E7%88%B1 "关注作者")[回帖地址](http://www.javaeye.com/topic/90347#312979 "抛出异常的爱回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347# "好") [0](http://www.javaeye.com/topic/90347# "差") 请登录后投票 * gigix
* 等级: ![资深会员]( "资深会员")资深会员
* [![gigix的博客]( "gigix的博客: 透明之眼")](http://gigix.javaeye.com/)
* 文章: 4810
* 积分: 2991
* 来自: 北京
* ![]()
    发表时间：2007-06-15  

抛出异常的爱 写道

不过对于拿不准的，写出来出错看不出来，找不到错的
还是要写中间测试帮助编码
nope...
if you make any mistake during refactoring and break any test,
and if you can't fix it immediately,
you should revert to previous successful step immediately.
(that's why you should check in as frequently as you can.) [返回顶楼](http://www.javaeye.com/topic/90347#) [ ](http://gigix.javaeye.com/ "浏览作者的博客")[ ](http://gigix.javaeye.com/blog/profile "浏览作者资料")[ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=gigix "发送站内短信")[ ](http://gigix.javaeye.com/blog/guest_book "给作者留言")[ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=gigix "关注作者")[回帖地址](http://www.javaeye.com/topic/90347#313053 "gigix回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347# "好") [0](http://www.javaeye.com/topic/90347# "差") 请登录后投票 * 抛出异常的爱
* 等级: ![五钻会员]( "五钻会员")
* [![抛出异常的爱的博客]( "抛出异常的爱的博客: 设计可以拯救国家")](http://loveexception.javaeye.com/)
* 文章: 12050
* 积分: 2782
* 来自: 北京
* [![]()](http://www.javaeye.com/topic/743366 "我正在看《纪念coding之路.祝福所有的coder都可以实现自己的梦想》")
    发表时间：2007-06-15  

gigix 写道

抛出异常的爱 写道

不过对于拿不准的，写出来出错看不出来，找不到错的
还是要写中间测试帮助编码
nope...
if you make any mistake during refactoring and break any test,
and if you can't fix it immediately,
you should revert to previous successful step immediately.
(that's why you should check in as frequently as you can.)
那么不动老的代码。。。
写新方法，新的测试，
所有需要的方法都写完了（小步前进？）
再一次把老的代码删去
用新的代码顶上？ [返回顶楼](http://www.javaeye.com/topic/90347#) [ ](http://loveexception.javaeye.com/ "浏览作者的博客")[ ](http://loveexception.javaeye.com/blog/profile "浏览作者资料")[ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E6%8A%9B%E5%87%BA%E5%BC%82%E5%B8%B8%E7%9A%84%E7%88%B1 "发送站内短信")[ ](http://loveexception.javaeye.com/blog/guest_book "给作者留言")[ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E6%8A%9B%E5%87%BA%E5%BC%82%E5%B8%B8%E7%9A%84%E7%88%B1 "关注作者")[回帖地址](http://www.javaeye.com/topic/90347#313088 "抛出异常的爱回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347# "好") [0](http://www.javaeye.com/topic/90347# "差") 请登录后投票 * gigix
* 等级: ![资深会员]( "资深会员")资深会员
* [![gigix的博客]( "gigix的博客: 透明之眼")](http://gigix.javaeye.com/)
* 文章: 4810
* 积分: 2991
* 来自: 北京
* ![]()
    发表时间：2007-06-15  

抛出异常的爱 写道

gigix 写道

抛出异常的爱 写道

不过对于拿不准的，写出来出错看不出来，找不到错的
还是要写中间测试帮助编码
nope...
if you make any mistake during refactoring and break any test,
and if you can't fix it immediately,
you should revert to previous successful step immediately.
(that's why you should check in as frequently as you can.)
那么不动老的代码。。。
写新方法，新的测试，
所有需要的方法都写完了
再一次把老的代码删去
用新的代码顶上？
please man...
what is "refactoring"?
answer: to improve the internal structure of code, WITHOUT CHANGING ITS EXTERNAL BEHAVIOR
when you refactoring, you move forward by SMALL STEPS. you remove the bad smells little by little, and always keep all tests passing. [返回顶楼](http://www.javaeye.com/topic/90347#) [ ](http://gigix.javaeye.com/ "浏览作者的博客")[ ](http://gigix.javaeye.com/blog/profile "浏览作者资料")[ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=gigix "发送站内短信")[ ](http://gigix.javaeye.com/blog/guest_book "给作者留言")[ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=gigix "关注作者")[回帖地址](http://www.javaeye.com/topic/90347#313092 "gigix回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347# "好") [0](http://www.javaeye.com/topic/90347# "差") 请登录后投票 * ojava
* 等级: 初级会员
* [![ojava的博客]( "ojava的博客: ojava")](http://ojava.javaeye.com/)
* 文章: 7
* 积分: 33
* 来自: 烟台
* ![]()
    发表时间：2007-06-15  

公司新加代码规范条目：所定义的方法尽量不要超过100行。
某一方面来说，可以避免这种流水账式的代码吧！
并且强制我们进行Xiaohanne所说的面向接口不要面向实现的编程。 [返回顶楼](http://www.javaeye.com/topic/90347#) [ ](http://ojava.javaeye.com/ "浏览作者的博客")[ ](http://ojava.javaeye.com/blog/profile "浏览作者资料")[ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=ojava "发送站内短信")[ ](http://ojava.javaeye.com/blog/guest_book "给作者留言")[ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=ojava "关注作者")[回帖地址](http://www.javaeye.com/topic/90347#313239 "ojava回帖:我的重构哪里不规范？")

[0](http://www.javaeye.com/topic/90347# "好") [0](http://www.javaeye.com/topic/90347# "差") 请登录后投票

« 上一页 1 [2](http://www.javaeye.com/topic/90347?page=2) [3](http://www.javaeye.com/topic/90347?page=3) [4](http://www.javaeye.com/topic/90347?page=4) [5](http://www.javaeye.com/topic/90347?page=5) [下一页 »](http://www.javaeye.com/topic/90347?page=2)
[论坛首页](http://www.javaeye.com/forums) →[软件开发和项目管理版](http://www.javaeye.com/forums/board/develop)→ [软件测试](http://www.javaeye.com/forums/tag/test)
跳转论坛:Java编程和Java企业应用Web前端技术 移动编程和手机应用开发C/C++编程Ruby编程Python编程 PHP编程Flash编程和RIA Microsoft .Net综合技术软件开发和项目管理行业应用 入门讨论招聘求职海阔天空

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

© 2003-2010 JavaEye.com. 上海炯耐计算机软件有限公司版权所有 [ [沪ICP备05023328号](http://www.miibeian.gov.cn/) ]  ![]()
