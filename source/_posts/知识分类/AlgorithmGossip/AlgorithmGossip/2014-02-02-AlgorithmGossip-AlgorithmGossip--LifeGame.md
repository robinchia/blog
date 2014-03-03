---
layout: post
title: "LifeGame"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# LifeGame

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 生命游戏]()

##  说明

生命游戏（game of life）为1970年由英国数学家J. H. Conway所提出，某一细胞的邻居包括上、下、左、右、左上、左下、右上与右下相邻之细胞，游戏规则如下：

1. 孤单死亡：如果细胞的邻居小于一个，则该细胞在下一次状态将死亡。
1. 拥挤死亡：如果细胞的邻居在四个以上，则该细胞在下一次状态将死亡。
1. 稳定：如果细胞的邻居为二个或三个，则下一次状态为稳定存活。
1. 复活：如果某位置原无细胞存活，而该位置的邻居为三个，则该位置将复活一细胞。

## 解法

生命游戏的规则可简化为以下，并使用CASE比对即可使用程式实作：

1. 邻居个数为0、1、4、5、6、7、8时，则该细胞下次状态为死亡。
1. 邻居个数为2时，则该细胞下次状态为稳定。
1. 邻居个数为3时，则该细胞下次状态为复活。

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
#define MAXROW 10
#define MAXCOL 25
#define DEAD 0
#define ALIVE 1
int map[MAXROW][MAXCOL], newmap[MAXROW][MAXCOL];
void init();
int neighbors(int, int);
void outputMap();
void copyMap();
int main() {
int row, col;
char ans;
init();
while(1) {
outputMap();
for(row = 0; row < MAXROW; row++) {
for(col = 0; col < MAXCOL; col++) {
switch (neighbors(row, col)) {
case 0:
case 1:
case 4:
case 5:
case 6:
case 7:
case 8:
newmap[row][col] = DEAD;
break;
case 2:
newmap[row][col] = map[row][col];
break;
case 3:
newmap[row][col] = ALIVE;
break;
}
}
}
copyMap();
printf("\nContinue next Generation ? ");
getchar();
ans = toupper(getchar());
if(ans != 'Y')
break;
}
return 0;
}
void init() {
int row, col;
for(row = 0; row < MAXROW; row++)
for(col = 0; col < MAXCOL; col++)
map[row][col] = DEAD;
puts("Game of life Program");
puts("Enter x, y where x, y is living cell");
printf("0 <= x <= %d, 0 <= y <= %d\n",
MAXROW-1, MAXCOL-1);
puts("Terminate with x, y = -1, -1");
while(1) {
scanf("%d %d", &row, &col);
if(0 <= row && row < MAXROW &&
0 <= col && col < MAXCOL)
map[row][col] = ALIVE;
else if(row == -1 || col == -1)
break;
else
printf("(x, y) exceeds map ranage!");
}
}
int neighbors(int row, int col) {
int count = 0, c, r;
for(r = row-1; r <= row+1; r++)
for(c = col-1; c <= col+1; c++) {
if(r < 0 || r >= MAXROW || c < 0 || c >= MAXCOL)
continue;
if(map[r][c] == ALIVE)
count++;
}
if(map[row][col] == ALIVE)
count--;
return count;
}
void outputMap() {
int row, col;
printf("\n\n%20cGame of life cell status\n");
for(row = 0; row < MAXROW; row++) {
printf("\n%20c", ' ');
for(col = 0; col < MAXCOL; col++)
if(map[row][col] == ALIVE)
putchar('#');
else
putchar('-');
}
}
void copyMap() {
int row, col;
for(row = 0; row < MAXROW; row++)
for(col = 0; col < MAXCOL; col++)
map[row][col] = newmap[row][col];
}

* Java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
public class LifeGame {
private boolean[][] map;
private boolean[][] newmap;
public LifeGame(int maxRow, int maxColumn) {
map = new boolean[maxRow][maxColumn];
newmap = new boolean[maxRow][maxColumn];
}
public void setCell(int x, int y) {
map[x][y] = true;
}
public void next() {
for(int row = 0; row < map.length; row++) {
for(int col = 0; col < map[0].length; col++) {
switch (neighbors(row, col)) {
case 0:
case 1:
case 4:
case 5:
case 6:
case 7:
case 8:
newmap[row][col] = false;
break;
case 2:
newmap[row][col] = map[row][col];
break;
case 3:
newmap[row][col] = true;
break;
}
}
}
copyMap();
}
public void outputMap() throws IOException {
System.out.println("\n\nGame of life cell status");
for(int row = 0; row < map.length; row++) {
System.out.print("\n ");
for(int col = 0; col < map[0].length; col++)
if(map[row][col] == true)
System.out.print('#');
else
System.out.print('-');
}
}
private void copyMap() {
for(int row = 0; row < map.length; row++)
for(int col = 0; col < map[0].length; col++)
map[row][col] = newmap[row][col];
}
private int neighbors(int row, int col) {
int count = 0;
for(int r = row-1; r <= row+1; r++)
for(int c = col-1; c <= col+1; c++) {
if(r < 0 || r >= map.length ||
c < 0 || c >= map[0].length)
continue;
if(map[r][c] == true)
count++;
}
if(map[row][col] == true)
count--;
return count;
}
public static void main(String[] args)
throws NumberFormatException, IOException {
BufferedReader bufReader =
new BufferedReader(
new InputStreamReader(System.in));
LifeGame game = new LifeGame(10, 25);
System.out.println("Game of life Program");
System.out.println(
"Enter x, y where x, y is living cell");
System.out.println("0 <= x < 10, 0 <= y < 25");
System.out.println("Terminate with x, y = -1, -1");
while(true) {
String[] strs = bufReader.readLine().split(" ");
int row = Integer.parseInt(strs[0]);
int col = Integer.parseInt(strs[1]);
if(0 <= row && row < 10 && 0 <= col && row < 25)
game.setCell(row, col);
else if(row == -1 || col == -1) {
break;
}
else {
System.out.print(
"(x, y) exceeds map ranage!");
}
}
while(true) {
game.outputMap();
game.next();
System.out.print(
"\nContinue next Generation ? ");
String ans = bufReader.readLine().toUpperCase();
if(!ans.equals("Y"))
break;
}
}
}
