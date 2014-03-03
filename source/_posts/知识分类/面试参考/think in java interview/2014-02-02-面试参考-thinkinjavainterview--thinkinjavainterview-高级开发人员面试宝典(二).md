---
layout: post
title: "think in java interview-高级开发人员面试宝典(二)"
categories: 面试参考
tags: 
 - 面试参考
 - think in java interview
--- 

# think in java interview-高级开发人员面试宝典(二)

 

### [[置顶] think in java interview-高级开发人员面试宝典(二)]()

分类： [面经](http://blog.csdn.net/lifetragedy/article/category/1544093) 2013-08-05 00:43 11494人阅读 [评论]()(34) [收藏]( "收藏") [举报]( "举报")
目录[(?)]( "系统根据文章中H1到H6标签自动生成文章目录")[[+]]( "展开")

1. [一个JAVA 的MAIN方法引发的一场血案]()
1. [hashcode equals之5重天]()

1. [何时需要重写equals]()
1. [如何覆写equals和hashcode]()

1. [覆写equals方法]()
1. [覆写hashcode]()
1. [当改写equals的时候总是要改写hashCode]()
1. [两个对象如果equals那么这两个对象的hashcode一定相等如果两个对象的hashcode相等那么这两个对象是否一定equals]()
1. []()
1. [comparable接口与comparator]()
1. []()
1. [java实现shallow clone浅克隆与深克隆deep clone]()
1. [谈谈final finally finalize的区别]()
1. [Java对象的强软弱和虚引用]()

1. []()
1. [强引用]()
1. [SoftReference软引用]()
1. [弱引用]()
1. [PhantomRefrence虚引用]()
1. [为什么需要使用软引用]()
1. [在J2EE的bean设计时我们对有些bean需要实现Serializable为什么]()
1. [每一篇不宜写得过长下篇继续]()

从现在开始，以样题的方式一一列出各种面试题以及点评，考虑到我在前文中说的，对于一些大型的外资型公司，你将会面临全程英语面试，因此我在文章中也会出现许多全英语样题。

这些题目来自于各个真实的公司，公司名我就不一一例举了，是本人一直以来苦心收藏的。

# []()一个JAVA 的MAIN方法引发的一场血案

Q: What if the main method is declared as private?

A: The program compiles properly but at run time it will give "Main method not public." message.

Q: What if the static modifier is removed from the signature of the main method?
A: Program compiles. But at run time throws an error "NoSuchMethodError".

Q: What if I write static public void instead of public static void?

A: Program compiles and runs properly.

Q: What if I do not provide the String array as the argument to the method?
A: Program compiles but throws a run time error "NoSuchMethodError".

Q: What is the first argument of the String array in main method?

A: The String array is empty. It does not have any element. This is unlike C/C++（读作plus plus) where the first element by default is the program name.

Q: If I do not provide any arguments on the command line, then the String array of Main method will be empty or null?
A: It is empty. But not null.

Q: How can one prove that the array is notnull but empty using one line of code?

A: Print args.length. It will print 0. That means it is empty. But if it would have been null then it would have thrown a NullPointerException on attempting to print args.length.

仔细看完后有人直接吐血了，拿个eclipse，这几个问题全部模拟一边就可以了，即无算法也不需要死记硬背

有人会说了，唉，我怎么写了5年的JAVA怎么就没记得多写多看，多想想这个public static void main(String[] args)方法呢？唉。。。

再来！！！

# []()hashcode & equals之5重天

## []()**何时需要重写equals()**

当一个类有自己特有的“逻辑相等”概念（不同于对象身份的概念）。

## []()**如何覆写equals()和hashcode**

****

### []()**覆写equals方法**

1 使用instanceof操作符检查“实参是否为正确的类型”。

2 对于类中的每一个“关键域”，检查实参中的域与当前对象中对应的域值。

3. 对于非float和double类型的原语类型域，使用==比较；

4 对于对象引用域，递归调用equals方法；

5 对于float域，使用Float.*floatToIntBits*(afloat)转换为int，再使用==比较；

6 对于double域，使用Double.*doubleToLongBits*(adouble)转换为int，再使用==比较；

7 对于数组域，调用Arrays.equals方法。

### []()**覆写hashcode**

1. 把某个非零常数值，例如17，保存在int变量result中；

2. 对于对象中每一个关键域f（指equals方法中考虑的每一个域）：

3, boolean型，计算(f? 0 : 1);

4. byte,char,short型，计算(int);

5. long型，计算(int)(f ^ (f>>>32));

6. float型，计算Float.*floatToIntBits*(afloat);

7. double型，计算Double.*doubleToLongBits*(adouble)得到一个long，再执行[2.3];

8. 对象引用，递归调用它的hashCode方法;

9. 数组域，对其中每个元素调用它的hashCode方法。

10. 将上面计算得到的散列码保存到int变量c，然后执行result=37*result+c;

11. 返回result。

举个例子：

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. public class MyUnit{
1. private short ashort;
1. private char achar;
1. private byte abyte;
1. private boolean abool;
1. private long along;
1. private float afloat;
1. private double adouble;
1. private Unit aObject;
1. private int[] ints;
1. private Unit[] units;
1.
1. public boolean equals(Object o) {
1. if (!(o instanceof Unit))
1. return false;
1. Unit unit = (Unit) o;
1. return unit.ashort == ashort
1. && unit.achar == achar
1. && unit.abyte == abyte
1. && unit.abool == abool
1. && unit.along == along
1. && Float.floatToIntBits(unit.afloat) == Float
1. .floatToIntBits(afloat)
1. && Double.doubleToLongBits(unit.adouble) == Double
1. .doubleToLongBits(adouble)
1. && unit.aObject.equals(aObject)
1. && equalsInts(unit.ints)
1. && equalsUnits(unit.units);
1. }
1.
1. private boolean equalsInts(int[] aints) {
1. return Arrays.equals(ints, aints);
1. }
1.
1. private boolean equalsUnits(Unit[] aUnits) {
1. return Arrays.equals(units, aUnits);
1. }
1.
1. public int hashCode() {
1. int result = 17;
1. result = 31 * result + (int) ashort;
1. result = 31 * result + (int) achar;
1. result = 31 * result + (int) abyte;
1. result = 31 * result + (abool ? 0 : 1);
1. result = 31 * result + (int) (along ^ (along >>> 32));
1. result = 31 * result + Float.floatToIntBits(afloat);
1. long tolong = Double.doubleToLongBits(adouble);
1. result = 31 * result + (int) (tolong ^ (tolong >>> 32));
1. result = 31 * result + aObject.hashCode();
1. result = 31 * result + intsHashCode(ints);
1. result = 31 * result + unitsHashCode(units);
1. return result;
1. }
1.
1. private int intsHashCode(int[] aints) {
1. int result = 17;
1. for (int i = 0; i < aints.length; i++)
1. result = 31 * result + aints[i];
1. return result;
1. }
1.
1. private int unitsHashCode(Unit[] aUnits) {
1. int result = 17;
1. for (int i = 0; i < aUnits.length; i++)
1. result = 31 * result + aUnits[i].hashCode();
1. return result;
1. }
1. }

