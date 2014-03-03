---
layout: post
title: "如何优化MySQL insert性能"
categories: DB
tags: 
 - DB
 - mysql
--- 

# 如何优化MySQL insert性能

您还未登录！|[登录](https://passport.csdn.net/account/login)|[注册](https://passport.csdn.net/account/register)|[帮助](https://passport.csdn.net/help/faq)

* [首页](http://www.csdn.net/)
* [业界](http://news.csdn.net/)
* [移动](http://mobile.csdn.net/)
* [云计算](http://cloud.csdn.net/)
* [研发](http://sd.csdn.net/)
* [论坛](http://bbs.csdn.net/)
* [博客](http://blog.csdn.net/)
* [下载](http://download.csdn.net/)
* 
## [更多]()

# [tigernorth的专栏](http://blog.csdn.net/tigernorth)

## Beginning Linux Programming 笔记

* [![]()目录视图](http://blog.csdn.net/tigernorth?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/tigernorth?viewmode=list)
* [![]()订阅](http://blog.csdn.net/tigernorth/rss/list)
[公告：博客新增直接引用代码功能](https://code.csdn.net/blog/12)        [专访谭海燕：移动互联网开发的那些事](http://www.csdn.net/article/2013-07-24/2816320)      [CSDN博客频道自定义域名、标签搜索功能上线啦！](http://blog.csdn.net/csdnproduct/article/details/9495139)      [独一无二的职位：开源社区经理](http://blog.csdn.net/adali/article/details/9813651)

### [如何优化MySQL insert性能]()

分类： [MySQL](http://blog.csdn.net/tigernorth/article/category/1222822)  2012-10-20 23:50 2602人阅读 [评论]()(14) [收藏]( "收藏") [举报]( "举报")
[insert](http://blog.csdn.net/tag/details.html?tag=insert)[mysql](http://blog.csdn.net/tag/details.html?tag=mysql)[优化](http://blog.csdn.net/tag/details.html?tag=%e4%bc%98%e5%8c%96)[sql](http://blog.csdn.net/tag/details.html?tag=sql)[测试](http://blog.csdn.net/tag/details.html?tag=%e6%b5%8b%e8%af%95)[table](http://blog.csdn.net/tag/details.html?tag=table)

    对于一些数据量较大的系统，面临的问题除了是查询效率低下，还有一个很重要的问题就是插入时间长。我们就有一个业务系统，每天的数据导入需要4-5个钟。这种费时的操作其实是很有风险的，假设程序出了问题，想重跑操作那是一件痛苦的事情。因此，提高大数据量系统的MySQL insert效率是很有必要的。

    经过对MySQL的测试，发现一些可以提高insert效率的方法，供大家参考参考。
**1. 一条SQL语句插入多条数据。**
常用的插入语句如：
**[sql]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('0', 'userid_0', 'content_0', 0);  
1. INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('1', 'userid_1', 'content_1', 1);  
INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('0', 'userid_0', 'content_0', 0); INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('1', 'userid_1', 'content_1', 1);修改成：
**[sql]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('0', 'userid_0', 'content_0', 0), ('1', 'userid_1', 'content_1', 1);  
INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('0', 'userid_0', 'content_0', 0), ('1', 'userid_1', 'content_1', 1);

修改后的插入操作能够提高程序的插入效率。这里第二种SQL执行效率高的主要原因有两个，一是减少SQL语句解析的操作， 只需要解析一次就能进行数据的插入操作，二是SQL语句较短，可以减少网络传输的IO。

这里提供一些测试对比数据，分别是进行单条数据的导入与转化成一条SQL语句进行导入，分别测试1百、1千、1万条数据记录。
记录数 单条数据插入 多条数据插入 1百 0.149s 0.011s 1千 1.231s 0.047s 1万 11.678s 0.218s
**2. 在事务中进行插入处理。**
把插入修改成：
**[sql]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. START TRANSACTION;  
1. INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('0', 'userid_0', 'content_0', 0);  
1. INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('1', 'userid_1', 'content_1', 1);  
1. ...  
1. COMMIT;  
START TRANSACTION; INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('0', 'userid_0', 'content_0', 0); INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('1', 'userid_1', 'content_1', 1); ... COMMIT;

使用事务可以提高数据的插入效率，这是因为进行一个INSERT操作时，MySQL内部会建立一个事务，在事务内进行真正插入处理。通过使用事务可以减少数据库执行插入语句时多次“创建事务，提交事务”的消耗，所有插入都在执行后才进行提交操作。

这里也提供了测试对比，分别是不使用事务与使用事务在记录数为1百、1千、1万的情况。
记录数 不使用事务 使用事务 1百 0.149s 0.033s 1千 1.231s 0.115s 1万 11.678s 1.050s
**性能测试：**
这里提供了同时使用上面两种方法进行INSERT效率优化的测试。即多条数据合并为同一个SQL，并且在事务中进行插入。
记录数 单条数据插入 合并数据+事务插入 1万 0m15.977s 0m0.309s 10万 1m52.204s 0m2.271s 100万 18m31.317s 0m23.332s

从测试结果可以看到，insert的效率大概有50倍的提高，这个一个很客观的数字。****

**注意事项：**

1. SQL语句是有长度限制，在进行数据合并在同一SQL中务必不能超过SQL长度限制，通过max_allowed_packe配置可以修改，默认是1M。

2. 事务需要控制大小，事务太大可能会影响执行的效率。MySQL有innodb_log_buffer_size配置项，超过这个值会日志会使用磁盘数据，这时，效率会有所下降。所以比较好的做法是，在事务大小达到配置项数据级前进行事务提交。

转载文章请注明来源： http://blog.csdn.net/tigernorth/article/details/8094277
分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")

1. 上一篇：[MySQL锁的用法之行级锁](http://blog.csdn.net/tigernorth/article/details/7948539)
1. 下一篇：[MySQL 查询数据不一致](http://blog.csdn.net/tigernorth/article/details/8184924)
顶 7 踩 0
查看评论[]()

7楼 [kxxxxxxxb](http://blog.csdn.net/kxxxxxxxb) 2013-05-21 15:19发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/kxxxxxxxb)楼主请教下，我一条sql语句一次性插入30000行数据出错，报的错误是mysql server has gone away ，插入25000行就可以，难道一行sql插入的大小是有限制的？（我已经调高了max_allowed_packet=16M）6楼 [huanggy001](http://blog.csdn.net/huanggy001) 2012-11-28 10:03发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/huanggy001)多谢分享！5楼 [chunhaicao](http://blog.csdn.net/chunhaicao) 2012-10-30 11:26发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/chunhaicao)方法很好啊
只是我们大部分的备份数据都是“
INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('0', 'userid_0', 'content_0', 0);
INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('1', 'userid_1', 'content_1', 1);”
类的，这个有什么好的方法可以快速的转为一条Sql语句吗？Re: [tigernorth](http://blog.csdn.net/tigernorth) 2012-10-30 13:11发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/tigernorth)回复chunhaicao：使用mysqldump就能生成多行数据合并的sql的，用--extended-insert指定一个sql多条记录。4楼 [Shellphon](http://blog.csdn.net/dont27) 2012-10-28 22:51发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/dont27)最后一句：“转载文章请注明来源： http://blog.csdn.net/tigernorth/article/details/8094277”是你写的么？Re: [tigernorth](http://blog.csdn.net/tigernorth) 2012-10-29 12:57发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/tigernorth)回复dont27：在百度搜博文标题，基本都只看到转载网站的搜索结果，CSND有事还看不到，很是郁闷。。Re: [Shellphon](http://blog.csdn.net/dont27) 2012-10-29 23:26发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/dont27)回复tigernorth：我勒个去，都是抓取的吧……话说那句话是你自己写的还是csdn自己加的？Re: [tigernorth](http://blog.csdn.net/tigernorth) 2012-10-29 23:43发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/tigernorth)回复dont27：自己写的。Re: [Shellphon](http://blog.csdn.net/dont27) 2012-10-30 21:28发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/dont27)回复tigernorth：之前百度一些技术的博客，貌似经常出现转载的情况，但都会留下最后“转载”的句子。我看那些抓取这文章的相当坑爹，都直接截取掉了……3楼 [dongfangling](http://blog.csdn.net/dongfangling) 2012-10-23 22:45发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/dongfangling)事物?还是事务?
通过使用事物可以减少创建事物的消耗?Re: [tigernorth](http://blog.csdn.net/tigernorth) 2012-10-24 12:55发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/tigernorth)回复dongfangling：是“事务”，写错了，多谢提醒哈。
是通过使用“事务”可以减少数据库插入数据时“创建事务，提交事务”的消耗，因为只需要开始创建一个事务，结束时提交即可。Re: [dongfangling](http://blog.csdn.net/dongfangling) 2012-10-24 13:52发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/dongfangling)回复tigernorth：COOL2楼 [特务苹果](http://blog.csdn.net/tewuxiaoqiang) 2012-10-23 20:47发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/tewuxiaoqiang)学习了。1楼 [limingchuan123456789](http://blog.csdn.net/limingchuan123456789) 2012-10-22 11:07发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/limingchuan123456789)学习了。
您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Ftigernorth%2Farticle%2Fdetails%2F8094277)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()
[![TOP]()]( "回到顶部")

个人资料

[![]( "访问我的空间")](http://my.csdn.net/tigernorth)
[tigernorth](http://my.csdn.net/tigernorth)

[]( "[加关注]") []( "[发私信]")

* 访问：16316次
* 积分：430分
* 排名：千里之外

* 原创：21篇
* 转载：1篇
* 译文：0篇
* 评论：24条

文章搜索

[]()

文章分类

* [Beginning Linux Programming 笔记](http://blog.csdn.net/tigernorth/article/category/806238)(7)
* [TCP/IP相关工具命令](http://blog.csdn.net/tigernorth/article/category/806236)(1)
* [Thinking in C++](http://blog.csdn.net/tigernorth/article/category/816887)(3)
* [微博开发平台](http://blog.csdn.net/tigernorth/article/category/811054)(1)
* [PHP](http://blog.csdn.net/tigernorth/article/category/1073888)(2)
* [linux](http://blog.csdn.net/tigernorth/article/category/1080760)(2)
* [MySQL](http://blog.csdn.net/tigernorth/article/category/1222822)(5)
* [锁机制](http://blog.csdn.net/tigernorth/article/category/1228827)(1)
* [MySQL锁](http://blog.csdn.net/tigernorth/article/category/1228828)(2)
文章存档

* [2012年11月](http://blog.csdn.net/tigernorth/article/month/2012/11)(1)
* [2012年10月](http://blog.csdn.net/tigernorth/article/month/2012/10)(1)
* [2012年09月](http://blog.csdn.net/tigernorth/article/month/2012/09)(2)
* [2012年08月](http://blog.csdn.net/tigernorth/article/month/2012/08)(1)
* [2012年06月](http://blog.csdn.net/tigernorth/article/month/2012/06)(1)
* [2012年02月](http://blog.csdn.net/tigernorth/article/month/2012/02)(2)
* [2011年10月](http://blog.csdn.net/tigernorth/article/month/2011/10)(1)
* [2011年05月](http://blog.csdn.net/tigernorth/article/month/2011/05)(3)
* [2011年04月](http://blog.csdn.net/tigernorth/article/month/2011/04)(10)

展开

阅读排行

* [如何优化MySQL insert性能]( "如何优化MySQL insert性能")(2602)
* [微博API：获取用户发布的微博](http://blog.csdn.net/tigernorth/article/details/6331008 "微博API：获取用户发布的微博")(2487)
* [如何使用CURL复用连接（PHP）](http://blog.csdn.net/tigernorth/article/details/7242146 "如何使用CURL复用连接（PHP）")(1094)
* [linux排重的方法](http://blog.csdn.net/tigernorth/article/details/7272075 "linux排重的方法")(832)
* [C++循环变量定义生命周期](http://blog.csdn.net/tigernorth/article/details/6388604 "C++循环变量定义生命周期")(700)
* [MySQL锁的用法之行级锁](http://blog.csdn.net/tigernorth/article/details/7948539 "MySQL锁的用法之行级锁")(593)
* [PHP中MySQL连接管理](http://blog.csdn.net/tigernorth/article/details/7909951 "PHP中MySQL连接管理")(583)
* [MySQL 查询数据不一致](http://blog.csdn.net/tigernorth/article/details/8184924 "MySQL 查询数据不一致")(576)
* [编写Linux定时处理程序](http://blog.csdn.net/tigernorth/article/details/7670768 "编写Linux定时处理程序")(515)
* [MySQL锁的用法之表级锁](http://blog.csdn.net/tigernorth/article/details/7935766 "MySQL锁的用法之表级锁")(425)
评论排行

* [如何优化MySQL insert性能]( "如何优化MySQL insert性能")(14)
* [微博API：获取用户发布的微博](http://blog.csdn.net/tigernorth/article/details/6331008 "微博API：获取用户发布的微博")(5)
* [如何使用CURL复用连接（PHP）](http://blog.csdn.net/tigernorth/article/details/7242146 "如何使用CURL复用连接（PHP）")(2)
* [C++循环变量定义生命周期](http://blog.csdn.net/tigernorth/article/details/6388604 "C++循环变量定义生命周期")(1)
* [【转】如何才能做到网站高并发访问?](http://blog.csdn.net/tigernorth/article/details/6844312 "【转】如何才能做到网站高并发访问?")(1)
* [编写Linux定时处理程序](http://blog.csdn.net/tigernorth/article/details/7670768 "编写Linux定时处理程序")(1)
* [linux排重的方法](http://blog.csdn.net/tigernorth/article/details/7272075 "linux排重的方法")(0)
* [PHP中MySQL连接管理](http://blog.csdn.net/tigernorth/article/details/7909951 "PHP中MySQL连接管理")(0)
* [MySQL锁的用法之表级锁](http://blog.csdn.net/tigernorth/article/details/7935766 "MySQL锁的用法之表级锁")(0)
* [MySQL锁的用法之行级锁](http://blog.csdn.net/tigernorth/article/details/7948539 "MySQL锁的用法之行级锁")(0)

推荐文章
最新评论

* [如何优化MySQL insert性能]()

[kxxxxxxxb](http://blog.csdn.net/kxxxxxxxb): 楼主请教下，我一条sql语句一次性插入30000行数据出错，报的错误是mysql server ha...
* [编写Linux定时处理程序](http://blog.csdn.net/tigernorth/article/details/7670768#comments)

[u010138411](http://blog.csdn.net/u010138411): good
* [如何优化MySQL insert性能]()

[huanggy001](http://blog.csdn.net/huanggy001): 多谢分享！
* [如何优化MySQL insert性能]()

[Shellphon](http://blog.csdn.net/dont27): @tigernorth:之前百度一些技术的博客，貌似经常出现转载的情况，但都会留下最后“转载”的句子...
* [如何优化MySQL insert性能]()

[tigernorth](http://blog.csdn.net/tigernorth): @chunhaicao:使用mysqldump就能生成多行数据合并的sql的，用--extended...
* [如何优化MySQL insert性能]()

[chunhaicao](http://blog.csdn.net/chunhaicao): 方法很好啊只是我们大部分的备份数据都是“INSERT INTO `insert_table` (`d...
* [如何优化MySQL insert性能]()

[tigernorth](http://blog.csdn.net/tigernorth): @dont27:自己写的。
* [如何优化MySQL insert性能]()

[Shellphon](http://blog.csdn.net/dont27): @tigernorth:我勒个去，都是抓取的吧……话说那句话是你自己写的还是csdn自己加的？
* [如何优化MySQL insert性能]()

[tigernorth](http://blog.csdn.net/tigernorth): @dont27:在百度搜博文标题，基本都只看到转载网站的搜索结果，CSND有事还看不到，很是郁闷。。
* [如何优化MySQL insert性能]()

[Shellphon](http://blog.csdn.net/dont27): 最后一句：“转载文章请注明来源： http://blog.csdn.net/tigernorth/a...

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)[QQ客服](http://wpa.qq.com/msgrd?v=3&uin=2355263776&site=qq&menu=yes) [微博客服](http://e.weibo.com/csdnsupport/profile) [论坛反馈](http://bbs.csdn.net/forums/Service) [联系邮箱：webmaster@csdn.net](mailto:webmaster@csdn.net) 服务热线：400-600-2320京 ICP 证 070598 号北京创新乐知信息技术有限公司 版权所有世纪乐知(北京)网络技术有限公司 提供技术支持江苏乐知网络技术有限公司 提供商务支持Copyright © 1999-2012, CSDN.NET, All Rights Reserved [![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
![](http://counter.csdn.net/pv.aspx?id=24)
