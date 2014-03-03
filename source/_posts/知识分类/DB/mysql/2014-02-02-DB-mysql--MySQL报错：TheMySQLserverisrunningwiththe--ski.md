---
layout: post
title: "MySQL报错：The MySQL server is running with the --ski"
categories: DB
tags: 
 - DB
 - mysql
--- 

# MySQL报错：The MySQL server is running with the --skip-grant-tables option so it cannot execute this

[]()

# [_安静](http://www.cnblogs.com/xionghui/)
## [MySQL报错：The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement](http://www.cnblogs.com/xionghui/archive/2013/03/01/2939342.html)

The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement

解决办法：

mysql> set global read_only=0;
（关掉新主库的只读属性）

 flush privileges;

set global read_only=1;(读写属相)

 flush privileges;

Cannot execute statement: impossible to write to binary log since BINLOG_FORMAT = STATEMENT and at least one table uses a storage engine limited to row-based logging. InnoDB is limited to row-logging when transaction isolation level is READ COMMITTED or READ UNCOMMITTED.

  mysql> SET SESSION binlog_format = 'ROW';
   mysql> SET GLOBAL binlog_format = 'ROW';

 

## MYSQL5.1复制参数binlogformat

MySQL 5.1 中，在复制方面的改进即便引进了新的复制技巧：基于行的复制。简言之，这种新技巧即便关怀表中发生改变的登记，而非过去的照抄 binlog 形式。从 MySQL 5.1.12 开始，能够用以下三种形式来告终：基于SQL语句的复制(statement-based replication, SBR)，基于行的复制(row-based replication, RBR)，混杂形式复制(mixed-based replication, MBR)。相应地，binlog的款式也有三种：STATEMENT，ROW，MIXED。MBR 形式中，SBR 形式是默认的。
在运行时能够动态低改换binlog的款式，除非以下几种情形：
1. 存储过程可能引发器其中
2. 启用了NDB
3. 目前会话试用 RBR 形式，并且已敞开了临时表
万一binlog批准了 MIXED 形式，那么在以下几种情形下会积极将binlog的形式由 SBR 形式改成 RBR 形式。
1. 当DML语句更新一个NDB表时
2. 当函数中包括 UUID() 时
3. 2个及以上包括 AUTO_INCREMENT 字段的表被更新时
4. 行任何 INSERT DELAYED 语句时
5. 用 UDF 时
6. 视图中定然要求利用 RBR 时，例如创立视图是利用了 UUID() 函数
设定主从复制形式的措施极其容易，凡是在过去设定复制搭配的基础上，再加一个参数：
binlog_format="STATEMENT"
#binlog_format="ROW"
#binlog_format="MIXED"
当然了，也能够在运行时动态修正binlog的款式。例如
mysql> SET SESSION binlog_format = 'STATEMENT';
mysql> SET SESSION binlog_format = 'ROW';
mysql> SET SESSION binlog_format = 'MIXED';
mysql> SET GLOBAL binlog_format = 'STATEMENT';
mysql> SET GLOBAL binlog_format = 'ROW';
mysql> SET GLOBAL binlog_format = 'MIXED';
目前来比拟以下 SBR 和 RBR 2中形式各自的优缺点
SBR 的优点：
1. 历史悠久，技巧成熟
2. binlog文件较小
3. binlog中包括了所有数据库改动消息，能够据此来核实数据库的平安等情形
4. binlog能够用于实时的还原，而不但仅用于复制
5. 主从版本能够不一样，从服务器版本能够比主服务器版本高
SBR 的缺点：
1. 不是所有的UPDATE语句都能被复制，尤其是包括不确定垄断的时候。
2. 调用具有不确定因素的 UDF 时复制也可能出问题
3. 利用以下函数的语句也无法被复制：
* LOAD_FILE()
* UUID()
* USER()
* FOUND_ROWS()
* SYSDATE() (除非启用时启用了 --sysdate-is-now 选项)
4. INSERT ... SELECT 会发生比 RBR 更多的行级锁
5. 复制必需举行全表扫描(WHERE 语句中没利于用到索引)的 UPDATE 时，必需比 RBR 哀求更多的行级锁
6. 对于有 AUTO_INCREMENT 字段的 InnoDB表而言，INSERT 语句会阻塞其他 INSERT 语句
7. 对于一些混杂的语句，在从服务器上的耗资源情形会更严重，而 RBR 形式下，只会对那个发生改变的登记发生波及
8. 存储函数(不是存储过程)在被调用的同时也会厉行顺次 NOW() 函数，这个能够说是坏事也可能是好事
9. 确定了的 UDF 也必需在从服务器上厉行
10. 数据表定然几乎和主服务器坚持统一才行，否则可能会导致复制出错
11. 厉行混杂语句万一出错的话，会花费更多资源
RBR 的优点：
1. 任何情形都能够被复制，这对复制来说是最平安可靠的
2. 和其他大多数数据库系统的复制技巧一样
3. 多数情形下，从服务器上的表万一有主键的话，复制就会快了许多
4. 复制以下几种语句时的行锁更少：
* INSERT ... SELECT
* 包括 AUTO_INCREMENT 字段的 INSERT
* 未曾附带条件可能并未曾修正许多登记的 UPDATE 或 DELETE 语句
5. 厉行 INSERT，UPDATE，DELETE 语句时锁更少
6. 从服务器上批准多线程来厉行复制成为可能
RBR 的缺点：
1. binlog 大了许多
2. 混杂的回滚时 binlog 中会包括许多的数据
3. 主服务器上厉行 UPDATE 语句时，所有发生改变的登记都会写到 binlog 中，而 SBR 只会写顺次，这会导致频繁发生
binlog 的并发写问题
4. UDF 发生的大 BLOB 值会导致复制变慢
5. 无法从 binlog 中看到都复制了写什么语句
6. 当在非事务表上厉行一段堆积的SQL语句时，良好批准 SBR 形式，否则很轻率导致主从服务器的数据不统一情形发生
另外，针对系统库 mysql 里面的表发生改变时的处理法定如下：
1. 万一是批准 INSERT，UPDATE，DELETE 直接垄断表的情形，则日志款式依据 binlog_format 的设定而登记
2. 万一是批准 GRANT，REVOKE，SET PASSWORD 等管教语句来做的话，那么无论如何都批准 SBR 形式登记
注：批准 RBR 形式后，能处理许多本来揭示的主键重复问题例如Pro Publica，SunlightFoundation和维基解密，开始添补鞭策媒体滑坡留下的空白。

 

