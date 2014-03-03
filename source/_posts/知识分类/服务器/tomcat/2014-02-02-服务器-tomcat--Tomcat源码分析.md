---
layout: post
title: "Tomcat源码分析"
categories: 服务器
tags: 
 - 服务器
 - tomcat
--- 

# Tomcat源码分析

[Tomcat源码分析](http://blog.csdn.net/haojun186/article/details/7464912)

TOMCAT源码分析(启动框架)
前言：
   本文是我阅读了TOMCAT源码后的一些心得。 主要是讲解TOMCAT的系统框架， 以及启动流程。若有错漏之处，敬请批评指教！
建议：
   毕竟TOMCAT的框架还是比较复杂的， 单是从文字上理解， 是不那么容易掌握TOMCAT的框架的。 所以得实践、实践、再实践。 建议下载一份TOMCAT的源码， 调试通过， 然后单步跟踪其启动过程。 如果有不明白的地方， 再来查阅本文， 看是否能得到帮助。 我相信这样效果以及学习速度都会好很多！
   
1. Tomcat的整体框架结构
   Tomcat的基本框架， 分为4个层次。
   Top Level Elements:
    Server
    Service   
   Connector
    HTTP
    AJP
   Container
   Engine
     Host
   Context
   Component  
    manager
   logger
   loader
   pipeline
   valve
         ...
   站在框架的顶层的是Server和Service
   Server:  其实就是BackGroud程序， 在Tomcat里面的Server的用处是启动和监听服务端事件（诸如重启、关闭等命令。 在tomcat的标准配置文件：server.xml里面， 我们可以看到“<Server port="8005" shutdown="SHUTDOWN" debug="0">;”这里的"SHUTDOWN"就是server在监听服务端事件的时候所使用的命令字）
   Service： 在tomcat里面，service是指一类问题的解决方案。  通常我们会默认使用tomcat提供的：Tomcat-Standalone 模式的service。 在这种方式下的service既给我们提供解析jsp和servlet的服务， 同时也提供给我们解析静态文本的服务。
   
   Connector: Tomcat都是在容器里面处理问题的， 而容器又到哪里去取得输入信息呢？
Connector就是专干这个的。 他会把从socket传递过来的数据， 封装成Request, 传递给容器来处理。
   通常我们会用到两种Connector,一种叫http connectoer， 用来传递http需求的。 另一种叫AJP， 在我们整合apache与tomcat工作的时候，apache与tomcat之间就是通过这个协议来互动的。 （说到apache与tomcat的整合工作， 通常我们的目的是为了让apache 获取静态资源， 而让tomcat来解析动态的jsp或者servlet。）
   Container: 当http connector把需求传递给顶级的container: Engin的时候， 我们的视线就应该移动到Container这个层面来了。
   在Container这个层， 我们包含了3种容器：Engin, Host, Context.
   Engin: 收到service传递过来的需求， 处理后， 将结果返回给service( service 是通过connector 这个媒介来和Engin互动的).
   Host: Engin收到service传递过来的需求后，不会自己处理， 而是交给合适的Host来处理。
Host在这里就是[虚拟主机](http://www.honhei.com/)的意思， 通常我们都只会使用一个主机，既“localhost”本地机来处理。
   Context: Host接到了从Host传过来的需求后， 也不会自己处理， 而是交给合适的Context来处理。
   比如：<http://127.0.0.1:8080/foo/index.jsp>;
         <http://127.0.1:8080/bar/index.jsp>;
   前者交给foo这个Context来处理， 后者交给bar这个Context来处理。
   很明显吧！context的意思其实就是一个web app的意思。
   我们通常都会在server.xml里面做这样的配置
   <Context path="/foo" docBase="D:/project/foo/web" />;
   这个context容器，就是用来干我们该干的事儿的地方的。
   
   Compenent: 接下来， 我们继续讲讲component是干什么用的。
   我们得先理解一下容器和组件的关系。
   需求被传递到了容器里面， 在合适的时候， 会传递给下一个容器处理。
   而容器里面又盛装着各种各样的组件， 我们可以理解为提供各种各样的增值服务。
   manager: 当一个容器里面装了manager组件后，这个容器就支持session管理了， 事实上在tomcat里面的session管理， 就是靠的在context里面装的manager component.
   logger: 当一个容器里面装了logger组件后， 这个容器里所发生的事情， 就被该组件记录下来啦！ 我们通常会在logs/ 这个目录下看见catalina_log.time.txt 以及localhost.time.txt 和localhost_examples_log.time.txt。 这就是因为我们分别为：engin, host以及context(examples)这三个容器安装了logger组件， 这也是默认安装， 又叫做标配 ：）
   loader: loader这个组件通常只会给我们的context容器使用，loader是用来启动context以及管理这个context的classloader用的。
    pipline: pipeline是这样一个东西， 当一个容器决定了要把从上级传递过来的需求交给子容器的时候， 他就把这个需求放进容器的管道(pipeline)里面去。 而需求傻呼呼得在管道里面流动的时候， 就会被管道里面的各个阀门拦截下来。 比如管道里面放了两个阀门。 第一个阀门叫做“access_allow_vavle”， 也就是说需求流过来的时候，它会看这个需求是哪个IP过来的， 如果这个IP已经在黑名单里面了，sure, 杀！ 第二个阀门叫做“defaul_access_valve”它会做例行的检查， 如果通过的话，OK， 把需求传递给当前容器的子容器。 就是通过这种方式， 需求就在各个容器里面传递，流动， 最后抵达目的地的了。
    valve: 就是上面所说的阀门啦。
   Tomcat里面大概就是这么些东西， 我们可以简单地这么理解tomcat的框架，它是一种自上而下， 容器里又包含子容器的这样一种结构。
2. Tomcat的启动流程
   这篇文章是讲tomcat怎么启动的，既然我们大体上了解了TOMCAT的框架结构了， 那么我们可以望文生意地就猜到tomcat的启动， 会先启动父容器，然后逐个启动里面的子容器。 启动每一个容器的时候， 都会启动安插在他身上的组件。 当所有的组件启动完毕， 所有的容器启动完毕的时候，tomcat本身也就启动完毕了。
   顺理成章地， 我们同样可以猜到，tomcat的启动会分成两大部分， 第一步是装配工作。 第二步是启动工作。
   装配工作就是为父容器装上子容器， 为各个容器安插进组件的工作。 这个地方我们会用到digester模式， 至于digester模式什么， 有什么用， 怎么工作的. 请参考<http://software.ccidnet.com/pub/article/c322_a31671_p2.html>;
   启动工作是在装配工作之后， 一旦装配成功了， 我们就只需要点燃最上面的一根导线， 整个tomcat就会被激活起来。 这就好比我们要开一辆已经装配好了的汽车的时候一样，我们只要把钥匙插进钥匙孔，一拧，汽车的引擎就会发动起来，空调就会开起来， 安全装置就会生效， 如此一来，汽车整个就发动起来了。（这个过程确实和TOMCAT的启动过程不谋而和， 让我们不得不怀疑TOMCAT的设计者是在GE做JAVA开发的）。
2.1 一些有意思的名称：
   Catalina
   Tomcat
   Bootstrap
   Engin
   Host
   Context
   他们的意思很有意思：
   Catalina: 远程轰炸机
   Tomcat: 熊猫轰炸机-- 轰炸机的一种（这让我想起了让国人引以为豪的熊猫手机，是不是英文可以叫做tomcat??? ， 又让我想起了另一则广告： 波导-手机中的战斗机、波音-客机中的战斗机 ）
   Bootstap: 引导
   Engin: 发动机
   Host: 主机，领土
   Context: 内容， 目标， 上下文
   
   ... 在许多许多年后， 现代人类已经灭绝。 后现代生物发现了这些单词零落零落在一块。 一个自以为聪明的家伙把这些东西翻译出来了：
   在地勤人员的引导(bootstrap)下， 一架轰炸架(catalina)腾空跃起， 远看是熊猫轰炸机(tomcat)， 近看还是熊猫轰炸机！ 凭借着优秀的发动机技术(engin)， 这架熊猫轰炸机飞临了敌国的领土上空(host)， 对准目标(context)投下了毁天灭地的核弹头，波~ 现代生物就这么隔屁了~
 
   综上所述， 这又不得不让人联想到GE是不是也参与了军事设备的生产呢？
   反对美帝国主义！ 反对美霸权主义！ 和平万岁！ 自由万岁！
   
2.2  历史就是那么惊人的相似！tomcat的启动就是从org.apache.catalina.startup.Bootstrap这个类悍然启动的！
   在Bootstrap里做了两件事：
   1. 指定了3种类型classloader:
      commonLoader: common/classes、common/lib、common/endorsed
      catalinaLoader: server/classes、server/lib、commonLoader
      sharedLoader：  shared/classes、shared/lib、commonLoader
   2. 引导Catalina的启动。
      用Reflection技术调用org.apache.catalina.startup.Catalina的process方法， 并传递参数过去。
   
2.3 Catalina.java
   Catalina完成了几个重要的任务：
   1. 使用Digester技术装配tomcat各个容器与组件。
      1.1 装配工作的主要内容是安装各个大件。 比如server下有什么样的servcie。Host会容纳多少个context。Context都会使用到哪些组件等等。
      1.2 同时呢， 在装配工作这一步， 还完成了mbeans的配置工作。 在这里，我简单地但不十分精确地描述一下mbean是什么，干什么用的。
          我们自己生成的对象， 自己管理， 天经地义！ 但是如果我们创建了对象了， 想让别人来管， 怎么办呢？ 我想至少得告诉别人我们都有什么， 以及通过什么方法可以找到  吧！JMX技术给我们提供了一种手段。JMX里面主要有3种东西。Mbean, agent, connector.
       Mbean： 用来映射我们的对象。也许mbean就是我们创建的对象， 也许不是， 但有了它， 就可以引用到我们的对象了。
       Agent:  通过它， 就可以找到mbean了。
       Connector: 连接Agent的方式。 可以是http的， 也可以是rmi的，还可以直接通过socket。
      发生在tomcat 装配过程中的事情:  GlobalResourcesLifecycleListener 类的初始化会被触发：
         protected static Registry registry = MBeanUtils.createRegistry();  会运行
         MBeanUtils.createRegistry()  会依据/org/apache/catalina/mbeans/mbeans- descriptors.xml这个配置文件创建mbeans. Ok, 外界就有了条途径访问tomcat中的各个组件了。（有点像后门儿）
   2. 为top level 的server 做初始化工作。 实际上就是做通常会配置给service的两条connector.(http, ajp)
   3. 从server这个容器开始启动， 点燃整个tomcat.
   4. 为server做一个hook程序， 检测当server shutdown的时候， 关闭tomcat的各个容器用。
   5. 监听8005端口， 如果发送"SHUTDOWN"（默认培植下字符串）过来， 关闭8005serverSocket。
2.4 启动各个容器
   1. Server
      触发Server容器启动前(before_start)， 启动中(start)， 启动后(after_start)3个事件， 并运行相应的事件处理器。
      启动Server的子容器：Servcie.
   2. Service
      启动Service的子容器：Engin
      启动Connector
   3. Engin
      到了Engin这个层次，以及以下级别的容器，Tomcat就使用了比较一致的启动方式了。
      首先，  运行各个容器自己特有一些任务
      随后，  触发启动前事件
      立即，  设置标签，就表示该容器已经启动
      接着，  启动容器中的各个组件：loader, logger, manager等等
      再接着，启动mapping组件。（注1）
      紧跟着，启动子容器。
      接下来，启动该容器的管道(pipline)
      然后，  触发启动中事件
      最后，  触发启动后事件。
 
      Engin大致会这么做，Host大致也会这么做， Context大致还是会这么做。 那么很显然地， 我们需要在这里使用到代码复用的技术。tomcat在处理这个问题的时候， 漂亮地使用了抽象类来处理。ContainerBase. 最后使得这部分完成复杂功能的代码显得干净利落， 干练爽快， 实在是令人觉得叹为观止， 细细品来， 直觉如享佳珍， 另人齿颊留香， 留恋往返啊！
      
      Engin的触发启动前事件里， 会激活绑定在Engin上的唯一一个Listener：EnginConfig。
      这个EnginConfig类基本上没有做什么事情， 就是把EnginConfig的调试级别设置为和Engin相当。 另外就是输出几行文本， 表示Engin已经配置完毕， 并没有做什么实质性的工作。
      注1: mapping组件的用处是， 当一个需求将要从父容器传递到子容器的时候， 而父容器又有多个子容器的话， 那么应该选择哪个子容器来处理需求呢？ 这个由mapping 组件来定夺。
   
   4. Host
       同Engin一样， 也是调用ContainerBase里面的start()方法， 不过之前做了些自个儿的任务,就是往Host这个容器的通道（pipline）里面， 安装了一个叫做
“org.apache.catalina.valves.ErrorReportValve”的阀门。
       这个阀门的用处是这样的：  需求在被Engin传递给Host后， 会继续传递给Context做具体的处理。 这里需求其实就是作为参数传递的Request, Response。 所以在context把需求处理完后， 通常会改动response。 而这个org.apache.catalina.valves.ErrorReportValve的作用就是检察response是否包含错误， 如果有就做相应的处理。
   5. Context
       到了这里， 就终于轮到了tomcat启动中真正的重头戏，启动Context了。
StandardContext.start() 这个启动Context容器的方法被StandardHost调用.
5.1 webappResources 该context所指向的具体目录
5.2 安装defaultContex, DefaultContext 就是默认Context。 如果我们在一个Host下面安装了DefaultContext，而且defaultContext里面又安装了一个数据库连接池资源的话。 那么其他所有的在该Host下的Context, 都可以直接使用这个数据库连接池， 而不用格外做配置了。
  5.3 指定Loader. 通常用默认的org.apache.catalina.loader.WebappLoader这个类。   Loader就是用来指定这个context会用到哪些类啊， 哪些jar包啊这些什么的。
5.4 指定Manager. 通常使用默认的org.apache.catalina.session. StandardManager 。Manager是用来管理session的。
     其实session的管理也很好实现。 以一种简单的session管理为例。 当需求传递过来的时候， 在Request对象里面有一个sessionId 属性。OK， 得到这个sessionId后， 我们就可以把它作为map的key，而value我们可以放置一个HashMap. HashMap里边儿， 再放我们想放的东西。
5.5 postWorkDirectory (). Tomcat下面有一个work目录。 我们把临时文件都扔在那儿去。 这个步骤就是在那里创建一个目录。 一般说来会在%CATALINA_HOME%/work/Standalone\localhost\ 这个地方生成一个目录。
5.6  Binding thread。到了这里， 就应该发生class Loader 互换了。 之前是看得见tomcat下面所有的class和lib. 接下来需要看得见当前context下的class。 所以要设置contextClassLoader, 同时还要把旧的ClassLoader记录下来，因为以后还要用的。
5.7  启动Loader. 指定这个Context具体要使用哪些classes， 用到哪些jar文件。 如果reloadable设置成了true, 就会启动一个线程来监视classes的变化， 如果有变化就重新启动Context。
5.8  启动logger
5.9  触发安装在它身上的一个监听器。
lifecycle.fireLifecycleEvent(START_EVENT, null);
作为监听器之一，ContextConfig会被启动. ContextConfig就是用来配置web.xml的。 比如这个Context有多少Servlet， 又有多少Filter， 就是在这里给Context装上去的。
5.9.1 defaultConfig. 每个context都得配置tomcat/conf/web.xml 这个文件。
5.9.2 applicationConfig 配置自己的WEB-INF/web.xml 文件
5.9.3 validateSecurityRoles 权限验证。 通常我们在访问/admin 或者/manager的时候，需要用户要么是admin的要么是manager的， 才能访问。 而且我们还可以限制那些资源可以访问， 而哪些不能。 都是在这里实现的。
5.9.4 tldScan: 扫描一下， 需要用到哪些标签(tag lab)
5.10 启动manager
5.11 postWelcomeFiles() 我们通常会用到的3个启动文件的名称：
index.html、index.htm、index.jsp 就被默认地绑在了这个context上
5.12 listenerStart 配置listener
5.13 filterStart 配置filter
5.14 启动带有<load-on-startup>;1</load-on-startup>;的Servlet.
  顺序是从小到大：1,2,3… 最后是0
  默认情况下， 至少会启动如下3个的Servlet:
  org.apache.catalina.servlets.DefaultServlet   
      处理静态资源的Servlet. 什么图片啊，html啊，css啊，js啊都找他
  org.apache.catalina.servlets.InvokerServlet
      处理没有做Servlet Mapping的那些Servlet.
  org.apache.jasper.servlet.JspServlet
      处理JSP文件的.
       5.15  标识context已经启动完毕。
走了多少个步骤啊，Context总算是启动完毕喽。
    OK! 走到了这里， 每个容器以及组件都启动完毕。Tomcat终于不辞辛劳地为人民服务了！
3. 参考文献：
    <http://jakarta.apache.org/tomcat/>;
    <http://www.onjava.com/pub/a/onjava/2003/05/14/java_webserver.html>;
   
4. 后记
    这篇文章是讲解tomcat启动框架的，还有篇文章是讲解TOMCAT里面的消息处理流程的细节的。 文章内容已经写好了， 现在正在整理阶段。 相信很快就可以做出来， 大家共同研究共同进步。
    这篇文章是独自分析TOMCAT[源码](http://www.2cto.com/ym)所写的， 所以一定有地方是带有个人主观色彩， 难免会有片面之处。若有不当之处敬请批评指教，这样不仅可以使刚开始研究TOMCAT的兄弟们少走弯路， 我也可以学到东西。
 
5. tomcat源码分析(消息处理)
 
[ZT]TOMCAT源码分析
0：前言
我们知道了tomcat的整体框架了， 也明白了里面都有些什么组件， 以及各个组件是干什么用的了。
 
http://www.csdn.net/Develop/read_article.[asp](http://www.2cto.com/kf/web/asp/)?id=27225
 
我想，接下来我们应该去了解一下tomcat 是如何处理[jsp](http://www.2cto.com/kf/web/jsp/)和servlet请求的。
 
 
 
1.  我们以一个具体的例子，来跟踪TOMCAT， 看看它是如何把Request一层一层地递交给下一个容器， 并最后交给Wrapper来处理的。
 
以http://localhost:8080/web/login.jsp为例子
 
（以下例子， 都是以tomcat4 源码为参考）
 
 
 
这篇心得主要分为3个部分： 前期， 中期， 和末期。
 
前期：讲解了在[浏览器](http://www.2cto.com/os/liulanqi/)里面输入一个URL，是怎么被tomcat抓住的。
 
中期：讲解了被tomcat抓住后，又是怎么在各个容器里面穿梭， 最后到达最后的处理地点。
 
末期：讲解到达最后的处理地点后，又是怎么具体处理的。
 
2、  前期Request的born.
 
    在这里我先简单讲一下request这个东西。
 
     我们先看着这个URL：http://localhost:8080/web/login.jsp  它是动用了8080端口来进行socket通讯的。
 
     我们知道, 通过
 
       InputStream in = socket.getInputStream() 和
 
       OutputStream out = socket.getOutputStream()
 
     就可以实现消息的来来往往了。
 
     但是如果把Stream给应用层看，显然操作起来不方便。
 
     所以，在tomcat 的Connector里面，socket被封装成了Request和Response这两个对象。
 
     我们可以简单地把Request看成管发到服务器来的数据，把Response看成想发出服务器的数据。
 
    
 
     但是这样又有其他问题了啊？Request这个对象是把socket封装起来了， 但是他提供的又东西太多了。
 
     诸如Request.getAuthorization(), Request.getSocket()。  像Authorization这种东西开发人员拿来基本上用不太着，而像socket这种东西，暴露给开发 人员又有潜在的危险。 而且啊， 在Servlet Specification里面标准的通信类是ServletRequest和HttpServletRequest，而非这个Request类。So, So, So. Tomcat必须得捣持捣持Request才行。 最后tomcat选择了使用捣持模式（应该叫适配器模式）来解决这个问题。它把org.apache.catalina.Request 捣持成了org.apache.coyote.tomcat4.CoyoteRequest。 而CoyoteRequest又实现了ServletRequest和HttpServletRequest 这两种接口。 这样就提供给开发人员需要且刚刚需要的方法了。
 
 
 
    ok, 让我们在tomcat的顶层容器- StandardEngin 的invoke()方法这里设置一个断点， 然后访问
 
    http://localhost:8080/web/login.jsp ， 我们来看看在前期都会路过哪些地方：
 
       1. run(): 536, java.lang.Thread, Thread.java
 
       CurrentThread
 
      2.  run():666, org.apache.tomcat.util.threads.ThreadPool$ControlRunnable, ThreadPool.java
 
               ThreadPool
 
       3.  runIt():589, org.apache.tomcat.util.net.TcpWorkerThread, PoolTcpEndpoint.java
 
          ThreadWorker
 
4.        processConnection():  549
 
org.apache.coyote.http11.Http11Protocol$Http11ConnectionHandler, Http11Protocol.java
 
                  http protocol parser
 
      5.  Process(): 781, org.apache.coyote.http11.Http11Processor, Http11Processor.java
 
          http request processor
 
       6. service(): 193, org.apache.coyote.tomcat4.CoyoteAdapter,CoyoteAdapter.java
 
          adapter
 
       7. invoke(): 995, org.apache.catalina.core.ContainerBase, ContainerBase.java
 
   StandardEngin
 
 
 
    1. 主线程
 
    2. 启动线程池.
 
    3. 调出线程池里面空闲的工作线程。
 
    4. 把8080端口传过来由httpd协议封装的数据，解析成Request和Response对象。
 
    5. 使用Http11Processor来处理request
 
    6. 在Http11Processor里面， 又会call CoyoteAdapter来进行适配处理，把Request适配成实现了ServletRequest和HttpServletRequest接口的CoyoteRequest.
 
7. 到了这里，前期的去毛拔皮工作就基本上搞定，可以交给StandardEngin 做核心的处理工作了。
 
 
 
3. 中期。 在各个容器间的穿梭。
 
    Request在各个容器里面的穿梭大致是这样一种方式：
 
    每个容器里面都有一个管道（pipline）， 专门用来传送Request用的。
 
    管道里面又有好几个阀门（valve）， 专门用来过滤Request用的。
 
    在管道的低部通常都会放上一个默认的阀们。 这个阀们至少会做一件事情，就是把Request交给子容器。
 
    让我们来想象一下：
 
     当一个Request进入一个容器后， 它就在管道里面流动，波罗~ 波罗~ 波罗~ 地穿过各个阀门。在流到最后一个阀门的时候，吧唧~ 那个该死的阀门就把它扔给了子容器。 然后又开始 波罗~ 波罗~ 波罗~ ... 吧唧~....  波罗~  波罗~ 波罗~ ....吧唧~....
 
    就是通过这种方式，Request 走完了所有的容器。（ 感觉有点像消化系统，最后一个地方有点像那里~  ）
 
    OK， 让我们具体看看都有些什么容器， 各个容器里面又都有些什么阀门，这些阀们都对我们的Request做了些什么吧：
 
 
 
3.1 StandardEngin 的pipeline里面放的是：StandardEnginValve
 
在这里，VALVE做了三件事：
 
1.   验证传递过来的request是不是httpservletRequest.
 
2    验证传递过来的request 是否携带了host header信息.
 
3    选择相应的host去处理它。（一般我们都只有一个host:localhost，也就是127.0.0.1）。
 
到了这个地方， 我们的request就已经完成了在Engin这个部分的历史使命， 通向前途未卜的下一站：host了。
 
 
 
3.2 StandardHost 的pipline里面放的是：StandardHostValve
 
1.   验证传递过来的request是不是httpservletRequest.
 
2.   根据Request来确定哪个Context来处理。
 
Context其实就是webapp， 比如http://localhost:8080/web/login.jsp
 
这里web就是Context罗！
 
3.   既然确定了是哪个Context了，那么就应该把那个Context的classloader付给当前线程了。
 
        Thread.currentThread().setContextClassLoader(context.getLoader().getClassLoader());
 
   这样request就只看得见指定的context下面的classes啊，jar啊这些， 而看不见tomcat本身的类， 什么Engin啊，Valve啊。 不然还得了啊！
 
4. 既然request到了这里了，看来用户是准备访问web这个web app了，咋们得更新一下这个用户的session不是！Ok , 就由manager更新一下用户的session信息
 
5. 交给具体的Context 容器去继续处理Request.
 
6. Context处理完毕了，把classloader还回来。
 
 
 
3.3 StandardContext 的pipline里面放的是：StandardContextValve
 
1.   验证传递过来的request是不是httpservletRequest.
 
2.   如果request意图不轨，想要访问/meta-inf, /web-inf这些目录下的东西，呵呵，没有用D!
 
3.   这个时候就会根据Request到底是Servlet，还是jsp，还是静态资源来决定到底用哪种Wrapper来处理这个Reqeust了。
 
4.   一旦决定了到底用哪种Wrapper，OK，交给那个Wrapper处理。
 
 
 
4. 末期。 不同的需求是怎么处理的.
 
StandardWrapper
 
之前对Wrapper没有做过讲解，其实它是这样一种东西。
 
我们在处理Request的时候，可以分成3种。
 
处理静态的：org.apache.catalina.servlets.DefaultServlet  
 
处理jsp的：org.apache.jasper.servlet.JspServlet
 
处理servlet的：org.apache.catalina.servlets.InvokerServlet
 
不同的request就用这3种不同的servlet去处理。
 
Wrapper就是对它们的一种简单的封装，有了Wrapper后，我们就可以轻松地拦截每次的Request。也可以容易地调用servlet的init()和destroy()方法， 便于管理嘛！
 
具体情况是这么滴：
 
   如果request是找jsp文件，StandardWrapper里面就会封装一个org.apache.jasper.servlet.JspServlet去处理它。
 
   如果request是找 静态资源 ，StandardWrapper里面就会封装一个org.apache.jasper.servlet.DefaultServlet  去处理它。
 
   如果request是找servlet ，StandardWrapper里面就会封装一个org.apache.jasper.servlet.InvokerServlet 去处理它。
 
 
 
StandardWrapper同样也是容器，既然是容器， 那么里面一定留了一个管道给request去穿，管道低部肯定也有一个阀门(注1)，用来做最后一道拦截工作.
 
在这最底部的阀门里，其实就主要做了两件事:
 
   一是启动过滤器，让request在N个过滤器里面筛一通，如果OK！ 那就PASS。 否则就跳到其他地方去了。
 
   二是servlet.service((HttpServletRequest) request,(HttpServletResponse) response); 这个方法.
 
     如果是JspServlet， 那么先把jsp文件编译成servlet_xxx, 再invoke servlet_xxx的servie()方法。
 
     如果是DefaultServlet， 就直接找到静态资源，取出内容， 发送出去。
 
     如果是InvokerServlet， 就调用那个具体的servlet的service()方法。
 
   ok! 完毕。
 
注1: StandardWrapper 里面的阀门是最后一道关口了。 如果这个阀门欲意把request交给StandardWrapper 的子容器处理。 对不起， 在设计考虑的时候，Wrapper就被考虑成最末的一个容器， 压根儿就不会给Wrapper添加子容器的机会！ 如果硬是要调用addChild(), 立马抛出IllegalArgumentException！
 
参考：
 
     <http://jakarta.apache.org/tomcat/>;
   <http://www.onjava.com/pub/a/onjava/2003/05/14/java_webserver.html>;

http://www.2cto.com/os/201202/119145.html
来源： <[http://blog.csdn.net/haojun186/article/details/7464912](http://blog.csdn.net/haojun186/article/details/7464912)> 
