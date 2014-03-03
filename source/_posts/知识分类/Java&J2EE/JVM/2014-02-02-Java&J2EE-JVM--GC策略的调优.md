---
layout: post
title: "GC策略的调优"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# GC策略的调优

# [BlueDavy之技术Blog](http://www.blogjava.net/BlueDavy/)

理论不懂就实践，实践不会就学理论！

## [GC策略的调优](http://www.blogjava.net/BlueDavy/archive/2009/10/09/297562.html)

摘自《构建高性能的大型分布式Java应用》第六章，感兴趣的同学们可以看看。

GC策略在G1还没成熟的情况下，目前主要有串行、并行和并发三种，对于大内存的应用而言，串行的性能太低，因此使用到的主要是并行和并发两种，具体这两种GC的策略在深入JVM章节中已讲解， 并行和并发GC的策略通过-XX:+UseParallelGC和-XX:+UseConcMarkSweepGC来指定，还有一些细节的配置参数用来配置策略的执行方式，例如：-XX:ParallelGCThreads、-XX:CMSInitiatingOccupancyFraction等，新生代对象回收只可选择并行，在此就举例来看看两种GC策略在Full GC时的具体表现状况。

测试GC策略状况的代码如下：

****
**public class GCPolicyDemo {
 
    /**
     * @param args
     */
    public static void main(String[] args) throws Exception{
       System.out.println("ready to start");
       Thread.sleep(10000);
       List<GCPolicyDataObject> cacheObjects=new ArrayList<GCPolicyDataObject>();
       for (int i = 0; i < 2048; i++) {
           cacheObjects.add(new GCPolicyDataObject(100));
       }
       System.gc();
       Thread.sleep(1000);
       for (int i = 0; i < 10; i++) {
           System.out.println("Round: "+(i+1));
           for (int j = 0; j < 5; j++) {
              System.out.println("put 64M objects");
              List<GCPolicyDataObject> tmpObjects=new ArrayList<GCPolicyDataObject>();
              for (int m = 0; m < 1024; m++) {
                  tmpObjects.add(new GCPolicyDataObject(64));
              }
              tmpObjects=null;
           }
       }
       cacheObjects.size();
       cacheObjects=null;
    }
 
}
 
class GCPolicyDataObject{
   
    byte[] bytes=null;
   
    GCPolicyRefObject object=null;
   
    public GCPolicyDataObject(int factor){
       bytes=new byte[factor*1024];
       object=new GCPolicyRefObject();
    }
   
}
 
class GCPolicyRefObject{
   
    GCPolicyRefChildObject object;
   
    public GCPolicyRefObject(){
       object=new GCPolicyRefChildObject();
    }
   
}
 
class GCPolicyRefChildObject{
   
    public GCPolicyRefChildObject(){
       ;
    }
   
}**

 

以-Xms680M -Xmx680M -Xmn80M -XX:+UseConcMarkSweepGC -XX:+PrintGCApplicationStoppedTime -XX:+UseCMSCompactAtFullCollection -XX:+UseParNewGC -XX:CMSMaxAbortablePrecleanTime=5参数执行以上代码，通过jstat观察到的GC状况如下：

共触发39次minor GC，耗时为1.197秒，共触发21次Full GC，耗时为0.136秒，GC总耗时为1.333秒。

GC动作造成应用暂停的时间为：1.74秒。

以-Xms680M -Xmx680M -Xmn80M -XX:+PrintGCApplicationStoppedTime –XX:+UseParallelGC参数执行以上代码，通过jstat观察到的GC状况如下：

共触发119次minor GC，耗时为2.774秒，共触发8次Full GC，耗时为0.243秒，GC总耗时为3.016秒。

GC动作造成应用暂停的时间为：3.11秒。

从上面的结果来看，由于CMS GC多数动作是和应用并发做的，采用CMS GC确实可以减小GC动作给应用造成的暂停，但也正因为是并发进行的，因此CMS GC需要耗费更多的CPU，因此对于CPU密集型应用而言，CMS不一定是好的选择。

在采用CMS GC的情况下，尤其要注意的是concurrent mode failure的现象，这可以通过-XX:+PrintGCDetails来观察，当出现concurrent mode failure的现象时，就意味着此时JVM将继续采用Stop-The-World的方式来进行Full GC，这种情况下，采用CMS就没什么意义了，造成concurrent mode failure的原因主要是当minor GC进行时，旧生代所剩下的空间小于Eden区域+From区域的空间，要避免这种现象，可以采用以下三种方法：