public class MyUnit{

private short ashort;
private char achar;

private byte abyte;
private boolean abool;

private long along;
private float afloat;

private double adouble;
private Unit aObject;

private int[] ints;
private Unit[] units;

public boolean equals(Object o) {
if (!(o instanceof Unit))

return false;
Unit unit = (Unit) o;

return unit.ashort == ashort
&& unit.achar == achar

&& unit.abyte == abyte
&& unit.abool == abool

&& unit.along == along
&& Float.floatToIntBits(unit.afloat) == Float

.floatToIntBits(afloat)
&& Double.doubleToLongBits(unit.adouble) == Double

.doubleToLongBits(adouble)
&& unit.aObject.equals(aObject)

&& equalsInts(unit.ints)
&& equalsUnits(unit.units);

}
private boolean equalsInts(int[] aints) {

return Arrays.equals(ints, aints);
}

private boolean equalsUnits(Unit[] aUnits) {
return Arrays.equals(units, aUnits);

}
public int hashCode() {

int result = 17;
result = 31 * result + (int) ashort;

result = 31 * result + (int) achar;
result = 31 * result + (int) abyte;

result = 31 * result + (abool ? 0 : 1);
result = 31 * result + (int) (along ^ (along >>> 32));

result = 31 * result + Float.floatToIntBits(afloat);
long tolong = Double.doubleToLongBits(adouble);

result = 31 * result + (int) (tolong ^ (tolong >>> 32));
result = 31 * result + aObject.hashCode();

result = 31 * result + intsHashCode(ints);
result = 31 * result + unitsHashCode(units);

return result;
}

private int intsHashCode(int[] aints) {
int result = 17;

for (int i = 0; i < aints.length; i++)
result = 31 * result + aints[i];

return result;
}

private int unitsHashCode(Unit[] aUnits) {
int result = 17;

for (int i = 0; i < aUnits.length; i++)
result = 31 * result + aUnits[i].hashCode();

return result;
}

}

### []()**当改写equals()的时候，总是要改写hashCode()**

根据一个类的equals方法（改写后），两个截然不同的实例有可能在逻辑上是相等的，但是，根据Object.hashCode方法，它们仅仅是两个对象。因此，违反了“相等的对象必须具有相等的散列码”。

### []()**两个对象如果equals那么这两个对象的hashcode一定相等，如果两个对象的hashcode相等那么这两个对象是否一定equals?**

回答是不一定，这要看这两个对象有没有重写Object的hashCode方法和equals方法。如果没有重写，是按Object默认的方式去处理。

试想我有一个桶，这个桶就是hashcode，桶里装的是西瓜我们认为西瓜就是object，有的桶是一个桶装一个西瓜，有的桶是一个桶装多个西瓜。

比如String重写了Object的hashcode和equals，但是两个String如果hashcode相等，那么equals比较肯定是相等的，但是“==”比较却不一定相等。如果自定义的对象重写了hashCode方法，有可能hashcode相等，equals却不一定相等，“==”比较也不一定相等。

**此处考的是你对object的hashcode的意义的真正的理解！！！如果作为一名高级开发人员或者是架构师，必须是要有这个概念的，否则，直接ban掉了。**

为什么我们在定义hashcode时如： h = 31*h + val[off++]; 要使用31这个数呢？
public int hashCode() {
int h = hash;
int len = count;
if (h == 0 && len > 0) {
int off = offset;
char val[] = value;
for (int i = 0; i < len; i++) {
h = 31*h + val[off++];
}
hash = h;
}
return h;
}

来看一段hashcode的覆写案例：

其实上面的实现也可以总结成数数里面下面这样的公式：

s[0]*31^(n-1) + s[1]*31^(n-2) + … + s[n-1]

我们来看这个要命的**31**这个系数为什么总是在里面乘啊乘的？为什么不适用**32**或者其他数字？

大家都知道，计算机的乘法涉及到移位计算。当一个数乘以2时，就直接拿该数左移一位即可！选择**31**原因是因为31是一个素数！

所谓素数：

质数又称素数

素数在使用的时候有一个作用就是如果我用一个数字来乘以这个素数，那么最终的出来的结果只能被素数本身和被乘数还有1来整除！如：我们选择素数3来做系数，那么3*n只能被3和n或者1来整除，我们可以很容易的通过3n来计算出这个n来。这应该也是一个原因！

**在存储数据计算hash地址的时候，我们希望尽量减少有同样的hash地址，所谓“冲突”**。

31是个神奇的数字，因为任何数n * 31就可以被JVM优化为 (n << 5) -n,移位和减法的操作效率要比乘法的操作效率高的多，对左移现在很多虚拟机里面都有做相关优化，并且31只占用5bits！

hashcode & equals基本问到第三问，很多人就已经挂了，如果问到了为什么要使用31比较好呢，90%的人无法回答或者只回答出一半。

此处考的是编程者在平时开发时是否经常考虑“性能”这个问题。

# []()

# []()comparable接口与comparator

两种比较接口分析

前者应该比较固定，和一个具体类相绑定，而后者比较灵活，它可以被用于各个需要比较功能的类使用。

一个类实现了 Camparable 接口表明这个类的对象之间是可以相互比较的。如果用数学语言描述的话就是这个类的对象组成的集合中存在一个全序。这样，这个类对象组成的集合就可以使用 Sort 方法排序了。

