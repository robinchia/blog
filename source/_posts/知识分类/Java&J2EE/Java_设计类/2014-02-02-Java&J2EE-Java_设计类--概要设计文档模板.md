---
layout: post
title: "概要设计文档模板"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_设计类
--- 

# 概要设计文档模板

![]()

[campo](http://www.cnblogs.com/campo/)
态度和细节决定一切 专注 务实 探索
随笔- 55  文章- 2  评论- 151 

[博客园](http://www.cnblogs.com/)  [首页](http://www.cnblogs.com/campo/)  [新随笔](http://www.cnblogs.com/campo/admin/EditPosts.aspx?opt=1)  [联系](http://space.cnblogs.com/msg/send/campo)  [管理](http://www.cnblogs.com/campo/admin/EditPosts.aspx)  [订阅](http://www.cnblogs.com/campo/rss) [![订阅]()](http://www.cnblogs.com/campo/rss)
# [概要设计文档模板](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html)

**[]()概要设计说明书** **一． 引言** 1． 编写目的 ![]()500){this.resized=true;this.style.width=500;}" resized="0">从该阶段开发正式进入软件的实际开发阶段，本阶段完成系统的大致设计并明确系统的数据结构与软件结构。在软件设计阶段主要是把一个软件需求转化为软件表示的过程，这种表示只是描绘出软件的总的概貌。本概要设计说明书的目的就是进一步细化软件设计阶段得出的软件总体概貌，把它加工成在程序细节上非常接近于源程序的软件表示。 2． 项目背景（略） 3． 定义 ![]()500){this.resized=true;this.style.width=500;}" resized="0">在该概要设计说明书中的专门术语有：
![]()500){this.resized=true;this.style.width=500;}" resized="0">**总体设计********![]()500){this.resized=true;this.style.width=500;}" resized="0">接口设计**

**![]()500){this.resized=true;this.style.width=500;}" resized="0">数据结构设计**

**![]()500){this.resized=true;this.style.width=500;}" resized="0">运行设计**

