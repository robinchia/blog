---
layout: post
title: "Lucene学习总结之二：Lucene的总体架构"
categories: lucene
tags: 
 - lucene
--- 

# Lucene学习总结之二：Lucene的总体架构

[]()

# [觉先](http://www.cnblogs.com/forfuture1978/)

  [博客园](http://www.cnblogs.com/) :: [首页](http://www.cnblogs.com/forfuture1978/) :: [博问](http://q.cnblogs.com/) :: [闪存](http://home.cnblogs.com/ing/) :: [新随笔](http://www.cnblogs.com/forfuture1978/admin/EditPosts.aspx?opt=1) :: [联系](http://space.cnblogs.com/msg/send/%e8%a7%89%e5%85%88) :: [订阅](http://www.cnblogs.com/forfuture1978/rss) [![订阅]()](http://www.cnblogs.com/forfuture1978/rss) :: [管理](http://www.cnblogs.com/forfuture1978/admin/EditPosts.aspx) :: ![]()   130 随笔 :: 0 文章 :: 544 评论 :: 0 引用
[<]()2009年12月[>]()日一二三四五六293012345678910111213[14](http://www.cnblogs.com/forfuture1978/archive/2009/12/14.html)151617[18](http://www.cnblogs.com/forfuture1978/archive/2009/12/18.html)1920[21](http://www.cnblogs.com/forfuture1978/archive/2009/12/21.html)22232425262728293031123456789

### 公告

昵称：[觉先](http://home.cnblogs.com/u/forfuture1978/)
园龄：[3年7个月](http://home.cnblogs.com/u/forfuture1978/ "入园时间：2009-12-10")
荣誉：[推荐博客](http://www.cnblogs.com/expert/)
粉丝：[560](http://home.cnblogs.com/u/forfuture1978/followers/)
关注：[3](http://home.cnblogs.com/u/forfuture1978/followees/)

[+加关注]()

### 搜索

 

 

### 常用链接

* [我的随笔](http://www.cnblogs.com/forfuture1978/MyPosts.html)
* [我的评论](http://www.cnblogs.com/forfuture1978/MyComments.html)
* [我的参与](http://www.cnblogs.com/forfuture1978/OtherPosts.html "我发表过评论的随笔")
* [最新评论](http://www.cnblogs.com/forfuture1978/RecentComments.html)
* [我的标签](http://www.cnblogs.com/forfuture1978/tag/)

### 随笔分类

* [Hadoop原理与代码分析(7)](http://www.cnblogs.com/forfuture1978/category/300670.html)
* [IT外企那点儿事(12)](http://www.cnblogs.com/forfuture1978/category/300669.html)
* [Java(2)](http://www.cnblogs.com/forfuture1978/category/345798.html)
* [Linux(14)](http://www.cnblogs.com/forfuture1978/category/345797.html)
* [Lucene原理与代码分析(38)](http://www.cnblogs.com/forfuture1978/category/300665.html)
* [长尾理论(16)](http://www.cnblogs.com/forfuture1978/category/300666.html)
* [管理学(10)](http://www.cnblogs.com/forfuture1978/category/345794.html)
* [经济学(4)](http://www.cnblogs.com/forfuture1978/category/345800.html)
* [算法(1)](http://www.cnblogs.com/forfuture1978/category/345796.html)
* [闲话IT业(3)](http://www.cnblogs.com/forfuture1978/category/345795.html)
* [心理学与管理学效应(9)](http://www.cnblogs.com/forfuture1978/category/300667.html)
* [组织行为学(15)](http://www.cnblogs.com/forfuture1978/category/300668.html)

### 随笔档案

* [2012年11月 (3)](http://www.cnblogs.com/forfuture1978/archive/2012/11.html)
* [2012年1月 (5)](http://www.cnblogs.com/forfuture1978/archive/2012/01.html)
* [2011年12月 (6)](http://www.cnblogs.com/forfuture1978/archive/2011/12.html)
* [2011年10月 (3)](http://www.cnblogs.com/forfuture1978/archive/2011/10.html)
* [2011年9月 (1)](http://www.cnblogs.com/forfuture1978/archive/2011/09.html)
* [2010年11月 (8)](http://www.cnblogs.com/forfuture1978/archive/2010/11.html)
* [2010年10月 (1)](http://www.cnblogs.com/forfuture1978/archive/2010/10.html)
* [2010年9月 (1)](http://www.cnblogs.com/forfuture1978/archive/2010/09.html)
* [2010年8月 (1)](http://www.cnblogs.com/forfuture1978/archive/2010/08.html)
* [2010年7月 (1)](http://www.cnblogs.com/forfuture1978/archive/2010/07.html)
* [2010年6月 (6)](http://www.cnblogs.com/forfuture1978/archive/2010/06.html)
* [2010年5月 (22)](http://www.cnblogs.com/forfuture1978/archive/2010/05.html)
* [2010年4月 (18)](http://www.cnblogs.com/forfuture1978/archive/2010/04.html)
* [2010年3月 (8)](http://www.cnblogs.com/forfuture1978/archive/2010/03.html)
* [2010年2月 (39)](http://www.cnblogs.com/forfuture1978/archive/2010/02.html)
* [2010年1月 (1)](http://www.cnblogs.com/forfuture1978/archive/2010/01.html)
* [2009年12月 (6)](http://www.cnblogs.com/forfuture1978/archive/2009/12.html)

### 相册

* [IT外企那点儿事](http://www.cnblogs.com/forfuture1978/gallery/247104.html)

### 最新评论

* [1. Re:IT外企那点儿事(12)：也说跳槽](http://www.cnblogs.com/forfuture1978/archive/2012/11/26/2788610.html#2727561)
* 楼主怎么之后没有更新hadoop的相关信息了呢？是没有再研究了吗？
* --lyeoswu
* [2. Re:Lucene 原理与代码分析完整版](http://www.cnblogs.com/forfuture1978/archive/2010/06/13/1757479.html#2713121)
* 提个建议，你生成的pdf中没有目录，影响阅读，用office转制的过程中其实设置一下即可，方便大众嘛~，还望能发我一份，谢谢！
sendreams@hotmail.com
* --sendreams
* [3. Re:IT外企那点儿事(12)：也说跳槽](http://www.cnblogs.com/forfuture1978/archive/2012/11/26/2788610.html#2712415)
* [@]( "查看所回复的评论")mojunbin
现在这公司，本来做的Siverlight，我进去后没多久就转JAVA了，最近在公司折腾JAVA的一些东西，业余时间玩玩游戏，看看CLR、并折腾linux。现在观点有所转变，觉得学技术更多的是为了扩宽思维、提高眼界
* --峰顶飞龙
* [4. Re:IT外企那点儿事(12)：也说跳槽](http://www.cnblogs.com/forfuture1978/archive/2012/11/26/2788610.html#2711923)
* [@]( "查看所回复的评论")峰顶飞龙
您的经历和我差不多，呵呵。不晓得现在兄弟在搞C/C++呢？
* --mojunbin
* [5. Re:Hadoop学习总结之五：Hadoop的运行痕迹](http://www.cnblogs.com/forfuture1978/archive/2010/11/23/1884967.html#2708814)
* 楼主你好，在远程调试MapReduce时，本地代码进入不了自定义的job类，而是进入到Credentials class中，此类在hadoop-core-1.0.4.jar中，请问楼主在调试过程可否遇到此问题？
* --彭莉珊

### 阅读排行榜

* [1. IT外企那点儿事(8)：又是一年加薪时(26798)](http://www.cnblogs.com/forfuture1978/archive/2010/08/04/1791660.html)
* [2. Lucene 原理与代码分析完整版(25615)](http://www.cnblogs.com/forfuture1978/archive/2010/06/13/1757479.html)
* [3. 从技术生命周期看IT历史(20877)](http://www.cnblogs.com/forfuture1978/archive/2010/02/23/1671909.html)
* [4. 101个著名的管理学及心理学效应(20826)](http://www.cnblogs.com/forfuture1978/archive/2009/12/21/1628546.html)
* [5. Hadoop学习总结之三：Map-Reduce入门(18674)](http://www.cnblogs.com/forfuture1978/archive/2010/11/14/1877086.html)

### 评论排行榜

* [1. Lucene 原理与代码分析完整版(68)](http://www.cnblogs.com/forfuture1978/archive/2010/06/13/1757479.html)
* [2. IT外企那点儿事(4)：激动人心的入职演讲(39)](http://www.cnblogs.com/forfuture1978/archive/2010/05/05/1727644.html)
* [3. IT外企那点儿事(6)：管理路线和技术路线(37)](http://www.cnblogs.com/forfuture1978/archive/2010/05/13/1734162.html)
* [4. IT外企那点儿事(8)：又是一年加薪时(35)](http://www.cnblogs.com/forfuture1978/archive/2010/08/04/1791660.html)
* [5. IT外企那点儿事(12)：也说跳槽(33)](http://www.cnblogs.com/forfuture1978/archive/2012/11/26/2788610.html)

### 推荐排行榜

* [1. Lucene 原理与代码分析完整版(55)](http://www.cnblogs.com/forfuture1978/archive/2010/06/13/1757479.html)
* [2. IT外企那点儿事(3)：奇怪的面试(39)](http://www.cnblogs.com/forfuture1978/archive/2010/05/03/1726200.html)
* [3. IT外企那点儿事(8)：又是一年加薪时(36)](http://www.cnblogs.com/forfuture1978/archive/2010/08/04/1791660.html)
* [4. IT外企那点儿事(12)：也说跳槽(34)](http://www.cnblogs.com/forfuture1978/archive/2012/11/26/2788610.html)
* [5. IT外企那点儿事(6)：管理路线和技术路线(27)](http://www.cnblogs.com/forfuture1978/archive/2010/05/13/1734162.html)

[Lucene学习总结之二：Lucene的总体架构](http://www.cnblogs.com/forfuture1978/archive/2009/12/14/1623596.html)

Lucene总的来说是：

* 一个高效的，可扩展的，全文检索库。
* 全部用Java实现，无须配置。
* 仅支持纯文本文件的索引(Indexing)和搜索(Search)。
* 不负责由其他格式的文件抽取纯文本文件，或从网络中抓取文件的过程。

在Lucene in action中，Lucene 的构架和过程如下图，

[![image]( "image")](http://images.cnblogs.com/cnblogs_com/forfuture1978/WindowsLiveWriter/LuceneLucene_FCFA/image_2.png)

**说明Lucene****是有索引和搜索的两个过程，包含索引创建，索引，搜索三个要点。**

让我们更细一些看Lucene的各组件：

[![lucene  zong ti jia gou]( "lucene  zong ti jia gou")](http://images.cnblogs.com/cnblogs_com/forfuture1978/WindowsLiveWriter/LuceneLucene_FCFA/lucene  zong ti jia gou_2.jpg)

* **被索引的文档用Document对象****表示。**
* **IndexWriter****通过函数addDocument****将文档添加到索引中，实现创建索引的过程。**
* **Lucene****的索引是应用反向索引。**
* **当用户有请求时，Query****代表用户的查询语句。**
* **IndexSearcher****通过函数search****搜索Lucene Index****。**
* **IndexSearcher****计算term weight****和score****并且将结果返回给用户。**
* **返回给用户的文档集合用TopDocsCollector****表示。**

 

那么如何应用这些组件呢？

让我们再详细到对Lucene API 的调用实现索引和搜索过程。

[![using lucene]( "using lucene")](http://images.cnblogs.com/cnblogs_com/forfuture1978/WindowsLiveWriter/LuceneLucene_FCFA/using lucene_2.jpg)

* **索引过程如下：**

* **创建一个IndexWriter****用来写索引文件，它有几个参数，INDEX_DIR****就是索引文件所存放的位置，Analyzer****便是用来对文档进行词法分析和语言处理的。**
* **创建一个Document****代表我们要索引的文档。**
* **将不同的Field****加入到文档中。我们知道，一篇文档有多种信息，如题目，作者，修改时间，内容等。不同类型的信息用不同的Field****来表示，在本例子中，一共有两类信息进行了索引，一个是文件路径，一个是文件内容。其中FileReader****的SRC_FILE****就表示要索引的源文件。**
* **IndexWriter****调用函数addDocument****将索引写到索引文件夹中。**
* **搜索过程如下：**

* **IndexReader****将磁盘上的索引信息读入到内存，INDEX_DIR****就是索引文件存放的位置。**
* **创建IndexSearcher****准备进行搜索。**
* **创建Analyer****用来对查询语句进行词法分析和语言处理。**
* **创建QueryParser****用来对查询语句进行语法分析。**
* **QueryParser****调用parser****进行语法分析，形成查询语法树，放到Query****中。**
* **IndexSearcher****调用search****对查询语法树Query****进行搜索，得到结果TopScoreDocCollector****。**

以上便是Lucene API函数的简单调用。

然而当进入Lucene的源代码后，发现Lucene有很多包，关系错综复杂。

然而通过下图，我们不难发现，Lucene的各源码模块，都是对普通索引和搜索过程的一种实现。

此图是上一节介绍的全文检索的流程对应的Lucene实现的包结构。(参照[http://www.lucene.com.cn/about.htm](http://www.lucene.com.cn/about.htm)中文章《开放源代码的全文检索引擎Lucene》)

[![clip_image008]( "clip_image008")](http://images.cnblogs.com/cnblogs_com/forfuture1978/WindowsLiveWriter/LuceneLucene_FCFA/clip_image008_2.jpg)

* **Lucene****的analysis****模块主要负责词法分析及语言处理而形成Term****。**
* **Lucene****的index****模块主要负责索引的创建，里面有IndexWriter****。**
* **Lucene****的store****模块主要负责索引的读写。**
* **Lucene****的QueryParser****主要负责语法分析。**
* **Lucene****的search****模块主要负责对索引的搜索。**
* **Lucene****的similarity****模块主要负责对相关性打分的实现。**

了解了Lucene的整个结构，我们便可以开始Lucene的源码之旅了。

 

另：

CSDN此文章链接为：[http://blog.csdn.net/forfuture1978/archive/2009/10/30/4745802.aspx](http://blog.csdn.net/forfuture1978/archive/2009/10/30/4745802.aspx)

Javaeye此文章链接为：[http://forfuture1978.javaeye.com/blog/546808](http://forfuture1978.javaeye.com/blog/546808) 

分类: [Lucene原理与代码分析](http://www.cnblogs.com/forfuture1978/category/300665.html)

绿色通道： [好文要顶]() [关注我]() [收藏该文]()[与我联系](http://space.cnblogs.com/msg/send/%e8%a7%89%e5%85%88) [![]()]( "分享至新浪微博")
[![]()](http://home.cnblogs.com/u/forfuture1978/)

[觉先](http://home.cnblogs.com/u/forfuture1978/)
[关注 - 3](http://home.cnblogs.com/u/forfuture1978/followees)
[粉丝 - 560](http://home.cnblogs.com/u/forfuture1978/followers)

荣誉：[推荐博客](http://www.cnblogs.com/expert/)
[+加关注]()

3

0
(请您对文章做出评价)

[«](http://www.cnblogs.com/forfuture1978/archive/2009/12/14/1623594.html) 上一篇：[Lucene学习总结之一：全文检索的基本原理](http://www.cnblogs.com/forfuture1978/archive/2009/12/14/1623594.html "发布于2009-12-14 12:31")
[»](http://www.cnblogs.com/forfuture1978/archive/2009/12/14/1623597.html) 下一篇：[Lucene学习总结之三：Lucene的索引文件格式(1)](http://www.cnblogs.com/forfuture1978/archive/2009/12/14/1623597.html "发布于2009-12-14 12:34")
posted on 2009-12-14 12:32 [觉先](http://www.cnblogs.com/forfuture1978/) 阅读(7647) 评论(2) [编辑](http://www.cnblogs.com/forfuture1978/admin/EditPosts.aspx?postid=1623596) [收藏]()

[]()
### 评论

[#1楼]()[]()  2010-02-23 10:33  [xuanfeng](http://www.cnblogs.com/xuanfeng/) [ ](http://space.cnblogs.com/msg/send/xuanfeng "发送站内短消息")

朋友,可不可以了解一下你是用什么工具画的图啊?

[支持(0)]()[反对(0)]()
  
[#2楼]()[]()[楼主]17671232010/2/23 10:57:04  2010-02-23 10:57  [觉先](http://www.cnblogs.com/forfuture1978/) [ ](http://space.cnblogs.com/msg/send/%e8%a7%89%e5%85%88 "发送站内短消息")

[@]( "查看所回复的评论")xuanfeng
比较复杂的图用visio，
简单的图可以用ppt，然后另存为jpg

[支持(0)]()[反对(0)]()
http://pic.cnitblog.com/face/u103165.jpg
  
[刷新评论]()[刷新页面]()[返回顶部]()

注册用户登录后才能发表评论，请 [登录]() 或 [注册]()，[访问](http://www.cnblogs.com/)网站首页。
[博客园首页](http://www.cnblogs.com/ "程序员的网上家园")[博问](http://q.cnblogs.com/ "程序员问答社区")[新闻](http://news.cnblogs.com/ "IT新闻")[闪存](http://home.cnblogs.com/ing/)[程序员招聘](http://job.cnblogs.com/)[知识库](http://kb.cnblogs.com/)

**最新IT新闻**:
· [这四年 Google中国落寂了 刘允尽力了](http://news.cnblogs.com/n/182453/)
· [在线课程“慕课”来袭 专家称大学应主动参与](http://news.cnblogs.com/n/182452/)
· [告别编程课，MIT 展示自然语言编程](http://news.cnblogs.com/n/182446/)
· [什么样的工程师才可能真正推动创新？](http://news.cnblogs.com/n/182445/)
· [在线教育“钱途”光明？](http://news.cnblogs.com/n/182444/)
» [更多新闻...](http://news.cnblogs.com/ "IT新闻")

**最新知识库文章**:
· [阿里巴巴集团去IOE运动的思考与总结](http://kb.cnblogs.com/page/141892/)
· [硅谷归来7点分享：创业者，做你自己](http://kb.cnblogs.com/page/182265/)
· [我为什么不能坚持？](http://kb.cnblogs.com/page/182200/)
· [成为高效程序员的7个重要习惯](http://kb.cnblogs.com/page/168725/)
· [谈谈对BPM的理解](http://kb.cnblogs.com/page/182047/)
» [更多知识库文章...](http://kb.cnblogs.com/)
Powered by:
[博客园](http://www.cnblogs.com/)
Copyright © 觉先
