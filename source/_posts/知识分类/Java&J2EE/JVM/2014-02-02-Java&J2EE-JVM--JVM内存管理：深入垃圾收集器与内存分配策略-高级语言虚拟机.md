---
layout: post
title: "JVM内存管理：深入垃圾收集器与内存分配策略 - 高级语言虚拟机"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# JVM内存管理：深入垃圾收集器与内存分配策略 - 高级语言虚拟机

[您还未登录 !](http://hllvm.group.iteye.com/login "登录") [登录](http://hllvm.group.iteye.com/login) [注册](http://hllvm.group.iteye.com/signup)

[![ITeye3.0]( "ITeye-最棒的软件开发交流社区")](http://www.iteye.com/)

[]()

[群组首页](http://www.iteye.com/groups) → [编程语言](http://hllvm.group.iteye.com/groups/category/language) → [高级语言虚拟机](http://hllvm.group.iteye.com/) → [知识库](http://hllvm.group.iteye.com/group/wiki) → [JVM基础](http://hllvm.group.iteye.com/group/wiki?category_id=316) → [JVM内存管理：深入垃圾收集器与内存分配策略]()
原创作者: [IcyFenix](http://www.javaeye.com/topic/802638)   阅读:6552次   评论:6条   更新时间:2011-05-26    

Java与C++之间有一堵由内存动态分配和垃圾收集技术所围成的高墙，墙外面的人想进去，墙里面的人却想出来。
**概述：**
说起垃圾收集（Garbage Collection，下文简称GC），大部分人都把这项技术当做Java语言的伴生产物。事实上GC的历史远远比Java来得久远，在1960年诞生于MIT的Lisp是第一门真正使用内存动态分配和垃圾收集技术的语言。当Lisp还在胚胎时期，人们就在思考GC需要完成的3件事情：哪些内存需要回收？什么时候回收？怎么样回收？
经过半个世纪的发展，目前的内存分配策略与垃圾回收技术已经相当成熟，一切看起来都进入“自动化”的时代，那为什么我们还要去了解GC和内存分配？答案很简单：当需要排查各种内存溢出、泄漏问题时，当垃圾收集成为系统达到更高并发量的瓶颈时，我们就需要对这些“自动化”的技术有必要的监控、调节手段。
把时间从1960年拨回现在，回到我们熟悉的Java语言。本文第一章中介绍了Java内存运行时区域的各个部分，其中程序计数器、VM栈、本地方法栈三个区域随线程而生，随线程而灭；栈中的帧随着方法进入、退出而有条不紊的进行着出栈入栈操作；每一个帧中分配多少内存基本上是在Class文件生成时就已知的（可能会由JIT动态晚期编译进行一些优化，但大体上可以认为是编译期可知的），因此这几个区域的内存分配和回收具备很高的确定性，因此在这几个区域不需要过多考虑回收的问题。而Java堆和方法区（包括运行时常量池）则不一样，我们必须等到程序实际运行期间才能知道会创建哪些对象，这部分内存的分配和回收都是动态的，我们本文后续讨论中的“内存”分配与回收仅仅指这一部分内存。
**对象已死？**
在堆里面存放着Java世界中几乎所有的对象，在回收前首先要确定这些对象之中哪些还在存活，哪些已经“死去”了，即不可能再被任何途径使用的对象。
引用计数算法（Reference Counting）
最初的想法，也是很多教科书判断对象是否存活的算法是这样的：给对象中添加一个引用计数器，当有一个地方引用它，计数器加1，当引用失效，计数器减1，任何时刻计数器为0的对象就是不可能再被使用的。
客观的说，引用计数算法实现简单，判定效率很高，在大部分情况下它都是一个不错的算法，但引用计数算法无法解决对象循环引用的问题。举个简单的例子：对象A和B分别有字段b、a，令A.b=B和B.a=A，除此之外这2个对象再无任何引用，那实际上这2个对象已经不可能再被访问，但是引用计数算法却无法回收他们。
根搜索算法（GC Roots Tracing）
在实际生产的语言中（Java、C#、甚至包括前面提到的Lisp），都是使用根搜索算法判定对象是否存活。算法基本思路就是通过一系列的称为“GC Roots”的点作为起始进行向下搜索，当一个对象到GC Roots没有任何引用链（Reference Chain）相连，则证明此对象是不可用的。在Java语言中，GC Roots包括：
1.在VM栈（帧中的本地变量）中的引用
2.方法区中的静态引用
3.JNI（即一般说的Native方法）中的引用
生存还是死亡？
判定一个对象死亡，至少经历两次标记过程：如果对象在进行根搜索后，发现没有与GC Roots相连接的引用链，那它将会被第一次标记，并在稍后执行他的finalize()方法（如果它有的话）。这里所谓的“执行”是指虚拟机会触发这个方法，但并不承诺会等待它运行结束。这点是必须的，否则一个对象在finalize()方法执行缓慢，甚至有死循环什么的将会很容易导致整个系统崩溃。finalize()方法是对象最后一次逃脱死亡命运的机会，稍后GC将进行第二次规模稍小的标记，如果在finalize()中对象成功拯救自己（只要重新建立到GC Roots的连接即可，譬如把自己赋值到某个引用上），那在第二次标记时它将被移除出“即将回收”的集合，如果对象这时候还没有逃脱，那基本上它就真的离死不远了。
需要特别说明的是，这里对finalize()方法的描述可能带点悲情的艺术加工，并不代表笔者鼓励大家去使用这个方法来拯救对象。相反，笔者建议大家尽量避免使用它，这个不是C/C++里面的析构函数，它运行代价高昂，不确定性大，无法保证各个对象的调用顺序。需要关闭外部资源之类的事情，基本上它能做的使用try-finally可以做的更好。
关于方法区
方法区即后文提到的永久代，很多人认为永久代是没有GC的，《Java虚拟机规范》中确实说过可以不要求虚拟机在这区实现GC，而且这区GC的“性价比”一般比较低：在堆中，尤其是在新生代，常规应用进行一次GC可以一般可以回收70%~95%的空间，而永久代的GC效率远小于此。虽然VM Spec不要求，但当前生产中的商业JVM都有实现永久代的GC，主要回收两部分内容：废弃常量与无用类。这两点回收思想与Java堆中的对象回收很类似，都是搜索是否存在引用，常量的相对很简单，与对象类似的判定即可。而类的回收则比较苛刻，需要满足下面3个条件：
1.该类所有的实例都已经被GC，也就是JVM中不存在该Class的任何实例。
2.加载该类的ClassLoader已经被GC。
3.该类对应的java.lang.Class 对象没有在任何地方被引用，如不能在任何地方通过反射访问该类的方法。
是否对类进行回收可使用-XX:+ClassUnloading参数进行控制，还可以使用-verbose:class或者-XX:+TraceClassLoading、-XX:+TraceClassUnLoading查看类加载、卸载信息。
在大量使用反射、动态代理、CGLib等bytecode框架、动态生成JSP以及OSGi这类频繁自定义ClassLoader的场景都需要JVM具备类卸载的支持以保证永久代不会溢出。
**垃圾收集算法**
在这节里不打算大量讨论算法实现，只是简单的介绍一下基本思想以及发展过程。最基础的搜集算法是“标记－清除算法”（Mark-Sweep），如它的名字一样，算法分层“标记”和“清除”两个阶段，首先标记出所有需要回收的对象，然后回收所有需要回收的对象，整个过程其实前一节讲对象标记判定的时候已经基本介绍完了。说它是最基础的收集算法原因是后续的收集算法都是基于这种思路并优化其缺点得到的。它的主要缺点有两个，一是效率问题，标记和清理两个过程效率都不高，二是空间问题，标记清理之后会产生大量不连续的内存碎片，空间碎片太多可能会导致后续使用中无法找到足够的连续内存而提前触发另一次的垃圾搜集动作。
为了解决效率问题，一种称为“复制”（Copying）的搜集算法出现，它将可用内存划分为两块，每次只使用其中的一块，当半区内存用完了，仅将还存活的对象复制到另外一块上面，然后就把原来整块内存空间一次过清理掉。这样使得每次内存回收都是对整个半区的回收，内存分配时也就不用考虑内存碎片等复杂情况，只要移动堆顶指针，按顺序分配内存就可以了，实现简单，运行高效。只是这种算法的代价是将内存缩小为原来的一半，未免太高了一点。
现在的商业虚拟机中都是用了这一种收集算法来回收新生代，IBM有专门研究表明新生代中的对象98%是朝生夕死的，所以并不需要按照1：1的比例来划分内存空间，而是将内存分为一块较大的eden空间和2块较少的survivor空间，每次使用eden和其中一块survivor，当回收时将eden和survivor还存活的对象一次过拷贝到另外一块survivor空间上，然后清理掉eden和用过的survivor。Sun Hotspot虚拟机默认eden和survivor的大小比例是8:1，也就是每次只有10%的内存是“浪费”的。当然，98%的对象可回收只是一般场景下的数据，我们没有办法保证每次回收都只有10%以内的对象存活，当survivor空间不够用时，需要依赖其他内存（譬如老年代）进行分配担保（Handle Promotion）。
复制收集算法在对象存活率高的时候，效率有所下降。更关键的是，如果不想浪费50%的空间，就需要有额外的空间进行分配担保用于应付半区内存中所有对象都100%存活的极端情况，所以在老年代一般不能直接选用这种算法。因此人们提出另外一种“标记－整理”（Mark-Compact）算法，标记过程仍然一样，但后续步骤不是进行直接清理，而是令所有存活的对象一端移动，然后直接清理掉这端边界以外的内存。
当前商业虚拟机的垃圾收集都是采用“分代收集”（Generational Collecting）算法，这种算法并没有什么新的思想出现，只是根据对象不同的存活周期将内存划分为几块。一般是把Java堆分作新生代和老年代，这样就可以根据各个年代的特点采用最适当的收集算法，譬如新生代每次GC都有大批对象死去，只有少量存活，那就选用复制算法只需要付出少量存活对象的复制成本就可以完成收集。
**垃圾收集器**
垃圾收集器就是收集算法的具体实现，不同的虚拟机会提供不同的垃圾收集器。并且提供参数供用户根据自己的应用特点和要求组合各个年代所使用的收集器。本文讨论的收集器基于Sun Hotspot虚拟机1.6版。
图1.Sun JVM1.6的垃圾收集器
![]()
图1展示了1.6中提供的6种作用于不同年代的收集器，两个收集器之间存在连线的话就说明它们可以搭配使用。在介绍着些收集器之前，我们先明确一个观点：没有最好的收集器，也没有万能的收集器，只有最合适的收集器。
1.Serial收集器
单线程收集器，收集时会暂停所有工作线程（我们将这件事情称之为Stop The World，下称STW），使用复制收集算法，虚拟机运行在Client模式时的默认新生代收集器。
2.ParNew收集器
ParNew收集器就是Serial的多线程版本，除了使用多条收集线程外，其余行为包括算法、STW、对象分配规则、回收策略等都与Serial收集器一摸一样。对应的这种收集器是虚拟机运行在Server模式的默认新生代收集器，在单CPU的环境中，ParNew收集器并不会比Serial收集器有更好的效果。
3.Parallel Scavenge收集器
Parallel Scavenge收集器（下称PS收集器）也是一个多线程收集器，也是使用复制算法，但它的对象分配规则与回收策略都与ParNew收集器有所不同，它是以吞吐量最大化（即GC时间占总运行时间最小）为目标的收集器实现，它允许较长时间的STW换取总吞吐量最大化。
4.Serial Old收集器
Serial Old是单线程收集器，使用标记－整理算法，是老年代的收集器，上面三种都是使用在新生代收集器。
5.Parallel Old收集器
老年代版本吞吐量优先收集器，使用多线程和标记－整理算法，JVM 1.6提供，在此之前，新生代使用了PS收集器的话，老年代除Serial Old外别无选择，因为PS无法与CMS收集器配合工作。
6.CMS（Concurrent Mark Sweep）收集器
CMS是一种以最短停顿时间为目标的收集器，使用CMS并不能达到GC效率最高（总体GC时间最小），但它能尽可能降低GC时服务的停顿时间，这一点对于实时或者高交互性应用（譬如证券交易）来说至关重要，这类应用对于长时间STW一般是不可容忍的。CMS收集器使用的是标记－清除算法，也就是说它在运行期间会产生空间碎片，所以虚拟机提供了参数开启CMS收集结束后再进行一次内存压缩。
内存分配与回收策略
了解GC其中很重要一点就是了解JVM的内存分配策略：即对象在哪里分配和对象什么时候回收。
关于对象在哪里分配，往大方向讲，主要就在堆上分配，但也可能经过JIT进行逃逸分析后进行标量替换拆散为原子类型在栈上分配，也可能分配在DirectMemory中（详见本文第一章）。往细节处讲，对象主要分配在新生代eden上，也可能会直接老年代中，分配的细节决定于当前使用的垃圾收集器类型与VM相关参数设置。我们可以通过下面代码来验证一下Serial收集器（ParNew收集器的规则与之完全一致）的内存分配和回收的策略。读者看完Serial收集器的分析后，不妨自己根据JVM参数文档写一些程序去实践一下其它几种收集器的分配策略。
清单1：内存分配测试代码
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public class YoungGenGC {  
1.   
1.     private static final int _1MB = 1024 * 1024;  
1.   
1.     public static void main(String[] args) {  
1.         // testAllocation();  
1.         testHandlePromotion();  
1.         // testPretenureSizeThreshold();  
1.         // testTenuringThreshold();  
1.         // testTenuringThreshold2();  
1.     }  
1.   
1.     /** 
1.      * VM参数：-verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:+PrintGCDetails -XX:SurvivorRatio=8 
1.      */  
1.     @SuppressWarnings("unused")  
1.     public static void testAllocation() {  
1.         byte[] allocation1, allocation2, allocation3, allocation4;  
1.         allocation1 = new byte[2 * _1MB];  
1.         allocation2 = new byte[2 * _1MB];  
1.         allocation3 = new byte[2 * _1MB];  
1.         allocation4 = new byte[4 * _1MB];  // 出现一次Minor GC  
1.     }  
1.   
1.     /** 
1.      * VM参数：-verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:+PrintGCDetails -XX:SurvivorRatio=8 
1.      * -XX:PretenureSizeThreshold=3145728 
1.      */  
1.     @SuppressWarnings("unused")  
1.     public static void testPretenureSizeThreshold() {  
1.         byte[] allocation;  
1.         allocation = new byte[4 * _1MB];  //直接分配在老年代中  
1.     }  
1.   
1.     /** 
1.      * VM参数：-verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:+PrintGCDetails -XX:SurvivorRatio=8 -XX:MaxTenuringThreshold=1 
1.      * -XX:+PrintTenuringDistribution 
1.      */  
1.     @SuppressWarnings("unused")  
1.     public static void testTenuringThreshold() {  
1.         byte[] allocation1, allocation2, allocation3;  
1.         allocation1 = new byte[_1MB / 4];  // 什么时候进入老年代决定于XX:MaxTenuringThreshold设置  
1.         allocation2 = new byte[4 * _1MB];  
1.         allocation3 = new byte[4 * _1MB];  
1.         allocation3 = null;  
1.         allocation3 = new byte[4 * _1MB];  
1.     }  
1.   
1.     /** 
1.      * VM参数：-verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:+PrintGCDetails -XX:SurvivorRatio=8 -XX:MaxTenuringThreshold=15 
1.      * -XX:+PrintTenuringDistribution 
1.      */  
1.     @SuppressWarnings("unused")  
1.     public static void testTenuringThreshold2() {  
1.         byte[] allocation1, allocation2, allocation3, allocation4;  
1.         allocation1 = new byte[_1MB / 4];   // allocation1+allocation2大于survivo空间一半  
1.         allocation2 = new byte[_1MB / 4];    
1.         allocation3 = new byte[4 * _1MB];  
1.         allocation4 = new byte[4 * _1MB];  
1.         allocation4 = null;  
1.         allocation4 = new byte[4 * _1MB];  
1.     }  
1.   
1.     /** 
1.      * VM参数：-verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:+PrintGCDetails -XX:SurvivorRatio=8 -XX:-HandlePromotionFailure 
1.      */  
1.     @SuppressWarnings("unused")  
1.     public static void testHandlePromotion() {  
1.         byte[] allocation1, allocation2, allocation3, allocation4, allocation5, allocation6, allocation7;  
1.         allocation1 = new byte[2 * _1MB];  
1.         allocation2 = new byte[2 * _1MB];  
1.         allocation3 = new byte[2 * _1MB];  
1.         allocation1 = null;  
1.         allocation4 = new byte[2 * _1MB];  
1.         allocation5 = new byte[2 * _1MB];  
1.         allocation6 = new byte[2 * _1MB];  
1.         allocation4 = null;  
1.         allocation5 = null;  
1.         allocation6 = null;  
1.         allocation7 = new byte[2 * _1MB];  
1.     }  
1. }  
public class YoungGenGC { private static final int _1MB = 1024 * 1024; public static void main(String[] args) { // testAllocation(); testHandlePromotion(); // testPretenureSizeThreshold(); // testTenuringThreshold(); // testTenuringThreshold2(); } /** * VM参数：-verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:+PrintGCDetails -XX:SurvivorRatio=8 */ @SuppressWarnings("unused") public static void testAllocation() { byte[] allocation1, allocation2, allocation3, allocation4; allocation1 = new byte[2 * _1MB]; allocation2 = new byte[2 * _1MB]; allocation3 = new byte[2 * _1MB]; allocation4 = new byte[4 * _1MB]; // 出现一次Minor GC } /** * VM参数：-verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:+PrintGCDetails -XX:SurvivorRatio=8 * -XX:PretenureSizeThreshold=3145728 */ @SuppressWarnings("unused") public static void testPretenureSizeThreshold() { byte[] allocation; allocation = new byte[4 * _1MB]; //直接分配在老年代中 } /** * VM参数：-verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:+PrintGCDetails -XX:SurvivorRatio=8 -XX:MaxTenuringThreshold=1 * -XX:+PrintTenuringDistribution */ @SuppressWarnings("unused") public static void testTenuringThreshold() { byte[] allocation1, allocation2, allocation3; allocation1 = new byte[_1MB / 4]; // 什么时候进入老年代决定于XX:MaxTenuringThreshold设置 allocation2 = new byte[4 * _1MB]; allocation3 = new byte[4 * _1MB]; allocation3 = null; allocation3 = new byte[4 * _1MB]; } /** * VM参数：-verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:+PrintGCDetails -XX:SurvivorRatio=8 -XX:MaxTenuringThreshold=15 * -XX:+PrintTenuringDistribution */ @SuppressWarnings("unused") public static void testTenuringThreshold2() { byte[] allocation1, allocation2, allocation3, allocation4; allocation1 = new byte[_1MB / 4]; // allocation1+allocation2大于survivo空间一半 allocation2 = new byte[_1MB / 4]; allocation3 = new byte[4 * _1MB]; allocation4 = new byte[4 * _1MB]; allocation4 = null; allocation4 = new byte[4 * _1MB]; } /** * VM参数：-verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:+PrintGCDetails -XX:SurvivorRatio=8 -XX:-HandlePromotionFailure */ @SuppressWarnings("unused") public static void testHandlePromotion() { byte[] allocation1, allocation2, allocation3, allocation4, allocation5, allocation6, allocation7; allocation1 = new byte[2 * _1MB]; allocation2 = new byte[2 * _1MB]; allocation3 = new byte[2 * _1MB]; allocation1 = null; allocation4 = new byte[2 * _1MB]; allocation5 = new byte[2 * _1MB]; allocation6 = new byte[2 * _1MB]; allocation4 = null; allocation5 = null; allocation6 = null; allocation7 = new byte[2 * _1MB]; } }
规则一：通常情况下，对象在eden中分配。当eden无法分配时，触发一次Minor GC。
执行testAllocation()方法后输出了GC日志以及内存分配状况。-Xms20M -Xmx20M -Xmn10M这3个参数确定了Java堆大小为20M，不可扩展，其中10M分配给新生代，剩下的10M即为老年代。-XX:SurvivorRatio=8决定了新生代中eden与survivor的空间比例是1：8，从输出的结果也清晰的看到“eden space 8192K、from space 1024K、to space 1024K”的信息，新生代总可用空间为9216K（eden+1个survivor）。
我们也注意到在执行testAllocation()时出现了一次Minor GC，GC的结果是新生代6651K变为148K，而总占用内存则几乎没有减少（因为几乎没有可回收的对象）。这次GC是发生的原因是为allocation4分配内存的时候，eden已经被占用了6M，剩余空间已不足分配allocation4所需的4M内存，因此发生Minor GC。GC期间虚拟机发现已有的3个2M大小的对象全部无法放入survivor空间（survivor空间只有1M大小），所以直接转移到老年代去。GC后4M的allocation4对象分配在eden中。
清单2：testAllocation()方法输出结果
[GC [DefNew: 6651K->148K(9216K), 0.0070106 secs] 6651K->6292K(19456K), 0.0070426 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
Heap
def new generation   total 9216K, used 4326K [0x029d0000, 0x033d0000, 0x033d0000)
  eden space 8192K,  51% used [0x029d0000, 0x02de4828, 0x031d0000)
  from space 1024K,  14% used [0x032d0000, 0x032f5370, 0x033d0000)
  to   space 1024K,   0% used [0x031d0000, 0x031d0000, 0x032d0000)
tenured generation   total 10240K, used 6144K [0x033d0000, 0x03dd0000, 0x03dd0000)
   the space 10240K,  60% used [0x033d0000, 0x039d0030, 0x039d0200, 0x03dd0000)
compacting perm gen  total 12288K, used 2114K [0x03dd0000, 0x049d0000, 0x07dd0000)
   the space 12288K,  17% used [0x03dd0000, 0x03fe0998, 0x03fe0a00, 0x049d0000)
No shared spaces configured.
规则二：配置了PretenureSizeThreshold的情况下，对象大于设置值将直接在老年代分配。
执行testPretenureSizeThreshold()方法后，我们看到eden空间几乎没有被使用，而老年代的10M控件被使用了40%，也就是4M的allocation对象直接就分配在老年代中，则是因为PretenureSizeThreshold被设置为3M，因此超过3M的对象都会直接从老年代分配。
清单3：
Heap
def new generation   total 9216K, used 671K [0x029d0000, 0x033d0000, 0x033d0000)
  eden space 8192K,   8% used [0x029d0000, 0x02a77e98, 0x031d0000)
  from space 1024K,   0% used [0x031d0000, 0x031d0000, 0x032d0000)
  to   space 1024K,   0% used [0x032d0000, 0x032d0000, 0x033d0000)
tenured generation   total 10240K, used 4096K [0x033d0000, 0x03dd0000, 0x03dd0000)
   the space 10240K,  40% used [0x033d0000, 0x037d0010, 0x037d0200, 0x03dd0000)
compacting perm gen  total 12288K, used 2107K [0x03dd0000, 0x049d0000, 0x07dd0000)
   the space 12288K,  17% used [0x03dd0000, 0x03fdefd0, 0x03fdf000, 0x049d0000)
No shared spaces configured.
规则三：在eden经过GC后存活，并且survivor能容纳的对象，将移动到survivor空间内，如果对象在survivor中继续熬过若干次回收（默认为15次）将会被移动到老年代中。回收次数由MaxTenuringThreshold设置。
分别以-XX:MaxTenuringThreshold=1和-XX:MaxTenuringThreshold=15两种设置来执行testTenuringThreshold()，方法中allocation1对象需要256K内存，survivor空间可以容纳。当MaxTenuringThreshold=1时，allocation1对象在第二次GC发生时进入老年代，新生代已使用的内存GC后非常干净的变成0KB。而MaxTenuringThreshold=15时，第二次GC发生后，allocation1对象则还留在新生代survivor空间，这时候新生代仍然有404KB被占用。
清单4：
MaxTenuringThreshold=1
[GC [DefNew
Desired survivor size 524288 bytes, new threshold 1 (max 1)
- age   1:     414664 bytes,     414664 total
: 4859K->404K(9216K), 0.0065012 secs] 4859K->4500K(19456K), 0.0065283 secs] [Times: user=0.02 sys=0.00, real=0.02 secs]
[GC [DefNew
Desired survivor size 524288 bytes, new threshold 1 (max 1)
: 4500K->0K(9216K), 0.0009253 secs] 8596K->4500K(19456K), 0.0009458 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
Heap
def new generation   total 9216K, used 4178K [0x029d0000, 0x033d0000, 0x033d0000)
  eden space 8192K,  51% used [0x029d0000, 0x02de4828, 0x031d0000)
  from space 1024K,   0% used [0x031d0000, 0x031d0000, 0x032d0000)
  to   space 1024K,   0% used [0x032d0000, 0x032d0000, 0x033d0000)
tenured generation   total 10240K, used 4500K [0x033d0000, 0x03dd0000, 0x03dd0000)
   the space 10240K,  43% used [0x033d0000, 0x03835348, 0x03835400, 0x03dd0000)
compacting perm gen  total 12288K, used 2114K [0x03dd0000, 0x049d0000, 0x07dd0000)
   the space 12288K,  17% used [0x03dd0000, 0x03fe0998, 0x03fe0a00, 0x049d0000)
No shared spaces configured.
MaxTenuringThreshold=15
[GC [DefNew
Desired survivor size 524288 bytes, new threshold 15 (max 15)
- age   1:     414664 bytes,     414664 total
: 4859K->404K(9216K), 0.0049637 secs] 4859K->4500K(19456K), 0.0049932 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
[GC [DefNew
Desired survivor size 524288 bytes, new threshold 15 (max 15)
- age   2:     414520 bytes,     414520 total
: 4500K->404K(9216K), 0.0008091 secs] 8596K->4500K(19456K), 0.0008305 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
Heap
def new generation   total 9216K, used 4582K [0x029d0000, 0x033d0000, 0x033d0000)
  eden space 8192K,  51% used [0x029d0000, 0x02de4828, 0x031d0000)
  from space 1024K,  39% used [0x031d0000, 0x03235338, 0x032d0000)
  to   space 1024K,   0% used [0x032d0000, 0x032d0000, 0x033d0000)
tenured generation   total 10240K, used 4096K [0x033d0000, 0x03dd0000, 0x03dd0000)
   the space 10240K,  40% used [0x033d0000, 0x037d0010, 0x037d0200, 0x03dd0000)
compacting perm gen  total 12288K, used 2114K [0x03dd0000, 0x049d0000, 0x07dd0000)
   the space 12288K,  17% used [0x03dd0000, 0x03fe0998, 0x03fe0a00, 0x049d0000)
No shared spaces configured.
规则四：如果在survivor空间中相同年龄所有对象大小的累计值大于survivor空间的一半，大于或等于个年龄的对象就可以直接进入老年代，无需达到MaxTenuringThreshold中要求的年龄。
执行testTenuringThreshold2()方法，并将设置-XX:MaxTenuringThreshold=15，发现运行结果中survivor占用仍然为0%，而老年代比预期增加了6%，也就是说allocation1、allocation2对象都直接进入了老年代，而没有等待到15岁的临界年龄。因为这2个对象加起来已经到达了512K，并且它们是同年的，满足同年对象达到survivor空间的一半规则。我们只要注释掉其中一个对象new操作，就会发现另外一个就不会晋升到老年代中去了。
清单5：
[GC [DefNew
Desired survivor size 524288 bytes, new threshold 1 (max 15)
- age   1:     676824 bytes,     676824 total
: 5115K->660K(9216K), 0.0050136 secs] 5115K->4756K(19456K), 0.0050443 secs] [Times: user=0.00 sys=0.01, real=0.01 secs]
[GC [DefNew
Desired survivor size 524288 bytes, new threshold 15 (max 15)
: 4756K->0K(9216K), 0.0010571 secs] 8852K->4756K(19456K), 0.0011009 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
Heap
def new generation   total 9216K, used 4178K [0x029d0000, 0x033d0000, 0x033d0000)
  eden space 8192K,  51% used [0x029d0000, 0x02de4828, 0x031d0000)
  from space 1024K,   0% used [0x031d0000, 0x031d0000, 0x032d0000)
  to   space 1024K,   0% used [0x032d0000, 0x032d0000, 0x033d0000)
tenured generation   total 10240K, used 4756K [0x033d0000, 0x03dd0000, 0x03dd0000)
   the space 10240K,  46% used [0x033d0000, 0x038753e8, 0x03875400, 0x03dd0000)
compacting perm gen  total 12288K, used 2114K [0x03dd0000, 0x049d0000, 0x07dd0000)
   the space 12288K,  17% used [0x03dd0000, 0x03fe09a0, 0x03fe0a00, 0x049d0000)
No shared spaces configured.
规则五：在Minor GC触发时，会检测之前每次晋升到老年代的平均大小是否大于老年代的剩余空间，如果大于，改为直接进行一次Full GC，如果小于则查看HandlePromotionFailure设置看看是否允许担保失败，如果允许，那仍然进行Minor GC，如果不允许，则也要改为进行一次Full GC。
前面提到过，新生代才有复制收集算法，但为了内存利用率，只使用其中一个survivor空间来作为轮换备份，因此当出现大量对象在GC后仍然存活的情况（最极端就是GC后所有对象都存活），就需要老年代进行分配担保，把survivor无法容纳的对象直接放入老年代。与生活中贷款担保类似，老年代要进行这样的担保，前提就是老年代本身还有容纳这些对象的剩余空间，一共有多少对象在GC之前是无法明确知道的，所以取之前每一次GC晋升到老年代对象容量的平均值与老年代的剩余空间进行比较决定是否进行Full GC来让老年代腾出更多空间。
取平均值进行比较其实仍然是一种动态概率的手段，也就是说如果某次Minor GC存活后的对象突增，大大高于平均值的话，依然会导致担保失败，这样就只好在失败后重新进行一次Full GC。虽然担保失败时做的绕的圈子是最大的，但大部分情况下都还是会将HandlePromotionFailure打开，避免Full GC过于频繁。
清单6：
HandlePromotionFailure = false
[GC [DefNew: 6651K->148K(9216K), 0.0078936 secs] 6651K->4244K(19456K), 0.0079192 secs] [Times: user=0.00 sys=0.02, real=0.02 secs]
[GC [DefNew: 6378K->6378K(9216K), 0.0000206 secs][Tenured: 4096K->4244K(10240K), 0.0042901 secs] 10474K->4244K(19456K), [Perm : 2104K->2104K(12288K)], 0.0043613 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
HandlePromotionFailure = true
[GC [DefNew: 6651K->148K(9216K), 0.0054913 secs] 6651K->4244K(19456K), 0.0055327 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
[GC [DefNew: 6378K->148K(9216K), 0.0006584 secs] 10474K->4244K(19456K), 0.0006857 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
**总结**
本章介绍了垃圾收集的算法、6款主要的垃圾收集器，以及通过代码实例具体介绍了新生代串行收集器对内存分配及回收的影响。
GC在很多时候都是系统并发度的决定性因素，虚拟机之所以提供多种不同的收集器，提供大量的调节参数，是因为只有根据实际应用需求、实现方式选择最优的收集方式才能获取最好的性能。没有固定收集器、参数组合，也没有最优的调优方法，虚拟机也没有什么必然的行为。笔者看过一些文章，撇开具体场景去谈论老年代达到92%会触发Full GC（92%应当来自CMS收集器触发的默认临界点）、98%时间在进行垃圾收集系统会抛出OOM异常（98%应该来自parallel收集器收集时间比率的默认临界点）其实意义并不太大。因此学习GC如果要到实践调优阶段，必须了解每个具体收集器的行为、优势劣势、调节参数。
[JVM内存管理：深入Java内存区域与OOM](http://hllvm.group.iteye.com/group/wiki/2857-JVM "JVM内存管理：深入Java内存区域与OOM")

评论 共 6 条 请[登录](http://hllvm.group.iteye.com/login)后发表评论 []()

### 6 楼 [newjunwei](http://newjunwei.iteye.com/ "newjunwei") 2013-07-09 16:52

![]()
### 5 楼 [junedo](http://junedo.iteye.com/ "junedo") 2013-05-31 13:19

引用

ParNew收集器 介绍中 “对应的这种收集器是虚拟机运行在Server模式的默认新生代收集器”
为什么我用hotspot 1.6.0_37 在windows 32 和linux 64环境下指定 -server时，新生代的收集器都是**PS Scavenge**？是不是此处描述存在错误呢？

### 4 楼 [PV_love](http://pv-love.iteye.com/ "PV_love") 2013-04-03 14:11

thetcc 写道

-XX:SurvivorRatio=8决定了新生代中eden与survivor的空间比例是1：8，从输出的结果也清晰的看到“eden space 8192K、from space 1024K、to space 1024K”的信息
这段说反了吧？eden是较大的空间，survivor是较小的空间，比例8：1
### 3 楼 [scgyyc820802](http://scgyyc820802.iteye.com/ "scgyyc820802") 2011-11-25 16:50

好文章，如果我能早点看到它，半年前的一个客户就不会退货了。
程序没有万能，只有最合适。

### 2 楼 [thetcc](http://thetcc.iteye.com/ "thetcc") 2011-08-31 14:48

-XX:SurvivorRatio=8决定了新生代中eden与survivor的空间比例是1：8，从输出的结果也清晰的看到“eden space 8192K、from space 1024K、to space 1024K”的信息
### 1 楼 [qepwqnp](http://chengjunflying.iteye.com/ "qepwqnp") 2011-05-05 20:28

不错，顶一个。 大概看了下，下来再细看
### 发表评论

[![]() 您还没有登录,请您登录后再发表评论](http://hllvm.group.iteye.com/login)

[![New-page]()](http://hllvm.group.iteye.com/group/wiki/new)

### 文章信息

[知识库: 高级语言虚拟机](http://hllvm.group.iteye.com/group/wiki/)

* 由[fantasy](http://kiral.iteye.com/ "fantasy")在2010-11-08创建
* 由[fantasy](http://kiral.iteye.com/ "fantasy")在2011-05-26更新
### 相关新闻

* [Azul Systems将要开源Managed Runtime Initiative中的重要技术](http://hllvm.group.iteye.com/news/16601)
* [ruby内存泄漏的罪魁祸首 - 幽灵指针](http://hllvm.group.iteye.com/news/4407)
* [推荐风轻扬：Java 6中的性能优化](http://hllvm.group.iteye.com/news/2823)

### 相关讨论

* [JVM内存管理：深入垃圾收集器与内存分配策略](http://hllvm.group.iteye.com/topic/802638)
* [CMS gc实践总结](http://hllvm.group.iteye.com/topic/473874)
* [优化JVM参数提高eclipse运行速度](http://hllvm.group.iteye.com/topic/756538)
* [一次Java垃圾收集调优实战](http://hllvm.group.iteye.com/topic/212967)
* [HotSpot VM 内存堆的两个Servivor区](http://hllvm.group.iteye.com/topic/894148)
### 相关博客

* [JVM内存分配策略](http://wujt.iteye.com/blog/1703772)
* [转-JVM内存管理：深入垃圾收集器与内存分配策略](http://shineman123.iteye.com/blog/988338)
* [JVM内存管理：深入垃圾收集器与内存分配策略](http://jczghost.iteye.com/blog/809783)
* [JVM内存管理：深入垃圾收集器与内存分配策略](http://millerhu.iteye.com/blog/890704)
* [java 内存管理（二）](http://baobeituping.iteye.com/blog/941279)
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
百联优力(北京)投资有限公司 版权所有 ![](http://stat.iteye.com/?url=http%3A%2F%2Fhllvm.group.iteye.com%2Fgroup%2Fwiki%2F2859-JVM&referrer=&user_id=)
