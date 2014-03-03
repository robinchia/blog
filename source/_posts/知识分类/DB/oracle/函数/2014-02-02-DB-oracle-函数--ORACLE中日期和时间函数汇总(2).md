---
layout: post
title: "ORACLE中日期和时间函数汇总 (2)"
categories: DB
tags: 
 - DB
 - oracle
 - 函数
--- 

# ORACLE中日期和时间函数汇总 (2)

[.Net](http://www.cnblogs.com/lizw/)   [![]()](http://www.cnblogs.com/) [![]()](http://www.cnblogs.com/logout.aspx) 公告

* [我的主页](http://home.cnblogs.com/lizw/)  [个人资料](http://home.cnblogs.com/lizw/detail/)
[我的闪存](http://home.cnblogs.com/lizw/ing/)  [发短消息](http://space.cnblogs.com/msg/send/ä¸æ¹æ°ç§)
日历 [<]( "Go to the previous month") 2007年4月 [>]( "Go to the next month") 日 一 二 三 四 五 六 25 26 27 28 29 30 31 [1](http://www.cnblogs.com/lizw/archive/2007/4/1.html) [2](http://www.cnblogs.com/lizw/archive/2007/4/2.html) 3 4 5 [6](http://www.cnblogs.com/lizw/archive/2007/4/6.html) 7 8 9 10 11 12 [13](http://www.cnblogs.com/lizw/archive/2007/4/13.html) 14 [15](http://www.cnblogs.com/lizw/archive/2007/4/15.html) 16 17 18 [19](http://www.cnblogs.com/lizw/archive/2007/4/19.html) 20 [21](http://www.cnblogs.com/lizw/archive/2007/4/21.html) [22](http://www.cnblogs.com/lizw/archive/2007/4/22.html) 23 24 25 [26](http://www.cnblogs.com/lizw/archive/2007/4/26.html) 27 28 [29](http://www.cnblogs.com/lizw/archive/2007/4/29.html) [30](http://www.cnblogs.com/lizw/archive/2007/4/30.html) 1 2 3 4 5

统计

* 随笔 - 88
* 文章 - 3
* 评论 - 10
* 引用 - 0

# 导航

* [博客园](http://www.cnblogs.com/)
* [首页](http://www.cnblogs.com/lizw/)
* [发新随笔](http://www.cnblogs.com/lizw/admin/EditPosts.aspx?opt=1)
* [发新文章](http://www.cnblogs.com/EnterMyBlog.aspx?NewArticle=1)
* [联系](http://space.cnblogs.com/msg/send/ä¸æ¹æ°ç§)
* [订阅](http://www.cnblogs.com/lizw/rss)[![订阅]()](http://www.cnblogs.com/lizw/rss)
* [管理](http://www.cnblogs.com/lizw/admin/EditPosts.aspx)
### 搜索

 

 

### 常用链接

* [我的随笔](http://www.cnblogs.com/lizw/MyPosts.html)
* [我的空间](http://home.cnblogs.com/lizw/)
* [我的短信](http://space.cnblogs.com/msg/recent)
* [我的评论](http://www.cnblogs.com/lizw/MyComments.html)
* [更多链接](http://www.cnblogs.com/lizw/archive/2007/04/15/714015.html#)
* [我的参与](http://www.cnblogs.com/lizw/OtherPosts.html "我发表过评论的随笔")
* [我的新闻](http://www.cnblogs.com/lizw/MyNews.html)
* [最新评论](http://www.cnblogs.com/lizw/RecentComments.html)
* [我的标签](http://www.cnblogs.com/lizw/tag/)

# 随笔档案

* [2008年3月 (1)](http://www.cnblogs.com/lizw/archive/2008/03.html)
* [2007年9月 (6)](http://www.cnblogs.com/lizw/archive/2007/09.html)
* [2007年8月 (7)](http://www.cnblogs.com/lizw/archive/2007/08.html)
* [2007年7月 (6)](http://www.cnblogs.com/lizw/archive/2007/07.html)
* [2007年6月 (17)](http://www.cnblogs.com/lizw/archive/2007/06.html)
* [2007年5月 (13)](http://www.cnblogs.com/lizw/archive/2007/05.html)
* [2007年4月 (21)](http://www.cnblogs.com/lizw/archive/2007/04.html)
* [2007年3月 (17)](http://www.cnblogs.com/lizw/archive/2007/03.html)

# 新闻档案

* [2007年3月 (1)](http://www.cnblogs.com/lizw/news/2007/03.html)

# 相册

* [常见名车标识](http://www.cnblogs.com/lizw/gallery/88847.html)

### 积分与排名

* 积分 - 53936
* 排名 - 1522

### 最新评论 [![]()](http://www.cnblogs.com/lizw/CommentsRSS.aspx)

* [1. re: Oracle分析函数RANK(),ROW_NUMBER(),LAG()等的使用方法(转载)](http://www.cnblogs.com/lizw/archive/2007/04/26/729005.html#1514361)
* LAG 表示 分组排序后 ，组内后面一条记录减前面一条记录的差，第一条可返回 NULL ——>应该是：同一字段的前N行的数据
* --guest
* [2. re: oracle查看所有函数或存储过程的代码](http://www.cnblogs.com/lizw/archive/2007/06/12/779853.html#1216344)
* 太谢谢了！
* --zqf
* [3. re: 从Oracle数据库中导出SQL脚本](http://www.cnblogs.com/lizw/archive/2007/09/05/883654.html#1074483)
* 我用了上面的方法，但我导出的内容却只显示了一个字段，这是为什么？以下是显示的结果： SQL> select dbms_metadata.get_ddl('TABLE','BB_ACCOUNT'...
* --landril
* [4. re: Oracle学习网址及论坛积累](http://www.cnblogs.com/lizw/archive/2007/05/28/763136.html#1036077)
* dd
* --匿名
* [5. re: Oracle学习网址及论坛积累](http://www.cnblogs.com/lizw/archive/2007/05/28/763136.html#1036075)
* 所谓Oracel 主键的删除是指 主键的创建 alter table userinfo add constraint user_id_key primary key(id); 上条语句是指，有创建命...
* --匿名

### 阅读排行榜

* [1. ORACLE中日期和时间函数汇总(转载) (14275)](http://www.cnblogs.com/lizw/archive/2007/04/15/714015.html)
* [2. Oracle分析函数RANK(),ROW_NUMBER(),LAG()等的使用方法(转载)(8217)](http://www.cnblogs.com/lizw/archive/2007/04/26/729005.html)
* [3. 利用ORACLE的MINUS函数和OVER函数，直接通过视图实现两个记录集的比较。(转载)(6045)](http://www.cnblogs.com/lizw/archive/2007/04/26/729006.html)
* [4. Oracle统计分析函数集之一(转载)(3729)](http://www.cnblogs.com/lizw/archive/2007/04/26/729004.html)
* [5. oracle查看所有函数或存储过程的代码(3508)](http://www.cnblogs.com/lizw/archive/2007/06/12/779853.html)

### 评论排行榜

* [1. 从Oracle数据库中导出SQL脚本(2)](http://www.cnblogs.com/lizw/archive/2007/09/05/883654.html)
* [2. Oracle学习网址及论坛积累(2)](http://www.cnblogs.com/lizw/archive/2007/05/28/763136.html)
* [3. oracle查看所有函数或存储过程的代码(1)](http://www.cnblogs.com/lizw/archive/2007/06/12/779853.html)
* [4. Oracle分析函数RANK(),ROW_NUMBER(),LAG()等的使用方法(转载)(1)](http://www.cnblogs.com/lizw/archive/2007/04/26/729005.html)
* [5. Oracle统计分析函数集之一(转载)(1)](http://www.cnblogs.com/lizw/archive/2007/04/26/729004.html)

 
 [ORACLE中日期和时间函数汇总(转载)](http://www.cnblogs.com/lizw/archive/2007/04/15/714015.html)

在oracle中处理日期大全
  TO_DATE格式  
Day:  
dd number 12  
dy abbreviated fri  
day spelled out friday  
ddspth spelled out, ordinal twelfth  
Month:  
mm number 03  
mon abbreviated mar  
month spelled out march  
Year:  
yy two digits 98  
yyyy four digits 1998  
24小时格式下时间范围为： 0:00:00 - 23:59:59....  
12小时格式下时间范围为： 1:00:00 - 12:59:59 ....  
1.  
日期和字符转换函数用法（to_date,to_char）  
2.  
select to_char( to_date(222,'J'),'Jsp') from dual  
显示Two Hundred Twenty-Two  
3.  
求某天是星期几  
select to_char(to_date('2002-08-26','yyyy-mm-dd'),'day') from dual;  
星期一  
select to_char(to_date('2002-08-26','yyyy-mm-dd'),'day','NLS_DATE_LANGUAGE = American') from dual;  
monday  
设置日期语言  
ALTER SESSION SET NLS_DATE_LANGUAGE='AMERICAN';  
也可以这样  
TO_DATE ('2002-08-26', 'YYYY-mm-dd', 'NLS_DATE_LANGUAGE = American')  
4.  
两个日期间的天数  
select floor(sysdate - to_date('20020405','yyyymmdd')) from dual;  
5. 时间为null的用法  
select id, active_date from table1  
UNION  
select 1, TO_DATE(null) from dual;  
注意要用TO_DATE(null)  
6.  
a_date between to_date('20011201','yyyymmdd') and to_date('20011231','yyyymmdd')  
那么12月31号中午12点之后和12月1号的12点之前是不包含在这个范围之内的。  
所以，当时间需要精确的时候，觉得to_char还是必要的  
7. 日期格式冲突问题  
输入的格式要看你安装的ORACLE字符集的类型, 比如: US7ASCII, date格式的类型就是: '01-Jan-01'  
alter system set NLS_DATE_LANGUAGE = American  
alter session set NLS_DATE_LANGUAGE = American  
或者在to_date中写  
select to_char(to_date('2002-08-26','yyyy-mm-dd'),'day','NLS_DATE_LANGUAGE = American') from dual;  
注意我这只是举了NLS_DATE_LANGUAGE，当然还有很多，  
可查看  
select * from nls_session_parameters  
select * from V$NLS_PARAMETERS  
8.  
select count(*)  
from ( select rownum-1 rnum  
from all_objects  
where rownum <= to_date('2002-02-28','yyyy-mm-dd') - to_date('2002-  
02-01','yyyy-mm-dd')+1  
)  
where to_char( to_date('2002-02-01','yyyy-mm-dd')+rnum-1, 'D' )  
not  
in ( '1', '7' )  
查找2002-02-28至2002-02-01间除星期一和七的天数  
在前后分别调用DBMS_UTILITY.GET_TIME, 让后将结果相减(得到的是1/100秒, 而不是毫秒).  
9.  
select months_between(to_date('01-31-1999','MM-DD-YYYY'),  
to_date('12-31-1998','MM-DD-YYYY')) "MONTHS" FROM DUAL;  
1  
select months_between(to_date('02-01-1999','MM-DD-YYYY'),  
to_date('12-31-1998','MM-DD-YYYY')) "MONTHS" FROM DUAL;  
1.03225806451613  
10. Next_day的用法  
Next_day(date, day)  
Monday-Sunday, for format code DAY  
Mon-Sun, for format code DY  
1-7, for format code D  
11  
select to_char(sysdate,'hh:mi:ss') TIME from all_objects  
注意：第一条记录的TIME 与最后一行是一样的  
可以建立一个函数来处理这个问题  
create or replace function sys_date return date is  
begin  
return sysdate;  
end;  
select to_char(sys_date,'hh:mi:ss') from all_objects;  
12.  
获得小时数  
SELECT EXTRACT(HOUR FROM TIMESTAMP '2001-02-16 2:38:40') from offer  
SQL> select sysdate ,to_char(sysdate,'hh') from dual;  
SYSDATE TO_CHAR(SYSDATE,'HH')  
-------------------- ---------------------  
2003-10-13 19:35:21 07  
SQL> select sysdate ,to_char(sysdate,'hh24') from dual;  
SYSDATE TO_CHAR(SYSDATE,'HH24')  
-------------------- -----------------------  
2003-10-13 19:35:21 19  
获取年月日与此类似  
13.  
年月日的处理  
select older_date,  
newer_date,  
years,  
months,  
abs(  
trunc(  
newer_date-  
add_months( older_date,years*12+months )  
)  
) days  
from ( select  
trunc(months_between( newer_date, older_date )/12) YEARS,  
mod(trunc(months_between( newer_date, older_date )),  
12 ) MONTHS,  
newer_date,  
older_date  
from ( select hiredate older_date,  
add_months(hiredate,rownum)+rownum newer_date  
from emp )  
)  
14.  
处理月份天数不定的办法  
select to_char(add_months(last_day(sysdate) +1, -2), 'yyyymmdd'),last_day(sysdate) from dual  
16.  
找出今年的天数  
select add_months(trunc(sysdate,'year'), 12) - trunc(sysdate,'year') from dual  
闰年的处理方法  
to_char( last_day( to_date('02' || :year,'mmyyyy') ), 'dd' )  
如果是28就不是闰年  
17.  
yyyy与rrrr的区别  
'YYYY99 TO_C  
------- ----  
yyyy 99 0099  
rrrr 99 1999  
yyyy 01 0001  
rrrr 01 2001  
18.不同时区的处理  
select to_char( NEW_TIME( sysdate, 'GMT','EST'), 'dd/mm/yyyy hh:mi:ss') ,sysdate  
from dual;  
19.  
5秒钟一个间隔  
Select TO_DATE(FLOOR(TO_CHAR(sysdate,'SSSSS')/300) * 300,'SSSSS') ,TO_CHAR(sysdate,'SSSSS')  
from dual  
2002-11-1 9:55:00 35786  
SSSSS表示5位秒数  
20.  
一年的第几天  
select TO_CHAR(SYSDATE,'DDD'),sysdate from dual  
310 2002-11-6 10:03:51  
21.计算小时,分,秒,毫秒  
select  
Days,  
A,  
TRUNC(A*24) Hours,  
TRUNC(A*24*60 - 60*TRUNC(A*24)) Minutes,  
TRUNC(A*24*60*60 - 60*TRUNC(A*24*60)) Seconds,  
TRUNC(A*24*60*60*100 - 100*TRUNC(A*24*60*60)) mSeconds  
from  
(  
select  
trunc(sysdate) Days,  
sysdate - trunc(sysdate) A  
from dual  
)  
select * from tabname  
order by decode(mode,'FIFO',1,-1)*to_char(rq,'yyyymmddhh24miss');  
//  
floor((date2-date1) /365) 作为年  
floor((date2-date1, 365) /30) 作为月  
mod(mod(date2-date1, 365), 30)作为日.  
23.next_day函数  
next_day(sysdate,6)是从当前开始下一个星期五。后面的数字是从星期日开始算起。  
1 2 3 4 5 6 7  
日 一 二 三 四 五 六

 

**oracle中有很多关于日期的函数
**在oracle中有很多关于日期的函数，如：
1、add_months()用于从一个日期值增加或减少一些月份
date_value:=add_months(date_value,number_of_months)
例：
SQL> select add_months(sysdate,12) "Next Year" from dual;
Next Year
----------
13-11月-04
SQL> select add_months(sysdate,112) "Last Year" from dual;
Last Year
----------
13-3月 -13
SQL>
2、current_date()返回当前会放时区中的当前日期
date_value:=current_date
SQL> column sessiontimezone for a15
SQL> select sessiontimezone,current_date from dual;
SESSIONTIMEZONE CURRENT_DA
--------------- ----------
+08:00 13-11月-03
SQL> alter session set time_zone='-11:00'
  2 /
会话已更改。
SQL> select sessiontimezone,current_timestamp from dual;
SESSIONTIMEZONE CURRENT_TIMESTAMP
--------------- ------------------------------------
-11:00 12-11月-03 04.59.13.668000 下午 -11:
                00
SQL>
3、current_timestamp()以timestamp with time zone数据类型返回当前会放时区中的当前日期
timestamp_with_time_zone_value:=current_timestamp([timestamp_precision])
SQL> column sessiontimezone for a15
SQL> column current_timestamp format a36
SQL> select sessiontimezone,current_timestamp from dual;
SESSIONTIMEZONE CURRENT_TIMESTAMP
--------------- ------------------------------------
+08:00 13-11月-03 11.56.28.160000 上午 +08:
                00
SQL> alter session set time_zone='-11:00'
  2 /
会话已更改。
SQL> select sessiontimezone,current_timestamp from dual;
SESSIONTIMEZONE CURRENT_TIMESTAMP
--------------- ------------------------------------
-11:00 12-11月-03 04.58.00.243000 下午 -11:
                00
SQL>
4、dbtimezone()返回时区
varchar_value:=dbtimezone
SQL> select dbtimezone from dual;
DBTIME
------
-07:00
SQL>
5、extract()找出日期或间隔值的字段值
date_value:=extract(date_field from [datetime_value|interval_value])
SQL> select extract(month from sysdate) "This Month" from dual;
This Month
----------
        11
SQL> select extract(year from add_months(sysdate,36)) "3 Years Out" from dual;
3 Years Out
-----------
       2006
SQL>
6、last_day()返回包含了日期参数的月份的最后一天的日期
date_value:=last_day(date_value)
SQL> select last_day(date'2000-02-01') "Leap Yr?" from dual;
Leap Yr?
----------
29-2月 -00
SQL> select last_day(sysdate) "Last day of this month" from dual;
Last day o
----------
30-11月-03
SQL>
7、localtimestamp()返回会话中的日期和时间
timestamp_value:=localtimestamp
SQL> column localtimestamp format a28
SQL> select localtimestamp from dual;
LOCALTIMESTAMP
----------------------------
13-11月-03 12.09.15.433000
下午
SQL> select localtimestamp,current_timestamp from dual;
LOCALTIMESTAMP CURRENT_TIMESTAMP
---------------------------- ------------------------------------
13-11月-03 12.09.31.006000 13-11月-03 12.09.31.006000 下午 +08:
下午 00
SQL> alter session set time_zone='-11:00';
会话已更改。
SQL> select localtimestamp,to_char(sysdate,'DD-MM-YYYY HH:MI:SS AM') "SYSDATE" from dual;
LOCALTIMESTAMP SYSDATE
---------------------------- ------------------------
12-11月-03 05.11.31.259000 13-11-2003 12:11:31 下午
下午
SQL>
8、months_between()判断两个日期之间的月份数量
number_value:=months_between(date_value,date_value)
SQL> select months_between(sysdate,date'1971-05-18') from dual;
MONTHS_BETWEEN(SYSDATE,DATE'1971-05-18')
----------------------------------------
                              389.855143
SQL> select months_between(sysdate,date'2001-01-01') from dual;
MONTHS_BETWEEN(SYSDATE,DATE'2001-01-01')
----------------------------------------
                              34.4035409
SQL>
9、next_day()给定一个日期值，返回由第二个参数指出的日子第一次出现在的日期值（应返回相应日子的名称字符串）

 

**與周相關日期函數
**1.查询某周的第一天
select trunc(decode(ww, 53, to_date(yy || '3112', 'yyyyddmm'), to_date(yy || '-' || to_char(ww * 7), 'yyyy-ddd')), 'd') last_day
from (select substr('2004-32', 1, 4) yy, to_number(substr('2004-32', 6)) ww
         from dual)
select trunc(to_date(substr('2003-01',1,5)||to_char((to_number(substr('2003-01',6)))*7),'yyyy-ddd'),'d')-6 first_day from dual
select min(v_date) from
  (select (to_date('200201','yyyymm') + rownum) v_date
  from all_tables
  where rownum < 370)
where to_char(v_date,'yyyy-iw') = '2002-49'
2.查询某周的最后一天
select trunc(decode(ww, 53, to_date(yy || '3112', 'yyyyddmm'), to_date(yy || '-' || to_char(ww * 7), 'yyyy-ddd')), 'd') - 6 first_day
  from (select substr('2004-33', 1, 4) yy, to_number(substr('2004-33', 6)) ww
          from dual)
         
select trunc(to_date(substr('2003-01',1,5)||to_char((to_number(substr('2003-01',6)))*7),'yyyy-ddd'),'d') last_day from dual
select max(v_date) from
  (select (to_date('200408','yyyymm') + rownum) v_date
  from all_tables
  where rownum < 370)
where to_char(v_date,'yyyy-iw') = '2004-33'
3.查询某周的日期
select min_date, to_char(min_date,'day') day from
(select to_date(substr('2004-33',1,4)||'001'+rownum-1,'yyyyddd') min_date
        from all_tables
  where rownum <= decode(mod(to_number(substr('2004-33',1,4)),4),0,366,365)  
  union
  select to_date(substr('2004-33',1,4)-1||
         decode(mod(to_number(substr('2004-33',1,4))-1,4),0,359,358)+rownum,'yyyyddd') min_date
        from all_tables         
          where rownum <= 7
  union
  select to_date(substr('2004-33',1,4)+1||'001'+rownum-1,'yyyyddd') min_date
        from all_tables         
          where rownum <= 7                       
)
where to_char(min_date,'yyyy-iw') ='2004-33'

 

**oracle中时间运算
**论坛中常常看到有对oracle中时间运算提问的问题，今天有时间，看了看以前各位兄弟的贴子，整理了一下，并作了个示例，希望会对大家有帮助。
首先感谢ern、eric.li及各版主还有热心的兄弟们
内容如下：
1、oracle支持对日期进行运算
2、日期运算时是以天为单位进行的
3、当需要以分秒等更小的单位算值时，按时间进制进行转换即可
4、进行时间进制转换时注意加括号（见示例中红色括号），否则会出问题
SQL> alter session set nls_date_format='yyyy-mm-dd hh:mi:ss';
会话已更改。
SQL> set serverout on
SQL> declare
  2 DateValue date;
  3 begin
  4 select sysdate into DateValue from dual;
  5 dbms_output.put_line('源时间:'||to_char(DateValue));
  6 dbms_output.put_line('源时间减1天:'||to_char(DateValue-1));
  7 dbms_output.put_line('源时间减1天1小时:'||to_char(DateValue-1-1/24));
  8 dbms_output.put_line('源时间减1天1小时1分:'||to_char(DateValue-1-1/24-1/(24*60)));
  9 dbms_output.put_line('源时间减1天1小时1分1秒:'||to_char(DateValue-1-1/24-1/(24*60)-1/(24*60*6
0)));
10 end;
11 /
源时间:2003-12-29 11:53:41
源时间减1天:2003-12-28 11:53:41
源时间减1天1小时:2003-12-28 10:53:41
源时间减1天1小时1分:2003-12-28 10:52:41
源时间减1天1小时1分1秒:2003-12-28 10:52:40
PL/SQL 过程已成功完成。
[东方新秀](http://home.cnblogs.com/lizw/)
关注 - 0
粉丝 - 0

[关注博主]()

0

0
0

(请您对文章做出评价)

[«](http://www.cnblogs.com/lizw/archive/2007/04/15/714013.html)上一篇：[Ubuntu实践之一](http://www.cnblogs.com/lizw/archive/2007/04/15/714013.html "发布于2007-04-15 12:05")
[»](http://www.cnblogs.com/lizw/archive/2007/04/15/714338.html)下一篇：[使用.NET开发Web应用程序时，数据库访问类定义](http://www.cnblogs.com/lizw/archive/2007/04/15/714338.html "发布于2007-04-15 18:07")
posted on 2007-04-15 12:09 [东方新秀](http://www.cnblogs.com/lizw/) 阅读(14275) [评论(0)](http://www.cnblogs.com/lizw/archive/2007/04/15/714015.html#commentform)  [编辑](http://www.cnblogs.com/lizw/admin/EditPosts.aspx?postid=714015) [收藏](http://www.cnblogs.com/lizw/archive/2007/04/15/714015.html#)![]()

注册用户登录后才能发表评论，请 [登录](http://passport.cnblogs.com/login.aspx?ReturnUrl=http%3a%2f%2fwww.cnblogs.com%2flizw%2farchive%2f2007%2f04%2f15%2f714015.html%3flogin%3d1%23commentform) 或 [注册](http://passport.cnblogs.com/register.aspx?ReturnUrl=http%3a%2f%2fwww.cnblogs.com%2flizw%2farchive%2f2007%2f04%2f15%2f714015.html%23Bottom2) 。

[返回博客园首页](http://www.cnblogs.com/)

[有道难题开锣头奖8万！](http://a4.yeshj.com/rd/35182/)
[IT新闻](http://news.cnblogs.com/):
· [.中国域名艰难国际化背后：3千专家曾无一支持](http://news.cnblogs.com/n/64532/)
· [科技公司游说费用一览 苹果花费最少](http://news.cnblogs.com/n/64531/)
· [谷歌本周I/O大会推云存储服务Google Storage 挑战亚马逊S3](http://news.cnblogs.com/n/64530/)
· [Opera+各国国旗：粉丝制作图标秀](http://news.cnblogs.com/n/64529/)
· [微软起诉Salesforce 称侵犯其9项专利权](http://news.cnblogs.com/n/64528/)
[每天10分钟，轻松学英语](http://a4.yeshj.com/rd/34138/)  [沪江网校](http://a4.yeshj.com/rd/35140/)
[![]()](http://www.feifanit.com.cn/productFlow.htm)

[推荐职位](http://job.cnblogs.com/):

网站导航：
[博客园首页](http://www.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [个人主页](http://home.cnblogs.com/)  [闪存](http://home.cnblogs.com/ing/)  [程序员招聘](http://job.cnblogs.com/)  [社区](http://space.cnblogs.com/)  [博问](http://space.cnblogs.com/q/)
[![]()](http://www.china-pub.com/STATIC07/0912/zh_ndcx_091212.asp)
[China-pub 计算机图书网上专卖店！6.5万品种2-8折！](http://www.china-pub.com/itbook/)
[China-Pub 计算机绝版图书按需印刷服务](http://www.china-pub.com/static07/0901/zh_jueba_090121.asp)

**在知识库中查看：**
[ORACLE中日期和时间函数汇总(转载)](http://kb.cnblogs.com/a/714015/)
  [![]()](http://www.cnblogs.com/) Copyright © 东方新秀 Powered by: [博客园](http://www.cnblogs.com/) 模板提供：[沪江博客](http://blog.hjenglish.com/)
