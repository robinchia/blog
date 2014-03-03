---
layout: post
title: "Hibernate知识点总结"
categories: 框架汇总
tags: 
 - 框架汇总
 - ORM
 - hibernate
--- 

# Hibernate知识点总结

### Hibernate(面试)知识点总结  

Hibernate是目前最流行的开源对象关系映射(ORM)框架。Hibernate采用低侵入式的设计，也即完全采用普通的Java对象（POJO），而不必继承Hibernate的某个基类，或实现Hibernate的某个接口。Hibernate是面向对象的程序设计语言和关系数据库之间的桥梁，Hibernate允许程序开发者采用面向对象的方式来操作关系数据库。

因为我们知道hibernate它能支持透明持久化从这个角度来看它没有侵入性所谓侵入性它没有侵入hibernate任何的API所以它叫轻量级框架，轻量级框架的好处是没有侵入性另外的一个好处是为测试带来了好处，测试非常简单测试就行我们写普通的java应用程序一样不需要什么环境只需要几个jar包就可以了写个main函数一侧就可以了它没有侵入性和测试非常简单 这是它流行的一个原因。

 

 

hibernate的优缺点 
优点：1、程序更加面向对象；
        2、提高了生产率；
        3、方便移植（修改配置文件）；
        4、无侵入性。
  缺点：
        1、效率比JDBC略差；
        2、不适合批量操作。

        3、只能配置一种关联关系

 

Hibernate有四种查询方案 
  1、get,load方法，根据id查找对象
  2、HQL--hibernate query language（查询对象：Query)
  3、Criteria--标准查询语言（查询对象：Criteria，查询条件：Criterion）
  4、通过sql来查（查询对象：SQLQuery）

具体

Query/Criteria:

1.Query接口封装了Hibernate强大的对象查询能力，同时也支持数据库的更新操作

2.提供了动态查询的参数绑定功能

3.提供list(),iterator(),scroll()等对象导航方法

4.提供uiqueResult()方法获取单独的对象

5.提供executeUpdate()方法来执行DML语句

6.提供了可移植的分页查询方法

Session：

1、save，presist保存数据，persist在事务外不会产生insert语句。

2、delete，删除对象

3、update，更新对象，如果数据库中没有记录，会出现异常，脱管对象如属性改变需要保存，则调用update方法

4、get，根据ID查，会立刻访问数据库

5、load，根据ID查，（返回的是代理，不会立即访问数据库)

6、saveOrUpdate,merge(根据ID和version的值来确定是否是save或update），调用merge你的对象还是托管的。当不知该对象的状态时可调用该方法

小贴士：瞬时对象id无值，脱管对象有值，hibernate通过该方式判断对象的状态

7、lock（把对象变成持久对象，但不会同步对象的状态）

 

hibernate的工作原理   
1.配置好hibernate的配置文件和与类对应的配置文件后，启动服务器
2.服务器通过实例化Configuration对象，读取hibernate.cfg.xml文件的配置内容，并根据相关的需求建好表或者和表建立好映射关系
3.通过实例化的Configuration对象就可以建立sessionFactory实例，进一步，通过sessionFactory实例可以创建 session对象
4.得到session之后，便可以对数据库进行增删改查操作了，除了比较复杂的全文搜索外，简单的操作都可以通过hibernate封装好的 session内置方法来实现
5.此外，还可以通过事物管理，表的关联来实现较为复杂的数据库设计
优点：hibernate相当于java类和数据库表之间沟通的桥梁，通过这座桥我们就可以做很多事情了

 

 

 

Hibernate 的初始化.
1)创建Configeration类的实例。
它的构造方法：将配置信息(Hibernate config.xml)读入到内存。
一个Configeration 实例代表Hibernate 所有Java类到Sql数据库映射的集合。

 

2)创建SessionFactory实例
把Configeration 对象中的所有配置信息拷贝到SessionFactory的缓存中。
SessionFactory的实例代表一个数据库存储员源，创建后不再与Configeration 对象关联。

 

3)调用SessionFactory创建Session的方法
1】用户自行提供JDBC连接。
Connection con=dataSource.getConnection();
Session s=sessionFactory.openSession(con);
2】让SessionFactory提供连接
Session s=sessionFactory.openSession();

 

状态转换

 

临时状态 (transient)

1】不处于Session 缓存中
2】数据库中没有对象记录

 

Java如何进入临时状态
1】通过new语句刚创建一个对象时
2】当调用Session 的delete()方法，从Session 缓存中删除一个对象时。

持久化状态(persisted)

1】处于Session 缓存中
2】持久化对象数据库中有对象记录
3】Session 在特定时刻会保持二者同步

