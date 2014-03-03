---
layout: post
title: "J2EE学习笔记"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# J2EE学习笔记

J2EE学习笔记
                                          
注：框架可以用Word菜单中的 “视图/文档结构图” 看到                                         
J2EE模式
Value Object(值对象)  用于把数据从某个对象/层传递到其他对象/层的任意Java对象。
通常不包含任何业务方法。
也许设计有公共属性，或者提供可以获取属性值的get方法。
JSP
1．JSP的基础知识    
                
               __  
        _____ |   directive  (指令)
            |     |-- scripting （脚本）
 JSP -------|     |__ action  （动作）
|
|_____Template data ：除JSP语法外，JSP引擎不能解读的东西
    
1）在JSP中使用的directive(指令)主要有三个：
a) page指令
b) include指令
c) taglib指令
在JSP的任何地方，以任何顺序，一个页面可以包含任意数量的page指令
    2)Scripting（脚本）包括三种类型
a) <%！declaraction  %>;
b) <%  scriptlet     %>;
c) <%= expression   %>;
3)action（动作）
  标准的动作类型有：
        a)  <jsp:useBean>;
b)  <jsp:setProperty>;
d) <jsp:getProperty>;
e) <jsp:param>;
f) <jsp:include>;
g) <jsp:forward>;
h) <jsp:plugin>;
1. 注释： <% -----jsp comment-------%>;
       <! -----html comment-------%>;
2. <%@ page session = “true”  import =”java.util.*” %>;
session可以不赋值，默认为true,如果session=”false”,则在JSP页面中，隐含的变量session就不能使用。
3. 请求控制器结构（Request Controller）
也被称之为JSP Model 2 Architecture
这种途径涉及到使用一个Servlet或一个JSP作为一个应用程序或一组页面的入口点。
为创建可维护的JSP系统，Request Controller是最有用的方式之一。
不是JSP，而是Java类才是放置控制逻辑的正确的地方。
请求控制器的命名模式为: xxxController.jsp
请求控制器类的命名模式为: xxxRequestController
2．JSP中的JavaBean
JSP三种bean的类型
1) 页面bean
2) 会话bean
3) 应用bean
大多数的系统会使用一个会话bean来保持状态，而对每一个页面使用一个页面bean 来对复杂的数据进行表示。
页面bean是一个模型，而JSP是一个视图。
3．Custom tag
bean是信息的携带者，
而tag更适用于处理信息。
标记库包含一个标记库描述符（TLD）和用于实现Custom tag的Java类
在翻译阶段，JSP容器将使用TLD来验证页面中的所有的tag是否都被正确的使用。
   标记处理程序只是一个简单的适配器，而真正的逻辑是在另一个类中实现的，标记处理程序只是提供了一个供其他的可复用的类的JSP接口
      
Servlet
1．ServletConfig
&#61548; 一个ServletConfig对象是servlet container在servlet initialization的时候传递给servlet的。
ServletConfig包涵 ServletContext 和 一些 Name/Value pair (来自于deployment descriptor)
&#61548; ServletContext接口封装了Web应用程序的上下文概念。
2．会话跟踪
1) Session
&#61548; 当一个Client请求多个Servlets时，一个session可以被多个servlet共享。
&#61548; 通常情况下，如果server detect到browser支持cookie，那么URL就不会重写。
2) cookie
&#61548; 在Java Servlet中，如果你光  Cookie cookie = new Cookie(name,value)
那么当用户退出Browser时，cookie会被删除掉，而不会被存储在客户端的硬盘上。
如果要存储 cookie，需加一句   cookie.setMaxAge(200)
&#61548; cookie是跟某一个server相关的，运行在同一个server上的servlet共享一个cookie.
3) URL Rewriting
在使用URL Rewriting来维护Session ID的时候，每一次HTTP请求都需要EncodeURL()
典型的用在两个地方
1） out.print(“form action=\” ”);
out.print(response.encodeURL(“sessionExample”));
out.print(“form action=\” ”);
out.print(“method = GET>;”);
2） out.print(“<p>;<a href=\” ”);
out.print(response.encodeURL(“SessionExample?database=foo&datavalue=bar”));
out.println(“\” >;URL encoded </a>;”);
3．SingleThreadModel
默认的，每一个servlet definition in a container只有一个servlet class的实例。
只有实现了SingleThreadModel，container才会让servlet有多个实例。
Servlet specification上建议，不要使用synchronized，而使用SingleThreadModel。
SingleThreadModel（没有方法）
保证servlet在同一时刻只处理一个客户的请求。
SingleThreadModel是耗费资源的，特别是当有大量的请求发送给Servlet时，SingleThreadModel的作用是使包容器以同步时钟的方式调用service方法。
这等同于在servlet的service()方法种使用synchronized.
Single Thread Model一般使用在需要响应一个heavy request的时候，比如是一个需要和数据库打交道的连接。
2. 在重载Servlet地init( )方法后，一定要记得调用super.init( );
3. the client通过发送一个blank line表示它已经结束request
而the server通过关闭the socket来表示response已结束了。
4. 一个Http Servlet可以送三种东西给Client
1) a single status code
2) any number of http headers
3) a response body
5. Servlet之间信息共享的一个最简单的方法就是
System.getProperties().put(“key”,”value”);
6. Post和Get
Post：将form内各字段名称和内容放置在html header内传送给server
Get:  ?之后的查询字符串要使用URLEncode，经过URLEncode后，这个字符串不再带有空格，以后将在server上恢复所带有的空格。
      
Get是Web上最经常使用的一种请求方法，每个超链接都使用这种方法。
7. Web.xml就是Web Applicatin 的deployment descriptor
作用有：组织各类元素
        设置init param
        设置安全性
 
8. Request Dispatcher用来把接收到的request forward processing到另一个servlet
要在一个response里包含另一个servlet的output时，也要用到Request Dispatcher.
9. Servlet和Jsp在同一个JVM中，可以通过ServeltContext的
setAttribute( )
getAttribute( )
removeAttribute( )
来共享对象
10. 利用request.getParameter( )得到的String存在字符集问题。
可以用  strTitle = request.getParameter(“title”);
        strTitle = new String(strTitle.getBytes(“8859-1”),”gb2312”);
如果你希望得到更大得兼容性
        String encoding = response.getCharacterEncoding();     
//确定Application server用什么编码来读取输入的。
        strTitle = new String(strTitle.getBytes(encoding),”gb2312”);
XML
1．XML基础知识
1. 一个xml文档可以分成两个基本部分：
首部( header )
内容( content )
2. xml名字空间规范中指定：
xml文档中的每一个元素都处在一个名字空间中；如果没有指定的名字空间，缺省的名字空间就是和该元素相关联的名字空间。
3. A document that is well-formed obeys all of the rules of XML documents (nested tags, etc.)
" If a well-formed document uses a Document Type Definition (more on these in a minute), and it follows all the rules of the DTD, then it is also a valid document
4. A tag is the text between the <angle brackets>;
" An element is the start tag, the end tag,and everything (including other elements) in between
5. 标签( tags ) 实际上包含了“元素”( elements ) 和 “属性”( attributes )两部分。
用元素( elements )来描述有规律的数据。
用属性( attributes ) 来描述系统数据。
如果你有一些数据要提供给某个应用程序，该数据就可能要用到一个元素。
如果该数据用于分类，或者用于告知应用程序如何处理某部分数据，或者该数据从来没有直接对客户程序公开，那么它就可能成为一种属性。
6. CDATA (读作：C data ) C是character的缩写。
7.      org.xml.sax.Reader
/|\
       org.xm.l.sax.XMLReader
/|\
org.apche.xerces.parsers.SAXParser
2．WebService
2．1 WebService的基本概念
WebService是一种可以接收从Internet或者Intranet上的其它系统中传递过来的请求，轻量级的独立的通讯技术。
这种技术允许网络上的所有系统进行交互。随着技术的发展，一个Web服务可以包含额外的指定功能并且可以在多个B2B应用中协作通讯。
Web服务可以理解请求中上下文的关系，并且在每一个特定的情况下产生动态的结果。这些服务会根据用户的身份，地点以及产生请求的原因来改变不同的处理，用以产生一个唯一的，定制的方案。这种协作机制对那些只对最终结果有兴趣的用户来说，是完全透明的。
UDDI 
在用户能够调用Web服务之前，必须确定这个服务内包含哪些商务方法，找到被调用的接口定义，还要在服务端来编制软件。所以，我们需要一种方法来发布我们的Web服务。 
UDDI (Universal Description, Discovery, and Integration) 是一个主要针对Web服务供应商和使用者的新项目。UDDI 项目中的成员可以通过UDDI Business Registry (UBR) 来操作Web服务的调用，UBR是一个全球性的服务。
Web服务供应商可以在UBR中描述并且注册他们的服务。
用户可以在UBR中查找并定位那些他们需要的服务。 
UDDI是一种根据描述文档来引导系统查找相应服务的机制。
UDDI包含标准的“白皮书”类型的商业查询方式，
“黄皮书”类型的局部查找，以及
“绿皮书”类型的服务类型查找。
UDDI利用SOAP消息机制（标准的XML/HTTP）来发布，编辑，浏览以及查找注册信息。它采用XML格式来封装各种不同类型的数据，并且发送到注册中心或者由注册中心来返回需要的数据。
WSDL 
对于商业用户来说，要找到一个自己需要使用的服务，他必须知道如何来调用。
WSDL (Web Services Description Language) 规范是一个描述接口，语义以及Web服务为了响应请求需要经常处理的工作的XML文档。这将使简单地服务方便，快速地被描述和记录。
以下是一个WSDL的样例： 
<?xml version="1.0"?>;
<definitions name="StockQuote"
                targetNamespace="http://example.com/stockquote.wsdl"
                xmlns:tns="http://example.com/stockquote.wsdl"
                xmlns:xsd1="http://example.com/stockquote.xsd"
                xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/"
                xmlns="http://schemas.xmlsoap.org/wsdl/">;
