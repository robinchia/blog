---
layout: post
title: "借助HotSpot SA来一窥PermGen上的对象"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 优化
--- 

# 借助HotSpot SA来一窥PermGen上的对象

[您还未登录 !](http://www.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://www.javaeye.com/login) [注册](http://www.javaeye.com/signup)

[![JavaEye-最棒的软件开发交流社区]( "JavaEye-最棒的软件开发交流社区")](http://www.javaeye.com/)

[论坛首页](http://www.javaeye.com/forums) → [Java编程和Java企业应用版](http://www.javaeye.com/forums/board/Java) → [Java综合](http://www.javaeye.com/forums/tag/Java) →

# 借助HotSpot SA来一窥PermGen上的对象

[全部](http://www.javaeye.com/forums/board/Java) [Hibernate](http://www.javaeye.com/forums/tag/Hibernate) [Spring](http://www.javaeye.com/forums/tag/Spring) [Struts](http://www.javaeye.com/forums/tag/Struts) [iBATIS](http://www.javaeye.com/forums/tag/iBATIS) [企业应用](http://www.javaeye.com/forums/tag/J2EE) [设计模式](http://www.javaeye.com/forums/tag/Design-Pattern) [DAO](http://www.javaeye.com/forums/tag/DAO) [领域模型](http://www.javaeye.com/forums/tag/Object-Domain) [OO](http://www.javaeye.com/forums/tag/OO) [Tomcat](http://www.javaeye.com/forums/tag/Tomcat) [SOA](http://www.javaeye.com/forums/tag/SOA) [JBoss](http://www.javaeye.com/forums/tag/JBoss) [Swing](http://www.javaeye.com/forums/tag/Swing) [Java综合](http://www.javaeye.com/forums/tag/Java)
[](http://www.javaeye.com/forums/39/topics/730461/posts/new "发表回复")

[myApps快速开发平台，配置即开发、所见即所得，节约85%工作量](http://www.javaeye.com/clicks/324)

« 上一页 1 [2](http://www.javaeye.com/topic/730461?page=2) [下一页 »](http://www.javaeye.com/topic/730461?page=2)
浏览 2354 次
 [主题：借助HotSpot SA来一窥PermGen上的对象](http://www.javaeye.com/topic/730461)

**该帖已经被评为精华帖** 作者 正文 * RednaxelaFX
* 等级: ![一钻会员]( "一钻会员")
* [![RednaxelaFX的博客]( "RednaxelaFX的博客: Script Ahead, Code Behind")](http://rednaxelafx.javaeye.com/)
* 文章: 385
* 积分: 900
* 来自: 杭州
* ![]()
    发表时间：2010-08-05   最后修改：前天

[<](http://www.javaeye.com/topic/730461#) [>](http://www.javaeye.com/topic/730461#) 猎头职位:

相关文章: [ ](http://www.javaeye.com/topic/730461# "关闭")

* [问题的产生(解决)??](http://www.javaeye.com/topic/247391 "问题的产生(解决)??")
* [分析java.lang.OutOfMemoryError: PermGen space](http://www.javaeye.com/topic/80620 "分析java.lang.OutOfMemoryError: PermGen space")
* [IKVM 编程武林之.NET派的北冥神功](http://www.javaeye.com/topic/555998 "IKVM 编程武林之.NET派的北冥神功")
* [一次Java垃圾收集调优实战](http://www.javaeye.com/topic/212967 "一次Java垃圾收集调优实战")
推荐圈子: [高级语言虚拟机](http://hllvm.group.javaeye.com/)
[更多相关推荐](http://www.javaeye.com/wiki/topic/730461)
（Disclaimer：如果需要转载请先与我联系；
作者：RednaxelaFX -> rednaxelafx.javaeye.com）
接着[昨天的](http://rednaxelafx.javaeye.com/blog/727938)与[前天的帖](http://rednaxelafx.javaeye.com/blog/729214)，今天也来介绍一个[HotSpot](http://openjdk.java.net/groups/hotspot/)的[Serviceability Agent](http://openjdk.java.net/groups/hotspot/docs/Serviceability.html)（以下简称SA）的玩法例子。
昨天用SA把x86机器码反汇编到汇编代码，或许对多数Java程序员来说并不怎么有趣。那么今天就来点更接近Java，但又经常被误解的话题——HotSpot的GC堆的permanent generation。
要用SA里最底层的API来连接上一个Java进程并不困难，不过SA还提供了更方便的封装：只要继承 *sun.jvm.hotspot.tools.Tool* 并实现一个 *run()* 方法，在该方法内使用SA的API访问JVM即可。
这次我们就把一个跑在HotSpot上的Java进程的perm gen里所有对象的信息打到标准输出流上看看吧。
测试环境是32位Linux，x86，Sun JDK 6 update 2
（手边可用的JDK版本很多，随便拿了一个来用，呵呵 >_<）
代码如下：
import sun.jvm.hotspot.gc_implementation.parallelScavenge.PSPermGen; import sun.jvm.hotspot.gc_implementation.parallelScavenge.ParallelScavengeHeap; import sun.jvm.hotspot.gc_implementation.shared.MutableSpace; import sun.jvm.hotspot.gc_interface.CollectedHeap; import sun.jvm.hotspot.memory.Universe; import sun.jvm.hotspot.oops.HeapPrinter; import sun.jvm.hotspot.oops.HeapVisitor; import sun.jvm.hotspot.oops.ObjectHeap; import sun.jvm.hotspot.runtime.VM; import sun.jvm.hotspot.tools.Tool; /** * @author sajia * */ public class TestPrintPSPermGen extends Tool { public static void main(String[] args) { TestPrintPSPermGen test = new TestPrintPSPermGen(); test.start(args); test.stop(); } @Override public void run() { VM vm = VM.getVM(); Universe universe = vm.getUniverse(); CollectedHeap heap = universe.heap(); puts("GC heap name: " + heap.kind()); if (heap instanceof ParallelScavengeHeap) { ParallelScavengeHeap psHeap = (ParallelScavengeHeap) heap; PSPermGen perm = psHeap.permGen(); MutableSpace permObjSpace = perm.objectSpace(); puts("Perm gen: [" + permObjSpace.bottom() + ", " + permObjSpace.end() + ")"); long permSize = 0; for (VM.Flag f : VM.getVM().getCommandLineFlags()) { if ("PermSize".equals(f.getName())) { permSize = Long.parseLong(f.getValue()); break; } } puts("PermSize: " + permSize); } puts(); ObjectHeap objHeap = vm.getObjectHeap(); HeapVisitor heapVisitor = new HeapPrinter(System.out); objHeap.iteratePerm(heapVisitor); } private static void puts() { System.out.println(); } private static void puts(String s) { System.out.println(s); } }
很简单，假定目标Java进程用的是Parallel Scavenge（PS）算法的GC堆，输出GC堆的名字，当前perm gen的起始和结束地址，VM参数中设置的PermSize（perm gen的初始大小）；然后是perm gen中所有对象的信息，包括对象摘要、地址、每个成员域的名字、偏移量和值等。
对HotSpot的VM参数不熟悉的同学可以留意一下几个参数在HotSpot源码中的定义：
product(ccstrlist, OnOutOfMemoryError, "", "Run user-defined commands on first java.lang.OutOfMemoryError") product(bool, UseParallelGC, false, "Use the Parallel Scavenge garbage collector") product_pd(uintx, PermSize, "Initial size of permanent generation (in bytes)") product_pd(uintx, MaxPermSize, "Maximum size of permanent generation (in bytes)")
要让SA连接到一个正在运行的Java进程最重要是提供进程ID。获取pid的方法有很多，今天演示的是利用OnOutOfMemoryError参数指定让HotSpot在遇到内存不足而抛出OutOfMemoryError时执行一段用户指定的命令；在这个命令中可以使用%p占位符表示pid，HotSpot在执行命令时会把真实pid填充进去。
然后来造一个引发OOM的导火索：
public class Foo { public static void main(String[] args) { Long[] array = new Long[256*1024*1024]; } }
对32位HotSpot来说，main()方法里的new Long[256*1024*1024]会试图创建一个大于1GB的数组对象，那么只要把-Xmx参数设到1GB或更小便足以引发OOM了。
如何知道这个数组对象会占用超过1GB的呢？Long[]是一个引用类型的数组，只要知道HotSpot中采用的对象布局：
----------------------- （+0） | _mark | ----------------------- （+4） | _metadata | ----------------------- （+8） | 数组长度 length | ----------------------- （+12+4*0） | 下标为0的元素 | ----------------------- （+12+4*1） | 下标为1的元素 | ----------------------- | ... | ----------------------- （+12+4*n） | 下标为n的元素 | ----------------------- | ... | -----------------------
就知道一大半了～
跑一下Foo程序。留意到依赖SA的代码要编译的话需要$JAVA_HOME/lib/sa-jdi.jar在classpath上，执行时同理。指定GC算法为Parallel Scavenge，并指定GC堆（不包括perm gen）的初始和最大值都为1GB：
[sajia@sajia ~]$ java -server -version java version "1.6.0_02" Java(TM) SE Runtime Environment (build 1.6.0_02-b05) Java HotSpot(TM) Server VM (build 1.6.0_02-b05, mixed mode) [sajia@sajia ~]$ javac Foo.java [sajia@sajia ~]$ javac -classpath ".:$JAVA_HOME/lib/sa-jdi.jar" TestPrintPSPermGen.java [sajia@sajia ~]$ java -server -XX:+UseParallelGC -XX:OnOutOfMemoryError='java -cp $JAVA_HOME/lib/sa-jdi.jar:. TestPrintPSPermGen %p > foo.txt' -Xms1g -Xmx1g Foo # # java.lang.OutOfMemoryError: Java heap space # -XX:OnOutOfMemoryError="java -cp $JAVA_HOME/lib/sa-jdi.jar:. TestPrintPSPermGen %p > foo.txt" # Executing /bin/sh -c "java -cp $JAVA_HOME/lib/sa-jdi.jar:. TestPrintPSPermGen 23373 > foo.txt"... Attaching to process ID 23373, please wait... Debugger attached successfully. Server compiler detected. JVM version is 1.6.0_02-b05 Exception in thread "main" java.lang.OutOfMemoryError: Java heap space at Foo.main(Foo.java:5) [sajia@sajia ~]$
得到的foo.txt就是要演示的输出结果。把它压缩了放在附件里，有兴趣但懒得自己实验的同学也可以观摩一下～
在foo.txt的开头可以看到：
GC heap name: ParallelScavengeHeap Perm gen: [0x70e60000, 0x71e60000) PermSize: 16777216
这里显示了GC堆确实是Parallel Scavenge的，其中perm gen当前的起始地址为0x70e60000，结束地址为0x71e60000，中间连续的虚拟内存空间都分配给perm gen使用。简单计算一下可知perm gen大小为16MB，与下面打出的PermSize参数的值完全吻合。
通过阅读该日志文件，可以得知HotSpot在perm gen里存放的对象主要有：
- Klass系对象
- java.lang.Class对象
- 字符串常量
- 符号（Symbol/symbolOop）常量
- 常量池对象
- 方法对象
等等，以及它们所直接依赖的一些对象。具体这些都是什么留待以后有空再写。
接下来挑几个例子来简单讲解一下如何阅读这个日志文件里的对象描述。
首先看一个String对象。先看看JDK里java.lang.String对象的声明是什么样的：
package java.lang; // imports ... public final class String implements java.io.Serializable, Comparable<String>, CharSequence { /** The value is used for character storage. */ private final char value[]; /** The offset is the first index of the storage that is used. */ private final int offset; /** The count is the number of characters in the String. */ private final int count; /** Cache the hash code for the string */ private int hash; // Default to 0 /** use serialVersionUID from JDK 1.0.2 for interoperability */ private static final long serialVersionUID = -6849794470754667710L; private static final ObjectStreamField[] serialPersistentFields = new ObjectStreamField[0]; public static final Comparator<String> CASE_INSENSITIVE_ORDER = new CaseInsensitiveComparator(); private static class CaseInsensitiveComparator implements Comparator<String>, java.io.Serializable { // ... } }
留意到String对象有4个成员域，分别是：
**名字** **类型** **引用类型还是值类型** value char[] 引用类型 offset int 值类型 count int 值类型 hash int 值类型
String类自身有三个静态变量，分别是：
**名字** **类型** **引用类型还是值类型** **备注** serialVersionUID long 值类型 常量 serialPersistentFields java.io.ObjectStreamField[] 引用类型 只读变量 CASE_INSENSITIVE_ORDER java.lang.String.CaseInsensitiveComparator 引用类型 只读变量
回到我们的foo.txt日志文件来看一个String的对象实例：
"main" @ 0x7100b140 (object size = 24) - _mark: {0} :1 - _klass: {4} :InstanceKlass for java/lang/String @ 0x70e6c6a0 - value: {8} :[C @ 0x7100b158 - offset: {12} :0 - count: {16} :4 - hash: {20} :0
这是在HotSpot的字符串池里的一个字符串常量对象，"main"。
日志中的“"main"”是对象的摘要，String对象有特别处理显示为它的内容，其它多数类型的对象都是显示类型名之类的。
在@符号之后的就是对象的起始地址，十六进制表示。
紧接着后面是对象占用GC堆的大小。很明显这个String对象自身占用了24字节。这里强调是“占用”的大小是因为对象除了存储必要的数据需要空间外，为了满足数据对齐的要求可能会有一部分空间作为填充数据而空占着。
String在内存中的布局是：
----------------------- （+0） | _mark | ----------------------- （+4） | _metadata | ----------------------- （+8） | value | ----------------------- （+12）| offset | ----------------------- （+16）| count | ----------------------- （+20）| hash | -----------------------
32位HotSpot上要求64位/8字节对齐，String占用的24字节正好全部都是有效数据，不需要填充空数据。
上面的String实例在内存中的实际数据如下：
**偏移量（字节）** **数值（二进制表示）** **数值（十六进制表示）** **宽度（位/字节）**   +0 00000000000000000000000000000001 00000001 32位/4字节   +4 01110000111001101100011010100000 70e6c6a0 32位/4字节   +8 01110001000000001011000101011000 7100b158 32位/4字节 +12 00000000000000000000000000000000 00000000 32位/4字节 +16 00000000000000000000000000000100 00000004 32位/4字节 +20 00000000000000000000000000000000 00000000 32位/4字节
OK，那我们来每个成员域都过一遍，看看有何玄机。
第一个是_mark。在HotSpot的C++代码里它的类型是markOop，在SA里以sun.jvm.hotspot.oops.Mark来表现。
它属于对象头（object header）的一部分，是个多用途标记，可用于记录GC的标记（mark）状态、锁状态、偏向锁（bias-locking）状态、身份哈希值（identity hash）缓存等等。它的可能组合包括：
**比特域（名字或常量值:位数）** **标识（tag）**  **状态** 身份哈希值:25, 年龄:4, 0:1 01  未锁 锁记录地址:30 00  被轻量级锁住 monitor对象地址:30 10  被重量级锁住 转向地址:30 11  被GC标记 线程ID:23, 纪元:2, 年龄:4, 1:1 01  被偏向锁住/可被偏向锁
例子中的"main"字符串的_mark值为1，也就是说它：
- 没有被锁住；
- 现在未被GC标记；
- 年龄为0（尚未经历过GC）；
- 身份哈希值尚未被计算。
HotSpot的GC堆中许多创建没多久的对象的_mark值都会是1，属于正常现象。
接下来看SA输出的日志中写为_klass而在我的图示上写为_metadata的这个域。
在HotSpot的C++代码里，oopDesc是所有放在GC堆上的对象的顶层类，它的成员就构成了对象头。HotSpot在C++代码中用instanceOopDesc类来表示Java对象，而该类继承oopDesc，所以HotSpot中的Java对象也自然拥有oopDesc所声明的头部。
hotspot/src/share/vm/oops/oop.hpp：
class oopDesc { private: volatile markOop _mark; union _metadata { wideKlassOop _klass; narrowOop _compressed_klass; } _metadata; };
_metadata与前面提过的_mark一同构成了对象头。
_metadata是个union，为了能兼容32位、64位与开了压缩指针（CompressedOops）等几种情况。无论是这个union中的_klass还是_compressed_klass域，它们都是用于指向一个描述该对象的klass对象的指针。SA的API屏蔽了普通指针与压缩指针之间的差异，所以就直接把_metadata._klass称为了_klass。
对象头的格式是固定的，而对象自身内容的布局则由HotSpot根据一定规则来决定。Java类在被HotSpot加载时，其对象实例的布局与类自身的布局都会被计算出来。这个计算规则有机会以后再详细写。
现在来看看"main"这个String对象实例自身的域都是些什么。
value：指向真正保存字符串内容的对象的引用。留意Java里String并不把真正的字符内容直接存在自己里面，而是引用一个char[]对象来承载真正的存储。
从Java一侧看value域的类型是char[]，而从HotSpot的C++代码来看它就是个普通的指针而已。它当前值是0x7100b158，指向一个char[]对象的起始位置。
offset：字符串的内容从value指向的char[]中的第几个字符开始算（0-based）。int型，32位带符号整数，这从Java和C++来看都差不多。当前值为0。
count：该字符串的长度，或者说包含的UTF-16字符的个数。类型同上。当前值为4，说明该字符串有4个UTF-16字符。
hash：缓存该String对象的哈希值的成员域。类型同上。当前值为0，说明该实例的String.hashCode()方法尚未被调用过，因而尚未缓存住该字符串的哈希值。
String对象的成员域都走过一遍了，来看看value所指向的对象状况。
[C @ 0x7100b158 (object size = 24) - _mark: {0} :1 - _klass: {4} :TypeArrayKlass for [C @ 0x70e60440 - _length: {8} :4 - 0: {12} :m - 1: {14} :a - 2: {16} :i - 3: {18} :n
这就是"main"字符串的value所引用的char[]的日志。
[C 是char[]在JVM中的内部名称。
在@符号之后的0x7100b158是该对象的起始地址。
该对象占用GC堆的大小是24字节。留意了哦。
看看它的成员域。
_mark与_klass构成的对象头就不重复介绍了。可以留意的是元素类型为原始类型（boolean、char、short、int、long、float、double）的数组在HotSpot的C++代码里是用typeArrayOopDesc来表示的；这里的char[]也不例外。描述typeArrayOopDesc的klass对象是typeArrayKlass类型的，所以可以看到日志里_klass的值写着TypeArrayKlass for [C。
接下来是_length域。HotSpot中，数组对象比普通对象的头要多一个域，正是这个描述数组元素个数的_length。Java语言中数组的.length属性、JVM字节码中的arraylength要取的也正是这个值。
日志中的这个数组对象有4个字符，所以_length值为4。
再后面就是数组的内容了。于是该char[]在内存中的布局是：
----------------------- （+0） | _mark | ----------------------- （+4） | _metadata | ----------------------- （+8） | 数组长度 length | ----------------------- （+12） | char[0] | char[1] | ----------------------- （+16） | char[2] | char[3] | ----------------------- （+20） | 填充0 | -----------------------
Java的char是UTF-16字符，宽度是16位/2字节；4个字符需要8字节，加上对象头的4*3=12字节，总共需要20字节。但该char[]却占用了GC堆上的24字节，正是因为前面提到的数据对齐要求——HotSpot要求GC堆上的对象是8字节对齐的，20向上找最近的8的倍数就是24了。用于对齐的这部分会被填充为0。
"main"对象的value指向的char[]也介绍过了，回过头来看看它的_metadata._klass所指向的klass对象又是什么状况。
从HotSpot的角度来看，klass就是用于描述GC堆上的对象的对象；如果一个对象的大小、域的个数与类型等信息不固定的话，它就需要特定的klass对象来描述。
instanceOopDesc用于表示Java对象，instanceKlass用于描述它，但自身却又有些不固定的信息需要被描述，因而又有instanceKlassKlass；如此下去会没完没了，所以有个klassKlass作为这个描述链上的终结符。
klass的关系图：
![]()
[（图片来源）](http://openjdk.java.net/groups/hotspot/docs/FOSDEM-2007-HotSpot.pdf)
回到foo.txt日志文件上来，找到"main"对象的_klass域所引用的instanceKlass对象：
InstanceKlass for java/lang/String @ 0x70e6c6a0 (object size = 384) - _mark: {0} :1 - _klass: {4} :InstanceKlassKlass @ 0x70e60168 - _java_mirror: {60} :Oop for java/lang/Class @ 0x70e77760 - _super: {64} :InstanceKlass for java/lang/Object @ 0x70e65af8 - _size_helper: {12} :6 - _name: {68} :#java/lang/String @ 0x70e613e8 - _access_flags: {84} :134217777 - _subklass: {72} :null - _next_sibling: {76} :InstanceKlass for java/lang/CharSequence @ 0x70e680e8 - _alloc_count: {88} :0 - _array_klasses: {112} :ObjArrayKlass for InstanceKlass for java/lang/String @ 0x70ef6298 - _methods: {116} :ObjArray @ 0x70e682a0 - _method_ordering: {120} :[I @ 0x70e61330 - _local_interfaces: {124} :ObjArray @ 0x70e67998 - _transitive_interfaces: {128} :ObjArray @ 0x70e67998 - _nof_implementors: {268} :0 - _implementors[0]: {164} :null - _implementors[0]: {168} :null - _fields: {132} :[S @ 0x70e68230 - _constants: {136} :ConstantPool for java/lang/String @ 0x70e65c38 - _class_loader: {140} :null - _protection_domain: {144} :null - _signers: {148} :null - _source_file_name: {152} :#String.java @ 0x70e67980 - _inner_classes: {160} :[S @ 0x70e6c820 - _nonstatic_field_size: {196} :4 - _static_field_size: {200} :4 - _static_oop_field_size: {204} :2 - _nonstatic_oop_map_size: {208} :1 - _is_marked_dependent: {212} :0 - _init_state: {220} :5 - _vtable_len: {228} :5 - _itable_len: {232} :9 - serialVersionUID: {368} :-6849794470754667710 - serialPersistentFields: {360} :ObjArray @ 0x74e882c8 - CASE_INSENSITIVE_ORDER: {364} :Oop for java/lang/String$CaseInsensitiveComparator @ 0x74e882c0
还记得上文提到过的String类的3个静态变量么？有没有觉得有什么眼熟的地方？
没错，在HotSpot中，Java类的静态变量就是作为该类对应的instanceKlass的实例变量出现的。上面的日志里最后三行描述了String的静态变量所在。
这是件非常自然的事：类用于描述对象，类自身也是对象，有用于描述自身的类；某个类的所谓“静态变量”就是该类对象的实例变量。很多对象系统都是这么设计的。HotSpot的这套oop体系（指“普通对象指针”，不是指“面向对象编程”）继承自[Strongtalk](http://www.strongtalk.org/)，实际上反而比暴露给Java的对象模型显得更加面向对象一些。
HotSpot并不把instanceKlass暴露给Java，而会另外创建对应的java.lang.Class对象，并将后者称为前者的“Java镜像”，两者之间互相持有引用。日志中的_java_mirror便是该instanceKlass对Class对象的引用。
镜像机制被认为是良好的面向对象的反射与元编程设计的重要机制。Gilad Bracha与David Ungar还专门写了篇论文来阐述此观点，参考[Mirrors: Design Principles for Meta-level Facilities of Object-Oriented Programming Languages](http://bracha.org/mirrors.pdf)。
顺带把"main"对象的_klass链上余下的两个对象的日志也贴出来：
InstanceKlassKlass @ 0x70e60168 (object size = 120) - _mark: {0} :1 - _klass: {4} :KlassKlass @ 0x70e60000 - _java_mirror: {60} :Oop for java/lang/Class @ 0x70e76f20 - _super: {64} :null - _size_helper: {12} :0 - _name: {68} :null - _access_flags: {84} :0 - _subklass: {72} :null - _next_sibling: {76} :null - _alloc_count: {88} :0
所有instanceKlass对象都是被这个instanceKlassKlass对象所描述的。
KlassKlass @ 0x70e60000 (object size = 120) - _mark: {0} :1 - _klass: {4} :KlassKlass @ 0x70e60000 - _java_mirror: {60} :Oop for java/lang/Class @ 0x70e76e00 - _super: {64} :null - _size_helper: {12} :0 - _name: {68} :null - _access_flags: {84} :0 - _subklass: {72} :null - _next_sibling: {76} :null - _alloc_count: {88} :0
而所有*KlassKlass对象都是被这个klassKlass对象所描述的。
klass对象的更详细的介绍也留待以后再写吧～至少得找时间写写instanceKlass与vtable、itable的故事。
嘛，今天的废话到此结束 ^_^
希望这帖能解答先前[关于java里面的全局变量的内存分配](http://hllvm.group.javaeye.com/group/topic/20168)一帖中的疑问。也希望大家能多多支持[高级语言虚拟机](http://hllvm.group.javaeye.com/)圈子，有什么HLL VM相关的话题想讨论的欢迎来转转～
* [foo.zip](http://dl.javaeye.com/topics/download/4e07be37-e608-3954-9a5f-7015fdb0a687) (1.7 MB)
* 描述: 文中例子的输出
* 下载次数: 0

* [![]( "点击查看原始大小图片")]()
* 大小: 36.1 KB

* [查看图片附件](http://www.javaeye.com/topic/730461#)

声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。
推荐链接

* [![adobe]( "adobe")](http://www.javaeye.com/clicks/373) [返回顶楼](http://www.javaeye.com/topic/730461#) [ ](http://rednaxelafx.javaeye.com/ "浏览作者的博客") [ ](http://rednaxelafx.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=RednaxelaFX "发送站内短信") [ ](http://rednaxelafx.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=RednaxelaFX "关注作者")  * melin
* 等级: ![三星会员]( "三星会员")
* [![melin的博客]( "melin的博客: melin")](http://melin.javaeye.com/)
* 文章: 327
* 积分: 383
* 来自: 合肥
* ![]()
    发表时间：前天  

兄弟有点入魔了。。。 [返回顶楼](http://www.javaeye.com/topic/730461#) [ ](http://melin.javaeye.com/ "浏览作者的博客") [ ](http://melin.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=melin "发送站内短信") [ ](http://melin.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=melin "关注作者") [回帖地址](http://www.javaeye.com/topic/730461#1613583 "melin回帖:借助HotSpot SA来一窥PermGen上的对象")

[0](http://www.javaeye.com/topic/730461# "好") [0](http://www.javaeye.com/topic/730461# "差") 请登录后投票  * kimmking
* 等级: ![二钻会员]( "二钻会员")
* [![kimmking的博客]( "kimmking的博客: 程序人生")](http://setting.javaeye.com/)
* 文章: 2091
* 积分: 1160
* 来自: 中华大丈夫学院
* ![]()
    发表时间：前天  

RednaxelaFX 的帖子，太深奥了。
我找本编译原理补补先。 [返回顶楼](http://www.javaeye.com/topic/730461#) [ ](http://setting.javaeye.com/ "浏览作者的博客") [ ](http://setting.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=kimmking "发送站内短信") [ ](http://setting.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=kimmking "关注作者") [回帖地址](http://www.javaeye.com/topic/730461#1613655 "kimmking回帖:借助HotSpot SA来一窥PermGen上的对象")

[0](http://www.javaeye.com/topic/730461# "好") [0](http://www.javaeye.com/topic/730461# "差") 请登录后投票  * youjianbo_han_87
* 等级: 初级会员
* [![youjianbo_han_87的博客]( "youjianbo_han_87的博客: ")](http://youjianbo-han-87.javaeye.com/)
* 文章: 62
* 积分: 0
* 来自: 上海
* ![]()
    发表时间：前天  

确实不错，基础基础。 [返回顶楼](http://www.javaeye.com/topic/730461#) [ ](http://youjianbo-han-87.javaeye.com/ "浏览作者的博客") [ ](http://youjianbo-han-87.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=youjianbo_han_87 "发送站内短信") [ ](http://youjianbo-han-87.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=youjianbo_han_87 "关注作者") [回帖地址](http://www.javaeye.com/topic/730461#1613721 "youjianbo_han_87回帖:借助HotSpot SA来一窥PermGen上的对象")

[0](http://www.javaeye.com/topic/730461# "好") [0](http://www.javaeye.com/topic/730461# "差") 请登录后投票  * reilost
* 等级: 初级会员
* [![reilost的博客]( "reilost的博客: Reilost")](http://reilost.javaeye.com/)
* 文章: 74
* 积分: 20
* 来自: 北京
* [![]()](http://www.javaeye.com/topic/731832 "我正在看《终于忍不住诱惑，脱身出来做Contractor了。》")
    发表时间：前天  

-。-fx同学威武。。每次看你的帖子都想回去再看一遍编译原理 [返回顶楼](http://www.javaeye.com/topic/730461#) [ ](http://reilost.javaeye.com/ "浏览作者的博客") [ ](http://reilost.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=reilost "发送站内短信") [ ](http://reilost.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=reilost "关注作者") [回帖地址](http://www.javaeye.com/topic/730461#1613784 "reilost回帖:借助HotSpot SA来一窥PermGen上的对象")

[0](http://www.javaeye.com/topic/730461# "好") [0](http://www.javaeye.com/topic/730461# "差") 请登录后投票  * kaka2008
* 等级: ![二星会员]( "二星会员")
* [![kaka2008的博客]( "kaka2008的博客: 真的勇士，敢于直面这扯淡的人生")](http://kaka2008.javaeye.com/)
* 文章: 241
* 积分: 200
* 来自: 北京
* ![]()
    发表时间：前天  

fx同学出手即精华
而且帖子太深奥。。。 [返回顶楼](http://www.javaeye.com/topic/730461#) [ ](http://kaka2008.javaeye.com/ "浏览作者的博客") [ ](http://kaka2008.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=kaka2008 "发送站内短信") [ ](http://kaka2008.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=kaka2008 "关注作者") [回帖地址](http://www.javaeye.com/topic/730461#1613802 "kaka2008回帖:借助HotSpot SA来一窥PermGen上的对象")

[0](http://www.javaeye.com/topic/730461# "好") [0](http://www.javaeye.com/topic/730461# "差") 请登录后投票  * liujun999999
* 等级: 初级会员
* [![liujun999999的博客]( "liujun999999的博客: liujun999999")](http://liujun999999.javaeye.com/)
* 文章: 35
* 积分: 30
* 来自: 广州
* ![]()
    发表时间：前天  

晕，说实话，我没有看懂，看出差距来了，唉 [返回顶楼](http://www.javaeye.com/topic/730461#) [ ](http://liujun999999.javaeye.com/ "浏览作者的博客") [ ](http://liujun999999.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=liujun999999 "发送站内短信") [ ](http://liujun999999.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=liujun999999 "关注作者") [回帖地址](http://www.javaeye.com/topic/730461#1613905 "liujun999999回帖:借助HotSpot SA来一窥PermGen上的对象")

[0](http://www.javaeye.com/topic/730461# "好") [0](http://www.javaeye.com/topic/730461# "差") 请登录后投票  * hymanyung
* 等级: 初级会员
* [![hymanyung的博客]( "hymanyung的博客: ")](http://hymanyung.javaeye.com/)
* 文章: 5
* 积分: 30
* 来自: 东莞
* ![]()
    发表时间：前天  

看得頭暈了. [返回顶楼](http://www.javaeye.com/topic/730461#) [ ](http://hymanyung.javaeye.com/ "浏览作者的博客") [ ](http://hymanyung.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=hymanyung "发送站内短信") [ ](http://hymanyung.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=hymanyung "关注作者") [回帖地址](http://www.javaeye.com/topic/730461#1613949 "hymanyung回帖:借助HotSpot SA来一窥PermGen上的对象")

[0](http://www.javaeye.com/topic/730461# "好") [0](http://www.javaeye.com/topic/730461# "差") 请登录后投票  * seraphim871211
* 等级: 初级会员
* [![seraphim871211的博客]( "seraphim871211的博客: ")](http://seraphim871211.javaeye.com/)
* 文章: 58
* 积分: 50
* 来自: 杭州
* ![]()
    发表时间：前天  

“大叔”，你最近的帖子都看不下去了。。。![]() [返回顶楼](http://www.javaeye.com/topic/730461#) [ ](http://seraphim871211.javaeye.com/ "浏览作者的博客") [ ](http://seraphim871211.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=seraphim871211 "发送站内短信") [ ](http://seraphim871211.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=seraphim871211 "关注作者") [回帖地址](http://www.javaeye.com/topic/730461#1613969 "seraphim871211回帖:借助HotSpot SA来一窥PermGen上的对象")

[0](http://www.javaeye.com/topic/730461# "好") [0](http://www.javaeye.com/topic/730461# "差") 请登录后投票  * AlwenS
* 等级: 初级会员
* [![AlwenS的博客]( "AlwenS的博客: ")](http://alwens.javaeye.com/)
* 文章: 20
* 积分: 30
* 来自: 深圳
* ![]()
    发表时间：前天   最后修改：前天

看完鸟,楼主的动手能力真强。![]()
我也喜欢探究这些东西，只是功力还不如楼主深。
其实这东东和编译原理倒没多大关系了，主要看的是具体OO语言的Runtime Env模型。
比如c++就是没有penmgen的，所以他的动态能力和RTTI都非常弱。 [返回顶楼](http://www.javaeye.com/topic/730461#) [ ](http://alwens.javaeye.com/ "浏览作者的博客") [ ](http://alwens.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=AlwenS "发送站内短信") [ ](http://alwens.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=AlwenS "关注作者") [回帖地址](http://www.javaeye.com/topic/730461#1614028 "AlwenS回帖:借助HotSpot SA来一窥PermGen上的对象")

[0](http://www.javaeye.com/topic/730461# "好") [0](http://www.javaeye.com/topic/730461# "差") 请登录后投票

[](http://www.javaeye.com/forums/39/topics/730461/posts/new "发表回复")

« 上一页 1 [2](http://www.javaeye.com/topic/730461?page=2) [下一页 »](http://www.javaeye.com/topic/730461?page=2)
[论坛首页](http://www.javaeye.com/forums) → [Java编程和Java企业应用版](http://www.javaeye.com/forums/board/Java) → [Java综合](http://www.javaeye.com/forums/tag/Java)
跳转论坛:Java编程和Java企业应用 Web前端技术 移动编程和手机应用开发 C/C++编程 Ruby编程 Python编程 PHP编程 Flash编程和RIA Microsoft .Net 综合技术 软件开发和项目管理 行业应用 入门讨论 招聘求职 海阔天空

* [北京: JavaEye猎头诚聘Java搜索工程师](http://www.javaeye.com/jobs/712 "北京:JavaEye猎头诚聘Java搜索工程师")
* [浙江: VMKID诚聘Java开发工程师(J2SE)（Client端方](http://www.javaeye.com/jobs/779 "浙江:VMKID诚聘Java开发工程师(J2SE)（Client端方向）")
* [广东: 穆迪moody's诚聘Java Developer](http://www.javaeye.com/jobs/794 "广东:穆迪moody's诚聘Java Developer")
* [北京: 去哪儿旅游搜索引擎诚聘java开发工程师](http://www.javaeye.com/jobs/523 "北京:去哪儿旅游搜索引擎诚聘java开发工程师")
* [上海: BShare诚聘资深Java Web开发工程师](http://www.javaeye.com/jobs/838 "上海:BShare诚聘资深Java Web开发工程师")
* [北京: chinacache诚聘Java应用架构师](http://www.javaeye.com/jobs/728 "北京:chinacache诚聘Java应用架构师")
* [北京: glodon诚聘Eclipse插件开发工程师](http://www.javaeye.com/jobs/839 "北京:glodon诚聘Eclipse插件开发工程师")
* [北京: 掌中魔力诚聘java资深工程师](http://www.javaeye.com/jobs/769 "北京:掌中魔力诚聘java资深工程师")
* [北京: chinacache诚聘高级Java开发工程师（后台应用](http://www.javaeye.com/jobs/726 "北京:chinacache诚聘高级Java开发工程师（后台应用）")
* [上海: 上海汉得诚聘高级基础架构研发工程师——IDE](http://www.javaeye.com/jobs/825 "上海:上海汉得诚聘高级基础架构研发工程师——IDE方向")
* [北京: 上海幽幽诚聘手机网游服务器端开发工程师](http://www.javaeye.com/jobs/806 "北京:上海幽幽诚聘手机网游服务器端开发工程师")
* [北京: 网易（NETEASE）诚聘高级Java开发工程师](http://www.javaeye.com/jobs/800 "北京:网易（NETEASE）诚聘高级Java开发工程师")
* [北京: Laszlo Systems 诚聘高级Java程序员](http://www.javaeye.com/jobs/817 "北京:Laszlo Systems 诚聘高级Java程序员")
* [上海: 恺英网络诚聘JS工程师](http://www.javaeye.com/jobs/682 "上海:恺英网络诚聘JS工程师")
* [福建: 福州几维网络诚聘游戏服务端程序员(Java)](http://www.javaeye.com/jobs/804 "福建:福州几维网络诚聘游戏服务端程序员(Java)")

* [首页](http://www.javaeye.com/)
* [新闻](http://www.javaeye.com/news)
* [论坛](http://www.javaeye.com/forums)
* [问答](http://www.javaeye.com/ask)
* [专栏](http://www.javaeye.com/wiki)
* [博客](http://www.javaeye.com/blogs)
* [圈子](http://www.javaeye.com/groups)
* [招聘](http://www.javaeye.com/job)
* [服务](http://www.javaeye.com/index/service)
* [搜索](http://www.javaeye.com/search)
* [Java](http://java.javaeye.com/)
* [Web](http://webapp.javaeye.com/)
* [Ruby](http://ruby.javaeye.com/)
* [Python](http://python.javaeye.com/)
* [敏捷](http://agile.javaeye.com/)
* [MySQL](http://mysql.javaeye.com/)
* [润亁报表](http://runqian.javaeye.com/)
* [图书](http://book.javaeye.com/)
* [广告服务](http://www.javaeye.com/index/service)
* [JavaEye黑板报](http://webmaster.javaeye.com/)
* [关于我们](http://www.javaeye.com/index/aboutus)
* [联系我们](http://www.javaeye.com/index/contactus)
* [友情链接](http://www.javaeye.com/index/friend_links)

© 2003-2010 JavaEye.com. 上海炯耐计算机软件有限公司版权所有 [ [沪ICP备05023328号](http://www.miibeian.gov.cn/) ]
