---
layout: post
title: "ShellSort"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# ShellSort

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: Shell 排序法 - 改良的插入排序]()

## 说明

插入排序法由未排序的后半部前端取出一个值，插入已排序前半部的适当位置，概念简单但速度不快。
排序要加快的基本原则之一，是让后一次的排序进行时，尽量利用前一次排序后的结果，以加快排序的速度，Shell排序法即是基于此一概念来改良插入排序法。

## 解法

Shell排序法最初是D.L Shell于1959所提出，假设要排序的元素有n个，则每次进行插入排序时并不是所有的元素同时进行时，而是取一段间隔。
Shell首先将间隔设定为n/2，然后跳跃进行插入排序，再来将间隔n/4，跳跃进行排序动作，再来间隔设定为n/8、n/16，直到间隔为1之后的最 后一次排序终止，由于上一次的排序动作都会将固定间隔内的元素排序好，所以当间隔越来越小时，某些元素位于正确位置的机率越高，因此最后几次的排序动作将 可以大幅减低。
举个例子来说，假设有一未排序的数字如右：89 12 65 97 61 81 27 2 61 98
数字的总数共有10个，所以第一次我们将间隔设定为10 / 2 = 5，此时我们对间隔为5的数字进行排序，如下所示：
![Shell 排序]( "Shell 排序")
画线连结的部份表示 要一起进行排序的部份，再来将间隔设定为5 / 2的商，也就是2，则第二次的插入排序对象如下所示：
![Shell 排序]( "Shell 排序")
再来间隔设定为2 / 2 = 1，此时就是单纯的插入排序了，由于大部份的元素都已大致排序过了，所以最后一次的插入排序几乎没作什么排序动作了：
![Shell 排序]( "Shell 排序")
 将间隔设定为n / 2是D.L Shell最初所提出，在教科书中使用这个间隔比较好说明，然而Shell排序法的关键在于间隔的选定，例如Sedgewick证明选用以下的间隔可以加 快Shell排序法的速度：
![Shell 排序]( "Shell 排序")
 其中4*(2j)2 + 3*(2j) + 1不可超过元素总数n值，使用上式找出j后代入4*(2j)2 + 3*(2j) + 1求得第一个间隔，然后将2j除以2代入求得第二个间隔，再来依此类推。
后来还有人证明有其它的间隔选定法可以将Shell排序法的速度再加快；另外Shell排序法的概念也可以用来改良气泡排序法。

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#define MAX 10
#define SWAP(x,y) {int t; t = x; x = y; y = t;}
void shellsort(int[]);
int main(void) {
int number[MAX] = {0};
int i;
srand(time(NULL));
printf("排序前：");
for(i = 0; i < MAX; i++) {
number[i] = rand() % 100;
printf("%d ", number[i]);
}
shellsort(number);
return 0;
}
void shellsort(int number[]) {
int i, j, k, gap, t;
gap = MAX / 2;
while(gap > 0) {
for(k = 0; k < gap; k++) {
for(i = k+gap; i < MAX; i+=gap) {
for(j = i - gap; j >= k; j-=gap) {
if(number[j] > number[j+gap]) {
SWAP(number[j], number[j+gap]);
}
else
break;
}
}
}
printf("\ngap = %d：", gap);
for(i = 0; i < MAX; i++)
printf("%d ", number[i]);
printf("\n");
gap /= 2;
}
}

* Java
public class ShellSort {
public static void sort(int[] number) {
int gap = number.length / 2;
while(gap > 0) {
for(int k = 0; k < gap; k++) {
for(int i = k+gap; i < number.length; i+=gap) {
for(int j = i - gap; j >= k; j-=gap) {
if(number[j] > number[j+gap]) {
swap(number, j, j+gap);
}
else
break;
}
}
}
gap /= 2;
}
}
private static void swap(int[] number, int i, int j) {
int t;
t = number[i];
number[i] = number[j];
number[j] = t;
}
}
