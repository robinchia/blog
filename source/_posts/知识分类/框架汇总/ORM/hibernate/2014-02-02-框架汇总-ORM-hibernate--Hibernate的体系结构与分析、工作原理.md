---
layout: post
title: "Hibernate的体系结构与分析、工作原理"
categories: 框架汇总
tags: 
 - 框架汇总
 - ORM
 - hibernate
--- 

# Hibernate的体系结构与分析、工作原理

本文摘自 李刚 著 《Java EE企业应用实战》

 

 

        现在我们知道了一个概念**Hibernate Session**，只有处于**Session**管理下的**POJO**才具有持久化操作能力。当应用程序对于处于**Session**管理下的**POJO**实例执行操作时，**Hibernate**将这种面向对象的操作转换成了持久化操作能力。

HIbernate简要的体系结构如下图所示：

![]()

        通过上图能够发现HIbernate需要一个**hibernate.properties**文件，该文件用于配置**Hibernate**和数据库连接的信息。还需要一个XML文件，该映射文件确定了持久化类和数据表、数据列之间的想对应关系。

除了使用hibernate.properties文件，还可以采用另一种形式的配置文件： ***.cfg.xml**文件。在实际应用中，采用XML配置文件的方式更加广泛，两种配置文件的实质是一样的。

        Hibernate的持久化解决方案将用户从赤裸裸的JDBC访问中释放出来，用户无需关注底层的JDBC操作，而是以面向对象的方式进行持久层操作。底层数据连接的获得、数据访问的实现、事务控制都无需用户关心。这是一种“全面解决”的体系结构方案，将应用层从底层的JDBC/JTA API中抽象出来。通过配置文件来管理底层的JDBC连接，让Hibernate解决持久化访问的实现。这种“全面解决”方案的体系结构图如图所示：

![]()

        针对以上的Hibernate全面解决方案架构图：

      （1）**SessionFactory**：**这是Hibernate的关键对象，**它是单个数据库映射关系经过编译后的内存镜像，它也是**线程安全的**。**它是生成Session的工厂**，本身要应用到ConnectionProvider，该对象可以在进程和集群的级别上，为那些事务之间可以重用的数据提供可选的二级缓存。

      （2）**Session**：它是应用程序和持久存储层之间交互操作的一个单线程对象。它也是Hibernate持久化操作的关键对象，所有的持久化对象必须在Session的管理下才能够进行持久化操作。**此对象的生存周期很短，其隐藏了JDBC连接，也是Transaction 的工厂。Session对象有一个一级缓存，现实执行Flush之前，所有的持久化操作的数据都在缓存中Session对象处。**

      （3）**持久化对象**：系统创建的POJO实例一旦与特定Session关联，并对应数据表的指定记录，那该对象就处于持久化状态，这一系列的对象都被称为持久化对象。程序中对持久化对象的修改，都将自动转换为持久层的修改。**持久化对象完全可以是普通的Java Beans/POJO，唯一的特殊性是它们正与Session关联着。**

      （4）**瞬态对象和脱管对象**：系统进行new关键字进行创建的Java 实例，没有Session 相关联，此时处于瞬态。瞬态实例可能是在被应用程序实例化后，尚未进行持久化的对象。如果一个曾今持久化过的实例，但因为Session的关闭而转换为脱管状态。

      （5）**事务(Transaction)**：代表一次原子操作，它具有数据库事务的概念。但它通过抽象，将应用程序从底层的具体的JDBC、JTA和CORBA事务中隔离开。在某些情况下，一个Session 之内可能包含多个Transaction对象。虽然事务操作是可选的，但是所有的持久化操作都应该在事务管理下进行，即使是只读操作。

      （6）**连接提供者(ConnectionProvider)：****它是生成JDBC的连接的工厂，同时具备连接池的作用。他通过抽象将底层的DataSource和DriverManager隔离开。这个对象无需应用程序直接访问，仅在应用程序需要扩展时使用。**

      （7）事务工厂(TransactionFactory)：他是生成Transaction对象实例的工厂。该对象也无需应用程序的直接访问。
