---
layout: post
title: "Linux大文件传输"
categories: linux
tags: 
 - linux
--- 

# Linux大文件传输

* [首页](http://www.yankay.com/)

* [关于](http://www.yankay.com/about/)

# [我自然](http://www.yankay.com/)

## 颜开的博客

## Linux大文件传输

作者: [颜开](http://www.yankay.com/author/admin/ "查看 颜开 的所有文章") 日期: 2012 年 2 月 7 日  [发表评论]( "发表评论？") (17) [查看评论]( "查看评论？")

我们经常需要在机器之间传输文件。比如备份，复制数据等等。这个是很常见，也是很简单的。用scp或者rsync就能很好的完成任务。但是如果文件很大，需要占用一些传输时间的时候，怎样又快又好地完成任务就很重要了。在我的测试用例中，一个最佳的方案比最差的方案，性能提高了10倍。

### **复制文件**

如果我们是复制一个**未压缩**的文件。这里走如下步骤：

1. 压缩数据
1. 发送到另外一台机器上
1. 数据解压缩
1. 校验正确性
这样做会很有效率，数据压缩后可以更有效的利用带宽

### **使用ZIP+SCP**

我们可以通过ZIP+SCP的组合实现这个功能。

gzip -c /home/yankay/data | ssh yankay01 "gunzip -c - > /home/yankay/data"

这条命令是将/home/yankay/data经过GZIP压缩，通过ssh传输到yankay01的机器上。
data文件的大小是1.1GB,经过Zip压缩后是183MB，执行上面的命令需要45.6s。平均吞吐量为24.7MB/s

我们会发现Scp也有压缩功能，所以上面的语句可以写成
scp -C -c blowfish /home/yankay/data yankay01:/home/yankay/data

这样运行效果是相同的，不通之处在于我使用了blowfish算法作为Scp的密匙算法，使用这个算法可以比默认的情况快很多。单单对与scp,使用了blowfish 吞吐量是62MB/s,不使用只有46MB/s。

可是我执行上面一条命令的时候，发现还是需要45s。平均吞吐量还为24MB/s。没有丝毫的提升，可见瓶颈不在网络上。
那瓶颈在哪里呢？

### **性能分析**

我们先定义几个变量

* 压缩工具的压缩比是 CompressRadio
* 压缩工具的压缩吞吐是CompressSpeed MB/s
* 网络传输的吞吐是 NetSpeed MB/s

由于使用了管道，管道的性能取决于管道中最慢的部分的性能，所以整体的性能是：

speed=min(NetSpeed/CompressRadio,CompressSpeed)

当压缩吞吐较网络传输慢的时候，压缩是瓶颈；但网络较慢的时候，网络传输/吞吐 是瓶颈。

根据现有的测试数据(纯文本)，可以得到表格：
压缩比 吞吐量 千兆网卡(100MB/s)吞吐量 千兆网卡吞吐量,基于ssh(62MB/s) 百兆网卡(10MB/s)吞吐量 ZLIB 35.80% 9.6 9.6 9.6 9.6 LZO 54.40% 101.7 101.7 101.7 18.38235294 LIBLZF 54.60% 134.3 134.3 113.5531136 18.31501832 QUICKLZ 54.90% 183.4 182.1493625 112.9326047 18.21493625 FASTLZ 56.20% 134.4 134.4 110.3202847 17.79359431 SNAPPY 59.80% 189 167.2240803 103.6789298 16.72240803 NONE 100% 300 100 62 10

可以看出来。在千兆网卡下，使用QuickLZ作为压缩算法，可以达到最高的性能。如果使用SSH作为数据传输通道，则远远没有达到网卡可以达到的最佳性能。在百兆网卡的情况下，各个算法相近。对比下来QuickLZ是有优势的。

对于不同的数据和不同的机器，可以得出不同的最佳压缩算法。但有一点是肯定的，尽量把瓶颈压在网络上。对于较慢的网络环境，高压缩比的算法会比较有优势；相反对于较快的网络环境，低压缩比的算法会更好。

### **结论**

根据上面的分析结果，我们不能是用SSH作为网络传输通道，可以使用NC这个基本网络工具，提高性能。同时使用qpress作为压缩算法。
scp /usr/bin/qpress yankay01:/usr/bin/qpress ssh yankay01 "nc -l 12345 | qpress -dio > /home/yankay/data" & qpress -o /home/yankay/data |nc yankay01 12345

第一行是将gpress安装到远程机器上，第二行在远程机器上使用nc监听一个端口，第三行压缩并传送数据。

执行上面的命令需要2.8s。平均吞吐量为402MB/s,比使用Gzip+Scp快了16倍！！

根据上文的公式，和自己的数据，可以绘出上面的表格，就可以选择出最适合的压缩算法和传输方式。达到满意的效果。如果是一个长期运行的脚本的话，这么做是值得的。
Share the post "Linux大文件传输"

* [Weibo](http://service.weibo.com/share/share.php?url=http%3A%2F%2Fwww.yankay.com%2Flinux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93%2F "Share this article on Weibo")
[每日心得](http://www.yankay.com/category/%e6%8a%80%e6%9c%af/%e6%af%8f%e6%97%a5%e5%bf%83%e5%be%97/ "查看 每日心得 中的全部文章")[Linux](http://www.yankay.com/tag/linux/), [传输](http://www.yankay.com/tag/%e4%bc%a0%e8%be%93/), [大文件](http://www.yankay.com/tag/%e5%a4%a7%e6%96%87%e4%bb%b6/)

[← 内存究竟有多快？](http://www.yankay.com/%e5%86%85%e5%ad%98%e7%a9%b6%e7%ab%9f%e6%9c%89%e5%a4%9a%e5%bf%ab%ef%bc%9f/)

[Amazon EBS架构猜想 →](http://www.yankay.com/amazon-ebs%e6%9e%b6%e6%9e%84%e7%8c%9c%e6%83%b3/)

[发表评论？]( "发表评论？")

## 17 条评论。

1. ![]() xjj07 [2012 年 2 月 14 日 在 下午 3:50]()

你说我当年那个网站还能找回来不？

有空再弄一下！
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=2764#respond)
1. ![]() zht [2012 年 2 月 21 日 在 上午 9:53]()

校验正确性

好像没有考虑啊？ ![:lol:]()
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=2791#respond)

* ![]() 颜开 [2012 年 2 月 21 日 在 上午 10:15]()

压缩已经内置了验证。
gzip是有验证的,其他几个没有仔细研究，应该也有。
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=2793#respond)
* ![]() [renenglish](http://magicode.me/) [2012 年 2 月 24 日 在 下午 12:10]()

"平均吞吐量为402MB/s" 压缩速度跟网速度都没打到200M/s ，平均怎么到了402MB/s ?
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=2813#respond)

* ![]() 颜开 [2012 年 2 月 28 日 在 下午 5:33]()

我又重新测了一下还是402MB/s。是这样的，上面200M/s用的测试数据和下面1.1GB的文件不是一个文件。压缩比相差很大，所以达到了402MB/s。怪我没有说清楚，自己都记不清了。
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=2847#respond)
* ![]() 阿干 [2013 年 1 月 8 日 在 下午 11:00]()

请参考engineering.tumblr.com/post/7658008285/efficiently-copying-files-to-multiple-destinations
或者andrew.tumblr.com/post/2316602611
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=4526#respond)

* ![]() 颜开 [2013 年 1 月 17 日 在 下午 1:13]()

谢谢你
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=4553#respond)
* ![]() [liuw](http://blog.liuw.name/) [2013 年 1 月 9 日 在 上午 9:37]()

1.1G的文件压缩到183M，这个说明文件冗余还是很大的。但是一般的二进制数据还会有这么好的效果吗？

另外有时候用scp还是有安全方面的考虑的吧，在公网上这样的方法可能要考虑过再使用了。

P.S 还是要大大赞一下钻研精神
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=4527#respond)
* ![]() 小莫飞 [2013 年 1 月 9 日 在 下午 8:02]()

请问性能运算公式是怎么看得？和表里的数据有对应关系吗
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=4529#respond)
* [Linux大文件传输 | 互联网纪事](http://www.xymyeah.com/1274.html) - pingback on 2013 年 1 月 10 日 在 下午 10:51
* ![]() erazy0 [2013 年 1 月 10 日 在 下午 11:20]()

压缩工具的压缩比是 CompressRadio
====
Radio --> Ratio
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=4533#respond)
* ![]() shell [2013 年 1 月 11 日 在 下午 1:06]()

猜测瓶颈在压缩算法跑满单核上。用mpstat测试一下？
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=4539#respond)
* ![]() 深夜两点 [2013 年 1 月 18 日 在 下午 7:18]()

我用下面的命令传数据是成功了，不过问题是在目标机器上，总是有俩tar进程，一个gzip进程，一个nc进程不结束。

cd /home/mengzang/zt;
ssh [http://www.myhost.com](http://www.myhost.com/) "nc -l 22345 | tar -zxf - -C /home/mengzang/zt/ggg" &
tar -zcf - 1.txt 2.txt d1 | nc [http://www.myhost.com](http://www.myhost.com/) 22345
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=4559#respond)
* ![]() [vfhky](http://www.huangkeye.com/) [2013 年 3 月 5 日 在 下午 2:59]()

博主真牛，不得不赞！
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=4823#respond)
* ![]() bridge [2013 年 4 月 22 日 在 上午 11:31]()

压缩是cpu换取传输时间
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=5770#respond)
* ![]() [lyman](http://blog1980.info/) [2013 年 5 月 3 日 在 下午 6:24]()

nc 很裸很强大。但是如果要考虑断点续传的话（大文件嘛，呵呵），还是直接 rsync 比较省事
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=5907#respond)
* ![]() 虎子 [2013 年 6 月 28 日 在 上午 11:31]()

hello,请问gpress 这个是哪个软件包里面的工具呢
[回复](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/?replytocom=6492#respond)
### 发表评论 [取消回复]()

昵称

邮箱

网址

[![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]()

注意 - 你可以用以下 HTML tags and attributes:

<a href="" title=""> <abbr title=""> <acronym title=""> <b> <blockquote cite=""> <cite> <code> <del datetime=""> <em> <i> <q cite=""> <strike> <strong>

### Trackbacks and Pingbacks:

* [Linux大文件传输 | 互联网纪事](http://www.xymyeah.com/1274.html) - Pingback on 2013/01/10/ 22:51
[RSS 订阅](http://www.yankay.com/feed/ "RSS 订阅") [微博](http://weibo.com/yankaycom "微博")

### 近期文章

* [Scala Tour - 精选](http://www.yankay.com/scala-tour-choiceness/ "Scala Tour - 精选")
* [NoSQL反模式 - 文档数据库篇](http://www.yankay.com/nosql-anti-pattern-document/ "NoSQL反模式 - 文档数据库篇")
* [2012年学习小结](http://www.yankay.com/2012%e5%b9%b4%e5%ad%a6%e4%b9%a0%e5%b0%8f%e7%bb%93/ "2012年学习小结")
* [Go-简洁的并发](http://www.yankay.com/go-clear-concurreny/ "Go-简洁的并发")
* [Google Spanner原理- 全球级的分布式数据库](http://www.yankay.com/google-spanner%e5%8e%9f%e7%90%86-%e5%85%a8%e7%90%83%e7%ba%a7%e7%9a%84%e5%88%86%e5%b8%83%e5%bc%8f%e6%95%b0%e6%8d%ae%e5%ba%93/ "Google Spanner原理- 全球级的分布式数据库")
* [Google Dremel 原理 - 如何能3秒分析1PB](http://www.yankay.com/google-dremel-rationale/ "Google Dremel 原理 - 如何能3秒分析1PB")
* [Java使用"指针"快速比较字节](http://www.yankay.com/java-fast-byte-comparison/ "Java使用"指针"快速比较字节")
* [HBase介绍PPT](http://www.yankay.com/hbase-slider/ "HBase介绍PPT")
* [Amazon EBS架构猜想](http://www.yankay.com/amazon-ebs%e6%9e%b6%e6%9e%84%e7%8c%9c%e6%83%b3/ "Amazon EBS架构猜想")
* [Linux大文件传输](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/ "Linux大文件传输")

### 标签

[Android](http://www.yankay.com/tag/android/ "1 个话题") [apache](http://www.yankay.com/tag/apache/ "1 个话题") [arch](http://www.yankay.com/tag/arch/ "1 个话题") [BigTable](http://www.yankay.com/tag/bigtable/ "1 个话题") [Common Cloud](http://www.yankay.com/tag/common-cloud/ "1 个话题") [eclipse](http://www.yankay.com/tag/eclipse/ "2 个话题") [Facebook](http://www.yankay.com/tag/facebook/ "2 个话题") [FriendMap](http://www.yankay.com/tag/friendmap/ "1 个话题") [Gae](http://www.yankay.com/tag/gae/ "3 个话题") [GaeS](http://www.yankay.com/tag/gaes/ "5 个话题") [GaeVfs](http://www.yankay.com/tag/gaevfs/ "1 个话题") [GDP](http://www.yankay.com/tag/gdp/ "1 个话题") [GGG](http://www.yankay.com/tag/ggg/ "1 个话题") [Giant Global Graph](http://www.yankay.com/tag/giant-global-graph/ "1 个话题") [Google Reader](http://www.yankay.com/tag/google-reader/ "1 个话题") [Google Reader Play](http://www.yankay.com/tag/google-reader-play/ "1 个话题") [Hadoop](http://www.yankay.com/tag/hadoop/ "2 个话题") [Hbase](http://www.yankay.com/tag/hbase/ "2 个话题") [HTML5](http://www.yankay.com/tag/html5/ "2 个话题") [Java](http://www.yankay.com/tag/java/ "4 个话题") [JPA2.0](http://www.yankay.com/tag/jpa2-0/ "1 个话题") [Jsa4j](http://www.yankay.com/tag/jsa4j/ "2 个话题") [latex](http://www.yankay.com/tag/latex/ "2 个话题") [Linux](http://www.yankay.com/tag/linux/ "2 个话题") [maven](http://www.yankay.com/tag/maven/ "1 个话题") [NoSQL](http://www.yankay.com/tag/nosql/ "5 个话题") [TortoiseHg](http://www.yankay.com/tag/tortoisehg/ "2 个话题") [Ubuntu](http://www.yankay.com/tag/ubuntu/ "2 个话题") [中文](http://www.yankay.com/tag/%e4%b8%ad%e6%96%87/ "2 个话题") [中科杯](http://www.yankay.com/tag/%e4%b8%ad%e7%a7%91%e6%9d%af/ "1 个话题") [乱码](http://www.yankay.com/tag/%e4%b9%b1%e7%a0%81/ "2 个话题") [优化](http://www.yankay.com/tag/%e4%bc%98%e5%8c%96/ "2 个话题") [动态规划](http://www.yankay.com/tag/%e5%8a%a8%e6%80%81%e8%a7%84%e5%88%92/ "1 个话题") [同学](http://www.yankay.com/tag/%e5%90%8c%e5%ad%a6/ "1 个话题") [图书馆](http://www.yankay.com/tag/%e5%9b%be%e4%b9%a6%e9%a6%86/ "1 个话题") [在水滩](http://www.yankay.com/tag/%e5%9c%a8%e6%b0%b4%e6%bb%a9/ "1 个话题") [数据库](http://www.yankay.com/tag/%e6%95%b0%e6%8d%ae%e5%ba%93/ "1 个话题") [文件](http://www.yankay.com/tag/%e6%96%87%e4%bb%b6/ "1 个话题") [模板](http://www.yankay.com/tag/%e6%a8%a1%e6%9d%bf/ "1 个话题") [江苏杯](http://www.yankay.com/tag/%e6%b1%9f%e8%8b%8f%e6%9d%af/ "1 个话题") [知识图](http://www.yankay.com/tag/%e7%9f%a5%e8%af%86%e5%9b%be/ "1 个话题") [立志](http://www.yankay.com/tag/%e7%ab%8b%e5%bf%97/ "1 个话题") [维基百科](http://www.yankay.com/tag/%e7%bb%b4%e5%9f%ba%e7%99%be%e7%a7%91/ "1 个话题") [视频](http://www.yankay.com/tag/%e8%a7%86%e9%a2%91/ "1 个话题") [酒](http://www.yankay.com/tag/%e9%85%92/ "1 个话题")
### 我的应用

* [Scala Tour](http://zh.scala-tour.com/ "精彩的Scala之旅")
* [趣味应用-微八卦](http://weibagua.cloudfoundry.com/)

### 相关组织

* [EMC中国研究院](http://qing.weibo.com/emclabschina "EMC中国研究院")
* [nosqlfan](http://blog.nosqlfan.com/)
### 友情链接

* [Layne Peng的博客](http://laynepeng.cloudfoundry.com/)
* [爱陶](http://www.iiira.com/)
* [王小龙的博客](http://larry-wang.com/)
* [高晟](http://loveharbor.net/)
版权所有 © 2013 我自然 | Powered by [WordPress](http://wordpress.org/) and [zBench](http://zww.me/)
↑ [回到顶部]( "Back to top")


