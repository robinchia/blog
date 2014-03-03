---
layout: post
title: "HDFS写入和读取流程"
categories: hadoop
tags: 
 - hadoop
--- 

# HDFS写入和读取流程

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

# [guisu，程序人生。](http://blog.csdn.net/hguisu)

## 能干的人解决问题。智慧的人绕开问题(A clever person solves a problem. A wise person avoids it)

* [![]()目录视图](http://blog.csdn.net/hguisu?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/hguisu?viewmode=list)
* [![]()订阅](http://blog.csdn.net/hguisu/rss/list)
[2014年1月微软MVP申请开始啦！](http://blog.csdn.net/blogdevteam/article/details/11889881)      [CSDN社区中秋晒福利活动正式开始啦！](http://bbs.csdn.net/topics/390594487)        [专访钟声：Java程序员，上班那点事儿](http://www.csdn.net/article/2013-09-17/2816962)      [独一无二的职位：开源社区经理](http://blog.csdn.net/adali/article/details/9813651)      [“说说家乡的互联网”主题有奖征文](http://blog.csdn.net/blogdevteam/article/details/11975399)

### [HDFS写入和读取流程]()

分类： [云计算hadoop](http://blog.csdn.net/hguisu/article/category/1072794)  2012-02-14 23:50 8282人阅读 [评论]()(17) [收藏]( "收藏") [举报]( "举报")
[存储](http://blog.csdn.net/tag/details.html?tag=%e5%ad%98%e5%82%a8)[hadoop](http://blog.csdn.net/tag/details.html?tag=hadoop)[image](http://blog.csdn.net/tag/details.html?tag=image)[system](http://blog.csdn.net/tag/details.html?tag=system)[mysql](http://blog.csdn.net/tag/details.html?tag=mysql)

目录[(?)]( "系统根据文章中H1到H6标签自动生成文章目录")[[+]]( "展开")

1. [一HDFS]()
1. [二HDFS的体系结构]()
1. [三读写流程]()

1. [GFS论文提到的文件读取简单流程]()
1. []()
1. []()
1. []()
1. [详细流程]()
1. []()
1. [GFS论文提到的写入文件简单流程]()

1. [详细流程]()
1. []()
## []()一、HDFS

HDFS全称是Hadoop Distributed System。HDFS是为以流的方式存取大文件而设计的。适用于几百MB，GB以及TB，并写一次读多次的场合。而对于低延时数据访问、大量小文件、同时写和任意的文件修改，则并不是十分适合。

目前HDFS支持的使用接口除了Java的还有，Thrift、C、FUSE、WebDAV、HTTP等。HDFS是以block-sized chunk组织其文件内容的，默认的block大小为64MB，对于不足64MB的文件，其会占用一个block，但实际上不用占用实际硬盘上的64MB，这可以说是HDFS是在文件系统之上架设的一个中间层。之所以将默认的block大小设置为64MB这么大，是因为block-sized对于文件定位很有帮助，同时大文件更使传输的时间远大于文件寻找的时间，这样可以最大化地减少文件定位的时间在整个文件获取总时间中的比例 。

## []()二、HDFS的体系结构

构成HDFS主要是Namenode（master）和一系列的Datanode（workers）。Namenode是管理HDFS的目录树和相关的文件元数据，这些信息是以"namespace image"和"edit log"两个文件形式存放在本地磁盘，但是这些文件是在HDFS每次重启的时候重新构造出来的。Datanode则是存取文件实际内容的节点，Datanodes会定时地将block的列表汇报给Namenode。

由于Namenode是元数据存放的节点，如果Namenode挂了那么HDFS就没法正常运行，因此一般使用将元数据持久存储在本地或远程的机器上，或者使用secondary namenode来定期同步Namenode的元数据信息，secondary namenode有点类似于MySQL的Master/Salves中的Slave，"edit log"就类似"bin log"。如果Namenode出现了故障，一般会将原Namenode中持久化的元数据拷贝到secondary namenode中，使secondary namenode作为新的Namenode运行起来。

                            ![]()

## []()三、读写流程

### []()GFS论文提到的文件读取简单流程：

### []()

### []()                ![]()

### []()**** 

### []()**详细流程：**![reading data from hdfs](http://blog.endlesscode.com/wp-content/uploads/2010/06/reading-data-from-hdfs.png "reading data from hdfs")

文件读取的过程如下：

1. 使用HDFS提供的客户端开发库Client，向远程的Namenode发起RPC请求；
1. Namenode会视情况返回文件的部分或者全部block列表，对于每个block，Namenode都会返回有该block拷贝的DataNode地址；
1. 客户端开发库Client会选取离客户端最接近的DataNode来读取block；如果客户端本身就是DataNode,那么将从本地直接获取数据.
1. 读取完当前block的数据后，关闭与当前的DataNode连接，并为读取下一个block寻找最佳的DataNode；
1. 当读完列表的block后，且文件读取还没有结束，客户端开发库会继续向Namenode获取下一批的block列表。
1. 读取完一个block都会进行checksum验证，如果读取datanode时出现错误，客户端会通知Namenode，然后再从下一个拥有该block拷贝的datanode继续读。

### []()

### []()GFS论文提到的写入文件简单流程：

                                     ![]()             

## []()详细流程：![writing data to hdfs](http://blog.endlesscode.com/wp-content/uploads/2010/06/writing-data-to-hdfs.png "writing data to hdfs")

写入文件的过程比读取较为复杂：

1. 使用HDFS提供的客户端开发库Client，向远程的Namenode发起RPC请求；
1. Namenode会检查要创建的文件是否已经存在，创建者是否有权限进行操作，成功则会为文件**创建一个记录**，否则会让客户端抛出异常；
1. 当客户端开始写入文件的时候，开发库会将文件切分成多个packets，并在内部以数据队列"data queue"的形式管理这些packets，并向Namenode申请新的blocks，获取用来存储replicas的合适的datanodes列表，列表的大小根据在Namenode中对replication的设置而定。
1. 开始以pipeline（管道）的形式将packet写入所有的replicas中。开发库把packet以流的方式写入第一个datanode，该datanode把该packet存储之后，再将其传递给在此pipeline中的下一个datanode，直到最后一个datanode，这种写数据的方式呈流水线的形式。
1. 最后一个datanode成功存储之后会返回一个ack packet，在pipeline里传递至客户端，在客户端的开发库内部维护着"ack queue"，成功收到datanode返回的ack packet后会从"ack queue"移除相应的packet。
1. 如果传输过程中，有某个datanode出现了故障，那么当前的pipeline会被关闭，出现故障的datanode会从当前的pipeline中移除，剩余的block会继续剩下的datanode中继续以pipeline的形式传输，同时Namenode会分配一个新的datanode，保持replicas设定的数量。

## []()

分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")
1. 上一篇：[Hadoop Hive sql语法详解](http://blog.csdn.net/hguisu/article/details/7256833)
1. 下一篇：[Hadoop HDFS分布式文件系统设计要点与架构](http://blog.csdn.net/hguisu/article/details/7261145)

顶 3 踩 1
查看评论[]()

3楼 [huangbo1988911](http://blog.csdn.net/huangbo1988911) 2012-04-09 11:25发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/huangbo1988911)在写数据的过程中，一个文件被分割成很多blocks，这些block是按顺序一个个操作的，还是并发的进行传输的？Re: [真实的归宿](http://blog.csdn.net/hguisu) 2012-04-09 12:45发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/hguisu)回复huangbo1988911：写数据的是以流的方式传输，即管道的方式，一个一个block顺序传输。而不是像树形拓扑结构那样分散传输。Re: [huangbo1988911](http://blog.csdn.net/huangbo1988911) 2012-04-17 18:03发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/huangbo1988911)回复hguisu：您好，很感谢您回答我，但是我仍有点疑惑，hdfs中的文件大小区分为：chunk<packet<block,在每个packet的传输到多个DN（datanode）的过程中是以pipeline方式，但是当其中一个block在以这种方式传输时，其他的block是要等待还是并发的进行呢？谢谢！Re: [真实的归宿](http://blog.csdn.net/hguisu) 2012-04-20 16:23发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/hguisu)回复huangbo1988911：管道方式，即是队列方式传输。只能一个block传完了，接着传下个block。Re: [huangbo1988911](http://blog.csdn.net/huangbo1988911) 2012-04-30 18:18发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/huangbo1988911)回复hguisu：你好！如果这样，那hdfs的并发写实如何体现的呢？Re: [真实的归宿](http://blog.csdn.net/hguisu) 2012-05-02 09:29发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/hguisu)回复huangbo1988911：hdfs没有并发写入。Re: [huangbo1988911](http://blog.csdn.net/huangbo1988911) 2012-05-03 23:39发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/huangbo1988911)回复hguisu：这样的话，那hadoop是比较适合大数据的处理了，对于文件的写的速度并没有多大的提高了?Re: [真实的归宿](http://blog.csdn.net/hguisu) 2012-05-04 09:40发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/hguisu)回复huangbo1988911：hadoop本来就是通往云服务的捷径，为处理超大数据集而准备。Re: [huangbo1988911](http://blog.csdn.net/huangbo1988911) 2012-05-07 10:57发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/huangbo1988911)回复hguisu：这样hadoop的主要优势是在map/reduce那一块，而其文件系统有什么样的优势呢（在文件的读写方面，和其他的文件系统）？Re: [真实的归宿](http://blog.csdn.net/hguisu) 2012-05-07 11:50发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/hguisu)回复huangbo1988911：hdfs可以存储超大数据，而map/reduce要处理的数据存储在hdfs上，即MR分布式运算。Re: [huangbo1988911](http://blog.csdn.net/huangbo1988911) 2012-05-09 22:35发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/huangbo1988911)回复hguisu： 那多个文件同时向hdfs写入是如何进行的呢？Re: [huangbo1988911](http://blog.csdn.net/huangbo1988911) 2012-05-09 00:39发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/huangbo1988911)回复hguisu：恩，hdfs相对于其他的文件系统，除了更适合存储大数据以外，而且有很强的容错能力，但是对数据的读写等，没有并发性，只是采用了管道的方式，这可能是它的一个小缺点吧。2楼 [CD_xiaoxin](http://blog.csdn.net/CD_xiaoxin) 2012-03-19 09:44发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/CD_xiaoxin)很详细 很有帮助 谢谢1楼 [lin_FS](http://blog.csdn.net/lin_FS) 2012-03-16 10:01发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/lin_FS)client端的那个queue是在内存中，还是写在临时文件里了？Re: [真实的归宿](http://blog.csdn.net/hguisu) 2012-03-19 09:43发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/hguisu)回复lin_FS：client分割数据成一个个block（packet）这些数据都不在内存中，你可以想象，如果一个数据是100G，它你那个放进内存吗？Re: [lin_FS](http://blog.csdn.net/lin_FS) 2012-03-21 17:26发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/lin_FS)在client端， 多个block（packet）组成一个队列，然后可以想象把文件（100G）分成若干个packet，如果队列满了就根本写不进去数据了，根本不会出现你想象的那种情况。我想了解的是，这个队列在内存中还是以文件的形式，呵呵Re: [真实的归宿](http://blog.csdn.net/hguisu) 2012-03-31 18:47发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/hguisu)回复lin_FS：这个队列肯定是文件的形式存在的。
您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fhguisu%2Farticle%2Fdetails%2F7259716)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()

[hguisu]()
[![TOP]()]( "回到顶部")

个人资料

[![]( "访问我的空间")](http://my.csdn.net/hguisu)
[真实的归宿](http://my.csdn.net/hguisu)

[]( "[加关注]") []( "[发私信]")
[![]()](http://medal.blog.csdn.net/allmedal.aspx)

* 访问：481035次
* 积分：6666分
* 排名：第614名

* 原创：190篇
* 转载：1篇
* 译文：0篇
* 评论：313条

文章分类

* [操作系统](http://blog.csdn.net/hguisu/article/category/1253451)(5)
* [Linux](http://blog.csdn.net/hguisu/article/category/796967)(17)
* [MySQL](http://blog.csdn.net/hguisu/article/category/796963)(12)
* [PHP](http://blog.csdn.net/hguisu/article/category/796962)(41)
* [PHP内核](http://blog.csdn.net/hguisu/article/category/1104862)(11)
* [技术人生](http://blog.csdn.net/hguisu/article/category/796968)(7)
* [数据结构与算法](http://blog.csdn.net/hguisu/article/category/1054628)(27)
* [云计算hadoop](http://blog.csdn.net/hguisu/article/category/1072794)(20)
* [网络知识](http://blog.csdn.net/hguisu/article/category/1075597)(7)
* [c/c++](http://blog.csdn.net/hguisu/article/category/1080443)(22)
* [memcache](http://blog.csdn.net/hguisu/article/category/1099674)(5)
* [HipHop](http://blog.csdn.net/hguisu/article/category/1111071)(2)
* [计算机原理](http://blog.csdn.net/hguisu/article/category/1112019)(4)
* [Java](http://blog.csdn.net/hguisu/article/category/1114530)(7)
* [socket网络编程](http://blog.csdn.net/hguisu/article/category/1122753)(5)
* [设计模式](http://blog.csdn.net/hguisu/article/category/1133340)(26)
* [AOP](http://blog.csdn.net/hguisu/article/category/1151353)(2)
* [重构](http://blog.csdn.net/hguisu/article/category/1152364)(11)
* [重构与模式](http://blog.csdn.net/hguisu/article/category/1173389)(1)
* [大数据处理](http://blog.csdn.net/hguisu/article/category/1209788)(11)
* [搜索引擎Search Engine](http://blog.csdn.net/hguisu/article/category/1230933)(15)
* [HTML5](http://blog.csdn.net/hguisu/article/category/1302430)(1)
* [Android](http://blog.csdn.net/hguisu/article/category/1309674)(1)
* [webserver](http://blog.csdn.net/hguisu/article/category/1422000)(3)
* [NOSQL](http://blog.csdn.net/hguisu/article/category/1429288)(6)
文章存档

* [2013年09月](http://blog.csdn.net/hguisu/article/month/2013/09)(2)
* [2013年08月](http://blog.csdn.net/hguisu/article/month/2013/08)(1)
* [2013年07月](http://blog.csdn.net/hguisu/article/month/2013/07)(2)
* [2013年06月](http://blog.csdn.net/hguisu/article/month/2013/06)(3)
* [2013年05月](http://blog.csdn.net/hguisu/article/month/2013/05)(3)
* [2013年03月](http://blog.csdn.net/hguisu/article/month/2013/03)(3)
* [2013年02月](http://blog.csdn.net/hguisu/article/month/2013/02)(2)
* [2013年01月](http://blog.csdn.net/hguisu/article/month/2013/01)(2)
* [2012年12月](http://blog.csdn.net/hguisu/article/month/2012/12)(4)
* [2012年11月](http://blog.csdn.net/hguisu/article/month/2012/11)(3)
* [2012年10月](http://blog.csdn.net/hguisu/article/month/2012/10)(2)
* [2012年09月](http://blog.csdn.net/hguisu/article/month/2012/09)(15)
* [2012年08月](http://blog.csdn.net/hguisu/article/month/2012/08)(6)
* [2012年07月](http://blog.csdn.net/hguisu/article/month/2012/07)(8)
* [2012年06月](http://blog.csdn.net/hguisu/article/month/2012/06)(14)
* [2012年05月](http://blog.csdn.net/hguisu/article/month/2012/05)(29)
* [2012年04月](http://blog.csdn.net/hguisu/article/month/2012/04)(26)
* [2012年03月](http://blog.csdn.net/hguisu/article/month/2012/03)(27)
* [2012年02月](http://blog.csdn.net/hguisu/article/month/2012/02)(18)
* [2011年12月](http://blog.csdn.net/hguisu/article/month/2011/12)(7)
* [2011年01月](http://blog.csdn.net/hguisu/article/month/2011/01)(8)
* [2010年07月](http://blog.csdn.net/hguisu/article/month/2010/07)(6)
* [2007年12月](http://blog.csdn.net/hguisu/article/month/2007/12)(2)

展开

阅读排行

* [Hadoop集群配置（最全面总结）](http://blog.csdn.net/hguisu/article/details/7237395 "Hadoop集群配置（最全面总结）")(23024)
* [设计模式（五）适配器模式Adapter（结构型）](http://blog.csdn.net/hguisu/article/details/7527842 "设计模式（五）适配器模式Adapter（结构型）")(21421)
* [hbase安装配置（整合到hadoop）](http://blog.csdn.net/hguisu/article/details/7244413 "hbase安装配置（整合到hadoop）")(20780)
* [socket阻塞与非阻塞，同步与异步、I/O模型](http://blog.csdn.net/hguisu/article/details/7453390 "socket阻塞与非阻塞，同步与异步、I/O模型")(12788)
* [Hadoop Hive与Hbase整合](http://blog.csdn.net/hguisu/article/details/7282050 "Hadoop Hive与Hbase整合")(11869)
* [设计模式 ( 十八 ) 策略模式Strategy（对象行为型）](http://blog.csdn.net/hguisu/article/details/7558249 "设计模式 ( 十八 ) 策略模式Strategy（对象行为型）")(11737)
* [B-树和B+树的应用：数据搜索和数据库索引](http://blog.csdn.net/hguisu/article/details/7786014 "B-树和B+树的应用：数据搜索和数据库索引")(11507)
* [Mysql 多表联合查询效率分析及优化](http://blog.csdn.net/hguisu/article/details/5731880 "Mysql 多表联合查询效率分析及优化")(10454)
* [谷歌三大核心技术（三）Google BigTable中文版](http://blog.csdn.net/hguisu/article/details/7244991 "谷歌三大核心技术（三）Google BigTable中文版")(9958)
* [HDFS写入和读取流程]( "HDFS写入和读取流程")(8282)
评论排行

* [设计模式 ( 十八 ) 策略模式Strategy（对象行为型）](http://blog.csdn.net/hguisu/article/details/7558249 "设计模式 ( 十八 ) 策略模式Strategy（对象行为型）")(33)
* [HDFS写入和读取流程]( "HDFS写入和读取流程")(17)
* [设计模式（六）桥连模式Bridge（结构型）](http://blog.csdn.net/hguisu/article/details/7529194 "设计模式（六）桥连模式Bridge（结构型）")(14)
* [设计模式（一）工厂模式Factory（创建型）](http://blog.csdn.net/hguisu/article/details/7505909 "设计模式（一）工厂模式Factory（创建型）")(13)
* [海量数据处理算法—Bit-Map](http://blog.csdn.net/hguisu/article/details/7880288 "海量数据处理算法—Bit-Map")(13)
* [Hadoop集群配置（最全面总结）](http://blog.csdn.net/hguisu/article/details/7237395 "Hadoop集群配置（最全面总结）")(13)
* [PHP SOCKET编程](http://blog.csdn.net/hguisu/article/details/7448528 "PHP SOCKET编程")(11)
* [socket阻塞与非阻塞，同步与异步、I/O模型](http://blog.csdn.net/hguisu/article/details/7453390 "socket阻塞与非阻塞，同步与异步、I/O模型")(11)
* [Hadoop Hive与Hbase整合](http://blog.csdn.net/hguisu/article/details/7282050 "Hadoop Hive与Hbase整合")(10)
* [hbase安装配置（整合到hadoop）](http://blog.csdn.net/hguisu/article/details/7244413 "hbase安装配置（整合到hadoop）")(10)

推荐文章
最新评论

* [socket阻塞与非阻塞，同步与异步、I/O模型](http://blog.csdn.net/hguisu/article/details/7453390#comments)

[ctqctq99](http://blog.csdn.net/ctqctq99): 他的意思可能是阻塞IO和非阻塞IO的区别就在于：应用程序的调用是否立即返回！这句话说反了。应该是非阻...
* [硬盘的读写原理](http://blog.csdn.net/hguisu/article/details/7408047#comments)

[m1013923728](http://blog.csdn.net/m1013923728): 写的通俗易懂！
* [C语言中的宏定义](http://blog.csdn.net/hguisu/article/details/7470695#comments)

[ouwen3536](http://blog.csdn.net/ouwen3536): 很半，转起！
* [C语言中的宏定义](http://blog.csdn.net/hguisu/article/details/7470695#comments)

[zhangyongbluesky](http://blog.csdn.net/zhangyongbluesky): good
* [hbase安装配置（整合到hadoop）](http://blog.csdn.net/hguisu/article/details/7244413#comments)

[真实的归宿](http://blog.csdn.net/hguisu): @u012171806:conf/hbase-site.xml你应该看官方文档的快速入门。安装的东西...
* [hbase安装配置（整合到hadoop）](http://blog.csdn.net/hguisu/article/details/7244413#comments)

[JAVA_小陈](http://blog.csdn.net/u012171806): 我把hbase下载了，你上面说的需要配置一个xml文件，请问配置在什么地方呢？然后启动的时候是在什么...
* [硬盘的读写原理](http://blog.csdn.net/hguisu/article/details/7408047#comments)

[mxhlee](http://blog.csdn.net/mxhlee): 好东西啊！！！（实际是斜切向运动）这句话让我纠结了很长时间、我自己就感觉是这运动 但是没有一个介绍...
* [socket阻塞与非阻塞，同步与异步、I/O模型](http://blog.csdn.net/hguisu/article/details/7453390#comments)

[真实的归宿](http://blog.csdn.net/hguisu): @liaokailin:并没有反。实际就是这样的。
* [socket阻塞与非阻塞，同步与异步、I/O模型](http://blog.csdn.net/hguisu/article/details/7453390#comments)

[真实的归宿](http://blog.csdn.net/hguisu): @liaokailin:实际就是这样的。
* [socket阻塞与非阻塞，同步与异步、I/O模型](http://blog.csdn.net/hguisu/article/details/7453390#comments)

[廖凯林](http://blog.csdn.net/liaokailin): 同步IO和异步IO的区别就在于：数据拷贝的时候进程是否阻塞！阻塞IO和非阻塞IO的区别就在于：应用程...

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)[QQ客服](http://wpa.qq.com/msgrd?v=3&uin=2355263776&site=qq&menu=yes) [微博客服](http://e.weibo.com/csdnsupport/profile) [论坛反馈](http://bbs.csdn.net/forums/Service) [联系邮箱：webmaster@csdn.net](mailto:webmaster@csdn.net) 服务热线：400-600-2320京 ICP 证 070598 号北京创新乐知信息技术有限公司 版权所有世纪乐知(北京)网络技术有限公司 提供技术支持江苏乐知网络技术有限公司 提供商务支持Copyright © 1999-2012, CSDN.NET, All Rights Reserved [![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
![](http://counter.csdn.net/pv.aspx?id=24)
