---
layout: post
title: "PascalTriangle"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# PascalTriangle

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 巴斯卡三角形]()

巴斯卡（Pascal）三角形基本上就是在解 nCr ，因为三角形上的每一个数字各对应一个nCr，其中 n 为 row，而 r 为 column，如下：
0C0
1C0 1C1
2C0 2C1 2C2
3C0 3C1 3C2 3C3
4C0 4C1 4C2 4C3 4C4
对应的数据如下图所示：
![巴斯卡三角]( "巴斯卡三角")
##  解法
巴斯卡三角形中的 nCr 可以使用以下这个公式来计算，以避免阶乘运算时的数值溢位：
nCr = [(n-r+1)/r] * nCr-1
nC0 = 1 

## 演算法

/* 计算nCr，但是并不快，只是方便 */
Procedure COMBI(n, r) [
FOR(i = 1; i <= r; i = i + 1)
p = p * (n-i+1) / i;
RETURN p;
]
解决 nCr 的算法之后，剩下的就是如何将这些数字排版成三角形的问题了，这就要看您是如何显示成果的了，下面的程式将分别示范文字模式（C实作）与视窗模式（Java实作）的解法。

## 实作

* C
#include <stdio.h>
#define N 12
long combi(int n, int r){
int i;
long p = 1;
for(i = 1; i <= r; i++)
p = p * (n-i+1) / i;
return p;
}
void paint() {
int n, r, t;
for(n = 0; n <= N; n++) {
for(r = 0; r <= n; r++) {
int i;
/* 排版设定开始 */
if(r == 0) {
for(i = 0; i <= (N-n); i++) {
printf(" ");
}
}
else {
printf(" ");
} /* 排版设定结束 */
printf("%3d", combi(n, r));
}
printf("\n");
}
}
int main() {
paint();
return 0;
}

* Java
import java.awt.*;
import javax.swing.*;
public class Pascal extends JFrame {
public Pascal() {
setBackground(Color.white);
setTitle("巴斯卡三角形");
setSize(520, 350);
setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
show();
}
private long combi(int n, int r){
int i;
long p = 1;
for(i = 1; i <= r; i++)
p = p * (n-i+1) / i;
return p;
}
public void paint(Graphics g) {
final int N = 12;
int n, r, t;
for(n = 0; n <= N; n++) {
for(r = 0; r <= n; r++)
g.drawString(" " + combi(n, r),
(N-n)*20 + r * 40, n * 20 + 50);
}
}
public static void main(String args[]) {
Pascal frm = new Pascal();
}
}
