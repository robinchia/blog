---
layout: post
title: "Redhat 5 下 Oracle10g 安装，相信没有比这个更全的了 - Linux - Tech"
categories: DB
tags: 
 - DB
 - oracle
 - 安装
--- 

# Redhat 5 下 Oracle10g 安装，相信没有比这个更全的了 - Linux - Tech - ITeye论坛

[您还未登录 !](http://www.iteye.com/login "登录") [登录](http://www.iteye.com/login) [注册](http://www.iteye.com/signup)

[![ITeye-最棒的软件开发交流社区]( "ITeye-最棒的软件开发交流社区")](http://www.iteye.com/)

[![]()](http://www.iteye.com/clicks/678)

[论坛首页](http://www.iteye.com/forums) → [综合技术版](http://www.iteye.com/forums/board/Tech) → [Linux](http://www.iteye.com/forums/tag/Linux) →

# Redhat 5 下 Oracle10g 安装，相信没有比这个更全的了

[全部](http://www.iteye.com/forums/board/Tech) [Database](http://www.iteye.com/forums/tag/Database) [Haskell](http://www.iteye.com/forums/tag/Haskell) [Erlang](http://www.iteye.com/forums/tag/Erlang) [FP](http://www.iteye.com/forums/tag/FP) [Linux](http://www.iteye.com/forums/tag/Linux) [数据结构和算法](http://www.iteye.com/forums/tag/algorithm) [mysql](http://www.iteye.com/forums/tag/mysql) [oracle](http://www.iteye.com/forums/tag/OracleDB) [DB2](http://www.iteye.com/forums/tag/DB2) [SQLServer](http://www.iteye.com/forums/tag/SQLServer) [PostgreSQL](http://www.iteye.com/forums/tag/PostgreSQL) [MacOSX](http://www.iteye.com/forums/tag/MacOSX) [Unix](http://www.iteye.com/forums/tag/Unix) [编程综合](http://www.iteye.com/forums/tag/Technology) [OS](http://www.iteye.com/forums/tag/OS)
[](http://www.iteye.com/forums/43/topics/846536/posts/new "发表回复")

[最成熟稳定甘特图控件,支持Java和.Net](http://www.iteye.com/clicks/593)

« 上一页 1 [2](http://www.iteye.com/topic/846536?page=2) [3](http://www.iteye.com/topic/846536?page=3) [下一页 »](http://www.iteye.com/topic/846536?page=2)
浏览 17684 次
 [主题：Redhat 5 下 Oracle10g 安装，相信没有比这个更全的了](http://www.iteye.com/topic/846536)

精华帖 (0) :: 良好帖 (0) :: 新手帖 (0) :: 隐藏帖 (1) 作者 正文 * cuisuqiang
* 等级: 初级会员
* [![cuisuqiang的博客]( "cuisuqiang的博客: 风中落叶")](http://cuisuqiang.iteye.com/)
* 性别: ![]( "男")
* 文章: 30
* 积分: 60
* 来自: 北京
* ![]()
    发表时间：2010-12-19  

[<](http://www.iteye.com/topic/846536#) [>](http://www.iteye.com/topic/846536#) 猎头职位:

相关文章: [ ](http://www.iteye.com/topic/846536# "关闭")

* [RHAS4安装oracle10g步骤](http://www.iteye.com/topic/48315 "RHAS4安装oracle10g步骤")
* [oracle9i installation on fedora core 6](http://www.iteye.com/topic/39577 "oracle9i installation on fedora core 6 ")
* [Redhat9.2 安装ORACLE 10.1g](http://www.iteye.com/topic/78485 "Redhat9.2 安装ORACLE 10.1g ")
推荐群组: [Groovy on Grails](http://grails.group.iteye.com/)
[更多相关推荐](http://www.iteye.com/wiki/topic/846536)

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
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。

推荐链接

*
* [20-30万急聘多名天才Java/MTA软件工程师](http://www.iteye.com/clicks/617)
* [3G培训就业月薪平均7K+，不3K就业不花一分钱！](http://www.iteye.com/clicks/508)
* [见证又一个准百万富翁的诞生！](http://www.iteye.com/clicks/527) [返回顶楼](http://www.iteye.com/topic/846536#) [ ](http://cuisuqiang.iteye.com/ "浏览作者的博客") [ ](http://cuisuqiang.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=cuisuqiang "发送站内短信") [ ](http://cuisuqiang.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=cuisuqiang "关注作者")  * genggeng
* 等级: 初级会员
* [![genggeng的博客]( "genggeng的博客: gavin的技术博客 java python nosql 分布式 缓存 ")](http://genggeng.iteye.com/)
* 性别: ![]( "男")
* 文章: 53
* 积分: 70
* 来自: 北京
* ![]()
    发表时间：2010-12-19  

shutdown immediate; [返回顶楼](http://www.iteye.com/topic/846536#) [ ](http://genggeng.iteye.com/ "浏览作者的博客") [ ](http://genggeng.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=genggeng "发送站内短信") [ ](http://genggeng.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=genggeng "关注作者") [回帖地址](http://www.iteye.com/topic/846536#1809714 "genggeng回帖:Redhat 5 下 Oracle10g 安装，相信没有比这个更全的了")

[0](http://www.iteye.com/topic/846536# "好") [0](http://www.iteye.com/topic/846536# "差") 请登录后投票  * cuisuqiang
* 等级: 初级会员
* [![cuisuqiang的博客]( "cuisuqiang的博客: 风中落叶")](http://cuisuqiang.iteye.com/)
* 性别: ![]( "男")
* 文章: 30
* 积分: 60
* 来自: 北京
* ![]()
    发表时间：2010-12-19  

这些命令的话，我想不用那么啰嗦吧！如果需要可以到网上查查，我不保证我的方法都是最好的，希望大家提出意见 [返回顶楼](http://www.iteye.com/topic/846536#) [ ](http://cuisuqiang.iteye.com/ "浏览作者的博客") [ ](http://cuisuqiang.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=cuisuqiang "发送站内短信") [ ](http://cuisuqiang.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=cuisuqiang "关注作者") [回帖地址](http://www.iteye.com/topic/846536#1810087 "cuisuqiang回帖:Redhat 5 下 Oracle10g 安装，相信没有比这个更全的了")

[0](http://www.iteye.com/topic/846536# "好") [0](http://www.iteye.com/topic/846536# "差") 请登录后投票  * mathgl
* 等级: 初级会员
* [![mathgl的博客]( "mathgl的博客: mathgl")](http://mathgl.iteye.com/)
* 性别: ![]( "男")
* 文章: 941
* 积分: 50
* 来自: HK
* ![]()
    发表时间：2010-12-20  

cuisuqiang 写道

这些命令的话，我想不用那么啰嗦吧！如果需要可以到网上查查，我不保证我的方法都是最好的，希望大家提出意见
oracle在非认证过的linux版本下装确实有些啰嗦，找到好几个版本的安装流程，还不怎么一样... [返回顶楼](http://www.iteye.com/topic/846536#) [ ](http://mathgl.iteye.com/ "浏览作者的博客") [ ](http://mathgl.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=mathgl "发送站内短信") [ ](http://mathgl.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=mathgl "关注作者") [回帖地址](http://www.iteye.com/topic/846536#1810794 "mathgl回帖:Redhat 5 下 Oracle10g 安装，相信没有比这个更全的了")

[1](http://www.iteye.com/topic/846536# "好") [0](http://www.iteye.com/topic/846536# "差") 请登录后投票  * cuisuqiang
* 等级: 初级会员
* [![cuisuqiang的博客]( "cuisuqiang的博客: 风中落叶")](http://cuisuqiang.iteye.com/)
* 性别: ![]( "男")
* 文章: 30
* 积分: 60
* 来自: 北京
* ![]()
    发表时间：2010-12-20  

mathgl 写道

cuisuqiang 写道

这些命令的话，我想不用那么啰嗦吧！如果需要可以到网上查查，我不保证我的方法都是最好的，希望大家提出意见
oracle在非认证过的linux版本下装确实有些啰嗦，找到好几个版本的安装流程，还不怎么一样...
这是Redhat5下Oracle10G的安装流程，其实其他版本的话，大同小异，可以再参考一下网上其他人的。不过相对于这个版本的来说，我觉得够全了吧 [返回顶楼](http://www.iteye.com/topic/846536#) [ ](http://cuisuqiang.iteye.com/ "浏览作者的博客") [ ](http://cuisuqiang.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=cuisuqiang "发送站内短信") [ ](http://cuisuqiang.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=cuisuqiang "关注作者") [回帖地址](http://www.iteye.com/topic/846536#1811273 "cuisuqiang回帖:Redhat 5 下 Oracle10g 安装，相信没有比这个更全的了")

[0](http://www.iteye.com/topic/846536# "好") [0](http://www.iteye.com/topic/846536# "差") 请登录后投票  * 伤心雨
* 等级: 初级会员
* [![伤心雨的博客]( "伤心雨的博客: ")](http://wang25262875-gmail-com.iteye.com/)
* 性别: ![]( "男")
* 文章: 28
* 积分: 0
* 来自: 北京
* ![]()
    发表时间：2010-12-23  

挺全的。。。
但是。。。真的需要安装JDK吗？ [返回顶楼](http://www.iteye.com/topic/846536#) [ ](http://wang25262875-gmail-com.iteye.com/ "浏览作者的博客") [ ](http://wang25262875-gmail-com.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=%E4%BC%A4%E5%BF%83%E9%9B%A8 "发送站内短信") [ ](http://wang25262875-gmail-com.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=%E4%BC%A4%E5%BF%83%E9%9B%A8 "关注作者") [回帖地址](http://www.iteye.com/topic/846536#1816005 "伤心雨回帖:Redhat 5 下 Oracle10g 安装，相信没有比这个更全的了")

[0](http://www.iteye.com/topic/846536# "好") [0](http://www.iteye.com/topic/846536# "差") 请登录后投票  * mathgl
* 等级: 初级会员
* [![mathgl的博客]( "mathgl的博客: mathgl")](http://mathgl.iteye.com/)
* 性别: ![]( "男")
* 文章: 941
* 积分: 50
* 来自: HK
* ![]()
    发表时间：2010-12-23  

伤心雨 写道

挺全的。。。
但是。。。真的需要安装JDK吗？
oracle那东西 安装还要swing呢... [返回顶楼](http://www.iteye.com/topic/846536#) [ ](http://mathgl.iteye.com/ "浏览作者的博客") [ ](http://mathgl.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=mathgl "发送站内短信") [ ](http://mathgl.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=mathgl "关注作者") [回帖地址](http://www.iteye.com/topic/846536#1816183 "mathgl回帖:Redhat 5 下 Oracle10g 安装，相信没有比这个更全的了")

[0](http://www.iteye.com/topic/846536# "好") [0](http://www.iteye.com/topic/846536# "差") 请登录后投票  * cuisuqiang
* 等级: 初级会员
* [![cuisuqiang的博客]( "cuisuqiang的博客: 风中落叶")](http://cuisuqiang.iteye.com/)
* 性别: ![]( "男")
* 文章: 30
* 积分: 60
* 来自: 北京
* ![]()
    发表时间：2010-12-23  

伤心雨 写道

挺全的。。。
但是。。。真的需要安装JDK吗？
我流程里面已经写了吧！ [返回顶楼](http://www.iteye.com/topic/846536#) [ ](http://cuisuqiang.iteye.com/ "浏览作者的博客") [ ](http://cuisuqiang.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=cuisuqiang "发送站内短信") [ ](http://cuisuqiang.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=cuisuqiang "关注作者") [回帖地址](http://www.iteye.com/topic/846536#1816866 "cuisuqiang回帖:Redhat 5 下 Oracle10g 安装，相信没有比这个更全的了")

[0](http://www.iteye.com/topic/846536# "好") [0](http://www.iteye.com/topic/846536# "差") 请登录后投票  * cuisuqiang
* 等级: 初级会员
* [![cuisuqiang的博客]( "cuisuqiang的博客: 风中落叶")](http://cuisuqiang.iteye.com/)
* 性别: ![]( "男")
* 文章: 30
* 积分: 60
* 来自: 北京
* ![]()
    发表时间：2010-12-23  

mathgl 写道

伤心雨 写道

挺全的。。。
但是。。。真的需要安装JDK吗？
oracle那东西 安装还要swing呢...
如果你有那个能力得话，可以考虑纯命令方式，不过我不会 [返回顶楼](http://www.iteye.com/topic/846536#) [ ](http://cuisuqiang.iteye.com/ "浏览作者的博客") [ ](http://cuisuqiang.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=cuisuqiang "发送站内短信") [ ](http://cuisuqiang.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=cuisuqiang "关注作者") [回帖地址](http://www.iteye.com/topic/846536#1816867 "cuisuqiang回帖:Redhat 5 下 Oracle10g 安装，相信没有比这个更全的了")

[0](http://www.iteye.com/topic/846536# "好") [0](http://www.iteye.com/topic/846536# "差") 请登录后投票  * 伤心雨
* 等级: 初级会员
* [![伤心雨的博客]( "伤心雨的博客: ")](http://wang25262875-gmail-com.iteye.com/)
* 性别: ![]( "男")
* 文章: 28
* 积分: 0
* 来自: 北京
* ![]()
    发表时间：2010-12-24   最后修改：2010-12-24

cuisuqiang 写道

mathgl 写道

伤心雨 写道

挺全的。。。
但是。。。真的需要安装JDK吗？
oracle那东西 安装还要swing呢...
如果你有那个能力得话，可以考虑纯命令方式，不过我不会
我教你。
1 首先安装必要工具包
mount /dev/cdrom /media/
cd /mnt/cdrom/Server/
rpm -Uvh setarch-2
rpm -Uvh make-3
rpm -Uvh glibc-2
rpm -Uvh libaio-0
rpm -Uvh compat-libstdc++-33-3
rpm -Uvh compat-gcc-34-3
rpm -Uvh compat-gcc-34-c++-3
rpm -Uvh gcc-4
rpm -Uvh libXp-1
rpm -Uvh openmotif-2
rpm -Uvh compat-db-4
编辑 /etc/hosts。文件应当包含类似以下的文本：
127.0.0.1      localhost.localdomain    localhost
192.168.203.11 stctestbox01.us.oracle.com stctestbox01
2 更改修改/etc/redhat-release文件，因为Oracle10g数据库暂不支持RHEL5：
# vi /etc/redhat-release
# Red Hat Enterprise Linux Server release 5.2 (Tikanga)
redhat-4
3 Oracle数据库必须在Oracle用户下才能安装。故，建立相应的用户群组、用户，以及设置相应的目录属主
、目录权限。切记，要给Oracle用户设置密码哦，同时，密码要符合复杂性要求，譬如：weiguo520.。
groupadd oinstall
groupadd dba
groupadd oper
useradd -g oinstall -G dba oracle
mkdir -p /opt/oracle/or10g
chown -R oracle.oinstall /opt/oracle
chmod -R 775 /opt/oracle
passwd oracle
4 配置内核相关参数，以便支持Oracle数据库。
# vim /etc/sysctl.conf
# For Oracle
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
fs.file-max = 65536
net.ipv4.ip_local_port_range = 1024 65000
net.core.rmem_default = 262144
net.core.rmem_max = 262144
net.core.wmem_default = 262144
net.core.wmem_max = 262144
5 设置Oracle用户Shell limit。
# vim /etc/security/limits.conf
# For Oracle
oracle soft nproc 2047
oracle hard nproc 16384
oracle soft nofile 1024
oracle hard nofile 65536
6 在/etc/pam.d/login file文件末端添加相关内容，如果它已经存在则退出。
# vim /etc/pam.d/login
# For Oracle
session    required     /lib/security/pam_limits.so
7 修改Oracle用户语言环境，注销掉root用户，以oracle用户登录系统。
$ touch .i18n
$ vi .i18n
export LC_CTYPE="US_en"
也可以不执行。但是在安装过程中在命令行执行export LC_CTYPE="US_en"
8 配置Oracle用户环境变量，以便支持Oracle数据库安装以及今后的操作、维护。
$ vim .bash_profile
# For Oracle
TMP=/tmp; export TMP
TMPDIR=$TMP; export TMPDIR
ORACLE_BASE=/opt/oracle; export ORACLE_BASE   #自己的路径oracle安装路径的上级路径
ORACLE_HOME=$ORACLE_BASE/or10g; export ORACLE_HOME   #自己的oracle安装路径
ORACLE_SID=orcl; export ORACLE_SID     #自己的 数据库实例
ORACLE_TERM=xterm; export ORACLE_TERM
PATH=/usr/sbin:$PATH; export PATH
PATH=$ORACLE_HOME/bin:$PATH; export PATH
LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib; export LD_LIBRARY_PATH
CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib; export CLASSPATH
if [ $USER = "oracle" ]; then
       if [ $SHELL = "/bin/ksh" ]; then
             ulimit -p 16384
             ulimit -n 65536
       else
             ulimit -u 16384 -n 65536
       fi
fi
9 启动安装，首先运行xhost hostname命令启动X-Windows安装界面，如下图所示：
$ xhost mail.weiguo.com
$ unzip 10201_database_linux32.zip
$ cd database
$ ./runInstaller
10 修改dbstart
找到ORACLE_HOME_LISTNER=/ade/vikrkuma_new/oracle这行， 修改成:
ORACLE_HOME_LISTNER=/u01/app/product/10.2.0/db_1
或者直接修改成：
ORACLE_HOME_LISTNER=$ORACLE_HOME
测试运行
oracle$dbshut
oracle$dbstart
看能否启动或关闭oracle 服务及listener服务
oracle$ ps -efw | grep ora_
oracle$ lsnrctl status
oracle$ ps -efw | grep LISTEN | grep -v grep
11 自启动
首先使用root用户修改：
编辑/etc/oratab， (将N该为Y)
orcl:/oracle/app/product/10.2.0/db_1:N (将N该为Y)
在root下/etc/init.d/路径中建立oracle
#!/bin/bash
# chkconfig:345 99 10
# description: Startup Script for oracle Databases
export ORACLE_BASE=/opt/oracle
export ORACLE_HOME=/opt/oracle/or10g
export ORACLE_SID=orcl
export PATH=$ORACLE_HOME/bin:$PATH
case "$1" in
  start)
    #
    #oracle10g start
    #
    echo -n "Starting Oracle"
    su - oracle -c "$ORACLE_HOME/bin/dbstart"
    su - oracle -c "$ORACLE_HOME/bin/emctl start dbconsole"
    su - oracle -c "$ORACLE_HOME/bin/lsnrctl start"
    su - oracle -c "$ORACLE_HOME/bin/isqlplusctl start"
    ;;
  stop)
    #
    #oracle stop
    #
    echo -n "Shutdown Oracle."
    su - oracle -c "$ORACLE_HOME/bin/emctl stop dbconsole"
    su - oracle -c "$ORACLE_HOME/bin/isqlplusctl stop"
    su - oracle -c "$ORACLE_HOME/bin/dbshut"
    su - oracle -c "$ORACLE_HOME/bin/lsnrctl stop"
    ;;
  restart)
    #
    #oracle restart
    # 
    $0 stop
    $0 start
    ;;
    *)
    echo "Oracle10g start|stop|restart"
    exit 1
esac
exit 0
12 加入服务
#service oracle start    测试oracle能不能启动
#chkconfig --add oracle
#chkconfig --level 345 oracle on
#chkconfig --list oracle 看运行情况
dbua中文运行方法:
前提安装了JDK1.5或者更高的版本。
修改dbua文件
找到 JRE_DIR文件修改为  $JAVA_HOME/jre就可以运行中文环境了。
13 打补丁
停止一切oracle。然后运行运行补丁程序
修改 dbstart dbshut中让ORACLE_HOME_LISTNER=$1改为
ORACLE_HOME_LISTNER=$ORACLE_HOME
dbua
重新启动
14、清理日志文件。（解决非正常关闭数据库引起的数据库无法启动）
alter database clear unarchived logfile group 2;
alter database open; [返回顶楼](http://www.iteye.com/topic/846536#) [ ](http://wang25262875-gmail-com.iteye.com/ "浏览作者的博客") [ ](http://wang25262875-gmail-com.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=%E4%BC%A4%E5%BF%83%E9%9B%A8 "发送站内短信") [ ](http://wang25262875-gmail-com.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=%E4%BC%A4%E5%BF%83%E9%9B%A8 "关注作者") [回帖地址](http://www.iteye.com/topic/846536#1817131 "伤心雨回帖:Redhat 5 下 Oracle10g 安装，相信没有比这个更全的了")

[1](http://www.iteye.com/topic/846536# "好") [0](http://www.iteye.com/topic/846536# "差") 请登录后投票

[](http://www.iteye.com/forums/43/topics/846536/posts/new "发表回复")

« 上一页 1 [2](http://www.iteye.com/topic/846536?page=2) [3](http://www.iteye.com/topic/846536?page=3) [下一页 »](http://www.iteye.com/topic/846536?page=2)
[论坛首页](http://www.iteye.com/forums) → [综合技术版](http://www.iteye.com/forums/board/Tech) → [Linux](http://www.iteye.com/forums/tag/Linux)
跳转论坛:Java编程和Java企业应用 Web前端技术 移动编程和手机应用开发 C/C++编程 Ruby编程 Python编程 PHP编程 Flash编程和RIA Microsoft .Net 综合技术 软件开发和项目管理 行业应用 入门讨论 招聘求职 海阔天空

* [北京: ThoughtWorks诚聘Architect](http://www.iteye.com/jobs/1139 "北京:ThoughtWorks诚聘Architect")
* [北京: 国政通诚聘互联网软件开发工程师](http://www.iteye.com/jobs/1889 "北京:国政通诚聘互联网软件开发工程师")
* [上海: Scilearn China诚聘Web 用户界面工程师](http://www.iteye.com/jobs/1829 "上海:Scilearn China诚聘Web 用户界面工程师")

* [首页](http://www.iteye.com/)
* [资讯](http://www.iteye.com/news)
* [精华](http://www.iteye.com/magazines)
* [论坛](http://www.iteye.com/forums)
* [问答](http://www.iteye.com/ask)
* [博客](http://www.iteye.com/blogs)
* [群组](http://www.iteye.com/groups)
* [招聘](http://www.iteye.com/job)
* [搜索](http://www.iteye.com/search)
* [广告服务](http://www.iteye.com/index/service)
* [ITeye黑板报](http://webmaster.iteye.com/)
* [联系我们](http://www.iteye.com/index/contactus)
* [友情链接](http://www.iteye.com/index/friend_links)

© 2003-2011 ITeye.com. [ [京ICP证110151号](http://www.miibeian.gov.cn/) 京公网安备110105010620 ]
