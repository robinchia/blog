---
layout: post
title: "通过java获取mac地址 - Terry.B.Li 彬 - BlogJava"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# 通过java获取mac地址 - Terry.B.Li 彬 - BlogJava

# [Terry.B.Li 彬](http://www.blogjava.net/libin2722/)

虚其心，可解天下之问；专其心，可治天下之学；静其心，可悟天下之理；恒其心，可成天下之业。

  [BlogJava](http://www.blogjava.net/) :: [首页](http://www.blogjava.net/libin2722/) :: [新随笔](http://www.blogjava.net/libin2722/admin/EditPosts.aspx?opt=1) :: [联系](http://www.blogjava.net/libin2722/contact.aspx?id=1) :: [聚合](http://www.blogjava.net/libin2722/rss) [![]()](http://www.blogjava.net/libin2722/rss) :: [管理](http://www.blogjava.net/libin2722/admin/EditPosts.aspx) :: ![]()   140 随笔 :: 314 文章 :: 90 评论 :: 0 Trackbacks

[<]( "Go to the previous month")2012年11月[>]( "Go to the next month")日一二三四五六2829303112345678910111213141516171819202122232425262728293012345678

*

### 常用链接

* [我的随笔](http://www.blogjava.net/libin2722/MyPosts.html)
* [我的文章](http://www.blogjava.net/libin2722/MyArticles.html)
* [我的评论](http://www.blogjava.net/libin2722/MyComments.html)
* [我的参与](http://www.blogjava.net/libin2722/OtherPosts.html)
* [最新评论](http://www.blogjava.net/libin2722/RecentComments.html)

### 留言簿(8)

* [给我留言](http://www.blogjava.net/libin2722/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/libin2722/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/libin2722/admin/MyMessages.aspx)

### 随笔分类(107)

* [CA(16)](http://www.blogjava.net/libin2722/category/29421.html) [(rss)](http://www.blogjava.net/libin2722/category/29421.html/rss "Subscribe to CA(16)")
* [Extremecomponents (1)](http://www.blogjava.net/libin2722/category/29914.html) [(rss)](http://www.blogjava.net/libin2722/category/29914.html/rss "Subscribe to Extremecomponents (1)")
* [ibatis(4)](http://www.blogjava.net/libin2722/category/27810.html) [(rss)](http://www.blogjava.net/libin2722/category/27810.html/rss "Subscribe to ibatis(4)")
* [Jakarta(9)](http://www.blogjava.net/libin2722/category/29422.html) [(rss)](http://www.blogjava.net/libin2722/category/29422.html/rss "Subscribe to Jakarta(9)")
* [Java(19)](http://www.blogjava.net/libin2722/category/25647.html) [(rss)](http://www.blogjava.net/libin2722/category/25647.html/rss "Subscribe to Java(19)")
* [Liferay(21)](http://www.blogjava.net/libin2722/category/29634.html) [(rss)](http://www.blogjava.net/libin2722/category/29634.html/rss "Subscribe to Liferay(21)")
* [maven2(15)](http://www.blogjava.net/libin2722/category/28813.html) [(rss)](http://www.blogjava.net/libin2722/category/28813.html/rss "Subscribe to maven2(15)")
* [postgresql(2)](http://www.blogjava.net/libin2722/category/29635.html) [(rss)](http://www.blogjava.net/libin2722/category/29635.html/rss "Subscribe to postgresql(2)")
* [sitemesh](http://www.blogjava.net/libin2722/category/28539.html) [(rss)](http://www.blogjava.net/libin2722/category/28539.html/rss "Subscribe to sitemesh")
* [spring2.0](http://www.blogjava.net/libin2722/category/28538.html) [(rss)](http://www.blogjava.net/libin2722/category/28538.html/rss "Subscribe to spring2.0")
* [struts2.0](http://www.blogjava.net/libin2722/category/28537.html) [(rss)](http://www.blogjava.net/libin2722/category/28537.html/rss "Subscribe to struts2.0")
* [struts-menu(1)](http://www.blogjava.net/libin2722/category/28540.html) [(rss)](http://www.blogjava.net/libin2722/category/28540.html/rss "Subscribe to struts-menu(1)")
* [webservice(17)](http://www.blogjava.net/libin2722/category/27390.html) [(rss)](http://www.blogjava.net/libin2722/category/27390.html/rss "Subscribe to webservice(17)")
* [设计模式(2)](http://www.blogjava.net/libin2722/category/30973.html) [(rss)](http://www.blogjava.net/libin2722/category/30973.html/rss "Subscribe to 设计模式(2)")

### 随笔档案(139)

* [2009年12月 (1)](http://www.blogjava.net/libin2722/archive/2009/12.html)
* [2009年9月 (3)](http://www.blogjava.net/libin2722/archive/2009/09.html)
* [2009年2月 (1)](http://www.blogjava.net/libin2722/archive/2009/02.html)
* [2008年12月 (2)](http://www.blogjava.net/libin2722/archive/2008/12.html)
* [2008年11月 (1)](http://www.blogjava.net/libin2722/archive/2008/11.html)
* [2008年9月 (2)](http://www.blogjava.net/libin2722/archive/2008/09.html)
* [2008年8月 (2)](http://www.blogjava.net/libin2722/archive/2008/08.html)
* [2008年5月 (1)](http://www.blogjava.net/libin2722/archive/2008/05.html)
* [2008年3月 (22)](http://www.blogjava.net/libin2722/archive/2008/03.html)
* [2008年2月 (34)](http://www.blogjava.net/libin2722/archive/2008/02.html)
* [2008年1月 (14)](http://www.blogjava.net/libin2722/archive/2008/01.html)
* [2007年12月 (7)](http://www.blogjava.net/libin2722/archive/2007/12.html)
* [2007年11月 (35)](http://www.blogjava.net/libin2722/archive/2007/11.html)
* [2007年10月 (1)](http://www.blogjava.net/libin2722/archive/2007/10.html)
* [2007年9月 (13)](http://www.blogjava.net/libin2722/archive/2007/09.html)

### 文章分类(255)

* [ActiveMQ(7)](http://www.blogjava.net/libin2722/category/46220.html) [(rss)](http://www.blogjava.net/libin2722/category/46220.html/rss "Subscribe to ActiveMQ(7)")
* [Ajax(4)](http://www.blogjava.net/libin2722/category/27087.html) [(rss)](http://www.blogjava.net/libin2722/category/27087.html/rss "Subscribe to Ajax(4)")
* [Axis(21)](http://www.blogjava.net/libin2722/category/27086.html) [(rss)](http://www.blogjava.net/libin2722/category/27086.html/rss "Subscribe to Axis(21)")
* [cache(5)](http://www.blogjava.net/libin2722/category/37910.html) [(rss)](http://www.blogjava.net/libin2722/category/37910.html/rss "Subscribe to cache(5)")
* [chat(1)](http://www.blogjava.net/libin2722/category/37288.html) [(rss)](http://www.blogjava.net/libin2722/category/37288.html/rss "Subscribe to chat(1)")
* [css](http://www.blogjava.net/libin2722/category/26105.html) [(rss)](http://www.blogjava.net/libin2722/category/26105.html/rss "Subscribe to css")
* [DataBase(5)](http://www.blogjava.net/libin2722/category/25650.html) [(rss)](http://www.blogjava.net/libin2722/category/25650.html/rss "Subscribe to DataBase(5)")
* [Dwr(3)](http://www.blogjava.net/libin2722/category/30614.html) [(rss)](http://www.blogjava.net/libin2722/category/30614.html/rss "Subscribe to Dwr(3)")
* [ejb3.0(4)](http://www.blogjava.net/libin2722/category/26107.html) [(rss)](http://www.blogjava.net/libin2722/category/26107.html/rss "Subscribe to ejb3.0(4)")
* [ESB(3)](http://www.blogjava.net/libin2722/category/41751.html) [(rss)](http://www.blogjava.net/libin2722/category/41751.html/rss "Subscribe to ESB(3)")
* [ESB(4)](http://www.blogjava.net/libin2722/category/41752.html) [(rss)](http://www.blogjava.net/libin2722/category/41752.html/rss "Subscribe to ESB(4)")
* [flex3(1)](http://www.blogjava.net/libin2722/category/43606.html) [(rss)](http://www.blogjava.net/libin2722/category/43606.html/rss "Subscribe to flex3(1)")
* [Freemarker(2)](http://www.blogjava.net/libin2722/category/36802.html) [(rss)](http://www.blogjava.net/libin2722/category/36802.html/rss "Subscribe to Freemarker(2)")
* [Hibernate Search(2)](http://www.blogjava.net/libin2722/category/46237.html) [(rss)](http://www.blogjava.net/libin2722/category/46237.html/rss "Subscribe to Hibernate Search(2)")
* [ibatis + spring (11)](http://www.blogjava.net/libin2722/category/27870.html) [(rss)](http://www.blogjava.net/libin2722/category/27870.html/rss "Subscribe to ibatis + spring (11)")
* [java(44)](http://www.blogjava.net/libin2722/category/25649.html) [(rss)](http://www.blogjava.net/libin2722/category/25649.html/rss "Subscribe to java(44)")
* [javascript(7)](http://www.blogjava.net/libin2722/category/26106.html) [(rss)](http://www.blogjava.net/libin2722/category/26106.html/rss "Subscribe to javascript(7)")
* [JBoss(5)](http://www.blogjava.net/libin2722/category/32629.html) [(rss)](http://www.blogjava.net/libin2722/category/32629.html/rss "Subscribe to JBoss(5)")
* [Jbpm(18)](http://www.blogjava.net/libin2722/category/25641.html) [(rss)](http://www.blogjava.net/libin2722/category/25641.html/rss "Subscribe to Jbpm(18)")
* [JBPM4(6)](http://www.blogjava.net/libin2722/category/40764.html) [(rss)](http://www.blogjava.net/libin2722/category/40764.html/rss "Subscribe to JBPM4(6)")
* [JMS(6)](http://www.blogjava.net/libin2722/category/37068.html) [(rss)](http://www.blogjava.net/libin2722/category/37068.html/rss "Subscribe to JMS(6)")
* [JPA(1)](http://www.blogjava.net/libin2722/category/46221.html) [(rss)](http://www.blogjava.net/libin2722/category/46221.html/rss "Subscribe to JPA(1)")
* [jquery(5)](http://www.blogjava.net/libin2722/category/31506.html) [(rss)](http://www.blogjava.net/libin2722/category/31506.html/rss "Subscribe to jquery(5)")
* [Jsp(5)](http://www.blogjava.net/libin2722/category/25732.html) [(rss)](http://www.blogjava.net/libin2722/category/25732.html/rss "Subscribe to Jsp(5)")
* [Liferay(5)](http://www.blogjava.net/libin2722/category/36801.html) [(rss)](http://www.blogjava.net/libin2722/category/36801.html/rss "Subscribe to Liferay(5)")
* [Log(6)](http://www.blogjava.net/libin2722/category/40802.html) [(rss)](http://www.blogjava.net/libin2722/category/40802.html/rss "Subscribe to Log(6)")
* [Maven2(9)](http://www.blogjava.net/libin2722/category/37136.html) [(rss)](http://www.blogjava.net/libin2722/category/37136.html/rss "Subscribe to Maven2(9)")
* [mysql(2)](http://www.blogjava.net/libin2722/category/40888.html) [(rss)](http://www.blogjava.net/libin2722/category/40888.html/rss "Subscribe to mysql(2)")
* [soa(1)](http://www.blogjava.net/libin2722/category/27239.html) [(rss)](http://www.blogjava.net/libin2722/category/27239.html/rss "Subscribe to soa(1)")
* [soap(5)](http://www.blogjava.net/libin2722/category/27231.html) [(rss)](http://www.blogjava.net/libin2722/category/27231.html/rss "Subscribe to soap(5)")
* [spring(7)](http://www.blogjava.net/libin2722/category/26104.html) [(rss)](http://www.blogjava.net/libin2722/category/26104.html/rss "Subscribe to spring(7)")
* [Spring Security(2)](http://www.blogjava.net/libin2722/category/46078.html) [(rss)](http://www.blogjava.net/libin2722/category/46078.html/rss "Subscribe to Spring Security(2)")
* [SSH(3)](http://www.blogjava.net/libin2722/category/27232.html) [(rss)](http://www.blogjava.net/libin2722/category/27232.html/rss "Subscribe to SSH(3)")
* [struts2.0(7)](http://www.blogjava.net/libin2722/category/26765.html) [(rss)](http://www.blogjava.net/libin2722/category/26765.html/rss "Subscribe to struts2.0(7)")
* [svn(2)](http://www.blogjava.net/libin2722/category/39922.html) [(rss)](http://www.blogjava.net/libin2722/category/39922.html/rss "Subscribe to svn(2)")
* [web service(20)](http://www.blogjava.net/libin2722/category/25733.html) [(rss)](http://www.blogjava.net/libin2722/category/25733.html/rss "Subscribe to web service(20)")
* [WebWork(1)](http://www.blogjava.net/libin2722/category/26100.html) [(rss)](http://www.blogjava.net/libin2722/category/26100.html/rss "Subscribe to WebWork(1)")
* [Web前端(1)](http://www.blogjava.net/libin2722/category/47429.html) [(rss)](http://www.blogjava.net/libin2722/category/47429.html/rss "Subscribe to Web前端(1)")
* [wireless(1)](http://www.blogjava.net/libin2722/category/40763.html) [(rss)](http://www.blogjava.net/libin2722/category/40763.html/rss "Subscribe to wireless(1)")
* [wsdl(1)](http://www.blogjava.net/libin2722/category/27233.html) [(rss)](http://www.blogjava.net/libin2722/category/27233.html/rss "Subscribe to wsdl(1)")
* [yav(1)](http://www.blogjava.net/libin2722/category/37296.html) [(rss)](http://www.blogjava.net/libin2722/category/37296.html/rss "Subscribe to yav(1)")
* [报表(3)](http://www.blogjava.net/libin2722/category/25734.html) [(rss)](http://www.blogjava.net/libin2722/category/25734.html/rss "Subscribe to 报表(3)")
* [模板(5)](http://www.blogjava.net/libin2722/category/34664.html) [(rss)](http://www.blogjava.net/libin2722/category/34664.html/rss "Subscribe to 模板(5)")
* [设计模式(2)](http://www.blogjava.net/libin2722/category/26101.html) [(rss)](http://www.blogjava.net/libin2722/category/26101.html/rss "Subscribe to 设计模式(2)")
* [通信(1)](http://www.blogjava.net/libin2722/category/46223.html) [(rss)](http://www.blogjava.net/libin2722/category/46223.html/rss "Subscribe to 通信(1)")

### 文章档案(312)

* [2011年8月 (4)](http://www.blogjava.net/libin2722/archives/2011/08.html)
* [2011年7月 (1)](http://www.blogjava.net/libin2722/archives/2011/07.html)
* [2011年6月 (6)](http://www.blogjava.net/libin2722/archives/2011/06.html)
* [2011年5月 (2)](http://www.blogjava.net/libin2722/archives/2011/05.html)
* [2011年4月 (2)](http://www.blogjava.net/libin2722/archives/2011/04.html)
* [2011年3月 (4)](http://www.blogjava.net/libin2722/archives/2011/03.html)
* [2011年2月 (2)](http://www.blogjava.net/libin2722/archives/2011/02.html)
* [2011年1月 (3)](http://www.blogjava.net/libin2722/archives/2011/01.html)
* [2010年12月 (8)](http://www.blogjava.net/libin2722/archives/2010/12.html)
* [2010年11月 (10)](http://www.blogjava.net/libin2722/archives/2010/11.html)
* [2010年10月 (7)](http://www.blogjava.net/libin2722/archives/2010/10.html)
* [2010年9月 (32)](http://www.blogjava.net/libin2722/archives/2010/09.html)
* [2010年8月 (6)](http://www.blogjava.net/libin2722/archives/2010/08.html)
* [2010年7月 (1)](http://www.blogjava.net/libin2722/archives/2010/07.html)
* [2010年6月 (2)](http://www.blogjava.net/libin2722/archives/2010/06.html)
* [2010年5月 (1)](http://www.blogjava.net/libin2722/archives/2010/05.html)
* [2010年4月 (1)](http://www.blogjava.net/libin2722/archives/2010/04.html)
* [2010年3月 (1)](http://www.blogjava.net/libin2722/archives/2010/03.html)
* [2010年2月 (1)](http://www.blogjava.net/libin2722/archives/2010/02.html)
* [2010年1月 (2)](http://www.blogjava.net/libin2722/archives/2010/01.html)
* [2009年12月 (1)](http://www.blogjava.net/libin2722/archives/2009/12.html)
* [2009年11月 (1)](http://www.blogjava.net/libin2722/archives/2009/11.html)
* [2009年10月 (5)](http://www.blogjava.net/libin2722/archives/2009/10.html)
* [2009年9月 (23)](http://www.blogjava.net/libin2722/archives/2009/09.html)
* [2009年8月 (6)](http://www.blogjava.net/libin2722/archives/2009/08.html)
* [2009年7月 (18)](http://www.blogjava.net/libin2722/archives/2009/07.html)
* [2009年6月 (2)](http://www.blogjava.net/libin2722/archives/2009/06.html)
* [2009年5月 (5)](http://www.blogjava.net/libin2722/archives/2009/05.html)
* [2009年4月 (5)](http://www.blogjava.net/libin2722/archives/2009/04.html)
* [2009年2月 (9)](http://www.blogjava.net/libin2722/archives/2009/02.html)
* [2009年1月 (19)](http://www.blogjava.net/libin2722/archives/2009/01.html)
* [2008年12月 (13)](http://www.blogjava.net/libin2722/archives/2008/12.html)
* [2008年10月 (1)](http://www.blogjava.net/libin2722/archives/2008/10.html)
* [2008年9月 (3)](http://www.blogjava.net/libin2722/archives/2008/09.html)
* [2008年8月 (2)](http://www.blogjava.net/libin2722/archives/2008/08.html)
* [2008年7月 (5)](http://www.blogjava.net/libin2722/archives/2008/07.html)
* [2008年6月 (5)](http://www.blogjava.net/libin2722/archives/2008/06.html)
* [2008年5月 (3)](http://www.blogjava.net/libin2722/archives/2008/05.html)
* [2008年4月 (6)](http://www.blogjava.net/libin2722/archives/2008/04.html)
* [2007年12月 (9)](http://www.blogjava.net/libin2722/archives/2007/12.html)
* [2007年11月 (32)](http://www.blogjava.net/libin2722/archives/2007/11.html)
* [2007年10月 (2)](http://www.blogjava.net/libin2722/archives/2007/10.html)
* [2007年9月 (41)](http://www.blogjava.net/libin2722/archives/2007/09.html)

### 相册

* [我的相册](http://www.blogjava.net/libin2722/gallery/27199.html)

### 收藏夹(56)

* [我的收藏(56)](http://www.blogjava.net/libin2722/favorite/25932.html) [(rss)](http://www.blogjava.net/libin2722/favorite/25932.html/rss "Subscribe to 我的收藏(56)")

### 家装

* [★榻榻米卡座衣帽间★田园暖家硬装完毕上软装咯](http://bbs.taobao.com/catalog/thread/154507-251198006-18-36320967.htm)
* [淘金币韩版短款小棉袄甜美棉服拉链厚外套面包棉衣秋冬装女装新款](http://item.taobao.com/item.htm?id=12772334301)

### 最新随笔

* [1. flex摄像头拍照 java上传到数据库 .](http://www.blogjava.net/libin2722/articles/355899.html)
* [2. Nginx+Tomcat+Memcached共享session集群配置](http://www.blogjava.net/libin2722/articles/355897.html)
* [3. 基于词典的正向最大匹配中文分词算法，能实现中英文数字混合分词](http://www.blogjava.net/libin2722/articles/355838.html)
* [4. linux下Nginx+tomcat整合的安装与配置](http://www.blogjava.net/libin2722/articles/355631.html)
* [5. 从 iBatis 到 MyBatis - MyBatis 简明学习教程](http://www.blogjava.net/libin2722/articles/354030.html)
* [6. Apache Http Server与Tomcat实现负载均衡和集群](http://www.blogjava.net/libin2722/articles/352842.html)
* [7. linux+nginx+tomcat负载均衡，实现session同步](http://www.blogjava.net/libin2722/articles/352841.html)
* [8. 通过java获取mac地址]()
* [9. 通过JAVA获取优酷、土豆、酷6、6间房等视频](http://www.blogjava.net/libin2722/articles/351642.html)
* [10. 使用 jsoup 对 HTML 文档进行解析和操作](http://www.blogjava.net/libin2722/articles/351641.html)
* [11. Installing Apache 2.2.11, PHP 5.3, MySQL 5.1.36 & PhpMyAdmin 3.2 in Windows 7/Vista/XP](http://www.blogjava.net/libin2722/articles/351579.html)
* [12. 让Apache Shiro保护你的应用](http://www.blogjava.net/libin2722/articles/350335.html)
* [13. .htaccess文件](http://www.blogjava.net/libin2722/articles/350049.html)
* [14. Windows系统Eclipse中集成的Tomcat的Java虚拟机属性设置](http://www.blogjava.net/libin2722/articles/348724.html)
* [15. Lucene-2.2.0 源代码阅读学习](http://www.blogjava.net/libin2722/articles/347516.html)
* [16. 常用SNS开源系统比较](http://www.blogjava.net/libin2722/articles/347036.html)
* [17. NoSQL数据库笔谈](http://www.blogjava.net/libin2722/articles/346055.html)
* [18. TUP第二期人人网张铁安：Feed系统架构分析](http://www.blogjava.net/libin2722/articles/346051.html)
* [19. 做SNS的，一起来猜猜新浪微博的核心Feed系统是怎么设计的吧](http://www.blogjava.net/libin2722/articles/346050.html)
* [20. Apache模块 mod_rewrite](http://www.blogjava.net/libin2722/articles/344405.html)
* [21. 串讲23种设计模式](http://www.blogjava.net/libin2722/articles/344406.html)
* [22. 一些常见异常](http://www.blogjava.net/libin2722/articles/343119.html)
* [23. 10个给力的在线Web设计开发工具介绍](http://www.blogjava.net/libin2722/articles/342744.html)
* [24. OAUTH协议简介](http://www.blogjava.net/libin2722/articles/342484.html)
* [25. 十个jQuery图片画廊插件推荐](http://www.blogjava.net/libin2722/articles/341947.html)
* [26. GWT基础框架使用](http://www.blogjava.net/libin2722/articles/341287.html)
* [27. GWT表格搭建](http://www.blogjava.net/libin2722/articles/341286.html)
* [28. GWT--MVC的简单介绍](http://www.blogjava.net/libin2722/articles/341285.html)
* [29. GWT1.6+Spring3.0+RESTlet+Jsonlib+Maven2+openCRX/MDX](http://www.blogjava.net/libin2722/articles/341122.html)
* [30. GXT（Ext-Gwt）例子的创建、配置、部署心得](http://www.blogjava.net/libin2722/articles/341105.html)
* [31. GWT + Spring MVC + JPA整合](http://www.blogjava.net/libin2722/articles/341101.html)
* [32.  geohash: 一个实用的geocoding方法](http://www.blogjava.net/libin2722/articles/340934.html)
* [33. 烧烤配方](http://www.blogjava.net/libin2722/articles/338599.html)
* [34. 纯真IP数据库格式详解](http://www.blogjava.net/libin2722/articles/338551.html)
* [35. java读取纯真IP数据库QQwry.dat的源代码](http://www.blogjava.net/libin2722/articles/338543.html)
* [36. 图片截取和缩略](http://www.blogjava.net/libin2722/articles/338536.html)
* [37. 30款ajax特效，针对Lightbox和Modal Dialog](http://www.blogjava.net/libin2722/articles/338535.html)
* [38. 中国主要城市DNS服务器IP地址列表](http://www.blogjava.net/libin2722/articles/338319.html)
* [39. 3行代码，实现IP到地理位置的反查功能](http://www.blogjava.net/libin2722/articles/338317.html)
* [40. JAVA解析纯真IP地址库](http://www.blogjava.net/libin2722/articles/338316.html)

### 搜索

*
*  

### 积分与排名

* 积分 - 230113
* 排名 - 98

### 最新评论 [![]()](http://www.blogjava.net/libin2722/CommentsRSS.aspx)

* [1. 文件位置好像放错了](http://www.blogjava.net/libin2722/archive/2012/11/20/162444.html#391642)
* 评论内容较长,点击标题查看
* --赵光培
* [2. re: OpenJPA](http://www.blogjava.net/libin2722/archive/2012/11/13/330635.html#391252)
* 真的是太全了。是在是太感谢了
* --tanzijie11
* [3. re: 从 iBatis 到 MyBatis - MyBatis 简明学习教程](http://www.blogjava.net/libin2722/archive/2012/10/17/354030.html#389750)
* o(￣▽￣)o
很好的一个技术总结
让我进一步的认识MyBatis，Good！！！
* --写字很丑星人
* [4. re: (转载)页面静态化(JSP动态页面转静态化)](http://www.blogjava.net/libin2722/archive/2012/09/14/252453.html#387728)
* 伪地址，没提升性能。提高了用户的感知度！这个动态页面静态化不科学。
* --路人
* [5. re: flex摄像头拍照 java上传到数据库 .](http://www.blogjava.net/libin2722/archive/2012/08/31/355899.html#386685)
* 你好，flex的需要引什么包吗
* --霞之大梦
* [6. re: JBPM数据库表说明](http://www.blogjava.net/libin2722/archive/2012/08/03/143249.html#384673)
* 挺不错的，不过怎么没有jbpm_hist_task这个表呢
* --文跃
* [7. re: (转载)页面静态化(JSP动态页面转静态化)[未登录]](http://www.blogjava.net/libin2722/archive/2012/06/11/252453.html#380515)
* 这个貌似没有静态化，哈哈！
* --Jerry
* [8. re: 打印出Ibatis最终的SQL语句](http://www.blogjava.net/libin2722/archive/2012/05/16/165153.html#378273)
* 学习了，解决问题的方法很好。我现在用的这个系统有这个功能。
* --王鹏飞
* [9. re: Lucene Syntax (lucene查询语法详解)(转)](http://www.blogjava.net/libin2722/archive/2012/04/16/332568.html#374763)
* 仅仅是翻译而已
* --guest
* [10. re: 权限控制：spring 3.0 security配置例子](http://www.blogjava.net/libin2722/archive/2012/03/12/332006.html#371727)
* 是生成了一个项目但是不能用的嘛
* --psc
* [11. re: JBPM数据库表说明](http://www.blogjava.net/libin2722/archive/2012/03/02/143249.html#371118)
* 谢谢！
* --醉雪
* [12. re: 从 iBatis 到 MyBatis - MyBatis 简明学习教程](http://www.blogjava.net/libin2722/archive/2011/11/27/354030.html#364908)
* 强人！！佩服！！
* --木易
* [13. re: 从 iBatis 到 MyBatis - MyBatis 简明学习教程](http://www.blogjava.net/libin2722/archive/2011/11/25/354030.html#364860)
* 好文，谢谢，又学到了一点
不过，还是建议楼主把代码的背景图片换一换，看的我眼花啊^_^
* --风铃中的刀声
* [14. re: 基于struts+spring+ibatis的 J2EE 开发(2)[未登录]](http://www.blogjava.net/libin2722/archive/2011/10/17/165550.html#361487)
* @毛毛
看得懂- -！
* --啊
* [15. re: （转）WebService大讲堂之Axis2（2）：复合类型数据的传递 [未登录]](http://www.blogjava.net/libin2722/archive/2011/09/01/296158.html#357700)
* 很有帮助，谢谢博主
* --David
* [16. re: ClassNotFoundException: org.hibernate.hql.ast.HqlToken](http://www.blogjava.net/libin2722/archive/2011/06/09/148815.html#351952)
* 那如果你Spring3+hibernate3+weblogic里有一句updete的更新语句就可以了
* --ss
* [17. re: .htaccess文件](http://www.blogjava.net/libin2722/archive/2011/05/26/350049.html#351079)
* .htaccess用处确实很多
* --就是上
* [18. re: (转载)页面静态化(JSP动态页面转静态化)](http://www.blogjava.net/libin2722/archive/2011/05/15/252453.html#350291)
* haha 这个不是静态化吧, 只是把url修饰了一下而已, 对性能什么的没有提高.
* --老胡
* [19. re: ibatis中由SELECT语句自动生成COUNT语句](http://www.blogjava.net/libin2722/archive/2011/04/25/197816.html#348987)
* 有没有通用的呀！！如果sql中包含子查询就不好用了
* --joliny
* [20. re: 在Struts2.0中如何得到绝对路径](http://www.blogjava.net/libin2722/archive/2011/04/19/171749.html#348545)
* getResourceAsStream()使用的是项目的相对路径,有没有使用绝对路径的?
* --两性知识,减肥方法,丰胸方法,祛雀斑方法
* [21. re: 权限控制：spring 3.0 security配置例子[未登录]](http://www.blogjava.net/libin2722/archive/2011/03/20/332006.html#346621)
* 谢谢，我也遇到同样的问题。
* --Joe
* [22. re: ibatis中由SELECT语句自动生成COUNT语句](http://www.blogjava.net/libin2722/archive/2011/01/17/197816.html#343090)
* 可以实现count查询，原则上会生成两条sql语句，一条是count一条是具体分页的，这个代码已经写了很早了，你最好进行debug跟踪一下就可以知道了
* --礼物
* [23. re: ibatis中由SELECT语句自动生成COUNT语句](http://www.blogjava.net/libin2722/archive/2011/01/13/197816.html#342919)
* 评论内容较长,点击标题查看
* --tylerLimin
* [24. re: jbpm oracle 数据库脚本](http://www.blogjava.net/libin2722/archive/2010/12/21/143256.html#341209)
* 放屁 这明明是mysql的
* --大jj
* [25. re: Apache Maven 2 简介(最全的文档)](http://www.blogjava.net/libin2722/archive/2010/08/18/174425.html#329247)
* 郁闷,图片看不到!一直提示登陆框!
* --逸飞
* [26. re: 论坛灌水机 -- HTTPClient[未登录]](http://www.blogjava.net/libin2722/archive/2010/08/16/179849.html#329001)
* 这个怎么都说是自己做的？
知道能否运行成功不就贴出来了？
* --an
* [27. re: HashMap 、HashTable、HashSet的区别 [未登录]](http://www.blogjava.net/libin2722/archive/2010/06/03/162853.html#322594)
* 评论内容较长,点击标题查看
* --我
* [28. re: JBPM数据库表说明](http://www.blogjava.net/libin2722/archive/2010/05/31/143249.html#322383)
* 好人！非常感谢！
* --小邓子
* [29. re: ClassNotFoundException: org.hibernate.hql.ast.HqlToken](http://www.blogjava.net/libin2722/archive/2009/11/21/148815.html#303147)
* @礼物
这样删除报错！！！！！！！
* --去
* [30. re: 使用maven2 打ear包](http://www.blogjava.net/libin2722/archive/2009/10/27/179037.html#299890)
* 评论内容较长,点击标题查看
* --Asran
* [31. re: 深入浅出Liferay Portal (12)](http://www.blogjava.net/libin2722/archive/2009/09/28/186285.html#296750)
* 欢迎加群49588979一起讨论学习liferay
* --liferay
* [32. re: ClassNotFoundException: org.hibernate.hql.ast.HqlToken[未登录]](http://www.blogjava.net/libin2722/archive/2009/09/27/148815.html#296584)
* 评论内容较长,点击标题查看
* --礼物
* [33. re: JBPM数据库表说明](http://www.blogjava.net/libin2722/archive/2009/09/18/143249.html#295591)
* 你就是个好人，我转载了阿，保留
* --acerbic coffee
* [34. re: ClassNotFoundException: org.hibernate.hql.ast.HqlToken](http://www.blogjava.net/libin2722/archive/2009/09/17/148815.html#295424)
* 两位老大你两演双黄那,到底怎么解决的呀
* --示范点
* [35. re: (转载)程立谈架构、敏捷和SOA实践](http://www.blogjava.net/libin2722/archive/2009/09/09/294202.html#294415)
* SOA现在好热呀～，不过没怎么搞明白。。。
* --买礼物
* [36. re: 在liferay中生成one to many 关系说明](http://www.blogjava.net/libin2722/archive/2009/09/09/247012.html#294413)
* 深奥呀～～
* --买礼物
* [37. re: (原创)yav - javascript validation tool](http://www.blogjava.net/libin2722/archive/2009/09/09/251702.html#294409)
* 谢谢
* --李彬
* [38. re: (原创)yav - javascript validation tool](http://www.blogjava.net/libin2722/archive/2009/09/08/251702.html#294349)
* 不错！支持你！
* --tj
* [39. re: 打印出Ibatis最终的SQL语句](http://www.blogjava.net/libin2722/archive/2009/08/21/165153.html#292026)
* =。= hibernate就比较方便了 showsql=true
ibatis解决方案我也找了好久
* --Illu
* [40. re: (原创)用Axis2写的通用WebService组件](http://www.blogjava.net/libin2722/archive/2009/08/10/250431.html#290552)
* 太强了
* --礼物

### 阅读排行榜

* [1. Axis1.4 利用 deploy.wsdd 发布 server-config.wsdd文件(4628)](http://www.blogjava.net/libin2722/archive/2007/11/25/163011.html)
* [2. server-config.wsdd配置一例(4626)](http://www.blogjava.net/libin2722/archive/2007/11/22/162436.html)
* [3. HashMap 、HashTable、HashSet的区别 (3377)](http://www.blogjava.net/libin2722/archive/2007/11/24/162853.html)
* [4. ClassNotFoundException: org.hibernate.hql.ast.HqlToken(3004)](http://www.blogjava.net/libin2722/archive/2007/09/27/148815.html)
* [5. 打印出Ibatis最终的SQL语句(2926)](http://www.blogjava.net/libin2722/archive/2007/12/04/165153.html)
* [6. Apache Maven 2 简介(最全的文档)(2505)](http://www.blogjava.net/libin2722/archive/2008/01/10/174425.html)
* [7. Liferay Portal二次开发指南(2074)](http://www.blogjava.net/libin2722/archive/2008/03/02/183216.html)
* [8. 一个服务返回一个ArrayList，如何使用Axis序列化/反序列化啊(1866)](http://www.blogjava.net/libin2722/archive/2007/11/24/162827.html)
* [9. 在Struts2.0中如何得到绝对路径(1609)](http://www.blogjava.net/libin2722/archive/2007/12/30/171749.html)
* [10. Struts2.0 中配置 Struts-Menu(1521)](http://www.blogjava.net/libin2722/archive/2008/01/04/172720.html)
* [11. Maven2 常用命令(1499)](http://www.blogjava.net/libin2722/archive/2008/01/08/173837.html)
* [12. Double：双精度类型(1389)](http://www.blogjava.net/libin2722/archive/2008/02/13/179857.html)
* [13. Axis1.4 开发笔记(1295)](http://www.blogjava.net/libin2722/archive/2007/11/22/162444.html)
* [14. 使用maven2 打ear包(1287)](http://www.blogjava.net/libin2722/archive/2008/02/02/179037.html)
* [15. ibatis中文与like的问题 (1230)](http://www.blogjava.net/libin2722/archive/2007/12/12/167220.html)
* [16. httpclient中MultipartPostMethod类上传文件(1186)](http://www.blogjava.net/libin2722/archive/2008/02/13/179848.html)
* [17. 深入浅出Liferay Portal (4) (1019)](http://www.blogjava.net/libin2722/archive/2008/03/14/186274.html)
* [18. 用axis发布webservices（一） (866)](http://www.blogjava.net/libin2722/archive/2007/11/24/162829.html)
* [19. JAVA中SSL证书认证通讯-Client(851)](http://www.blogjava.net/libin2722/archive/2008/02/13/179833.html)
* [20. 深入浅出Liferay Portal (12) (815)](http://www.blogjava.net/libin2722/archive/2008/03/14/186285.html)
* [21. 深入浅出Liferay Portal (3) (809)](http://www.blogjava.net/libin2722/archive/2008/03/14/186273.html)
* [22. Problem with Sybase, PostgreSQL and Timestamp columns(803)](http://www.blogjava.net/libin2722/archive/2008/02/24/181747.html)
* [23. 教程--开始使用Maven下(793)](http://www.blogjava.net/libin2722/archive/2007/12/24/169977.html)
* [24. 深入浅出Liferay Portal (1) (787)](http://www.blogjava.net/libin2722/archive/2008/03/14/186271.html)
* [25. 深入浅出Liferay Portal (2) (785)](http://www.blogjava.net/libin2722/archive/2008/03/14/186272.html)
* [26. 一个可以在页面上随意画线、多边形、圆，填充等功能的js (754)](http://www.blogjava.net/libin2722/archive/2007/10/06/150631.html)
* [27. portal专题（一）用liferay server简单开发portlet快速上手(705)](http://www.blogjava.net/libin2722/archive/2008/03/14/186260.html)
* [28. 深入浅出Liferay Portal (8) (686)](http://www.blogjava.net/libin2722/archive/2008/03/14/186279.html)
* [29. JSF最佳入门(673)](http://www.blogjava.net/libin2722/archive/2007/11/24/162833.html)
* [30. 深入浅出Liferay Portal (10) (672)](http://www.blogjava.net/libin2722/archive/2008/03/14/186281.html)
* [31. maven2完全使用手册(671)](http://www.blogjava.net/libin2722/archive/2008/01/10/174413.html)
* [32. 编写你自己的单点登录（SSO）服务 (668)](http://www.blogjava.net/libin2722/archive/2007/09/11/144175.html)
* [33. 深入浅出Liferay Portal (5) (637)](http://www.blogjava.net/libin2722/archive/2008/03/14/186275.html)
* [34. maven2.0学习笔记 (604)](http://www.blogjava.net/libin2722/archive/2008/01/10/174421.html)
* [35. 用axis发布webservices（五）(590)](http://www.blogjava.net/libin2722/archive/2007/11/24/162845.html)
* [36. 深入浅出Liferay Portal (6) (587)](http://www.blogjava.net/libin2722/archive/2008/03/14/186277.html)
* [37. 将JBoss启动做成Windows的系统服务(576)](http://www.blogjava.net/libin2722/archive/2008/05/29/203794.html)
* [38. mvn功能简介 (570)](http://www.blogjava.net/libin2722/archive/2008/01/10/174420.html)
* [39. Tomcat 5.5.2 下部署 Liferay 4.4.1(568)](http://www.blogjava.net/libin2722/archive/2008/02/24/181745.html)
* [40. 深入了解Java ClassLoader、Bytecode 、ASM、cglib (567)](http://www.blogjava.net/libin2722/archive/2007/09/23/147513.html)

### 评论排行榜

* [1. ClassNotFoundException: org.hibernate.hql.ast.HqlToken(8)](http://www.blogjava.net/libin2722/archive/2007/09/27/148815.html)
* [2. 测试-答对5道题的人是天才，答对4道的是帅才，答对3道的是将才，答对2道的是奇才，答对1道的是人才(4)](http://www.blogjava.net/libin2722/archive/2007/11/10/159603.html)
* [3. 用axis发布webservices（一） (3)](http://www.blogjava.net/libin2722/archive/2007/11/24/162829.html)
* [4. Liferay Portal学习笔记（一）：安装(3)](http://www.blogjava.net/libin2722/archive/2008/03/02/183317.html)
* [5. JAVA中SSL证书认证通讯-Client(3)](http://www.blogjava.net/libin2722/archive/2008/02/13/179833.html)
* [6. 打印出Ibatis最终的SQL语句(2)](http://www.blogjava.net/libin2722/archive/2007/12/04/165153.html)
* [7. 深入浅出Liferay Portal (12) (2)](http://www.blogjava.net/libin2722/archive/2008/03/14/186285.html)
* [8. Tomcat 5.5.2 下部署 Liferay 4.4.1(2)](http://www.blogjava.net/libin2722/archive/2008/02/24/181745.html)
* [9. Axis1.4 开发笔记(2)](http://www.blogjava.net/libin2722/archive/2007/11/22/162444.html)
* [10. HashMap 、HashTable、HashSet的区别 (2)](http://www.blogjava.net/libin2722/archive/2007/11/24/162853.html)
* [11. java线程综述 (2)](http://www.blogjava.net/libin2722/archive/2007/11/24/162843.html)
* [12. java对word、excel、pdf等操作综合文章(1)](http://www.blogjava.net/libin2722/archive/2007/11/24/162841.html)
* [13. 使用maven2 打ear包(1)](http://www.blogjava.net/libin2722/archive/2008/02/02/179037.html)
* [14. 深入了解Java ClassLoader、Bytecode 、ASM、cglib (1)](http://www.blogjava.net/libin2722/archive/2007/09/23/147513.html)
* [15. 代理模式(1)](http://www.blogjava.net/libin2722/archive/2007/09/22/147477.html)
* [16. 深入浅出Liferay Portal (11) (1)](http://www.blogjava.net/libin2722/archive/2008/03/14/186282.html)
* [17. 深入浅出Liferay Portal (10) (1)](http://www.blogjava.net/libin2722/archive/2008/03/14/186281.html)
* [18. Axis1.4 利用 deploy.wsdd 发布 server-config.wsdd文件(1)](http://www.blogjava.net/libin2722/archive/2007/11/25/163011.html)
* [19. 在Struts2.0中如何得到绝对路径(1)](http://www.blogjava.net/libin2722/archive/2007/12/30/171749.html)
* [20. Apache Maven 2 简介(最全的文档)(1)](http://www.blogjava.net/libin2722/archive/2008/01/10/174425.html)
* [21. maven2完全使用手册(1)](http://www.blogjava.net/libin2722/archive/2008/01/10/174413.html)
* [22. 论坛灌水机 -- HTTPClient(1)](http://www.blogjava.net/libin2722/archive/2008/02/13/179849.html)
* [23. httpclient中MultipartPostMethod类上传文件(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179848.html)
* [24. 应用HttpClient来对付各种顽固的WEB服务器(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179847.html)
* [25. Jakarta Commons HttpClient 学习笔记(2)(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179846.html)
* [26. Jakarta Commons HttpClient 学习笔记(1)(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179845.html)
* [27. JAVA对数字证书的常用操作(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179844.html)
* [28. 使用Java实现CA(四)(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179843.html)
* [29. 使用Java实现CA(三)(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179842.html)
* [30. 使用Java实现CA(二)(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179841.html)
* [31. 使用Java实现CA(一)(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179839.html)
* [32. 使用Java开发和信息安全相关的程序(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179838.html)
* [33. JAVA中SSL证书认证通讯-Server(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179836.html)
* [34. JAVA中SSL证书认证通讯-Certification(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179834.html)
* [35. Tomcat5SSL_ServerAndClient 的配置实例(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179832.html)
* [36. Tomcat5SSL_ServerAndClient(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179831.html)
* [37. 配置Tomcat 4使用SSL(0)](http://www.blogjava.net/libin2722/archive/2008/02/13/179828.html)
* [38. Liferay Portal二次开发指南(0)](http://www.blogjava.net/libin2722/archive/2008/03/02/183216.html)
* [39. smsql自动生成.(0)](http://www.blogjava.net/libin2722/archive/2008/02/28/182720.html)
* [40. Problem with Sybase, PostgreSQL and Timestamp columns(0)](http://www.blogjava.net/libin2722/archive/2008/02/24/181747.html)

*
[通过java获取mac地址]()

package  com.juziku.util;
import  java.io.BufferedReader;
import  java.io.InputStreamReader;
import  java.util.regex.Matcher;
import  java.util.regex.Pattern;
public   class  GetMacAddress {
     
      public   static  String callCmd(String[] cmd) {  
         String result  =   "" ;  
         String line  =   "" ;  
          try  {  
             Process proc  =  Runtime.getRuntime().exec(cmd);  
             InputStreamReader is  =   new  InputStreamReader(proc.getInputStream());  
             BufferedReader br  =   new  BufferedReader (is);  
              while  ((line  =  br.readLine ())  !=   null ) {  
             result  +=  line;  
             }  
         }  
          catch (Exception e) {  
             e.printStackTrace();  
         }  
          return  result;  
     }
     
     
     
     
      /**  
      * 
      *  @param  cmd  第一个命令 
      *  @param  another 第二个命令 
      *  @return    第二个命令的执行结果 
       */   
      public   static  String callCmd(String[] cmd,String[] another) {  
         String result  =   "" ;  
         String line  =   "" ;  
          try  {  
             Runtime rt  =  Runtime.getRuntime();  
             Process proc  =  rt.exec(cmd);  
             proc.waitFor();   // 已经执行完第一个命令，准备执行第二个命令  
             proc  =  rt.exec(another);  
             InputStreamReader is  =   new  InputStreamReader(proc.getInputStream());  
             BufferedReader br  =   new  BufferedReader (is);  
              while  ((line  =  br.readLine ())  !=   null ) {  
                 result  +=  line;  
             }  
         }  
          catch (Exception e) {  
             e.printStackTrace();  
         }  
          return  result;  
     }
     
     
     
      /**  
      * 
      *  @param  ip  目标ip,一般在局域网内 
      *  @param  sourceString 命令处理的结果字符串 
      *  @param  macSeparator mac分隔符号 
      *  @return   mac地址，用上面的分隔符号表示 
       */   
      public   static  String filterMacAddress( final  String ip,  final  String sourceString, final  String macSeparator) {  
         String result  =   "" ;  
         String regExp  =   " ((([0-9,A-F,a-f]{1,2} "   +  macSeparator  +   " ){1,5})[0-9,A-F,a-f]{1,2}) " ;  
         Pattern pattern  =  Pattern.compile(regExp);  
         Matcher matcher  =  pattern.matcher(sourceString);  
          while (matcher.find()){  
             result  =  matcher.group( 1 );  
              if (sourceString.indexOf(ip)  <=  sourceString.lastIndexOf(matcher.group( 1 ))) {  
                  break ;   // 如果有多个IP,只匹配本IP对应的Mac.  
             }  
         }
   
          return  result;  
     }
     
     
     
      /**  
      * 
      *  @param  ip 目标ip 
      *  @return    Mac Address 
      * 
       */   
      public   static  String getMacInWindows( final  String ip){  
         String result  =   "" ;  
         String[] cmd  =  {  
                  " cmd " ,  
                  " /c " ,  
                  " ping  "   +   ip  
                 };  
         String[] another  =  {  
                  " cmd " ,  
                  " /c " ,  
                  " arp -a "   
                 };  
   
         String cmdResult  =  callCmd(cmd,another);  
         result  =  filterMacAddress(ip,cmdResult, " - " );  
   
          return  result;  
     }  
   
   
      /**  
     * 
     *  @param  ip 目标ip 
     *  @return    Mac Address 
     * 
      */   
      public   static  String getMacInLinux( final  String ip){  
         String result  =   "" ;  
         String[] cmd  =  {  
                  " /bin/sh " ,  
                  " -c " ,  
                  " ping  "   +   ip  +   "  -c 2 && arp -a "   
                 };  
         String cmdResult  =  callCmd(cmd);  
         result  =  filterMacAddress(ip,cmdResult, " : " );  
   
          return  result;  
     }  
     
      /**
      * 获取MAC地址 
      *  @return  返回MAC地址
       */
      public   static  String getMacAddress(String ip){
         String macAddress  =   "" ;
         macAddress  =  getMacInWindows(ip).trim();
          if (macAddress == null || "" .equals(macAddress)){
             macAddress  =  getMacInLinux(ip).trim();
         }
          return  macAddress;
     }
   
      /**  
     * 测试 
      */   
      public   static   void  main(String[] args) {           
         System.out.println(getMacAddress( " 192.168.10.203 " ));
     }
     
}通过java获取mac地址，下面是完整的代码：
posted on 2011-06-02 23:52 [礼物](http://www.blogjava.net/libin2722/) 阅读(825) [评论(0)](http://www.blogjava.net/libin2722/articles/351643.html#Post)  [编辑](http://www.blogjava.net/libin2722/admin/EditArticles.aspx?postid=351643)  [收藏](http://www.blogjava.net/libin2722/AddToFavorite.aspx?id=351643) ![]()

[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [SpringOne 开发者大会（北京，12月7日-8日，免费）](http://springonechina.cloudfoundry.com/register?code=Xcq3Z1)   [博客园](http://www.cnblogs.com/)  [博问](http://q.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/libin2722/articles/351643.html&SourceURL=/libin2722/articles/351643.html)       [使用Ctrl+Enter键可以直接提交]    IT新闻：
· [百度收购今晚看啥团队 加强视频搜索个性化](http://news.cnblogs.com/n/165285/)
· [土卫三红外图像似有趣“吃豆人”](http://news.cnblogs.com/n/165284/)
· [夏普首款5寸1080p手机港行今日发布](http://news.cnblogs.com/n/165283/)
· [手机巨头大会战：Lumia 920搏杀iPhone及Galaxy](http://news.cnblogs.com/n/165282/)
· [永远的颠覆者——奇虎360董事长周鸿祎专访](http://news.cnblogs.com/n/165279/)

博客园首页随笔：
· [win8应用商店安装路径](http://www.cnblogs.com/qiqiBoKe/archive/2012/11/29/2794179.html)
· [分享ISTQB培训体验](http://www.cnblogs.com/powertoolsteam/archive/2012/11/29/2794168.html)
· [Javascript知识分享，不是闭包这回事。](http://www.cnblogs.com/lindaohui/archive/2012/11/29/2794136.html)
· [[Silverlight]MVVM+MEF框架Jounce学习(2):标记和绑定](http://www.cnblogs.com/slmk/archive/2012/11/29/2792880.html)
· [“图灵原创”教你如何用C语言给情书加密——关于《C程序设计伴侣》10.3.1、10.3.2](http://www.cnblogs.com/pmer/archive/2012/11/29/2794025.html)
知识库：
· [走出浮躁的泥沼](http://kb.cnblogs.com/page/163463/)
· [HTTP服务七层架构技术探讨](http://kb.cnblogs.com/page/158568/)
· [HTTP协议之基本认证](http://kb.cnblogs.com/page/158829/)
· [17家中国初创公司的失败史](http://kb.cnblogs.com/page/164879/)
· [高斯模糊的算法](http://kb.cnblogs.com/page/164877/)  网站导航:

[博客园](http://www.cnblogs.com/ "程序员的网上家园")   [IT新闻](http://news.cnblogs.com/)   [知识库](http://kb.cnblogs.com/)   [C++博客](http://www.cppblog.com/)   [程序员招聘](http://job.cnblogs.com/)   [管理](http://www.blogjava.net/libin2722/articles/351643.html?opt=admin)   

Powered by:
[BlogJava](http://www.blogjava.net/)
Copyright © 礼物
