---
layout: post
title: "Oracle10_2 for RHEL5_4_x86_64 安装手册 - HankenGt的个人空间"
categories: DB
tags: 
 - DB
 - oracle
 - 安装
--- 

# Oracle10_2 for RHEL5_4_x86_64 安装手册 - HankenGt的个人空间 - ITPUB个人空间 - powered by X-Space

**HankenGt的个人空间**

[copy]( "复制地址") [Bookmark](http://space.itpub.net/22650465 "加入收藏") http://space.itpub.net/22650465

* [博客](http://space.itpub.net/22650465/spacelist-blog)
* [图片](http://space.itpub.net/22650465/spacelist-image)
* [商品](http://space.itpub.net/22650465/spacelist-goods)
* [下载](http://space.itpub.net/22650465/spacelist-file)
* [收藏](http://space.itpub.net/22650465/spacelist-link)
* [影音](http://space.itpub.net/22650465/spacelist-video)
* [圈子](http://space.itpub.net/22650465/spacelist-group)
* [好友](http://space.itpub.net/22650465/spacelist-friend)
* [论坛](http://space.itpub.net/22650465/spacelist-bbs)
* [留言](http://space.itpub.net/22650465/action-viewpro)

[空间管理](http://space.itpub.net/batch.manage.php?uid=22650465) 您的位置: [ITPUB个人空间](http://space.itpub.net/) » [HankenGt的个人空间](http://space.itpub.net/22650465/) » [日志](http://space.itpub.net/22650465/spacelist-blog)

# Oracle10.2 for RHEL5.4.x86_64 安装手册

[上一篇](http://space.itpub.net/batch.common.php?action=viewspace&op=up&itemid=625729&uid=22650465) / [下一篇](http://space.itpub.net/batch.common.php?action=viewspace&op=next&itemid=625729&uid=22650465)  2010-01-21 23:02:02 / 天气: 晴朗 / 心情: 高兴 / 精华(3) / 置顶(1)
[查看( 558 )](http://space.itpub.net/22650465/viewspace-625729#xspace-tracks) / [评论( 1 )](http://space.itpub.net/22650465/viewspace-625729#xspace-itemreply) / [评分( 0 / 0 )](http://space.itpub.net/22650465/viewspace-625729#xspace-itemform)

Oracle10.2 for RHEL5.4.x86_64[**安装**]()手册

# 1.   系统环境

OS：Redhat Enterprise [**Linux**]() 5.4.x86_64

OS kenrel：2.6.18-164.EL5

[**Oracle**]()：Oracle10.2.0.1EnterpriseFor Linux 86_64

Harddisk:320G

Memory:4G

Ethernet adapter：100Base-T

 

# 2.   操作系统要求

##            硬盘的分区和目录规划

**Filesystem**
 
**Type******
 
**Size******
 
**Mounted on****** /dev/hda1
 
ext3
 
1,024M
 
/boot /dev/mapper/VG00-LV00
 
ext3
 
30,720M
 
/ /dev/mapper/VG00-LV01
 
swap
 
6,144M
 
N/A /dev/mapper/VG00-LV02
 
ext3
 
20,480 M
 
/home /dev/mapper/VG00-LV03
 
ext3
 
20,480 M
 
/opt /dev/mapper/VG00-LV04
 
ext3 
 
20,480 M
 
/usr /dev/mapper/VG00-LV05
 
ext3
 
20,480 M
 
/var /dev/mapper/VG00-LV06
 
ext3
 
20,480 M
 
/tmp /dev/mapper/VG00-LV07
 
ext3
 
61,440M
 
/oracle /dev/mapper/VG00-LV08
 
ext3
 
61,440M
 
/websphere tmpfs
 
ext3
 
20,480 M
 
 /tmp

      

         

      

      

     

         

     

         

           

         

 

 

 

**1)       ****检查硬件信息**

#grep MemTotal /proc/meminfo

#grep SwapTotal /proc/meminfo

#df –h

##            验证Linux安装

**2)       ****检查内核版本**

#uname -r

**3)       ****安装[**软件**]()包**

 系统安装软件包的时候，选择“自定义”方式安装软件包，并且选择“安装全部软件包”的方式进行安装，如下补丁一定要安装，版本可以高于下面列出的

 

1)     binutils-2.17.50.0.6-12.EL5

2)     compat-db-4.2.52-5.1

3)     compat-libstdc++-33-3.2.3(i386)

4)     compat-libstdc++-33-3.2.3(x86_64)

5)     control-center-2.16.0-16. EL5

6)     gcc-4.1.2-46.EL5

7)     gcc-c++-4.1.2-46.EL5

8)     glibc-2.5-42

9)     glibc-common-2.5-42

10) libaio-0.3.106.3.2

11) libstdc++-4.1.2-46.EL5

