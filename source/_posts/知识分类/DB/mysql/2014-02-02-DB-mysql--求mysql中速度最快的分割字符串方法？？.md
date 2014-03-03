---
layout: post
title: "求 mysql中 速度最快的分割字符串方法？？"
categories: DB
tags: 
 - DB
 - mysql
--- 

# 求 mysql中 速度最快的分割字符串方法？？

[![]()](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#)

* [首页](http://www.csdn.net/)|
* [空间](http://hi.csdn.net/)|
* [新闻](http://news.csdn.net/)|
* [论坛](http://bbs.csdn.net/)|
* [博客](http://blog.csdn.net/)|
* [下载](http://download.csdn.net/)|
* [读书](http://book.csdn.net/)|
* [网摘](http://wz.csdn.net/)|
* [视频](http://live.csdn.net/)|
* [书店](http://www.dearbook.com.cn/)|
* [程序员](http://www.programmer.com.cn/)|
* [求职招聘](http://www.itliyu.com/)|
* [项目交易](http://prj.csdn.net/)|
* [培训](http://training.csdn.net/)|
* [网址](http://daohang.csdn.net/)

* 欢迎您：[游客](http://hi.csdn.net/my.html)|[退出](http://forum.csdn.net/User/LoginOut.aspx)|[登录](http://passport.csdn.net/UserLogin.aspx)[注册](http://passport.csdn.net/CSDNUserRegister.aspx)|[帮助](http://community.csdn.net/Help/HelpCenter.htm)  []() []()

[CSDN](http://www.csdn.net/)-[CSDN社区](http://community.csdn.net/)-[其他数据库开发](http://forum.csdn.net/BList/OtherDatabase/)-[MySQL/Postgresql](http://forum.csdn.net/SList/MySQLPostgresql//)
* [管理菜单]()

* [生成帖子](http://forum.csdn.net/PointForum/BuildTopic.aspx?topicId=5d80ce75-305b-420c-95b4-ca1437c6a221&postDate=2009-10-20+15%3a42%3a21&return=)
* [置顶]()
* [推荐]()
* [取消推荐]()
* [锁定]()
* [解锁]()
* [移动]()
* [编辑]()
* [删除]()
* [帖子加分]()
* [结  帖](http://forum.csdn.net/PointForum/Manage/TopicManageView.aspx?forumID=ba09fe7e-2fb7-42d3-805e-578a4a8485e1&topicID=5d80ce75-305b-420c-95b4-ca1437c6a221&date=2009-10-20+15%3a42%3a21&v=13)
* [发  帖](http://forum.csdn.net/PointForum/Forum/PostTopic.aspx?forumID=ba09fe7e-2fb7-42d3-805e-578a4a8485e1)
* [回  复](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#r_achor)

* [共2页](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#)
* *[1](http://topic.csdn.net/u/20091020/15/5d80ce75-305b-420c-95b4-ca1437c6a221.html)*
* *[2](http://topic.csdn.net/u/20091020/15/5d80ce75-305b-420c-95b4-ca1437c6a221_2.html)*

# [收藏]() 不显示删除回复显示所有回复显示星级回复显示得分回复 [推荐] []()求 mysql中 速度最快的分割字符串方法？？*[问题点数:20分]*
* [![]()](http://hi.csdn.net/doney_dongxiang)
* [doney_dongxiang](http://hi.csdn.net/doney_dongxiang)
* (doney)
*
* 等　级：![]()
* 结帖率：93.02%
* *楼主*发表于：2009-10-20 15:42:21   今有 一万个手机号码已 " ;" 分割符连接的字符串，需要在存储过程中，需要将所有号码添加到另外一个表中，并且号码需要分开（既每个手机号码为一行）。有没有快捷的方法？
下面是目前采用的方法：
SQL codeset t_MCODE=ltrim(t_code); /* t_code 为号码字符串 */ set t_MCODE=rtrim(t_MCODE); set t_i=POSITION(';' in t_MCODE); WHILE t_i >=1 do set str=left(t_MCODE,t_i-1); set str=ltrim(str) ; set str=rtrim(str); insert test(names) values(str); set t_MCODE=substring(t_MCODE,t_i+1,length(t_MCODE)-t_i); set t_i=POSITION(';' in t_MCODE); end WHILE ;/*把号码插入表*/ insert test(names) values(t_MCODE); /*将最后剩余的号码放入临时表*/
 
一万条需要 20s 很慢，如果大量，用户并发操作就更加慢了 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)
回复次数：114  * [![wwwwb用户头像]( "wwwwb用户自定义头像")](http://hi.csdn.net/wwwwb)
* [wwwwb](http://hi.csdn.net/wwwwb)
*
*
* 等　级：![]()
* 9

10
16 #1楼 得分：0回复于：2009-10-20 15:48:01[]() 1、贴记录及要求结果出来看看；
2、有SQL，对字符操作速度不会快；
3、建议修改成列的形式
号码1
号码2
...
号码n * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![vinsonshen用户头像]( "vinsonshen用户自定义头像")](http://hi.csdn.net/vinsonshen)
* [vinsonshen](http://hi.csdn.net/vinsonshen)
* (阿呢陀佛,一切皆空)
*
* 等　级：![]()
* 2 #2楼 得分：0回复于：2009-10-20 15:48:20[]() 大致思路就像你这样来处理了。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![vinsonshen用户头像]( "vinsonshen用户自定义头像")](http://hi.csdn.net/vinsonshen)
* [vinsonshen](http://hi.csdn.net/vinsonshen)
* (阿呢陀佛,一切皆空)
*
* 等　级：![]()
* 2 #3楼 得分：0回复于：2009-10-20 15:54:18[]() 这样的需求，直接用一句SQL是无法处理的，基本上像你那样写个存储过程来处理（也可以写程序用程序处理）。
至于你说的“如果大量，用户并发操作就更加慢了 ”，那可以在存储过程里面的循环那部分用的是个临时表（有利于独立会话，减少竞争），然后循环处理完成后，在存储过程尾部一次性把临时表里面的所有记录插入到正式表中去（批量提交，会提高速度不少，同时减少并发竞争）。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![ACMAIN_CHM用户头像]( "ACMAIN_CHM用户自定义头像")](http://hi.csdn.net/ACMAIN_CHM)
* [ACMAIN_CHM](http://hi.csdn.net/ACMAIN_CHM)
* (acmain)
*
* 等　级：![]()
* 2

9 #4楼 得分：0回复于：2009-10-20 16:02:48[]() set t_MCODE=substring(t_MCODE,t_i+1,length(t_MCODE)-t_i);
这个操作比较浪费时间。 不要复制这个文本串。换个思路。
iPosCur, iPosNext 得到当前的 ; 位置iPosCur,，然后 找 iPosCur 后下一个; 的位置 iPosNext, 取 iPosCur, iPosNext  的字符串为号码，再 set iPosCur = iPosNext ; 再继续循环。
10,000 个号码 * 11 = 110,000 字节的字符串你的程序中复制了太多的次数！ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![foolbirdflyfirst用户头像]( "foolbirdflyfirst用户自定义头像")](http://hi.csdn.net/foolbirdflyfirst)
* [foolbirdflyfirst](http://hi.csdn.net/foolbirdflyfirst)
* (湖水清澈)
*
* 等　级：![]()
* 3

6
3 #5楼 得分：0回复于：2009-10-20 17:27:18[]() 这种思路会不会快一点。
===============================================================================
SQL codemysql> set @b = '123;234;567;789'; Query OK, 0 rows affected (0.00 sec) mysql> set @a = concat(concat("insert into x values('",replace(@b,';',"'),('"))," ')"); Query OK, 0 rows affected (0.00 sec) mysql> select @a; +----------------------------------------------------+ | @a | +----------------------------------------------------+ | insert into x values('123'),('234'),('567'),('789') | +----------------------------------------------------+ 1 row in set (0.00 sec)
然后prepare,execute. * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![storyxsj用户头像]( "storyxsj用户自定义头像")](http://hi.csdn.net/storyxsj)
* [storyxsj](http://hi.csdn.net/storyxsj)
* (慢慢地说，但要迅速地想)
*
* 等　级：![]()
* #6楼 得分：0回复于：2009-10-20 17:46:21[]() 楼上的思路很新颖! * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![ACMAIN_CHM用户头像]( "ACMAIN_CHM用户自定义头像")](http://hi.csdn.net/ACMAIN_CHM)
* [ACMAIN_CHM](http://hi.csdn.net/ACMAIN_CHM)
* (acmain)
*
* 等　级：![]()
* 2

9 #7楼 得分：0回复于：2009-10-20 18:59:26[]() #5楼 快慢没试过，不过的确另人耳目一新。![]() * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![xiaocongzhi用户头像]( "xiaocongzhi用户自定义头像")](http://hi.csdn.net/xiaocongzhi)
* [xiaocongzhi](http://hi.csdn.net/xiaocongzhi)
* (xiaocongzhi)
*
* 等　级：![]()
* #8楼 得分：0回复于：2009-10-20 19:41:03[]() 进来学习一下 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![robyjeffding用户头像]( "robyjeffding用户自定义头像")](http://hi.csdn.net/robyjeffding)
* [robyjeffding](http://hi.csdn.net/robyjeffding)
* (robyjeffding)
*
* 等　级：![]()
* #9楼 得分：0回复于：2009-10-20 20:21:19[]() 顶一下！ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![zswyhyfmz用户头像]( "zswyhyfmz用户自定义头像")](http://hi.csdn.net/zswyhyfmz)
* [zswyhyfmz](http://hi.csdn.net/zswyhyfmz)
* (zswyhyfmz)
*
* 等　级：![]()
* #10楼 得分：0回复于：2009-10-20 20:33:54[]() 学习了~~~~ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![zswyhyfmz用户头像]( "zswyhyfmz用户自定义头像")](http://hi.csdn.net/zswyhyfmz)
* [zswyhyfmz](http://hi.csdn.net/zswyhyfmz)
* (zswyhyfmz)
*
* 等　级：![]()
* #11楼 得分：0回复于：2009-10-20 20:34:18[]() 大家都是牛人啊~~~ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![zswyhyfmz用户头像]( "zswyhyfmz用户自定义头像")](http://hi.csdn.net/zswyhyfmz)
* [zswyhyfmz](http://hi.csdn.net/zswyhyfmz)
* (zswyhyfmz)
*
* 等　级：![]()
* #12楼 得分：0回复于：2009-10-20 20:34:42[]() 我学了好久现在都忘记光啦 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![vinsonshen用户头像]( "vinsonshen用户自定义头像")](http://hi.csdn.net/vinsonshen)
* [vinsonshen](http://hi.csdn.net/vinsonshen)
* (阿呢陀佛,一切皆空)
*
* 等　级：![]()
* 2 #13楼 得分：0回复于：2009-10-20 20:54:46[]() 引用 5 楼 foolbirdflyfirst 的回复:
这种思路会不会快一点。
===============================================================================
SQL codemysql>set@b='123;234;567;789';
Query OK,0 rows affected (0.00 sec)
mysql>set@a= concat(concat("insertinto xvalues('",replace(@b,';',"'),('")),"')");
Query OK,0 rows affected (0.00 sec)
mysql>select@a;+----------------------------------------------------+|@a|+----------------------------------------------------+|insertinto xvalues('123'),('234'),('567'),('789')|+----------------------------------------------------+1 rowinset (0.00 sec)
然后prepare,execute.
这个要注意变量的值是否超出系统允许范围哦 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![vinsonshen用户头像]( "vinsonshen用户自定义头像")](http://hi.csdn.net/vinsonshen)
* [vinsonshen](http://hi.csdn.net/vinsonshen)
* (阿呢陀佛,一切皆空)
*
* 等　级：![]()
* 2 #14楼 得分：0回复于：2009-10-20 20:56:00[]() 引用 7 楼 acmain_chm 的回复:
#5楼 快慢没试过，不过的确另人耳目一新。
这个也是mysql独有的，因为其insert支持多值插入的写法，也就造就了写法“独特”。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![nianzhang747用户头像]( "nianzhang747用户自定义头像")](http://hi.csdn.net/nianzhang747)
* [nianzhang747](http://hi.csdn.net/nianzhang747)
* (飓风)
*
* 等　级：![]()
* #15楼 得分：0回复于：2009-10-20 21:02:46[]() 只能存储过程了 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![shlxbj用户头像]( "shlxbj用户自定义头像")](http://hi.csdn.net/shlxbj)
* [shlxbj](http://hi.csdn.net/shlxbj)
* (shlxbj)
*
* 等　级：![]()
* #16楼 得分：0回复于：2009-10-20 21:02:52[]() 该回复于2009-10-20 22:52:45被版主删除 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![china92用户头像]( "china92用户自定义头像")](http://hi.csdn.net/china92)
* [china92](http://hi.csdn.net/china92)
*
*
* 等　级：![]()
* #17楼 得分：0回复于：2009-10-20 21:32:47[]() 观摩 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![ibelive007用户头像]( "ibelive007用户自定义头像")](http://hi.csdn.net/ibelive007)
* [ibelive007](http://hi.csdn.net/ibelive007)
*
*
* 等　级：![]()
* #18楼 得分：0回复于：2009-10-20 21:32:59[]() 今天涨了见识。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![yanronggui用户头像]( "yanronggui用户自定义头像")](http://hi.csdn.net/yanronggui)
* [yanronggui](http://hi.csdn.net/yanronggui)
*
*
* 等　级：![]()
* #19楼 得分：0回复于：2009-10-20 21:44:45[]() o了 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![muqiaoqiao用户头像]( "muqiaoqiao用户自定义头像")](http://hi.csdn.net/muqiaoqiao)
* [muqiaoqiao](http://hi.csdn.net/muqiaoqiao)
* (muqiaoqiao)
*
* 等　级：![]()
* #20楼 得分：0回复于：2009-10-20 21:48:52[]() 我是来学习的~牛人啊 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![ms45574511用户头像]( "ms45574511用户自定义头像")](http://hi.csdn.net/ms45574511)
* [ms45574511](http://hi.csdn.net/ms45574511)
* (ms45574511)
*
* 等　级：![]()
* #21楼 得分：0回复于：2009-10-20 22:50:13[]() sss * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![ms45574511用户头像]( "ms45574511用户自定义头像")](http://hi.csdn.net/ms45574511)
* [ms45574511](http://hi.csdn.net/ms45574511)
* (ms45574511)
*
* 等　级：![]()
* #22楼 得分：0回复于：2009-10-20 22:50:37[]() 好 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![ms45574511用户头像]( "ms45574511用户自定义头像")](http://hi.csdn.net/ms45574511)
* [ms45574511](http://hi.csdn.net/ms45574511)
* (ms45574511)
*
* 等　级：![]()
* #23楼 得分：0回复于：2009-10-20 22:50:58[]() 好好好 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![nettman用户头像]( "nettman用户自定义头像")](http://hi.csdn.net/nettman)
* [nettman](http://hi.csdn.net/nettman)
* (nm)
*
* 等　级：![]()
* #24楼 得分：0回复于：2009-10-20 23:12:28[]() Mark！ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![zhoudashu用户头像]( "zhoudashu用户自定义头像")](http://hi.csdn.net/zhoudashu)
* [zhoudashu](http://hi.csdn.net/zhoudashu)
* (zhoudashu)
*
* 等　级：![]()
* #25楼 得分：0回复于：2009-10-20 23:48:19[]() BUCUO ,XUEDAOLE * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![hiily用户头像]( "hiily用户自定义头像")](http://hi.csdn.net/hiily)
* [hiily](http://hi.csdn.net/hiily)
* (hiily)
*
* 等　级：![]()
* #26楼 得分：0回复于：2009-10-21 00:19:55[]() dddd[aaa](http://ffff/ "aaa") * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![fanxing168用户头像]( "fanxing168用户自定义头像")](http://hi.csdn.net/fanxing168)
* [fanxing168](http://hi.csdn.net/fanxing168)
* (我吃西红柿)
*
* 等　级：![]()
* #27楼 得分：0回复于：2009-10-21 02:45:34[]() xuexi * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![bgx51用户头像]( "bgx51用户自定义头像")](http://hi.csdn.net/bgx51)
* [bgx51](http://hi.csdn.net/bgx51)
* (bgx51)
*
* 等　级：![]()
* #28楼 得分：0回复于：2009-10-21 04:30:58[]() 我是新手来看看。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![anycall2004用户头像]( "anycall2004用户自定义头像")](http://hi.csdn.net/anycall2004)
* [anycall2004](http://hi.csdn.net/anycall2004)
* (没事，瞎转悠！)
*
* 等　级：![]()
* #29楼 得分：0回复于：2009-10-21 07:04:46[]() 学习了 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![yongy1978用户头像]( "yongy1978用户自定义头像")](http://hi.csdn.net/yongy1978)
* [yongy1978](http://hi.csdn.net/yongy1978)
* (yongy1978)
*
* 等　级：![]()
* #30楼 得分：0回复于：2009-10-21 07:32:05[]() 学习了，谢谢！ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![VCCxiaoting用户头像]( "VCCxiaoting用户自定义头像")](http://hi.csdn.net/VCCxiaoting)
* [VCCxiaoting](http://hi.csdn.net/VCCxiaoting)
* (VCCxiaoting)
*
* 等　级：![]()
* #31楼 得分：0回复于：2009-10-21 07:54:15[]() 学习了 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![yangyongbo123456用户头像]( "yangyongbo123456用户自定义头像")](http://hi.csdn.net/yangyongbo123456)
* [yangyongbo123456](http://hi.csdn.net/yangyongbo123456)
* (yangyongbo123456)
*
* 等　级：![]()
* #32楼 得分：0回复于：2009-10-21 08:25:17[]() 新手学黑客快速入门的好地方：
    http://vip.hackbase.com * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![SlaughtChen用户头像]( "SlaughtChen用户自定义头像")](http://hi.csdn.net/SlaughtChen)
* [SlaughtChen](http://hi.csdn.net/SlaughtChen)
*
*
* 等　级：![]()
* #33楼 得分：0回复于：2009-10-21 08:48:44[]() 进来学习一下 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![macchen1224用户头像]( "macchen1224用户自定义头像")](http://hi.csdn.net/macchen1224)
* [macchen1224](http://hi.csdn.net/macchen1224)
*
*
* 等　级：![]()
* #34楼 得分：0回复于：2009-10-21 08:52:46[]() up. * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![gefieder用户头像]( "gefieder用户自定义头像")](http://hi.csdn.net/gefieder)
* [gefieder](http://hi.csdn.net/gefieder)
*
*
* 等　级：![]()
* #35楼 得分：0回复于：2009-10-21 09:11:06[]() 关注中...... * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![liudejie用户头像]( "liudejie用户自定义头像")](http://hi.csdn.net/liudejie)
* [liudejie](http://hi.csdn.net/liudejie)
* (java)
*
* 等　级：![]()
* #36楼 得分：0回复于：2009-10-21 09:18:12[]() xuexi le * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![coky2008用户头像]( "coky2008用户自定义头像")](http://hi.csdn.net/coky2008)
* [coky2008](http://hi.csdn.net/coky2008)
* (coky2008)
*
* 等　级：![]()
* #37楼 得分：0回复于：2009-10-21 09:22:51[]() 顶了。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![mykoma521用户头像]( "mykoma521用户自定义头像")](http://hi.csdn.net/mykoma521)
* [mykoma521](http://hi.csdn.net/mykoma521)
* (mykoma521)
*
* 等　级：![]()
* #38楼 得分：0回复于：2009-10-21 09:28:21[]() 顶~~ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![a043028448用户头像]( "a043028448用户自定义头像")](http://hi.csdn.net/a043028448)
* [a043028448](http://hi.csdn.net/a043028448)
* (a043028448)
*
* 等　级：![]()
* #39楼 得分：0回复于：2009-10-21 09:37:07[]() 观贴。。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![qiao246用户头像]( "qiao246用户自定义头像")](http://hi.csdn.net/qiao246)
* [qiao246](http://hi.csdn.net/qiao246)
* (qiao246)
*
* 等　级：![]()
* #40楼 得分：0回复于：2009-10-21 09:57:05[]() 士大夫似的发 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![amstar用户头像]( "amstar用户自定义头像")](http://hi.csdn.net/amstar)
* [amstar](http://hi.csdn.net/amstar)
* (阿门)
*
* 等　级：![]()
* #41楼 得分：0回复于：2009-10-21 10:01:01[]() 用数组和split * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![egyptcaesar用户头像]( "egyptcaesar用户自定义头像")](http://hi.csdn.net/egyptcaesar)
* [egyptcaesar](http://hi.csdn.net/egyptcaesar)
* (笨小孩)
*
* 等　级：![]()
* #42楼 得分：0回复于：2009-10-21 10:14:51[]() 进来长知识了。。5楼不错。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![benbenhai2008用户头像]( "benbenhai2008用户自定义头像")](http://hi.csdn.net/benbenhai2008)
* [benbenhai2008](http://hi.csdn.net/benbenhai2008)
* (benbenhai2008)
*
* 等　级：![]()
* #43楼 得分：0回复于：2009-10-21 10:29:57[]() up * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![ZHULIULIU用户头像]( "ZHULIULIU用户自定义头像")](http://hi.csdn.net/ZHULIULIU)
* [ZHULIULIU](http://hi.csdn.net/ZHULIULIU)
* (ZHULIULIU)
*
* 等　级：![]()
* #44楼 得分：0回复于：2009-10-21 10:44:13[]() 记号学习 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![sugarnirvana用户头像]( "sugarnirvana用户自定义头像")](http://hi.csdn.net/sugarnirvana)
* [sugarnirvana](http://hi.csdn.net/sugarnirvana)
* (TINA)
*
* 等　级：![]()
* #45楼 得分：0回复于：2009-10-21 10:51:29[]() 学习 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![yjsgidt用户头像]( "yjsgidt用户自定义头像")](http://hi.csdn.net/yjsgidt)
* [yjsgidt](http://hi.csdn.net/yjsgidt)
* (yjsgidt)
*
* 等　级：![]()
* #46楼 得分：0回复于：2009-10-21 10:54:20[]() 顶~~~ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![wangtao198377用户头像]( "wangtao198377用户自定义头像")](http://hi.csdn.net/wangtao198377)
* [wangtao198377](http://hi.csdn.net/wangtao198377)
*
*
* 等　级：![]()
* #47楼 得分：0回复于：2009-10-21 10:56:34[]() 学习 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![fcly1981826ly用户头像]( "fcly1981826ly用户自定义头像")](http://hi.csdn.net/fcly1981826ly)
* [fcly1981826ly](http://hi.csdn.net/fcly1981826ly)
* (fcly1981826ly)
*
* 等　级：![]()
* #48楼 得分：0回复于：2009-10-21 10:57:40[]() 学习了 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![sishenqiao用户头像]( "sishenqiao用户自定义头像")](http://hi.csdn.net/sishenqiao)
* [sishenqiao](http://hi.csdn.net/sishenqiao)
* (sishenqiao)
*
* 等　级：![]()
* #49楼 得分：0回复于：2009-10-21 11:09:52[]() 记号
学习 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![sunnymnz用户头像]( "sunnymnz用户自定义头像")](http://hi.csdn.net/sunnymnz)
* [sunnymnz](http://hi.csdn.net/sunnymnz)
* (sunnymnz)
*
* 等　级：![]()
* #50楼 得分：0回复于：2009-10-21 11:16:15[]() 帮定议席 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![zhuoyue用户头像]( "zhuoyue用户自定义头像")](http://hi.csdn.net/zhuoyue)
* [zhuoyue](http://hi.csdn.net/zhuoyue)
* (海阔天空)
*
* 等　级：![]()
* #51楼 得分：0回复于：2009-10-21 11:26:22[]() set t_i=POSITION(';' in t_MCODE);
这个 POSITION() 是怎么个用法啊，怎么baidu不到啊 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![werwfh用户头像]( "werwfh用户自定义头像")](http://hi.csdn.net/werwfh)
* [werwfh](http://hi.csdn.net/werwfh)
* (werwfh)
*
* 等　级：![]()
* #52楼 得分：0回复于：2009-10-21 11:32:14[]() 看看学习中 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![qinqinxiatiao用户头像]( "qinqinxiatiao用户自定义头像")](http://hi.csdn.net/qinqinxiatiao)
* [qinqinxiatiao](http://hi.csdn.net/qinqinxiatiao)
* (亲亲虾条)
*
* 等　级：![]()
* #53楼 得分：0回复于：2009-10-21 11:32:33[]() mark!!!!! * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![amloy520用户头像]( "amloy520用户自定义头像")](http://hi.csdn.net/amloy520)
* [amloy520](http://hi.csdn.net/amloy520)
*
*
* 等　级：![]()
* #54楼 得分：0回复于：2009-10-21 12:19:53[]() 顶!!!!!!!!!!! * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![Lithree1987用户头像]( "Lithree1987用户自定义头像")](http://hi.csdn.net/Lithree1987)
* [Lithree1987](http://hi.csdn.net/Lithree1987)
* (Lithree1987)
*
* 等　级：![]()
* #55楼 得分：0回复于：2009-10-21 13:13:51[]() 观摩学习中....... * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![china_west用户头像]( "china_west用户自定义头像")](http://hi.csdn.net/china_west)
* [china_west](http://hi.csdn.net/china_west)
* (china_west)
*
* 等　级：![]()
* #56楼 得分：0回复于：2009-10-21 13:15:12[]() ding * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![xsm545用户头像]( "xsm545用户自定义头像")](http://hi.csdn.net/xsm545)
* [xsm545](http://hi.csdn.net/xsm545)
*
*
* 等　级：![]()
* #57楼 得分：0回复于：2009-10-21 13:33:28[]() 5楼方法很不错.学习个 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![haklk用户头像]( "haklk用户自定义头像")](http://hi.csdn.net/haklk)
* [haklk](http://hi.csdn.net/haklk)
* (haklk)
*
* 等　级：![]()
* #58楼 得分：0回复于：2009-10-21 13:34:45[]() 进来学习一下 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![yidichaxiang用户头像]( "yidichaxiang用户自定义头像")](http://hi.csdn.net/yidichaxiang)
* [yidichaxiang](http://hi.csdn.net/yidichaxiang)
* (幽灵·逐梦)
*
* 等　级：![]()
* #59楼 得分：0回复于：2009-10-21 13:51:50[]() mark * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![liangguanggui用户头像]( "liangguanggui用户自定义头像")](http://hi.csdn.net/liangguanggui)
* [liangguanggui](http://hi.csdn.net/liangguanggui)
* (liangguanggui)
*
* 等　级：![]()
* #60楼 得分：0回复于：2009-10-21 14:15:55[]() 顶顶顶顶顶顶顶顶顶 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![mlsml用户头像]( "mlsml用户自定义头像")](http://hi.csdn.net/mlsml)
* [mlsml](http://hi.csdn.net/mlsml)
*
*
* 等　级：![]()
* #61楼 得分：0回复于：2009-10-21 14:35:54[]() set @a = concat(concat("insert into xvalues('",replace(@b,';',"'),('")),"')");
----------这都什么意思啊，格式方面都看不太懂。。。。。
"insert into xvalues('",replace(@b,';',"'),('")),"' * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![andelingzi用户头像]( "andelingzi用户自定义头像")](http://hi.csdn.net/andelingzi)
* [andelingzi](http://hi.csdn.net/andelingzi)
*
*
* 等　级：![]()
* #62楼 得分：0回复于：2009-10-21 14:40:13[]() 长见识了 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![zhaoyanhua807102用户头像]( "zhaoyanhua807102用户自定义头像")](http://hi.csdn.net/zhaoyanhua807102)
* [zhaoyanhua807102](http://hi.csdn.net/zhaoyanhua807102)
*
*
* 等　级：![]()
* #63楼 得分：0回复于：2009-10-21 15:30:11[]() 可以使用xml处理。
microsoft sql server 中支持 xml很好。
不知道mysql怎样。
先处理成可执行的xml，然后一条语句就可以处理。
说的不对的话，请大家见谅。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![liyongbin123用户头像]( "liyongbin123用户自定义头像")](http://hi.csdn.net/liyongbin123)
* [liyongbin123](http://hi.csdn.net/liyongbin123)
* (liyongbin123)
*
* 等　级：![]()
* #64楼 得分：0回复于：2009-10-21 15:53:56[]() 学学习习，支持 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![kanon_lgt用户头像]( "kanon_lgt用户自定义头像")](http://hi.csdn.net/kanon_lgt)
* [kanon_lgt](http://hi.csdn.net/kanon_lgt)
* (光导纤维)
*
* 等　级：![]()
* #65楼 得分：0回复于：2009-10-21 16:35:20[]() 请教下：一万个手机号码，组成的字符串长度是11万个字符。t_code您用了什么列类型呢？ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![crskyp用户头像]( "crskyp用户自定义头像")](http://hi.csdn.net/crskyp)
* [crskyp](http://hi.csdn.net/crskyp)
* (孤舟)
*
* 等　级：![]()
* #66楼 得分：0回复于：2009-10-21 16:40:13[]() mark * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![hbwz1985用户头像]( "hbwz1985用户自定义头像")](http://hi.csdn.net/hbwz1985)
* [hbwz1985](http://hi.csdn.net/hbwz1985)
* (C@nDour)
*
* 等　级：![]()
* #67楼 得分：0回复于：2009-10-21 17:01:19[]() 5楼：赏，欣赏，很欣赏！路过 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![hbwz1985用户头像]( "hbwz1985用户自定义头像")](http://hi.csdn.net/hbwz1985)
* [hbwz1985](http://hi.csdn.net/hbwz1985)
* (C@nDour)
*
* 等　级：![]()
* #68楼 得分：0回复于：2009-10-21 17:01:56[]() 测试下，哈哈！@ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![ssDEais用户头像]( "ssDEais用户自定义头像")](http://hi.csdn.net/ssDEais)
* [ssDEais](http://hi.csdn.net/ssDEais)
* (ssDEais)
*
* 等　级：![]()
* #69楼 得分：0回复于：2009-10-21 17:04:37[]() ～ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![egceo用户头像]( "egceo用户自定义头像")](http://hi.csdn.net/egceo)
* [egceo](http://hi.csdn.net/egceo)
* (郑广智)
*
* 等　级：![]()
* #70楼 得分：0回复于：2009-10-21 17:08:57[]() mark * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![zhuzhupj用户头像]( "zhuzhupj用户自定义头像")](http://hi.csdn.net/zhuzhupj)
* [zhuzhupj](http://hi.csdn.net/zhuzhupj)
* (zhuzhupj)
*
* 等　级：![]()
* #71楼 得分：0回复于：2009-10-21 17:09:50[]() 学习下 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![ckl881003用户头像]( "ckl881003用户自定义头像")](http://hi.csdn.net/ckl881003)
* [ckl881003](http://hi.csdn.net/ckl881003)
* (闪电)
*
* 等　级：![]()
* #72楼 得分：0回复于：2009-10-21 17:10:41[]() 引用 65 楼 kanon_lgt 的回复:
请教下：一万个手机号码，组成的字符串长度是11万个字符。t_code您用了什么列类型呢？
同问、、、 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![s_h1869用户头像]( "s_h1869用户自定义头像")](http://hi.csdn.net/s_h1869)
* [s_h1869](http://hi.csdn.net/s_h1869)
* (s_h1869)
*
* 等　级：![]()
* #73楼 得分：0回复于：2009-10-21 17:24:11[]() 太复杂，看不懂了 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![hankanling123用户头像]( "hankanling123用户自定义头像")](http://hi.csdn.net/hankanling123)
* [hankanling123](http://hi.csdn.net/hankanling123)
* (hankanling123)
*
* 等　级：![]()
* #74楼 得分：0回复于：2009-10-21 17:27:07[]() T-SQL语句是一样的。。。。。。。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![sxq51189540用户头像]( "sxq51189540用户自定义头像")](http://hi.csdn.net/sxq51189540)
* [sxq51189540](http://hi.csdn.net/sxq51189540)
* (zero)
*
* 等　级：![]()
* #75楼 得分：0回复于：2009-10-21 17:30:07[]() 学习 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![fxltsblsds用户头像]( "fxltsblsds用户自定义头像")](http://hi.csdn.net/fxltsblsds)
* [fxltsblsds](http://hi.csdn.net/fxltsblsds)
* (风雨彩虹)
*
* 等　级：![]()
* #76楼 得分：0回复于：2009-10-21 18:19:40[]() 好久没来了！！！ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![beimgly用户头像]( "beimgly用户自定义头像")](http://hi.csdn.net/beimgly)
* [beimgly](http://hi.csdn.net/beimgly)
* (beimgly)
*
* 等　级：![]()
* #77楼 得分：0回复于：2009-10-21 20:03:39[]() mysql> set @b = '123;234;567;789';
Query OK, 0 rows affected (0.00 sec)
mysql> set @a = concat(concat("insert into x values('",replace(@b,';',"'),('")),"
')");
Query OK, 0 rows affected (0.00 sec)
mysql> select @a;
+----------------------------------------------------+
| @a                                                |
+----------------------------------------------------+
| insert into x values('123'),('234'),('567'),('789') |
+----------------------------------------------------+
1 row in set (0.00 sec) * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![lixiankun001用户头像]( "lixiankun001用户自定义头像")](http://hi.csdn.net/lixiankun001)
* [lixiankun001](http://hi.csdn.net/lixiankun001)
* (lixiankun001)
*
* 等　级：![]()
* #78楼 得分：0回复于：2009-10-21 20:16:49[]() 学习学习了！~ * [对我有用]()[0]
* [丢个板砖]()[1]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![fenyun5686用户头像]( "fenyun5686用户自定义头像")](http://hi.csdn.net/fenyun5686)
* [fenyun5686](http://hi.csdn.net/fenyun5686)
*
*
* 等　级：![]()
* #79楼 得分：0回复于：2009-10-21 20:50:00[]() 学习了。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![vakano用户头像]( "vakano用户自定义头像")](http://hi.csdn.net/vakano)
* [vakano](http://hi.csdn.net/vakano)
* (vakano)
*
* 等　级：![]()
* #80楼 得分：0回复于：2009-10-21 21:31:08[]() 谢谢分享！ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![missbeast用户头像]( "missbeast用户自定义头像")](http://hi.csdn.net/missbeast)
* [missbeast](http://hi.csdn.net/missbeast)
* (missbeast)
*
* 等　级：![]()
* #81楼 得分：0回复于：2009-10-21 21:39:57[]() 每天回帖即可获得10分可用分！ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![ths10086用户头像]( "ths10086用户自定义头像")](http://hi.csdn.net/ths10086)
* [ths10086](http://hi.csdn.net/ths10086)
* (ths10086)
*
* 等　级：![]()
* #82楼 得分：0回复于：2009-10-21 21:43:55[]() 刚开通...看些新面孔.. * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![Beloria用户头像]( "Beloria用户自定义头像")](http://hi.csdn.net/Beloria)
* [Beloria](http://hi.csdn.net/Beloria)
* (Beloria)
*
* 等　级：![]()
* #83楼 得分：0回复于：2009-10-21 21:47:30[]() 恁多牛人。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![learnonedelisonedel用户头像]( "learnonedelisonedel用户自定义头像")](http://hi.csdn.net/learnonedelisonedel)
* [learnonedelisonedel](http://hi.csdn.net/learnonedelisonedel)
* (learnonedelison)
*
* 等　级：![]()
* #84楼 得分：0回复于：2009-10-21 21:50:41[]() 5L独具匠心 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![hwhunter用户头像]( "hwhunter用户自定义头像")](http://hi.csdn.net/hwhunter)
* [hwhunter](http://hi.csdn.net/hwhunter)
* (Iverson)
*
* 等　级：![]()
* #85楼 得分：0回复于：2009-10-21 21:59:45[]() 这种思路会不会快一点。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![clplayboy1用户头像]( "clplayboy1用户自定义头像")](http://hi.csdn.net/clplayboy1)
* [clplayboy1](http://hi.csdn.net/clplayboy1)
* (clplayboy1)
*
* 等　级：![]()
* #86楼 得分：0回复于：2009-10-21 22:12:38[]() 呵呵 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![chaozai2688用户头像]( "chaozai2688用户自定义头像")](http://hi.csdn.net/chaozai2688)
* [chaozai2688](http://hi.csdn.net/chaozai2688)
* (chaozai2688)
*
* 等　级：![]()
* #87楼 得分：0回复于：2009-10-21 22:28:30[]() 学习中。。。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![guizhiqin用户头像]( "guizhiqin用户自定义头像")](http://hi.csdn.net/guizhiqin)
* [guizhiqin](http://hi.csdn.net/guizhiqin)
* (guizhiqin)
*
* 等　级：![]()
* #88楼 得分：0回复于：2009-10-21 22:53:46[]() 学习中 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![sdnjwang用户头像]( "sdnjwang用户自定义头像")](http://hi.csdn.net/sdnjwang)
* [sdnjwang](http://hi.csdn.net/sdnjwang)
* (sdnjwang)
*
* 等　级：![]()
* #89楼 得分：0回复于：2009-10-21 23:39:58[]() 学习 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![liyaqin88用户头像]( "liyaqin88用户自定义头像")](http://hi.csdn.net/liyaqin88)
* [liyaqin88](http://hi.csdn.net/liyaqin88)
* (liyaqin88)
*
* 等　级：![]()
* #90楼 得分：0回复于：2009-10-22 01:27:10[]() 以我觉得应该分类处理就会减小处理的数据冗余度！！ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![chen_767用户头像]( "chen_767用户自定义头像")](http://hi.csdn.net/chen_767)
* [chen_767](http://hi.csdn.net/chen_767)
* (chen_767)
*
* 等　级：![]()
* #91楼 得分：0回复于：2009-10-22 07:58:24[]() 学习了。 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![xiedi1209用户头像]( "xiedi1209用户自定义头像")](http://hi.csdn.net/xiedi1209)
* [xiedi1209](http://hi.csdn.net/xiedi1209)
* (谢頔)
*
* 等　级：![]()
* #92楼 得分：0回复于：2009-10-22 08:26:45[]() 纯sql写？ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![gahyyai用户头像]( "gahyyai用户自定义头像")](http://hi.csdn.net/gahyyai)
* [gahyyai](http://hi.csdn.net/gahyyai)
* (冰岛男孩)
*
* 等　级：![]()
* #93楼 得分：0回复于：2009-10-22 08:35:28[]() mark * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![YHL27用户头像]( "YHL27用户自定义头像")](http://hi.csdn.net/YHL27)
* [YHL27](http://hi.csdn.net/YHL27)
* (追梦)
*
* 等　级：![]()
* #94楼 得分：0回复于：2009-10-22 08:45:24[]() sf! * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![mlsml用户头像]( "mlsml用户自定义头像")](http://hi.csdn.net/mlsml)
* [mlsml](http://hi.csdn.net/mlsml)
*
*
* 等　级：![]()
* #95楼 得分：0回复于：2009-10-22 08:57:38[]() 谁能解答下这个
set @a = concat(concat("insert into x values('",replace(@b,';',"'),('")),"
')");
标点符号都不匹配却可以运行的问题~~~ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![aadss用户头像]( "aadss用户自定义头像")](http://hi.csdn.net/aadss)
* [aadss](http://hi.csdn.net/aadss)
* (悠闲)
*
* 等　级：![]()
* #96楼 得分：0回复于：2009-10-22 09:51:58[]() 热心帮顶 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![flybao51用户头像]( "flybao51用户自定义头像")](http://hi.csdn.net/flybao51)
* [flybao51](http://hi.csdn.net/flybao51)
* (flybao51)
*
* 等　级：![]()
* #97楼 得分：0回复于：2009-10-22 09:58:32[]() 学习一下 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![kirave用户头像]( "kirave用户自定义头像")](http://hi.csdn.net/kirave)
* [kirave](http://hi.csdn.net/kirave)
* (kirave)
*
* 等　级：![]()
* #98楼 得分：0回复于：2009-10-22 09:59:09[]() 好东东呀 * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![scutLiu用户头像]( "scutLiu用户自定义头像")](http://hi.csdn.net/scutLiu)
* [scutLiu](http://hi.csdn.net/scutLiu)
* (EndIf)
*
* 等　级：![]()
* #99楼 得分：0回复于：2009-10-22 10:07:54[]() mark * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)  * [![crb89643806用户头像]( "crb89643806用户自定义头像")](http://hi.csdn.net/crb89643806)
* [crb89643806](http://hi.csdn.net/crb89643806)
* (crb89643806)
*
* 等　级：![]()
* #100楼 得分：0回复于：2009-10-22 10:57:42[]() 来学习了，呵呵！！！ * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#top)

* [管理菜单]()

* [生成帖子](http://forum.csdn.net/PointForum/BuildTopic.aspx?topicId=5d80ce75-305b-420c-95b4-ca1437c6a221&postDate=2009-10-20+15%3a42%3a21&return=)
* [置顶]()
* [推荐]()
* [取消推荐]()
* [锁定]()
* [解锁]()
* [移动]()
* [编辑]()
* [删除]()
* [帖子加分]()
* [结  帖](http://forum.csdn.net/PointForum/Manage/TopicManageView.aspx?forumID=ba09fe7e-2fb7-42d3-805e-578a4a8485e1&topicID=5d80ce75-305b-420c-95b4-ca1437c6a221&date=2009-10-20+15%3a42%3a21&v=13)
* [发  帖](http://forum.csdn.net/PointForum/Forum/PostTopic.aspx?forumID=ba09fe7e-2fb7-42d3-805e-578a4a8485e1)
* [回  复](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#r_achor)

* [共2页](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#)
* *[1](http://topic.csdn.net/u/20091020/15/5d80ce75-305b-420c-95b4-ca1437c6a221.html)*
* *[2](http://topic.csdn.net/u/20091020/15/5d80ce75-305b-420c-95b4-ca1437c6a221_2.html)*
[公司简介](http://www.csdn.net/company/about.html)| [广告服务](http://www.csdn.net/company/marketing.html)| [银行汇款帐号](http://www.csdn.net/company/account.html)| [联系方式](http://www.csdn.net/company/contact.html)| [版权声明](http://www.csdn.net/company/statement.html)| [问题报告](http://topic.csdn.net/u/20091020/15/5D80CE75-305B-420C-95B4-CA1437C6A221.html#)北京创新乐知广告有限公司 版权所有, 京 ICP 证 070598 号世纪乐知(北京)网络技术有限公司 提供技术支持文明办网文明上网举报电话：13552009689 ![]()Copyright © 1999-2009, CSDN.NET, All Rights Reserved[![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)![]()
