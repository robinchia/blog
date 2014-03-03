---
layout: post
title: "java enum"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_常用
--- 

# java enum

Java代码  [![收藏代码]()]( "收藏这段代码")

1. public enum Operation {  
1.   
1.    PLUS    { double eval(double x, double y) { return x + y; } },  
1.   
1.    MINUS   { double eval(double x, double y) { return x - y; } },  
1.   
1.    TIMES   { double eval(double x, double y) { return x * y; } },  
1.   
1.    DIVIDE { double eval(double x, double y) { return x / y; } };  
1.   
1.   
1.    // Do arithmetic op represented by this constant  
1.   
1.    abstract double eval(double x, double y);  
1.   
1.    public static void main(String args[]) {  
1.   
1.          double x = Double.parseDouble("3");  
1.   
1.          double y = Double.parseDouble("2");  
1.   
1.          for (Operation op : Operation.values())  
1.   
1.              System.out.printf("%f %s %f = %f%n", x, op, y, op.eval(x, y));  
1.   
1.          System.out.println(Operation.valueOf("PLUS").eval(x, y));  
1.      }  
1.   
1. }  
1. public enum RandomStr {  
1.     /** 
1.      * 只有数字 
1.      */  
1.     Number("0123456789"),  
1.   
1.     /** 
1.      * 包含数字和大小写字符 
1.      */  
1.     NumberAndChar("abcdefghjkmnpqrstuvwxyz23456789ABCDEFGHJKLMNPQRSTUVWXYZ"),  
1.   
1.     /** 
1.      * 包含数字和大写字符 
1.      */  
1.     NumberAndCharIgnoreCase("ABCDEFGHJKLMNPQRSTUVWXYZ23456789"),  
1.   
1.     /** 
1.      * 数字和字符平均出现 
1.      */  
1.     AvgNumberAndCharIgnoreCase(  
1.             "ABCDEFGHJKLMNPQRSTUVWXYZ234567892345678923456789");  
1.   
1.     /** 
1.      * 原始值 
1.      */  
1.     private String original;  
1.   
1.     /** 
1.      * 产生指定长度的随机码。 
1.      */  
1.     public String rand(int length) {  
1.         String codes = "";  
1.         if (length > 0) {  
1.             int max = original.length();  
1.             long seed = System.currentTimeMillis();  
1.             Random random = new Random(seed);  
1.             for (int i = 0; i < length; i++) {  
1.                 codes += original.charAt(random.nextInt(max));  
1.             }  
1.         }  
1.         return codes;  
1.     }  
1.   
1.     /** 
1.      * 私有构造器。 
1.      */  
1.     private RandomStr(String original) {  
1.         this.original = original;  
1.     }  
1.   
1. }  
注意：JDK4.0以前的都不支持，转成5.0以上才支持 
jdk5.0发布以后，添加了枚举类型，其实当初在从Delphi转向Java的时候，我就在为java中没有枚举这个功能感到不可思议。因为枚举类型在很多方面有着独特作用，现在好了，java中添加了这项功能，今天我就试了试，还满好的。 
                                                                                    
