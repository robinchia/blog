---
layout: post
title: "RadixSort"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# RadixSort

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 基数排序法]()

## 说明

在之前所介绍过的排序方法，都是属于“比较性”的排序法，也就是每次排序时 ，都是比较整个键值的大小以进行排序。
这边所要介绍的“基数排序法”（radix sort）则是属于“分配式排序”（distribution sort），基数排序法又称“桶子法”（bucket sort）或bin sort，顾名思义，它是透过键值的部份资讯，将要排序的元素分配至某些“桶”中，藉以达到排序的作用，基数排序法是属于稳定性的排序，其时间复杂度为O (nlog(r)m)，其中r为所采取的基数，而m为堆数，在某些时候，基数排序法的效率高于其它的比较性排序法。

## 解法

基数排序的方式可以采用LSD（Least sgnificant digital）或MSD（Most sgnificant digital），LSD的排序方式由键值的最右边开始，而MSD则相反，由键值的最左边开始。
以LSD为例，假设原来有一串数值如下所示：
73, 22, 93, 43, 55, 14, 28, 65, 39, 81
首先根据个位数的数值，在走访数值时将它们分配至编号0到9的桶子中：
0 1 2 3 4 5 6 7 8 9 81    65    39 43 14 55   28 93 22 73
接下来将这些桶子中的数值重新串接起来，成为以下的数列：
******

81, 22, 73, 93, 43, 14, 55, 65, 28, 39
接着再进行一次分配，这次是根据十位数来分配：
0 1 2 3 4 5 6 7 8 9 28 39 14 22  43 55 65 73 81 93
接下来将这些桶子中的数值重新串接起来，成为以下的数列：
******

14, 22, 28, 39, 43, 55, 65, 73, 81, 93
这时候整个数列已经排序完毕；如果排序的对象有三位数以上，则持续进行以上的动作直至最高位数为止。
LSD的基数排序适用于位数小的数列，如果位数多的话，使用MSD的效率会比较好，MSD的方式恰与LSD相反，是由高位数为基底开始进行分配，其他的演 算方式则都相同。

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
int main(void) {
int data[10] = {73, 22, 93, 43, 55, 14, 28, 65, 39, 81};
int temp[10][10] = {0};
int order[10] = {0};
int i, j, k, n, lsd;
k = 0;
n = 1;
printf("\n排序前: ");
for(i = 0; i < 10; i++)
printf("%d ", data[i]);
putchar('\n');
while(n <= 10) {
for(i = 0; i < 10; i++) {
lsd = ((data[i] / n) % 10);
temp[lsd][order[lsd]] = data[i];
order[lsd]++;
}
printf("\n重新排列: ");
for(i = 0; i < 10; i++) {
if(order[i] != 0)
for(j = 0; j < order[i]; j++) {
data[k] = temp[i][j];
printf("%d ", data[k]);
k++;
}
order[i] = 0;
}
n *= 10;
k = 0;
}
putchar('\n');
printf("\n排序后: ");
for(i = 0; i < 10; i++)
printf("%d ", data[i]);
return 0;
}

* Java
public class RadixSort {
public static void sort(int[] number, int d) {
int k = 0;
int n = 1;
int[][] temp = new int[number.length][number.length];
int[] order = new int[number.length];
while(n <= d) {
for(int i = 0; i < number.length; i++) {
int lsd = ((number[i] / n) % 10);
temp[lsd][order[lsd]] = number[i];
order[lsd]++;
}
for(int i = 0; i < number.length; i++) {
if(order[i] != 0)
for(int j = 0; j < order[i]; j++) {
number[k] = temp[i][j];
k++;
}
order[i] = 0;
}
n *= 10;
k = 0;
}
}
public static void main(String[] args) {
int[] data =
{73, 22, 93, 43, 55, 14, 28, 65, 39, 81, 33, 100};
RadixSort.sort(data, 100);
for(int i = 0; i < data.length; i++) {
System.out.print(data[i] + " ");
}
}
}
