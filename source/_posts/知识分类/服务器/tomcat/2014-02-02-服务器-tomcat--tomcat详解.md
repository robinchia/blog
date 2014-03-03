---
layout: post
title: "tomcat详解"
categories: 服务器
tags: 
 - 服务器
 - tomcat
--- 

# tomcat详解

**一、Tomcat简介**

** **

**1、Tomcat**

   

Tomcat在严格意义上并不是一个真正的应用服务器，它只是一个可以支持运行Serlvet/JSP的Web容器，不过Tomcat也扩展了一些应用服务器的功能，如JNDI，数据库连接池，用户事务处理等等。Tomcat是Apache组织下Jakarta项目下的一个子项目，目前Tomcat被非常广泛的应用在中小规模的Java Web应用中。

Tomcat 是一种具有JSP环境的Servlet容器。Servlet容器是代替用户管理和调用 Servlet的运行时外壳。作为一个开放源代码的软件， Jakarta -Tomcat有着自己独特的优势：

l         首先，它容易得到。事实上，任何人都可以从互联网上自由地下载这个软件。无论从http://jakarta.Apache.org还是从其他网站（Jakarta Tomcat是Apache软件基金会开发的一个开放源码的应用服务器）。

l         其次，对于开发人员，特别是Java开发人员，Tomcat提供了全部的源代码，包括Servlet引擎、JSP引擎、HTTP服务器。无论是对哪一方面感兴趣的程序员，都可以从这些由世界顶尖的程序员书写的代码中获得收益。

l         最后，由于源代码的开放及世界上许多程序员的卓有成效的工作， Tomcat已经可以和大部分的主流服务器一起工作，而且是以相当高的效率一起工作。如：以模块的形式被载入Apache，以ISAPI形式被载入IIS或PWS，以NSAPI的形式被载入Netscape Enterprise Server。

l         由于Java的跨平台特性，基于Java的Tomcat也具有跨平台性。

** **

** **

**2、Tomcat5.0包含三个主要的部分**

 

包括：
* Catalina - 一个符合Servlet API规范2.3的Servlet Container
* Jasper - 一个符合JSP规范1.2的JSP编译器和运行环境
* Webapps - Tomcat中包含的一些例子和用于测试的web例程，以及相关文档。

** **

** **

**3、应用服务器（如WebLogic）与Tomcat有何区别。**

 

       应用服务器提供更多的J2EE特征，如EJB，JMS，JAAS等，同时也支持Jsp和Servlet。而Tomcat则功能没有那么强大，它不提供EJB等支持。但如果与JBoss（一个开源的应用服务器）集成到一块，则可以实现J2EE的全部功能。

** **

** **

**4、Tomcat 目录的结构**

** **

**（1）Tomcat的安装**

** **

    其实对于完全由Java写成的Tomcat，Win32版本和Linux版本没有多大区别，比如Linux版本，在Solaris下也没有问题。这里，主要以Win32版本作为示例。

注意：在安装使用Tomcat之前，先安装JDK，最好是Sun的JDK 1 .2 以上版。

** **

**（2）Tomcat的目录结构**

 

首先，下载jakarta-tomcat.zip包，解压缩到一个目录下，如：“c:/tomcat”。这时，会得到如下的Tomcat的目录结构：

- - - jakarta - tomcat

| - - - bin             Tomcat执行脚本目录

| - - - Common          放置一些通用类（如JDBC的驱动程序等）

| - - - conf               Tomcat配置文件

| - - - doc                 Tomcat文档

| - - - lib                 Tomcat运行需要的库文件（JARS）

| - - - logs               Tomcat执行时的LOG文件

| - - - src            Tomcat的源代码

| - - - webapps             Tomcat的主要Web发布目录（存放我们自己的JSP,SERVLET,类）

| - - - work            Tomcat的工作目录，Tomcat将翻译JSP文件到的Java文件和class文件放在这里。
**目 录 名**

**该目录内的文件的一般功能描述**bin

包含有Startup.bat（启动服务器）与shutdown.bat（关闭服务器）文件conf

包含设置部署在Tomcat上的Web应用的变量的初始值的设置文件，包括 *server.xml* (Tomcat的全局配置文件) 和 *web.xml* （为不同的Tomcat配置的web应用设置缺省值的文件）doc

包含关于Tomcat的各种各样的文档。common

在其lib目录下，主要存放如JDBC的驱动程序等lib

包含被Tomcat使用的各种各样的jar文件。在UNIX上，任何这个目录中的文件将被附加到Tomcat的classpath中。logs

Tomcat的log文件。src

servlet API的源文件。webapps

包含Web应用的程序 （JSP、Servlet和JavaBean等）work

由Tomcat自动生成，这是Tomcat放置它运行期间的中间(intermediate)文件(诸如编译的JSP文件)地方。 如果当Tomcat运行时，你删除了这个目录那么将不能够执行包含JSP的页面。

** **

**（3）、各个目录下所应该存放的文件：**按照Tomcat的规范，Tomcat的Web应用程序应该由如下目录组成

         页面内容等文件的存放位置：*.html, *.jsp等可以有许多目录层次，由用户的网站结构而定，实现的功能应该是网站的界面，也就是用户主要的可见部分。除了HTML文件、JSP文件外，还有js（JavaScript）文件和css（样式表）文件以及其他多媒体文件等。

 

         Web-INF/web.xml这是一个Web应用程序的描述文件。这个文件是一个XML文件，描述了Servlet和这个Web应用程序的其他组件信息，此外还包括一些初始化信息和安全约束等等。

 

        Web-INF/classes/这个目录及其下的子目录应该包括这个Web应用程序的所有JavaBean及Servlet等编译好的Java类文件（*.class）文件，以及没有被压缩打入JAR包的其他class文件和相关资源。注意，在这个目录下的Java类应该按照其所属的包层次组织目录（即如果该*.class文件具有包的定义，则该*.class文件应该放在./WEB-INF/classes/包名下）。

 

 

 

 

        通常Web-INF/classes/这个目录下的类文件也可以打包成JAR文件,并可以放到WEB-INF下的lib目录下。如将 classes目录下的各个*.class文件打包成WebMis.jar文件（jar cvf WebMis.jar *.*）

 

**注意：**

（1）WEB-INF目录中包含应用软件所使用的资源，但是WEB-INF却不在公共文档根目录之中。在这个目录中所包含的文件都不能被客户机所访问。

（2）类目录中（在WEB-INF下）包含运行Web应用程序时所需的Servlets，Beans等类。

（3）lib目录（在WEB-INF下）包含有Java archive files (JARs)，例如标签库或者Servlets，Beans等类的*.jar文件。

（4）如果一个类出现在JAR文件中同时也出现在类的目录中，类加载器会加载位于类目录中的那一个。

 

         common/lib/ 这个目录下包含了所有压缩到JAR文件中的类文件和相关文件。比如：第三方提供的Java库文件、JDBC驱动程序等。

         其中msbase.jar、mssqlserver.jar、msutil.jar文件为SqlServer2000的JDBC驱动程序

         其中servlet-api.jar和jsp-api.jar为Servlet和JSP的API所在的包

**二、Tomcat的环境配置**

** **

** **

**1、启动Tomcat**

 

在Bin目录下，有一个名为startup.bat的脚本文件，执行这个脚本文件，就可以启动Tomcat服务器，不过，在启动服务器之前，还需要进行一些设置。

l         **首先，设置系统的环境变量。**

         **TOMCAT_HOME（或者：CATALINA_HOME）值：**

d:/jakarta-tomcat-5.0.16 (用TOMCAT_HOME指示Tomcat根目录，下面以Tomcat 5.0.16版为例)。

         **JAVA_HOME值：**

c:/j2sdk1.4.0(用JAVA_HOME指示jdk1.4的安装目录)。

