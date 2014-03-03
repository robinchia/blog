---
layout: post
title: "深入理解Java内存模型（七）——总结"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_多线程
 - 内存
--- 

# 深入理解Java内存模型（七）——总结

### 分享到

* [一键分享]()
* [QQ空间]()
* [新浪微博]()
* [百度搜藏]()
* [人人网]()
* [腾讯微博]()
* [百度相册]()
* [开心网]()
* [腾讯朋友]()
* [百度贴吧]()
* [豆瓣网]()
* [搜狐微博]()
* [百度新首页]()
* [QQ好友]()
* [和讯微博]()
* [更多...]()

[百度分享]()

* [关于我们](http://www.infoq.com/cn/aboutus "关于我们")
* [让大家在InfoQ上听见你的声音](http://www.infoq.com/cn/contribute "让大家在InfoQ上听见你的声音")

* 欢迎关注我们的：
* [![]()](http://e.weibo.com/infoqchina)
* [![]()](http://www.infoq.com/cn/news/2013/02/infoq-wechat)
* [![]()](http://www.infoq.com/cn/rss/rss.action?token=AjlwmaWcwi5ejkcT6GcRk1pCqJ4K7ocs)

促进软件开发领域知识与创新的传播

[登录]( "Login")
[![]()](http://www.infoq.com/cn/)

* [En](http://www.infoq.com/) |
* [中文]( "InfoQ China") |
* [日本](http://www.infoq.com/jp/) |
* [Fr](http://www.infoq.com/fr/) |
* [Br](http://www.infoq.com/br/)

164,153 六月 独立访问用户

* [语言 & 开发](http://www.infoq.com/cn/development/ "Development")

* [Java](http://www.infoq.com/cn/java/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "Java")
* [.Net](http://www.infoq.com/cn/dotnet/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk ".Net")
* [云计算](http://www.infoq.com/cn/cloud-computing/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "云计算")
* [移动](http://www.infoq.com/cn/mobile/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "移动")
* [HTML 5](http://www.infoq.com/cn/HTML5Topic/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "HTML 5")
* [JavaScript](http://www.infoq.com/cn/javascript/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "JavaScript")
* [Ruby](http://www.infoq.com/cn/ruby/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "Ruby")
* [DSLs](http://www.infoq.com/cn/dsl/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "DSLs")
* [Python](http://www.infoq.com/cn/python/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "Python")
* [PHP](http://www.infoq.com/cn/Topic_PHP/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "PHP")
* [PaaS](http://www.infoq.com/cn/PaaS/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "PaaS")

## 特别专题语言 & 开发

### [Juergen Fesslmeier谈端到端的JavaScript开发](http://www.infoq.com/cn/interviews/end-to-end-javascript)

![]() Juergen谈论了使用JavaScript进行端对端开发的好处和开发团队可能遇见的挑战。他还谈到了Wakanda Studio以及如何仅用JavaScript通过它来开发复杂的应用程序。
[浏览所有**语言 & 开发**](http://www.infoq.com/cn/development/)
* [架构 & 设计](http://www.infoq.com/cn/architecture-design/ "Architecture & Design")

* [建模](http://www.infoq.com/cn/Modeling/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "建模")
* [性能和可伸缩性](http://www.infoq.com/cn/performance-scalability/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "性能和可伸缩性")
* [领域驱动设计](http://www.infoq.com/cn/domain-driven-design/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "领域驱动设计")
* [AOP](http://www.infoq.com/cn/AOP/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "AOP")
* [设计模式](http://www.infoq.com/cn/DesignPattern/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "设计模式")
* [安全](http://www.infoq.com/cn/Security/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "安全")
* [云计算](http://www.infoq.com/cn/cloud-computing/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "云计算")
* [SOA](http://www.infoq.com/cn/soa_platforms/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "SOA")

## 特别专题架构 & 设计

### [设计模式自动化](http://www.infoq.com/cn/articles/Design-Pattern-Automation)

![]() 尽管维护每行代码的成本如此高昂，但我们仍然每天都在编写着大量的样板代码。如果我们有更智能的编译器，那其中很大一部分是可以避免的。实际上，多数模板代码只是重复地实现那些我们已理解透彻的设计模式，只要我们教会编译器一些技巧，有一些设计模式完全是可以自动实现的。
[浏览所有**架构 & 设计**](http://www.infoq.com/cn/architecture-design/)
* [过程 & 实践](http://www.infoq.com/cn/process-practices/ "Process & Practices")

* [Agile](http://www.infoq.com/cn/agile/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "Agile")
* [领导能力](http://www.infoq.com/cn/Leadership/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "领导能力")
* [团队协作](http://www.infoq.com/cn/team-collaboration/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "团队协作")
* [敏捷技术](http://www.infoq.com/cn/agile_techniques/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "敏捷技术")
* [方法论](http://www.infoq.com/cn/methodologies/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "方法论")
* [持续集成](http://www.infoq.com/cn/continuous_integration/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "持续集成")
* [精益](http://www.infoq.com/cn/lean/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "精益")
* [客户及需求](http://www.infoq.com/cn/cust_requirements/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "客户及需求")

## 特别专题过程 & 实践

### [成功的根本—集成的ALM工具](http://www.infoq.com/cn/articles/Integrated-ALM)

![]() 典型的软件交付项目会无数次地去获取需求，并在多个地方描述测试，但它们却与某一特定的构建里的具体内容并不相符合，因此项目往往需要大量分析来获知谁在做什么以及为什么做。Dave West深入研究造成该问题的原因，并致力研究一个整体的、集成的ALM方法。
[浏览所有**过程 & 实践**](http://www.infoq.com/cn/process-practices/)
* [运维 & 基础架构](http://www.infoq.com/cn/operations-infrastructure/ "Operations & Infrastructure")

* [性能和可伸缩性](http://www.infoq.com/cn/performance-scalability/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "性能和可伸缩性")
* [大数据](http://www.infoq.com/cn/bigdata/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "大数据")
* [DevOps](http://www.infoq.com/cn/devops/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "DevOps")
* [云计算](http://www.infoq.com/cn/cloud-computing/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "云计算")
* [虚拟化](http://www.infoq.com/cn/virtualization/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "虚拟化")
* [NoSQL](http://www.infoq.com/cn/NoSQL/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "NoSQL")
* [应用服务器](http://www.infoq.com/cn/ApplicationServers/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "应用服务器")
* [运维](http://www.infoq.com/cn/operations/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "运维")

## 特别专题运维 & 基础架构

### [书评：验收测试驱动开发实践指南](http://www.infoq.com/cn/articles/atdd-by-example-book)

![]() 《验收测试驱动开发实践指南》一书的目的是作为一个介绍性使用指南指导那些从零开始的团队成功执行和应用验收测试驱动开发（ATDD）。尽管该书在指出及总结了成功敏捷测试人员应该掌握的多个测试相关实践上做了有效的工作，但该书最终并没有为它的各层读者提供他们所需要的信息。By Manuel Pais
[浏览所有**运维 & 基础架构**](http://www.infoq.com/cn/operations-infrastructure/)
* [企业架构](http://www.infoq.com/cn/enterprise-architect/ "Enterprise Architecture")

* [企业架构](http://www.infoq.com/cn/enterprise-architecture/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "企业架构")
* [业务流程建模](http://www.infoq.com/cn/bpm/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "业务流程建模")
* [业务/IT整合](http://www.infoq.com/cn/business_it_alignment/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "业务/IT整合")
* [Integration (EAI)](http://www.infoq.com/cn/EAI/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "Integration (EAI)")
* [治理](http://www.infoq.com/cn/Governance/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "治理")
* [Web 2.0](http://www.infoq.com/cn/web20/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "Web 2.0")
* [SOA](http://www.infoq.com/cn/soa/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "SOA")

## 特别专题企业架构

### [设计指尖上的世界：移动用户界面一瞥](http://www.infoq.com/cn/articles/designing-a-world-at-your-fingertips-mobile-user-interfaces)

![]() 对任何成功的移动应用来说，用户界面（UI）是至关重要的组成部分。在这篇文章中，Forrest Skull展示了他与人机交互（HCI）研究者们进行的访谈与讨论，他们探讨了移动设备UI方面的原则，以及其它一些正在研究的领域，包括多设备、隐私、安全和语音。这篇文章还描述了开发移动设备用户界面过程中会面临的挑战。
[浏览所有**企业架构**](http://www.infoq.com/cn/enterprise-architect/)
[**QCon上海2013**
11月1-3日
上海光大会展中心](http://www.qconshanghai.com/ "New York 2013")

* [移动](http://www.infoq.com/cn/mobile/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "移动")
* [HTML 5](http://www.infoq.com/cn/html-5/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "HTML 5")
* [Node.js](http://www.infoq.com/cn/nodejs/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "Node.js")
* [云计算](http://www.infoq.com/cn/cloud-computing/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "云计算")
* [大数据](http://www.infoq.com/cn/bigdata/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "大数据")
* [运维](http://www.infoq.com/cn/operations/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "运维")
* [架构师](http://www.infoq.com/cn/architect/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "架构师")
* [百度云](http://www.infoq.com/cn/baidu_cloud/?utm_source=infoq&utm_medium=header_graybar&utm_campaign=topic_clk "百度云")
[全部话题](http://www.infoq.com/cn/topics)

[New UI](http://www.infoq.com/news/2013/04/infoq-redesign)
您目前处于： [InfoQ首页](http://www.infoq.com/cn "InfoQ首页") [文章](http://www.infoq.com/cn/articles "文章") 深入理解Java内存模型（七）——总结
# 深入理解Java内存模型（七）——总结

作者 [程晓明](http://www.infoq.com/cn/author/%E7%A8%8B%E6%99%93%E6%98%8E) 发布于 三月 15, 2013 *|* [4 评论]()

* [新浪微博]( "分享到新浪微博") [腾讯微博]( "分享到腾讯微博") [豆瓣网]( "分享到豆瓣网") [Twitter]( "分享到Twitter") [Facebook]( "分享到Facebook") [linkedin]( "分享到linkedin") [邮件分享]( "分享到邮件分享") 更多 [23]( "累计分享23次")
* [稍后阅读]()
* [我的阅读清单](http://www.infoq.com/cn/showbookmarks.action)
## 处理器内存模型

顺序一致性内存模型是一个理论参考模型，JMM和处理器内存模型在设计时通常会把顺序一致性内存模型作为参照。JMM和处理器内存模型在设计时会对顺序一致性模型做一些放松，因为如果完全按照顺序一致性模型来实现处理器和JMM，那么很多的处理器和编译器优化都要被禁止，这对执行性能将会有很大的影响。

根据对不同类型读/写操作组合的执行顺序的放松，可以把常见处理器的内存模型划分为下面几种类型：

1. 放松程序中写-读操作的顺序，由此产生了total store ordering内存模型（简称为TSO）。
1. 在前面1的基础上，继续放松程序中写-写操作的顺序，由此产生了partial store order 内存模型（简称为PSO）。
1. 在前面1和2的基础上，继续放松程序中读-写和读-读操作的顺序，由此产生了relaxed memory order内存模型（简称为RMO）和PowerPC内存模型。

注意，这里处理器对读/写操作的放松，是以两个操作之间不存在数据依赖性为前提的（因为处理器要遵守as-if-serial语义，处理器不会对存在数据依赖性的两个内存操作做重排序）。

下面的表格展示了常见处理器内存模型的细节特征：
内存模型名称
 
对应的处理器
相关厂商内容
### [JavaOne上海：迁移Spring应用到Java EE 6](http://www.infoq.com/infoq/url.action?i=3379&t=f)

### [JavaOne上海：淘宝的鹰眼分布式日志系统](http://www.infoq.com/infoq/url.action?i=3380&t=f)

### [豆瓣工程副总裁段念确认参与QCon上海2013，担任团队文化专题出品人](http://www.infoq.com/cn/vendorcontent/show.action?vcr=2272&utm_source=infoq&utm_medium=VCR&utm_campaign=vcr_articles_click)

### [白皮书下载：用HTML5 Builder构建单一代码库的Web/移动应用](http://www.infoq.com/cn/vendorcontent/show.action?vcr=2308&utm_source=infoq&utm_medium=VCR&utm_campaign=vcr_articles_click)

### [首届QCon上海20个专题确认，80余场分享，全面征集演讲主题](http://www.infoq.com/cn/vendorcontent/show.action?vcr=2311&utm_source=infoq&utm_medium=VCR&utm_campaign=vcr_articles_click)

相关赞助商
[![]()](http://www.infoq.com/infoq/url.action?i=3367&t=f)

JavaOne大会独家社区合作，[InfoQ用户享75折购票](http://www.infoq.com/infoq/url.action?i=3511&t=f)。 
Store-Load 重排序
 
Store-Store重排序
 
Load-Load 和Load-Store重排序
 
可以更早读取到其它处理器的写
 
可以更早读取到当前处理器的写 TSO
 
sparc-TSO

X64
 
Y
       
Y PSO
 
sparc-PSO
 
Y
 
Y
     
Y RMO
 
ia64
 
Y
 
Y
 
Y
   
Y PowerPC
 
PowerPC
 
Y
 
Y
 
Y
 
Y
 
Y

在这个表格中，我们可以看到所有处理器内存模型都允许写-读重排序，原因在第一章以说明过：它们都使用了写缓存区，写缓存区可能导致写-读操作重排序。同时，我们可以看到这些处理器内存模型都允许更早读到当前处理器的写，原因同样是因为写缓存区：由于写缓存区仅对当前处理器可见，这个特性导致当前处理器可以比其他处理器先看到临时保存在自己的写缓存区中的写。

上面表格中的各种处理器内存模型，从上到下，模型由强变弱。越是追求性能的处理器，内存模型设计的会越弱。因为这些处理器希望内存模型对它们的束缚越少越好，这样它们就可以做尽可能多的优化来提高性能。

由于常见的处理器内存模型比JMM要弱，java编译器在生成字节码时，会在执行指令序列的适当位置插入内存屏障来限制处理器的重排序。同时，由于各种处理器内存模型的强弱并不相同，为了在不同的处理器平台向程序员展示一个一致的内存模型，JMM在不同的处理器中需要插入的内存屏障的数量和种类也不相同。下图展示了JMM在不同处理器内存模型中需要插入的内存屏障的示意图：

![]()

如上图所示，JMM屏蔽了不同处理器内存模型的差异，它在不同的处理器平台之上为java程序员呈现了一个一致的内存模型。

## JMM，处理器内存模型与顺序一致性内存模型之间的关系

JMM是一个语言级的内存模型，处理器内存模型是硬件级的内存模型，顺序一致性内存模型是一个理论参考模型。下面是语言内存模型，处理器内存模型和顺序一致性内存模型的强弱对比示意图：

![]()

从上图我们可以看出：常见的4种处理器内存模型比常用的3中语言内存模型要弱，处理器内存模型和语言内存模型都比顺序一致性内存模型要弱。同处理器内存模型一样，越是追求执行性能的语言，内存模型设计的会越弱。

## JMM的设计

从JMM设计者的角度来说，在设计JMM时，需要考虑两个关键因素：

* 程序员对内存模型的使用。程序员希望内存模型易于理解，易于编程。程序员希望基于一个强内存模型来编写代码。
* 编译器和处理器对内存模型的实现。编译器和处理器希望内存模型对它们的束缚越少越好，这样它们就可以做尽可能多的优化来提高性能。编译器和处理器希望实现一个弱内存模型。

由于这两个因素互相矛盾，所以JSR-133专家组在设计JMM时的核心目标就是找到一个好的平衡点：一方面要为程序员提供足够强的内存可见性保证；另一方面，对编译器和处理器的限制要尽可能的放松。下面让我们看看JSR-133是如何实现这一目标的。

为了具体说明，请看前面提到过的计算圆面积的示例代码：
double pi = 3.14; //A double r = 1.0; //B double area = pi * r * r; //C

上面计算圆的面积的示例代码存在三个happens- before关系：

1. A happens- before B；
1. B happens- before C；
1. A happens- before C；

由于A happens- before B，happens- before的定义会要求：A操作执行的结果要对B可见，且A操作的执行顺序排在B操作之前。 但是从程序语义的角度来说，对A和B做重排序即不会改变程序的执行结果，也还能提高程序的执行性能（允许这种重排序减少了对编译器和处理器优化的束缚）。也就是说，上面这3个happens- before关系中，虽然2和3是必需要的，但1是不必要的。因此，JMM把happens- before要求禁止的重排序分为了下面两类：

* 会改变程序执行结果的重排序。
* 不会改变程序执行结果的重排序。

JMM对这两种不同性质的重排序，采取了不同的策略：

* 对于会改变程序执行结果的重排序，JMM要求编译器和处理器必须禁止这种重排序。
* 对于不会改变程序执行结果的重排序，JMM对编译器和处理器不作要求（JMM允许这种重排序）。

下面是JMM的设计示意图：

![]()

从上图可以看出两点：

* JMM向程序员提供的happens- before规则能满足程序员的需求。JMM的happens- before规则不但简单易懂，而且也向程序员提供了足够强的内存可见性保证（有些内存可见性保证其实并不一定真实存在，比如上面的A happens- before B）。
* JMM对编译器和处理器的束缚已经尽可能的少。从上面的分析我们可以看出，JMM其实是在遵循一个基本原则：只要不改变程序的执行结果（指的是单线程程序和正确同步的多线程程序），编译器和处理器怎么优化都行。比如，如果编译器经过细致的分析后，认定一个锁只会被单个线程访问，那么这个锁可以被消除。再比如，如果编译器经过细致的分析后，认定一个volatile变量仅仅只会被单个线程访问，那么编译器可以把这个volatile变量当作一个普通变量来对待。这些优化既不会改变程序的执行结果，又能提高程序的执行效率。

## JMM的内存可见性保证

Java程序的内存可见性保证按程序类型可以分为下列三类：

1. 单线程程序。单线程程序不会出现内存可见性问题。编译器，runtime和处理器会共同确保单线程程序的执行结果与该程序在顺序一致性模型中的执行结果相同。
1. 正确同步的多线程程序。正确同步的多线程程序的执行将具有顺序一致性（程序的执行结果与该程序在顺序一致性内存模型中的执行结果相同）。这是JMM关注的重点，JMM通过限制编译器和处理器的重排序来为程序员提供内存可见性保证。
1. 未同步/未正确同步的多线程程序。JMM为它们提供了最小安全性保障：线程执行时读取到的值，要么是之前某个线程写入的值，要么是默认值（0，null，false）。

下图展示了这三类程序在JMM中与在顺序一致性内存模型中的执行结果的异同：

![]()

只要多线程程序是正确同步的，JMM保证该程序在任意的处理器平台上的执行结果，与该程序在顺序一致性内存模型中的执行结果一致。

## JSR-133对旧内存模型的修补

JSR-133对JDK5之前的旧内存模型的修补主要有两个：

* 增强volatile的内存语义。旧内存模型允许volatile变量与普通变量重排序。JSR-133严格限制volatile变量与普通变量的重排序，使volatile的写-读和锁的释放-获取具有相同的内存语义。
* 增强final的内存语义。在旧内存模型中，多次读取同一个final变量的值可能会不相同。为此，JSR-133为final增加了两个重排序规则。现在，final具有了初始化安全性。

## 参考文献

1. [Computer Architecture: A Quantitative Approach, 4th Edition](http://www.amazon.com/Computer-Architecture-Fourth-Quantitative-Approach/dp/0123704901/ref=sr_1_10/102-0116773-7214567?ie=UTF8&s=books&qid=1188797467&sr=1-10)
1. [Shared memory consistency models: A tutorial](http://www.hpl.hp.com/techreports/Compaq-DEC/WRL-95-7.pdf)
1. [Intel® Itanium® Architecture Software Developer’s Manual Volume 2: System Architecture](http://www.intel.com/content/dam/www/public/us/en/documents/manuals/itanium-architecture-software-developer-rev-2-3-vol-2-manual.pdf)
1. [Concurrent Programming on Windows](http://www.amazon.com/Concurrent-Programming-Windows-Joe-Duffy/dp/032143482X/ref=sr_1_1?ie=UTF8&s=books&qid=1262571776&sr=1-1)
1. [JSR 133 (Java Memory Model) FAQ](http://www.cs.umd.edu/users/pugh/java/memoryModel/jsr-133-faq.html)
1. [The JSR-133 Cookbook for Compiler Writers](http://gee.cs.oswego.edu/dl/jmm/cookbook.html)
1. [Java theory and practice: Fixing the Java Memory Model, Part 2](http://www.ibm.com/developerworks/java/library/j-jtp03304/index.html)

## 关于作者

**程晓明**，Java软件工程师，国家认证的系统分析师、信息项目管理师。专注于并发编程，就职于富士通南大。个人邮箱：[asst2003@163.com](mailto:asst2003@163.com)。
* [Sections]()
* [**架构 & 设计**](http://www.infoq.com/cn/architecture-design)
* [**语言 & 开发**](http://www.infoq.com/cn/development)
* [Topics]()
* [多线程](http://www.infoq.com/cn/Multi-threading)
* [内存模型](http://www.infoq.com/cn/memory-model)
* [并发](http://www.infoq.com/cn/concurrency)
* [Java](http://www.infoq.com/cn/java)
* [专栏](http://www.infoq.com/cn/special-column)

相关内容
### [深入理解Java内存模型（六）——final](http://www.infoq.com/cn/articles/java-memory-model-6?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（五）——锁](http://www.infoq.com/cn/articles/java-memory-model-5?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（四）——volatile](http://www.infoq.com/cn/articles/java-memory-model-4?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（三）——顺序一致性](http://www.infoq.com/cn/articles/java-memory-model-3?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)

### [深入理解Java内存模型（二）——重排序](http://www.infoq.com/cn/articles/java-memory-model-2?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)
## 您好，陌生人！

您需要 [注册一个InfoQ账号](http://www.infoq.com/reginit.action) 或者 [登录]() 才能进行评论。在您完成注册后还需要进行一些设置。
## 获得来自InfoQ的更多体验。 []()

### 告诉我们您的想法

允许的HTML标签: a,b,br,blockquote,i,li,pre,u,ul,p
当有人回复此评论时请E-mail通知我

社区评论  [Watch Thread]()

[**看完后有两点疑问，请教一下：**  by Z CS Posted 21/03/2013 07:37]()
[**Re: 看完后有两点疑问，请教一下：**  by 程 晓明 Posted 23/03/2013 12:50]()

[**编译器什么时候插入内存屏障？**  by 黄 春 Posted 10/04/2013 12:49]()
[**Re: 编译器什么时候插入内存屏障？**  by 程 晓明 Posted 14/04/2013 11:01]()
[]()

**看完后有两点疑问，请教一下：**  21/03/2013 07:37 by Z CS

1. 文中所写的 “未同步/未正确同步的多线程程序。JMM为它们提供了最小安全性保障：线程执行时读取到的值，要么是之前某个线程写入的值，要么是默认值（0，null，false）。”，JSR-133 真的做到这样了么? 那 么 long和double的 读写 的非原子性是怎么回事啊？是不是矛盾啊？
2. 关于本文中最后“JSR-133为final增加了两个重排序规则。现在，final具有了初始化安全性。”，这句话，是不是也还要加上那个 前提“保证final引用不从构造函数内逸出”，final才能具有“初始化安全性”啊？

* [回复]()
* [回到顶部]()

[]()

**Re: 看完后有两点疑问，请教一下：**  23/03/2013 12:50 by 程 晓明

谢谢您的关注。
******************************************
问题1
最小安全性与64位数据的非原子性读/写并不矛盾。
它们是两个不同的概念，它们“发生”的时间点也不同。
最小安全性保证对象默认初始化之后（设置成员域为0，null或false），才会被任意线程使用。
最小安全性“发生”在对象被任意线程使用之前。
64位数据的非原子性读/写“发生”在对象被多个线程使用的过程中（读/写共享变量）。
当发生《本文三》末尾的那种问题时（处理器B看到仅仅被处理器A“写了一半“的无效值），
这里虽然处理器B读取到一个被写了一半的无效值，但这个值任然是处理器A写入的，只不过处理器A还没有写完而已。
最小安全性保证线程读取到的值，要么是之前某个线程写入的值，要么是默认值（0，null，false）。
但最小安全性并不保证线程读取到的共享变量的值，一定是某个线程写完后的值。
最小安全性保证线程读取到的值不会无中生有的冒出来，但并不保证线程读取到的值一定是正确的。
*************************************
问题2
是的，你的理解是对的。
这句话也要加上那个前提：“保证final引用不从构造函数内逸出”，final才能具有“初始化安全性”。

* [回复]()
* [回到顶部]()
[]()

**编译器什么时候插入内存屏障？**  10/04/2013 12:49 by 黄 春

厚积薄发之做。 明白了很多不解的地方。 这里有个问题想问下。 文中说“java编译器在生成字节码时，会在执行指令序列的适当位置插入内存屏障来限制处理器的重排序”。 这里有个问题请教。
插入内存屏障， 是在编译成字节码的时候完成， 还是在JIT执行的时候重新调整指令生成的？ 或者JIT只生产本地代码， 跟插入内存屏障之类的没关系？

* [回复]()
* [回到顶部]()

[]()

**Re: 编译器什么时候插入内存屏障？**  14/04/2013 11:01 by 程 晓明

Doug Lea在《The JSR-133 Cookbook for Compiler Writers》中，并没有明确指定插入内存屏障的时机。
个人估计，应该是取决于具体的JVM实现。

* [回复]()
* [回到顶部]()

[关闭]()

### **** by

发布于

* [查看]()
* [回复]()
* [回到顶部]()
[关闭]()  主题   您的回复 [引用原消息]()

允许的HTML标签: a,b,br,blockquote,i,li,pre,u,ul,p
当有人回复此评论时请E-mail通知我

[关闭]()  主题  您的回复

允许的HTML标签: a,b,br,blockquote,i,li,pre,u,ul,p
当有人回复此评论时请E-mail通知我
[关闭]()

* 热点内容
* [本周]()
* [本月]()
* [近6个月]()
### [计算机科学中最重要的32个算法](http://www.infoq.com/cn/news/2012/08/32-most-important-algorithms?utm_source=infoq&utm_medium=popular_links_homepage)

### [ThoughtWorks技术雷达（2013年5月）](http://www.infoq.com/cn/articles/thoughtWorks-technology-radar?utm_source=infoq&utm_medium=popular_links_homepage)

### [让1.5亿移动端用户第一时间获取消息](http://www.infoq.com/cn/articles/iqiyi-cloud-push-practices?utm_source=infoq&utm_medium=popular_links_homepage)

### [ThoughtWorks全球CEO郭晓谈软件人才的招聘与培养](http://www.infoq.com/cn/news/2013/06/tw-guoxiao-on-talents?utm_source=infoq&utm_medium=popular_links_homepage)

### [影响可扩展性的十宗罪](http://www.infoq.com/cn/news/2013/06/10-sins-for-scalability?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（6月刊）](http://www.infoq.com/cn/minibooks/architect-jun-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [使用功能开关更好地实现持续部署](http://www.infoq.com/cn/articles/function-switch-realize-better-continuous-implementations?utm_source=infoq&utm_medium=popular_links_homepage)

### [Eclipse迁移到GitHub](http://www.infoq.com/cn/news/2013/06/eclipse-github?utm_source=infoq&utm_medium=popular_links_homepage)

### [Newegg.com大数据实践](http://www.infoq.com/cn/presentations/newegg-big-data-practice?utm_source=infoq&utm_medium=popular_links_homepage)

### [软件系统架构：使用视点和视角与利益相关者合作](http://www.infoq.com/cn/minibooks/software-sys-architect?utm_source=infoq&utm_medium=popular_links_homepage)

### [计算机科学中最重要的32个算法](http://www.infoq.com/cn/news/2012/08/32-most-important-algorithms?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（6月刊）](http://www.infoq.com/cn/minibooks/architect-jun-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [REST的缺点是什么？](http://www.infoq.com/cn/news/2013/06/rest-drawbacks?utm_source=infoq&utm_medium=popular_links_homepage)

### [Google 发布新一代Web UI库Polymer](http://www.infoq.com/cn/news/2013/06/webcomponents?utm_source=infoq&utm_medium=popular_links_homepage)

### [大众点评网域名劫持事件概述](http://www.infoq.com/cn/news/2013/06/dianping-xinnet-hacked?utm_source=infoq&utm_medium=popular_links_homepage)

### [基于Node.js的自动化构建工具Grunt.js](http://www.infoq.com/cn/articles/GruntJs?utm_source=infoq&utm_medium=popular_links_homepage)

### [深入浅出Node.js（一）：什么是Node.js](http://www.infoq.com/cn/articles/what-is-nodejs?utm_source=infoq&utm_medium=popular_links_homepage)

### [代码之殇·第二版](http://www.infoq.com/cn/minibooks/i-m-wrights-hard-code?utm_source=infoq&utm_medium=popular_links_homepage)

### [软件系统架构：使用视点和视角与利益相关者合作](http://www.infoq.com/cn/minibooks/software-sys-architect?utm_source=infoq&utm_medium=popular_links_homepage)

### [大家谈18岁的Java——朱鸿：开过跑车后再去开大巴车总是有点不爽的](http://www.infoq.com/cn/news/2013/06/zhuhong-on-java?utm_source=infoq&utm_medium=popular_links_homepage)
### [计算机科学中最重要的32个算法](http://www.infoq.com/cn/news/2012/08/32-most-important-algorithms?utm_source=infoq&utm_medium=popular_links_homepage)

### [深入理解Java内存模型（一）——基础](http://www.infoq.com/cn/articles/java-memory-model-1?utm_source=infoq&utm_medium=popular_links_homepage)

### [深入浅出Node.js（一）：什么是Node.js](http://www.infoq.com/cn/articles/what-is-nodejs?utm_source=infoq&utm_medium=popular_links_homepage)

### [深入浅出Node.js（二）：Node.js&NPM的安装与配置](http://www.infoq.com/cn/articles/nodejs-npm-install-config?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（3月刊）](http://www.infoq.com/cn/minibooks/architect-mar-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（1月刊）](http://www.infoq.com/cn/minibooks/architect-jan-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（4月刊）](http://www.infoq.com/cn/minibooks/architect-apr-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [聊聊并发（三）——JAVA线程池的分析和使用](http://www.infoq.com/cn/articles/java-threadPool?utm_source=infoq&utm_medium=popular_links_homepage)

### [架构师（2月刊）](http://www.infoq.com/cn/minibooks/architect-feb-10-2013?utm_source=infoq&utm_medium=popular_links_homepage)

### [12306订票助手插件拖垮GitHub事件原因始末](http://www.infoq.com/cn/news/2013/01/12306-plugin-ddos-github?utm_source=infoq&utm_medium=popular_links_homepage)

## 深度内容

* [全部]()
* [文章]()
* [演讲]()
* [访谈]()
* [迷你书]()
## [Juergen Fesslmeier谈端到端的JavaScript开发](http://www.infoq.com/cn/interviews/end-to-end-javascript "Juergen Fesslmeier谈端到端的JavaScript开发")

[Juergen Fesslmeier](http://www.infoq.com/cn/author/Juergen-Fesslmeier "Juergen Fesslmeier") 七月 02, 2013

[![]()](http://www.infoq.com/cn/interviews/end-to-end-javascript "Juergen Fesslmeier谈端到端的JavaScript开发")

## [设计模式自动化](http://www.infoq.com/cn/articles/Design-Pattern-Automation "设计模式自动化")

[Gael Fraiteur and Yan Cui](http://www.infoq.com/cn/author/Gael-Fraiteur-and-Yan-Cui "Gael Fraiteur and Yan Cui") 七月 01, 2013

[![]()](http://www.infoq.com/cn/articles/Design-Pattern-Automation "设计模式自动化")
## [设计指尖上的世界：移动用户界面一瞥](http://www.infoq.com/cn/articles/designing-a-world-at-your-fingertips-mobile-user-interfaces "设计指尖上的世界：移动用户界面一瞥")

[Forrest Shull](http://www.infoq.com/cn/author/Forrest-Shull "Forrest Shull") 六月 28, 2013

[![]()](http://www.infoq.com/cn/articles/designing-a-world-at-your-fingertips-mobile-user-interfaces "设计指尖上的世界：移动用户界面一瞥")

## [成功的根本—集成的ALM工具](http://www.infoq.com/cn/articles/Integrated-ALM "成功的根本—集成的ALM工具")

[Dave West](http://www.infoq.com/cn/author/Dave-West "Dave West") 六月 28, 2013

[![]()](http://www.infoq.com/cn/articles/Integrated-ALM "成功的根本—集成的ALM工具")
## [书评：验收测试驱动开发实践指南](http://www.infoq.com/cn/articles/atdd-by-example-book "书评：验收测试驱动开发实践指南")

[Manuel Pais](http://www.infoq.com/cn/author/Manuel-Pais "Manuel Pais") 六月 26, 2013

[![]()](http://www.infoq.com/cn/articles/atdd-by-example-book "书评：验收测试驱动开发实践指南")

## [跨终端的web](http://www.infoq.com/cn/presentations/across-device-web "跨终端的web")

[舒文亮](http://www.infoq.com/cn/author/%E8%88%92%E6%96%87%E4%BA%AE "舒文亮") 六月 26, 2013

[![]()](http://www.infoq.com/cn/presentations/across-device-web "跨终端的web")

* [更早的 >]()

赞助商链接
### InfoQ每周精要

通过个性化定制的新闻邮件、RSS Feeds和InfoQ业界邮件通知，保持您对感兴趣的社区内容的时刻关注。
[![]()](http://www.infoq.com/cn/newsletter_sample.html)

语言 & 开发

[Juergen Fesslmeier谈端到端的JavaScript开发](http://www.infoq.com/cn/interviews/end-to-end-javascript "Juergen Fesslmeier谈端到端的JavaScript开发")

[MobileCloud for TFS支持测试Windows Phone,Android,iOS及BlackBerry应用](http://www.infoq.com/cn/news/2013/07/mobilecloud-tfs "MobileCloud for TFS支持测试Windows Phone,Android,iOS及BlackBerry应用")

[百度技术沙龙第39期回顾：前端快速开发实践（含资料下载）](http://www.infoq.com/cn/news/2013/07/baidu-salon39-summary "百度技术沙龙第39期回顾：前端快速开发实践（含资料下载）")

架构 & 设计

[内存与本机代码的性能](http://www.infoq.com/cn/news/2013/07/Native-Performance "内存与本机代码的性能")

[设计模式自动化](http://www.infoq.com/cn/articles/Design-Pattern-Automation "设计模式自动化")

[连接设备编程](http://www.infoq.com/cn/news/2013/07/WinRT-Devices "连接设备编程")
过程 & 实践

[成功的根本—集成的ALM工具](http://www.infoq.com/cn/articles/Integrated-ALM "成功的根本—集成的ALM工具")

[ThoughtWorks全球CEO郭晓谈软件人才的招聘与培养](http://www.infoq.com/cn/news/2013/06/tw-guoxiao-on-talents "ThoughtWorks全球CEO郭晓谈软件人才的招聘与培养")

[书评：验收测试驱动开发实践指南](http://www.infoq.com/cn/articles/atdd-by-example-book "书评：验收测试驱动开发实践指南")

运维 & 基础架构

[在传统企业中引入DevOps](http://www.infoq.com/cn/news/2013/06/devops-in-traditional-enterprise "在传统企业中引入DevOps")

[安全性——“DevOpS”中的S](http://www.infoq.com/cn/news/2013/06/dod-ams-day1-s-is-for-security "安全性——“DevOpS”中的S")

[书评：验收测试驱动开发实践指南](http://www.infoq.com/cn/articles/atdd-by-example-book "书评：验收测试驱动开发实践指南")
企业架构

[设计指尖上的世界：移动用户界面一瞥](http://www.infoq.com/cn/articles/designing-a-world-at-your-fingertips-mobile-user-interfaces "设计指尖上的世界：移动用户界面一瞥")

[Stratos 2.0已发布，支持所有运行时环境和30个IaaS](http://www.infoq.com/cn/news/2013/06/stratos-2 "Stratos 2.0已发布，支持所有运行时环境和30个IaaS")

[让1.5亿移动端用户第一时间获取消息](http://www.infoq.com/cn/articles/iqiyi-cloud-push-practices "让1.5亿移动端用户第一时间获取消息")

* [首页](http://www.infoq.com/cn/ "首页")
* [全部话题](http://www.infoq.com/cn/topics "全部话题")
* [QCon全球软件开发大会](http://www.qconferences.com/ "QCon全球软件开发大会")
* [关于我们](http://www.infoq.com/cn/aboutus "关于我们")
* [让大家在InfoQ上听见你的声音](http://www.infoq.com/cn/contribute "让大家在InfoQ上听见你的声音")
* [创建账号](http://www.infoq.com/cn/reginit.action "创建账号")
* [登录]( "登录")

* **会议**
* [QCon全球软件开发大会（上海站）2013，11月1-3日](http://www.qconshanghai.com/ "技术大会：QCon上海2013，11月1-3日 ")
### InfoQ每周精要

通过个性化定制的新闻邮件、RSS Feeds和InfoQ业界邮件通知，保持您对感兴趣的社区内容的时刻关注。

[![]()](http://www.infoq.com/cn/newsletter_sample.html)

* [属于您的个性化RSS](http://www.infoq.com/cn/rss/rss.action?token=AjlwmaWcwi5ejkcT6GcRk1pCqJ4K7ocs)
* [新浪微博](http://weibo.com/infoqchina)
* [社区新闻和热点](http://www.facebook.com/InfoQ)

特别专题

* [**技术社区活动日历**](http://www.infoq.com/cn/chinaitevents)
* [**百度技术沙龙**](http://www.infoq.com/cn/zones/baidu-salon/)
* [**Scrum开发**](http://www.infoq.com/cn/scrum/)
* [**月刊：《架构师》**](http://www.infoq.com/cn/architect/)
* [**线下活动：QClub**](http://www.infoq.com/cn/qclub/)
定制您感兴趣的技术领域

语言 & 开发 架构 & 设计 过程 & 实践 运维 & 基础架构 企业架构
提供反馈

[feedback@cn.infoq.com](mailto:feedback@cn.infoq.com)
 错误报告

[bugs@cn.infoq.com](mailto:bugs@cn.infoq.com)
 商务合作

[sales@cn.infoq.com](mailto:sales@cn.infoq.com)
 内容合作

[editors@cn.infoq.com](mailto:editors@cn.infoq.com)
 InfoQ.com及所有内容，版权所有 © 2006-2013 C4Media Inc. InfoQ.com 服务器由 [Contegix](http://www.contegix.com/)提供, 我们最信赖的ISP合作伙伴。
[隐私政策](http://www.infoq.com/cn/privacypolicy)

[Close]()  E-mail  密码

[使用Google账号登录](http://www.infoq.com/cn/social/googleLogin.action?fl=login "使用Google账号登录")  [使用Microsoft账号登录](http://www.infoq.com/cn/social/liveLogin.action?fl=login "使用Microsoft账号登录")
[忘记密码？]()
   InfoQ账号使用的E-mail  发送邮件

[重新登录]()
重新发送激活信息  重新发送

[重新登录]()
[没有用户名？](http://www.infoq.com/cn/reginit.action)

[点击注册](http://www.infoq.com/cn/reginit.action)
