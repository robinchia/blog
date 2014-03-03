---
layout: post
title: "建立mysql可远程连接root权限用户-电脑知识网-电脑基础知识"
categories: DB
tags: 
 - DB
 - mysql
--- 

# 建立mysql可远程连接root权限用户-电脑知识网-电脑基础知识

[生活百宝箱](http://www.diannaozs.com/bbx/) | **[IT术语解释](http://www.diannaozs.com/itshuyu/Index.html)** | [Alexa排名查询](http://www.diannaozs.com/alexa/ "alexa查询系统,alexa排名查询,alexa网站排名查询,全球alexa排名查询,alexa世界排名查询,alexa排名,alexa工具条") | [PR 值查询](http://www.diannaozs.com/pr/ "PR值查询") | [在线代理](http://www.diannaozs.com/daili/ "网页代理,在线网站代理,ip代理,IE代理,QQ代理") | [IP地址查询](http://www.diannaozs.com/ip/ "IP地址查询，IP查询所属地区查询") | [在线翻译](http://www.diannaozs.com/fanyi.htm "中英互译，在线翻译，翻译查询") | [邮政系统查询](http://www.diannaozs.com/yz/ "邮政编码查询，邮政地址查询，街道查询") | [火车票在线查询](http://www.diannaozs.com/huoche/ "全国火车票查询") | [万年历](http://www.diannaozs.com/wl.asp "节日查询，万年历，日期查询，时间查询") | [颜色选择器](http://www.diannaozs.com/color/ "网页颜色选择器，颜色选取工具，颜色查询工具") | [常用网页小图标](http://www.diannaozs.com/webico.html)

[![]()](http://www.diannaozs.com/ "电脑知识网")

[主板](http://www.diannaozs.com/zt/zb/Index.html) | [CPU](http://www.diannaozs.com/zt/CPU/Index.html) | [硬盘](http://www.diannaozs.com/zt/yp/Index.html) | [内存](http://www.diannaozs.com/zt/nc/Index.html) [显卡](http://www.diannaozs.com/zt/xk/Index.html) | [声卡](http://www.diannaozs.com/zt/sk/Index.html) | [网卡](http://www.diannaozs.com/zt/wk/Index.html) | [显示器](http://www.diannaozs.com/zt/xs/Index.html) | [机箱](http://www.diannaozs.com/zt/jx/Index.html) | [音响](http://www.diannaozs.com/zt/yx/Index.html) | [摄像头](http://www.diannaozs.com/zt/sx/Index.html)
[路由器](http://www.diannaozs.com/zt/ly/Index.html) | [交换机](http://www.diannaozs.com/zt/jh/Index.html) | [服务器](http://www.diannaozs.com/zt/fw/Index.html) | [打印机](http://www.diannaozs.com/zt/pt/Index.html) | [复印机](http://www.diannaozs.com/zt/fy/Index.html) | [传真机](http://www.diannaozs.com/zt/cz/Index.html) | [网线](http://www.diannaozs.com/zt/wx/Index.html) | [水晶头](http://www.diannaozs.com/zt/sj/Index.html) [更多专题…](http://www.diannaozs.com/zt/)
[杀毒](http://www.diannaozs.com/zt/sd/Index.html) | [教程](http://www.diannaozs.com/zt/jc/Index.html) | [注册表](http://www.diannaozs.com/zt/zc/Index.html) | [组策略](http://www.diannaozs.com/zt/zl/Index.html) | [DOS](http://www.diannaozs.com/zt/DOS/Index.html) | [网络组建](http://www.diannaozs.com/zt/zj/Index.html) | [电脑组装](http://www.diannaozs.com/zt/zz/Index.html) | [QQ空间](http://www.diannaozs.com/zt/Q/Index.html) | [中国百科](http://www.diannaozs.com/zt/chinaabc/Index.html)

[[繁体版]()]
[订阅![]()](http://www.diannaozs.com/xml/Rss.xml)
 | [网站首页](http://www.diannaozs.com/Index.html) | [新闻中心](http://www.diannaozs.com/Article/Index.html) | [系统知识](http://www.diannaozs.com/xitongzhishi/Index.html) | [网络安全](http://www.diannaozs.com/wangluoanquan/Index.html) | [平面动画](http://www.diannaozs.com/donghuasheji/Index.html) | [网站程序](http://www.diannaozs.com/wangzhanchenxu/Index.html) | [数据库](http://www.diannaozs.com/shujukuzhishi/Index.html) | [报错查询](http://www.diannaozs.com/baocuochaxun/Index.html) | [注册码](http://www.diannaozs.com/key/Index.html) | [网站模板](http://www.diannaozs.com/mb/Index.html) | [网站优化](http://www.diannaozs.com/SEO/Index.html) | [图库](http://www.diannaozs.com/Photo/Index.html) | [下载](http://www.diannaozs.com/Soft/Index.html) | **[[电脑论坛]](http://www.diannaozs.com/bbs/)**

**>>** 您现在的位置： [电脑知识网](http://www.diannaozs.com/) >> [数据库](http://www.diannaozs.com/shujukuzhishi/Index.html) >> [Mysql](http://www.diannaozs.com/shujukuzhishi/m/Index.html) >> 正文
  **建立mysql可远程连接root权限用户** 双击自动滚屏  【字体：[小]() [大]()】 建立mysql可远程连接root权限用户  [电脑知识网](http://www.diannaozs.com/)为您报时：今天是  

     以下语句具有和ROOT用户一样的权限。大家在拿站时应该碰到过。root用户的mysql，只可以本地连，对外拒绝连接。以下方法可以帮助你解决这个问题了，下面的语句功能是，建立一个用户为itpro 密码123 权限为和root一样。允许任意主机连接。这样你可以方便进行在本地远程操作数据库了。

CREATE USER ['itpro'@'%'](mailto:'itpro'@'%') IDENTIFIED BY '123';

GRANT ALL PRIVILEGES ON *.* TO ['itpro'@'%'](mailto:'itpro'@'%') IDENTIFIED BY '123'WITH GRANT OPTION

MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0

MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0;

搞完事记得删除脚印哟。

DROP USER ['itpro'@'%'](mailto:'itpro'@'%');

DROP DATABASE IF EXISTS `itpro` ;
![]() **[我想对电脑方面的知识提问](http://www.diannaozs.com/bbs/index.asp?boardid=3 "欢迎您对电脑知识提问")** ![]() **[我想撰写关于电脑知识方面的文章](http://www.diannaozs.com/bbs/index.asp?boardid=5 "欢迎您撰写电脑文章")** ![]() [我想对生活方面的知识提问](http://www.diannaozs.com/bbs/index.asp?boardid=4 "欢迎您提问") ![]() [我想撰写生活方面的文章](http://www.diannaozs.com/bbs/index.asp?boardid=6 "欢迎您撰写文章")     **推荐视频：**
[Windows Vista安装](http://www.diannaozs.com/2009sp_vista.html)
[Windows XP安装](http://www.diannaozs.com/2009sp_xp.html)
[设置BIOS](http://www.diannaozs.com/2009sp_bios.html)
[硬盘分区](http://www.diannaozs.com/2009sp_fq.html)
[经典FLASH动画视频教程](http://www.diannaozs.com/flash_20090906/1.html)
[Photoshop视频教程](http://www.diannaozs.com/book/tucengshipin.html)
[Dreamweaver视频教程](http://www.diannaozs.com/book2/dream.html)
[C语言视频](http://www.diannaozs.com/book2/C/Index.html)
[DOS 视频教程](http://www.diannaozs.com/book2/DOS.html) (黑客入门) **推荐工具：**
[手机号、身份证、区号查询](http://www.diannaozs.com/bbx/query/index.htm)
[城市地图](http://www.diannaozs.com/bbx/map/zl_ditu.htm)
[计算器](http://www.diannaozs.com/bbx/computer/jsq_shuxue.htm)
[火车查询](http://www.diannaozs.com/bbx/train/index.htm)
[健康指数查询](http://www.diannaozs.com/bbx/computer/jsq_jiankang.htm)
[许愿树](http://www.diannaozs.com/hopetree/)
[爱墙祝福](http://www.diannaozs.com/love/)
[Q爱](http://www.diannaozs.com/qqlove/default.asp)
[flash小游戏](http://www.diannaozs.com/youxi/)
 **站长工具：**
[IP查询](http://www.diannaozs.com/bbx/query/index.htm)
[PR 值查询](http://www.diannaozs.com/pr/ "PR值查询")
[Alexa排名查询](http://www.diannaozs.com/alexa/ "alexa查询系统,alexa排名查询,alexa网站排名查询,全球alexa排名查询,alexa世界排名查询,alexa排名,alexa工具条")
[网站运营](http://www.diannaozs.com/SEO/yy/Index.html)
[百度优化](http://www.diannaozs.com/SEO/baidu/Index.html)
[Google优化](http://www.diannaozs.com/SEO/google/Index.html)
[Alexa排名](http://www.diannaozs.com/SEO/Alexa/Index.html)
[更多……](http://www.diannaozs.com/seo/) **推荐网站：**
[健康者](http://www.jkzhe.com/)（健康类型网站）
[PCWindows](http://www.pcwindows.net/)（系统学习）
[第三导航](http://www.dh3.org/)（协助上网）
[Flash小游戏](http://www.youxi28.com/)（小游戏）
[人人健康](http://www.diannaozs.com/shujukuzhishi/m/200905/www.rrjk.com.cn)(rrjk.com.cn)
[我爱PC](http://www.diannaozs.com/shujukuzhishi/m/200905/www.5apc.cn)(5apc.cn) * 上一篇数据库： [MySQL数据库函数详解(目录)](http://www.diannaozs.com/shujukuzhishi/m/200904/20090407093714.html "文章标题：MySQL数据库函数详解(目录)
作    者：佚名
更新时间：2009-4-7 9:37:14")
* 下一篇数据库： [基于MySQL数据库的UTF8中文网站全文检索的实现](http://www.diannaozs.com/shujukuzhishi/m/200905/20090515150846.html "文章标题：基于MySQL数据库的UTF8中文网站全文检索的实现
作    者：admin
更新时间：2009-5-15 15:08:46") ![]() **相关文章** [MySQL数据库数据备份和恢复详…](http://www.diannaozs.com/shujukuzhishi/m/200907/20090719210814.html "文章标题：MySQL数据库数据备份和恢复详解
作    者：admin
更新时间：2009-7-19 21:08:14")
[基于MySQL数据库的UTF8中文网…](http://www.diannaozs.com/shujukuzhishi/m/200905/20090515150846.html "文章标题：基于MySQL数据库的UTF8中文网站全文检索的实现
作    者：admin
更新时间：2009-5-15 15:08:46")
[MYSQL出错代码列表](http://www.diannaozs.com/shujukuzhishi/s/200904/20090427082753.html "文章标题：MYSQL出错代码列表
作    者：佚名
更新时间：2009-4-27 8:27:53")
[MySQL数据库在指定位置增加字…](http://www.diannaozs.com/shujukuzhishi/s/200904/20090426181143.html "文章标题：MySQL数据库在指定位置增加字段
作    者：佚名
更新时间：2009-4-26 18:11:43")
[图形化MySQL工具:SQLyog MyS…](http://www.diannaozs.com/shujukuzhishi/s/200904/20090426180556.html "文章标题：图形化MySQL工具:SQLyog MySQL GUI
作    者：佚名
更新时间：2009-4-26 18:05:56")
[精简mysql中文乱码解决方法](http://www.diannaozs.com/shujukuzhishi/s/200904/20090426180154.html "文章标题：精简mysql中文乱码解决方法
作    者：佚名
更新时间：2009-4-26 18:01:54")
[MySQL数据库函数详解(目录)](http://www.diannaozs.com/shujukuzhishi/m/200904/20090407093714.html "文章标题：MySQL数据库函数详解(目录)
作    者：佚名
更新时间：2009-4-7 9:37:14")
[.Net中操作MySql数据库](http://www.diannaozs.com/shujukuzhishi/m/200904/20090407092220.html "文章标题：.Net中操作MySql数据库
作    者：admin
更新时间：2009-4-7 9:22:20")
[1.8.6.3. ENUM和SET约束…](http://www.diannaozs.com/shujukuzhishi/m/200903/20090326153808.html "文章标题：1.8.6.3. ENUM和SET约束
作    者：佚名
更新时间：2009-3-26 15:38:08")
[1.8.6.2. 对无效数据的约束…](http://www.diannaozs.com/shujukuzhishi/m/200903/20090326153548.html "文章标题：1.8.6.2. 对无效数据的约束
作    者：佚名
更新时间：2009-3-26 15:35:48")  [电子书教程](http://www.diannaozs.com/book/)   [设为首页](http://www.diannaozs.com/$)   [加入收藏]()   [网站地图](http://www.diannaozs.com/SiteMap/Article1.htm)   [软件下载地图](http://www.diannaozs.com/SiteMap/Soft1.htm)   [友情连接](http://www.diannaozs.com/FriendSite/)   [联系站长](mailto:diannaozs@126.com%20<diannaozs@126.com>)   [留言求助](http://www.diannaozs.com/bbs/index.asp?boardid=3) Copyright 2007-2011 电脑知识网(http://www.diannaozs.com) All rights reserved
信息产业部备案：京ICP备08100023号
