---
layout: post
title: "MYSQL不能从远程连接的一个解决方法"
categories: DB
tags: 
 - DB
 - mysql
--- 

# MYSQL不能从远程连接的一个解决方法

所有栏目 服务器 社交网络 电子商务 注册破解 网络资讯 本站作品 搜索引擎 酷站欣赏 安全防护 精品美文 整站程序 入侵检测 精品美图 ASP程序 精美整站 常用技巧 免费空间 架站技术 搜索引擎 PHP程序 新闻文章 免费电影 精美音乐 平面设计 站长心得 .NET程序 精品MV 社区论坛 DIV+CSS 有奖活动 数据库技术 网络常识 其他免费 JSP程序 服务器安全 JavaScript 生活常识 Vbscript VB程序 其他程序  标题 作者 内容

* [首页](http://www.haishui.net/index.php)
* [热点资讯](http://www.haishui.net/list.php?tid=53)
* [免费资源](http://www.haishui.net/list.php?tid=12)
* [网络编程](http://www.haishui.net/list.php?tid=11)
* [站长天地](http://www.haishui.net/list.php?tid=10)
* [网页设计](http://www.haishui.net/list.php?tid=16)
* [网络安全](http://www.haishui.net/list.php?tid=15)
* [精品赏析](http://www.haishui.net/list.php?tid=13)
* [程序下载](http://www.haishui.net/list.php?tid=17)
* [代理IP](http://www.haishui.net/proxy/index.php)
[死灰复燃那天才会有更新改版！ [08-05-19 14:49:51]](http://www.haishui.net/list.php)

当前位置：[首 页](http://www.haishui.net/index.php) >> [站长天地](http://www.haishui.net/list.php?tid=10) >> [数据库技术](http://www.haishui.net/list.php?tid=30) >> [MYSQL不能从远程连接的一个解决方法](http://www.haishui.net/view.php?tid=30&id=483) **MYSQL不能从远程连接的一个解决方法** 搜集者：海水 发布时间：06-03-13 浏览次数：12483 [[大]() [中]() [小]()] 如果你想连接你的mysql的时候发生这个错误：
以下是引用内容：
ERROR 1130: Host '192.168.1.3' is not allowed to connect to this MySQL server
解决方法：
1。 改表法。可能是你的帐号不允许从远程登陆，只能在localhost。这个时候只要在localhost的那台电脑，登入mysql后，更改 "mysql" 数据库里的 "user" 表里的 "host" 项，从"localhost"改称"%"
mysql -u root -pvmwaremysql>use mysql;mysql>update user set host = '%' where user = 'root';mysql>select host, user from user;
2. 授权法。例如，你想myuser使用mypassword从任何主机连接到mysql服务器的话。
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
如果你想允许用户myuser从ip为192.168.1.3的主机连接到mysql服务器，并使用mypassword作为密码
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'192.168.1.3' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;    相关附件：无  相关文章[MYSQL]：

* [05-10-09] [MYSQL 5.0.13 RC发布](http://www.haishui.net/view.php?tid=30&id=13 " MYSQL 5.0.13 RC发布")
* [05-11-23] [配置最新的PHP加MYSQL服务器](http://www.haishui.net/view.php?tid=1&id=165 "配置最新的PHP加MYSQL服务器")
* [05-12-03] [MYSQL出错代码列表](http://www.haishui.net/view.php?tid=30&id=184 "MYSQL出错代码列表")
* [06-01-27] [免费200MB，PHP，MYSQL，CGI，FTP空间](http://www.haishui.net/view.php?tid=39&id=326 "免费200MB，PHP，MYSQL，CGI，FTP空间")
* [06-09-29] [MYSQL常见出错代码](http://www.haishui.net/view.php?tid=30&id=893 "MYSQL常见出错代码") 下一篇：[06-03-13] [一个馒头引发的血案](http://www.haishui.net/view.php?tid=58&id=484 "一个馒头引发的血案") 上一篇：[06-03-13] [女生教你怎么追MM--送给没有女朋友的朋友](http://www.haishui.net/view.php?tid=5&id=482 "女生教你怎么追MM--送给没有女朋友的朋友") 相关评论： 添加评论:  Email: 验证码: ![]() 评论[最多255字节]:

## 广告时间

## 搜索信息

输入您的搜索字词 站内搜索 网络搜索
提交搜索表单

## 置顶文章

## 推荐文章
## 点击排行

* [MySQL开发中的外键与参照完整性](http://www.haishui.net/view.php?tid=30&id=719 "MySQL开发中的外键与参照完整性")
* [MYSQL不能从远程连接的一个解决方法](http://www.haishui.net/view.php?tid=30&id=483 "MYSQL不能从远程连接的一个解决方法")
* [SQL Server 2000安装图解](http://www.haishui.net/view.php?tid=30&id=150 "SQL Server 2000安装图解")
* [忘记MySQL密码的更改方法](http://www.haishui.net/view.php?tid=30&id=215 "忘记MySQL密码的更改方法")
* [远程连接sql server 2000服务器的解决方案..](http://www.haishui.net/view.php?tid=30&id=419 "远程连接sql server 2000服务器的解决方案")
* [MySQL字段类型说明](http://www.haishui.net/view.php?tid=30&id=720 "MySQL字段类型说明")
* [MYSQL 5.0.13 RC发布](http://www.haishui.net/view.php?tid=30&id=13 " MYSQL 5.0.13 RC发布")
版权所有 海水工作室 程序支持 海水 & Spring 界面风格 MacroLong    [晋ICP备05007933号](http://www.miibeian.gov.cn/)

本站所有信息均转载自网络，供大家阅读、使用，不负责任何侵权行为。
  
