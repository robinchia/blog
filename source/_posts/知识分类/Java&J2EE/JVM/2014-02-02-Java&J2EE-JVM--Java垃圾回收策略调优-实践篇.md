---
layout: post
title: "Java 垃圾回收策略调优-实践篇"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# Java 垃圾回收策略调优-实践篇

[登录](http://www.douban.com/accounts/login?source=group) [注册](http://www.douban.com/accounts/register?source=group)

* [豆瓣](http://www.douban.com/)
* [读书](http://book.douban.com/)
* [电影](http://movie.douban.com/)
* [音乐](http://music.douban.com/)
* [同城](http://www.douban.com/location/)
* [小组](http://www.douban.com/group/)
* [阅读](http://read.douban.com/?dcs=top-nav&dcm=douban)
* [豆瓣FM](http://douban.fm/)
* [更多]()

[九点](http://9.douban.com/) [阿尔法城](http://alphatown.com/) [移动应用](http://www.douban.com/mobile/)

[豆瓣小组](http://www.douban.com/group/)

* [发现小组](http://www.douban.com/group/explore)
* [发现话题](http://www.douban.com/group/explore_topic)
搜索：  小组、话题
# Java 垃圾回收策略调优，实践篇

[![KK]()](http://www.douban.com/people/1851090/)

### 来自: [KK](http://www.douban.com/people/1851090/) 2008-10-22 13:26:30

JVM参数调优是一个很头痛的问题，可能和应用有关系，下面是本人一些调优的实践经验，希望对读者能有帮助，环境LinuxAS4,resin2.1.17,JDK6.0,2CPU,4G内存,dell2950服务器，网站是shedewang.com，新手可能觉得这文章没有用。
一：串行垃圾回收，也就是默认配置，完成10万request用时153秒，JVM参数配置如下
$JAVA_ARGS .= " -Dresin.home=$SERVER_ROOT -server -Xms2048M -Xmx2048M -Xmn512M -XX:PermSize=256M -XX:MaxPermSize=256M -XX:MaxTenuringThreshold=7 -XX:GCTimeRatio=19 -Xnoclassgc -Xloggc:log/gc.log -XX:+PrintGCDetails -XX:+PrintGCTimeStamps ";
这种配置一般在resin启动24小时内似乎没有大问题，网站可以正常访问，但查看日志发现，在接近24小时时，Full GC执行越来越频繁，大约每隔3分钟就有一次Full GC，每次Full GC系统会停顿6秒左右，作为一个网站来说，用户等待6秒恐怕太长了，所以这种方式有待改善。MaxTenuringThreshold=7表示一个对象如果在救助空间移动7次还没有被回收就放入年老代，GCTimeRatio=19表示java可以用5%的时间来做垃圾回收，1/(1+19)=1 /20=5%。
二：并行回收，完成10万request用时117秒，配置如下：
$JAVA_ARGS .= " -Dresin.home=$SERVER_ROOT -server -Xmx2048M -Xms2048M -Xmn512M -XX:PermSize=256M -XX:MaxPermSize=256M -Xnoclassgc -Xloggc:log/gc.log -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+UseParallelGC -XX:ParallelGCThreads=20 -XX:+UseParallelOldGC -XX:MaxGCPauseMillis=500 -XX:+UseAdaptiveSizePolicy -XX:MaxTenuringThreshold=7 -XX:GCTimeRatio=19 ";
并行回收我尝试过多种组合配置，似乎都没什么用，resin启动3小时左右就会停顿，时间超过10 秒。也有可能是参数设置不够好的原因，MaxGCPauseMillis表示GC最大停顿时间，在resin刚启动还没有执行Full GC时系统是正常的，但一旦执行Full GC，MaxGCPauseMillis根本没有用，停顿时间可能超过20秒，之后会发生什么我也不再关心了，赶紧重启resin，尝试其他回收策略。
三：并发回收，完成10万request用时60秒，比并行回收差不多快一倍，是默认回收策略性能的2.5倍，配置如下：
$JAVA_ARGS .= " -Dresin.home=$SERVER_ROOT -server -Xms2048M -Xmx2048M -Xmn512M -XX:PermSize=256M -XX:MaxPermSize=256M -XX:+UseConcMarkSweepGC -XX:MaxTenuringThreshold=7 -XX:GCTimeRatio=19 -Xnoclassgc -Xloggc:log/gc.log -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+UseCMSCompactAtFullCollection -XX:CMSFullGCsBeforeCompaction=0 ";
这个配置虽然不会出现10秒连不上的情况，但系统重启3个小时左右，每隔几分钟就会有5秒连不上的情况，查看gc.log，发现在执行ParNewGC时有个promotion failed错误，从而转向执行Full GC，造成系统停顿，而且会很频繁，每隔几分钟就有一次，所以还得改善。UseCMSCompactAtFullCollection是表是执行Full GC后对内存进行整理压缩，免得产生内存碎片，CMSFullGCsBeforeCompaction=N表示执行N次Full GC后执行内存压缩。
四：增量回收，完成10万request用时171秒，太慢了，配置如下
$JAVA_ARGS .= " -Dresin.home=$SERVER_ROOT -server -Xms2048M -Xmx2048M -Xmn512M -XX:PermSize=256M -XX:MaxPermSize=256M -XX:MaxTenuringThreshold=7 -XX:GCTimeRatio=19 -Xnoclassgc -Xloggc:log/gc.log -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -Xincgc ";
似乎回收得也不太干净，而且也对性能有较大影响，不值得试。
五：并发回收的I-CMS模式，和增量回收差不多，完成10万request用时170秒。
$JAVA_ARGS .= " -Dresin.home=$SERVER_ROOT -server -Xms2048M -Xmx2048M -Xmn512M -XX:PermSize=256M -XX:MaxPermSize=256M -XX:MaxTenuringThreshold=7 -XX:GCTimeRatio=19 -Xnoclassgc -Xloggc:log/gc.log -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+UseConcMarkSweepGC -XX:+CMSIncrementalMode -XX:+CMSIncrementalPacing -XX:CMSIncrementalDutyCycleMin=0 -XX:CMSIncrementalDutyCycle=10 -XX:-TraceClassUnloading ";
采用了sun推荐的参数，回收效果不好，照样有停顿，数小时之内就会频繁出现停顿，什么sun推荐的参数，照样不好使。
六：递增式低暂停收集器，还叫什么火车式回收，不知道属于哪个系，完成10万request用时153秒
$JAVA_ARGS .= " -Dresin.home=$SERVER_ROOT -server -Xms2048M -Xmx2048M -Xmn512M -XX:PermSize=256M -XX:MaxPermSize=256M -XX:MaxTenuringThreshold=7 -XX:GCTimeRatio=19 -Xnoclassgc -Xloggc:log/gc.log -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+UseTrainGC ";
该配置效果也不好，影响性能，所以没试。
七：相比之下，还是并发回收比较好，性能比较高，只要能解决ParNewGC（并行回收年轻代）时的promotion failed错误就一切好办了，查了很多文章，发现引起promotion failed错误的原因是CMS来不及回收（CMS默认在年老代占到90%左右才会执行），年老代又没有足够的空间供GC把一些活的对象从年轻代移到年老代，所以执行Full GC。CMSInitiatingOccupancyFraction=70表示年老代占到约70%时就开始执行CMS，这样就不会出现Full GC了。SoftRefLRUPolicyMSPerMB这个参数也是我认为比较有用的，官方解释是softly reachable objects will remain alive for some amount of time after the last time they were referenced. The default value is one second of lifetime per free megabyte in the heap，我觉得没必要等1秒，所以设置成0。配置如下
$JAVA_ARGS .= " -Dresin.home=$SERVER_ROOT -server -Xms2048M -Xmx2048M -Xmn512M -XX:PermSize=256M -XX:MaxPermSize=256M -XX:SurvivorRatio=8 -XX:MaxTenuringThreshold=7 -XX:GCTimeRatio=19 -Xnoclassgc -XX:+DisableExplicitGC -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:+CMSPermGenSweepingEnabled -XX:+UseCMSCompactAtFullCollection -XX:CMSFullGCsBeforeCompaction=0 -XX:+CMSClassUnloadingEnabled -XX:-CMSParallelRemarkEnabled -XX:CMSInitiatingOccupancyFraction=70 -XX:SoftRefLRUPolicyMSPerMB=0 -XX:+PrintClassHistogram -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+PrintGCApplicationConcurrentTime -XX:+PrintGCApplicationStoppedTime -Xloggc:log/gc.log ";
上面这个配置内存上升的很慢，24小时之内几乎没有停顿现象，最长的只停滞了0.8s，ParNew GC每30秒左右才执行一次，每次回收约0.2秒，看来问题应该暂时解决了。
参数不明白的可以上网查，本人认为比较重要的几个参数是：-Xms -Xmx -Xmn MaxTenuringThreshold GCTimeRatio UseConcMarkSweepGC CMSInitiatingOccupancyFraction SoftRefLRUPolicyMSPerMB
[7人](http://www.douban.com/group/topic/4450520/?type=like#sep) 喜欢  [喜欢](http://www.douban.com/accounts/register?reason=like "标为喜欢？")

[回应]() [推荐](http://www.douban.com/group/topic/4450520/?type=rec#sep) [喜欢](http://www.douban.com/group/topic/4450520/?type=like#sep)  [只看楼主](http://www.douban.com/group/topic/4450520/?author=1#sep)

* [![肖]()](http://www.douban.com/people/xds2000/)

### [肖](http://www.douban.com/people/xds2000/) (@Beijing) 2008-10-22 22:53:34

首先使用64 bit os,是一个提高性能的不二法规.
第二,任何调优应该有场景,在未知情况应该使用loggc导出日志分析,再加参数.对于参数的好与坏,我希望楼主能贴出更多的自已的思路

[回应](http://www.douban.com/group/topic/4450520/?cid=36481624#last) [删除]( "真的要删除肖的发言?")
* [![KK]()](http://www.douban.com/people/1851090/)

### [KK](http://www.douban.com/people/1851090/) 2008-10-27 15:32:12

还有，如果遇到内存泄漏，要用jstack和jmap、jstat等多看看，应该可以找到问题，还要多看看gc日志各个区域内存使用情况和gc频率，这样才可以作出最佳配置。Good Luck

[回应](http://www.douban.com/group/topic/4450520/?cid=36890822#last) [删除]( "真的要删除KK的发言?")
* [![KK]()](http://www.douban.com/people/1851090/)

### [KK](http://www.douban.com/people/1851090/) 2008-11-03 17:37:21

内存问题和应用有很大关系，我的应用因为用到了缓存，所以年老代比较大，多看看gc日志，出了问题多用Jstack,jmap等工具查看，这样才可以最大的优化JVM参数。经过进两个星期的调试，我的配置是/usr/local/jdk/bin/java -Dresin.home=/usr/local/resin -server -Xms1800M -Xmx1800M -Xmn300M -Xss512K -XX:PermSize=300M -XX:MaxPermSize=300M -XX:SurvivorRatio=8 -XX:MaxTenuringThreshold=5 -XX:GCTimeRatio=19 -Xnoclassgc -XX:+DisableExplicitGC -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:+UseCMSCompactAtFullCollection -XX:CMSFullGCsBeforeCompaction=0 -XX:-CMSParallelRemarkEnabled -XX:CMSInitiatingOccupancyFraction=70 -XX:SoftRefLRUPolicyMSPerMB=0 -XX:+PrintClassHistogram -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+PrintHeapAtGC -Xloggc:log/gc.log
72小时之内没有一点停顿，我是32位LinuxAS4操作系统。

[回应](http://www.douban.com/group/topic/4450520/?cid=37459097#last) [删除]( "真的要删除KK的发言?")
* [![Foxswily]()](http://www.douban.com/people/foxswily/)

### [Foxswily](http://www.douban.com/people/foxswily/) 2008-11-03 17:45:09

mark先，前一段做一个对性能要求高的项目，典型的高DB负载，把着眼点都放在了DB优化上(最终效果也不错，大概提高了40倍左右的性能)，一直没有考虑jvm优化，疏忽了。
but，jvm的优化性价比貌似不是很高，化很大力气得到回报如何有待实践检验。

[回应](http://www.douban.com/group/topic/4450520/?cid=37459727#last) [删除]( "真的要删除Foxswily的发言?")
* [![KK]()](http://www.douban.com/people/1851090/)

### [KK](http://www.douban.com/people/1851090/) 2008-11-03 17:55:02

不是纯粹性能问题，到一定时候JVM参数必须优化，不然访问量大的时候会造成频繁的Full GC或者CMS，Full GC有时能让整个系统停顿10秒，10秒内用户的任何点击都在等待状态，要是隔2分钟就来一次Full GC，恐怕用户和老板都受不了了。
并发回收如果配置的不好照样会产生Full GC。:)

[回应](http://www.douban.com/group/topic/4450520/?cid=37460534#last) [删除]( "真的要删除KK的发言?")
* [![Foxswily]()](http://www.douban.com/people/foxswily/)

### [Foxswily](http://www.douban.com/people/foxswily/) 2008-11-03 17:58:43

有道理，支持这种有内容、有见地的讨论贴。

[回应](http://www.douban.com/group/topic/4450520/?cid=37460849#last) [删除]( "真的要删除Foxswily的发言?")
* [![洛臻星轮]()](http://www.douban.com/people/lozen/)

### [洛臻星轮](http://www.douban.com/people/lozen/) 2012-05-22 23:22:11

感谢下，用了里面的很多参数，目前好使了，让它再多跑跑
项目中Full GC一次停顿了个快十秒，一停顿事件就积压，隔段时间又停顿，事件积压得越来越严重，程序就崩了...

[回应](http://www.douban.com/group/topic/4450520/?cid=347193386#last) [删除]( "真的要删除洛臻星轮的发言?")
* ### [alzq](http://www.douban.com/people/55956054/) 2012-07-20 13:00:19

好配置，实验中……效果不错～

[回应](http://www.douban.com/group/topic/4450520/?cid=364769409#last) [删除]( "真的要删除alzq的发言?")

## 你的回应

回应请先 [登录](http://www.douban.com/accounts/register?reason=discuss) , 或 [注册](http://www.douban.com/accounts/register?reason=discuss)

推荐到广播

[![]()](http://www.douban.com/group/java/?ref=sidebar)

[Java&Android移动应用编程](http://www.douban.com/group/java/?ref=sidebar)

9641 人聚集在这个小组
 [加入小组]()

## 最新话题  ( [更多](http://www.douban.com/group/java/#topics) )

* [[Java开发] Java怎么用POI读取Excel函数](http://www.douban.com/group/topic/41487832/ "[Java开发] Java怎么用POI读取Excel函数")   (若水520sunny)
* [帮5买-招聘ETL开发工程师](http://www.douban.com/group/topic/41485336/ "帮5买-招聘ETL开发工程师")   (挚爱Bernabeu)
* [搜狗-招聘Java软件工程师](http://www.douban.com/group/topic/41482883/ "搜狗-招聘Java软件工程师")   (挚爱Bernabeu)
* [谁能稍稍帮我分析下Logcat，给个提示，感激不尽！](http://www.douban.com/group/topic/41472005/ "谁能稍稍帮我分析下Logcat，给个提示，感激不尽！")   (shanby)
* [推荐一个很好的android&ios学习网站](http://www.douban.com/group/topic/41466302/ "推荐一个很好的android&ios学习网站")   (　)
## 全站热点话题:

* [【搬运中】别小看吃货的智慧 818某些外面...](http://www.douban.com/group/topic/41486032/?r=1 "【搬运中】别小看吃货的智慧 818某些外面卖的看起来挺贵的甜品饮料 很多可以自己在家做")   37回应
* [【推荐】夏日彩妆routine](http://www.douban.com/group/topic/41483943/?r=1 "【推荐】夏日彩妆routine")   55回应
* [有关毕业报到证，公务员，事业单位考试面...](http://www.douban.com/group/topic/41414533/?r=1 "有关毕业报到证，公务员，事业单位考试面试神马的都可以问我")   197回应
* [【意向】Mitsui.家的男装](http://www.douban.com/group/topic/41448032/?r=1 "【意向】Mitsui.家的男装")   45回应
* [【异国】分享一点关于西方世界的看法](http://www.douban.com/group/topic/41321572/?r=1 "【异国】分享一点关于西方世界的看法")   184回应
* [【住在我房子里的美女到底是个什么东西？...](http://www.douban.com/group/topic/41462291/?r=1 "【住在我房子里的美女到底是个什么东西？？？!!!】")   63回应
* [关于天蝎男喜欢女生类型的讨论](http://www.douban.com/group/topic/41358235/?r=1 "关于天蝎男喜欢女生类型的讨论")   283回应
* [求扒苏见信！！！196的黑料有木有？](http://www.douban.com/group/topic/41460023/?r=1 "求扒苏见信！！！196的黑料有木有？")   285回应

### 手机扫描二维码，让小组随时陪伴你

![]()

[App Store下载](http://www.douban.com/mobile/ipa?app_name=group) [点击直接下载](http://www.douban.com/j/app/apk?app_name=group_android)

© 2005－2013 douban.com, all rights reserved   [关于豆瓣](http://www.douban.com/about) · [在豆瓣工作](http://www.douban.com/jobs) · [联系我们](http://www.douban.com/about?topic=contactus) · [免责声明](http://www.douban.com/about?policy=disclaimer) · [帮助中心](http://www.douban.com/help/group) · [开发者](http://developers.douban.com/) · [手机小组](http://www.douban.com/mobile/group) · [豆瓣广告](http://www.douban.com/partner/)

[↑回顶部]()
[×]()
