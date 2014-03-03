---
layout: post
title: "ShakerSort"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# ShakerSort

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: Shaker 排序法 - 改良的气泡排序]()

## 说明

请看看之前介绍过的气泡排序法：
     for(i = 0; i < MAX-1 && flag == 1; i++) {
        flag = 0;
        for(j = 0; j < MAX-i-1; j++) {
            if(number[j+1] < number[j]) {
                SWAP(number[j+1], number[j]);
                flag = 1;
            }
        }
    }
  
事实上这个气泡排序法已经不是单纯的气泡排序了，它使用了旗标与右端左移两个方法来改进排序的效能，而Shaker排序法使用到后面这个观念进一步改良气泡排序法。

## 解法

在上面的气泡排序法中，交换的动作并不会一直进行至阵列的最后一个，而是会进行至MAX-i-1，所以排序的过程中，阵列右方排序好的元素会一直增加，使得左边排序的次数逐渐减少，如我们的例子所示：
排序前：95 27 90 49 80 58 6 9 18 50

1. 27 90 49 80 58 6 9 18 50 [95] 95浮出
1. 27 49 80 58 6 9 18 50 [90 95] 90浮出
1. 27 49 58 6 9 18 50 [80 90 95] 80浮出
1. 27 49 6 9 18 50 [58 80 90 95] ......
1. 27 6 9 18 49 [50 58 80 90 95] ......
1. 6 9 18 27 [49 50 58 80 90 95] ......
1. 6 9 18 [27 49 50 58 80 90 95]
方括号括住的部份表示已排序完毕，Shaker排序使用了这个概念，如果让左边的元素也具有这样的性质，让左右两边的元素都能先排序完成，如此未排序的元素会集中在中间，由于左右两边同时排序，中间未排序的部份将会很快的减少。
方法就在于气泡排序的双向进行，先让气泡排序由左向右进行，再来让气泡排序由右往左进行，如此完成一次排序的动作，而您必须使用left与right两个旗标来记录左右两端已排序的元素位置。
一个排序的例子如下所示：
排序前：45 19 77 81 13 28 18 19 77 11
往右排序：19 45 77 13 28 18 19 77 11 [81]
向左排序：[11] 19 45 77 13 28 18 19 77 [81]
往右排序：[11] 19 45 13 28 18 19 [77 77 81]
向左排序：[11 13] 19 45 18 28 19 [77 77 81]
往右排序：[11 13] 19 18 28 19 [45 77 77 81]
向左排序：[11 13 18] 19 19 28 [45 77 77 81]
往右排序：[11 13 18] 19 19 [28 45 77 77 81]
向左排序：[11 13 18 19 19] [28 45 77 77 81]
如上所示，括号中表示左右两边已排序完成的部份，当left > right时，则排序完成。

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#define MAX 10
#define SWAP(x,y) {int t; t = x; x = y; y = t;}
void shakersort(int[]);
int main(void) {
int number[MAX] = {0};
int i;
srand(time(NULL));
printf("排序前：");
for(i = 0; i < MAX; i++) {
number[i] = rand() % 100;
printf("%d ", number[i]);
}
shakersort(number);
printf("\n");
return 0;
}
void shakersort(int number[]) {
int i, left = 0, right = MAX - 1, shift = 0;
while(left < right) {
// 向右进行气泡排序
for(i = left; i < right; i++) {
if(number[i] > number[i+1]) {
SWAP(number[i], number[i+1]);
shift = i;
}
}
right = shift;
printf("\n往右排序：");
for(i = 0; i < MAX; i++)
printf("%d ", number[i]);
// 向左进行气泡排序
for(i = right; i > left; i--) {
if(number[i] < number[i-1]) {
SWAP(number[i], number[i-1]);
shift = i;
}
}
left = shift;
printf("\n向左排序：");
for(i = 0; i < MAX; i++)
printf("%d ", number[i]);
}
}

* Java
public class ShakerSort {
public static void sort(int[] number) {
int i, left = 0,
right = number.length - 1,
shift = 0;
while(left < right) {
// 向右进行气泡排序
for(i = left; i < right; i++) {
if(number[i] > number[i+1]) {
swap(number, i, i+1);
shift = i;
}
}
right = shift;
// 向左进行气泡排序
for(i = right; i > left; i--) {
if(number[i] < number[i-1]) {
swap(number, i ,i-1);
shift = i;
}
}
left = shift;
}
}
private static void swap(int[] number, int i, int j) {
int t;
t = number[i];
number[i] = number[j];
number[j] = t;
}
}
