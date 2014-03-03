---
layout: post
title: "使用Memory Analyzer tool(MAT)分析内存泄漏（二）"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# 使用Memory Analyzer tool(MAT)分析内存泄漏（二）

# [**成都心情**](http://www.blogjava.net/rosen/)

关注 ODBMS noSQL RDBMS 还是把标题换回来

  [BlogJava](http://www.blogjava.net/) :: [首页](http://www.blogjava.net/rosen/) ::  :: [联系](http://www.blogjava.net/rosen/contact.aspx?id=1) :: [聚合](http://www.blogjava.net/rosen/rss) [![]()](http://www.blogjava.net/rosen/rss) :: [管理](http://www.blogjava.net/rosen/admin/EditPosts.aspx) :: ![]()   87 随笔 :: 2 文章 :: 436 评论 :: 1 Trackbacks

### 公告

[![&#0;0;0;0;6211;&#0;0;0;0;8981;&#0;0;0;0;5566;&#0;0;0;0;514D;&#0;0;0;0;8D39;&#0;0;0;0;7EDF;&#0;0;0;0;8BA1;]()](http://www.51.la/?298319)
[![Creative Commons License]()](http://creativecommons.org/licenses/by-sa/2.5/cn/)
本作品采用[知识共享署名-相同方式共享 2.5 中国大陆许可协议](http://creativecommons.org/licenses/by-sa/2.5/cn/)进行许可。 [![Locations of visitors to this page]( "Locations of visitors to this page")](http://www2.clustrmaps.com/counter/maps.php?url=http://www.blogjava.net/rosen)

### 留言簿(4)

* [给我留言](http://www.blogjava.net/rosen/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/rosen/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/rosen/admin/MyMessages.aspx)

### 随笔分类(88)

* [Java EE 服务器端(14)](http://www.blogjava.net/rosen/category/2689.html) [(rss)](http://www.blogjava.net/rosen/category/2689.html/rss "Subscribe to Java EE 服务器端(14)")
* [Java EE 表现层及容器(12)](http://www.blogjava.net/rosen/category/2686.html) [(rss)](http://www.blogjava.net/rosen/category/2686.html/rss "Subscribe to Java EE 表现层及容器(12)")
* [Java ME(1)](http://www.blogjava.net/rosen/category/5482.html) [(rss)](http://www.blogjava.net/rosen/category/5482.html/rss "Subscribe to Java ME(1)")
* [Java 基础(10)](http://www.blogjava.net/rosen/category/2690.html) [(rss)](http://www.blogjava.net/rosen/category/2690.html/rss "Subscribe to Java 基础(10)")
* [MatLab(1)](http://www.blogjava.net/rosen/category/7219.html) [(rss)](http://www.blogjava.net/rosen/category/7219.html/rss "Subscribe to MatLab(1)")
* [O/R Mapping(13)](http://www.blogjava.net/rosen/category/2688.html) [(rss)](http://www.blogjava.net/rosen/category/2688.html/rss "Subscribe to O/R Mapping(13)")
* [Versant db4o 中文项目(11)](http://www.blogjava.net/rosen/category/13739.html) [(rss)](http://www.blogjava.net/rosen/category/13739.html/rss "Subscribe to Versant db4o 中文项目(11)")
* [五花八门(8)](http://www.blogjava.net/rosen/category/2685.html) [(rss)](http://www.blogjava.net/rosen/category/2685.html/rss "Subscribe to 五花八门(8)")
* [工作流(10)](http://www.blogjava.net/rosen/category/2684.html) [(rss)](http://www.blogjava.net/rosen/category/2684.html/rss "Subscribe to 工作流(10)")
* [数据库(2)](http://www.blogjava.net/rosen/category/2682.html) [(rss)](http://www.blogjava.net/rosen/category/2682.html/rss "Subscribe to 数据库(2)")
* [模式与策略(6)](http://www.blogjava.net/rosen/category/2683.html) [(rss)](http://www.blogjava.net/rosen/category/2683.html/rss "Subscribe to 模式与策略(6)")

### 随笔档案(88)

* [2010年6月 (1)](http://www.blogjava.net/rosen/archive/2010/06.html)
* [2010年5月 (3)](http://www.blogjava.net/rosen/archive/2010/05.html)
* [2010年3月 (1)](http://www.blogjava.net/rosen/archive/2010/03.html)
* [2010年1月 (1)](http://www.blogjava.net/rosen/archive/2010/01.html)
* [2009年10月 (1)](http://www.blogjava.net/rosen/archive/2009/10.html)
* [2009年9月 (1)](http://www.blogjava.net/rosen/archive/2009/09.html)
* [2009年7月 (1)](http://www.blogjava.net/rosen/archive/2009/07.html)
* [2009年6月 (1)](http://www.blogjava.net/rosen/archive/2009/06.html)
* [2009年3月 (1)](http://www.blogjava.net/rosen/archive/2009/03.html)
* [2009年2月 (1)](http://www.blogjava.net/rosen/archive/2009/02.html)
* [2008年12月 (2)](http://www.blogjava.net/rosen/archive/2008/12.html)
* [2008年9月 (1)](http://www.blogjava.net/rosen/archive/2008/09.html)
* [2008年8月 (1)](http://www.blogjava.net/rosen/archive/2008/08.html)
* [2008年7月 (1)](http://www.blogjava.net/rosen/archive/2008/07.html)
* [2008年6月 (1)](http://www.blogjava.net/rosen/archive/2008/06.html)
* [2008年4月 (1)](http://www.blogjava.net/rosen/archive/2008/04.html)
* [2008年3月 (1)](http://www.blogjava.net/rosen/archive/2008/03.html)
* [2008年1月 (1)](http://www.blogjava.net/rosen/archive/2008/01.html)
* [2007年12月 (2)](http://www.blogjava.net/rosen/archive/2007/12.html)
* [2007年10月 (1)](http://www.blogjava.net/rosen/archive/2007/10.html)
* [2007年9月 (1)](http://www.blogjava.net/rosen/archive/2007/09.html)
* [2007年8月 (1)](http://www.blogjava.net/rosen/archive/2007/08.html)
* [2007年6月 (2)](http://www.blogjava.net/rosen/archive/2007/06.html)
* [2007年5月 (1)](http://www.blogjava.net/rosen/archive/2007/05.html)
* [2007年4月 (1)](http://www.blogjava.net/rosen/archive/2007/04.html)
* [2007年2月 (1)](http://www.blogjava.net/rosen/archive/2007/02.html)
* [2007年1月 (1)](http://www.blogjava.net/rosen/archive/2007/01.html)
* [2006年12月 (1)](http://www.blogjava.net/rosen/archive/2006/12.html)
* [2006年11月 (1)](http://www.blogjava.net/rosen/archive/2006/11.html)
* [2006年10月 (1)](http://www.blogjava.net/rosen/archive/2006/10.html)
* [2006年9月 (1)](http://www.blogjava.net/rosen/archive/2006/09.html)
* [2006年8月 (1)](http://www.blogjava.net/rosen/archive/2006/08.html)
* [2006年7月 (1)](http://www.blogjava.net/rosen/archive/2006/07.html)
* [2006年6月 (1)](http://www.blogjava.net/rosen/archive/2006/06.html)
* [2006年5月 (1)](http://www.blogjava.net/rosen/archive/2006/05.html)
* [2006年4月 (1)](http://www.blogjava.net/rosen/archive/2006/04.html)
* [2006年3月 (1)](http://www.blogjava.net/rosen/archive/2006/03.html)
* [2006年2月 (1)](http://www.blogjava.net/rosen/archive/2006/02.html)
* [2006年1月 (1)](http://www.blogjava.net/rosen/archive/2006/01.html)
* [2005年12月 (1)](http://www.blogjava.net/rosen/archive/2005/12.html)
* [2005年11月 (1)](http://www.blogjava.net/rosen/archive/2005/11.html)
* [2005年10月 (1)](http://www.blogjava.net/rosen/archive/2005/10.html)
* [2005年9月 (2)](http://www.blogjava.net/rosen/archive/2005/09.html)
* [2005年8月 (39)](http://www.blogjava.net/rosen/archive/2005/08.html)

### 文章分类(2)

* [我的收藏(2)](http://www.blogjava.net/rosen/category/3474.html) [(rss)](http://www.blogjava.net/rosen/category/3474.html/rss "Subscribe to 我的收藏(2)")

### 友情链接

* [david.turing](http://www.blogjava.net/openssl/) [(rss)](http://www.blogjava.net/openssl/Rss.aspx "Subscribe to david.turing")
* [wyingquan的专栏](http://blog.csdn.net/wyingquan) [(rss)](http://blog.csdn.net/wyingquan/Rss.aspx "Subscribe to wyingquan的专栏")
* [俺的猪窝~！@](http://yfeng.cn/)
* [喜马拉雅的雪杉](http://spaces.msn.com/members/Deodar/) [(rss)](http://spaces.msn.com/members/Deodar/feed.rss "Subscribe to 喜马拉雅的雪杉")
* [无聊人士](http://www.blogjava.net/mmwy) [(rss)](http://www.blogjava.net/mmwy/Rss.aspx "Subscribe to 无聊人士")
* [竹十一](http://www.blogjava.net/juleven) [(rss)](http://www.blogjava.net/juleven/rss "Subscribe to 竹十一")
* [老刘忙不忙](http://www.cnblogs.com/xd125) [(rss)](http://www.cnblogs.com/xd125/rss "Subscribe to 老刘忙不忙")
* [邢红瑞的blog](http://blogger.org.cn/blog/blog.asp?name=hongrui) [(rss)](http://blogger.org.cn/blog/rss2.asp?name=hongrui "Subscribe to 邢红瑞的blog")

### 积分与排名

* 积分 - 243427
* 排名 - 45

### 最新评论 [![]()](http://www.blogjava.net/rosen/CommentsRSS.aspx)

* [1. re: 使用Memory Analyzer tool(MAT)分析内存泄漏（二）](http://www.blogjava.net/rosen/archive/2010/06/14/323522.html#323567)
* @Jet
@滴水
@BeanSoft
感谢各位的关注！非常感谢。
* --Rosen
* [2. re: 使用Memory Analyzer tool(MAT)分析内存泄漏（二）](http://www.blogjava.net/rosen/archive/2010/06/14/323522.html#323561)
* 经典文章!
* --BeanSoft
* [3. re: 使用Memory Analyzer tool(MAT)分析内存泄漏（二）](http://www.blogjava.net/rosen/archive/2010/06/14/323522.html#323559)
* 很棒，几种分类测试让人更加深刻的理解了JVM的内存分配机制。
* --滴水
* [4. re: 使用Memory Analyzer tool(MAT)分析内存泄漏（二）[未登录]](http://www.blogjava.net/rosen/archive/2010/06/13/323522.html#323527)
* 好文章啊，支持一直写下去~~
* --Jet
* [5. re: 使用Memory Analyzer tool(MAT)分析内存泄漏（一）](http://www.blogjava.net/rosen/archive/2010/05/25/321575.html#321837)
* 很精辟，感谢博主，你的前途一定会有。
* --liucr

### 阅读排行榜

* [1. Java 中的位运算(13441)](http://www.blogjava.net/rosen/archive/2005/08/12/9955.html)
* [2. CentOS 5 Apache,Java,Tomcat,PostgreSQL...安装笔记（不断更新中）(10024)](http://www.blogjava.net/rosen/archive/2007/06/04/121994.html)
* [3. Hibernate、iBATIS 与 BLOB(7417)](http://www.blogjava.net/rosen/archive/2005/08/12/9937.html)
* [4. OSWorkflow 探索(7122)](http://www.blogjava.net/rosen/archive/2005/08/12/9880.html)
* [5. BIRT 总览（翻译）(6576)](http://www.blogjava.net/rosen/archive/2005/12/17/24348.html)

### 评论排行榜

* [1. 德国申根商务签证攻略（成都版）(36)](http://www.blogjava.net/rosen/archive/2008/03/21/187814.html)
* [2. OSWorkflow 探索(29)](http://www.blogjava.net/rosen/archive/2005/08/12/9880.html)
* [3. 北漂找工作经历(26)](http://www.blogjava.net/rosen/archive/2008/07/25/217210.html)
* [4. 开源面向对象数据库 db4o 之旅: 初识 db4o“db4o 之旅（一）”(21)](http://www.blogjava.net/rosen/archive/2006/06/15/53094.html)
* [5. 致《点击Tree中的一行打开/关闭节点》一文作者(17)](http://www.blogjava.net/rosen/archive/2008/09/02/226348.html)
[使用Memory Analyzer tool(MAT)分析内存泄漏（二）](http://www.blogjava.net/rosen/archive/2010/06/13/323522.html)

**前言的前言**
写blog就是好，在大前提下可以想说什么写什么，不像投稿那么字字斟酌。上周末回了趟成都办事，所以本文来迟了。K117从达州经由达成线往成都方向走的时候，发现铁路边有条河，尽管我现在也不知道其名字，但已被其深深的陶醉。河很宽且水流平缓，河边山丘森林密布，民房星星点点的分布在河边，河里偶尔些小船。当时我就在想，在这里生活是多么的惬意，夏天还可以下去畅游一番，闲来无事也可垂钓。唉，越来越讨厌北漂了。
**前言**
在[使用Memory Analyzer tool(MAT)分析内存泄漏（一）](http://www.blogjava.net/rosen/archive/2010/05/21/321575.html)中，我介绍了内存泄漏的前因后果。在本文中，将介绍MAT如何根据heap dump分析泄漏根源。由于测试范例可能过于简单，很容易找出问题，但我期待借此举一反三。
一开始不得不说说ClassLoader，本质上，它的工作就是把磁盘上的类文件读入内存，然后调用java.lang.ClassLoader.defineClass方法告诉系统把内存镜像处理成合法的字节码。Java提供了抽象类ClassLoader，所有用户自定义类装载器都实例化自ClassLoader的子类。system class loader在没有指定装载器的情况下默认装载用户类，在Sun Java 1.5中既sun.misc.Launcher$AppClassLoader。更详细的内容请参看下面的资料。
**准备heap dump**
请看下面的Pilot类，没啥特殊的。
/**
 * Pilot class
 * @author rosen jiang
 */
package org.rosenjiang.bo;
public class Pilot{
    
    String name;
    int age;
    
    public Pilot(String a, int b){
        name = a;
        age = b;
    }
}
然后再看OOMHeapTest类，它是如何撑破heap dump的。
/**
 * OOMHeapTest class
 * @author rosen jiang
 */
package org.rosenjiang.test;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;
import org.rosenjiang.bo.Pilot;
public class OOMHeapTest {
    public static void main(String[] args){
        oom();
    }
    
    private static void oom(){
        Map<String, Pilot> map = new HashMap<String, Pilot>();
        Object[] array = new Object[1000000];
        for(int i=0; i<1000000; i++){
            String d = new Date().toString();
            Pilot p = new Pilot(d, i);
            map.put(i+"rosen jiang", p);
            array[i]=p;
        }
    }
}
是的，上面构造了很多的Pilot类实例，向数组和map中放。由于是Strong Ref，GC自然不会回收这些对象，一直放在heap中直到溢出。当然在运行前，先要在Eclipse中配置VM参数-XX:+HeapDumpOnOutOfMemoryError。好了，一会儿功夫内存溢出，控制台打出如下信息。
java.lang.OutOfMemoryError: Java heap space
Dumping heap to java_pid3600.hprof ![]()
Heap dump file created [78233961 bytes in 1.995 secs]
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
![]()![]()![]()![]()![]()
![]()![]()![]()
![]()![]()
java_pid3600.hprof既是heap dump，可以在OOMHeapTest类所在的工程根目录下找到。
**MAT安装**
话分两头说，有了heap dump还得安装MAT。可以在http://www.eclipse.org/mat/downloads.php选择合适的方式安装。安装完成后切换到Memory Analyzer视图。在Eclipse的左上角有Open Heap Dump按钮，按照刚才说的路径找到java_pid3600.hprof文件并打开。解析hprof文件会花些时间，然后会弹出向导，直接Finish即可。稍后会看到下图所示的界面。
![]()
MAT工具分析了heap dump后在界面上非常直观的展示了一个饼图，该图深色区域被怀疑有内存泄漏，可以发现整个heap才64M内存，深色区域就占了99.5%。接下来是一个简短的描述，告诉我们main线程占用了大量内存，并且明确指出system class loader加载的"java.lang.Thread"实例有内存聚集，并建议用关键字"java.lang.Thread"进行检查。所以，MAT通过简单的两句话就说明了问题所在，就算使用者没什么处理内存问题的经验。在下面还有一个"Details"链接，在点开之前不妨考虑一个问题：为何对象实例会聚集在内存中，为何存活（而未被GC）？是的——Strong Ref，那么再走近一些吧。
![]()
点击了"Details"链接之后，除了在上一页看到的描述外，还有Shortest Paths To the Accumulation Point和Accumulated Objects部分，这里说明了从GC root到聚集点的最短路径，以及完整的reference chain。观察Accumulated Objects部分，java.util.HashMap和java.lang.Object[1000000]实例的retained heap(size)最大，在上一篇文章中我们知道retained heap代表从该类实例沿着reference chain往下所能收集到的其他类实例的shallow heap(size)总和，所以明显类实例都聚集在HashMap和Object数组中了。这里我们发现一个有趣的现象，既Object数组的shallow heap和retained heap竟然一样，通过[Shallow and retained sizes](http://www.yourkit.com/docs/90/help/sizes.jsp)一文可知，数组的shallow heap和一般对象（非数组）不同，依赖于数组的长度和里面的元素的类型，对数组求shallow heap，也就是求数组集合内所有对象的shallow heap之和。好，再来看org.rosenjiang.bo.Pilot对象实例的shallow heap为何是16，因为对象头是8字节，成员变量int是4字节、String引用是4字节，故总共16字节。
![]()
接着往下看，来到了Accumulated Objects by Class区域，顾名思义，这里能找到被聚集的对象实例的类名。org.rosenjiang.bo.Pilot类上头条了，被实例化了290,325次，再返回去看程序，我承认是故意这么干的。还有很多有用的报告可用来协助分析问题，只是本文中的例子太简单，也用不上。以后如有用到，一定撰文详细叙述。
**又是perm gen**
我们在上一篇文章中知道，perm gen是个异类，里面存储了类和方法数据（与class loader有关）以及interned strings（字符串驻留）。在heap dump中没有包含太多的perm gen信息。那么我们就用这些少量的信息来解决问题吧。
看下面的代码，利用interned strings把perm gen撑破了。
/**
 * OOMPermTest class
 * @author rosen jiang
 */
package org.rosenjiang.test;
public class OOMPermTest {
    public static void main(String[] args){
        oom();
    }
    
    private static void oom(){
        Object[] array = new Object[10000000];
        for(int i=0; i<10000000; i++){
            String d = String.valueOf(i).intern();
            array[i]=d;
        }
    }
}
控制台打印如下的信息，然后把java_pid1824.hprof文件导入到MAT。其实在MAT里，看到的状况应该和“OutOfMemoryError: Java heap space”差不多（用了数组），因为heap dump并没有包含interned strings方面的任何信息。只是在这里需要强调，使用intern()方法的时候应该多加注意。
java.lang.OutOfMemoryError: PermGen space
Dumping heap to java_pid1824.hprof ![]()
Heap dump file created [121273334 bytes in 2.845 secs]
Exception in thread "main" java.lang.OutOfMemoryError: PermGen space
![]()![]()![]()![]()![]()
![]()![]()![]()
![]()![]()
倒是在思考如何把class loader撑破废了些心思。经过尝试，发现使用ASM来动态生成类才能达到目的。ASM(http://asm.objectweb.org)的主要作用是处理已编译类(compiled class)，能对已编译类进行生成、转换、分析（功能之一是实现动态代理），而且它运行起来足够的快和小巧，文档也全面，实属居家必备之良品。ASM提供了core API和tree API，前者是基于事件的方式，后者是基于对象的方式，类似于XML的SAX、DOM解析，但是使用tree API性能会有损失。既然下面要用到ASM，这里不得不啰嗦下已编译类的结构，包括：
    1、修饰符（例如public、private）、类名、父类名、接口和annotation部分。
    2、类成员变量声明，包括每个成员的修饰符、名字、类型和annotation。
    3、方法和构造函数描述，包括修饰符、名字、返回和传入参数类型，以及annotation。当然还包括这些方法或构造函数的具体Java字节码。
    4、常量池(constant pool)部分，constant pool是一个包含类中出现的数字、字符串、类型常量的数组。
![]()
已编译类和原来的类源码区别在于，已编译类只包含类本身，内部类不会在已编译类中出现，而是生成另外一个已编译类文件；其二，已编译类中没有注释；其三，已编译类没有package和import部分。
这里还得说说已编译类对Java类型的描述，对于原始类型由单个大写字母表示，Z代表boolean、C代表char、B代表byte、S代表short、I代表int、F代表float、J代表long、D代表double；而对类类型的描述使用内部名(internal name)外加前缀L和后面的分号共同表示来表示，所谓内部名就是带全包路径的表示法，例如String的内部名是java/lang/String；对于数组类型，使用单方括号加上数据元素类型的方式描述。最后对于方法的描述，用圆括号来表示，如果返回是void用V表示，具体参考下图。
![]()![]()
下面的代码中会使用ASM core API，注意接口ClassVisitor是核心，FieldVisitor、MethodVisitor都是辅助接口。ClassVisitor应该按照这样的方式来调用：visit visitSource? visitOuterClass? ( visitAnnotation | visitAttribute )*( visitInnerClass | visitField | visitMethod )* visitEnd。就是说visit方法必须首先调用，再调用最多一次的visitSource，再调用最多一次的visitOuterClass方法，接下来再多次调用visitAnnotation和visitAttribute方法，最后是多次调用visitInnerClass、visitField和visitMethod方法。调用完后再调用visitEnd方法作为结尾。
注意ClassWriter类，该类实现了ClassVisitor接口，通过toByteArray方法可以把已编译类直接构建成二进制形式。由于我们要动态生成子类，所以这里只对ClassWriter感兴趣。首先是抽象类原型：
/**
 * @author rosen jiang
 * MyAbsClass class
 */
package org.rosenjiang.test;
public abstract class MyAbsClass {
    int LESS = -1;
    int EQUAL = 0;
    int GREATER = 1;
    abstract int absTo(Object o);
}
其次是自定义类加载器，实在没法，ClassLoader的defineClass方法都是protected的，要加载字节数组形式（因为toByteArray了）的类只有继承一下自己再实现。
/**
 * @author rosen jiang
 * MyClassLoader class
 */
package org.rosenjiang.test;
public class MyClassLoader extends ClassLoader {
    public Class defineClass(String name, byte[] b) {
        return defineClass(name, b, 0, b.length);
    }
}
最后是测试类。
/**
 * @author rosen jiang
 * OOMPermTest class
 */
package org.rosenjiang.test;
import java.util.ArrayList;
import java.util.List;
import org.objectweb.asm.ClassWriter;
import org.objectweb.asm.Opcodes;
public class OOMPermTest {
    public static void main(String[] args) {
        OOMPermTest o = new OOMPermTest();
        o.oom();
    }
    private void oom() {
        try {
            ClassWriter cw = new ClassWriter(0);
            cw.visit(Opcodes.V1_5, Opcodes.ACC_PUBLIC + Opcodes.ACC_ABSTRACT,
            "org/rosenjiang/test/MyAbsClass", null, "java/lang/Object",
            new String[] {});
            cw.visitField(Opcodes.ACC_PUBLIC + Opcodes.ACC_FINAL + Opcodes.ACC_STATIC, "LESS", "I",
            null, new Integer(-1)).visitEnd();
            cw.visitField(Opcodes.ACC_PUBLIC + Opcodes.ACC_FINAL + Opcodes.ACC_STATIC, "EQUAL", "I",
            null, new Integer(0)).visitEnd();
            cw.visitField(Opcodes.ACC_PUBLIC + Opcodes.ACC_FINAL + Opcodes.ACC_STATIC, "GREATER", "I",
            null, new Integer(1)).visitEnd();
            cw.visitMethod(Opcodes.ACC_PUBLIC + Opcodes.ACC_ABSTRACT, "absTo",
            "(Ljava/lang/Object;)I", null, null).visitEnd();
            cw.visitEnd();
            byte[] b = cw.toByteArray();
            List<ClassLoader> classLoaders = new ArrayList<ClassLoader>();
            while (true) {
                MyClassLoader classLoader = new MyClassLoader();
                classLoader.defineClass("org.rosenjiang.test.MyAbsClass", b);
                classLoaders.add(classLoader);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
不一会儿，控制台就报错了。
java.lang.OutOfMemoryError: PermGen space
Dumping heap to java_pid3023.hprof ![]()
Heap dump file created [92593641 bytes in 2.405 secs]
Exception in thread "main" java.lang.OutOfMemoryError: PermGen space
![]()![]()
![]()
打开java_pid3023.hprof文件，注意看下图的Classes: 88.1k和Class Loader: 87.7k部分，从这点可看出class loader加载了大量的类。
![]()
更进一步分析，点击上图中红框线圈起来的按钮，选择Java Basics——Class Loader Explorer功能。打开后能看到下图所示的界面，第一列是class loader名字；第二列是class loader已定义类(defined classes)的个数，这里要说一下已定义类和已加载类(loaded classes)了，当需要加载类的时候，相应的class loader会首先把请求委派给父class loader，只有当父class loader加载失败后，该class loader才会自己定义并加载类，这就是Java自己的“双亲委派加载链”结构；第三列是class loader所加载的类的实例数目。
![]()
在Class Loader Explorer这里，能发现class loader是否加载了过多的类。另外，还有Duplicate Classes功能，也能协助分析重复加载的类，在此就不再截图了，可以肯定的是MyAbsClass被重复加载了N多次。
**最后**
其实MAT工具非常的强大，上面故弄玄虚的范例代码根本用不上MAT的其他分析功能，所以就不再描述了。其实对于OOM不只我列举的两种溢出错误，还有多种其他错误，但我想说的是，对于perm gen，如果实在找不出问题所在，建议使用JVM的-verbose参数，该参数会在后台打印出日志，可以用来查看哪个class loader加载了什么类，例：“[Loaded org.rosenjiang.test.MyAbsClass from org.rosenjiang.test.MyClassLoader]”。
全文完。
**参考资料**
[memoryanalyzer Blog](http://dev.eclipse.org/blogs/memoryanalyzer/)
[java类加载器体系结构](http://kenwublog.com/structure-of-java-class-loader)
[ClassLoader](http://mindprod.com/jgloss/classloader.html)

[**请注意！引用、转贴本文应注明原作者：Rosen Jiang 以及出处：**](http://www.yourkit.com/docs/90/help/gc_roots.jsp)[**http://www.blogjava.net/rosen**](http://www.blogjava.net/rosen/archive/2010/06/archive/2010/05/archive/2010/rosen/archive/2010/rosen/archive/2009/10/archive/2009/09/archive/2009/07/archive/2009/06/archive/2009/04/archive/2009/03/archive/2009/02/archive/2008/12/archive/2008/rosen)
posted on 2010-06-13 16:13 [Rosen](http://www.blogjava.net/rosen/) 阅读(1184) [评论(4)](http://www.blogjava.net/rosen/archive/2010/06/13/323522.html#Post)  [编辑](http://www.blogjava.net/rosen/admin/EditPosts.aspx?postid=323522)  [收藏](http://www.blogjava.net/rosen/AddToFavorite.aspx?id=323522) 所属分类: [Java 基础](http://www.blogjava.net/rosen/category/2690.html)![]()

[]()[]()

[
### 评论

[#](http://www.blogjava.net/rosen/archive/2010/06/13/323522.html#323527 "permalink: re: 使用Memory Analyzer tool(MAT)分析内存泄漏（二）[未登录]") []()re: 使用Memory Analyzer tool(MAT)分析内存泄漏（二）[未登录] 2010-06-13 16:31 [Jet]()

好文章啊，支持一直写下去~~  [回复](http://www.blogjava.net/rosen/archive/2010/06/13/323522.html#post)  [更多评论](http://www.blogjava.net/comment?author=Jet "查看该作者发表过的评论")
[]()  []()]()
[#](http://www.blogjava.net/rosen/archive/2010/06/13/323522.html#323559 "permalink: re: 使用Memory Analyzer tool(MAT)分析内存泄漏（二）") []()re: 使用Memory Analyzer tool(MAT)分析内存泄漏（二） 2010-06-14 14:11 [滴水]()

很棒，几种分类测试让人更加深刻的理解了JVM的内存分配机制。  [回复](http://www.blogjava.net/rosen/archive/2010/06/13/323522.html#post)  [更多评论](http://www.blogjava.net/comment?author=%e6%bb%b4%e6%b0%b4 "查看该作者发表过的评论")
[]()  []()
[#](http://www.blogjava.net/rosen/archive/2010/06/13/323522.html#323561 "permalink: re: 使用Memory Analyzer tool(MAT)分析内存泄漏（二）") []()re: 使用Memory Analyzer tool(MAT)分析内存泄漏（二） 2010-06-14 14:50 [BeanSoft](http://www.blogjava.net/beansoft/)

经典文章!  [回复](http://www.blogjava.net/rosen/archive/2010/06/13/323522.html#post)  [更多评论](http://www.blogjava.net/comment?author=BeanSoft "查看该作者发表过的评论")
[]()  []()
[#](http://www.blogjava.net/rosen/archive/2010/06/13/323522.html#323567 "permalink: re: 使用Memory Analyzer tool(MAT)分析内存泄漏（二）") []()re: 使用Memory Analyzer tool(MAT)分析内存泄漏（二）[]() 2010-06-14 18:12 [Rosen](http://www.blogjava.net/rosen/)

@Jet
@滴水
@BeanSoft
感谢各位的关注！非常感谢。  [回复](http://www.blogjava.net/rosen/archive/2010/06/13/323522.html#post)  [更多评论](http://www.blogjava.net/comment?author=Rosen "查看该作者发表过的评论")
[]()  []()
[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() 推荐链接：
[众里寻你千百度，百度期待您的加盟](http://a4.yeshj.com/rd/35698/)
[软件研发团队管理年会(7.10-7.11)](http://a4.yeshj.com/rd/35449/)
[博客园2010T恤正式发布](http://www.cnblogs.com/cmt/archive/2010/06/08/1753881.html)

IT新闻：
· [富士通东芝同意合并手机业务 10月建合资公司](http://news.cnblogs.com/n/66449/)
· [微软拼音输入法15周年](http://news.cnblogs.com/n/66448/)
· [苹果设备占美国移动上网流量58.8%](http://news.cnblogs.com/n/66447/)
· [微软改变Office营销策略：允许厂商捆绑](http://news.cnblogs.com/n/66446/)
· [木马来袭：Linux IRC服务器软件发现后门](http://news.cnblogs.com/n/66445/)    [博客园首页](http://www.cnblogs.com/)  [学英语](http://a4.yeshj.com/rd/34138/)  [IT新闻](http://news.cnblogs.com/)  [知识库](http://kb.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/rosen/archive/2010/06/13/323522.html&SourceURL=/rosen/archive/2010/06/13/323522.html)       [使用Ctrl+Enter键可以直接提交]   [每天10分钟，轻松学英语](http://a4.yeshj.com/rd/34138/)  推荐职位：
· [Web前端研发工程师 (百度)](http://job.cnblogs.com/offer/7699/)
· [数据挖掘工程师(百度)](http://job.cnblogs.com/offer/7697/)
· [PHP研发工程师(百度)](http://job.cnblogs.com/offer/7696/)
· [急聘进销存软件开发经理(北京拜思腾信息)](http://job.cnblogs.com/Offer/7582/)
· [高级.NET开发工程师(盛大网络)](http://job.cnblogs.com/offer/7378/)
· [Coolite VS2008 开发工程师 (深圳创新通软)](http://job.cnblogs.com/offer/7515/)
· [分布式系统开发高级工程师(深圳创新科)](http://job.cnblogs.com/offer/7479/)
· [Linux开发工程师(深圳创新科)](http://job.cnblogs.com/offer/7474/)

博客园首页随笔：
· [WinForm下PictureBox和Panel控件的On_Paint事件有何区别](http://www.cnblogs.com/81/archive/2010/06/17/1759596.html)
· [ChartPart 图表显示](http://www.cnblogs.com/Sunmoonfire/archive/2010/06/17/1759506.html)
· [随手写个Repeater分页控件，奉上自己源码，欢迎大虾挑刺](http://www.cnblogs.com/yangtongnet/archive/2010/06/17/1759569.html)
· [浅谈AnkhSvn](http://www.cnblogs.com/wynsdie/archive/2010/06/17/1759551.html)
· [Android小项目之--前台界面与用户交互的对接 进度条与拖动条（附源码）](http://www.cnblogs.com/TerryBlog/archive/2010/06/17/1759542.html)
知识库：
· [关于Page Fault的一些整理](http://kb.cnblogs.com/page/66268/)
· [我在南大的七年](http://kb.cnblogs.com/page/65995/)
· [编写跨浏览器兼容的 CSS 代码的金科玉律](http://kb.cnblogs.com/page/65966/)
· [程序员应知——首先检查自己的问题](http://kb.cnblogs.com/page/65875/)
· [程序员的语言“艳遇史”（五）——办公室秘书Smalltalk](http://kb.cnblogs.com/page/65843/)  网站导航:

[博客园](http://www.cnblogs.com/)   [IT新闻](http://news.cnblogs.com/)   [个人主页](http://home.cnblogs.com/)   [博客生活](http://www.cnweblog.com/)   [IT博客网](http://www.cnitblog.com/)   [C++博客](http://www.cppblog.com/)   [博客园社区](http://space.cnblogs.com/) [管理](http://www.blogjava.net/rosen/archive/2010/06/13/323522.html?opt=admin) 相关文章:

* [使用Memory Analyzer tool(MAT)分析内存泄漏（二）](http://www.blogjava.net/rosen/archive/2010/06/13/323522.html)
* [使用Memory Analyzer tool(MAT)分析内存泄漏（一）](http://www.blogjava.net/rosen/archive/2010/05/21/321575.html)
* [VSS Plugin配置FAQ（翻译）](http://www.blogjava.net/rosen/archive/2007/10/26/156286.html)
* [Jakarta-ORO 分解 IP 地址](http://www.blogjava.net/rosen/archive/2005/10/25/16729.html)
* [利用反射从 XML 构造 VO](http://www.blogjava.net/rosen/archive/2005/08/12/9974.html)
* [Java 短路运算符和非短路运算符](http://www.blogjava.net/rosen/archive/2005/08/12/9956.html)
* [Java 中的位运算](http://www.blogjava.net/rosen/archive/2005/08/12/9955.html)
* [Java 工具，你用了吗？（翻译）](http://www.blogjava.net/rosen/archive/2005/08/12/9954.html)
* [循证克隆](http://www.blogjava.net/rosen/archive/2005/08/12/9951.html)
* [在 Eclipse 中使用 JUnit（翻译）](http://www.blogjava.net/rosen/archive/2005/08/12/9945.html)  

Powered by:
[BlogJava](http://www.blogjava.net/)
Copyright © Rosen
