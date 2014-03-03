---
layout: post
title: "Spring-设计模式-aop-ioc"
categories: 框架汇总
tags: 
 - 框架汇总
 - Spring
--- 

# Spring-设计模式-aop-ioc

[黄金档](http://www.goldendoc.org/)

路漫漫其修远兮，吾将上下而求索……

* [首页](http://www.goldendoc.org/ "首页")
* [招聘](http://www.goldendoc.org/hire/)
* [关于](http://www.goldendoc.org/about/)

# Spring

## [Spring中的设计模式](http://www.goldendoc.org/2010/12/spring_design_pattern/ "永久链接：Spring中的设计模式")

[2](http://www.goldendoc.org/2010/12/spring_design_pattern/#comments)

2年前

由[jackey](http://www.goldendoc.org/author/jackey/ "jackey发表的文章 ")发布 在[Spring](http://www.goldendoc.org/category/spring/ "Spring (4主题)")
应该说设计模式是我们在写代码时候的一种被承认的较好的模式。好的设计模式就像是给代码造了一个很好的骨架，在这个骨架里，你可以知道心在哪里，肺在哪里，因为大多数人都认识这样的骨架，就有了很好的传播性。这是从易读和易传播来感知设计模式的好处。当然设计模式本身更重要的是设计原则的一种实现，比如开闭原则，依赖倒置原则，这些是在代码的修改和扩展上说事。说到底就是人类和代码发生关系的四种场合：阅读，修改，增加，删除。让每一种场合都比较舒服的话，就需要用设计模式。

下面来简单列举Spring中的设计模式：
**1. 简单工厂**
又叫做静态工厂方法（StaticFactory Method）模式，但不属于23种GOF设计模式之一。
简单工厂模式的实质是由一个工厂类根据传入的参数，动态决定应该创建哪一个产品类。
Spring中的BeanFactory就是简单工厂模式的体现，根据传入一个唯一的标识来获得Bean对象，但是否是在传入参数后创建还是传入参数前创建这个要根据具体情况来定。
**2. 工厂方法（Factory Method）**
定义一个用于创建对象的接口，让子类决定实例化哪一个类。Factory Method使一个类的实例化延迟到其子类。
Spring中的FactoryBean就是典型的工厂方法模式。如下图：
[![]()](http://pic.yupoo.com/goldendoc/AFbYXQy8/cgvMF.jpg)

 

**3. 单例（Singleton）**
保证一个类仅有一个实例，并提供一个访问它的全局访问点。
Spring中的单例模式完成了后半句话，即提供了全局的访问点BeanFactory。但没有从构造器级别去控制单例，这是因为Spring管理的是是任意的Java对象。
**4. 适配器（Adapter）**
将一个类的接口转换成客户希望的另外一个接口。Adapter模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。
Spring中在对于AOP的处理中有Adapter模式的例子，见如下图：
[![]()](http://pic.yupoo.com/goldendoc/AFbYXG9r/yaKsE.jpg)
由于Advisor链需要的是MethodInterceptor对象，所以每一个Advisor中的Advice都要适配成对应的MethodInterceptor对象。
**5.包装器（Decorator）**
动态地给一个对象添加一些额外的职责。就增加功能来说，Decorator模式相比生成子类更为灵活。 ****

[![]()](http://pic.yupoo.com/goldendoc/AFbYXVzM/iEPst.jpg)
Spring中用到的包装器模式在类名上有两种表现：一种是类名中含有Wrapper，另一种是类名中含有Decorator。基本上都是动态地给一个对象添加一些额外的职责。
**6. 代理（Proxy）**
为其他对象提供一种代理以控制对这个对象的访问。
从结构上来看和Decorator模式类似，但Proxy是控制，更像是一种对功能的限制，而Decorator是增加职责。
![]()
Spring的Proxy模式在aop中有体现，比如JdkDynamicAopProxy和Cglib2AopProxy。
**7.观察者（Observer）**
定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。
[![]()](http://pic.yupoo.com/goldendoc/AFbYY79B/QRttX.jpg)
Spring中Observer模式常用的地方是listener的实现。如ApplicationListener。
**8. 策略（Strategy）**
定义一系列的算法，把它们一个个封装起来，并且使它们可相互替换。本模式使得算法可独立于使用它的客户而变化。
Spring中在实例化对象的时候用到Strategy模式，见如下图：
[![]()](http://pic.yupoo.com/goldendoc/AFbYYjl6/c2IJb.jpg)
在SimpleInstantiationStrategy中有如下代码说明了策略模式的使用情况：
[![]()](http://pic.yupoo.com/goldendoc/AFbYYecL/xJ2n1.jpg)
**9.模板方法（Template Method）**
定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。Template Method使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。
![]()
Template Method模式一般是需要继承的。这里想要探讨另一种对Template Method的理解。Spring中的JdbcTemplate，在用这个类时并不想去继承这个类，因为这个类的方法太多，但是我们还是想用到JdbcTemplate已有的稳定的、公用的数据库连接，那么我们怎么办呢？我们可以把变化的东西抽出来作为一个参数传入JdbcTemplate的方法中。但是变化的东西是一段代码，而且这段代码会用到JdbcTemplate中的变量。怎么办？那我们就用回调对象吧。在这个回调对象中定义一个操纵JdbcTemplate中变量的方法，我们去实现这个方法，就把变化的东西集中到这里了。然后我们再传入这个回调对象到JdbcTemplate，从而完成了调用。这可能是Template Method不需要继承的另一种实现方式吧。

以下是一个具体的例子：
JdbcTemplate中的execute方法：

[![]()](http://pic.yupoo.com/goldendoc/AFbYXJ1i/Gqxeo.jpg)
JdbcTemplate执行execute方法：
[![]()](http://pic.yupoo.com/goldendoc/AFbYYoCZ/coCXA.jpg)

[设计模式](http://www.goldendoc.org/tag/%e8%ae%be%e8%ae%a1%e6%a8%a1%e5%bc%8f/ "设计模式 (1主题)")

## [Spring AOP介绍及源码分析](http://www.goldendoc.org/2010/12/spring_aop/ "永久链接：Spring AOP介绍及源码分析")

[3](http://www.goldendoc.org/2010/12/spring_aop/#comments)

2年前

由[lwei](http://www.goldendoc.org/author/lwei/ "lwei发表的文章 ")发布 在[Spring](http://www.goldendoc.org/category/spring/ "Spring (4主题)")
软件开发经历了从汇编语言到高级语言和从过程化编程到面向对象编程；前者是为了提高开发效率，而后者则使用了归纳法，把具有共性的东西进行归类并使之模块化，达到便于维护和扩展的目的；如果说面向对象编程可以对业务需求进行很好的分解使之模块化；那么面向切面编程AOP（Aspect-Oriented Programming）则可以对系统需求进行很好的模软件开发经历了从汇编语言到高级语言和从过程化编程到面向对象编程；前者是为了提高开发效率，而后者则使用了归纳法，把具有共性的东西进行归类并使之模块化，达到便于维护和扩展的目的；如果说面向对象编程可以对业务需求进行很好的分解使之模块化；那么面向切面编程AOP（Aspect-Oriented Programming）则可以对系统需求进行很好的模块组织，简化系统需求和实现之间的对比关系，是对OOP思想的一种补充；块组织，简化系统需求和实现之间的对比关系，是对OOP思想的一种补充；

 

**一、AOP介绍
**

举个例子来说明一下吧！现在系统中有很多的业务方法，如上传产品信息、修改产品信息、发布公司库等；现在需要对这些方法的执行做性能监控，看每个业务方法的执行时间；在不改变原业务代码的基础上，也许我们会这么做：

**Offer接口：
**

![]()

**Offer实现：
**

![]()

**Offer代理：
**

![]()

我们要通过下面的方式来使用：

![]()

上面的例子的输出为：

![]()

上面的例子中，OfferProxy实现了IOffer，而所有的业务实现均委托给其成员offer；可以想像，这应该就是最简单的AOP的实现了；但这种方式会存在一个问题：如果有非常多的这种业务对象需要性能监控，我们就需要写同样多的XyzProxy来满足需求，这也是非常巨大的工作量。

上面说到了代理，我们先看看代理模式吧！

 

**二、代理模式及实现
**

下面是代理模式的类图：

![]()

代理模式类图

代理模式中，存在一个称为ProxyObject的代理对象和RealObject的真实对象，它们都实现了相同的接口；在调用的地方持有ProxyObject的实例，当调用request()方法时，ProxyObject可以在执行RealObject.request()前后做一些特定的业务，甚至不调用RealObject.request()方法。

目前实现代理模式的方式有两种：基于JDK的动态代理和基于CGLIB字节码的代理。

**2.1 JDK动态代理
**

JDK动态代理，顾名思义，是基于JDK的反射(reflect)机制；在JDK中，提供了InvocationHandler这个接口，下面是JDK里面的注释：

 

InvocationHandler is the interface implemented by the invocation handler of a proxy instance.

Each proxy instance has an associated invocation handler. When a method is invoked on a proxy instance, the method invocation is encoded and dispatched to the invoke method of its invocation handler.

 

简单翻译，意思是说：该接口由被代理对象的handler所实现；当调用代理对象的方法时，该方法调用将被编码，然后交给代理对象的invoke方法去执行；

是不是有一种豁然开朗的感觉呢？没错，答案就在你心中。

这样，上面的代码就可以改成下面的实现方式：

![]()****

**调用端：
**

![]()

通过这种方式，你不需要为针对每一个业务写一个代理对象，就可以很轻松地完成你的需求；但也许你已经注意到了，JDK的动态代理，在创建代理对象(上面红色代码部分)时，被代理的对象需要实现接口(即面向接口编程)；

这就是JDK的动态代理，简单吧！下面看看CGLIB代理方式。

**2.2 CGLIB代理
**

如果目标对象没有实现任何接口，那怎么办呢？不用担心，你可以用CGLIB来实现代理：

![]()

**调用端：
**

![]()

使用CGLIB创建的代理对象，其实就是继承了要代理的目标类，然后对目标类中所有非final方法进行覆盖，但在覆盖方法时会添加一些拦截代码(上面CglibProxyFactory类中的intercept方法)。

下面看看Spring中是如何实现AOP的。

 

**三、Spring AOP的实现
**

 

**3.1 Spring AOP的几个概念
**

Spring AOP中的几个基本概念，每次学习AOP都被这几个概念折腾的很不爽，我们在这里再把这几个概念描述一遍，力争把这几个概念搞清，在每次review这块内容的时候可以很快上手。

1. 切面(Aspect)：切面就是一个关注点的模块化，如事务管理、日志管理、权限管理等；
1. 连接点(Joinpoint)：程序执行时的某个特定的点，在Spring中就是一个方法的执行；
1. 通知(Advice)：通知就是在切面的某个连接点上执行的操作，也就是事务管理、日志管理等；
1. 切入点(Pointcut)：切入点就是描述某一类选定的连接点，也就是指定某一类要织入通知的方法；
1. 目标对象(Target)：就是被AOP动态代理的目标对象；

用一张图来形象地表达AOP的概念及其关系如下：

![]()

 

**3.2 Spring AOP中切入点、通知、切面的实现
**

理解了上面的几个概念后，我们分别来看看Spring AOP是如何实现这些概念的；

1. 切入点(Pointcut)：它定义了哪些连接点需要被织入横切逻辑；在Java中，连接点对应哪些类(接口)的方法。因此，我们都能猜到，所谓的切入点，就是定义了匹配哪些娄的哪些方法的一些规则，可以是静态的基于类(方法)名的值匹配，也可以是基于正则表达式的模式匹配。来看看Spring AOP Pointcut相关的类图：

![]()

在Pointcut接口的定义中，也许你已经想到了，ClassFilter是类过滤器，它定义了哪些类名需要拦截；典型的两个实现类为**TypePatternClassFilter**和**TrueClassFilter**(所有类均匹配)；而MethodMatcher为方法匹配器，定义哪些方法需要拦截。

在上面的类图中：

* StaticMethodMatch与DynamicMethodMatch的区别是后者在运行时会依据方法的参数值进行匹配。
* NameMatchMethodPointCut根据指定的mappedNames来匹配方法。
* AbstractRegexpMethodPointCut根据正则表达式来匹配方法。

1. 通知(Advice)：通知定义了具体的横切逻辑。在Spring中，存在两种类型的Advice，即per-class和per-instance的Advice。

所谓per-class，即该类型的Advice只提供方法拦截，不会为目标对象保存任何状态或者添加新的特性，它也是我们最常见的Advice。下面是per-class的类图：

![]()

* BeforeAdvice：在连接点前执行的横切逻辑。
* AfterReturningAdvice：在连接点执行后，再执行横切逻辑。
* AfterAdvice：一般由程序自己实现，当抛出异常后，执行横切逻辑。
* AroundAdvice：Spring AOP中并没有提供这个接口，而是采用了AOP Alliance的MethodInteceptor接口；通过看AfterReturningAdvice的源码我们知道，它是不能更改连接点所在方法的返回值的(更改引用)；但使用的MethodInteceptor，所有的事情，都不在话下。

在上面的类图中，还有两种类没有介绍，那就是***AdviceAdapter和***AdviceInteceptor，我们以AfterReturningAdviceInterceptor为例来说明：

![]()

该类实现了MethodInterceptor和AfterAdvice接口，同时构造函数中还有一个AfterReturningAdvice实例的参数；这个类存在的作用是为了什么呢？对，没错，Spring AOP把所有的Advice都适配成了MethodInterceptor，统一的好处是方便后面横切逻辑的执行(参看下一节)，适配的工作即由***AdviceAdapter完成；

哈哈，Spring AOP的代码也不过如此嘛：所谓的AfterReturningAdvice，通过适配成MethodInterceptor后，其实就是在invoke方法中，先执行目标对象的方法，再执行的AfterReturningAdvice所定义的横切逻辑。你现在明白它为什么不能修改返回值的引用了吧？

对于per-instance的Advice，目前只有一种实现，就是Introduction，使用的场景比较少，有兴趣的同学可以自己研究一下，呵呵！

1. 切面(Aspect)：在Spring中，Advisor就是切面；但与通常的Aspect不同的是，Advisor通常只有一个Pointcut和一个Advice，而Aspect则可以包含多个Pointcut和多个Advice，因此Advisor是一种特殊的Aspect。但，这已经够用了！

接下来看下per-class Advisor的类图：

![]()

其实没有什么好看的，前面已经说过，Advisor包含一个Pointcut和一个Advisor；在AbstractGenericPointcutAdvisor中，持有一个Advice的引用；下面的几个实现，均是针对前面提到的几种不同的Pointcut的实现。

 

**3.3 Spring AOP实现的基本线索
**

我们选择ProxyFactoryBean作为入口点和分析的开始。ProxyFactoryBean是在Spring IoC环境中，创建AOP应用的最底层方法，从中，可以看到一条实现AOP的基本线索。

所有的逻辑从以下的方法开始,我们主要针对单例的代理对象的生成：

![]()

下面我们深入到SpringAOP核心代码的内部，看看代理对象的生成机制，拦截器横切逻辑以及织入的实现。

 

**3.4  代理对象的生成
**

对于getSingletonInstance()方法返回了什么，这就是代理对象如何产生的逻辑了，然我们须根溯源，看看传说中的proxy到底是如何一步一步的产生的。

![]()

ProxyFactoryBean是AdvisedSupport的子类，Spring使用AopProxy接口把AOP代理的实现与框架的其他部分分离开来。在AdvisedSupport中通过这样的方式来得到AopProxy,当然这里需要得到AopProxyFactory的帮助 ，从JDK或者cglib中得到想要的代理对象：

![]()

这个DefaultAopProxyFactory是Spring用来生成AopProxy的地方，它包含JDK和Cglib两种实现方式。让我接着往里面看：

![]()

可以看到其中的代理对象可以由JDK或者Cglib来生成，JdkDynamicAopProxy类和Cglib2AopProxy都实现的是AopProxy的接口，我们进入JdkDynamicAopProxy实现中看看Proxy是怎样生成的：

![]()

用Proxy包装target之后，通过ProxyFactoryBean得到对其方法的调用就被Proxy拦截了， ProxyFactoryBean的getObject()方法得到的实际上是一个Proxy了，target对象已经被封装了。对 ProxyFactoryBean这个工厂bean而言，其生产出来的对象是封装了目标对象的代理对象。

 

**3.5 拦截器的作用
**

前面分析了SpringAOP实现中得到Proxy对象的过程，接下来我们去探寻Spring AOP中拦截器链是怎样被调用的，也就是Proxy模式是怎样起作用的。
还记得在JdkDynamicAopProxy中生成Proxy对象的时候，有一句这样的代码吗？

**return** Proxy.*newProxyInstance*(classLoader, proxiedInterfaces, **this**);****

这里我们的JdkDynamicAopProxy实现了InvocationHandler这个接口，this参数对应的是InvocationHandler对象,也就是说当 Proxy对象的函数被调用的时候，InvocationHandler的invoke方法会被作为回调函数调用：

![]()

![]()

上面所说的目标对象方法的调用，是通过AopUtils的方法调用，使用反射机制来对目标对象的方法进行的：

![]()

接下来，我们来看具体的ReflectiveMethodInvocation中proceed()方法的实现，也就是拦截器链的实现机制：

![]()

从上面的分析我们看到了Spring AOP拦截机制的基本实现，比如Spring怎样得到Proxy，怎样利用JAVA Proxy以及反射机制对用户定义的拦截器链进行处理。

 

**3.6 织入的实现
**

在上面调用拦截器的时候，经过一系列的注册，适配的过程以后，拦截器在拦截的时候，会调用到预置好的一个通知适配器，设置通知拦截器，这是一系列Spring设计好为通知服务的类的一个，是最终完成通知拦截和实现的地方，例如对 MethodBeforeAdviceInterceptor的实现是这样的：

![]()

可以看到通知适配器将advice适配成Interceptor以后，会调用advice的before方法去执行横切逻辑。这样就成功的完成了before通知的织入。

[AOP](http://www.goldendoc.org/tag/aop/ "AOP (1主题)")
## [Spring IoC之ApplicationContext](http://www.goldendoc.org/2010/11/spring_ioc_applicationcontext-2/ "永久链接：Spring IoC之ApplicationContext")

[0](http://www.goldendoc.org/2010/11/spring_ioc_applicationcontext-2/#comments)

2年前

由[iwlh](http://www.goldendoc.org/author/iwlh/ "iwlh发表的文章 ")发布 在[Spring](http://www.goldendoc.org/category/spring/ "Spring (4主题)")
本章主要讲ApplicationContext接口对BeanFactory接口的扩展内容。BeanFactory接口主要围绕着bean和bean相关配置方式，没有关注应用环境的相关配置。ApplicationContext接口从BeanFactory接口派生而来，它与BeanFactory的对比如下图所示：
  
BeanFactory 
 
ApplicationContext  Bean配置/实例化
 
Yes 
 
Yes  自动装配BeanPostProcessor
 
No 
 
Yes  自动装配BeanFactoryPostProcessor
 
No 
 
Yes  国际化信息（MessageSources）支持
 
No 
 
Yes  容器内部事件（ApplicationEvent）支持
 
No 
 
Yes  多配置模块加载
 
No 
 
Yes 

 

**1、统一资源加载
**

Spring中提供了org.springframework.core.io.Resource接口作为所有资源的抽象。Spring中默认提供了一些Resource接口的实现类，如图所示：

![]()

实现类命名上就可以看出对应的资源，比如ClassPathResource类是从java应用程序的ClassPath中加载相关资源等等。

Spring中使用ResourceLoader来查找和定位Resource资源。ResorceLoader接口类图如下所示：

![]()

从上图可以看出，ApplicationContext接口继承自ResourceLoader接口。AbstractApplicationContext抽象类也继承自DefaultResourceLoader类，而且还拥有一个PathMatchingResourcePatternResolver属性字段，需要加载多个Resource时候，委派给PathMatchingResourcePatternResolver类加载即可。

 

回过来，让我们再看下DefaultResourceLoader中getResource方法的实现代码。

![]()

可以看到getResource方法尝试了classPath、url方式加载资源。需要注意的是，该类不能加载相对路径或绝对路径下的资源（例如文件），如果需要加载绝对路径的资源，可以使用FileSystemResourceLoader对象。

 

**2、国际化信息支持
**

Java SE中已经有了国际化支持，也就是java.util.Locale和java.util.ResourceBundle。Spring在JavaSE的国际化支持上，进一步抽象了国际化信息的访问接口，提供了org.springframework.context.MessageSource接口，该接口提供一下方法：

![]()

ApplicationContext也实现了MessageSource接口。当ApplicationContext初始化时，它会自动在容器中查找名称为”messageSource”的bean。如果找到，对上述方法的调用将被委托给该bean。否则ApplicationContext会在其父类中查找是否含有同名的bean。如果有，就把它作为MessageSource。如果它最终没有找到任何的消息源，一个空的StaticMessageSource将会被实例化，使它能够接受上述方法的调用。messageSource bean的配置实例如下：

![]()

Spring提供了三种MessageSource的实现。即StaticMessageSource（提供简单实现，可通过编程方式添加信息条目，多用于测试，不应该用于正式的生产环境）、ResourceBundleMessageSource（基于标准的java.util.ResourceBundle而实现的MessageSource，对父类AbstractMessageSource进行扩展，提供对多个ResourceBundle的缓存以提高查询速度。是最常用的，可用于生产环境下的MessageSource）、ReloadableResourceBundleMessageSource（同样是基于标准的java.util.ResourceBundle而实现的MessageSource，通过cacheSeconds属性可以定期刷新并检查properties资源文件是否发生变化，并且通过ResourceLoader加载properties资源文件。）

另外，MessageSourceAware接口还能用于获取任何已定义的MessageSource引用。任何实现了MessageSourceAware接口的bean将在Spring容器初始化时候与MessageSource一同被注入。

MessageSource与ApplicationContext的类结构图如下所示：

![]()

 

**3、Spring容器内部事件发布
**

ApplicationContext容器提供了容器内部事件发布功能，是继承自JavaSE标准自定义事件类而实现的。

JavaSE标准自定义事件结构不在此详细描述，一张图很直观的描述清楚：

![]()

EventObject，为JavaSE提供的事件类型基类，任何自定义的事件都继承自该类，例如上图中右侧灰色的各个事件。Spring中提供了该接口的子类ApplicationEvent。

EventListener，为JavaSE提供的事件监听者接口，任何自定义的事件监听者都实现了该接口，如上图左侧的各个事件监听者。Spring中提供了该接口的子类ApplicationListener接口。

JavaSE中未提供事件发布者这一角色类， 由各个应用程序自行实现事件发布者这一角色。Spring中提供了ApplicationEventPublisher接口作为事件发布者，并且ApplicationContext实现了这个接口，担当起了事件发布者这一角色。但ApplicationContext在具体实现上有所差异，Spring提供了ApplicationEventMulticaster接口，负责管理ApplicationListener和发布ApplicationEvent。ApplicationContext会把相应的事件相关工作委派给ApplicationEventMulticaster接口实现类来做。类图如下所示：

![]()

附上一张事件发布的时序图：

[![]()](http://pic.yupoo.com/goldendoc/AH0MxsWp/jBIbm.png)

 

**4、多配置模块加载
**

现在的应用程序，一般都会把配置信息按照某些规则进行分割，将不同类型或关注点的配置项放置在不同的文件中。相对BeanFactory来说，ApplicationContext已经支持了加载多个配置文件。

ApplicationContext加载多个配置文件的方法有：

1) 数组方式

String[] locations = new String[]{ “conf/bean1.xml”,”conf/bean2.xml”, “conf/bean3.xml”};

ApplicationContext container = new FileSystemXmlApplicationContext(locations);

2) 通配符

ApplicationContext container = new FileSystemXmlApplicationContext(“conf/**/*.xml”);

3) ClassPathXmlApplicationContext特性

ApplicationContext ctx = new ClassPathXmlApplicationContext(new String[] {“services.xml”, “daos.xml”}, MessengerService.class);

该方法可以通过MessengerService类在ClassPath中的位置定位配置文件，而不用指定每个配置文件的完整路径名。

[ApplicationContext](http://www.goldendoc.org/tag/applicationcontext/ "ApplicationContext (1主题)")

## [Spring IoC之BeanFactory](http://www.goldendoc.org/2010/11/spring-ioc%e4%b9%8bbeanfactory/ "永久链接：Spring IoC之BeanFactory")

[0](http://www.goldendoc.org/2010/11/spring-ioc%e4%b9%8bbeanfactory/#comments)

2年前

由[khotyn](http://www.goldendoc.org/author/khotyn/ "khotyn发表的文章 ")发布 在[Spring](http://www.goldendoc.org/category/spring/ "Spring (4主题)")
本文的内容为对Spring IoC容器实现的分析。

本文一共分为5个部分：

* 第一部分简要讲述了IoC的概念
* 第二部分对Spring IoC容器中的主要类及其职责做一些了解
* 第三部分分析了Spring IoC容器的初始化过程
* 第四部分分析了从Spring IoC容器中获取Bean的过程
* 第五部分简要讲述了Spring IoC容器对Bean生命周期的管理。

本文假设读者对以下的概念有所了解：IoC（控制反转），DI（依赖注入），Bean，并且读者有使用Spring IoC容器的经验。

约定：本文中所指的IoC容器没有特别说明均为Spring IoC容器

### [一、什么是]()[IoC]()[]()

[]()

[IoC是Inversion of Control的缩写，中文的意思是控制反转，在IoC中，组件不需要去寻找它所依赖的对象，而是由IoC容器来负责将组件所依赖的对象通过Java Bean的Setter方法或者是构造函数等方式注入给组件。IoC的另一个名字是DI，即依赖注入，关于IoC和DI之间的关系以及关于IoC的更多内容，大家可以参考Wiki上的]()[控制反转条目。
](http://www.google.com/url?q=http%3A%2F%2Fzh.wikipedia.org%2Fzh%2F%E6%8E%A7%E5%88%B6%E5%8F%8D%E8%BD%AC&sa=D&sntz=1&usg=AFQjCNGHDFZSBvJ9wlCDET_IgqV-4DVCzw)

[
](http://www.google.com/url?q=http%3A%2F%2Fzh.wikipedia.org%2Fzh%2F%E6%8E%A7%E5%88%B6%E5%8F%8D%E8%BD%AC&sa=D&sntz=1&usg=AFQjCNGHDFZSBvJ9wlCDET_IgqV-4DVCzw)

### [](http://www.google.com/url?q=http%3A%2F%2Fzh.wikipedia.org%2Fzh%2F%E6%8E%A7%E5%88%B6%E5%8F%8D%E8%BD%AC&sa=D&sntz=1&usg=AFQjCNGHDFZSBvJ9wlCDET_IgqV-4DVCzw)[二、IoC容器中的类主要类及其职责
]()

[
我们先来看下IoC容器的一个大概的类图：
]()

[]()[![]()](http://www.google.com/url?q=http%3A%2F%2Fpic.yupoo.com%2Fkhotyn%2F816909ab9c75%2Fji8tx5ue.jpg&sa=D&sntz=1&usg=AFQjCNEixIuYlet6oZmgnSPQy-ZxrCs4yA)

[点击查看大图](http://www.google.com/url?q=http%3A%2F%2Fpic.yupoo.com%2Fkhotyn%2F816909ab9c75%2Fji8tx5ue.jpg&sa=D&sntz=1&usg=AFQjCNEixIuYlet6oZmgnSPQy-ZxrCs4yA)

这张图中比较简单的展示了IoC容器中的各个类及其指责，我们需要重点把握几个接口的职责：

* BeanFactory：这个接口是整个IoC容器最底层的接口，定义了一组访问Bean容器的基本方法。一些其他的接口，比如ListableBeanFactory和ConfigurableBeanFactory，都是继承了BeanFactory，并添加了其他的方法来完成某些特别的功能（比如ConfigurableBeanFactory，顾名思义，这个接口的职责是让BeanFactory变得可配置，那么它就定义了一组可以配置BeanFactory的方法）。
* AbstractBeanFactory：从名字可以看出，这个类是BeanFactory接口的一个抽象实现类，这个类本身实现的是ConfigurableBeanFactory，对ConfigurableBeanFactory以及BeanFactory中的方法提供了实现，并且提供了一些诸如单例缓存，别名等等功能。
* SingletonBeanRegistry：定义了一组操作单例Bean的方法

### [三、]()[IoC容器的初始化
]()[]()

[
使用过Spring的人都知道，我们都是在一份Bean配置文件中定义Bean，然后就可以通过BeanFactory的getBean()方法来获取Bean，那么我们就可以大致猜想到IoC容器的初始化工作大概就是将我们编写的Bean配置文件转换成IoC容器内部定义的用于放置Bean定义信息的数据结构，而这个数据结构就是BeanDefinition这个类。下面我们就来了解下这个转换过程是如何进行的。

我们通过实例化ClassPathXmlApplicationContext这个类来一步步来看其初始化的过程，简单地实例化ClassPathXmlApplicationContext的代码如下：

![]()

这里我们传入一个beans.xml作为配置文件的路径去实例化一个ClassPathXmlApplicationContext。

首先我们还是来看下整个初始化过程的序列图：
]()

[]()[![]()](http://www.google.com/url?q=http%3A%2F%2Fpic.yupoo.com%2Fkhotyn%2F005389ab9c75%2Fogfhmraj.jpg&sa=D&sntz=1&usg=AFQjCNHa07fld9fiZq1BzNzeljU3nfHFXQ)

[点击查看大图](http://www.google.com/url?q=http%3A%2F%2Fpic.yupoo.com%2Fkhotyn%2F005389ab9c75%2Fogfhmraj.jpg&sa=D&sntz=1&usg=AFQjCNHa07fld9fiZq1BzNzeljU3nfHFXQ)

这个图中涉及到的类或许有点吓人，且慢，下面我会慢慢带你了解整个过程。从序列图里面我们看到初始化过程首先调用了refresh()方法，后面调用到了AbstractXmlApplicationContext的loadBeanDefinitions方法，来看下这个方法的实现：

![]()

这个方法将beanFactory（实现了BeanDefinitionRegistry接口，后续将通过这个接口将Bean定义信息注册到BeanFactory中去）传入new了一个XmlBeanDefinitionReader对象，然后将刚刚new出来的beanDefinitionReader传入调用loadBeanDefinitions方法，最终调用了XmlBeanDefinitionReader的loadBeanDefinitions(EncodedResource encodedResource)方法：

![]()

这个方法获取了Bean配置文件的输入流，并且调用了doLoadBeanDefinitions方法，在这个方法里面，程序将输入流转换成Document对象，然后调用了下面这个方法：

![]()

需要注意这个createReaderContext(resource)方法，创建这个方法的时候XmlBeanDefinitionReader将自己传入，以便在后面可以获取到它的Registry对象。最终DefaultBeanDefinitionDocumentReader将解析BeanDefinition的工作又交给了BeanDefinitionParserDelegate对象：

![]()

从上面的方法中，我们可以看到BeanDefinitionParserDelegate对象解析出BeanDefinition后，就由BeanDefinitionRegistry来将BeanDefinition注册到BeanFactory中去了。

至此，整个BeanFactory就初始化完毕了，可能一大堆方法调来调去地早就把大家给调晕了，我们就来总结下初始化过程中设计到的几个主要的类以及它们的职责吧：

* XmlBeanDefinitionReader：读取定义Bean的XML文件并且将XML文件转成Document对象，交给BeanDefinitionDocuementReader再做解析。它持有一个BeanDefinitionRegistry对象，用于将BeanDefinition注册到BeanFactory中去。
* XmlBeanDefinitionDocumentReader：取出Docuement对象内的各个元素并将这些元素交给BeanDefinitionParserDelegate来解析。
* BeanDefinitionParserDelegate：用于解析Bean定义信息的代理类，负责一个Bean定义信息（可以看作是一个<bean></bean>标签对）解析成一个BeanDefinition对象。
* BeanDefinitionRegister：负责将BeanDefinition注册到BeanFactory中去。

### [四、从]()[IoC容器中获取Bean]()[]()

[
用过Spring的人大概都知道，在Spring中，我们是通过调用BeanFactory的getBean()方法来取得我们所需要的Bean的，而getBean()方法的主要逻辑在AbstactBeanFactory的doGetBean()方法中。在Spring中，有单例Bean和原型Bean的区分，在从容器中获取Bean的时候，单例Bean和原型Bean有些不同，当第一次获取单例Bean的时候，整个过程和获取原型Bean几乎是一样的，都需要创建一个Bean，但是当第二次，第三次，……，获取同样的单例Bean的时候，容器就直接从单例缓存中获取Bean了，而不会再去像获取原型Bean一样一而再再而三地创建Bean了。这样我们这一节也主要从两个方面来讲，一个是获取原型Bean，第一次获取单例Bean的逻辑和这个类似，有特别的地方也会在这里顺带提到，二则是将从单例缓存中获取单例Bean的过程，首先我们来看获取原型Bean：
]()

### []()[4.1、获取原型Bean
]()

[
正如前面所说，我们来看下AbstractBeanFactory的doGetBean()方法来了解获取原型Bean的整个过程。

在获取Bean的时候，无论这个Bean是单例的还是原型的，Spring都会尝试从单例缓存中获取Bean，但是当拿原型Bean的时候，这里显然是拿不到的，接下来程序就会根据BeanDefinition信息来判断要创建的Bean是不是原型Bean，如果是，则进入下面这段逻辑：

![]()

程序在上图的（1）中的位置调用了beforePrototypeCreation方法，告诉容器当前的这个Bean正在创建中，来防止发生重复创建的情况。接下来，程序在（2）处调用了createBean方法来创建这个prototypeBean，最后，在（3）处，程序调用afterProtytypeCreation来告诉容器，这个Bean现在已经不再创建过程中了。

那么，让我们来关注下createBean这个方法里面干了些什么事情：

![]()

在做了一堆准备工作后，程序就到了上面的这一段中，从代码中我们可以看出这个方法主要的功能为以下两点：

* 调用resolveBeforeInstantiation方法，让BeanPostProcessor可以有机会给你返回一个代理类而不是原来的类，当后面我们看到Spring AOP代理类的生成的时候，就会看到这个方法的用处了。
* 调用doCreateBean()方法创建Bean

我们再来看下doCreateBean方法：

![]()

同样，这个方法里面也有两个主要的功能：

* 一是调用createBeanInstance方法，创建一个Bean实例，在这个方法的内部，会调用BeanUtils的instantiateClass来实例化Bean，并把它包装成一个BeanWrapper
* 二是调用populateBean方法来将Bean依赖的属性设置进去。
]()

### []()[4.2、创建单例Bean
]()

[
整个获取原型Bean的过程大概就是这样样子，因为创建单例Bean和这个过程基本上是一样的，但是也有一些稍微不一样的地方，这里也稍微提到一下：

![]()

创建单例Bean是通过调用getSingleton来实现的，这个方法传入一个beanName和一个ObjectFactory，这个ObjectFactory的getObject方法里面调用到了我们前面提到的createBean方法，所以我们看下getSingleton这个方法的实现：

![]()

这个方法先尝试从singletonObjects中获取单例Bean，如果获取不到，则自己创建，同样，和创建原型Bean一样，在创建开始之前会调用beforeSingletonCreation方法来将beanName放到singletonsCurrentlyInCreation来告诉容器这个Bean已经在创建中了，在创建完成之后，会将BeanName从singletonsCurrentlyInCreation中删除掉。创建的过程是调用了传入的ObjectFactory的getObject方法，和创建原型Bean类似。在创建完成之后，还有一步addSingleton的操作，来讲单例放到单例缓存中去，看一下这个的实现：

![]()

方法的逻辑非常简单：把创建出来的单例Bean放到singletonObjects中去，然后从singletonFactories和earlySingletonObjects中删除掉，最后在registeredSingletons里面再加入这个Bean，对于这里面用到的几个容器，我觉得有必要在这里描述一下其作用，要不然读者肯定是晕呼晕呼的：

* singletonObjects：用于保存BeanName和Bean实例之间的关系
* singletonFactories：用于保存BeanName和创建Bean的工厂之间的关系
* earlySingletonObjects：也是保存BeanName和Bean实例之间的关系，与singletonObjects的不同之处在于，当一个单例Bean被放到这里面去后，那么当Bean还在创建过程中，就可以通过getBean来拿到了，其目的是用来检测循环引用。
* registeredSingletons：用来保存当前所有已注册的Bean
]()

### []()[4.3、从单例缓存中获取单例Bean
]()

[
单例在Spring的同一个容器内只会被创建一次，后续再获取Bean，就直接从单例缓存中获取了，我们来看下这一段过程，看下doGetBean里面调用的getSingleton方法：

![]()

这个方法的逻辑也相对简单，先尝试从singletonObjects里面获取，如果获取不到再从earlySingletonObjects里面获取，如果再获取不到，再尝试从singletonFactories里面获取beanName对应的ObjectFactory，然后调用这个ObjectFactory的getObject来创建Bean，并放到earlySingletonObjects里面去，并且从singletonFacotories里面remove掉这个ObjectFactory。

### 六、IoC容器对Bean生命周期的管理

Spring有一套Bean生命周期去管理Bean，值得注意的是，Spring只对非单例的Bean进行生命周期管理。关于Spring中Bean的生命周期，我们来看下一张老图：

![]()

在上面这张图里面，我们看到有很多的生命周期方法，那么这些生命周期方法是在哪里调用的呢？在前面获取原型Bean的一节中，我们已经知道，Spring会先调用createBeanInstance方法来创建Bean实例，然后通过populateBean方法来设置Bean的属性，在调用这个方法之后，其实Spring还调用了一个initializeBean的方法，上图中我们看到的生命周期方法基本上都在这个方法里面调用：

![]()

在这个方法里面：

* Spring首先调用了invokeAwareMethods来调用各个Aware方法，包括BeanNameAware，BeanClassLoaderAware和BeanFactoryAware
* 然后调用了所有BeanPostProcessor的postProcessBeforeInitialization方法
* 接着调用invokeInitMethods方法，里面包括调用afterPropertiesSet和自定义的init方法
* 最后调用了所有BeanPostProcessor的postProcessAfterInitialization方法

在调用这些方法以后，我们的Bean才算是可以使用啦。至于生命周期的最后两个方法，是在容器销毁的时候来调用的。
]()[]()

[]()

[]()[BeanFactory](http://www.goldendoc.org/tag/beanfactory/ "BeanFactory (1主题)")

*
* ### 分类

* [Java NIO **(9)** Java NIO相关内容](http://www.goldendoc.org/category/java-nio/ "查看 Java NIO下的所有文章")
* [JMS **(3)** Java Messaging Service](http://www.goldendoc.org/category/jms/ "查看 JMS下的所有文章")
* [JUC **(4)**](http://www.goldendoc.org/category/juc/ "查看 JUC下的所有文章")
* [JVM **(8)** Java Virtual Machine](http://www.goldendoc.org/category/jvm/ "查看 JVM下的所有文章")
* [Spring **(4)**](http://www.goldendoc.org/category/spring/ "查看 Spring下的所有文章")
* [Tomcat **(7)**](http://www.goldendoc.org/category/tomcat/ "查看 Tomcat下的所有文章")
* [翻译 **(1)**](http://www.goldendoc.org/category/translation/ "查看 翻译下的所有文章")
* ### 最新评论

* [![熊猫家族]()  熊猫家族 很详细，了解了很多 5个月前](http://www.goldendoc.org/2012/01/optimization-barrier3/#comment-306 "在 优化屏障（Optimization barrier）第三讲")
* [![聊聊并发（四）深入分析ConcurrentHashMap并发编程网 | 并发编程网]()  聊聊并发（四）深入分析ConcurrentHashMap并发编程网 | 并发编程网 [...] Java并发编程之ConcurrentHashMap [...] 6个月前](http://www.goldendoc.org/2011/06/juc_concurrenthashmap/#comment-304 "在 Java并发编程之ConcurrentHashMap")
* [![Jun]()  Jun for (HashEntry p = first; p != e; p = p.next) newFirst = new HashEntry(p.key, p.hash, […] 6个月前](http://www.goldendoc.org/2011/06/juc_concurrenthashmap/#comment-301 "在 Java并发编程之ConcurrentHashMap")
* [![khotyn]()  khotyn 这里是我搞错了，已经纠正过来了，谢谢赵姐夫指正。 7个月前](http://www.goldendoc.org/2011/06/juc_concurrenthashmap/#comment-294 "在 Java并发编程之ConcurrentHashMap")
* [![老赵]()  老赵 这里Segment的数量是不大于concurrentLevel的最大的2的指数 应该是“不小于concurrentLevel”的最小的2的指数吧。 7个月前](http://www.goldendoc.org/2011/06/juc_concurrenthashmap/#comment-293 "在 Java并发编程之ConcurrentHashMap")
[显示更多]( "显示下5个条目")
* ### 最新文章

* [从问题出发看双亲委派 (0)  同事A问了这样一个问题： *BootstrapClassLoader为什么能够加载用户自定义的类？* 当时我想到的是这样的场景： javaagent设置Boot-Class-Path，或者添加启动参数-Xbootclasspath、-Xbootclasspath/a […] 5个月前](http://www.goldendoc.org/2013/01/parentdelegation/ "从问题出发看双亲委派")
* [我学到的一些关于编程的事儿（翻译） (3) 原文地址：Some things I've learnt about programming ---- By John Graham-Cumming 我已经从事编程 30 年了，用过的机器包括从现在看来很差的（基于 Z80 和 6502）到最新的，用过的语言包括 […] 11个月前](http://www.goldendoc.org/2012/07/somethings_i_ve_learnt_about_programming/ "我学到的一些关于编程的事儿（翻译）")
* [ClassLoader与JVM (0) **一、ClassLoader与HotSpot** 对JVM有所了解的人应该知道，类文件的格式是不能违背JVM规范的，而JVM自然会有解析类文件的工具ClassFileParser。 ClassFileParser由ClassFileStream*构造，其实 […] 1年前](http://www.goldendoc.org/2012/02/classloader-jvm/ "ClassLoader与JVM")
* [优化屏障（Optimization barrier）第三讲 (1) 上一篇 优化屏障（Optimization barrier）第二讲 1. […] 1年前](http://www.goldendoc.org/2012/01/optimization-barrier3/ "优化屏障（Optimization barrier）第三讲")
* [优化屏障（Optimization barrier）第二讲 (1) 上一篇 优化屏障（Optimization barrier）第一讲 1. gcc编译的大致过程 可以看到，gcc优化主要分两大部分:Tree优化和RTL(Register Transfer Language)优化； 前文所说的指令调度（Instruction […] 1年前](http://www.goldendoc.org/2012/01/optimization-barrier2/ "优化屏障（Optimization barrier）第二讲")
[显示更多]( "显示下5个条目")
* ### 友情链接

* [并发编程促进并发编程的研究和传播](http://ifeve.com/)
自豪地使用[WordPress](http://wordpress.org/)，Mystique主题来自[digitalnature](http://digitalnature.eu/) | [RSS订阅](http://www.goldendoc.org/feed/)  [回到顶部]()

[ ]()

[ ]()
