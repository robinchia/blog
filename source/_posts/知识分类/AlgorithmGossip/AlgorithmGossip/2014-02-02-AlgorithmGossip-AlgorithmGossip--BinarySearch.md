---
layout: post
title: "BinarySearch"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# BinarySearch

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 二分搜寻法（搜寻原则的代表）]()

## 说明

如果搜寻的数列已经有排序，应该尽量利用它们已排序的特性，以减少搜寻比对的次数，这是搜寻的基本原则，二分搜寻法是这个基本原则的代表。

## 解法

在二分搜寻法中，从数列的中间开始搜寻，如果这个数小于我们所搜寻的数，由于数列已排序，则该数左边的数一定都小于要搜寻的对象，所以无需浪费时间在左边的数；如果搜寻的数大于所搜寻的对象，则右边的数无需再搜寻，直接搜寻左边的数。
所以在二分搜寻法中，将数列不断的分为两个部份，每次从分割的部份中取中间数比对，例如要搜寻92于以下的数列，首先中间数索引为(0+9)/2 = 4（索引由0开始）：
[3 24 57 57 67 68 83 90 92 95]
由于67小于92，所以转搜寻右边的数列：
3 24 57 57 67 [68 83 90 92 95]
由于90小于92，再搜寻右边的数列，这次就找到所要的数了：
3 24 57 57 67 68 83 90 [92 95]

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#define MAX 10
#define SWAP(x,y) {int t; t = x; x = y; y = t;}
void quicksort(int[], int, int);
int bisearch(int[], int);
int main(void) {
int number[MAX] = {0};
int i, find;
srand(time(NULL));
for(i = 0; i < MAX; i++) {
number[i] = rand() % 100;
}
quicksort(number, 0, MAX-1);
printf("数列：");
for(i = 0; i < MAX; i++)
printf("%d ", number[i]);
printf("\n输入寻找对象：");
scanf("%d", &find);
if((i = bisearch(number, find)) >= 0)
printf("找到数字于索引 %d ", i);
else
printf("\n找不到指定数");
printf("\n");
return 0;
}
int bisearch(int number[], int find) {
int low, mid, upper;
low = 0;
upper = MAX - 1;
while(low <= upper) {
mid = (low+upper) / 2;
if(number[mid] < find)
low = mid+1;
else if(number[mid] > find)
upper = mid - 1;
else
return mid;
}
return -1;
}
void quicksort(int number[], int left, int right) {
int i, j, k, s;
if(left < right) {
s = number[(left+right)/2];
i = left - 1;
j = right + 1;
while(1) {
while(number[++i] < s) ; // 向右找
while(number[--j] > s) ; // 向左找
if(i >= j)
break;
SWAP(number[i], number[j]);
}
quicksort(number, left, i-1); // 对左边进行递回
quicksort(number, j+1, right); // 对右边进行递回
}
}

* Java
public class BinarySearch {
public static int search(int[] number, int des) {
int low = 0;
int upper = number.length - 1;
while(low <= upper) {
int mid = (low+upper) / 2;
if(number[mid] < des)
low = mid+1;
else if(number[mid] > des)
upper = mid - 1;
else
return mid;
}
return -1;
}
public static void main(String[] args) {
int[] number = {1, 4, 2, 6, 7, 3, 9, 8};
QuickSort.sort(number);
int find = BinarySearch.search(number, 3);
if(find != -1)
System.out.println("找到数值于索引" + find);
else
System.out.println("找不到数值");
}
}
