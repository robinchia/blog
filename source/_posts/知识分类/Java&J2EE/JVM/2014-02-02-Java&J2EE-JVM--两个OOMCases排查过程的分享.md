---
layout: post
title: "两个OOM Cases排查过程的分享"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# 两个OOM Cases排查过程的分享

# [BlueDavy之技术blog](https://blog.bluedavy.com/)

{互联网，OSGi，Java, High Scalability, High Performance,HA}

* [Home](https://blog.bluedavy.com/)
* [About](https://blog.bluedavy.com/?page_id=2)
* [Photos](https://blog.bluedavy.com/?page_id=81)

## [两个OOM Cases排查过程的分享](https://blog.bluedavy.com/?p=205 "两个OOM Cases排查过程的分享")

Jan 13

[bluedavy](http://bluedavy.com/ "Visit bluedavy’s website")[jvm](https://blog.bluedavy.com/?cat=13 "View all posts in jvm") [5 Comments]( "Comment on 两个OOM Cases排查过程的分享")
分享一下两个OOM Cases的查找过程，一个应用是Native OOM；另外一个应用其实没有OOM，只是每隔一段时间就会出现频繁FGC的现象，OOM的查找已经具备了不错的工具，但有些时候还是会出现很难查的现象，希望这两个排查过程的分享能给需要的同学带来一些帮助。

**Native OOM的排查Case**
之前的几个PPT里我都说到了，目前查找Native OOM最好的方法就是用google perftools了，于是挂上google perftools，等待应用再次native oom，很幸运，两天后，应用就再次native oom了，于是分析crash之前那段时间谁在不断的分配堆外的内存，pprof看到的结果主要是java.util.Inflater造成的，由于之前已经碰到过类似的case，知道如果使用了Inflater，但不显式的调用Inflater.end的话，确实会造成这个现象。
于是剩下的问题就是找出代码里什么地方调用了Inflater，这种时候btrace这个神器就可以发挥作用了，一个简单的btrace脚本：
[?]()1

2
3

4
5

6
7

8
9

10
11

12
13import

static

com.sun.btrace.BTraceUtils.*;

import

com.sun.btrace.annotations.*;
 

@BTrace

public

class

Trace{
   

@OnMethod

(

      

clazz=

"java.util.zip.Inflater"

,
      

method=

"/.*/"

   

)
   

public

static

void

traceExecute(

@ProbeMethodName

String methodName){

     

println(concat(

"who call Inflater."

,methodName));
     

jstack();

   

}
}

执行后很快就找到了代码什么地方调用了Inflater，于是加上了显式的调用Inflater.end，搞定收工。

**偶尔频繁FGC的排查Case**
这个Case就没上面的那个那么顺利了，查了有接近一个月才查出最终的原因。
当这个应用出现频繁FGC时，dump了内存，用MAT分析后，看到内存被消耗完的原因是由于几个线程内的ThreadLocalMap中有大量的数据，ThreadLocal中消耗最多内存的主要是一个HashMap，这里面有大量的数据。
于是当时想到的第一个方法就是查查应用里面什么地方往ThreadLocal里放了HashMap，杯具的是，当查找代码后发现应用本身的代码并没有往ThreadLocal里放HashMap，那就只能是应用依赖的其他jar包做了这样的事了，但不可能去抓出这个应用依赖的所有的jar的源码来扫描，于是继续借助BTrace，写了个脚本来跟踪这类型的线程中谁调用了ThreadLocal.set，并且放的是HashMap，btrace脚本如下：
[?]()1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
import

static

com.sun.btrace.BTraceUtils.*;

import

com.sun.btrace.annotations.*;
 

@BTrace

public

class

Trace{
   

@OnMethod

(

      

clazz=

"java.lang.ThreadLocal"

,
      

method=

"set"

   

)
   

public

static

void

traceExecute(Object value){

      

if

(startsWith(name(currentThread()),

"xxx"

) && startsWith(

"java.util.HashMap"

,name(classOf(value))) ){
           

println(

"-------------------------"

);

           

jstack();
           

println();

      

}
   

}

}

OK，开始运行上面的脚本，发现竟然一直都没打印出什么内容，只能一直等了，杯具的是一直到了一周后再次出现频繁FGC时，这个脚本都没输出任何的东西，于是只好转换思路。

既然是HashMap里put了大量的某种类型的数据，那干脆用btrace来看看是谁在往HashMap里put这些数据，于是又写了一个btrace脚本，执行后，很快就看到了是代码中什么地方在put这些数据，但是从抓到的调用者来看，不仅仅是目前有大量数据的这类型的线程会调，其他类型的线程也会调用，如果这个地方有问题的话，应该就全部有问题了，于是跳过这里。

回到MAT看到的现象，会不会是因为代码什么地方用ThreadLocal的方式不对，又或是什么地方往ThreadLocal里放了东西，又忘了清除呢，因此要做的就是找出这个应用中所有属性为ThreadLocal的地方，来人肉分析了，于是写了一个jsp，扫描所有的classloader中的所有class，找出属性类型为ThreadLocal的，扫描后找到了一些，还真发现有一个和现在HashMap中放的数据一样的private ThreadLocal，这种用法在线程复用的情况下，如果是每次new ThreadLocal的话，会导致ThreadLocal放的东西一直不释放，兴奋的以为已经发现原因了，可惜和业务方一确认，这个类借助Spring保证了singleton的，因此不会有问题。
好吧，到这一步，只能猜想是由于某种参数请求的时候造成业务上会获得大量的数据了，于是想着要找业务方来分析代码了，这个非常麻烦，于是到此就几乎停滞不前了。

今天静下心来，重新仔细的看了下MAT分析的结果，决定仍然用btrace跟踪下之前往HashMap中put数据的那个业务代码，突然发现，在web类型的处理线程中它借助的是filter去clear数据的，而杯具的是出问题的这种类型线程的处理机制是没有filter机制的，因此猜测问题估计出在这里了，继续btrace，看看这种类型的线程中是不是只有人调put，没人调clear，btrace脚本运行，很快就验证了这个猜测，于是相应的解决掉了这个case，搞定收工。

在这第二个case中，可见在频繁FGC或者OOM时，很有可能MAT只能告诉你初步的原因，但要对应到代码上到底是什么地方造成的，还得花很大精力分析了，这个时候BTrace通常能帮上很大的忙。

[Sun JDK 1.6内存管理](https://blog.bluedavy.com/?p=200) [服务框架演变过程](https://blog.bluedavy.com/?p=210)

### 5 Comments *([+add yours?]())*

1. 
![](https://secure.gravatar.com/avatar/8a0a3550a77608414631e27fbda32c48?s=48&d=<path_to_url>&r=G) Jimmy
**Jan 18, 2011** @ 15:40:15
google perftools这个怎么用呢？能不能讲讲？
1. 
![](https://secure.gravatar.com/avatar/5267242305332fe1affd9c4788191e1d?s=48&d=<path_to_url>&r=G) [bluedavy](http://bluedavy.com/)
**Jan 18, 2011** @ 21:04:09
perftools装上后，只用设置上export LD_PRELOAD=[libtcmalloc所在的目录]/libtcmalloc.so，然后env HEAPPROFILE=dump的内存信息的文件名 java …，启动后就会每隔一段时间自动生成一些dump文件，最后通过perftools里面的pprof对dump文件进行分析就可以看到结果了。
1. 
![](https://secure.gravatar.com/avatar/ee65a80dc2effc685329d0630603a118?s=48&d=<path_to_url>&r=G) [sand](http://www.sandzhang.com/)
**May 21, 2011** @ 23:05:26
要是能有人多分享一下这种实际案例就好了
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

### 推荐书籍
### My Book

[![]()](http://book.douban.com/subject/4848587/ "分布式Java应用：基础与实践") [![]()](http://book.douban.com/subject/3843896/ "OSGi原理与最佳实践")
© [BlueDavy之技术blog](https://blog.bluedavy.com/) 2013

[Icons](http://icondock.com/) & [Wordpress Theme](http://www.ndesign-studio.com/wp-themes) by [N.Design](http://www.ndesign-studio.com/)
