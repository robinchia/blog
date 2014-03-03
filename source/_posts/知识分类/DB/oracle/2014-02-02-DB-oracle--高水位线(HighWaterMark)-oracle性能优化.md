---
layout: post
title: "高水位线(High Water Mark) - oracle性能优化"
categories: DB
tags: 
 - DB
 - oracle
--- 

# 高水位线(High Water Mark) - oracle性能优化

  [博客首页](http://blog.chinaunix.net/) [注册](http://blog.chinaunix.net/register.php) [建议与交流](http://bbs.chinaunix.net/forumdisplay.php?fid=51) [排行榜](http://blog.chinaunix.net/top/) [加入友情链接](http://blog.chinaunix.net/u2/60332/)  ![]() [推荐](http://blog.chinaunix.net/u2/star.php?blogid=60332 "给此博客推荐值") [投诉](http://blog.chinaunix.net/u2/complaint.php?blogid=60332 "投诉此博客") 搜索：  [帮助](http://blog.chinaunix.net/help/)  **huaihe0410**

 努力,坚持,成败在此    [huaihe0410.cublog.cn](http://huaihe0410.cublog.cn/) 
* [管理博客](http://control.cublog.cn/)
* [发表文章](http://control.cublog.cn/article_new.php)
* [留言](http://blog.chinaunix.net/u2/60332/guestbook.html)
* [收藏夹](http://blog.chinaunix.net/u2/60332/links.html)
* [博客圈](http://blog.chinaunix.net/u2/60332/group.html)
* [音乐](http://blog.chinaunix.net/u2/60332/music.html)
* [相册](http://blog.chinaunix.net/u2/60332/photo.html)
* [文章](http://blog.chinaunix.net/u2/60332/article.html)

* [· AIX](http://blog.chinaunix.net/u2/60332/article_138247.html)
* [· 协议   }](http://blog.chinaunix.net/u2/60332/article_125862.html)

* [· socket](http://blog.chinaunix.net/u2/60332/article_126906.html)
* [· jsp](http://blog.chinaunix.net/u2/60332/article_93657.html)
* [· linux/unix   }](http://blog.chinaunix.net/u2/60332/article_77152.html)

* [· cvs](http://blog.chinaunix.net/u2/60332/article_138139.html)
* [· 网络](http://blog.chinaunix.net/u2/60332/article_120120.html)
* [· shell](http://blog.chinaunix.net/u2/60332/article_133308.html)
* [· 指令](http://blog.chinaunix.net/u2/60332/article_133261.html)
* [· mysql   }](http://blog.chinaunix.net/u2/60332/article_78134.html)

* [· cluster](http://blog.chinaunix.net/u2/60332/article_141554.html)
* [· oracle   }](http://blog.chinaunix.net/u2/60332/article_77026.html)

* [· oracle 并行](http://blog.chinaunix.net/u2/60332/article_105725.html)
* [· oracle1011新特性](http://blog.chinaunix.net/u2/60332/article_105724.html)
* [· Oracle9i初始化参数中文说明](http://blog.chinaunix.net/u2/60332/article_105726.html)
* [· oracle性能优化](http://blog.chinaunix.net/u2/60332/article_80194.html)
* [· oracle索引组织表](http://blog.chinaunix.net/u2/60332/article_105313.html)
* [· oracle分区表](http://blog.chinaunix.net/u2/60332/article_80555.html)
* [· oracle函数](http://blog.chinaunix.net/u2/60332/article_105723.html)
* [· oracle字符集](http://blog.chinaunix.net/u2/60332/article_125712.html)
* [· PL/SQL](http://blog.chinaunix.net/u2/60332/article_110365.html)
* [· python   }](http://blog.chinaunix.net/u2/60332/article_99870.html)

* [· smtp](http://blog.chinaunix.net/u2/60332/article_120449.html)
* [· socket](http://blog.chinaunix.net/u2/60332/article_126980.html)
* [· 中文字符](http://blog.chinaunix.net/u2/60332/article_126157.html)
* [· resin/java](http://blog.chinaunix.net/u2/60332/article_97234.html)
* [· 编码](http://blog.chinaunix.net/u2/60332/article_125210.html)
* [· 编译](http://blog.chinaunix.net/u2/60332/article_106939.html)
* [· 测试](http://blog.chinaunix.net/u2/60332/article_111905.html)
* [· 其他](http://blog.chinaunix.net/u2/60332/article_77025.html)
* [首页](http://blog.chinaunix.net/u2/60332/index.html)  **关于作者** ![]( "收起")   [![]()](http://blog.chinaunix.net/u2/60332/)
**我的分类** ![]( "收起") 
[![]()](http://blog.chinaunix.net/u2/rss.php?id=60332)
  **高水位线(High Water Mark)****什么是水线**(High Water Mark)?
----------------------------
所有的oracle段(segments，在此，为了理解方便，建议把segment作为表的一个同义词) 都有一个在段内容纳数据的上限，我们把这个上限称为"high water mark"或HWM。这个HWM是一个标记，用来说明已经有多少没有使用的数据块分配给这个segment。HWM通常增长的幅度为一次5个数据块，原则上HWM只会增大，不会缩小，即使将表中的数据全部删除，HWM还是为原值，由于这个特点，使HWM很象一个水库的历史最高水位，这也就是HWM的原始含义，当然不能说一个水库没水了，就说该水库的历史最高水位为0。但是如果我们在表上使用了truncate命令，则该表的HWM会被重新置为0。
**HWM数据库的操作有如下影响：**
a) 全表扫描通常要读出直到HWM标记的所有的属于该表数据库块，即使该表中没有任何数据。
b) 即使HWM以下有空闲的数据库块，键入在插入数据时使用了append关键字，则在插入时使用HWM以上的数据块，此时HWM会自动增大。
**如何知道一个表的HWM？**
a) 首先对表进行分析:
ANALYZE TABLE <tablename> ESTIMATE/COMPUTE STATISTICS;
b) SELECT blocks, empty_blocks, num_rows
FROM user_tables
WHERE table_name = <tablename>;
BLOCKS 列代表该表中曾经使用过得数据库块的数目，即水线。
EMPTY_BLOCKS 代表分配给该表，但是在水线以上的数据库块，即从来没有使用的数据块。
让我们以一个有28672行的BIG_EMP1表为例进行说明：
1) SQL> SELECT segment_name,segment_type,blocks
FROM dba_segments
WHERE segment_name='BIG_EMP1';
SEGMENT_NAME SEGMENT_TYPE BLOCKS EXTENTS
----------------------------- ----------------- ---------- -------
BIG_EMP1 TABLE 1024 2
1 row selected.
2) SQL> ANALYZE TABLE big_emp1 ESTIMATE STATISTICS;
Statement processed.
3) SQL> SELECT table_name,num_rows,blocks,empty_blocks
FROM user_tables
WHERE table_name='BIG_EMP1';
TABLE_NAME NUM_ROWS BLOCKS EMPTY_BLOCKS
------------------------------ ---------- ---------- ------------
BIG_EMP1 28672 700 323
1 row selected.
**注意：**
BLOCKS + EMPTY_BLOCKS (700+323=1023)比DBA_SEGMENTS.BLOCKS少个数据库块，这是因为有一个数据库块被保留用作segment header。DBA_SEGMENTS.BLOCKS 表示分配给这个表的所有的数据库块的数目。USER_TABLES.BLOCKS表示已经使用过的数据库块的数目。
4) SQL> SELECT COUNT (DISTINCT
DBMS_ROWID.ROWID_BLOCK_NUMBER(rowid)||
DBMS_ROWID.ROWID_RELATIVE_FNO(rowid)) "Used"
FROM big_emp1;
Used
----------
700
1 row selected.
5) SQL> DELETE from big_emp1;
28672 rows processed.
6) SQL> commit;
Statement processed.
7) SQL> ANALYZE TABLE big_emp1 ESTIMATE STATISTICS;
Statement processed.
8) SQL> SELECT table_name,num_rows,blocks,empty_blocks
FROM user_tables
WHERE table_name='BIG_EMP1';
TABLE_NAME NUM_ROWS BLOCKS EMPTY_BLOCKS
------------------------------ ---------- ---------- ------------
BIG_EMP1 0 700 323
1 row selected.
9) SQL> SELECT COUNT (DISTINCT
DBMS_ROWID.ROWID_BLOCK_NUMBER(rowid)||
DBMS_ROWID.ROWID_RELATIVE_FNO(rowid)) "Used"
FROM big_emp1;
Used
----------
0 -- 这表名没有任何数据库块容纳数据，即表中无数据
1 row selected.
10) SQL> TRUNCATE TABLE big_emp1;
Statement processed.
11) SQL> ANALYZE TABLE big_emp1 ESTIMATE STATISTICS;
Statement processed.
12) SQL> SELECT table_name,num_rows,blocks,empty_blocks
2> FROM user_tables
3> WHERE table_name='BIG_EMP1';
TABLE_NAME NUM_ROWS BLOCKS EMPTY_BLOCKS
------------------------------ ---------- ---------- ------------
BIG_EMP1 0 0 511
1 row selected.
13) SQL> SELECT segment_name,segment_type,blocks
FROM dba_segments
WHERE segment_name='BIG_EMP1';
SEGMENT_NAME SEGMENT_TYPE BLOCKS EXTENTS
----------------------------- ----------------- ---------- -------
BIG_EMP1 TABLE 512 1
1 row selected.
**注意:**
TRUNCATE命令回收了由delete命令产生的空闲空间，注意该表分配的空间由原先的1024块降为512块。
为了保留由delete命令产生的空闲空间，可以使用
TRUNCATE TABLE big_emp1 REUSE STORAGE
用此命令后，该表还会是原先的1024块。
************************************************************

 
**Oracle表段中的高水位线HWM**  

 
在Oracle数据的存储中，可以把存储空间想象为一个水库，数据想象为水库中的水。水库中的水的位置有一条线叫做水位线，在Oracle中，这条线被称为高水位线（High-warter mark, HWM）。在数据库表刚建立的时候，由于没有任何数据，所以这个时候水位线是空的，也就是说HWM为最低值。当插入了数据以后，高水位线就会上涨，但是这里也有一个特性，就是如果你采用delete语句删除数据的话，数据虽然被删除了，但是高水位线却没有降低，还是你刚才删除数据以前那么高的水位。也就是说，这条高水位线在日常的增删操作中只会上涨，不会下跌。

下面我们来谈一下Oracle中Select语句的特性。Select语句会对表中的数据进行一次扫描，但是究竟扫描多少数据存储块呢，这个并不是说数据库中有多少数据，Oracle就扫描这么大的数据块，而是Oracle会扫描高水位线以下的数据块。现在来想象一下，如果刚才是一张刚刚建立的空表，你进行了一次Select操作，那么由于高水位线HWM在最低的0位置上，所以没有数据块需要被扫描，扫描时间会极短。而如果这个时候你首先插入了一千万条数据，然后再用delete语句删除这一千万条数据。由于插入了一千万条数据，所以这个时候的高水位线就在一千万条数据这里。后来删除这一千万条数据的时候，由于delete语句不影响高水位线，所以高水位线依然在一千万条数据这里。这个时候再一次用select语句进行扫描，虽然这个时候表中没有数据，但是由于扫描是按照高水位线来的，所以需要把一千万条数据的存储空间都要扫描一次，也就是说这次扫描所需要的时间和扫描一千万条数据所需要的时间是一样多的。所以有时候有人总是经常说，怎么我的表中没有几条数据，但是还是这么慢呢，这个时候其实奥秘就是这里的高水位线了。

    那有没有办法让高水位线下降呢，其实有一种比较简单的方法，那就是采用TRUNCATE语句进行删除数据。采用TRUNCATE语句删除一个表的数据的时候，类似于重新建立了表，不仅把数据都删除了，还把HWM给清空恢复为0。所以如果需要把表清空，在有可能利用TRUNCATE语句来删除数据的时候就利用TRUNCATE语句来删除表，特别是那种数据量有可能很大的临时存储表。

    在手动段空间管理（Manual Segment Space Management）中，段中只有一个HWM，但是在Oracle9iRelease1才添加的自动段空间管理（Automatic Segment Space Management）中，又有了一个低HWM的概念出来。为什么有了HWM还又有一个低HWM呢，这个是因为自动段空间管理的特性造成的。在手段段空间管理中，当数据插入以后，如果是插入到新的数据块中，数据块就会被自动格式化等待数据访问。而在自动段空间管理中，数据插入到新的数据块以后，数据块并没有被格式化，而是在第一次在第一次访问这个数据块的时候才格式化这个块。所以我们又需要一条水位线，用来标示已经被格式化的块。这条水位线就叫做低HWM。一般来说，低HWM肯定是低于等于HWM的。

 
 

************************************************
**修正ORACLE表的高水位线**

 

在ORACLE中，执行对表的删除操作不会降低该表的高水位线。而全表扫描将始终读取一个段(extent)中所有**低于高水位线标记**的块。如果在执行删除操作后不降低高水位线标记，则将导致查询语句的性能低下。下面的方法都可以降低高水位线标记。

1.执行表重建指令 alter table *table_name* move;
(在线转移表空间ALTER TABLE 。。。 MOVE TABLESPACE 。。。
 ALTER TABLE 。。。 MOVE 后面不跟参数也行，
 不跟参数表还是在原来的表空间，move后记住重建索引
 如果以后还要继续向这个表增加数据，没有必要move，
 只是释放出来的空间，只能这个表用，其他的表或者segment无法使用该空间
 )
2.执行alter table *table_name* shrink space; 注意，此命令为Oracle 10g新增功能，再执行该指令之前必须允许行移动 alter table *table_name* enable row movement;
3.复制要保留的数据到临时表t，drop原表，然后rename临时表t为原表
4.emp/imp
5.alter   table  table_name  deallocate   unused  
6.尽量truncate 吧

 
 

   发表于： 2008-03-13，修改于： 2008-03-13 16:42 已浏览4027次，有评论1条 [推荐](http://blog.chinaunix.net/u2/star.php?blogid=60332&artid=495441 "推荐这篇文章") [投诉](http://blog.chinaunix.net/u2/complaint.php?blogid=60332&artid=495441 "投诉这篇文章")
  **网友评论**  **凝结水晶** 时间：2010-06-20 14:09:09 IP地址：122.233.180.★  **** SQL> select  EMPTY_BLOCKS,NUM_ROWS,BLOCKS,AVG_SPACE,AVG_ROW_LEN from user_tables where TABLE_NAME=upper('lyq_index_test');
EMPTY_BLOCKS   NUM_ROWS     BLOCKS  AVG_SPACE AVG_ROW_LEN
------------ ---------- ---------- ---------- -----------
          98     821426       3358       3330         116
Elapsed: 00:00:00.05
SQL> 
SQL> 
SQL> 
SQL> select  segment_name,segment_type,blocks 
  2  from dba_segments
  3  where segment_name=upper('lyq_index_test');
SEGMENT_NAME                                                                      SEGMENT_TYPE           BLOCKS
--------------------------------------------------------------------------------- ------------------ ----------
LYQ_INDEX_TEST                                                                    TABLE                    3456
为什么我从user_tables 表中出来的和dba_segments中出来的blocks是相等的？不是说会有一个保留的segments作为header吗？
  **发表评论** Copyright © 2001-2010 ChinaUnix.net All Rights Reserved

感谢所有关心和支持过ChinaUnix的朋友们
页面生成时间：0.07427

[京ICP证041476号](http://www.miibeian.gov.cn/)
