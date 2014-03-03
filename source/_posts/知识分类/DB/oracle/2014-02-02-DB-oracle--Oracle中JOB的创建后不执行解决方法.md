---
layout: post
title: "Oracle中JOB的创建后不执行解决方法"
categories: DB
tags: 
 - DB
 - oracle
--- 

# Oracle中JOB的创建后不执行解决方法

**leegstar的个人空间**

[copy]( "复制地址") [Bookmark](http://space.itpub.net/26585184 "加入收藏") http://space.itpub.net/26585184

* [博客](http://space.itpub.net/26585184/spacelist-blog)
* [图片](http://space.itpub.net/26585184/spacelist-image)
* [商品](http://space.itpub.net/26585184/spacelist-goods)
* [下载](http://space.itpub.net/26585184/spacelist-file)
* [收藏](http://space.itpub.net/26585184/spacelist-link)
* [影音](http://space.itpub.net/26585184/spacelist-video)
* [圈子](http://space.itpub.net/26585184/spacelist-group)
* [好友](http://space.itpub.net/26585184/spacelist-friend)
* [论坛](http://space.itpub.net/26585184/spacelist-bbs)
* [留言](http://space.itpub.net/26585184/action-viewpro)

[空间管理](http://space.itpub.net/batch.manage.php?uid=26585184) 您的位置: [ITPUB个人空间](http://space.itpub.net/) » [leegstar的个人空间](http://space.itpub.net/26585184/) » [日志](http://space.itpub.net/26585184/spacelist-blog)

# Oracle中JOB的创建后不执行解决方法

[上一篇](http://space.itpub.net/batch.common.php?action=viewspace&op=up&itemid=767960&uid=26585184) / [下一篇](http://space.itpub.net/batch.common.php?action=viewspace&op=next&itemid=767960&uid=26585184)  2013-08-07 15:13:48 / 个人分类：[oracle](http://space.itpub.net/26585184/spacelist-blog-itemtypeid-89966)
[查看( 174 )]() / [评论( 1 )]() / [评分( 5 / 0 )]()

在[****](http://space.itpub.net/519536/viewspace-626310/)**[**Oracle**]()**中可以使用[**JOB**](http://space.itpub.net/519536/viewspace-626310/)来实现一些任务的自动化执行，类似于UNIX操作系统crontab命令的功能。
简单演示一下，供参考。
1.创建表T，包含一个X字段，定义为日期类型，方便后面的定时任务[**测试**](http://space.itpub.net/519536/viewspace-626310/)。
sec@ora10g> create [**table**]() t (x date);
Table created.
2.创建存储过程p_insert_into_t，每次执行该存储过程都会向T表中插入一条系统当前时间。
sec@ora10g> create or replace procedure p_insert_into_t
  2  as
  3  begin
  4     insert into t
  5     values (SYSDATE);
  6  end;
  7  /
Procedure created.
3.OK，准备就绪，我们来[**创建**](http://space.itpub.net/519536/viewspace-626310/)一个JOB，这个JOB会每分钟运行一次？需要注意一个[**细节**](http://space.itpub.net/519536/viewspace-626310/)！
sec@ora10g> variable job_number number;
sec@ora10g> begin
  2     DBMS_JOB.submit (:job_number,
  3                      'P_INSERT_INTO_T;',
  4                      SYSDATE,
  5                      'sysdate+1/(24*60)');
  6  end;
  7  /
PL/SQL procedure successfully completed.
4.我们通过USER_JOBS视图查看一下创建的JOB信息。
sec@ora10g> select job,
  2         log_user,
  3         to_char(last_date,'yyyy-mm-dd hh24:mi:ss') last_date,
  4         to_char(next_date,'yyyy-mm-dd hh24:mi:ss') next_date,
  5         interval,
  6         what
  7    from user_jobs
  8  /
    JOB LOG_USER LAST_DATE           NEXT_DATE           INTERVAL          WHAT
------- -------- ------------------- ------------------- ----------------- ----------------
     27 SEC                          2010-01-29 00:34:20 sysdate+1/(24*60) P_INSERT_INTO_T;
细节之处在此，此处的LAST_DATE内容是空，表示此JOB没有被执行过，因此这个JOB将永远不会被自动的执行。
这一点可以从T表没有数据来得到验证：
sec@ora10g> select * from t;
no rows selected
那么，如何才能使它自动执行起来呢？
很简单，只要我们手动将这个JOB执行一下即可。
5.手工执行JOB一次，使之按照既定的时间间隔执行。
sec@ora10g> execute dbms_job.run(27);
PL/SQL procedure successfully completed.
此时T表中将会被插入一条具有当前时间的数据。
sec@ora10g> select * from t;
X
-------------------
2010-01-29 00:37:42
再次查看JOB的信息
sec@ora10g> select job,
  2         log_user,
  3         to_char(last_date,'yyyy-mm-dd hh24:mi:ss') last_date,
  4         to_char(next_date,'yyyy-mm-dd hh24:mi:ss') next_date,
  5         interval,
  6         what
  7    from user_jobs
  8  /
    JOB LOG_USER LAST_DATE           NEXT_DATE           INTERVAL          WHAT
------- -------- ------------------- ------------------- ----------------- ----------------
     27 SEC      2010-01-29 00:37:42 2010-01-29 00:38:42 sysdate+1/(24*60) P_INSERT_INTO_T;
此时LAST_DATE显示了我们执行JOB的时间，同时NEXT_DATE显示了下次JOB将被执行的时间。此后这个JOB将会每隔一分钟被执行一次。
自动执行一段时间后的T表内容如下：
sec@ora10g> select * from t order by x;
X
-------------------
2010-01-29 00:37:42
2010-01-29 00:38:46
2010-01-29 00:39:46
2010-01-29 00:40:46
2010-01-29 00:41:46
2010-01-29 00:42:46
2010-01-29 00:43:46
2010-01-29 00:44:46
2010-01-29 00:45:46
2010-01-29 00:46:46
2010-01-29 00:47:46
2010-01-29 00:48:46
2010-01-29 00:49:46
2010-01-29 00:50:46
2010-01-29 00:51:46
2010-01-29 00:52:46
16 rows selected.
6.为什么刚刚创建后的JOB不能自动的执行呢？
这是一个疏忽导致的！
在创建JOB的时候，需要在结尾处指定“COMMIT;”！表示创建完成之后便执行一次。
删除之前的JOB，重新创建一个带有“COMMIT”语句的新JOB。
sec@ora10g> variable job_number number;
sec@ora10g> begin
  2     DBMS_JOB.submit (:job_number,
  3                      'P_INSERT_INTO_T;',
  4                      SYSDATE,
  5                      'sysdate+1/(24*60)');
  6     commit;
  7  end;
  8  /
sec@ora10g> print job_number;
JOB_NUMBER
----------
        29
此次创建的JOB信息如下，可见LAST_DATE在创建完之后便有内容，表示已经被执行了一次。
sec@ora10g> select job,
  2         log_user,
  3         to_char(last_date,'yyyy-mm-dd hh24:mi:ss') last_date,
  4         to_char(next_date,'yyyy-mm-dd hh24:mi:ss') next_date,
  5         interval,
  6         what
  7    from user_jobs
  8  /
    JOB LOG_USER LAST_DATE           NEXT_DATE           INTERVAL          WHAT
------- -------- ------------------- ------------------- ----------------- ----------------
     29 SEC      2010-01-29 01:02:11 2010-01-29 01:03:11 sysdate+1/(24*60) P_INSERT_INTO_T;
一分钟过后便可看到T表中已有两条记录。
sec@ora10g> select * from t;
X
-------------------
2010-01-29 01:02:11
2010-01-29 01:03:11
7.删除JOB方法
很简单，使用“dbms_job.remove”即可。
sec@ora10g> execute dbms_job.remove(29);
PL/SQL procedure successfully completed.
8.最后，谈一下创建JOB时用到的参数。
1）使用desc命令查看DBMS_JOB，可以得到SUBMIT这个存储过程的参数列表。
sec@ora10g> desc DBMS_JOB
...
PROCEDURE SUBMIT
 Argument Name                  Type                    In/Out Default?
 ------------------------------ ----------------------- ------ --------
 JOB                            BINARY_INTEGER          OUT
 WHAT                           VARCHAR2                IN
 NEXT_DATE                      DATE                    IN     DEFAULT
 INTERVAL                       VARCHAR2                IN     DEFAULT
 NO_PARSE                       BOOLEAN                 IN     DEFAULT
 INSTANCE                       BINARY_INTEGER          IN     DEFAULT
 FORCE                          BOOLEAN                 IN     DEFAULT
...
2）如果希望对这些参数有更好的理解，可以参考Oracle的官方文档描述，细致而周到。
http://download.oracle.com/docs/cd/B19306_01/appdev.102/b14258/d_job.htm#sthref2936
3）重点关注一下官方文档中关于INTERVAL参数的示例
'sysdate + 7'表示一周执行一次；
'next_day(sysdate,''TUESDAY'')' 表示每周二执行一次；
'null'表示只执行一次。
本文中我使用的是'sysdate+1/(24*60)'表示每分钟执行一次。很形象，一天的二十四分之一是一小时，一小时的六十分之一就是一分钟的意思。
9.小结
通过这个文章和大家分享了一点关于JOB的创建方法和使用，希望对大家有帮助。
细节不容错过！
Good luck.
[****](http://space.itpub.net/519536/viewspace-626310/)**[**secooler**]()**
10.01.28
-- The End --

http://space.itpub.net/519536/viewspace-626310/
### [全部脚印](http://space.itpub.net/batch.track.php?itemid=767960 "查看全部脚印") [不留脚印]( "清除我的脚印") 留下脚印:

* [![51317曾经在2013-8-13访问过该主题]()](http://space.itpub.net/51317/ "51317曾经在2013-8-13访问过该主题")

[51317](http://space.itpub.net/51317/)
* [![baojh曾经在2013-8-13访问过该主题]()](http://space.itpub.net/8615444/ "baojh曾经在2013-8-13访问过该主题")

[baojh](http://space.itpub.net/8615444/)
* [![淡定的DBA曾经在2013-8-13访问过该主题]()](http://space.itpub.net/27144762/ "淡定的DBA曾经在2013-8-13访问过该主题")

[淡定的DBA](http://space.itpub.net/27144762/)
* [![29112063曾经在2013-8-12访问过该主题]()](http://space.itpub.net/29112063/ "29112063曾经在2013-8-12访问过该主题")

[29112063](http://space.itpub.net/29112063/)
* [![hwayw曾经在2013-8-09访问过该主题]()](http://space.itpub.net/25454855/ "hwayw曾经在2013-8-09访问过该主题")

[hwayw](http://space.itpub.net/25454855/)
* [![yhj20041128001曾经在2013-8-08访问过该主题]()](http://space.itpub.net/23757700/ "yhj20041128001曾经在2013-8-08访问过该主题")

[yhj20041128001](http://space.itpub.net/23757700/)
* [![29036651曾经在2013-8-08访问过该主题]()](http://space.itpub.net/29036651/ "29036651曾经在2013-8-08访问过该主题")

[29036651](http://space.itpub.net/29036651/)
* [![ITPUB博客管理员曾经在2013-8-08访问过该主题]()](http://space.itpub.net/12637597/ "ITPUB博客管理员曾经在2013-8-08访问过该主题")

[ITPUB博客管理员](http://space.itpub.net/12637597/)

[导入论坛](http://www.itpub.net/post.php?action=import&itemid=767960) [引用链接]() [收藏]() [分享给好友]() [推荐到圈子]() [管理](http://space.itpub.net/batch.manage.php?itemid=767960) [举报]()

TAG:
![]() [引用]() [删除]() Guest    /   2013-08-08 14:56:40 评 5 分

[查看全部评论]()

 

[-5]() [-3]() [-1]() [-]() [+1]() [+3]() [+5]()

评分：0

我来说两句

显示全部

![:loveliness:]() ![:handshake]() ![:victory:]() ![:funk:]() ![:time:]() ![:kiss:]() ![:call:]() ![:hug:]() ![:lol]() ![:'(]() ![:Q]() ![:L]() ![;P]() ![:$]() ![:P]() ![:o]() ![:@]() ![:D]() ![:(]() ![:)]()

内容

昵称

验证  ![seccode]( "看不清？点击换一个")

提交评论

![leegstar]()

[leegstar](http://space.itpub.net/26585184/action-viewpro-showpro-1)

### 用户菜单

* [给我留言](http://space.itpub.net/26585184/action-viewpro)
* [加入好友]()
* [发短消息](http://www.itpub.net/home.php?mod=spacecp&ac=pm&op=showmsg&handlekey=showmsg_26585184&touid=26585184&pmid=0&daterange=2)
* [我的介绍](http://space.itpub.net/26585184/action-viewpro-showpro-1)
* [论坛资料](http://www.itpub.net/space-uid-26585184.html)
* [空间管理](http://space.itpub.net/batch.manage.php?uid=26585184)
### 我的栏目

* [oracle](http://space.itpub.net/26585184/spacelist-blog-itemtypeid-89966)
* [linux](http://space.itpub.net/26585184/spacelist-blog-itemtypeid-91065)
* [虚拟机](http://space.itpub.net/26585184/spacelist-blog-itemtypeid-90945)
* [PL/SQL](http://space.itpub.net/26585184/spacelist-blog-itemtypeid-90958)
* [mysql](http://space.itpub.net/26585184/spacelist-blog-itemtypeid-91708)
* [Hadoop](http://space.itpub.net/26585184/spacelist-blog-itemtypeid-91998)
* [svn](http://space.itpub.net/26585184/spacelist-blog-itemtypeid-93422)

### 标题搜索
### 日历

[«](http://space.itpub.net/action-spacelist-uid-26585184-type-blog-itemtypeid-89966-starttime-1372608000-endtime-1375286400) [2013-08-13](http://space.itpub.net/action-spacelist-uid-26585184-type-blog-itemtypeid-89966-starttime-1375286400-endtime-1377964800)   日 一 二 三 四 五 六     123456[**7**](http://space.itpub.net/action-spacelist-uid-26585184-type-blog-itemtypeid-89966-starttime-1375833600-endtime-1375920000 " 1 ")8910111213141516171819202122232425262728293031

### 我的存档

* [2013年08月](http://space.itpub.net/26585184/action-spacelist-starttime-1375286400-endtime-1377964800)   [2013年07月](http://space.itpub.net/26585184/action-spacelist-starttime-1372608000-endtime-1375286400)   
* [2013年06月](http://space.itpub.net/26585184/action-spacelist-starttime-1370016000-endtime-1372608000)   [2013年05月](http://space.itpub.net/26585184/action-spacelist-starttime-1367337600-endtime-1370016000)   
* [2013年04月](http://space.itpub.net/26585184/action-spacelist-starttime-1364745600-endtime-1367337600)   [2013年03月](http://space.itpub.net/26585184/action-spacelist-starttime-1362067200-endtime-1364745600)   
* [2013年02月](http://space.itpub.net/26585184/action-spacelist-starttime-1359648000-endtime-1362067200)   [2013年01月](http://space.itpub.net/26585184/action-spacelist-starttime-1356969600-endtime-1359648000)   
* [2012年12月](http://space.itpub.net/26585184/action-spacelist-starttime-1354291200-endtime-1356969600)   [2012年11月](http://space.itpub.net/26585184/action-spacelist-starttime-1351699200-endtime-1354291200)   
* [2012年10月](http://space.itpub.net/26585184/action-spacelist-starttime-1349020800-endtime-1351699200)   [2012年09月](http://space.itpub.net/26585184/action-spacelist-starttime-1346428800-endtime-1349020800)   
* [2012年08月](http://space.itpub.net/26585184/action-spacelist-starttime-1343750400-endtime-1346428800)   [2012年07月](http://space.itpub.net/26585184/action-spacelist-starttime-1341072000-endtime-1343750400)   
* [2012年06月](http://space.itpub.net/26585184/action-spacelist-starttime-1338480000-endtime-1341072000)   [2012年05月](http://space.itpub.net/26585184/action-spacelist-starttime-1335801600-endtime-1338480000)   
* [2012年04月](http://space.itpub.net/26585184/action-spacelist-starttime-1333209600-endtime-1335801600)   [2012年03月](http://space.itpub.net/26585184/action-spacelist-starttime-1330531200-endtime-1333209600)   
* [2012年02月](http://space.itpub.net/26585184/action-spacelist-starttime-1328025600-endtime-1330531200)   [2012年01月](http://space.itpub.net/26585184/action-spacelist-starttime-1325347200-endtime-1328025600)   
* [2011年12月](http://space.itpub.net/26585184/action-spacelist-starttime-1322668800-endtime-1325347200)   
* [查看所有存档](http://space.itpub.net/26585184/action-spacelist-starttime-1320076799)
### 数据统计

* 访问量: 3312
* 日志数: 29
* 建立时间: 2011-12-28
* 更新时间: 2013-08-07

### RSS订阅

* [![RSS订阅]()](http://space.itpub.net/26585184/action-rss-type-blog)

[清空Cookie](http://space.itpub.net/batch.login.php?action=logout) - [联系我们](mailto:admin@yourdomain.com) - [ITPUB个人空间](http://space.itpub.net/) - [交流论坛](http://www.itpub.net/) - [空间列表](http://space.itpub.net/action/spaces) - [站点存档](http://space.itpub.net/archiver/) - [升级自己的空间](http://space.itpub.net/action/register)

Powered by [**X-Space**](http://www.supesite.com/) *3.0.2* © 2001-2007 [Comsenz Inc.](http://www.comsenz.com/)
[京ICP证:010037号](http://www.miibeian.gov.cn/)[网站统计](http://www.51.la/?1711153 "51.la 专业、免费、强健的访问统计") ![](http://web2.51.la:82/go2.asp?svid=5&id=1711153&tpages=3&ttimes=1&tzone=8&tcolor=32&sSize=1600,900&referrer=http%3A//www.itpub.net/&vpage=http%3A//space.itpub.net/26585184/viewspace-767960)<a href="/1711153" target="_blank"><img alt="我要啦免费统计" src="http://img.users.51.la/1711153.asp" style="border:none" /></a>
[Open Toolbar]()   <img src="http://it168.wrating.com/a.gif?a=&c=860010-2083110100" width="1" height="1"/>
