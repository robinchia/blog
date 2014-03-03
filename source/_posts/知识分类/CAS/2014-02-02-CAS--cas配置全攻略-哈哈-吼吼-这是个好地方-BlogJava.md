---
layout: post
title: "cas配置全攻略 - 哈哈-吼吼-这是个好地方 - BlogJava"
categories: CAS
tags: 
 - CAS
--- 

# cas配置全攻略 - 哈哈,吼吼,这是个好地方 - BlogJava

[哈哈,吼吼,这是个好地方](http://www.blogjava.net/tufanshu/)
没有风雨躲的过， 没有坎坷不必走……

[BlogJava](http://www.blogjava.net/)    [首页](http://www.blogjava.net/tufanshu/)    [新随笔](http://www.blogjava.net/tufanshu/admin/EditPosts.aspx?opt=1)    [联系](http://www.blogjava.net/tufanshu/contact.aspx?id=1)    [聚合](http://www.blogjava.net/tufanshu/rss)[![]()](http://www.blogjava.net/tufanshu/rss)    [管理](http://www.blogjava.net/tufanshu/admin/EditPosts.aspx)

posts - 37,  comments - 50,  trackbacks - 0

[cas配置全攻略]()

经过将近两天的测试，参考众多网友的贡献，终于完成了对cas的主要配置和测试，现记录如下

基本需求：

1.cas server-3.4.5,casclient-3.2（官方版本），均可在cas官方网站下载，[http://www.jasig.org](http://www.jasig.org/)

2.使用低成本的http协议进行传输，俺买不起ssl证书

3.通过jdbc进行用户验证

4.需要通过casserver提供除登录用户名以外的附加信息

参考资料：

1.cas官方网站的用户帮助手册和wiki

2.网友“城市猎人”的blog，[http://yuzhwe.javaeye.com/blog/830143](http://yuzhwe.javaeye.com/blog/830143)

3.网友“悟空悟道”的blog，[http://llhdf.javaeye.com/blog/764385](http://llhdf.javaeye.com/blog/764385)

4.其他网友贡献的相关的blog，都是通过google出来，就不一一列出了，一并致谢！！！

好了，下面进入正题，如果您不想测试中出现异常情况，或是获取不到相关数据，请关注文中的红色字体部分。

（1）使用http协议的设置，如果您也像我一样，买不起ssl数字证书，对安全的要求也不是特别的搞，下面的配置就可以帮助解决这个问题：

在cas-server-webapp中的/WEB-INF/spring-configuration/ticketGrantingTicketCookieGenerator.xml文件中有如下配置

<bean id="ticketGrantingTicketCookieGenerator" class="org.jasig.cas.web.support.CookieRetrievingCookieGenerator"
  p:cookieSecure="true"     //默认为true，使用https,如果只需要http，修改为false即可
  p:cookieMaxAge="-1"
  p:cookieName="CASTGC"
  p:cookiePath="/cas" />

 （2）使用jdbc数据源进行用户认证，需要修改cas的authenticationHandlers方式，在文件/WEB-INF/deployerConfigContext.xml有如下配置：

<property name="authenticationHandlers">
   <list>
    <!--
     | This is the authentication handler that authenticates services by means of callback via SSL, thereby validating
     | a server side SSL certificate.
     +-->
    <bean class="org.jasig.cas.authentication.handler.support.HttpBasedServiceCredentialsAuthenticationHandler"
     p:httpClient-ref="httpClient" />
    <!--
     | This is the authentication handler declaration that every CAS deployer will need to change before deploying CAS
     | into production.  The default SimpleTestUsernamePasswordAuthenticationHandler authenticates UsernamePasswordCredentials
     | where the username equals the password.  You will need to replace this with an AuthenticationHandler that implements your
     | local authentication strategy.  You might accomplish this by coding a new such handler and declaring
     | edu.someschool.its.cas.MySpecialHandler here, or you might use one of the handlers provided in the adaptors modules.
     +-->
    <!--<bean class="org.jasig.cas.authentication.handler.support.SimpleTestUsernamePasswordAuthenticationHandler" />-->
     <bean  class="org.jasig.cas.adaptors.jdbc.QueryDatabaseAuthenticationHandler">
         <property name="dataSource" ref="dataSource" />
        <property name="sql" value="select password from userInfo where username=? and enabled=true" />
         //用户密码编码方式
         <property name="passwordEncoder"
           ref="passwordEncoderBean"/>
         </bean>  
   </list>
  </property>

该属性中的list只要用一个认证通过即可，建议将红色部分放在第一位，如果确认只用jdbc一种方式，其他认证方式均可删除。另外需要在在文件中添加datasoure和passordEncoder两个bean，如下

<!-- Data source definition -->
 <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
  <property name="driverClassName">
    <value>com.mysql.jdbc.Driver</value>
  </property>
  <property name="url">
    <value>jdbc:mysql://localhost:3306/test?useUnicode=true&amp;characterEncoding=utf-8</value>    //如果使用mysql数据库，应该加上后面的编码参数，否则可能导致客户端对TGT票据无法识别的问题
  </property>
  <property name="username"><value>root</value></property>
  <property name="password"><value>password</value></property>
 </bean>
 <bean id="passwordEncoderBean" class="org.jasig.cas.authentication.handler.DefaultPasswordEncoder">
        <constructor-arg value="SHA1" />  //cas
server默认支持MD5和SHA1两种编码方式，如果需要其他的编码方式例如SHA256,512等，可自行实现org.jasig.cas.authentication.handler.PasswordEncoder接口
    </bean>

附加备注：如果您是使用cas server的源码自行编译的话，需要在cas-server-web模块的pom.xml中添加如下模块的依赖：

<dependency>
       <groupId>${project.groupId}</groupId>
       <artifactId>cas-server-support-jdbc</artifactId>
       <version>${project.version}</version>
  </dependency>  

并添加对应数据库的jdbc的jar包。

（3）让cas server提供更多的用户数据共客户端使用

通过测试，由于cas的代码更新过程中的变化较大，所以包兼容的问题好像一直存在，在测试中我就碰到过，花费时间比较多，建议同学们在使用过程中使用官方的最新的发布版本。在我使用的这个版本中，请参考前面的关于server和client端的版本说明，应该没有包冲突的问题，测试通过。下面进行配置，配置文件：/WEB-INF/deployerConfigContext.xml
<property name="credentialsToPrincipalResolvers">
   <list>
       <!--<bean class="org.jasig.cas.authentication.principal.UsernamePasswordCredentialsToPrincipalResolver" />-->
    <!-- modify on 2011-01-18,add user info -->
    <bean class="org.jasig.cas.authentication.principal.UsernamePasswordCredentialsToPrincipalResolver" >
      <property name="attributeRepository" >   //为认证过的用户的Principal添加属性
      <ref local="attributeRepository"/>
     </property> 
    </bean>
      <bean
     class="org.jasig.cas.authentication.principal.HttpBasedServiceCredentialsToPrincipalResolver" />
   </list>
  </property>
 修改该文件中默认的 attributeRepositorybean配置
<!-- 在这里配置获取更多用户的信息 -->
 <bean id="attributeRepository" class="org.jasig.services.persondir.support.jdbc.SingleRowJdbcPersonAttributeDao">
  <constructor-arg index="0" ref="dataSource" />
  <constructor-arg index="1" value="select id as UId, password_hint as ph from userInfo where username=? and enabled=true" />
  <property name="queryAttributeMapping">
   <map>
   <entry key="username" value="uid"/><!-- 这里必须这么写，系统会自己匹配，貌似和where语句后面的用户名字段的拼写没有什么关系 -->
   </map>
  </property>
   <!-- 要获取的属性在这里配置 -->
  <property name="resultAttributeMapping">
   <map>
   <entry key="UId" value="userId" /> //key为对应的数据库字段名称，value为提供给客户端获取的属性名字，系统会自动填充值
   <entry key="ph" value="passwordHint" />   
   </map>
  </property>
</bean> 
备注：网上有很多的关于这个的配置，但是如果您使用的是我提供的版本或是高于这个版本，就应该象上面这样配置，无用质疑，网上大部分的配置都是基于
person-directory-impl,person-directory-api
1.1左右的版本，而最新的cas使用的是1.5的版本，经过查看源代码和api docs确定最新版本的属性参数如上配置。

修改该xml文件中最后一个默认的serviceRegistryDao bean中的属性全部注释掉，或者删除，
这个bean中的RegisteredServiceImpl的ignoreAttributes属性将决定是否添加attributes属性内容，默认为false:不添加，只有去掉这个配置，
cas server才会将获取的用户的附加属性添加到认证用的Principal的attributes中去，我在这里犯过这样的错误，最后还是通过跟踪源码才发现的。
<bean
  id="serviceRegistryDao"
        class="org.jasig.cas.services.InMemoryServiceRegistryDaoImpl">
        <!--
            <property name="registeredServices">
                <list>
                    <bean class="org.jasig.cas.services.RegisteredServiceImpl">
                        <property name="id" value="0" />
                        <property name="name" value="HTTP" />
                        <property name="description" value="Only Allows HTTP Urls" />
                        <property name="serviceId" value="http://**" />
                    </bean>

                    <bean class="org.jasig.cas.services.RegisteredServiceImpl">
                        <property name="id" value="1" />
                        <property name="name" value="HTTPS" />
                        <property name="description" value="Only Allows HTTPS Urls" />
                        <property name="serviceId" value="https://**" />
                    </bean>

                    <bean class="org.jasig.cas.services.RegisteredServiceImpl">
                        <property name="id" value="2" />
                        <property name="name" value="IMAPS" />
                        <property name="description" value="Only Allows HTTPS Urls" />
                        <property name="serviceId" value="imaps://**" />
                    </bean>

                    <bean class="org.jasig.cas.services.RegisteredServiceImpl">
                        <property name="id" value="3" />
                        <property name="name" value="IMAP" />
                        <property name="description" value="Only Allows IMAP Urls" />
                        <property name="serviceId" value="imap://**" />
                    </bean>
                </list>
            </property>-->
           </bean>

 修改WEB-INF\view\jsp\protocol\2.0\casServiceValidationSuccess.jsp文件，如下：

<%@ page session="false"%>
<%@ taglib prefix="c" uri="[http://java.sun.com/jsp/jstl/core"%](http://java.sun.com/jsp/jstl/core%22%)>
<%@ taglib uri="[http://java.sun.com/jsp/jstl/functions](http://java.sun.com/jsp/jstl/functions)" prefix="fn"%>
<cas:serviceResponse xmlns:cas='http://www.yale.edu/tp/cas'>
 <cas:authenticationSuccess>
  <cas:user>${fn:escapeXml(assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.id)}</cas:user>
  <c:if test="${not empty pgtIou}">
   <cas:proxyGrantingTicket>${pgtIou}</cas:proxyGrantingTicket>
  </c:if>
  <c:if test="${fn:length(assertion.chainedAuthentications) > 1}">
   <cas:proxies>
    <c:forEach var="proxy" items="${assertion.chainedAuthentications}"
     varStatus="loopStatus" begin="0"
     end="${fn:length(assertion.chainedAuthentications)-2}" step="1">
     <cas:proxy>${fn:escapeXml(proxy.principal.id)}</cas:proxy>
    </c:forEach>
   </cas:proxies>
  </c:if>
   <c:if
   test="${fn:length(assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.attributes)
>
0}">
   <cas:attributes>
    <c:forEach
var="attr"
     items="${assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.attributes}"
     varStatus="loopStatus"
begin="0"
     end="${fn:length(assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.attributes)-1}"
     step="1">
     <cas:${fn:escapeXml(attr.key)}>${fn:escapeXml(attr.value)}</cas:${fn:escapeXml(attr.key)}>
    </c:forEach>
   </cas:attributes>
  </c:if>
 </cas:authenticationSuccess>
</cas:serviceResponse>
客户端配置:
1.过滤器CAS Validation Filter：
<filter>
  <filter-name>CAS Validation Filter</filter-name>
  <filter-class> org.jasig.cas.client.validation.Cas20ProxyReceivingTicketValidationFilter</filter-class>
  <init-param>
    <param-name>casServerUrlPrefix</param-name>
    <param-value>http://domainserver:8081/cas</param-value>
  </init-param>
</filter>
在客户端获取信息
AttributePrincipal principal = (AttributePrincipal) request.getUserPrincipal();
String loginName = principal.getName();//获取用户名
Map<String, Object> attributes = principal.getAttributes();
if(attributes != null) {
 System.out.println(attributes.get("userId"));
 System.out.println(attributes.get("passwordHint"));
}

 

 
posted on 2011-01-21 10:06 [雪地孤鸿](http://www.blogjava.net/tufanshu/) 阅读(3704) [评论(7)](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#Post)  [编辑](http://www.blogjava.net/tufanshu/admin/EditPosts.aspx?postid=343290)  [收藏](http://www.blogjava.net/tufanshu/AddToFavorite.aspx?id=343290) 所属分类: [java](http://www.blogjava.net/tufanshu/category/4974.html) ![]()

[]()
**FeedBack:**

[#](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#343395 "permalink: re: cas配置全攻略") []()re: cas配置全攻略

2011-01-23 13:13 | [来如风]()
非常好，我上周也配置成功
只是最后过滤器有疑问
<init-param>
<param-name>casServerUrlPrefix</param-name>
<param-value>[http://domainserver:8081/cas</param-value>](http://domainserver:8081/cas%3C/param-value%3E)
</init-param>
我是集成了spring security 的配置，貌似这些不在过滤其中配置，在bean文件中有  [回复](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#post)  [更多评论](http://www.blogjava.net/comment?author=%e6%9d%a5%e5%a6%82%e9%a3%8e "查看该作者发表过的评论")
[]()  []()
[#](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#343442 "permalink: re: cas配置全攻略") []()re: cas配置全攻略

2011-01-24 16:29 | [coolszy]()
你好，按照你的配置，但是
Map<String, Object> attributes = principal.getAttributes();
还是得到空值，不知道什么原因  [回复](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#post)  [更多评论](http://www.blogjava.net/comment?author=coolszy "查看该作者发表过的评论")
[]()  []()

[#](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#343924 "permalink: re: cas配置全攻略[未登录]") []()re: cas配置全攻略[未登录]

2011-02-07 11:02 | [mcikeay]()
<entry key="username" value="uid"/><!-- 这里必须这么写，系统会自己匹配，貌似和where语句后面的用户名字段的拼写没有什么关系 -->
cas 会自己组装<map>节点中的key-value, 节点间默认是 and 连接  [回复](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#post)  [更多评论](http://www.blogjava.net/comment?author=mcikeay "查看该作者发表过的评论")
[]()  []()
[#](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#344268 "permalink: re: cas配置全攻略") []()re: cas配置全攻略

2011-02-14 15:15 | [tufanshu](http://040922/)
@coolszy
请检查cas client的版本是否正确  [回复](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#post)  [更多评论](http://www.blogjava.net/comment?author=tufanshu "查看该作者发表过的评论")
[]()  []()

[#](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#347172 "permalink: re: cas配置全攻略") []()re: cas配置全攻略

2011-03-29 10:45 | [wuximin]()
通过ldap验证，改怎样修改呢  [回复](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#post)  [更多评论](http://www.blogjava.net/comment?author=wuximin "查看该作者发表过的评论")
[]()  []()
[#](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#351783 "permalink: re: cas配置全攻略") []()re: cas配置全攻略

2011-06-05 18:19 | [artd](http://xn--rgv/)
按您的要求，为何还是不行呀，不知道那里出问题了。
不过我用的server是3.3.5.1
用3.4.5也不行，有空交流一下哟。
以下是log信息
2011-06-05 18:09:19,515 DEBUG [org.jasig.cas.authentication.principal.UsernamePasswordCredentialsToPrincipalResolver] - Attempting to resolve a principal...
2011-06-05 18:09:19,515 DEBUG [org.jasig.cas.authentication.principal.UsernamePasswordCredentialsToPrincipalResolver] - Creating SimplePrincipal for [test]
2011-06-05 18:09:20,421 DEBUG [org.jasig.services.persondir.support.jdbc.SingleRowJdbcPersonAttributeDao] - Created seed map='{username=[test]}' for uid='test'
2011-06-05 18:09:20,421 DEBUG [org.jasig.services.persondir.support.jdbc.SingleRowJdbcPersonAttributeDao] - Adding attribute 'username' with value '[test]' to query builder 'null'
2011-06-05 18:09:20,421 DEBUG [org.jasig.services.persondir.support.jdbc.SingleRowJdbcPersonAttributeDao] - Generated query builder 'sql=[username = ?] args=[test]' from query Map {username=[test]}.
2011-06-05 18:09:20,546 DEBUG [org.jasig.services.persondir.support.jdbc.SingleRowJdbcPersonAttributeDao] - Executed 'select id,username,password,email,score from user where username = ?' with arguments [test] and got results [{id=20428, username=test, password=1a16128849444873, email=et@163.com, score=6}]
2011-06-05 18:09:20,562 INFO [org.jasig.cas.CentralAuthenticationServiceImpl] - Granted service ticket [ST-1-cp3cb14yiqbqnYfg7C1f-cas] for service [[http://d7.abc.com/simple.php](http://d7.abc.com/simple.php)] for user [test]
2011-06-05 18:09:20,640 DEBUG [org.springframework.web.servlet.view.RedirectView] - Rendering view with name 'null' with model null and static attributes {}
2011-06-05 18:09:20,703 DEBUG [org.springframework.web.servlet.view.ResourceBundleViewResolver] - Cached view [casServiceSuccessView_zh_CN]
2011-06-05 18:09:20,703 DEBUG [org.springframework.web.servlet.view.JstlView] - Rendering view with name 'casServiceSuccessView' with model {assertion=[principals={[[Principal=test, attributes={authenticationMethod=com.jute.framework.jdbc.QueryDatabaseAuthenticationHandler}]]} for service=[http://d7.abc.com/simple.php](http://d7.abc.com/simple.php)]} and static attributes {}
2011-06-05 18:09:20,750 DEBUG [org.springframework.web.servlet.view.JstlView] - Added model object 'assertion' of type [org.jasig.cas.validation.ImmutableAssertionImpl] to request in view with name 'casServiceSuccessView'
2011-06-05 18:09:20,750 DEBUG [org.springframework.web.servlet.view.JstlView] - Forwarding to resource [/WEB-INF/view/jsp/protocol/2.0/casServiceValidationSuccess.jsp] in InternalResourceView 'casServiceSuccessView'
  [回复](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#post)  [更多评论](http://www.blogjava.net/comment?author=artd "查看该作者发表过的评论")
[]()  []()

[#](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#361580 "permalink: re: cas配置全攻略") []()re: cas配置全攻略[]()

2011-10-19 15:24 | [fanfree]()
同样遇到 Map是空的，没有附加属性  [回复](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html#post)  [更多评论](http://www.blogjava.net/comment?author=fanfree "查看该作者发表过的评论")
[]()  []()
[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [博问 - 解决您的IT难题](http://q.cnblogs.com/)   [博客园](http://www.cnblogs.com/)  [博问](http://q.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html&SourceURL=/tufanshu/archive/2011/01/21/343290.html)       [使用Ctrl+Enter键可以直接提交]    IT新闻：
· [苹果在移动支付领域之慢，是福是祸？](http://news.cnblogs.com/n/149115/)
· [黑莓应用商店应用下载量突破30亿次，应用数量达9万款](http://news.cnblogs.com/n/149114/)
· [微软将推出Bing天使基金孵化器](http://news.cnblogs.com/n/149113/)
· [苹果在华测试iPad mini：微胖 具备通话功能](http://news.cnblogs.com/n/149112/)
· [抱上微软粗腿不怕！诺基亚豪言明年定走出困境](http://news.cnblogs.com/n/149111/)

博客园首页随笔：
· [MVC3使用Unity实现依赖注入接口与于实现类自动注册](http://www.cnblogs.com/lzrabbit/archive/2012/07/09/2574491.html)
· [k-means clustering K平均算法](http://www.cnblogs.com/stoic/archive/2012/07/09/2582212.html)
· [【hdu - 1061 Rightmost Digit】](http://www.cnblogs.com/izumu/archive/2012/07/09/2582200.html)
· [SqlServer 对 数据类型 text 的操作](http://www.cnblogs.com/refactor/archive/2012/07/09/2575995.html)
· [Step By Step(Lua弱引用table)](http://www.cnblogs.com/stephen-liu74/archive/2012/07/09/2423565.html)
知识库：
· [页面前端的水有多深？再议页面开发](http://kb.cnblogs.com/page/143279/)
· [Fiddler 教程](http://kb.cnblogs.com/page/130367/)
· [关于编程学习的七点思索](http://kb.cnblogs.com/page/148772/)
· [一地鸡毛 — 软件项目中的人际困局](http://kb.cnblogs.com/page/115175/)
· [互联网——降级论](http://kb.cnblogs.com/page/148841/)  网站导航:

[博客园](http://www.cnblogs.com/ "程序员的网上家园")   [IT新闻](http://news.cnblogs.com/)   [知识库](http://kb.cnblogs.com/)   [C++博客](http://www.cppblog.com/)   [程序员招聘](http://job.cnblogs.com/)   [管理](http://www.blogjava.net/tufanshu/archive/2011/01/21/343290.html?opt=admin) 相关文章:

* [cas server 3.4.5 部署在weblogic问题记录](http://www.blogjava.net/tufanshu/archive/2011/02/09/343972.html)
* [cas server使用mysql数据库和oralce数据库的差异](http://www.blogjava.net/tufanshu/archive/2011/01/26/343543.html)
* [cas server logout的问题](http://www.blogjava.net/tufanshu/archive/2011/01/21/343318.html)
* [cas配置全攻略]()
* [JForum 的 SSO集成的问题解决](http://www.blogjava.net/tufanshu/archive/2008/06/26/210852.html)
* [apache2.2.6+tomcat5.5.17配置说明](http://www.blogjava.net/tufanshu/archive/2007/12/24/170126.html)
* [找不到C.TLD的问题](http://www.blogjava.net/tufanshu/archive/2006/12/04/85371.html)
* [roller2.3源代码部署笔录](http://www.blogjava.net/tufanshu/archive/2006/11/01/78479.html)
* [tomcat 服务器抛出socket异常“文件打开太多”的问题](http://www.blogjava.net/tufanshu/archive/2006/07/31/60981.html)
* [hsql的使用](http://www.blogjava.net/tufanshu/archive/2005/12/26/25499.html)  
Copyright ©2012 雪地孤鸿 Powered By: [博客园](http://www.cnblogs.com/) 模板提供：[沪江博客](http://blog.hjenglish.com/)
[<]( "Go to the previous month")2011年1月[>]( "Go to the next month")日一二三四五六2627282930311234567891011121314151617181920[21](http://www.blogjava.net/tufanshu/archive/2011/01/21.html)22232425[26](http://www.blogjava.net/tufanshu/archive/2011/01/26.html)272829303112345

### 常用链接

* [我的随笔](http://www.blogjava.net/tufanshu/MyPosts.html)
* [我的评论](http://www.blogjava.net/tufanshu/MyComments.html)
* [我的参与](http://www.blogjava.net/tufanshu/OtherPosts.html)
* [最新评论](http://www.blogjava.net/tufanshu/RecentComments.html)

### 留言簿(13)

* [给我留言](http://www.blogjava.net/tufanshu/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/tufanshu/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/tufanshu/admin/MyMessages.aspx)

# 随笔分类

* [appfuse(1)](http://www.blogjava.net/tufanshu/category/9675.html)[![]( "Subscribe to appfuse(1)")](http://www.blogjava.net/tufanshu/category/9675.html/rss "Subscribe to appfuse(1)")
* [CRM&ERP(1)](http://www.blogjava.net/tufanshu/category/33305.html)[![]( "Subscribe to CRM&ERP(1)")](http://www.blogjava.net/tufanshu/category/33305.html/rss "Subscribe to CRM&ERP(1)")
* [Grails(3)](http://www.blogjava.net/tufanshu/category/44350.html)[![]( "Subscribe to Grails(3)")](http://www.blogjava.net/tufanshu/category/44350.html/rss "Subscribe to Grails(3)")
* [java(11)](http://www.blogjava.net/tufanshu/category/4974.html)[![]( "Subscribe to java(11)")](http://www.blogjava.net/tufanshu/category/4974.html/rss "Subscribe to java(11)")
* [ofbiz学习(1)](http://www.blogjava.net/tufanshu/category/41549.html)[![]( "Subscribe to ofbiz学习(1)")](http://www.blogjava.net/tufanshu/category/41549.html/rss "Subscribe to ofbiz学习(1)")
* [seam2.0学习笔记(1)](http://www.blogjava.net/tufanshu/category/30111.html)[![]( "Subscribe to seam2.0学习笔记(1)")](http://www.blogjava.net/tufanshu/category/30111.html/rss "Subscribe to seam2.0学习笔记(1)")
* [SPRING](http://www.blogjava.net/tufanshu/category/4243.html)[![]( "Subscribe to SPRING")](http://www.blogjava.net/tufanshu/category/4243.html/rss "Subscribe to SPRING")
* [sso(3)](http://www.blogjava.net/tufanshu/category/30359.html)[![]( "Subscribe to sso(3)")](http://www.blogjava.net/tufanshu/category/30359.html/rss "Subscribe to sso(3)")
* [STRUTS(1)](http://www.blogjava.net/tufanshu/category/4244.html)[![]( "Subscribe to STRUTS(1)")](http://www.blogjava.net/tufanshu/category/4244.html/rss "Subscribe to STRUTS(1)")
* [Sync4j(1)](http://www.blogjava.net/tufanshu/category/3522.html)[![]( "Subscribe to Sync4j(1)")](http://www.blogjava.net/tufanshu/category/3522.html/rss "Subscribe to Sync4j(1)")
* [SyncML(2)](http://www.blogjava.net/tufanshu/category/3293.html)[![]( "Subscribe to SyncML(2)")](http://www.blogjava.net/tufanshu/category/3293.html/rss "Subscribe to SyncML(2)")
* [大战SOA & MULE](http://www.blogjava.net/tufanshu/category/36144.html)[![]( "Subscribe to 大战SOA & MULE")](http://www.blogjava.net/tufanshu/category/36144.html/rss "Subscribe to 大战SOA & MULE")
* [工作日志(2)](http://www.blogjava.net/tufanshu/category/15210.html)[![]( "Subscribe to 工作日志(2)")](http://www.blogjava.net/tufanshu/category/15210.html/rss "Subscribe to 工作日志(2)")
* [开发工具(3)](http://www.blogjava.net/tufanshu/category/4242.html)[![]( "Subscribe to 开发工具(3)")](http://www.blogjava.net/tufanshu/category/4242.html/rss "Subscribe to 开发工具(3)")
* [生活感悟(1)](http://www.blogjava.net/tufanshu/category/22461.html)[![]( "Subscribe to 生活感悟(1)")](http://www.blogjava.net/tufanshu/category/22461.html/rss "Subscribe to 生活感悟(1)")
* [系统维护(3)](http://www.blogjava.net/tufanshu/category/40597.html)[![]( "Subscribe to 系统维护(3)")](http://www.blogjava.net/tufanshu/category/40597.html/rss "Subscribe to 系统维护(3)")

# 随笔档案

* [2011年2月 (2)](http://www.blogjava.net/tufanshu/archive/2011/02.html)
* [2011年1月 (3)](http://www.blogjava.net/tufanshu/archive/2011/01.html)
* [2010年12月 (1)](http://www.blogjava.net/tufanshu/archive/2010/12.html)
* [2010年5月 (1)](http://www.blogjava.net/tufanshu/archive/2010/05.html)
* [2010年3月 (1)](http://www.blogjava.net/tufanshu/archive/2010/03.html)
* [2009年12月 (1)](http://www.blogjava.net/tufanshu/archive/2009/12.html)
* [2009年9月 (1)](http://www.blogjava.net/tufanshu/archive/2009/09.html)
* [2009年8月 (1)](http://www.blogjava.net/tufanshu/archive/2009/08.html)
* [2009年7月 (1)](http://www.blogjava.net/tufanshu/archive/2009/07.html)
* [2009年5月 (1)](http://www.blogjava.net/tufanshu/archive/2009/05.html)
* [2009年4月 (1)](http://www.blogjava.net/tufanshu/archive/2009/04.html)
* [2009年2月 (1)](http://www.blogjava.net/tufanshu/archive/2009/02.html)
* [2008年7月 (1)](http://www.blogjava.net/tufanshu/archive/2008/07.html)
* [2008年6月 (1)](http://www.blogjava.net/tufanshu/archive/2008/06.html)
* [2008年3月 (3)](http://www.blogjava.net/tufanshu/archive/2008/03.html)
* [2007年12月 (1)](http://www.blogjava.net/tufanshu/archive/2007/12.html)
* [2007年8月 (1)](http://www.blogjava.net/tufanshu/archive/2007/08.html)
* [2007年7月 (1)](http://www.blogjava.net/tufanshu/archive/2007/07.html)
* [2006年12月 (1)](http://www.blogjava.net/tufanshu/archive/2006/12.html)
* [2006年11月 (1)](http://www.blogjava.net/tufanshu/archive/2006/11.html)
* [2006年10月 (1)](http://www.blogjava.net/tufanshu/archive/2006/10.html)
* [2006年9月 (1)](http://www.blogjava.net/tufanshu/archive/2006/09.html)
* [2006年7月 (1)](http://www.blogjava.net/tufanshu/archive/2006/07.html)
* [2006年4月 (2)](http://www.blogjava.net/tufanshu/archive/2006/04.html)
* [2005年12月 (1)](http://www.blogjava.net/tufanshu/archive/2005/12.html)
* [2005年11月 (1)](http://www.blogjava.net/tufanshu/archive/2005/11.html)
* [2005年10月 (4)](http://www.blogjava.net/tufanshu/archive/2005/10.html)
* [2005年9月 (1)](http://www.blogjava.net/tufanshu/archive/2005/09.html)

# 文章档案

* [2005年9月 (1)](http://www.blogjava.net/tufanshu/archives/2005/09.html)

# blog

* [eamoi之Coder日志](http://www.blogjava.net/eamoi/)
* ajax
* [liferayPortal](http://www.blogjava.net/eamoi)
* [steeven](http://steeven.cnblogs.com/)
* [sync4j](http://www.cnblogs.com/zhengyun_ustc/)
* [sync4j1](http://www.u2o.net/blog/default.asp)
* [大胃](http://www.blogjava.net/sean)
* [月亮的太阳](http://www.blogjava.net/zyb9114/)
* [铁手剑谱](http://www.blogjava.net/SteelHand)
* 铁手

### 搜索

*
*  

### 最新评论 [![]()](http://www.blogjava.net/tufanshu/CommentsRSS.aspx)

* [1. re: cas server 3.4.5 部署在weblogic问题记录[未登录]](http://www.blogjava.net/tufanshu/archive/2011/11/02/343972.html#362546)
* 评论内容较长,点击标题查看
* --大雄
* [2. re: cas配置全攻略](http://www.blogjava.net/tufanshu/archive/2011/10/19/343290.html#361580)
* 同样遇到 Map是空的，没有附加属性
* --fanfree
* [3. re: cas server logout的问题](http://www.blogjava.net/tufanshu/archive/2011/06/19/343318.html#352630)
* 加了这个还是停在原来的退出页面，
* --artd
* [4. re: cas配置全攻略](http://www.blogjava.net/tufanshu/archive/2011/06/05/343290.html#351783)
* 评论内容较长,点击标题查看
* --artd
* [5. re: cas配置全攻略](http://www.blogjava.net/tufanshu/archive/2011/03/29/343290.html#347172)
* 通过ldap验证，改怎样修改呢
* --wuximin

### 阅读排行榜

* [1. linux+php5.1.6+mysql5.0.2+apache2.0.55安装配置说明：(12245)](http://www.blogjava.net/tufanshu/archive/2006/10/20/76389.html)
* [2. cas配置全攻略(3704)]()
* [3. 在tomcat下发布使用eclipse中的ant打包后的war文件(3490)](http://www.blogjava.net/tufanshu/archive/2006/09/14/69691.html)
* [4. cas配置之初体验(3313)](http://www.blogjava.net/tufanshu/archive/2008/03/25/188552.html)
* [5. opencrx安装手记(2914)](http://www.blogjava.net/tufanshu/archive/2008/07/26/217681.html)

### 评论排行榜

* [1. cas配置全攻略(7)]()
* [2. linux+php5.1.6+mysql5.0.2+apache2.0.55安装配置说明：(4)](http://www.blogjava.net/tufanshu/archive/2006/10/20/76389.html)
* [3. SyncML Intensive 继续(4)](http://www.blogjava.net/tufanshu/archive/2005/10/15/15574.html)
* [4. jdk1.4升级到JDK1.5的问题(3)](http://www.blogjava.net/tufanshu/archive/2005/11/17/20309.html)
* [5. ofbiz9 + oracle 11g(3)](http://www.blogjava.net/tufanshu/archive/2009/09/03/293787.html)
