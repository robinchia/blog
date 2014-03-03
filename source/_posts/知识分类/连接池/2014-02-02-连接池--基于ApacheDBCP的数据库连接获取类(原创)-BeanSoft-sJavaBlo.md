---
layout: post
title: "基于 Apache DBCP 的数据库连接获取类(原创) - BeanSoft-s Java Blo"
categories: 连接池
tags: 
 - 连接池
--- 

# 基于 Apache DBCP 的数据库连接获取类(原创) - BeanSoft's Java Blog - BlogJava

[BeanSoft's Java Blog](http://www.blogjava.net/beansoft/)

MyEclipse 教程, Java EE 5, JSPWiki, Spring, Struts, Hibernate, JPA, SWT, Swing, AJAX, JavaScript, Netbeans

# 导航

* [BlogJava](http://www.blogjava.net/)
* [首页](http://www.blogjava.net/beansoft/)
* [新随笔](http://www.blogjava.net/beansoft/admin/EditPosts.aspx?opt=1)
*
* [聚合](http://www.blogjava.net/beansoft/rss)[![]()](http://www.blogjava.net/beansoft/rss)
* [管理](http://www.blogjava.net/beansoft/admin/EditPosts.aspx)
公告

* Java EE 专家
Java 高清视频
Mail: [beansoft@126.com](mailto:beansoft@126.com)
Location: Beijing
![]()
![]()

# 随笔分类

* [AJAX(22)](http://www.blogjava.net/beansoft/category/19463.html) [(rss)](http://www.blogjava.net/beansoft/category/19463.html/rss "Subscribe to AJAX(22)")
* [Database(11)](http://www.blogjava.net/beansoft/category/21451.html) [(rss)](http://www.blogjava.net/beansoft/category/21451.html/rss "Subscribe to Database(11)")
* [Eclipse(30)](http://www.blogjava.net/beansoft/category/17556.html) [(rss)](http://www.blogjava.net/beansoft/category/17556.html/rss "Subscribe to Eclipse(30)")
* [Firefox(3)](http://www.blogjava.net/beansoft/category/17516.html) [(rss)](http://www.blogjava.net/beansoft/category/17516.html/rss "Subscribe to Firefox(3)")
* [Free Software(20)](http://www.blogjava.net/beansoft/category/17577.html) [(rss)](http://www.blogjava.net/beansoft/category/17577.html/rss "Subscribe to Free Software(20)")
* [Game(4)](http://www.blogjava.net/beansoft/category/24282.html) [(rss)](http://www.blogjava.net/beansoft/category/24282.html/rss "Subscribe to Game(4)")
* [Hibernate(8)](http://www.blogjava.net/beansoft/category/23349.html) [(rss)](http://www.blogjava.net/beansoft/category/23349.html/rss "Subscribe to Hibernate(8)")
* [Java Code Share(12)](http://www.blogjava.net/beansoft/category/22785.html) [(rss)](http://www.blogjava.net/beansoft/category/22785.html/rss "Subscribe to Java Code Share(12)")
* [Java EE(32)](http://www.blogjava.net/beansoft/category/17555.html) [(rss)](http://www.blogjava.net/beansoft/category/17555.html/rss "Subscribe to Java EE(32)")
* [Java SE(27)](http://www.blogjava.net/beansoft/category/17517.html) [(rss)](http://www.blogjava.net/beansoft/category/17517.html/rss "Subscribe to Java SE(27)")
* [JavaMail(5)](http://www.blogjava.net/beansoft/category/26076.html) [(rss)](http://www.blogjava.net/beansoft/category/26076.html/rss "Subscribe to JavaMail(5)")
* [JavaScript(1)](http://www.blogjava.net/beansoft/category/26857.html) [(rss)](http://www.blogjava.net/beansoft/category/26857.html/rss "Subscribe to JavaScript(1)")
* [JBoss(3)](http://www.blogjava.net/beansoft/category/26074.html) [(rss)](http://www.blogjava.net/beansoft/category/26074.html/rss "Subscribe to JBoss(3)")
* [JSF(4)](http://www.blogjava.net/beansoft/category/26022.html) [(rss)](http://www.blogjava.net/beansoft/category/26022.html/rss "Subscribe to JSF(4)")
* [JSP(3)](http://www.blogjava.net/beansoft/category/27072.html) [(rss)](http://www.blogjava.net/beansoft/category/27072.html/rss "Subscribe to JSP(3)")
* [JSPWiki(16)](http://www.blogjava.net/beansoft/category/17506.html) [(rss)](http://www.blogjava.net/beansoft/category/17506.html/rss "Subscribe to JSPWiki(16)")
* [Michael Jackson(1)](http://www.blogjava.net/beansoft/category/18621.html) [(rss)](http://www.blogjava.net/beansoft/category/18621.html/rss "Subscribe to Michael Jackson(1)")
* [My Open Source Toys(30)](http://www.blogjava.net/beansoft/category/17569.html) [(rss)](http://www.blogjava.net/beansoft/category/17569.html/rss "Subscribe to My Open Source Toys(30)")
* [MyEclipse(43)](http://www.blogjava.net/beansoft/category/26073.html) [(rss)](http://www.blogjava.net/beansoft/category/26073.html/rss "Subscribe to MyEclipse(43)")
* [NetBeans(15)](http://www.blogjava.net/beansoft/category/21997.html) [(rss)](http://www.blogjava.net/beansoft/category/21997.html/rss "Subscribe to NetBeans(15)")
* [Open Source World(14)](http://www.blogjava.net/beansoft/category/17575.html) [(rss)](http://www.blogjava.net/beansoft/category/17575.html/rss "Subscribe to Open Source World(14)")
* [Oracle(2)](http://www.blogjava.net/beansoft/category/26075.html) [(rss)](http://www.blogjava.net/beansoft/category/26075.html/rss "Subscribe to Oracle(2)")
* [Other(72)](http://www.blogjava.net/beansoft/category/17585.html) [(rss)](http://www.blogjava.net/beansoft/category/17585.html/rss "Subscribe to Other(72)")
* [Portable Java IDE(3)](http://www.blogjava.net/beansoft/category/17518.html) [(rss)](http://www.blogjava.net/beansoft/category/17518.html/rss "Subscribe to Portable Java IDE(3)")
* [ROR(2)](http://www.blogjava.net/beansoft/category/25965.html) [(rss)](http://www.blogjava.net/beansoft/category/25965.html/rss "Subscribe to ROR(2)")
* [Spring(29)](http://www.blogjava.net/beansoft/category/23350.html) [(rss)](http://www.blogjava.net/beansoft/category/23350.html/rss "Subscribe to Spring(29)")
* [Swing, GUI(9)](http://www.blogjava.net/beansoft/category/20433.html) [(rss)](http://www.blogjava.net/beansoft/category/20433.html/rss "Subscribe to Swing, GUI(9)")
* [SWT/JFACE/RCP(24)](http://www.blogjava.net/beansoft/category/20278.html) [(rss)](http://www.blogjava.net/beansoft/category/20278.html/rss "Subscribe to SWT/JFACE/RCP(24)")
* [Tomcat(1)](http://www.blogjava.net/beansoft/category/26856.html) [(rss)](http://www.blogjava.net/beansoft/category/26856.html/rss "Subscribe to Tomcat(1)")
* [Web(42)](http://www.blogjava.net/beansoft/category/17587.html) [(rss)](http://www.blogjava.net/beansoft/category/17587.html/rss "Subscribe to Web(42)")
* [Web Framework(8)](http://www.blogjava.net/beansoft/category/22027.html) [(rss)](http://www.blogjava.net/beansoft/category/22027.html/rss "Subscribe to Web Framework(8)")
* [Weblogic(7)](http://www.blogjava.net/beansoft/category/17929.html) [(rss)](http://www.blogjava.net/beansoft/category/17929.html/rss "Subscribe to Weblogic(7)")
* [What's Hot(17)](http://www.blogjava.net/beansoft/category/17574.html) [(rss)](http://www.blogjava.net/beansoft/category/17574.html/rss "Subscribe to What's Hot(17)")
* [战地2 Battle Field 2(2)](http://www.blogjava.net/beansoft/category/17511.html) [(rss)](http://www.blogjava.net/beansoft/category/17511.html/rss "Subscribe to 战地2 Battle Field 2(2)")
* [老军医悬壶济世(13)](http://www.blogjava.net/beansoft/category/18212.html) [(rss)](http://www.blogjava.net/beansoft/category/18212.html/rss "Subscribe to 老军医悬壶济世(13)")

### 最新随笔

* [1. 关于MyEclipse 6.0.0 GA开发SSH应用的%%%% Error Creating SessionFactory %%%% java.lang.SecurityException: class "org.apache.commons.collections.SequencedHashMap"'异常的解决方案](http://www.blogjava.net/beansoft/archive/2008/01/11/174610.html)
* [2. 可以在多普达微软 Pocket PC Windows Mobile 5 手机上运行的eswt程序包](http://www.blogjava.net/beansoft/archive/2008/01/10/174248.html)
* [3. BlogJava 备份文章阅读器:增加日期范围增量备份功能](http://www.blogjava.net/beansoft/archive/2008/01/08/173714.html)
* [4. 电子书 《MyEclipse 6 Java 开发中文教程》 第十章 开发 Spring 应用 完工](http://www.blogjava.net/beansoft/archive/2008/01/07/173395.html)
* [5. 如何购买《MyEclipse 6 Java 开发中文教程》DVD光盘（含源代码，视频和软件）及网上答疑](http://www.blogjava.net/beansoft/archive/2008/01/07/173298.html)
* [6. IBM 中文网站的 Spring 系列等资料](http://www.blogjava.net/beansoft/archive/2008/01/04/172725.html)
* [7. Spring 1.2和2.0的简单AOP例子](http://www.blogjava.net/beansoft/archive/2008/01/02/172266.html)
* [8. Word 文档加入带链接功能的目录的方法](http://www.blogjava.net/beansoft/archive/2007/12/27/171006.html)
* [9. 电子书 《MyEclipse 6 Java 开发中文教程》 第九章 开发Struts 1.x应用 完工](http://www.blogjava.net/beansoft/archive/2007/12/27/170997.html)
* [10. 电子书 《MyEclipse 6 Java 开发中文教程》第八章 开发 Web 应用](http://www.blogjava.net/beansoft/archive/2007/12/21/169400.html)
* [11. 电子书 《MyEclipse 6 Java 开发中文教程》 第七章 Hibernate 开发预览](http://www.blogjava.net/beansoft/archive/2007/12/19/168742.html)
* [12. MyEclipse 6 实战开发讲解视频入门 11 Struts 文件上传](http://www.blogjava.net/beansoft/archive/2007/12/18/168600.html)
* [13. 电子书 《MyEclipse 6 Java 开发中文教程》 第五，六章预览](http://www.blogjava.net/beansoft/archive/2007/12/14/167733.html)
* [14. 原创电子书 《MyEclipse 6 Java 开发中文教程》下载](http://www.blogjava.net/beansoft/archive/2007/12/10/166631.html)
* [15. 感谢 2007 年 12 月的 <<程序员>> 杂志推荐我的 Blog](http://www.blogjava.net/beansoft/archive/2007/12/03/164796.html)
* [16. 视频讲解: Eclipse/MyEclipse 如何导入项目源代码](http://www.blogjava.net/beansoft/archive/2007/12/03/164748.html)
* [17. MyEclipse 6 实战开发讲解视频入门 10 JSP 文件上传下载](http://www.blogjava.net/beansoft/archive/2007/12/02/164704.html)
* [18. Java SE 6 Update N Early Access 提供下载](http://www.blogjava.net/beansoft/archive/2007/12/02/164696.html)
* [19. Spring 2.5 标注开发的简单例子](http://www.blogjava.net/beansoft/archive/2007/11/30/164230.html)
* [20. MyEclipse 6 实战开发讲解视频入门 9 JSF 1.2 入门开发](http://www.blogjava.net/beansoft/archive/2007/11/30/164166.html)

### 搜索

*
*  

### 积分与排名

* 积分 - 632502
* 排名 - 1

### 最新评论 [![]()](http://www.blogjava.net/beansoft/CommentsRSS.aspx)

* [1. re: 如何购买《MyEclipse 6 Java 开发中文教程》DVD光盘（含源代码，视频和软件）及网上答疑](http://www.blogjava.net/beansoft/archive/2008/01/12/173298.html#174772)
* 评论内容较长,点击标题查看
* --jerryhu
* [2. re: 如何购买《MyEclipse 6 Java 开发中文教程》DVD光盘（含源代码，视频和软件）及网上答疑](http://www.blogjava.net/beansoft/archive/2008/01/11/173298.html#174727)
* 建议增加spring+jdbc介绍另外JPA部分也增加与spring整和的介绍
* --建议
* [3. re: 电子书 《MyEclipse 6 Java 开发中文教程》 第十章 开发 Spring 应用 完工](http://www.blogjava.net/beansoft/archive/2008/01/11/173395.html#174585)
* 评论内容较长,点击标题查看
* --BeanSoft
* [4. re: 老军医讲牙石](http://www.blogjava.net/beansoft/archive/2008/01/11/104339.html#174565)
* 正在想最近总是牙出血是什么原因呢，原来如此！
* --爱上鸟的鱼
* [5. re: 老军医讲牙石](http://www.blogjava.net/beansoft/archive/2008/01/11/104339.html#174559)
* 正在向最近总是牙出血是什么原因呢，原来如此！
* --爱上鸟的鱼
* [6. re: 原创电子书 《MyEclipse 6 Java 开发中文教程》下载[未登录]](http://www.blogjava.net/beansoft/archive/2008/01/11/166631.html#174553)
* 评论内容较长,点击标题查看
* --什么捷
* [7. re: 电子书 《MyEclipse 6 Java 开发中文教程》 第十章 开发 Spring 应用 完工](http://www.blogjava.net/beansoft/archive/2008/01/11/173395.html#174537)
* 评论内容较长,点击标题查看
* --爱上鸟的鱼
* [8. re: MyEclipse 6 实战开发讲解视频入门 6 Web 入门开发 - JSP/HTML/JDBC 登录](http://www.blogjava.net/beansoft/archive/2008/01/11/161502.html#174516)
* 美国就是变态!怎么搞的!下你个东西,也是左搞搞右搞搞!
* --pzxy
* [9. re: 原创歌曲: Java 就是好呀](http://www.blogjava.net/beansoft/archive/2008/01/10/157925.html#174437)
* 没发现,老师您这么有喜剧天份
* --tott
* [10. re: MyEclipse 6 实战开发讲解视频入门 7 Struts 入门开发](http://www.blogjava.net/beansoft/archive/2008/01/10/163354.html#174429)
* 正好,我急用学struts1.0.thanks
* --张晓云

### 阅读排行榜

* [1. XLoadTree 基于AJAX + XML动态加载的JS树组件的文档翻译- 已完成(原创)(5081)](http://www.blogjava.net/beansoft/archive/2006/11/25/83486.html)
* [2. MyEclipse Hibernate 快速入门中文版(4417)](http://www.blogjava.net/beansoft/archive/2007/08/15/137067.html)
* [3. Sysdeo Eclipse Tomcat 3.2.1 插件中文版下载(4393)](http://www.blogjava.net/beansoft/archive/2007/06/18/124894.html)
* [4. MyEclipse 5.5 开发 Spring + Struts + Hibernate 的详解视频(长1.5小时)(4341)](http://www.blogjava.net/beansoft/archive/2007/10/07/150877.html)
* [5. 《jsf入门》简体中文版.rar 电子书下载(4100)](http://www.blogjava.net/beansoft/archive/2007/05/10/116486.html)
* [6. JSP 中 AJAX 的表单提交中文问题的简单解决方案 - GBK 版本(原创)(3942)](http://www.blogjava.net/beansoft/archive/2006/12/31/91144.html)
* [7. JSP 文件下载的相对完整代码(解决中文问题和Weblogic报错)(3807)](http://www.blogjava.net/beansoft/archive/2007/02/01/97294.html)
* [8. 北京市IT公司黑名单(转载)(3707)](http://www.blogjava.net/beansoft/archive/2007/03/27/106733.html)
* [9. AJAX 入门培训 PPT 及示例代码(3651)](http://www.blogjava.net/beansoft/archive/2007/05/08/115878.html)
* [10. Jigloo 开发 SWT 的入门教程(3626)](http://www.blogjava.net/beansoft/archive/2007/03/18/104577.html)
* [11. Eclipse 插件精选介绍(原创)(3612)](http://www.blogjava.net/beansoft/archive/2006/11/22/82765.html)
* [12. Tomcat 5.5 的 Apache Tomcat Native library(3557)](http://www.blogjava.net/beansoft/archive/2006/12/22/89577.html)
* [13. JSP 中 AJAX 的表单提交中文问题的简单解决方案(3536)](http://www.blogjava.net/beansoft/archive/2006/12/25/89835.html)
* [14. MySQL 5 绿色版(BAT版本)(3515)](http://www.blogjava.net/beansoft/archive/2007/02/05/97940.html)
* [15. MyEclipse 6 学习视频集中下载(文档,视频,源代码)(3470)](http://www.blogjava.net/beansoft/archive/2007/10/24/155586.html)
* [16. LambdaProbe 中文包下载(99%完成)(3434)](http://www.blogjava.net/beansoft/archive/2006/12/19/88881.html)
* [17. 用开源软件搭建企业内部协作平台, Kill QQ MSN(3391)](http://www.blogjava.net/beansoft/archive/2007/02/03/97760.html)
* [18. 理解并使用 JSPWiki 中的权限控制(3299)](http://www.blogjava.net/beansoft/archive/2007/01/08/92320.html)
* [19. 使用 JDK 6 中的 JConsole 监控应用(原创)(3289)](http://www.blogjava.net/beansoft/archive/2006/12/13/87494.html)
* [20. Netbeans 5.5 + JPA + Hibernate 3 + Tomcat 实例有声视频(3173)](http://www.blogjava.net/beansoft/archive/2007/05/21/118869.html)

### 60天内阅读排行

* [1. 翻译: MyEclipse Spring 入门教程(含官方视频和AOP例子)(2239)](http://www.blogjava.net/beansoft/archive/2007/11/27/163416.html)
* [2. 翻译: MyEclipse AJAX 调试教程(含官方视频)(2203)](http://www.blogjava.net/beansoft/archive/2007/11/22/162498.html)
* [3. 原创电子书 《MyEclipse 6 Java 开发中文教程》下载(2063)](http://www.blogjava.net/beansoft/archive/2007/12/10/166631.html)
* [4. Spring 2.5 标注开发的简单例子(2030)](http://www.blogjava.net/beansoft/archive/2007/11/30/164230.html)
* [5. 翻译: MyEclipse Hibernate 入门教程(含官方视频)(1966)](http://www.blogjava.net/beansoft/archive/2007/11/13/160288.html)
* [6. 击破谎言: Spring 2.5 并非"完全"支持基于标注的配置(1914)](http://www.blogjava.net/beansoft/archive/2007/11/23/162700.html)
* [7. MyEclipse 6 实战开发讲解视频入门 6 Web 入门开发 - JSP/HTML/JDBC 登录(1668)](http://www.blogjava.net/beansoft/archive/2007/11/19/161502.html)
* [8. MyEclipse 6 实战开发讲解视频入门 7 Struts 入门开发(1644)](http://www.blogjava.net/beansoft/archive/2007/11/26/163354.html)
* [9. MyEclipse 6 实战开发讲解视频入门 9 JSF 1.2 入门开发(1554)](http://www.blogjava.net/beansoft/archive/2007/11/30/164166.html)
* [10. MyEclipse 6 实战开发讲解视频入门 10 JSP 文件上传下载(1424)](http://www.blogjava.net/beansoft/archive/2007/12/02/164704.html)
* [11. MyEclipse 6 实战开发讲解视频入门 8 XFire Web Service 入门开发(1369)](http://www.blogjava.net/beansoft/archive/2007/11/29/163907.html)
* [12. 翻译: MyEclipse Struts 1.x 教程(PDF格式)(1351)](http://www.blogjava.net/beansoft/archive/2007/11/16/160876.html)
* [13. Spring 1.2和2.0的简单AOP例子(1253)](http://www.blogjava.net/beansoft/archive/2008/01/02/172266.html)
* [14. 电子书 《MyEclipse 6 Java 开发中文教程》 第七章 Hibernate 开发预览(1174)](http://www.blogjava.net/beansoft/archive/2007/12/19/168742.html)
* [15. 电子书 《MyEclipse 6 Java 开发中文教程》 第五，六章预览(1165)](http://www.blogjava.net/beansoft/archive/2007/12/14/167733.html)
* [16. 电子书 《MyEclipse 6 Java 开发中文教程》 第十章 开发 Spring 应用 完工(1101)](http://www.blogjava.net/beansoft/archive/2008/01/07/173395.html)
* [17. MyEclipse 6 实战开发讲解视频入门 11 Struts 文件上传(1099)](http://www.blogjava.net/beansoft/archive/2007/12/18/168600.html)
* [18. 电子书 《MyEclipse 6 Java 开发中文教程》 第九章 开发Struts 1.x应用 完工(961)](http://www.blogjava.net/beansoft/archive/2007/12/27/170997.html)
* [19. 视频讲解: Eclipse/MyEclipse 如何导入项目源代码(900)](http://www.blogjava.net/beansoft/archive/2007/12/03/164748.html)
* [20. 感谢 2007 年 12 月的 <<程序员>> 杂志推荐我的 Blog(736)](http://www.blogjava.net/beansoft/archive/2007/12/03/164796.html)

 
[基于 Apache DBCP 的数据库连接获取类(原创)](http://www.blogjava.net/beansoft/archive/2007/01/19/94882.html)

2005年12月5日星期一

基于 Apache DBCP 的数据库连接获取类, 可以让你在 Tomcat 之外的 J2SE 程序或者其它应用服务器上使用 Apache 的数据库连接池.

TODO: 增加最大连接数和最小连接数的设置功能

配置文件:

ConnectionFactory.properties

# 2004-12-30
# 数据库连接工厂的配置文件, 类文件参见 util.ConnectionFactory
# 调试标志, 为 true 时使用 JDBC 直接连接(使用 url, driver, user, password 四个参数)
# 为 false 时使用内置数据库连接池(使用参数 jdbc.jndi), 配置文件位于 db.properties 中
debug=false
# JDBC的连接地址 URL
jdbc.url=jdbc:microsoft:sqlserver://127.0.0.1:1433;DatabaseName=yuelao
# JDBC 的驱动
jdbc.driver=com.microsoft.jdbc.sqlserver.SQLServerDriver
# JDBC 的连接用户名
jdbc.user=sa
# JDBC 的连接密码
jdbc.password=
#debug 为 false 时, 将根据连接池名获取数据库连接, 适用于程序在服务器运行的时候

.Java 文件:

/*
* @(#)ConnectionFactory.java 1.2 2005-11-25
*
* Copyright 2005 BeanSoft Studio. All rights reserved.
* PROPRIETARY/CONFIDENTIAL. Use is subject to license terms.
*/

package beansoft.util;

import java.io.ByteArrayInputStream;
import java.io.FileOutputStream;
import java.io.FileWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

import javax.sql.DataSource;
import org.apache.commons.dbcp.BasicDataSource;

import beansoft.jsp.StringUtil;
import beansoft.sql.DatabaseUtil;

/**
* ConnectionFactory provide a factory class that produces all database
* connections from here, and it provides methods for shutdown and restart
* data sources as well as reading and saving configuration parameters from/to file.
*
* 数据库连接工厂类, 所有的数据库连接都从这里产生, 提供关闭和重启数据源的方法, 以及读取和保存
* 配置参数到文件的能力.
*
* 2005-08-19
* Using Apache DBCP as database connection pool provider.
* 使用 Apache DBCP 作为连接池的提供类.
*
* @link [http://jakarta.apache.org/commons/dbcp/](http://jakarta.apache.org/commons/dbcp/)
*
* Dependency:
* commons-collections.jar
* commons-pool.jar
* commons-dbcp.jar
* j2ee.jar (for the javax.sql classes)
*
* If you using this class with Tomcat's web application, then all the above jars
* not need to be added because the Tomcat it self has included these class libs.
* 如果你在 Tomcat 的 Web Application 中调用这个类, 以上的 JAR 都不用单独
* 加入的, 因为 Tomcat 默认已经自带了这些类库.
*
* @author BeanSoft
* @version 1.2
* 2005-11-25
*/
public class ConnectionFactory {

/** Database password */
private static String password;

/** Database username */
private static String user;

/** JDBC URL */
private static String url;

/**
* JDBC driver class name
*/
private static String driver;

/**
* DEBUG flag, default value is true, returns a connection that directly fetched using
* JDBC API; if falg is false, returns a connection returned by the connection pool,
* if u want depoly the application, please make this flag be false.
* 调试标记, 默认值为 true, 则返回直接使用 JDBC 获取的连接; 如果标记为 false,
* 则从连接池中返回器连接, 发布程序时请将这个标志 设置为 false.
*/
private static boolean DEBUG = true;

/** Connection properties value */
private static Properties props = new Properties();

/** The data source object, added at 2005-08-19 */
private static DataSource dataSource = null;

// Load configuration from resource /ConnectionFactory.properties
static {
loadConfiguration();
}

private ConnectionFactory() {
}

/**
* Factory method: obtain a database connection.
* 工厂方法: 获取一个数据库连接.
*
*
* @return Connection a java.sql.Connection object
*/
public static Connection getConnection() {
try {
Connection conn = null;

// Debug mode, obtain connection directly through JDBC API
if (DEBUG) {

Class.forName(driver);

conn = DriverManager.getConnection(url, user, password);
} else {
// TODO
// // Looking data source through JNDI tree
// DataSource dataSource = (DataSource) getInitialContext()
// .lookup(jndi);
// conn = dataSource.getConnection();
conn = setupDataSource().getConnection();
}

return conn;
} catch (Exception ex) {
System.err.println("Error: Unable to get a connection: " + ex);
ex.printStackTrace();
}

return null;
}

/**
* Load and parse configuration.
*/
public static void loadConfiguration() {
try {
props.load(new ByteArrayInputStream(readConfigurationString().getBytes()));

// Load DEBUG flag, default to true
DEBUG = Boolean.valueOf(props.getProperty("debug", "true"))
.booleanValue();
password = props.getProperty("jdbc.password", null);
user = props.getProperty("jdbc.user", null);
url = props.getProperty("jdbc.url", null);
driver = props.getProperty("jdbc.driver");
} catch (Exception e) {
e.printStackTrace();
}
}

/**
* Save the current configuration properties.
*/
public static void saveConfiguration() {
saveConfiguration(getProperties());
}

/**
* Read content string from configuration file. Because Class.getResourceAsStream(String)
* sometimes cache the contents, so here used this method.
* 读取配置文件中的字符串.
* 因为 Class 类的 getResourceAsStream(String) 方法有时候会出现缓存, 因此
* 不得已使用了这种办法.
* @return String, null if failed
*/
public static String readConfigurationString() {
try {
java.io.FileInputStream fin = new java.io.FileInputStream(
getConfigurationFilePath());
java.io.ByteArrayOutputStream bout = new
java.io.ByteArrayOutputStream();

int data;
while( (data = fin.read()) != -1) {
bout.write(data);
}
bout.close();
fin.close();

return bout.toString();
} catch (Exception ex) {
System.err.println("Unable to load ConnectionFactory.properties:" +
ex.getMessage());
ex.printStackTrace();
}
return null;
}

/**
* Get the configuration file's real physical path.
*/
private static String getConfigurationFilePath() {
return StringUtil.getRealFilePath("/ConnectionFactory.properties");
}

/**
* Save string content of a java.util.Properties object.
* 保存配置文件中的字符串.
*
* @param props configuration string
* @return operation result
*/
protected static boolean saveConfigurationString(String props) {
if(props == null || props.length() <= 0) return false;
try {
FileWriter out = new FileWriter(getConfigurationFilePath());
out.write(props);
out.close();

return true;
} catch (Exception ex) {
System.err.println("Unable save configuration string to ConnectionFactory.properties:"
+ ex);
ex.printStackTrace();
}

return false;
}

/**
* Returns the current database connection properties.
* @return Properties object
*/
public static Properties getProperties() {
return props;
}

/**
* Save configuration properties.
*
* @param props Properties
* @return operation result
*/
public static boolean saveConfiguration(Properties props) {
if(props == null || props.size() <= 0) return false;

try {
FileOutputStream out = new FileOutputStream(getConfigurationFilePath());
props.store(out, "");
out.close();

return true;
} catch (Exception ex) {
System.err.println("Unable to save ConnectionFactory.properties:" + ex.getMessage());
ex.printStackTrace();
}

return false;
}

/**
* Create a DataSource instance based on the Apache DBCP.
* 创建基于 Apache DBCP 的 DataSource.
*
* @return a poolable DataSource
*/
public static DataSource setupDataSource() {
if(dataSource == null) {
BasicDataSource ds = new BasicDataSource();
ds.setDriverClassName(driver);
ds.setUsername(user);
ds.setPassword(password);
ds.setUrl(url);

dataSource = ds;
}

return dataSource;
}

/**
* Display connection status of current data source.
*
* 显示当前数据源的状态.
*/
public static String getDataSourceStats() {
BasicDataSource bds = (BasicDataSource) setupDataSource();
StringBuffer info = new StringBuffer();

info.append("Active connection numbers: " + bds.getNumActive());
info.append("\n");
info.append("Idle connection numbers: " + bds.getNumIdle());

return info.toString();
}

/**
* Shut down the data source, if want use it again,
* please call setupDataSource().
*/
public static void shutdownDataSource() {
BasicDataSource bds = (BasicDataSource) setupDataSource();
try {
bds.close();
} catch (SQLException e) {
// TODO auto generated try-catch
e.printStackTrace();
}
}

/**
* Restart the data source.
* 重新启动数据源.
*/
public static void restartDataSource() {
shutdownDataSource();
setupDataSource();
}

/** Test method */
public static void main(String[] args) {
Connection conn = ConnectionFactory.getConnection();
DatabaseUtil dbUtil = new DatabaseUtil();
dbUtil.setConnection(conn);
// try {
// java.sql.ResultSet rs = conn.createStatement().executeQuery(
// "SELECT MAX(ID) FROM items");
//
// while(rs.next()) {
// System.out.println(rs.getString(1));
// }
//
// rs.close();
//
// } catch (Exception ex) {
// ex.printStackTrace();
// }
System.out.println(dbUtil.getAllCount("SELECT MAX(ID) FROM items"));

System.out.println(conn);

try {
conn.close();
} catch (Exception ex) {
// ex.printStackTrace();
}

conn = ConnectionFactory.getConnection();

System.out.println(conn);

try {
conn.close();
} catch (Exception ex) {
// ex.printStackTrace();
}

System.exit(0);
}

}
posted on 2007-01-19 11:04 [BeanSoft](http://www.blogjava.net/beansoft/) 阅读(2136) [评论(1)](http://www.blogjava.net/beansoft/archive/2007/01/19/94882.html#Post)  [编辑](http://www.blogjava.net/beansoft/admin/EditPosts.aspx?postid=94882)  [收藏](http://www.blogjava.net/beansoft/AddToFavorite.aspx?id=94882) 所属分类: [Database](http://www.blogjava.net/beansoft/category/21451.html)![]()

[]()[]()

[Comments

* [#](http://www.blogjava.net/beansoft/archive/2007/01/19/94882.html#95631 "permalink: re: 基于 Apache DBCP 的数据库连接获取类(原创)") []()re: 基于 Apache DBCP 的数据库连接获取类(原创)[]()[小车马](http://www.blogjava.net/liubowu/)
Posted @ 2007-01-23 23:38
恩，以前也用过，apache的很多东西都非常不错，
楼主，潜力贴论坛（[http://content.uu1001.com/](http://content.uu1001.com/)）是我个人的一个设想，如果你对java非常的专注，并且愿意交我这个朋友，可以发邮件给我（lbw070105@gmail.com），希望我们可以一起发展它。  [回复](http://www.blogjava.net/beansoft/archive/2007/01/19/94882.html#post)  [更多评论](http://www.blogjava.net/comment?author=%e5%b0%8f%e8%bd%a6%e9%a9%ac "查看该作者发表过的评论") []()  []()
]()
[刷新评论列表]()  

[]() 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/beansoft/archive/2007/01/19/94882.html&SourceURL=/beansoft/archive/2007/01/19/94882.html)  [使用高级评论](http://www.blogjava.net/beansoft/archive/2007/01/19/94882.html?login=1#Post)  [新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [返回页首](http://www.blogjava.net/beansoft/archive/2007/01/19/94882.html#Top)  [恢复上次提交](http://www.blogjava.net/beansoft/archive/2007/01/19/94882.html#Post)       [使用Ctrl+Enter键可以直接提交] 该文被作者在 2007-09-22 12:56 编辑过      相关文章:

* [基于_JDBC_2.0_驱动的分页代码实现](http://www.blogjava.net/beansoft/archive/2007/10/23/155318.html)
* [BeanSoft MySQL Java 开发套装(服务器,管理工具,JDBC驱动,示例代码) 无中文问题](http://www.blogjava.net/beansoft/archive/2007/10/01/150075.html)
* [SQL中主键和外键的区别?(zz)](http://www.blogjava.net/beansoft/archive/2007/09/24/147657.html)
* [常用 JDBC 驱动名字和 URL 列表](http://www.blogjava.net/beansoft/archive/2007/09/22/147366.html)
* [如何连接SQL Server数据库(Tomcat 连接池配置) (转载)](http://www.blogjava.net/beansoft/archive/2007/09/18/146115.html)
* [Oracle Sequence 和 java.sql.PreparedStatement 的协同使用](http://www.blogjava.net/beansoft/archive/2007/07/11/129652.html)
* [学习 SQL 语法的好资料: Transact-SQL 参考](http://www.blogjava.net/beansoft/archive/2007/07/10/129354.html)
* [Hibernate 如何配置显示生成的 SQL?](http://www.blogjava.net/beansoft/archive/2007/07/10/129329.html)
* [Mysql 优化的资料(收集转载)](http://www.blogjava.net/beansoft/archive/2007/04/09/109432.html)
* [MySQL 5 绿色版(BAT版本)](http://www.blogjava.net/beansoft/archive/2007/02/05/97940.html)
 

Powered by:
[BlogJava](http://www.blogjava.net/)
Copyright © BeanSoft