而 Comparator 的作用有两个：
1. 如果类的设计师没有考虑到 Compare 的问题而没有实现 Comparable 接口，可以通过 Comparator 来实现比较算法进行排序；
2. 为了使用不同的排序标准做准备，比如：升序、降序或其他什么序。

在 “集合框架” 中有两种比较接口： Comparable 接口和 Comparator 接口。

Comparable 是通用的接口，用户可以实现它来完成自己特定的比较，而 Comparator 可以看成一种算法的实现，在需要容器集合实现比较功能的时候，来指定这个比较器，这可以看成一种设计模式，将算法和数据分离。

来看样例：

PersonBean类

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">public class PersonBean implements Comparable<PersonBean> {
1. public PersonBean(int age, String name) {
1. this.age = age;
1. this.name = name;
1. }
1.
1. int age = 0;
1. String name = "";
1.
1. public int getAge() {
1. return age;
1. }
1.
1. public void setAge(int age) {
1. this.age = age;
1. }
1.
1. public String getName() {
1. return name;
1. }
1.
1. public void setName(String name) {
1. this.name = name;
1. }
1.
1. public boolean equals(Object o) {
1. if (!(o instanceof PersonBean)) {
1. return false;
1. }
1. PersonBean p = (PersonBean) o;
1. return (age == p.age) && (name.equals(p.name));
1. }
1.
1. public int hashCode() {
1. int result = 17;
1. result = 31 * result + age;
1. result = 31 * result + name.hashCode();
1. return result;
1.
1. }
1.
1. public String toString() {
1. return (age + "{" + name + "}");
1. }
1.
1. public int compareTo(PersonBean person) {
1. int cop = age - person.getAge();
1. if (cop != 0)
1. return cop;
1. else
1. return name.compareTo(person.name);
1. }
1. }
1. </span>

public class PersonBean implements Comparable<PersonBean> {

public PersonBean(int age, String name) {
this.age = age;

this.name = name;
}

int age = 0;
String name = "";

public int getAge() {
return age;

}
public void setAge(int age) {

this.age = age;
}

public String getName() {
return name;

}
public void setName(String name) {

this.name = name;
}

public boolean equals(Object o) {
if (!(o instanceof PersonBean)) {

return false;
}

PersonBean p = (PersonBean) o;
return (age == p.age) && (name.equals(p.name));

}
public int hashCode() {

int result = 17;
result = 31 * result + age;

result = 31 * result + name.hashCode();
return result;

}
public String toString() {

return (age + "{" + name + "}");
}

public int compareTo(PersonBean person) {
int cop = age - person.getAge();

if (cop != 0)
return cop;

else
return name.compareTo(person.name);

}
}
AlphDesc类

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">import java.util.Comparator;
1.
1. public class AlphDesc implements Comparator<PersonBean> {
1.
1. public int compare(PersonBean personA, PersonBean personB) {
1. int cop = personA.age - personB.age;
1. if (cop != 0)
1. return cop;
1. else
1. return personB.getName().compareTo(personA.getName());
1.
1. }
1.
1. }
1. </span>

import java.util.Comparator;

public class AlphDesc implements Comparator<PersonBean> {
public int compare(PersonBean personA, PersonBean personB) {

int cop = personA.age - personB.age;
if (cop != 0)

return cop;
else

return personB.getName().compareTo(personA.getName());
}

}
TestComparable类

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">import java.util.*;
1.
1. public class TestComparable {
1.
1. /**
1. * @param args
1. */
1. public void compare() {
1. PersonBean[] p = { new PersonBean(20, "Tom"),
1. new PersonBean(20, "Jeff"),
1. new PersonBean(30, "Mary"),
1. new PersonBean(20, "Ada"),
1. new PersonBean(40, "Walton"),
1. new PersonBean(61, "Peter"),
1. new PersonBean(20, "Bush") };
1. System.out.println("before sort:\n" + Arrays.toString(p));
1. AlphDesc desc = new AlphDesc();
1. Arrays.sort(p,desc);
1. System.out.println("after sort:\n" + Arrays.toString(p));
1. }
1.
1. public static void main(String[] args) {
1. TestComparable tc = new TestComparable();
1. tc.compare();
1. }
1.
1. }</span>

import java.util.*;

public class TestComparable {
/**

* @param args
*/

public void compare() {
PersonBean[] p = { new PersonBean(20, "Tom"),

new PersonBean(20, "Jeff"),
new PersonBean(30, "Mary"),

new PersonBean(20, "Ada"),
new PersonBean(40, "Walton"),

new PersonBean(61, "Peter"),
new PersonBean(20, "Bush") };

System.out.println("before sort:\n" + Arrays.toString(p));
AlphDesc desc = new AlphDesc();

Arrays.sort(p,desc);
System.out.println("after sort:\n" + Arrays.toString(p));

}
public static void main(String[] args) {

TestComparable tc = new TestComparable();
tc.compare();

}
}

# []()

# []()java实现shallow clone(浅克隆）与深克隆(deep clone)

克隆就是复制一个对象的复本.但一个对象中可能有基本数据类型,如:int,long,float 等,也同时含有非基本数据类型如(数组,集合等)被克隆得到的对象基本类型的值修改了,原对象的值不会改变.这种适合shadow clone(浅克隆).
但如果你要改变一个非基本类型的值时,原对象的值却改变了,.比如一个数组,内存中只copy他的地址,而这个地址指向的值并没有 copy,当clone时,两个地址指向了一个值,这样一旦这个值改变了,原来的值当然也变了,因为他们共用一个值.,这就必须得用深克隆(deep clone)
以下举个例子,说明以上情况.
被克隆类:ShadowClone.java

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">public class ShadowClone implements Cloneable
1. {
1. // 基本类型
1. private int a;
1. // 非基本类型
1. private String b;
1. // 非基本类型
1. private int[] c;
1. // 重写Object.clone()方法,并把protected改为public
1. @Override
1. public Object clone()
1. {
1. ShadowClone sc = null;
1. try
1. {
1. sc = (ShadowClone) super.clone();
1. } catch (CloneNotSupportedException e)
1. {
1. e.printStackTrace();
1. }
1. return sc;
1. }
1. public int getA()
1. {
1. return a;
1. }
1. public void setA(int a)
1. {
1. this.a = a;
1. }
1. public String getB()
1. {
1. return b;
1. }
1. public void setB(String b)
1. {
1. this.b = b;
1. }
1. public int[] getC()
1. {
1. return c;
1. }
1. public void setC(int[] c)
1. {
1. this.c = c;
1. }
1. }</span>

