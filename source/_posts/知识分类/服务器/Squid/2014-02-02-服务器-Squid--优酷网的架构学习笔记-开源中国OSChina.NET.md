---
layout: post
title: "优酷网的架构学习笔记 - 开源中国 OSChina.NET"
categories: 服务器
tags: 
 - 服务器
 - Squid
--- 

# 优酷网的架构学习笔记 - 开源中国 OSChina.NET

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

当前访客身份：游客 [ [登录](http://www.oschina.net/home/login?goto_page=http%3A%2F%2Fwww.oschina.net%2Fquestion%2F12_32460) | [加入开源中国](http://www.oschina.net/home/reg) ]

[开源中国](http://www.oschina.net/ "OSChina 开源中国")

# [讨论区](http://www.oschina.net/question)

当前位置： [讨论区](http://www.oschina.net/question) » [技术分享](http://www.oschina.net/question?catalog=2) » [MySQL](http://www.oschina.net/p/mysql)    搜 索
[![红薯]( "红薯")](http://my.oschina.net/javayou)

# [优酷网的架构学习笔记]()

[红薯](http://my.oschina.net/javayou) 发表于 2011-11-21 23:23 1年前, [32](http://www.oschina.net/question/12_32460#answers)回/12340阅, 最后回答: 21天前

Java、PHP、Ruby、iOS、Python 等 JetBrains 开发工具低至 99 元（3折），[详情»](http://www.oschina.net/shop/jetbrains)

记得以前给大家介绍过视频网站龙头老大[YouTube的技术架构](http://www.oschina.net/question/12_32459)， 相信大家看了都会有不少的感触，互联网就是这么一个神奇的东西。今天我突然想到，优酷网在国内也算是视频网站的老大了，不知道他的架构相对于 YouTube是怎么样的，于是带着这个好奇心去网上找了优酷网架构的各方面资料，虽然谈得没有YouTube那么详细，但多少还是挖掘了一点，现在总结 一下，希望对喜欢架构的朋友有所帮助。

一、网站基本数据概览

* 据2010年统计，优酷网日均独立访问人数（uv)达到了8900万，日均访问量（pv）更是达到了17亿，优酷凭借这一数据成为google榜单中国内视频网站排名最高的厂商。
* 硬件方面，优酷网引进的戴尔服务器主要以 PowerEdge 1950与PowerEdge 860为主，存储阵列以戴尔MD1000为主，2007的数据表明，优酷网已有1000多台服务器遍布在全国各大省市，现在应该更多了吧。

二、网站前端框架

从一开始，优酷网就自建了一套CMS来解决前端的页面显示，各个模块之间分离得比较恰当，前端可扩展性很好，UI的分离，让开发与维护变得十分简单和灵活，下图是优酷前端的模块调用关系：

![]()

这样，就根据module、method及params来确定调用相对独立的模块，显得非常简洁。下面附一张优酷的前端局部架构图：

![]()

 

三、数据库架构

应该说优酷的数据库架构也是经历了许多波折，从一开始的单台MySQL服务器（Just Running）到简单的MySQL主从复制、SSD优化、垂直分库、水平sharding分库，这一系列过程只有经历过才会有更深的体会吧，就像[MySpace的架构经历](http://www.itivy.com/ivy/archive/2011/3/7/634351257301504864.html)一样，架构也是一步步慢慢成长和成熟的。

1、简单的MySQL主从复制:

MySQL的主从复制解决了数据库的读写分离，并很好的提升了读的性能，其原来图如下：

![]()

其主从复制的过程如下图所示：

![]()

但是，主从复制也带来其他一系列性能瓶颈问题：

1. 写入无法扩展
1. 写入无法缓存
1. 复制延时
1. 锁表率上升
1. 表变大，缓存率下降

那问题产生总得解决的，这就产生下面的优化方案，一起来看看。

2、MySQL垂直分区

如果把业务切割得足够独立，那把不同业务的数据放到不同的数据库服务器将是一个不错的方案，而且万一其中一个业务崩溃了也不会影响其他业务的正常进行，并且也起到了负载分流的作用，大大提升了数据库的吞吐能力。经过垂直分区后的数据库架构图如下：

![]()

然而，尽管业务之间已经足够独立了，但是有些业务之间或多或少总会有点联系，如用户，基本上都会和每个业务相关联，况且这种分区方式，也不能解决单张表数据量暴涨的问题，因此为何不试试水平sharding呢？

3、MySQL水平分片（Sharding）

这是一个非常好的思路，将用户按一定规则（按id哈希）分组，并把该组用户的数据存储到一个数据库分片中，即一个sharding，这样随着用户数量的增加，只要简单地配置一台服务器即可，原理图如下：

![]()

如何来确定某个用户所在的shard呢，可以建一张用户和shard对应的数据表，每次请求先从这张表找用户的shard id，再从对应shard中查询相关数据，如下图所示：

![]()

但是，优酷是如何解决跨shard的查询呢，这个是个难点，据介绍优酷是尽量不跨shard查询，实在不行通过多维分片索引、分布式搜索引擎，下策是分布式数据库查询（这个非常麻烦而且耗性能）

 

四、缓存策略

貌似大的系统都对“缓存”情有独钟，从http缓存到memcached内存数据缓存，但优酷表示没有用内存缓存，理由如下：

1. 避免内存拷贝，避免内存锁
1. 如接到老大哥通知要把某个视频撤下来，如果在缓存里是比较麻烦的

而且Squid 的 write() 用户进程空间有消耗，Lighttpd 1.5 的 AIO(异步I/O) 读取文件到用户内存导致效率也比较低下。

但为何我们访问优酷会如此流畅，与土豆相比优酷的视频加载速度略胜一筹？这个要归功于优酷建立的比较完善的内容分发网络（CDN），它 通过多种方式保证分布在全国各地的用户进行就近访问——用户点击视频请求后，优酷网将根据用户所处地区位置，将离用户最近、服务状况最好的视频服务器地址 传送给用户，从而保证用户可以得到快速的视频体验。这就是CDN带来的优势，就近访问，有关CDN的更多内容，请大家Google一下。

出处：[青藤屋](http://www.itivy.com/ivy) [原文链接](http://www.itivy.com/ivy/archive/2011/8/13/the-architecture-of-youku.html)

**标签：** [MySQL](http://www.oschina.net/question/tag/mysql "数据库服务器 MySQL") [Squid](http://www.oschina.net/question/tag/squid "代理服务器 Squid") [Lighttpd](http://www.oschina.net/question/tag/lighttpd "高性能Web服务器 Lighttpd") [memcached](http://www.oschina.net/question/tag/memcached "集中式缓存系统 memcached")
[补充话题说明»]()

**分享到** []( "分享到新浪微博")[]( "分享到腾讯微博")**

[收藏]( "收藏此话题")
**

202
**

[举报]()
**

[踩]( "踩：这问题不知道在说什么，或者没什么用") 0 | [顶]( "顶：这问题很有用或者很清晰明了") 4
**
## [按默认排序](http://www.oschina.net/question/12_32460#answers) | [显示最新评论](http://www.oschina.net/question/12_32460?sort=time#answers) | [回页面顶部](http://www.oschina.net/question/12_32460#top)  []()共有*32*个评论 [发表评论»](http://www.oschina.net/question/12_32460#answerform)

* [![lace]( "lace")](http://my.oschina.net/u/32103)

[lace](http://my.oschina.net/u/32103) 回答于 2011-11-22 08:20

[举报]()
层次有限，我表示看不明白。
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32460&answer=132545)
* [![ikeng]( "ikeng")](http://my.oschina.net/u/129005)

[ikeng](http://my.oschina.net/u/129005) 回答于 2011-11-22 09:15

[举报]()
最后一段霸气了，用钱砸
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32460&answer=132562)
* [![_Aaron_]( "_Aaron_")](http://my.oschina.net/keven)

[_Aaron_](http://my.oschina.net/keven) 回答于 2011-11-22 09:16

[举报]()
“老大哥”，是亮点~![]()
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32460&answer=132565)
* [![JacarriChan]( "JacarriChan")](http://my.oschina.net/772929989)

[JacarriChan](http://my.oschina.net/772929989) 回答于 2011-11-22 09:23

[举报]()
youku 确实比较流畅。其技术架构也比较简洁，值得学习……
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32460&answer=132569)
* [![天赐]( "天赐")](http://my.oschina.net/tianci)

[天赐](http://my.oschina.net/tianci) 回答于 2011-11-22 09:27

[举报]()
o(∩_∩)o...哈哈！！！

挺不错
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32460&answer=132575)
* [![YiseNet]( "YiseNet")](http://my.oschina.net/yisenn)

[YiseNet](http://my.oschina.net/yisenn) 回答于 2011-11-22 09:48

[举报]()
1. 如接到老大哥通知要把某个视频撤下来，如果在缓存里是比较麻烦的
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32460&answer=132589)
1. [![liueric]( "liueric")](http://my.oschina.net/breaker)

[liueric](http://my.oschina.net/breaker) 回答于 2011-11-22 09:51

[举报]()
其实cdn也用了内存缓存的，优酷只是把内存缓存的责任归到cdn身上而已
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32460&answer=132591)
1. [![方旭]( "方旭")](http://my.oschina.net/fangxu)

[方旭](http://my.oschina.net/fangxu) 回答于 2011-11-22 10:00

[举报]()
想不明白优酷没有使用内存缓存:)

理由都很牵强，要说有强大的CDN也不能说过去。

网站做大了，缓存是必修课。 当然有钱确实就不一样。
**--- 共有 1 条评论 ---**

* [![苏大泉]( "苏大泉")](http://my.oschina.net/littlebird)  上面说的对 它把缓存的功能交给cdn做了  (8个月前 by 苏大泉) [回复]()

[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(1)* | [引用此评论](http://www.oschina.net/question/answer?question=32460&answer=132596)
* [![puras]( "puras")](http://my.oschina.net/puras)

[puras](http://my.oschina.net/puras) 回答于 2011-11-22 10:12

[举报]()
整成PDF吧。好东西啊
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32460&answer=132601)
* [![memeyang]( "memeyang")](http://my.oschina.net/aboutblankchina)

[memeyang](http://my.oschina.net/aboutblankchina) 回答于 2011-11-22 10:37

[举报]()
老大哥 亮了。
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=32460&answer=132612)

* [1](http://www.oschina.net/question/12_32460?sort=default&p=1)
* [2](http://www.oschina.net/question/12_32460?sort=default&p=2#answers)
* [3](http://www.oschina.net/question/12_32460?sort=default&p=3#answers)
* [4](http://www.oschina.net/question/12_32460?sort=default&p=4#answers)
* [>](http://www.oschina.net/question/12_32460?sort=default&p=2#answers)

[![非会员用户]( "非会员用户")]()
[回评论顶部](http://www.oschina.net/question/12_32460#answers) | [回页面顶部](http://www.oschina.net/question/12_32460#top)

[![]()](http://www.oschina.net/action/visit/ad?id=1033 "JPush——极光推送")

有什么技术问题吗？ [我要提问](http://www.oschina.net/question/ask)
**[全部(4926)...](http://my.oschina.net/javayou/?ft=bbs&scope=2&showme=1)*红薯*的其他问题**

* [类似 DNSPod 智能 DNS 解析无效的一种情形](http://www.oschina.net/question/12_106724 "类似 DNSPod 智能 DNS 解析无效的一种情形")(3回/198阅,7天前)
* [Ruby 2.0 中未被提及的特性](http://www.oschina.net/question/12_106555 "Ruby 2.0 中未被提及的特性")(0回/128阅,8天前)
* [2013 第一期：OSC 深圳源创会高清无码大片放映](http://www.oschina.net/question/12_106453 "2013 第一期：OSC 深圳源创会高清无码大片放映")(106回/7120阅,9天前)
* [PostgreSQL 9.3 新特性预览 —— JSON 操作](http://www.oschina.net/question/12_106368 "PostgreSQL 9.3 新特性预览 —— JSON 操作")(18回/4385阅,10天前)
* [号召大家给 @滔哥 和 @走在路上 送上祝福](http://www.oschina.net/question/12_106012 "号召大家给 @滔哥 和 @走在路上 送上祝福")(35回/2011阅,12天前)

**类似的话题**

* [谈谈对于企业级系统架构的理解](http://www.oschina.net/question/54100_20535 "谈谈对于企业级系统架构的理解")(4回/435阅,1年前)
* [架构腐化之谜](http://www.oschina.net/question/12_23278 "架构腐化之谜")(12回/951阅,1年前)

© 开源中国(OsChina.NET) | [关于我们](http://www.oschina.net/home/about) | [广告联系](mailto:oschina.net@gmail.com) | [@新浪微博](http://weibo.com/oschina2010) | [开源中国手机版](http://m.oschina.net/) | [粤ICP备12009483号-3](http://www.miitbeian.gov.cn/) 开源中国手机客户端： [Android](http://www.oschina.net/app "Android客户端") [iPhone](http://www.oschina.net/app "iPhone 客户端") [WP7](http://www.oschina.net/app "Windows Phone 客户端")

[]()[]()[]()
![]()
