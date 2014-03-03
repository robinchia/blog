---
layout: post
title: "EhCache配置文件 - 奈何桥收费站。。。 - ITeye技术网站"
categories: 缓存
tags: 
 - 缓存
 - ehcache
--- 

# EhCache配置文件 - 奈何桥收费站。。。 - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼](http://zuzong.iteye.com/blog/1048714#)

[招聘](http://www.iteye.com/job) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://zuzong.iteye.com/login "登录") [登录](http://zuzong.iteye.com/login) [注册](http://zuzong.iteye.com/signup)

# [奈何桥收费站。。。](http://zuzong.iteye.com/)

* [**博客**](http://zuzong.iteye.com/)
* [微博](http://zuzong.iteye.com/weibo)
* [相册](http://zuzong.iteye.com/album)
* [收藏](http://zuzong.iteye.com/link)
* [留言](http://zuzong.iteye.com/blog/guest_book)
* [关于我](http://zuzong.iteye.com/blog/profile)

### [EhCache配置文件]() **

**博客分类：**
* [缓存](http://zuzong.iteye.com/category/157174)
[Cache](http://www.iteye.com/blogs/tag/Cache)[应用服务器](http://www.iteye.com/blogs/tag/%E5%BA%94%E7%94%A8%E6%9C%8D%E5%8A%A1%E5%99%A8)[Linux](http://www.iteye.com/blogs/tag/Linux)[嵌入式](http://www.iteye.com/blogs/tag/%E5%B5%8C%E5%85%A5%E5%BC%8F)[performance](http://www.iteye.com/blogs/tag/performance) 

Xml代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <?xml version="1.0" encoding="UTF-8"?>       
1. <!--  
1.   
1. CacheManager配置  
1. ==========================  
1.   
1.   
1. 一个ehcache.xml 相当于一个单个的CacheManager  
1.   
1. 学习下面的说明或者ehcache构架（ehcache.xsd）怎样配置  
1. 系统资源标识在这个文件中能被具体制定，当配置被加载时，他们将会被替换。例如 multicastGroupPort=${multicastGroupPort}被环境变量的系统属性替换，或是使用如-DmulticastGroupPort=4446命令行开关指定一个系统属性。  
1.   
1. <ehcache>的属性如下：  
1.   
1. * name – CacheManager的可选名称。这个名称起初主要是用于文档记录或辨别Terracotta集群状态。对于Terracotta集群的缓存，一组CacheManager名称和cache名称唯一的鉴定了一个特定的存储于Terracotta集群存储器的缓存。  
1.   
1. * updateCheck – 一个可选的boolean标识符，指定这个CacheManager是否通过Internet检查Ehcache的新版本。如果没有特别指明，updateCheck="true".  
1.   
1. * monitoring – 一个可选的设置，决定CacheManager是否应该自动的用系统MBean服务器注册SampledCacheMBean。当下，这个监测。只有当使用Terracotta集群和使用Terracotta Developer Console时才有用。使用"autodetect"值，Terracotta集群的出现将被检测和监视，并通过Developer控制台激活。其他允许的值有："on" 和 "off"。默认为"autodetect"。当使用JMX监测时，这个设置不会产生任何作用。  
1.   
1. * dynamicConfig – 一个可选设置，能够使与这个CacheManager相关联的动态配置失活。这个设置的默认值是true-例如，动态配置是激活的。动态配置的缓存通过缓存的配置对象让他们的TTI, TTL 和maximum disk 和in-memory capacity在运行时改变。  
1. -->    
1. <ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
1.          xsi:noNamespaceSchemaLocation="ehcache.xsd"  
1.          updateCheck="true" monitoring="autodetect"  
1.          dynamicConfig="true" >  
1.     
1.     <!--  
1.     DiskStore configuration  
1.     =======================  
1. 磁盘存储器是可选的。关闭磁盘存储路径创建，解释下面的磁盘存储器元素。  
1. 对于任何缓存，如果你已经激活了overflowToDisk或diskPersistent，就要配置磁盘存储器。  
1. 如果他未配置，并且创建了一个需要磁盘存储的缓存，将会发出一个警告并且java.io.tmpdir将会自动使用。  
1. 磁盘存储器仅有一个属性- "path"。这个路径就是.data and .index文件被创建的目录路径。  
1. 如果这个路径是下述Java系统属性之一，他将会被运行中的VM中的值替换。为了向后兼容，这些应该被特别规定，而不会被${token}替换语法封闭。  
1.   
1. 下列属性翻译：  
1. * user.home -用户的根目录  
1. * user.dir – 用户的当前工作目录  
1. * java.io.tmpdir – 默认临时文件路径  
1. * ehcache.disk.store.dir – 一个系统属性，你通常用命令行指定；  
1. 例如： java -Dehcache.disk.store.dir=/u01/myapp/diskdir ...  
1.    
1.    
1.   
1. 子目录通过如下属性指定，例如：java.io.tmpdir/one  
1.   -->  
1.     <diskStore path="java.io.tmpdir"/>  
1.    
1.     
1.     <!--  
1. TransactionManagerLookup configuration  
1. ======================================  
1. TransactionManagerLookup 配置  
1. 这个类被ehcache用XA激活的ehcache来查找用于应用程序中的JTA TransactionManager。如果没有指定类，那么DefaultTransactionManagerLookup将以如下顺序找到TransactionManager。  
1.   
1.    *GenericJNDI（例如：jboss，属性jndiName控制TransactionManager对象的名称来查找）  
1. *Websphere  
1.      *Bitronix  
1.      *Atomikos  
1. 你可以提供自己的查找类实现net.sf.ehcache.transaction.manager.TransactionManagerLookup接口。  
1.     <transactionManagerLookup class="net.sf.ehcache.transaction.manager.DefaultTransactionManagerLookup" properties="" propertySeparator=":"/>  
1.    
1.     <!--  
1.     CacheManagerEventListener  
1. =========================  
1. 指定一个CacheManagerEventListenerFactory，当缓存被增加或从CacheManager移除时被告知。  
1.   
1. CacheManagerEventListenerFactory的属性有：  
1. * class – 一个完全限定的工厂类名。  
1. * properties – 逗号隔开的属性只对工厂有意义。  
1.  overflowToDisk:设置元素是否能溢出磁盘，当存储器容量达到最大存储限制。  
1.   
1. 如下属性和元素是可选的。  
1.    
1.     timeToIdleSeconds:  
1. Sets the time to idle for an element before it expires.  
1. 设置一个元素在过期前的空闲时间  
1.     i.e. The maximum amount of time between accesses before an element expires  
1. Is only used if the element is not eternal.  
1. 换言之，最大时间数在进入之后和元素过期之前这段时间之间，只有元素在非持久化时才有用。  
1. Optional attribute. A value of 0 means that an Element can idle for infinity.  
1. 可选属性，0表示一个元素可以无限的空闲  
1. The default value is 0.  
1. 默认值是0。  
1.    
1.     timeToLiveSeconds:  
1.     Sets the time to live for an element before it expires.  
1.     i.e. The maximum time between creation time and when an element expires.  
1.     Is only used if the element is not eternal.  
1.     Optional attribute. A value of 0 means that and Element can live for infinity.  
1. The default value is 0.  
1. 在元素过期之前，设置一个存留时间。换言之，最大时间在创建时间和元素过期之间。仅用于元素非持久化。可选属性。值为0表示元素可无限存留，默认值是0  
1.    
1.     diskPersistent:  
1.     Whether the disk store persists between restarts of the Virtual Machine.  
1. The default value is false.  
1. 磁盘存储是否在虚拟机重启后持续存在。默认只是false。  
1.    
1.     diskExpiryThreadIntervalSeconds:  
1.     The number of seconds between runs of the disk expiry thread. The default value  
1. is 120 seconds.  
1. 秒数在运行和磁盘终止线程之间，默认值是120秒。  
1.    
1.     diskSpoolBufferSizeMB:  
1.     This is the size to allocate the DiskStore for a spool buffer. Writes are made  
1.     to this area and then asynchronously written to disk. The default size is 30MB.  
1.     Each spool buffer is used only by its cache. If you get OutOfMemory errors consider  
1.     lowering this value. To improve DiskStore performance consider increasing it. Trace level  
1. logging in the DiskStore will show if put back ups are occurring.  
1. 这是为后台打印缓冲分配在DiskStore的大小。在这一区域进行写入，并同步写入磁盘。默认值是30M。每个后台缓冲区仅由他的缓存使用，如出现OutOfMemory错误，考虑降低该值。为了提高DiskStore性能，考虑增加它。跟踪级别的DiskStore工作 将显示是否推迟出现。  
1.    
1.     clearOnFlush:  
1.     whether the MemoryStore should be cleared when flush() is called on the cache.  
1. By default, this is true i.e. the MemoryStore is cleared.  
1. 当flush()在缓存中被调用时，MemoryStore是否被清除。默认是true，即MemoryStore被清除。  
1.    
1.     memoryStoreEvictionPolicy:  
1.     Policy would be enforced upon reaching the maxElementsInMemory limit. Default  
1.     policy is Least Recently Used (specified as LRU). Other policies available -  
1.     First In First Out (specified as FIFO) and Less Frequently Used  
1.     (specified as LFU)  
1.    
1.     Cache elements can also contain sub elements which take the same format of a factory class  
1. and properties. Defined sub-elements are:  
1. 当达到maxElementsInMemory限制时，策略将强制执行。策略是最近最少使用的算法（简称为LRU）。其他的策略通用。缓存元素也可以有子元素，子元素拥有相同格式的工厂类和属性。定义的sub-elements有：  
1.    
1.     * cacheEventListenerFactory - Enables registration of listeners for cache events, such as  
1.       put, remove, update, and expire.  
1. * cacheEventListenerFactory – 启用缓存事件监听器的注册，如put, remove, update, and expire.  
1.    
1.     * bootstrapCacheLoaderFactory - Specifies a BootstrapCacheLoader, which is called by a  
1.       cache on initialisation to prepopulate itself.  
1.     * bootstrapCacheLoaderFactory – 指定一个BootstrapCacheLoader，它被一个缓存在初始化时调用，用来预填充自己。  
1.    
1.     * cacheExtensionFactory - Specifies a CacheExtension, a generic mechansim to tie a class  
1.       which holds a reference to a cache to the cache lifecycle.  
1. * cacheExtensionFactory – 指定一个CacheExtension，一个通用的mechansim来联系一个保存引用到缓存的类到缓存生命周期。  
1.    
1.     * cacheExceptionHandlerFactory - Specifies a CacheExceptionHandler, which is called when  
1.       cache exceptions occur.  
1.     * cacheExceptionHandlerFactory – 指定一个CacheExceptionHandler，每当缓存异常出现时调用。  
1.    
1.     * cacheLoaderFactory - Specifies a CacheLoader, which can be used both asynchronously and  
1.       synchronously to load objects into a cache. More than one cacheLoaderFactory element  
1.       can be added, in which case the loaders form a chain which are executed in order. If a  
1.       loader returns null, the next in chain is called.  
1. * cacheLoaderFactory – 指定一个CacheLoader，能够同步和异步装载对象到一个缓存。可以添加多个cacheLoaderFactory元素，在这种情况装载机形成一个链，被有序的执行。如果一个装载机返回null，下一个链就被调用。  
1.                                    
1. Cache Event Listeners  
1. 缓存事件监听器  
1.    
1.     All cacheEventListenerFactory elements can take an optional property listenFor that describes  
1.     which events will be delivered in a clustered environment.  The listenFor attribute has the  
1. following allowed values:  
1. 所有的cacheEventListenerFactory元素能选取一个可选属性listenFor描述的事件将在一个集群环境中交付。这个listenFor属性有如下允许的值：  
1.    
1.     * all - the default is to deliver all local and remote events  
1.     * local - deliver only events originating in the current node  
1. * remote - deliver only events originating in other nodes  
1. * all – 默认交付所有的本地和远程事件  
1. * local – 交付的只是源于当前节点的事件  
1. * remote - 交付的只是源于其他节点  
1.    
1. Example of setting up a logging listener for local cache events:  
1. 设置一个本地缓存事件监听器的例子：  
1.    
1.     <cacheEventListenerFactory class="my.company.log.CacheLogger"  
1.         listenFor="local" />  
1.    
1.    
1. Cache Exception Handling  
1. 缓存异常处理  
1.     ++++++++++++++++++++++++  
1.    
1.     By default, most cache operations will propagate a runtime CacheException on failure. An  
1.     interceptor, using a dynamic proxy, may be configured so that a CacheExceptionHandler can  
1. be configured to intercept Exceptions. Errors are not intercepted.  
1. 通常，大多数cache运行失败将产生运行时CacheException。通过使用代理，一个拦截器应该被配置，以便于能够配置CacheExceptionHandler拦截异常。错误并不被拦截。  
1.    
1. It is configured as per the following example:  
1. 按照下面的例子配置：  
1.    
1.       <cacheExceptionHandlerFactory class="com.example.ExampleExceptionHandlerFactory"  
1.                                       properties="logLevel=FINE"/>  
1.    
1.     Caches with ExceptionHandling configured are not of type Cache, but are of type Ehcache only,  
1. and are not available using CacheManager.getCache(), but using CacheManager.getEhcache().  
1. 有ExceptionHandling配置的缓存并不是典型的Cache，但却是典型的Ehcache，并且不能使用CacheManager.getCache()，但能够使用CacheManager.getEhcache()。  
1.    
1.    
1. Cache Loader  
1. 缓存装载  
1.     ++++++++++++  
1.      
1.     A default CacheLoader may be set which loads objects into the cache through asynchronous and  
1.     synchronous methods on Cache. This is different to the bootstrap cache loader, which is used  
1. only in distributed caching.  
1. 一个默认的CacheLoader应该被设置成这样，能够通过Cache中的同步和异步方法装载对象到缓存中。这和仅在分布是缓存中被用到的缓存装载引导程序是不同的。  
1.    
1. It is configured as per the following example:  
1. 按照如下示例配置：  
1.    
1.         <cacheLoaderFactory class="com.example.ExampleCacheLoaderFactory"  
1.                                       properties="type=int,startCounter=10"/>  
1.    
1.     XA Cache  
1.     ++++++++  
1.    
1. To enable an ehcache as a participant in the JTA Transaction, just have the following attribute  
1. 使ehcache作为JTA事务的参与者，只需要如下属性。  
1.     
1. transactionalMode="xa", otherwise the default is transactionalMode="off"  
1.     transactionalMode="xa", 否则，默认是 transactionalMode="off"  
1.    
1.    
1.     Cache Writer  
1.     ++++++++++++  
1.    
1.     A CacheWriter maybe be set to write to an underlying resource. Only one CacheWriter can be  
1. been to a cache.  
1. 一个CacheWriter可以设置写到底层资源中。只有一个CacheWriter能够成为一个cache。  
1.    
1. It is configured as per the following example for write-through:  
1. 按照如下示例配置write-through：  
1.    
1.         <cacheWriter writeMode="write-through" notifyListenersOnException="true">  
1.             <cacheWriterFactory class="net.sf.ehcache.writer.TestCacheWriterFactory"  
1.                                 properties="type=int,startCounter=10"/>  
1.         </cacheWriter>  
1.    
1. And it is configured as per the following example for write-behind:  
1. 按照如下示例配置write-behind:  
1.    
1.         <cacheWriter writeMode="write-behind" minWriteDelay="1" maxWriteDelay="5"  
1.                      rateLimitPerSecond="5" writeCoalescing="true" writeBatching="true" writeBatchSize="1"  
1.                      retryAttempts="2" retryAttemptDelaySeconds="1">  
1.             <cacheWriterFactory class="net.sf.ehcache.writer.TestCacheWriterFactory"  
1.                                 properties="type=int,startCounter=10"/>  
1.         </cacheWriter>  
1.    
1. The cacheWriter element has the following attributes:  
1. cacheWriter元素有如下属性：  
1. * writeMode: the write mode, write-through or write-behind  
1.    
1. These attributes only apply to write-through mode:  
1. 这些属性仅适用于write-through模式：  
1. * notifyListenersOnException: Sets whether to notify listeners when an exception occurs on a writer operation.  
1. * notifyListenersOnException:设置当一个写操作出现异常时是否告知监听器。  
1.    
1. These attributes only apply to write-behind mode:  
1. 这些属性仅适用于write-behind模式：  
1.    
1.     * minWriteDelay: Set the minimum number of seconds to wait before writing behind. If set to a value greater than 0,  
1.       it permits operations to build up in the queue. This is different from the maximum write delay in that by waiting  
1.       a minimum amount of time, work is always being built up. If the minimum write delay is set to zero and the  
1.       CacheWriter performs its work very quickly, the overhead of processing the write behind queue items becomes very  
1.       noticeable in a cluster since all the operations might be done for individual items instead of for a collection  
1.       of them.  
1. * minWriteDelay:设置write-behind之前的等待最小秒数。如果设置值比0大，则允许操作建立在队列中。和最大写入延迟不同，通过等待的最短时间，工作将同时被建立。如果最小写入延迟设置成0，并且CacheWriter快速执行程序，在一个集群中处理队列项目后的写入开销将会非常显著，因为所有的运行被单个项目完成，代替他们的一个集合。  
1.    
1.     * maxWriteDelay: Set the maximum number of seconds to wait before writing behind. If set to a value greater than 0,  
1.       it permits operations to build up in the queue to enable effective coalescing and batching optimisations.  
1. * maxWriteDelay:设置在后面写入之前等待的最大秒数。如设置值为0，它允许在队列里建立运行程序，以便有效地合并和批量优化。  
1.    
1.     * writeBatching: Sets whether to batch write operations. If set to true, writeAll and deleteAll will be called on  
1.       the CacheWriter rather than write and delete being called for each key. Resources such as databases can perform  
1.       more efficiently if updates are batched, thus reducing load.  
1. * writeBatching:设置是否批量写入操作。如果设为true，writeAll 和deleteAll将调用CacheWriter，而不是为每个键调用write和delete。如果更新是批量的，诸如数据库资源可以更高效的执行，因此减少了负荷。  
1.     * writeBatchSize: Sets the number of operations to include in each batch when writeBatching is enabled. If there are  
1.       less entries in the write-behind queue than the batch size, the queue length size is used.  
1. * writeBatchSize:当writeBatching处于激活时，设置每批包含的操作的数目。如果write-behind队列的实体数少于每批的数目，就使用队列的长度。  
1. * rateLimitPerSecond: Sets the maximum number of write operations to allow per second when writeBatching is enabled.  
1. * rateLimitPerSecond:当writeBatching激活时，设置写操作每秒允许的最大数目。  
1.     * writeCoalescing: Sets whether to use write coalescing. If set to true and multiple operations on the same key are  
1.       present in the write-behind queue, only the latest write is done, as the others are redundant.  
1. * writeCoalescing: 设置是否使用写入联合。如果设为true并且同样的键有多个操作出现在write-behind队列，只有最新的写入完成，因为其他的成了多余的。  
1.     * retryAttempts: Sets the number of times the operation is retried in the CacheWriter, this happens after the  
1.       original operation.  
1. * retryAttempts:设置CacheWriter中重复操作的总次数，这发生在初次操作之后。  
1. * retryAttemptDelaySeconds: Sets the number of seconds to wait before retrying an failed operation.  
1. * retryAttemptDelaySeconds:设置在失败操作重试之前等待的秒数。  
1.    
1.     Cache Extension  
1.     +++++++++++++++  
1.    
1.     CacheExtensions are a general purpose mechanism to allow generic extensions to a Cache.  
1.     CacheExtensions are tied into the Cache lifecycle.  
1. CacheExtensions是一个总的作用机制允许Cache有普通异常。CacheExtensions与Cache生命周期紧密相连。  
1.     CacheExtensions are created using the CacheExtensionFactory which has a  
1.     <code>createCacheCacheExtension()</code> method which takes as a parameter a  
1.     Cache and properties. It can thus call back into any public method on Cache, including, of  
1. course, the load methods.  
1. 创建CacheExtensions 来使用CacheExtensionFactory，他有一个<code>createCacheCacheExtension()</code>方法可以当做一个参数一个Cache和属性。因此CacheExtensions能够回调所有Cache中的公有方法，当然，包括装载方法。  
1.    
1. Extensions are added as per the following example:  
1. 按照如下示例增加Extensions：  
1.    
1.          <cacheExtensionFactory class="com.example.FileWatchingCacheRefresherExtensionFactory"  
1.                              properties="refreshIntervalMillis=18000, loaderTimeout=3000,  
1.                                          flushPeriod=whatever, someOtherProperty=someValue ..."/>  
1.    
1.     Terracotta Clustering  
1.     +++++++++++++++++++++  
1.     
1.     Cache elements can also contain information about whether the cache can be clustered with Terracotta.  
1. The <terracotta> sub-element has the following attributes:  
1. Cache元素也包含了有关是否缓存能和Terracotta聚集的信息。<terracotta>子元素有如下属性：  
1.    
1.     * clustered=true|false - indicates whether this cache should be clustered with Terracotta. By  
1.       default, if the <terracotta> element is included, clustered=true.  
1. * clustered=true|false – 显示这个cache是否应该和Terracotta聚集。如果包括<terracotta>元素，默认的是clustered=true。  
1.     * valueMode=serialization|identity - indicates whether this cache should be clustered with  
1.       serialized copies of the values or using Terracotta identity mode.  By default, values will  
1.       be cached in serialization mode which is similar to other replicated Ehcache modes.  The identity  
1.       mode is only available in certain Terracotta deployment scenarios and will maintain actual object  
1.       identity of the keys and values across the cluster.  In this case, all users of a value retrieved from  
1.       the cache are using the same clustered value and must provide appropriate locking for any changes  
1.       made to the value (or objects referred to by the value).  
1. * valueMode=serialization|identity – 指出是否这个cache和值的序列化拷贝聚合或者使用Terracotta鉴定模式。通常，值将会在序列化模式中缓存，这和其他的再生Ehcache模式相似。身份模式只有在某些Terracotta部署方案中有效，并且通过集群保持实际对象身份的键和值。在这种情况下，所有从缓存取值的用户都使用相同的集群值，并且必须对值（或者值所引用的对象）的任何改变提供了合适的锁定。  
1.     * synchronousWrites=true|false - When set to true, clustered caches use  
1.       Terracotta SYNCHRONOUS WRITE locks. Asynchronous writes (synchronousWrites="false") maximize performance by  
1.       allowing clients to proceed without waiting for a "transaction received" acknowledgement from the server.  
1.       Synchronous writes (synchronousWrites="true") maximize data safety by requiring that a client receive server  
1.       acknowledgement of a transaction before that client can proceed. If coherence mode is disabled using  
1.       configuration (coherent="false") or through the coherence API, only asynchronous writes can occur  
1.       (synchronousWrites="true" is ignored). By default this value is false (i.e. clustered caches use normal  
1.       Terracotta WRITE locks).  
1. * synchronousWrites=true|false – 当设为true时，缓存集群使用Terracotta SYNCHRONOUS WRITE锁。异步写入(synchronousWrites="false")最大的性能允许客户端继续而无需等待"transaction received"服务器的回应。同步写入(synchronousWrites="true")最大数据安全性，要求客户端再继续之前接收服务器端的事务响应。如果coherence模式不能使用配置(coherent="false")或者通过coherence API，仅异步写入能够存在(synchronousWrites="true"被忽略)。通常，这个值为false（例. 聚集缓存使用正常的Terracotta WRITE锁）。  
1.     * coherent=true|false - indicates whether this cache should have coherent reads and writes with guaranteed  
1.       consistency across the cluster.  By default, its value is true.  If this attribute is set to false  
1.       (or "incoherent" mode), values from the cache are read without locking, possibly yielding stale data.  
1.       Writes to a cache in incoherent mode are batched and applied without acquiring cluster-wide locks,  
1.       possibly creating inconsistent values across cluster. Incoherent mode is a performance optimization  
1.       with weaker concurrency guarantees and should generally be used for bulk-loading caches, for loading  
1.       a read-only cache, or where the application that can tolerate reading stale data. This setting overrides  
1.       coherentReads, which is deprecated.  
1. * coherent=true|false – 指出是否这个缓存应使读和写前后一致并通过集群确保其一致性。通常默认值是true。如这个属性被设为false（或"incoherent"模式），缓存里没有锁定直接读取的值，可能产生坏的数据。随着并发保证的减弱，Incoherent模式是最优化性能，应该用于bulk-loading缓存，用于装载一个只读缓存，或用于程序中能够容忍读取损坏数据的地方。这个设置重载了coherentReads，这是不赞成的。  
1.     * copyOnRead=true|false - indicates whether cache values are deserialized on every read or if the  
1.       materialized cache value can be re-used between get() calls. This setting is useful if a cache  
1.       is being shared by callers with disparate classloaders or to prevent local drift if keys/values  
1.       are mutated locally w/o putting back to the cache. NOTE: This setting is only relevant for caches  
1.       with valueMode=serialization  
1. * copyOnRead=true|false – 指出是否每个读出的缓存值是反序列化的，或者，是否具体的缓存值能够在get()调用之间被重用。这个设置很有用，如果一个缓存被不同的类装载器的调用者共享，或是阻止本地偏移如果键/值被组织本地w/o放回缓存。注意：这个设置仅对valueMode=serialization的缓存有意义。  
1.    
1.    
1. Simplest example to indicate clustering:  
1. 最简单的集群实例：  
1.         <terracotta/>  
1.         
1. To indicate the cache should not be clustered (or remove the <terracotta> element altogether):  
1. 指明缓存不被聚集（或一起移除<terracotta>元素）   
1.         <terracotta clustered="false"/>  
1.    
1. To indicate the cache should be clustered using identity mode:  
1. 表示使用模式聚集缓存：  
1.         <terracotta clustered="true" valueMode="identity"/>        
1.    
1. To indicate the cache should be clustered using incoherent mode for bulk load:  
1. 对大量的装载使用incoherent模式聚集缓存。  
1.         <terracotta clustered="true" coherent="false"/>  
1.    
1. To indicate the cache should be clustered using synchronous-write locking level:  
1. 使用synchronous-write锁定水平应该聚集缓存。  
1.         <terracotta clustered="true" synchronousWrites="true"/>  
1.     -->  
1.    
1.     <!--  
1.     Mandatory Default Cache configuration. These settings will be applied to caches  
1. created programmtically using CacheManager.add(String cacheName).  
1. 强制预设缓存配置。这个设置将应用于缓存创建CacheManager.add(String cacheName)。  
1.    
1. The defaultCache has an implicit name "default" which is a reserved cache name.  
1. defaultCache有一个内涵的名称“default”，是一个预设的缓存名称。  
1.     -->  
1.     <defaultCache  
1.            maxElementsInMemory="0"  
1.            eternal="false"  
1.            timeToIdleSeconds="1200"  
1.            timeToLiveSeconds="1200">  
1.       <terracotta/>  
1.     </defaultCache>  
1.    
1.     <!--  
1. Sample caches. Following are some example caches. Remove these before use.  
1. 缓存样本。以下是一些缓存实例，在使用前删掉这些。  
1.     -->  
1.    
1.     <!--  
1.     Sample cache named sampleCache1  
1.     This cache contains a maximum in memory of 10000 elements, and will expire  
1.     an element if it is idle for more than 5 minutes and lives for more than  
1. 10 minutes.  
1. 缓存实例名为sampleCache1，这个缓存的最大存储量为10000个元素，如果一个元素空闲时间超过5分钟就会失效并且生命周期超过10分钟。  
1.    
1.     If there are more than 10000 elements it will overflow to the  
1.     disk cache, which in this configuration will go to wherever java.io.tmp is  
1. defined on your system. On a standard Linux system this will be /tmp"  
1. 如果超过了10000个元素，磁盘缓存将会溢出，在这个缓存中，这个配置将找到你系统中任何定义java.io.tmp的地方。在标准的Linux系统中，这将会是/tmp"。  
1.    
1.    
1.     -->  
1.     <cache name="sampleCache1"  
1.            maxElementsInMemory="10000"  
1.            maxElementsOnDisk="1000"  
1.            eternal="false"  
1.            overflowToDisk="true"  
1.            diskSpoolBufferSizeMB="20"  
1.            timeToIdleSeconds="300"  
1.            timeToLiveSeconds="600"  
1.            memoryStoreEvictionPolicy="LFU"  
1.             />  
1.    
1.    
1.     <!--  
1. Sample cache named sampleCache2  
1. 实例缓存sampleCache2  
1.     This cache has a maximum of 1000 elements in memory. There is no overflow to disk, so 1000  
1.     is also the maximum cache size. Note that when a cache is eternal, timeToLive and  
1. timeToIdle are not used and do not need to be specified.  
1. 这个缓存的最大存储容量是1000个元素。没有磁盘溢出，因此，1000也是缓存的最大长度。要注意的是，当缓存持久化后, timeToLive和timeToIdle将不被使用，并且不需要特别指定。  
1.     -->  
1.     <cache name="sampleCache2"  
1.            maxElementsInMemory="1000"  
1.            eternal="true"  
1.            overflowToDisk="false"  
1.            memoryStoreEvictionPolicy="FIFO"  
1.             />  
1.    
1.    
1.     <!--  
1.     Sample cache named sampleCache3. This cache overflows to disk. The disk store is  
1.     persistent between cache and VM restarts. The disk expiry thread interval is set to 10  
1. minutes, overriding the default of 2 minutes.  
1. 示例sampleCache3。这个缓存溢出到磁盘。磁盘存储在缓存和VM重启的时候是持续的。磁盘期满间隔设置为10分钟，覆盖原来的2分钟。  
1.     -->  
1.     <cache name="sampleCache3"  
1.            maxElementsInMemory="500"  
1.            eternal="false"  
1.            overflowToDisk="true"  
1.            timeToIdleSeconds="300"  
1.            timeToLiveSeconds="600"  
1.            diskPersistent="true"  
1.            diskExpiryThreadIntervalSeconds="1"  
1.            memoryStoreEvictionPolicy="LFU"  
1.             />  
1.    
1.     <!--  
1.     Sample Terracotta clustered cache named sampleTerracottaCache.  
1. This cache uses Terracotta to cluster the contents of the cache.  
1. Terracotta集群缓存示例sampleTerracottaCache。这个缓存使用Terracotta聚集缓存内容。  
1.     -->  
1.     <cache name="sampleTerracottaCache"  
1.            maxElementsInMemory="1000"  
1.            eternal="false"  
1.            timeToIdleSeconds="3600"  
1.            timeToLiveSeconds="1800"  
1.            overflowToDisk="false">  
1.    
1.         <terracotta/>  
1.     </cache>  
1.    
1.       <!--  
1.       Sample xa enabled cache name xaCache  
1. Xa激活缓存示例xaCache  
1.     -->  
1.    
1.     <cache name="xaCache"  
1.         maxElementsInMemory="500"  
1.         eternal="false"  
1.         timeToIdleSeconds="300"  
1.         timeToLiveSeconds="600"  
1.         overflowToDisk="false"  
1.         diskPersistent="false"  
1.         diskExpiryThreadIntervalSeconds="1"  
1.         transactionalMode="xa">  
1.       <terracotta clustered="true"/>  
1.   </cache>  
1.    
1.    
1. </ehcache>  
1.    
1.    
1.   
1. 设置完全限定类名被注册为CacheManager事件监听器。  
1.   
1. 事件包括：  
1.     * adding a Cache增加一个缓存  
1.     * removing a Cache移除一个缓存  
1. 设置元素是否持久化，如果持久化，将忽视超时并且元素永不过期。  
1. 回调监听器的方法有同步和异步两种。安全的处理潜在的麻烦和线程安全问题将是实施者的责任，这依取决于他们的监听器在干什么。  
1.   
1. 如果没有类指定，就不会创建监听器。这里没有默认值。  
1.     <cacheManagerEventListenerFactory class="" properties=""/>  
1.    
1.     <!--  
1.     TerracottaConfig  
1.     ========================  
1.  maxElementsOnDisk:  
1.   
1. 设置磁盘存储器维持的对象的最大数目。默认是0，意味着没有限制。  
1. eternal:  
1. 激活Terracotta集群选项)  
1.   
1. 注意：你需要安装运行一个或多个Terracotta服务器来使用Terracotta集群。  
1. 参看http://www.terracotta.org/web/display/orgsite/Download  
1.    
1.   
1. 使用多个Terracotta服务器实例URLs（容错能力）的例子  
1. <terracottaConfig url="host1:9510,host2:9510,host3:9510"/>  
1. maxElementsInMemory:  
1.   
1. 设置创建到存储器中的对象的最大数目。  
1. 在ehcache配置文件中嵌入一个Terracotta配置文件简单的放置一个普通的Terracotta XML配置到<terracottaConfig>元素中。  
1.     
1.     Example:  
1.     <terracottaConfig>  
1.         <tc-config>  
1.             <servers>  
1.                 <server host="server1" name="s1"/>  
1.                 <server host="server2" name="s2"/>  
1.             </servers>  
1.             <clients>  
1.                 <logs>app/logs-%i</logs>  
1.             </clients>           
1.         </tc-config>  
1.     </terracottaConfig>  
1. 更多的Terracotta信息，参看Terracotta文档。  
1.     -->  
1.     <terracottaConfig url="localhost:9510"/>  
1.    
1.     <!--  
1.     Cache configuration  
1.     ===================  
1.    
1. The following attributes are required.  
1. 如下属性都需要：  
1.  name:  
1.   
1. 设置缓存的名称。用于鉴定缓存，他必须是唯一的。  
1.   
1. 指定一个TerracottaConfig用于为CacheManager配置Terracotta运行时。  
1.   
1. 配置文件可以通过两种主要方式指定：通过引用配置文件或者使用Terracotta嵌入式配置文件。  
1.   
1. 使用URL属性指定一个配置资源（或者多个）的引用。URL属性必须包含一个逗号隔开的列表：  
1. * path（Terracotta配置文件的路径）(通常命名为 tc-config.xml)  
1.     * URL Terracotta配置文件的URL  
1. * <server host>:<port> Terracotta服务器运行实例  
1. 最简单的例子指出这台机器上的一个Terracotta服务器：  
1. <terracottaConfig url="localhost:9510"/>  
1. 使用Terracotta配置文件路径的例子：  
1. <terracottaConfig url="/app/config/tc-config.xml"/>  
1. 使用Terracotta配置文件URL的例子：  
1. <terracottaConfig url="http://internal/ehcache/app/tc-config.xml"/>  
<?xml version="1.0" encoding="UTF-8"?> <!-- CacheManager配置 ========================== 一个ehcache.xml 相当于一个单个的CacheManager 学习下面的说明或者ehcache构架（ehcache.xsd）怎样配置 系统资源标识在这个文件中能被具体制定，当配置被加载时，他们将会被替换。例如 multicastGroupPort=${multicastGroupPort}被环境变量的系统属性替换，或是使用如-DmulticastGroupPort=4446命令行开关指定一个系统属性。 <ehcache>的属性如下： * name – CacheManager的可选名称。这个名称起初主要是用于文档记录或辨别Terracotta集群状态。对于Terracotta集群的缓存，一组CacheManager名称和cache名称唯一的鉴定了一个特定的存储于Terracotta集群存储器的缓存。 * updateCheck – 一个可选的boolean标识符，指定这个CacheManager是否通过Internet检查Ehcache的新版本。如果没有特别指明，updateCheck="true". * monitoring – 一个可选的设置，决定CacheManager是否应该自动的用系统MBean服务器注册SampledCacheMBean。当下，这个监测。只有当使用Terracotta集群和使用Terracotta Developer Console时才有用。使用"autodetect"值，Terracotta集群的出现将被检测和监视，并通过Developer控制台激活。其他允许的值有："on" 和 "off"。默认为"autodetect"。当使用JMX监测时，这个设置不会产生任何作用。 * dynamicConfig – 一个可选设置，能够使与这个CacheManager相关联的动态配置失活。这个设置的默认值是true-例如，动态配置是激活的。动态配置的缓存通过缓存的配置对象让他们的TTI, TTL 和maximum disk 和in-memory capacity在运行时改变。 --> <ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="ehcache.xsd" updateCheck="true" monitoring="autodetect" dynamicConfig="true" > <!-- DiskStore configuration ======================= 磁盘存储器是可选的。关闭磁盘存储路径创建，解释下面的磁盘存储器元素。 对于任何缓存，如果你已经激活了overflowToDisk或diskPersistent，就要配置磁盘存储器。 如果他未配置，并且创建了一个需要磁盘存储的缓存，将会发出一个警告并且java.io.tmpdir将会自动使用。 磁盘存储器仅有一个属性- "path"。这个路径就是.data and .index文件被创建的目录路径。 如果这个路径是下述Java系统属性之一，他将会被运行中的VM中的值替换。为了向后兼容，这些应该被特别规定，而不会被${token}替换语法封闭。 下列属性翻译： * user.home -用户的根目录 * user.dir – 用户的当前工作目录 * java.io.tmpdir – 默认临时文件路径 * ehcache.disk.store.dir – 一个系统属性，你通常用命令行指定； 例如： java -Dehcache.disk.store.dir=/u01/myapp/diskdir ... 子目录通过如下属性指定，例如：java.io.tmpdir/one --> <diskStore path="java.io.tmpdir"/> <!-- TransactionManagerLookup configuration ====================================== TransactionManagerLookup 配置 这个类被ehcache用XA激活的ehcache来查找用于应用程序中的JTA TransactionManager。如果没有指定类，那么DefaultTransactionManagerLookup将以如下顺序找到TransactionManager。 *GenericJNDI（例如：jboss，属性jndiName控制TransactionManager对象的名称来查找） *Websphere *Bitronix *Atomikos 你可以提供自己的查找类实现net.sf.ehcache.transaction.manager.TransactionManagerLookup接口。 <transactionManagerLookup class="net.sf.ehcache.transaction.manager.DefaultTransactionManagerLookup" properties="" propertySeparator=":"/> <!-- CacheManagerEventListener ========================= 指定一个CacheManagerEventListenerFactory，当缓存被增加或从CacheManager移除时被告知。 CacheManagerEventListenerFactory的属性有： * class – 一个完全限定的工厂类名。 * properties – 逗号隔开的属性只对工厂有意义。 overflowToDisk:设置元素是否能溢出磁盘，当存储器容量达到最大存储限制。 如下属性和元素是可选的。 timeToIdleSeconds: Sets the time to idle for an element before it expires. 设置一个元素在过期前的空闲时间 i.e. The maximum amount of time between accesses before an element expires Is only used if the element is not eternal. 换言之，最大时间数在进入之后和元素过期之前这段时间之间，只有元素在非持久化时才有用。 Optional attribute. A value of 0 means that an Element can idle for infinity. 可选属性，0表示一个元素可以无限的空闲 The default value is 0. 默认值是0。 timeToLiveSeconds: Sets the time to live for an element before it expires. i.e. The maximum time between creation time and when an element expires. Is only used if the element is not eternal. Optional attribute. A value of 0 means that and Element can live for infinity. The default value is 0. 在元素过期之前，设置一个存留时间。换言之，最大时间在创建时间和元素过期之间。仅用于元素非持久化。可选属性。值为0表示元素可无限存留，默认值是0 diskPersistent: Whether the disk store persists between restarts of the Virtual Machine. The default value is false. 磁盘存储是否在虚拟机重启后持续存在。默认只是false。 diskExpiryThreadIntervalSeconds: The number of seconds between runs of the disk expiry thread. The default value is 120 seconds. 秒数在运行和磁盘终止线程之间，默认值是120秒。 diskSpoolBufferSizeMB: This is the size to allocate the DiskStore for a spool buffer. Writes are made to this area and then asynchronously written to disk. The default size is 30MB. Each spool buffer is used only by its cache. If you get OutOfMemory errors consider lowering this value. To improve DiskStore performance consider increasing it. Trace level logging in the DiskStore will show if put back ups are occurring. 这是为后台打印缓冲分配在DiskStore的大小。在这一区域进行写入，并同步写入磁盘。默认值是30M。每个后台缓冲区仅由他的缓存使用，如出现OutOfMemory错误，考虑降低该值。为了提高DiskStore性能，考虑增加它。跟踪级别的DiskStore工作 将显示是否推迟出现。 clearOnFlush: whether the MemoryStore should be cleared when flush() is called on the cache. By default, this is true i.e. the MemoryStore is cleared. 当flush()在缓存中被调用时，MemoryStore是否被清除。默认是true，即MemoryStore被清除。 memoryStoreEvictionPolicy: Policy would be enforced upon reaching the maxElementsInMemory limit. Default policy is Least Recently Used (specified as LRU). Other policies available - First In First Out (specified as FIFO) and Less Frequently Used (specified as LFU) Cache elements can also contain sub elements which take the same format of a factory class and properties. Defined sub-elements are: 当达到maxElementsInMemory限制时，策略将强制执行。策略是最近最少使用的算法（简称为LRU）。其他的策略通用。缓存元素也可以有子元素，子元素拥有相同格式的工厂类和属性。定义的sub-elements有： * cacheEventListenerFactory - Enables registration of listeners for cache events, such as put, remove, update, and expire. * cacheEventListenerFactory – 启用缓存事件监听器的注册，如put, remove, update, and expire. * bootstrapCacheLoaderFactory - Specifies a BootstrapCacheLoader, which is called by a cache on initialisation to prepopulate itself. * bootstrapCacheLoaderFactory – 指定一个BootstrapCacheLoader，它被一个缓存在初始化时调用，用来预填充自己。 * cacheExtensionFactory - Specifies a CacheExtension, a generic mechansim to tie a class which holds a reference to a cache to the cache lifecycle. * cacheExtensionFactory – 指定一个CacheExtension，一个通用的mechansim来联系一个保存引用到缓存的类到缓存生命周期。 * cacheExceptionHandlerFactory - Specifies a CacheExceptionHandler, which is called when cache exceptions occur. * cacheExceptionHandlerFactory – 指定一个CacheExceptionHandler，每当缓存异常出现时调用。 * cacheLoaderFactory - Specifies a CacheLoader, which can be used both asynchronously and synchronously to load objects into a cache. More than one cacheLoaderFactory element can be added, in which case the loaders form a chain which are executed in order. If a loader returns null, the next in chain is called. * cacheLoaderFactory – 指定一个CacheLoader，能够同步和异步装载对象到一个缓存。可以添加多个cacheLoaderFactory元素，在这种情况装载机形成一个链，被有序的执行。如果一个装载机返回null，下一个链就被调用。 Cache Event Listeners 缓存事件监听器 All cacheEventListenerFactory elements can take an optional property listenFor that describes which events will be delivered in a clustered environment. The listenFor attribute has the following allowed values: 所有的cacheEventListenerFactory元素能选取一个可选属性listenFor描述的事件将在一个集群环境中交付。这个listenFor属性有如下允许的值： * all - the default is to deliver all local and remote events * local - deliver only events originating in the current node * remote - deliver only events originating in other nodes * all – 默认交付所有的本地和远程事件 * local – 交付的只是源于当前节点的事件 * remote - 交付的只是源于其他节点 Example of setting up a logging listener for local cache events: 设置一个本地缓存事件监听器的例子： <cacheEventListenerFactory class="my.company.log.CacheLogger" listenFor="local" /> Cache Exception Handling 缓存异常处理 ++++++++++++++++++++++++ By default, most cache operations will propagate a runtime CacheException on failure. An interceptor, using a dynamic proxy, may be configured so that a CacheExceptionHandler can be configured to intercept Exceptions. Errors are not intercepted. 通常，大多数cache运行失败将产生运行时CacheException。通过使用代理，一个拦截器应该被配置，以便于能够配置CacheExceptionHandler拦截异常。错误并不被拦截。 It is configured as per the following example: 按照下面的例子配置： <cacheExceptionHandlerFactory class="com.example.ExampleExceptionHandlerFactory" properties="logLevel=FINE"/> Caches with ExceptionHandling configured are not of type Cache, but are of type Ehcache only, and are not available using CacheManager.getCache(), but using CacheManager.getEhcache(). 有ExceptionHandling配置的缓存并不是典型的Cache，但却是典型的Ehcache，并且不能使用CacheManager.getCache()，但能够使用CacheManager.getEhcache()。 Cache Loader 缓存装载 ++++++++++++ A default CacheLoader may be set which loads objects into the cache through asynchronous and synchronous methods on Cache. This is different to the bootstrap cache loader, which is used only in distributed caching. 一个默认的CacheLoader应该被设置成这样，能够通过Cache中的同步和异步方法装载对象到缓存中。这和仅在分布是缓存中被用到的缓存装载引导程序是不同的。 It is configured as per the following example: 按照如下示例配置： <cacheLoaderFactory class="com.example.ExampleCacheLoaderFactory" properties="type=int,startCounter=10"/> XA Cache ++++++++ To enable an ehcache as a participant in the JTA Transaction, just have the following attribute 使ehcache作为JTA事务的参与者，只需要如下属性。 transactionalMode="xa", otherwise the default is transactionalMode="off" transactionalMode="xa", 否则，默认是 transactionalMode="off" Cache Writer ++++++++++++ A CacheWriter maybe be set to write to an underlying resource. Only one CacheWriter can be been to a cache. 一个CacheWriter可以设置写到底层资源中。只有一个CacheWriter能够成为一个cache。 It is configured as per the following example for write-through: 按照如下示例配置write-through： <cacheWriter writeMode="write-through" notifyListenersOnException="true"> <cacheWriterFactory class="net.sf.ehcache.writer.TestCacheWriterFactory" properties="type=int,startCounter=10"/> </cacheWriter> And it is configured as per the following example for write-behind: 按照如下示例配置write-behind: <cacheWriter writeMode="write-behind" minWriteDelay="1" maxWriteDelay="5" rateLimitPerSecond="5" writeCoalescing="true" writeBatching="true" writeBatchSize="1" retryAttempts="2" retryAttemptDelaySeconds="1"> <cacheWriterFactory class="net.sf.ehcache.writer.TestCacheWriterFactory" properties="type=int,startCounter=10"/> </cacheWriter> The cacheWriter element has the following attributes: cacheWriter元素有如下属性： * writeMode: the write mode, write-through or write-behind These attributes only apply to write-through mode: 这些属性仅适用于write-through模式： * notifyListenersOnException: Sets whether to notify listeners when an exception occurs on a writer operation. * notifyListenersOnException:设置当一个写操作出现异常时是否告知监听器。 These attributes only apply to write-behind mode: 这些属性仅适用于write-behind模式： * minWriteDelay: Set the minimum number of seconds to wait before writing behind. If set to a value greater than 0, it permits operations to build up in the queue. This is different from the maximum write delay in that by waiting a minimum amount of time, work is always being built up. If the minimum write delay is set to zero and the CacheWriter performs its work very quickly, the overhead of processing the write behind queue items becomes very noticeable in a cluster since all the operations might be done for individual items instead of for a collection of them. * minWriteDelay:设置write-behind之前的等待最小秒数。如果设置值比0大，则允许操作建立在队列中。和最大写入延迟不同，通过等待的最短时间，工作将同时被建立。如果最小写入延迟设置成0，并且CacheWriter快速执行程序，在一个集群中处理队列项目后的写入开销将会非常显著，因为所有的运行被单个项目完成，代替他们的一个集合。 * maxWriteDelay: Set the maximum number of seconds to wait before writing behind. If set to a value greater than 0, it permits operations to build up in the queue to enable effective coalescing and batching optimisations. * maxWriteDelay:设置在后面写入之前等待的最大秒数。如设置值为0，它允许在队列里建立运行程序，以便有效地合并和批量优化。 * writeBatching: Sets whether to batch write operations. If set to true, writeAll and deleteAll will be called on the CacheWriter rather than write and delete being called for each key. Resources such as databases can perform more efficiently if updates are batched, thus reducing load. * writeBatching:设置是否批量写入操作。如果设为true，writeAll 和deleteAll将调用CacheWriter，而不是为每个键调用write和delete。如果更新是批量的，诸如数据库资源可以更高效的执行，因此减少了负荷。 * writeBatchSize: Sets the number of operations to include in each batch when writeBatching is enabled. If there are less entries in the write-behind queue than the batch size, the queue length size is used. * writeBatchSize:当writeBatching处于激活时，设置每批包含的操作的数目。如果write-behind队列的实体数少于每批的数目，就使用队列的长度。 * rateLimitPerSecond: Sets the maximum number of write operations to allow per second when writeBatching is enabled. * rateLimitPerSecond:当writeBatching激活时，设置写操作每秒允许的最大数目。 * writeCoalescing: Sets whether to use write coalescing. If set to true and multiple operations on the same key are present in the write-behind queue, only the latest write is done, as the others are redundant. * writeCoalescing: 设置是否使用写入联合。如果设为true并且同样的键有多个操作出现在write-behind队列，只有最新的写入完成，因为其他的成了多余的。 * retryAttempts: Sets the number of times the operation is retried in the CacheWriter, this happens after the original operation. * retryAttempts:设置CacheWriter中重复操作的总次数，这发生在初次操作之后。 * retryAttemptDelaySeconds: Sets the number of seconds to wait before retrying an failed operation. * retryAttemptDelaySeconds:设置在失败操作重试之前等待的秒数。 Cache Extension +++++++++++++++ CacheExtensions are a general purpose mechanism to allow generic extensions to a Cache. CacheExtensions are tied into the Cache lifecycle. CacheExtensions是一个总的作用机制允许Cache有普通异常。CacheExtensions与Cache生命周期紧密相连。 CacheExtensions are created using the CacheExtensionFactory which has a <code>createCacheCacheExtension()</code> method which takes as a parameter a Cache and properties. It can thus call back into any public method on Cache, including, of course, the load methods. 创建CacheExtensions 来使用CacheExtensionFactory，他有一个<code>createCacheCacheExtension()</code>方法可以当做一个参数一个Cache和属性。因此CacheExtensions能够回调所有Cache中的公有方法，当然，包括装载方法。 Extensions are added as per the following example: 按照如下示例增加Extensions： <cacheExtensionFactory class="com.example.FileWatchingCacheRefresherExtensionFactory" properties="refreshIntervalMillis=18000, loaderTimeout=3000, flushPeriod=whatever, someOtherProperty=someValue ..."/> Terracotta Clustering +++++++++++++++++++++ Cache elements can also contain information about whether the cache can be clustered with Terracotta. The <terracotta> sub-element has the following attributes: Cache元素也包含了有关是否缓存能和Terracotta聚集的信息。<terracotta>子元素有如下属性： * clustered=true|false - indicates whether this cache should be clustered with Terracotta. By default, if the <terracotta> element is included, clustered=true. * clustered=true|false – 显示这个cache是否应该和Terracotta聚集。如果包括<terracotta>元素，默认的是clustered=true。 * valueMode=serialization|identity - indicates whether this cache should be clustered with serialized copies of the values or using Terracotta identity mode. By default, values will be cached in serialization mode which is similar to other replicated Ehcache modes. The identity mode is only available in certain Terracotta deployment scenarios and will maintain actual object identity of the keys and values across the cluster. In this case, all users of a value retrieved from the cache are using the same clustered value and must provide appropriate locking for any changes made to the value (or objects referred to by the value). * valueMode=serialization|identity – 指出是否这个cache和值的序列化拷贝聚合或者使用Terracotta鉴定模式。通常，值将会在序列化模式中缓存，这和其他的再生Ehcache模式相似。身份模式只有在某些Terracotta部署方案中有效，并且通过集群保持实际对象身份的键和值。在这种情况下，所有从缓存取值的用户都使用相同的集群值，并且必须对值（或者值所引用的对象）的任何改变提供了合适的锁定。 * synchronousWrites=true|false - When set to true, clustered caches use Terracotta SYNCHRONOUS WRITE locks. Asynchronous writes (synchronousWrites="false") maximize performance by allowing clients to proceed without waiting for a "transaction received" acknowledgement from the server. Synchronous writes (synchronousWrites="true") maximize data safety by requiring that a client receive server acknowledgement of a transaction before that client can proceed. If coherence mode is disabled using configuration (coherent="false") or through the coherence API, only asynchronous writes can occur (synchronousWrites="true" is ignored). By default this value is false (i.e. clustered caches use normal Terracotta WRITE locks). * synchronousWrites=true|false – 当设为true时，缓存集群使用Terracotta SYNCHRONOUS WRITE锁。异步写入(synchronousWrites="false")最大的性能允许客户端继续而无需等待"transaction received"服务器的回应。同步写入(synchronousWrites="true")最大数据安全性，要求客户端再继续之前接收服务器端的事务响应。如果coherence模式不能使用配置(coherent="false")或者通过coherence API，仅异步写入能够存在(synchronousWrites="true"被忽略)。通常，这个值为false（例. 聚集缓存使用正常的Terracotta WRITE锁）。 * coherent=true|false - indicates whether this cache should have coherent reads and writes with guaranteed consistency across the cluster. By default, its value is true. If this attribute is set to false (or "incoherent" mode), values from the cache are read without locking, possibly yielding stale data. Writes to a cache in incoherent mode are batched and applied without acquiring cluster-wide locks, possibly creating inconsistent values across cluster. Incoherent mode is a performance optimization with weaker concurrency guarantees and should generally be used for bulk-loading caches, for loading a read-only cache, or where the application that can tolerate reading stale data. This setting overrides coherentReads, which is deprecated. * coherent=true|false – 指出是否这个缓存应使读和写前后一致并通过集群确保其一致性。通常默认值是true。如这个属性被设为false（或"incoherent"模式），缓存里没有锁定直接读取的值，可能产生坏的数据。随着并发保证的减弱，Incoherent模式是最优化性能，应该用于bulk-loading缓存，用于装载一个只读缓存，或用于程序中能够容忍读取损坏数据的地方。这个设置重载了coherentReads，这是不赞成的。 * copyOnRead=true|false - indicates whether cache values are deserialized on every read or if the materialized cache value can be re-used between get() calls. This setting is useful if a cache is being shared by callers with disparate classloaders or to prevent local drift if keys/values are mutated locally w/o putting back to the cache. NOTE: This setting is only relevant for caches with valueMode=serialization * copyOnRead=true|false – 指出是否每个读出的缓存值是反序列化的，或者，是否具体的缓存值能够在get()调用之间被重用。这个设置很有用，如果一个缓存被不同的类装载器的调用者共享，或是阻止本地偏移如果键/值被组织本地w/o放回缓存。注意：这个设置仅对valueMode=serialization的缓存有意义。 Simplest example to indicate clustering: 最简单的集群实例： <terracotta/> To indicate the cache should not be clustered (or remove the <terracotta> element altogether): 指明缓存不被聚集（或一起移除<terracotta>元素） <terracotta clustered="false"/> To indicate the cache should be clustered using identity mode: 表示使用模式聚集缓存： <terracotta clustered="true" valueMode="identity"/> To indicate the cache should be clustered using incoherent mode for bulk load: 对大量的装载使用incoherent模式聚集缓存。 <terracotta clustered="true" coherent="false"/> To indicate the cache should be clustered using synchronous-write locking level: 使用synchronous-write锁定水平应该聚集缓存。 <terracotta clustered="true" synchronousWrites="true"/> --> <!-- Mandatory Default Cache configuration. These settings will be applied to caches created programmtically using CacheManager.add(String cacheName). 强制预设缓存配置。这个设置将应用于缓存创建CacheManager.add(String cacheName)。 The defaultCache has an implicit name "default" which is a reserved cache name. defaultCache有一个内涵的名称“default”，是一个预设的缓存名称。 --> <defaultCache maxElementsInMemory="0" eternal="false" timeToIdleSeconds="1200" timeToLiveSeconds="1200"> <terracotta/> </defaultCache> <!-- Sample caches. Following are some example caches. Remove these before use. 缓存样本。以下是一些缓存实例，在使用前删掉这些。 --> <!-- Sample cache named sampleCache1 This cache contains a maximum in memory of 10000 elements, and will expire an element if it is idle for more than 5 minutes and lives for more than 10 minutes. 缓存实例名为sampleCache1，这个缓存的最大存储量为10000个元素，如果一个元素空闲时间超过5分钟就会失效并且生命周期超过10分钟。 If there are more than 10000 elements it will overflow to the disk cache, which in this configuration will go to wherever java.io.tmp is defined on your system. On a standard Linux system this will be /tmp" 如果超过了10000个元素，磁盘缓存将会溢出，在这个缓存中，这个配置将找到你系统中任何定义java.io.tmp的地方。在标准的Linux系统中，这将会是/tmp"。 --> <cache name="sampleCache1" maxElementsInMemory="10000" maxElementsOnDisk="1000" eternal="false" overflowToDisk="true" diskSpoolBufferSizeMB="20" timeToIdleSeconds="300" timeToLiveSeconds="600" memoryStoreEvictionPolicy="LFU" /> <!-- Sample cache named sampleCache2 实例缓存sampleCache2 This cache has a maximum of 1000 elements in memory. There is no overflow to disk, so 1000 is also the maximum cache size. Note that when a cache is eternal, timeToLive and timeToIdle are not used and do not need to be specified. 这个缓存的最大存储容量是1000个元素。没有磁盘溢出，因此，1000也是缓存的最大长度。要注意的是，当缓存持久化后, timeToLive和timeToIdle将不被使用，并且不需要特别指定。 --> <cache name="sampleCache2" maxElementsInMemory="1000" eternal="true" overflowToDisk="false" memoryStoreEvictionPolicy="FIFO" /> <!-- Sample cache named sampleCache3. This cache overflows to disk. The disk store is persistent between cache and VM restarts. The disk expiry thread interval is set to 10 minutes, overriding the default of 2 minutes. 示例sampleCache3。这个缓存溢出到磁盘。磁盘存储在缓存和VM重启的时候是持续的。磁盘期满间隔设置为10分钟，覆盖原来的2分钟。 --> <cache name="sampleCache3" maxElementsInMemory="500" eternal="false" overflowToDisk="true" timeToIdleSeconds="300" timeToLiveSeconds="600" diskPersistent="true" diskExpiryThreadIntervalSeconds="1" memoryStoreEvictionPolicy="LFU" /> <!-- Sample Terracotta clustered cache named sampleTerracottaCache. This cache uses Terracotta to cluster the contents of the cache. Terracotta集群缓存示例sampleTerracottaCache。这个缓存使用Terracotta聚集缓存内容。 --> <cache name="sampleTerracottaCache" maxElementsInMemory="1000" eternal="false" timeToIdleSeconds="3600" timeToLiveSeconds="1800" overflowToDisk="false"> <terracotta/> </cache> <!-- Sample xa enabled cache name xaCache Xa激活缓存示例xaCache --> <cache name="xaCache" maxElementsInMemory="500" eternal="false" timeToIdleSeconds="300" timeToLiveSeconds="600" overflowToDisk="false" diskPersistent="false" diskExpiryThreadIntervalSeconds="1" transactionalMode="xa"> <terracotta clustered="true"/> </cache> </ehcache> 设置完全限定类名被注册为CacheManager事件监听器。 事件包括： * adding a Cache增加一个缓存 * removing a Cache移除一个缓存 设置元素是否持久化，如果持久化，将忽视超时并且元素永不过期。 回调监听器的方法有同步和异步两种。安全的处理潜在的麻烦和线程安全问题将是实施者的责任，这依取决于他们的监听器在干什么。 如果没有类指定，就不会创建监听器。这里没有默认值。 <cacheManagerEventListenerFactory class="" properties=""/> <!-- TerracottaConfig ======================== maxElementsOnDisk: 设置磁盘存储器维持的对象的最大数目。默认是0，意味着没有限制。 eternal: 激活Terracotta集群选项) 注意：你需要安装运行一个或多个Terracotta服务器来使用Terracotta集群。 参看http://www.terracotta.org/web/display/orgsite/Download 使用多个Terracotta服务器实例URLs（容错能力）的例子 <terracottaConfig url="host1:9510,host2:9510,host3:9510"/> maxElementsInMemory: 设置创建到存储器中的对象的最大数目。 在ehcache配置文件中嵌入一个Terracotta配置文件简单的放置一个普通的Terracotta XML配置到<terracottaConfig>元素中。 Example: <terracottaConfig> <tc-config> <servers> <server host="server1" name="s1"/> <server host="server2" name="s2"/> </servers> <clients> <logs>app/logs-%i</logs> </clients> </tc-config> </terracottaConfig> 更多的Terracotta信息，参看Terracotta文档。 --> <terracottaConfig url="localhost:9510"/> <!-- Cache configuration =================== The following attributes are required. 如下属性都需要： name: 设置缓存的名称。用于鉴定缓存，他必须是唯一的。 指定一个TerracottaConfig用于为CacheManager配置Terracotta运行时。 配置文件可以通过两种主要方式指定：通过引用配置文件或者使用Terracotta嵌入式配置文件。 使用URL属性指定一个配置资源（或者多个）的引用。URL属性必须包含一个逗号隔开的列表： * path（Terracotta配置文件的路径）(通常命名为 tc-config.xml) * URL Terracotta配置文件的URL * <server host>:<port> Terracotta服务器运行实例 最简单的例子指出这台机器上的一个Terracotta服务器： <terracottaConfig url="localhost:9510"/> 使用Terracotta配置文件路径的例子： <terracottaConfig url="/app/config/tc-config.xml"/> 使用Terracotta配置文件URL的例子： <terracottaConfig url="http://internal/ehcache/app/tc-config.xml"/>
分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[Spring 3.0.5 MVC 基于注解ehcache.xml 配 ...](http://zuzong.iteye.com/blog/1048731 "Spring 3.0.5 MVC 基于注解ehcache.xml 配置方式") | [过滤掉非指定保留的html元素，保留元素间的 ...](http://zuzong.iteye.com/blog/941759 "过滤掉非指定保留的html元素，保留元素间的内容和指定的html")
* 2011-05-18 17:20
* 浏览 507
* [评论(0)](http://zuzong.iteye.com/blog/1048714#comments)
* 分类:[企业架构](http://www.iteye.com/blogs/category/architecture)
* [相关推荐](http://www.iteye.com/wiki/blog/1048714)

### 评论

[]()
### 发表评论

[![]()](http://zuzong.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://zuzong.iteye.com/login)

[![zuzong的博客]( "zuzong的博客: 奈何桥收费站。。。")](http://zuzong.iteye.com/)

zuzong

* 浏览: 24410 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 济南
* ![]()
### 最近访客 [更多访客>>](http://zuzong.iteye.com/blog/user_visits)

[![小菜菜的博客]( "小菜菜的博客: ")](http://136203489-qq-com.iteye.com/)

[小菜菜](http://136203489-qq-com.iteye.com/ "小菜菜")

[![donald3003a的博客]( "donald3003a的博客: ")](http://donald3003a.iteye.com/)

[donald3003a](http://donald3003a.iteye.com/ "donald3003a")
[![china_ymex的博客]( "china_ymex的博客: ")](http://umex.iteye.com/)

[china_ymex](http://umex.iteye.com/ "china_ymex")

[![dylinshi126的博客]( "dylinshi126的博客: ")](http://dylinshi126.iteye.com/)

[dylinshi126](http://dylinshi126.iteye.com/ "dylinshi126")

### 文章分类

* [全部博客 (52)](http://zuzong.iteye.com/)
* [java (22)](http://zuzong.iteye.com/category/51241)
* [UI (4)](http://zuzong.iteye.com/category/51242)
* [临时 (1)](http://zuzong.iteye.com/category/52900)
* [缓存 (2)](http://zuzong.iteye.com/category/157174)
* [Python (1)](http://zuzong.iteye.com/category/157184)
* [安全 (1)](http://zuzong.iteye.com/category/157246)
* [Perl (1)](http://zuzong.iteye.com/category/157330)
* [软件设计 (4)](http://zuzong.iteye.com/category/157455)
* [MySQL (2)](http://zuzong.iteye.com/category/159555)
* [Groovy on Grails (1)](http://zuzong.iteye.com/category/181752)
* [Linux (11)](http://zuzong.iteye.com/category/182760)
* [Eclipse (1)](http://zuzong.iteye.com/category/183872)
### 社区版块

* [我的资讯](http://zuzong.iteye.com/blog/news) (0)
* [我的论坛](http://zuzong.iteye.com/blog/post) (30)
* [我的问答](http://zuzong.iteye.com/blog/answered_problems) (1)

### 存档分类

* [2011-10](http://zuzong.iteye.com/blog/monthblog/2011-10) (17)
* [2011-09](http://zuzong.iteye.com/blog/monthblog/2011-09) (1)
* [2011-06](http://zuzong.iteye.com/blog/monthblog/2011-06) (5)
* [更多存档...](http://zuzong.iteye.com/blog/monthblog_more)
### 最新评论

* [yangbaodi516](http://yangbaodi516.iteye.com/ "yangbaodi516")： XMLInputFactory2 xmlif = (XMLIn ...
[基于Woodstox的StAX 2 解析XML](http://zuzong.iteye.com/blog/1073358#bc2240488)
* [AK53pro](http://ak53pro.iteye.com/ "AK53pro")： SSL证书怎么伪造啊...有数字签名的啊...
[SSL中间人攻击及防范](http://zuzong.iteye.com/blog/1050164#bc2237084)
* [lysino](http://lysino.iteye.com/ "lysino")： 若把此问题交给oracle的sequence来解决岂不是很简单 ...
[一个循环流水号实现，求评](http://zuzong.iteye.com/blog/1174278#bc2218914)
* [zuzong](http://zuzong.iteye.com/ "zuzong")： 写的时候，考虑过用indexof查一次，删一次，后来写着写着就 ...
[过滤掉非指定保留的html元素，保留元素间的内容和指定的html](http://zuzong.iteye.com/blog/941759#bc1948620)
* [zuzong](http://zuzong.iteye.com/ "zuzong")： 我一开始用的stringbuffer，发现删除了那些不需要的h ...
[过滤掉非指定保留的html元素，保留元素间的内容和指定的html](http://zuzong.iteye.com/blog/941759#bc1948610)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2012 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![]()