java中的枚举类型包括了其他语言中枚举类型的一般特性。 
class EnumDemo{ 
   public enum Seasons { 
    winter,spring,summer,fall; 
    } 
   
   
    public static void main(String[] args){ 
     
       for(Seasons s:Seasons.values()) 
        System.out.println(s); 
      
        } 
} 
运行结果： 
winter 
spring 
summer 
fall 
上面这个例子，展示了枚举类型的一般用法，在java的枚举类中提供了静态values()方法以供循环迭代时使用。大家再看一看下面这个例子： 
public enum Seasons { 
winter, 
spring, 
summer, 
fall; 
//list the values 
public static void main(String[] args) { 
for(Seasons s:Seasons.values()) 
   { 
      System.out.println(s); 
   } 
} 
} 
这两个例子得出的是一样的结果。由此可知enum关键字是代表一个类相当于class的意思，但是它又比class的范围要小，仅仅代表枚举类而已。 
java中的枚举类除了有这些一般的功能外还包括一些特殊的功能，例如：枚举类型可以有构造函数、可以添加任意多的方法和属性；同时枚举类型还可以为不同的属性添加不同的方法。 
在这里我们假设你希望向一个枚举类中添加数据和行为。例如我们可以设想一下银河系的星球。每个星球的它自己的特定数据，由此来计算物体在其表面上的重量。下面就是实例： 
public enum Planet { 
     MERCURY (3.303e+23, 2.4397e6), //水星 
     VENUS    (4.869e+24, 6.0518e6), //金星 
     EARTH    (5.976e+24, 6.37814e6), //地球 
     MARS     (6.421e+23, 3.3972e6), //火星 
     JUPITER (1.9e+27,    7.1492e7), //木星 
     SATURN   (5.688e+26, 6.0268e7), //土星 
     URANUS   (8.686e+25, 2.5559e7), //天王星 
     NEPTUNE (1.024e+26, 2.4746e7),  //海王星 
     PLUTO    (1.27e+22,   1.137e6); //冥王星 
     private final double mass;    // in kilograms 
     private final double radius; // in meters 
     Planet(double mass, double radius) {   //枚举类不需要被实例化，所以构造器是私有的private,不加默认为私有类型 
         this.mass = mass; 
         this.radius = radius; 
     } 
     public double mass()    { return mass; } 
     public double radius() { return radius; } 
     // universal gravitational constant   (m 3 kg -1 s-2) 
     public static final double G = 6.67300E-11; 
     public double surfaceGravity() { 
         return G * mass / (radius * radius); 
     } 
     public double surfaceWeight(double otherMass) { 
         return otherMass * surfaceGravity(); 
     } 
     public static void main(String[] args) { 
         double earthWeight = Double.parseDouble(args[0]); 
         double mass = earthWeight/EARTH.surfaceGravity(); 
         for (Planet p : Planet.values()) 
            System.out.printf("Your weight on %s is %f%n", 
                              p, p.surfaceWeight(mass)); 
     } 
} 
运行结果： 
C:\java>java Planet 60 
Your weight on MERCURY is 22.665457 
Your weight on VENUS is 54.299946 
Your weight on EARTH is 60.000000 
Your weight on MARS is 22.724231 
Your weight on JUPITER is 151.833452 
Your weight on SATURN is 63.960932 
Your weight on URANUS is 54.307632 
Your weight on NEPTUNE is 68.299684 
Your weight on PLUTO is 4.012468 
在这里我们可以看到这个枚举类中含有一个带有两个参数的构造函数。通过构造函数我们可以产生含有不同数据特征的星球对象。Planet 的构造函数参数值从枚举常量里获取，如： 
当遍历到水星时 MERCURY (3.303e+23, 2.4397e6), //水星 
就会把里面的值3.303e+23传给mass, 而2.4397e6传给radius 
Planet(double mass, double radius) { 
         this.mass = mass; 
         this.radius = radius; 
     } 
