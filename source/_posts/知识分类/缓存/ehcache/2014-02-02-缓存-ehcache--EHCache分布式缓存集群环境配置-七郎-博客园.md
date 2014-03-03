---
layout: post
title: "EHCache分布式缓存集群环境配置 - 七郎 - 博客园"
categories: 缓存
tags: 
 - 缓存
 - ehcache
--- 

# EHCache分布式缓存集群环境配置 - 七郎 - 博客园

[]()

[![返回主页]()](http://www.cnblogs.com/yangy608/)

# [七郎's Blog](http://www.cnblogs.com/yangy608/)

## 草木竹石皆可為劒。至人之用人若鏡，不將不迎，應而不藏，故能勝物而不傷。

* [博客园](http://www.cnblogs.com/)
* [首页](http://www.cnblogs.com/yangy608/)
* [新闻](http://news.cnblogs.com/)
* [新随笔](http://www.cnblogs.com/yangy608/admin/EditPosts.aspx?opt=1)
* [联系](http://space.cnblogs.com/msg/send/%e4%b8%83%e9%83%8e)
* [管理](http://www.cnblogs.com/yangy608/admin/EditPosts.aspx)
* [订阅](http://www.cnblogs.com/yangy608/rss)[![订阅]()](http://www.cnblogs.com/yangy608/rss)
随笔- 144 文章- 1 评论- 1 

# [EHCache分布式缓存集群环境配置](http://www.cnblogs.com/yangy608/archive/2011/10/07/2200669.html)

ehcache提供三种网络连接策略来实现集群，rmi,jgroup还有jms。同时ehcache可以可以实现多播的方式实现集群,也可以手动指定集群主机序列实现集群。

 

Ehcache支持的分布式缓存支持有三种RMI，JGroups，JMS，这里介绍下MRI和JGrpups两种方式，Ehcache使用版本为1.5.0，关于ehcache的其他信息请参考[http://ehcache.sourceforge.net/EhcacheUserGuide.html](http://ehcache.sourceforge.net/EhcacheUserGuide.html)，

关于jgroups的信息请参考http://www.jgroups.org/manual/html_single/index.html。

环境为两台机器 server1 ip：192.168.2.154，server2 ip：192.168.2.23

### 1. RMI方式：

rmi的方式配置要点（下面均是server1上的配置，server2上的只需要把ip兑换即可）

**a. 配置PeerProvider：**

Xml代码
<cacheManagerPeerProviderFactory class="net.sf.ehcache.distribution.RMICacheManagerPeerProviderFactory"

properties="peerDiscovery=manual,rmiUrls=//192.168.2.23:40001/userCache|//192.168.2.23:40001/resourceCache" /> 

配置中通过手动方式同步sever2中的userCache和resourceCache。

**b. 配置CacheManagerPeerListener：**

Xml代码
<cacheManagerPeerListenerFactory class="net.sf.ehcache.distribution.RMICacheManagerPeerListenerFactory"

properties="hostName=192.168.2.154, port=40001,socketTimeoutMillis=2000" /> 

配置中server1监听本机40001端口。

**c. 在每一个cache中添加cacheEventListener，例子如下：**

Xml代码
<cache name="userCache" maxElementsInMemory="10000" eternal="true" overflowToDisk="true" timeToIdleSeconds="0" timeToLiveSeconds="0"diskPersistent="false" diskExpiryThreadIntervalSeconds="120">

<cacheEventListenerFactory class="net.sf.ehcache.distribution.RMICacheReplicatorFactory" properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true,replicateUpdatesViaCopy= false, replicateRemovals= true " />

</cache>

**属性解释：**

必须属性：

        name:设置缓存的名称，用于标志缓存,惟一

        maxElementsInMemory:在内存中最大的对象数量

        maxElementsOnDisk：在DiskStore中的最大对象数量，如为0，则没有限制

        eternal：设置元素是否永久的，如果为永久，则timeout忽略

        overflowToDisk：是否当memory中的数量达到限制后，保存到Disk

可选的属性：

        timeToIdleSeconds：设置元素过期前的空闲时间

        timeToLiveSeconds：设置元素过期前的活动时间

        diskPersistent：是否disk store在虚拟机启动时持久化。默认为false

   diskExpiryThreadIntervalSeconds:运行disk终结线程的时间，默认为120秒

        memoryStoreEvictionPolicy：策略关于Eviction

缓存子元素：

    cacheEventListenerFactory：注册相应的的缓存监听类，用于处理缓存事件，如put,remove,update,和expire

    bootstrapCacheLoaderFactory:指定相应的BootstrapCacheLoader，用于在初始化缓存，以及自动设置。

 

 

参考另外一篇学习笔记[http://wozailongyou.javaeye.com/blog/230252](http://wozailongyou.javaeye.com/blog/230252 "http://wozailongyou.javaeye.com/blog/230252")，也有集群的说明

### 2. JGroups方式：

ehcache 1.5.0之后版本支持的一种方式，配置起来比较简单，要点：

**a. 配置PeerProvider，使用tcp的方式，例子如下：**

Xml代码
<cacheManagerPeerProviderFactory class="net.sf.ehcache.distribution.jgroups.JGroupsCacheManagerPeerProviderFactory"

properties="connect=TCP(start_port=7800):

TCPPING(initial_hosts=192.168.2.154[7800],192.168.2.23[7800];port_range=10;timeout=3000;

num_initial_members=3;up_thread=true;down_thread=true):

VERIFY_SUSPECT(timeout=1500;down_thread=false;up_thread=false):

pbcast.NAKACK(down_thread=true;up_thread=true;gc_lag=100;retransmit_timeout=3000):

pbcast.GMS(join_timeout=5000;join_retry_timeout=2000;shun=false;

print_local_addr=false;down_thread=true;up_thread=true)"

propertySeparator="::" />

**** 

**b.为每个cache添加cacheEventListener：**

Xml代码
<cache name="userCache" maxElementsInMemory="10000" eternal="true"

overflowToDisk="true" timeToIdleSeconds="0" timeToLiveSeconds="0"

diskPersistent="false" diskExpiryThreadIntervalSeconds="120">

<cacheEventListenerFactory class="net.sf.ehcache.distribution.jgroups.JGroupsCacheReplicatorFactory"

properties="replicateAsynchronously=true, replicatePuts=true,

replicateUpdates=true, replicateUpdatesViaCopy=false, replicateRemovals=true"/>

</cache>

JGroup方式配置的两个server上的配置文件一样，若有多个server，在initial_hosts中将server ip加上即可。

一个完整的ehcache.xml文件：

Xml代码
<?xml version="1.0" encoding="UTF-8"?>

<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xsi:noNamespaceSchemaLocation="http://ehcache.sf.net/ehcache.xsd">

<diskStore path="java.io.tmpdir" />

 

<cacheManagerPeerProviderFactory class="net.sf.ehcache.distribution.jgroups.JGroupsCacheManagerPeerProviderFactory"

properties="connect=TCP(start_port=7800):

TCPPING(initial_hosts=192.168.2.154[7800],192.168.2.23[7800];port_range=10;timeout=3000;

num_initial_members=3;up_thread=true;down_thread=true):

VERIFY_SUSPECT(timeout=1500;down_thread=false;up_thread=false):

pbcast.NAKACK(down_thread=true;up_thread=true;gc_lag=100;retransmit_timeout=3000):

pbcast.GMS(join_timeout=5000;join_retry_timeout=2000;shun=false;

print_local_addr=false;down_thread=true;up_thread=true)"

propertySeparator="::" />

 

<defaultCache maxElementsInMemory="10000" eternal="true"

overflowToDisk="true" timeToIdleSeconds="0" timeToLiveSeconds="0"

diskPersistent="false" diskExpiryThreadIntervalSeconds="120">

<cacheEventListenerFactory class="net.sf.ehcache.distribution.jgroups.JGroupsCacheReplicatorFactory"

properties="replicateAsynchronously=true, replicatePuts=true,

replicateUpdates=true, replicateUpdatesViaCopy=false, replicateRemovals=true"/>

</defaultCache>

 

<cache name="velcroCache" maxElementsInMemory="10000" eternal="true"

overflowToDisk="true" timeToIdleSeconds="0" timeToLiveSeconds="0"

diskPersistent="false" diskExpiryThreadIntervalSeconds="120">

<cacheEventListenerFactory class="net.sf.ehcache.distribution.jgroups.JGroupsCacheReplicatorFactory"

properties="replicateAsynchronously=true, replicatePuts=true,

replicateUpdates=true, replicateUpdatesViaCopy=false, replicateRemovals=true"/>

</cache>

<cache name="userCache" maxElementsInMemory="10000" eternal="true"

overflowToDisk="true" timeToIdleSeconds="0" timeToLiveSeconds="0"

diskPersistent="false" diskExpiryThreadIntervalSeconds="120">

<cacheEventListenerFactory class="net.sf.ehcache.distribution.jgroups.JGroupsCacheReplicatorFactory"

properties="replicateAsynchronously=true, replicatePuts=true,

replicateUpdates=true, replicateUpdatesViaCopy=false, replicateRemovals=true"/>

</cache>

<cache name="resourceCache" maxElementsInMemory="10000"

eternal="true" overflowToDisk="true" timeToIdleSeconds="0"

timeToLiveSeconds="0" diskPersistent="false"

diskExpiryThreadIntervalSeconds="120">

<cacheEventListenerFactory class="net.sf.ehcache.distribution.jgroups.JGroupsCacheReplicatorFactory"

properties="replicateAsynchronously=true, replicatePuts=true,

replicateUpdates=true, replicateUpdatesViaCopy=false, replicateRemovals=true"/>

</cache>

</ehcache>

posted @ 2011-10-07 17:30 [七郎](http://www.cnblogs.com/yangy608/) 阅读(1963) 评论(0) [编辑](http://www.cnblogs.com/yangy608/admin/EditPosts.aspx?postid=2200669) [收藏](http://www.cnblogs.com/yangy608/archive/2011/10/07/2200669.html#)
![]()

[刷新页面](http://www.cnblogs.com/yangy608/archive/2011/10/07/2200669.html#)[返回顶部](http://www.cnblogs.com/yangy608/archive/2011/10/07/2200669.html#top)

（评论功能已被博主禁用）
[程序员问答社区，解决您的技术难题](http://q.cnblogs.com/)

[博客园首页](http://www.cnblogs.com/ "程序员的网上家园")[博问](http://q.cnblogs.com/ "程序员问答社区")[新闻](http://news.cnblogs.com/ "IT新闻")[闪存](http://home.cnblogs.com/ing/)[程序员招聘](http://job.cnblogs.com/)[知识库](http://kb.cnblogs.com/)

### 公告
Copyright ©2012 七郎