l  调低触发CMS GC执行的阀值

CMS GC触发主要由CMSInitiatingOccupancyFraction值决定，默认情况是当旧生代已用空间为68%时，即触发CMS GC。

在出现concurrent mode failure的情况下，可考虑调小这个值，提前CMS GC的触发，以保证旧生代有足够的空间。

l  扩大旧生代空间

调小新生代占用的空间或增大整个JVM Heap的空间可扩大旧生代空间，这对于避免concurrent mode failure现象可以提供很大的帮助。

l  调小CMSMaxAbortablePrecleanTime的值
CMS GC需要经过较多步骤才能完成一次GC的动作，在minor GC较为频繁的情况下，很有可能造成CMS GC尚未完成，从而造成concurrent mode failure，这种情况下，减少minor GC触发的频率是一种方法，另外一种方法则是加快CMS GC执行时间，在CMS的整个步骤中，JDK 5.0+、6.0+的有些版本在CMS-concurrent-abortable-preclean-start和CMS-concurrent-abortable-preclean这两步间有可能会耗费很长的时间，导致可回收的旧生代的对象很长时间后才被回收，这是Sun JDK CMS GC的一个bug[[1]]()，如通过PrintGCDetails观察到这两步之间耗费了较长的时间，可以通过-XX: CMSMaxAbortablePrecleanTime设置较小的值，以保证CMS GC尽快完成对象的回收，避免concurrent mode failure的现象。

[[1]]() 详细bug信息请见：http://www.nabble.com/CMS-GC-tuning-under-JVM-5.0-td16759819.html

posted on 2009-10-09 15:57 [BlueDavy](http://www.blogjava.net/BlueDavy/) 阅读(9126) [评论(5)]()  [编辑](http://www.blogjava.net/BlueDavy/admin/EditPosts.aspx?postid=297562)  [收藏](http://www.blogjava.net/BlueDavy/AddToFavorite.aspx?id=297562) 所属分类: [Java](http://www.blogjava.net/BlueDavy/category/1366.html)

 ![]()

[]() []()

[
### 评论

]()

### []()[#]( "permalink: re: GC策略的调优") []()re: GC策略的调优  2009-10-09 16:36  [agraviton]()

bluedavy 又出手了，出手就不凡。  [回复]()  [更多评论](http://www.blogjava.net/comment?author=agraviton "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: GC策略的调优") []()re: GC策略的调优  2009-10-09 18:06  [glf]()

书什么时候出啊  [回复]()  [更多评论](http://www.blogjava.net/comment?author=glf "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: GC策略的调优") []()re: GC策略的调优  2009-10-09 20:10  [BlueDavy](http://www.blogjava.net/BlueDavy/)

@glf
这书估计要到明年三月左右才能上市了。  [回复]()  [更多评论](http://www.blogjava.net/comment?author=BlueDavy "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: GC策略的调优") []()re: GC策略的调优  2009-10-10 13:54  [hucq]()

期待实体书  [回复]()  [更多评论](http://www.blogjava.net/comment?author=hucq "查看该作者发表过的评论") []()  []()

### [#]( "permalink: re: GC策略的调优") []()re: GC策略的调优[]()  2009-12-21 21:16  [study]()

太期待了！   [回复]()  [更多评论](http://www.blogjava.net/comment?author=study "查看该作者发表过的评论") []()  []()
[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [推荐购买云服务器（15%返利+最高千元奖金）](http://www.aliyun.com/cps/channel?channel_id=1306&user=0&lv=1)   [博客园](http://www.cnblogs.com/)  [博问](http://q.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![](http://www.blogjava.net/Modules/CaptchaImage/JpegImage.aspx?cacheid=20130727130705) 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/BlueDavy/archive/2009/10/09/297562.html&SourceURL=/BlueDavy/archive/2009/10/09/297562.html)       [使用Ctrl+Enter键可以直接提交]      网站导航:

[博客园](http://www.cnblogs.com/ "程序员的网上家园")   [IT新闻](http://news.cnblogs.com/)   [知识库](http://kb.cnblogs.com/)   [C++博客](http://www.cppblog.com/)   [程序员招聘](http://job.cnblogs.com/)   [管理](http://www.blogjava.net/BlueDavy/archive/2009/10/09/297562.html?opt=admin) 相关文章:

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
