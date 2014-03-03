---
layout: post
title: "oracle 查询占用消耗 CPU 的进程sql"
categories: DB
tags: 
 - DB
 - oracle
 - oracle-
--- 

# oracle 查询占用消耗 CPU 的进程sql

**枫的个人空间**

[copy]( "复制地址") [Bookmark](http://space.itpub.net/24015283 "加入收藏") http://space.itpub.net/24015283

* [博客](http://space.itpub.net/24015283/spacelist-blog)
* [图片](http://space.itpub.net/24015283/spacelist-image)
* [商品](http://space.itpub.net/24015283/spacelist-goods)
* [下载](http://space.itpub.net/24015283/spacelist-file)
* [收藏](http://space.itpub.net/24015283/spacelist-link)
* [影音](http://space.itpub.net/24015283/spacelist-video)
* [圈子](http://space.itpub.net/24015283/spacelist-group)
* [好友](http://space.itpub.net/24015283/spacelist-friend)
* [论坛](http://space.itpub.net/24015283/spacelist-bbs)
* [留言](http://space.itpub.net/24015283/action-viewpro)

[空间管理](http://space.itpub.net/batch.manage.php?uid=24015283) 您的位置: [ITPUB个人空间](http://space.itpub.net/) [枫的个人空间](http://space.itpub.net/24015283/) [日志](http://space.itpub.net/24015283/spacelist-blog)

一叶知秋！
# oracle 查询占用消耗CPU的进程sql

[上一篇](http://space.itpub.net/batch.common.php?action=viewspace&op=up&itemid=709152&uid=24015283) / [下一篇](http://space.itpub.net/batch.common.php?action=viewspace&op=next&itemid=709152&uid=24015283)  2011-10-14 14:47:47
[查看( 220 )](http://space.itpub.net/24015283/viewspace-709152#xspace-tracks) / [评论( 0 )](http://space.itpub.net/24015283/viewspace-709152#xspace-itemreply) / [评分( 0 / 0 )](http://space.itpub.net/24015283/viewspace-709152#xspace-itemform)

1. top查看占用CPU比较高的进程ID，并记录下来

例如：

 PID  USER      PR  NI  VIRT  RES  SHR S  %CPU %MEM    TIME+  COMMAND                                                               
31189 nxuser    15   0 12436  11m  256 S  55.4  0.1  35626:21 oracle                                                             
32902 oracle    16   0 1304m 1.0g 1.0g S     1 12.5  13:07.28 oracle                                                                
 5784 nxuser    16   0  2368 1104  780 S     0  0.0   0:00.02 oracle                                                                   
28779 nxuser    16   0  2364 1140  780 R     0  0.0   0:25.28 top   

Pid 31189占用CPU显然是比较高的

Sqlpusl连接登陆[**ORACLE**]()

2.执行下面的语句，查得相对应的系统进程对应的session id

SQL> select sid from v$session where paddr in (select addr from v$process where spid=&spid);           

Enter value for spid: 31189

old  1: select sid from v$session where paddr in (select addr from v$process where spid=&spid)

new  1: select sid from v$session where paddr in (select addr from v$process where spid=31189)

 

      SID

----------

       206

 

3.根据所得的会话ID查得[**sql**]()地址和hash值

SQL> select sql_address,sql_hash_value from v$session where sid=206;

 

SQL_ADDR SQL_HASH_VALUE

-------- --------------

6EC554F4    3141392848

 

 

4.根据sql hash值查得sql语句

SQL> select sql_text from v$sqltext where hash_value=3141392848;

 

SQL_TEXT

----------------------------------------------------------------

INSERT INTO TEST SELECT * FROM SYS.DBA_OBJECTS

 

5．若没查得相应的sql地址和hash值,请查询job

 

 

[导入论坛](http://www.itpub.net/post.php?action=import&itemid=709152) [引用链接]() [收藏]() [分享给好友]() [推荐到圈子]() [管理](http://space.itpub.net/batch.manage.php?itemid=709152) [举报]()

TAG:
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

![yefeng139]()

[yefeng139](http://space.itpub.net/24015283/action-viewpro-showpro-1)

### 用户菜单

* [给我留言](http://space.itpub.net/24015283/action-viewpro)
* [加入好友]()
* [发短消息](http://www.itpub.net/home.php?mod=spacecp&ac=pm&op=showmsg&handlekey=showmsg_24015283&touid=24015283&pmid=0&daterange=2)
* [我的介绍](http://space.itpub.net/24015283/action-viewpro-showpro-1)
* [论坛资料](http://www.itpub.net/space-uid-24015283.html)
* [空间管理](http://space.itpub.net/batch.manage.php?uid=24015283)
### 标题搜索

### 日历

[](http://space.itpub.net/action-spacelist-uid-24015283-type-blog-starttime-1349020800-endtime-1351699200) [2012-11-07](http://space.itpub.net/action-spacelist-uid-24015283-type-blog-starttime-1351699200-endtime-1354291200)   日 一 二 三 四 五 六     123456789101112131415161718192021222324252627282930 
### 我的存档

* [2012年06月](http://space.itpub.net/24015283/action-spacelist-starttime-1338480000-endtime-1341072000)   [2012年05月](http://space.itpub.net/24015283/action-spacelist-starttime-1335801600-endtime-1338480000)   
* [2012年04月](http://space.itpub.net/24015283/action-spacelist-starttime-1333209600-endtime-1335801600)   [2012年03月](http://space.itpub.net/24015283/action-spacelist-starttime-1330531200-endtime-1333209600)   
* [2012年02月](http://space.itpub.net/24015283/action-spacelist-starttime-1328025600-endtime-1330531200)   [2012年01月](http://space.itpub.net/24015283/action-spacelist-starttime-1325347200-endtime-1328025600)   
* [2011年12月](http://space.itpub.net/24015283/action-spacelist-starttime-1322668800-endtime-1325347200)   [2011年11月](http://space.itpub.net/24015283/action-spacelist-starttime-1320076800-endtime-1322668800)   
* [2011年10月](http://space.itpub.net/24015283/action-spacelist-starttime-1317398400-endtime-1320076800)   [2011年09月](http://space.itpub.net/24015283/action-spacelist-starttime-1314806400-endtime-1317398400)   
* [查看所有存档](http://space.itpub.net/24015283/action-spacelist-starttime-1312127999)

### 数据统计

* 访问量: 521
* 日志数: 8
* 建立时间: 2011-09-05
* 更新时间: 2012-06-15
### RSS订阅

* [![RSS订阅]()](http://space.itpub.net/24015283/action-rss-type-blog)

[清空Cookie](http://space.itpub.net/batch.login.php?action=logout) - [联系我们](mailto:admin@yourdomain.com) - [ITPUB个人空间](http://space.itpub.net/) - [交流论坛](http://www.itpub.net/) - [空间列表](http://space.itpub.net/action/spaces) - [站点存档](http://space.itpub.net/archiver/) - [升级自己的空间](http://space.itpub.net/action/register)

Powered by [**X-Space**](http://www.supesite.com/) *3.0.2* 2001-2007 [Comsenz Inc.](http://www.comsenz.com/)
[京ICP证:010037号](http://www.miibeian.gov.cn/)[网站统计](http://www.51.la/?1711153 "51.la 专业、免费、强健的访问统计") ![]()<a href="/1711153" target="_blank"><img alt="我要啦免费统计" src="http://img.users.51.la/1711153.asp" style="border:none" /></a>![](http://log.tigerbar.net/itpubspace.gif)
[Open Toolbar]()

![]()
<img src="http://it168.wrating.com/a.gif?a=&c=860010-2083110100" width="1" height="1"/>
