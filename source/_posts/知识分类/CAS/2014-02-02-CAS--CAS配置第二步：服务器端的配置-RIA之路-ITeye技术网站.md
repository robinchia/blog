---
layout: post
title: "CAS配置第二步：服务器端的配置 - RIA之路 - ITeye技术网站"
categories: CAS
tags: 
 - CAS
--- 

# CAS配置第二步：服务器端的配置 - RIA之路 - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼](http://andy-ghg.iteye.com/blog/942653#)

[招聘](http://www.iteye.com/job) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://andy-ghg.iteye.com/login "登录") [登录](http://andy-ghg.iteye.com/login) [注册](http://andy-ghg.iteye.com/signup)

# [RIA之路](http://andy-ghg.iteye.com/)

* [**博客**](http://andy-ghg.iteye.com/)
* [微博](http://andy-ghg.iteye.com/weibo)
* [相册](http://andy-ghg.iteye.com/album)
* [收藏](http://andy-ghg.iteye.com/link)
* [留言](http://andy-ghg.iteye.com/blog/guest_book)
* [关于我](http://andy-ghg.iteye.com/blog/profile)

### [CAS配置第二步：服务器端的配置]() **

**博客分类：**
* [Java](http://andy-ghg.iteye.com/category/146164)
[Oracle](http://www.iteye.com/blogs/tag/Oracle)[XML](http://www.iteye.com/blogs/tag/XML)[Tomcat](http://www.iteye.com/blogs/tag/Tomcat)[JSP](http://www.iteye.com/blogs/tag/JSP)[SQL Server](http://www.iteye.com/blogs/tag/SQL%20Server) 

紧接上一篇[CAS配置第一步：准备工作](http://andy-ghg.iteye.com/blog/942595)
**CAS Java 群35271653**
**1:加入数据库验证**
再将cas.war部署到tomcat-cas-server下之后，
将oracle数据库驱动加入到cas工程的lib下
打开目录X:\tomcat-cas-server\webapps\cas\WEB-INF下的deployerConfigContext.xml修改里面的内容，注释掉以下代码：
Xml代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <bean class="org.jasig.cas.authentication.handler.support.SimpleTestUsernamePasswordAuthenticationHandler"  
<bean class="org.jasig.cas.authentication.handler.support.SimpleTestUsernamePasswordAuthenticationHandler"
这段代码CAS默认的验证方式，就是用户名和密码相同即可通过认证，我们现在做的时候加入数据库验证，就是用户名密码来自数据库。
在相同的位置上加入如下代码：
Xml代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <bean class="org.jasig.cas.adaptors.jdbc.QueryDatabaseAuthenticationHandler">  
1.     <property name="sql" value="select pwd from portal_user where id=? and sessionid is null" />  
1.     <property name="dataSource" ref="dataSource" />  
1. </bean>  
<bean class="org.jasig.cas.adaptors.jdbc.QueryDatabaseAuthenticationHandler"> <property name="sql" value="select pwd from portal_user where id=? and sessionid is null" /> <property name="dataSource" ref="dataSource" /> </bean>
其中，sql语句可以自定义成你想要的结果，例如：
Sql代码  [![收藏代码]()![]()]( "收藏这段代码")

1. select password from tablename where username=?  
select password from tablename where username=?
然后在根节点<beans>下加入节点如下：
Xml代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <bean id="dataSource"  
1.     class="org.springframework.jdbc.datasource.DriverManagerDataSource">  
1.     <property name="driverClassName">  
1.         <value>oracle.jdbc.driver.OracleDriver</value>  
1.     </property>  
1.     <property name="url">  
1.         <value>jdbc:oracle:thin:@xxxxxxxxxxxxx:1521:pisdb</value>  
1.     </property>  
1.     <property name="username">  
1.         <value>pis</value>  
1.     </property>  
1.     <property name="password">  
1.         <value>pis</value>  
1.     </property>  
1. </bean>  
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource"> <property name="driverClassName"> <value>oracle.jdbc.driver.OracleDriver</value> </property> <property name="url"> <value>jdbc:oracle:thin:@xxxxxxxxxxxxx:1521:pisdb</value> </property> <property name="username"> <value>pis</value> </property> <property name="password"> <value>pis</value> </property> </bean>
里面的参数配置，XML诠释的非常清楚，不再解释。
**2:让CAS返回更多用户信息XML修改**
然后找到serviceRegistryDao这个节点，将里面的内容全部注释掉：
Xml代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <bean id="serviceRegistryDao" class="org.jasig.cas.services.InMemoryServiceRegistryDaoImpl">  
1.     <!--<property name="registeredServices">  
1.         <list>  
1.             <bean class="org.jasig.cas.services.RegisteredServiceImpl">  
1.                 <property name="id" value="0" />  
1.                 <property name="name" value="HTTP" />  
1.                 <property name="description" value="Only Allows HTTP Urls" />  
1.                 <property name="serviceId" value="http://**" />  
1.             </bean>  
1.   
1.             <bean class="org.jasig.cas.services.RegisteredServiceImpl">  
1.                 <property name="id" value="1" />  
1.                 <property name="name" value="HTTPS" />  
1.                 <property name="description" value="Only Allows HTTPS Urls" />  
1.                 <property name="serviceId" value="https://**" />  
1.             </bean>  
1.   
1.             <bean class="org.jasig.cas.services.RegisteredServiceImpl">  
1.                 <property name="id" value="2" />  
1.                 <property name="name" value="IMAPS" />  
1.                 <property name="description" value="Only Allows HTTPS Urls" />  
1.                 <property name="serviceId" value="imaps://**" />  
1.             </bean>  
1.   
1.             <bean class="org.jasig.cas.services.RegisteredServiceImpl">  
1.                 <property name="id" value="3" />  
1.                 <property name="name" value="IMAP" />  
1.                 <property name="description" value="Only Allows IMAP Urls" />  
1.                 <property name="serviceId" value="imap://**" />  
1.             </bean>  
1.         </list>  
1.     </property>-->  
1. </bean>  
<bean id="serviceRegistryDao" class="org.jasig.cas.services.InMemoryServiceRegistryDaoImpl"> <!--<property name="registeredServices"> <list> <bean class="org.jasig.cas.services.RegisteredServiceImpl"> <property name="id" value="0" /> <property name="name" value="HTTP" /> <property name="description" value="Only Allows HTTP Urls" /> <property name="serviceId" value="http://**" /> </bean> <bean class="org.jasig.cas.services.RegisteredServiceImpl"> <property name="id" value="1" /> <property name="name" value="HTTPS" /> <property name="description" value="Only Allows HTTPS Urls" /> <property name="serviceId" value="https://**" /> </bean> <bean class="org.jasig.cas.services.RegisteredServiceImpl"> <property name="id" value="2" /> <property name="name" value="IMAPS" /> <property name="description" value="Only Allows HTTPS Urls" /> <property name="serviceId" value="imaps://**" /> </bean> <bean class="org.jasig.cas.services.RegisteredServiceImpl"> <property name="id" value="3" /> <property name="name" value="IMAP" /> <property name="description" value="Only Allows IMAP Urls" /> <property name="serviceId" value="imap://**" /> </bean> </list> </property>--> </bean>
此节点的作用为：如果你在<list>中加入了bean，并设定了serviceId的value，那么通过CAS你只能访问这个url地址，其他的url地址将不能访问，**代表的意思就是指该协议下所有的都允许被访问。但是在实际操作中，加入如果不注释掉里面的内容，将会在客户端无法获取到用户更多的登录信息。
然后在配置文件的根节点（<beans>）下加入以下XML配置：
Xml代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <bean id="attributeRepository" class="org.jasig.services.persondir.support.jdbc.SingleRowJdbcPersonAttributeDao">  
1.     <constructor-arg index="0" ref="dataSource"/>  
1.     <constructor-arg index="1" value="select * from portal_user where {0}" />  
1.     <property name="queryAttributeMapping">    
1.             <map>    
1.                 <entry key="username" value="id" />    
1.             </map>    
1.     </property>  
1.     <property name="resultAttributeMapping">  
1.         <map>  
1.             <entry key="id" value="UserId"/>  
1.   
1.             <entry key="sessionid" value="UserName"/>  
1.             <entry key="uclass" value="UserClass"/>  
1.         </map>  
1.     </property>  
1. </bean>  
<bean id="attributeRepository" class="org.jasig.services.persondir.support.jdbc.SingleRowJdbcPersonAttributeDao"> <constructor-arg index="0" ref="dataSource"/> <constructor-arg index="1" value="select * from portal_user where {0}" /> <property name="queryAttributeMapping"> <map> <entry key="username" value="id" /> </map> </property> <property name="resultAttributeMapping"> <map> <entry key="id" value="UserId"/> <entry key="sessionid" value="UserName"/> <entry key="uclass" value="UserClass"/> </map> </property> </bean>
其中的sql语句可以根据自己的情况来写。下面resultAttributeMapping中的参数解释如下：
<entry key="id" value="UserId"/>
key代表的是你数据库中的字段，Value是客户端通过 AttributePrincipal获取时的参数
**3:CAS登录获取更多用户信息的JSP修改**
仅仅是修改xml是不够的，你必须修改他的casServiceValidationSuccess.jsp，路径为：
X:\tomcat-cas-server\webapps\cas\WEB-INF\view\jsp\protocol\2.0
在其中紧接着加入以下代码：
Xml代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <c:if test="${fn:length(assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.attributes) > 0}">  
1.     <cas:attributes>  
1.         <c:forEach var="attr" items="${assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.attributes}">  
1.             <cas:attribute>  
1.                 <cas:${fn:escapeXml(attr.key)}>${fn:escapeXml(attr.value)}</cas:${fn:escapeXml(attr.key)}>  
1.             </cas:attribute>  
1.         </c:forEach>  
1.     </cas:attributes>  
1. </c:if>  
<c:if test="${fn:length(assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.attributes) > 0}"> <cas:attributes> <c:forEach var="attr" items="${assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.attributes}"> <cas:attribute> <cas:${fn:escapeXml(attr.key)}>${fn:escapeXml(attr.value)}</cas:${fn:escapeXml(attr.key)}> </cas:attribute> </c:forEach> </cas:attributes> </c:if>
**注意：**
1.在casServiceValidationSuccess.jsp中加入我们的代码的时候
Xml代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <cas:${fn:escapeXml(attr.key)}>${fn:escapeXml(attr.value)}</cas:${fn:escapeXml(attr.key)}>  
<cas:${fn:escapeXml(attr.key)}>${fn:escapeXml(attr.value)}</cas:${fn:escapeXml(attr.key)}>
这里千万不能有空格！
2.在attributeRepository这个节点中，resultAttributeMapping下的配置，例如<entry key="id" value="UserId"/>在配置的时候需要注意数据库中存放的不能是中文（不过理论上可以解决，具体原因就是casServiceValidationSuccess.jsp在拼接XML的时候出现乱码。）有兴趣的可以尝试解决一下。
**2**
顶

**9**
踩

分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[CAS配置第三步：客户端的配置](http://andy-ghg.iteye.com/blog/942700 "CAS配置第三步：客户端的配置") | [CAS配置第一步：准备工作](http://andy-ghg.iteye.com/blog/942595 "CAS配置第一步：准备工作")
* 2011-03-06 15:37
* 浏览 1030
* [评论(3)](http://andy-ghg.iteye.com/blog/942653#comments)
* 分类:[企业架构](http://www.iteye.com/blogs/category/architecture)
* [相关推荐](http://www.iteye.com/wiki/blog/942653)

### 评论

[]()

3 楼 [andy_ghg](http://andy-ghg.iteye.com/ "andy_ghg") 2012-02-10  

queryAttributeMapping

01jiangwei01 写道
引用

<bean id="attributeRepository" class="org.jasig.services.persondir.support.jdbc.SingleRowJdbcPersonAttributeDao"> 
    <constructor-arg index="0" ref="dataSource"/> 
    <constructor-arg index="1" value="select * from portal_user where {0}" /> 
   <property name="queryAttributeMapping">    
            <map>    
                <entry key="username" value="id" />    
            </map>    
    </property> 
    <property name="resultAttributeMapping"> 
        <map> 
            <entry key="id" value="UserId"/> 
            <entry key="sessionid" value="UserName"/> 
            <entry key="uclass" value="UserClass"/> 
        </map> 
    </property> 
</bean>
中的queryAttributeMapping是什麼意思啊？
查询数据库的时候所使用的参数，我也很久没有做这个配置了。可能也记不太清楚了。不好意思。
2 楼 [01jiangwei01](http://01jiangwei01.iteye.com/ "01jiangwei01") 2012-02-08  

引用

<bean id="attributeRepository" class="org.jasig.services.persondir.support.jdbc.SingleRowJdbcPersonAttributeDao"> 
    <constructor-arg index="0" ref="dataSource"/> 
    <constructor-arg index="1" value="select * from portal_user where {0}" /> 
   <property name="queryAttributeMapping">    
            <map>    
                <entry key="username" value="id" />    
            </map>    
    </property> 
    <property name="resultAttributeMapping"> 
        <map> 
            <entry key="id" value="UserId"/> 
            <entry key="sessionid" value="UserName"/> 
            <entry key="uclass" value="UserClass"/> 
        </map> 
    </property> 
</bean>
中的queryAttributeMapping是什麼意思啊？

1 楼 [runkityboy](http://runkityboy.iteye.com/ "runkityboy") 2011-12-19  

谢谢帮我解决问题了
### 发表评论

[![]()](http://andy-ghg.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://andy-ghg.iteye.com/login)

[![andy_ghg的博客]( "andy_ghg的博客: RIA之路")](http://andy-ghg.iteye.com/)

andy_ghg

* 浏览: 50654 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 扬州
* ![]()
### 最近访客 [更多访客>>](http://andy-ghg.iteye.com/blog/user_visits)

[![asd51731的博客]( "asd51731的博客: ")](http://asd51731.iteye.com/)

[asd51731](http://asd51731.iteye.com/ "asd51731")

[![shuta的博客]( "shuta的博客: ")](http://shuta.iteye.com/)

[shuta](http://shuta.iteye.com/ "shuta")
[![lzf0112的博客]( "lzf0112的博客: ")](http://lzf0112.iteye.com/)

[lzf0112](http://lzf0112.iteye.com/ "lzf0112")

[![young.java的博客]( "young.java的博客: young.java")](http://young-java.iteye.com/)

[young.java](http://young-java.iteye.com/ "young.java")

### 文章分类

* [全部博客 (39)](http://andy-ghg.iteye.com/)
* [EXTJS (11)](http://andy-ghg.iteye.com/category/80684)
* [Java (10)](http://andy-ghg.iteye.com/category/146164)
* [Flex (8)](http://andy-ghg.iteye.com/category/80685)
* [杂谈 (5)](http://andy-ghg.iteye.com/category/80686)
* [C# (1)](http://andy-ghg.iteye.com/category/151763)
* [hadoop (3)](http://andy-ghg.iteye.com/category/175959)
* [flash (1)](http://andy-ghg.iteye.com/category/179432)
* [Nutch (1)](http://andy-ghg.iteye.com/category/226152)
* [Eclipse (1)](http://andy-ghg.iteye.com/category/226153)
### 社区版块

* [我的资讯](http://andy-ghg.iteye.com/blog/news) (0)
* [我的论坛](http://andy-ghg.iteye.com/blog/post) (124)
* [我的问答](http://andy-ghg.iteye.com/blog/answered_problems) (9)

### 存档分类

* [2012-06](http://andy-ghg.iteye.com/blog/monthblog/2012-06) (1)
* [2011-09](http://andy-ghg.iteye.com/blog/monthblog/2011-09) (3)
* [2011-08](http://andy-ghg.iteye.com/blog/monthblog/2011-08) (1)
* [更多存档...](http://andy-ghg.iteye.com/blog/monthblog_more)
### 评论排行榜

* [ExtJS 4.0 的改变--较为完整的介绍。](http://andy-ghg.iteye.com/blog/1133456 "ExtJS 4.0 的改变--较为完整的介绍。")
* [ExtJS 4.0.2a ActionColumn的使用](http://andy-ghg.iteye.com/blog/1121285 "ExtJS 4.0.2a ActionColumn的使用")
* [Hadoop学习笔记－读取视频流给Flash播放器](http://andy-ghg.iteye.com/blog/1179848 "Hadoop学习笔记－读取视频流给Flash播放器")

### 最新评论

* [l5061109](http://l5061109.iteye.com/ "l5061109")： 密码校验是自己通过从数据库中取到用户信息，包括用户名，密码，角 ...
[MyEclipse中Spring Security 3.0.3无废话配置（第一章）。](http://andy-ghg.iteye.com/blog/1071639#bc2257421)
* [优游人间](http://happy-every-daya-163-com.iteye.com/ "优游人间")： 刚刚开始学，我也mark一下
[ExtJS 4.0 的改变--较为完整的介绍。](http://andy-ghg.iteye.com/blog/1133456#bc2252138)
* [andy_ghg](http://andy-ghg.iteye.com/ "andy_ghg")： Navee 写道我有一点疑惑就是密码的验证是交给这个框架做的还 ...
[MyEclipse中Spring Security 3.0.3无废话配置（第一章）。](http://andy-ghg.iteye.com/blog/1071639#bc2248201)
* [Navee](http://navee.iteye.com/ "Navee")： 我有一点疑惑就是密码的验证是交给这个框架做的还是服务层自己实现 ...
[MyEclipse中Spring Security 3.0.3无废话配置（第一章）。](http://andy-ghg.iteye.com/blog/1071639#bc2248169)
* [andy_ghg](http://andy-ghg.iteye.com/ "andy_ghg")： queryAttributeMapping01jiangwei ...
[CAS配置第二步：服务器端的配置](http://andy-ghg.iteye.com/blog/942653#bc2242603)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![]()
