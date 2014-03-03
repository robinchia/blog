---
layout: post
title: "TwoNOneArray"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# TwoNOneArray

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 2(2N+1) 魔方阵]()

## 说明

方阵的维度整体来看是偶数，但是其实是一个奇数乘以一个偶数，例如6X6，其中6=2X3，我们也称这种方阵与单偶数方阵。

## 解法

如果您会解奇数魔术方阵，要解这种方阵也就不难理解，首先我们令n=2(2m+1)，并将整个方阵看作是数个奇数方阵的组合，如下所示：
![2(2N+1)魔方阵]( "2(2N+1)魔方阵")
首先依序将A、B、C、D四个位置，依奇数方阵的规则填入数字，填完之后，方阵中各行的和就相同了，但列与对角线则否，此时必须在A-D与C- B之间，作一些对应的调换，规则如下：

1. 将A中每一列(中间列除外)的头m个元素，与D中对应位置的元素调换。
1. 将A的中央列、中央那一格向左取m格，并与D中对应位置对调
1. 将C中每一列的倒数m-1个元素，与B中对应的元素对调
举个实例来说，如何填6X6方阵，我们首先将之分解为奇数方阵，并填入数字，如下所示：
![2(2N+1)魔方阵]( "2(2N+1)魔方阵")
接下来进行互换的动作，互换的元素以不同颜色标示，如下：
![2(2N+1)魔方阵]( "2(2N+1)魔方阵")
由于m-1的数为0，所以在这个例子中，C-B部份并不用进行对调。

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
#define N 6
#define SWAP(x,y) {int t; t = x; x = y; y = t;}
void magic_o(int [][N], int);
void exchange(int [][N], int);
int main(void) {
int square[N][N] = {0};
int i, j;
magic_o(square, N/2);
exchange(square, N);
for(i = 0; i < N; i++) {
for(j = 0; j < N; j++)
printf("%2d ", square[i][j]);
printf("\n");
}
return 0;
}
void magic_o(int square[][N], int n) {
int count, row, column;
row = 0;
column = n / 2;
for(count = 1; count <= n*n; count++) {
square[row][column] = count; // 填A
square[row+n][column+n] = count + n*n; // 填B
square[row][column+n] = count + 2*n*n; // 填C
square[row+n][column] = count + 3*n*n; // 填D
if(count % n == 0)
row++;
else {
row = (row == 0) ? n - 1 : row - 1 ;
column = (column == n-1) ? 0 : column + 1;
}
}
}
void exchange(int x[][N], int n) {
int i, j;
int m = n / 4;
int m1 = m - 1;
for(i = 0; i < n/2; i++) {
if(i != m) {
for(j = 0; j < m; j++) // 处理规则 1
SWAP(x[i][j], x[n/2+i][j]);
for(j = 0; j < m1; j++) // 处理规则 2
SWAP(x[i][n-1-j], x[n/2+i][n-1-j]);
}
else { // 处理规则 3
for(j = 1; j <= m; j++)
SWAP(x[m][j], x[n/2+m][j]);
for(j = 0; j < m1; j++)
SWAP(x[m][n-1-j], x[n/2+m][n-1-j]);
}
}
}

* Java
public class Matrix {
public static int[][] magic22mp1(int n) {
int[][] square = new int[n][n];
magic_o(square, n/2);
exchange(square, n);
return square;
}
private static void magic_o(int[][] square, int n) {
int row = 0;
int column = n / 2;
for(int count = 1; count <= n*n; count++) {
square[row][column] = count; // 填A
square[row+n][column+n] = count + n*n; // 填B
square[row][column+n] = count + 2*n*n; // 填C
square[row+n][column] = count + 3*n*n; // 填D
if(count % n == 0)
row++;
else {
row = (row == 0) ? n - 1 : row - 1 ;
column = (column == n-1) ? 0 : column + 1;
}
}
}
private static void exchange(int[][] x, int n) {
int i, j;
int m = n / 4;
int m1 = m - 1;
for(i = 0; i < n/2; i++) {
if(i != m) {
for(j = 0; j < m; j++) // 处理规则 1
swap(x, i, j, n/2+i, j);
for(j = 0; j < m1; j++) // 处理规则 2
swap(x, i, n-1-j, n/2+i, n-1-j);
}
else { // 处理规则 3
for(j = 1; j <= m; j++)
swap(x, m, j, n/2+m, j);
for(j = 0; j < m1; j++)
swap(x, m, n-1-j, n/2+m, n-1-j);
}
}
}
private static void swap(int[][] number,
int i, int j, int k, int l) {
int t;
t = number[i][j];
number[i][j] = number[k][l];
number[k][l] = t;
}
public static void main(String[] args) {
int[][] magic = Matrix.magic22mp1(6);
for(int k = 0; k < magic.length; k++) {
for(int l = 0; l < magic[0].length; l++) {
System.out.print(magic[k][l] + " ");
}
System.out.println();
}
}
}
