---
layout: post
title: "MySQL 忘记root密码，解决办法"
categories: DB
tags: 
 - DB
 - mysql
--- 

# MySQL 忘记root密码，解决办法

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼]()

[招聘](http://job.iteye.com/iteye) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://snowolf.iteye.com/login "登录") [登录](http://snowolf.iteye.com/login) [注册](http://snowolf.iteye.com/signup)

# [Snowolf的意境空间！](http://snowolf.iteye.com/)

* [**博客**](http://snowolf.iteye.com/)
* [微博](http://snowolf.iteye.com/weibo)
* [相册](http://snowolf.iteye.com/album)
* [收藏](http://snowolf.iteye.com/link)
* [留言](http://snowolf.iteye.com/blog/guest_book)
* [关于我](http://snowolf.iteye.com/blog/profile)

### [MySQL 忘记root密码，解决办法]() **

**博客分类：**
* [DB／MySQL](http://snowolf.iteye.com/category/34962)
[mysql](http://www.iteye.com/blogs/tag/mysql)[root](http://www.iteye.com/blogs/tag/root)

今天遇到个破问题：用了N久的MySQL要新建数据库，竟然忘记了密码。![]() 而这个问题居然也很常见！![]()
要修改MySQL的root密码，有两个先决条件：

* 有修改MySQL配置文件的权限
* 有重启MySQL服务的权限
先修改配置文件：
引用

# vim /etc/my.cnf
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
user=mysql
# Default to using old password format for compatibility with mysql 3.x
# clients (those using the mysqlclient10 compatibility package).
old_passwords=1
# Disabling symbolic-links is recommended to prevent assorted security risks;
# to do so, uncomment this line:
# symbolic-links=0
[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
在**[mysqld]**下增加**skip-grant-tables**，即跳过权限验证。
然后登录MySQL，修改root密码：
引用

# mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.0.95 Source distribution
Copyright (c) 2000, 2011, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
Database changed
mysql> update user SET Password = password('<Your Password>') where user='root';
Query OK, 3 rows affected (0.00 sec)
Rows matched: 3  Changed: 3  Warnings: 0
mysql> flush privileges ;
Query OK, 0 rows affected (0.00 sec)
mysql> quit
Bye
然后把刚才修改的配置文件再改回来，最后重启服务：
引用

# service mysqld restart
停止 MySQL：                                               [确定]
启动 MySQL：                                               [确定]
大功告成！![]()
PS：更果断的办法：
引用

关闭mysqld
命令行执行 mysqld --skip-grant-tables & 无密码登陆！
突然了解了，黑客入侵，就这么简单。。。。![]()
**5**
顶

**4**
踩

分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[Eclipse+Maven快速生成Web项目，解决部署 ...](http://snowolf.iteye.com/blog/1627343 "Eclipse+Maven快速生成Web项目，解决部署时Maven lib依赖问题") | [Linux环境小问题——Get HostName Error](http://snowolf.iteye.com/blog/1622203 "Linux环境小问题——Get HostName Error")
* 2012-08-08 18:09
* 浏览 3165
* [评论(4)]()
* 分类:[数据库](http://www.iteye.com/blogs/category/database)
* [相关推荐](http://www.iteye.com/wiki/blog/1625674)

### 评论

[]()

4 楼 [snowolf](http://snowolf.iteye.com/ "snowolf") 2012-08-09  

dwangel 写道

先关掉mysqld
然后，直接命令行执行 mysqld --skip-grant-tables &就可以了。
不用改配置。
改配置要改过去，还要改回来。
这个果断！！！![]()
3 楼 [dwangel](http://dwangel.iteye.com/ "dwangel") 2012-08-09  

先关掉mysqld
然后，直接命令行执行 mysqld --skip-grant-tables &就可以了。
不用改配置。
改配置要改过去，还要改回来。

2 楼 [jinnianshilongnian](http://jinnianshilongnian.iteye.com/ "jinnianshilongnian") 2012-08-09  

![]() ，哈哈哈
我转载了啊![]()
1 楼 [smiky](http://smiky.iteye.com/ "smiky") 2012-08-09  

其实最简单的办法是找一个安装好了的mysql,其对应安装目录下的系统数据库内的文件全部copy到有问题的mysql下就OK了
### 发表评论

[![]()](http://snowolf.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://snowolf.iteye.com/login)

[![snowolf的博客]( "snowolf的博客: Snowolf的意境空间！")](http://snowolf.iteye.com/)

snowolf

* 浏览: 902575 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 北京
* ![]()
### 最近访客 [更多访客>>](http://snowolf.iteye.com/blog/user_visits)

[![D_e_人的博客]( "D_e_人的博客: 醉爱Java")](http://wdr.iteye.com/)

[D_e_人](http://wdr.iteye.com/ "D_e_人")

[![华个子的博客]( "华个子的博客: ")](http://201308215603.iteye.com/)

[华个子](http://201308215603.iteye.com/ "华个子")
[![torry_he的博客]( "torry_he的博客: ")](http://torry-he.iteye.com/)

[torry_he](http://torry-he.iteye.com/ "torry_he")

[![cxzucc的博客]( "cxzucc的博客: ")](http://cxzucc.iteye.com/)

[cxzucc](http://cxzucc.iteye.com/ "cxzucc")

### 文章分类

* [全部博客 (161)](http://snowolf.iteye.com/)
* [职场 && 心情 (22)](http://snowolf.iteye.com/category/44718)
* [Java／Basic (17)](http://snowolf.iteye.com/category/28520)
* [Java／Compression (7)](http://snowolf.iteye.com/category/102898)
* [Java／Security (22)](http://snowolf.iteye.com/category/68576)
* [Java／Maven (3)](http://snowolf.iteye.com/category/147023)
* [Java／Cache (11)](http://snowolf.iteye.com/category/214766)
* [Eclipse (4)](http://snowolf.iteye.com/category/38039)
* [Spring (19)](http://snowolf.iteye.com/category/28518)
* [ORM／Hibernate (2)](http://snowolf.iteye.com/category/39212)
* [ORM／iBatis (3)](http://snowolf.iteye.com/category/36643)
* [DB／NoSQL (10)](http://snowolf.iteye.com/category/28851)
* [DB／MySQL (7)](http://snowolf.iteye.com/category/34962)
* [DB／MS SQL Server (4)](http://snowolf.iteye.com/category/93301)
* [OS／Linux (10)](http://snowolf.iteye.com/category/64032)
* [OS／Mac (7)](http://snowolf.iteye.com/category/124565)
* [C／C++ (4)](http://snowolf.iteye.com/category/67920)
* [Server Architecture／Basic (11)](http://snowolf.iteye.com/category/48004)
* [Server Architecture／Distributed (17)](http://snowolf.iteye.com/category/64267)
* [Moblie／Andriod (2)](http://snowolf.iteye.com/category/51869)
* [WebService (3)](http://snowolf.iteye.com/category/64109)
* [Objective-C (1)](http://snowolf.iteye.com/category/195003)
* [Html (1)](http://snowolf.iteye.com/category/28519)
* [设计模式 (1)](http://snowolf.iteye.com/category/68028)
* [Scala (0)](http://snowolf.iteye.com/category/271868)
* [Kafka (1)](http://snowolf.iteye.com/category/274875)
### 社区版块

* [我的资讯](http://snowolf.iteye.com/blog/news) (0)
* [我的论坛](http://snowolf.iteye.com/blog/post) (66)
* [我的问答](http://snowolf.iteye.com/blog/answered_problems) (4)

### 存档分类

* [2013-05](http://snowolf.iteye.com/blog/monthblog/2013-05) (2)
* [2013-04](http://snowolf.iteye.com/blog/monthblog/2013-04) (1)
* [2013-03](http://snowolf.iteye.com/blog/monthblog/2013-03) (3)
* [更多存档...](http://snowolf.iteye.com/blog/monthblog_more)
### 评论排行榜

* [征服 Redis + Jedis + Spring （二）—— ...](http://snowolf.iteye.com/blog/1667104 "征服 Redis + Jedis + Spring （二）—— 哈希表操作（HMGET HMSET）")
* [MySQL 查询时强制区分大小写](http://snowolf.iteye.com/blog/1681944 "MySQL 查询时强制区分大小写")
* [MySQL 运维笔记（一）—— 终止高负载SQL](http://snowolf.iteye.com/blog/1679923 "MySQL 运维笔记（一）—— 终止高负载SQL")
* [征服 Redis + Jedis + Spring （一）—— ...](http://snowolf.iteye.com/blog/1666908 "征服 Redis + Jedis + Spring （一）—— 配置&常规操作（GET SET DEL）")
* [我的职场生涯（十）——又一个两年](http://snowolf.iteye.com/blog/1748094 "我的职场生涯（十）——又一个两年")

### 最新评论

* [dbh0512](http://dbh0512.iteye.com/ "dbh0512")： 这样操作只是在缓存中读取吗?项目中是不是只有在海量频繁读取某一 ...
[征服 Redis + Jedis + Spring （一）—— 配置&常规操作（GET SET DEL）](http://snowolf.iteye.com/blog/1666908#bc2322585)
* [愤怒的程序猿](http://jacy9.iteye.com/ "愤怒的程序猿")： LZ:keytool错误:java.lang.Exceptio ...
[Java加密技术（九）——初探SSL](http://snowolf.iteye.com/blog/397693#bc2322089)
* [snowolf](http://snowolf.iteye.com/ "snowolf")： juwend 写道如果被加密的明文使用String ming ...
[Java加密技术（四）——非对称加密算法RSA](http://snowolf.iteye.com/blog/381767#bc2321550)
* [simylau](http://simylau.iteye.com/ "simylau")： water_lang 写道@Autowired      pr ...
[Spring 注解学习手札（一） 构建简单Web应用](http://snowolf.iteye.com/blog/577989#bc2320738)
* [ifox](http://ifox.iteye.com/ "ifox")： phrmgb 写道学习了，这些东东有时感觉很偏的，但是特殊的业 ...
[Java关键字——transient](http://snowolf.iteye.com/blog/1329649#bc2319809)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2012 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![](http://stat.iteye.com/?url=http%3A%2F%2Fsnowolf.iteye.com%2Fblog%2F1625674&referrer=http%3A%2F%2Fsnowolf.iteye.com%2F%3Fpage%3D2&user_id=) ![](http://dt.tongji.linezing.com/tongji.do?unit_id=1122445&uv_id=15753577192130187469&uv_new=0&cna=&cg=&mid=&mmland=&ade=&adtm=&sttm=&cpa=&ss_id=3627902245&ss_no=4&ec=1&ref=http%3A//snowolf.iteye.com/%3Fpage%3D2&url=http%3A//snowolf.iteye.com/blog/1625674&title=MySQL %u5FD8%u8BB0root%u5BC6%u7801%uFF0C%u89E3%u51B3%u529E%u6CD5 - Snowolf%u7684%u610F%u5883%u7A7A%u95F4%uFF01 - ITeye%u6280%u672F%u7F51%u7AD9&charset=utf-8&domain=iteye.com&hashval=909&filtered=0&app=Microsoft Internet Explorer&agent=Mozilla/5.0 %28compatible%3B MSIE 9.0%3B Windows NT 6.1%3B WOW64%3B Trident/5.0%3B SLCC2%3B .NET CLR 2.0.50727%3B .NET CLR 3.5.30729%3B .NET CLR 3.0.30729%3B Media Center PC 6.0%3B .NET4.0C%3B .NET4.0E%3B InfoPath.3%29&color=24-bit&screen=1600x900&lg=zh-cn&je=1&fv=10.0&st=1377062586&vc=eee3dea2&ut=2&url_id=0&cnu=0.8579118113412903)[![量子统计]()](http://tongji.alimama.com/report.html?unit_id=1122445)
