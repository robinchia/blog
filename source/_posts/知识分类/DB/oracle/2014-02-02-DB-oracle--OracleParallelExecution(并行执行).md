---
layout: post
title: "Oracle Parallel Execution(并行执行)"
categories: DB
tags: 
 - DB
 - oracle
--- 

# Oracle Parallel Execution(并行执行)

您还未登录！|[登录](https://passport.csdn.net/account/login)|[注册](https://passport.csdn.net/account/register)|[帮助](https://passport.csdn.net/help/faq)

* [首页](http://www.csdn.net/)
* [业界](http://news.csdn.net/)
* [移动](http://mobile.csdn.net/)
* [云计算](http://cloud.csdn.net/)
* [研发](http://sd.csdn.net/)
* [论坛](http://bbs.csdn.net/)
* [博客](http://blog.csdn.net/)
* [下载](http://download.csdn.net/)
* 
## [更多]()

# [David Dai -- Focus on Oracle](http://blog.csdn.net/tianlesoftware)

## The important thing in life is to have a great aim ,and the determination to attain it！

* [![]()目录视图](http://blog.csdn.net/tianlesoftware?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/tianlesoftware?viewmode=list)
* [![]()订阅](http://blog.csdn.net/tianlesoftware/rss/list)
[公告：博客新增直接引用代码功能](https://code.csdn.net/blog/12)        [专访李铁军：从医生到金山首席安全专家的转变](http://www.csdn.net/article/2013-08-06/2816471)      [CSDN博客频道自定义域名、标签搜索功能上线啦！](http://blog.csdn.net/csdnproduct/article/details/9495139)      [独一无二的职位：开源社区经理](http://blog.csdn.net/adali/article/details/9813651)

### [Oracle Parallel Execution(并行执行)]()

分类： [Oracle Advanced Knowledge](http://blog.csdn.net/tianlesoftware/article/category/706520) [Oracle Performance](http://blog.csdn.net/tianlesoftware/article/category/668807)  2010-09-01 02:20 14688人阅读 [评论]()(7) [收藏]( "收藏") [举报]( "举报")
[parallel](http://blog.csdn.net/tag/details.html?tag=parallel)[oracle](http://blog.csdn.net/tag/details.html?tag=oracle)[sql](http://blog.csdn.net/tag/details.html?tag=sql)[insert](http://blog.csdn.net/tag/details.html?tag=insert)[table](http://blog.csdn.net/tag/details.html?tag=table)[iterator](http://blog.csdn.net/tag/details.html?tag=iterator)

 

关于Oracle 的并行执行，Oracle 官方文档有详细的说明：

                                Using Parallel Execution

[http://download.oracle.com/docs/cd/E11882_01/server.112/e10837/parallel.htm#VLDBG010](http://download.oracle.com/docs/cd/E11882_01/server.112/e10837/parallel.htm#VLDBG010)

This chapter covers tuning in a parallel execution environment and discusses the following topics:

·         [Introduction to Parallel Execution](http://download.oracle.com/docs/cd/E11882_01/server.112/e10837/parallel001.htm#CACEEBJJ)

·         [How Parallel Execution Works](http://download.oracle.com/docs/cd/E11882_01/server.112/e10837/parallel002.htm#i1014083)

·         [Types of Parallelism](http://download.oracle.com/docs/cd/E11882_01/server.112/e10837/parallel003.htm#CACBAABB)

·         [Initializing and Tuning Parameters for Parallel Execution](http://download.oracle.com/docs/cd/E11882_01/server.112/e10837/parallel004.htm#i1007196)

·         [Tuning General Parameters for Parallel Execution](http://download.oracle.com/docs/cd/E11882_01/server.112/e10837/parallel005.htm#i1007962)

·         [Monitoring Parallel Execution Performance](http://download.oracle.com/docs/cd/E11882_01/server.112/e10837/parallel006.htm#i1008510)

·         [Miscellaneous Parallel Execution Tuning Tips](http://download.oracle.com/docs/cd/E11882_01/server.112/e10837/parallel007.htm#i1008847)

 

一．     并行（Parallel）和OLAP系统

并行的实现机制是： 首先，Oracle 会创建一个进程用于协调并行服务进程之间的信息传递，这个协调进程将需要操作的数据集（比如表的数据块）分割成很多部分，称为并行处理单元，然后并行协调进程给每个并行进程分配一个数据单元。比如有四个并行服务进程，他们就会同时处理各自分配的单元，当一个并行服务进程处理完毕后，协调进程就会给它们分配另外的单元，如此反复，直到表上的数据都处理完毕，最后协调进程负责将每个小的集合合并为一个大集合作为最终的执行结果，返回给用户。

 

并行处理的机制实际上就是把一个要扫描的数据集分成很多小数据集，Oracle 会启动几个并行服务进程同时处理这些小数据集，最后将这些结果汇总，作为最终的处理结果返回给用户。

 

这种数据并行处理方式在OLAP系统中非常有用，OLAP系统的表通常来说都是非常大，如果系统的CPU比较多，让所有的CPU共同来处理这些数据，效果就会比串行执行要高的多。

 

然而对于OLTP系统，通常来讲，并行并不合适，原因是OLTP系统上几乎在所有的SQL操作中，数据访问路劲基本上以索引访问为主，并且返回结果集非常小，这样的SQL 操作的处理速度一般非常快，不需要启用并行。

 

 

二． 并行处理的机制

                当Oracle 数据库启动的时候，实例会根据初始化参数：

                                PARALLEL_MIN_SERVERS=n

                的值来预先分配n个并行服务进程，当一条SQL 被CBO判断为需要并行执行时发出SQL的会话进程变成并行协助进程，它按照并行执行度的值来分配进程服务器进程。

 

                首先协调进程会使用ORACLE 启动时根据参数： parallel_min_servers=n的值启动相应的并行服务进程，如果启动的并行服务器进程数不足以满足并行度要求的并行服务进程数，则并行协调进程将额外启动并行服务进程以提供更多的并行服务进程来满足执行的需求。 然后星星协调进程将要处理的对象划分成小数据片，分给并行服务进程处理；并行服务进程处理完毕后将结果发送给并行协调进程，然后由并行协调进程将处理结果汇总并发送给用户。

 

                刚才讲述的是一个并行处理的基本流程。 实际上，在一个并行执行的过程中，还存在着并行服务进程之间的通信问题。

                在一个并行服务进程需要做两件事情的时候，它会再启用一个进程来配和当前的进程完成一个工作，比如这样的一条SQL语句：

                Select * from employees order by last_name;

               

                假设employees表中last_name 列上没有索引，并且并行度为4，此时并行协调进程会分配4个并行服务进程对表employees进行全表扫描操作，因为需要对结果集进行排序，所以并行协调进程会额外启用4个并行服务进程，用于处理4个进程传送过来的数据，这新启用的用户处理传递过来数据的进程称为父进程，用户传出数据（最初的4个并行服务进程）成为子进程，这样整个并行处理过程就启用了8个并行服务进程。 其中每个单独的并行服务进程的行为叫作并行的内部操作，而并行服务进程之间的数据交流叫做并行的交互操作。

                这也是有时我们发现并行服务进程数量是并行度的2倍，就是因为启动了并行服务父进程操作的缘故。

 

 

三. 读懂一个并行处理的执行计划

 

CREATE TABLE emp2 AS SELECT * FROM employees;

ALTER TABLE emp2 PARALLEL 2;

 

EXPLAIN PLAN FOR

  SELECT SUM(salary) FROM emp2 GROUP BY department_id;

SELECT PLAN_TABLE_OUTPUT FROM TABLE(DBMS_XPLAN.DISPLAY());

 

--------------------------------------------------------------------------------------------------------

| Id  | Operation                | Name     | Rows  | Bytes | Cost (%CPU) |    TQ  |IN-OUT| PQ Distrib |

--------------------------------------------------------------------------------------------------------

|   0 | SELECT STATEMENT         |          |   107 |  2782 |     3 (34)  |        |      |            |

|   1 |  PX COORDINATOR          |          |       |       |             |        |      |            |

|   2 |   PX SEND QC (RANDOM)    | :TQ10001 |   107 |  2782 |     3 (34)  |  Q1,01 | P->S | QC (RAND)  |

|   3 |    HASH GROUP BY         |          |   107 |  2782 |     3 (34)  |  Q1,01 | PCWP |            |

|   4 |     PX RECEIVE           |          |   107 |  2782 |     3 (34)  |  Q1,01 | PCWP |            |

|   5 |      PX SEND HASH        | :TQ10000 |   107 |  2782 |     3 (34)  |  Q1,00 | P->P | HASH       |

|   6 |       HASH GROUP BY      |          |   107 |  2782 |     3 (34)  |  Q1,00 | PCWP |            |

|   7 |        PX BLOCK ITERATOR |          |   107 |  2782 |     2 (0)   |  Q1,00 | PCWP |            |

|   8 |         TABLE ACCESS FULL| EMP2     |   107 |  2782 |     2 (0)   |  Q1,00 | PCWP |            |

--------------------------------------------------------------------------------------------------------

 

The table EMP2 is scanned in parallel by one set of slaves while the aggregation for the GROUP BY is done by the second set. The PX BLOCK ITERATOR row source represents the splitting up of the table EMP2 into pieces so as to divide the scan workload between the parallel scan slaves. The PX SEND and PX RECEIVE row sources represent the pipe that connects the two slave sets as rows flow up from the parallel scan, get repartitioned through the HASH table queue, and then read by and aggregated on the top slave set. The PX SEND QC row source represents the aggregated values being sent to the QC in random (RAND) order. The PX COORDINATOR row source represents the QC or Query Coordinator which controls and schedules the parallel plan appearing below it in the plan tree.

 

                上面这段文字是从Oracle 联机文档上荡下来的。

[http://download.oracle.com/docs/cd/E11882_01/server.112/e10821/ex_plan.htm#PFGRF94687](http://download.oracle.com/docs/cd/E11882_01/server.112/e10821/ex_plan.htm#PFGRF94687)

 

通过执行计划，我们来看一下它的执行步骤：

                （1）并行服务进程对EMP2表进行全表扫描。

                （2）并行服务进程以ITERATOR（迭代）方式访问数据块，也就是并行协调进程分给每个并行服务进程一个数据片，在这个数据片上，并行服务进程顺序地访问每个数据块（Iterator），所有的并行服务进程将扫描的数据块传给另一组并行服务进程（父进程）用于做Hash Group操作。

                （3）并行服务父进程对子进程传递过来的数据做Hash Group操作。

                （4）并行服务进程（子进程）将处理完的数据发送出去。

                （5）并行服务进程（父进程）接收到处理过的数据。

                （6）合并处理过的数据，按照随即的顺序发给并行协调进程（QC：Query Conordinator）。

                （7）并行协调进程将处理结果发给用户。

 

当使用了并行执行，SQL的执行计划中就会多出一列：in-out。 该列帮助我们理解数据流的执行方法。 它的一些值的含义如下：

Parallel to Serial（P->S）: 表示一个并行操作发送数据给一个串行操作，通常是并行incheng将数据发送给并行调度进程。

Parallel to Parallel（P->P）：表示一个并行操作向另一个并行操作发送数据，疆场是两个从属进程之间的数据交流。

Parallel Combined with parent(PCWP): 同一个从属进程执行的并行操作，同时父操作也是并行的。

Parallel Combined with Child(PCWC): 同一个从属进程执行的并行操作，子操作也是并行的。

Serial to Parallel（S->P）: 一个串行操作发送数据给并行操作，如果select 部分是串行操作，就会出现这个情况。

 

 

四．并行执行等待事件

                在做并行执行方面的性能优化的时候，可能会遇到如下等待时间：

                                PX Deq Credit: send blkd

                这是一个有并行环境的数据库中，从statspack 或者AWR中经常可以看到的等待事件。 在Oracle 9i 里面， 这个等待时间被列入空闲等待。 关于等待时间参考：

                Oracle 常见的33个等待事件

                [http://blog.csdn.net/tianlesoftware/archive/2010/08/12/5807800.aspx](http://blog.csdn.net/tianlesoftware/archive/2010/08/12/5807800.aspx)

 

一般来说空闲等待可以忽略它，但是实际上空闲等待也是需要关注的，因为一个空闲的等待，它反映的是另外的资源已经超负荷运行了。 基于这个原因，在Oracle 10g里已经把PX Deq Credit: send blkd等待时间不在视为空闲等待，而是列入了Others 等待事件范围。

 

PX Deq Credit: send blkd 等待事件的意思是： 当并行服务进程向并行协调进程QC（也可能是上一层的并行服务进程）发送消息时，同一时间只有一个并行服务进程可以向上层进程发送消息，这时候如果有其他的并行服务进程也要发送消息，就只能等待了。 知道获得一个发送消息的信用信息（Credit），这时候会触发这个等待事件，这个等待事件的超时时间为2秒钟。

 

                如果我们启动了太多的并行进程，实际上系统资源（CPU）或者QC 无法即时处理并行服务发送的数据，那么等待将不可避免。 对于这种情况，我们就需要降低并行处理的并行度。

 

                当出现PX Deq Credit：send blkd等待的时间很长时，我们可以通过平均等待时间来判断等待事件是不是下层的并行服务进程空闲造成的。该等待事件的超时时间是2秒，如果平均等待时间也差不多是2秒，就说明是下层的并行进程“无事所做”，处于空闲状态。 如果和2秒的差距很大，就说明不是下层并行服务超时导致的空闲等待，而是并行服务之间的竞争导致的，因为这个平均等待事件非常短，说明并行服务进程在很短时间的等待之后就可以获取资源来处理数据。

所以对于非下层的并行进程造成的等待，解决的方法就是降低每个并行执行的并行度，比如对象（表，索引）上预设的并行度或者查询Hint 指定的并行度。

 

 

五． 并行执行的使用范围

Oracle的并行技术在下面的场景中可以使用：

（1）       Parallel Query（并行查询）

（2）       Parallel DDL（并行DDL操作，如建表，建索引等）

（3）       Parallel DML（并行DML操作，如insert，update，delete等）

 

5.1 并行查询

                并行查询可以在查询语句，子查询语句中使用，但是不可以使用在一个远程引用的对象上（如DBLINK）。

 

                一个查询能够并行执行，需要满足一下条件：

（1）       SQL语句中有Hint提示，比如Parallel 或者 Parallel_index.

（2）       SQL语句中引用的对象被设置了并行属性。

（3）       多表关联中，至少有一个表执行全表扫描（Full table scan）或者跨分区的Index range SCAN。

 

如： select  /*+parallel(t 4) * from t;

 

5.2 并行DDL 操作

 

5.2.1 表操作的并行执行

                以下表操作可以使用并行执行：

CREATE TABLE … AS SELECT

       ALTER TABLE … move partition

       Alter table … split partition

       Alter table … coalesce partition

 

DDL操作，我们可以通过trace 文件来查看它的执行过程。

 

示例：

 

查看当前的trace 文件：

*/* Formatted on 2010/8/31 23:33:00 (QP5 v5.115.810.9015) */*

SELECT      u_dump.VALUE

         || '/'

         || db_name.VALUE

         || '_ora_'

         || v$process.spid

         || NVL2 (v$process.traceid, '_' || v$process.traceid, NULL)

         || '.trc'

            "Trace File"

  FROM            v$parameter u_dump

               CROSS JOIN

                  v$parameter db_name

            CROSS JOIN

               v$process

         JOIN

            v$session

         ON v$process.addr = v$session.paddr

 WHERE       u_dump.name = 'user_dump_dest'

         AND db_name.name = 'db_name'

         AND v$session.audsid = SYS_CONTEXT ('userenv', 'sessionid');

 

Trace File

------------------------------------------------------------------------------

d:/app/administrator/diag/rdbms/orcl/orcl/trace/orcl_ora_5836.trc

d:/app/administrator/diag/rdbms/orcl/orcl/trace/orcl_ora_3048.trc

 

SQL> alter session set events '10046 trace name context forever,level 12';

会话已更改。

SQL> create table 怀宁 parallel 4 as select * from dba_objects;

表已创建。

SQL> alter session set events '10046 trace name context off' ;

会话已更改。

 

这里用到了ORACLE的event 时间。 10046事件是用来跟踪SQL语句的。开启事件后，相关的信息会写道trace 文件中，这也是之前我们查看trace 文件名的原因。 关于event事件，参考我的blog：

                Oracle 跟踪事件 set event

                [http://blog.csdn.net/tianlesoftware/archive/2009/12/13/4977827.aspx](http://blog.csdn.net/tianlesoftware/archive/2009/12/13/4977827.aspx)

 

有了trace文件， 我们可以用tkprof 工具，来查看trace 文件的内容。 关于tkprof 工具介绍，参考blog：

                使用 Tkprof 分析 ORACLE 跟踪文件

                [http://blog.csdn.net/tianlesoftware/archive/2010/05/29/5632003.aspx](http://blog.csdn.net/tianlesoftware/archive/2010/05/29/5632003.aspx)

 

 

进入trace 目录，用tkprof命令生成txt 文件，然后查看txt 文件。

d:/app/Administrator/diag/rdbms/orcl/orcl/trace>tkprof orcl_ora_3048.trc 安庆.txt sys=no

TKPROF: Release 11.2.0.1.0 - Development on 星期二 8月 31 23:45:25 2010

Copyright (c) 1982, 2009, Oracle and/or its affiliates.  All rights reserved.

d:/app/Administrator/diag/rdbms/orcl/orcl/trace>

 

 

5.2.2 创建索引的并行执行

                创建索引时使用并行方式在系统资源充足的时候会使性能得到很大的提高，特别是在OLAP系统上对一些很大的表创建索引时更是如此。 以下的创建和更改索引的操作都可以使用并行：

                Create index

                Alter index … rebuild

                Alter index … rebuild partition

                Alter index … split partition

 

一个简单的语法：create index t_ind on t(id) parallel 4;

 

监控这个过程和5.2.1 中表一样，需要通过10046事件。 这里就不多说了。

 

有关减少创建时间方法，参考blog：

                如何加快建 index 索引 的时间

                [http://blog.csdn.net/tianlesoftware/archive/2010/07/11/5664019.aspx](http://blog.csdn.net/tianlesoftware/archive/2010/07/11/5664019.aspx)

** **

 

总结：

使用并行方式，不论是创建表，修改表，创建索引，重建索引，他们的机制都是一样的，那就是Oracle 给每个并行服务进程分配一块空间，每个进程在自己的空间里处理数据，最后将处理完毕的数据汇总，完成SQL的操作。

 

 

5.3 并行DML 操作

                Oracle 可以对DML操作使用并行执行，但是有很多限制。 如果我们要让DML 操作使用并行执行，必须显示地在会话里执行如下命令：

                SQL> alter session enable parallel dml;

会话已更改。

 

                只有执行了这个操作，Oracle 才会对之后符合并行条件的DML操作并行执行，如果没有这个设定，即使SQL中指定了并行执行，Oracle也会忽略它。

 

5.3.1 delete，update和merge 操作

                Oracle 对Delete,update,merge的操作限制在，只有操作的对象是分区表示，Oracle 才会启动并行操作。原因在于，对于分区表，Oracle 会对每个分区启用一个并行服务进程同时进行数据处理，这对于非分区表来说是没有意义的。

 

5.3.2 Insert 的并行操作

                实际上只有对于insert into … select … 这样的SQL语句启用并行才有意义。 对于insert into .. values… 并行没有意义，因为这条语句本身就是一个单条记录的操作。

 

                Insert 并行常用的语法是：

                                Insert /*+parallel(t 2) */ into t select /*+parallel(t1 2) */ * from t1;

 

                这条SQL 语句中，可以让两个操作insert 和select 分别使用并行，这两个并行是相互独立，互补干涉的，也可以单独使用其中的一个并行。

 

 

六． 并行执行的设定

 

6.1 并行相关的初始话参数

 

6.1.1 parallel_min_servers=n

                在初始化参数中设置了这个值，Oracle 在启动的时候就会预先启动N个并行服务进程，当SQL执行并行操作时，并行协调进程首先根据并行度的值，在当前已经启动的并行服务中条用n个并行服务进程，当并行度大于n时，Oracle将启动额外的并行服务进程以满足并行度要求的并行服务进程数量。

 

6.1.2 parallel_max_servers=n

                如果并行度的值大于parallel_min_servers或者当前可用的并行服务进程不能满足SQL的并行执行要求，Oracle将额外创建新的并行服务进程，当前实例总共启动的并行服务进程不能超过这个参数的设定值。

 

6.1.3 parallel_adaptive_multi_user=true|false

                Oracle 10g R2下，并行执行默认是启用的。 这个参数的默认值为true，它让Oracle根据SQL执行时系统的负载情况，动态地调整SQL的并行度，以取得最好的SQL    执行性能。

 

6.1.4 parallel_min_percent

                这个参数指定并行执行时，申请并行服务进程的最小值，它是一个百分比，比如我们设定这个值为50. 当一个SQL需要申请20个并行进程时，如果当前并行服务进程不足，按照这个参数的要求，这个SQL比如申请到20*50%=10个并行服务进程，如果不能够申请到这个数量的并行服务，SQL 将报出一个ORA-12827的错误。

                当这个值设为Null时，表示所有的SQL在做并行执行时，至少要获得两个并行服务进程。

 

 

6.2 并行度的设定

                并行度可以通过以下三种方式来设定：

（1）使用Hint 指定并行度。

（2）使用alter session force parallel 设定并行度。

（3）使用SQL中引用的表或者索引上设定的并行度，原则上Oracle 使用这些对象中并行度最高的那个值作为当前执行的并行度。

 

 

示例：

                SQL>Select /*+parallel(t 4) */ count(*) from t;

                SQL>Alter table t parallel 4;

                SQL>Alter session force parallel query parallel 4;

 

Oracle 默认并行度计算方式：

（1）Oracle 根据CPU的个数，RAC实例的个数以及参数parallel_threads_per_cpu的值，计算出一个并行度。

（2）对于并行访问分区操作，取需要访问的分区数为并行度。

 

并行度的优先级别从高到低：

                Hint->alter session force parallel->表，索引上的设定-> 系统参数

 

 

实际上，并行只有才系统资源比较充足的情况下，才会取得很好的性能，如果系统负担很重，不恰当的设置并行，反而会使性能大幅下降。

 

 

七． 直接加载

                在执行数据插入或者数据加载的时候，可以通过append hint的方式进行数据的直接加载。

 

在insert 的SQL中使用APPEND，如：

                                Insert /*+append */ into t select * from t1;

               

还可以在SQL*LOADER里面使用直接加载：

       Sqlldr userid=user/pwd control=load.ctl direct=true

 

Oracle 执行直接加载时，数据直接追加到数据段的最后，不需要花费时间在段中需找空间，数据不经过data buffer直接写到数据文件中，效率要比传统的加载方式高。

 

示例：

SQL> create table t as select * from user_tables;

表已创建。

SQL> select segment_name,extent_id,bytes from user_extents where segment_name='T';

SEGMENT_NA  EXTENT_ID      BYTES

---------- ---------- ----------

T                   0      65536

T                   1      65536

T                   2      65536

T                   3      65536

T                   4      65536

 

这里我们创建了一张表，分配了5个extents。

 

SQL> delete from t;

已删除979行。

SQL> select segment_name,extent_id,bytes from user_extents where segment_name='T';

SEGMENT_NA  EXTENT_ID      BYTES

---------- ---------- ----------

T                   0      65536

T                   1      65536

T                   2      65536

T                   3      65536

T                   4      65536

 

这里删除了表里的数据，但是查询，依然占据5个extents。因为delete不会收缩表空间，不能降低高水位。

 

SQL> insert into t select * from user_tables;

已创建980行。

SQL> commit;

提交完成。

 

SQL> select segment_name,extent_id,bytes from user_extents where segment_name='T';

SEGMENT_NA  EXTENT_ID      BYTES

---------- ---------- ----------

T                   0      65536

T                   1      65536

T                   2      65536

T                   3      65536

T                   4      65536

 

用传统方式插入，数据被分配到已有的空闲空间里。

 

 

SQL> delete from t;

已删除980行。

SQL> commit;

提交完成。

SQL> select segment_name,extent_id,bytes from user_extents where segment_name='T';

SEGMENT_NA  EXTENT_ID      BYTES

---------- ---------- ----------

T                   0      65536

T                   1      65536

T                   2      65536

T                   3      65536

T                   4      65536

 

删除数据，用append直接插入看一下。

 

SQL> insert /*+append */ into t select * from user_tables;

已创建980行。

SQL> commit;

提交完成。

SQL> select segment_name,extent_id,bytes from user_extents where segment_name='T';

SEGMENT_NA  EXTENT_ID      BYTES

---------- ---------- ----------

T                   0      65536

T                   1      65536

T                   2      65536

T                   3      65536

T                   4      65536

T                   5      65536

T                   6      65536

T                   7      65536

T                   8      65536

T                   9      65536

已选择10行。

 

从结果可以看出，直接加载方式时，虽然表中有很多空的数据块，Oracle 仍然会额外的分配4个extent用于直接加载数据。

                直接加载的数据放在表的高水位（High water Mark：hwm）以上，当直接加载完成后，Oracle 将表的高水位线移到新加入的数据之后，这样新的数据就可以被用户使用了。

 

Oracle 高水位(HWM)

[http://blog.csdn.net/tianlesoftware/archive/2009/10/22/4707900.aspx](http://blog.csdn.net/tianlesoftware/archive/2009/10/22/4707900.aspx)

 

 

7.1 直接加载和REDO

                直接加载在logging模式下，与传统加载方式产生的redo 日志差别不大，因为当一个表有logging属性时，即使使用直接加载，所有改变的数据依然要产生redo，实际上是所有修改的数据块全部记录redo，以便于以后的恢复，这时候直接加载并没有太大的优势。

 

                直接加载最常见的是和nologging一起使用，这时候可以有效地减少redo 的生成量。 注意的是，在这种情况下，直接加载的数据块是不产生redo的，只有一些其他改变的数据产生一些redo，比如表空间分配需要修改字典表或者修改段头数据块，这些修改会产生少量的redo。

 

                实际上，对于nologging 方式的直接加载，undo 的数据量也产生的很少，因为直接加载的数据并不会在回滚段中记录，这些记录位于高水位之上，在事务提交之前，对于其他用户来说是不可见的，所以不需要产生undo，事务提交时，Oracle 将表的高水位线移到新的数据之后，如果事务回滚，只需要保持高水位线不动即可，就好像什么都没有发生一样。

 

                注意，由于在nologging模式下，redo 不记录数据修改的信息，所以直接加载完后，需要立即进行相关的备份操作，因为这些数据没有记录在归档日志中，一旦数据损坏，只能用备份来恢复，而不能使用归档恢复。

 

Logging模式下示例：

SQL> set autot trace stat;

SQL> insert /*+append */ into t select * from user_tables;

已创建980行。

统计信息

----------------------------------------------------------

        132  recursive calls

         87  db block gets

       8967  consistent gets

          0  physical reads

     286572  redo size

        911  bytes sent via SQL*Net to client

       1017  bytes received via SQL*Net from client

          4  SQL*Net roundtrips to/from client

          2  sorts (memory)

          0  sorts (disk)

        980  rows processed

SQL> rollback;

回退已完成。

SQL> insert into t select * from user_tables;

已创建980行。

统计信息

----------------------------------------------------------

          0  recursive calls

        144  db block gets

       9027  consistent gets

          0  physical reads

     267448  redo size

        927  bytes sent via SQL*Net to client

       1004  bytes received via SQL*Net from client

          4  SQL*Net roundtrips to/from client

          2  sorts (memory)

          0  sorts (disk)

        980  rows processed

 

Nologging模式下示例：

SQL> alter table t nologging;

表已更改。

SQL> insert into t select * from user_tables;

已创建980行。

统计信息

----------------------------------------------------------

        239  recursive calls

        132  db block gets

       9061  consistent gets

          0  physical reads

     262896  redo size

        927  bytes sent via SQL*Net to client

       1004  bytes received via SQL*Net from client

          4  SQL*Net roundtrips to/from client

          7  sorts (memory)

          0  sorts (disk)

        980  rows processed

SQL> rollback;

回退已完成。

SQL> insert /*+append */ into t select * from user_tables;

已创建980行。

统计信息

----------------------------------------------------------

          8  recursive calls

         40  db block gets

       8938  consistent gets

          0  physical reads

        340  redo size  -- redo 减少很多

        911  bytes sent via SQL*Net to client

       1017  bytes received via SQL*Net from client

          4  SQL*Net roundtrips to/from client

          2  sorts (memory)

          0  sorts (disk)

        980  rows processed

 

这部分内容也可参考Blog：

                Oracle DML NOLOGGING

[http://blog.csdn.net/tianlesoftware/archive/2010/07/11/5701596.aspx](http://blog.csdn.net/tianlesoftware/archive/2010/07/11/5701596.aspx)

 

 

7.2 直接加载和索引

                如果直接加载的表上有索引，Oracle不会像加载数据的方式那样来处理索引的数据，但是它同样需要维护一个索引，这个成本很高，同时会生成很多的redo。

                所以当使用直接加载时，通常是针对一些数据量非常大的表。如果这些表存在索引，将会带来很大的性能影响，这时可以考虑先将索引disable或者drop掉，等加载数据后，之后在重新建立索引。

 

nologging示例：

 

SQL> insert /*+append */ into t select * from user_tables;

已创建980行。

 

统计信息

----------------------------------------------------------

          0  recursive calls

         40  db block gets

       8936  consistent gets

          0  physical reads

        384  redo size

        911  bytes sent via SQL*Net to client

       1017  bytes received via SQL*Net from client

          4  SQL*Net roundtrips to/from client

          2  sorts (memory)

          0  sorts (disk)

        980  rows processed

 

SQL> rollback;

回退已完成。

SQL> create index t_ind on t(table_name);

索引已创建。

SQL> insert /*+append */ into t select * from user_tables;

已创建980行。

 

统计信息

----------------------------------------------------------

         40  recursive calls

        170  db block gets

       8955  consistent gets

          4  physical reads

     149424  redo size

        911  bytes sent via SQL*Net to client

       1017  bytes received via SQL*Net from client

          4  SQL*Net roundtrips to/from client

          3  sorts (memory)

          0  sorts (disk)

        980  rows processed

SQL> rollback;

回退已完成。

SQL> insert  into t select * from user_tables;

已创建980行。

 

统计信息

----------------------------------------------------------

          8  recursive calls

        828  db block gets

       9037  consistent gets

          0  physical reads

     382832  redo size

        927  bytes sent via SQL*Net to client

       1005  bytes received via SQL*Net from client

          4  SQL*Net roundtrips to/from client

          2  sorts (memory)

          0  sorts (disk)

        980  rows processed

SQL> rollback;

回退已完成。

 

 

7.3 直接加载和并行

                直接加载可以和并行执行一同使用，这样可以并行地向表中插入数据。 如：

               

SQL>alter session enable parallel dml;  -- 这里必须显示的申明

SQL>insert /*+append parallel(t,2) */ into t select * from t1;

SQL>insert /*+append */ into t select * from t1;

 

注：在对insert 使用并行时，Oracle自动使用直接加载的方式进行数据加载，所以在这种情况下append是可以省略的。

 

                当使用并行加载时，Oracle 会按照并行度启动相应数量的并行服务进程，像串行执行的直接加载的方式一样，每个并行服务进程都单独分配额外的空间用于加载数据，实际上Oracle 为每个并行服务进程分配了一个临时段，每个并行服务进程将数据首先加载到各自的临时段上，当所有的并行进程执行完毕后，将各自的数据块合并到一起，放到高水位之后，如果事务提交，则将高水位移到新加载的数据之后。

 

 

7.4 直接加载和SQL*LOADER

                在SQL*LOADER中也可以使用直接加载，它比传统方式效率更高，因为它绕开了SQL的解析和数据缓冲区，直接将数据加载到数据文件，这对OLAP或者数据仓库系统非常有用。

 

指定加载：

                Sqlldr userid=user/pwd control=control.ctl direct=true

 

指定并行和加载：

                Sqlldr userid=user/pwd control=control.ctl direct=true parallel=true

 

 

SQL*LOADER直接加载对索引的影响：

（1）索引为非约束性，直接加载可以在加载完毕后维护索引的完整性。

（2）索引为约束性索引，比如主键，直接加载仍然会将数据加载入库，但是会将索引置为unusable.

 

 

如果使用SQL*LOADER的并行直接加载选项，并且表上有索引，将导致加载失败，这是我们可以在sqlloader中指定skip_index_maintenance=true, 来允许加载完成，但是索引状态会变成unusable，需要手工rebuild.

 

关于SQL*LOADER的更多内容，参考blog：

                Oracle SQL Loader

                [http://blog.csdn.net/tianlesoftware/archive/2009/10/16/4674063.aspx](http://blog.csdn.net/tianlesoftware/archive/2009/10/16/4674063.aspx)

** **

 

 

 

整理自《让Oracle 跑的更快》

------------------------------------------------------------------------------

Blog： http://blog.csdn.net/tianlesoftware

网上资源： http://tianlesoftware.download.csdn.net

相关视频：http://blog.csdn.net/tianlesoftware/archive/2009/11/27/4886500.aspx

DBA1 群：62697716(满); DBA2 群：62697977(满)

DBA3 群：63306533;     聊天 群：40132017
分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")

* 上一篇：[Oracle 索引扫描的五种类型](http://blog.csdn.net/tianlesoftware/article/details/5852106)
* 下一篇：[Oracle 绑定变量 详解](http://blog.csdn.net/tianlesoftware/article/details/5856430)
查看评论[]()

7楼 [tiankui6658](http://blog.csdn.net/tiankui6658) 2013-03-31 11:16发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/tiankui6658)简直就是官方教材啊6楼 [syzcch](http://blog.csdn.net/syzcch) 2012-10-16 13:05发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/syzcch)总结的很到位啊5楼 [syzcch](http://blog.csdn.net/syzcch) 2012-09-13 14:18发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/syzcch)| 0 | SELECT STATEMENT | | 107 | 2782 | 3 (34) | | |
| 1 | PX COORDINATOR | | | | | | |
| 2 | PX SEND QC (RANDOM) | :TQ10001 | 107 | 2782 | 3 (34) | Q1,01 | P->S | QC (RAND) |
| 3 | HASH GROUP BY | | 107 | 2782 | 3 (34) | Q1,01 | PCWP |
| 4 | PX RECEIVE | | 107 | 2782 | 3 (34) | Q1,01 | PCWP | |
| 5 | PX SEND HASH | :TQ10000 | 107 | 2782 | 3 (34) | Q1,00 | P->P | HASH
| 6 | HASH GROUP BY | | 107 | 2782 | 3 (34) | Q1,00 | PCWP |
| 7 | PX BLOCK ITERATOR | | 107 | 2782 | 2 (0) | Q1,00 | PCWP |
| 8 | TABLE ACCESS FULL| EMP2 | 107 | 2782 | 2 (0) | Q1,00 | PCWP |
在这个例子中，我觉得是有两组并行服务进程，分别用Q1，00和Q1,01来标注的吧 比如paralle（degree 4） 说明Q1,00是4个进程在跑，而Q1,01也是4个，他们的关系类似于生产者和消费者，请问这样理解对吗？ 谢谢！4楼 [我个名叫麦兜_sco](http://blog.csdn.net/scogeek) 2012-02-14 15:44发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/scogeek)有空希望能把你的文章看完...3楼 [aimingl](http://blog.csdn.net/aimingl) 2011-09-21 09:55发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/aimingl)牛人，顶一下；再学习。。。2楼 [gaopengtttt](http://blog.csdn.net/gaopengtttt) 2011-07-20 11:56发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/gaopengtttt)bu cuo o1楼 [fendou1314](http://blog.csdn.net/fendou1314) 2011-06-22 23:58发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/fendou1314)[e01][e01][e01][e03]
太棒了@！
您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Ftianlesoftware%2Farticle%2Fdetails%2F5854583)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()
[![TOP]()]( "回到顶部")

个人资料

[![]( "访问我的空间")](http://my.csdn.net/tianlesoftware)
[Dave](http://my.csdn.net/tianlesoftware)

[]( "[加关注]") []( "[发私信]")
[![]()](http://medal.blog.csdn.net/allmedal.aspx)[![]()](http://medal.blog.csdn.net/allmedal.aspx)[![]()](http://medal.blog.csdn.net/allmedal.aspx)

* 访问：4265571次
* 积分：49439分
* 排名：第13名

* 原创：1009篇
* 转载：96篇
* 译文：1篇
* 评论：1481条

文章搜索

[]()

Dave's Links

* QQ:251097186
* Email:tianlesoftware@gmail.com
* Skype:tianlesoftware
* [My LinkedIn](http://cn.linkedin.com/in/tianlesoftware)
* [My FaceBook](http://www.facebook.com/tianlesoftware)
* [My Twitter](http://twitter.com/tianlesoftware)
* [My Sina Weibo](http://weibo.com/tianlesoftware)
* [My Tencent Weibo](http://t.qq.com/tianlesoftware)
* [LineZing](http://tongji.linezing.com/mystat.html)
文章分类

* [Dave 随笔](http://blog.csdn.net/tianlesoftware/article/category/684508)(39)
* [Oracle Troubleshooting](http://blog.csdn.net/tianlesoftware/article/category/679087)(186)
* [My Blog Summary](http://blog.csdn.net/tianlesoftware/article/category/1092236)(14)
* [Tools & Scripts](http://blog.csdn.net/tianlesoftware/article/category/1154895)(11)
* [IBM AIX](http://blog.csdn.net/tianlesoftware/article/category/746816)(35)
* [Linux](http://blog.csdn.net/tianlesoftware/article/category/569752)(96)
* [Storage](http://blog.csdn.net/tianlesoftware/article/category/668414)(19)
* [Network](http://blog.csdn.net/tianlesoftware/article/category/657796)(9)
* [MySQL](http://blog.csdn.net/tianlesoftware/article/category/811179)(14)
* [Oracle Exadata](http://blog.csdn.net/tianlesoftware/article/category/915216)(0)
* [Oracle 12c](http://blog.csdn.net/tianlesoftware/article/category/1076473)(1)
* [Oracle ASM](http://blog.csdn.net/tianlesoftware/article/category/799909)(85)
* [Oracle Data Guard](http://blog.csdn.net/tianlesoftware/article/category/700326)(38)
* [Oracle Golden Gate](http://blog.csdn.net/tianlesoftware/article/category/776328)(21)
* [Oracle RAC](http://blog.csdn.net/tianlesoftware/article/category/700327)(110)
* [Oracle RMAN](http://blog.csdn.net/tianlesoftware/article/category/697504)(45)
* [Oracle Stream](http://blog.csdn.net/tianlesoftware/article/category/729879)(2)
* [Oracle Timesten](http://blog.csdn.net/tianlesoftware/article/category/804599)(0)
* [Oracle Basic Knowledge](http://blog.csdn.net/tianlesoftware/article/category/569751)(222)
* [Oracle Advanced Knowledge](http://blog.csdn.net/tianlesoftware/article/category/706520)(190)
* [Oracle Backup & Recovery](http://blog.csdn.net/tianlesoftware/article/category/698471)(14)
* [Oracle Developer](http://blog.csdn.net/tianlesoftware/article/category/719638)(22)
* [Oracle Certification](http://blog.csdn.net/tianlesoftware/article/category/714767)(9)
* [Oracle Performance](http://blog.csdn.net/tianlesoftware/article/category/668807)(87)
* [System Architecture](http://blog.csdn.net/tianlesoftware/article/category/1169693)(1)

文章存档

* [2013年08月](http://blog.csdn.net/tianlesoftware/article/month/2013/08)(3)
* [2013年07月](http://blog.csdn.net/tianlesoftware/article/month/2013/07)(8)
* [2013年06月](http://blog.csdn.net/tianlesoftware/article/month/2013/06)(5)
* [2013年05月](http://blog.csdn.net/tianlesoftware/article/month/2013/05)(1)
* [2013年04月](http://blog.csdn.net/tianlesoftware/article/month/2013/04)(3)
* [2013年03月](http://blog.csdn.net/tianlesoftware/article/month/2013/03)(7)
* [2013年02月](http://blog.csdn.net/tianlesoftware/article/month/2013/02)(5)
* [2013年01月](http://blog.csdn.net/tianlesoftware/article/month/2013/01)(4)
* [2012年12月](http://blog.csdn.net/tianlesoftware/article/month/2012/12)(15)
* [2012年11月](http://blog.csdn.net/tianlesoftware/article/month/2012/11)(23)
* [2012年10月](http://blog.csdn.net/tianlesoftware/article/month/2012/10)(7)
* [2012年09月](http://blog.csdn.net/tianlesoftware/article/month/2012/09)(5)
* [2012年08月](http://blog.csdn.net/tianlesoftware/article/month/2012/08)(3)
* [2012年07月](http://blog.csdn.net/tianlesoftware/article/month/2012/07)(21)
* [2012年06月](http://blog.csdn.net/tianlesoftware/article/month/2012/06)(16)
* [2012年05月](http://blog.csdn.net/tianlesoftware/article/month/2012/05)(9)
* [2012年04月](http://blog.csdn.net/tianlesoftware/article/month/2012/04)(5)
* [2012年03月](http://blog.csdn.net/tianlesoftware/article/month/2012/03)(26)
* [2012年02月](http://blog.csdn.net/tianlesoftware/article/month/2012/02)(34)
* [2012年01月](http://blog.csdn.net/tianlesoftware/article/month/2012/01)(7)
* [2011年12月](http://blog.csdn.net/tianlesoftware/article/month/2011/12)(29)
* [2011年11月](http://blog.csdn.net/tianlesoftware/article/month/2011/11)(37)
* [2011年10月](http://blog.csdn.net/tianlesoftware/article/month/2011/10)(34)
* [2011年09月](http://blog.csdn.net/tianlesoftware/article/month/2011/09)(26)
* [2011年08月](http://blog.csdn.net/tianlesoftware/article/month/2011/08)(24)
* [2011年07月](http://blog.csdn.net/tianlesoftware/article/month/2011/07)(34)
* [2011年06月](http://blog.csdn.net/tianlesoftware/article/month/2011/06)(45)
* [2011年05月](http://blog.csdn.net/tianlesoftware/article/month/2011/05)(20)
* [2011年04月](http://blog.csdn.net/tianlesoftware/article/month/2011/04)(96)
* [2011年03月](http://blog.csdn.net/tianlesoftware/article/month/2011/03)(52)
* [2011年02月](http://blog.csdn.net/tianlesoftware/article/month/2011/02)(30)
* [2011年01月](http://blog.csdn.net/tianlesoftware/article/month/2011/01)(53)
* [2010年12月](http://blog.csdn.net/tianlesoftware/article/month/2010/12)(56)
* [2010年11月](http://blog.csdn.net/tianlesoftware/article/month/2010/11)(42)
* [2010年10月](http://blog.csdn.net/tianlesoftware/article/month/2010/10)(23)
* [2010年09月](http://blog.csdn.net/tianlesoftware/article/month/2010/09)(29)
* [2010年08月](http://blog.csdn.net/tianlesoftware/article/month/2010/08)(26)
* [2010年07月](http://blog.csdn.net/tianlesoftware/article/month/2010/07)(26)
* [2010年06月](http://blog.csdn.net/tianlesoftware/article/month/2010/06)(22)
* [2010年05月](http://blog.csdn.net/tianlesoftware/article/month/2010/05)(18)
* [2010年04月](http://blog.csdn.net/tianlesoftware/article/month/2010/04)(24)
* [2010年03月](http://blog.csdn.net/tianlesoftware/article/month/2010/03)(25)
* [2010年02月](http://blog.csdn.net/tianlesoftware/article/month/2010/02)(11)
* [2010年01月](http://blog.csdn.net/tianlesoftware/article/month/2010/01)(14)
* [2009年12月](http://blog.csdn.net/tianlesoftware/article/month/2009/12)(25)
* [2009年11月](http://blog.csdn.net/tianlesoftware/article/month/2009/11)(30)
* [2009年10月](http://blog.csdn.net/tianlesoftware/article/month/2009/10)(47)

展开
国外技术链接

* [ixora](http://www.ixora.com.au/notes/)
* [dioncho](http://dioncho.wordpress.com/)
* [OraDBA](http://www.oradba.ch/)
* [Gavin Soorma](http://gavinsoorma.com/)
* [oracle-base](http://www.oracle-base.com/)
* [oracle-developer](http://www.oracle-developer.net/)
* [databasejournal](http://www.databasejournal.com/)
* [Julian Dyke](http://www.juliandyke.com/)
* [Jonathan Lewis(Oracle Core)](http://jonathanlewis.wordpress.com/)
* [oracle-scripts](http://www.oracle-scripts.net/)
* [oaktable](http://www.oaktable.net/)

官网链接

* [E-Delivery](https://edelivery.oracle.com/)
* [Oracle Public-Yum](http://public-yum.oracle.com/)
* [Oracle Documents](http://tahiti.oracle.com/)
* [MySQL Documents](http://dev.mysql.com/doc/)
* [GG Documents](http://download.oracle.com/docs/cd/E22355_01/index.htm)
* [ORACLE CPU（Critical Patch Update）](http://www.oracle.com/technology/deploy/security/alerts.htm)
* [Patchset Number Overview](http://wiki.oracle.com/page/Patchset Number Overview)
* [Oracle 10g/11g 下载](http://www.oracle.com/technology/software/products/database/index.html)
* [Unix-Center](http://www.unix-center.net/)
* [IBM Technical library](http://www.ibm.com/developerworks/library/)
* [IBM 技术知识库](http://www-900.ibm.com/cn/support/viewdoc/knowledgebase)
* [IBM AIX 认证专题](http://www.ibm.com/developerworks/cn/aix/aixcert/)
* [IBM DeveloperWorks](http://www.ibm.com/developerworks/cn/)
* [idevelopment](http://www.idevelopment.info/)
* [OCM 查询](http://education.oracle.com/education/otn/)
国内技术链接

* [warehouse](http://warehouse.itpub.net/)
* [dbsnake](http://dbsnake.com/)
* [eygle](http://www.eygle.com/)
* [冯大辉(Fenng)](http://www.dbanotes.net/)
* [HelloDBA](http://www.hellodb.net/)
* [Kamus](http://www.dbform.com/)
* [骨骨](http://hi.baidu.com/edeed/blog)
* [paul](http://space.itpub.net/7199859/spacelist-blog)
* [Roger](http://www.killdb.com/)
* [Maclean](http://www.oracledatabase12g.com/)
* [老 熊](http://www.laoxiong.net/)
* [Tomas Zhang](http://tomszrp.itpub.net/)
* [ixora](http://www.ixora.com.au/notes/)
* [tianlesoftware](http://tianlesoftware.wordpress.com/)
* [d.c.b.a](http://www.anysql.net/)
* [xifenfei](http://www.xifenfei.com/)
* [Ludatou](http://www.ludatou.com/)
* [robinson1988](http://blog.csdn.net/robinson1988)
* [简朝阳](http://isky000.com/)
* [mysqlops](http://www.mysqlops.com/)
* [数据工人（boypoo）](http://www.zhihong.org/)
* [Jerry Wu](http://www.database-consultant.net/)
* [文本时代](http://www.mythdata.com/)
* [d-zero](http://www.d-zero.com.cn/)
* [道道](http://blog.csdn.net/huangchao_sky)
* [邓伟](http://blog.csdn.net/laven54)

最新评论

* [Oracle Data Guard Linux 平台 Physical Standby 搭建实例](http://blog.csdn.net/tianlesoftware/article/details/5547565#comments)

[SP1022](http://blog.csdn.net/wdnq1022): 大哥，在主库执行startup pfile='/home/oracle/product/10.2.0...
* [2013年7月22日 匆匆](http://blog.csdn.net/tianlesoftware/article/details/9468081#comments)

[暖爱_晨](http://blog.csdn.net/chen0032): @chen0032:如果细想这个世界什么东西跟我们有关，那就只有感激每天的生活
* [2013年7月22日 匆匆](http://blog.csdn.net/tianlesoftware/article/details/9468081#comments)

[暖爱_晨](http://blog.csdn.net/chen0032): 被突然的非技术类文章吸引，便决定关注lz!一篇在旅途的文章，一段追梦的旅程，一份难以割舍的亲情，一个...
* [Oracle Rman 命令详解(List report backup configure)](http://blog.csdn.net/tianlesoftware/article/details/4976998#comments)

[bule2011orange](http://blog.csdn.net/bule2011orange): 很详细，谢谢dave
* [2013年7月22日 匆匆](http://blog.csdn.net/tianlesoftware/article/details/9468081#comments)

[houxp666](http://blog.csdn.net/houxp666): 文笔真不错，淡而美
* [Oracle Data Guard 支持的异构平台 说明](http://blog.csdn.net/tianlesoftware/article/details/7241488#comments)

[Enward](http://blog.csdn.net/enward): 很好，很强大，学些了
* [Dave Oracle 学习 手册 第一版 下载 说明](http://blog.csdn.net/tianlesoftware/article/details/7335660#comments)

[Theven](http://blog.csdn.net/Theven): @mn900809:哦？我还是下载完，不能打开。你那个要是能打开，给我发一份吧！邮箱：aolin.w...
* [Dave Oracle 学习 手册 第一版 下载 说明](http://blog.csdn.net/tianlesoftware/article/details/7335660#comments)

[mn900809](http://blog.csdn.net/mn900809): 我下载下来了 您现在有没？ 需要给您发送一份么？
* [Linux 下挂载硬盘的 方法](http://blog.csdn.net/tianlesoftware/article/details/5642883#comments)

[已经擦肩而过](http://blog.csdn.net/u010148545): 不错啊，但有的我的自启动怎么是这样的啊？UUID=f5c84165-6890-470f-b731-d...
* [Linux SWAP 交换分区配置说明](http://blog.csdn.net/tianlesoftware/article/details/8741873#comments)

[zhanglu668](http://blog.csdn.net/zhanglu668): @liyoubaidu:swap就是交换的意思，这里都指的是交换分区。
阅读排行

* [Oracle 字符集的查看和修改](http://blog.csdn.net/tianlesoftware/article/details/4915223 "Oracle 字符集的查看和修改")(118984)
* [Linux Crontab 定时任务 命令详解](http://blog.csdn.net/tianlesoftware/article/details/5315039 "Linux Crontab 定时任务 命令详解")(108658)
* [Oracle 索引 详解](http://blog.csdn.net/tianlesoftware/article/details/5347098 "Oracle  索引 详解")(38296)
* [Linux awk 命令 说明](http://blog.csdn.net/tianlesoftware/article/details/6278273 "Linux awk 命令 说明")(33921)
* [RMAN 备份与恢复 实例](http://blog.csdn.net/tianlesoftware/article/details/4699320 "RMAN 备份与恢复 实例")(29596)
* [有关 ORA-00604 错误的总结](http://blog.csdn.net/tianlesoftware/article/details/4787074 "有关 ORA-00604 错误的总结")(28877)
* [Oracle 分区表 总结](http://blog.csdn.net/tianlesoftware/article/details/4717318 "Oracle 分区表  总结")(26157)
* [Oracle AWR（Automatic Workload Repository） 说明](http://blog.csdn.net/tianlesoftware/article/details/4682300 "Oracle AWR（Automatic Workload Repository） 说明")(25712)
* [Redhat 5.4 + ASM + RAW+ Oracle 10g RAC 安装文档](http://blog.csdn.net/tianlesoftware/article/details/5872593 "Redhat 5.4 + ASM + RAW+ Oracle 10g  RAC 安装文档")(24895)
* [Oracle ASM 详解](http://blog.csdn.net/tianlesoftware/article/details/5314541 "Oracle ASM 详解")(22268)

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)[QQ客服](http://wpa.qq.com/msgrd?v=3&uin=2355263776&site=qq&menu=yes) [微博客服](http://e.weibo.com/csdnsupport/profile) [论坛反馈](http://bbs.csdn.net/forums/Service) [联系邮箱：webmaster@csdn.net](mailto:webmaster@csdn.net) 服务热线：400-600-2320京 ICP 证 070598 号北京创新乐知信息技术有限公司 版权所有世纪乐知(北京)网络技术有限公司 提供技术支持江苏乐知网络技术有限公司 提供商务支持Copyright © 1999-2012, CSDN.NET, All Rights Reserved [![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
![](http://counter.csdn.net/pv.aspx?id=24)