**注意**：对于设置Windows的系统环境变量，可以打开控制面板中的“系统”程序；在“系统环境变量”中增加两个环境变量项目JAVA_HOME（最好为大写）指向JDK的目录和TOMCAT_HOME（最好为大写）指向所安装的tomcat的目录。

 

 

 

**2、启动和关闭Tomcat服务器**

 

（1）启动Tomcat服务器：执行在Bin目录下的名为startup.bat的脚本文件可以启动Tomcat服务器

 

现在可以运行TOMCAT并作为一个独立的Servlet容器。

 

** **

**（2）测试Tomcat的服务器启动与否：**

可以在浏览器中输入[http://127.0.0.1:8080/index.jsp](http://127.0.0.1:8080/index.html)，是否出现如下内容。

 

****

** **

**（3）启动本站点的JSP页面：**在Tomcat中的JSP文件和JavaBean程序的存放位置

         JSP文件放在“Webapps/站点名称”的目录下

        自定义的JavaBean程序*.java文件（可以不需要它）及*.class类文件存放在“Webapps/站点名称/ WEB-INF/classes/”目录下****

因此，将*.jsp文件拷贝到“TOMCAT_HOME/Webapps/站点名称”目录下，然后输入其URL地址

 

**（4）关闭Tomcat服务器：**执行在Bin目录下的名为shutdown.bat的脚本文件可以终止Tomcat服务器。

 

** **

**三、配置Tomcat服务器**

** **

**1、概述**

   

Tomcat为用户提供了一系列的配置文件来帮助用户配置自己的Tomcat，Tomcat的配置文件主要是基于XML的；如server.xml、web.xml等，下面将详细讨论Tomcat的主要配置文件以及如何利用这些配置文件解决常见问题。

** **

**2、server.xml 主配置文件**

 

server.xml是Tomcat的主配置文件，主要完成如下两个目标：

         提供Tomcat组件的初始配置；

         说明Tomcat的结构,含义,使得Tomcat通过实例化组件完成起动及构建自身。

观察**server.xml**，可以发现其中有如下的一些元素。

 

（1）Server元素：

 

Server元素是**server.xml**文件的最高级别的元素， Server元素描述一个Tomcat服务器，一般来说用户不用关心这个元素。一个Server元素一般会包括Logger和ContextManager两个元素

         Logger：Logger元素定义了一个日志对象，一个日志对象包含有如下属性：

1) name：表示这个日志对象的名称。

2) path：表示这个日志对象包含的日志内容要输出到哪一个日志文件。

3) verbosityLevel：表示这个日志文件记录的日志的级别。

一般来说，Logger对象是对Java Servlet、JSP和Tomcat运行期事件的记录

         ContextManager：ContextManager定义了一组ContextInterceptors（ContextManager的事件监听器） , RequestInterceptors（的事件监听器）、Contexts（Web应用程序的上下文目录）和它们的Connectors（连接器）的结构和配置。ContextManager包含如下一些属性：

1) debug：记录日志记录调试信息的等级。

2) home：webapps /、conf /、logs /和所有Context的根目录信息。这个属性的作用是从一个不同于TOMCAT _ HOME的目录启动Tomcat。

3) workDir：Tomcat工作目录。

ContextInterceptor 和RequestInterceptors两者都是监听ContextManager的特定事件的拦截器。ContextInterceptor监听Tomcat的启动和结束事件信息。而RequestInterceptors监听用户对服务器发出的请求信息。一般用户无需关心这些拦截器，对于开发人员需要了解这就是全局性的操作得以实现的方法

** **

**（2）Connector元素：**

 

Connector（连接器）元素描述了一个到用户的连接，不管是直接由Tomcat到用户的浏览器还是通过一个Web服务器。Tomcat的工作进程和由不同的用户建立的连接传来的读/写信息和请求/答复信息都是由连接器对象管理的。对连接器对象的配置中应当包含管理类、TCP/IP端口等内容。****

** **

**（3）Context元素：**

 

每一个Context都描述了一个Tomcat的Web应用程序的目录。这个对象包含以下属性：

1)docBase。这是Context的目录。可以是绝对目录也可以是基于ContextManage的根目录的相对目录。

2)path。这是Context在Web服务时的虚拟目录位置和目录名。

3)debug。日志记录的调试信息记录等级。

4)reloadable。这是为了方便Servlet的开发人员而设置的，当这个属性开关打开的时候，Tomcat将检查Servlet是否被更新而决定是否自动重新载入它

 

3、**配置实例**：打开Tomcat下的conf文件夹下的server.xml文件

 

（1）改变Tomcat服务器的端口号

需要使用Connector 元素，Connector表示一个到用户的联接,不管是通过web服务器或直接到用户浏览器(在一个独立配置中)。Connector负责管理Tomcat的工作线程和读/写连接到不同用户的端口的请求/响应。Connector的配置包含如下信息：句柄类、句柄监听的TCP/IP端口、句柄服务器端口的TCP/IP的backlog。修改后，必须重新启动Tomcat的服务器。

 

 

**注意：**可以将端口号改变为80，单要保证80端口没有被占用；另外，也可以同时分配两个端口号，只要产生两个Connector的配置信息。

**    <!-- Define a non-SSL Coyote HTTP/1.1 Connector on port 8080 -->**

**   <Connector port="8080"    maxThreads="150" minSpareThreads="25" maxSpareThreads="75"**

**     enableLookups="false" redirectPort="8443" acceptCount="100"  debug="0" connectionTimeout="20000"  disableUploadTimeout="true" />**

**              **

**    <!-- Define a non-SSL Coyote HTTP/1.1 Connector on port 8000 -->**

**    <Connector port="8000"    maxThreads="150" minSpareThreads="25" maxSpareThreads="75"**

**enableLookups="false" redirectPort="8443" acceptCount="100"  debug="0" connectionTimeout="20000"  disableUploadTimeout="true" />**

 

（2）增加新的虚拟目录并指向物理目录

 

设立一个虚拟工作目录是比较简单的，只需要在server.xml文件中添加一个Context对象就可以了。如，要在webapps/下增加一个WebMis文件夹以存放jsp页面文件，并且让用户可以使用http://127.0.0.1:8080/WebMis虚拟目录访问，则：需要使用Context 元素，每个Context提供一个指向你放置你Web项目的Tomcat的下属目录。每个Context包含如下配置：  

l         Context放置的路径，可以是与ContextManager主目录相关的路径；

l         纪录调试信息的调试级别；

l         可重载的标志，开发Servlet时，重载更改后的Servlet。这是一个非常便利的特性,你可以调试或用Tomcat测试新代码而不用停止或重新启动Tomcat。要打开重载,把reloadable设为true即可。

 

