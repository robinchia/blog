---
layout: post
title: "JVM调优总结（二）-一些概念 - 高级语言虚拟机"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# JVM调优总结（二）-一些概念 - 高级语言虚拟机

[您还未登录 !](http://hllvm.group.iteye.com/login "登录") [登录](http://hllvm.group.iteye.com/login) [注册](http://hllvm.group.iteye.com/signup)

[![ITeye3.0]( "ITeye-最棒的软件开发交流社区")](http://www.iteye.com/)

[]()

[群组首页](http://www.iteye.com/groups) → [编程语言](http://hllvm.group.iteye.com/groups/category/language) → [高级语言虚拟机](http://hllvm.group.iteye.com/) → [知识库](http://hllvm.group.iteye.com/group/wiki) → [JVM调优](http://hllvm.group.iteye.com/group/wiki?category_id=301) → [JVM调优总结（二）-一些概念]()
原创作者: [和你在一起](http://www.javaeye.com/topic/519471)   阅读:4764次   评论:6条   更新时间:2011-05-26    

 

## Java对象的大小

    基本数据的类型的大小是固定的，这里就不多说了。对于非基本类型的Java对象，其大小就值得商榷。

    在Java中，**一个空Object对象的大小是8byte**，这个大小只是保存堆中一个没有任何属性的对象的大小。看下面语句：
Object ob = new Object();

    这样在程序中完成了一个Java对象的生命，但是它所占的空间为：**4byte+8byte**。4byte是上面部分所说的Java栈中保存引用的所需要的空间。而那8byte则是Java堆中对象的信息。因为所有的Java非基本类型的对象都需要默认继承Object对象，因此不论什么样的Java对象，其大小都必须是大于8byte。

   有了Object对象的大小，我们就可以计算其他对象的大小了。
Class NewObject {

    int count;

    boolean flag;

    Object ob;

}

    其大小为：空对象大小(8byte)+int大小(4byte)+Boolean大小(1byte)+空Object引用的大小(4byte)=17byte。但是因为Java在对对象内存分配时都是以8的整数倍来分，因此大于17byte的最接近8的整数倍的是24，因此此对象的大小为24byte。

    这里需要注意一下**基本类型的包装类型的大小**。因为这种包装类型已经成为对象了，因此需要把他们作为对象来看待。包装类型的大小至少是12byte（声明一个空Object至少需要的空间），而且12byte没有包含任何有效信息，同时，因为Java对象大小是8的整数倍，因此**一个基本类型包装类的大小至少是16byte**。这个内存占用是很恐怖的，它是使用基本类型的N倍（N>2），有些类型的内存占用更是夸张（随便想下就知道了）。因此，可能的话应尽量少使用包装类。在JDK5.0以后，因为加入了自动类型装换，因此，Java虚拟机会在存储方面进行相应的优化。

## 引用类型

    对象引用类型分为**强引用、软引用、弱引用和虚引用**。

 

**强引用:**就是我们一般声明对象是时虚拟机生成的引用，强引用环境下，垃圾回收时需要严格判断当前对象是否被强引用，如果被强引用，则不会被垃圾回收

 

**软引用:**软引用一般被做为缓存来使用。与强引用的区别是，软引用在垃圾回收时，虚拟机会根据当前系统的剩余内存来决定是否对软引用进行回收。如果剩余内存比较紧张，则虚拟机会回收软引用所引用的空间；如果剩余内存相对富裕，则不会进行回收。换句话说，虚拟机在发生OutOfMemory时，肯定是没有软引用存在的。

 

**弱引用:**弱引用与软引用类似，都是作为缓存来使用。但与软引用不同，弱引用在进行垃圾回收时，是一定会被回收掉的，因此其生命周期只存在于一个垃圾回收周期内。

 

    强引用不用说，我们系统一般在使用时都是用的强引用。而“软引用”和“弱引用”比较少见。他们一般被作为缓存使用，而且一般是在内存大小比较受限的情况下做为缓存。因为如果内存足够大的话，可以直接使用强引用作为缓存即可，同时可控性更高。因而，他们常见的是被使用在桌面应用系统的缓存。
[JVM调优总结（一）-- 一些概念](http://hllvm.group.iteye.com/group/wiki/2858-JVM "JVM调优总结（一）-- 一些概念") | [JVM调优总结（三）-基本垃圾回收算法](http://hllvm.group.iteye.com/group/wiki/2861-JVM "JVM调优总结（三）-基本垃圾回收算法")

评论 共 6 条 请[登录](http://hllvm.group.iteye.com/login)后发表评论 []()

### 6 楼 [xianneng.lin](http://xianneng-lin.iteye.com/ "xianneng.lin") 2012-11-21 15:47

虚引用呢？
### 5 楼 [xiaodatao](http://xiaodatao.iteye.com/ "xiaodatao") 2012-01-19 16:43

基本数据的类型的大小是固定的。
Object ob = new Object();
    这样在程序中完成了一个Java对象的生命，但是它所占的空间为：4byte+8byte。4byte是Java栈中保存实例引用。![]() ![]() 

### 4 楼 [shaomeng95](http://shaomeng95.iteye.com/ "shaomeng95") 2011-07-08 13:13

官方文档上说：This data type represents one bit of information, but its "size" isn't something that's precisely defined.
### 3 楼 [shaomeng95](http://shaomeng95.iteye.com/ "shaomeng95") 2011-07-08 13:10

而且12byte没有包含

kthh0226 写道
这个有问题吧，java没有说明boolean的大小，不一定是1byte的
boolean的大小是依赖VM的

### 2 楼 [kthh0226](http://kthh0226.iteye.com/ "kthh0226") 2011-06-17 17:30

这个有问题吧，java没有说明boolean的大小，不一定是1byte的
### 1 楼 [smalltalker](http://smalltalker.iteye.com/ "smalltalker") 2011-05-30 20:18

![]()
### 发表评论

[![]() 您还没有登录,请您登录后再发表评论](http://hllvm.group.iteye.com/login)

[![New-page]()](http://hllvm.group.iteye.com/group/wiki/new)

### 文章信息

[知识库: 高级语言虚拟机](http://hllvm.group.iteye.com/group/wiki/)

* 由[fantasy](http://kiral.iteye.com/ "fantasy")在2010-11-08创建
* 由[fantasy](http://kiral.iteye.com/ "fantasy")在2011-05-26更新
### 相关新闻

* [Leak Finder：谷歌的JavaScript内存泄露检测工具](http://hllvm.group.iteye.com/news/25761)
* [Azul Systems将要开源Managed Runtime Initiative中的重要技术](http://hllvm.group.iteye.com/news/16601)
* [struts2新特性预览](http://hllvm.group.iteye.com/news/4)

### 相关讨论

* [封装ConcurrentHashMap成为具有各种引用类型key与value的ConcurrentReferenceMap，完美取代WeakHashMap](http://hllvm.group.iteye.com/topic/671298)
* [关于java 垃圾回收的理解](http://hllvm.group.iteye.com/topic/1030509)
* [闲来无事，用Java的软引用写了一个山寨的缓存](http://hllvm.group.iteye.com/topic/1039464)
* [理解 Java 的 GC 与 幽灵引用](http://hllvm.group.iteye.com/topic/401478)
* [关于ThreadLocal的内存泄露](http://hllvm.group.iteye.com/topic/704710)
### 相关博客

* [JVM概念中的Java对象的大小，以及三种引用类型的定义与区分](http://hzbaihu.iteye.com/blog/961293)
* [JVM调优总结（二）-一些概念](http://jiaozhiguang-126-com.iteye.com/blog/1701027)
* [JVM调优总结（二）-一些概念](http://pengjiaheng.iteye.com/blog/519471)
* [[转] JVM调优总结（二）-一些概念](http://qjbtj999.iteye.com/blog/660134)
* [JVM调优总结（二）-一些概念](http://millerhu.iteye.com/blog/890685)
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
百联优力(北京)投资有限公司 版权所有 ![](http://stat.iteye.com/?url=http%3A%2F%2Fhllvm.group.iteye.com%2Fgroup%2Fwiki%2F2860-JVM&referrer=http%3A%2F%2Fhllvm.group.iteye.com%2Fgroup%2Fwiki%2F%3Fcategory_id%3D301&user_id=)
