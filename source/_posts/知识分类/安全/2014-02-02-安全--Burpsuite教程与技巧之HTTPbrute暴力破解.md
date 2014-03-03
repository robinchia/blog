---
layout: post
title: "Burpsuite教程与技巧之HTTP brute暴力破解"
categories: 安全
tags: 
 - 安全
--- 

# Burpsuite教程与技巧之HTTP brute暴力破解

### Burpsuite教程与技巧之HTTP brute暴力破解

[](http://www.freebuf.com/articles/web/7457.html#)[Gall](http://www.freebuf.com/author/gall "由 Gall 发布") @ [WEB安全](http://www.freebuf.com/category/articles/web "查看 WEB安全 中的全部文章") 2013-02-28 共 **5644** 人围观

感谢[Gall](http://www.freebuf.com/author/gall)投递

**常规的对username/passwprd进行payload测试，我想大家应该没有什么问题，但****对于Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=这样的问题，很多朋友疑惑了.**

之前，我记得我介绍过burpsuite的intruder功能([BurpSuite教程与技巧之SQL Injection](http://www.freebuf.com/articles/5560.html))，想必很多人没什么印象，在此，以HTTP brute重提intruder功能.

以下面案例进行说明(只作演示之用，具体以自己的目标为准)

![]()

Auth=dXNlcjpwYXNzd29yZA==处，也就是我们的关键位置.

那么具体该如何做呢?大致操作过程如下:

1.解密base64字符串2.生成测试用的payload3.利用payload进行测试

**1.解密验证用的base64字符串**

![]()

解密后的字符串为:

Auth=user:password

问题来了，针对user:password这种形式的字符串，我们该如何设置payload呢?

想必很多人在此处了费尽心思。为了解决这个问题，接下来请看第二部分。

**2.生成测试用的payload**

对于这种格式，无法利用burpsuite顺利的完成测试，那个就需要丰富对应的payload了.

我的做法就是，利用burpsuite生成我要的payload文本.

Auth=§user§§:§§password§

设置3处payloads，

1 ------ §user§2 ------ §:§3 ------ §password§

然后根据intruder自带的battering ram/pitchfork/cluster bomb生成payloads(根据自己的需求生成)

我在此处选择以cluster bomb为例，利用intruder生成需要的payloads，然后保存到文本文件中.

![]()

![]()

**3.利用payload进行测试**

测试的时候，我们选用sniper，我们只需一个payload变量

![]()

![]()

![]()

若有不足之处，欢迎指正.
