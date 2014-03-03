---
layout: post
title: "JVM调优的-标准参数-的各种陷阱"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# JVM调优的"标准参数"的各种陷阱

[您还未登录 !](http://hllvm.group.iteye.com/login "登录") [登录](http://hllvm.group.iteye.com/login) [注册](http://hllvm.group.iteye.com/signup)

[![ITeye3.0]( "ITeye-最棒的软件开发交流社区")](http://www.iteye.com/)

[群组首页](http://www.iteye.com/groups) → [编程语言](http://hllvm.group.iteye.com/groups/category/language) → [高级语言虚拟机](http://hllvm.group.iteye.com/) → [论坛](http://hllvm.group.iteye.com/group/forum)

![]() [发表回复](http://hllvm.group.iteye.com/group/topic/27945/post/new)

« 上一页 1 [2](http://hllvm.group.iteye.com/group/topic/27945?page=2) [3](http://hllvm.group.iteye.com/group/topic/27945?page=3) [下一页 »](http://hllvm.group.iteye.com/group/topic/27945?page=2)

### [[讨论]](http://hllvm.group.iteye.com/group/forum?tag_id=690) [JVM调优的"标准参数"的各种陷阱]()

[![RednaxelaFX的博客]( "RednaxelaFX的博客: Script Ahead, Code Behind")](http://rednaxelafx.iteye.com/) [RednaxelaFX](http://rednaxelafx.iteye.com/ "RednaxelaFX") 2011-10-23

开个帖大家来讨论下自己遇到过的情况吧？我在顶楼举几个例子。
开这帖的目的是想让大家了解到，所谓“标准参数”是件很微妙的事情。确实有许多前辈经过多年开发积累下了许多有用的调优经验，但向他们问“标准参数”并照单全收是件危险的事情。
前辈们提供的“标准参数”或许适用于他们的应用场景，他们或许也知道这些参数里隐含的陷阱；但听众却不一定知道各种参数背后的缘由。
原则上说，在生产环境使用非标准参数（这里指的是在各JDK/JRE实现特有的、相互之间不通用的参数）应该尽量避免。这些参数与具体实现密切相关，不是光了解很抽象的“JVM原理”就足以理解的；即便在同一系列的JDK/JRE实现中，非标准参数也不保证在各版本间有一样的作用；而且许多人只看名字就猜想参数的左右，做“调优”却适得其反。
非标准参数的默认值在不同版本间或许会悄然发生变化。这些变化的背后多半有合理的理由。设了一大堆非标准参数、不明就里的同学在升级JDK/JRE的时候也容易掉坑里。
下面用**Oracle/Sun JDK 6**来举几个例子。这帖顶楼里的讨论如果没明确指出JDK版本的都是指Oracle/Sun JDK 6（OpenJDK 6也可以算在内）。
经验不一定适用于Sun JDK 1.4.2、Sun JDK 5、Oracle JDK 7。
======================================================================
0、各参数的默认值
在讨论HotSpot VM的各参数的陷阱前，大家应该先了解HotSpot VM到底有哪些参数可以设置，这些参数的默认值都是什么。
有几种办法可以帮助大家获取参数的信息。首先为了大致了解都有些什么参数可以设置，可以参考HotSpot VM里的各个globals.hpp文件：（以下链接取自HotSpot 20.0，与JDK 6 update 25对应）
[globals.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/share/vm/runtime/globals.hpp)
[globals_extension.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/share/vm/runtime/globals_extension.hpp)
[c1_globals.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/share/vm/c1/c1_globals.hpp)
[c1_globals_linux.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os/linux/vm/c1_globals_linux.hpp)
[c1_globals_solaris.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os/solaris/vm/c1_globals_solaris.hpp)
[c1_globals_sparc.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/cpu/sparc/vm/c1_globals_sparc.hpp)
[c1_globals_windows.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os/windows/vm/c1_globals_windows.hpp)
[c1_globals_x86.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/cpu/x86/vm/c1_globals_x86.hpp)
[c2_globals.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/share/vm/opto/c2_globals.hpp)
[c2_globals_linux.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os/linux/vm/c2_globals_linux.hpp)
[c2_globals_solaris.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os/solaris/vm/c2_globals_solaris.hpp)
[c2_globals_sparc.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/cpu/sparc/vm/c2_globals_sparc.hpp)
[c2_globals_windows.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os/windows/vm/c2_globals_windows.hpp)
[c2_globals_x86.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/cpu/x86/vm/c2_globals_x86.hpp)
[g1_globals.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/share/vm/gc_implementation/g1/g1_globals.hpp)
[globals_linux.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os/linux/vm/globals_linux.hpp)
[globals_linux_sparc.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os_cpu/linux_sparc/vm/globals_linux_sparc.hpp)
[globals_linux_x86.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os_cpu/linux_x86/vm/globals_linux_x86.hpp)
[globals_linux_zero.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os_cpu/linux_zero/vm/globals_linux_zero.hpp)
[globals_solaris.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os/solaris/vm/globals_solaris.hpp)
[globals_solaris_sparc.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os_cpu/solaris_sparc/vm/globals_solaris_sparc.hpp)
[globals_solaris_x86.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os_cpu/solaris_x86/vm/globals_solaris_x86.hpp)
[globals_sparc.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/cpu/sparc/vm/globals_sparc.hpp)
[globals_windows.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os/windows/vm/globals_windows.hpp)
[globals_windows_x86.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/os_cpu/windows_x86/vm/globals_windows_x86.hpp)
[globals_x86.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/cpu/x86/vm/globals_x86.hpp)
[globals_zero.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/cpu/zero/vm/globals_zero.hpp)
[shark_globals.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/share/vm/shark/shark_globals.hpp)
[shark_globals_zero.hpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/cpu/zero/vm/shark_globals_zero.hpp)
[arguments.cpp](http://hg.openjdk.java.net/hsx/hsx20/master/file/f0f676c5a2c6/src/share/vm/runtime/arguments.cpp)
然后是 **-XX:+PrintCommandLineFlags** 。这个参数的作用是显示出VM初始化完毕后所有跟最初的默认值不同的参数及它们的值。
这个参数至少在Sun JDK 5上已经开始支持，Oracle/Sun JDK 6以及Oracle JDK 7上也可以使用。Sun JDK 1.4.2还不支持这个参数。
例子：
Command prompt代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. $ java -XX:+PrintCommandLineFlags  
1. VM option '+PrintCommandLineFlags'  
1. -XX:InitialHeapSize=57344000 -XX:MaxHeapSize=917504000 -XX:ParallelGCThreads=4 -XX:+PrintCommandLineFlags -XX:+UseCompressedOops -XX:+UseParallelGC   
$ java -XX:+PrintCommandLineFlags VM option '+PrintCommandLineFlags' -XX:InitialHeapSize=57344000 -XX:MaxHeapSize=917504000 -XX:ParallelGCThreads=4 -XX:+PrintCommandLineFlags -XX:+UseCompressedOops -XX:+UseParallelGC
[《Java Performance》一书](http://book.douban.com/annotation/15146892/)主要是用这个参数来介绍HotSpot VM各参数的效果的。
接着是 **-XX:+PrintFlagsFinal** 。前一个参数只显示跟默认值不同的，而这个参数则可以显示所有可设置的参数及它们的值。不过这个参数本身只从JDK 6 update 21开始才可以用，之前的Oracle/Sun JDK则用不了。
可以设置的参数默认是不包括diagnostic或experimental系的。要在-XX:+PrintFlagsFinal的输出里看到这两种参数的信息，分别需要显式指定**-XX:+UnlockDiagnosticVMOptions** / **-XX:+UnlockExperimentalVMOptions** 。
再下来，**-XX:+PrintFlagsInitial** 。这个参数显示在处理参数之前所有可设置的参数及它们的值，然后直接退出程序。“参数处理”包括许多步骤，例如说检查参数之间是否有冲突，通过ergonomics调整某些参数的值，之类的。
结合-XX:+PrintFlagsInitial与-XX:+PrintFlagsFinal，对比两者的差异，就可以知道ergonomics对哪些参数做了怎样的调整。
这两个参数的例子：
Command prompt代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. $ java -version  
1. java version "1.6.0_29"  
1. Java(TM) SE Runtime Environment (build 1.6.0_29-b11)  
1. Java HotSpot(TM) 64-Bit Server VM (build 20.4-b02, mixed mode)  
1. $ java -XX:+PrintFlagsInitial | grep UseCompressedOops  
1.      bool UseCompressedOops                         = false           {lp64_product}        
1. $ java -XX:+PrintFlagsFinal | grep UseCompressedOops  
1.      bool UseCompressedOops                        := true            {lp64_product}        
$ java -version java version "1.6.0_29" Java(TM) SE Runtime Environment (build 1.6.0_29-b11) Java HotSpot(TM) 64-Bit Server VM (build 20.4-b02, mixed mode) $ java -XX:+PrintFlagsInitial | grep UseCompressedOops bool UseCompressedOops = false {lp64_product} $ java -XX:+PrintFlagsFinal | grep UseCompressedOops bool UseCompressedOops := true {lp64_product}
[Oracle/Sun JDK 6从update 23开始会由ergonomics在合适的条件下默认启用压缩指针功能](http://rednaxelafx.iteye.com/blog/1010079)。这个例子演示的是UseCompressedOops的初始默认值是false，由PrintFlagsInitial的输出可以看到；然后经过ergonomics自动调整后，最终采用的默认值是true，由PrintFlagsFinal的输出可以看到。
输出里“=”表示使用的是初始默认值，而“:=”表示使用的不是初始默认值，可能是命令行传进来的参数、配置文件里的参数或者是ergonomics自动选择了别的值。
除了在VM启动时传些特殊的参数让它打印出自己的各参数外，**jinfo -flag** 可以用来查看某个参数的值，也可以用来设定manageable系参数的值。请参考这帖的例子：[通过jinfo工具在full GC前后做heap dump](http://rednaxelafx.iteye.com/blog/1049240)
之前发过[某些环境中HotSpot VM的各参数的默认值](http://rednaxelafx.iteye.com/blog/906807)，可以参考一下。
======================================================================
1、-XX:+DisableExplicitGC 与 NIO的direct memory
很多人都见过JVM调优建议里使用这个参数，对吧？但是为什么要用它，什么时候应该用而什么时候用了会掉坑里呢？
首先要了解的是这个参数的作用。在Oracle/Sun JDK这个具体实现上，System.gc()的默认效果是引发一次stop-the-world的full GC，对整个GC堆做收集。有几个参数可以改变默认行为，之前[发过一帖简单描述](http://rednaxelafx.iteye.com/blog/1042471)过，这里就不重复了。关键点是，用了-XX:+DisableExplicitGC参数后，System.gc()的调用就会变成一个空调用，完全不会触发任何GC（但是“函数调用”本身的开销还是存在的哦～）。
为啥要用这个参数呢？最主要的原因是为了防止某些手贱的同学在代码里到处写System.gc()的调用而干扰了程序的正常运行吧。有些应用程序本来可能正常跑一天也不会出一次full GC，但就是因为有人在代码里调用了System.gc()而不得不间歇性被暂停。也有些时候这些调用是在某些库或框架里写的，改不了它们的代码但又不想被这些调用干扰也会用这参数。
OK。看起来这参数应该总是开着嘛。有啥坑呢？
其中一种情况是下述三个条件同时满足时会发生的：
1、应用本身在GC堆内的对象行为良好，正常情况下很久都不发生full GC；
2、应用大量使用了NIO的direct memory，经常、反复的申请DirectByteBuffer
3、使用了-XX:+DisableExplicitGC
能观察到的现象是：
Log代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. java.lang.OutOfMemoryError: Direct buffer memory  
1.     at java.nio.Bits.reserveMemory(Bits.java:633)  
1.     at java.nio.DirectByteBuffer.<init>(DirectByteBuffer.java:98)  
1.     at java.nio.ByteBuffer.allocateDirect(ByteBuffer.java:288)  
1. ...  
java.lang.OutOfMemoryError: Direct buffer memory at java.nio.Bits.reserveMemory(Bits.java:633) at java.nio.DirectByteBuffer.<init>(DirectByteBuffer.java:98) at java.nio.ByteBuffer.allocateDirect(ByteBuffer.java:288) ...
做个简单的例子来演示这现象：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. import java.nio.*;  
1.   
1. public class DisableExplicitGCDemo {  
1.   public static void main(String[] args) {  
1.     for (int i = 0; i < 100000; i++) {  
1.       ByteBuffer.allocateDirect(128);  
1.     }  
1.     System.out.println("Done");  
1.   }  
1. }  
import java.nio.*; public class DisableExplicitGCDemo { public static void main(String[] args) { for (int i = 0; i < 100000; i++) { ByteBuffer.allocateDirect(128); } System.out.println("Done"); } }
然后编译、运行之：
Command prompt代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. $ java -version  
1. java version "1.6.0_25"  
1. Java(TM) SE Runtime Environment (build 1.6.0_25-b06)  
1. Java HotSpot(TM) 64-Bit Server VM (build 20.0-b11, mixed mode)  
1. $ javac DisableExplicitGCDemo.java   
1. $ java -XX:MaxDirectMemorySize=10m -XX:+PrintGC -XX:+DisableExplicitGC DisableExplicitGCDemo  
1. Exception in thread "main" java.lang.OutOfMemoryError: Direct buffer memory  
1.     at java.nio.Bits.reserveMemory(Bits.java:633)  
1.     at java.nio.DirectByteBuffer.<init>(DirectByteBuffer.java:98)  
1.     at java.nio.ByteBuffer.allocateDirect(ByteBuffer.java:288)  
1.     at DisableExplicitGCDemo.main(DisableExplicitGCDemo.java:6)  
1. $ java -XX:MaxDirectMemorySize=10m -XX:+PrintGC DisableExplicitGCDemo  
1. [GC 10996K->10480K(120704K), 0.0433980 secs]  
1. [Full GC 10480K->10415K(120704K), 0.0359420 secs]  
1. Done  
$ java -version java version "1.6.0_25" Java(TM) SE Runtime Environment (build 1.6.0_25-b06) Java HotSpot(TM) 64-Bit Server VM (build 20.0-b11, mixed mode) $ javac DisableExplicitGCDemo.java $ java -XX:MaxDirectMemorySize=10m -XX:+PrintGC -XX:+DisableExplicitGC DisableExplicitGCDemo Exception in thread "main" java.lang.OutOfMemoryError: Direct buffer memory at java.nio.Bits.reserveMemory(Bits.java:633) at java.nio.DirectByteBuffer.<init>(DirectByteBuffer.java:98) at java.nio.ByteBuffer.allocateDirect(ByteBuffer.java:288) at DisableExplicitGCDemo.main(DisableExplicitGCDemo.java:6) $ java -XX:MaxDirectMemorySize=10m -XX:+PrintGC DisableExplicitGCDemo [GC 10996K->10480K(120704K), 0.0433980 secs] [Full GC 10480K->10415K(120704K), 0.0359420 secs] Done
可以看到，同样的程序，不带-XX:+DisableExplicitGC时能正常完成运行，而带上这个参数后却出现了OOM。
例子里用-XX:MaxDirectMemorySize=10m限制了DirectByteBuffer能分配的空间的限额，以便问题更容易展现出来。不用这个参数就得多跑一会儿了。
在这个例子里，main()里的循环不断申请DirectByteBuffer但并没有引用、使用它们，所以这些DirectByteBuffer应该刚创建出来就已经满足被GC的条件，等下次GC运行的时候就应该可以被回收。
实际上却没这么简单。DirectByteBuffer是种典型的“冰山”对象，也就是说它的Java对象虽然很小很无辜，但它背后却会关联着一定量的native memory资源，而这些资源并不在GC的控制之下，需要自己注意控制好。对JVM如何使用native memory不熟悉的同学可以参考去年JavaOne上IBM的一个演讲，“Where Does All the Native Memory Go”。
Oracle/Sun JDK的实现里，DirectByteBuffer有几处值得注意的地方。
1、DirectByteBuffer没有finalizer，它的native memory的清理工作是通过sun.misc.Cleaner自动完成的。
2、sun.misc.Cleaner是一种基于PhantomReference的清理工具，比普通的finalizer轻量些。对PhantomReference不熟悉的同学请参考Bob Lee最近几年在JavaOne上做的演讲，["The Ghost in the Virtual Machine: A Reference to References"](http://www.parleys.com/d/2657)。[今年的JavaOne](https://oracleus.wingateweb.com/scheduler/eventcatalog/eventCatalogJavaOne.do)上他也讲了同一个主题，内容比前几年的稍微更新了些。PPT可以从链接里的页面下载到。
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. /** 
1.  * General-purpose phantom-reference-based cleaners. 
1.  * 
1.  * <p> Cleaners are a lightweight and more robust alternative to finalization. 
1.  * They are lightweight because they are not created by the VM and thus do not 
1.  * require a JNI upcall to be created, and because their cleanup code is 
1.  * invoked directly by the reference-handler thread rather than by the 
1.  * finalizer thread.  They are more robust because they use phantom references, 
1.  * the weakest type of reference object, thereby avoiding the nasty ordering 
1.  * problems inherent to finalization. 
1.  * 
1.  * <p> A cleaner tracks a referent object and encapsulates a thunk of arbitrary 
1.  * cleanup code.  Some time after the GC detects that a cleaner's referent has 
1.  * become phantom-reachable, the reference-handler thread will run the cleaner. 
1.  * Cleaners may also be invoked directly; they are thread safe and ensure that 
1.  * they run their thunks at most once. 
1.  * 
1.  * <p> Cleaners are not a replacement for finalization.  They should be used 
1.  * only when the cleanup code is extremely simple and straightforward. 
1.  * Nontrivial cleaners are inadvisable since they risk blocking the 
1.  * reference-handler thread and delaying further cleanup and finalization. 
1.  * 
1.  * 
1.  * @author Mark Reinhold 
1.  * @version %I%, %E% 
1.  */  
/** * General-purpose phantom-reference-based cleaners. * * <p> Cleaners are a lightweight and more robust alternative to finalization. * They are lightweight because they are not created by the VM and thus do not * require a JNI upcall to be created, and because their cleanup code is * invoked directly by the reference-handler thread rather than by the * finalizer thread. They are more robust because they use phantom references, * the weakest type of reference object, thereby avoiding the nasty ordering * problems inherent to finalization. * * <p> A cleaner tracks a referent object and encapsulates a thunk of arbitrary * cleanup code. Some time after the GC detects that a cleaner's referent has * become phantom-reachable, the reference-handler thread will run the cleaner. * Cleaners may also be invoked directly; they are thread safe and ensure that * they run their thunks at most once. * * <p> Cleaners are not a replacement for finalization. They should be used * only when the cleanup code is extremely simple and straightforward. * Nontrivial cleaners are inadvisable since they risk blocking the * reference-handler thread and delaying further cleanup and finalization. * * * @author Mark Reinhold * @version %I%, %E% */
重点是这两句："A cleaner tracks a referent object and encapsulates a thunk of arbitrary cleanup code.  Some time after the GC detects that a cleaner's referent has become phantom-reachable, the reference-handler thread will run the cleaner."
Oracle/Sun JDK 6中的HotSpot VM只会在old gen GC（full GC/major GC或者concurrent GC都算）的时候才会对old gen中的对象做reference processing，而在young GC/minor GC时只会对young gen里的对象做reference processing。
（死在young gen中的DirectByteBuffer对象会在young GC时被处理的例子，请参考这里：[https://gist.github.com/1614952](https://gist.github.com/1614952)）
也就是说，做full GC的话会对old gen做reference processing，进而能触发Cleaner对已死的DirectByteBuffer对象做清理工作。而如果很长一段时间里没做过GC或者只做了young GC的话则不会在old gen触发Cleaner的工作，那么就可能让本来已经死了的、但已经晋升到old gen的DirectByteBuffer关联的native memory得不到及时释放。
3、为DirectByteBuffer分配空间过程中会显式调用System.gc()，以期通过full GC来强迫已经无用的DirectByteBuffer对象释放掉它们关联的native memory：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. // These methods should be called whenever direct memory is allocated or  
1. // freed.  They allow the user to control the amount of direct memory  
1. // which a process may access.  All sizes are specified in bytes.  
1. static void reserveMemory(long size) {  
1.   
1.     synchronized (Bits.class) {  
1.         if (!memoryLimitSet && VM.isBooted()) {  
1.             maxMemory = VM.maxDirectMemory();  
1.             memoryLimitSet = true;  
1.         }  
1.         if (size <= maxMemory - reservedMemory) {  
1.             reservedMemory += size;  
1.             return;  
1.         }  
1.     }  
1.   
1.     System.gc();  
1.     try {  
1.         Thread.sleep(100);  
1.     } catch (InterruptedException x) {  
1.         // Restore interrupt status  
1.         Thread.currentThread().interrupt();  
1.     }  
1.     synchronized (Bits.class) {  
1.         if (reservedMemory + size > maxMemory)  
1.             throw new OutOfMemoryError("Direct buffer memory");  
1.         reservedMemory += size;  
1.     }  
1.   
1. }  
// These methods should be called whenever direct memory is allocated or // freed. They allow the user to control the amount of direct memory // which a process may access. All sizes are specified in bytes. static void reserveMemory(long size) { synchronized (Bits.class) { if (!memoryLimitSet && VM.isBooted()) { maxMemory = VM.maxDirectMemory(); memoryLimitSet = true; } if (size <= maxMemory - reservedMemory) { reservedMemory += size; return; } } System.gc(); try { Thread.sleep(100); } catch (InterruptedException x) { // Restore interrupt status Thread.currentThread().interrupt(); } synchronized (Bits.class) { if (reservedMemory + size > maxMemory) throw new OutOfMemoryError("Direct buffer memory"); reservedMemory += size; } }
这几个实现特征使得Oracle/Sun JDK 6依赖于System.gc()触发GC来保证DirectByteMemory的清理工作能及时完成。如果打开了-XX:+DisableExplicitGC，清理工作就可能得不到及时完成，于是就有机会见到direct memory的OOM，也就是上面的例子演示的情况。我们这边在实际生产环境中确实遇到过这样的问题。
教训是：如果你在使用Oracle/Sun JDK 6，应用里有任何地方用了direct memory，那么使用-XX:+DisableExplicitGC要小心。如果用了该参数而且遇到direct memory的OOM，可以尝试去掉该参数看是否能避开这种OOM。如果担心System.gc()调用造成full GC频繁，可以尝试下面提到 -XX:+ExplicitGCInvokesConcurrent 参数
======================================================================
2、-XX:+DisableExplicitGC 与 Remote Method Invocation (RMI) 与 -Dsun.rmi.dgc.{server|client}.gcInterval=
看了上一个例子有没有觉得-XX:+DisableExplicitGC参数用起来很危险？那干脆完全不要用这个参数吧。又有什么坑呢？
前段时间有个应用的开发来抱怨，说某次升级JDK之前那应用的GC状况都很好，很长时间都不会发生full GC，但升级后发现每一小时左右就会发生一次。经过对比发现，升级的同时也吧启动参数改了，把原本有的-XX:+DisableExplicitGC给去掉了。
观察到的日志有明显特征。一位同事表示：
引用

线上机器出现一个场景；每隔1小时出现一次Full GC,用btrace看了一下调用地：
who call system.gc :
sun.misc.GC$Daemon.run(GC.java:92)
预发机没什么流量，也会每一小时一次Full GC
频率正好是一小时一次
Gc log代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. 2011-09-23T10:49:38.071+0800: 327692.227: [Full GC (System) 327692.227: [CMS: 75793K->75759K(2097152K), 0.6923690 secs] 430298K->75759K(3984640K), [CMS Perm : 104136K->104124K(173932K)], 0.6925570 secs]  
2011-09-23T10:49:38.071+0800: 327692.227: [Full GC (System) 327692.227: [CMS: 75793K->75759K(2097152K), 0.6923690 secs] 430298K->75759K(3984640K), [CMS Perm : 104136K->104124K(173932K)], 0.6925570 secs]
实际上这里在做的是分布式GC。Sun JDK的分布式GC是用纯Java实现的，为RMI服务。
RMI DGC相关参数的介绍文档：[http://docs.oracle.com/javase/6/docs/technotes/guides/rmi/sunrmiproperties.html](http://docs.oracle.com/javase/6/docs/technotes/guides/rmi/sunrmiproperties.html)
（后面回头再补…先睡觉去了）
资料：
[jGuru: Distributed Garbage Collection](http://java.sun.com/developer/onlineTraining/rmi/exercises/DistributedGarbageCollector/index.html)
[http://mail.openjdk.java.net/pipermail/hotspot-dev/2011-December/004909.html](http://mail.openjdk.java.net/pipermail/hotspot-dev/2011-December/004909.html)
Tony Printezis 写道

RMI has a distributed GC that relies on reference processing to allow
each node to recognize that some objects are unreachable so it can
notify a remote node (or nodes) that some remote references to them do
not exist any more. The remote node might then be able to reclaim
objects that are only remotely reachable. (Or this is how I understood
it at least.)
RMI used to call System.gc() once a minute (!!!) but after some
encouragement from yours truly they changed the default to once an hour
(this is configurable using a property). Note that a STW Full GC is not
really required as long as references are processed. So, in CMS (and
G1), a concurrent cycle is fine which is why we recommend to use
-XX:+ExplicitGCInvokesConcurrent in this case.
I had been warned by the RMI folks against totally disabling those
System.gc()'s (e.g., using -XX:+DisableExplicitGC) given that if Full
GCs / concurrent cycles do not otherwise happen at a reasonable
frequency then remote nodes might experience memory leaks since they
will consider that some otherwise unreachable remote references are
still live. I have no idea how severe such memory leaks would be. I
guess they'd be very application-dependent.
An additional thought that just occurred to me: instead of calling
System.gc() every hour what RMI should really be doing is calling
System.gc() every hour provided no old gen GC has taken place during the
last hour. This would be relatively easy to implement by accessing the
old GC counter through the GC MXBeans.
Tony
再加俩链接：[http://mail.openjdk.java.net/pipermail/hotspot-dev/2012-January/004929.html](http://mail.openjdk.java.net/pipermail/hotspot-dev/2012-January/004929.html)
[http://mail.openjdk.java.net/pipermail/hotspot-dev/2012-January/004946.html](http://mail.openjdk.java.net/pipermail/hotspot-dev/2012-January/004946.html)
《Java Performance》的303和411页正好也提到了-XX:+DisableExplicitGC与RMI之间的干扰的事情，有兴趣可以读一下，虽然只有一小段。
======================================================================
3、-XX:+ExplicitGCInvokesConcurrent 或 -XX:+ExplicitGCInvokesConcurrentAndUnloadsClasses
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. product(bool, ExplicitGCInvokesConcurrent, false,                  \  
1.   "A System.gc() request invokes a concurrent collection;"         \  
1.   " (effective only when UseConcMarkSweepGC)")                     \  
1.                                    \  
1. product(bool, ExplicitGCInvokesConcurrentAndUnloadsClasses, false, \  
1.   "A System.gc() request invokes a concurrent collection and "     \  
1.   "also unloads classes during such a concurrent gc cycle "        \  
1.   "(effective only when UseConcMarkSweepGC)")                      \  
product(bool, ExplicitGCInvokesConcurrent, false, \ "A System.gc() request invokes a concurrent collection;" \ " (effective only when UseConcMarkSweepGC)") \ \ product(bool, ExplicitGCInvokesConcurrentAndUnloadsClasses, false, \ "A System.gc() request invokes a concurrent collection and " \ "also unloads classes during such a concurrent gc cycle " \ "(effective only when UseConcMarkSweepGC)") \
跟上面的第一个例子的-XX:+DisableExplicitGC一样，这两个参数也是用来改变System.gc()的默认行为用的；不同的是这两个参数只能配合CMS使用（-XX:+UseConcMarkSweepGC），而且System.gc()还是会触发GC的，只不过不是触发一个完全stop-the-world的full GC，而是一次并发GC周期。
CMS GC周期中也会做reference processing。所以如果用这两个参数的其中一个，而不是用-XX:+DisableExplicitGC的话，就避开了由full GC带来的长GC pause，同时NIO direct memory的OOM也不会那么容易发生。
做了个跟第一个例子类似的例子，在这里：[https://gist.github.com/1344251](https://gist.github.com/1344251)
《Java Performance》的303页有讲到这俩参数。
相关bug：[6919638 CMS: ExplicitGCInvokesConcurrent misinteracts with gc locker](http://bugs.sun.com/view_bug.do?bug_id=6919638)
<< JDK6u23修复了这个问题
======================================================================
4、-XX:+GCLockerInvokesConcurrent
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. product(bool, GCLockerInvokesConcurrent, false,        \  
1.   "The exit of a JNI CS necessitating a scavenge also" \  
1.   " kicks off a bkgrd concurrent collection")          \  
product(bool, GCLockerInvokesConcurrent, false, \ "The exit of a JNI CS necessitating a scavenge also" \ " kicks off a bkgrd concurrent collection") \
（内容回头补…）
======================================================================
5、MaxDirectMemorySize 与 NIO direct memory 的默认上限
**-XX:MaxDirectMemorySize** 是用来配置NIO direct memory上限用的VM参数。
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. product(intx, MaxDirectMemorySize, -1,                         \  
1.         "Maximum total size of NIO direct-buffer allocations") \  
product(intx, MaxDirectMemorySize, -1, \ "Maximum total size of NIO direct-buffer allocations") \
但如果不配置它的话，direct memory默认最多能申请多少内存呢？这个参数默认值是-1，显然不是一个“有效值”。所以真正的默认值肯定是从别的地方来的。
在Sun JDK 6和OpenJDK 6里，有这样一段代码，sun.misc.VM：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. // A user-settable upper limit on the maximum amount of allocatable direct  
1. // buffer memory.  This value may be changed during VM initialization if  
1. // "java" is launched with "-XX:MaxDirectMemorySize=<size>".  
1. //  
1. // The initial value of this field is arbitrary; during JRE initialization  
1. // it will be reset to the value specified on the command line, if any,  
1. // otherwise to Runtime.getRuntime().maxMemory().  
1. //  
1. private static long directMemory = 64 * 1024 * 1024;  
1.   
1. // If this method is invoked during VM initialization, it initializes the  
1. // maximum amount of allocatable direct buffer memory (in bytes) from the  
1. // system property sun.nio.MaxDirectMemorySize.  The system property will  
1. // be removed when it is accessed.  
1. //  
1. // If this method is invoked after the VM is booted, it returns the  
1. // maximum amount of allocatable direct buffer memory.  
1. //  
1. public static long maxDirectMemory() {  
1.     if (booted)  
1.         return directMemory;  
1.   
1.     Properties p = System.getProperties();  
1.     String s = (String)p.remove("sun.nio.MaxDirectMemorySize");  
1.     System.setProperties(p);  
1.   
1.     if (s != null) {  
1.         if (s.equals("-1")) {  
1.             // -XX:MaxDirectMemorySize not given, take default  
1.             directMemory = Runtime.getRuntime().maxMemory();  
1.         } else {  
1.             long l = Long.parseLong(s);  
1.             if (l > -1)  
1.                 directMemory = l;  
1.         }  
1.     }  
1.   
1.     return directMemory;  
1. }  
// A user-settable upper limit on the maximum amount of allocatable direct // buffer memory. This value may be changed during VM initialization if // "java" is launched with "-XX:MaxDirectMemorySize=<size>". // // The initial value of this field is arbitrary; during JRE initialization // it will be reset to the value specified on the command line, if any, // otherwise to Runtime.getRuntime().maxMemory(). // private static long directMemory = 64 * 1024 * 1024; // If this method is invoked during VM initialization, it initializes the // maximum amount of allocatable direct buffer memory (in bytes) from the // system property sun.nio.MaxDirectMemorySize. The system property will // be removed when it is accessed. // // If this method is invoked after the VM is booted, it returns the // maximum amount of allocatable direct buffer memory. // public static long maxDirectMemory() { if (booted) return directMemory; Properties p = System.getProperties(); String s = (String)p.remove("sun.nio.MaxDirectMemorySize"); System.setProperties(p); if (s != null) { if (s.equals("-1")) { // -XX:MaxDirectMemorySize not given, take default directMemory = Runtime.getRuntime().maxMemory(); } else { long l = Long.parseLong(s); if (l > -1) directMemory = l; } } return directMemory; }
（代码里原本的注释有个写错的地方，上面有修正）
当MaxDirectMemorySize参数没被显式设置时它的值就是-1，在Java类库初始化时maxDirectMemory()被java.lang.System的静态构造器调用，走的路径就是这条：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. if (s.equals("-1")) {  
1.     // -XX:MaxDirectMemorySize not given, take default  
1.     directMemory = Runtime.getRuntime().maxMemory();  
1. }  
if (s.equals("-1")) { // -XX:MaxDirectMemorySize not given, take default directMemory = Runtime.getRuntime().maxMemory(); }
而Runtime.maxMemory()在HotSpot VM里的实现是：
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. JVM_ENTRY_NO_ENV(jlong, JVM_MaxMemory(void))  
1.   JVMWrapper("JVM_MaxMemory");  
1.   size_t n = Universe::heap()->max_capacity();  
1.   return convert_size_t_to_jlong(n);  
1. JVM_END  
JVM_ENTRY_NO_ENV(jlong, JVM_MaxMemory(void)) JVMWrapper("JVM_MaxMemory"); size_t n = Universe::heap()->max_capacity(); return convert_size_t_to_jlong(n); JVM_END
这个max_capacity()实际返回的是 -Xmx减去一个survivor space的预留大小（G1除外）。
结论：MaxDirectMemorySize没显式配置的时候，NIO direct memory可申请的空间的上限就是**-Xmx减去一个survivor space的预留大小**。
大家感兴趣的话可以试试在不同的-Xmx的条件下不设置MaxDirectMemorySize，并且调用一下sun.misc.VM.maxDirectMemory()看得到的值的相关性。
该行为在JDK7里没变，虽然具体实现的代码有些变化。请参考[http://hg.openjdk.java.net/jdk7/jdk7/jdk/rev/b444f86c4abe](http://hg.openjdk.java.net/jdk7/jdk7/jdk/rev/b444f86c4abe)
======================================================================
6、-verbose:gc 与 -XX:+PrintGCDetails
经常能看到在推荐的标准参数里这两个参数一起出现。实际上它们有啥关系？
在Oracle/Sun JDK 6里，"java"这个启动程序遇到"-verbosegc"会将其转换为"-verbose:gc"，将启动参数传给HotSpot VM后，HotSpot VM遇到"-verbose:gc"则会当作"-XX:+PrintGC"来处理。
也就是说 -verbosegc、-verbose:gc、-XX:+PrintGC 三者的作用是完全一样的。
而当HotSpot VM遇到 -XX:+PrintGCDetails 参数时，会顺带把 -XX:+PrintGC 给设置上。
也就是说 -XX:+PrintGCDetails 包含 -XX:+PrintGC，进而也就包含 -verbose:gc。
既然 -verbose:gc 都被包含了，何必在命令行参数里显式设置它呢？![]()
======================================================================
7、-XX:+UseFastEmptyMethods 与 -XX:+UseFastAccessorMethods
虽然不常见，但偶尔也会见到推荐的标准参数上有这俩的身影。
empty method顾名思义就是空方法，也就是方法体只包含一条return指令、返回值类型为void的Java方法。
accessor method在这里则有很具体的定义：
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. bool methodOopDesc::is_accessor() const {  
1.   if (code_size() != 5) return false;  
1.   if (size_of_parameters() != 1) return false;  
1.   if (java_code_at(0) != Bytecodes::_aload_0 ) return false;  
1.   if (java_code_at(1) != Bytecodes::_getfield) return false;  
1.   if (java_code_at(4) != Bytecodes::_areturn &&  
1.       java_code_at(4) != Bytecodes::_ireturn ) return false;  
1.   return true;  
1. }  
bool methodOopDesc::is_accessor() const { if (code_size() != 5) return false; if (size_of_parameters() != 1) return false; if (java_code_at(0) != Bytecodes::_aload_0 ) return false; if (java_code_at(1) != Bytecodes::_getfield) return false; if (java_code_at(4) != Bytecodes::_areturn && java_code_at(4) != Bytecodes::_ireturn ) return false; return true; }
如果从Java源码的角度来理解，accessor method就是形如这样的：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. public class Foo {  
1.   private int value;  
1.     
1.   public int getValue() {  
1.     return this.value;  
1.   }  
1. }  
public class Foo { private int value; public int getValue() { return this.value; } }
关键点是：
1、必须是成员方法；静态方法不行
2、返回值类型必须是引用类型或者int，其它都不算
3、方法体的代码必须满足aload_0; getfield #index; areturn或ireturn这样的模式。
留意：方法名是什么都没关系，是不是get、is、has开头都不重要。
那么这俩有啥问题？
取自JDK 6 update 27：
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. product(bool, UseFastEmptyMethods, true,                   \  
1.         "Use fast method entry code for empty methods")    \  
1.                                                            \  
1. product(bool, UseFastAccessorMethods, true,                \  
1.         "Use fast method entry code for accessor methods") \  
product(bool, UseFastEmptyMethods, true, \ "Use fast method entry code for empty methods") \ \ product(bool, UseFastAccessorMethods, true, \ "Use fast method entry code for accessor methods") \
看到这俩参数的默认值都是true了么？也就是说，在Oracle/Sun JDK 6上设置这参数其实也是没意义的，跟默认一样，一直到最新的JDK 6 update 29都是如此。
不过在Oracle/Sun JDK 7里，情况有变化。
[Bug ID: 6385687 UseFastEmptyMethods/UseFastAccessorMethods considered harmful](http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=6385687)
在[上述bug对应的代码变更](http://hg.openjdk.java.net/jdk7/hotspot-comp/hotspot/rev/c2323e2ea62b)后，这俩参数的默认值改为了false。
本来想多写点这块的…算，还是长话短说。
Oracle JDK 7里的HotSpot VM已经开始有比较好的[多层编译（tiered compilation）支持](http://rednaxelafx.iteye.com/blog/1022095)，可以预见在不久的将来该模式将成为HotSpot VM默认的执行模式。当前该模式尚未默认开启；可以通过 **-XX:+TieredCompilation** 来开启。
有趣的是，在使用多层编译模式时，如果UseFastAccessorMethods/UseFastEmptyMethods是开着的，有些多态方法调用点的性能反而会显著下降。所以，为了适应多层编译模式，JDK 7里这两个参数的默认值就被改为false了。
在邮件列表上有过相关讨论：[review for 6385687: UseFastEmptyMethods/UseFastAccessorMethods considered harmful](http://mail.openjdk.java.net/pipermail/hotspot-compiler-dev/2011-March/005057.html)
======================================================================
8、-XX:+UseCMSCompactAtFullCollection
这个参数在Oracle/Sun JDK 6里一直都默认是true，完全没必要显式设置，设了也不会有啥不同的效果。
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. product(bool, UseCMSCompactAtFullCollection, true,    \  
1.         "Use mark sweep compact at full collections") \  
product(bool, UseCMSCompactAtFullCollection, true, \ "Use mark sweep compact at full collections") \
我不认为显式设置一个跟默认值相同的参数有什么维护上的好处。要维护的参数多了反而更容易成为维护的噩梦吧。后面的人会不知道到底当初为什么要设置这个参数。
相关的有个 **CMSFullGCsBeforeCompaction** 参数，请参考另一帖里的讨论：[http://hllvm.group.iteye.com/group/topic/28854#209294](http://hllvm.group.iteye.com/group/topic/28854#209294)
同样，在Oracle/Sun JDK 6和OpenJDK 6里，**CMSParallelRemarkEnabled** 也一直默认是true，没必要显式设置-XX:+CMSParallelRemarkEnabled。
有很多bool类型的参数默认都是true，显式设置它们之前最好先用这帖开头介绍的办法看看默认值是否已经是想要的值了。
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. product(bool, CMSScavengeBeforeRemark, false,          \  
1.         "Attempt scavenge before the CMS remark step") \  
product(bool, CMSScavengeBeforeRemark, false, \ "Attempt scavenge before the CMS remark step") \
这个默认倒是false。如果一个应用统计到的young GC时间都比较短而CMS remark的时间比较长，那么可以试试打开这个参数，在做remark之前先做一次young GC。是否能有效缩短remark的时间视应用情况而异，所以开这个参数的话请一定做好测试。
======================================================================
9、-XX:CMSMaxAbortablePrecleanTime=5000
同上…默认就是5000
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. product(intx, CMSMaxAbortablePrecleanTime, 5000,    \  
1.         "(Temporary, subject to experimentation)"   \  
1.         "Maximum time in abortable preclean in ms") \  
product(intx, CMSMaxAbortablePrecleanTime, 5000, \ "(Temporary, subject to experimentation)" \ "Maximum time in abortable preclean in ms") \
还是不要设跟默认值一样的参数了吧。
======================================================================
10、-Xss 与 -XX:ThreadStackSize
参考我之前发过的两帖：
[What the difference between -Xss and -XX:ThreadStackSize is?](http://mail.openjdk.java.net/pipermail/hotspot-dev/2011-June/004272.html)
[Inconsistency between -Xss and -XX:ThreadStackSize in the java launcher](http://mail.openjdk.java.net/pipermail/hotspot-dev/2011-July/004288.html)
（详情回头补～）
======================================================================
11、-Xmn 与 -XX:NewSize、-XX:MaxNewSize
如果同时设置了-XX:NewSize与-XX:MaxNewSize遇到“Could not reserve enough space for object heap”错误的话，请看看是不是[这帖所说的问题](http://hllvm.group.iteye.com/group/topic/28467#206341)。早期JDK 6似乎都受这问题影响，一直到JDK 6 update 14才修复。
======================================================================
12、-Xmn 与 -XX:NewRatio
======================================================================
13、-XX:NewRatio 与 -XX:NewSize、-XX:OldSize
======================================================================
14、jmap -heap看到的参数值与实际起作用的参数的关系？
发了几个例子在这里：[https://gist.github.com/1363195](https://gist.github.com/1363195)
其中有个看起来很恐怖的值：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. MaxNewSize       = 17592186044415 MB  
MaxNewSize = 17592186044415 MB
这是啥来的？
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. product(uintx, MaxNewSize, max_uintx,                                  \  
1.         "Maximum new generation size (in bytes), max_uintx means set " \  
1.         "ergonomically")     
product(uintx, MaxNewSize, max_uintx, \ "Maximum new generation size (in bytes), max_uintx means set " \ "ergonomically")
在HotSpot VM里，intx是跟平台字长一样宽的带符号整型，uintx是其无符号版。
max_uintx是(uintx) -1，也就是说在32位平台上是无符号的0xFFFFFFFF，64位平台上则是0xFFFFFFFFFFFFFFFF。
jmap -heap显示的部分参数是以MB为单位来显示的，而MaxNewSize的单位是byte。我跑例子的平台是64位的，于是算一下 0xFFFFFFFFFFFFFFFF / 1024 / 1024 = 17592186044415 MB 。
参数的说明告诉我们，当MaxNewSize的值等于max_uintx时，意思就是交由ergonomics来自动选择young gen的最大大小。**并不是说young gen的最大大小真的有0xFFFFFFFFFFFFFFFF这么大**。
要注意的是，HotSpot VM有大量可调节的参数，并不是所有参数在某次运行的时候都有效。
例如说设置了-Xmn的话，NewRatio就没作用了。
又例如说，
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. product(uintx, OldSize, ScaleForWordSize(4*M),        \  
1.         "Initial tenured generation size (in bytes)") \  
product(uintx, OldSize, ScaleForWordSize(4*M), \ "Initial tenured generation size (in bytes)") \
-XX:OldSize参数的默认值在32位平台上是4M，在64位平台上是5M多。但如果这个参数没有被显式设置过，那它实际上是没作用的；old gen的大小会通过Java heap的整体大小与young gen的大小配置计算出来，但OldSize参数却没有被更新（因为根本没用它）。于是这个参数的值与实际运行的状况就可能会不相符。
一种例外的情况是，如果-Xmx非常小，比NewSize+OldSize的默认值还小，那这个OldSize的默认值就会起作用，把MaxHeapSize给撑大。
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. void TwoGenerationCollectorPolicy::initialize_flags() {  
1.   GenCollectorPolicy::initialize_flags();  
1.   
1.   OldSize = align_size_down(OldSize, min_alignment());  
1.   if (NewSize + OldSize > MaxHeapSize) {  
1.     MaxHeapSize = NewSize + OldSize;  
1.   }  
1.   MaxHeapSize = align_size_up(MaxHeapSize, max_alignment());  
1. //...  
1. }  
void TwoGenerationCollectorPolicy::initialize_flags() { GenCollectorPolicy::initialize_flags(); OldSize = align_size_down(OldSize, min_alignment()); if (NewSize + OldSize > MaxHeapSize) { MaxHeapSize = NewSize + OldSize; } MaxHeapSize = align_size_up(MaxHeapSize, max_alignment()); //... }
可以看这边的一个例子：[https://gist.github.com/1375782](https://gist.github.com/1375782)
======================================================================
15、-XX:+AlwaysTenure、-XX:+NeverTenure、-XX:MaxTenuringThreshold=0 或 "-XX:MaxTenuringThreshold=markOopDesc::max_age + 1"
ParNew的时候，设定-XX:+AlwaysTenure隐含-XX:MaxTenuringThreshold=0；不过-XX:+NeverTenure却没啥特别的作用。
-XX:+AlwaysTenure 与 -XX:+NeverTenure 是互斥的，最后一个出现的那个会同时决定这两个参数的值。
======================================================================
16、-XX:MaxTenuringThreshold 的默认值？
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. product(intx, MaxTenuringThreshold,    15, \  
1.   "Maximum value for tenuring threshold")  \  
product(intx, MaxTenuringThreshold, 15, \ "Maximum value for tenuring threshold") \
Oracle/Sun JDK 6中，选择CMS之外的GC时，MaxTenuringThreshold（以下简称MTT）的默认值是15；而选择了CMS的时候，MTT的默认值是4而不是15。设定是在 Arguments::set_cms_and_parnew_gc_flags() 里做的。
在Sun JDK 6之前（1.4.2、5），选择CMS的时候MTT的默认值则是0，也就是等于设定了-XX:+AlwaysTenure——所有eden里的活对象在经历第一次minor GC的时候就会直接晋升到old gen，而survivor space直接就没用了。
Command prompt代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. $ java -version  
1. java version "1.6.0_25"  
1. Java(TM) SE Runtime Environment (build 1.6.0_25-b06)  
1. Java HotSpot(TM) 64-Bit Server VM (build 20.0-b11, mixed mode)  
1. $ java -XX:+PrintFlagsFinal | egrep 'MaxTenuringThreshold|UseParallelGC|UseConcMarkSweepGC'  
1.      intx MaxTenuringThreshold                      = 15              {product}             
1.      bool UseConcMarkSweepGC                        = false           {product}             
1.      bool UseParallelGC                            := true            {product}             
1. $ java -XX:+PrintFlagsFinal -XX:+UseConcMarkSweepGC | egrep 'MaxTenuringThreshold|UseParallelGC|UseConcMarkSweepGC'  
1.      intx MaxTenuringThreshold                     := 4               {product}             
1.      bool UseConcMarkSweepGC                       := true            {product}             
1.      bool UseParallelGC                             = false           {product}             
$ java -version java version "1.6.0_25" Java(TM) SE Runtime Environment (build 1.6.0_25-b06) Java HotSpot(TM) 64-Bit Server VM (build 20.0-b11, mixed mode) $ java -XX:+PrintFlagsFinal | egrep 'MaxTenuringThreshold|UseParallelGC|UseConcMarkSweepGC' intx MaxTenuringThreshold = 15 {product} bool UseConcMarkSweepGC = false {product} bool UseParallelGC := true {product} $ java -XX:+PrintFlagsFinal -XX:+UseConcMarkSweepGC | egrep 'MaxTenuringThreshold|UseParallelGC|UseConcMarkSweepGC' intx MaxTenuringThreshold := 4 {product} bool UseConcMarkSweepGC := true {product} bool UseParallelGC = false {product}
======================================================================
17、-XX:+CMSClassUnloadingEnabled
CMS remark暂停时间会增加，所以如果类加载并不频繁、String的intern也没有大量使用的话，这个参数还是不开的好。
======================================================================
18、-XX:+AggressiveHeap
======================================================================
19、-XX:+UseCompressedOops 有益？有害？
先把微博上回复别人问题的解答放这边。
本来如果功能没bug的话，Oracle/Sun JDK 6的64位HotSpot上，GC堆在26G以下（-Xmx + -XX:MaxPermSize）的时候用多数都是有益的。
开启压缩指针后，从代码路径（code path）和CPI（cycles per instruction）两个角度看，情况是不一样的：
·开启压缩指针会使代码路径变长，因为所有在GC堆里的、指向GC堆内对象的指针都会被压缩，这些指针的访问就需要更多的代码才可以实现。不要以为只是读写字段才受影响，其实实例方法调用、子类型检查等操作也受影响——“klass”也是一个指针，也被压缩了。
·但从CPI的角度看，由于压缩指针使需要拷贝的数据量变小了，cache miss的几率随之降低，结果CPI可能会比压缩前降低。综合来看，开了压缩指针通常能大幅降低GC堆内存的消耗，同时维持或略提高Java程序的速度。
但，JDK6u23之前那个参数的bug实在太多，最好别用；而6u23之后它就由ergonomics自动开启了，不用自己设。如果在6u23或更高版本碰到压缩指针造成的问题的话，显式设置 **-XX:-UseCompressedOops** 。
我能做的建议是如果在64位Oracle/Sun JDK 6/7上，那个参数不要显式设置。
关于HotSpot VM的ergonomics自动开启压缩指针功能，请参考[之前的一帖](http://rednaxelafx.iteye.com/blog/1010079)。
有些库比较“聪明”，会自行读取VM参数来调整自己的一些参数，例如[Berkeley DB Java Edition](http://www.oracle.com/technetwork/database/berkeleydb/overview/index-093405.html)。但这些库实现得不好的时候反而会带来一些麻烦：BDB JE要求**显式指定-XX:+UseCompressedOops**才能有效的调整它的缓存大小。所以在用BDB JE并且Java堆+PermGen大小小于32GB的时候，请显式指定-XX:+UseCompressedOops吧。参考[Warning on Compressed Oops](https://forums.oracle.com/forums/thread.jspa?messageID=10017916)
======================================================================
20、-XX:LargePageSizeInBytes=128m ？
或者是 -XX:LargePageSizeInBytes=256m ？
其实这个参数的值是多少不是问题，问题是这个参数到底有没有起作用。
或许有人读过很老的调优建议资料，例如这个：
[(2005) Java Tuning White Paper - 4.2.3 Tuning Example 3: Try 256 MB pages](http://java.sun.com/performance/reference/whitepapers/tuning.html#section4.2.3)
或者是别的一些内容很老的资料。它们提到了-XX:LargePageSizeInBytes=参数。这些老资料也没说错，在Sun JDK 5里 -XX:LargePageSizeInBytes= 参数只在Solaris上有效，使用的时候没有别的参数保护。
但是，实际上这个参数在Oracle/Sun JDK 6里不配合-XX:+UseLargePages的话是不会起任何作用的。
JDK 6里的JVM的Linux版上初始化large page的地方：
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. bool os::large_page_init() {  
1.   if (!UseLargePages) return false;  
1.   
1.   if (LargePageSizeInBytes) {  
1.     _large_page_size = LargePageSizeInBytes;  
1.   } else {  
1.     // ...  
1.   }  
1.     
1.   // ...  
1.   
1.   // Large page support is available on 2.6 or newer kernel, some vendors  
1.   // (e.g. Redhat) have backported it to their 2.4 based distributions.  
1.   // We optimistically assume the support is available. If later it turns out  
1.   // not true, VM will automatically switch to use regular page size.  
1.   return true;  
1. }  
bool os::large_page_init() { if (!UseLargePages) return false; if (LargePageSizeInBytes) { _large_page_size = LargePageSizeInBytes; } else { // ... } // ... // Large page support is available on 2.6 or newer kernel, some vendors // (e.g. Redhat) have backported it to their 2.4 based distributions. // We optimistically assume the support is available. If later it turns out // not true, VM will automatically switch to use regular page size. return true; }
看到了么，没有将UseLargePages设置为true的话，LargePageSizeInBytes根本没机会被用上。
对应的，Solaris版：
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. bool os::large_page_init() {  
1.   if (!UseLargePages) {  
1.     UseISM = false;  
1.     UseMPSS = false;  
1.     return false;  
1.   }  
1.   
1.   // ...  
1. }  
bool os::large_page_init() { if (!UseLargePages) { UseISM = false; UseMPSS = false; return false; } // ... }
以及Windows版：
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. bool os::large_page_init() {  
1.   if (!UseLargePages) return false;  
1.   
1.   // ...  
1. }  
bool os::large_page_init() { if (!UseLargePages) return false; // ... }
在Oracle/Sun JDK 6以及Oracle JDK 7上要使用 -XX:LargePageSizeInBytes= 的话，请务必也设置上 -XX:+UseLargePages 。使用这两个参数之前最好先确认操作系统是否真的只是large pages；操作系统不支持的话，设置这两个参数也没作用，只会退回到使用regular pages而已。
======================================================================
21、-XX:+AlwaysPreTouch
会把commit的空间跑循环赋值为0以达到“pretouch”的目的。开这个参数会增加VM初始化时的开销，但后面涉及虚拟内存的开销可能降低。
在另一个讨论帖里有讲该参数：[http://hllvm.group.iteye.com/group/topic/28839#209144](http://hllvm.group.iteye.com/group/topic/28839#209144)
======================================================================
22、-XX:+UseTLAB 与 Runtime.freeMemory()
======================================================================
23、-XX:+ParallelRefProcEnabled
这个功能可以加速reference processing，但在JDK6u25和6u26上不要使用，有bug：
[Bug ID 7028845: CMS: 6984287 broke parallel reference processing in CMS](http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=7028845)
======================================================================
24、-XX:+UseConcMarkSweepGC 与 -XX:+UseAdaptiveSizePolicy
这两个选项在现有的Oracle/Sun JDK 6和Oracle JDK 7上都不要搭配在一起使用——CMS用的adaptive size policy还没实现完，用的话可能会crash。
目前HotSpot VM上只有ParallelScavenge系的GC才可以配合-XX:+UseAdaptiveSizePolicy使用；也就是只有-XX:+UseParallelGC或者-XX:+UseParallelOldGC。[Jon Masamitsu在邮件列表上提到过](http://mail.openjdk.java.net/pipermail/hotspot-gc-use/2011-November/000954.html)。
题外话：开着UseAdaptiveSizePolicy的ParallelScavenge会动态调整各空间的大小，有可能会造成两个survivor space的大小被调整得不一样大。Jon Masamitsu在[这封邮件](http://mail.openjdk.java.net/pipermail/hotspot-gc-use/2012-October/001390.html)里解释了原因。
======================================================================
25、-XX:+UseAdaptiveGCBoundary
JDK 6里不要用这个选项，有bug。
======================================================================
26、-XX:HeapDumpPath 与 -XX:+HeapDumpBeforeFullGC、-XX:+HeapDumpAfterFullGC、-XX:+HeapDumpOnOutOfMemoryError
-XX:+HeapDumpBeforeFullGC、-XX:+HeapDumpAfterFullGC、-XX:+HeapDumpOnOutOfMemoryError 这几个参数可以在不同条件下做出HPROF格式的heap dump。但很多人都会疑惑：做出来的heap dump存到哪里去了？
如果不想费神去摸索到底各种环境被配置成什么样、“working directory”到底在哪里的话，就在VM启动参数里加上 **-XX:HeapDumpPath=一个绝对路径** 吧。这样，自动做出的heap dump就会被存到指定的目录里去。
当然相对路径也支持，不过用了相对路径就又得弄清楚当前的“working directory”在哪里了。
======================================================================
26、UseDepthFirstScavengeOrder
以前有过这样一个参数可以设置young gen遍历对象图的顺序，深度还是广度优先不过高于JDK 6 update 22就没用了，ParallelScavenge变为只用深度优先而不用广度优先。
具体的changeset在这里：[http://hg.openjdk.java.net/hsx/hotspot-main/hotspot/rev/9d7a8ab3736b](http://hg.openjdk.java.net/hsx/hotspot-main/hotspot/rev/9d7a8ab3736b)
HotSpot VM里的arguments.cpp文件里有obsolete_jvm_flags数组，那边声明的参数都要留意是已经没用的。
（下文待续～）
![Spinner]() [![learnworld的博客]( "learnworld的博客: 一路远行")](http://learnworld.iteye.com/) [learnworld](http://learnworld.iteye.com/ "learnworld") 2011-10-23

好贴，坐等下文。
![Spinner]() [![jolestar的博客]( "jolestar的博客: 午夜咖啡")](http://jolestar.iteye.com/) [jolestar](http://jolestar.iteye.com/ "jolestar") 2011-10-23

lz不厚道，说了个半句话就闪了。太吊胃口了。
![Spinner]() [![boy00fly的博客]( "boy00fly的博客: 石头边的老牛")](http://boy00fly.iteye.com/) [boy00fly](http://boy00fly.iteye.com/ "boy00fly") 2011-10-24

jolestar 写道

lz不厚道，说了个半句话就闪了。太吊胃口了。
哈哈，确实吊胃口。。下文呢？
![Spinner]() [![icanfly的博客]( "icanfly的博客: 老罗宣言")](http://icanfly.iteye.com/) [icanfly](http://icanfly.iteye.com/ "icanfly") 2011-10-24

好文。![]()
![Spinner]() [![liuyes的博客]( "liuyes的博客: ")](http://liuyes.iteye.com/) [liuyes](http://liuyes.iteye.com/ "liuyes") 2011-10-24

楼主人呢？这发文的也有坑![]()
![Spinner]() [![khotyn的博客]( "khotyn的博客: 码工作坊")](http://khotyn.iteye.com/) [khotyn](http://khotyn.iteye.com/ "khotyn") 2011-10-24

坐等R大把坑填上～～
![Spinner]() [![程序新手的博客]( "程序新手的博客: HappyProgramme")](http://furturestrategist.iteye.com/) [程序新手](http://furturestrategist.iteye.com/ "程序新手") 2011-10-24

内容受益、风格简洁，期待下文
  另外大胆猜测从日志中获取的信息是部分FULL GC由System.gc()引发的
  Full GC (System)
![Spinner]() [![程序新手的博客]( "程序新手的博客: HappyProgramme")](http://furturestrategist.iteye.com/) [程序新手](http://furturestrategist.iteye.com/ "程序新手") 2011-10-25

又受益了 ![]() ..想了解大大收集这些知识点的方法~
![Spinner]() [![hittyt的博客]( "hittyt的博客: 笨小孩")](http://hittyt.iteye.com/) [hittyt](http://hittyt.iteye.com/ "hittyt") 2011-11-08

本人的java环境如下：
Command line代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. java -version  
1. java version "1.6.0_18"  
1. Java(TM) SE Runtime Environment (build 1.6.0_18-b07)  
1. Java HotSpot(TM) 64-Bit Server VM (build 16.0-b13, mixed mode)  
java -version java version "1.6.0_18" Java(TM) SE Runtime Environment (build 1.6.0_18-b07) Java HotSpot(TM) 64-Bit Server VM (build 16.0-b13, mixed mode)
但使用PrintFlagsFinal时得到的输出却是下面的结果：
Command line代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. java -XX:+PrintFlagsFinal  
1. Unrecognized VM option '+PrintFlagsFinal'  
1. Could not create the Java virtual machine.  
java -XX:+PrintFlagsFinal Unrecognized VM option '+PrintFlagsFinal' Could not create the Java virtual machine.
求LZ解答问题在哪里呢？
![Spinner]()

![]() [发表回复](http://hllvm.group.iteye.com/group/topic/27945/post/new)

« 上一页 1 [2](http://hllvm.group.iteye.com/group/topic/27945?page=2) [3](http://hllvm.group.iteye.com/group/topic/27945?page=3) [下一页 »](http://hllvm.group.iteye.com/group/topic/27945?page=2)

[>>返回群组首页](http://hllvm.group.iteye.com/)

[![高级语言虚拟机群组]( "高级语言虚拟机: 关注各种高级语言虚拟机（high-level language virtual machine，HLL VM）的设计与实现，泛化至各种高级语言的运行时的设计与实现。讨论范围包括JVM、CLI、Parrot等当前流行的VM平台，也包括Python、Ruby、JavaScript、Lua、Perl、Forth、Smalltalk等众多语言的引擎，还有历史上有影响的各种高级语言虚拟机，如SECD等。")](http://hllvm.group.iteye.com/)

### 相关讨论

* [一次Java垃圾收集调优实战](http://www.iteye.com/topic/212967)
* [HotSpot VM 内存堆的两个Servivor区](http://www.iteye.com/topic/894148)
* [JVM内存管理：深入垃圾收集器与内存分配策略](http://www.iteye.com/topic/802638)
* [CMS gc实践总结](http://www.iteye.com/topic/473874)
* [优化JVM参数提高eclipse运行速度](http://www.iteye.com/topic/756538)
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
百联优力(北京)投资有限公司 版权所有 ![](http://stat.iteye.com/?url=http%3A%2F%2Fhllvm.group.iteye.com%2Fgroup%2Ftopic%2F27945&referrer=http%3A%2F%2Fhllvm.group.iteye.com%2F&user_id=)
