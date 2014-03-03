---
layout: post
title: "EhCache小结 - - ITeye技术网站"
categories: 缓存
tags: 
 - 缓存
 - ehcache
--- 

# EhCache小结 - - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼](http://nanguocoffee.iteye.com/blog/356324#)

[招聘](http://www.iteye.com/job) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://nanguocoffee.iteye.com/login "登录") [登录](http://nanguocoffee.iteye.com/login) [注册](http://nanguocoffee.iteye.com/signup)

# [nanguocoffee](http://nanguocoffee.iteye.com/)

* [**博客**](http://nanguocoffee.iteye.com/)
* [微博](http://nanguocoffee.iteye.com/weibo)
* [相册](http://nanguocoffee.iteye.com/album)
* [收藏](http://nanguocoffee.iteye.com/link)
* [留言](http://nanguocoffee.iteye.com/blog/guest_book)
* [关于我](http://nanguocoffee.iteye.com/blog/profile)

### [EhCache小结]() **

**博客分类：**
* [缓存](http://nanguocoffee.iteye.com/category/60003)
[Cache](http://www.iteye.com/blogs/tag/Cache)[数据结构](http://www.iteye.com/blogs/tag/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)[算法](http://www.iteye.com/blogs/tag/%E7%AE%97%E6%B3%95)[配置管理](http://www.iteye.com/blogs/tag/%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86)[Hibernate](http://www.iteye.com/blogs/tag/Hibernate) 

                                            
一：接口和类的作用：
Elemenet：
key
value
lastAccessTime
hitCount
等描述cache中元素的信息。
Store: 实际上存放Element的对象，Cache针对对象的操作一般都委托Store对象。
- MemoryStore： 继承该类来实现自定义MemoryStore。该类存放数据时，先使用Map存放数据，然后再判断是否超出容量。
put()
doPut() // 用来清理元素
-LruMemoryStore.java  使用 LinkedHashMap的子类，doput（）为空，因为该map的实现中的removeEldestEntry方法清理了超出容量的元素。
-LfuMemoryStore.java  使用HashMap
-FifoMemoryStore.java 使用LinkedHashMap  这里用队列应该更快吧？？源码中还需要遍历map。
-DiskStore：
其实diskStore也是用一个hashmap来存放数据，只有显式的调用flush（）方法的时候才判断是否要将元素写入文件中。
CacheManager 来管理 Cache ；
Cache用来操作Element，Cache是抽象出来的逻辑概念，其 内部使用Store来处理Element
Element就是数据描述缓存对象的基本信息。
Store是保管缓存对象的仓库，这个仓库分Memory和Disk两种。
EHCache中的EventListener是针对分布式的，普通的应用可以不用考虑。
二：一些功能细节：
1：超出设定的元素容量的解决方法
MemoryStore：
When an element is added to a cache and it goes beyond its maximum memory size,
an existing element is either deleted, if overflowToDisk is false, or evaluated for spooling to disk,
if overflowToDisk is true. In the latter case, a check for expiry is carried out. If it is expired it is deleted; if not it is spooled.
判断元素是否过期，MemoryStore有3种策略可选，LRU，LFU，FIFO，默认为LRU。
DiskStore：
使用LFU，DiskStore使用一个守护线程，DiskStore.SpoolAndExpiryThread内部类来定时检查diskStore中的元素，
定时刷新到文件中，如果遇到过期的元素则移除。
2：元素存活时间到期
MemoryStore：
在get（）和超出容量的时候才判断是否过期
DiskStore：
只有在flush的时候才判断是否过期，如果过期则移除。
也就是说，这两种方式，不论在文件或者内存中，都会依然存在过期的元素，
这些元算可能永远存在，但不影响我们的操作，因为我们get（）或者超出容量的时候都会判断是否过期并清除。
三：初始化
CacheManager cacheManager = new CacheManager();
调用protected void init(Configuration configuration, String configurationFileName, URL configurationURL,   InputStream configurationInputStream) 方法进行初始化。
1：解析配置信息：
1.1 查找ehcache.xml文件，如果找不到则使用默认的ehcache-failsafe.xml，返回一个URL对象。
1.2 调用ConfigurationFactory.parseConfiguration(input) 方法来进行解析。返回Configuration对象。
Configuration的数据结构：
    private DiskStoreConfiguration diskStoreConfiguration;
    private CacheConfiguration defaultCacheConfiguration;
    private List<FactoryConfiguration> cacheManagerPeerProviderFactoryConfiguration = new ArrayList<FactoryConfiguration>();
    private List<FactoryConfiguration> cacheManagerPeerListenerFactoryConfiguration = new ArrayList<FactoryConfiguration>();
    private FactoryConfiguration cacheManagerEventListenerFactoryConfiguration;
    private final Map cacheConfigurations = new HashMap();
    private String configurationSource;
该数据结构是针对xml定义来的，其中最复杂也最常用的算是CacheConfiguration，其内部结构也很简单。
cacheConfigurations存放多个CacheConfiguration。
2：根据Configuration进行CacheManager，Cache，Store等数据结构的初始化。
总结：
EHCache是一款比较简单的开源缓存产品，但也存在一些不足之处：
比如：
1：CacheManager和Cache这个具体类耦合，当然也提供了针对EHCache接口的方法，如果使用自定义的Cache的话容易混淆方法。
2：针对不同的Cache类，我们希望指定不同的MemoryStore实现、DiskStore实现，EHCache只提供了通过程序来达到这一功能，
没有提供通过配置文件来提供(类似于Hibernate配置文件中CacheProvider，JDBC  dialect 等)。
3：有些算法实现得不算好，比如FifoMemoryStore.java中的清理元素算法选择的数据结构不够快速。
分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[JForum源码学习研究1-起步](http://nanguocoffee.iteye.com/blog/467199 "JForum源码学习研究1-起步") | [EhCache入门](http://nanguocoffee.iteye.com/blog/356322 "EhCache入门")
* 2009-03-27 18:34
* 浏览 1010
* [评论(0)](http://nanguocoffee.iteye.com/blog/356324#comments)
* [相关推荐](http://www.iteye.com/wiki/blog/356324)

### 评论

[]()
### 发表评论

[![]()](http://nanguocoffee.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://nanguocoffee.iteye.com/login)

[![NanguoCoffee的博客]( "NanguoCoffee的博客: ")](http://nanguocoffee.iteye.com/)

NanguoCoffee

* 浏览: 10206 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 北京
* ![]()
### 最近访客 [更多访客>>](http://nanguocoffee.iteye.com/blog/user_visits)

[![bingki的博客]( "bingki的博客: 我的IT生活...")](http://bingki.iteye.com/)

[bingki](http://bingki.iteye.com/ "bingki")

[![Turandot的博客]( "Turandot的博客: ")](http://turandot.iteye.com/)

[Turandot](http://turandot.iteye.com/ "Turandot")
[![dylinshi126的博客]( "dylinshi126的博客: ")](http://dylinshi126.iteye.com/)

[dylinshi126](http://dylinshi126.iteye.com/ "dylinshi126")

[![luwies的博客]( "luwies的博客: ")](http://luwies.iteye.com/)

[luwies](http://luwies.iteye.com/ "luwies")

### 文章分类

* [全部博客 (23)](http://nanguocoffee.iteye.com/)
* [缓存 (2)](http://nanguocoffee.iteye.com/category/60003)
* [JForum (3)](http://nanguocoffee.iteye.com/category/79080)
* [Java IO (3)](http://nanguocoffee.iteye.com/category/121602)
* [其他 (7)](http://nanguocoffee.iteye.com/category/121603)
* [Java 多线程 (1)](http://nanguocoffee.iteye.com/category/121996)
* [性能优化 (1)](http://nanguocoffee.iteye.com/category/132461)
* [Oracle (1)](http://nanguocoffee.iteye.com/category/132820)
* [编码相关 (6)](http://nanguocoffee.iteye.com/category/137742)
### 社区版块

* [我的资讯](http://nanguocoffee.iteye.com/blog/news) (0)
* [我的论坛](http://nanguocoffee.iteye.com/blog/post) (118)
* [我的问答](http://nanguocoffee.iteye.com/blog/answered_problems) (18)

### 存档分类

* [2011-02](http://nanguocoffee.iteye.com/blog/monthblog/2011-02) (1)
* [2011-01](http://nanguocoffee.iteye.com/blog/monthblog/2011-01) (1)
* [2010-12](http://nanguocoffee.iteye.com/blog/monthblog/2010-12) (6)
* [更多存档...](http://nanguocoffee.iteye.com/blog/monthblog_more)
### 最新评论

* [NanguoCoffee](http://nanguocoffee.iteye.com/ "NanguoCoffee")： javantsky 写道楼主为什么要自己实现分离锁呢？ jav ...
[知道为啥HashMap里面的数组size必须是2的次幂？](http://nanguocoffee.iteye.com/blog/907824#bc1905975)
* [javantsky](http://javantsky.iteye.com/ "javantsky")： 楼主为什么要自己实现分离锁呢？java.util.concur ...
[知道为啥HashMap里面的数组size必须是2的次幂？](http://nanguocoffee.iteye.com/blog/907824#bc1905933)
* [obullxl](http://obullxl.iteye.com/ "obullxl")： LZ分析有道理，最后的&操作，（LOCK_NUM - ...
[知道为啥HashMap里面的数组size必须是2的次幂？](http://nanguocoffee.iteye.com/blog/907824#bc1905828)
* [NanguoCoffee](http://nanguocoffee.iteye.com/ "NanguoCoffee")： sniffer123 写道LZ你自己的写法有问题啊。。跟HAS ...
[知道为啥HashMap里面的数组size必须是2的次幂？](http://nanguocoffee.iteye.com/blog/907824#bc1905087)
* [sniffer123](http://sniffer123.iteye.com/ "sniffer123")： LZ你自己的写法有问题啊。。跟HASH是不是 2的幂一点关系也 ...
[知道为啥HashMap里面的数组size必须是2的次幂？](http://nanguocoffee.iteye.com/blog/907824#bc1905063)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2012 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![]()
