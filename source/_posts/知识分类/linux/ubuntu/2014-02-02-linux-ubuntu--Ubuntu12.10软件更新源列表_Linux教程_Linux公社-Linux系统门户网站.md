---
layout: post
title: "Ubuntu 12.10 软件更新源列表_Linux教程_Linux公社-Linux系统门户网站"
categories: linux
tags: 
 - linux
 - ubuntu
--- 

# Ubuntu 12.10 软件更新源列表_Linux教程_Linux公社-Linux系统门户网站

### 分享到

* [一键分享](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [QQ空间](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [新浪微博](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [百度搜藏](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [人人网](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [腾讯微博](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [百度相册](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [开心网](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [腾讯朋友](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [百度贴吧](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [豆瓣网](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [搜狐微博](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [百度新首页](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [QQ](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [和讯微博](http://www.linuxidc.com/Linux/2012-10/73114.htm#)
* [更多...](http://www.linuxidc.com/Linux/2012-10/73114.htm#)

[百度分享](http://www.linuxidc.com/Linux/2012-10/73114.htm#)

[繁體]( "點擊以繁體中文方式浏覽")

你好，游客 [登录](http://www.linuxidc.com/Linux/2012-10/73114.htm#) [注册](http://www.linuxidc.com/memberreg.aspx) [发布](http://www.linuxidc.com/membernewsadd.aspx)[搜索](http://www.linuxidc.com/search.aspx)
[![Linux公社]()](http://www.linuxidc.com/)
[![Android专题]()](http://www.linuxidc.com/topicnews.aspx?tid=11 "Android专题")

[首页](http://www.linuxidc.com/index.htm)[Linux新闻](http://www.linuxidc.com/it/)[Linux教程](http://www.linuxidc.com/Linuxit/)[数据库技术](http://www.linuxidc.com/MySql/)[Linux编程](http://www.linuxidc.com/RedLinux/)[服务器应用](http://www.linuxidc.com/Apache/)[Linux安全](http://www.linuxidc.com/Unix/)[Linux下载](http://www.linuxidc.com/download/)[Linux认证](http://www.linuxidc.com/Linuxrz/)[Linux主题](http://www.linuxidc.com/theme/)[Linux壁纸](http://www.linuxidc.com/Linuxwallpaper/)[Linux软件](http://www.linuxidc.com/linuxsoft/)[数码](http://www.linuxidc.com/digi/)[手机](http://www.linuxidc.com/mobile/)[电脑](http://www.linuxidc.com/diannao/)
[首页](http://www.linuxidc.com/index.htm) → [Linux教程](http://www.linuxidc.com/Linuxit/)

[![]()](http://www.yutianedu.com/list.asp?Unid=22239) [![]()](http://www.boxue.com.cn/)
背景：![#EDF0F5]() ![#FAFBE6]() ![#FFF2E2]() ![#FDE6E0]() ![#F3FFE1]() ![#DAFAF3]() ![#EAEAEF]() ![默认]() 阅读新闻

# Ubuntu 12.10 软件更新源列表

 [日期：2012-10-28] 来源：Linux公社  作者：Linux [字体：[大]() [中]() [小]()]

[![]()](http://www.upemb.com/page/uealinuxidc12531.php "尚观Linux")
[Ubuntu](http://www.linuxidc.com/topicnews.aspx?tid=2 "Ubuntu") 12.10也正式发布了， 安装好后第一件事就是更换源，Ubuntu网易的更新源速度很不错。

Ubuntu 12.10正式版发布下载  [http://www.linuxidc.com/Linux/2012-10/72581.htm](http://www.linuxidc.com/Linux/2012-10/72581.htm)

废话少说， 上源：

首先，备份一下Ubuntu 12.10 原来的源地址列表文件

sudo cp /etc/apt/sources.list /etc/apt/sources.list.old

然后进行修改
sudo gedit /etc/apt/sources.list

可以在里面添加资源地址，我是直接覆盖掉原来的。

下面是网上找到的一些较好的源，有大型网站的，也有教育网的，可以根据自己的情况添加两三个即可。

#网易的源（163源，无论是不是教育网，速度都很快）
deb http://mirrors.163.com/ubuntu/ quantal main universe restricted multiverse
deb-src http://mirrors.163.com/ubuntu/ quantal main universe restricted multiverse
deb http://mirrors.163.com/ubuntu/ quantal-security universe main multiverse restricted
deb-src http://mirrors.163.com/ubuntu/ quantal-security universe main multiverse restricted
deb http://mirrors.163.com/ubuntu/ quantal-updates universe main multiverse restricted
deb http://mirrors.163.com/ubuntu/ quantal-proposed universe main multiverse restricted
deb-src http://mirrors.163.com/ubuntu/ quantal-proposed universe main multiverse restricted
deb http://mirrors.163.com/ubuntu/ quantal-backports universe main multiverse restricted
deb-src http://mirrors.163.com/ubuntu/ quantal-backports universe main multiverse restricted
deb-src http://mirrors.163.com/ubuntu/ quantal-updates universe main multiverse restricted

#搜狐的源（sohu 源今天还没有更新，不过应该快了）
deb http://mirrors.sohu.com/ubuntu/ quantal main restricted
deb-src http://mirrors.sohu.com/ubuntu/ quantal main restricted
deb http://mirrors.sohu.com/ubuntu/ quantal-updates main restricted
deb-src http://mirrors.sohu.com/ubuntu/ quantal-updates main restricted
deb http://mirrors.sohu.com/ubuntu/ quantal universe
deb-src http://mirrors.sohu.com/ubuntu/ quantal universe
deb http://mirrors.sohu.com/ubuntu/ quantal-updates universe
deb-src http://mirrors.sohu.com/ubuntu/ quantal-updates universe
deb http://mirrors.sohu.com/ubuntu/ quantal multiverse
deb-src http://mirrors.sohu.com/ubuntu/ quantal multiverse
deb http://mirrors.sohu.com/ubuntu/ quantal-updates multiverse
deb-src http://mirrors.sohu.com/ubuntu/ quantal-updates multiverse
deb http://mirrors.sohu.com/ubuntu/ quantal-backports main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ quantal-backports main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ quantal-security main restricted
deb-src http://mirrors.sohu.com/ubuntu/ quantal-security main restricted
deb http://mirrors.sohu.com/ubuntu/ quantal-security universe
deb-src http://mirrors.sohu.com/ubuntu/ quantal-security universe
deb http://mirrors.sohu.com/ubuntu/ quantal-security multiverse
deb-src http://mirrors.sohu.com/ubuntu/ quantal-security multiverse
deb http://extras.ubuntu.com/ubuntu quantal main
deb-src http://extras.ubuntu.com/ubuntu quantal main

#台湾源（台湾的ubuntu 更新源还是很给力的）
deb http://tw.archive.ubuntu.com/ubuntu/ quantal main universe restricted multiverse
deb-src http://tw.archive.ubuntu.com/ubuntu/ quantal main universe restricted multiverse
deb http://tw.archive.ubuntu.com/ubuntu/ quantal-security universe main multiverse restricted
deb-src http://tw.archive.ubuntu.com/ubuntu/ quantal-security universe main multiverse restricted
deb http://tw.archive.ubuntu.com/ubuntu/ quantal-updates universe main multiverse restricted
deb-src http://tw.archive.ubuntu.com/ubuntu/ quantal-updates universe main multiverse restricted

#骨头源，骨头源是bones7456架设的一个Ubuntu源 ，提供ubuntu,deepin
deb http://ubuntu.srt.cn/ubuntu/ quantal main universe restricted multiverse
deb-src http://ubuntu.srt.cn/ubuntu/ quantal main universe restricted multiverse
deb http://ubuntu.srt.cn/ubuntu/ quantal-security universe main multiverse restricted
deb-src http://ubuntu.srt.cn/ubuntu/ quantal-security universe main multiverse restricted
deb http://ubuntu.srt.cn/ubuntu/ quantal-updates universe main multiverse restricted
deb http://ubuntu.srt.cn/ubuntu/ quantal-proposed universe main multiverse restricted
deb-src http://ubuntu.srt.cn/ubuntu/ quantal-proposed universe main multiverse restricted
deb http://ubuntu.srt.cn/ubuntu/ quantal-backports universe main multiverse restricted
deb-src http://ubuntu.srt.cn/ubuntu/ quantal-backports universe main multiverse restricted
deb-src http://ubuntu.srt.cn/ubuntu/ quantal-updates universe main multiverse restricted

#ubuntu.cn99.com源（推荐）:
deb http://ubuntu.cn99.com/ubuntu/ quantal main restricted universe multiverse
deb http://ubuntu.cn99.com/ubuntu/ quantal-updates main restricted universe multiverse
deb http://ubuntu.cn99.com/ubuntu/ quantal-security main restricted universe multiverse
deb http://ubuntu.cn99.com/ubuntu/ quantal-backports main restricted universe multiverse
deb http://ubuntu.cn99.com/ubuntu-cn/ quantal main restricted universe multiverse

#教育网源
#电子科技大学
deb http://ubuntu.uestc.edu.cn/ubuntu/ quantal main restricted universe multiverse
deb http://ubuntu.uestc.edu.cn/ubuntu/ quantal-backports main restricted universe multiverse
deb http://ubuntu.uestc.edu.cn/ubuntu/ quantal-proposed main restricted universe multiverse
deb http://ubuntu.uestc.edu.cn/ubuntu/ quantal-security main restricted universe multiverse
deb http://ubuntu.uestc.edu.cn/ubuntu/ quantal-updates main restricted universe multiverse
deb-src http://ubuntu.uestc.edu.cn/ubuntu/ quantal main restricted universe multiverse
deb-src http://ubuntu.uestc.edu.cn/ubuntu/ quantal-backports main restricted universe multiverse
deb-src http://ubuntu.uestc.edu.cn/ubuntu/ quantal-proposed main restricted universe multiverse
deb-src http://ubuntu.uestc.edu.cn/ubuntu/ quantal-security main restricted universe multiverse
deb-src http://ubuntu.uestc.edu.cn/ubuntu/ quantal-updates main restricted universe multiverse

#中国科技大学
deb http://debian.ustc.edu.cn/ubuntu/ quantal main restricted universe multiverse
deb http://debian.ustc.edu.cn/ubuntu/ quantal-backports restricted universe multiverse
deb http://debian.ustc.edu.cn/ubuntu/ quantal-proposed main restricted universe multiverse
deb http://debian.ustc.edu.cn/ubuntu/ quantal-security main restricted universe multiverse
deb http://debian.ustc.edu.cn/ubuntu/ quantal-updates main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ quantal main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ quantal-backports main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ quantal-proposed main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ quantal-security main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ quantal-updates main restricted universe multiverse
#北京理工大学
deb http://mirror.bjtu.edu.cn/ubuntu/ quantal main multiverse restricted universe
deb http://mirror.bjtu.edu.cn/ubuntu/ quantal-backports main multiverse restricted universe
deb http://mirror.bjtu.edu.cn/ubuntu/ quantal-proposed main multiverse restricted universe
deb http://mirror.bjtu.edu.cn/ubuntu/ quantal-security main multiverse restricted universe
deb http://mirror.bjtu.edu.cn/ubuntu/ quantal-updates main multiverse restricted universe
deb-src http://mirror.bjtu.edu.cn/ubuntu/ quantal main multiverse restricted universe
deb-src http://mirror.bjtu.edu.cn/ubuntu/ quantal-backports main multiverse restricted universe
deb-src http://mirror.bjtu.edu.cn/ubuntu/ quantal-proposed main multiverse restricted universe
deb-src http://mirror.bjtu.edu.cn/ubuntu/ quantal-security main multiverse restricted universe
deb-src http://mirror.bjtu.edu.cn/ubuntu/ quantal-updates main multiverse restricted universe

#兰州大学
deb ftp://mirror.lzu.edu.cn/ubuntu/ quantal main multiverse restricted universe
deb ftp://mirror.lzu.edu.cn/ubuntu/ quantal-backports main multiverse restricted universe
deb ftp://mirror.lzu.edu.cn/ubuntu/ quantal-proposed main multiverse restricted universe
deb ftp://mirror.lzu.edu.cn/ubuntu/ quantal-security main multiverse restricted universe
deb ftp://mirror.lzu.edu.cn/ubuntu/ quantal-updates main multiverse restricted universe
deb ftp://mirror.lzu.edu.cn/ubuntu-cn/ quantal main multiverse restricted universe

#上海交通大学（上海交大源，教育网的速度不用说了）
deb http://ftp.sjtu.edu.cn/ubuntu/ quantal main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ quantal-backports main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ quantal-proposed main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ quantal-security main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ quantal-updates main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu-cn/ quantal main multiverse restricted universe
deb-src http://ftp.sjtu.edu.cn/ubuntu/ quantal main multiverse restricted universe
deb-src http://ftp.sjtu.edu.cn/ubuntu/ quantal-backports main multiverse restricted universe
deb-src http://ftp.sjtu.edu.cn/ubuntu/ quantal-proposed main multiverse restricted universe
deb-src http://ftp.sjtu.edu.cn/ubuntu/ quantal-security main multiverse restricted universe
deb-src http://ftp.sjtu.edu.cn/ubuntu/ quantal-updates main multiverse restricted universe

添加好后保存，再输入 sudo apt-get update 就可以更新了，等着慢慢下载东西吧。

* 1
* [顶一下](http://www.linuxidc.com/Linux/2012-10/73114.htm###)
[](http://www.linuxidc.com/Linux/2012-10/73114.htm# "分享到新浪微博") [](http://www.linuxidc.com/Linux/2012-10/73114.htm# "分享到百度搜藏") [](http://www.linuxidc.com/Linux/2012-10/73114.htm# "分享到QQ空间") [](http://www.linuxidc.com/Linux/2012-10/73114.htm# "分享到腾讯微博") [](http://www.linuxidc.com/Linux/2012-10/73114.htm# "分享到人人网") [](http://www.linuxidc.com/Linux/2012-10/73114.htm# "分享到搜狐微博") [](http://www.linuxidc.com/Linux/2012-10/73114.htm# "分享到有道云笔记") [2](http://www.linuxidc.com/Linux/2012-10/73114.htm# "累计分享2次")
[Ubuntu Grub2启动上一次正确启动的内核](http://www.linuxidc.com/Linux/2012-10/73111.htm)

[Linux PAM make err : undefine yywrap()问题](http://www.linuxidc.com/Linux/2012-10/73115.htm)

相关资讯       [Ubuntu源](http://www.linuxidc.com/search.aspx?where=nkey&keyword=3408)  [ubuntu更新源](http://www.linuxidc.com/search.aspx?where=nkey&keyword=3835)  [Ubuntu 12.10更新源](http://www.linuxidc.com/search.aspx?where=nkey&keyword=14209)  [Ubuntu教育网更新源](http://www.linuxidc.com/search.aspx?where=nkey&keyword=14210) 

* [Ubuntu 12.04 Server 用户更新源](http://www.linuxidc.com/Linux/2012-07/64562.htm)  (07月07日)
* [Ubuntu 10.10更新源](http://www.linuxidc.com/Linux/2010-12/30938.htm)  (12/29/2010 20:10:37)
* [架设Ubuntu源时的两个脚本](http://www.linuxidc.com/Linux/2009-09/21827.htm)  (09/22/2009 05:48:40)

* [Ubuntu 11.10 教育网源](http://www.linuxidc.com/Linux/2011-11/47624.htm)  (11/20/2011 04:08:48)
* [Ubuntu 9.10更新源的添加和更新](http://www.linuxidc.com/Linux/2010-01/24170.htm)  (01/23/2010 12:09:55)
* [利用Nginx反向代理功能架设Ubuntu](http://www.linuxidc.com/Linux/2009-09/21812.htm "利用Nginx反向代理功能架设Ubuntu升级源")  (09/20/2009 14:38:44)
图片资讯      

* [![Ubuntu 10.10更新源]()Ubuntu 10.10更新源](http://www.linuxidc.com/Linux/2010-12/30938.htm)
* [![Ubuntu 9.10更新源的添加和更新]()Ubuntu 9.10更新源的](http://www.linuxidc.com/Linux/2010-01/24170.htm "Ubuntu 9.10更新源的添加和更新")

本文评论 　　[查看全部评论](http://www.linuxidc.com/remark.aspx?id=73114) (0)

表情： ![表情]() 姓名：  匿名 字数
点评：
同意评论声明 　　　发表评论声明

* 尊重网上道德，遵守中华人民共和国的各项有关法律法规
* 承担一切因您的行为而直接或间接导致的民事或刑事法律责任
* 本站管理人员有权保留或删除其管辖留言中的任意内容
* 本站有权在网站内转载或引用您的评论
* 参与本评论即表明您已经阅读并接受上述条款
Digg排行

* [10Ubuntu 12.04安装QQ2012](http://www.linuxidc.com/Linux/2012-05/59564.htm)
* [6Linux下除了某个文件外的其他文](http://www.linuxidc.com/Linux/2011-11/47705.htm "Linux下除了某个文件外的其他文件全部删除")
* [4在VMware虚拟机上安装Ubuntu 10.](http://www.linuxidc.com/Linux/2010-04/25829.htm "在VMware虚拟机上安装Ubuntu 10.04")
* [3如何从CentOS 6.0升级到CentOS 6](http://www.linuxidc.com/Linux/2012-10/71796.htm "如何从CentOS 6.0升级到CentOS 6.2")
* [3Win7用VMware安装Fedora 16](http://www.linuxidc.com/Linux/2012-06/63424.htm)
* [3Ubuntu 12.04 root用户登录设置](http://www.linuxidc.com/Linux/2012-05/60806.htm)
* [3虚拟机下安装BackTrack5 (BT5)教](http://www.linuxidc.com/Linux/2011-12/48609.htm "虚拟机下安装BackTrack5 (BT5)教程及BT5汉化")
* [2新安装 Ubuntu 12.10 需要做的](http://www.linuxidc.com/Linux/2012-10/71874.htm "新安装 Ubuntu 12.10 需要做的 10 件事")
* [2Ubuntu使用conky美化监测系统状](http://www.linuxidc.com/Linux/2012-09/71478.htm "Ubuntu使用conky美化监测系统状态")
* [2Ubuntu在顶部面板显示系统负载、](http://www.linuxidc.com/Linux/2012-09/71477.htm "Ubuntu在顶部面板显示系统负载、流量，磁盘I/O")

[![]()](http://www.linuxidc.com/Linux/2012-07/66157.htm "Oracle") [![LinuxIDC]()](http://www.linuxidc.net/) [![Ubuntu]()](http://www.linuxidc.com/search.aspx?Where=Nkey&Keyword=Ubuntu+11.10) [![]()](http://www.linuxidc.com/topicnews.aspx?tid=2)
最新资讯

* [Linux PAM make err : undefine yywrap()问题](http://www.linuxidc.com/Linux/2012-10/73115.htm)
* [Ubuntu 12.10 软件更新源列表]()
* [C# 使用定时任务 之 Timer类](http://www.linuxidc.com/Linux/2012-10/73113.htm)
* [C# 使用SQLite数据库](http://www.linuxidc.com/Linux/2012-10/73112.htm)
* [Ubuntu Grub2启动上一次正确启动的内核](http://www.linuxidc.com/Linux/2012-10/73111.htm)
* [Mozilla 向微软送蛋糕 祝贺其 IE10 发布](http://www.linuxidc.com/Linux/2012-10/73110.htm)
* [Wine 1.5.16 发布](http://www.linuxidc.com/Linux/2012-10/73109.htm)
* [FreeNAS 8.3.0 正式版发布](http://www.linuxidc.com/Linux/2012-10/73108.htm)
* [Debian 7.0 “Wheezy” Beta3 发布](http://www.linuxidc.com/Linux/2012-10/73107.htm)
* [Hadoop安装配置入门手册](http://www.linuxidc.com/Linux/2012-10/73106.htm)
* [配置Hive使用嵌入式derby或客服模式derby方法](http://www.linuxidc.com/Linux/2012-10/73105.htm)
* [Canonical发布Ubuntu Nexus 7 Desktop](http://www.linuxidc.com/Linux/2012-10/73104.htm "Canonical发布Ubuntu Nexus 7 Desktop Installer")

本周热门

* [在VMware虚拟机上安装Ubuntu 10.04](http://www.linuxidc.com/Linux/2010-04/25829.htm)
* [Ubuntu 12.04安装QQ2012](http://www.linuxidc.com/Linux/2012-05/59564.htm)
* [Fedora 12 LiveCD安装记录[图文]](http://www.linuxidc.com/Linux/2010-03/24993.htm)
* [Ubuntu中用VirtualBox虚拟机安装Windows XP完整](http://www.linuxidc.com/Linux/2010-09/28435.htm "Ubuntu中用VirtualBox虚拟机安装Windows XP完整图解")
* [Ubuntu 12.04和Windows 7双系统安装图解](http://www.linuxidc.com/Linux/2012-05/59663.htm)
* [虚拟机下安装BackTrack5 (BT5)教程及BT5汉化](http://www.linuxidc.com/Linux/2011-12/48609.htm)
* [Windows XP硬盘安装Ubuntu 12.04双系统图文详解](http://www.linuxidc.com/Linux/2012-04/59433.htm)
* [Virtualbox虚拟机安装Ubuntu图文教程](http://www.linuxidc.com/Linux/2010-04/25573.htm)
* [红旗Linux5.0安装教程](http://www.linuxidc.com/Linux/2006-11/1006.htm)
* [Windows XP硬盘安装Ubuntu 11.10双系统全程图解](http://www.linuxidc.com/Linux/2011-10/46327.htm)
* [Linux卷管理详解--VG LV PV](http://www.linuxidc.com/Linux/2012-05/60321.htm)
* [Win7用VMware安装Fedora 16](http://www.linuxidc.com/Linux/2012-06/63424.htm)

[Linux公社简介](http://www.linuxidc.com/aboutus.htm) - [广告服务](http://www.linuxidc.com/adsense.htm) - [网站地图](http://www.linuxidc.com/sitemap.aspx) - [帮助信息](http://www.linuxidc.com/help.htm) - [联系我们](http://www.linuxidc.com/contactus.htm)
本站（LinuxIDC）所刊载文章不代表同意其说法或描述，仅为提供更多信息，也不构成任何建议。
主编：漏网的鱼 (QQ:3165270) 联系邮箱:![]() (如有版权及广告合作请联系)
本站带宽由[[6688.CC](http://www.6688.cc/)]友情提供
关注Linux，关注LinuxIDC.com，请向您的QQ好友宣传LinuxIDC.com，多谢支持！
Copyright © 2006-2011　[Linux公社](http://www.linuxidc.com/)　All rights reserved 浙ICP备06018118号

成功接收数据
