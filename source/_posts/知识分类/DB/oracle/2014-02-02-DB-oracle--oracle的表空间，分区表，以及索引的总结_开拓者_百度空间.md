---
layout: post
title: "oracle的表空间，分区表，以及索引的总结_开拓者_百度空间"
categories: DB
tags: 
 - DB
 - oracle
--- 

# oracle的表空间，分区表，以及索引的总结_开拓者_百度空间

### 分享到

* [QQ空间](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [新浪微博](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [百度搜藏](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [人人网](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [腾讯微博](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [开心网](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [腾讯朋友](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [百度空间](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [豆瓣网](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [搜狐微博](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [百度新首页](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [QQ收藏](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [和讯微博](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [我的淘宝](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [百度贴吧](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
* [更多...](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)

[百度分享](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)

[](http://hi.baidu.com/)

* [广场](http://hi.baidu.com/index)
* [jywv](https://passport.baidu.com/center)
*
* [退出](https://passport.baidu.com/?logout&bdstoken=aa9855a9aad4216b27441b12182ad1a7&u=http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3)
[关注此空间](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)
# [开拓者](http://hi.baidu.com/new/cnbxj)

开拓者

2008-06-02 22:46

## oracle的表空间，分区表，以及索引的总结

表空间：
Oracle的UNDOTBS01.DBF文件太大的解决办法
1、.禁止undo tablespace自动增长
alter database datafile 'full_path\undotbs01.dbf' autoextend off;
2.-- 创建一个新的小空间的undo tablespace
create undo tablespace undotBS2 datafile 'full_path\UNDOTBS02.DBF' size 100m;
-- 设置新的表空间为系统undo_tablespace
alter system set undo_tablespace=undotBS2;
-- Drop 旧的表空间
drop tablespace undotbs1 including contents;
--查看所有表空间的情况
select * from dba_tablespaces
--创建表空间
create tablespace HRPM0
datafile '/oradata/misdb/HRPM0.DBF' size 5m autoextend on next 10m maxsize unlimited
--删除表空间
DROP TABLESPACE data01 INCLUDING CONTENTS AND DATAFILES;
--修改表空间大小
alter database datafile '/path/NADDate05.dbf' resize 100M
分区表：
当表中的数据量不断增大，查询数据的速度就会变慢，应用程序的性能就会下降，这时就应该考虑对表进行分区。表进行分区后，逻辑上表仍然是一张完整的表，只是将表中的数据在物理上存放到多个表空间(物理文件上)，这样查询数据时，不至于每次都扫描整张表。
Oracle中提供了以下几种表分区：
一、范围分区：这种类型的分区是使用列的一组值，通常将该列成为分区键。
示例1：假设有一个CUSTOMER表，表中有数据200000行，我们将此表通过CUSTOMER_ID进行分区，每个分区存储100000行，我们将每个分区保存到单独的表空间中，这样数据文件就可以跨越多个物理磁盘。下面是创建表和分区的代码，如下：
CREATE TABLE CUSTOMER
(
CUSTOMER_ID NUMBER NOT NULL PRIMARY KEY,
FIRST_NAME VARCHAR2(30) NOT NULL,
LAST_NAME VARCHAR2(30) NOT NULL,
PHONE VARCHAR2(15) NOT NULL,
EMAIL VARCHAR2(80),
STATUS CHAR(1)
)
PARTITION BY RANGE (CUSTOMER_ID)
(
PARTITION CUS_PART1 VALUES LESS THAN (100000) TABLESPACE CUS_TS01,
PARTITION CUS_PART2 VALUES LESS THAN (200000) TABLESPACE CUS_TS02
)
注意：在创建表进行分区时，表空间必须先存在，而且建议将不同的分区放入不同的表空间中。
示例2：假设有ORDER_ACTIVITIES表，每6个月对订单进行清理，我们可以按月份对表进行分区，分区代码如下：
CREATE TABLE ORDER_ACTIVITIES
(
ORDER_ID NUMBER(7) NOT NULL,
ORDER_DATE DATE,
TOTAL_AMOUNT NUMBER,
CUSTOTMER_ID NUMBER(7),
PAID CHAR(1)
)
PARTITION BY RANGE (ORDER_DATE)
(
PARTITION ORD_ACT_PART01 VALUES LESS THAN (TO_DATE('01-MAY-2003','DD-MON-YYYY')) TABLESPACE ORD_TS01,
PARTITION ORD_ACT_PART02 VALUES LESS THAN (TO_DATE('01-JUN-2003','DD-MON-YYYY')) TABLESPACE ORD_TS02,
PARTITION ORD_ACT_PART02 VALUES LESS THAN (TO_DATE('01-JUL-2003','DD-MON-YYYY')) TABLESPACE ORD_TS03
)
二、列表分区：该分区的特点是某列的值只有几个，基于这样的特点我们可以采用列表分区。
示例1：
CREATE TABLE PROBLEM_TICKETS
(
PROBLEM_ID NUMBER(7) NOT NULL PRIMARY KEY,
DESCRIPTION VARCHAR2(2000),
CUSTOMER_ID NUMBER(7) NOT NULL,
DATE_ENTERED DATE NOT NULL,
STATUS VARCHAR2(20)
)
PARTITION BY LIST (STATUS)
(
PARTITION PROB_ACTIVE VALUES ('ACTIVE') TABLESPACE PROB_TS01,
PARTITION PROB_INACTIVE VALUES ('INACTIVE') TABLESPACE PROB_TS02
)
三、散列分区：这类分区是在列值上使用散列算法，以确定将行放入哪个分区中。当列的值没有合适的条件时，建议使用散列分区。请看下列示例：
示例1：
CREATE TABLE HASH_TABLE
(
COL NUMBER(8),
INF VARCHAR2(100)
)
PARTITION BY HASH (COL)
(
PARTITION PART01 TABLESPACE HASH_TS01,
PARTITION PART02 TABLESPACE HASH_TS02,
PARTITION PART03 TABLESPACE HASH_TS03
)
四、复合范围列表分区：这种分区是基于范围分区和列表分区，表首先按某列进行范围分区，然后再按某列进行列表分区，分区之中的分区被称为子分区。
示例1：
CREATE TABLE SALES
(
PRODUCT_ID VARCHAR2(5),
SALES_DATE DATE,
SALES_COST NUMBER(10),
STATUS VARCHAR2(20)
)
PARTITION BY RANGE(SALES_DATE)
SUBPARTITION BY LIST (STATUS)
(
PARTITION P1 VALUES LESS THAN (TO_DATE('2003-01-01','YYYY-MM-DD')) TABLESPACE P1_TS
(
SUBPARTITION P1SUB1 VALUES ('ACTIVE') TABLESPACE SUBP1_TS1,
SUBPARTITION P1SUB2 VALUES ('INACTIVE') TABLESPACE SUBP1_TS2
),
PARTITION P2 VALUES LESS THAN (TO_DATE('2003-03-01','YYYY-MM-DD')) TABLESPACE P2_TS
(
SUBPARTITION P2SUB1 VALUES ('ACTIVE') TABLESPACE SUBP2_TS1,
SUBPARTITION P2SUB2 VALUES ('INACTIVE') TABLESPACE SUBP2_TS2
)
)
示例2：使用TEMPLATE模板
CREATE TABLE SALES
(
PRODUCT_ID VARCHAR2(5),
SALES_DATE DATE,
SALES_COST NUMBER(10),
STATUS VARCHAR2(20)
)
PARTITION BY RANGE(SALES_DATE)
SUBPARTITION BY LIST (STATUS)
SUBPARTITION TEMPLATE
(
SUBPARTITION SUB1 VALUES ('ACTIVE') TABLESPACE SUBP1_TS1,
SUBPARTITION SUB2 VALUES ('INACTIVE') TABLESPACE SUBP2_TS2
)
(
PARTITION P1 VALUES LESS THAN (TO_DATE('2003-01-01','YYYY-MM-DD')) TABLESPACE P1_TS,
PARTITION P2 VALUES LESS THAN (TO_DATE('2003-03-01','YYYY-MM-DD')) TABLESPACE P2_TS
)
五、复合范围散列分区：这种分区是基于范围分区和散列分区，表首先按某列进行范围分区，然后再按某列进行散列分区。与上面的定义方式非常的类似，在此不单独举例。
表分区对于用户来说是透明的，我们在插入数据时Oracle会自动判断插入的数据，然后放入相应的表分区中。但有时我们想单独查询某个分区中的数据时，就必须手工指定分区的名称。
示例1：(此示例基于：四、复合范围列表分区的示例一)
向SALES表插入记录，不必指定表分区。
INSERT INTO SALES VALUES('00001','01-1月-02',100,'ACTIVE')
/
INSERT INTO SALES VALUES('00002','01-1月-01',200,'ACTIVE')
/
INSERT INTO SALES VALUES('00003','01-2月-03',300,'INACTIVE')
/
INSERT INTO SALES VALUES('00004','04-2月-03',300,'INACTIVE')
/
INSERT INTO SALES VALUES('00005','04-2月-02',300,'INACTIVE')
/
不指定表分区查看SALES表信息：
SELECT * FROM SALES; 结果如下所示：
指定P1表分区查询SALES表信息：
SELECT * FROM SALES PARTITION(P1); 结果如下所示：
指定P1SUB1子分区查询SALES表信息:
SELECT * FROM SALES SUBPARTITION(P1SUB1); 结果如下所示：
示例2：(此示例基于：四、复合范围列表分区的示例二)
示例2基于TEMPLATE模板的表分区，查询稍稍烦琐一点。
指定P1表分区查询SALES表信息：
SELECT * FROM SALES PARTITION(P1); 结果如下所示,和刚才查询一致。
指定SUB1子分区查询SALES表信息:
SELECT * FROM SALES SUBPARTITION(SUB1); 出现如下错误信息：
怎么解决以上问题呢？我们通过sys模式查看分区信息的数据字典,如下：
可以看出子分区不叫SUB1，而是P1_SUB1,重新查询信息，如下图所示：
有关表分区的一些维护性操作：
一、添加分区
以下代码给SALES表添加了一个P3分区
ALTER TABLE SALES ADD PARTITION P3 VALUES LESS THAN(TO_DATE('2003-06-01','YYYY-MM-DD'));
注意：以上添加的分区界限应该高于最后一个分区界限。
以下代码给SALES表的P3分区添加了一个P3SUB1子分区
ALTER TABLE SALES MODIFY PARTITION P3 ADD SUBPARTITION P3SUB1 VALUES('COMPLETE');
二、删除分区
以下代码删除了P3表分区：
ALTER TABLE SALES DROP PARTITION P3;
在以下代码删除了P4SUB1子分区：
ALTER TABLE SALES DROP SUBPARTITION P4SUB1;
注意：如果删除的分区是表中唯一的分区，那么此分区将不能被删除，要想删除此分区，必须删除表。
三、截断分区
截断某个分区是指删除某个分区中的数据，并不会删除分区，也不会删除其它分区中的数据。当表中即使只有一个分区时，也可以截断该分区。通过以下代码截断分区：
ALTER TABLE SALES TRUNCATE PARTITION P2;
通过以下代码截断子分区：
ALTER TABLE SALES TRUNCATE SUBPARTITION P2SUB2;
四、合并分区
合并分区是将相邻的分区合并成一个分区，结果分区将采用较高分区的界限，值得注意的是，不能将分区合并到界限较低的分区。以下代码实现了P1 P2分区的合并：
ALTER TABLE SALES MERGE PARTITIONS P1,P2 INTO PARTITION P2;
五、拆分分区
拆分分区将一个分区拆分两个新分区，拆分后原来分区不再存在。注意不能对HASH类型的分区进行拆分。
ALTER TABLE SALES SBLIT PARTITION P2 AT(TO_DATE('2003-02-01','YYYY-MM-DD'))
INTO (PARTITION P21,PARTITION P22);
六、接合分区(coalesca)
结合分区是将散列分区中的数据接合到其它分区中，当散列分区中的数据比较大时，可以增加散列分区，然后进行接合，值得注意的是，接合分区只能用于散列分区中。通过以下代码进行接合分区：
ALTER TABLE SALES COALESCA PARTITION;
七、重命名表分区
以下代码将P21更改为P2
ALTER TABLE SALES RENAME PARTITION P21 TO P2;
九、跨分区查询
select sum( *) from (
(select count(*) cn from t_table_SS PARTITION (P200709_1)
union all
select count(*) cn from t_table_SS PARTITION (P200709_2));
十、查询表上有多少分区
SELECT * FROM useR_TAB_PARTITIONS WHERE TABLE_NAME='tableName'
十一、查询索引信息
select object_name,object_type,tablespace_name,sum(value)
from v$segment_statistics
where statistic_name IN ('physical reads','physical write','logical reads')and object_type='INDEX'
group by object_name,object_type,tablespace_name
order by 4 desc
--显示数据库所有分区表的信息：
select * from DBA_PART_TABLES
--显示当前用户可访问的所有分区表信息:
select * from ALL_PART_TABLES
--显示当前用户所有分区表的信息：
select * from USER_PART_TABLES
--显示表分区信息 显示数据库所有分区表的详细分区信息：
select * from DBA_TAB_PARTITIONS
--显示当前用户可访问的所有分区表的详细分区信息：
select * from ALL_TAB_PARTITIONS
--显示当前用户所有分区表的详细分区信息：
select * from USER_TAB_PARTITIONS
--显示子分区信息 显示数据库所有组合分区表的子分区信息：
select * from DBA_TAB_SUBPARTITIONS
--显示当前用户可访问的所有组合分区表的子分区信息：
select * from ALL_TAB_SUBPARTITIONS
--显示当前用户所有组合分区表的子分区信息：
select * from USER_TAB_SUBPARTITIONS
--显示分区列 显示数据库所有分区表的分区列信息：
select * from DBA_PART_KEY_COLUMNS
--显示当前用户可访问的所有分区表的分区列信息：
select * from ALL_PART_KEY_COLUMNS
--显示当前用户所有分区表的分区列信息：
select * from USER_PART_KEY_COLUMNS
--显示子分区列 显示数据库所有分区表的子分区列信息：
select * from DBA_SUBPART_KEY_COLUMNS
--显示当前用户可访问的所有分区表的子分区列信息：
select * from ALL_SUBPART_KEY_COLUMNS
--显示当前用户所有分区表的子分区列信息：
select * from USER_SUBPART_KEY_COLUMNS
--怎样查询出oracle数据库中所有的的分区表
select * from user_tables a where a.partitioned='YES'
--删除一个表的数据是
truncate table table_name;
--删除分区表一个分区的数据是
alter table table_name truncate partition p5;
注：分区根据具体情况选择。
表分区有以下优点：
1、数据查询：数据被存储到多个文件上，减少了I/O负载，查询速度提高。
2、数据修剪：保存历史数据非常的理想。
3、备份：将大表的数据分成多个文件，方便备份和恢复。
4、并行性：可以同时向表中进行DML操作，并行性性能提高。
================================================
索引：
1、一般索引：
create index index_name on table(col_name);
2、Oracle 分区索引详解
语法：Table Index
CREATE [UNIQUE|BITMAP] INDEX [schema.]index_name
ON [schema.]table_name [tbl_alias]
(col [ASC | DESC]) index_clause index_attribs
index_clauses:
分以下两种情况
1. Local Index
就是索引信息的存放位置依赖于父表的Partition信息，换句话说创建这样的索引必须保证父表是Partition
1.1 索引信息存放在父表的分区所在的表空间。但是仅可以创建在父表为HashTable或者composite分区表的。
LOCAL STORE IN (tablespace)
1.2 仅可以创建在父表为HashTable或者composite分区表的。并且指定的分区数目要与父表的分区数目要一致
LOCAL STORE IN (tablespace) (PARTITION [partition [LOGGING|NOLOGGING] [TABLESPACE {tablespace|DEFAULT}] [PCTFREE int] [PCTUSED int] [INITRANS int] [MAXTRANS int] [STORAGE storage_clause] [STORE IN {tablespace_name|DEFAULT] [SUBPARTITION [subpartition [TABLESPACE tablespace]]]])
1.3 索引信息存放在父表的分区所在的表空间，这种语法最简单，也是最常用的分区索引创建方式。
Local
1.4 并且指定的Partition 数目要与父表的Partition要一致
LOCAL (PARTITION [partition
[LOGGING|NOLOGGING]
[TABLESPACE {tablespace|DEFAULT}]
[PCTFREE int]
[PCTUSED int]
[INITRANS int]
[MAXTRANS int]
[STORAGE storage_clause]
[STORE IN {tablespace_name|DEFAULT]
[SUBPARTITION [subpartition [TABLESPACE tablespace]]]])
Global Index
索引信息的存放位置与父表的Partition信息完全不相干。甚至父表是不是分区表都无所谓的。语法如下：
GLOBAL PARTITION BY RANGE (col_list)
( PARTITION partition VALUES LESS THAN (value_list)
[LOGGING|NOLOGGING]
[TABLESPACE {tablespace|DEFAULT}]
[PCTFREE int]
[PCTUSED int]
[INITRANS int]
[MAXTRANS int]
[STORAGE storage_clause] )
但是在这种情况下，如果父表是分区表，要删除父表的一个分区都必须要更新Global Index ,否则索引信息不正确
ALTER TABLE TableName DROP PARTITION PartitionName Update Global Indexes
--查询索引
select object_name,object_type,tablespace_name,sum(value)
from v$segment_statistics
where statistic_name IN ('physical reads','physical write','logical reads')and object_type='INDEX'
group by object_name,object_type,tablespace_name
order by 4 desc
[#Oracle](http://hi.baidu.com/tag/Oracle/feeds)

分享到： [](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3# "分享到QQ空间") [](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3# "分享到新浪微博") [](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3# "分享到腾讯微博") [](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3# "分享到人人网")

[举报](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#) 浏览(64) [评论](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#) [转载](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)

### 您可能也喜欢
[](http://hi.baidu.com/cnbxj/item/7e7a9de4639d4ab72f140be7 "上一篇")

[](http://hi.baidu.com/cnbxj/item/535c1a31f17a51f1e7bb7ae7 "下一篇")

评论

 同时评论给 

 同时评论给原文作者 

[ 发布 ](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)

500/0

[收起](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#)|[查看更多](http://hi.baidu.com/cnbxj/item/9b1f8128e885e90842634ae3#reply)

[帮助中心](http://hi.baidu.com/go/show/introduce) | [空间客服](http://tieba.baidu.com/p/1781683739) | [投诉中心](http://tousu.baidu.com/hi/add) | [空间协议](http://www.baidu.com/search/hi_contract.html)

©2012 Baidu
