---
layout: post
title: "StackByObject"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# StackByObject

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 堆叠 - 使用 Java 作物件封装]()

## 说明

如果您使用C++或Java等支援物件导向的语言来实作堆叠，您可以使用类别的方式来包括堆叠的功能，将所有的堆叠操作由堆叠物件来处理，一旦包装完成，则使用堆叠物件的时候，只要呼叫加入、删除等方法，而无需处理堆叠的top或判断是否为空等细节。

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
  
其中next是个物件参考名称，它可以用来参考至（指向）下一个节点物件的记忆体位置，而堆叠类别可以如下包装：
class Stack {
    private Node top;    // 堆叠顶端
    private String name;  // 只是个名称
    // 利用建构子建立堆叠
    public Stack();
    public Stack(String name);
    // 插入资料至顶端
    public void add(int data);
    // 传回顶端资料
    public int printTop();
    // 删除顶端资料
    public void del();
    // 列出堆叠内容
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
public void setData(int data) { // 节点资料
this.data = data;
}
public void setNext(Node next) { // 下一个节点位置
this.next = next;
}
public int getData() { // 传回节点资料
return data;
}
public Node getNext() { // 传回下一个节点位置
return next;
}
}
// 堆叠
class Stack {
private Node top;
private String name; // 只是个名称
public Stack() {
this("list");
}
// 利用建构子建立堆叠
public Stack(String name) {
this.name = name;
top = null;
}
// 插入资料至顶端
public void add(int data) {
Node newNode = new Node();
newNode.setData(data);
newNode.setNext(top);
top = newNode;
}
// 传回顶端资料
public int printTop() {
return top.getData();
}
// 删除顶端资料
public void del() {
Node tmpNode;
tmpNode = top;
if(tmpNode == null) {
System.out.println("\n堆叠已空！");
return;
}
top = top.getNext();
tmpNode = null;
}
// 列出堆叠内容
public void list() {
Node tmpNode;
tmpNode = top;
System.out.print("\n堆叠内容：");
while(tmpNode != null) {
System.out.print(tmpNode.getData() + " ");
tmpNode = tmpNode.getNext();
}
}
}
public class StackShow {
public static void main(String[] args)
throws IOException {
int input, select;
BufferedReader buf;
buf = new BufferedReader(
new InputStreamReader(System.in));
Stack s1 = new Stack("堆叠测试");
while(true) {
System.out.print("\n\n请输入选项(-1结束)：");
System.out.print("\n(1)插入值至堆叠");
System.out.print("\n(2)显示堆叠顶端");
System.out.print("\n(3)删除顶端值");
System.out.print("\n(4)显示所有内容");
System.out.print("\n$c>");
select = Integer.parseInt(buf.readLine());
if(select == -1)
break;
switch(select) {
case 1:
System.out.print("\n输入值：");
input = Integer.parseInt(buf.readLine());
s1.add(input);
break;
case 2:
System.out.print("\n顶端值：" +
s1.printTop());
break;
case 3:
s1.del();
break;
case 4:
s1.list();
break;
default:
System.out.print("\n选项错误！");
}
}
System.out.println("");
}
}
