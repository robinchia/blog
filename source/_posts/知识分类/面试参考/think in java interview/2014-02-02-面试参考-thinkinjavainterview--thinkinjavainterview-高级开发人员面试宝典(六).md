---
layout: post
title: "think in java interview-高级开发人员面试宝典(六)"
categories: 面试参考
tags: 
 - 面试参考
 - think in java interview
--- 

# think in java interview-高级开发人员面试宝典(六)

### [[置顶] think in java interview-高级开发人员面试宝典(六)]()

分类： [面经](http://blog.csdn.net/lifetragedy/article/category/1544093) 2013-08-12 22:12 1412人阅读 [评论]()(9) [收藏]( "收藏") [举报]( "举报")
目录[(?)]( "系统根据文章中H1到H6标签自动生成文章目录")[[+]]( "展开")

1. []()
1. [由4张简单的不能再简单的表演变出50道SQL]()
1. []()
1. [道问题开始]()

1. [查询001课程比002课程成绩高的所有学生的学号]()
1. [查询平均成绩大于60分的同学的学号和平均成绩]()
1. [查询所有同学的学号姓名选课数总成绩]()
1. [查询姓李的老师的个数]()
1. [查询没学过叶平老师课的同学的学号姓名]()
1. [查询学过001并且也学过编号002课程的同学的学号姓名]()
1. [查询学过叶平老师所教的所有课的同学的学号姓名]()
1. [查询课程编号002的成绩比课程编号001课程低的所有同学的学号姓名]()
1. [查询所有课程成绩小于60分的同学的学号姓名]()
1. [查询没有学全所有课的同学的学号姓名]()
1. [查询至少有一门课与学号为1001的同学所学相同的同学的学号和姓名]()
1. [查询至少学过学号为001同学所有一门课的其他同学学号和姓名]()
1. [把SC表中叶平老师教的课的成绩都更改为此课程的平均成绩]()
1. [查询和1002号的同学学习的课程完全相同的其他同学学号和姓名]()
1. [删除学习叶平老师课的SC表记录]()
1. [向SC表中插入一些记录这些记录要求符合以下条件没有上过编号003课程的同学学]()
1. [号2号课的平均成绩]()
1. [按平均成绩从高到低显示所有学生的数据库企业管理英语三门的课程成绩按]()
1. [如下形式显示 学生ID数据库企业管理英语有效课程数有效平均分]()
1. [查询各科成绩最高和最低的分以如下形式显示课程ID最高分最低分]()
1. [按各科平均成绩从低到高和及格率的百分数从高到低顺序]()
1. [查询如下课程平均成绩和及格率的百分数用1行显示]()
1. [企业管理001马克思002OOUML 003数据库004]()
1. [查询不同老师所教不同课程平均分从高到低显示]()
1. [查询如下课程成绩第 3 名到第 6 名的学生成绩单]()
1. [企业管理001马克思002UML 003数据库004]()
1. [学生ID学生姓名企业管理马克思UML数据库平均成绩]()
1. [统计列印各科成绩各分数段人数课程ID课程名称100-8585-7070-60 60]()
1. [查询学生平均成绩及其名次]()
1. [查询各科成绩前三名的记录不考虑成绩并列情况]()
1. [查询每门课程被选修的学生数]()
1. [查询出只选修了一门课程的全部学生的学号和姓名]()
1. [查询男生女生人数]()
1. [查询姓张的学生名单]()
1. [查询同名同性学生名单并统计同名人数]()
1. [年出生的学生名单注Student表中Sage列的类型是datetime]()
1. [查询每门课程的平均成绩结果按平均成绩升序排列平均成绩相同时按课程号降序排列]()
1. [查询平均成绩大于85的所有学生的学号姓名和平均成绩]()
1. [查询课程名称为数据库且分数低于60的学生姓名和分数]()
1. [查询所有学生的选课情况]()
1. [查询任何一门课程成绩在70分以上的姓名课程名称和分数]()
1. [查询不及格的课程并按课程号从大到小排列]()
1. [查询课程编号为003且课程成绩在80分以上的学生的学号和姓名]()
1. [求选了课程的学生人数]()
1. [查询选修叶平老师所授课程的学生中成绩最高的学生姓名及其成绩]()
1. [查询各个课程及相应的选修人数]()
1. [查询不同课程成绩相同的学生的学号课程号学生成绩]()
1. [查询每门功成绩最好的前两名]()
1. [统计每门课程的学生选修人数超过10人的课程才统计要求输出课程号和选修人数查询结果按人数降序排列查询结果按人数降序排列若人数相同按课程号升序排列]()
1. [检索至少选修两门课程的学生学号]()
1. [查询全部学生都选修的课程的课程号和课程名]()
1. [查询没学过叶平老师讲授的任一门课程的学生姓名]()
1. [查询两门以上不及格课程的同学的学号及其平均成绩]()
1. [检索004课程分数小于60按分数降序排列的同学学号]()
1. [删除002同学的001课程的成绩]()
1. []()

写了这么多JAVA基础，来点SQL吧！

一般面试时考SQL，主要就是考你“**统计分析**”这一块，下面我们来看面试官经常采用的手段。

# []()

# []()由4张简单的不能再简单的表，演变出50道SQL

哈哈哈哈，够这个面试官面个15，20个人，不带重复的了，而且每个SQL你真的不动动脑子还写不出呢，你别不服气，下面开始。

表结构：

**表Student**

(S#,Sname,Sage,Ssex) 学生表

**S#
** **student_no** Sage
 student_age Ssex
 student_sex

**表Course**

(C#,Cname,T#) 课程表

**C#
** **course_no** Cname course_name T# teacher_no

**表SC（学生与课程的分数mapping 表）**

(S#,C#,score) 成绩表

**S#** **student_no** C# course_no score 分数啦

**表Teacher**

(T#,Tname) 教师表

**T#** **teacher_no** Tname teacher_name

# []()

# []()50道问题开始

## []()**1**、查询“**001**”课程比“**002**”课程成绩高的所有学生的学号；

select a.S# from (select s#,score from SC where C#='001') a,(select s#,score

from SC where C#='002')

where a.score>b.score and a.s#=b.s#;

## []()**2**、查询平均成绩大于60分的同学的学号和平均成绩；

select S#,avg(score)

from sc

group by S# having avg(score) >**60**;

## []()**3**、查询所有同学的学号、姓名、选课数、总成绩；

select Student.S#,Student.Sname,count(SC.C#),sum(score)

from Student left Outer join SC on Student.S#=SC.S#

group by Student.S#,Sname

## []()**4**、查询姓“李”的老师的个数；

select count(distinct(Tname))

from Teacher

where Tname like '李%';

## []()**5**、查询没学过“叶平”老师课的同学的学号、姓名；

select Student.S#,Student.Sname

from Student

where S# not in (select distinct( SC.S#) fromSC,Course,Teacher where SC.C#=Course.C#and Teacher.T#=Course.T# andTeacher.Tname='叶平');

## []()**6**、查询学过“**001**”并且也学过编号“**002**”课程的同学的学号、姓名；

select Student.S#,Student.Sname fromStudent,SC where Student.S#=SC.S# andSC.C#='001'and exists( Select * from SC as SC_2 where SC_2.S#=SC.S# and SC_2.C#='002');

## []()**7**、查询学过“叶平”老师所教的所有课的同学的学号、姓名；

select S#,Sname

from Student

where S# in (select S# from SC,Course ,Teacher where SC.C#=Course.C# andTeacher.T#=Course.T# and Teacher.Tname='叶平'group by S# having count(SC.C#)=(select count(C#) fromCourse,Teacher whereTeacher.T#=Course.T# and Tname='叶平'));

## []()**8**、查询课程编号“**002**”的成绩比课程编号“**001**”课程低的所有同学的学号、姓名；

Select S#,Sname from (select Student.S#,Student.Sname,score ,(select score from SC SC_2 where SC_2.S#=Student.S#and SC_2.C#='002') score2

from Student,SC where Student.S#=SC.S# andC#='001') S_2 where score2 <score;

## []()
**9**、查询所有课程成绩小于60分的同学的学号、姓名；

select S#,Sname

from Student

where S# not in (select Student.S# fromStudent,SC where S.S#=SC.S# andscore>**60**);

## []()**10**、查询没有学全所有课的同学的学号、姓名；

select Student.S#,Student.Sname

from Student,SC
whereStudent.S#=SC.S# group by Student.S#,Student.Sname having count(C#) <(select count(C#) from Course);

## []()**11**、查询至少有一门课与学号为“**1001**”的同学所学相同的同学的学号和姓名；

select S#,Sname from Student,SC whereStudent.S#=SC.S# and C# in select C# from SC where S#='1001';

## []()**12**、查询至少学过学号为“**001**”同学所有一门课的其他同学学号和姓名；

select distinct SC.S#,Sname

from Student,SC

where Student.S#=SC.S# and C# in(select C# from SC where S#='001');

## []()**13**、把“SC”表中“叶平”老师教的课的成绩都更改为此课程的平均成绩；

update SC set score=(select avg(SC_2.score)

from SC SC_2

where SC_2.C#=SC.C# ) fromCourse,Teacher where Course.C#=SC.C# andCourse.T#=Teacher.T# and Teacher.Tname='叶平');

## []()**14**、查询和“**1002**”号的同学学习的课程完全相同的其他同学学号和姓名；

select S# from SC where C# in(select C# from SC where S#='1002')

group by S# having count(*)=(select count(*) from SC where S#='1002');

## []()**15**、删除学习“叶平”老师课的SC表记录；

DelectSC

from course ,Teacher

where Course.C#=SC.C# and Course.T#=Teacher.T# and Tname='叶平';

## []()**16**、向SC表中插入一些记录，这些记录要求符合以下条件：没有上过编号“**003**”课程的同学学

## []()号、**2**号课的平均成绩；

Insert SC select S#,'002',(Select avg(score)

from SC where C#='002') from Student where S# notin (Select S# from SC where C#='002');

## []()**17**、按平均成绩从高到低显示所有学生的“数据库”、“企业管理”、“英语”三门的课程成绩，按

## []()如下形式显示： 学生ID,,数据库,企业管理,英语,有效课程数,有效平均分

SELECT S# as 学生ID

,(SELECT score FROM SC WHERE SC.S#=t.S#AND C#='004') AS 数据库

,(SELECT score FROM SC WHERE SC.S#=t.S#AND C#='001') AS 企业管理

,(SELECT score FROM SC WHERE SC.S#=t.S#AND C#='006') AS 英语

,COUNT(*) AS 有效课程数, AVG(t.score) AS 平均成绩

FROM SC AS t

GROUP BY S#

ORDER BY avg(t.score)

## []()**18**、查询各科成绩最高和最低的分：以如下形式显示：课程ID，最高分，最低分

SELECT L.C# As 课程ID,L.score AS 最高分,R.score AS 最低分

FROM SC L ,SC AS R

WHERE L.C# = R.C# and

L.score = (SELECT MAX(IL.score)

FROM SC ASIL,Student AS IM

WHERE L.C# =IL.C# and IM.S#=IL.S#

GROUP BYIL.C#)

AND

R.Score= (SELECT MIN(IR.score)

FROM SC ASIR

WHERE R.C# =IR.C#

GROUP BY IR.C#

);

## []()**19**、按各科平均成绩从低到高和及格率的百分数从高到低顺序

SELECT t.C# AS 课程号,max(course.Cname)AS 课程名,isnull(AVG(score),**0**) AS平均成绩

,**100** * SUM(CASE WHEN isnull(score,**0**)>=**60** THEN **1** ELSE **0** END)/COUNT(*) AS 及格百分数

FROM SC T,Course

where t.C#=course.C#

GROUP BY t.C#

ORDER BY **100*** SUM(CASE WHEN isnull(score,**0**)>=**60** THEN **1** ELSE **0** END)/COUNT(*) DESC

## []()
20、查询如下课程平均成绩和及格率的百分数(用"1行"显示):

## []()
企业管理（001），马克思（002），OO&UML （003），数据库（004）

SELECT SUM(CASE WHEN C# ='001' THEN score ELSE **0** END)/SUM(CASE C# WHEN '001' THEN **1** ELSE **0** END) AS 企业管理平均分

,**100** * SUM(CASE WHEN C# = '001' AND score >= **60** THEN **1** ELSE **0** END)/SUM(CASE WHEN C# = '001' THEN **1** ELSE **0** END) AS 企业管理及格百分数

,SUM(CASE WHEN C# = '002' THEN score ELSE **0** END)/SUM(CASE C# WHEN '002' THEN **1** ELSE **0** END) AS 马克思平均分

,**100** * SUM(CASE WHEN C# = '002' AND score >= **60** THEN **1** ELSE **0** END)/SUM(CASE WHEN C# = '002' THEN **1** ELSE **0** END) AS 马克思及格百分数

,SUM(CASE WHEN C# = '003' THEN score ELSE **0** END)/SUM(CASE C# WHEN '003' THEN **1** ELSE **0** END) AS UML平均分

,**100*** SUM(CASE WHEN C# = '003' AND score >= **60** THEN **1** ELSE **0** END)/SUM(CASE WHEN C# = '003' THEN **1** ELSE **0** END) AS UML及格百分数

,SUM(CASE WHEN C# = '004' THEN score ELSE **0** END)/SUM(CASE C# WHEN '004' THEN **1** ELSE **0** END) AS 数据库平均分

,**100** * SUM(CASE WHEN C# = '004' AND score >= **60** THEN **1** ELSE **0** END)/SUM(CASE WHEN C# = '004' THEN **1** ELSE **0** END) AS 数据库及格百分数
FROM SC

## []()**21**、查询不同老师所教不同课程平均分从高到低显示

SELECT max(Z.T#) AS 教师ID,MAX(Z.Tname) AS 教师姓名,C.C# AS 课程ＩＤ,MAX(C.Cname) AS 课程名称,AVG(Score) AS 平均成绩

FROM SC AS T,Course AS C ,Teacher AS Z
where T.C#=C.C# and C.T#=Z.T#

GROUP BY C.C#
ORDER BY AVG(Score) DESC

## []()**22**、查询如下课程成绩第 **3** 名到第 **6** 名的学生成绩单：

## []()企业管理（**001**），马克思（**002**），UML （**003**），数据库（**004**）

## []()[学生ID],[学生姓名],企业管理,马克思,UML,数据库,平均成绩

SELECT DISTINCT top **3**

SC.S# As 学生学号,
Student.Sname AS 学生姓名 ,

T1.score AS 企业管理,
T2.score AS 马克思,

T3.score AS UML,
T4.score AS 数据库,

ISNULL(T1.score,**0**) + ISNULL(T2.score,**0**) + ISNULL(T3.score,**0**) + ISNULL(T4.score,**0**) as 总分
FROM Student,SC LEFT JOIN SC AS T1

ON SC.S# = T1.S# AND T1.C# = '001'
LEFT JOIN SC AS T2

ON SC.S# = T2.S# AND T2.C# = '002'
LEFT JOIN SC AS T3

ON SC.S# = T3.S# AND T3.C# = '003'
LEFT JOIN SC AS T4

ON SC.S# = T4.S# AND T4.C# = '004'
WHERE student.S#=SC.S# and

ISNULL(T1.score,**0**) + ISNULL(T2.score,**0**) + ISNULL(T3.score,**0**) + ISNULL(T4.score,**0**)
NOT IN

(SELECT
DISTINCT

TOP **15** WITH TIES
ISNULL(T1.score,**0**) + ISNULL(T2.score,**0**) + ISNULL(T3.score,**0**) + ISNULL(T4.score,**0**)

FROM sc
LEFT JOIN sc AS T1

ON sc.S# = T1.S# AND T1.C# = 'k1'
LEFT JOIN sc AS T2

ON sc.S# = T2.S# AND T2.C# = 'k2'
LEFT JOIN sc AS T3

ON sc.S# = T3.S# AND T3.C# = 'k3'
LEFT JOIN sc AS T4

ON sc.S# = T4.S# AND T4.C# = 'k4'
ORDER BY ISNULL(T1.score,**0**) + ISNULL(T2.score,**0**) + ISNULL(T3.score,**0**) + ISNULL(T4.score,**0**) DESC);

## []()**23**、统计列印各科成绩,各分数段人数:课程ID,课程名称,[100-85],[85-70],[70-60],[ <60]

SELECT SC.C# as 课程ID, Cname as 课程名称

,SUM(CASE WHEN score BETWEEN **85** AND **100** THEN **1** ELSE **0** END) AS [100 - 85]
,SUM(CASE WHEN score BETWEEN **70** AND **85** THEN **1** ELSE **0** END) AS [85 - 70]

,SUM(CASE WHEN score BETWEEN **60** AND **70** THEN **1** ELSE **0** END) AS [70 - 60]
,SUM(CASE WHEN score < **60** THEN **1** ELSE **0** END) AS [60 -]

FROM SC,Course
where SC.C#=Course.C#

GROUP BY SC.C#,Cname;

## []()**24**、查询学生平均成绩及其名次

SELECT **1**+(SELECT COUNT( distinct 平均成绩)

FROM (SELECT S#,AVG(score) AS 平均成绩
FROM SC

GROUP BY S#
) AS T1

WHERE 平均成绩 > T2.平均成绩) as 名次,
S# as 学生学号,平均成绩

FROM (SELECT S#,AVG(score) 平均成绩
FROM SC

GROUP BY S#
) AS T2

ORDER BY 平均成绩 desc;

## []()**25**、查询各科成绩前三名的记录:(不考虑成绩并列情况)

SELECT t1.S# as 学生ID,t1.C# as 课程ID,Score as 分数

FROM SC t1
WHERE score IN (SELECT TOP **3** score

FROM SC
WHERE t1.C#= C#

ORDER BY score DESC
)

ORDER BY t1.C#;

## []()**26**、查询每门课程被选修的学生数

select c#,count(S#) from sc group by C#;

## []()**27**、查询出只选修了一门课程的全部学生的学号和姓名

select SC.S#,Student.Sname,count(C#) AS 选课数

from SC ,Student
where SC.S#=Student.S# group by SC.S# ,Student.Sname having count(C#)=**1**;

## []()**28**、查询男生、女生人数

Select count(Ssex) as 男生人数 from Student group by Ssex having Ssex='男';

Select count(Ssex) as 女生人数 from Student group by Ssex having Ssex='女'；

## []()**29**、查询姓“张”的学生名单

SELECT Sname FROM Student WHERE Sname like '张%';

## []()**30**、查询同名同性学生名单，并统计同名人数

select Sname,count(*) from Student group by Sname having count(*)>**1**;

## []()**31**、1981年出生的学生名单(注：Student表中Sage列的类型是datetime)

select Sname, CONVERT(char (**11**),DATEPART(year,Sage)) as age

from student
where CONVERT(char(**11**),DATEPART(year,Sage))='1981';

## []()**32**、查询每门课程的平均成绩，结果按平均成绩升序排列，平均成绩相同时，按课程号降序排列

Select C#,Avg(score) from SC group by C# order by Avg(score),C# DESC ;

## []()
**33**、查询平均成绩大于85的所有学生的学号、姓名和平均成绩

select Sname,SC.S# ,avg(score)

from Student,SC
where Student.S#=SC.S# group by SC.S#,Sname having avg(score)>**85**;

## []()**34**、查询课程名称为“数据库”，且分数低于60的学生姓名和分数

Select Sname,isnull(score,**0**)

from Student,SC,Course
where SC.S#=Student.S# and SC.C#=Course.C# and Course.Cname='数据库'and score <**60**;

## []()**35**、查询所有学生的选课情况；

SELECT SC.S#,SC.C#,Sname,Cname

FROM SC,Student,Course
where SC.S#=Student.S# and SC.C#=Course.C# ;

## []()**36**、查询任何一门课程成绩在70分以上的姓名、课程名称和分数；

SELECT distinct student.S#,student.Sname,SC.C#,SC.score

FROM student,Sc
WHERE SC.score>=**70** AND SC.S#=student.S#;

## []()**37**、查询不及格的课程，并按课程号从大到小排列

select c# from sc where scor e <**60** order by C# ;

## []()**38**、查询课程编号为003且课程成绩在80分以上的学生的学号和姓名；

select SC.S#,Student.Sname from SC,Student where SC.S#=Student.S# and Score>**80** and C#='003';

## []()**39**、求选了课程的学生人数

select count(*) from sc;
## []()**40**、查询选修“叶平”老师所授课程的学生中，成绩最高的学生姓名及其成绩

select Student.Sname,score

from Student,SC,Course C,Teacher
where Student.S#=SC.S# and SC.C#=C.C# and C.T#=Teacher.T# and Teacher.Tname='叶平' and SC.score=(select max(score)from SC where C#=C.C# );

## []()**41**、查询各个课程及相应的选修人数

select count(*) from sc group by C#;

## []()**42**、查询不同课程成绩相同的学生的学号、课程号、学生成绩

select distinct A.S#,B.score from SC A ,SC B where A.Score=B.Score and A.C# <>B.C# ;

## []()**43**、查询每门功成绩最好的前两名

SELECT t1.S# as 学生ID,t1.C# as 课程ID,Score as 分数

FROM SC t1
WHERE score IN (SELECT TOP **2** score

FROM SC
WHERE t1.C#= C#

ORDER BY score DESC
)

ORDER BY t1.C#;

## []()**44**、统计每门课程的学生选修人数（超过10人的课程才统计）。要求输出课程号和选修人数，查询结果按人数降序排列，查询结果按人数降序排列，若人数相同，按课程号升序排列

select C# as 课程号,count(*) as 人数

from sc
group by C#

order by count(*) desc,c#

## []()**45**、检索至少选修两门课程的学生学号

select S#

from sc
group by s#

having count(*) > = **2**

## []()**46**、查询全部学生都选修的课程的课程号和课程名

select C#,Cname

from Course
where C# in (select c# from sc group by c#)

## []()**47**、查询没学过“叶平”老师讲授的任一门课程的学生姓名

select Sname from Student where S# not in (select S# from Course,Teacher,SC where Course.T#=Teacher.T# and SC.C#=course.C# and Tname='叶平');

## []()**48**、查询两门以上不及格课程的同学的学号及其平均成绩

select S#,avg(isnull(score,**0**)) from SC where S# in (select S# from SC where score <**60** group by S# having count(*)>**2**)group by S#;

## []()**49**、检索“**004**”课程分数小于60，按分数降序排列的同学学号

select S# from SC where C#='004'and score <**60** order by score desc;

## []()**50**、删除“**002**”同学的“**001**”课程的成绩

delete from Sc where S#='001'and C#='001';

## []()
分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")

1. 上一篇：[think in java interview-高级开发人员面试宝典(五)](http://blog.csdn.net/lifetragedy/article/details/9878839)
1. 下一篇：[think in java interview-高级开发人员面试宝典(七)](http://blog.csdn.net/lifetragedy/article/details/10305735)
顶11 踩1

查看评论[]()
4楼 [jones_ios](http://blog.csdn.net/jones_ios) 2013-08-20 10:25发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/jones_ios)写得真好，通俗易懂。支持一下，哈哈 3楼 [mahuxiaozi123](http://blog.csdn.net/mahuxiaozi123) 2013-08-15 10:16发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/mahuxiaozi123)受教了，顶一个！ 2楼 [英子DD](http://blog.csdn.net/yingzidd) 2013-08-13 15:26发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/yingzidd)文章写得真好，或许可以考虑出版，以惠及更多读者？ Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-14 09:14发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复yingzidd：有得出书挣取读者们微薄的稿费，还不如自己多写博客，让更多读者们可以免费看到呢！商人啊 ，呵呵！ Re: [水哥709](http://blog.csdn.net/leon709) 2013-08-16 09:56发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/leon709)回复lifetragedy：我觉得CSDN应该给你这种比较火的博客发点奖励金。 Re: [Fly_fans](http://blog.csdn.net/Fly_fans) 6天前 15:20发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/Fly_fans)回复leon709：顶一个！ Re: [英子DD](http://blog.csdn.net/yingzidd) 2013-08-14 11:15发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/yingzidd)回复lifetragedy：您真是有大爱的人！但是博客的读者不一定比书多，而且书有一定的系统要求，对于读者来说更易读和理解，传播的范围更广！我们也不算真正的商人，还有很多社会责任。。。 1楼 [wuming5920](http://blog.csdn.net/wm5920) 2013-08-13 10:28发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/wm5920)党和人民以及无数的码农会记得lz的无私奉献滴 Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-13 12:31发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复wm5920：one for all, all for one！

您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Flifetragedy%2Farticle%2Fdetails%2F9935699)
* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()

[![TOP]()]( "回到顶部")