<types>;
   <schema targetNamespace=http://example.com/stockquote.xsd
                 xmlns="http://www.w3.org/2000/10/XMLSchema">;
      <element name="TradePriceRequest">;
        <complexType>;
           <all>;
                <element name="tickerSymbol" type="string"/>;
              </all>;
           </complexType>;
        </element>;
        <element name="TradePrice">;
            <complexType>;
                <all>;
                   <element name="price" type="float"/>;
                 </all>;
              </complexType>;
           </element>;
        </schema>;
     </types>;
     <message name="GetLastTradePriceInput">;
         <part name="body" element="xsd1:TradePriceRequest"/>;
     </message>;
     <message name="GetLastTradePriceOutput">;
         <part name="body" element="xsd1:TradePrice"/>;
    </message>;
    <portType name="StockQuotePortType">;
        <operation name="GetLastTradePrice">;
            <input message="tns:GetLastTradePriceInput"/>;
            <output message="tns:GetLastTradePriceOutput"/>;
         </operation>;
      </portType>;
      <binding name="StockQuoteSoapBinding"
                    type="tns:StockQuotePortType">;
         <soap:binding style="document"
                                 transport="http://schemas.xmlsoap.org/soap/http"/>;
        <operation name="GetLastTradePrice">;
            <soap:operation
                           soapAction="http://example.com/GetLastTradePrice"/>;
            <input>;
               <soap:body use="literal"/>;
            </input>;
            <output>;
                <soap:body use="literal"/>;
            </output>;
         </operation>;
      </binding>;
     <service name="StockQuoteService">;
        <documentation>;My first service</documentation>;
        <port name="StockQuotePort" binding="tns:StockQuoteBinding">;
            <soap:address location="http://example.com/stockquote"/>;
        </port>;
     </service>;
  </definitions>;
它包含了以下的关键信息： 
消息的描述和格式定义可以通过XML文档中的<types>;和<message>; 标记来传送。 
<portType>; 标记中表示了消息传送机制。 (e.g. request-only, request-response, response-only) 。
<binding>; 标记指定了编码的规范 。
<service>; 标记中表示服务所处的位置 (URL)。 
WSDL在UDDI中总是作为一个接口描述文档。因为UDDI是一个通用的用来注册WSDL规范的地方，UDDI的规范并不限制任何类型或者格式描述文档。这些文档可能是一个WSDL文档，或者是一个正规的包含导向文档的Web页面，也可能只是一个包含联系信息的电子邮件地址。
现在Java提供了一个 Java API for WSDL (JWSDL)规范。它提供了一套能快速处理WSDL文档的方法，并且不用直接对XML文档进行操作，它会比JAXP更方便，更快速。
SOAP 
当商业用户通过UDDI找到你的WSDL描述文档后，他通过可以Simple Object Access Protocol (SOAP) 调用你建立的Web服务中的一个或多个操作。
SOAP是XML文档形式的调用商业方法的规范，它可以支持不同的底层接口，象HTTP(S)或者SMTP。
之所以使用XML是因为它的独立于编程语言，良好的可扩展性以及强大的工业支持。之所以使用HTTP是因为几乎所有的网络系统都可以用这种协议来通信，由于它是一种简单协议，所以可以与任何系统结合，还有一个原因就是它可以利用80端口来穿越过防火墙。
SOAP的强大是因为它简单。SOAP是一种轻量级的，非常容易理解的技术，并且很容易实现。它有工业支持，可以从各主要的电子商务平台供应商那里获得。
从技术角度来看，SOAP详细指明了如何响应不同的请求以及如何对参数编码。一个SOAP封装了可选的头信息和正文，并且通常使用HTTP POST方法来传送到一个HTTP 服务器，当然其他方法也是可以的，例如SMTP。SOAP同时支持消息传送和远程过程调用。以下是一个SOAP请求。
POST /StockQuote HTTP/1.1
Host: www.stockquoteserver.com
Content-Type: text/xml; charset="utf-8"
Content-Length: nnnn 
SOAPAction: "Some-URI" 
<SOAP-ENV:Envelope
    xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
    SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"/>;
    <SOAP-ENV:Header>;
        <t:Transaction xmlns:t="some-URI" SOAP-ENV:mustUnderstand="1">;
           5
        </t:Transaction>;
    </SOAP-ENV:Header>;
    <SOAP-ENV:Body>;
        <m:GetLastTradePrice xmlns:m="Some-URI">;
            <symbol>;SUNW</symbol>;
        </m:GetLastTradePrice>;
    </SOAP-ENV:Body>;