其中：path="/WebMis"说明其相对web URL的路径，是一个虚拟的路径，如：[http://127.0.0.1:8080/WebMis](http://127.0.0.1:8080/WebMis)，docBase="WebMis"说明其相对webapps的位置，是物理存在的目录，同时需要在webapps/下增加一个WebMis物理文件夹。

 

 

 

 

（3）       加入自己的日志文件

 

添加Logger对象就可以加入自己的日志文件，添加工作相当简单，只需要将作为示例的Logger对象复制一份，然后修改一下前面介绍的几个属性就可以了。在设定了Logger以后，就可以在自己的Servlet中使用ServletContext.log()方法来建立自己的日志文件。

 

4、**配置实例**：打开conf文件夹下的web.xml文件

 

 

（1）web.xml文件：它包含了描述整个Web应用程序（Web应用程序由一整套Web文件jsp、servlet、html、jpg、gif、class等组成）的信息。下面以一个web.xml文件为例，讲解里面的各个对象。

 

<!DOCTYPE web-app PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.2//EN"

"http://java.sun.com/j2ee/dtds/web-app_2_2.dtd">

<web-app>

<display-name>My Web Application</display-name>

<description>在这里加入Web应用程序的描述信息</description>

<!-

下面定义了Web应用程序的初始化参数，在JSP或Servlet文件中使用下面的语句

来得到初始化参数

String value =

getServletContext().getInitParameter("name");

这里可以定义任意多的初始化参数

-->

<context-param>

<param-name>webmaster</param-name>

<param-value>myaddress@mycompany.com</param-value>

<description>这里包含了初始化参数的描述</description>

</context-param>

<!-

下面的定义描述了组成这个Web应用程序的Servlet，还包含初始化参数。在Tomcat中，也可以将放在Web-INF/classes中的Servlet直接以servlet/Servlet名访问，但是一般来说，不推荐这样使用。而且这样的使用方法还会导致Servlet的相关资源组织的复杂性。所以一般来说推荐将所有的Servlet在这里定义出来。初始化参数可以在Servlet中使用如下语句来获得：

String value =getServletConfig().getInitParameter("name");

-->

<servlet>

<servlet-name>controller</servlet-name>

<description>这里加入这个Servlet的描述</description>

<servlet-class>com.mycompany.mypackage.ControllerServlet</servlet-class>

**<init-param>**

**<param-name>listOrders</paramName>**

**<param-value>com.mycompany.myactions.ListOrdersAction</param-value>**

**</init-param>**

<init-param>

<param-name>saveCustomer</paramName>

<param-value>com.mycompany.myactions.SaveCustomerAction</param-value>

</init-param>

<!-

服务器启动后这个Servlet加载的时间

-->

<load-on-startup>5</load-on-startup>

</servlet>

<servlet>

<servlet-name>graph</servlet-name>

<description>这个Servlet的描述</description>

</servlet>

<!-

Servlet映射对应了一个特殊的URI请求到一个特殊的Servlet的关系

-->

<servlet-mapping>

<servlet-name>controller</servlet-name>

<url-pattern>*.do</url-pattern>

</servlet-mapping>

<servlet-mapping>

<servlet-name>graph</servlet-name>

<url-pattern>/graph</url-pattern>

</servlet-mapping>

<!-

设定缺省的Session过期时间（单位为分）

-->

<session-config>

<session-timeout>30</session-timeout>

</session-config>

</web-app>****

**（2）配置实例：会话(session)超时修改，**修改conf/web.xml中的如下数据值（单位为分）

 

**5、在Tomcat中实现利用JDBC驱动程序访问SQLServer2000数据库**

 

只需要将SQLServer2000的JDBC驱动程序的三个*.jar（msbase.jar、mssqlserver.jar和msutil.jar）文件放在/common/lib目录下，然后在*.java程序中访问它。

 

**四、在Tomcat5中配置连接池和数据源**

** **

**1、DataSource接口介绍**

** **

**（1）DataSource 概述**

JDBC1.0原来是用DriverManager类来产生一个对数据源的连接。JDBC2.0用一种替代的方法，使用DataSource的实现，代码变的更小巧精致，也更容易控制。

一个DataSource对象代表了一个真正的数据源。根据DataSource的实现方法，数据源既可以是从关系数据库，也电子表格，还可以是一个表格形式的文件。当一个DataSource对象注册到名字服务中（JNDI），应用程序就可以通过名字服务获得DataSource对象，并用它来产生一个与DataSource代表的数据源之间的连接。

javax.sql包中的DataSource接口，可以采用三种实现形式：简单的实现（只提供Connection对象）、连接池形式的实现和分布式事务形式的实现。

javax.sql包中的ConnectionPoolDataSource提供对连接池实现的接口。

**（2）使用DataSource的优点**

l         DataSource与DriverManager的不同

关于数据源的信息和如何来定位数据源，例如数据库服务器的名字，在哪台机器上，端口号等等，都包含在DataSource对象的属性里面去了。这样，对应用程序的设计来说是更方便了，因为并不需要硬性的把驱动的名字写死到程序里面去。通常驱动名字中都包含了驱动提供商的名字，而在DriverManager类中通常是这么做的。

l         可移植性

如果数据源要移植到另一个数据库驱动中，代码也很容易做修改。所需要做的修改只是更改DataSource的相关的属性。而使用DataSource对象的代码不需要做任何改动。

**（3）配置DataSource**

主要包括设定DataSource的属性，然后将它注册到JNDI名字服务中去。在注册DataSource对象的的过程中，系统管理员需要把DataSource对象和一个逻辑名字关联起来。名字可以是任意的，通常取成能代表数据源并且容易记住的名字。

在下面的例子中，名字起为：WebMisDB，按照惯例，逻辑名字通常都在jdbc的子上下文中。这样，逻辑名字的全名就是：jdbc/WebMisDB。

**（4）产生一个与数据源的连接**

一旦配置好了数据源对象，应用程序设计者就可以用它来产生一个与数据源的连接。下面的代码片段示例了如何用JNDI上下文获得一个数据源对象，然后如何用数据源对象产生一个与数据源的连接。开始的两行用的是JNDI API，第三行用的才是JDBC的API： 
Context ctx = new InitialContext(); 

DataSource ds = (DataSource)ctx.lookup("jdbc/WebMisDB");

Connection con = ds.getConnection("myPassword", "myUserName"); 
在一个基本的DataSource实现中，DataSource.getConnection方法返回的Connection对象和用DriverManager.getConnection方法返回的Connection对象是一样的。因为DataSource提供的方便性，我们推荐使用DataSource对象来得到一个Connection对象。

**（5）DataSource的应用场合**

对于普通的应用程序设计者，是否使用DataSource对象只是一个选择问题。但是，对于那些需要用的连接池或者分布式的事务的应用程序设计者来说，就必须使用DataSource对象来获得Connection。需要注意的是对Tomcat而言，在JNDI的名称前面应该加上"java:comp/env/" 

**（6）数据源（DataSource）的作用**

它相当于客户端程序和连接池的中介，想要获得连接池中的连接对象，必须建立一个与该连接池相应的数据源，然后通过该数据源获得连接。

**2、JNDI（**JAVA NAMING AND DIRECTORY INTERFACE---Java 命名和目录接口**）**

**（1）**JNDI简介****

分布式计算环境通常使用命名和目录服务来获取共享的组件和资源。命名和目录服务将名称与位置、服务、信息和资源关联起来。它是一个为JAVA应用程序提供命名服务的应用程序编程接口（API）。

命名服务提供了一种为对象命名的机制，这样你就可以在无需知道对象位置的情况下获取和使用对象。只要该对象在命名服务器上注册过，且你必须知道命名服务器的地址和该对象在命名服务器上注册的JNDI名。就可以找到该对象，获得其引用，从而运用它提供的服务。

命名服务提供名称—对象的映射。目录服务提供有关对象的信息，并提供定位这些对象所需的搜索工具。

Java 命名和目录接口或 JNDI 提供了一个用于访问不同的命名和目录服务的公共接口（JAVA API）。运用一个命名服务来查找与一个特定名字相关的一个对象，JDBC可以用JNDI来访问一个关系数据库。

**（2）获得JNDI的初始环境**

在JNDI中，在目录结构中的每一个结点称为Context 。每一个JNDI名字都是相对于Context 的。这里没有绝对名字的概念存在。对一个应用来说，它可以通过使用InitialContext 类来得到其第一个Context：

Context  ctx = new InitialContext ();

应用可以通过这个初始化的Context经由这个目录树来定位它所需要的资源或对象。InitialContext在网页应用程序初始化时被设置，用来支持网页应用程序组件。所有的入口和资源都放在JNDI命名空间里的java:comp/env段里。

**（3）查找已绑定的对象**

用ctx..lookup(String name); 根据name找对象

例：

import javax.naming.*;

public class TestJNDI

{    

public static void main(String[] args)

{

        try

{

        Context ctx=new InitialContext();

        Object object=ctx.lookup(“JNDIName”);       //根据JNDI名查找绑定的对象

        String str=(String) object;                                 //强制转换

        }

catch(NamingException e)

{    e.printStackTrace();

        }

catch(ClassCastException e)

{    e.printStackTrace();

        }

   }

}****

** **

**3、数据库连接池技术**

** **

**（1）**传统的Web数据库编程模式

 

l         在主程序（如Servlet、Beans）中建立数据库连接。

l         进行SQL操作，取出数据。

l         断开数据库连接。

使用这种模式开发，存在很多问题。

l         首先，我们要为每一次WEB请求（例如察看某一篇文章的内容）建立一次数据库连接，对于一次或几次操作来讲，或许你觉察不到系统的开销，但是，对于WEB程序来讲，即使在某一较短的时间段内，其操作请求数也远远不是一两次，而是数十上百次（想想全世界的网友都有可能在您的网页上查找资料），在这种情况下，系统开销是相当大的。事实上，在一个基于数据库的WEB系统中，建立数据库连接的操作将是系统中代价最大的操作之一。很多时候，可能您的网站速度瓶颈就在于此。

l         其次，使用传统的模式，你必须去管理每一个连接，确保他们能被正确关闭，如果出现程序异常而导致某些连接未能关闭，将导致数据库系统中的内存泄露，最终我们将不得不重启数据库。****

l         频繁的建立、关闭连接，会极大的减低系统的性能，因为对于连接的使用成了系统性能的瓶颈。****

** **

**（2）数据库连接是一种关键的有限的昂贵的资源**

 

这一点在多用户的网页应用程序中体现得尤为突出。对数据库连接的管理能显著影响到整个应用程序的伸缩性和健壮性，影响到程序的性能指标。数据库连接池正是针对这个问题提出来的。

连接池是这么一种机制，当应用程序关闭一个Connection的时候，这个连接被回收，而不是被destroy，因为建立一个连接是一个很费资源的操作。如果能把回收的连接重新利用，会减少新创建连接的数目，显著的提高运行的性能。该策略的核心思想是：连接复用。

通过采用连接池的方法，服务器在启动时先打开一定数量的连接。当应用需要连接时，就可以从服务器请求一个连接。当应用结束该连接时，服务器就把它释放到连接池，以备其他客户机使用。
客户获得连接并访问数据库以后结束客户获得连接并访问数据库以后结束客户获得连接并访问数据库以后结束开始停止服务吗

？产生新的连接等待引入的连接结束是服务器监听客户的连接请求                    客户获得连接

 

**（3）连接池的主要作用**

l         减少了建立和释放数据库连接的消耗

l         数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而再不是重新建立一个；

l         释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏。这项技术能明显提高对数据库操作的性能。

l         封装用户信息  使用连接池可以封装连接数据库系统所用的用户信息（帐号和密码），这样客户端程序在建立连接时不用考虑安全信息。

**（4）数据库连接池的工作原理**

       当程序中需要建立数据库连接时，只须从内存中取一个来用而不用新建。同样，使用完毕后，只需放回内存即可。而连接的建立、断开都有连接池自身来管理。同时，我们还可以通过设置连接池的参数来控制连接池中的连接数、每个连接的最大使用次数等等

 

 

 

**（5）数据库连接池的最小连接数和最大连接数**

数据库连接池的最小连接数和最大连接数的设置要考虑到下列几个因素：

l         最小连接数是连接池一直保持的数据库连接，所以如果应用程序对数据库连接的使用量不大，将会有大量的数据库连接资源被浪费；

l         最大连接数是连接池能申请的最大连接数，如果数据库连接请求超过此数，后面的数据库连接请求将被加入到等待队列中，这会影响之后的数据库操作。

如果最小连接数与最大连接数相差太大，那么最先的连接请求将会获利，之后超过最小连接数量的连接请求等价于建立一个新的数据库连接。不过，这些大于最小连接数的数据库连接在使用完不会马上被释放，它将被放到连接池中等待重复使用或是空闲超时后被释放。

**（6）使用连接池得到连接**
假设应用程序需要建立到一个名字为EmpolyeeDB的DataSource的连接。使用连接池得到连接的代码如下： 
Context ctx = new InitialContext(); 

DataSource ds = (DataSource)ctx.lookup("jdbc/EmployeeDB");

Connection con = ds.getConnection("myPassword", "myUserName");

或者：

Context ctx = new InitialContext(); 

ConnectionPoolDataSource ds = (ConnectionPoolDataSource)ctx.lookup("jdbc/EmployeeDB");

PooledConnection con = ds.getConnection("myPassword", "myUserName");

是否使用连接池获得一个连接，在应用程序的代码上是看不出不同的。在使用这个Connection连接上也没有什么不一样的地方，唯一的不同是在java的finally语句块中来关闭一个连接。在finally中关闭连接是一个好的编程习惯。这样，即使方法抛出异常，Connection也会被关闭并回收到连接池中去。代码应该如下所示： 
try

{… 
}

catch（）

{… 
}

finally

{ 

if（con!=null）

con.close();

}****

** **

**4、在Tomcat中配置数据库的连接池**

** **

**（1）连接池配置(Database Connection Pool (DBCP) Configurations)**

DBCP使用的是Jakarta-Commons Database Connection Pool 要使用连接池需要如下的组件即jar文件。

l         Jakarta-Commons DBCP 1.1 对应commons-dbcp-1.1.jar。

l         Jakarta-Commons Collections 2.0 对应commons-collections.jar。

l         Jakarta-Commons Pool 1.1 对应commons-pool-1.1.jar。

这三个jar文件要与你的JDBC驱动程序一起放到【TOMCAT_HOME】/common/lib目录下以便让tomcat和你的web应用都能够找到。

****

**注：**

l         这三个jar文件是默认存在与【TOMCAT_HOME】/common/lib下的。

l         需要注意的地方：第三方的驱动程序或者其他类只能以*.jar的形式放到Tomcat的common/lib目录中,因为Tomcat只把*.jar文件加到CLASSPATH中。

l         不要把上诉三个文件放到WEB-INF/lib或者其他地方因为这样会引起混淆。****

** **

**（2）通过配置阻止连接池漏洞**

数据库连接池创建和管理连接池中建立好的数据库连接，循环使用这些连接以得到更好的效率。这样比始终为一个用户保持一个连接和为用户的请求频繁的建立和销毁数据库连接要高效的多。

这样就有一个问题出现了，一个Web应用程序必须显示的释放ResultSet，Statement和Connection。如果在关闭这些资源的过程中失败将导致这些资源永远不在可用，这就是所谓的连接池漏洞。这个漏洞最终会导致连接池中所有的连接不可用。

通过配置Jakarta Common DBCP可以跟踪和恢复那些被遗弃的数据库连接。

以下是一系列相关配置：

l         通过配置DBCP数据源中的参数removeAbandoned来保证删除被遗弃的连接使其可以被重新利用。

为ResourceParams(见下文的数据源配置)标签添加参数removeAbandoned

<parameter>

<name>removeAbandoned</name>

<value>true</value>

</parameter>

通过这样配置的以后当连接池中的有效连接接近用完时DBCP将试图恢复和重用被遗弃的连接。这个参数的值默认是false。

l         通过设置removeAbandonedTimeout来设置被遗弃的连接的超时的时间，即当一个连接连接被遗弃的时间超过设置的时间时那么它会自动转换成可利用的连接。

    <parameter>

     <name>removeAbandonedTimeout</name>

     <value>60</value>

     </parameter>

    默认的超时时间是300秒。

l         设置logAbandoned参数，以将被遗弃的数据库连接的回收记入日志中

<parameter>

<name>logAbandoned</name>

<value>true</value>

</parameter>

这个参数默认为false。****

**（3）修改server.xml文件**

       <Context path="/WebMis" docBase="WebMis" debug="0" reloadable="true">

                    

                   <Resource name="jdbc/webmis" auth="Container" type="javax.sql.DataSource"/>  

                   <ResourceParams** name="jdbc/webmis"**>      

                                 <parameter>             

                                         <name>

                                                factory

                                         </name>        

                                         <value>org.apache.commons.dbcp.BasicDataSourceFactory

                                         </value>        

                              </parameter>

                              <parameter>             

                                       <name>

                                              driverClassName

                                       </name>        

                                         <value>**com.microsoft.jdbc.sqlserver.SQLServerDriver**

                                         </value>        

                                  </parameter>

                                 <parameter>      

                                         <name>

                                                url

                                         </name>        

                               <value>**jdbc:microsoft:sqlserver://127.0.0.1:1433;DatabaseName=DataBase**

                              </value>

                                 </parameter>            

                                 <parameter>

                                            <name>

                                                   username

                                             </name>             

                                         <value>

                                              **sa**

                                         </value>

                                </parameter>            

                                 <parameter>

                                            <name>

                                                   password

                                            </name>

                                            <value>

maxActive 连接池的最大数据库连接数。设为0表示无限制。                                            </value> 

                                 </parameter>            

                                 <parameter>

 

                                            <name>

                                                   maxActive

                                            </name>

                                            <value>

                                                   20

回收被遗弃的（一般是忘了释放的）数据库连接到连接池中，设为－1表示无限制。maxIdle  数据库连接的最大空闲时间。超过此空闲时间，数据库连接将被标记为不可用，然后被释放。设为0表示无限制。                                            </value>

                                 </parameter>

                                 <parameter>             

                                        <name>

                                                 maxIdle

                                          </name>

maxWait 最大建立连接等待时间。如果超过此时间将接到异常。设为－1表示无限制。                                        <value>10</value>

                                </parameter>

                                <parameter>

                                            <name>maxWait</name>

                                            <value>-1</value>

                              </parameter> 

<parameter>
  <name>removeAbandoned</name>
  <!-- Abandoned DB connections are removed and recycled -->
  <value>true</value>
 </parameter>
 <parameter>
  <name>removeAbandonedTimeout</name>
  <!-- Use the removeAbandonedTimeout parameter to set the number of seconds a DB connection has been idle before it is considered abandoned.  -->
  <value>60</value>
 </parameter>
 <parameter>
  <name>logAbandoned</name>
  <!-- Log a stack trace of the code which abandoned -->
  <value>false</value>
 </parameter>    

数据库连接过多长时间不用将被视为被遗弃而收回连接池中将被遗弃的数据库连接的回收记入日志                   </ResourceParams>

              </Context>

注意：

l         所有的入口和资源都放在JNDI命名空间里的java:comp/env段里

l         设置JNDI资源要在$CATALINA_HOME/conf/server.xml文件里使用下列标志符：
1) <Resource>--设置应用程序可用的资源的名字和类型（同上面说的<resource-ref>等价）。
2) <ResourceParams>--设置Java资源类工厂的名称或将用的JavaBean属性。
上述这些标志符必须放在<Context>和</Context>之间

