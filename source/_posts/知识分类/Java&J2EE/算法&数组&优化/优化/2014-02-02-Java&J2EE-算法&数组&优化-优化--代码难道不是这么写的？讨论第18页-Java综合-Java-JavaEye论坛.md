---
layout: post
title: "代码难道不是这么写的？讨论第18页 - Java综合 - Java - JavaEye论坛"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 优化
--- 

# 代码难道不是这么写的？讨论第18页 - Java综合 - Java - JavaEye论坛

[您还未登录 !](http://www.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://www.javaeye.com/login) [注册](http://www.javaeye.com/signup)

[![JavaEye-最棒的软件开发交流社区]( "JavaEye-最棒的软件开发交流社区")](http://www.javaeye.com/)

[论坛首页](http://www.javaeye.com/forums) → [Java编程和Java企业应用版](http://www.javaeye.com/forums/board/Java) → [Java综合](http://www.javaeye.com/forums/tag/Java) →

# 代码难道不是这么写的？

[全部](http://www.javaeye.com/forums/board/Java) [Hibernate](http://www.javaeye.com/forums/tag/Hibernate) [Spring](http://www.javaeye.com/forums/tag/Spring) [Struts](http://www.javaeye.com/forums/tag/Struts) [iBATIS](http://www.javaeye.com/forums/tag/iBATIS) [企业应用](http://www.javaeye.com/forums/tag/J2EE) [设计模式](http://www.javaeye.com/forums/tag/Design-Pattern) [DAO](http://www.javaeye.com/forums/tag/DAO) [领域模型](http://www.javaeye.com/forums/tag/Object-Domain) [OO](http://www.javaeye.com/forums/tag/OO) [Tomcat](http://www.javaeye.com/forums/tag/Tomcat) [SOA](http://www.javaeye.com/forums/tag/SOA) [JBoss](http://www.javaeye.com/forums/tag/JBoss) [Swing](http://www.javaeye.com/forums/tag/Swing) [Java综合](http://www.javaeye.com/forums/tag/Java)
[](http://www.javaeye.com/forums/39/topics/722599/posts/new "发表回复")

[myApps快速开发平台，配置即开发、所见即所得，节约85%工作量](http://www.javaeye.com/clicks/324)

[« 上一页](http://www.javaeye.com/topic/722599?page=17) [1](http://www.javaeye.com/topic/722599?page=1) [2](http://www.javaeye.com/topic/722599?page=2) … [17](http://www.javaeye.com/topic/722599?page=17) 18 [19](http://www.javaeye.com/topic/722599?page=19) [20](http://www.javaeye.com/topic/722599?page=20) [下一页 »](http://www.javaeye.com/topic/722599?page=19)
浏览 21808 次
 [主题：代码难道不是这么写的？](http://www.javaeye.com/topic/722599)

**该帖已经被评为良好帖** 作者 正文 * moonranger
* 等级: 初级会员
* [![moonranger的博客]( "moonranger的博客: moonranger")](http://moonranger.javaeye.com/)
* 文章: 44
* 积分: 40
* 来自: 天津
* ![]()
    发表时间：2010-07-30  

[<](http://www.javaeye.com/topic/722599?page=18#) [>](http://www.javaeye.com/topic/722599?page=18#) 猎头职位:

qiushily2030 写道
fengfeng925 写道

qiushily2030 写道

A a;
申明在内部，每次遍历都申明一个a的变量.  
申明在外面 只申明一次 一直复用.  申明也是要资源的..
一个变量就相当于一个指针,循环1亿次,LZ你说呢.
评审官说的还是有理的,不过这个得看需求.  JVM回收是在内存不足的时候.
  还是不要想当然了,我就那么干过一回. 程序是最真实的答案...
简直是TMD胡扯。
JVM回收是在内存不足的时候.这个已经修正. 不是内存不足才调用。。
你说我胡扯？ 那for里定义的局部变量放哪里？不要也和上面一个的观点一样：只申明一次,后调用索引.
这个我不可理解。。
方法的参数连同方法里的变量所占用的空间，在编译时就已经确定了，它决定了运行时一个方法被调用的时侯要分配多大的栈帧。一个变量放在循环外还是循环里，只是在编译期限定了其作用域而已，到了运行时是没有区别的（也许只是在栈帧的local variables区里的位置不同）。
看一下方法的字节码，一切都非常明了：
private final int[] numbers = {1, 2, 3, 4, 5}; public void varOutsideLoop() { int n; for (int i = 0; i < numbers.length; i++) { n = numbers[i]; System.out.println(n); } } public void varInsideLoop() { for (int i = 0; i < numbers.length; i++) { int n = numbers[i]; System.out.println(n); } }
对应的字节码：
public void varOutsideLoop(); Signature: ()V Code: Stack=2, Locals=3, Args_size=1 0: iconst_0 1: istore_2 2: goto 22 5: aload_0 6: getfield #12; //Field numbers:[I 9: iload_2 10: iaload 11: istore_1 12: getstatic #19; //Field java/lang/System.out:Ljava/io/PrintStream; 15: iload_1 16: invokevirtual #25; //Method java/io/PrintStream.println:(I)V 19: iinc 2, 1 22: iload_2 23: aload_0 24: getfield #12; //Field numbers:[I 27: arraylength 28: if_icmplt 5 31: return LineNumberTable: line 43: 0 line 44: 5 line 45: 12 line 43: 19 line 47: 31 LocalVariableTable: Start Length Slot Name Signature 0 32 0 this Lcom/jstudio/learnjvm/classloader/VarInLoop; 12 10 1 n I 2 29 2 i I public void varInsideLoop(); Signature: ()V Code: Stack=2, Locals=3, Args_size=1 0: iconst_0 1: istore_1 2: goto 22 5: aload_0 6: getfield #12; //Field numbers:[I 9: iload_1 10: iaload 11: istore_2 12: getstatic #19; //Field java/lang/System.out:Ljava/io/PrintStream; 15: iload_2 16: invokevirtual #25; //Method java/io/PrintStream.println:(I)V 19: iinc 1, 1 22: iload_1 23: aload_0 24: getfield #12; //Field numbers:[I 27: arraylength 28: if_icmplt 5 31: return LineNumberTable: line 50: 0 line 51: 5 line 52: 12 line 50: 19 line 54: 31 LocalVariableTable: Start Length Slot Name Signature 0 32 0 this Lcom/jstudio/learnjvm/classloader/VarInLoop; 2 29 1 i I 12 7 2 n I
仔细看字节码就能发现，除了n和循环变量i在local variable table中的位置不同，其他的所有的都没有区别。
varOutsideLoop那个方法里我故意没有初始化n，如果加上初始化，就会多一条iconst和一条istore指令。
至于位置不一样的原因，个人猜测与变量出现的位置有关，n在循环外的版本，它出现在前，所以占据了第一个slot（第0个slot是对象本身，即this）；n在循环里的版本，循环变量i出现在前，所以i占据了第一个slot，而n占用的是第二个slot。
除此之外，两个方法没有任何的区别。
mercyblitz 写道

主要你误解了a变量在遍历中，有入栈和出栈的操作。并没有重复开辟局部变量，再说JVM优化之后，不会重复创建。
个人认为这种说法也不太对，在一个方法内部不会有所谓的“入栈”和“出栈”操作。方法里，一个变量占用一个local variable table的slot，一个萝卜一个坑。所有这些变量都是在方法结束，堆栈帧被销毁的时侯才真正被“回收”，即使方法结束的时侯它早以超出了自己的作用域……
实践中，变量的作用域当然是尽可能小才好，即有利于垃圾收集、也消除了一些潜在的导致bug的隐患。
楼主别理那个“代码评审官”就好……话说如果我遇到这种人这种事，我八成不愿继续干下去…… [返回顶楼](http://www.javaeye.com/topic/722599?page=18#) [ ](http://moonranger.javaeye.com/ "浏览作者的博客") [ ](http://moonranger.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=moonranger "发送站内短信") [ ](http://moonranger.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=moonranger "关注作者") [回帖地址](http://www.javaeye.com/topic/722599?page=18#1603622 "moonranger回帖:代码难道不是这么写的？")

[5](http://www.javaeye.com/topic/722599?page=18# "好") [0](http://www.javaeye.com/topic/722599?page=18# "差") 请登录后投票  * mercyblitz
* 等级: 初级会员
* [![mercyblitz的博客]( "mercyblitz的博客: mErcy blog")](http://mercyblitz.javaeye.com/)
* 文章: 465
* 积分: 30
* 来自: 长沙市
* ![]()
    发表时间：2010-07-30  

moonranger 写道

qiushily2030 写道

fengfeng925 写道

qiushily2030 写道

A a;
申明在内部，每次遍历都申明一个a的变量.  
申明在外面 只申明一次 一直复用.  申明也是要资源的..
一个变量就相当于一个指针,循环1亿次,LZ你说呢.
评审官说的还是有理的,不过这个得看需求.  JVM回收是在内存不足的时候.
  还是不要想当然了,我就那么干过一回. 程序是最真实的答案...
简直是TMD胡扯。
JVM回收是在内存不足的时候.这个已经修正. 不是内存不足才调用。。
你说我胡扯？ 那for里定义的局部变量放哪里？不要也和上面一个的观点一样：只申明一次,后调用索引.
这个我不可理解。。
方法的参数连同方法里的变量所占用的空间，在编译时就已经确定了，它决定了运行时一个方法被调用的时侯要分配多大的栈帧。一个变量放在循环外还是循环里，只是在编译期限定了其作用域而已，到了运行时是没有区别的（也许只是在栈帧的local variables区里的位置不同）。
看一下方法的字节码，一切都非常明了：
private final int[] numbers = {1, 2, 3, 4, 5}; public void varOutsideLoop() { int n; for (int i = 0; i < numbers.length; i++) { n = numbers[i]; System.out.println(n); } } public void varInsideLoop() { for (int i = 0; i < numbers.length; i++) { int n = numbers[i]; System.out.println(n); } }
对应的字节码：
public void varOutsideLoop(); Signature: ()V Code: Stack=2, Locals=3, Args_size=1 0: iconst_0 1: istore_2 2: goto 22 5: aload_0 6: getfield #12; //Field numbers:[I 9: iload_2 10: iaload 11: istore_1 12: getstatic #19; //Field java/lang/System.out:Ljava/io/PrintStream; 15: iload_1 16: invokevirtual #25; //Method java/io/PrintStream.println:(I)V 19: iinc 2, 1 22: iload_2 23: aload_0 24: getfield #12; //Field numbers:[I 27: arraylength 28: if_icmplt 5 31: return LineNumberTable: line 43: 0 line 44: 5 line 45: 12 line 43: 19 line 47: 31 LocalVariableTable: Start Length Slot Name Signature 0 32 0 this Lcom/jstudio/learnjvm/classloader/VarInLoop; 12 10 1 n I 2 29 2 i I public void varInsideLoop(); Signature: ()V Code: Stack=2, Locals=3, Args_size=1 0: iconst_0 1: istore_1 2: goto 22 5: aload_0 6: getfield #12; //Field numbers:[I 9: iload_1 10: iaload 11: istore_2 12: getstatic #19; //Field java/lang/System.out:Ljava/io/PrintStream; 15: iload_2 16: invokevirtual #25; //Method java/io/PrintStream.println:(I)V 19: iinc 1, 1 22: iload_1 23: aload_0 24: getfield #12; //Field numbers:[I 27: arraylength 28: if_icmplt 5 31: return LineNumberTable: line 50: 0 line 51: 5 line 52: 12 line 50: 19 line 54: 31 LocalVariableTable: Start Length Slot Name Signature 0 32 0 this Lcom/jstudio/learnjvm/classloader/VarInLoop; 2 29 1 i I 12 7 2 n I
仔细看字节码就能发现，除了n和循环变量i在local variable table中的位置不同，其他的所有的都没有区别。
varOutsideLoop那个方法里我故意没有初始化n，如果加上初始化，就会多一条iconst和一条istore指令。
至于位置不一样的原因，个人猜测与变量出现的位置有关，n在循环外的版本，它出现在前，所以占据了第一个slot（第0个slot是对象本身，即this）；n在循环里的版本，循环变量i出现在前，所以i占据了第一个slot，而n占用的是第二个slot。
除此之外，两个方法没有任何的区别。
mercyblitz 写道

主要你误解了a变量在遍历中，有入栈和出栈的操作。并没有重复开辟局部变量，再说JVM优化之后，不会重复创建。
个人认为这种说法也不太对，在一个方法内部不会有所谓的“入栈”和“出栈”操作。方法里，一个变量占用一个local variable table的slot，一个萝卜一个坑。所有这些变量都是在方法结束，堆栈帧被销毁的时侯才真正被“回收”，即使方法结束的时侯它早以超出了自己的作用域……
实践中，变量的作用域当然是尽可能小才好，即有利于垃圾收集、也消除了一些潜在的导致bug的隐患。
楼主别理那个“代码评审官”就好……话说如果我遇到这种人这种事，我八成不愿继续干下去……
恩，谢谢指正，我也仔细想了一下。在楼主的例子中，字节码说明两个局部变量指向不同的地方。不会重复开辟。要想开辟更多，哪么需要不同名称的变量申明。 [返回顶楼](http://www.javaeye.com/topic/722599?page=18#) [ ](http://mercyblitz.javaeye.com/ "浏览作者的博客") [ ](http://mercyblitz.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=mercyblitz "发送站内短信") [ ](http://mercyblitz.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=mercyblitz "关注作者") [回帖地址](http://www.javaeye.com/topic/722599?page=18#1603661 "mercyblitz回帖:代码难道不是这么写的？")

[0](http://www.javaeye.com/topic/722599?page=18# "好") [0](http://www.javaeye.com/topic/722599?page=18# "差") 请登录后投票  * cisumer
* 等级: 初级会员
* [![cisumer的博客]( "cisumer的博客: ")](http://cisumer.javaeye.com/)
* 文章: 4
* 积分: 30
* 来自: 上海
* [![]()](http://www.javaeye.com/blog/479667 "我正在读《Java 位运算》")
    发表时间：2010-07-30  

ei0 写道

我只对s感兴趣！！！ [返回顶楼](http://www.javaeye.com/topic/722599?page=18#) [ ](http://cisumer.javaeye.com/ "浏览作者的博客") [ ](http://cisumer.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=cisumer "发送站内短信") [ ](http://cisumer.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=cisumer "关注作者") [回帖地址](http://www.javaeye.com/topic/722599?page=18#1603762 "cisumer回帖:代码难道不是这么写的？")

[0](http://www.javaeye.com/topic/722599?page=18# "好") [2](http://www.javaeye.com/topic/722599?page=18# "差") 请登录后投票  * beneo
* 等级: 初级会员
* [![beneo的博客]( "beneo的博客: ")](http://beneo.javaeye.com/)
* 文章: 115
* 积分: 50
* 来自: 希伯來
* ![]()
    发表时间：2010-07-30  

mercyblitz 写道

moonranger 写道

qiushily2030 写道

fengfeng925 写道

qiushily2030 写道

A a;
申明在内部，每次遍历都申明一个a的变量.  
申明在外面 只申明一次 一直复用.  申明也是要资源的..
一个变量就相当于一个指针,循环1亿次,LZ你说呢.
评审官说的还是有理的,不过这个得看需求.  JVM回收是在内存不足的时候.
  还是不要想当然了,我就那么干过一回. 程序是最真实的答案...
简直是TMD胡扯。
JVM回收是在内存不足的时候.这个已经修正. 不是内存不足才调用。。
你说我胡扯？ 那for里定义的局部变量放哪里？不要也和上面一个的观点一样：只申明一次,后调用索引.
这个我不可理解。。
方法的参数连同方法里的变量所占用的空间，在编译时就已经确定了，它决定了运行时一个方法被调用的时侯要分配多大的栈帧。一个变量放在循环外还是循环里，只是在编译期限定了其作用域而已，到了运行时是没有区别的（也许只是在栈帧的local variables区里的位置不同）。
看一下方法的字节码，一切都非常明了：
private final int[] numbers = {1, 2, 3, 4, 5}; public void varOutsideLoop() { int n; for (int i = 0; i < numbers.length; i++) { n = numbers[i]; System.out.println(n); } } public void varInsideLoop() { for (int i = 0; i < numbers.length; i++) { int n = numbers[i]; System.out.println(n); } }
对应的字节码：
public void varOutsideLoop(); Signature: ()V Code: Stack=2, Locals=3, Args_size=1 0: iconst_0 1: istore_2 2: goto 22 5: aload_0 6: getfield #12; //Field numbers:[I 9: iload_2 10: iaload 11: istore_1 12: getstatic #19; //Field java/lang/System.out:Ljava/io/PrintStream; 15: iload_1 16: invokevirtual #25; //Method java/io/PrintStream.println:(I)V 19: iinc 2, 1 22: iload_2 23: aload_0 24: getfield #12; //Field numbers:[I 27: arraylength 28: if_icmplt 5 31: return LineNumberTable: line 43: 0 line 44: 5 line 45: 12 line 43: 19 line 47: 31 LocalVariableTable: Start Length Slot Name Signature 0 32 0 this Lcom/jstudio/learnjvm/classloader/VarInLoop; 12 10 1 n I 2 29 2 i I public void varInsideLoop(); Signature: ()V Code: Stack=2, Locals=3, Args_size=1 0: iconst_0 1: istore_1 2: goto 22 5: aload_0 6: getfield #12; //Field numbers:[I 9: iload_1 10: iaload 11: istore_2 12: getstatic #19; //Field java/lang/System.out:Ljava/io/PrintStream; 15: iload_2 16: invokevirtual #25; //Method java/io/PrintStream.println:(I)V 19: iinc 1, 1 22: iload_1 23: aload_0 24: getfield #12; //Field numbers:[I 27: arraylength 28: if_icmplt 5 31: return LineNumberTable: line 50: 0 line 51: 5 line 52: 12 line 50: 19 line 54: 31 LocalVariableTable: Start Length Slot Name Signature 0 32 0 this Lcom/jstudio/learnjvm/classloader/VarInLoop; 2 29 1 i I 12 7 2 n I
仔细看字节码就能发现，除了n和循环变量i在local variable table中的位置不同，其他的所有的都没有区别。
varOutsideLoop那个方法里我故意没有初始化n，如果加上初始化，就会多一条iconst和一条istore指令。
至于位置不一样的原因，个人猜测与变量出现的位置有关，n在循环外的版本，它出现在前，所以占据了第一个slot（第0个slot是对象本身，即this）；n在循环里的版本，循环变量i出现在前，所以i占据了第一个slot，而n占用的是第二个slot。
除此之外，两个方法没有任何的区别。
mercyblitz 写道

主要你误解了a变量在遍历中，有入栈和出栈的操作。并没有重复开辟局部变量，再说JVM优化之后，不会重复创建。
个人认为这种说法也不太对，在一个方法内部不会有所谓的“入栈”和“出栈”操作。方法里，一个变量占用一个local variable table的slot，一个萝卜一个坑。所有这些变量都是在方法结束，堆栈帧被销毁的时侯才真正被“回收”，即使方法结束的时侯它早以超出了自己的作用域……
实践中，变量的作用域当然是尽可能小才好，即有利于垃圾收集、也消除了一些潜在的导致bug的隐患。
楼主别理那个“代码评审官”就好……话说如果我遇到这种人这种事，我八成不愿继续干下去……
恩，谢谢指正，我也仔细想了一下。在楼主的例子中，字节码说明两个局部变量指向不同的地方。不会重复开辟。要想开辟更多，哪么需要不同名称的变量申明。
第一个是frame append
第二个是frame same
所以还是有些许区别的吧 [返回顶楼](http://www.javaeye.com/topic/722599?page=18#) [ ](http://beneo.javaeye.com/ "浏览作者的博客") [ ](http://beneo.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=beneo "发送站内短信") [ ](http://beneo.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=beneo "关注作者") [回帖地址](http://www.javaeye.com/topic/722599?page=18#1604053 "beneo回帖:代码难道不是这么写的？")

[0](http://www.javaeye.com/topic/722599?page=18# "好") [0](http://www.javaeye.com/topic/722599?page=18# "差") 请登录后投票  * RednaxelaFX
* 等级: ![五星会员]( "五星会员")
* [![RednaxelaFX的博客]( "RednaxelaFX的博客: Script Ahead, Code Behind")](http://rednaxelafx.javaeye.com/)
* 文章: 385
* 积分: 790
* 来自: 杭州
* ![]()
    发表时间：2010-07-30   最后修改：2010-07-30

moonranger 写道

mercyblitz 写道

主要你误解了a变量在遍历中，有入栈和出栈的操作。并没有重复开辟局部变量，再说JVM优化之后，不会重复创建。
个人认为这种说法也不太对，在一个方法内部不会有所谓的“入栈”和“出栈”操作。方法里，一个变量占用一个local variable table的slot，一个萝卜一个坑。所有这些变量都是在方法结束，堆栈帧被销毁的时侯才真正被“回收”，即使方法结束的时侯它早以超出了自己的作用域……
[ECMA-335 CLI](http://www.ecma-international.org/publications/standards/Ecma-335.htm)是规定方法中每个局部变量占用局部变量区的一个坑，不在方法中复用局部变量区。但它在设计之初就是倾向于使用JIT编译器来实现执行引擎的；虽然中间代码（CIL）中局部变量不复用局部变量区的坑，但实际JIT编译后的代码不关心这个，局部变量的存储分配（栈/寄存器）还是有复用。可以参考.NET CLR的实现。
（以下说明针对概念中的Java虚拟机）
Java虚拟机在设计之初就同时考虑到要能方便的用解释器或JIT编译器来实现。解释器一般做的优化较少，执行过程与字节码中指定的方式基本是直接对应的，所以字节码上就已经需要考虑到实际执行的情况了。Java虚拟机的局部变量区是可复用的，在同一方法里作用域不交叠的局部变量可以分配在同一个局部变量区的slot上。有兴趣的话可以自己做实验验证，也可以参考[之前我做的分享的演示稿](http://rednaxelafx.javaeye.com/blog/656951)，20100621的版本。
例子的话，可以看这么一段代码：
public class LocalsDemo { // locals = 5 public static void main(String[] args) { // args in slot 0 int a = 1; // a in slot 1 int b = 2; // b in slot 2 { long c = 3; // c in slot 3 (to slot 4) } { int d = 4; // d in slot 3 int e = 5; // e in slot 4 } for (int i = 0; i < 3; i++) { // i in slot 3 int j = i + 1; // j in slot 4 } } }
main()方法编译后对应的字节码和元信息是：
public static void main(java.lang.String[]); Code: Stack=2, Locals=5, Args_size=1 0: iconst_1 1: istore_1 2: iconst_2 3: istore_2 4: ldc2_w #2; //long 3l 7: lstore_3 8: iconst_4 9: istore_3 10: iconst_5 11: istore 4 13: iconst_0 14: istore_3 15: iload_3 16: iconst_3 17: if_icmpge 31 20: iload_3 21: iconst_1 22: iadd 23: istore 4 25: iinc 3, 1 28: goto 15 31: return LineNumberTable: line 4: 0 line 5: 2 line 7: 4 line 10: 8 line 11: 10 line 13: 13 line 14: 20 line 13: 25 line 16: 31 LocalVariableTable: Start Length Slot Name Signature 8 0 3 c J 10 3 3 d I 13 0 4 e I 25 0 4 j I 15 16 3 i I 0 32 0 args [Ljava/lang/String; 2 30 1 a I 4 28 2 b I
要在Class文件里看到LocalVariableTable属性表的话，编译Java源码时请加上-g参数。
Java虚拟机中，
方法调用涉及的栈帧push/pop是对Java栈（Java stack），或者有些文档里叫“Java控制栈”（[Java control stack](http://wikis.sun.com/display/HotSpotInternals/JavaControlStack)），或者叫“Java方法调用栈”；
在方法中局部变量的写/读操作涉及的push/pop则是对操作数栈（operand stack），或者有些文档叫“表达式栈”（[expression stack](http://gbenson.net/?p=122)），或者叫求值栈（evaluation stack）。
两个栈的区别可以参考[Java虚拟机规范第二版3.5.2](http://java.sun.com/docs/books/jvms/second_edition/html/Overview.doc.html#6654)、[3.6.2](http://java.sun.com/docs/books/jvms/second_edition/html/Overview.doc.html#28851)两个小节，我之前的[介绍基于栈/寄存器的虚拟机的帖](http://rednaxelafx.javaeye.com/blog/492667)里也有提到，也可以参考前面说的JVM分享的演示稿。
最后上一老图……逃
![]() [返回顶楼](http://www.javaeye.com/topic/722599?page=18#) [ ](http://rednaxelafx.javaeye.com/ "浏览作者的博客") [ ](http://rednaxelafx.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=RednaxelaFX "发送站内短信") [ ](http://rednaxelafx.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=RednaxelaFX "关注作者") [回帖地址](http://www.javaeye.com/topic/722599?page=18#1604262 "RednaxelaFX回帖:代码难道不是这么写的？")

[7](http://www.javaeye.com/topic/722599?page=18# "好") [0](http://www.javaeye.com/topic/722599?page=18# "差") 请登录后投票  * moonranger
* 等级: 初级会员
* [![moonranger的博客]( "moonranger的博客: moonranger")](http://moonranger.javaeye.com/)
* 文章: 44
* 积分: 40
* 来自: 天津
* ![]()
    发表时间：2010-07-30  

RednaxelaFX 写道

moonranger 写道

mercyblitz 写道

主要你误解了a变量在遍历中，有入栈和出栈的操作。并没有重复开辟局部变量，再说JVM优化之后，不会重复创建。
个人认为这种说法也不太对，在一个方法内部不会有所谓的“入栈”和“出栈”操作。方法里，一个变量占用一个local variable table的slot，一个萝卜一个坑。所有这些变量都是在方法结束，堆栈帧被销毁的时侯才真正被“回收”，即使方法结束的时侯它早以超出了自己的作用域……
[ECMA-335 CLI](http://www.ecma-international.org/publications/standards/Ecma-335.htm)是规定方法中每个局部变量占用局部变量区的一个坑，不在方法中复用局部变量区。但它在设计之初就是倾向于使用JIT编译器来实现执行引擎的；虽然中间代码（CIL）中局部变量不复用局部变量区的坑，但实际JIT编译后的代码不关心这个，局部变量的存储分配（栈/寄存器）还是有复用。可以参考.NET CLR的实现。
（以下说明针对概念中的Java虚拟机）
Java虚拟机在设计之初就同时考虑到要能方便的用解释器或JIT编译器来实现。解释器一般做的优化较少，执行过程与字节码中指定的方式基本是直接对应的，所以字节码上就已经需要考虑到实际执行的情况了。Java虚拟机的局部变量区是可复用的，在同一方法里作用域不交叠的局部变量可以分配在同一个局部变量区的slot上。有兴趣的话可以自己做实验验证，也可以参考[之前我做的分享的演示稿](http://rednaxelafx.javaeye.com/blog/656951)，20100621的版本。
例子的话，可以看这么一段代码：
public class LocalsDemo { // locals = 5 public static void main(String[] args) { // args in slot 0 int a = 1; // a in slot 1 int b = 2; // b in slot 2 { long c = 3; // c in slot 3 (to slot 4) } { int d = 4; // d in slot 3 int e = 5; // e in slot 4 } for (int i = 0; i < 3; i++) { // i in slot 3 int j = i + 1; // j in slot 4 } } }
main()方法编译后对应的字节码和元信息是：
public static void main(java.lang.String[]); Code: Stack=2, Locals=5, Args_size=1 0: iconst_1 1: istore_1 2: iconst_2 3: istore_2 4: ldc2_w #2; //long 3l 7: lstore_3 8: iconst_4 9: istore_3 10: iconst_5 11: istore 4 13: iconst_0 14: istore_3 15: iload_3 16: iconst_3 17: if_icmpge 31 20: iload_3 21: iconst_1 22: iadd 23: istore 4 25: iinc 3, 1 28: goto 15 31: return LineNumberTable: line 4: 0 line 5: 2 line 7: 4 line 10: 8 line 11: 10 line 13: 13 line 14: 20 line 13: 25 line 16: 31 LocalVariableTable: Start Length Slot Name Signature 8 0 3 c J 10 3 3 d I 13 0 4 e I 25 0 4 j I 15 16 3 i I 0 32 0 args [Ljava/lang/String; 2 30 1 a I 4 28 2 b I
要在Class文件里看到LocalVariableTable属性表的话，编译Java源码时请加上-g参数。
Java虚拟机中，
方法调用涉及的栈帧push/pop是对Java栈（Java stack），或者有些文档里叫“Java控制栈”（[Java control stack](http://wikis.sun.com/display/HotSpotInternals/JavaControlStack)），或者叫“Java方法调用栈”；
在方法中局部变量的读/写操作涉及的push/pop则是对操作数栈（operand stack），或者有些文档叫“表达式栈”（[expression stack](http://gbenson.net/?p=122)），或者叫求值栈（evaluation stack）。
两个栈的区别可以参考[Java虚拟机规范第二版3.5.2](http://java.sun.com/docs/books/jvms/second_edition/html/Overview.doc.html#6654)、[3.6.2](http://java.sun.com/docs/books/jvms/second_edition/html/Overview.doc.html#28851)两个小节，我之前的[介绍基于栈/寄存器的虚拟机的帖](http://rednaxelafx.javaeye.com/blog/492667)里也有提到，也可以参考前面说的JVM分享的演示稿。
最后上一老图……逃
![]()
还真没有研究到这种程度。学习了！ [返回顶楼](http://www.javaeye.com/topic/722599?page=18#) [ ](http://moonranger.javaeye.com/ "浏览作者的博客") [ ](http://moonranger.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=moonranger "发送站内短信") [ ](http://moonranger.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=moonranger "关注作者") [回帖地址](http://www.javaeye.com/topic/722599?page=18#1604445 "moonranger回帖:代码难道不是这么写的？")

[2](http://www.javaeye.com/topic/722599?page=18# "好") [0](http://www.javaeye.com/topic/722599?page=18# "差") 请登录后投票  * linliangyi2007
* 等级: ![一钻会员]( "一钻会员")
* [![linliangyi2007的博客]( "linliangyi2007的博客: Java、咖啡与茶")](http://linliangyi2007.javaeye.com/)
* 文章: 803
* 积分: 996
* 来自: 福州
* ![]()
    发表时间：2010-07-30  

高人出现了，哈哈哈！！
哪些只懂得代码文本面上搞所谓性能优化的家伙，可以羞愧的匿了~~~ [返回顶楼](http://www.javaeye.com/topic/722599?page=18#) [ ](http://linliangyi2007.javaeye.com/ "浏览作者的博客") [ ](http://linliangyi2007.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=linliangyi2007 "发送站内短信") [ ](http://linliangyi2007.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=linliangyi2007 "关注作者") [回帖地址](http://www.javaeye.com/topic/722599?page=18#1604541 "linliangyi2007回帖:代码难道不是这么写的？")

[0](http://www.javaeye.com/topic/722599?page=18# "好") [0](http://www.javaeye.com/topic/722599?page=18# "差") 请登录后投票  * murainwood
* 等级: 初级会员
* [![murainwood的博客]( "murainwood的博客: murainwood")](http://murainwood.javaeye.com/)
* 文章: 1077
* 积分: 30
* 来自: ...
* ![]()
    发表时间：2010-07-30  

精彩，秒杀！ [返回顶楼](http://www.javaeye.com/topic/722599?page=18#) [ ](http://murainwood.javaeye.com/ "浏览作者的博客") [ ](http://murainwood.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=murainwood "发送站内短信") [ ](http://murainwood.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=murainwood "关注作者") [回帖地址](http://www.javaeye.com/topic/722599?page=18#1604574 "murainwood回帖:代码难道不是这么写的？")

[0](http://www.javaeye.com/topic/722599?page=18# "好") [0](http://www.javaeye.com/topic/722599?page=18# "差") 请登录后投票  * 分离的北极熊
* 等级: 初级会员
* [![分离的北极熊的博客]( "分离的北极熊的博客: ")](http://clear2012-gmail-com.javaeye.com/)
* 文章: 19
* 积分: 30
* 来自: 北京
* ![]()
    发表时间：2010-07-30  

太狠了……………… [返回顶楼](http://www.javaeye.com/topic/722599?page=18#) [ ](http://clear2012-gmail-com.javaeye.com/ "浏览作者的博客") [ ](http://clear2012-gmail-com.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E5%88%86%E7%A6%BB%E7%9A%84%E5%8C%97%E6%9E%81%E7%86%8A "发送站内短信") [ ](http://clear2012-gmail-com.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E5%88%86%E7%A6%BB%E7%9A%84%E5%8C%97%E6%9E%81%E7%86%8A "关注作者") [回帖地址](http://www.javaeye.com/topic/722599?page=18#1604606 "分离的北极熊回帖:代码难道不是这么写的？")

[0](http://www.javaeye.com/topic/722599?page=18# "好") [0](http://www.javaeye.com/topic/722599?page=18# "差") 请登录后投票  * lijihuai
* 等级: 初级会员
* [![lijihuai的博客]( "lijihuai的博客: lijihuai")](http://lijihuai.javaeye.com/)
* 文章: 8
* 积分: 30
* ![]()
    发表时间：2010-07-30  

没事，实际上，编译以后的字节码是一样的 [返回顶楼](http://www.javaeye.com/topic/722599?page=18#) [ ](http://lijihuai.javaeye.com/ "浏览作者的博客") [ ](http://lijihuai.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=lijihuai "发送站内短信") [ ](http://lijihuai.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=lijihuai "关注作者") [回帖地址](http://www.javaeye.com/topic/722599?page=18#1604696 "lijihuai回帖:代码难道不是这么写的？")

[0](http://www.javaeye.com/topic/722599?page=18# "好") [0](http://www.javaeye.com/topic/722599?page=18# "差") 请登录后投票

[](http://www.javaeye.com/forums/39/topics/722599/posts/new "发表回复")

[« 上一页](http://www.javaeye.com/topic/722599?page=17) [1](http://www.javaeye.com/topic/722599?page=1) [2](http://www.javaeye.com/topic/722599?page=2) … [17](http://www.javaeye.com/topic/722599?page=17) 18 [19](http://www.javaeye.com/topic/722599?page=19) [20](http://www.javaeye.com/topic/722599?page=20) [下一页 »](http://www.javaeye.com/topic/722599?page=19)
[论坛首页](http://www.javaeye.com/forums) → [Java编程和Java企业应用版](http://www.javaeye.com/forums/board/Java) → [Java综合](http://www.javaeye.com/forums/tag/Java)
跳转论坛:Java编程和Java企业应用 Web前端技术 移动编程和手机应用开发 C/C++编程 Ruby编程 Python编程 PHP编程 Flash编程和RIA Microsoft .Net 综合技术 软件开发和项目管理 行业应用 入门讨论 招聘求职 海阔天空

* [广东: 穆迪moody's诚聘Java Developer](http://www.javaeye.com/jobs/794 "广东:穆迪moody's诚聘Java Developer")
* [北京: 掌中魔力诚聘java资深工程师](http://www.javaeye.com/jobs/769 "北京:掌中魔力诚聘java资深工程师")
* [北京: JavaEye猎头诚聘Java搜索工程师](http://www.javaeye.com/jobs/712 "北京:JavaEye猎头诚聘Java搜索工程师")
* [浙江: 网易（NETEASE）诚聘java开发工程师](http://www.javaeye.com/jobs/823 "浙江:网易（NETEASE）诚聘java开发工程师")
* [上海: BShare诚聘资深Java Web开发工程师](http://www.javaeye.com/jobs/838 "上海:BShare诚聘资深Java Web开发工程师")
* [北京: glodon诚聘Eclipse插件开发工程师](http://www.javaeye.com/jobs/839 "北京:glodon诚聘Eclipse插件开发工程师")
* [北京: Laszlo Systems 诚聘高级Java程序员](http://www.javaeye.com/jobs/817 "北京:Laszlo Systems 诚聘高级Java程序员")
* [北京: 去哪儿旅游搜索引擎诚聘java开发工程师](http://www.javaeye.com/jobs/523 "北京:去哪儿旅游搜索引擎诚聘java开发工程师")
* [上海: 上海汉得诚聘高级基础架构研发工程师——IDE](http://www.javaeye.com/jobs/825 "上海:上海汉得诚聘高级基础架构研发工程师——IDE方向")
* [北京: chinacache诚聘高级Java开发工程师（后台应用](http://www.javaeye.com/jobs/726 "北京:chinacache诚聘高级Java开发工程师（后台应用）")
* [北京: chinacache诚聘Java应用架构师](http://www.javaeye.com/jobs/728 "北京:chinacache诚聘Java应用架构师")
* [北京: 网易（NETEASE）诚聘高级Java开发工程师](http://www.javaeye.com/jobs/800 "北京:网易（NETEASE）诚聘高级Java开发工程师")
* [福建: 福州几维网络诚聘游戏服务端程序员(Java)](http://www.javaeye.com/jobs/804 "福建:福州几维网络诚聘游戏服务端程序员(Java)")
* [浙江: VMKID诚聘Java开发工程师(J2SE)（Client端方](http://www.javaeye.com/jobs/779 "浙江:VMKID诚聘Java开发工程师(J2SE)（Client端方向）")
* [上海: 恺英网络诚聘JS工程师](http://www.javaeye.com/jobs/682 "上海:恺英网络诚聘JS工程师")
* [北京: 上海幽幽诚聘手机网游服务器端开发工程师](http://www.javaeye.com/jobs/806 "北京:上海幽幽诚聘手机网游服务器端开发工程师")

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
