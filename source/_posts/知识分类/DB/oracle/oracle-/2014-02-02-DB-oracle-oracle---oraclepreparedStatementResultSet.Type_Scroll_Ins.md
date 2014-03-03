---
layout: post
title: "oracle preparedStatement ResultSet.Type_Scroll_Ins"
categories: DB
tags: 
 - DB
 - oracle
 - oracle-
--- 

# oracle preparedStatement ResultSet.Type_Scroll_Insensitive 乱码 - 我是一只愚 - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼](http://vvnet.iteye.com/blog/1469370#)

[招聘](http://www.iteye.com/job) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://vvnet.iteye.com/login "登录") [登录](http://vvnet.iteye.com/login) [注册](http://vvnet.iteye.com/signup)

# [我是一只愚](http://vvnet.iteye.com/)

* [**博客**](http://vvnet.iteye.com/)
* [微博](http://vvnet.iteye.com/weibo)
* [相册](http://vvnet.iteye.com/album)
* [收藏](http://vvnet.iteye.com/link)
* [留言](http://vvnet.iteye.com/blog/guest_book)
* [关于我](http://vvnet.iteye.com/blog/profile)

### [oracle preparedStatement ResultSet.Type_Scroll_Insensitive 乱码]() **

**博客分类：**
* [JAVA](http://vvnet.iteye.com/category/47121)

问题：

我用的数据库是 oracle 字符集 us7ascii
用preparedStatement(sqlString, ResultSet.Type_Scroll_Insensitive, ResultSet.Concur_read_only)
从数据库中 读取数据后 中文字段乱码,不管是转不转码都是乱码
但是如果preparedStatement(sqlString) 转码成GBK后就能正常显示中，服了。

查询百度，有说是oracle jdbc驱动的问题，这个还没有验证，有时间验证一下，作参考。

深入理解（**原文http://www.iteye.com/topic/123557** ）

JDBC2.0后提出了三种不同的cursor类型，用户代码可以在创建Statement指定cursor类型，如下：
Statement createStatement( int resultSetType, int resultSetConcurrency)

# **cursor类型**

## ResultSet.TYPE_FORWARD_ONLY

   默认的cursor类型，仅仅支持向前forward，不支持backforward，random，last，first操作，类似单向链表。
   TYPE_FORWARD_ONLY类型通常是效率最高最快的cursor类型

## ResultSet.TYPE_SCROLL_INSENSITIVE

   支持backforward，random，last，first操作，对其它数据session对选择数据做出的更改是不敏感，不可见的。

## ResultSet.TYPE_SCROLL_SENSITIVE

   支持backforward，random，last，first操作，对其它数据session对选择数据做出的更改是敏感，可见的。

# 分析

   众所周知，JDBC对数据库进行数据查询时，数据库会创建查询结果的cache和cursor。而数据库的cursor是不支持 backforward，random，last，first操作，仅仅只支持向前forward。那么TYPE_SCROLL_INSENSITIVE 是如何实现支持backforward，random，last，first的呢？很简单，TYPE_SCROLL_INSENSITIVE的 Statement查询把所有fetch的记录都缓存到JVM的Resultset对象内，如果有10个记录，直接跳到最后记 录，TYPE_SCROLL_INSENSITIVE方式下把fetch所有记录到jvm端，并缓存下来，再进行random就是在数据库数组里面进行 的。这也是TYPE_FORWARD_ONLY类型通常是效率最高最快的cursor类型原因，如果要做一些复杂的功能，必然是要牺牲一些效率的。
    那么为什么TYPE_SCROLL_INSENSITIVE对选择数据做出的更改是不敏感，不可见的呢？前面提到，JDBC对数据库进行数据查询时，数据库会创建查询结果的cache和cursor，如下面sql:
    select name,id from foo
    用jdbc执行上面的sql语句时，数据库会把foo表所有记录的name和id字段缓存到cache中，之后cache和真正的数据库数据文件没有任何 联系了，foo表发生的改变在查询完成后不会自动同步到cache上去，因此TYPE_SCROLL_INSENSITIVE对选择数据做出的更改是不敏 感，不可见。
    那么TYPE_SCROLL_SENSITIVE是怎么做到其它数据session对选择数据做出的更改是敏感，可见的。上面的sql语句用TYPE_SCROLL_SENSITIVE的Statement来执行，会转化成以下的sql语句：
    select rowid from foo
    数据库这时候是把foo表所有记录的rowid缓存到cache中，用户代码在fetch记录时，再继续做以下查询：
    select name,id from foo where rowid=?
    因此这时候发生的查询是实时从真正的数据库数据文件中取，因此对期间发生的数据更改是可见的，敏感的。但是这种可见性仅限于update操作，而 insert和delete同样是不可见的。因为如果查询发生在insert之前，insert生成的rowid并不会反应在cache中的rowid结 果集上。在一个记录的rowid已经缓存到cache中，这时候被删除了，但一般数据库的删除是标记删除，也就是说rowid对应那行记录并没有真正从数 据库文件中抹去，一般是可以再次取到记录的。

# 总结

    TYPE_FORWARD_ONLY类型通常是效率最高最快的cursor类型，也是最常用的选择。
    TYPE_SCROLL_INSENSITIVE需要在jvm中cache所有fetch到的记录实体，在大量记录集返回时慎用。
    TYPE_SCROLL_SENSITIVE在jvm中cache所有fetch到的记录rowid，需要进行二次查询，效率最低，开销最大

 

 

JDBC ResultSet分析 （**原文http://www.iteye.com/topic/560109** ）

JDBC1.0 、JDBC2.0 、JDBC3.0 中分别用以下方法创建Statement 。

JDBC1.0 ： createStatement()

JDBC2.0 ： createStatement(resultSetType, resultSetConcurrency)

JDBC3.0 ： createStatement(resultSetType, resultSetConcurrency, resultSetHoldability)

 

下面依次分析resultSetType 、resultSetConcurrency 、resultSetHoldability 这几个参数的含义。

**一 ResultSetType**

      resultSetType 的可选值有： ResultSet.TYPE_FORWARD_ONLY 、ResultSet.TYPE_SCROLL_INSENSITIVE 、ResultSet.TYPE_SCROLL_SENSITIVE 。

**1** **：ResultSet.TYPE_FORWARD_ONLY**

默认的cursor 类型，仅仅支持结果集forward ，不支持backforward ，random ，last ，first 等操作。  

**2** **：ResultSet.TYPE_SCROLL_INSENSITIVE**

支持结果集backforward ，random ，last ，first 等操作，对其它session 对数据库中数据做出的更改是不敏感的。

实现方法：从数据库取出数据后，会把全部数据缓存到cache 中，对结果集的后续操作，是操作的cache 中的数据，数据库中记录发生变化后，不影响cache 中的数据，所以ResultSet 对结果集中的数据是**INSENSITIVE** 的。

**3** **：ResultSet.TYPE_SCROLL_SENSITIVE**

支持结果集backforward ，random ，last ，first 等操作，对其它session 对数据库中数据做出的更改是敏感的，即其他session 修改了数据库中的数据，会反应到本结果集中。

 

实现方法：从数据库取出数据后，不是把全部数据缓存到cache 中，而是把每条数据的rowid 缓存到cache 中，对结果集后续操作时，是根据rowid 再去数据库中取数据。所以数据库中记录发生变化后，通过ResultSet 取出的记录是最新的，即ResultSet 是**SENSITIVE** 的。 但insert 和delete 操作不会影响到ResultSet ，因为insert 数据的rowid 不在ResultSet 取出的rowid 中，所以insert 的数据对ResultSet 是不可见的，而delete 数据的rowid 依旧在ResultSet 中，所以ResultSet 仍可以取出被删除的记录（ 因为一般数据库的删除是标记删除，不是真正在数据库文件中删除 ）。

做个试验，验证一下SENSITIVE 特性。数据库为oracle10g ，驱动为ojdbc14.jar 。

test 表中数据如下：

c1 c2 c3 1c1 1c2 1c3 2c1 2c2 2c3 3c1 3c2 3c3

程序如下：
Java代码  [![收藏代码]()]( "收藏这段代码")

1. public   static   void  testResultSetSensitive(Connection conn)  throws  Exception{  
1.   
1.         String sql = "SELECT c1,c2,c3 FROM test" ;  
1.         try  {  
1.             Statement stmt = conn  
1.                     .createStatement(ResultSet.TYPE_SCROLL_SENSITIVE, ResultSet.CONCUR_READ_ONLY);  
1.               
1.             ResultSet rs = stmt.executeQuery(sql);  
1.               
1.             while  (rs.next()) {  
1.                 System.out.println("[行号："  + rs.getRow() +  "]\t"  + rs.getString( 1 ) +  "\t"  + rs.getString( 2 )  
1.                         + "\t"  + rs.getString( 3 ));  
1.                 Thread.sleep(20000 );  
1.             }  
1.               
1.             rs.close();  
1.             stmt.close();  
1.         } catch  (SQLException e) {  
1.             e.printStackTrace();  
1.         } finally  {  
1.             try  {  
1.                 conn.close();  
1.             } catch  (Exception e) {  
1.             }  
1.         }  
1.     }  

定义ResultSet 为 ResultSet. TYPE_SCROLL_SENSITIVE 类型，首先执行 sql 访问数据库，然后执行 rs.next() 移动游标取数据。在循环里面加上 Thread.*sleep* (20000) 的目的是为了我们有时间在后台把数据库里的数据改了。比如当在循环里打印出第一行的数据后，我们在后台，把第三行数据的 c3 列改成 ”3uuu” 。如果 ResultSet 真的是敏感的话，那应该取出 ”3uuu” ，而不是原始的“ 3c 3 ”。但最终的结果却是如下：

 

[ 行号： 1]  1c1    1c2    1c3

[ 行号： 2]  2c1    2c2    2c3

[ 行号： 3]  3c1    3c2    3c3

 

数据没变呀，ResultSet 不敏感啊！于是去查阅资料，找了n 久，还是在英文文档上找到了答案。原来是fetchsize 的问题。调用ResultSet 的next 方法取数据时，并不是每调用一次方法就去数据库里查一次，而是有个fetchSize, 一次取fetchSize 条数据。Oracle 默认的fetchsize 等于10 ，所以上面的代码在第一次调用rs.next() 时，就已经把3 条数据都取出来了，所以才会有上面的结果。

 

      第二次实验，在ResultSet rs = stmt.executeQuery(sql); 前面加上 stmt.setFetchSize(1); 将fetchSize 设置为1 。然后重新第一次实验的步骤，发现最 终结果为：

[ 行号： 1]  1c1    1c2    1c3

[ 行号： 2]  2c1    2c2    2c3

[ 行号： 3]  3c1    3c2    3uuu

    原因就是 fetchsize 设置为 1 时，每次 next 取数时都会重新用 rowid 取数据库里取数据，当然取到的是最新的数据了。

  

     **二 ResultSetConcurrency**

 

      ResultSetConcurrency的可选值有2个：
      ResultSet.CONCUR_READ_ONLY 在ResultSet中的数据记录是只读的，不可以修改
      ResultSet.CONCUR_UPDATABLE 在ResultSet中的数据记录可以任意修改，然后更新到数据库，可以插入，删除，修改。

 

     **三 ResultSetHoldability**

#    ResultSetHoldability    的可选值有2 个 ：

     HOLD_CURSORS_OVER_COMMIT:  在事务commit 或rollback 后，ResultSet 仍然可用。
     CLOSE_CURSORS_AT_COMMIT:  在事务commit 或rollback 后，ResultSet  被关闭。

  

     需要注意的地方：

   1 ：Oracle 只支持HOLD_CURSORS_OVER_COMMIT 。

   2 ：当Statement 执行下一个查询，生成第二个ResultSet 时，第一个ResultSet 会被关闭，这和是否支持支持HOLD_CURSORS_OVER_COMMIT 无关。

 

**     四 验证数据库是否支持ResultSet的各种特性**

 

不同的数据库版本及 JDBC 驱动版本，对 ResultSet 的各种高级特性的支持是不一样的，我们可以通过以下方法，来验证具体的数据库及 JDBC 驱动，是否支持 ResultSet 的各种特性。

    

     DatabaseMetaData dbMeta = conn.getMetaData();

       然后调用 DatabaseMetaData 对象的以下方法：

       boolean supportsResultSetType(int resultSetType);

       boolean supportsResultSetConcurrency(int type, int concurrency);

        boolean supportsResultSetHoldability(int holdability);

 

      参考的2 篇英文文档：

 http://cs.felk.cvut.cz/10gr2/java.102/b14355/jdbcvers.htm  （JDBC Standards Support ）

 http://download.oracle.com/docs/cd/B10501_01/java.920/a96654/resltset.htm#1023642 (Oracle9i JDBC Developer's Guide and Reference Release 2 (9.2))
分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[JAVA5的新特性收集](http://vvnet.iteye.com/blog/840267 "JAVA5的新特性收集") | [ORACLE：按月累计问题的解决方法](http://vvnet.iteye.com/blog/1397474 "ORACLE：按月累计问题的解决方法")
* 2012-03-30 00:31:23
* 浏览 25
* [评论(0)](http://vvnet.iteye.com/blog/1469370#comments)
* 分类:[数据库](http://www.iteye.com/blogs/category/database)
* [相关推荐](http://www.iteye.com/wiki/blog/1469370)

### 评论

[]()
### 发表评论

[![]()](http://vvnet.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://vvnet.iteye.com/login)

[![vvnet的博客]( "vvnet的博客: 我是一只愚")](http://vvnet.iteye.com/)

vvnet

* 浏览: 28526 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 青岛
* ![]()
### 最近访客 [更多访客>>](http://vvnet.iteye.com/blog/user_visits)

[![coldsummerwei的博客]( "coldsummerwei的博客: ")](http://coldsummerwei.iteye.com/)

[coldsummerwei](http://coldsummerwei.iteye.com/)

[![shadow19831112的博客]( "shadow19831112的博客: ")](http://shadow19831112.iteye.com/)

[shadow19831112](http://shadow19831112.iteye.com/)
[![hwf52591的博客]( "hwf52591的博客: ")](http://hwf52591.iteye.com/)

[hwf52591](http://hwf52591.iteye.com/)

[![17369644的博客]( "17369644的博客: ")](http://17369644.iteye.com/)

[17369644](http://17369644.iteye.com/)

### 文章分类

* [全部博客 (61)](http://vvnet.iteye.com/)
* [JAVA (26)](http://vvnet.iteye.com/category/47121)
* [数据库 (14)](http://vvnet.iteye.com/category/47122)
* [JavaScript (7)](http://vvnet.iteye.com/category/47123)
* [操作系统 (2)](http://vvnet.iteye.com/category/47710)
* [爱好 (3)](http://vvnet.iteye.com/category/48387)
* [杂文 (4)](http://vvnet.iteye.com/category/49118)
* [闲言碎语 (1)](http://vvnet.iteye.com/category/52379)
* [装修 (1)](http://vvnet.iteye.com/category/55223)
* [考试 (2)](http://vvnet.iteye.com/category/55346)
### 社区版块

* [我的资讯](http://vvnet.iteye.com/blog/news) (0)
* [我的论坛](http://vvnet.iteye.com/blog/post) (20)
* [我解决的问题](http://vvnet.iteye.com/blog/solution) (0)

### 存档分类

* [2012-03](http://vvnet.iteye.com/blog/monthblog/2012-03) (2)
* [2012-02](http://vvnet.iteye.com/blog/monthblog/2012-02) (1)
* [2011-05](http://vvnet.iteye.com/blog/monthblog/2011-05) (2)
* [更多存档...](http://vvnet.iteye.com/blog/monthblog_more)
### 最新评论

* [623deyingxiong](http://623deyingxiong.iteye.com/)： 补充一点：在Eclipse里，子类的方法名只要满足参数列表，方 ...
[overload和override的区别](http://vvnet.iteye.com/blog/309691#bc1715319)
* [qq274035206](http://whz1987.iteye.com/)： 很好 写的很详细
[Eclipse下支持jQuery1.2.6](http://vvnet.iteye.com/blog/311393#bc862725)
* [vvnet](http://vvnet.iteye.com/)： 我用的版本是Myeclipse6.6
[Eclipse下支持jQuery1.2.6](http://vvnet.iteye.com/blog/311393#bc832009)
* [rockjava](http://rockjava.iteye.com/)： 这个插件我试了N回都没好用过,不知道为什么?
[Eclipse下支持jQuery1.2.6](http://vvnet.iteye.com/blog/311393#bc831948)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![]()
