---
layout: post
title: "eden区没有发生minor gc，对象直接分配在了old gen"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# eden区没有发生minor gc，对象直接分配在了old gen

[您还未登录 !](http://hllvm.group.iteye.com/login "登录") [登录](http://hllvm.group.iteye.com/login) [注册](http://hllvm.group.iteye.com/signup)

[![ITeye3.0]( "ITeye-最棒的软件开发交流社区")](http://www.iteye.com/)

[群组首页](http://www.iteye.com/groups) → [编程语言](http://hllvm.group.iteye.com/groups/category/language) → [高级语言虚拟机](http://hllvm.group.iteye.com/) → [论坛](http://hllvm.group.iteye.com/group/forum)

![]() [发表回复](http://hllvm.group.iteye.com/group/topic/38293/post/new)

### [[讨论]](http://hllvm.group.iteye.com/group/forum?tag_id=690) [eden区没有发生minor gc，对象直接分配在了old gen]()

[![等待雨季的到来的博客]( "等待雨季的到来的博客: ")](http://zsl8544-163-com.iteye.com/) [等待雨季的到来](http://zsl8544-163-com.iteye.com/ "等待雨季的到来") 2013-07-21

我的测试代码如下
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. /** 
1.      * -Xmx100M -XX:+PrintGCDetails -XX:MaxNewSize=40M -XX:+PrintTenuringDistribution 
1.      */  
1.     public static void testAllocation(){  
1.         byte[] byte1 = new byte[_1MB*5];  
1.         byte[] byte2 = new byte[_1MB*10];  
1.         byte1 = null;  
1.         byte2 = null;                         
1.         byte[] byte3 = new byte[_1MB*5];  
1.         byte[] byte4 = new byte[_1MB*10];  
1.         byte3 = null;  
1.         byte4 = null;  
1.         byte[] byte5 = new byte[_1MB*15];   
1.     }  
/** * -Xmx100M -XX:+PrintGCDetails -XX:MaxNewSize=40M -XX:+PrintTenuringDistribution */ public static void testAllocation(){ byte[] byte1 = new byte[_1MB*5]; byte[] byte2 = new byte[_1MB*10]; byte1 = null; byte2 = null; byte[] byte3 = new byte[_1MB*5]; byte[] byte4 = new byte[_1MB*10]; byte3 = null; byte4 = null; byte[] byte5 = new byte[_1MB*15]; }
因为没有用NewSize限定新生代的初始大小，所以eden区的初始大小为20M，而我用MaxNewSize限定了新生代的初始大小为40M。
GC Log
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. [GC  
1. Desired survivor size 3538944 bytes, new threshold 7 (max 15)  
1.  [PSYoungGen: 16194K->368K(24320K)] 16194K->368K(79872K), 0.0020460 secs] [Times: user=0.01 sys=0.00, real=0.00 secs]   
1. Heap  
1.  PSYoungGen      total 24320K, used 17199K [0x0000000115490000, 0x0000000117c90000, 0x0000000117c90000)  
1.   eden space 20864K, 80% used [0x0000000115490000,0x00000001164ffe38,0x00000001168f0000)  
1.   from space 3456K, 10% used [0x00000001168f0000,0x000000011694c010,0x0000000116c50000)  
1.   to   space 3456K, 0% used [0x0000000117930000,0x0000000117930000,0x0000000117c90000)  
1.  ParOldGen       total 55552K, used 15360K [0x0000000111890000, 0x0000000114ed0000, 0x0000000115490000)  
1.   object space 55552K, 27% used [0x0000000111890000,0x0000000112790010,0x0000000114ed0000)  
1.  PSPermGen       total 21248K, used 2695K [0x000000010c690000, 0x000000010db50000, 0x0000000111890000)  
1.   object space 21248K, 12% used [0x000000010c690000,0x000000010c931ef0,0x000000010db50000)  
[GC Desired survivor size 3538944 bytes, new threshold 7 (max 15) [PSYoungGen: 16194K->368K(24320K)] 16194K->368K(79872K), 0.0020460 secs] [Times: user=0.01 sys=0.00, real=0.00 secs] Heap PSYoungGen total 24320K, used 17199K [0x0000000115490000, 0x0000000117c90000, 0x0000000117c90000) eden space 20864K, 80% used [0x0000000115490000,0x00000001164ffe38,0x00000001168f0000) from space 3456K, 10% used [0x00000001168f0000,0x000000011694c010,0x0000000116c50000) to space 3456K, 0% used [0x0000000117930000,0x0000000117930000,0x0000000117c90000) ParOldGen total 55552K, used 15360K [0x0000000111890000, 0x0000000114ed0000, 0x0000000115490000) object space 55552K, 27% used [0x0000000111890000,0x0000000112790010,0x0000000114ed0000) PSPermGen total 21248K, used 2695K [0x000000010c690000, 0x000000010db50000, 0x0000000111890000) object space 21248K, 12% used [0x000000010c690000,0x000000010c931ef0,0x000000010db50000)
现象：
当分配byte3时触发了一次minor gc，byte1和byte2都被回收了，可是我分配byte5的时候，却直接分配在了老生代，byte3和byte4依旧在eden区，而byte5直接被分配在了老生代。
问题：
1、byte5为什么没有触发minor gc而是直接分配在老生代，byte3和byte4为什么没有被回收？
2、eden区什么时候才会自动扩容到40M，当创建超过20M的对象时，都是直接分配在了老生，而创建大小超过老生代的对象时老生代会自动扩容。
其他：
我在用eclipse调试的时候发现，同样的测试代码在执行的时候输出的GC log居然会有很大的变化，有的时候eden区被初始化为8M有的时候初始化为20M。我工作在OS X下，JDK版本如下。
java version "1.7.0_25"
Java(TM) SE Runtime Environment (build 1.7.0_25-b15)
Java HotSpot(TM) 64-Bit Server VM (build 23.25-b01, mixed mode)
![Spinner]() [![RednaxelaFX的博客]( "RednaxelaFX的博客: Script Ahead, Code Behind")](http://rednaxelafx.iteye.com/) [RednaxelaFX](http://rednaxelafx.iteye.com/ "RednaxelaFX") 2013-07-25

楼主是看了毕玄的实验之后自己动手测试么？
把楼主的代码弄成可执行的，我用的测试代码是：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. public class PSAllocTest {  
1.   private static final int _1KB = 1024;  
1.   private static final int _1MB = _1KB * 1024;  
1.     
1.   /** 
1.    * -Xmx100M -XX:+PrintGCDetails -XX:MaxNewSize=40M -XX:+PrintTenuringDistribution 
1.    */  
1.   public static void testAllocation() {  
1.     byte[] byte1 = new byte[_1MB*5];  
1.     byte[] byte2 = new byte[_1MB*10];  
1.     byte1 = null;  
1.     byte2 = null;                         
1.     byte[] byte3 = new byte[_1MB*5];  
1.     byte[] byte4 = new byte[_1MB*10];  
1.     byte3 = null;  
1.     byte4 = null;  
1.     byte[] byte5 = new byte[_1MB*15];   
1.   }  
1.     
1.   public static void main(String[] args) {  
1.     testAllocation();  
1.   }  
1. }  
public class PSAllocTest { private static final int _1KB = 1024; private static final int _1MB = _1KB * 1024; /** * -Xmx100M -XX:+PrintGCDetails -XX:MaxNewSize=40M -XX:+PrintTenuringDistribution */ public static void testAllocation() { byte[] byte1 = new byte[_1MB*5]; byte[] byte2 = new byte[_1MB*10]; byte1 = null; byte2 = null; byte[] byte3 = new byte[_1MB*5]; byte[] byte4 = new byte[_1MB*10]; byte3 = null; byte4 = null; byte[] byte5 = new byte[_1MB*15]; } public static void main(String[] args) { testAllocation(); } }
我这边在JDK7u9 64-bit Server VM上跑看到的GC日志会有两次GC：
Gc log代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. D:\test\pstest>java -Xmx100M -XX:+PrintGCDetails -XX:MaxNewSize=40M PSAllocTest  
1. [GC [PSYoungGen: 16002K->584K(18688K)] 16002K->584K(61376K), 0.0016857 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]  
1. [GC [PSYoungGen: 16648K->576K(34752K)] 32008K->15936K(77440K), 0.0014163 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]  
1. Heap  
1.  PSYoungGen      total 34752K, used 1218K [0x00000000fd800000, 0x00000000ffc80000, 0x0000000100000000)  
1.   eden space 32128K, 2% used [0x00000000fd800000,0x00000000fd8a0ab0,0x00000000ff760000)  
1.   from space 2624K, 21% used [0x00000000ff9f0000,0x00000000ffa80030,0x00000000ffc80000)  
1.   to   space 2624K, 0% used [0x00000000ff760000,0x00000000ff760000,0x00000000ff9f0000)  
1.  ParOldGen       total 42688K, used 15360K [0x00000000f9c00000, 0x00000000fc5b0000, 0x00000000fd800000)  
1.   object space 42688K, 35% used [0x00000000f9c00000,0x00000000fab00010,0x00000000fc5b0000)  
1.  PSPermGen       total 21248K, used 2505K [0x00000000f4a00000, 0x00000000f5ec0000, 0x00000000f9c00000)  
1.   object space 21248K, 11% used [0x00000000f4a00000,0x00000000f4c72638,0x00000000f5ec0000)  
D:\test\pstest>java -Xmx100M -XX:+PrintGCDetails -XX:MaxNewSize=40M PSAllocTest [GC [PSYoungGen: 16002K->584K(18688K)] 16002K->584K(61376K), 0.0016857 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] [GC [PSYoungGen: 16648K->576K(34752K)] 32008K->15936K(77440K), 0.0014163 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] Heap PSYoungGen total 34752K, used 1218K [0x00000000fd800000, 0x00000000ffc80000, 0x0000000100000000) eden space 32128K, 2% used [0x00000000fd800000,0x00000000fd8a0ab0,0x00000000ff760000) from space 2624K, 21% used [0x00000000ff9f0000,0x00000000ffa80030,0x00000000ffc80000) to space 2624K, 0% used [0x00000000ff760000,0x00000000ff760000,0x00000000ff9f0000) ParOldGen total 42688K, used 15360K [0x00000000f9c00000, 0x00000000fc5b0000, 0x00000000fd800000) object space 42688K, 35% used [0x00000000f9c00000,0x00000000fab00010,0x00000000fc5b0000) PSPermGen total 21248K, used 2505K [0x00000000f4a00000, 0x00000000f5ec0000, 0x00000000f9c00000) object space 21248K, 11% used [0x00000000f4a00000,0x00000000f4c72638,0x00000000f5ec0000)
第二次GC其实已经在第5个数组成功分配之后了，就楼主关心的部分看我这边看到的跟楼主实验看到的行为一致，只要关心日志里的第一次GC就好了。
如果加上额外参数的话，可以看到更多GC信息：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. D:\test\pstest>java -Xmx100M -XX:+PrintGCDetails -XX:+PrintHeapAtGC -XX:+PrintTLAB -XX:MaxNewSize=40M -XX:+PrintTenuringDistribution PSAllocTest  
1. {Heap before GC invocations=1 (full 0):  
1.  PSYoungGen      total 18688K, used 16002K [0x00000000fd800000, 0x00000000fecd0000, 0x0000000100000000)  
1.   eden space 16064K, 99% used [0x00000000fd800000,0x00000000fe7a0b60,0x00000000fe7b0000)  
1.   from space 2624K, 0% used [0x00000000fea40000,0x00000000fea40000,0x00000000fecd0000)  
1.   to   space 2624K, 0% used [0x00000000fe7b0000,0x00000000fe7b0000,0x00000000fea40000)  
1.  ParOldGen       total 42688K, used 0K [0x00000000f9c00000, 0x00000000fc5b0000, 0x00000000fd800000)  
1.   object space 42688K, 0% used [0x00000000f9c00000,0x00000000f9c00000,0x00000000fc5b0000)  
1.  PSPermGen       total 21248K, used 2495K [0x00000000f4a00000, 0x00000000f5ec0000, 0x00000000f9c00000)  
1.   object space 21248K, 11% used [0x00000000f4a00000,0x00000000f4c6fc80,0x00000000f5ec0000)  
1. TLAB: gc thread: 0x000000000028c000 [id: 10344] desired_size: 321KB slow allocs: 3  refill waste: 5232B alloc: 0.99998    16003KB refills: 2 waste  6.2% gc: 40912B slow: 40B fast: 0B  
1. TLAB totals: thrds: 1  refills: 2 max: 2 slow allocs: 3 max 3 waste:  6.2% gc: 40912B max: 40912B slow: 40B max: 40B fast: 0B max: 0B  
1. [GC  
1. Desired survivor size 2686976 bytes, new threshold 7 (max 15)  
1.  [PSYoungGen: 16002K->640K(18688K)] 16002K->640K(61376K), 0.0015035 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]   
1. Heap after GC invocations=1 (full 0):  
1.  PSYoungGen      total 18688K, used 640K [0x00000000fd800000, 0x00000000ffc80000, 0x0000000100000000)  
1.   eden space 16064K, 0% used [0x00000000fd800000,0x00000000fd800000,0x00000000fe7b0000)  
1.   from space 2624K, 24% used [0x00000000fe7b0000,0x00000000fe850030,0x00000000fea40000)  
1.   to   space 2624K, 0% used [0x00000000ff9f0000,0x00000000ff9f0000,0x00000000ffc80000)  
1.  ParOldGen       total 42688K, used 0K [0x00000000f9c00000, 0x00000000fc5b0000, 0x00000000fd800000)  
1.   object space 42688K, 0% used [0x00000000f9c00000,0x00000000f9c00000,0x00000000fc5b0000)  
1.  PSPermGen       total 21248K, used 2495K [0x00000000f4a00000, 0x00000000f5ec0000, 0x00000000f9c00000)  
1.   object space 21248K, 11% used [0x00000000f4a00000,0x00000000f4c6fc80,0x00000000f5ec0000)  
1. }  
1. {Heap before GC invocations=2 (full 0):  
1.  PSYoungGen      total 18688K, used 16704K [0x00000000fd800000, 0x00000000ffc80000, 0x0000000100000000)  
1.   eden space 16064K, 100% used [0x00000000fd800000,0x00000000fe7b0000,0x00000000fe7b0000)  
1.   from space 2624K, 24% used [0x00000000fe7b0000,0x00000000fe850030,0x00000000fea40000)  
1.   to   space 2624K, 0% used [0x00000000ff9f0000,0x00000000ff9f0000,0x00000000ffc80000)  
1.  ParOldGen       total 42688K, used 15360K [0x00000000f9c00000, 0x00000000fc5b0000, 0x00000000fd800000)  
1.   object space 42688K, 35% used [0x00000000f9c00000,0x00000000fab00010,0x00000000fc5b0000)  
1.  PSPermGen       total 21248K, used 2498K [0x00000000f4a00000, 0x00000000f5ec0000, 0x00000000f9c00000)  
1.   object space 21248K, 11% used [0x00000000f4a00000,0x00000000f4c708a0,0x00000000f5ec0000)  
1. TLAB: gc thread: 0x00000000022a5800 [id: 10100] desired_size: 321KB slow allocs: 0  refill waste: 5136B alloc: 0.99998    16064KB refills: 1 waste 100.0% gc: 328952B slow: 0B fast: 0B  
1. TLAB: gc thread: 0x000000000229e800 [id: 3928] desired_size: 321KB slow allocs: 0  refill waste: 5136B alloc: 0.99998    16064KB refills: 1 waste 100.0% gc: 328984B slow: 0B fast: 0B  
1. TLAB totals: thrds: 2  refills: 2 max: 1 slow allocs: 0 max 0 waste: 100.0% gc: 657936B max: 328984B slow: 0B max: 0B fast: 0B max: 0B  
1. [GC  
1. Desired survivor size 2686976 bytes, new threshold 7 (max 15)  
1.  [PSYoungGen: 16704K->520K(34752K)] 32064K->15880K(77440K), 0.0012669 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]   
1. Heap after GC invocations=2 (full 0):  
1.  PSYoungGen      total 34752K, used 520K [0x00000000fd800000, 0x00000000ffc80000, 0x0000000100000000)  
1.   eden space 32128K, 0% used [0x00000000fd800000,0x00000000fd800000,0x00000000ff760000)  
1.   from space 2624K, 19% used [0x00000000ff9f0000,0x00000000ffa72020,0x00000000ffc80000)  
1.   to   space 2624K, 0% used [0x00000000ff760000,0x00000000ff760000,0x00000000ff9f0000)  
1.  ParOldGen       total 42688K, used 15360K [0x00000000f9c00000, 0x00000000fc5b0000, 0x00000000fd800000)  
1.   object space 42688K, 35% used [0x00000000f9c00000,0x00000000fab00010,0x00000000fc5b0000)  
1.  PSPermGen       total 21248K, used 2498K [0x00000000f4a00000, 0x00000000f5ec0000, 0x00000000f9c00000)  
1.   object space 21248K, 11% used [0x00000000f4a00000,0x00000000f4c708a0,0x00000000f5ec0000)  
1. }  
1. Heap  
1.  PSYoungGen      total 34752K, used 1162K [0x00000000fd800000, 0x00000000ffc80000, 0x0000000100000000)  
1.   eden space 32128K, 2% used [0x00000000fd800000,0x00000000fd8a0ab0,0x00000000ff760000)  
1.   from space 2624K, 19% used [0x00000000ff9f0000,0x00000000ffa72020,0x00000000ffc80000)  
1.   to   space 2624K, 0% used [0x00000000ff760000,0x00000000ff760000,0x00000000ff9f0000)  
1.  ParOldGen       total 42688K, used 15360K [0x00000000f9c00000, 0x00000000fc5b0000, 0x00000000fd800000)  
1.   object space 42688K, 35% used [0x00000000f9c00000,0x00000000fab00010,0x00000000fc5b0000)  
1.  PSPermGen       total 21248K, used 2505K [0x00000000f4a00000, 0x00000000f5ec0000, 0x00000000f9c00000)  
1.   object space 21248K, 11% used [0x00000000f4a00000,0x00000000f4c72638,0x00000000f5ec0000)  
D:\test\pstest>java -Xmx100M -XX:+PrintGCDetails -XX:+PrintHeapAtGC -XX:+PrintTLAB -XX:MaxNewSize=40M -XX:+PrintTenuringDistribution PSAllocTest {Heap before GC invocations=1 (full 0): PSYoungGen total 18688K, used 16002K [0x00000000fd800000, 0x00000000fecd0000, 0x0000000100000000) eden space 16064K, 99% used [0x00000000fd800000,0x00000000fe7a0b60,0x00000000fe7b0000) from space 2624K, 0% used [0x00000000fea40000,0x00000000fea40000,0x00000000fecd0000) to space 2624K, 0% used [0x00000000fe7b0000,0x00000000fe7b0000,0x00000000fea40000) ParOldGen total 42688K, used 0K [0x00000000f9c00000, 0x00000000fc5b0000, 0x00000000fd800000) object space 42688K, 0% used [0x00000000f9c00000,0x00000000f9c00000,0x00000000fc5b0000) PSPermGen total 21248K, used 2495K [0x00000000f4a00000, 0x00000000f5ec0000, 0x00000000f9c00000) object space 21248K, 11% used [0x00000000f4a00000,0x00000000f4c6fc80,0x00000000f5ec0000) TLAB: gc thread: 0x000000000028c000 [id: 10344] desired_size: 321KB slow allocs: 3 refill waste: 5232B alloc: 0.99998 16003KB refills: 2 waste 6.2% gc: 40912B slow: 40B fast: 0B TLAB totals: thrds: 1 refills: 2 max: 2 slow allocs: 3 max 3 waste: 6.2% gc: 40912B max: 40912B slow: 40B max: 40B fast: 0B max: 0B [GC Desired survivor size 2686976 bytes, new threshold 7 (max 15) [PSYoungGen: 16002K->640K(18688K)] 16002K->640K(61376K), 0.0015035 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] Heap after GC invocations=1 (full 0): PSYoungGen total 18688K, used 640K [0x00000000fd800000, 0x00000000ffc80000, 0x0000000100000000) eden space 16064K, 0% used [0x00000000fd800000,0x00000000fd800000,0x00000000fe7b0000) from space 2624K, 24% used [0x00000000fe7b0000,0x00000000fe850030,0x00000000fea40000) to space 2624K, 0% used [0x00000000ff9f0000,0x00000000ff9f0000,0x00000000ffc80000) ParOldGen total 42688K, used 0K [0x00000000f9c00000, 0x00000000fc5b0000, 0x00000000fd800000) object space 42688K, 0% used [0x00000000f9c00000,0x00000000f9c00000,0x00000000fc5b0000) PSPermGen total 21248K, used 2495K [0x00000000f4a00000, 0x00000000f5ec0000, 0x00000000f9c00000) object space 21248K, 11% used [0x00000000f4a00000,0x00000000f4c6fc80,0x00000000f5ec0000) } {Heap before GC invocations=2 (full 0): PSYoungGen total 18688K, used 16704K [0x00000000fd800000, 0x00000000ffc80000, 0x0000000100000000) eden space 16064K, 100% used [0x00000000fd800000,0x00000000fe7b0000,0x00000000fe7b0000) from space 2624K, 24% used [0x00000000fe7b0000,0x00000000fe850030,0x00000000fea40000) to space 2624K, 0% used [0x00000000ff9f0000,0x00000000ff9f0000,0x00000000ffc80000) ParOldGen total 42688K, used 15360K [0x00000000f9c00000, 0x00000000fc5b0000, 0x00000000fd800000) object space 42688K, 35% used [0x00000000f9c00000,0x00000000fab00010,0x00000000fc5b0000) PSPermGen total 21248K, used 2498K [0x00000000f4a00000, 0x00000000f5ec0000, 0x00000000f9c00000) object space 21248K, 11% used [0x00000000f4a00000,0x00000000f4c708a0,0x00000000f5ec0000) TLAB: gc thread: 0x00000000022a5800 [id: 10100] desired_size: 321KB slow allocs: 0 refill waste: 5136B alloc: 0.99998 16064KB refills: 1 waste 100.0% gc: 328952B slow: 0B fast: 0B TLAB: gc thread: 0x000000000229e800 [id: 3928] desired_size: 321KB slow allocs: 0 refill waste: 5136B alloc: 0.99998 16064KB refills: 1 waste 100.0% gc: 328984B slow: 0B fast: 0B TLAB totals: thrds: 2 refills: 2 max: 1 slow allocs: 0 max 0 waste: 100.0% gc: 657936B max: 328984B slow: 0B max: 0B fast: 0B max: 0B [GC Desired survivor size 2686976 bytes, new threshold 7 (max 15) [PSYoungGen: 16704K->520K(34752K)] 32064K->15880K(77440K), 0.0012669 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] Heap after GC invocations=2 (full 0): PSYoungGen total 34752K, used 520K [0x00000000fd800000, 0x00000000ffc80000, 0x0000000100000000) eden space 32128K, 0% used [0x00000000fd800000,0x00000000fd800000,0x00000000ff760000) from space 2624K, 19% used [0x00000000ff9f0000,0x00000000ffa72020,0x00000000ffc80000) to space 2624K, 0% used [0x00000000ff760000,0x00000000ff760000,0x00000000ff9f0000) ParOldGen total 42688K, used 15360K [0x00000000f9c00000, 0x00000000fc5b0000, 0x00000000fd800000) object space 42688K, 35% used [0x00000000f9c00000,0x00000000fab00010,0x00000000fc5b0000) PSPermGen total 21248K, used 2498K [0x00000000f4a00000, 0x00000000f5ec0000, 0x00000000f9c00000) object space 21248K, 11% used [0x00000000f4a00000,0x00000000f4c708a0,0x00000000f5ec0000) } Heap PSYoungGen total 34752K, used 1162K [0x00000000fd800000, 0x00000000ffc80000, 0x0000000100000000) eden space 32128K, 2% used [0x00000000fd800000,0x00000000fd8a0ab0,0x00000000ff760000) from space 2624K, 19% used [0x00000000ff9f0000,0x00000000ffa72020,0x00000000ffc80000) to space 2624K, 0% used [0x00000000ff760000,0x00000000ff760000,0x00000000ff9f0000) ParOldGen total 42688K, used 15360K [0x00000000f9c00000, 0x00000000fc5b0000, 0x00000000fd800000) object space 42688K, 35% used [0x00000000f9c00000,0x00000000fab00010,0x00000000fc5b0000) PSPermGen total 21248K, used 2505K [0x00000000f4a00000, 0x00000000f5ec0000, 0x00000000f9c00000) object space 21248K, 11% used [0x00000000f4a00000,0x00000000f4c72638,0x00000000f5ec0000)
加上-XX:+PrintHeapAtGC可以看到每次GC前后堆布局的情况；加上-XX:+PrintTLAB可以看到TLAB的使用状况。
从日志可以看到，这个线程的TLAB才只有321KB，不足以分配楼主例子里的任何一个数组。也就是说例子里的数组全部都不是在TLAB里分配的，而是直接分配在GC堆的共享部分。
我们可以模拟一下楼主的例子的执行过程。整个例子都肯定是在解释器里跑的，所以我们就从解释器来切入。
分配第一个5MB的数组时，解释器要执行newarray字节码指令，来到这里：
hotspot/src/share/vm/interpreter/interpreterRuntime.cpp
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. IRT_ENTRY(void, InterpreterRuntime::newarray(JavaThread* thread, BasicType type, jint size))  
1.   oop obj = oopFactory::new_typeArray(type, size, CHECK);  
1.   thread->set_vm_result(obj);  
1. IRT_END  
IRT_ENTRY(void, InterpreterRuntime::newarray(JavaThread* thread, BasicType type, jint size)) oop obj = oopFactory::new_typeArray(type, size, CHECK); thread->set_vm_result(obj); IRT_END
里面调用到：
hotspot/src/share/vm/memory/oopFactory.cpp
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. typeArrayOop oopFactory::new_typeArray(BasicType type, int length, TRAPS) {  
1.   klassOop type_asKlassOop = Universe::typeArrayKlassObj(type);  
1.   typeArrayKlass* type_asArrayKlass = typeArrayKlass::cast(type_asKlassOop);  
1.   typeArrayOop result = type_asArrayKlass->allocate(length, THREAD);  
1.   return result;  
1. }  
typeArrayOop oopFactory::new_typeArray(BasicType type, int length, TRAPS) { klassOop type_asKlassOop = Universe::typeArrayKlassObj(type); typeArrayKlass* type_asArrayKlass = typeArrayKlass::cast(type_asKlassOop); typeArrayOop result = type_asArrayKlass->allocate(length, THREAD); return result; }
进一步调用到：
hotspot/src/share/vm/oops/typeArrayKlass.cpp
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. typeArrayOop typeArrayKlass::allocate(int length, TRAPS) {  
1.   assert(log2_element_size() >= 0, "bad scale");  
1.   if (length >= 0) {  
1.     if (length <= max_length()) {  
1.       size_t size = typeArrayOopDesc::object_size(layout_helper(), length);  
1.       KlassHandle h_k(THREAD, as_klassOop());  
1.       typeArrayOop t;  
1.       CollectedHeap* ch = Universe::heap();  
1.       if (size < ch->large_typearray_limit()) {  
1.         t = (typeArrayOop)CollectedHeap::array_allocate(h_k, (int)size, length, CHECK_NULL);  
1.       } else {  
1.         t = (typeArrayOop)CollectedHeap::large_typearray_allocate(h_k, (int)size, length, CHECK_NULL);  
1.       }  
1.       assert(t->is_parsable(), "Don't publish unless parsable");  
1.       return t;  
1.     } else {  
1.       report_java_out_of_memory("Requested array size exceeds VM limit");  
1.       THROW_OOP_0(Universe::out_of_memory_error_array_size());  
1.     }  
1.   } else {  
1.     THROW_0(vmSymbols::java_lang_NegativeArraySizeException());  
1.   }  
1. }  
typeArrayOop typeArrayKlass::allocate(int length, TRAPS) { assert(log2_element_size() >= 0, "bad scale"); if (length >= 0) { if (length <= max_length()) { size_t size = typeArrayOopDesc::object_size(layout_helper(), length); KlassHandle h_k(THREAD, as_klassOop()); typeArrayOop t; CollectedHeap* ch = Universe::heap(); if (size < ch->large_typearray_limit()) { t = (typeArrayOop)CollectedHeap::array_allocate(h_k, (int)size, length, CHECK_NULL); } else { t = (typeArrayOop)CollectedHeap::large_typearray_allocate(h_k, (int)size, length, CHECK_NULL); } assert(t->is_parsable(), "Don't publish unless parsable"); return t; } else { report_java_out_of_memory("Requested array size exceeds VM limit"); THROW_OOP_0(Universe::out_of_memory_error_array_size()); } } else { THROW_0(vmSymbols::java_lang_NegativeArraySizeException()); } }
这个地方我们看到代码根据要分配的数组是否大于large_typearray_limit()做了分支。那么这个界限是多大呢？
hotspot/src/share/vm/gc_implementation/parallelScavenge/parallelScavengeHeap.hpp
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. size_t large_typearray_limit() { return FastAllocateSizeLimit; }  
size_t large_typearray_limit() { return FastAllocateSizeLimit; }
hotspot/src/share/vm/runtime/globals.hpp
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. develop(intx, FastAllocateSizeLimit, 128*K,                               \  
1.         /* Note:  This value is zero mod 1<<13 for a cheap sparc set. */  \  
1.         "Inline allocations larger than this in doublewords must go slow")\  
develop(intx, FastAllocateSizeLimit, 128*K, \ /* Note: This value is zero mod 1<<13 for a cheap sparc set. */ \ "Inline allocations larger than this in doublewords must go slow")\
这里我们知道了如果对象大小大于128K个doubleword（8字节单元），也就是1MB，就不会进入快速分配路径，而会走慢速分配路径。楼主例子里的数组全部都大于1MB，所以都肯定会走满足分配路径。
不过其实具体到现在HotSpot VM的实现，FastAllocateSizeLimit参数的影响甚微。
它最明显的作用就是让HotSpot Server Compiler（C2）在编译Java方法时看到要分配大于该参数指定的大小的对象时不生成快速分配路径的代码，而直接调用回到VM的慢速分配路径。
而对ParallelScanvege GC来说，这个参数其实设到多少都不影响该GC的行为：本来它就只间接影响到下面讲到的is_noref参数的值，但ParallelScavenge无视了is_noref参数。
回到分配的模拟。接下来调用到：
hotspot/src/share/vm/gc_interface/collectedHeap.inline.hpp
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. oop CollectedHeap::large_typearray_allocate(KlassHandle klass,  
1.                                             int size,  
1.                                             int length,  
1.                                             TRAPS) {  
1.   debug_only(check_for_valid_allocation_state());  
1.   assert(!Universe::heap()->is_gc_active(), "Allocation during gc not allowed");  
1.   assert(size >= 0, "int won't convert to size_t");  
1.   HeapWord* obj = common_mem_allocate_init(size, true, CHECK_NULL);  
1.   post_allocation_setup_array(klass, obj, size, length);  
1.   NOT_PRODUCT(Universe::heap()->check_for_bad_heap_word_value(obj, size));  
1.   return (oop)obj;  
1. }  
oop CollectedHeap::large_typearray_allocate(KlassHandle klass, int size, int length, TRAPS) { debug_only(check_for_valid_allocation_state()); assert(!Universe::heap()->is_gc_active(), "Allocation during gc not allowed"); assert(size >= 0, "int won't convert to size_t"); HeapWord* obj = common_mem_allocate_init(size, true, CHECK_NULL); post_allocation_setup_array(klass, obj, size, length); NOT_PRODUCT(Universe::heap()->check_for_bad_heap_word_value(obj, size)); return (oop)obj; }
这里留意它传给common_mem_allocate_init()的is_noref参数为true，就是说不要refill TLAB。
同一文件里，
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. HeapWord* CollectedHeap::common_mem_allocate_noinit(size_t size, bool is_noref, TRAPS) {  
1.   
1.   // Clear unhandled oops for memory allocation.  Memory allocation might  
1.   // not take out a lock if from tlab, so clear here.  
1.   CHECK_UNHANDLED_OOPS_ONLY(THREAD->clear_unhandled_oops();)  
1.   
1.   if (HAS_PENDING_EXCEPTION) {  
1.     NOT_PRODUCT(guarantee(false, "Should not allocate with exception pending"));  
1.     return NULL;  // caller does a CHECK_0 too  
1.   }  
1.   
1.   // We may want to update this, is_noref objects might not be allocated in TLABs.  
1.   HeapWord* result = NULL;  
1.   if (UseTLAB) {  
1.     result = CollectedHeap::allocate_from_tlab(THREAD, size);  
1.     if (result != NULL) {  
1.       assert(!HAS_PENDING_EXCEPTION,  
1.              "Unexpected exception, will result in uninitialized storage");  
1.       return result;  
1.     }  
1.   }  
1.   bool gc_overhead_limit_was_exceeded = false;  
1.   result = Universe::heap()->mem_allocate(size,  
1.                                           is_noref,  
1.                                           false,  
1.                                           &gc_overhead_limit_was_exceeded);  
1.   if (result != NULL) {  
1.     NOT_PRODUCT(Universe::heap()->  
1.       check_for_non_bad_heap_word_value(result, size));  
1.     assert(!HAS_PENDING_EXCEPTION,  
1.            "Unexpected exception, will result in uninitialized storage");  
1.     THREAD->incr_allocated_bytes(size * HeapWordSize);  
1.     return result;  
1.   }  
1.   
1.   
1.   if (!gc_overhead_limit_was_exceeded) {  
1.     // -XX:+HeapDumpOnOutOfMemoryError and -XX:OnOutOfMemoryError support  
1.     report_java_out_of_memory("Java heap space");  
1.   
1.     if (JvmtiExport::should_post_resource_exhausted()) {  
1.       JvmtiExport::post_resource_exhausted(  
1.         JVMTI_RESOURCE_EXHAUSTED_OOM_ERROR | JVMTI_RESOURCE_EXHAUSTED_JAVA_HEAP,  
1.         "Java heap space");  
1.     }  
1.   
1.     THROW_OOP_0(Universe::out_of_memory_error_java_heap());  
1.   } else {  
1.     // -XX:+HeapDumpOnOutOfMemoryError and -XX:OnOutOfMemoryError support  
1.     report_java_out_of_memory("GC overhead limit exceeded");  
1.   
1.     if (JvmtiExport::should_post_resource_exhausted()) {  
1.       JvmtiExport::post_resource_exhausted(  
1.         JVMTI_RESOURCE_EXHAUSTED_OOM_ERROR | JVMTI_RESOURCE_EXHAUSTED_JAVA_HEAP,  
1.         "GC overhead limit exceeded");  
1.     }  
1.   
1.     THROW_OOP_0(Universe::out_of_memory_error_gc_overhead_limit());  
1.   }  
1. }  
1.   
1. HeapWord* CollectedHeap::common_mem_allocate_init(size_t size, bool is_noref, TRAPS) {  
1.   HeapWord* obj = common_mem_allocate_noinit(size, is_noref, CHECK_NULL);  
1.   init_obj(obj, size);  
1.   return obj;  
1. }  
HeapWord* CollectedHeap::common_mem_allocate_noinit(size_t size, bool is_noref, TRAPS) { // Clear unhandled oops for memory allocation. Memory allocation might // not take out a lock if from tlab, so clear here. CHECK_UNHANDLED_OOPS_ONLY(THREAD->clear_unhandled_oops();) if (HAS_PENDING_EXCEPTION) { NOT_PRODUCT(guarantee(false, "Should not allocate with exception pending")); return NULL; // caller does a CHECK_0 too } // We may want to update this, is_noref objects might not be allocated in TLABs. HeapWord* result = NULL; if (UseTLAB) { result = CollectedHeap::allocate_from_tlab(THREAD, size); if (result != NULL) { assert(!HAS_PENDING_EXCEPTION, "Unexpected exception, will result in uninitialized storage"); return result; } } bool gc_overhead_limit_was_exceeded = false; result = Universe::heap()->mem_allocate(size, is_noref, false, &gc_overhead_limit_was_exceeded); if (result != NULL) { NOT_PRODUCT(Universe::heap()-> check_for_non_bad_heap_word_value(result, size)); assert(!HAS_PENDING_EXCEPTION, "Unexpected exception, will result in uninitialized storage"); THREAD->incr_allocated_bytes(size * HeapWordSize); return result; } if (!gc_overhead_limit_was_exceeded) { // -XX:+HeapDumpOnOutOfMemoryError and -XX:OnOutOfMemoryError support report_java_out_of_memory("Java heap space"); if (JvmtiExport::should_post_resource_exhausted()) { JvmtiExport::post_resource_exhausted( JVMTI_RESOURCE_EXHAUSTED_OOM_ERROR | JVMTI_RESOURCE_EXHAUSTED_JAVA_HEAP, "Java heap space"); } THROW_OOP_0(Universe::out_of_memory_error_java_heap()); } else { // -XX:+HeapDumpOnOutOfMemoryError and -XX:OnOutOfMemoryError support report_java_out_of_memory("GC overhead limit exceeded"); if (JvmtiExport::should_post_resource_exhausted()) { JvmtiExport::post_resource_exhausted( JVMTI_RESOURCE_EXHAUSTED_OOM_ERROR | JVMTI_RESOURCE_EXHAUSTED_JAVA_HEAP, "GC overhead limit exceeded"); } THROW_OOP_0(Universe::out_of_memory_error_gc_overhead_limit()); } } HeapWord* CollectedHeap::common_mem_allocate_init(size_t size, bool is_noref, TRAPS) { HeapWord* obj = common_mem_allocate_noinit(size, is_noref, CHECK_NULL); init_obj(obj, size); return obj; }
然后会先调用到：
hotspot/src/share/vm/gc_interface/collectedHeap.inline.hpp
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. HeapWord* CollectedHeap::allocate_from_tlab(Thread* thread, size_t size) {  
1.   assert(UseTLAB, "should use UseTLAB");  
1.   
1.   HeapWord* obj = thread->tlab().allocate(size);  
1.   if (obj != NULL) {  
1.     return obj;  
1.   }  
1.   // Otherwise...  
1.   return allocate_from_tlab_slow(thread, size);  
1. }  
HeapWord* CollectedHeap::allocate_from_tlab(Thread* thread, size_t size) { assert(UseTLAB, "should use UseTLAB"); HeapWord* obj = thread->tlab().allocate(size); if (obj != NULL) { return obj; } // Otherwise... return allocate_from_tlab_slow(thread, size); }
前面已经说了，TLAB的大小不足以分配楼主例子里的数组，所以thread->tlab().allocate()肯定会返回NULL表示无法分配，于是调用到：
hotspot/src/share/vm/gc_interface/collectedHeap.cpp
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. HeapWord* CollectedHeap::allocate_from_tlab_slow(Thread* thread, size_t size) {  
1.   
1.   // Retain tlab and allocate object in shared space if  
1.   // the amount free in the tlab is too large to discard.  
1.   if (thread->tlab().free() > thread->tlab().refill_waste_limit()) {  
1.     thread->tlab().record_slow_allocation(size);  
1.     return NULL;  
1.   }  
1.   // ...  
1. }  
HeapWord* CollectedHeap::allocate_from_tlab_slow(Thread* thread, size_t size) { // Retain tlab and allocate object in shared space if // the amount free in the tlab is too large to discard. if (thread->tlab().free() > thread->tlab().refill_waste_limit()) { thread->tlab().record_slow_allocation(size); return NULL; } // ... }
例子里TLAB几乎是空的，所以这个if判断肯定会通过，TLAB就会记录下一次slow allocation。从GC日志中第一次minor GC的TLAB统计信息可以看到3次slow allocation，其实就是头两个数组（5MB和10MB那两个）成功分配后，第三个数组（又一个5MB的）尝试在TLAB分配失败后打出的日志。
那么回到CollectedHeap::common_mem_allocate_noinit()，它会调用Universe::heap()->mem_allocate(...)进一步尝试分配，传入的is_noref为true，is_tlab为false：
hotspot/src/share/vm/gc_implementation/parallelScavengeHeap.cpp
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. // There are two levels of allocation policy here.  
1. //  
1. // When an allocation request fails, the requesting thread must invoke a VM  
1. // operation, transfer control to the VM thread, and await the results of a  
1. // garbage collection. That is quite expensive, and we should avoid doing it  
1. // multiple times if possible.  
1. //  
1. // To accomplish this, we have a basic allocation policy, and also a  
1. // failed allocation policy.  
1. //  
1. // The basic allocation policy controls how you allocate memory without  
1. // attempting garbage collection. It is okay to grab locks and  
1. // expand the heap, if that can be done without coming to a safepoint.  
1. // It is likely that the basic allocation policy will not be very  
1. // aggressive.  
1. //  
1. // The failed allocation policy is invoked from the VM thread after  
1. // the basic allocation policy is unable to satisfy a mem_allocate  
1. // request. This policy needs to cover the entire range of collection,  
1. // heap expansion, and out-of-memory conditions. It should make every  
1. // attempt to allocate the requested memory.  
1.   
1. // Basic allocation policy. Should never be called at a safepoint, or  
1. // from the VM thread.  
1. //  
1. // This method must handle cases where many mem_allocate requests fail  
1. // simultaneously. When that happens, only one VM operation will succeed,  
1. // and the rest will not be executed. For that reason, this method loops  
1. // during failed allocation attempts. If the java heap becomes exhausted,  
1. // we rely on the size_policy object to force a bail out.  
1. HeapWord* ParallelScavengeHeap::mem_allocate(  
1.                                      size_t size,  
1.                                      bool is_noref,  
1.                                      bool is_tlab,  
1.                                      bool* gc_overhead_limit_was_exceeded) {  
1.   assert(!SafepointSynchronize::is_at_safepoint(), "should not be at safepoint");  
1.   assert(Thread::current() != (Thread*)VMThread::vm_thread(), "should not be in vm thread");  
1.   assert(!Heap_lock->owned_by_self(), "this thread should not own the Heap_lock");  
1.   
1.   // In general gc_overhead_limit_was_exceeded should be false so  
1.   // set it so here and reset it to true only if the gc time  
1.   // limit is being exceeded as checked below.  
1.   *gc_overhead_limit_was_exceeded = false;  
1.   
1.   HeapWord* result = young_gen()->allocate(size, is_tlab);  
1.   
1.   uint loop_count = 0;  
1.   uint gc_count = 0;  
1.   
1.   while (result == NULL) {  
1.     // We don't want to have multiple collections for a single filled generation.  
1.     // To prevent this, each thread tracks the total_collections() value, and if  
1.     // the count has changed, does not do a new collection.  
1.     //  
1.     // The collection count must be read only while holding the heap lock. VM  
1.     // operations also hold the heap lock during collections. There is a lock  
1.     // contention case where thread A blocks waiting on the Heap_lock, while  
1.     // thread B is holding it doing a collection. When thread A gets the lock,  
1.     // the collection count has already changed. To prevent duplicate collections,  
1.     // The policy MUST attempt allocations during the same period it reads the  
1.     // total_collections() value!  
1.     {  
1.       MutexLocker ml(Heap_lock);  
1.       gc_count = Universe::heap()->total_collections();  
1.   
1.       result = young_gen()->allocate(size, is_tlab);  
1.   
1.       // (1) If the requested object is too large to easily fit in the  
1.       //     young_gen, or  
1.       // (2) If GC is locked out via GCLocker, young gen is full and  
1.       //     the need for a GC already signalled to GCLocker (done  
1.       //     at a safepoint),  
1.       // ... then, rather than force a safepoint and (a potentially futile)  
1.       // collection (attempt) for each allocation, try allocation directly  
1.       // in old_gen. For case (2) above, we may in the future allow  
1.       // TLAB allocation directly in the old gen.  
1.       if (result != NULL) {  
1.         return result;  
1.       }  
1.       if (!is_tlab &&  
1.           size >= (young_gen()->eden_space()->capacity_in_words(Thread::current()) / 2)) {  
1.         result = old_gen()->allocate(size, is_tlab);  
1.         if (result != NULL) {  
1.           return result;  
1.         }  
1.       }  
1.       if (GC_locker::is_active_and_needs_gc()) {  
1.         // GC is locked out. If this is a TLAB allocation,  
1.         // return NULL; the requestor will retry allocation  
1.         // of an idividual object at a time.  
1.         if (is_tlab) {  
1.           return NULL;  
1.         }  
1.   
1.         // If this thread is not in a jni critical section, we stall  
1.         // the requestor until the critical section has cleared and  
1.         // GC allowed. When the critical section clears, a GC is  
1.         // initiated by the last thread exiting the critical section; so  
1.         // we retry the allocation sequence from the beginning of the loop,  
1.         // rather than causing more, now probably unnecessary, GC attempts.  
1.         JavaThread* jthr = JavaThread::current();  
1.         if (!jthr->in_critical()) {  
1.           MutexUnlocker mul(Heap_lock);  
1.           GC_locker::stall_until_clear();  
1.           continue;  
1.         } else {  
1.           if (CheckJNICalls) {  
1.             fatal("Possible deadlock due to allocating while"  
1.                   " in jni critical section");  
1.           }  
1.           return NULL;  
1.         }  
1.       }  
1.     }  
1.   
1.     if (result == NULL) {  
1.   
1.       // Generate a VM operation  
1.       VM_ParallelGCFailedAllocation op(size, is_tlab, gc_count);  
1.       VMThread::execute(&op);  
1.   
1.       // Did the VM operation execute? If so, return the result directly.  
1.       // This prevents us from looping until time out on requests that can  
1.       // not be satisfied.  
1.       if (op.prologue_succeeded()) {  
1.         assert(Universe::heap()->is_in_or_null(op.result()),  
1.           "result not in heap");  
1.   
1.         // If GC was locked out during VM operation then retry allocation  
1.         // and/or stall as necessary.  
1.         if (op.gc_locked()) {  
1.           assert(op.result() == NULL, "must be NULL if gc_locked() is true");  
1.           continue;  // retry and/or stall as necessary  
1.         }  
1.   
1.         // Exit the loop if the gc time limit has been exceeded.  
1.         // The allocation must have failed above ("result" guarding  
1.         // this path is NULL) and the most recent collection has exceeded the  
1.         // gc overhead limit (although enough may have been collected to  
1.         // satisfy the allocation).  Exit the loop so that an out-of-memory  
1.         // will be thrown (return a NULL ignoring the contents of  
1.         // op.result()),  
1.         // but clear gc_overhead_limit_exceeded so that the next collection  
1.         // starts with a clean slate (i.e., forgets about previous overhead  
1.         // excesses).  Fill op.result() with a filler object so that the  
1.         // heap remains parsable.  
1.         const bool limit_exceeded = size_policy()->gc_overhead_limit_exceeded();  
1.         const bool softrefs_clear = collector_policy()->all_soft_refs_clear();  
1.         assert(!limit_exceeded || softrefs_clear, "Should have been cleared");  
1.         if (limit_exceeded && softrefs_clear) {  
1.           *gc_overhead_limit_was_exceeded = true;  
1.           size_policy()->set_gc_overhead_limit_exceeded(false);  
1.           if (PrintGCDetails && Verbose) {  
1.             gclog_or_tty->print_cr("ParallelScavengeHeap::mem_allocate: "  
1.               "return NULL because gc_overhead_limit_exceeded is set");  
1.           }  
1.           if (op.result() != NULL) {  
1.             CollectedHeap::fill_with_object(op.result(), size);  
1.           }  
1.           return NULL;  
1.         }  
1.   
1.         return op.result();  
1.       }  
1.     }  
1.   
1.     // The policy object will prevent us from looping forever. If the  
1.     // time spent in gc crosses a threshold, we will bail out.  
1.     loop_count++;  
1.     if ((result == NULL) && (QueuedAllocationWarningCount > 0) &&  
1.         (loop_count % QueuedAllocationWarningCount == 0)) {  
1.       warning("ParallelScavengeHeap::mem_allocate retries %d times \n\t"  
1.               " size=%d %s", loop_count, size, is_tlab ? "(TLAB)" : "");  
1.     }  
1.   }  
1.   
1.   return result;  
1. }  
// There are two levels of allocation policy here. // // When an allocation request fails, the requesting thread must invoke a VM // operation, transfer control to the VM thread, and await the results of a // garbage collection. That is quite expensive, and we should avoid doing it // multiple times if possible. // // To accomplish this, we have a basic allocation policy, and also a // failed allocation policy. // // The basic allocation policy controls how you allocate memory without // attempting garbage collection. It is okay to grab locks and // expand the heap, if that can be done without coming to a safepoint. // It is likely that the basic allocation policy will not be very // aggressive. // // The failed allocation policy is invoked from the VM thread after // the basic allocation policy is unable to satisfy a mem_allocate // request. This policy needs to cover the entire range of collection, // heap expansion, and out-of-memory conditions. It should make every // attempt to allocate the requested memory. // Basic allocation policy. Should never be called at a safepoint, or // from the VM thread. // // This method must handle cases where many mem_allocate requests fail // simultaneously. When that happens, only one VM operation will succeed, // and the rest will not be executed. For that reason, this method loops // during failed allocation attempts. If the java heap becomes exhausted, // we rely on the size_policy object to force a bail out. HeapWord* ParallelScavengeHeap::mem_allocate( size_t size, bool is_noref, bool is_tlab, bool* gc_overhead_limit_was_exceeded) { assert(!SafepointSynchronize::is_at_safepoint(), "should not be at safepoint"); assert(Thread::current() != (Thread*)VMThread::vm_thread(), "should not be in vm thread"); assert(!Heap_lock->owned_by_self(), "this thread should not own the Heap_lock"); // In general gc_overhead_limit_was_exceeded should be false so // set it so here and reset it to true only if the gc time // limit is being exceeded as checked below. *gc_overhead_limit_was_exceeded = false; HeapWord* result = young_gen()->allocate(size, is_tlab); uint loop_count = 0; uint gc_count = 0; while (result == NULL) { // We don't want to have multiple collections for a single filled generation. // To prevent this, each thread tracks the total_collections() value, and if // the count has changed, does not do a new collection. // // The collection count must be read only while holding the heap lock. VM // operations also hold the heap lock during collections. There is a lock // contention case where thread A blocks waiting on the Heap_lock, while // thread B is holding it doing a collection. When thread A gets the lock, // the collection count has already changed. To prevent duplicate collections, // The policy MUST attempt allocations during the same period it reads the // total_collections() value! { MutexLocker ml(Heap_lock); gc_count = Universe::heap()->total_collections(); result = young_gen()->allocate(size, is_tlab); // (1) If the requested object is too large to easily fit in the // young_gen, or // (2) If GC is locked out via GCLocker, young gen is full and // the need for a GC already signalled to GCLocker (done // at a safepoint), // ... then, rather than force a safepoint and (a potentially futile) // collection (attempt) for each allocation, try allocation directly // in old_gen. For case (2) above, we may in the future allow // TLAB allocation directly in the old gen. if (result != NULL) { return result; } if (!is_tlab && size >= (young_gen()->eden_space()->capacity_in_words(Thread::current()) / 2)) { result = old_gen()->allocate(size, is_tlab); if (result != NULL) { return result; } } if (GC_locker::is_active_and_needs_gc()) { // GC is locked out. If this is a TLAB allocation, // return NULL; the requestor will retry allocation // of an idividual object at a time. if (is_tlab) { return NULL; } // If this thread is not in a jni critical section, we stall // the requestor until the critical section has cleared and // GC allowed. When the critical section clears, a GC is // initiated by the last thread exiting the critical section; so // we retry the allocation sequence from the beginning of the loop, // rather than causing more, now probably unnecessary, GC attempts. JavaThread* jthr = JavaThread::current(); if (!jthr->in_critical()) { MutexUnlocker mul(Heap_lock); GC_locker::stall_until_clear(); continue; } else { if (CheckJNICalls) { fatal("Possible deadlock due to allocating while" " in jni critical section"); } return NULL; } } } if (result == NULL) { // Generate a VM operation VM_ParallelGCFailedAllocation op(size, is_tlab, gc_count); VMThread::execute(&op); // Did the VM operation execute? If so, return the result directly. // This prevents us from looping until time out on requests that can // not be satisfied. if (op.prologue_succeeded()) { assert(Universe::heap()->is_in_or_null(op.result()), "result not in heap"); // If GC was locked out during VM operation then retry allocation // and/or stall as necessary. if (op.gc_locked()) { assert(op.result() == NULL, "must be NULL if gc_locked() is true"); continue; // retry and/or stall as necessary } // Exit the loop if the gc time limit has been exceeded. // The allocation must have failed above ("result" guarding // this path is NULL) and the most recent collection has exceeded the // gc overhead limit (although enough may have been collected to // satisfy the allocation). Exit the loop so that an out-of-memory // will be thrown (return a NULL ignoring the contents of // op.result()), // but clear gc_overhead_limit_exceeded so that the next collection // starts with a clean slate (i.e., forgets about previous overhead // excesses). Fill op.result() with a filler object so that the // heap remains parsable. const bool limit_exceeded = size_policy()->gc_overhead_limit_exceeded(); const bool softrefs_clear = collector_policy()->all_soft_refs_clear(); assert(!limit_exceeded || softrefs_clear, "Should have been cleared"); if (limit_exceeded && softrefs_clear) { *gc_overhead_limit_was_exceeded = true; size_policy()->set_gc_overhead_limit_exceeded(false); if (PrintGCDetails && Verbose) { gclog_or_tty->print_cr("ParallelScavengeHeap::mem_allocate: " "return NULL because gc_overhead_limit_exceeded is set"); } if (op.result() != NULL) { CollectedHeap::fill_with_object(op.result(), size); } return NULL; } return op.result(); } } // The policy object will prevent us from looping forever. If the // time spent in gc crosses a threshold, we will bail out. loop_count++; if ((result == NULL) && (QueuedAllocationWarningCount > 0) && (loop_count % QueuedAllocationWarningCount == 0)) { warning("ParallelScavengeHeap::mem_allocate retries %d times \n\t" " size=%d %s", loop_count, size, is_tlab ? "(TLAB)" : ""); } } return result; }
这里会先尝试在young gen里分配
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. // Allocation  
1. HeapWord* allocate(size_t word_size, bool is_tlab) {  
1.   HeapWord* result = eden_space()->cas_allocate(word_size);  
1.   return result;  
1. }  
// Allocation HeapWord* allocate(size_t word_size, bool is_tlab) { HeapWord* result = eden_space()->cas_allocate(word_size); return result; }
实际上就是尝试在eden里做一次CAS分配。楼主例子里的头两个数组都会在这里分配成功，把eden占用到99%。然后到第三个数组触发GC前试图分配的时候，eden已经没有足够空间了，分配就会失败从而返回NULL。
然后就开始进入“试图分配 -> 不行的话GC -> 再试图分配 -> 不行的话再GC -> 再试图分配”的循环。
做了第一次minor GC后，头两个数组都被GC掉了，eden为空，于是再试图在young gen里分配第三个数组（5MB那个）就成功了。第四个数组（10MB那个）分配的状况与第二个数组类似，都不需要另外触发GC就可以成功。
等到要分配第5个数组（15MB那个）时，堆的状况跟要分配第三个数组时类似，eden都是99%满，所以楼主可能会以为这个数组也会在GC后分配到eden里。实际状况却是它在young gen尝试分配失败后，真的进入GC前还会试图分配，来到ParallelScavengeHeap::mem_allocate()的这个分支：
C++代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. if (!is_tlab &&  
1.     size >= (young_gen()->eden_space()->capacity_in_words(Thread::current()) / 2)) {  
1.   result = old_gen()->allocate(size, is_tlab);  
1.   if (result != NULL) {  
1.     return result;  
1.   }  
1. }  
if (!is_tlab && size >= (young_gen()->eden_space()->capacity_in_words(Thread::current()) / 2)) { result = old_gen()->allocate(size, is_tlab); if (result != NULL) { return result; } }
此时eden的capacity有15MB多，一半就是不到8MB，第5个数组有15MB多一点，肯定超过了这个判断条件的限制，于是就直接在old gen尝试分配，成功，就不需要做GC，因而之前的第3和第4个数组就都还在。
等待雨季的到来 写道

1、byte5为什么没有触发minor gc而是直接分配在老生代，byte3和byte4为什么没有被回收？
总结一下过程：
第1个数组：
大小5MB，慢速路径上在young gen分配直接成功；
第2个数组：
大小10MB，慢速路径上在young gen分配直接成功；
第3个数组：
大小5MB，慢速路径上在young gen分配失败，
（进入循环）再尝试在young gen分配，还是失败，
没达到去old gen分配的大小限制，
于是真的触发一次minor GC，eden清空，
再尝试在young gen分配，成功；
第4个数组：
大小10MB，慢速路径上在young gen分配直接成功；
第5个数组：
大小15MB，慢速路径上在young gen分配失败，
（进入循环）再尝试在young gen分配，还是失败，
达到了去old gen直接分配的大小限制，在old gen上分配成功。没有触发GC。
=========================================================
等待雨季的到来 写道

2、eden区什么时候才会自动扩容到40M，当创建超过20M的对象时，都是直接分配在了老生，而创建大小超过老生代的对象时老生代会自动扩容。
上面的过程已经很清楚了。如果创建20MB的对象，它在eden分配会失败（此时只有15MB多一点的容量），然后又满足了直接在old gen分配的条件（试图分配的大小大于eden的capacity的一半），就跑到old gen分配了。
如果是比较正常的Java程序，创建的多数对象都是小对象，都通过TLAB来分配，那就可以观察到eden经过几次GC后渐渐扩容到最大大小。
=========================================================
等待雨季的到来 写道

其他：
我在用eclipse调试的时候发现，同样的测试代码在执行的时候输出的GC log居然会有很大的变化，有的时候eden区被初始化为8M有的时候初始化为20M。
这个问题我就不太清楚了。通过Eclipse来调试Java程序，在目标Java程序里会产生一些额外的垃圾，我不太肯定这跟您看到的抖动有没有关系。
![Spinner]() [![等待雨季的到来的博客]( "等待雨季的到来的博客: ")](http://zsl8544-163-com.iteye.com/) [等待雨季的到来](http://zsl8544-163-com.iteye.com/ "等待雨季的到来") 前天

实在是太感谢R大神百忙之中能给出这么细致入微的解释了！受益匪浅！以前我的认识只是eden区会在GC之后分配内存，并不知道如果分配内存大于eden区capacity一半就会直接在old gen分配的规则。
只是由于我对TLAB的具体细节只知甚少，对您下面的eden扩容的解释还是不是太清楚。
引用

如果是比较正常的Java程序，创建的多数对象都是小对象，都通过TLAB来分配，那就可以观察到eden经过几次GC后渐渐扩容到最大大小。
为什么说创建小对象，通过TLAB来分配，eden就会渐渐扩容到最大了呢？
还有一个比较傻的问题，内存区域扩容之后还会再缩小吗？有内存扩容这个设计的原因是不是capacity小得内存区域收集的会更快呢？
![Spinner]() [![等待雨季的到来的博客]( "等待雨季的到来的博客: ")](http://zsl8544-163-com.iteye.com/) [等待雨季的到来](http://zsl8544-163-com.iteye.com/ "等待雨季的到来") 前天

对了，我没有看毕玄的实验，能给个链接吗？
我看的是您推荐的周志明老师写的深入Java虚拟机。
![Spinner]() [![IcyFenix的博客]( "IcyFenix的博客: FenixSoft 3.0")](http://icyfenix.iteye.com/) [IcyFenix](http://icyfenix.iteye.com/ "IcyFenix") 昨天

等待雨季的到来 写道

实在是太感谢R大神百忙之中能给出这么细致入微的解释了！受益匪浅！以前我的认识只是eden区会在GC之后分配内存，并不知道如果分配内存大于eden区capacity一半就会直接在old gen分配的规则。
只是由于我对TLAB的具体细节只知甚少，对您下面的eden扩容的解释还是不是太清楚。
引用

如果是比较正常的Java程序，创建的多数对象都是小对象，都通过TLAB来分配，那就可以观察到eden经过几次GC后渐渐扩容到最大大小。
为什么说创建小对象，通过TLAB来分配，eden就会渐渐扩容到最大了呢？
还有一个比较傻的问题，内存区域扩容之后还会再缩小吗？有内存扩容这个设计的原因是不是capacity小得内存区域收集的会更快呢？
赞R大的详细解答。我被召唤来回答楼上的2个问题“eden是如何扩容的？”，“eden扩容之后还会缩小吗？”
也按照上面一步一步分析代码来解释，但过程有点多，我就只帖方法名，具体代码不粘上来了，代码可以到openjdk的repo上直接看。
1.遇到分配内存指令（InterpreterRuntime中的new或者newarray这些字节码模版）
2.开始分配内存（前面R大写的分配数组oopFactory::new_typeArray或者分配对象instanceKlass::allocate_instance）
3.向CollectedHeap请求内存（入口是CollectedHeap::array_allocate或者CollectedHeap::obj_allocate，两者最终走到CollectedHeap::common_mem_allocate_noinit方法中）
4.如果能够通过TLAB顺利分配（allocate_from_tlab方法）就在TLAB上划
5.否则，通过mem_allocate方法在eden上划。这个是纯虚方法，因此要看CollectedHeap的子类ParallelScavengeHeap（LZ使用PS收集器）
6.在men_allocate方法中，先通过cas_allocate在MutableSpace上划，不成功的话，堕入GC-尝试分配-GC-尝试分配……的循环中，这个R大在前面讲的很详细，其他就不多说了，着重讲GC。
7.进入GC的过程不是直接方法调用，是通过发送VM_ParallelGCFailedAllocation信号给VM线程触发的，最终转到ParallelScavengeHeap::failed_mem_allocate()方法中。
8.该通过PSScavenge::invoke()开始GC过程，实际GC的活是在PSScavenge::invoke_no_policy()做的。
9.invoke_no_policy()方法可以看到，实际上每次GC都会重置eden大小，即调用了ParallelScavengeHeap::resize_young_gen()方法，eden是否会真正改变，取决于这个方法传入的参数，如果和eden的原本大小不一样，那就相当于增加或者缩小了eden了。
10.这里关系有点烦，补一段说清楚一些。resize_young_gen()方法的参数值就是PSAdaptiveSizePolicy::calculated_eden_size_in_bytes()返回值，换句话说，GC是否要改变eden大小，完全是由PSAdaptiveSizePolicy控制的（这个方法在它的父类AdaptiveSizePolicy中，但calculated_eden_size_in_bytes只是简单返回一个数值，数值是在子类中计算的，PSScavenge::invoke_no_policy()中调用了PSAdaptiveSizePolicy::compute_generation_free_space()去改变这个数值）。
11.到这里，eden如何改变就取决于PSAdaptiveSizePolicy::compute_generation_free_space()的计算结果了。PS收集器的特点是可以设置一些目标策略，譬如追求吞吐量，追求最小停顿时间等，这个方法会根据用户选定的目标，调用adjust_for_minor_pause_time()、adjust_eden_for_footprint()、adjust_for_pause_time()等方法去计算eden的值，具体就不一个一个细说了。即使不详细看方法内容，光从eden_increment_with_supplement_aligned_up、eden_decrement_aligned_down这些明显含有“increment”、“decrement”的方法名字，也可以回答“内存区域扩容之后还会再缩小吗？”这个问题了。
PS：如果对这个确实感兴趣，可以加个PrintAdaptiveSizePolicy，在每次重新计算内存区域大小时输出一些信息看看。
PSS：我把自己的理解写完了，但也木有搞清楚PS收集器eden的大小计算，和TLAB分配的联系在哪里，请R大继续讲解。
![Spinner]()

![]() [发表回复](http://hllvm.group.iteye.com/group/topic/38293/post/new)

[>>返回群组首页](http://hllvm.group.iteye.com/)

[![高级语言虚拟机群组]( "高级语言虚拟机: 关注各种高级语言虚拟机（high-level language virtual machine，HLL VM）的设计与实现，泛化至各种高级语言的运行时的设计与实现。讨论范围包括JVM、CLI、Parrot等当前流行的VM平台，也包括Python、Ruby、JavaScript、Lua、Perl、Forth、Smalltalk等众多语言的引擎，还有历史上有影响的各种高级语言虚拟机，如SECD等。")](http://hllvm.group.iteye.com/)

### 相关讨论

* [JVM内存管理：深入垃圾收集器与内存分配策略](http://www.iteye.com/topic/802638)
* [java内存管理以及GC](http://www.iteye.com/topic/976522)
* [HotSpot VM 内存堆的两个Servivor区](http://www.iteye.com/topic/894148)
* [jvm crash,疑似GC的bug](http://www.iteye.com/topic/814815)
* [借助HotSpot SA来一窥PermGen上的对象](http://www.iteye.com/topic/730461)
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
百联优力(北京)投资有限公司 版权所有 ![](http://stat.iteye.com/?url=http%3A%2F%2Fhllvm.group.iteye.com%2Fgroup%2Ftopic%2F38293&referrer=http%3A%2F%2Fhllvm.group.iteye.com%2F&user_id=)