来源： <[http://blog.csdn.net/titilover/article/details/6920457](http://blog.csdn.net/titilover/article/details/6920457)> 

    前面说了一些宏观上学习框架相关的思想方面的东西，下面继续来介绍我经常使用的框架和框架的分析，这篇博客主要介绍的是hibernate框架。

      首先说hibernate框架是数据持久层的框架，这个框架是非常强大的，它让编程人员纯粹的用面向对象的方式来做开发，让编程人员所面对的都是对象。仅仅从这一点它的设计思路就是非常让编程人员喜爱的。

      回想我们普通的开发流程，和客户沟通定需求，抽象出来原型，从原型中建立数据模型到库表结构的建立，之后在映射成对象模型，之后在用oo的设计思想完成后续的程序开发。但是当我们使用了hibernate框架以后，原先的设计思路就显得不再那么具有优势了。我们直接建立对象模型，之后利用hibernate框架映射成数据模型，我们不再去考虑数据库关系模型的东西，仅仅考虑的东西仅仅就是类和对象，这样的开发才是面向对象的开发，也才是最接近人类思考问题的方式。所以hibernate框架的设计思路是非常好的。

        hibernate框架设计思路的优越性其实体现在了它本身的框架的原理上。hibernate封装了JDBC，减轻了开发人员在持久层的大量重复性工作，它利用了java反射机制来实现程序的透明性；它就是通过这两点才达到从对象出发而非关系数据库出发的效果。

 

      介绍这么多理论性的东西之后我们能够感觉到hibernate框架的强大，来看看它的结构图：

 

![]()
 

       在hibernate框架中有几个比较重要的接口和类：

1. Query接口：Query负责执行各种数据库查询。它可以使用HQL语句或SQL语句两种表达方式。

1. Configuration类：Configuration类负责配置并启动Hibernate，创建SessionFactory对象

1. SessionFactory接口：SessionFactory接口负责初始化Hibernate。它充当数据存储源的代理，并负责创建Session对象

1. Session接口：Session接口负责执行被持久化对象的CRUD操作

1. Transaction接口：Transaction接口负责事务相关的操作

 

         hibernate框架就是在利用这几个接口来封装了JDBC，而且我们用这些接口来操作数据库变得非常简单，减少了我们在持久层的代码量。

       从这个结构图和我的一些分析就能发现hibernate框架是非常强大，而且它给我们开发人员的开发带来了非常大的便利，尤其是他的设计思路还有它的“全自动”的映射对象模型和关系模型。

       但是hibernate框架也有它的一些缺点：

1. 既然是封装了JDBC，所以很明显它没有JDBC的效率高，尤其是在大量的处理表更新操作的时候。

1. 它有局限性，一个持久化类不能映射多个表
1. 它应对大数量的时候显得非常笨拙，这一点没有JDBC和接下来要介绍的IBatis框架

 

       其实一项技术或者一个框架都有它的优缺点，选择最合适的才是王道。

 

      
来源： <[http://blog.csdn.net/lfsf802/article/details/7941958](http://blog.csdn.net/lfsf802/article/details/7941958)> 

Hibernate工作原理：
 

1.Hibernate 的初始化.

读取Hibernate 的配置信息-〉创建Session Factory

1)创建Configeration类的实例：

它的构造方法：将配置信息(Hibernate config.xml)读入到内存。 
一个Configeration 实例代表Hibernate 所有Java类到Sql数据库映射的集合。

2)创建SessionFactory实例：

把Configeration 对象中的所有配置信息拷贝到SessionFactory的缓存中。 
SessionFactory的实例代表一个数据库存储员源，创建后不再与Configeration 对象关联。

3)调用SessionFactory创建Session的方法：

        1】用户自行提供JDBC连接。

        Connection con=dataSource.getConnection();
        Session s=sessionFactory.openSession(con);

        2】让SessionFactory提供连接

        Session s=sessionFactory.openSession();

4)通过Session 接口提供的各种方法来操纵数据库访问。

Hibernate 的缓存体系

一级缓存：

Session 有一个内置的缓存，其中存放了被当前工作单元加载的对象。 
每个Session 都有自己独立的缓存，且只能被当前工作单元访问。

二级缓存：

SessionFactory的外置的可插拔的缓存插件。其中的数据可被多个Session共享访问。

SessionFactory的内置缓存：存放了映射元数据，预定义的Sql语句。

Hibernate 中Java对象的状态

1.临时状态 (transient)

特征：

1】不处于Session 缓存中
2】数据库中没有对象记录

Java如何进入临时状态

1】通过new语句刚创建一个对象时 
2】当调用Session 的delete()方法，从Session 缓存中删除一个对象时。

2.持久化状态(persisted)

特征：

1】处于Session 缓存中 
2】持久化对象数据库中设有对象记录 
3】Session 在特定时刻会保持二者同步

Java如何进入持久化状态

1】Session 的save()把临时－》持久化状态 
2】Session 的load(),get()方法返回的对象 
3】Session 的find()返回的list集合中存放的对象 
4】Session 的update(),saveOrupdate()使游离－》持久化

3.游离状态(detached)

特征：

1】不再位于Session 缓存中 
2】游离对象由持久化状态转变而来，数据库中可能还有对应记录。

Java如何进入持久化状态－》游离状态

1】Session 的close()方法 
2】Session 的evict()方法，从缓存中删除一个对象。提高性能。少用。

 

总结一下Hibernate的好处：

1.JDBC访问数据库的代码做了封装，大大简化了数据访问层繁琐的重复性代码。

2. Hibernate是一个基于JDBC的主流持久化框架，是一个优秀的ORM实现。他很大程度的简化DAO层的编码工作

3. hibernate使用Java反射机制，而不是字节码增强程序来实现透明性。

4. hibernate的性能非常好，因为它是个轻量级框架。映射的灵活性很出色。它支持各种关系数据库，从一对一到多对多的各种复杂关系。
来源： <[http://www.linuxidc.com/Linux/2012-12/76684.htm](http://www.linuxidc.com/Linux/2012-12/76684.htm)> 

![]()
