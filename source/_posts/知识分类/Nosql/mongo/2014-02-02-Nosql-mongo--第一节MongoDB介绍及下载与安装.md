---
layout: post
title: "第一节 MongoDB介绍及下载与安装"
categories: Nosql
tags: 
 - Nosql
 - mongo
--- 

# 第一节 MongoDB介绍及下载与安装

## [第一节 MongoDB介绍及下载与安装](http://www.cnblogs.com/mecity/archive/2011/06/11/2078527.html)

**引言**
    MongoDB是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。他支持的数据结构非常松散，是类似json的bjson格式，因此可以存储比较复杂的数据类型。Mongo最大的特点是他支持的查询语言非常强大，其语法有点类似于面向对象的查询语言，几乎可以实现类似关系数据库单表查询的绝大部分功能，而且还支持对数据建立索引。

它的特点是高性能、易部署、易使用，存储数据非常方便。主要功能特性有：

* 面向集合存储，易存储对象类型的数据。
* 模式自由。
* 支持动态查询。
* 支持完全索引，包含内部对象。
* 支持查询。
* 支持复制和故障恢复。
* 使用高效的二进制数据存储，包括大型对象（如视频等）。
* 自动处理碎片，以支持云计算层次的扩展性
* 支持RUBY，PYTHON，JAVA，C++，PHP等多种语言。
* 文件存储格式为BSON（一种JSON的扩展）
* 可通过网络访问

所谓“面向集合”（Collenction-Orented），意思是数据被分组存储在数据集中，被称为一个集合（Collenction)。每个 集合在数据库中都有一个唯一的标识名，并且可以包含无限数目的文档。集合的概念类似关系型数据库（RDBMS）里的表（table），不同的是它不需要定 义任何模式（schema)。
模式自由（schema-free)，意味着对于存储在mongodb数据库中的文件，我们不需要知道它的任何结构定义。如果需要的话，你完全可以把不同结构的文件存储在同一个数据库里。
存储在集合中的文档，被存储为键-值对的形式。键用于唯一标识一个文档，为字符串类型，而值则可以是各中复杂的文件类型。我们称这种存储形式为BSON（Binary Serialized dOcument Format）。

MongoDB服务端可运行在Linux、Windows或OS X平台，支持32位和64位应用，默认端口为27017。推荐运行在64位平台，因为MongoDB

在32位模式运行时支持的最大文件尺寸为2GB。

MongoDB把数据存储在文件中（默认路径为：/data/db），为提高效率使用内存映射文件进行管理。

以上为随便摘的，其实就是非传统的非关系数据库，现在归到文档型数据库分类之中，注意32位操作系统支持的最大文件为2GB，所以做大文件海量储存的朋友要选择64位的系统安装。开始我们的下载安装之路吧。

### 一、下载