public class ShadowClone implements Cloneable

{
// 基本类型

private int a;
// 非基本类型

private String b;
// 非基本类型

private int[] c;
// 重写Object.clone()方法,并把protected改为public

@Override
public Object clone()

{
ShadowClone sc = null;

try
{

sc = (ShadowClone) super.clone();
} catch (CloneNotSupportedException e)

{
e.printStackTrace();

}
return sc;

}
public int getA()

{
return a;

}
public void setA(int a)

{
this.a = a;

}
public String getB()

{
return b;

}
public void setB(String b)

{
this.b = b;

}
public int[] getC()

{
return c;

}
public void setC(int[] c)

{
this.c = c;

}
}
测试类Test.java
**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">public class Test
1. {
1. public static void main(String[] args) throws CloneNotSupportedException
1. {
1. ShadowClone c1 = new ShadowClone();
1. //对c1赋值
1. c1.setA(100) ;
1. c1.setB("clone1") ;
1. c1.setC(new int[]{1000}) ;
1.
1. System.out.println("克隆前: c1.a="+c1.getA() );
1. System.out.println("克隆前: c1.b="+c1.getB() );
1. System.out.println("克隆前: c1.c[0]="+c1.getC()[0]);
1. System.out.println("-----------") ;
1.
1. //克隆出对象c2,并对c2的属性A,B,C进行修改
1.
1. ShadowClone c2 = (ShadowClone) c1.clone();
1.
1. //对c2进行修改
1. c2.setA(50) ;
1. c2.setB("clone2");
1. int []a = c2.getC() ;
1. a[0]=500 ;
1. c2.setC(a);
1.
1. System.out.println("克隆后: c1.a="+c1.getA() );
1. System.out.println("克隆后: c1.b="+c1.getB() );
1. System.out.println("克隆后: c1.c[0]="+c1.getC()[0]);
1. System.out.println("---------------") ;
1.
1. System.out.println("克隆后: c2.a=" + c2.getA());
1. System.out.println("克隆后: c2.b=" + c2.getB());
1. System.out.println("克隆后: c2.c[0]=" + c2.getC()[0]);
1. }
1. }</span>

public class Test

{
public static void main(String[] args) throws CloneNotSupportedException

{
ShadowClone c1 = new ShadowClone();

//对c1赋值
c1.setA(100) ;

c1.setB("clone1") ;
c1.setC(new int[]{1000}) ;

System.out.println("克隆前: c1.a="+c1.getA() );
System.out.println("克隆前: c1.b="+c1.getB() );

System.out.println("克隆前: c1.c[0]="+c1.getC()[0]);
System.out.println("-----------") ;

//克隆出对象c2,并对c2的属性A,B,C进行修改
ShadowClone c2 = (ShadowClone) c1.clone();

//对c2进行修改
c2.setA(50) ;

c2.setB("clone2");
int []a = c2.getC() ;

a[0]=500 ;
c2.setC(a);

System.out.println("克隆后: c1.a="+c1.getA() );
System.out.println("克隆后: c1.b="+c1.getB() );

System.out.println("克隆后: c1.c[0]="+c1.getC()[0]);
System.out.println("---------------") ;

System.out.println("克隆后: c2.a=" + c2.getA());
System.out.println("克隆后: c2.b=" + c2.getB());

System.out.println("克隆后: c2.c[0]=" + c2.getC()[0]);
}

}
结果:
克隆前: c1.a=100
克隆前: c1.b=clone1
克隆前: c1.c[0]=1000
-----------
克隆后: c1.a=100
克隆后: c1.b=clone1
克隆后: c1.c[0]=500
---------------
克隆后: c2.a=50
克隆后: c2.b=clone2
克隆后: c2.c[0]=500
问题出现了,我指修改了克隆后的对象c2.c的值,但c1.c的值也改变了,与c2的值相等.
以下针对浅克隆得出结论:基本类型是可以被克隆的,但引用类型只是copy地址,并没有copy这个地址指向的对象的值,这使得两个地址指向同一值,修改其中一个,当然另一个也就变了.
由此可见,浅克隆只适合克隆基本类型,对于引用类型就不能实现克隆了.
那如何实现克隆引用对象呢,以下提供一种方法. 用序列化与反序列化实现深克隆(deep copy)
被克隆对象.DeepClone.java

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">import java.io.Serializable;
1. //要实现深克隆必须实现Serializable接口
1. public class DeepClone implements Serializable
1. {
1. private int a;
1. private String b;
1. private int[] c;
1. public int getA()
1. {
1. return a;
1. }
1. public void setA(int a)
1. {
1. this.a = a;
1. }
1. public String getB()
1. {
1. return b;
1. }
1. public void setB(String b)
1. {
1. this.b = b;
1. }
1. public int[] getC()
1. {
1. return c;
1. }
1. public void setC(int[] c)
1. {
1. this.c = c;
1. }
1. }</span>

import java.io.Serializable;

//要实现深克隆必须实现Serializable接口
public class DeepClone implements Serializable

{
private int a;

private String b;
private int[] c;

public int getA()
{

return a;
}

public void setA(int a)
{

this.a = a;
}

public String getB()
{

return b;
}

public void setB(String b)
{

this.b = b;
}

public int[] getC()
{

return c;
}

public void setC(int[] c)
{

this.c = c;
}

}

