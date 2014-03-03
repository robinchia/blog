---
layout: post
title: "代理服务器Squid 使用详解 - 51CTO.COM"
categories: 服务器
tags: 
 - 服务器
 - Squid
--- 

# 代理服务器Squid 使用详解 - 51CTO.COM

### 分享到

* [一键分享](http://network.51cto.com/art/200601/18507.htm#)
* [QQ空间](http://network.51cto.com/art/200601/18507.htm#)
* [新浪微博](http://network.51cto.com/art/200601/18507.htm#)
* [百度搜藏](http://network.51cto.com/art/200601/18507.htm#)
* [人人网](http://network.51cto.com/art/200601/18507.htm#)
* [腾讯微博](http://network.51cto.com/art/200601/18507.htm#)
* [百度相册](http://network.51cto.com/art/200601/18507.htm#)
* [开心网](http://network.51cto.com/art/200601/18507.htm#)
* [腾讯朋友](http://network.51cto.com/art/200601/18507.htm#)
* [百度贴吧](http://network.51cto.com/art/200601/18507.htm#)
* [豆瓣网](http://network.51cto.com/art/200601/18507.htm#)
* [搜狐微博](http://network.51cto.com/art/200601/18507.htm#)
* [百度新首页](http://network.51cto.com/art/200601/18507.htm#)
* [QQ好友](http://network.51cto.com/art/200601/18507.htm#)
* [和讯微博](http://network.51cto.com/art/200601/18507.htm#)
* [更多...](http://network.51cto.com/art/200601/18507.htm#)

[百度分享](http://network.51cto.com/art/200601/18507.htm#)

[首页](http://www.51cto.com/)![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/top_bg_xian.gif)[技术频道![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/nav_ico1.gif)](http://www.51cto.com/col/35/)![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/top_bg_xian.gif)[51CTO旗下网站![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/nav_ico1.gif)](http://www.51cto.com/)![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/top_bg_xian.gif)[地图](http://www.51cto.com/about/map.htm) [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/143749870.gif)](http://www.51cto.com/about/rss.html)

![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/top_shequ.gif)社区：[论坛](http://bbs.51cto.com/)[博客](http://blog.51cto.com/)[下载](http://down.51cto.com/)[读书](http://book.51cto.com/)[更多![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/nav_ico1.gif)]()

[![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/dia.gif "今天领无忧币了吗？")](http://home.51cto.com/index.php?s=/Home/index)[登录](http://home.51cto.com/?reback=http%253A%252F%252Fnetwork.51cto.com%252Fart%252F200601%252F18507.htm)[注册](http://ucenter.51cto.com/reg_01.php?reback=http%253A%252F%252Fnetwork.51cto.com%252Fart%252F200601%252F18507.htm)
* [网络](http://network.51cto.com/)
* [安全](http://netsecurity.51cto.com/)
* [开发](http://developer.51cto.com/)
* [数据库](http://database.51cto.com/)
* [服务器](http://server.51cto.com/)
* [系统](http://os.51cto.com/)
* [虚拟化](http://virtual.51cto.com/)
* [云计算](http://cloud.51cto.com/)
* [嵌入式](http://developer.51cto.com/embed/)
* [移动开发](http://mobile.51cto.com/)

* [51CTO.COM](http://www.51cto.com/ "中国领先的IT技术网站")
* [CIOage.com](http://www.cioage.com/ "中国首个专门服务于CIO的专业网站")
* [WatchStor.com](http://www.watchstor.com/ "领先的中文存储网络媒体")
* [HC3i.cn](http://www.hc3i.cn/ "中国首家专注于数字医疗及医疗信息化的专业网站")
* [灵客风LinkPhone](http://www.linkphone.cn/ "移动互联网生活门户")
* [家园](http://home.51cto.com/)
* [微博](http://t.51cto.com/)
* [博客](http://blog.51cto.com/)
* [论坛](http://bbs.51cto.com/)
* [下载](http://down.51cto.com/)
* [自测](http://selftest.51cto.com/)
* [门诊](http://doctor.51cto.com/)
* [周刊](http://blog.51cto.com/newsletter/)
* [读书](http://book.51cto.com/)
* [技术圈](http://g.51cto.com/)
* [知道](http://zhidao.51cto.com/)

*
*
*
*
![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/zuwang_logo.gif)

[首页](http://network.51cto.com/) | [思科](http://network.51cto.com/col/786/) | [路由](http://network.51cto.com/router/) | [交换](http://network.51cto.com/switch) | [无线](http://network.51cto.com/wlan) | [ADSL](http://network.51cto.com/col/888/) | [VPN](http://network.51cto.com/col/946/) | [网管](http://network.51cto.com/col/1022/) | [布线](http://network.51cto.com/col/493/) | [接入](http://network.51cto.com/col/513/) | [全部文章](http://publish.51cto.com/list/481/)
[](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/search.gif)

您所在的位置：[网络频道](http://network.51cto.com/) > **代理服务器Squid 使用详解**

# 代理服务器Squid 使用详解

2006-01-17 13:55 nsfocus [我要评论(0)](http://network.51cto.com/art/200601/18507.htm#commment) 字号：[T]() | [T]()

[![一键收藏，随时查看，分享好友！](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/Fav.gif)]( "一键收藏，随时查看，分享好友！")

做为眼下最流行的操作系统，Linux已经越来越受到世人的关注。虽然目前Linux的软件还不是很丰富，替代WINDOWS作为普通PC机操作系统还为时过早，但是在服务器领域，Linux的稳定性，可操作性决不输于任何操作系统，并且也有优秀的软件支持

AD： [2013大数据全球技术峰会低价抢票中](http://wot.51cto.com/bigdata2013/buyticket.html)

做为眼下最流行的操作系统，Linux已经越来越受到世人的关注。虽然目前Linux的软件还不是很丰富，替代WINDOWS作为普通PC机操作系统还为时过早，但是在服务器领域，Linux的稳定性，可操作性决不输于任何操作系统，并且也有优秀的软件支持 。Squid就是其中之一。Linux加Squid的组合做为代理服务器，性能远远超过WINNT加MSPROXY2.0（个人观点），为几百人的小型局域网代理绰绰有余。下面，我就详细的介绍Squid的安装及使用技巧，希望大家能够喜欢上它。
1.Squid简介

Squid是一个缓存internet数据的一个软件，它接收用户的下载申请，并自动处理所下载的数据。也就是说，当一个用户象要下载一个主页时，它向Squid发出一个申请，要Squid替它下载，然后Squid连接所申请网站并请求该主页，接着把该主页传给用户同时保留一个备份，当别的用户申请同样的页面时，Squid把保存的备份立即传给用户，使用户觉得速度相当快。目前，Squid 可以代理HTTP, FTP, GOPHER, SSL 和 WAIS 协议，暂不能代理POP, NNTP等协议。不过，已经有人开始修改Squid，相信不久的将来，Squid能够代理这些协议。

Squid能够缓存任何数据吗？不是的。象缓存信用卡帐号、可以远方执行的scripts、经常变换的主页等是不合适的也是不安全的。Squid可以自动的进行处理，你也可以根据自己的需要设置Squid，使之过滤掉你不想要的东西。
Squid可以工作在很多的操作系统中，如AIX, Digital Unix, FreeBSD, HP-UX, Irix, Linux, NetBSD, Nextstep, SCO, Solaris，OS/2等，也有不少人在其他操作系统中重新编译过Squid。
Squid对硬件的要求是内存一定要大，不应小于128M，硬盘转速越快越好，最好使用服务器专用SCSI硬盘，处理器要求不高，400MH以上既可。
2. Squid的编译和运行
其实现在的Linux发行套件中基本都有已经编译好的Squid，你所作的就是安装它既可。如果你手头没有现成的编译好的Squid或想使用最新的版本，去ftp:squid.nlanr.net下载一份，自己编译。
Squid的编译是非常简单的，因为它基本上是自己配置自己。最容易出现的问题是你的系统上没有合适的编译器，这可以通过安装相应的编译器解决。如果出现其他问题，你可以问一下有经验的用户或到相应的邮件列表寻找帮助。
编译Squid之前，最好建一个专门运行Squid的用户和组。我就在自己的服务器上建了一个名为squid的用户和组，用户目录设为/usr/local/squid。然后su为用户squid并从squid.nlanr.net下载Squid的源文件到目录 /usr/local/squid/src中，用如下命令进行解压：
％tar xzf squid-2.0.RELEASE-src.tar.gz
％cd /usr/local/squid/src/ squid-*.*.RELEASE /
％./configure
%make
％make install

第一个命令在目录/usr/local/squid/src中产生一个新的子目录/squid-*.*.RELEASE/。命令./configure会自动查询你的系统配置情况以及你系统中使用的头文件。不加参数的./configure会把Squid安装在目录/usr/local/squid中，如果你想使用其他目录，用如下命令./configure --prefix=/some/other/directory，这会把Squid安装在目录/some/other/directory中。make命令编译Squid，make install命令安装Squid。
不出意外的话，目录/usr/local/squid中会出现如下目录：

/bin
/cache
/etc
/logs/
/src （自己创建的）

目录/bin中含有Squid可执行程序，包括Squid本身，ftpget等。
目录/cache包含Squid缓存的数据，其中包含象/00/ /01/ /02/ 以及/03/这样的目录，这些目录中还有子目录，因为目录多了比在一个目录成千上万的文件中寻找一个文件更容易，速度更快。
目录/etc中包含Squid的唯一的配置文件squid.conf。
目录/logs中包含Squid的日志。
3. squid.conf文件的配置
在安装Squid后，在目录/usr/local/squid /etc中会自动产生一个样本squid.conf文件，文件中对每一个选项都有详细的说明，用户可以通过修改该文件以满足不同的需要。
总的来说，有如下几个重要选项：
http_port:设定Squid监听的端口，你最好设一个比较好记的端口号，以便在进行客户机配置时容易记住。我的机器上端口号设的是8080。缺省为3128。
cache_mem：设定Squid占用的物理内存，根据我的经验，cache_mem的大小不应超过你的服务器物理内存的三分之一，否则将会影响机器的总体性能。
maximum_object_size：设定Squid可以接收的最大对象的大小。Squid缺省值为4M，我自己入认为太大，你可以根据自己的需要进行设定。
cache_dir：设定缓存的位置、大小。一般看起来形式如下“cache_dir /usr/local/squid/cache 100 16 256”。 /usr/local/squid/cache代表缓存的位置；100代表缓存最大为100M；16和256代表一级和二级目录数。
cache_effective_user：设定使用缓存的有效用户。缺省为用户nobody，如果你的系统中没有用户nobody，最好建一个或以非root用户运行Squid。
下面我给出一个最简单的squid.conf文件：

#squid.conf - a very basic config file for squid
#Turn logging to it's lowest level
debug_options ALL,1
#defines a group (or Access Control List)
that includes all IP addresses
acl all src 0.0.0.0/0.0.0.0
#define RAM used
cache_mem 32M
#defines the cache size
cache_dir /usr/local/squid/cache 100 16 256
#allow all sites to use connect to us via HTTP
http_access allow all
#allow all sites to use us as a sibling
icp_access allow all
#test the following sites to check that we are connected
dns_testnames internic.net usc.edu
cs.colorado.edu mit.edu yale.edu
#run as the squid user
cache_effective_user squid squid

这个配置文件允许所有人使用Squid，创建了100M缓存，使用32M内存，在缺省位置"/usr/local/squid/cache"缓存数据，所有缓存数据以组squid和用户squid身份保存，端口为3128。虽然这个配置很不安全，但是它已经能使用了。
4. 运行Squid
首先以root身份登陆。运行如下命令：
％/usr/local/squid/bin/squid ？z
该命令会产生Squid所有的缓存目录。
如果你想前台执行Squid,接着执行命令：
％/usr/local/squid/bin/squid -NCd1
该命令正式启动Squid。如果一切正常，你会看到一行输出
Ready to serve requests.
如果想后台运行Squid，把它做为一个精灵进程，执行命令：
％/usr/local/squid/bin/squid
观察Squid是否运行使用命令：
% squid -k check
输出会告诉你Squid的当前状态。
好了，文章先写到这里，其实这里介绍的都是最基本的东西，Squid有好多高级的功能，如做WEB服务器的高速缓存，做二级代理服务器，做为防火墙，以及怎样设定过滤规则等，这里就不详述了，如果有机会再奉献给大家。(责任编辑：[liucl](mailto:chenliang.liu@gmail.com))

* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/bq1.jpg)]()

[给力]()
(47票)
* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/bq2.jpg)]()

[动心]()
(42票)
* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/bq3.jpg)]()

[废话]()
(49票)
* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/bq4.jpg)]()

[专业]()
(42票)
* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/bq5.jpg)]()

[标题党]()
(40票)
* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/bq6.jpg)]()

[路过]()
(42票)

原文：[**代理服务器Squid 使用详解**]() [返回网络频道首页](http://network.51cto.com/)

分享到：

[](http://network.51cto.com/art/200601/18507.htm# "分享到QQ空间") [](http://network.51cto.com/art/200601/18507.htm# "分享到新浪微博") [](http://network.51cto.com/art/200601/18507.htm# "分享到腾讯微博") [](http://network.51cto.com/art/200601/18507.htm# "分享到人人网") [](http://network.51cto.com/art/200601/18507.htm# "分享到网易微博")  [1](http://network.51cto.com/art/200601/18507.htm# "累计分享1次")

[收藏]()|[打印]()|[复制]( "分享本资源给好友")

关于[代理服务器Squid](http://www.51cto.com/php/search.php?keyword=%B4%FA%C0%ED%B7%FE%CE%F1%C6%F7Squid)的更多文章

[**谁来统领企业无线网络-最新802.11n无线路由综合评测**](http://network.51cto.com/art/200907/136542.htm "谁来统领企业无线网络-最新802.11n无线路由综合评测") [![谁来统领企业无线网络-最新802.11n无线路由综合评测](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/133548583.jpg)](http://network.51cto.com/art/200907/136542.htm "谁来统领企业无线网络-最新802.11n无线路由综合评测")

从802.11n诞生以来，一直就没有正式案发布，现在市面上的802.11n[[详细]](http://network.51cto.com/art/200907/136542.htm "谁来统领企业无线网络-最新802.11n无线路由综合评测")

### 网友评论*TOP5*

[查看所有评论（0）](http://www.51cto.com/php/feedbackt.php?id=18507)

[]() 提交评论 通行证： 密码：   [注册通行证](http://ucenter.51cto.com/reg_01.php)
验证码：![]()点击图片可刷新验证码请点击后输入验证码匿名发表

### 栏目热门

[更多>>](http://network.51cto.com/click/481)

* [IT宴席开局 网友热议海底捞远程视频聚餐](http://network.51cto.com/art/201202/314707.htm "IT宴席开局 网友热议海底捞远程视频聚餐")
* [12306订票网烧钱数千万 太极、网宿参建龟速](http://network.51cto.com/art/201201/311658.htm "12306订票网烧钱数千万 太极、网宿参建龟速网")
* [2012春节长假IT系统运维支招之管理篇](http://network.51cto.com/art/201201/312810.htm "2012春节长假IT系统运维支招之管理篇")
* [千兆Wi-Fi——高速无线不是梦](http://network.51cto.com/art/201202/317743.htm "千兆Wi-Fi——高速无线不是梦")
* [CISCO路由器启动流程图](http://network.51cto.com/art/201201/311217.htm "CISCO路由器启动流程图")

### 同期最新

[更多>>](http://network.51cto.com/col/481)

* [万物求源 原理解析网络代理](http://network.51cto.com/art/200601/18504.htm "万物求源 原理解析网络代理")
* [多重功能 且看网络代理](http://network.51cto.com/art/200601/18494.htm "多重功能 且看网络代理")
* [畅游其间 网络代理](http://network.51cto.com/art/200601/18490.htm "畅游其间 网络代理")
* [代理服务器软件SuperProxy](http://network.51cto.com/art/200601/18486.htm "代理服务器软件SuperProxy")
* [局域网资源共享故障分析与解决](http://network.51cto.com/art/200601/18249.htm "局域网资源共享故障分析与解决")

### [网络频道](http://network.51cto.com/)

频道导航

* 热门

[路由技术](http://network.51cto.com/router/)|[交换网络](http://network.51cto.com/switch/)|[无线网络](http://network.51cto.com/wlan/)
* 技术

[布线技术](http://network.51cto.com/cabling/)|[运维技术](http://network.51cto.com/col/1022/)|[接入技术](http://network.51cto.com/access/)
* 专题

[专题汇总](http://network.51cto.com/speclist/481/)|[网管软件](http://network.51cto.com/art/201101/242851.htm)|[网络控速](http://network.51cto.com/art/201011/235584.htm)

### 热点推荐

* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/153546162.jpg)](http://mobile.51cto.com/mobile-182392.htm)

[Android开发应用详解](http://mobile.51cto.com/mobile-182392.htm)
* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/133620622.jpg)](http://developer.51cto.com/art/201106/266369.htm)

[那些性感的让人尖叫的程序员](http://developer.51cto.com/art/201106/266369.htm)
* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/134217922.jpg)](http://developer.51cto.com/art/200907/133407.htm)

[HTML5 下一代Web开发标准详解](http://developer.51cto.com/art/200907/133407.htm)
* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/123123954.jpg)](http://developer.51cto.com/art/201104/257581.htm)

[高性能WEB开发应用指南](http://developer.51cto.com/art/201104/257581.htm)
* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/195816929.jpg)](http://os.51cto.com/art/200706/49181.htm)

[Ubuntu开源技术交流频道](http://os.51cto.com/art/200706/49181.htm)

**热门标签：** [windows频道](http://os.51cto.com/windows/)[移动开发](http://mobile.51cto.com/)[云计算](http://cloud.51cto.com/)[objective-c](http://mobile.51cto.com/mobile/objc/)[tp-link路由器设置图解](http://network.51cto.com/art/200909/151122.htm)[html5](http://developer.51cto.com/art/200907/133407.htm)

[![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/wKioJlEjPKXh0JE0AABEhq3s9JY868.jpg)](http://wot.51cto.com/bigdata2013/buyticket.html)

* *专题* [思科网联天下 共启智慧未来](http://network.51cto.com/network/cisco/201212)
* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/wKioOVFBO1ryB-WmAAAfx2V-oPA593.jpg)](http://network.51cto.com/network/cisco/201212) 思科联所未连，与您共启未来！一起来探索万物互联...

1. [2013 RSA大会：掌控数据 保护世界](http://netsecurity.51cto.com/secu/RSA2013/)
1. [思科UCS助力医疗数据库部署优化](http://network.51cto.com/art/201301/378017.htm)

### 文章排行

[本月]()[本周]()[24小时]()

* [宽带路由器设置图解七步骤](http://network.51cto.com/art/200912/171496.htm "宽带路由器设置图解七步骤")
* [详解TP-Link路由器设置（图解）](http://network.51cto.com/art/200909/151122.htm "详解TP-Link路由器设置（图解）")
* [192.168.1.1打不开或进不去怎么办？](http://network.51cto.com/art/201108/287705.htm "192.168.1.1打不开或进不去怎么办？")
* [宽带路由器设置：192.168.0.1](http://network.51cto.com/art/201109/290308.htm "宽带路由器设置：192.168.0.1")
* [为什么路由器设置地址192.168.1.1打不](http://network.51cto.com/art/201109/289287.htm "为什么路由器设置地址192.168.1.1打不")
* [tenda无线路由器设置图解](http://network.51cto.com/art/201109/290947.htm "tenda无线路由器设置图解")
* [无线路由器的桥接和覆盖图文教程](http://network.51cto.com/art/200806/78880.htm "无线路由器的桥接和覆盖图文教程")
* [TP-Link无线路由器设置和密码破解](http://network.51cto.com/art/201109/291036.htm "TP-Link无线路由器设置和密码破解")
* [无线“蹭网卡”热卖 任意密码5分钟破解](http://network.51cto.com/art/200908/144482.htm "无线“蹭网卡”热卖 任意密码5分钟破解")
* [教你找回无线路由器密码的方法](http://network.51cto.com/art/201109/289392.htm "教你找回无线路由器密码的方法")
* [F5 Networks:让应用和网络完美融合 轻](http://network.51cto.com/art/201304/389875.htm "F5 Networks:让应用和网络完美融合 轻")
* [路由器频繁死机是怎么回事?](http://network.51cto.com/art/201304/390081.htm "路由器频繁死机是怎么回事?")
* [数据中心交换机何时才能有中国“芯”](http://network.51cto.com/art/201304/390055.htm "数据中心交换机何时才能有中国“芯”")
* [阿朗SDN战略：聚焦网络与应用融合 注重](http://network.51cto.com/art/201304/390097.htm "阿朗SDN战略：聚焦网络与应用融合 注重")
* [极速WIFI时代 华硕PCE-AC66首款千兆网](http://network.51cto.com/art/201304/390183.htm "极速WIFI时代 华硕PCE-AC66首款千兆网")
* [改进WAN安全架构的两种选择](http://network.51cto.com/art/201304/390060.htm "改进WAN安全架构的两种选择")
* [突破传统网络格局 革新思想加速推进SDN](http://network.51cto.com/art/201304/390414.htm "突破传统网络格局 革新思想加速推进SDN")
* [连上交换机后部分电脑无法上网?](http://network.51cto.com/art/201304/390067.htm "连上交换机后部分电脑无法上网?")
* [三网融合缓慢 部门利益争夺仍是根本原](http://network.51cto.com/art/201304/389911.htm "三网融合缓慢 部门利益争夺仍是根本原")
* [VIP企业专线 MSTP刚性管道凸显大神通](http://network.51cto.com/art/201304/390009.htm "VIP企业专线 MSTP刚性管道凸显大神通")

* [了解光纤上网的实质是什么](http://network.51cto.com/art/201304/387782.htm "了解光纤上网的实质是什么")
* [面粉比面包贵频频发生 宽带中国需要真](http://network.51cto.com/art/201304/387581.htm "面粉比面包贵频频发生 宽带中国需要真")
* [虚拟化该成为网络面向应用的第一步](http://network.51cto.com/art/201304/387608.htm "虚拟化该成为网络面向应用的第一步")
* [网吧路由器qos设置的操作步骤](http://network.51cto.com/art/201304/387365.htm "网吧路由器qos设置的操作步骤")
* [迎合网络时代脚步 探索SDN全新发展机遇](http://network.51cto.com/art/201303/386815.htm "迎合网络时代脚步 探索SDN全新发展机遇")
* [路由器qos设置包括哪些内容](http://network.51cto.com/art/201304/387370.htm "路由器qos设置包括哪些内容")
* [解析中国移动互联网未来的发展趋势](http://network.51cto.com/art/201304/387494.htm "解析中国移动互联网未来的发展趋势")
* [F5 Networks:让应用和网络完美融合 轻](http://network.51cto.com/art/201304/389875.htm "F5 Networks:让应用和网络完美融合 轻")
* [三种应用性能监控工具对比](http://network.51cto.com/art/201303/386554.htm "三种应用性能监控工具对比")
* [有关三层交换机的四个特性](http://network.51cto.com/art/201303/386767.htm "有关三层交换机的四个特性")

### 热点专题

[更多>>](http://network.51cto.com/speclist/481)

* [![网管软件聚宝盆](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/140824724.jpg)](http://network.51cto.com/art/201101/242851.htm "网管软件聚宝盆")

### [网管软件聚宝盆](http://network.51cto.com/art/201101/242851.htm "网管软件聚宝盆")

网络管理的定义非常广泛，随着互联网的不断发展，对网
* [![给力呀！负载均衡](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/180328253.jpg)](http://network.51cto.com/art/201101/241997.htm "给力呀！负载均衡")

### [给力呀！负载均衡](http://network.51cto.com/art/201101/241997.htm "给力呀！负载均衡")

互联网的发展快得无法言喻，随之而来的各种应用更是层
* [![2010年中国十大布线品牌](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/160830764.jpg)](http://network.51cto.com/art/201012/237093.htm "2010年中国十大布线品牌")

### [2010年中国十大布线品](http://network.51cto.com/art/201012/237093.htm "2010年中国十大布线品牌")

中国工程建设标准化协会信息通信专业委员会综合布线工

### 热点标签

[局域网网速](http://network.51cto.com/art/201011/235584.htm "局域网网速") [IPv6](http://network.51cto.com/art/200703/41485.htm "IPv6") [负载均衡](http://network.51cto.com/art/201101/241997.htm "负载均衡") [思科](http://network.51cto.com/cisco/uc/ "思科") [交换机典型配置](http://network.51cto.com/art/200508/1688.htm "交换机典型配置") [VPN技术](http://network.51cto.com/art/200511/11865.htm "VPN技术") [路由器设置](http://network.51cto.com/art/200512/13240.htm "路由器设置") [家庭无线局域网](http://network.51cto.com/art/200512/13840.htm "家庭无线局域网") [路由故障](http://network.51cto.com/art/200511/12105.htm "路由故障") [网管软件](http://network.51cto.com/art/201101/242851.htm "网管软件") [广域网优化](http://network.51cto.com/network/sangfor/optimization/ "广域网优化") [视频会议](http://network.51cto.com/network/adslr/facemeeting/ "视频会议") [MOVE无线](http://network.51cto.com/network/move/ "MOVE无线") [十大布线品牌](http://network.51cto.com/network/cteam/ccb2010/ "十大布线品牌") [tenda无线路由器设置](http://network.51cto.com/art/200912/169486.htm "十大布线品牌")

* [**点击这里查看样刊**](http://news.51cto.com/col/1323/)
* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/dingy.gif)](http://home.51cto.com/index.php?s=/Subscribe)
### 全站热点

* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/133258109.png)](http://os.51cto.com/art/201201/312873.htm "《Linux运维趋势》第16期")

[《Linux运维趋势》第16期](http://os.51cto.com/art/201201/312873.htm "《Linux运维趋势》第16期")
* [![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/101803336.jpg)](http://developer.51cto.com/art/201202/314276.htm "Java路线图：甲骨文的两年计划")

[Java路线图：甲骨文的两年计划](http://developer.51cto.com/art/201202/314276.htm "Java路线图：甲骨文的两年计划")

* [VMware不止虚拟化 云计算全方案齐亮](http://virtual.51cto.com/art/201111/300033.htm "VMware不止虚拟化 云计算全方案齐亮相")
* [Citrix Synergy 2011独家报道：协作](http://server.51cto.com/News-300011.htm "Citrix Synergy 2011独家报道：协作、云、移动")
* [SNS营销--网商成功之道](http://book.51cto.com/art/201111/300006.htm "SNS营销--网商成功之道")
* [海底捞你学不会](http://book.51cto.com/art/201110/299915.htm "海底捞你学不会")
* [编程匠艺--编写卓越的代码](http://book.51cto.com/art/201110/299819.htm "编程匠艺--编写卓越的代码")

### 读书

[![](http://./代理服务器Squid 使用详解 - 51CTO.COM_files/174609703.gif)](http://book.51cto.com/art/200801/63496.htm "软件架构设计")

### [软件架构设计](http://book.51cto.com/art/200801/63496.htm)

本书紧紧围绕“软件架构设计”这一主题，立足实践解析了软件架构的概念，阐述了切实可行的软件架构设计方法，提供了可操作性极强

* [鸟哥的Linux私房菜 基础学习篇（第二版）](http://book.51cto.com/art/200709/57047.htm "鸟哥的Linux私房菜 基础学习篇（第二版）")
* [Groovy入门经典](http://book.51cto.com/art/200711/57494.htm "Groovy入门经典")
* [PHP程序开发范例宝典](http://book.51cto.com/art/200710/57701.htm "PHP程序开发范例宝典")
* [SQL Server 2005中文版精粹](http://book.51cto.com/art/200710/57877.htm "SQL Server 2005中文版精粹")

### 博文推荐

[更多>>](http://blog.51cto.com/)

* [IT服务中的人员外包与管理服务外包](http://tkato.blog.51cto.com/1119634/262700/ "IT服务中的人员外包与管理服务外包")
* [知识改变命运？](http://windwing75.blog.51cto.com/191437/262693/ "知识改变命运？")
* [别乎略安身立命的基础本领](http://miracle.blog.51cto.com/255044/262656/ "别乎略安身立命的基础本领")
* [[职业发展]中国本科生的6大软肋，彰](http://davyyew.blog.51cto.com/1084625/262648/ "[职业发展]中国本科生的6大软肋，彰显就业意识不强")

### 最新热帖

[更多>>](http://bbs.51cto.com/hotthreads.php)

* [大量的H3C方案Visio图标1](http://bbs.51cto.com/thread-471418-1.html "大量的H3C方案Visio图标1")
* [大量的H3C方案Visio图标1](http://bbs.51cto.com/thread-471418-1.html "大量的H3C方案Visio图标1")
* [大量的H3C方案Visio图标1](http://bbs.51cto.com/thread-471418-1.html "大量的H3C方案Visio图标1")
* [BE IT规划资料](http://bbs.51cto.com/thread-426679-1.html "BE IT规划资料")
* [联想集团内部管理系统（非常好的实用](http://bbs.51cto.com/thread-422145-1.html "联想集团内部管理系统（非常好的实用文档）")
### 51CTO旗下网站

[领先的IT技术网站 51CTO](http://www.51cto.com/) [领先的中文存储媒体 WatchStor](http://www.watchstor.com/) [中国首个CIO网站 CIOage](http://www.cioage.com/) [中国首家数字医疗网站 HC3i](http://www.hc3i.cn/) [移动互联网生活门户 灵客风LinkPhone](http://www.linkphone.cn/)

Copyright©2005-2013 [51CTO.COM](http://www.51cto.com/) 版权所有 未经许可 请勿转载
0
