---
layout: post
title: "动态跟踪Java代码的执行状况工具--BTrace"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# 动态跟踪Java代码的执行状况工具--BTrace

# [BlueDavy之技术Blog](http://www.blogjava.net/BlueDavy/)

理论不懂就实践，实践不会就学理论！

## [动态跟踪Java代码的执行状况工具--BTrace](http://www.blogjava.net/BlueDavy/archive/2009/10/10/297661.html)

非常强烈的推荐下BTrace这个工具，用了后不得不说太强大了，BTrace简单来说，就是能在不改动当前程序的情况下，运行时的去监控Java程序的执行状况，例如可以做到内存状况的监控、方法调用的监控等等，官方网站上有非常多详细的例子，我不说太多，只在下面举一个简单的例子来说明它的作用，BTrace的User Guide请见：[http://kenai.com/projects/btrace/pages/UserGuide](http://kenai.com/projects/btrace/pages/UserGuide)。
对于运行中的Java程序，尤其是出了问题的程序，会需要跟踪其执行状况，例如传入的参数是什么、执行了多少时间，返回的对象是什么，抛出了什么异常，传统的做法只能是把程序改一遍，加上一堆log，一个例子来展示下用BTrace的情况下，怎么来跟踪一个方法的执行时间：
@BTrace public class MethodResponseTime {
    
    @TLS private static long startTime;
    
    @OnMethod(clazz="类名",method="方法名")
    public static void onCall(){
        println("enter this method");
        startTime=timeMillis();
    }
    
    @OnMethod(clazz="类名",method="方法名",location=@Location(Kind.RETURN))
    public static void onReturn(){
        println("method end!");
        println(strcat("Time taken ms",str(timeMillis()-startTime)));
    }
    
}
用btrace执行上面的代码，就可以动态的监控任意的目前运行的Java程序中某类的某方法的执行时间，执行上面代码的方式如下（jdk 6+）：
btrace [pid] MethodResponseTime.class
还有例如获取调用参数、调用者的对象实例以及返回值等请参看User Guide。
btrace为了保持JVM运行的安全性，因此做了很多的限制，例如不能抛出异常、修改传入的参数的值、修改返回值等，基本是一个只读的动态分析代码运行状况的工具，但仍然是非常的有用，其实现机制是attach api + asm +  instrumentation。

posted on 2009-10-10 12:41 [BlueDavy](http://www.blogjava.net/BlueDavy/) 阅读(11121) [评论(8)]()  [编辑](http://www.blogjava.net/BlueDavy/admin/EditPosts.aspx?postid=297661)  [收藏](http://www.blogjava.net/BlueDavy/AddToFavorite.aspx?id=297661) 所属分类: [Java](http://www.blogjava.net/BlueDavy/category/1366.html)

 ![]()

[]() []()

[
### 评论

]()

### []()[#]( "permalink: re: 动态跟踪Java代码的执行状况工具--BTrace") []()re: 动态跟踪Java代码的执行状况工具--BTrace  2009-10-10 14:02  [argan](http://argan.javaeye.com/)

不错，这个有点强悍的，使用一下先  [回复]()  [更多评论](http://www.blogjava.net/comment?author=argan "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: 动态跟踪Java代码的执行状况工具--BTrace") []()re: 动态跟踪Java代码的执行状况工具--BTrace  2009-10-11 01:02  [隔叶黄莺](http://www.blogjava.net/Unmi/)

收藏了，以后可能用得着。  [回复]()  [更多评论](http://www.blogjava.net/comment?author=%e9%9a%94%e5%8f%b6%e9%bb%84%e8%8e%ba "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: 动态跟踪Java代码的执行状况工具--BTrace[未登录]") []()re: 动态跟踪Java代码的执行状况工具--BTrace[未登录]  2009-10-11 23:28  [海边沫沫](http://www.blogjava.net/youxia)

要钱吗?  [回复]()  [更多评论](http://www.blogjava.net/comment?author=%e6%b5%b7%e8%be%b9%e6%b2%ab%e6%b2%ab "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: 动态跟踪Java代码的执行状况工具--BTrace") []()re: 动态跟踪Java代码的执行状况工具--BTrace  2009-10-12 03:54  [duguo](http://duguo.com/)

试了一下trace Apache Felix 2.0.0, 已加载了的类不好使，抛出NullPointerException。  [回复]()  [更多评论](http://www.blogjava.net/comment?author=duguo "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: 动态跟踪Java代码的执行状况工具--BTrace") []()re: 动态跟踪Java代码的执行状况工具--BTrace  2009-10-12 17:12  [weager](http://www.blogjava.net/dongritengfei/)

貌似没有什么界面，还得用命令行啊~~~  [回复]()  [更多评论](http://www.blogjava.net/comment?author=weager "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: 动态跟踪Java代码的执行状况工具--BTrace") []()re: 动态跟踪Java代码的执行状况工具--BTrace  2009-10-13 00:13  [gengmao]()

visualvm有btrace的插件  [回复]()  [更多评论](http://www.blogjava.net/comment?author=gengmao "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: 动态跟踪Java代码的执行状况工具--BTrace") []()re: 动态跟踪Java代码的执行状况工具--BTrace  2009-10-13 00:57  [sswv](http://blog.linjian.org/)

林昊朋友，你好，
首先祝贺你的blog被ZDNet评为了“最受欢迎中国技术博客”之一。
不过你的照片……不知你弄错了还是ZDNet弄错了，是我的头像呵。
详情参考： [http://blog.linjian.org/articles/my-photo-misused-again/](http://blog.linjian.org/articles/my-photo-misused-again/)
有机会看看你写的书。我前不久也与博文视点合作过，是《我是一只IT小小鸟》的合作者之一。  [回复]()  [更多评论](http://www.blogjava.net/comment?author=sswv "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: 动态跟踪Java代码的执行状况工具--BTrace[未登录]") []()re: 动态跟踪Java代码的执行状况工具--BTrace[未登录][]()  2009-10-13 08:13  [BlueDavy](http://www.blogjava.net/bluedavy)

@sswv
...我并不知道ZDNET评选这件事情，我联系下他们吧，不好意思了，<IT小小鸟>可是现在相当火的书，非常恭喜。
  [回复]()  [更多评论](http://www.blogjava.net/comment?author=BlueDavy "查看该作者发表过的评论") []()  []()
[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [推荐购买云服务器（15%返利+最高千元奖金）](http://www.aliyun.com/cps/channel?channel_id=1306&user=0&lv=1)   [博客园](http://www.cnblogs.com/)  [博问](http://q.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![](http://www.blogjava.net/Modules/CaptchaImage/JpegImage.aspx?cacheid=20130727130706) 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/BlueDavy/archive/2009/10/10/297661.html&SourceURL=/BlueDavy/archive/2009/10/10/297661.html)       [使用Ctrl+Enter键可以直接提交]      网站导航:

[博客园](http://www.cnblogs.com/ "程序员的网上家园")   [IT新闻](http://news.cnblogs.com/)   [知识库](http://kb.cnblogs.com/)   [C++博客](http://www.cppblog.com/)   [程序员招聘](http://job.cnblogs.com/)   [管理](http://www.blogjava.net/BlueDavy/archive/2009/10/10/297661.html?opt=admin) 相关文章:

* [关于《分布式Java应用：基础与实践》一书](http://www.blogjava.net/BlueDavy/archive/2010/05/25/321796.html)
* [杭州程序员圆桌交流第二期视频](http://www.blogjava.net/BlueDavy/archive/2010/04/30/319794.html)
* [第一次杭州程序员交流会总结](http://www.blogjava.net/BlueDavy/archive/2010/03/22/316148.html)
* [杭州程序员圆桌交流第一期–并发编程PPT](http://www.blogjava.net/BlueDavy/archive/2010/03/19/315989.html)
* [GCLogViewer(tool to visualize gc log) V0.2 Release](http://www.blogjava.net/BlueDavy/archive/2009/12/03/304597.html)
* [Simple Scala actor Vs java Thread Vs Kilim Test](http://www.blogjava.net/BlueDavy/archive/2009/11/25/303662.html)
* [《构建高性能的大型分布式Java应用》目录&试读样章](http://www.blogjava.net/BlueDavy/archive/2009/11/06/301448.html)
* [动态跟踪Java代码的执行状况工具--BTrace](http://www.blogjava.net/BlueDavy/archive/2009/10/10/297661.html)
* [GC策略的调优](http://www.blogjava.net/BlueDavy/archive/2009/10/09/297562.html)
* [Hessian 3.2.0的两个bug](http://www.blogjava.net/BlueDavy/archive/2009/08/06/290003.html)  
Powered by: [BlogJava](http://www.blogjava.net/) Copyright © BlueDavy
### 公告

[![]()](http://www.douban.com/subject/3843896/) 
[![]()](http://china.osgiusers.org/)
[![]()](http://feed.feedsky.com/bluedavy "BlogJava-BlueDavy之技术Blog") [![feedsky]()](http://feed.feedsky.com/bluedavy)
[![抓虾]()](http://www.zhuaxia.com/add_channel.php?url=http://feed.feedsky.com/bluedavy)
[![google reader]()](http://fusion.google.com/add?feedurl=http://feed.feedsky.com/bluedavy)
[![鲜果]()](http://www.xianguo.com/subscribe.php?url=http://feed.feedsky.com/bluedavy)

### 导航

* [BlogJava](http://www.blogjava.net/)
* [首页](http://www.blogjava.net/BlueDavy/)
* [新随笔](http://www.blogjava.net/BlueDavy/admin/EditPosts.aspx?opt=1)
* [联系](http://www.blogjava.net/BlueDavy/contact.aspx?id=1)
* [聚合](http://www.blogjava.net/BlueDavy/rss)[![]()](http://www.blogjava.net/BlueDavy/rss)
* [管理](http://www.blogjava.net/BlueDavy/admin/EditPosts.aspx)
[<]( "转到上一个月")2009年10月[>]( "转到下一个月")日一二三四五六2728293012345678[9](http://www.blogjava.net/BlueDavy/archive/2009/10/09.html)[10](http://www.blogjava.net/BlueDavy/archive/2009/10/10.html)1112131415161718192021222324252627282930311234567

### 统计

* 随笔 - 294
* 文章 - 2
* 评论 - 2068
* 引用 - 1

### 随笔分类

* [@RIAWork(10)](http://www.blogjava.net/BlueDavy/category/10609.html) [(rss)](http://www.blogjava.net/BlueDavy/category/10609.html/rss "Subscribe to @RIAWork(10)")
* [Internet(20)](http://www.blogjava.net/BlueDavy/category/32839.html) [(rss)](http://www.blogjava.net/BlueDavy/category/32839.html/rss "Subscribe to Internet(20)")
* [Java(82)](http://www.blogjava.net/BlueDavy/category/1366.html) [(rss)](http://www.blogjava.net/BlueDavy/category/1366.html/rss "Subscribe to Java(82)")
* [Javascript(7)](http://www.blogjava.net/BlueDavy/category/8127.html) [(rss)](http://www.blogjava.net/BlueDavy/category/8127.html/rss "Subscribe to Javascript(7)")
* [OSGi、SOA、SCA(81)](http://www.blogjava.net/BlueDavy/category/18356.html) [(rss)](http://www.blogjava.net/BlueDavy/category/18356.html/rss "Subscribe to OSGi、SOA、SCA(81)")
* [Plugin Architecture(10)](http://www.blogjava.net/BlueDavy/category/1514.html) [(rss)](http://www.blogjava.net/BlueDavy/category/1514.html/rss "Subscribe to Plugin Architecture(10)")
* [Workflow(4)](http://www.blogjava.net/BlueDavy/category/1513.html) [(rss)](http://www.blogjava.net/BlueDavy/category/1513.html/rss "Subscribe to Workflow(4)")
* [业界随想(28)](http://www.blogjava.net/BlueDavy/category/11559.html) [(rss)](http://www.blogjava.net/BlueDavy/category/11559.html/rss "Subscribe to 业界随想(28)")
* [数据集成(8)](http://www.blogjava.net/BlueDavy/category/22527.html) [(rss)](http://www.blogjava.net/BlueDavy/category/22527.html/rss "Subscribe to 数据集成(8)")
* [系统设计(38)](http://www.blogjava.net/BlueDavy/category/1911.html) [(rss)](http://www.blogjava.net/BlueDavy/category/1911.html/rss "Subscribe to 系统设计(38)")
* [软件工程(22)](http://www.blogjava.net/BlueDavy/category/1367.html) [(rss)](http://www.blogjava.net/BlueDavy/category/1367.html/rss "Subscribe to 软件工程(22)")

### 随笔档案

* [2010年5月 (3)](http://www.blogjava.net/BlueDavy/archive/2010/05.html)
* [2010年4月 (4)](http://www.blogjava.net/BlueDavy/archive/2010/04.html)
* [2010年3月 (3)](http://www.blogjava.net/BlueDavy/archive/2010/03.html)
* [2010年2月 (1)](http://www.blogjava.net/BlueDavy/archive/2010/02.html)
* [2010年1月 (2)](http://www.blogjava.net/BlueDavy/archive/2010/01.html)
* [2009年12月 (1)](http://www.blogjava.net/BlueDavy/archive/2009/12.html)
* [2009年11月 (3)](http://www.blogjava.net/BlueDavy/archive/2009/11.html)
* [2009年10月 (2)](http://www.blogjava.net/BlueDavy/archive/2009/10.html)
* [2009年9月 (2)](http://www.blogjava.net/BlueDavy/archive/2009/09.html)
* [2009年8月 (2)](http://www.blogjava.net/BlueDavy/archive/2009/08.html)
* [2009年7月 (3)](http://www.blogjava.net/BlueDavy/archive/2009/07.html)
* [2009年6月 (1)](http://www.blogjava.net/BlueDavy/archive/2009/06.html)
* [2009年5月 (4)](http://www.blogjava.net/BlueDavy/archive/2009/05.html)
* [2009年4月 (5)](http://www.blogjava.net/BlueDavy/archive/2009/04.html)
* [2009年3月 (5)](http://www.blogjava.net/BlueDavy/archive/2009/03.html)
* [2009年1月 (1)](http://www.blogjava.net/BlueDavy/archive/2009/01.html)
* [2008年11月 (3)](http://www.blogjava.net/BlueDavy/archive/2008/11.html)
* [2008年10月 (1)](http://www.blogjava.net/BlueDavy/archive/2008/10.html)
* [2008年9月 (2)](http://www.blogjava.net/BlueDavy/archive/2008/09.html)
* [2008年8月 (1)](http://www.blogjava.net/BlueDavy/archive/2008/08.html)
* [2008年7月 (4)](http://www.blogjava.net/BlueDavy/archive/2008/07.html)
* [2008年6月 (4)](http://www.blogjava.net/BlueDavy/archive/2008/06.html)
* [2008年5月 (3)](http://www.blogjava.net/BlueDavy/archive/2008/05.html)
* [2008年4月 (1)](http://www.blogjava.net/BlueDavy/archive/2008/04.html)
* [2008年3月 (3)](http://www.blogjava.net/BlueDavy/archive/2008/03.html)
* [2008年2月 (1)](http://www.blogjava.net/BlueDavy/archive/2008/02.html)
* [2008年1月 (10)](http://www.blogjava.net/BlueDavy/archive/2008/01.html)
* [2007年12月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/12.html)
* [2007年11月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/11.html)
* [2007年10月 (6)](http://www.blogjava.net/BlueDavy/archive/2007/10.html)
* [2007年9月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/09.html)
* [2007年8月 (4)](http://www.blogjava.net/BlueDavy/archive/2007/08.html)
* [2007年7月 (5)](http://www.blogjava.net/BlueDavy/archive/2007/07.html)
* [2007年6月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/06.html)
* [2007年5月 (4)](http://www.blogjava.net/BlueDavy/archive/2007/05.html)
* [2007年4月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/04.html)
* [2007年3月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/03.html)
* [2007年2月 (2)](http://www.blogjava.net/BlueDavy/archive/2007/02.html)
* [2007年1月 (1)](http://www.blogjava.net/BlueDavy/archive/2007/01.html)
* [2006年12月 (6)](http://www.blogjava.net/BlueDavy/archive/2006/12.html)
* [2006年11月 (5)](http://www.blogjava.net/BlueDavy/archive/2006/11.html)
* [2006年10月 (8)](http://www.blogjava.net/BlueDavy/archive/2006/10.html)
* [2006年9月 (13)](http://www.blogjava.net/BlueDavy/archive/2006/09.html)
* [2006年8月 (15)](http://www.blogjava.net/BlueDavy/archive/2006/08.html)
* [2006年7月 (3)](http://www.blogjava.net/BlueDavy/archive/2006/07.html)
* [2006年6月 (7)](http://www.blogjava.net/BlueDavy/archive/2006/06.html)
* [2006年5月 (9)](http://www.blogjava.net/BlueDavy/archive/2006/05.html)
* [2006年4月 (12)](http://www.blogjava.net/BlueDavy/archive/2006/04.html)
* [2006年3月 (13)](http://www.blogjava.net/BlueDavy/archive/2006/03.html)
* [2006年2月 (9)](http://www.blogjava.net/BlueDavy/archive/2006/02.html)
* [2006年1月 (14)](http://www.blogjava.net/BlueDavy/archive/2006/01.html)
* [2005年12月 (11)](http://www.blogjava.net/BlueDavy/archive/2005/12.html)
* [2005年11月 (14)](http://www.blogjava.net/BlueDavy/archive/2005/11.html)
* [2005年10月 (9)](http://www.blogjava.net/BlueDavy/archive/2005/10.html)
* [2005年9月 (10)](http://www.blogjava.net/BlueDavy/archive/2005/09.html)
* [2005年8月 (5)](http://www.blogjava.net/BlueDavy/archive/2005/08.html)
* [2005年7月 (9)](http://www.blogjava.net/BlueDavy/archive/2005/07.html)
* [2005年6月 (9)](http://www.blogjava.net/BlueDavy/archive/2005/06.html)
* [2005年5月 (2)](http://www.blogjava.net/BlueDavy/archive/2005/05.html)
* [2005年2月 (1)](http://www.blogjava.net/BlueDavy/archive/2005/02.html)

### 文章档案

* [2005年5月 (2)](http://www.blogjava.net/BlueDavy/archives/2005/05.html)

### Blogger's

* [DBANotes](http://www.dbanotes.net/)
* 大名鼎鼎了，勿需多说
* [ESBZone](http://www.esbzone.net/) [(rss)](http://www.esbzone.net/feed "Subscribe to ESBZone")
* SOA实战者，强烈推荐
* [kenwu's blog[推荐]](http://kenwublog.com/) [(rss)](http://feed.kenwublog.com/ "Subscribe to kenwu's blog[推荐]")
* Java底层和JVM的很多文章，强烈推荐。
* [梦想风暴](http://dreamhead.blogbus.com/)
* [西湖边的穷秀才](http://www.blogjava.net/cenwenchu/)

### 搜索

*
*  

### 最新评论 [![]()](http://www.blogjava.net/BlueDavy/CommentsRSS.aspx)

* [1. re: 关于《分布式Java应用：基础与实践》一书](http://www.blogjava.net/BlueDavy/archive/2013/07/19/321796.html#401750)
* 博主 大神啊 这个博客很久没有更新了
* --1836567962@qq.com
* [2. re: 网站架构相关PPT、文章整理（更新于2009-7-15）](http://www.blogjava.net/BlueDavy/archive/2013/07/11/267970.html#401444)
* 这个文章很有用，收藏
* --度哥网
* [3. re: OSGi Opendoc & Book](http://www.blogjava.net/BlueDavy/archive/2013/05/28/267968.html#399877)
* 评论内容较长,点击标题查看
* --stono
* [4. re: OSGi Opendoc & Book](http://www.blogjava.net/BlueDavy/archive/2013/05/28/267968.html#399865)
* 我再看《OSGi》原理与最佳实践，可是源码没有办法下载呀；
有没有什么办法呢？
* --stono
* [5. re: 关于《分布式Java应用：基础与实践》一书](http://www.blogjava.net/BlueDavy/archive/2013/05/27/321796.html#399801)
* 在这篇blog中放置了我收集的一些网站架构相关的PPT和文章
* --色都

### 阅读排行榜

* [1. 大型网站架构演变和知识体系(57638)](http://www.blogjava.net/BlueDavy/archive/2008/09/03/226749.html)
* [2. 网站架构相关PPT、文章整理（更新于2009-7-15）(38732)](http://www.blogjava.net/BlueDavy/archive/2009/04/28/267970.html)
* [3. Hibernate实践(31780)](http://www.blogjava.net/BlueDavy/archive/2006/03/27/37582.html)
* [4. 系统设计说明书(架构、概要、详细)目录结构(24258)](http://www.blogjava.net/BlueDavy/archive/2005/06/13/6037.html)
* [5. 发布《OSGi实战》正式版(17725)](http://www.blogjava.net/BlueDavy/archive/2006/08/25/65741.html)

### 评论排行榜

* [1. 大型网站架构演变和知识体系(96)](http://www.blogjava.net/BlueDavy/archive/2008/09/03/226749.html)
* [2. 漫谈CMS(88)](http://www.blogjava.net/BlueDavy/archive/2005/09/07/12340.html)
* [3. 网站架构相关PPT、文章整理（更新于2009-7-15）(66)](http://www.blogjava.net/BlueDavy/archive/2009/04/28/267970.html)
* [4. 《OSGi进阶》预览版发布(59)](http://www.blogjava.net/BlueDavy/archive/2007/09/29/149631.html)
* [5. 发布《OSGi实战》正式版(54)](http://www.blogjava.net/BlueDavy/archive/2006/08/25/65741.html)
