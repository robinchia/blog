---
layout: post
title: "Twitter架构图(cache篇) – Tim[后端技术]"
categories: 架构师
tags: 
 - 架构师
 - 架构
--- 

# Twitter架构图(cache篇) – Tim[后端技术]

# [Tim[后端技术]](http://timyang.net/ "Home")

Tim's blog, 关于后端架构、互联网技术、分布式、大型网络应用、NoSQL、技术感悟等

[Home](http://timyang.net/) | [About](http://timyang.net/about/) | [English version](http://timyang.net/tag/English/) | [留言(Guestbook)](http://timyang.net/about/#comment) | [![]()](http://timyang.net/feed/)[订阅RSS](http://timyang.net/feed/) [![Add to Google]()](http://fusion.google.com/add?source=atgs&feedurl=http%3A//timyang.net/feed/)
* 
* Email: ![]()

* 
### Similar Posts

* [Notes about Timelines @ Twitter](http://timyang.net/architecture/notes-timelines-twitter/ "May 3, 2012")
* [Twitter“鲸鱼”故障技术剖析](http://timyang.net/tech/twitter-whale/ "March 8, 2010")
* [MemcacheDB, Tokyo Tyrant, Redis performance test](http://timyang.net/data/mcdb-tt-redis/ "August 11, 2009")
* [FarmVille(美版开心农场)谈架构:所有模块都是一个可降级的服务](http://timyang.net/architecture/farmville/ "March 8, 2010")
* [Twitter系统运维经验](http://timyang.net/tech/twitter-operations/ "November 2, 2009")

* 
### Most Commented

* [MemcacheDB, Tokyo Tyrant, Redis performance test](http://timyang.net/data/mcdb-tt-redis/ "MemcacheDB, Tokyo Tyrant, Redis performance test") (101)
* [C, Erlang, Java and Go Web Server performance test](http://timyang.net/programming/c-erlang-java-performance/ "C, Erlang, Java and Go Web Server performance test") (85)
* [About Tim Yang](http://timyang.net/about/ "About Tim Yang") (83)
* [Redis几个认识误区](http://timyang.net/data/redis-misunderstanding/ "Redis几个认识误区") (62)
* [Memcache mutex设计模式](http://timyang.net/programming/memcache-mutex/ "Memcache mutex设计模式") (47)
* [构建可扩展的微博架构(qcon beijing 2010演讲)](http://timyang.net/architecture/microblog-design-qcon-beijing/ "构建可扩展的微博架构(qcon beijing 2010演讲)") (39)
* [某分布式应用实践一致性哈希的一些问题](http://timyang.net/architecture/consistent-hashing-practice/ "某分布式应用实践一致性哈希的一些问题") (38)
* [为什么优秀开发者进入Google后就不参与开源了](http://timyang.net/google/open-source/ "为什么优秀开发者进入Google后就不参与开源了") (33)
* [MacBook Air与工作效率](http://timyang.net/misc/macbook-air-productive/ "MacBook Air与工作效率") (32)
* [Memcached数据被踢(evictions>0)现象分析](http://timyang.net/data/memcached-lru-evictions/ "Memcached数据被踢(evictions>0)现象分析") (30)

* 
### Recent Posts

* [演讲小组的第一次活动](http://timyang.net/tao/speech-practice/ "演讲小组的第一次活动")
* [“connect the dots” 随想](http://timyang.net/tao/connect-the-dots/ "“connect the dots” 随想")
* [跨领域人才](http://timyang.net/tao/talents/ "跨领域人才")
* [产品下线杂谈](http://timyang.net/product/discontinued-servic/ "产品下线杂谈")
* [当我谈演讲时候，我谈些什么](http://timyang.net/misc/speech/ "当我谈演讲时候，我谈些什么")
* [案例与故障的知识库](http://timyang.net/architecture/k/ "案例与故障的知识库")
* [团队中的为师之道](http://timyang.net/management/learning-tao/ "团队中的为师之道")
* [有感Google的混合研究方法](http://timyang.net/google/hybrid-research/ "有感Google的混合研究方法")
* [谈技术人员“转正”](http://timyang.net/management/probation/ "谈技术人员“转正”")
* [微信架构的启示](http://timyang.net/architecture/notes-weixin/ "微信架构的启示")
* [MacBook Air与工作效率](http://timyang.net/misc/macbook-air-productive/ "MacBook Air与工作效率")
* [Notes about Timelines @ Twitter](http://timyang.net/architecture/notes-timelines-twitter/ "Notes about Timelines @ Twitter")
* [技术工程师的能力与目标](http://timyang.net/management/engineer-performance/ "技术工程师的能力与目标")
* [技术方案评审](http://timyang.net/management/design-review/ "技术方案评审")
* [谈技术团队目标](http://timyang.net/management/planning/ "谈技术团队目标")

* 
### Recent Comments

* [Redis几个认识误区 | 创造](http://emake.info/the-redis-several-misunderstandings.html) on [Redis几个认识误区](http://timyang.net/data/redis-misunderstanding/comment-page-2/#comment-224409)
* [Redis几个认识误区 | 创造](http://emake.info/the-redis-several-misunderstandings.html) on [MemcacheDB, Tokyo Tyrant, Redis performance test](http://timyang.net/data/mcdb-tt-redis/comment-page-3/#comment-224408)
* [甄码农](http://outofmemory.cn/) on [团队中的为师之道](http://timyang.net/management/learning-tao/comment-page-1/#comment-224161)
* [甄码农](http://outofmemory.cn/) on [跨领域人才](http://timyang.net/tao/talents/comment-page-1/#comment-224158)
* 阿秀 on [About Tim Yang](http://timyang.net/about/comment-page-2/#comment-223584)

* 
### Categories

* [data](http://timyang.net/category/data/ "View all posts filed under data")
* [Erlang](http://timyang.net/category/erlang/ "Erlang语言")
* [Google](http://timyang.net/category/google/ "View all posts filed under Google")
* [Java](http://timyang.net/category/java/ "View all posts filed under Java")
* [Linux](http://timyang.net/category/linux/ "View all posts filed under Linux")
* [Lua](http://timyang.net/category/lua/ "View all posts filed under Lua")
* [Python](http://timyang.net/category/python/ "View all posts filed under Python")
* [SNS](http://timyang.net/category/sns/ "View all posts filed under SNS")
* [tech](http://timyang.net/category/tech/ "View all posts filed under tech")
* [Web](http://timyang.net/category/web/ "View all posts filed under Web")
* [产品](http://timyang.net/category/product/ "View all posts filed under 产品")
* [分布式](http://timyang.net/category/distributed/ "分布式算法及思想")
* [技术管理](http://timyang.net/category/management/ "View all posts filed under 技术管理")
* [架构](http://timyang.net/category/architecture/ "后端架构")
* [编程](http://timyang.net/category/programming/ "View all posts filed under 编程")
* [随想](http://timyang.net/category/tao/ "View all posts filed under 随想")
* [非技术](http://timyang.net/category/misc/ "非技术话题")

* 
### Archives

* [October 2012](http://timyang.net/2012/10/ "October 2012") (5)
* [September 2012](http://timyang.net/2012/09/ "September 2012") (4)
* [May 2012](http://timyang.net/2012/05/ "May 2012") (3)
* [February 2012](http://timyang.net/2012/02/ "February 2012") (3)
* [January 2012](http://timyang.net/2012/01/ "January 2012") (2)
* [December 2011](http://timyang.net/2011/12/ "December 2011") (1)
* [August 2011](http://timyang.net/2011/08/ "August 2011") (1)
* [July 2011](http://timyang.net/2011/07/ "July 2011") (1)
* [April 2011](http://timyang.net/2011/04/ "April 2011") (2)
* [February 2011](http://timyang.net/2011/02/ "February 2011") (1)
* [January 2011](http://timyang.net/2011/01/ "January 2011") (2)
* [December 2010](http://timyang.net/2010/12/ "December 2010") (2)
* [November 2010](http://timyang.net/2010/11/ "November 2010") (2)
* [September 2010](http://timyang.net/2010/09/ "September 2010") (1)
* [August 2010](http://timyang.net/2010/08/ "August 2010") (1)
* [July 2010](http://timyang.net/2010/07/ "July 2010") (3)
* [June 2010](http://timyang.net/2010/06/ "June 2010") (2)
* [May 2010](http://timyang.net/2010/05/ "May 2010") (2)
* [April 2010](http://timyang.net/2010/04/ "April 2010") (1)
* [March 2010](http://timyang.net/2010/03/ "March 2010") (4)
* [February 2010](http://timyang.net/2010/02/ "February 2010") (1)
* [January 2010](http://timyang.net/2010/01/ "January 2010") (1)
* [December 2009](http://timyang.net/2009/12/ "December 2009") (3)
* [November 2009](http://timyang.net/2009/11/ "November 2009") (3)
* [October 2009](http://timyang.net/2009/10/ "October 2009") (3)
* [September 2009](http://timyang.net/2009/09/ "September 2009") (4)
* [August 2009](http://timyang.net/2009/08/ "August 2009") (5)
* [July 2009](http://timyang.net/2009/07/ "July 2009") (6)
* [June 2009](http://timyang.net/2009/06/ "June 2009") (5)
* [May 2009](http://timyang.net/2009/05/ "May 2009") (11)
* [April 2009](http://timyang.net/2009/04/ "April 2009") (7)
* [March 2009](http://timyang.net/2009/03/ "March 2009") (3)
* [February 2009](http://timyang.net/2009/02/ "February 2009") (2)
* [January 2009](http://timyang.net/2009/01/ "January 2009") (2)
* [December 2008](http://timyang.net/2008/12/ "December 2008") (2)
* 
### Feeds

* [Entries (RSS)](http://timyang.net/feed/)
* [Comments (RSS)](http://timyang.net/comments/feed/)
*

## [Twitter架构图(cache篇)](http://timyang.net/architecture/twitter-cache-architecture/ "Permanent Link to Twitter架构图(cache篇)")

Wednesday, Oct 28th, 2009 by Tim | Tags: [twitter](http://timyang.net/tag/twitter/)

根据网上公开资料整理的Twitter架构，主要是cache方面，加了作者自己的补充，跟实际的架构未必完全一致。
![twitter cache]( "twitter cache")
一些数据：

* Cache分Page cache, fragment cache, row cache, vector Cache, cache命中率见图。
* Fragment cache存放了API各种请求格式的数据，包括XML, JSON, RSS, ATOM。
* 发表Tweets是先放入Kestrel, 再异步处理，Kestrel用的也是memcached协议。
* API requests: 550 r/s。
* POST tweets: 峰值：平时 80tweets/s, 奥巴马就任时达到 350tweets/s。
* Aggregator模块需要访问memcached multi get  数百个/s。
* Ruby on Rails前面还用了Varnish作前端反向代理。

参考资料：

* [QCon London 2009: Upgrading Twitter without service disruptions](http://gojko.net/2009/03/16/qcon-london-2009-upgrading-twitter-without-service-disruptions/)
* [Improving Running Components at Twitter](http://qconlondon.com/london-2009/file?path=/qcon-london-2009/slides/EvanWeaver_ImprovingRunningComponentsAtTwitter.pdf) (PDF slide)

### 9 Comments [»](http://timyang.net/architecture/twitter-cache-architecture/#postcomment "Jump to the comments form")

1. ![]()Lee Su Min says:

[Nov 1st 2009 at 17:15](http://timyang.net/architecture/twitter-cache-architecture/comment-page-1/#comment-2070)

Is a blue print same as a jia gou tu?
1. ![]()[王潇](http://hi.baidu.com/crazy_computer) says:

[Mar 8th 2010 at 18:10](http://timyang.net/architecture/twitter-cache-architecture/comment-page-1/#comment-3405)

我是一个大三的学生，现在在学习PHP！看了您的博客，学习一下，也就只是看看表面，现在对整个的WEB体系有了一定的了解了，但是我还是不太清晰！请您指点一下，我还不清楚现在所做的东西处于什么地方，对整个系统有什么影响？我开发网站时也都没用到您所做的东西啊，究竟何时用！还有就是究竟应该按照什么线路来学习技术，请您务必指教！！！
1. ![]()[遥方](http://blog.chinaunix.net/u2/84280) says:

[Jun 25th 2010 at 16:42](http://timyang.net/architecture/twitter-cache-architecture/comment-page-1/#comment-4367)

博主能力很强。向你学习
1. ![]()[小黑](http://www.wangdacuo.com/) says:

[Sep 11th 2010 at 22:25](http://timyang.net/architecture/twitter-cache-architecture/comment-page-1/#comment-4940)

请问像这样实时性强、数据量巨大的数据库应该做哪些优化和处理？
1. ![]()[Pan](http://apan.me/) says:

[May 21st 2011 at 12:50](http://timyang.net/architecture/twitter-cache-architecture/comment-page-1/#comment-29611)

Row Cache是否有些出入？图中Row Cache似乎是用来存储关系链的，而Evan Weaver在2009 London Qcon上的ppt中，显示Row Cache应该是用来缓存tweet正文内容的吧。http://qconlondon.com/dl/qcon-london-2009/slides/EvanWeaver_ImprovingRunningComponentsAtTwitter.pdf
1. ![]()[罗青](http://tsingroo.sinaapp.com/) says:

[Jan 17th 2012 at 12:53](http://timyang.net/architecture/twitter-cache-architecture/comment-page-1/#comment-124260)

我觉得除了缓存之外，数据库的表设计是最关键的部分。架构这些什么的都要依据数据库结构来设计。
1. ![]()段见尼宜 says:

[Nov 7th 2012 at 06:43](http://timyang.net/architecture/twitter-cache-architecture/comment-page-1/#comment-220186)

(网站打不开请点百度快照}手机号码任意显示|号码任意显示|去电号码任意显示|来电号码任意显示|号码随意显|电话号码任意显示|手机号码随意显|电话号码随意显|
手机号码任意显示 (可以免费测试一次)
鹏诚通讯科技有限公司
24小时客服QQ：13417527

24小时免费咨询电话：18268787444

公司网址：http://www.pcwltx.com/
朋友,想不想让你的手机很有个性? 明明你在深圳,而你跟朋友聊天的时候,你朋友的电话上显示你在广州(上 海/天津/武汉/兰州等)以至香港. 明明你在跟朋友玩,而你打电话给你家人,让家人以为你在外地出差. 明天你不是刘德华,而你总是你用刘德华的电话号在打电话 明明你现在在内地,而你非要在你朋友面前说你在香港. 想不想拥有了?

专业手机，座机，小灵通等打电话发短信可以任意显示电话号码的, 此产品不需要下载任何软件,只需要绑定您的号码即可实现显示.您要对方显示什么号码!我这里免费给您测试,让您亲眼所见！不想让别人知道自己在哪里的时候，被人追踪讨债不接您电话的时候很有用出的！您只需花上几百元钱就能买到您想要的任何靓号，吉祥号！以及港澳台和国际号码，任意显示您的电话号码，随时隐藏您的号码！
本业务不会影响你本手机的使用，想使用改号业务时就拨一下我们系统的预约号码，不想使用时就直接用你手机打出还是原来的号，不会改变你手机原来的实际号码，互不干扰.

为防止各种违法乱纪,本软件特定设置号码规范如下：
1、出于安全考虑国家特殊号码 110 120 119等号码，不可以随意设置，
一经发现删除该帐户！
2、不可以设置各种银行，移动的客服号码（95599 10086等）一经发现删除该帐户！

交易方式：支持各大银行和淘宝交易.

1， 无须在电话（手机，小灵通，座机）上下载任何软件
2， 无月租，无长途漫游费，全国号码任意设置，任意拨打，任意显示。0.2元/分钟
3， 绑定号码无次数限制，设置显示号码无次数限制。自行操作，自己做主，
4， 开通后没有时效限制，有话费即可使用。
5， 话费没了，续费继续使用。
6， 不影响你本机号码的话费，和正常使用，两者互不干扰。
7， 双方都为接电话型式，不扣通话双方的电话（手机，小灵通，座机）话费。
8，不想使用时就直接用你手机打出还是原来的号，不会改变你手机原来的实际号码，互不干扰.
显示号码自己设置，操作方法可以手机直接操作也可以网页上操作最后都是通过自己的手机打出，
让对方显示你要的号码，本机是接回拨所以免费,显示号码自由设置
可以让你亲自测试，自己打出一个任意显示的电话。
交易方式：支持各大银行和淘宝交易.

24小时客服QQ：13417527

24小时免费咨询电话：18268781444

公司网址：http://www.pcwltx.com/
骗子各种手法介绍：不允许测试，让你直接汇款，汇款后开通。

[RSS feed for comments on this post](http://timyang.net/architecture/twitter-cache-architecture/feed/), [TrackBack URI](http://timyang.net/architecture/twitter-cache-architecture/trackback/)
[Cancel Reply](http://timyang.net/architecture/twitter-cache-architecture/#respond)

### Leave a Comment

Name (required)

E-mail (required, never displayed)

URI

Except where otherwise noted, content on this site is licensed under a [Creative Commons Attribution 3.0](http://creativecommons.org/licenses/by/3.0/) License
