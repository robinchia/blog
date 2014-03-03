---
layout: post
title: "MySQL-5.6-和-MariaDB-10.0-的性能比较测试"
categories: DB
tags: 
 - DB
 - mariadb
--- 

# MySQL-5.6-和-MariaDB-10.0-的性能比较测试

[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/mini_07a0c0d02f0ff9b5eb35_120_120.jpg)

上使用MySQL的简单总结
](http://www.xiuxiandou.com/content-blog-3041-LJMM%E5%B9%B3%E5%8F%B0%EF%BC%88%20Linux%20+Jexus+MySQL+mono%EF%BC%89%20%E4%B8%8A%E4%BD%BF%E7%94%A8MySQL%E7%9A%84%E7%AE%80%E5%8D%95%E6%80%BB%E7%BB%93 "上使用MySQL的简单总结")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/21.jpg)

php与mysql
](http://www.xiuxiandou.com/content-blog-3114-php%E4%B8%8Emysql-web-%E5%BC%80%E5%8F%91%E6%8A%80%E6%9C%AF%E8%AF%A6%E8%A7%A3---php%E6%8E%A7%E5%88%B6%E7%BB%93%E6%9E%84%E5%92%8C%E5%87%BD%E6%95%B0 "php与mysql")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/4.jpg)

LJMM平台（
](http://www.xiuxiandou.com/content-blog-3041-LJMM%E5%B9%B3%E5%8F%B0%EF%BC%88%20Linux%20%20Jexus%20MySQL%20mono%EF%BC%89%20%E4%B8%8A%E4%BD%BF%E7%94%A8MySQL%E7%9A%84%E7%AE%80%E5%8D%95%E6%80%BB%E7%BB%93 "LJMM平台（")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/42.jpg)

JDeveloper
](http://www.xiuxiandou.com/content-blog-220-How-to-Run-Standard-OA-Framework-Pages-from-JDeveloper "JDeveloper")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/33.jpg)

《审计学习题集》扫描版[PDF]
](http://www.xiuxiandou.com/content-book-38684-%E3%80%8A%E5%AE%A1%E8%AE%A1%E5%AD%A6%E4%B9%A0%E9%A2%98%E9%9B%86%E3%80%8B%E6%89%AB%E6%8F%8F%E7%89%88[PDF] "《审计学习题集》扫描版[PDF]")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/26.jpg)

盖茨扎克伯格现身说法：讲述编程之酷
](http://www.xiuxiandou.com/content-it-14757-%E7%9B%96%E8%8C%A8%E6%89%8E%E5%85%8B%E4%BC%AF%E6%A0%BC%E7%8E%B0%E8%BA%AB%E8%AF%B4%E6%B3%95%EF%BC%9A%E8%AE%B2%E8%BF%B0%E7%BC%96%E7%A8%8B%E4%B9%8B%E9%85%B7 "盖茨扎克伯格现身说法：讲述编程之酷")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/mini_5d312b52998536eb1b8b_120_120.jpg)

360黑匣子之谜，你怎么看？
](http://www.1ban.com.cn/zh-CN/surveys/extend_info?survey_id=97 "360黑匣子之谜，你怎么看？")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/mini_4e33b31aa1a9b82f7701_120_120.jpg)

云访问-企业网站全球云加速服务
](http://www.yunfangwen.com/?ujian "云访问-企业网站全球云加速服务")

[友荐[?]](http://www.ujian.cc/index2 "友荐是什么？")

[返回顶部](http://www.xiuxiandou.com/# "返回顶部")[猜你喜欢](http://www.xiuxiandou.com/# "猜你喜欢")
[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/mini_07a0c0d02f0ff9b5eb35_120_120.jpg)

上使用MySQL的简单总结
](http://www.xiuxiandou.com/content-blog-3041-LJMM%E5%B9%B3%E5%8F%B0%EF%BC%88%20Linux%20+Jexus+MySQL+mono%EF%BC%89%20%E4%B8%8A%E4%BD%BF%E7%94%A8MySQL%E7%9A%84%E7%AE%80%E5%8D%95%E6%80%BB%E7%BB%93 "上使用MySQL的简单总结")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/3.jpg)

php与mysql
](http://www.xiuxiandou.com/content-blog-3114-php%E4%B8%8Emysql-web-%E5%BC%80%E5%8F%91%E6%8A%80%E6%9C%AF%E8%AF%A6%E8%A7%A3---php%E6%8E%A7%E5%88%B6%E7%BB%93%E6%9E%84%E5%92%8C%E5%87%BD%E6%95%B0 "php与mysql")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/13.jpg)

LJMM平台（
](http://www.xiuxiandou.com/content-blog-3041-LJMM%E5%B9%B3%E5%8F%B0%EF%BC%88%20Linux%20%20Jexus%20MySQL%20mono%EF%BC%89%20%E4%B8%8A%E4%BD%BF%E7%94%A8MySQL%E7%9A%84%E7%AE%80%E5%8D%95%E6%80%BB%E7%BB%93 "LJMM平台（")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/19.jpg)

JDeveloper
](http://www.xiuxiandou.com/content-blog-220-How-to-Run-Standard-OA-Framework-Pages-from-JDeveloper "JDeveloper")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/7.jpg)

《审计学习题集》扫描版[PDF]
](http://www.xiuxiandou.com/content-book-38684-%E3%80%8A%E5%AE%A1%E8%AE%A1%E5%AD%A6%E4%B9%A0%E9%A2%98%E9%9B%86%E3%80%8B%E6%89%AB%E6%8F%8F%E7%89%88[PDF] "《审计学习题集》扫描版[PDF]")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/43.jpg)

盖茨扎克伯格现身说法：讲述编程之酷
](http://www.xiuxiandou.com/content-it-14757-%E7%9B%96%E8%8C%A8%E6%89%8E%E5%85%8B%E4%BC%AF%E6%A0%BC%E7%8E%B0%E8%BA%AB%E8%AF%B4%E6%B3%95%EF%BC%9A%E8%AE%B2%E8%BF%B0%E7%BC%96%E7%A8%8B%E4%B9%8B%E9%85%B7 "盖茨扎克伯格现身说法：讲述编程之酷")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/mini_a897a95cdb5fc07444b6_120_120.jpg)

【爱慕20周年庆】新品首次特惠 8折体验
](http://www.aimer.com.cn/interface.php?u_id=20016&sub_id=tongtou&m=4&url=http://www.aimer.com.cn/z/es/38new20130301.shtml "【爱慕20周年庆】新品首次特惠 8折体验")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/mini_f142ed01aae662177abf_120_120.jpg)

中高考冲刺计时 早一天抓住“核心要点” 就多一分制胜把握
](http://rrurl.cn/pNQDfO "中高考冲刺计时 早一天抓住“核心要点” 就多一分制胜把握")

[友荐[?]](http://www.ujian.cc/index2 "友荐是什么？")

[返回顶部](http://www.xiuxiandou.com/# "返回顶部")[猜你喜欢](http://www.xiuxiandou.com/# "猜你喜欢")
[设为首页]() [收藏本站]()

## []( "MySQL-5.6-和-MariaDB-10.0-的性能比较测试")

用户名 暂时未开放注册密码*登录*

[]()

* [今日更新](http://www.xiuxiandou.com/indexs "BT电影高清种子,下载")
* [今日兼职](http://www.xiuxiandou.com/indexs "BT电影高清种子,下载")
* [日志Blog](http://www.xiuxiandou.com/blog "Blog-界面来源于<水平凡>")
* [记记JiJi](http://www.xiuxiandou.com/jiji "记记,每天记录一点")
* [留言Blog](http://www.xiuxiandou.com/me "给我留言")
电影 游戏 电子书 资讯 IT博客  **搜索**

今日 资讯:*90* 条| 电影:*1*部|书籍: *5* 本|游戏: *5* 款|IT博客:*46*条|评论: *1* 条| 留言: *0* 条

# 某鲜花店的广告：今日本店的玫瑰售价最为低廉，甚至可以买几朵送给太太。

# 我不是随便的人！但随便起来就不是人！
# MySQL 5.6 和 MariaDB-10.0 的性能比较测试

[休闲豆](http://www.xiuxiandou.com/ "休闲豆-MySQL 5.6 和 MariaDB-10.0 的性能比较测试") 已有 33 次阅读 2013-02-14 10:41:24 来源 **[开源中国社区](http://www.oschina.net/question/12_90065 "点击查看来源: 开源中国社区")**

       

JetBrains 开发工具全场2折，[详情»](http://www.oschina.net/shop/jetbrains)
 
Oracle 刚刚发布了 [MySQL 5.6.10 GA](http://www.oschina.net/news/37438/oracle-mysql-5-6-final) 版本，所以是时候更新下之前的性能测试数据了，此次的测试包括以下几个版本：

 
* MySQL-5.5.29 
* MySQL-5.6.10 
* MariaDB-5.5.28a 
* MariaDB-10.0.1 

此次测试还保留了 5.5 版本是为了进行回归测试。之前我们经常发现新版本在性能上反倒而落后的情况。

此次测试在不同的环境下执行，主要的不同是没用 SSD 而是使用高性能的带 512 兆 battery-backed 缓存的 RAID-5 存储。此外测试的机器具有 16 核，其中 12 核运行 mysqld ，另外 4 核运行 sysbench。

测试使用 sysbench-0.5 OLTP ，包好 8 个表和 10G 的数据。InnoDB 的缓冲池大小是 16G，日志 4G。不同的磁盘系统要求不同的 InnoDB 配置：

 
* innodb_io_capacity = 1000 (was 20000 for SSD) 
* innodb_flush_neighbors = 1 (was 0 for SSD) 

下面是测试结果，首先是 OLTP 只读：

[![xiuxiandou.com](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/14101044_u5Dc.png "xiuxiandou.com")
](http://blog.mariadb.org/wp-content/uploads/2013/02/20130213-sb-ro-tps.png)

非常奇怪，MySQL 5.6 在此轮测试中居然表现异常。在 8 个线程时相差不大，在 16 个线程时变现最佳。但更高的并发下性能就迅速下降，甚至比 MySQL 5.5 还差。而 MariaDB 10.0 则比 MariaDB 5.5 表现上要差一些，但没那么明显。

而响应时间图表则表示比较好而且平滑：

[![xiuxiandou.com](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/14101044_1vV1.png "xiuxiandou.com")
](http://blog.mariadb.org/wp-content/uploads/2013/02/20130213-sb-ro-rt.png)

MySQL-5.6 和 MariaDB-10.0 看起来要稍微好一些，意味着它们能更好的分配 CPU 周期。

声明: 此次测试没有使用线程池。Oracle 的线程池实现已经闭源了，因此没法进行测试和使用，如果在 MariaDB 上使用线程池的话就显得有点不公平。

如果你想了解线程池对性能的影响，可查看之前的两篇文章：

 
* [MariaDB-5.5 thread pool performance](http://blog.mariadb.org/mariadb-5-5-thread-pool-performance/) 
* [MariaDB-5.5 performance on Windows](http://blog.mariadb.org/mariadb-5-5-performance-on-windows/) 

第二个测试：OLTP 读写测试

[![xiuxiandou.com](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/14101045_l8vb.png "xiuxiandou.com")
](http://blog.mariadb.org/wp-content/uploads/2013/02/20130213-sb-rw-tps.png)

这个图跟前一个测试差不多，MySQL 5.6 和 MariaDB 10.0 在性能表现上都比 5.5 版本要下滑不少，在高负载的情况下，下滑了 10% 左右。

这是一个人所共知的事实，MySQL 5.5 在高负载下因为同步的 flush 操作导致的性能下滑。

响应时间图相对要好一些：

[![xiuxiandou.com](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/14101045_7ZB4.png "xiuxiandou.com")
](http://blog.mariadb.org/wp-content/uploads/2013/02/20130213-sb-rw-rt.png)

这是一个好消息，5.5 版本在 64 个线程或者更多线程的情况下响应时间差了很多。而 MySQL 5.6 和 MariaDB 10.0 的适应性 flush 算法似乎工作良好。

这里还有一个问题：如果你使用多个缓冲池实例，你将会看到写操作延迟更厉害。上面的结果中，只读测试使用了 16 个缓冲池，而读写测试只用了 1个。

结论：

 
* MySQL-5.6 性能表现比前一个版本要差，特别是高并发的情况下。这与 Oracle 发布的测试结果不符。我只能推测为什么结果差异那么大，我猜是 Oracle 闭源的线程池以及 Oracle 在更大的机器上进行测试导致。 
* 使用单个缓冲池情况下不需要担心写延迟的问题。同时 MySQL 5.6 允许高达 512G 的 redo 日志可降低同步 flush 操作。 

此次测试的脚本可通过下面地址访问：
[http://bazaar.launchpad.net/~ahel/maria/mariadb-benchmarks/revision/20](http://bazaar.launchpad.net/~ahel/maria/mariadb-benchmarks/revision/20)

欢迎大家重做这个测试并与我们分享测试结果。

via [mariadb](http://blog.mariadb.org/sysbench-oltp-mysql-5-6-vs-mariadb-10-0/)

  **标签：**   [MySQL](http://www.oschina.net/question/tag/mysql "数据库服务器 MySQL") [MariaDB](http://www.oschina.net/question/tag/mariadb "MySQL 分支 MariaDB") [SysBench](http://www.oschina.net/question/tag/sysbench "性能测试工具 SysBench")
    [补充话题说明»]() 

 
 
 
       
**分享到** []( "分享到新浪微博")[]( "分享到腾讯微博")**

[收藏]( "收藏此话题")
**

9
**

[举报]()
**

        [踩]( "踩：这问题不知道在说什么，或者没什么用")  0  |        [顶]( "顶：这问题很有用或者很清晰明了")  0 
  ** 
 
       
##     [按默认排序](http://www.oschina.net/question/12_90065#answers) |  [显示最新评论](http://www.xiuxiandou.com/?sort=time#answers) | [回页面顶部](http://www.xiuxiandou.com/#top)    []()共有*6*个评论 [发表评论»](http://www.xiuxiandou.com/#answerform) 

* [![xiuxiandou.com](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/84873_50.jpg "xiuxiandou.com")
](http://my.oschina.net/yz2yz)

[铂金小鬼](http://my.oschina.net/yz2yz) 回答于 2013-02-14 10:16

      [举报]()   
哇哇，到底是好，还是差啊？！ 
[有帮助]( "这是一个好评论，能解决问题")*(0)* |  [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* |  [评论]()*(0)* |    [引用此评论](http://www.xiuxiandou.com/question/answer?question=90065&answer=415509) 
* [![xiuxiandou.com](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/23734_50.jpg "xiuxiandou.com")
](http://my.oschina.net/mallon)

[Mallon](http://my.oschina.net/mallon) 回答于 2013-02-14 10:25

      [举报]()   
### 引用来自“铂金小鬼”的答案

哇哇，到底是好，还是差啊？！

半斤八两，在误差允许范围内，哈哈
 
**—- 共有 1 条评论—-**

 
*   [![xiuxiandou.com](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/130291_50.jpg "xiuxiandou.com")
](http://my.oschina.net/samshuai)    差距不大的话…我还是愿意用mysql  (11小时前 by 蟋蟀哥哥)  [回复]()     
  

[有帮助]( "这是一个好评论，能解决问题")*(0)* |  [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* |  [评论]()*(1)* |    [引用此评论](http://www.xiuxiandou.com/question/answer?question=90065&answer=415510)
 
* [](http://my.oschina.net/u/569120)
[![xiuxiandou.com](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/569120_50.jpg "xiuxiandou.com")
](http://my.oschina.net/u/569120)

[](http://my.oschina.net/u/569120)

[光头程序员](http://my.oschina.net/u/569120) 回答于 2013-02-14 10:45 来自 [Android](http://www.oschina.net/mobile)

      [举报]()   
说这么多 我又看不懂 直接说应该用哪个不就行了 
[有帮助]( "这是一个好评论，能解决问题")*(0)* |  [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* |  [评论]()*(0)* |    [引用此评论](http://www.xiuxiandou.com/question/answer?question=90065&answer=415513) 
* [](http://my.oschina.net/u/725743)
[![xiuxiandou.com](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/725743_50.jpg "xiuxiandou.com")
](http://my.oschina.net/u/725743)

[](http://my.oschina.net/u/725743)

[严瑾](http://my.oschina.net/u/725743) 回答于 2013-02-14 13:55 来自 [Android](http://www.oschina.net/mobile)

      [举报]()   
！！！！！！！！！！！没看懂 
[有帮助]( "这是一个好评论，能解决问题")*(0)* |  [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* |  [评论]()*(0)* |    [引用此评论](http://www.xiuxiandou.com/question/answer?question=90065&answer=415545) 
* [](http://my.oschina.net/bwin)
[![xiuxiandou.com](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/152915_50.jpg "xiuxiandou.com")
](http://my.oschina.net/bwin)

[](http://my.oschina.net/bwin)

[新君悦](http://my.oschina.net/bwin) 回答于 2013-02-14 14:11

      [举报]()   
按照以往的"经验"  凡是出现Mysql的文章都会被某些人"喷"的,正在奇怪这篇文章怎么没人喷口水呢，拖上一看 原来作者是站长 难怪...
 
**—- 共有 1 条评论—-**

 
*   [![xiuxiandou.com](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/12_50.jpg "xiuxiandou.com")
](http://my.oschina.net/javayou)    翻译的  (8小时前 by 红薯)  [回复]()     
  

[有帮助]( "这是一个好评论，能解决问题")*(0)* |  [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* |  [评论]()*(1)* |    [引用此评论](http://www.xiuxiandou.com/question/answer?question=90065&answer=415549)
 
* [](http://my.oschina.net/bootstrap)
[![xiuxiandou.com](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/736867_50.jpg "xiuxiandou.com")
](http://my.oschina.net/bootstrap)

[](http://my.oschina.net/bootstrap)

[抓瓦Or](http://my.oschina.net/bootstrap) 回答于 2013-02-14 14:58

      [举报]()   
mysql这么好的东西，为啥乱喷，Oracle 虽然商业化严重，但是其技术还是可以的吧 
 
[有帮助]( "这是一个好评论，能解决问题")*(0)* |  [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* |  [评论]()*(0)* |    [引用此评论](http://www.xiuxiandou.com/question/answer?question=90065&answer=415559) 
       
 
[]()
[![xiuxiandou.com](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/portrait.gif "xiuxiandou.com")
]()

[]()

          
  [回评论顶部](http://www.xiuxiandou.com/#answers) | [回页面顶部](http://www.xiuxiandou.com/#top)    
 

资源来源互联网,版权归该下载资源的合法拥有者所有。

本文链接：[http://www.xiuxiandou.com/content-it-11099.html](http://www.xiuxiandou.com/content-it-11099)
关键字：MySQL 5.6 和 MariaDB-10.0 的性能比较测试
若无特别注明，文章皆为[休闲豆](http://www.xiuxiandou.com/)原创，转载请注明出处

注:本文来源地址为 :[开源中国社区](http://www.oschina.net/question/12_90065 "点击查看来源: 开源中国社区")

[0]()

![正在加载推荐文章](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/loading.gif)

休闲豆推荐：
[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/mini_07a0c0d02f0ff9b5eb35_120_120.jpg)

上使用MySQL的简单总结
](http://www.xiuxiandou.com/content-blog-3041-LJMM%E5%B9%B3%E5%8F%B0%EF%BC%88%20Linux%20+Jexus+MySQL+mono%EF%BC%89%20%E4%B8%8A%E4%BD%BF%E7%94%A8MySQL%E7%9A%84%E7%AE%80%E5%8D%95%E6%80%BB%E7%BB%93 "上使用MySQL的简单总结")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/1.jpg)

php与mysql
](http://www.xiuxiandou.com/content-blog-3114-php%E4%B8%8Emysql-web-%E5%BC%80%E5%8F%91%E6%8A%80%E6%9C%AF%E8%AF%A6%E8%A7%A3---php%E6%8E%A7%E5%88%B6%E7%BB%93%E6%9E%84%E5%92%8C%E5%87%BD%E6%95%B0 "php与mysql")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/6.jpg)

LJMM平台（
](http://www.xiuxiandou.com/content-blog-3041-LJMM%E5%B9%B3%E5%8F%B0%EF%BC%88%20Linux%20%20Jexus%20MySQL%20mono%EF%BC%89%20%E4%B8%8A%E4%BD%BF%E7%94%A8MySQL%E7%9A%84%E7%AE%80%E5%8D%95%E6%80%BB%E7%BB%93 "LJMM平台（")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/11.jpg)

JDeveloper
](http://www.xiuxiandou.com/content-blog-220-How-to-Run-Standard-OA-Framework-Pages-from-JDeveloper "JDeveloper")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/23.jpg)

《审计学习题集》扫描版[PDF]
](http://www.xiuxiandou.com/content-book-38684-%E3%80%8A%E5%AE%A1%E8%AE%A1%E5%AD%A6%E4%B9%A0%E9%A2%98%E9%9B%86%E3%80%8B%E6%89%AB%E6%8F%8F%E7%89%88[PDF] "《审计学习题集》扫描版[PDF]")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/14.jpg)

盖茨扎克伯格现身说法：讲述编程之酷
](http://www.xiuxiandou.com/content-it-14757-%E7%9B%96%E8%8C%A8%E6%89%8E%E5%85%8B%E4%BC%AF%E6%A0%BC%E7%8E%B0%E8%BA%AB%E8%AF%B4%E6%B3%95%EF%BC%9A%E8%AE%B2%E8%BF%B0%E7%BC%96%E7%A8%8B%E4%B9%8B%E9%85%B7 "盖茨扎克伯格现身说法：讲述编程之酷")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/mini_579fbfdf75e09aeb0b83_120_120.jpg)

牙疼小妙招，立竿见影！
](http://www.duobei.com/room/2831608242?utm_source=weibo&utm_medium=plugin&utm_campaign=UweiboA115 "牙疼小妙招，立竿见影！")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/mini_87b8684f627768210ab3_120_120.jpg)

【爱慕官方商城】爱慕20周年庆 3.8女人节 感恩回馈3折起！
](http://www.aimer.com.cn/interface.php?u_id=20016&sub_id=tongtou&url=http://www.aimer.com.cn/ "【爱慕官方商城】爱慕20周年庆 3.8女人节 感恩回馈3折起！")

[友荐[?]](http://www.ujian.cc/index2 "友荐是什么？")
[]()[]()[]()[]()[]()

[社区]()
0[ 喜欢]()

[]()

头衔:[确定]()[取消]()

[编辑头衔]()
邮箱:[确定]()[取消]()

[跟踪回复]()

0条评论
0次被喜欢
![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/duface.png)

请输入你的评论

140

* 默认表情
* 阿狸
[发  布]()

[]( "加入邮箱和网址信息")
[]( "使用新浪微博账号登陆")[]( "使用QQ账号账号登陆")[]( "使用腾讯微博账号登陆")[]( "使用人人账号登陆")[]( "使用搜狐微博账号登陆")[]( "选择更多登陆账号")[]( "kaixin001")[]( "163")

![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/kaixin001.png)

![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/tsohu.png)
![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/t163.png)
[按时间排序]()

|
[新浪微博]()

|
[腾讯微博]()

排序方式:[按时间排序]()[按热度排序]()
还没有评论内容

更多热评文章

[* ![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/mini_07a0c0d02f0ff9b5eb35_120_120.jpg)

上使用MySQL的简单总结](http://www.xiuxiandou.com/content-blog-3041-LJMM%E5%B9%B3%E5%8F%B0%EF%BC%88%20Linux%20+Jexus+MySQL+mono%EF%BC%89%20%E4%B8%8A%E4%BD%BF%E7%94%A8MySQL%E7%9A%84%E7%AE%80%E5%8D%95%E6%80%BB%E7%BB%93 "上使用MySQL的简单总结")

[* ![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/3.jpg)

php与mysql](http://www.xiuxiandou.com/content-blog-3114-php%E4%B8%8Emysql-web-%E5%BC%80%E5%8F%91%E6%8A%80%E6%9C%AF%E8%AF%A6%E8%A7%A3---php%E6%8E%A7%E5%88%B6%E7%BB%93%E6%9E%84%E5%92%8C%E5%87%BD%E6%95%B0 "php与mysql")
[* ![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/mini_5d312b52998536eb1b8b_120_120.jpg)

360黑匣子之谜，你怎么看？](http://www.1ban.com.cn/zh-CN/surveys/extend_info?survey_id=97 "360黑匣子之谜，你怎么看？")

[* ![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/mini_4e33b31aa1a9b82f7701_120_120.jpg)

云访问-企业网站全球云加速服务](http://www.yunfangwen.com/?ujian "云访问-企业网站全球云加速服务")
[友言[?]](http://www.uyan.cc/ "社会化评论是什么？")

![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/duface.png)

请输入你的评论

140

* 默认表情
* 阿狸
[回  复]()

[]( "加入邮箱和网址信息")
[]( "使用新浪微博账号登陆")[]( "使用QQ账号账号登陆")[]( "使用腾讯微博账号登陆")[]( "使用人人账号登陆")[]( "使用搜狐微博账号登陆")[]( "选择更多登陆账号")[]( "kaixin001")[]( "163")

![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/kaixin001.png)

![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/tsohu.png)
![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/t163.png)
www.xiuxiandou.com

1
评论数

-1
喜欢数
1
用户数
数据正在加载中...

[友言](http://www.uyan.cc/)

社会化帐号登录

## 点击按钮，使用社交帐号评论

[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/tsina.png "用新浪微博帐号登录")  新浪微博]( "用新浪微博帐号登录")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/qq.png "用QQ帐号登录")  QQ]( "用QQ帐号登录")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/tqq.png "用腾讯微博帐号登录")  腾讯微博]( "用腾讯微博帐号登录")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/renren.png "用人人网帐号登录")  人人网]( "用人人网帐号登录")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/t163.png "用网易微博帐号登录")  网易微博]( "用网易微博帐号登录")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/tsohu.png "用搜狐微博帐号登录")  搜狐微博]( "用搜狐微博帐号登录")[![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/kaixin001.png "用开心网帐号登录")  开心网]( "用开心网帐号登录")

## 作为游客留言

Email *

昵  称
顶并分享到:

新浪微博
新浪微博

新浪微博
[顶]()[取消]()

* ### [
## 简明现代魔法

](http://www.nowamagic.net/ "简明现代魔法")
* ### [
## 百度博客

](http://hi.baidu.com/273579540 "我的百度博客")
* ### [
## 我的代码托管

](https://github.com/hubs "代码托管")
* ### [
## 不求人薪水网

](http://www.orzro.net/ "不求人薪水网")
* ### [
## 360安全网址导航

](http://hao.360.cn/ "360安全网址导航")
### ****网站数据** - **828** 部电影, - **1463** 款游戏, -**10812** 条资讯, -**950** 本电子书, -**3351** 条IT博客 , -**13** 条评论, -**1** 条留言, -**10** 个网站模板, .**

[**休闲豆QQ在线**](http://wpa.qq.com/msgrd?v=3&uin=1963612630&site=qq&menu=yes)| [本站RSS](http://www.xiuxiandou.com/feed)| [网站地图](http://www.xiuxiandou.com/sitemap)| [申请友链](http://www.xiuxiandou.com/indexs/link)| [关于我们](http://www.xiuxiandou.com/indexs/about)| [手机版](http://wap.xiuxiandou.com/)| **[休闲豆](http://www.xiuxiandou.com/)** ( [桂ICP备11002319号-1](http://www.miitbeian.gov.cn/) )   [![](http://./MySQL-5.6-和-MariaDB-10.0-的性能比较测试_files/21.gif)](http://tongji.baidu.com/hm-web/welcome/ico?s=cc369fcd21817e7c8c74e1ce3ee37f8f)

13-03-06 10:03:21

Powered by **[Xiuxiandou.com!](http://www.xiuxiandou.com/)**

© 2001-2012 [Xiuxiandou.com](http://www.comsenz.com/) skin by [eisdl](http://www.eisdl.com/)
本站是一个免费提供电影资源下载的网站，本站资源均为网络免费资源搜索机器人自动搜索的结果，本站只和网友相互分享和测试电影资源搜索结果，并不存放任何资源！所有视频版权归原权利人，如有关视频侵犯了你的权益，请联系本站告之，本站将于24小时内删除！ 我们强烈建议所有影视爱好者测试后24小时内删除资源！并购买正版音像制品！
