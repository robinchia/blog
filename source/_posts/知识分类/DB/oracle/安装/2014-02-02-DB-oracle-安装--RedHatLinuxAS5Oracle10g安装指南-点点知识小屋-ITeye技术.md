---
layout: post
title: "RedHat Linux AS 5 Oracle10g安装指南 - 点点知识小屋 - ITeye技术"
categories: DB
tags: 
 - DB
 - oracle
 - 安装
--- 

# RedHat Linux AS 5 Oracle10g安装指南 - 点点知识小屋 - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [群组](http://www.iteye.com/groups) [更多 ▼](http://tsunzhang.iteye.com/blog/651079#)

[招聘](http://www.iteye.com/job) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://tsunzhang.iteye.com/login "登录") [登录](http://tsunzhang.iteye.com/login) [注册](http://tsunzhang.iteye.com/signup)

# [点点知识小屋](http://tsunzhang.iteye.com/)

永久域名 [http://tsunzhang.iteye.com/](http://tsunzhang.iteye.com/)

[oracle更改字符集步骤方法](http://tsunzhang.iteye.com/blog/652973 "oracle更改字符集步骤方法") | [Redhat Linux AS4(AS5)下oracle10g自启动 ...](http://tsunzhang.iteye.com/blog/651047 "Redhat Linux AS4(AS5)下oracle10g自启动脚本设置")

2010-04-23

### [RedHat Linux AS 5 Oracle10g安装指南](http://tsunzhang.iteye.com/blog/651079) **

**博客分类：**
* [linux,unix,windows](http://tsunzhang.iteye.com/category/104329)
[RedHat](http://www.iteye.com/blogs/tag/RedHat)[Linux](http://www.iteye.com/blogs/tag/Linux)[Oracle](http://www.iteye.com/blogs/tag/Oracle)[GCC](http://www.iteye.com/blogs/tag/GCC)[SQL Server](http://www.iteye.com/blogs/tag/SQL%20Server)
参数如下两个网址:
[http://www.club.zj.com/viewthread.php?tid=1127180](http://www.club.zj.com/viewthread.php?tid=1127180)
[http://bbs.chinaunix.net/thread-1035512-1-1.html](http://bbs.chinaunix.net/thread-1035512-1-1.html)
与网址1为准:
现结合两个网址,写Oracle10g如果在RHEL5下安装:
RHEL5 上 安装 Oracle 10.2.0.1
这两天在Red Hat Enterprise Linux 5 (RHEL5)上安装了Oracle 10g(10.2.0.1)
下载
可以从Oracle的主页上下载:
Oracle Database 10g Release 2 (10.2.0.1) Software
解压文件
解压下载好的文件:
**unzip 10201_database_linux32.zip**
你可以把他解压到一个目录中，例如 "db/Disk1" 或者 "database".
以root的身份完成下面的工作：
修改内核参数
**增加下面的内容到文件 /etc/sysctl.conf 中:**
kernel.shmall = 2097152
kernel.shmmax = 2147483648
kernel.shmmni = 4096
# semaphores: semmsl, semmns, semopm, semmni
kernel.sem = 250 32000 100 128
fs.file-max = 65536
net.ipv4.ip_local_port_range = 1024 65000
net.core.rmem_default=262144
net.core.rmem_max=262144
net.core.wmem_default=262144
net.core.wmem_max=262144
**运行下面的命令使得内核参数生效:**
/sbin/sysctl -p
**增加下面的内容到文件 /etc/security/limits.conf 文件中:**
* soft nproc 2047
* hard nproc 16384
* soft nofile 1024
* hard nofile 65536
**增加下面的内容到文件 /etc/pam.d/login 中:**
session required /lib/security/pam_limits.so
**因为SELINUX对oracle有影响，所以把secure linux设成无效，编辑文件 /etc/selinux/config :**
SELINUX=disabled
当然你也可以用图形界面下的工具 (系统 > 管理 > 安全级别和防火墙). 选择SELinux页面并且设为无效.
安装
**安装下面的包:**
# 从RedHat AS5 光盘1
cd /media/cdrom/Server
rpm -Uvh setarch-2*
rpm -Uvh make-3*
rpm -Uvh glibc-2*
rpm -Uvh libaio-0*
cd /
eject
# 从RedHat AS5 光盘2
cd /media/cdrom/Server
rpm -Uvh compat-libstdc++-33-3*
rpm -Uvh compat-gcc-34-3*
rpm -Uvh compat-gcc-34-c++-3*
rpm -Uvh gcc-4*
rpm -Uvh libXp-1*
cd /
eject
# 从RedHat AS5 光盘3
cd /media/cdrom/Server
rpm -Uvh openmotif-2*
rpm -Uvh compat-db-4*
cd /
eject
**新增组和用户:**
groupadd oinstall
groupadd dba
groupadd oper
useradd -g oinstall -G dba oracle
passwd oracle
**创建Oracle的安装目录，并把权限付给oracle用户:**
mkdir -p /u01/app/oracle/product/10.2.0/db_1
chown -R oracle.oinstall /u01
**因为oracle 的官方只支持到RHEL4为止，所以要修改版本说明，编辑文件 /etc/redhat-release 把Red Hat Enterprise Linux Server release 5 (Tikanga) 改成版本4，当然oracle安装完成后,要修改回来:**
redhat-4
**登录到oracle 用户并且配置环境变量(增加下面的内容到文件 .bash_profile**
# Oracle Settings
TMP=/tmp; export TMP
TMPDIR=$TMP; export TMPDIR
ORACLE_BASE=/u01/app/oracle; export ORACLE_BASE
ORACLE_HOME=$ORACLE_BASE/product/10.2.0/db_1; export ORACLE_HOME
ORACLE_SID=orcl; export ORACLE_SID
ORACLE_TERM=xterm; export ORACLE_TERM
PATH=$PATH:$ORACLE_HOME/bin; export PATH
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$ORACLE_HOME/lib; export LD_LIBRARY_PATH
CLASSPATH=$ORACLE_HOME/JREORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib; export CLASSPATH
if [ $USER = "oracle" ]; then
if [ $SHELL = "/bin/ksh" ]; then
ulimit -p 16384
ulimit -n 65536
else
ulimit -u 16384 -n 65536
fi
fi
**修改/etc/hosts.conf**
把127.0.0.1改为具体的ip地址,如(192.168.5.253)，注意最好去掉那些无用的，格式就是

ip地址   主机名   localhost

**特殊处理,如果没有下面这些步骤,oracle在安装时,可能出现问题**
#vi /etc/inittab
把 id:5:initdefault: 修改为 id:3:initdefault 等oracle安装完成后,可以修改回来
#reboot(重启)
在文本模式下 用root登录
# startx
# xhost +
# su - oracle
$ export DISPLAY="192.168.1.253:0.0"
$ export LANG=en_US
$ cd /tmp/10201_database_linux32/databases
$ ./runInstaller
**安装时要注意:在安装到最后处理sqlplus时,系统会要求切换用户root上,执行两相script.**
**备注:**
1。为了让其他计算机能够访问，必须把下面端口打开，端口1521(用于连接数据库)，端口1158(如果要用浏览器访问enterprise managment)，端口5560(如果要用浏览器访问isqlplus)。你可以用图形界面下的工具 (系统 > 管理 > 安全级别和防火墙). 选择防火墙页面，并且增加上面的端口。
2。如果想开机时自动启动oracle的话，还需另外配置自动启动的脚本。
**启动oracle**

 

su oracle

cd /u01/app/oracle/product/10.2.0/db_1/bin
1.调用./lsnrctl service(可以查看当前监听器服务情况)
2.调用./lsnrctl start(启动监听器),如想停用则lsnrctl stop
判断监听器服务是否好用,可以使用./tnsping ip地址.如果不能正常结束,则说明监听有问题.
3.调用./sqlplus "/as sysdba"
4.start 开启数据库.
**自动启动oracle**
1.修改了/etc/oratab 将N改为Y
2.在su - oracle 主目录下 编辑 vi .bash_profile
修改oracle_home
oracle_sid
3.修改/etc/rc.local
su - oracle -c 'lsnrctl start'
su - oracle -c 'dbstart'
修改ORACLE_HOME/bin下面的dbstart 修改oratab=/etc/oratab
/etc下面没有oratab文件的话

 

**关于数据库删除重新安装的问题**
把ORACLE安装目录删除及/etc/ora*.*删除就行了

 

**关于oracle database备份**
(1)vi bachupDb.sh
#!/bin/sh
#oracle用户下
#crontab -e 增加 "35 4 * * * /home/oracle/dbbackup/backupDb.sh",保存后自动安装
#或echo "35 4 * * * /home/oracle/dbbackup/backupDb.sh" > backupDb.cron
#crontab backupDb.cron
#############
#@tip 修改为本机数据库home目录
export ORACLE_HOME=/opt/oracle/product/10g
export PATH=$ORACLE_HOME/bin:$PATH:$HOME/bin
# 注意字符集必须和数据库的字符集一致，以避免字符集转化失败
export NLS_LANG=AMERICAN_AMERICA.zhs16gbk
#@tip 125修改为要备份的oracle的ip地址的最后一段
dmpfile="`echo ~/`dbbackup/gedb_`date +%w`.dmp"
logfile="`echo ~/`dbbackup/gedb_`date +%w`.log"

if [ -w $dmpfile ]
then
  echo "rm -f $dmpfile"
  rm -f "$dmpfile"
fi

#@tip ip地址修改为要备份的oracle的主机地址
exp USERID=gedb/gedb@10.248.1.5/ge01 file=$dmpfile log=$logfile  owner=gedb grants=y
  (2)copy bachupDb.sh 到slave oracle srever 相应目录,
     chown oracle.oinstall bachupDb.sh
     chmod 744 bachupDb.sh   
     vi bachupDb.sh 以符合安装情况
(3)以oracle user role
    crontab -e
    35 4 * * * /home/oracle/dbbackup/backupDb.sh
9. restore oracle backup
su - oracle
imp USERID=gedb/gedb file=gedb_6.dmp log=implogfile  commit=y  grants=y full=y 

 

注意：

最好在安装oracle时不要创建数据库，只安装oracle基本系统。系统安装好后用$ORACLE_HOME/bin/dbca,命令创建数据库，创建数据库时我们可以选择针对数据库的各种参数如“字符集”等。

 

 

===========================================================================

1.安装JDK
http://java.sun.com
(1) 下载后的BIN文件可以直接执行
# chmod 755 jdk-1.6.0_23-linux-i586.rpm.bin
# ./ jdk-1.6.0_23-linux-i586.rpm.bin
此步完成后，会生成jdk-1.6.0_23-linux-i586.rpm的文件
默认安装到了/usr/java/jdk1.6.0_23
(2) /etc/profile  设置环境变量
增加如下内容：
JAVA_HOME=/usr/java/jdk1.6.0_23
JRE_HOME=/usr/java/jdk1.6.0_23/jre
PATH=$PATH:$JAVA_HOME/bin:JRE_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/jt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
export JAVA_HOME JRE_HOME PATH CLASSPATH
按Esc,然后:wq保存退出
使环境变量生效 source /etc/profile
查看:  echo $JAVA_HOME (会显示JDK所在目录)
***********************************************************************************************************
2.检查安装包
make-3.79.1
gcc-3.2.3-34
glibc-2.3.2-95.20
glibc-devel-2.5-12.i386.rpm
glibc-headers-2.5-12.i386.rpm
compat-db-4.0.14-5
compat-gcc-7.3-2.96.128
compat-gcc-c++-7.3-2.96.128 compat-libstdc++-7.3-2.96.128
compat-libstdc++-devel-7.3-2.96.128
libXpm-3.5.5-3.i386.rpm libXp
openmotif21-2.1.30-8 setarch-1.3-1
libgomp-4.1.1-52.el5.i386.rpm
查询所需安装包是否完整
rpm -q gcc make binutils openmotif setarch compat-db compat-gcc compat-gcc-c++ compat-libstdc++ compat-libstdc++-devel libXp
由于缺失的包之间有严格的依赖关系，所以必须按照如下顺序安装缺失的包
rpm -Uvh compat-db-4*
rpm -Uvh libaio-0*
rpm -Uvh compat-libstdc++-33-3*
rpm -Uvh glibc-headers-2.5-12.i386.rpm
rpm -Uvh glibc-devel-2.5-12.i386.rpm
rpm -Uvh compat-gcc-34-3*
rpm -Uvh compat-gcc-34-c++-3*
rpm -Uvh libXp-1*
rpm -Uvh openmotif-2*
rpm -Uvh gcc-4*
rpm -Uvh glibc-2.5-12.i686.rpm
rpm -Uvh libgomp-4.1.1-52.el5.i386.rpm
rpm -Uvh gcc-4.1.1-52.el5.i386.rpm
安装完成后仍然提示部分包没有安装，不过不影响使用
package compat-gcc is not installed
package compat-gcc-c++ is not installed
package compat-libstdc++ is not installed
package compat-libstdc++-devel is not installed
另一种说法：
查询所需安装包是否完整
rpm -q gcc make binutils openmotif setarch libXp
而对于需要安装的包，按如下关键字搜索和安装即可 compat -> libXp -> openmotif 全部安装完毕即可(我是这样做的)
***********************************************************************************************************
3.增加Oracle安装和使用的用户
(1) 新增组和用户
groupadd oinstall
groupadd dba
groupadd oper
useradd -g oinstall -G dba oracle
passwd oracle
(2) 创建Oracle的安装目录，并把权限付给oracle用户,其实创建用户后就已经有该文件了
mkdir -p /home/oracle/
chown -R oracle:oinstall /home/oracle
chmod -R 775 /home/oracle
***********************************************************************************************************
4.修改配置文件
(1) /etc/sysctl.conf  行末添加以下内容，已有的修改
kernel.shmall = 2097152
kernel.shmmax = 2147483648
kernel.shmmni = 4096
# semaphors: semmsl, semmns, semopm, semmni
kernel.sem = 250 32000 100 128
fs.file-max = 65536
net.ipv4.ip_local_port_range = 1024 65000
net.core.rmem_default=262144
net.core.rmem_max=262144
net.core.wmem_default=262144
net.core.wmem_max=262144
运行下面的命令使得内核参数生效
/sbin/sysctl -p
(2) /etc/security/limits.conf  行末添加以下内容
#use for oracle
* soft nproc 2047
* hard nproc 16384
* soft nofile 1024
* hard nofile 65536
(3) /etc/pam.d/login  行末添加以下内容
session required pam_limits.so
(4) /etc/selinux/config
更改 SELINUX=disabled 关闭防火墙，必须的
(5) /etc/redhat-release  Linux版本信息，5不支持Oracle，安装后可以改回去
Red Hat Enterprise Linux AS release 3 (Taroon)
或Red Hat Enterprise Linux AS release 4 (Nahant Update 4)
(6) gedit /etc/profile 就是增加JDK配置的文件，在增加JDK配置后紧接着增加如下内容
if [ $USER = "oracle" ];then
if [ $SHELL = "/bin/ksh" ]; then
ulimit -p 16384
ulimit -n 65536
else
ulimit -u 16384 -n 65536
fi
fi
(6) bash_profile 在创建用户后在用户的目录下有一个.bash_profile(使用Oracle用户)
并在文件中增加如下内容
(ORACLE_BASE是最重要的，他代表Oracle的安装路径)
(在安装时就可以创建数据库，如果安装完毕重启，则再启动监听时无法启动，则要注意ORACLE_HOME在数据库安装后要根据实际路径进行修改)
ORACLE_BASE=/home/oracle/oracle
ORACLE_HOME=$ORACLE_BASE/product/10.2.0/db_1
ORACLE_SID=CUI
PATH=$PATH:$HOME/bin:$ORACLE_HOME/bin
LD_LIBRARY_PATH=$ORACLE_HOME/lib:/usr/lib
export ORACLE_BASE ORACLE_HOME ORACLE_SID PATH LD_LIBRARY_PATH
***********************************************************************************************************
5.解压(使用Oracle用户)
unzip 10201_database_linux32.zip -d /tmp/oracle
改权限
chown oracle /tmp/oracle
chmod -R  755 /tmp/oracle
安装
到根目录下：./runInstaller
(如果安装时不创建数据库，可以在Oracle_HOME/bin 下运行 dbca 来创建和管理数据库)
***********************************************************************************************************
6.配置Oracle在Linux下的命令
(1) 修改Rehhat版本信息
/etc/redhat-release 将版本改为原来版本
(2) 启动数据库与监听
/etc/oratab
SID名字:/Oracle/app/product/10.2.0/db_1:N为
oracle:/Oracle/app/product/10.2.0/db_1:Y
$Oracle_HOME/bin/dbstart
把其中的Oracle_HOME_LISTNER=什么东西，注释掉
加上    Oracle_HOME_LISTNER=$Oracle_HOME
修改/增加配置文件，起名字叫oracle,添加下面的script
(如果.bash_profile文件中配置过的话，就把export注销)
===== Script ====
#!/bin/bash
#
# chkconfig: 35 95 1
# description: init script to start/stop oracle database 10g, TNS listener, EMS
# match these values to your environment:
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/10.2.0/db_1
export ORACLE_TERM=xterm
export PATH=/u01/app/oracle/bin:$ORACLE_HOME/bin:$PATH
export ORACLE_SID=oracle
export DISPLAY=localhost:0
export ORACLE_USER=oracle
# see how we are called:
case $1 in
start)
su - "$ORACLE_USER"<<EOO
lsnrctl start
sqlplus /nolog<<EOS
connect / as sysdba
startup
EOS
emctl start dbconsole
EOO
touch /var/lock/subsys/$scriptname
;;
stop)
su - "$ORACLE_USER"<<EOO
lsnrctl stop
sqlplus /nolog<<EOS
connect / as sysdba
shutdown immediate
EOS
emctl stop dbconsole
EOO
rm -f /var/lock/subsys/scriptname
;;
*)
echo "Usage: $0 {start|stop}"
;;
esac
===========end of script==============
授权
chown root:root /etc/rc.d/init.d/oracle
chmod 755 /etc/rc.d/init.d/oracle
(3) 启动/关闭服务
service oracle start / service oracle stop
(有可能启动会报syntax error: unexpected end of file错)
(这是因为回车的问题,你用vi把它去掉。在windows里，换行用的两个符号，回车符\r换行符\n；在linux下只需一个符号\n就可以了)
***********************************************************************************************************
附(一):卸载（简单，全是rm）
1)使用SQL*PLUS停止数据库
$ sqlplus /nolog
SQL> connect / as sysdba
SQL> shutdown [immediate]
SQL> exit
2)停止Listener
$ lsnrctl stop
3)停止HTTP服务
$ $ORACLE_HOME/Apache/Apache/bin/apachectl stop
4)用su或者重新登录到root
(1)运行 $ORACLE_HOME/bin/localconfig delete
(2)# rm -rf $ORACLE_BASE/*
(3)# rm -f /etc/oraInst.loc /etc/oratab
(4)# rm -rf /etc/oracle
(5)# rm -f /etc/inittab.cssd
(6)# rm -f /usr/local/bin/coraenv
(7)# rm -f /usr/local/bin/dbhome
(8)# rm -f /usr/local/bin/oraenv
(9)删除oracle用户和组
userdel –r oracle
groupdel oinstall
groupdel dba
(10)将启动服务删除
chkconfig --del dbora
附(二):正常模式启动和关闭数据库
9i 之后已经没有 svrmgrl 了，所有的管理工作都通过 sqlplus 来完成
启动数据库步骤如下:
注:$ORACLE_HOME为oracle的安装路径
1,以oracle用户登录
su oracle
2,启动TNS监听器
$ORACLE_HOME/bin/lsnrctl start
3,用sqlplus启动数据库
$ORACLE_HOME/bin/sqlplus /nolog
SQL> connect system/change_on_install as sysdba
SQL> startup
出现如下显示，表示Oracle已经成功启动
ORACLE instance started.
Total System Global Area  205520896 bytes
Fixed Size                   778392 bytes
Variable Size              74456936 bytes
Database Buffers          130023424 bytes
Redo Buffers                 262144 bytes
Database mounted.
Database opened.
4,用sqlplus停止数据库
$ORACLE_HOME/bin/sqlplus /nolog
SQL> connect system/change_on_install as sysdba
SQL> shutdown
注：shutdown可加关闭选项,从最温和到最粗暴的行为选项为(shutdown、shutdown transactional、shutdown immediate、shutdown abort)
命令解释如下
shutdown:关闭，等待每个用户退出系统戓被取消后退出关闭数据库
shutdown transactional:事务性关闭，等待每个用户提交戓回退当前的事务，然后oracle取消对话，在所有用户退出系统后执行关闭
shutdown immediate:直接关闭，取消所有用户对话（促使回退），执行正常的关闭程序
shutdown abort:终止关闭，关闭数据库时没有自动检查点戓日志开关
出现如下显示,表示oracle已经停止
Database closed
Database dismounted
ORACLE instance shut down

* [【原创】Oracle在RHEL4或5下安装指南.pdf](http://dl.iteye.com/topics/download/3a252eec-f68f-38f8-9cb6-9536dae43b54) (1.2 MB)
* 下载次数: 57
分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[oracle更改字符集步骤方法](http://tsunzhang.iteye.com/blog/652973 "oracle更改字符集步骤方法") | [Redhat Linux AS4(AS5)下oracle10g自启动 ...](http://tsunzhang.iteye.com/blog/651047 "Redhat Linux AS4(AS5)下oracle10g自启动脚本设置")
* 11:18
* [评论](http://tsunzhang.iteye.com/blog/651079#comments) / 浏览 (0 / 608)
* 分类:[数据库](http://www.iteye.com/blogs/category/database)
* [相关推荐](http://www.iteye.com/wiki/blog/651079)

### 评论

[]()
### 发表评论

[![]()](http://tsunzhang.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://tsunzhang.iteye.com/login)

[![coconut_zhang的博客]( "coconut_zhang的博客: 点点知识小屋")](http://tsunzhang.iteye.com/)

coconut_zhang

* 浏览: 56419 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 天津
* ![]()
* [详细资料](http://tsunzhang.iteye.com/blog/profile) [留言簿](http://tsunzhang.iteye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://tsunzhang.iteye.com/blog/user_visits)

[![gatesanye的博客]( "gatesanye的博客: ")](http://gatesanye.iteye.com/)

[gatesanye](http://gatesanye.iteye.com/)

[![olo的博客]( "olo的博客: ")](http://han-zc-126-com.iteye.com/)

[olo](http://han-zc-126-com.iteye.com/)
[![baungham的博客]( "baungham的博客: ")](http://baungham.iteye.com/)

[baungham](http://baungham.iteye.com/)

[![a307487540的博客]( "a307487540的博客: ")](http://a307487540.iteye.com/)

[a307487540](http://a307487540.iteye.com/)

### 博客分类

* [全部博客 (120)](http://tsunzhang.iteye.com/)
* [hibernate,struts,spring (9)](http://tsunzhang.iteye.com/category/60979)
* [java (27)](http://tsunzhang.iteye.com/category/60981)
* [c,c++,c# (39)](http://tsunzhang.iteye.com/category/60982)
* [eclipse,myeclipse,lomboz (2)](http://tsunzhang.iteye.com/category/60983)
* [tomcat,weblogic (2)](http://tsunzhang.iteye.com/category/60984)
* [javascript,ajax,web (4)](http://tsunzhang.iteye.com/category/60985)
* [oracle,sqlserver,mysql,sql (22)](http://tsunzhang.iteye.com/category/60980)
* [linux,unix,windows (11)](http://tsunzhang.iteye.com/category/104329)
* [文摘 (2)](http://tsunzhang.iteye.com/category/61096)
* [生活 (0)](http://tsunzhang.iteye.com/category/104330)
* [javascript (1)](http://tsunzhang.iteye.com/category/183172)
* [ajax (1)](http://tsunzhang.iteye.com/category/183173)
* [web (1)](http://tsunzhang.iteye.com/category/183174)
### 我的相册

[![]()](http://tsunzhang.iteye.com/album)P1010287
[共 10 张](http://tsunzhang.iteye.com/album)

### 我的留言簿 [>>更多留言](http://tsunzhang.iteye.com/blog/guest_book)

* 朋友，想向你请教问题，关于这个applet数字签名的，QQ286358399.
-- by [fke286358399](http://tsunzhang.iteye.com/blog/guest_book#35998)
### 其他分类

* [我的收藏](http://tsunzhang.iteye.com/blog/favorite) (6)
* [我的代码](http://tsunzhang.iteye.com/blog/code_favorite) (0)
* [我的论坛主题帖](http://tsunzhang.iteye.com/blog/topic) (1)
* [我的所有论坛帖](http://tsunzhang.iteye.com/blog/post) (217)
* [我的精华良好帖](http://tsunzhang.iteye.com/blog/article) (0)

### 最近加入群组
### 存档

* [2011-10](http://tsunzhang.iteye.com/blog/monthblog/2011-10) (2)
* [2011-07](http://tsunzhang.iteye.com/blog/monthblog/2011-07) (1)
* [2011-05](http://tsunzhang.iteye.com/blog/monthblog/2011-05) (4)
* [更多存档...](http://tsunzhang.iteye.com/blog/monthblog_more)

### 评论排行榜

* [Struts2拦截器执行顺序](http://tsunzhang.iteye.com/blog/811566 "Struts2拦截器执行顺序")
* [关于shell脚本中报 “/bin/sh^M: bad int ...](http://tsunzhang.iteye.com/blog/1064298 "关于shell脚本中报 “/bin/sh^M: bad interpreter: 没有那个文件或目录”的解决方法")
* [![Rss]()](http://tsunzhang.iteye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://tsunzhang.iteye.com/rss)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
