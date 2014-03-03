---
layout: post
title: "MatchString"
categories: AlgorithmGossip
tags: 
 - AlgorithmGossip
 - AlgorithmGossip
--- 

# MatchString

### [# 面试时穿什么着装合适，这里找答案! #](http://taobao.esmartweb.com/man.htm)

# [Algorithm Gossip: 字串核对]()

##  说明

今日的一些高阶程式语言对于字串的处理支援越来越强大（例如Java、Perl等），不过字串搜寻本身仍是个值得探讨的课题，在这边以Boyer- Moore法来说明如何进行字串说明，这个方法快且原理简洁易懂。

## 解法

字串搜寻本身不难，使用暴力法也可以求解，但如何快速搜寻字串就不简单了，传统的字串搜寻是从关键字与字串的开头开始比对，例如 [Knuth-Morris-Pratt 演算法](http://www.ics.uci.edu/%7Eeppstein/161/960227.html) 字串搜寻，这个方法也不错，不过要花时间在公式计算上；Boyer-Moore字串核对改由关键字的后面开始核对字串，并制作前进表，如果比对不符合则依前进表中的值前进至下一个核对处，假设是p好了，然后比对字串中p-n+1至p的值是否与关键字相同。
![字串核对]( "字串核对")
那么前进表该如何前进，举个实际的例子，如果要在字串中搜寻JUST这个字串，则可能遇到的几个情况如下所示：
![字串核对]( "字串核对")
 依照这个例子，可以决定出我们的前进值表如下：
其它 J U S T 4 3 2 1 4（match?）
如果关键字中有重复出现的字元，则前进值就会有两个以上的值，此时则取前进值较小的值，如此就不会跳过可能的位置，例如texture这个关键字，t的前 进值应该取后面的3而不是取前面的7。

## 实作

* C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
void table(char*); // 建立前进表
int search(int, char*, char*); // 搜寻关键字
void substring(char*, char*, int, int); // 取出子字串
int skip[256];
int main(void) {
char str_input[80];
char str_key[80];
char tmp[80] = {'\0'};
int m, n, p;
printf("请输入字串：");
gets(str_input);
printf("请输入搜寻关键字：");
gets(str_key);
m = strlen(str_input); // 计算字串长度
n = strlen(str_key);
table(str_key);
p = search(n-1, str_input, str_key);
while(p != -1) {
substring(str_input, tmp, p, m);
printf("%s\n", tmp);
p = search(p+n+1, str_input, str_key);
}
printf("\n");
return 0;
}
void table(char *key) {
int k, n;
n = strlen(key);
for(k = 0; k <= 255; k++)
skip[k] = n;
for(k = 0; k < n - 1; k++)
skip[key[k]] = n - k - 1;
}
int search(int p, char* input, char* key) {
int i, m, n;
char tmp[80] = {'\0'};
m = strlen(input);
n = strlen(key);
while(p < m) {
substring(input, tmp, p-n+1, p);
if(!strcmp(tmp, key)) // 比较两字串是否相同
return p-n+1;
p += skip[input[p]];
}
return -1;
}
void substring(char *text, char* tmp, int s, int e) {
int i, j;
for(i = s, j = 0; i <= e; i++, j++)
tmp[j] = text[i];
tmp[j] = '\0';
}

* Java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
public class StringMatch {
private int[] skip;
private int p;
private String str;
private String key;
public StringMatch(String key) {
skip = new int[256];
this.key = key;
for(int k = 0; k <= 255; k++)
skip[k] = key.length();
for(int k = 0; k < key.length() - 1; k++)
skip[key.charAt(k)] = key.length() - k - 1;
}
public void search(String str) {
this.str = str;
p = search(key.length()-1, str, key);
}
private int search(int p, String input, String key) {
while(p < input.length()) {
String tmp = input.substring(
p-key.length()+1, p+1);
if(tmp.equals(key)) // 比较两字串是否相同
return p-key.length()+1;
p += skip[input.charAt(p)];
}
return -1;
}
public boolean hasNext() {
return (p != -1);
}
public String next() {
String tmp = str.substring(p);
p = search(p+key.length()+1, str, key);
return tmp;
}
public static void main(String[] args)
throws IOException {
BufferedReader bufReader =
new BufferedReader(
new InputStreamReader(System.in));
System.out.print("请输入字串：");
String str = bufReader.readLine();
System.out.print("请输入搜寻关键字：");
String key = bufReader.readLine();
StringMatch strMatch = new StringMatch(key);
strMatch.search(str);
while(strMatch.hasNext()) {
System.out.println(strMatch.next());
}
}
}
