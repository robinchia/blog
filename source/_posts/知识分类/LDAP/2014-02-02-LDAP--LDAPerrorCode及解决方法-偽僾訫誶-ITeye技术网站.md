---
layout: post
title: "LDAP error Code 及解决方法 - 偽僾訫誶 - ITeye技术网站"
categories: LDAP
tags: 
 - LDAP
--- 

# LDAP error Code 及解决方法 - 偽僾訫誶 - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [群组](http://www.iteye.com/groups) [更多 ▼](http://king-wangyao.iteye.com/blog/998851#)

[招聘](http://www.iteye.com/job) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://king-wangyao.iteye.com/login "登录") [登录](http://king-wangyao.iteye.com/login) [注册](http://king-wangyao.iteye.com/signup)

# [偽僾訫誶](http://king-wangyao.iteye.com/)

永久域名 [http://king-wangyao.iteye.com](http://king-wangyao.iteye.com/)

[出现了ORA-01033:ORACLE initialization or ...](http://king-wangyao.iteye.com/blog/1011550 "出现了ORA-01033:ORACLE initialization or shutdown in progress ") | [Apache Tomcat 5.5配置-多域名绑定与虚拟 ...](http://king-wangyao.iteye.com/blog/998255 "Apache Tomcat 5.5配置-多域名绑定与虚拟目录设置")

2011-04-12

### [LDAP error Code 及解决方法]() **

**博客分类：**
* [综合技术](http://king-wangyao.iteye.com/category/151321)
[Tomcat](http://www.iteye.com/blogs/tag/Tomcat)[Security](http://www.iteye.com/blogs/tag/Security)[Websphere](http://www.iteye.com/blogs/tag/Websphere)[配置管理](http://www.iteye.com/blogs/tag/%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86)[JDK](http://www.iteye.com/blogs/tag/JDK)
**1.  error code 53**

===========================================================================

问题：创建新用户时出现数据后端异常

在 WebSphere Portal Express 中，您可以设置密码的最短和最长长度。如果设置的密码长度与 LDAP 服务器的策略不相同，则在创建用户时您可能会看到以下异常：

EJPSG0015E: Data Backend Problem com.ibm.websphere.wmm.exception.WMMSystemException:

The following Naming Exception occurred during processing:

"javax.naming.OperationNotSupportedException: [LDAP: error code 53 - 0000052D:

SvcErr: DSID-031A0FBC, problem 5003 (WILL_NOT_PERFORM), data 0

]; remaining name 'cn=see1anna,cn=users,dc=wps510,dc=rtp,dc=raleigh,dc=ibm,dc=com';

resolved object [com.sun.jndi.ldap.LdapCtx@7075b1b4](mailto:com.sun.jndi.ldap.LdapCtx@7075b1b4)".

原因：这是由于“密码不能满足密码策略的要求”导致

**解决方案：**

1. 打开域安全策略-安全设置-账户策略-密码策略-密码必须符合复杂性要求。定义这个策略设置为:已禁用。/ 密码长度最小值：定义这个策略设置为0。

2. 打开域控制器安全策略-安全设置-账户策略-密码策略-密码必须符合复杂性要求。定义这个策略设置为：已禁用。/ 密码长度最小值：定义这个策略设置为0。

3. 最后运行刷新组策略命令为：gpupdate /force

===========================================================================

 

 

**2. Need to specify class name**

===========================================================================

javax.naming.NoInitialContextException: Need to specify class name in environment or system property, or as an applet parameter, or in an application resource file:  java.naming.factory.initial

原因：LdapContext在处理完上个环节被close(),LdapContext=null;

解决方案：不close;

 

 

**3. error code 50**

===========================================================================

javax.naming.NoPermissionException: [LDAP: error code 50 - 00002098: SecErr: DSID-03150A45, problem 4003 (INSUFF_ACCESS_RIGHTS), data 0

 

 

**4. error code 68**

===========================================================================

javax.naming.NameAlreadyBoundException: [LDAP: error code 68 - 00000524: UpdErr: DSID-031A0F4F, problem 6005 (ENTRY_EXISTS), data 0

原因：创建的用户已经存在了

 

 

**5. No trusted certificate**

===========================================================================

javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: No trusted certificate found

1.cas机器A，A上a,b,c服务运行良好

2.website 位于B机器，cas可以截获请求，跳转javax.net.ssl.SSLHandshakeException

将A上生生成的客户端密钥，导入B

A运行

sudo keytool -genkey -alias tomcat -keyalg RSA -keypass changeit -storepass changeit -keystore server.keystore -validity 3600

$ keytool -export -trustcacerts -alias tomcat -file server.cer -keystore server.keystore -storepass changeit

$ sudo keytool -import -trustcacerts -alias tomcat -file server.cer -keystore $JAVA_HOME/jre/lib/security/cacerts -storepass changeit

B运行最后一句即可

建立信任关系，客户，服务密钥，客户多处

 

 

**6. error code 1**

===========================================================================

javax.naming.NamingException: [LDAP: error code 1 - 00000000: LdapErr: DSID-0C090AE2, comment: In order to perform this operation a successful bind must be completed on the connection., data 0, vece

原因：新增域用户的时候，ctx没有绑定管理员用户

解决方法：ctx.addToEnvironment(Context.SECURITY_PRINCIPAL, adminUser + "@" + ldapProperty.getDomain());

 ctx.addToEnvironment(Context.SECURITY_CREDENTIALS, adminPwd);

 

 

**7. error code 50**

==========================================================================

javax.naming.NoPermissionException: [LDAP: error code 50 - 00000005: SecErr: DSID-03151E04, problem 4003 (INSUFF_ACCESS_RIGHTS)

原因：新建域用户时候，ctx绑定到一个普通用户（该用户没有新建用户的权限）

解决方法：使用管理员用户进行绑定：

          ctx.addToEnvironment(Context.SECURITY_PRINCIPAL, adminUser + "@" + ldapProperty.getDomain());

 ctx.addToEnvironment(Context.SECURITY_CREDENTIALS, adminPwd);

 

 

**8. error code 19**

==========================================================================

javax.naming.directory.InvalidAttributeValueException: [LDAP: error code 19 - 0000052D: AtrErr: DSID-03190F00, #1:

0: 0000052D: DSID-03190F00, problem 1005 (CONSTRAINT_ATT_TYPE)

原因：这个最大的可能是不满足域安全策略：如密码复杂性、密码最短使用期限、强制密码历史。即长度、包含的字符、多久可以修改密码、是否可以使用历史密码等。

 

 

**9. LDAP: error code 50**

==========================================================================

javax.naming.NoPermissionException: [LDAP: error code 50 - 00000005: SecErr: DSID-031A0F44, problem 4003 (INSUFF_ACCESS_RIGHTS)

原因:这个是最初代码使用的replace操作，这个在AD里对应的是密码重设（普通用户默认没有这个权限，管理员可以操作），另外remove操作时提供的旧密码错误也可能报这个异常

 

 

**10. RSA premaster secret error**

==========================================================================

javax.naming.CommunicationException: simple bind failed: 172.18.20.4:636 [Root exception is javax.net.ssl.SSLKeyException: RSA premaster secret error]

原因：Tomcat 配置的JDK与添加证书的的JDK不一致。如：证书存放路径为C:\Java\jdk1.6.0_10\jre\lib\cacerts  而Tomcat 配置的JDK为C:\Java\jre6 ，使得两者路径不一致，SSL验证的时候，找不到证书

 

 

**11.No trusted certificate found**

==========================================================================

javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: No trusted certificate found

原因：信任证书库文件路径不正确

解决方法：将正确工程中 \WEB-INF\classes目录下

 

 

**12. error code 49**

==========================================================================

javax.naming.AuthenticationException: [LDAP: error code 49 - 80090308: LdapErr: DSID-0C090334, comment: AcceptSecurityContext error, data 52e, vece

原因：用户名或密码错误

 

分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")
[出现了ORA-01033:ORACLE initialization or ...](http://king-wangyao.iteye.com/blog/1011550 "出现了ORA-01033:ORACLE initialization or shutdown in progress ") | [Apache Tomcat 5.5配置-多域名绑定与虚拟 ...](http://king-wangyao.iteye.com/blog/998255 "Apache Tomcat 5.5配置-多域名绑定与虚拟目录设置")

* 13:54
* [评论](http://king-wangyao.iteye.com/blog/998851#comments) / 浏览 (0 / 572)
* 分类:[编程语言](http://www.iteye.com/blogs/category/language)
* [相关推荐](http://www.iteye.com/wiki/blog/998851)
### 评论

[]()

### 发表评论

[![]()](http://king-wangyao.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://king-wangyao.iteye.com/login)

[![king_wangyao的博客]( "king_wangyao的博客: 偽僾訫誶")](http://king-wangyao.iteye.com/)

king_wangyao

* 浏览: 57403 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 上海
* ![]()
* [详细资料](http://king-wangyao.iteye.com/blog/profile) [留言簿](http://king-wangyao.iteye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://king-wangyao.iteye.com/blog/user_visits)

[![hothotmail的博客]( "hothotmail的博客: ")](http://hothotmail.iteye.com/)

[hothotmail](http://hothotmail.iteye.com/)

[![nehcbai的博客]( "nehcbai的博客: ")](http://nehcbai.iteye.com/)

[nehcbai](http://nehcbai.iteye.com/)
[![hmilypxy的博客]( "hmilypxy的博客: ")](http://hmilypxy.iteye.com/)

[hmilypxy](http://hmilypxy.iteye.com/)

[![亦梦亦真的博客]( "亦梦亦真的博客: 小菜鸟的成长之路")](http://eclecl1314-163-com.iteye.com/)

[亦梦亦真](http://eclecl1314-163-com.iteye.com/)

### 博客分类

* [全部博客 (156)](http://king-wangyao.iteye.com/)
* [Java (69)](http://king-wangyao.iteye.com/category/127576)
* [MySQL (3)](http://king-wangyao.iteye.com/category/151318)
* [Oracle (13)](http://king-wangyao.iteye.com/category/151320)
* [Microsoft SQL Server (9)](http://king-wangyao.iteye.com/category/151319)
* [HTML (21)](http://king-wangyao.iteye.com/category/151144)
* [javaScript (0)](http://king-wangyao.iteye.com/category/180520)
* [jQuery (0)](http://king-wangyao.iteye.com/category/180519)
* [程序案例 (3)](http://king-wangyao.iteye.com/category/174176)
* [IT生活 (3)](http://king-wangyao.iteye.com/category/151799)
* [PowerDesigner (1)](http://king-wangyao.iteye.com/category/180518)
* [互联网 (1)](http://king-wangyao.iteye.com/category/151800)
* [综合技术 (11)](http://king-wangyao.iteye.com/category/151321)
* [软件开发管理 (6)](http://king-wangyao.iteye.com/category/151322)
* [行业应用 (4)](http://king-wangyao.iteye.com/category/151801)
### 我的相册

[![]()](http://king-wangyao.iteye.com/album) 714AAC4A
[共 3 张](http://king-wangyao.iteye.com/album)

### 我的留言簿 [>>更多留言](http://king-wangyao.iteye.com/blog/guest_book)
### 其他分类

* [我的收藏](http://king-wangyao.iteye.com/blog/favorite) (0)
* [我的代码](http://king-wangyao.iteye.com/blog/code_favorite) (0)
* [我的论坛主题帖](http://king-wangyao.iteye.com/blog/topic) (0)
* [我的所有论坛帖](http://king-wangyao.iteye.com/blog/post) (0)
* [我的精华良好帖](http://king-wangyao.iteye.com/blog/article) (0)

### 最近加入群组

* [struts2](http://struts2.group.iteye.com/)
* [Spring之旅](http://spring.group.iteye.com/)
* [Hibernate](http://hibernate.group.iteye.com/)
* [Android](http://android.group.iteye.com/)
* [EXT](http://ext.group.iteye.com/)
### 存档

* [2011-10](http://king-wangyao.iteye.com/blog/monthblog/2011-10) (2)
* [2011-09](http://king-wangyao.iteye.com/blog/monthblog/2011-09) (3)
* [2011-08](http://king-wangyao.iteye.com/blog/monthblog/2011-08) (3)
* [更多存档...](http://king-wangyao.iteye.com/blog/monthblog_more)

### 评论排行榜

* [查询数据库后是返回ResultSet还是返回Coll ...](http://king-wangyao.iteye.com/blog/986864 "查询数据库后是返回ResultSet还是返回Collection?")
* [jQuery Ajax分页(pagination.js)分页插件 ...](http://king-wangyao.iteye.com/blog/991283 "jQuery Ajax分页(pagination.js)分页插件 （转载）")
* [97编程大赛第一名的神奇程序](http://king-wangyao.iteye.com/blog/1051780 "97编程大赛第一名的神奇程序")
* [Java 7七大新功能预览](http://king-wangyao.iteye.com/blog/996522 "Java 7七大新功能预览")
* [![Rss]()](http://king-wangyao.iteye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://king-wangyao.iteye.com/rss)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![]()