** **

**（2）、拷贝SQLServer的JDBC驱动程序到Tomcat的/common/lib目录下**

** **

**（3）、在程序中利用数据源来访问数据库**

              try

              {

                     Context initCtx = new InitialContext();

                   Context envCtx = (Context) initCtx.lookup("java:comp/env");

                DataSource ds = (DataSource)envCtx.lookup("jdbc/webmis");

                 Connection con=ds.getConnection();

          }

        catch (NamingException e)

           {

                   e.printStackTrace();

            }

            catch (SQLException e)

           {

                   e.printStackTrace();

             }

** **

**5、在server.xml文件中与数据源的描述相关的标签含义**

l         maxActive 连接池的最大数据库连接数。设为0表示无限制。****

l         maxIdle  数据库连接的最大空闲时间。超过此空闲时间，数据库连接将被标记为不可用，然后被释放。设为0表示无限制。****

l         maxWait 最大建立连接等待时间。如果超过此时间将接到异常。设为－1表示无限制。****

l         removeAbandoned 回收被遗弃的（一般是忘了释放的）数据库连接到连接池中。****

l         removeAbandonedTimeout 数据库连接过多长时间不用将被视为被遗弃而收回连接池中。****

l         logAbandoned 将被遗弃的数据库连接的回收记入日志。****

