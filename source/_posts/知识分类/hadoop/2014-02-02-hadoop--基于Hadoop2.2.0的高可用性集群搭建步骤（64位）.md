---
layout: post
title: "基于Hadoop 2.2.0的高可用性集群搭建步骤（64位）"
categories: hadoop
tags: 
 - hadoop
--- 

# 基于Hadoop 2.2.0的高可用性集群搭建步骤（64位）

 内容概要: CentSO_64bit集群搭建， hadoop2.2(64位)编译，安装，配置以及测试步骤

新版亮点:基于yarn计算框架和高可用性DFS的第一个稳定版本。

注1：官网只提供32位release版本, 若机器为64位，需要手动编译。

注2：目前网上传的2.2版本的安装步骤几乎都有问题，没有一个版本是完全正确的。若不懂新框架内部机制，不要照抄网传的版本。

**0. 编译前的准备**

**虚拟机vmware准备，64bit CentOS准备**

**节点ip**

cluster1   172.16.102. 201

cluster2  172.16.102. 202

cluster3  172.16.102. 203

cluster4  172.16.102. 204

**各节点职能划分**     

cluster1        resourcemanager, nodemanager,

proxyserver,historyserver, datanode, namenode,

cluster2        datanode, nodemanager

cluster3 datanode, nodemanager

cluster4 datanode, nodemanager

**说明**

以下关于修改hostname, 设置静态ip, ssh免登陆，关闭防火墙等步骤，放在文章末尾，这些内容并不是本文讨论的重点。

**1. hadoop2.2****编译**
1说明：标准的

bash

提示符，root用户为

'#'

，普通用户为

'%'

，由于博客编辑器的缘故，

'#'

提示符会被默认为comment, 因此在这篇博文中不再区分root和普通user的提示符， 默认全部为

'$'

因为我们安装的CentOS是64bit的，而官方release的hadoop2.2.0版本没有对应的64bit安装包，故需要自行编译。

首先需要去oracle下载64位jdk:
1

2
$

su

root

$wget http:

//download

.oracle.com

/otn-pub/java/jdk/7u45-b18/jdk-7u45-linux-x64

.

tar

.gz

注: prompt（提示符）为%默认为当前用户， #则为root,注意以下各步骤中的prompt类型。

下面为hadoop编译步骤（注：中间部分的文本框里内容提要只是一些补充说明，不要执行框里的命令）

1.1 BOTPROTO改为”dhcp”
1

2
$

su

root

$

sed

–i  s

/static/dhcp/g

/etc/sysconfig/network-scripts/ifcfg-eth0

#servicenetwork restart

1.2 下载hadoop2.2.0 源码

1

2
3$

su

grid

$

cd

~
wget  http:

//apache

.dataguru.cn

/hadoop/common/stable/hadoop-2

.2.0-src.

tar

.gz

1.3 安装maven

1

2
3

4
5$

su

root

$

cd

/opt
wget http:

//apache

.fayea.com

/apache-mirror/maven/maven-3/3

.1.1

/binaries/apache-maven-3

.1.1-bin.

tar

.gz

$

tar

zxvf apache-maven-3.1.1-bin.

tar

.gz
$

cd

apache-maven-3.1.1

****修改系统环境变量有两种方式，修改/etc/profile， 或者在/etc/profile.d/下添加定制的shell文件，

鉴于profile文件的重要性，尽量不要在profile文件里添加内容，官方建议采用第二种，以保证profile文件的绝对安全。

下面采用第二种方式：

首先，创建一个简单shell脚脚本并添加相关内容进去：
1

2
$

cd

/etc/profile

.d/

$

touch

maven.sh     其次，maven.sh里添加内容如下：1

2
3#environmentvariable settings for mavn2

export

MAVEN_HOME=

'/opt/apache-maven-3.1.1'

3
export

PATH=$MAVEN_HOME

/bin

:$PATH

最后，source一下

1$

source

/etc/profile

<br><br>$

source

/etc/profile1

2
3<em

id

=

"__mceDel"

>$mvn -version

Apache Maven 3.1.1
<

/em

>

1.4 安装protobuf

注意apache官方网站上的提示“NOTE: You will need protoc 2.5.0 installed.”
1

2
3

4
5

6
$

su

root

$

cd

/opt
wget https:

//protobuf

.googlecode.com

/files/protobuf-2

.5.0.

tar

.bz2

$

tar

xvf protobuf-2.5.0.

tar

.bz2 (注意压缩文件后缀， maven安装包是—

gzip

文件，解压时需加–z)
$

cd

protobuf-2.5.0

.

/configure

安装protobuf时提示报错”configure: error: C++ preprocessor "/lib/cpp" failssanity check”

安装gcc
1$yum

install

gcc

1.5 编译hadoop

首先从官网下载hadoop2.2.0source code:
1

2
3$

su

grid;

$

cd

~grid/
wget http:

//apache

.dataguru.cn

/hadoop/common/stable/hadoop-2

.2.0-src.

tar

.gz

好了，痛苦的编译过程来了。

解压之：
1

2
$

tar
 

zxvf hadoop-2.2.0-src.

tar

.gz

$

cd

hadoop-2.2.0-src

1.6 给maven指定国内镜像源

1.6.1. 切换root权限, 修改/opt/apache-maven-3.1.1/conf/settings.xml
1

2
$

su

root

$vim

/opt/apache-maven-3

.1.1

/conf/settings

.xml

修改1. 在<mirrors>…</mirrors>里添加国内源（注意，保留原本就有的<mirrors>...</mirrors>）：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")
1  <mirrors> 2     <mirror>3         <id>nexus-osc</id>4         <mirrorOf>*</mirrorOf>5     <name>Nexusosc</name>6     <url>http://maven.oschina.net/content/groups/public/</url>7     </mirror>8 </mirrors>

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

 

