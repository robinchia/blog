---
layout: post
title: "Oracle Start Up 2 Oracle 框架构件、启动、解决一个问题 - Kylin So"
categories: DB
tags: 
 - DB
 - oracle
 - oracle-
--- 

# Oracle Start Up 2 Oracle 框架构件、启动、解决一个问题 - Kylin Soong Blog - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [群组](http://www.iteye.com/groups) [更多 ▼](http://kylinsoong.iteye.com/blog/776654#)

[招聘](http://www.iteye.com/job) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://kylinsoong.iteye.com/login "登录") [我的应用](http://www.iteye.com/all) [登录](http://kylinsoong.iteye.com/login) [注册](http://kylinsoong.iteye.com/signup)

# [Kylin Soong Blog](http://kylinsoong.iteye.com/)

永久域名 [http://kylinsoong.iteye.com/](http://kylinsoong.iteye.com/)

8顶
0踩

[Oracle Start Up 3：Oracle数据库基础](http://kylinsoong.iteye.com/blog/777540 "Oracle Start Up 3：Oracle数据库基础 ")| [Oracle Start Up 1： 几个概念和Oracle数 ...](http://kylinsoong.iteye.com/blog/775518 "Oracle Start Up 1： 几个概念和Oracle数据库的物理结构和逻辑结构")

2010-10-02

### [Oracle Start Up 2 Oracle 框架构件、启动、解决一个问题](http://kylinsoong.iteye.com/blog/776654) **

**博客分类：**
* [DATABASE](http://kylinsoong.iteye.com/category/125064)
[Oracle](http://www.iteye.com/blogs/tag/Oracle)[UP](http://www.iteye.com/blogs/tag/UP)[框架](http://www.iteye.com/blogs/tag/%E6%A1%86%E6%9E%B6)[数据结构](http://www.iteye.com/blogs/tag/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)[SQL](http://www.iteye.com/blogs/tag/SQL)
Warming Up:

      本文适合Oracle初学者。

      在[Oracle Start Up 1](http://kylinsoong.iteye.com/admin/blogs/775518)中我说了几个概念和Oracle数据库的结构，当然在Oracle Start Up 1 里面主要说明的是Oracle数据库的结构（物理组成和逻辑结构）。

      本文解决一个初学者很可能常遇到的问题（我遇到了，花了好久才解决）。

      本文一切测试实验基于Oracle 10g 企业版。

**1. 先描述一个连接Oracle 10g的错误：“shared memory realm does not exist”**
![]()
 如图所示Sqlplus连接时出现这个错误；

关于此错误的详细描述参见：[http://kylinsoong.iteye.com/blog/774091](http://kylinsoong.iteye.com/blog/774091 "http://kylinsoong.iteye.com/blog/774091")，本文最后解决这个问题；

 

**2. Oracle 服务器主要组件分析**

下图所示为Oracle服务器主要组件：

![]()
      如上图所示：Oracle服务器的组件结构，Oracle服务器可以看做由**两部分组成：Oracle实例和Oracle数据库，**上图被加粗直线分为两个部分，直线上半部分表示Oracle实例，而直线之下表示Oracle数据库。如[Oracle Start Up 1](http://kylinsoong.iteye.com/admin/blogs/775518)中所说，Oracle数据库在物理上可以看做是由不同文件组成的文件系统，从逻辑上可以看做是一个由TableSpace、Segment、Extent、Blocks组成的四维结构；

**关于Oracle实例：**

      是访问Oracle数据库的一种方法，一个Oracle实例只可以打开一个Oracle数据库；

      由内存和后台进程组成；

      存在于内存中，一个数据库对应一个共享的内存区，共享的内存区被Oracle后台进程所共享；

      建立与客户端的连接（连接到Oracle服务器实质上与Oracle服务器的Oracle实例建立连接）

从以下几个方面分解上图：

(1)Oracle两大进程

![]()
 

 用户进程：

      在客户机内存上运行的程序,用来访问Oracle数据库；     

      与数据库实例建立连接，不能直接与数据库直接连接；

      如在客户机上运行的 SQL PLUS,企业管理器;

      用户进程向服务进程提出操作请求；

服务器进程：

      一个直接与数据库服务器对话的进程；

      响应用户进程提出的操作请求，并返回结果；

(2) Oracle的主要内存结构包括两个部分
![]()

程序全局区Program Global Area (PGA): 当服务器进程启动的时候分配。   

      每个用户连接到Oracle数据库的内存区域；

      当进程建立的时候单独分配；

      当进程终止的时候释放；

      只能用于一个进程,是私有的，不能够共享

系统全局区System Global Area (SGA): 在实例启动的时候分配, 是数据库实例的基本组成部分，从结构框架图中分离出SGA部分如下图：

![]()

SGA包括几个内存结构:

      共享池（Shared Pool）
      数据库缓冲区快速缓存（Database Buffer Cache）
      重做日志缓冲区（Redo Log Buffer ）

      大池（Large Pool）
      Java池（Java Pool）
      其它结构 (锁（lock）、锁存管理 （latch）、统计数据（statistical data）)
 SGA几个特性：

      动态分配
      由SGA_MAX_SIZE确定大小
      有时称为共享全局区域(Shared Global Area)
       在SGA内以内存颗粒（ granules ）进行分配
SGA各内存部分功能键下表：

Library Cache
 
库快速缓冲区
存储最近使用的SQL和pl/sql语句的信息

共享最近使用过的SQL语句

通过“最近最少使用”（ least recently used (LRU) ）算法来管理
包括两种结构:共享SQL区和共享PL/SQL区
大小由共享池（Shared Pool）的大小确定 Data Dictionary Cache 数据字典缓冲区
数据库中最近使用过的定义的一个集合
包括数据库文件、表、索引、列、用户、权限、和其它对象的信息
在解析阶段，服务器进程寻找数据字典信息来解析对象的名字和访问权限
把数据字典的信息加载到内存里面，来提高查询和DML(Data Manipulation Language)数据操纵语言语句的反应时间
大小由共享池（Shared Pool）的大小确定 Database Buffer Cache 数据库缓冲区快速缓存
存储从数据文件提取出来的数据块的一个拷贝
当提取数据或者修改数据的时候，能很大的提高性能
通过LRU(Least Recently Used )算法来管理
DB_BLOCK_SIZE决定了主数据块的大小 Redo Log Buffer 重做日志缓冲区
记录了对数据块的所有改变
主要的目的是为了恢复数据库
改变被记录在成为重做目录的对象里面（redo entries）
重做目录（Redo entries）包含重建或者重做改变的信息
大小通过LOG_BUFFER来定义 Java Pool Java池
用于解析java命令
在安装和使用java的时候需要使用
大小确定：JAVA_POOL_SIZE Large Pool 大池
SGA中可选的一块内存区域
减少了共享池（Shared Pool）的负担

(3)Oracle实例的后台进程

![]()
 感觉挺复杂，将在以后做专门说明；

 

**3.Oracle数据库的启动**

要启动和关闭数据库，必须要以具有Oracle 管理员权限的用户登陆，通常也就是以具有SYSDBA权限的用户登陆
启动一个数据库需要三个步骤：
(1)、 创建一个Oracle实例（非安装阶段）
(2)、 由实例安装数据库（安装阶段）
(3)、 打开数据库（打开阶段）

下面从实验的角度来实践这三个阶段：

**Step one：以具有Oracle 管理员权限的用户登陆**

sqlplus命令
sqlplus /nolog conn USER/PASSWORD as sysdba

截图：
![]()
 **Step two：创建一个Oracle实例（非安装阶段）
**sqlplus命令

startup nomount

截图：
![]()
       如上所示：NONOUNT选项仅仅创建一个Oracle实例。读取init.ora初始化参数文件、启动后台进程、初始化系统全局区（SGA）。Init.ora文件定义了实例的配置，包括内存结构的大小和启动后台进程的数量和类型等。当实例打开后，系统将显示一个SGA内存结构和大小的列表，如上截图所示

**Step three： 由实例安装数据库（安装阶段）**
命令：
alter database mount;

截图：
![]()
      该命令创建实例并且安装数据库，但没有打开数据库。Oracle系统读取控制文件中关于数据文件和重作日志文件的内容，但并不打开该文件。

**Step four：打开数据库（打开阶段）**

命令：
alter database open;

截图：
![]()

      该命令完成创建实例、安装实例和打开数据库的所有三个步骤。此时数据库使数据文件和重作日志文件在线，通常还会请求一个或者是多个回滚段。这时系统除了可以看到前面Startup Mount方式下的所有提示外，还会给出一个"数据库已经打开"的提示。此时，数据库系统处于正常工作状态，可以接受用户请求。

**Note that：**

 当然可以用用一条命令打开
startup

给出截图：
![]()
 上图中红色线分开的三部分表示三个启动阶段

 

**4，解决一个问题**

      本文一开始提出问题，这里做一解决，为什么会出现那个问题了是因为Oracle数据库没有被启动，解决的方法就是如上面3所示操作打开数据库；

      上述问题表现最直接的一个现象：查看任务管理器下oracle.exe所占内存，当oracle.exe所占内存为几十兆说明Oracle数据库没有启动，正常oracle.exe所占内存如下：
![]()
 现在还原错误：关闭数据库后连接数据库，查看oracle.exe所占内存
![]()
 查看内存：
![]()
 与正常启动时相差比价大，所以总结一下解决“**shared memory realm does not exist**”方法：

(1)任务管理器中查看oracle.exe所占内存，当oracle.exe所占内存仅为几十兆，说明问题是数据库没有启动

(2)启动Oracle数据库，管理员登录，启动
sqlplus /nolog conn USER/PASSWORD as sysdba startup

 

PS: **shared memory realm does not exist 这个错误我用了好长时间都没有解决主要原因是对数据库太陌生。**

**现在回想起来这个问题真是有点……**

* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/324493/7c3b4413-de81-385b-b39a-77a5c6634615.png)
* 大小: 37.6 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 5.4 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 2 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 6.6 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 5.2 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 4.5 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 4.1 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 1.2 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 1.2 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 4.6 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 10 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 4 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 2.6 KB

* [查看图片附件](http://kylinsoong.iteye.com/blog/776654#)
**8**
顶

**0**
踩

分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[Oracle Start Up 3：Oracle数据库基础](http://kylinsoong.iteye.com/blog/777540 "Oracle Start Up 3：Oracle数据库基础 ")| [Oracle Start Up 1： 几个概念和Oracle数 ...](http://kylinsoong.iteye.com/blog/775518 "Oracle Start Up 1： 几个概念和Oracle数据库的物理结构和逻辑结构")
* 14:07
* [评论](http://kylinsoong.iteye.com/blog/776654#comments) / 浏览 (0 / 4382)
* 分类:[数据库](http://www.iteye.com/blogs/category/database)
* [相关推荐](http://www.iteye.com/wiki/blog/776654)

### 评论

[]()
### 发表评论

您还没有登录，请[登录](http://kylinsoong.iteye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![kylinsoong的博客]( "kylinsoong的博客: Kylin Soong Blog")](http://kylinsoong.iteye.com/)

kylinsoong

* 浏览: 22311 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 北京
* ![]()
* [详细资料](http://kylinsoong.iteye.com/blog/profile) [留言簿](http://kylinsoong.iteye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://kylinsoong.iteye.com/blog/user_visits)

[![1号拖鞋的博客]( "1号拖鞋的博客: ")](http://pengxiaohui-huawei-com.iteye.com/)

[1号拖鞋](http://pengxiaohui-huawei-com.iteye.com/)

[![gong1的博客]( "gong1的博客: gong1")](http://gong1.iteye.com/)

[gong1](http://gong1.iteye.com/)
[![gyx_tc的博客]( "gyx_tc的博客: ")](http://gyx-tc.iteye.com/)

[gyx_tc](http://gyx-tc.iteye.com/)

[![zhangyiwen1999的博客]( "zhangyiwen1999的博客: ")](http://zhangyiwen1999.iteye.com/)

[zhangyiwen1999](http://zhangyiwen1999.iteye.com/)

### 博客分类

* [全部博客 (58)](http://kylinsoong.iteye.com/)
* [Search Engine (10)](http://kylinsoong.iteye.com/category/115445)
* [JavaEE (25)](http://kylinsoong.iteye.com/category/117875)
* [JAVA (5)](http://kylinsoong.iteye.com/category/117876)
* [LINUX (1)](http://kylinsoong.iteye.com/category/122556)
* [CODING LIFE (4)](http://kylinsoong.iteye.com/category/123758)
* [DATABASE (13)](http://kylinsoong.iteye.com/category/125064)
* [XML (2)](http://kylinsoong.iteye.com/category/139564)
* [JBOSS (1)](http://kylinsoong.iteye.com/category/168691)
### 我的留言簿 [>>更多留言](http://kylinsoong.iteye.com/blog/guest_book)

### 其他分类

* [我的收藏](http://kylinsoong.iteye.com/blog/favorite) (0)
* [我的代码](http://kylinsoong.iteye.com/blog/code_favorite) (2)
* [我的书籍](http://kylinsoong.iteye.com/blog/pdf) (1)
* [我的论坛主题帖](http://kylinsoong.iteye.com/blog/topic) (7)
* [我的所有论坛帖](http://kylinsoong.iteye.com/blog/post) (19)
* [我的精华良好帖](http://kylinsoong.iteye.com/blog/article) (0)
### 最近加入群组

* [Hibernate](http://hibernate.group.iteye.com/)
* [lucene爱好者](http://lucene-group.group.iteye.com/)

### 存档

* [2011-08](http://kylinsoong.iteye.com/blog/monthblog/2011-08) (1)
* [2011-07](http://kylinsoong.iteye.com/blog/monthblog/2011-07) (3)
* [2011-05](http://kylinsoong.iteye.com/blog/monthblog/2011-05) (5)
* [更多存档...](http://kylinsoong.iteye.com/blog/monthblog_more)
### 评论排行榜

* [Hibernate，JDBC性能探讨](http://kylinsoong.iteye.com/blog/743613 "Hibernate，JDBC性能探讨")
* [jvm classloader](http://kylinsoong.iteye.com/blog/753834 "jvm classloader")
* [Oracle 连接错误;ORA-27101: shared memor ...](http://kylinsoong.iteye.com/blog/774091 "Oracle 连接错误;ORA-27101: shared memory realm does not exist")
* [大家说说BBC的网站用的是什么技术做的](http://kylinsoong.iteye.com/blog/914122 "大家说说BBC的网站用的是什么技术做的")
* [Hibernate 错误总结](http://kylinsoong.iteye.com/blog/739936 "Hibernate 错误总结")

* [![Rss]()](http://kylinsoong.iteye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://kylinsoong.iteye.com/rss)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 ]