12) libstdc++-devel-4.1.2-46.EL5

13) libXp-1.0.0-8

14) make-3.81-1.1

15) openmotif-2.3.1-2.3.EL5

16) pdksh-5.2.14-36.EL5

17) setarch-2.0-1.1

18) sysstat-7.0.2-3.EL5

（Tips： 建议先检查，再安装，参考步骤**4）**

 

以下包建议安装i386和x86_64两者

libaio-0.3.106-3.2.x86_64.rpm

libaio-0.3.106-3.2.i386.rpm

libXp-1.0.0-8.x86_64.rpm 

libXp-1.0.0-8.i386.rpm 

**4)       ****检查所需软件包**

# rpm -q binutils compat-db control-center compat-libstdc++ gcc gcc-c++ glibc glibc-common libaio libstdc++ libstdc++-devel libXp make openmotif pdksh setarch sysstat

Ip地址的规划
主机名
 
Eth0
 
Eth1 devdb
 
130.120.2.158
 
 

**5)       ****/etc/hosts****文件修改**

把127.0.0.1修改为网卡设置的(直实)ip地址,其后把多余的主机名去掉,只保留一个真实的主机名

如：130.120.2.158  devdb

      

##            ORACLE所需的系统配置

**6)       ****修改/etc/profile****文件**

添加如下内容:

# for oracle

if [ $USER = "oracle" ]; then

       if [ $SHELL = "/bin/ksh" ]; then

               ulimit -p 16384

               ulimit -n 65536

       else

               ulimit -u 16384 -n 65536

       fi

fi

 

**修改系统内核参数******

**7)       ****编辑/etc/sysctl.conf****文件**

kernel.shmall = 2097152

kernel.shmmax = 2147483648

kernel.shmmni = 4096

# semaphores: semmsl, semmns, semopm, semmni

kernel.sem = 250 32000 100 128

fs.file-max = 65536

net.ipv4.ip_local_port_range = 1024 65000

net.core.rmem_default = 1048576

net.core.rmem_max = 1048576

net.core.wmem_default = 262144

net.core.wmem_max = 262144

 

修改完成后，用root用户执行以下命令导入刚才写入的参数

＃sysctl -p

**8)       ****编辑/etc/security/limits.conf****文件**

添加如下内容

*  soft  nproc  2047

*  hard nproc  16384

*  soft  nofile  1024

*  hard nofile  65536

**9)       ****编辑/etc/pam.d/login****文件**

添加如下内容

session   required    /lib/security/pam_limits.so

session   required    pam_limits.so

**10)    ****vi /etc/selinux/config**

确保以下内容

SELINUX=disabled

**11)    ****创建oinstall dba****组**

＃groupadd oinstall

# groupadd dba

**12)    ****创建oracle****用户**

＃useradd –g oinstall –G dba oracle

#passwd oracle

**13)    ****创建oracle****软件安装目录**

＃mkdir /oracle/oradata

**14)    ****修改目录属主属组**

＃chown –R oracle:oinstall /oracle/oradata

# chmod 766 / oracle/oradata

**15)    **** ****以oracle****用户登录系统**

＃su – oracle

**16)    ****  ****修改.bash_profile****文件**

在里面添加如下内容

#ORACLE ENVIRONENMENT

export ORACLE_BASE=/oracle/oradata

export ORACLE_HOME=$ORACLE_BASE/product/10.2.0/db_1