修改2. 在<profiles>标签中增加以下内容（保留原来的<profiles>…</profiles>， jdk版本根据用户的情况填写）

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")
 1  <profile> 2       <id>jdk-1.4</id> 3       <activation> 4         <jdk>1.4</jdk> 5       </activation> 6       <repositories> 7         <repository> 8           <id>nexus</id> 9           <name>local private nexus</name>10           <url>http://maven.oschina.net/content/groups/public/</url>11           <releases>12             <enabled>true</enabled>13           </releases>14           <snapshots>15             <enabled>false</enabled>16           </snapshots>17         </repository>18       </repositories>19       <pluginRepositories>20         <pluginRepository>21           <id>nexus</id>22           <name>local private nexus</name>23           <url>http://maven.oschina.net/content/groups/public/</url>24           <releases>25             <enabled>true</enabled>26           </releases>27           <snapshots>28             <enabled>false</enabled>29           </snapshots>30         </pluginRepository>31       </pluginRepositories>32     </profile>33 </profiles>

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

   1.6.2 将刚才修改的配置文件拷到当前用户home目录下

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")
$ su grid $ sudo cp /opt/apache-maven-3.1.1/conf/settings.xml ~/.m2/#若提示该用户不在sudoers里，执行以下步骤： $ su root  #在sudoers里第99行添加当前用户（下面行号不要加）： $ cat /etc/sudoers98  root     ALL=(ALL)       ALL99  grid     ALL=(ALL)       ALL

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

1.7 现在执行官方的clean步骤： $mvn clean install –DskipTests

漫长的等待后发现安装一切正常。

1.8 安装3个依赖包
1

2
3Cmake

ncurses-devel
openssl-devel

执行以下步骤：

1

2
3

4
$

su

root

$yum

install

ncurses-devel
$yum

install

openssl-devel

$yum

install

cmake89

以上安装完成后，切回用户grid：

1

2
$

su

grid

$

cd

~

/hadoop-2

.2.0-src

****1.9 所有依赖已安装完毕，开始编译

1$mvn package-Pdist,native -DskipTests -Dtar

漫长的等待后，编译成功，查看结果：

