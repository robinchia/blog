---
layout: post
title: "CentOS的Hadoop集群配置"
categories: hadoop
tags: 
 - hadoop
--- 

# CentOS的Hadoop集群配置

 

### [CentOS的Hadoop集群配置（一）](http://blog.csdn.net/inte_sleeper/article/details/6569985)

### 
参考资料：

[http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/](http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/)

[http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-multi-node-cluster/](http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-multi-node-cluster/)

[http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/)

[http://hadoop.apache.org/common/docs/current/cluster_setup.html](http://hadoop.apache.org/common/docs/current/cluster_setup.html)

 

以下集群配置内容，以两台机器为例。其中一台是 master ，另一台是 slave1 。

master 上运行 name node, data node, task tracker, job tracker ， secondary name node ；

slave1 上运行 data node, task tracker 。

 

前面加 * 表示对两台机器采取相同的操作

1.       安装 JDK *

yum install java-1.6.0-openjdk-devel

 

2.       设置环境变量 *

编辑 /etc/profile 文件，设置 JAVA_HOME 环境变量以及类路径：

export JAVA_HOME="/usr/lib/jvm/java-1.6.0-openjdk-1.6.0.0.x86_64"

export PATH=$PATH:$JAVA_HOME/bin

export CLASSPATH=.:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar

 

3.       添加 hosts 的映射 *

编辑 /etc/hosts 文件，**注意** **host name ****不要有下划线，见下步骤 9**

192.168.225.16 master

192.168.225.66 slave1

 

4.       配置 SSH *

cd /root & mkdir .ssh

chmod 700 .ssh & cd .ssh

 

创建密码为空的 RSA 密钥对：

ssh-keygen -t rsa -P ""

在提示的对称密钥名称中输入 id_rsa

将公钥添加至 authorized_keys 中：

cat id_rsa.pub >> authorized_keys

chmod 644 authorized_keys **#** **重要**

 

编辑 sshd 配置文件 /etc/ssh/sshd_config ，把 #AuthorizedKeysFile  .ssh/authorized_keys 前面的注释取消掉。

重启 sshd 服务：

service sshd restart

 

测试 SSH 连接。连接时会提示是否连接，按回车后会将此公钥加入至 knows_hosts 中：

ssh localhost

 

5.       配置 master 和 slave1 的 ssh 互通

在 slave1 中重复步骤 4 ，然后把 slave1 中的 .ssh/authorized_keys 复制至 master 的 .ssh/authorized_keys中。注意复制过去之后，要看最后的类似 root@localhost 的字符串，修改成 root@slave1 。同样将 master的 key 也复制至 slave1 ，并将最后的串修改成 root@master 。

 

或者使用如下命令：

ssh-copy-id -i ~/.ssh/id_rsa.pub root@slave1

 

测试 SSH 连接：

在 master 上运行：

ssh slave1

在 slave1 上运行：

ssh master

 

6.      安装 Hadoop

下载 hadoop 安装包：

wget [http://mirror.bjtu.edu.cn/apache/hadoop/common/hadoop-0.20.203.0/hadoop-0.20.203.0rc1.tar.gz](http://mirror.bjtu.edu.cn/apache/hadoop/common/hadoop-0.20.203.0/hadoop-0.20.203.0rc1.tar.gz)

 

复制安装包至 slave1 ：

scp hadoop-0.20.203.0rc1.tar.gz root@slave1:/root/

 

解压：

tar xzvf hadoop-0.20.203.0rc1.tar.gz

mkdir /usr/local/hadoop

mv hadoop-0.20.203.0/* /usr/local/hadoop

 

         修改 .bashrc 文件（位于用户目录下，即 ~/.bashrc ，对于 root ，即为 /root/.bashrc ）

         添加环境变量：

         export HADOOP_HOME=/usr/local/hadoop

    export PATH=$PATH:$HADOOP_HOME/bin

        

 

7.      配置 Hadoop 环境变量 *

**以下所有 hadoop ****目录下的文件，均以相对路径 hadoop ****开始**

修改 hadoop/conf/hadoop-env.sh 文件，将里面的 JAVA_HOME 改成步骤 2 中设置的值。

 

8.       创建 Hadoop 本地临时文件夹 *

mkdir /root/hadoop_tmp （**注意这一步，千万不要放在** **/tmp ****目录下面！！因为 ****/tmp ****默认分配的空间是很小的，往 ****hdfs ****里放几个大文件就会导致空间满了，就会报错）**

修改权限：

chown -R hadoop:hadoop /root/hadoop_tmp

更松地，也可以这样：

chmod –R 777 /root/hadoop_tmp

 

 

9.      配置 Hadoop

修改 master 的 hadoop/conf/core-site.xml ，在 <configuration> 节中添加如下内容：

注意： **fs.default.name ****的值不能带下划线**

<property>

    <name>hadoop.tmp.dir</name>

    <value>/root/hadoop_tmp/hadoop_${user.name}</value>

</property>

<property>

    <name>fs.default.name</name>

    <value>hdfs://localhost:54310</value> 

</property>

<property>

    <name>io.sort.mb</name>

    <value>1024</value> 

</property>

         其中 io.sort.mb 值，指定了排序使用的内存，大的内存可以加快 job 的处理速度。

 

         修改 hadoop/conf/mapred-site.xml ，在 <configuration> 节中添加如下内容：

<property>

    <name>mapred.job.tracker</name>

    <value>localhost:54311</value>

</property>

<property>

    <name>mapred.map.child.java.opts</name>

    <value>-Xmx4096m</value>

</property>

<property>

    <name>mapred.reduce.child.java.opts</name>

    <value>-Xmx4096m</value>

</property>

         其中 mapred.map.child.java.opts, mapred.reduce.child.java.opts 分别指定 map/reduce 任务使用的最大堆内存。较小的内存可能导致程序抛出 OutOfMemoryException 。

 

修改 conf/hdfs -site.xml ，在 <configuration> 节中添加如下内容：

<property>

    <name>dfs.replication</name>

    <value>2</value>

</property>

 

同样，修改 slave1 的 /usr/local/hadoop/conf/core-site.xml ，在 <configuration> 节中添加如下内容：

<property>

    <name>hadoop.tmp.dir</name>

    <value>/root/hadoop_tmp/hadoop_${user.name}</value>

</property>

<property>

    <name>fs.default.name</name>

    <value>hdfs://localhost:54310</value> 

</property>

<property>

    <name>io.sort.mb</name>

    <value>1024</value> 

</property>

 

         修改 conf/mapred-site.xml ，在 <configuration> 节中添加如下内容：

<property>

    <name>mapred.job.tracker</name>

    <value>localhost:54311</value>

</property>

<property>

    <name>mapred.map.child.java.opts</name>

    <value>-Xmx4096m</value>

</property>

<property>

    <name>mapred.reduce.child.java.opts</name>

    <value>-Xmx4096m</value>

</property>

 

         修改 conf/hdfs -site.xml ，在 <configuration> 节中添加如下内容：

<property>

    <name>dfs.replication</name>

    <value>2</value>

    </property>

 

10.   修改 hadoop/bin/hadoop 文件

把 221 行修改成如下。因为对于 root 用户， -jvm 参数是有问题的，所以需要加一个判断 ( 或者以非 root 用户运行这个脚本也没问题 )

HADOOP_OPTS="$HADOOP_OPTS -jvm server $HADOOP_DATANODE_OPTS"  à

    #for root, -jvm option is invalid.

    CUR_USER=`whoami`

    if [ "$CUR_USER" = "root" ]; then

        HADOOP_OPTS="$HADOOP_OPTS -server $HADOOP_DATANODE_OPTS"

    else

        HADOOP_OPTS="$HADOOP_OPTS -jvm server $HADOOP_DATANODE_OPTS"

    fi 

unset $CUR_USER

 

至此， master 和 slave1 都已经完成了 single_node 的搭建，可以分别在两台机器上测试单节点。

启动节点：

hadoop/bin/start-all.sh

运行 jps 命令，应能看到类似如下的输出：

937 DataNode

9232 Jps

8811 NameNode

12033 JobTracker

12041 TaskTracker
来源： <[http://blog.csdn.net/inte_sleeper/article/details/6569985](http://blog.csdn.net/inte_sleeper/article/details/6569985)> 

[CentOS的Hadoop集群配置（二）](http://blog.csdn.net/inte_sleeper/article/details/6569990)
下面的教程把它们合并至 multi-node cluster 。

 

1.     合并 single-node 至 multi-node cluster

修改 master 的 hadoop/conf/core-site.xml ：

<property>

    <name>hadoop.tmp.dir</name>

    <value>/root/hadoop_tmp/hadoop_${user.name}</value>

</property>

<property>

    <name>fs.default.name</name>

    <value>hdfs://**master** :54310</value>

</property>

<property>

    <name>io.sort.mb</name>

    <value>1024</value> 

</property>

 

         修改 conf/mapred-site.xml ，在 <configuration> 节中添加如下内容：

<property>

    <name>mapred.job.tracker</name>

    <value>**master** :54311</value>

</property>

<property>

    <name>mapred.map.child.java.opts</name>

    <value>-Xmx4096m</value>

</property>

<property>

    <name>mapred.reduce.child.java.opts</name>

    <value>-Xmx4096m</value>

</property>

         修改 conf/hdfs -site.xml ，在 <configuration> 节中添加如下内容：

<property>

    <name>dfs.replication</name>

    <value>2</value>

</property>

把这三个文件复制至 slave1 相应的目录 hadoop/conf 中 **( ****即 master ****和 slave1 ****的内容完全一致 )**

 

         修改所有节点的 hadoop/conf/masters ，把文件内容改成： master

         修改所有节点的 hadoop/conf/slaves ，把文件内容改成：

master

    slave1

 

         分别删除 master 和 slave1 的 dfs/data 文件：

         rm –rf /root/hadoop_tmp/hadoop_root/dfs/data

        

重新格式化 namenode ：

         hadoop/bin/hadoop namenode -format

 

         测试，在 master 上运行：

         hadoop/bin/start-all.sh

         在 master 上运行 jps 命令

        

此时输出应类似于：

         11648 TaskTracker

11166 NameNode

11433 SecondaryNameNode

12552 Jps

11282 DataNode

11525 JobTracker

 

在 slave1 上运行 jps

此时输出应包含 ( 即至少有 DataNode, 否则即为出错 ) ：

3950 Jps

3121 TaskTracker

3044 DataNode

 

2.      测试一个 JOB

首先升级 python( 可选，如果 JOB 是 python 写的 ) ：

cd /etc/yum.repos.d/

wget http://mirrors.geekymedia.com/centos/geekymedia.repo

yum makecache

yum -y install python26

**升级 python ****的教程，见另外一篇文档。如果已经通过以上方法安装了 python2.6 ****，那需要先卸载：**

yum remove python26 python26-devel

 

         CentOS 的 yum 依赖于 python2.4 ，而 /usr/bin 中 python 程序即为 python2.4 。我们需要把它修改成python2.6 。

 

         cd /usr/bin/

         编辑 yum 文件，把第一行的

#!/usr/bin/python   à   #!/usr/bin/python2.4  

保存文件。

         删除旧版本的 python 可执行文件（这个文件跟该目录下 python2.4 其实是一样的，所以可以直接删除）

         rm -f python

         让 python 指向 python2.6 的可执行程序。

         ln -s python26 python  

 

3.      Word count python 版本

**Map.py**

 

#! /usr/bin/python

import sys;

 

for line in sys.stdin:

  line =  line.strip();

  words = line.split();

  for word in words:

      print '%s/t%s' % (word,1);

 

**Reduce.py**

#!/usr/bin/python

import sys;

 

wc = {};

 

for line in sys.stdin:

  line = line.strip();

  word,count = line.split('/t',1);

  try:

      count = int(count);

  except Error:

      pass;

  if wc.has_key(word):

      wc[word] += count;

  else: wc[word] = count;

 

for key in wc.keys():

  print '%s/t%s' % (key, wc[key]);

 

本机测试：

echo "foo foo bar bar foo abc" | map.py

echo "foo foo bar bar foo abc" | map.py | sort | reduce.py

 

在 hadoop 中测试：

hadoop jar /usr/local/hadoop/contrib/streaming/hadoop-streaming-0.20.203.0.jar -file mapper.py -mapper mapper.py -file reducer.py -reducer reducer.py -input wc/* -output wc-out

Job 成功后，会在 HDFS 中生成 wc-out 目录。

查看结果：

hadoop fs –ls wc-out

hadoop fs –cat wc-out/part-00000

 

 

4.      集群增加新节点

a.       执行步骤 1 ， 2.

b.       修改 hosts 文件，将集群中的 hosts 加入本身 /etc/hosts 中。并修改集群中其他节点的 hosts ，将新节点加入。

c.       master 的 conf/slaves 文件中，添加新节点。

d.       启动 datanode 和 task tracker 。

hadoop-daemon.sh start datanode

hadoop-daemon.sh start tasktracker

 

5.      Trouble-shooting

hadoop 的日志在 hadoop/logs 中。

其中， logs 根目录包含的是 namenode, datanode, jobtracker, tasktracker 等的日志。分别以 hadoop-{username}-namenode/datanode/jobtracker/tasktracker-hostname.log 命名。

userlogs 目录里包含了具体的 job 日志，每个 job 有一个单独的目录，以 job_YYYYmmddHHmm_xxxx 命名。里面包含数个 attempt_{jobname}_m_xxxxx 或 attempt_{jobname}_r_xxxx 等数个目录。其中目录名中的m 表示 map 任务的日志， r 表示 reduce 任务的日志。因此，出错时，可以有针对性地查看特定的日志。

 

常见错误：

1.       出现类似：

*ERROR org.apache.hadoop.hdfs.server.datanode.DataNode: java.io.IOException: Incompatible namespaceIDs …*

的异常，是因为先格式化了 namenode ，后来又修改了配置导致。将 dfs/data 文件夹内容删除，再重新格式化 namenode 即可。

2.       出现类似：

*INFO org.apache.hadoop.ipc.Client: Retrying connect to server:…*

的异常，首先确认 name node 是否启动。如果已经启动，有可能是 master 或 slave1 中的配置出错，集群配置参考步骤 11 。也有可能是防火墙问题，需添加以下例外：

50010 端口用于数据传输， 50020 用于 RPC 调用， 50030 是 WEB 版的 JOB 状态监控， 54311 是job tracker ， 54310 是与 master 通信的端口。

完整的端口列表见：

[http://www.cloudera.com/blog/2009/08/hadoop-default-ports-quick-reference/](http://www.cloudera.com/blog/2009/08/hadoop-default-ports-quick-reference/)

 

iptables -A RH-Firewall-1-INPUT -p tcp -m tcp --dport 50010 -j ACCEPT

iptables -A RH-Firewall-1-INPUT -p tcp -m tcp --dport 50020 -j ACCEPT

iptables -A RH-Firewall-1-INPUT -p tcp -m tcp --dport 50030 -j ACCEPT

iptables -A RH-Firewall-1-INPUT -p tcp -m tcp --dport 50060 -j ACCEPT

iptables -A RH-Firewall-1-INPUT -p tcp -m tcp --dport 54310 -j ACCEPT

iptables -A RH-Firewall-1-INPUT -p tcp -m tcp --dport 54311 -j ACCEPT

 

iptables -A OUTPUT -p tcp -m tcp --dport 50010 -j ACCEPT

iptables -A OUTPUT -p tcp -m tcp --dport 50020 -j ACCEPT

iptables -A OUTPUT -p tcp -m tcp --sport 50010 -j ACCEPT

iptables -A OUTPUT -p tcp -m tcp --sport 50020 -j ACCEPT

iptables -A OUTPUT -p tcp -m tcp --dport 50030 -j ACCEPT

iptables -A OUTPUT -p tcp -m tcp --sport 50030 -j ACCEPT

iptables -A OUTPUT -p tcp -m tcp --dport 50060 -j ACCEPT

iptables -A OUTPUT -p tcp -m tcp --sport 50060 -j ACCEPT

iptables -A OUTPUT -p tcp -m tcp --dport 54310 -j ACCEPT

iptables -A OUTPUT -p tcp -m tcp --sport 54310 -j ACCEPT

iptables -A OUTPUT -p tcp -m tcp --dport 54311 -j ACCEPT

iptables -A OUTPUT -p tcp -m tcp --sport 54311 -j ACCEPT

 

保存规则：

/etc/init.d/iptables save

重启 iptables 服务：

service iptables restart

如果还是出现问题 2 的错误，那可能需要手工修改 /etc/sysconfig/iptables 的规则。手动添加这些规则。若有 ”reject-with icmp-host-prohibited” 的规则，需将规则加到它的前面。注意修改配置文件的时候，不需要带 iptables 命令。直接为类似于：

-A OUTPUT -p tcp -m tcp --sport 54311 -j ACCEPT

 

或关闭防火墙 **( ****建议，因为端口太多，要加的例外很多 )**

service iptables stop

 

3.       在 /etc/hosts 文件中，确保一个 host 只对应一个 IP ，否则会出错（如同时将 slave1 指向 127.0.0.1 和192.168.225.66 ），**可能导致数据无法从一个节点复制至另一节点。**

4.       出现类似：

*FATAL org.apache.hadoop.mapred.TaskTracker: Error running child : java.lang.OutOfMemoryError: Java heap space…*

的异常，是因为堆内存不够。有以下几个地方可以考虑配置：

a.       conf/hadoop-env.sh 中， export HADOOP_HEAPSIZE=1000 这一行，默认为注释掉，堆大小为1000M ，可以取消注释，将这个值调大一些（对于 16G 的内存，可以调至 8G ）。

b.       conf/mapred-site.xml 中，添加 mapred.map.child.java.opts 属性，手动指定 JAVA 堆的参数值为 -Xmx2048m 或更大。这个值调整 map 任务的堆大小。即：

<property>

    <name>mapred.map.child.java.opts </name>

    <value>-Xmx2048m</value>

</property>

c.       conf/mapred-site.xml 中，添加 mapred.reduce.child.java.opts 属性，手动指定 JAVA 堆的参数值为 -Xmx2048m 或更大。这个值调整 reduce 任务的堆大小。即：

<property>

    <name>mapred.reduce.child.java.opts </name>

    <value>-Xmx2048m</value>

</property>

                   注意调整这些值之后，要重启 name node 。

 

*5.       *出现类似： *java.io.IOException: File /user/root/pv_product_110124 could only be replicated to 0 nodes, instead of 1…*

的异常，首先确保 hadoop 临时文件夹中有足够的空间，空间不够会导致这个错误。

如果空间没问题，那就尝试把临时文件夹中 dfs/data 目录删除，然后重新格式化 name node ：

hadoop namenode -format

注意：此命令会删除 hdfs 上的文件

 

*6.       *出现类似： *java.io.IOException: Broken pipe…*

的异常，检查你的程序吧，没准输出了不该输出的信息，如调试信息等。
来源： <[http://blog.csdn.net/inte_sleeper/article/details/6569990](http://blog.csdn.net/inte_sleeper/article/details/6569990)> 
