---
layout: post
title: "cas改造随笔 - horus - 博客园"
categories: CAS
tags: 
 - CAS
--- 

# cas改造随笔 - horus - 博客园

[]()

[hello wood](http://www.cnblogs.com/hellowood/)
[博客园](http://www.cnblogs.com/)   [首页](http://www.cnblogs.com/hellowood/)   [社区](http://space.cnblogs.com/)   [新随笔](http://www.cnblogs.com/hellowood/admin/EditPosts.aspx?opt=1)   [联系](http://space.cnblogs.com/msg/send/horus)   [订阅](http://www.cnblogs.com/hellowood/rss)[![订阅]()](http://www.cnblogs.com/hellowood/rss)   [管理](http://www.cnblogs.com/hellowood/admin/EditPosts.aspx)

随笔-9  评论-5  文章-1  trackbacks-0

# [cas改造随笔](http://www.cnblogs.com/hellowood/archive/2010/08/05/1793364.html)

Sso doc

 

关键字：

sso域名：cas.server.com

登陆地址（spring web flow）：[https://cas.800jit.com/cas/login](https://cas.800jit.com/cas/login)

登陆地址（直接）：[https://cas.800jit.com/cas/directLogin](https://cas.800jit.com/cas/directLogin)

退出地址：[https://cas.800jit.com/cas/logout](https://cas.800jit.com/cas/logout)

语言参数：locale=zh_CN，locale=en

 

# 环境篇

 ![]()

 

## 一、所需软件

Jdk：jdk1.6.0_13

Apache：httpd-2.2.15-win32-x86-openssl-0.9.8m-r2

Tomcat：apache-tomcat-6.0.10

jks2pfx：证书导出工具[http://www.myssl.cn/download/jks2pfx.zip](http://www.myssl.cn/download/jks2pfx.zip)

memcache：memcached-1.2.1-win32（需要memcache集群环境）

 

## 二、安全证书生成

1、**keytool**

Apache、tomcat、jdk需要使用安全证书，进windows command窗口生成证书，命令如下：

--生成证书库

keytool -genkey -alias 800jit -keyalg RSA -keystore d:/cert/800jitkey

--从证书库中到处证书

keytool -export -file d:/cert/800jit.crt -alias 800jit -keystore d:/cert/800jitkey

--把证书导入jdk的证书库

keytool -import -keystore d: /jdk1.6.0_13/jre/lib/security/cacerts -file d:/cert/800jit.crt -alias 800jit

--生成Apache服务器的SSL连接需要配置私钥文件和证书文件

D:\casex\jks2pfx>JKS2PFX.bat .keystore 800jitkey tomcat server_dev00

生成的server_dev00.crt，server_dev00.key放到D:/work/Apache2.2/conf/

 

**2****、openssl**

openssl req -config ..\conf\openssl.cnf -new -out olymtech.csr

openssl rsa -in privkey.pem -out olymtech.key

openssl x509 -in olymtech.csr -out olymtech.cert -req -signkey olymtech.key -days 3650

openssl x509 -in olymtech.cert -out olymtech.der.crt -outform DER

keytool -import -keystore d:/work/jdk1.6.0_13/jre/lib/security/cacerts -file d:/casex/ssl/olymtech.crt -alias olymtech

keytool -import -keystore D:/casex/800jitkey -file D:/casex/cas-doc/ssl/olymtech.crt -alias olymtech

 

## 三、Apache配置

Apache+ssl+虚拟机

--修改http.conf文件

取消注释 LoadModule ssl_module modules/mod_ssl.so

取消注释 Include conf/extra/httpd-ssl.conf

取消注释 Include conf/extra/httpd-vhosts.conf

 

ProxyPass /cas  balancer://cas lbmethod=bytraffic stickysession=jsessionid

<proxy balancer://cas/>

    BalancerMember ajp://192.168.1.190:10009/cas  loadfactor=1 route=jvm1

    BalancerMember ajp://192.168.1.190:8009/cas  loadfactor=1 route=jvm2

</proxy>

 

--修改httpd-ssl.conf

在<VirtualHost _default_:443>下增加

ProxyPass /cas  balancer://cas/ lbmethod=bytraffic stickysession=jsessionid

ProxyPassReverse /cas balancer://cas/

SSLProxyEngine On

 

修改

SSLCertificateFile "D:/work/Apache2.2/conf/server_dev00.crt"

SSLCertificateKeyFile "D:/work/Apache2.2/conf/server_dev00.key"

 

--修改httpd-vhosts.conf

在<VirtualHost *:80>下增加

ProxyPass /cas  balancer://cas/ lbmethod=bytraffic stickysession=jsessionid

ProxyPassReverse /cas balancer://cas/

 

## 四、Tomcat配置

修改server.xml

--配置ssl

<Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"

               maxThreads="150" scheme="https" secure="true"

               clientAuth="false" sslProtocol="TLS"

                        keystoreFile="d:/work/apache-tomcat-6.0.10/conf/800jitkey"

                        keystorePass="111111" />

 

## 五、session共享（Tomcat）

## 六、memcache集群

 

 

# CAS SERVER篇

## 一、CAS 原理介绍

 ![]()

访问流程图

 

**主要原理：**用户第一次访问一个CAS 服务的客户web 应用时（访问URL ：[http://192.168.1.90:8081/web1](http://192.168.1.90:8081/web1) ），部署在客户web 应用的cas AuthenticationFilter ，会截获此请求，生成service 参数，然后redirect 到CAS 服务的login 接口，url为[https://cas:8443/cas/login?service=http%3A%2F%2F192.168.1.90%3A8081%2Fweb1%2F](https://cas:8443/cas/login?service=http%3A%2F%2F192.168.1.90%3A8081%2Fweb1%2F) ，认证成功后，CAS 服务器会生成认证cookie ，写入浏览器，同时将cookie 缓存到服务器本地，CAS 服务器还会根据service 参数生成ticket,ticket 会保存到服务器，也会加在url 后面，然后将请求redirect 回客户web 应用，url 为[http://192.168.1.90:8081/web1/?ticket=ST-5-Sx6eyvj7cPPCfn0pMZuMwnbMvxpCBcNAIi6-20](http://192.168.1.90:8081/web1/?ticket=ST-5-Sx6eyvj7cPPCfn0pMZuMwnbMvxpCBcNAIi6-20) 。这时客户端的AuthenticationFilter 看到ticket 参数后，会跳过，由其后面的TicketValidationFilter 处理，TicketValidationFilter 会利用httpclient 工具访问cas 服务的/serviceValidate 接口, 将ticket 、service 都传到此接口，由此接口验证ticket 的有效性，TicketValidationFilter 如果得到验证成功的消息，就会把用户信息写入web 应用的session里。至此为止，SSO 会话就建立起来了，以后用户在同一浏览器里访问此web 应用时，AuthenticationFilter 会在session 里读取到用户信息，所以就不会去CAS 认证，如果在此浏览器里访问别的web 应用时，AuthenticationFilter 在session 里读取不到用户信息，会去CAS 的login 接口认证，但这时CAS 会读取到浏览器传来的cookie ，所以CAS 不会要求用户去登录页面登录，只是会根据service 参数生成一个ticket ，然后再和web 应用做一个验证ticket 的交互。

 

 

## 二、CAS 服务端的处理逻辑

CAS 服务端总共对外定义了9 个接口，客户端通过访问这9 个接口与服务端交互，这9个接口为：
接口
 
说明
 
备注 /login
 
认证接口
 
  /logout
 
退出接口，负责销毁认证cookie
 
  /validate
 
验证ticket 用的接口，CAS1.0 定义
 
  /serviceValidate
 
验证ticket 用的接口，CAS2.0 定义，返回xml 格式的数据
 
  /proxy
 
支持代理认证功能的接口
 
  /proxyValidate
 
支持代理认证功能的接口
 
  /CentralAuthenticationService
 
用于和远程的web services 交互
 
  /remoteLogin（新增）
 
认证接口
 
  /directLogin（新增）
 
认证接口
 
 

 

详细说明：

 **/login:**

登录流程这部分要考虑到不同种类用户凭证的获取方案，以及客户应用传来的service 、gateway 、renew 参数的不同取值组合，CAS 为了实现流程的高度可配置性，采用了Spring Web Flow 技术。通过CAS 发布包里的login-webflow.xml 、cas-servlet.xml 、applicationContext.xml 这3 个文件，找出 了登录有关的所有组件，画出处理流程图。

 ![]()

CAS 默认的登录处理流程

 ![]()

第一次访问Web 应用的流程走向

 ![]()

已经登录web1 后，访问web1 的资源（web1 没有启动session ），或访问web2 的资源

 

注：

1 ： InitialFlowSetupAction: 是流程的入口。用 request.getContextPath() 的值来设置 cookie 的 Path 值， Cookie 的 path 值是在配置文件里定义的，但这个 Action 负责将 request.getContextPath() 的值设置为 Cookie 的 path 值，这是在 cas 部署环境改变的情况下，灵活地设置 cookie path 的方式；把 cookie 的值以及 service 参数的值放入 requestContext 的 flowscope 里。

2 ： GenerateServiceTicketAction 此 Action 负责根据 service 、 GTC cookie 值生成 ServiceTicket 对象， ServiceTicket 的 ID 就是返回给客户应用的 ticket 参数，如果成功创建 ServiceTicket ，则转发到 WarnAction ，如果创建失败，且 gateway 参数为 true ，则直接redirect 到客户应用， 否则则需要重新认证。

3 ： viewLoginForm 这是登录页面， CAS 在此收集用户凭证。 CAS 提供的默认实现是 /WEB-INF/view/jsp/simple/ui/casLoginView.jsp 。

4 ： bindAndValidate 对应 AuthenticationViaFormAction 的 doBind 方法，该方法负责搜集登录页面上用户录入的凭证信息（用户名、密码等），然后把这些信息封装到 CAS 内部的 Credentials 对象中。用户在 casLoginView.jsp 页面上点击提交后，会触发此方法。

5:submit   对应 AuthenticationViaFormAction 的 submit 方法 , 如果 doBind 方法成功执行完， 则触发 submit 方法，此方法负责调用centralAuthenticationService 的      grantServiceTicket 方法，完成认证工作，如果认证成功，则生成 TicketGrantingTicket 对象，放在缓存里， TicketGrantingTicket 的 ID 就是 TGC Cookie 的 value 值。

6 ： warn  CAS 提供了一个功能：用户在一个 web 应用中跳到另一个 web 应用时， CAS 可以跳转到一个提示页面，该页面提示用户要离开一个应用进入另一个应用，可以让用户自己选择。用户在登录页面 viewLoginForm 上选中了 id=”warn” 的复选框，才能开启这个功能。

WarnAction 就检查用户有没有开启这个功能，如果开启了，则转发到showWarnView, 如果没开启，则直接redirect 到客户应用。

7 ：SendTicketGrantingTicketAction 此Action 负责为response 生成TGC Cookie ，cookie 的值就是 AuthenticationViaFormAction 的submit 方法生成的 TicketGrantingTicket 对象的 ID 。

8 ： viewGenerateLoginSuccess 这是 CAS 的认证成功页面。

 

**/logout:** （ 对应实现类 org.jasig.cas.web.LogoutController ）

处理逻辑：   

        1) removeCookie

       2) 在服务端删除TicketGrantingTicket 对象（此对象封装了cookie 的value 值）

       3 ）redirect 到退出页面，有2 种选择：

          if(LogoutController 的followServiceRedirects 属性为true 值，且url 里的service 参数非空){

                redirect 到 sevice 参数标识的url

             }

          else{

             redirect 到内置的casLogoutView （cas/WEB-INF/view/jsp/default/ui/casLogoutView.jsp ），如果url 里有url 参数，则此url 参数标识的链接会显示在casLogoutView 页面上。

           }

 

**/serviceValidate: **（对应实现类 org.jasig.cas.web.ServiceValidateController ）

 处理逻辑：  

  如果service 参数为空或ticket 参数为空，则转发到failureView （/WEB-INF/view/jsp/default/protocol/2.0/casServiceValidationFailure.jsp ）

    验证ticket 。以ticket 为参数，去缓存里找ServiceTicketImpl 对象，如果能找到，且没有过期，且ServiceTicketImpl 对象对应的service 属性和service 参数对应，则验证通过，验证通过后，请求转发至casServiceSuccessView （cas/WEB-INF/view/jsp/default/protocol/2.0/casServiceValidationSuccess.jsp ），验证不通过，则转发到failureView 。

 

## 三、认证相关的概念及流程

概念

* **Credentials**** **用户提供的用于登录用的凭据信息，如用户名/ 密码、证书、IP 地址、Cookie 值等。比如 UsernamePasswordCredentials ，封装的是用户名和密码。CAS 进行认证的第一步，就是把从UI 或request 对象里取到的用户凭据封装成Credentials 对象，然后交给认证管理器去认证。
* **AuthenticationHandler**** **认证Handler, 每种AuthenticationHandler 只能处理一种Credentials ，如AbstractUsernamePasswordAuthenticationHandler 只负责处理 U sernamePasswordCredentials 。
* **Principal** 封装用户标识，比如 SimplePrincipal, 只是封装了用户名。认证成功后， credentialsToPrincipalResolvers 负责由Credentials 生成 Principal 对象。
* **CredentialsToPrincipalResolvers**** **负责由 Credentials 生成 Principal 对象，每种 CredentialsToPrincipalResolvers 只处理 一种Credentials ，比如 UsernamePasswordCredentialsToPrincipalResolver 负责从 U sernamePasswordCredentials 中取出用户名，然后将其赋给生成的 SimplePrincipal 的 ID 属性。
* **AuthenticationMetaDataPopulators** 负责将 Credentials 的一些属性赋值给 Authentication 的 attributes 属性。
* **Authentication**   Authentication是认证管理器的最终处理结果， Authentication 封装了 Principal ，认证时间，及其他一些属性（可能来自 Credentials ）。
* **AuthenticationManager** 认证管理器得到 Credentials 对象后，负责调度AuthenticationHandler 去完成认证工作，最后返回的结果是 Authentication 对象。
* **CentralAuthenticationService**** ** CAS 的服务类，对 Web 层提供了一些方法。该类还负责调用 AuthenticationManager 完成认证逻辑**。**

 

序列图

 ![]()

 

类图

 ![]()

 

## 四、[Ticket介绍](http://zhenkm0507.javaeye.com/blog/546805)

CAS的核心就是其Ticket，及其在Ticket之上的一系列处理操作。CAS的主要票据有TGT、ST、PGT、PGTIOU、PT，其中TGT、ST是CAS1.0协议中就有的票据，PGT、PGTIOU、PT是CAS2.0协议中有的票据。

 

### 一 名词解释

* TGT（Ticket Grangting Ticket）

TGT是CAS为用户签发的登录票据，拥有了TGT，用户就可以证明自己在CAS成功登录过。TGT封装了Cookie值以及此Cookie值对应的用户信息。用户在CAS认证成功后，CAS生成cookie，写入浏览器，同时生成一个TGT对象，放入自己的缓存，TGT对象的ID就是cookie的值。当HTTP再次请求到来时，如果传过来的有CAS生成的cookie，则CAS以此cookie值为key查询缓存中有无TGT ，如果有的话，则说明用户之前登录过，如果没有，则用户需要重新登录。

 

* ST（Service Ticket）

ST是CAS为用户签发的访问某一service的票据。用户访问service时，service发现用户没有ST，则要求用户去CAS获取ST。用户向CAS发出获取ST的请求，如果用户的请求中包含cookie，则CAS会以此cookie值为key查询缓存中有无TGT，如果存在TGT，则用此TGT签发一个ST，返回给用户。用户凭借ST去访问service，service拿ST去CAS验证，验证通过后，允许用户访问资源。

 

* PGT（Proxy Granting Ticket）

Proxy Service的代理凭据。用户通过CAS成功登录某一Proxy Service后，CAS生成一个PGT对象，缓存在CAS本地，同时将PGT的值（一个UUID字符串）回传给Proxy Service，并保存在Proxy Service里。Proxy Service拿到PGT后，就可以为Target Service（back-end service）做代理，为其申请PT。

 

* PGTIOU（Proxy Granting Ticket IOU）

PGTIOU是CAS协议中定义的一种附加票据，它增强了传输、获取PGT的安全性。
PGT的传输与获取的过程：Proxy Service调用CAS的serviceValidate接口验证ST成功后，CAS首先会访问pgtUrl指向的https url，将生成的 PGT及PGTIOU传输给proxy service，proxy service会以PGTIOU为key，PGT为value，将其存储在Map中；然后CAS会生成验证ST成功的xml消息，返回给Proxy Service，xml消息中含有PGTIOU，proxy service收到Xml消息后，会从中解析出PGTIOU的值，然后以其为key，在map中找出PGT的值，赋值给代表用户信息的Assertion对象的pgtId，同时在map中将其删除。

 

* PT（Proxy Ticket）

PT是用户访问Target Service（back-end service）的票据。如果用户访问的是一个Web应用，则Web应用会要求浏览器提供ST，浏览器就会用cookie去CAS获取一个ST，然后就可以访问这个Web应用了。如果用户访问的不是一个Web应用，而是一个C/S结构的应用，因为C/S结构的应用得不到cookie，所以用户不能自己去CAS获取ST，而是通过访问proxy service的接口，凭借proxy service的PGT去获取一个PT，然后才能访问到此应用。

 

### 二 代码解析

 ![]()

CAS Ticket类图

 

* TicketGrantingTicket 的 grantServiceTicket方法

方法声明：public synchronized ServiceTicket grantServiceTicket(final String id,final Service service, final ExpirationPolicy expirationPolicy, final boolean credentialsProvided)

方法描述:

 1：生成SerivceTicketImpl

 2：更新属性：

this.previousLastTimeUsed = this.lastTimeUsed;

   this.lastTimeUsed = System.currentTimeMillis();

   this.countOfUses++;

 3：给service对象的principal属性赋值

 4：将service对象放入map services

 

* ServiceTicket 的 grantTicketGrantingTicket方法

方法声明：

public TicketGrantingTicket grantTicketGrantingTicket(final String id, final Authentication authentication,final ExpirationPolicy expirationPolicy)

方法描述：在CAS3.3对CAS2.0协议的实现中，PGT是由ST签发的，调用的就是ServiceTicket的grantTicketGrantingTicket方法。方法返回的TicketGrantingTicket对象，表征的是一个PGT对象，其中的ticketGrantingTicket属性的值是签发ST的TGT对象。

 

* TicketGrantingTicket 的 expire方法

方法声明：void expire()

方法描述：

在CAS的logout接口实现中，要调用TGT对象的expire方法，然后会在缓存中清除此TGT对象。

expire方法的内容：循环遍历 services 中的Service对象，调用其logoutOfService方法。具体Service实现类中的logoutOfService方法的实现，要通知具体的应用，客户要退出。

 

### 三、TGT、ST、PGT、PT关系

1：ST是TGT签发的。用户在CAS上认证成功后，CAS生成TGT，用TGT签发一个ST，ST的ticketGrantingTicket属性值是TGT对象，然后把ST的值redirect到客户应用。

 

2：PGT是ST签发的。用户凭借ST去访问Proxy service，Proxy service去CAS验证ST（同时传递PgtUrl参数给CAS），如果ST验证成功，则CAS用ST签发一个PGT，PGT对象里的ticketGrantingTicket是签发ST的TGT对象。

 

3：PT是PGT签发的。Proxy service代理back-end service去CAS获取PT的时候，CAS根据传来的pgt参数，获取到PGT对象，然后调用其grantServiceTicket方法，生成一个PT对象。

 ![]()

TGT、ST、PGT、PT之间的关联关系

 

 

 

 

# CAS CLIENT篇

## CASFilter 参数说明：

参数
 
是否必须
 
说明 com.olymtech.cas.client.filter.loginUrl
 
是
 
指定 CAS 提供登录页面的 URL com.olymtech.cas.client.filter.validateUrl
 
是
 
指定 CAS 提供 service ticket 或 proxy ticket 验证服务的 URL com.olymtech.cas.client.filter.serverName
 
是
 
指定客户端的域名和端口，是指客户端应用所在机器而不是 CAS Server 所在机器，该参数或 serviceUrl 至少有一个必须指定 com.olymtech.cas.client.filter.serviceUrl
 
否
 
该参数指定过后将覆盖 serverName 参数，成为登录成功过后重定向的目的地址 com.olymtech.cas.client.filter.wrapRequest
 
否
 
如果指定为 true，那么 CASFilter 将重新包装 HttpRequest,并且使 getRemoteUser() 方法返回当前登录用户的用户名 com.olymtech.cas.client.filter.noFilter
 
否
 
设置不过滤的路径，语法如下：/name1/,/name2, com.olymtech.cas.client.filter.proxyCallbackUrl
 
否
 
用于当前应用需要作为其他服务的代理(proxy)时获取 Proxy Granting Ticket 的地址 com.olymtech.cas.client.filter.authorizedProxy
 
否
 
用于允许当前应用从代理处获取 proxy tickets，该参数接受以空格分隔开的多个 proxy URLs，但实际使用只需要一个成功即可。当指定该参数过后，需要修改 validateUrl 到 proxyValidate，而不再是 serviceValidate com.olymtech.cas.client.filter.renew
 
否
 
如果指定为 true，那么受保护的资源每次被访问时均要求用户重新进行验证，而不管之前是否已经通过 com.olymtech.cas.client.filter.gateway
 
否
 
指定 gateway 属性

 

 

 

# 应用整合篇

## 一、JAVA程序整合

**1、****应用配置说明（结合cas client****）**

导入key到本地jdk（tomcat使用的jdk必须）

keytool -import -keystore d:/work/jdk1.6.0_13/jre/lib/security/cacerts -file d:/casex/800jit.crt -alias 800jit

在ie中导入证书800jit.crt

修改web程序的web.xml添加

    <!-- 用于单点退出 -->

    <listener>

        <listener-class>org.jasig.cas.client.session.SingleSignOutHttpSessionListener</listener-class>

    </listener>

 

    <filter>

    <filter-name>CAS Single Sign Out Filter</filter-name>

    <filter-class>org.jasig.cas.client.session.SingleSignOutFilter</filter-class>

    </filter>

   

    <!-- 用于单点登录 -->

 

    <filter>

       <filter-name>CAS Filter</filter-name>

       <filter-class>com.olymtech.cas.client.filter.CASFilter</filter-class>

       <init-param>

           <param-name>com.olymtech.cas.client.filter.loginUrl</param-name>

           <param-value>https://cas.server.com/cas/login</param-value>

       </init-param>

       <init-param>

           <param-name>com.olymtech.cas.client.filter.validateUrl</param-name>

           <param-value>https://cas.server.com/cas/serviceValidate</param-value>

       </init-param>

       <init-param>

           <param-name>com.olymtech.cas.client.filter.serverName</param-name>

           <param-value>saas.server.com</param-value>

       </init-param>

       <init-param>

           <param-name>com.olymtech.cas.client.filter.wrapRequest</param-name>

           <param-value>true</param-value>

       </init-param>

    </filter>

 

    <filter-mapping>

       <filter-name>CAS Single Sign Out Filter</filter-name>

       <url-pattern>/*</url-pattern>

    </filter-mapping>

 

    <filter-mapping>

       <filter-name>CAS Filter</filter-name>

       <url-pattern>/*</url-pattern>

    </filter-mapping>

 

在jsp/servlet中得到登陆数据：

<%@ page language="java" import="com.olymtech.cas.client.filter.CASFilterRequestWrapper"%>

<%

CASFilterRequestWrapper  reqWrapper=**new** CASFilterRequestWrapper(request);

String username = reqWrapper.getRemoteUser();

%>

 

## 二、.NET程序整合

 

 

 

# 附件

ssl.rar（证书）

http.conf（apache配置文件）

httpd-ssl.conf（apache配置文件）

httpd-vhosts.conf（apache配置文件）

server.xml（tomcat配置文件）

AppSsoDemo-java.rar（java整合例子，包含所需jar包）

posted on 2010-08-05 17:23 [horus](http://www.cnblogs.com/hellowood/) 阅读(2394) 评论(2) [编辑](http://www.cnblogs.com/hellowood/admin/EditPosts.aspx?postid=1793364) [收藏](http://www.cnblogs.com/hellowood/archive/2010/08/05/1793364.html#)
![]()

[刷新评论]()[刷新页面](http://www.cnblogs.com/hellowood/archive/2010/08/05/1793364.html#)[返回顶部](http://www.cnblogs.com/hellowood/archive/2010/08/05/1793364.html#top)

[程序员问答社区，解决您的技术难题](http://q.cnblogs.com/)
[博客园首页](http://www.cnblogs.com/ "程序员的网上家园")[博问](http://q.cnblogs.com/ "程序员问答社区")[新闻](http://news.cnblogs.com/ "IT新闻")[闪存](http://home.cnblogs.com/ing/)[程序员招聘](http://job.cnblogs.com/)[知识库](http://kb.cnblogs.com/)
Powered by: [博客园](http://www.cnblogs.com/) 模板提供：[沪江博客](http://blog.hjenglish.com/) Copyright ©2012 horus
