---
layout: post
title: "Oracle 10g SQL 优化再学习 - joe -专注java-开源-架构-项目管理 - B"
categories: DB
tags: 
 - DB
 - oracle
 - oracle-
--- 

# Oracle 10g SQL 优化再学习 - joe --专注java,开源,架构,项目管理 - BlogJava

# [joe --专注java,开源,架构,项目管理](http://www.blogjava.net/freeman1984/)

STANDING ON THE SHOULDERS OF GIANTS

posts - 339, comments - 292, trackbacks - 0, articles - 1  [BlogJava](http://www.blogjava.net/) :: [首页](http://www.blogjava.net/freeman1984/) :: [新随笔](http://www.blogjava.net/freeman1984/admin/EditPosts.aspx?opt=1) :: [联系](http://www.blogjava.net/freeman1984/contact.aspx?id=1) :: [聚合](http://www.blogjava.net/freeman1984/rss) [![]()](http://www.blogjava.net/freeman1984/rss) :: [管理](http://www.blogjava.net/freeman1984/admin/EditPosts.aspx) ## [Oracle 10g SQL 优化再学习](http://www.blogjava.net/freeman1984/archive/2010/10/10/334235.html)

Posted on 2010-10-10 23:52 [@joe](http://www.blogjava.net/freeman1984/) 阅读(107) [评论(0)](http://www.blogjava.net/freeman1984/archive/2010/10/10/334235.html#Post)  [编辑](http://www.blogjava.net/freeman1984/admin/EditPosts.aspx?postid=334235)  [收藏](http://www.blogjava.net/freeman1984/AddToFavorite.aspx?id=334235) 所属分类: [java性能](http://www.blogjava.net/freeman1984/category/43783.html) 、[方法论](http://www.blogjava.net/freeman1984/category/46503.html) 、[架构](http://www.blogjava.net/freeman1984/category/46575.html) ![]()

 

从8i到10g，Oracle不断进化自己的SQL Tuning智能，一些秘籍级的优化口诀已经失效。
   但我喜欢失效，不用记口诀，操个Toad for Oracle Xpert ，按照大方向舒舒服服的调优才是爱做的事情。

1.Excution Plan
     Excution Plan是最基本的调优概念，不管你的调优吹得如何天花乱堕，结果还是要由Excution plan来显示Oracle 最终用什么索引、按什么顺序连接各表，Full Table Scan还是Access by Rowid Index，瓶颈在什么地方。如果没有它的指导，一切调优都是蒙的。

2.Toad for Oracle Xpert
    用它来调优在真的好舒服。Quest 吞并了Lecco后，将它整合到了Toad 的SQL Tunning里面：最清晰的执行计划显示，自动生成N条等价SQL、给出优化建议，不同SQL执行计划的对比，还有实际执行的逻辑读、物理读数据等等一目了然。

3.索引
   大部分的性能问题其实都是索引应用的问题，Where子句、Order By、Group By 都要用到索引。
   一般开发人员认为将索引建全了就可以下班回家了，实则还有颇多的思量和陷阱。

3.1 索引列上不要进行计算
      这是最最普遍的失效陷阱，比如where trunc(order_date)=trunc(sysdate), i+2>4。索引失效的原因也简单，索引是针对原值建的二叉树，你将列值*3/4+2折腾一番后，原来的二叉树当然就用不上了。解决的方法:
1.　换成等价语法，比如trunc(order_date) 换成

where order_date>trunc(sysdate)-1 and order_date<trunc(sysdate)+1　 2.    特别为计算建立函数索引

create index Ｉ_XXXX on shop_order(trunc(order_date))    3.    将计算从等号左边移到右边
这是针对某些无心之失的纠正，把a*2>4　改为a>4/2；把TO_CHAR(zip) = '94002' 改为zip = TO_NUMBER('94002');

3.2 CBO与索引选择性
     建了索引也不一定会被Oracle用的，就像个挑食的孩子。基于成本的优化器(CBO, Cost-Based Optimizer)，会先看看表的大小，还有索引的重复度，再决定用还是不用。表中有100 条记录而其中有80 个不重复的索引键值. 这个索引的选择性就是80/100 = 0.8，留意Toad里显示索引的Selective和Cardinailty。实在不听话时，就要用hints来调教。
     另外，where语句存在多条索引可用时，只会选择其中一条。所以索引也不是越多越好：）

3.3 索引重建
     传说中数据更新频繁导致有20%的碎片时，Oracle就会放弃这个索引。宁可信其有之下，应该时常alter index <INDEXNAME> rebuild一下。

3.4 其他要注意的地方
      不要使用Not，如goods_no != 2，要改为

where goods_no>2 or goods_no<2      不要使用is null , 如WHERE DEPT_CODE IS NOT NULL 要改为

WHERE DEPT_CODE >=0;3.5 select 的列如果全是索引列时
   又如果没有where 条件，或者where条件全部是索引列时，Oracle 将直接从索引里获取数据而不去读真实的数据表，这样子理论上会快很多，比如

select order_no,order_time from shop_order where shop_no=4当order_no,order_time,shop_no 这三列全为索引列时，你将看到一个和平时完全不同的执行计划。

3.6 位图索引
     传说中当数据值较少，比如某些表示分类、状态的列，应该建位图索引而不是普通的二叉树索引，否则效率低下。不过看执行计划，这些位图索引鲜有被Oracle临幸的。
 

4.减少查询往返和查询的表
这也是很简单的大道理，程序与Oracle交互的成本极高，所以一个查询能完成的不要分开两次查，如果一个循环执行１万条查询的，怎么都快不到哪里去了。

4.1 封装PL/SQL存储过程
最高级的做法是把循环的操作封装到PL/SQL写的存储过程里，因为存储过程都在服务端执行，所以没有数据往返的消耗。

4.2 封装PL/SQL内部函数
  有机会，将一些查询封装到函数里，而在普通SQL里使用这些函数，同样是很有效的优化。

4.3 Decode/Case
但存储过程也麻烦，所以有case/decode把几条条件基本相同的重复查询合并为一条的用法：

SELECT
 COUNT(CASE WHEN price < 13 THEN 1 ELSE null END) low,
 COUNT(CASE WHEN price BETWEEN 13 AND 15 THEN 1 ELSE null END) med,
 COUNT(CASE WHEN price > 15 THEN 1 ELSE null END) high
FROM products;4.4 一种Where/Update语法

SELECT TAB_NAME　FROM TABLES
WHERE (TAB_NAME,DB_VER) = （( SELECT TAB_NAME,DB_VER)
FROM TAB_COLUMNS WHERE VERSION = 604)

UPDATE EMP
SET (EMP_CAT, SAL_RANGE)
= (SELECT MAX(CATEGORY)FROM EMP_CATEGORIES)
5.其他优化
5.1RowID和ROWNUM
     连Hibernate 新版也支持ROWID了，证明它非常有用。比如号称删除重复数据的最快写法：

DELETE FROM EMP E
WHERE E.ROWID > (SELECT MIN(X.ROWID)
FROM EMP X
WHERE X.EMP_NO = E.EMP_NO);6.终极秘技 - Hints
   这是Oracle DBA的玩具，也是终极武器，比如Oracle在CBO,RBO中所做的选择总不合自己心水时，可以用它来强力调教一下Oracle，结果经常让人喜出望外。
   如果开发人员没那么多时间来专门学习它，可以依靠Toad SQL opmitzer 来自动生成这些提示，然后对比一下各种提示的实际效果。不过随着10g智能的进化，hints的惊喜少了。

7. 找出要优化的Top SQL
    磨了这么久的枪，如果找不到敌人是件郁闷的事情。
    幸亏10g这方面做得非常好。进入Web管理界面，就能看到当前或者任意一天的SQL列表，按性能排序。
    有了它，SQL Trace和TKPROF都可以不用了。

本文来自CSDN博客，转载请标明出处：http://blog.csdn.net/calvinxiu/archive/2005/11/15/529756.aspx

[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [找优秀程序员，就在博客园](http://job.cnblogs.com/)
IT新闻：
· [婚恋网世纪佳缘乱象重生 成一夜情猎场](http://news.cnblogs.com/n/112126/)
· [帝国时代 Online 已发布，可免费下载](http://news.cnblogs.com/n/112124/)
· [小米发布会中的亮点与尿点：“狗日的 1999”](http://news.cnblogs.com/n/112123/)
· [315投诉网疑被关后改名重开张 新网站否认](http://news.cnblogs.com/n/112122/)
· [团购网站融资冷却裁员求生 行业洗牌危机并存](http://news.cnblogs.com/n/112121/)    [博客园](http://www.cnblogs.com/)  [博问](http://home.cnblogs.com/q/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/freeman1984/archive/2010/10/10/334235.html&SourceURL=/freeman1984/archive/2010/10/10/334235.html)       [使用Ctrl+Enter键可以直接提交]    推荐职位：
· [广州ASP.NET程序员(广州丹霄信息技术)](http://job.cnblogs.com/offer/15741/)
· [上海 .NET软件工程师(上海苏秦网络)](http://job.cnblogs.com/offer/15624/)
· [上海.NET软件开发工程师(东方财富信息)](http://job.cnblogs.com/offer/15546/)
· [北京 SQL数据库开发工程师(圣特尔科技)](http://job.cnblogs.com/offer/14109/)
· [北京高新诚聘 ASP.NET 程序员(盈科融通软件)](http://job.cnblogs.com/offer/15489/)
· [北京 .NET软件工程师(北京科胜永昌)](http://job.cnblogs.com/Offer/15346/)
· [北京.NET 研发工程师 (北京捷报数据)](http://job.cnblogs.com/offer/5914/)
· [北京C#开发工程师 B/S方向(圣特尔科技)](http://job.cnblogs.com/offer/14108/)

博客园首页随笔：
· [C#设计模式——命令模式(Command Pattern)](http://www.cnblogs.com/saville/archive/2011/08/17/2142754.html)
· [老系统维护](http://www.cnblogs.com/ahl5esoft/archive/2011/08/17/2142739.html)
· [使用单例模式实现自己的HttpClient工具类](http://www.cnblogs.com/codingmyworld/archive/2011/08/17/2141706.html)
· [Spread for Windows Forms高级主题(2)---理解单元格类型](http://www.cnblogs.com/powertoolsteam/archive/2011/08/17/2142085.html)
· [ERP/MIS开发 菜单设计器(Menu Designer)及其B/S,C/S双重实现（B/S开源）](http://www.cnblogs.com/JamesLi2015/archive/2011/08/17/2142012.html)
知识库：
· [IT项目管理的六种错误思维](http://kb.cnblogs.com/page/104475/)
· [我的10个开发原则](http://kb.cnblogs.com/page/107415/)
· [大数据下的数据分析平台架构](http://kb.cnblogs.com/page/111704/)
· [为什么编程是独一无二的职业](http://kb.cnblogs.com/page/109493/)
· [分享8年开发经验，浅谈个人发展经历，明确自己发展方向](http://kb.cnblogs.com/page/104736/) 最简洁阅读版式：
[Oracle 10g SQL 优化再学习](http://archive.cnblogs.com/b/334235/) 网站导航:

[博客园](http://www.cnblogs.com/ "程序员的网上家园")   [IT新闻](http://news.cnblogs.com/)   [知识库](http://kb.cnblogs.com/)   [博客生活](http://www.cnweblog.com/)   [IT博客网](http://www.cnitblog.com/)   [C++博客](http://www.cppblog.com/)   [博问](http://space.cnblogs.com/q/ "IT问答")   [管理](http://www.blogjava.net/freeman1984/archive/2010/10/10/334235.html?opt=admin) 相关文章:

* [转载一篇文章oracle的一些操作](http://www.blogjava.net/freeman1984/archive/2011/08/03/355643.html)
* [过v$sqlarea,v$sql查询最占用资源的查询](http://www.blogjava.net/freeman1984/archive/2011/08/03/355634.html)
* [JVM内存模型以及垃圾回收](http://www.blogjava.net/freeman1984/archive/2011/03/08/345929.html)
* [加速Javascript：DOM操作优化](http://www.blogjava.net/freeman1984/archive/2010/10/17/335395.html)
* [Tomcat内存溢出的三种情况及解决办法分析](http://www.blogjava.net/freeman1984/archive/2010/10/16/335295.html)
* [Tomcat启动分析server.xml](http://www.blogjava.net/freeman1984/archive/2010/10/11/334485.html)
* [Java虚拟机参数 -XX等相关参数应用](http://www.blogjava.net/freeman1984/archive/2010/10/11/334483.html)
* [Oracle 10g SQL 优化再学习](http://www.blogjava.net/freeman1984/archive/2010/10/10/334235.html)
* [google Analytics 初探](http://www.blogjava.net/freeman1984/archive/2010/09/16/332234.html)
* [使用tomcat的compression来提高网页加载速度](http://www.blogjava.net/freeman1984/archive/2010/09/15/332121.html)   Powered by:
[BlogJava](http://www.blogjava.net/)
Copyright © @joe

![]()

### 公告

[](http://shop59092284.taobao.com/)[](http://shop59092284.taobao.com/)[** 老婆的淘宝,欢迎选购**](http://shop59092284.taobao.com/)

![]()

### 留言簿(2)

* [给我留言](http://www.blogjava.net/freeman1984/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/freeman1984/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/freeman1984/admin/MyMessages.aspx)
![]()

### 随笔分类

* [![]( "Subscribe to all 生活杂谈(14)")](http://www.blogjava.net/freeman1984/category/26087.html/rss "Subscribe to all 生活杂谈(14)")[all 生活杂谈(14)](http://www.blogjava.net/freeman1984/category/26087.html)
* [![]( "Subscribe to android(18)")](http://www.blogjava.net/freeman1984/category/42532.html/rss "Subscribe to android(18)")[android(18)](http://www.blogjava.net/freeman1984/category/42532.html)
* [![]( "Subscribe to apache项目(13)")](http://www.blogjava.net/freeman1984/category/46243.html/rss "Subscribe to apache项目(13)")[apache项目(13)](http://www.blogjava.net/freeman1984/category/46243.html)
* [![]( "Subscribe to chart(1)")](http://www.blogjava.net/freeman1984/category/43680.html/rss "Subscribe to chart(1)")[chart(1)](http://www.blogjava.net/freeman1984/category/43680.html)
* [![]( "Subscribe to concurrent(1)")](http://www.blogjava.net/freeman1984/category/49019.html/rss "Subscribe to concurrent(1)")[concurrent(1)](http://www.blogjava.net/freeman1984/category/49019.html)
* [![]( "Subscribe to database(34)")](http://www.blogjava.net/freeman1984/category/38740.html/rss "Subscribe to database(34)")[database(34)](http://www.blogjava.net/freeman1984/category/38740.html)
* [![]( "Subscribe to dwr(3)")](http://www.blogjava.net/freeman1984/category/42595.html/rss "Subscribe to dwr(3)")[dwr(3)](http://www.blogjava.net/freeman1984/category/42595.html)
* [![]( "Subscribe to flex(1)")](http://www.blogjava.net/freeman1984/category/43586.html/rss "Subscribe to flex(1)")[flex(1)](http://www.blogjava.net/freeman1984/category/43586.html)
* [![]( "Subscribe to hibernate(23)")](http://www.blogjava.net/freeman1984/category/42534.html/rss "Subscribe to hibernate(23)")[hibernate(23)](http://www.blogjava.net/freeman1984/category/42534.html)
* [![]( "Subscribe to java (123)")](http://www.blogjava.net/freeman1984/category/26086.html/rss "Subscribe to java (123)")[java (123)](http://www.blogjava.net/freeman1984/category/26086.html)
* [![]( "Subscribe to javafx(1)")](http://www.blogjava.net/freeman1984/category/41058.html/rss "Subscribe to javafx(1)")[javafx(1)](http://www.blogjava.net/freeman1984/category/41058.html)
* [![]( "Subscribe to java安全(6)")](http://www.blogjava.net/freeman1984/category/43930.html/rss "Subscribe to java安全(6)")[java安全(6)](http://www.blogjava.net/freeman1984/category/43930.html)
* [![]( "Subscribe to java性能(12)")](http://www.blogjava.net/freeman1984/category/43783.html/rss "Subscribe to java性能(12)")[java性能(12)](http://www.blogjava.net/freeman1984/category/43783.html)
* [![]( "Subscribe to jbpm(1)")](http://www.blogjava.net/freeman1984/category/38739.html/rss "Subscribe to jbpm(1)")[jbpm(1)](http://www.blogjava.net/freeman1984/category/38739.html)
* [![]( "Subscribe to jquery(3)")](http://www.blogjava.net/freeman1984/category/44698.html/rss "Subscribe to jquery(3)")[jquery(3)](http://www.blogjava.net/freeman1984/category/44698.html)
* [![]( "Subscribe to linux(6)")](http://www.blogjava.net/freeman1984/category/46405.html/rss "Subscribe to linux(6)")[linux(6)](http://www.blogjava.net/freeman1984/category/46405.html)
* [![]( "Subscribe to lucene(1)")](http://www.blogjava.net/freeman1984/category/47431.html/rss "Subscribe to lucene(1)")[lucene(1)](http://www.blogjava.net/freeman1984/category/47431.html)
* [![]( "Subscribe to lucene(1)")](http://www.blogjava.net/freeman1984/category/47801.html/rss "Subscribe to lucene(1)")[lucene(1)](http://www.blogjava.net/freeman1984/category/47801.html)
* [![]( "Subscribe to others(2)")](http://www.blogjava.net/freeman1984/category/26088.html/rss "Subscribe to others(2)")[others(2)](http://www.blogjava.net/freeman1984/category/26088.html)
* [![]( "Subscribe to questions(7)")](http://www.blogjava.net/freeman1984/category/38742.html/rss "Subscribe to questions(7)")[questions(7)](http://www.blogjava.net/freeman1984/category/38742.html)
* [![]( "Subscribe to questions_hander(5)")](http://www.blogjava.net/freeman1984/category/38741.html/rss "Subscribe to questions_hander(5)")[questions_hander(5)](http://www.blogjava.net/freeman1984/category/38741.html)
* [![]( "Subscribe to spring(24)")](http://www.blogjava.net/freeman1984/category/42533.html/rss "Subscribe to spring(24)")[spring(24)](http://www.blogjava.net/freeman1984/category/42533.html)
* [![]( "Subscribe to struts(8)")](http://www.blogjava.net/freeman1984/category/42535.html/rss "Subscribe to struts(8)")[struts(8)](http://www.blogjava.net/freeman1984/category/42535.html)
* [![]( "Subscribe to swing")](http://www.blogjava.net/freeman1984/category/42536.html/rss "Subscribe to swing")[swing](http://www.blogjava.net/freeman1984/category/42536.html)
* [![]( "Subscribe to UML(2)")](http://www.blogjava.net/freeman1984/category/48926.html/rss "Subscribe to UML(2)")[UML(2)](http://www.blogjava.net/freeman1984/category/48926.html)
* [![]( "Subscribe to web(33)")](http://www.blogjava.net/freeman1984/category/44672.html/rss "Subscribe to web(33)")[web(33)](http://www.blogjava.net/freeman1984/category/44672.html)
* [![]( "Subscribe to webservice(5)")](http://www.blogjava.net/freeman1984/category/38743.html/rss "Subscribe to webservice(5)")[webservice(5)](http://www.blogjava.net/freeman1984/category/38743.html)
* [![]( "Subscribe to xml(2)")](http://www.blogjava.net/freeman1984/category/43702.html/rss "Subscribe to xml(2)")[xml(2)](http://www.blogjava.net/freeman1984/category/43702.html)
* [![]( "Subscribe to 敏捷(6)")](http://www.blogjava.net/freeman1984/category/46247.html/rss "Subscribe to 敏捷(6)")[敏捷(6)](http://www.blogjava.net/freeman1984/category/46247.html)
* [![]( "Subscribe to 方法论(14)")](http://www.blogjava.net/freeman1984/category/46503.html/rss "Subscribe to 方法论(14)")[方法论(14)](http://www.blogjava.net/freeman1984/category/46503.html)
* [![]( "Subscribe to 架构(10)")](http://www.blogjava.net/freeman1984/category/46575.html/rss "Subscribe to 架构(10)")[架构(10)](http://www.blogjava.net/freeman1984/category/46575.html)
* [![]( "Subscribe to 网络通讯(5)")](http://www.blogjava.net/freeman1984/category/49063.html/rss "Subscribe to 网络通讯(5)")[网络通讯(5)](http://www.blogjava.net/freeman1984/category/49063.html)
* [![]( "Subscribe to 项目管理(17)")](http://www.blogjava.net/freeman1984/category/46246.html/rss "Subscribe to 项目管理(17)")[项目管理(17)](http://www.blogjava.net/freeman1984/category/46246.html)

![]()

### 相册

* [我的相册](http://www.blogjava.net/freeman1984/gallery/42537.html)
![]()

### 搜索

*  

![]()

### 积分与排名

* 积分 - 249820
* 排名 - 69
![]()

### 最新评论 [![]()](http://www.blogjava.net/freeman1984/CommentsRSS.aspx)

* [1. re: 一篇不错的讲解Java异常的文章(转载）----感觉很不错，读了以后很有启发[未登录]](http://www.blogjava.net/freeman1984/archive/2011/08/08/148850.html#356038)
* 受教了。非常感谢。
不过，后面罗列的那么多异常有点不必要了。
* --jake
* [2. re: strtuts2 异常之Could not create and/or set value back on to object [未登录]](http://www.blogjava.net/freeman1984/archive/2011/07/25/324133.html#355011)
* thank you very much！
* --fighting
* [3. re: Android屏幕元素层次结构[未登录]](http://www.blogjava.net/freeman1984/archive/2011/07/22/301081.html#354834)
* <p style="color:red">顶一下</p>
* --黑石
* [4. re: 不同技术团队的配合问题及DevOps(不错的文章，来自infoq)](http://www.blogjava.net/freeman1984/archive/2011/07/21/354712.html#354750)
* 做个记号。
* --何杨
* [5. re: other[未登录]](http://www.blogjava.net/freeman1984/archive/2011/07/12/154309.html#354154)
* 希望学习，谢谢！
trust_myself@126.com
* --asa

![]()

### 阅读排行榜

* [1. 一篇不错的讲解Java异常的文章(转载）----感觉很不错，读了以后很有启发(17440)](http://www.blogjava.net/freeman1984/archive/2007/09/27/148850.html)
* [2. 世界上最健康的作息时间表 大家对比下(16717)](http://www.blogjava.net/freeman1984/archive/2009/11/05/301262.html)
* [3. ibatis学习（三）---ibatis与spring的整合(13592)](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html)
* [4. 如何用javascript控制checkbox，并进行批量删除(8527)](http://www.blogjava.net/freeman1984/archive/2007/09/24/147879.html)
* [5. ibatis学习（二）--ibatis使用介绍(6368)](http://www.blogjava.net/freeman1984/archive/2007/12/07/166113.html)
![]()

### 评论排行榜

* [1. other(41)](http://www.blogjava.net/freeman1984/archive/2007/10/19/154309.html)
* [2. 一篇不错的讲解Java异常的文章(转载）----感觉很不错，读了以后很有启发(22)](http://www.blogjava.net/freeman1984/archive/2007/09/27/148850.html)
* [3. ssh中利用pager-taglib和filter进行分页(14)](http://www.blogjava.net/freeman1984/archive/2007/11/16/161093.html)
* [4. 团队内是有必要统一IDE(14)](http://www.blogjava.net/freeman1984/archive/2010/11/12/337878.html)
* [5. 世界上最健康的作息时间表 大家对比下(13)](http://www.blogjava.net/freeman1984/archive/2009/11/05/301262.html)

### 60天内阅读排行

* [1. 大家都用什么bug管理软件？(2031)](http://www.blogjava.net/freeman1984/archive/2011/06/20/352649.html)
* [2. CountDownLatch 简介和例子(1412)](http://www.blogjava.net/freeman1984/archive/2011/07/04/353654.html)
* [3. 统计指标和术语汇总(1396)](http://www.blogjava.net/freeman1984/archive/2011/08/04/355820.html)
* [4. 不同技术团队的配合问题及DevOps(不错的文章，来自infoq)(1321)](http://www.blogjava.net/freeman1984/archive/2011/07/20/354712.html)
* [5. connection.release_mode，connection.autocommit和transaction.auto_close_session用法(1003)](http://www.blogjava.net/freeman1984/archive/2011/08/04/355808.html)
