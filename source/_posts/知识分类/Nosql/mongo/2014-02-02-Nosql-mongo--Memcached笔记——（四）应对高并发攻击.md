---
layout: post
title: "Memcached笔记——（四）应对高并发攻击"
categories: Nosql
tags: 
 - Nosql
 - mongo
--- 

# Memcached笔记——（四）应对高并发攻击

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼]()

[招聘](http://job.iteye.com/iteye) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://snowolf.iteye.com/login "登录") [登录](http://snowolf.iteye.com/login) [注册](http://snowolf.iteye.com/signup)

# [Snowolf的意境空间！](http://snowolf.iteye.com/)

* [**博客**](http://snowolf.iteye.com/)
* [微博](http://snowolf.iteye.com/weibo)
* [相册](http://snowolf.iteye.com/album)
* [收藏](http://snowolf.iteye.com/link)
* [留言](http://snowolf.iteye.com/blog/guest_book)
* [关于我](http://snowolf.iteye.com/blog/profile)

### [Memcached笔记——（四）应对高并发攻击]() **

**博客分类：**
* [Java／Cache](http://snowolf.iteye.com/category/214766)
* [Spring](http://snowolf.iteye.com/category/28518)
* [Server Architecture／Distributed](http://snowolf.iteye.com/category/64267)
[memcached](http://www.iteye.com/blogs/tag/memcached)[add](http://www.iteye.com/blogs/tag/add)[xmemcached](http://www.iteye.com/blogs/tag/xmemcached) 

近半个月过得很痛苦，主要是产品上线后，引来无数机器用户恶意攻击，不停的刷新产品各个服务入口，制造垃圾数据，消耗资源。他们的最好成绩，1秒钟可以并发6次，赶在Database入库前，Cache进行Missing Loading前，强占这其中十几毫秒的时间，进行恶意攻击。

 

**相关链接： 
[Memcached笔记——（一）安装&常规错误&监控 ](http://snowolf.iteye.com/blog/1447348)
[Memcached笔记——（二）XMemcached&Spring集成](http://snowolf.iteye.com/blog/1471805) 
[Memcached笔记——（三）Memcached使用总结](http://snowolf.iteye.com/blog/1576818) **

[**Memcached笔记——（四）应对高并发攻击**](http://snowolf.iteye.com/admin/blogs/1677495/edit "Memcached笔记——（四）应对高并发攻击")

 

为了应对上述情况，做了如下调整：

 

1. 更新数据时，先写Cache，然后写Database，如果可以，写操作交给队列后续完成。
1. 限制统一帐号，同一动作，同一秒钟并发次数，超过1次不做做动作，返回操作失败。
1. 限制统一用户，每日动作次数，超限返回操作失败。

要完成上述操作，同事给我支招。用Memcached的add方法，就可以很快速的解决问题。不需要很繁琐的开发，也不需要依赖数据库记录，完全内存操作。![]()

以下实现一个判定冲突的方法：

 
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. /** 
1.  * 冲突延时 1秒 
1.  */  
1. public static final int MUTEX_EXP = 1;  
1. /** 
1.  * 冲突键 
1.  */  
1. public static final String MUTEX_KEY_PREFIX = "MUTEX_";  
1.   
1. /** 
1.  * 冲突判定 
1.  *  
1.  * @param key 
1.  */  
1. public boolean isMutex(String key) {  
1.     return isMutex(key, MUTEX_EXP);  
1. }  
1.   
1. /** 
1.  * 冲突判定 
1.  *  
1.  * @param key 
1.  * @param exp 
1.  * @return true 冲突 
1.  */  
1. public boolean isMutex(String key, int exp) {  
1.     boolean status = true;  
1.     try {  
1.         if (memcachedClient.add(MUTEX_KEY_PREFIX + key, exp, "true")) {  
1.             status = false;  
1.         }  
1.     } catch (Exception e) {  
1.         logger.error(e.getMessage(), e);  
1.     }  
1.     return status;  
1.   
1. }  
/** * 冲突延时 1秒 */ public static final int MUTEX_EXP = 1; /** * 冲突键 */ public static final String MUTEX_KEY_PREFIX = "MUTEX_"; /** * 冲突判定 * * @param key */ public boolean isMutex(String key) { return isMutex(key, MUTEX_EXP); } /** * 冲突判定 * * @param key * @param exp * @return true 冲突 */ public boolean isMutex(String key, int exp) { boolean status = true; try { if (memcachedClient.add(MUTEX_KEY_PREFIX + key, exp, "true")) { status = false; } } catch (Exception e) { logger.error(e.getMessage(), e); } return status; }

 

做个说明：

 
选项 说明 add 仅当存储空间中不存在键相同的数据时才保存 replace 仅当存储空间中存在键相同的数据时才保存 set 与add和replace不同，无论何时都保存

也就是说，如果add操作返回为true，则认为当前不冲突！![]()

 

回归场景，恶意用户1秒钟操作6次，遇到上述这个方法，只有乖乖地1秒后再来。别小看这1秒钟，一个数据库操作不过几毫秒。1秒延迟，足以降低系统负载，增加恶意用户成本。

 

附我用到的基于XMemcached实现：

 
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. import net.rubyeye.xmemcached.MemcachedClient;  
1.   
1. import org.apache.log4j.Logger;  
1. import org.springframework.beans.factory.annotation.Autowired;  
1. import org.springframework.stereotype.Component;  
1.   
1. /** 
1.  *  
1.  * @author Snowolf 
1.  * @version 1.0 
1.  * @since 1.0 
1.  */  
1. @Component  
1. public class MemcachedManager {  
1.   
1.     /** 
1.      * 缓存时效 1天 
1.      */  
1.     public static final int CACHE_EXP_DAY = 3600 * 24;  
1.   
1.     /** 
1.      * 缓存时效 1周 
1.      */  
1.     public static final int CACHE_EXP_WEEK = 3600 * 24 * 7;  
1.   
1.     /** 
1.      * 缓存时效 1月 
1.      */  
1.     public static final int CACHE_EXP_MONTH = 3600 * 24 * 30;  
1.   
1.     /** 
1.      * 缓存时效 永久 
1.      */  
1.     public static final int CACHE_EXP_FOREVER = 0;  
1.   
1.     /** 
1.      * 冲突延时 1秒 
1.      */  
1.     public static final int MUTEX_EXP = 1;  
1.     /** 
1.      * 冲突键 
1.      */  
1.     public static final String MUTEX_KEY_PREFIX = "MUTEX_";  
1.     /** 
1.      * Logger for this class 
1.      */  
1.     private static final Logger logger = Logger  
1.             .getLogger(MemcachedManager.class);  
1.   
1.     /** 
1.      * Memcached Client 
1.      */  
1.     @Autowired  
1.     private MemcachedClient memcachedClient;  
1.   
1.     /** 
1.      * 缓存 
1.      *  
1.      * @param key 
1.      * @param value 
1.      * @param exp 
1.      *            失效时间 
1.      */  
1.     public void cacheObject(String key, Object value, int exp) {  
1.         try {  
1.             memcachedClient.set(key, exp, value);  
1.         } catch (Exception e) {  
1.             logger.error(e.getMessage(), e);  
1.         }  
1.         logger.info("Cache Object: [" + key + "]");  
1.     }  
1.   
1.     /** 
1.      * Shut down the Memcached Cilent. 
1.      */  
1.     public void finalize() {  
1.         if (memcachedClient != null) {  
1.             try {  
1.                 if (!memcachedClient.isShutdown()) {  
1.                     memcachedClient.shutdown();  
1.                     logger.debug("Shutdown MemcachedManager...");  
1.                 }  
1.             } catch (Exception e) {  
1.                 logger.error(e.getMessage(), e);  
1.             }  
1.         }  
1.     }  
1.   
1.     /** 
1.      * 清理对象 
1.      *  
1.      * @param key 
1.      */  
1.     public void flushObject(String key) {  
1.         try {  
1.             memcachedClient.deleteWithNoReply(key);  
1.         } catch (Exception e) {  
1.             logger.error(e.getMessage(), e);  
1.         }  
1.         logger.info("Flush Object: [" + key + "]");  
1.     }  
1.   
1.     /** 
1.      * 冲突判定 
1.      *  
1.      * @param key 
1.      */  
1.     public boolean isMutex(String key) {  
1.         return isMutex(key, MUTEX_EXP);  
1.     }  
1.   
1.     /** 
1.      * 冲突判定 
1.      *  
1.      * @param key 
1.      * @param exp 
1.      * @return true 冲突 
1.      */  
1.     public boolean isMutex(String key, int exp) {  
1.         boolean status = true;  
1.         try {  
1.             if (memcachedClient.add(MUTEX_KEY_PREFIX + key, exp, "true")) {  
1.                 status = false;  
1.             }  
1.         } catch (Exception e) {  
1.             logger.error(e.getMessage(), e);  
1.         }  
1.         return status;  
1.     }  
1.   
1.     /** 
1.      * 加载缓存对象 
1.      *  
1.      * @param key 
1.      * @return 
1.      */  
1.     public <T> T loadObject(String key) {  
1.         T object = null;  
1.         try {  
1.             object = memcachedClient.<T> get(key);  
1.         } catch (Exception e) {  
1.             logger.error(e.getMessage(), e);  
1.         }  
1.         logger.info("Load Object: [" + key + "]");  
1.         return object;  
1.     }  
1.   
1. }  
import net.rubyeye.xmemcached.MemcachedClient; import org.apache.log4j.Logger; import org.springframework.beans.factory.annotation.Autowired; import org.springframework.stereotype.Component; /** * * @author Snowolf * @version 1.0 * @since 1.0 */ @Component public class MemcachedManager { /** * 缓存时效 1天 */ public static final int CACHE_EXP_DAY = 3600 * 24; /** * 缓存时效 1周 */ public static final int CACHE_EXP_WEEK = 3600 * 24 * 7; /** * 缓存时效 1月 */ public static final int CACHE_EXP_MONTH = 3600 * 24 * 30; /** * 缓存时效 永久 */ public static final int CACHE_EXP_FOREVER = 0; /** * 冲突延时 1秒 */ public static final int MUTEX_EXP = 1; /** * 冲突键 */ public static final String MUTEX_KEY_PREFIX = "MUTEX_"; /** * Logger for this class */ private static final Logger logger = Logger .getLogger(MemcachedManager.class); /** * Memcached Client */ @Autowired private MemcachedClient memcachedClient; /** * 缓存 * * @param key * @param value * @param exp * 失效时间 */ public void cacheObject(String key, Object value, int exp) { try { memcachedClient.set(key, exp, value); } catch (Exception e) { logger.error(e.getMessage(), e); } logger.info("Cache Object: [" + key + "]"); } /** * Shut down the Memcached Cilent. */ public void finalize() { if (memcachedClient != null) { try { if (!memcachedClient.isShutdown()) { memcachedClient.shutdown(); logger.debug("Shutdown MemcachedManager..."); } } catch (Exception e) { logger.error(e.getMessage(), e); } } } /** * 清理对象 * * @param key */ public void flushObject(String key) { try { memcachedClient.deleteWithNoReply(key); } catch (Exception e) { logger.error(e.getMessage(), e); } logger.info("Flush Object: [" + key + "]"); } /** * 冲突判定 * * @param key */ public boolean isMutex(String key) { return isMutex(key, MUTEX_EXP); } /** * 冲突判定 * * @param key * @param exp * @return true 冲突 */ public boolean isMutex(String key, int exp) { boolean status = true; try { if (memcachedClient.add(MUTEX_KEY_PREFIX + key, exp, "true")) { status = false; } } catch (Exception e) { logger.error(e.getMessage(), e); } return status; } /** * 加载缓存对象 * * @param key * @return */ public <T> T loadObject(String key) { T object = null; try { object = memcachedClient.<T> get(key); } catch (Exception e) { logger.error(e.getMessage(), e); } logger.info("Load Object: [" + key + "]"); return object; } }

 

**相关链接： 
[Memcached笔记——（一）安装&常规错误&监控 ](http://snowolf.iteye.com/blog/1447348)
[Memcached笔记——（二）XMemcached&Spring集成](http://snowolf.iteye.com/blog/1471805) 
[Memcached笔记——（三）Memcached使用总结](http://snowolf.iteye.com/blog/1576818) **

[**Memcached笔记——（四）应对高并发攻击**]()
**2**
顶

**2**
踩

分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[MySQL 运维笔记（一）—— 终止高负载SQL](http://snowolf.iteye.com/blog/1679923 "MySQL 运维笔记（一）—— 终止高负载SQL") | [征服 Redis + Jedis + Spring （二）—— ...](http://snowolf.iteye.com/blog/1667104 "征服 Redis + Jedis + Spring （二）—— 哈希表操作（HMGET HMSET）")
* 2012-09-13 09:48
* 浏览 3788
* [评论(4)]()
* 分类:[企业架构](http://www.iteye.com/blogs/category/architecture)
* [相关推荐](http://www.iteye.com/wiki/blog/1677495)

### 评论

[]()

4 楼 [snowolf](http://snowolf.iteye.com/ "snowolf") 2012-12-11  

zym820910 写道

snowolf 写道

CurrentJ 写道

先写Cache，然后写Database，断电或者故障会导致用户数据丢失。
各有利弊，需要根据业务需求权衡。![]()
写得非常好！应对高并发的时候，我们通常的思维是泄洪模式，通过一道又一道的防洪大堤将洪水分流，尤其是在应对数据要求不严厉的SNS这类产品，异步的保存数据值得提倡！
不过，更好的方式是：通过旁路式架构，解决代码层面的大部分压力。现在很多商城的商品展示和搜索都采用NOSQL技术来应对处理，异步增加或更新，并不显得那么重要了，更多的是通过产品和技术架构来调整，比如通过分析用户喜好，事先静态化搜索结果。
赞同，感谢分享！![]() 最核心的优化，还是应当在产品层面多下工夫。找到用户-产品-技术，三方都能满足的平衡点。
3 楼 [zym820910](http://zym820910.iteye.com/ "zym820910") 2012-12-11  

snowolf 写道

CurrentJ 写道

先写Cache，然后写Database，断电或者故障会导致用户数据丢失。
各有利弊，需要根据业务需求权衡。![]()
写得非常好！应对高并发的时候，我们通常的思维是泄洪模式，通过一道又一道的防洪大堤将洪水分流，尤其是在应对数据要求不严厉的SNS这类产品，异步的保存数据值得提倡！
不过，更好的方式是：通过旁路式架构，解决代码层面的大部分压力。现在很多商城的商品展示和搜索都采用NOSQL技术来应对处理，异步增加或更新，并不显得那么重要了，更多的是通过产品和技术架构来调整，比如通过分析用户喜好，事先静态化搜索结果。

2 楼 [snowolf](http://snowolf.iteye.com/ "snowolf") 2012-11-07  

CurrentJ 写道

先写Cache，然后写Database，断电或者故障会导致用户数据丢失。
各有利弊，需要根据业务需求权衡。![]()
1 楼 [CurrentJ](http://currentj.iteye.com/ "CurrentJ") 2012-11-07  

先写Cache，然后写Database，断电或者故障会导致用户数据丢失。
### 发表评论

[![]()](http://snowolf.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://snowolf.iteye.com/login)

[![snowolf的博客]( "snowolf的博客: Snowolf的意境空间！")](http://snowolf.iteye.com/)

snowolf

* 浏览: 902532 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 北京
* ![]()
### 最近访客 [更多访客>>](http://snowolf.iteye.com/blog/user_visits)

[![torry_he的博客]( "torry_he的博客: ")](http://torry-he.iteye.com/)

[torry_he](http://torry-he.iteye.com/ "torry_he")

[![cxzucc的博客]( "cxzucc的博客: ")](http://cxzucc.iteye.com/)

[cxzucc](http://cxzucc.iteye.com/ "cxzucc")
[![greatestrabit的博客]( "greatestrabit的博客: ")](http://greatestrabit.iteye.com/)

[greatestrabit](http://greatestrabit.iteye.com/ "greatestrabit")

[![zhumingyuan的博客]( "zhumingyuan的博客: ")](http://zhumingyuan.iteye.com/)

[zhumingyuan](http://zhumingyuan.iteye.com/ "zhumingyuan")

### 文章分类

* [全部博客 (161)](http://snowolf.iteye.com/)
* [职场 && 心情 (22)](http://snowolf.iteye.com/category/44718)
* [Java／Basic (17)](http://snowolf.iteye.com/category/28520)
* [Java／Compression (7)](http://snowolf.iteye.com/category/102898)
* [Java／Security (22)](http://snowolf.iteye.com/category/68576)
* [Java／Maven (3)](http://snowolf.iteye.com/category/147023)
* [Java／Cache (11)](http://snowolf.iteye.com/category/214766)
* [Eclipse (4)](http://snowolf.iteye.com/category/38039)
* [Spring (19)](http://snowolf.iteye.com/category/28518)
* [ORM／Hibernate (2)](http://snowolf.iteye.com/category/39212)
* [ORM／iBatis (3)](http://snowolf.iteye.com/category/36643)
* [DB／NoSQL (10)](http://snowolf.iteye.com/category/28851)
* [DB／MySQL (7)](http://snowolf.iteye.com/category/34962)
* [DB／MS SQL Server (4)](http://snowolf.iteye.com/category/93301)
* [OS／Linux (10)](http://snowolf.iteye.com/category/64032)
* [OS／Mac (7)](http://snowolf.iteye.com/category/124565)
* [C／C++ (4)](http://snowolf.iteye.com/category/67920)
* [Server Architecture／Basic (11)](http://snowolf.iteye.com/category/48004)
* [Server Architecture／Distributed (17)](http://snowolf.iteye.com/category/64267)
* [Moblie／Andriod (2)](http://snowolf.iteye.com/category/51869)
* [WebService (3)](http://snowolf.iteye.com/category/64109)
* [Objective-C (1)](http://snowolf.iteye.com/category/195003)
* [Html (1)](http://snowolf.iteye.com/category/28519)
* [设计模式 (1)](http://snowolf.iteye.com/category/68028)
* [Scala (0)](http://snowolf.iteye.com/category/271868)
* [Kafka (1)](http://snowolf.iteye.com/category/274875)
### 社区版块

* [我的资讯](http://snowolf.iteye.com/blog/news) (0)
* [我的论坛](http://snowolf.iteye.com/blog/post) (66)
* [我的问答](http://snowolf.iteye.com/blog/answered_problems) (4)

### 存档分类

* [2013-05](http://snowolf.iteye.com/blog/monthblog/2013-05) (2)
* [2013-04](http://snowolf.iteye.com/blog/monthblog/2013-04) (1)
* [2013-03](http://snowolf.iteye.com/blog/monthblog/2013-03) (3)
* [更多存档...](http://snowolf.iteye.com/blog/monthblog_more)
### 评论排行榜

* [征服 Redis + Jedis + Spring （二）—— ...](http://snowolf.iteye.com/blog/1667104 "征服 Redis + Jedis + Spring （二）—— 哈希表操作（HMGET HMSET）")
* [MySQL 查询时强制区分大小写](http://snowolf.iteye.com/blog/1681944 "MySQL 查询时强制区分大小写")
* [MySQL 运维笔记（一）—— 终止高负载SQL](http://snowolf.iteye.com/blog/1679923 "MySQL 运维笔记（一）—— 终止高负载SQL")
* [征服 Redis + Jedis + Spring （一）—— ...](http://snowolf.iteye.com/blog/1666908 "征服 Redis + Jedis + Spring （一）—— 配置&常规操作（GET SET DEL）")
* [我的职场生涯（十）——又一个两年](http://snowolf.iteye.com/blog/1748094 "我的职场生涯（十）——又一个两年")

### 最新评论

* [dbh0512](http://dbh0512.iteye.com/ "dbh0512")： 这样操作只是在缓存中读取吗?项目中是不是只有在海量频繁读取某一 ...
[征服 Redis + Jedis + Spring （一）—— 配置&常规操作（GET SET DEL）](http://snowolf.iteye.com/blog/1666908#bc2322585)
* [愤怒的程序猿](http://jacy9.iteye.com/ "愤怒的程序猿")： LZ:keytool错误:java.lang.Exceptio ...
[Java加密技术（九）——初探SSL](http://snowolf.iteye.com/blog/397693#bc2322089)
* [snowolf](http://snowolf.iteye.com/ "snowolf")： juwend 写道如果被加密的明文使用String ming ...
[Java加密技术（四）——非对称加密算法RSA](http://snowolf.iteye.com/blog/381767#bc2321550)
* [simylau](http://simylau.iteye.com/ "simylau")： water_lang 写道@Autowired      pr ...
[Spring 注解学习手札（一） 构建简单Web应用](http://snowolf.iteye.com/blog/577989#bc2320738)
* [ifox](http://ifox.iteye.com/ "ifox")： phrmgb 写道学习了，这些东东有时感觉很偏的，但是特殊的业 ...
[Java关键字——transient](http://snowolf.iteye.com/blog/1329649#bc2319809)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2012 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![](http://stat.iteye.com/?url=http%3A%2F%2Fsnowolf.iteye.com%2Fblog%2F1677495&referrer=&user_id=) ![](http://dt.tongji.linezing.com/tongji.do?unit_id=1122445&uv_id=15753577192130187469&uv_new=0&cna=&cg=&mid=&mmland=&ade=&adtm=&sttm=&cpa=&ss_id=3627902245&ss_no=0&ec=1&ref=&url=http%3A//snowolf.iteye.com/blog/1677495&title=Memcached%u7B14%u8BB0%u2014%u2014%uFF08%u56DB%uFF09%u5E94%u5BF9%u9AD8%u5E76%u53D1%u653B%u51FB - Snowolf%u7684%u610F%u5883%u7A7A%u95F4%uFF01 - ITeye%u6280%u672F%u7F51%u7AD9&charset=utf-8&domain=iteye.com&hashval=909&filtered=0&app=Microsoft Internet Explorer&agent=Mozilla/5.0 %28compatible%3B MSIE 9.0%3B Windows NT 6.1%3B WOW64%3B Trident/5.0%3B SLCC2%3B .NET CLR 2.0.50727%3B .NET CLR 3.5.30729%3B .NET CLR 3.0.30729%3B Media Center PC 6.0%3B .NET4.0C%3B .NET4.0E%3B InfoPath.3%29&color=24-bit&screen=1600x900&lg=zh-cn&je=1&fv=10.0&st=1377062586&vc=eee3dea2&ut=2&url_id=0&cnu=0.01284455030611481)[![量子统计]()](http://tongji.alimama.com/report.html?unit_id=1122445)
