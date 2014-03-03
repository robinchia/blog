---
layout: post
title: "利用Squid构建高速的Proxy Server - 开源中国 OSChina.NET"
categories: 服务器
tags: 
 - 服务器
 - Squid
--- 

# 利用Squid构建高速的Proxy Server - 开源中国 OSChina.NET

* [首页](http://www.oschina.net/)
* [开源软件](http://www.oschina.net/project)
* [讨论区](http://www.oschina.net/question)

* [技术问答 »](http://www.oschina.net/question?catalog=1)
* [技术分享 »](http://www.oschina.net/question?catalog=2)
* [IT大杂烩 »](http://www.oschina.net/question?catalog=3)
* [职业生涯 »](http://www.oschina.net/question?catalog=100)
* [站务/建议 »](http://www.oschina.net/question?catalog=4)
* [支付宝专区 »](http://www.oschina.net/alipay)
* [代码分享](http://www.oschina.net/code/list)
* [博客](http://www.oschina.net/blog)
* [翻译](http://www.oschina.net/translate)
* [资讯](http://www.oschina.net/news)
* [移动开发](http://www.oschina.net/android)

* [Android开发专区](http://www.oschina.net/android)
* [iOS开发专区](http://www.oschina.net/ios/home)
* [iOS代码库](http://www.oschina.net/ios/codingList)
* [WP7开发专区](http://www.oschina.net/wp7)
* [招聘](http://www.oschina.net/job)

当前访客身份：游客 [ [登录](http://www.oschina.net/home/login?goto_page=http%3A%2F%2Fwww.oschina.net%2Fquestion%2F12_6378) | [加入开源中国](http://www.oschina.net/home/reg) ]

[开源中国](http://www.oschina.net/ "OSChina 开源中国")

# [讨论区](http://www.oschina.net/question)

当前位置： [讨论区](http://www.oschina.net/question) » [技术分享](http://www.oschina.net/question?catalog=2) » [Squid](http://www.oschina.net/p/squid)    搜 索
[![红薯]( "红薯")](http://my.oschina.net/javayou)

# [利用Squid构建高速的Proxy Server]()

[红薯](http://my.oschina.net/javayou) 发表于 2009-5-5 17:43 3年前, [1](http://www.oschina.net/question/12_6378#answers)回/1412阅, 最后回答: 4个月前

Java、PHP、Ruby、iOS、Python 等 JetBrains 开发工具低至 99 元（3折），[详情»](http://www.oschina.net/shop/jetbrains)

纪增雄        来自：[url=http://www.cnlinux.net]http://www.cnlinux.net
                转载请保留此信息，谢谢！
一、什么是Proxy Server(代理服务器)，Proxy的作用。
    在真实世界中我们常常会去帮人家办一些事情，例如帮人家交电费什么的，在这种情况下你不是电表的主人，而是代办者（代理者）的身份。在网络世界中Proxy就是相当于那个帮人家交电费的人了，当我们发出连接请求的时候，就会通过Proxy去帮我们直接与目标服务器沟通，帮我们取得资料。
    通常我们所说的高速缓存代理，就是以空间换时间，就如下图那样。
![]() 
client通过Proxy Server上网的步骤如下：
①client端向Server发出请求。
②Server收到请求后比较判断Cache中时候存在client想要的资料，如果没有则向远程Server发送数据请求。
③将请求回来的资料先存放到Cache中，再将资料传送给client端。
④当client发出的请求中所需要的资料在Cache中有，则将Cache中的资料直接传送给client端。
虽然当第一访问这向Proxy请求的数据Cache中没有时，Proxy抓取数据后会先保存在Cache中，这样访问速度变慢了，可是第二个访问者以及后来的访问者需要该资料的时候，proxy都不要想远程服务器请求，直接将cache中的资料发送给后来的请求者就行了，这样就减少了连接远程服务器的流量，另外由于proxy是在本地的，所以传输速度也更快。
二、使用Squid在构建Proxy Server
本文中笔者所使用的环境是：
操作系统: Redhat 9.0，内核:2.4.20-31.9，其他系统套件已经通过apt更新到最新了
1.编译安装Squid
由于Squid对系统硬件要求比较高，所以我们安装的时候应尽量优化。
#groupadd squid
#useradd squid
添加suqid用户和用户组
#export CFLAGES='-O2 -mcpu=pentium4 -march=pentium4 -mmmx -msse -msse2'
可以根据你的CPU选择相应的参数
GCC-3.1以上可針對CPU最佳化： 
Pentium2: -O2 -mcpu=i686 -march=i686 -mmmx
Pentium3: -O2 -mcpu=pentium3 -march=pentium3 -mmmx -msse
Pentium4: -O2 -mcpu=pentium4 -march=pentium4 -mmmx -msse -msse2 
#./configure --prefix=/usr/local/squid --enable-gnuregex --enable-async-io=80 --enable-icmp --enable-kill-parent-hack --enable-snmp --disable-ident-lookups --enable-cahce-digests --enable-arp-acl --enable-err-language="Simplify_Chinese" --enable-default-err-languages="Simplify_Chinese"  --enable-poll --enable-linux-netfilter --enable-underscore
#make
#make install
我个人安装软件都比较喜欢用源码包自己编译，觉得这样知道你自己在做什么，用rpm包好像不知道做什么的就安装好了。下面我们对各个编译参数进行解释，当然你可以通过./configure --help来查看其他的参数，以及各个参数的英文解释。
--prefix=/usr/local/squid  :指定软件的安装路径
--enable-gnuregex  :由于Squid大量使用字符串处理做各种判断，加入此项能更好的处理。
--enable-async-io=80 :这个主要是设置async模式来运行squid，我的理解是设置用线程来运行squid，如果服务器配置很不错，有1G以上内存，cpu使用SMP的方式的话可以考虑设成160或者更高。如果服务器比较糟糕就根据实际情况设了。另外此项还另cache文件支持aufs
--enable-icmp  :加入icmp支持
--enable-kill-parent-hack :关掉suqid的时候，要不要连同父进程一起关掉，这个当然要啦
--enable-snmp  :此选项可以让MRTG使用SNMP协议对服务器的流量状态进行监测，因此必须选择此项，使Squid支持SNMP接口。
--disable-ident-lookups  :防止系统使用RFC931规定的身份识别方法。
--enable-cahce-digests  :加快请求时，检索缓存内容的速度。
--enable-arp-acl  :可以在规则设置中直接通过客户端的MAC地址进行管理，防止客户使用IP欺骗。
--enable-err-language="Simplify_Chinese" 和
--enable-default-err-languages="Simplify_Chinese" :指定出错是显示的错误页面为简体中文
--enable-poll  :应启用Poll()函数而不是select()函数，通常而言poll(轮询)比select要好，但configure(脚本程序)已知Poll在某些平台下失效, 若你认为你比configure编译配置脚本程序要聪明的话，可以用这个选项启用Poll。总之就是用这个可以提升性能就是啦。
--enable-linux-netfilter :可以支持透明代理
--enable-underscore  :允许解析的URL中出现下划先，因为默认squid会认为带下划线的URL地址是非法的，并拒绝访问该地址。
这里我们就安装好了，接下来就是修改配置文件了。
2.修改定义配置参数
下面是我的squid.conf文件
# NETWORK OPTIONS（有关的网络选项）
# -----------------------------------------------------------------------------
http_port 3128  #代理端口
icp_port 3130  #icp端口
# OPTIONS WHICH AFFECT THE NEIGHBOR SELECTION ALGORITHM（作用于邻居选择算法的有关选项）
#-----------------------------------------------------------------------------
#禁止缓存
hierarchy_stoplist cgi-bin ?
hierarchy_stoplist -i ^https:\\ ?
acl QUERY urlpath_regex -i cgi-bin \? \.asp \.php \.jsp \.cgi
acl denyssl urlpath_regex -i ^https:\\
no_cache deny QUERY
no_cache deny denyssl
#上面几个就是说遇到URL中有包含cgi-bin和以https:\\开头的都不要缓存，
#还有asp、cgi、php等动态脚本也不要缓存，
#因为这些脚本通常都是动态更新的，这样数据不同步。
#还有https://开通的不缓存是因为一般我们进行电子商务交易，
#例如银行付款等都是采用这个的，如果把信用卡号什么缓存那不是很危险。
# OPTIONS WHICH AFFECT THE CACHE SIZE(定义cache大小的选项)
# -----------------------------------------------------------------------------
cache_mem 8 MB   #额外使用内存量，可根据你的系统内存在设定，一般为实际内存的1/3
cache_swap_low 90    #最低缓存百分比
cache_swap_high 95     ##最高缓存百分比，就是上面那个额外内存的使用百分比
maximum_object_size 4096 KB  #单个文件最大缓存大小，超过这个大小将不缓存
maximum_object_size_in_memory 8 KB  #在内存中单个文件最大缓存大小，超过这个大小将不缓存到内存中
#有DNS正反解所得到的IP存在缓存区的大小，这样可以加快解析速度
ipcache_size 1024
ipcache_low 90
ipcache_high 95
fqdncache_size 1024
# LOGFILE PATHNAMES AND CACHE DIRECTORIES(定义日志文件的路径及cache的目录）
# -----------------------------------------------------------------------------
#　　  ;   ;  <目录所在> ;   ;   ;   ; 
#　　那个 aufs 只有在编译的时候加入 --enable-async-io 那个选项才有支持， 
#　　至于目录所在地与所占用的磁盘大小则请视您的主机情况而定， 
#　　而后面 dir1, dir2 则是两个次目录的大小，通常 16 256 或 64 64 皆可， 
#　　一般来说，数字最好是 16 的倍数，据说性能会比较好啦！
cache_dir aufs /Cache1 100 16 256  
cache_dir aufs /Cache2 100 16 256
#日志存放位置
cache_access_log /usr/local/squid/var/logs/access.log
cache_log /usr/local/squid/var/logs/cache.log
#  TAG: cache_store_log
cache_store_log /usr/local/squid/var/logs/store.log
#  TAG: pid_filename
pid_filename /usr/local/squid/var/logs/squid.pid
# OPTIONS FOR EXTERNAL SUPPORT PROGRAMS(外部支持程序选项）
# -----------------------------------------------------------------------------
#用代理登陆匿名ftp服务选项
#  TAG: ftp_user
ftp_user Squid@    #用户名
ftp_passive on     #被动模式
#认证
#auth_param basic children 5
#auth_param basic realm Squid proxy-caching web server
#auth_param basic credentialsttl 2 hours
#auth_param basic casesensitive off
# OPTIONS FOR TUNING THE CACHE（调整cache的选项）
# -----------------------------------------------------------------------------
#  TAG: refresh_pattern    Cache更新时间设置
#  ;   ;  <最小时间> ;  <百分比> ;  <最大时间> ;
refresh_pattern ^ftp: 1440 20% 10080
refresh_pattern ^gopher: 1440 0% 1440
refresh_pattern . 0 20% 4320
#上面第一行如果网址开头是 ftp 的话，那么在一天(1440分钟)后，
#如果proxy 再次取用这个档案时，则 cache 内的数据会被更新！
# TIMEOUTS （超时）
# -----------------------------------------------------------------------------
#连接到其他机器的最大尝试时间
connect_timeout 1 minute
#连接到上层代理的超时时间
peer_connect_timeout 30 seconds
#返回超时
request_timeout 2 minutes
#持续连接时间
persistent_request_timeout 1 minute
# ACCESS CONTROLS（访问控制）
# -----------------------------------------------------------------------------
#  TAG: acl
#Examples:
#acl myexample dst_as 1241
#acl password proxy_auth REQUIRED
#acl fileupload req_mime_type -i ^multipart/form-data$
#acl javascript rep_mime_type -i ^application/x-javascript$
#
#Recommended minimum configuration:
acl all src 0.0.0.0/0.0.0.0
acl manager proto cache_object
acl localhost src 127.0.0.1/255.255.255.255
acl to_localhost dst 127.0.0.0/8
acl SSL_ports port 443 563
acl Safe_ports port 80 # http
acl Safe_ports port 21 # ftp
acl Safe_ports port 443 563 # https, snews
acl Safe_ports port 70 # gopher
acl Safe_ports port 210 # wais
acl Safe_ports port 1025-65535 # unregistered ports
acl Safe_ports port 280 # http-mgmt
acl Safe_ports port 488 # gss-http
acl Safe_ports port 591 # filemaker
acl Safe_ports port 777 # multiling http
acl CONNECT method CONNECT
acl inside src 192.168.1.0/24   #内部网IP段
acl localmac arp "/usr/local/squid/localmac"  #mac地址文件
#  TAG: http_access
http_access allow inside  #允许inside规则通过
http_access allow localmac  #允许localmac里面有登记的mac地址通过
#
#Recommended minimum configuration:
#
# Only allow cachemgr access from localhost
http_access allow manager localhost
http_access deny manager
# Deny requests to unknown ports
http_access deny !Safe_ports
# Deny CONNECT to other than SSL ports
http_access deny CONNECT !SSL_ports
#
#http_access deny to_localhost
#
# And finally deny all other access to this proxy
http_access deny all
#  TAG: http_reply_access
http_reply_access allow all
#  TAG: icp_access
#icp_access allow all
#  TAG: cache_peer_access
# ADMINISTRATIVE PARAMETERS（管理参数）
# -----------------------------------------------------------------------------
#  TAG: cache_mgr
cache_mgr webmaster@localhost  #管理员信箱
#  TAG: cache_effective_user
cache_effective_user squid  #运行squid时的用户
cache_effective_group  squid #运行squid时的组
#  TAG: visible_hostname
visible_hostname ProxyServer  #代理服务器名称
# OPTIONS FOR THE CACHE REGISTRATION SERVICE（cache注册服务选项）
# -----------------------------------------------------------------------------
# HTTPD-ACCELERATOR OPTIONS（HTTPD加速选项）
# -----------------------------------------------------------------------------
#设定透明代理
httpd_accel_host ProxyServer  #主机名
httpd_accel_port 80  #透明代理端口
httpd_accel_with_proxy on
httpd_accel_uses_host_header on
# MISCELLANEOUS（杂项）
# -----------------------------------------------------------------------------
#  TAG: logfile_rotate
#squid会定期的将日志文件更名并打包。
#比如正在使用的日志文件为access.log,squid会将其更名并打包为 access.log.1.gz；
#过了一定时间后，squid又会将access.log.1.gz更名为access.log.2.gz
#并将当前的日志文件更名并打包为access.log.1.gz，以此循环。
#logfile_rotate指定的数字即为打包并备份的文件的数量，当达到这一数目时，
#squid将删除最老的备份文件。默认值为1 0。如果想手动来进行这些操作，
#可以用logfile_rotate 0来取消自动操作。
logfile_rotate 4
#  TAG: forwarded_for on|off
#关闭此项将在访问某些论坛时显示的IP是unknown，
#如果打开则显示的是你client的内网IP
forwarded_for off
#图标文件目录
# icon_directory /usr/local/squid/share/icons
#错误提示文件目录
# error_directory /usr/local/squid/share/errors/Simplify_Chinese
#  TAG: snmp_port
# Squid can now serve statistics and status information via SNMP.
# By default it listens to port 3401 on the machine. If you don't
# wish to use SNMP, set this to "0".
#
#Default:
# snmp_port 3401
#  TAG: snmp_access
# Allowing or denying access to the SNMP port.
#
# All access to the agent is denied by default.
# usage:
#
# snmp_access allow|deny [!]aclname ...
#
#Example:
# snmp_access allow snmppublic localhost
# snmp_access deny all
#
#Default:
# snmp_access deny all
# DELAY POOL PARAMETERS (all require DELAY_POOLS compilation option)（延时池参数）
# -----------------------------------------------------------------------------
#  TAG: coredump_dir
#当squid突然挂掉的时候，或者突然出现什么故障的时候，将squid在内存中的资料写到硬盘中
coredump_dir /usr/local/squid/var/cache
3.设置iptables支持透明代理
设置squid+iptables支持透明代理前请先设置好NAT，可以使用下面的简单语句
echo "1" >; /proc/sys/net/ipv4/ip_forward   #设置转发
/sbin/iptables -t nat -A POSTROUTING -j MASQUERADE  #设置nat功能
iptables -t nat -A PREROUTING -i eth0 -p tcp -s 192.168.1.0/24 --dport 80 -j REDIRECT --to-ports 3128 
#将所有80端口的请求都转发到suqid的3128端口上
其中192.168.1.0/24表示192.168.1.1-254这个网段通过squid和nat做透明代理。
这样，当用户访问www服务的时候可以使用cache作为高速代理，减少流量，而其他服务则通过nat转发。
4.使用上层代理
当你访问国外网站比较慢的时候，可以通过设置代理访问，那么我们自己的代理服务器能否也设置别人的代理来访问国外的网站呢？答案是肯定的。
例如有代理proxy1.cnlinux.net能以较快的速度访问国外，且我们访问它也比较快，所以我们用它来作为我们访问国外网站的上层代理。
我们需要在squid.conf中添加如下参数：
;  <主机名称/地址> ;  <类别> ;   ;   ;  <其他参数> ;
类别主要有上层的parent和同一层的sibling两种，我们这里主要介绍的是上层代理，就是parent，如果你需要架设代理服务器集群的话可以采用sibling，这里我们就不做讨论了。
其他参数有：
proxy-only :只向上层代理要资料，自己不缓存到本地proxy中。
weight=n   :比重，当我们设置多台上层代理的时候，这几台代理的功能都相同的，可以通过设置此项来决定那台上层代理比较重要，n越大表示越重要。
no-query   :当使用sibling类别的时候，向同一层的proxy索要资料的时候就会向其送出icp请求，可以使用no-query来取消icp请求，一般我们向上层proxy请求资料的时候可以不需要发送icp包，以降低流量。
default    :表示将这台proxy设置为默认proxy
no-netdb-exchange :表示不向proxy送出imcp包的请求。
no-digest  :表示不纪录向上层proxy提交的请求。
#上层proxy设置
cache_peer proxy1.cnlinux.net parent 3128 3130 no-digest no-netdb-exchange
#设置访问规则，可以用域名，也可以用IP
acl usa dstdomain .com.us #美国.com.us的网站
acl usaip dst 18.0.0.0/8  #美国的部分IP段
#放行禁止规则
cache_peer_access proxy1.cnlinux.net allow usa #允许usa规则使用此上层proxy
cache_peer_access proxy1.cnlinux.net deny !usa #禁止所有非usa规则使用此上层proxy
cache_peer_access proxy1.cnlinux.net allow usaip
cache_peer_access proxy1.cnlinux.net deny !usaip
5.启动，关闭squid
a.将cache目录的所有者更改为squid
#chown -R squid:squid /Cache1
#chown -R squid:squid /Cache2
b.对cache目录进行初始化
#/usr/local/squid/sbin/squid -z
2004/11/01 23:06:29| Creating Swap Directories
FATAL: Failed to make swap directory /Cache1/00: (13) Permission denied
Squid Cache (Version 2.5.STABLE7): Terminated abnormally.
CPU Usage: 0.000 seconds = 0.000 user + 0.000 sys
Maximum Resident Size: 0 KB
Page faults with physical i/o: 10
如果出现上面这样的错误信息，表示你/Cache1目录权限错误，请检查/Cache1目录所有者是否为squid用户所有。
c.启动squid
#su squid -c "/usr/local/squid/bin/RunCache &"
d.关闭squid
#/usr/local/squid/sbin/squid -k shutdown
要执行两次才能正常关闭suqid
e.重新读取squid.conf文件
#/usr/local/squid/sbin/squid -k reconfigure
需要执行两次才能重新读取squid.conf文件
6.日志分析
Proxy服务器安装好后，我们当然要对服务器进行监控，通过日志分析，我们可以知道那些用户上了那些网站，用了多少流量等，下面为大家介绍sarg这个日志分析工具，在squid的官方网站还推介了其他几种日志分析工具，大家有兴趣的话可以上去看看。
a.安装
#./configure --prefix=/usr/local/sarg --enable-bindir=/usr/local/sarg/bin
#make && make install
b.设置sarg.conf文件
#vi /usr/local/sarg/sarg.conf
language language English  #由于官方网站还没有发布中文版，所以我们就使用英文好了，那位有兴趣可以自己翻译一下
access_log /usr/local/squid/var/logs/access.log.0 #squid日志文件存放位置
title "Squid 使用报告"   #标题
temporary_dir /tmp   #临时目录
output_dir /var/www/html/sarg   #生成后的html存放到那里，设置到你的网站目录下，以便浏览
overwrite_report no  #是否覆盖报告，当那个日期的报告已经存在时是否覆盖掉
mail_utility mail 
topsites_num 100 
exclude_codes /usr/local/sarg/exclude_codes 
max_elapsed 28800000 
charset GB2312  #字符集
c.生成报告
设置好sarg.conf文件后，执行
#/usr/local/sarg/bin/sarg
将提示：SARG: Successful report generated on /usr/local/apache/htdocs/sarg/2004Oct31-2004Nov01
表示报告生成成功，还有报告存放位置，可以马上打开您的浏览器查看报告了。
三、关于Cache目录的建议
由于cache目录是经常的读写，所以最好硬盘能用SCSI的，速度比较快而且稳定。如果我们的cache大概需要40G的大小，那么我们尽量使用多硬盘，不要当纯用一个40G的硬盘，可以使用4个10G的硬盘，这样，对于cache的速度更快。比如，当你有10M的东西要写到cache中，如果是只是用一个硬盘的话，虽然可能你已经将4个cache目录分别放在4个分区，可是你只有一个硬盘，同时只有一个在写入，可是当你有4个硬盘的时候，你每个硬盘就只要写入2.5M的东西，那样是不是更快呢？
还有，建议将每个cache目录单独存放在一个分区中，分区不要划分太大，一般2至4G就行，这样proxy搜索资料的时候不用耗费太多的时间。
本文参考了各位前辈的文章，如有冒犯请多多包涵！
![]()

**标签：** [Squid](http://www.oschina.net/question/tag/squid "代理服务器 Squid")
[补充话题说明»]()

**分享到** []( "分享到新浪微博")[]( "分享到腾讯微博")**

[收藏]( "收藏此话题")
**

3
**

[举报]()
**

[踩]( "踩：这问题不知道在说什么，或者没什么用") 0 | [顶]( "顶：这问题很有用或者很清晰明了") 0
**
## [按默认排序](http://www.oschina.net/question/12_6378#answers) | [显示最新评论](http://www.oschina.net/question/12_6378?sort=time#answers) | [回页面顶部](http://www.oschina.net/question/12_6378#top)  []()共有*1*个评论 [发表评论»](http://www.oschina.net/question/12_6378#answerform)

* [![隋济远]( "隋济远")](http://my.oschina.net/u/876254)

[隋济远](http://my.oschina.net/u/876254) 回答于 2012-11-29 10:12

[举报]()
学习了，非常感谢！
[有帮助]( "这是一个好评论，能解决问题")*(0)* | [没帮助]( "这评论无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此评论](http://www.oschina.net/question/answer?question=6378&answer=360621)

[![非会员用户]( "非会员用户")]()
[回评论顶部](http://www.oschina.net/question/12_6378#answers) | [回页面顶部](http://www.oschina.net/question/12_6378#top)

[![]()](http://www.oschina.net/action/visit/ad?id=1033 "JPush——极光推送")

有什么技术问题吗？ [我要提问](http://www.oschina.net/question/ask)
**[全部(4926)...](http://my.oschina.net/javayou/?ft=bbs&scope=2&showme=1)*红薯*的其他问题**

* [类似 DNSPod 智能 DNS 解析无效的一种情形](http://www.oschina.net/question/12_106724 "类似 DNSPod 智能 DNS 解析无效的一种情形")(3回/198阅,7天前)
* [Ruby 2.0 中未被提及的特性](http://www.oschina.net/question/12_106555 "Ruby 2.0 中未被提及的特性")(0回/128阅,8天前)
* [2013 第一期：OSC 深圳源创会高清无码大片放映](http://www.oschina.net/question/12_106453 "2013 第一期：OSC 深圳源创会高清无码大片放映")(106回/7120阅,9天前)
* [PostgreSQL 9.3 新特性预览 —— JSON 操作](http://www.oschina.net/question/12_106368 "PostgreSQL 9.3 新特性预览 —— JSON 操作")(18回/4385阅,10天前)
* [号召大家给 @滔哥 和 @走在路上 送上祝福](http://www.oschina.net/question/12_106012 "号召大家给 @滔哥 和 @走在路上 送上祝福")(35回/2011阅,12天前)

**类似的话题**

* [Squid应用 高效配置Linux系统代理服务器](http://www.oschina.net/question/12_6962 "Squid应用 高效配置Linux系统代理服务器  ")(0回/1046阅,3年前)
* [Squid 安装调试过程中的几个常用命令](http://www.oschina.net/question/12_8153 "Squid 安装调试过程中的几个常用命令")(0回/919阅,3年前)
* [利用 squid 反向代理提高网站性能](http://www.oschina.net/question/12_295 "利用 squid 反向代理提高网站性能")(4回/2635阅,4年前)
* [有没有人做过squid+wccp！！跪求！！！！谢谢各位！！！](http://www.oschina.net/question/44278_3403 "有没有人做过squid+wccp！！跪求！！！！谢谢各位！！！")(15回/756阅,3年前)
* [批量更新 squid 缓存。](http://www.oschina.net/question/17_3563 "批量更新 squid 缓存。")(0回/913阅,3年前)
* [Squid 3.0 更新某连接缓存的方法...其实太简单了!!](http://www.oschina.net/question/17_4996 "Squid 3.0 更新某连接缓存的方法...其实太简单了!! ")(0回/128阅,4年前)
* [squid 2.6.0 配置方法](http://www.oschina.net/question/17_4997 "squid 2.6.0 配置方法")(0回/79阅,4年前)
* [用squid再次疯狂加速你的web](http://www.oschina.net/question/1_5230 "用squid再次疯狂加速你的web")(0回/113阅,4年前)
* [squid 配置详解＋认证](http://www.oschina.net/question/12_6365 " squid 配置详解＋认证")(0回/1133阅,3年前)
* [netfilter和squid配合创建透明代理的问题讨论](http://www.oschina.net/question/17_6371 " netfilter和squid配合创建透明代理的问题讨论")(0回/172阅,3年前)
* [squid开多端口代理的心得](http://www.oschina.net/question/12_6374 " squid开多端口代理的心得")(0回/411阅,3年前)
* [SQUID的ncsa_auth认证原理](http://www.oschina.net/question/16_6380 " SQUID的ncsa_auth认证原理")(0回/144阅,3年前)
* [带用户验证的SQUID源码编译安装](http://www.oschina.net/question/16_6382 " 带用户验证的SQUID源码编译安装")(0回/273阅,3年前)
* [[转]linux+squid+wccp+cisco，虽是转也希望加精](http://www.oschina.net/question/12_6383 " [转]linux+squid+wccp+cisco，虽是转也希望加精")(0回/988阅,3年前)
* [squid用openldap实现验证](http://www.oschina.net/question/11_6385 " squid用openldap实现验证")(0回/275阅,3年前)
* [[好文共享]《Squid 中文权威指南》第1章 译者：彭勇华](http://www.oschina.net/question/16_6387 " [好文共享]《Squid 中文权威指南》第1章 译者：彭勇华")(0回/211阅,3年前)

© 开源中国(OsChina.NET) | [关于我们](http://www.oschina.net/home/about) | [广告联系](mailto:oschina.net@gmail.com) | [@新浪微博](http://weibo.com/oschina2010) | [开源中国手机版](http://m.oschina.net/) | [粤ICP备12009483号-3](http://www.miitbeian.gov.cn/) 开源中国手机客户端： [Android](http://www.oschina.net/app "Android客户端") [iPhone](http://www.oschina.net/app "iPhone 客户端") [WP7](http://www.oschina.net/app "Windows Phone 客户端")

[]()[]()[]()
![]()
