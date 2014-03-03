---
layout: post
title: "oracle 日期常用函數"
categories: DB
tags: 
 - DB
 - oracle
 - 函数
--- 

# oracle 日期常用函數

[![返回主页]()](http://www.cnblogs.com/hellofei/)

# [hellofei](http://www.cnblogs.com/hellofei/)

##

* [博客园](http://www.cnblogs.com/)
* [社区](http://space.cnblogs.com/)
* [首页](http://www.cnblogs.com/hellofei/)
* [新随笔](http://www.cnblogs.com/hellofei/admin/EditPosts.aspx?opt=1)
* [联系](http://space.cnblogs.com/msg/send/hellofei)
* [管理](http://www.cnblogs.com/hellofei/admin/EditPosts.aspx)
* [订阅](http://www.cnblogs.com/hellofei/rss) [![订阅]()](http://www.cnblogs.com/hellofei/rss)
随笔- 45  文章- 0  评论- 1 

# [oracle 日期常用函數 (日期運算)](http://www.cnblogs.com/hellofei/archive/2010/02/03/1662924.html)

oracle 日期常用函數 (日期運算)

[http://blog.blueshop.com.tw/pili9141/articles/52501.aspx](http://blog.blueshop.com.tw/pili9141/articles/52501.aspx)

 

1 日期運算  2   3 1. 更改日期顯示的format  4  ex.  5  ALTER SESSION SET NLS_DATE_FORMAT = 'YYYY/MM/DD';     6  階段作業已被更改     7             8  select sysdate from dual;     9             10  SYSDATE     11  ----------     12  2007/09/20     13             14  --只對目前session有效，一個 connect 視為一個 session  15   16 2. 日期 + 數值  17  ex.  18  select sysdate + 10 from dual;  19    20  SYSDATE+10  21  ----------  22  01-OCT-07   23          24 3. 日期 - 數值  25  ex.  26  select sysdate - 10 from dual;  27   28  SYSDATE-10  29  ----------  30  11-SEP-07  31   32 4. 日期相減得到日期差  33  ex.  34  select sysdate - to_date('20070901','yyyymmdd') aa from dual;  35    36            AA  37  -------------  38    20.4508218    39    40  --◎ 包含時間，所以有小數  41  --◎ 可做日期欄位的計算  42    43  select trunc(sysdate - to_date('20070901','yyyymmdd')) aa from dual;  44    45         AA  46  ----------  47         20  48  --使用trunc取整數，得到日期  49   50 5. 日期相減得到小時差  51  ex.  52  select trunc((sysdate - to_date('20070901','yyyymmdd'))*24) aa from dual;  53   54          AA  55  ----------  56         490  57   58 6. 日期相減得到分鐘差  59  ex.  60  select trunc((sysdate - to_date('20070901','yyyymmdd'))*24*60) aa from dual;  61    62        AA  63  ---------  64      29459  65   66 7. 日期相減得到秒數差  67  ex.  68  select trunc((sysdate - to_date('20070901','yyyymmdd'))*24*60*60) aa from dual;  69   70         AA  71  ----------  72     1767606  73   74 8. 日期 + n小時  75  ex.  76  select to_char(sysdate,'YYYY/MM/DD HH24:MI:SS') aa from dual;  77    78  AA  79  --------------------  80  2007/09/21 11:03:47  --系統時間  81    82  select to_char(sysdate+2/24,'YYYY/MM/DD HH24:MI:SS') aa from dual;  83   84  AA  85  --------------------  86  2007/09/21 13:03:47  --加2小時(理論值)  87   88 9. 日期 + n分鐘   89  ex.  90  select to_char(sysdate+10/1440,'YYYY/MM/DD HH24:MI:SS') aa from dual;  91   92  AA  93  --------------------  94  2007/09/21 11:13:47  --加10分鐘(理論值)  95   96 10. 日期+ n秒鐘  97  ex.  98  select to_char(sysdate+10/86400,'YYYY/MM/DD HH24:MI:SS') aa from dual;  99       100  AA  101  --------------------  102  2007/09/21 11:13:57  --加10秒鐘(理論值) 

[hellofei](http://home.cnblogs.com/hellofei/)
关注 - 0
粉丝 - 0

[关注博主]()

0

0
0

(请您对文章做出评价)

[«](http://www.cnblogs.com/hellofei/archive/2010/02/03/1662922.html) 上一篇：[oracle 內建函數-字串常用函數](http://www.cnblogs.com/hellofei/archive/2010/02/03/1662922.html "发布于2010-02-03 18:52")
[»](http://www.cnblogs.com/hellofei/archive/2010/02/03/1662926.html) 下一篇：[oracle 常用轉換函數(to_char,to_date,to_number)](http://www.cnblogs.com/hellofei/archive/2010/02/03/1662926.html "发布于2010-02-03 18:54")

posted @ 2010-02-03 18:53 [hellofei](http://www.cnblogs.com/hellofei/) 阅读(95) [评论(0)]() [编辑]() [收藏]()
 ![]()

注册用户登录后才能发表评论，请 [登录](http://passport.cnblogs.com/login.aspx?ReturnUrl=http%3a%2f%2fwww.cnblogs.com%2fhellofei%2farchive%2f2010%2f02%2f03%2f1662924.html%3flogin%3d1%23commentform) 或 [注册](http://passport.cnblogs.com/register.aspx?ReturnUrl=http%3a%2f%2fwww.cnblogs.com%2fhellofei%2farchive%2f2010%2f02%2f03%2f1662924.html%23Bottom2)，[返回博客园首页](http://www.cnblogs.com/)。

[IT新闻](http://news.cnblogs.com/):
· [铁山：地球上最安全数据中心](http://news.cnblogs.com/n/71061/)
· [Facebook遭遇早期员工离职潮](http://news.cnblogs.com/n/71064/)
· [25个顶级的Android 应用介绍](http://news.cnblogs.com/n/71060/)
· [新服务为web程序开发者提供位置数据](http://news.cnblogs.com/n/71062/)
· [全球最好国家榜 中国列59位](http://news.cnblogs.com/n/71057/)
[更多IT新闻...](http://news.cnblogs.com/)
[**知识库最新文章**](http://kb.cnblogs.com/ "程序员知识库"):
[开发人员为何应该使用 Mac OS X 兼 OS X 小史](http://kb.cnblogs.com/page/71038/)
[Xvfb+YSlow+ShowSlow搭建前端性能测试框架](http://kb.cnblogs.com/page/70592/)
[.NET 3.x新特性之自动属性及集合初始化](http://kb.cnblogs.com/page/70752/)
[LINQ to SQL快速上手 step by step](http://kb.cnblogs.com/page/70851/)
[ASP.NET:性能与缓存](http://kb.cnblogs.com/page/70671/)

网站导航：
[博客园首页](http://www.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [个人主页](http://home.cnblogs.com/)  [闪存](http://home.cnblogs.com/ing/)  [程序员招聘](http://job.cnblogs.com/)  [社区](http://space.cnblogs.com/)  [博问](http://space.cnblogs.com/q/)
[![]()](http://www.china-pub.com/STATIC07/1005/zh_loving_100528.asp)
[China-pub 计算机图书网上专卖店！6.5万品种2-8折！](http://www.china-pub.com/itbook/)
[China-Pub 计算机绝版图书按需印刷服务](http://www.china-pub.com/static07/0901/zh_jueba_090121.asp)

[<]( "Go to the previous month")2010年2月[>]( "Go to the next month")日一二三四五六311[2](http://www.cnblogs.com/hellofei/archive/2010/2/2.html)[3](http://www.cnblogs.com/hellofei/archive/2010/2/3.html)45678910111213141516171819[20](http://www.cnblogs.com/hellofei/archive/2010/2/20.html)21[22](http://www.cnblogs.com/hellofei/archive/2010/2/22.html)[23](http://www.cnblogs.com/hellofei/archive/2010/2/23.html)242526272812345678910111213

### 公告

粉丝 - 0
关注 - 0
[我的主页](http://home.cnblogs.com/hellofei/)  [个人资料](http://home.cnblogs.com/hellofei/detail/)
[我的闪存](http://home.cnblogs.com/hellofei/ing/)  [发短消息](http://space.cnblogs.com/msg/send/hellofei)
### 搜索

 

 

### 常用链接

* [我的随笔](http://www.cnblogs.com/hellofei/MyPosts.html)
* [我的空间](http://home.cnblogs.com/hellofei/)
* [我的短信](http://space.cnblogs.com/msg/recent)
* [我的评论](http://www.cnblogs.com/hellofei/MyComments.html)
* [更多链接]()
* [我的参与](http://www.cnblogs.com/hellofei/OtherPosts.html "我发表过评论的随笔")
* [我的新闻](http://www.cnblogs.com/hellofei/MyNews.html)
* [最新评论](http://www.cnblogs.com/hellofei/RecentComments.html)
* [我的标签](http://www.cnblogs.com/hellofei/tag/)
### 随笔分类

* [IntelliJ(3)](http://www.cnblogs.com/hellofei/category/232458.html) [(rss)]()
* [Java SE(3)](http://www.cnblogs.com/hellofei/category/239755.html) [(rss)]()
* [Oracle(8)](http://www.cnblogs.com/hellofei/category/232453.html) [(rss)]()

### 随笔档案

* [2010年4月 (2)]()
* [2010年3月 (10)]()
* [2010年2月 (19)]()
* [2010年1月 (14)]()
### 文章分类

* [Javascript](http://www.cnblogs.com/hellofei/category/227909.html) [(rss)]()

### 积分与排名

* 积分 - 5774
* 排名 - 9290
### 最新随笔

* [1. 利用org.apache.commons.io.FileUtils快速读写文件](http://www.cnblogs.com/hellofei/archive/2010/04/08/1707131.html)
* [2. 谜题20：我的类是什么？谜题21：我的类是什么？II](http://www.cnblogs.com/hellofei/archive/2010/04/01/1702113.html)
* [3. -jar参数运行应用时classpath的设置方法](http://www.cnblogs.com/hellofei/archive/2010/03/31/1701658.html)
* [4. 一些淡忘了的Java日期时间函数](http://www.cnblogs.com/hellofei/archive/2010/03/31/1701436.html)
* [5. Oracle大对象处理](http://www.cnblogs.com/hellofei/archive/2010/03/25/1696091.html)
* [6. dbms_lob包学习笔记之三：instr和substr存储过程](http://www.cnblogs.com/hellofei/archive/2010/03/25/1695363.html)
* [7. Oracle中的Raw类型解释](http://www.cnblogs.com/hellofei/archive/2010/03/25/1695297.html)
* [8. How to Submit a Form Using JavaScript](http://www.cnblogs.com/hellofei/archive/2010/03/17/1688319.html)
* [9. AJAX小经验之一：变量冲突处理](http://www.cnblogs.com/hellofei/archive/2010/03/17/1688307.html)
* [10. window.location和window.open的区别](http://www.cnblogs.com/hellofei/archive/2010/03/17/1688305.html)
* [11. 误用Connection.setAutoCommit导致的数据库死锁问题](http://www.cnblogs.com/hellofei/archive/2010/03/03/1677202.html)
* [12. java.lang.ClassCastException: oracle.sql.CLOB](http://www.cnblogs.com/hellofei/archive/2010/03/01/1675788.html)
* [13. 如何判断oracle大字段（clob）为空？](http://www.cnblogs.com/hellofei/archive/2010/02/23/1672062.html)
* [14. Apache Tomcat 服务因 0 (0x0) 服务性错误而停止](http://www.cnblogs.com/hellofei/archive/2010/02/22/1671377.html)
* [15. Apache 防盗链(Apache Anti-Leech)技术的简单实现](http://www.cnblogs.com/hellofei/archive/2010/02/20/1669829.html)
* [16. Tomcat 下配置一个ip绑定多个域名](http://www.cnblogs.com/hellofei/archive/2010/02/20/1669828.html)

### 最新评论[![]( "RSS订阅最最新评论")](http://www.cnblogs.com/hellofei/CommentsRSS.aspx "RSS订阅最最新评论")

[1. Re:利用org.apache.commons.io.FileUtils快速读写文件](http://www.cnblogs.com/hellofei/archive/2010/04/08/1707131.html#1797828)

呵呵，学习了。。。。 (小临)
### 阅读排行榜

* [1. oracle 日期常用函數 (ADD_MONTHS,LAST_DAY,NEXT_DAY,MONTHS_BETWEEN,NEW_TIME,ROUND,TRUNC)(501)](http://www.cnblogs.com/hellofei/archive/2010/02/03/1662925.html)
* [2. 利用org.apache.commons.io.FileUtils快速读写文件(497)](http://www.cnblogs.com/hellofei/archive/2010/04/08/1707131.html)
* [3. dbms_lob包学习笔记之三：instr和substr存储过程(423)](http://www.cnblogs.com/hellofei/archive/2010/03/25/1695363.html)
* [4. HTTP 协议简介(320)](http://www.cnblogs.com/hellofei/archive/2010/01/13/1646276.html)
* [5. Apache Tomcat 服务因 0 (0x0) 服务性错误而停止(263)](http://www.cnblogs.com/hellofei/archive/2010/02/22/1671377.html)

### 评论排行榜

* [1. 利用org.apache.commons.io.FileUtils快速读写文件(1)](http://www.cnblogs.com/hellofei/archive/2010/04/08/1707131.html)
* [2. 谜题20：我的类是什么？谜题21：我的类是什么？II(0)](http://www.cnblogs.com/hellofei/archive/2010/04/01/1702113.html)
* [3. -jar参数运行应用时classpath的设置方法(0)](http://www.cnblogs.com/hellofei/archive/2010/03/31/1701658.html)
* [4. 一些淡忘了的Java日期时间函数(0)](http://www.cnblogs.com/hellofei/archive/2010/03/31/1701436.html)
* [5. Oracle大对象处理(0)](http://www.cnblogs.com/hellofei/archive/2010/03/25/1696091.html)
Copyright ©2010 hellofei

![]() ![]() ![]()
