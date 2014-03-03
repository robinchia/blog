---
layout: post
title: "Oracle SQL学习笔记 之 GROUP函数与GROUP BY子句_Dragon Zone_百度"
categories: DB
tags: 
 - DB
 - oracle
 - 函数
--- 

# Oracle SQL学习笔记 之 GROUP函数与GROUP BY子句_Dragon Zone_百度空间

[](http://hi.baidu.com/)

* [首页](http://hi.baidu.com/home)
* [我的主页](http://hi.baidu.com/robinchia)
* [相册](http://hi.baidu.com/robinchia/albums)
* [广场](http://hi.baidu.com/index/)
* [消息 ](http://hi.baidu.com/qmsg)
*
* [私信](http://msg.baidu.com/)
*
* [模板](http://hi.baidu.com/set/show/theme)
*
* [设置](http://hi.baidu.com/set/show/profile)
*
* [退出](https://passport.baidu.com/?logout&bdstoken=aa9855a9aad4216b27441b12182ad1a7&u=http://hi.baidu.com/dianjinglong/item/be3883d0846b6e15d90e44d9)
[关注此空间](http://hi.baidu.com/dianjinglong/item/be3883d0846b6e15d90e44d9#)

# [Dragon Zone](http://hi.baidu.com/dianjinglong)

价值不是你拥有多少，而是你留下多少。

2007-11-23 14:08

## Oracle SQL学习笔记 之 GROUP函数与GROUP BY子句

1. Group函数
AVG
COUNT
MAX
MIN
STDDEV
SUM
VARIANCE
使用格式：
SELECT column, group_function(column)
FROM   table
[WHERE condition]
[ORDER BY column];

Group 函数忽视列里面的 null 值，受影响的语句可能是：
SQL> SELECT AVG(comm)
2 FROM   emp;
结果为：
AVG(COMM)
---------
      550

但可以使用NVL函数强迫group函数在统计时包含null值
【实例】
SQL> SELECT AVG(NVL(comm,0))
2 FROM   emp;
结果为：
AVG(NVL(COMM,0))
----------------
       157.14286

2. 使用GROUP BY子句
格式为：
SELECT column, group_function(column)
FROM   table
[WHERE condition]
[GROUP BY group_by_expression]
[ORDER BY column];
【实例】
SQL> SELECT   deptno, AVG(sal)
2 FROM     emp
3 GROUP BY deptno;
输出为：
   DEPTNO AVG(SAL)
--------- ---------
       10 2916.6667
       20      2175
       30 1566.6667

GROUP BY 列可以不在select的列表里面
【实例】
SQL> SELECT   AVG(sal)
2 FROM     emp
3 GROUP BY deptno;
输出为：
AVG(SAL)
---------
2916.6667
     2175
1566.6667

可以根据多个列进行分组
【实例】要实现“根据每个工作求出汇总工资，并按部门分组”：
SQL> SELECT   deptno, job, sum(sal)
2 FROM     emp
3 GROUP BY deptno, job;
原始数据为：
   DEPTNO JOB             SAL
--------- --------- ---------
       10 MANAGER        2450
       10 PRESIDENT      5000
       10 CLERK          1300
       20 CLERK           800
       20 CLERK          1100
       20 ANALYST        3000
       20 ANALYST        3000
       20 MANAGER        2975
       30 SALESMAN       1600
       30 MANAGER        2850
       30 SALESMAN       1250
       30 CLERK           950
       30 SALESMAN       1500
       30 SALESMAN       1250
输出为：
DEPTNO JOB        SUM(SAL)
-------- --------- ---------
      10 CLERK          1300
      10 MANAGER        2450
      10 PRESIDENT      5000
      20 ANALYST        6000
      20 CLERK          1900
      20 MANAGER        2975
      30 CLERK           950
      30 MANAGER        2850
      30 SALESMAN       5600

3. 使用 HAVING 子句来限制分组
SELECT column, group_function
FROM   table
[WHERE condition]
[GROUP BY group_by_expression]
[HAVING group_condition]
[ORDER BY column];
【实例】
SQL> SELECT   deptno, max(sal)
2 FROM     emp
3 GROUP BY deptno
4 HAVING   max(sal)>2900;
输出为：
   DEPTNO MAX(SAL)
--------- ---------
       10      5000
       20      3000

【实例】
SQL> SELECT    job, SUM(sal) PAYROLL
2 FROM      emp
3 WHERE   job NOT LIKE 'SALES%'
4 GROUP BY job
5 HAVING    SUM(sal)>5000
6 ORDER BY SUM(sal);
输出为：
JOB         PAYROLL
--------- ---------
ANALYST        6000
MANAGER        8275

4. 嵌套GROUP函数
【实例】
显示最大的平均工资
SQL> SELECT   max(avg(sal))
2 FROM     emp
3 GROUP BY deptno;
输出为：
MAX(AVG(SAL))
-------------
    2916.6667
[#操纵bit流_dbms](http://hi.baidu.com/tag/%E6%93%8D%E7%BA%B5bit%E6%B5%81_dbms/feeds)

分享到： []()[]()[]()[]()

[举报](http://hi.baidu.com/dianjinglong/item/be3883d0846b6e15d90e44d9#)浏览(754) [评论](http://hi.baidu.com/dianjinglong/item/be3883d0846b6e15d90e44d9#)[转载](http://hi.baidu.com/dianjinglong/item/be3883d0846b6e15d90e44d9#)

### 你可能也喜欢

* [![晴天里的花草]( "晴天里的花草")](http://hi.baidu.com/yuuika/item/9ad0fc95200ae2895914614b)[晴天里的花草](http://hi.baidu.com/yuuika/item/9ad0fc95200ae2895914614b)
* [![天池的第一场雪]( "天池的第一场雪")](http://hi.baidu.com/fotocome/item/c60d10444a87c9226dc2f0e1)[天池的第一场雪](http://hi.baidu.com/fotocome/item/c60d10444a87c9226dc2f0e1)
* [![夕阳中的红松鸡（念）]( "夕阳中的红松鸡（念）")](http://hi.baidu.com/15210449266/item/fc45c2d2d99028f23cc2cb73)[夕阳中的红松鸡（念）](http://hi.baidu.com/15210449266/item/fc45c2d2d99028f23cc2cb73)
* [![北红尾鸲GG]( "北红尾鸲GG")](http://hi.baidu.com/shijian2012/item/6e602a488c3eedeabcf45179)[北红尾鸲GG](http://hi.baidu.com/shijian2012/item/6e602a488c3eedeabcf45179)
* [![摄影美丽中国自然生态篇]( "摄影美丽中国自然生态篇")](http://hi.baidu.com/gunxueqiu8888/item/416b1fb96101e25e244b090d)[摄影美丽中国自然生态篇](http://hi.baidu.com/gunxueqiu8888/item/416b1fb96101e25e244b090d)
* [![山寺的迟来之秋 照样迷人的秋韵]( "山寺的迟来之秋 照样迷人的秋韵")](http://hi.baidu.com/mxw717/item/b73c9282892f9ff25e0ec1a4)[山寺的迟来之秋 照样迷人的秋韵](http://hi.baidu.com/mxw717/item/b73c9282892f9ff25e0ec1a4)
* [![Hello World!]( "Hello World!")](http://hi.baidu.com/dianjinglong/item/dcb7ecf47b3eb9dd6325d2d5)[Hello World!](http://hi.baidu.com/dianjinglong/item/dcb7ecf47b3eb9dd6325d2d5)
[](http://hi.baidu.com/dianjinglong/item/afab2a925e4958f3291647d9 "上一篇")

[](http://hi.baidu.com/dianjinglong/item/107a59f2b28d9e0dc6dc45d9 "下一篇")

评论
[帮助中心](http://help.baidu.com/question?prod_en=hi) | [空间客服](http://tieba.baidu.com/p/1781683739) | [投诉中心](http://tousu.baidu.com/hi/add) | [空间协议](http://www.baidu.com/search/hi_contract.html)

©2012 Baidu
