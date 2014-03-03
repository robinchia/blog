---
layout: post
title: "tomcat中的context.xml元素详解 - pasha - BlogJava"
categories: 服务器
tags: 
 - 服务器
--- 

# tomcat中的context.xml元素详解 - pasha - BlogJava

# [pasha](http://www.blogjava.net/pasha/)

 

## [tomcat中的context.xml元素详解]()

**tomcat中的context.xml元素详解小人物，大博客 Q'e ^ L0J*J4h
** **元素名** **属性** **解释** server port 指定一个端口，这个端口负责监听关闭tomcat的请求 shutdown 指定向端口发送的命令字符串 service name 指定service的名字 Connector(表示客户端和service之间的连接) port 指定服务器端要创建的端口号，并在这个端口监听来自客户端的请求 minProcessors 服务器启动时创建的处理请求的线程数 maxProcessors 最大可以创建的处理请求的线程数 enableLookups 如果为true，则可以通过调用request.getRemoteHost()进行DNS查询来得到远程客户端的实际主机名，若为false则不进行DNS查询，而是返回其ip地址 redirectPort 指定服务器正在处理http请求时收到了一个SSL传输请求后重定向的端口号 acceptCount 指定当所有可以使用的处理请求的线程数都被使用时，可以放到处理队列中的请求数，超过这个数的请求将不予处理 connectionTimeout 指定超时的时间数(以毫秒为单位) Engine(表示指定service中的请求处理机，接收和处理来自Connector的请求) defaultHost 指定缺省的处理请求的主机名，它至少与其中的一个host元素的name属性值是一样的 Context(表示一个web应用程序，通常为WAR文件，关于WAR的具体信息见servlet规范) docBase 应用程序的路径或者是WAR文件存放的路径 path 表示此web应用程序的url的前缀，这样请求的url为http://localhost:8080/path/**** reloadable 这个属性非常重要，如果为true，则tomcat会自动检测应用程序的/WEB-INF/lib 和/WEB-INF/classes目录的变化，自动装载新的应用程序，我们可以在不重起tomcat的情况下改变应用程序 host(表示一个虚拟主机) name 指定主机名 appBase 应用程序基本目录，即存放应用程序的目录 unpackWARs 如果为true，则tomcat会自动将WAR文件解压，否则不解压，直接从WAR文件中运行应用程序 Logger(表示日志，调试和错误信息) className 指定logger使用的类名，此类必须实现org.apache.catalina.Logger 接口 prefix 指定log文件的前缀 suffix 指定log文件的后缀 timestamp 如果为true，则log文件名中要加入时间，如下例:localhost_log.2001-10-04.txt Realm(表示存放用户名，密码及role的数据库) className 指定Realm使用的类名，此类必须实现org.apache.catalina.Realm接口 Valve(功能与Logger差不多，其prefix和suffix属性解释和Logger 中的一样) className 指定Valve使用的类名，如用org.apache.catalina.valves.AccessLogValve类可以记录应用程序的访问信息 directory 指定log文件存放的位置 pattern 有两个值，common方式记录远程主机名或ip地址，用户名，日期，第一行请求的字符串，HTTP响应代码，发送的字节数。combined方式比common方式记录的值更多

