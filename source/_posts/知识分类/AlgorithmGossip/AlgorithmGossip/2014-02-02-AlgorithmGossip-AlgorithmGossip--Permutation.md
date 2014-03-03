---
layout: post
title: "Permutation"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# Permutation

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 排列组合]()

## 说明

将一组数字、字母或符号进行排列，以得到不同的组合顺序，例如1 2 3这三个数的排列组合有：1 2 3、1 3 2、2 1 3、2 3 1、3 1 2、3 2 1。

## 解法

可以使用递回将问题切割为较小的单元进行排列组合，例如1 2 3 4的排列可以分为1 [2 3 4]、2 [1 3 4]、3 [1 2 4]、4 [1 2 3]进行排列，这边利用旋转法，先将旋转间隔设为0，将最右边的数字旋转至最左边，并逐步增加旋转的间隔，例如：
1 2 3 4 -> 旋转1 -> 继续将右边2 3 4进行递回处理
2 1 3 4 -> 旋转1 2 变为 2 1-> 继续将右边1 3 4进行递回处理
3 1 2 4 -> 旋转1 2 3变为 3 1 2 -> 继续将右边1 2 4进行递回处理
4 1 2 3 -> 旋转1 2 3 4变为4 1 2 3 -> 继续将右边1 2 3进行递回处理

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
#define N 4
void perm(int*, int);
int main(void) {
int num[N+1], i;
for(i = 1; i <= N; i++)
num[i] = i;
perm(num, 1);
return 0;
}
void perm(int* num, int i) {
int j, k, tmp;
if(i < N) {
for(j = i; j <= N; j++) {
tmp = num[j];
// 旋转该区段最右边数字至最左边
for(k = j; k > i; k--)
num[k] = num[k-1];
num[i] = tmp;
perm(num, i+1);
// 还原
for(k = i; k < j; k++)
num[k] = num[k+1];
num[j] = tmp;
}
}
else { // 显示此次排列
for(j = 1; j <= N; j++)
printf("%d ", num[j]);
printf("\n");
}
}

* Java
public class Permutation {
public static void perm(int[] num, int i) {
if(i < num.length - 1) {
for(int j = i; j <= num.length - 1; j++) {
int tmp = num[j];
// 旋转该区段最右边数字至最左边
for(int k = j; k > i; k--)
num[k] = num[k-1];
num[i] = tmp;
perm(num, i+1);
// 还原
for(int k = i; k < j; k++)
num[k] = num[k+1];
num[j] = tmp;
}
}
else { // 显示此次排列
for(int j = 1; j <= num.length - 1; j++)
System.out.print(num[j] + " ");
System.out.println();
}
}
public static void main(String[] args) {
int[] num = new int[4+1];
for(int i = 1; i <= num.length - 1; i++)
num[i] = i;
perm(num, 1);
}
}
