---
layout: post
title: "JRockit读书笔记I — Java代码的高效执行"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# JRockit读书笔记I — Java代码的高效执行

# [BlueDavy之技术blog](https://blog.bluedavy.com/)

{互联网，OSGi，Java, High Scalability, High Performance,HA}

* [Home](https://blog.bluedavy.com/)
* [About](https://blog.bluedavy.com/?page_id=2)
* [Photos](https://blog.bluedavy.com/?page_id=81)

## [JRockit读书笔记I — Java代码的高效执行](https://blog.bluedavy.com/?p=198 "JRockit读书笔记I — Java代码的高效执行")

Dec 16

[bluedavy](http://bluedavy.com/ "Visit bluedavy’s website")[jvm](https://blog.bluedavy.com/?cat=13 "View all posts in jvm") [java code generation](https://blog.bluedavy.com/?tag=java-code-generation), [java代码执行](https://blog.bluedavy.com/?tag=java%e4%bb%a3%e7%a0%81%e6%89%a7%e8%a1%8c), [jit](https://blog.bluedavy.com/?tag=jit), [jvm](https://blog.bluedavy.com/?tag=jvm) [10 Comments]( "Comment on JRockit读书笔记I — Java代码的高效执行")
《Oracle JRockit: The Definitive Guide》一书是由Oracle JRockit的两位资深开发人员写的，其中的Marcus Hirt更是JRockit Mission Control的leader，这本书详细的对Oracle JRockit进行了介绍，最突出的特点非常系统化的介绍了一个JVM通常是如何实现的，而JRockit这样一个极为优秀的JVM又是做了哪些优化，为什么做这些优化，这本书对于对JVM感兴趣的同学而言应该是必读的一本书，其实即使对于JVM兴趣不强的同学，里面的优化思路的介绍也是值得学习，本系列的blog主要是总结看这本书得到的一些收获，由于书中知识量巨大，因此得分成多篇blog来总结了。

书的第二章为：Adaptive Code Generation，在这章中作者向我们讲解了一个优秀的JVM是如何来实现代码的高效执行的，感兴趣的同学其实可以在不看下面blog内容之前，先考虑下如果是你做的话，你会怎么做来实现Java代码的高效执行呢，然后再对比下这章的内容，我想你能学到很多的，:)

用过Java的同学都知道，Java是通过javac将Java源码编译为class文件，然后通过ClassLoader装载此class文件，之后就可执行此class了，要最高效的执行这个class，最好的方法莫过于class文件直接就是机器码，这样直接执行就可以了，但Java是跨平台的，因此class文件就不能是机器码了。

由于class文件不是直接的机器码，要执行它最简单的方法就是采用纯粹的解释方式，解释方式由于每次都得将class文件中的指令翻译为对应的机器环境的指令，效率是很低的。

为了能更高效的执行，同时又保持跨平台的特性，另外一个方法就是在执行class时再将其翻译为对应的机器码，这个方法是比较靠谱的，因此无论是Hotspot、还是JRockit，都采用了这种方式，也就是大家熟知的JIT(Just In Time) Compiler。

OK，既然觉得在装载class后翻译成机器码去执行可以比较高效，那这个时候又会出现两种状况，是执行class的时候就立刻翻译成机器码，还是先用解释模式执行，然后到一定时机再翻译成机器码呢，之所以出现这两种状况，原因在于将class翻译为机器码是需要消耗时间的，因此如果执行class的时候就立刻翻译成机器码的话，也就会导致Java程序启动速度会比较慢，JRockit是这么认为的，JRockit的服务对象是server级应用，这类应用的特点是没那么在乎启动速度，而更在乎的是执行时的高效，而且如果执行的时候就立刻翻译成机器码的话，就意味着压根不需要实现解释器，因此JRockit采取的方法是在执行class时直接编译为机器码，而Hotspot由于需要同时支持client和server应用，对于client应用而言，启动速度非常重要，因此Hotspot采用的是先解释执行，到了一定时机后再翻译成机器码。

如果认为就这样就完成了Java代码的执行的实现，那就太小看JVM了，由于JVM能够知道代码运行的全部状况，自然还可以做出更多更出色的提升代码执行速度的优化，例如标量替换、更好的inline等，后面再来细说，因此这样就出现了一个状况，什么时候对哪些代码来做这些更猛的优化呢。

真正值得做更猛的优化的代码自然是所谓的”热点”代码，如何来发现哪些代码是热点代码呢，通常有三种方法：
1、方法调用计数器
方法调用计数器是常见的方式，hotspot采用的即为这种，这种方式不好的地方就在于计数器本身经常是cpu cache misses的，因此稍微会有点影响性能。
2、对线程进行采样
可采用软件或硬件方式来实现，软件方式实现不好的地方在于采样的时候需要暂停线程，好处是因为是采样，不需要对所有方法进行计数，硬件方式自然是最好的，但不是所有的硬件都支持的，支持的硬件中最典型的是intel IA-64的CPU。

在有了发现热点代码的方法后，接下来需要做的就是更猛的优化，有很多种，例如Java的代码中，通常会是接口方式的调用，但因为是接口方式的调用，所以其实默认情况下是不好做inline处理的，但JVM为了更高效的执行代码，如发现这代码为热点代码，那么就会做一些激进的优化，例如会假设这个接口只有一个实现，然后就可以直接将此实现对应的代码inline进来了（至于为什么inline后效率更高，这个请参考编译原理之类的书），这些激进优化同样适合于if、抛异常这些状况，当然，当激进优化的条件失效时，就会逆优化回到之前基本编译的代码。
而其他的更猛的优化还包括根据线程执行路径进行逃逸分析等，后面再专门写一篇blog来讲解下一些翻译为机器码的优化吧，其实大多都是编译原理的一些东西。

书中在介绍JRockit如何实现自己的JIT Compiler时，提到了Bytecode混淆以及bytecode优化，JRockit的态度是bytecode混淆时将name进行混淆是靠谱的，但如果对control flow进行混淆，就不太好了，因为这有可能会导致jit compile时的有些优化也做不了了，而bytecode优化，JRockit的态度是应该避免，因为没什么太大的意义，更主要的优化还是得靠jit compiler。

JRockit的JIT Compiler的实现和Hotspot另外一个很大的不同在于JRockit并未采用on-stack replacement，据JRockit的研究，这个没有太大必要，当然，对于编写benchmark代码时则要注意这个不同。

JIT Compiler在compile时还需要考虑的几个重点问题：
1、为GC提供必要的信息；
2、为查错提供必要的信息，例如代码的行数、变量名等；

从这章的内容可以看到，JRockit为了能够让Java代码能够高效的执行，是做出了非常多的努力的，也可以看到很多JRockit与Hotspot不同的地方，甚至可以看出Java代码的执行比C代码的执行高效都是有可能的，:)。

[学习JVM的References](https://blog.bluedavy.com/?p=187) [Sun JDK 1.6内存管理](https://blog.bluedavy.com/?p=200)

### 10 Comments *([+add yours?]())*

1. 
![](https://secure.gravatar.com/avatar/67ffe1e5c1e1114f9cd68a1d6770d528?s=48&d=<path_to_url>&r=G) h
**Dec 16, 2010** @ 18:57:35
我还没有读完……啊……
1. 
![](https://secure.gravatar.com/avatar/ba40b479e7eca4475a6a9c9d992d89b2?s=48&d=<path_to_url>&r=G) cauherk
**Dec 16, 2010** @ 20:44:19
JRockit好像不是在执行的时候进行jit 编译操作的吧。
打开jrmc的flight record，可以详细的看到jit编译的情况，其中并没有列出所有执行过的代码。
1. 
![](https://secure.gravatar.com/avatar/2c48e9c1957bc1e99f7508ab1aca3bcb?s=48&d=<path_to_url>&r=G) jianying
**Dec 16, 2010** @ 23:09:24
运行时的内联优化的确是可能比C更快的。
1. 
![](https://secure.gravatar.com/avatar/5267242305332fe1affd9c4788191e1d?s=48&d=<path_to_url>&r=G) [bluedavy](http://bluedavy.com/)
**Dec 17, 2010** @ 09:49:57
@cauherk
JRockit是没有解释器的，如果不编译，你觉得它该怎么执行呢，具体可以看看书中的这章。
“When the method is first called, and control jumps to the trampoline, all it does is execute a call that tells JRockit that the real method needs to be generated.”
1. 
![](https://secure.gravatar.com/avatar/5267242305332fe1affd9c4788191e1d?s=48&d=<path_to_url>&r=G) [bluedavy](http://bluedavy.com/)
**Dec 17, 2010** @ 09:51:28
@cauherk
flight recorder多数情况是采样机制和阀值机制，因此没包含所有执行过的代码也是正常的。
1. 
![](https://secure.gravatar.com/avatar/67e4249167a3260501c410623b7e9a83?s=48&d=<path_to_url>&r=G) hanguokai
**Dec 17, 2010** @ 13:46:45
对线程进行采样，采了什么，如何判断热点？
1. 
![](https://secure.gravatar.com/avatar/5267242305332fe1affd9c4788191e1d?s=48&d=<path_to_url>&r=G) [bluedavy](http://bluedavy.com/)
**Dec 17, 2010** @ 21:39:38
@hanguokai
对线程进行采样，看看线程在执行些什么方法，采样多次后，自然也就知道哪里是热点了。
1. 
![](https://secure.gravatar.com/avatar/f1e9ff94f7de8e17b9bc4c60a6b246ee?s=48&d=<path_to_url>&r=G) Wen
**Jan 11, 2011** @ 22:33:45
您好！我是一名在校大学生，看了您写的这篇博文，很感兴趣。我现在正在做一个关于JVM优化的问题，刚刚开始，用C写了一个简单的Bytecode解释器，能够进行类型转换，算术逻辑运算，控制转移等工作，我和我的同学希望能够做出一个面向移动嵌入式平台的小型JVM，现在遇到一些问题，能请您给我们一些建议吗？
我们的问题主要集中在优化的方向选择上，现在大致有两条思路：一个是将bytecode反编译为一种中间语言（比如C），再通过中间语言编译机器码实现其功能，另一个是优化bytecode编译的方式。这是大体思路，我们对JVM的理解还很粗浅，见笑了~
谢谢！
1. 
![](https://secure.gravatar.com/avatar/d9b07be4b9b23099363ffcab6305cf88?s=48&d=<path_to_url>&r=G) RednaxelaFX
**Jan 14, 2011** @ 15:36:25
@Wen
这两种思路都有现成的实现，如果想参考的话资源非常丰富。
例如说，同样是应用在嵌入式领域的小型JVM，Squawk（http://labs.oracle.com/projects/dashboard.php?id=155 [https://squawk.dev.java.net/）就是先将Java字节码编译为C，然后再用C编译器编译为native](https://squawk.dev.java.net/）就是先将Java字节码编译为C，然后再用C编译器编译为native) code，再部署到设备上的。
如果想在运行时直接将字节码编译为native code，那么嵌入式领域可以参考phoneME（https://phoneme.dev.java.net/）开源的CDC HotSpot Implementation或者CLDC HotSpot Implementation。或者像是Google Android里的Dalvik虚拟机，里面也有JIT编译器，并且使用的是最近比较流行的trace-based compiler。

对这些感兴趣的话，也欢迎来JavaEye的高级语言虚拟机圈子来讨论 ^_^
[http://hllvm.group.javaeye.com/](http://hllvm.group.javaeye.com/)
### Leave a Reply

[Cancel]()

Name (required)

Mail (required)

Website

![CAPTCHA Image]( "CAPTCHA Image")

[![Refresh Image]()]( "Refresh Image")

CAPTCHA Code *

<style type='text/css'>#submit {display:none;}</style><br /> <input name="submit" type="submit" id="submit-alt" tabindex="6" value="Submit Comment" />

July 2013 M T W T F S S [« Mar](https://blog.bluedavy.com/?m=201303 "View posts for March 2013")     1234567 891011121314 15161718192021 22232425262728 293031  

### Categories

* [Java](https://blog.bluedavy.com/?cat=63 "View all posts filed under Java") (10)
* [jvm](https://blog.bluedavy.com/?cat=13 "View all posts filed under jvm") (19)
* [NoSQL](https://blog.bluedavy.com/?cat=56 "View all posts filed under NoSQL") (7)
* [SOA](https://blog.bluedavy.com/?cat=4 "View all posts filed under SOA") (1)
* [书:分布式Java应用](https://blog.bluedavy.com/?cat=11 "关于《分布式Java应用：基础与实践》书部分章节的公开、书内容的纠错以及补充完善。") (5)
* [互联网](https://blog.bluedavy.com/?cat=6 "View all posts filed under 互联网") (5)
* [产品总结](https://blog.bluedavy.com/?cat=46 "View all posts filed under 产品总结") (1)
* [优化案例](https://blog.bluedavy.com/?cat=18 "View all posts filed under 优化案例") (1)
* [圆桌交流](https://blog.bluedavy.com/?cat=12 "View all posts filed under 圆桌交流") (2)
* [容量规划](https://blog.bluedavy.com/?cat=8 "View all posts filed under 容量规划") (2)
* [迁户口](https://blog.bluedavy.com/?cat=97 "View all posts filed under 迁户口") (1)
* [高可用](https://blog.bluedavy.com/?cat=52 "View all posts filed under 高可用") (1)
* [高并发](https://blog.bluedavy.com/?cat=3 "View all posts filed under 高并发") (3)
* [高性能](https://blog.bluedavy.com/?cat=1 "关于性能优化方面的一些东西。") (2)
### Recent Comments

* [JVM调优 | code1（code1.riaos.com）](http://code1.riaos.com/?p=5030138) on [说说MaxTenuringThreshold这个参数](https://blog.bluedavy.com/?p=70&cpage=1#comment-16520)
* [JVM调优 | architecture（riaos.com）](http://architecture1.riaos.com/?p=3063358) on [说说MaxTenuringThreshold这个参数](https://blog.bluedavy.com/?p=70&cpage=1#comment-16519)
* [bluedavy](http://bluedavy.com/) on [一个Java应用频繁抛异常导致cpu us诡异现象的案例](https://blog.bluedavy.com/?p=409&cpage=1#comment-16462)
* xiaobo on [一个Java应用频繁抛异常导致cpu us诡异现象的案例](https://blog.bluedavy.com/?p=409&cpage=1#comment-16460)
* [bluedavy](http://bluedavy.com/) on [一个Java应用频繁抛异常导致cpu us诡异现象的案例](https://blog.bluedavy.com/?p=409&cpage=1#comment-16459)

### Tags

[btrace](https://blog.bluedavy.com/?tag=btrace "2 topics") [c1](https://blog.bluedavy.com/?tag=c1 "1 topic") [c2](https://blog.bluedavy.com/?tag=c2 "1 topic") [Deflater](https://blog.bluedavy.com/?tag=deflater "1 topic") [facebook](https://blog.bluedavy.com/?tag=facebook "2 topics") [gc](https://blog.bluedavy.com/?tag=gc "4 topics") [gc tuning](https://blog.bluedavy.com/?tag=gc-tuning "2 topics") [Grizzly](https://blog.bluedavy.com/?tag=grizzly "2 topics") [HBase](https://blog.bluedavy.com/?tag=hbase "6 topics") [hotspot](https://blog.bluedavy.com/?tag=hotspot "1 topic") [Inflater](https://blog.bluedavy.com/?tag=inflater "1 topic") [interpreter](https://blog.bluedavy.com/?tag=interpreter "1 topic") [javac](https://blog.bluedavy.com/?tag=javac "1 topic") [java code generation](https://blog.bluedavy.com/?tag=java-code-generation "1 topic") [JavaOne](https://blog.bluedavy.com/?tag=javaone "4 topics") [javaone general technical session](https://blog.bluedavy.com/?tag=javaone-general-technical-session "1 topic") [java代码执行](https://blog.bluedavy.com/?tag=java%e4%bb%a3%e7%a0%81%e6%89%a7%e8%a1%8c "1 topic") [Java 并发](https://blog.bluedavy.com/?tag=java-%e5%b9%b6%e5%8f%91 "1 topic") [jit](https://blog.bluedavy.com/?tag=jit "1 topic") [jvm](https://blog.bluedavy.com/?tag=jvm "12 topics") [memory management](https://blog.bluedavy.com/?tag=memory-management "1 topic") [Native Memory Leak](https://blog.bluedavy.com/?tag=native-memory-leak "1 topic") [NoSQL](https://blog.bluedavy.com/?tag=nosql "2 topics") [oom](https://blog.bluedavy.com/?tag=oom "1 topic") [oracle keynote](https://blog.bluedavy.com/?tag=oracle-keynote "1 topic") [pessimism policy](https://blog.bluedavy.com/?tag=pessimism-policy "1 topic") [references](https://blog.bluedavy.com/?tag=references "1 topic") [RPC](https://blog.bluedavy.com/?tag=rpc "2 topics") [serial gc](https://blog.bluedavy.com/?tag=serial-gc "1 topic") [SOA](https://blog.bluedavy.com/?tag=soa "2 topics") [sun jdk](https://blog.bluedavy.com/?tag=sun-jdk "1 topic") [sun jdk oom](https://blog.bluedavy.com/?tag=sun-jdk-oom "1 topic") [Web容量规划的艺术](https://blog.bluedavy.com/?tag=web%e5%ae%b9%e9%87%8f%e8%a7%84%e5%88%92%e7%9a%84%e8%89%ba%e6%9c%af "1 topic") [yuanzhuo](https://blog.bluedavy.com/?tag=yuanzhuo "1 topic") [书:分布式Java应用](https://blog.bluedavy.com/?tag=%e4%b9%a6 "1 topic") [书评](https://blog.bluedavy.com/?tag=%e4%b9%a6%e8%af%84 "1 topic") [互联网技术](https://blog.bluedavy.com/?tag=%e4%ba%92%e8%81%94%e7%bd%91%e6%8a%80%e6%9c%af "1 topic") [交流](https://blog.bluedavy.com/?tag=%e4%ba%a4%e6%b5%81 "1 topic") [内存管理](https://blog.bluedavy.com/?tag=%e5%86%85%e5%ad%98%e7%ae%a1%e7%90%86 "1 topic") [分布式Java应用](https://blog.bluedavy.com/?tag=%e5%88%86%e5%b8%83%e5%bc%8fjava%e5%ba%94%e7%94%a8 "2 topics") [圆桌交流](https://blog.bluedavy.com/?tag=%e5%9c%86%e6%a1%8c%e4%ba%a4%e6%b5%81 "1 topic") [容量规划](https://blog.bluedavy.com/?tag=%e5%ae%b9%e9%87%8f%e8%a7%84%e5%88%92 "2 topics") [悲观策略](https://blog.bluedavy.com/?tag=%e6%82%b2%e8%a7%82%e7%ad%96%e7%95%a5 "3 topics") [服务框架](https://blog.bluedavy.com/?tag=%e6%9c%8d%e5%8a%a1%e6%a1%86%e6%9e%b6 "1 topic") [硅谷公司](https://blog.bluedavy.com/?tag=%e7%a1%85%e8%b0%b7%e5%85%ac%e5%8f%b8 "1 topic")
### 订阅

[![feedsky]()](http://feed.feedsky.com/bluedavy)
[![抓虾]()](http://www.zhuaxia.com/add_channel.php?url=http://feed.feedsky.com/bluedavy)
[![google reader]()](http://fusion.google.com/add?feedurl=http://feed.feedsky.com/bluedavy)
[![鲜果]()](http://www.xianguo.com/subscribe.php?url=http://feed.feedsky.com/bluedavy)
[![九点]()](http://9.douban.com/reader/subscribe?url=http://feed.feedsky.com/bluedavy)

### 推荐书籍
### My Book

[![]()](http://book.douban.com/subject/4848587/ "分布式Java应用：基础与实践") [![]()](http://book.douban.com/subject/3843896/ "OSGi原理与最佳实践")
© [BlueDavy之技术blog](https://blog.bluedavy.com/) 2013

[Icons](http://icondock.com/) & [Wordpress Theme](http://www.ndesign-studio.com/wp-themes) by [N.Design](http://www.ndesign-studio.com/)
