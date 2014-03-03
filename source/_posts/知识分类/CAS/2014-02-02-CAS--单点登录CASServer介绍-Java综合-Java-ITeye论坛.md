---
layout: post
title: "单点登录CAS Server 介绍 - Java综合 - Java - ITeye论坛"
categories: CAS
tags: 
 - CAS
--- 

# 单点登录CAS Server 介绍 - Java综合 - Java - ITeye论坛

[您还未登录 !](http://www.iteye.com/login "登录") [登录](http://www.iteye.com/login) [注册](http://www.iteye.com/signup)

[![ITeye-最棒的软件开发交流社区]( "ITeye-最棒的软件开发交流社区")](http://www.iteye.com/)

[论坛首页](http://www.iteye.com/forums) → [Java企业应用论坛](http://www.iteye.com/forums/board/Java) →

# 单点登录CAS Server 介绍

[全部](http://www.iteye.com/forums/board/Java) [Hibernate](http://www.iteye.com/forums/tag/Hibernate) [Spring](http://www.iteye.com/forums/tag/Spring) [Struts](http://www.iteye.com/forums/tag/Struts) [iBATIS](http://www.iteye.com/forums/tag/iBATIS) [企业应用](http://www.iteye.com/forums/tag/%E4%BC%81%E4%B8%9A%E5%BA%94%E7%94%A8) [Lucene](http://www.iteye.com/forums/tag/Lucene) [SOA](http://www.iteye.com/forums/tag/SOA) [Java综合](http://www.iteye.com/forums/tag/Java%E7%BB%BC%E5%90%88) [设计模式](http://www.iteye.com/forums/tag/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F) [Tomcat](http://www.iteye.com/forums/tag/Tomcat) [OO](http://www.iteye.com/forums/tag/OO) [JBoss](http://www.iteye.com/forums/tag/JBoss)
浏览 3057 次 锁定老帖子 [主题：单点登录CAS Server 介绍]()

精华帖 (0) :: 良好帖 (0) :: 新手帖 (2) :: 隐藏帖 (0) 作者 正文 * boli.jiang
* 等级: 初级会员
* [![boli.jiang的博客]( "boli.jiang的博客: 天天向上")](http://bolijiang.iteye.com/)
* 性别: ![]( "男")
* 文章: 3
* 积分: 40
* 来自: 成都
* ![]()
    发表时间：2010-04-22   最后修改：2010-04-23

[<](http://www.iteye.com/topic/650595#) [>](http://www.iteye.com/topic/650595#)  猎头职位: [上海:  【上海】外资企业高新诚聘web开发工程师](http://www.iteye.com/jobs/2245)

相关文章: [ ](http://www.iteye.com/topic/650595# "关闭")

* [CAS与LDAP整合的实现](http://www.iteye.com/topic/257036 "CAS与LDAP整合的实现")
* [扩展cas验证系统自己又一些小小的疑问请教大家!](http://www.iteye.com/topic/87654 "扩展cas验证系统自己又一些小小的疑问请教大家!")
* [【原创】CAS调研总结](http://www.iteye.com/topic/544899 "【原创】CAS调研总结")
推荐群组: [liferay](http://liferay.group.iteye.com/)
[更多相关推荐](http://www.iteye.com/wiki/topic/650595)
[Java综合](http://www.iteye.com/forums/tag/Java%E7%BB%BC%E5%90%88)

下面的讲解基于CAS Server 3.3.5版本。

 

CAS Server 配置文件

login-webflow.xml：其中内容指定了当访问cas/login时的程序流程，初始“initialFlowSetup”

cas-servlet.xml：servlet与class对应关系

deployerConfigContext.xml：认证管理器相关

cas.properties：系统属性设置

applicationContext.xml：系统属性相关

argumentExtractorsConfiguration.xml：不是很了解它的用途

ticketExpirationPolicies.xml：ticket过期时间设置

ticketGrantingTicketCookieGenerator.xml：TGT cookie属性相关，是否支持http也在这儿修改

ticketRegistry.xml：保存ticket的类相关设置

uniqueIdGenerators.xml：ticket自动生成类设置

warnCookieGenerator.xml：同ticketGrantingTicketCookieGenerator.xml，生成的 cookie名为CASPRIVACY

 

**/login** ：

当访问/login时，会调用login-webflow.xml中的流程图：

[![]( "点击查看原始大小图片")
 ]()

 

**/serviceValidate:**

对应的处理类是org.jasig.cas.web.ServiceValidateController，主要负责对service ticket的验证，失败返回casServiceValidationFailure.jsp，成功返回casServiceValidationSuccess.jsp

对service ticket的验证是通过client端向server端发送http（或https）实现的

逻辑：

1.通过由client端传来的ticket到DefaultTicketRegistry中获取缓存的ServiceTicketImpl对象，并判断其是否已经过期（ST过期时间默认是5分钟，TGT默认是2个小时，可以在ticketExpirationPolicies.xml中进行修改）以及与当前service的id是否相一，以上都满足则表示验证通过。

2.通过ServiceTicketImpl对象获取到登录之后的Authentication对象，借助于它生成ImmutableAssertionImpl对象并返回

3.成功返回

 

**CAS数据流程**

Credentials-->Principal-->Authentication

 

**定义自己的AuthenticationHandler**

在中心认证进行认证的过程中会调用deployerConfigContext.xml中设置的AuthenticationHandler来进行认证工作。
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1.         <property name="authenticationHandlers">  
1.             <list>  
1.  <!--  
1.   
1.                     This is the authentication handler that authenticates services by means of callback via SSL, thereby validating  
1.   
1.                     a server side SSL certificate.  
1. -->  
1.                 <bean class="org.jasig.cas.authentication.handler.support.HttpBasedServiceCredentialsAuthenticationHandler"  
1.                     p:httpClient-ref="httpClient" />  
1.                 <!--  
1.                      This is the authentication handler declaration that every CAS deployer will need to change before deploying CAS   
1.                      into production.  The default SimpleTestUsernamePasswordAuthenticationHandler authenticates UsernamePasswordCredentials  
1.                      where the username equals the password.  You will need to replace this with an AuthenticationHandler that implements your  
1.                      local authentication strategy.  You might accomplish this by coding a new such handler and declaring  
1.                      edu.someschool.its.cas.MySpecialHandler here, or you might use one of the handlers provided in the adaptors modules.  
1.                     -->  
1.                 <bean  
1.                     class="org.jasig.cas.authentication.handler.support.SimpleTestUsernamePasswordAuthenticationHandler" />  
1.                 <bean class="com.goldarmor.live800.cas.Live800CasAuthenticationHandler">  
1.                     <property name="dataSource" ref="casDataSource" />  
1.                 </bean>  
1.             </list  
1.         </property>  
<property name="authenticationHandlers"> <list>  <!-- This is the authentication handler that authenticates services by means of callback via SSL, thereby validating a server side SSL certificate. --> <bean class="org.jasig.cas.authentication.handler.support.HttpBasedServiceCredentialsAuthenticationHandler" p:httpClient-ref="httpClient" /> <!-- This is the authentication handler declaration that every CAS deployer will need to change before deploying CAS into production. The default SimpleTestUsernamePasswordAuthenticationHandler authenticates UsernamePasswordCredentials where the username equals the password. You will need to replace this with an AuthenticationHandler that implements your local authentication strategy. You might accomplish this by coding a new such handler and declaring edu.someschool.its.cas.MySpecialHandler here, or you might use one of the handlers provided in the adaptors modules. --> <bean class="org.jasig.cas.authentication.handler.support.SimpleTestUsernamePasswordAuthenticationHandler" /> <bean class="com.goldarmor.live800.cas.Live800CasAuthenticationHandler"> <property name="dataSource" ref="casDataSource" /> </bean> </list </property>

 如上，我们定义了3个AuthenticationHandler，这正是CAS的一个![]() ，通过配置，我们可以实现针对不同的应用提供不同的认证方式，这样可以实现任意的中心认证。再来看看AuthenticationHandler的代码

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. /** 
1.  * Method to determine if the credentials supplied are valid. 
1.  *  
1.  * @param credentials The credentials to validate. 
1.  * @return true if valid, return false otherwise. 
1.  * @throws AuthenticationException An AuthenticationException can contain 
1.  * details about why a particular authentication request failed. 
1.  */  
1.   
1. boolean authenticate(Credentials credentials)  
1.     throws AuthenticationException;  
1.   
1. /** 
1.  * Method to check if the handler knows how to handle the credentials 
1.  * provided. It may be a simple check of the Credentials class or something 
1.  * more complicated such as scanning the information contained in the 
1.  * Credentials object. 
1.  *  
1.  * @param credentials The credentials to check. 
1.  * @return true if the handler supports the Credentials, false othewrise. 
1.  */  
1. boolean supports(Credentials credentials);  
/** * Method to determine if the credentials supplied are valid. * * @param credentials The credentials to validate. * @return true if valid, return false otherwise. * @throws AuthenticationException An AuthenticationException can contain * details about why a particular authentication request failed. */ boolean authenticate(Credentials credentials) throws AuthenticationException; /** * Method to check if the handler knows how to handle the credentials * provided. It may be a simple check of the Credentials class or something * more complicated such as scanning the information contained in the * Credentials object. * * @param credentials The credentials to check. * @return true if the handler supports the Credentials, false othewrise. */ boolean supports(Credentials credentials);

 我们要做的就是实现这俩个方法而已，特别提醒：可以在cas-servlet.xml中设置你所使用的Credentials，如下：（其中的p:formObjectClass值，如果不指定默认使用UsernamePasswordCredentials）

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <bean id="authenticationViaFormAction" class="org.jasig.cas.web.flow.AuthenticationViaFormAction"  
1.     p:formObjectClass="com.goldarmor.live800.cas.Live800CasCredentials"  
1.     p:centralAuthenticationService-ref="centralAuthenticationService"  
1.     p:warnCookieGenerator-ref="warnCookieGenerator" />  
<bean id="authenticationViaFormAction" class="org.jasig.cas.web.flow.AuthenticationViaFormAction" p:formObjectClass="com.goldarmor.live800.cas.Live800CasCredentials" p:centralAuthenticationService-ref="centralAuthenticationService" p:warnCookieGenerator-ref="warnCookieGenerator" />

 

**定义自己的credentialsToPrincipalResolvers**

通过AuthenticationHandler的认证后，会调用在deployerConfigContext.xml中配置的credentialsToPrincipalResolvers来处理Credentials，生成Principal对象：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1.         <property name="credentialsToPrincipalResolvers">  
1.             <list>  
1.                 <!--  
1.                     UsernamePasswordCredentialsToPrincipalResolver supports the UsernamePasswordCredentials that we use for /login   
1.                     by default and produces SimplePrincipal instances conveying the username from the credentials.  
1.                     If you've changed your LoginFormAction to use credentials other than UsernamePasswordCredentials then you will also  
1.                     need to change this bean declaration (or add additional declarations) to declare a CredentialsToPrincipalResolver that supports the  
1.                     Credentials you are using.  
1. -->              <bean  
1.                     class="org.jasig.cas.authentication.principal.UsernamePasswordCredentialsToPrincipalResolver" />  
1.                 <!--  
1.                     HttpBasedServiceCredentialsToPrincipalResolver supports HttpBasedCredentials.  It supports the CAS 2.0 approach of  
1.                     authenticating services by SSL callback, extracting the callback URL from the Credentials and representing it as a  
1.                     SimpleService identified by that callback URL.  
1.                     If you are representing services by something more or other than an HTTPS URL whereat they are able to  
1.                     receive a proxy callback, you will need to change this bean declaration (or add additional declarations).  
1.                     -->  
1.                 <bean  
1.                     class="org.jasig.cas.authentication.principal.HttpBasedServiceCredentialsToPrincipalResolver" />  
1.                 <bean class="com.goldarmor.live800.cas.Live800CasCredentialsToPrincipalResolver"/>  
1.             </list>  
1.         </property>  
<property name="credentialsToPrincipalResolvers"> <list> <!-- UsernamePasswordCredentialsToPrincipalResolver supports the UsernamePasswordCredentials that we use for /login by default and produces SimplePrincipal instances conveying the username from the credentials. If you've changed your LoginFormAction to use credentials other than UsernamePasswordCredentials then you will also need to change this bean declaration (or add additional declarations) to declare a CredentialsToPrincipalResolver that supports the Credentials you are using. --> <bean class="org.jasig.cas.authentication.principal.UsernamePasswordCredentialsToPrincipalResolver" /> <!-- HttpBasedServiceCredentialsToPrincipalResolver supports HttpBasedCredentials. It supports the CAS 2.0 approach of authenticating services by SSL callback, extracting the callback URL from the Credentials and representing it as a SimpleService identified by that callback URL. If you are representing services by something more or other than an HTTPS URL whereat they are able to receive a proxy callback, you will need to change this bean declaration (or add additional declarations). --> <bean class="org.jasig.cas.authentication.principal.HttpBasedServiceCredentialsToPrincipalResolver" /> <bean class="com.goldarmor.live800.cas.Live800CasCredentialsToPrincipalResolver"/> </list> </property>

 如上：我们也可以像定义AuthenticationHandler一样，可以定义多个credentialsToPrincipalResolvers来处理Credentials，返回你所需要的Principal对象，下面来看看credentialsToPrincipalResolvers的方法：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. /** 
1.  * Turn Credentials into a Principal object by analyzing the information 
1.  * provided in the Credentials and constructing a Principal object based on 
1.  * that information or information derived from the Credentials object. 
1.  *  
1.  * @param credentials from which to resolve Principal 
1.  * @return resolved Principal, or null if the principal could not be resolved. 
1.  */  
1. Principal resolvePrincipal(Credentials credentials);  
1.   
1. /** 
1.  * Determine if a credentials type is supported by this resolver. This is 
1.  * checked before calling resolve principal. 
1.  *  
1.  * @param credentials The credentials to check if we support. 
1.  * @return true if we support these credentials, false otherwise. 
1.  */  
1.   
1.   
1. boolean supports(Credentials credentials);  
/** * Turn Credentials into a Principal object by analyzing the information * provided in the Credentials and constructing a Principal object based on * that information or information derived from the Credentials object. * * @param credentials from which to resolve Principal * @return resolved Principal, or null if the principal could not be resolved. */ Principal resolvePrincipal(Credentials credentials); /** * Determine if a credentials type is supported by this resolver. This is * checked before calling resolve principal. * * @param credentials The credentials to check if we support. * @return true if we support these credentials, false otherwise. */ boolean supports(Credentials credentials);  

在CAS验证的时候，通过访问/serviceValidate可知：验证成功之后返回的casServiceValidationSuccess.jsp中的数据来源于Assertion，下面来看看它的代码：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. List<Authentication> getChainedAuthentications();  
1.   
1. /** 
1.  * True if the validated ticket was granted in the same transaction as that 
1.  * in which its grantor GrantingTicket was originally issued. 
1.  *  
1.  * @return true if validated ticket was granted simultaneous with its 
1.  * grantor's issuance 
1.  */  
1.   
1. boolean isFromNewLogin();  
1.   
1.   
1. /** 
1.  * Method to obtain the service for which we are asserting this ticket is 
1.  * valid for. 
1.  *  
1.  * @return the service for which we are asserting this ticket is valid for. 
1.  */  
1.   
1. Service getService();  
List<Authentication> getChainedAuthentications(); /** * True if the validated ticket was granted in the same transaction as that * in which its grantor GrantingTicket was originally issued. * * @return true if validated ticket was granted simultaneous with its * grantor's issuance */ boolean isFromNewLogin(); /** * Method to obtain the service for which we are asserting this ticket is * valid for. * * @return the service for which we are asserting this ticket is valid for. */ Service getService();

 通过getChainedAuthentications()方法，我们可以得到Authentication对象列表，再看看Authentication的代码：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. /** 
1.  * Method to obtain the Principal. 
1.  *  
1.  * @return a Principal implementation 
1.  */  
1.   
1. Principal getPrincipal();  
1.   
1. /** 
1.  * Method to retrieve the timestamp of when this Authentication object was 
1.  * created. 
1.  *  
1.  * @return the date/time the authentication occurred. 
1.  */  
1.   
1. Date getAuthenticatedDate();  
1.   
1. /** 
1.  * Attributes of the authentication (not the Principal). 
1.  * @return the map of attributes. 
1.  */  
1. Map<String, Object> getAttributes();  
/** * Method to obtain the Principal. * * @return a Principal implementation */ Principal getPrincipal(); /** * Method to retrieve the timestamp of when this Authentication object was * created. * * @return the date/time the authentication occurred. */ Date getAuthenticatedDate(); /** * Attributes of the authentication (not the Principal). * @return the map of attributes. */ Map<String, Object> getAttributes();

 而这其中的Principal就来源于上面提到的由credentialsToPrincipalResolvers处理得到的Principal对象，最后看一下Principal的代码，我们只要再做一个实现他的代码，整个CAS Server就可以信手拈来了，呵呵

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. /** 
1.  * Returns the unique id for the Principal 
1.  * @return the unique id for the Principal. 
1.  */  
1.   
1. String getId();  
1.   
1.   
1.  /** 
1.  *  
1.  * @return 
1.  */  
1.   
1. Map<String, Object> getAttributes();  
/** * Returns the unique id for the Principal * @return the unique id for the Principal. */ String getId(); /** * * @return */ Map<String, Object> getAttributes();

 

我们还可以自定义自己的casServiceValidationSuccess.jsp和casLoginView.jsp页面等，具体的操作办法也是最简单的办法就是备份以前的页面之后修改成自己需要的页面。

 
* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/0023/8503/ccbda995-b8f2-3cfa-9c4c-f7f841391045.jpg)
* 大小: 150.5 KB

* [查看图片附件](http://www.iteye.com/topic/650595#)

声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。
推荐链接

* [](http://www.iteye.com/clicks/609)
* [想进外企，出国，跳槽，找工作？英语不好,快来充电吧](http://www.iteye.com/clicks/716)
* [世界级研发人员汇聚深圳！](http://www.iteye.com/clicks/770) [返回顶楼](http://www.iteye.com/topic/650595#) [ ](http://bolijiang.iteye.com/ "浏览作者的博客") [ ](http://bolijiang.iteye.com/blog/profile "浏览作者资料") [ ](http://my.iteye.com/messages/new?message%5Breceiver_name%5D=boli.jiang "发送站内短信") [ ](http://bolijiang.iteye.com/blog/guest_book "给作者留言") [ ](http://my.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=boli.jiang "关注作者")

[论坛首页](http://www.iteye.com/forums) → [Java企业应用版](http://www.iteye.com/forums/board/Java)
跳转论坛:移动开发技术 Web前端技术 Java企业应用 编程语言技术 综合技术 入门技术 招聘求职 海阔天空

* [上海: 宝尊电商诚聘JAVA高级软件工程师](http://www.iteye.com/jobs/2344 "上海:宝尊电商诚聘JAVA高级软件工程师")
* [吉林: 用友汽车软件诚聘高级Java工程师工作地点：长](http://www.iteye.com/jobs/2137 "吉林:用友汽车软件诚聘高级Java工程师工作地点：长春、北京")
* [浙江: 阿里巴巴诚聘高级Java工程师](http://www.iteye.com/jobs/1628 "浙江:阿里巴巴诚聘高级Java工程师")
* [上海: 用友汽车软件诚聘中级java程序员（工作地点：](http://www.iteye.com/jobs/2143 "上海:用友汽车软件诚聘中级java程序员（工作地点：上海安亭）")
* [浙江: 阿里巴巴诚聘C++开发工程师(搜索引擎和广告](http://www.iteye.com/jobs/1849 "浙江:阿里巴巴诚聘C++开发工程师(搜索引擎和广告引擎方向，偏底层)")
* [云南: 云南远信诚聘高级软件工程师(J2EE)](http://www.iteye.com/jobs/2321 "云南:云南远信诚聘高级软件工程师(J2EE) ")
* [上海: 爱可生技术诚聘高级JAVA工程师](http://www.iteye.com/jobs/2106 "上海:爱可生技术诚聘高级JAVA工程师")
* [吉林: 用友汽车软件诚聘java开发工程师 工作地点：](http://www.iteye.com/jobs/2136 "吉林:用友汽车软件诚聘java开发工程师 工作地点：长春、北京")
* [上海: 用友汽车软件诚聘高级java开发工程师（UAP平](http://www.iteye.com/jobs/2140 "上海:用友汽车软件诚聘高级java开发工程师（UAP平台）")
* [北京: 都乐保诚聘Java开发工程师](http://www.iteye.com/jobs/2427 "北京:都乐保诚聘Java开发工程师")
* [陕西: ThoughtWorks诚聘西安Senior Java Applicatio](http://www.iteye.com/jobs/1144 "陕西:ThoughtWorks诚聘西安Senior Java Application Developer")
* [广东: 用友汽车软件诚聘中级JAVA工程师 工作地点：](http://www.iteye.com/jobs/2144 "广东:用友汽车软件诚聘中级JAVA工程师 工作地点：广州")
* [上海: 宝尊电商诚聘JAVA中级软件工程师](http://www.iteye.com/jobs/2343 "上海:宝尊电商诚聘JAVA中级软件工程师")
* [广东: ★路人甲★诚聘高级Java程序员 广州](http://www.iteye.com/jobs/2437 "广东:★路人甲★诚聘高级Java程序员 广州")
* [上海: 用友汽车软件诚聘高级Java程序员](http://www.iteye.com/jobs/2064 "上海:用友汽车软件诚聘高级Java程序员")
* [浙江: 全麦电子商务诚聘J2EE系统架构师](http://www.iteye.com/jobs/2358 "浙江:全麦电子商务诚聘J2EE系统架构师")
* [上海: 用友汽车软件诚聘初级java开发工程师（UAP平](http://www.iteye.com/jobs/2141 "上海:用友汽车软件诚聘初级java开发工程师（UAP平台）")
* [上海: 用友汽车软件诚聘项目经理](http://www.iteye.com/jobs/2078 "上海:用友汽车软件诚聘项目经理")
* [云南: 云南远信诚聘软件工程师（Java）](http://www.iteye.com/jobs/2297 "云南:云南远信诚聘软件工程师（Java）")
* [北京: 都乐保诚聘Java高级开发工程师](http://www.iteye.com/jobs/2426 "北京:都乐保诚聘Java高级开发工程师")

* [首页](http://www.iteye.com/)
* [资讯](http://www.iteye.com/news)
* [精华](http://www.iteye.com/magazines)
* [论坛](http://www.iteye.com/forums)
* [问答](http://www.iteye.com/ask)
* [博客](http://www.iteye.com/blogs)
* [专栏](http://www.iteye.com/blogs/subjects)
* [群组](http://www.iteye.com/groups)
* [招聘](http://www.iteye.com/job)
* [搜索](http://www.iteye.com/search)
* [广告服务](http://www.iteye.com/index/service)
* [ITeye黑板报](http://webmaster.iteye.com/)
* [联系我们](http://www.iteye.com/index/contactus)
* [友情链接](http://www.iteye.com/index/friend_links)

© 2003-2012 ITeye.com. [ [京ICP证110151号](http://www.miibeian.gov.cn/) 京公网安备110105010620 ]
百联优力(北京)投资有限公司 版权所有 ![]()
