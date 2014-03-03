---
layout: post
title: "Java知识点总结1"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# Java知识点总结1

**Java****知识点总结****1**

前言：现就java学习中的部分知识点做一下总结，以供同学们参考
1 String,StringBuffer和StringBuilder的用法
String 字符串常量
StringBuffer 字符串变量（线程安全）
StringBuilder 字符串变量（非线程安全）
 简要的说， String 类型和 StringBuffer 类型的主要性能区别其实在于 String 是不可变的对象, 因此在每次对 String 类型进行改变的时候其实都等同于生成了一个新的 String 对象，然后将指针指向新的 String 对象，所以经常改变内容的字符串最好不要用 String ，因为每次生成对象都会对系统性能产生影响，特别当内存中无引用对象多了以后， JVM 的 GC 就会开始工作，那速度是一定会相当慢的。
 而如果是使用 StringBuffer 类则结果就不一样了，每次结果都会对 StringBuffer 对象本身进行操作，而不是生成新的对象，再改变对象引用。所以在一般情况下我们推荐使用 StringBuffer ，特别是字符串对象经常改变的情况下。而在某些特别情况下， String 对象的字符串拼接其实是被 JVM 解释成了 StringBuffer 对象的拼接，所以这些时候 String 对象的速度并不会比 StringBuffer 对象慢，而特别是以下的字符串对象生成中， String 效率是远要比 StringBuffer 快的：
 String S1 = “This is only a” + “ simple” + “ test”;
 StringBuffer Sb = new StringBuilder(“This is only a”).append(“ simple”).append(“ test”);
 我们会很惊讶的发现，生成 String S1 对象的速度简直太快了，而这个时候 StringBuffer 居然速度上根本一点都不占优势。其实这是 JVM 的一个把戏，在 JVM 眼里，这个
 String S1 = “This is only a” + “ simple” + “test”; 其实就是：
 String S1 = “This is only a simple test”; 所以当然不需要太多的时间了。但大家这里要注意的是，如果你的字符串是来自另外的 String 对象的话，速度就没那么快了，譬如：
String S2 = “This is only a”;
String S3 = “ simple”;
String S4 = “ test”;
String S1 = S2 +S3 + S4;
这时候 JVM 会规规矩矩的按照原来的方式去做

在大部分情况下 StringBuffer > String
StringBuffer
Java.lang.StringBuffer线程安全的可变字符序列。一个类似于 String 的字符串缓冲区，但不能修改。虽然在任意时间点上它都包含某种特定的字符序列，但通过某些方法调用可以改变该序列的长度和内容。
可将字符串缓冲区安全地用于多个线程。可以在必要时对这些方法进行同步，因此任意特定实例上的所有操作就好像是以串行顺序发生的，该顺序与所涉及的每个线程进行的方法调用顺序一致。
StringBuffer 上的主要操作是 append 和 insert 方法，可重载这些方法，以接受任意类型的数据。每个方法都能有效地将给定的数据转换成字符串，然后将该字符串的字符追加或插入到字符串缓冲区中。append 方法始终将这些字符添加到缓冲区的末端；而 insert 方法则在指定的点添加字符。
例如，如果 z 引用一个当前内容是“start”的字符串缓冲区对象，则此方法调用 z.append(le) 会使字符串缓冲区包含“startle”，而 z.insert(4, le) 将更改字符串缓冲区，使之包含“starlet”。
在大部分情况下 StringBuilder > StringBuffer
java.lang.StringBuilde
java.lang.StringBuilder一个可变的字符序列是5.0新增的。此类提供一个与 StringBuffer 兼容的 API，但不保证同步。该类被设计用作 StringBuffer 的一个简易替换，用在字符串缓冲区被单个线程使用的时候（这种情况很普遍）。如果可能，建议优先采用该类，因为在大多数实现中，它比 StringBuffer 要快。两者的方法基本相同。
2 Vector 还是ArrayList――哪一个更好，为什么？
要回答这个问题不能一概而论，有时候使用Vector比较好；有时是ArrayList，有时候这两个都不是最好的选择。你别指望能够获得一个简单肯定答案，因为这要看你用它们干什么。下面有4个要考虑的因素：
l API
l 同步处理
l 数据增长性
l 使用模式
下面针对这4个方面进行一一探讨
API
在由Ken&nbspArnold等编著的《Java&nbspProgramming&nbspLanguage》 (Addison-Wesley,&nbspJune&nbsp2000)一书中有这样的描述，Vector类似于 ArrayList.。所有从API的角度来看这两个类非常相[b]似。但他们之间也还是有一些主要的区别的。