export ORACLE_SID=YJZH10G

export ORACLE_TERM=xterm

export PATH=/usr/sbin:$PATH

export PATH=$ORACLE_HOME/bin:$PATH

export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib64:/usr/lib64:/usr/local/lib64:/usr/X11R6/lib64/

export CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib

export TMP=/tmp

export TMPDIR=$TMP

umask 022

unset USERNAME

执行环境设置

source ~/.bash_profile

验证是否正确

echo $ORACLE_BASE

**17)    ****修改版本**

方法1:编辑文件/etc/redhat-release把Red Hat Enterprise Linux [**Server**]() release 5 (Tikanga)改成版本4，当然oracle安装完成后,要修改回来: redhat-release是只读文件,先更改它的属性,然后再更改

chmod 777 /etc/redhat-release

vi /etc/redhat-release

删除或注释掉这一行：Red Hat Enterprise Linux Server release 5 (Tikanga)

添加这一行：Red Hat Enterprise Linux AS release 4 (Nahant Update 4)

方法2:安装时忽略系统检查

#./runInstaller -ignoreSysPrereqs

**18)    ****重起系统**

＃reboot

# 3.   安装ORACLE软件

**19)    ****将[**数据库**]()软件上传到主机/oracle****目录，**

完毕后可以看到本路径下多了个文件：10201_database_linux_x86_64.cpio

**20)    ****解压Oracle****安装文件**

cpio -idmv < 10201_database_linux_x86_64cpio

**21)    ****使GUI****终端上可以正常安装操作**

以root用户登录

＃xhost +  

  或这样

＃vncserver

然后切换为Oracle用户

#su – oracle

# cd /oracle/database

**   **

**22)    ****执行runInstaller****安装软件**

$.database/runInstaller  

   

出现安装画面

**23)    ****选择Advanced Installation**

**24)    ****指定存储oracle****安装信息的目录和该目录拥有者（oinstall****）**

 

**25)****选择安装方式，推荐选择[**企业**]()版**

请根据license情况选择安装内容。

 

**26)    ****选择安装语言**

 

**27)    ****选择安装目录**

 

**28)    ****系统安装先决条件的检查**

 

**29)    ****指定拥有dba****[**管理**]()权限的操作系统用户组**

 

**30)    ****选择只安装数据库**

**31)    ****显示安装清单**

 

**32)    ****开始安装**

** **

**33)    ****开始进行组件的配置**

 

**34)    ****以root****用户执行如下的脚本**

 

**35)    ****完成安装软件**

 

# 4.   创建ORACLE实例

以oracle用户登陆，

**36)    ****执行dbca****命令**

$dbca

出现欢迎画面

$dbca

**37)    ****选择创建数据库**

 

**38)    ****选择数据库模板（自定义数据库）**

 

**39) ****定义数据库的“全局数据库名”和“SID”****为“YJZH10G”**

**40)    ****选择数据库的管理选项**

 

**41)    ****设置数据库的帐户密码，这里建议初始密码为：ora123**

 

**42)    ****选择数据库的存储模式（文件系统）**

 

**43)    ****使用模板中的文件位置来创建数据文件**

 

**44)    ****查看并确认安装过程中会引用的文件位置变量**

**45)    ****配置数据库的恢复选项（启用快闪恢复区和日志归档）**

 

**46)    ****日志归档位置和格式的配置**

%t：线程号，%s：日志文件的序列号，%r：重设日志id

{ORACLE_BASE}/oradata/data2/{DB_NAME}/archive1

{ORACLE_BASE}/orabak/{DB_NAME}/archive2

 

**47)    ****配置数据库内容**

 

**48)    ****配置初试化参数选项－内存**

 

**49)    ****配置初试化参数选项－数据块大小，数据库并发用户连接线程数**

 

**50)    ****配置初试化参数－数据库字符集，国家字符集**

这是亚运会项目的特别需要，一般按以下选择安装

 

** **

**51)    ****配置初试化参数－连接模式，选择共享模式**

**52)    ****存储配置－控制文件位置**

 

