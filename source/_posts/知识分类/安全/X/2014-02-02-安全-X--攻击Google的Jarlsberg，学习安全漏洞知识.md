---
layout: post
title: "攻击Google的Jarlsberg，学习安全漏洞知识"
categories: 安全
tags: 
 - 安全
 - X
--- 

# 攻击Google的Jarlsberg，学习安全漏洞知识

[关闭]()

## 现有用户：

Email:

密码：

## 新用户：

# [InfoQ](http://www.infoq.com/cn/news/2010/05/Jarlsberg#)

[直接转至内容](http://www.infoq.com/cn/news/2010/05/Jarlsberg#content)
时刻关注企业软件开发领域的变化与创新

[En](http://www.infoq.com/) | 中文 | [日本語](http://www.infoq.com/jp/) | [Br](http://www.infoq.com/br/)

* [提供新闻](mailto:news-cn@infoq.com?Subject=[NEWS]&Body=Thanks for your interest in submitting news to InfoQ.com. Please provide as much detail about your news suggestion in this email as possible.  Your suggestion will be considered and if selected an InfoQ editor will write an editted version and publish it. An automated version of this form will be coming sometime in the next few weeks. :) Thanks - InfoQ team.)
* [打印]()

## 新闻

# 攻击Google的Jarlsberg，学习安全漏洞知识

作者 **[Abel Avram](http://www.infoq.com/cn/bycategory.action?authorName=Abel-Avram)**译者 **[侯伯薇](http://www.infoq.com/cn/bycategory.action?authorName=%E4%BE%AF%E4%BC%AF%E8%96%87)**发布于 2010年5月8日 下午10时41分
社区[架构](http://www.infoq.com/cn/architecture),[Java](http://www.infoq.com/cn/java),[.NET](http://www.infoq.com/cn/dotnet),[Ruby](http://www.infoq.com/cn/ruby)主题[安全](http://www.infoq.com/cn/Security)标签[Google](http://www.infoq.com/cn/google),[漏洞](http://www.infoq.com/cn/Vulnerabilities)

[分享 ![Share]()](http://www.addthis.com/bookmark.php?v=250&username=infoq)| []()[]()[]()[]()[]()[]()[]()[]()

很多人都想知道黑客是如何攻击并进入到系统的，为了帮助他们，Google创建了一个特殊的名为[Jarlsberg](http://jarlsberg.appspot.com/) 的实验室，其中的应用程序满是安全漏洞，开发者可以利用它以实践的方式学习到潜在的漏洞是什么样的，恶意用户是如何利用这些漏洞的，以及如何做才能免受攻击。

这个实验室是围绕安全漏洞的不同类型来组织的，对于每个漏洞，都有找寻和攻击漏洞的任务可供完成。 该实验室使用了下列三种主要的技术：

* 黑盒：用户看不到应用程序的源代码，因此他们需要猜测服务器工作的方式，从而设法利用安全漏洞。
* 白盒：以开源应用的形式提供了[源代码](http://jarlsberg.appspot.com/code/)（Python）。 用户可以阅读代码，从而找到弱点所在。
* 灰盒：实验室会给出一些应用程序是如何编写的提示，但不会让用户看到完整的源代码。

Jarlsberg特意使用了大量[特性](http://jarlsberg.appspot.com/part1#1__features_and_technologies)，以扩大应用程序的攻击面。
* 代码片段中的HTML： 用户可以在代码片段中包含特定的HTML代码。
* 文件上载： 用户可以向服务器上载文件，例如，在他们的代码片断中包含图片。
* Web形式的管理： 系统管理员可以使用web界面来管理系统。
* 新建账号： 用户可以创建属于自己的账号。
* 模板语言： Jarlsberg模板语言（JTL）是种新语言，它使得开发者可以更容易地编写网页，作为直接与数据库连接的模板。 你可以在

[jarlsberg/jtl.py](http://jarlsberg.appspot.com/code/?jtl.py)
找到JTL的相关文档。
* AJAX: Jarlsberg使用AJAX来实现在主页和代码片断页中的刷新。 如果不需要特别关注AJAX内容，你可以忽略Jarlsberg中的AJAX部分。

在Jarlsberg中，你可以找到、利用并最终修复以下安全漏洞：

* 跨站点脚本攻击（XSS）
* 跨站点伪造请求攻击（XSRF）
* 跨站点包含脚本攻击（CSSI）
* 客户端状态操控攻击
* 路径模式发掘攻击
* 拒绝服务攻击（DoS）
* 执行代码攻击
* 配置漏洞
* AJAX漏洞攻击

你可以在本地运行该实验室，以在整个学习过程中完全掌控该实验室；或者也可以在Google的云中作为沙盒实例运行。 该实验室大部分是基于[Creative Commons Attribution 3.0](http://creativecommons.org/licenses/by/3.0/us)协议发布的，但也有一部分是基于[Creative Commons Attribution-No Derivative Works 3.0](http://creativecommons.org/licenses/by-nd/3.0/us)协议发布的，这使得对于大学培训学生或者公司培训员工，让他们理解并保护他们系统免受安全漏洞影响都是很理想的。

**查看原文：**[Learning About Security Vulnerabilities by Hacking Google’s Jarlsberg](http://www.infoq.com/news/2010/05/Jarlsberg)

 
[分享 ![Share]()](http://www.addthis.com/bookmark.php?v=250&username=valic4)| [](http://www.infoq.com/cn/news/2010/05/ "Send to Facebook")[](http://www.infoq.com/cn/news/2010/05/ "Digg This")[](http://www.infoq.com/cn/news/2010/05/ "Send to Dzone")[](http://www.infoq.com/cn/news/2010/05/ "Send to Slashdot")[](http://www.infoq.com/cn/news/2010/05/ "Tweet This")[](http://www.infoq.com/cn/news/2010/05/ "Send to Reddit")[](http://www.infoq.com/cn/news/2010/05/ "Send to Delicious")[]( "Email")

### 相关厂商内容

[Visual Studio 2010专业版Beta 2(Web安装程序，4.4M)](http://www.infoq.com/infoq/url.action?i=1087&t=f)

[Visual Studio 2010基础版Beta 2(Web安装程序，4.9M)](http://www.infoq.com/infoq/url.action?i=1088&t=f)

[Visual Studio 2010旗舰版Beta 2(Web安装程序，4.6M)](http://www.infoq.com/infoq/url.action?i=1089&t=f)

[QClub：微软核心开发团队深入剖析Agile与TFS(4月13日北京，4月15日上海)](http://www.infoq.com/infoq/url.action?i=1194&t=f)

[创建移动富互联网应用(RIA)的设计技巧](http://www.infoq.com/cn/vendorcontent/show.action?vcr=931)

### 相关赞助商

[![]()](http://www.infoq.com/infoq/url.action?i=1033&t=f)

Visual Studio 2010产品系列提供了工具、技术支持与团队平台组合，激发你的创作灵感，并兼容当前的主流标准。现在就来[免费下载试用](http://clk.atdmt.com/MCH/go/192829996/direct/01/)！
### 2 条回复

[关注此讨论]()[回复]()

[google真是NB]()发表人 Karl MA 发表于 2010年5月9日 下午11时38分
[Re: google真是NB]()发表人 ben jamin_zhou 发表于 2010年5月12日 上午12时48分

[按日期倒序排列]()

1. []()

[返回顶部](http://www.infoq.com/cn/news/2010/05/Jarlsberg#)
### [google真是NB](http://www.infoq.com/cn/news/2010/05/Jarlsberg#view_56300)

2010年5月9日 下午11时38分 发表人 **Karl MA**
rt

[回复]()
1. []()

[返回顶部](http://www.infoq.com/cn/news/2010/05/Jarlsberg#)
### [Re: google真是NB](http://www.infoq.com/cn/news/2010/05/Jarlsberg#view_56374)

2010年5月12日 上午12时48分 发表人 **ben jamin_zhou**
NB ，还怕中国攻击了他的服务器

[回复]()

[![InfoQ]()](http://www.infoq.com/cn/)
4月139,277名独立访问用户

* [注册](http://www.infoq.com/cn/reginit.action)
* [登录]()
* [关于我们](http://www.infoq.com/cn/about)
* [合作伙伴](http://www.infoq.com/cn/partners.jsp)
* [个性化RSS![RSS Feed]()](http://www.infoq.com/cn/rss/rss.action?token=3uwzmdYw7IaERDWVN30webvW6cR8vhUb)
* [QCon](http://www.infoq.com/cn/qcon)
## 您的社区

* [Java](http://www.infoq.com/cn/java/)
* [.NET](http://www.infoq.com/cn/dotnet/)
* [Ruby](http://www.infoq.com/cn/ruby/)
* [SOA](http://www.infoq.com/cn/soa/)
* [敏捷](http://www.infoq.com/cn/agile/)
* [运维](http://www.infoq.com/cn/Operations/)
* [架构](http://www.infoq.com/cn/architecture/)

[![Search]()](http://www.infoq.com/cn/news/2010/05/Jarlsberg#)
## 特别专题

* [富互联网应用-RIA](http://www.infoq.com/cn/zones/flash/)
*
* [Visual Studio 2010](http://www.infoq.com/cn/zones/visualstudio2010/)
*
* [SOA应用集成方案](http://www.infoq.com/cn/zones/primeton/)
*
* [Scrum开发](http://www.infoq.com/cn/scrum/)
*
* [月刊：《架构师》](http://www.infoq.com/cn/architect/)
*
* [线下活动：QClub](http://www.infoq.com/cn/qclub/)

### 赞助商链接

[IBM精英讲师团招募](http://www.infoq.com/ads/textLinkUrlAction.action?i=178&url=http%3A%2F%2Fwww.infoq.com%2Fcn%2Fvendorcontent%2Fshow.action%3Fvcr%3D891)
每个爱技术乐分享
的人都有机会加入
站在巨人的肩上
IBM将助您攀登新
的职业高峰立刻报名

[Visual Studio
2010](http://www.infoq.com/ads/textLinkUrlAction.action?i=176&url=http%3A%2F%2Fclk.atdmt.com%2FMCH%2Fgo%2F229960548%2Fdirect%2F01%2F),支持多显示
器,强大的Web开发
功能,自带大量模板
和Web部件 下载

[QClub报名：大规](http://www.infoq.com/ads/textLinkUrlAction.action?i=175&url=http%3A%2F%2Fwww.infoq.com%2Fcn%2Fvendorcontent%2Fshow.action%3Fvcr%3D960)
模数据处理和互联
网应用服务扩展
5月15日北京 免费

[SOA白皮书](http://www.infoq.com/ads/textLinkUrlAction.action?i=177&url=http%3A%2F%2Fwww.infoq.com%2Fcn%2Fvendorcontent%2Fshow.action%3Fvcr%3D957)
最佳的SOA实施
策略,阐述ONE集
成解决方案的重
点内容 立刻下载
## 深度内容

* [全部]()
* [文章]()
* [技术演讲]()
* [技术访谈]()
* [迷你书]()
# [MonoTouch：用.NET开发iPhone应用]()

[![]()]()

MonoTouch是一个基于Mono的用于开发iPhone应用程序的框架。Bryan Costanich为大家展示了如何使用MonoDevelop IDE来快速地创建基于.NET的iPhone应用程序。

* [.NET](),
* **[Bryan Costanich]()** 2010年5月11日 ,
* [  4]()

# [使用JBoss jBPM实现流程访问和执行的授权]()

[![]()]()

能够对流程定义和实例进行访问控制，保证用户只能使用和监视他们被授权的那部分流程，可以极大地让集中化的BPM部署受益。在这篇文章中，Boris Lublinsky给出了如何扩展JBoss jBPM，使之能够定义并支持流程访问授权的方法。

* [Java](), [SOA](),
* **[Boris Lublinsky]()** 2010年5月10日 ,
* [  1]()
# [WEB数据交互的艺术]()

[![]()]()

Web发展的源动力是用户的需求，用户的需求又都通过数据的交互来实现，即什么样的交互数据决定着什么样的Web。本演讲介绍了交互数据的格式、几种疑难数据交互的实现（跨页交互、跨域交互、即时交互等）等等。

* [Java](),
* **[黄方荣]()** 2010年5月7日 ,
* [  3]()

# [让Scrum之树长青：战胜紧张和恐惧]()

[![]()]()

团队迅速紧握像Scrum这般简单有效的东西时，与其相关的改变可能引发担忧。了解了本文列举的问题，你就可以为之做好准备。如果自己碰到这些问题，感觉不会那么糟——它们本来就很普遍。

* [敏捷](),
* **[Geoff Watts]()** 2010年5月6日
# [HTML5 Web Sockets与代理服务器交互]()

[![]()]()

Peter Lubbers在本文中解释了HTML5 Web Sockets是如何与代理服务器交互的，以及Web Sockets通信需要怎样的代理配置或更新。

* [架构](),
* **[Peter Lubbers]()** 2010年5月5日 ,
* [  1]()

# [内存屏障与JVM并发]()

[![]()]()

本文介绍了内存屏障对多线程程序的影响。我们将研究内存屏障与JVM并发机制的关系，如易变量、同步和原子条件式。

* [Java](),
* **[Dennis Byrne]()** 2010年4月21日 ,
* [  8]()
# [你是个软件架构师吗？]()

[![]()]()

开发和架构的界限到底是不是存在呢，似乎有点难以捉摸？有必要竖立这样一个学术区分么？ 在两者之间有个平衡，但是你怎么样从一个开发者成为架构师呢？

* [架构](),
* **[Simon Brown]()** 2010年4月15日 ,
* [  20]()

# [敏捷宣言的发起者如何完成从技术领导者到文化变革领袖的转变？]()

[![]()]()

虽然敏捷宣言的发起者是由技术主导的，但实际上，软件社区倾心于敏捷的原因却是敏捷宣言注重人性的方面。

* [敏捷](),
* **[Orit Hazzan, Tali Seger, and Gil Luria]()** 2010年4月14日 ,
* [  2]()

* [更早的 >]()

联系我们 [反馈](mailto:feedback@infoq.com)
feedback@infoq.com [Bugs](mailto:bugs@infoq.com)
bugs@infoq.com [广告](mailto:sales@cn.infoq.com)
sales@cn.infoq.com [编辑](mailto:editors@cn.infoq.com)
editors@cn.infoq.com [Twitter](http://twitter.com/infoqchina)
http://twitter.com/infoqchina [InfoQ讨论组](http://groups.google.com/group/infoqchina)
http://groups.google.com/group/infoqchina

InfoQ.com 及其所有内容，版权所有© 2006-2010 C4Media Inc. InfoQ.com 服务器由 [Contegix](http://www.contegix.com/) 提供，我们最信赖的 ISP 合作伙伴。 [隐私政策]()
