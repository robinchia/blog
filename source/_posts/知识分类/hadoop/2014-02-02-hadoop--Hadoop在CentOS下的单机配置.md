---
layout: post
title: "Hadoop在CentOS下的单机配置"
categories: hadoop
tags: 
 - hadoop
--- 

# Hadoop在CentOS下的单机配置

前言的前言

如果你做某件从未接触过的事的时候很纠结很曲折，那么为你自己高兴吧，你能学到很多东西！

以下的东西都是贴图，所以你们只有手敲了。我也不清楚这个东西是不是应该花很多时间去做，有得有失，某些付出不知道到底值多少。据**说一下午都能配出来，谁叫我傻呢，谁叫我蠢呢，不过该走的路咱还是踏实点走吧，不去跟人比。所以现在我把细节写出来，供大家参考，让你能在两小时内完成。希望它能帮助你学习，而不是让你变得更依赖。如有不对的地方请指正，我也是初学者。谢谢！

前言

做事总有个原因吧，那么我们为什么安装单机的[Hadoop](http://www.linuxidc.com/topicnews.aspx?tid=13 "Hadoop")呢？因为官网上有安装单机hadoop，因为某权威网站有[Ubuntu](http://www.linuxidc.com/topicnews.aspx?tid=2 "Ubuntu")下安装单机hadoop，但是没有一个网站有[CentOS](http://www.linuxidc.com/topicnews.aspx?tid=14 "CentOS")下单机安装，所以我现在CentOS下面单机配置hadoop。

其实单机hadoop的安装没有什么实质的用处，主要用于初学者熟悉指令，以及对hadoop配置有个大致了解，以便于安装分布式。

首先，我们来理清思路。

目的：安装hadoop

Hadoop是需要在java环境下面运行，所以，首先要保证你的系统下面装有JDK。那么步骤是：配置SSH——安装JDK——安装hadoop（当然你愿意先安装它也完全没问题）——配置java的环境变量（需要知道java的安装路径）——配置namenode下面3个配置文件——格式化hadoop——启动hadoop。

我们用一般用户登录，然后切换到root下面，因为权限的问题，这样相比下会更安全点，注意linux下面尽量不要用root登录。

开始了

所需软件

CentOS、Java、Hadoop安装软件。本人用的版本为Linux Cent OS 5.5、jdk1.6.0_13、hadoop-0.20.2.tar.gz。

我们要提醒一下，linux下面很注意权限问题。我们应该以一般用户登录，然后切换至root用户才能使用某些命令，并能使系统处于相对安全的状态。

所以做如下处理，来切换到root用户。
![Hadoop在CentOS下的单机配置]()

1.       SSH无密码验证配置（更建议放到最后一步进行，为非核心步骤，只是方便而已）

Hadoop 需要使用SSH 协议。

namenode 将使用SSH 协议启动 namenode和datanode 进程，配置 SSH localhost无密码验证。

(1)生成密钥对
![Hadoop在CentOS下的单机配置]()

前面是为了切换到root下面

通过以上命令将在/root/.ssh/ 目录下生成id_rsa私钥和id_rsa.pub公钥。

（2）进入/root/.ssh目录在namenode节点下做如下配置：
![Hadoop在CentOS下的单机配置]()

可以用键入ssh localhost命令来看已经连接，会有这样的显示

![Hadoop在CentOS下的单机配置]()

注意最后一行！跟第一行比较，发现我们用ssh进入到localhost了！但已不需要输入密码了。（这样说你们也一定不知道，如果把这个放到最后一步做就会更懂。）

本人认为这样设置会发现后面操作不会让你老是输入密码，并非核心步骤，大家可以试试先配置其它的，再到这一步，就明白为什么了。
来源： <[http://www.linuxidc.com/Linux/2011-07/37992.htm](http://www.linuxidc.com/Linux/2011-07/37992.htm)> 

2.       安装JDK

(1)下载JDK

建议到sun的官网上下载,地址如下：[https://cds.sun.com/is-bin/INTERSHOP.enfinity/WFS/CDS-CDS_Developer-Site/en_US/-/USD/ViewFilteredProducts-SingleVariationTypeFilter](https://cds.sun.com/is-bin/INTERSHOP.enfinity/WFS/CDS-CDS_Developer-Site/en_US/-/USD/ViewFilteredProducts-SingleVariationTypeFilter)

选择jdk-6u24-linux-i586.bin

(2)安装JDK

我把它装在/opt里面,所以切换到/opt下面。在命令行输入如下指令来执行JDK文件:

![]()

权限有问题！我们看看它的权限
![Hadoop在CentOS下的单机配置]()

没有可执行的x标志，那么我们可以通过命令改变。如下操作：

![Hadoop在CentOS下的单机配置]()

看到没，变成绿色的了。有人是把所有者、组、其他用户对该文件的权限都设置为可执行，不过我在这就只让它能被所有者执行就行了。（该文件可能不管紧要，其他重要的文件，我认为不能像他们那样设置。）

现在我们再执行它
![Hadoop在CentOS下的单机配置]()

没有问题了吧，在开始解包了。

(1)Java环境变量配置

输入vim /etc/profile，添加如下的内容（在此我建议所有的都编辑都用vim取代vi，因为它有颜色变化，有语法问题的话很容易发现。）
![Hadoop在CentOS下的单机配置]()

保存好退出后，我们需要改变一下改文件的权限，并执行一下该文件使配置生效。（注：大家一定要小心版本和路径啊，）

![Hadoop在CentOS下的单机配置]()

配置完后执行java –version

![Hadoop在CentOS下的单机配置]()

显示java的版本

来源： <[http://www.linuxidc.com/Linux/2011-07/37992p2.htm](http://www.linuxidc.com/Linux/2011-07/37992p2.htm)> 

3. 安装[Hadoop](http://www.linuxidc.com/topicnews.aspx?tid=13 "Hadoop")

（1）下载hadoop

到如下网址下载hadoop，存到/opt中,当然也可以手动点击下载。
![Hadoop在CentOS下的单机配置]()

（2）解压hadoop到/opt/hadoop下面，当然没有现成的opt/hadoop这个目录，所以要新建。

![Hadoop在CentOS下的单机配置]()

然后解压到/opt/hadoop下

![Hadoop在CentOS下的单机配置]()

3.1   进入/opt/hadoop/hadoop-0.20.2/conf，配置Hadoop配置文件。

（1）配置java环境：修改hadoop-env.sh文件
![Hadoop在CentOS下的单机配置]()

在最后加上这样的内容

![Hadoop在CentOS下的单机配置]()

(2)配置Namenode的三个配置文件core-site.xml, hdfs-site.xml, mapred-site.xml。对应于/src/core/core-default.xml，但不能直接修改它，（hadoop启动时先读取src下面的core/core-default.xml,hdfs/hdfs-default.xml,apred/mapred-default.xml，里面缺失的变量由conf下面的三个-site文件提供）

这部分的配置建议参考官方网站（建议大家多上官网），如下：[http://hadoop.apache.org/common/docs/current/single_node_setup.html](http://hadoop.apache.org/common/docs/current/single_node_setup.html)

(2.1)配置core
![Hadoop在CentOS下的单机配置]()

（2.2）配置hdfs

![Hadoop在CentOS下的单机配置]()

（2.3）配置mapred

![Hadoop在CentOS下的单机配置]()

来源： <[http://www.linuxidc.com/Linux/2011-07/37992p3.htm](http://www.linuxidc.com/Linux/2011-07/37992p3.htm)>
 

4、启动[Hadoop](http://www.linuxidc.com/topicnews.aspx?tid=13 "Hadoop")

(1)格式化namenode，（注意看清路径哦）
![Hadoop在CentOS下的单机配置]()

(2) 启动Hadoop守护进程

![Hadoop在CentOS下的单机配置]()

这就表示你配置成功了，上面的一个都不能少

这时候你就可以点击进入下面的网站了。

NameNode - http://localhost:50070/

JobTracker - http://localhost:50030/

good luck

其实刚刚接触一个东西可能会觉得不好弄，一旦你弄好了以后就会很顺手。那时候你会告诉自己，这个东西装起来怎么这么白痴啊！赶紧开始下一个工作！加油！
来源： <[http://www.linuxidc.com/Linux/2011-07/37992p4.htm](http://www.linuxidc.com/Linux/2011-07/37992p4.htm)> 
