---
layout: post
title: "一次Java内存溢出异常的分析过程"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 内存分析
--- 

# 一次Java内存溢出异常的分析过程

前些天，[服务器](http://server.chinabyte.com/)上一个服务跑了一个多月突然当掉了。看了下日志，程序抛出了java.lang.OutOfMemoryError，之前也出现过同样的错误，服务跑了三个月内存溢出。出现这个异常，初步判断是程序有内存泄漏，接下来需要利用一些工具来分析具体原因。

首先使用jdk自带的工具jmap转储(dump)java内存堆数据到本地文件中。jmap转储(dump)命令格式如下：

jmap -dump:

表示dump选项，表示需要dump的java应用程序的进程ID

dump-options可以有以下三种参数，参数之间用逗号隔开：

live，只dump活动的object，如果没有指定，表示dump所有object

format=b，字节流格式

file=，是转储文件的保存路径

下面是dump命令的一个例子:

jmap -dump:format=b,file=heap.bin 8023

dump完成后，用Eclipse Memory Analyzer打开转储文件进行分析。Eclipse Memory Analyzer是一个java内存堆转储文件分析工具，可以生成内存分析报告(包括内存泄露Leak Suspects )。下面是分析完成后查看Top Consumers所看到的图表:

![]()

![]()

看到这两张图表，问题就比较明朗了。berkeley db中的数据结点com.sleepycat.je.tree.BIN占用了大量内存，导致内存溢出了。为了提高访问效率，berkeley db会缓存大量的结点，缓存大小限制可以在EnvironmentConfig设置，默认为jvm内存大小限制的60%。而这个应用程序中使用了5个EnvironmentImpl，并且都未单独设置缓存大小，总的缓存限制为jvm内存限制的300%(图表中BIN结点已经占用了94.55%的内存)，远远超出java的内存限制。随着服务的运行，缓存数据越来越多，最后导致内存溢出错误。因此，为每个EnvironmentImpl设置合适的缓存大小限制，就可以避免再次发生内存溢出。
来源： <[http://soft.chinabyte.com/database/475/12147475.shtml](http://soft.chinabyte.com/database/475/12147475.shtml)> 
