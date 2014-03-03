---
layout: post
title: "think in java interview-高级开发人员面试宝典(四)"
categories: 面试参考
tags: 
 - 面试参考
 - think in java interview
--- 

# think in java interview-高级开发人员面试宝典(四)

### [[置顶] think in java interview-高级开发人员面试宝典(四)]()

分类： [面经](http://blog.csdn.net/lifetragedy/article/category/1544093) 2013-08-07 11:00 1772人阅读 [评论]()(16) [收藏]( "收藏") [举报]( "举报")
目录[(?)]( "系统根据文章中H1到H6标签自动生成文章目录")[[+]]( "展开")

1. [算出num个数内的质数]()
1. []()
1. [约瑟夫环算法]()
1. []()
1. [求Missed Number找丢失的那个数]()
1. []()
1. [硬币组合问题]()
1. []()
1. [在List内去除重复值的问题]()
1. []()
1. [一道题即考了冒泡又考了二分]()
1. []()
1. [Collection 和 Collections的区别]()
1. []()
1. [什么时候用assert]()
1. []()
1. [企业级开发写多了忘了JAVA是一门计算机语言计算机语言最早是用来做运算的]()

1. [问Mathround115等於多少 Mathround-115等於多少]()
1. [问short s1 1 s1 s1 1有什么错 short s1 1 s1 1有什么错]()
1. []()
1. [数组有没有length这个方法 String有没有length这个方法]()
1. []()
1. [error和exception有什么区别]()
1. []()
1. [abstract的method是否可同时是static是否可同时是native是否可同时是synchronized]()
1. []()
1. [可变类与不可变类的区别]()

1. [所谓不可变类]()
1. [不可变类]()
1. [如何创建一个可变类]()

# []()算出num个数内的质数

质数即大于1的一个自然数，这个数可以被1和自身整除，如算出20之内的质数，它们有2，3，5，7，11，13，17，19这样的数字。这道题也是面试过程中笔试常问的一道题。

这道题的其目的在于：

1. 看笔试者的数学还记不记得

2. 看笔试者平时的算法

因此答题有两种。

第一种，通用做法

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. public class prime {
1. public static boolean isPrime(int num) {
1. for (int i = 2; i <= Math.sqrt(num); i++) {
1. if (num % i == 0) {
1. return false;
1. }
1. }
1. return true;
1. }
1.
1. public static void main(String[] args) {
1. for (int j = 2; j <= 20; j++) {
1. if (isPrime(j)) {
1. System.out.println(j + " is a prime");
1. }
1. }
1. }
1.
1. }

public class prime {

public static boolean isPrime(int num) {
for (int i = 2; i <= Math.sqrt(num); i++) {

if (num % i == 0) {
return false;

}
}

return true;
}

public static void main(String[] args) {
for (int j = 2; j <= 20; j++) {

if (isPrime(j)) {
System.out.println(j + " is a prime");

}
}

}
}

这种做法是对每个2--num个数内的数，进行扫描，需要用到2个循环，其重复度高，效率低下，因此我们还有一种更先进的做法。

**“筛选"法**

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. public class Prime2 {
1.
1. /**
1. * @param args
1. */
1. public static void main(String[] args) {
1.
1. int n = 20;
1.
1. int[] array = new int[n];
1.
1. for (int i = 2; i < n; i++) {
1.
1. array[i] = i;
1.
1. }
1.
1. for (int i = 2; i < n; i++) {
1. if (array[i] != 0) {
1.
1. int j, temp;
1.
1. temp = array[i];
1.
1. for (j = 2 * temp; j < n; j = j + temp) {
1. array[j] = 0;
1. }
1. System.out.println("\n");
1.
1. }
1.
1. }
1.
1. }
1.
1. }

public class Prime2 {

/**
* @param args

*/
public static void main(String[] args) {

int n = 20;
int[] array = new int[n];

for (int i = 2; i < n; i++) {
array[i] = i;

}
for (int i = 2; i < n; i++) {

if (array[i] != 0) {
int j, temp;

temp = array[i];
for (j = 2 * temp; j < n; j = j + temp) {

array[j] = 0;
}

System.out.println("\n");
}

}
}

}

它的原理就是把所有的偶数去掉后即留下质数。

# []()

# []()约瑟夫环算法

假设有20个人手拉手围成一圈，顺时针开始报号，报到3的人出圈，然后继续往后报，报到3的人出圈，依次把所有报到3的人都踢出圈，最后剩下一人也踢出圈，问先后被踢出圈的那些人原来是圈内的几号？

不难不难，关键记住这个环的算法（有人看到这个”环“字，要笑了，打住，别想歪了，俗人！）

环的算法即闭合算法，永远依次1，2，3，4，5...20...1再2,3,4,5...20这样一直转下去，假设要让20以内的20个数以环型转起来，它的核心算法如下：

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. int flag=0;
1. while(true){
1. flag=(flag+1)%n;
1. }

int flag=0;

while(true){
flag=(flag+1)%n;

}

即”取模运算“，循环继续。

有了核心算法，我们的问题就可以解决了，来：

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. public class JosephCircle {
1.
1. /**
1. * @param args
1. */
1. public void josephCircle(int n,int k){
1. int flag=0;
1. boolean[] kick = new boolean[n];
1. //set kick flag to False;
1. for(int i=0;i<n-1;i++){
1. kick[flag]=false;
1. }
1. int counter=0;
1. int accumulate=0;
1. while(true){
1. if(!kick[flag]){
1. accumulate++;
1. if(counter==n-1){
1. System.out.println("kick last person===="+(flag+1));
1. break;
1. }
1. if(accumulate==k){
1. kick[flag]=true;
1. System.out.println("kick person===="+(flag+1));
1. accumulate=0;
1. counter++;
1. }
1. }
1. flag=(flag+1)%n;
1. }
1.
1. }
1. public static void main(String[] args) {
1. JosephCircle jCircle = new JosephCircle();
1. jCircle.josephCircle(20, 3);
1.
1. }
1.
1. }

public class JosephCircle {

/**
* @param args

*/
public void josephCircle(int n,int k){

int flag=0;
boolean[] kick = new boolean[n];

//set kick flag to False;
for(int i=0;i<n-1;i++){

kick[flag]=false;
}

int counter=0;
int accumulate=0;

while(true){
if(!kick[flag]){

accumulate++;
if(counter==n-1){

System.out.println("kick last person===="+(flag+1));
break;

}
if(accumulate==k){

kick[flag]=true;
System.out.println("kick person===="+(flag+1));

accumulate=0;
counter++;

}
}

flag=(flag+1)%n;
}

}
public static void main(String[] args) {

JosephCircle jCircle = new JosephCircle();
jCircle.josephCircle(20, 3);

}
}

# []()

# []()求Missed Number，找丢失的那个数

上次有朋友在美国去yahoo面试，说一道题把它给整了，题目如下：

说有一数组如 int[]{0,1,2} 这样的一个数组，这个数组的第一个必须从0开始，以次+1列出，该数组内最后一个数是这个数组的长度，因此：

int[]{1,2}, missed number为0

int[]{0,1,2}, missed number为3

int[]{0,2}, missed number为1

我朋友和我说这看上去有点像等差数列，我当时就在MSN里和他说：等差数你个头啊！

然后我朋友说，他用了两个循环嵌套也搞不定

我和他说：两个循环你个头啊

他在MSN上又要和我说什么，我还没等他把它打出来直接就是”你个头啊“回过去了。

大家看，这个找missed number是很好玩的一个东西，如果你想着用什么循环，什么算法，什么数据结构，我一律在这边回”你个头啊“，为什么，这么简单的东西，直接套公式啊，唉。。。

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. public class MissedNumber {
1.
1. public int findMissedOne(int[] numArray) {
1. int sum = 0;
1. int idx = -1;
1. for (int i = 0; i < numArray.length; i++) {
1. if (numArray[i] == 0) {
1. idx = i;
1. } else {
1. sum += numArray[i];
1. }
1. }
1.
1. // the total sum of numbers between 1 and arr.length.
1. int total = (numArray.length + 1) * numArray.length / 2;
1. int missedNumber = total - sum;
1. return missedNumber;
1.
1. }
1. }

public class MissedNumber {

public int findMissedOne(int[] numArray) {
int sum = 0;

int idx = -1;
for (int i = 0; i < numArray.length; i++) {

if (numArray[i] == 0) {
idx = i;

} else {
sum += numArray[i];

}
}

// the total sum of numbers between 1 and arr.length.
int total = (numArray.length + 1) * numArray.length / 2;

int missedNumber = total - sum;
return missedNumber;

}
}

来个测试类

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. public class TestMissedNumber {
1.
1. protected MissedNumber mNum = new MissedNumber();
1.
1. @Test
1. public void testFindMissedOne() {
1. int[] testArray = new int[] {0,2};
1. int missedNumber = mNum.findMissedOne(testArray);
1. System.out.println(missedNumber);
1. }
1.
1. }

public class TestMissedNumber {

protected MissedNumber mNum = new MissedNumber();
@Test

public void testFindMissedOne() {
int[] testArray = new int[] {0,2};

int missedNumber = mNum.findMissedOne(testArray);
System.out.println(missedNumber);

}
}

简单吧，数组内的数的总和减去（（数组长度+1）*数组长度/2），即为missed number

所以你就循环求一个数组内各数的和即可鸟。

# []()

# []()硬币组合问题

这道题老套了，即2角，5角，1角硬币，问有多少种组合可得到1块钱（一块钱？一块钱以前能够买奶油雪糕，一块钱以前可以买一个铅笔盒，一块钱以前可以吃到大排面，现在能干吗？）

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. public class CentsCombinations {
1. public static void main(String[] args) {
1. int i, j, k, total;
1. System.out.println("1 cent"+" 2 cents"+" 5 cents");
1. for (i = 0; i <= 10; i++)
1. for (j = 0; j <= 5; j++)
1. for (k = 0; k <= 2; k++) {
1. total = i * 1 + j * 2 + k * 5;
1. if (total > 10)
1. break;
1. if (10 == total)
1. System.out.println(" " + i + ", " + j + ", " + k);
1. }
1.
1. }
1. }

public class CentsCombinations {

public static void main(String[] args) {
int i, j, k, total;

System.out.println("1 cent"+" 2 cents"+" 5 cents");
for (i = 0; i <= 10; i++)

for (j = 0; j <= 5; j++)
for (k = 0; k <= 2; k++) {

total = i * 1 + j * 2 + k * 5;
if (total > 10)

break;
if (10 == total)

System.out.println(" " + i + ", " + j + ", " + k);
}

}
}

# []()

# []()在List内去除重复值的问题

说有一个ArrayList{1,2,3,5,1,2,7,5,3,3,4,5,6,9,11,11}，它里面有许多重复数，我要求去除所有的重复数据，得到一个不重复的List。

有的人碰到这个问题，循环2个，嵌套，还有用二分算法的（这种人已经算好的了）。

要什么循环啊？

这道题考的是你对JDK的API是否熟悉，来看下面答案，如果以前没碰到过这个问题的人看完下面的答案后直接就吐血了

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. public class DuplicateValue {
1.
1. public void removeDuplicateValue() {
1. List myList = new ArrayList();
1. myList.add(1);
1. myList.add(2);
1. myList.add(1);
1. myList.add(3);
1. myList.add(4);
1. myList.add(5);
1. myList.add(6);
1. myList.add(5);
1.
1. Set myset = new HashSet(myList);
1. myList = new ArrayList(myset);
1. Iterator it = myList.iterator();
1. while (it.hasNext()) {
1. System.out.println(""+it.next());
1. }
1. }
1.
1. public static void main(String[] args) {
1. DuplicateValue dv = new DuplicateValue();
1. dv.removeDuplicateValue();
1.
1. }
1.
1. }

public class DuplicateValue {

public void removeDuplicateValue() {
List myList = new ArrayList();

myList.add(1);
myList.add(2);

myList.add(1);
myList.add(3);

myList.add(4);
myList.add(5);

myList.add(6);
myList.add(5);

Set myset = new HashSet(myList);
myList = new ArrayList(myset);

Iterator it = myList.iterator();
while (it.hasNext()) {

System.out.println(""+it.next());
}

}
public static void main(String[] args) {

DuplicateValue dv = new DuplicateValue();
dv.removeDuplicateValue();

}
}

# []()

# []()一道题即考了冒泡又考了二分

说有一数组int[]{1,2,2,4,6,8,9,5,3,7,5}，里面可能还有很多，想要知道”两两相加等于10“的这样的数有几对，可以重复如：2+6，6+2

上手就是冒泡

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. public Vector<SumPossibleBean> checkSumPossible(ArrayList<Integer> intList,
1. int sumResult) {
1. Vector<SumPossibleBean> possibleList = new Vector<SumPossibleBean>();
1. int size = intList.size();
1. for (int i = 0; i < size; i++) {
1. for (int j = i + 1; j < size; j++) {
1. if ((intList.get(i).intValue() + intList.get(j).intValue()) == sumResult) {
1.
1. SumPossibleBean spBean = new SumPossibleBean();
1. spBean.setFirst(intList.get(i).intValue());
1. spBean.setSecond(intList.get(j).intValue());
1. possibleList.addElement(spBean);
1. }
1. }
1. }
1. return possibleList;
1. }

public Vector<SumPossibleBean> checkSumPossible(ArrayList<Integer> intList,

int sumResult) {
Vector<SumPossibleBean> possibleList = new Vector<SumPossibleBean>();

int size = intList.size();
for (int i = 0; i < size; i++) {

for (int j = i + 1; j < size; j++) {
if ((intList.get(i).intValue() + intList.get(j).intValue()) == sumResult) {

SumPossibleBean spBean = new SumPossibleBean();
spBean.setFirst(intList.get(i).intValue());

spBean.setSecond(intList.get(j).intValue());
possibleList.addElement(spBean);

}
}

}
return possibleList;

}

不错，两个循环嵌套，当考官问你，有没有更好的算法时，70%以上的人基本歇菜！！！

唉哎。。。二分来一个吗，以前在学校怎么学的？

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. public Vector<SumPossibleBean> checkSumPossibleBinSearch(
1. ArrayList<Integer> intList, int sumResult) {
1. Vector<SumPossibleBean> possibleList = new Vector<SumPossibleBean>();
1. int size = intList.size();
1. Collections.sort(intList);
1. for (int i = 0, j = (intList.size() - 1); i < j;) {
1. Integer s = intList.get(i);
1. Integer e = intList.get(j);
1. if ((s + e) == sumResult) {
1. SumPossibleBean spBean = new SumPossibleBean();
1. spBean.setFirst(intList.get(i).intValue());
1. spBean.setSecond(intList.get(j).intValue());
1. possibleList.addElement(spBean);
1. i++;
1. j--;
1. } else if ((s + e) > sumResult) {
1. j--;
1. } else if ((s + e) < sumResult) {
1. i++;
1. }
1. }
1. return possibleList;
1. }

public Vector<SumPossibleBean> checkSumPossibleBinSearch(

ArrayList<Integer> intList, int sumResult) {
Vector<SumPossibleBean> possibleList = new Vector<SumPossibleBean>();

int size = intList.size();
Collections.sort(intList);

for (int i = 0, j = (intList.size() - 1); i < j;) {
Integer s = intList.get(i);

Integer e = intList.get(j);
if ((s + e) == sumResult) {

SumPossibleBean spBean = new SumPossibleBean();
spBean.setFirst(intList.get(i).intValue());

spBean.setSecond(intList.get(j).intValue());
possibleList.addElement(spBean);

i++;
j--;

} else if ((s + e) > sumResult) {
j--;

} else if ((s + e) < sumResult) {
i++;

}
}

return possibleList;
}

算法讲一些，讲多了枯燥，来几道问答题，如果你没准备过的话也一样可以让你爽到吐血。

# []()

# []()Collection 和 Collections的区别

Collections是个java.util下的类，它包含有各种有关集合操作的静态方法。
Collection是个java.util下的接口，它是各种集合结构的父接口。
吐血了吗？没吐，OK，下面这道如果在面试时问到一定让你吐，面架构师的人50%以上被问到过这道题

# []()

# []()什么时候用assert

断言是一个包含布尔表达式的语句，在执行这个语句时假定该表达式为 true。如果表达式计算为 false，那么系统会报告一个 AssertionError。它用于调试目的：
assert(a > 0); // throws an AssertionError if a <= 0

断言可以有两种形式：
assert Expression1 ;
assert Expression1 : Expression2 ;
Expression1 应该总是产生一个布尔值。
Expression2 可以是得出一个值的任意表达式。这个值用于生成显示更多调试信息的 String 消息。
断言在默认情况下是禁用的。要在编译时启用断言，需要使用 source 1.4 标记：
javac -source 1.4 Test.java
要在运行时启用断言，可使用 -enableassertions 或者 -ea 标记。
要在运行时选择禁用断言，可使用 -da 或者 -disableassertions 标记。
要系统类中启用断言，可使用 -esa 或者 -dsa 标记。还可以在包的基础上启用或者禁用断言。

可以在预计正常情况下不会到达的任何位置上放置断言。断言可以用于验证传递给私有方法的参数。不过，断言不应该用于验证传递给公有方法的参数，因为不管是否启用了断言，公有方法都必须检查其参数。不过，既可以在公有方法中，也可以在非公有方法中利用断言测试后置条件。另外，断言不应该以任何方式改变程序的状态。

# []()

# []()企业级开发写多了，忘了JAVA是一门计算机语言，计算机语言最早是用来做运算的

来一道

## []()问：Math.round(11.5)等於多少? Math.round(-11.5)等於多少

Math.round(11.5)返回（long）12

Math.round(-11.5)返回（long）-11;

再来一道

## []()问：short s1 = 1; s1 = s1+ 1;有什么错? short s1 = 1; s1 += 1;有什么错

short s1 = 1; s1 = s1 + 1;有错，s1是short型，s1+1是int型,不能显式转化为short型。可修改为s1 =(short)(s1 + 1) 。short s1 = 1; s1 += 1正确。

# []()

# []()数组有没有length()这个方法? String有没有length()这个方法

数组没有length()这个方法，有length的属性。

String有有length()这个方法。

# []()

# []()error和exception有什么区别?

error 表示恢复不是不可能但很困难的情况下的一种严重问题。比如说内存溢出。不可能指望程序能处理这样的情况。

exception 表示一种设计或实现问题。也就是说，它表示如果程序运行正常，从不会发生的情况。

# []()

# []()abstract的method是否可同时是static,是否可同时是native，是否可同时是synchronized?

都不能

# []()

# []()可变类与不可变类的区别

## []()所谓不可变类：

是指当创建了这个类的实例后，就不允许修改它的属性值。在JDK的基本类库中，所有基本类型的包装类，如Integer和Long类，都是不可变类，java.lang.String也是不可变类。

## []()不可变类：

当你获得这个类的一个实例引用时，你不可以改变这个实例的内容。不可变类的实例一但创建，其内在成员变量的值就不能被修改。

## []()如何创建一个可变类？

这道题90%以上的人都会被挂或者只回答一半对，来看：

### 1. 所有成员都是private
2. 不提供对成员的改变方法，例如：setXXXX
3. 确保所有的方法不会被重载。手段有两种：使用final Class(强不可变类)，或者将所有类方法加上final(弱不可变类)。
4. 如果某一个类成员不是原始变量(primitive)或者不可变类，必须通过在成员初始化(in)或者get方法(out)时通过深度clone方法，来确保类的不可变。

四点中少一点，回答失败，来看一个例子：

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. public final class MyImmutableWrong {
1. private final int[] myArray;
1.
1.
1. public MyImmutableWrong(int[] anArray) {
1. this.myArray = anArray; // wrong
1. }
1.
1.
1. public String toString() {
1. StringBuffer sb = new StringBuffer("Numbers are: ");
1. for (int i = 0; i < myArray.length; i++) {
1. sb.append(myArray[i] + " ");
1. }
1. return sb.toString();
1. }
1. }

public final class MyImmutableWrong {

private final int[] myArray;
public MyImmutableWrong(int[] anArray) {

this.myArray = anArray; // wrong
}

public String toString() {
StringBuffer sb = new StringBuffer("Numbers are: ");

for (int i = 0; i < myArray.length; i++) {
sb.append(myArray[i] + " ");

}
return sb.toString();

}
}

以上可变类书写错误，为什么？违犯了第四点

正确的写法：

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. public final class MyImmutableCorrect {
1. private final int[] myArray;
1.
1. public MyImmutableCorrect(int[] anArray) {
1. this.myArray = anArray.clone();
1. }
1.
1. public String toString() {
1. StringBuffer sb = new StringBuffer("Numbers are: ");
1. for (int i = 0; i < myArray.length; i++) {
1. sb.append(myArray[i] + " ");
1. }
1. return sb.toString();
1. }
1. }

public final class MyImmutableCorrect {

private final int[] myArray;
public MyImmutableCorrect(int[] anArray) {

this.myArray = anArray.clone();
}

public String toString() {
StringBuffer sb = new StringBuffer("Numbers are: ");

for (int i = 0; i < myArray.length; i++) {
sb.append(myArray[i] + " ");

}
return sb.toString();

}
}

记住，不可变类的写法，我说过90%的人碰到这个问题会挂，另10%回答出。

但 是这10%里90%的人只回答出第1点，第2点和第3点，对于第四点即：

**如果某一个类成员不是原始变量(primitive)或者不可变类，必须通过在成员初始化(in)或者get方法(out)时通过深度clone方法，来确保类的不可变。**

则都没回答上来。

**由此可判断这个面试者是真正写过还是只是为了应付面试而背原理**。

所以，不要把面试当儿戏，每一次面试可能就是对自己这段时间的一个检查，看看自己平时缺了什么，JAVA是一门体系化的语言，要想面试的好，不只是死记硬背，是真的平时要化时间自己去补一些基础的。

**上手就学SSH，JSP能连个数据库满天飞，这就是JAVA了？这就是中国IT了。。。这是错误的**。

今天暂写这么多，下次继续。
分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")

1. 上一篇：[think in java interview-高级开发人员面试宝典(三)](http://blog.csdn.net/lifetragedy/article/details/9766087)
1. 下一篇：[think in java interview-高级开发人员面试宝典(五)](http://blog.csdn.net/lifetragedy/article/details/9878839)
顶8 踩0

查看评论[]()
9楼 [Fly_fans](http://blog.csdn.net/Fly_fans) 6天前 14:56发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/Fly_fans)第一题的筛选法，去掉偶数，为什么不用
for(int i = 1 ; i < n ; i += 2){} ? 8楼 [wenzhibinbin_pt](http://blog.csdn.net/wenzhibinbin_pt) 2013-08-15 23:01发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/wenzhibinbin_pt)1
数组内的数的总和减去（（数组长度+1）*数组长度/2）？？？
应该是反过来说吧。
2
for(int i=0;i<n-1;i++){
kick[flag]=false;
}
代码运行没影响，默认为false，但严谨应该是 kick[i]=false; Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-16 09:33发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复wenzhibinbin_pt：关于问题1我建议你运行程序看看结果，如果你觉得反过来，是什么结果呢？呵呵 Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-16 09:31发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复wenzhibinbin_pt：就这段代码我没用ECLIPSE写是用NOTEPAD写的，为了急着出，才出此错误，我错了，呵呵，因该是kick[i]=false; 7楼 [rain082900](http://blog.csdn.net/rain082900) 2013-08-13 18:02发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/rain082900)用二分的那个，j的初始值可以先判断下，题目没说所有的元素都小于10。 6楼 [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-11 21:46发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)最后一个数为该数组长度！这是题目要求，所以缺失得这个数为3对吧？该题难就难在数组即使全了最后一个数总是missed得，其目的是诱导你思考该题得思路方向 5楼 [水哥709](http://blog.csdn.net/leon709) 2013-08-11 21:38发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/leon709)有个疑问，int[]{0,1,2}, missed number为3，这个数组最后一个数不是该数组的长度？ Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-11 21:48发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复leon709：如果是013那missed number为多少？嘿嘿，很好玩得一道题 Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-11 21:42发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复leon709：数组内有012三个元素，该数组长度多少？ 4楼 [百斯特数据](http://blog.csdn.net/zzw222222) 2013-08-11 20:00发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/zzw222222)呵呵，比较实用，看看。 3楼 [canghai21](http://blog.csdn.net/canghai21) 2013-08-10 23:45发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/canghai21)这里是Lobs LOL Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-11 14:17发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复canghai21：祝工作顺利，前程似锦 Re: [canghai21](http://blog.csdn.net/canghai21) 2013-08-11 19:45发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/canghai21)回复lifetragedy：前程似锦不敢
前段时间项目要玩EXTJS，完了一个半月，项目报废了，现在要回到一个C写的CS结构的客户端。和一个基于JAVA的手提终端，好无奈。我在想是继续呆着，还是再找新的项目。 2楼 [andy_713](http://blog.csdn.net/wyx713510713) 2013-08-09 21:45发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/wyx713510713)继续追随楼主 1楼 [canghai21](http://blog.csdn.net/canghai21) 2013-08-09 13:59发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/canghai21)二分算法在运算前用了colleaction.sort() 如果把这个也考虑到效率里的话算不算作弊啊
好多东西以前ELT里都讲过 Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 2013-08-09 21:19发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复canghai21：CTS ELT?

您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Flifetragedy%2Farticle%2Fdetails%2F9812419)
* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()

[![TOP]()]( "回到顶部")
