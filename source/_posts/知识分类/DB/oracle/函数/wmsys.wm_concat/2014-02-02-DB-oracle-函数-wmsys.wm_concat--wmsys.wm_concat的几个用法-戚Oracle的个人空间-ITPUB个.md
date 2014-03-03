---
layout: post
title: "wmsys.wm_concat的几个用法 -   戚  Oracle  的个人空间 - ITPUB个"
categories: DB
tags: 
 - DB
 - oracle
 - 函数
 - wmsys.wm_concat
--- 

# wmsys.wm_concat的几个用法 - 戚 Oracle 的个人空间 - ITPUB个人空间 - powered by X-Space

****戚**Oracle**的个人空间**

[copy]( "复制地址") [Bookmark](http://space.itpub.net/13387766 "加入收藏") http://space.itpub.net/13387766

* [博客](http://space.itpub.net/13387766/spacelist-blog)
* [图片](http://space.itpub.net/13387766/spacelist-image)
* [商品](http://space.itpub.net/13387766/spacelist-goods)
* [下载](http://space.itpub.net/13387766/spacelist-file)
* [收藏](http://space.itpub.net/13387766/spacelist-link)
* [影音](http://space.itpub.net/13387766/spacelist-video)
* [圈子](http://space.itpub.net/13387766/spacelist-group)
* [好友](http://space.itpub.net/13387766/spacelist-friend)
* [论坛](http://space.itpub.net/13387766/spacelist-bbs)
* [留言](http://space.itpub.net/13387766/action-viewpro)

[空间管理](http://space.itpub.net/batch.manage.php?uid=13387766) 您的位置: [ITPUB个人空间](http://space.itpub.net/) » [**戚**Oracle**的个人空间](http://space.itpub.net/13387766/) » [日志](http://space.itpub.net/13387766/spacelist-blog)

快乐地学习ORACLE,享受oracle里面的乐趣!
# wmsys.wm_concat的几个用法

[上一篇](http://space.itpub.net/batch.common.php?action=viewspace&op=up&itemid=448841&uid=13387766) / [下一篇](http://space.itpub.net/batch.common.php?action=viewspace&op=next&itemid=448841&uid=13387766)  2008-09-18 14:02:52 / 个人分类：[笔记](http://space.itpub.net/13387766/spacelist-blog-itemtypeid-72758)
[查看( 2049 )](http://space.itpub.net/13387766/viewspace-448841#xspace-tracks) / [评论( 7 )](http://space.itpub.net/13387766/viewspace-448841#xspace-itemreply) / [评分( 17 / 0 )](http://space.itpub.net/13387766/viewspace-448841#xspace-itemform)

今天才发现了wmsys.wm_concat这个有趣有用的函数，它的作用是以','链接字符。

例子如下：

SQL> create table idtable (id number,name varchar2(30));

Table created

SQL> insert into idtable values(10,'ab');

1 row inserted

SQL> insert into idtable values(10,'bc');

1 row inserted

SQL> insert into idtable values(10,'cd');

1 row inserted

SQL> insert into idtable values(20,'hi');

1 row inserted

SQL> insert into idtable values(20,'ij');

1 row inserted
SQL> insert into idtable values(20,'mn');

1 row inserted

SQL> select * from idtable;

        ID NAME
---------- ------------------------------
        10 ab
        10 bc
        10 cd
        20 hi
        20 ij
        20 mn

6 rows selected
SQL> select id,wmsys.wm_concat(name) name from idtable
  2  group by id;

        ID NAME
---------- --------------------------------------------------------------------------------
        10 ab,bc,cd
        20 hi,ij,mn

SQL> select id,wmsys.wm_concat(name) over (order by id) name from idtable;

        ID NAME
---------- --------------------------------------------------------------------------------
        10 ab,bc,cd
        10 ab,bc,cd
        10 ab,bc,cd
        20 ab,bc,cd,hi,ij,mn
        20 ab,bc,cd,hi,ij,mn
        20 ab,bc,cd,hi,ij,mn

6 rows selected

SQL> select id,wmsys.wm_concat(name) over (order by id,name) name from idtable;

        ID NAME
---------- --------------------------------------------------------------------------------
        10 ab
        10 ab,bc
        10 ab,bc,cd
        20 ab,bc,cd,hi
        20 ab,bc,cd,hi,ij
        20 ab,bc,cd,hi,ij,mn

6 rows selected

个人觉得这个用法比较有趣.

SQL> select id,wmsys.wm_concat(name) over (partition by id) name from idtable;

        ID NAME
---------- --------------------------------------------------------------------------------
        10 ab,bc,cd
        10 ab,bc,cd
        10 ab,bc,cd
        20 hi,ij,mn
        20 hi,ij,mn
        20 hi,ij,mn

6 rows selected

SQL> select id,wmsys.wm_concat(name) over (partition by id,name) name from idtable;

        ID NAME
---------- --------------------------------------------------------------------------------
        10 ab
        10 bc
        10 cd
        20 hi
        20 ij
        20 mn

6 rows selected

[导入论坛](http://www.itpub.net/post.php?action=import&itemid=448841) [引用链接]() [收藏]() [分享给好友]() [推荐到圈子]() [管理](http://space.itpub.net/batch.manage.php?itemid=448841) [举报]()

TAG:
![]() [引用]() [删除]() Guest    /   2011-08-25 17:26:42 评 5 分 ![]() [引用]() [删除]() Guest    /   2010-12-15 12:56:42 评 5 分 ![]() [引用]() [删除]() Guest    /   2009-09-14 16:02:08 评 3 分 ![]() [引用]() [删除]() Guest    /   2009-09-11 15:35:56 评 3 分 ![]() [引用]() [删除]() Guest    /   2009-09-11 09:52:35 学习 [![e_soft的个人空间]()](http://space.itpub.net/21220558/) [引用]() [删除]() [e_soft](http://space.itpub.net/21220558/)    /   2009-04-11 22:02:45 评 1 分 [![]()](http://space.itpub.net/10054159/) [引用]() [删除]() [liukaiming](http://space.itpub.net/10054159/)    /   2008-11-28 09:25:49 有意思，学习了。

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

![qgw521]()

[qgw521](http://space.itpub.net/13387766/action-viewpro-showpro-1)

### 用户菜单

* [给我留言](http://space.itpub.net/13387766/action-viewpro)
* [加入好友]()
* [发短消息](http://www.itpub.net/home.php?mod=spacecp&ac=pm&op=showmsg&handlekey=showmsg_13387766&touid=13387766&pmid=0&daterange=2)
* [我的介绍](http://space.itpub.net/13387766/action-viewpro-showpro-1)
* [论坛资料](http://www.itpub.net/space-uid-13387766.html)
* [空间管理](http://space.itpub.net/batch.manage.php?uid=13387766)
### 我的栏目

* [sql](http://space.itpub.net/13387766/spacelist-blog-itemtypeid-72569)
* [oracle管理](http://space.itpub.net/13387766/spacelist-blog-itemtypeid-72630)
* [转贴](http://space.itpub.net/13387766/spacelist-blog-itemtypeid-72688)
* [笔记](http://space.itpub.net/13387766/spacelist-blog-itemtypeid-72758)
* [备份与恢复](http://space.itpub.net/13387766/spacelist-blog-itemtypeid-72944)
* [生活](http://space.itpub.net/13387766/spacelist-blog-itemtypeid-73128)
* [EPR二次开发](http://space.itpub.net/13387766/spacelist-blog-itemtypeid-73612)
* [Oracle错误](http://space.itpub.net/13387766/spacelist-blog-itemtypeid-73636)
* [感悟](http://space.itpub.net/13387766/spacelist-blog-itemtypeid-73923)
* [工作](http://space.itpub.net/13387766/spacelist-blog-itemtypeid-74417)
* [测试](http://space.itpub.net/13387766/spacelist-blog-itemtypeid-78356)

### 标题搜索
### 日历

[«](http://space.itpub.net/action-spacelist-uid-13387766-type-blog-itemtypeid-72758-starttime-1322668800-endtime-1325347200) [2012-01-09](http://space.itpub.net/action-spacelist-uid-13387766-type-blog-itemtypeid-72758-starttime-1325347200-endtime-1328025600)   日 一 二 三 四 五 六 12345678910111213141516171819202122232425262728293031    

### 我的存档

* [2010年04月](http://space.itpub.net/13387766/action-spacelist-starttime-1270051200-endtime-1272643200)   [2010年03月](http://space.itpub.net/13387766/action-spacelist-starttime-1267372800-endtime-1270051200)   
* [2010年02月](http://space.itpub.net/13387766/action-spacelist-starttime-1264953600-endtime-1267372800)   [2010年01月](http://space.itpub.net/13387766/action-spacelist-starttime-1262275200-endtime-1264953600)   
* [2009年12月](http://space.itpub.net/13387766/action-spacelist-starttime-1259596800-endtime-1262275200)   [2009年11月](http://space.itpub.net/13387766/action-spacelist-starttime-1257004800-endtime-1259596800)   
* [2009年10月](http://space.itpub.net/13387766/action-spacelist-starttime-1254326400-endtime-1257004800)   [2009年09月](http://space.itpub.net/13387766/action-spacelist-starttime-1251734400-endtime-1254326400)   
* [2009年08月](http://space.itpub.net/13387766/action-spacelist-starttime-1249056000-endtime-1251734400)   [2009年07月](http://space.itpub.net/13387766/action-spacelist-starttime-1246377600-endtime-1249056000)   
* [2009年06月](http://space.itpub.net/13387766/action-spacelist-starttime-1243785600-endtime-1246377600)   [2009年05月](http://space.itpub.net/13387766/action-spacelist-starttime-1241107200-endtime-1243785600)   
* [2009年04月](http://space.itpub.net/13387766/action-spacelist-starttime-1238515200-endtime-1241107200)   [2009年03月](http://space.itpub.net/13387766/action-spacelist-starttime-1235836800-endtime-1238515200)   
* [2009年02月](http://space.itpub.net/13387766/action-spacelist-starttime-1233417600-endtime-1235836800)   [2009年01月](http://space.itpub.net/13387766/action-spacelist-starttime-1230739200-endtime-1233417600)   
* [2008年12月](http://space.itpub.net/13387766/action-spacelist-starttime-1228060800-endtime-1230739200)   [2008年11月](http://space.itpub.net/13387766/action-spacelist-starttime-1225468800-endtime-1228060800)   
* [2008年10月](http://space.itpub.net/13387766/action-spacelist-starttime-1222790400-endtime-1225468800)   [2008年09月](http://space.itpub.net/13387766/action-spacelist-starttime-1220198400-endtime-1222790400)   
* [2008年08月](http://space.itpub.net/13387766/action-spacelist-starttime-1217520000-endtime-1220198400)   [2008年07月](http://space.itpub.net/13387766/action-spacelist-starttime-1214841600-endtime-1217520000)   
* [2008年06月](http://space.itpub.net/13387766/action-spacelist-starttime-1212249600-endtime-1214841600)   
* [查看所有存档](http://space.itpub.net/13387766/action-spacelist-starttime-1209571199)
### 数据统计

* 访问量: 22466
* 日志数: 114
* 建立时间: 2008-06-06
* 更新时间: 2010-04-27

### RSS订阅

* [![RSS订阅]()](http://space.itpub.net/13387766/action-rss-type-blog)

[清空Cookie](http://space.itpub.net/batch.login.php?action=logout) - [联系我们](mailto:admin@yourdomain.com) - [ITPUB个人空间](http://space.itpub.net/) - [交流论坛](http://www.itpub.net/) - [空间列表](http://space.itpub.net/action/spaces) - [站点存档](http://space.itpub.net/archiver/) - [升级自己的空间](http://space.itpub.net/action/register)

Powered by [**X-Space**](http://www.supesite.com/) *3.0.2* © 2001-2007 [Comsenz Inc.](http://www.comsenz.com/)
[京ICP证:010037号](http://www.miibeian.gov.cn/)[网站统计](http://www.51.la/?1711153 "51.la 专业、免费、强健的访问统计") ![]()<a href="/1711153" target="_blank"><img alt="我要啦免费统计" src="http://img.users.51.la/1711153.asp" style="border:none" /></a>![](http://log.tigerbar.net/itpubspace.gif)
[Open Toolbar]()

![]()
<img src="http://it168.wrating.com/a.gif?a=&c=860010-2083110100" width="1" height="1"/>
