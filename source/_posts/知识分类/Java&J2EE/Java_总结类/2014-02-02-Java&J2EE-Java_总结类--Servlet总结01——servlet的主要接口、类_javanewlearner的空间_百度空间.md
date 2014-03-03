---
layout: post
title: "Servlet总结01——servlet的主要接口、类_javanewlearner的空间_百度空间"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# Servlet总结01——servlet的主要接口、类_javanewlearner的空间_百度空间

[](http://hi.baidu.com/)

* [广场](http://hi.baidu.com/index)
* [jywv](https://passport.baidu.com/center)
*
* [退出](https://passport.baidu.com/?logout&bdstoken=aa9855a9aad4216b27441b12182ad1a7&u=http://hi.baidu.com/yrynxtecldbjmxq/item/4070a6ad7af544d15bf19195)
[关注此空间](http://hi.baidu.com/yrynxtecldbjmxq/item/4070a6ad7af544d15bf19195#)

# [javanewlearner的空间](http://hi.baidu.com/new/yrynxtecldbjmxq)

2012-02-25 15:47

## Servlet总结01——servlet的主要接口、类

**（一）servlet类**

Servlet主要类、接口的结构如下图所示：

![]()

要编写一个Servlet需要实现javax.servlet.Servlet接口，该接口定义了5个方法。如下：

![]()

1.init()，初始化servlet对象，完成一些初始化工作。

它是由servlet容器控制的，该方法只能被调用一次，**初始化过程**如下：

![]()

2.service()，接受客户端请求对象，执行业务操作，利用响应对象响应客户端请求。

3.destroy()，当容器监测到一个servlet从服务中被移除时，容器调用该方法，释放资源。

4.getServletConfig()，ServletConfig是容器向servlet传递参数的载体。

5.getServletInfo()，获取servlet相关信息。

**（二）与servlet相关的接口**

从servlet仅有的5个方法当中，我们知道其涉及3个接口，分别是：

ServletConfig

ServletRequest

ServletResponse

2.1. ServletConfig

主要方法：

![]()

重点关注getServletContext，之前说servletConfig是容器向servlet传递参数的载体，那么它也可以让Servlet获取其在容器中的上下文。

ServletContext是针对一个web应用，jdk中具体描述——

*There is one context per "web application" per Java Virtual Machine. (A "web application" is a collection of servlets and content installed under a specific subset of the server's URL namespace such as /catalog and possibly installed via a .war file.)*

2.2.ServletRequest

获取客户端发来的请求数据。（查看[api](http://download.oracle.com/javaee/5/api/javax/servlet/ServletRequest.html)）

***note：注意getAttribute和getParameter的区别。***

*getAttribute( String name )可以得到由setAttribute()设置的参数值，相当于是使用getAttribute()得到一*

*个自己定义的参数，而不是从客户端得到的参数。*

*getParameter( String name )它用来获取客户端通过get或post方法等传递过来的值，是从客户端传递过来的，*

*一般指的是客户端提交的表单组件的值。*

***note：setCharacterEncoding在什么时候使用才有效？***

*它可以覆盖请求正文中所使用的字符编码，但是它必须在读取parameters之前设置，否则无效。*

2.3.ServletResponse

响应客户端请求。（查看[api](http://download.oracle.com/javaee/5/api/javax/servlet/ServletResponse.html)）

**（三）GenericServlet抽象类**

为了简化serlvet的编写，在javax.servlet包中提供了一个抽象类GenericServlet，它给出了除service()方法以外的简单实现。

GenericServlet定义了一个通用的，不依赖具体协议的Servlet，它实现了Servlet接口和ServletConfig接口。

***public abstract class GenericServlet implements Servlet, ServletConfig, java.io.Serializable***

**（四）HttpServlet抽象类**

HttpServlet主要是应用于HTTP协议的请求和响应，为了快速开发HTTP协议的serlvet，sun提供了一个继承自GenericServlet的抽象类HttpServlet，

用于创建适合Web站点的HTTP Servlet。

**public abstract class HttpServlet extends GenericServlet implements java.io.Serializable**

重点关注HttpServlet中的一个私有方法service。
protectedvoidservice(HttpServletRequest req, HttpServletResponse resp)         throwsServletException, IOException {           String method = req.getMethod();           if(method.equals(METHOD_GET)) {             longlastModified = getLastModified(req);             if(lastModified == -1) {                 // servlet doesn't support if-modified-since, no reason                 // to go through further expensive logic                 doGet(req, resp);             } else{                 longifModifiedSince = req.getDateHeader(HEADER_IFMODSINCE);                 if(ifModifiedSince < (lastModified / 1000* 1000)) {                     // If the servlet mod time is later, call doGet()                     // Round down to the nearest second for a proper compare                     // A ifModifiedSince of -1 will always be less                     maybeSetLastModified(resp, lastModified);                     doGet(req, resp);                 } else{                     resp.setStatus(HttpServletResponse.SC_NOT_MODIFIED);                 }             }           } elseif(method.equals(METHOD_HEAD)) {             longlastModified = getLastModified(req);             maybeSetLastModified(resp, lastModified);             doHead(req, resp);           } elseif(method.equals(METHOD_POST)) {             doPost(req, resp);                       } elseif(method.equals(METHOD_PUT)) {             doPut(req, resp);                               } elseif(method.equals(METHOD_DELETE)) {             doDelete(req, resp);                       } elseif(method.equals(METHOD_OPTIONS)) {             doOptions(req,resp);                       } elseif(method.equals(METHOD_TRACE)) {             doTrace(req,resp);                       } else{             //             // Note that this means NO servlet supports whatever             // method was requested, anywhere on this server.             //               String errMsg = lStrings.getString("http.method_not_implemented");             Object[] errArgs = newObject[1];             errArgs[0] = method;             errMsg = MessageFormat.format(errMsg, errArgs);                           resp.sendError(HttpServletResponse.SC_NOT_IMPLEMENTED, errMsg);         }     }

而它实现Servlet接口的service方法，是将ServletResponse对象和ServletRequest对象转化成httpServletResponse对象和HttpservletRequest对象，

然后再调用私有方法service。它根据request获取Http method(get、post等)的名称，根据http method调用不同的方法执行操作。

**（五）HttpServletRequest、HttpServletResponse**

由于HttpServletRequest和HttpServletResponse都是基于Http协议“升级改造”的，所以这两个对象较之ServletRequest和ServletResponse多出的方法主要和

Http协议相关。

例如：Cookie、request（response）headers、Session、URL等。

具体请查看api。

**（六）重点**

6.1.servlet的生命周期。

6.2.servlet的上下文。
[#J2ee](http://hi.baidu.com/tag/J2ee/feeds)

[举报](http://hi.baidu.com/yrynxtecldbjmxq/item/4070a6ad7af544d15bf19195#)浏览(5) [评论](http://hi.baidu.com/yrynxtecldbjmxq/item/4070a6ad7af544d15bf19195#)[转载](http://hi.baidu.com/yrynxtecldbjmxq/item/4070a6ad7af544d15bf19195#)

[](http://hi.baidu.com/yrynxtecldbjmxq/item/da67bbe6ad8d7adbea34c991 "上一篇")

[](http://hi.baidu.com/yrynxtecldbjmxq/item/3aaa4928228f59846e2cc391 "下一篇")
评论
[帮助中心](http://hi.baidu.com/go/show/introduce) | [空间客服](http://tieba.baidu.com/p/1781683739) | [投诉中心](http://tousu.baidu.com/hi/add) | [空间协议](http://www.baidu.com/search/hi_contract.html)

©2012 Baidu
