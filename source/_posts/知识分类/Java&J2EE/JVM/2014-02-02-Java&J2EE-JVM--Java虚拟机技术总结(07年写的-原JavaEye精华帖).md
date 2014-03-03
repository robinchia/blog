---
layout: post
title: "Java虚拟机技术总结(07年写的-原JavaEye精华帖)"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# Java虚拟机技术总结(07年写的,原JavaEye精华帖)

原文：IBM WebSphere Application Server 诊断和调优（一））
大家可以google：“IBM WebSphere Application Server 诊断和调优”。
近段时间，我们项目中用到的WebSphere应用服务器(WAS)，但在客户的production环境下极不稳定，经常宕机。给客户造成非常不好的影响，同时，也给项目组很大压力。为此，我们花了近一个月时间对其诊断，现在基本上稳定了，需要继续观察一段时间。现在我主要将工作做一个阶段性的总结。
我们的产品环境是：WAS6.0＋DB2 8.1＋AIX5.3＋RS/6000。在该产品环境下，出现的问题非常多，现象如下：
WAS经常不稳定、宕机几乎一天一次，经常报告OutOfMemory(内存泄漏吗？NO)。
DB2连接数过大，有时把DB2撑死，有时也把AIX撑死。
AIX虚拟内存报错、分页报错、IO也报错、还有很多其它莫名奇妙的错。
总是，每次问题发生的现象和理论上的总是不一致，导致我们不知道从何入手，也无从检测自己的优化参数。咨询过多次IBM技术支持，只解决了某些局部问题。
虽然问题依然存在，但我想，解决问题的思路、特别是理论基础，还是有一些规律和原则。
对于WAS这块，我近段时间的主要时间集中在以下几个方面(时间顺序)：
1、Java性能监测工具：Jprofiler，也用到Jprobe。后来发现Jprofiler在AIX下几乎不可用。
2、IBM Java虚拟机和WAS技术细节，特别是IBM JVM的GC原理，我发现它和sun、bea的差别很大。
3、IBM的heap分析器Heap Analyzer、GCCollector。这两个事后监测工具非常实用，特别是我们的产品运行环境，非测试环境。
4、某些Application的怀疑和诊断。
5、AIX诊断，我几乎没有这个能力，只能常规监测一下，需另请高人。
我打算将本文分成以下几个部分总结：
JVM原理、IBM JVM的GC策略和调优
Jprofiler和IBM工具的实际体会
WAS的诊断体会和AIX调优
下面开始主题吧，可能比较零碎，另外，开始的理论篇基本上看书都可以，我只是总结一下，再添加一些自己的理解。
以下是我参考的最重要的两本电子书和一些网站：
《Inside Java Vrtual Machine》:半部分有约四章我认为非常棒，其它章节可能意义不大。
《The Java Virtual Machine Specification, 2nd》：前半部分有两三章很不错，不过可以对照上一本书看。
sun的hotspot虚拟机技术：[url]http://java.sun.com/javase/technologies/hotspot/ [/url]
BEA的JRockit虚拟机技术：[url]http://edocs.bea.com/jrockit/geninfo/genintro/index.html [/url]JVM技术文档入口，虚拟机理论，内存泄漏诊断等的索引页。
IBM诊断资料：http://www-128.ibm.com/developerworks/java/jdk/diagnosis/ 上面有一个500多页的pdf文档，对IBM JVM技术和诊断讲解很深入。
我不得不提的是，在查资料这块，BEA和Sun都有很好的官方文档和论坛支持，并且官方文档导航非常好。虽然IBM的诊断资料也不少，但需要搜索，其搜索是很痛苦的。而且，IBM官方论坛很差。如果用IBM的产品出问题，切记：找IBM技术支持，千万不要蒙着头搞！反正它们的产品很少免费。说实话，它们的技术支持还是挺负责的，一般会为你推荐很多support资料，而该资料往往都在developerworks网站上，属于support那个频道，但你就是搜不着。
**Java虚拟机规范概要**
研究Java虚拟机，首先要了解Sun的Java虚拟机规范。现在，该实现版本很多，如比较有名的Sun、IBM、BEA、Apple、HP、MS、Apache Harmony。它们都实现了JVM规范，但有各自扩展。譬如，针对IBM虚拟机的堆碎片导致OutOfMemory（OOM），在Sun的虚拟机上就不会发生。Sun的JVM有maxPermSize的概念，IBM就没有，如果你设置这个参数，虚拟机根本就启动不了。
比较有意思的是，学Java，就一定要了解各种规范，这和MS的风格很不一样。Sun总是在定义一些规范，实现都留给各厂商。我们除了理解规范本身外，一定要理解规范和实现之间的关系，譬如JDBC规范和JDBC驱动的关系，它们是怎么组合到一起的。要是你用过php的xml解析库，或db函数，就会体会深刻，它们可没有什么规范可言，所以每个数据库厂商的db函数用法都不一样。我推荐大家研读一下HSQLDB的jdbc和Tomcat的servlet相关实现，因为我认为它们还是比较好懂的。
JVM规范只是定义一个虚拟机该做什么，但它并没有要求你该怎么做。例如我们最常见的Servlet规范，在该规范中，有HttpServletRequest、HttpServletResponse，HttpSession等接口，但它们的实现都留给了各个容器厂商。遗憾的是，规范留下的空白，会把我们这些开发人员给整惨了：容器间移植有时候就是恶梦。譬如J2EE并没有SSO规范，但它很重要，我以前专门针对它做过WebSphere AppServer和Weblogic AppServer的SSO项目，差别还是不小，不过还是有点共通，那就是都遵循JAAS规范。
**JVM的结构**
从功能上分，Java虚拟机主要由六个部分组成，可以分成三类：
第一类：JVM API：就是我们最常用的Java API，它是开发人员和Java交互的入口，它主要是JAVA_HOME/jre/lib下的运行时类库rt.jar和编译相关的tools.jar
第二类：JVM内部组件
类装载器(ClassLoader)：将Byte Array的 .class文件装载、链接和初始化。
内存管理(Memory Managent)：为对象分配内存，以及释放内存。后者就是垃圾回收Garbage Collector（GC）。由于JVM最复杂的、最影响性能的就是GC，所以内存管理一般就指垃圾回收。
诊断接口(Diagostics Interface)：这主要体现在JVMTI(jdk1.4下的JVMPI和JVMDI)，它主要用来诊断程序的问题和性能，一般提供给工具厂商实现。如eclispe IDE下的debug功能，Jprofiler性能调优工具。
类解释器(Interpreter)：解释装载进虚拟机的class对象，包括JIT等特性相关。
第三类：平台相关接口(Platform Interface)：主要为了跨操作系统平台重用JVM代码，不过，它和我们开发人员关系不大。
在以上六个组件中，我们开发人员最关心的是ClassLoader和GC，用Java做系统框架、容器和它们密切相关。做业务系统时一些基础代码也和它们打交道，譬如最常用的Class.forName(),Thread.currentThread.getContextClassLoader()。我们仔细想想，为什么是上面两个问题？因为，它和我们class的整个生命周期最为相关：怎么将一个class和相关class加载进来，class实例什么时候创建，什么时候被销毁？
所以，下面的部分我们要专门讨论这些问题。
**ClassLoader**
JVM主要有三类ClassLoader：Bootstrap、Extention、Application，该三类ClassLoader从上到下是分级(hierarchy)结构，遵循代理模型(Delegation Model)。
Tip：大家可以看看sun.misc.Launcher的源码，Bootstrap和Extention就在该文件里。该src可以在sun的网站上下载该压缩包，约60M(jdk-1_5_0-src-scsl.zip)，它不在jdk自带的那个src.zip里。
Bootstrap ClassLoader：也称为primordial(root) class loader。主要是负责装载jre/lib下的jar文件，当然，你也可以通过-Xbootclasspath参数定义。该ClassLoader不能被Java代码实例化，因为它是JVM本身的一部分。
Extention ClassLoader：该ClassLoader是Bootstrap classLoader的子class loader。它主要负责加载jre/lib/ext/下的所有jar文件。只要jar包放置这个位置，就会被虚拟机加载。一个常见的、类似的问题是，你将mysql的低版本驱动不小心放置在这儿，但你的Web应用程序的lib下有一个新的jdbc驱动，但怎么都报错，譬如不支持JDBC2.0的DataSource，这时你就要当心你的新jdbc可能并没有被加载。这就是ClassLoader的delegate现象。常见的有log4j、common-log、dbcp会出现问题，因为它们很容易被人塞到这个ext目录，或是Tomcat下的common/lib目录。
Application ClassLoader：也称为System ClassLoaer。它负责加载CLASSPATH环境变量下的classes。缺省情况下，它是用户创建的任何ClassLoader的父ClassLoader，我们创建的standalone应用的main class缺省情况下也是由它加载(通过Thread.currentThread().getContextClassLoader()查看)。
我们实际开发中，用ClassLoader更多时候是用其加载classpath下的资源，特别是配置文件，如ClassLoader.getResource()，比FileInputStream直接。
ClassLoader是一种分级(hierarchy)的代理(delegation)模型。
Delegation：其实是Parent Delegation，当需要加载一个class时，当前线程的ClassLoader首先会将请求代理到其父classLoader，递归向上，如果该class已经被父classLoader加载，那么直接拿来用，譬如典型的ArrayList，它最终由Bootstrap ClassLoader加载。并且，每个ClassLoader只有一个父ClassLoader。
Class查找的位置和顺序依次是：Cache、parent、self。
Hierarchy：上面的delegation已经暗示了一种分级结构，同时它也说明：一个ClassLoader只能看到被它自己加载的classes，或是看到其父(parent) ClassLoader或祖先(ancestor) ClassLoader加载的Classes。
在一个单虚拟机环境下，标识一个类有两个因素：class的全路径名、该类的ClassLoader。
我碰到的一个典型的例子是：在做WAS的SSO开发时，由于我们的类是由WAS在启动时加载，该ClassLoader比下面的部署的Applicaton的ClassLoader的级别高。所以，在我们自己的类中没法用到应用程序的连接池，必须自建。
代理模型是Java安全模型的保证。譬如，我们自己写一个String.java，并且编译、package到自己的java.lang包下。按照代理模型，当前线程的ClassLoader会将其代理到父ClassLoader，父ClassLoader(最终会是Bootstrap)会找到rt.jar下的String.class，也就是说我们的String.class不会捣乱。
**自定义ClassLoader**
我们前面说过，自定义ClassLoader的缺省父ClassLoader是Application ClassLoader。一般的应用开发用不到它，但我们最好理解。因为在内存泄漏查找、应用程序部署出问题时，很多都和它有关。
譬如，内存泄漏是怎么产生的？这就涉及到ClassLoader和Class的生命周期。我曾经碰到这样一个问题：我们的程序用到了Webwork和Spring框架，当部署到Tomcat下时没有任何问题，但部署到WAS下，报告找不到Webwork的xml的DTD文件，而且Spring的日志也总是失效。Why？因为解析xml dtd时，用的是IBM的Xerces，不是我们的。而Spring日志问题是因为应用程序用的是WAS的Common-log.jar，而不是我们的。将应用的ClassLoader从默认的Parent-First，改成Parent-Last就可以解决，不过我们项目中用到其它库，又发生了其它问题。
**一般来说，用到自定义ClassLoader有三种情况：**
1、应用框架可以自己控制Classes的目录，并且自动部署。
我读过Jive公司的Wildfire(著名的即时通讯服务器)，它自己有一套应用框架，非常灵活，遵循该框架插件规范的的第三方的plug-in放置在指定目录可以自动部署，实现某些扩展功能，如文件传输、语音聊天。
2、区分用户代码
这被广泛应用在Servlet容器和类似容器，譬如EJB Container设计中，大家看到Tomcat下有common、server、share三个目录吧(ClassLoader顺序从左到有)，另外也有用户应用的WEB-INF目录，它是我们自己开发的。
3、允许Classes卸载
如果没有自定义的ClassLoader，那么我们自己应用中的classes永远都不能被卸载，因为这些类被Application ClassLoader加载后cache起来了，我们的classes一直对该ClassLoader有引用，而该系统级的ClassLoader永远都不会被卸载，除非JVM shutdown了。JSP和Servlet的动态部署就用到这个特性。
待续…….
Note: 还有JVM运行时(Runtime)架构，ClassLoader加载class过程没有总结，这两部分我觉得太重要了，但内容太多，写不完啊。
这部分内容，《Inside Java Virtual Machine》讲解非常清楚，BEA的官方网站这部分也非常不错，要理解深刻，我建议结合JProfiler工具，非常直观。
待续…….
该文仅存的一点回复：
[zwchen]
**为什么都说WAS难用？**
**安装过程**
1、先安装WAS，然后创建三种不同类型的profiles(manager,default,custom)，譬如缺省的profiles吧，在创建这个profiles过程中，需要启动至少10个端口，要是那10个默认端口有些被你的系统占用了，麻烦就来了。改端口不就ok了吗？但你知道怎么改吗？可能你觉得没有必要10个端口，但WAS就是需要这么多：控制台端口两个(http，https),引导端口2089，soap、ORB，单元 cell发现….  总之，你都不知道你为什么要关心这么多端口。WLS够绝了：一个smart端口 7001全部搞定，在上走各种协议。
2、系统为你生成的上百个bat命令文件，里面交叉引用的path都是绝对路径，所以，往往你的机器重装了，那么WAS也重装吧。WLS和JBoss可以将整个文件夹移动，WAS不行，如果你喜欢折腾，当然也可以做到，就看值不值了。
控制台的部署过程
1、它总是以为我们的应用前台是httpd Server分发，于是让我们选择虚拟主机。其实，我们不是CMS系统，我们的Web前端就是WAS的web容器。
2、它总是以为我们的应用包含EJB，或是要发布成Web Services，于是让我们选择进退两难。其实，我们只是简单的Web应用。
3、它总是以为我们的应用要部署分散在多个WAS上，或者在cluster里，所以会强制要求我们选择发布目标位置。其实，我们只是用单Server。
4、它以为我们发布的应用是EAR企业包，而WAR包只是一个Web前端展示模块，所以它总是在xml配置里面写一堆。其实，我们很想手动修改配置文件。
我刚才特别部署了一个简单的web应用，就是Struts的sample，写入了29个xml配置文件。设想，保存部署文件是一个事务性的过程，如果你在大连，服务器在北京，速度超级慢，这个写入文件过程失败，呵呵，你就下地狱吧，我们碰到个几次，有一次就被迫服务器profiles重建。
虽然上面很多步骤我省略了，对于我特别说到的，虽然有默认，但每个默认页有上10个选项，而你并不是很明白这些选择的确切含义，它们是否相互冲突。
我建议WAS的设计师：请留下尽量少的发布步骤，需要定制，可以部署后修改，学学WLS吧：考虑普通开发人员的感受。
**开发过程**
用WAS，最好有其配套的开发工具，eclipse那个WAS插件，我试过不太好用。用RAD吧，要lisence。RAD也是个超级大户，官方推荐内存是最低768M，WAS官方推荐大概是1000M，我的1G机器也可以跑，就是慢了点。
大家如果用WAS的EJB容器，并且你想做standalone的ejb客户端，那么，千万不要选择IBM之外的JVM，我固执地试过，最后用sun的JVM成功了，但需要一堆IBM WAS下的jar，而且必须走IIOP协议，我可以说，初次使用99%的尝试是失败的。而且，此时的开发工具一定要选择IBM的 RAD或WSAD。建议，学习EJB这类分布式开发阶段，用WLS或JBoss吧，因为它们的EJB standalone客户端都支持smart stub。譬如，对于JBoss，它会通过socket先将客户端stub下载到本地，你察觉不到这个过程。大概WLS是通过反射生成stub。而WAS 必须部署过程编译一堆stub，需要引用一堆jar，sun的ejb部署大概也这样。之所以sun的简单点，那是因为你的ejb客户端jar往往自动被它加入了classpath，或者客户端是在AppServer里。
如果你要开发基于WAS的Web Services，并且用WAS内置的Web Services引擎，那么你一定要用它自己的开发工具发布和部署，我虽然成功了，只是因为太固执，付出的代价太大。
**问题诊断**
用WAS出问题比不出问题正常。出问题咋办？这个我暂时不说了，留给我的后文吧，总之一句话：问题独一无二。
我本人并不是说WAS做得烂，我只是想说明一点，它的使用，对我们的理论技术水平要求太高，特别不适合初学者。
———————————————————-
抛出异常的爱 写道
用的是5.只有一本。。。中文的。。
6有没有中文的红宝书？
[zwchen]像是现在还没有，我建议看它的原著吧，非常易懂，图特多，一天看200多页应该不困难。学习WAS，往往一本红皮书是不够的。市面上介绍WAS的书，大概极少有超越红皮书的。
——————————————————–
[zwchen]惭愧啊，我确实算不上高手，真正的高手在北京的IBM中国技术支持中心。我和那边的人电话和邮件聊过好多次，那才是一个字：高！
了解WAS，我随便总结一下吧：
引用

