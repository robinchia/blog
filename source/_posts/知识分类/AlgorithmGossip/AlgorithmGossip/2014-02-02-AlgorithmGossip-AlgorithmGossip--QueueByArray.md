---
layout: post
title: "QueueByArray"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# QueueByArray

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 伫列 - 使用阵列实作]()

## 说明

伫列是一种先进先出的资料结构，想像您在管子中放入球，最先放入的球在另一端会最先跑出来，在这边介绍如何使用阵列来实作伫列。

## 解法

使用阵列来实作伫列，我们必须保留两个旗标，假设front指向伫列的前端，rear向伫列的后端，我们每次从伫列后端加入一个资料，rear就加1指向最后一个资料，每次从伫列前端取出一个资料，front就加1指向伫列的最前端，如下图所示：
![Quene]( "Quene")
这是最简单的伫列实作，但是由于阵列的大小必须先决定，所以这种线性的结构有个问题，front与rear会到达阵列的后端，而这个阵列就不能再使用了， 为了解决这个问题，将阵列当作环状来使用，当front或rear到达阵列后端时，就重新从阵列前端再循环，也就是形成环状伫列，如下图所示：
![Quene]( "Quene")
不过阵列的容量还是受限制，所以这个阵列还是会满的，当front = rear时，伫列就满了；伫列的基本操作有五项：新增伫列、加入资料、显示前端资料、取出前端资料、显示所有的伫列元素。

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
#define N 10
void createq(int[], int*, int*);
void showfront(int[], int, int);
void add(int[], int*, int*, int);
void del(int[], int*, int*);
void showqueue(int[], int, int);
int main(void) {
int queue[N];
int front, rear;
int input, select;
createq(queue, &front, &rear);
while(1) {
printf("\n\n请输入选项(-1结束)：");
printf("\n(1)插入值至伫列");
printf("\n(2)显示伫列前端");
printf("\n(3)删除前端值");
printf("\n(4)显示所有内容");
printf("\n$c>");
scanf("%d", &select);
if(select == -1)
break;
switch(select) {
case 1:
printf("\n输入值：");
scanf("%d", &input);
add(queue, &front, &rear, input);
break;
case 2:
showfront(queue, front, rear);
break;
case 3:
del(queue, &front, &rear);
break;
case 4:
showqueue(queue, front, rear);
break;
default:
printf("\n选项错误！");
}
}
printf("\n");
return 0;
}
void createq(int queue[], int* front, int* rear) {
int i;
for(i = 0; i < N; i++)
queue[i] = 0;
*front = *rear = 0;
}
void showfront(int queue[], int front, int rear) {
if(front == rear)
printf("\n伫列为空！");
else
printf("%d", queue[(front+1) % N]);
}
void add(int queue[], int* front, int* rear, int data) {
int f, r;
f = *front;
r = *rear;
r = (r+1) % N;
if(f == r) {
printf("\n伫列已满！");
return;
}
queue[r] = data;
*rear = r;
}
void del(int queue[], int* front, int* rear) {
int f, r;
f = *front;
r = *rear;
if(f == r) {
printf("\n伫列为空！");
return;
}
f = (f+1) % N;
*front = f;
}
void showqueue(int queue[], int front, int rear) {
int i;
printf("\n伫列内容：");
for(i = (front+1) % N; i <= rear; i++)
printf("%d ", queue[i]);
}

## 补充

您如果仔细演算过上面的环状伫列，您会发现有一个空间会被浪费掉，这是因为判断伫列已满或已空的条件都是front = rear，浪费一个空间对现在的电脑记忆体如此足够来说，并不是个大问题，如果您一定要解决这个问题，可以多使用一个flag来判断，如果flag设定为 1且front = rear，则表示伫列已满，如果flag设定为0则front = rear，则表示伫列已空，这样就不会浪费一个伫列空间了，提供改良后的虚拟码如下：
Procedure add(queue, n, front, rear, flag, data)
if(front = rear and flag = 1)
then call QUEUE_FULL
queue(rear) <- data
if(front = rear)
then flag <- 1
end add
Procedure del(queue, n, front, rear, flag, data)
if(front = rear and flag = 0)
then call QUEUE_EMPTY
front <- (front+1) mod n
if(front = rear)
then flag <- 1
end del