在main()函数中，我们通过有不同的星球调用相同的方法来得到物体在该星球上的重量。 
我们可以把为枚举常量添加行为的主意更向前推进一步。我们可以为不同枚举常量添加不同的行为。通过使用switch语句是达到这个目的的一种方法。下面就有一个实例： 
public enum Operation { 
     PLUS, MINUS, TIMES, DIVIDE; 
     // Do arithmetic op represented by this constant 
     double eval(double x, double y){ 
         switch(this) { 
             case PLUS:    return x + y; 
             case MINUS:   return x - y; 
             case TIMES:   return x * y; 
             case DIVIDE: return x / y; 
         } 
         throw new AssertionError("Unknown op: " + this); 
     } 
} 
它工作的非常好，当时如果没有throw语句的话，它将不能通过编译，因此它就显得不是那么完美了。更加糟糕的是，你一定要记住在你向枚举类中添加枚举变量时，你要为这个变量添加操作。如果你忘了的话，eval方法将会操作失败。 
这里有另外一种给枚举常量添加行为的方法。使用这种方法你可以避免上面说提到的问题。你可以在枚举类型中添加一个abstract方法，然后在每一个枚举常量中重载它。这就是有名的constant-specific方法。下面就是用这种技术对以前实例的重写： 
public enum Operation { 
   PLUS    { double eval(double x, double y) { return x + y; } }, 
   MINUS   { double eval(double x, double y) { return x - y; } }, 
   TIMES   { double eval(double x, double y) { return x * y; } }, 
   DIVIDE { double eval(double x, double y) { return x / y; } }; 
   // Do arithmetic op represented by this constant 
   abstract double eval(double x, double y); 
   public static void main(String args[]) { 
         double x = Double.parseDouble(args[0]); 
         double y = Double.parseDouble(args[1]); 
         for (Operation op : Operation.values()) 
             System.out.printf("%f %s %f = %f%n", x, op, y, op.eval(x, y)); 
     } 
} 
运行结果： 
C:\java>java Operation 24 56 
24.000000 PLUS 56.000000 = 80.000000 
24.000000 MINUS 56.000000 = -32.000000 
24.000000 TIMES 56.000000 = 1344.000000 
24.000000 DIVIDE 56.000000 = 0.428571 
大家可能会不太明白“PLUS    { double eval(double x, double y) { return x + y; } }”的意思。其实如果大家理解内部类的话，可能就不难理解这句话的含义了。我的理解是: 
class MyenumOperation implements enumOperation 
{ 
     double eval(double x, double y) { return x + y; } 
} 
MyenumOperation plus = new MyenumOperation(); 
与枚举类型一起添加进来的还有enumset和enummap，我会在下一篇文章中介绍。 
Enum是enumeration(列举)的简写形式,包含在java.lang包中.熟悉C, C++, C 
#, 或 Pascal人应该对列举有所了解,先看个例子: 
public enum Season { winter, spring, summer, fall } 
一个enum是定义一组值的对象,它可以包括零个或多个值成员.它是属于enum类型的,一个enum对象中不可有两个或多个相同的属性或值.在次之前的java程序员一般是 用接口的方法实现列举的,如 : 
public interface Season { 
    static winter = 0; 
    static spring = 1; //etc.. 
} 
引入了enum的java的列举的编写方便了许多,只须定义一个enum型的对象.enum对象的值都回自动获得一个数字值,从0开始,依次递增.看一个比较简单的enum实现的例子: 
EnumDemo.java 
package net.javagarage.enums; 
/* 
We can loop over the values we put into the enum 
using the values() method. 
Note that the enum Seasons is compiled into a 
separate unit, called EnumDemo$Seasons.class 
*/ 
public class EnumDemo { 
       /*declare the enum and add values to it. note that, like in C#, we don't use a ; to 
end this statement and we use commas to separate the values */ 
       private enum Seasons { winter, spring, 
        summer, fall } 
       //list the values 
       public static void main(String[] args) { 
             for (Seasons s : Seasons.values()){ 
                   System.out.println(s); 
             } 
       } 
} 
运行上述代码你回得到 以下结果: 
winter 
spring 
summer 
fall 
Enum的属性调用: 
下面的代码展示了调用enum对象的方法,这也是它通常的用法: 
package net.javagarage.enums; 
/* 
File: EnumSwitch.java 
Purpose: show how to switch against the values in an enum. 
*/ 
public class EnumSwitch { 
       private enum Color { red, blue, green } 
       //list the values 
       public static void main(String[] args) { 
             //refer to the qualified value 
             doIt(Color.red); 
       } 
       /*note that you switch against the UNQUALIFIED name. that is, "case Color.red:" is a 
compiler error */ 
       private static void doIt(Color c){ 
       switch (c) { 
       case red: 
             System.out.println("value is " + Color.red); 
             break; 
       case green: 
             System.out.println("value is " + Color.green); 
             break; 
       case blue: 
             System.out.println("value is : " + Color.blue); 
             break; 
       default : 
             System.out.println("default"); 
       } 
       } 
} 
为enums添加属性和方法 
enums也可以象一般的类一样添加方法和属性,你可以为它添加静态和非静态的属性或方法,这一切都象你在一般的类中做的那样. 
package net.javagarage.enums; 
/* 
File: EnumDemo.java 
Purpose: show how to use an enum that also defines its own fields and methods 
*/ 
public class EnumWithMethods { 
//declare the enum and add values to it. 
public enum Season { 
       winter, spring, summer, fall; 
       private final static String location = "Phoenix"; 
       public static Season getBest(){ 
             if (location.equals("Phoenix")) 
                   return winter; 
             else 
                   return summer; 
       } 
       public static void main(String[] args) { 
       System.out.println(Season.getBest()); 
       } 
} 
就是这么的简单.但是有一点是需要注意的,那就是enums的值列表必须紧跟在enum声明,不然编译时将会出错. 
Enums构造函数: 
和类一样enums也可以有自己的构造函数,如下: 
package net.javagarage.enums; 
public class EnumConstructor { 
       public static void main(String[] a) { 
             //call our enum using the values method 
             for (Temp t : Temp.values()) 
                   System.out.println(t + " is : " + t.getValue()); 
       } 
       //make the enum 
       public enum Temp { 
             absoluteZero(-459), freezing(32), 
             boiling(212), paperBurns(451); 
       //constructor here 
       Temp(int value) { 
             this.value = value; 
       } 
       //regular field?but make it final, 
       //since that is the point, to make constants 
       private final int value; 
       //regular get method 
       public int getValue() { 
       return value; 
       } 
       } 
} 
输出结果是: 
absoluteZero is : -459 
freezing is : 32 
boiling is : 212 
paperBurns is : 451 
尽管enums有这么多的属性,但并不是用的越多越好,如果那样还不如直接用类来的直接.enums的优势在定义int最终变量仅当这些值有一定特殊含义时.但是如果你需要的是一个类,就定义一个类,而不是enum.

来源： <[http://xiewenbo.iteye.com/blog/1313676](http://xiewenbo.iteye.com/blog/1313676)>
 
