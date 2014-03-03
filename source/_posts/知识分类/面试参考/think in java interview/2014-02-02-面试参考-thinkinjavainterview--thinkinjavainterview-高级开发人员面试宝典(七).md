---
layout: post
title: "think in java interview-高级开发人员面试宝典(七)"
categories: 面试参考
tags: 
 - 面试参考
 - think in java interview
--- 

# think in java interview-高级开发人员面试宝典(七)

### [[置顶] think in java interview-高级开发人员面试宝典(七)]()

分类： [面经](http://blog.csdn.net/lifetragedy/article/category/1544093) 2013-08-25 17:42 823人阅读 [评论]()(2) [收藏]( "收藏") [举报]( "举报")
上两周研发任务太紧了，所以担搁了一下，我们继续我们的面试之旅。

下面是一个基于图书系统的15道SQL问答，供大家参考

问题描述：
本题用到下面三个关系表：
CARD 借书卡。 CNO 卡号，NAME 姓名，CLASS 班级
BOOKS 图书。 BNO 书号，BNAME 书名,AUTHOR 作者，PRICE 单价，QUANTITY 库存册数
BORROW 借书记录。 CNO 借书卡号，BNO 书号，RDATE 还书日期
备注：限定每人每种书只能借一本；库存册数随借书、还书而改变。
要求实现如下15个处理：
1． 写出建立BORROW表的SQL语句，要求定义主码完整性约束和引用完整性约束。
2． 找出借书超过5本的读者,输出借书卡号及所借图书册数。
3． 查询借阅了"水浒"一书的读者，输出姓名及班级。
4． 查询过期未还图书，输出借阅者（卡号）、书号及还书日期。
5． 查询书名包括"网络"关键词的图书，输出书号、书名、作者。
6． 查询现有图书中价格最高的图书，输出书名及作者。
7． 查询当前借了"计算方法"但没有借"计算方法习题集"的读者，输出其借书卡号，并按卡号降序排序输出。
8． 将"C01"班同学所借图书的还期都延长一周。
9． 从BOOKS表中删除当前无人借阅的图书记录。
10．如果经常按书名查询图书信息，请建立合适的索引。
11．在BORROW表上建立一个触发器，完成如下功能：如果读者借阅的书名是"数据库技术及应用"，就将该读者的借阅记录保存在BORROW_SAVE表中（注ORROW_SAVE表结构同BORROW表）。
12．建立一个视图，显示"力01"班学生的借书信息（只要求显示姓名和书名）。
13．查询当前同时借有"计算方法"和"组合数学"两本书的读者，输出其借书卡号，并按卡号升序排序输出。
14．假定在建BOOKS表时没有定义主码，写出为BOOKS表追加定义主码的语句。
15．对CARD表做如下修改：
a. 将NAME最大列宽增加到10个字符（假定原为6个字符）。
b. 为该表增加1列NAME（系名），可变长，最大20个字符。

**1**. 写出建立BORROW表的SQL语句，要求定义主码完整性约束和引用完整性约束

--实现代码：
CREATE TABLE BORROW(

CNO int FOREIGN KEY REFERENCES CARD(CNO),
BNO int FOREIGN KEY REFERENCES BOOKS(BNO),

RDATE datetime,
PRIMARY KEY(CNO,BNO))

**2**. 找出借书超过5本的读者,输出借书卡号及所借图书册数
--实现代码：

SELECT CNO,借图书册数=COUNT(*)
FROM BORROW

GROUP BY CNO
HAVING COUNT(*)>**5**

**3**. 查询借阅了"水浒"一书的读者，输出姓名及班级
--实现代码：

SELECT * FROM CARD c
WHERE EXISTS(

SELECT * FROM BORROW a,BOOKS b
WHERE a.BNO=b.BNO

AND b.BNAME=N'水浒'
AND a.CNO=c.CNO)

**4**. 查询过期未还图书，输出借阅者（卡号）、书号及还书日期
--实现代码：

SELECT * FROM BORROW
WHERE RDATE<GETDATE()

**5**. 查询书名包括"网络"关键词的图书，输出书号、书名、作者
--实现代码：

SELECT BNO,BNAME,AUTHOR FROM BOOKS
WHERE BNAME LIKE N'%网络%'

**6**. 查询现有图书中价格最高的图书，输出书名及作者
--实现代码：

SELECT BNO,BNAME,AUTHOR FROM BOOKS
WHERE PRICE=(

SELECT MAX(PRICE) FROM BOOKS)
**7**. 查询当前借了"计算方法"但没有借"计算方法习题集"的读者，输出其借书卡号，并按卡号降序排序输出

--实现代码：
SELECT a.CNO

FROM BORROW a,BOOKS b
WHERE a.BNO=b.BNO AND b.BNAME=N'计算方法'

AND NOT EXISTS(
SELECT * FROM BORROW aa,BOOKS bb

WHERE aa.BNO=bb.BNO
AND bb.BNAME=N'计算方法习题集'

AND aa.CNO=a.CNO)
ORDER BY a.CNO DESC

**8**. 将"C01"班同学所借图书的还期都延长一周
--实现代码：

UPDATE b SET RDATE=DATEADD(Day,**7**,b.RDATE)
FROM CARD a,BORROW b

WHERE a.CNO=b.CNO
AND a.CLASS=N'C01'

**9**. 从BOOKS表中删除当前无人借阅的图书记录
--实现代码：

DELETE A FROM BOOKS a
WHERE NOT EXISTS(

SELECT * FROM BORROW
WHERE BNO=a.BNO)

**10**. 如果经常按书名查询图书信息，请建立合适的索引
--实现代码：

CREATE CLUSTERED INDEX IDX_BOOKS_BNAME ON BOOKS(BNAME)
**11**. 在BORROW表上建立一个触发器，完成如下功能：如果读者借阅的书名是"数据库技术及应用"，就将该读者的借阅记录保存在BORROW_SAVE表中（注ORROW_SAVE表结构同BORROW表）

--实现代码：
CREATE TRIGGER TR_SAVE ON BORROW

FOR INSERT,UPDATE
AS

IF **@@ROWCOUNT**>**0**
INSERT BORROW_SAVE SELECT i.*

FROM INSERTED i,BOOKS b
WHERE i.BNO=b.BNO

AND b.BNAME=N'数据库技术及应用'
**12**. 建立一个视图，显示"力01"班学生的借书信息（只要求显示姓名和书名）

--实现代码：
CREATE VIEW V_VIEW

AS
SELECT a.NAME,b.BNAME

FROM BORROW ab,CARD a,BOOKS b
WHERE ab.CNO=a.CNO

AND ab.BNO=b.BNO
AND a.CLASS=N'力01'

**13**. 查询当前同时借有"计算方法"和"组合数学"两本书的读者，输出其借书卡号，并按卡号升序排序输出
--实现代码：

SELECT a.CNO
FROM BORROW a,BOOKS b

WHERE a.BNO=b.BNO
AND b.BNAME IN(N'计算方法',N'组合数学')

GROUP BY a.CNO
HAVING COUNT(*)=**2**

ORDER BY a.CNO DESC
**14**. 假定在建BOOKS表时没有定义主码，写出为BOOKS表追加定义主码的语句

--实现代码：
ALTER TABLE BOOKS ADD PRIMARY KEY(BNO)

**15.1** 将NAME最大列宽增加到10个字符（假定原为6个字符）
--实现代码：

ALTER TABLE CARD ALTER COLUMN NAME varchar(**10**)
**15.2** 为该表增加1列NAME（系名），可变长，最大20个字符

--实现代码：
ALTER TABLE CARD ADD 系名 varchar(**20**)

问题描述：
为管理岗位业务培训信息，建立3个表:

S (S#,SN,SD,SA) S#,SN,SD,SA 分别代表学号、学员姓名、所属单位、学员年龄
C (C#,CN ) C#,CN 分别代表课程编号、课程名称SC ( S#,C#,G ) S#,C#,G 分别代表学号、所选修的课程编号、学习成绩

分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")
1. 上一篇：[think in java interview-高级开发人员面试宝典(六)](http://blog.csdn.net/lifetragedy/article/details/9935699)
1. 下一篇：[think in java interview-高级开发人员面试宝典(八)](http://blog.csdn.net/lifetragedy/article/details/10363489)

顶3 踩0

查看评论[]()
2楼 [LI597494570](http://blog.csdn.net/LI597494570) 3天前 10:29发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/LI597494570)你这里面 第3个SQL语句“查询借阅了"水浒"一书的读者，输出姓名及班级” 我在SQL SERVER2008上 试了下 不成功。。。说是第2个SELECT 附近 语法不对。。。 1楼 [lamplk](http://blog.csdn.net/lamplk) 6天前 15:37发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lamplk)楼主，关注您的博客有很长一段时间了，从中受益匪浅，本文第10点说到， 如果经常按书名查询图书信息，请建立合适的索引，我有一些东西想要请教，我对聚簇索引的理解是它不属于索引的组织形式，而是表的组织形式，多用于表之间的连接字段，而且建立聚簇索引的空间消耗比较大，对以后的增删操作影响也比较大，为什么不考虑用非聚簇索引呢？您能不能详细说说这两者各自的特点和应用场景呢？

您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Flifetragedy%2Farticle%2Fdetails%2F10305735)
* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()

[![TOP]()]( "回到顶部")
