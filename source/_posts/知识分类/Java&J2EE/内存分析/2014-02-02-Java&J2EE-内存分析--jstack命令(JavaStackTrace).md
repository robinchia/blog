---
layout: post
title: "jstack命令(Java Stack Trace)"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 内存分析
--- 

# jstack命令(Java Stack Trace)

[JDK内置工具使用](http://blog.csdn.net/fenglibing/archive/2011/05/11/6411999.aspx)

[一、javah命令(C Header and Stub File Generator)](http://blog.csdn.net/fenglibing/archive/2011/05/11/6411924.aspx)

[二、jps命令(Java Virtual Machine Process Status Tool)](http://blog.csdn.net/fenglibing/archive/2011/05/11/6411932.aspx)

[三、jstack命令(Java Stack Trace)](http://blog.csdn.net/fenglibing/archive/2011/05/11/6411940.aspx)

[四、jstat命令(Java Virtual Machine Statistics Monitoring Tool)](http://blog.csdn.net/fenglibing/archive/2011/05/11/6411951.aspx)

[五、jmap命令(Java Memory Map)](http://blog.csdn.net/fenglibing/archive/2011/05/11/6411953.aspx)

[六、jinfo命令(Java Configuration Info)](http://blog.csdn.net/fenglibing/archive/2011/05/11/6411958.aspx)

[七、jconsole命令(Java Monitoring and Management Console)](http://blog.csdn.net/fenglibing/archive/2011/05/11/6411965.aspx)

[八、jvisualvm命令(Java Virtual Machine Monitoring, Troubleshooting, and Profiling Tool)](http://blog.csdn.net/fenglibing/archive/2011/05/11/6411969.aspx)

[九、jhat命令(Java Heap Analyse Tool)](http://blog.csdn.net/fenglibing/archive/2011/05/11/6411971.aspx)

[十、Jdb命令(The Java Debugger)](http://blog.csdn.net/fenglibing/archive/2011/05/11/6411974.aspx)

## []()1、介绍

jstack用于打印出给定的java进程ID或core file或远程调试服务的Java堆栈信息，如果是在64位机器上，需要指定选项"-J-d64"，Windows的jstack使用方式只支持以下的这种方式：

jstack [-l] pid

如果java程序崩溃生成core文件，jstack工具可以用来获得core文件的java stack和native stack的信息，从而可以轻松地知道java程序是如何崩溃和在程序何处发生问题。另外，jstack工具还可以附属到正在运行的java程序中，看到当时运行的java程序的java stack和native stack的信息, 如果现在运行的java程序呈现hung的状态，jstack是非常有用的。

2、命令格式
jstack [ option ] pid
jstack [ option ] executable core
jstack [ option ] [server-id@]remote-hostname-or-IP

## []()3、常用参数说明

1)、options： 

executable Java executable from which the core dump was produced.

(可能是产生core dump的java可执行程序)

core 将被打印信息的core dump文件

remote-hostname-or-IP 远程debug服务的主机名或ip

server-id 唯一id,假如一台主机上多个远程debug服务 

2）、基本参数：

-F当’jstack [-l] pid’没有相应的时候强制打印栈信息

-l长列表. 打印关于锁的附加信息,例如属于java.util.concurrent的ownable synchronizers列表.

-m打印java和native c/c++框架的所有栈信息.

-h | -help打印帮助信息

pid 需要被打印配置信息的java进程id,可以用jps查询.

## []()4、使用示例

## []()![]()

来源： <[http://blog.csdn.net/fenglibing/article/details/6411940](http://blog.csdn.net/fenglibing/article/details/6411940)>
 