测试类Test.java

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">import java.io.ByteArrayInputStream;
1. import java.io.ByteArrayOutputStream;
1. import java.io.IOException;
1. import java.io.ObjectInputStream;
1. import java.io.ObjectOutputStream;
1. public class Test
1. {
1. public static void main(String[] args) throws CloneNotSupportedException
1. {
1. Test t = new Test();
1. DeepClone dc1 = new DeepClone();
1. // 对dc1赋值
1. dc1.setA(100);
1. dc1.setB("clone1");
1. dc1.setC(new int[] { 1000 });
1. System.out.println("克隆前: dc1.a=" + dc1.getA());
1. System.out.println("克隆前: dc1.b=" + dc1.getB());
1. System.out.println("克隆前: dc1.c[0]=" + dc1.getC()[0]);
1. System.out.println("-----------");
1. DeepClone dc2 = (DeepClone) t.deepClone(dc1);
1. // 对c2进行修改
1. dc2.setA(50);
1. dc2.setB("clone2");
1. int[] a = dc2.getC();
1. a[0] = 500;
1. dc2.setC(a);
1. System.out.println("克隆前: dc1.a=" + dc1.getA());
1. System.out.println("克隆前: dc1.b=" + dc1.getB());
1. System.out.println("克隆前: dc1.c[0]=" + dc1.getC()[0]);
1. System.out.println("-----------");
1. System.out.println("克隆后: dc2.a=" + dc2.getA());
1. System.out.println("克隆后: dc2.b=" + dc2.getB());
1. System.out.println("克隆后: dc2.c[0]=" + dc2.getC()[0]);
1. }
1. // 用序列化与反序列化实现深克隆
1. public Object deepClone(Object src)
1. {
1. Object o = null;
1. try
1. {
1. if (src != null)
1. {
1. ByteArrayOutputStream baos = new ByteArrayOutputStream();
1. ObjectOutputStream oos = new ObjectOutputStream(baos);
1. oos.writeObject(src);
1. oos.close();
1. ByteArrayInputStream bais = new ByteArrayInputStream(baos
1. .toByteArray());
1. ObjectInputStream ois = new ObjectInputStream(bais);
1. o = ois.readObject();
1. ois.close();
1. }
1. } catch (IOException e)
1. {
1. e.printStackTrace();
1. } catch (ClassNotFoundException e)
1. {
1. e.printStackTrace();
1. }
1. return o;
1. }
1. }</span>

import java.io.ByteArrayInputStream;

import java.io.ByteArrayOutputStream;
import java.io.IOException;

import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;

public class Test
{

public static void main(String[] args) throws CloneNotSupportedException
{

Test t = new Test();
DeepClone dc1 = new DeepClone();

// 对dc1赋值
dc1.setA(100);

dc1.setB("clone1");
dc1.setC(new int[] { 1000 });

System.out.println("克隆前: dc1.a=" + dc1.getA());
System.out.println("克隆前: dc1.b=" + dc1.getB());

System.out.println("克隆前: dc1.c[0]=" + dc1.getC()[0]);
System.out.println("-----------");

DeepClone dc2 = (DeepClone) t.deepClone(dc1);
// 对c2进行修改

dc2.setA(50);
dc2.setB("clone2");

int[] a = dc2.getC();
a[0] = 500;

dc2.setC(a);
System.out.println("克隆前: dc1.a=" + dc1.getA());

System.out.println("克隆前: dc1.b=" + dc1.getB());
System.out.println("克隆前: dc1.c[0]=" + dc1.getC()[0]);

System.out.println("-----------");
System.out.println("克隆后: dc2.a=" + dc2.getA());

System.out.println("克隆后: dc2.b=" + dc2.getB());
System.out.println("克隆后: dc2.c[0]=" + dc2.getC()[0]);

}
// 用序列化与反序列化实现深克隆

public Object deepClone(Object src)
{

Object o = null;
try

{
if (src != null)

{
ByteArrayOutputStream baos = new ByteArrayOutputStream();

ObjectOutputStream oos = new ObjectOutputStream(baos);
oos.writeObject(src);

oos.close();
ByteArrayInputStream bais = new ByteArrayInputStream(baos

.toByteArray());
ObjectInputStream ois = new ObjectInputStream(bais);

o = ois.readObject();
ois.close();

}
} catch (IOException e)

{
e.printStackTrace();

} catch (ClassNotFoundException e)
{

e.printStackTrace();
}

return o;
}

}
结果:
克隆前: dc1.a=100
克隆前: dc1.b=clone1
克隆前: dc1.c[0]=1000
-----------
克隆前: dc1.a=100
克隆前: dc1.b=clone1
克隆前: dc1.c[0]=1000
-----------
克隆后: dc2.a=50
克隆后: dc2.b=clone2
克隆后: dc2.c[0]=500
深克隆后:修改dc1或者dc2,无论是基本类型还是引用类型,他们的值都不会随着一方改变另一方也改变.

**总结:**

当克隆的对象只有基本类型,不含引用类型时,可以用浅克隆实现.

当克隆的对象含有引用类型时,必须使用深克隆实现.

# []()谈谈final， finally， finalize的区别

final修饰符（关键字）

如果一个类被声明为final，意味着它不能再派生出新的子类，不能作为父类被继承。因此一个类不能既被声明为 abstract的，又被声明为final的。将变量或方法声明为final，可以保证它们在使用中不被改变。被声明为final的变量必须在声明时给定初值，而在以后的引用中只能读取，不可修改。被声明为final的方法也同样只能使用，不能重载finally？再异常处理时提供 finally 块来执行任何清除操作。如果抛出一个异常，那么相匹配的 catch 子句就会执行，然后控制就会进入 finally 块（如果有的话）。
finalize方法名

Java 技术允许使用 finalize（）方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。这个方法是由垃圾收集器在确定这个对象没有被引用时对这个对象调用的。它是在 Object 类中定义的，因此所有的类都继承了它。子类覆盖 finalize（） 方法以整理系统资源或者执行其他清理工作。finalize（）方法是在垃圾收集器删除对象之前对这个对象调用的。

# []()Java对象的强、软、弱和虚引用

## []()

## []()**强引用**

****

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">Object o=new Object();
1. Object o1=o; </span>
Object o=new Object();

Object o1=o;
上面代码中第一句是在heap堆中创建新的Object对象通过o引用这个对象，第二句是通过o建立o1到new Object()这个heap堆中的对象的引用，这两个引用都是强引用.只要存在对heap中对象的引用，gc就不会收集该对象.如果通过如下代码：

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">o=null;
1. o1=null;</span>

o=null;

