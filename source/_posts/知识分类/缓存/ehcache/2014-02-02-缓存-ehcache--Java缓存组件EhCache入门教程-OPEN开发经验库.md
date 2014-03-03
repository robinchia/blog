---
layout: post
title: "Java缓存组件 EhCache 入门教程 - OPEN 开发经验库"
categories: 缓存
tags: 
 - 缓存
 - ehcache
--- 

# Java缓存组件 EhCache 入门教程 - OPEN 开发经验库

[登录](http://lib.open-open.com/members/login?keepThis=true&TB_iframe=true&height=177&width=278 "登录OPEN经验库")   [注册](http://lib.open-open.com/members/register?keepThis=true&TB_iframe=true&height=460&width=710 "注册新的账号")

* [Java开源](http://www.open-open.com/ "Java开源")
* [PHP开源](http://www.php-open.com/ "PHP开源")
* [JS脚本大全](http://ajax.open-open.com/ "JS脚本大全")
* [OPEN家园](http://home.open-open.com/ "OPEN家园")
* OPEN经验库
* [OPEN文档](http://doc.open-open.com/ "OPEN文档频道")
* [OPEN资讯](http://news.open-open.com/ "OPEN资讯频道")
* [OPEN论坛](http://www.open-open.com/bbs "OPEN论坛频道")

[![OPEN开发经验库]()](http://lib.open-open.com/ "OPEN开发经验库")
[所有分类](http://lib.open-open.com/list/all)  >  [软件开发](http://lib.open-open.com/list/1)  >  [缓存组件](http://lib.open-open.com/list/7)  >  [Ehcache](http://lib.open-open.com/list/287)

### Java缓存组件 EhCache 入门教程

**您的评价**:         [**收藏该经验**]()

1.技术背景：

    系统缓存是位于应用程序与物理数据源之间，用于临时存放复制数据的内存区域，目的是为减少应用程序对物理数据源访问的次数，从而提高应用程序的运行性能。缓存设想内存是有限的，缓存的时效性也是有限的，所以可以设定内存数量的大小可以执行失效算法，可以在内存满了的情况下，按照最少访问等算法将缓存直接移除或切换到硬盘上。

    Ehcache从Hibernate发展而来，逐渐涵盖了Cache界的全部功能，是目前发展势头最好的一个项目，具有快速、简单、低消耗、扩展性强、支持对象或序列化缓存，支持缓存或元素的失效，提供LRU、LFU和FIFO缓存策略，支持内存缓存和硬盘缓存和分布式缓存机制等特点。其中Cache的存储方式为内存或磁盘（ps：无须担心容量问题）

2.EhCahe的类层次介绍：

    主要分为三层，最上层是CacheManager，它是操作Ehcache的入口。可以通过CacheManager.getInstance()获得一个单子的CacheManager，或者通过CacheManager的构造函数创建一个新的CacheManager。每个CacheManger都管理多个Cache。每个Cache都以一种类Hash的方式，关联多个Element。Element就是我们用于存放缓存内容的地方。

3.环境搭建：

    很简单只需要将ehcache-2.1.0-distribution.tar.gz和ehcache-web-2.0.2-distribution.tar.gz挤压的jar包放入WEB-INF/lib下。

    再创建一个重要的配置文件ehcache.xml，可以从ehcache组件包中拷贝一个，也可以自己建立一个，需要放到classpath下，一般放于/WEB-INF/classed/ehcache.xml；具体的配置文件可以网上搜一下

4.实际运用

    一个网站的首页估计是被访问次数最多的，我们可以考虑给首页做一个页面缓存；

    缓存策略：应该是某个固定时间之内不变的，比如说2分钟更新一次，以应用结构page-filter-action-service-dao-db为例。

    位置：页面缓存做到尽量靠近客户的地方，就是在page和filter之间，这样的优点就是第一个用户请求后，页面被缓存，第二个用户在请求，走到filter这个请求就结束了，需要在走到action-service-dao-db，好处当然是服务器压力大大降低和客户端页面响应速度加快。

    首页页面缓存存活时间定为2分钟，也就是参数timeToLiveSeconds（缓存的存活时间）应该设置为120，同时timeToIdleSeconds（多长时间不访问缓存，就清楚该缓存）最好也设为2分钟或者小于2分钟。

﻿﻿﻿﻿

接着我们来看一下SimplePageCachingFilter 的配置，
[查看源码](http://lib.open-open.com/view/open1322892011546.html#viewSource "查看源码")[打印](http://lib.open-open.com/view/open1322892011546.html#printSource "打印")[?](http://lib.open-open.com/view/open1322892011546.html#about "?")

01

<

filter

>

02

<

filter-name

>indexCacheFilterfilter-name>
03

<

filter-class

>

04

net.sf.ehcache.constructs.web.filter.SimplePageCachingFilter
05

<

filter-class

>

06

<

filter

>
07

<

filter-mapping

>

08

<

filter-name

>indexCacheFilterfilter-name>
09

<

url-pattern

>*index.actionurl-pattern>

10

<

filter-mapping

>
将上述代码加入到web.xml，那么当打开首页时，你会发现2分钟才会有一堆sql语句出现在控制台，也可以调整为5分钟，总之一切尽在掌控之中。

 

当然，如果你像缓存首页的部分内容时，你需要使用SimplePageFragmentCachingFilter这个filter，我看一下：
[查看源码](http://lib.open-open.com/view/open1322892011546.html#viewSource "查看源码")[打印](http://lib.open-open.com/view/open1322892011546.html#printSource "打印")[?](http://lib.open-open.com/view/open1322892011546.html#about "?")

01

<

filter

>

02

<

filter-name

>indexCacheFilterfilter-name>
03

<

filter-class

>

04

net.sf.ehcache.constructs.web.filter.SimplePageFragmentCachingFilter
05

<

filter-class

>

06

filter>
07

<

filter-mapping

>

08

<

filter-name

>indexCacheFilterfilter-name>
09

<

url-pattern

>*/index_right.jsp<

url-pattern

>

10

<

filter-mapping

>
如此我们将jsp页面通过jsp：include到其他页面，这样就做到了页面局部缓存的效果，这一点貌似没有oscache的tag好用。

 

此外cachefilter中还有一个特性，就是gzip，也就是缓存中的元素是被压缩过的，如果客户端浏览器支持压缩的话，filter会直接返回压缩过的流，这样节省了带宽，把解压的工作交给了客户端浏览即可，当然如果客户端不支持gzip，那么filter会把缓存的元素拿出来解压后在返回给客户端浏览器（大多数爬虫是不支持gzip的，所以filter也会解压后在返回流）。

总之，Ehcache是一个非常轻量级的缓存实现，而且从1.2之后支持了集群，而且是hibernate默认的缓存provider，本文主要介绍Ehcahe对页面缓存的支持，但是它的功能远不止如此，要用好缓存，对J2ee中缓存的原理、适用范围、适用场景等等都需要比较深刻的理解，这样才能用好用对缓存。

 

为了大家通过实际例子加深了解与场景运用，在奉献一个实例：

*在Spring中运用EhCache

    适用任意一个现有开源Cache Framework，要求可以Cache系统中service或者DAO层的get/find等方法返回结果，如果数据更新（适用了Create/update/delete），则刷新cache中相应的内容。

    根据需求，计划适用Spring AOP+enCache来实现这个功能，采用ehCache原因之一就是Spring提供了enCache的支持，至于为何仅仅支持ehcache而不支持oscache和jbosscache就无从得知了。

    AOP少不了拦截器，先创建一个实现了MethodInterceptor接口的拦截器，用来拦截Service/DAO的方法调用，拦截到方法后，搜索该方法的结果在cache中是否存在，如果存在，返回cache中结果，如果不存在返回数据库查询结果，并将结果返回到缓存。
[查看源码](http://lib.open-open.com/view/open1322892011546.html#viewSource "查看源码")[打印](http://lib.open-open.com/view/open1322892011546.html#printSource "打印")[?](http://lib.open-open.com/view/open1322892011546.html#about "?")

01

public

class

MethodCacheInterceptor

implements

MethodInterceptor, InitializingBean

02

{
03

private

static

final

Log logger = LogFactory.getLog(MethodCacheInterceptor.

class

);

04

private

Cache cache;
05

public

void

setCache(Cache cache) {

06

this

.cache = cache;
07

}

08

public

MethodCacheInterceptor() {
09

super

();

10

}
11

/**

12

* 拦截Service/DAO 的方法，并查找该结果是否存在，如果存在就返回cache 中的值，
13

* 否则，返回数据库查询结果，并将查询结果放入cache

14

*/
15

public

Object invoke(MethodInvocation invocation)

throws

Throwable {

16

String targetName = invocation.getThis().getClass().getName();
17

String methodName = invocation.getMethod().getName();

18

Object[] arguments = invocation.getArguments();
19

Object result;

20

logger.debug(

"Find object from cache is "

+ cache.getName());
21

String cacheKey = getCacheKey(targetName, methodName, arguments);

22

Element element = cache.get(cacheKey);
23

Page

13

of

26

24

if

(element ==

null

) {
25

logger.debug(

"Hold up method , Get method result and create cache........!"

);

26

result = invocation.proceed();
27

element =

new

Element(cacheKey, (Serializable) result);

28

cache.put(element);
29

}

30

return

element.getValue();
31

}

32

/**
33

* 获得cache key 的方法，cache key 是Cache 中一个Element 的唯一标识

34

* cache key 包括包名+类名+方法名，如com.co.cache.service.UserServiceImpl.getAllUser
35

*/

36

private

String getCacheKey(String targetName, String methodName, Object[] arguments) {
37

StringBuffer sb =

new

StringBuffer();

38

sb.append(targetName).append(

"."

).append(methodName);
39

if

((arguments !=

null

) && (arguments.length !=

0

)) {

40

for

(

int

i =

0

; i < arguments.length; i++) {
41

sb.append(

"."

).append(arguments[i]);

42

}
43

}

44

return

sb.toString();
45

}

46

/**
47

* implement InitializingBean，检查cache 是否为空

48

*/
49

public

void

afterPropertiesSet()

throws

Exception {

50

Assert.notNull(cache,

"Need a cache. Please use setCache(Cache) create it."

);
51

}

52

}

上面的代码可以看到，在方法invoke中，完成了搜索cache/新建cache的功能

随后，再建立一个拦截器MethodCacheAfterAdvice，作用是在用户进行create/update/delete操作时来刷新、remove相关cache内容，这个拦截器需要实现AfterRetruningAdvice接口，将会在所拦截的方法执行后执行在afterReturning（object arg0，Method arg1，Object[] arg2,object arg3)方法中所预定的操作
[查看源码](http://lib.open-open.com/view/open1322892011546.html#viewSource "查看源码")[打印](http://lib.open-open.com/view/open1322892011546.html#printSource "打印")[?](http://lib.open-open.com/view/open1322892011546.html#about "?")

01

public

class

MethodCacheAfterAdvice

implements

AfterReturningAdvice, InitializingBean

02

{
03

private

static

final

Log logger = LogFactory.getLog(MethodCacheAfterAdvice.

class

);

04

private

Cache cache;
05

Page

15

of

26

06

public

void

setCache(Cache cache) {
07

this

.cache = cache;

08

}
09

public

MethodCacheAfterAdvice() {

10

super

();
11

}

12

public

void

afterReturning(Object arg0, Method arg1, Object[] arg2, Object arg3)

throws
13

Throwable {

14

String className = arg3.getClass().getName();
15

List list = cache.getKeys();

16

for

(

int

i =

0

;i<list.size();i++){
17

String cacheKey = String.valueOf(list.get(i));

18

if

(cacheKey.startsWith(className)){
19

cache.remove(cacheKey);

20

logger.debug(

"remove cache "

+ cacheKey);
21

}

22

}
23

}

24

public

void

afterPropertiesSet()

throws

Exception {
25

Assert.notNull(cache,

"Need a cache. Please use setCache(Cache) create it."

);

26

}
27

}
该方法获取目标class的全名，如：com.co.cache.test.TestServiceImpl，然后循环cache的key list，刷新/remove cache中所有和该class相关的element。

 

接着就是配置encache的属性，如最大缓存数量、cache刷新的时间等等。
[查看源码](http://lib.open-open.com/view/open1322892011546.html#viewSource "查看源码")[打印](http://lib.open-open.com/view/open1322892011546.html#printSource "打印")[?](http://lib.open-open.com/view/open1322892011546.html#about "?")

01

<

ehcache

>

02

<

diskStore

path

=

"c:\\myapp\\cache"

/>
03

<

defaultCache

04

maxElementsInMemory

=

"1000"
05

eternal

=

"false"

06

timeToIdleSeconds

=

"120"
07

timeToLiveSeconds

=

"120"

08

overflowToDisk

=

"true"
09

/>

10

<

cache

name

=

"DEFAULT_CACHE"
11

maxElementsInMemory

=

"10000"

12

eternal

=

"false"
13

timeToIdleSeconds

=

"300000"

14

timeToLiveSeconds

=

"600000"
15

overflowToDisk

=

"true"

16

/>
17

</

ehcache

>
这里需要注意的是defaultCache定义了一个默认的cache，这个Cache不能删除，否则会抛出No default cache is configured异常。另外由于使用拦截器来刷新Cache内容，因此在定义cache生命周期时可以定义较大的数值，timeToIdleSeconds="30000000",timeToLiveSeconds="6000000"，好像还不够大？

 

然后再将Cache和两个拦截器配置到Spring的配置文件cache.xml中即可，需要创建两个“切入点”，分别用于拦截不同方法名的方法。在配置application.xml并且导入cache.xml。这样一个简单的Spring+Encache框架就搭建完成。

 

由于时间关系就介绍到这里，以后有机会还会介绍Ehcache在分布式集群系统中的使用，谢谢大家。
转自：[http://blog.csdn.net/yangchao228/article/details/7027485](http://blog.csdn.net/yangchao228/article/details/7027485)
**相关资讯** 　—　[更多](http://news.open-open.com/)

* [Java 缓存组件Ehcache 2.5.0 发布](http://news.open-open.com/view/1f8183e "Java 缓存组件Ehcache 2.5.0 发布")
* [Web缓存组件 Ehcache Web 2.0.4 发布](http://news.open-open.com/view/b9f7fd "Web缓存组件 Ehcache Web 2.0.4 发布")
* [Java的缓存框架 Ehcache 2.4.4 发布](http://news.open-open.com/view/31cbce "Java的缓存框架 Ehcache 2.4.4 发布")
* [Java 的缓存框架，Ehcache 2.5.1 发布](http://news.open-open.com/view/a3d21b "Java 的缓存框架，Ehcache 2.5.1 发布")
* [Java缓存框架 ehcache 2.4.6 发布](http://news.open-open.com/view/96fb86 "Java缓存框架 ehcache 2.4.6 发布")
* [Java的缓存框架 Ehcache 2.4.5 发布](http://news.open-open.com/view/1ed8575 "Java的缓存框架 Ehcache 2.4.5 发布")
* [Java 缓存框架，Ehcache 2.5.5 发布](http://news.open-open.com/view/621c2b "Java 缓存框架，Ehcache 2.5.5 发布")
* [Java缓存框架 Ehcache 2.5 beta 发布](http://news.open-open.com/view/9fecb "Java缓存框架 Ehcache 2.5 beta 发布")
* [JSR-107（JCache）正在制定当中，将成为Java EE 7的一部分](http://news.open-open.com/view/12e2e14 "JSR-107（JCache）正在制定当中，将成为Java EE 7的一部分")
* [整合Ehcache的Java集群平台 Terracotta 3.5.4 发布](http://news.open-open.com/view/1392891 "整合Ehcache的Java集群平台  Terracotta 3.5.4 发布")
* [分布式缓存能否作为NoSQL数据库？](http://news.open-open.com/view/1c91d17 "分布式缓存能否作为NoSQL数据库？")
* [Java 的 ORM 框架，Hibernate 4.1.7 发布](http://news.open-open.com/view/11cdbc8 "Java 的 ORM 框架，Hibernate 4.1.7 发布")
* [Java极速Web框架 JFinal 1.0.7 正式发布](http://news.open-open.com/view/13a6403 "Java极速Web框架 JFinal 1.0.7 正式发布")
* [JavaEE参考示例，SpringSide 4.0.0 RC3 版发布](http://news.open-open.com/view/34159c "JavaEE参考示例，SpringSide 4.0.0 RC3 版发布")
* [JavaEE参考示例 SpringSide 4.0.0.RC4 -- 2012.08.26发布](http://news.open-open.com/view/12399fd "JavaEE参考示例 SpringSide 4.0.0.RC4 -- 2012.08.26发布")
* [SpringSide 4.0.0 GA 版发布, JavaEE 参考示例](http://news.open-open.com/view/abda76 "SpringSide 4.0.0 GA 版发布, JavaEE 参考示例")
* [Java数据持久化组件 Apache Empire-db 2.2.0-incubating 发布](http://news.open-open.com/view/cceee7 "Java数据持久化组件 Apache Empire-db 2.2.0-incubating 发布")
* [JavaOne 2011战略主题：Java ME、SE和EE的未来规划](http://news.open-open.com/view/1f3a61f "JavaOne 2011战略主题：Java ME、SE和EE的未来规划")
* [Java Web框架Apache Wicket 1.5发布](http://news.open-open.com/view/669161 "Java Web框架Apache Wicket 1.5发布")
* [Java分布式内存对象缓存系统，Sapphire 1.1.9 发布](http://news.open-open.com/view/137d738 "Java分布式内存对象缓存系统，Sapphire 1.1.9 发布")
  **相关文档** 　—　[更多](http://doc.open-open.com/)

* [EHCache使用.doc](http://doc.open-open.com/view/52e79572119048649cf9e0df1b0df443 "EHCache使用.doc")
* [Ehcache文档_单点登录.doc](http://doc.open-open.com/view/7ee6bdf742114414a58eafda5d83521a "Ehcache文档_单点登录.doc")
* [Java 框架思路.doc](http://doc.open-open.com/view/13df1074061f477b8ac85bad611d23be "Java 框架思路.doc")
* [Ehcache用户指南(分布式缓存).pdf](http://doc.open-open.com/view/8ae6e9269cd74418a9e3f4ca8ccab45c "Ehcache用户指南(分布式缓存).pdf")
* [EHCache 总结.doc](http://doc.open-open.com/view/8feca624e6674c5e9b98c02644b0b65e "EHCache 总结.doc")
* [EHCache技术文档.pdf](http://doc.open-open.com/view/3cad4d15c8ba4ae39d46378ccf71f016 "EHCache技术文档.pdf")
* [EHCache 详解 技术文档.doc](http://doc.open-open.com/view/829642794b3b4d5f9c8e77e706f1fc75 "EHCache 详解 技术文档.doc")
* [EHCache 详解技术文档.doc](http://doc.open-open.com/view/8836684368864a9db0d1980a885caad1 "EHCache 详解技术文档.doc")
* [Ehcache 学习手册.doc](http://doc.open-open.com/view/e6a6faf0ebb34bb993a984483415c0b3 "Ehcache 学习手册.doc")
* [使用ehcache .doc](http://doc.open-open.com/view/6a2b3b07d3b44678a0039af87af28bd8 "使用ehcache .doc")
* [Ehcache技术文档详解.doc](http://doc.open-open.com/view/b50e8ad1e2d84ad9a0da96dc988d9d77 "Ehcache技术文档详解.doc")
* [关于ehcache配置.doc](http://doc.open-open.com/view/f3f353ba610944b695617d775fd8f245 "关于ehcache配置.doc")
* [Ehcache缓存配置.doc](http://doc.open-open.com/view/46b071301657417abd9453eb521d1fee "Ehcache缓存配置.doc")
* [Hibernate 的缓存.doc](http://doc.open-open.com/view/fab64aa1633e4795847a5ee35fdf0f2c "Hibernate 的缓存.doc")
* [JAVA 缓存详解.doc](http://doc.open-open.com/view/0e9db4af60694e8dbd714f1081976d7c "JAVA 缓存详解.doc")
* [Hibernate实践(2)-Java EE分布式开发.ppt](http://doc.open-open.com/view/04fba124ce414965968b2c5b940a7894 "Hibernate实践(2)-Java EE分布式开发.ppt")
* [Hibernate 的二级缓存介绍.doc](http://doc.open-open.com/view/f771c4b17a21435eba6bc6831b3000aa "Hibernate 的二级缓存介绍.doc")
* [Memcached 入门资料.ppt](http://doc.open-open.com/view/c5d24b54dcbe40958c8c89b2613e7ce9 "Memcached 入门资料.ppt")
* [JAVA缓存体系及应用.ppt](http://doc.open-open.com/view/e2e77590b0b14b74be0ea8c981ff5b07 "JAVA缓存体系及应用.ppt")
* [Java组件设计.doc](http://doc.open-open.com/view/55c8674b67e54a239b7677ac52a35a5e "Java组件设计.doc")

-
[](http://lib.open-open.com/view/open1322892011546.html#)

内容信息

 4.5
(已有8个人评价)

   63%
   25%
   13%
   0%
   0%

浏览：1568次
贡献者：[openkk](http://home.open-open.com/361)
上传时间：2011-12-03 14:00:11
经验标签

[EhCache](http://lib.open-open.com/tag/EhCache)

同类热门经验

* [Ehcache入门指南](http://lib.open-open.com/view/open1322893330405.html "Ehcache入门指南")

1225次浏览
* [在Hibernate中使用EhCache](http://lib.open-open.com/view/open1322893871640.html "在Hibernate中使用EhCache")

1125次浏览
* [Java Cache 的HashMap实现, 适用场景及分布式ehcache实例](http://lib.open-open.com/view/open1329182508390.html "Java")

1347次浏览
* [超轻量级Java缓存组件 - EhCache](http://lib.open-open.com/view/open1329923205374.html "超轻量级Java缓存组件")

670次浏览
* [EhCache 分布式缓存/缓存集群](http://lib.open-open.com/view/open1342696876495.html "EhCache")

471次浏览
* [EHCache 初步使用指南](http://lib.open-open.com/view/open1322893835030.html "EHCache")

1002次浏览
相关经验

* [超轻量级Java缓存组件 - EhCache](http://lib.open-open.com/view/open1329923205374.html "超轻量级Java缓存组件")
  3人评
* [【EhCache】Java缓存框架使用EhCache结合Spring AOP](http://lib.open-open.com/view/open1344438054796.html "【EhCache】Java缓存框架使用EhCache结合Spring")
  0人评
* [EhCahce缓存框架使用](http://lib.open-open.com/view/open1344322434702.html "EhCahce缓存框架使用")
  1人评
* [Java集成各种cache组件 multicache4j](http://lib.open-open.com/view/open1324813123124.html "Java集成各种cache组件")
  0人评
* [Hibernate一级缓存 & 二级缓存](http://lib.open-open.com/view/open1330063709155.html "Hibernate一级缓存")
  3人评
* [Java Cache 的HashMap实现, 适用场景及分布式ehcache实例](http://lib.open-open.com/view/open1329182508390.html "Java")
  3人评
* [Portal-Basic Java Web 应用开发框架](http://lib.open-open.com/view/open1347268610553.html "Portal-Basic")
  0人评
* [EhCache 分布式下缓存对象的同步](http://lib.open-open.com/view/open1346722442303.html "EhCache")
  0人评
* [Ehcache入门指南](http://lib.open-open.com/view/open1322893330405.html "Ehcache入门指南")
  6人评
* [Ehcache缓存之web应用实例](http://lib.open-open.com/view/open1338942486578.html "Ehcache缓存之web应用实例")
  1人评
* [EhCache 分布式缓存/缓存集群](http://lib.open-open.com/view/open1342696876495.html "EhCache")
  2人评
* [Ehcache 整合Spring 使用页面、对象缓存](http://lib.open-open.com/view/open1342061957640.html "Ehcache")
  3人评
* [ehCache+spring的简单实用](http://lib.open-open.com/view/open1329463211437.html "ehCache+spring的简单实用")
  7人评
* [EHCache 初步使用指南](http://lib.open-open.com/view/open1322893835030.html "EHCache")
  1人评

相关讨论 - [更多](http://www.open-open.com/bbs)

* [软件架构的一致性](http://www.open-open.com/bbs/view/1324279205202 "软件架构的一致性")
* [Java入门教程学习](http://www.open-open.com/bbs/view/1318332443124 "Java入门教程学习")
* [Java NIO基本使用](http://www.open-open.com/bbs/view/1320567848811 "Java")
* [java的concurrent用法详解](http://www.open-open.com/bbs/view/1320131360999 "java的concurrent用法详解")
* [Android 中各种 JAVA 包的功能描述](http://www.open-open.com/bbs/view/1318820562093 "Android")
* [Java开源类库 Gilead](http://www.open-open.com/bbs/view/1319456388311 "Java开源类库")
* [Java Concurrent处理并发需求](http://www.open-open.com/bbs/view/1320131462624 "Java")

[联系我们](http://www.open-open.com/home/1) - [问题反馈](http://www.open-open.com/home/1)

2005-2012 OPEN-OPEN, all rights reserved.
-

收藏提示[close](http://lib.open-open.com/view/open1322892011546.html#)

文件夹 请选择... ------------- 新增文件夹... 新增文件夹 标签 (多个标签用逗号分隔)
取消确认
