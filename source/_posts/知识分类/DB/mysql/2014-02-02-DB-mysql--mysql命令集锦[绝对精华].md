---
layout: post
title: "mysql命令集锦[绝对精华]"
categories: DB
tags: 
 - DB
 - mysql
--- 

# mysql命令集锦[绝对精华]

[首页](http://www.javaeye.com/) [新闻](http://www.javaeye.com/news) [论坛](http://www.javaeye.com/forums) [博客](http://www.javaeye.com/blogs) [招聘](http://www.javaeye.com/job) [更多 ▼](http://jiajun.javaeye.com/blog/411079#)

[问答](http://www.javaeye.com/ask) [知识库](http://www.javaeye.com/wiki) [圈子](http://www.javaeye.com/groups) [搜索](http://www.javaeye.com/google_search)

[您还未登录 !](http://jiajun.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://jiajun.javaeye.com/login) [注册](http://jiajun.javaeye.com/signup)

# [加俊](http://jiajun.javaeye.com/)

永久域名 [http://jiajun.javaeye.com/](http://jiajun.javaeye.com/)

### [40顶](http://jiajun.javaeye.com/blog/411079#)
[0踩](http://jiajun.javaeye.com/blog/411079#)

[“懒惰” Linux 管理员的 10 个关键技巧](http://jiajun.javaeye.com/blog/411369 "“懒惰” Linux 管理员的 10 个关键技巧") | [我的googleAdSense和阿里妈妈](http://jiajun.javaeye.com/blog/407654 "我的googleAdSense和阿里妈妈")

2009-06-18

### [mysql命令集锦[绝对精华]](http://jiajun.javaeye.com/blog/411079)

**关键字: mysql命令集锦**
整理By：leo,感谢leo

 

测试环境：mysql 5.0.45
【注：可以在mysql中通过mysql> SELECT VERSION();来查看数据库版本】

 

 

 

**一、连接MYSQL。**

格式： mysql -h主机地址 -u用户名 －p用户密码

1、连接到本机上的MYSQL。

首先打开DOS窗口，然后进入目录mysql\bin，再键入命令mysql -u root -p，回车后提示你输密码.注意用户名前可以有空格也可以没有空格，但是密码前必须没有空格，否则让你重新输入密码.

如果刚安装好MYSQL，超级用户root是没有密码的，故直接回车即可进入到MYSQL中了，MYSQL的提示符是： mysql>

2、连接到远程主机上的MYSQL。假设远程主机的IP为：110.110.110.110，用户名为root,密码为abcd123。则键入以下命令：

mysql -h110.110.110.110 -u root -p 123;（注:u与root之间可以不用加空格，其它也一样）

3、退出MYSQL命令： exit （回车）

 

**二、修改密码。**

格式：mysqladmin -u用户名 -p旧密码 password 新密码

1、给root加个密码ab12。首先在DOS下进入目录mysql\bin，然后键入以下命令

mysqladmin -u root -password ab12

注：因为开始时root没有密码，所以-p旧密码一项就可以省略了。
2、再将root的密码改为djg345。

mysqladmin -u root -p ab12 password djg345

 

**三、增加新用户。**
（注意：和上面不同，下面的因为是MYSQL环境中的命令，所以后面都带一个分号作为命令结束符）

格式：grant select on 数据库.* to 用户名@登录主机 identified by “密码”

1、增加一个用户test1密码为abc，让他可以在任何主机上登录，并对所有数据库有查询、插入、修改、删除的权限。首先用root用户连入MYSQL，然后键入以下命令：

grant select,insert,update,delete on *.* to test1@”%” Identified by “abc”;

但增加的用户是十分危险的，你想如某个人知道test1的密码，那么他就可以在internet上的任何一台电脑上登录你的mysql数据库并对你的数据可以为所欲为了，解决办法见2。

2、增加一个用户test2密码为abc,让他只可以在localhost上登录，并可以对数据库mydb进行查询、插入、修改、删除的操作（localhost指本地主机，即MYSQL数据库所在的那台主机），

这样用户即使用知道test2的密码，他也无法从internet上直接访问数据库，只能通过MYSQL主机上的web页来访问了。

grant select,insert,update,delete on mydb.* to test2@localhost identified by “abc”;

如果你不想test2有密码，可以再打一个命令将密码消掉。

grant select,insert,update,delete on mydb.* to test2@localhost identified by “”;

下篇我是MYSQL中有关数据库方面的操作。注意：你必须首先登录到MYSQL中，以下操作都是在MYSQL的提示符下进行的，而且每个命令以分号结束。

一、操作技巧

1、如果你打命令时，回车后发现忘记加分号，你无须重打一遍命令，只要打个分号回车就可以了。

也就是说你可以把一个完整的命令分成几行来打，完后用分号作结束标志就OK。

2、你可以使用光标上下键调出以前的命令。

二、显示命令

1、显示当前数据库服务器中的数据库列表：

mysql> SHOW DATABASES;

注意：mysql库里面有MYSQL的系统信息，我们改密码和新增用户，实际上就是用这个库进行操作。

2、显示数据库中的数据表：

mysql> USE 库名；
mysql> SHOW TABLES;

3、显示数据表的结构：

mysql> DESCRIBE 表名;

4、建立数据库：

mysql> CREATE DATABASE 库名;

5、建立数据表：

mysql> USE 库名;
mysql> CREATE TABLE 表名 (字段名 VARCHAR(20), 字段名 CHAR(1));

6、删除数据库：

mysql> DROP DATABASE 库名;

7、删除数据表：

mysql> DROP TABLE 表名；

8、将表中记录清空：

mysql> DELETE FROM 表名;

9、显示表中的记录：

mysql> SELECT * FROM 表名;

10、往表中插入记录：

mysql> INSERT INTO 表名 VALUES (”hyq”,”M”);

11、更新表中数据：

mysql-> UPDATE 表名 SET 字段名1=’a',字段名2=’b’ WHERE 字段名3=’c';

12、用文本方式将数据装入数据表中：

mysql> LOAD DATA LOCAL INFILE “D:/mysql.txt” INTO TABLE 表名;

13、导入.sql文件命令：

mysql> USE 数据库名;
mysql> SOURCE d:/mysql.sql;

14、命令行修改root密码：

mysql> UPDATE mysql.user SET password=PASSWORD(’新密码’) WHERE User=’root’;
mysql> FLUSH PRIVILEGES;

15、显示use的数据库名：

mysql> SELECT DATABASE();

16、显示当前的user：

mysql> SELECT USER();

三、一个建库和建表以及插入数据的实例

drop database if exists school; //如果存在SCHOOL则删除

create database school; //建立库SCHOOL

use school; //打开库SCHOOL

create table teacher //建立表TEACHER
(
id int(3) auto_increment not null primary key,
name char(10) not null,
address varchar(50) default ‘深圳’,
year date
); //建表结束

//以下为插入字段
insert into teacher values(”,’allen’,'大连一中’,'1976-10-10′);
insert into teacher values(”,’jack’,'大连二中’,'1975-12-23′);

如果你在mysql提示符键入上面的命令也可以，但不方便调试。

（1）你可以将以上命令原样写入一个文本文件中，假设为school.sql，然后复制到c:\\下，并在DOS状态进入目录\\mysql\\bin，然后键入以下命令：

mysql -uroot -p密码 < c:\\school.sql

如果成功，空出一行无任何显示；如有错误，会有提示。（以上命令已经调试，你只要将//的注释去掉即可使用）。

（2）或者进入命令行后使用 mysql> source c:\\school.sql; 也可以将school.sql文件导入数据库中。

**四、将文本数据转到数据库中**

1、文本数据应符合的格式：字段数据之间用tab键隔开，null值用\\n来代替.例：

3 rose 大连二中 1976-10-10

4 mike 大连一中 1975-12-23

假设你把这两组数据存为school.txt文件，放在c盘根目录下。

2、数据传入命令 load data local infile “c:\\school.txt” into table 表名;

注意：你最好将文件复制到\\mysql\\bin目录下，并且要先用use命令打表所在的库。

 

**五、备份数据库：（命令在DOS的\\mysql\\bin目录下执行）**

1.导出整个数据库

导出文件默认是存在mysql\bin目录下

mysqldump -u 用户名 -p 数据库名 > 导出的文件名

mysqldump -u user_name -p123456 database_name > outfile_name.sql

2.导出一个表

mysqldump -u 用户名 -p 数据库名 表名> 导出的文件名

mysqldump -u user_name -p database_name table_name > outfile_name.sql

3.导出一个数据库结构

mysqldump -u user_name -p -d –add-drop-table database_name > outfile_name.sql

-d 没有数据 –add-drop-table 在每个create语句之前增加一个drop table

4.带语言参数导出

mysqldump -uroot -p –default-character-set=latin1 –set-charset=gbk –skip-opt database_name > outfile_name.sql

[**40**
顶](http://jiajun.javaeye.com/blog/411079#)[**0**
踩](http://jiajun.javaeye.com/blog/411079#)
* 18:29
* 浏览 (4031)
* [评论](http://jiajun.javaeye.com/blog/411079#comments) (8)
* 分类: [mySQL](http://jiajun.javaeye.com/category/47548)
* [相关推荐](http://www.javaeye.com/wiki/topic/411079)

### 评论

[]()

8 楼 [jordan421](http://jordan421.javaeye.com/) 2009-06-26   [引用](http://jiajun.javaeye.com/blog/411079#)

学习了~~~~~
7 楼 [lseeo](http://lseeo.javaeye.com/) 2009-06-23   [引用](http://jiajun.javaeye.com/blog/411079#)

thanks!

6 楼 [MyDicta](http://mydicta.javaeye.com/) 2009-06-22   [引用](http://jiajun.javaeye.com/blog/411079#)

嗯   ~~  楼主辛苦了。。。
  讲的很浅了， 不对我相信这与文章篇幅有关， 呵呵~~
  不过文章对于初学者到是一个不错的学习机会，收藏了，防止以后忘记。。。
   lz的SQL约束不规范哦，没涉及到储存过程，触发器，索引，高级查询。。。。。这方面的内容
5 楼 [flyfan](http://flyfan.javaeye.com/) 2009-06-20   [引用](http://jiajun.javaeye.com/blog/411079#)

谢谢分享，好东西，收藏之

4 楼 [sirxenofex](http://sirxenofex.javaeye.com/) 2009-06-20   [引用](http://jiajun.javaeye.com/blog/411079#)

grant select,insert,update,delete on *.* to test1@”%” Identified by “abc”;
这一句在我经验好像是达不到预期效果的。MySQL默认有个空用户名的账户，并且在本级与在任意地址，都是没有任何权限的。好像必须删除这条记录，才可以实现@"%"的权限
3 楼 [lucky16](http://lucky16.javaeye.com/) 2009-06-20   [引用](http://jiajun.javaeye.com/blog/411079#)

确实很详细哦普1!!
呵呵 
代表新手感谢你啊!

2 楼 [rongxh7](http://rongxh7.javaeye.com/) 2009-06-19   [引用](http://jiajun.javaeye.com/blog/411079#)

辛苦了,写得很详细的.
1 楼 [whaosoft](http://whaosoft.javaeye.com/) 2009-06-19   [引用](http://jiajun.javaeye.com/blog/411079#)

![]() lz辛苦了
### 发表评论

您还没有登录，请[登录](http://jiajun.javaeye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![iammonster的博客]( "iammonster的博客: 加俊")](http://jiajun.javaeye.com/)

iammonster

 
何去何从
*[2009-09-03](http://jiajun.javaeye.com/blog/chat/42433) 通过网页*

[>>更多闲聊](http://jiajun.javaeye.com/blog/chat)

* 浏览: 59575 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 北京
* ![]()
* [详细资料](http://jiajun.javaeye.com/blog/profile) [留言簿](http://jiajun.javaeye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://jiajun.javaeye.com/blog/user_visits)

[![neilzwb的博客]( "neilzwb的博客: ")](http://neilzwb.javaeye.com/)

[neilzwb](http://neilzwb.javaeye.com/)

[![jacki6的博客]( "jacki6的博客: ")](http://jacki6.javaeye.com/)

[jacki6](http://jacki6.javaeye.com/)
[![天才狐狸的博客]( "天才狐狸的博客: 天才狐狸")](http://geniusfox.javaeye.com/)

[天才狐狸](http://geniusfox.javaeye.com/)

[![rfv1116的博客]( "rfv1116的博客: rfv1116")](http://rfv1116.javaeye.com/)

[rfv1116](http://rfv1116.javaeye.com/)

### 博客分类

* [全部博客 (131)](http://jiajun.javaeye.com/)
* [JAVA (21)](http://jiajun.javaeye.com/category/47481)
* [LINUX (10)](http://jiajun.javaeye.com/category/47484)
* [html (2)](http://jiajun.javaeye.com/category/47543)
* [JavaScript (22)](http://jiajun.javaeye.com/category/47483)
* [Ajax (4)](http://jiajun.javaeye.com/category/47547)
* [CSS (1)](http://jiajun.javaeye.com/category/76555)
* [DB2 (8)](http://jiajun.javaeye.com/category/47482)
* [ORACLE (2)](http://jiajun.javaeye.com/category/47541)
* [SQL SERVER (7)](http://jiajun.javaeye.com/category/47540)
* [mySQL (4)](http://jiajun.javaeye.com/category/47548)
* [Eclipse (8)](http://jiajun.javaeye.com/category/47546)
* [IBATIS (4)](http://jiajun.javaeye.com/category/47542)
* [struts (1)](http://jiajun.javaeye.com/category/47549)
* [apache-tomcat (5)](http://jiajun.javaeye.com/category/47553)
* [lighttpd (3)](http://jiajun.javaeye.com/category/66310)
* [linux-bash (1)](http://jiajun.javaeye.com/category/47554)
* [ant (2)](http://jiajun.javaeye.com/category/48837)
* [Maven (4)](http://jiajun.javaeye.com/category/74493)
* [Architecture (0)](http://jiajun.javaeye.com/category/67603)
* [WebServer (8)](http://jiajun.javaeye.com/category/68325)
* [Android (2)](http://jiajun.javaeye.com/category/74749)
* [游玩 (1)](http://jiajun.javaeye.com/category/66959)
* [生活 (4)](http://jiajun.javaeye.com/category/70898)
* [FAQ (2)](http://jiajun.javaeye.com/category/75801)
### 我的留言簿 [>>更多留言](http://jiajun.javaeye.com/blog/guest_book)

* 出来看上帝
-- by [iammonster](http://jiajun.javaeye.com/blog/guest_book#6723)
* 挖鼻孔
-- by [diegoball](http://jiajun.javaeye.com/blog/guest_book#6713)

### 其他分类

* [我的收藏](http://jiajun.javaeye.com/blog/favorite) (7)
* [我的新闻](http://jiajun.javaeye.com/blog/news) (1)
* [我的论坛帖子](http://jiajun.javaeye.com/blog/forum) (5)
* [我的精华良好贴](http://jiajun.javaeye.com/blog/article) (0)
### 最近加入圈子

* [struts2](http://struts2.group.javaeye.com/)
* [Android](http://android.group.javaeye.com/)
* [代码生成器](http://srcgen.group.javaeye.com/)

### 链接

* [测试](http://www.baobeil.com/)
* [测试解梦](http://jiemeng.baobeil.com/)
* [图管家](http://www.tuguanjia.com/)
### 存档

* [2009-10](http://jiajun.javaeye.com/blog/monthblog/2009-10) (2)
* [2009-09](http://jiajun.javaeye.com/blog/monthblog/2009-09) (12)
* [2009-08](http://jiajun.javaeye.com/blog/monthblog/2009-08) (14)
* [更多存档...](http://jiajun.javaeye.com/blog/monthblog_more)

### 最新评论

* [CentOS5.3安装lighttpd1. ...](http://jiajun.javaeye.com/blog/468831#comments "CentOS5.3安装lighttpd1.4.23全过程")
你现在都使用lighttpd 了, 不用apache了? 
-- by [elf8848](http://elf8848.javaeye.com/)
* [Eclipse is running in a ...](http://jiajun.javaeye.com/blog/458429#comments "Eclipse is running in a JRE, but a JDK is required")
...
-- by [dongwei_6688](http://dongwei.javaeye.com/)
* [软件版本号讲解:什么是Al ...](http://jiajun.javaeye.com/blog/446252#comments "软件版本号讲解:什么是Alpha,Beta,RC,Release")
来看看, 不错, 了解一下
-- by [elf8848](http://elf8848.javaeye.com/)
* [JE的内容不允许爬虫抓取？](http://jiajun.javaeye.com/blog/392419#comments "JE的内容不允许爬虫抓取？")
JE屏蔽的正常的蜘蛛，经过稍微变异的蜘蛛就无能为力了。上次发现一个网站很强悍把我们 ...
-- by [D04540214](http://d04540214.javaeye.com/)
* [Google Android开发第一步 ...](http://jiajun.javaeye.com/blog/440686#comments "Google Android开发第一步HelloWord的诞生")
看起来也不复杂嘛，我也想试试
-- by [lqixv](http://lqixv.javaeye.com/)
### 评论排行榜

* [你是爱剑之人吗？-Eclipse提高工作效率的好 ...](http://jiajun.javaeye.com/blog/386811 "你是爱剑之人吗？-Eclipse提高工作效率的好习惯")
* [如何摆脱JS糟糕的字符串连接,我掉进了陷阱](http://jiajun.javaeye.com/blog/377605 "如何摆脱JS糟糕的字符串连接,我掉进了陷阱")
* [mysql命令集锦[绝对精华]](http://jiajun.javaeye.com/blog/411079 "mysql命令集锦[绝对精华]")
* [像Linux和MacOS一样在Windows下打开多个 ...](http://jiajun.javaeye.com/blog/398602 "像Linux和MacOS一样在Windows下打开多个虚拟桌面")
* [Lighttpd+Squid+Apache搭建高效率Web服务 ...](http://jiajun.javaeye.com/blog/399403 "Lighttpd+Squid+Apache搭建高效率Web服务器")

* [![Rss]()](http://jiajun.javaeye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://jiajun.javaeye.com/rss)
* [![Rss_zhuaxia]()](http://www.zhuaxia.com/add_channel.php?url=http://jiajun.javaeye.com/rss)
* [![Rss_xianguo]()](http://www.xianguo.com/subscribe.php?url=http://jiajun.javaeye.com/rss)
* [[什么是RSS?]](http://www.google.com/search?hl=zh-CN&q=RSS)
声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2009 JavaEye.com. All rights reserved. 上海炯耐计算机软件有限公司 [ 沪ICP备05023328号 ]
