---
layout: post
title: "通过Java-JMX得到full GC次数？ - 高级语言虚拟机"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# 通过Java/JMX得到full GC次数？ - 高级语言虚拟机

[您还未登录 !](http://hllvm.group.iteye.com/login "登录") [登录](http://hllvm.group.iteye.com/login) [注册](http://hllvm.group.iteye.com/signup)

[![ITeye3.0]( "ITeye-最棒的软件开发交流社区")](http://www.iteye.com/)

[]()

[群组首页](http://www.iteye.com/groups) → [编程语言](http://hllvm.group.iteye.com/groups/category/language) → [高级语言虚拟机](http://hllvm.group.iteye.com/) → [知识库](http://hllvm.group.iteye.com/group/wiki) → [JVM实战](http://hllvm.group.iteye.com/group/wiki?category_id=315) → [通过Java/JMX得到full GC次数？]()
原创作者: [RednaxelaFX](http://www.javaeye.com/topic/790015)   阅读:2261次   评论:1条   更新时间:2011-05-26    

今天有个同事问如何能通过[JMX](http://java.sun.com/javase/technologies/core/mntr-mgmt/javamanagement/)获取到某个Java进程的full GC次数：
引用

hi,问个问题，怎们在java中获取到full gc的次数呢？
我现在用jmx的那个得到了gc次数，不过不能细化出来full gc的次数
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. for (final GarbageCollectorMXBean garbageCollector  
1.         : ManagementFactory.getGarbageCollectorMXBeans()) {  
1.     gcCounts += garbageCollector.getCollectionCount();  
1. }  
for (final GarbageCollectorMXBean garbageCollector : ManagementFactory.getGarbageCollectorMXBeans()) { gcCounts += garbageCollector.getCollectionCount(); }
你比如我现在是这样拿次数的
我回答说因为full GC概念只有在分代式GC的上下文中才存在，而JVM并不强制要求GC使用分代式实现，所以JMX提供的标准[MXBean](http://download-llnw.oracle.com/javase/6/docs/technotes/guides/management/mxbeans.html) API里不提供“full GC次数”这样的方法也正常。
既然“full GC”本来就是非常平台相关的概念，那就hack一点，用平台相关的代码来解决问题好了。这些GC的MXBean都是有名字的，而主流的JVM的GC名字相对稳定，非要通过JMX得到full GC次数的话，用名字来判断一下就好了。
举个例子来看看。通过JDK 6自带的[JConsole](http://download-llnw.oracle.com/javase/6/docs/technotes/guides/management/jconsole.html)工具来查看相关的MXBean的话，可以看到，
GC的MXBean在这个位置：
![]( "点击查看原始大小图片")
这个例子是用server模式启动JConsole的，使用的是ParallelScavenge GC，它的年老代对应的收集器在这里：
![]( "点击查看原始大小图片")
该收集器的总收集次数在此，这也就是full GC的次数：
![]( "点击查看原始大小图片")
于是只要知道我们用的JVM提供的GC MXBean的名字与分代的关系，就可以知道full GC的次数了。
Java代码写起来冗长，这帖就不用Java来写例子了，反正API是一样的，意思能表达清楚就OK。
用一个[Groovy](http://groovy.codehaus.org/)脚本简单演示一下适用于Oracle (Sun) HotSpot与Oracle (BEA) JRockit的GC统计程序：
Groovy代码  [![收藏代码]()![]()]( "收藏这段代码")

1. import java.lang.management.ManagementFactory  
1.   
1. printGCStats = {  
1.   def youngGenCollectorNames = [  
1.     // Oracle (Sun) HotSpot  
1.     // -XX:+UseSerialGC  
1.     'Copy',  
1.     // -XX:+UseParNewGC  
1.     'ParNew',  
1.     // -XX:+UseParallelGC  
1.     'PS Scavenge',  
1.       
1.     // Oracle (BEA) JRockit  
1.     // -XgcPrio:pausetime  
1.     'Garbage collection optimized for short pausetimes Young Collector',  
1.     // -XgcPrio:throughput  
1.     'Garbage collection optimized for throughput Young Collector',  
1.     // -XgcPrio:deterministic  
1.     'Garbage collection optimized for deterministic pausetimes Young Collector'  
1.   ]  
1.   
1.   def oldGenCollectorNames = [  
1.     // Oracle (Sun) HotSpot  
1.     // -XX:+UseSerialGC  
1.     'MarkSweepCompact',  
1.     // -XX:+UseParallelGC and (-XX:+UseParallelOldGC or -XX:+UseParallelOldGCCompacting)  
1.     'PS MarkSweep',  
1.     // -XX:+UseConcMarkSweepGC  
1.     'ConcurrentMarkSweep',  
1.       
1.     // Oracle (BEA) JRockit  
1.     // -XgcPrio:pausetime  
1.     'Garbage collection optimized for short pausetimes Old Collector',  
1.     // -XgcPrio:throughput  
1.     'Garbage collection optimized for throughput Old Collector',  
1.     // -XgcPrio:deterministic  
1.     'Garbage collection optimized for deterministic pausetimes Old Collector'  
1.   ]  
1.   
1.   R: {  
1.     ManagementFactory.garbageCollectorMXBeans.each {  
1.       def name  = it.name  
1.       def count = it.collectionCount  
1.       def gcType;  
1.       switch (name) {  
1.       case youngGenCollectorNames:  
1.         gcType = 'Minor Collection'  
1.         break  
1.       case oldGenCollectorNames:  
1.         gcType = 'Major Collection'  
1.         break  
1.       default:  
1.         gcType = 'Unknown Collection Type'  
1.         break  
1.       }  
1.       println "$count <- $gcType: $name"  
1.     }  
1.   }  
1. }  
1.   
1. printGCStats()  
import java.lang.management.ManagementFactory printGCStats = { def youngGenCollectorNames = [ // Oracle (Sun) HotSpot // -XX:+UseSerialGC 'Copy', // -XX:+UseParNewGC 'ParNew', // -XX:+UseParallelGC 'PS Scavenge', // Oracle (BEA) JRockit // -XgcPrio:pausetime 'Garbage collection optimized for short pausetimes Young Collector', // -XgcPrio:throughput 'Garbage collection optimized for throughput Young Collector', // -XgcPrio:deterministic 'Garbage collection optimized for deterministic pausetimes Young Collector' ] def oldGenCollectorNames = [ // Oracle (Sun) HotSpot // -XX:+UseSerialGC 'MarkSweepCompact', // -XX:+UseParallelGC and (-XX:+UseParallelOldGC or -XX:+UseParallelOldGCCompacting) 'PS MarkSweep', // -XX:+UseConcMarkSweepGC 'ConcurrentMarkSweep', // Oracle (BEA) JRockit // -XgcPrio:pausetime 'Garbage collection optimized for short pausetimes Old Collector', // -XgcPrio:throughput 'Garbage collection optimized for throughput Old Collector', // -XgcPrio:deterministic 'Garbage collection optimized for deterministic pausetimes Old Collector' ] R: { ManagementFactory.garbageCollectorMXBeans.each { def name = it.name def count = it.collectionCount def gcType; switch (name) { case youngGenCollectorNames: gcType = 'Minor Collection' break case oldGenCollectorNames: gcType = 'Major Collection' break default: gcType = 'Unknown Collection Type' break } println "$count <- $gcType: $name" } } } printGCStats()
执行可以看到类似这样的输出：
Command prompt代码  [![收藏代码]()![]()]( "收藏这段代码")

1. 5 <- Minor Collection: Copy  
1. 0 <- Major Collection: MarkSweepCompact  
5 <- Minor Collection: Copy 0 <- Major Collection: MarkSweepCompact
↑这是用client模式的HotSpot执行得到的；
Command prompt代码  [![收藏代码]()![]()]( "收藏这段代码")

1. 0 <- Minor Collection: Garbage collection optimized for throughput Young Collector  
1. 0 <- Major Collection: Garbage collection optimized for throughput Old Collector  
0 <- Minor Collection: Garbage collection optimized for throughput Young Collector 0 <- Major Collection: Garbage collection optimized for throughput Old Collector
↑这是用JRockit R28在32位Windows上的默认模式得到的。
通过上述方法，要包装起来方便以后使用的话也很简单，例如下面Groovy程序：
Groovy代码  [![收藏代码]()![]()]( "收藏这段代码")

1. import java.lang.management.ManagementFactory  
1.   
1. class GCStats {  
1.   static final List<String> YoungGenCollectorNames = [  
1.     // Oracle (Sun) HotSpot  
1.     // -XX:+UseSerialGC  
1.     'Copy',  
1.     // -XX:+UseParNewGC  
1.     'ParNew',  
1.     // -XX:+UseParallelGC  
1.     'PS Scavenge',  
1.       
1.     // Oracle (BEA) JRockit  
1.     // -XgcPrio:pausetime  
1.     'Garbage collection optimized for short pausetimes Young Collector',  
1.     // -XgcPrio:throughput  
1.     'Garbage collection optimized for throughput Young Collector',  
1.     // -XgcPrio:deterministic  
1.     'Garbage collection optimized for deterministic pausetimes Young Collector'  
1.   ]  
1.     
1.   static final List<String> OldGenCollectorNames = [  
1.     // Oracle (Sun) HotSpot  
1.     // -XX:+UseSerialGC  
1.     'MarkSweepCompact',  
1.     // -XX:+UseParallelGC and (-XX:+UseParallelOldGC or -XX:+UseParallelOldGCCompacting)  
1.     'PS MarkSweep',  
1.     // -XX:+UseConcMarkSweepGC  
1.     'ConcurrentMarkSweep',  
1.       
1.     // Oracle (BEA) JRockit  
1.     // -XgcPrio:pausetime  
1.     'Garbage collection optimized for short pausetimes Old Collector',  
1.     // -XgcPrio:throughput  
1.     'Garbage collection optimized for throughput Old Collector',  
1.     // -XgcPrio:deterministic  
1.     'Garbage collection optimized for deterministic pausetimes Old Collector'  
1.   ]  
1.     
1.   static int getYoungGCCount() {  
1.     ManagementFactory.garbageCollectorMXBeans.inject(0) { youngGCCount, gc ->  
1.       if (YoungGenCollectorNames.contains(gc.name))  
1.         youngGCCount + gc.collectionCount  
1.       else  
1.         youngGCCount  
1.     }  
1.   }  
1.     
1.   static int getFullGCCount() {  
1.     ManagementFactory.garbageCollectorMXBeans.inject(0) { fullGCCount, gc ->  
1.       if (OldGenCollectorNames.contains(gc.name))  
1.         fullGCCount + gc.collectionCount  
1.       else  
1.         fullGCCount  
1.     }  
1.   }  
1. }  
import java.lang.management.ManagementFactory class GCStats { static final List<String> YoungGenCollectorNames = [ // Oracle (Sun) HotSpot // -XX:+UseSerialGC 'Copy', // -XX:+UseParNewGC 'ParNew', // -XX:+UseParallelGC 'PS Scavenge', // Oracle (BEA) JRockit // -XgcPrio:pausetime 'Garbage collection optimized for short pausetimes Young Collector', // -XgcPrio:throughput 'Garbage collection optimized for throughput Young Collector', // -XgcPrio:deterministic 'Garbage collection optimized for deterministic pausetimes Young Collector' ] static final List<String> OldGenCollectorNames = [ // Oracle (Sun) HotSpot // -XX:+UseSerialGC 'MarkSweepCompact', // -XX:+UseParallelGC and (-XX:+UseParallelOldGC or -XX:+UseParallelOldGCCompacting) 'PS MarkSweep', // -XX:+UseConcMarkSweepGC 'ConcurrentMarkSweep', // Oracle (BEA) JRockit // -XgcPrio:pausetime 'Garbage collection optimized for short pausetimes Old Collector', // -XgcPrio:throughput 'Garbage collection optimized for throughput Old Collector', // -XgcPrio:deterministic 'Garbage collection optimized for deterministic pausetimes Old Collector' ] static int getYoungGCCount() { ManagementFactory.garbageCollectorMXBeans.inject(0) { youngGCCount, gc -> if (YoungGenCollectorNames.contains(gc.name)) youngGCCount + gc.collectionCount else youngGCCount } } static int getFullGCCount() { ManagementFactory.garbageCollectorMXBeans.inject(0) { fullGCCount, gc -> if (OldGenCollectorNames.contains(gc.name)) fullGCCount + gc.collectionCount else fullGCCount } } }
用的时候：
Groovysh代码  [![收藏代码]()![]()]( "收藏这段代码")

1. D:\>\sdk\groovy-1.7.2\bin\groovysh  
1. Groovy Shell (1.7.2, JVM: 1.6.0_20)  
1. Type 'help' or '\h' for help.  
1. --------------------------------------------------  
1. groovy:000> GCStats.fullGCCount  
1. ===> 0  
1. groovy:000> System.gc()  
1. ===> null  
1. groovy:000> GCStats.fullGCCount  
1. ===> 1  
1. groovy:000> System.gc()  
1. ===> null  
1. groovy:000> System.gc()  
1. ===> null  
1. groovy:000> GCStats.fullGCCount  
1. ===> 3  
1. groovy:000> GCStats.youngGCCount  
1. ===> 9  
1. groovy:000> GCStats.youngGCCount  
1. ===> 9  
1. groovy:000> GCStats.youngGCCount  
1. ===> 9  
1. groovy:000> System.gc()  
1. ===> null  
1. groovy:000> GCStats.youngGCCount  
1. ===> 9  
1. groovy:000> GCStats.fullGCCount  
1. ===> 4  
1. groovy:000> quit  
D:\>\sdk\groovy-1.7.2\bin\groovysh Groovy Shell (1.7.2, JVM: 1.6.0_20) Type 'help' or '\h' for help. -------------------------------------------------- groovy:000> GCStats.fullGCCount ===> 0 groovy:000> System.gc() ===> null groovy:000> GCStats.fullGCCount ===> 1 groovy:000> System.gc() ===> null groovy:000> System.gc() ===> null groovy:000> GCStats.fullGCCount ===> 3 groovy:000> GCStats.youngGCCount ===> 9 groovy:000> GCStats.youngGCCount ===> 9 groovy:000> GCStats.youngGCCount ===> 9 groovy:000> System.gc() ===> null groovy:000> GCStats.youngGCCount ===> 9 groovy:000> GCStats.fullGCCount ===> 4 groovy:000> quit
这是在Sun JDK 6 update 20上跑的。顺带一提，如果这是跑在JRockit上的话，那full GC的次数就不会增加——因为JRockit里System.gc()默认是触发young GC的；请不要因为Sun HotSpot的默认行为而认为System.gc()总是会触发full GC的。
关于JMX的MXBean的使用，也可以参考下面两篇文档：
[Groovy and JMX](http://docs.codehaus.org/display/GROOVY/Groovy+and+JMX)
[Monitoring the JVM Heap with JRuby](http://www.engineyard.com/blog/2010/monitoring-the-jvm-heap-with-jruby/)
[如何更快的启动eclipse](http://hllvm.group.iteye.com/group/wiki/3042-JVM-eclipse "如何更快的启动eclipse")

评论 共 1 条 请[登录](http://hllvm.group.iteye.com/login)后发表评论 []()

### 1 楼 [xgj1988](http://xgj1988.iteye.com/ "xgj1988") 2011-04-27 15:07

SUN jdk   使用FULL GC 是如下的几种之一？
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. static final List<String> OldGenCollectorNames = [     
1.    // Oracle (Sun) HotSpot     
1.    // -XX:+UseSerialGC     
1.    'MarkSweepCompact',     
1.    // -XX:+UseParallelGC and (-XX:+UseParallelOldGC or -XX:+UseParallelOldGCCompacting)     
1.    'PS MarkSweep',     
1.    // -XX:+UseConcMarkSweepGC     
1.    'ConcurrentMarkSweep',     
1.     
1.  ]    
static final List<String> OldGenCollectorNames = [ // Oracle (Sun) HotSpot // -XX:+UseSerialGC 'MarkSweepCompact', // -XX:+UseParallelGC and (-XX:+UseParallelOldGC or -XX:+UseParallelOldGCCompacting) 'PS MarkSweep', // -XX:+UseConcMarkSweepGC 'ConcurrentMarkSweep', ]
这几个是young gc?
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. 'Copy',     
1.     // -XX:+UseParNewGC     
1.     'ParNew',     
1.     // -XX:+UseParallelGC     
1.     'PS Scavenge',     
1.          
'Copy', // -XX:+UseParNewGC 'ParNew', // -XX:+UseParallelGC 'PS Scavenge',
根据名字判断来获取full gc![]()
### 发表评论

[![]() 您还没有登录,请您登录后再发表评论](http://hllvm.group.iteye.com/login)

[![New-page]()](http://hllvm.group.iteye.com/group/wiki/new)

### 文章信息

[知识库: 高级语言虚拟机](http://hllvm.group.iteye.com/group/wiki/)

* 由[fantasy](http://kiral.iteye.com/ "fantasy")在2010-12-16创建
* 由[fantasy](http://kiral.iteye.com/ "fantasy")在2011-05-26更新
### 相关新闻

* [Java SE 6 Update 14 Early Access 早期使用版本现已发布](http://hllvm.group.iteye.com/news/6318-java-se-6-update-14-early-access-version)
* [JDK 7 中新的垃圾收集机制](http://hllvm.group.iteye.com/news/4146-jdk-7-in-the-new-garbage-collection-mechanism)
* [Java 7的新功能和Java 1.5,1.6,1.7的性能测试比较](http://hllvm.group.iteye.com/news/10069)

### 相关讨论

* [HotSpot VM 内存堆的两个Servivor区](http://hllvm.group.iteye.com/topic/894148)
* [java内存管理以及GC](http://hllvm.group.iteye.com/topic/976522)
* [JVM内存管理：深入垃圾收集器与内存分配策略](http://hllvm.group.iteye.com/topic/802638)
* [一次Java垃圾收集调优实战](http://hllvm.group.iteye.com/topic/212967)
* [JVM的GC-生命不能承受之重](http://hllvm.group.iteye.com/topic/262541)
### 相关博客

* [通过Java/JMX得到full GC次数？](http://rednaxelafx.iteye.com/blog/790015)
* [通过Java/JMX得到full GC次数？](http://millerhu.iteye.com/blog/890724)
* [用Java获取full GC的次数？（2）](http://rednaxelafx.iteye.com/blog/790864)
* [常用垃圾收集器在Mbean上的名称](http://tianshibaijia.iteye.com/blog/1343308)
* [答复: HotSpot VM 内存堆的两个Survivor区](http://rednaxelafx.iteye.com/blog/1042471)
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
百联优力(北京)投资有限公司 版权所有 ![](http://stat.iteye.com/?url=http%3A%2F%2Fhllvm.group.iteye.com%2Fgroup%2Fwiki%2F2950-gc&referrer=&user_id=)
