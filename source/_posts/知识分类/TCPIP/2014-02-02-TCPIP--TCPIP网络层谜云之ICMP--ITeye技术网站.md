---
layout: post
title: "TCP IP网络层谜云之ICMP - - ITeye技术网站"
categories: TCPIP
tags: 
 - TCPIP
--- 

# TCP IP网络层谜云之ICMP - - ITeye技术网站

[首页](http://www.iteye.com/) [新闻](http://www.iteye.com/news) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [招聘](http://www.iteye.com/job) [更多 ▼](http://java-mzd.iteye.com/blog/1019089#)

[专栏](http://www.iteye.com/wiki)  [群组](http://www.iteye.com/groups) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://java-mzd.iteye.com/login "登录") [我的应用](http://www.iteye.com/all) [登录](http://java-mzd.iteye.com/login) [注册](http://java-mzd.iteye.com/signup)

# [java-mzd](http://java-mzd.iteye.com/)

永久域名 [http://java-mzd.iteye.com](http://java-mzd.iteye.com/)

[2顶](http://java-mzd.iteye.com/blog/1019089#)
[4踩](http://java-mzd.iteye.com/blog/1019089#)

[腾讯、淘宝、金山网络，实习生我该何去何从](http://java-mzd.iteye.com/blog/1050043 "腾讯、淘宝、金山网络，实习生我该何去何从") | [TCP/IP网络层谜云](http://java-mzd.iteye.com/blog/1019088 "TCP/IP网络层谜云")

2011-04-27

### [TCP/IP网络层谜云之ICMP]()

**文章分类:[Java编程](http://www.iteye.com/blogs/category/java)**
本文上接《[TCP/IP网络层谜云](http://java-mzd.iteye.com/blog/1019088)》 [http://java-mzd.iteye.com/blog/1019088](http://java-mzd.iteye.com/blog/1019088)

**十四。为什么需要ICMP？**

**因为IP协议不提供可靠性且不能保证信息传递，因此发生问题时，通知发送人是很重要的。**（IP协议是一种不可靠的协议，无法进行差错控制。但IP协议可以借助其他协议来实现这一功能，如ICMP）

**十五。什么是ICMP？** 

ICMP： Internet Control Message Protocol 即Internet消息控制协议。

ICMP定义了一套**差错报文**和**控制报文**，用于用户主机与路由之间交换不可到达地址、网络拥塞、重定向到更好的路径、报文生命周期超时等信息。

**ICMP协议是一种提供**（有关阻止数据包传递的）**网络故障问题反馈信息的机制**。（针对阻止数据包传递或者网络故障。）它让TCP等上层协议能够意识到数据包没有送达目的地。
**关于ICMP与TCP的差错控制对比？**

比如主机A到主机B的通信，中间Router  r1 到Router  r2 的网络连接断了。

通过ICMP，则**可以快速诊断出出错原因**，并且报告主机。（差错诊断）

TCP的差错控制，主要是体现在，网络断了，收不到确认回复，TCP会一直再次发送数据包，直到收到确认回复。（差错处理）

 

ICMP提供一组易懂的出错报告信息。发送的出错报文返回到发送原数据的设备，因为**只有发送设备才是出错报文的逻辑接受者**。**发送设备随后可根据ICMP报文确定发生错误的类型，确定如何才能更好地重发失败的数据报**。但是**ICMP唯一的功能是报告问题而不是纠正错误，纠正错误的任务由发送方完成。**

 

**十六。ICMP协议有哪些数据包？**

******一个ICMP数据包实际上就是一个（数据部分为ICMP协议数据的）****IP数据包。** 

IP头
 
ICMP头
Type
 
Code
 
Checksum 
ICMP数据

如前所述，ICMP主要分为**差错报文**和**控制报文**。

**差错报文包括**：目标不可到达（网络、协议、主机、端口不可到达；禁止分割、目标网络不认识、目标主机不认识等等）、超时、参数问题、重定向（网络重定向、主机重定向等）等等

**控制报文包括**：请求回显（ping请求）、回显应答（ping应答）、地址掩码请求、地址掩码应答等等

如上，我们可以发现，同一类型的错误（不可到达）可能有不同种类（网络不可到达、主机不可到达），因此，**我们使用type来code两个标志来确定一个具体的错误。**

**因为需要指出具体是哪个主机上的哪个程序发出的信息没有到达。**

**因此，每一个ICMP的错误消息，应该包含：**

1.具体的错误类型（Type/code 决定）

2.引发ICMP错误消息的数据包的完全IP包头（哪个主机的数据）

3.数据报的前8个字节----UDP报头或者TCP中的port部分(主机上的哪个程序)

因此，**ICMP错误消息的格式应该为如下**：
Type(8)
 
Code(8)
 
Checksum(16) Unused(32) Internet Header +64 Bits of Original Data Datagram

 
控制报文因为控制的消息各不相同，所以有所差异。下面分析下请求回显（ping）和回显应答，其他的消息，大家可以触类旁通。

****
Type
 
Code
 
Checksum Identifier
 
Sequence Number Data

**当主机****A****需要知道和主机****B****通信的状况****(****信息传递延时、丢包率****)****时，我们该怎么办呢？******
我们可以参考声纳和雷达的原理：主机A发送一个ICMP回显请求（type=8,code=0）报文,数据域中存放当前时间T1，目的IP为主机B。主机B收到该ICMP回显请求报文后，将目的IP和源IP调换位置，其他信息都不变（Indentifier，Sequence Number）,回复一个ICMP回显应答（type=0,code=0）。主机A收到改ICMP回显应答的时间为T2。则，到主机B的通信时间为：T2-T1。

又因为，要考虑丢包，所以我们向主机B发送多个回显请求，用Sequence Number来区分各个请求，根据Sequence Number，即可知道应答对应的请求数据包。

 

Ping命令就是这样的一个实现，其实我们在命令行下输入ping ip命令时，就是调用Ping程序。Ping程序根据输入的IP（域名）封装ICMP请求应答，发送出去，并且接受ICMP回显应答，进行解析，输出。

 

**关于超时**：IP报头中的生存期（TTL）属性，用来控制报文段在网络中的生存期。

 

试想一下，当网络中的某些路由出现问题，Router A将IP a 的下一跳路由为Router B。同时在Router B中，又将IP a 的下一跳路由为 Router A。那么显然，两个路由器之将会间形成回路，通往网段a的数据包，将会一直在两个路由之间发送，导致网络流量爆炸，同时，数据包也无法正确的到达网络a。

 

因此，当IP数据包每经过一个路由器时，路由器将IP数据包中的TTL值减一。当TTL值为0时，路由器判断数据包超时，发送ICMP超时信息给源主机。

 

**当我们想知道：从主机****A****发送到主机****B****的数据包在网络中都经过了哪些路由器的时候，我们有什么办法呢？******
我们知道，当IP数据包在路由中出错时，路由器会向发送源发送一个ICMP错误报文，发送端从该ICMP错误报文中，可以得到该路由的IP。

我们可以利用此原理。要得到从主机A到目标主机B之间的所有路由的IP，那么我们必须让IP数据包在每个路由器中都出错一次。

又因此，TTL值在经过的每个路由器中都会减1。因此，我们可以利用TTL的超时信息，在每个路由中都发生一次。即可得到从从主机A到主机B之间的所有路由的IP。

 

PS.怎么判断数据包正确到达了目标主机B？

当IP数据包到达了目标主机，将不会再发送TTL超时错误。而且在目标主机B中，没有运行相对应的应用层程序，因此，将没有程序会回应我们发送的IP数据包。那么，我们将如何知道IP数据包已经到达了目标主机呢？

我们可以为数据包分配一个目标主机几乎不可能监听的端口，从而，当IP数据包到达目标主机后，目标主机会回复相应的”不可到达、端口不可到达”的ICMP错误信息，从而，我们可以确认IP数据包已经到达了目标主机。

 

综上，我们可以：源主机A发送IP数据包，IP为目标主机B，port几乎不可能监听的port，TTL从一开始一直往上增加，直道收到来自主机B的ICMP 不可到达（端口不可到达）信息。

Tracerouter 命令就是实现这样功能的一个程序。我们可以通过tracerouter ip来调用此功能。

 

PS.因为每次路由时的路由路径可能不一样，那么在tracerouter过程中，我们又如何解决这个问题？

而且，如果目标主机正好监听了这一我们认为不可能监听的端口呢？

针对这些问题，欢迎各位读者回答。

 

PS。这是第四次更改了。。希望JE别抽风了。。让他们正常显示吧。。

这年头，每个用心写总结的人。。你们都伤不起啊。。。

好不容易写了一周啊。。

从2000写到8000啊。。又从8000改到4000啊。。又从4000改到6600啊

写了改

改了啊。。

伤不起啊。。

好不容易写好了。。

JE还不给力。。显示不正常啊。。

又重新写了后面两个大问题好几遍啊。。好几遍啊

伤不起啊。。。

[**2**
顶](http://java-mzd.iteye.com/blog/1019089#)[**4**
踩](http://java-mzd.iteye.com/blog/1019089#)
[腾讯、淘宝、金山网络，实习生我该何去何从](http://java-mzd.iteye.com/blog/1050043 "腾讯、淘宝、金山网络，实习生我该何去何从") | [TCP/IP网络层谜云](http://java-mzd.iteye.com/blog/1019088 "TCP/IP网络层谜云")

* 00:39
* 浏览 (277)
* [评论](http://java-mzd.iteye.com/blog/1019089#comments) (2)
* 分类: [TCP/IP](http://java-mzd.iteye.com/category/153171)
* [相关推荐](http://www.iteye.com/wiki/topic/1019089)
### 评论

[]()

2 楼 [leelege](http://leelege.iteye.com/) 2011-05-04   [引用](http://java-mzd.iteye.com/blog/1019089#)

感谢楼主分享 期待更多内容～～
1 楼 [cosina](http://cosina.iteye.com/) 2011-04-28   [引用](http://java-mzd.iteye.com/blog/1019089#)

![]()   学倒不少！ 继续写哈哈

### 发表评论

### 表情图标

![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()

字体颜色: 标准深红红色橙色棕色黄色绿色橄榄青色蓝色深蓝靛蓝紫色灰色白色黑色 字体大小: 标准1 (xx-small)2 (x-small)3 (small)4 (medium)5 (large)6 (x-large)7 (xx-large) 对齐: 标准居左居中居右

提示：选择您需要装饰的文字, 按上列按钮即可添加上相应的标签

您还没有登录，请[登录](http://java-mzd.iteye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![java_mzd的博客]( "java_mzd的博客: ")](http://java-mzd.iteye.com/)

java_mzd

* 浏览: 70753 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 长沙
* ![]()
* [详细资料](http://java-mzd.iteye.com/blog/profile) [留言簿](http://java-mzd.iteye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://java-mzd.iteye.com/blog/user_visits)

[![aria的博客]( "aria的博客: ")](http://aria.iteye.com/)

[aria](http://aria.iteye.com/)

[![zhxing的博客]( "zhxing的博客: ヾ孤星随缘ツ  http://t.sina.com.cn/samzhxing")](http://zhxing.iteye.com/)

[zhxing](http://zhxing.iteye.com/)
[![java_suddy的博客]( "java_suddy的博客: “平凡”的思想")](http://java-suddy.iteye.com/)

[java_suddy](http://java-suddy.iteye.com/)

[![liuxinglanyue的博客]( "liuxinglanyue的博客: liuxinglanyue")](http://liuxinglanyue.iteye.com/)

[liuxinglanyue](http://liuxinglanyue.iteye.com/)

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
* [TCP/IP传输层，你懂多少？](http://java-mzd.iteye.com/blog/1007577 "TCP/IP传输层，你懂多少？")
* [淘宝武汉*面试归来](http://java-mzd.iteye.com/blog/1004784 "淘宝武汉*面试归来")
* [开源软件？自由软件？免费软件？你了解多少 ...](http://java-mzd.iteye.com/blog/862787 "开源软件？自由软件？免费软件？你了解多少？")

* [![Rss]()](http://java-mzd.iteye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://java-mzd.iteye.com/rss)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 ]
![]()
