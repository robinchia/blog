---
layout: post
title: "MongoDB基础篇 - 残夜 - 博客园"
categories: Nosql
tags: 
 - Nosql
 - mongo
--- 

# MongoDB基础篇 - 残夜 - 博客园

[]()

[![返回主页]()](http://www.cnblogs.com/oubo/)

# [Oubo的博客](http://www.cnblogs.com/oubo/)

##

* [博客园](http://www.cnblogs.com/)
* [首页](http://www.cnblogs.com/oubo/)
* [博问](http://q.cnblogs.com/)
* [闪存](http://home.cnblogs.com/ing/)
* [新随笔](http://www.cnblogs.com/oubo/admin/EditPosts.aspx?opt=1)
* [联系](http://space.cnblogs.com/msg/send/%e6%ae%8b%e5%a4%9c)
* [订阅](http://www.cnblogs.com/oubo/rss)
* [管理](http://www.cnblogs.com/oubo/admin/EditPosts.aspx)
随笔- 139 文章- 23 评论- 18 

# [MongoDB基础篇](http://www.cnblogs.com/oubo/archive/2012/02/21/2394663.html)

# **一、走进MongoDB**

MongoDB 是一个高性能，开源，面向集合，无模式的文档型数据库。它在许多场景下可用于替代传统的关系型数据库或键-值存储方式，MongoDB 使用C++开发。

## 1.1、初识MongoDB

MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。他支持的数据结构非常松散，是类似json 的bjson 格式，因此可以存储比较复杂的数据类型。MongoDB 最大的特点是他支持的查询语言非常强大，其语法有点类似于面向对象的查询语言，几乎可以实现类似关系数据库单表查询的绝大部分功能，而且还支持对数据建立索引。它是一个面向集合的,模式自由的文档型数据库。

### 1.1.1 面向集合（Collenction-Orented）

数据被分组存储在数据集中， 被称为一个集合（Collenction)。每个集合在数据库中都有一个唯一的标识名，并且可以包含无限数目的文档。集合的概念类似关系型数据库（RDBMS）里的表（table），不同的是它不需要定义任何模式（schema)。

### 1.1.2 模式自由（Schema-free)

存储在MongoDB 数据库中的文件，我们不需要知道它的任何结构定义。

### 1.1.3 文档型（Document File）

存储的数据是键-值对的集合"JSON"，键是字符串，值可以是数据类型集合里的任意类型，包括数组和文档。我们把这个数据格式称作 “BSON” 即 “Binary Serialized DocumentNotation.”  

## **2、MongoDB的特点**

* 面向集合存储，易于存储对象类型的数据
* 模式自由
* 支持动态查询
* 支持完全索引，包含内部对象
* 支持复制和故障恢复
* 使用高效的二进制数据存储，包括大型对象（如视频、图片等）
* 自动处理碎片，以支持云计算层次的扩展性
* 支持Python，PHP，Ruby，Java，C，C#，Javascript，Perl 及C++语言的驱动程序，社区中也提供了对Erlang 及.NET 等平台的驱动程序 文件存储格式为BSON（一种JSON 的扩展）
* 可通过网络访问
 

## **3、MongoDB的功能**

* 面向集合的存储：适合存储对象及JSON 形式的数据
* 动态查询：MongoDB 支持丰富的查询表达式。查询指令使用JSON 形式的标记，可轻易查询文档中内嵌的对象及数组
* 完整的索引支持：包括文档内嵌对象及数组。MongoDB 的查询优化器会分析查询表达式，并生成一个高效的查询计划
* 查询监视：MongoDB 包含一系列监视工具用于分析数据库操作的性能
* 复制及自动故障转移：MongoDB 数据库支持服务器之间的数据复制，支持主-从模式及服务器之间的相互复制。复制的主要目标是提供冗余及自动故障转移
* 高效的传统存储方式：支持二进制数据及大型对象（如照片或视频）
* 自动分片以支持云级别的伸缩性：自动分片功能支持水平的数据库集群，可动态添加额外的机器。
 

## **4、MongoDB的场景**

适用场景：

* 网站数据：MongoDB 非常适合实时的插入，更新与查询，并具备网站实时数据存储所需的复制及高度伸缩性
* 缓存：由于性能很高，MongoDB 也适合作为信息基础设施的缓存层。在系统重启之后，由MongoDB 搭建的持久化缓存层可以避免下层的数据源过载
* 大尺寸，低价值的数据：使用传统的关系型数据库存储一些数据时可能会比较昂贵，在此之前，很多时候程序员往往会选择传统的文件进行存储
* 高伸缩性的场景：MongoDB 非常适合由数十或数百台服务器组成的数据库。MongoDB的路线图中已经包含对MapReduce 引擎的内置支持
* 用于对象及JSON 数据的存储：MongoDB 的BSON 数据格式非常适合文档化格式的存储及查询
不适用场景：

* 高度事务性的系统，例如银行或会计系统。
* 传统的商业智能应用：针对特定问题的BI数据库会对产生高度优化的查询方式。
* 极为复杂的SQL查询
 

# **二、安装和配置**

MongoDB 的官方下载站是http://www.mongodb.org/downloads，可以去上面下载最新的安装程序下来。在下载页面可以看到，它对操作系统支持很全面，如OS X、Linux、Windows、Solaris都支持，而且都有各自的32 位和64 位版本。 **注意:**

1. MongoDB 1.8.1 Linux 版要求glibc 必须是2.5 以上。
1. 在32 位平台MongoDB 不允许数据库文件（累计总和）超过2G，而64 位平台没有这个限制。
这里主要介绍Linux平台的安装：

1. 步骤一: 下载MongoDB下载安装包：curl -O http://fastdl.mongodb.org/linux/mongodb-linux-i686-1.8.1.tgz
1. 步骤二: 设置MongoDB程序存放目录将其解压到/Apps，再重命名为mongo，路径为/Apps/mongo
1. 步骤三: 设置数据文件存放目录建立/data/db 的目录, mkdir –p /data/db
1. 步骤四: 启动MongoDB服务/Apps/mongo/bin/mongod --dbpath=/data/db；（MongoDB 服务端的默认连接端口是 27017）
1. 步骤五: 将MongoDB作为 Linux 服务随机启动；先创建/Apps/mongo/logs/mongodb.log 文件，用于存储MongoDB 的日志文件vi /etc/rc.local, 使用vi 编辑器打开配置文件，并在其中加入下面一行代码/Apps/mongo/bin/mongod --dbpath=/data/db --logpath=/Apps/mongo/logs/mongodb.log
1. 步骤六: 客户端连接验证；新打开一个Shell 输入：/Apps/mongo/bin/mongo，就可以开始我们的mongo之旅了。
1. 步骤七: 查看MongoDB日志；
 

# 三、体系结构

MongoDB 是一个可移植的数据库，它在流行的每一个平台上都可以使用，即所谓的跨平台特性。在不同的操作系统上虽然略有差别，但是从整体构架上来看，MongoDB 在不同的平台上是一样的，如数据逻辑结构和数据的存储等等。 一个运行着的MongoDB 数据库就可以看成是一个MongoDB Server，该Server 由实例和数据库组成，在一般的情况下一个MongoDB Server 机器上包含一个实例和多个与之对应的数据库，但是在特殊情况下，如硬件投入成本有限或特殊的应用需求，也允许一个Server 机器上可以有多个实例和多个数据库。 MongoDB 中一系列物理文件（数据文件，日志文件等）的集合或与之对应的逻辑结构（集合，文档等）被称为数据库，简单的说，就是数据库是由一系列与磁盘有关系的物理文件的组成。

## 3.1 数据逻辑结构

MongoDB 的逻辑结构是一种层次结构。主要由：文档(document)、集合(collection)、数据库(database)这三部分组成的。 逻辑结构是面向用户的，用户使用MongoDB 开发应用程序使用的就是逻辑结构。

1. MongoDB 的文档（document），相当于关系数据库中的一行记录。
1. 多个文档组成一个集合（collection），相当于关系数据库的表。
1. 多个集合（collection），逻辑上组织在一起，就是数据库（database）。
1. 一个MongoDB 实例支持多个数据库（database）。
[![]( "RDBMS和MongoDB")](http://zhoubo.sinaapp.com/?attachment_id=1646) 

## 3.2 数据存储结构

MongoDB 内部有预分配空间的机制，每个预分配的文件都用0进行填充，由于有了这个机制，MongoDB 始终保持额外的空间和空余的数据文件，从而有效避免了由于数据暴增而带来的磁盘压力过大的问题。 由于表中数据量的增加，数据文件每新分配一次，它的大小都会是上一个数据文件大小的2倍，每个数据文件最大2G。这样的机制有利于防止较小的数据库浪费过多的磁盘空间，同时又能保证较大的数据库有相应的预留空间使用。 数据库的每张表都对应一个命名空间，每个索引也有对应的命名空间。这些命名空间的元数据都集中在*.ns 文件中。每个命名空间可以包含多个不同的盘区，这些盘区并不是连续的。与数据文件的增长相同，每一个命名空间对应的盘区大小的也是随着分配的次数不断增长的。这样做的目的是为了平衡命名空间浪费的空间与保持某一个命名空间中数据的连续性。每当命名空间需要分配新的盘区的时候，都会先查看$freelist (用于记录不再使用的盘区的命名空间)是否有大小合适的盘区可以使用，这样就回收空闲的磁盘空间。  

# 四、快速入门

## 4.1 启动数据库

### 4.1.1 命令行启动方式

MongoDB 默认存储数据目录为/data/db/ (或者 c:\data\db), 默认端口27017，默认HTTP 端口28017。当然你也可以修改成不同目录,只需要指定dbpath 参数:

$mongod -h
$mongod --dbpath=/data/db --logpath=/tmp/mongod.log

### 4.1.2 配置文件启动方式

为减少命令后参数过多和维护成本，MongoDB 也支持同mysql 一样的读取启动配置文件的方式来启动数据库，配置文件的内容如下:

$ cat /etc/mongod.conf
dbpath=/data/db
$ mongod -f /etc/mongod.conf

### 4.1.3 Daemon方式启动

前面两种都提供的前台启动mongodb进程，但当mongodb进程的session被关闭时，mongodb进程也随之停止，这是非常不安全的。MongoDB提供了一种后台启动的选择，只需要加上一个--fork参数即可。

$ mongod --dbpath=/data/db --logpath=/tmp/mongod.log --fork

##  

## 4.2 停止数据库

MongoDB 提供的停止数据库命令也非常丰富，如果Control-C、发送shutdownServer()指令及发送Unix 系统中断信号等。

### 4.2.1 Control-C

$ mongo
> Control-C

### 4.2.2 shutdownServer()

$ mongo
> db.shutdownServer()

### 4.2.3 linux 系统中断信号

$ ps aux|grep mongod
$ kill -9 pid

##  

## 4.3 Mongo  API

$ ./mongo
>help //查看客户端帮忙命令
>db.help() //查看db的api
>db.mycroll.help() //查看collection的api

### 4.3.1 插入操作

>use mydb
>db.things.save({name:"mongo"})
>db.things.insert({x:1})

### 4.3.2 更新操作

>use mydb
>db.things.update({name:"mongo"},{$set:{name:"new_mongo"}})

### 4.3.3 删除操作

>use mydb
>db.things.remove({name:"mongo"})

### 4.3.4 查询操作

>use mydb
>db.things.find({name:"mongo"})

## 4.4 常用工具集

MongoDB 在bin 目录下提供了一系列有用的工具，这些工具提供了MongoDB 在运维管理上的方便。

* bsondump: 将bson 格式的文件转储为json 格式的数据
* mongo: 客户端命令行工具，其实也是一个js 解释器，支持js 语法
* mongod: 数据库服务端，每个实例启动一个进程，可以fork 为后台运行
* mongodump/ mongorestore: 数据库备份和恢复工具
* mongoexport/ mongoimport: 数据导出和导入工具
* mongofiles:GridFS 管理工具，可实现二制文件的存取
* mongos: 分片路由，如果使用了sharding 功能，则应用程序连接的是mongos 而不是mongod
* mongosniff: 这一工具的作用类似于tcpdump，不同的是他只监控MongoDB 相关的包请求，并且是以指定的可读性的形式输出
* mongostat: 实时性能监控工具
 

## 4.6 客户端工具

* MongoVUE：一个桌面程序，提供了对MongoDB 数据库的基本操作，如查看、查询、更新、删除等，简单易用，但是功能还比较弱，以后发展应该不错。
* RockMongo：是一个PHP5 写的MongoDB 管理工具。
* MongoHub：是一个针对Mac 平台的MongoDB 图形管理客户端，可用来管理MongoDB 数据的应用程序。

 
参考资料：《MongoDB总结.pdf》+[http://www.mongodb.org/display/DOCS/Home](http://www.mongodb.org/display/DOCS/Home)    

 

posted @ 2012-02-21 17:55 [残夜](http://www.cnblogs.com/oubo/) 阅读(359) 评论(0) [编辑](http://www.cnblogs.com/oubo/admin/EditPosts.aspx?postid=2394663) [收藏](http://www.cnblogs.com/oubo/archive/2012/02/21/2394663.html#)
![]()

[刷新评论]()[刷新页面](http://www.cnblogs.com/oubo/archive/2012/02/21/2394663.html#)[返回顶部](http://www.cnblogs.com/oubo/archive/2012/02/21/2394663.html#top)

[博客园首页](http://www.cnblogs.com/ "程序员的网上家园")[博问](http://q.cnblogs.com/ "程序员问答社区")[新闻](http://news.cnblogs.com/ "IT新闻")[闪存](http://home.cnblogs.com/ing/)[程序员招聘](http://job.cnblogs.com/)[知识库](http://kb.cnblogs.com/)

### 公告
Copyright ©2013 残夜
