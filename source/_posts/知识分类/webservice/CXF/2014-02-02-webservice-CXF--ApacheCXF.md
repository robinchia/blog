---
layout: post
title: "Apache CXF"
categories: webservice
tags: 
 - webservice
 - CXF
--- 

# Apache CXF

CXF是基于JAX-WS实现的，JAX-WS规范是一组XML web services的JAVA API，它使用户无需编写复杂的SOAP ENV,WSDL。在 JAX-WS中，一个远程调用可以转换为一个基于XML的协议例如SOAP。在使用JAX-WS过程中，开发者不需要编写任何生成和处理SOAP消息的代码。JAX-WS的运行时实现会将这些API的调用转换成为对于SOAP消息。

在服务器端，用户只需要通过Java语言定义远程调用所需要实现的接口SEI (service endpoint interface)，并提供相关的实现，通过调用JAX-WS的服务发布接口就可以将其发布为WebService接口。 
在客户端，用户可以通过JAX-WS的API创建一个代理（用本地对象来替代远程的服务）来实现对于远程服务器端的调用。
来源： <[http://blog.csdn.net/chjttony/article/details/6196289](http://blog.csdn.net/chjttony/article/details/6196289)> 

一、与Axis2的不同之处
1、Apache CXF 支持 WS-Addressing、WS-Policy、WS-RM、WS-Security和WS-I BasicProfile 
2、Axis2 支持 WS-Addressing、WS-RM、WS-Security和WS-I BasicProfile，WS-Policy将在新版本里得到支持 
3、Apache CXF 是根据Spring哲学来进行编写的，即可以无缝地与Spring进行整合 
4、Axis2 不是 
5、Axis2 支持更多的 data bindings，包括 XMLBeans、JiBX、JaxMe 和 JaxBRI，以及它原生的 data binding（ADB）。 
6、Apache CXF 目前仅支持 JAXB 和 Aegis，并且默认是 JAXB 2.0，与 XFire 默认是支持 Aegis 不同，XMLBeans、JiBX 和 Castor 将在 CXF 2.1 版本中得到支持，目前版本是 2.0.2 
7、Axis2 支持多种语言，它有 C/C++ 版本。 
8、Apache CXF 提供方便的Spring整合方法，可以通过注解、Spring标签式配置来暴露Web Services和消费Web Services

 
二、A simple JAX-WS service
原文见[http://cwiki.apache.org/CXF20DOC/a-simple-jax-ws-service.html](http://cwiki.apache.org/CXF20DOC/a-simple-jax-ws-service.html)

 来源： [http://www.iteye.com/topic/143877](http://www.iteye.com/topic/143877)
CXF旨在为服务创建必要的基础设施，它的整体架构主要由以下几个部分组成：

1.Bus

它是C X F架构的主干，为共享资源提供了一个可配置的场所，作用非常类似于S p r i n g的ApplicationContext。这些共享资源包括WSDL管理器、绑定工厂等。通过对Bus进行扩展，可以方便地容纳自己的资源，或替换现有的资源。默认Bus实现是基于Spring的，通过依赖注入，将运行时组件串起来。Bus的创建由BusFactory负责，默认是 SpringBusFactory，对应于默认Bus实现。在构造过程中，SpringBusFactory会搜索META-INF/cxf（就包含在 CXF的Jar中）下的所有Bean配置文件，根据它们构建一个ApplicationContext。开发者也可提供自己的配置文件来定制Bus。

2.消息传递和拦截器（Interceptor）

CXF建立于一个通用的消息层之上，主要由消息、拦截器和拦截器链（InterceptorChain）组成。CXF是以消息处理为中心的，熟悉 JSP/Servlet的开发者可以将拦截器视为CXF架构中的“Filter”，拦截器链也与“FilterChain”类似。通过拦截器，开发者可以方便地在消息传递、处理的整个过程中对CXF进行扩展。拦截器的方法主要有两个：handleMessage和handleFault，分别对应消息处理和错误处理。在开发拦截器的时候需要注意两点：

拦截器不是线程安全的，不建议在拦截器中定义实例变量并使用它。这一点跟JSP/Servlet中对于Filter的处理是一样的；

不要调用下一个拦截器的handleMessage或handleFault，这个工作由InterceptorChain来完成。

3.前端（Front End）

它为CXF提供了创建服务的编程模型，当前主要的前端就是JAX-WS。

4.服务模型

CXF中的服务通过服务模型来表示。它主要有两部分：ServiceInfo和服务本身。ServiceInfo作用类似WSDL，包含接口信息、绑定、端点（EndPoint）等信息；服务则包含了ServiceInfo、数据绑定、拦截器和服务属性等信息。可使用Java类和WSDL来创建服务。一般是由前端负责服务的创建，它通过ServiceFactory来完成。

5.绑定（Binding）

绑定提供了在传输之上映射具体格式和协议的方法，主要的两个类是Binding和BindingFactory。BindingFactory负责创建Binding。

6.传输（Transport）

为了向绑定和前端屏蔽传输细节，CXF提供了自己的传输抽象。其中主要有两个对象：Conduit和Destination。前者是消息发送的基础，后者则对应消息接收。开发者还可以给Conduit和Destination注册MessageObserver，以便在消息发送和接收时获得通知。

**开发方法**

CXF 可以创建的Web 服务应用有两种：服务提供者和服务消费者。这种结构可类比客户端/ 服务器结构，服务消费者类似于客户端，服务提供者类似于服务器。使用CXF 创建应用时，服务提供者和服务消费者并不需要同时出现，因为有可能应用只是作为服务提供者或服务消费者单独出现。
来源： <[http://blog.csdn.net/jacklee_6297/article/details/5888232](http://blog.csdn.net/jacklee_6297/article/details/5888232)>** 开发Webservice工程步骤：**

2.使用CXF开发Webservice工程步骤： 

1).为CXF设置编译和开发环境 
在http://cxf.apache.org/download.html 下载相应的CXF包，/lib目录下的jar 文件引入工程 
2).创建基于XCF的Webservice服务端工程。
3).编写Webservice的客户端程序，调用服务端服务。

3.CXF中的Factory：

CXF不但可以使用JAX-WS开发web服务，也可以将POJO发布为web服务，对于这两种不同的方式，对应的factory如下：

                                 服务端                               客户端  

JAX-WS                      JaxWsServerFactoryBean             JaxWsProxyFactoryBean

POJO                           ServiceFactoryBean                     ClientProxyFactoryBean

4.CXF使用JAX-WS开发服务端：

(1).定义服务接口：

在接口上添加Webservice注解：@WebService。如：

**[java]** [view plain](http://blog.csdn.net/chjttony/article/details/6196289# "view plain")[copy](http://blog.csdn.net/chjttony/article/details/6196289# "copy")

1. package service;  
1. import javax.jws.WebService;  
1. @WebService  
1. public interface OrderProcess {  
1.     String processOrder(Order order);  
1. }  

 

(2).实现服务接口：

在实现类上也添加Webservice注解：@WebService(endpointInterface = 服务接口全路径,   serviceName = 对外发布的服务名)。如：

**[java]** [view plain](http://blog.csdn.net/chjttony/article/details/6196289# "view plain")[copy](http://blog.csdn.net/chjttony/article/details/6196289# "copy")

1. package service;  
1. import javax.jws.WebService;  
1. @WebService(endpointInterface = "service.OrderProcess"，serviceName=”order”)  
1. public class OrderProcessImpl implements OrderProcess {  
1.     public String processOrder(Order order) {  
1.         return "hello world"+order;  
1.     }  
1. }  

 

(3).对外发布服务：

**[java]** [view plain](http://blog.csdn.net/chjttony/article/details/6196289# "view plain")[copy](http://blog.csdn.net/chjttony/article/details/6196289# "copy")

1. //创建web服务工厂  
1. JaxWsServerFactoryBean factory = new JaxWsServerFactoryBean();  
1. //设置服务类  
1. factory.setServiceClass(服务接口实现类.class);  
1. //设置对外发布服务地址  
1. factory.setAddress(对外发布的服务地址);  
1. //创建服务  
1. Server server = factory.create();  
1. //启动服务  
1. server.start();  

 

5.CXF使用JAX-WS开发客户端：

**[java]** [view plain](http://blog.csdn.net/chjttony/article/details/6196289# "view plain")[copy](http://blog.csdn.net/chjttony/article/details/6196289# "copy")

1. //创建web服务代理工厂  
1. JaxWsProxyFactoryBean factory = new JaxWsProxyFactoryBean();  
1. //设置要调用的web服务服务端发布地址  
1. factory.setAddress(web服务的发布地址);  
1. //设置要调用的web服务  
1. factory.setServiceClass(web服务接口.class);  
1. //创建web服务对象  
1. 服务接口 对象 = (服务接口) factory.create();  
1. 通过对象调用web服务的方法  
1. 6.CXF使用POJO开发服务端：  
1. 和使用JAX-WS开发方式前两步完全一样，第三步稍有不同如下：  
1. //创建web服务工厂  
1. ServiceFactoryBean svrFactory = new ServiceFactoryBean();  
1. //设置服务类  
1. svrFactory.setServiceClass(服务接口实现类.class);  
1. //设置对外发布服务地址  
1. svrFactory.setAddress(对外发布的服务地址);  
1. //创建服务  
1. Server server = svrFactory.create();  
1. //启动服务  
1. server.start();  

 

6.CXF使用POJO开发客户端：

和JAX-WS方式除了代理工厂不同以外，其他均相同：

**[java]** [view plain](http://blog.csdn.net/chjttony/article/details/6196289# "view plain")[copy](http://blog.csdn.net/chjttony/article/details/6196289# "copy")

1. //创建web服务代理工厂  
1. ClientProxyFactoryBean factory = new ClientProxyFactoryBean();  
1. //设置要调用的web服务服务端发布地址  
1. factory.setAddress(web服务的发布地址);  
1. //设置要调用的web服务  
1. factory.setServiceClass(web服务接口.class);  
1. //创建web服务对象  
1. 服务接口 对象 = (服务接口) factory.create();  

 

通过对象调用web服务的方法

7.CXF与Spring的集成：

(1).对工程引入spring支持。

(2).在web.xml文件中添加spring和CXF相应的配置如下：

**[xhtml]** [view plain](http://blog.csdn.net/chjttony/article/details/6196289# "view plain")[copy](http://blog.csdn.net/chjttony/article/details/6196289# "copy")

1. <web-app>  
1.   <context-param>  
1.   <param-name>contextConfigLocation</param-name>  
1.   <param-value>spring配置文件路径</param-value>  
1.  </context-param>  
1.  <listener>  
1.   <listener-class>  
1.    org.springframework.web.context.ContextLoaderListener  
1.   </listener-class>  
1.  </listener>  
1.  <servlet>  
1.   <servlet-name>CXFServlet</servlet-name>  
1.   <display-name>CXF Servlet</display-name>  
1.   <servlet-class>  
1.    org.apache.cxf.transport.servlet.CXFServlet  
1.   </servlet-class>  
1.   <load-on-startup>1</load-on-startup>  
1.  </servlet>  
1.  <servlet-mapping>  
1.   <servlet-name>CXFServlet</servlet-name>  
1.   <url-pattern>/service/*</url-pattern>  
1.  </servlet-mapping>  
1. </web-app>  

 

(3).在spring配置文件中导入CXF的相关配置如下：

**[xhtml]** [view plain](http://blog.csdn.net/chjttony/article/details/6196289# "view plain")[copy](http://blog.csdn.net/chjttony/article/details/6196289# "copy")

1. <import resource="classpath:META-INF/cxf/cxf.xml" />  
1. <import resource="classpath:META-INF/cxf/cxf-extension-soap.xml" />  
1. <import resource="classpath:META-INF/cxf/cxf-servlet.xml" />  

 

(4).在spring配置文件中配置要发布的web服务如下：

**[xhtml]** [view plain](http://blog.csdn.net/chjttony/article/details/6196289# "view plain")[copy](http://blog.csdn.net/chjttony/article/details/6196289# "copy")

1. <jaxws:endpoint  
1.    id="……"  
1.    implementor="服务接口实现类全路径"  
1.    address="/web服务发布地址(相对地址)" />  

 来源：[http://blog.csdn.net/chjttony/article/details/6196289](http://blog.csdn.net/chjttony/article/details/6196289)

**拦截器方式的使用实例：**

基于CXF2.0前2个学习笔记，对原先服务端与客户端进行修改，实现在SOAP Header里面添加自定义的数据进行认证
在做之前，先要理解如下的信息
**拦截器(Interceptor)简单说明**
      Interceptor是CXF架构中一个很有特色的模式。你可以在不对核心模块进行修改的情况下，动态添加很多功能。这对于CXF这个以处理消息为中心的服务框架来说是非常有用的，CXF通过在Interceptor中对消息进行特殊处理，实现了很多重要功能模块，例如：日志记录，Soap消息处理，消息的压缩处。简单的说，可以在收到请求后，还未进行业务处理前，进行处理。或者在请求包发送前，进行报文的处理。
**几个的API的介绍**
**Interceptor**

定义两个方法，一个处理消息 handleMessage， 一个是处理错误 handleFault。

**InterceptorChain
**  单个的Interceptor功能有限，CXF要实现一个SOAP消息处理，需要将许许多多的Interceptor组合在一起使用。因此设计了 InterceptorChain，在我看了InterceptorChain就像是一个Interceptor的小队长。 小队长有调配安置Interceptor的权力（add，remove），也有控制消息处理的权力（doInterceptor，pause，resume，reset，abort），同时也有交付错误处理的权力（ {get|set}FaultObserver）。更有意思的是为灵活控制Interceptor的处理消息顺序（doInterceptStartingAt，doInterceptorStartingAfter），这也是InterceptorChain比较难理解的地方。
**Fault**
  定义了CXF中的错误消息。
**InterceptorProvider**
这里定义了Interceptor的后备保障部队。我们可以在InterceptorProvider中设置In，Out，InFault，OutFault 后备小分队，添加我们所希望添加的Interceptor。而InterceptorChain会根据这些后备小分队，组建自己的小分队实例，完成具体的作战功能任务。
**AbstractAttributedInterceptorProvider**
   InterceptorProvider实现的抽象类，由于这个类来继承了HashMap，我们可以像这个类中存储一些属性信息。
**AbstractBasicInterceptorProvider**
   与AbstractAttributedInterceptorProvider不同，这个Interceptor只是简单实现了InterceptorProvider的功能，并不提供对其属性存储的扩展。
**Message**
   由于Interceptor是针对Message来进行处理的，当你打开Message这个类文件时，你会发现在Message中定义了很多常量，同时你还可以从Message中获取到很多与Message操作相关的信息。可以获取设置的对象有InterceptorChain Exchange Destination，还有获取设置Content的泛型接口，是不是感觉Message和Bus差不多，都成了大杂货铺，一切与消息处理相关的信息都可以放在Message中。

理解了Interceptor功能，下面的修改就很简单了

**服务端修改**
1.新建一个拦截器(Interceptor)
![]()package hs.cxf.soapHeader;
![]()
![]()import javax.xml.soap.SOAPException;
![]()import javax.xml.soap.SOAPHeader;
![]()import javax.xml.soap.SOAPMessage;
![]()import org.apache.cxf.binding.soap.SoapMessage;
![]()import org.apache.cxf.binding.soap.saaj.SAAJInInterceptor;
![]()import org.apache.cxf.interceptor.Fault;
![]()import org.apache.cxf.phase.AbstractPhaseInterceptor;
![]()import org.apache.cxf.phase.Phase;
![]()import org.w3c.dom.NodeList;
![]()
![]()/**
![]() * 
![]() * @Title:获取soap头信息
![]() * 
![]() * @Description:
![]() * 
![]() * @Copyright:
![]() * 
![]() * @author zz
![]() * @version 1.00.000
![]() * 
![]() */
![]()public class ReadSoapHeader extends AbstractPhaseInterceptor<SoapMessage> {
![]()
![]()    private SAAJInInterceptor saa = new SAAJInInterceptor();
![]()
![]()    public ReadSoapHeader() {
![]()        super(Phase.PRE_PROTOCOL);
![]()        getAfter().add(SAAJInInterceptor.class.getName());
![]()    }
![]()
![]()    public void handleMessage(SoapMessage message) throws Fault {
![]()
![]()        SOAPMessage mess = message.getContent(SOAPMessage.class);
![]()        if (mess == null) {
![]()            saa.handleMessage(message);
![]()            mess = message.getContent(SOAPMessage.class);
![]()        }
![]()        SOAPHeader head = null;
![]()        try {
![]()            head = mess.getSOAPHeader();
![]()        } catch (SOAPException e) {
![]()            e.printStackTrace();
![]()        }
![]()        if (head == null) {
![]()            return;
![]()        }
![]()        try {
![]()            //读取自定义的节点
![]()            NodeList nodes = head.getElementsByTagName("tns:spId");
![]()            NodeList nodepass = head.getElementsByTagName("tns:spPassword");
![]()            //获取节点值，简单认证
![]()            if (nodes.item(0).getTextContent().equals("wdw")) {
![]()                if (nodepass.item(0).getTextContent().equals("wdwsb")) {
![]()                    System.out.println("认证成功");
![]()                }
![]()            } else {
![]()                SOAPException soapExc = new SOAPException("认证错误");
![]()                throw new Fault(soapExc);
![]()            }
![]()
![]()        } catch (Exception e) {
![]()            SOAPException soapExc = new SOAPException("认证错误");
![]()            throw new Fault(soapExc);
![]()        }
![]()    }
![]()
![]()}
2.配置文件中新增拦截器配置
![]()<beans xmlns="http://www.springframework.org/schema/beans"  
![]()    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
![]()    xmlns:jaxws="http://cxf.apache.org/jaxws"  
![]()    xsi:schemaLocation="   
![]()http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd   
![]()http://cxf.apache.org/jaxws http://cxf.apache.org/schemas/jaxws.xsd">   
![]()  
![]()    <import resource="classpath:META-INF/cxf/cxf.xml" />   
![]()    <import resource="classpath:META-INF/cxf/cxf-extension-soap.xml" />   
![]()    <import resource="classpath:META-INF/cxf/cxf-servlet.xml" />   
![]()  
![]()    <bean id="jaxWsServiceFactoryBean"  
![]()        class="org.apache.cxf.jaxws.support.JaxWsServiceFactoryBean">   
![]()        <property name="wrapped" value="true" />   
![]()        <property name="dataBinding" ref="aegisBean" />   
![]()    </bean>   
![]()  
![]()    <bean id="aegisBean"  
![]()        class="org.apache.cxf.aegis.databinding.AegisDatabinding" />   
![]()  
![]()    <jaxws:endpoint id="CollectiveServices"  
![]()        implementor="hs.cxf.server.WebServiceSampleImpl" address="/HelloWorld">   
![]()        <jaxws:inInterceptors>   
![]()          <!-- 日志拦截器 -->      
![]()          <bean class="org.apache.cxf.interceptor.LoggingOutInterceptor"/>   
![]()          <!-- 自定义拦截器 --> 
![]()          <bean class="hs.cxf.soapHeader.ReadSoapHeader"/>   
![]()          </jaxws:inInterceptors>    
![]()        <jaxws:serviceFactory>   
![]()            <ref bean="jaxWsServiceFactoryBean"/>   
![]()        </jaxws:serviceFactory>   
![]()    </jaxws:endpoint>   
![]()</beans>  
![]()
服务端的配置就告一段落了，接下来是客户端的修改
**客户端
**1.同样新增一个Interceptor
![]()package hs.cxf.client.SoapHeader;
![]()
![]()
![]()import java.util.List;
![]()import javax.xml.namespace.QName;
![]()import org.apache.cxf.binding.soap.SoapHeader;
![]()import org.apache.cxf.binding.soap.SoapMessage;
![]()import org.apache.cxf.binding.soap.interceptor.AbstractSoapInterceptor;
![]()import org.apache.cxf.headers.Header;
![]()import org.apache.cxf.helpers.DOMUtils;
![]()import org.apache.cxf.interceptor.Fault;
![]()import org.apache.cxf.phase.Phase;
![]()import org.w3c.dom.Document;
![]()import org.w3c.dom.Element;
![]()/**
![]() * 
![]() * @Title:在发送消息前，封装Soap Header 信息
![]() * 
![]() * @Description:
![]() * 
![]() * @Copyright: 
![]() *
![]() * @author zz
![]() * @version 1.00.000
![]() *
![]() */
![]()
![]()public class AddSoapHeader extends AbstractSoapInterceptor {
![]()      private static String nameURI="http://127.0.0.1:8080/cxfTest/ws/HelloWorld";   
![]()      
![]()        public AddSoapHeader(){   
![]()            super(Phase.WRITE);   
![]()        }   
![]()        
![]()        public void handleMessage(SoapMessage message) throws Fault {   
![]()            String spPassword="wdwsb";   
![]()            String spName="wdw";   
![]()               
![]()            QName qname=new QName("RequestSOAPHeader");   
![]()            Document doc=DOMUtils.createDocument();   
![]()            //自定义节点
![]()            Element spId=doc.createElement("tns:spId");   
![]()            spId.setTextContent(spName);   
![]()            //自定义节点
![]()            Element spPass=doc.createElement("tns:spPassword");   
![]()            spPass.setTextContent(spPassword);   
![]()               
![]()            Element root=doc.createElementNS(nameURI, "tns:RequestSOAPHeader");   
![]()            root.appendChild(spId);   
![]()            root.appendChild(spPass);   
![]()               
![]()            SoapHeader head=new SoapHeader(qname,root);   
![]()            List<Header> headers=message.getHeaders();   
![]()            headers.add(head);   
![]()            System.out.println(">>>>>添加header<<<<<<<");
![]()        }   
![]()
![]()}
![]()
2.客户端调用程序修改
![]()package hs.cxf.client;
![]()
![]()import hs.cxf.client.SoapHeader.AddSoapHeader;
![]()import java.util.ArrayList;
![]()import javax.xml.bind.JAXBElement;
![]()import javax.xml.namespace.QName;
![]()import org.apache.cxf.endpoint.Client;
![]()import org.apache.cxf.frontend.ClientProxy;
![]()import org.apache.cxf.interceptor.Interceptor;
![]()import org.apache.cxf.jaxws.JaxWsProxyFactoryBean;
![]()import org.apache.cxf.transport.http.HTTPConduit;
![]()import org.apache.cxf.transports.http.configuration.HTTPClientPolicy;
![]()
![]()/**
![]() * @Title:
![]() * 
![]() * @Description:
![]() * 
![]() * @Copyright: 
![]() * 
![]() * @author zz
![]() * @version 1.00.000
![]() * 
![]() */
![]()public class TestClient {
![]()
![]()    /**
![]()     * 测试1
![]()     */
![]()    @SuppressWarnings("unchecked")
![]()    public void testSend1() {
![]()        try {
![]()            JaxWsProxyFactoryBean factory = new JaxWsProxyFactoryBean();
![]()
![]()            ArrayList<Interceptor> list = new ArrayList<Interceptor>();
![]()            // 添加soap header 
![]()            list.add(new AddSoapHeader());
![]()            // 添加soap消息日志打印
![]()            list.add(new org.apache.cxf.interceptor.LoggingOutInterceptor());
![]()            factory.setOutInterceptors(list);
![]()            factory.setServiceClass(WebServiceSample.class);
![]()            factory.setAddress("http://127.0.0.1:8080/cxfTest/ws/HelloWorld");
![]()
![]()            Object obj = factory.create();
![]()            System.out.println(obj == null ? "NULL" : obj.getClass().getName());
![]()            if (obj != null) {
![]()                WebServiceSample ws = (WebServiceSample) obj;
![]()                String str = ws.say("test");
![]()                System.out.println(str);
![]()
![]()                str = ws.say("1111");
![]()                System.out.println(str);
![]()
![]()                User u = new User();
![]()                JAXBElement<String> je = new JAXBElement<String>(new QName(
![]()                        "http://bean.cxf.hs", "name"), String.class, "张三");
![]()                u.setName(je);
![]()                str = ws.sayUserName(u);
![]()                System.out.println(str);
![]()
![]()                // 通过对象来交互
![]()                ReqBean req = new ReqBean();
![]()                req.setExp(new JAXBElement<String>(new QName(
![]()                        "http://bean.cxf.hs", "exp"), String.class,
![]()                        "<exp>111<exp>"));
![]()                req.setSeqId(new JAXBElement<String>(new QName(
![]()                        "http://bean.cxf.hs", "seqId"), String.class,
![]()                        "12345678"));
![]()                RespBean resp = ws.action(req);
![]()                System.out.println("resp_id:" + resp.getRespId().getValue());
![]()                System.out.println("resp_exp:" + resp.getExp().getValue());
![]()            }
![]()        } catch (Exception ex) {
![]()            ex.printStackTrace();
![]()        }
![]()    }
![]()
![]()    /**
![]()     * 测试2
![]()     */
![]()    @SuppressWarnings("unchecked")
![]()    public void testSend2() {
![]()        String webServiceUrl = "http://127.0.0.1:8080/cxfTest/ws/HelloWorld";
![]()        String webServiceConTimeout = "60000";
![]()        String webServiceRevTimeout = "60000";
![]()        JaxWsProxyFactoryBean factory = new JaxWsProxyFactoryBean();
![]()
![]()        ArrayList<Interceptor> list = new ArrayList<Interceptor>();
![]()        // 添加soap header 信息
![]()        list.add(new AddSoapHeader());
![]()        // 添加soap消息日志打印
![]()        list.add(new org.apache.cxf.interceptor.LoggingOutInterceptor());
![]()        factory.setOutInterceptors(list);
![]()        factory.setServiceClass(WebServiceSample.class);
![]()        factory.setAddress(webServiceUrl);
![]()        WebServiceSample service = (WebServiceSample) factory.create();
![]()
![]()        //超时时间设置
![]()        Client clientP = ClientProxy.getClient(service);
![]()        HTTPConduit http = (HTTPConduit) clientP.getConduit();
![]()        HTTPClientPolicy httpClientPolicy = new HTTPClientPolicy();
![]()        httpClientPolicy.setConnectionTimeout(Integer
![]()                .valueOf(webServiceConTimeout));
![]()        httpClientPolicy.setReceiveTimeout(Integer
![]()                .valueOf(webServiceRevTimeout));
![]()        httpClientPolicy.setAllowChunking(false);
![]()        http.setClient(httpClientPolicy);
![]()        
![]()    
![]()        // 通过对象来交互
![]()        ReqBean req = new ReqBean();
![]()        req.setExp(new JAXBElement<String>(new QName(
![]()                "http://bean.cxf.hs", "exp"), String.class,
![]()                "<exp>111<exp>"));
![]()        req.setSeqId(new JAXBElement<String>(new QName(
![]()                "http://bean.cxf.hs", "seqId"), String.class,
![]()                "12345678"));
![]()        System.out.println(">>>>>>发送消息<<<<<<<<<");
![]()        RespBean resp = service.action(req);
![]()        System.out.println("resp_id:" + resp.getRespId().getValue());
![]()        System.out.println("resp_exp:" + resp.getExp().getValue());
![]()
![]()    }
![]()
![]()    /**
![]()     * @param args
![]()     */
![]()    public static void main(String[] args) {
![]()        TestClient tc = new TestClient();
![]()        tc.testSend1();
![]()        System.out.println(">>>>>>>>>>>>2<<<<<<<<<<<<<");
![]()        tc.testSend2();
![]()        System.out.println(">>>>>>>>>>>>END<<<<<<<<<<<<<");
![]()    }
![]()
![]()}
到这里就结束了，可以进行测试了