前提：你是项目组专门负责WAS上开发、部署、调优的，并且有与WAS抗争的决心。
1、学习WAS宝典丛书：WAS相关红皮书，前面我介绍过几本。并且多研究研究WAS的目录结构。
2、一本非红皮书，IBM内部的技术支持汇总，主要是关于诊断，但它最难吃透但最受用：《IBM® Developer Kit and Runtime Environment, Java™ 2 Technology Edition, Version 6.0  Diagnostics Guide》，500多页。
3、一定要有JVM相关技术积累：ClassLoader、GC策略，而且一定要注意它们与Sun的JVM的区别，往往WAS的问题发生在这个上面。如Sun的JVM一般建议heap的最大最小值一样，但IBM的JVM你要是这么做会导致严重的碎片问题。Why？默认的GC策略不同。
4、用JProfiler这类工具深入到WAS内部。
5、最好对JavaEE相关技术的原理有较深入的了解，譬如EJB的原理、Servlet的原理。而且，最好是这些技术怎么实现的，譬如读读Tomcat的源码,我强烈建议大家读一下这篇文章：http://www.onjava.com/pub/a/onjava/2003/05/14 /java_webserver.html。
整体体会，WAS的功底是在WAS之外，它只是对JVM、JavaEE规范的一个实现罢了。
另外也建议：不要花太多的时间的在WAS上，我认为非常不值。想研究，还不如去看JBoss，WLS。WAS这东西，知道一般的用法就够了，而且你永远都不可能明白它为什么有那么多的bug和不合理，明白了又能咋样？你的时间都白白浪费了。
