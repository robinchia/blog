---
layout: post
title: "EHCache的使用教程-黑神领主的博客-dev26.com"
categories: 缓存
tags: 
 - 缓存
 - ehcache
--- 

# EHCache的使用教程-黑神领主的博客-dev26.com

### 分享到

* [QQ空间](http://www.dev26.com/blog/article/352/1#)
* [新浪微博](http://www.dev26.com/blog/article/352/1#)
* [百度搜藏](http://www.dev26.com/blog/article/352/1#)
* [人人网](http://www.dev26.com/blog/article/352/1#)
* [腾讯微博](http://www.dev26.com/blog/article/352/1#)
* [开心网](http://www.dev26.com/blog/article/352/1#)
* [腾讯朋友](http://www.dev26.com/blog/article/352/1#)
* [更多...](http://www.dev26.com/blog/article/352/1#)

[百度分享](http://www.dev26.com/blog/article/352/1#)

[首页](http://www.dev26.com/blog/article/352/1#)|[文章](http://www.dev26.com/blog/article/352/1#)|[博客](http://www.dev26.com/blog/article/352/1#)|[开源](http://www.dev26.com/blog/article/352/1#)

# www.dev26.com

## 黑神领主的Web开发站博客

在 文章 博客 开源 论坛 中检索
### 个人资料

[![黑神领主]()](http://www.dev26.com/blog/start/undead)

这家伙很懒，什么都没有留下……

等级: ![20]( "等级：20")

加入时间:2011-12-05 22:36

[发送消息](http://www.dev26.com/blog/article/352/1#inline1)

### 操作中心

* [我的博客]( "我的博客")
* [我关注的话题](http://www.dev26.com/bbs/topic/focus/1 "我关注的话题")
* 新闻投递
* [管理](http://www.dev26.com/manage/home/start)
* [发表文章](http://www.dev26.com/manage/blog/edit)
* [安全退出]()
### 操作中心

* [登录](http://www.dev26.com/login)
* [注册](http://www.dev26.com/runtime/register)

### [所有文章](http://www.dev26.com/blog/start/undead) [![]( "订阅")](http://www.dev26.com/rss/blog?username=undead)

* [JDK基础](http://www.dev26.com/blog/category/32)
* [html5](http://www.dev26.com/blog/category/33)
* [javascript](http://www.dev26.com/blog/category/35)
* [我的JavaEE](http://www.dev26.com/blog/category/5)
* [我的博客](http://www.dev26.com/blog/category/6)
* [数据结构](http://www.dev26.com/blog/category/34)
### 最新发表>>

* [javascript中使用mvc](http://www.dev26.com/blog/article/622/1 "javascript中使用mvc")
* [java实现二级域名](http://www.dev26.com/blog/article/621/1 "java实现二级域名")
* [java Annotation注解知识](http://www.dev26.com/blog/article/620/1 "java Annotation注解知识")
* [inner join,left join,right join,full join 的区别说明](http://www.dev26.com/blog/article/618/1 "inner join,left join,right join,full join 的区别说明")
* [REST的概念是什么](http://www.dev26.com/blog/article/617/1 "REST的概念是什么")

### 最新评论>>

[你好，能不能把示例代码和xml发给我，我研究下自定义类，我这里用起来有点问题。谢谢
jiangdk85@126.com](http://www.dev26.com/blog/article.htm?articleId=558&tmp=1#comments)

[上面的代码找不到ResourceDetailsBuilder这个类](http://www.dev26.com/blog/article.htm?articleId=113&tmp=1#comments)
[上面已经是源码了哟，只是没有JAR包而已，你下载一下吧](http://www.dev26.com/blog/article.htm?articleId=113&tmp=1#comments)

[大大共享下源码可以么？](http://www.dev26.com/blog/article.htm?articleId=113&tmp=1#comments)
[是内存中的权限缓存，这样设置的权限可以即时生效](http://www.dev26.com/blog/article.htm?articleId=113&tmp=1#comments)
-

### EHCache的使用教程

发表于2012-06-08 19:19 | 阅读 218 | 1人对此综合评价 ![5分]( "5分")

开发高并发量，高性能的网站应用系统时，缓存Cache起到了非常重要的作用。本文主要介绍EHCache的使用，以及使用EHCache的实践经验。
笔者使用过多种基于Java的开源Cache组件，其中包括OSCache、JBossCache、EHCache。OSCache功能强大，使用灵活，可用于对象缓存、Filter缓存以及在JSP中直接使用cache标签。笔者在最近的使用过程中发现，在并发量较高时，OSCache会出现线程阻塞和数据错误，通过分析源代码发现是其内部实现的缺陷。JBossCache最大的优点是支持基于对象属性的集群同步，不过JBossCache的配置使用都较复杂，在并发量较高的情况下，对象属性数据在集群中同步也会加大系统的开销。以上两种Cache本文仅作简单介绍，不做深入探讨。
EHCache是来自sourceforge（[http://ehcache.sourceforge.net/](http://ehcache.sourceforge.net/)）的开源项目，也是纯Java实现的简单、快速的Cache组件。EHCache支持内存和磁盘的缓存，支持LRU、LFU和FIFO多种淘汰算法，支持分布式的Cache，可以作为Hibernate的缓存插件。同时它也能提供基于Filter的Cache，该Filter可以缓存响应的内容并采用Gzip压缩提高响应速度。

 EHCache API的基本用法
首先介绍CacheManager类。它主要负责读取配置文件，默认读取CLASSPATH下的ehcache.xml，根据配置文件创建并管理Cache对象。
1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
// 使用默认配置文件创建CacheManager

CacheManager manager = CacheManager.create();
// 通过manager可以生成指定名称的Cache对象

Cache cache = cache = manager.getCache(

"demoCache"

);
// 使用manager移除指定名称的Cache对象

manager.removeCache(

"demoCache"

);
可以通过调用manager.removalAll()来移除所有的Cache。通过调用manager的shutdown()方法可以关闭CacheManager。

有了Cache对象之后就可以进行一些基本的Cache操作，例如：
//往cache中添加元素

Element element =

new

Element(

"key"

,

"value"

);
cache.put(element);

//从cache中取回元素
Element element = cache.get(

"key"

);

element.getValue();
//从Cache中移除一个元素

cache.remove(

"key"

);

可以直接使用上面的API进行数据对象的缓存，这里需要注意的是对于缓存的对象都是必须可序列化的。在下面的篇幅中笔者还会介绍EHCache和Spring、Hibernate的整合使用。

 配置文件
配置文件ehcache.xml中命名为demoCache的缓存配置：
1

2
3

4
5

6
7<

cache

name

=

"demoCache"

maxElementsInMemory

=

"10000"
eternal

=

"false"

overflowToDisk

=

"true"
timeToIdleSeconds

=

"300"

timeToLiveSeconds

=

"600"
memoryStoreEvictionPolicy

=

"LFU"

/>

各配置参数的含义：
maxElementsInMemory：缓存中允许创建的最大对象数
eternal：缓存中对象是否为永久的，如果是，超时设置将被忽略，对象从不过期。
timeToIdleSeconds：缓存数据的钝化时间，也就是在一个元素消亡之前，两次访问时间的最大时间间隔值，这只能在元素不是永久驻留时有效，如果该值是 0 就意味着元素可以停顿无穷长的时间。
timeToLiveSeconds：缓存数据的生存时间，也就是一个元素从构建到消亡的最大时间间隔值，这只能在元素不是永久驻留时有效，如果该值是0就意味着元素可以停顿无穷长的时间。
overflowToDisk：内存不足时，是否启用磁盘缓存。
memoryStoreEvictionPolicy：缓存满了之后的淘汰算法。LRU和FIFO算法这里就不做介绍。LFU算法直接淘汰使用比较少的对象，在内存保留的都是一些经常访问的对象。对于大部分网站项目，该算法比较适用。
如果应用需要配置多个不同命名并采用不同参数的Cache，可以相应修改配置文件，增加需要的Cache配置即可。

 利用Spring APO整合EHCache
首先，在CLASSPATH下面放置ehcache.xml配置文件。在Spring的配置文件中先添加如下cacheManager配置：
1

2
3

4
5

6
7

8
9

10
<bean id=

"cacheManager"

class=

"org.springframework.cache.ehcache.EhCacheManagerFactoryBean"

>
</bean>

配置demoCache：
<bean id=

"demoCache"

class=

"org.springframework.cache.ehcache.EhCacheFactoryBean"

>

<property name=

"cacheManager"

ref=

"cacheManager"

/>
<property name=

"cacheName"

>

<value>demoCache</value>
</property>

</bean>

接下来，写一个实现org.aopalliance.intercept.MethodInterceptor接口的拦截器类。有了拦截器就可以有选择性的配置想要缓存的 bean 方法。如果被调用的方法配置为可缓存，拦截器将为该方法生成 cache key 并检查该方法返回的结果是否已缓存。如果已缓存，就返回缓存的结果，否则再次执行被拦截的方法，并缓存结果供下次调用。具体代码如下：

1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
17

18
19

20
21

22
23

24
25

26
27

28
29

30
31

32
33

34
35

36
37

38
39

40
41

42
43

44
public

class

MethodCacheInterceptor

implements

MethodInterceptor,

InitializingBean {
private

Cache cache;

  
 
public

void

setCache(Cache cache) {

this

.cache = cache;
}

  
 
public

void

afterPropertiesSet()

throws

Exception {

Assert.notNull(cache,
"A cache is required. Use setCache(Cache) to provide one."

);

}
  
 

public

Object invoke(MethodInvocation invocation)

throws

Throwable {
String targetName = invocation.getThis().getClass().getName();

String methodName = invocation.getMethod().getName();
Object[] arguments = invocation.getArguments();

Object result;
String cacheKey = getCacheKey(targetName, methodName, arguments);

Element element =

null

;
synchronized

(

this

){

element = cache.get(cacheKey);
if

(element ==

null

) {

//调用实际的方法
result = invocation.proceed();

element =

new

Element(cacheKey, (Serializable) result);
cache.put(element);

}
}

return

element.getValue();
}

  
 
private

String getCacheKey(String targetName, String methodName,

Object[] arguments) {
StringBuffer sb =

new

StringBuffer();

sb.append(targetName).append(

"."

).append(methodName);
if

((arguments !=

null

) && (arguments.length !=

0

)) {

for

(

int

i =

0

; i < arguments.length; i++) {
sb.append(

"."

).append(arguments[i]);

}
}

return

sb.toString();
}

}

synchronized (this)这段代码实现了同步功能。为什么一定要同步？Cache对象本身的get和put操作是同步的。如果我们缓存的数据来自数据库查询，在没有这段同步代码时，当key不存在或者key对应的对象已经过期时，在多线程并发访问的情况下，许多线程都会重新执行该方法，由于对数据库进行重新查询代价是比较昂贵的，而在瞬间大量的并发查询，会对数据库服务器造成非常大的压力。所以这里的同步代码是很重要的。

接下来，继续完成拦截器和Bean的配置：
1

2
3

4
5

6
7

8
9

10
11

12
13

14
15<

bean

id

=

"methodCacheInterceptor"

class

=

"com.xiebing.utils.interceptor.MethodCacheInterceptor"

>

<

property

name

=

"cache"

>
<

ref

local

=

"demoCache"

/>

</

property

>
</

bean

>

<

bean

id

=

"methodCachePointCut"

class

=

"org.springframework.aop.support.RegexpMethodPointcutAdvisor"

>
<

property

name

=

"advice"

>

<

ref

local

=

"methodCacheInterceptor"

/>
</

property

>

<

property

name

=

"patterns"

>
<

list

>

<

value

>.*myMethod</

value

>
</

list

>

</

property

>
</

bean

>

1

2
3

4
5

6
7

8
9

10
11

12
13<

bean

id

=

"myServiceBean"

class

=

"com.xiebing.ehcache.spring.MyServiceBean"

>
</

bean

>

<

bean

id

=

"myService"

class

=

"org.springframework.aop.framework.ProxyFactoryBean"

>
<

property

name

=

"target"

>

<

ref

local

=

"myServiceBean"

/>
</

property

>

<

property

name

=

"interceptorNames"

>
<

list

>

<

value

>methodCachePointCut</

value

>
</

list

>

</

property

>
</

bean

>

其中myServiceBean是实现了业务逻辑的Bean，里面的方法myMethod()的返回结果需要被缓存。这样每次对myServiceBean的myMethod()方法进行调用,都会首先从缓存中查找,其次才会查询数据库。使用AOP的方式极大地提高了系统的灵活性，通过修改配置文件就可以实现对方法结果的缓存，所有的对Cache的操作都封装在了拦截器的实现中。

 CachingFilter功能
使用Spring的AOP进行整合，可以灵活的对方法的的返回结果对象进行缓存。CachingFilter功能可以对HTTP响应的内容进行缓存。这种方式缓存数据的粒度比较粗，例如缓存整张页面。它的优点是使用简单、效率高，缺点是不够灵活，可重用程度不高。
EHCache使用SimplePageCachingFilter类实现Filter缓存。该类继承自CachingFilter，有默认产生cache key的calculateKey()方法，该方法使用HTTP请求的URI和查询条件来组成key。也可以自己实现一个Filter，同样继承CachingFilter类,然后覆写calculateKey()方法，生成自定义的key。
在笔者参与的项目中很多页面都使用AJAX，为保证JS请求的数据不被浏览器缓存，每次请求都会带有一个随机数参数i。如果使用SimplePageCachingFilter，那么每次生成的key都不一样，缓存就没有意义了。这种情况下，我们就会覆写calculateKey()方法。

要使用SimplePageCachingFilter，首先在配置文件ehcache.xml中，增加下面的配置：
1

2
3

4
5

6
7

8
9

10
11

12
<

cache

name

=

"SimplePageCachingFilter"

maxElementsInMemory

=

"10000"

eternal

=

"false"

overflowToDisk

=

"false"

timeToIdleSeconds

=

"300"

timeToLiveSeconds

=

"600"
memoryStoreEvictionPolicy

=

"LFU"

/>

其中name属性必须为SimplePageCachingFilter，修改web.xml文件，增加一个Filter的配置：
<

filter

>

<

filter-name

>SimplePageCachingFilter</

filter-name

>
<

filter-class

>net.sf.ehcache.constructs.web.filter.SimplePageCachingFilter</

filter-class

>

</

filter

>
<

filter-mapping

>

<

filter-name

>SimplePageCachingFilter</

filter-name

>
<

url-pattern

>/test.jsp</

url-pattern

>

</

filter-mapping

>

下面我们写一个简单的test.jsp文件进行测试，缓存后的页面每次刷新，在600秒内显示的时间都不会发生变化的。代码如下：

1

2
3<%

out.println(

new

Date());
%>

CachingFilter输出的数据会根据浏览器发送的Accept-Encoding头信息进行Gzip压缩。经过笔者测试，Gzip压缩后的数据量是原来的1/4，速度是原来的4-5倍，所以缓存加上压缩，效果非常明显。
在使用Gzip压缩时，需注意两个问题：
1. Filter在进行Gzip压缩时，采用系统默认编码，对于使用GBK编码的中文网页来说，需要将操作系统的语言设置为：zh_CN.GBK，否则会出现乱码的问题。
2. 默认情况下CachingFilter会根据浏览器发送的请求头部所包含的Accept-Encoding参数值来判断是否进行Gzip压缩。虽然IE6/7浏览器是支持Gzip压缩的，但是在发送请求的时候却不带该参数。为了对IE6/7也能进行Gzip压缩，可以通过继承CachingFilter，实现自己的Filter，然后在具体的实现中覆写方法acceptsGzipEncoding。

具体实现参考：
1

2
3

4
5protected

boolean

acceptsGzipEncoding(HttpServletRequest request) {

final

boolean

ie6 = headerContains(request,

"User-Agent"

,

"MSIE 6.0"

);
final

boolean

ie7 = headerContains(request,

"User-Agent"

,

"MSIE 7.0"

);

return

acceptsEncoding(request,

"gzip"

) || ie6 || ie7;
}

EHCache在Hibernate中的使用
EHCache可以作为Hibernate的二级缓存使用。在hibernate.cfg.xml中需增加如下设置：

1

2
3<

prop

key

=

"hibernate.cache.provider_class"

>

org.hibernate.cache.EhCacheProvider
</

prop

>

然后在Hibernate映射文件的每个需要Cache的Domain中，加入类似如下格式信息：
<cache usage="read-write|nonstrict-read-write|read-only" />
比如：

1

2
3

4
5

6
7

8
9<

cache

usage

=

"read-write"

/>

最后在配置文件ehcache.xml中增加一段cache的配置，其中name为该domain的类名。
<

cache

name

=

"domain.class.name"

maxElementsInMemory

=

"10000"
eternal

=

"false"

timeToIdleSeconds

=

"300"
timeToLiveSeconds

=

"600"

overflowToDisk

=

"false"
/>

EHCache的监控
对于Cache的使用，除了功能，在实际的系统运营过程中，我们会比较关注每个Cache对象占用的内存大小和Cache的命中率。有了这些数据，我们就可以对Cache的配置参数和系统的配置参数进行优化，使系统的性能达到最优。EHCache提供了方便的API供我们调用以获取监控数据，其中主要的方法有：

1

2
3

4
5

6
7

8
//得到缓存中的对象数

cache.getSize();
//得到缓存对象占用内存的大小

cache.getMemoryStoreSize();
//得到缓存读取的命中次数

cache.getStatistics().getCacheHits()
//得到缓存读取的错失次数

cache.getStatistics().getCacheMisses()

分布式缓存
EHCache从1.2版本开始支持分布式缓存。分布式缓存主要解决集群环境中不同的服务器间的数据的同步问题。具体的配置如下：

在配置文件ehcache.xml中加入
1

2
3

4
5<

cacheManagerPeerProviderFactory

class

=

"net.sf.ehcache.distribution.RMICacheManagerPeerProviderFactory"
properties

=

"peerDiscovery=automatic, multicastGroupAddress=230.0.0.1, multicastGroupPort=4446"

/>

<

cacheManagerPeerListenerFactory
class

=

"net.sf.ehcache.distribution.RMICacheManagerPeerListenerFactory"

/>

另外，需要在每个cache属性中加入

1

2
3

4
5

6
7

8
<

cacheEventListenerFactory

class

=

"net.sf.ehcache.distribution.RMICacheReplicatorFactory"

/>

例如：
<

cache

name

=

"demoCache"

maxElementsInMemory

=

"10000"
eternal

=

"true"

overflowToDisk

=

"true"

>
<

cacheEventListenerFactory

class

=

"net.sf.ehcache.distribution.RMICacheReplicatorFactory"

/>

</

cache

>

总结
EHCache是一个非常优秀的基于Java的Cache实现。它简单、易用，而且功能齐全，并且非常容易与Spring、Hibernate等流行的开源框架进行整合。通过使用EHCache可以减少网站项目中数据库服务器的访问压力，提高网站的访问速度，改善用户的体验。

原文:http://blog.sina.com.cn/s/blog_46d5caa40100ka9z.html
[](http://www.dev26.com/blog/article/352/1# "分享到QQ空间") [](http://www.dev26.com/blog/article/352/1# "分享到新浪微博") [](http://www.dev26.com/blog/article/352/1# "分享到腾讯微博") [](http://www.dev26.com/blog/article/352/1# "分享到人人网") 更多 [0](http://www.dev26.com/blog/article/352/1# "累计分享0次")

-
评分
请[登录](http://www.dev26.com/login)后再对此文章评分

[]()列出所有评论

**没有可显示的项目**

发表您的评论

您尚未登录，无法发表评论   ![dev26登录]( "登录")为什么要登录后才能发表评论？ 我们发现许多网站的大多数恶意评论（人身攻击，广告等）都是以匿名形式发布的，这样难以使一个技术社区形成一个和谐的讨论氛围。我们鼓励网友提出中肯的评论，留下您的身份也使得作者更容易与您交流。  您当前的登录名为**** 评论：

[加入收藏]()- [设为首页]( "设为首页")- [隐私保护](http://www.dev26.com/runtime/privacy.html)- [联系我们](http://www.dev26.com/runtime/contact.html)- [获得帮助](http://www.dev26.com/runtime/help.html)

Web开发站——为Web开发增添活力 [![]()](http://tongji.baidu.com/hm-web/welcome/ico?s=ac500a971f22019aa0e7403308701c33)

版权所有 © 2011-2012 备案号:京ICP备12003253号-1
### 发送消息
