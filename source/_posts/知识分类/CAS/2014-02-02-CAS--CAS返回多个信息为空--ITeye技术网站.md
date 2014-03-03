---
layout: post
title: "CAS 返回多个信息为空 - - ITeye技术网站"
categories: CAS
tags: 
 - CAS
--- 

# CAS 返回多个信息为空 - - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼](http://liuzhiyi7288.iteye.com/blog/1135001#)

[招聘](http://www.iteye.com/job) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://liuzhiyi7288.iteye.com/login "登录") [登录](http://liuzhiyi7288.iteye.com/login) [注册](http://liuzhiyi7288.iteye.com/signup)

# [liuzhiyi7288](http://liuzhiyi7288.iteye.com/)

* [**博客**](http://liuzhiyi7288.iteye.com/)
* [微博](http://liuzhiyi7288.iteye.com/weibo)
* [相册](http://liuzhiyi7288.iteye.com/album)
* [收藏](http://liuzhiyi7288.iteye.com/link)
* [留言](http://liuzhiyi7288.iteye.com/blog/guest_book)
* [关于我](http://liuzhiyi7288.iteye.com/blog/profile)

### [CAS 返回多个信息为空]() **

 

CAS client 端使用Filter 获得server端传回的信息，如果多个信息使用：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. assertion.getPrincipal().getAttributes()  
assertion.getPrincipal().getAttributes()来获得一个Map，但是有的时候Map中没有数据为null；
解决方案：配置deployerConfigContext.xml中名称为serviceRegistryDao的bean，如下
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <bean id="serviceRegistryDao" class="org.jasig.cas.services.InMemoryServiceRegistryDaoImpl">  
1.             <property name="registeredServices">  
1.                     <list>  
1.                             <bean class="org.jasig.cas.services.RegisteredServiceImpl"   
1.                     p:id="1"  
1.                     p:description="test"  
1.                     p:serviceId="http://192.168.104.101:8080/*"  
1.                     p:enabled="true"   
1.                     p:ssoEnabled="true"   
1.                     p:anonymousAccess="false">  
1.                     <property name="allowedAttributes" value="key1,key2,key3"/>  
1.                </bean>                    </list>  
1.             </property>  
<bean id="serviceRegistryDao" class="org.jasig.cas.services.InMemoryServiceRegistryDaoImpl"> <property name="registeredServices"> <list> <bean class="org.jasig.cas.services.RegisteredServiceImpl" p:id="1" p:description="test" p:serviceId="http://192.168.104.101:8080/*" p:enabled="true" p:ssoEnabled="true" p:anonymousAccess="false"> <property name="allowedAttributes" value="key1,key2,key3"/> </bean> </list> </property>，其中最重要的配置为：<property name="allowedAttributes" value="key1,key2,key3"/>
这个属性决定了我们在client端获得map是否为null，如果不配置该属性则为null，该属性配置我们返回多个信息对应的key；
此处对应CAS SERVER端源码如下 CentralAuthenticationServiceImpl.java：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. // 获得deployerConfigContext.xml中serviceRegistryDao配置的registeredServices  
1.         final RegisteredService registeredService = this.servicesManager  
1.             .findServiceBy(service);  
// 获得deployerConfigContext.xml中serviceRegistryDao配置的registeredServices final RegisteredService registeredService = this.servicesManager .findServiceBy(service);
allowwedAttributes配置对应的代码如下：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. /** 
1.              * 从principal.getAttributes()获得map后，获取registeredService的allowedAttributes属性， 
1.              * 可以对应的key存入返回客户端信息的中map中 
1.              */  
1.             for (final String attribute : registeredService  
1.                 .getAllowedAttributes()) {  
1.                 final Object value = principal.getAttributes().get(  
1.                     attribute);  
1.                 if (value != null) {  
1.                     attributes.put(attribute, value);  
1.                 }  
1.             }  
1.   
1.             final Principal modifiedPrincipal = new SimplePrincipal(  
1.                 principalId, attributes);  
1.               
1.             final MutableAuthentication mutableAuthentication = new MutableAuthentication(  
1.                 modifiedPrincipal);  
/** * 从principal.getAttributes()获得map后，获取registeredService的allowedAttributes属性， * 可以对应的key存入返回客户端信息的中map中 */ for (final String attribute : registeredService .getAllowedAttributes()) { final Object value = principal.getAttributes().get( attribute); if (value != null) { attributes.put(attribute, value); } } final Principal modifiedPrincipal = new SimplePrincipal( principalId, attributes); final MutableAuthentication mutableAuthentication = new MutableAuthentication( modifiedPrincipal);；
希望对大家有所帮助
分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[CAS client 端添加初始化参数传到server端](http://liuzhiyi7288.iteye.com/blog/1135037 "CAS client 端添加初始化参数传到server端") | [CAS 集成java 登录成功后空白页面](http://liuzhiyi7288.iteye.com/blog/1132260 "CAS 集成java 登录成功后空白页面")
* 2011-07-28 17:21
* 浏览 389
* [评论(1)](http://liuzhiyi7288.iteye.com/blog/1135001#comments)
* 分类:[开源软件](http://www.iteye.com/blogs/category/opensource)
* [相关推荐](http://www.iteye.com/wiki/blog/1135001)

### 评论

[]()

1 楼 [solomon](http://solomon.iteye.com/ "solomon") 2011-10-31  

  不错，雪中送炭。　![]()
### 发表评论

[![]()](http://liuzhiyi7288.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://liuzhiyi7288.iteye.com/login)

[![liuzhiyi7288的博客]( "liuzhiyi7288的博客: ")](http://liuzhiyi7288.iteye.com/)

liuzhiyi7288

* 浏览: 2639 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 北京
* ![]()
### 最近访客 [更多访客>>](http://liuzhiyi7288.iteye.com/blog/user_visits)

[![mzmingly的博客]( "mzmingly的博客: ")](http://mzmingly.iteye.com/)

[mzmingly](http://mzmingly.iteye.com/ "mzmingly")

[![hasker的博客]( "hasker的博客: ")](http://hasker.iteye.com/)

[hasker](http://hasker.iteye.com/ "hasker")
[![ninisui的博客]( "ninisui的博客: ")](http://ninisui.iteye.com/)

[ninisui](http://ninisui.iteye.com/ "ninisui")

[![dawnstars的博客]( "dawnstars的博客: dawnstars")](http://dawnstars.iteye.com/)

[dawnstars](http://dawnstars.iteye.com/ "dawnstars")

### 文章分类

* [全部博客 (9)](http://liuzhiyi7288.iteye.com/)
* [svn branch merge trunk (1)](http://liuzhiyi7288.iteye.com/category/199341)
### 社区版块

* [我的资讯](http://liuzhiyi7288.iteye.com/blog/news) (0)
* [我的论坛](http://liuzhiyi7288.iteye.com/blog/post) (0)
* [我的问答](http://liuzhiyi7288.iteye.com/blog/answered_problems) (1)

### 存档分类

* [2012-01](http://liuzhiyi7288.iteye.com/blog/monthblog/2012-01) (1)
* [2011-12](http://liuzhiyi7288.iteye.com/blog/monthblog/2011-12) (1)
* [2011-10](http://liuzhiyi7288.iteye.com/blog/monthblog/2011-10) (1)
* [更多存档...](http://liuzhiyi7288.iteye.com/blog/monthblog_more)
### 评论排行榜

* [openOffice conversion failed: could not ...](http://liuzhiyi7288.iteye.com/blog/1290155 "openOffice  conversion failed: could not load input document")
* [from branch merge to trunk](http://liuzhiyi7288.iteye.com/blog/1334978 "from branch merge to trunk")
* [CAS 返回多个信息为空]( "CAS 返回多个信息为空")
* [CAS 集成java 登录成功后空白页面](http://liuzhiyi7288.iteye.com/blog/1132260 "CAS 集成java 登录成功后空白页面")

### 最新评论

* [muqingren](http://muqingren.iteye.com/ "muqingren")： 我没用过这个组件，抱歉
[openOffice conversion failed: could not load input document](http://liuzhiyi7288.iteye.com/blog/1290155#bc2252321)
* [muqingren](http://muqingren.iteye.com/ "muqingren")： 辛苦
[from branch merge to trunk](http://liuzhiyi7288.iteye.com/blog/1334978#bc2252320)
* [muqingren](http://muqingren.iteye.com/ "muqingren")： ...
[from branch merge to trunk](http://liuzhiyi7288.iteye.com/blog/1334978#bc2252317)
* [chinacq](http://chinacq.iteye.com/ "chinacq")： 我没有用到 XComponent document，但是上传有 ...
[openOffice conversion failed: could not load input document](http://liuzhiyi7288.iteye.com/blog/1290155#bc2248988)
* [solomon](http://solomon.iteye.com/ "solomon")：   不错，雪中送炭。　
[CAS 返回多个信息为空](http://liuzhiyi7288.iteye.com/blog/1135001#bc2225743)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![]()
