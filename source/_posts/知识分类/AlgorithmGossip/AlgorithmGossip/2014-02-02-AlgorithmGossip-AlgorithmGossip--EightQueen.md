---
layout: post
title: "EightQueen"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# EightQueen

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 八个皇后]()

##  说明

西洋棋中的皇后可以直线前进，吃掉遇到的所有棋子，如果棋盘上有八个皇后，则这八个皇后如何相安无事的放置在棋盘上，1970年与1971年， E.W.Dijkstra与N.Wirth曾经用这个问题来讲解程式设计之技巧。

## 解法

关于棋盘的问题，都可以用递回求解，然而如何减少递回的次数？在八个皇后的问题中，不必要所有的格子都检查过，例如若某列检查过，该该列的其它格子就不用再检查了，这个方法称为分支修剪。
![八个皇后]( "八个皇后")
所以检查时，先判断是否在已放置皇后的可行进方向上，如果没有再行放置下一个皇后，如此就可大大减少递回的次数，例如以下为修剪过后的递回检查行进路径：
![八个皇后]( "八个皇后")
八个皇后的话，会有92个解答，如果考虑棋盘的旋转，则旋转后扣去对称的，会有12组基本解。 

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
#define N 8
int column[N+1]; // 同栏是否有皇后，1表示有
int rup[2*N+1]; // 右上至左下是否有皇后
int lup[2*N+1]; // 左上至右下是否有皇后
int queen[N+1] = {0};
int num; // 解答编号
void backtrack(int); // 递回求解
int main(void) {
int i;
num = 0;
for(i = 1; i <= N; i++)
column[i] = 1;
for(i = 1; i <= 2*N; i++)
rup[i] = lup[i] = 1;
backtrack(1);
return 0;
}
void showAnswer() {
int x, y;
printf("\n解答 %d\n", ++num);
for(y = 1; y <= N; y++) {
for(x = 1; x <= N; x++) {
if(queen[y] == x) {
printf(" Q");
}
else {
printf(" .");
}
}
printf("\n");
}
}
void backtrack(int i) {
int j;
if(i > N) {
showAnswer();
}
else {
for(j = 1; j <= N; j++) {
if(column[j] == 1 &&
rup[i+j] == 1 && lup[i-j+N] == 1) {
queen[i] = j;
// 设定为占用
column[j] = rup[i+j] = lup[i-j+N] = 0;
backtrack(i+1);
column[j] = rup[i+j] = lup[i-j+N] = 1;
}
}
}
}

* Java
public class Queen {
// 同栏是否有皇后，1表示有
private int[] column;
// 右上至左下是否有皇后
private int[] rup;
// 左上至右下是否有皇后
private int[] lup;
// 解答
private int[] queen;
// 解答编号
private int num;
public Queen() {
column = new int[8+1];
rup = new int[2*8+1];
lup = new int[2*8+1];
for(int i = 1; i <= 8; i++)
column[i] = 1;
for(int i = 1; i <= 2*8; i++)
rup[i] = lup[i] = 1;
queen = new int[8+1];
}
public void backtrack(int i) {
if(i > 8) {
showAnswer();
}
else {
for(int j = 1; j <= 8; j++) {
if(column[j] == 1 &&
rup[i+j] == 1 &&
lup[i-j+8] == 1) {
queen[i] = j;
// 设定为占用
column[j] = rup[i+j] = lup[i-j+8] = 0;
backtrack(i+1);
column[j] = rup[i+j] = lup[i-j+8] = 1;
}
}
}
}
protected void showAnswer() {
num++;
System.out.println("\n解答 " + num);
for(int y = 1; y <= 8; y++) {
for(int x = 1; x <= 8; x++) {
if(queen[y] == x) {
System.out.print(" Q");
}
else {
System.out.print(" .");
}
}
System.out.println();
}
}
public static void main(String[] args) {
Queen queen = new Queen();
queen.backtrack(1);
}
}