o1=null;
如果显式地设置o和o1为null，或超出范围，则gc认为该对象不存在引用，这时就可以收集它了。可以收集并不等于就一会被收集，什么时候收集这要取 决于gc的算法，这要就带来很多不确定性。例如你就想指定一个对象，希望下次gc运行时把它收集了，那就没办法了，有了其他的三种引用就可以做到了。其他 三种引用在不妨碍gc收集的情况下，可以做简单的交互。
heap中对象有强可及对象、软可及对象、弱可及对象、虚可及对象和不可到达对象。应用的强弱顺序是强、软、弱、和虚。对于对象是属于哪种可及的对象，由他的最强的引用决定。如下：
**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">String abc=new String("abc"); //1
1. SoftReference<String> abcSoftRef=new SoftReference<String>(abc); //2
1. WeakReference<String> abcWeakRef = new WeakReference<String>(abc); //3
1. abc=null; //4
1. abcSoftRef.clear();//5</span>

String abc=new String("abc"); //1

SoftReference<String> abcSoftRef=new SoftReference<String>(abc); //2
WeakReference<String> abcWeakRef = new WeakReference<String>(abc); //3

abc=null; //4
abcSoftRef.clear();//5
第一行在heap对中创建内容为“abc”的对象，并建立abc到该对象的强引用,该对象是强可及的。
第二行和第三行分别建立对heap中对象的软引用和弱引用，此时heap中的对象仍是强可及的。
第四行之后heap中对象不再是强可及的，变成软可及的。同样第五行执行之后变成弱可及的。

## []()SoftReference（软引用）

软引用是主要用于内存敏感的高速缓存。在jvm报告内存不足之前会清除所有的软引用，这样以来gc就有可能收集软可及的对象，可能解决内存吃紧问题，避 免内存溢出。什么时候会被收集取决于gc的算法和gc运行时可用内存的大小。当gc决定要收集软引用是执行以下过程,以上面的abc SoftRef为例：
1、首先将abcSoftRef的referent设置为null，不再引用heap中的new String("abc")对象。
2、将heap中的new String("abc")对象设置为可结束的(finalizable)。
3、当heap中的new String("abc")对象的finalize()方法被运行而且该对象占用的内存被释放， abcSoftRef被添加到它的ReferenceQueue中。
**注:对ReferenceQueue软引用和弱引用可以有可无，但是虚引用必须有，参见：**
**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">Reference(T paramT, ReferenceQueue<? super T>paramReferenceQueue) </span>

Reference(T paramT, ReferenceQueue<? super T>paramReferenceQueue)
被 Soft Reference 指到的对象，即使没有任何 Direct Reference，也不会被清除。
一直要到 JVM 内存不足且 没有 Direct Reference 时才会清除，SoftReference 是用来设计 object-cache 之用的。

如此一来 SoftReference 不但可以把对象 cache 起来，也不会造成内存不足的错误 （OutOfMemoryError）。我觉得 Soft Reference 也适合拿来实作 pooling 的技巧。
**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">A obj = new A();
1. SoftRefenrence sr = new SoftReference(obj);
1.
1. //引用时
1. if(sr!=null){
1. obj = sr.get();
1. }else{
1. obj = new A();
1. sr = new SoftReference(obj);
1. }</span>

A obj = new A();

SoftRefenrence sr = new SoftReference(obj);
//引用时

if(sr!=null){
obj = sr.get();

}else{
obj = new A();

sr = new SoftReference(obj);
}

## []()弱引用

当gc碰到弱可及对象，并释放abcWeakRef的引用，收集该对象。但是gc可能需要对此运用才能找到该弱可及对象。通过如下代码可以了明了的看出它的作用：
**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">String abc=new String("abc");
1. WeakReference<String> abcWeakRef = new WeakReference<String>(abc);
1. abc=null;
1. System.out.println("before gc: "+abcWeakRef.get());
1. System.gc();
1. System.out.println("after gc: "+abcWeakRef.get()); </span>

String abc=new String("abc");

WeakReference<String> abcWeakRef = new WeakReference<String>(abc);
abc=null;

System.out.println("before gc: "+abcWeakRef.get());
System.gc();

System.out.println("after gc: "+abcWeakRef.get());
运行结果：
before gc: abc
after gc: null
gc收集弱可及对象的执行过程和软可及一样，只是gc不会根据内存情况来决定是不是收集该对象。
如果你希望能随时取得某对象的信息，但又不想影响此对象的垃圾收集，那么你应该用 Weak Reference 来记住此对象，而不是用一般的 reference。

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">A obj = new A();
1.
1. WeakReference wr = new WeakReference(obj);
1.
1. obj = null;
1.
1. //等待一段时间，obj对象就会被垃圾回收
1. ...
1.
1. if (wr.get()==null) {
1. System.out.println("obj 已经被清除了 ");
1. } else {
1. System.out.println("obj 尚未被清除，其信息是 "+obj.toString());
1. }
1. ...</span>

A obj = new A();

WeakReference wr = new WeakReference(obj);
obj = null;

//等待一段时间，obj对象就会被垃圾回收
...

if (wr.get()==null) {
System.out.println("obj 已经被清除了 ");

} else {
System.out.println("obj 尚未被清除，其信息是 "+obj.toString());

}
...
在此例中，透过 get() 可以取得此 Reference 的所指到的对象，如果返回值为 null 的话，代表此对象已经被清除。
这类的技巧，在设计 Optimizer 或 Debugger 这类的程序时常会用到，因为这类程序需要取得某对象的信息，但是不可以 影响此对象的垃圾收集。
## []()PhantomRefrence（虚引用）

