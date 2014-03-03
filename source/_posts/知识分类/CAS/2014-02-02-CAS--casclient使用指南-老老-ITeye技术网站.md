---
layout: post
title: "cas client使用指南 - 老老 - ITeye技术网站"
categories: CAS
tags: 
 - CAS
--- 

# cas client使用指南 - 老老 - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼](http://lvyanglin.iteye.com/blog/618081#)

[招聘](http://www.iteye.com/job) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://lvyanglin.iteye.com/login "登录") [登录](http://lvyanglin.iteye.com/login) [注册](http://lvyanglin.iteye.com/signup)

# [老老](http://lvyanglin.iteye.com/)

* [**博客**](http://lvyanglin.iteye.com/)
* [微博](http://lvyanglin.iteye.com/weibo)
* [相册](http://lvyanglin.iteye.com/album)
* [收藏](http://lvyanglin.iteye.com/link)
* [留言](http://lvyanglin.iteye.com/blog/guest_book)
* [关于我](http://lvyanglin.iteye.com/blog/profile)

### [cas client使用指南]() **

**博客分类：**
* [java](http://lvyanglin.iteye.com/category/67638)
[企业应用](http://www.iteye.com/blogs/tag/%E4%BC%81%E4%B8%9A%E5%BA%94%E7%94%A8)[SSO](http://www.iteye.com/blogs/tag/SSO)[SQL Server](http://www.iteye.com/blogs/tag/SQL%20Server)[应用服务器](http://www.iteye.com/blogs/tag/%E5%BA%94%E7%94%A8%E6%9C%8D%E5%8A%A1%E5%99%A8)[Spring](http://www.iteye.com/blogs/tag/Spring) 

2008-02-27
JA-SIG（CAS）学习笔记3
关键字: cas sso 统一身份认证 java ja-sig
技术背景知识：
  JA-SIG CAS服务环境搭建，请参考 ：JA-SIG（CAS）学习笔记1 
  JA-SIG CAS业务架构介绍，请参考 ：JA-SIG（CAS）学习笔记2
  HTTPS所涉及的Java安全证书知识，请参考 ：Java  keytool 安全证书学习笔记
CAS技术框架
  CAS  Server
    目前，我们使用的CAS Server 3.1.1的是基于Spring Framework编写的，因此在CAS服务器端的配置管理中，绝大多数是Spring式的Java Bean XML配置。CAS 的服务器提供了一套易于定制的用户认证器接口，用户可以根据自身企业的在线系统的认证方式，来定制自己的认证逻辑。不论是传统的用户名/密码方式，还是基于安全证书的方式；是基于关系数据库的存储，还是采用LDAP服务器，CAS  Server给我们提供了这些常用的验证器模板代码，只要稍作修改，便可灵活使用了。
    对于广大的中国企业用户而言，另一个需要定制的功能莫过于全中文、企业特色的用户身份认证页面了。CAS Server提供了两套系统界面，一套是默认的CAS英文标准页面，另一套则是专门提供给用户来定制修改的。（PS：老外们做事情就是人性化啊~~）那么对CAS Server端的后续学习，我们将围绕着身份认证模块定制和界面定制这两方面展开。
  CAS  Client
    客户端我们使用的是CAS Client 2.1.1。虽然在官方网站上已出现了3.1.0版本的下载，但该版本地代码已经完全重写，使用的package和类名同2.1.1大相径庭了，最关键的是，该版本暂时没有对应的API说明文档。虽然咖啡我对程序版本怀有极大的“喜新厌旧”的心态，但安全起见，还是先2.1.1吧，相信3.1.0的文档耶鲁大学的大牛们已经在整理了，期待中……
CAS Client2.1.1.jar中的代码是相当精炼的，有兴趣的朋友建议阅读一下源码。Jar包中的代码分成三个大部分
  1. edu.yale.its.tp.cas.util 包，其中只有一个工具类 SecureURL.java 用来访问HTTPS URL
  2. edu.yale.its.tp.cas.proxy包，用来处理Proxy Authentication代理认证的3个类，其中ProxyTicketReceptor.java是 接收PGT回调的servlet，在下文中我们会提及。
  3. edu.yale.its.tp.cas.client包，其中包含了CAS Filter ，Tag Library等主要的认证客户端工具类，我们在后面会进行重点介绍。
针对CAS Client的学习，我们的重点将放在CAS Filter 和ProxyTicketReceptor 的配置以及在Java SE环境下，直接使用 ServiceTicketValidator进行Ticket认证实现上。
CAS服务器端应用
定制适合你的身份认证程序
    通过前面的学习，我们了解了CAS具有一个良好而强大的SSO功能框架。接下来，我们要学习如何将实际企业应用中的身份认证同CAS进行整合。
    简单的说，要将现有企业应用中的认证集成到CAS Server中，只要实现一个名为AuthenticationHandler的一个认证处理Java接口就行。以下是该接口的源代码：
Java代码
public interface AuthenticationHandler {  
/** 
* 该方法决定一个受支持的credentials是否是可用的， 
* 如果可用，该方法返回true，则说明身份认证通过 
*/ 
boolean  authenticate(Credentials credentials)  throws  AuthenticationException;  
/** 
* 该方法决定一个credentials是否是当前的handle所支持的 
*/ 
boolean  supports(Credentials credentials);  
} 
public interface AuthenticationHandler {
/**
* 该方法决定一个受支持的credentials是否是可用的，
* 如果可用，该方法返回true，则说明身份认证通过
*/
boolean  authenticate(Credentials credentials)  throws  AuthenticationException;
/**
* 该方法决定一个credentials是否是当前的handle所支持的
*/
boolean  supports(Credentials credentials);
}
这里我们要说明一下Credentials这个CAS的概念。所谓Credentials是由外界提供给CAS来证明自身身份的信息，简单的如一个用户名/密码对就是一个Credentials，或者一个经过某种加密算法生成的密文证书也可以是一个Credentials。在程序的实现上，Credentials被声明为一个可序列化的接口，仅仅起着标识作用，源代码如下：
Java代码
public interface Credentials extends Serializable {  
    // marker interface contains no methods  
} 
public interface Credentials extends Serializable {
    // marker interface contains no methods
}
CAS的API中，已经为我们提供了一个最常用的实现UsernamePasswordCredentials  用户名/密码凭证，代码如下：
Java代码
public class UsernamePasswordCredentials implements Credentials {  
    /** Unique ID for serialization. */ 
    private static final long serialVersionUID = -8343864967200862794L;  
    /** The username. */ 
    private String username;  
    /** The password. */ 
    private String password;  
    public final String getPassword() {  
        return this.password;  
    }  
    public final void setPassword(final String password) {  
        this.password = password;  
    }  
    public final String getUsername() {  
        return this.username;  
    }  
    public final void setUsername(final String userName) {  
        this.username = userName;  
    }  
    public String toString() {  
        return this.username;  
    }  
    public boolean equals(final Object obj) {  
        if (obj == null || !obj.getClass().equals(this.getClass())) {  
            return false;  
        }  
        final UsernamePasswordCredentials c = (UsernamePasswordCredentials) obj;  
        return this.username.equals(c.getUsername())  
            && this.password.equals(c.getPassword());  
    }  
    public int hashCode() {  
        return this.username.hashCode() ^ this.password.hashCode();  
    }  
} 
public class UsernamePasswordCredentials implements Credentials {
    /** Unique ID for serialization. */
    private static final long serialVersionUID = -8343864967200862794L;
    /** The username. */
    private String username;
    /** The password. */
    private String password;
    public final String getPassword() {
        return this.password;
    }
    public final void setPassword(final String password) {
        this.password = password;
    }
    public final String getUsername() {
        return this.username;
    }
    public final void setUsername(final String userName) {
        this.username = userName;
    }
    public String toString() {
        return this.username;
    }
    public boolean equals(final Object obj) {
        if (obj == null || !obj.getClass().equals(this.getClass())) {
            return false;
        }
        final UsernamePasswordCredentials c = (UsernamePasswordCredentials) obj;
        return this.username.equals(c.getUsername())
            && this.password.equals(c.getPassword());
    }
    public int hashCode() {
        return this.username.hashCode() ^ this.password.hashCode();
    }
}
很简单不是吗？就是存储一个用户名和密码的java bean而已。
      接下来，我们将一个Credentials传给一个AuthenticationHandler进行认证，首先调用boolean  supports(Credentials credentials)方法察看当前传入的Credentials实例，AuthenticationHandler实例现是否支持它？如果支持，再调用boolean  authenticate(Credentials credentials)方法进行认证。由于用户名/密码方式是最常用的认证方法，因此CAS为我们提供了一个现成的基于该方式的抽象认证处理类AbstractUsernamePasswordAuthenticationHandler。通常我们只需要继承该类，并实现其中的    authenticateUsernamePasswordInternal方法即可。下面我们给出一个Demo的实现类，它的校验逻辑很简单——仅校验用户名的字符长度是否与密码的相等（这里密码是一个表示长度的整数），如果相等则认为认证通过，请看代码：
Java代码
public class UsernameLengthAuthnHandler  
                       extends AbstractUsernamePasswordAuthenticationHandler {  
protected boolean authenticateUsernamePasswordInternal( UsernamePasswordCredentials credentials)  throws AuthenticationException {  
/*  
* 这里我们完全可以用自己的认证逻辑代替，比如将用户名/密码传入一个SQL语句  
* 向数据库验证是否有对应的用户账号，这不是我们最经常干的事么？  
* 只需要将下面的程序替换掉就OK了！！So  easy，so  simple！  
/  
    String username = credentials.getUsername();  
    String password = credentials.getPassword();  
    String correctPassword = Integer.toString(username.length());  
    return correctPassword.equals(password);  
}  
} 
public class UsernameLengthAuthnHandler
                       extends AbstractUsernamePasswordAuthenticationHandler {
protected boolean authenticateUsernamePasswordInternal( UsernamePasswordCredentials credentials)  throws AuthenticationException {
/*
* 这里我们完全可以用自己的认证逻辑代替，比如将用户名/密码传入一个SQL语句
* 向数据库验证是否有对应的用户账号，这不是我们最经常干的事么？
* 只需要将下面的程序替换掉就OK了！！So  easy，so  simple！
/
    String username = credentials.getUsername();
    String password = credentials.getPassword();
    String correctPassword = Integer.toString(username.length());
    return correctPassword.equals(password);
}
}
介绍到这里，大家应该清楚如何定制自己的AuthenticationHandler类了吧！这里要附带说明的是，在CAS Server的扩展API中已经提供了大量常用认证形式的实现类，它们同CAS Server的war包一同分发：
   cas-server-support-generic-3.1.1.jar ——使用Map记录用户认证信息的实现
   cas-server-support-jdbc-3.1.1.jar —— 基于Spring JDBC的数据库实现（我们常用的）
   cas-server-support-ldap-3.1.1.jar —— 基于LDAP的用户认证实现
更多其他形式的实现各位看官有兴趣的，可以一一阅读源码。
配置你的身份认证程序
     完成了定制认证类的代码编写，接下来就是要让CAS Server来调用它了。在CAS的框架中，对程序的配置都是使用Spring Framework的xml文件，这对于熟悉Spring的程序员而言算驾轻就熟了。
     配置文件位于应用部署目录的WEB-INF子目录下——deployerConfigContext.xml。在bean id=authenticationManager 的 authenticationHandlers属性中配置我们的AuthenticationHandlers：
引用
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.0.xsd">
<bean id="authenticationManager"
  class="org.jasig.cas.authentication.AuthenticationManagerImpl">
        。。。
        。。。
  <property name="authenticationHandlers">
   <list>
    <bean class="org.jasig.cas.authentication.handler.support.HttpBasedServiceCredentialsAuthenticationHandler" p:httpClient-ref="httpClient" />
        <!—下面就是系统默认的验证器配置，你可以替换它，或者增加一个新的handler -->
    <bean     class="org.jasig.cas.authentication.handler.support.SimpleTestUsernamePasswordAuthenticationHandler" />
   </list>
  </property>
</bean>
。。。
。。。
</beans>
我们发现authenticationHandlers属性是一个list，在这个list中可以配置多个AuthenticationHandlers。这些AuthenticationHandlers形成了一个验证器链，所有提交给CAS的Credentials信息将通过这个验证器链的链式过滤，只要这链中有一个验证器通过了对Credentials的验证，就认为这个Credentials是合法的。这样的设计使得我们可以很轻松的整合不同验证体系的已有应用到同一个CAS上，比如：A验证器负责校验alpha系统提交的Credentials，它是基于LDAP服务器的；B验证器负责校验beta系统提交的Credentials，它是一个传统的RDB用户表认证；C验证器负责校验gamma系统提交的基于RSA证书加密的Credentials。3种完全不同的用户身份认证通过配置就可以统一在同一个CAS服务内，很好很强大，不是吗！！
定制身份验证登录界面
      CAS Server在显示界面层view使用了“主题Theme”的概念。在{project.home}/webapp/WEB-INF/view/jsp/目录下，系统默认提供了两套得UI —— default和simple 。default方案使用了CSS等相对复杂得界面元素，而simple方案提供了最简化的界面表示方式。在整个的CAS Server服务器端，有四个界面是我们必须要实现的：
    casConfirmView.jsp —— 确认信息（警告信息）页面
    casGenericSuccess.jsp —— 登陆成功提示页面
    casLoginView.jsp —— 登录输入页面
    casLogoutView.jsp —— SSO登出提示页面
这些都是标准的jsp页面，如何实现他们，完全由您说了算，除了名字不能改。
CAS为view的展示提供了3个级别的定制方式，让我们从最直观简单的开始吧。
1． 采用文件覆盖方式：直接修改default中的页面或者将新写好的四个jsp文件覆盖到default目录中。这种方式最直观和简单，但咖啡建议各位在使用这种方式前将原有目录中的文件备份一下，以备不时之需。
2． 修改UI配置文件，定位UI目录：在CAS Server端/webapp/WEB-INF/classes/ 目录下，有一个名为default_views.properties的属性配置文件，你可以通过修改配置文件中的各个页面文件位置，指向你新UI文件，来达到修改页面展示的目的。
3． 修改配置文件的配置文件，这话看起来有点别扭，其实一点不难理解。在方法2中的default_views.properties文件是一整套的UI页面配置。如果我想保存多套的UI页面配置就可以写多个的properties文件来保存这些配置。在CAS Server端/webapp/WEB-INF/目录下有cas-servlet.xml和cas.properties两个文件，cas-servlet.xml使用了cas.properties文件中的cas.viewResolver.basename属性来定义view属性文件的名字，因此你可以选者直接修改cas-servlet.xml中的viewResolver 下的basenames属性，或者修改cas.properties中的cas.viewResolver.basename属性，指定新的properties文件名，这样可以轻松的替换全套UI。
CAS客户端配置及API应用
CASFilter的配置
     对于大部分web应用而言，使用CAS集成统一认证是相对简单的事，只要为需要认证的URL配置edu.yale.its.tp.cas.client.filter.CASFilter认证过滤器。下面我们就针对过滤器的配置进行说明。首先参看一下Filter的基本配置：
引用
<web-app>
  ...
  <filter>
<filter-name>CAS Filter</filter-name>
<filter-class>edu.yale.its.tp.cas.client.filter.CASFilter</filter-class>
    <init-param>
       <param-name>edu.yale.its.tp.cas.client.filter.loginUrl</param-name>
       <param-value>https://secure.its.yale.edu/cas/login<;/param-value>
    </init-param>
    <init-param>
       <param-name>edu.yale.its.tp.cas.client.filter.validateUrl</param-name>
       <param-value>https://secure.its.yale.edu/cas/serviceValidate<;/param-value>
    </init-param>
    <init-param>
       <param-name>edu.yale.its.tp.cas.client.filter.serverName</param-name>
       <param-value>your server name and port (e.g., www.yale.edu:8080)</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CAS Filter</filter-name>
    <url-pattern>/requires-cas-authetication/*</url-pattern>
</filter-mapping>
  ...
</web-app>
上述配置中的init-param是filter的3个必备的属性，下面这张表则是filter全部属性的详细说明：
ProxyTicketReceptor的配置
    大家还记得在前面我们说过的Proxy Authentication中的call back URL吗？ProxyTicketReceptor是部署在client端的一个servlet，提供server端回传PGT和PGTIOU的。它的xml部署如下：
引用
<web-app>
  ...
  <servlet>
  <servlet-name>ProxyTicketReceptor</servlet-name>
  <servlet-class>edu.yale.its.tp.cas.proxy.ProxyTicketReceptor</servlet-class>
    <init-param>
       <param-name>edu.yale.its.tp.cas.proxyUrl</param-name>
       <param-value>https://secure.its.yale.edu/cas/proxy<;/param-value>
    </init-param>
  </servlet>
<servlet-mapping>
  <servlet-name>ProxyTicketReceptor</servlet-name>
  <url-pattern>/CasProxyServlet</url-pattern>
  </servlet-mapping>
  ...
</webapp>
这里要说明的是它的参数edu.yale.its.tp.cas.proxyUrl。在服务端通过ProxyTicketReceptor将PGT和PGTIOU传给客户端后，ProxyTicketReceptor在进行Proxy Authentication的过程中需要向服务端请求一个ProxyTicket（PT），这个proxyUrl就是服务端的请求入口了。（关于Proxy Authentication的运作原理，参见JA-SIG（CAS）学习笔记2 ）
CAS Client端的API应用1．用户可以通过以下两种方式的任意一种，从JSP或servlet中获取通过认证的用户名：
引用
String username = (String)session.getAttribute(CASFilter.CAS_FILTER_USER);
或者
String username = (String)session.getAttribute("edu.yale.its.tp.cas.client.filter.user");
2．获得更完整的受认证用户信息对象CASReceipt Java Bean，可以使用以下语句的任一:
引用
CASReceipt  receipt = (CASReceipt )session.getAttribute(CASFilter.CAS_FILTER_RECEIPT);
或者
CASReceipt  receipt = (CASReceipt )session.getAttribute("edu.yale.its.tp.cas.client.filter.receipt");
3．手工编码使用CAS Java Object进行用户验证，使用ServiceTicketValidator或者 ProxyTicketValidator（代理认证模式下），在servlet中对用户身份进行验证。
3-1．ServiceTicketValidator
Java代码
import edu.yale.its.tp.cas.client.*;   
...  
String user = null;  
String errorCode = null;  
String errorMessage = null;  
String xmlResponse = null;  
   
/* instantiate a new ServiceTicketValidator */ 
ServiceTicketValidator sv = new ServiceTicketValidator();  
   
/* set its parameters */ 
sv.setCasValidateUrl("https://secure.its.yale.edu/cas/serviceValidate");  
sv.setService(urlOfThisService);  
sv.setServiceTicket(request.getParameter("ticket"));   
   
String urlOfProxyCallbackServlet = "https://portal.yale.edu/CasProxyServlet";   
sv.setProxyCallbackUrl(urlOfProxyCallbackServlet);  
   
/* contact CAS and validate */ 
sv.validate();  
   
/* if we want to look at the raw response, we can use getResponse() */ 
xmlResponse = sv.getResponse();  
   
if(sv.isAuthenticationSuccesful()) {  
  user = sv.getUser();  
} else {  
  errorCode = sv.getErrorCode();  
  errorMessage = sv.getErrorMessage();  
}  
  /* The user is now authenticated. */ 
  /* If we did set the proxy callback url, we can get proxy tickets with: */ 
  String urlOfTargetService = "http://hkg2.its.yale.edu/someApp/portalFeed";  
  String proxyTicket = ProxyTicketReceptor.getProxyTicket( sv.getPgtIou() , urlOfTargetService); 
import edu.yale.its.tp.cas.client.*;
...
String user = null;
String errorCode = null;
String errorMessage = null;
String xmlResponse = null;
/* instantiate a new ServiceTicketValidator */
ServiceTicketValidator sv = new ServiceTicketValidator();
/* set its parameters */
sv.setCasValidateUrl("https://secure.its.yale.edu/cas/serviceValidate");
sv.setService(urlOfThisService);
sv.setServiceTicket(request.getParameter("ticket"));
String urlOfProxyCallbackServlet = "https://portal.yale.edu/CasProxyServlet";
sv.setProxyCallbackUrl(urlOfProxyCallbackServlet);
/* contact CAS and validate */
sv.validate();
/* if we want to look at the raw response, we can use getResponse() */
xmlResponse = sv.getResponse();
if(sv.isAuthenticationSuccesful()) {
  user = sv.getUser();
} else {
  errorCode = sv.getErrorCode();
  errorMessage = sv.getErrorMessage();
}
  /* The user is now authenticated. */
  /* If we did set the proxy callback url, we can get proxy tickets with: */
  String urlOfTargetService = "http://hkg2.its.yale.edu/someApp/portalFeed";
  String proxyTicket = ProxyTicketReceptor.getProxyTicket( sv.getPgtIou() , urlOfTargetService);
3-2．ProxyTicketValidator
Java代码
import edu.yale.its.tp.cas.client.*;  
...   
String user = null;  
String errorCode = null;  
String errorMessage = null;  
String xmlResponse = null;  
List proxyList = null;  
   
/* instantiate a new ProxyTicketValidator */ 
ProxyTicketValidator pv = new ProxyTicketValidator();   
   
/* set its parameters */ 
pv.setCasValidateUrl("https://secure.its.yale.edu/cas/proxyValidate");  
pv.setService(urlOfThisService);  
pv.setServiceTicket(request.getParameter("ticket"));   
   
String urlOfProxyCallbackServlet = "https://portal.yale.edu/CasProxyServlet";  
pv.setProxyCallbackUrl(urlOfProxyCallbackServlet);   
   
/* contact CAS and validate */ 
pv.validate();  
   
/* if we want to look at the raw response, we can use getResponse() */ 
xmlResponse = pv.getResponse();   
   
/* read the response */ 
if(pv.isAuthenticationSuccesful()) {  
  user = pv.getUser();  
  proxyList = pv.getProxyList();  
} else {  
  errorCode = pv.getErrorCode();  
  errorMessage = pv.getErrorMessage();  
  /* handle the error */ 
}   
/* The user is now authenticated. */   
/* If we did set the proxy callback url, we can get proxy tickets with this method call: */   
String urlOfTargetService = "http://hkg2.its.yale.edu/someApp/portalFeed";   
String proxyTicket = ProxyTicketReceptor.getProxyTicket( pv.getPgtIou() , urlOfTargetService); 
import edu.yale.its.tp.cas.client.*;
...
String user = null;
String errorCode = null;
String errorMessage = null;
String xmlResponse = null;
List proxyList = null;
/* instantiate a new ProxyTicketValidator */
ProxyTicketValidator pv = new ProxyTicketValidator();
/* set its parameters */
pv.setCasValidateUrl("https://secure.its.yale.edu/cas/proxyValidate");
pv.setService(urlOfThisService);
pv.setServiceTicket(request.getParameter("ticket"));
String urlOfProxyCallbackServlet = "https://portal.yale.edu/CasProxyServlet";
pv.setProxyCallbackUrl(urlOfProxyCallbackServlet);
/* contact CAS and validate */
pv.validate();
/* if we want to look at the raw response, we can use getResponse() */
xmlResponse = pv.getResponse();
/* read the response */
if(pv.isAuthenticationSuccesful()) {
  user = pv.getUser();
  proxyList = pv.getProxyList();
} else {
  errorCode = pv.getErrorCode();
  errorMessage = pv.getErrorMessage();
  /* handle the error */
}
/* The user is now authenticated. */
/* If we did set the proxy callback url, we can get proxy tickets with this method call: */
String urlOfTargetService = "http://hkg2.its.yale.edu/someApp/portalFeed";
String proxyTicket = ProxyTicketReceptor.getProxyTicket( pv.getPgtIou() , urlOfTargetService);
在这里，我们假设上下文环境中的用户已经通过了CAS登录认证，被重定向到当前的servlet下，我们在servlet中获取ticket凭证，servlet的URL对用户身份进行确认。如果上下文参数中无法获取ticket凭证，我们就认为用户尚未登录，那么，该servlet必须负责将用户重定向到CAS的登录页面去。
初战告捷
      到今天为止，我们已经通过JA-SIG学习笔记的1-3部分，对CAS这个开源SSO的框架有了个大体的了解和初步的掌握，希望这些知识能为各位步入CAS殿堂打开一扇的大门。咖啡希望在今后的工作应用中，能同大家一块共同探讨，进一步深入了解CAS。
学无止境。。。。。。
分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[cas-server-3.3.2 使用举例](http://lvyanglin.iteye.com/blog/622656 "cas-server-3.3.2  使用举例") | [cas 服务器端的配置](http://lvyanglin.iteye.com/blog/615166 "cas 服务器端的配置")
* 2010-03-17 14:56
* 浏览 770
* [评论(0)](http://lvyanglin.iteye.com/blog/618081#comments)
* 分类:[编程语言](http://www.iteye.com/blogs/category/language)
* [相关推荐](http://www.iteye.com/wiki/blog/618081)

### 评论

[]()
### 发表评论

[![]()](http://lvyanglin.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://lvyanglin.iteye.com/login)

[![lvyanglin的博客]( "lvyanglin的博客: 老老")](http://lvyanglin.iteye.com/)

lvyanglin

* 浏览: 11625 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 广州
* ![]()
### 最近访客 [更多访客>>](http://lvyanglin.iteye.com/blog/user_visits)

[![shangyue1110的博客]( "shangyue1110的博客: ")](http://shangyue1110.iteye.com/)

[shangyue1110](http://shangyue1110.iteye.com/ "shangyue1110")

[![dylinshi126的博客]( "dylinshi126的博客: ")](http://dylinshi126.iteye.com/)

[dylinshi126](http://dylinshi126.iteye.com/ "dylinshi126")
[![dada7020的博客]( "dada7020的博客: ")](http://dada7020.iteye.com/)

[dada7020](http://dada7020.iteye.com/ "dada7020")

[![k175981998的博客]( "k175981998的博客: ")](http://k175981998.iteye.com/)

[k175981998](http://k175981998.iteye.com/ "k175981998")

### 文章分类

* [全部博客 (35)](http://lvyanglin.iteye.com/)
* [java (20)](http://lvyanglin.iteye.com/category/67638)
* [数据库 (5)](http://lvyanglin.iteye.com/category/67639)
* [其他 (5)](http://lvyanglin.iteye.com/category/67640)
* [在centsos上安装chrome (1)](http://lvyanglin.iteye.com/category/187499)
* [centos 安装 (1)](http://lvyanglin.iteye.com/category/194903)
* [学习linux (0)](http://lvyanglin.iteye.com/category/220785)
### 社区版块

* [我的资讯](http://lvyanglin.iteye.com/blog/news) (0)
* [我的论坛](http://lvyanglin.iteye.com/blog/post) (0)
* [我的问答](http://lvyanglin.iteye.com/blog/answered_problems) (1)

### 存档分类

* [2011-12](http://lvyanglin.iteye.com/blog/monthblog/2011-12) (2)
* [2011-11](http://lvyanglin.iteye.com/blog/monthblog/2011-11) (1)
* [2011-05](http://lvyanglin.iteye.com/blog/monthblog/2011-05) (2)
* [更多存档...](http://lvyanglin.iteye.com/blog/monthblog_more)
### 最新评论

* [403577706](http://403577706.iteye.com/ "403577706")： sudo apt-get build-dep #(packag ...
[sudo apt-get -f install](http://lvyanglin.iteye.com/blog/800337#bc2250038)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![]()
