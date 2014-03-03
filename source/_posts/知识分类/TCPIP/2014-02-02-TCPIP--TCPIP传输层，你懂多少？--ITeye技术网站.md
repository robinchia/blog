---
layout: post
title: "TCP IP传输层，你懂多少？ - - ITeye技术网站"
categories: TCPIP
tags: 
 - TCPIP
--- 

# TCP IP传输层，你懂多少？ - - ITeye技术网站

[首页](http://www.iteye.com/) [新闻](http://www.iteye.com/news) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [招聘](http://www.iteye.com/job) [更多 ▼](http://java-mzd.iteye.com/blog/1007577#)

[专栏](http://www.iteye.com/wiki)  [群组](http://www.iteye.com/groups) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://java-mzd.iteye.com/login "登录") [我的应用](http://www.iteye.com/all) [登录](http://java-mzd.iteye.com/login) [注册](http://java-mzd.iteye.com/signup)

# [java-mzd](http://java-mzd.iteye.com/)

永久域名 [http://java-mzd.iteye.com](http://java-mzd.iteye.com/)

[141顶](http://java-mzd.iteye.com/blog/1007577#)
[25踩](http://java-mzd.iteye.com/blog/1007577#)

[TCP/IP网络层谜云](http://java-mzd.iteye.com/blog/1019088 "TCP/IP网络层谜云") | [Jpcap简易教程](http://java-mzd.iteye.com/blog/1005945 "Jpcap简易教程")

2011-04-19

### [TCP/IP传输层，你懂多少？]()

**文章分类:[综合技术](http://www.iteye.com/blogs/category/tech)**
 

你所不知道的传输层
题记：23页的文档上，满满当当的写满了笔记，纸质的东西，始终害怕丢失，还是选择把它总结到博客上来。

PS.老规矩，列出可能遇到的20个问题，如果您是都能回答的高手，请您绕道，我是小菜，只做自己的学习笔记。

 

1. 传输层的主要功能是什么？
2. 传输层如何区分不同应用程序的数据流？
3. 传输层有哪些协议？
4. 什么是UDP协议？
5. 为什么有了UDP，还需要TCP？
6. 什么是TCP协议？
7. 怎么理解协议和程序？
8. TCP是否真的有链接？
9. 链接是如何建立的（逻辑上）？
10. 所谓的建立TCP链接开销很大，具体是指什么？
11. 三次握手的目的是什么？
12. TCP如何提供可靠性？
13. 什么是预期确认？什么是肯定确认与重新传输？哪些情况会重传？
14. TCP中，序列号和应答号有哪些作用？
15. TCP链接中，网络失败，是怎么判断的？
16. 为什么需要窗口技术？
17. 如何实现流量控制？
18. UDP的开销很小，具体是指什么？
19. UDP数据包、TCP数据包大小如何确认？
20. UDP适合哪些环境？TCP适合哪些环境？

 

**** 

**一。传输层的主要功能是什么？**

 分割并重新组装上层提供的数据流，为数据流提供端到端的传输服务。

 

**二。传输层如何区分不同应用程序的数据流？**

**** 

因为，对应传输层而言，它只需要知道目标主机上的哪个服务程序来响应这个程序，而不需要知道这个服务程序是干什么的。因此，我们只需要能够抽象的表示出来这些应用程序和服务程序即可。我们使用端口号来抽象标识每个网络程序。

 
**传输层的TCP和UDP可以接收来自多个应用程序的数据流，用端口号标识他们，然后把他们送给Internet层处理；**

**同时TCP和UDP接收来自Internet层的数据包，用端口号区分他们，然后交给不同的应用程序。**

![]()
 
因此：在同一IP地址（同一个目标主机）上不同的端口号是两个不同的链接。IP地址和端口号用来唯一的确定网络上数据的目的地。

 

**三。传输层有哪些协议？**

****传输层的两大协议：TCP（传输控制协议）UDP（用户数据包协议）
**TCP是一个可靠的面向链接的协议，UDP是不可靠的或者说无连接的协议。**
可以用打电话和发短信来说明这种关系：

UDP就好似发短信，只管发出去，至于对方是不是空号（网络不可到达）能不能收到（丢包）等并不关心。

TCP好像打电话，双方要通话，首先，要确定对方不是开机（网络可以到达），然后要确定是不是没有信号（），然后还需要对方接听（通信链接）。

**** 

**四。什么是UDP协议？**

****
UDP数据包结构如下图所示

 
源端口(16)
 
目标端口(16) 报文长度(16)
 
校验和(16) 数据（可变）

UDP为应用程序提供的是一种不可靠的、无连接的分组交付，因此，UDP报文可能会出现丢失、乱序、重复、延时等问题。
因为它不提供可靠性，它的开销很小。（**开销很小具体指什么？**下文揭秘）

**** 

**五。为什么有了UDP，还需要TCP？**

问题4中已经说到，UDP为应用程序提供的是一种无连接、不可靠的分组交付。当网络硬件失效或者负担太重时，数据包可能就会产生丢失、重复、延时、乱序的现象。这些都会导致我们的通信不正常。如果让应用程序来担负差错控制的工作，无疑将给程序员带来许多复杂的工作，于是，我们使用独立的通信协议来保证通信的可靠性是非常必要的。

**** 

**六。什么是TCP协议？**

 传输控制协议TCP是一个面向链接的、可靠的通信协议。

1. 在开始传输前，需要进行三次握手建立链接
2. 可靠性：在传输过程中，通信双方的协议模块继续进行通信
3. 通信结束后，通信双方都会使用改进的三次握手来关闭链接

TCP数据包结构如下图

 
源端口(16)
 
目标端口(16) 序号(32) 应答号(32) 头长度(4)
 
保留(6)
 
编码位(6)
 
窗口(16) 校验和(16)
 
紧急(16) 可选项(如果有，0或32) 数据(可变)

 

 

****七。怎么理解协议和程序？****

如同我们自定义的应用层协议一样：**协议只是给出了一组规范，规定我们应该怎么样（按什么规则）保存数据。**

在计算机间传输的永远都是二进制字节码（对于传输层，可以理解为传输的始终是下层的IP数据包），**是****计算机中的程序通过对这些字节码进行逻辑分析、判断，来控制程序完成差错控制等功能。**
至于解析这些字节码的程序，则可以有不同的实现，只要我们按照规则来解析，并作出相应的控制，我们大可以自己写个程序是实现相应功能。

 
知道了这些后，显然，我们也可以使用前面说的Jpcap，来自己实现一个基于Java的TCP或者UDP协议。可以参考Linux下的Tcp源码。

/net/ipv4/udp.c
/net/ipv4/datagram.c
/net/ipv4/tcp_input.c
/net/ipv4//tcp_output.c
/net/ipv4/tcp.c  

 

**八。TCP是否真的有链接？**

我们都知道，TCP通过完成三次握手来建立链接的，但是这种连接是面向虚电路的，是物理上不存在的，**只是双方的TCP程序，逻辑上的认为建立了这样的链接****。 **

**** 

**九。链接是如何建立的（逻辑上）？**

假设：当我们在主机A上启动一个程序，通过TCP去链接主机B上的9091端口。

![]()
 整个过程是怎么样的呢？逻辑上我们可以这么理解建立链接的过程：

  

1.SYN:seq=X;

1.1 A的TCP程序，为这个链接分配一个端口（设为9090）。
1.2 同时**逻辑上**的将TCP连接的状态设置为：正在连接。（通过在链接状态表中添加一条记录，记录中状态为：正在连接）

猜想：

TCP程序中， 应该有张表来保持链接的状态，其中每个状态应该有：

本机地址（IP加port）、对方地址、链接状态

1.3 同时，随机生成一个初始序列号X，生成一个TCP包，将初始化序列号X设置为TCP中的序列号，发送给主机B。

 

 

**2.SYN:seq=Y ACK:ack=X+1;**

2.1 B上TCP程序收到该数据包，查询9091端口状态，如果可以链接。
2.2 同样的，在逻辑上的将TCP连接的状态设置为：正在连接
2.3 同时，随机生成一个初始化序列号Y，根据接收的序列号X，生成应答号X+1，生成一个TCP包，将序列号和应答号分别设置到TCP包头中，将TCP数据包发给主机A。

 

**3.SYN:seq=X+1 ACK:ack=Y+1.**

3.1  A上的TCP程序接收到数据包，查询9090端口状态。
3.2 根据收到的SYN:seq=Y;ACK:ack=X+1; 封装一个TCP包 SYN:seq=x+1;ACK:ack=Y+1;发送给主机B。同时，TCP程序将链接状态表中该条记录状态设置为已连接。
3.3 主机B收到数据包，TCP程序将链接状态表中该条记录状态设置为已连接。

 

至此，一个TCP链接建立（三次握手）完成。
我们可以看到：
第一：传送的都是IP数据包，其实只是将收到的数据包交给TCP程序处理。
第二：**链接状态，只是TCP程序中的一个逻辑状态。**

 

**十：所谓的建立TCP链接开销很大，具体是指什么？**

从九中，很容易看出。要简历TCP链接，必须进行三次IP数据包的成功传输。

 

**十一：三次握手的目的是什么？**

TCP是面向链接的，在面向链接的环境中，开始传输数据之前，在两个中端之间必须先建立一个链接。建立链接的过程可以确保通信双方在发送应用程序数据包之前，都已经准备好了传送和接收数据。并且使通信双方统一了初始化序列号。

 

十二：TCP如何提供可靠性？

**在传输过程中，通信双方的协议模块继续进行通信，从而确保了传输的可靠性。**
**针对乱序：**在通过三次握手进行链接时，序列号被初始化。在传输过程中，TCP继续使用这个序列号来标记发送的每一个数据段，没传送一个数据段，序列号加一。接收方依据序列号重装收到的数据段。
**针对丢包**：在传输过程中，接收方收到一个数据段后，会用ACK应答码向发送端回复一个IP包进行应答，确认号ACK用来告诉发送端哪些数据包已经成功接收，发送方对未被应答的报文段提供重传。
**针对重复**：接收端收到数据段后，查看序列号，如果已经成功接收改数据包，则丢弃后面这个数据段。
**针对延时**：延时造成的第一个问题，就是数据包达到接收端时乱序。
当延时严重时，接收端一直未收到数据段，则不会回复ACK，发送端认为丢包，重发。

 

**十三：什么是预期确认？什么是肯定确认与重新传输？哪些情况会重传？**

1.确认号ACK会告诉发送端哪些数据段已经成功接收，并且确认号会向发送端指出接收端希望收到的下一个序列号。即，确实号ACK为上个数据序列号+1，这种机制称为**预期确认**。

2.为了提高效率，我们在发送端，将数据段保存在缓冲区中，直道发送端收到来自接收端的确认号。这种机制被称为“**肯定确认与重新传输**”。

3.当**发送端在给定时间间隔内收不到那个数据段的应答时，发送端就会重传那个数据段****。**
情况1：网络延时/环路，数据段丢失
情况2：网络延时，数据段推迟到达
情况3：数据段成功到达，应答因为1.2不能达到。

 

 

 

**十四： TCP中，序列号和应答号有哪些作用？**

从以上10,11,12中，很明显的可以看到

1.  

1.  

1. 依靠序列号重组数据段
1. 依靠数据包消除网络中的重复包
1. 依靠序列号和应答号进行差错重传，提高了TCP的可靠性

**十六：为什么需要窗口技术？**

前面我们已经说了，TCP的可靠性，是通过预期确认来实现的。即发送方发送一个数据段后，需要得到对方的确认后，才会发送下一个数据段。
因此，假设一个数据段大小为64KB（IP包最大值），一次发送和确认需要的时间为500MS，则，1S内，只能传送128KB的数据，如果带宽为1M，显然很浪费带宽。为了充分利用带宽，我们使用窗口技术。滑动窗口允许发送方在收到接收方的确认之前发送多个数据段。（窗口大小决定了在收到确认前可以发送的数据段数量）

 

**十七：如何实现流量控制？**

**窗口数决定了当前传输的最大流量**。当我们在传输过程中，通信双方可以根据网络条件动态协商窗口大小，调整窗口大小时，即可实现流量控制。（在TCP的每个确认中，除了ACK外，还包括一个窗口通知）

 

**十八：UDP的开销很小，具体是指什么？**

1.因为UDP是无连接的。在传输数据之前，不需要进行复杂的三次握手来建立连接。
2.在传输数据时，没有协议间通信流量（确认信号），也不需要浪费不必要的处理时间（接收确认信号再发一下）。
3；传输结束后，也不用再用改进的三次握手来端口连接。

 

**十九：UDP数据包、TCP数据包大小如何确认？**

1.  

1. 无论TCP还是UDP数据包，都需要交给Internet层封装为IP包，而一个IP包，包头中的长度位为16位，所以IP包最大为2的16方，即**65535**（64KB还需要减去各种包头长度）。
1. TCP因为面向流，且可以凭借序列号对大文件进行分段和重组，因此，**TCP可以用来传输较大的文件**。而UDP，如果要传输大于64KB的数据，则需要自己在应用层进行差错控制。
1. 为了提高传输效率和减少网络通信量（协议间的通信），TCP也会一次传输足够多的数据。
1. 因为MTU的存在，TCP包和UDP包不是越大越好。（在路由中分包，在接收端重组，加大路由与接收端负担，增大丢包概率。分组丢失，整个数据包重传。）

**二十：UDP适用哪些环境？TCP适用哪些环境？**

适合UDP的环境：
1.在**高效可靠的网络**环境中（不需要考虑网络不好导致的丢包、乱序、延时、重复等问题），因为UDP是无连接的服务，不用消耗不必要的网络资源（TCP中的协议间通信）和处理时间（预期确认需要的时间），从而效率要高的多。
2.在**轻权通信**中，当需要传输的数据量很小（可以装在一个IP数据包内）时。如果我们使用TCP协议，那么，先建立连接，一共需要发送3个IP数据包，然后数据传输，1个IP数据包，产生一个确认信号的IP包，然后关闭连接，需要传输5个IP数据包。使用TCP协议IP包的利用率为1/10。而使用UDP，只需要发送一个IP数据包。哪怕丢包（服务不成功），也可重新申请服务（重传）。
注：而且无论UDP还是TCP，传输的都是IP数据包。当网络环境不好导致丢包时，无论TCP还是UDP都会丢包，这是没有区别的。（如果考虑发送丢包，那么TCP效率更低），只是使用TCP，当连接建立成功后，TCP程序会进行可靠性控制。
**UDP很适合这种客户机向服务器传送简单服务请求的环境**。此类应用层协议包括TFTP , SNMP , DNS ,DHCP等。
3.在**对实时性要求很强**的通信中：在诸如实时视频直播等对实时性要求很高的环境中，从而允许一定量的丢包的情况下（直播比赛，前面丢失的包，重传出来已经意义不大了），UDP更适合。（可以根据具体需要通过应用层协议提供可靠性，不用像TCP那么严格。）

 

适合TCP协议的环境：

当网络硬件失效或者负担太重时，数据包可能就会产生丢失、重复、延时、乱序的现象。这些都会导致**我们的通信不正常的时候**。如果让应用程序来担负差错控制的工作，无疑将给程序员带来许多复杂的工作，于是，我们使用独立的通信协议来保证通信的可靠性是非常必要的。 

 

 

 
**PS.写博客真累。。**

**特别是根据一堆读书笔记写博客。。**

**特别是写4-5000字的总结博客。。**

**累的感觉瘫痪了两次。。唉。。伤不起啊。。**

* [![]( "点击查看原始大小图片")]()
* 大小: 11.3 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 11.7 KB

* [查看图片附件](http://java-mzd.iteye.com/blog/1007577#)
[**141**
顶](http://java-mzd.iteye.com/blog/1007577#)[**25**
踩](http://java-mzd.iteye.com/blog/1007577#)

[TCP/IP网络层谜云](http://java-mzd.iteye.com/blog/1019088 "TCP/IP网络层谜云") | [Jpcap简易教程](http://java-mzd.iteye.com/blog/1005945 "Jpcap简易教程")
* 02:04
* 浏览 (11300)
* [评论](http://java-mzd.iteye.com/blog/1007577#comments) (53)
* 分类: [TCP/IP](http://java-mzd.iteye.com/category/153171)
* [相关推荐](http://www.iteye.com/wiki/topic/1007577)

### 评论

[]()

53 楼 [lucane](http://lucane.iteye.com/) 2011-05-14   [引用](http://java-mzd.iteye.com/blog/1007577#)

十：所谓的建立TCP链接开销很大，具体是指什么？
从九中，很容易看出。要"简历"TCP链接，必须进行三次IP数据包的成功传输。
52 楼 [xsz](http://xsz.iteye.com/) 2011-05-14   [引用](http://java-mzd.iteye.com/blog/1007577#)

顶顶楼主。

51 楼 [wgh596](http://wgh596.iteye.com/) 2011-05-14   [引用](http://java-mzd.iteye.com/blog/1007577#)

![]() 辛苦，辛苦。
50 楼 [hbice](http://hbice.iteye.com/) 2011-05-14   [引用](http://java-mzd.iteye.com/blog/1007577#)

获益匪浅，lz辛苦，我也辛苦了，顶着牙疼看帖子。。。赶紧去看牙医了。。。

49 楼 [Lagunarock](http://lagunarock.iteye.com/) 2011-05-14   [引用](http://java-mzd.iteye.com/blog/1007577#)

非常不错
涵盖面广，排版又好
值得学习
48 楼 [adeline.pan](http://adeline-pan.iteye.com/) 2011-05-13   [引用](http://java-mzd.iteye.com/blog/1007577#)

写得很好，上学时候学的差不多都忘了，看了又都想起来了，收藏

47 楼 [qq376727939](http://qq376727939.iteye.com/) 2011-05-12   [引用](http://java-mzd.iteye.com/blog/1007577#)

好贴啊,dou zhu
46 楼 [gubingo](http://vcloud.iteye.com/) 2011-05-12   [引用](http://java-mzd.iteye.com/blog/1007577#)

议和程序？**

greatwqs 写道
![]() 总结的很好!

45 楼 [greatwqs](http://greatwqs.iteye.com/) 2011-05-10   [引用](http://java-mzd.iteye.com/blog/1007577#)

![]() 总结的很好!
44 楼 [嘻哈方式](http://794217655-qq-com.iteye.com/) 2011-05-10   [引用](http://java-mzd.iteye.com/blog/1007577#)

这方面一直都是我的薄弱项 看后我的功力又增强啦！ 多谢楼主！！！

43 楼 [steafler](http://steafler.iteye.com/) 2011-05-08   [引用](http://java-mzd.iteye.com/blog/1007577#)

IP是网络层协议
42 楼 [wuhuajun](http://wuhuajun.iteye.com/) 2011-05-07   [引用](http://java-mzd.iteye.com/blog/1007577#)

好![]()

41 楼 [pan_java](http://pan-java.iteye.com/) 2011-05-05   [引用](http://java-mzd.iteye.com/blog/1007577#)

顶楼主，不错！
40 楼 [liuyes](http://liuyes.iteye.com/) 2011-05-05   [引用](http://java-mzd.iteye.com/blog/1007577#)

写得很好哈

39 楼 [beiyangshuishi](http://beiyangshuishi.iteye.com/) 2011-05-03   [引用](http://java-mzd.iteye.com/blog/1007577#)

写的不错，增长知识了。以前我对网络知识一窍不通。
38 楼 [gogopengyou](http://gogopengyou.iteye.com/) 2011-05-03   [引用](http://java-mzd.iteye.com/blog/1007577#)

基本都看明白了，总结的挺好的

37 楼 [小猪笨笨](http://housonglin1122-163-com.iteye.com/) 2011-04-28   [引用](http://java-mzd.iteye.com/blog/1007577#)

哈哈，谢谢了，其实我对这些东西一直一知半解，看了你这篇文有很大收获
36 楼 [torry_1979](http://torry-1979.iteye.com/) 2011-04-27   [引用](http://java-mzd.iteye.com/blog/1007577#)

作为一个普及性的资料还不错

35 楼 [sswh](http://sswh.iteye.com/) 2011-04-27   [引用](http://java-mzd.iteye.com/blog/1007577#)

mark
谢谢分享。
34 楼 [20444465](http://yanglijun888.iteye.com/) 2011-04-26   [引用](http://java-mzd.iteye.com/blog/1007577#)

做it的一家要懂

« 上一页 1 [2](http://java-mzd.iteye.com/blog/1007577?page=2#comments) [3](http://java-mzd.iteye.com/blog/1007577?page=3#comments) [下一页 »](http://java-mzd.iteye.com/blog/1007577?page=2#comments)
### 发表评论

### 表情图标

![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()

字体颜色: 标准深红红色橙色棕色黄色绿色橄榄青色蓝色深蓝靛蓝紫色灰色白色黑色 字体大小: 标准1 (xx-small)2 (x-small)3 (small)4 (medium)5 (large)6 (x-large)7 (xx-large) 对齐: 标准居左居中居右

提示：选择您需要装饰的文字, 按上列按钮即可添加上相应的标签

您还没有登录，请[登录](http://java-mzd.iteye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![java_mzd的博客]( "java_mzd的博客: ")](http://java-mzd.iteye.com/)

java_mzd

* 浏览: 70746 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 长沙
* ![]()
* [详细资料](http://java-mzd.iteye.com/blog/profile) [留言簿](http://java-mzd.iteye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://java-mzd.iteye.com/blog/user_visits)

[![zhxing的博客]( "zhxing的博客: ヾ孤星随缘ツ  http://t.sina.com.cn/samzhxing")](http://zhxing.iteye.com/)

[zhxing](http://zhxing.iteye.com/)

[![java_suddy的博客]( "java_suddy的博客: “平凡”的思想")](http://java-suddy.iteye.com/)

[java_suddy](http://java-suddy.iteye.com/)
[![liuxinglanyue的博客]( "liuxinglanyue的博客: liuxinglanyue")](http://liuxinglanyue.iteye.com/)

[liuxinglanyue](http://liuxinglanyue.iteye.com/)

[![libo_591的博客]( "libo_591的博客: 让更多的人站在巨人的肩膀上")](http://libo-591.iteye.com/)

[libo_591](http://libo-591.iteye.com/)

### 博客分类

* [全部博客 (43)](http://java-mzd.iteye.com/)
* [数据结构----------JAVA类集 (5)](http://java-mzd.iteye.com/category/133623)
* [TCP/IP (7)](http://java-mzd.iteye.com/category/153171)
### 我的留言簿 [>>更多留言](http://java-mzd.iteye.com/blog/guest_book)

* 楼主 你工作多久了。。。。
-- by [fanmingxing](http://java-mzd.iteye.com/blog/guest_book#39913)
* 写的不错，学习，谢谢！
-- by [chenge2k](http://java-mzd.iteye.com/blog/guest_book#39693)
* 看了LZ的文章，发现工作快两年的我，就像是一块浮起来的木头，真的很惭愧！也不怪我拿 ...
-- by [GoTiger](http://java-mzd.iteye.com/blog/guest_book#39618)

### 其他分类

* [我的收藏](http://java-mzd.iteye.com/blog/favorite) (23)
* [我的代码](http://java-mzd.iteye.com/blog/code_favorite) (0)
* [我的论坛主题帖](http://java-mzd.iteye.com/blog/topic) (3)
* [我的所有论坛帖](http://java-mzd.iteye.com/blog/post) (37)
* [我的精华良好帖](http://java-mzd.iteye.com/blog/article) (0)
### 最近加入群组

* [Android](http://android.group.iteye.com/)

### 存档

* [2011-05](http://java-mzd.iteye.com/blog/monthblog/2011-05) (2)
* [2011-04](http://java-mzd.iteye.com/blog/monthblog/2011-04) (7)
* [2011-03](http://java-mzd.iteye.com/blog/monthblog/2011-03) (2)
* [更多存档...](http://java-mzd.iteye.com/blog/monthblog_more)
### 评论排行榜

* [腾讯、淘宝、金山网络，实习生我该何去何从](http://java-mzd.iteye.com/blog/1050043 "腾讯、淘宝、金山网络，实习生我该何去何从")
* [淘宝、金山网络，百感交集](http://java-mzd.iteye.com/blog/1050926 "淘宝、金山网络，百感交集")
* [TCP/IP传输层，你懂多少？]( "TCP/IP传输层，你懂多少？")
* [淘宝武汉*面试归来](http://java-mzd.iteye.com/blog/1004784 "淘宝武汉*面试归来")
* [开源软件？自由软件？免费软件？你了解多少 ...](http://java-mzd.iteye.com/blog/862787 "开源软件？自由软件？免费软件？你了解多少？")

* [![Rss]()](http://java-mzd.iteye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://java-mzd.iteye.com/rss)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 ]
![]()