l         driverClassName JDBC驱动程序。****

l         url   数据库DSN连接字符串****

**6****、在Web应用的web.xml文件中引用该资源**

 

将下面的标签放在放在<web-app>和</web-app>中间

**<!-- Database Config start -->**

**<resource-ref>**

**<description>connectDB test</description>**

**<res-ref-name>jdbc/webmis</res-ref-name>**

**<res-type>javax.sql.DataSource</res-type>**

**<res-auth>Container</res-auth>**

**</resource-ref>**

**<!-- Database Config end -->**

 

**4，综合配置实例**

 

首先在C:根目录下建立文件夹mywebapp，作为一个虚拟目录的位置。

 

建立一个Sql Server数据库DataBonus

 

找到C:/jakarta-tomcat-5.0.19/conf/server.xml，打开。

 

加入：

 

 <Context path="/mywebapp" docBase="C:/mywebapp" debug="0" reloadable="true">

                    

       <Resource name="jdbc/mybonusds" auth="Container" type="javax.sql.DataSource"/>  

<ResourceParams name="jdbc/mybonusds">     

              <parameter>         

                 <name>factory</name>         

                 <value>org.apache.commons.dbcp.BasicDataSourceFactory</value>        

              </parameter>

              <parameter>         

                 <name>driverClassName</name>         

                 <value>com.microsoft.jdbc.sqlserver.SQLServerDriver</value>        

              </parameter>

              <parameter>  

                  <name>url</name>             

                  <value>

jdbc:microsoft:sqlserver://127.0.0.1:1433;DatabaseName=DataBonus

                   </value>

               </parameter>             

               <parameter>

                 <name>username</name>            

                 <value>sa</value>

                </parameter>            

                <parameter>

                   <name>password</name>

                   <value></value>  

                </parameter>            

                <parameter>

<name>maxActive</name>

                    <value>20</value>

                </parameter>

                <parameter>             

                   <name>maxIdle</name>

                   <value>10</value>

                </parameter>

                <parameter>      

                   <name>maxWait</name>

                   <value>-1</value>

                </parameter>     

<parameter>

             <name>removeAbandoned</name>

             <!-- Abandoned DB connections are removed and recycled -->

             <value>true</value>

          </parameter>

          <parameter>

              <name>removeAbandonedTimeout</name>

  <!-- Use the removeAbandonedTimeout parameter to set the number of seconds a DB connection has been idle before it is considered abandoned.  -->

              <value>60</value>

           </parameter>

           <parameter>

              <name>logAbandoned</name>

            <!-- Log a stack trace of the code which abandoned -->

              <value>false</value>

           </parameter>   

         </ResourceParams>

</Context>

 

   做一个JSP页面index.jsp放到mywebapp下面，代码：

 

<%--字符集设为"gb2312",使动态页面支持中文--%>

<%@ page contentType="text/html; charset=GB2312"%>

 

<!-- 这里使用一个字串变量 ("PAGETITLE") 保持题目和主标题的一致性。-->

<html>

<head>

<title>

<%= pagetitle %>

</title>

</head>

 

<body bgcolor=#FFFFFF>

 

<font face="Helvetica">

 

<h2>

<font color=#DB1260>

<%= pagetitle %>

</font>

</h2>

 

<!-- 导入必要的类和类库 -->

 

<%@ page import="

    javax.naming.*,

    java.sql.*,

    javax.sql.DataSource

"%>

 

<!-- 声明一个类方法 -->

 

<%!

//声明变量

//标题

  String pagetitle = "这是JSP调用数据库的例子";

 

%>

 

<!-- 下面这些代码将被插入到servlet中 -->

 

