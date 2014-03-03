---
layout: post
title: "apache+tomcat整合(LINUX) - 技术文档 - 网络技术 Linux时代 - 开源、"
categories: 服务器
tags: 
 - 服务器
 - 集群
--- 

# apache+tomcat整合(LINUX) - 技术文档 - 网络技术 Linux时代 - 开源、自由、共享 - 中国最大的Linux技术社区

·[ChinaUnix首页](http://www.chinaunix.net/) ·[论坛](http://bbs.chinaunix.net/) ·[博客](http://blog.chinaunix.net/)   ![]() ![]() [Linux首页](http://linux.chinaunix.net/) | [Linux新闻](http://linux.chinaunix.net/news/) | [**Linux论坛**](http://linux.chinaunix.net/bbs) | [Linux文档](http://linux.chinaunix.net/techdoc/) | [Linux下载](http://download.chinaunix.net/disc/linux/) | [Linux博客](http://blog.chinaunix.net/techart.php?frmid=6) | [Linux搜索](http://search.chinaunix.net/) | [开源项目孵化平台](http://linux.chinaunix.net/bbs/index.php?gid=68) | [《开源时代》](http://linux.chinaunix.net/ebook/) ![]() [新手入门](http://linux.chinaunix.net/techdoc/beginner/) |  [安装启动](http://linux.chinaunix.net/techdoc/install/) |  [管理员指南](http://linux.chinaunix.net/techdoc/system/) | [开发手册](http://man.chinaunix.net/) | [桌面应用](http://linux.chinaunix.net/techdoc/desktop/) | [程序开发](http://linux.chinaunix.net/techdoc/develop/) | [数据库](http://linux.chinaunix.net/techdoc/database/) |  [网络技术](http://linux.chinaunix.net/techdoc/net/)| [CentOS](http://linux.chinaunix.net/topics/2007-01-25/15/index.shtml) | [Fedora](http://linux.chinaunix.net/search2.php?key=fedora&id=0) |  [MySQL](http://linux.chinaunix.net/search2.php?key=mysql&id=0) |  [Apache](http://linux.chinaunix.net/search2.php?key=apache&id=0) |  [Ubuntu](http://linux.chinaunix.net/search2.php?key=ubuntu&id=0) |  [Gentoo](http://linux.chinaunix.net/topics/2008-07-10/18/index.shtml)| **[OSCON08](http://linux.chinaunix.net/topics/2008-07-30/19/index.shtml)**    [Linux时代](http://linux.chinaunix.net/) >> [技术文档](http://linux.chinaunix.net/techdoc/) >> [网络技术](http://linux.chinaunix.net/techdoc/net/)  apache+tomcat整合(LINUX)  来源: ChinaUnix博客 　日期： 2007.12.28 13:38　(共有条评论) [我要评论](http://linux.chinaunix.net/bbs/thread-975323-1-1.html)   这里介绍两类方法：
    这里假设你已安装并且配置好了JDK
                           一：用apt-get 安装
apt-get install apache2 tomcat5.5 libapache2-mod-jk
设置Tomcat管理员帐号
Tomcat的用户帐号信息都保存在tomcat-users.xml的文件中，运行
sudo gedit /usr/share/tomcat6/conf/tomcat-users.xml在的标签前添加一行
保存并关闭。重新运行tomcat即可输入该用户名和密码，登录Tomcat的管理页面。
确认一下在apache2的启动模块中是否有jk.load
#sudo ls /etc/apache2/mods-enabled/
#sudo vi /etc/libapache2-mod-jk/workers.properties
workers.tomcat_home=/usr/share/tomcat6
workers.java_home=/usr/lib/jvm/java-6-sun
#sudo vi /usr/share/doc/libapache2-mod-jk/httpd_example_apache2.conf
复制里面的内容到apache2.conf
最后重启apache tomcat上传jsp文件到usr/share/tomcat6/webapps/ROOT
                               二:手动下载安装
1.安装jdk
2.安装tomcat
3.编译安装apache2
4.停止以上两个启动的服务:tomcat,apache
5.编译安装jk-connector
6.编辑配置文件
7.看结果吧
详细流程：
1.安装jdk
1.1
下载jdk包，可以是rpm包或者tar包，解压缩到某目录，然后
ln -s /usr/local/src/jdk-1.6.* /usr/local/java
1.2
设置环境变量
vi /etc/profile
export JAVA_HOME=/usr/local/java/
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JRE_HOME=$JAVA_HOME/jre
1.3
启用环境变量
source /etc/profile
1.4
检验成功否
java -version 或 javac -v
2.安装tomcat
tar zxvf /mnt/hgfs/Untitled-1/apache-tomcat-5.5.12.tar.gz -C /usr/local/src/
ln -s /usr/local/src/apache-tomcat-5.5.12 /usr/local/tomcat
/usr/local/tomcat/bin/startup.sh
在浏览器中输入http://IP地址:8080
可以看到大花猫了。
3.编译安装apache2
tar zxvf /mnt/hgfs/Untitled-1/httpd-2.2.4.tar.gz  -C /usr/local/src/
cd /usr/local/src/httpd-2.2.4
在进行编译之前要确保系统中没有apr或者apr-util的rpm包或者deb包。下面一下子编译安装了apr，apr-util，httpd
cd srclib/apr && ./configure --prefix=/usr/local/apr && make && make install && cd ../apr-util && ./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr && make && make install && cd ../.. && ./configure --prefix=/usr/local/httpd --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util --enable-so && make && make install
下面编辑httpd.conf文件中ServerName字段的注释符，可以换成你的IP地址，或者什么都不换。
下面启动apache看看效果
/usr/local/httpd/bin/apachectl -k start
看到了It Works就对了。
4.停止以上两个启动的服务:tomcat,apache。
/usr/local/tomcat/bin/shutdown.sh
/usr/local/httpd/bin/apachectl stop
5.编译安装jk-connector
tar zxvf /mnt/hgfs/Untitled-1/tomcat-connectors-1.2.21-src.tar.gz -C /usr/local/src/
cd /usr/local/src/tomcat-connectors-1.2.21-src/native/
编译安装jk-connector，带上apxs的地址，会在安装的时候自动把mod_jk.so放到/usr/local/httpd/modules里面
./configure --with-apxs=/usr/local/httpd/bin/apxs && make && make install
ls /usr/local/httpd/modules
看到mod_jk.so表示成功
6.编辑配置文件
6.1编辑/usr/local/httpd/conf/mod_jk.conf
# 指出mod_jk模块工作所需要的工作文件workers.properties的位置
JkWorkersFile /usr/local/httpd/conf/workers.properties
# Where to put jk logs
JkLogFile /var/log/apache2/mod_jk.log
# Set the jk log level [debug/error/info]
JkLogLevel info
# Select the log format
JkLogStampFormat "[%a %b %d %H:%M:%S %Y]"
# JkOptions indicate to send SSL KEY SIZE,
JkOptions  +ForwardKeySize +ForwardURICompat -ForwardDirectories
# JkRequestLogFormat set the request format
JkRequestLogFormat "%w %V %T"
# 将所有servlet 和jsp请求通过ajp13的协议送给Tomcat，让Tomcat来处理
JkMount /servlet/*  worker1
JkMount /*.jsp worker1
6.2编辑/usr/local/httpd/conf/workers.properties
# Defining a worker named worker1 and of type ajp13
worker.list=worker1
# Set properties for worker1
worker.worker1.type=ajp13  
worker.worker1.host=localhost  
worker.worker1.port=8009
worker.worker1.lbfactor=50  
worker.worker1.cachesize=10  
worker.worker1.cache_timeout=600  
worker.worker1.socket_keepalive=1  
worker.worker1.socket_timeout=300
6.3编辑httpd.conf文件
加入如下：
LoadModule jk_module modules/mod_jk.so
Include /usr/local/httpd/conf/mod_jk.conf
修改htdoc的默认地址为/usr/local/tomcat/webapps
修改htdoc的参数，也就是给原来htdoc的目录参数给webapps
修改DirectoryIndex index.html index.htm index.jsp
7.看结果吧
http://IP地址
可以看到一只大花猫。ok，成功了。
如何修改tomcat单独启动时候的端口为80？
   
   
里面的Connector port="8080"改为Connector port="80"
               
               
               
**本文来自ChinaUnix博客，如果查看原文请点：**[http://blog.chinaunix.net/u1/51404/showart_452032.html](http://blog.chinaunix.net/u1/51404/showart_452032.html) [发表评论](http://linux.chinaunix.net/bbs/thread-975323-1-1.html) [查看评论](http://linux.chinaunix.net/bbs/thread-975323-1-1.html)(共有条评论) [我要提问](http://linux.chinaunix.net/bbs/forum-4-1.html)     最新资讯[更多>>](http://linux.chinaunix.net/news)  · [金山卫士开源计划首周源码下载..](http://linux.chinaunix.net/news/2010/12/10/1175490.shtml "金山卫士开源计划首周源码下载突破两万")
· [谷歌劝说诺基亚采用Android操作..](http://linux.chinaunix.net/news/2010/12/10/1175491.shtml "谷歌劝说诺基亚采用Android操作系统")
· [11月份Linux市场占有率升至5%](http://linux.chinaunix.net/news/2010/12/10/1175492.shtml "11月份Linux市场占有率升至5%")
· [Apache 基金会确认退出 JCP 执..](http://linux.chinaunix.net/news/2010/12/10/1175493.shtml "Apache 基金会确认退出 JCP 执行委员会")
· [Chrome 10 新功能探秘：新增GP..](http://linux.chinaunix.net/news/2010/12/07/1175306.shtml "Chrome 10 新功能探秘：新增GPU混合加速功能")
· [金山宣布开源其安全软件](http://linux.chinaunix.net/news/2010/12/07/1175307.shtml "金山宣布开源其安全软件")
· [开源FTP服务器ProFTPD发现后门](http://linux.chinaunix.net/news/2010/12/07/1175308.shtml "开源FTP服务器ProFTPD发现后门")
· [女黑客在开源会议上抱受骚扰](http://linux.chinaunix.net/news/2010/12/07/1175309.shtml "女黑客在开源会议上抱受骚扰")
· [21款值得关注的Linux游戏](http://linux.chinaunix.net/news/2010/12/07/1175310.shtml "21款值得关注的Linux游戏")
· [马化腾：腾讯半年后彻底转型，..](http://linux.chinaunix.net/news/2010/12/07/1175311.shtml "马化腾：腾讯半年后彻底转型，开放和分享") 论坛热点[更多>>](http://linux.chinaunix.net/bbs)  · [Linux系统移植从零开始！参与..](http://linux.chinaunix.net/bbs/thread-1177273-1-1.html "Linux系统移植从零开始！参与讨论即有机会获得图书一本！")
· [学习linux的意义在哪里](http://linux.chinaunix.net/bbs/thread-1177271-1-1.html "学习linux的意义在哪里")
· [使用netfilter在哪能获取到原..](http://linux.chinaunix.net/bbs/thread-1177409-1-1.html "使用netfilter在哪能获取到原始以太包")
· [哥纠结了](http://linux.chinaunix.net/bbs/thread-1177298-1-1.html "哥纠结了")
· [一个在线读开源代码的工具，..](http://linux.chinaunix.net/bbs/thread-1177227-1-1.html "一个在线读开源代码的工具，希望版主能保留几天，谢谢！")
· [为什么我的目录下没有.cshrc..](http://linux.chinaunix.net/bbs/thread-1177374-1-1.html "为什么我的目录下没有.cshrc文件？")
· [初学linux从哪里开始](http://linux.chinaunix.net/bbs/thread-1177344-1-1.html "初学linux从哪里开始")
· [linux 系统无法上网](http://linux.chinaunix.net/bbs/thread-1177382-1-1.html "linux 系统无法上网")
· [新手安装UCenter 时总是出错..](http://linux.chinaunix.net/bbs/thread-1177380-1-1.html "新手安装UCenter 时总是出错。望高手指点")
· [cacti添加主机显示的状态都是..](http://linux.chinaunix.net/bbs/thread-1177274-1-1.html "cacti添加主机显示的状态都是unknow，不能进行资源绘图") 文档更新[更多>>](http://linux.chinaunix.net/techdoc)  · [菜鸟入门三星ARM11嵌入式系统，是..](http://linux.chinaunix.net/techdoc/beginner/2010/11/17/1174196.shtml "菜鸟入门三星ARM11嵌入式系统，是否困难？")
· [寻redhat 5.3 的中文手册 for ia64](http://linux.chinaunix.net/techdoc/beginner/2010/09/16/1171153.shtml "寻redhat 5.3 的中文手册 for ia64")
· [请问redhat 5.3 企业版的用户手册..](http://linux.chinaunix.net/techdoc/beginner/2010/09/06/1170624.shtml "请问redhat 5.3 企业版的用户手册去哪里下载？")
· [LINUX与UNIX SHELL编程指南(中文)](http://linux.chinaunix.net/techdoc/beginner/2010/08/18/1169548.shtml "LINUX与UNIX SHELL编程指南(中文)")
· [一些基本用户管理以及基本安装方法](http://linux.chinaunix.net/techdoc/beginner/2010/03/24/1160550.shtml "一些基本用户管理以及基本安装方法")
· [菜鸟学习linux笔记与练习-----第..](http://linux.chinaunix.net/techdoc/beginner/2010/03/23/1160525.shtml "菜鸟学习linux笔记与练习-----第二天。一些基本命令以及初级网络配置")
· [菜鸟学习linux笔记与练习-----第..](http://linux.chinaunix.net/techdoc/beginner/2010/03/23/1160523.shtml "菜鸟学习linux笔记与练习-----第一天。一些初级命令以及基本用户管理")
· [服务器配置：Squid配置详解](http://linux.chinaunix.net/techdoc/beginner/2010/03/22/1160427.shtml "服务器配置：Squid配置详解")
· [linux下u盘使用](http://linux.chinaunix.net/techdoc/install/2010/01/29/1156159.shtml "linux下u盘使用")
· [ubuntu dynamips 绑定网卡到虚拟机](http://linux.chinaunix.net/techdoc/install/2010/01/29/1156155.shtml "ubuntu dynamips 绑定网卡到虚拟机")    [关于我们](http://www.chinaunix.net/about/index.shtml) | [联系方式](http://www.chinaunix.net/about/connect.html) | [广告合作](http://www.chinaunix.net/about/service.html) | [诚聘英才](http://www.chinaunix.net/about/hr.html) | [网站地图](http://www.chinaunix.net/about/channel.html) | [友情链接](http://www.chinaunix.net/about/friend.html) | [免费注册](http://linux.chinaunix.net/bbs/register.php)  Copyright © 2001-2009 ChinaUnix.net All Rights Reserved

感谢所有关心和支持过ChinaUnix的朋友们

[京ICP证:060528号](http://www.it168.com/images/icp.jpg)
