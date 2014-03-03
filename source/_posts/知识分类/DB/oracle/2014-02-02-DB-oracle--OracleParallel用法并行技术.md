---
layout: post
title: "Oracle Parallel用法 并行技术"
categories: DB
tags: 
 - DB
 - oracle
--- 

# Oracle Parallel用法 并行技术

一、Parallel
1．用途
强行启用并行度来执行当前SQL。这个在Oracle 9i之后的版本可以使用，之前的版本现在没有环境进行测试。也就是说，加上这个说明，可以强行启用Oracle的多线程处理功能。举例的话，就像电脑装了多核的CPU，但大多情况下都不会完全多核同时启用（2核以上的比较明显），使用parallel说明，就会多核同时工作，来提高效率。
但本身启动这个功能，也是要消耗资源与性能的。所有，一般都会在返回记录数大于100万时使用，效果也会比较明显。
2．语法
/*+parallel(table_short_name,cash_number)*/
这个可以加到insert、delete、update、select的后面来使用（和rule的用法差不多，有机会再分享rule的用法）
开启parallel功能的语句是：
alter session enable parallel dml;
这个语句是DML语句哦，如果在程序中用，用execute的方法打开。
3．实例说明
用ERP中的transaction来说明下吧。这个table记录了所有的transaction，而且每天数据量也算相对比较大的（根据企业自身业务量而定）。假设我们现在要查看对比去年一年当中每月的进、销情况，所以，一般都会写成：
[复制内容到剪贴板]()![程序代码]() 程序代码

select to_char(transaction_date,'yyyymm') txn_month,
       sum(
        decode(
            sign(transaction_quantity),1,transaction_quantity,0
              )
          ) in_qty,
       sum(
        decode(
            sign(transaction_quantity),-1,transaction_quantity,0
              )
          ) out_qty
  from mtl_material_transactions mmt
where transaction_date >= add_months(
                            to_date(    
                                to_char(sysdate,'yyyy')||'0101','yyyymmdd'),
                                -12)
   and transaction_date <= add_months(
                            to_date(
                                to_char(sysdate,'yyyy')||'1231','yyyymmdd'),
                                -12)
group by to_char(transaction_date,'yyyymm') 
这个SQL执行起来，如果transaction_date上面有加index的话，效率还算过的去；但如果没有加index的话，估计就会半个小时内都执行不出来。这是就可以在select 后面加上parallel说明。例如：
[复制内容到剪贴板]()![程序代码]() 程序代码

select /*+parallel(mmt,10)*/
       to_char(transaction_date,'yyyymm') txn_month,
...
这样的话，会大大提高执行效率。如果要将检索出来的结果insert到另一个表tmp_count_tab的话，也可以写成：
[复制内容到剪贴板]()![程序代码]() 程序代码

insert /*+parallel(t,10)*/
  into tmp_count_tab
(
    txn_month,
    in_qty,
    out_qty
)
select /*+parallel(mmt,10)*/
       to_char(transaction_date,'yyyymm') txn_month,
...
插入的机制和检索机制差不多，所以，在insert后面加parallel也会加速的。关于insert机制，这里暂不说了。
Parallel后面的数字，越大，执行效率越高。不过，貌似跟server的配置还有oracle的配置有关，增大到一定值，效果就不明显了。所以，一般用8,10,12,16的比较常见。我试过用30，发现和16的效果一样。不过，数值越大，占用的资源也会相对增大的。如果是在一些package、function or procedure中写的话，还是不要写那么大，免得占用太多资源被DBA开K。
4．Parallel也可以用于多表
多表的话，就是在第一后面，加入其他的就可以了。具体写法如下：
/*+parallel(t,10) (b,10)*/
5．小结
关于执行效率，建议还是多按照index的方法来提高效果。Oracle有自带的explan road的方法，在执行之前，先看下执行计划路线，对写好的SQL tuned之后再执行。实在没办法了，再用parallel方法。Parallel比较邪恶，对开发者而言，不是好东西，会养成不好习惯，导致很多bad SQL不会暴漏，SQL Tuning的能力得不到提升。我有见过某些人create table后，从不create index或primary key，认为写SQL时加parallel就可以了。