![](http://m1.img.libdd.com/farm4/d/2013/1114/11/160569527B88184A5CFD1B4C6D260A60_B500_900_500_214.png)

一切正常。至此，hadoop2.2.0编译完成。

****1.10 验证

下面验证编译结果是否符合预期, 注意我们当前是在目录~/hadoop-2.2.0-src下，
1

2
3$

cd

hadoop-dist/

$

ls
pom.xml  target

以上为maven编译的配置文件

1

2
3

4
5

6
7$

cd

target

$

ls

-sF
total 276M

4.0K antrun/                    92M hadoop-2.2.0.

tar

.gz            4.0K maven-archiver/
4.0K dist-layout-stitching.sh  4.0K hadoop-dist-2.2.0.jar          4.0K

test

-

dir

/

4.0K dist-

tar

-stitching.sh     184M hadoop-dist-2.2.0-javadoc.jar
4.0K hadoop-2.2.0/             4.0K javadoc-bundle-options/

以上为maven编译后自动生成的目录文件，进入hadoop-2.2.0:

1

2
3$

cd

hadoop-2.2.023

$

ls
bin  etc  include  lib libexec  sbin  share

这才是和官方release2.2.0版本（官方只有32bit版本）的相同的目录结构。

1.10.1 下面主要验证两项：

****a.验证版本号
1

2
3

4
5

6
7$bin

/hadoop

version

Hadoop 2.2.0
Subversion Unknown -r Unknown

Compiled by grid on 2013-11-06T13:51Z
Compiled with protoc 2.5.0

From

source

with checksum 79e53ce7994d1628b240f09af91e1af4
This

command

was run using

/home/grid/hadoop-2

.2.0-src

/hadoop-dist/target/hadoop-2

.2.0

/share/hadoop/common/hadoop-common-2

.2.0.jar

可以看到hadoop版本号，编译工具（protoc2.5.0版本号与官方要求一致）以及编译日期.

****b.验证hadoop lib的位数
1

2
3

4
5

6
$

file
 

lib

//native/

*

lib

//native/libhadoop

.a:        current ar archive
lib

//native/libhadooppipes

.a:   current ar archive

lib

//native/libhadoop

.so:       symbolic link to `libhadoop.so.1.0.0'
lib

//native/libhadoop

.so.1.0.0:<strong>ELF 64-bitLSB<

/strong

> shared object, x86-64, version 1 (SYSV), dynamically linked, notstripped1011 lib

//native/libhadooputils

.a:   current ar archive1213 lib

//native/libhdfs

.a:          current ar archive1415 lib

//native/libhdfs

.so:         symbolic link to `libhdfs.so.0.0.0'

lib

//native/libhdfs

.so.0.0.0:   <strong>ELF 64-bit LSB shared object<

/strong

>, x86-64,version 1 (SYSV), dynamically linked, not stripped

看到黑色的“ELF-64bit LSB”证明64bit hadoop2.2.0初步编译成功，查看我们之前的hadoop0.20.3版本，会发现lib//native/libhadoop.so.1.0.0是32bit，这是不正确的！。^_^

**2. hadoop2.2****配置**

******2.1 home设置**

为了和MRv1区别， 2.2版本的home目录直接命名为yarn:
1

2
3

4
$

su

hadoop

$

cd

~
$

mkdir

–p yarn

/yarn_data67

$

cp

–a ~hadoop

/hadoop-2

.2.0-src

/hadoop-dist/target/hadoop-2

.2.0  ~hadoop

/yarn

**2.2 环境变量设置**

~/.bashrc里添加新环境变量：

1

2
3

4
5

6
7

8
9

10
11

12
# javaenv

export

JAVA_HOME=

"/usr/java/jdk1.7.0_45"

exportPATH=

"$JAVA_HOME/bin:$PATH"
# hadoopvariable settings

export

HADOOP_HOME=

"$HOME/yarn/hadoop-2.2.0"
export

HADOOP_PREFIX=

"$HADOOP_HOME/"

export

YARN_HOME=$HADOOP_HOME
export

HADOOP_MAPRED_HOME=

"$HADOOP_HOME"

export

HADOOP_COMMON_HOME=

"$HADOOP_HOME"
export

HADOOP_HDFS_HOME=

"$HADOOP_HOME"

export

HADOOP_CONF_DIR=

"$HADOOP_HOME/etc/hadoop/"
export

YARN_CONF_DIR=$HADOOP_CONF_DIR

export

PATH=

"$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH"

以上操作注意2点：

1. jdk一定要保证是64bit的

2.HADOOP_PREFIX极其重要，主要是为了兼容MRv1，优先级最高（比 如寻找conf目录，即使我们配置了HADOOP_CONF_DIR,启动脚本依然会优先从$HADOOP_PREFIX/conf/里查找），一定要保 证此变量正确配置(也可不设置，则默认使用HADOOP_HOME/etc/hadoop/下的配置文件)

******2.3 改官方启动脚本的bug**

****说明：此版本虽然是release稳定版，但是依然有非常弱智的bug存在。

正常情况下，启动守护进程$YARN_HOME/sbin/hadoop-daemons.sh中可以指定node.

我们看下该启动脚本的说明：
1

2
$sbin

/hadoop-daemons

.sh

Usage:hadoop-daemons.sh [--config confdir] [--hosts hostlistfile] [start|stop]

command

args...

可以看到--hosts可以指定需要启动的存放节点名的文件名：

但这是无效的，此脚本调用的$YARN_HOME/libexec/hadoop-config.sh脚本有bug.

先建一个文件datanodefile 添加datanode节点，放入conf/文件夹下，然后执行一下启动脚本：
1

2
$sbin

/hadoop-daemons

.sh --hosts datanodefile start datanode

at:

/home/grid/yarn/hadoop-2

.2.0

/etc/hadoop//126571

:No such

file

or directory

分析脚本，定位到嵌套脚本$YARN_HOME/libexec/hadoop-config.sh第96行：

196        

export

HADOOP_SLAVES=

"${HADOOP_CONF_DIR}/$$1"

看到红色部分是不对的，应该修改为：

196        

export

HADOOP_SLAVES=

"${HADOOP_CONF_DIR}/$1"

备注1：此版本11月初发布至今，网上教程不论中文还是英文，均未提及此错误。

备注2：$YARN_HOME/libexec/hadoop-config.sh中分析hostfile的逻辑非常脑残：
1

2
3

4
593    

if

[

"--hosts"

=

"$1"

]

94    

then
95        

shift

96        

export

HADOOP_SLAVES=

"${HADOOP_CONF_DIR}/%$1"
97        

shift

因此，用户只能将自己的hostfile放在${HADOOP_CONF_DIR}/ 下面，放在其它地方是无效的。

备注3：按照$YARN_HOME/libexec/hadoop-config.sh脚本的逻辑，还有一种方式指定host
1$hadoop-daemons.sh –hostnames cluster1 start datanode

注意，因为bash脚本区分输入参数的分割符为\t或\s，所以限制了此种方式只能指定一个单独的节点

总结以上分析和修改步骤：
1

2
3

4
5$

cd

$YARN_HOME

/libexec/

$vim hadoop-config.sh
#修改第96行代码为：

export

HADOOP_SLAVES=

"${HADOOP_CONF_DIR}/$1"
#保存退出vim

******2.4 配置文件设置**

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")
 1 <!--$YARN_HOME/etc/hadoop/core-site.xml--> 2                                                                                                                                                                                                                                  3 <?xml version="1.0"?> 4 <?xml-stylesheet type="text/xsl" href="configuration.xsl"?> 5                                                                                                                                                                                                                                  6 <configuration> 7 <property> 8 <name>fs.defaultFS</name> 9   <value>hdfs://cluster1:9000</value>10 </property>11                                                                                                                                                                                                                                 12 <property>13 <name>hadoop.tmp.dir</name>14   <value>/home/grid/hadoop-2.2.0-src/hadoop-dist/target/hadoop-2.2.0//yarn_data/tmp/hadoop-grid</value>15 </property>16 </configuration>17   备注1：注意fs.defaultFS为新的变量，代替旧的：fs.default.name18 19  备注2：tmp文件夹放在我们刚才新建的$HOME/yarn/yarn_data/下面。20 21     <!--$YARN_HOME/etc/hadoop/hdfs-site.xml-->22 <configuration>23     <property>24     <name>dfs.replication</name>25     <value>3</value>26     </property>27 </configuration>

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

 

****备注1. 新：dfs.namenode.name.dir，旧：dfs.name.dir，新：dfs.datanode.name.dir，旧：dfs.data.dir

****备注2. dfs.replication确定 data block的副本数目，hadoop基于rackawareness(机架感知)默认复制3份分block,（同一个rack下两个，另一个rack下一 份，按照最短距离确定具体所需block, 一般很少采用跨机架数据块，除非某个机架down了）

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")
 1     <!--$YARN_HOME/etc/hadoop/yarn-site.xml--> 2                                                                                                                                                                                                                             3 <configuration> 4    <property> 5      <name>yarn.nodemanager.aux-services</name> 6      <value>mapreduce_shuffle</value> 7   </property> 8                                                                                                                                                                                                                             9   <property>10      <name>yarn.resourcemanager.address</name>11      <value>cluster1:8032</value>12   </property>13                                                                                                                                                                                                                            14   <property>15       <name>yarn.resourcemanager.resource-tracker.address</name>16       <value>cluster1:8031</value>17   </property>18                                                                                                                                                                                                                            19   <property>20       <name>yarn.resourcemanager.admin.address</name>21       <value>cluster1:8033</value>22   </property>23                                                                                                                                                                                                                            24   <property>25       <name>yarn.resourcemanager.scheduler.address</name>26       <value>cluster1:8030</value>27   </property>28                                                                                                                                                                                                                            29   <property>30       <name>yarn.nodemanager.loacl-dirs</name>31       <value>/home/grid/hadoop-2.2.0-src/hadoop-dist/target//hadoop-2.2.0/yarn_data/mapred/nodemanager</value>32       <final>true</final>33   </property>34                                                                                                                                                                                                                            35   <property>36       <name>yarn.web-proxy.address</name>37       <value>cluster1:8888</value>38   </property>39                                                                                                                                                                                                                            40   <property>41      <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>42      <value>org.apache.hadoop.mapred.ShuffleHandler</value>43   </property>44 </configuration>

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

 

**在此特意提醒：**

(1) 目前网上盛传的各种hadoop2.2的基于分布式集群的安装教程大部分都是错的,hadoop2.2配置里最重要的也莫过于yarn-site.xml里的变量了吧？

(2)不同于单机安装，基于集群的搭建必须设置

yarn-site.xml可选变量yarn.web-proxy.address
和

yarn.nodemanager.loacl-dirs
外的其它所有变量！后续会专门讨论yarn-site.xml里各个端口配置的含义。

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")
1 <!--$YARN_HOME/etc/hadoop/mapred-site.xml-->2                                                                                                                                                                                                            3 <configuration>4     <property>5         <name>mapreduce.framework.name</name>6         <value>yarn</value>7     </property>8 </configuration>

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

 

备注1：新的计算框架取消了实体上的jobtracker, 故不需要再指定mapreduce.jobtracker.addres，而是要指定一种框架，这里选择yarn. 备注2：hadoop2.2.还支持第三方的计算框架，但没怎么关注过。
          配置好以后将$HADOOP_HOME下的所有文件，包括hadoop目录分别copy到其它3个节点上。

**2.5****各节点功能规划**

确保在每主机的/etc/hosts里添加了所有node的域名解析表（i.e.cluster1   198.0.0.1）；iptables已关闭；/etc/sysconfig/network-script/ifcfg-eth0里 BOTPROTO=static；/etc/sysconfig/network文件里已设置了各台主机的hostname, 静态ip地址，且已经重启过每台机器；jdk和hadoop都为64bit；ssh免登陆已配置；完成以上几项后，就可以启动hadoop2.2.0了。

注意到从头到尾都没有提到Master, Slave,也没有提到namenode,datanode,是因为，新的计算框架，新的hdfs中不存在物理上的Master节点，所有的节点都是等价的。

各节点职能划分在篇首已有说明， 在此重提一下：
1

2
3

4
cluster1    resourcemanager, nodemanager, proxyserver,historyserver, datanode, namenode,

cluster2    datanode, nodemanager
cluster3    datanode, nodemanager

cluster4    datanode, nodemanager

**2.6 hdfs 格式化**

1$bin

/hdfs

namenode –

format

(注意：hadoop 2.2.0的格式化步骤和旧版本不一样，旧的为 $YARN_HOME/bin/hadoop namenode –format)

**2.7 hadoop整体启动**

****启动方式（1）分别登录各自主机开启

在cluster1节点上，分别启动resourcemanager,nodemanager, proxyserver, historyserver, datanode, namenode,

在cluster2, cluster3, cluster4 节点主机上，分别启动datanode,nodemanager

**备注**：如果resourcemanager是 独立的，则除了resourcemanager,其余每一个节点都需要一个nodemanager，我们可以在$YARN_HOME/etc /hadoop/下新建一个nodehosts, 在里面添加所有的除了resourcemanager外的所有node，因为此处我们配置的resourcemanager和namenode是同一台主 机，所以此处也需要添加nodemanager

执行步骤如下：
1

2
3

4
5

6
7

8
9

10
11

12
13$

hostname

#查看host名字

cluster1
$sbin

/hadoop-daemon

.sh  --script hdfs start namenode

# 启动namenode

$sbin

/hadoop-daemon

.sh --script hdfs start datanode

# 启动datanode
$sbin

/yarndaemon

.shstart nodemanager

#启动nodemanager

$sbin

/yarn-daemon

.sh   start resourcemanager

# 启动resourcemanager
$sbin

/yarn-daemon

.shstart proxyserver

# 启动web App proxy, 作用类似jobtracker,若yarn-site.xml里没有设置yarn.web-proxy.address的host和端口，或者设置了和 resourcemanager相同的host和端口，则hadoop默认proxyserver和resourcemanager共享 host:port

$sbin

/mr-jobhistory-daemon

.sh   start historyserver

#你懂得
$

ssh

cluster2 

#登录cluster2

$

hostname

#查看host名字cluster2
$sbin

/yarndaemon

.shstart nodemanager

# 启动nodemanager

$sbin

/hadoop-daemon

.sh  --script hdfs start datanode

# 启动datanode
$

ssh

cluster3 

#登录cluster3...# cluster2, cluster3, cluster4启动方式和cluster2一样。

启动方式（2）使用hadoop自带的批处理脚本开启

Step1.确认已登录cluster1:
1$

hostname

cluster1

在$YARN_HOME/etc/hadoop/下新建namenodehosts,添加所有namenode节点

1

2
    

$

cat

$YARN_HOME

/etc/hadoop/namenodehosts

cluster1

在$YARN_HOME/etc/hadoop/下新建datanodehosts,添加所有datanode节点

1

2
3

4
$

cat

$YARN_HOME

/etc/hadoop/datanodehosts

cluster2
cluster3

cluster4

在$YARN_HOME/etc/hadoop/下新建nodehosts,添加所有datanode和namenode节点

1

2
3

4
5$

cat

$YARN_HOME

/etc/hadoop/datanodehosts

cluster1
cluster2

cluster3
cluster4

**备注**：以上的hostfile名字是随便起的，可以是任意的file1,file2,file3, 但是必须放在$YARN_HOME/etc/hadoop/下面！

Step2.执行脚本
1

2
3

4
5

6
$sbin

/hadoop-daemons

.sh--hosts namenodehosts --script  hdfsstart  namenode

$sbin

/hadoop-daemons

.sh--hosts datanodehosts --script  hdfsstart  datanode
$sbin

/yarn-daemons

.sh--hostnames cluster1 start resourcemanager

$sbin

/yarn-daemons

.sh--hosts allnodehosts start nodemanager
$sbin

/yarn-daemons

.sh--hostnames cluster1 start proxyserver

$sbin

/mr-jobhistory-daemon

.sh   start  historyserver

**在这里不得不纠正一个其他教程上关于对hadoop2.2做相关配置和运行时的错误!**

我们在core-site.xml指定了default filesystem为cluster1, 并指定了其节点ip（或节点名）和端口号， 这意味着若启动hadoop时不额外添加配置（启动hadoop时命令行里加--conf指定用户配置文件的存放位置)，则默认的namenode就一 个，那就是cluster1, 如果随意指定namenode必然会出现错误！