<%

 

   java.sql.Connection conn= null;

   java.sql.Statement stmt =null;

   java.sql.ResultSet rs=null;

 

  try {

    // 通过JNDI获取主接口

 

     Context initCtx = new InitialContext();

     Context envCtx = (Context) initCtx.lookup("java:comp/env");

     DataSource ds = (DataSource)envCtx.lookup("jdbc/mybonusds");

     conn=ds.getConnection();

 

      stmt = conn.createStatement();

 

   

           //执行SQL语句

      stmt.execute("select * from 奖金");

    //取得结果集

      rs = stmt.getResultSet();

     

    %>

 

  <table border="1">

   <tr>

      <td width="60" height="20"><% out.print("编号"); %></td>

      <td width="80" height="20"><% out.print("姓名"); %></td>

      <td width="200" height="20"><% out.print("发奖名称"); %></td>

      <td width="100" height="20"><% out.print("金额"); %></td>

      <td width="200" height="20"><% out.print("备注"); %></td>

   </tr>

  <%   while (rs.next()) {

 

  %>

    <tr>

      <td width="60" height="20"><% out.print(rs.getString("编号")); %></td>

      <td width="80" height="20"><% out.print(rs.getString("姓名")); %></td>

      <td width="200" height="20"><% out.print(rs.getString("发奖名称")); %></td>

      <td width="100" height="20"><% out.print(rs.getString("金额")); %></td>

      <td width="200" height="20"><% out.print(rs.getString("备注")); %></td>

   </tr>

     <%  } %>

  </table>

 

 

<%

 // Catch exceptions

  }

 

  catch (Exception e) {

  }

  finally {

 

 if (rs != null)

     {

      try{rs.close();}catch(Exception ignore){};

      }

 

     if (stmt != null)

     {

      try{stmt.close();}catch(Exception ignore){};

      }

     if (conn != null)

     {

      try{conn.close();}catch(Exception ignore){};

      }

 

 

 %>

 

 

<%

  }

%>

 

</font>

</body>

</html>

 

启动Tomcat。

 

浏览：http://127.0.0.1:8080/mywebapp/index.jsp

 

 

 

 

**四、在Tomcat中实现系统和Web管理的配置**

** **

**1、配置系统管理（Admin Web Application）**

** **

**（1）概述**

大多数商业化的J2EE服务器都提供一个功能强大的管理界面（如Weblogic的管理控制台），且大都采用易于理解的Web应用界面。Tomcat按照自己的方式，同样提供一个成熟的管理工具，并且丝毫不逊于那些商业化的竞争对手。

Tomcat的Admin Web Application最初在4.1版本时出现，当时的功能包括管理context、data source、user和group等。当然也可以管理像初始化参数，user、group、role的多种数据库管理等。在后续的版本中，这些功能将得到很大的扩展，但现有的功能已经非常实用了。

** **

**（2）系统管理Web应用程序**

Tomcat中的Admin Web Application被定义在自动部署文件：C:/jakarta-tomcat-5.0.19/server/webapps/admin/ admin.xml 中（请见下图所示）。

      

** **

**（3）编辑admin.xml文件**

 

通过编辑admin.xml文件，以确定Context中的docBase参数设置为Admin Web Application所在的目录路径（应该是绝对路径）。作为另外一种选择，你也可以删除这个自动部署文件，而在C:/jakarta-tomcat-5.0.19/conf/server.xml文件中建立一个Admin Web Application的context，效果是一样的。

       你不能管理Admin Web Application这个应用，换而言之，除了删除CATALINA_BASE/webapps/admin.xml ，你可能什么都做不了。

 

**注意：**如果将其中的被注释掉的<Valve className="org.apache.catalina.valves.RemoteAddrValve"

    allow="127.0.0.1"/>打开，将能够限制访问Admin Web Application的程序主机为本机（服务器主机）；当然也可以设置为其它的主机IP地址（如设置为 Web管理员所的工作主机）。

** **

**（4）在C:/jakarta-tomcat-5.0.19/conf/ tomcat-users.xml 文件中添加系统管理员的角色和系统管理员**

 

Tomcat中提供UserDatabaseRealm（默认），这样我们可以根据管理的需要添加不同的用户角色和与该角色相配置的用户名称和密码

 

l         添加用户角色

<role name="admin"/>

l         添加与该角色相配置的用户名称和密码

<user name="admin" password="12345678" roles="admin"/>

当你完成这些步骤后，请重新启动Tomcat，访问http://localhost:8080/admin，你将看到一个登录界面。Admin Web Application程序采用基于容器管理的安全机制，并采用了Jakarta Struts框架。下面是在原来的tomcat-users.xml 文件中再添加了两个角色admin和manager，同时也添加了与该两个角色相配置的用户admin和manager。

<?xml version='1.0' encoding='utf-8'?>

<tomcat-users>

  <role rolename="role1"/>

  <role rolename="tomcat"/>

**  <role rolename="admin"/>**

**  <role rolename="manager"/>**

  **<user username="admin" password="12345678" roles="admin"/>**

**  <user name="manager" password="12345678" roles="manager"/>**

  <user username="role1" password="tomcat" roles="role1"/>

  <user username="tomcat" password="tomcat" roles="tomcat"/>

  <user username="both" password="tomcat" roles="tomcat,role1"/>

</tomcat-users>

**（5）登录Admin Web Application程序**

