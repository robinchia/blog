---
layout: post
title: "StackByLink"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# StackByLink

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 堆叠 - 使用链结实作（C 语言动态记忆体宣告）]()

## 说明

使用阵列实作堆叠，会受到阵列大小必须事先宣告好的限制，我们可以使用链结（link）的方式来实作堆叠，以动态记忆体宣告的方式来新增每一个元素。

## 解法

链结（link）是由节点组成，每一个节点储存资料之外，还储存下一个节点的位置，如下图所示：
![Link]( "Link")
对堆叠而言，加入新节点与删除节点的方法如下图所示：
新增节点：
![Link]( "Link")
删除节点：
![Link]( "Link")
链结也可以使用阵列来实作，不过在这边我们以动态记忆体宣告的方式来进行，在C语言中，这是实作链结的基本作法，可以不受阵列大小必须先行宣告的限制，所以使用链结实作堆叠时，就不会有堆叠已满的问题（除了记忆体用尽之外）。
上面的图说只是个示意，在演算的时候，会需要一些暂存变数来协助节点新增与删除时的连结更动，请直接参照以下的程式实作。

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
struct node {
int data;
struct node *next;
};
typedef struct node getnode;
getnode* creates(void); // 建立堆叠
int isEmpty(getnode*); // 堆叠已空
int stacktop(getnode*); // 传回顶端元素
getnode* add(getnode*, int); // 新增元素
getnode* delete(getnode*); // 删除元素
void list(getnode*); // 显示所有内容
int main(void) {
getnode* sTop;
int input, select;
sTop = creates();
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
sTop = add(sTop, input);
break;
case 2:
printf("\n顶端值：%d", stacktop(sTop));
break;
case 3:
sTop = delete(sTop);
break;
case 4:
list(sTop);
break;
default:
printf("\n选项错误！");
}
}
printf("\n");
return 0;
}
getnode* creates() {
return NULL;
}
int isEmpty(getnode* top) {
return (top == NULL);
}
int stacktop(getnode* top) {
return top->data;
}
getnode* add(getnode* top, int item) {
getnode* newnode;
newnode = (getnode*) malloc(sizeof(getnode));
if(newnode == NULL) {
printf("\n记忆体配置失败！");
exit(1);
}
newnode->data = item;
newnode->next = top;
top = newnode;
return top;
}
getnode* delete(getnode* top) {
getnode* tmpnode;
tmpnode = top;
if(tmpnode == NULL) {
printf("\n堆叠已空！");
return NULL;
}
top = top->next;
free(tmpnode);
return top;
}
void list(getnode* top) {
getnode* tmpnode;
tmpnode = top;
printf("\n堆叠内容：");
while(tmpnode != NULL) {
printf("%d ", tmpnode->data);
tmpnode = tmpnode->next;
}
}