**53)    ****存储配置－控制文件选项**

 

**54)    ****存储配置－数据文件**

 

**55)    ****数据库创建选项（生成创建数据库脚本）**

生成数据库脚本以便在创建另一节点个数据库的时候使用该脚本

 

**56)    ****检查并确认先前的配置选项**

 

**57)    ****开始创建数据库实例**

 

**58)    ****创建完成**

 

**59)    ****通过以下信息访问数据库**

 

http://devdb:5560/isqlplus

http://devdb:5560/isqlplus/dba

http://devdb:1158/em

如果不能解析机器名，用IP地址直接访问

http://130.120.2.158:5560/isqlplus

http://130.120.2.158:5560/isqlplus/dba

http://130.120.2.158:1158/em

 

[导入论坛](http://www.itpub.net/post.php?action=import&itemid=625729) [引用链接]() [收藏]() [分享给好友]() [推荐到圈子]() [管理](http://space.itpub.net/batch.manage.php?itemid=625729) [举报]()

TAG:
![]() [引用]() [删除]() 支持开源   /   2010-01-26 08:45:28 Oracle不开源，即使运行在linux上，也照样影响中国信息安全，要坚决抵制！

[查看全部评论]()

[-5]() [-3]() [-1]() [-]() [+1]() [+3]() [+5]()

评分：0

我来说两句

显示全部

![:loveliness:]() ![:handshake]() ![:victory:]() ![:funk:]() ![:time:]() ![:kiss:]() ![:call:]() ![:hug:]() ![:lol]() ![:'(]() ![:Q]() ![:L]() ![;P]() ![:$]() ![:P]() ![:o]() ![:@]() ![:D]() ![:(]() ![:)]()

内容

昵称

验证  ![seccode]( "看不清？点击换一个")

提交评论

![HankenGt]()

[HankenGt](http://space.itpub.net/22650465/action-viewpro-showpro-1)

### 用户菜单

* [给我留言](http://space.itpub.net/22650465/action-viewpro)
* [加入好友]()
* [发短消息](http://www.itpub.net/home.php?mod=spacecp&ac=pm&op=showmsg&handlekey=showmsg_22650465&touid=22650465&pmid=0&daterange=2)
* [我的介绍](http://space.itpub.net/22650465/action-viewpro-showpro-1)
* [论坛资料](http://www.itpub.net/space-uid-22650465.html)
* [空间管理](http://space.itpub.net/batch.manage.php?uid=22650465)
### 标题搜索

### 日历

[«](http://space.itpub.net/action-spacelist-uid-22650465-type-blog-starttime-1317398400-endtime-1320076800) [2011-11-14](http://space.itpub.net/action-spacelist-uid-22650465-type-blog-starttime-1320076800-endtime-1322668800)   日 一 二 三 四 五 六     1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30      
### 我的存档

* [2010年01月](http://space.itpub.net/22650465/action-spacelist-starttime-1262275200-endtime-1264953600)   
* [查看所有存档](http://space.itpub.net/22650465/action-spacelist-starttime-1259596799)

### 数据统计

* 访问量: 617
* 日志数: 1
* 建立时间: 2010-01-21
* 更新时间: 2010-01-21
### RSS订阅

* [![RSS订阅]()](http://space.itpub.net/22650465/action-rss-type-blog)

[清空Cookie](http://space.itpub.net/batch.login.php?action=logout) - [联系我们](mailto:admin@yourdomain.com) - [ITPUB个人空间](http://space.itpub.net/) - [交流论坛](http://www.itpub.net/) - [空间列表](http://space.itpub.net/action/spaces) - [站点存档](http://space.itpub.net/archiver/) - [升级自己的空间](http://space.itpub.net/action/register)

Powered by [**X-Space**](http://www.supesite.com/) *3.0.2* © 2001-2007 [Comsenz Inc.](http://www.comsenz.com/)
[京ICP证:010037号](http://www.miibeian.gov.cn/)[](http://www.miibeian.gov.cn/)[![我要啦免费统计]()](http://space.itpub.net/1711153)![]()
[Open Toolbar]()     ![]()