</SOAP-ENV:Envelope>; 
JAXR 
为了支持UDDI在Java平台上的功能，Java APIs for XML Registries (JAXR)允许开发者来访问注册中心。
值得注意的是，JAXR并不是建立Web服务必需的，你可以利用其他常用的XML APIs来直接集成这些协议。
JAXR是一个方便的API，它提供了Java API来发布，查找以及编辑那些注册信息。它的重点在于基于XML的B2B应用，复杂的地址本查找以及对XML消息订阅的支持等Web服务。
它也可以用来访问其他类型的注册中心，象ebXML注册中心。
这些对Web服务的注册信息进行的操作，可以使用当前的一些Web服务工具来完成（例如第三方的SOAP和ebXML消息工具）。另外，当JAXP提供了一致并具有针对性的API来完成这些操作，这将使开发变得更加容易。
JAX/RPC 
为了使开发人员专注于建立象SOAP那样的基于XML的请求，JCP正在开发基于RPC (JAX/RPC) 的Java API。JAX/RPC是用来发送和接收方法调用请求的，它基于XML协议，象SOAP，或者其他的象XMLP (XML Protocol，要了解更多可以参考http://www.w3.org/2000/xp/)。JAX/RPC使你不用再关注这些协议的规范，使应用的开发更快速。不久，开发人员就不用直接以XML表示方法调用了。
目前有很多第三方实现了SOAP，开发人员可以在不同的层次上调用SOAP，并选择使用哪一种。将来，JAX/RPC会取代这些APIs并提供一个统一的接口来构造以及处理SOAP RPC请求。
在接收一个从商业伙伴那里过来的SOAP请求的时候，一个Java servlet用JAX/RPC来接收这个基于XML的请求。一旦接收到请求后，servlet会调用商务方法，并且把结果回复给商业伙伴。
JAXM 
当从商业合作伙伴那里接收一个Web服务的请求时，我们需要Java API实现一个Servlet来处理ebXML消息，就象我们用JAX/RPC来处理SOAP请求一样。
Java API for XML Messaging (JAXM) 是集成XML消息标准（象ebXML消息或者SOAP消息）的规范。
这个API是用来推动XML消息处理的，它检测那些预定单的消息格式以及约束。它控制了所有的消息封装机制，用一种直观的方式分割了消息中的信息，象路由信息，发货单。这样，开发人员只要关注消息的有效负载，而不用去担心那些消息的重复处理。
目前的开发人员用JAXP来实现JAXM将要提供的功能，JAXM将会提供一套非常具有针对性的API来处理基于XML的消息传送。这将大大简化开发人员的代码，并使它们具有统一的接口。
JAXM和JAX/RPC的差别在于处理消息导向的中间件以及远程过程调用的不同。JAXM注重于消息导向，而JAX/RPC是用来完成远程过程调用的。以下是图解。
 
请注意，在JAXM 和 JAX/RPC技术成熟之前，开发人员还是依赖于第三方的SOAP APIs，象Apache SOAP, IdooXOAP, 以及 GLUE。当JAXM 和 JAX/RPC正式发布后，它将为当前不同的SOAP和ebXML消息提供统一的接口。就象JDBC位多种不同的数据库提供统一的接口。
JAXB 
XML绑定技术可以把XML文档和Java对象进行自由转换。
用JAXB，你可以在后台的EJB层，把XML文档转换成Java对象。同样你也可以把从EJB中取出的Java对象转换成XML文档返回给用户。
JAXB接口提供了比SAX和DOM更高级的方法来处理XML文档。它提供的特性可以在XML数据和Java类之间互相映射，提供了一个简单的方法来转换XML数据。它比逐个解析标记更简单。
2．2 建立WeService的步骤
在建立WeService的时候，有三个主要步骤：
1．建立客户端联接 
为了允许Applets，Applications，商业合作伙伴，浏览器和PDAs 使用Web服务。 
2．实现Web服务 
包括工作流，数据传送，商业逻辑以及数据访问。这些功能是隐藏在Web服务后，并且为客户端工作的。 
3．联接后台系统 
这个系统可能包括一个或多个数据库，现存的企业信息系统，商业合作伙伴自己的系统或者Web服务，以及在多个系统中共享的数据。
基于J2EE的Web服务的核心构架：
 
RMI
1. RMI-IIOP
2. RMI 是在java中使用remote method invocation的最初的方法，RMI使用java.rmi包
RMI－IIOP 是RMI的一个特殊版本，RMI－IIOP可以和CORBA兼容，RMI-IIOP使用java.rmi包和javax.rmi
JAF(Java活动构架)
开发者可以使用JAF来决定任意一块数据的类型、封装对数据的访问、寻找合适的操作、实例化相关的bean来执行这些操作等。
例如，JavaMail就是使用JAF根据MIME类型来决定实例化那一个对象。
EJB
1. EJB组件实现代码的限制
    EJB组件的约束
    EJB的开发者并不需要在EJB的组件实现代码中编写系统级的服务，EJB提供商/开发
者需知道并且严格地遵守一些限制，这些限制与开发稳定的和可移植的EJB组件的利益有
关。
    以下是你应该回避使用的一些Java特色，并且在你的EJB组件的实现代码中要严格限
制它们的使用：
    1.使用static，非final 字段。建议你在EJB组件中把所有的static字段都声明为final型的。这样可以保证前后一致的运行期语义，使得EJB容器有可以在多个Java虚拟机之间分发组件实例的灵活性。
    2.使用线程同步原语来同步多个组件实例的运行。避免这个问题，你就可以使EJB容器灵活的在多个Java虚拟机之间分发组件实例。
    3.使用AWT函数完成键盘的输入和显示输出。约束它的原因是服务器方的商业组件意味着提供商业功能而不包括用户界面和键盘的I/O功能。
    4.使用文件访问/java.io 操作。EJB商业组件意味着使用资源管理器如JDBC来存储和检索数据而不是使用文件系统API。同时，部署工具提供了在部署描述器（descriptor）中存储环境实体，以至于EJB组件可以通过环境命名上下文用一种标准的方法进行环境实体查询。所以，使用文件系统的需求基本上是被排除了。
    5.监听和接收socket连接，或者用socket进行多路发送。EJB组件并不意味着提供网络socket服务器功能，但是，这个体系结构使得EJB组件可以作为socket客户或是RMI客户并且可以和容器所管理的环境外面的代码进行通讯。
    6.使用映象API查询EJB组件由于安全规则所不能访问的类。这个约束加强了Java平台的安全性。
    7.欲创建或获得一个类的加载器，设置或创建一个新的安全管理器，停止Java虚拟机，改变输入、输出和出错流。这个约束加强了安全性同时保留了EJB容器管理运行环境的能力。
    8.设置socket工厂被URL's ServerSocket,Socket和Stream handler使用。避免这个特点，可以加强安全性同时保留了EJB容器管理运行环境的能力。
    9.使用任何方法启动、停止和管理线程。这个约束消除了与EJB容器管理死锁、线程
和并发问题的责任相冲突的可能性。
    通过限制使用10－16几个特点，你的目标是堵上一个潜在的安全漏洞：
    10.直接读写文件描述符。
    11.为一段特定的代码获得安全策略信息。
    12.加载原始的类库。
    13.访问Java一般角色所不能访问的包和类。
    14.在包中定义一个类。
    15.访问或修改安全配置对象（策略、安全、提供者、签名者和实体）。
    16.使用Java序列化特点中的细分类和对象替代。
    17.传递this引用指针作为一个参数或者作为返回值返回this引用指针。你必须使用
SessionContext或EntityContext中的getEJBObject()的结果。
    Java2平台的安全策略
    以上所列的特点事实上正是Java编程语言和Java2标准版中的标准的、强有力的特色。EJB容器允许从J2SE中使用一些或全部的受限制的特色，尽管对于EJB组件是不可用的，但需通过J2SE的安全机制来使用而不是通过直接使用J2SE的API。
    Java2平台为EJB1.1规范中的EJB容器所制定的安全策略定义了安全许可集，这些许可在EJB组件的编程限制中出现。通过这个策略，定义了一些许可诸如：java.io.FilePermission,java.net.NetPermission,java.io.reflect.ReflectPermission,java.lang.security.SecurityPermission,以便加强先前所列出的编程限制。
    许多EJB容器没有加强这些限制，他们希望EJB组件开发者能遵守这些编程限制或者是带有冒险想法违背了这些限制。违背这些限制的EJB组件，比标准方法依赖过多或过少的安全许可，都将很少能在多个EJB容器间移植。另外，代码中都将隐藏着一些不确定的、难以预测的问题。所有这些都足以使EJB组件开发者应该知道这些编程限制，同时也应该认真地遵守它们。
    任何违背了这些编程限制的EJB组件的实现代码在编译时都不能检查出来，因为这些特点都是Java语言和J2SE中不可缺少的部分。
    对于EJB组件的这些限制同样适用于EJB组件所使用的帮助/访问（helper/access）类，J2EE应用程序使用Java文档（jar）文件格式打包到一个带.ear（代表Enterprise Archive）扩展名的文件中，这个ear文件对于发送给文件部署器来说是标准的格式。ear文件中包括在一个或多个ejb－jar文件中的EJB组件，还可能有ejb－jar所依赖的库文件。所有ear文件中的代码都是经过深思熟虑开发的应用程序并且都遵守编程限制和访问许可集。
未来版本的规范可能会指定通过部署工具来定制安全许可的能力，通过这种方法指定了一个合法的组件应授予的许可权限，也指定了一个标准方法的需求：如从文件系统中读文件应有哪些要求。一些EJB容器/服务器目前在它们的部署工具中都提供了比标准权限或多或少的许可权限，这些并不是EJB1.1规范中所需要的。
  理解这些约束
    EJB容器是EJB组件生存和执行的运行期环境，EJB容器为EJB组件实例提供了一些服务如：事务管理、安全持久化、资源访问、客户端连接。EJB容器也负责EJB组件实例整个生命期的管理、扩展问题以及并发处理。所以，EJB组件就这样寄居在一个被管理的执行环境中－－即EJB容器。
    
    因为EJB容器完全负责EJB组件的生命期、并发处理、资源访问、安全等等，所以与容器本身的锁定和并发管理相冲突的可能性就需要消除，许多限制都需要使用来填上潜在的安全漏洞。除了与EJB容器责任与安全冲突的问题，EJB组件还意味着仅仅聚焦于商务逻辑，它依赖于EJB容器所提供的服务而不是自己来直接解决底层的系统层的问题。
    可能的问题
    通常，EJB组件在容器之间的移植不可避免地与如下问题相关：
    1.它需要依靠的受限制的特点在特定EJB容器中没有得到加强。
    2.它需要依靠的非标准的服务从容器中可获得。
    为了保证EJB组件的可移植性和一致的行为，你应该使用一个具有与Java2平台安全
策略集相一致的策略集的容器来测试EJB组件，并且其加强了前述的编程限制。
    总结
    EJB组件开发者应该知道这些推荐的关于EJB组件的编程限制，明白它们的重要性，并且从组件的稳定性和可移植性利益方面考虑来遵循它们。因为这些编程限制能阻止你使用标准的Java语言的特点，违背了这些编程限制在编译时不会知道，并且加强这些限制也不是EJB容器的责任。所有这些原因都使你应很小心地遵守这些编程限制，这些限制在组件的合同中已经成为了一个条款，并且它们对于建造可靠的、可移植的组件是非常重要的。
2. 优化EJB
entity bean为在应用程序和设计中描述持久化商业对象（persistent business objec ts）提供了一个清晰的模型。在java对象模型中，简单对象通常都是以一种简单的方式进行处理但是，很多商业对象所需要的事务化的持久性管理没有得到实现。entity bean将持久化机制封装在容器提供的服务里，并且隐藏了所有的复杂性。entity bean允许应用程序操纵他们就像处理一个一般的java对象应用。除了从调用代码中隐藏持久化的形式和机制外，entity bean还允许EJB容器对对象的持久化进行优化，保证数据存储具有开放性，灵活性，以及可部署性。在一些基于EJB技术的项目中，广泛的使用OO技术导致了对entity bean的大量使用，SUN的工程师们已经积累了很多使用entity Bean的经验，这篇文章就详细阐述的这些卡发经验：
*探索各种优化方法
*提供性能优化和提高适用性的法则和建议
*讨论如何避免一些教训。
法则1：只要可以，尽量使用CMP 
CMP方式不仅减少了编码的工作量，而且在Container中以及container产生的数据库访问代码中包括了许多优化的可能。Container可以访问内存缓冲中的bean,这就允许它可以监视缓冲中的任何变化。这样的话就在事物没有提交之前，如果缓存的数据没有变化就不用写到数据库中。就可以避免许多不必要的数据库写操作。另外一个优化是在调用find方法的时候。通常情况下find方法需要进行以下数据库操作：
查找数据库中的纪录并且获得主键
将纪录数据装入缓存
CMP允许将这两步操作优化为一步就可以搞定。[具体怎么做我也没弄明白，原文没有具体阐述]
法则2：写代码时尽量保证对BMP和CMP都支持
许多情况下，EJB的开发者可能无法控制他们写的bean怎么样被部署，以及使用的container是不是支持CMP.
一个有效的解决方案是，将商业逻辑的编码完全和持久化机制分离。再CMP类中实现商业逻辑，然后再编写一个BMP类，用该类继承CMP类。这样的话，所有的商业逻辑都在CMP类中，而持久化机制在BMP中实现。[我觉得这种情况在实际工作中很少遇到，但是作者解决问题的思路值得学习]
法则3：把ejbStore中的数据库访问减小到最少。
如果使用BMP,设置一个缓存数据改变标志dirty非常有用。所有改变数据库中底层数据的操作，都要设置dirty,而在ejbStore（）中，首先检测dirty的值，如果dirty的值没有改变，表明目前数据库中的数据与缓存的一致，就不必进行数据库操作了，反之，就要把缓存数据写入数据库。
法则4：总是将从lookup和find中获得的引用进行缓存。（cache）
引用缓存对session bean和entity bean 都是适用的。
通过JNDI lookup获得EJB资源。比如DataSource,bean的引用等等都要付出相当大的代价。因此应该避免多余的lookup.可以这样做：
将这些引用定义为实例变量。
从setEntityContext(session Bean使用setSessionContext)方法查找他们。SetEntityContext方法对于一个bean实例只执行一次，所有的相关引用都在这一次中进行查找，这样查找的代价就不是那么昂贵了。应该避免在其他方法中查找引用。尤其是访问数据库的方法：ejbLoad()和ejbStore(),如果在这些频繁调用的方法中进行DataSource的查找，势必造成时间的浪费。
调用其他entity bean的finder方法也是一种重量级的调用。多次调用finder()方法的代价非常高。如果这种引用不适合放在setEntityContext这样的初始化时执行的方法中执行，就应该在适当的时候缓存finder的执行结果。只是要注意的是，如果这个引用只对当前的entity有效，你就需要在bean从缓冲池中取出来代表另外一个实体时清除掉这些引用。，这些操作应该在ejbActivate（）中进行。
法则5:总是使用prepare statements
这条优化法则适用于所有访问关系数据库的操作。
数据库在处理每一个SQL Statement的时候，执行前都要对Statement进行编译。一些数据库具有缓存statement和statement的编译后形式的功能。数据库可以把新的Statement和缓存中的进行匹配。然而，如果要使用这一优化特性，新的Statement要必须和缓存中的Statement完全匹配。
对于Non-prepared Statement,数据和Statement本身作为一个字符串传递，这样由于前后调用的数据不同而不能匹配，就导致无法使用这种优化。而对于prepared Statement,数据和Statement是分开传递给数据库的，这样Statement就可以和cache中已编译的Statement进行匹配。Statement就不必每次都进行编译操作。从而使用该优化属性。
这项技术在一些小型的数据库访问中能够减少Statement将近90%的执行时间。
法则6：完全关闭所有的Statement
在编写BMP的数据库访问代码时，记住一定要在数据库访问调用之后关闭Statement,因为每个打开的Statement对应于数据库中的一个打开的游标。
Security
1．加密
对称加密
  （1）分组密码
  （2）流密码
常用的对称加密算法：
   DES和TripleDES
   Blowfish
   RC4
   AES
非对称加密
    常用的非对称加密算法
RSA
ElGamal
      会话密钥加密（对称加密和非对称加密一起使用）
常用的会话密钥加密协议
S/MIME
PGP
SSL和TLS   SSL是在Application level protocal和Transport protocal之间的。
比如：Http和TCP/IP之间
SSL 提供了服务器端认证和可选的客户端认证，保密性和数据完整性。
提供基于SSL方式的传输加密和认证，确保以下三种安全防护：
数据的机密性和准确性、
服务器端认证
客户端认证。
客户端认证比服务器端认证不很普遍的原因是每一个要被认证的客户都必须有一张Verisign这样的CA签发的证书。
通常，在进行身份认证的时候，应当只接受一个CA，这个CA的名字包含在客户证书中。
由于不可能随意创建一个由指定CA签发的证书，所以这可以有效的防御通过伪造证书来进行的攻击尝试。
2．认证（Authentication）
认证就是确定一条消息或一个用户的可靠性的过程。
1.消息摘要
   MD5
   SHA和SHA-1
2.消息认证码（Message Authientication Codes,MAC）
3.数字签名
用户可以用自己的密钥对信息加以处理，由于密钥仅为本人所有，这样就产生了别人无法生成的文件，也就形成了数字签名
数字签名可以
1）保证数据的完整性
2）验证用户的身份
数字签名采用一个人的私钥计算出来，然后用公钥去检验。
           hash算法                             私钥加密
原报文 ――――――>;报文摘要( Message Digest ) ―――――>;数字签名
原报文和数字签名一起被发送到接受者那里，接受者用同样的hash算法得到报文摘要，然后用发送者的公钥解开数字签名。
比较是否相同，则可以确定报文确定来自发送者。
验证数字签名必须使用公钥，但是，除非你是通过安全的方式直接得到，否则不能保证公钥的正确性。（数字证书可以解决这个问题）
一个接受者在使用公钥（public key）检查数字签名（digital signature）的可信度时，通常先要检查收到的公钥（public key）是否可信的。
因此发送方不是单单地发送公钥（public key），而是发送一个包含公钥（public key）的数字证书（cetificate ）。
4.数字证书
   数字证书是一个经证书授权中心数字签名的包含公开密钥所有者信息以及公开密钥的文件。
数字证书Cetificate中包括：
I. 用户的公钥（public key） 
II. 用户的一些信息，如姓名，email
III. 发行机构的数字签名（digital signature）， 用于保证证书的可信度
IV. 发行机构的一些信息
数字证书的格式遵循X.509国际标准。
          
注意：一个数字证书certificate并不适用于多种browser,甚至一种Browser的多个版本。
数字标识由公用密钥、私人密钥和数字签名三部分组成。
当在邮件中添加数字签名时，您就把数字签名和公用密钥加入到邮件中。数字签名和公用密钥统称为证书。您可以使用 Outlook Express 来指定他人向您发送加密邮件时所需使用的证书。这个证书可以不同于您的签名证书。
收件人可以使用您的数字签名来验证您的身份，并可使用公用密钥给您发送加密邮件，这些邮件必须用您的私人密钥才能阅读。
要发送加密邮件，您的通讯簿必须包含收件人的数字标识。这样，您就可以使用他们的公用密钥来加密邮件了。当收件人收到加密邮件后，用他们的私人密钥来对邮件进行解密才能阅读。
在能够发送带有数字签名的邮件之前，您必须获得数字标识。如果您正在发送加密邮件，您的通讯簿中必须包含每位收件人的数字标识。
数字证书，可以是个人证书或 Web 站点证书，用于将身份与"公开密钥"关联。只有证书的所有者才知道允许所有者"解密"或进行"数字签名"的相应"私人密钥"。当您将自己的证书发送给其他人时，实际上发给他们的是您的公开密钥，这样他们就可以向您发送只能由您使用私人密钥解密和读取的加密信息。　
通过浏览器使用数字证书，必须先要设置浏览器软件 Internet Explorer 或 NetScape使用此证书，才能开始发送加密或需要数字签名的信息。访问安全的 Web 站点（以"https"打头的站点）时，该站点将自动向您发送他们的Web站点证书。
3．CA(证书授证中心)
CA机构，又称为证书授证(Certificate Authority)中心，作为电子商务交易中受信任的第三方，承担公钥体系中公钥的合法性检验的责任。CA中心为每个使用公开密钥的用户发放一个数字证书，数字证书的作用是证明证书中列出的用户合法拥有证书中列出的公开密钥。CA机构的数字签名使得攻击者不能伪造和篡改证书。在SET交易中，CA不仅对持卡人、商户发放证书，还要对获款的银行、网关发放证书。它负责产生、分配并管理所有参与网上交易的个体所需的数字证书，因此是安全电子交易的核心环节。
对证书的信任基于对根证书的信任. 例如在申请SHECA的个人数字证书前，需要先下载根证书，然后再进行各类证书的申请。
下载根证书的目的：
网络服务器验证(S)；安全电子邮件(E)
申请个人数字证书可以为Internet用户提供发送电子邮件的安全和访问需要安全连接（需要客户证书）的站点。
1）个人数字证书 
a.个人身份证书  
个人身份证书是用来表明和验证个人在网络上的身份的证书，它确保了网上交易和作业的安全性和可靠性。可应用于：网上炒股、网上理财、网上保险、网上缴费、网上购物、网上办公等等。个人身份证书可以存储在软盘或IC卡中。   　
b.个人安全电子邮件证书   
个人安全电子邮件证书可以确保邮件的真实性和保密性。申请后一般是安装在用户的浏览器里。用户可以利用它来发送签名或加密的电子邮件。 
  
用户在申请安装完安全安全电子邮件数字证书后，就可以对要发送的邮件进行数字签名。收信人收到该邮件后，就可以看到数字签名的标记，这样就可以证明邮件肯定来自发信者本人，而不是别人盗用该帐号伪造信件，同时也保证该邮件在传送过程中没被他人篡改过任何数据。
安全电子邮件中使用的数字证书可以实现：
保密性  通过使用收件人的数字证书对电子邮件加密。如此以来，只有收件人才能阅读加密的邮件，在Internet上传递的电子邮件信息不会被人窃取，即使发错邮件，收件人也无法看到邮件内容。
认证身份  在Internet上传递电子邮件的双方互相不能见面，所以必须有方法确定对方的身份。利用发件人数字证书在传送前对电子邮件进行数字签名即可确定发件人身份，而不是他人冒充的。
完整性  利用发件人数字证书在传送前对电子邮件进行数字签名不仅可确定发件人身份，而且传递的电子邮件信息也不能被人在传输过程中修改。
不可否认性  由于发件人的数字证书只有发件人唯一拥有，故发件人利用其数字证书在传送前对电子邮件进行数字签名，发件人就无法否认发过这个电子邮件。
OutLook Express中的个人安全电子邮件证书
签名邮件带有签名邮件图标。 
签名邮件可能出现的任何问题都将在本信息之后可能出现的“安全警告”中得到描述。如果存在问题，您应该认为邮件已被篡改，或并非来自所谓的发件人。
当收到一封加密邮件时，您应该可以自信地认为邮件未被任何第三者读过。Outlook Express 会自动对电子邮件解密， 如果在您的计算机上装有正确的数字标识。
 
2）企业数字证书 
a．企业身份证书
企业身份证书是用来表明和验证企业用户在网络上身份的证书，它确保了企业网上交易和作业的安全性和可靠性。可应用于：网上证券、网上办公、网上交税、网上采购、网上资金转帐、网上银行等。企业身份证书可以存储在软盘和IC卡中。    　　
b．企业安全电子邮件证书   
企业安全电子邮件证书可以确保邮件的真实性和保密性。申请后一般是安装在用户的浏览器里。企业可以利用它来发送签名或加密的电子邮件。
可使用 Windows 2000 中的证书服务来创建证书颁发机构 (CA)，它负责接收证书申请、验证申请中的信息和申请者的身份、颁发证书、吊销证书以及发布证书吊销列表 (CRL)。 
通常，当用户发出证书申请时，在其计算机上的加密服务提供程序 (CSP) 为用户生成公钥和私钥对。用户的公钥随同必要的识别信息发送至 CA。如果用户的识别信息符合批准申请的 CA 标准，那么 CA 将生成证书，该证书由客户应用程序检索并就地存储。
4．SET
安全接口层协议——SSL(Se cure SocketsLayer)，并且已经几乎成为了目前WWW 世界的事实标准。这一标准使用公共密钥编码方案来对传输数据进行加密，在双方之间建立一个Internet 上的加密通道，从而使第三方无法获得其中的信息，其思路与目前流行的VPN方案大致相同，目的都是要保护数据不被未经授权的第三方所窃听，或即使窃听到也不知所云。但就象VPN 一样，SSL 在认证方面没有任何作为，它们都需要通过另外的手段来确认身份和建立双方彼此间的信任，然后再通过SSL 进行交易。 
正是由于SSL 标准在认证方面的缺憾，所以SET 才有存在的必要。SET(Secure Electronic Transactions) 规范由Masterc ard 和Visa 公司于1996 年发布，专家们认为SET 是保证用户与商家在电子商务与在线交易中免受欺骗的重要手段。传统的信用卡交易者总在担心不诚实的店员会将自己的信用卡号码透露给他人，而在线交易也是如此，持卡者总在担心服务器端的管理员会将信用卡号码泄露出去，或者担心黑客会在管理员不知情的情况下盗取信用卡号码。事实上这些担心都是必要的，而SET 标准则可以保证用户的信用卡号码只传送给信用卡公司进行认证，不会被系统管理员看到，也不会留在交易服务器的硬盘上给黑客以可乘之机。
5．PKI
PKI是一种易于管理的、集中化的网络安全方案。它可支持多种形式的数字认证: 数据加密、数字签字、不可否认、身份鉴别、密钥管理以及交叉认证等。PKI可通过一个基于认证的框架处理所有的数据加密和数字签字工作。P KI标准与协议的开发迄今已有15年的历史,目前的PKI已完全可以向企业网络提供有效的安全保障。
PKI是一种遵循标准的密钥管理平台,它能够为所有网络应用透明地提供采用加密和数字签名等密码服务所必需的密钥和证书管理。PKI必须具有
  1）CA、
2）证书库、
3）密钥备份及恢复系统、
4）证书作废处理系统、
5）客户端证书处理系统
等基本成分,构建PKI也将围绕着这五大系统来构建
一个PKI由众多部件组成,这些部件共同完成两个主要功能:
1）为数据加密
2）创建数字认证。
服务器(即后端)产品是这一系统的核心,这些数据库管理着数字认证、公共密钥及专用密钥( 分别用于数据的加密和解密)。
CA数据库负责发布、废除和修改X.509数字认证信息,它装有用户的公共密钥、证书有效期以及认证功能(例如对数据的加密或对数字签字的验证) 。为了防止对数据签字的篡改,CA在把每一数字签字发送给发出请求的客户机之前,需对每一个数字签字进行认证。一旦数字认证得以创建, 它将会被自动存储于X.500目录中,X.500目录为树形结构。LDAP(Lightweight Directory Access Protocol)协议将响应那些要求提交所存储的公共密钥认证的请求。CA为每一用户或服务器生成两对独立的公共和专用密钥。其中一对用于信息的加密和解密, 另一对由客户机应用程序使用,用于文档或信息传输中数字签字的创建。 
大多数PKI均支持证书分布,这是一个把已发布过的或续延生命期的证书加以存储的过程。这一过程使用了一个公共查询机制,X.500目录可自动完成这一存储过程。影响企业普遍接受P KI的一大障碍是不同CA之间的交叉认证。假设有两家公司,每一家企业分别使用来自不同供应商的CA,现在它们希望相互托管一段时间。如果其后援数据库支持交叉认证, 则这两家企业显然可以互相托管它们的CA,因而它们所托管的所有用户均可由两家企业的CA所托管。  
＊ 认证机关  
    CA是证书的签发机构,它是PKI的核心。众所周知,构建密码服务系统的核心内容是如何实现密钥管理,公钥体制涉及到一对密钥,即私钥和公钥, 私钥只由持有者秘密掌握,无须在网上传送,而公钥是公开的,需要在网上传送,故公钥体制的密钥管理主要是公钥的管理问题,目前较好的解决方案是引进证书（certificate）机制。  
    
    证书是公开密钥体制的一种密钥管理媒介。它是一种权威性的电子文档，形同网络计算环境中的一种身份证，用于证明某一主体（如人、服务器等）的身份以及其公开密钥的合法性。在使用公钥体制的网络环境中, 必须向公钥的使用者证明公钥的真实合法性。因此,在公钥体制环境中,必须有一个可信的机构来对任何一个主体的公钥进行公证,证明主体的身份以及他与公钥的匹配关系。C A正是这样的机构,它的职责归纳起来有:  
   1、验证并标识证书申请者的身份；  
   2、确保CA用于签名证书的非对称密钥的质量；  
   3、确保整个签证过程的安全性,确保签名私钥的安全性；  
   4、证书材料信息（包括公钥证书序列号、CA标识等）的管理；  
   5、确定并检查证书的有效期限；  
   6、确保证书主体标识的唯一性,防止重名；  
   7、发布并维护作废证书表；  
   8、对整个证书签发过程做日志记录；  
   9、向申请人发通知。  
     
    其中最为重要的是CA自己的一对密钥的管理，它必须确保其高度的机密性,防止他方伪造证书。CA的公钥在网上公开,整个网络系统必须保证完整性。  
＊ 证书库  
    证书库是证书的集中存放地,它与网上"白页”类似,是网上的一种公共信息库,用户可以从此处获得其他用户的证书和公钥。  
构造证书库的最佳方法是采用支持LDAP协议的目录系统,用户或相关的应用通过LDAP来访问证书库。系统必须确保证书库的完整性,防止伪造、篡改证书。  
＊ 密钥备份及恢复系统  
    
＊ 证书作废处理系统  
    
＊ PKI应用接口系统  
PKI的价值在于使用户能够方便地使用加密、数字签名等安全服务，因此一个完整的PKI必须提供良好的应用接口系统，使得各种各样的应用能够以安全、一致、可信的方式与P KI交互，确保所建立起来的网络环境的可信性，同时降低管理维护成本。最后，PKI应用接口系统应该是跨平台的。
        
许多权威的认证方案供应商(例如VeriSign、Thawte以及GTE)目前都在提供外包的PKI。外包PKI最大的问题是,用户必须把企业托管给某一服务提供商, 即让出对网络安全的控制权。如果不愿这样做,则可建造一个专用的PKI。专用方案通常需把来自Entrust、Baltimore Technologies以及Xcert的多种服务器产品与来自主流应用程序供应商(如Microsoft、Netscape以及Qualcomm)的产品组合在一起。专用PK I还要求企业在准备其基础设施的过程中投入大量的财力与物力。
7．JAAS
扩展JAAS实现类实例级授权    
“Java 认证和授权服务”（Java Authentication and Authorization Service，JAAS）
在 JAAS 下，可以给予用户或服务特定的许可权来执行 Java 类中的代码。在本文中，软件工程师 Carlos Fonseca 向您展示如何为企业扩展 JAAS 框架。向 JAAS 框架添加类实例级授权和特定关系使您能够构建更动态、更灵活并且伸缩性更好的企业应用程序。
大多数 Java 应用程序都需要某种类实例级的访问控制。例如，基于 Web 的、自我服务的拍卖应用程序的规范可能有下列要求：
任何已注册（经过认证）的用户都可以创建一个拍卖，但只有创建拍卖的用户才可以修改这个拍卖。 
这意味着任何用户都可以执行被编写用来创建 Auction 类实例的代码，但只有拥有该实例的用户可以执行用来修改它的代码。通常情况下，创建 Auction 实例的用户就是所有者。这被称为类实例所有者关系（class instance owner relationship）。
该应用程序的另一个要求可能是：
任何用户都可以为拍卖创建一个投标，拍卖的所有者可以接受或拒绝任何投标。 
再一次，任何用户都可以执行被编写用来创建 Bid 类实例的代码，但只有拥有该实例的用户会被授予修改该实例的许可权。而且，Auction 类实例的所有者必须能够修改相关的 Bid 类实例中的接受标志。这意味着在 Auction 实例和相应的 Bid 实例之间有一种被称为特定关系（special relationship）的关系。
不幸的是，“Java 认证和授权服务”（JAAS）— 它是 Java 2 平台的一部分 — 没有考虑到类实例级访问控制或者特定关系。在本文中，我们将扩展 JAAS 框架使其同时包含这两者。推动这种扩展的动力是允许我们将访问控制分离到一个通用的框架，该框架使用基于所有权和特定关系的策略。然后管理员可以在应用程序的生命周期内更改这些策略。
在深入到扩展 JAAS 框架之前，我们将重温一下 Java 2 平台的访问控制机制。我们将讨论策略文件和许可权的使用，并讨论 SecurityManager 和 AccessController 之间的关系。 
Java 2 平台中的访问控制
在 Java 2 平台中，所有的代码，不管它是本地代码还是远程代码，都可以由策略来控制。策略（policy）由不同位置上的代码的一组许可权定义，或者由不同的签发者定义、或者由这两者定义。许可权允许对资源进行访问；它通过名称来定义，并且可能与某些操作关联在一起。
抽象类 java.security.Policy 被用于表示应用程序的安全性策略。缺省的实现由 sun.security.provider.PolicyFile 提供，在 sun.security.provider.PolicyFile 中，策略被定义在一个文件中。清单 1 是一个典型策略文件示例：
清单 1. 一个典型的策略文件
// Grant these permissions to code loaded from a sample.jar file
// in the C drive and if it is signed by XYZ
grant codebase "file:/C:/sample.jar", signedby "XYZ" {
// Allow socket actions to any host using port 8080
permission java.net.SocketPermission "*:8080", "accept, connect, 
  listen, resolve";
// Allows file access (read, write, execute, delete) in
// the user's home directory.
Permission java.io.FilePermission "${user.home}/-", "read, write, 
  execute, delete";
};
SecurityManager 对 AccessController
在标准 JDK 分发版中，控制代码源访问的机制缺省情况下是关闭的。在 Java 2 平台以前，对代码源的访问都是由 SecurityManager 类管理的。SecurityManager 是由 java.security.manager 系统属性启动的，如下所示：
java -Djava.security.manager
在 Java 2 平台中，可以将一个应用程序设置为使用 java.lang.SecurityManager 类或者 java.security.AccessController 类管理敏感的操作。AccessController 在 Java 2 平台中是新出现的。为便于向后兼容，SecurityManager 类仍然存在，但把自己的决定提交 AccessController 类裁决。SecurityManager 和 AccessController 都使用应用程序的策略文件确定是否允许一个被请求的操作。清单 2 显示了 AccessController 如何处理 SocketPermission 请求：
清单 2. 保护敏感操作
Public void someMethod() {
Permission permission = 
  new java.net.SocketPermission("localhost:8080", "connect");
AccessController.checkPermission(permission);
// Sensitive code starts here
Socket s = new Socket("localhost", 8080);
}
在这个示例中，我们看到 AccessController 检查应用程序的当前策略实现。如果策略文件中定义的任何许可权暗示了被请求的许可权，该方法将只简单地返回；否则抛出一个 AccessControlException 异常。在这个示例中，检查实际上是多余的，因为缺省套接字实现的构造函数也执行相同的检查。
在下一部分，我们将更仔细地看一下 AccessController 如何与 java.security.Policy 实现共同合作安全地处理应用程序请求。
运行中的 AccessController
AccessController 类典型的 checkPermission(Permission p) 方法调用可能会导致下面的一系列操作：
AccessController 调用 java.security.Policy 类实现的 getPermissions(CodeSource codeSource) 方法。
getPermissions(CodeSource codeSource) 方法返回一个 PermissionCollection 类实例，这个类实例代表一个相同类型许可权的集合。
AccessController 调用 PermissionCollection 类的 implies(Permission p) 方法。
接下来，PermissionCollection 调用集合中包含的单个 Permission 对象的 implies(Permission p) 方法。如果集合中的当前许可权对象暗示指定的许可权，则这些方法返回 true，否则返回 false。
现在，让我们更详细地看一下这个访问控制序列中的重要元素。
PermissionCollection 类
大多数许可权类类型都有一个相应的 PermissionCollection 类。这样一个集合的实例可以通过调用 Permission 子类实现定义的 newPermissionCollection() 方法来创建。java.security.Policy 类实现的 getPermissions() 方法也可以返回 Permissions 类实例 — PermissionCollection 的一个子类。这个类代表由 PermissionCollection 组织的不同类型许可权对象的一个集合。Permissions 类的 implies(Permission p) 方法可以调用单个 PermissionCollection 类的 implies(Permission p) 方法。
CodeSource 和 ProtectionDomain 类
许可权组合与 CodeSource（被用于验证签码（signed code）的代码位置和证书）被封装在 ProtectionDomain 类中。有相同许可权和相同 CodeSource 的类实例被放在相同的域中。带有相同许可权，但不同 CodeSource 的类被放在不同的域中。一个类只可属于一个 ProtectionDomain。要为对象获取 ProtectionDomain，请使用 java.lang.Class 类中定义的 getProtectionDomain() 方法。
许可权
赋予 CodeSource 许可权并不一定意味着允许所暗示的操作。要使操作成功完成，调用栈中的每个类必须有必需的许可权。换句话说，如果您将 java.io.FilePermission 赋给类 B，而类 B 是由类 A 来调用，那么类 A 必须也有相同的许可权或者暗示 java.io.FilePermission 的许可权。
在另一方面，调用类可能需要临时许可权来完成另一个拥有那些许可权的类中的操作。例如，当从另一个位置加载的类访问本地文件系统时，我们可能不信任它。但是，本地加载的类被授予对某个目录的读许可权。这些类可以实现 PrivilegedAction 接口来给予调用类许可权以便完成指定的操作。调用栈的检查在遇到 PrivilegedAction 实例时停止，有效地将执行指定操作所必需的许可权授予所有的后继类调用。
使用 JAAS
顾名思义，JAAS 由两个主要组件组成：认证和授权。我们主要关注扩展 JAAS 的授权组件，但开始我们先简要概述一下 JAAS 认证，紧接着看一下一个简单的 JAAS 授权操作。
JAAS 中的用户认证
JAAS 通过添加基于 subject 的策略加强了 Java 2 中定义的访问控制安全性模型。许可权的授予不仅基于 CodeSource，还基于执行代码的用户。显然，要使这个模型生效，每个用户都必须经过认证。
JAAS 的认证机制建立在一组可插登录模块的基础上。JAAS 分发版包含几个 LoginModule 实现。LoginModules 可以用于提示用户输入用户标识和密码。LoginContext 类使用一个配置文件来确定使用哪个 LoginModule 对用户进行认证。这个配置可以通过系统属性 java.security.auth.login.config 指定。一个示例配置是： 
java -Djava.security.auth.login.config=login.conf
下面是一个登录配置文件的样子：
Example {
  com.ibm.resource.security.auth.LoginModuleExample required 
    debug=true userFile="users.xml" groupFile="groups.xml";
};
认识您的主体
Subject 类被用于封装一个被认证实体（比如用户）的凭证。一个 Subject 可能拥有一个被称为主体（principal）的身份分组。例如，如果 Subject 是一个用户，用户的名字和相关的社会保险号可能是 Subject 的某些身份或主体。主体是与身份名关联在一起的。
Principal 实现类及其名称都是在 JAAS 策略文件中指定的。缺省的 JAAS 实现使用的策略文件与 Java 2 实现的策略文件相似 — 除了每个授权语句必须与至少一个主体关联在一起。javax.security.auth.Policy 抽象类被用于表示 JAAS 安全性策略。它的缺省实现由 com.sun.security.auth.PolicyFile 提供，在 com.sun.security.auth.PolicyFile 中策略定义在一个文件中。清单 3 是 JAAS 策略文件的一个示例：
清单 3. 示例 JAAS 策略文件
// Example grant entry
grant codeBase "file:/C:/sample.jar", signedby "XYZ",
  principal com.ibm.resource.security.auth.PrincipalExample "admin" {
    // Allow socket actions to any host using port 8080
    permission java.net.SocketPermission 
      "*:8080", "accept, connect, listen, resolve";
    // Allows file access (read, write, execute, delete) in
    // the user's home directory.
    Permission java.io.FilePermission 
      "${user.home}/-", "read, write, execute, delete";
};
这个示例与清单 1 中所示的标准 Java 2 策略文件相似。实际上，唯一的不同是主体语句，该语句声明只有拥有指定主体和主体名字的 subject（用户）被授予指定的许可权。
再一次，使用系统属性 java.security.auth.policy 指出 JAAS 策略文件驻留在何处，如下所示：
java -Djava.security.auth.policy=policy.jaas
Subject 类包含几个方法来作为特殊 subject 执行工作；这些方法如下所示：
public static Object 
  doAs(Subject subject, java.security.PrivilegedAction action)
public static Object 
  doAs(Subject subject, java.security.PrivilegedAction action)
  throws java.security.PrivilegedActionException
注意，用来保护敏感代码的方法与“Java 2 代码源访问控制”（Java 2 CodeSource Access Control）概述中描述的方法相同。请参阅参考资料部分以了解更多关于 JAAS 中代码源访问控制和认证的信息。
JAAS 中的授权
清单 4 显示一个授权请求的结果，该请求使用清单 3 中显示的 JAAS 策略文件。假设已经安装了 SecurityManager，并且 loginContext 已经认证了一个带有名为“admin”的 com.ibm.resource.security.auth.PrincipalExample 主体的 Subject。
清单 4. 一个简单的授权请求
public class JaasExample {
public static void main(String[] args) {
...
// where authenticatedUser is a Subject with
// a PrincipalExample named admin.
Subject.doAs(authenticatedUser, new JaasExampleAction());
...
}
}
public class JaasExampleAction implements PrivilegedAction {
public Object run() {
FileWriter fw = new FileWriter("hi.txt");
fw.write("Hello, World!");
fw.close();
}
}
这里，敏感代码被封装在 JaasExampleAction 类中。还要注意，调用类不要求为 JaasExampleAction 类代码源授予许可权，因为它实现了一个 PrivilegedAction。
扩展 JAAS
大多数应用程序都有定制逻辑，它授权用户不仅仅在类上执行操作，而且还在该类的实例上执行操作。这种授权通常建立在用户和实例之间的关系上。这是 JAAS 的一个小缺点。然而，幸运的是，这样设计 JAAS 使得 JAAS 可以扩展。只要做一点工作，我们将可以扩展 JAAS，使其包含一个通用的、类实例级的授权框架。
在文章开头处我已经说明了，抽象类 javax.security.auth.Policy 被用于代表 JAAS 安全性策略。它的缺省实现是由 com.sun.security.auth.PolicyFile 类提供。PolicyFile 类从 JAAS 格式的文件（象清单 3 中显示的那个一样）中读取策略。
我们需要向这个文件添加一个东西为类实例级授权扩展策略定义：一个与许可权语句相关的可选关系参数。
缺省 JAAS 许可权语句的格式如下：
permission <permission implementation class>; [name], [actions];
我们在这个许可权语句的末尾添加一个可选的关系参数来完成策略定义。下面是新许可权语句的格式：
permission <permission implementation class>; 
  [name], [actions], [relationship];
在为类实例级授权扩展 JAAS 时要注意的最重要的一点是：许可权实现类必须有一个带三个参数的构造函数。第一个参数是名称参数，第二个是行为参数，最后一个是关系参数。
解析新文件格式
既然文件格式已经改变，就需要一个新的 javax.security.auth.Policy 子类来解析文件。
为简单起见，我们的示例使用了一个新的 javax.security.auth.Policy 子类 com.ibm.resource.security.auth.XMLPolicyFile，来从 XML 文件读取策略。在实际的企业应用程序中，关系数据库更适合执行这个任务。
使用 XMLPolicyFile 类代替缺省的 JAAS 访问控制策略实现的最容易的方法是向 java.security 属性文件添加 auth.policy.provider=com.ibm.resource.security.auth.XMLPolicyFile 条目。java.security 属性文件位于 Java 2 平台运行时的 lib/security 目录下。清单 5 是与 XMLPolicyFile 类一起使用的样本 XML 策略文件：
清单 5. 一个 XML 策略文件
<?xml version="1.0"?>;
<policy>;
    <grant codebase="file:/D:/sample_actions.jar">;
      <principal classname=
        "com.ibm.resource.security.auth.PrincipalExample" name="users">;
        <permission classname=
          "com.ibm.resource.security.auth.ResourcePermission"
          name="com.ibm.security.sample.Auction"
          actions="create" />;
        <permission classname=
         "com.ibm.resource.security.auth.ResourcePermission"
          name="com.ibm.security.sample.Auction"
          actions="read" />;
        <permission classname=
         "com.ibm.resource.security.auth.ResourcePermission"
          name="com.ibm.security.sample.Auction"
          actions="write"
          relationship="owner" />;
        <permission classname=
         "com.ibm.resource.security.auth.ResourcePermission"
          name="com.ibm.security.sample.Bid"
          actions="create" />;
        <permission classname=
         "com.ibm.resource.security.auth.ResourcePermission"
          name="com.ibm.security.sample.Bid"
          actions="read" />;
        <permission classname=
         "com.ibm.resource.security.auth.ResourcePermission"
          name="com.ibm.security.sample.Bid"
          actions="write"
          relationship="owner" />;
        <permission classname=
         "com.ibm.resource.security.auth.ResourcePermission"
          name="com.ibm.security.sample.Bid"
          actions="accept"
          relationship="actionOwner" />;
    </principal>;
  </grant>;
</policy>;
在这个示例策略文件中，任何与名为 PrincipalExample 的用户有关的用户（Subject）都可以创建并读取一个 Auction.class 实例。但是，只有创建该实例的用户才可以更新（写）它。这是第三个 permission 元素定义的，该元素包含值为 owner 的 relationship 属性。Bid.class 实例也是一样，除了相应 Auction.class 实例的所有者可以更改投标接受标志。
Resource 接口
要求类实例级访问控制的类必须实现 Resource 接口。该接口的 getOwner() 方法返回类实例的所有者。fulfills(Subject subject, String relationship) 方法被用于处理特定关系。另外，这些类使用 com.ibm.resource.security.auth.ResourcePermission 类保护敏感代码。例如，Auction 类拥有下列构造函数：
public Auction() {
  Permission permission = 
    new ResourcePermission("com.ibm.security.sample.Auction", "create");
  AccessController.checkPermission(permission);
}
所有者关系
ResourcePermission 类的 implies(Permission p) 方法是这个框架的关键。implies() 方法就等同性比较名称和行为属性。如果定义了一个关系，那么必须把受保护的类实例（Resource）传递到 ResourcePermission 构造函数中。ResourcePermission 类理解所有者关系。它将类实例的所有者与执行代码的 subject（用户）进行比较。特定关系被委托给受保护类的 fulfills() 方法。
例如，在清单 5 中所示的 XML 策略文件中，只有 Auction 类实例的所有者可以更新（写）文件。该类的 setter 方法使用清单 6 中显示的保护代码：
清单 6. 运行中的 implies(Permission) 方法
public void setName(String newName) {
  Permission permission = 
    new ResourcePermission("com.ibm.security.sample.Auction", "write", this);
  AccessController.checkPermission(permission);
  // sensitive code
  this.name = newName;
}
被传递到 ResourcePermission 构造函数中的 this 引用代表 Auction 类实现的 Resource 接口。由于策略文件中列出的关系是 owner，所以 ResourcePermission 类使用这个引用检查当前 Subject（用户）是否拥有与实例所有者相匹配的主体。如果指定了另一个关系，那么 ResourcePermission 类调用 Auction 类的 fulfills(Subject subject, String relationship) 方法。由 Resource 实现类提供 fulfills() 方法中的逻辑。
XML 策略文件中列出的 Bid 类拥有清单 7 中所示的方法（假设 Bid 类实例有一个对相应 Auction 类实例的引用 — auction）。
清单 7. 处理特定关系
public void setAccepted(boolean flag) {
  Permission permission = 
    new ResourcePermission("com.ibm.security.sample.Auction", "accept", this);
  AccessController.checkPermission(permission);
  // sensitive code
  this.accepted = flag;
  }
public boolean fulfills(Subject user, String relationship) {
  if( relationship.equalsIgnoreCase("auctionOwner") ) {
    String auctionOwner = auction.getOwner();
    Iterator principalIterator = user.getPrincipals().iterator();
    while(principalIterator.hasNext()) {
      Principal principal = (Principal) principalIterator.next();
      if( principal.getName().equals(auctionOwner) )
        return true;
    }
  }
  return false;
}
传递到 fulfills() 方法中的关系字符串是策略文件中列出的关系。在这个案例中，我们使用了“auctionOwner”字符串。
缺省情况下，XMLPolicyFile 类在当前工作目录中查找名为 ResourcePolicy.xml 的文件。系统属性 com.ibm.resource.security.auth.policy 可以用于指定另一个不同的文件名和位置。
WebSphere Application Server 示例
除命令行示例之外，您可能还想运行这个简单的程序，该程序为了 IBM WebSphere Application Server，version 4.0.2 而被优化。
 
一个可运行的示例
综合这些信息，我们将运行一个简单的命令行示例。该示例程序包含三个 jar 文件：
resourceSecurity.jar 
example.jar 
exampleActions.jar 
resourceSecurity.jar 文件包含允许实例级访问控制的 JAAS 扩展框架。它还包含一个 LoginModuleExample 类，这个类从 XML 文件读取用户认证信息。用户标识和密码存储在 users.xml 文件中。用户组存储在 groups.xml 文件中。关于 LoginModuleExample 的更多信息，请参阅参考资料部分。
该示例包含四个附加的文件：
login.conf 
policy 
resourcePolicy.xml 
run.bat 
在试图运行这个示例程序之前，请确保更新了 run.bat、policy 和 resourcePolicy.xml 文件中的路径。缺省情况下，所有的密码都是“passw0rd”。 
示例如何工作
该示例程序提示输入用户标识和密码。它用 users.xml 文件中的条目核对所提供的用户标识和密码。在认证了用户之后，程序设法创建一个 UserProfile 类实例，修改它并从中读取。缺省情况下，UserProfile 类的所有者是 Jane（jane）。当 Jane 登录时，三个操作全部成功。当 John（john）登录时，只有创建操作成功。当 Jane 的经理 Lou（lou）登录时，只有第一个和最后一个操作成功。当系统管理员（admin）登录时，操作全部成功。当然，只有当提供的 ResourcePolicy.xml 文件未被修改时，上述这些才都是真的。
示例安装 
下面的安装指导假设您正在使用 JDK 1.3 并且已经把文件解压缩到 d:\JaasExample 目录。通过将文件解压缩到这个目录，您可以省去一些工作；否则您就必须使用正确的路径名修改 policy 和 ResourceSecurity.xml 策略文件。
下面是运行该示例需要做的工作：
下载这个示例的源文件。
把 jaas.jar 和 jaasmod.jar 复制到 JDK jre\lib\ext 目录（即 D:\JDK1.3\jre\lib\ext）。
向位于 JDK 的 jre\lib\security 目录（即 D:\JDK1.3\jre\lib\security）中的 java.security 文件的末尾添加下面的字符串：auth.policy.provider=com.ibm.resource.security.auth.XMLPolicyFile。 
执行 run.bat 文件。 
结束语
类实例级授权把访问控制分离到一个通用框架（该框架使用基于所有权和特定关系的策略）中。然后管理员可以在应用程序的生命周期内更改这些策略。用这种方法扩展 JAAS 减少了您或另一个程序员必须在应用程序生命周期内业务规则发生更改时重写代码的可能性。
通过将关系字符串抽象为类可以进一步扩展特定关系这个概念。不调用 Resource 实现类的 fulfills(Subject user, String relationship) 方法，而只要调用 Relationship 实现类中定义的新 fulfills(Subject user, Resource resource) 方法。这样就会允许许多 Resource 实现类使用相同的关系逻辑。
6．Java的安全性
1. the security manager是一个application-wide object ( java.lang.SecurityManager)
每个Java Application都可以有自己地Security Manager,但是默认地Java Application没有一个Security Manager
可以通过下面地代码得到一个Security Manager
try
{
  System.setSecurityManager(new SecurityManager(“--”));
}
catch( )
{}
2.
JDBC
在 JDBC 2 开发的过程中，SQL99 还处在一种变化不定的情况下。现在规范已经完成了，而且数据库厂商已经采用了部分标准。所以自然地，JDBC 规范就跟着将自己与 SQL99 功能的一部分相统一。最新的 JDBC 规范已经采用了 SQL99 标准中那些已经被广泛支持的功能，还有那些在五年内可能会获得支持的功能。
1. DataSource
在JDBC2.0 Optional Package中，提供了透明的连接池（Connection pooling）。
一旦配置了J2EE应用服务器后，只要用DataSource获取连接(Connection)，连接池（Connection pooling）就会自动的工作。
如果用户希望建立一个数据库连接，通过查询在JNDI服务中的DataSource，可以从DataSource中获取相应的数据库连接。
DataSource被认为是从JNDI中获取的网络资源。
DataSource在池中保存的对象都实现了PooledConnection接口。
当应用程序向DataSource请求一个Connection时，它会找到一个可用的PooledConnection对象。
如果连接池空了，它就向ConnectionPoolecDataSource请求一个新的PooledConnection对象
通过使用 DataSource 接口 (JDBC 2.0) 或 DriverManager (JDBC 1.0) 接口，J2EE 组件可以获得物理数据库连接对象（Connection）。要获得逻辑（合用的）连接，J2EE 组件必须使用以下这些 JDBC 2.0 合用管理器接口：
javax.sql.ConnectionPoolDataSource 接口，该接口充当合用的 java.sql.Connection 对象的资源管理器连接 factory。每家数据库服务器供应商都提供该接口的实现
（例如，Oracle 实现 oracle.jdbc.pool.OracleConnectionPoolDataSource 类）。 
javax.sql.PooledConnection 接口，该接口封装到数据库的物理连接。同样，数据库供应商提供其实现。 
对于那些接口和 XA 连接的每一个，都存在一个 XA（X/Open 规范）等价定义。
2. ResultSet
在JDBC2.0中，为了获得一个Uptatable Result，在Query语句里必须包含Primarykey，并且查询的内容里必须来自一个table
ava.sql.ResultSet接口中定义了三种类型的结果集
TYPE_FORWARD_ONLY
TYPE_SCROLL_INSENSITIVE   这种类型的结果集支持双向滚动
TYPE_SCROLL_SENSITIVE
如果要建立一个双向滚动的ResultSet，一定要在建立Statement的时候使用如下参数
Statement stmt = conn.createStatement(ResultSet.TYPE_SCROLL_INSENSITIVE,
ResultSet.CONCUR_READ_ONLY);
3. JDBC驱动程序
连通oracle8.1.6的JDBC
把oracle8.1.6/lib/jdbc/*.zip copy 到 %JAVA_HOME%/jre/lib/ext/*.jar
如果光copy不ren为.jar是没有用的。
4. 事务处理
本地事务
  java.sql.Connection接口可以控制事务边界（即开始和结束）。
在事务开始的时候调用setAutoCommit( false ), 而在中止事务时调用rollback或commit()方法。这类事务叫本地事务。
分布式事务
 但是，在特定的情况下，可能有多个客户（例如两个不同的servlet或EJB组件）参与了同一个事务。
 或者，客户在同一个事务中可能会执行跨越多个数据库的数据库操作。
 JDBC2.0 Optional Package 同JTA一起来实现分布式样事务。
5. 一些技巧
检索自动产生的关键字
为了解决对获取自动产生的或自动增加的关键字的值的需求，JDBC 3.0 API 现在将获取这种值变得很轻松。要确定任何所产生的关键字的值，只要简单地在语句的 execute() 方法中指定一个可选的标记，表示您有兴趣获取产生的值。您感兴趣的程度可以是 Statement.RETURN_GENERATED_KEYS，也可以是 Statement.NO_GENERATED_KEYS。在执行这条语句后，所产生的关键字的值就会通过从 Statement 的实例方法 getGeneratedKeys() 来检索 ResultSet 而获得。ResultSet 包含了每个所产生的关键字的列。清单 1 中的示例创建一个新的作者并返回对应的自动产生的关键字。
清单 1. 检索自动产生的关键字
Statement stmt = conn.createStatement();
// Obtain the generated key that results from the query.
stmt.executeUpdate("INSERT INTO authors " +
           '(first_name, last_name) " +
           "VALUES ('George', 'Orwell')",
           Statement.RETURN_GENERATED_KEYS);
ResultSet rs = stmt.getGeneratedKeys();
if ( rs.next() ) {
    // Retrieve the auto generated key(s).
    int key = rs.getInt();
}
JTA/JTS
1．JTA/JTS基本知识
服务器实现JTS是否对应用程序开发人员来说不是很重要的。
对你来说，应该把JTA看作是可用的API。
JTA是用来开发distributed tansaction的 API.
而JTS定义了支持JTA中实现Transaction Manager 的规范。
JavaTransaction Service (JTS) specifies the implementation of a Transaction Manager which supports the Java Transaction API (JTA) 1.0 Specification at the high-level and implements the Java mapping of the OMG Object Transaction Service (OTS) 1.1 Specification at the low-level. JTS uses the standard CORBA ORB/TS interfaces and Internet Inter-ORB Protocol (IIOP) for transaction context propagation between JTS Transaction Managers. 
A JTS Transaction Manager provides transaction services to the parties involved in distributed transactions: the application server, the resource manager, the standalone transactional application, and the Communication Resource Manager (CRM).
2．JTA
1.1 事务处理的概念
JTA实际上是由两部分组成的：一个高级的事务性客户接口和一个低级的 X/Open XA接口。
我们关心的是高级客户接口，因为bean可以访问它，而且是推荐的客户应用程序的事务性接口。
低级的XA接口是由EJB服务器和容器使用来自动协调事务和资源（如数据库）的
1.1.1事务划分
a．程序划分
  使用UserTransaction启动JTA事务
  The UserTransaction interface defines the methods that allow an application to explicitly manage transaction boundaries.（from j2ee API document）
b．声明划分
  EJB容器使用TransactionManager启动JTA事务
  The TransactionManager interface defines the methods that allow an application server to manage transaction boundaries. （from j2ee API document）
1.1.2事务上下文及其传播
事务上下文是一种对资源上的事务操作之间和调用操作的组件之间的联系。
1.1.3资源加入
资源加入（resource enlistment）是一个过程，在这个过程中资源管理器通知事务管理器它要参与事务。
1.1.4两阶段提交
两阶段提交是事务管理器和所有加入到事务中的资源之间的协议，确保要么所有的资源管理器都提交了事务，要么都撤销了事务。
如果在一个事务内部只是访问一个单一资源管理器，不需要执行一个两阶段提交。
如果在一个事务内部只是访问多个资源管理器，两阶段提交是有益的。
1.2事务处理系统中的构件模块
应用组件
资源管理器
  资源管理器管理持久和稳定的数据存储系统，并且与事务管理器一起参与两阶段提交和恢复协议。典型的资源管理器如数据库系统和消息队列。
事务管理器
3．JTS
JTS 是一个组件事务监视器（component transaction monitor）。
这是什么意思？在第 1 部分，我们介绍了事务处理监视器（TPM）这个概念，TPM 是一个程序，它代表应用程序协调分布式事务的执行。
TPM 与数据库出现的时间长短差不多；在 60 年代后期，IBM 首先开发了 CICS，至今人们仍在使用。经典的（或者说程序化）TPM 管理被程序化定义为针对事务性资源（比如数据库）的操作序列的事务。随着分布式对象协议，如 CORBA、DCOM 和 RMI 的出现，人们希望看到事务更面向对象的前景。将事务性语义告知面向对象的组件要求对 TPM 模型进行扩展 — 在这个模型中事务是按照事务性对象的调用方法定义的。
JTS 只是一个组件事务监视器（有时也称为对象事务监视器（object transaction monitor）），或称为 CTM。
JTS 和 J2EE 的事务支持设计受 CORBA 对象事务服务（CORBA Object Transaction Service，OTS）的影响很大。实际上，JTS 实现 OTS 并充当 Java 事务 API（Java Transaction API）— 一种用来定义事务边界的低级 API — 和 OTS 之间的接口。使用 OTS 代替创建一个新对象事务协议遵循了现有标准，并使 J2EE 和 CORBA 能够互相兼容。
乍一看，从程序化事务监视器到 CTM 的转变好像只是术语名称改变了一下。然而，差别不止这一点。当 CTM 中的事务提交或回滚时，与事务相关的对象所做的全部更改都一起被提交或取消。但 CTM 怎么知道对象在事务期间做了什么事？象 EJB 组件之类的事务性组件并没有 commit() 或 rollback() 方法，它们也没向事务监视器注册自己做了什么事。那么 J2EE 组件执行的操作如何变成事务的一部分呢？
透明的资源征用
当应用程序状态被组件操纵时，它仍然存储在事务性资源管理器（例如，数据库和消息队列服务器）中，这些事务性资源管理器可以注册为分布式事务中的资源管理器。在第 1 部分中，我们讨论了如何在单个事务中征用多个资源管理器，事务管理器如何协调这些资源管理器。资源管理器知道如何把应用程序状态中的变化与特定的事务关联起来。
但这只是把问题的焦点从组件转移到了资源管理器 — 容器如何断定什么资源与该事务有关，可以供它征用？请考虑下面的代码，在典型的 EJB 会话 bean 中您可能会发现这样的代码：
清单 1. bean 管理的事务的透明资源征用 
   InitialContext ic = new InitialContext();
   UserTransaction ut = ejbContext.getUserTransaction();
   ut.begin();
   DataSource db1 = (DataSource) ic.lookup("java:comp/env/OrdersDB");
   DataSource db2 = (DataSource) ic.lookup("java:comp/env/InventoryDB");
   Connection con1 = db1.getConnection();
   Connection con2 = db2.getConnection();
   // perform updates to OrdersDB using connection con1
   // perform updates to InventoryDB using connection con2
   ut.commit();
注意，这个示例中没有征用当前事务中 JDBC 连接的代码 — 容器会为我们完成这个任务。我们来看一下它是如何发生的。
资源管理器的三种类型
当一个 EJB 组件想访问数据库、消息队列服务器或者其它一些事务性资源时，它需要到资源管理器的连接（通常是使用 JNDI）。而且，J2EE 规范只认可三种类型的事务性资源 — JDBC 数据库、JMS 消息队列服务器和“其它通过 JCA 访问的事务性服务”。后面一种服务（比如 ERP 系统）必须通过 JCA（J2EE Connector Architecture，J2EE 连接器体系结构）访问。对于这些类型资源中的每一种，容器或提供者都会帮我们把资源征调到事务中。
在清单 1 中，con1 和 con2 好象是普通的 JDBC 连接，比如那些从 DriverManager.getConnection() 返回的连接。我们从一个 JDBC DataSource 得到这些连接，JDBC DataSource 可以通过查找 JNDI 中的数据源名称得到。EJB 组件中被用来查找数据源（java:comp/env/OrdersDB）的名称是特定于组件的；组件的部署描述符的 resource-ref 部分将其映射为容器管理的一些应用程序级 DataSource 的 JNDI 名称。
隐藏的 JDBC 驱动器
每个 J2EE 容器都可以创建有事务意识的池态 DataSource 对象，但 J2EE 规范并不向您展示如何创建，因为这不在 J2EE 规范内。浏览 J2EE 文档时，?br>
