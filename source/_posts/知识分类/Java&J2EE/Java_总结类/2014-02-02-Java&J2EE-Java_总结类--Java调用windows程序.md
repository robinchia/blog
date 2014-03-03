---
layout: post
title: "Java调用windows程序"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# Java调用windows程序

[**Derek.Guo BLOG**](http://www.blogjava.net/envoydada/)

[BlogJava](http://www.blogjava.net/)   [首页](http://www.blogjava.net/envoydada/)   [新随笔](http://www.blogjava.net/envoydada/admin/EditPosts.aspx?opt=1) [联系](http://www.blogjava.net/envoydada/contact.aspx?id=1)   [聚合](http://www.blogjava.net/envoydada/rss)[![]()](http://www.blogjava.net/envoydada/rss)   [管理](http://www.blogjava.net/envoydada/admin/EditPosts.aspx)

随笔-75  评论-29  文章-0  trackbacks-0
[Java调用windows程序](http://www.blogjava.net/envoydada/archive/2006/05/11/45645.html)     Runtime ru = Runtime.getRuntime();
        try {
            //调用播放器文件播放指定MP3
            Process p1 = ru.exec("C:\\Program Files\\Windows Media Player\\wmplayer d:\\DADA\\mp3\\0197.mp3");
           //调用批处理文件 
            Process p2 = ru.exec("d:\\a.bat");
            //显示执行结果
            BufferedReader br = new BufferedReader(new InputStreamReader(p.getInputStream()));
            String line;
            while((line=br.readLine())!=null){
                System.out.println(line);
            }
        } catch (IOException ex) {ex.printStackTrace();}

posted on 2006-05-11 13:27 [Derek.Guo](http://www.blogjava.net/envoydada/) 阅读(602) [评论(0)](http://www.blogjava.net/envoydada/archive/2006/05/11/45645.html#Post)  [编辑](http://www.blogjava.net/envoydada/admin/EditPosts.aspx?postid=45645)  [收藏](http://www.blogjava.net/envoydada/AddToFavorite.aspx?id=45645) 所属分类: [JAVA](http://www.blogjava.net/envoydada/category/32099.html)
![]()

[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() IT新闻：
· [下一代iPhone将于6月22日公布](http://news.cnblogs.com/n/61666/)
· [英特尔发力Smart TV 未与索尼谷歌推Google TV](http://news.cnblogs.com/n/61665/)
· [传魔兽计划回购废弃老帐号 玩家反应激烈](http://news.cnblogs.com/n/61664/)
· [PCWorld：谷歌最有望推出“iPad杀手”](http://news.cnblogs.com/n/61663/)
· [访问 T.J：16 岁的 iPad 软件开发者](http://news.cnblogs.com/n/61662/)
专题：[Android](http://kb.cnblogs.com/zt/android/)  [iPad](http://kb.cnblogs.com/zt/iPad/)  [jQuery](http://kb.cnblogs.com/zt/jquery/)  [Chrome OS](http://kb.cnblogs.com/zt/chrome/)    [博客园首页](http://www.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [知识库](http://kb.cnblogs.com/)  [学英语](http://a4.yeshj.com/rd/34143/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/envoydada/archive/2006/05/11/45645.html&SourceURL=/envoydada/archive/2006/05/11/45645.html)       [使用Ctrl+Enter键可以直接提交]   [每天10分钟，轻松学英语](http://a4.yeshj.com/rd/34138/)  推荐职位：
· [ASP.NET高级程序员(北京世纪英博)](http://job.cnblogs.com/offer/6695/)
· [飞信服务器端高级.NET开发工程师(新媒传信)](http://job.cnblogs.com/offer/6318/)
· [.NET飞信官网开发工程师(新媒传信)](http://job.cnblogs.com/offer/6319/)
· [Web前端研发工程师(百度)](http://job.cnblogs.com/offer/6492/)
· [C++开发工程师(沪江网)](http://job.cnblogs.com/zt/yeshj/#offer3)
· [前端开发工程师(沪江网)](http://job.cnblogs.com/zt/yeshj/#offer2)
· [产品经理(沪江网)](http://job.cnblogs.com/zt/yeshj/#offer6)
· [运维工程师(沪江网)](http://job.cnblogs.com/zt/yeshj/#offer9)

博客园首页随笔：
· [GridView自定义分页样式（上一页，下一页，到第几页）（新手教程）](http://www.cnblogs.com/xuanhun/archive/2010/04/14/1712132.html)
· [升级到NVelocity1.1版本](http://www.cnblogs.com/liubiqu/archive/2010/04/14/1712126.html)
· [Windows 7生成系统健康报告](http://www.cnblogs.com/greenerycn/archive/2010/04/14/win7_system_health_report.html)
· [继"一题比较刁的面试题" - 其实也不过如此。 sliverlight 实现](http://www.cnblogs.com/tandly/archive/2010/04/14/1712111.html)
· [更改MVC注册Areas的顺序，掌控Areas的运作](http://www.cnblogs.com/dozer/archive/2010/04/14/change-order-of-MVC-Areas.html)
知识库：
· [20%顶尖人才的成功密码](http://kb.cnblogs.com/page/61564/)
· [IT经理世界：被低估的用友](http://kb.cnblogs.com/page/61529/)
· [大型机Linux 风雨十周年](http://kb.cnblogs.com/page/61479/)
· [和谐共进的项目组——产品经理提高技术理解力123](http://kb.cnblogs.com/page/61450/)
· [IIS配置PHP环境（快速最新版）](http://kb.cnblogs.com/page/61439/)  网站导航:

[博客园](http://www.cnblogs.com/)   [IT新闻](http://news.cnblogs.com/)   [个人主页](http://home.cnblogs.com/)   [博客生活](http://www.cnweblog.com/)   [IT博客网](http://www.cnitblog.com/)   [C++博客](http://www.cppblog.com/)   [博客园社区](http://space.cnblogs.com/) [管理](http://www.blogjava.net/envoydada/archive/2006/05/11/45645.html?opt=admin) 相关文章:

* [J2SE6 分析工具](http://www.blogjava.net/envoydada/archive/2010/04/08/317713.html)
* [Memcached 剖析(转)](http://www.blogjava.net/envoydada/archive/2008/09/28/231708.html)
* [Hibernate属性延迟加载](http://www.blogjava.net/envoydada/archive/2007/09/20/146797.html)
* [JAVA缩放图片（转贴）](http://www.blogjava.net/envoydada/archive/2007/08/22/138549.html)
* [工具分析GC日志](http://www.blogjava.net/envoydada/archive/2007/07/30/133329.html)
* [java虚拟机参数详解](http://www.blogjava.net/envoydada/archive/2007/07/23/131803.html)
* [GC调优](http://www.blogjava.net/envoydada/archive/2007/07/20/131433.html)
* [Spring配置总结](http://www.blogjava.net/envoydada/archive/2007/07/20/131410.html)
* [JmakiDemo](http://www.blogjava.net/envoydada/archive/2007/07/04/128069.html)
* [Hibernate3支持DetachedCriteria(转贴)](http://www.blogjava.net/envoydada/archive/2007/06/08/122881.html)  

[<]( "Go to the previous month") 2006年5月 [>]( "Go to the next month") 日 一 二 三 四 五 六 30 1 2 3 4 5 6 7 8 9 [10](http://www.blogjava.net/envoydada/archive/2006/05/10.html) [11](http://www.blogjava.net/envoydada/archive/2006/05/11.html) 12 13 14 15 [16](http://www.blogjava.net/envoydada/archive/2006/05/16.html) [17](http://www.blogjava.net/envoydada/archive/2006/05/17.html) 18 [19](http://www.blogjava.net/envoydada/archive/2006/05/19.html) 20 21 22 23 24 25 26 27 28 29 30 31 1 2 3 4 5 6 7 8 9 10

### 留言簿(6)

* [给我留言](http://www.blogjava.net/envoydada/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/envoydada/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/envoydada/admin/MyMessages.aspx)

# 随笔分类(73)

* [DATABASE(9)](http://www.blogjava.net/envoydada/category/32101.html)[![]( "Subscribe to DATABASE(9)")](http://www.blogjava.net/envoydada/category/32101.html/rss "Subscribe to DATABASE(9)")
* [JAVA(52)](http://www.blogjava.net/envoydada/category/32099.html)[![]( "Subscribe to JAVA(52)")](http://www.blogjava.net/envoydada/category/32099.html/rss "Subscribe to JAVA(52)")
* [LINUX/UNIX(12)](http://www.blogjava.net/envoydada/category/32100.html)[![]( "Subscribe to LINUX/UNIX(12)")](http://www.blogjava.net/envoydada/category/32100.html/rss "Subscribe to LINUX/UNIX(12)")

### 积分与排名

* 积分 - 75149
* 排名 - 196

### 最新随笔

* [1. mysql常用的hint](http://www.blogjava.net/envoydada/archive/2010/04/08/317718.html)
* [2. Mysql innodb引擎优化](http://www.blogjava.net/envoydada/archive/2010/04/08/317716.html)
* [3. J2SE6 分析工具](http://www.blogjava.net/envoydada/archive/2010/04/08/317713.html)
* [4. liunx下安装Subversion](http://www.blogjava.net/envoydada/archive/2010/04/08/317711.html)
* [5. ORACLE 中dbms_stats的使用](http://www.blogjava.net/envoydada/archive/2009/02/07/253698.html)
* [6. Memcached 剖析(转)](http://www.blogjava.net/envoydada/archive/2008/09/28/231708.html)
* [7. Window下配置SVN服务器与客户端(转)](http://www.blogjava.net/envoydada/archive/2008/07/29/218323.html)
* [8. Oracle 10g Recycle Bin](http://www.blogjava.net/envoydada/archive/2008/06/18/208808.html)
* [9. Oracle中分区表的使用](http://www.blogjava.net/envoydada/archive/2008/06/16/208221.html)
* [10. ORACLE CTXCAT-CATSEARCH](http://www.blogjava.net/envoydada/archive/2008/06/12/207472.html)
* [11. ORACLE JOBS](http://www.blogjava.net/envoydada/archive/2008/06/12/207288.html)
* [12. Oracle 分区表(Partition)](http://www.blogjava.net/envoydada/archive/2008/06/11/207090.html)
* [13. Solaris系统进程的查看和管理](http://www.blogjava.net/envoydada/archive/2008/06/11/207028.html)
* [14. 数据库性能 常用SQL](http://www.blogjava.net/envoydada/archive/2007/12/27/170920.html)
* [15. Solaris 操作系统的动态跟踪工具Dtrace](http://www.blogjava.net/envoydada/archive/2007/09/29/149640.html)
* [16. Hibernate属性延迟加载](http://www.blogjava.net/envoydada/archive/2007/09/20/146797.html)
* [17. JAVA缩放图片（转贴）](http://www.blogjava.net/envoydada/archive/2007/08/22/138549.html)
* [18. Linux自启Tomcat,Solaris自启glassfish](http://www.blogjava.net/envoydada/archive/2007/08/01/133838.html)
* [19. 工具分析GC日志](http://www.blogjava.net/envoydada/archive/2007/07/30/133329.html)
* [20. JProfiler远程监控Tomcat](http://www.blogjava.net/envoydada/archive/2007/07/30/133285.html)

### 最新评论 [![]()](http://www.blogjava.net/envoydada/CommentsRSS.aspx)

* [1. re: JAVA缩放图片（转贴）](http://www.blogjava.net/envoydada/archive/2010/04/07/138549.html#317660)
* 希望能用
* --moguji
* [2. re: Hibernate 本地SQL查询SQLQuery[未登录]](http://www.blogjava.net/envoydada/archive/2009/10/20/106322.html#299083)
* 很好，谢谢啦
* --xx
* [3. re: Hibernate 本地SQL查询SQLQuery](http://www.blogjava.net/envoydada/archive/2009/09/30/106322.html#296990)
* 很不错啊， 谢谢。。
* --carvin
* [4. re: JProfiler远程监控Tomcat[未登录]](http://www.blogjava.net/envoydada/archive/2009/09/30/133285.html#296976)
* 有没有发生tomcat端口打不开的情况。
* --sun
* [5. re: Spring+Hibernate+Struts](http://www.blogjava.net/envoydada/archive/2009/09/03/45438.html#293687)
* 能把整个例子打包也发给我吗，我试了很久都不行啊。 zhaos@sunward.com.cn
* --zhaos

### 阅读排行榜

* [1. Hibernate批量更新和批量删除(6416)](http://www.blogjava.net/envoydada/archive/2005/09/13/12894.html)
* [2. Hibernate 本地SQL查询SQLQuery(6106)](http://www.blogjava.net/envoydada/archive/2007/03/26/106322.html)
* [3. Spring+Hibernate+Struts(4241)](http://www.blogjava.net/envoydada/archive/2006/05/10/45438.html)
* [4. JProfiler远程监控Tomcat(3292)](http://www.blogjava.net/envoydada/archive/2007/07/30/133285.html)
* [5. Spring+hibernate分页查询(2853)](http://www.blogjava.net/envoydada/archive/2007/03/06/102152.html)
* [6. Spring DataSource注入(2794)](http://www.blogjava.net/envoydada/archive/2006/11/07/79516.html)
* [7. Spring Hibernate 模板实现分页(2697)](http://www.blogjava.net/envoydada/archive/2006/08/28/66182.html)
* [8. Hibernate one-to-many学习笔记(2340)](http://www.blogjava.net/envoydada/archive/2006/09/07/68308.html)
* [9. Apache + Tomcat*2集群 负载平衡(Linux环境)(2209)](http://www.blogjava.net/envoydada/archive/2006/11/15/81196.html)
* [10. WEB定时器-Timer(1766)](http://www.blogjava.net/envoydada/archive/2006/04/26/43355.html)
* [11. Struts中logic:iterate标记的使用(1759)](http://www.blogjava.net/envoydada/archive/2005/09/11/12654.html)
* [12. Tomcat内存配置(1592)](http://www.blogjava.net/envoydada/archive/2006/11/10/80309.html)
* [13. Tomcat5.0连接池(1583)](http://www.blogjava.net/envoydada/archive/2005/09/11/12655.html)
* [14. Java调用Linux命令(1542)](http://www.blogjava.net/envoydada/archive/2007/05/08/115877.html)
* [15. 工具分析GC日志(1374)](http://www.blogjava.net/envoydada/archive/2007/07/30/133329.html)
* [16. java虚拟机参数详解(1310)](http://www.blogjava.net/envoydada/archive/2007/07/23/131803.html)
* [17. Hibernate-Extension和Middlegen-Hibernate(1292)](http://www.blogjava.net/envoydada/archive/2005/09/11/12679.html)
* [18. Hibernate主键生成方式(1226)](http://www.blogjava.net/envoydada/archive/2006/04/20/42153.html)
* [19. JAVA访问LDAP(1202)](http://www.blogjava.net/envoydada/archive/2007/05/09/116195.html)
* [20. Spring配置总结(1170)](http://www.blogjava.net/envoydada/archive/2007/07/20/131410.html)
* [21. Struts常用标签(1043)](http://www.blogjava.net/envoydada/archive/2006/03/28/37791.html)
* [22. ORACLE 中dbms_stats的使用(1037)](http://www.blogjava.net/envoydada/archive/2009/02/07/253698.html)
* [23. EL表达式(1029)](http://www.blogjava.net/envoydada/archive/2006/08/30/66559.html)
* [24. Hibernate3.0批量更新和批量删除(938)](http://www.blogjava.net/envoydada/archive/2006/03/15/35434.html)
* [25. GC调优(928)](http://www.blogjava.net/envoydada/archive/2007/07/20/131433.html)
* [26. hibernate二级缓存攻略 Ehcache(转贴)(911)](http://www.blogjava.net/envoydada/archive/2006/12/15/87862.html)
* [27. Hibernate属性延迟加载(908)](http://www.blogjava.net/envoydada/archive/2007/09/20/146797.html)
* [28. Tomcat 通过数据库验证的配置方法(BASIC,FORM).(881)](http://www.blogjava.net/envoydada/archive/2006/11/07/79585.html)
* [29. RedHat终端中文乱码解决(869)](http://www.blogjava.net/envoydada/archive/2006/10/12/74758.html)
* [30. JAVA的RSS阅读器(793)](http://www.blogjava.net/envoydada/archive/2006/08/15/63743.html)
* [31. Log4j配置(784)](http://www.blogjava.net/envoydada/archive/2006/01/18/28446.html)
* [32. JAVA缩放图片（转贴）(774)](http://www.blogjava.net/envoydada/archive/2007/08/22/138549.html)
* [33. DES加密(722)](http://www.blogjava.net/envoydada/archive/2006/05/19/46964.html)
* [34. 数据库性能 常用SQL(711)](http://www.blogjava.net/envoydada/archive/2007/12/27/170920.html)
* [35. Spring 定时器(655)](http://www.blogjava.net/envoydada/archive/2006/06/05/50423.html)
* [36. Glassfish的安装(636)](http://www.blogjava.net/envoydada/archive/2007/07/24/132080.html)
* [37. SQL SERVER事务处理(613)](http://www.blogjava.net/envoydada/archive/2006/08/04/61650.html)
* [38. Java调用windows程序(601)](http://www.blogjava.net/envoydada/archive/2006/05/11/45645.html)
* [39. Hibernate3支持DetachedCriteria(转贴)(596)](http://www.blogjava.net/envoydada/archive/2007/06/08/122881.html)
* [40. JavaMail核心类(581)](http://www.blogjava.net/envoydada/archive/2006/03/23/37042.html)
Powered by: [博客园](http://www.cnblogs.com/) 模板提供：[沪江博客](http://blog.hjenglish.com/) Copyright ©2010 Derek.Guo

MSN:envoydada@hotmail.com QQ:34935442
