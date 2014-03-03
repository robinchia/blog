---
layout: post
title: "Java空格转码"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_常用
--- 

# Java空格转码

字符串中的空格转义
String a = "hello world baby";
请问如何将空格转义成转义字符，结果是a = "hello\u0000world\u0000baby"
------解决方案--------------------
Java code

a = a.replaceAll("\\s","\u0000")
------解决方案--------------------
探讨
Java code
a = a.replaceAll("\\s","\u0000")
------解决方案--------------------
话说java正则中，用\\\\代表\
------解决方案--------------------
a = a.replaceAll("\\s","\u0000")
来源： <[http://www.myexception.cn/java-web/45129.html](http://www.myexception.cn/java-web/45129.html)> 
