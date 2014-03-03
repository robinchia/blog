---
layout: post
title: "windows下squid安装 - 水晶坊的博客 - 我的搜狐"
categories: 服务器
tags: 
 - 服务器
 - Squid
--- 

# windows下squid安装 - 水晶坊的博客 - 我的搜狐

### 分享到

* [一键分享](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [QQ空间](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [新浪微博](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [百度搜藏](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [人人网](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [腾讯微博](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [百度相册](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [开心网](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [腾讯朋友](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [百度贴吧](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [豆瓣网](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [搜狐微博](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [百度新首页](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [QQ好友](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [和讯微博](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
* [更多...](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)

[百度分享](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)

![]()  [水晶坊](http://crystalroom.i.sohu.com/ "水晶坊")

# 水晶坊

(@crystalroom)  被访问过 *5538* 次

[查看更多>>](http://crystalroom.i.sohu.com/profile/index.htm)
[送TA礼物]() [发短消息]() [查看留言>>](http://crystalroom.i.sohu.com/guestbook/index.htm) [跟随]() 留言:   对他说点什么  验证码  ![]() [看不清验证码？]()    发表

* [首页](http://crystalroom.i.sohu.com/)
* [我说两句](http://crystalroom.i.sohu.com/scomment/index.htm)
* [博客](http://crystalroom.i.sohu.com/blog/index.htm)
* [相册](http://crystalroom.i.sohu.com/album/index.htm)
* [视频](http://crystalroom.i.sohu.com/video/index.htm)
* [微博](http://t.sohu.com/people/Z194aWFvY2h1bkBjaGluYXJlbi5jb20=)
* [圈子](http://crystalroom.i.sohu.com/group/)
* ### 添加导航模块

* [首页](http://crystalroom.i.sohu.com/)
* [我说两句](http://crystalroom.i.sohu.com/scomment/index.htm)
* [相册](http://crystalroom.i.sohu.com/album/index.htm)
* [视频](http://crystalroom.i.sohu.com/video/index.htm)
* [博客](http://crystalroom.i.sohu.com/blog/index.htm)
* [微博](http://t.sohu.com/people/Z194aWFvY2h1bkBjaGluYXJlbi5jb20=)
* [圈子](http://crystalroom.i.sohu.com/group/index.htm)
* [育儿](http://crystalroom.i.sohu.com/app/baby/)
* [问答](http://crystalroom.i.sohu.com/app/wenda/)

### 添加自定义链接(仅限搜狐网链接)

网址：

名称：
[确认]()  使用新窗口打开 []()

[]()

# windows下squid安装

[收藏到手机]()    [转发]()   [评论(1)]()

2009-11-06 21:07

    

1、到[http://www.acmeconsulting.it/](http://www.acmeconsulting.it/)网站获取最新版本的squid for windows,我的是 2.6ST

2、解压缩 c:\squid

3、在c:\squid\etc目录下，

修改下列名字，最好保存原有文件Old

   squid.conf.default 修改为 squid.conf

   mime.conf.default 修改为 mime.conf

   cachemgr.conf.default 修改为 cachemgr.conf

4、建立d:\squid\var目录,在var目录下建立logs和cache目录，其中logs目录用于存放日志，cache目录用于存放硬盘缓存数据

5、将squid安装为服务，命令格式：squid -i [-f configfile] [-n servicename]，如c:\squid\sbin\squid -i -n Squid_Proxy，将使用默认的配置文件c:\squid\etc\squid.conf，服务名称为Squid_Proxy

6、修改配置文件squid.conf

# 监听80端口，并配置为加速模式

http_port 80 vhost

#添加需要反向代理的域名等:

cache_peer www.tx8.cn parent 80 0 no-query originserver name=www

cache_peer_domain www www.tx8.cn

# cache目录和大小的设置，10GB硬盘空间和512M内存

cache_dir ufs e:/squid/var/cache 10240 16 256

cache_mem 512 MB

# 主机文件路径

hosts_file d:/windows/system32/drivers/etc/hosts

# 设置日志目录和日志格式

access_log e:/squid/var/logs/access.log squid

cache_log e:/squid/var/logs/cache.log

cache_store_log e:/squid/var/logs/store.log

emulate_httpd_log on

# 允许所有用户访问
acl all src 0.0.0.0/0.0.0.0

http_access allow all

# 缓存管理员

cache_mgr webmaster@example.com

 

7、初始化cache目录

c:\squid\sbin\squid -z

如果配置文件出错的话，初始化cache目录将会出错。

8、启动Squid_Proxy服务

运行services.msc打开服务窗口，选择Squid_Proxy服务，将启动账号设置为开始建立的squid.

net start squid_proxy

9、检查Cache服务器运行是否正常

找一台终端，修改终端的hosts文件，将域名指向cache服务器的ip地址，检查网站是否正常访问。

也可以用自带的SquidClient 来检查；

基本的使用方法

*取得squid运行状态信息： squidclient -p 80 mgr:info

*取得squid内存使用情况： squidclient -p 80 mgr:mem

*取得squid已经缓存的列表： squidclient -p 80 mgr:objects. use it carefully,it may crash

*取得squid的磁盘使用情况： squidclient -p 80 mgr:diskd

*更多的请查看：squidclient -h 或者 squidclient -p 80 mgr
分享到： [](http://crystalroom.i.sohu.com/blog/view/136068744.htm# "分享到新浪微博") [](http://crystalroom.i.sohu.com/blog/view/136068744.htm# "分享到QQ空间") [](http://crystalroom.i.sohu.com/blog/view/136068744.htm# "分享到人人网") [](http://crystalroom.i.sohu.com/blog/view/136068744.htm# "分享到搜狐微博") [](http://crystalroom.i.sohu.com/blog/view/136068744.htm# "分享到豆瓣网")

[编辑](http://i.sohu.com/blog/home/entry/edit.htm?id=136068744)   [删除]()   [阅读(572)]()   [转发]()   [评论(1)]()

来自[我的搜狐](http://i.sohu.com/) [在博客中查看](http://crystalroom.blog.sohu.com/136068744.html)

分类：暂无分类
上一篇： [code](http://crystalroom.i.sohu.com/blog/view/153219018.htm "code")

下一篇： [squid3.0安装需要安装工具包](http://crystalroom.i.sohu.com/blog/view/136038839.htm "squid3.0安装需要安装工具包")

### 我要评论:此处评论内容会与搜狐博客同步哦！

[![]()]( "未登录")

[评论]()

[]()0 / 300
同时转发

* [![]()](http://i.sohu.com/p/=v2=eMF2ITLATjk0nMN4AmNvbQ==/)

[搜狐网友11693012](http://i.sohu.com/p/=v2=eMF2ITLATjk0nMN4AmNvbQ==/) 作者你自己也亲自做通了的吗 我的SQUID服务启动时始终报1067的错误你知道否，留下QQ7628813，盼复

[回复]()01月19日 15:07
评论加载中

* [跟随](http://crystalroom.i.sohu.com/friend/index.htm) [0](http://crystalroom.i.sohu.com/friend/index.htm)
* [跟随者](http://crystalroom.i.sohu.com/friend/fans.htm) [1](http://crystalroom.i.sohu.com/friend/fans.htm)
* [新鲜事](http://crystalroom.i.sohu.com/) [0](http://crystalroom.i.sohu.com/)

* [等级](http://crystalroom.i.sohu.com/app/score/) *[0](http://crystalroom.i.sohu.com/app/score/)*
* [金币](http://crystalroom.i.sohu.com/app/score/) *[0](http://crystalroom.i.sohu.com/app/score/)*
* [勋章](http://crystalroom.i.sohu.com/app/score/) *[暂无勋章！](http://crystalroom.i.sohu.com/app/score/)*
* [礼物](http://crystalroom.i.sohu.com/app/score/) [暂无礼物！](http://crystalroom.i.sohu.com/app/score/)  
### []()TA推荐的人

正在加载...

### 博客分类

* [全部分类](http://crystalroom.i.sohu.com/blog/show/entry/list.htm)
* [阿里开发](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?cid=6333433 "阿里开发")
* [TX8管理](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?cid=6265638 "TX8管理")
* [硬件](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?cid=6257567 "硬件")
* [javascript](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?cid=4959662 "javascript")
* [网站设计](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?cid=4403105 "网站设计")
* [网络技术](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?cid=4093803 "网络技术")
* [技术方案](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?cid=4093605 "技术方案")
* [股票](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?cid=3928722 "股票")
* [linux技术](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?cid=3919387 "linux技术")
* [数据库技术](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?cid=3919261 "数据库技术")
* [asp技术](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?cid=3919259 "asp技术")
### 博客标签

[fdisk](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=fdisk "1篇")[frame](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=frame "1篇")[javascript](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=javascript "1篇")[linux](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=linux "1篇")[named.conf](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=named.conf "1篇")[parent](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=parent "1篇")[xml](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=xml "1篇")[不能识别硬盘](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%B2%BB%C4%DC%CA%B6%B1%F0%D3%B2%C5%CC "1篇")[参数](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%B2%CE%CA%FD "1篇")[存储过程](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%B4%E6%B4%A2%B9%FD%B3%CC "1篇")[代理](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%B4%FA%C0%ED "1篇")[电信](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%B5%E7%D0%C5 "1篇")[调用](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%B5%F7%D3%C3 "1篇")[动态解析](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%B6%AF%CC%AC%BD%E2%CE%F6 "1篇")[负载均衡](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%B8%BA%D4%D8%BE%F9%BA%E2 "1篇")[格式](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%B8%F1%CA%BD "1篇")[股指期货](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%B9%C9%D6%B8%C6%DA%BB%F5 "1篇")[进度条](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%BD%F8%B6%C8%CC%F5 "1篇")[两个陈列](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%C1%BD%B8%F6%B3%C2%C1%D0 "1篇")[路由表](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%C2%B7%D3%C9%B1%ED "1篇")[路由](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%C2%B7%D3%C9 "3篇")[内存](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%C4%DA%B4%E6 "1篇")[配置](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%C5%E4%D6%C3 "1篇")[平衡](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%C6%BD%BA%E2 "1篇")[启动](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%C6%F4%B6%AF "1篇")[上传](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%C9%CF%B4%AB "1篇")[上网](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%C9%CF%CD%F8 "1篇")[示例](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%CA%BE%C0%FD "1篇")[双网关](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%CB%AB%CD%F8%B9%D8 "1篇")[网卡](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%CD%F8%BF%A8 "1篇")[网络](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%CD%F8%C2%E7 "1篇")[网通](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%CD%F8%CD%A8 "1篇")[下拉菜单](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%CF%C2%C0%AD%B2%CB%B5%A5 "1篇")[线路](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%CF%DF%C2%B7 "1篇")[硬盘分区](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%D3%B2%C5%CC%B7%D6%C7%F8 "1篇")[硬盘](http://crystalroom.i.sohu.com/blog/show/entry/list.htm?tag=%D3%B2%C5%CC "1篇")

### [更多>>](http://blog.sohu.com/)精彩博文

* [黎明](http://cat898-com.i.sohu.com/)[抗震救灾为何堵成一片](http://cat898-com.i.sohu.com/blog/view/261882543.htm)
* [陶短房](http://taoduanfang.i.sohu.com/)[曝壹基金管理费惊人](http://taoduanfang.i.sohu.com/blog/view/261756188.htm)
* [徐潜川](http://xuqianchuan.i.sohu.com/)[揭开器官买卖黑市内幕](http://xuqianchuan.i.sohu.com/blog/view/261829390.htm)
* [兜兜](http://sangedoudou.i.sohu.com/)[幼升小必考题及报考攻略](http://sangedoudou.i.sohu.com/blog/view/261771925.htm)
* [专题](http://zt.blog.sohu.com/s2013/mango/index.shtml)[晒搭配成为超人气封面女郎](http://zt.blog.sohu.com/s2013/mango/index.shtml)
* [月儿雾](http://niangzi456.i.sohu.com/)[新娘婚前美容四大误区](http://niangzi456.i.sohu.com/blog/view/261811580.htm)
[]()

[帮助](http://z.i.sohu.com/help/helpqa.html) - [客服中心](http://sohucallcenter.blog.sohu.com/) - [意见建议](http://q.sohu.com/group/23063) - [举报](mailto:jubao@contact.sohu.com) - [搜狐首页](http://www.sohu.com/)

Copyright © 2013 Sohu.com Inc. All rights reserved. 搜狐公司 版权所有
![]() ![]()

This ad is supporting your extension Allow Right-Click: [More info]() | [Privacy Policy]() | [Hide on this page](http://crystalroom.i.sohu.com/blog/view/136068744.htm#)
[意见反馈](http://feedback.q.sohu.com/)[]( "返回页面顶部")
