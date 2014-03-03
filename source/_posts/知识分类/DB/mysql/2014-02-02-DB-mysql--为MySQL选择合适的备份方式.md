---
layout: post
title: "为MySQL选择合适的备份方式"
categories: DB
tags: 
 - DB
 - mysql
--- 

# 为MySQL选择合适的备份方式

[![]()

mysql 5.1同步到5.5卡库问题一则
](http://nettedfish.sinaapp.com/blog/2013/02/22/51-55-slave-sql-thread-error/ "mysql 5.1同步到5.5卡库问题一则")[![]()

一键搭建blackhole从库
](http://nettedfish.sinaapp.com/blog/2012/10/19/auto-build-blackhole-slave "一键搭建blackhole从库")[![]()

如何产生唯一的server id
](http://nettedfish.sinaapp.com/blog/2013/07/24/how-to-generate-unique-server_id/ "如何产生唯一的server id")[![]()

如何生成唯一的server id
](http://nettedfish.sinaapp.com//blog/2013/07/24/how-to-generate-unique-server_id/ "如何生成唯一的server id")

[友荐[?]](http://www.ujian.cc/index2 "友荐是什么？")

[返回顶部]( "返回顶部")[猜你喜欢]( "猜你喜欢")
[![]()

mysql 5.1同步到5.5卡库问题一则
](http://nettedfish.sinaapp.com/blog/2013/02/22/51-55-slave-sql-thread-error/ "mysql 5.1同步到5.5卡库问题一则")[![]()

金泫雅清新素颜照
](http://www.cmsbs.com/pic/zp/4993.html "金泫雅清新素颜照")[![]()

学生办理签证重要的标准
](http://www.cmsbs.com/eut/luixue/5105.html "学生办理签证重要的标准")[![]()

深田恭子艳丽沙滩迷情写真
](http://www.cmsbs.com/pic/hot/5228.html "深田恭子艳丽沙滩迷情写真")[![]()

李颖芝穿金色比基尼与保时捷亲密接触
](http://www.cmsbs.com/pic/xg/5166.html "李颖芝穿金色比基尼与保时捷亲密接触")

[友荐[?]](http://www.ujian.cc/index2 "友荐是什么？")

[返回顶部]( "返回顶部")[猜你喜欢]( "猜你喜欢")

# [Learn and Share](http://nettedfish.sinaapp.com/)

## 陌上发花，可以缓缓醉矣， 忍把浮名，换了浅酌低唱

* [RSS](http://nettedfish.sinaapp.com/atom.xml "RSS订阅")
Navigate…» 首页» 文章归档» 资料分享» 去哪儿招聘» 关于我» RSS

* [首页](http://nettedfish.sinaapp.com/)
* [文章归档](http://nettedfish.sinaapp.com/blog/archives)
* [资料分享](http://nettedfish.sinaapp.com/my_share.html)
* [去哪儿招聘](http://nettedfish.sinaapp.com/enroll.html)
* [关于我](http://nettedfish.sinaapp.com/about.html)
# 为MySQL选择合适的备份方式

2013年-05月-31日 16:01:00
数据库的备份是极其重要的事情。如果没有备份，遇到下列情况就会抓狂：

### UPDATE or DELETE whitout where…

### table was DROPPed accidentally…

### INNODB was corrupt…

### entire datacenter loses power…

从数据安全的角度来说，服务器磁盘都会做raid，MySQL本身也有主从、drbd等容灾机制，但它们都无法完全取代备份。容灾和高可用能帮我们有效的应对物理的、硬件的、机械的故障，而对我们犯下的逻辑错误却无能为力。每一种逻辑错误发生的概率都极低，但是当多种可能性叠加的时候，小概率事件就放大成很大的安全隐患，这时候备份的必要性就凸显了。那么在众多的MySQL备份方式中，哪一种才是适合我们的呢？

## 常见的备份方式

MySQL本身为我们提供了mysqldump、mysqlbinlog远程备份工具，percona也为我们提供了强大的Xtrabackup，加上开源的mydumper，还有基于主从同步的延迟备份、从库冷备等方式，以及基于文件系统快照的备份，其实选择已经多到眼花缭乱。而备份本身是为了恢复，所以能够让我们在出现故障后迅速、准确恢复的备份方式，就是最适合我们的，当然，同时能够省钱、省事，那就非常完美。下面就我理解的几种备份工具进行一些比较，探讨下它们各自的适用场景。

### 1. mysqldump & mydumper

mysqldump是最简单的逻辑备份方式。在备份myisam表的时候，如果要得到一致的数据，就需要锁表，简单而粗暴。而在备份innodb表的时候，加上–master-data=1 –single-transaction 选项，在事务开始时刻，记录下binlog pos点，然后利用mvcc来获取一致的数据，由于是一个长事务，在写入和更新量很大的数据库上，将产生非常多的undo，显著影响性能，所以要慎用。 ![]()

* 优点：简单，可针对单表备份，在全量导出表结构的时候尤其有用。
* 缺点：简单粗暴，单线程，备份慢而且恢复慢，跨IDC有可能遇到时区问题。
mydumper是mysqldump的加强版。相比mysqldump：
* 内置支持压缩，可以节省2-4倍的存储空间。
* 支持并行备份和恢复，因此速度比mysqldump快很多，但是由于是逻辑备份，仍不是很快。

### 2. 基于文件系统的快照

基于文件系统的快照，是物理备份的一种。在备份前需要进行一些复杂的设置，在备份开始时刻获得快照并记录下binlog pos点，然后采用类似copy-on-write的方式，把快照进行转储。转储快照本身会消耗一定的IO资源，而且在写入压力较大的实例上，保存被更改数据块的前印象也会消耗IO，最终表现为整体性能的下降。而且服务器还要为copy-on-write快照预留较多的磁盘空间，这本身对资源也是一种浪费。因此这种备份方式我们使用的不多。 ![]()

### 3. Xtrabackup

这或许是最为广泛的备份方式。percona之所以家喻户晓，Xtrabackup应该功不可没。它实际上是物理备份+逻辑备份的组合。在备份innodb表的时候，它拷贝ibd文件，并一刻不停的监视redo log的变化，append到自己的事务日志文件。在拷贝ibd文件过程中，ibd文件本身可能被写”花”，这都不是问题，因为在拷贝完成后的第一个prepare阶段，Xtrabackup采用类似于innodb崩溃恢复的方法，把数据文件恢复到与日志文件一致的状态，并把未提交的事务回滚。如果同时需要备份myisam表以及innodb表结构等文件，那么就需要用flush tables with lock来获得全局锁，开始拷贝这些不再变化的文件，同时获得binlog位置，拷贝结束后释放锁，也停止对redo log的监视。
它的工作原理如下：
![]()
由于mysql中不可避免的含有myisam表，同时innobackup并不备份表结构等文件，因此想要完整的备份mysql实例，就少不了要执行flush tables with read lock，而这个语句会被任何查询(包括select)阻塞，在阻塞过程中，它又反过来阻塞任何查询(包括select)。如果碰巧备份实例上有长查询先于flush tables with read lock执行，数据库就会hang住。而当flush tables with read lock获得全局锁后，虽然查询可以执行，但是仍会阻塞更新，所以，我们希望flush tables with read lock从发起到结束，持续的时间越短越好。
为了解决这个问题，有两种比较有效的方法：

### 1. 尽量不用myisam表。

### 2. Xtrabackup增加了–rsync选项，通过两次rsync来减少持有全局锁的时间。

优化后的备份过程如下：
![]()

* 优点：在线热备，全备+增备+流备，支持限速，支持压缩，支持加密。
* 缺点：需要获取全局锁，如果遇到长查询，等待时间将不可控，因此要做好监控，必要时杀死长查询或自杀；遇到超大的实例，备份过程较长，redo log太大会影响恢复速度，这种情况下最好采用延迟备份。

### 4. mysqlbinlog 5.6

上述所有的备份方式，都只能把数据库恢复到备份的某个时间点：mysqldump和mydumper，以及snapshot是备份开始的时间点；Xtrabackup是备份结束的时间点。要想实现point in time的恢复，还必须备份binlog。同时binlog也是实现增备的宝贵资源。
幸运的是，mysql 5.6为我们提供了远程备份binlog的选项：

mysqlbinlog --raw --read-from-remote-server --stop-never
它会伪装成mysql从库，从远程获取binlog然后进行转储。这对线上主库容量不够无法保存较多binlog的场景非常有用。但是，它毕竟不像真正的mysql从库实例，状态监控和同步都需要单独部署。因此个人觉得采用blackhole来备份全量的binlog是更好的选择。笔者曾经实现过一个[自动搭建blackhole从库的工具](https://github.com/nettedfish/create_blackhole_slave)，稍加修改，就可以完美搭建出blackhole从库。一旦同步起来，基本一劳永逸，很少出问题，主从切换的时候跟着切了就行。

### 提示：

* 不要小看binlog的备份。当5.6的多线程复制大规模使用后，从库追赶主库命令点的耗时将被极大缩短，这样我们把每天一次的全量备份改为每3天一次、甚至每周一次的全量备份，和持续的binlog增量备份。遇到故障需要恢复数据的时候，重放3、5天的binlog也是极快的。降低备份频率最直接的好处是，省钱、省事。
* blackhole对于备份binlog是极好的。一方面可以长久的备份binlog用于恢复数据库，另一方面，在其上配置半同步复制，可以有效防止主库的binlog丢失。

## 总结

备份方式各有千秋，而对我们来说，面对数千实例，选择合适的备份工具来实现统一配置、统一规划，构建智能调度的备份云平台才是王道。毕竟，多种备份方式共存的运维成本是不容忽视的。
从使用经验来看，用Xtrabackup全备数据，用blackhole增备binlog，并定期对备份数据的有效性进行验证，是当下比较好的选择。

Posted by nettedfish 2013年-05月-31日 16:01:00  [mysql](http://nettedfish.sinaapp.com/blog/categories/mysql/)

[« mysql 5.1同步到5.5卡库问题一则](http://nettedfish.sinaapp.com/blog/2013/02/22/51-55-slave-sql-thread-error/ "Previous Post: mysql 5.1同步到5.5卡库问题一则") [用pt-table-checksum校验数据一致性 »](http://nettedfish.sinaapp.com/blog/2013/06/04/check-replication-consistency-by-pt-table-checksum/ "Next Post: 用pt-table-checksum校验数据一致性")
[![友荐云推荐]()](http://www.ujian.cc/)

[![]()]()[![]()]()

共有1人喜欢  共3条评论
[登录]()[社区]()

[]()

头衔:[确定]()[取消]()

[编辑头衔]()
邮箱:[确定]()[取消]()

[跟踪回复]()

0条评论

0次被喜欢
发表评论

[]()[]()[]()[]()[]()
[]()[]()[]()[]()[]()

![]()

请输入你的评论
        表情
* 默认表情
* 阿狸

140
[发  布]()

![]()

![]()
![]()
[按时间排序]()

排序方式:[按时间排序]()[按热度排序]()
[![]()](http://s.uyan.cc/?u=http%3A%2F%2Fweibo.com%2F1774036133)  [](http://s.uyan.cc/?u=http%3A%2F%2Fweibo.com%2F1774036133 "新浪微博")

[jianfengye110](http://s.uyan.cc/?u=http%3A%2F%2Fweibo.com%2F1774036133) ([新浪微博](http://s.uyan.cc/?u=http%3A%2F%2Fweibo.com%2F1774036133))

推荐～～![]()
[回复]()[]() [踩]() []() [顶]() []()

22小时前

[![]()](http://s.uyan.cc/?u=http%3A%2F%2Fweibo.com%2F1642647770)  [](http://s.uyan.cc/?u=http%3A%2F%2Fweibo.com%2F1642647770 "新浪微博")

[sin30_net](http://s.uyan.cc/?u=http%3A%2F%2Fweibo.com%2F1642647770) ([新浪微博](http://s.uyan.cc/?u=http%3A%2F%2Fweibo.com%2F1642647770))

微博转发：转发微博
[回复]()[]() [踩]() []() [顶]() []()

22小时前
[![]()](http://s.uyan.cc/?u=http%3A%2F%2Fweibo.com%2F2000702247)  [](http://s.uyan.cc/?u=http%3A%2F%2Fweibo.com%2F2000702247 "新浪微博")

[vvsherry](http://s.uyan.cc/?u=http%3A%2F%2Fweibo.com%2F2000702247) ([新浪微博](http://s.uyan.cc/?u=http%3A%2F%2Fweibo.com%2F2000702247))

![]()
[回复]()[]() [踩]() []() [顶1]() []()

6月1日 00:44

更多热评文章

[* ![]()

mysql 5.1同步到5.5卡库问题一则](http://nettedfish.sinaapp.com/blog/2013/02/22/51-55-slave-sql-thread-error/ "mysql 5.1同步到5.5卡库问题一则")

[* ![]()

一键搭建blackhole从库](http://nettedfish.sinaapp.com/blog/2012/10/19/auto-build-blackhole-slave "一键搭建blackhole从库")
[* ![]()

如何产生唯一的server id](http://nettedfish.sinaapp.com/blog/2013/07/24/how-to-generate-unique-server_id/ "如何产生唯一的server id")

[* ![]()

如何生成唯一的server id](http://nettedfish.sinaapp.com//blog/2013/07/24/how-to-generate-unique-server_id/ "如何生成唯一的server id")
[友言[?]](http://www.uyan.cc/ "社会化评论是什么？")

![]()

请输入你的评论
        表情

* 默认表情
* 阿狸
140

[回  复]()
![]()

![]()
![]()
nettedfish.sinaapp.com

43
评论数

8
喜欢数
21
用户数
数据正在加载中...

[友言](http://www.uyan.cc/)

社会化帐号登录

## 点击按钮，使用社交帐号评论

[![]( "用新浪微博账号登陆")  新浪微博]( "用新浪微博账号登陆")[![]( "用腾讯微博账号登陆")  腾讯微博]( "用腾讯微博账号登陆")[![]( "用人人网账号登陆")  人人网]( "用人人网账号登陆")[![]( "用网易微博账号登陆")  网易微博]( "用网易微博账号登陆")[![]( "用搜狐微博账号登陆")  搜狐微博]( "用搜狐微博账号登陆")[![]( "用开心网账号登陆")  开心网]( "用开心网账号登陆")

## 作为游客留言

Email *

昵  称
顶并分享到:

新浪微博
新浪微博

新浪微博
[顶]()[取消]()

## [qunar诚招MySQL DBA](http://nettedfish.sinaapp.com/enroll.html)

# 最新发表

* [图解GIT](http://nettedfish.sinaapp.com/blog/2013/08/05/deep-into-git-with-diagrams/)
* [如何生成唯一的server id](http://nettedfish.sinaapp.com/blog/2013/07/24/how-to-generate-unique-server_id/)
* [用pt-table-sync修复不一致的数据](http://nettedfish.sinaapp.com/blog/2013/06/05/synchronizes-data-efficiently-by-pt-table-sync/)
* [用pt-table-checksum校验数据一致性](http://nettedfish.sinaapp.com/blog/2013/06/04/check-replication-consistency-by-pt-table-checksum/)
* [为MySQL选择合适的备份方式]()
* [mysql 5.1同步到5.5卡库问题一则](http://nettedfish.sinaapp.com/blog/2013/02/22/51-55-slave-sql-thread-error/)
* [python 字符串编解码研究](http://nettedfish.sinaapp.com/blog/2013/01/30/python-encode-decode/)
* [test disk throughtput with dd](http://nettedfish.sinaapp.com/blog/2013/01/07/disk-throughtput/)

# 分类目录

* [linux (1)](http://nettedfish.sinaapp.com/blog/categories/linux)
* [mysql (7)](http://nettedfish.sinaapp.com/blog/categories/mysql)
* [python (1)](http://nettedfish.sinaapp.com/blog/categories/python)
* [tools (3)](http://nettedfish.sinaapp.com/blog/categories/tools)
* [生活札记 (1)](http://nettedfish.sinaapp.com/blog/categories/生活札记)

# GitHub Repos

* [dzpk](https://github.com/nettedfish/dzpk)

德州扑克筹码计算工具
* [gnuplot](https://github.com/nettedfish/gnuplot)

gnuplot作图的一些例子
* [create_blackhole_slave](https://github.com/nettedfish/create_blackhole_slave)

一键搭建blackhole从库
* [psutil](https://github.com/nettedfish/psutil)

psutil
* [codernitydb](https://github.com/nettedfish/codernitydb)

开源的纯python写的nosql数据库,学习
[@nettedfish](https://github.com/nettedfish) on GitHub

# 友情链接

* [淘宝周振兴](http://www.orczhou.com/)
* [去哪儿周彦伟](http://papaisadba.puyu.me/)
* [oracle innodb 官方](https://blogs.oracle.com/MySQL/)
* [坚毅的刀刀](http://www.cnblogs.com/shenguanpu/)
* [存储专栏](http://blog.csdn.net/liuben)
* [火丁笔记](http://huoding.com/)
* [淘宝褚霸](http://blog.yufeng.info/)
* [淘宝丁奇](http://dinglin.iteye.com/)
* [mysql ops](http://www.mysqlops.com/)
* [plinux](http://www.penglixun.com/)
* [hellodba](http://www.hellodb.net/)
* [timyang](http://timyang.net/)
* [ningoo](http://www.ningoo.net/)
* [performanceblog](http://www.mysqlperformanceblog.com/)
* [Xaprb](http://www.xaprb.com/blog/)
* [登博](http://hedengcheng.com/)
* [Domas Mituzas](http://dom.as/)
* [hack mysql](http://hackmysql.com/)
* [dbsnake](http://www.dbsnake.com/)
* [dcy](http://blog.cydu.net/)
* [Shlomi Noach](http://code.openark.org/blog/)
* [yankay](http://www.yankay.com/)
* [吴炳锡](http://www.mysqlsupport.cn/)
* [左耳朵耗子](http://coolshell.cn/)
* [张云杨](http://www.realzyy.com/)
* [最醉红楼](http://www.iamcjd.com/)
* [杨德华](http://dbahacker.com/)
* [火丁笔记](http://huoding.com/)
* [简朝阳](http://isky000.com/)
* [谭俊青](http://www.mysqlab.net/)
* [赖永浩](http://blog.csdn.net/lanphaday)
* [阿里巴巴DBA](http://www.alidba.net/)

Copyright © 2013 - nettedfish - Powered by [Octopress](http://octopress.org/)   [![]()](http://tongji.baidu.com/hm-web/welcome/ico?s=e0f5d44e4da361e95c65c951fc44fec6)
