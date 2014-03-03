---
layout: post
title: "DWR Reverse Ajax功能实践的要点"
categories: dwr
tags: 
 - dwr
--- 

# DWR Reverse Ajax功能实践的要点

* [网摘首页](http://wz.csdn.net/)
* [管理网摘](http://wz.csdn.net/my)
* [实时网摘](http://wz.csdn.net/spy)
* [工具和帮助](http://wz.csdn.net/tool)

# DWR Reverse Ajax功能实践的要点

Reverse Ajax主要是在BS架构中，从服务器端向多个浏览器主动推数据的一种技术。它的一种实现就是客户端向服务器请求后，服务器不立即回应，从而导致一个http长连接，等到有更新数据的时候，再利用这个连接“主动”向客户端回送。
如果是初次接触，那一定要看下这篇文章
其中，详述了这种技术和JETTY服务器Continuations功能结合时的强大性能：运行在非阻塞方式下，当多个客户端请求时不会占用过多线程。
最后，此文申明DWR的实现已经天然使用了JETTY这一功能。所以使用DWR还是非常有好处的。如何使用及示例上面这篇文章已经有说明，下面就以我实际使用中碰到的问题和要点做个说明。
首先，说一下代码的组织和声明。
将使用到Reverse Ajax的代码归入一个类，比如是NotifyClient，并用spring的bean来声明。在将要用到这个类的地方(NodeSvcImpl类)，也通过成员变量引入:
然后在dwr.xml里这样声明：
其次一个要点是，如果你不是在DWR所开的线程中使用Reverse Ajax，那WebContextFactory.get()会返回空，这是因为只有DWR自己的线程才会初始化它。那如果你是在DWR之外使用，比如说收到JMS消息，或者UDP消息后，想通知所有客户端，你就要用到ServerContext。
要得到ServerContext，就需要用到spring的ServletContextAware接口，下面是完整代码：
package com.hhh.nms.remote;
import org.apache.log4j.Logger;
import javax.servlet.ServletContext;
import org.springframework.web.context.ServletContextAware;
import java.util.Collection;
import org.directwebremoting.ScriptBuffer;
import org.directwebremoting.ScriptSession;
import org.directwebremoting.WebContext;
import org.directwebremoting.WebContextFactory;
import org.directwebremoting.ServerContext;
import org.directwebremoting.ServerContextFactory;
import org.directwebremoting.proxy.dwr.Util;
public class NotifyClient implements ServletContextAware {
static Logger logger = Logger.getLogger (NotifyClient.class.getName());
private ServletContext servletContext = null;
public void setServletContext( ServletContext servletContext )
{
this.servletContext = servletContext;
}
public int serviceUpdate (String str1, String str2) {
logger.info ("entered");
logger.info ("WebContext1"+servletContext);
ServerContext ctx = ServerContextFactory.get(servletContext );
// Generate JavaScript code to call client-side
// WebContext ctx = WebContextFactory.get();
logger.info ("WebContext"+ctx);
if (ctx != null) {
//String currentPage = ctx.getCurrentPage();
//logger.info ("current page:" + currentPage);
ScriptBuffer script = new ScriptBuffer();
script.appendScript("updatePoint(")
.appendData(str1)
.appendScript(",")
.appendData (str2)
.appendScript(");");
// Push script out to clients viewing the page
Collection sessions =
ctx.getScriptSessionsByPage("/ebnms/index.eb?do=dwrtest");
logger.info ("jsp session size:" + sessions.size ());
// or
Collection sessions2 =
ctx.getAllScriptSessions ();
logger.info ("all session size:" + sessions2.size ());
for (ScriptSession session : sessions2) {
session.addScript(script);
}
}
return 0;
}
}
另外，ScriptBuffer的appendScript方法是插入原始字串，appendData会根据参数类型做相应转换。

[http://www.blogjava.net/alwayscy/archive/2007/11/01/157552.html](http://www.blogjava.net/alwayscy/archive/2007/11/01/157552.html)

# 他们设置了哪些标签：

[WEB开发](http://wz.csdn.net/tag/WEBå¼å/ "1个网摘")

# 谁收藏了这个网址：

## [conanpaul](http://wz.csdn.net/conanpaul/)收录

使用标签：[Web开发](http://wz.csdn.net/conanpaul/Web开发/)，时间：2007-11-2 9:34:36 | [相关网摘](http://wz.csdn.net/item/1204886/)
Reverse Ajax主要是在BS架构中，从服务器端向多个浏览器主动推数据的一种技术。它的一种实现就是客户端向服务器请求后，服务器不立即回应，从而导致一个http长连接，等到有更新数据的时候，再利用这个连接“主动”向客户端回送。
如果是初次接触，那一定要看下这篇文章
其中，详述了这种技术和JETTY服务器Continuations功能结合时的强大性能：运行在非阻塞方式下，当多个客户端请求时不会占用过多线程。
最后，此文申明DWR的实现已经天然使用了JETTY这一功能。所以使用DWR还是非常有好处的。如何使用及示例上面这篇文章已经有说明，下面就以我实际使用中碰到的问题和要点做个说明。
首先，说一下代码的组织和声明。
将使用到Reverse Ajax的代码归入一个类，比如是NotifyClient，并用spring的bean来声明。在将要用到这个类的地方(NodeSvcImpl类)，也通过成员变量引入:
然后在dwr.xml里这样声明：
其次一个要点是，如果你不是在DWR所开的线程中使用Reverse Ajax，那WebContextFactory.get()会返回空，这是因为只有DWR自己的线程才会初始化它。那如果你是在DWR之外使用，比如说收到JMS消息，或者UDP消息后，想通知所有客户端，你就要用到ServerContext。
要得到ServerContext，就需要用到spring的ServletContextAware接口，下面是完整代码：
package com.hhh.nms.remote;
import org.apache.log4j.Logger;
import javax.servlet.ServletContext;
import org.springframework.web.context.ServletContextAware;
import java.util.Collection;
import org.directwebremoting.ScriptBuffer;
import org.directwebremoting.ScriptSession;
import org.directwebremoting.WebContext;
import org.directwebremoting.WebContextFactory;
import org.directwebremoting.ServerContext;
import org.directwebremoting.ServerContextFactory;
import org.directwebremoting.proxy.dwr.Util;
public class NotifyClient implements ServletContextAware {
static Logger logger = Logger.getLogger (NotifyClient.class.getName());
private ServletContext servletContext = null;
public void setServletContext( ServletContext servletContext )
{
this.servletContext = servletContext;
}
public int serviceUpdate (String str1, String str2) {
logger.info ("entered");
logger.info ("WebContext1"+servletContext);
ServerContext ctx = ServerContextFactory.get(servletContext );
// Generate JavaScript code to call client-side
// WebContext ctx = WebContextFactory.get();
logger.info ("WebContext"+ctx);
if (ctx != null) {
//String currentPage = ctx.getCurrentPage();
//logger.info ("current page:" + currentPage);
ScriptBuffer script = new ScriptBuffer();
script.appendScript("updatePoint(")
.appendData(str1)
.appendScript(",")
.appendData (str2)
.appendScript(");");
// Push script out to clients viewing the page
Collection sessions =
ctx.getScriptSessionsByPage("/ebnms/index.eb?do=dwrtest");
logger.info ("jsp session size:" + sessions.size ());
// or
Collection sessions2 =
ctx.getAllScriptSessions ();
logger.info ("all session size:" + sessions2.size ());
for (ScriptSession session : sessions2) {
session.addScript(script);
}
}
return 0;
}
}
另外，ScriptBuffer的appendScript方法是插入原始字串，appendData会根据参数类型做相应转换。
[](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)

[网站简介](http://www.csdn.net/help/intro.htm)－[广告服务](http://www.csdn.net/help/ads.htm)－[网站地图](http://www.csdn.net/help/sitemap.htm)－[帮助](http://www.csdn.net/help/help.htm)－[联系方式](http://www.csdn.net/help/contact.htm)－[诚聘英才](http://job.csdn.net/Jobs/f9c75c9f2ad14404a604669b757b9ed0/viewcompany.aspx)－[English](http://www.csdn.net/english/)－[问题报告](http://wz.csdn.net/url/708367/#)
北京百联美达美数码科技有限公司 版权所有 京 ICP 证 020026 号
Copyright © 2000-2009, CSDN.NET, All Rights Reserved
[![]()](http://www.vdoing.com/ "Vdoing StatsX No.56805")
