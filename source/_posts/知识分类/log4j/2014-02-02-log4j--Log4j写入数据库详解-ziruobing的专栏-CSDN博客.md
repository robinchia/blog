---
layout: post
title: "Log4j写入数据库详解 - ziruobing的专栏 - CSDN博客"
categories: log4j
tags: 
 - log4j
--- 

# Log4j写入数据库详解 - ziruobing的专栏 - CSDN博客

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

# [ziruobing的专栏](http://blog.csdn.net/ziruobing)

## 黑暗中漫舞

* [条新通知](http://hi.csdn.net/space-notice.html)
* [登录](http://passport.csdn.net/UserLogin.aspx?from=http%3A%2F%2Fblog.csdn.net%2Fziruobing%2Farchive%2F2009%2F02%2F22%2F3919501.aspx)
* [注册](http://passport.csdn.net/CSDNUserRegister.aspx)
* [欢迎](http://hi.csdn.net/)
* [退出](http://writeblog.csdn.net/Signout.aspx)
* [我的博客](http://blog.csdn.net/)
* [配置](http://writeblog.csdn.net/configure.aspx)
* [写文章](http://writeblog.csdn.net/PostEdit.aspx)
* [文章管理](http://writeblog.csdn.net/PostList.aspx)
* [博客首页](http://blog.csdn.net/)

*
* 全站 当前博客
*

* [空间](http://hi.csdn.net/ziruobing)
* [博客](http://blog.csdn.net/ziruobing)
* [好友](http://hi.csdn.net/!s/friend/list/ziruobing)
* [相册](http://hi.csdn.net/!s/album/list/ziruobing)
* [留言](http://hi.csdn.net/!s/wall/to/ziruobing)
用户操作 [[留言]](http://hi.csdn.net/!s/wall/to/ziruobing)  [[发消息]](http://hi.csdn.net/!s/msg/to/ziruobing)  [[加为好友]](http://hi.csdn.net/!s/friend/add/ziruobing)  ziruobingID：[ziruobing](http://hi.csdn.net/ziruobing)[![ziruobing]()](http://hi.csdn.net/ziruobing)共*5086*次访问，排名*2万外*ziruobing的文章原创 7 篇翻译 0 篇转载 4 篇评论 8 篇  订阅我的博客 [![XML聚合]()](http://feeds.feedsky.com/csdn.net/ziruobing)   [![FeedSky]()](http://feeds.feedsky.com/csdn.net/ziruobing) [![订阅到鲜果]()](http://www.xianguo.com/subscribe.php?url=http://feeds.feedsky.com/csdn.net/ziruobing) [![订阅到Google]()](http://fusion.google.com/add?feedurl=http://feeds.feedsky.com/csdn.net/ziruobing) [![订阅到抓虾]()](http://www.zhuaxia.com/add_channel.php?url=http://feeds.feedsky.com/csdn.net/ziruobing)  [[编辑]](http://writeblog.csdn.net/configure.aspx)ziruobing的公告 [[编辑]](http://writeblog.csdn.net/EditCategories.aspx?catID=1)文章分类 * [![(RSS)]()](http://blog.csdn.net/ziruobing/category/517642.aspx/rss)[database](http://blog.csdn.net/ziruobing/category/517642.aspx "数据库")
* [![(RSS)]()](http://blog.csdn.net/ziruobing/category/515395.aspx/rss)[java](http://blog.csdn.net/ziruobing/category/515395.aspx "java")
* [![(RSS)]()](http://blog.csdn.net/ziruobing/category/515969.aspx/rss)[其它](http://blog.csdn.net/ziruobing/category/515969.aspx) 存档 * [2009年09月(1)](http://blog.csdn.net/ziruobing/archive/2009/09.aspx)
* [2009年06月(1)](http://blog.csdn.net/ziruobing/archive/2009/06.aspx)
* [2009年03月(2)](http://blog.csdn.net/ziruobing/archive/2009/03.aspx)
* [2009年02月(7)](http://blog.csdn.net/ziruobing/archive/2009/02.aspx)

### 公告：

* [CSDN社区招聘.Net开发工程师、运营专员，UI设计师](http://topic.csdn.net/u/20110519/09/63bafb67-8580-4f4f-a8a6-0e8469459875.html)
[[意见反馈]](http://forum.csdn.net/SList/blogSupport)[[官方博客]](http://blog.csdn.net/blogdevteam)

# ![原创]()  Log4j写入数据库详解 [收藏]( "收藏到我的网摘中，并分享给我的朋友")

 log4j是一个优秀的开源日志记录项目，我们不仅可以对输出的日志的格式自定义，还可以自己定义日志输出的目的地，比如：屏幕，文本文件，数据库，甚至能通过socket输出。本节主要讲述如何将日志信息输入到数据库（可以插入任何数据库，在此主要以MSSQL为例进行详解）。
用log4j将日志写入数据库主要用到是log4j包下的JDBCAppender类，它提供了将日志信息异步写入数据的功能，我们可以直接使用这个类将我们的日志信息写入数据库；也可以扩展JDBCAppender类，就是将JDBCAppender类作为基类。下面将通过一个实例来讲解log4j是如何将日志信息写入数据库的。
我们的需求：我们在软件开发的过程中需要将调试信息、操作信息等记录下来，以便后面的审计，这些日志信息包括用户ID、用户姓名、操作类、路径、方法、操作时间、日志信息。
设计思想：我们采用JDBCAppender类直接将日志信息插入数据库，所有只需要在配置文件配置此类就可以;要获得用户信息需要用过滤器来实现；（假如不需要用户的信息，就不需要设计过滤器，其实大部分情况下都是需要这些用户信息，尤其是在web应用开发中）在日志信息中获得用户信息，就的通过过滤器的request或session对象，从session中拿到用户信息怎样传到log4j呢，log4j为我们提供了MDC（MDC是log4j种非常有用类，它们用于存储应用程序的上下文信息（context infomation），从而便于在log中使用这些上下文信息。MDC内部使用了类似map的机制来存储信息，上下文信息也是每个线程独立地储存，所不同的是信息都是以它们的key值存储在”map”中。相对应的方法，

MDC.put(key, value); MDC.remove(key); MDC.get(key);

在配置PatternLayout的时候使用：%x{key}来输出对应的value）。有了MDC，我们可以在过滤器中先获得用户信息，再用MDC.Put（“key”）方法，log在执行sql语句时通过%x{key}来输出对应的value。

实现步骤：
1、在你的项目中要确保有log4j和commons-logging这两个jar文件；
2、设置要你要插入日志信息的表结构

1. if exists (select * from dbo.sysobjects where id = object_id(N'[dbo].[WDZLOG]') and OBJECTPROPERTY(id, N'IsUserTable') = 1)  
1. drop table [dbo].[WDZLOG]  
1. GO  
1.   
1. CREATE TABLE [dbo].[WDZLOG] (  
1.     [WDZLOGID] [int] IDENTITY (1, 1) NOT NULL ,  
1.     [LogName] [varchar] (255) COLLATE Chinese_PRC_CI_AS NULL ,//用户ID  
1.     [UserName] [varchar] (255) COLLATE Chinese_PRC_CI_AS NULL ,//用户姓名  
1.     [Class] [varchar] (255) COLLATE Chinese_PRC_CI_AS NULL ,//类名  
1.     [Mothod] [varchar] (255) COLLATE Chinese_PRC_CI_AS NULL //,方法名  
1.     [CreateTime] [varchar] (255) COLLATE Chinese_PRC_CI_AS NULL ,//产生时间  
1.     [LogLevel] [varchar] (20) COLLATE Chinese_PRC_CI_AS NULL ,//日志级别  
1.     [MSG] [varchar] (555) COLLATE Chinese_PRC_CI_AS NULL //日志信息  
1. ) ON [PRIMARY]  
1. GO  
if exists (select * from dbo.sysobjects where id = object_id(N'[dbo].[WDZLOG]') and OBJECTPROPERTY(id, N'IsUserTable') = 1) drop table [dbo].[WDZLOG] GO CREATE TABLE [dbo].[WDZLOG] ( [WDZLOGID] [int] IDENTITY (1, 1) NOT NULL , [LogName] [varchar] (255) COLLATE Chinese_PRC_CI_AS NULL ,//用户ID [UserName] [varchar] (255) COLLATE Chinese_PRC_CI_AS NULL ,//用户姓名 [Class] [varchar] (255) COLLATE Chinese_PRC_CI_AS NULL ,//类名 [Mothod] [varchar] (255) COLLATE Chinese_PRC_CI_AS NULL //,方法名 [CreateTime] [varchar] (255) COLLATE Chinese_PRC_CI_AS NULL ,//产生时间 [LogLevel] [varchar] (20) COLLATE Chinese_PRC_CI_AS NULL ,//日志级别 [MSG] [varchar] (555) COLLATE Chinese_PRC_CI_AS NULL //日志信息 ) ON [PRIMARY] GO

3、配置文件（摘自我们的项目）后面将对此配置文件进行详细讲解，它也log4j的核心部分。
1. log4j.properties  
1. log4j.rootLogger=INFO,stdout  
1.               
1. log4j.logger.org.springframework.web.servlet=INFO,db  
1.   
1. log4j.logger.org.springframework.beans.factory.xml=INFO  
1. log4j.logger.com.neam.stum.user=INFO,db  
1.   
1. log4j.appender.stdout=org.apache.log4j.ConsoleAppender  
1. log4j.appender.stdout.layout=org.apache.log4j.PatternLayout  
1. log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %p [%c] - - <%m>%n  
1.   
1. log4j.appender.logfile=org.apache.log4j.DailyRollingFileAppender  
1. log4j.appender.logfile.File=${webapp.root}/WEB-INF/logs/exppower.log  
1. log4j.appender.logfile.DatePattern=.yyyy-MM-dd  
1. log4j.appender.logfile.layout=org.apache.log4j.PatternLayout  
1. log4j.appender.logfile.layout.ConversionPattern=%d %p [%c] wang- <%m>%n  
1.  
1. ########################  
1.  
1. # JDBC Appender  
1.  
1. #######################  
1.  
1.  
1. #log4j.logger.business=INFO,db  
1. #log4j.appender.db=com.neam.commons.MyJDBCAppender  
1. log4j.appender.db=JDBCExtAppender  
1.   
1. log4j.appender.db.BufferSize=10  
1.   
1. log4j.appender.db.sqlname=log  
1.   
1. log4j.appender.db.driver=net.sourceforge.jtds.jdbc.Driver  
1.                         
1. log4j.appender.db.URL=jdbc:jtds:SqlServer://localhost:1433;DatabaseName=pubs  
1.   
1. log4j.appender.db.user=sa  
1.   
1. log4j.appender.db.password=sa  
1.   
1. log4j.appender.db.sql=insert into WDZLOG (LogName,UserName,Class,Mothod,createTime,LogLevel,MSG) values ('%X{userId}','%X{userName}','%C','%M','%d{yyyy-MM-dd HH:mm:ss}','%p','%m')  
1.   
1. log4j.appender.db.layout=org.apache.log4j.PatternLayout  
log4j.properties log4j.rootLogger=INFO,stdout log4j.logger.org.springframework.web.servlet=INFO,db log4j.logger.org.springframework.beans.factory.xml=INFO log4j.logger.com.neam.stum.user=INFO,db log4j.appender.stdout=org.apache.log4j.ConsoleAppender log4j.appender.stdout.layout=org.apache.log4j.PatternLayout log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %p [%c] - - <%m>%n log4j.appender.logfile=org.apache.log4j.DailyRollingFileAppender log4j.appender.logfile.File=${webapp.root}/WEB-INF/logs/exppower.log log4j.appender.logfile.DatePattern=.yyyy-MM-dd log4j.appender.logfile.layout=org.apache.log4j.PatternLayout log4j.appender.logfile.layout.ConversionPattern=%d %p [%c] wang- <%m>%n ######################## # JDBC Appender ####################### #log4j.logger.business=INFO,db #log4j.appender.db=com.neam.commons.MyJDBCAppender log4j.appender.db=JDBCExtAppender log4j.appender.db.BufferSize=10 log4j.appender.db.sqlname=log log4j.appender.db.driver=net.sourceforge.jtds.jdbc.Driver log4j.appender.db.URL=jdbc:jtds:SqlServer://localhost:1433;DatabaseName=pubs log4j.appender.db.user=sa log4j.appender.db.password=sa log4j.appender.db.sql=insert into WDZLOG (LogName,UserName,Class,Mothod,createTime,LogLevel,MSG) values ('%X{userId}','%X{userName}','%C','%M','%d{yyyy-MM-dd HH:mm:ss}','%p','%m') log4j.appender.db.layout=org.apache.log4j.PatternLayout

4、编写过滤器（ResFilter.java）
1. import java.io.IOException;  
1. import javax.servlet.Filter;  
1. import javax.servlet.FilterChain;  
1. import javax.servlet.FilterConfig;  
1. import javax.servlet.ServletException;  
1. import javax.servlet.ServletRequest;  
1. import javax.servlet.ServletResponse;  
1. import javax.servlet.http.HttpServletRequest;  
1. import javax.servlet.http.HttpSession;  
1.   
1. import org.apache.log4j.Logger;  
1. import org.apache.log4j.MDC;  
1.   
1. import com.neam.domain.User;  
1.   
1. public class ResFilter implements Filter{  
1.   
1.        
1.     private final static double DEFAULT_USERID= Math.random()*100000.0;    
1.   
1.     public void destroy() {  
1.     }  
1.   
1.     public void doFilter(ServletRequest request, ServletResponse response,  
1.            FilterChain chain) throws IOException, ServletException {  
1.        HttpServletRequest req=(HttpServletRequest)request;  
1.         HttpSession session= req.getSession();  
1.         if (session==null){  
1.             MDC.put("userId",DEFAULT_USERID);    
1.         }  
1.         else{  
1.             User customer=(User)session.getAttribute("user");  
1.             if (customer==null){  
1.                 MDC.put("userId",DEFAULT_USERID);  
1.                 MDC.put("userName",DEFAULT_USERID);  
1.             }  
1.             else  
1.             {  
1.                 MDC.put("userId",customer.getName());  
1.                 MDC.put("userName",customer.getName());  
1.             }  
1.         }  
1.         //logger.info("test for MDC.");  
1.   
1.        chain.doFilter(request, response);  
1.     }  
1.     public void init(FilterConfig Config) throws ServletException {  
1. //     this.filterConfig = Config;  
1. //     String ccc = Config.getServletContext().getInitParameter("cherset");  
1. //     this.targetEncoding = Config.getInitParameter("cherset");  
1.   
1.     }  
1. }  
import java.io.IOException; import javax.servlet.Filter; import javax.servlet.FilterChain; import javax.servlet.FilterConfig; import javax.servlet.ServletException; import javax.servlet.ServletRequest; import javax.servlet.ServletResponse; import javax.servlet.http.HttpServletRequest; import javax.servlet.http.HttpSession; import org.apache.log4j.Logger; import org.apache.log4j.MDC; import com.neam.domain.User; public class ResFilter implements Filter{ private final static double DEFAULT_USERID= Math.random()*100000.0; public void destroy() { } public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException { HttpServletRequest req=(HttpServletRequest)request; HttpSession session= req.getSession(); if (session==null){ MDC.put("userId",DEFAULT_USERID); } else{ User customer=(User)session.getAttribute("user"); if (customer==null){ MDC.put("userId",DEFAULT_USERID); MDC.put("userName",DEFAULT_USERID); } else { MDC.put("userId",customer.getName()); MDC.put("userName",customer.getName()); } } //logger.info("test for MDC."); chain.doFilter(request, response); } public void init(FilterConfig Config) throws ServletException { // this.filterConfig = Config; // String ccc = Config.getServletContext().getInitParameter("cherset"); // this.targetEncoding = Config.getInitParameter("cherset"); } }

5、在需要写入日志的地方引入

 
1. private Log logger = LogFactory.getLog(this.getClass());  
1.   
1. 在具体方法中就可以写入日志  
1. logger.info("");  
1. logger.debug("");  
1. logger.warn("");  
1. logger.error("");  
private Log logger = LogFactory.getLog(this.getClass()); 在具体方法中就可以写入日志 logger.info(""); logger.debug(""); logger.warn(""); logger.error("");

**配置文件详解：
**log4j.properties
log4j.properties
log4j.rootLogger=INFO,stdout

//**配置根Logger，**其语法为：
log4j.rootLogger = [ level ] , appenderName1, appenderName2, …
level : 是日志记录的优先级，分为OFF、FATAL、ERROR、WARN、INFO、DEBUG、ALL或者您定义的级别。Log4j建议只使用四个级别，优先级从高到低分别是ERROR、WARN、INFO、DEBUG。通过在这里定义的级别，您可以控制到应用程序中相应级别的日志信息的开关。比如在这里定义了INFO级别，则应用程序中所有DEBUG级别的日志信息将不被打印出来。
appenderName:就是指定日志信息输出到哪个地方。您可以同时指定多个输出目的地。
例如：log4j.rootLogger＝info,A1,B2,C3 配置了3个输出地方我们可以设置让A1在控制台输出；B2生产日志文件；C3让日志信息插入数据库中。
本例中是将所有的日志信息在控制台打印出来。
log4j.logger.org.springframework.web.servlet=INFO,db
//设置将spring下包的某些类的日志信息写入数据库中，并且在控制台上打印出来。（是通过log4j.rootLogger=INFO,stdout来体现的）db是将日志信息写入数据库中
log4j.logger.org.springframework.beans.factory.xml=INFO
//本实例中为了让某些包下的日志信息能写入数据库
log4j.logger.com.neam.stum.user=INFO,db
//设置自己某个模块下的日志信息既在控制台上打印而且往数据库中保存
//下面是配置在控制台上打印日志信息，在这里就不再仔细描述了
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %p [%c] - - <%m>%n
//下面是配置将日志信息写入文件中，在这里也就不再仔细描述了
log4j.appender.logfile=org.apache.log4j.DailyRollingFileAppender
log4j.appender.logfile.File=${webapp.root}/WEB-INF/logs/exppower.log
log4j.appender.logfile.DatePattern=.yyyy-MM-dd
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d %p [%c] wang- <%m>%n
########################
# JDBC Appender
#######################
#log4j.appender.db=com.neam.commons.MyJDBCAppender
//下面是配置将日志信息插入数据库，
log4j.appender.db=org.apache.log4j.jdbc.JDBCAppender
//配置输出目标为数据库（假如要将日志在控制台输出，配置为log4j.appender. stdout =org.apache.log4j.ConsoleAppender；将日志写入文件，配置为log4j.appender.logfile=org.apache.log4j.DailyRollingFileAppender
这样的配置在许多地方都要有，需要可查有关资料）,当然你也可以自己扩展org.apache.log4j.jdbc.JDBCAppender这个类，只需要在这里配置就可以了例如我们配置我自己扩展的MyJDBCAppender，配置为#log4j.appender.db=com.neam.commons.MyJDBCAppender
log4j.appender.db.BufferSize=10
//设置缓存大小，就是当有10条日志信息是才忘数据库插一次
log4j.appender.db.driver=net.sourceforge.jtds.jdbc.Driver
//设置要将日志插入到数据库的驱动                    
log4j.appender.db.URL=jdbc:jtds:SqlServer://localhost:1433;DatabaseName=pubs
log4j.appender.db.user=sa
log4j.appender.db.password=sa
log4j.appender.db.sql=insert into WDZLOG (LogName,UserName,Class,Mothod,createTime,LogLevel,MSG) values ('%X{userId}','%X{userName}','%C','%M','%d{yyyy-MM-dd HH:mm:ss}','%p','%m')
//设置要插入日志信息的格式和内容，%X{userId}是置取MDC中的key值，因为我们在过滤器中是将用户id和用户姓名放入MDC中，所有在这里可以用%X{userId}和%X{userName}取出用户的ID和用户姓名；'%C'表示日志信息是来自于那个类；%M表示日志信息来自于那个方法中；%d{yyyy-MM-dd HH:mm:ss}表示日志信息产生的时间，{yyyy-MM-dd HH:mm:ss}表示一种时间格式，你也可以直接写成%d；%p表示日志信息的级别（debug info warn error）；
%m表示你写入的日志信息
log4j.appender.db.layout=org.apache.log4j.PatternLayout

发表于 @ 2009年02月22日　01:05:00 | [评论( 5  )](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#FeedBack "评论")| [编辑](http://writeblog.csdn.net/PostEdit.aspx?entryId=3919501 "编辑")| [举报](mailto:webmaster@csdn.net?subject=Article%20Report!!!&body=Author:ziruobing%0D%0AURL:http://blog.csdn.net/ArticleContent.aspx?UserName=ziruobing&Entryid=3919501)| [收藏]( "收藏到我的网摘中，并分享给我的朋友")

### [旧一篇:正则表达式入门](http://blog.csdn.net/ziruobing/archive/2009/02/20/3914902.aspx) | [新一篇:Hibernate的两种事务管理jdbc 和jta方式](http://blog.csdn.net/ziruobing/archive/2009/02/26/3938601.aspx)

-

[查看最新精华文章 请访问博客首页](http://blog.csdn.net/)相关文章 [SQL计算表达式](http://blog.csdn.net/cosio/archive/2005/11/05/523334.aspx)[用SQL向*.txt中追加文本](http://blog.csdn.net/bugchen888/archive/2005/11/24/536069.aspx)[儲存過程萬能分頁](http://blog.csdn.net/frankchenyj/archive/2007/11/19/1892075.aspx)[利用SQL移动硬盘文件](http://blog.csdn.net/zhaowei001/archive/2008/01/03/2021036.aspx)[改变SQLServer 数据库所有对象的所有者成dbo](http://blog.csdn.net/taikeqi/archive/2008/11/05/3220018.aspx)[log4j把日志写入数据库详解](http://blog.csdn.net/dahaizisheng/archive/2009/09/23/4579491.aspx) []()

[](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#1154556 "permalink: Re:� Log4j写入数据库详解")[wofile](http://hi.csdn.net/wofile) 发表于Wed Sep 30 2009 12:13:26 GMT+0800 (China Standard Time)  IP:[举报](mailto:webmaster@csdn.net?subject=Comment%20Report!!!&body=Author:wofile%20URL:http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx)[回复]()[删除]()![]()tai hao le .... xie xie a ~~~[](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#1179457 "permalink: Re:Log4j写入数据库详解")[luchangbin_66](http://hi.csdn.net/luchangbin_66) 发表于Thu Nov 19 2009 10:53:29 GMT+0800 (China Standard Time)  IP:[举报](mailto:webmaster@csdn.net?subject=Comment%20Report!!!&body=Author:luchangbin_66%20URL:http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx)[回复]()[删除]()![]()Filter在web.xml中过滤那些。。。。。[](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#1185590 "permalink: Re:� Log4j写入数据库详解")[liang_gdong](http://hi.csdn.net/liang_gdong) 发表于Mon Nov 30 2009 16:06:18 GMT+0800 (China Standard Time)  IP:[举报](mailto:webmaster@csdn.net?subject=Comment%20Report!!!&body=Author:liang_gdong%20URL:http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx)[回复]()[删除]()![]()好东西，支持[](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#1211462 "permalink: Log4j写入数据库详解")[mwj0103](http://hi.csdn.net/mwj0103) 发表于Wed Jan 06 2010 13:24:41 GMT+0800 (China Standard Time)  IP:[举报](mailto:webmaster@csdn.net?subject=Comment%20Report!!!&body=Author:mwj0103%20URL:http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx)[回复]()[删除]()![]()![]()太让人感动了！~[](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#1219539 "permalink: Log4j写入数据库详解")[iceyrain](http://hi.csdn.net/iceyrain) 发表于Tue Jan 12 2010 10:22:37 GMT+0800 (China Standard Time)  IP:[举报](mailto:webmaster@csdn.net?subject=Comment%20Report!!!&body=Author:iceyrain%20URL:http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx)[回复]()[删除]()![]()很牛， 受教了 谢谢
* 发表评论
 
* 表 情：
* [![顶]( "顶")](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#) [![砸]( "砸")](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#) [![棒]( "棒")](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#) [![大笑]( "大笑")](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#) [![愤怒]( "愤怒")](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#) [![大哭]( "大哭")](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#) [![疑问]( "疑问")](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#) [![汗]( "汗")](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#) [![呕吐]( "呕吐")](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#) [![送花]( "送花")](http://blog.csdn.net/ziruobing/archive/2009/02/22/3919501.aspx#)

* 评论内容：
*
* 用 户 名：
* [登录]() [注册](http://passport.csdn.net/CSDNUserRegister.aspx)  匿名评论 匿名用户

* 验 证 码：
* [![验证码]()]() [重新获得验证码]()

*  
*

### 专区推荐内容

* [哥是传奇—组团参赛心得](http://articles.csdn.net/plus/view.php?aid=298921 "哥是传奇—组团参赛心得")
* [【教程】Windows平台下MeeGo v1.2 SDK的安装](http://articles.csdn.net/plus/view.php?aid=298920 "【教程】Windows平台下MeeGo v1.2 SDK的安装")
* [MeeGo 1.2 正式版发布](http://articles.csdn.net/plus/view.php?aid=298814 "MeeGo 1.2 正式版发布")
* [在生命走到尽头前用脚贡献了最后一个代码补丁](http://articles.csdn.net/plus/view.php?aid=298657 "在生命走到尽头前用脚贡献了最后一个代码补丁")
* [浅谈QT中窗口刷新事件](http://articles.csdn.net/plus/view.php?aid=298656 "浅谈QT中窗口刷新事件")
* [赢笔记本电脑，提升管理软件新水平！](http://articles.csdn.net/plus/view.php?aid=298655 "赢笔记本电脑，提升管理软件新水平！")
* [【教程】安装MeeGO和Windows 7双系统的方法](http://articles.csdn.net/plus/view.php?aid=298548 "【教程】安装MeeGO和Windows 7双系统的方法")
* [分享我的个人初赛体会](http://articles.csdn.net/plus/view.php?aid=298449 "分享我的个人初赛体会")
* [【免费】参与移动应用投票赢手机话费](http://articles.csdn.net/plus/view.php?aid=298309 "【免费】参与移动应用投票赢手机话费")
* [Nokia宣布Qt 5计划](http://articles.csdn.net/plus/view.php?aid=298307 "Nokia宣布Qt 5计划")
* [立即加入IBM dW，万千技术尽网罗](http://articles.csdn.net/plus/view.php?aid=298138 "立即加入IBM dW，万千技术尽网罗")
* [Linux 上简单的MeeGo 开发 QT 程序](http://articles.csdn.net/plus/view.php?aid=298040 "Linux 上简单的MeeGo 开发 QT 程序")
* [软件产品性能优化注意事项](http://articles.csdn.net/plus/view.php?aid=298039 "软件产品性能优化注意事项")
* [用C#实现HTTP协议下的多线程文件传输](http://articles.csdn.net/plus/view.php?aid=297526 "用C#实现HTTP协议下的多线程文件传输")
* [【实战】搭建Meego Tablet开发测试平台](http://articles.csdn.net/plus/view.php?aid=297525 "【实战】搭建Meego Tablet开发测试平台")
* [AppUp Center为更多程序员创造机会](http://articles.csdn.net/plus/view.php?aid=297421 "AppUp Center为更多程序员创造机会")
* [【源码分享】一个多线程下载文件的程序](http://articles.csdn.net/plus/view.php?aid=297420 "【源码分享】一个多线程下载文件的程序")
* [轻松漫画聊快速构建网站](http://articles.csdn.net/plus/view.php?aid=297204 "轻松漫画聊快速构建网站")
* [如何创建一个简单的Qt应用程序](http://articles.csdn.net/plus/view.php?aid=296905 "如何创建一个简单的Qt应用程序")
* [【赢取旧金山之旅】2011线程挑战赛](http://articles.csdn.net/plus/view.php?aid=296820 "【赢取旧金山之旅】2011线程挑战赛")
* [【图】爱上NOOK COLOR的5个理由](http://articles.csdn.net/plus/view.php?aid=296709 "【图】爱上NOOK COLOR的5个理由")
* [IPAD&amp;NOOK COLOR屏幕对比多图](http://articles.csdn.net/plus/view.php?aid=296610 "IPAD&amp;NOOK COLOR屏幕对比多图")
* [【教程】AppUp 进阶基础篇](http://articles.csdn.net/plus/view.php?aid=296533 "【教程】AppUp 进阶基础篇")
* [Nokia CEO：下一步会与谁合作？](http://articles.csdn.net/plus/view.php?aid=296502 "Nokia CEO：下一步会与谁合作？")
* [点评三星Smart TV智能电视](http://articles.csdn.net/plus/view.php?aid=296501 "点评三星Smart TV智能电视")
* [太震撼了！首次参加IDF有感](http://articles.csdn.net/plus/view.php?aid=296407 "太震撼了！首次参加IDF有感")
* [【教程】基于VC色温图效果实现](http://articles.csdn.net/plus/view.php?aid=296334 "【教程】基于VC色温图效果实现")
* [【教程】游戏技巧特效处理](http://articles.csdn.net/plus/view.php?aid=296333 "【教程】游戏技巧特效处理")
* [Firefox 4在meego上成功安装](http://articles.csdn.net/plus/view.php?aid=296243 "Firefox 4在meego上成功安装")
* [IDF2011：多图详解MeeGo 3月后正式发布](http://articles.csdn.net/plus/view.php?aid=296239 "IDF2011：多图详解MeeGo 3月后正式发布")
* [PayPal助力移动支付应用](http://articles.csdn.net/plus/view.php?aid=296238 "PayPal助力移动支付应用")
* [Android应用换电视，前30名有效！](http://articles.csdn.net/plus/view.php?aid=296079 "Android应用换电视，前30名有效！")
* [【教程】笔记本安装MeeGo](http://articles.csdn.net/plus/view.php?aid=296068 "【教程】笔记本安装MeeGo")
* [微软BI解决方案开发简介](http://articles.csdn.net/plus/view.php?aid=296067 "微软BI解决方案开发简介")
* [下载Windows Phone 中文培训包](http://articles.csdn.net/plus/view.php?aid=296064 "下载Windows Phone 中文培训包")
* [下载 Windows Phone 开发工具](http://articles.csdn.net/plus/view.php?aid=296060 "下载 Windows Phone 开发工具")
* [全新Windows Phone 开发中心](http://articles.csdn.net/plus/view.php?aid=296059 "全新Windows Phone 开发中心")
* [VS2010 SharePoint 入门](http://articles.csdn.net/plus/view.php?aid=295728 "VS2010 SharePoint 入门")
* [【免费下载】WebMatrix建站工具](http://articles.csdn.net/plus/view.php?aid=295726 "【免费下载】WebMatrix建站工具")
* [AIX 专区有奖话题讨论](http://articles.csdn.net/plus/view.php?aid=295725 "AIX 专区有奖话题讨论")
* [4.21日Adobe企业RIA开发者研讨会](http://articles.csdn.net/plus/view.php?aid=295724 "4.21日Adobe企业RIA开发者研讨会")
* [MeeGo中文社区全新呈现](http://articles.csdn.net/plus/view.php?aid=295713 "MeeGo中文社区全新呈现")
* [羡慕嫉妒恨！MeeGO平板到手](http://articles.csdn.net/plus/view.php?aid=299402 "羡慕嫉妒恨！MeeGO平板到手")
* [MeeGo SDK 1.2 for Linux 初窥](http://articles.csdn.net/plus/view.php?aid=299401 "MeeGo SDK 1.2 for Linux 初窥")
* [2011台北国际电脑展开幕（5.31-6.4）](http://articles.csdn.net/plus/view.php?aid=299243 "2011台北国际电脑展开幕（5.31-6.4）")
* [关于QT编程入门的那些事](http://articles.csdn.net/plus/view.php?aid=299235 "关于QT编程入门的那些事")
* [相见 ——“人生若只如初见”](http://articles.csdn.net/plus/view.php?aid=299234 "相见 ——“人生若只如初见”")
* [游戏远程代码注入和动态连接库的使用](http://articles.csdn.net/plus/view.php?aid=299094 "游戏远程代码注入和动态连接库的使用")
 << >>

### 热门招聘职位 【[更多](http://special.csdn.net/zhaopin/index.html)】

* [【傲盾软件】高薪诚聘C、C++、JAVA、cavium/linux工程师](http://g.csdn.net/5185888)
* [【道达天际】高薪诚聘：渗透测试、网络安全、逆向技术分析 专业人才](http://g.csdn.net/5185906)
* [诚聘高级软件工程师，架构师，待遇从优](http://g.csdn.net/5185702)
* [【山重融资】诚聘软件开发工程师、网络管理工程师](http://g.csdn.net/5185637)
* [【卓坤信息】诚聘MFC高级客户端、网页设计师、PHP开发等](http://g.csdn.net/5185599)
* [爱福康诚招：C++ / Directshow 软件工程师](http://g.csdn.net/5185494)
* [【天健集团】诚聘架构师，高级软件开发工程师（.NET、PB、J2EE）,实施人员](http://g.csdn.net/5185490)
* [爱唱数码诚聘 研发经理&程序员](http://g.csdn.net/5184976)
* [【北京联银通科技有限制公司】高薪诚聘技术经理、高级工程师等职位](http://g.csdn.net/5184945)
* [【安博教育】诚聘软件开发、架构师、技术总监等技术人才](http://g.csdn.net/5184844)
* [【Autodesk】欧特克软件(中国)诚聘软件开发,测试,研究员](http://g.csdn.net/5162966)
* [【北京平川嘉恒】团队Leader及客户端/服务器端研发工程师](http://g.csdn.net/5185477)
* [【欢网科技】诚聘系统架构师、需求分析师、开发工程师](http://g.csdn.net/5183628)
* [【酷狗音乐】诚聘VC、服务端开发工程师等职位](http://g.csdn.net/5183513)
* [【法国电信】诚聘研发类人才](http://g.csdn.net/5182525)
* [【飞漫公司】诚聘C/C++研发工程师、软件测试等！](http://g.csdn.net/5183220)
* [【上海电科智能】高新诚聘JAVA和C#等软件工程师](http://g.csdn.net/5182403)
* [【仙掌软件】高新诚聘java、android、iPhone软件工程师等职位，期待您的加入！](http://g.csdn.net/5182398)
* [【完美世界】（原完美时空）诚聘各类游戏领域人才](http://g.csdn.net/5181583)
* [【亿阳信通】诚邀您的加盟！](http://g.csdn.net/5180531)
* [【Amazon】亚马逊诚聘技术专家！](http://g.csdn.net/5182687)
* [【航天信息股份有限公司】诚聘系统架构，需求分析、JAVA开发、C/C++开发研发岗位热招中](http://g.csdn.net/5180413)
* [【杭州引力】高薪诚聘ios开发人员](http://g.csdn.net/5180371)
* [【 CSDN】高薪诚聘：java、运营、就业、商务策划经理、网站编辑！](http://g.csdn.net/5178940)
* [【傲盾软件】高薪诚聘C、C++、JAVA、cavium/linux工程师](http://g.csdn.net/5185888)
* [【道达天际】高薪诚聘：渗透测试、网络安全、逆向技术分析 专业人才](http://g.csdn.net/5185906)
* [诚聘高级软件工程师，架构师，待遇从优](http://g.csdn.net/5185702)
* [【山重融资】诚聘软件开发工程师、网络管理工程师](http://g.csdn.net/5185637)
* [【卓坤信息】诚聘MFC高级客户端、网页设计师、PHP开发等](http://g.csdn.net/5185599)
* [爱福康诚招：C++ / Directshow 软件工程师](http://g.csdn.net/5185494)
* [【天健集团】诚聘架构师，高级软件开发工程师（.NET、PB、J2EE）,实施人员](http://g.csdn.net/5185490)
* [爱唱数码诚聘 研发经理&程序员](http://g.csdn.net/5184976)
* [【北京联银通科技有限制公司】高薪诚聘技术经理、高级工程师等职位](http://g.csdn.net/5184945)
* [【安博教育】诚聘软件开发、架构师、技术总监等技术人才](http://g.csdn.net/5184844)
* [【Autodesk】欧特克软件(中国)诚聘软件开发,测试,研究员](http://g.csdn.net/5162966)
* [【北京平川嘉恒】团队Leader及客户端/服务器端研发工程师](http://g.csdn.net/5185477)
* [【欢网科技】诚聘系统架构师、需求分析师、开发工程师](http://g.csdn.net/5183628)
* [【酷狗音乐】诚聘VC、服务端开发工程师等职位](http://g.csdn.net/5183513)
* [【法国电信】诚聘研发类人才](http://g.csdn.net/5182525)
* [【飞漫公司】诚聘C/C++研发工程师、软件测试等！](http://g.csdn.net/5183220)
* [【上海电科智能】高新诚聘JAVA和C#等软件工程师](http://g.csdn.net/5182403)
* [【仙掌软件】高新诚聘java、android、iPhone软件工程师等职位，期待您的加入！](http://g.csdn.net/5182398)
* [【完美世界】（原完美时空）诚聘各类游戏领域人才](http://g.csdn.net/5181583)
* [【亿阳信通】诚邀您的加盟！](http://g.csdn.net/5180531)
* [【Amazon】亚马逊诚聘技术专家！](http://g.csdn.net/5182687)
* [【航天信息股份有限公司】诚聘系统架构，需求分析、JAVA开发、C/C++开发研发岗位热招中](http://g.csdn.net/5180413)
* [【杭州引力】高薪诚聘ios开发人员](http://g.csdn.net/5180371)
* [【 CSDN】高薪诚聘：java、运营、就业、商务策划经理、网站编辑！](http://g.csdn.net/5178940)
![]()
[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)北京创新乐知信息技术有限公司 版权所有, 京 ICP 证 070598 号世纪乐知(北京)网络技术有限公司 提供技术支持江苏乐知网络技术有限公司 提供商务支持![]() Email:webmaster@csdn.netCopyright © 1999-2011, CSDN.NET, All Rights Reserved[![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010) ![]() ![]()
