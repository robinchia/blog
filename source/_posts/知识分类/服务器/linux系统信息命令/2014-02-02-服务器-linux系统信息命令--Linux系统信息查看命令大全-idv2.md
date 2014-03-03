---
layout: post
title: "Linux系统信息查看命令大全 - idv2"
categories: 服务器
tags: 
 - 服务器
 - linux系统信息命令
--- 

# Linux系统信息查看命令大全 - idv2

# [idv2](http://tech.idv2.com/)

关注Web开发技术，关注Internet。

这里是charlee的个人技术Blog，关注Web开发，关注网页设计。在这里你可以找到有关Linux、 Apache、PHP、Perl、HTML、CSS、Photoshop的使用经验和开发技巧。 如果你想更多地了解charlee，请访问本站的[关于](http://tech.idv2.com/about/)页面。

* [首页](http://tech.idv2.com/)
* [推荐作品](http://tech.idv2.com/bookmark/ "推荐作品")
* [关于](http://tech.idv2.com/about/ "关于")
[上一篇：Javascript的变量与delete操作符](http://tech.idv2.com/2008/01/09/javascript-variables-and-delete-operator/) - [下一篇：在SATA硬盘上安装gentoo](http://tech.idv2.com/2008/01/13/install-gentoo-on-sata/)
2008-01

11

[Linux系统信息查看命令大全]( "永久链接: Linux系统信息查看命令大全")

**版权声明**：可以任意转载，但转载时必须标明原作者charlee、原始链接[http://tech.idv2.com/2008/01/11/linux-sysinfo-cmds/]()以及本声明。

最近看了一些Linux命令行的文章，在系统信息查看方面学到不少命令。 想起以前写过的一篇[其实Linux这样用更简单](http://tech.idv2.com/2007/08/25/simpler-linux/)， 发现这些系统信息查看命令也可以总结出一篇小小的东西来了。

另外[这里](http://www.pixelbeat.org/cmdline.html)还有非常多的命令， 可以作为参考。

**系统**
# uname -a # 查看内核/操作系统/CPU信息 # head -n 1 /etc/issue # 查看操作系统版本 # cat /proc/cpuinfo # 查看CPU信息 # hostname # 查看计算机名 # lspci -tv # 列出所有PCI设备 # lsusb -tv # 列出所有USB设备 # lsmod # 列出加载的内核模块 # env # 查看环境变量

**资源**

# free -m # 查看内存使用量和交换区使用量 # df -h # 查看各分区使用情况 # du -sh <目录名> # 查看指定目录的大小 # grep MemTotal /proc/meminfo # 查看内存总量 # grep MemFree /proc/meminfo # 查看空闲内存量 # uptime # 查看系统运行时间、用户数、负载 # cat /proc/loadavg # 查看系统负载

**磁盘和分区**

# mount | column -t # 查看挂接的分区状态 # fdisk -l # 查看所有分区 # swapon -s # 查看所有交换分区 # hdparm -i /dev/hda # 查看磁盘参数(仅适用于IDE设备) # dmesg | grep IDE # 查看启动时IDE设备检测状况

**网络**

# ifconfig # 查看所有网络接口的属性 # iptables -L # 查看防火墙设置 # route -n # 查看路由表 # netstat -lntp # 查看所有监听端口 # netstat -antp # 查看所有已经建立的连接 # netstat -s # 查看网络统计信息

**进程**

# ps -ef # 查看所有进程 # top # 实时显示进程状态

**用户**

# w # 查看活动用户 # id <用户名> # 查看指定用户信息 # last # 查看用户登录日志 # cut -d: -f1 /etc/passwd # 查看系统所有用户 # cut -d: -f1 /etc/group # 查看系统所有组 # crontab -l # 查看当前用户的计划任务

**服务**

# chkconfig --list # 列出所有系统服务 # chkconfig --list | grep on # 列出所有启动的系统服务

**程序**

# rpm -qa # 查看所有安装的软件包

标签: [linux](http://tech.idv2.com/tag/linux/) | 分类: [linux](http://tech.idv2.com/category/linux/ "查看 linux 的全部文章") | [引用通告](http://tech.idv2.com/2008/01/11/linux-sysinfo-cmds/trackback/ "使用本链接来引用这篇文章。") | [添加到delicious](http://delicious.com/save?jump=yes&url=http://tech.idv2.com/2008/01/11/linux-sysinfo-cmds/&title=Linux%E7%B3%BB%E7%BB%9F%E4%BF%A1%E6%81%AF%E6%9F%A5%E7%9C%8B%E5%91%BD%E4%BB%A4%E5%A4%A7%E5%85%A8) | 回到页面顶端
看完这篇文章后感觉怎样？如果还需要更多的内容，可以看看下面这些，也许会对你有帮助：

* [Xming：好用的Xserver](http://tech.idv2.com/2009/09/28/xming-good-xserver/ "Xming：好用的Xserver") (1)
* [SuSE Linux安装死机](http://tech.idv2.com/2009/09/23/suse-linux-install-freeze/ "SuSE Linux安装死机") (3)
* [利用gettext做软件翻译](http://tech.idv2.com/2009/07/08/translate-with-gettext/ "利用gettext做软件翻译") (5)
* [diff命令的好用功能](http://tech.idv2.com/2009/02/21/gnu-diff-tips/ "diff命令的好用功能") (3)
* [OpenNMS配置笔记](http://tech.idv2.com/2009/01/13/opennms-install-memo/ "OpenNMS配置笔记 ") (6)
* [Linux下Trac安装手记](http://tech.idv2.com/2008/12/26/install-trac-on-linux/ "Linux下Trac安装手记") (0)
* [rpm -i –nodeps时出现警告的原因](http://tech.idv2.com/2008/12/18/rpm-i-nodeps-warning/ "rpm -i –nodeps时出现警告的原因") (0)
* [memcached全面剖析–PDF总结篇](http://tech.idv2.com/2008/08/17/memcached-pdf/ "memcached全面剖析–PDF总结篇") (71)
* [Apache报告No space left on device的解决办法](http://tech.idv2.com/2008/08/15/apache-no-space-left-on-device/ "Apache报告No space left on device的解决办法") (0)
* [memcached全面剖析–5. memcached的应用和兼容程序](http://tech.idv2.com/2008/07/31/memcached-005/ "memcached全面剖析–5. memcached的应用和兼容程序 ") (14)

这篇文章有 12 条评论了，快来一起讨论讨论吧！

#1 fcicq
2008-01-11 17:17

1 不是所有的命令都需要 root 权限
2 sata 也可用 hdparm, 同时也有 sdparm.
3 mount 这么用的话有点太长?
#2 [charlee](http://www.idv2.com/)
2008-01-11 20:33

@fcicq 原谅我吧，犯了点懒直接都写成#了。sdparm倒是第一次听说，学习一下。mount是说mount column 吗？不过格式倒是好看了很多…

#3 [jackie](http://www.hinn.cn/)
2008-01-11 21:33

[http://www.linuxguide.it/linux_commands_line_en.htm](http://www.linuxguide.it/linux_commands_line_en.htm) 上面有比较详细的,我最近在翻译, [http://www.hinn.cn/2008/01/linux_command_1.html](http://www.hinn.cn/2008/01/linux_command_1.html) .
#4 [charlee](http://www.idv2.com/)
2008-01-14 10:45

@jackie 谢谢说明，期待你的翻译 :D

#5 [系统装机软件大全 » 在这里Linux系统信息查看命令大全](http://system.softcc.org/archives/1864.html)
2008-07-27 04:35

[...] Linux系统信息查看命令大全版权声明：可以任意转载，但转载时必须标明原作者charlee、原始链接http://tech.idv2.com/2008/01/11/linux-sysinfo-cmds/以及本声明。看Linux系统版本主要采用的方法。登录到服务器执行lsb_release -a，即可列出所有版本信息。登录到linux执行cat /etc/redhat-release，或登录到linux执行rpm -q redhat-release。 另外这里还有非常多的命令，可以作为参考。 系统# uname -a # 查看内核/操作系统/CPU信息# head -n 1 /etc/issue # 查看操作系统版本# cat /proc/cpuinfo # 查看CPU信息# hostname # 查看计算机名# lspci -tv # 列出所有PCI设备# lsusb -tv # 列出所有USB设备# lsmod # 列出加载的内核模块# env # 查看环境变量资源# free -m # 查看内存使用量和交换区使用量# df -h # 查看各分区使用情况# du -sh <目录名> # 查看指定目录的大小# grep MemTotal /proc/meminfo # 查看内存总量# grep MemFree /proc/meminfo # 查看空闲内存量# uptime # 查看系统运行时间、用户数、负载# cat /proc/loadavg # 查看系统负载磁盘和分区# mount | column -t # 查看挂接的分区状态# fdisk -l # 查看所有分区# swapon -s # 查看所有交换分区# hdparm -i /dev/hda # 查看磁盘参数(仅适用于IDE设备)# dmesg | grep IDE # 查看启动时IDE设备检测状况网络# ifconfig # 查看所有网络接口的属性# iptables -L # 查看防火墙设置# route -n # 查看路由表# netstat -lntp # 查看所有监听端口# netstat -antp # 查看所有已经建立的连接# netstat -s # 查看网络统计信息进程# ps -ef # 查看所有进程# top # 实时显示进程状态用户# w # 查看活动用户# id <用户名> # 查看指定用户信息# last # 查看用户登录日志# cut -d: -f1 /etc/passwd # 查看系统所有用户# cut -d: -f1 /etc/group # 查看系统所有组# crontab -l # 查看当前用户的计划任务服务# chkconfig –list # 列出所有系统服务# chkconfig –list | grep on # 列出所有启动的系统服务程序# rpm -qa # 查看所有安装的软件包>>>KEYAUTOBLOG [...]
#6 [Linux系统信息查看命令大全 at Nova’s Life](http://www.5thinknet.com/blog/?p=944)
2008-12-08 20:30

[...] 版权声明：可以任意转载，但转载时必须标明原作者charlee、原始链接 [http://tech.idv2.com/2008/01/11/linux-sysinfo-cmds/以及本声明。](http://tech.idv2.com/2008/01/11/linux-sysinfo-cmds/%E4%BB%A5%E5%8F%8A%E6%9C%AC%E5%A3%B0%E6%98%8E%E3%80%82) [...]

#7 [Linux系统信息查看命令大全 | PHPZ 动手吧！学PHP!](http://phpz.net.ru/linux_inf_cmd.html)
2009-04-09 00:42

[...] 版权声明：可以任意转载，但转载时必须标明原作者charlee、原始链接http://tech.idv2.com/2008/01/11/linux-sysinfo-cmds/以及本声明。 [...]
#8 [VPS 学习笔记 - Debian WEB服务器架设篇 - 寂静街](http://silentstreet.net/archives/vps-study-notes-debian-web-server-to-set-up-articles.html)
2009-04-09 14:38

[...] [http://tech.idv2.com/2008/01/11/linux-sysinfo-cmds/]() [...]

#9 [阳光不锈](http://www.936000.com/)
2011-02-27 09:51

谢谢，很有帮助，特别是在现在我刚买了VPS打算学习的时候。
#10 generic Cymbalta
2011-03-01 19:27

Really great post, Thank you for sharing This knowledge.

#11 [Linux下系统信息查询命令 | Linkanyway](http://blog.linkanyway.com/linux/linux-system/linux-sysinfo-cmd.html)
2012-01-27 23:23

[...] 命令转载自 Linux System/系统bash, Linux ← Linux-vsftpd限制用户chroot切换目录 /* */ [...]
#12 [dieta online](http://www.dietasmauri.com/)
2012-02-09 15:22

Brilliant post and useful information…I think this is what I read somewhere…but
I don’t know with your experience…

添加评论

姓名 (必须)

电子邮件 (邮件地址不会被公开) (必须)

主页

[上一篇：Javascript的变量与delete操作符](http://tech.idv2.com/2008/01/09/javascript-variables-and-delete-operator/) - [下一篇：在SATA硬盘上安装gentoo](http://tech.idv2.com/2008/01/13/install-gentoo-on-sata/)

### 最热门内容

* [利用delegate调试Ajax应用](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/) (166)
* [Excel常用函数扫盲](http://tech.idv2.com/2007/08/30/useful-excel-functions/) (88)
* [memcached全面剖析--PDF总结篇](http://tech.idv2.com/2008/08/17/memcached-pdf/) (71)
* [memcached完全剖析--1. memcached的基础](http://tech.idv2.com/2008/07/10/memcached-001/) (49)
* [memcached全面剖析--4. memcached的分布式算法](http://tech.idv2.com/2008/07/24/memcached-004/) (41)
* [Unicode详解](http://tech.idv2.com/2008/02/21/unicode-intro/) (40)
* [如何制作漂亮的Excel表格](http://tech.idv2.com/2007/08/29/make-beautiful-excel-sheet/) (38)
* [如何成为一个优秀的程序员](http://tech.idv2.com/2007/04/12/how-to-be-a-good-programmer/) (29)
* [Servlet/JSP学习笔记(4)-Servlet入门](http://tech.idv2.com/2007/09/14/servlet-basic/) (27)
* [Web 2.0 的视觉设计](http://tech.idv2.com/2006/11/30/the-visual-design-of-web-20-chinese/) (26)
* [BlackBerry 7230一周使用体验](http://tech.idv2.com/2007/09/08/blackberry-7230-tips/) (26)
* [如何寻找优秀程序员](http://tech.idv2.com/2008/01/24/how-to-recognise-a-good-programmer/) (25)
* [43个你应当避免的Web设计错误](http://tech.idv2.com/2007/05/02/43-web-design-mistakes-you-should-avoid/) (23)
* [海明码的计算方法](http://tech.idv2.com/2005/02/14/how-to-calculate-hamming-code/) (21)
* [memcached全面剖析--2.理解memcached的内存存储](http://tech.idv2.com/2008/07/11/memcached-002/) (20)
* [利用CSS使Div水平垂直居中](http://tech.idv2.com/2007/07/22/center-div-horizontally-and-vertically-with-css/) (18)
* [微软powershell=powertoy](http://tech.idv2.com/2007/06/20/microsoft-powershell-powertoy/) (16)
* [新主题Crystal上线](http://tech.idv2.com/2007/10/31/newtheme/) (16)
* [8个你未必知道的Excel技巧](http://tech.idv2.com/2007/05/23/8-excel-tips-you-dont-know/) (15)
* [其实Linux这样用更简单](http://tech.idv2.com/2007/08/25/simpler-linux/) (14)

### 最新评论

* [Hong](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-455005 "View the entire comment by Hong"): over counter Olmesartan - Hydrochlorothiazide alt...
* [Augustine](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-455004 "View the entire comment by Augustine"): cheap Olmesartan - Hydrochlorothiazide no script,...
* [Rod](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-455003 "View the entire comment by Rod"): cheapest place to buy Olmesartan - Hydrochlorothi...
* [Duane](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-455002 "View the entire comment by Duane"): canadian discount Olmesartan - Hydrochlorothiazid...
* [Efren](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-455001 "View the entire comment by Efren"): cheap Olmesartan - Hydrochlorothiazide no script,...
* [Garret](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-455000 "View the entire comment by Garret"): Olmesartan - Hydrochlorothiazide online consultan...
* [Guadalupe](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-454999 "View the entire comment by Guadalupe"): pancreatitis seroquel, seroquel buy, is seroquel ...
* [Robin](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-454998 "View the entire comment by Robin"): zyprexa seroquel, seroquel tiredness, seroquel do...
* [Johnson](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-454997 "View the entire comment by Johnson"): tapering off seroquel, how long does seroquel wit...
* [Jerold](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-454996 "View the entire comment by Jerold"): seroquel 150, depression seroquel, seroquel drugs...
* [commercial playground](http://tech.idv2.com/2008/07/24/memcached-004/#comment-454995 "View the entire comment by commercial playground"): I daily watch your website because it helps me in...
* [Emmitt](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-454994 "View the entire comment by Emmitt"): seroquel xr coupon, celexa and seroquel, seroquel...
* [Ira](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-454993 "View the entire comment by Ira"): proscar for women, proscar and hair loss, hair lo...
* [Timothy](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-454992 "View the entire comment by Timothy"): proscar for women, avodart vs proscar, splitting ...
* [Delmar](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-454991 "View the entire comment by Delmar"): proscar for hair growth, proscar buy, proscar res...
* [Willian](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-454990 "View the entire comment by Willian"): proscar hair regrowth, proscar and hair loss, pro...
* [Stanford](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-454989 "View the entire comment by Stanford"): proscar indications, proscar hair loss, proscar o...
* [anopos](http://tech.idv2.com/2012/01/20/face-detection-with-python-opencv/#comment-454988 "View the entire comment by anopos"): 精彩...
* [失控](http://tech.idv2.com/2006/08/31/burp-suite/#comment-454987 "View the entire comment by 失控"): 为什么我打开suite.bat，没有运行...
* [Isaac](http://tech.idv2.com/2007/01/30/debug-javascript-at-localhost-with-delegate/#comment-454986 "View the entire comment by Isaac"): nolvadex pct dosage, nolvadex only cycle, nolvade...

### 分类

* [database](http://tech.idv2.com/category/database/ "查看 database 下的所有文章") (12)
* [hardware](http://tech.idv2.com/category/hardware/ "查看 hardware 下的所有文章") (10)
* [linux](http://tech.idv2.com/category/linux/ "查看 linux 下的所有文章") (67)
* [perl](http://tech.idv2.com/category/perl/ "查看 perl 下的所有文章") (39)
* [photoshop](http://tech.idv2.com/category/photoshop/ "查看 photoshop 下的所有文章") (7)
* [python](http://tech.idv2.com/category/python/ "查看 python 下的所有文章") (7)
* [software](http://tech.idv2.com/category/software/ "查看 software 下的所有文章") (50)
* [web](http://tech.idv2.com/category/web/ "查看 web 下的所有文章") (65)
* [windows](http://tech.idv2.com/category/windows/ "查看 windows 下的所有文章") (17)
* [wordpress](http://tech.idv2.com/category/wordpress/ "查看 wordpress 下的所有文章") (16)
* [其他](http://tech.idv2.com/category/uncategorized/ "查看 其他 下的所有文章") (51)
* [管理](http://tech.idv2.com/category/%e7%ae%a1%e7%90%86/ "查看 管理 下的所有文章") (8)
* [编程开发](http://tech.idv2.com/category/develop/ "开发环境配置、C语言等。") (28)

### 随便看看

* [如何删除品牌电脑中的隐藏分区](http://tech.idv2.com/2009/11/09/delete-hidden-part/ "
家里有一台清华同方的品牌电脑，三年前的配置，现在有些力不从心了。最近打算给它升级，于是加了一条1G内存，然后整理好C盘，杀毒升级装软件，一切都弄好了。最后打算用一键还原精灵给它做个备份，但忽然发现，系统预装了清华同方的“同方急救中心”——一般来说，这种软件都会在硬盘上...  9.11.2009")
* [数据安全：备份，备份，再备份](http://tech.idv2.com/2007/09/27/backup-your-blog/ "

最近看到一个[[Tag丢失事件>http://blog.okevin.net/20070926/bang.html]]，想到最近随着Wordpress的不断升级，
各位blogger或许都在考虑着将自己的blog升级吧。但是又有几人考虑到自己数据的安全性呢？

相信[[okevin>http://blog.okevin.net]]就是因为太过于自信而直接在正式服务器上运行升级程序而造成的惨剧吧。
插...  27.09.2007")
* [并行口针脚定义](http://tech.idv2.com/2004/06/06/parallel/ "

#ref(2009/04/o_parallel.gif,nolink)

|  脚    |    名称     |   I/O  |   有效 | 描述    |
|  1     |   STROBE    |   O    |   L    | 指示数据准备好  |
|  2     |   DATA0     |   I/O  |   H    | 数据最低位  |
|  3     |   DATA1     |   I/O  |   H    |      |
|  4     |   DATA2     |   I/O  |   H    |      |
|  5     |   DATA3     |   I/O  |   H    |      |
|  6     |...  6.06.2004")
* [mysql_connect报告"No such file or directory"错误的解决方法](http://tech.idv2.com/2011/04/27/mysql_connect_no_such_file/ "
今天在MacBookPro上安装wordpress时，安装程序一直报错说连不上数据库。mysql客户端可以正常使用，可以确定不是服务器的问题。写了个php脚本单独执行mysql_connect()，发现错误信息居然是“No such file or directory！这里应该没涉及到文件啊？

在网上搜了一下，找到了这篇文章：[[mysql_connect and No such file or d...  27.04.2011")
* [GoDaddy域名重定向DIY](http://tech.idv2.com/2007/02/16/godaddy-domain-redirect-diy/ "
最近好多人都在问我GoDaddy的域名转向设置方法。其实我没用过GoDaddy的域名转向，
而且据说在中国国内无法访问域名转发服务。幸好GoDaddy对每个域名都提供了免费的虚拟主机，
虽然是带广告的，不过我们可以用它来做自己的域名重定向。
方法么，自然是用[[mod_rewrite>http://tech.inspiremedia.org/archives/304...  16.02.2007")
* [通过免费的VMware Server充分利用你的服务器资源](http://tech.idv2.com/2007/05/22/vmware-server/ "

VMware相信大家都耳熟能详，不过估计大家用的都是价值200多刀的VMware Workstation版。
而VMware公司的另一个产品——VMware Server，不收取一分钱费用却能让你实现真正的虚拟服务器。

有关VMware Workstation版和VMware Server版的详细区别请参见
[[smalldust的这篇文章>http://www.codesoil.net/2007/05/19/vmware-features-compari...  22.05.2007")
* [如何在下载文件名中使用UTF-8编码](http://tech.idv2.com/2009/03/05/use-utf8-in-download-filename/ "

通过把Content-Type设置为application/octet-stream，
可以把动态生成的内容当作文件来下载，相信这个大家都会。
那么用Content-Disposition设置下载的文件名，
这个也有不少人知道吧。
基本上，下载程序都是这么写的：

 

这样用浏览器打开之后，就可以下载document.doc。

但是，如果$filename是UTF-8编码...  5.03.2009")
* [关闭VMware的PC喇叭](http://tech.idv2.com/2010/02/26/disable-pc-speaker-in-vmware/ "
在VMWare中运行一些Linux上的软件如vi，出错时PC喇叭会不停地叫，很烦人。
其实只要在 c:Documents and Settings用户名Application DataVMwareconfig.ini （如不存在请自行建立）中加入这样一行：
 mks.noBeep = TRUE
就可以从虚拟硬件上关闭VMWare的PC喇叭。

我用的VMware是 VMware Workstation 5.5.1版。

...  26.02.2010")
* [跨站脚本攻击(XSS)FAQ](http://tech.idv2.com/2006/08/30/xss-faq/ "
该文章简单地介绍了XSS的基础知识及其危害和预防方法。Web开发人员的必读。译自 http://www.cgisecurity.com/articles/xss-faq.shtml。



#contents
--------------------

* 简介

现在的网站包含大量的动态内容以提高用户体验，比过去要复杂得多。
所谓动态内容，就是根据用户环境和需要，Web应用程序能够输出...  30.08.2006")
* [[Perl]使用DProf测定程序执行效率](http://tech.idv2.com/2008/10/30/perl-dprof/ "

代码写多了，程序就会变得臃肿；程序臃肿了，就会变慢。
这时提高代码执行效率就非常重要了。
但是，代码优化并不是几条best practice就能完成的。
那些无关痛痒的空间分配、减少复制等优化措施，虽然有效，但却微乎其微。
优化的关键，是要''找出瓶颈并解决之''，这样才能以最小的代价获...  30.10.2008")
* [opqcp:C语言混淆器](http://tech.idv2.com/2006/04/10/opqcp-c-scrambler/ "
从C FAQ上看到的一个程序，可以将C语言源文件变成难以识别的代码。文件从http://www.faqs.org/ftp/usenet/comp.sources.misc/packages/opqcp/下载得到。

#ref(2006/09/opqcp.zip)

...  10.04.2006")
* [BlackBerry 7230刷机后的三篇欢迎邮件](http://tech.idv2.com/2007/09/20/blackberry-7230-welcome-mail/ "BlackBerry 7230刷机之后第一次开机，短信收件箱里面会有三封初始邮件，标题分别是《Welcome》、《Top 20 Tips》、《Top 10 Phone Tips》。其中后两篇提到了很多连老手都不知道的应用技巧。可是又有谁真正认真地读过呢？

刚刚刷完ROM，准备删除这三封邮件，立此存照。



From: BlackBerry
Subject: Welcome
-------...  20.09.2007")
* [PostScript入门(1)-基本知识](http://tech.idv2.com/2010/07/25/postscript-tuturial-1/ "

最近由于项目需要，一直在研究PostScript语言。由于这个语言通常用在打印机上，
一般用户接触不到，因此网上的资料也十分罕见。所以，我想把这段时间的心得
整理成一篇入门文章，与大家分享，希望能对想研究打印机的朋友们有所帮助。

这篇文章计划分成七个部分，分别是：

+ 基本知识(...  25.07.2010")
* [项目经理和技术主管的分工](http://tech.idv2.com/2008/06/05/pm-and-tech-leader/ "

本文是日经BP上的一篇关于项目管理方法的实践的文章。对于短期的Web小型项目，
无论是开发周期、成本，还是技术上都有很大的风险。然而许多人对此认识不足，
导致项目失败，或是产品发布日期一拖再拖、成本大幅上升，或是匆忙发布后漏洞百出。
本文从项目管理的角度说明，即使在小型项...  5.06.2008")
* [Oracle常用SQL语句](http://tech.idv2.com/2006/06/13/useful-oracle-sql/ "Oracle使用过程中经常会用到的SQL语句。


#contents
-----------------------------------

** 表

创建表。
 CREATE TABLE [schema.]t_employees (
     employee_id NUMBER(2),                  -- 长度=2的整数
     hire_date DATE DEFAULT SYSDATE,         -- default的例子
	 ...
 ) AS 子查询;                                -- 利用子查询建立表

确认表...  13.06.2006")
* [[PHP]getdate()等时间函数的时区问题](http://tech.idv2.com/2007/10/08/php-getdate-timezone/ "这两天一直被一个问题所困扰。同样的一段程序，在自己的机器上调试完全没有问题，放到服务器上时间显示就快了8个小时。显然这8个小时就差在时区设置上。我在自己的机器上写代码时，对timestamp进行了时区调整（$now += 8 * 3600）之后再进行getdate()转换，结果完全正确；放到服务器上执行，就要将时...  8.10.2007")
* [使用Apache做负载均衡](http://tech.idv2.com/2009/07/22/loadbalancer-with-apache/ "

第一次看到这个标题时我也很惊讶，Apache居然还能做负载均衡？真是太强大了。
经过一番调查后发现的确可以，而且功能一点都不差。
这都归功于 mod_proxy 这个模块。
不愧是强大的Apache啊。

废话少说，下面就来解释一下负载均衡的设置方法。





一般来说，负载均衡就是将客户端的请求...  22.07.2009")
* [Ubuntu 6.10 edgy升级指南](http://tech.idv2.com/2006/11/03/ubuntu-610-edgy-upgrade-howto/ "
&color(red){[[这里>http://tech.idv2.com/2006/09/10/ubuntu-install-memo/]]有一篇关于如何安装 Ubuntu 6.06 的文章，虽然有些老了，但是本文是在这篇文章的基础上写成的，看看也许能有些参考。};

修改 /etc/source.list，将其中的所有 dapper 替换成 edgy。

如果你使用了 http://packages.freecontrib.org/ubuntu/plf/ 的源，则要将这...  3.11.2006")
* [使用nc快速传送文件](http://tech.idv2.com/2005/03/30/use-nc-to-send-file/ "
如果两台计算机之间突然需要传送一个文件，而一时又没有什么好用的通讯工具，也来不及开服务器的时候，那么可以使用nc来传送文件。方法如下：
 接收者:  $ nc -l -p 12345 > save_filename    ; 12345为1024-65535的任意端口号
 发送者： $ nc  12345 < send_file

...  30.03.2005")
* [Servlet/JSP学习笔记(6)-从配置文件获取参数](http://tech.idv2.com/2007/09/20/get-init-params/ "

前两节([[1>http://tech.idv2.com/2007/09/14/servlet-basic/]], [[2>http://tech.idv2.com/2007/09/16/httpservlet/]])
分别介绍了 GenericServlet 和 HttpServlet 的用法。
这一节将介绍 ServletContext 和 ServletConfig 这两个接口。
通过这两个接口，我们可以在web.xml中设置一些参数，如数据库地址、用户名密码等，供 Servlet 使用，
这样每...  20.09.2007")

### 其他

* [登录](http://tech.idv2.com/wp-login.php)
### 搜索

### 订阅这个Blog

* [订阅到Google](http://fusion.google.com/add?feedurl=http://tech.idv2.com/feed/)
* [订阅到抓虾](http://www.zhuaxia.com/add_channel.php?url=http://tech.idv2.com/feed/)
* [订阅到鲜果](http://www.xianguo.com/subscribe.php?url=http://tech.idv2.com/feed/)
* [订阅到Bloglines](http://www.bloglines.com/sub/http://tech.idv2.com/feed/)

### Blogger们

* [awflasher](http://www.awflasher.com/blog/)
* [cutecool is me](http://cutecool.me/)
* [DBA notes](http://www.dbanotes.net/)
* [demo@virushuo](http://blog.devep.net/virushuo/)
* [fcicq’s blog-beta](http://www.fcicq.net/wp/)
* [Nicky’s blog](http://www.osxcn.com/)
* [Nings blog](http://nings.cn/)
* [Realazy](http://realazy.org/blog "许多Javascript开发文章")
* [RSS相关](http://aboutrss.cn/ "RSS知识普及")
* [Tinyfool的开发日记](http://tiny4.org/prog/diary/diary.htm)
* [三秒改变世界](http://blog.yangyc.com/)
* [乱象，印迹](http://www.luanxiang.org/blog/)
* [午夜兰花手札](http://www.xyzlove.com/)
* [抽筋](http://www.xuchi.name/blog)
* [林健的BLOG](http://blog.linjian.org/)
* [红尘的尘–Tinyfool的随想录](http://tiny4.org/jsjy/sxl/sxl.htm)
* [老赵点滴](http://blog.zhaojie.me/)
* [车东](http://www.chedong.com/blog/)

### 我的其他网站

* [ideapool](http://www.idv2.com/ideapool)
* [主页](http://www.idv2.com/)

### 我的朋友

* [&Zz的主页](http://nand.me/)
* [Code Soil](http://www.codesoil.net/)
* [Frank's MSN Site](http://wuyizhou.spaces.live.com/)
* [Kernel Space](http://mykernelspace.com/)

### 推荐书籍

[![]()](http://book.douban.com/subject/3142173/) [![]()](http://book.douban.com/subject/3598757/)  [![]()](http://book.douban.com/subject/3590787/) [![]()](http://book.douban.com/subject/4827960/)  [![]()](http://book.douban.com/subject/6758780/) [![]()](http://book.douban.com/subject/6799412/)

[![Creative Commons License]()](http://creativecommons.org/licenses/by-nc-sa/2.5/)
本网站作品采用[知识共享署名-非商业性使用-相同方式共享 2.5 许可协议](http://creativecommons.org/licenses/by-nc-sa/2.5/)进行许可。
由 [**WordPress**](http://wordpress.org/ "基于 WordPress，优美的个人信息发布平台。") 所驱动
网站主题[Crystal](http://tech.idv2.com/)由[charlee](http://www.idv2.com/)设计制作
