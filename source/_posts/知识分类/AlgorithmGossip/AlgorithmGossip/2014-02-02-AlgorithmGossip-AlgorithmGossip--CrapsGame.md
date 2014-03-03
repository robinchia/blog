---
layout: post
title: "CrapsGame"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# CrapsGame

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: Craps赌博游戏]()

## 说明

一个简单的赌博游戏，游戏规则如下：玩家掷两个骰子，点数为1到6，如果第一次点数和为7或11，则玩家胜，如果点数和为2、3或12，则玩家输，如果和 为其它点数，则记录第一次的点数和，然后继续掷骰，直至点数和等于第一次掷出的点数和，则玩家胜，如果在这之前掷出了点数和为7，则玩家输。

## 解法

规则看来有些复杂，但是其实只要使用switch配合if条件判断来撰写即可，小心不要弄错胜负顺序即可。

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#define WON 0
#define LOST 1
#define CONTINUE 2
int rollDice() {
return (rand() % 6) + (rand() % 6) + 2;
}
int main(void) {
int firstRoll = 1;
int gameStatus = CONTINUE;
int die1, die2, sumOfDice;
int firstPoint = 0;
char c;
srand(time(0));
printf("Craps赌博游戏，按Enter键开始游戏****");
while(1) {
getchar();
if(firstRoll) {
sumOfDice = rollDice();
printf("\n玩家掷出点数和：%d\n", sumOfDice);
switch(sumOfDice) {
case 7: case 11:
gameStatus = WON; break;
case 2: case 3: case 12:
gameStatus = LOST; break;
default:
firstRoll = 0;
gameStatus = CONTINUE;
firstPoint = sumOfDice;
break;
}
}
else {
sumOfDice = rollDice();
printf("\n玩家掷出点数和：%d\n", sumOfDice);
if(sumOfDice == firstPoint)
gameStatus = WON;
else if(sumOfDice == 7)
gameStatus = LOST;
}
if(gameStatus == CONTINUE)
puts("未分胜负，再掷一次****\n");
else {
if(gameStatus == WON)
puts("玩家胜");
else
puts("玩家输");
printf("再玩一次？");
scanf("%c", &c);
if(c == 'n') {
puts("游戏结束");
break;
}
firstRoll = 1;
}
}
return 0;
}

* Java
import java.io.*;
public class Craps {
public static void main(String[] args)
throws IOException {
final int WON = 0, LOST = 1, CONTINUE = 2;
boolean firstRoll = true;
int gameStatus = CONTINUE;
int die1, die2, sumOfDice;
int firstPoint = 0;
System.out.print(
"Craps赌博游戏，按Enter键开始游戏****");
while(true) {
System.in.read();
if(firstRoll) {
sumOfDice = rollDice();
System.out.println(
"\n玩家掷出点数和：" + sumOfDice);
switch(sumOfDice) {
case 7: case 11:
gameStatus = WON; break;
case 2: case 3: case 12:
gameStatus = LOST; break;
default:
firstRoll = false;
gameStatus = CONTINUE;
firstPoint = sumOfDice;
break;
}
}
else {
sumOfDice = rollDice();
System.out.println(
"\n玩家掷出点数和：" + sumOfDice);
if(sumOfDice == firstPoint)
gameStatus = WON;
else if(sumOfDice == 7)
gameStatus = LOST;
}
if(gameStatus == CONTINUE)
System.out.println("未分胜负，再掷一次****");
else {
if(gameStatus == WON)
System.out.println("玩家胜");
else
System.out.println("玩家输");
System.out.print("再玩一次？");
if(System.in.read() == 'n') {
System.out.println("游戏结束");
break;
}
firstRoll = true;
}
}
}
public static int rollDice() {
int roll = ((int)(Math.random() * 6) +
(int)(Math.random() * 6));
if(roll < 2) {
roll = 2;
}
return roll;
}
}
