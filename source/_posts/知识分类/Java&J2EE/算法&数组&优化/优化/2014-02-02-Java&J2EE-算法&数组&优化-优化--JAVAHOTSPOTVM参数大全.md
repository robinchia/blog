---
layout: post
title: "JAVA HOTSPOT VM参数大全"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 优化
--- 

# JAVA HOTSPOT VM参数大全

[首页](http://www.javaeye.com/) [新闻](http://www.javaeye.com/news) [论坛](http://www.javaeye.com/forums) [问答](http://www.javaeye.com/ask) [博客](http://www.javaeye.com/blogs) [招聘](http://www.javaeye.com/job) [更多 ▼](http://longdick.javaeye.com/blog/472477#)

[专栏](http://www.javaeye.com/wiki) [圈子](http://www.javaeye.com/groups) [搜索](http://www.javaeye.com/search)

[您还未登录 !](http://longdick.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://longdick.javaeye.com/login) [注册](http://longdick.javaeye.com/signup)

# [幸福的懦夫](http://longdick.javaeye.com/)

永久域名 [http://longdick.javaeye.com/](http://longdick.javaeye.com/)

[2顶](http://longdick.javaeye.com/blog/472477#)
[0踩](http://longdick.javaeye.com/blog/472477#)

[Freaker与妖人](http://longdick.javaeye.com/blog/472881 "Freaker与妖人") | [图解JVM在内存中申请对象及垃圾回收流程](http://longdick.javaeye.com/blog/468368 "图解JVM在内存中申请对象及垃圾回收流程")

2009-09-20

### [JAVA HOTSPOT VM参数大全](http://longdick.javaeye.com/blog/472477)

关键字: hotspot vm 参数
/**

*  转载请注明作者longdick    http://longdick.javaeye.com

*

*/

 

（本文JDK版本6.0）

 

SUN的JDK版本从1.3.1开始使用HotSpot虚拟机技术。
HotSpot是较新的Java虚拟机技术，用来代替JIT（Just in Time）技术，可以大大提高Java运行的性能。
Java原先是把源代码编译为字节码在虚拟机执行，这样执行速度较慢。而该技术将常用的部分代码编译为本地（原生，native）代码，这样
显著提高了性能。
用于服务器版和标准版的HotSpot有所不同。
其他的Java虚拟机也有类似的技术。
HotSpot JVM 参数可以分为标准参数（standard options）和非标准参数（non-standard options）。
标准参数相对稳定，在JDK未来的版本里不会有太大的改动。
非标准参数则有因升级JDK而改变的可能。
**标准参数：**
**-client**
  使用Java HotSpot 客户端版VM。
**-server**
  使用Java HotSpot 服务器版VM。如果是64位的JDK，默认只有server版，所以以上两个参数对64位版本JDK无效。
**-agentlib:** *libname* [=options]
  加载本地代理函数库, e.g.
  -agentlib:jdwp=help
**-agentpath** :*pathname* [=options]
  使用给定的路径加载本地代理库。
**-classpath** *classpath*
**-cp** *classpath*
  不用说了。
**-Dproperty** =value
  设置一个系统属性。
**-d32
-d64**
  要求程序在32位或64位下跑，未来这个参数可能有变。
-**enableassertions** [:<package name>"..." | :<class name> ]
-**ea** [:<package name>"..." | :<class name> ]
  开启断言。
**-disableassertions** [:<package name>"..." | :<class name> ]
**-da** [:<package name>"..." | :<class name> ]
  关闭断言。
**-enablesystemassertions
-** **esa**
  启动所有系统类的断言。

**-disablesystemassertions
-dsa**
  关闭所有系统类的断言。
**-jar**
  这个也没什么说的。
**-javaagent** :*jarpath* [=options]
  加载Java 程序语言代理
**-verbose:class**
  输出每个加载的类详细信息。
**-verbose:gc**
  输出GC的详细信息。
**-verbose:jni**
  输出本地方法接口的调用信息。
**-version
-help
-?**
  不用说了。
**-X**
  显示可用的非标准参数
**非标准参数：**
**-Xint**
  以解释模式运行。JVM不会使用HotSpot的新特性，不会将部分常用代码编译为本地代码，所有代码都以字节码的方式解释运行。你可以理解为使用JDK1.3.1以前的JIT方式运行程序。
**-Xbatch**
  不使用后台编译。
**-Xbootclasspath:bootclasspath**
  使用bootstrap classloader 加载指定路径的class或jar，这是种完全覆盖默认系统类加载的方案，慎用。
**-Xbootclasspath/a:bootclasspath**
  将指定的classpath追加到默认的bootclasspath的后面加载
**-Xbootclasspath/p:bootclasspath**
  将指定的classpath追加到默认的bootclasspath的前面加载
**-Xcheck:jni**
  在调用JNI函数时做额外的检查。这个参数会降低JVM的执行性能。
**-Xfuture**
  执行严格的class文件格式检查，不加这个参数，默认使用JDK1.1.* 版本的class格式检查方法。
**-Xnoclassgc**
  禁用class垃圾收集。
**-Xincgc**
  开启增量垃圾收集机制。默认为关闭。增量垃圾收集能减少因垃圾收集而引起的程序中断，它会在程序运行期间不定期地以并发的方式运行，在运行期间不会引起中断但是会减少分配给程序的cpu资源。
**-Xloggc:file**
  GC详情日志。效果如-verbose:gc ，不过这个可以输出到一个文件。除了-verbose:gc包含的信息，还有显示发生的时间。 文件可以是远程的，但是考虑到网络延迟会引起JVM中断，一般建议使用本地文件，
**-Xms**
  分配的堆空间初始值：
  -Xms6291456
  -Xms6144k
  -Xms6m
**-Xmx**
  分配的堆空间最大值：
  -Xmx83886080
  -Xmx81920k
  -Xmx80m
**-Xprof**
  在运行程序时给出分析数据。适用于开发环境，不适用于生产环境。
**-Xrs**
  减少JVM的操作系统信号的使用量。
**-Xss**
  线程栈内存。
**非标准-XX参数**

 

有三种-XX参数的形式：

 

* Boolean 型的参数 开启如 -XX:+<option> 关闭如 -XX:-<option>.
* Numeric 型参数 -XX:<option>=<number>. 数字可以包括单位 'm' 或 'M' 代表MB, 'k' or 'K' 代表KB， 'g' or 'G' 代表 GB ，没有单位意味着bytes。
* String 参数 -XX:<option>=<string>

以下是-XX参数列表，本来想都翻译过来，但是发现一些技术术语如果硬是翻译可能会导致词不达意，因此大部分描述都保持原文。

 

 
参数名和默认值
 描述 -XX:-AllowUserSignalHandlers 允许使用用户自定义的信号处理器 (只对应Solaris和Linux) -XX:AltStackSize=16384 修改栈容量 (单位 Kb) (对应Solaris, JDK 5.0以后弃用) -XX:-DisableExplicitGC 禁止手动调用System.gc() -XX:+FailOverToOldVerifier 如果新的类型校验器验证失败使用旧版本的类型校验器 (开始于JDK6.) -XX:+HandlePromotionFailure The youngest generation collection does not require a guarantee of full promotion of all live objects. (Introduced in 1.4.2 update 11) [5.0 and earlier: false.] -XX:+MaxFDLimit 将文件描述符加到最大 (对应Solaris) -XX:PreBlockSpin=10 Spin count variable for use with -XX:+UseSpinning. Controls the maximum spin iterations allowed before entering operating system thread synchronization code. (Introduced in 1.4.2.) -XX:-RelaxAccessControlCheck 放宽类型校验机的准入控制(JDK6) -XX:+ScavengeBeforeFullGC 在full GC之前先做年轻代GC (开始于JDK1.4.1.) -XX:+UseAltSigs Use alternate signals instead of SIGUSR1 and SIGUSR2 for VM internal signals. (Introduced in 1.3.1 update 9, 1.4.1. Relevant to Solaris only.) -XX:+UseBoundThreads Bind user level threads to kernel threads. (Relevant to Solaris only.) -XX:-UseConcMarkSweepGC 使用并发的mark-sweep GC收集年老代 (始于JDK1.4.1) -XX:+UseGCOverheadLimit 使用一种限制VM做GC操作的时间所占比例过高的策略 (始于JDK6.) -XX:+UseLWPSynchronization 使用轻量级进程同步替代线程同步 (始于JDK1.4.0. Solaris相关) -XX:-UseParallelGC 使用并发平行GC(始于JDK1.4.1) -XX:-UseParallelOldGC 使用并发平行GC做 full GC. (始于JDK5.0 update 6.) -XX:-UseSerialGC 使用串行GC (始于JDK5.0.) -XX:-UseSpinning Enable naive spinning on Java monitor before entering operating system thread synchronizaton code. (Relevant to 1.4.2 and 5.0 only.) [1.4.2, multi-processor Windows platforms: true] -XX:+UseTLAB Use thread-local object allocation (Introduced in 1.4.0, known as UseTLE prior to that.) [1.4.2 and earlier, x86 or with -client: false] -XX:+UseSplitVerifier Use the new type checker with StackMapTable attributes. (Introduced in 5.0.)[5.0: false] -XX:+UseThreadPriorities Use native thread priorities. -XX:+UseVMInterruptibleIO Thread interrupt before or with EINTR for I/O operations results in OS_INTRPT. (Introduced in 6. Relevant to Solaris only.)

 

 

 
参数名和默认值 描述 -XX:+AggressiveOpts Turn on point performance compiler optimizations that are expected to be default in upcoming releases. (Introduced in 5.0 update 6.) -XX:CompileThreshold=10000 Number of method invocations/branches before compiling [-client: 1,500] -XX:LargePageSizeInBytes=4m Sets the large page size used for the Java heap. (Introduced in 1.4.0 update 1.) [amd64: 2m.] -XX:MaxHeapFreeRatio=70 Maximum percentage of heap free after GC to avoid shrinking. -XX:MaxNewSize=size Maximum size of new generation (in bytes). Since 1.4, MaxNewSize is computed as a function of NewRatio. [1.3.1 Sparc: 32m; 1.3.1 x86: 2.5m.] -XX:MaxPermSize=64m Size of the Permanent Generation.  [5.0 and newer: 64 bit VMs are scaled 30% larger; 1.4 amd64: 96m; 1.3.1 -client: 32m.] -XX:MinHeapFreeRatio=40 Minimum percentage of heap free after GC to avoid expansion. -XX:NewRatio=2 Ratio of new/old generation sizes. [Sparc -client: 8; x86 -server: 8; x86 -client: 12.]-client: 4 (1.3) 8 (1.3.1+), x86: 12] -XX:NewSize=2.125m Default size of new generation (in bytes) [5.0 and newer: 64 bit VMs are scaled 30% larger; x86: 1m; x86, 5.0 and older: 640k] -XX:ReservedCodeCacheSize=32m Reserved code cache size (in bytes) - maximum code cache size. [Solaris 64-bit, amd64, and -server x86: 48m; in 1.5.0_06 and earlier, Solaris 64-bit and and64: 1024m.] -XX:SurvivorRatio=8 Ratio of eden/survivor space size [Solaris amd64: 6; Sparc in 1.3.1: 25; other Solaris platforms in 5.0 and earlier: 32] -XX:TargetSurvivorRatio=50 Desired percentage of survivor space used after scavenge. -XX:ThreadStackSize=512 Thread Stack Size (in Kbytes). (0 means use default stack size) [Sparc: 512; Solaris x86: 320 (was 256 prior in 5.0 and earlier); Sparc 64 bit: 1024; Linux amd64: 1024 (was 0 in 5.0 and earlier); all others 0.] -XX:+UseBiasedLocking Enable biased locking. For more details, see this [tuning example](http://java.sun.com/performance/reference/whitepapers/tuning.html#section4.2.5) . (Introduced in 5.0 update 6.) [5.0: false] -XX:+UseFastAccessorMethods Use optimized versions of Get<Primitive>Field. -XX:-UseISM Use Intimate Shared Memory. [Not accepted for non-Solaris platforms.] For details, see [Intimate Shared Memory](http://java.sun.com/docs/hotspot/ism.html) . -XX:+UseLargePages Use large page memory. (Introduced in 5.0 update 5.) For details, see [Java Support for Large Memory Pages](http://java.sun.com/javase/technologies/hotspot/largememory.jsp) . -XX:+UseMPSS Use Multiple Page Size Support w/4mb pages for the heap. Do not use with ISM as this replaces the need for ISM. (Introduced in 1.4.0 update 1, Relevant to Solaris 9 and newer.) [1.4.1 and earlier: false] -XX:+StringCache Enables caching of commonly allocated strings. -XX:AllocatePrefetchLines=1 Number of cache lines to load after the last object allocation using prefetch instructions generated in JIT compiled code. Default values are 1 if the last allocated object was an instance and 3 if it was an array. -XX:AllocatePrefetchStyle=1 Generated code style for prefetch instructions.
0 - no prefetch instructions are generate*d*,
1 - execute prefetch instructions after each allocation,
2 - use TLAB allocation watermark pointer to gate when prefetch instructions are executed.

 

 
参数名和默认值 描述 -XX:-CITime Prints time spent in JIT Compiler. (Introduced in 1.4.0.) -XX:ErrorFile=./hs_err_pid<pid>.log If an error occurs, save the error data to this file. (Introduced in 6.) -XX:-ExtendedDTraceProbes Enable performance-impacting [dtrace](http://java.sun.com/javase/6/docs/technotes/guides/vm/dtrace.html) probes. (Introduced in 6. Relevant to Solaris only.) -XX:HeapDumpPath=./java_pid<pid>.hprof Path to directory or filename for heap dump. *Manageable* . (Introduced in 1.4.2 update 12, 5.0 update 7.) -XX:-HeapDumpOnOutOfMemoryError Dump heap to file when java.lang.OutOfMemoryError is thrown. *Manageable* . (Introduced in 1.4.2 update 12, 5.0 update 7.) -XX:OnError="<cmd args>;<cmd args>" Run user-defined commands on fatal error. (Introduced in 1.4.2 update 9.) -XX:OnOutOfMemoryError="<cmd args>;
<cmd args>" Run user-defined commands when an OutOfMemoryError is first thrown. (Introduced in 1.4.2 update 12, 6) -XX:-PrintClassHistogram Print a histogram of class instances on Ctrl-Break. *Manageable* . (Introduced in 1.4.2.) The [jmap -histo](http://java.sun.com/javase/6/docs/technotes/tools/share/jmap.html) command provides equivalent functionality. -XX:-PrintConcurrentLocks Print java.util.concurrent locks in Ctrl-Break thread dump. *Manageable* . (Introduced in 6.) The [jstack -l](http://java.sun.com/javase/6/docs/technotes/tools/share/jstack.html) command provides equivalent functionality. -XX:-PrintCommandLineFlags Print flags that appeared on the command line. (Introduced in 5.0.) -XX:-PrintCompilation Print message when a method is compiled. -XX:-PrintGC Print messages at garbage collection. *Manageable* . -XX:-PrintGCDetails Print more details at garbage collection. *Manageable* . (Introduced in 1.4.0.) -XX:-PrintGCTimeStamps Print timestamps at garbage collection. *Manageable* (Introduced in 1.4.0.) -XX:-PrintTenuringDistribution Print tenuring age information. -XX:-TraceClassLoading Trace loading of classes. -XX:-TraceClassLoadingPreorder Trace all classes loaded in order referenced (not loaded). (Introduced in 1.4.2.) -XX:-TraceClassResolution Trace constant pool resolutions. (Introduced in 1.4.2.) -XX:-TraceClassUnloading Trace unloading of classes. -XX:-TraceLoaderConstraints Trace recording of loader constraints. (Introduced in 6.) -XX:+PerfSaveDataToFile Saves jvmstat binary data on exit.

 

 

 

 

参考资料：

http://java.sun.com/javase/6/docs/technotes/tools/windows/java.html

http://java.sun.com/javase/technologies/hotspot/vmoptions.jsp

 

* [![]( "点击查看原始大小图片")](http://dl.javaeye.com/upload/attachment/148685/089c5f6b-6e67-3782-99d4-cef9ec353694.gif)
* 大小: 23.6 KB

* [查看图片附件](http://longdick.javaeye.com/blog/472477#)
[**2**
顶](http://longdick.javaeye.com/blog/472477#)[**0**
踩](http://longdick.javaeye.com/blog/472477#)

[Freaker与妖人](http://longdick.javaeye.com/blog/472881 "Freaker与妖人") | [图解JVM在内存中申请对象及垃圾回收流程](http://longdick.javaeye.com/blog/468368 "图解JVM在内存中申请对象及垃圾回收流程")
* 20:06
* 浏览 (447)
* [评论](http://longdick.javaeye.com/blog/472477#comments) (0)
* 分类: [JVM](http://longdick.javaeye.com/category/73063)
* [相关推荐](http://www.javaeye.com/wiki/topic/472477)

### 评论

[]()
### 发表评论

您还没有登录，请[登录](http://longdick.javaeye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![longdick的博客]( "longdick的博客: 幸福的懦夫")](http://longdick.javaeye.com/)

longdick

* 浏览: 52771 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 0
* ![]()
* [详细资料](http://longdick.javaeye.com/blog/profile) [留言簿](http://longdick.javaeye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://longdick.javaeye.com/blog/user_visits)

[![hqm1988的博客]( "hqm1988的博客: ")](http://hqm1988.javaeye.com/)

[hqm1988](http://hqm1988.javaeye.com/)

[![cesc的博客]( "cesc的博客: ")](http://cesc.javaeye.com/)

[cesc](http://cesc.javaeye.com/)
[![shirennostone的博客]( "shirennostone的博客: ")](http://shirennostone.javaeye.com/)

[shirennostone](http://shirennostone.javaeye.com/)

[![etheme的博客]( "etheme的博客: ")](http://etheme.javaeye.com/)

[etheme](http://etheme.javaeye.com/)

### 博客分类

* [全部博客 (75)](http://longdick.javaeye.com/)
* [java全文检索 (4)](http://longdick.javaeye.com/category/27870)
* [开心最重要 (5)](http://longdick.javaeye.com/category/29876)
* [Hibernate (2)](http://longdick.javaeye.com/category/30468)
* [DWR (2)](http://longdick.javaeye.com/category/32248)
* [Ubuntu (2)](http://longdick.javaeye.com/category/33473)
* [杂项 (5)](http://longdick.javaeye.com/category/44947)
* [Oracle (4)](http://longdick.javaeye.com/category/45536)
* [Ajax (4)](http://longdick.javaeye.com/category/46583)
* [spring (3)](http://longdick.javaeye.com/category/73061)
* [应用服务器 (3)](http://longdick.javaeye.com/category/73062)
* [JVM (8)](http://longdick.javaeye.com/category/73063)
* [webservice (1)](http://longdick.javaeye.com/category/73064)
* [IDE (1)](http://longdick.javaeye.com/category/73065)
* [模式 (1)](http://longdick.javaeye.com/category/73066)
* [简单心情 (17)](http://longdick.javaeye.com/category/73067)
* [OSGI (1)](http://longdick.javaeye.com/category/77407)
* [版本管理 (1)](http://longdick.javaeye.com/category/77536)
* [jms (2)](http://longdick.javaeye.com/category/78703)
* [uml (7)](http://longdick.javaeye.com/category/81824)
* [java 基础 (7)](http://longdick.javaeye.com/category/85447)
* [毋忘在莒 (0)](http://longdick.javaeye.com/category/86210)
### 我的相册

[![]()](http://longdick.javaeye.com/album)HiYrh.jpg
[共 3 张](http://longdick.javaeye.com/album)

### 我的留言簿 [>>更多留言](http://longdick.javaeye.com/blog/guest_book)

* 楼主，你对JVM了解很深吧，我们能否联系一下，也许能合作哦！
-- by [linux1689](http://longdick.javaeye.com/blog/guest_book#12981)
* 漂过
-- by [suitmefine](http://longdick.javaeye.com/blog/guest_book#12953)
* 好厉害！
-- by [poson](http://longdick.javaeye.com/blog/guest_book#11806)
### 其他分类

* [我的收藏](http://longdick.javaeye.com/blog/favorite) (44)
* [我的论坛主题贴](http://longdick.javaeye.com/blog/topic) (15)
* [我的所有论坛贴](http://longdick.javaeye.com/blog/post) (12)
* [我的精华良好贴](http://longdick.javaeye.com/blog/article) (0)

### 最近加入圈子
### 存档

* [2010-04](http://longdick.javaeye.com/blog/monthblog/2010-04) (1)
* [2009-11](http://longdick.javaeye.com/blog/monthblog/2009-11) (3)
* [2009-10](http://longdick.javaeye.com/blog/monthblog/2009-10) (11)
* [更多存档...](http://longdick.javaeye.com/blog/monthblog_more)

### 最新评论

* [人人都是领域专家-类图关 ...](http://longdick.javaeye.com/blog/487640#comments "人人都是领域专家-类图关系说明")
这个系列写得非常得好 楼主辛苦
-- by [luedipiaofeng](http://luedipiaofeng.javaeye.com/)
* [人人都是领域专家-用例图](http://longdick.javaeye.com/blog/483772#comments "人人都是领域专家-用例图")
非常清晰明了，非常精彩。
-- by [ziwen](http://ziwen.javaeye.com/)
* [最简实例说明wait、notify ...](http://longdick.javaeye.com/blog/453615#comments "最简实例说明wait、notify、notifyAll的使用方法")
感谢楼主啊！！讲得非常到位，解决了我的大问题！！！
-- by [itpentiuman](http://itpentiuman.javaeye.com/)
* [最简实例说明wait、notify ...](http://longdick.javaeye.com/blog/453615#comments "最简实例说明wait、notify、notifyAll的使用方法")
终于弄明白 wait notify notifyAll这三个方法的使用规则了
-- by [qingyunzh](http://qingyunzh.javaeye.com/)
* [神秘的数字9](http://longdick.javaeye.com/blog/504864#comments "神秘的数字9")
quote="nethibernate"]1楼nb, 崇拜一下 ...
-- by [sunwenran](http://sunwenran.javaeye.com/)
### 评论排行榜

* [给思维做体操的谜题--光有IQ还不够（二）](http://longdick.javaeye.com/blog/483185 "给思维做体操的谜题--光有IQ还不够（二）")
* [图解JVM在内存中申请对象及垃圾回收流程](http://longdick.javaeye.com/blog/468368 "图解JVM在内存中申请对象及垃圾回收流程")
* [完美解决甲醛问题的终极方案--只要人人都献 ...](http://longdick.javaeye.com/blog/461144 "完美解决甲醛问题的终极方案--只要人人都献出一点气，世界将变成更美好人间")
* [人人都是领域专家-用例图](http://longdick.javaeye.com/blog/483772 "人人都是领域专家-用例图")
* [三种GC大揭秘](http://longdick.javaeye.com/blog/474764 "三种GC大揭秘")

* [![Rss]()](http://longdick.javaeye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://longdick.javaeye.com/rss)
* [![Rss_xianguo]()](http://www.xianguo.com/subscribe.php?url=http://longdick.javaeye.com/rss)
* [[什么是RSS?]](http://www.google.com/search?hl=zh-CN&q=RSS)
声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2010 JavaEye.com. All rights reserved. 上海炯耐计算机软件有限公司 [ 沪ICP备05023328号 ]