如果你要还要再启动一个新的namenode(以cluster3为例)，则必须如下操作：

a. 新建一个core-site.xml，添加fs.defaultFS的value为hdfs://cluster3:9000

b. command：
1$sbin

/hadoop-daemon

.sh --hostnames cluster3 --conf you_conf_dir --script hdfs --start namenode

我们之前启动的namenode为cluster1, 假如要查看放在此文件系统根目录下的文件input_file，则它的完整路径为 hdfs://cluster1:9000/input_file

Step3.查看启动情况

在cluster1上：
1

2
3

4
5

6
$jps

22411 NodeManager
23356 Jps

22292 ResourceManager
22189 DataNode

22507 WebAppProxyServer

在cluster2上

1

2
3

4
$jps

8147 DataNode
8355 Jps

8234 NodeManagercluster3，cluster4上和cluster2一样。

****Step4.查看各节点状态以及yarncluster运行状态

（1）查看各节点状态

FireFox进入： http://cluster1:50070(cluster1为namenode所在节点)

在主页面（第一张图）上点击Live Node，查看（第二张图）上各Live Node状态：

![](http://m1.img.libdd.com/farm4/d/2013/1114/14/5D0856F11A09F93C0BD97246AE6DEE43_B500_900_500_218.png)

（2）查看resourcemanager上cluster运行状态

Firefox进入：http://cluster2:8088(node1为resourcemanager所在节点)

![](http://m3.img.libdd.com/farm4/d/2013/1114/14/5B46215F81D52CF3C3DB80D2DEFF8770_B500_900_500_195.png)

** **Step5. Cluster上MapReduce测试

现提供3个test cases

**Test Case 1 testimated_value_of_pi**

command:
$sbin/yarnjar $YARN_HOME/share/hadoop//mapreduce/hadoop-mapreduce-examples-2.2.0.jar \pi 101000000

console输出：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")
Number of Maps  =10                        Samples per Map = 1000000                         Wrote input for Map #0                          Wrote input for Map #1                          Wrote input for Map #2                         Wrote input for Map #3                        Wrote input for Map #4                       Wrote input for Map #5                         Wrote input for Map #6                        Wrote input for Map #7                        Wrote input for Map #8                        Wrote input for Map #9                        Starting Job                        13/11/06 23:20:07 INFO Configuration.deprecation: mapred.map.tasksis deprecated. Instead, use mapreduce.job.maps                        13/11/06 23:20:07 INFO Configuration.deprecation:mapred.output.key.class is deprecated. Instead, usemapreduce.job.output.key.class                         13/11/06 23:20:07 INFO Configuration.deprecation:mapred.working.dir is deprecated. Instead, use mapreduce.job.working.dir                         13/11/06 23:20:11 INFO mapreduce.JobSubmitter: Submittingtokens for job: job_1383806445149_0001                         13/11/06 23:20:15 INFO impl.YarnClientImpl: Submittedapplication application_1383806445149_0001 to ResourceManager at /0.0.0.0:8032                       13/11/06 23:20:16 INFO mapreduce.Job: The url to trackthe job: http://Node1:8088/proxy/application_1383806445149_0001/                        13/11/06 23:20:16 INFO mapreduce.Job: Running job:job_1383806445149_0001                         13/11/06 23:21:09 INFO mapreduce.Job: Jobjob_1383806445149_0001 running in uber mode : false                        13/11/06 23:21:10 INFO mapreduce.Job:  map 0% reduce 0%                          13/11/06 23:24:28 INFO mapreduce.Job:  map 20% reduce 0%                         13/11/06 23:24:30 INFO mapreduce.Job:  map 30% reduce 0%                          13/11/06 23:26:56 INFO mapreduce.Job:  map 57% reduce 0%                          13/11/06 23:26:58 INFO mapreduce.Job:  map 60% reduce 0%                          13/11/06 23:28:33 INFO mapreduce.Job:  map 70% reduce 20%                         13/11/06 23:28:35 INFO mapreduce.Job:  map 80% reduce 20%                         13/11/06 23:28:39 INFO mapreduce.Job:  map 80% reduce 27%                         13/11/06 23:30:06 INFO mapreduce.Job:  map 90% reduce 27%                         13/11/06 23:30:09 INFO mapreduce.Job:  map 100% reduce 27%                         13/11/06 23:30:12 INFO mapreduce.Job:  map 100% reduce 33%                         13/11/06 23:30:25 INFO mapreduce.Job:  map 100% reduce 100%                          13/11/06 23:30:54 INFO mapreduce.Job: Jobjob_1383806445149_0001 completed successfully                          13/11/06 23:31:10 INFO mapreduce.Job: Counters: 43                                    File SystemCounters                                             FILE:Number of bytes read=226                                             FILE:Number of bytes written=879166                                              FILE:Number of read operations=0                                             FILE:Number of large read operations=0                                            FILE:Number of write operations=0                                             HDFS:Number of bytes read=2590                                               HDFS:Number of bytes written=215                                               HDFS:Number of read operations=43                                               HDFS:Number of large read operations=0                                                 HDFS:Number of write operations=3                                    JobCounters                                              Launchedmap tasks=10                                                Launchedreduce tasks=1                                               Data-localmap tasks=10                                                Totaltime spent by all maps in occupied slots (ms)=1349359                                             Totaltime spent by all reduces in occupied slots (ms)=190811                                    Map-ReduceFramework                                             Mapinput records=10                                              Mapoutput records=20                                                Mapoutput bytes=180                                                 Mapoutput materialized bytes=280                                                Inputsplit bytes=1410                                                 Combineinput records=0                                                Combineoutput records=0                                                  Reduceinput groups=2                                                 Reduceshuffle bytes=280                                              Reduceinput records=20                                                Reduceoutput records=0                                                 SpilledRecords=40                                                 ShuffledMaps =10                                                  FailedShuffles=0                                                MergedMap outputs=10                                                 GCtime elapsed (ms)=45355                                                 CPUtime spent (ms)=29860                                                 Physicalmemory (bytes) snapshot=1481818112                                                 Virtualmemory (bytes) snapshot=9214468096                                                 Totalcommitted heap usage (bytes)=1223008256                                        ShuffleErrors                                              BAD_ID=0                                              CONNECTION=0                                             IO_ERROR=0                                              WRONG_LENGTH=0                                               WRONG_MAP=0                                            WRONG_REDUCE=0                                    File InputFormat Counters                                              BytesRead=1180                                    File OutputFormat Counters                                             BytesWritten=97                           13/11/06 23:31:15 INFO mapred.ClientServiceDelegate:Application state is completed. FinalApplicationStatus=SUCCEEDED. Redirectingto job history server                           Job Finished in 719.041 seconds                           Estimated value of Pi is 3.14158440000000000000

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

 

**说明**：可以看到最后输出值为该job使用了 10个maps, job id为job_1383806445149_000, 最后计算得Pi的值为13.14158440000000000000， job Id分配原则为job_年月日时分_job序列号，序列号从0开始，上限值为1000， task id分配原则为job_年月日时分_job序列号_task序列号_m, job_年月日时分_job序列号_task序列号_r, m代表map taskslot , r代表reduce task slot, task 序列号从0开始，上限值为1000.

**Test Case 2 random_writting**
#command line $sbin/yarnjar $YARN_HOME/share/hadoop//mapreduce/hadoop-mapreduce-examples-2.2.0.jar \randomwriter/user/grid/test/test_randomwriter/out

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

#Console输出摘录：                                                                                                                                                    Running 10 maps.Job started: Wed Nov 0623:42:17 PST 201313/11/0623:42:17 INFO client.RMProxy: Connecting toResourceManager at /0.0.0.0:803213/11/0623:42:19 INFO mapreduce.JobSubmitter: number ofsplits:1013/11/0623:42:20 INFO mapreduce.JobSubmitter: Submittingtokens for job: job_1383806445149_000213/11/0623:42:21 INFO impl.YarnClientImpl: Submittedapplication application_1383806445149_0002 to ResourceManager at /0.0.0.0:803213/11/0623:42:21 INFO mapreduce.Job: The url to trackthe job: http://Master:8088/proxy/application_1383806445149_0002/13/11/0623:42:21 INFO mapreduce.Job: Running job:job_1383806445149_000213/11/0623:42:40 INFO mapreduce.Job: Jobjob_1383806445149_0002 running in uber mode : false13/11/0623:42:40 INFO mapreduce.Job:  map 0% reduce 0%                  13/11/0623:55:02 INFO mapreduce.Job:  map 10% reduce 0%                    13/11/0623:55:14 INFO mapreduce.Job:  map 20% reduce 0%                  13/11/0623:55:42 INFO mapreduce.Job:  map 30% reduce 0%                    13/11/0700:06:55 INFO mapreduce.Job:  map 40% reduce 0%                    13/11/0700:07:10 INFO mapreduce.Job:  map 50% reduce 0%                   13/11/0700:07:36 INFO mapreduce.Job:  map 60% reduce 0%                    13/11/0700:13:47 INFO mapreduce.Job:  map 70% reduce 0%                     13/11/0700:13:54 INFO mapreduce.Job:  map 80% reduce 0%                     13/11/0700:13:58 INFO mapreduce.Job:  map 90% reduce 0%                    13/11/0700:16:29 INFO mapreduce.Job:  map 100% reduce 0%                    13/11/0700:16:37 INFO mapreduce.Job: Jobjob_1383806445149_0002 completed successfully        File OutputFormat Counters                  BytesWritten=10772852496Job ended: Thu Nov 0700:16:40 PST 2013The job took 2062 seconds.

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

**说明**：电脑存储空间足够的话，可以从hdfs里down下来看看。

现只能看一看输出文件存放的具体形式：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")
./bin/hadoopfs -ls /user/grid/test/test_randomwriter/out/Found 11items-rw-r--r--   2 grid supergroup          02013-11-0700:16/user/grid/test/test_randomwriter/out/_SUCCESS-rw-r--r--   2 grid supergroup 10772782142013-11-0623:54 /user/grid/test/test_randomwriter/out/part-m-00000                                                                                                                                                   -rw-r--r--   2 grid supergroup 10772827512013-11-0623:55 /user/grid/test/test_randomwriter/out/part-m-00001                                                                                                                                                   -rw-r--r--   2 grid supergroup 10772802982013-11-0623:55 /user/grid/test/test_randomwriter/out/part-m-00002                                                                                                                                                   -rw-r--r--   2 grid supergroup 10773031522013-11-0700:07 /user/grid/test/test_randomwriter/out/part-m-00003                                                                                                                                                   -rw-r--r--   2 grid supergroup 10772842402013-11-0700:06 /user/grid/test/test_randomwriter/out/part-m-00004                                                                                                                                                   -rw-r--r--   2 grid supergroup 10772866042013-11-0700:07 /user/grid/test/test_randomwriter/out/part-m-00005                                                                                                                                                   -rw-r--r--   2 grid supergroup 10772843362013-11-0700:13 /user/grid/test/test_randomwriter/out/part-m-00006                                                                                                                                                   -rw-r--r--   2 grid supergroup 10772848292013-11-0700:13 /user/grid/test/test_randomwriter/out/part-m-00007                                                                                                                                                   -rw-r--r--   2 grid supergroup 10772897062013-11-0700:13 /user/grid/test/test_randomwriter/out/part-m-00008                                                                                                                                                   -rw-r--r--   2 grid supergroup 10772783662013-11-0700:16 /user/grid/test/test_randomwriter/out/part-m-00009

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

 

**Test Case3 word_count**

（1）Locaol上创建文件：
$mkdirinput%echo ‘hello,world’ >> input/file1.in$echo ‘hello, ruby’ >> input/file2.in

 

（2）上传到hdfs上：
./bin/hadoop fs -mkdir -p /user/grid/test/test_wordcount/./bin/hadoop fs –put input/user/grid/test/test_wordcount/in

 

（3）用yarn新计算框架运行mapreduce：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")
#command line $bin/yarn jar$YARN_HOME/share/hadoop//mapreduce/hadoop-mapreduce-examples-2.2.0.jarwordcount  /user/grid/test/test_wordcount/in/user/grid/test/test_wordcount/out                                                                                                                                             #ConSole输出摘录：3/11/0700:35:03 INFO client.RMProxy:Connecting to ResourceManager at /0.0.0.0:803213/11/0700:35:05 INFO input.FileInputFormat:Total input paths to process : 213/11/0700:35:05 INFO mapreduce.JobSubmitter:number of splits:213/11/0700:35:06 INFO mapreduce.JobSubmitter:Submitting tokens for job: job_1383806445149_000313/11/0700:35:08 INFO impl.YarnClientImpl:Submitted application application_1383806445149_0003 to ResourceManager at /0.0.0.0:803213/11/0700:35:08 INFO mapreduce.Job: The urlto track the job: http://Master:8088/proxy/application_1383806445149_0003/13/11/0700:35:08 INFO mapreduce.Job: Runningjob: job_1383806445149_000313/11/0700:35:25 INFO mapreduce.Job: Jobjob_1383806445149_0003 running in uber mode : false13/11/0700:35:25 INFO mapreduce.Job:  map 0% reduce 0%                                                                                                                                              13/11/0700:37:50 INFO mapreduce.Job:  map 33% reduce 0%                                                                                                                                              13/11/0700:37:54 INFO mapreduce.Job:  map 67% reduce 0%                                                                                                                                              13/11/0700:37:55 INFO mapreduce.Job:  map 83% reduce 0%                                                                                                                                              13/11/0700:37:58 INFO mapreduce.Job:  map 100% reduce 0%                                                                                                                                              13/11/0700:38:51 INFO mapreduce.Job:  map 100% reduce 100%                                                                                                                                              13/11/0700:38:54 INFO mapreduce.Job: Jobjob_1383806445149_0003 completed successfully13/11/0700:38:56 INFO mapreduce.Job:Counters: 43

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

 

**说明**：查看word count的计算结果：
1

2
3

4
$bin

/hadoop

fs -

cat

/user/grid/test//test_wordcount/out/

*

hadoop 1
hello  1

ruby

**补充**：因为新的YARN为了保持与MRv1框 架的旧版本兼容性，很多老的API还是可以用，但是会有INFO。此处通过修改$YARN_HOME/etc/hadoop /log4j.properties可以turn offconfiguration deprecation warnings.

建议去掉第138行的注释（可选），确保错误级别为WARN（默认为INFO级别，详见第20行：hadoop.root.logger=INFO,console）：
1138 log4j.logger.org.apache.hadoop.conf.Configuration.deprecation=WARN

附文：

**集群搭建、配置步骤（基于CentOS_64bit）**

**0. 说明**

大体规划如下：

虚拟机： VMware-workstation-full-8.0.3-703057（VMware10中文版不完整，此版本内含vmware tools，为设定共享文件夹所必须）

电脑1，VMWare,内装2个虚拟系统，(cluster1, cluster2)

电脑2,，VMware内装2个虚拟系统，(cluster3, cluster4)

虚拟主机： CentOS x86 64bit

局域网IP设置：****
1

2
3

4
    

cluster1  172.16.102. 201

cluster2   172.16.102. 202
cluster3   172.16.102. 203

cluster4   172.16.102. 204

网关

1172.16.102.254

**1. Linux集群安装**

(1) 准备

Vmware: VMware-workstation-full-8.0.3-703057(此安装包自带VMWare Tools)

Linux:CentOS.iso

(2) VMWare配置

VMWare以及所有安装的虚拟机均为桥接

Step1. 配置VMWare 联网方式： Editor->Virtual Network Editor->选择Bridged， 点击确定

Step2. 安装虚拟机

Step3.配置各虚拟机接网方式：右键已安装虚拟机->点击NetworkAdapter, 选择桥接，确定

Step4. 为所有安装好的虚拟系统设置一个共享目录(类似FTP，但是设置共享目录更方便) ：右键已安装虚拟机->点击Virtual Machine Settings对话框上部Options， 选择Shared Folder， 在本地新建SharedFolder并添加进来,确定。

(3) linux下网卡以及IP配置

以下配置在三个虚拟系统里均相同, 以cluster1为例：

配置前需切换为root

Step1. 修改主机名， 设置为开启网络

配置/etc/sysconfig/network：
1

2
3[root@localhost ~]

# cat /etc/sysconfig/network

NETWORKING=

yes
HOSTNAME=cluster1

Step2.修改当前机器的IP， 默认网关， IP分配方式， 设置网络接口为系统启动时有效：

a.查看配置前的ip:
1

2
3

4
[root@localhost ~]

# ifconfig

eth0      Link encap:Ethernet  HWaddr 00:0C:29:E1:FB:95 
inet addr:172.16.102.3  Bcast:172.16.102.255  Mask:255.255.255.0

inet6 addr: fe80::20c:29ff:fee1:fb95

/64

Scope:Lin

b.配置/etc/sysconfig/network-scripts/ifcfg-eth0

注意以下几项：

BOOTPROTO="static" (IP分配方式, 设为static则主机ip即为IPADDR里所设，若为”dhcp”, 此时只有局域网有效，则vmware会启动自带虚拟”dhcp”, 动态分配ip， 此时可以连接互联网 )

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")
IPV6INIT="no"  #你懂得 IPADDR="172.16.102.201" #ip地址 GETEWAY="172.16.102.254" #默认网关地址 ONBOOT="yes" #系统启动时网络接口有效 [root@localhost ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth0  DEVICE="eth0"BOOTPROTO="static"HWADDR="00:0C:29:E1:FB:95"BOOTPROTO="dhcp"IPV6INIT="no"IPADDR="172.16.102.201"GETEWAY="172.16.102.254"ONBOOT="yes"NM_CONTROLLED="yes"TYPE="Ethernet"UUID="79612b26-326b-472c-94af-9ab151fc2831

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

 

c.使当前设置生效：
$service network restart $ifdown eth0 #关闭默认网卡 $ifup eth#重新启用默认网卡 $service network restart; ifdown eth0; ifup eth0

 

d.查看新设置的ip:
$ifconfigeth0      Link encap:Ethernet  HWaddr 00:0C:29:E1:FB:95  inet addr:192.168.1.200  Bcast:192.168.1.255  Mask:255.255.255.0inet6 addr: fe80::20c:29ff:fee1:fb95/64 Scope:Link UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1

 

Step3. 修改hosts文件，此文件会被ＤＮＳ解析，类似linux里的alias, 设置后以后，hostname就是ip地址， 两者可以互换。

配置/etc/hosts, 添加三行如下， （注意，此3行在3个虚拟主机里都相同，切必须全部都要加上）

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")
[root@localhost ~]# cat /etc/hosts127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6 cluster1   172.16.102. 201cluster2   172.16.102. 202cluster3   172.16.102. 203cluster4   172.16.102. 204

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

 

Step4. 按照以上3个步骤分别设置两外两个虚拟主机，

配置完以后必须按照之前的三个命令分别重启 network, ifeth0:
$service network restart $ifdown eth0 $ifup eth0

 

重新初始化以后查看各自主机的ip配置是否正确。

在任意一台主机上执行： ping cluster1; ping cluster2; ping cluster3; ping cluster4

若配置正确，一定可以ping通，否则，请检查各个主机的/etc/hosts文件是否已经添加新的映射！至此，linux集群已成功配置。

**2. 设置 ssh免登陆**

a. 新建一个用户

在三台主机上分别以root权限新建一个用户，此处默认为grid:

cluster1,cluster2, cluster3, cluster4上：
$useradd –m grid $passwd grid 1qaz!QAZ

 

注意一定要保证4台主机上有相同的用户名， 以实现同一个用户在4台主机上登录。

b. 在cluster1上生成RSA密钥

切换回user: grid
$su grid $cd ~

生成密钥对：

1

2
3$

ssh

-keygen –t rsa

#一路回车到最后（ 此处生成无需密码的秘钥-公钥对）。
#上一个步骤 ssh-keygen –t rsa会在grid的home目录下生成一个.ssh文件夹

之后：

$cd ~/.ssh/$cp id_rsa.pub authorized_keys

 

c. 在另外3个主机上的grid用户home目录下也声称相应的密钥对, 并在.ssh目录下生成一个

authorized_keys
文件

d. 想办法将4台主机grid用户下刚生成的

authorized_keys
里的内容整合成一个完整的

authorized_keys.

   
比如将4个authorized_keys里的内容全部复制到cluster1上的authorized_keys里， 然后：
$chmod 600 authorized_keys $scp .ssh/authorized_keys  cluster2:/home/grid/.ssh/$scp .ssh/authorized_keys  cluster3:/home/grid/.ssh/$scp .ssh/authorized_keys  cluster4:/home/grid/.ssh/

若要求输入密码，则输入之前新建用户grid时设置的密码， 一路回车到最后.

e. 下面尝试ssh无秘钥登录：

在cluster1主机上：ssh cluster2， 依次尝试登陆cluster2, cluster3, cluster4

若均可可以免密码登录，则直接跳到下一步骤，否则，请看下面的解决方案：

可能出现的问题有3，

第一种可能，.ssh文件夹非自动生成，而是你手动新建的，若如此，会出现.ssh的安全上下文问题， 解决方案， 在三个主机上以grid用户，删除刚才生成的.ssh文件夹，重新进入home目录，务必用 # ssh-keygen –t rsa 生成秘钥， 此过程ssh程序会自动在home目录下成成一个.ssh文件夹

第二种可能, authorized_keys权限不一致。 在各自.ssh目录下：ls–alauthorizedkeys,查看此文件的权限，一定为600(−rw−−−−−−−),即只对当前用户grid开放可读可写权限，若不是，则修改authorizedkeys文件权限chmod 600 authorized_keys

若经过以上两步还不行，则执行以下命令，重新启动ssh-agent， 且加入当前用户秘钥id_rsa
$ssh-add ~grid/.ssh/id_rsa

 

经过以上三步，一定可以实现grid从cluster1到其它3个节点的免秘钥登录。

因为hadoop2.2新架构的缘故，我们还应该设置为每一个节点到其它任意节点免登陆。

具体步骤:

1.在将cluster2, cluster3, cluster4上各自的.ssh/id_rsa.pub 分别复制到cluster1的.ssh/下： scp id_rsa.pub cluster1:/home/grid/.ssh/tmp2, scp id_rsa.pub cluster1:/home/grid/.ssh/tmp3 ...

2.将cluster1节点/home/grid/.ssh/下的tmp2, tmp3, tmp4分别appand到authorized_keys里， 并请将各自节点上的id_rsa.pub也append到各自节点的authorized_keys里，以实现本地登陆（类似： ssh localhost)

至此，ssh免秘钥登陆设置完成。

**3. 安装jdk**

Step1. 记得之前在安装cluster1时在VMWare里设置的共享目录吗？

在Windows下将Hadoop安装包，jdk安装包Copy到共享目录下，然后在linux下从/mnt/hgfs/Data-shared/下cp到/home/grid/下，直接执行jdk安装包，不需要解压。

注意：若在安装过程中提示”Extracting… install.sfx 5414: /lib/ld-linux.50.2 No such file or directory, 则执行以下命令:

a.我们之前为了测试自定义的ip是否有效，已将各台主机的ip分配方式设为了BOOTPROTO=”static”, 这种方式是无法连入外部网络的，所以此时为了安装缺省的包，切换到root, 修改/etc/sysconfig/network-script/ifcfg-eth0，将BOOTPROTO=”static” 修改为BOOTPROTO=”dhcp”,

b.重启网络服务和网卡: 在root权限下:
$service network restart; ifdown eth0; ifup eth0 $yum install glibc.i686

d.切换回grid,重新安装jdk

$cd /usr/java/  #注意必须进入java文件夹，因为java安装包默认安装在当前目录下 $./jdk-6u25-linux-i586.bin #安装jdk

安装完以后，记得将eth0文件修改回来：

$sed -i s/dhcp/static/g /etc/sysconfig/network-scripts/ifcfg-eth0 #此处用sed直接替换，若不放心也可以用编辑器修改 #service network restart; ifdown eth0;ifup eth0

至此，jdk已在linux下安装完毕。

最后，将java安装好的路径append到$PATH变量里（处于个人习惯，新环境变量一律添加到所需用户的.bashrc文件里， 即只对当前用户有效）
$su grid $vim ~/.bashrc （修改.bashrc文件，添加如下两行：） $tail -2 ~/.bashrc export JAVA_HOME="/usr/java/jdk1.6.0_25/"export PATH="${PATH}:${JAVA_HOME}/bin"

测试一下java是否可以正常启动：

$source ~/.bashrc $which java $/usr/java/jdk1.6.0_25/bin/java

至此，jdk安装完毕。

用同样的方式在另外3台虚拟主机上安装jdk, 提示：先复制到home下
$scp -r /mnt/hgfs/Data-shared/Archive/jdk-6u25-linux-i586.bin  cluster2:/home/grid/$scp -r /mnt/hgfs/Data-shared/Archive/jdk-6u25-linux-i586.bin  cluster3:/home/grid/$scp -r /mnt/hgfs/Data-shared/Archive/jdk-6u25-linux-i586.bin  cluster4:/home/grid/

再切root, copy到/usr/java下， cd /usr/java, 然后再安装.