Java如何进入持久化状态
1】Session 的save()把临时－》持久化状态
2】Session 的load(),get()方法返回的对象
3】Session 的find()返回的list集合中存放的对象
4】Session 的update(),saveOrupdate()使游离－》持久化

游离状态(detached)

1】不再位于Session 缓存中
2】游离对象由持久化状态转变而来，数据库中可能还有对应记录。

Java如何进入持久化状态－》游离状态
1】Session 的close()方法
2】Session 的evict()方法，从缓存中删除一个对象。提高性能。少用。

 

具体如图：

 

 

 

缓存

 

主要有session缓存（一级缓存）和sessionfactory缓存（内置缓存和外置缓存（也叫二级缓存））

 

Session的缓存是内置的，不能被卸载，也被称为Hibernate的第一级缓存（Session的一些集合属性包含的数据）。在第一级缓存中，持久化类的每个实例都具有唯一的OID。

 

SessionFactory的缓存又可以分为两类：

 

内置缓存（存放SessionFactory对象的一些集合属性包含的数据，SessionFactory的内置缓存中存放了映射元数据和预定义SQL语句，映射元数据是映射文件中数据的拷贝，而预定义SQL语句是在Hibernate初始化阶段根据映射元数据推导出来，SessionFactory的内置缓存是只读的，应用程序不能修改缓存中的映射元数据和预定义SQL语句，因此SessionFactory不需要进行内置缓存与映射文件的同步。）

 

外置缓存。SessionFactory的外置缓存是一个可配置的插件。在默认情况下，SessionFactory不会启用这个插件。外置缓存的数据是数据库数据的拷贝，外置缓存的介质可以是内存或者硬盘。SessionFactory的外置缓存也被称为Hibernate的第二级缓存。

 

缓存的范围分为三类。

1 事务范围：缓存只能被当前事务访问。

2 进程范围：缓存被进程内的所有事务共享。

3 集群范围：在集群环境中，缓存被一个机器或者多个机器的进程共享。

 

持久化层可以提供多种范围的缓存。如果在事务范围的缓存中没有查到相应的数据，还可以到进程范围或集群范围的缓存内查询，如果还是没有查到，那么只有到数据库中查询。事务范围的缓存是持久化层的第一级缓存，通常它是必需的；进程范围或集群范围的缓存是持久化层的第二级缓存，通常是可选的。

在进程范围或集群范围的缓存，即第二级缓存，会出现并发问题。因此可以设定以下四种类型的并发访问策略，每一种策略对应一种事务隔离级别。

 

事务型、读写型、非严格读写型、只读型

什么样的数据适合存放到第二级缓存中？

1 很少被修改的数据

2 不是很重要的数据，允许出现偶尔并发的数据

3 不会被并发访问的数据

4 参考数据

不适合存放到第二级缓存的数据？

1 经常被修改的数据

2 财务数据，绝对不允许出现并发

3 与其他应用共享的数据。

 

Hibernate的二级缓存策略的一般过程如下：

1) 条件查询的时候，总是发出一条select * from table_name where …. （选择所有字段）这样的SQL语句查询数据库，一次获得所有的数据对象。

2) 把获得的所有数据对象根据ID放入到第二级缓存中。

3) 当Hibernate根据ID访问数据对象的时候，首先从Session一级缓存中查；查不到，如果配置了二级缓存，那么从二级缓存中查；查不到，再查询数据库，把结果按照ID放入到缓存。

4) 删除、更新、增加数据的时候，同时更新缓存。

 

 

注意几个关键字的含义

inverse="true"表示此表不维护表之间的关系，由另外的表维护。

inverse属性默认是false的，就是说关系的两端都来维护关系。

false代表由己方来维护关系，true代表由对方来维护关系。在一个关系中，只能由一方来维护关系，否则会出问题（解疑中会讲到）；同时也必须由一方来维护关系，否则会出现双方互相推卸责任，谁也不管。

 

可以这样理解，cascade定义的是关系两端对象到对象的级联关系；而inverse定义的是关系和对象的级联关系。

Cascade属性的可能值有

    all: 所有情况下均进行关联操作，即save-update和delete。
    none: 所有情况下均不进行关联操作。这是默认值。
    save-update: 在执行save/update/saveOrUpdate时进行关联操作。
    delete: 在执行delete 时进行关联操作。

    all-delete-orphan: 当一个节点在对象图中成为孤儿节点时，删除该节点。比如在一个一对多的关系中，Student包含多个book，当在对象关系中删除一个book时，此book即成为孤儿节点。
来源： <[http://blog.163.com/benbenfafa_88/blog/static/649301622011523909352/](http://blog.163.com/benbenfafa_88/blog/static/649301622011523909352/)> 