## MYSQL5.5MySQL 5.5 中对于二进制日志 (binlog) 有 3 种不同的格式可选：Mixed,Statement,Row，默认格式是 Statement。总结一下这三种格式日志的优缺点。

MySQL Replication 复制可以是基于一条语句 (Statement Level) ，也可以是基于一条记录 (Row Level)，可以在 MySQL 的配置参数中设定这个复制级别，不同复制级别的设置会影响到 Master 端的 bin-log 日志格式。

**1. Row**
日志中会记录成每一行数据被修改的形式，然后在 slave 端再对相同的数据进行修改。

**优点**： 在 row 模式下，bin-log 中可以不记录执行的 SQL 语句的上下文相关的信息，仅仅只需要记录那一条记录被修改了，修改成什么样了。所以 row 的日志内容会非常清楚的记录下每一行数据修改的细节，非常容易理解。而且不会出现某些特定情况下的存储过程或 function ，以及 trigger 的调用和触发无法被正确复制的问题。

**缺点**：在 row 模式下，所有的执行的语句当记录到日志中的时候，都将以每行记录的修改来记录，这样可能会产生大量的日志内容，比如有这样一条 update 语句：
1 UPDATE product SET owner_member_id = 'b' WHERE owner_member_id = 'a'

执 行之后，日志中记录的不是这条 update 语句所对应的事件 (MySQL 以事件的形式来记录 bin-log 日志) ，而是这条语句所更新的每一条记录的变化情况，这样就记录成很多条记录被更新的很多个事件。自然，bin-log 日志的量就会很大。尤其是当执行 alter table 之类的语句的时候，产生的日志量是惊人的。因为 MySQL 对于 alter table 之类的表结构变更语句的处理方式是整个表的每一条记录都需要变动，实际上就是重建了整个表。那么该表的每一条记录都会被记录到日志中。

**2. Statement**
每一条会修改数据的 SQL 都会记录到 master 的 bin-log 中。slave 在复制的时候 SQL 进程会解析成和原来 master 端执行过的相同的 SQL 再次执行。

**优点**：在 statement 模式下，首先就是解决了 row 模式的缺点，不需要记录每一行数据的变化，减少了 bin-log 日志量，节省 I/O 以及存储资源，提高性能。因为他只需要记录在 master 上所执行的语句的细节，以及执行语句时候的上下文的信息。

