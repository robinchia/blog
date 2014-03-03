---
layout: post
title: "KnightTour"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# KnightTour

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 骑士走棋盘]()

##  说明

骑士旅游（Knight tour）在十八世纪初倍受数学家与拼图迷的注意，它什么时候被提出已不可考，骑士的走法为西洋棋的走法，骑士可以由任一个位置出发，它要如何走完[所有的位置？

## 解法

骑士的走法，基本上可以使用递回来解决，但是纯綷的递回在维度大时相当没有效率，一个聪明的解法由J.C. Warnsdorff在1823年提出，简单的说，先将最难的位置走完，接下来的路就宽广了，骑士所要走的下一步，“为下一步再选择时，所能走的步数最少 的一步。”，使用这个方法，在不使用递回的情况下，可以有较高的机率找出走法（找不到走法的机会也是有的）。

## 演算法

FOR(m = 2; m <= 总步数; m++) [
测试下一步可以走的八个方向，记录可停留的格数count。
IF(count == 0) [
游历失败
]
ELSE IF(count == 1) [
下一步只有一个可能
]
ELSE [
找出下一步的出路最少的格子
如果出路值相同，则选第一个遇到的出路。
]
走最少出路的格子，记录骑士的新位置。
]

## 实作

* C
#include <stdio.h>
int board[8][8] = {0};
int main(void) {
int startx, starty;
int i, j;
printf("输入起始点：");
scanf("%d %d", &startx, &starty);
if(travel(startx, starty)) {
printf("游历完成！\n");
}
else {
printf("游历失败！\n");
}
for(i = 0; i < 8; i++) {
for(j = 0; j < 8; j++) {
printf("%2d ", board[i][j]);
}
putchar('\n');
}
return 0;
}
int travel(int x, int y) {
// 对应骑士可走的八个方向
int ktmove1[8] = {-2, -1, 1, 2, 2, 1, -1, -2};
int ktmove2[8] = {1, 2, 2, 1, -1, -2, -2, -1};
// 测试下一步的出路
int nexti[8] = {0};
int nextj[8] = {0};
// 记录出路的个数
int exists[8] = {0};
int i, j, k, m, l;
int tmpi, tmpj;
int count, min, tmp;
i = x;
j = y;
board[i][j] = 1;
for(m = 2; m <= 64; m++) {
for(l = 0; l < 8; l++) {
exists[l] = 0;
}
l = 0;
// 试探八个方向
for(k = 0; k < 8; k++) {
tmpi = i + ktmove1[k];
tmpj = j + ktmove2[k];
// 如果是边界了，不可走
if(tmpi < 0 || tmpj < 0 || tmpi > 7 || tmpj > 7)
continue;
// 如果这个方向可走，记录下来
if(board[tmpi][tmpj] == 0) {
nexti[l] = tmpi;
nextj[l] = tmpj;
// 可走的方向加一个
l++;
}
}
count = l;
// 如果可走的方向为0个，返回
if(count == 0) {
return 0;
}
else if(count == 1) {
// 只有一个可走的方向
// 所以直接是最少出路的方向
min = 0;
}
else {
// 找出下一个位置的出路数
for(l = 0; l < count; l++) {
for(k = 0; k < 8; k++) {
tmpi = nexti[l] + ktmove1[k];
tmpj = nextj[l] + ktmove2[k];
if(tmpi < 0 || tmpj < 0 ||
tmpi > 7 || tmpj > 7) {
continue;
}
if(board[tmpi][tmpj] == 0)
exists[l]++;
}
}
tmp = exists[0];
min = 0;
// 从可走的方向中寻找最少出路的方向
for(l = 1; l < count; l++) {
if(exists[l] < tmp) {
tmp = exists[l];
min = l;
}
}
}
// 走最少出路的方向
i = nexti[min];
j = nextj[min];
board[i][j] = m;
}
return 1;
}

* Java
public class Knight {
public boolean travel(int startX,
int startY, int[][] board) {
// 对应骑士可走的八个方向
int[] ktmove1 = {-2, -1, 1, 2, 2, 1, -1, -2};
int[] ktmove2 = {1, 2, 2, 1, -1, -2, -2, -1};
// 下一步出路的位置
int[] nexti = new int[board.length];
int[] nextj = new int[board.length];
// 记录出路的个数
int[] exists = new int[board.length];
int x = startX;
int y = startY;
board[x][y] = 1;
for(int m = 2; m <= Math.pow(board.length, 2); m++) {
for(int k = 0; k < board.length; k++) {
exists[k] = 0;
}
int count = 0;
// 试探八个方向
for(int k = 0; k < board.length; k++) {
int tmpi = x + ktmove1[k];
int tmpj = y + ktmove2[k];
// 如果是边界了，不可走
if(tmpi < 0 || tmpj < 0 ||
tmpi > 7 || tmpj > 7) {
continue;
}
// 如果这个方向可走，记录下来
if(board[tmpi][tmpj] == 0) {
nexti[count] = tmpi;
nextj[count] = tmpj;
// 可走的方向加一个
count++;
}
}
int min = -1;
if(count == 0) {
return false;
}
else if(count == 1) {
min = 0;
}
else {
// 找出下一个位置的出路数
for(int l = 0; l < count; l++) {
for(int k = 0; k < board.length; k++) {
int tmpi = nexti[l] + ktmove1[k];
int tmpj = nextj[l] + ktmove2[k];
if(tmpi < 0 || tmpj < 0 ||
tmpi > 7 || tmpj > 7) {
continue;
}
if(board[tmpi][tmpj] == 0)
exists[l]++;
}
}
int tmp = exists[0];
min = 0;
// 从可走的方向中寻找最少出路的方向
for(int l = 1; l < count; l++) {
if(exists[l] < tmp) {
tmp = exists[l];
min = l;
}
}
}
// 走最少出路的方向
x = nexti[min];
y = nextj[min];
board[x][y] = m;
}
return true;
}
public static void main(String[] args) {
int[][] board = new int[8][8];
Knight knight = new Knight();
if(knight.travel(
Integer.parseInt(args[0]),
Integer.parseInt(args[1]), board)) {
System.out.println("游历完成！");
}
else {
System.out.println("游历失败！");
}
for(int i = 0; i < board.length; i++) {
for(int j = 0; j < board[0].length; j++) {
if(board[i][j] < 10) {
System.out.print(" " + board[i][j]);
}
else {
System.out.print(board[i][j]);
}
System.out.print(" ");
}
System.out.println();
}
}
}
