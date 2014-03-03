---
layout: post
title: "Jsp知识总结"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# Jsp知识总结

Jsp知识总结

Servlet 容器模型
? 三个基本层次：
1、 上下文（应用程序中的所有全局资源）
2、 会话
3、 应用程序在分布式环境中不同区域
? ServletContext：
其包含在ServletConfig 对象中。所有的Servlet 都有 ServletContext getServletContext( ) 该方法最初在ServletConfig 中定义，后来由GenericServlet 实现。
在DTD中使用context-param标志指定名称/值对，它必须放在web-app标志的开始位置。
它的方法：
String getInitParameter(String name)
Enumeration getInitParameterName( )
URLgetResource(Stringpath)throws java.net.MalformedURLException
InputStream getResourseAsStream(String path)
String getMimeType(String file)
RequestDispatcher getRequestDispatcher(String path)
Void setAttribute(String name,Object value)
Object getAttribute(String name)
? 上下文参数和属性的区别：
1、 参数只能在容器或者是web.xml 中设置而属性通过servlet或容器设置
2、 参数只返回String 而属性返回Object
3、 属性名名称用包结构
? ServleContextListener：
Void contextInitialized(ServletContextEvent e)
Void contextDestroyed(ServletContextEvent e)
其作用为在调用servlet请求之前初始化所有数据。
<listener>
<listener-class>
的标志出现在servlet 声明之前。
? ServletContextEvent:
ServletContext getServletContext()
? ServletContextAttributeListenr:
Void attributeAdded(ServletContextAttributeEvent e)
Void attributeRemoved(ServletContextAttributeEvent e)
Void attributeReplaced(ServletContextAttributeEvent e)
? ServletContextAttributeEvent:
String getName( )
Object getValue( )
? HttpSession:
会话在客户发送第一个请求是建立，它不会自动跟踪用户操作，用HttpServletRequest 类的方法进行访问：
HttpSession getSession (boolean )
然后用HttpSession类的方法使用会话数据：
Object getAttribute(String name)
Void setAttribute(String name,Object value)
Void removeAttribute(String name)
? HttpSessionListener：
必须实现以下的方法:
void sessionCreate ( HttpSessionEvent e)
void sessionDestryed(HttpSessionEvent e)
? HttpSessionEvent:
HttpSession getSession()
? HttpSessionAttributeListener:
Void attributeAdded(HttpSessionBindingEvent e)
Void attributeRemoved(HttpSessionBindingEvent e)
Void attributeReplaced(HttpSessionBindingEvent e)
? HttpSessionBindingEvent:
HttpSession getSession( )
String getName( )
Object getValue( )
? HttpSessionActivationListener:
Void sessionDidActive(HttpSessionEvent se)
上面的方法负责在转移会话时，读取会话中的文件内容，并将其写到新的服务器的文件中。
Void sessionWillPassivate(HttpSessionEvent se)
上面的方法负责转移对话前将文件中的内容写到会话中。
? HttpSessionBindingListener:
它和HttpSessionAttributeListener 的区别是它是从对象绑定的角度去观察，而HttpSessionAttributeListener是从会话的角度来观察。
当会话超时或无效时，对象会从会话解除绑定。此时HttpSessionBindingListener会得到通知，而HttpSessionAttributeListener不会。
Void valueBound(HttpSessionBindingEvent event)
Void valueUnbound(HttpSessionBindingEvent event)
? 可分布环境
1. 变量应区别对待：实例变量或静态变量应避免。
2. ServletContext 应避免存储状态。
3. HttpSession对象也必须是可转移的。
4. 文件也必须分别处理应使用相对于上下文的路径。
在应用程序的描述和上下文参数之间放置一个空的标志<distributable/>
? 过滤器
1. 生命周期
在过滤器截取响应对象的时候，如果输出流被servlet 关闭了，那么过滤器将不能改变输出流的信息，因此，鼓励用servlet的flush( )刷新流，而不是用close( )关闭流。
2. 过滤器
过滤器类必须：
1. 实现适当接口
2. 定义方法
3. 在部署描述符中被声明
3. 创建过滤器
实现javax.servlet.Filter接口
实现方法：
public void init(FilterConfig config)
public void doFilter(
ServletRequest req,ServletResponse resp,FilterChain chain)
Public void destroy( )
当前的过滤器调用chain.do Filter(req,resp) 可以转移到下一个过滤器。
Destroy( ) 方法是由Web容器调用的。
4. 定义部署描述符(必须出现在servlet标志之前)
<filter>
<filter-name></filter-name>
<filter-class></filter-class>
<init-param>
<param-name></>
<param-value></>
映射到servlet或jsp的方法：
1. <filter-mapping>
<filter-name></>
<servlet-name></>
</>
2. <filter-mapping>
<filter-name></>
<url-pattern>/*.jsp</>
</>
术语：
clustering (聚类)、listener(接收器)、context object(上下文对象)
scope(作用域)、distributable(可分布的)、session(会话)
event(事件)、synchronization(同步)、filter(过滤器)
处理异常
? 问题通知
sendError:
HttpServletResponse 类提供的sendError( ) 方法
Public void sendError(int sc)
Public void sendError(int sc,String msg)
使用sendError() 方法将发生三件事情:
1. 使用指定的状态代码给客户端发送一个出错响应
2. 用包含具体消息的html 格式的服务器错误页面替换servlet 的响应正文.
3. 将内容类型设成text/html, 并不修改cookie 和其他标题.
setStatus
public void setStatus(int statusCode)
setStatus(int statusCode)不能产生自动的出错响应页面,而sendError(…) 能够.
setStatus(…)不会提交响应,他会导致容器清除响应缓冲区.容器设置Location标题,保持cookie 和其他标题. 如果已提交响应,则忽略setStatus(…)的调用.
一般用于非错误情形(SC_OK\SC_MOVED_TEMPORARILY)
sendError(…)一般用于错误情形.
? 错误页面
setStatus(…) 和 sendError(…) 默认是产生一个由服务器格式化的错误页面.
a) 静态错误页面
<web-app>
<error-page>
<error-code>
404
</error-code>
<location>
/error/404.html(必须以斜线开始)
</location>
</>
</>
b) 动态错误页面
javax.servlet.error.status_code 返回一个指定错误状态代码的Integer对象.
javax.servlet.error.message 返回一个String消息,通常是通过传递给endError(…)的第二个参数指定的.
一个动态页面或servlet可用于多个错误代码.每个都要在dtd 中声明.每个错误代码都必须有自己的<error-page></>
? 传递错误
必须在转发请求之前设置错误属性.
? 记录消息
GenericServlet类提供了两个方法,使servlet能将错误写到日志文件:
public void log(String msg) 将一条错误消息写入servlet 日志文件.
public void log(String msg,Throwable t) 将一条错误消息和Throwable对象写入servlet 日志文件.
? 报告消息
printStackTrace( ) 方法返回void. 为了将错误消息传递给客户端,
必须调用sendError(…) , 但是在消息可被传递之前,必须捕获记录并将其写入PrintStream 或 PrintWriter. Throwable 类提供了:
void printStackTrace()
void printStackTrace(PrintStream p)
void printStackTrace(PrintWriter p)
? servlet异常
方法抛出异常:
1. 让servlet 捕捉
2. 抛给服务器处理
规范明确指出服务器处理的异常必须是IOException ServletException RuntimeException 的子类.
ServletException: (Exception的子类)
ServletException( )
ServletException(String message)
ServletException(Throwable rootCause)
ServeltException(String message , Throwable rootCause)
UnavailableException: (ServletException的子类)
永久不可用: servlet 以某种方式销毁或没有适当配置.
暂时不可用: 由于系统范畴的问题.
Java服务器页面
Jsp 模型
Jsp 模型的两个目标：
1． 允许并鼓励Presentation层与java代码的分离
2． 使开发人员能以最大的能力和灵活性编写具有动态内容的web.
标准的servlet 扩展了java.servlet.HttpServlet
从jsp 转换的servlet都必须利用javax.servlet.jsp.JspPage接口
方法:
void jspInit()
void jspDestroy()
javax.servlet.jsp.HttpJspPage接口增加了
void _jspService(HttpServletRequest req,HttpServletResponse res)
过程如下:
1. 翻译页面(.jsp到.java)
2. 编译jsp(.java到.class)
3. 加载类
4. 创建实例
5. 在该页面第一次初始化时调用jspInit().
6. 为每个jsp 请求调用_jspDestroy( )
7. 在服务器销毁该jsp时调用_jspDestroy( ).
JSP元素
? 隐含的注释:
jsp 语法 <%--comment--%>
? 声明: 可以访问的java变量和方法
jsp 语法 <%! Declaration; %>
xml 语法 <jsp:declaration>
declaration;
</jsp:declaration>
书写声明的规则:
1. 可以声明静态或实例变量、新的方法或是内部类。
2. 每个变量声明都必须以分号结束。
3. 可以通过import 语句使用的变量和方法在没有所需的额外声明的情况下也能使用。
4. 声明方法和变量后，以后的java代码都可用。
5. 它通常包含将超出servlet的_jspService(…) 方法之外的代码。
? 表达式
jsp 语法
<%=expression%>
xml 语法
<jsp:expression>
expression;
</jsp:expression>
表达式规则：
1. 通常不以分号结束。
2. 从左到右求值。
3. 可由多个部分或表达式组成
? Scriplet
Jsp 语法
<% code fragment %>
xml 语法
<jsp:scriptlet>
code fragment;
</jsp:scriptlet>
? 语句
处理花括号
? 指令
1. include指令 包含一个静态文件
jsp 语法：
<%@ include file = “relativeURL” %>
xml 语法：
<jsp:directive.include file=”relativeURL”/>
路径以“/”开始是相对于上下文。
路径起始于一个目录或文件名，那么该路径是相对于jsp页面的。
2. page 指令定义将应用于整个jsp页面的属性。
Jsp 语法 〈%@ page Attributes %〉
language\extends\import\session\buffer\autoFlush
\isThreadSafe\info\errorPage\isErrorPage\contentType
\pageEncoding
xml 语法
〈jsp:directive.page pageDirectiveAttribute〉
规则：
1） 在一个翻译单元可多次使用page
2） 除了import 外，一个翻译单元只能用一个page属性
3） page指令可放在jsp页面的任何地方。
3. taglib指令指定在jsp页面内使用的标志库和定制标志的前缀。
Jsp 语法：
<%@ taglib uri=”URIForLiabrary” prefix=”tagPrefix” %>
前缀是唯一的，且不能使用：jsp jspx java javax servlet sun sunw.
? 隐含对象
plication对象:
web.xml:
<context-param>
<param-name></>
<param-value></>
</>
可如下获取：
application.getInitParametr(“paraname”);
pageContext对象：
方法：
getOut( )
getException( )
getPage( )
getRequest( )
getResponse( )
getSession( )
getServletConfig( )
getServletContext( )
forward( )
include( )
handlePageException( )
它拥有页面作用域，这意味着只能通过_jspService( )方法访问该对象。
config 对象：
通过pageContext 的getAttribute(String name)访问config属性。
request对象：
即使使用了forward( ) include( ) 等，该请求也在作用域内。
response 对象：
被绑定到PageContext ,能够在_jspService(…)中访问。
如page指令的缓冲区没有设成none，http状态代码和标题能够在发送到客户端后在被更改。
session 对象：
被绑定到PageContext，getAttribute(String name)可访问数据。
out 对象：
属于javax.servlet.jsp.JspWriter 类，是java.io.PrintWriter的缓冲版本。
缓冲大小可设置〈%@ page buffer=”16kb” %〉
page对象：
相当于java中的this.
Exception对象：
是java.lang.Throwable的实例，如果使用page指令的errorPage属性，那么exception对象对该页面就不可用。例如：
〈%@page isErrorPage=”true” %〉
<pre>
<% exception.printStackTrace(new PrintWriter(out)); %>
表达式〈%= … %〉是在处理HTTP或HTTP服务器处理请求的时候求值的。
Scriptlet 〈% …%〉
处理请求时执行，被嵌入到_jspService(…)方法中
声明 〈%!…%〉
调用jsp的jspInit( )方法时初始化
指令<%@ …%>
是在翻译时调用的。
? 操作
jsp:include
<jsp:include page=”relativeURL”>
or
<jsp:include page=”relativeURL” flush=”true|false”>
<jsp:param name=”parameterName” value=”parameterValue”>
</jsp:inlcude>
page 指定目标资源的相对URL
param标志确定需要传递的参数值
flush标志等于true或false 默认为false 要求设为true
include 有两种使用方法：
1． 具有file属性的指令，包含一个静态文件
2． 使用操作将令一个文件的静态或动态的输出结合到响应中
jsp:forward:
不会处理jsp:forward模块后的任何代码。
jsp:plugin:
<jsp:plugin
type=”bean|applet”
code=”classFileName.class”
>
<jsp:params>
<jsp:param name=”pramName” value=”paramvalue”/>
</jsp:params>
<jsp:fallback> text message for user</jsp:fallback>
</jsp:plugin>
jsp:useBean
<jsp:useBean id=”beanInstanceName”
scope=”page|request|session|application”
{
class=”package.class”|
type=”package.class”|
class=”package.class” type=”package.class”|
beanName=”{package.class|<%=expression %>}”
type=”package.class”
}
/>|</jsp:useBean>
同时使用class和beanName是无效的
jsp:setProperty
jsp:getProperty
