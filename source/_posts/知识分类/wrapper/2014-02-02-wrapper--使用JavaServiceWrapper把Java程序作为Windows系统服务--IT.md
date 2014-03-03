---
layout: post
title: "使用Java Service Wrapper 把Java程序作为Windows系统服务"
categories: wrapper
tags: 
 - wrapper
--- 

\# 使用Java Service Wrapper 把Java程序作为Windows系统服务 


Java程序很多情况下是作为服务程序运行的，在Unix平台下可以利用在命令后加“&”把程序作为后台服务运行，但在Windows下看作那个Console窗口在桌面上，你是否一直担心别的同时把你的Console窗口关闭？是否怀念用VC写的Win32服务程序
翻开JBOSS、Tomcat的发布包，发现他们都使用了一个Open source——Java Service Wrapper。用Java Service Wrapper可以轻松解决我们的需求，让我们的服务程序成为 Win32系统服务。
当然，在Unix下也可以使用Java Service Wrapper，可以避免加“&”的粗暴方式，导致每天收到一堆mail，通过Java Service Wrapper提供的日志方式查看运行信息。
Java Service Wrapper功能很强大，同时支持Windows及Unix平台，提供三种方式把你的Java程序包装成系统服务，这里只介绍最简单的一种方式，因这种方式无需对已有的服务程序作任何改变，仅仅增加几个script、配置文件就可以把你的Java服务程序改造成系统服务程序了。
当然在使用之前需要到http://sourceforge.net/project/showfiles.php?group_id=39428下载Java Service Wrapper的发布包。
下面简单介绍一下具体的使用步骤：
1.  将下载的Java Service Wrapper包解压到本地，目录为{WRAPPER_HOME}；
2.  服务应用程序名为MyServApp，在目录d:\MyServApp下建立bin、conf、logs、lib目录；并把你的已有应用程序如NioBlockingServer.class拷贝到该目录下；
3.  将{WRAPPER_HOME}\src\bin\下的遗以下文件拷贝到MyServApp目录下，并重命名。
{WRAPPER_HOME}\bin\Wrapper.exe  C:\ MyServApp \bin\Wrapper.exe
{WRAPPER_HOME}\src\bin\App.bat.in  C:\ MyServApp\bin\MyApp.bat
{WRAPPER_HOME}\src\bin\InstallApp-NT.bat.in  C:\ MyServApp\bin\InstallMyApp-NT.bat
{WRAPPER_HOME}\src\bin\UninstallApp-NT.bat.in  C:\ MyServApp\bin\UninstallMyApp-NT.bat
4.  将{WRAPPER_HOME}\lib下的以下文件拷贝到C:\ MyServApp \lib目录下
{WRAPPER_HOME}\lib\Wrapper.DLL
{WRAPPER_HOME}\lib\wrapper.jar
5.  将{WRAPPER_HOME}\src\conf\wrapper.conf.in拷贝到C:\ MyServApp \conf目录下并命名为wrapper.conf；并修改wrapper.conf文件，在其中配置您的应用服务。
主要修改以下几项即可：
\#你的JVM位置：
wrapper.java.command=D:\Sun\j2sdk1.4.0_03\bin\java
\#运行参数：如：
wrapper.java.additional.1=-Dprogram.name=run.bat
\#classpath：
wrapper.java.classpath.1=../lib/wrapper.jar
wrapper.java.classpath.2=../bin/.
\# Java Library Path (location of Wrapper.DLL or libwrapper.so)
wrapper.java.library.path.1=../lib
\#MAIN CLASS 此处决定了使用Java Service Wrapper的方式
wrapper.java.mainclass=org.tanukisoftware.wrapper.WrapperSimpleApp
\#你的Java应用类
wrapper.app.parameter.1= NonBlockingServer
\# 服务名
wrapper.ntservice.name=NB
\# Display name of the service
wrapper.ntservice.displayname=Nio Nonblocking Server
\# 服务描述
wrapper.ntservice.description=Nio Nonblocking Server
其他的配置根据你的需要改变即可
6.  对以上配置的MyApp.bat进行测试，运行MyApp.bat，就像在Console窗口下运行Tomcat一样；
7.  对以上配置的服务进行测试，运行C:\ MyServApp\bin\InstallMyApp-NT.bat将把你的应用（此处为NioBlockingServer）安装到Win32系统服务中了。
8.  打开控制面板－管理程序－服务，看到Nio Nonblocking Server已经在系统服务中了，其他用法就与我们熟悉的Windows服务一样了。
Tomcat使用的是Java Service Wrapper模式二，这种方式需要对已有的程序进行小的改动，但可以通过Socket端口的方式控制服务程序核心的启动，更加灵活。Java Service Wrapper提供的模式三比较复杂，需要作出更多的编码，我没有研究。
采用模式一，即可简单有效的把我们的服务程序包装成为系统服务程序，并增强了日志功能，我们可以把MyServApp的几个文件做成模板，每次修改文件名，配置文件就可以了，有精力的朋友更可以做成Eclipse的plugin，鼠标点点就把应用配成服务了。
附件是一个模板，可以直接修改文件名和配置文件就可以把服务改造了。
希望本文能对大家有所帮助，当然也申请加分了，我的2分实在不够混的了。 
[/Files/cai9911/NTServiceWrapperTemplate.rar](http://files.cnblogs.com/cai9911/NTServiceWrapperTemplate.rar)
====================
自己做的一个例子,可以直接拿来修改:
(写下来备忘)
主要修改config/wrapper.conf 一个地方:
wrapper.app.parameter.1= HelloWorld  <要启动的类>
其他的可以修改应用程序的服务名称等.
[/Files/cai9911/JavaService.rar](http://files.cnblogs.com/cai9911/JavaService.rar)
