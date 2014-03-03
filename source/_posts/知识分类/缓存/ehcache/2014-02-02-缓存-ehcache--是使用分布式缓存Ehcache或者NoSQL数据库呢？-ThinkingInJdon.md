---
layout: post
title: "是使用分布式缓存Ehcache或者NoSQL数据库呢？ - Thinking In Jdon"
categories: 缓存
tags: 
 - 缓存
 - ehcache
--- 

# 是使用分布式缓存Ehcache或者NoSQL数据库呢？ - Thinking In Jdon

 标签的主题列表  [**缓存Cache**     ](http://www.jdon.com/tags/313)[![关注本标签 有新回复自动通知我]() 关注](http://www.jdon.com/account/protected/sub/subAction.shtml?subscribeType=2&subscribeId=313) ![]()[通过cache来发挥同步与异步的优势](http://www.jdon.com/44212 "通过cache来发挥同步与异步的优势") ![]()[关于CQRS中，对于一个领域对象的lock](http://www.jdon.com/44161 "关于CQRS中，对于一个领域对象的lock") ![]()[JVM堆大小的建议](http://www.jdon.com/44132 "JVM堆大小的建议") ![]()[关于牛XX的内存领域+事件驱动的问题！！！](http://www.jdon.com/43889 "关于牛XX的内存领域+事件驱动的问题！！！") ![]()[Stack Overflow:memcached和Redis的比较](http://www.jdon.com/43818 "Stack Overflow:memcached和Redis的比较") ![]()[海量数据的查询缓存问题](http://www.jdon.com/43802 "海量数据的查询缓存问题") ![]()[为Domain寻求一种缓存](http://www.jdon.com/43760 "为Domain寻求一种缓存") ![]()[突破JVM内存：开源DirectMemory](http://www.jdon.com/43743 "突破JVM内存：开源DirectMemory") ![]()[数据网格 Data Grid和NoSQL相同和区别-异同](http://www.jdon.com/43724 "数据网格 Data Grid和NoSQL相同和区别-异同") ![]()[高并发开发？？](http://www.jdon.com/43717 "高并发开发？？") ![]()[弱一致性在现实世界中到处存在](http://www.jdon.com/43246 "弱一致性在现实世界中到处存在") ![]()[一个常见但不是那么容易的架构问题](http://www.jdon.com/43156 "一个常见但不是那么容易的架构问题") ![]()[如何处理频繁创建对象然后丢弃导致频繁GC的情况](http://www.jdon.com/43067 "如何处理频繁创建对象然后丢弃导致频繁GC的情况") ![]()[使用ehcache元注释提高Spring 性能源码案例](http://www.jdon.com/42736 "使用ehcache元注释提高Spring 性能源码案例") ![]()[SNS Webgame 社区类页面网游开发， 自己所用的架构，以及遇到的一些问题和困惑](http://www.jdon.com/42661 "SNS Webgame 社区类页面网游开发， 自己所用的架构，以及遇到的一些问题和困惑") ![]()[JVM伪共享](http://www.jdon.com/42451 "JVM伪共享") ![]()[数据管理的未来: “Disk-less” 风格数据库?](http://www.jdon.com/42438 "数据管理的未来: “Disk-less” 风格数据库?") ![]()[亚马逊的弹性缓存](http://www.jdon.com/42361 "亚马逊的弹性缓存") ![]()[如何驯服java GC导致暂停? 使用16GiB以上heap](http://www.jdon.com/41686 "如何驯服java GC导致暂停? 使用16GiB以上heap") ![]()[使用设计模式后的副作用](http://www.jdon.com/41612 "使用设计模式后的副作用")

### 分享到

* [QQ空间](http://www.jdon.com/39931#)
* [新浪微博](http://www.jdon.com/39931#)
* [百度搜藏](http://www.jdon.com/39931#)
* [人人网](http://www.jdon.com/39931#)
* [腾讯微博](http://www.jdon.com/39931#)
* [开心网](http://www.jdon.com/39931#)
* [腾讯朋友](http://www.jdon.com/39931#)
* [百度空间](http://www.jdon.com/39931#)
* [豆瓣网](http://www.jdon.com/39931#)
* [搜狐微博](http://www.jdon.com/39931#)
* [百度新首页](http://www.jdon.com/39931#)
* [QQ收藏](http://www.jdon.com/39931#)
* [和讯微博](http://www.jdon.com/39931#)
* [我的淘宝](http://www.jdon.com/39931#)
* [百度贴吧](http://www.jdon.com/39931#)
* [更多...](http://www.jdon.com/39931#)

[百度分享](http://www.jdon.com/39931#)
[![JiveJdon Community Forums]()](http://www.jdon.com/)[在线]()    [主题表](http://www.jdon.com/threads)   [标签](http://www.jdon.com/tags)   [查搜](http://www.jdon.com/query/threadViewQuery.shtml)   [注册](http://www.jdon.com/account/newAccountForm.shtml)   [登陆]()   [发帖](http://www.jdon.com/forum/post.jsp)   [关注](http://www.jdon.com/followus.html)   [设计模式](http://www.jdon.com/designpatterns/) [领域驱动设计](http://www.jdon.com/ddd.html) [云架构](http://www.jdon.com/design.htm) [JiveJdon](http://www.jdon.com/jdonframework/jivejdon3/index.html) [Jdon框架](http://www.jdon.com/jdonframework/) [企业咨询](http://www.jdon.com/consultant.html) [主题讨论](http://www.jdon.com/threads)

  用户      自动登陆 密码      [新用户注册](http://www.jdon.com/account/newAccountForm.shtml) [忘记密码?](http://www.jdon.com/account/forgetPasswd.jsp) [![登录]()新浪微博](http://www.jdon.com/account/oauth/sinaCallAction.shtml) [![登录]()腾讯微博](http://www.jdon.com/account/oauth/tecentCallAction.shtml) [![登录]()Google](http://www.jdon.com/account/oauth/googleCallAction.shtml)
**[首页](http://www.jdon.com/) » [论坛](http://www.jdon.com/forum/ "返回论坛列表") » [云架构等扩展性讨论](http://www.jdon.com/forum/121)** ![]() [  ]( "网上收藏本主题")

[  ]( "手机条码扫描浏览本页")
[  ](http://www.jdon.com/account/protected/sub/subAction.shtml?subscribeType=1&subscribeId=39931 "关注本主题")

[  ]( "加入本帖到收藏夹")

阅读3831次 1人关注

前往下页:
  [![发表新帖子]()](http://www.jdon.com/message/messageAction.shtml?forum.forumId=121)  [![回复该主题贴]()]() []()

**banq**
[![]()
个人详介按这里
](http://www.jdon.com/blog/banq) [文章: 12593](http://www.jdon.com/query/threadViewQuery.shtml?queryType=userMessageQueryAction&user=2)
注册: 2002-08-03
* 当前离线
* [184人关注](http://www.jdon.com/account/protected/sub/subAction.shtml?subscribeType=3&subscribeId=2 "我要关注该作者发言")
* [悄悄话]()
* [个人博客](http://www.jdon.com/profile.jsp?user=banq)

是使用分布式缓存Ehcache或者NoSQL数据库呢？

2011-03-03 10:31
[  ]( "到本帖网址")  [  ]()  [  ]()

[![标签]()](http://www.jdon.com/query/tagsList.shtml?count=150 "标签") [缓存Cache](http://www.jdon.com/tags/313)        [NoSQL](http://www.jdon.com/tags/8600)        [ehcache](http://www.jdon.com/tags/1103)        [可扩展性Scalable](http://www.jdon.com/tags/2513)      
10

[顶一下]()

# Ehcache实际是一个Java的开源[**缓存**](http://www.jdon.com/cache.html)产品，用来提升性能，降低负载，方便可[**伸缩性**](http://www.jdon.com/scalable.html)。后面有秦始皇Terracotta兵马俑式的服务器矩阵支持, Ehcache成为一种线性可扩展的分布式[**缓存**](http://www.jdon.com/cache.html)。它是一个无关系结构schema-less, key-value, 基于Java的分布式[**缓存**](http://www.jdon.com/cache.html)，它提供可靠的一致性控制，可根据主键key 值value或属性索引搜索。
**Flexible Consistency可靠的一致性**
从Ehcache 和 Terracotta整合性来看, 我们可以激活相关的数据跨集群共享，按照CAP理论来说， 由于避免单机硬编码权衡考虑，我们在一个个[**缓存**](http://www.jdon.com/cache.html)中创建了一个丰富的一致性模型；同时为了易于理解，我们采取统一标准客户端一致性模型来描述和配置它。
我们可以实现一个比[**NoSQL**](http://www.jdon.com/DistributedSystems.html)解决方案更加丰富的一致性富模型(模型驱动框架开源[**Jdonframework**](http://www.jdon.com/jdonframework/)缺省采取echcahe的原因)。我们可以为每一个[**缓存**](http://www.jdon.com/cache.html)提供跨集群的如下特性:
1.通过悲观锁支持强壮的一致性(缺省，一致性也就是[**事务**](http://www.jdon.com/jivejdon/tags/354)性，数据[**事务**](http://www.jdon.com/jivejdon/tags/354)安全性)
2.未加锁的弱一致性，用来先写后读，或只读，或者只写。
3.乐观锁 Compare and Swap (“CAS”)，跨集群服务器的原子操作 。
4.XA分布式[**事务**](http://www.jdon.com/jivejdon/tags/354)和本地[**事务**](http://www.jdon.com/jivejdon/tags/354)机制transactions
5.一个显式的锁API，可以运行用户优化[**事务**](http://www.jdon.com/jivejdon/tags/354)一致性带来的性能问题。
Ehcache架构非常不同于[**NoSQL**](http://www.jdon.com/DistributedSystems.html)(如memorycache ). 每个服务器内存中会驻留使用最频繁(LRU算法)的数据或对象。可以有4-6 GB heap 空间, 如果辅助以BigMemory, 能够扩展至上百 GBs(注意这是每台，如果考虑集群多台，几乎可以把数据库中数据都加载到内存中，相当于一个内存数据库). 这是一种Level 1 (“L1”) [**缓存**](http://www.jdon.com/cache.html)，完全基于内存in-process. 访问L1 速度一般少于1 μs.
整个[**缓存**](http://www.jdon.com/cache.html)一般都驻扎Terracotta服务器阵列中. 这是一种Level 2 (“L2”) [**缓存**](http://www.jdon.com/cache.html). 访问L2少于2 ms.
你从这个架构中得到的依赖于你使用经验，最普通一个情况是：the Pareto distribution, 80% 数据从 L1获得 , 20%时间从L2获得， 这样，普通延迟性一般是少于.401 ms. 通过比较，Yahoo! Cloud Serving Benchmark 基准测试表明 HBase 和 Cassandra延迟性是在 8 to 18 ms.
这使得Ehcache要快于[**NoSQL**](http://www.jdon.com/DistributedSystems.html)一个数量级别.
**持久化**
缓存能够被设置为持久化，能够提供永久保存和可重启性。一般使用一种写日志。缓存将能够恢复到一种一致性状态. 当ehcache被配置为一种分布式[**缓存**](http://www.jdon.com/cache.html)时，Ehcache将使用Terracotta Server Array 作为二级[**缓存**](http://www.jdon.com/cache.html)Level 2 cache. Terracotta servers通常是每个区配置两个，有HA. 另外，Terracotta 使用JMX tools实现备份,备份或恢复都能立即进行。 RDBMS关系数据库典型提供一个广泛归档索引 ETL等系列功能, 正因为这些理由，作者认为 Ehcache具有一些持久化小特性. 当然，这和许多[**NoSQL**](http://www.jdon.com/DistributedSystems.html) 产品类似。
**大数据量Big Data**
大数据量指成打T级别terabytes甚至达到 petabytes.
Ehcache目前 可以达到2 TB. 以后会不断提升(banq注：集群的问题还是一个群的问题，群的规模是有上限天花板的，这和破坏了数据关系的[**NoSQL**](http://www.jdon.com/DistributedSystems.html)分布式是无法比拟的)
ehcache对大数据量的限制主要还是受制于关系数据库RDBMSs.
如果使用BigMemory，Terracotta server可以加载到每台服务器内存中有250GB或更多. [**NoSQL**](http://www.jdon.com/DistributedSystems.html) 解决方案使用内存和磁盘混合，基于Java的Cassandra问题可能是垃圾回收, 受其限制只能有很小的heaps. BigMemory 是脱离heap使用额外内存，使用NIO的 DirectByteBuffer. 最终结果你可以用很小的服务部署付出得到同样的回报。
**进入分布式[**缓存**](http://www.jdon.com/cache.html)**
如果Ehcahce不是[**NoSQL**](http://www.jdon.com/DistributedSystems.html), 那它是什么? 答案是它是一个分布式[**缓存**](http://www.jdon.com/cache.html)distributed cache. 象它的[**NoSQL**](http://www.jdon.com/DistributedSystems.html)表兄弟, 它经常用于传统数据库不能覆盖的领域，缓存一个web page或或者耗费CPU计算是经常碰到的情况.
关键问题是要更快地访问到这些计算好的结果，这时并不是需要持久化. 在Java中最快的访问架构就是in-process caching, 通过网络作为一种in-memory cache.
Gartner 和Forrester已经对分布式[**缓存**](http://www.jdon.com/cache.html)Distributed Caching 和弹性[**缓存**](http://www.jdon.com/cache.html)Elastic Caching有一个公开定义. Gartner： “分布式[**缓存**](http://www.jdon.com/cache.html)Distributed caching平台让用户管理内存中非常大的in-memory data，以便使得关系数据库减轻负载、以及[**云计算**](http://www.jdon.com/cloudcompute.html)和云[**事务**](http://www.jdon.com/jivejdon/tags/354)处理、 以及更杰出性能的[**事务**](http://www.jdon.com/jivejdon/tags/354)处理，或复杂的事件处理以及高性能计算”.
他们也将分布式[**缓存**](http://www.jdon.com/cache.html)作为一个服务(“aPaaS”) 加入他们应用平台。
当然，我们可以在RDBMSs 和 [**NoSQL**](http://www.jdon.com/DistributedSystems.html)两者之前都使用分布式[**缓存**](http://www.jdon.com/cache.html)(因为[**事务**](http://www.jdon.com/jivejdon/tags/354)的需要)。
但是，同样[**缓存**](http://www.jdon.com/cache.html)作为分布，而需要得到一些搜索之类企业特性时，和[**NoSQL**](http://www.jdon.com/DistributedSystems.html)自己区别就非常明显。
**Use Cases场景用例**
如果需要一个‘key-value plus search’key-value增强式的搜索，一旦数据量超过2 TB，那么[**NoSQL**](http://www.jdon.com/DistributedSystems.html)合适。
我们看看下面一些常用ehache缓存场景：
Hibernate Caching. JDBC caching. Web caching. Collection caching.使用echcache
快速的分析结果查询
信用卡等与钱有关......见原文
**结论**
NoSQL是瞄准替代RDBMS, [**缓存**](http://www.jdon.com/cache.html)是瞄准低延迟和速度，可以根据CAP理论来衡量。缓存主要和应用有关，可以帮助应用脱离任何存储技术，无论是RDBMS 和 [**NoSQL**](http://www.jdon.com/DistributedSystems.html)(这点比较认同，jdonframework将[**缓存**](http://www.jdon.com/cache.html)作为领域模型的生存空间，后面可以接关系数据库或[**NoSQL**](http://www.jdon.com/DistributedSystems.html)).
原文网址被封，需要翻墙。监管带来信息闭塞以致落后在我们经常看外文资料的人来说来感受非常明显。
[Ehcache: Distributed Cache or](http://nosql.mypopescu.com/post/3585092735/ehcache-distributed-cache-or-nosql-store)[**NoSQL**](http://www.jdon.com/DistributedSystems.html) Store? :: myNoSQL
[该贴被banq于2011-03-03 10:33修改过]
[]()

**banq**
[![]()
个人详介按这里
](http://www.jdon.com/blog/banq) [文章: 12593](http://www.jdon.com/query/threadViewQuery.shtml?queryType=userMessageQueryAction&user=2)
注册: 2002-08-03
* 当前离线
* [184人关注](http://www.jdon.com/account/protected/sub/subAction.shtml?subscribeType=3&subscribeId=2 "我要关注该作者发言")
* [悄悄话]()
* [个人博客](http://www.jdon.com/profile.jsp?user=banq)

1楼 是使用分布式缓存Ehcache或者NoSQL数据库呢？

2011-03-03 12:30
[  ](http://www.jdon.com/nav/39931/23131906#23131906 "到本帖网址")  [  ]()  [  ]()
[顶一下]()

# ehcache与memcached区别是在一致性，也就是[**事务**](http://www.jdon.com/jivejdon/tags/354)性方面，memcached使用HASH算法实现负载平衡，但是集群的另外一个要素：失败恢复，也就是服务器当了，数据还在其他地方存在共享；memcached的一台服务器宕了，上面数据就丢失了，由于采取Hash环，损失有限。
但是也由于没有状态数据在服务器之间共享复制，memcached对于只读性能应该比集群的更好。
上面文章主要是在基于Java的比较。

-
[]()

**SpeedVan**
[![]()
个人详介按这里
](http://www.jdon.com/blog/SpeedVan) [文章: 494](http://www.jdon.com/query/threadViewQuery.shtml?queryType=userMessageQueryAction&user=71950)
注册: 2010-08-24
* 当前离线
* [8人关注](http://www.jdon.com/account/protected/sub/subAction.shtml?subscribeType=3&subscribeId=71950 "我要关注该作者发言")
* [悄悄话]()
* [个人博客](http://www.jdon.com/profile.jsp?user=SpeedVan)

是使用分布式缓存Ehcache或者NoSQL数据库呢？

2011-03-03 21:24
[  ](http://www.jdon.com/nav/39931/23131916#23131916 "到本帖网址")  [  ]()  [  ]()
[顶一下]()

# 我现在使用cache，只是用Repository封装后，作为单系统内存接口来使用。分布技术，有待学习呢，真是路漫漫啊。自从认识web的cache之后，就知道了一个web架构，就相当于一个游戏引擎——缓存管理是其一个核心部分。
[](http://www.jdon.com/39931# "分享到QQ空间") [](http://www.jdon.com/39931# "分享到新浪微博") [](http://www.jdon.com/39931# "分享到腾讯微博") [](http://www.jdon.com/39931# "分享到人人网") 更多[![标签]()](http://www.jdon.com/query/tagsList.shtml?count=150 "标签") [缓存Cache(171)](http://www.jdon.com/tags/313)       [NoSQL(55)](http://www.jdon.com/tags/8600)       [ehcache(9)](http://www.jdon.com/tags/1103)       [可扩展性Scalable(98)](http://www.jdon.com/tags/2513)     
推荐贴列表  **置顶热贴** ![]()[LMAX架构](http://www.jdon.com/42452 "LMAX架构") [[EDA事件驱动](http://www.jdon.com/tags/3380) [异步编程](http://www.jdon.com/tags/544) ] ![]()[五年java人的一点感悟](http://www.jdon.com/42708 "五年java人的一点感悟") [[软件观点](http://www.jdon.com/tags/619) [工具心得](http://www.jdon.com/tags/514) ] ![]()[分享我的：领域驱动设计（DDD）学习成果精简总结](http://www.jdon.com/43463 "分享我的：领域驱动设计（DDD）学习成果精简总结") [[DDD领域驱动设计](http://www.jdon.com/tags/272) ] ![]()[如何提高web系统的吞吐能力？](http://www.jdon.com/43666 "如何提高web系统的吞吐能力？") [[吞吐量](http://www.jdon.com/tags/18277) [高性能](http://www.jdon.com/tags/344) ] ![]()[实践ddd，太让人沮丧了。。](http://www.jdon.com/43750 "实践ddd，太让人沮丧了。。") [[DDD领域驱动设计](http://www.jdon.com/tags/272) [jivejdon](http://www.jdon.com/tags/365) ] ![]()[使用Disruptor实现并发编程 PPT文档](http://www.jdon.com/43808 "使用Disruptor实现并发编程 PPT文档") [[disruptor](http://www.jdon.com/tags/18365) [并发并行](http://www.jdon.com/tags/4973) ] ![]()[jdon框架优缺点之我见](http://www.jdon.com/43851 "jdon框架优缺点之我见") [[JdonFramework](http://www.jdon.com/tags/315) [DSL领域特定语言](http://www.jdon.com/tags/1013) ] ![]()[Instagram卖出10亿美金的启示](http://www.jdon.com/43895 "Instagram卖出10亿美金的启示") [[云计算](http://www.jdon.com/tags/612) [项目管理](http://www.jdon.com/tags/13342) ] ![]()[Martin Fowler厌倦ORM了](http://www.jdon.com/43937 "Martin Fowler厌倦ORM了") [[对象数据库阻抗](http://www.jdon.com/tags/564) [ORM模式](http://www.jdon.com/tags/11737) ] ![]()[Spring创始人Rod Johnson离开东家VMWARE](http://www.jdon.com/44074 "Spring创始人Rod Johnson离开东家VMWARE") [[spring](http://www.jdon.com/tags/253) [akka](http://www.jdon.com/tags/10531) ]-

[使用帮助]() [联系管理员]() 探索 分享 交流 解惑 授道 OpenSource [**JIVEJDON**](http://www.jdon.com/jdonframework/jivejdon3/index.html) Powered by [**JdonFramework**](http://www.jdon.com/jdonframework/) Code © 2002-15 [**jdon.com**](http://www.jdon.com/)  [anti spam](http://www.jdon.com/common/503warn.jsp)

-

 
-

 
