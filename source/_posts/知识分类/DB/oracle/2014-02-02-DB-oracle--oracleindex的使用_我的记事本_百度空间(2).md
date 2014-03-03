---
layout: post
title: "oracle index的使用_我的记事本_百度空间 (2)"
categories: DB
tags: 
 - DB
 - oracle
--- 

# oracle index的使用_我的记事本_百度空间 (2)

### 分享到

* [QQ空间](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [新浪微博](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [百度搜藏](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [人人网](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [腾讯微博](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [开心网](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [腾讯朋友](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [百度空间](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [豆瓣网](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [搜狐微博](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [百度新首页](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [QQ收藏](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [和讯微博](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [我的淘宝](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [百度贴吧](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
* [更多...](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)

[百度分享](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)

[](http://hi.baidu.com/)

* [广场](http://hi.baidu.com/index)
* [jywv](https://passport.baidu.com/center)
*
* [退出](https://passport.baidu.com/?logout&bdstoken=aa9855a9aad4216b27441b12182ad1a7&u=http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0)
[关注此空间](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)
# [我的记事本](http://hi.baidu.com/new/yh1350)

2011-07-23 10:13

## oracle index的使用

Oracle提供了大量索引选项。知道在给定条件下使用哪个选项对于一个应用程序的性能来说非常重要。一个错误的选择可能会引发死锁，并导致数据库性能急剧下降或进程终止。而如果做出正确的选择，则可以合理使用资源，使那些已经运行了几个小时甚至几天的进程在几分钟得以完成，这样会使您立刻成为一位英雄。这篇文章就将简单的讨论每个索引选项。主要有以下内容：

[1]基本的索引概念

查询DBA_INDEXES视图可得到表中所有索引的列表，注意只能通过USER_INDEXES的方法来检索模式(schema)的索引。访问USER_IND_COLUMNS视图可得到一个给定表中被索引的特定列。

[2]组合索引

当某个索引包含有多个已索引的列时，称这个索引为组合（concatented）索引。在Oracle9i引入跳跃式扫描的索引访问方法之前，查询只能在有限条件下使用该索引。比如：表emp有一个组合索引键，该索引包含了empno、ename和deptno。在Oracle9i之前除非在where之句中对第一列（empno）指定一个值，否则就不能使用这个索引键进行一次范围扫描。

特别注意：在Oracle9i之前，只有在使用到索引的前导索引时才可以使用组合索引！

[3] ORACLE ROWID

通过每个行的ROWID，索引Oracle提供了访问单行数据的能力。ROWID其实就是直接指向单独行的线路图。如果想检查重复值或是其他对ROWID本身的引用，可以在任何表中使用和指定rowid列。

[4]限制索引

限制索引是一些没有经验的开发人员经常犯的错误之一。在SQL中有很多陷阱会使一些索引无法使用。下面讨论一些常见的问题：

    4.1使用不等于操作符（<>、!=）

       下面的查询即使在cust_rating列有一个索引，查询语句仍然执行一次全表扫描。

         select cust_Id,cust_name

         from   customers

         where  cust_rating <> 'aa';

        把上面的语句改成如下的查询语句，这样，在采用基于规则的

        优化器而不是基于代价的优化器（更智能）时，将会使用索引。

         select cust_Id,cust_name<?xml:namespace prefix = o ns = "urn:schemas-microsoft-com:office:office" />

         from   customers

         where  cust_rating < 'aa' or cust_rating > 'aa';

    特别注意：通过把不等于操作符改成OR条件，就可以使用索引，以避免全表扫描。

     4.2使用IS NULL或IS NOT NULL

使用IS NULL或IS NOT NULL同样会限制索引的使用。因为NULL值并没有被定义。在SQL语句中使用NULL会有很多的麻烦。因此建议开发人员在建表时，把需要索引的列设成NOT NULL。如果被索引的列在某些行中存在NULL值，就不会使用这个索引（除非索引是一个位图索引，关于位图索引在稍后在详细讨论）。

4.3使用函数

如果不使用基于函数的索引，那么在SQL语句的WHERE子句中对存在索引的列使用函数时，会使优化器忽略掉这些索引。下面的查询不会使用索引（只要它不是基于函数的索引）

          select empno,ename,deptno

          from   emp

          where  trunc(hiredate)='01-MAY-81';

         把上面的语句改成下面的语句，这样就可以通过索引进行查找。

          select empno,ename,deptno

          from   emp

          where  hiredate<(to_date('01-MAY-81')+0.9999);

 

     4.4比较不匹配的数据类型

        比较不匹配的数据类型也是比较难于发现的性能问题之一。

        注意下面查询的例子，account_number是一个VARCHAR2类型，

        在account_number字段上有索引。下面的语句将执行全表扫描。

         select bank_name,address,city,state,zip

         from   banks

         where  account_number = 990354;

         Oracle可以自动把where子句变成to_number(account_number)=990354，这样就限制了

         索引的使用,改成下面的查询就可以使用索引：

         select bank_name,address,city,state,zip

         from   banks

         where  account_number ='990354';

    特别注意：不匹配的数据类型之间比较会让Oracle自动限制索引的使用，

       即便对这个查询执行Explain Plan也不能让您明白为什么做了一次“全表扫描”。

[5]选择性

使用USER_INDEXES视图，该视图中显示了一个distinct_keys列。比较一下唯一键的数量和表中的行数，就可以判断索引的选择性。选择性越高，索引返回的数据就越少。

[6]群集因子(Clustering Factor)

Clustering Factor位于USER_INDEXES视图中。该列反映了数据相对于已索引的列是否显得有序。如果Clustering Factor列的值接近于索引中的树叶块(leaf block)的数目，表中的数据就越有序。如果它的值接近于表中的行数，则表中的数据就不是很有序。

[7]二元高度(Binary height)

索引的二元高度对把ROWID返回给用户进程时所要求的I/O量起到关键作用。在对一个索引进行分析后，可以通过查询DBA_INDEXES的B- level列查看它的二元高度。二元高度主要随着表的大小以及被索引的列中值的范围的狭窄程度而变化。索引上如果有大量被删除的行，它的二元高度也会增加。更新索引列也类似于删除操作，因为它增加了已删除键的数目。重建索引可能会降低二元高度。

[8]快速全局扫描

在Oracle7.3后就可以使用快速全局扫描(Fast Full Scan)这个选项。这个选项允许Oracle执行一个全局索引扫描操作。快速全局扫描读取B-树索引上所有树叶块。初始化文件中的DB_FILE_MULTIBLOCK_READ_COUNT参数可以控制同时被读取的块的数目。

[9]跳跃式扫描

从Oracle9i开始，索引跳跃式扫描特性可以允许优化器使用组合索引，即便索引的前导列没有出现在WHERE子句中。索引跳跃式扫描比全索引扫描要快的多。下面的程序清单显示出性能的差别：

    create index skip1 on emp5(job,empno);

    index created.

 

    select count(*)

    from emp5

    where empno=7900;

 

    Elapsed:00:00:03.13

 

    Execution Plan

    0     SELECT STATEMENT Optimizer=CHOOSE(Cost=4 Card=1 Bytes=5)

    1  0    SORT(AGGREGATE)

    2  1      INDEX(FAST FULL SCAN) OF 'SKIP1'(NON-UNIQUE)

 

    Statistics

 

    6826 consistent gets

    6819 physical   reads

 

    select /*+ index(emp5 skip1)*/ count(*)

    from emp5

    where empno=7900;

 

    Elapsed:00:00:00.56

 

    Execution Plan

    0     SELECT STATEMENT Optimizer=CHOOSE(Cost=6 Card=1 Bytes=5)

    1  0    SORT(AGGREGATE)

    2  1      INDEX(SKIP SCAN) OF 'SKIP1'(NON-UNIQUE)

 

    Statistics

 

    21 consistent gets

    17 physical   reads

 

[10]索引的类型

     B-树索引

    位图索引

     HASH索引

    索引编排表

    反转键索引

    基于函数的索引

    分区索引

    本地和全局索引

如果一个表有大量数据被删除 需要rebuild索引，不然索引文件会变大
[#Oralce](http://hi.baidu.com/tag/Oralce/feeds)

分享到： [](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0# "分享到QQ空间") [](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0# "分享到新浪微博") [](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0# "分享到腾讯微博") [](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0# "分享到人人网")

[举报](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#) 浏览(153) [评论](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#) [转载](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)

### 您可能也喜欢
[](http://hi.baidu.com/yh1350/item/8402ef3d842dec8ef5e4adc0 "上一篇")

[](http://hi.baidu.com/yh1350/item/ce58dfc361c472c1984aa0c0 "下一篇")

评论

 同时评论给 

 同时评论给原文作者 

[ 发布 ](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)

500/0

[收起](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#)|[查看更多](http://hi.baidu.com/yh1350/item/655eb439fa11cc5e81f1a7c0#reply)

[帮助中心](http://hi.baidu.com/go/show/introduce) | [空间客服](http://tieba.baidu.com/p/1781683739) | [投诉中心](http://tousu.baidu.com/hi/add) | [空间协议](http://www.baidu.com/search/hi_contract.html)

©2012 Baidu
