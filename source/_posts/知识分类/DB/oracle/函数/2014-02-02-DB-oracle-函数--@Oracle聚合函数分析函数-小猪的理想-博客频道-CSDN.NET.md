---
layout: post
title: "@Oracle聚合函数 分析函数 - 小猪的理想 - 博客频道 - CSDN.NET"
categories: DB
tags: 
 - DB
 - oracle
 - 函数
--- 

# @Oracle聚合函数 分析函数 - 小猪的理想 - 博客频道 - CSDN.NET

您还未登录！|[登录](http://passport.csdn.net/UserLogin.aspx)|[注册](http://passport.csdn.net/CSDNUserRegister.aspx)|[帮助](http://passport.csdn.net/help/faq)

* [CSDN首页](http://www.csdn.net/)
* [资讯](http://news.csdn.net/)
* [论坛](http://bbs.csdn.net/)
* [博客](http://blog.csdn.net/)
* [下载](http://download.csdn.net/)
* [搜索](http://so.csdn.net/)
* 
## [更多]()

* [CTO俱乐部](http://cto.csdn.net/)
* [学生大本营](http://student.csdn.net/)
* [培训充电](http://edu.csdn.net/)
* [移动开发](http://mobile.csdn.net/)
* [软件研发](http://sd.csdn.net/)
* [云计算](http://cloud.csdn.net/)
* [程序员](http://www.programmer.com.cn/)
* [TUP](http://tup.csdn.net/)

# [小猪的理想](http://blog.csdn.net/nshk)

## 桃李春风一杯酒，江湖夜雨十年灯。

* [![]()目录视图](http://blog.csdn.net/nshk?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/nshk?viewmode=list)
* [![]()订阅](http://blog.csdn.net/nshk/rss/list)
[2012年4月微软MVP申请开始！](http://blog.csdn.net/blogdevteam/article/details/7161925)                  [点击了解英特尔云计算](http://ad.doubleclick.net/click;h=v2%7C3E84%7C0%7C0%7C%2a%7Cb;247908458;0-0;0;73605427;31-1%7C1;44759227%7C44777015%7C1;;%3fhttp://www.ithaowai.com/cloud?utm_source=CSDN&utm_medium=CSDN_HomePage_news_picture_g01&utm_campaign=CN2011Q4BIZ_Xeon_IT_Center_Promotion)            [2012年1月当选微软MVP的CSDN会员名单揭晓！](http://blog.csdn.net/blogdevteam/article/details/7177569)

### [@Oracle聚合函数/分析函数]( "@Oracle聚合函数/分析函数")

2008-03-21 10:44 2445人阅读 [评论](http://blog.csdn.net/nshk/article/details/2202088#comments)(2) [收藏]( "收藏") [举报](http://blog.csdn.net/nshk/article/details/2202088#report "举报")
参考文献： 《expert one-on-one》、《Oracle 9i reference》
oracle函数分两类：单行函数、多行函数 。多行函数也成为聚合函数、组合函数，参数为数组，数据大小为记录数，这种数组不是普通高级语言的数组，是一种虚拟数组，当记录数大时，会将数据写入硬盘，内存中放的只是影像。
oracle从8.1.6开始提供分析函数，用于计算基于组的某种聚合值。它和聚合函数的不同之处在于每个组返回多行，聚合函数每个组只返回一行。
开窗函数：指定了分析函数工作的数据窗口大小，这个数据大小会随数据行数变化而变化，示例如下：
over（order by salary） 按照salary排序进行累计，order by是个默认的开窗函数
over（partition by deptno）按照部门分区
over（order by salary range between 50 preceding and 150 following）每行对应的数据窗口是之前行幅度值不超过50，之后行幅度值不超过150
over（order by salary rows between 50 preceding and 150 following）每行对应的数据窗口是之前50行，之后150行
over（order by salary rows between unbounded preceding and unbounded following）每行对应的数据窗口是从第一行到最后一行，等效：over（order by salary range between unbounded preceding and unbounded following）
AVG功能描述，用于计算一个组和数据窗口内表达式的平均值。
 sample:select avg(salary) over(partition by manager_id order by hire_date rows between 1 preceding and 1 following) as avg_salary from employee.
CORR，返回一对表达式的相关系数。缩写如下：
COVAR_POP(expr1,expr2)/STDDEV_POP(expr1)*STDDEV_POP(expr2))，从统计上讲，相关性是变量之间关联的强度，变量之间的关联意味着一定程度上一个变量的值可以由其他变量值进行预测，返回一个-1~1的数，相关系数给出了关联的强度，0表示不相关。
COVAR_POP，返回一对表达式的总体协方差。
COVAR_SAMP，返回一对表达式的样本协方差。
COUNT，对组内发生的事情进行累计。如果指定*或一些非空常数，count将对所有行计数，如果指定一个表达式，count返回表达式非空赋值的计数，当有相同值出现时，这些相等的值都会被纳入被计算的值；可以使用DISTINCT来记录去掉一组中完全相同的数据后出现的行数。
CUME_DIST，计算一行在组内的相对位置。CUME_DIST总是返回大于0、小于或等于1的数，该数表示该行在N行中的位置。
DENSE_RANK，根据ORDER BY子句中表达式的值，从查询返回的每一行，计算它们与其它行的相对位置。组内的数据按ORDER BY子句排序，然后给每一行赋一个号，从而形成一个序列，该序列从1开始，往后累加。每次ORDER BY表达式的值发生变化时，该序列也随之增加。有同样值的行得到同样的数字序号（认为null时相等的）。密集的序列返回的时没有间隔的数。
FIRST，从DENSE_RANK返回的集合中取出排在最前面的一个值的行（可能多行，因为值可能相等），因此完整的语法需要在开始处加上一个集合函数以从中取出记录。
FIRST_VALUE，返回组中数据窗口的第一个值。
LAG，可以访问结果集中的其它行而不用进行自连接。它允许去处理游标，就好像游标是一个数组一样。在给定组中可参考当前行之前的 行，这样就可以从组中与当前行一起选择以前的行。Offset是一个正整数，其默认值为1，若索引超出窗口的范围，就返回默认值（默认返回的是组中第一 行），其相反的函数是LEAD
LAST，从DENSE_RANK返回的集合中取出排在最后面的一个值的行（可能多行，因为值可能相等），因此完整的语法需要在开始处加上一个集合函数以从中取出记录。
LAST_VALUE，返回组中数据窗口的最后一个值。
LEAD，LEAD与LAG相反，LEAD可以访问组中当前行之后的行。Offset是一个正整数，其默认值为1，若索引超出窗口的范围，就返回默认值（默认返回的是组中第一行）。
MAX，在一个组中的数据窗口中查找表达式的最大值。
MIN，在一个组中的数据窗口中查找表达式的最小值。
NTILE，将一个组分为"表达式"的散列表示，例如，如果表达式=4，则给组中的每一行分配一个数（从1到4），如果组中有20行， 则给前5行分配1，给下5行分配2等等。如果组的基数不能由表达式值平均分开，则对这些行进行分配时，组中就没有任何percentile的行数比其它 percentile的行数超过一行，最低的percentile是那些拥有额外行的percentile。例如，若表达式=4，行数=21，则 percentile=1的有5行，percentile=2的有5行等等。
PERCENT_RANK，和CUME_DIST（累积分配）函数类似，对于一个组中给定的行来说，在计算那行的序号时，先减1，然后除以n-1（n为组中所有的行数）。该函数总是返回0～1（包括1）之间的数。
PERCENTILE_RANK，返回一个与输入的分布百分比值相对应的数据值，分布百分比的计算方法见函数PERCENT_RANK，如果没有正好对应的数据值，就通过下面算法来得到值：
RN = 1+ (P*(N-1)) 其中P是输入的分布百分比值，N是组内的行数
CRN = CEIL(RN) FRN = FLOOR(RN)
if (CRN = FRN = RN) then
(value of expression from row at RN)
else
(CRN - RN) * (value of expression for row at FRN) +
(RN - FRN) * (value of expression for row at CRN)
注意：本函数与PERCENTILE_DISC的区别在找不到对应的分布值时返回的替代值的计算方法不同。
PERCENTILE_DISC，返回一个与输入的分布百分比值相对应的数据值，分布百分比的计算方法见函数CUME_DIST，如果没有正好对应的数据值，就取大于该分布值的下一个值。
注意：本函数与PERCENTILE_CONT的区别在找不到对应的分布值时返回的替代值的计算方法不同。
RANK，根据ORDER BY子句中表达式的值，从查询返回的每一行，计算它们与其它行的相对位置。组内的数据按ORDER BY子句排序，然后给每一行赋一个号，从而形成一个序列，该序列从1开始，往后累加。每次ORDER BY表达式的值发生变化时，该序列也随之增加。有同样值的行得到同样的数字序号（认为null时相等的）。然而，如果两行的确得到同样的排序，则序数将随 后跳跃。若两行序数为1，则没有序数2，序列将给组中的下一行分配值3，DENSE_RANK则没有任何跳跃。
RATIO_TO_REPORT，该函数计算expression/(sum(expression))的值，它给出相对于总数的百分比，即当前行对sum(expression)的贡献。
REGR_ (Linear Regression) Functions
功能描述：这些线性回归函数适合最小二乘法回归线，有9个不同的回归函数可使用。
REGR_SLOPE：返回斜率，等于COVAR_POP(expr1, expr2) / VAR_POP(expr2)
REGR_INTERCEPT：返回回归线的y截距，等于
AVG(expr1) - REGR_SLOPE(expr1, expr2) * AVG(expr2)
REGR_COUNT：返回用于填充回归线的非空数字对的数目
REGR_R2：返回回归线的决定系数，计算式为：
If VAR_POP(expr2) = 0 then return NULL
If VAR_POP(expr1) = 0 and VAR_POP(expr2) != 0 then return 1
If VAR_POP(expr1) > 0 and VAR_POP(expr2 != 0 then
return POWER(CORR(expr1,expr),2)
REGR_AVGX：计算回归线的自变量(expr2)的平均值，去掉了空对(expr1, expr2)后，等于AVG(expr2)
REGR_AVGY：计算回归线的应变量(expr1)的平均值，去掉了空对(expr1, expr2)后，等于AVG(expr1)
REGR_SXX： 返回值等于REGR_COUNT(expr1, expr2) * VAR_POP(expr2)
REGR_SYY： 返回值等于REGR_COUNT(expr1, expr2) * VAR_POP(expr1)
REGR_SXY: 返回值等于REGR_COUNT(expr1, expr2) * COVAR_POP(expr1, expr2)
ROW_NUMBER，返回有序组中一行的偏移量，从而可用于按特定标准排序的行号。
STDDEV ，计算当前行关于组的标准偏离（Standard Deviation）。
STDDEV_POP，该函数计算总体标准偏离，并返回总体变量的平方根，其返回值与VAR_POP函数的平方根相同（Standard Deviation－Population）。
STDDEV_SAMP，该函数计算累积样本标准偏离，并返回总体变量的平方根，其返回值与VAR_POP函数的平方根相同（Standard Deviation－Sample）。
SUM，该函数计算组中表达式的累积和。
VAR_POP，（Variance Population）该函数返回非空集合的总体变量（忽略null），VAR_POP进行如下计算：
(SUM(expr2) - SUM(expr2) / COUNT(expr)) / COUNT(expr)。
VAR_SAMP，（Variance Sample）该函数返回非空集合的样本变量（忽略null），VAR_POP进行如下计算：
(SUM(expr*expr)-SUM(expr)*SUM(expr)/COUNT(expr))/(COUNT(expr)-1)。
VARIANCE，该函数返回表达式的变量，Oracle计算该变量如下：
如果表达式中行数为1，则返回0
如果表达式中行数大于1，则返回VAR_SAMP
3月28日补充，关于rollup 和 cube：
group by 语句在基本语法外，还支持rollup 和 cube语句。
ROLLUP(A, B, C)，首先会对(A、B、C)进行GROUP BY，然后对(A、B)进行GROUP BY，然后是(A)进行GROUP BY，最后对全表进行GROUP BY操作。
GROUP BY CUBE(A, B, C)，则首先会对(A、B、C)进行GROUP BY，然后依次是(A、B)，(A、C)，(A)，(B、C)，(B)，(C)，最后对全表进行GROUP BY操作。
GROUPING_ID()可以美化一下效果。

分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")
* 上一篇：[----单词：pursuit、canonical、两会](http://blog.csdn.net/nshk/article/details/2190862)
* 下一篇：[----工作流引擎几种实现 -- Work Flow Application 4](http://blog.csdn.net/nshk/article/details/2217077)
查看评论[]()

2楼 [tianlincao](http://blog.csdn.net/tianlincao) 2010-07-01 16:58发表 [[回复]](http://blog.csdn.net/nshk/article/details/2202088#reply "回复")  [[引用]](http://blog.csdn.net/nshk/article/details/2202088#quote "引用") [[举报]](http://blog.csdn.net/nshk/article/details/2202088#report "举报")[![]()](http://blog.csdn.net/tianlincao)真的写的很好![]()1楼 [kid_ren](http://blog.csdn.net/kid_ren) 2010-01-29 10:56发表 [[回复]](http://blog.csdn.net/nshk/article/details/2202088#reply "回复")  [[引用]](http://blog.csdn.net/nshk/article/details/2202088#quote "引用") [[举报]](http://blog.csdn.net/nshk/article/details/2202088#report "举报")[![]()](http://blog.csdn.net/kid_ren)写的太好了![]()
您还没有登录,请[[登录]](http://passport.csdn.net/account/login?from=http%3A%2F%2Fblog.csdn.net%2Fnshk%2Farticle%2Fdetails%2F2202088)或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fnshk%2Farticle%2Fdetails%2F2202088)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()

个人资料

[![]()](http://blog.csdn.net/nshk "我的博客主页")
nshk

* 访问：4829次
* 积分：180分
* 排名：千里之外

* 原创：11篇
* 转载：8篇
* 译文：0篇
* 评论：2条

文章搜索

[]()
文章存档

* [2008年04月](http://blog.csdn.net/nshk/article/month/2008/04)(10)
* [2008年03月](http://blog.csdn.net/nshk/article/month/2008/03)(9)

阅读排行

* [@Oracle聚合函数/分析函数]( "@Oracle聚合函数/分析函数") (2444)
* [@FaceBook UML图示说明](http://blog.csdn.net/nshk/article/details/2219059 "@FaceBook UML图示说明") (284)
* [----工作流引擎几种实现 -- Wor...](http://blog.csdn.net/nshk/article/details/2217077 "----工作流引擎几种实现 -- Work Flow Application 4") (246)
* [XML详解----Schema](http://blog.csdn.net/nshk/article/details/2343124 "XML详解----Schema") (189)
* [XML Schema 与 XML DTD...](http://blog.csdn.net/nshk/article/details/2343109 "XML Schema 与 XML DTD的技术比较与分析    ") (181)
* [spring框架简介（笔记）](http://blog.csdn.net/nshk/article/details/2223007 "spring框架简介（笔记）") (170)
* [@批处理命令](http://blog.csdn.net/nshk/article/details/2223369 "@批处理命令") (168)
* [@温故知新 之 java时间类](http://blog.csdn.net/nshk/article/details/2327420 "@温故知新 之 java时间类") (149)
* [@google自动翻译的实现](http://blog.csdn.net/nshk/article/details/2218660 "@google自动翻译的实现") (109)
* [@Java反射--侯捷](http://blog.csdn.net/nshk/article/details/2315568 "@Java反射--侯捷") (100)
评论排行

* [@Oracle聚合函数/分析函数]( "@Oracle聚合函数/分析函数") (2)
* [----单词：pursuit、canon...](http://blog.csdn.net/nshk/article/details/2190862 "----单词：pursuit、canonical、两会") (0)
* [----GoF 设计模式概要](http://blog.csdn.net/nshk/article/details/2327388 "----GoF 设计模式概要") (0)
* [@温故知新 之 java时间类](http://blog.csdn.net/nshk/article/details/2327420 "@温故知新 之 java时间类") (0)
* [@en java](http://blog.csdn.net/nshk/article/details/2327547 "@en java") (0)
* [----js牛文](http://blog.csdn.net/nshk/article/details/2331115 "----js牛文") (0)
* [IPO](http://blog.csdn.net/nshk/article/details/2334521 "IPO") (0)
* [温故知新之 解码XML和DTD](http://blog.csdn.net/nshk/article/details/2334891 "温故知新之 解码XML和DTD") (0)
* [XML Schema 与 XML DTD...](http://blog.csdn.net/nshk/article/details/2343109 "XML Schema 与 XML DTD的技术比较与分析    ") (0)
* [@Java反射--侯捷](http://blog.csdn.net/nshk/article/details/2315568 "@Java反射--侯捷") (0)

推荐文章
最新评论

* [@Oracle聚合函数/分析函数](http://blog.csdn.net/nshk/article/details/2202088#comments)

tianlincao: 真的写的很好
* [@Oracle聚合函数/分析函数](http://blog.csdn.net/nshk/article/details/2202088#comments)

kid_ren: 写的太好了
    ![]() ![]()

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)北京创新乐知信息技术有限公司 版权所有, 京 ICP 证 070598 号世纪乐知(北京)网络技术有限公司 提供技术支持江苏乐知网络技术有限公司 提供商务支持![]() Email:webmaster@csdn.netCopyright © 1999-2011, CSDN.NET, All Rights Reserved[![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