posted on 2008-09-21 11:14 [pasha](http://www.blogjava.net/pasha/) 阅读(2524) [评论(0)](http://www.blogjava.net/pasha/articles/230241.html#Post)  [编辑](http://www.blogjava.net/pasha/admin/EditArticles.aspx?postid=230241)  [收藏](http://www.blogjava.net/pasha/AddToFavorite.aspx?id=230241)

 ![]()

[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [开发者的云计算盛会，期待您的参与（11月20日~21日，北京，免费）](http://www.cnblogs.com/cmt/archive/2012/10/31/vmware_vforum_2012.html)   [博客园](http://www.cnblogs.com/)  [博问](http://q.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/pasha/articles/230241.html&SourceURL=/pasha/articles/230241.html)       [使用Ctrl+Enter键可以直接提交]    IT新闻：
· [让你的iPhone 5变身小电视](http://news.cnblogs.com/n/163329/)
· [流言终结者 #19 –「我只要专心把产品做好…」](http://news.cnblogs.com/n/163348/)
· [来看看1399元的Lumia究竟如何](http://news.cnblogs.com/n/163347/)
· [周鸿祎给李彦宏一个支点](http://news.cnblogs.com/n/163346/)
· [Twitter HOLD 住大选：少点 Ruby，多些 Java](http://news.cnblogs.com/n/163345/)

博客园首页随笔：
· [[Android问答] 如何实现“退出应用”功能？](http://www.cnblogs.com/bjzhanghao/archive/2012/11/09/2762550.html)
· [Python内置函数清单](http://www.cnblogs.com/vamei/archive/2012/11/09/2762224.html)
· [C#数据结构与算法揭秘13](http://www.cnblogs.com/manuosex/archive/2012/11/09/2762405.html)
· [Portal-Basic Java Web 应用开发框架：应用篇（八） —— Freemarker 整合](http://www.cnblogs.com/ldcsaa/archive/2012/11/09/2762373.html)
· [如何找到并完成兼职项目](http://www.cnblogs.com/nnhy/archive/2012/11/09/Money.html)
知识库：
· [项目管理心得：一个项目经理的个人体会、经验总结](http://kb.cnblogs.com/page/157087/)
· [十天内掌握线性代数：惊人的超速学习实验](http://kb.cnblogs.com/page/162480/)
· [可伸缩性最佳实践：来自eBay的经验](http://kb.cnblogs.com/page/157745/)
· [一堂如何提高代码质量的培训课](http://kb.cnblogs.com/page/160526/)
· [程序江湖](http://kb.cnblogs.com/page/161182/)  网站导航:

[博客园](http://www.cnblogs.com/ "程序员的网上家园")   [IT新闻](http://news.cnblogs.com/)   [知识库](http://kb.cnblogs.com/)   [C++博客](http://www.cppblog.com/)   [程序员招聘](http://job.cnblogs.com/)   [管理](http://www.blogjava.net/pasha/articles/230241.html?opt=admin)   
### 导航

* [BlogJava](http://www.blogjava.net/)
* [首页](http://www.blogjava.net/pasha/)
* [新随笔](http://www.blogjava.net/pasha/admin/EditPosts.aspx?opt=1)
* [联系](http://www.blogjava.net/pasha/contact.aspx?id=1)
* [聚合](http://www.blogjava.net/pasha/rss)[![]()](http://www.blogjava.net/pasha/rss)
* [管理](http://www.blogjava.net/pasha/admin/EditPosts.aspx)

### 统计

* 随笔 - 7
* 文章 - 10
* 评论 - 1
* 引用 - 0

### 常用链接

* [我的随笔](http://www.blogjava.net/pasha/MyPosts.html)
* [我的文章](http://www.blogjava.net/pasha/MyArticles.html)
* [我的评论](http://www.blogjava.net/pasha/MyComments.html)
* [我的参与](http://www.blogjava.net/pasha/OtherPosts.html)
* [最新评论](http://www.blogjava.net/pasha/RecentComments.html)

### 留言簿(2)

* [给我留言](http://www.blogjava.net/pasha/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/pasha/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/pasha/admin/MyMessages.aspx)

### 随笔分类

* [ajax](http://www.blogjava.net/pasha/category/33818.html) [(rss)](http://www.blogjava.net/pasha/category/33818.html/rss "Subscribe to ajax")
* [database](http://www.blogjava.net/pasha/category/34754.html) [(rss)](http://www.blogjava.net/pasha/category/34754.html/rss "Subscribe to database")
* [struts2(1)](http://www.blogjava.net/pasha/category/33817.html) [(rss)](http://www.blogjava.net/pasha/category/33817.html/rss "Subscribe to struts2(1)")
* [tomcat(2)](http://www.blogjava.net/pasha/category/34753.html) [(rss)](http://www.blogjava.net/pasha/category/34753.html/rss "Subscribe to tomcat(2)")
* [其他(2)](http://www.blogjava.net/pasha/category/35370.html) [(rss)](http://www.blogjava.net/pasha/category/35370.html/rss "Subscribe to 其他(2)")

### 随笔档案

* [2008年10月 (2)](http://www.blogjava.net/pasha/archive/2008/10.html)
* [2008年9月 (3)](http://www.blogjava.net/pasha/archive/2008/09.html)
* [2008年6月 (1)](http://www.blogjava.net/pasha/archive/2008/06.html)
* [2008年4月 (1)](http://www.blogjava.net/pasha/archive/2008/04.html)

### 文章分类

* [其它](http://www.blogjava.net/pasha/category/35369.html) [(rss)](http://www.blogjava.net/pasha/category/35369.html/rss "Subscribe to 其它")

### 文章档案

* [2008年9月 (4)](http://www.blogjava.net/pasha/archives/2008/09.html)
* [2008年8月 (2)](http://www.blogjava.net/pasha/archives/2008/08.html)
* [2008年7月 (1)](http://www.blogjava.net/pasha/archives/2008/07.html)
* [2008年6月 (2)](http://www.blogjava.net/pasha/archives/2008/06.html)

### 最常用和实用的CSS技巧

### 最新随笔

* [1. Ubuntu Linux与Windows系统多启动的配置(Webmaster )](http://www.blogjava.net/pasha/archive/2008/10/21/235638.html)
* [2. Bcdedit命令详解](http://www.blogjava.net/pasha/archive/2008/10/21/235617.html)
* [3. JQuery Ajax 方法说明](http://www.blogjava.net/pasha/articles/230555.html)
* [4. EditPlus技巧(Liangjhrn)](http://www.blogjava.net/pasha/articles/230458.html)
* [5. 转载：《关于JSP页面中的pageEncoding和contentType两种属性的区别》](http://www.blogjava.net/pasha/articles/230372.html)
* [6. tomcat中的context.xml元素详解]()
* [7. Tomcat6数据源配置（转）](http://www.blogjava.net/pasha/archive/2008/09/21/230237.html)
* [8. tomcat 6.18 设置](http://www.blogjava.net/pasha/archive/2008/09/21/230227.html)
* [9. struts2UI td 的问题](http://www.blogjava.net/pasha/archive/2008/09/09/227881.html)
* [10. 几款MYSQL管理工具](http://www.blogjava.net/pasha/articles/222926.html)

### 搜索

*
*  

### 最新评论 [![]()](http://www.blogjava.net/pasha/CommentsRSS.aspx)

* [1. re: SESSION](http://www.blogjava.net/pasha/archive/2008/09/24/208536.html#230938)
* 啊！！原来是这样啊 终于明白了 谢了！！
* --ndj

### 阅读排行榜

* [1. tomcat 6.18 设置(711)](http://www.blogjava.net/pasha/archive/2008/09/21/230227.html)
* [2. SSO (转)(613)](http://www.blogjava.net/pasha/archive/2008/04/08/191507.html)
* [3. Bcdedit命令详解(407)](http://www.blogjava.net/pasha/archive/2008/10/21/235617.html)
* [4. Tomcat6数据源配置（转）(338)](http://www.blogjava.net/pasha/archive/2008/09/21/230237.html)
* [5. SESSION(181)](http://www.blogjava.net/pasha/archive/2008/06/17/208536.html)

### 评论排行榜

* [1. SESSION(1)](http://www.blogjava.net/pasha/archive/2008/06/17/208536.html)
* [2. SSO (转)(0)](http://www.blogjava.net/pasha/archive/2008/04/08/191507.html)
* [3. Ubuntu Linux与Windows系统多启动的配置(Webmaster )(0)](http://www.blogjava.net/pasha/archive/2008/10/21/235638.html)
* [4. Bcdedit命令详解(0)](http://www.blogjava.net/pasha/archive/2008/10/21/235617.html)
* [5. Tomcat6数据源配置（转）(0)](http://www.blogjava.net/pasha/archive/2008/09/21/230237.html)

Powered by:
[BlogJava](http://www.blogjava.net/)
Copyright © pasha
