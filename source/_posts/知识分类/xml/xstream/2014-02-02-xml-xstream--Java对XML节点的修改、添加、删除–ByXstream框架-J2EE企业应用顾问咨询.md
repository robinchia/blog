---
layout: post
title: "Java对XML节点的修改、添加、删除 –By Xstream框架 - J2EE企业应用 顾问 咨询"
categories: xml
tags: 
 - xml
 - xstream
--- 

# Java对XML节点的修改、添加、删除 –By Xstream框架 - J2EE企业应用 顾问 咨询 Java传教士 -H.E.'s Blog

# [![订阅]()](http://www.javabloger.com/index.php/feed/) [H.E.'s Blog](http://www.javabloger.com/)

 用最简洁的页面描述企业应用与Java艺术!     [首页](http://www.javabloger.com/) [关于作者](http://www.javabloger.com/about_njthnet.html) [项目合作](http://www.javabloger.com/work_together.html) [本站文章](http://www.javabloger.com/article) [开源项目](http://www.javabloger.com/opensource_project.html)  

## Java对XML节点的修改、添加、删除 –By Xstream框架

12 三月, 2010 (10:36) | [代码](http://www.javabloger.com/article/category/code "查看 代码 的全部文章") |  [繁体](http://translate.google.com/translate?langpair=zh-CN%7Czh-TW&u=http://www.javabloger.com/article/java-xml-node-add-delete-update.html) [English](http://translate.google.com/translate?langpair=zh-CN%7Cen&u=http://www.javabloger.com/article/java-xml-node-add-delete-update.html)    ![]()[**DeliciOus**](http://delicious.com/save)    [0](http://www.google.com/buzz/post "在 Google Buzz 上分享本文")  【[分享到**新浪微博**](http://v.t.sina.com.cn/share/share.php?title=Java%E5%AF%B9XML%E8%8A%82%E7%82%B9%E7%9A%84%E4%BF%AE%E6%94%B9%E3%80%81%E6%B7%BB%E5%8A%A0%E3%80%81%E5%88%A0%E9%99%A4%20%E2%80%93By%20Xstream%E6%A1%86%E6%9E%B6&url=http://www.javabloger.com/article/java-xml-node-add-delete-update.html?source=rss&conten=%E5%86%8D%E6%AC%A1%E6%84%9F%E8%B0%A2%E4%BD%A0%E7%9A%84%E5%88%86%E4%BA%AB)】
作者: H.E. | 您可以转载, 但必须以超链接形式标明文章原始出处和作者信息及版权声明
网址: [http://www.javabloger.com/article/java-xml-node-add-delete-update.html]()
**豆瓣读书** 向你推荐有关 [代码](http://book.douban.com/subject_search?search_text=%E4%BB%A3%E7%A0%81&cat=1001)、 类别的图书。

在J2EE、Java项目中对xml操作是一项非常常见的事情，在我认识了XStream以后，才彻底明白XML模型对象的概念，使用XStream让我XML的设计不由自主更符合OO的风格。另外，除了在设计上得到的体验，还在实际操作中得到了便捷的体验。

简单来说，我在IBM的Java开发园地上看过一些操作XML经典的文章被广为流传，但是无论是采用DOM4J还是JDOM对XML文件中的节点或者整个文件的进行 修改、添加、删除 都没有XStream 简单、方便。

下面来看一下 XStream 官方网站上的例子，网站地址：     http://xstream.codehaus.org/tutorial.html。

官方网站上的例子只是提供了一个例子，并没有说明如何对 XML节点进行修改、添加、删除，XStream 官方网站上只是给出了一个从对象到XML，从XML到对象的例子。

Javabloger的作者H.E.通过实践发现采用 XStream可以很简单的对XML节点进行 修改、添加、删除，比传统的XML框架简单很多倍，用过一次的人都不会忘记，**因为实在是太方便了**。
 
**修改XML节点代码示例如下：**
   
    public void upDateMySQLRecentHost(String  HostID)  throws  Exception {
        List <RecentHost> list=null;
        try {
                list=getMySQLRecentHost();   **// 1. 先通过 XStream读取XML文件，变成List集合里面包含节点对象。**

                for (int i=0;i<list.size();i++){      **  //  2. 遍历整个 List 对象集合**
                    if (list.get(i).getId().equals(HostID )){        **// 3. 比较 需要进行修改的节点 ID**
                        list.remove(i);                                     **//  4. 移除 指定的节点  **
                        list.add(recentHost);                          **  //  5. 添加 定义的新节点**
                    }
                }                                                                **   //  完成操作，对节点进行 匹配的删除和添加 这个就修改。**
               
           *    //  以下代码H.E.就不一一介绍了，大致意思就是 将修改过的List 对象集合，重新写入XML中。*
                xstream.alias("RecentHost",  RecentHost.class);         
                String xml="<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n"+xstream.toXML(list);
                 System.out.println ( xml );
                FileWriter resultFile = new FileWriter(recentLoginHostCaseXMLFile);
                PrintWriter myFile = new PrintWriter(resultFile);
                myFile.println(xml);
                resultFile.close();
            }
        catch ( Exception e) {
            logger.error(e);
        }
    }

**添加XML节点代码示例如下：**

    public void AddMySQLRecentHost(RecentHost RecentHostInfo)  throws  Exception {
        List <RecentHost> list=null;
        try {
                list=getMySQLRecentHost(); **  // 1. 先通过 XStream读取XML文件，变成List集合里面包含节点对象。**
                list.add( RecentHostInfo  );    **//   2. 添加到  List集合里面。**
             
                xstream.alias("RecentHost",  RecentHost.class);  **// 3.进行XML对象映射**
                String xml="<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n"+xstream.toXML(list);
                 
             *   //  以下代码H.E.就不一一介绍了，大致意思就是 将添加过的List 对象集合，重新写入XML中。*
                FileWriter resultFile = new FileWriter(recentLoginHostCaseXMLFile);
                PrintWriter myFile = new PrintWriter(resultFile);
                myFile.println(xml);
                resultFile.close();      
            }
        catch ( Exception e) {
            logger.error(e);
        }
    }

**删除XML节点代码示例如下：**

    public void deleteMySQLRecentHost(String id)  throws  Exception {
        List <RecentHost> list=null;
        try {
                list=getMySQLRecentHost();   **// 1. 先通过 XStream读取XML文件，变成List集合里面包含节点对象。**

                for (int i=0;i<list.size();i++){      **  //  2. 遍历整个 List 对象集合**
                    if (list.get(i).getId().equals(HostID )){       **// 3. 比较 需要进行修改的节点 ID**
                        list.remove(i);                                    **  //  4. 移除 指定的节点  **
                    }                                                                                                                     
                }                                                             **      //  完成删除操作。**                            
                                                                                                                                       
    *            //  以下代码H.E.就不一一介绍了，大致意思就是 将修改过的List 对象集合，重新写入XML中。*
                xstream.alias("RecentHost",  RecentHost.class);                                     
                String xml="<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n"+xstream.toXML(list);
                 System.out.println ( xml );                                                   
                FileWriter resultFile = new FileWriter(recentLoginHostCaseXMLFile);
                PrintWriter myFile = new PrintWriter(resultFile);                                      
                myFile.println(xml);                                                                                        
                resultFile.close();                                                                                           
            }                                                                                                                              
        catch ( Exception e) {                                                                                              
            logger.error(e);                                                                                                    
        }                                                                                                                                  
    }

–end–

 

**豆瓣读书**  向你推荐有关[代码](http://book.douban.com/subject_search?search_text=%E4%BB%A3%E7%A0%81&cat=1001)、 类别的图书。
[![Creative Commons License]()
](http://creativecommons.org/licenses/by/2.5/cn/) 本文由[J2ee企业顾问](http://www.javabloger.com/)-黄毅创作,并已采用[创作共用署名2.5中国大陆版许可证](http://creativecommons.org/licenses/by/2.5/cn/ "Creative Commons Attribution 2.5 China Mainland License")授权。

标签: [XML节点](http://www.javabloger.com/article/tag/xml%e8%8a%82%e7%82%b9), [Xstream](http://www.javabloger.com/article/tag/xstream), [修改、添加、删除](http://www.javabloger.com/article/tag/%e4%bf%ae%e6%94%b9%e3%80%81%e6%b7%bb%e5%8a%a0%e3%80%81%e5%88%a0%e9%99%a4)

« [大型JavaMail网站Mailinator架构(Linux+Tomcat+Java)](http://www.javabloger.com/article/javamail-web-site-mailinator.html)

[大型网站的web服务器](http://www.javabloger.com/article/top-20-web-stie-web-server.html) »

## 评论

评论也是有版权的！

姓名:

E-mail:

网址:

验证码:  8138

正文 (必填, 最长字数:1200):
## 我的废话

印象中google、微软中的工程师和普通程序员最大的区别并不是他们的薪水与待遇，而是他们用80%时间去思考，20%的时间去修改代码，另则是用80%的时间去修改代码，**只**用20%的时间去思考问题。

## 关注我@

   [![]()](http://t.sina.com.cn/javabloger)
   [![]()](http://feed.feedsky.com/javaBloger)
   [![]()](http://www.douban.com/people/njthnet)
   [![]()](http://www.google.com/profiles/njthnet)
   **IM提醒**
   [![]()](http://inezha.com/add2?url=http://feed.feedsky.com/javaBloger)
## 个人介绍

**H.E.@Info:**
J2EE Architect/**J**Evangelist
Life@NanJing
Hobby@Photography
Email@njthnet(at)gmail.com
[更多...](http://www.javabloger.com/about_njthnet.html)

## 加入小圈子
## 小道消息

[本站Service API 测试ing](http://www.javabloger.com/webservice)
[第三期**“迷你书” JMS与消息中间件 专题**(PDF)](http://javabloger-mini-books.googlecode.com/files/Java-JMS-Javabloger-mini-book.pdf)
[第二期**“迷你书” 架构! 专题**(PDF)](http://javabloger-mini-books.googlecode.com/files/JavaBloger-mini_book-2010-4.pdf)
[MongoDB**入门介绍**(PDF)](http://javabloger-mini-books.googlecode.com/files/MongoDB_1_en.pdf)

## 最被关注

* [Google Analytics(谷歌分析) 架构与原理](http://www.javabloger.com/article/google-analytics-architecture.html)
* [HBase 技术专题](http://www.javabloger.com/article/category/hbase " Spring3 REST实现html伪静态分页效果")
* [J2ee企业顾问的职责](http://www.javabloger.com/article/j2ee-adviser.html)
* [J2ee核心模式–微架构](http://www.javabloger.com/article/core-j2ee-patterns-micr_architecture.html)
* [Java+PHP=混血新宠儿](http://www.javabloger.com/article/java_and_php_mixed.html)
* [Java操作MongoDB NoSQL数据库](http://www.javabloger.com/article/mongodb-java.html)
* [Memcached集群/分布式的单点故障](http://www.javabloger.com/article/memcached-cluster-error-msag.html)
* [MongoDB 集群](http://www.javabloger.com/article/mongodb-cluster.html)
* [SEO与Java Web网站开发](http://www.javabloger.com/article/seo-java-j2ee-web-development.html)
* [Spring3 REST MVC框架,提速你的Web开发](http://www.javabloger.com/article/spring3-rest-mvc-example.html "Spring3 REST MVC框架,提速你的Web开发")
* [Spring3 REST实现html伪静态分页](http://www.javabloger.com/article/spring3-rest-java-on-google.html)
* [什么使用J2ee开源框架](http://www.javabloger.com/article/why_use_j2ee_framework.html)
* [哪些大型网站采用J2EE架构](http://www.javabloger.com/article/what-large-web-site-using-j2ee-architecture.html)
* [大型J2EE系统架构](http://www.javabloger.com/article/j2ee-clustering-architecture.html)
* [大型网站百万级高并发“云测试”](http://www.javabloger.com/article/myspace-cloudtest.html)
* [大型网站的web服务器](http://www.javabloger.com/article/top-20-web-stie-web-server.html)
* [敏捷的要素](http://www.javabloger.com/article/implement-agile.html)
* [理想的敏捷工作环境与氛围](http://www.javabloger.com/article/great-agile-workspace.html)
* [百万级 J2EE Push Mail 项目后记2](http://www.javabloger.com/article/j2ee-push-mail-of-team.html "百万级 J2EE Push Mail 项目后记2")
* [说说每日项目例会的必要性](http://www.javabloger.com/article/talk-about-project-dailymeeting.html)
* [谈谈我知道的微软、SUN公司 测试工作](http://www.javabloger.com/article/microsoft_and_sun_testing_process.html)
## 最近文章

* [Apache Thrift入门2-代码实现例子](http://www.javabloger.com/article/thrift-java-code-example.html "Apache Thrift入门2-代码实现例子")
* [Apache Thrift入门1-架构&介绍](http://www.javabloger.com/article/apache-thrift-architecture.html "Apache Thrift入门1-架构&介绍")
* [2011 新年里](http://www.javabloger.com/article/2011-pic-1.html "2011 新年里")
* [HBase入门7 -安全&权限](http://www.javabloger.com/article/hbase-secure-privilege.html "HBase入门7 -安全&权限")
* [Hbase入门6 -白话MySQL(RDBMS)与HBase之间](http://www.javabloger.com/article/hbase-mysql-rdbms.html "Hbase入门6 -白话MySQL(RDBMS)与HBase之间")
* [Lily-建立在HBase上的分布式搜索](http://www.javabloger.com/article/lily-hbase-solr-lucene-zookeeper.html "Lily-建立在HBase上的分布式搜索")
* [基于Hbase存储的分布式消息(IM)系统-JABase](http://www.javabloger.com/article/hbase-im-jabase-xmpp.html "基于Hbase存储的分布式消息(IM)系统-JABase")
* [MySQL向Hive/HBase的迁移工具](http://www.javabloger.com/article/hadoop-hive-mysql-sqoop.html "MySQL向Hive/HBase的迁移工具")
* [JEE技术在移动互联网中的应用](http://www.javabloger.com/article/jee-and-mobile-internet.html "JEE技术在移动互联网中的应用")
* [新年，新功能](http://www.javabloger.com/article/javabloger-site-features.html "新年，新功能")
* [谷歌分析数据](http://www.javabloger.com/article/javabloger-google-analytics-2010.html "谷歌分析数据")
* [谁引用了本站的内容?](http://www.javabloger.com/article/egosurf.html "谁引用了本站的内容?")
* [GoodBye,2010](http://www.javabloger.com/article/bye-2010-hello-hty.html "GoodBye,2010")
* [Hadoop Hama项目–BSP模型的实现](http://www.javabloger.com/article/apache-hadoop-hama-bsp.html "Hadoop Hama项目–BSP模型的实现")
* [停止你的抱怨！](http://www.javabloger.com/article/stop-complaining.html "停止你的抱怨！")

## 摄影/生活–LOMO

### ·[北京`咖啡店`小酒馆`pizza](http://www.javabloger.com/article/beijing_cafe_bistro_pizza-factory.html)

### ·[2009与我的照片精选](http://www.javabloger.com/2009_selected_photo)

### ·[初学摄影时获奖的照片](http://www.javabloger.com/post_by_2008)

### ·[鼓浪屿游记](http://www.javabloger.com/other_pics_1)

### ·[有趣的照片](http://www.javabloger.com/other_pics_2)

### ·[收藏岁月的时光](http://www.javabloger.com/other_pics_3)

### ·[关于H.E.的黑白照片](http://www.javabloger.com/black_and_white_photograph)

### ·[花花 草草,多么环保](http://www.javabloger.com/mass_flower_photo)

### ·[google的办公环境【转】](http://www.javabloger.com/google_office_environment)

[更多...](http://www.javabloger.com/album)
## 本站文章分类

* [Android](http://www.javabloger.com/article/category/android "查看 Android 下的所有文章")
* [ChukWa](http://www.javabloger.com/article/category/chukwa "查看 ChukWa 下的所有文章")
* [Download](http://www.javabloger.com/article/category/download "查看 Download 下的所有文章")
* [GlassFish](http://www.javabloger.com/article/category/glassfish-j2ee-server "GlassFish应用服务器")
* [google-analytics](http://www.javabloger.com/article/category/google-analytics "查看 google-analytics 下的所有文章")
* [Hadoop](http://www.javabloger.com/article/category/hadoop "查看 Hadoop 下的所有文章")
* [hama](http://www.javabloger.com/article/category/hama "查看 hama 下的所有文章")
* [HBase](http://www.javabloger.com/article/category/hbase "查看 HBase 下的所有文章")
* [Hive](http://www.javabloger.com/article/category/hive "查看 Hive 下的所有文章")
* [J2ee企业顾问](http://www.javabloger.com/article/category/j2ee-adviser "软件开发的职场经验")
* [J2EE服务器](http://www.javabloger.com/article/category/j2ee-server "查看 J2EE服务器 下的所有文章")
* [J2EE框架](http://www.javabloger.com/article/category/j2ee-framework "查看 J2EE框架 下的所有文章")
* [JMS](http://www.javabloger.com/article/category/jms "查看 JMS 下的所有文章")
* [Life](http://www.javabloger.com/article/category/life "查看 Life 下的所有文章")
* [Linux/Unix](http://www.javabloger.com/article/category/linux "查看 Linux/Unix 下的所有文章")
* [lucene](http://www.javabloger.com/article/category/lucene "查看 lucene 下的所有文章")
* [mac](http://www.javabloger.com/article/category/mac "查看 mac 下的所有文章")
* [MapReduce](http://www.javabloger.com/article/category/mapreduce "查看 MapReduce 下的所有文章")
* [MiddleWare](http://www.javabloger.com/article/category/middleware "查看 MiddleWare 下的所有文章")
* [MongoDB](http://www.javabloger.com/article/category/mongodb "查看 MongoDB 下的所有文章")
* [NoSQL](http://www.javabloger.com/article/category/nosql "查看 NoSQL 下的所有文章")
* [OO](http://www.javabloger.com/article/category/oo "查看 OO 下的所有文章")
* [openmq](http://www.javabloger.com/article/category/openmq "查看 openmq 下的所有文章")
* [OpenSource](http://www.javabloger.com/article/category/opensource "查看 OpenSource 下的所有文章")
* [php](http://www.javabloger.com/article/category/php "查看 php 下的所有文章")
* [Pig](http://www.javabloger.com/article/category/pig "查看 Pig 下的所有文章")
* [SEO](http://www.javabloger.com/article/category/seo "搜索引擎优化")
* [spring3](http://www.javabloger.com/article/category/spring3 "查看 spring3 下的所有文章")
* [Testing](http://www.javabloger.com/article/category/testing "查看 Testing 下的所有文章")
* [Thrift](http://www.javabloger.com/article/category/thrift "查看 Thrift 下的所有文章")
* [web](http://www.javabloger.com/article/category/web-dev "查看 web 下的所有文章")
* [zookeeper](http://www.javabloger.com/article/category/zookeeper "查看 zookeeper 下的所有文章")
* [云计算](http://www.javabloger.com/article/category/cloud "查看 云计算 下的所有文章")
* [代码](http://www.javabloger.com/article/category/code "查看 代码 下的所有文章")
* [分布式](http://www.javabloger.com/article/category/distributed "查看 分布式 下的所有文章")
* [存储](http://www.javabloger.com/article/category/storage "查看 存储 下的所有文章")
* [性能](http://www.javabloger.com/article/category/performance "查看 性能 下的所有文章")
* [招聘](http://www.javabloger.com/article/category/retain "查看 招聘 下的所有文章")
* [推荐引擎](http://www.javabloger.com/article/category/recommendation "查看 推荐引擎 下的所有文章")
* [敏捷](http://www.javabloger.com/article/category/agile "查看 敏捷 下的所有文章")
* [数据库](http://www.javabloger.com/article/category/database "查看 数据库 下的所有文章")
* [杂类](http://www.javabloger.com/article/category/other "查看 杂类 下的所有文章")
* [架构设计](http://www.javabloger.com/article/category/architecture "查看 架构设计 下的所有文章")
* [案例与故事](http://www.javabloger.com/article/category/success_story "工作与实施顾问中的记实录，讲述身边的事儿，讲述与你共鸣的那点事儿。")
* [职场](http://www.javabloger.com/article/category/occupation "查看 职场 下的所有文章")
* [迷你书](http://www.javabloger.com/article/category/mini-book "查看 迷你书 下的所有文章")

## 文章索引模板

* [2011年二月](http://www.javabloger.com/article/2011/02 "2011年二月")
* [2011年一月](http://www.javabloger.com/article/2011/01 "2011年一月")
* [2010年十二月](http://www.javabloger.com/article/2010/12 "2010年十二月")
* [2010年十一月](http://www.javabloger.com/article/2010/11 "2010年十一月")
* [2010年十月](http://www.javabloger.com/article/2010/10 "2010年十月")
* [2010年九月](http://www.javabloger.com/article/2010/09 "2010年九月")
* [2010年八月](http://www.javabloger.com/article/2010/08 "2010年八月")
* [2010年七月](http://www.javabloger.com/article/2010/07 "2010年七月")
* [2010年六月](http://www.javabloger.com/article/2010/06 "2010年六月")
* [2010年五月](http://www.javabloger.com/article/2010/05 "2010年五月")
* [2010年四月](http://www.javabloger.com/article/2010/04 "2010年四月")
* [2010年三月](http://www.javabloger.com/article/2010/03 "2010年三月")
* [2010年二月](http://www.javabloger.com/article/2010/02 "2010年二月")
* [2010年一月](http://www.javabloger.com/article/2010/01 "2010年一月")
* [2008年六月](http://www.javabloger.com/article/2008/06 "2008年六月")
* [2008年二月](http://www.javabloger.com/article/2008/02 "2008年二月")
* [2007年六月](http://www.javabloger.com/article/2007/06 "2007年六月")
## 本站信息

![]()
网志文章均为原创
本站版权[创作共用](http://www.javabloger.com/article/creative-commons.html)
[3A网络](http://www.cnaaa.com/)提供本站网络资源
**Started**@2009/01/15

 © 2011 H.E. 's Blog | [Entries (RSS)](http://www.javabloger.com/index.php/feed=rss2) | [Comments (RSS)](http://www.javabloger.com/index.php/feed=comments-rss2)   [本站地图](http://www.javabloger.com/sitemap.xml)
苏ICP备10012508号
