---
layout: post
title: "MySQL命令行 不同端口登录 执行SQL文件 创建用户 赋予权限 修改root密码 - Love "
categories: DB
tags: 
 - DB
 - mysql
--- 

# MySQL命令行 不同端口登录 执行SQL文件 创建用户 赋予权限 修改root密码 - Love in coding___ - 博客园

[Love in coding...](http://www.cnblogs.com/freeliver54/)

**    Free and Susan**

## [MySQL命令行 不同端口登录 执行SQL文件 创建用户 赋予权限 修改root密码](http://www.cnblogs.com/freeliver54/archive/2009/07/25/1530970.html)

0.安装MySQL服务

1.[不同端口登录]

  通过开始菜单-> 程序-> MySQL-> MySQL Command Line Client
  通过输入密码Enter password:******进行登录 该MySQL服务端口已定

  或者

  通过运行命令->C:\Program Files\MySQL\MySQL Server 5.2\bin>
  然后利用mysql进行登录
  C:\Program Files\MySQL\MySQL Server 5.2\bin>mysql -uroot -h127.0.0.1 -P3307 -p
  Enter password:******
  亦可选择登录远程MySQL

2.[执行SQL文件]

  创建测试数据库及数据表
  mysql> source c:/test/test.sql;

  test.sql文件内容：
  create database IF NOT EXISTS testdb;

  use testdb;

  create table testtable
         (testName varchar(10));

3.[创建用户]

  mysql> create user testuser identified by 'testpass';

4.[赋予权限]

  mysql> grant all privileges on testdb.* to 'testuser'@'%' identified by 'testpass';

5.[修改root密码](转自网络)

  如果已登录
  1)use mysql 
  2)update user set password=password('你的密码') where user='root';
  3)flush privileges;

  如果没登录，你想进数据库而没有密码

  先关掉服务
  然后以safe模式进入
  mysqld_safe --skip-grant-tables &
  这个窗口不要关，这样进数据库就不用密码，进去后 做 “如果已登录”的 步骤
0

0
0

(请您对文章做出评价)

