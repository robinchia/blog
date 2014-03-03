---
layout: post
title: "J2EE总结--Servlet技术"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# J2EE总结--Servlet技术

[J2EE总结--Servlet技术 我的所有随笔属于个人总结，有不足之处请回复指出](http://www.blogjava.net/todd841026/archive/2007/04/08/Servlet.html)

Servlet技术：

1.       什么是servlet？

Servlet是一个java类，是一个提供基于协议请求和响应的java类；

2.       它的生命周期

      1.       启动服务器时就会实例化并加载servlet实例；

      2.       进行初始化：自动调用init（ServletConfig servletConfig）方法；

      3.       Servlet就绪：调用service（HttpServletRequest request，HttpServletResponse response）方法（其   中     service   就是dopost（）或doget（）方法），这是客户提交时，自动调用的；

      4.       Servlet销毁：自动调用调用distory（） ；

   注意：在实例化并加载servlet后，步骤二和四只调用一次，而步骤三，是在每次客户端发出请求时都调用；

3.       怎样部署一个servlet？

Servlet类是必须在web.xml中注册才能使用的，例如，我有一个MyServlet类：

必须在web.xml中注册：

<web-app>

//-----------------------Servlet声明----------------------

           <servlet>

                 <servlet-name>myServlet</servlet-name>

                 <servlet-class>servletPakage.MyServlet</servlet-class>

      </servlet>

//------------------------Servlet注册(镜像)---------------

           <servlet-mapping>

                 <servlet-name>myServlet</servlet-name>

                 <url-pattern>/myServletURL</ url-pattern >

      </servlet-mapping>

</web-app>

这样你在提交时的Url地址就是/myServletURL了；

4.       什么是service（HttpServletRequest request，HttpServletResponse response）方法？

其中service（HttpServletRequest request，HttpServletResponse response）方法包括两种：

      1.       doget（HttpServletRequest request，HttpServletResponse response）方法：

      这种方法被称为显式提交方法，主要原因是它的得到的参数放在url中，可以被看到，所以称为显示提      交；

      例如：有个表单：

      <form action[=”/myServletURL?name=todd](http://www.blogjava.net/myServletURL?name=todd)” method=”get”>

      </form>

      这种方法其request获得的参数就是你看到的name=todd；

      例如：String s=request.getParameter(“name”);

            其结果s=”todd”;

      2.       dopost（HttpServletRequest request，HttpServletResponse response）方法：

         这种方法被称为隐式提交方法，它的参数不会在url里得到，而是在请求数据体得到参数；

         例如：有个表单：

            <form action=”/myServletURL” method=”post”>

                     <input type=”text” name=”name” value=”todd”>

         </form>

      这种方法其request获得的参数就是表单体的name=todd；

            例如：String s=request.getParameter(“name”);

            其结果s=”todd”;

5.       什么是ServletContext?

         ServletContext是一个接口，是WebApplication的视图，它的作用域时Application，它能访问Application中的初始化参数和属性，它不局限域一个Servlet，它属于整个Application；

ServletContext的初始化参数：

在web.xml中：

<web-app>

    <context-param>

<param-name>myBlog</param-name>

<param-value>www.blogjava.net/todd841026</param-value>

</context-param>

</web-app>

 这样在application中任意一个Servlet中可以得到这个参数，

例如：ServletContext sc = getServletContext ();

          String s = sc.getInitParameter(“myBlog”);

那么结果s就是”www.blogjava.net/todd841026”

6.       什么是ServletConfig？

是单独的Servlet初始化配置；

例如：在web.xml中

<web-app>

            <servlet>

                 <servlet-name>myServlet</servlet-name>

                 <servlet-class>servletPakage.MyServlet</servlet-class>

      </servlet>

      <init-param>

            <param-name>cache</param-name>

            <param-value>off</param-value>

</init-param>

</web-app>

在这个Servlet中：ServletConfig sc = getServletConfig();

                            String s = sc.getInitParameter(“cache”);

那么结果s就是”off”;

7.       Servlet怎样处理多线程

在默认的情况下，单个Servlet实例是可以处理多个并发请求的，所以要考虑到多线程的共享同一对象的问题，例如：

//做个Servlet中产生了多少个object对象一个变量的例子

Private int count = 0 ;

Public void dopost(HttpServletRequest request,HttpServletResponse response){

           Object object = new Object() ;

           count++ ;

           System.out.println(“count = ” + count) ;

}

当有5个用户提交数据时，因为Servlet是处理多线程的，所以可能出现，第四个用户的程序已经执行了count++，而第五个用户刚执行完Object object = new Object() ，就会出现数据不一致性，因为当前有5个object对象，但是count却是4；

解决方案一：

Private boolean flag = false ；

Private int count = 0 ;

Public void dopost(HttpServletRequest request,HttpServletResponse response){

           synchronized(flag){

                    Object object = new Object() ;

                    count++ ;

}                

           System.out.println(“count = ” + count) ;

}

用同步程序块解决多线程的问题，这样在同一时刻就只能有一个访问该程序块了；

解决方案二：

Private int count = 0 ;

Public void dopost(HttpServletRequest request,HttpServletResponse response)

Implements SingleThreadModel{

           Object object = new Object() ;

           count++ ;

           System.out.println(“count = ” + count) ;

}

实现SingleThreadModel接口，可以解决多线程问题；

8.       什么是servlet过滤器？

也是一个java类，只是它实现了Filter这个接口；

9.       servlet过滤器的生命周期；

初始化：自动调用init(FilterConfig config)方法

执行：自动调用doFilter()方法；

销毁：自动调用destory()方法；

10.   servlet过滤器有什么用途？

      个人认为目前自己用到的Servlet过滤器的主要用途：是安全性检查

         当然过滤器在Servlet之前也可以修改请求，要是在Servlet之后，也可以修改响应； 

11.   servlet过滤器怎样部署？

在web.xml中：

<web-app>

    <filter>

      <filter-name>myFilter</filter-name>

      <filter-class>filterPage.MyFilter</filter-class>

</filter>

<filter-mapping>

      <filter-name>myFilter</filter-name>

      <url-pattern>/Todd/*</url-pattern>

</filter-mapping>

</web-app>

这样就是说要访问WEB-INF下的Todd包下的jsp或Servlet的话，就必须要先通过myFilter这个类；

 
