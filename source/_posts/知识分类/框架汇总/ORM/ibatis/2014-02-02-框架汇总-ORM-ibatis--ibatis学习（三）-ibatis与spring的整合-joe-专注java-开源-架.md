---
layout: post
title: "ibatis学习（三）-ibatis与spring的整合 - joe -专注java-开源-架"
categories: 框架汇总
tags: 
 - 框架汇总
 - ORM
 - ibatis
--- 

# ibatis学习（三）---ibatis与spring的整合 - joe --专注java,开源,架构,项目管理 - BlogJava

# [joe --专注java,开源,架构,项目管理](http://www.blogjava.net/freeman1984/)

STANDING ON THE SHOULDERS OF GIANTS

posts - 339, comments - 292, trackbacks - 0, articles - 1  [BlogJava](http://www.blogjava.net/) :: [首页](http://www.blogjava.net/freeman1984/) :: [新随笔](http://www.blogjava.net/freeman1984/admin/EditPosts.aspx?opt=1) :: [联系](http://www.blogjava.net/freeman1984/contact.aspx?id=1) :: [聚合](http://www.blogjava.net/freeman1984/rss) [![]()](http://www.blogjava.net/freeman1984/rss) :: [管理](http://www.blogjava.net/freeman1984/admin/EditPosts.aspx) ## [ibatis学习（三）---ibatis与spring的整合](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html)

Posted on 2007-12-07 18:26 [@joe](http://www.blogjava.net/freeman1984/) 阅读(13593) [评论(11)](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#Post)  [编辑](http://www.blogjava.net/freeman1984/admin/EditPosts.aspx?postid=166112)  [收藏](http://www.blogjava.net/freeman1984/AddToFavorite.aspx?id=166112) ![]()

 

Spring通过DAO模式，提供了对iBATIS的良好支持。SqlMapClient对象是iBATIS中的主要对象，我们可以通过配置让spring来管理SqlMapClient对象的创建。

与hibernate类似，Spring 提供了SqlMapClientDaoSupport对象，我们的DAO可以继承这个类，通过它所提供的SqlMapClientTemplate对象来操纵数据库。看起来这些概念都与hibernate类似。

通过SqlMapClientTemplate来操纵数据库的CRUD是没有问题的，这里面关键的问题是事务处理。Spring提供了强大的声明式事务处理的功能，我们已经清楚hibernate中如何配置声明式的事务，那么在iBATIS中如何获得声明式事务的能力呢？

第一，我们需要了解的是spring通过AOP来拦截方法的调用，从而在这些方法上面添加声明式事务处理的能力。典型配置如下：applicationContext-common.xml
    <!-- 配置事务特性 -->

    <tx:advice id="txAdvice" transaction-manager="*事务管理器名称*">

        <tx:attributes>

           <tx:method name="add*" propagation="REQUIRED"/>

           <tx:method name="del*" propagation="REQUIRED"/>

           <tx:method name="update*" propagation="REQUIRED"/>

           <tx:method name="*" read-only="true"/>

       </tx:attributes>

    </tx:advice>

   

    <!-- 配置哪些类的方法需要进行事务管理 -->

    <aop:config>

       <aop:pointcut id="allManagerMethod" expression="execution(* com.ibatis.manager.*.*(..))"/>

       <aop:advisor advice-ref="txAdvice" pointcut-ref="allManagerMethod"/>

    </aop:config>

这些事务都是声明在业务逻辑层的对象上的。

第二，我们需要一个事务管理器，对事务进行管理。
    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">

    <property name="dataSource" ref="dataSource"/>

    </bean>

    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">

        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>

        <property name="url" value="jdbc:mysql://127.0.0.1/ibatis"/>

        <property name="username" value="root"/>

        <property name="password" value="mysql"/>

    </bean>

此后，我们需要让spring来管理SqlMapClient对象：

    <bean id="sqlMapClient" class="org.springframework.orm.ibatis.SqlMapClientFactoryBean">

       <property name="configLocation"><value>classpath:sqlMapConfig.xml</value></property>

    </bean>

我们的sqlMapConfig.xml就可以简写为：

<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE sqlMapConfig     

    PUBLIC "-//ibatis.apache.org//DTD SQL Map Config 2.0//EN"     

    "http://ibatis.apache.org/dtd/sql-map-config-2.dtd">

<sqlMapConfig>

    <settings

       lazyLoadingEnabled="true"

        useStatementNamespaces="true" />

    <!-- 使用spring之后，数据源的配置移植到了spring上，所以iBATIS本身的配置可以取消 -->

  <sqlMap resource="com/ibatis/dao/impl/ibatis/User.xml"/>

</sqlMapConfig>

User.xml:如下

<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE sqlMap     

    PUBLIC "-//ibatis.apache.org//DTD SQL Map 2.0//EN"     

    "http://ibatis.apache.org/dtd/sql-map-2.dtd">

<sqlMap namespace="User">

 <!-- Use type aliases to avoid typing the full classname every time. -->

 <typeAlias alias="User" type="com.ibatis.User"/>

 <!-- Select with no parameters using the result map for Account class. -->

 <select id="selectAllUsers" resultClass="User">

    select * from t_user

 </select>

 

 <select id="selectUser" resultClass="User" parameterClass="int">

  select * from t_user where id=#id#

 </select>

 

 <insert id="insertUser" parameterClass="User">

  insert into t_user values (

       null,#username#,#password#

  )

 </insert>

 

 <update id="updateUser" parameterClass="User">

  update t_user set username = #username#,password=#password#

  where id=#id#

  </update>

 

 <delete id="deleteUser" parameterClass="int">

  delete from t_user where id=#id#

 </delete>

</sqlMap>

我们的DAO的编写：
package com.iabtis.dao.impl.ibatis;

import java.util.List;

import org.springframework.orm.ibatis.support.SqlMapClientDaoSupport;

import com.ibatis.dao.UserDAO;

import com.ibatis.crm.model.User;

public class UserDAOImpl extends SqlMapClientDaoSupport implements UserDAO {

    public void select(User user) {

              getSqlMapClientTemplate().delete("selectUser ",user.getId());

       }

   public List findAll() {

              return getSqlMapClientTemplate().queryForList("selectAllUsers ");

       }

       public void delete(User user) {

              getSqlMapClientTemplate().delete("deleteUser ",user.getId());

       }

       public void save(User user) {

              getSqlMapClientTemplate().insert("insertUser ",user);

       }

       public void update(User user) {

              getSqlMapClientTemplate().update("updateUser ",user);

       }

}

继承SqlMapClientDaoSupport，要求我们注入SqlMapClient对象，因此，需要有如下的DAO配置：

<bean id="userDAO" class="com.ibatils.dao.impl.ibatis.UserDAOImpl">

     <property name=”sqlMapClient” ref=”sqlMapClient”/>

</bean>

这就是所有需要注意的问题了，此后就可以在业务逻辑层调用DAO对象了！

[]()

### 评论

## [#](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#166209 "permalink: re: ibatis学习（三）---ibatis与spring的整合") []()re: ibatis学习（三）---ibatis与spring的整合  [回复](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#post)  [更多评论](http://www.blogjava.net/comment?author=laocat "查看该作者发表过的评论") []()  []()

2007-12-08 09:42 by [laocat]()

豁然开朗 ！！

## [#](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#188269 "permalink: re: ibatis学习（三）---ibatis与spring的整合") []()re: ibatis学习（三）---ibatis与spring的整合  [回复](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#post)  [更多评论](http://www.blogjava.net/comment?author=%e5%b1%b9%e7%a0%be "查看该作者发表过的评论") []()  []()

2008-03-24 16:08 by [屹砾](http://www.blogjava.net/eli/)

整合的问题现在终于清楚了，
对于advice和aop还有点不清楚。
## [#](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#207721 "permalink: re: ibatis学习（三）---ibatis与spring的整合[未登录]") []()re: ibatis学习（三）---ibatis与spring的整合[未登录]  [回复](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#post)  [更多评论](http://www.blogjava.net/comment?author=%e5%86%b7%e6%bc%a0%e5%a4%a7%e7%a5%9e "查看该作者发表过的评论") []()  []()

2008-06-13 17:26 by [冷漠大神](http://o(â©_â©)o.../)

真是不错的文章啊 如果在在可以把源码提供下载 那就更完美了 :-) 呵呵 是不是有点贪心

## [#](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#207724 "permalink: re: ibatis学习（三）---ibatis与spring的整合[未登录]") []()re: ibatis学习（三）---ibatis与spring的整合[未登录]  [回复](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#post)  [更多评论](http://www.blogjava.net/comment?author=%e5%86%b7%e6%bc%a0%e5%a4%a7%e7%a5%9e "查看该作者发表过的评论") []()  []()

2008-06-13 17:34 by [冷漠大神](http://o(â©_â©)o.../)

有没有代码下载啊？
## [#](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#224098 "permalink: re: ibatis学习（三）---ibatis与spring的整合") []()re: ibatis学习（三）---ibatis与spring的整合  [回复](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#post)  [更多评论](http://www.blogjava.net/comment?author=379548695qq "查看该作者发表过的评论") []()  []()

2008-08-25 11:35 by [379548695qq]()

UserDAO类里面的内容是什么？

## [#](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#260672 "permalink: re: ibatis学习（三）---ibatis与spring的整合[未登录]") []()re: ibatis学习（三）---ibatis与spring的整合[未登录]  [回复](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#post)  [更多评论](http://www.blogjava.net/comment?author=%e8%99%ab%e5%ad%90 "查看该作者发表过的评论") []()  []()

2009-03-19 10:17 by [虫子]()

正在烦恼中，google到了你的方案！呵呵
## [#](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#291041 "permalink: re: ibatis学习（三）---ibatis与spring的整合") []()re: ibatis学习（三）---ibatis与spring的整合  [回复](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#post)  [更多评论](http://www.blogjava.net/comment?author=jadmin "查看该作者发表过的评论") []()  []()

2009-08-13 16:32 by [jadmin](http://hi.baidu.com/jadmin)

很好，学习了

## [#](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#291296 "permalink: re: ibatis学习（三）---ibatis与spring的整合") []()re: ibatis学习（三）---ibatis与spring的整合  [回复](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#post)  [更多评论](http://www.blogjava.net/comment?author=%e5%8c%b9%e9%a9%ac%e5%8d%95%e6%9e%aa "查看该作者发表过的评论") []()  []()

2009-08-15 23:15 by [匹马单枪]()

好帖， 学习了！
## [#](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#293434 "permalink: re: ibatis学习（三）---ibatis与spring的整合") []()re: ibatis学习（三）---ibatis与spring的整合  [回复](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#post)  [更多评论](http://www.blogjava.net/comment?author=2 "查看该作者发表过的评论") []()  []()

2009-09-01 13:07 by [2]()

为什么我的出错了

## [#](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#319581 "permalink: re: ibatis学习（三）---ibatis与spring的整合[未登录]") []()re: ibatis学习（三）---ibatis与spring的整合[未登录]  [回复](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#post)  [更多评论](http://www.blogjava.net/comment?author=test "查看该作者发表过的评论") []()  []()

2010-04-28 11:10 by [test]()

autocommit 如果不设置为false, 事务有用么
## [#](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#336667 "permalink: re: ibatis学习（三）---ibatis与spring的整合") []()re: ibatis学习（三）---ibatis与spring的整合[]()  [回复](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html#post)  [更多评论](http://www.blogjava.net/comment?author=rr "查看该作者发表过的评论") []()  []()

2010-11-01 13:42 by [rr]()

sqlMapClient里应该注入dataSource

[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  
[]() [找优秀程序员，就在博客园](http://job.cnblogs.com/)
IT新闻：
· [婚恋网世纪佳缘乱象重生 成一夜情猎场](http://news.cnblogs.com/n/112126/)
· [帝国时代 Online 已发布，可免费下载](http://news.cnblogs.com/n/112124/)
· [小米发布会中的亮点与尿点：“狗日的 1999”](http://news.cnblogs.com/n/112123/)
· [315投诉网疑被关后改名重开张 新网站否认](http://news.cnblogs.com/n/112122/)
· [团购网站融资冷却裁员求生 行业洗牌危机并存](http://news.cnblogs.com/n/112121/)    [博客园](http://www.cnblogs.com/)  [博问](http://home.cnblogs.com/q/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html&SourceURL=/freeman1984/archive/2007/12/07/166112.html)       [使用Ctrl+Enter键可以直接提交]    推荐职位：
· [广州ASP.NET程序员(广州丹霄信息技术)](http://job.cnblogs.com/offer/15741/)
· [上海 .NET软件工程师(上海苏秦网络)](http://job.cnblogs.com/offer/15624/)
· [上海.NET软件开发工程师(东方财富信息)](http://job.cnblogs.com/offer/15546/)
· [北京 SQL数据库开发工程师(圣特尔科技)](http://job.cnblogs.com/offer/14109/)
· [北京高新诚聘 ASP.NET 程序员(盈科融通软件)](http://job.cnblogs.com/offer/15489/)
· [北京 .NET软件工程师(北京科胜永昌)](http://job.cnblogs.com/Offer/15346/)
· [北京.NET 研发工程师 (北京捷报数据)](http://job.cnblogs.com/offer/5914/)
· [北京C#开发工程师 B/S方向(圣特尔科技)](http://job.cnblogs.com/offer/14108/)

博客园首页随笔：
· [C#设计模式——命令模式(Command Pattern)](http://www.cnblogs.com/saville/archive/2011/08/17/2142754.html)
· [老系统维护](http://www.cnblogs.com/ahl5esoft/archive/2011/08/17/2142739.html)
· [使用单例模式实现自己的HttpClient工具类](http://www.cnblogs.com/codingmyworld/archive/2011/08/17/2141706.html)
· [Spread for Windows Forms高级主题(2)---理解单元格类型](http://www.cnblogs.com/powertoolsteam/archive/2011/08/17/2142085.html)
· [ERP/MIS开发 菜单设计器(Menu Designer)及其B/S,C/S双重实现（B/S开源）](http://www.cnblogs.com/JamesLi2015/archive/2011/08/17/2142012.html)
知识库：
· [IT项目管理的六种错误思维](http://kb.cnblogs.com/page/104475/)
· [我的10个开发原则](http://kb.cnblogs.com/page/107415/)
· [大数据下的数据分析平台架构](http://kb.cnblogs.com/page/111704/)
· [为什么编程是独一无二的职业](http://kb.cnblogs.com/page/109493/)
· [分享8年开发经验，浅谈个人发展经历，明确自己发展方向](http://kb.cnblogs.com/page/104736/) 最简洁阅读版式：
[ibatis学习（三）---ibatis与spring的整合](http://archive.cnblogs.com/b/166112/) 网站导航:

[博客园](http://www.cnblogs.com/ "程序员的网上家园")   [IT新闻](http://news.cnblogs.com/)   [知识库](http://kb.cnblogs.com/)   [博客生活](http://www.cnweblog.com/)   [IT博客网](http://www.cnitblog.com/)   [C++博客](http://www.cppblog.com/)   [博问](http://space.cnblogs.com/q/ "IT问答")   [管理](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html?opt=admin)    Powered by:
[BlogJava](http://www.blogjava.net/)
Copyright © @joe

![]()

### 公告

[](http://shop59092284.taobao.com/)[](http://shop59092284.taobao.com/)[** 老婆的淘宝,欢迎选购**](http://shop59092284.taobao.com/)

![]()

### 留言簿(2)

* [给我留言](http://www.blogjava.net/freeman1984/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/freeman1984/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/freeman1984/admin/MyMessages.aspx)
![]()

### 随笔分类

* [![]( "Subscribe to all 生活杂谈(14)")](http://www.blogjava.net/freeman1984/category/26087.html/rss "Subscribe to all 生活杂谈(14)")[all 生活杂谈(14)](http://www.blogjava.net/freeman1984/category/26087.html)
* [![]( "Subscribe to android(18)")](http://www.blogjava.net/freeman1984/category/42532.html/rss "Subscribe to android(18)")[android(18)](http://www.blogjava.net/freeman1984/category/42532.html)
* [![]( "Subscribe to apache项目(13)")](http://www.blogjava.net/freeman1984/category/46243.html/rss "Subscribe to apache项目(13)")[apache项目(13)](http://www.blogjava.net/freeman1984/category/46243.html)
* [![]( "Subscribe to chart(1)")](http://www.blogjava.net/freeman1984/category/43680.html/rss "Subscribe to chart(1)")[chart(1)](http://www.blogjava.net/freeman1984/category/43680.html)
* [![]( "Subscribe to concurrent(1)")](http://www.blogjava.net/freeman1984/category/49019.html/rss "Subscribe to concurrent(1)")[concurrent(1)](http://www.blogjava.net/freeman1984/category/49019.html)
* [![]( "Subscribe to database(34)")](http://www.blogjava.net/freeman1984/category/38740.html/rss "Subscribe to database(34)")[database(34)](http://www.blogjava.net/freeman1984/category/38740.html)
* [![]( "Subscribe to dwr(3)")](http://www.blogjava.net/freeman1984/category/42595.html/rss "Subscribe to dwr(3)")[dwr(3)](http://www.blogjava.net/freeman1984/category/42595.html)
* [![]( "Subscribe to flex(1)")](http://www.blogjava.net/freeman1984/category/43586.html/rss "Subscribe to flex(1)")[flex(1)](http://www.blogjava.net/freeman1984/category/43586.html)
* [![]( "Subscribe to hibernate(23)")](http://www.blogjava.net/freeman1984/category/42534.html/rss "Subscribe to hibernate(23)")[hibernate(23)](http://www.blogjava.net/freeman1984/category/42534.html)
* [![]( "Subscribe to java (123)")](http://www.blogjava.net/freeman1984/category/26086.html/rss "Subscribe to java (123)")[java (123)](http://www.blogjava.net/freeman1984/category/26086.html)
* [![]( "Subscribe to javafx(1)")](http://www.blogjava.net/freeman1984/category/41058.html/rss "Subscribe to javafx(1)")[javafx(1)](http://www.blogjava.net/freeman1984/category/41058.html)
* [![]( "Subscribe to java安全(6)")](http://www.blogjava.net/freeman1984/category/43930.html/rss "Subscribe to java安全(6)")[java安全(6)](http://www.blogjava.net/freeman1984/category/43930.html)
* [![]( "Subscribe to java性能(12)")](http://www.blogjava.net/freeman1984/category/43783.html/rss "Subscribe to java性能(12)")[java性能(12)](http://www.blogjava.net/freeman1984/category/43783.html)
* [![]( "Subscribe to jbpm(1)")](http://www.blogjava.net/freeman1984/category/38739.html/rss "Subscribe to jbpm(1)")[jbpm(1)](http://www.blogjava.net/freeman1984/category/38739.html)
* [![]( "Subscribe to jquery(3)")](http://www.blogjava.net/freeman1984/category/44698.html/rss "Subscribe to jquery(3)")[jquery(3)](http://www.blogjava.net/freeman1984/category/44698.html)
* [![]( "Subscribe to linux(6)")](http://www.blogjava.net/freeman1984/category/46405.html/rss "Subscribe to linux(6)")[linux(6)](http://www.blogjava.net/freeman1984/category/46405.html)
* [![]( "Subscribe to lucene(1)")](http://www.blogjava.net/freeman1984/category/47431.html/rss "Subscribe to lucene(1)")[lucene(1)](http://www.blogjava.net/freeman1984/category/47431.html)
* [![]( "Subscribe to lucene(1)")](http://www.blogjava.net/freeman1984/category/47801.html/rss "Subscribe to lucene(1)")[lucene(1)](http://www.blogjava.net/freeman1984/category/47801.html)
* [![]( "Subscribe to others(2)")](http://www.blogjava.net/freeman1984/category/26088.html/rss "Subscribe to others(2)")[others(2)](http://www.blogjava.net/freeman1984/category/26088.html)
* [![]( "Subscribe to questions(7)")](http://www.blogjava.net/freeman1984/category/38742.html/rss "Subscribe to questions(7)")[questions(7)](http://www.blogjava.net/freeman1984/category/38742.html)
* [![]( "Subscribe to questions_hander(5)")](http://www.blogjava.net/freeman1984/category/38741.html/rss "Subscribe to questions_hander(5)")[questions_hander(5)](http://www.blogjava.net/freeman1984/category/38741.html)
* [![]( "Subscribe to spring(24)")](http://www.blogjava.net/freeman1984/category/42533.html/rss "Subscribe to spring(24)")[spring(24)](http://www.blogjava.net/freeman1984/category/42533.html)
* [![]( "Subscribe to struts(8)")](http://www.blogjava.net/freeman1984/category/42535.html/rss "Subscribe to struts(8)")[struts(8)](http://www.blogjava.net/freeman1984/category/42535.html)
* [![]( "Subscribe to swing")](http://www.blogjava.net/freeman1984/category/42536.html/rss "Subscribe to swing")[swing](http://www.blogjava.net/freeman1984/category/42536.html)
* [![]( "Subscribe to UML(2)")](http://www.blogjava.net/freeman1984/category/48926.html/rss "Subscribe to UML(2)")[UML(2)](http://www.blogjava.net/freeman1984/category/48926.html)
* [![]( "Subscribe to web(33)")](http://www.blogjava.net/freeman1984/category/44672.html/rss "Subscribe to web(33)")[web(33)](http://www.blogjava.net/freeman1984/category/44672.html)
* [![]( "Subscribe to webservice(5)")](http://www.blogjava.net/freeman1984/category/38743.html/rss "Subscribe to webservice(5)")[webservice(5)](http://www.blogjava.net/freeman1984/category/38743.html)
* [![]( "Subscribe to xml(2)")](http://www.blogjava.net/freeman1984/category/43702.html/rss "Subscribe to xml(2)")[xml(2)](http://www.blogjava.net/freeman1984/category/43702.html)
* [![]( "Subscribe to 敏捷(6)")](http://www.blogjava.net/freeman1984/category/46247.html/rss "Subscribe to 敏捷(6)")[敏捷(6)](http://www.blogjava.net/freeman1984/category/46247.html)
* [![]( "Subscribe to 方法论(14)")](http://www.blogjava.net/freeman1984/category/46503.html/rss "Subscribe to 方法论(14)")[方法论(14)](http://www.blogjava.net/freeman1984/category/46503.html)
* [![]( "Subscribe to 架构(10)")](http://www.blogjava.net/freeman1984/category/46575.html/rss "Subscribe to 架构(10)")[架构(10)](http://www.blogjava.net/freeman1984/category/46575.html)
* [![]( "Subscribe to 网络通讯(5)")](http://www.blogjava.net/freeman1984/category/49063.html/rss "Subscribe to 网络通讯(5)")[网络通讯(5)](http://www.blogjava.net/freeman1984/category/49063.html)
* [![]( "Subscribe to 项目管理(17)")](http://www.blogjava.net/freeman1984/category/46246.html/rss "Subscribe to 项目管理(17)")[项目管理(17)](http://www.blogjava.net/freeman1984/category/46246.html)

![]()

### 相册

* [我的相册](http://www.blogjava.net/freeman1984/gallery/42537.html)
![]()

### 搜索

*  

![]()

### 积分与排名

* 积分 - 249820
* 排名 - 69
![]()

### 最新评论 [![]()](http://www.blogjava.net/freeman1984/CommentsRSS.aspx)

* [1. re: 一篇不错的讲解Java异常的文章(转载）----感觉很不错，读了以后很有启发[未登录]](http://www.blogjava.net/freeman1984/archive/2011/08/08/148850.html#356038)
* 受教了。非常感谢。
不过，后面罗列的那么多异常有点不必要了。
* --jake
* [2. re: strtuts2 异常之Could not create and/or set value back on to object [未登录]](http://www.blogjava.net/freeman1984/archive/2011/07/25/324133.html#355011)
* thank you very much！
* --fighting
* [3. re: Android屏幕元素层次结构[未登录]](http://www.blogjava.net/freeman1984/archive/2011/07/22/301081.html#354834)
* <p style="color:red">顶一下</p>
* --黑石
* [4. re: 不同技术团队的配合问题及DevOps(不错的文章，来自infoq)](http://www.blogjava.net/freeman1984/archive/2011/07/21/354712.html#354750)
* 做个记号。
* --何杨
* [5. re: other[未登录]](http://www.blogjava.net/freeman1984/archive/2011/07/12/154309.html#354154)
* 希望学习，谢谢！
trust_myself@126.com
* --asa

![]()

### 阅读排行榜

* [1. 一篇不错的讲解Java异常的文章(转载）----感觉很不错，读了以后很有启发(17440)](http://www.blogjava.net/freeman1984/archive/2007/09/27/148850.html)
* [2. 世界上最健康的作息时间表 大家对比下(16717)](http://www.blogjava.net/freeman1984/archive/2009/11/05/301262.html)
* [3. ibatis学习（三）---ibatis与spring的整合(13592)](http://www.blogjava.net/freeman1984/archive/2007/12/07/166112.html)
* [4. 如何用javascript控制checkbox，并进行批量删除(8527)](http://www.blogjava.net/freeman1984/archive/2007/09/24/147879.html)
* [5. ibatis学习（二）--ibatis使用介绍(6368)](http://www.blogjava.net/freeman1984/archive/2007/12/07/166113.html)
![]()

### 评论排行榜

* [1. other(41)](http://www.blogjava.net/freeman1984/archive/2007/10/19/154309.html)
* [2. 一篇不错的讲解Java异常的文章(转载）----感觉很不错，读了以后很有启发(22)](http://www.blogjava.net/freeman1984/archive/2007/09/27/148850.html)
* [3. ssh中利用pager-taglib和filter进行分页(14)](http://www.blogjava.net/freeman1984/archive/2007/11/16/161093.html)
* [4. 团队内是有必要统一IDE(14)](http://www.blogjava.net/freeman1984/archive/2010/11/12/337878.html)
* [5. 世界上最健康的作息时间表 大家对比下(13)](http://www.blogjava.net/freeman1984/archive/2009/11/05/301262.html)

### 60天内阅读排行

* [1. 大家都用什么bug管理软件？(2031)](http://www.blogjava.net/freeman1984/archive/2011/06/20/352649.html)
* [2. CountDownLatch 简介和例子(1412)](http://www.blogjava.net/freeman1984/archive/2011/07/04/353654.html)
* [3. 统计指标和术语汇总(1396)](http://www.blogjava.net/freeman1984/archive/2011/08/04/355820.html)
* [4. 不同技术团队的配合问题及DevOps(不错的文章，来自infoq)(1321)](http://www.blogjava.net/freeman1984/archive/2011/07/20/354712.html)
* [5. connection.release_mode，connection.autocommit和transaction.auto_close_session用法(1003)](http://www.blogjava.net/freeman1984/archive/2011/08/04/355808.html)
