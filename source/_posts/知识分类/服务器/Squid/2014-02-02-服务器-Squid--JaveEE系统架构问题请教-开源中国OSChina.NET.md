---
layout: post
title: "Jave EE系统架构问题请教 - 开源中国 OSChina.NET"
categories: 服务器
tags: 
 - 服务器
 - Squid
--- 

# Jave EE系统架构问题请教 - 开源中国 OSChina.NET

* [首页](http://www.oschina.net/)
* [开源软件](http://www.oschina.net/project)
* [讨论区](http://www.oschina.net/question)

* [技术问答 »](http://www.oschina.net/question?catalog=1)
* [技术分享 »](http://www.oschina.net/question?catalog=2)
* [IT大杂烩 »](http://www.oschina.net/question?catalog=3)
* [职业生涯 »](http://www.oschina.net/question?catalog=100)
* [站务/建议 »](http://www.oschina.net/question?catalog=4)
* [支付宝专区 »](http://www.oschina.net/alipay)
* [代码分享](http://www.oschina.net/code/list)
* [博客](http://www.oschina.net/blog)
* [翻译](http://www.oschina.net/translate)
* [资讯](http://www.oschina.net/news)
* [移动开发](http://www.oschina.net/android)

* [Android开发专区](http://www.oschina.net/android)
* [iOS开发专区](http://www.oschina.net/ios/home)
* [iOS代码库](http://www.oschina.net/ios/codingList)
* [WP7开发专区](http://www.oschina.net/wp7)
* [招聘](http://www.oschina.net/job)

当前访客身份：游客 [ [登录](http://www.oschina.net/home/login?goto_page=http%3A%2F%2Fwww.oschina.net%2Fquestion%2F134411_82561) | [加入开源中国](http://www.oschina.net/home/reg) ]

[开源中国](http://www.oschina.net/ "OSChina 开源中国")

# [讨论区](http://www.oschina.net/question)

当前位置： [讨论区](http://www.oschina.net/question) » [技术问答](http://www.oschina.net/question?catalog=1) » [Nginx](http://www.oschina.net/p/nginx)    搜 索
[![adua]( "adua")](http://my.oschina.net/u/134411)

# [Jave EE系统架构问题请教]()

[adua](http://my.oschina.net/u/134411) 发表于 2012-12-9 10:04 4个月前, [3](http://www.oschina.net/question/134411_82561#answers)回/213阅, 最后回答: 4个月前

Java、PHP、Ruby、iOS、Python 等 JetBrains 开发工具低至 99 元（3折），[详情»](http://www.oschina.net/shop/jetbrains)

[@鉴客](http://my.oschina.net/javaeye) 你好，想跟你请教个问题：一个基于JavaEE开发的系统，目前系统均为动态页面，如何使用nginx和squid技术来增强系统的页面缓存和负载均衡。谢谢！

**标签：** [Nginx](http://www.oschina.net/question/tag/nginx "高性能Web服务器 Nginx") [Squid](http://www.oschina.net/question/tag/squid "代理服务器 Squid")
[我想问同样的问题]()  共*0*个人想要问同样的问题 [补充话题说明»]()

**分享到** []( "分享到新浪微博")[]( "分享到腾讯微博")**

[收藏]( "收藏此话题")
**

0
**

[举报]()
**

[踩]( "踩：这问题不知道在说什么，或者没什么用") 0 | [顶]( "顶：这问题很有用或者很清晰明了") 0
**
## [按评价排序](http://www.oschina.net/question/134411_82561#answers) | [显示最新答案](http://www.oschina.net/question/134411_82561?sort=time#answers) | [回页面顶部](http://www.oschina.net/question/134411_82561#top)  []()共有*3*个答案 [我要回答»](http://www.oschina.net/question/134411_82561#answerform)

* [![鉴客]( "鉴客")](http://my.oschina.net/javaeye)

[鉴客](http://my.oschina.net/javaeye) 回答于 2012-12-09 20:39

[举报]()
你先说下为什么要这么做呢？
[有帮助]( "这是一个好答案，能解决问题")*(0)* | [没帮助]( "这答案无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此答案](http://www.oschina.net/question/answer?question=82561&answer=368789)
* [![loyal]( "loyal")](http://my.oschina.net/tester)

[loyal](http://my.oschina.net/tester) 回答于 2012-12-09 20:52

[举报]()
nginx 就够了吧???
[有帮助]( "这是一个好答案，能解决问题")*(0)* | [没帮助]( "这答案无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此答案](http://www.oschina.net/question/answer?question=82561&answer=368797)
* [![adua]( "adua")](http://my.oschina.net/u/134411)

[adua](http://my.oschina.net/u/134411) 回答于 2012-12-17 20:30

[举报]()
之前看到 [http://labs.chinamobile.com/mblog/466/189076](http://labs.chinamobile.com/mblog/466/189076)帖子将经典分层设计，发现我们系统即未用反向代理，也未用到应用缓存。目的想让1.系统宕机后用户无感知 2.系统访问速度加快。请求意见！
[有帮助]( "这是一个好答案，能解决问题")*(0)* | [没帮助]( "这答案无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此答案](http://www.oschina.net/question/answer?question=82561&answer=375191)

[![非会员用户]( "非会员用户")]()
[回答案顶部](http://www.oschina.net/question/134411_82561#answers) | [回页面顶部](http://www.oschina.net/question/134411_82561#top)

[![]()](http://www.oschina.net/action/visit/ad?id=1033 "JPush——极光推送")

有什么技术问题吗？ [我要提问](http://www.oschina.net/question/ask)
**类似的话题**

* [linux下怎么使用 JAVE](http://www.oschina.net/question/876909_81521 "linux下怎么使用 JAVE")(13回/362阅,4个月前)
* [jave音频文件格式转换问题](http://www.oschina.net/question/857445_76392 "jave音频文件格式转换问题")(0回/134阅,6个月前)
* [JAVE怎么截图](http://www.oschina.net/question/96568_18174 "JAVE怎么截图")(1回/244阅,2年前)
* [J2EE到底要学些什么？](http://www.oschina.net/question/251064_49753 "J2EE到底要学些什么？")(7回/532阅,1年前)
* [求JavaEE6中CDI教程](http://www.oschina.net/question/95583_50378 "求JavaEE6中CDI教程")(2回/294阅,1年前)
* [请问有的网站页面里的所有连接有的是网站主域名连接,有的是二级域名连接是如何动态实现的](http://www.oschina.net/question/139190_37329 "请问有的网站页面里的所有连接有的是网站主域名连接,有的是二级域名连接是如何动态实现的")(0回/79阅,1年前)
* [推荐一本好一点的javaee课本啊 打基础的](http://www.oschina.net/question/243690_39654 "推荐一本好一点的javaee课本啊   打基础的")(1回/39阅,1年前)
* [大家用的Java EE 服务器都是什么](http://www.oschina.net/question/70870_40873 "大家用的Java EE 服务器都是什么")(5回/243阅,1年前)
* [关于struts2.3的小问题?](http://www.oschina.net/question/247635_41247 "关于struts2.3的小问题?")(1回/120阅,1年前)
* [我所发现的j2ee的价值](http://www.oschina.net/question/96003_46290 "我所发现的j2ee的价值")(82回/3843阅,1年前)
* [请问是否有自动建表、发布内容的开源框架？](http://www.oschina.net/question/4814_182 "请问是否有自动建表、发布内容的开源框架？")(5回/286阅,4年前)
* [个人发展](http://www.oschina.net/question/1151_398 "个人发展")(7回/306阅,3年前)
* [谁有物流管理软件 基于J2EE架构的](http://www.oschina.net/question/10852_426 "谁有物流管理软件 基于J2EE架构的")(1回/839阅,3年前)
* [发送邮件进行活动提醒](http://www.oschina.net/question/143193_24744 "发送邮件进行活动提醒")(2回/61阅,1年前)
* [有基于插件的j2ee框架吗？](http://www.oschina.net/question/173061_25743 "有基于插件的j2ee框架吗？")(2回/149阅,1年前)
* [业务层进度条界面显示？](http://www.oschina.net/question/6556_26640 "业务层进度条界面显示？")(10回/429阅,1年前)

© 开源中国(OsChina.NET) | [关于我们](http://www.oschina.net/home/about) | [广告联系](mailto:oschina.net@gmail.com) | [@新浪微博](http://weibo.com/oschina2010) | [开源中国手机版](http://m.oschina.net/) | [粤ICP备12009483号-3](http://www.miitbeian.gov.cn/) 开源中国手机客户端： [Android](http://www.oschina.net/app "Android客户端") [iPhone](http://www.oschina.net/app "iPhone 客户端") [WP7](http://www.oschina.net/app "Windows Phone 客户端")

[]()[]()[]()
![]()
