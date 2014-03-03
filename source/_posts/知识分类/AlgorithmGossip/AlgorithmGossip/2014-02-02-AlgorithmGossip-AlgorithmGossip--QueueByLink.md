---
layout: post
title: "QueueByLink"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# QueueByLink

### [From Gossip@caterpillar](http://caterpillar.onlyfun.net/GossipCN/index.html)

# [Algorithm Gossip: 伫列 - 使用链结实作（C语言动态记忆体宣告）]()

## 说明

使用阵列来实作伫列，会有伫列空间的限制，如果使用链结配合动态记忆体宣告，就不会有长度的限制。

## 解法

在使用链结实作伫列时，为了方便，使用一个空的节点来指向伫列前端第一个元素，这个空节点的data栏位并不储存资料，而next当作front的角色，如下图所示：
![Link]( "Link")
为了配合以上的空节点，将伫列是否为空的判断方式，改为front->next是否指向下一个节点来完成，如果front->next指向NULL，表示链结中已无下一个资料，所以伫列为空。
由于必须同时维护front与rear两个资讯，而C语言一次只能传回一个值，为了简化程式逻辑，我们将front与rear宣告为全域变数，有兴趣的话，请自己试着不设为全域变数来撰写，这会让程式变得复杂一些。

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
struct node {
int data;
struct node *next;
};
typedef struct node getnode;
void createq();
void showfront();
void addq(int);
void delq();
void showqueue();
getnode *front, *rear;
int main(void) {
int input, select;
createq();
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
addq(input);
break;
case 2:
showfront();
break;
case 3:
delq();
break;
case 4:
showqueue();
break;
default:
printf("\n选项错误！");
}
}
printf("\n");
return 0;
}
void createq() {
front = rear = (getnode*) malloc(sizeof(getnode));
front->next = rear->next = NULL;
}
void showfront(){
if(front->next == NULL)
printf("\n伫列为空！");
else
printf("\n顶端值：%d", front->next->data);
}
void addq(int data) {
getnode *newnode;
newnode = (getnode*) malloc(sizeof(getnode));
if(front->next == NULL)
front->next = newnode;
newnode->data = data;
newnode->next = NULL;
rear->next = newnode;
rear = newnode;
}
void delq() {
getnode* tmpnode;
if(front->next == NULL) {
printf("\n伫列已空！");
return;
}
tmpnode = front->next;
front->next = tmpnode->next;
free(tmpnode);
}
void showqueue() {
getnode* tmpnode;
tmpnode = front->next;
printf("\n伫列内容：");
while(tmpnode != NULL) {
printf("%d ", tmpnode->data);
tmpnode = tmpnode->next;
}
}