同步性
Vector是同步的。这个类中的一些方法保证了Vector中的对象是线程安全的。而ArrayList则是异步的，因此ArrayList中的对象并不是线程安全的。因为同步的要求会影响执行的效率，所以如果你不需要线程安全的集合那么使用ArrayList是一个很好的选择，这样可以避免由于同步带来的不必要的性能开销。
数据增长
从内部实现机制来讲ArrayList和Vector都是使用数组(Array)来控制集合中的对象。当你向这两种类型中增加元素的时候，如果元素的数目超出了内部数组目前的长度它们都需要扩展内部数组的长度，Vector缺省情况下自动增长原来一倍的数组长度，ArrayList是原来的50%,所以最后你获得的这个集合所占的空间总是比你实际需要的要大。所以如果你要在集合中保存大量的数据那么使用Vector有一些优势，因为你可以通过设置集合的初始化大小来避免不必要的资源开销。
使用模式
在ArrayList和Vector中，从一个指定的位置（通过索引）查找数据或是在集合的末尾增加、移除一个元素所花费的时间是一样的，这个时间我们用 O(1)表示。但是，如果在集合的其他位置增加或移除元素那么花费的时间会呈线形增长：O(n-i)，其中n代表集合中元素的个数，i代表元素增加或移除元素的索引位置。为什么会这样呢？以为在进行上述操作的时候集合中第i和第i个元素之后的所有元素都要执行位移的操作。这一切意味着什么呢？
这意味着，你只是查找特定位置的元素或只在集合的末端增加、移除元素，那么使用Vector或ArrayList都可以。如果是其他操作，你最好选择其他的集合操作类。比如，LinkList集合类在增加或移除集合中任何位置的元素所花费的时间都是一样的?O(1)，但它在索引一个元素的使用缺比较慢－O (i),其中i是索引的位置.使用ArrayList也很容易，因为你可以简单的使用索引来代替创建iterator对象的操作。LinkList也会为每个插入的元素创建对象，所有你要明白它也会带来额外的开销。
最后，在《Practical&nbspJava》一书中Peter&nbspHaggar建议使用一个简单的数组（Array）来代替 Vector或ArrayList。尤其是对于执行效率要求高的程序更应如此。因为使用数组(Array)避免了同步、额外的方法调用和不必要的重新分配空间的操作。

3 throw 和throws的区别
这两者虽然看起来只有一个s的区别，但是作用完全不一样
/////java处理异常方式///////////////////////////////
在java代码中如果发生异常的话，jvm会抛出异常对象，导致程序代码中断，这个时候jvm在做的操作就是：创建异常对象，然后抛出，比如：

int i= 1；
int j = 0；
int res = 0；
res = i/j；//除0错误
System.out.println(res);

这5句代码运行到第四句会中断，因为jvm抛出了异常

////throw的作用/////////////////////////////////////////
手动抛出异常

但是有时候有些错误在jvm看来不是错误，比如说
int age = 0;
age = -100;
System.out.println(age);
很正常的整形变量赋值，但是在我们眼中看来就不正常，谁的年龄会是负的呢。
所以我们需要自己手动引发异常，这就是throw的作用
int age = 0;
age = -100;
if(age<0)
{
Exception e = new Exception();//创建异常对象
throw e;//抛出异常
}
System.out.println(age);

////throws的作用///////////////////////////////////
声明方法可能回避的异常