**![]()500){this.resized=true;this.style.width=500;}" resized="0">出错设计**
![]()500){this.resized=true;this.style.width=500;}" resized="0">具体的概念与含义在文档后将会解释。 4． 参考资料 ![]()500){this.resized=true;this.style.width=500;}" resized="0"><软件工程概论> 李存珠 李宣东 编著 南京大学[计算机](http://www.mflw.com/search.aspx?keyword=%BC%C6%CB%E3%BB%FA&where=title "电脑和计算机方面")系出版 2001年8月 **二． 任务概述** 1． 目标 ![]()500){this.resized=true;this.style.width=500;}" resized="0">该阶段目的在于明确系统的数据结构和软件结构，此外总体设计还将给出内部软件和外部系统部件之间的接口定义，各个软件模块的功能说明，数据结构的细节以及具体的装配要求。 2． 运行环境 ![]()500){this.resized=true;this.style.width=500;}" resized="0">软件基本运行环境为Windows环境。 3． 需求概述（略） 4． 条件与限制 ![]()500){this.resized=true;this.style.width=500;}" resized="0">为了评价该设计阶段的设计表示的“优劣程度”，必须遵循以下几个准则：
![]()500){this.resized=true;this.style.width=500;}" resized="0">1.软件设计应当表现出层次结构，它应巧妙地利用各个软件部件之间的控制关系。
![]()500){this.resized=true;this.style.width=500;}" resized="0">2.设计应当是模块化的，即该软件应当从逻辑上被划分成多个部件，分别实现各种特定功能和子功能。
![]()500){this.resized=true;this.style.width=500;}" resized="0">3.设计最终应当给出具体的模块（例如子程序或过程），这些模块就具有独立的功能特性。
![]()500){this.resized=true;this.style.width=500;}" resized="0">4.应当应用在软件需求分析期间得到的信息，采取循环反复的方法来获得设计。 **三． 总体设计** 1．处理流程 ![]()500){this.resized=true;this.style.width=500;}" resized="0">系统的总体处理数据流程如下图： ![]()500){this.resized=true;this.style.width=500;}" resized="0"> **图八**![]()500){this.resized=true;this.style.width=500;}" resized="0">**总体处理流程图**   2．总体结构和模块外部设计 ![]()500){this.resized=true;this.style.width=500;}" resized="0">模块是软件结构的基础，软件结构的好坏完全由模块的属性体现出来，把软件模块化的目的是为了降低软件复杂性，使软件设计，测试，调试，维护等工作变得简易，但随着模块数目的增加，通过接口连接这些模块的工作量也随之增加。从这些特性可得出如图九的一条总的成本（或工作量）曲线，在考虑模块化时，应尽量使模块数接近于图中的M，它使得研制成本最小，而且应尽量避免不足的模块化或超量。 ![]()500){this.resized=true;this.style.width=500;}" resized="0"> **图九**![]()500){this.resized=true;this.style.width=500;}" resized="0">**模块化与总体成本** 3．功能分配 ![]()500){this.resized=true;this.style.width=500;}" resized="0">从程序的结构中可以看出，学生的信息输入输出功能是由学生管理系统进行的。课程的信息输入输出是由课程管理系统进行的，而班级的信息流动则是班级管理系统进行的。 **四． 接口设计** ![]()500){this.resized=true;this.style.width=500;}" resized="0">由于系统的各种内外部接口是通过借助数据库开发软件来实现的，是完全在数据库内部操作的，故在此略过此内容。 1． 外部接口（略） 2． 内部接口（略） **五． 数据结构设计** 1． 逻辑结构设计 **student_Info 学生基本信息表** **列名** **数据类型** **可否为空** **说明** student_ID INT(4) NOT NULL 学生学号（主键） student_Name CHAR(10) NULL 学生姓名 student_Gender CHAR(2) NULL 学生性别 born_Date DATETIME(8) NULL 出生日期 class_No INT(4) NULL 班号 tele_Number CHAR(10) NULL 联系电话 ru_Date DATETIME(8) NULL 入校时间 address VARCHAR(50) NULL 家庭住址 comment VARCHAR(200) NULL 注释   **class_Info 班级信息表格** **列名** **数据类型** **可否为空** **说明** class_No INT(4) NOT NULL 班号(主键) grade CHAR(10) NULL 年级 Director CHAR(10) NULL 班主任 Classroom_No CHAR(10) NULL 教室   **course_Info 课程基本信息表** **列名** **数据类型** **可否为空** **说明** course_No INT(4) NOT NULL 课程编号(主键) course_Name CHAR(10) NULL 课程名称 course_Type CHAR(10) NULL 课程类型 course_Des CHAR(50) NULL 课程描述   **gradecourse_Info 年级课程设置表** **列名** **数据类型** **可否为空** **说明** grade CHAR(10) NULL 年级 course_Name CHAR(10) NULL 课程名称   **result_Info 学生成绩信息表** **列名** **数据类型** **可否为空** **说明** exam_No CHAR(10) NOT NULL 考试编号 student_ID INT(4) NOT NULL 学生学号 student_Name CHAR(10) NULL 学生姓名 class_No INT(4) NULL 学生班号 course_Name CHAR(10) NULL 课程名称 result FLOAT(8) NULL 分数   **user_Info 系统用户表** **列名** **数据类型** **可否为空** **说明** user_ID CHAR(10) NOT NULL 用户名称（主键） user_PWD CHAR(10) NULL 用户密码 user_DES CHAR(10) NULL 用户描述 **图十**![]()500){this.resized=true;this.style.width=500;}" resized="0">**数据库逻辑结构图表** 2． 物理结构设计 ![]()500){this.resized=true;this.style.width=500;}" resized="0">系统的物理结构具体由数据库来设计与生成，此处略。 3． 数据结构与程序的关系 ![]()500){this.resized=true;this.style.width=500;}" resized="0">系统的数据结构由标准数据库语言SQL生成。
![]()500){this.resized=true;this.style.width=500;}" resized="0">具体的例如创建系统用户表格 user_Info的程序用SQL表示就是：
![]()500){this.resized=true;this.style.width=500;}" resized="0">**CREATE TABLE[dbo].[user_Info](
![]()500){this.resized=true;this.style.width=500;}" resized="0">![]()500){this.resized=true;this.style.width=500;}" resized="0">[user_ID][char](10)COLLATE Chinese_PRC_CI_AS NOT NULL,
![]()500){this.resized=true;this.style.width=500;}" resized="0">![]()500){this.resized=true;this.style.width=500;}" resized="0">[user_PWD][char](10)COLLATE Chinese_PRC_CI_AS NULL,
![]()500){this.resized=true;this.style.width=500;}" resized="0">![]()500){this.resized=true;this.style.width=500;}" resized="0">[user_Des][char](10)COLLATE Chinese_PRC_CI_AS NULL
![]()500){this.resized=true;this.style.width=500;}" resized="0">) ON [PRIMARY]** **六． 运行设计** 1． 运行模块的组合 **![]()500){this.resized=true;this.style.width=500;}" resized="0">**具体软件的运行模块组合为程序多窗口的运行环境，各个模块在软件运行过程中能较好的交换信息，处理数据。 2． 运行控制 **![]()500){this.resized=true;this.style.width=500;}" resized="0">**软件运行时有较友好的界面，基本能够实现用户的数据处理要求。 3． 运行时间 **![]()500){this.resized=true;this.style.width=500;}" resized="0">**系统的运行时间基本可以达到用户所提出的要求。 **七． 出错处理设计** 1． 出错输出信息 **![]()500){this.resized=true;this.style.width=500;}" resized="0">**在用户使用错误的数据或访问没有权限的数据后，系统给出提示：“对不起，你非法使用数据，没有权限！”而且用户的密码管理可以允许用户修改自己的密码，不允许用户的匿名登录。 2． 出错处理对策 **![]()500){this.resized=true;this.style.width=500;}" resized="0">**由于数据在数据库中已经有备份，故在系统出错后可以依靠数据库的恢复功能，并且依靠日志文件使系统再启动，就算系统崩溃用户数据也不会丢失或遭到破坏。但有可能占用更多的数据存储空间，权衡措施由用户来决定。 **八． 安全保密设计** **![]()500){this.resized=true;this.style.width=500;}" resized="0">**系统的系统用户管理保证了只有授权的用户才能进入系统进行数据操作，而且对一些重要数据，系统设置为只有更高权限的人员方可读取或是操作。系统安全保密性较高。 **九． 维护设计** **![]()500){this.resized=true;this.style.width=500;}" resized="0">**由于系统较小没有外加维护模块，因为维护工作比较简单，仅靠数据库的一些基本维护