[«](http://www.cnblogs.com/freeliver54/archive/2009/07/24/1530146.html)上一篇：[婚姻 一辈子的幸福厮守 请不要多拿彩礼和父母说事](http://www.cnblogs.com/freeliver54/archive/2009/07/24/1530146.html "发布于2009-07-24 13:44")
[»](http://www.cnblogs.com/freeliver54/archive/2009/08/05/1539449.html)下一篇：[[书目20090805]钻石成功法则 - 陈安之](http://www.cnblogs.com/freeliver54/archive/2009/08/05/1539449.html "发布于2009-08-05 12:49")

posted on 2009-07-25 18:02 [freeliver54](http://www.cnblogs.com/freeliver54/) 阅读(406) [评论(0)](http://www.cnblogs.com/freeliver54/archive/2009/07/25/1530970.html#commentform)  [编辑](http://www.cnblogs.com/freeliver54/admin/EditPosts.aspx?postid=1530970) [收藏](http://www.cnblogs.com/freeliver54/AddToFavorite.aspx?id=1530970) [网摘](http://www.cnblogs.com/freeliver54/archive/2009/07/25/1530970.html#) 所属分类: [Oracle/MySQL](http://www.cnblogs.com/freeliver54/category/89971.html)
![]()

注册用户登录后才能发表评论，请[登录](http://passport.cnblogs.com/login.aspx?ReturnUrl=http%3a%2f%2fwww.cnblogs.com%2ffreeliver54%2farchive%2f2009%2f07%2f25%2f1530970.html%3flogin%3d1%23commentform)或[注册](http://passport.cnblogs.com/register.aspx?ReturnUrl=http%3a%2f%2fwww.cnblogs.com%2ffreeliver54%2farchive%2f2009%2f07%2f25%2f1530970.html%23Bottom2)。

网站导航：
[博客园首页](http://www.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [个人主页](http://home.cnblogs.com/)  [闪存](http://home.cnblogs.com/ing/)  [程序员招聘](http://job.cnblogs.com/)  [社区](http://space.cnblogs.com/)  [博问](http://space.cnblogs.com/q/)  [网摘](http://wz.cnblogs.com/)
IT新闻:
· [豆瓣网推出手机版 提供书影音资讯搜索服](http://news.cnblogs.com/n/54542/)
· [美国将推3D频道 6月直播世界杯](http://news.cnblogs.com/n/54541/)
· [用iPhone遥控无人直升机](http://news.cnblogs.com/n/54540/)
· [IT职业仍然是美国最好的工作之一](http://news.cnblogs.com/n/54539/)
· [马化腾：“全民公敌”偶像](http://news.cnblogs.com/n/54538/)

[![]()](http://www.china-pub.com/STATIC07/0912/zh_ndcx_091212.asp)
[China-pub 计算机图书网上专卖店！6.5万品种2-8折！](http://www.china-pub.com/itbook/)
[China-Pub 计算机绝版图书按需印刷服务](http://www.china-pub.com/static07/0901/zh_jueba_090121.asp)
链接：[切换模板](http://www.cnblogs.com/freeliver54/archive/2009/07/25/1530970.html?switchskin=1#skinlist)

**相关搜索:**
[Oracle/MySQL](http://zzk.cnblogs.com/s?w=Oracle%2fMySQL)
**在知识库中查看：**
[MySQL命令行 不同端口登录 执行SQL文件 创建用户 赋予权限 修改root密码](http://kb.cnblogs.com/a/1530970/)

Powered by:
[博客园](http://www.cnblogs.com/)
Copyright © freeliver54
### 导航

* [博客园](http://www.cnblogs.com/)
* [首页](http://www.cnblogs.com/freeliver54/)
* [新随笔](http://www.cnblogs.com/freeliver54/admin/EditPosts.aspx?opt=1)
* [联系](http://space.cnblogs.com/msg/send/freeliver54)
* [订阅](http://www.cnblogs.com/freeliver54/rss)[![订阅]()](http://www.cnblogs.com/freeliver54/rss)
* [管理](http://www.cnblogs.com/freeliver54/admin/EditPosts.aspx)

### 统计

* 随笔 - 714
* 文章 - 0
* 评论 - 1245
* 引用 - 157

### 公告

每一天都有新的开始 每一天都是新的开始 愿我们 善始善终 持之以恒 不管前方 是希望还是迷茫 我们都只有一个信念 让我们的爱 伴我们终生

[我的主页](http://home.cnblogs.com/freeliver54/)  [个人资料](http://home.cnblogs.com/freeliver54/detail/)
[我的闪存](http://home.cnblogs.com/freeliver54/ing/)  [发短消息](http://space.cnblogs.com/msg/send/freeliver54)
### 搜索

 

### 随笔分类

* [Ajax.Net(12)](http://www.cnblogs.com/freeliver54/category/76308.html) [(rss)](http://www.cnblogs.com/freeliver54/category/76308.html/rss "Subscribe to Ajax.Net(12)")
* [C++(3)](http://www.cnblogs.com/freeliver54/category/121615.html) [(rss)](http://www.cnblogs.com/freeliver54/category/121615.html/rss "Subscribe to C++(3)")
* [CSS/网页相关(19)](http://www.cnblogs.com/freeliver54/category/133235.html) [(rss)](http://www.cnblogs.com/freeliver54/category/133235.html/rss "Subscribe to CSS/网页相关(19)")
* [English(9)](http://www.cnblogs.com/freeliver54/category/110145.html) [(rss)](http://www.cnblogs.com/freeliver54/category/110145.html/rss "Subscribe to English(9)")
* [Java(9)](http://www.cnblogs.com/freeliver54/category/111073.html) [(rss)](http://www.cnblogs.com/freeliver54/category/111073.html/rss "Subscribe to Java(9)")
* [JavaScript(67)](http://www.cnblogs.com/freeliver54/category/76352.html) [(rss)](http://www.cnblogs.com/freeliver54/category/76352.html/rss "Subscribe to JavaScript(67)")
* [Linux(2)](http://www.cnblogs.com/freeliver54/category/107484.html) [(rss)](http://www.cnblogs.com/freeliver54/category/107484.html/rss "Subscribe to Linux(2)")
* [MS SQL(57)](http://www.cnblogs.com/freeliver54/category/77443.html) [(rss)](http://www.cnblogs.com/freeliver54/category/77443.html/rss "Subscribe to MS SQL(57)")
* [Oracle/MySQL(23)](http://www.cnblogs.com/freeliver54/category/89971.html) [(rss)](http://www.cnblogs.com/freeliver54/category/89971.html/rss "Subscribe to Oracle/MySQL(23)")
* [PhotoShop&Flash(13)](http://www.cnblogs.com/freeliver54/category/103597.html) [(rss)](http://www.cnblogs.com/freeliver54/category/103597.html/rss "Subscribe to PhotoShop&Flash(13)")
* [Report(13)](http://www.cnblogs.com/freeliver54/category/77424.html) [(rss)](http://www.cnblogs.com/freeliver54/category/77424.html/rss "Subscribe to Report(13)")
* [RFID相关(1)](http://www.cnblogs.com/freeliver54/category/172773.html) [(rss)](http://www.cnblogs.com/freeliver54/category/172773.html/rss "Subscribe to RFID相关(1)")
* [Visual Basic/ASP(9)](http://www.cnblogs.com/freeliver54/category/104390.html) [(rss)](http://www.cnblogs.com/freeliver54/category/104390.html/rss "Subscribe to Visual Basic/ASP(9)")
* [VS2008(10)](http://www.cnblogs.com/freeliver54/category/138020.html) [(rss)](http://www.cnblogs.com/freeliver54/category/138020.html/rss "Subscribe to VS2008(10)")
* [VS技術實踐(347)](http://www.cnblogs.com/freeliver54/category/47995.html) [(rss)](http://www.cnblogs.com/freeliver54/category/47995.html/rss "Subscribe to VS技術實踐(347)")
* [Windows Server(24)](http://www.cnblogs.com/freeliver54/category/90122.html) [(rss)](http://www.cnblogs.com/freeliver54/category/90122.html/rss "Subscribe to Windows Server(24)")
* [WinForm 开发(68)](http://www.cnblogs.com/freeliver54/category/88169.html) [(rss)](http://www.cnblogs.com/freeliver54/category/88169.html/rss "Subscribe to WinForm 开发(68)")
* [XML/UML(9)](http://www.cnblogs.com/freeliver54/category/77442.html) [(rss)](http://www.cnblogs.com/freeliver54/category/77442.html/rss "Subscribe to XML/UML(9)")
* [安全与加密(15)](http://www.cnblogs.com/freeliver54/category/86175.html) [(rss)](http://www.cnblogs.com/freeliver54/category/86175.html/rss "Subscribe to 安全与加密(15)")
* [安装部署(36)](http://www.cnblogs.com/freeliver54/category/85805.html) [(rss)](http://www.cnblogs.com/freeliver54/category/85805.html/rss "Subscribe to 安装部署(36)")
* [励志创业(25)](http://www.cnblogs.com/freeliver54/category/132722.html) [(rss)](http://www.cnblogs.com/freeliver54/category/132722.html/rss "Subscribe to 励志创业(25)")
* [每日文摘(122)](http://www.cnblogs.com/freeliver54/category/86343.html) [(rss)](http://www.cnblogs.com/freeliver54/category/86343.html/rss "Subscribe to 每日文摘(122)")
* [其他(11)](http://www.cnblogs.com/freeliver54/category/84533.html) [(rss)](http://www.cnblogs.com/freeliver54/category/84533.html/rss "Subscribe to 其他(11)")
* [软考(2)](http://www.cnblogs.com/freeliver54/category/121011.html) [(rss)](http://www.cnblogs.com/freeliver54/category/121011.html/rss "Subscribe to 软考(2)")
* [图书目录(24)](http://www.cnblogs.com/freeliver54/category/113983.html) [(rss)](http://www.cnblogs.com/freeliver54/category/113983.html/rss "Subscribe to 图书目录(24)")
* [项目工程(25)](http://www.cnblogs.com/freeliver54/category/89347.html) [(rss)](http://www.cnblogs.com/freeliver54/category/89347.html/rss "Subscribe to 项目工程(25)")
* [心灵之旅(66)](http://www.cnblogs.com/freeliver54/category/47972.html) [(rss)](http://www.cnblogs.com/freeliver54/category/47972.html/rss "Subscribe to 心灵之旅(66)")
* [硬件网络(14)](http://www.cnblogs.com/freeliver54/category/99498.html) [(rss)](http://www.cnblogs.com/freeliver54/category/99498.html/rss "Subscribe to 硬件网络(14)")
* [智能设备开发(12)](http://www.cnblogs.com/freeliver54/category/83924.html) [(rss)](http://www.cnblogs.com/freeliver54/category/83924.html/rss "Subscribe to 智能设备开发(12)")

### 随笔档案

* [2010年1月 (1)](http://www.cnblogs.com/freeliver54/archive/2010/01.html)
* [2009年12月 (10)](http://www.cnblogs.com/freeliver54/archive/2009/12.html)
* [2009年11月 (4)](http://www.cnblogs.com/freeliver54/archive/2009/11.html)
* [2009年10月 (8)](http://www.cnblogs.com/freeliver54/archive/2009/10.html)
* [2009年9月 (5)](http://www.cnblogs.com/freeliver54/archive/2009/09.html)
* [2009年8月 (9)](http://www.cnblogs.com/freeliver54/archive/2009/08.html)
* [2009年7月 (8)](http://www.cnblogs.com/freeliver54/archive/2009/07.html)
* [2009年6月 (4)](http://www.cnblogs.com/freeliver54/archive/2009/06.html)
* [2009年5月 (6)](http://www.cnblogs.com/freeliver54/archive/2009/05.html)
* [2009年4月 (4)](http://www.cnblogs.com/freeliver54/archive/2009/04.html)
* [2009年3月 (12)](http://www.cnblogs.com/freeliver54/archive/2009/03.html)
* [2009年2月 (24)](http://www.cnblogs.com/freeliver54/archive/2009/02.html)
* [2009年1月 (10)](http://www.cnblogs.com/freeliver54/archive/2009/01.html)
* [2008年12月 (14)](http://www.cnblogs.com/freeliver54/archive/2008/12.html)
* [2008年11月 (18)](http://www.cnblogs.com/freeliver54/archive/2008/11.html)
* [2008年10月 (23)](http://www.cnblogs.com/freeliver54/archive/2008/10.html)
* [2008年9月 (22)](http://www.cnblogs.com/freeliver54/archive/2008/09.html)
* [2008年8月 (9)](http://www.cnblogs.com/freeliver54/archive/2008/08.html)
* [2008年7月 (30)](http://www.cnblogs.com/freeliver54/archive/2008/07.html)
* [2008年6月 (17)](http://www.cnblogs.com/freeliver54/archive/2008/06.html)
* [2008年5月 (11)](http://www.cnblogs.com/freeliver54/archive/2008/05.html)
* [2008年4月 (45)](http://www.cnblogs.com/freeliver54/archive/2008/04.html)
* [2008年3月 (28)](http://www.cnblogs.com/freeliver54/archive/2008/03.html)
* [2008年2月 (11)](http://www.cnblogs.com/freeliver54/archive/2008/02.html)
* [2008年1月 (17)](http://www.cnblogs.com/freeliver54/archive/2008/01.html)
* [2007年12月 (15)](http://www.cnblogs.com/freeliver54/archive/2007/12.html)
* [2007年11月 (20)](http://www.cnblogs.com/freeliver54/archive/2007/11.html)
* [2007年10月 (22)](http://www.cnblogs.com/freeliver54/archive/2007/10.html)
* [2007年9月 (36)](http://www.cnblogs.com/freeliver54/archive/2007/09.html)
* [2007年8月 (23)](http://www.cnblogs.com/freeliver54/archive/2007/08.html)
* [2007年7月 (24)](http://www.cnblogs.com/freeliver54/archive/2007/07.html)
* [2007年6月 (6)](http://www.cnblogs.com/freeliver54/archive/2007/06.html)
* [2007年5月 (7)](http://www.cnblogs.com/freeliver54/archive/2007/05.html)
* [2007年4月 (32)](http://www.cnblogs.com/freeliver54/archive/2007/04.html)
* [2007年3月 (46)](http://www.cnblogs.com/freeliver54/archive/2007/03.html)
* [2007年2月 (22)](http://www.cnblogs.com/freeliver54/archive/2007/02.html)
* [2007年1月 (31)](http://www.cnblogs.com/freeliver54/archive/2007/01.html)
* [2006年12月 (45)](http://www.cnblogs.com/freeliver54/archive/2006/12.html)
* [2006年11月 (8)](http://www.cnblogs.com/freeliver54/archive/2006/11.html)
* [2006年7月 (1)](http://www.cnblogs.com/freeliver54/archive/2006/07.html)
* [2006年6月 (3)](http://www.cnblogs.com/freeliver54/archive/2006/06.html)
* [2006年5月 (1)](http://www.cnblogs.com/freeliver54/archive/2006/05.html)
* [2006年4月 (13)](http://www.cnblogs.com/freeliver54/archive/2006/04.html)
* [2006年2月 (8)](http://www.cnblogs.com/freeliver54/archive/2006/02.html)

### techLINKS

* [Ajax.net](http://ajax.asp.net/)
* [ASP.NET 快速入门教程](http://www.asp.net/QuickStart/aspnet/)
* [MSDN](http://www.microsoft.com/china/msdn/library/default.aspx)
* [UML](http://www.uml.org.cn/)
* [winforms](http://samples.gotdotnet.com/quickstart/winforms/)
* [XSLT](http://www.w3.org/TR/xslt)
* [窗体 快速入门教程](http://chs.gotdotnet.com/quickstart/winforms/)
* [移动开发人员中心](http://msdn.microsoft.com/windowsmobile/default.aspx)

### 友情链接

* [Sun&Livia](http://www.cnweblog.com/homeus/)
* [新华网](http://www.xinhuanet.com/)
* [雪中路人](http://blog.sina.com.cn/u/1280914171#aList_1)
* [有国才有家 爱我中华](http://hi.baidu.com/freeliver54)
* [中国经营网](http://www.cb.com.cn/)
* [自由 成功 富足 从容](http://www.cgxdt.com/go.asp?user=freeliver54)

### 积分与排名

* 积分 - 584146
* 排名 - 57

### 最新评论 [![]()](http://www.cnblogs.com/freeliver54/CommentsRSS.aspx)

* [1. Re:[转]英语字根](http://www.cnblogs.com/freeliver54/archive/2009/12/10/1621022.html#1728924)
* +　 plus　加号；正号 -　 minus　减号；负号 ±　plus or minus　正负号 ×　is multiplied by　乘号 ÷　is divided b...
* --freea
* [2. Re:心居何处？](http://www.cnblogs.com/freeliver54/archive/2009/12/15/1624696.html#1726601)
* 与世界同在
* --freea
* [3. Re:成家撑家 不要妄自菲薄](http://www.cnblogs.com/freeliver54/archive/2009/12/15/1624998.html#1726597)
* 躺在过去的经验和教训上迟早是会玩完的 明天的美好不仅在于明天是未知的更多的是因为明天是可以被按新的变化意愿创造的 拥抱明天的世界以未曾束缚的心机会 属于努力创造和利用
* --freea
* [4. Re:程序员的人生 该将如何规划?](http://www.cnblogs.com/freeliver54/archive/2007/02/12/648209.html#1726562)
* 如果心不退那就意勇进热爱并享受自己的选择
* --freea
* [5. Re:[转]js获取网站根路径(站点及虚拟目录)](http://www.cnblogs.com/freeliver54/archive/2009/10/20/1586889.html#1726260)
* 不错。谢谢。
* --零度的火

### 阅读排行榜

* [1. 程序员的人生 该将如何规划?(23944)](http://www.cnblogs.com/freeliver54/archive/2007/02/12/648209.html)
* [2. VS2005中GridView簡單應用(23026)](http://www.cnblogs.com/freeliver54/archive/2006/02/09/327853.html)
* [3. SQL 2005 Express 的“企业管理器” 下载(14827)](http://www.cnblogs.com/freeliver54/archive/2006/12/19/596603.html)
* [4. 遭遇“windows已经阻止此软件因为无法验证发行者”(11305)](http://www.cnblogs.com/freeliver54/archive/2008/04/14/1152769.html)
* [5. 下载Eclipse的SWT插件(6901)](http://www.cnblogs.com/freeliver54/archive/2007/11/05/949935.html)
* [6. VS2005中ReportViewer 本地模式下报表呈现 入门示例(6829)](http://www.cnblogs.com/freeliver54/archive/2006/11/27/573453.html)
* [7. JS操作select相关方法：新增 修改 删除 选中 清空 判断存在 等(5887)](http://www.cnblogs.com/freeliver54/archive/2007/02/09/645983.html)
* [8. [转]C#中的IntPtr类型(5485)](http://www.cnblogs.com/freeliver54/archive/2008/10/15/1311371.html)
* [9. [转]FreeTextBox使用详解(4571)](http://www.cnblogs.com/freeliver54/archive/2007/01/08/614626.html)
* [10. C# 利用WinRAR (加密)压缩及解压缩 相关文件夹及文件(4461)](http://www.cnblogs.com/freeliver54/archive/2007/01/17/622992.html)

### 评论排行榜

* [1. 程序员的人生 该将如何规划?(166)](http://www.cnblogs.com/freeliver54/archive/2007/02/12/648209.html)
* [2. VS2005 TreeView 的 CheckBox 被点击时的引发页面回发事件(29)](http://www.cnblogs.com/freeliver54/archive/2007/01/05/612393.html)
* [3. VS2005中GridView簡單應用(25)](http://www.cnblogs.com/freeliver54/archive/2006/02/09/327853.html)
* [4. C# 利用WinRAR (加密)压缩及解压缩 相关文件夹及文件(23)](http://www.cnblogs.com/freeliver54/archive/2007/01/17/622992.html)
* [5. 分页及页码导航 用户控件(22)](http://www.cnblogs.com/freeliver54/archive/2006/12/29/607331.html)

### 60天内阅读排行

* [1. [文摘20091116]一生必看的88本书(118)](http://www.cnblogs.com/freeliver54/archive/2009/11/16/1604192.html)
* [2. [转]SourceSafe中的权限管理(95)](http://www.cnblogs.com/freeliver54/archive/2009/11/27/1611915.html)
* [3. [转]Marquee 处理中 请等待 进度条(92)](http://www.cnblogs.com/freeliver54/archive/2009/11/23/1608877.html)
* [4. [文摘20091204]中国佛学66句禅语(88)](http://www.cnblogs.com/freeliver54/archive/2009/12/04/1616913.html)
* [5. windows 2003 组策略 设置开机登录时不需按CTRL+ALT+DEL 及关机时不需输关机原因(81)](http://www.cnblogs.com/freeliver54/archive/2009/12/09/1619883.html)
* [6. [English20091217]英语口语444句(60)](http://www.cnblogs.com/freeliver54/archive/2009/12/17/1626287.html)
* [7. [文字20091204]佛说--贪、嗔、痴、妒、慢、疑(55)](http://www.cnblogs.com/freeliver54/archive/2009/12/04/1616901.html)
* [8. [转]英语字根(52)](http://www.cnblogs.com/freeliver54/archive/2009/12/10/1621022.html)
* [9. [文摘20091126]不要让愤怒伤了自己(51)](http://www.cnblogs.com/freeliver54/archive/2009/11/26/1611244.html)
* [10. 心居何处？(50)](http://www.cnblogs.com/freeliver54/archive/2009/12/15/1624696.html)

*
