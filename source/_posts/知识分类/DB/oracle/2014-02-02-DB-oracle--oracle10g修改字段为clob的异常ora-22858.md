---
layout: post
title: "oracle10g修改字段为clob的异常ora-22858"
categories: DB
tags: 
 - DB
 - oracle
--- 

# oracle10g修改字段为clob的异常ora-22858

修改oracle 的表的字段VARCHAR2为clob时，当是10g版本的时候包ora-22858异常；

解决办法如下：
列入修改表PLUGIN_CONF，先将字段修改为long型：

ALTER TABLE TSOCNEW.PLUGIN_CONF

MODIFY(TASKCURSOR LONG);
然后在修改为clob型：

ALTER TABLE TSOCNEW.PLUGIN_CONF

MODIFY(TASKCURSOR CLOB);
原因：Oracle的文档对这个错误的描述是：

ORA-22858: invalid alteration of datatype
Cause: An attempt was made to modify the column type to object, REF, nested table, VARRAY or LOB type.
Action: Create a new column of the desired type and copy the current column data to the new type using the appropriate type constructor.

显然是CLOB字段的特殊性，限制了直接修改数据类型。

虽然不能直接修改为CLOB，但是如果记录为空，可以直接修改为LONG类型：
不久前的一篇文章介绍过，对于LONG类型，不管有没有数据存在，可以直接修改为CLOB类型：[http://yangtingkun.itpub.net/post/468/501094](http://yangtingkun.itpub.net/post/468/501094)

SQL> INSERT INTO T_VAR
  2  VALUES (LPAD('A', 4000, 'A'));

1 row created.

SQL> COMMIT;

Commit complete.

SQL> ALTER TABLE T_VAR MODIFY (C CLOB);

Table altered.

对于LONG类型的转换，Oracle并不是简单的将列的定义换成CLOB，而是生成了一个临时列，将数据保存，然后删除原LONG列。

Oracle可以对LONG类型的转换操作进行封装，不知道为什么没有对VARCHAR2类型转换为CLOB进行封装，使得一个简单的ALTER TABLE命令必须通过多个命令才能完成。

至于VARCHAR2到CLOB的转换就没有必要详细说明了，采用在线重定义就能实现。如果有足够的维护时间，也可以直接添加CLOB列，对新增CLOB列赋值、删除原VARCHAR2类型列，最后对新增CLOB列改名。
来源： <[http://www.360doc.com/content/12/0627/10/7662927_220705696.shtml](http://www.360doc.com/content/12/0627/10/7662927_220705696.shtml)>  
