---
layout: post
title: "think in java interview-高级开发人员面试宝典(五)"
categories: 面试参考
tags: 
 - 面试参考
 - think in java interview
--- 

# think in java interview-高级开发人员面试宝典(五)

### [[置顶] think in java interview-高级开发人员面试宝典(五)]()

分类： [面经](http://blog.csdn.net/lifetragedy/article/category/1544093) 2013-08-10 17:07 1431人阅读 [评论]()(9) [收藏]( "收藏") [举报]( "举报")
目录[(?)]( "系统根据文章中H1到H6标签自动生成文章目录")[[+]]( "展开")

1. [Overriding Overloading]()
1. [Whats the difference between an interface and an abstract class]()
1. [What do you understand by Synchronization]()
1. [Synchronized statement and Synchronized block]()
1. [What is difference between jdbc30 and jdbc20 Whats the new features in jdbc30]()
1. [Whats the difference between the methods sleep and wait]()
1. [What is oops]()
1. [What is Authentication Authorization]()

1. [Authentication]()

1. [Password based authentication]()
1. [Device based authentication]()
1. [Biometric Authentication]()
1. [Retina Scanners]()
1. [Hand Scanners]()
1. [Authorization]()
1. [What is generic type]()
1. [What is autoboxing and unboxing]()
1. [What is equals and hashcode]()
1. []()
1. [下面开始来几道J2EE的面试题]()

1. []()
1. [What is the Lazy Loading]()
1. [What is MVC]()
1. [What is Spring]()
1. [Why use hibernate]()
1. []()
1. [What is dependency-injection]()
1. [Whats difference between criterial query and hql]()
1. [Pls tell me the advantage points or new features of Spring FrameWork]()
1. [What is spring transaction managerment]()

1. [the difference between BeanFactory and ApplicationContext]()
1. [Tell me what is AOP]()

1. [How many Types of advice are there in Spring Framework]()
1. [每篇不宜写过多先写这么多吧下次继续嘿嘿]()

这次开始我们来点洋文吧。

有些基础，大家可能用中文知道如何表示，但是面试官如果让你用全英语表达你就不知道如何去说了，那么下面我们将给出对于一些常用的JAVA基础知识的英语问答以及相关的答案。

大家可以看一下如何用英语去回答这些基础的问题，找一下感觉。

# []()Overriding & Overloading

Overriding - same method names with same arguments and same return types associated in a class and its subclass.
Overloading - same method name with different arguments, may or may not be same return type written in the same class itself.

# []()What's the difference between an interface and an abstract class?

* An abstract class may contain code in method bodies, which is not allowed in an interface. With abstract classes, you have to inherit your class from it and Java does not allow multiple inheritance. On the other hand, you can implement multiple interfaces in your class.
* What’s the difference between Hashtable and Hashmap
* They Both provide key-value access to data.
* The key difference between the two is that access to the Hashtable is synchronized on the table while access to the HashMap isn't. You can add it, but it isn't there by default.
* Another difference is that iterator in the HashMap is fail-safe while the enumerator for the Hashtable isn't.
* And, a third difference is that HashMap permits null values in it, while Hashtable doesn't.

# []()What do you understand by Synchronization?

Synchronization is a process of controlling the access of shared resources by the multiple threads in such a manner that only one thread can access one resource at a time. In non synchronized multithreaded application, it is possible for one thread to modify a shared object while another thread is in the process of using or updating the object's value. Synchronization prevents such type of data corruption.

# []()Synchronized statement and Synchronized block

Synchronized methods are methods that are used to control access to an object. A thread only executes a synchronized method after it has acquired the lock for the method'sobject or class. Synchronized statements are similar to synchronized methods, always use synchronized(this) to instead of.

# []()What is difference between jdbc3.0 and jdbc2.0? What’s the new features in jdbc3.0?

这道题可见老外的面试对基础是多么的重视，回答时掌握好6点

The JDBC 3.0 API introduces new material and changes in these areas:

**1. Savepoint support**
Added the Savepoint interface, which contains new methods to set, release, or roll back a transaction to designated savepoints.

**2. Reuse of prepared statements by connection pools**

Added the ability for deployers to control how prepared statements are pooled and reused by connections.

**3. Connection pool configuration Defined a number of properties for the ConnectionPoolDataSource interface.**

These properties can be used to describe how pooledConnection objects created by DataSource objects should be pooled.

**4. Retrieval of parameter metadata**

Added the interface ParameterMetaData, which describes the number, type and properties of parameters to prepared statements.

**5. Retrieval of auto-generated keys**

Added a means of retrieving values from columns containing automatically generated values.

**6. Ability to have multiple open ResultSet objects**

Added the new method getMoreResults(int), whichtakes an argument that specifies whether ResultSet objects returned by a Statement object should be closed before returning any subsequent ResultSet objects.

# []()What's the difference between the methods sleep() and wait()

简单了不能再简单的一道题了，可是用英语如何回答呢？来！

The code sleep(1000); puts thread aside for exactly one second. The code wait(1000), causes a wait of up to one second. A thread could stop waiting earlier if it receives the notify() or notifyAll() call. The method wait() is defined in the class Object and the method sleep() is defined in the class Thread

# []()What is oops

OOPs is the new concept of programming parallel to Procedure oriented programming
There are three main principals of oops which are called Polymorphism, Inheritance and Encapsulation
It help in programming approach in order to built robust user friendly and efficient softwares and provide the efficient way to maintain real world softwares..

# []()What is Authentication & Authorization

这道题很猛，来自于Oracle的一次面试。
Fundamentally, authentication is performed by a series of Spring Security filter (implementations of J2EE Servlet Filters) chains, linked together. Each element in a given chain has a dedicated responsibility, while each chain is responsible for accomplishing high-level goals towards the handling of a request. Ultimately, these chains must prepare a request to fulfill a single contract enforced by the last link in the primary security filter chain.

## []()
Authentication

An authentication system is how you identify yourself to the computer. The goal behind an authentication system is to verify that the user is actually who they say they are.
There are many ways of authenticating a user. Any combination of the following are good examples.

### []()**Password based authentication**

Requires the user to know some predetermined quantity (their password).

* Advantages: Easy to impliemnt, requires no special equipemnt.
* Disadvantages: Easy to forget password. User can tell another user their password. Password can be written down. Password can be reused.

### []()Device based authentication

Requires the user to posses some item such as a key, mag strip, card, s/key device, etc.

* Advantages: Difficult to copy. Cannot forget password. If used with a PIN is near useless if stolen.
* Disadvantages: Must have device to use service so the user might forget it at home. Easy target for theft. Still doesn't actually actively identify the user.

### []()Biometric Authentication

* My voice is my passport. Verify me. This is from the movie sneakers and demonstrates one type of biometric authentication device. It identifies some physical charactistic of the user that cannot be seperated from their body.

### []()Retina Scanners

* Advantages: Accurately identifies the user when it works.
* Disadvantages: New technology that is still evolving. Not perfect yet.

### []()Hand Scanners

* Advantages: Difficult to seperate from the user. Accurately identifies the user.
* Disadvantages: Getting your hand stolen to break into a vault sucks a lot more than getting your ID card stolen.

## []()Authorization

Once the system knows who the user is through authentication, authorization is how the system decides what the user can do.
A good example of this is using group permissions or the difference between a normal user and the superuser on a unix system.
There are other more compicated ACL (Access Control Lists) available to decide what a user can do and how they can do it. Most unix systems don't impliment this very well (if at all.)

# []()What is generic type?

**中文回答一下，谁都可以答得出，够简单吧，试着用英语回答呢？**
When you take an element out of a Collection, you must cast it to the type of element that is stored in the collection. Besides being inconvenient, this is unsafe. The compiler does not check that your cast is the same as the collection's type, so the cast can fail at run time.
Generics provides a way for you to communicate the type of a collection to the compiler, so that it can be checked. Once the compiler knows the element type of the collection, the compiler can check that you have used the collection consistently and can insert the correct casts on values being taken out of the collection.

# []()What is autoboxing and unboxing?

很多人中文可能都不一定回答得出这道题，自动装箱，折箱，忘了吧？
**Autoboxing**, introduced in Java 5, is the automatic conversion the Java compiler makes between the primitive (basic) types and their corresponding object wrapper classes (eg, int and Integer, double and Double, etc). The underlying code that is generated is the same, **but autoboxing** provides a sugar coating that avoids the tedious and hard-to-read casting typically required by Java Collections, which can not be used with primitive types.

# []()What is equals() and hashcode()

又来了，具体原理和算法看：[hashcode & equals之5重天](http://blog.csdn.net/lifetragedy/article/details/9751079)

Java.lang.Object has methods called hasCode() and equals(). These methods play a significant role in the real time application. However its use is not always common to all applications. In some case these methods are overridden to perform certain purpose. In this article I will explain you some concept of these methods and why it becomes necessary to override these methods.
****

**hashCode()**
As you know this method provides the has code of an object. Basically the default implementation of hashCode() provided by Object is derived by mapping the memory address to an integer value. If look into the source of Object class , you will find the following code for the hashCode.
public native int hashCode();
It indicates that hashCode is the native implementation which provides the memory address to a certain extent. However it is possible to override the hashCode method in your implementation class.

**equals()**
This particular method is used to make equal comparison between two objects. There are two types of comparisons in Java. One is using “= =” operator and another is “equals()”. I hope that you know the difference between this two. More specifically the “.equals()” refers to equivalence relations. So in broad sense you say that two objects are equivalent they satisfy the “equals()” condition. If you look into the source code of Object class you will find the following code for the equals() method

# []()

# []()下面开始来几道J2EE的面试题

## []()

## []()What is the Lazy Loading

Lazy loading in Hibernate means fetching and loading the data,
only when it is needed, from a persistent storage like a database.
Lazy loading improves the performance of data fetching and significantly reduces the memory footprint.
In Hibernate, there are two main components which can be lazy loaded.
They are namely Entities and Collections.
An Entity represents a relational data in database, whereas a collection represents collection of children for an entity.

## []()**What is MVC**

Model–view–controller (MVC) is an [architecturalpattern](http://en.wikipedia.org/wiki/Architectural_pattern_%28computer_science%29 "Architectural pattern (computer science)") used in [softwareengineering](http://en.wikipedia.org/wiki/Software_engineering "Software engineering"). Successful use of the pattern isolates [business logic](http://en.wikipedia.org/wiki/Business_logic "Business logic") from [user interface](http://en.wikipedia.org/wiki/User_interface "User interface") considerations, resulting inan application where it is easier to modify either the visual appearance of theapplication or the underlying business rules without affecting the other****

Model

Business logic goes here

View

Presentation logic goes here

Controller

Application logic goes here

## []()What is Spring

Springfeature:

The Spring Framework comprisesseveral modules that provide a range of services:

* [Inversion of Control](http://en.wikipedia.org/wiki/Inversion_of_Control "Inversion of Control") container: configuration of application components and lifecycle management of Java objects
* [Aspect-oriented programming](http://en.wikipedia.org/wiki/Aspect-oriented_programming "Aspect-oriented programming"): enables implementation of cross-cutting routines
* [Data access](http://en.wikipedia.org/wiki/Data_access "Data access"): working with [relational database management systems](http://en.wikipedia.org/wiki/RDBMS "RDBMS") on the Java platform using [JDBC](http://en.wikipedia.org/wiki/JDBC "JDBC") and [object-relational mapping](http://en.wikipedia.org/wiki/Object-relational_mapping "Object-relational mapping") tools
* [Transaction management](http://en.wikipedia.org/wiki/Transaction_processing "Transaction processing"): unifies several transaction management APIs and coordinates transactions for Java objects
* [Model-view-controller](http://en.wikipedia.org/wiki/Model-view-controller "Model-view-controller"): an [HTTP](http://en.wikipedia.org/wiki/HTTP "HTTP") and [Servlet](http://en.wikipedia.org/wiki/Java_Servlet_API "Java Servlet API")-based framework providing hooks for extension and customization
* Remote Access framework: configurative [RPC](http://en.wikipedia.org/wiki/Remote_procedure_call "Remote procedure call")-style export and import of Java objects over networks supporting [RMI](http://en.wikipedia.org/wiki/Java_remote_method_invocation "Java remote method invocation"), [CORBA](http://en.wikipedia.org/wiki/CORBA "CORBA") and [HTTP](http://en.wikipedia.org/wiki/HTTP "HTTP")-based protocols including [web services](http://en.wikipedia.org/wiki/Web_services "Web services") ([SOAP](http://en.wikipedia.org/wiki/SOAP_%28protocol%29 "SOAP (protocol)"))
* [Batch processing](http://en.wikipedia.org/wiki/Batch_processing "Batch processing"): a framework for high-volume processing featuring reusable functions including logging/tracing, transaction management, job processing statistics, job restart, skip, and resource management
* [Authentication](http://en.wikipedia.org/wiki/Authentication "Authentication") and [authorization](http://en.wikipedia.org/wiki/Authorization "Authorization"): configurable security processes that support a range of standards, protocols, tools and practices via the [Spring Security](http://en.wikipedia.org/wiki/Spring_Security "Spring Security") sub-project (formerly [Acegi](http://en.wikipedia.org/wiki/Acegi_security_framework_%28Java%29 "Acegi security framework (Java)")).
* Remote Management: configurative exposure and management of Java objects for local or remote configuration via [JMX](http://en.wikipedia.org/wiki/JMX "JMX")
* Messaging: configurative registration of message listener objects for transparent message consumption from [message queues](http://en.wikipedia.org/wiki/Message_queue "Message queue") via [JMS](http://en.wikipedia.org/wiki/Java_Message_Service "Java Message Service"), improvement of message sending over standard JMS APIs
* [Testing](http://en.wikipedia.org/wiki/Software_testing "Software testing"): support classes for writing unit tests and integratio

## []()**Why use hibernate**

1. Hibernate generates very efficient queries very consistently

Hibernateemploys very agressive, and very intelligent first and second level cachingstrategy.

Thisis a major factor in acheiving the high scalability.

2. Cross-Database You only have to change the databasedialect.

## []()

## []()What is dependency-injection

Software components (Clients), are often a part of a set of collaborating components which depend upon other components (Services) to successfully complete their intended purpose. In many scenarios, they need to know “which” components to communicate with, “where” to locate them, and “how” to communicate with them. When the way such services can be accessed is changed, such changes can potentially require the source of lot of clients to be changed.

One way of structuring the code is to let the clients embed the logic of locating and/or instantiating the services as a part of their usual logic. Another way to structure the code is to have the clients declare their dependency on services, and have some "external" piece of code assume the responsibility of locating and/or instantiating the services and simply supplying the relevant service references to the clients when needed.

## []()What's difference between criterial query and hql:

Criteria is an object-oriented API, while HQL means string concatenation. That means all of the benefits of object-orientedness apply:
1. All else being equal, the OO version is somewhat less prone to error. Any old string could get appended into the HQL query, whereas only valid Criteria objects can make it into a Criteria tree. Effectively, the Criteria classes are more constrained.
2. With auto-complete, the OO is more discoverable (and thus easier to use, for me at least). You don't necessarily need to remember which parts of the query go where; the IDE can help you
3. You also don't need to remember the particulars of the syntax (like which symbols go where). All you need to know is how to call methods and create objects.
Since HQL is very much like SQL (which most devs know very well already) then these "don't have to remember" arguments don't carry as much weight. If HQL was more different, then this would be more importatnt.

## []()Pls tell me the advantage points or new features of Spring FrameWork?

The Spring Framework comprises several modules that provide a range of services:

**Inversion of Control container:** configuration of application components and lifecycle management of Java objects

**Aspect-oriented programming**: enables implementation of cross-cutting routines
Data access: working with relational database management systems on the Java platform using JDBC and object-relational mapping tools
****

**Transaction management**: unifies several transaction management APIs and coordinates transactions for Java objects
**Model-view-controller**: an HTTP and Servlet-based framework providing hooks for extension and customization
****

**Remote Access framework**: configurative RPC-style export and import of Java objects over networks supporting RMI, CORBA and HTTP-based protocols including web services (SOAP)
****

**Batch processing**: a framework for high-volume processing featuring reusable functions including logging/tracing, transaction management, job processing statistics, job restart, skip, and resource management
****

**Authentication and authorization**: configurable security processes that support a range of standards, protocols, tools and practices via the Spring Security sub-project (formerly Acegi).
Remote Management: configurative exposure and management of Java objects for local or remote configuration via JMX
****

**Messaging**: configurative registration of message listener objects for transparent message consumption from message queues via JMS, improvement of message sending over standard JMS APIs
****

**Testing**: support classes for writing unit tests and integratio

# []()What is spring transaction managerment?

Spring provides a consistent abstraction for transaction management. This abstraction is one of the most important of Spring's abstractions, and delivers the following benefits:
Provides a consistent programming model across different transaction APIs such as JTA, JDBC, Hibernate, iBATIS Database Layer and JDO.
Provides a simpler, easier to use, API for programmatic transaction management than most of these transaction APIs
Integrates with the Spring data access abstraction
Supports Spring declarative transaction management

## []()the difference between BeanFactory and ApplicationContext ？

Two of the most fundamental and important packages in Spring are the org.springframework.beans and org.springframework.context packages. Code in these packages provides the basis for Spring's Inversion of Control (alternately called Dependency Injection) features. The BeanFactory provides an advanced configuration mechanism capable of managing beans (objects) of any nature, using potentially any kind of storage facility. The ApplicationContext builds on top of the BeanFactory (it's a subclass) and adds other functionality such as easier integration with Springs AOP features, message resource handling (for use in internationalization), event propagation, declarative mechanisms to create the ApplicationContext and optional parent contexts, and application-layer specific contexts such as the WebApplicationContext, among other enhancements.

# []()Tell me what is AOP?

**1. Aspect**: A modularization of a concern that cuts across multiple objects. Transaction management is a good example of a crosscutting concern in J2EE applications.

**2. Join point
**

**3. Advice
**

**4. Pointcut**

**5. Introduction
**

**6. Target object
**

**7. Weaving
**

## []()How many Types of advice are there in Spring Framework:

* Before advice
* After returning advice
* After throwing advice
* After (finally) advice
* Around advice

# []()每篇不宜写过多，先写这么多吧，下次继续，嘿嘿！！！
分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")

* 上一篇：[think in java interview-高级开发人员面试宝典(四)](http://blog.csdn.net/lifetragedy/article/details/9812419)
* 下一篇：[think in java interview-高级开发人员面试宝典(六)](http://blog.csdn.net/lifetragedy/article/details/9935699)
顶4 踩0

查看评论[]()
4楼 [liwei0602_](http://blog.csdn.net/u011278496) 4天前 11:36发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/u011278496)看了你的博客，觉得自己真是什么都不会了。
膜拜一下。 3楼 [kenanliming](http://blog.csdn.net/kenanliming) 4天前 16:48发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/kenanliming)一直对自己说狠下心学习java,总是被各种事情耽误了，哎，现在的社会已经让人没时间去学习了 Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 4天前 17:52发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复kenanliming：真要学，到处都是时间，打游戏，看电视，看电影，泡妞，吃零食这些时间我相信多少每天能挤出个2小时来吧，坚持2-3年呢？4年不看电视，不打游戏，又如何，你损失的是什么得到的是什么呢？对吧！ 2楼 [水哥709](http://blog.csdn.net/leon709) 2013-08-11 21:58发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/leon709)What is IOC? Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-11 23:15发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复leon709：expecting u to provide the answer Man:) 1楼 [水哥709](http://blog.csdn.net/leon709) 2013-08-10 18:51发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/leon709)我上次去Morgan Stanley面试，也是差不多，全英文，问基础，问细节的，拿个纸和笔，直接让我写代码，让我实现一个HashMap，我答得不好，还问了数据库聚簇索引和非聚簇索引等。最后败下来了，以后有机会再去。 Re: [tangzhenyu1990](http://blog.csdn.net/tangzhenyu1990) 2013-08-13 17:23发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/tangzhenyu1990)回复leon709：大神，你是面试正式岗还是实习生啊？ Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-10 18:55发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复leon709：一般好点的公司的笔试是这样的，面试官把笔记本给你，让你在ECLIPSE里敲代码，然后敲完代码后直接拖到JUNIT里运行，这样比较好，嘿！
你还算好，Morgan Stanley还有一次面到过，让人实现：
<{[()]}>这样的，括号配对，让你写出算法，就纸和笔，嘿。
其实，这都是平时的基本功！ Re: [defty](http://blog.csdn.net/defty) 2013-08-12 14:00发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/defty)回复lifetragedy：估计你没遇到过，让你在黑板上写代码，然后面试官开着笔记本把你的代码敲到eclipse里去，然后告诉你编译错误或者结果不对

您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Flifetragedy%2Farticle%2Fdetails%2F9878839)
* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()

[![TOP]()]( "回到顶部")
