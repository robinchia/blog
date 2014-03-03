---
layout: post
title: "使用Java Service Wrapper设置tomcat作为linux服务并且开机自动启动"
categories: wrapper
tags: 
 - wrapper
--- 


前几天总结了使用JSVC来设置tomcat作为linux服务并且开机自动启动，但是如果要更专业一点来控制tomcat的启动，使用Java Service Wrapper应该不失为一个好的选择，下面来总结一下Java Service Wrapper在Linux中对于tomcat启动的设置：  
\**1、\** 安装JDK、Tomcat,此处略过。比如tomcat安装在\opt\tomcat目录中。  
\**2\** 、使用命令wget 下载Java Service Wrapper(目前版本wrapper-linux-x86-32-3.2.3，官网：http:\\wrapper.tanukisoftware.org), 用命令tar -zxvf wrapper-linux-x86-32-3.2.3.tar.gz 解压，得到目录wrapper-linux-x86-32-3.2.3，使用ln -s wrapper-linux-x86-32-3.2.3 wrapper 给目录wrapper-linux-x86-32-3.2.3 建一个名称为wrapper的快捷方式。  
\**3\** 、复制wrapper\src\bin\sh.script.in到\opt\tomcat\bin目录，重命名为tomcat
复制wrapper\src\conf\wrapper.conf.in到\opt\tomcat\conf目录，重命名为wrapper.conf  
复制wrapper\lib目录下的所有3个文件到\opt\tomcat\lib目录  
复制wrapper\bin目录下的wrapper文件到\opt\tomcat\bin目录  
\**4\** 、修改\opt\testapp\bin\tomcat文件  
APP_NAME="tomcat"  
APP_LONG_NAME="Tomcat Application Server"  
WRAPPER_CMD=".\wrapper"  
WRAPPER_CONF="..\conf\wrapper.conf"  
赋予执行权限  
chmod 775 \opt\tomcat\bin\tomcat  
chmod 775 \opt\tomcat\bin\tomcat  
\**5\** 、修改\opt\tomcat\conf\wrapper.conf文件，如：  
\#********************************************************************  
\# Wrapper Properties  
\#********************************************************************  
\# Java Application  
\# 设置环境变量  
set.JAVA_HOME=\usr\java\jdk1.6.0_01  
set.CATALINA_HOME=\opt\tomcat  
set.CATALINA_BASE=\opt\tomcat  
wrapper.java.command=\usr\java\jdk1.6.0_01\bin\java  
\# Java Main class. This class must implement the WrapperListener interface  
\# or guarantee that the WrapperManager class is initialized. Helper  
\# classes are provided to do this for you. See the Integration section  
\# of the documentation for details.  
\# 使用WrapperStartStopApp，这样可以通过命令带start\stop来启动\停止程序。  
wrapper.java.mainclass=org.tanukisoftware.wrapper.WrapperStartStopApp  
\# Java Classpath (include wrapper.jar) Add class path elements as  
\# needed starting from 1  
\# 设置执行tomcat的classpath文件  
wrapper.java.classpath.1=%CATALINA_HOME%\lib\wrapper.jar  
wrapper.java.classpath.2=%CATALINA_BASE%\bin\bootstrap.jar    
\# Java Library Path (location of Wrapper.DLL or libwrapper.so)    
\# 设置tomcat的lib路径  
wrapper.java.library.path.1=%CATALINA_HOME%\lib\  
\# Java Additional Parameters  
\# 设置额外参数  
wrapper.java.additional.1=-Djava.endorsed.dirs=%CATALINA_HOME%\common\endorsed
wrapper.java.additional.2=-Dcatalina.base=%CATALINA_BASE%
wrapper.java.additional.3=-Dcatalina.home=%CATALINA_HOME%
wrapper.java.additional.4=-Djava.io.tmpdir=%CATALINA_BASE%\temp
\# Initial Java Heap Size (in MB)  
\# 设置tomcat的JVM初始化堆的大小  
wrapper.java.initmemory=128
\# Maximum Java Heap Size (in MB)  
\# 设置tomcat的JVM堆的最大值  
wrapper.java.maxmemory=512
\# Application parameters. Add parameters as needed starting from 1  
\# 设置启动、停止和重启参数
wrapper.app.parameter.1=org.apache.catalina.startup.Bootstrap
wrapper.app.parameter.2=1
wrapper.app.parameter.3=start
wrapper.app.parameter.4=org.apache.catalina.startup.Bootstrap
wrapper.app.parameter.5=true
wrapper.app.parameter.6=1
wrapper.app.parameter.7=stop
wrapper.filter.trigger.1=java.lang.OutOfMemoryError
wrapper.filter.action.1=RESTART
\#********************************************************************  
\# Wrapper Logging Properties  
\#********************************************************************  
\# Format of output for the console. (See docs for formats)  
wrapper.console.format=PM
\# Log Level for console output. (See docs for log levels)  
wrapper.console.loglevel=INFO
\# Log file to use for wrapper output logging.  
\# 设置log文件路径  
wrapper.logfile=%CATALINA_BASE%\logs\wrapper.log
\# Format of output for the log file. (See docs for formats)  
wrapper.logfile.format=LPTM
\# Log Level for log file output. (See docs for log levels)  
wrapper.logfile.loglevel=INFO
\# Maximum size that the log file will be allowed to grow to before
\# the log is rolled. Size is specified in bytes. The default value
\# of 0, disables log rolling. May abbreviate with the 'k' (kb) or
\# 'm' (mb) suffix. For example: 10m = 10 megabytes.
\# 设置log文件最大值
wrapper.logfile.maxsize=5
\# Maximum number of rolled log files which will be allowed before old
\# files are deleted. The default value of 0 implies no limit.
\#设置log文件最多个数
wrapper.logfile.maxfiles=10
\# Log Level for sys\event log output. (See docs for log levels)
wrapper.syslog.loglevel=NONE
\#********************************************************************
\# Wrapper Windows Properties
\#********************************************************************
\# Title to use when running as a console
\# windows下tomcat控制台名称
wrapper.console.title=Tomcat6 Application Server
\#********************************************************************
\# Wrapper Windows NT\2000\XP Service Properties
\#********************************************************************
\# WARNING - Do not modify any of these properties when an application
\# using this configuration file has been installed as a service.
\# Please uninstall the service before modifying this section. The
\# service can then be reinstalled.
\# Name of the service
\# 设置服务名称
wrapper.ntservice.name=tomcat6
\# Display name of the service
wrapper.ntservice.displayname=@app.long.name@
\# Description of the service
wrapper.ntservice.description=@app.description@
\# Service dependencies. Add dependencies as needed starting from 1
wrapper.ntservice.dependency.1=
\# Mode in which the service is installed. AUTO_START or DEMAND_START
\# 设置允许Tomcat服务自动启动
wrapper.ntservice.starttype=AUTO_START
\# Allow the service to interact with the desktop.
wrapper.ntservice.interactive=false
\**6\** 、设置tomcat开机自动运行：
ln -s \opt\tomcat\bin\tomcat \etc\init.d\tomcat
\**7\** 、测试,执行命令：service tomcat start|stop|restart|status
至此，使用Java Service Wrapper来设置Tomcat作为Linux的服务完成，从此过程看来，Java Service Wrapper对tomcat的控制程度比tomcat自带的JSVC深入多了。
目录结构：
\opt\tomcat\bin
                 | tomcat
                 | wrapper
\opt\tomcat\logs
                 | wrapper.log（程序运行时自动产生）
\opt\tomcat\conf
                 | wrapper.conf
\opt\tomcat\lib
                 | libwrapper.so
                 | wrapper.jar
                 | test.jar

