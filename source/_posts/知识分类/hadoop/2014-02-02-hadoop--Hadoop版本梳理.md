---
layout: post
title: "Hadoop版本梳理"
categories: hadoop
tags: 
 - hadoop
--- 

# Hadoop版本梳理

由于Hadoop版本混乱多变，因此，Hadoop的版本选择问题一直令很多初级用户苦恼。本文总结了Apache Hadoop和Cloudera Hadoop的版本衍化过程，并给出了选择Hadoop版本的一些建议。

### 1. Apache Hadoop

### 1.1  Apache版本衍化

截至目前（2012年12月23日），Apache Hadoop版本分为两代，我们将第一代Hadoop称为Hadoop 1.0，第二代Hadoop称为Hadoop 2.0。第一代Hadoop包含三个大版本，分别是0.20.x，0.21.x和0.22.x，其中，0.20.x最后演化成1.0.x，变成了稳定版，而0.21.x和0.22.x则NameNode HA等新的重大特性。第二代Hadoop包含两个版本，分别是0.23.x和2.x，它们完全不同于Hadoop 1.0，是一套全新的架构，均包含HDFS Federation和YARN两个系统，相比于0.23.x，2.x增加了NameNode HA和Wire-compatibility两个重大特性。

经过上面的大体解释，大家可能明白了Hadoop以重大特性区分各个版本的，总结起来，用于区分Hadoop版本的特性有以下几个：

**（1）Append** 支持文件追加功能，如果想使用HBase，需要这个特性。

**（2）RAID **在保证数据可靠的前提下，通过引入校验码较少数据块数目。详细链接：

https://issues.apache.org/jira/browse/HDFS/component/12313080