输入[http://localhost:8080/admin/](http://localhost:8080/admin/)进入系统管理员的登录页，然后在页中

输入用户名称：admin

密码： 12345678

 

       将进入系统管理的界面，在该系统管理的程序中将可以配置各种资源如Data Sources、Mail Sessions、Environment Entries，并且也可以管理Users 和Groups 以及Roles等功能。

 

** **

**2、配置应用管理（Manager Web Application）**

** **

**（1）概述**

 

Tomcat中所提供的Manager Web Application让你通过一个比Admin Web Application更为简单的用户界面，执行一些与Web应用任务相关的一些管理功能。

** **

**（2）Manager Web Application程序**

 

Manager Web Application被被定义在一个自动部署文件中C:/jakarta-tomcat-5.0.19/server/webapps/manager/manager.xml 。

 

** **

**（3）编辑manager.xml文件**

 

通过编辑这个文件，以确保其中的context中的docBase属性参数是C:/jakarta-tomcat-5.0.19/server/webapps/manager的绝对路径。

 

** **

**（4）在C:/jakarta-tomcat-5.0.19/conf/ tomcat-users.xml 文件中添加Web管理员的角色和Web管理员**

l         添加用户角色

**<role name=" manager "/>**

l         添加与该角色相配置的用户名称和密码

**  <user name="manager" password="12345678" roles="manager"/>**

** **

**（5）登录Web管理员的页面**

l         文本型管理界面

然后重新启动Tomcat，输入[http://localhost:8080/manager/](http://localhost:8080/manager/)，将进入看到一个很朴素的文本型管理界面

 

如果输入[http://localhost:8080/manager/](http://localhost:8080/manager/)list，将进入一个登录管理界面，然后

输入用户名称：manager（前面在tomcat-users.xml中设置的）

密码：12345678

 

将显示出

 

l         HTML 型管理界面

输入[http://localhost:8080/manager/html/list](http://localhost:8080/manager/html/list)，将出现如下的页面，然后再

输入用户名称：manager

密码：12345678

 

将出现Web方式的管理页面

 

Manager application可以让用户在没有系统管理特权的基础上，部署安装新的Web应用，以用于测试。同时也可以对所部署的Web应用程序的工作状态进行控制（Start 或者 Stop），以免重新启动服务器（这在对web.xml等配置的内容发生改变的情况下，特别有效）。当有用户尝试访问这个被停止的应用时，将看到一个503的错误——“503 - This application is not currently available”。

 

** **

**3、配置各种用户角色、用户组和用户**

 

（1）添加用户角色：在 admin的界面中点击左面的Roles节点，然后在右面的下拉列表框中选择Create New Role项目。

 

然后输入角色的名称和描述

 

       最后点击“保存”，将存储在C:/jakarta-tomcat-5.0.19/conf/tomcat-users.xml文件中并且在管理界面中显示出。

 

 

**（2）添加用户组：**在 admin的界面中点击左面的Groups节点，然后在右面的下拉列表框中选择Create New Group项目。

****

然后输入组的名称和描述，并且设置该组的角色。所应该注意的是，给组分配角色，则意味着该组中的各个成员（用户）将具有该角色所分配的各种权限。

****

最后点击“Save”以保存它（仍然放在C:/jakarta-tomcat-5.0.19/conf/tomcat-users.xml文件中）

 

** **

**（3）添加属于某一用户组内的用户**

在 admin的界面中点击左面的Users节点，然后在右面的下拉列表框中选择Create New User项目。

 

然后该用户的名称同时包括全名称、密谋，并且设置该用户所属的用户组；同时也可以为该用户再设置其它的角色以使该用户除了具有用户组的通用的权利以外，还具有其他方面的权利。

下面对“teacherZhang”这个用户进行设置，同时他也是系统管理员，因此将下面的admin的角色也选中。

 

最后点击保存（仍然放在C:/jakarta-tomcat-5.0.19/conf/tomcat-users.xml文件中）

 

 

** **

**4、添加其它的系统资源**

** **

**（1）DataSource**

       在 admin的界面中点击左面的DataSourcs节点，然后在右面的下拉列表框中选择Create New DataSource项目。

       在各个输入的项目中根据数据库的特性进行输入。最后点击“Save”以保存。

 

** **

**（2）添加环境变量**

 

       在 admin的界面中点击左面的Environment Entries节点，然后在右面的下拉列表框中选择Create New Env Entry项目。

       在各个输入的项目中根据数据库的特性进行输入。最后点击“Save”以保存。****

 

 

** **

**5、对Web应用程序进行管理**

 

（1）输入[http://localhost:8080/manager/html/list](http://localhost:8080/manager/html/list)，将出现登录页并且进行登录，然后再进入Tomcat Web Application Manager

（2）查看在Web服务中所发布的各个Web应用

 

** **

**（3）启动或者终止、移除某一Web应用：**

 

点击该 Web应用右面的Stop链接，也可以点击Start再次启动它。Undeploy（移除）一个Web应用，只是指从Tomcat的运行拷贝中删除了该应用，如果你重新启动Tomcat，被删除的应用将再次出现（也就是说，移除并不是指从硬盘上删除）。

** **

**（4）部署某一Web应用**

 

有三种方式可以在Tomcat系统中部署Web应用。

l         直接拷贝你的WAR文件或者你的Web应用文件夹（包括该Web应用的所有内容）到C:/jakarta-tomcat-5.0.19/webapps目录下。

该文件必须以“.war”作为扩展名。一旦Tomcat监听到这个文件，它将（缺省的）解开该文件包作为一个子目录，并以WAR文件的文件名作为子目录的名字。接下来，Tomcat将在内存中建立一个context，就好象你在server.xml文件里建立一样。当然，其他必需的内容，将从server.xml中的DefaultContext获得。

 

l         部署web应用的另一种方式是写一个Context XML片断文件，然后把该文件拷贝到C:/jakarta-tomcat-5.0.19/webapps目录下。

一个Context片断并非一个完整的XML文件，而只是一个Context元素，以及对该应用的相应描述。这种片断文件就像是从server.xml中切取出来的context元素一样，所以这种片断被命名为“context片断”。这个web应用本身可以存储在硬盘上的任何地方。

 

举个例子，如果我们想部署一个名叫JspExamples的Web应用，该应用使用realm作为访问控制方式，我们可以使用下面这个片断：

<!-- 

 Context fragment for deploying JspExamples 

-->

<Context path="/JspExamples" docBase="JspExamples" debug="0" reloadable="true">

<RealmclassName="org.apache.catalina.realm.UserDatabaseRealm"  resourceName="UserDatabase"/>

</Context>

把该片断命名为“JspExamples.xml”，然后拷贝到C:/jakarta-tomcat-5.0.19/webapps目录下。这种Context片断提供了一种便利的方法来部署web应用，你不需要编辑server.xml，除非你想改变缺省的部署特性，安装一个新的Web应用时不需要重启动Tomcat。

 

l         采用GUI管理界面进行发布

如果提供了该Web应用的*.war文件，直接浏览并发布它

 

如果Web应用是以目录形式存在的，则可以：

 

 

 

**五、Tomcat服务器的Web安全的解决方法**

** **

**1、概述**

 

在任何一种WEB应用开发中，不论大中小规模的，每个开发者都会遇到一些需要保护程序数据的问题，涉及到用户的LOGIN ID和PASSWORD。那么如何执行验证方式更好呢？实际上，有很多方式来实现。

下面将讨论在Tomcat中实现基本的（BASIC）和基于表单的（FORM-BASED）验证方式。它通过server.xml和web.xml文件提供基本的和基于表单的验证。

对于采用基于表单的（FORM-BASED）验证方式，只是要求在登录的JSP页面中的j_security_check 表单(for FORM-based) 需要两个参数：j_username和j_password。

对于用户的登录的名称和密码在Tomcat中可以以两种形式来存放，一是采用server.xml；另一种也可以采用用户自己的数据库表来存储。

** **

**2、设计系统中的各种人员的角色**

** **

**（1）设计思想**

 

l         统一用户管理，实现基于角色、粗粒度（基于URL）和细粒度（基于应用组件的方法调用）的访问策略管理体系，

l         基于分级角色的权限管理、统一证书管理和统一资源管理

 

 

** **

**（2）设计目标**

 

一般采用数据库表（对于复杂的也可以采用LDAP）记录每个系统用户的帐号信息、功能权限和数据权限信息，这样能够增加用户管理和权限设置的灵活性，同时也避免多个用户共用一个帐号的情况。

**（3）优点**

** **

l         从用户角度来看，登录所有应用系统都使用唯一的用户名和口令（数字证书）同时在访问系统时，也只需要登录一次（单点登录全网漫游---SSO（Single Sign-On））。

l        从管理者角度来看，提供了统一、集中、有效的用户管理。

**七、在Tomcat中采用基于表单的安全验证**

** **

**1、概述**

** **

**（1）基于表单的验证**

 

基于From的安全认证可以通过Tomcat Server对Form表单中所提供的数据进行验证，基于表单的验证使系统开发者可以自定义用户的登陆页面和报错页面。这种验证方法与基本HTTP的验证方法的唯一区别就在于它可以根据用户的要求制定登陆和出错页面。

通过拦截并检查用户的请求，检查用户是否在应用系统中已经创建好login session。如果没有，则将用户转向到认证服务的登录页面。但在Tomcat中的基于表单的验证凭证不被保护并以纯文本发送。

** **

**（2）在Tomcat 中的实现**

 

在Tomcat中，用户、用户组和角色都是在XML配置文件（C:/jakarta-tomcat-5.0.19/conf/tomcat-users.xml）中指定的，我们只需要提供一个登陆页面，包含一个名为j_security_check的Form表单，一个名为j_username的TextBox和一个名为j_password的PasswordBox，然后在/WEB-INF/web.xml中配置即可使用Tomcat默认的JAAS身份验证。

使用JAAS验证的好处是，验证逻辑从页面中分离，对页面的限制访问是通过/WEB-INF/web.xml中的配置指定的，无需自定义过滤器。

** **

**（3）为了实现Web应用程序的安全，Tomcat Web容器执行下面的步骤：**

l         在受保护的Web资源被访问时，判断用户是否被认证。

l         如果用户没有得到认证，则通过重定向到部署描述符中定义的注册页面，要求用户提供安全信任状。

l         根据为该容器配置的安全领域，确认用户的信任状有效。

l         判断得到认证的用户是否被授权访问部署描述符（web.xml）中定义的Web资源。****

** **

** **

**2、设计步骤**

** **

** **

**（1）编写登录页面和错误处理页面：**请见FormSafeWebApp 程序中的页面

** **

****

** **

**（2）登录的页面文件的内容如下**

 

基于FORM的用户认证要求你返回一个包括用户名和密码的HTML表单，这个表单相对应与用户名和密码的元素必须是j_username和j_password，并且表单的action描述必须为j_security_check（其实是一个Servlet）。该表单的具体操作以及j_username和j_password名字在Servlet中定义。当这个表单到达服务器的时候，由内部的Tomcat Server安全区对它进行确认。

包括这个表单的资源可以是一个HTML页面、一个JSP页面或者一个Servlet。你可以在<form-login-page>元素中定义。基于表单的认证能够使开发人员定制认证的用户界面。在web.xml的login-config标签项目定义了认证机制的类型、登录的URI和错误页面。

下面为该页面的内容：

<%@ page contentType="text/html; charset=GBK" %>

**注意：**action应该为j_security_check<html><head><title>在Tomcat 中采用Form 验证方式实现的安全Web应用程序的登录页</title>

</head><body bgcolor="#ffffff">

<form method="post" name="Login" **action="j_security_check"**>

<table width="500" border="1" align="center"> <tr>

**注意：**用户名称和密码的输入应该为j_username 和j_password    <td colspan="2"> <div align="center"><strong>在Tomcat中采用</strong><strong>基于表单的安全验证的登录表单 </strong> </div></td> </tr>

  <tr><td width="224"><div align="right">用户名称：</div></td>

    <td width="260"><input type="text" **name="j_username"**></td> </tr>

  <tr> <td><div align="right">密码：</div></td>

<td><input type="password" **name="j_password"**></td>

  </tr>

  <tr><td><div align="right"><input type="submit" name="Submit" value="提交">

    </div></td> <td><input type="reset" name="Submit2" value="重置"></td>

  </tr></table></form></body></html>****

** **

**（3）修改web.xml文件**

 

<?xml version="1.0" encoding="UTF-8"?>

定义本Web应用的默认启始页面<!DOCTYPE web-app PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN" "http://java.sun.com/dtd/web-app_2_3.dtd">

<web-app>

       <welcome-file-list>

           <welcome-file>index.jsp</welcome-file>

        </welcome-file-list>

 <!-- Security is active on entire directory -->

 ** <security-constraint>**

**    <display-name>Tomcat Server Form Security Constraint</display-name>**

**    <web-resource-collection>**

**     <web-resource-name>Protected Area</web-resource-name>**

**      <description>A Page of Login Success</description>**

**       <url-pattern>/ProtectedDirOne/index.jsp</url-pattern>**

**      </web-resource-collection>**

**    <auth-constraint>**

指定Form验证的用户的角色名称**      <!-- Anyone with one of the listed roles may access this area -->**

**      <role-name>admin</role-name>**

**    </auth-constraint>**

指定验证的方式为Form**  </security-constraint>**

**  <!-- Login configuration uses form-based authentication -->**

**  <login-config>**

**    <auth-method>FORM</auth-method>**

**    <realm-name>Tomcat Server Configuration Form-Based Authentication Area</realm-name>**

**    <form-login-config>**

**             <form-login-page>/login.jsp </form-login-page>**

**              <form-error-page>/Error.htm </form-error-page>**

** **

**    </form-login-config>**

**  </login-config>**

**  <!-- Security roles referenced by this web application -->**

**  <security-role>**

关联Tomcat中的admin的角色**    <description>**

**      The role is Administration**

**    </description>**

**    <role-name>admin</role-name>**

**  </security-role>**

</web-app>

** **

**（4）在C:/jakarta-tomcat-5.0.19/conf/tomcat-users.xml文件**中配置admin的角色以及与该 admin角色相匹配的用户名称和密码

****

** **

**（5）执行该页面**

在浏览器中直接输入受保护的页面的URL地址：

[http://127.0.0.1:8080/FormSafeWebApp/ProtectedDirOne/](http://127.0.0.1:8080/FormSafeWebApp/ProtectedDirOne/)，将出现要求登录的页面。

 

****

 

在表单中输入用户名称为admin（前面在tomcat-users.xml文件中所设置的某一用户名称），密码为12345678。然后点击“提交”，将出现如下页面

 

如果用户名称或者密码输入不正确，将出现如下的页面也就是错误页面

 

**（6）在页面中获得当前登录成功后的用户名称和实体名称**

       利用request对象中的getRemoteUser()方法获得当前登录成功后的用户名称和利用getUserPrincipal()方法获得当前登录成功后的实体名称。

** **

** **

**八、在Tomcat中配置单点登录（Single Sign-On）**

** **

**1、概述**

 

一旦你设置了realm和验证的方法，你就需要进行实际的用户登录处理。一般说来，对用户而言登录系统是一件很麻烦的事情，你必须尽量减少用户登录验证的次数。作为缺省的情况，当用户第一次请求受保护的资源时，每一个Web应用都会要求用户登录。

如果你运行了多个Web应用，并且每个应用都需要进行单独的用户验证，那这看起来就有点像你在与你的用户搏斗。用户们不知道怎样才能把多个分离的应用整合成一个单独的系统，所有他们也就不知道他们需要访问多少个不同的应用，只是很迷惑，为什么总要不停的登录。

** **

**2、Tomcat 中的“Single Sign-On”特性及配置**

 

其主要的特性是能够允许用户在访问同一虚拟主机下所有Web应用时，只需登录一次。为了使用这个功能，你只需要在C:/jakarta-tomcat-5.0.19/conf /server.xml文件中的Host标签上添加一个SingleSignOn Valve元素即可，如下所示：

<Valve className="org.apache.catalina.authenticator.SingleSignOn"   debug="0"/>

在Tomcat初始安装后，server.xml的注释里面包括SingleSignOn Valve配置的例子，你只需要去掉注释（**在339行左右**），即可使用。那么，任何用户只要登录过一个应用，则对于同一虚拟主机下的所有应用同样有效。

 

** **

**3、测试单点登录**

** **

**（1）      直接进入前面的Form验证所产生的Web应用**

**（[http://127.0.0.1:8080/FormSafeWebApp/ProtectedDirOne/](http://127.0.0.1:8080/FormSafeWebApp/ProtectedDirOne/)）将出现要求登录的页面**

** **

****

 

在表单中输入用户名称为admin（前面在tomcat-users.xml文件中所设置的某一用户名称），密码为12345678。然后点击“提交”，将以用户名admin进行成功登录该Web应用。

（2）再在该浏览器窗口内（不能在新窗口，否则会成为另一用户）直接输入[http://127.0.0.1:8080/admin/frameset.jsp](http://127.0.0.1:8080/admin/frameset.jsp)，此时将以admin的用户浏览另一Web应用。观察能否直接进入Tomcat的系统管理的页面，此时应该可以并且出现下面的页面。

****

       如果新开一浏览器窗口并直接输入[http://127.0.0.1:8080/admin/frameset.jsp](http://127.0.0.1:8080/admin/frameset.jsp)，看能否直接进入Tomcat的系统管理的页面，此时将会出现要求登录的页面。

 

** **

** **

**4、使用single sign-on valve所应该注意的问题**

l         value必须被配置和嵌套在相同的Host元素里，并且所有需要进行单点验证的web应用（必须通过context元素定义）都位于该Host下。

l         包括共享用户信息的realm必须被设置在同一级Host中或者嵌套之外。

l         不能被context中的realm覆盖。

l         使用单点登录的web应用最好使用一个Tomcat的内置的验证方式（Basic或者Form）（被定义在web.xml中的<auth-method>中），这比自定义的验证方式强。

l         单点登录需要使用cookies。
来源： <[http://blog.csdn.net/haojun186/article/details/7564174](http://blog.csdn.net/haojun186/article/details/7564174)> 
