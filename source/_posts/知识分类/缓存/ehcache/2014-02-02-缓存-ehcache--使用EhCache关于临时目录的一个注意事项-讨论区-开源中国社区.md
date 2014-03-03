---
layout: post
title: "使用 EhCache 关于临时目录的一个注意事项 - 讨论区 - 开源中国社区"
categories: 缓存
tags: 
 - 缓存
 - ehcache
--- 

# 使用 EhCache 关于临时目录的一个注意事项 - 讨论区 - 开源中国社区

* [首页](http://www.oschina.net/)
* [开源软件](http://www.oschina.net/project)
* [讨论区](http://www.oschina.net/question)
* [代码分享](http://www.oschina.net/code/list)
* [资讯](http://www.oschina.net/news)
* [博客](http://www.oschina.net/blog)
* [Android](http://www.oschina.net/android)
* [招聘](http://www.oschina.net/job)

当前访客身份：游客 [ [登录](http://www.oschina.net/home/login) | [加入开源中国](http://www.oschina.net/home/reg) ]

当前位置：[讨论区](http://www.oschina.net/question) » [技术分享](http://www.oschina.net/question?catalog=2) » [EhCache](http://www.oschina.net/p/ehcache)

软件 代码 讨论区 新闻 博客
[![红薯]( "红薯")](http://my.oschina.net/javayou)

# [使用 EhCache 关于临时目录的一个注意事项]()

[红薯](http://my.oschina.net/javayou) 发表于 8-20 09:24 3年前, [0](http://www.oschina.net/question/12_2368#answers)回/524阅
**[讨论区](http://www.oschina.net/question) » [技术分享](http://www.oschina.net/question?catalog=2)**

【杭州】开源中国-源创会第十三期开始报名 [我要报名»](http://www.oschina.net/question/28_72721)
一般 ehcache 的配置中默认的 diskStore 的路径设置的是 java.io.tmpdir ，等于是当前系统的临时目录。

但是在 Tomcat  和 Resin 这两个应用服务器上，临时目录是有区别的，在 Tomcat 上运行的应用通过 java.io.tmpdir 系统变量获取到的路径是 [Tomcat](http://www.oschina.net/p/tomcat) 目录下的 temp 子目录，而 [Resin](http://www.oschina.net/p/resin) 返回的是系统的临时目录，linux下可能就是 /tmp

在 Linux 下如果我们使用的是 root 账号来启动 Tomcat 和 Resin 的话，那这个问题就不存在。但是我们非常不建议用 root 来启动 Tomcat 和 Resin，这时候我们会单独的创建一个非特权账号，假设该账号名为 www 来运行应用服务器。

我们需要将 Tomcat 和 Resin 所在的目录授权给 www 账号，这样应用服务器的日志文件才能正常的写入，但是由于 Resin 的临时目录是对应系统的 /tmp 目录，因此如果应用中使用了 ehcache 并设置了存储路径为 java.io.tmpdir ，你就会发现启动的时候报错，提示没有在 /tmp 目录下创建文件的权限，这是因为 www 账号没有写 tmp 目录的权限。

解决的办法就是修改 ehcache 的 diskStore 配置的值为  user.home ，将存储文件路径指定到用户的主目录下即可。

而 Tomcat 就没有这个问题，因为它的临时目录在 {tomcat}/temp ，而整个 {tomcat} 都已经授权给 www 账号了。

**标签：** [EhCache](http://www.oschina.net/question/tag/ehcache "Java缓存框架 EhCache") [缓存](http://www.oschina.net/question/tag/cache)
[补充话题说明»]()

**分享到** []( "分享到新浪微博")[]( "分享到腾讯微博")**

[收藏]( "收藏此话题")
**

3
**

[举报]()
**

[踩]( "踩：这问题不知道在说什么，或者没什么用") 0 | [顶]( "顶：这问题很有用或者很清晰明了") 0
**
## [按默认排序](http://www.oschina.net/question/12_2368#answers) | [显示最新评论](http://www.oschina.net/question/12_2368?sort=time#answers) | [回页面顶部](http://www.oschina.net/question/12_2368#top)  []()共有*0*个评论 [发表评论»](http://www.oschina.net/question/12_2368#answerform)

[![非会员用户]( "非会员用户")](http://www.oschina.net/question/$link.user($g_user))   []( "粗体(Ctrl+B)")[]( "斜体(Ctrl+I)")[]( "下划线(Ctrl+U)")[]( "删除线")[]( "删除格式")[]( "编号")[]( "项目符号")[]( "文字颜色")[]( "文字背景")[]( "字体")[]( "文字大小")[]( "超级链接")[]( "取消超级链接")[]( "插入表情")[]( "插入程序代码或脚本")[]( "图片")[]( "插入Flash")[]( "引用某段文字")[]( "全选")[]( "HTML代码")[]( "关于")
[回评论顶部](http://www.oschina.net/question/12_2368#answers) | [回页面顶部](http://www.oschina.net/question/12_2368#top)
有什么技术问题吗？ [我要提问](http://www.oschina.net/question/ask)

**[全部(4786)...](http://my.oschina.net/javayou/?ft=bbs&scope=2&showme=1)*红薯*的其他问题**

* [解读 Oracle 12c 的 12 个新特性](http://www.oschina.net/question/12_73010 "解读 Oracle 12c 的 12 个新特性")(19回/2930阅,昨天(15:20))
* [配置 Gitosis](http://www.oschina.net/question/12_72989 "配置 Gitosis")(0回/17阅,昨天(13:10))
* [Git 服务器 Gitosis 架设指南](http://www.oschina.net/question/12_72988 "Git 服务器 Gitosis 架设指南")(1回/127阅,昨天(13:06))
* [MySQL 5.6.7-RC 的 tpcc-mysql 基准测试结果](http://www.oschina.net/question/12_72813 "MySQL 5.6.7-RC 的 tpcc-mysql 基准测试结果")(6回/1397阅,2天前)
* [使用 Web API 作为动态 TypeScript 编译器运行环境](http://www.oschina.net/question/12_72796 "使用 Web API 作为动态 TypeScript 编译器运行环境")(15回/3539阅,2天前)
**类似的话题**

* [发布 oschina 缓存管理的源码，基于 ehcache](http://www.oschina.net/question/12_12938 "发布 oschina 缓存管理的源码，基于 ehcache")(4回/899阅,1年前)
* [快来看看 EhCache 卖掉后有多恶心](http://www.oschina.net/question/12_3699 "快来看看 EhCache 卖掉后有多恶心")(8回/1928阅,2年前)
* [关于Ehcache集群缓存在应用重启后的加载问题](http://www.oschina.net/question/107963_13557 "关于Ehcache集群缓存在应用重启后的加载问题")(0回/453阅,1年前)
* [2010年3月9日ehcache 2.0发布](http://www.oschina.net/question/62530_4270 "2010年3月9日ehcache 2.0发布")(3回/165阅,2年前)
* [EhCache的网友评论](http://www.oschina.net/question/12_2940 "EhCache的网友评论")(1回/186阅,2年前)
* [深入探讨在集群环境中使用 EhCache 缓存系统](http://www.oschina.net/question/12_7773 "深入探讨在集群环境中使用 EhCache 缓存系统")(4回/1372阅,2年前)
* [下载 EhCache 集群演示程序](http://www.oschina.net/question/12_3984 "下载 EhCache 集群演示程序")(29回/8486阅,2年前)
* [EhCache在acegi中的应用](http://www.oschina.net/question/1_5194 "EhCache在acegi中的应用")(1回/480阅,3年前)
* [ehcache集群时同步不了](http://www.oschina.net/question/102297_17302 "ehcache集群时同步不了")(3回/501阅,1年前)
* [在 JPA、Hibernate 和 Spring 中配置 Ehcache 缓存](http://www.oschina.net/question/54100_31552 "在 JPA、Hibernate 和 Spring 中配置 Ehcache 缓存")(2回/967阅,11个月前)
* [Ehcache 2.0 支持新的 Hibernate 3.3/3.5 缓存 SPI](http://www.oschina.net/question/12_4358 "Ehcache 2.0 支持新的 Hibernate 3.3/3.5 缓存 SPI")(1回/701阅,2年前)
* [EHCache 初步使用指南](http://www.oschina.net/question/1_5192 "EHCache 初步使用指南 ")(0回/520阅,3年前)
* [EhCache CacheManager 初次获取key值对象为空](http://www.oschina.net/question/98610_14858 "EhCache CacheManager 初次获取key值对象为空")(4回/394阅,1年前)
* [简述 EhCache 的几个模块](http://www.oschina.net/question/8676_3506 "简述 EhCache 的几个模块")(1回/1623阅,2年前)
* [找到了 OSChina 出问题的原因了～～](http://www.oschina.net/question/12_8718 "找到了 OSChina 出问题的原因了～～")(2回/539阅,2年前)
* [关于ehcahce配置](http://www.oschina.net/question/163220_62999 "关于ehcahce配置")(0回/138阅,2个月前)

© 开源中国社区(OsChina.NET) | [关于我们](http://www.oschina.net/home/about) | [广告联系](mailto:oschina.net@gmail.com) | [@新浪微博](http://weibo.com/oschina2010) | [开源中国手机版](http://m.oschina.net/) | 粤ICP备12009483号-3

[]()[]()[]()
![]()