**（3）Symlink** 支持HDFS文件链接，具体可参考： [https://issues.apache.org/jira/browse/HDFS-245](https://issues.apache.org/jira/browse/HDFS-245)

**（4）Security** Hadoop安全，具体可参考：[https://issues.apache.org/jira/browse/HADOOP-4487](https://issues.apache.org/jira/browse/HADOOP-4487)

**（5） NameNode HA** 具体可参考：[https://issues.apache.org/jira/browse/HDFS-1064](https://issues.apache.org/jira/browse/HDFS-1064)

**（6） HDFS Federation和YARN**

![]( "apache-hadoop-versions")

需要注意的是，Hadoop 2.0主要由Yahoo独立出来的hortonworks公司主持开发。

### 1.2  Apache版本下载

（1） 各版本说明：[http://hadoop.apache.org/releases.html](http://hadoop.apache.org/releases.html)。

（2） 下载稳定版：找到一个镜像，下载stable文件夹下的版本。

（3） Hadoop最全版本：[http://svn.apache.org/repos/asf/hadoop/common/branches/](http://svn.apache.org/repos/asf/hadoop/common/branches/)，可直接导到eclipse中。

### 2. Cloudera Hadoop

### 2.1  CDH版本衍化

Apache当前的版本管理是比较混乱的，各种版本层出不穷，让很多初学者不知所措，相比之下，Cloudera公司的Hadoop版本管理的要很多。

我们知道，Hadoop遵从Apache开源协议，用户可以免费地任意使用和修改Hadoop，也正因此，市面上出现了很多Hadoop版本，其中比较出名的一是Cloudera公司的发行版，我们将该版本称为CDH（Cloudera Distribution Hadoop）。截至目前为止，CDH共有4个版本，其中，前两个已经不再更新，最近的两个，分别是CDH3（在Apache Hadoop 0.20.2版本基础上演化而来的）和CDH4在Apache Hadoop 2.0.0版本基础上演化而来的），分别对应Apache的Hadoop 1.0和Hadoop 2.0，它们每隔一段时间便会更新一次。

![]( "cloudera-hadoop-versions")

Cloudera以patch level划分小版本，比如patch level为923.142表示在原生态Apache Hadoop 0.20.2基础上添加了1065个patch（这些patch是各个公司或者个人贡献的，在Hadoop jira上均有记录），其中923个是最后一个beta版本添加的patch，而142个是稳定版发行后新添加的patch。由此可见，patch level越高，功能越完备且解决的bug越多。

Cloudera版本层次更加清晰，且它提供了适用于各种操作系统的Hadoop安装包，可直接使用apt-get或者yum命令进行安装，更加省事。

### 2.2 CDH版本下载

（1） 版本含义介绍：

[https://ccp.cloudera.com/display/DOC/CDH+Version+and+Packaging+Information](https://ccp.cloudera.com/display/DOC/CDH+Version+and+Packaging+Information)

（2）各版本特性查看：

[https://ccp.cloudera.com/display/DOC/CDH+Packaging+Information+for+Previous+Releases](https://ccp.cloudera.com/display/DOC/CDH+Packaging+Information+for+Previous+Releases)

（3）各版本下载：

CDH3：[http://archive.cloudera.com/cdh/3/](http://archive.cloudera.com/cdh/3/)

CDH4：[http://archive.cloudera.com/cdh4/cdh/4/](http://archive.cloudera.com/cdh4/cdh/4/)

注意，Hadoop压缩包在这两个链接中的最上层目录中，不在某个文件夹里，很多人进到链接还找不到安装包！

### 3. 如何选择Hadoop版本

当前Hadoop版本比较混乱，让很多用户不知所措。实际上，当前Hadoop只有两个版本：Hadoop 1.0和Hadoop 2.0，其中，Hadoop 1.0由一个分布式文件系统HDFS和一个离线计算框架MapReduce组成，而Hadoop 2.0则包含一个支持NameNode横向扩展的HDFS，一个资源管理系统YARN和一个运行在YARN上的离线计算框架MapReduce。相比于Hadoop 1.0，Hadoop 2.0功能更加强大，且具有更好的扩展性、性能，并支持多种计算框架。

当我们决定是否采用某个软件用于开源环境时，通常需要考虑以下几个因素：

（1）是否为开源软件，即是否免费。

（2） 是否有稳定版，这个一般软件官方网站会给出说明。

（3） 是否经实践验证，这个可通过检查是否有一些大点的公司已经在生产环境中使用知道。

（4） 是否有强大的社区支持，当出现一个问题时，能够通过社区、论坛等网络资源快速获取解决方法。

如今Hadoop 2.0已经发布了最新的稳定版2.2.0，推荐使用该版本，具体介绍可阅读：“[Hadoop 2.0稳定版本2.2.0新特性剖析](http://dongxicheng.org/mapreduce-nextgen/hadoop-2-2-0/)”，升级方法可参考：“[Hadoop升级方案（二）：从Hadoop 1.0升级到2.0（1）](http://dongxicheng.org/mapreduce-nextgen/hadoop-upgrade-to-version-2/)”。
来源： <[http://dongxicheng.org/mapreduce-nextgen/how-to-select-hadoop-versions/](http://dongxicheng.org/mapreduce-nextgen/how-to-select-hadoop-versions/)>

另外一篇：
# [hadoop的版本问题](http://www.cnblogs.com/xuxm2007/archive/2013/04/04/2999741.html)

现在hadoop的版本比较乱,常常搞不清楚版本之间的关系,下面简单的摘要了,apache hadoop和cloudera hadoop 的版本的演化.

**apache hadoop官方给出的版本说明是:**

**1.0.X -**current stable version, 1.0 release

**1.1.X -**current beta version, 1.1 release

**2.X.X -**current alpha version

**0.23.X -**simmilar to 2.X.X but missing NN HA.

**0.22.X -**does not include security

**0.20.203.X -**old legacy stable version

**0.20.X -**old legacy version

下图来自[http://blog.cloudera.com/blog/2012/01/an-update-on-apache-hadoop-1-0/](http://blog.cloudera.com/blog/2012/01/an-update-on-apache-hadoop-1-0/)

可以简单说明apache hadoop和cloudera hadoop版本之间的变化关系
 
[![diagram-3]( "diagram-3")](http://images.cnitblog.com/blog/73083/201304/04194843-67590ee16a15440497b1153b688fad40.png)
 
0.20.x版本最后演化成了现在的1.0.x版本

0.23.x版本最后演化成了现在的2.x版本

hadoop 1.0 指的是1.x(0.20.x),0.21,0.22

hadoop 2.0 指的是2.x,0.23.x

CDH3,CDH4分别对应了hadoop1.0 hadoop2.0

董的博客有2篇文章也很清晰的解释了,hadoop版本以及各自的版本特性:

[http://dongxicheng.org/mapreduce-nextgen/how-to-select-hadoop-versions/](http://dongxicheng.org/mapreduce-nextgen/how-to-select-hadoop-versions/)

[http://dongxicheng.org/mapreduce-nextgen/hadoop-2-0-terms-explained/](http://dongxicheng.org/mapreduce-nextgen/hadoop-2-0-terms-explained/)
 
[![apache-hadoop-versions]( "apache-hadoop-versions")](http://images.cnitblog.com/blog/73083/201304/04195633-b2c2d402daf94e2187a2130c24d441b1.jpg)
 
最后给出常见的下载hadoop不同版本的地址:

[http://archive.apache.org/dist/hadoop/core/](http://archive.apache.org/dist/hadoop/core/)

[http://archive.cloudera.com/cdh/3/](http://archive.cloudera.com/cdh/3/)

[http://archive.cloudera.com/cdh4/cdh/4/](http://archive.cloudera.com/cdh4/cdh/4/)

 
另外附注一个 hadoop各商业发行版的比较:

[http://www.xiaohui.org/archives/795.html](http://www.xiaohui.org/archives/795.html)
来源： <[hadoop的版本问题 - 阿笨猫 - 博客园](http://www.cnblogs.com/xuxm2007/archive/2013/04/04/2999741.html)>  
