---
layout: post
title: "Hadoop知识分享文稿 ( by quqi99 ) - 技术并艺术着"
categories: hadoop
tags: 
 - hadoop
--- 

# Hadoop知识分享文稿 ( by quqi99 ) - 技术并艺术着

您还未登录！|[登录](https://passport.csdn.net/account/login)|[注册](https://passport.csdn.net/account/register)|[帮助](https://passport.csdn.net/help/faq)

* [首页](http://www.csdn.net/)
* [业界](http://news.csdn.net/)
* [移动](http://mobile.csdn.net/)
* [云计算](http://cloud.csdn.net/)
* [研发](http://sd.csdn.net/)
* [论坛](http://bbs.csdn.net/)
* [博客](http://blog.csdn.net/)
* [下载](http://download.csdn.net/)
* 
## [更多]()

# [技术并艺术着](http://blog.csdn.net/quqi99)

## 张华的技术Blog

* [![]()目录视图](http://blog.csdn.net/quqi99?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/quqi99?viewmode=list)
* [![]()订阅](http://blog.csdn.net/quqi99/rss/list)
[公告：博客新增直接引用代码功能](https://code.csdn.net/blog/12)        [专访李铁军：从医生到金山首席安全专家的转变](http://www.csdn.net/article/2013-08-06/2816471)      [CSDN博客频道自定义域名、标签搜索功能上线啦！](http://blog.csdn.net/csdnproduct/article/details/9495139)      [独一无二的职位：开源社区经理](http://blog.csdn.net/adali/article/details/9813651)

### [[置顶] Hadoop知识分享文稿 ( by quqi99 )]()

分类： [Seach Engine](http://blog.csdn.net/quqi99/article/category/328029)  2011-03-31 15:19 1977人阅读 [评论]()(0) [收藏]( "收藏") [举报]( "举报")
[hadoop](http://blog.csdn.net/tag/details.html?tag=hadoop)[任务](http://blog.csdn.net/tag/details.html?tag=%e4%bb%bb%e5%8a%a1)[mapreduce](http://blog.csdn.net/tag/details.html?tag=mapreduce)[任务调度](http://blog.csdn.net/tag/details.html?tag=%e4%bb%bb%e5%8a%a1%e8%b0%83%e5%ba%a6)[集群](http://blog.csdn.net/tag/details.html?tag=%e9%9b%86%e7%be%a4)[作业](http://blog.csdn.net/tag/details.html?tag=%e4%bd%9c%e4%b8%9a)

目录[(?)]( "系统根据文章中H1到H6标签自动生成文章目录")[[+]]( "展开")

1. [作者张华 写于2010-08-15   发表于2011-03-31 版权声明可以任意转载转载时请务必以超链接形式标明文章原始出处和作者信息及本版权声明]()
1. [httpblogcsdnnetquqi99]()
1. [hadoop 理论基础]()

1. [hadoop 是什么]()
1. [hadoop 项目]()
1. [MapReduce 任务的运行流程]()
1. [MapReduce 任务的数据流图]()

1. [hadoop 入门实战]()

1. [测试环境]()
1. [测试程序]()
1. [属性配置]()
1. [免密码 SSH 设置]()
1. [配置 hosts]()
1. [格式化 HDFS 文件系统]()
1. [启动守护进程]()
1. [运行程序]()

1. [hadoop 高级进阶]()
1. [hadoop 应用案例]()
1. [参考文献]()
                              **Hadoop知识分享文稿 ( by quqi99 )**

 

 

## []()作者：张华 写于：2010-08-15   发表于：2011-03-31
版权声明：可以任意转载，转载时请务必以超链接形式标明文章原始出处和作者信息及本版权声明

## []()( http://blog.csdn.net/quqi99 )

 
**内容目录**

目 录

1 hadoop 理论基础 3

1.1 hadoop 是什么 3

1.2 hadoop 项目 3

1.3 Map/Reduce 任务的运行流程 4

1.4 Map/Reduce 任务的数据流图 5

2 hadoop 入门实战 7

2.1  测试环境 7

2.2  测试程序 7

2.3  属性配置 9

2.4  免密码SSH 设置 10

2.5  配置hosts 11

2.6  格式化HDFS 文件系统 11

2.7  启动守护进程 11

2.8  运行程序 11

3 hadoop 高级进阶 12

4 hadoop 应用案例 12

5  参考文献 12

 

 

 

# []()1 hadoop  理论基础

## []()1.1 hadoop  是什么

Hadoop  是 Doug Cutting  开发的，他是一个相当牛的哥们，他同时是大名鼎鼎的 Lucene  及 Nutch  的作者。

我是这样理解 hadoop  的，它就是用来对海量数据进行存储与分析的一个开源软件。它包括两块：

1  ） HDFS ( Hadoop Distrubuted File System )  ，可以对重要数据进行冗余存储，有点类似于冗余磁盘陈列。

2  ）对 Map/Reduce  编程模型的一个实现。当然，关系型数据库（ RDBMS  ）也能做类似的事情，但为什么不用 RDBMS  呢？我们知道，让计算移动于数据上比让数据移动到计算更有效率。这使得 Map/Reduce  适合数据被一次写入和多次读取的应用，而 RDBMS  更适合持续更新的数据集。

## []()1.2 hadoop  项目

如今，广义上的 Hadoop  已经发展成为一个分布式计算基础架构这把“大伞”下相关子项目的集合，其技术栈如下图所示：

 

图：

                                         ![]()

![](http://blog.csdn.net/root/Library/Caches/TemporaryItems/moz-screenshot-4.png)

 

                                                    图1 hadoop 的子项目

* 
Core ： 一系列分布式文件系统和通用I/O 的组件和接口( 序列化、Java RPC 和持久化数据结构) 。
* 
Avro ： 用于数据的序列化，当然，JDK 中也有Seriable 接口，但hadoop 中有它自己的序列化方式，具说更有效率。
* 
MapReduce ： 分布式数据处理模式和执行环境，运行于大型商用机集群。
* 
HDFS ： 分布式文件系统，运行于大型商用机集群。
* 
Pig ： HDFS 上的数据检索语言，类似于RDBMS 中的SQL 语言。
* 
Hbase ： 一个分布式的、列存储数据库。HBase 使用HDFS 作为底层存储，同时支持MapReduce 的批量式计算和点查询( 随机读取) 。
* 
ZooKeeper ： 一个分布式的、高可用性的协调服务。ZooKeeper 提供分布式锁之类的基本服务用于构建分布式应用。
* 
Hive ： 分布式数据仓库。Hive 管理HDFS 中存储的数据，并提供基于SQL 的查询语言( 由运行时引擎翻译成MapReduce 作业) 用以查询数据。
* 
Chukwa ： 分布式数据收集和分析系统。Chukwa 运行HDFS 中存储数据的收集器，它使用MapReduce 来生成报告。

## []() 1.3 Map/Reduce  任务的运行流程

                     ![]()

JobClient  的  submitJob()  方法的作业提交过程如下：

1  ）向 Jobtraker  请求一个新作业 ID

2  ） 调用 JobTracker  的 getNewJobId()

3  ）  JobClient  进行作业划分，并将划分后的输入及作业的 JAR  文件、配置文件等复制到 HDFS  中去

4  ） 提交作业，会把此调用放入到一个内部的队列中，交由作业调度器进行调度。值得一提的是，针对  Map  任务与 Reduce  任务，任务调度器是优先选择 Map  任务的，另外，任务调度器在选择 Reduce  任务时并没有考虑数据的本地化。然而，针对一个 Map  任务，它考虑的是 Tasktracker  网络位置和选取一个距离其输入划分文件最近的 Tasktracker  ，它可能是数据本地化的，也可能是机架本地化的，还可能得到不同的机架上取数据。

5  ） 初始化包括创建一个代表该正在运行的作业的对象，它封装任务和记录信息，以便跟踪任务的状态和进度。

6  ） JobTracker  任务调度器首先从共享文件系统中获取 JobClient  已计算好的输入划分信息，然后为每个划分创建一个 Map  任务。创建 的 reduce  任务的数量是由 JobConf  的 Mapred.reduce.tasks  属性决定，它是用 setNumReduceTask()  方法来设置的。

7  ） TaskTracker  执行一个简单的循环，定期发送心跳（ Heartbeat  ）方法调用 Jobtracker  告诉是否还活着，同时，心跳还会报告任务运行的是否已经准备运行新的任务。

8  ） TaskTracker  已经被分配了任务，下一步是运行任务。首先它需要将它所需的全部文件从 HDFS  中复制到本地磁盘。

9  ）紧接着，它要启动一个新的 Java  虚拟机来运行每个任务，这使得用户所定义的 Map  和 Reduce  函数的任务缺陷都不会影响 TaskTracker  （比如导致它崩溃或者挂起）

10  ）运行 Map  任务或者 Reduce  任务，值得一提的是，这些任务使用标准输入与输出流，换句话说，你可以用任务语言（如 JAVA  ， C++  ， Shell  等）来实现 Map  和 Reduce  ，只要保证它们也使用标准输入与输出流，就可以将输出的键值对传回给 JAVA  进程了。

## []() 1.4 Map/Reduce  任务的数据流图

![]()

 

 

        图3  Map/Reduce  中单一 Reduce  任务的数据流图

![]()

 

 

                 图4  Map/Reduce  中多个 Reduce  任务的数据流图

![]()

 

                图5  MapReduce  中没有 Reduce  任务的数据流图

 

**任务粒度**   ： 分片的个数，在将原始大数据切割成小数据集时，通常让小数据集小于或等于 HDFS  中的一个 Block  的大小（缺省是 64M)  ，这样能够保证一个小数据集位于一台计算机上，便于本地计算。 有 M   个 小数据集 待处理，就启动 M   个 Map   任务，注意这 M   个 Map   任务分布于 N   台计算机上并行运行，Reduce   任务的数量 R   则可由用户指定 。

**Map**   ： 输入 <k1, v1>   输出 List(<k2,v2>)

**Reduce**   ： 输入 <k2,List(v2)>   输出 <k3,v3>

**分区（**  **Partition)**  :   把 Map   任务输出的中间结果按 key   的范围划分成 R   份 ( R  是预先定义的 Reduce  任务的个数) ，划分时通常使用 hash  函数如: hash(key) mod R ，这样可以保证某一段范围内的 key ，一定是由一个 Reduce  任务来处理，可以简化 Reduce  的过程。

**Combine**   :   在  partition   之前，还可以对中间结果先做  combine  ，即将中间结果中有相同  key  的  <key, value>   对合并成一对。 combine   的过程与  Reduce   的过程类似，很多情况下就可以直接使用  Reduce   函数，但  combine   是作为  Map   任务的一部分，在执行完  Map   函数后紧接着执行的。 Combine   能够减少中间结果中  <key, value>   对的数目，从而减少网络流量。

下面举个例子来着重说明 Combine  ， hadoop  允许用户声明一个 combiner  运行在 Map  的输出上，它的输出再作为 Reduce  的输入。例如，找出每一年的最调气温：

假如用户的输入的分片数是 2  ，那么：

1  ）第一个 Map  的输出如下：

（ 1950  ， 0  ）

（ 1950  ， 20  ）

（ 1950  ， 10  ）

2  ） 第二个 Map  的输出如下：

（ 1950  ， 25  ）

（ 1950  ， 15  ）

3  ） Reduce  的输入如下：

（ 1950  ，［ 0  ， 20  ， 10  ， 25  ， 15  ］）

**注意：如果有**   **combine**    **的话，此时**   **Reduce**    **的输入应该是：**

**max(0, 20, 10, 25, 15) = max(max(0,20,10), max(25,15)) = max(20,25)**

**combine**    **并不能取代**   **reduce,**    **例如，如果我们计算平均气温，便不能使用**   **combine**    **，因为：**

**mean(0,20,10,25,15) = 14**

**但是：**

**mean(mean(0,20,10), mean(25,15)) = mean(10,20) = 15**

4  ） Reduce  的输出如下：

（ 1950  ， 25  ）

# []()2 hadoop  入门实战

hadoop  有三种部署模式：

* 
单机模式：没有守护进程，一切都运行在单个 JVM  上，适合测试与调试。
* 
伪集群模式：守护进程在本地运行，适合模拟集群。
* 
集群模式：守护进程运行在集群的某台机器上。

所以，在以上任一特定模式运行 hadoop  时，只需要做两件事情：

1  ） 设置适当属性

2  ）启动 hadoop  的守护进程（名称节点，二级名称节名，数据节点）

hadoop  默认的是单机模式，下面，我们将着重介绍在集群模式是如何部署？

## []()2.1   测试环境

用两台机器做为测试环境 ,   通常，集群里的一台机器被指定为  NameNode  ，另一台不同的机器被指定为 JobTracker  ，这些机器是 **masters;**  余下的机器即作为 DataNode  **也** 作为 TaskTracker  ，这些机器是 **slaves**  **。**

1  ）  master (JobTracker & NameNode)  ：我的工作机  ( zhanghua  .quqi.com)

2  ）  slave (TaskTracker & DataNode)  ：我的开发机 ( tadev03  .quqi.com)

3)   两机均已安装 ssh   与  rsync

## []()2.2   测试程序

1  ）  /home/workspace/hadoopExample/input/file01:

Hello World Bye World

2) /home/workspace/hadoopExample/input/file02:

Hello  Hadoop    Goodbye  Hadoop

1. 
WordCount.java

 

**package**    com.TripResearch.hadoop;

 

**import**   java.io.IOException;

**import**   java.util.Iterator;

**import**   java.util.StringTokenizer;

 

**import**   org.apache.hadoop.fs.Path;

**import**   org.apache.hadoop.io.IntWritable;

**import**   org.apache.hadoop.io.LongWritable;

**import**   org.apache.hadoop.io.Text;

**import**   org.apache.hadoop.mapred. FileInputFormat  ;

**import**   org.apache.hadoop.mapred.FileOutputFormat;

**import**   org.apache.hadoop.mapred.JobClient;

**import**   org.apache.hadoop.mapred. JobConf  ;

**import**   org.apache.hadoop.mapred. MapReduceBase  ;

**import**   org.apache.hadoop.mapred. Mapper  ;

**import**   org.apache.hadoop.mapred.OutputCollector;

**import**   org.apache.hadoop.mapred. Reducer  ;

**import**   org.apache.hadoop.mapred.Reporter;

**import**   org.apache.hadoop.mapred. TextInputFormat  ;

**import**   org.apache.hadoop.mapred. TextOutputFormat  ;

/**

*  **@author**    huazhang

*/

@SuppressWarnings ( "deprecation" )

**public**    **class**   WordCount {

 

**public**    **static**    **class**   MyMap  **extends**    MapReduceBase    **implements**

Mapper <LongWritable, Text, Text, IntWritable> {

**private**    **final**    **static**   IntWritable  *one*   =  **new**   IntWritable(1);

**private**   Text  word  =  **new**   Text();

 

**public**    **void**   map(LongWritable key, Text value,

OutputCollector<Text, IntWritable> output, Reporter reporter)

**throws**   IOException {

String line = value.toString();

StringTokenizer tokenizer =  **new**   StringTokenizer(line);

**while**   (tokenizer.hasMoreTokens()) {

word .set(tokenizer.nextToken());

output.collect( word ,  *one*  );

}

}

}

 

**public**    **static**    **class**   MyReduce  **extends**    MapReduceBase    **implements**

Reducer <Text, IntWritable, Text, IntWritable> {

**public**    **void**   reduce(Text key, Iterator<IntWritable> values,

OutputCollector<Text, IntWritable> output, Reporter reporter)

**throws**   IOException {

**int**   sum = 0;

**while**   (values.hasNext()) {

sum += values.next().get();

}

output.collect(key,  **new**   IntWritable(sum));

}

}

 

**public**    **static**    **void**   main(String[] args)  **throws**   Exception {

JobConf   conf =  **new**   JobConf(WordCount. **class**  );

conf.setJobName( "wordcount" );

 

conf.setOutputKeyClass(Text. **class**  );

conf.setOutputValueClass(IntWritable. **class**  );

 

conf.setMapperClass(MyMap. **class**  );

conf.setCombinerClass(MyReduce. **class**  );

conf.setReducerClass(MyReduce. **class**  );

 

conf.setInputFormat( TextInputFormat  . **class**  );

conf.setOutputFormat( TextOutputFormat  . **class**  );

 

FileInputFormat  . *setInputPaths*  (conf,  **new**   Path(args[0]));

FileOutputFormat. *setOutputPath*  (conf,  **new**   Path(args[1]));

 

JobClient.*runJob* (conf);

}

}

 

![]()

## []()2.3   属性配置

按下图所示修改至少 3  个属性, 如下图所示：

   ![]()

 

1.  

1. 
conf/core-site.xml

<configuration>

<property>

<name>fs.default.name</name>

<value>hdfs://zhanghua  .quqi.com:9000</value>

</property>

</configuration>

注意：此处如果是伪集群模式可配置为  hdfs://localhost:9000 ,    是本地模式则为：  localhost:9000   。另外，其他输入输入路径，是本地模式是本地文件系统的路径，是非地模式，用 hdfs  文件系统的路径格式。
1. 
conf/hdfs-site.xml

<configuration>

<property>

<name>dfs.replication</name>

<value>1</value>

</property>

</configuration>
1. 
conf/mapred-site.xml

<configuration>

<property>

<name>mapred.job.tracker</name>

<value>zhanghua  .quqi.com:8021</value>

</property>

</configuration>
1. 
masters

zhanghua  .quqi.com (   伪分布模式就配成  localhost)
1. 
slaves

tadev03  .quqi.com  (   伪分布模式就配成 localhost)
1. 
将以上配置好的 hadoop  文件夹拷到所有机器的相同目录下：

scp -r /home/soft/hadoop-0.20.2 [root@tadev03](mailto:root@tadev03.daodao.com)   [.quqi.com](mailto:root@tadev03.daodao.com)  :/home/soft/hadoop-0.20.2

注意：确保两台机器的  JAVA_HOME   的路径一致，如果不一致，就要改 。

 

hadoop  所有可配置的配置文件说明如下：

hadoop-env.sh   运行 hadoop  的脚本中使用的环境变量

core-site.xml hadoop  的核心配置，如 HDFS  和 MapReduce  中很普遍的 I/O  设置

hdfs-site.xml HDFS  后台程序设置的配置：名称节点，第二名称节点及数据节点

mapred-site.xml MapReduce  后台程序设置的配置： jobtracker  和 tasktracker

masters   记录运行第二名称节点 的机器（一行一个）的列表

slaves   记录运行数据节点的机器（一行一个）的列表

## []()2.4   免密码 SSH  设置

免密码  ssh   设置， 保证至少从   master    可以不用口令登陆所有的   slaves    。

1  ）生成密钥对： ssh-keygen -t rsa -P '' -f /root/.ssh/id_rsa (  这样密钥就留在了客户端 )

2)   将公钥拷到要连接的服务器，

scp /root/.ssh/id_rsa.pub root@tadev03  .quqi.com:/tmp

ssh -l root tadev03  .quqi.com

more /tmp/id_rsa.pub >> /root/.ssh/authorized_keys

1. 
ssh tadev03  .quqi.com   不需要输入密码即为成功。

（注意：伪分布模式也要配置  ssh localhost   无密码登录，如果是  mac   ，请将  ssh   打开）

(  另外，在 mac  中请在 hadoop-config.sh  文件中配置  export JAVA_HOME=/System/Library/Frameworks/JavaVM.framework/Versions/1.6/Home)

 

三条控制线线：

SSH →   这样就可以直接从主节点远程启动从节点上的脚本，如  ssh tadev03  .quqi.com '/var/aa.sh'

NameNode ([http://localhost:50070](http://localhost:50030/) ) → DataNode

JobTracker ( [http://localhost:50030](http://localhost:50030/) )→ TaskTracker ([http://localhost:50060](http://localhost:50030/) )

## []()2.5   配置 hosts

必须配置 master   和 slaves   之间的双向 hosts.   修改 /etc/hosts   进行配置，略。

## []()2.6   格式化 HDFS  文件系统

和我们常见的 NTFS  ， FAT32  文件系统一样， NDFS  最开始也是需要格式化的。格式化过程用来创建存储目录以及名称节点的永久数据结构的初始版本来创建一个空的文件系统。命令如下：

hadoop namenode -format

 

已知问题：在重新格式化时，可能会报： SHUTDOWN_MSG: Shutting down NameNode

解决办法： rm -rf /tmp/hadoop-root/dfs/name

## []()2.7   启动守护进程

1    ）启动   HDFS    守护进程：    start-dfs.sh

(      start-dfs.sh    脚本会参照 NameNode    上 ${HADOOP_CONF_DIR}/slaves    文件的内容，在所有列出的 slave    上启动 DataNode    守护进程。   )

已知问题：在已设置   JAVA_HOME    的情况下仍会报：   Error: JAVA_HOME is not set

解决办法：我是在  hadoop.sh  文件中加下面一句解决的：

JAVA_HOME=/System/Library/Frameworks/JavaVM.framework/Versions/1.6/Home

2  ）启动  Map/Reduce  守护进程：   start-mapred.sh

(      start-mapred.sh   脚本会参照 JobTracker   上 ${HADOOP_CONF_DIR}/slaves   文件的内容，在所有列出的 slave   上启动 TaskTracker   守护进程  )

3)   启动成功后，可以通过访问  http://localhost:50030   验证。

 

注意：也可直接使用  start-all.sh       与  stop-all.sh       脚本  ,       在主节点   master    上面启动   hadoop    ，主节点会启动  /    停止所有从节点的   hadoop    。会启动  5       个   java        进程  ,        同时会在   /tmp        目录下创建五个   pid        文件记录这些进程   ID        号。通过这五个文件，可以得知   namenode, datanode, secondary namenode, jobtracker, tasktracker        分别对应于哪一个   Java        进程。

 

已知问题：启动后，日志中报：  java.io.IOException: File /tmp/hadoop-root/mapred/system/jobtracker.info could only be replicated to 0 nodes, instead of 1

解决办法：原因是    从  tadev03     .quqi.com       机器上无法  ping zhanghua     .quqi.com

## []()2.8   运行程序

先将测试数据及其他输入由本地文件系统拷到  HFDS  文件系统中去（注意：   jar   除外 ）

1.  

1. 
hadoop fs -mkdir input
1. 
hadoop fs -ls .
1. 
hadoop fs -copyFromLocal /home/workspace/hadoopExample/input/file01 input/file01
1. 
hadoop fs -copyFromLocal /home/workspace/hadoopExample/input/file02 input/file02

 

这时候就可以执行下列命令运行程序了，注意：后面的input , output  等目录都是HDFS  文件系统的路径。(  如果是本地模式，就用本地文件系统的绝对路径）

1.  

hadoop     jar   /home/workspace/hadoopExample/hadoopExample.jar com.TripResearch.hadoop.WordCount input/ output

已知问题：在集群模式下运行时任务会Pending

 

最后，运行下列命令查看结果：

/home/soft/hadoop-0.20.2/bin/hadoop fs -cat output/part-00000

 

也可访问下列地址查看状态：

NameNode – [http://zhanghua](http://localhost:50070/)   [.quqi.com](http://localhost:50070/) [:50070/](http://localhost:50070/)

JobTracker - [http://zhanghua](http://localhost:50030/)   [.quqi.com](http://localhost:50030/) [:50030/](http://localhost:50030/)

常用命令说明如下：

hadoop dfs –ls   查看 /usr/root  目录下的内容径；
hadoop dfs –rmr xxx xxx  就是删除目录；
hadoop dfsadmin -report   这个命令可以全局的查看 DataNode  的情况；
hadoop job -list   后面增加参数是对于当前运行的 Job  的操作，例如 list,kill  等；
hadoop balancer   均衡磁盘负载的命令。

# []()3 hadoop  高级进阶

# []()4 hadoop  应用案例

# []()5   参考文献

1. 
[http://hadoop.apache.org/common/docs/r0.18.2/cn/](http://hadoop.apache.org/common/docs/r0.18.2/cn/)
1. 
hadoop 0.20.2  集群配置入门 [http://dev.firnow.com/course/3_program/java/javajs/](http://dev.firnow.com/course/3_program/java/javajs/20100719/453042.html)
1. 
Hadoop 分布式文件系统（HDFS ）初步实践 [http://huatai.me/?p=352](http://huatai.me/?p=352)
1. 
Hadoop 分布式部署实验2_ 格式化分布式文件系统 [http://hi.baidu.com/thinke365/blog/item/15602aa8f9074cf41e17a235.html](http://hi.baidu.com/thinke365/blog/item/15602aa8f9074cf41e17a235.html)
1. 
hadoop 安装出现问题（紧急），请前辈指教 [http://forum.hadoop.tw/viewtopic.php?f=4&t=90](http://forum.hadoop.tw/viewtopic.php?f=4&t=90)
1. 
用 Hadoop  进行分布式并行编程 [http://www.ibm.com/developerworks/cn/opensource/os-cn-hadoop1/index.html](http://www.ibm.com/developerworks/cn/opensource/os-cn-hadoop1/index.html)
1. 
用 Hadoop  进行分布式数据处理 [http://tech.ddvip.com/2010-06/1275983295155033.html](http://tech.ddvip.com/2010-06/1275983295155033.html)

 

分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")
1. 上一篇：[Lucene Scoring 评分机制 （ by quqi99 )](http://blog.csdn.net/quqi99/article/details/6160846)
1. 下一篇：[深入理解各JEE服务器Web层集群原理 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/6292472)
查看评论[]()

  暂无评论
您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fquqi99%2Farticle%2Fdetails%2F6291788)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()
[![TOP]()]( "回到顶部")

个人资料

[![]( "访问我的空间")](http://my.csdn.net/quqi99)
[quqi99](http://my.csdn.net/quqi99)

[]( "[加关注]") []( "[发私信]")
[![]()](http://medal.blog.csdn.net/allmedal.aspx)

* 访问：198660次
* 积分：3337分
* 排名：第1895名

* 原创：146篇
* 转载：23篇
* 译文：0篇
* 评论：123条

文章搜索

[]()

文章分类

* [VM / Cloud](http://blog.csdn.net/quqi99/article/category/875141)(7)
* [Middleware / Java AppServer](http://blog.csdn.net/quqi99/article/category/557281)(13)
* [Seach Engine](http://blog.csdn.net/quqi99/article/category/328029)(5)
* [Linux / Unix / Shell](http://blog.csdn.net/quqi99/article/category/674417)(24)
* [J2SE / JEE](http://blog.csdn.net/quqi99/article/category/328188)(40)
* [DB / NoSQL](http://blog.csdn.net/quqi99/article/category/347580)(9)
* [Architecture](http://blog.csdn.net/quqi99/article/category/803236)(0)
* [Android](http://blog.csdn.net/quqi99/article/category/351802)(3)
* [Life](http://blog.csdn.net/quqi99/article/category/803239)(4)
* [Other](http://blog.csdn.net/quqi99/article/category/689016)(9)
* [OpenStack](http://blog.csdn.net/quqi99/article/category/1112756)(37)
* [Python](http://blog.csdn.net/quqi99/article/category/1139084)(1)
* [C / C++](http://blog.csdn.net/quqi99/article/category/1167554)(2)
* [Networking](http://blog.csdn.net/quqi99/article/category/1490633)(1)
文章存档

* [2013年08月](http://blog.csdn.net/quqi99/article/month/2013/08)(4)
* [2013年07月](http://blog.csdn.net/quqi99/article/month/2013/07)(13)
* [2013年06月](http://blog.csdn.net/quqi99/article/month/2013/06)(6)
* [2013年05月](http://blog.csdn.net/quqi99/article/month/2013/05)(2)
* [2013年04月](http://blog.csdn.net/quqi99/article/month/2013/04)(2)
* [2013年03月](http://blog.csdn.net/quqi99/article/month/2013/03)(3)
* [2013年02月](http://blog.csdn.net/quqi99/article/month/2013/02)(1)
* [2013年01月](http://blog.csdn.net/quqi99/article/month/2013/01)(9)
* [2012年12月](http://blog.csdn.net/quqi99/article/month/2012/12)(3)
* [2012年11月](http://blog.csdn.net/quqi99/article/month/2012/11)(2)
* [2012年08月](http://blog.csdn.net/quqi99/article/month/2012/08)(1)
* [2012年07月](http://blog.csdn.net/quqi99/article/month/2012/07)(1)
* [2012年06月](http://blog.csdn.net/quqi99/article/month/2012/06)(7)
* [2012年05月](http://blog.csdn.net/quqi99/article/month/2012/05)(2)
* [2012年04月](http://blog.csdn.net/quqi99/article/month/2012/04)(6)
* [2012年03月](http://blog.csdn.net/quqi99/article/month/2012/03)(2)
* [2012年02月](http://blog.csdn.net/quqi99/article/month/2012/02)(1)
* [2011年12月](http://blog.csdn.net/quqi99/article/month/2011/12)(1)
* [2011年09月](http://blog.csdn.net/quqi99/article/month/2011/09)(5)
* [2011年08月](http://blog.csdn.net/quqi99/article/month/2011/08)(2)
* [2011年06月](http://blog.csdn.net/quqi99/article/month/2011/06)(2)
* [2011年04月](http://blog.csdn.net/quqi99/article/month/2011/04)(5)
* [2011年03月](http://blog.csdn.net/quqi99/article/month/2011/03)(2)
* [2011年01月](http://blog.csdn.net/quqi99/article/month/2011/01)(4)
* [2010年12月](http://blog.csdn.net/quqi99/article/month/2010/12)(1)
* [2010年07月](http://blog.csdn.net/quqi99/article/month/2010/07)(1)
* [2010年05月](http://blog.csdn.net/quqi99/article/month/2010/05)(1)
* [2010年04月](http://blog.csdn.net/quqi99/article/month/2010/04)(2)
* [2010年03月](http://blog.csdn.net/quqi99/article/month/2010/03)(2)
* [2010年02月](http://blog.csdn.net/quqi99/article/month/2010/02)(4)
* [2010年01月](http://blog.csdn.net/quqi99/article/month/2010/01)(6)
* [2009年12月](http://blog.csdn.net/quqi99/article/month/2009/12)(6)
* [2009年11月](http://blog.csdn.net/quqi99/article/month/2009/11)(5)
* [2009年10月](http://blog.csdn.net/quqi99/article/month/2009/10)(6)
* [2009年09月](http://blog.csdn.net/quqi99/article/month/2009/09)(1)
* [2009年08月](http://blog.csdn.net/quqi99/article/month/2009/08)(1)
* [2009年06月](http://blog.csdn.net/quqi99/article/month/2009/06)(4)
* [2009年03月](http://blog.csdn.net/quqi99/article/month/2009/03)(1)
* [2008年11月](http://blog.csdn.net/quqi99/article/month/2008/11)(1)
* [2008年10月](http://blog.csdn.net/quqi99/article/month/2008/10)(2)
* [2008年08月](http://blog.csdn.net/quqi99/article/month/2008/08)(1)
* [2008年06月](http://blog.csdn.net/quqi99/article/month/2008/06)(3)
* [2008年04月](http://blog.csdn.net/quqi99/article/month/2008/04)(1)
* [2008年03月](http://blog.csdn.net/quqi99/article/month/2008/03)(1)
* [2008年02月](http://blog.csdn.net/quqi99/article/month/2008/02)(1)
* [2008年01月](http://blog.csdn.net/quqi99/article/month/2008/01)(3)
* [2007年12月](http://blog.csdn.net/quqi99/article/month/2007/12)(4)
* [2007年11月](http://blog.csdn.net/quqi99/article/month/2007/11)(1)
* [2007年10月](http://blog.csdn.net/quqi99/article/month/2007/10)(4)
* [2007年08月](http://blog.csdn.net/quqi99/article/month/2007/08)(5)
* [2007年07月](http://blog.csdn.net/quqi99/article/month/2007/07)(6)
* [2007年06月](http://blog.csdn.net/quqi99/article/month/2007/06)(1)
* [2007年05月](http://blog.csdn.net/quqi99/article/month/2007/05)(8)

展开

阅读排行

* [Android学习笔记 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1916553 "Android学习笔记 ( by quqi99 )")(22132)
* [java.lang.NoClassDefFoundError: org/apache/commons/io/output/DeferredFileOutputStream](http://blog.csdn.net/quqi99/article/details/1680647 "java.lang.NoClassDefFoundError: org/apache/commons/io/output/DeferredFileOutputStream")(17851)
* [建立openstack quantum开发环境](http://blog.csdn.net/quqi99/article/details/7433285 "建立openstack quantum开发环境")(6747)
* [在linux中安装字库( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1755642 "在linux中安装字库( by quqi99 )")(5151)
* [Android手机客户端与Servlet交换数据(by quqi99)](http://blog.csdn.net/quqi99/article/details/1930304 "Android手机客户端与Servlet交换数据(by quqi99)")(5140)
* [ReentrantLock与synchronized的区别 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/5298017 "ReentrantLock与synchronized的区别 ( by quqi99 )")(4864)
* [个人户口档案转移笔记（适用北京集体户口）](http://blog.csdn.net/quqi99/article/details/5604732 "个人户口档案转移笔记（适用北京集体户口）")(4802)
* [Magnolia学习笔记（一个基于JSR170的内容管理系统） ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1624225 "Magnolia学习笔记（一个基于JSR170的内容管理系统） ( by quqi99 )")(4342)
* [JSpider学习笔记 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1624218 "JSpider学习笔记 ( by quqi99 )")(4149)
* [Plone学习笔记 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/3099945 "Plone学习笔记 ( by quqi99 )")(4057)
评论排行

* [Android学习笔记 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1916553 "Android学习笔记 ( by quqi99 )")(21)
* [java.lang.NoClassDefFoundError: org/apache/commons/io/output/DeferredFileOutputStream](http://blog.csdn.net/quqi99/article/details/1680647 "java.lang.NoClassDefFoundError: org/apache/commons/io/output/DeferredFileOutputStream")(18)
* [Android手机客户端与Servlet交换数据(by quqi99)](http://blog.csdn.net/quqi99/article/details/1930304 "Android手机客户端与Servlet交换数据(by quqi99)")(12)
* [个人户口档案转移笔记（适用北京集体户口）](http://blog.csdn.net/quqi99/article/details/5604732 "个人户口档案转移笔记（适用北京集体户口）")(8)
* [Magnolia学习笔记（一个基于JSR170的内容管理系统） ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1624225 "Magnolia学习笔记（一个基于JSR170的内容管理系统） ( by quqi99 )")(7)
* [使用itext生成word格式的报表(by quqi99)](http://blog.csdn.net/quqi99/article/details/2591768 "使用itext生成word格式的报表(by quqi99)")(5)
* [在linux中安装字库( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1755642 "在linux中安装字库( by quqi99 )")(4)
* [Android分享文稿 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/6305061 "Android分享文稿 ( by quqi99 )")(4)
* [OpenDaylight学习 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/9156497 "OpenDaylight学习 ( by quqi99 )")(3)
* [使用jacob生成word(by quqi99)](http://blog.csdn.net/quqi99/article/details/2590703 "使用jacob生成word(by quqi99)")(3)

推荐文章
最新评论

* [OpenDaylight学习 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/9156497#comments)

[quqi99](http://blog.csdn.net/quqi99): hi whoeversucks, 谢谢你的实时信息，非常有用，我已经更新到博客里了。另外，问个问题，...
* [OpenDaylight学习 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/9156497#comments)

[whoeversucks](http://blog.csdn.net/whoeversucks): 注意，OpenDayLight Controller和OSCP实际上2个独立的SDN控制器项目（分别...
* [给Linux虚机扩充硬盘空间 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/9102745#comments)

[quqi99](http://blog.csdn.net/quqi99): hi dalinhuang, 谢谢你的回复，你给的这个方法是只适合LVM场景的啊，我没有使用LVM。
* [给Linux虚机扩充硬盘空间 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/9102745#comments)

[dalinhuang](http://blog.csdn.net/dalinhuang): 给根（/）扩充的步骤：（以你的virtualbox并使用LVM为例）1. 新增一块虚拟硬盘，给虚机。...
* [OpenDaylight学习 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/9156497#comments)

[piaochenping](http://blog.csdn.net/piaochenping): 你好，为什么我安装时老是出现这个错误呢？ Failed to execute goal org.co...
* [OpenStack CI测试之devstack-gate ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/7659905#comments)

[quqi99](http://blog.csdn.net/quqi99): @sunyilong2012: 这种错误应该是差模块吧，可以单独安装一下试试, sudo pip i...
* [Fedora 16上源码建立pydev + eclipse的OpenStack开发环境笔记草稿 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/7411091#comments)

[quqi99](http://blog.csdn.net/quqi99): openstack因为用到了一些linux特有的东西，如iptables，所以目前只能跑在linux...
* [Fedora 16上源码建立pydev + eclipse的OpenStack开发环境笔记草稿 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/7411091#comments)

[javaerss](http://blog.csdn.net/javaerss): 大神...看哭了，为此特地跑去下载fedora 16来做实验。之前用ubuntu下用eclipse ...
* [玩转play framework ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/6576375#comments)

[nanfu08](http://blog.csdn.net/nanfu08): 你能看得清，如果只是自己看的话我没话说，这样的文字叫人怎么读？？
* [OpenStack CI测试之devstack-gate ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/7659905#comments)

[dragonsun](http://blog.csdn.net/sunyilong2012): 您好，我在做这个测试的时候遇到了无法导入statsd的问题，请问您有解决的方法吗？+ /home/j...

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)[QQ客服](http://wpa.qq.com/msgrd?v=3&uin=2355263776&site=qq&menu=yes) [微博客服](http://e.weibo.com/csdnsupport/profile) [论坛反馈](http://bbs.csdn.net/forums/Service) [联系邮箱：webmaster@csdn.net](mailto:webmaster@csdn.net) 服务热线：400-600-2320京 ICP 证 070598 号北京创新乐知信息技术有限公司 版权所有世纪乐知(北京)网络技术有限公司 提供技术支持江苏乐知网络技术有限公司 提供商务支持Copyright © 1999-2012, CSDN.NET, All Rights Reserved [![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
![](http://counter.csdn.net/pv.aspx?id=24)