**缺点**： 在 statement 模式下，由于他是记录的执行语句，所以，为了让这些语句在 slave 端也能正确执行，那么他还必须记录每条语句在执行的时候的一些相关信息，也就是上下文信息，以保证所有语句在 slave 端杯执行的时候能够得到和在 master 端执行时候相同的结果。另外就是，由于 MySQL 现在发展比较快，很多的新功能不断的加入，使 MySQL 的复制遇到了不小的挑战，自然复制的时候涉及到越复杂的内容，bug 也就越容易出现。在 statement 中，目前已经发现的就有不少情况会造成 MySQL 的复制出现问题，主要是修改数据的时候使用了某些特定的函数或者功能的时候会出现，比如：sleep() 函数在有些版本中就不能被正确复制，在存储过程中使用了 last_insert_id() 函数，可能会使 slave 和 master 上得到不一致的 id 等等。由于 row 是基于每一行来记录的变化，所以不会出现类似的问题。

**3. Mixed**
从 官方文档中看到，之前的 MySQL 一直都只有基于 statement 的复制模式，直到 5.1.5 版本的 MySQL 才开始支持 row 复制。从 5.0 开始，MySQL 的复制已经解决了大量老版本中出现的无法正确复制的问题。但是由于存储过程的出现，给 MySQL Replication 又带来了更大的新挑战。另外，看到官方文档说，从 5.1.8 版本开始，MySQL 提供了除 Statement 和 Row 之外的第三种复制模式：Mixed，实际上就是前两种模式的结合。在 Mixed 模式下，MySQL 会根据执行的每一条具体的 SQL 语句来区分对待记录的日志形式，也就是在 statement 和 row 之间选择一种。新版本中的 statment 还是和以前一样，仅仅记录执行的语句。而新版本的 MySQL 中对 row 模式也被做了优化，并不是所有的修改都会以 row 模式来记录，比如遇到表结构变更的时候就会以 statement 模式来记录，如果 SQL 语句确实就是 update 或者 delete 等修改数据的语句，那么还是会记录所有行的变更。

**其他参考信息**

**除以下几种情况外，在运行时可以动态改变 binlog 的格式**：
. 存储流程或者触发器中间；
. 启用了 NDB；
. 当前会话使用 row 模式，并且已打开了临时表；

**如果 binlog 采用了 Mixed 模式，那么在以下几种情况下会自动将 binlog 的模式由 statement 模式变为 row 模式**：
. 当 DML 语句更新一个 NDB 表时；
. 当函数中包含 UUID() 时；
. 2 个及以上包含 AUTO_INCREMENT 字段的表被更新时；
. 执行 INSERT DELAYED 语句时；
. 用 UDF 时；
. 视图中必须要求运用 row 时，例如建立视图时使用了 UUID() 函数；

**设定主从复制模式**：
1 2 3 4 log-bin=mysql-bin #binlog_format="STATEMENT" #binlog_format="ROW" binlog_format="MIXED"

**也可以在运行时动态修改 binlog 的格式。例如**：

1 2 3 4 5 6 mysql> SET SESSION binlog_format = 'STATEMENT'; mysql> SET SESSION binlog_format = 'ROW'; mysql> SET SESSION binlog_format = 'MIXED'; mysql> SET GLOBAL binlog_format = 'STATEMENT'; mysql> SET GLOBAL binlog_format = 'ROW'; mysql> SET GLOBAL binlog_format = 'MIXED';

**两种模式的对比**：
**Statement 优点**
历史悠久，技术成熟；
产生的 binlog 文件较小；
binlog 中包含了所有数据库修改信息，可以据此来审核数据库的安全等情况；
binlog 可以用于实时的还原，而不仅仅用于复制；
主从版本可以不一样，从服务器版本可以比主服务器版本高；

**Statement 缺点**：
不是所有的 UPDATE 语句都能被复制，尤其是包含不确定操作的时候；
调用具有不确定因素的 UDF 时复制也可能出现问题；
运用以下函数的语句也不能被复制：
* LOAD_FILE()
* UUID()
* USER()
* FOUND_ROWS()
* SYSDATE() (除非启动时启用了 –sysdate-is-now 选项)
INSERT … SELECT 会产生比 RBR 更多的行级锁；
复制须要执行全表扫描 (WHERE 语句中没有运用到索引) 的 UPDATE 时，须要比 row 请求更多的行级锁；
对于有 AUTO_INCREMENT 字段的 InnoDB 表而言，INSERT 语句会阻塞其他 INSERT 语句；
对于一些复杂的语句，在从服务器上的耗资源情况会更严重，而 row 模式下，只会对那个发生变化的记录产生影响；
存储函数(不是存储流程 )在被调用的同时也会执行一次 NOW() 函数，这个可以说是坏事也可能是好事；
确定了的 UDF 也须要在从服务器上执行；
数据表必须几乎和主服务器保持一致才行，否则可能会导致复制出错；
执行复杂语句如果出错的话，会消耗更多资源；