有异常被抛出了，就要做处理，所以java中有try-catch
可是有时候一个方法中产生了异常，但是不知道该怎么处理它，那么就放着不管，当有异常抛出时会中断该方法，而异常被抛到这个方法的调用者那里。这个有点像下属处理不了的问题就交到上司手里一样，这种情况称为回避异常
但是这使得调用这个方法就有了危险，因为谁也不知道这个方法什么时候会丢一个什么样的异常给调用者，所以在定义方法时，就需要在方法头部分使用throws来声明这个方法可能回避的异常
void fun()throws IOException,SQLException
{
...
}
这表示 fun方法可能会丢两个异常出来，那么在调用fun的时候就会做好准备，比如可以这样
try
{
fun();
}catch(IOException e)
{
}catch(SQLException e)
{
}
////////完毕////////////////////

**Java****知识点总结****2**

前言：现就java学习中的部分知识点做一下总结，以供同学们参考
1    Java中Class类工作原理详解

Class对象
Class对象包含了与类相关的信息。事实上，Class对象就是用来创建类的所有的“普通”对象的。
    类是程序的一部分，每个类都有一个Class对象。换言之，每当编写并且编译了一个新类，就会产生一个Class对象（恰当地说，是被保存在一个同名的.class文件中）。在运行时，当我们想生成这个类的对象时，运行这个程序的 Java虚拟机（JVM）首先检查这个类的Class对象是否已经加载。如果尚未加载，JVM就会根据类名查找.class文件，并将其载入。
一旦某个类的Class对象被载入内存，它就被用来创建这个类的所有对象。看下面示例。

SweetShop.java
package com.zj.sample;
class Candy {
      static {
         System.out.println(Loading Candy);
      }
}
class Gum {
      static {
         System.out.println(Loading Gum);
      }
}
class Cookie {
      static {
         System.out.println(Loading Cookie);
      }
}
public class SweetShop {
      public static void main(String[] args) {
         System.out.println(inside main);
         new Candy();
         System.out.println(After creating Candy);
         try {
             Class.forName(com.zj.sample.Gum);
         } catch (ClassNotFoundException e) {
             System.out.println(Couldn't find Gum);
         }
         System.out.println(After Class.forName(Gum));
         new Cookie();
         System.out.println(After creating Cookie);
      }
}
结果：
inside main
Loading Candy
After creating Candy
Loading Gum
After Class.forName(Gum)
Loading Cookie
After creating Cookie
2．获取Class实例的三种方式
1）利用对象调用getClass()方法获取该对象的Class实例。
2）使用Class类的静态方法forName()，用类的名字获取一个Class实例。
3）运用.class的方式来获取Class实例，对于基本数据类型的封装类，还可以采用.TYPE来获取相对应的基本数据类型的Class实例。
3．Class.forName
上面的示例中：
Class.forName(com.zj.sample.Gum);
这个方法是Class类（所有Class对象都属于这个类）的一个static成员。Class对象就和其它对象一样，我们可以获取并操作它的引用。forName()是取得Class对象的引用的一种方法。它是用一个包含目标类的文本名的String作输入参数，返回的是一个Class对象的引用。
4．类字面常量
Java还提供了另一种方法来生成对Class对象的引用，即使用“类字面常量”。对上述程序来说，可以是：
com.zj.sample.Gum.class;
5．关键字instanceof
关键字instanceof返回一个布尔值，判断是不是某个特定类型的实例。
if(x instanceof Dog) ((Dog)x).bark();
6．获取Class实例
package com.zj.sample;
class Point {
      int x, y;
}
class ClassTest {
      public static void main(String[] args) {
         Point pt = new Point();
         Class c1 = pt.getClass();
         System.out.println(c1.getName());
         try {
             Class c2 = Class.forName(com.zj.sample.Point);
             System.out.println(c2.getName());
         } catch (Exception e) {
             e.printStackTrace();
         }
         Class c3 = Point.class;
         System.out.println(c3.getName());
         Class c4 = int.class;
         System.out.println(c4.getName());
 
         Class c5 = Integer.TYPE;
         System.out.println(c5.getName());
         Class c6 = Integer.class;
         System.out.println(c6.getName());
      }
}
结果：
com.zj.sample.Point
com.zj.sample.Point
com.zj.sample.Point
int
int
java.lang.Integer

7．Class的其他方法
1）Class.newInstance()使用所选的Class对象生成该类的新实例。它调用了缺省（无参数）的类构造器生成新的对象。所以使用newInstance()创建的类必须有一个缺省构造器。对于newInstance ()来说，可以在原先没有任何对象存在的情况下，使用它创建一个新的对象。
利用newInstance()实例化一个对象：
package com.zj.sample;
class Point {
      static {
         System.out.println(Loading Point);
      }
 
