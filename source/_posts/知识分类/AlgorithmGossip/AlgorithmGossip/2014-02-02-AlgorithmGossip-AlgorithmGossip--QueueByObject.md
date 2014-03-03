---
layout: post
title: "QueueByObject"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# QueueByObject

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 伫列 - 使用Java 作物件封装]()

## 说明

如果您使用C++或Java等支援物件导向的语言来实作伫列，您可以使用类别的方式来包括伫列的功能，将所有的伫列操作由堆叠物件来处理，一旦包装完成，则使用伫列物件的时候，只要呼叫加入、删除等方法，而无需处理伫列的front、rear或判断是否为空等细节。

## 解法

使用C++与使用Java来作类别包装其实是类似的，在这边我们使用Java实作，因为它的语法看来较简洁；Java虽然没有指标，但可以使用参考（Reference）来达到链结的效果，一个节点的类别包装方式如下：
class Node {
    private int data;   // 节点资料
    private Node next;  // 下一个节点位置
    public void setData(int data);  // 节点资料
    public void setNext(Node next);  // 下一个节点位置
    public int getData();  // 传回节点资料
    public Node getNext();  // 传回下一个节点位置
}
  
其中next是个物件参考名称，它可以用来参考至（指向）下一个节点物件的记忆体位置，而伫列类别可以如下包装：
class Queue {
    private Node front;
    private Node rear;
    private String name;  // 只是个名称
    // 利用建构子建立伫列
    public Queue();
    public Queue(String name);
    // 插入资料至前端
    public void add(int data);
    // 显示前端资料
    public void printFront();
    // 删除前端资料
    public void del();
    // 列出伫列内容
    public void list();
}
  
利用物件导向来包装资料结构，虽然在设计时需要花较多的心思，但设计完成之后，日后呼叫使用就简单了，以后您只要注意主程式的逻辑设计就可以了。

## 实作

* Java
import java.io.*;
// 节点
class Node {
private int data; // 节点资料
private Node next; // 下一个节点位置
public void setData(int data) {
this.data = data;
}
public void setNext(Node next) {
this.next = next;
}
public int getData() {
return data;
}
public Node getNext() {
return next;
}
}
// 伫列
class Queue {
private Node front;
private Node rear;
private String name; // 只是个名称
public Queue() {
this("list");
}
// 利用建构子建立伫列
public Queue(String name) {
this.name = name;
front = new Node();
front.setNext(null);
rear = front;
}
// 插入资料至顶端
public void add(int data) {
Node newNode = new Node();
if(front.getNext() == null)
front.setNext(newNode);
newNode.setData(data);
newNode.setNext(null);
rear.setNext(newNode);
rear = newNode;
}
// 显示前端资料
public void printFront() {
if(front.getNext() == null)
System.out.print("\n伫列为空！");
else
System.out.print("\n顶端值：" +
front.getNext().getData());
}
// 删除前端资料
public void del() {
Node tmpNode;
if(front.getNext() == null) {
System.out.print("\n伫列已空！");
return;
}
tmpNode = front.getNext();
front.setNext(tmpNode.getNext());
tmpNode = null;
}
// 列出伫列内容
public void list() {
Node tmpNode;
tmpNode = front.getNext();
System.out.print("\n伫列内容：");
while(tmpNode != null) {
System.out.print(tmpNode.getData() + " ");
tmpNode = tmpNode.getNext();
}
}
}
public class Queueshow {
public static void main(String[] args)
throws IOException {
int input, select;
BufferedReader buf;
buf = new BufferedReader(
new InputStreamReader(System.in));
Queue q1 = new Queue("伫列测试");
while(true) {
System.out.print("\n\n请输入选项(-1结束)：");
System.out.print("\n(1)插入值至伫列");
System.out.print("\n(2)显示伫列前端");
System.out.print("\n(3)删除前端值");
System.out.print("\n(4)显示所有内容");
System.out.print("\n$c>");
select = Integer.parseInt(buf.readLine());
if(select == -1)
break;
switch(select) {
case 1:
System.out.print("\n输入值：");
input = Integer.parseInt(buf.readLine());
q1.add(input);
break;
case 2:
q1.printFront();
break;
case 3:
q1.del();
break;
case 4:
q1.list();
break;
default:
System.out.print("\n选项错误！");
}
}
System.out.println("");
}
}
