---
layout: post
title: "本地phpmyadmin访问远程mysql数据库"
categories: DB
tags: 
 - DB
 - mysql
--- 

# 本地phpmyadmin访问远程mysql数据库

**方法：**

**1、在本地访问phpmyadmin 可查看远程mysql数据库,phpMyAdmin 3.3.1只需要修改 $cfg['Servers'][$i]['host']的值;**

**2、修改**libraries目录下的config.default.php(写字板打开)文件中的**AllowArbitraryServer为真即可（$cfg['AllowArbitraryServer'] = true;）;**

‍
