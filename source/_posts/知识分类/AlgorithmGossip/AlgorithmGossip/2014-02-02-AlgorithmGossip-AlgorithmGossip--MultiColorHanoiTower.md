---
layout: post
title: "MultiColorHanoiTower"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# MultiColorHanoiTower

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 双色、三色河内塔]()

## 说明

双色河内塔与三色河内塔是由之前所介绍过的河内塔规则衍生而来，双色河内塔的目的是将下图左上的圆环位置经移动成为右下的圆环位置：
![多色河内塔]( "多色河内塔")
而三色河内塔则是将下图左上的圆环经移动成为右上的圆环：
![多色河内塔]( "多色河内塔")
在移动的过程中，一样遵守大盘必须在小盘之下的规则，而颜色顺序则无限制。

## 解法

无论是双色河内塔或是三色河内塔，其解法观念与之前介绍过的河内塔是类似的，同样也是使用递回来解，不过这次递回解法的目的不同，我们先来看只有两个 盘的情况，这很简单，只要将第一柱的黄色移动至第二柱，而接下来第一柱的蓝色移动至第三柱。
再来是四个盘的情况，首先必须用递回完成下图左上至右下的移动：
![多色河内塔]( "多色河内塔")
接下来最底层的就不用管它们了，因为它们已经就定位，只要再处理第一柱的上面两个盘子就可以了。
那么六个盘的情况呢？一样！首先必须用递回完成下图左上至右下的移动：
![多色河内塔]( "多色河内塔")
接下来最底层的就不用管它们了，因为它们已经就定位，只要再处理第一柱上面的四个盘子就可以了，这又与之前只有四盘的情况相同，接下来您就知道该如何进行解题了，无论是八个盘、十个盘以上等，都是用这个观念来解题。
那么三色河内塔呢？一样，直接来看九个盘的情况，首先必须完成下图的移动结果：
![多色河内塔]( "多色河内塔")
接下来最底两层的就不用管它们了，因为它们已经就定位，只要再处理第一柱上面的三个盘子就可以了。
![多色河内塔]( "多色河内塔")
您也可以看看 [Towers of Hanoi Page](http://obelix.ee.duth.gr/%7Eapostolo/TowersOfHanoi/index.html) 中有关于河内塔的讨论。

##  实作

以下Java演算实例由 [http://www.javaworld.com.tw](http://www.javaworld.com.tw) 的 [T5555](http://www.javaworld.com.tw/jute/user/info?uid=3056) 提供，感谢他的协助，我根据他的实例，转换为C语言的实作部份，您可以参考这个 [讨论串](http://www.javaworld.com.tw/jute/post/view?bid=29&id=81268&tpg=1&ppg=1&sty=0&age=0#81268) 有关于河内塔的讨论。

* 双色河内塔 C 实作
#include <stdio.h>
void hanoi(int disks, char source, char temp, char target) {
if (disks == 1) {
printf("move disk from %c to %c\n", source, target);
printf("move disk from %c to %c\n", source, target);
} else {
hanoi(disks-1, source, target, temp);
hanoi(1, source, temp, target);
hanoi(disks-1, temp, source, target);
}
}
void hanoi2colors(int disks) {
char source = 'A';
char temp = 'B';
char target = 'C';
int i;
for(i = disks / 2; i > 1; i--) {
hanoi(i-1, source, temp, target);
printf("move disk from %c to %c\n", source, temp);
printf("move disk from %c to %c\n", source, temp);
hanoi(i-1, target, temp, source);
printf("move disk from %c to %c\n", temp, target);
}
printf("move disk from %c to %c\n", source, temp);
printf("move disk from %c to %c\n", source, target);
}
int main() {
int n;
printf("请输入盘数：");
scanf("%d", &n);
hanoi2colors(n);
return 0;
}

* 三色河内塔 C 实作
#include <stdio.h>
void hanoi(int disks, char source, char temp, char target) {
if (disks == 1) {
printf("move disk from %c to %c\n", source, target);
printf("move disk from %c to %c\n", source, target);
printf("move disk from %c to %c\n", source, target);
} else {
hanoi(disks-1, source, target, temp);
hanoi(1, source, temp, target);
hanoi(disks-1, temp, source, target);
}
}
void hanoi3colors(int disks) {
char source = 'A';
char temp = 'B';
char target = 'C';
int i;
if(disks == 3) {
printf("move disk from %c to %c\n", source, temp);
printf("move disk from %c to %c\n", source, temp);
printf("move disk from %c to %c\n", source, target);
printf("move disk from %c to %c\n", temp, target);
printf("move disk from %c to %c\n", temp, source);
printf("move disk from %c to %c\n", target, temp);;
}
else {
hanoi(disks/3-1, source, temp, target);
printf("move disk from %c to %c\n", source, temp);
printf("move disk from %c to %c\n", source, temp);
printf("move disk from %c to %c\n", source, temp);
hanoi(disks/3-1, target, temp, source);
printf("move disk from %c to %c\n", temp, target);
printf("move disk from %c to %c\n", temp, target);
printf("move disk from %c to %c\n", temp, target);
hanoi(disks/3-1, source, target, temp);
printf("move disk from %c to %c\n", target, source);
printf("move disk from %c to %c\n", target, source);
hanoi(disks/3-1, temp, source, target);
printf("move disk from %c to %c\n", source, temp);
for (i = disks / 3 - 1; i > 0; i--) {
if (i>1) {
hanoi(i-1, target, source, temp);
}
printf("move disk from %c to %c\n",
target, source);
printf("move disk from %c to %c\n",
target, source);
if (i>1) {
hanoi(i-1, temp, source, target);
}
printf("move disk from %c to %c\n",
source, temp);
}
}
}
int main() {
int n;
printf("请输入盘数：");
scanf("%d", &n);
hanoi3colors(n);
return 0;
}

* 双色河内塔 Java 实作
public class Hanoi2Colors {
public static void help() {
System.out.println(
"Usage: java Hanoi2Colors number_of_disks");
System.out.println(
"\t number_of_disks: must be a even number.");
System.exit(0);
}
public static void main(String[] args) {
int disks = 0;
try {
disks = Integer.parseInt(args[0]);
} catch (Exception e) {
help();
}
if ((disks <= 0) || (disks % 2 != 0)) {
help();
}
hanoi2colors(disks);
}
public static void hanoi(int disks,
char source, char temp, char target) {
if (disks == 1) {
System.out.println("move disk from "
+ source + " to " + target);
System.out.println("move disk from "
+ source + " to " + target);
} else {
hanoi(disks-1, source, target, temp);
hanoi(1, source, temp, target);
hanoi(disks-1, temp, source, target);
}
}
public static void hanoi2colors(int disks) {
char source = 'A';
char temp = 'B';
char target = 'C';
for (int i = disks / 2; i > 1; i--) {
hanoi(i-1, source, temp, target);
System.out.println("move disk from "
+ source + " to " + temp);
System.out.println("move disk from "
+ source + " to " + temp);
hanoi(i-1, target, temp, source);
System.out.println("move disk from "
+ temp + " to " + target);
}
System.out.println("move disk from "
+ source + " to " + temp);
System.out.println("move disk from "
+ source + " to " + target);
}
}

* 三色河内塔 Java 实作
public class Hanoi3Colors {
public static void help() {
System.out.println(
"Usage: java Hanoi3Colors number_of_disks");
System.out.println(
"\tnumber_of_disks: must be a number divisible by 3.");
System.exit(0);
}
public static void main(String[] args) {
int disks = 0;
try {
disks = Integer.parseInt(args[0]);
} catch (Exception e) {
help();
}
if ((disks <= 0) || (disks % 3 != 0)) {
help();
}
hanoi3colors(disks);
}
public static void hanoi(int disks,
char source, char temp, char target) {
if (disks == 1) {
System.out.println("move disk from "
+ source + " to " + target);
System.out.println("move disk from "
+ source + " to " + target);
System.out.println("move disk from "
+ source + " to " + target);
} else {
hanoi(disks-1, source, target, temp);
hanoi(1, source, temp, target);
hanoi(disks-1, temp, source, target);
}
}
public static void hanoi3colors(int disks) {
char source = 'A';
char temp = 'B';
char target = 'C';
if (disks == 3) {
System.out.println("move disk from "
+ source + " to " + temp);
System.out.println("move disk from "
+ source + " to " + temp);
System.out.println("move disk from "
+ source + " to " + target);
System.out.println("move disk from "
+ temp + " to " + target);
System.out.println("move disk from "
+ temp + " to " + source);
System.out.println("move disk from "
+ target + " to " + temp);
} else {
hanoi(disks/3-1, source, temp, target);
System.out.println("move disk from "
+ source + " to " + temp);
System.out.println("move disk from "
+ source + " to " + temp);
System.out.println("move disk from "
+ source + " to " + temp);
hanoi(disks/3-1, target, temp, source);
System.out.println("move disk from "
+ temp + " to " + target);
System.out.println("move disk from "
+ temp + " to " + target);
System.out.println("move disk from "
+ temp + " to " + target);
hanoi(disks/3-1, source, target, temp);
System.out.println("move disk from "
+ target + " to " + source);
System.out.println("move disk from "
+ target + " to " + source);
hanoi(disks/3-1, temp, source, target);
System.out.println("move disk from "
+ source + " to " + temp);
for (int i = disks / 3 - 1; i > 0; i--) {
if (i>1) {
hanoi(i-1, target, source, temp);
}
System.out.println("move disk from "
+ target + " to " + source);
System.out.println("move disk from "
+ target + " to " + source);
if (i>1) {
hanoi(i-1, temp, source, target);
}
System.out.println("move disk from "
+ source + " to " + temp);
}
}
}
}