**Row 优点**
任何情况都可以被复制，这对复制来说是最安全可靠的；
和其他大多数数据库系统的复制技能一样；
多数情况下，从服务器上的表如果有主键的话，复制就会快了很多；
复制以下几种语句时的行锁更少：
* INSERT … SELECT
* 包含 AUTO_INCREMENT 字段的 INSERT
* 没有附带条件或者并没有修改很多记录的 UPDATE 或 DELETE 语句
执行 INSERT，UPDATE，DELETE 语句时锁更少；
从服务器上采用多线程来执行复制成为可能；

**Row 缺点**
生成的 binlog 日志体积大了很多；
复杂的回滚时 binlog 中会包含大量的数据；
主服务器上执行 UPDATE 语句时，所有发生变化的记录都会写到 binlog 中，而 statement 只会写一次，这会导致频繁发生 binlog 的写并发请求；
UDF 产生的大 BLOB 值会导致复制变慢；
不能从 binlog 中看到都复制了写什么语句(加密过的)；
当在非事务表上执行一段堆积的 SQL 语句时，最好采用 statement 模式，否则很容易导致主从服务器的数据不一致情况发生；
另外，针对系统库 MySQL 里面的表发生变化时的处理准则如下：
如果是采用 INSERT，UPDATE，DELETE 直接操作表的情况，则日志格式根据 binlog_format 的设定而记录；
如果是采用 GRANT，REVOKE，SET PASSWORD 等管理语句来做的话，那么无论如何都要使用 statement 模式记录；
使用 statement 模式后，能处理很多原先出现的主键重复问题；