来源： <[http://www.wudongqi.com/article/519.htm](http://www.wudongqi.com/article/519.htm)>
 

# oracle的Parallel 并行技术
 

**启用Parallel****前的忠告：**只有在需要处理一个很大的任务，如需要几十分钟，几个小时的作业中，并且要有足够的系统资源的情况下(这些资源包括cpu，内存，io),您才应该考虑使用parallel。否则，在一个多并发用户下，系统本身资源负担已经很大的情况下，启用parallel，将会导致某一个会话试图占用了所有的资源，其他会话不得不去等待，从而导致系统系能反而下降的情况，一般情况下，oltp系统不要使用parallel，olap系统中可以考虑去使用。

 

Parallel分类

l        并行查询parallel query

l        并行dml parallel dml pdml

 

 

l        **并行查询**

并行查询允许将一个**[**sql**]()**select语句划分为多个较小的查询，每个部分的查询并发地运行，然后将各个部分的结果组合起来，提供最终的结果，多用于全表扫描，索引全扫描等，大表的扫描和连接、创建大的索引、分区索引扫描、大批量插入更新和删除
**启用并行查询**

 

告知**[**oracle**]()**，对T1启用parallel查询，但并行度要参照系统的资源负载状况来确定。

利用hints提示，启用并行，同时也可以告知明确的并行度，否则oracle自行决定启用的并行度，这些提示只对该sql语句有效。

SQL> select /*+ parallel(t1 8) */ count(*) from t1;

 

SQL> select degree from user_tables**where**table_name='T1';

DEGREE

--------------------

  DEFAULT

 

并行度为Default，其值由下面2个参数决定

SQL> show parameter cpu

 

NAME                                TYPE       VALUE

------------------------------------ ----------- ------------------------------

cpu_count                           integer    2

parallel_threads_per_cpu            integer    2

 

cpu_count表示cpu数

parallel_threads_per_cpu表示每个cpu允许的并行进程数

default情况下，并行数为cpu_count*parallel_threads_per_cpu

**取消并行设置**

SQL> alter table t1 noparallel;

SQL> select degree from user_tables where table_name='T1';

 

DEGREE

----------------------------------------

        1

对于一个大的任务，一般的做法是利用一个进程，串行的执行，如果系统资源足够，可以采用parallel技术，把一个大的任务分成若干个小的任务，同时启用n个进程/线程，并行的处理这些小的任务，这些并发的进程称为并行执行[**服务器**]()(parallel executeion**[**server**]()**)，这些并发进程由一个称为并发协调进程的进程来[**管理**]()。

**启用Parallel****前的忠告：**只有在需要处理一个很大的任务，如需要几十分钟，几个小时的作业中，并且要有足够的系统资源的情况下(这些资源包括cpu，内存，io),您才应该考虑使用parallel。否则，在一个多并发用户下，系统本身资源负担已经很大的情况下，启用parallel，将会导致某一个会话试图占用了所有的资源，其他会话不得不去等待，从而导致系统系能反而下降的情况，一般情况下，oltp系统不要使用parallel，oltp系统中可以考虑去使用。

 

Parallel分类

l        并行查询parallel query

l        并行dml parallel dml pdml

l        并行ddl parallel ddl pddl

 

l        **并行查询**

并行查询允许将一个**sql**select语句划分为多个较小的查询，每个部分的查询并发地运行，然后将各个部分的结果组合起来，提供最终的结果，多用于全表扫描，索引全扫描等，大表的扫描和连接、创建大的索引、分区索引扫描、大批量插入更新和删除

 

**启用并行查询**

SQL> ALTER TABLE T1 PARALLEL;

告知**oracle**，对T1启用parallel查询，但并行度要参照系统的资源负载状况来确定。

利用hints提示，启用并行，同时也可以告知明确的并行度，否则oracle自行决定启用的并行度，这些提示只对该sql语句有效。

SQL> select /*+ parallel(t1 8) */ count(*) from t1;

 

SQL> select degree from user_tables**where**table_name='T1';

DEGREE

--------------------

  DEFAULT

 

并行度为Default，其值由下面2个参数决定

SQL> show parameter cpu

 

NAME                                TYPE       VALUE

------------------------------------ ----------- ------------------------------

cpu_count                           integer    2

parallel_threads_per_cpu            integer    2

 

cpu_count表示cpu数

parallel_threads_per_cpu表示每个cpu允许的并行进程数

default情况下，并行数为cpu_count*parallel_threads_per_cpu

 

**取消并行设置**

SQL> alter table t1 noparallel;

SQL> select degree from user_tables where table_name='T1';

 

DEGREE

----------------------------------------

        1

 

**数据字典视图**

v$px_session

sid：各个并行会话的sid

qcsid：query coordinator sid,查询协调器sid

 

l        **并行dml**
**
并行dml包括insert，update，delete，merge，在pdml期间，oracle可以使用多个并行执行服务器来执行insert，update，delete，merge，多个会话同时执行，同时每个会话(并发进程)都有自己的undo段，都是独立的一个事务，这些事务要么由pdml协调器进程提交，要么都rollback。

在一个有充足I/o带宽的多cpu主机中，对于大规模的dml，速度可能会有很大的提升，尤其是在大型的数据仓库环境中。

并行dml需要显示的启用

SQL> alter session enable parallel dml;

 

Disable并行dml

SQL> alter session disable parallel dml;
来源： <[http://space.itpub.net/8183550/viewspace-667633](http://space.itpub.net/8183550/viewspace-667633)> 

 

![]() oracle并行查询一例

今天碰到一个开发人员反映SQL执行时间过长。根本无法得到结果集。
        看到服务器压力也没有很高，估计又是一个非常消耗磁盘的查询。果然，发现是一个200w的表和一个超过1100w表的HASH JOIN . 
        简单的帮助优化了一个SQL后，SQL如下:
    
select    count(ui.usin_uid_fk)
    from table1 av, table2 ui
where av.av_usse_activatedate >= to_date('20090102', 'yyyymmdd')
     and av.av_usse_activatedate < to_date('20090401', 'yyyymmdd')
     and av.av_usse_uid_fk = ui.usin_uid_fk
     and ui.usin_mcnc_fk =XXX%'

       不难想象执行的不是很理想。近20分钟的执行时间，真是让人崩溃。

COUNT(UI.USIN_UID_FK)
---------------------
                            1918591
Elapsed: 00:19:03.07
Statistics
----------------------------------------------------------
                    0    recursive calls
                    0    db block gets
     32921639    consistent gets
         352073    physical reads
                    0    redo size
                395    bytes sent via SQL*Net to client
                503    bytes received via SQL*Net from client
                    2    SQL*Net roundtrips to/from client
                    0    sorts (memory)
                    0    sorts (disk)
                    1    rows processed

        对于那张TABLE2的大表（符合条件的超过1100w），决定试图通过并行来提高执行速度。SQL如下：

select /*+parallel (tbl_userinfo 4)*/ count(ui.usin_uid_fk)
from table1 av, table2 ui
where av.av_usse_activatedate >= to_date('20090101', 'yyyymmdd')
and av.av_usse_activatedate < to_date('20090401', 'yyyymmdd')
and av.av_usse_uid_fk = ui.usin_uid_fk
and ui.usin_mcnc_fk like 'XXX%';

      执行效果还是非常明显的。从19分钟多到1分45秒！其中consistent gets更是减少了一个数量级 -:)
    

COUNT(UI.USIN_UID_FK)
---------------------
                            1918591
Elapsed: 00:01:45.15
Statistics
----------------------------------------------------------
                    0    recursive calls
                    0    db block gets
        2571109    consistent gets
         124523    physical reads
                    0    redo size
                395    bytes sent via SQL*Net to client
                504    bytes received via SQL*Net from client
                    2    SQL*Net roundtrips to/from client
                    0    sorts (memory)
                    0    sorts (disk)
                    1    rows processed

    
       
      因为这个服务器为2×4核心的cpu，应该可以算是8个CPU，所以应该可以通过增加并行度来进一步减少执行时间。如下SQL：
    

SQL> select /*+parallel (tbl_userinfo 8)*/ count(ui.usin_uid_fk)
    2        from table1 av, table2 ui
    3     where av.av_usse_activatedate >= to_date('20090101', 'yyyymmdd')
    4         and av.av_usse_activatedate < to_date('20090401', 'yyyymmdd')
    5         and av.av_usse_uid_fk = ui.usin_uid_fk
    6         and ui.usin_mcnc_fk like '460%';
COUNT(UI.USIN_UID_FK)
---------------------
                            1949033
Elapsed: 00:00:20.60
Statistics
----------------------------------------------------------
                    0    recursive calls
                    0    db block gets
        2607524    consistent gets
            55050    physical reads
                    0    redo size
                395    bytes sent via SQL*Net to client
                503    bytes received via SQL*Net from client
                    2    SQL*Net roundtrips to/from client
                    0    sorts (memory)
                    0    sorts (disk)
                    1    rows processed

       可以说还是比较理想的。只有20S左右了。虽然最大并行度可以到CPU*2，但是效果未必会好。进一步做一个16个并行度的SQL执行测试。
     

COUNT(UI.USIN_UID_FK)
---------------------
                            1949033
Elapsed: 00:00:20.64
Statistics
----------------------------------------------------------
                    0    recursive calls
                    0    db block gets
        2607524    consistent gets
            55299    physical reads
                    0    redo size
                395    bytes sent via SQL*Net to client
                504    bytes received via SQL*Net from client
                    2    SQL*Net roundtrips to/from client
                    0    sorts (memory)
                    0    sorts (disk)
                    1    rows processed

      
       没有任何提高，并且执行时间还稍高于并行度为8的SQL。
       通过以上测试我们不难发现：
       在处理大量数据查询，例如出现HASH JOIN的情况下，并行查询非常有效果的。也就是说并行查询在数据仓库这样的应用中会“大显身手”。
        但是并行查询的使用还是有很多限制的。例如相对较小的数据查询和连接是会适得其反的。盲目增加并行度也是大忌，相对来讲，并行度和CPU数相同比较好。这里的CPU数应该是指的核心数。例如服务器中有一个CPU是4核心的，并行度为4是好的。
        技术很难有十全十美的，最重要的是对于特定技术的使用要恰到好处，保证扬长避短。 -:)
 ----------------------------
 以上测试环境：
ORACLE 9.2.0.4 
RHEL 4 U4

来源： <[http://miracle.blog.51cto.com/255044/147058](http://miracle.blog.51cto.com/255044/147058)> **
