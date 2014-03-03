---
layout: post
title: "利用SNMP实现remote ping"
categories: 安全
tags: 
 - 安全
 - 绿盟
--- 

# 利用SNMP实现remote ping

[]() [![Logo of NSFOCUS]()](http://www.nsfocus.com/) [![English Version]()](http://www.nsfocus.com/en/index.html)  [![Chinese Version]()](http://www.nsfocus.com/index.html)  [![Japanese Version]()](http://www.nsfocus.com/jp/index.html) [![]()](http://weibo.com/nsfocus) ![]() [![]()](http://www.nsfocus.com/) [![产品与解决方案]()](http://www.nsfocus.com/1_solution/) [![专业服务]()](http://www.nsfocus.com/2_services/) [![客户支持]()](http://support.nsfocus.com/) [![安全研究]()](http://www.nsfocus.net/) [![工作机会]()](http://www.nsfocus.com/5_careers/) [![关于我们]()](http://www.nsfocus.com/6_about/) ![]()   ![]()  [安全漏洞](http://www.nsfocus.net/index.php?act=sec_bug)    [业界动态](http://www.nsfocus.net/index.php?act=sec_news)    [紧急通告](http://www.nsfocus.net/index.php?act=alert)    [研究成果](http://www.nsfocus.net/index.php?act=advisory)    [研究机构](http://www.nsfocus.com/4_research/4_5.html)  [安全视角](http://www.nsfocus.com/4_research/4_6.html)  ![]() ![]()   绿盟安全月刊->[第56期](http://www.nsfocus.net/index.php?act=magazine&do=one&periodical=56)->[技术专题](http://www.nsfocus.net/index.php?act=magazine&do=search&periodical=56&kind_id=4) 期刊号：  --全部--第57期第56期第55期第54期第53期第52期第51期第50期第49期第48期第47期第46期第45期第44期第43期第42期第41期第40期第39期第38期第37期第36期第35期第34期第33期第32期第31期第30期第29期第28期第27期第26期第25期第24期第23期第22期第21期第20期第19期第18期第17期第16期第15期第14期第13期第12期第11期第10期第9期第8期第7期第6期第5期第4期第3期第2期第1期  类型：  --全部--业界动态安全新闻最新漏洞技术专题工具介绍安全文摘专题报道安全资源  关键词：  **利用snmp实现remote ping**
作者：Yiming Gong
出处：http://www.nsfocus.com
主页：http://security.zz.ha.cn
日期：2005-02-04
声明:摘抄请保留上述作者和http地址
CNNOG1上我的ppt稿子中第18页的snmp部分引来了几封邮件，问该页的内容是什么意思，一次次的回答比较麻烦，干脆写个小东西说明一下。
先把ppt中该页内容引用如下（有细微改动）：
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.16.333 integer 6 /*ciscoPingEntryStatus
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.16.333 integer 5
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.15.333 s "yiming"
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.2.333 integer 1 / *ciscoPingProtocol
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.3.333 x "DB 96 20 C5"
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.6.333 integer 1000 /*ciscoPingPacketTimeout
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.4.333 integer 10 /*"ciscoPingPacketCount
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.5.333 integer 111 /*ciscoPingPacketSize
? snmpset -v 1 -c 100wxs9dj 219.150.128.1 .1.3.6.1.4.1.9.9.16.1.1.1.16.333 i 1
? snmpwalk -v 1 -c private 192.168.1.1 1.3.6.1.4.1.9.9.16.1.1.1.12.333  /*ciscoPingAvgRtt
上述snmp命令实际上最终实现的是remote ping功能，ping我们都很熟悉，但什么是remote ping?
在拥有自己网络的企业内，尤其是大型的ISP，网络维护的工作中一般都有一项固定任务：“测算几台路由器之间时延和丢包率”。通常是每天定时登录到路由器，在路由器而非管理终端（此即remote）上面执行ping（或扩展ping）命令测量到其它router或指定地址的时延和丢包并将结果记录，进行后期的各种分析比较工作。
为完成上述需求，如果不使用这个ppt里面谈到的方法，有3种选择：
*一种是笨办法，管理员每天定时从管理终端挨个telnet/ssh到路由器上，手工执行ping（或扩展ping）命令，并copy回结果，这个法子比较麻烦。
*方法2是通过expect之类的东西来写脚本，并设置cron等代替管理员定时登录路由器执行命令并取回数据，这个法子也不错，就是对管理员的编程有一些要求。
*方法3最简单，就是买商用的hpopenview之类支持remote ping的工具，不过感觉这个有点夸张。
实际上除了这三种方法以外，对于国内广泛使用的cisco的设备来说，还有不花钱又简单的方法，就是利用它自己的ping mib。
除了这个MIB外，我们还需要snmp的工具，当然可以选择商用的solarwind之类的东西，但是对于看此文的读者来说开源的NETSNMP应该是方便自己定制的不二选择。
然后就是snmp 通讯字串了，本例子里面以private表示。
最后是router的ip,本例子中以192.168.1.1表示。
打个茬，我之所以突然对这个remote ping感兴趣，就是因为前一段一个电信的朋友问有没有HP openview，他需要里面的rping模块来对他们省内的routers做remote ping，我手头自然是不会有这个了，但是感觉不至于非要商用的产品才能搞这个事情吧。好奇之下就在cisco的网站狂扒了一阵，先是找到了一篇cisco的Ping MIB Implementation文档，可惜如法炮制后发现不好使，后来发现是cisco的文档写错了，这是后话不提。不过这个文档中提到了cisco自己的私有mib, CISCO-PING-MIB-V1SMI  (参见2)，知道了需要用到这个mib，这个任务就简单了（p.s. SNMP的相关概念请参阅其他文档）
首先要仔细查看CISCO-PING-MIB-V1SMI这个mib文件，搞清楚mib中每个分支的意思，其次为了方便使用命令时快速查找，我们还需要找到对应oid表 (参见3)
在CISCO-PING-MIB-V1SMI.my的开头部分我们看到如下内容：
-- snip --    
        
        A management station wishing to create an entry should
        first generate a pseudo-random serial number to be used
        as the index to this sparse table.  The station should
        then create the associated instance of the row status
        and row owner objects.  It must also, either in the same
        or in successive PDUs, create the associated instance of
        the protocol and address objects.  It should also modify
        the default values for the other configuration objects
        if the defaults are not appropriate.
        
-- end snip --  
读懂了这个mib文件，剩下的工作就简单得很了，我们按照对上面ppt的内容逐行解释一下大家立刻就能明白：
首先ciscoPingEntry的oid是1.3.6.1.4.1.9.9.16.1.1.1，如上面所引用内容，我们要先给它建立一个实例，这里随便选一个，333
1.    "ciscoPingEntryStatus"    "1.3.6.1.4.1.9.9.16.1.1.1.16"
第一步需要设置ping entry的状态，在mib文件中ciscoPingEntryStatus描述如下
--snip--
ciscoPingEntryStatus OBJECT-TYPE
    SYNTAX RowStatus
--    Rsyntax INTEGER {
--        active(1),
--        notInService(2),
--        notReady(3),
--        createAndGo(4),
--        createAndWait(5),
--        destroy(6)
--        }
-- end snip --  
    为了保险起见，我们先destroy(6)以前可能有的实例，这就是ppt中的第一行：
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.16.333 integer 6
2.    然后我们要create这个实例，并等待后面的动作，设置 createAndWait(5), 这就是ppt中的第二行
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.16.333 integer 5
3.    设置一个ciscoPingEntryOwner，这就是ppt中的第三行：
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.15.333 s "yiming"
4.    设置使用IP协议来执行Ping动作，这就是ppt中的第四行：
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.2.333 integer 1
5.    设置ping的destination IP地址，注意目的IP是16进制表示，这就是ppt中的第五行：
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.3.333 x "DB 96 20 C5"
6.    设置超时时间，ppt中的第6行
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.6.333 integer 1000
7.    设置要发出的ping packets数目，ppt中的第7行
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.4.333 integer 10
8.    设置ping包的大小，ppt中的第8行
? snmpset -v 1 -c private 192.168.1.1 .1.3.6.1.4.1.9.9.16.1.1.1.5.333 integer 111
9.    将PingEntryStatus设置为1，激活开始执行命令动作，ppt中的第9行
? snmpset -v 1 -c 100wxs9dj 219.150.128.1 .1.3.6.1.4.1.9.9.16.1.1.1.16.333 i 1
10.    取结果的平均RTT值，ppt中的第10行
? snmpwalk -v 1 -c private 192.168.1.1 1.3.6.1.4.1.9.9.16.1.1.1.12.333
1到9的输出结果不必要copy出来了，这里单把第10个命令的输出结果copy如下：
Yiming# snmpwalk -v 1 -c private 192.168.1.1 1.3.6.1.4.1.9.9.16.1.1.1.12.333
SNMPv2-SMI::enterprises.9.9.16.1.1.1.12.333 = INTEGER: 28
也就是说从此次测算的router到dst IP的平均RTT是28毫秒。
同样，我们可以对照MIB表查询本次动作的其他各个指标，常见的如
.1.3.6.1.4.1.9.9.16.1.1.1.10.333
.1.3.6.1.4.1.9.9.16.1.1.1.12.333
.1.3.6.1.4.1.9.9.16.1.1.1.14.333 等等等等，
通过这些分支，我们可以得知执行每个实例的最小最大RTT, 接收，发送的报文数等等项目，具体内容大家可以查看这些mib文件的说明，此外我们可以改动实例值和目的IP，同时起n个remote ping的实例，对多个目的地址remote ping，然后将结果导入rrdtools之类的软件，这里就不废话了。
  本小文只是简单的举了个利用snmp实现remote ping的例子，其实在我们的日常工作中，还有非常多的技术问题等待挖掘，管理员利用开源的东西，自己动手搞一些东西出来，可能会比商用产品起到更好的效果。
--
链接：
NETSNMP        http://www.net-snmp.org/
CISCO-PING-MIB-V1SMI    ftp://ftp.cisco.com/pub/mibs/v1/CISCO-PING-MIB-V1SMI.my
CISCO-PING-MIB.oid         ftp://ftp-sj.cisco.com/pub/mibs/oid/CISCO-PING-MIB.oid 版权所有，未经许可，不得转载  [![法律声明]()](http://www.nsfocus.com/7_law/index.html) ![]() [![联系我们]()](http://www.nsfocus.com/8_contact/index.html) ![]() [![在线客服]()](http://www.nsfocus.net/index.php?act=magazine&do=view&mid=2550#)   ![©2011 绿盟科技]()[![京ICP证110355号]()](http://www.miibeian.gov.cn/)      
