---
layout: post
title: "GC悲观策略之Parallel GC篇"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# GC悲观策略之Parallel GC篇

# [BlueDavy之技术blog](https://blog.bluedavy.com/)

{互联网，OSGi，Java, High Scalability, High Performance,HA}

* [Home](https://blog.bluedavy.com/)
* [About](https://blog.bluedavy.com/?page_id=2)
* [Photos](https://blog.bluedavy.com/?page_id=81)

## [GC悲观策略之Parallel GC篇](https://blog.bluedavy.com/?p=166 "GC悲观策略之Parallel GC篇")

Nov 07

[bluedavy](http://bluedavy.com/ "Visit bluedavy’s website")[jvm](https://blog.bluedavy.com/?cat=13 "View all posts in jvm") [gc](https://blog.bluedavy.com/?tag=gc), [jvm](https://blog.bluedavy.com/?tag=jvm), [pessimism policy](https://blog.bluedavy.com/?tag=pessimism-policy), [悲观策略](https://blog.bluedavy.com/?tag=%e6%82%b2%e8%a7%82%e7%ad%96%e7%95%a5) [12 Comments]( "Comment on GC悲观策略之Parallel GC篇")
先来看段代码：
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
import

java.util.*;

public

class

SummaryCase{
   

public

static

void

main(String[] args)

throws

Exception{

      

List<Object> caches=

new

ArrayList<Object>();
      

for

(

int

i=

0

;i<

7

;i++){

          

caches.add(

new

byte

[

1024

*

1024

*

3

]);
      

}

      

caches.clear();
      

for

(

int

i=

0

;i<

2

;i++){

         

caches.add(

new

byte

[

1024

*

1024

*

3

]);
      

}

      

Thread.sleep(

10000

);
   

}

}

当用-Xms30m -Xmx30m -Xmn10m -XX:+UseParallelGC执行上面的代码时会执行几次Minor GC和几次Full GC呢？
按照eden空间不足时触发minor gc的规则，上面代码执行后的GC应为：M、M、M、M，但实际上上面代码执行后GC则为：M、M、M、F、F。
这里的原因就在于Parallel Scavenge GC时的悲观策略，当在eden上分配内存失败时且对象的大小尚不需要直接在old上分配时，会触发YGC，代码片段如下：

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
17

18
19

20
21

22
23

24
25

26
27

28
29

30
31

32
33

34
35

36
37void

PSScavenge::invoke(){

...
bool

scavenge_was_done = PSScavenge::invoke_no_policy();

PSGCAdaptivePolicyCounters* counters = heap->gc_policy_counters();
if

(UsePerfData)

counters->update_full_follows_scavenge(0);
if

(!scavenge_was_done ||

policy->should_full_GC(heap->old_gen()->free_in_bytes())) {
if

(UsePerfData)

counters->update_full_follows_scavenge(full_follows_scavenge);<
 

GCCauseSetter gccs(heap, GCCause::_adaptive_size_policy);
if

(UseParallelOldGC) {

PSParallelCompact::invoke_no_policy(

false

);
}

else

{

PSMarkSweep::invoke_no_policy(

false

);
}

}
...

}
PSScavenge::invoke_no_policy{

...
if

(!should_attempt_scavenge()) {

return

false

;
}

...
}

bool

PSScavenge::should_attempt_scavenge() {
...

PSAdaptiveSizePolicy* policy = heap->size_policy();
 

size_t

avg_promoted = (

size_t

) policy->padded_average_promoted_in_bytes();
size_t

promotion_estimate = MIN2(avg_promoted, young_gen->used_in_bytes());

bool

result = promotion_estimate < old_gen->free_in_bytes();
...

return

result;
}

在上面should_attempt_scavenge代码片段中，可以看到会比较之前YGC晋升到Old中的平均大小与当前新生代中已被使用的字节数大小，取更小的值与旧生代目前剩余空间大小对比，如更大，则返回false，就终止了YGC的执行了，当返回false时，PSScavenge::invoke就将触发Full GC了。
在PSScavenge:invoke中还有一个条件为：policy->should_full_GC(heap->old_gen()->free_in_bytes()，来看看这段代码片段：

[?]()1

2
3

4
5bool

PSAdaptiveSizePolicy::should_full_GC(

size_t

old_free_in_bytes) {

bool

result = padded_average_promoted_in_bytes() > (

float

) old_free_in_bytes;
...

return

result;
}

可看到，这段代码检查的也是之前YGC时晋升到old的平均大小是否大于了旧生代的剩余空间，如大于，则触发full gc。
总结上面分析的策略，可以看到采用Parallel GC的情况下，当YGC触发时，会有两个检查：
1、在YGC执行前，min(目前新生代已使用的大小,之前平均晋升到old的大小中的较小值) > 旧生代剩余空间大小 ? 不执行YGC，直接执行Full GC : 执行YGC；
2、在YGC执行后，平均晋升到old的大小 > 旧生代剩余空间大小 ? 触发Full GC ： 什么都不做。

按照这样的说明，再来看看上面代码的执行过程中eden和old大小的变化状况：
代码 eden old YGC FGC
第一次循环 3 0 0 0
第二次循环 6 0 0 0
第三次循环 3 6 1 0
第四次循环 6 6 1 0
第五次循环 3 12 2 0
第六次循环 6 12 2 0
第七次循环 3 18 3 1
第八次循环 6 18 3 1
第九次循环 3 3 3 2
在第7次循环时，YGC后旧生代剩余空间为2m，而之前平均晋级到old的对象大小为6m，因此在YGC后会触发一次FGC。
而第9次循环时，在YGC执行前，此时新生代已使用的大小为6m，之前晋级到old的平均大小为6m，这两者去最小值为6m，这个值已大于old的剩余空间，因此就不执行YGC，直接执行FGC了。

Sun JDK之所以要有悲观策略，我猜想理由是程序最终是会以一个较为稳态的状况执行的，此时每次YGC后晋升到old的对象大小应该是差不多的，在YGC时做好检查，避免等YGC后晋升到Old的对象导致old空间不足，因此还不如干脆就直接执行FGC，正因为悲观策略的存在，大家有些时候可能会看到old空间没满但full gc执行的状况。
埋个伏笔，大家将上面的执行参数换为-XX:+UseSerialGC执行看看，会发生什么呢？ ![:)]()

[JavaOne美国之行–硅谷公司交流篇](https://blog.bluedavy.com/?p=114) [GC悲观策略之Serial GC篇](https://blog.bluedavy.com/?p=170)

### 12 Comments *([+add yours?]())*

1. 
![]() xan
**Nov 08, 2010** @ 11:58:44
代码布局不怎么给力啊 ![:)]()
1. 
![](https://secure.gravatar.com/avatar/5267242305332fe1affd9c4788191e1d?s=48&d=<path_to_url>&r=G) [bluedavy](http://bluedavy.com/)
**Nov 08, 2010** @ 12:23:52
确实不给力，我折腾下。
1. 
![](https://secure.gravatar.com/avatar/3ed95d835a4c573899a9be257428c69f?s=48&d=<path_to_url>&r=G) [imbeneo](http://twitter.com/imbeneo)
**Nov 08, 2010** @ 13:12:45
我忘记了clean，结果总是OOM。
1. 
![](https://secure.gravatar.com/avatar/03ec79001b70a7dc41d51c1db4db4baa?s=48&d=<path_to_url>&r=G) oliver
**Apr 26, 2011** @ 11:34:35
我的疑问是为什么第一次full gc时old里面使用空间没有被回收掉，第二次full gc时old里面才回收？
1. 
![](https://secure.gravatar.com/avatar/5267242305332fe1affd9c4788191e1d?s=48&d=<path_to_url>&r=G) [bluedavy](http://bluedavy.com/)
**Apr 26, 2011** @ 13:32:18
因为caches.clear是后来才调的…
1. 
![](https://secure.gravatar.com/avatar/3cc9849504cb6c7c78b5533df02043ac?s=48&d=<path_to_url>&r=G) [yinhex](http://www.dosmile.net/)
**Jul 09, 2011** @ 22:38:20
这里我有个问题啊：我看你的分布式基础看到你能举例出很多大网站的设计。不知道你是从哪里可以看到这些信息的呢？可以细数一下你经常上去获取信息国外网站吗？
1. 
![](https://secure.gravatar.com/avatar/5267242305332fe1affd9c4788191e1d?s=48&d=<path_to_url>&r=G) [bluedavy](http://bluedavy.com/)
**Jul 11, 2011** @ 10:01:48
@yinhex
一般来说，例如highscalability.com，还有各种国外的技术大会，各家著名公司的engineer的blog，都会有这些信息，:)
1. 
![](https://secure.gravatar.com/avatar/f2ec9eb448e778dc947957c779fdd735?s=48&d=<path_to_url>&r=G) Gary
**Sep 29, 2011** @ 11:09:58
-XX:+UseSerialGC gc.log发现是MMMMF,符合预期啊
1. 
![](https://secure.gravatar.com/avatar/e9ef6892fbac714e6678cd30f4a212f4?s=48&d=<path_to_url>&r=G) yangxuesong
**Nov 29, 2011** @ 17:31:49
一次full gc的执行流程是怎么用的呢？
1. 
![](https://secure.gravatar.com/avatar/5267242305332fe1affd9c4788191e1d?s=48&d=<path_to_url>&r=G) [bluedavy](http://bluedavy.com/)
**Dec 03, 2011** @ 11:10:22
@yangxuesong
这个…要看是什么垃圾收集器…
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