虚顾名思义就是没有的意思，建立虚引用之后通过get方法返回结果始终为null，通过源代码你会发现，虚引用通向会把引用的对象写进referent，只是get方法返回结果为null。先看一下和gc交互的过程在说一下他的作用。
1 不把referent设置为null，直接把heap中的new String("abc")对象设置为可结束的(finalizable).
2 与软引用和弱引用不同，先把PhantomRefrence对象添加到它的ReferenceQueue中，然后在释放虚可及的对象。
你会发现在收集heap中的new String("abc")对象之前，你就可以做一些其他的事情。通过以下代码可以了解他的作用。

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. <span style="font-size:12px;">import java.lang.ref.PhantomReference;
1. import java.lang.ref.Reference;
1. import java.lang.ref.ReferenceQueue;
1. import java.lang.reflect.Field;
1.
1. public class Test {
1. public static boolean isRun = true;
1.
1. public static void main(String[] args) throws Exception {
1. String abc = new String("abc");
1. System.out.println(abc.getClass() + "@" + abc.hashCode());
1. final ReferenceQueue referenceQueue = new ReferenceQueue<String>();
1. new Thread() {
1. public void run() {
1. while (isRun) {
1. Object o = referenceQueue.poll();
1. if (o != null) {
1. try {
1. Field rereferent = Reference.class
1. .getDeclaredField("referent");
1. rereferent.setAccessible(true);
1. Object result = rereferent.get(o);
1. System.out.println("gc will collect:"
1. + result.getClass() + "@"
1. + result.hashCode());
1. } catch (Exception e) {
1.
1. e.printStackTrace();
1. }
1. }
1. }
1. }
1. }.start();
1. PhantomReference<String> abcWeakRef = new PhantomReference<String>(abc,
1. referenceQueue);
1. abc = null;
1. Thread.currentThread().sleep(3000);
1. System.gc();
1. Thread.currentThread().sleep(3000);
1. isRun = false;
1. }
1.
1. }</span>

import java.lang.ref.PhantomReference;

import java.lang.ref.Reference;
import java.lang.ref.ReferenceQueue;

import java.lang.reflect.Field;
public class Test {

public static boolean isRun = true;
public static void main(String[] args) throws Exception {

String abc = new String("abc");
System.out.println(abc.getClass() + "@" + abc.hashCode());

final ReferenceQueue referenceQueue = new ReferenceQueue<String>();
new Thread() {

public void run() {
while (isRun) {

Object o = referenceQueue.poll();
if (o != null) {

try {
Field rereferent = Reference.class

.getDeclaredField("referent");
rereferent.setAccessible(true);

Object result = rereferent.get(o);
System.out.println("gc will collect:"

+ result.getClass() + "@"
+ result.hashCode());

} catch (Exception e) {
e.printStackTrace();

}
}

}
}

}.start();
PhantomReference<String> abcWeakRef = new PhantomReference<String>(abc,

referenceQueue);
abc = null;

Thread.currentThread().sleep(3000);
System.gc();

Thread.currentThread().sleep(3000);
isRun = false;

}
}
结果为：
**class java.lang.String@96354
gc will collect:class java.lang.String@96354**

## []()为什么需要使用软引用

首先，我们看一个雇员信息查询系统的实例。

我们将使用一个Java语言实现的雇员信息查询系统查询存储在磁盘文件或者数据库中的雇员人事档案信息。

作为一个用户，我们完全有可能需要回头去查看几分钟甚至几秒钟前查看过的雇员档案信息(同样，我们在浏览WEB页面的时候也经常会使用“后退”按钮)。

这时我们通常会有两种程序实现方式:

一种是把过去查看过的雇员信息保存在内存中，每一个存储了雇员档案信息的Java对象的生命周期贯穿整个应用程序始终;

另一种是当用户开始查看其他雇员的档案信息的时候，把存储了当前所查看的雇员档案信息的Java对象结束引用，使得垃圾收集线程可以回收其所占用的内存空间，当用户再次需要浏览该雇员的档案信息的时候，重新构建该雇员的信息。

很显然，第一种实现方法将造成大量的内存浪费，而第二种实现的缺陷在于即使垃圾收集线程还没有进行垃圾收集，包含雇员档案信息的对象仍然完好地保存在内存中，应用程序也要重新构建一个对象。

我们知道，访问磁盘文件、访问网络资源、查询数据库等操作都是影响应用程序执行性能的重要因素，如果能重新获取那些尚未被回收的Java对象的引用，必将减少不必要的访问，大大提高程序的运行速度。

# []()在J2EE的bean设计时我们对有些bean需要实现Serializable，为什么？

实现了Serializable接口的对象，可将它们转换成一系列字节，并可在以后完全恢复回原来的样子。这一过程亦可通过网络进行。这意味着序列化机制能自动补偿操作系统间的差异。换句话说，可以先在Windows机器上创建一个对象，对其序列化，然后通过网络发给一台Unix机器，然后在那里准确无误地重新“装配”。不必关心数据在不同机器上如何表示，也不必关心字节的顺序或者其他任何细节。

# []()每一篇不宜写得过长，下篇继续
分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")