0

0
0

(请您对文章做出评价)
[«](http://www.cnblogs.com/campo/archive/2007/07/26/832590.html)上一篇：[windows文章串联](http://www.cnblogs.com/campo/archive/2007/07/26/832590.html "发布于2007-07-26 17:53")
[»](http://www.cnblogs.com/campo/archive/2007/08/24/867928.html)下一篇：[概要设计模板](http://www.cnblogs.com/campo/archive/2007/08/24/867928.html "发布于2007-08-24 10:36")

posted @ 2007-08-24 10:33 [campo](http://www.cnblogs.com/campo/) 阅读(6961) [评论(3)](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#commentform)  [编辑](http://www.cnblogs.com/campo/admin/EditPosts.aspx?postid=867923) [收藏](http://www.cnblogs.com/campo/AddToFavorite.aspx?id=867923) [网摘](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#)
![]()[]()

发表评论

1600226
  [回复](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#commentform)  [引用](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#commentform)  []()  []()[#1楼](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#1013155)[]()2007-12-24 20:52 | [fghfdsgh[未注册用户]](http://www.cnitblog.com/r.aspx?url=http://dgdfg)

gsdfgdf
;asdlf
laji

  [回复](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#commentform)  [引用](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#commentform)  []()  []()[#2楼](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#1600225)[]()2009-07-30 14:58 | [chris.wang[未注册用户]]()

十分感谢您发布的概要设计文档，对我的工作提供了很大的帮助。
祝您工作顺利！
  [回复](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#commentform)  [引用](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#commentform)  []()  []()[#3楼](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#1600226)[]()[]()2009-07-30 14:58 | [chris.wang[未注册用户]]()

十分感谢您发布的概要设计文档，对我的工作提供了很大的帮助。
祝您工作顺利！
注册用户登录后才能发表评论，请 [登录](http://passport.cnblogs.com/login.aspx?ReturnUrl=http%3a%2f%2fwww.cnblogs.com%2fcampo%2farchive%2f2007%2f08%2f24%2f867923.html%3flogin%3d1%23commentform) 或 [注册](http://passport.cnblogs.com/register.aspx?ReturnUrl=http%3a%2f%2fwww.cnblogs.com%2fcampo%2farchive%2f2007%2f08%2f24%2f867923.html%23Bottom2) 。

[博客园首页](http://www.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [闪存](http://home.cnblogs.com/ing/)  [知识库](http://kb.cnblogs.com/)  [招聘](http://job.cnblogs.com/)

[IT新闻](http://news.cnblogs.com/):
· [给Oracle支招：改善Java的15种方式](http://news.cnblogs.com/n/57476/)
· [谷歌中国面临人才流失 猎头公司挖走多名员工](http://news.cnblogs.com/n/57475/)
· [惠普正式开放新加坡云计算实验室](http://news.cnblogs.com/n/57474/)
· [微软向美国联邦政府提供云计算服务](http://news.cnblogs.com/n/57473/)
· [中国3G运营商 真正的大考才刚开始](http://news.cnblogs.com/n/57472/)
[每天10分钟，轻松学英语](http://a4.yeshj.com/rd/34138/)
专题：[iPad](http://kb.cnblogs.com/zt/iPad/)  [jQuery](http://kb.cnblogs.com/zt/jquery/)  [Windows 7](http://kb.cnblogs.com/zt/windows7/)

网站导航：
[博客园首页](http://www.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [个人主页](http://home.cnblogs.com/)  [闪存](http://home.cnblogs.com/ing/)  [程序员招聘](http://job.cnblogs.com/)  [社区](http://space.cnblogs.com/)  [博问](http://space.cnblogs.com/q/)  [网摘](http://wz.cnblogs.com/)
[![]()](http://www.china-pub.com/STATIC07/0912/zh_ndcx_091212.asp)
[China-pub 计算机图书网上专卖店！6.5万品种2-8折！](http://www.china-pub.com/itbook/)
[China-Pub 计算机绝版图书按需印刷服务](http://www.china-pub.com/static07/0901/zh_jueba_090121.asp)

**在知识库中查看：**
[概要设计文档模板](http://kb.cnblogs.com/a/867923/)

Copyright ©2010 campo

[我的主页](http://home.cnblogs.com/campo/)  [个人资料](http://home.cnblogs.com/campo/detail/)
[我的闪存](http://home.cnblogs.com/campo/ing/)  [发短消息](http://space.cnblogs.com/msg/send/campo)

[<]( "Go to the previous month") 2007年8月 [>]( "Go to the next month") 日 一 二 三 四 五 六 29 30 31 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 [24](http://www.cnblogs.com/campo/archive/2007/8/24.html) 25 26 27 28 29 [30](http://www.cnblogs.com/campo/archive/2007/8/30.html) 31 1 2 3 4 5 6 7 8
### 搜索

 

 

### 常用链接

* [我的随笔](http://www.cnblogs.com/campo/MyPosts.html)
* [我的空间](http://home.cnblogs.com/campo/)
* [我的短信](http://space.cnblogs.com/msg/recent)
* [我的评论](http://www.cnblogs.com/campo/MyComments.html)
* [更多链接](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#)
* [我的参与](http://www.cnblogs.com/campo/OtherPosts.html "我发表过评论的随笔")
* [我的新闻](http://www.cnblogs.com/campo/MyNews.html)
* [最新评论](http://www.cnblogs.com/campo/RecentComments.html)
* [我的标签](http://www.cnblogs.com/campo/tag/)
### 我的标签

* [0x80040155 接口没有注册](http://www.cnblogs.com/campo/tag/0x80040155++æ¥å£æ²¡ææ³¨å/)(1)

# 随笔分类

* [Asp.Net Ajax](http://www.cnblogs.com/campo/category/133119.html)[![]( "Subscribe to Asp.Net Ajax")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to Asp.Net Ajax")
* [CodeSmith](http://www.cnblogs.com/campo/category/122183.html)[![]( "Subscribe to CodeSmith")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to CodeSmith")
* [DotNet(9)](http://www.cnblogs.com/campo/category/78191.html)[![]( "Subscribe to DotNet(9)")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to DotNet(9)")
* [Enterprise Library](http://www.cnblogs.com/campo/category/122184.html)[![]( "Subscribe to Enterprise Library")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to Enterprise Library")
* [Java(1)](http://www.cnblogs.com/campo/category/78193.html)[![]( "Subscribe to Java(1)")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to Java(1)")
* [Javascript小技巧(1)](http://www.cnblogs.com/campo/category/125077.html)[![]( "Subscribe to Javascript小技巧(1)")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to Javascript小技巧(1)")
* [Linux(1)](http://www.cnblogs.com/campo/category/95054.html)[![]( "Subscribe to Linux(1)")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to Linux(1)")
* [OpenCms](http://www.cnblogs.com/campo/category/99054.html)[![]( "Subscribe to OpenCms")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to OpenCms")
* [PS(1)](http://www.cnblogs.com/campo/category/105128.html)[![]( "Subscribe to PS(1)")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to PS(1)")
* [程序员必经之路(2)](http://www.cnblogs.com/campo/category/105561.html)[![]( "Subscribe to 程序员必经之路(2)")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to 程序员必经之路(2)")
* [代码生产](http://www.cnblogs.com/campo/category/133120.html)[![]( "Subscribe to 代码生产")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to 代码生产")
* [软件工程](http://www.cnblogs.com/campo/category/78195.html)[![]( "Subscribe to 软件工程")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to 软件工程")
* [数据库(5)](http://www.cnblogs.com/campo/category/78194.html)[![]( "Subscribe to 数据库(5)")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to 数据库(5)")

# 随笔档案

* [2008年9月 (1)](http://www.cnblogs.com/campo/archive/2008/09.html)
* [2008年6月 (1)](http://www.cnblogs.com/campo/archive/2008/06.html)
* [2008年3月 (2)](http://www.cnblogs.com/campo/archive/2008/03.html)
* [2008年1月 (1)](http://www.cnblogs.com/campo/archive/2008/01.html)
* [2007年11月 (3)](http://www.cnblogs.com/campo/archive/2007/11.html)
* [2007年10月 (4)](http://www.cnblogs.com/campo/archive/2007/10.html)
* [2007年9月 (9)](http://www.cnblogs.com/campo/archive/2007/09.html)
* [2007年8月 (3)](http://www.cnblogs.com/campo/archive/2007/08.html)
* [2007年7月 (7)](http://www.cnblogs.com/campo/archive/2007/07.html)
* [2007年6月 (2)](http://www.cnblogs.com/campo/archive/2007/06.html)
* [2007年5月 (1)](http://www.cnblogs.com/campo/archive/2007/05.html)
* [2007年4月 (2)](http://www.cnblogs.com/campo/archive/2007/04.html)
* [2007年2月 (2)](http://www.cnblogs.com/campo/archive/2007/02.html)
* [2007年1月 (2)](http://www.cnblogs.com/campo/archive/2007/01.html)

# 文章分类

* [编程知识](http://www.cnblogs.com/campo/category/80348.html)[![]( "Subscribe to 编程知识")](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html### "Subscribe to 编程知识")

# 相册

* [basketball](http://www.cnblogs.com/campo/gallery/105746.html)

# 编程技术文章

* [C#学习笔记之程序集](http://www.cnblogs.com/3echo/archive/2006/02/14/330579.html)
* [Jeffrey Zhao](http://www.cnblogs.com/JeffreyZhao/)
* [NickLee](http://mail-ricklee.cnblogs.com/)
* [沉香](http://blog.sina.com.cn/campoem)
* [大胡仔](http://www.cnblogs.com/ruxpinsp1/)
### 最新评论[![]( "RSS订阅最最新评论")](http://www.cnblogs.com/campo/CommentsRSS.aspx "RSS订阅最最新评论")

[1. Re:概要设计文档模板](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#1600226)

十分感谢您发布的概要设计文档，对我的工作提供了很大的帮助。祝您工作顺利！ (chris.wang)
[2. Re:概要设计文档模板](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html#1600225)

十分感谢您发布的概要设计文档，对我的工作提供了很大的帮助。祝您工作顺利！ (chris.wang)
[3. re: 2007IT业薪资调查，请注明城市](http://www.cnblogs.com/campo/archive/2007/10/05/914504.html#1429366)

4000 应届 北京 (飞林沙)
[4. re: 2007IT业薪资调查，请注明城市](http://www.cnblogs.com/campo/archive/2007/10/05/914504.html#1300074)

1500 一年 青岛 唉 (随&风)
[5. re: 2007IT业薪资调查，请注明城市](http://www.cnblogs.com/campo/archive/2007/10/05/914504.html#1299291)

6K-8K。 (养猪设备)

### 阅读排行榜

* [1. 概要设计文档模板(6961)](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html)
* [2. 2007IT业薪资调查，请注明城市(6651)](http://www.cnblogs.com/campo/archive/2007/10/05/914504.html)
* [3. sqlserver2000下载地址(5108)](http://www.cnblogs.com/campo/archive/2007/09/07/885354.html)
* [4. 概要设计模板(4755)](http://www.cnblogs.com/campo/archive/2007/08/24/867928.html)
* [5. SQLServer2000安装图解(3051)](http://www.cnblogs.com/campo/archive/2007/09/07/885342.html)
### 评论排行榜

* [1. 2007IT业薪资调查，请注明城市(138)](http://www.cnblogs.com/campo/archive/2007/10/05/914504.html)
* [2. 概要设计文档模板(3)](http://www.cnblogs.com/campo/archive/2007/08/24/867923.html)
* [3. 工作流技术学习专题(3)](http://www.cnblogs.com/campo/archive/2007/06/28/798991.html)
* [4. 用例 UseCase(2)](http://www.cnblogs.com/campo/archive/2007/09/10/888924.html)
* [5. 往消息队列传数据的存储过程(2)](http://www.cnblogs.com/campo/archive/2008/01/07/1029353.html)

msn:campolake@hotmail.com