      int x, y;
}
class ClassTest {
      public static void main(String[] args) {
         try {
             Class c = Class.forName(com.zj.sample.Point);
             Point pt = (Point) c.newInstance();
         } catch (Exception e) {
             e.printStackTrace();
         }
      }
}
结果：
Loading Point
2）Class.isInstance()方法提供了一种动态地调用instanceof运算符的途径。
3）Class.getInterfaces()方法返回Class对象的数组，这些对象代表的是某个Class对象所包含的接口。
4）如果有一个Class对象，那么就可以通过getSuperclass()获取它的直接基类。这个方法自然也是返回一个Class引用，所以可以进一步查询其基类。这意味着在运行时，可以找到一个对象完整的类层次结构。
5）Class类支持反射的概念，Java附带的库 java.lang.reflect包含了Field、Method以及Constructor类（每个类都实现了Member接口）。这些类型的对象是由JVM在运行时创建的，用以表示未知类里对应的成员。这样可以使用Constructor创建新的对象，用get()和set()方法读取和修改与 Field对象关联的字段，用invoke()方法调用与Method对象关联的方法。另外，还可以调用getFields()、getMethods、 getConstrucotrs()方法，返回表示字段、方法以及构造器的对象的数组。
8．利用反射API察看未知类的构造方法与方法
package com.zj.sample;
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;
class Point {
      static {
         System.out.println(Loading Point);
      }
      int x, y;
      void output() {
         System.out.println(x= + x + , + y= + y);
      }
      Point(int x, int y) {
         this.x = x;
         this.y = y;
      }
}
class ClassTest {
      public static void main(String[] args) {        
         try {
             Class c = Class.forName(com.zj.sample.Point);
             Constructor[] cons = c.getDeclaredConstructors();
             for (int i = 0; i < cons.length; i++)// 返回所有声明的构造方法
             {
                System.out.println(cons[i]);
             }
             Method[] ms = c.getDeclaredMethods();
             for (int i = 0; i < ms.length; i++)// 返回所有声明的方法
             {
                System.out.println(ms[i]);
             }
         } catch (Exception e) {
             e.printStackTrace();
         }
      }
}
 
结果：
Loading Point
com.zj.sample.Point(int,int)
void com.zj.sample.Point.output()
9．动态调用一个类的实例（完全没有出现point这个名字）
package com.zj.sample;
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;
class Point {
      static {
         System.out.println(Loading Point);
      }
      int x, y;
      void output() {
         System.out.println(x= + x + , + y= + y);
      }
      Point(int x, int y) {
         this.x = x;
         this.y = y;
      }
}
class ClassTest {
      public static void main(String[] args) {
        
         try {
             Class c = Class.forName(com.zj.sample.Point);
             Constructor[] cons = c.getDeclaredConstructors();
             Class[] params = cons[0].getParameterTypes();// 察看构造器的参数信息
             Object[] paramValues = new Object[params.length];// 构建数组传递参数
             for (int i = 0; i < params.length; i++) {
                if (params[i].isPrimitive())// 判断class对象表示是否是基本数据类型
                {
                    paramValues[i] = new Integer(i);
                }
             }
             Object o = cons[0].newInstance(paramValues);// 创建一个对象的实例
             Method[] ms = c.getDeclaredMethods();// 调用方法
             ms[0].invoke(o, null);// 用指定的参数调用（output方法没有参数，null）
         } catch (Exception e) {
             e.printStackTrace();
         }
    }
}
结果：
Loading Point
x=0,y=1

 
