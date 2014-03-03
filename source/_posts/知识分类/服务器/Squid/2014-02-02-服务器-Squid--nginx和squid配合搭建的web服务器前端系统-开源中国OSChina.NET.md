---
layout: post
title: "nginx和squid配合搭建的web服务器前端系统 - 开源中国 OSChina.NET"
categories: 服务器
tags: 
 - 服务器
 - Squid
--- 

# nginx和squid配合搭建的web服务器前端系统 - 开源中国 OSChina.NET

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

当前访客身份：游客 [ [登录](http://www.oschina.net/home/login?goto_page=http%3A%2F%2Fwww.oschina.net%2Fquestion%2F54100_11156) | [加入开源中国](http://www.oschina.net/home/reg) ]

[开源中国](http://www.oschina.net/ "OSChina 开源中国")

# [讨论区](http://www.oschina.net/question)

当前位置： [讨论区](http://www.oschina.net/question) » [技术分享](http://www.oschina.net/question?catalog=2) » [Nginx](http://www.oschina.net/p/nginx)    搜 索
[![鉴客]( "鉴客")](http://my.oschina.net/javaeye)

# [nginx和squid配合搭建的web服务器前端系统]()

[鉴客](http://my.oschina.net/javaeye) 发表于 2010-9-13 07:46 2年前, [3](http://www.oschina.net/question/54100_11156#answers)回/5096阅, 最后回答: 4个月前

Java、PHP、Ruby、iOS、Python 等 JetBrains 开发工具低至 99 元（3折），[详情»](http://www.oschina.net/shop/jetbrains)

这个架构是目前我个人觉得比较稳妥并且最方便的架构，易于多数人接受：
![]()
前端的lvs和squid，按照安装方法，把epoll打开，配置文件照搬，基本上问题不多。
这个架构和app_squid架构的区别，也是关键点就是：加入了一级中层代理，中层代理的好处实在太多了：
1、gzip压缩
压缩可以通过nginx做，这样，后台应用服务器不管是apache、resin、lighttpd甚至iis或其他古怪服务器，都不用考虑压缩的功能问题。
2、负载均衡和故障屏蔽
nginx可以作为负载均衡代理使用，并有故障屏蔽功能，这样，根据目录甚至一个正则表达式来制定负载均衡策略变成了小case。
3、方便的运维管理，在各种情况下可以灵活制订方案。
例如，如果有人用轻量级的ddos穿透squid进行攻击，可以在中层代理想办法处理掉；访问量和后台负载突变时，可以随时把一个域名或一个目录的请求扔入二级cache服务器；可以很容易地控制no-cache和expires等header。等等功能。。。
4、权限清晰
这台机器就是不写程序的维护人员负责，程序员一般不需要管理这台机器，这样假如出现故障，很容易能找到正确的人。
对于应用服务器和数据库服务器，最好是从维护人员的视线中消失，我的目标是，这些服务只要能跑得起来就可以了，其它的事情全部可以在外部处理掉。

**标签：** [Nginx](http://www.oschina.net/question/tag/nginx "高性能Web服务器 Nginx") [Squid](http://www.oschina.net/question/tag/squid "代理服务器 Squid") [LVS](http://www.oschina.net/question/tag/lvs "Linux虚拟服务器 LVS")
[补充话题说明»]()

**分享到** []( "分享到新浪微博")[]( "分享到腾讯微博")**

[收藏]( "收藏此话题")
**

21
**

[举报]()
**

[踩]( "踩：这问题不知道在说什么，或者没什么用") 0 | [顶]( "顶：这问题很有用或者很清晰明了") 1
**
## [按默认排序](http://www.oschina.net/question/54100_11156#answers) | [显示最新评论](http://www.oschina.net/question/54100_11156?sort=time#answers) | [回页面顶部](http://www.oschina.net/question/54100_11156#top)  []()共有*3*个评论 [发表评论»](http://www.oschina.net/question/54100_11156#answerform)

* [![江边望海]( "江边望海")](http://my.oschina.net/jiangbianwanghai)

[江边望海](http://my.oschina.net/jiangbianwanghai) 回答于 2012-02-17 08:25

[举报]()
非常欣赏你的系统架构设计 ![]()
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=11156&answer=160393)
* [![中共中央]( "中共中央")](http://my.oschina.net/CCP)

[中共中央](http://my.oschina.net/CCP) 回答于 2012-06-02 18:54

[举报]()
灰常欣赏！
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=11156&answer=228730)
* [![隋济远]( "隋济远")](http://my.oschina.net/u/876254)

[隋济远](http://my.oschina.net/u/876254) 回答于 2012-11-29 10:10

[举报]()
受益，谢谢。
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=11156&answer=360616)

[![非会员用户]( "非会员用户")]()
[回评论顶部](http://www.oschina.net/question/54100_11156#answers) | [回页面顶部](http://www.oschina.net/question/54100_11156#top)

[![]()](http://www.oschina.net/action/visit/ad?id=1033 "JPush——极光推送")

有什么技术问题吗？ [我要提问](http://www.oschina.net/question/ask)
**[全部(1710)...](http://my.oschina.net/javaeye/?ft=bbs&scope=2&showme=1)*鉴客*的其他问题**

* [开源技术解决方案与商业产品的对应表](http://www.oschina.net/question/54100_107100 "开源技术解决方案与商业产品的对应表")(2回/269阅,5天前)
* [MYSQL 数据丢失讨论](http://www.oschina.net/question/54100_105530 "MYSQL 数据丢失讨论")(7回/458阅,15天前)
* [IP地理位置数据库（ip-to-country.csv）130401 英文与简繁中文版](http://www.oschina.net/question/54100_103459 "IP地理位置数据库（ip-to-country.csv）130401 英文与简繁中文版")(4回/569阅,24天前)
* [libuv 初窥](http://www.oschina.net/question/54100_103457 "libuv 初窥")(2回/206阅,24天前)
* [37signals员工的办公环境，太爽了！！！](http://www.oschina.net/question/54100_103022 "37signals员工的办公环境，太爽了！！！")(37回/4039阅,27天前)

**类似的话题**

* [利用 nginx url hash 提高squid服务器命中率](http://www.oschina.net/question/12_622 "利用 nginx url hash 提高squid服务器命中率")(0回/1274阅,3年前)
* [Nginx 作为最前端的 web cache 系统](http://www.oschina.net/question/54100_11154 "Nginx 作为最前端的 web cache 系统")(0回/1076阅,2年前)
* [squid配合nginx的gzip压缩的完美解决方案](http://www.oschina.net/question/17_3972 "squid配合nginx的gzip压缩的完美解决方案")(1回/2169阅,3年前)
* [在Squid后面的Nginx如何记录客户端IP](http://www.oschina.net/question/12_5084 "在Squid后面的Nginx如何记录客户端IP")(0回/300阅,4年前)
* [【PPT分享】某大型社区网站系统](http://www.oschina.net/question/12_12903 "【PPT分享】某大型社区网站系统")(1回/1250阅,2年前)
* [新型的大型bbs架构（squid+nginx）](http://www.oschina.net/question/54100_11155 "新型的大型bbs架构（squid+nginx）")(3回/4623阅,2年前)
* [如何修改Web服务器的Server值](http://www.oschina.net/question/12_5543 "如何修改Web服务器的Server值")(0回/165阅,4年前)
* [【PPT分享】天涯大型bbs社区网站系统.ppt](http://www.oschina.net/question/54100_11157 "【PPT分享】天涯大型bbs社区网站系统.ppt")(2回/2600阅,2年前)
* [Squid应用 高效配置Linux系统代理服务器](http://www.oschina.net/question/12_6962 "Squid应用 高效配置Linux系统代理服务器  ")(0回/1046阅,3年前)
* [Squid 安装调试过程中的几个常用命令](http://www.oschina.net/question/12_8153 "Squid 安装调试过程中的几个常用命令")(0回/919阅,3年前)
* [利用 squid 反向代理提高网站性能](http://www.oschina.net/question/12_295 "利用 squid 反向代理提高网站性能")(4回/2635阅,4年前)
* [有没有人做过squid+wccp！！跪求！！！！谢谢各位！！！](http://www.oschina.net/question/44278_3403 "有没有人做过squid+wccp！！跪求！！！！谢谢各位！！！")(15回/756阅,3年前)
* [批量更新 squid 缓存。](http://www.oschina.net/question/17_3563 "批量更新 squid 缓存。")(0回/913阅,3年前)
* [Squid 3.0 更新某连接缓存的方法...其实太简单了!!](http://www.oschina.net/question/17_4996 "Squid 3.0 更新某连接缓存的方法...其实太简单了!! ")(0回/128阅,4年前)
* [squid 2.6.0 配置方法](http://www.oschina.net/question/17_4997 "squid 2.6.0 配置方法")(0回/79阅,4年前)
* [用squid再次疯狂加速你的web](http://www.oschina.net/question/1_5230 "用squid再次疯狂加速你的web")(0回/113阅,4年前)

© 开源中国(OsChina.NET) | [关于我们](http://www.oschina.net/home/about) | [广告联系](mailto:oschina.net@gmail.com) | [@新浪微博](http://weibo.com/oschina2010) | [开源中国手机版](http://m.oschina.net/) | [粤ICP备12009483号-3](http://www.miitbeian.gov.cn/) 开源中国手机客户端： [Android](http://www.oschina.net/app "Android客户端") [iPhone](http://www.oschina.net/app "iPhone 客户端") [WP7](http://www.oschina.net/app "Windows Phone 客户端")

[]()[]()[]()
![]()