MongoDB的官网是：[http://www.mongodb.org/](http://www.mongodb.org/)

MongoDB最新版本下载在官网的DownLoad菜单下：[http://www.mongodb.org/downloads](http://www.mongodb.org/downloads) 

本人选择的是Windows 32-bit 1.8.1版本

MongoDB For .net 驱动开发包位于官网的Driver菜单下（含其它语言开发链接）：[https://github.com/mongodb/mongo-csharp-driver/downloads](https://github.com/mongodb/mongo-csharp-driver/downloads)

本人操作系统为Windows7 专业版，选择MongoDB版本为Windows 32-bit 1.8.1，开发包为VS2008版本

开始我们的安装过程了

### 二、安装

1.解压mongodb-win32-i386-1.8.1.zip ，创建路径C:\Program Files\mongodb ，将解压后的Bin文件Copy to 此文件夹下

2.C:\Program Files\mongodb 下建立Data文件夹 C:\Program Files\mongodb\data ,然后分别建立db,log两个文件夹，至此mongodb下有以下文件夹

C:\Program Files\mongodb\bin

C:\Program Files\mongodb\data\db

C:\Program Files\mongodb\data\log

在log文件夹下创建一个日志文件MongoDB.log，即C:\Program Files\mongodb\data\log\MongoDB.log

完成以上工作后，你为奇怪为什么要建立这些文件夹（因为，Mongodb安装需要这些文件夹，默认安装是不用创建，但是文件都为安装到C:\data\下）

**** 

3.几种安装方式介绍

**3.1 程序启动方式**

    运行cmd.exe 进入DOS命中界面

> cd C:\Program Files\mongodb\bin

> C:\Program Files\mongodb\bin>mongod -dbpath "C:\Program Files\mongodb\data\db"

执行此命令即将mongodb的数据库文件创建到C:\Program Files\mongodb\data\db 目录，不出意外的会看到命令最后一行sucess的成功提示

此时数据库就已启动，该界面为Mongo的启动程序，关闭后可直接双击bin下的mongod.exe  (注意是d，这个是启动程序）

启动程序开启后，再运行mongo.exe 程序（注意没有d) ，界面如下

![]()

测试数据库操作

>help  (查看相关信息）

>db.foo.insert({a:1})    （往foo表插入a,1字段值,foo表为默认表)

>db.foo.find()                (查看foo表数据）

结果如下：

  ![]()

可以看到插入了3条记录分别人a,cctv,set 。

当mongod.exe被关闭时，mongo.exe 就无法连接到数据库了，因此每次想使用mongodb数据库都要开启mongod.exe程序，所以比较麻烦，接下来我们将

MongoDB安装为windows服务吧

**3.2 windows service方式**

运行cmd.exe

> cd C:\Program Files\mongodb\bin

> C:\Program Files\mongodb\bin>mongod --dbpath "C:\Program Files\mongodb\data\db" --logpath "C:\Program Files\mongodb\data\log\MongoDB.log" --install --serviceName "MongoDB"

这里MongoDB.log就是开始建立的日志文件，--serviceName "MongoDB" 服务名为MongoDB

运行命令成功为如下图：

![]()

引时服务已经安装成功，运行

>NET START MongoDB   (开启服务）

>NET stop MongoDB   (关闭服务)

>

> C:\Program Files\mongodb\bin>mongod --dbpath "C:\Program Files\mongodb\data\db" --logpath "C:\Program Files\mongodb\data\log\MongoDB.log" --remove --serviceName "MongoDB"      (删除，注意不是--install了）

其它命令可查阅help命令或官网说明。

查看服务![]()

运行bin文件夹下mongo.exe 客户端测试一下吧。测试同3.1相同 。

**3.3 守护进程方式创**

--fork 以守护进程方式运行MongoDB，创建服务器进程

>C:\Program Files\mongodb\bin>mongod --port 10220 --fork  --dbpath "C:\Program Files\mongodb\data\db" --logpath "C:\Program Files\mongodb\data\log\MongoDB.log"

forked process : 44086

all output going to : MongoDB.log

到此几种安装就介绍完了。

4、停止MongoDB

最稳妥的方式,处理完当前所有操作并将缓存的数据保存到磁盘上才停止

>user admin

>db.shutdownServer();

当然我们也可以直接关闭进程，但这种方式会导致缓存中的数据未急时刷新保存到磁盘上而丢失。下一章就是mongo for .net开发了。

分类: [MongoDB](http://www.cnblogs.com/mecity/category/304803.html)

标签: [mongodb](http://www.cnblogs.com/mecity/tag/mongodb/), [mongo for .net](http://www.cnblogs.com/mecity/tag/mongo for .net/), [nosql](http://www.cnblogs.com/mecity/tag/nosql/)
绿色通道： [好文要顶]() [关注我]() [收藏该文]()[与我联系](http://space.cnblogs.com/msg/send/%e5%b0%8f%e5%9f%8e%e5%b2%81%e6%9c%88) [![]()]( "分享至新浪微博")

[![]()](http://home.cnblogs.com/u/mecity/)

[小城岁月](http://home.cnblogs.com/u/mecity/)
[关注 - 26](http://home.cnblogs.com/u/mecity/followees)
[粉丝 - 221](http://home.cnblogs.com/u/mecity/followers)

[+加关注]()

3

0
(请您对文章做出评价)

[»](http://www.cnblogs.com/mecity/archive/2011/06/12/MongoDB.html)下一篇：[第二节 为什么用MongoDB及.NET开发入门](http://www.cnblogs.com/mecity/archive/2011/06/12/MongoDB.html "发布于2011-06-12 15:17")

posted on 2011-06-11 20:07 [小城岁月](http://www.cnblogs.com/mecity/) 阅读(10141) 评论(5) [编辑](http://www.cnblogs.com/mecity/admin/EditPosts.aspx?postid=2078527) [收藏]()

## [#1楼]()[]()   

小城 ,这是原创???

[支持(0)]()[反对(0)]()
2011-06-12 00:07 | [白菜.tang](http://home.cnblogs.com/u/293506/) [ ](http://space.cnblogs.com/msg/send/%e7%99%bd%e8%8f%9c.tang "发送站内短消息")
## [#2楼]()[]()[楼主]   

[@]( "查看所回复的评论")白菜.tang
鄙视你

[支持(0)]()[反对(0)]()
http://pic.cnitblog.com/face/u131283.jpg?id=15165123

2011-06-12 10:08 | [小城岁月](http://www.cnblogs.com/mecity/) [ ](http://space.cnblogs.com/msg/send/%e5%b0%8f%e5%9f%8e%e5%b2%81%e6%9c%88 "发送站内短消息")

## [#3楼]()[]()   

支持个~

[支持(0)]()[反对(0)]()
2011-06-12 21:23 | [布莱特](http://www.cnblogs.com/brett80/) [ ](http://space.cnblogs.com/msg/send/%e5%b8%83%e8%8e%b1%e7%89%b9 "发送站内短消息")
## [#4楼]()[]()   

看来是玩命的写，我们就玩命的拜吧~~~紧跟时代潮流哈

[支持(0)]()[反对(0)]()
2011-06-15 16:14 | [happydream](http://home.cnblogs.com/u/308327/) [ ](http://space.cnblogs.com/msg/send/happydream "发送站内短消息")

## [#5楼]()[]()21611232011/7/26 15:05:12   

好料

[支持(0)]()[反对(0)]()
2011-07-26 15:05 | [技术拓荒者](http://www.cnblogs.com/techPioneer/) [ ](http://space.cnblogs.com/msg/send/%e6%8a%80%e6%9c%af%e6%8b%93%e8%8d%92%e8%80%85 "发送站内短消息")
