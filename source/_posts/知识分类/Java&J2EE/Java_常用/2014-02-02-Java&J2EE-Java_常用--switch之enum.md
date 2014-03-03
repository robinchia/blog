---
layout: post
title: "switch之enum"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_常用
--- 

# switch之enum

### [switch之enum](http://blog.csdn.net/wzju64676266/article/details/6140871)

2011-01-14 23:46 
[spring](http://blog.csdn.net/tag/details.html?tag=spring)[table](http://blog.csdn.net/tag/details.html?tag=table)[class](http://blog.csdn.net/tag/details.html?tag=class)[string](http://blog.csdn.net/tag/details.html?tag=string)[编译器](http://blog.csdn.net/tag/details.html?tag=%e7%bc%96%e8%af%91%e5%99%a8)[byte](http://blog.csdn.net/tag/details.html?tag=byte)

记得曾经去一家公司面试，那时啥也不懂，面试我的那个人好像呆过IBM，数据结构、编译原理这些都很NB。

 

问答环节

他：java switch中能支持什么类型？

 

我：byte short char int ，jdk1.5出来了enum，同样也支持enum

 

他：为什么能支持byte short char int   而long不行？

 

我：这个可能是设计问题

 

他：其实jvm执行class文件的时候，byte short char int这些都是当int类型来执行的，long不能直接转换成int，编译阶段就通不过了。

 

我：我那个时候不太理解他说的那个意思，只能点点头

 

 

他：好，那接着讨论switch为什么支持enum，刚才也讨论过switch其实都是int类型，也只支持int，那enum不是int类型，是个对象，那为什么支持呢！

 

我：那个时候我就蒙了（心里想着，你这家伙，就胡扯），但我讲不出理由，就直接说不知道

 

 

他：其实在switch中enum也是int类型

 

我：心想----我不知道你说的是对还是错，你怎么说都行

自从那以后，哥去研究虚拟机，java指令

 

 **好，废话不多说了，现在来看一下代码，代码比较简单！**

 

**[java]** [view plain](http://blog.csdn.net/wzju64676266/article/details/6140871# "view plain")[copy](http://blog.csdn.net/wzju64676266/article/details/6140871# "copy")

1. /************************************* 
1.  ************************************* 
1.                 源代码 
1.  ************************************* 
1.  *************************************/  
1. public static void testSwitchInt() {  
1.     int intElement = 3;  
1.     switch (intElement) {  
1.     case 3:  
1.         System.out.println("3");  
1.         break;  
1.     default:  
1.         System.out.println("int DEFAULT");  
1.         break;  
1.     }  
1. }  
1. //int 类型反编译跟源代码是一样的  
1.   
1. /************************************* 
1.  ************************************* 
1.         用javap工具看class指令 
1.  ************************************* 
1.  *************************************/  
1. //case 的值本来就是int，这没什么好说的  
1. public static void testSwitchInt();  
1.   Code:  
1.    Stack=2, Locals=1, Args_size=0  
1.    0:   iconst_3      //解释：加载int常量3  
1.    1:   istore_0     //解释：保存int类型到局部变量表index为0的位置（其实保存的就是3）  
1.    2:   iload_0      //加载局部变量表index为0的位置的int变量，用于switch里面  
1.    3:   lookupswitch{ //1  
1.                 3: 20;  
1.                 default: 31 }  
1.    20:  getstatic       #9; //Field java/lang/System.out:Ljava/io/PrintStream;  
1.    23:  ldc     #13; //String 3  
1.    25:  invokevirtual   #11; //Method java/io/PrintStream.println:(Ljava/lang/St  
1. ring;)V  
1.    28:  goto    39  
1.    31:  getstatic       #9; //Field java/lang/System.out:Ljava/io/PrintStream;  
1.    34:  ldc     #14; //String int DEFAULT  
1.    36:  invokevirtual   #11; //Method java/io/PrintStream.println:(Ljava/lang/St  
1. ring;)V  
1.    39:  return  
1.    
1. --------------------------------------------------------------------------------------------------------------------  
1. --------------------------------------------------------------------------------------------------------------------  
1.   
1.   
1. /************************************* 
1.  ************************************* 
1.                 源代码 
1.  ************************************* 
1.  *************************************/  
1. public static void testSwitchChar() {  
1.     int charElement = 'a';   //ascii对应的是97，编译器直接把这个值编译成97，case里面也是这样的  
1.     switch (charElement) {  
1.     case 'a':  
1.         System.out.println("a");  
1.         break;  
1.     default:  
1.         System.out.println("char DEFAULT");  
1.         break;  
1.     }  
1. }  
1. /************************************* 
1.  ************************************* 
1.                 编译后的代码 
1.  ************************************* 
1.  *************************************/  
1. public static void testSwitchChar()  
1. {  
1.     int charElement = 97;     
1.     switch(charElement)  
1.     {  
1.     case 97: // 'a'  
1.         System.out.println("a");  
1.         break;  
1.     default:  
1.         System.out.println("char DEFAULT");  
1.         break;  
1.     }  
1. }  
1. /************************************* 
1.  ************************************* 
1.         用javap工具看class指令 
1.  ************************************* 
1.  *************************************/  
1. //case 的值本来就是char类型，但被编译器处理成int   
1. public static void testSwitchChar();  
1.   Code:  
1.    Stack=2, Locals=1, Args_size=0  
1.    0:   bipush  97   //解释：加载int常量97,a的ascii码  
1.    2:   istore_0    //接下来和上面都一样的  
1.    3:   iload_0      
1.    4:   tableswitch{ //97 to 97  
1.                 97: 24;  
1.                 default: 35 }  
1.    24:  getstatic       #46; //Field java/lang/System.out:Ljava/io/PrintStream;  
1.    27:  ldc     #68; //String a  
1.    29:  invokevirtual   #53; //Method java/io/PrintStream.println:(Ljava/lang/St  
1. ring;)V  
1.    32:  goto    43  
1.    35:  getstatic       #46; //Field java/lang/System.out:Ljava/io/PrintStream;  
1.    38:  ldc     #70; //String char DEFAULT  
1.    40:  invokevirtual   #53; //Method java/io/PrintStream.println:(Ljava/lang/St  
1. ring;)V  
1.    43:  return  
1. byte  short  也是同理，都会编译成int  
1.   
1. --------------------------------------------------------------------------------------------------------------------  
1. --------------------------------------------------------------------------------------------------------------------  
1. *************************上面的例子都比较好理解，enum大家可能也会有点疑惑*********************************  
1.   
1. /************************************* 
1.  ************************************* 
1.                 源代码 
1.  ************************************* 
1.  *************************************/  
1. enum EnumTest {  
1.     WINTER, SUMMER, SPRING, AUTUMN;  
1. }  
1. public static void testSwitchEnum() {  
1.     EnumTest enumElement = EnumTest.AUTUMN;  
1.     switch (enumElement) {  
1.     case AUTUMN:  
1.         System.out.println("AUTUMN");  
1.         break;  
1.     default:  
1.         System.out.println("enum DEFAULT");  
1.         break;  
1.     }  
1. }  
1. /************************************* 
1.  ************************************* 
1.             enum类编译后的代码 
1.  ************************************* 
1.  *************************************/  
1.  //enum其实也就是个普通的类，继承Enum  
1. public final class EnumTest extends Enum  
1. {  
1.     private EnumTest(String s, int i)  
1.     {  
1.         super(s, i);      
1.           
1.         /*调用父类的构造函数 
1.         protected Enum(String name, int ordinal) { 
1.         this.name = name;    //名称 
1.         this.ordinal = ordinal;   元素位置 
1.         } 
1.         */  
1.     }  
1.     public static EnumTest[] values()  
1.     {  
1.         EnumTest aenumtest[];  
1.         int i;  
1.         EnumTest aenumtest1[];  
1.         System.arraycopy(aenumtest = ENUM$VALUES, 0, aenumtest1 = new EnumTest[i = aenumtest.length], 0, i);  
1.         return aenumtest1;  
1.     }  
1.     public static EnumTest valueOf(String s)  
1.     {  
1.         return (EnumTest)Enum.valueOf(meiju/EnumTest, s);  
1.     }  
1.     public static final EnumTest WINTER;  
1.     public static final EnumTest SUMMER;  
1.     public static final EnumTest SPRING;  
1.     public static final EnumTest AUTUMN;  
1.     private static final EnumTest ENUM$VALUES[];  
1.     static   
1.     {  
1.         //enum的位置的排好的，想数组一样，enum元素最终都保存在ENUM$VALUES数组  
1.         WINTER = new EnumTest("WINTER", 0);    
1.         SUMMER = new EnumTest("SUMMER", 1);  
1.         SPRING = new EnumTest("SPRING", 2);  
1.         AUTUMN = new EnumTest("AUTUMN", 3);  
1.         ENUM$VALUES = (new EnumTest[] {  
1.             WINTER, SUMMER, SPRING, AUTUMN  
1.         });  
1.     }  
1. }  
1. /************************************* 
1.  ************************************* 
1.     testSwitchEnum方法编译后的代码 
1.  ************************************* 
1.  *************************************/  
1.  用到enum元素，所以会在当前类中多生成一个$SWITCH_TABLE$meiju$EnumTest()方法和$SWITCH_TABLE$meiju$EnumTest[]变量，用于switch  
1.  static int[] $SWITCH_TABLE$meiju$EnumTest()  
1. {  
1.     $SWITCH_TABLE$meiju$EnumTest;  
1.     if($SWITCH_TABLE$meiju$EnumTest == null) goto _L2; else goto _L1  
1. _L1:  
1.     return;  
1. _L2:  
1.     JVM INSTR pop ;  
1.     int ai[] = new int[EnumTest.values().length];  
1.     try  
1.     {  
1.         ai[EnumTest.AUTUMN.ordinal()] = 4;  
1.     }  
1.     catch(NoSuchFieldError _ex) { }  
1.     try  
1.     {  
1.         ai[EnumTest.SPRING.ordinal()] = 3;  
1.     }  
1.     catch(NoSuchFieldError _ex) { }  
1.     try  
1.     {  
1.         ai[EnumTest.SUMMER.ordinal()] = 2;  
1.     }  
1.     catch(NoSuchFieldError _ex) { }  
1.     try  
1.     {  
1.         ai[EnumTest.WINTER.ordinal()] = 1;  
1.     }  
1.     catch(NoSuchFieldError _ex) { }  
1.     return $SWITCH_TABLE$meiju$EnumTest = ai;  
1. }  
1. private static int $SWITCH_TABLE$meiju$EnumTest[];//保存的是enum的index  
1.   
1.  public static void testSwitchEnum()  
1. {  
1.     EnumTest enumElement = EnumTest.AUTUMN;  
1.     //这个就是上面所用到的变量  
1.     switch($SWITCH_TABLE$meiju$EnumTest()[enumElement.ordinal()])  
1.     {  
1.     case 4: // '/004'     因为enum类的元素其实就是个常量，在编译阶段就能确定值，在源代码的case AUTUMN:   其实也就被他所在的ordinal()给替换掉了，其实就是索引  
1.         System.out.println("AUTUMN");  
1.         break;  
1.     default:  
1.         System.out.println("enum DEFAULT");  
1.         break;  
1.     }  
1. }  
1.   
1. /************************************* 
1.  ************************************* 
1.         用javap工具看class指令 
1.  ************************************* 
1.  *************************************/  
1. public static void testSwitchEnum();  
1.   Code:  
1.    Stack=2, Locals=1, Args_size=0  
1.    0:   getstatic       #6; //Field meiju/EnumTest.AUTUMN:Lmeiju/EnumTest;  
1.    3:   astore_0  
1.    4:   getstatic       #7; //Field meiju/SwitchEnum$1.$SwitchMap$meiju$EnumTest  
1. :[I  
1.    7:   aload_0  
1.    8:   invokevirtual   #8; //Method meiju/EnumTest.ordinal:()I  
1.    11:  iaload  
1.    12:  lookupswitch{ //1  
1.                 1: 32;  
1.                 default: 43 }  
1.    32:  getstatic       #9; //Field java/lang/System.out:Ljava/io/PrintStream;  
1.    35:  ldc     #10; //String AUTUMN  
1.    37:  invokevirtual   #11; //Method java/io/PrintStream.println:(Ljava/lang/St  
1. ring;)V  
1.    40:  goto    51  
1.    43:  getstatic       #9; //Field java/lang/System.out:Ljava/io/PrintStream;  
1.    46:  ldc     #12; //String enum DEFAULT  
1.    48:  invokevirtual   #11; //Method java/io/PrintStream.println:(Ljava/lang/St  
1. ring;)V  
1.    51:  return  
  

事实证明当时他不是忽悠我，确实是这样的:)

来源： <[http://blog.csdn.net/wzju64676266/article/details/6140871](http://blog.csdn.net/wzju64676266/article/details/6140871)> 
