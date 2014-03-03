---
layout: post
title: "认识Squid_inJava_百度空间"
categories: 服务器
tags: 
 - 服务器
 - Squid
--- 

# 认识Squid_inJava_百度空间

[](http://hi.baidu.com/)

* [相册](http://xiangce.baidu.com/)
* [广场](http://hi.baidu.com/index/)
* [登录](http://hi.baidu.com/go/login)
*
* [注册](http://hi.baidu.com/go/reg?pmfrom=qingtop)
[关注此空间](http://hi.baidu.com/injava/item/b6886fc1fc72af52ac00ef7d#)

# [inJava](http://hi.baidu.com/injava)

J2EE、架构、搜索引擎、数据挖掘、互联网、创业、媒体、资料库

2008-03-22 13:48

## 认识Squid

Squid是什么?
Squid能做什么?
Squid如何工作的?
[Squid是什么]
   Squid是一个高性能的代理缓存服务器，Squid支持FTP、gopher和HTTP协议。和一般的代理缓存软件不同，Squid用一个单独的、非模块化的、I/O驱动的进程来处理所有的客户端请求。
   Squid将数据元缓存在内存中，同时也缓存DNS查询的结果，除此之外，它还支持非模块化的DNS查询，对失败的请求进行消极缓存。Squid支持SSL，支持访问控制。由于使用了ICP（轻量Internet缓存协议），Squid能够实现层叠的代理阵列，从而最大限度地节约带宽。
   Squid由一个主要的服务程序squid,一个DNS查询程序dnsserver，几个重写请求和执行认证的程序，以及几个管理工具组成。当Squid启动以后，它可以派生出预先指定数目的dnsserver进程，而每一个dnsserver进程都可以执行单独的DNS查询，这样一来就大大减少了服务器等待DNS查询的时间。
Squid是一个缓存internet数据的一个软件，它接收用户的下载申请，并自动处理所下载的数据。也就是说，当一个用户象要下载一个主页时，它向Squid发出一个申请，要Squid替它下载，然后Squid连接所申请网站并请求该主页，接着把该主页传给用户同时保留一个备份，当别的用户申请同样的页面时，Squid把保存的备份立即传给用户，使用户觉得速度相当快。目前，Squid 可以代理HTTP, FTP, GOPHER, SSL 和 WAIS 协议，暂不能代理POP, NNTP等协议。不过，已经有人开始修改Squid，相信不久的将来，Squid能够代理这些协议。
  
Squid能够缓存任何数据吗？不是的。象缓存信用卡帐号、可以远方执行的scripts、经常变换的主页等是不合适的也是不安全的。Squid可以自动的进行处理，你也可以根据自己的需要设置Squid，使之过滤掉你不想要的东西。
  
Squid可以工作在很多的操作系统中，如AIX, Digital Unix, FreeBSD, HP-UX, Irix, Linux, NetBSD, Nextstep, SCO, Solaris，OS/2等，也有不少人在其他操作系统中重新编译过Squid。
  
Squid对硬件的要求是内存一定要大，不应小于128M，硬盘转速越快越好，最好使用服务器专用SCSI硬盘，处理器要求不高，400MH以上既可。
[SQUID能做什么]
    1、代理服务器
    2、透明代理
    3、反向代理
什么是代理服务器
    做为代理服务器，这是SQUID的最基本功能；通过在squid.conf文件里添加一系列访问及控制规则，用户在客户端设置服务器地址和端口，即可通过SQUID访问INTERNET。
什么是透明代理
   所谓透明代理，是相对于代理服务器而言，客户端不需要做任何和代理服务器相关的设置和操作，对用户而言，更本感觉不到代理服务器的存在，所以称之为透明代理。
透明代理流程说明
   用户A发送一个访问请求到防火墙，由防火墙将该用户的访问请求转发到SQUID，SQUID在先检查自身缓存中有无该用户请求的访问内容，如果没有，则请求远端目的服务器，获取该用户的访问内容，在返回给用户的同时，在自身缓存保留一份记录以备下次调用；当用户B发送一个和用户A相同的访问请求时，由防火墙将转发该用户请求到SQUID，SQUID检查自身缓存发现有同样内容后，直接将该内容返回给用户。
注：在实际使用中，通常将SQUID和防火墙放在同一台机器上，为了更清楚的象浏览者描述其工作流程，在以下的流程图中将防火墙和SQUID分开显示。
   ![]()
什么是反向代理
   普通代理方式是代理内部网络用户访问internet上服务器的连接请求，客户端必须指定代理服务器,并将本来要直接发送到internet上服务器的连接请求发送给代理服务器处理。反向代理（Reverse Proxy）方式是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个服务器。
  
反向代理流程说明
   SQUID做为反向代理服务器，通常工作在一个服务器集群的前端，在用户端看来，SQUID服务器就是他所要访问的服务器，而实际意义上SQUID只是接受用户的请求，同时将用户请求转发给内网真正的WEB服务器，如果SQUID本身有用户要访问的内容，则SQUID直接将数据返回给用户。
![]()
[SQUID如何工作]
     ![]()
[#linux+windows](http://hi.baidu.com/tag/linux%2Bwindows/feeds)

分享到： []()[]()[]()[]()

[举报](http://hi.baidu.com/injava/item/b6886fc1fc72af52ac00ef7d#)浏览(117) [评论](http://hi.baidu.com/injava/item/b6886fc1fc72af52ac00ef7d#)[转载](http://hi.baidu.com/injava/item/b6886fc1fc72af52ac00ef7d#)

### 你可能也喜欢

* [![一组速写风景画❤ ~]( "一组速写风景画❤ ~")](http://hi.baidu.com/nieve_kx/item/b48fff87fa92a5ef98255fea)[一组速写风景画❤ ~](http://hi.baidu.com/nieve_kx/item/b48fff87fa92a5ef98255fea)
* [![David Scheirer萌猫酷狗水彩画]( "David Scheirer萌猫酷狗水彩画")](http://hi.baidu.com/ajnsfms/item/b1d2c5ab45b47bde5af191b2)[David Scheirer萌猫酷狗水彩画](http://hi.baidu.com/ajnsfms/item/b1d2c5ab45b47bde5af191b2)
* [![你的梦是什么颜色？]( "你的梦是什么颜色？")](http://hi.baidu.com/xunyaoyang/item/3bfd319772fa24de7b7f01b0)[你的梦是什么颜色？](http://hi.baidu.com/xunyaoyang/item/3bfd319772fa24de7b7f01b0)
* [![世界上最远的距离]( "世界上最远的距离")](http://hi.baidu.com/wxljjoujlibfjvd/item/6d14f0ead253353f8c3ea87d)[世界上最远的距离](http://hi.baidu.com/wxljjoujlibfjvd/item/6d14f0ead253353f8c3ea87d)
* [![板绘]( "板绘")](http://hi.baidu.com/jfvkwmnvjkbcqrd/item/d30978d0635bf5c13cc2cb6e)[板绘](http://hi.baidu.com/jfvkwmnvjkbcqrd/item/d30978d0635bf5c13cc2cb6e)
* [![水彩芭蕾]( "水彩芭蕾")](http://hi.baidu.com/cangbto/item/007c0630e7eb8bafb611dbec)[水彩芭蕾](http://hi.baidu.com/cangbto/item/007c0630e7eb8bafb611dbec)
* [![apache限制并发数 IP 带宽设置教程]( "apache限制并发数 IP 带宽设置教程")](http://hi.baidu.com/injava/item/1e0d9e602de5d1157cdecc7a)[apache限制并发数 IP 带宽设置教程](http://hi.baidu.com/injava/item/1e0d9e602de5d1157cdecc7a)
[](http://hi.baidu.com/injava/item/34ac120d7fa07b113b53ee7d "上一篇")

[](http://hi.baidu.com/injava/item/c444ec13283138011994ec7d "下一篇")

评论
[帮助中心](http://help.baidu.com/question?prod_en=hi) | [空间客服](http://tieba.baidu.com/p/1781683739) | [投诉中心](http://tousu.baidu.com/hi/add) | [空间协议](http://www.baidu.com/search/hi_contract.html)

©2013 Baidu
