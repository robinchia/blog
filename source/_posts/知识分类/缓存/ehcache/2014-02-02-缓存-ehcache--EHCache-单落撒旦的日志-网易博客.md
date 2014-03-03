---
layout: post
title: "EHCache - 单落撒旦的日志 - 网易博客"
categories: 缓存
tags: 
 - 缓存
 - ehcache
--- 

# EHCache - 单落撒旦的日志 - 网易博客

[帮助](http://www.360doc.com/help.html)|[留言交流](http://www.360doc.com/advice.html)|  [登录](http://www.360doc.com/login.aspx?reurl=http://www.360doc.com/content/10/1207/16/3880760_75861362.shtml)

![]()

[首 页](http://www.360doc.com/index.html) [阅览室](http://www.360doc.com/readroom.html) [馆友](http://www.360doc.com/weekstar.html) [我的图书馆](http://www.360doc.com/my360doc.aspx)
![]()

[![]()]()
来自：[软件123](http://www.360doc.com/userhome/3880760) > [Hibernate](http://www.360doc.com/userhome.aspx?userid=3880760&cid=3)

配色： ![]() ![]() ![]() ![]() ![]() ![]() 字号：大中小 EHCache - 单落撒旦的日志 - 网易博客 2010-12-07 | 阅：257  转：1  |  [分享](http://www.360doc.com/content/10/1207/16/3880760_75861362.shtml#) 

![]()

* [![分享到QQ空间]()腾讯空间]( "分享到QQ空间")
* [![]()人人网]( "分享到人人")
* [![]()开心网]( "分享到开心")
* [![分享到新浪微博]()新浪微博]()
* [![分享到腾讯微博]()腾讯微博]()
* [![分享到搜狐微博]()搜狐空间]( "分享到搜狐微博")
* ![]()推荐给朋友
* ![]()举报
[![]()](http://www.360doc.com/register.aspx?refer=57&reurl=resaveArt.aspx?articleid=)
   [](http://yxp.163.com/h/action/photoplay.html?sss=frombkerz1116)  

[Cache技术―OSCache](http://blog.163.com/zhang-_-jie/blog/static/16178437820105811397991/)

### EHCache

[缓存技术](http://blog.163.com/zhang-_-jie/blog/#m=0&t=1&c=fks_084067086083080064080080082095085080080068092082085068092 "缓存技术") 2010-06-08 14:07:18 阅读170 评论0   字号：大**中**小 [订阅]()
一、简介

非常简单，而且易用。

ehcache 是一个非常轻量级的缓存实现，而且从1.2 之后就支持了集群，而且是hibernate 默认的缓存provider 。EhCache 是一个纯Java的进程内缓存框架，具有快速、精干等特点，是Hibernate中默认的CacheProvider。

Ehcache可以直接使用。也可以和Hibernate对象/关系框架结合使用。还可以做Servlet缓存。

Cache 存储方式 ：内存或磁盘。

       官方网站：[http://ehcache.sourceforge.net/](http://ehcache.sourceforge.net/)

 

主要特性

1. 快速.
2. 简单.
3. 多种缓存策略
4. 缓存数据有两级：内存和磁盘，因此无需担心容量问题
5. 缓存数据会在虚拟机重启的过程中写入磁盘
6. 可以通过RMI、可插入API等方式进行分布式缓存
7. 具有缓存和缓存管理器的侦听接口
8. 支持多缓存管理器实例，以及一个实例的多个缓存区域
9. 提供Hibernate的缓存实现
10. 等等

 

 

二、快速上手

1、  项目类库中添加ehcache.jar；

2、  在类路径下编写ehcache.xml配置文件。

 

 

三、配置文件参数详解

ehcache.xml是ehcache的配置文件，并且存放在应用的classpath中。下面是对该XML文件中的一些元素及其属性的相关说明： 

<diskStore>元素：指定一个文件目录，当EHCache把数据写到硬盘上时，将把数据写到这个文件目录下。 下面的参数这样解释：   

         user.home – 用户主目录   

         user.dir      – 用户当前工作目录   

         java.io.tmpdir – 默认临时文件路径

<defaultCache>元素：设定缓存的默认数据过期策略。 

<cache>元素：设定具体的命名缓存的数据过期策略。

 

<cache>元素的属性 

name：缓存名称。通常为缓存对象的类名（非严格标准）。 

maxElementsInMemory：设置基于内存的缓存可存放对象的最大数目。 

maxElementsOnDisk：设置基于硬盘的缓存可存放对象的最大数目。  

eternal：如果为true，表示对象永远不会过期，此时会忽略timeToIdleSeconds和timeToLiveSeconds属性，默认为false; 

timeToIdleSeconds： 设定允许对象处于空闲状态的最长时间，以秒为单位。当对象自从最近一次被访问后，如果处于空闲状态的时间超过了timeToIdleSeconds属性值，这个对象就会过期。当对象过期，EHCache将把它从缓存中清空。只有当eternal属性为false，该属性才有效。如果该属性值为0，则表示对象可以无限期地处于空闲状态。 

timeToLiveSeconds：设定对象允许存在于缓存中的最长时间，以秒为单位。当对象自从被存放到缓存中后，如果处于缓存中的时间超过了 timeToLiveSeconds属性值，这个对象就会过期。当对象过期，EHCache将把它从缓存中清除。只有当eternal属性为false，该属性才有效。如果该属性值为0，则表示对象可以无限期地存在于缓存中。timeToLiveSeconds必须大于timeToIdleSeconds属性，才有意义。 

overflowToDisk：如果为true,表示当基于内存的缓存中的对象数目达到了maxElementsInMemory界限后，会把益出的对象写到基于硬盘的缓存中。注意：如果缓存的对象要写入到硬盘中的话，则该对象必须实现了Serializable接口才行。

memoryStoreEvictionPolicy：缓存对象清除策略。有三种：

1 FIFO ，first in first out ，这个是大家最熟的，先进先出，不多讲了

2 LFU ， Less Frequently Used ，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit 属性，hit 值最小的将会被清出缓存。

2 LRU ，Least Recently Used ，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。

四、单独使用EHCache

1.创建CacheManager （net.sf.ehcache.CacheManager）

（1）使用默认配置文件创建

CacheManager manager = CacheManager.create();

（2）使用指定配置文件创建

CacheManager manager = CacheManager.create("src/config/ehcache.xml");

（3）从classpath找寻配置文件并创建

URL url = getClass().getResource("/anothername.xml");

CacheManager manager = CacheManager.create(url);

（4）通过输入流创建

InputStream fis = new FileInputStream(new File("src/config/ehcache.xml").getAbsolutePath());

try { manager = CacheManager.create(fis); } finally { fis.close(); }

 

2.创建Caches （net.sf.ehcache.Cache）

（1）取得配置文件中预先 定义的sampleCache1设置,生成一个Cache

Cache cache = manager.getCache("sampleCache1");

 

（2）设置一个名为test 的新cache,test属性为默认

CacheManager manager = CacheManager.create();

manager.addCache("test");

 

（3）设置一个名为test 的新cache,并定义其属性

CacheManager manager = CacheManager.create();

Cache cache = new Cache("test", 1, true, false, 5, 2);

manager.addCache(cache);

 

（4）删除cache

CacheManager singletonManager = CacheManager.create();

singletonManager.removeCache("sampleCache1");  

 

3.使用Caches

（1）往cache中加入元素

Element element = new Element("key1", "value1");

cache.put(new Element(element);

 

（2）从cache中取得元素

Element element = cache.get("key1");

 

（3）从cache中删除元素

Cache cache = manager.getCache("sampleCache1");

Element element = new Element("key1", "value1");

cache.remove("key1");    

 

3.卸载CacheManager ,关闭Cache

 manager.shutdown();

下附代码。

 

 

五、在 Hibernate 中运用EHCache

1、hibernate.cfg.xml中需设置如下：

3系列版本加入
<property name=” hibernate.cache.provider_class”>
org.hibernate.cache.EhCacheProvider
</property>  EhCacheProvider类位于hibernate3.jar

2.1版本加入

net.sf.ehcache.hibernate.Provider

2.1以下版本加入

net.sf.hibernate.cache.EhCache

 

2、在Hibernate3.x中的etc目录下有ehcache.xml的示范文件，将其复制应用程序的src目录下（编译时会把ehcache.xml复制到WEB-INF/classess目录下），对其中的相关值进行更改以和自己的程序相适合。

 

3、持久化类的映射文件进行配置

       <cache usage="read-write"/>

在<set>标记中设置了<cache usage="read-write"/>，但Hibernate仅把和Group相关的Student的主键id加入到缓存中，如果希望把整个Student的散装属性都加入到二级缓存中，还需要在Student.hbm.xml文件的<class>标记中加入<cache>子标记，如下所示：

<cache usage="read-write" /> <!--cache标记需跟在class标记后-->

 

注：SSH中hibernate配置的cache信息

<prop key="hibernate.cache.provider_class">org.hibernate.cache.EhCacheProvider</prop>

六、在页面中使用EHCache缓存

       简单的来说，如果一个应用中80% 的时间内都在访问20% 的数据，那么，这时候就应该使用缓存了。

       在80/20 原则生效的地方，我们都应该考虑是否可以使用缓存。但即使是这样，缓存也有不同的用法，举个例子，一个网站的首页估计是被访问的次数最多的，我们可以考虑给首页做一个页面缓存。页面访问最频繁的，做缓存。不同的页面的缓存策略有可能有天壤之别。

       毫无疑问，几乎所有的网站的首页都是访问率最高的，而首页上的数据来源又是非常广泛的，大多数来自不同的对象，而且有可能来自不同的db ，所以给首页做缓存是一个不错的主意，那么主页的缓存策略是什么样子的呢，我认为应该是某个固定时间之内不变的，比如说2 分钟更新一次。或者根据不同的网页功能采取合理的策略。

 

在使用ehcache 的页面缓存之前，我们必须要了解ehcache 的2个概念：

（1）timeToIdleSeconds ，多长时间不访问该缓存，那么ehcache 就会清除该缓存。

（2）timeToLiveSeconds ，缓存的存活时间，从开始创建的时间算起。

 

1、配置ehcache.xml文件

2、在web.xml配置文件中配置过滤器信息

好了，缓存整个页面看上去是非常的简单，甚至都不需要写一行代码，只需要几行配置就行了，够简单吧，虽然看上去简单，但是事实上内部实现却不简单哦，有兴趣的话，大家可以看看SimplePageCachingFilter 继承体系的源代码。

 

缓存首页（整个页面）示例：

< filter >

        < filter-name > indexCacheFilter </filter-name >

        < filter-class >

            net.sf.ehcache.constructs.web.filter.SimplePageCachingFilter

        </filter-class >

</filter >  

< filter-mapping >

        < filter-name > indexCacheFilter </filter-name >

        < url-pattern > *index.action </url-pattern >

</filter-mapping >

 

缓存首页的部分内容时，需要使用SimplePageFragmentCachingFilter 这个filter 。如：  

< filter >

        < filter-name > indexCacheFilter </filter-name >

        < filter-class >

            net.sf.ehcache.constructs.web.filter.SimplePageFragmentCachingFilter

        </filter-class >

</filter >  

< filter-mapping >

        < filter-name > indexCacheFilter </filter-name >

        < url-pattern > */index_right.jsp </url-pattern >

</filter-mapping >  

这个jsp 需要被jsp:include 到其他页面，这样就做到的局部页面的缓存。这一点貌似没有oscache 的tag 好用。

 

七、在Spring框架中使用EHCache缓存

     

       就是使用Spring提供的springmodules和EHCache来简化程序的开发，通过配置文件来完成提供缓存。参考springmodules的文档。

 

1、配置ehcache.xml文件

2、创建Spring EHCache的配置xml文件

配置文件代码示例（调试通过）：

<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"

    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

    xmlns:aop="http://www.springframework.org/schema/aop"

    xmlns:tx="http://www.springframework.org/schema/tx"

    xmlns:ehcache="http://www.springmodules.org/schema/ehcache"

    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.5.xsd  

    http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.5.xsd  

    http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd   

    http://www.springmodules.org/schema/ehcache http://www.springmodules.org/schema/cache/springmodules-ehcache.xsd">

 

    <bean id="EhCacheFacade"

       class="org.springmodules.cache.provider.ehcache.EhCacheFacade">

       <property name="failQuietlyEnabled" value="true" />

       <property name="cacheManager">

           <bean

              class="org.springframework.cache.ehcache.EhCacheManagerFactoryBean">

              <property name="configLocation"

                  value="classpath:ehcache.xml">

              </property>

           </bean>

       </property>

    </bean>

 

    <bean id="cachingInterceptor"

       class="org.springmodules.cache.interceptor.caching.MethodMapCachingInterceptor">

       <property name="cacheProviderFacade" ref="EhCacheFacade" />

       <property name="cachingModels">

           <props>

              <prop key="com....test.Manager.get*">

                  cacheName=dictCache

              </prop>

           </props>

       </property>

    </bean>

   

    <bean id="flushingInterceptor"

       class="org.springmodules.cache.interceptor.flush.MethodMapFlushingInterceptor">

       <property name="cacheProviderFacade" ref="EhCacheFacade" />

       <property name="flushingModels">

           <props>

              <prop key="com....test.Manager.update*">

                  cacheNames=dictCache

              </prop>

           </props>

       </property>

    </bean>

   

    <bean

       class="org.springframework.aop.framework.autoproxy.BeanNameAutoProxyCreator">

       <property name="beanNames">

           <value>*anager</value>

       </property>

       <property name="interceptorNames">

           <list>

              <value>flushingInterceptor</value>

              <value>cachingInterceptor</value>

           </list>

       </property>

    </bean>

    <bean id="manager" class="com...test.Manager"></bean>

</beans>

 <cache name="dictCache"

    maxElementsInMemory="50"

    eternal="false"

    timeToIdleSeconds="60"

    timeToLiveSeconds="60"

    overflowToDisk="false"

    memoryStoreEvictionPolicy="LFU">

    </cache>

 

也可以使用注解的形式进行标注缓存方法，不过要修改配置文件，详见springmodules的文档，这里就不提供了。

 

缓存说明：

1、方法不含有参数

    时间到期缓存失效；调用flush，缓存失效。

2、方法中含有参数

    参数不同则每次都缓存,若缓存中存在相同对象，则调用缓存。

    当调用flush，该id对应的缓存都失效。

    当缓存时间到期，该id对应的缓存都失效。

建议：对没有关联的缓存对象采取不同的id配置。所以ehcache会有好多的cache-id配置信息。

           <props>

              <prop key="com....test.Manager.get*">

                  cacheName=dictCache

              </prop>

              ………                     

              <prop key="com....test.Manager2.get*">

                  cacheName=dictCache2

              </prop>

           </props>

 

           <props>

              <prop key="com....test.Manager.update*">

                  cacheNames=dictCache

              </prop>

              ………

              <prop key="com....test.Manager2.update*">

                  cacheNames=dictCache2

              </prop>

           </props>

  上一篇：[基于按annotation的hibernate主键生成策略 - 白首穷经通秘义...](http://www.360doc.com/content/10/1206/20/3880760_75614432.shtml)

下一篇：[Cache技术―OSCache - 单落撒旦的日志 - 网易博客](http://www.360doc.com/content/10/1207/16/3880760_75862016.shtml) [![]()]()

献花(0)

+1 分享到：[![分享到QQ空间]()]( "分享到QQ空间")[![]()]( "分享到人人")[![]()]( "分享到开心")[![分享到新浪微博]()]()[![分享到腾讯微博]()]()[![分享到搜狐微博]()]( "分享到搜狐微博")[![]()]()

* ![]()推荐给朋友
* ![]()举报  (本文系[软件123](http://www.360doc.com/userhome/3880760)首藏  [源文网址](http://blog.163.com/zhang-_-jie/blog/static/1617843782010582718212/))

类似文章

* [Ehcache](http://www.360doc.com/content/11/0811/15/7270363_139632250.shtml)
* [配置Spring+hibernate使用ehcache作为se...](http://www.360doc.com/content/07/0828/15/5136_700268.shtml)
* [Hibernate ehcache二级缓存技术](http://www.360doc.com/content/06/0927/12/6272_218072.shtml)
* [EhCache使用详细介绍（转）](http://www.360doc.com/content/10/1011/11/2560742_60066155.shtml)
* [二级 ehcache](http://www.360doc.com/content/11/0402/17/6069386_106731794.shtml)
* [Hibernate一级缓存和二级缓存](http://www.360doc.com/content/11/1216/17/6088543_172744745.shtml)
* [【有声读物】单田芳评书 - 浅草细浪的日...](http://www.360doc.com/content/10/1128/16/1689336_73159043.shtml)
* [低血压 - 只要有你的日志 - 网易博客](http://www.360doc.com/content/09/0803/16/189491_4643744.shtml)
* [网易博客教程一 - 闲人SGM的日志 - 网易...](http://www.360doc.com/content/10/0207/18/289502_15377865.shtml)
[更多](http://www.360doc.com/relevant/75861362_more.shtml)

0

![正在加载推荐文章]()

您可能会喜欢

[![]()

老师开博客 天天开"网上家长会"
](http://www.360doc.com/content/07/0508/10/26144_488073.shtml "老师开博客 天天开"网上家长会"") [![]()

博客冲击波（1）----商业周刊精华文章
](http://www.360doc.com/content/08/0706/10/8596_1402791.shtml "博客冲击波（1）----商业周刊精华文章") [![]()

红杏妹的博客 l
](http://www.360doc.com/content/12/0307/16/5060984_192513101.shtml "红杏妹的博客 l") [![]()

Ehcache
](http://www.360doc.com/content/11/0811/15/7270363_139632250.shtml "Ehcache") [![]()

教你怎样制作博客首页 - 敏儿的日志 - 网易博客
](http://www.360doc.com/content/10/0317/13/826574_19113845.shtml "教你怎样制作博客首页 - 敏儿的日志 - 网易博客")

[无觅](http://www.wumii.com/widget/relatedItems "无觅相关文章插件")
[]()

![]()

发表评论：

您好，请 [登录]() 或者 [注册](http://www.360doc.com/register.aspx?refer=53&reurl=showweb/0/0/75861362.aspx) 后再进行评论
使用合作网站登录：![]()新浪微博![]()QQ![]()人人
![]()
  [![]()](http://www.360doc.com/userhome/3880760)

[孙中熙——路](http://www.360doc.com/userhome/3880760)

馆藏：[157](http://www.360doc.com/userhome.aspx?userid=3880760&app=1)
关注我：[1](http://www.360doc.com/userhome.aspx?userid=3880760&app=2)

[![]()]() [![]()]()

最新文章

* [JS计算字符串所占字节数](http://www.360doc.com/content/12/0912/20/3880760_235775281.shtml)
* [List的功能方法](http://www.360doc.com/content/12/0624/09/3880760_220086498.shtml)
* [maven dependency:sources](http://www.360doc.com/content/12/0225/15/3880760_189532854.shtml)
* [IBatis](http://www.360doc.com/content/12/0219/15/3880760_187825046.shtml)
* [myeclipse 8.5-9.0 安装 svn 方...](http://www.360doc.com/content/12/0218/13/3880760_187576669.shtml)
* [深入理解Java序列化中的SerialV...](http://www.360doc.com/content/12/0218/11/3880760_187551055.shtml)
* [关于spring中 几个 java.lang.C...](http://www.360doc.com/content/12/0218/11/3880760_187550886.shtml)
* [Derby 轻量级数据库](http://www.360doc.com/content/12/0216/20/3880760_187173708.shtml)
* [Template method pattern 模板...](http://www.360doc.com/content/12/0216/20/3880760_187173508.shtml)
* [工厂方法模式(Factory method)...](http://www.360doc.com/content/12/0216/20/3880760_187173416.shtml)
* [DAO层的分析（一）](http://www.360doc.com/content/12/0216/20/3880760_187173300.shtml)
* [提高数据库并发性能概要](http://www.360doc.com/content/12/0216/20/3880760_187173130.shtml)
[更多>>](http://www.360doc.com/userhome/3880760)
-

热门文章
* [99天毛笔书法速成练习](http://www.360doc.com/content/12/0428/23/0_207401601.shtml)
* [素饺子 做法集锦](http://www.360doc.com/content/11/0111/14/0_85740115.shtml)
* [趣图：我就喜欢你最丑的样子](http://www.360doc.com/content/11/0609/10/0_122630336.shtml)
* [巧做爽口腌黄瓜（图）](http://www.360doc.com/content/12/0222/16/0_188656716.shtml)
* [一个人的私奔](http://www.360doc.com/content/12/0305/14/0_191838346.shtml)
* [老中医的长寿秘诀——长寿粥](http://www.360doc.com/content/10/0719/06/0_39976494.shtml)
* [海论英语《从零开始学口语》（4...](http://www.360doc.com/content/11/0813/18/0_140148370.shtml)
* [尿路结石的秘方](http://www.360doc.com/content/11/1205/23/0_169982196.shtml)
* [能不能不要装可爱(图片)](http://www.360doc.com/content/10/0713/13/0_38709357.shtml)
* [好吃的茄饼蒸肉](http://www.360doc.com/content/12/0511/08/0_210233282.shtml)
* [庄子的快活 穿越时空之舞](http://www.360doc.com/content/10/0811/21/0_45363446.shtml)
* [健康教育处方大全](http://www.360doc.com/content/11/0613/15/0_126649064.shtml)
[更多>>](http://www.360doc.com/readroom.html)

 

[关闭]()

[![]()](http://www.360doc.com/mobilapp.htm)

[](http://www.360doc.com/content/10/1207/16/3880760_75861362.shtml#)
