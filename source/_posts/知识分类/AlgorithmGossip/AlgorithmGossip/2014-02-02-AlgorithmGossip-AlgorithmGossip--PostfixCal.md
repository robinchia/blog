---
layout: post
title: "PostfixCal"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# PostfixCal

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 后序式的运算]()

说明 将 [中序式转换为后序式]() 的好处是，不用处理运算子先后顺序问题，只要依序由运算式由前往后读取即可。

## 解法

运算时由后序式的前方开始读取，遇到运算元先存入堆叠，如果遇到运算子，则由堆叠中取出两个运算元进行对应的运算，然后将结果存回堆叠，如果运算式读取完 毕，那么堆叠顶的值就是答案了，例如我们计算12+34+*这个运算式（也就是(1+2)*(3+4)）： 读取 堆叠 1 1 2 1 2 + 3 // 1+2 后存回 3 3 3 4 3 3 4 + 3 7 // 3+4 后存回 * 21 // 3 * 7 后存回

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
void evalPf(char*);
double cal(double, char, double);
int main(void) {
char input[80];
printf("输入后序式：");
scanf("%s", input);
evalPf(input);
return;
}
void evalPf(char* postfix) {
double stack[80] = {0.0};
char temp[2];
char token;
int top = 0, i = 0;
temp[1] = '\0';
while(1) {
token = postfix[i];
switch(token) {
case '\0':
printf("ans = %f\n", stack[top]);
return;
case '+': case '-': case '*': case '/':
stack[top-1] =
cal(stack[top-1], token, stack[top]);
top--;
break;
default:
if(top < sizeof(stack) / sizeof(float)) {
temp[0] = postfix[i];
top++;
stack[top] = atof(temp);
}
break;
}
i++;
}
}
double cal(double p1, char op, double p2) {
switch(op) {
case '+':
return p1 + p2;
case '-':
return p1 - p2;
case '*':
return p1 * p2;
case '/':
return p1 / p2;
}
}

* Java
public class InFix {
private static int priority(char op) {
switch(op) {
case '+': case '-':
return 1;
case '*': case '/':
return 2;
default:
return 0;
}
}
public static char[] toPosfix(char[] infix) {
char[] stack = new char[infix.length];
char[] postfix = new char[infix.length];
char op;
StringBuffer buffer = new StringBuffer();
int top = 0;
for(int i = 0; i < infix.length; i++) {
op = infix[i];
switch(op) {
// 运算子堆叠
case '(':
if(top < stack.length) {
top++;
stack[top] = op;
}
break;
case '+': case '-': case '*': case '/':
while(priority(stack[top]) >=
priority(op)) {
buffer.append(stack[top]);
top--;
}
// 存入堆叠
if(top < stack.length) {
top++;
stack[top] = op;
}
break;
// 遇 ) 输出至 (
case ')':
while(stack[top] != '(') {
buffer.append(stack[top]);
top--;
}
top--; // 不输出(
break;
// 运算元直接输出
default:
buffer.append(op);
break;
}
}
while(top > 0) {
buffer.append(stack[top]);
top--;
}
return buffer.toString().toCharArray();
}
private static double cal(double p1, char op, double p2) {
switch(op) {
case '+':
return p1 + p2;
case '-':
return p1 - p2;
case '*':
return p1 * p2;
case '/':
return p1 / p2;
}
return 0.0;
}
public static double eval(char[] postfix) {
double[] stack = new double[postfix.length];
char token;
int top = 0;
for(int i = 0; i < postfix.length; i++) {
token = postfix[i];
switch(token) {
case '+': case '-': case '*': case '/':
stack[top-1] =
cal(stack[top-1], token, stack[top]);
top--;
break;
default:
if(top < stack.length) {
char temp = postfix[i];
top++;
stack[top] =
Double.parseDouble(
String.valueOf(temp));
}
break;
}
}
return stack[top];
}
public static void main(String[] args) {
String infix = "(1+2)*(3+4)";
System.out.println(InFix.eval(
InFix.toPosfix(infix.toCharArray())));
}
}
