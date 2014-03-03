---
layout: post
title: "使用log4j显示quartz的debug信息 - 键盘是用来敲的不是看的 - ITeye技术网站"
categories: log4j
tags: 
 - log4j
--- 

# 使用log4j显示quartz的debug信息 - 键盘是用来敲的不是看的 - ITeye技术网站

[首页](http://www.iteye.com/) [新闻](http://www.iteye.com/news) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [招聘](http://www.iteye.com/job) [更多 ▼](http://corejava2008.iteye.com/blog/871045#)

[群组](http://www.iteye.com/groups) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://corejava2008.iteye.com/login "登录") [我的应用](http://www.iteye.com/all) [登录](http://corejava2008.iteye.com/login) [注册](http://corejava2008.iteye.com/signup)

# [键盘是用来敲的不是看的](http://corejava2008.iteye.com/)

永久域名 [http://corejava2008.iteye.com](http://corejava2008.iteye.com/)

[Quartz Job Scheduling Framework](http://corejava2008.iteye.com/blog/871094 "Quartz Job Scheduling Framework") | [spring 通过配置向quartz 注入service](http://corejava2008.iteye.com/blog/870966 "spring 通过配置向quartz 注入service")

2011-01-14

### [使用log4j显示quartz的debug信息]()

**文章分类:[Java编程](http://www.iteye.com/blogs/category/java)**
非常的简单，在log4中设置quartz的显示级别就可以啦。
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. log4j.rootLogger=INFO, Console  
1. log4j.appender.Console=org.apache.log4j.ConsoleAppender  
1. log4j.appender.Console.layout=org.apache.log4j.PatternLayout  
1. log4j.appender.Console.layout.ConversionPattern=(%r ms) [%t] %-5p: %c#%M %x: %m%n  
1. log4j.logger.com.genuitec.eclipse.sqlexplorer=WARN  
1. log4j.logger.org.apache=WARN  
1. log4j.logger.org.hibernate=WARN  
1. log4j.logger.org.hibernate.sql=WARN  
1. log4j.appender.R=org.apache.log4j.RollingFileAppender  
1. log4j.appender.R.File=${catalina.home}/logs/out.log    
1. log4j.appender.R.MaxFileSize=1024KB     
1. log4j.appender.R.MaxBackupIndex=1     
1. log4j.appender.R.layout=org.apache.log4j.PatternLayout  
1. log4j.appender.R.layout.ConversionPattern=%p %t %c - %m%n  
1. log4j.logger.org.quartz=DEBUG  
log4j.rootLogger=INFO, Console log4j.appender.Console=org.apache.log4j.ConsoleAppender log4j.appender.Console.layout=org.apache.log4j.PatternLayout log4j.appender.Console.layout.ConversionPattern=(%r ms) [%t] %-5p: %c#%M %x: %m%n log4j.logger.com.genuitec.eclipse.sqlexplorer=WARN log4j.logger.org.apache=WARN log4j.logger.org.hibernate=WARN log4j.logger.org.hibernate.sql=WARN log4j.appender.R=org.apache.log4j.RollingFileAppender log4j.appender.R.File=${catalina.home}/logs/out.log log4j.appender.R.MaxFileSize=1024KB log4j.appender.R.MaxBackupIndex=1 log4j.appender.R.layout=org.apache.log4j.PatternLayout log4j.appender.R.layout.ConversionPattern=%p %t %c - %m%n log4j.logger.org.quartz=DEBUG
设置quartz的log信息为DEBUG级别

[Quartz Job Scheduling Framework](http://corejava2008.iteye.com/blog/871094 "Quartz Job Scheduling Framework") | [spring 通过配置向quartz 注入service](http://corejava2008.iteye.com/blog/870966 "spring 通过配置向quartz 注入service")
* 11:20
* 浏览 (120)
* [评论](http://corejava2008.iteye.com/blog/871045#comments) (0)
* 分类: [quartz](http://corejava2008.iteye.com/category/140328)
* [相关推荐](http://www.iteye.com/wiki/topic/871045)

### 评论

[]()
### 发表评论

### 表情图标

![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()

字体颜色: 标准深红红色橙色棕色黄色绿色橄榄青色蓝色深蓝靛蓝紫色灰色白色黑色 字体大小: 标准1 (xx-small)2 (x-small)3 (small)4 (medium)5 (large)6 (x-large)7 (xx-large) 对齐: 标准居左居中居右

代码: [code="ruby"]...[/code] (支持java, ruby, js, xml, html, php, python, c, c++, c#, sql)

您还没有登录，请[登录](http://corejava2008.iteye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![corejava2008的博客]( "corejava2008的博客: 键盘是用来敲的不是看的")](http://corejava2008.iteye.com/)

corejava2008

* 浏览: 1200 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 上海
* ![]()
* [详细资料](http://corejava2008.iteye.com/blog/profile) [留言簿](http://corejava2008.iteye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://corejava2008.iteye.com/blog/user_visits)

[![KATHY_123的博客]( "KATHY_123的博客: ")](http://kathy-123.iteye.com/)

[KATHY_123](http://kathy-123.iteye.com/)

[![songfantasy的博客]( "songfantasy的博客: 一路向北")](http://songfantasy.iteye.com/)

[songfantasy](http://songfantasy.iteye.com/)
[![chenlk823的博客]( "chenlk823的博客: 黑夜中的流星")](http://chlk823.iteye.com/)

[chenlk823](http://chlk823.iteye.com/)

[![xrqsjj的博客]( "xrqsjj的博客: xrqsjj")](http://xrqsjj.iteye.com/)

[xrqsjj](http://xrqsjj.iteye.com/)

### 博客分类

* [全部博客 (13)](http://corejava2008.iteye.com/)
* [Maven (2)](http://corejava2008.iteye.com/category/137745)
* [J2SE (0)](http://corejava2008.iteye.com/category/137746)
* [Ant (2)](http://corejava2008.iteye.com/category/139060)
* [Web (0)](http://corejava2008.iteye.com/category/139795)
* [spring (1)](http://corejava2008.iteye.com/category/140285)
* [quartz (2)](http://corejava2008.iteye.com/category/140328)
* [jquery (2)](http://corejava2008.iteye.com/category/140359)
* [JavaScript (1)](http://corejava2008.iteye.com/category/141025)
* [hibernate (2)](http://corejava2008.iteye.com/category/141550)
* [liunx (1)](http://corejava2008.iteye.com/category/145184)
### 其他分类

* [我的收藏](http://corejava2008.iteye.com/blog/favorite) (5)
* [我的代码](http://corejava2008.iteye.com/blog/code_favorite) (0)
* [我的论坛主题帖](http://corejava2008.iteye.com/blog/topic) (1)
* [我的所有论坛帖](http://corejava2008.iteye.com/blog/post) (8)
* [我的精华良好帖](http://corejava2008.iteye.com/blog/article) (0)

### 最近加入群组

* [WebServices](http://webservices.group.iteye.com/)
### 存档

* [2011-02](http://corejava2008.iteye.com/blog/monthblog/2011-02) (2)
* [2011-01](http://corejava2008.iteye.com/blog/monthblog/2011-01) (10)
* [2010-12](http://corejava2008.iteye.com/blog/monthblog/2010-12) (1)
* [更多存档...](http://corejava2008.iteye.com/blog/monthblog_more)

### 评论排行榜

* [spring 通过配置向quartz 注入service](http://corejava2008.iteye.com/blog/870966 "spring 通过配置向quartz 注入service")
* [Ant入门教程二，使用Ant自动生成War文件 ...](http://corejava2008.iteye.com/blog/859708 "Ant入门教程二，使用Ant自动生成War文件,并部署到Tomcat下")
* [MappingException:Repeated column in mapp ...](http://corejava2008.iteye.com/blog/895007 "MappingException:Repeated column in mapping for entity")
* [Ant入门教程，使用Ant自动生成JAR文件](http://corejava2008.iteye.com/blog/859701 "Ant入门教程，使用Ant自动生成JAR文件")
* [使用Maven执行Java代码](http://corejava2008.iteye.com/blog/933186 "使用Maven执行Java代码")
* [![Rss]()](http://corejava2008.iteye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://corejava2008.iteye.com/rss)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 ]
![]()
