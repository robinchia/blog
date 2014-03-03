---
layout: post
title: "关于HttpClient的中文参数编码问题 - 讨论区 - 开源中国社区"
categories: Httpclient
tags: 
 - Httpclient
--- 

# 关于HttpClient的中文参数编码问题 - 讨论区 - 开源中国社区

* [首页](http://www.oschina.net/)
* [开源软件](http://www.oschina.net/project)
* [讨论区](http://www.oschina.net/question)
* [代码分享](http://www.oschina.net/code/list)
* [资讯](http://www.oschina.net/news)
* [博客](http://www.oschina.net/blog)
* [Android](http://www.oschina.net/android)
* [招聘](http://www.oschina.net/job)

当前访客身份：游客 [ [登录](http://www.oschina.net/home/login) | [加入开源中国](http://www.oschina.net/home/reg) ]

当前位置：[讨论区](http://www.oschina.net/question) » [技术分享](http://www.oschina.net/question?catalog=2) » [HttpComponents](http://www.oschina.net/p/httpclient)

软件 代码 讨论区 新闻 博客
[![红薯]( "红薯")](http://my.oschina.net/javayou)

# [关于HttpClient的中文参数编码问题]()

[红薯](http://my.oschina.net/javayou) 发表于 10-5 17:13 3年前, [1](http://www.oschina.net/question/12_4535#answers)回/762阅, 最后回答: 4个月前
**[讨论区](http://www.oschina.net/question) » [技术分享](http://www.oschina.net/question?catalog=2)**

开源扑克第二轮预售，截止时间 9 月 30 日 [我要购买](http://www.oschina.net/shop/item6/12)
今天在使用HttpClient提交中文参数的时候发现服务器不管怎么处理得到的字符串都是乱码，考虑应该是客户端的问题，查阅 HttpClient的文档，提到这么一段：

The standard for URLs ( RFC1738 ) explictly states that URLs may only contain graphic printable characters of the US-ASCII coded character set and is defined in terms of octets. The octets 80-FF hexadecimal are not used in US-ASCII and the octets OO-1F hexadecimal represent control characters; characters in these ranges must be encoded.
Characters which cannot be represented by an 8-bit ASCII code, can not be used in an URL as there is no way to reliably encode them
(the encoding scheme for URLs is based off of octets). Despite this, some servers do support varying means of encoding double byte characters in URLs,
the most common technique seems to be to use UTF-8 encoding and encode each octet separately even if a pair of octets represents one character. This however, is not specified by the standard and is highly prone to error, so it is recommended that URLs be restricted to the 8-bit ASCII range.
因此在提交中文参数的时候必须进行转码：
NameValuePair content = new NameValuePair("content",new String(" 你好中国".getBytes(),"8859_1"));
搞定！

**标签：** [HttpComponents](http://www.oschina.net/question/tag/httpclient "Java的HTTP协议库 HttpComponents") [HttpClient](http://www.oschina.net/question/tag/httpclient_old " HttpClient")
[补充话题说明»]()

**分享到** []( "分享到新浪微博")[]( "分享到腾讯微博")**

[收藏]( "收藏此话题")
**

1
**

[举报]()
**

[踩]( "踩：这问题不知道在说什么，或者没什么用") 0 | [顶]( "顶：这问题很有用或者很清晰明了") 0
**
## [按默认排序](http://www.oschina.net/question/12_4535#answers) | [显示最新评论](http://www.oschina.net/question/12_4535?sort=time#answers) | [回页面顶部](http://www.oschina.net/question/12_4535#top)  []()共有*1*个评论 [发表评论»](http://www.oschina.net/question/12_4535#answerform)

* [![hujiong]( "hujiong")](http://my.oschina.net/u/437619)

[hujiong](http://my.oschina.net/u/437619) 回答于 2012-05-08 09:25

[举报]()
[![]()]()
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=4535&answer=211268)

[![非会员用户]( "非会员用户")](http://www.oschina.net/question/$link.user($g_user))   []( "粗体(Ctrl+B)")[]( "斜体(Ctrl+I)")[]( "下划线(Ctrl+U)")[]( "删除线")[]( "删除格式")[]( "编号")[]( "项目符号")[]( "文字颜色")[]( "文字背景")[]( "字体")[]( "文字大小")[]( "超级链接")[]( "取消超级链接")[]( "插入表情")[]( "插入程序代码或脚本")[]( "图片")[]( "插入Flash")[]( "引用某段文字")[]( "全选")[]( "HTML代码")[]( "关于")
[回评论顶部](http://www.oschina.net/question/12_4535#answers) | [回页面顶部](http://www.oschina.net/question/12_4535#top)
有什么技术问题吗？ [我要提问](http://www.oschina.net/question/ask)

**[全部(4766)...](http://my.oschina.net/javayou/?ft=bbs&scope=2&showme=1)*红薯*的其他问题**

* [OSChina 用户动态设计说明](http://www.oschina.net/question/12_70587 "OSChina 用户动态设计说明")(2回/241阅,26分钟前)
* [提取 SQL Server 的日志信息并发送邮件](http://www.oschina.net/question/12_70313 "提取 SQL Server 的日志信息并发送邮件")(0回/71阅,3天前)
* [OSChina 的留言表设计说明](http://www.oschina.net/question/12_70252 "OSChina 的留言表设计说明")(59回/5932阅,3天前)
* [遠端管理 VirtualBox 的神兵利器－RemoteBox](http://www.oschina.net/question/12_70228 "遠端管理 VirtualBox 的神兵利器－RemoteBox")(0回/70阅,4天前)
* [在 Node.js 上运行 Perl 脚本](http://www.oschina.net/question/12_70166 "在 Node.js 上运行 Perl 脚本")(0回/75阅,5天前)
**类似的话题**

* [HttpClient 爬数据时 中文编码问题](http://www.oschina.net/question/1092_2616 "HttpClient 爬数据时  中文编码问题")(2回/1351阅,2年前)
* [HTTPClient PostMethod 中文乱码问题解决方法](http://www.oschina.net/question/157182_34442 "HTTPClient PostMethod 中文乱码问题解决方法")(1回/382阅,9个月前)
* [应用HttpClient来对付各种顽固的WEB服务器](http://www.oschina.net/question/12_4534 "应用HttpClient来对付各种顽固的WEB服务器")(2回/1389阅,3年前)
* [httpClient 4.x中，MultipartEntity中附加中文信息时的乱码解决](http://www.oschina.net/question/163910_34519 "httpClient 4.x中，MultipartEntity中附加中文信息时的乱码解决")(1回/566阅,9个月前)
* [Android 使用 httpClient 取消http请求的方法](http://www.oschina.net/question/54100_32895 "Android 使用 httpClient 取消http请求的方法")(0回/411阅,9个月前)
* [使用 Apache HttpClient 突破 J2EE 站点认证](http://www.oschina.net/question/12_4839 "使用 Apache HttpClient 突破 J2EE 站点认证")(0回/361阅,3年前)
* [为 Android 开发访问 JAX-RS Web 服务的 Apache HttpClient 客户端](http://www.oschina.net/question/129540_31320 "为 Android 开发访问 JAX-RS Web 服务的 Apache HttpClient 客户端")(3回/714阅,10个月前)
* [使用 HttpClient 和 HtmlParser 实现简易爬虫](http://www.oschina.net/question/12_4532 "使用 HttpClient 和 HtmlParser 实现简易爬虫")(3回/2861阅,3年前)
* [Android 与 HttpClient 通讯出现乱码问题的解决](http://www.oschina.net/question/54100_31016 "Android 与 HttpClient 通讯出现乱码问题的解决")(0回/223阅,10个月前)
* [Apache HttpClient Cookie rejected 解决方法](http://www.oschina.net/question/54100_34472 "Apache HttpClient Cookie rejected 解决方法")(1回/279阅,9个月前)
* [豆瓣API求帮助](http://www.oschina.net/question/53600_13088 "豆瓣API求帮助")(0回/245阅,1年前)
* [怎么自动注册一个账户，类似yahoo.com](http://www.oschina.net/question/96637_11083 "怎么自动注册一个账户，类似yahoo.com")(18回/883阅,2年前)
* [Android Http异步请求，Callback](http://www.oschina.net/question/54100_33230 "Android Http异步请求，Callback")(0回/650阅,9个月前)
* [Android中的几种网络请求方式详解](http://www.oschina.net/question/166763_35390 "Android中的几种网络请求方式详解")(0回/1424阅,8个月前)
* [打造一款 Android 联网 tic-tac-toe 游戏](http://www.oschina.net/question/12_29683 "打造一款 Android 联网 tic-tac-toe 游戏")(4回/709阅,11个月前)
* [HttpClient, 使用C#操作Web](http://www.oschina.net/question/54100_34407 "HttpClient, 使用C#操作Web")(0回/35阅,9个月前)

© 开源中国社区(OsChina.NET) | [关于我们](http://www.oschina.net/home/about) | [广告联系](mailto:oschina.net@gmail.com) | [@新浪微博](http://weibo.com/oschina2010) | [开源中国手机版](http://m.oschina.net/) | 粤ICP备12009483号-3

[]()[]()[]()
![]()
