---
layout: post
title: "BigNumber"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# BigNumber

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 超长整数运算（大数运算）]()

## 说明

基于记忆体的有效运用，程式语言中规定了各种不同的资料型态，也因此变数所可以表达的最大整数受到限制，例如123456789123456789这样的 整数就不可能储存在long变数中（例如C/C++等），我们称这为long数，这边翻为超长整数（避免与资料型态的长整数翻译混淆），或俗称大数运算。

## 解法

一个变数无法表示超长整数，则就使用多个变数，当然这使用阵列最为方便，假设程式语言的最大资料型态可以储存至65535的数好了，为了计算方便及符合使用十进位制的习惯，让每一个阵列元素可以储存四个位数，也就是0到9999的数，例如：
![大数运算]( "大数运算")
很多人问到如何计算像50!这样的问题，解法就是使用程式中的乘法函式，至于要算到多大，就看需求了。
如果您使用的是Java，那么在java.lang下有BigInteger与BigDecimal可以直接进行大数运算。
由于使用阵列来储存数值，关于数值在运算时的加减乘除等各种运算、位数的进位或借位就必须自行定义，加、减、乘都是由低位数开始运算，而除法则是由高位数开始运算，这边直接提供加减乘除运算的函式供作参考，以下的N为阵列长度。

## 实作

* C
void add(int *a, int *b, int *c) {
int i, carry = 0;
for(i = N - 1; i >= 0; i--) {
c[i] = a[i] + b[i] + carry;
if(c[i] < 10000)
carry = 0;
else { // 进位
c[i] = c[i] - 10000;
carry = 1;
}
}
}
void sub(int *a, int *b, int *c) {
int i, borrow = 0;
for(i = N - 1; i >= 0; i--) {
c[i] = a[i] - b[i] - borrow;
if(c[i] >= 0)
borrow = 0;
else { // 借位
c[i] = c[i] + 10000;
borrow = 1;
}
}
}
void mul(int *a, int b, int *c) { // b 为乘数
int i, tmp, carry = 0;
for(i = N - 1; i >=0; i--) {
tmp = a[i] * b + carry;
c[i] = tmp % 10000;
carry = tmp / 10000;
}
}
void div(int *a, int b, int *c) { // b 为除数
int i, tmp, remain = 0;
for(i = 0; i < N; i++) {
tmp = a[i] + remain;
c[i] = tmp / b;
remain = (tmp % b) * 10000;
}
}

* Java
public class BigNumber {
public static int[] add(int[] a, int[] b) {
int carry = 0;
int[] c = new int[a.length];
for(int i = a.length - 1; i >= 0; i--) {
c[i] = a[i] + b[i] + carry;
if(c[i] < 10000)
carry = 0;
else { // 进位
c[i] = c[i] - 10000;
carry = 1;
}
}
return c;
}
public static int[] sub(int[] a, int[] b) {
int borrow = 0;
int[] c = new int[a.length];
for(int i = a.length - 1; i >= 0; i--) {
c[i] = a[i] - b[i] - borrow;
if(c[i] >= 0)
borrow = 0;
else { // 借位
c[i] = c[i] + 10000;
borrow = 1;
}
}
return c;
}
public static int[] mul(int[] a, int b) { // b 为乘数
int carry = 0;
int[] c = new int[a.length];
for(int i = a.length - 1; i >=0; i--) {
int tmp = a[i] * b + carry;
c[i] = tmp % 10000;
carry = tmp / 10000;
}
return c;
}
public static int[] div(int[] a, int b) { // b 为除数
int remain = 0;
int[] c = new int[a.length];
for(int i = 0; i < a.length; i++) {
int tmp = a[i] + remain;
c[i] = tmp / b;
remain = (tmp % b) * 10000;
}
return c;
}
public static void main(String[] args) {
int[] a = {1234, 5678, 9910, 1923, 1124};
int[] b = {1234, 5678, 9910, 1923, 1124};
int[] c = BigNumber.add(a, b);
for(int i = 0; i < c.length; i++) {
System.out.print(c[i]);
}
System.out.println();
}
}