1. 上一篇：[think in java interview-高级开发人员面试宝典(一)](http://blog.csdn.net/lifetragedy/article/details/9718567)
1. 下一篇：[think in java interview-高级开发人员面试宝典(三)](http://blog.csdn.net/lifetragedy/article/details/9766087)
顶23 踩1

查看评论[]()
28楼 [wuming5920](http://blog.csdn.net/wm5920) 3小时前发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/wm5920)来看第二遍 27楼 [lovexieyuan520](http://blog.csdn.net/lovexieyuan520) 昨天 23:33发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lovexieyuan520)看看 26楼 [王斐-Beta2](http://blog.csdn.net/waylife) 3天前 11:09发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/waylife)先收藏了 25楼 [local_host_8080](http://blog.csdn.net/yb331851841) 3天前 15:07发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/yb331851841)这几篇文章很好啊，看了受益匪浅，但是我有个问题关于hashcode（）的，既然JVM能够将乘法优化成<<<，那为什么我们写程序的时候不直接写<<< 而要乘以31呢 Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 3天前 16:10发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复yb331851841：当然可以，为什么不可以，你可以自己根据“计算机原理”去左移实现啊，呵呵，是不是比较烦？
因此我们用*，然后寄存器在把这个*的操作变成<<操作时一样可以优化！！！ 24楼 [柔龙笑](http://blog.csdn.net/SSH8080) 3天前 15:01发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/SSH8080)好一个31，现在才知道有这个数！谢谢楼主！ 23楼 [wj321666](http://blog.csdn.net/wj32166) 3天前 15:00发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/wj32166)equals方法中这句 unit.aObject.equals(aObject)没有判断null。
类名应该是Unit。 22楼 [Spring_Rookie](http://blog.csdn.net/hulu2moyu) 4天前 17:36发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/hulu2moyu)谢谢楼主分享，正在学习 21楼 [hxw4656](http://blog.csdn.net/hxw4656) 4天前 15:53发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/hxw4656)**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. public boolean equals(Object obj) {
1. if(obj instanceof DataSource == false) return false;
1. if(this == obj) return true;
1. DataSource other = (DataSource)obj;
1. return new EqualsBuilder().append(this.getDatasourceIp(),other.getDatasourceIp())
1. .append(this.getDatasourcePort(),other.getDatasourcePort())
1. .append(this.getUserName(),other.getUserName())
1. .append(this.getPassword(),other.getPassword())
1. .isEquals();
1. }

public boolean equals(Object obj) {

if(obj instanceof DataSource == false) return false;
if(this == obj) return true;

DataSource other = (DataSource)obj;
return new EqualsBuilder().append(this.getDatasourceIp(),other.getDatasourceIp())

.append(this.getDatasourcePort(),other.getDatasourcePort())
.append(this.getUserName(),other.getUserName())

.append(this.getPassword(),other.getPassword())
.isEquals();

}
这个写法的equals方法不知道给你的有什么区别 Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 4天前 17:10发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复hxw4656：EQUALS覆写为得是什么呢？
为的是逻辑比较，比如说我有一个PERSON，NEW了两个一个p1,一个p2。
我的需求是：如果两个p间的name和年龄，性别一样我就认为是一个人
那么你需要去翻写你的equals方法，翻写好后如何测试呢？去拿个p1.equals(p2)就可以测试了。
所以你要为了测你的equals写的对不对，你也可以NEW出来两个对象，然后去equals试一下！ 20楼 [jiujie395](http://blog.csdn.net/jiujie395) 5天前 11:31发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/jiujie395)看到这些我歇菜了..看来还有很长的路要走.. 19楼 [椰树海岛](http://blog.csdn.net/caodegao) 5天前 10:43发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/caodegao)原来我也只是个渣... 18楼 [tchqiq](http://blog.csdn.net/tchqiq) 5天前 13:45发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/tchqiq)牛啊~等有时间再好好看看 17楼 [Fly_fans](http://blog.csdn.net/Fly_fans) 6天前 14:10发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/Fly_fans)太有技术含量了。。。做了几年开发，我连深克隆都写不出来。。。以后得多注重细节技术了 16楼 [sling](http://blog.csdn.net/sling) 2013-08-22 15:14发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/sling)路过............... 15楼 [WTBEE](http://blog.csdn.net/WTBEE) 2013-08-22 15:13发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/WTBEE)文章不错，真心学习~ 14楼 [qq467339640](http://blog.csdn.net/qq467339640) 2013-08-21 18:39发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/qq467339640)先马克，以后继续，博主辛苦了 13楼 [u010395943](http://blog.csdn.net/u010395943) 2013-08-21 12:40发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/u010395943)写的不错，值得学习 12楼 [小麦_comeon](http://blog.csdn.net/wzu_xiaomai) 2013-08-19 23:47发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/wzu_xiaomai)“那么3*n只能被3和n或者1来整除”这个地方我没看明白，n如果是合数的话会不会不一样了。。。 11楼 [yjingy](http://blog.csdn.net/yjingy) 2013-08-18 22:47发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/yjingy)谢谢分享，这些都没有考虑过这么深过。
另：在“覆写hashcode”部分的第十条中，以及示例代码中的几个地方，系数被写成了37。 Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-19 09:38发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复yjingy：打错了，哈哈 10楼 [a1318535253](http://blog.csdn.net/a1318535253) 2013-08-13 16:44发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/a1318535253)浅克隆 测试类Test.java 中的 int []a = c2.getC() ;
a[0]=500 ;
c2.setC(a);
写成 c2.setC(new int[]{500}) ;
那么原值就不会变,什么原因? Re: [Fly_fans](http://blog.csdn.net/Fly_fans) 6天前 14:13发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/Fly_fans)回复a1318535253：c2.setC(a)是把a的引用复制了一份，给c2
c2.setC(new int[500]) 是在栈中新建了一个数组，然后赋值给c2.这个新建的数组跟a不是一个，在内存中的存储地址不一样。所以不会变。
请楼主鉴别... 9楼 [Wuerselen](http://blog.csdn.net/pandoraliu) 2013-08-12 19:27发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/pandoraliu)不错的东东，Mark一下，有些东西就省了自己写了。 8楼 [Allen-PengYe](http://blog.csdn.net/u010259250) 2013-08-12 09:20发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/u010259250)thx，对刚学java的我，太有作用了 7楼 [kaijiemn](http://blog.csdn.net/kaijiemn) 2013-08-11 23:35发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/kaijiemn)shadow-->shallow 6楼 [水哥709](http://blog.csdn.net/leon709) 2013-08-11 21:21发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/leon709)java.util.Arrays这个类中提供了一些计算数组的hashcode方法，可以直接使用。 Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-11 21:51发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复leon709：考得就是你自己写程序得能力，这是判别高级程序员和普通程序员得标识，在国内高级程序员太好当了 5楼 [asking1233](http://blog.csdn.net/asking1233) 2013-08-10 10:14发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/asking1233)分析的很不错 学习了 4楼 [jeallan](http://blog.csdn.net/jeallan) 2013-08-09 09:11发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/jeallan)当改写对象的equals()的时候，如果该对象应用到字典的键值，总是要改写hashCode()。 3楼 [canghai21](http://blog.csdn.net/canghai21) 2013-08-05 15:22发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/canghai21)"如果自定义的对象重写了hashCode方法，有可能hashcode相等，equals却不一定相等，“==”比较也不一定相等"
复写hasCode的目的不就是为了保证对象的唯一性，为什么还会有hashCode相等而equals不等的情况?是不是即便给了那个素数，还是会有计算出相同hashCode的情况？ Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-05 15:46发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复canghai21：你说了对，那个素数是为了减少HASHCODE重复的机率而己。
但不能排除hashcode一致equals不一致的情况。 2楼 [鹳狸媛](http://blog.csdn.net/suannai0314) 2013-08-05 14:06发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/suannai0314)您的文章已被推荐到CSDN首页，感谢您的分享。 1楼 [liuswen2722](http://blog.csdn.net/u011604033) 2013-08-05 11:58发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/u011604033)路过，看看。

您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Flifetragedy%2Farticle%2Fdetails%2F9751079)
* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()

[![TOP]()]( "回到顶部")
