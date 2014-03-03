---
layout: post
title: "StackByArray"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# StackByArray

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 堆叠 - 使用阵列实作]()

## 说明

堆叠是一种先进后出的资料结构，就如同您将书本放入箱子，最先放进的书本在最后才能拿出来，所有资料的加入与删除都在堆叠顶端完成。堆叠的使用很广，递回 就是一种堆叠，在之前介绍中序式转后序式时，也使用到堆叠的结构。
堆叠可以使用多种方式实作，其中使用阵列是最简单的方法，也最不受使用的程式语言所限制。

## 解法

堆叠最重要的就是记录顶端的位置，尤其是在堆叠空间固定的情况下，必须注意堆叠已满或已空的判断，当使用阵列实作堆叠时尤其重要。
堆叠的基本操作有五项：建立堆叠、传回顶端元素、加入元素至堆叠、删除元素至堆叠、显示堆叠所有内容。为了方便，加入一个测试堆叠是否为空的方法，详 细的演算并不难，直接列出程式实作。

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
#define MAX 10
int creates(int[]); // 建立堆叠
int isEmpty(int); // 堆叠已空
int stacktop(int[], int); // 传回顶端元素
int add(int[], int, int); // 插入元素
int delete(int[], int); // 删除元素
void list(int[], int); // 显示所有内容
int main(void) {
int stack[MAX];
int top;
int input, select;
top = creates(stack);
while(1) {
printf("\n\n请输入选项(-1结束)：");
printf("\n(1)插入值至堆叠");
printf("\n(2)显示堆叠顶端");
printf("\n(3)删除顶端值");
printf("\n(4)显示所有内容");
printf("\n$c>");
scanf("%d", &select);
if(select == -1)
break;
switch(select) {
case 1:
printf("\n输入值：");
scanf("%d", &input);
top = add(stack, top, input);
break;
case 2:
printf("\n顶端值：%d", stacktop(stack, top));
break;
case 3:
top = delete(stack, top);
break;
case 4:
list(stack, top);
break;
default:
printf("\n选项错误！");
}
}
printf("\n");
return 0;
}
// 以下为堆叠操作的实作
int creates(int stack[]) {
int i;
for(i = 0; i < MAX; i++)
stack[i] = 0;
return -1;
}
int isEmpty(int top) {
return (top == -1);
}
int stacktop(int stack[], int top) {
return stack[top];
}
int add(int stack[], int top, int item) {
int t = top;
if(t >= MAX-1) {
printf("\n堆叠已满！");
return t;
}
stack[++t] = item;
return t;
}
int delete(int stack[], int top) {
int t = top;
if(isEmpty(t)) {
printf("\n堆叠已空！");
return t;
}
return --t;
}
void list(int stack[], int top) {
int t = top;
printf("\n堆叠内容：");
while(!isEmpty(t)) {
printf("%d ", stack[t]);
t--;
}
}