分类: [mysql](http://www.cnblogs.com/xionghui/category/341683.html)

绿色通道： [好文要顶]() [关注我]() [收藏该文]()[与我联系](http://space.cnblogs.com/msg/send/_%e5%ae%89%e9%9d%99) [![]()]( "分享至新浪微博")
[_安静](http://home.cnblogs.com/u/xionghui/)
[关注 - 5](http://home.cnblogs.com/u/xionghui/followees)
[粉丝 - 8](http://home.cnblogs.com/u/xionghui/followers)

[+加关注]()

0

0
(请您对文章做出评价)

[«](http://www.cnblogs.com/xionghui/archive/2012/07/15/2592876.html) 上一篇：[vs2005下配置OGRE](http://www.cnblogs.com/xionghui/archive/2012/07/15/2592876.html "发布于2012-07-15 23:50")
[»](http://www.cnblogs.com/xionghui/archive/2013/03/07/2947384.html) 下一篇：[Cubic Spline三次样条曲线](http://www.cnblogs.com/xionghui/archive/2013/03/07/2947384.html "发布于2013-03-07 09:13")

posted on 2013-03-01 19:37 [_安静](http://www.cnblogs.com/xionghui/) 阅读(641) 评论(0) [编辑](http://www.cnblogs.com/xionghui/admin/EditPosts.aspx?postid=2939342) [收藏]()

 
[刷新评论]()[刷新页面]()[返回顶部]()

注册用户登录后才能发表评论，请 [登录]() 或 [注册]()，[访问](http://www.cnblogs.com/)网站首页。
[博客园首页](http://www.cnblogs.com/ "程序员的网上家园")[博问](http://q.cnblogs.com/ "程序员问答社区")[新闻](http://news.cnblogs.com/ "IT新闻")[闪存](http://home.cnblogs.com/ing/)[程序员招聘](http://job.cnblogs.com/)[知识库](http://kb.cnblogs.com/)

**最新IT新闻**:
· [烧钱不断的Ubuntu——一个理想主义者的故事](http://news.cnblogs.com/n/185243/)
· [配4100万像素摄像头 Hyetis Crossbow智能手表](http://news.cnblogs.com/n/185242/)
· [金正恩都说好的朝鲜手机是国产山寨？](http://news.cnblogs.com/n/185241/)
· [3D视差效果：23张iOS 7壁纸免费下载](http://news.cnblogs.com/n/185240/)
· [免费！广告屏蔽插件Adblock正式发布IE版](http://news.cnblogs.com/n/185239/)
» [更多新闻...](http://news.cnblogs.com/ "IT新闻")

**最新知识库文章**:
· [我现在是这样编程的](http://kb.cnblogs.com/page/180323/)
· [如何做到 jQuery-free？](http://kb.cnblogs.com/page/178891/)
· [如何成为一位优秀的创业CEO](http://kb.cnblogs.com/page/185000/)
· [十年前的Java企业应用开发世界](http://kb.cnblogs.com/page/184977/)
· [专注做好一件事](http://kb.cnblogs.com/page/184935/)
» [更多知识库文章...](http://kb.cnblogs.com/)
**历史上的今天:**
2012-03-01 [Eclipse中web-inf和meta-inf文件夹的信息](http://www.cnblogs.com/xionghui/archive/2012/03/01/2375513.html)
2012-03-01 [在Eclipse中创建WEB工程步骤](http://www.cnblogs.com/xionghui/archive/2012/03/01/2375503.html)

### 公告

昵称：[_安静](http://home.cnblogs.com/u/xionghui/)
园龄：[1年8个月](http://home.cnblogs.com/u/xionghui/ "入园时间：2011-12-07")
粉丝：[8](http://home.cnblogs.com/u/xionghui/followers/)
关注：[5](http://home.cnblogs.com/u/xionghui/followees/)

[+加关注]()

### 导航

* [博客园](http://www.cnblogs.com/)
* [首页](http://www.cnblogs.com/xionghui/)
* [新随笔](http://www.cnblogs.com/xionghui/admin/EditPosts.aspx?opt=1)
* [联系](http://space.cnblogs.com/msg/send/_%e5%ae%89%e9%9d%99)
* [订阅](http://www.cnblogs.com/xionghui/rss)[![订阅]()](http://www.cnblogs.com/xionghui/rss)
* [管理](http://www.cnblogs.com/xionghui/admin/EditPosts.aspx)
[<]()2013年3月[>]()日一二三四五六2425262728[1](http://www.cnblogs.com/xionghui/archive/2013/03/01.html)23456[7](http://www.cnblogs.com/xionghui/archive/2013/03/07.html)8910111213141516171819202122232425262728293031123456

### 统计

* 随笔 - 63
* 文章 - 0
* 评论 - 2
* 引用 - 0
### 搜索

 

 

### 常用链接

* [我的随笔](http://www.cnblogs.com/xionghui/MyPosts.html)
* [我的评论](http://www.cnblogs.com/xionghui/MyComments.html)
* [我的参与](http://www.cnblogs.com/xionghui/OtherPosts.html "我发表过评论的随笔")
* [最新评论](http://www.cnblogs.com/xionghui/RecentComments.html)
* [我的标签](http://www.cnblogs.com/xionghui/tag/)

### 我的标签

* [redmine1.2.2安装 ruby1.8.7 rubygem1.6.2](http://www.cnblogs.com/xionghui/tag/redmine1.2.2%E5%AE%89%E8%A3%85    ruby1.8.7   rubygem1.6.2/)(5)
* [eclipse](http://www.cnblogs.com/xionghui/tag/eclipse/)(3)
* [html css javascript jquery easyui](http://www.cnblogs.com/xionghui/tag/html css javascript jquery easyui/)(3)
* [tomcat安装和配置](http://www.cnblogs.com/xionghui/tag/tomcat%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE/)(2)
* [xhEditor](http://www.cnblogs.com/xionghui/tag/xhEditor/)(1)
* [HTML5](http://www.cnblogs.com/xionghui/tag/HTML5/)(1)
* [jdk配置](http://www.cnblogs.com/xionghui/tag/jdk%E9%85%8D%E7%BD%AE/)(1)
* [ogre](http://www.cnblogs.com/xionghui/tag/ogre/)(1)
* [php安装和配置](http://www.cnblogs.com/xionghui/tag/php%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE/)(1)
* [serv-u安装配置](http://www.cnblogs.com/xionghui/tag/serv-u%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE/)(1)
* [更多](http://www.cnblogs.com/xionghui/tag/)

### 随笔分类(46)

* [ant(1)](http://www.cnblogs.com/xionghui/category/362695.html)
* [c c++](http://www.cnblogs.com/xionghui/category/362804.html)
* [Coldfusion(6)](http://www.cnblogs.com/xionghui/category/341738.html)
* [discuz(3)](http://www.cnblogs.com/xionghui/category/367982.html)
* [easyUI](http://www.cnblogs.com/xionghui/category/361900.html)
* [html(1)](http://www.cnblogs.com/xionghui/category/361898.html)
* [inno setup(2)](http://www.cnblogs.com/xionghui/category/367981.html)
* [java(2)](http://www.cnblogs.com/xionghui/category/362803.html)
* [javascript(4)](http://www.cnblogs.com/xionghui/category/361899.html)
* [jquery(2)](http://www.cnblogs.com/xionghui/category/361897.html)
* [mysql(9)](http://www.cnblogs.com/xionghui/category/341683.html)
* [Ogre(1)](http://www.cnblogs.com/xionghui/category/396246.html)
* [ruby redmine(4)](http://www.cnblogs.com/xionghui/category/341014.html)
* [struts2(3)](http://www.cnblogs.com/xionghui/category/357315.html)
* [study(2)](http://www.cnblogs.com/xionghui/category/366403.html)
* [tomcat(1)](http://www.cnblogs.com/xionghui/category/361984.html)
* [web(4)](http://www.cnblogs.com/xionghui/category/362028.html)
* [数值分析(1)](http://www.cnblogs.com/xionghui/category/457545.html)

### 随笔档案(63)

* [2013年3月 (2)](http://www.cnblogs.com/xionghui/archive/2013/03.html)
* [2012年7月 (2)](http://www.cnblogs.com/xionghui/archive/2012/07.html)
* [2012年5月 (8)](http://www.cnblogs.com/xionghui/archive/2012/05.html)
* [2012年4月 (5)](http://www.cnblogs.com/xionghui/archive/2012/04.html)
* [2012年3月 (28)](http://www.cnblogs.com/xionghui/archive/2012/03.html)
* [2012年2月 (9)](http://www.cnblogs.com/xionghui/archive/2012/02.html)
* [2011年12月 (9)](http://www.cnblogs.com/xionghui/archive/2011/12.html)

### mysql下载

### 最新评论

### 阅读排行榜

* [1. 在Eclipse中创建WEB工程步骤(11416)](http://www.cnblogs.com/xionghui/archive/2012/03/01/2375503.html)
* [2. redmine-1.2.2安装截图粘贴插件(7722)](http://www.cnblogs.com/xionghui/archive/2011/12/09/2281827.html)
* [3. redmine-1.2.2安装代码评审插件(6289)](http://www.cnblogs.com/xionghui/archive/2011/12/08/2281229.html)
* [4. tomcat配置文件web.xml与server.xml解析--重要(6112)](http://www.cnblogs.com/xionghui/archive/2012/03/11/2390258.html)
* [5. redmine-1.2.2安装服务（附图）(6089)](http://www.cnblogs.com/xionghui/archive/2011/12/08/2280518.html)

### 评论排行榜

* [1. Mysql安装配置(2)](http://www.cnblogs.com/xionghui/archive/2011/12/10/2283229.html)
* [2. redmine-1.2.2安装截图粘贴插件(0)](http://www.cnblogs.com/xionghui/archive/2011/12/09/2281827.html)
* [3. jQuery 数据表： 将单个列导出到 Excel(0)](http://www.cnblogs.com/xionghui/archive/2012/03/11/2389927.html)
* [4. MYSQL启用日志，查看日志，利用mysqlbinlog工具恢复MySQL数据库(0)](http://www.cnblogs.com/xionghui/archive/2012/03/11/2389792.html)
* [5. mysql数据库my.ini配置文件中文详解(0)](http://www.cnblogs.com/xionghui/archive/2012/03/11/2389790.html)

### 推荐排行榜

* [1. mysqldump 常用备份选项,只备份数据或结构的方法(3)](http://www.cnblogs.com/xionghui/archive/2012/03/11/2389791.html)
* [2. Mysql安装配置(2)](http://www.cnblogs.com/xionghui/archive/2011/12/10/2283229.html)
* [3. web绿色环境搭建(2)](http://www.cnblogs.com/xionghui/archive/2012/03/12/2392555.html)
* [4. tomcat配置文件web.xml与server.xml解析--重要(2)](http://www.cnblogs.com/xionghui/archive/2012/03/11/2390258.html)
* [5. Coldfusion 7 生成验证码实例(CF验证码)(1)](http://www.cnblogs.com/xionghui/archive/2011/12/10/2283556.html)

Powered by: [博客园](http://www.cnblogs.com/) Copyright © _安静
