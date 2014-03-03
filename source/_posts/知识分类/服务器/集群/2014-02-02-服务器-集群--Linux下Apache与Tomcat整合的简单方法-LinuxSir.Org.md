---
layout: post
title: "Linux下Apache与Tomcat整合的简单方法 - LinuxSir.Org"
categories: 服务器
tags: 
 - 服务器
 - 集群
--- 

# Linux下Apache与Tomcat整合的简单方法 - LinuxSir.Org

[]() [![LinuxSir.Org]( "LinuxSir.Org")](http://www.linuxsir.org/bbs/index.php?s=b91078e77de0bd2a626708973b8082ac)   用户名  密码   记住信息

| [网站首页](http://www.linuxsir.org/) |   [论坛帮助](http://www.linuxsir.org/bbs/faq.php?s=b91078e77de0bd2a626708973b8082ac) |
欢迎来到**LinuxSir.Org**! 您还未登录，请登录后查看论坛，或者点击论坛上方的注册链接注册新账号。
[![返回]( "返回")](http://www.linuxsir.org/bbs/showthread.php?t=236915#)   [LinuxSir.Org](http://www.linuxsir.org/bbs/index.php?s=b91078e77de0bd2a626708973b8082ac) > [Linux 发行版讨论区 —— LinuxSir.Org](http://www.linuxsir.org/bbs/forumdisplay.php?s=b91078e77de0bd2a626708973b8082ac&f=39) > [Linux 发行版Debian专题](http://www.linuxsir.org/bbs/forumdisplay.php?s=b91078e77de0bd2a626708973b8082ac&f=49) > Linux下Apache与Tomcat整合的简单方法
转到页面...  []()  [![发表新主题]( "发表新主题")](http://www.linuxsir.org/bbs/newthread.php?s=b91078e77de0bd2a626708973b8082ac&do=newthread&f=49)  [![回复]( "回复")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&noquote=1&p=1354073)     [主题工具](http://www.linuxsir.org/bbs/showthread.php?t=236915&nojs=1#goto_threadtools)  ![]()

[![旧]( "旧")]() 05-12-23, 20:51 [**第 1 帖**](http://www.linuxsir.org/bbs/showpost.php?s=b91078e77de0bd2a626708973b8082ac&p=1354073&postcount=1) [**Kilin**](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=45616)

![]()
  
[![Kilin 的头像]( "Kilin 的头像")](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=45616)  
 
注册会员  
  注册日期: Jun 2004

  帖子: 321
  [精华](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&goodnees=1&u=45616): 1        **标题: Linux下Apache与Tomcat整合的简单方法**
1、准备，下载需要的文件。这里假定你已经正确安装配置好了JDK。
到Apache官方网站下载所需要的文件：
httpd-2.2.0.tar.gz
apache-tomcat-5.5.12.tar.gz
jakarta-tomcat-connectors-1.2.15-src.tar.gz
其中httpd和jakarta-tomcat-connectors为源码包，apache-tomcat为二进制包。
2、安装Apache。
代码: # tar xzvf httpd-2.2.0.tar.gz # cd httpd-2.2.0 # ./configure --prefix=/usr/local/apache2 --enable-so # make # make install3、安装Tomcat。
代码: # cp apache-tomcat-5.5.12.tar.gz /usr/local/ # cd /usr/local # tar xzvf apache-tomcat-5.5.12.tar.gz # ln -s apache-tomcat-5.5.12 tomcat4、编译生成mod_jk。
代码: # tar xzvf jakarta-tomcat-connectors-1.2.15-src.tar.gz # cd jakarta-tomcat-connectors-1.2.15-src/jk/native # ./configure --with-apxs=/usr/local/apache2/bin/apxs # make # cp ./apache-2.0/mod_jk.so /usr/local/apache2/modules/5、配置。
在/usr/local/apache2/conf/下面建立两个配置文件mod_jk.conf和workers.properties。
# vi mod_jk.conf
添加以下内容：
代码: # 指出mod_jk模块工作所需要的工作文件workers.properties的位置 JkWorkersFile /usr/local/apache2/conf/workers.properties # Where to put jk logs JkLogFile /usr/local/apache2/logs/mod_jk.log # Set the jk log level [debug/error/info] JkLogLevel info # Select the log format JkLogStampFormat "[%a %b %d %H:%M:%S %Y]" # JkOptions indicate to send SSL KEY SIZE, JkOptions +ForwardKeySize +ForwardURICompat -ForwardDirectories # JkRequestLogFormat set the request format JkRequestLogFormat "%w %V %T" # 将所有servlet 和jsp请求通过ajp13的协议送给Tomcat，让Tomcat来处理 JkMount /servlet/* worker1 JkMount /*.jsp worker1# vi workers.properties
添加以下内容：
代码: # Defining a worker named worker1 and of type ajp13 worker.list=worker1 # Set properties for worker1 worker.worker1.type=ajp13 worker.worker1.host=localhost worker.worker1.port=8009 worker.worker1.lbfactor=50 worker.worker1.cachesize=10 worker.worker1.cache_timeout=600 worker.worker1.socket_keepalive=1 worker.worker1.socket_timeout=300再配置httpd.conf，作以下修改：
将Listen 80 修改为 Listen 127.0.0.1:80
将ServerName 修改为 ServerName LocalHost:80
在DirectoryIndex中添加 index.jsp
我的网页放在/var/wwwroot下，所以要修改DocumentRoot
代码: DocumentRoot "/var/wwwroot" <Directory "/var/wwwroot"> Options Includes FollowSymLinks AllowOverride None Order deny,allow Allow from all XBitHack on </Directory> <Directory "/var/wwwroot/WEB-INF"> Order deny,allow Deny from all </Directory>增加关于加载mod_jk的语句：
代码: LoadModule jk_module modules/mod_jk.so Include /usr/local/apache2/conf/mod_jk.conf最后编辑Tomcat的配置文件server.xml，在HOST段中加入：
代码: <Context path="" docBase="/var/wwwroot" debug="0" reloadable="true" crossContext="true"/>在/var/wwwroot下建立一个index.jsp，启动Apache和Tomcat，用浏览器访问http://localhost/，应该可以看到正确的页面了。
__________________
Only the strong survive.
Blog:http://www.ylxiong.cn/
Linux 2.6

*此帖于 05-12-23 20:55 被 Kilin 编辑.*   ![Kilin 当前离线]( "Kilin 当前离线")   [![回复时引用此帖]( "回复时引用此帖")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&p=1354073)

Kilin [查看公开信息](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=45616) [查找 Kilin 发表的更多帖子](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&u=45616)

[![旧]( "旧")]() 05-12-24, 19:59 [**第 2 帖**](http://www.linuxsir.org/bbs/showpost.php?s=b91078e77de0bd2a626708973b8082ac&p=1354576&postcount=2) [**lyingjie**](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  ![帅哥]( "帅哥")

![]()
  
[![lyingjie 的头像]( "lyingjie 的头像")](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  
 
注册会员  
  注册日期: Aug 2005

  帖子: 284
  精华: 0        问一下老兄，Apache2不能用apt进行安装吗
__________________
Debian Linux学习基地
http://www.debian.ha.cn   ![lyingjie 当前离线]( "lyingjie 当前离线")   [![回复时引用此帖]( "回复时引用此帖")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&p=1354576)

lyingjie [查看公开信息](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579) [查找 lyingjie 发表的更多帖子](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&u=89579)
[![旧]( "旧")]() 05-12-24, 20:58 [**第 3 帖**](http://www.linuxsir.org/bbs/showpost.php?s=b91078e77de0bd2a626708973b8082ac&p=1354612&postcount=3) [**lyingjie**](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  ![帅哥]( "帅哥")

![]()
  
[![lyingjie 的头像]( "lyingjie 的头像")](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  
 
注册会员  
  注册日期: Aug 2005

  帖子: 284
  精华: 0        我用apt安装时进入apache2目录后没有conf这个子目录   ![lyingjie 当前离线]( "lyingjie 当前离线")   [![回复时引用此帖]( "回复时引用此帖")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&p=1354612)

lyingjie [查看公开信息](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579) [查找 lyingjie 发表的更多帖子](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&u=89579)

[![旧]( "旧")]() 05-12-24, 21:35 [**第 4 帖**](http://www.linuxsir.org/bbs/showpost.php?s=b91078e77de0bd2a626708973b8082ac&p=1354644&postcount=4) [**Kilin**](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=45616)

![]()
  
[![Kilin 的头像]( "Kilin 的头像")](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=45616)  
 
注册会员  
  注册日期: Jun 2004

  帖子: 321
  [精华](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&goodnees=1&u=45616): 1        可以吧 应该在/etc/local/etc/目录下吧.......
或者在/etc/local/share/etc/下
如果做服务器用的话 建议安装基本系统 然后再自己安装需要的Server端软件。   ![Kilin 当前离线]( "Kilin 当前离线")   [![回复时引用此帖]( "回复时引用此帖")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&p=1354644)

Kilin [查看公开信息](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=45616) [查找 Kilin 发表的更多帖子](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&u=45616)
[![旧]( "旧")]() 05-12-25, 11:35 [**第 5 帖**](http://www.linuxsir.org/bbs/showpost.php?s=b91078e77de0bd2a626708973b8082ac&p=1354810&postcount=5) [**lyingjie**](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  ![帅哥]( "帅哥")

![]()
  
[![lyingjie 的头像]( "lyingjie 的头像")](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  
 
注册会员  
  注册日期: Aug 2005

  帖子: 284
  精华: 0        谢了我在试试   ![lyingjie 当前离线]( "lyingjie 当前离线")   [![回复时引用此帖]( "回复时引用此帖")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&p=1354810)

lyingjie [查看公开信息](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579) [查找 lyingjie 发表的更多帖子](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&u=89579)

[![旧]( "旧")]() 05-12-25, 11:58 [**第 6 帖**](http://www.linuxsir.org/bbs/showpost.php?s=b91078e77de0bd2a626708973b8082ac&p=1354826&postcount=6) [**Kilin**](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=45616)

![]()
  
[![Kilin 的头像]( "Kilin 的头像")](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=45616)  
 
注册会员  
  注册日期: Jun 2004

  帖子: 321
  [精华](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&goodnees=1&u=45616): 1        汗.....昨天喝高了 把路径打得一塌糊涂.....
应该是/etc
或者是/usr/etc
或者是/usr/local/etc
猜的~   ![Kilin 当前离线]( "Kilin 当前离线")   [![回复时引用此帖]( "回复时引用此帖")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&p=1354826)

Kilin [查看公开信息](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=45616) [查找 Kilin 发表的更多帖子](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&u=45616)
[![旧]( "旧")]() 05-12-25, 14:24 [**第 7 帖**](http://www.linuxsir.org/bbs/showpost.php?s=b91078e77de0bd2a626708973b8082ac&p=1354897&postcount=7) [**lyingjie**](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  ![帅哥]( "帅哥")

![]()
  
[![lyingjie 的头像]( "lyingjie 的头像")](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  
 
注册会员  
  注册日期: Aug 2005

  帖子: 284
  精华: 0        好的，那在问你一个问题我现在按照你所讲的我已经配成了JSP可以为什么我打开静态页是打不开呀？
提示你无权查看该页   ![lyingjie 当前离线]( "lyingjie 当前离线")   [![回复时引用此帖]( "回复时引用此帖")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&p=1354897)

lyingjie [查看公开信息](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579) [查找 lyingjie 发表的更多帖子](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&u=89579)

[![旧]( "旧")]() 05-12-25, 17:38 [**第 8 帖**](http://www.linuxsir.org/bbs/showpost.php?s=b91078e77de0bd2a626708973b8082ac&p=1354988&postcount=8) [**Kilin**](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=45616)

![]()
  
[![Kilin 的头像]( "Kilin 的头像")](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=45616)  
 
注册会员  
  注册日期: Jun 2004

  帖子: 321
  [精华](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&goodnees=1&u=45616): 1        httpd.conf里的权限设置.....   ![Kilin 当前离线]( "Kilin 当前离线")   [![回复时引用此帖]( "回复时引用此帖")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&p=1354988)

Kilin [查看公开信息](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=45616) [查找 Kilin 发表的更多帖子](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&u=45616)
[![旧]( "旧")]() 05-12-25, 17:43 [**第 9 帖**](http://www.linuxsir.org/bbs/showpost.php?s=b91078e77de0bd2a626708973b8082ac&p=1354995&postcount=9) [**lyingjie**](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  ![帅哥]( "帅哥")

![]()
  
[![lyingjie 的头像]( "lyingjie 的头像")](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  
 
注册会员  
  注册日期: Aug 2005

  帖子: 284
  精华: 0        能不能说的在明白点呀，我以前做LAMP时有时也会出现这样子的问题   ![lyingjie 当前离线]( "lyingjie 当前离线")   [![回复时引用此帖]( "回复时引用此帖")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&p=1354995)

lyingjie [查看公开信息](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579) [查找 lyingjie 发表的更多帖子](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&u=89579)

[![旧]( "旧")]() 05-12-26, 09:59 [**第 10 帖**](http://www.linuxsir.org/bbs/showpost.php?s=b91078e77de0bd2a626708973b8082ac&p=1355323&postcount=10) [**lyingjie**](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  ![帅哥]( "帅哥")

![]()
  
[![lyingjie 的头像]( "lyingjie 的头像")](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  
 
注册会员  
  注册日期: Aug 2005

  帖子: 284
  精华: 0        还有我如果用Apt安装的话那不能给讲讲如何做呀   ![lyingjie 当前离线]( "lyingjie 当前离线")   [![回复时引用此帖]( "回复时引用此帖")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&p=1355323)

lyingjie [查看公开信息](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579) [查找 lyingjie 发表的更多帖子](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&u=89579)
[![旧]( "旧")]() 05-12-27, 22:02 [**第 11 帖**](http://www.linuxsir.org/bbs/showpost.php?s=b91078e77de0bd2a626708973b8082ac&p=1356579&postcount=11) [**marvel**](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=55742)

![]()
  
 
 
注册会员  
  注册日期: Oct 2004

  帖子: 489
  精华: 0        apache配置文件的默认路径应该是/etc/apache/httpd.conf吧
__________________
Thinkpad T60
酷睿 1.83GHz+512Mb DDR+80Gb+ATI(64Mb)
Debian unstable   ![marvel 当前离线]( "marvel 当前离线")   [![回复时引用此帖]( "回复时引用此帖")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&p=1356579)

marvel [查看公开信息](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=55742) [访问 marvel 的个人网站](http://marvel.hit.edu.cn/) [查找 marvel 发表的更多帖子](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&u=55742)

[![旧]( "旧")]() 05-12-28, 03:25 [**第 12 帖**](http://www.linuxsir.org/bbs/showpost.php?s=b91078e77de0bd2a626708973b8082ac&p=1356702&postcount=12) [**wjl**](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=27503)

![]()
  
[![wjl 的头像]( "wjl 的头像")](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=27503)  
 
注册会员  
  注册日期: Nov 2003

  我的住址: 北京
  帖子: 90

  精华: 0
  good谢谢
__________________
FreeBSD
RHEL AS
Windows XP
----------------------------------------------------------------------------
借我一生,好好爱你   ![wjl 当前离线]( "wjl 当前离线")   [![回复时引用此帖]( "回复时引用此帖")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&p=1356702)

wjl [查看公开信息](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=27503) [查找 wjl 发表的更多帖子](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&u=27503)
[![旧]( "旧")]() 05-12-28, 10:35 [**第 13 帖**](http://www.linuxsir.org/bbs/showpost.php?s=b91078e77de0bd2a626708973b8082ac&p=1356852&postcount=13) [**lyingjie**](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  ![帅哥]( "帅哥")

![]()
  
[![lyingjie 的头像]( "lyingjie 的头像")](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579)  
 
注册会员  
  注册日期: Aug 2005

  帖子: 284
  精华: 0        你说的是Apache1.3的位置   ![lyingjie 当前离线]( "lyingjie 当前离线")   [![回复时引用此帖]( "回复时引用此帖")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&p=1356852)

lyingjie [查看公开信息](http://www.linuxsir.org/bbs/member.php?s=b91078e77de0bd2a626708973b8082ac&u=89579) [查找 lyingjie 发表的更多帖子](http://www.linuxsir.org/bbs/search.php?s=b91078e77de0bd2a626708973b8082ac&do=finduser&u=89579)
[![发表新主题]( "发表新主题")](http://www.linuxsir.org/bbs/newthread.php?s=b91078e77de0bd2a626708973b8082ac&do=newthread&f=49)  [![回复]( "回复")](http://www.linuxsir.org/bbs/newreply.php?s=b91078e77de0bd2a626708973b8082ac&do=newreply&noquote=1&p=1356852)
**«** [上一主题](http://www.linuxsir.org/bbs/showthread.php?s=b91078e77de0bd2a626708973b8082ac&t=236915&goto=nextoldest) | [下一主题](http://www.linuxsir.org/bbs/showthread.php?s=b91078e77de0bd2a626708973b8082ac&t=236915&goto=nextnewest) **»**
主题工具[]() ![显示可打印版本]( "显示可打印版本") [显示可打印版本](http://www.linuxsir.org/bbs/printthread.php?s=b91078e77de0bd2a626708973b8082ac&t=236915) ![邮寄本页给好友]( "邮寄本页给好友") [邮寄本页给好友](http://www.linuxsir.org/bbs/sendmessage.php?s=b91078e77de0bd2a626708973b8082ac&do=sendtofriend&t=236915)      [![]()](http://www.linuxsir.org/bbs/showthread.php?t=236915#top) 发帖规则 您 [不可以] 发表新主题

您 [不可以] 回复主题
您 [不可以] 上传附件

您 [不可以] 编辑您的帖子
已 [启用] [BB 代码](http://www.linuxsir.org/bbs/misc.php?s=b91078e77de0bd2a626708973b8082ac&do=bbcode)

已 [启用] [表情符号](http://www.linuxsir.org/bbs/misc.php?s=b91078e77de0bd2a626708973b8082ac&do=showsmilies)
已 [启用] [IMG 代码](http://www.linuxsir.org/bbs/misc.php?s=b91078e77de0bd2a626708973b8082ac&do=bbcode#imgcode)

已 [禁用] HTML 代码 [论坛跳转…]
用户控制面板 悄悄话 收藏夹 会员在线状态 搜索论坛 论坛首页    Linux 综合讨论区 —— LinuxSir.Org     Linux 基础建设讨论专版     Linux shell进阶应用与shell编程     Linux 专业英文精品技术文档专题         Gas中文小组讨论区     Linux 硬件及周边设备     Linux 网络与服务器架设     Linux 系统及网络安全讨论专版     Linux及计算机学科基础理论版  Linux 发行版讨论区 —— LinuxSir.Org     Linux 发行版SuSE专题     Linux 发行版Archlinux讨论区     Linux 发行版Debian专题     Ubuntu Linux 专题讨论     Linux 发行版Slackware专题     Linux 发行版 LFS 讨论区         Olive讨论区 （实验版）     Linux 发行版Mandriva专题     Linux 发行版Redhat/Fedora/CentOS专题     Linux 发行版Gentoo讨论区     Mini Linux 及准系统研究     Linux 发行版红旗专题     LoongSon-龙芯讨论区     Linux 发行版其他专题         Linux发行版 Turbolinux专题         PowerPC Linux 讨论区  Google 产品讨论区 —— LinuxSir.Org     Android  Linux 软件应用讨论区 —— LinuxSir.Org     Linux 输入开发与研究     Linux 软件专题讨论         软件下载讨论区         即时通讯  Linux 高级应用讨论区 —— LinuxSir.Org     Linux 数据库专题讨论     Linux 认证考试学习与经验交流     Linux 内核研究小组     Linux 企业级应用专题讨论  编程开发讨论区 —— LinuxSir.Org     Linux 程序设计专题讨论     Java 程序设计开发讨论     Perl | PHP | Python 脚本程序开发     嵌入式Linux讨论区──实验田版  Unix 技术讨论区 —— LinuxSir.Org     BSD 讨论专题         BSD 新闻安全观察         RelaxBSD 讨论区     Solaris 讨论专题     MacOSX & Darwin 讨论专题  社区中心 —— LinuxSir.Org     LinuxSir 论坛管理         LinuxSir 论坛临时存放区     LinuxSir 文章管理系统和BBS程序研究小组     小企鹅新闻图书馆     LinuxSir.Org 同城行 ── 我的城市
所有时间均为[北京时间]。现在的时间是 21:28。
**[网站首页](http://www.linuxsir.org/) - [联系我们](mailto:kevin@linuxsir.org) - [返回顶端](http://www.linuxsir.org/bbs/showthread.php?t=236915#top)**
Powered by vBulletin 版本 3.6.8
版权所有 ©2000 - 2012, Jelsoft Enterprises Ltd.
官方中文技术支持: [vBulletin 中文](http://www.vbulletin-china.cn/)

版权所有 ©2002 - 2011, LinuxSir.Org
