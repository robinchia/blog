---
layout: post
title: "关于cas-server-webapp for ldap的principal其他属性返回的问题 - "
categories: CAS
tags: 
 - CAS
--- 

# 关于cas-server-webapp for ldap的principal其他属性返回的问题 - 贝贝爸爸的程序人生 - BlogJava

# [贝贝爸爸的程序人生](http://www.blogjava.net/yuanqixun/)

关注Seam、BPM

posts - 23, comments - 6, trackbacks - 0, articles - 32   [BlogJava](http://www.blogjava.net/) :: [首页](http://www.blogjava.net/yuanqixun/) :: [新随笔](http://www.blogjava.net/yuanqixun/admin/EditPosts.aspx?opt=1) :: [联系](http://www.blogjava.net/yuanqixun/contact.aspx?id=1) :: [聚合](http://www.blogjava.net/yuanqixun/rss) [![]()](http://www.blogjava.net/yuanqixun/rss) :: [管理](http://www.blogjava.net/yuanqixun/admin/EditPosts.aspx) ![]()

### 日历

[<]( "Go to the previous month")2012年4月[>]( "Go to the next month")日一二三四五六25262728293031123[4](http://www.blogjava.net/yuanqixun/archive/2012/04/04.html)5678910111213141516171819[20](http://www.blogjava.net/yuanqixun/archive/2012/04/20.html)21222324[25](http://www.blogjava.net/yuanqixun/archive/2012/04/25.html)[26](http://www.blogjava.net/yuanqixun/archive/2012/04/26.html)2728293012345

![]()

### 常用链接

* [我的随笔](http://www.blogjava.net/yuanqixun/MyPosts.html)
* [我的评论](http://www.blogjava.net/yuanqixun/MyComments.html)
* [我的参与](http://www.blogjava.net/yuanqixun/OtherPosts.html)
* [最新评论](http://www.blogjava.net/yuanqixun/RecentComments.html)
![]()

### 留言簿(1)

* [给我留言](http://www.blogjava.net/yuanqixun/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/yuanqixun/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/yuanqixun/admin/MyMessages.aspx)

![]()

### 随笔分类(19)

* [![]( "Subscribe to CAS(1)")](http://www.blogjava.net/yuanqixun/category/51531.html/rss "Subscribe to CAS(1)")[CAS(1)](http://www.blogjava.net/yuanqixun/category/51531.html)
* [![]( "Subscribe to 我的儿子(1)")](http://www.blogjava.net/yuanqixun/category/48487.html/rss "Subscribe to 我的儿子(1)")[我的儿子(1)](http://www.blogjava.net/yuanqixun/category/48487.html)
* [![]( "Subscribe to 生活感悟(1)")](http://www.blogjava.net/yuanqixun/category/48486.html/rss "Subscribe to 生活感悟(1)")[生活感悟(1)](http://www.blogjava.net/yuanqixun/category/48486.html)
* [![]( "Subscribe to 程序人生(16)")](http://www.blogjava.net/yuanqixun/category/48485.html/rss "Subscribe to 程序人生(16)")[程序人生(16)](http://www.blogjava.net/yuanqixun/category/48485.html)
![]()

### 随笔档案(40)

* [2012年5月 (2)](http://www.blogjava.net/yuanqixun/archive/2012/05.html)
* [2012年4月 (4)](http://www.blogjava.net/yuanqixun/archive/2012/04.html)
* [2012年3月 (7)](http://www.blogjava.net/yuanqixun/archive/2012/03.html)
* [2012年2月 (2)](http://www.blogjava.net/yuanqixun/archive/2012/02.html)
* [2011年8月 (1)](http://www.blogjava.net/yuanqixun/archive/2011/08.html)
* [2011年7月 (1)](http://www.blogjava.net/yuanqixun/archive/2011/07.html)
* [2011年6月 (1)](http://www.blogjava.net/yuanqixun/archive/2011/06.html)
* [2011年5月 (21)](http://www.blogjava.net/yuanqixun/archive/2011/05.html)
* [2008年7月 (1)](http://www.blogjava.net/yuanqixun/archive/2008/07.html)

![]()

### 文章分类(17)

* [![]( "Subscribe to BPM(1)")](http://www.blogjava.net/yuanqixun/category/48484.html/rss "Subscribe to BPM(1)")[BPM(1)](http://www.blogjava.net/yuanqixun/category/48484.html)
* [![]( "Subscribe to GIT(2)")](http://www.blogjava.net/yuanqixun/category/48754.html/rss "Subscribe to GIT(2)")[GIT(2)](http://www.blogjava.net/yuanqixun/category/48754.html)
* [![]( "Subscribe to J2EE(2)")](http://www.blogjava.net/yuanqixun/category/49452.html/rss "Subscribe to J2EE(2)")[J2EE(2)](http://www.blogjava.net/yuanqixun/category/49452.html)
* [![]( "Subscribe to Linux(9)")](http://www.blogjava.net/yuanqixun/category/48483.html/rss "Subscribe to Linux(9)")[Linux(9)](http://www.blogjava.net/yuanqixun/category/48483.html)
* [![]( "Subscribe to Seam2")](http://www.blogjava.net/yuanqixun/category/48482.html/rss "Subscribe to Seam2")[Seam2](http://www.blogjava.net/yuanqixun/category/48482.html)
* [![]( "Subscribe to Seam3(1)")](http://www.blogjava.net/yuanqixun/category/48545.html/rss "Subscribe to Seam3(1)")[Seam3(1)](http://www.blogjava.net/yuanqixun/category/48545.html)
* [![]( "Subscribe to 数据库(2)")](http://www.blogjava.net/yuanqixun/category/48715.html/rss "Subscribe to 数据库(2)")[数据库(2)](http://www.blogjava.net/yuanqixun/category/48715.html)
![]()

### 文章档案(12)

* [2011年11月 (6)](http://www.blogjava.net/yuanqixun/archives/2011/11.html)
* [2011年10月 (2)](http://www.blogjava.net/yuanqixun/archives/2011/10.html)
* [2011年8月 (3)](http://www.blogjava.net/yuanqixun/archives/2011/08.html)
* [2011年7月 (1)](http://www.blogjava.net/yuanqixun/archives/2011/07.html)

![]()

### 收藏夹(25)

* [![]( "Subscribe to git(15)")](http://www.blogjava.net/yuanqixun/favorite/48642.html/rss "Subscribe to git(15)")[git(15)](http://www.blogjava.net/yuanqixun/favorite/48642.html)
* [![]( "Subscribe to HttpComponent(2)")](http://www.blogjava.net/yuanqixun/favorite/48644.html/rss "Subscribe to HttpComponent(2)")[HttpComponent(2)](http://www.blogjava.net/yuanqixun/favorite/48644.html)
* [![]( "Subscribe to JBOSS(1)")](http://www.blogjava.net/yuanqixun/favorite/49099.html/rss "Subscribe to JBOSS(1)")[JBOSS(1)](http://www.blogjava.net/yuanqixun/favorite/49099.html)
* [![]( "Subscribe to linux")](http://www.blogjava.net/yuanqixun/favorite/51142.html/rss "Subscribe to linux")[linux](http://www.blogjava.net/yuanqixun/favorite/51142.html)
* [![]( "Subscribe to seam3(7)")](http://www.blogjava.net/yuanqixun/favorite/48645.html/rss "Subscribe to seam3(7)")[seam3(7)](http://www.blogjava.net/yuanqixun/favorite/48645.html)
![]()

### 常用链接

* [Nginx中文](http://wiki.nginx.org/Chs)

![]()

### 搜索

*  
![]()

### 最新评论 [![]()](http://www.blogjava.net/yuanqixun/CommentsRSS.aspx)

* [1. re: Fedora16下安装mysql[未登录]](http://www.blogjava.net/yuanqixun/archive/2012/03/15/364865.html#371970)
* 评论内容较长,点击标题查看
* --fg
* [2. re: Fedora16下安装mysql[未登录]](http://www.blogjava.net/yuanqixun/archive/2012/01/04/364865.html#367842)
* 评论内容较长,点击标题查看
* --fedora
* [3. re: 关于GIT Flow的学习理解[未登录]](http://www.blogjava.net/yuanqixun/archive/2011/08/12/351379.html#356402)
* 嘿嘿，被我发现了吧
我告诉你的喽。。。
* --dd
* [4. re: centos下samba的安装配置](http://www.blogjava.net/yuanqixun/archive/2011/08/12/349819.html#356362)
* 谢谢楼主，简单明了
* --里一方
* [5. re: 正在安装Fedora操作系统](http://www.blogjava.net/yuanqixun/archive/2011/05/08/349765.html#349789)
* 嗯，自己开发倒没问题，问题就是不好和公司其他同事沟通，例如公司的oa系统就只能ie，唉。linux应用还是很多阻力。
* --贝贝爸爸

![]()

### 阅读排行榜

* [1. CentOS下安装postgresql(369)](http://www.blogjava.net/yuanqixun/archive/2011/05/18/350525.html)
* [2. 在centos下配置gitosis(364)](http://www.blogjava.net/yuanqixun/archive/2011/05/26/351057.html)
* [3. 【转】centos卸载系统与环境部署(340)](http://www.blogjava.net/yuanqixun/archive/2011/05/11/349988.html)
* [4. centos下samba的安装配置(263)](http://www.blogjava.net/yuanqixun/archive/2011/05/09/349819.html)
* [5. 在CentOS下安装配置git(167)](http://www.blogjava.net/yuanqixun/archive/2011/05/10/349909.html)
![]()

### 评论排行榜

* [1. 正在安装Fedora操作系统(2)](http://www.blogjava.net/yuanqixun/archive/2011/05/08/349765.html)
* [2. centos下samba的安装配置(1)](http://www.blogjava.net/yuanqixun/archive/2011/05/09/349819.html)
* [3. 关于GIT Flow的学习理解(1)](http://www.blogjava.net/yuanqixun/archive/2011/05/31/351379.html)
* [4. 【转】在CentOS下安装配置postgresql8.3(0)](http://www.blogjava.net/yuanqixun/archive/2011/05/27/351210.html)
* [5. Mysql常用命令(0)](http://www.blogjava.net/yuanqixun/archive/2011/05/26/351129.html) ## [关于cas-server-webapp for ldap的principal其他属性返回的问题]()

Posted on 2012-04-25 23:12 [贝贝爸爸](http://www.blogjava.net/yuanqixun/) 阅读(44) [评论(0)](http://www.blogjava.net/yuanqixun/archive/2012/04/25/376638.html#Post)  [编辑](http://www.blogjava.net/yuanqixun/admin/EditPosts.aspx?postid=376638)  [收藏](http://www.blogjava.net/yuanqixun/AddToFavorite.aspx?id=376638) 所属分类: [CAS](http://www.blogjava.net/yuanqixun/category/51531.html) ![]()

nnd，今天搞了快2个小时，总是无法解决ldap支持的其他属性返回问题，因为之前配置的是3.3.5版本，现在最新的版本是3.4.11，原来的配置竟然无法使用了，原来是因为增加服务：
<bean id="serviceRegistryDao" class="org.jasig.cas.services.InMemoryServiceRegistryDaoImpl">
导致无法返回principal的其他属性到客户端，其实象这样配置即可：<bean id="attributeRepository"
  class="org.jasig.services.persondir.support.ldap.LdapPersonAttributeDao">
  <property name="contextSource" ref="contextSource" />
  <property name="baseDN" value="ou=users,${ldap.basePath}" />
  <property name="requireAllQueryAttributes" value="true" />
  <!--
  Attribute mapping beetween principal (key) and LDAP (value) names
  used to perform the LDAP search.  By default, multiple search criteria
  are ANDed together.  Set the queryType property to change to OR.
  -->
  <property name="queryAttributeMapping">
    <map>
      <entry key="username" value="uid" />
    </map>
  </property>
 
  <property name="resultAttributeMapping">
    <map>
    <!-- Mapping beetween LDAP entry attributes (key) and Principal's (value) -->
    <entry key="name" value="userName"/>
    <entry key="uid" value="userId"/>
    </map>
  </property>
</bean>
    <bean id="serviceRegistryDao" class="org.jasig.cas.services.InMemoryServiceRegistryDaoImpl">
        <property name="registeredServices">
            <list>
                <bean class="org.jasig.cas.services.RegisteredServiceImpl">
                    <property name="id" value="0" />
                    <property name="name" value="HTTP" />
                    <property name="description" value="Only Allows HTTP Urls" />
                    <property name="serviceId" value="http://**" />
                    <property name="evaluationOrder" value="10000001" />
                    <property name="ignoreAttributes" value="true" />
                </bean>
                                ………………
                        </list>
                </property>
        </bean> 如上所示，其中注册的服务registeredServices
默认是不允许返回其他属性到客户端的！！！！！，真的是很坑爹啊，不过，配置一下ignoreAttributes即可，也可以指定allowedAttributes
如下：
<property name="allowedAttributes">
    <list>
            <value><!-- your attribute key --></value>
    </list>
</property>

[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [博问 - 解决您的IT难题](http://q.cnblogs.com/)   [博客园](http://www.cnblogs.com/)  [博问](http://q.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/yuanqixun/archive/2012/04/25/376638.html&SourceURL=/yuanqixun/archive/2012/04/25/376638.html)       [使用Ctrl+Enter键可以直接提交]    IT新闻：
· [指尖上的体验：从可折叠iPhone物理键盘Spike说起](http://news.cnblogs.com/n/149144/)
· [解密腾讯星云](http://news.cnblogs.com/n/149143/)
· [图书出版业如何应对数字盗版](http://news.cnblogs.com/n/149142/)
· [资本局中局：成也萧何败也萧何](http://news.cnblogs.com/n/149141/)
· [微软本周发布9个补丁 影响所有Windows/Office](http://news.cnblogs.com/n/149138/)

博客园首页随笔：
· [设计模式学习笔记-适配器模式](http://www.cnblogs.com/wangjq/archive/2012/07/09/2582485.html)
· [[原创]一起来做网页游戏---第一章 巧妇难为无米之炊](http://www.cnblogs.com/liusy1988/archive/2012/07/09/2570438.html)
· [Spring3开发实战 之 第二章：IoC/DI开发（1）](http://www.cnblogs.com/kaitao/archive/2012/07/09/2582571.html)
· [第七话 Asp.Net Mvc 3.0【MVC项目实战の三】](http://www.cnblogs.com/HuiTai/archive/2012/07/09/MVC-7.html)
· [GNU C - Using GNU GCC __attribute__ mechanism 01](http://www.cnblogs.com/respawn/archive/2012/07/09/2582548.html)
知识库：
· [Web前端：11个让你代码整洁的原则](http://kb.cnblogs.com/page/148238/)
· [页面前端的水有多深？再议页面开发](http://kb.cnblogs.com/page/143279/)
· [Fiddler 教程](http://kb.cnblogs.com/page/130367/)
· [关于编程学习的七点思索](http://kb.cnblogs.com/page/148772/)
· [一地鸡毛 — 软件项目中的人际困局](http://kb.cnblogs.com/page/115175/)  网站导航:

[博客园](http://www.cnblogs.com/ "程序员的网上家园")   [IT新闻](http://news.cnblogs.com/)   [知识库](http://kb.cnblogs.com/)   [C++博客](http://www.cppblog.com/)   [程序员招聘](http://job.cnblogs.com/)   [管理](http://www.blogjava.net/yuanqixun/archive/2012/04/25/376638.html?opt=admin)    Powered by:
[BlogJava](http://www.blogjava.net/)
Copyright © 贝贝爸爸
