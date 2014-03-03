---
layout: post
title: "YouTube 架构学习体会 - 开源中国 OSChina.NET"
categories: 架构师
tags: 
 - 架构师
--- 

# YouTube 架构学习体会 - 开源中国 OSChina.NET

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

当前访客身份：游客 [ [登录](http://www.oschina.net/home/login?goto_page=http%3A%2F%2Fwww.oschina.net%2Fquestion%2F12_32459) | [加入开源中国](http://www.oschina.net/home/reg) ]

[开源中国](http://www.oschina.net/ "OSChina 开源中国")

# [讨论区](http://www.oschina.net/question)

当前位置： [讨论区](http://www.oschina.net/question) » [技术分享](http://www.oschina.net/question?catalog=2) » [MySQL](http://www.oschina.net/p/mysql)    搜 索
[![红薯]( "红薯")](http://my.oschina.net/javayou)

# [YouTube 架构学习体会]()

[红薯](http://my.oschina.net/javayou) 发表于 2011-11-21 23:20 1年前, [19](http://www.oschina.net/question/12_32459#answers)回/7508阅, 最后回答: 5个月前

Java、PHP、Ruby、iOS、Python 等 JetBrains 开发工具低至 99 元（3折），[详情»](http://www.oschina.net/shop/jetbrains)

这几天一直在关注和学习一些大型网站的架构，希望有一天自己也能设计一个高并发、高容错的系统并能应用在实践上。今天在网上找架构相关的资料时，看到一个被和谐的视频网站YouTube的架构分析，看了以后觉得自己又向架构走近了一步，于是赶快拿出来与大家一起分享。
YouTube发展迅速，每天超过1亿的视频点击量，但只有很少人在维护站点和确保伸缩性。这点和PlentyOfFish类似，少数人维护庞大系统。是什么原因呢？放心绝对不是靠人品，也不是靠寂寞，下面就来看看YouTube的整体技术架构吧。
平台
    
Apache
Python
Linux(SuSe)
MySQL
psyco，一个动态的Python到C的编译器
lighttpd代替Apache做视频查看</strong>
状态
支持每天超过1亿的视频点击量
成立于2005年2月
于2006年3月达到每天3千万的视频点击量
于2006年7月达到每天1亿的视频点击量
2个系统管理员，2个伸缩性软件架构师
2个软件开发工程师，2个网络工程师，1个DBA</strong>
Web服务器
1，NetScaler用于负载均衡和静态内容缓存
2，使用mod_fast_cgi运行Apache
3，使用一个Python应用服务器来处理请求的路由
4，应用服务器与多个数据库和其他信息源交互来获取数据和格式化html页面
5，一般可以通过添加更多的机器来在Web层提高伸缩性
6，Python的Web层代码通常不是性能瓶颈，大部分时间阻塞在RPC
7，Python允许快速而灵活的开发和部署
8，通常每个页面服务少于100毫秒的时间
9，使用psyco(一个类似于JIT编译器的动态的Python到C的编译器)来优化内部循环
10，对于像加密等密集型CPU活动，使用C扩展
11，对于一些开销昂贵的块使用预先生成并缓存的html
12，数据库里使用行级缓存
13，缓存完整的Python对象

14，有些数据被计算出来并发送给各个程序，所以这些值缓存在本地内存中。这是个使用不当的策略。

应用服务器里最快的缓存将预先计算的值发送给所有服务器也花不了多少时间。只需弄一个代理来监听更改，预计算，然后发送。
视频服务
1，花费包括带宽，硬件和能源消耗
2，每个视频由一个迷你集群来host，每个视频被超过一台机器持有
3，使用一个集群意味着：
   -更多的硬盘来持有内容意味着更快的速度
   -failover。如果一台机器出故障了，另外的机器可以继续服务
   -在线备份
4，使用lighttpd作为Web服务器来提供视频服务：
   -Apache开销太大
   -使用epoll来等待多个fds
   -从单进程配置转变为多进程配置来处理更多的连接
5，大部分流行的内容移到CDN：
  -CDN在多个地方备份内容，这样内容离用户更近的机会就会更高
  -CDN机器经常内存不足，因为内容太流行以致很少有内容进出内存的颠簸
6，不太流行的内容(每天1-20浏览次数)在许多colo站点使用YouTube服务器
  -长尾效应。一个视频可以有多个播放，但是许多视频正在播放。随机硬盘块被访问
  -在这种情况下缓存不会很好，所以花钱在更多的缓存上可能没太大意义。
  -调节RAID控制并注意其他低级问题
  -调节每台机器上的内存，不要太多也不要太少 </strong>
视频服务关键点
    
1，保持简单和廉价
2，保持简单网络路径，在内容和用户间不要有太多设备
3，使用常用硬件，昂贵的硬件很难找到帮助文档
4，使用简单而常见的工具，使用构建在Linux里或之上的大部分工具

5，很好的处理随机查找(SATA，tweaks)

缩略图服务
1，做到高效令人惊奇的难
2，每个视频大概4张缩略图，所以缩略图比视频多很多
3，缩略图仅仅host在几个机器上
4，持有一些小东西所遇到的问题：
   -OS级别的大量的硬盘查找和inode和页面缓存问题
   -单目录文件限制，特别是Ext3，后来移到多分层的结构。内核2.6的最近改进可能让 Ext3允许大目录，但在一个文件系统里存储大量文件不是个好主意
   -每秒大量的请求，因为Web页面可能在页面上显示60个缩略图
   -在这种高负载下Apache表现的非常糟糕
   -在Apache前端使用squid，这种方式工作了一段时间，但是由于负载继续增加而以失败告终。它让每秒300个请求变为20个
   -尝试使用lighttpd但是由于使用单线程它陷于困境。遇到多进程的问题，因为它们各自保持自己单独的缓存
   -如此多的图片以致一台新机器只能接管24小时
   -重启机器需要6-10小时来缓存
5，为了解决所有这些问题YouTube开始使用Google的BigTable，一个分布式数据存储：
   -避免小文件问题，因为它将文件收集到一起
   -快，错误容忍
   -更低的延迟，因为它使用分布式多级缓存，该缓存与多个不同collocation站点工作
   -更多信息参考Google Architecture，GoogleTalk Architecture和BigTable
数据库
    
1，早期
   -使用MySQL来存储元数据，如用户，tags和描述
   -使用一整个10硬盘的RAID 10来存储数据
   -依赖于信用卡所以YouTube租用硬件
   -YouTube经过一个常见的革命：单服务器，然后单master和多read slaves，然后数据库分区，然后sharding方式
   -痛苦与备份延迟。master数据库是多线程的并且运行在一个大机器上所以它可以处理许多工作，slaves是单线程的并且通常运行在小一些的服务器上并且备份是异步的，所以slaves会远远落后于master
   -更新引起缓存失效，硬盘的慢I/O导致慢备份
   -使用备份架构需要花费大量的money来获得增加的写性能
   -YouTube的一个解决方案是通过把数据分成两个集群来将传输分出优先次序：一个视频查看池和一个一般的集群
2，后期
   -数据库分区
   -分成shards，不同的用户指定到不同的shards
   -扩散读写
   -更好的缓存位置意味着更少的IO
   -导致硬件减少30%
   -备份延迟降低到0

   -现在可以任意提升数据库的伸缩性

数据中心策略
1，依赖于信用卡，所以最初只能使用受管主机提供商
2，受管主机提供商不能提供伸缩性，不能控制硬件或使用良好的网络协议
3，YouTube改为使用colocation arrangement。现在YouTube可以自定义所有东西并且协定自己的契约
4，使用5到6个数据中心加CDN
5，视频来自任意的数据中心，不是最近的匹配或其他什么。如果一个视频足够流行则移到CDN
6，依赖于视频带宽而不是真正的延迟。可以来自任何colo
7，图片延迟很严重，特别是当一个页面有60张图片时

8，使用BigTable将图片备份到不同的数据中心，代码查看谁是最近的

学到的东西
1，Stall for time。创造性和风险性的技巧让你在短期内解决问题而同时你会发现长期的解决方案
2，Proioritize。找出你的服务中核心的东西并对你的资源分出优先级别
3，Pick your battles。别怕将你的核心服务分出去。YouTube使用CDN来分布它们最流行的内容。创建自己的网络将花费太多时间和太多money
4，Keep it simple！简单允许你更快的重新架构来回应问题
5，Shard。Sharding帮助隔离存储，CPU，内存和IO，不仅仅是获得更多的写性能
6，Constant iteration on bottlenecks：
   -软件：DB，缓存
   -OS：硬盘I/O
   -硬件：内存，RAID
7，You succeed as a team。拥有一个跨越条律的了解整个系统并知道系统内部是什么样的团队，如安装打印机，安装机器，安装网络等等的人。

   With a good team all things are possible。

文章出处：[http://www.itivy.com/ivy/archive/2011/3/6/634350416046298451.html](http://www.itivy.com/ivy/archive/2011/3/6/634350416046298451.html)

**标签：** [MySQL](http://www.oschina.net/question/tag/mysql "数据库服务器 MySQL") [Nginx](http://www.oschina.net/question/tag/nginx "高性能Web服务器 Nginx") [Lighttpd](http://www.oschina.net/question/tag/lighttpd "高性能Web服务器 Lighttpd") [Apache](http://www.oschina.net/question/tag/apache+http+server "HTTP服务器 Apache") [Youtube](http://www.oschina.net/question/tag/Youtube)
[补充话题说明»]()

**分享到** []( "分享到新浪微博")[]( "分享到腾讯微博")**

[收藏]( "收藏此话题")
**

134
**

[举报]()
**

[踩]( "踩：这问题不知道在说什么，或者没什么用") 0 | [顶]( "顶：这问题很有用或者很清晰明了") 3
**
## [按默认排序](http://www.oschina.net/question/12_32459#answers) | [显示最新评论](http://www.oschina.net/question/12_32459?sort=time#answers) | [回页面顶部](http://www.oschina.net/question/12_32459#top)  []()共有*19*个评论 [发表评论»](http://www.oschina.net/question/12_32459#answerform)

* [![FoxHu]( "FoxHu")](http://my.oschina.net/hil2010)

[FoxHu](http://my.oschina.net/hil2010) 回答于 2011-11-22 18:05

[举报]()
学习了！
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32459&answer=132838)
* [![沈健]( "沈健")](http://my.oschina.net/davidjianl)

[沈健](http://my.oschina.net/davidjianl) 回答于 2011-11-22 18:25

[举报]()
真是只有这么点人？9个人？
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32459&answer=132845)
* [![timo]( "timo")](http://my.oschina.net/u/208276)

[timo](http://my.oschina.net/u/208276) 回答于 2011-11-22 18:52

[举报]()
果然都是精兵强将
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32459&answer=132853)
* [![Richardx]( "Richardx")](http://my.oschina.net/richardx)

[Richardx](http://my.oschina.net/richardx) 回答于 2011-11-22 21:58

[举报]()
最近也在学习大型网站的架构
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32459&answer=132882)
* [![pizigou]( "pizigou")](http://my.oschina.net/pizigou)

[pizigou](http://my.oschina.net/pizigou) 回答于 2011-11-23 09:29

[举报]()
2，后期 -数据库分区 -分成shards，不同的用户指定到不同的shards -扩散读写 -更好的缓存位置意味着更少的IO -导致硬件减少30% -备份延迟降低到0 -现在可以任意提升数据库的伸缩性 这块红薯老大可否解说下？这里说得太笼统，看起来跟初期的结构没什么差别，貌似还增加了管理维护成本。
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32459&answer=132940)
* [![该用户已被和谐]( "该用户已被和谐")](http://my.oschina.net/onlyoutman)

[该用户已被和谐](http://my.oschina.net/onlyoutman) 回答于 2011-11-23 09:37

[举报]()
不错！ ![]()
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32459&answer=132945)
* [![javaspace]( "javaspace")](http://my.oschina.net/u/179641)

[javaspace](http://my.oschina.net/u/179641) 回答于 2011-11-23 21:11

[举报]()
有受益 ![]()
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32459&answer=133248)
* [![黑曜石]( "黑曜石")](http://my.oschina.net/sylar066)

[黑曜石](http://my.oschina.net/sylar066) 回答于 2011-11-23 21:37

[举报]()
我1年前爬到一本youtube的构架PDF 貌似是在sohu的书库  当时我就震精了  偌大一个项目 只有4个engineer 其中哥们 活着只做两件事 一是写代码 二就是烂醉 醒来继续写代码
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32459&answer=133259)
* [![我是常萎]( "我是常萎")](http://my.oschina.net/cool2010)

[我是常萎](http://my.oschina.net/cool2010) 回答于 2011-11-23 21:49

[举报]()
太牛掰了，仅仅一年多就 每天1亿的视频点击，视频是从哪来的，都是网友上传的吗？
**--- 共有 2 条评论 ---**

* [![缪斯的情人]( "缪斯的情人")](http://my.oschina.net/mousai)  回复 [@专业打酱油](http://my.oschina.net/yasenagat) : 我来打假：此人非嫡系，实乃“李鬼” (3个月前 by 缪斯的情人) [回复]()
* [![专业打酱油]( "专业打酱油")](http://my.oschina.net/yasenagat)  [@缪斯的情人](http://my.oschina.net/mousai)  (3个月前 by 专业打酱油) [回复]()

[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(2)* | [引用此评论](http://www.oschina.net/question/answer?question=32459&answer=133267)
* [![笨笨熊]( "笨笨熊")](http://my.oschina.net/lyncn)

[笨笨熊](http://my.oschina.net/lyncn) 回答于 2011-12-08 16:15

[举报]()
牛人基本在国外
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32459&answer=137942)

* [1](http://www.oschina.net/question/12_32459?sort=default&p=1)
* [2](http://www.oschina.net/question/12_32459?sort=default&p=2#answers)
* [>](http://www.oschina.net/question/12_32459?sort=default&p=2#answers)

[![非会员用户]( "非会员用户")]()
[回评论顶部](http://www.oschina.net/question/12_32459#answers) | [回页面顶部](http://www.oschina.net/question/12_32459#top)

[![]()](http://www.oschina.net/action/visit/ad?id=1033 "JPush——极光推送")

有什么技术问题吗？ [我要提问](http://www.oschina.net/question/ask)
**[全部(4926)...](http://my.oschina.net/javayou/?ft=bbs&scope=2&showme=1)*红薯*的其他问题**

* [类似 DNSPod 智能 DNS 解析无效的一种情形](http://www.oschina.net/question/12_106724 "类似 DNSPod 智能 DNS 解析无效的一种情形")(3回/198阅,7天前)
* [Ruby 2.0 中未被提及的特性](http://www.oschina.net/question/12_106555 "Ruby 2.0 中未被提及的特性")(0回/129阅,8天前)
* [2013 第一期：OSC 深圳源创会高清无码大片放映](http://www.oschina.net/question/12_106453 "2013 第一期：OSC 深圳源创会高清无码大片放映")(106回/7138阅,9天前)
* [PostgreSQL 9.3 新特性预览 —— JSON 操作](http://www.oschina.net/question/12_106368 "PostgreSQL 9.3 新特性预览 —— JSON 操作")(18回/4393阅,10天前)
* [号召大家给 @滔哥 和 @走在路上 送上祝福](http://www.oschina.net/question/12_106012 "号召大家给 @滔哥 和 @走在路上 送上祝福")(35回/2014阅,12天前)

**类似的话题**

* [Android手机如何在国内看Youtube](http://www.oschina.net/question/12_1538 "Android手机如何在国内看Youtube")(9回/7770阅,3年前)

© 开源中国(OsChina.NET) | [关于我们](http://www.oschina.net/home/about) | [广告联系](mailto:oschina.net@gmail.com) | [@新浪微博](http://weibo.com/oschina2010) | [开源中国手机版](http://m.oschina.net/) | [粤ICP备12009483号-3](http://www.miitbeian.gov.cn/) 开源中国手机客户端： [Android](http://www.oschina.net/app "Android客户端") [iPhone](http://www.oschina.net/app "iPhone 客户端") [WP7](http://www.oschina.net/app "Windows Phone 客户端")

[]()[]()[]()
![]()
