---
layout: post
title: "JVM调优总结（一）-- 一些概念 - 高级语言虚拟机"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# JVM调优总结（一）-- 一些概念 - 高级语言虚拟机

[您还未登录 !](http://hllvm.group.iteye.com/login "登录") [登录](http://hllvm.group.iteye.com/login) [注册](http://hllvm.group.iteye.com/signup)

[![ITeye3.0]( "ITeye-最棒的软件开发交流社区")](http://www.iteye.com/)

[]()

[群组首页](http://www.iteye.com/groups) → [编程语言](http://hllvm.group.iteye.com/groups/category/language) → [高级语言虚拟机](http://hllvm.group.iteye.com/) → [知识库](http://hllvm.group.iteye.com/group/wiki) → [JVM调优](http://hllvm.group.iteye.com/group/wiki?category_id=301) → [JVM调优总结（一）-- 一些概念]()
原创作者: [和你在一起](http://www.javaeye.com/topic/518623)   阅读:8621次   评论:14条   更新时间:2011-05-26    

 

## **数据类型**

    Java虚拟机中，数据类型可以分为两类：**基本类型**和**引用类型**。基本类型的变量保存原始值，即：他代表的值就是数值本身；而引用类型的变量保存引用值。“引用值”代表了某个对象的引用，而不是对象本身，对象本身存放在这个引用值所表示的地址的位置。

基本类型包括：byte,short,int,long,char,float,double,Boolean,returnAddress

引用类型包括：**类类型**，**接口类型**和**数组**。

## **堆与栈**

    堆和栈是程序运行的关键，很有必要把他们的关系说清楚。

 

   ![]()

###     **栈是运行时的单位，而堆是存储的单位**。

    栈解决程序的运行问题，即程序如何执行，或者说如何处理数据；堆解决的是数据存储的问题，即数据怎么放、放在哪儿。

    在Java中一个线程就会相应有一个线程栈与之对应，这点很容易理解，因为不同的线程执行逻辑有所不同，因此需要一个独立的线程栈。而堆则是所有线程共享的。栈因为是运行单位，因此里面存储的信息都是跟当前线程（或程序）相关信息的。包括局部变量、程序运行状态、方法返回值等等；而堆只负责存储对象信息。

### **    为什么要把堆和栈区分出来呢？栈中不是也可以存储数据吗**？

    第一，从软件设计的角度看，栈代表了处理逻辑，而堆代表了数据。这样分开，使得处理逻辑更为清晰。分而治之的思想。这种隔离、模块化的思想在软件设计的方方面面都有体现。

    第二，堆与栈的分离，使得堆中的内容可以被多个栈共享（也可以理解为多个线程访问同一个对象）。这种共享的收益是很多的。一方面这种共享提供了一种有效的数据交互方式(如：共享内存)，另一方面，堆中的共享常量和缓存可以被所有栈访问，节省了空间。

    第三，栈因为运行时的需要，比如保存系统运行的上下文，需要进行地址段的划分。由于栈只能向上增长，因此就会限制住栈存储内容的能力。而堆不同，堆中的对象是可以根据需要动态增长的，因此栈和堆的拆分，使得动态增长成为可能，相应栈中只需记录堆中的一个地址即可。

    第四，面向对象就是堆和栈的完美结合。其实，面向对象方式的程序与以前结构化的程序在执行上没有任何区别。但是，面向对象的引入，使得对待问题的思考方式发生了改变，而更接近于自然方式的思考。当我们把对象拆开，你会发现，对象的属性其实就是数据，存放在堆中；而对象的行为（方法），就是运行逻辑，放在栈中。我们在编写对象的时候，其实即编写了数据结构，也编写的处理数据的逻辑。不得不承认，面向对象的设计，确实很美。

###     **在Java中，Main函数就是栈的起始点，也是程序的起始点**。

    程序要运行总是有一个起点的。同C语言一样，java中的Main就是那个起点。无论什么java程序，找到main就找到了程序执行的入口：）

###     **堆中存什么？栈中存什么**？

    堆中存的是对象。栈中存的是基本数据类型和堆中对象的引用。一个对象的大小是不可估计的，或者说是可以动态变化的，但是在栈中，一个对象只对应了一个4btye的引用（堆栈分离的好处：））。

    为什么不把基本类型放堆中呢？因为其占用的空间一般是1~8个字节——需要空间比较少，而且因为是基本类型，所以不会出现动态增长的情况——长度固定，因此栈中存储就够了，如果把他存在堆中是没有什么意义的（还会浪费空间，后面说明）。可以这么说，基本类型和对象的引用都是存放在栈中，而且都是几个字节的一个数，因此在程序运行时，他们的处理方式是统一的。但是基本类型、对象引用和对象本身就有所区别了，因为一个是栈中的数据一个是堆中的数据。最常见的一个问题就是，Java中参数传递时的问题。

###     **Java中的参数传递时传值呢？还是传引用**？

    要说明这个问题，先要明确两点：

         1. **不要试图与C进行类比，Java中没有指针的概念**

         2. **程序运行永远都是在栈中进行的，因而参数传递时，只存在传递基本类型和对象引用的问题**。不会直接传对象本身。

    明确以上两点后。Java在方法调用传递参数时，因为没有指针，所以**它都是进行传值调用**（这点可以参考C的传值调用）。因此，很多书里面都说Java是进行传值调用，这点没有问题，而且也简化的C中复杂性。

    *但是传引用的错觉是如何造成的呢？*在运行栈中，基本类型和引用的处理是一样的，都是传值，所以，如果是传引用的方法调用，也同时可以理解为“传引用值”的传值调用，即引用的处理跟基本类型是完全一样的。但是当进入被调用方法时，被传递的这个引用的值，被程序解释（或者查找）到堆中的对象，这个时候才对应到真正的对象。如果此时进行修改，修改的是引用对应的对象，而不是引用本身，即：修改的是堆中的数据。所以这个修改是可以保持的了。

    对象，从某种意义上说，是由基本类型组成的。可以把一个对象看作为一棵树，对象的属性如果还是对象，则还是一颗树（即非叶子节点），基本类型则为树的叶子节点。程序参数传递时，被传递的值本身都是不能进行修改的，但是，如果这个值是一个非叶子节点（即一个对象引用），则可以修改这个节点下面的所有内容。

 

    堆和栈中，栈是程序运行最根本的东西。程序运行可以没有堆，但是不能没有栈。而堆是为栈进行数据存储服务，说白了堆就是一块共享的内存。不过，正是因为堆和栈的分离的思想，才使得Java的垃圾回收成为可能。

     Java中，栈的大小通过-Xss来设置，当栈中存储数据比较多时，需要适当调大这个值，否则会出现java.lang.StackOverflowError异常。常见的出现这个异常的是无法返回的递归，因为此时栈中保存的信息都是方法返回的记录点。
[JVM调优总结（二）-一些概念](http://hllvm.group.iteye.com/group/wiki/2860-JVM "JVM调优总结（二）-一些概念")

评论 共 14 条 请[登录](http://hllvm.group.iteye.com/login)后发表评论 []()

### 14 楼 [yangpanwww](http://yangpanwww.iteye.com/ "yangpanwww") 2013-05-24 10:44

![]() ![]() ![]()   受益匪浅
### 13 楼 [wangpeihu](http://wangpeihu.iteye.com/ "wangpeihu") 2013-03-22 10:48

楼主分析的很透彻，很到位。
在这里我有2个问题想请教楼主：
引用

栈因为是运行单位，因此里面存储的信息都是跟当前线程（或程序）相关信息的。包括局部变量、程序运行状态、方法返回值等等
能否将这些局部变量、程序运行状态、方法返回值等放在堆里，而堆里的对象放在栈里呢？说白了，就是他俩的工作职责对调一下可以吗？
我经常看到人说，栈的运行效率较高，堆的效率较低（相对于栈而言），是否是因为这个原因，而促成了堆栈现在的功能分配呢？

### 12 楼 [chancong](http://chancong.iteye.com/ "chancong") 2013-03-21 17:42

学习了，受益匪浅
### 11 楼 [xwl1991](http://xwl1990.iteye.com/ "xwl1991") 2013-02-18 10:05

受益匪浅 非常感谢！![]()

### 10 楼 [xiaodatao](http://xiaodatao.iteye.com/ "xiaodatao") 2012-01-19 16:33

    程序运行永远都是在栈中进行的，因而参数传递时，只存在传递基本类型和对象引用的问题。不会直接传对象本身。
栈解决程序的运行问题，即程序如何执行，或者说如何处理数据；堆解决的是数据存储的问题，即数据怎么放、放在哪儿。
在Java中一个线程就会相应有一个线程栈与之对应，因为不同的线程执行逻辑有所不同，因此需要一个独立的线程栈。而堆则是所有线程共享的。栈因为是运行单位，因此里面存储的信息都是跟当前线程（或程序）相关信息的。包括局部变量、程序运行状态、方法返回值等等；而堆只负责存储对象信息。
堆和栈中，栈是程序运行最根本的东西。程序运行可以没有堆，但是不能没有栈。而堆是为栈进行数据存储服务，说白了堆就是一块共享的内存。不过，正是因为堆和栈的分离的思想，才使得Java的垃圾回收成为可能。![]() ![]()
### 9 楼 [diecui1202](http://diecui1202.iteye.com/ "diecui1202") 2012-01-05 14:16

楼主Boolean是不是写错了？是boolean吧！

### 8 楼 [bsr1983](http://bsr1983.iteye.com/ "bsr1983") 2011-12-16 22:54

写的很不错，赞一个！![]()
### 7 楼 [lknh](http://chenzone.iteye.com/ "lknh") 2011-09-22 23:46

学习了，写得很好，很详细、易懂！

### 6 楼 [shaomeng95](http://shaomeng95.iteye.com/ "shaomeng95") 2011-07-08 10:37

写的很好，通俗易懂！
### 5 楼 [smalltalker](http://smalltalker.iteye.com/ "smalltalker") 2011-05-30 13:47

非常不错。文章讲的很透彻，和容易让人理解，看了之后有种豁然开朗的感觉。![]()

### 4 楼 [ivin](http://ivin.iteye.com/ "ivin") 2011-03-15 11:56

不错啊，非常透彻，感谢感谢！
### 3 楼 [suxianchun](http://suxianchun.iteye.com/ "suxianchun") 2011-01-13 17:25

gauge

### 2 楼 [suxianchun](http://suxianchun.iteye.com/ "suxianchun") 2011-01-13 17:24

![]()
### 1 楼 [suxianchun](http://suxianchun.iteye.com/ "suxianchun") 2011-01-13 17:24

![]()
### 发表评论

[![]() 您还没有登录,请您登录后再发表评论](http://hllvm.group.iteye.com/login)

[![New-page]()](http://hllvm.group.iteye.com/group/wiki/new)

### 文章信息

[知识库: 高级语言虚拟机](http://hllvm.group.iteye.com/group/wiki/)

* 由[fantasy](http://kiral.iteye.com/ "fantasy")在2010-11-08创建
* 由[fantasy](http://kiral.iteye.com/ "fantasy")在2011-05-26更新
### 相关新闻

* [ruby内存泄漏的罪魁祸首 - 幽灵指针](http://hllvm.group.iteye.com/news/4407)
* [Azul Systems将要开源Managed Runtime Initiative中的重要技术](http://hllvm.group.iteye.com/news/16601)
* [离开Java，寻找更佳语言的10大理由](http://hllvm.group.iteye.com/news/8909)

### 相关讨论

* [Java中堆内存与栈内存分配浅析](http://hllvm.group.iteye.com/topic/941682)
* [Java堆.栈和常量池 笔记](http://hllvm.group.iteye.com/topic/634530)
* [慢慢琢磨JVM——恭喜JavaEye重新开张](http://hllvm.group.iteye.com/topic/821872)
* [Java堆、栈和常量池详解](http://hllvm.group.iteye.com/topic/1097896)
* [一个绝对害了不少人的Java技术问题！](http://hllvm.group.iteye.com/topic/4189)
### 相关博客

* [JVM基础：深入学习JVM堆与JVM栈](http://hy90171.iteye.com/blog/1879242)
* [JVM基础概念总结：数据类型、堆与栈](http://tangchenglin.iteye.com/blog/701681)
* [JVM调优总结（一）一些概念](http://jiaozhiguang-126-com.iteye.com/blog/1701015)
* [JVM调优总结（一）-- 一些概念](http://pengjiaheng.iteye.com/blog/518623)
* [JVM 概念总结 数据类型、堆与栈](http://bobostudio.iteye.com/blog/602519)
* [首页](http://www.iteye.com/)
* [资讯](http://www.iteye.com/news)
* [精华](http://www.iteye.com/magazines)
* [论坛](http://www.iteye.com/forums)
* [问答](http://www.iteye.com/ask)
* [博客](http://www.iteye.com/blogs)
* [专栏](http://www.iteye.com/blogs/subjects)
* [群组](http://www.iteye.com/groups)
* [招聘](http://job.iteye.com/iteye)
* [搜索](http://www.iteye.com/search)
* [广告服务](http://hllvm.group.iteye.com/index/service)
* [ITeye黑板报](http://webmaster.iteye.com/)
* [联系我们](http://hllvm.group.iteye.com/index/contactus)
* [友情链接](http://hllvm.group.iteye.com/index/friend_links)

© 2003-2012 ITeye.com. [ [京ICP证110151号](http://www.miibeian.gov.cn/) 京公网安备110105010620 ]
百联优力(北京)投资有限公司 版权所有 ![](http://stat.iteye.com/?url=http%3A%2F%2Fhllvm.group.iteye.com%2Fgroup%2Fwiki%2F2858-JVM&referrer=&user_id=)
