---
layout: post
title: "数据库调整和优化： Slony-I"
categories: DB
tags: 
 - DB
 - oracle
 - oracle-
--- 

# 数据库调整和优化： Slony-I

[首页](http://wap.oschina.net/) » [数据库调整和优化](http://wap.oschina.net/project/tag/81) » [Slony-I](http://wap.oschina.net/p/slony-i)

# Slony-I

[简介]() [类似项目(25)](http://wap.oschina.net/p/slony-i/similar_projects) [新闻(0)](http://wap.oschina.net/p/slony-i/news) [讨论区(0)](http://wap.oschina.net/p/slony-i/discuss)

Slony-I可以来实现PostgreSQL数据库的主从复制。

![]()

下面是Slony-I 的安装配置简明指南，实现主副数据库的同步。后面我会再介绍Pgbouncer的安装和配置
1. 主副数据库机器
Master:
hostname: M_DB
inet addr:10.0.0.11
OS: Linux 2.6.9-42.ELsmp
CPU:Intel(R) Xeon(R) CPU L5320 @ 1.86GHz
MemTotal: 254772 kB
PgSQL: postgresql-8.3.0
Slave:
hostname:S_DB
inet addr:10.0.0.12
OS: Linux 2.6.9-42.ELsmp
CPU:Intel(R) Xeon(R) CPU L5320 @ 1.86GHz
MemTotal: 514440 kB
PgSQL: postgresql-8.3.0
#在M_DB和S_DB上安装postgresql-8.3.0, 安装和配置过程参见我的上一篇Blog，确保超级用户是postgres，数据库名是URT。
#检查M_DB和S_DB上的超级用户postgres是否可以访问对方的机器
#分别在M_DB和S_DB上执行
sudo -u postgres /home/y/pgsql/bin/createlang plpgsql URT
#分别在M_DB和S_DB上的URT数据库里创建相同的表accounts。
2. 安装Slony-I
#分别在M_DB和S_DB上安装Slony-I
tar xfj slony1-1.2.13.tar.bz2
cd slony1-1.2.13
./configure –with-pgconfigdir=/home/y/pgsql/bin
gmake all
sudo gmake install
3. Slony Config
创建urt_replica_init.sh文件:
##############################
#!/bin/sh
SLONIK=/home/y/pgsql/bin/slonik
#slonik可执行文件位置
CLUSTER=URT
#你的集群的名称
SET_ID=1
#你的复制集的名称
MASTER=1
#主服务器ID
HOST1=M_DB
#源库IP或主机名
DBNAME1=URT
#需要复制的源数据库
SLONY_USER=postgres
#源库数据库超级用户名
SLAVE=2
#从服务器ID
HOST2=S_DB
#目的库IP或主机名
DBNAME2=URT
#需要复制的目的数据库
PGBENCH_USER=postgres
#目的库用户名
$SLONIK <<_EOF_
#这句是定义集群名
cluster name = $CLUSTER;
#这两句是定义复制节点
node $MASTER admin conninfo = 'dbname=$DBNAME1 host=$HOST1 user=$SLONY_USER ';
node $SLAVE admin conninfo = 'dbname=$DBNAME2 host=$HOST2 user=$PGBENCH_USER ';
#初始化集群和主节点，id从1开始，如果只有一个集群，那么肯定是1
#comment里可以写一些自己的注释，随意
init cluster ( id = $MASTER, comment = 'Primary Node' );
#下面是从节点
store node ( id = $SLAVE, comment = 'Slave Node' );
#配置主从两个节点的连接信息，就是告诉Slave服务器如何来访问Master服务器
#下面是主节点的连接参数
store path ( server = $MASTER, client = $SLAVE,
conninfo = 'dbname=$DBNAME1 host=$HOST1 user=$SLONY_USER ');
#下面是从节点的连接参数
store path ( server = $SLAVE, client = $MASTER,
conninfo = 'dbname=$DBNAME2 host=$HOST2 user=$PGBENCH_USER ');
#设置复制中角色，主节点是原始提供者，从节点是接受者
store listen ( origin = $MASTER, provider = 1, receiver = 2 );
store listen ( origin = $SLAVE, provider = 2, receiver = 1 );
#创建一个复制集，id也是从1开始
create set ( id = $SET_ID, origin = $MASTER, comment = 'All pgbench tables' );
#向自己的复制集种添加表，每个需要复制的表添加一条set命令，id从1开始，逐次递加，步进为1；
#fully qualified name是表的全称：模式名.表名
#这里的复制集id需要和前面创建的复制集id一致
set add table ( set id = $SET_ID, origin = $MASTER,
id = 1, fully qualified name = 'public.accounts',
comment = 'Table accounts' );
_EOF_
########################
#在M_DB或者S_DB上执行
./urt_replica_init.sh
4. Slony Start
创建Master.slon文件:
########################
cluster_name="URT"
conn_info="dbname=URT host=M_DB user=postgres"
########################
创建Slave.slon文件:
########################
cluster_name="URT"
conn_info="dbname=URT host=S_DB user=postgres"
########################
#在M_DB上执行
/home/y/pgsql/bin/slon -f master.slon >> master.log &
#在S_DB上执行
/home/y/pgsql/bin/slon -f slave.slon >> slave.log &
5. Slony Subscribe
创建urt_replica_subscribe.sh文件:
########################
#!/bin/sh
SLONIK=/home/y/pgsql/bin/slonik
#slonik可执行文件位置
CLUSTER=URT
#你的集群的名称
SET_ID=1
#你的复制集的名称
MASTER=1
#主服务器ID
HOST1=M_DB
#源库IP或主机名
DBNAME1=URT
#需要复制的源数据库
SLONY_USER=postgres
#源库数据库超级用户名
SLAVE=2
#从服务器ID
HOST2=S_DB
#目的库IP或主机名
DBNAME2=URT
#需要复制的目的数据库
PGBENCH_USER=postgres
#目的库用户名
$SLONIK <<_EOF_
#这句是定义集群名
cluster name = $CLUSTER;
#这两句是定义复制节点
node $MASTER admin conninfo = 'dbname=$DBNAME1 host=$HOST1 user=$SLONY_USER';
node $SLAVE admin conninfo = 'dbname=$DBNAME2 host=$HOST2 user=$PGBENCH_USER ';
#提交复制集
subscribe set ( id = $SET_ID, provider = $MASTER, receiver = $SLAVE, forward = no);
_EOF_
########################
#在M_DB或者S_DB上执行
./urt_replica_subscribe.sh
6. 测试
修改M_DB上URT数据里的accounts表，S_DB上的accounts表也会随之改变

开发语言： 不详
收录时间：2008年10月06日
主页:[http://wap.oschina.net/home/goweb?url=http%3A%2F%2Fwww.slony.info%2F](http://wap.oschina.net/home/goweb?url=http%3A%2F%2Fwww.slony.info%2F)
文档:[http://wap.oschina.net/home/goweb?url=http%3A%2F%2Fwww.slony.info%2Fdocumentation%2F%0A](http://wap.oschina.net/home/goweb?url=http%3A%2F%2Fwww.slony.info%2Fdocumentation%2F%0A)
下载:[http://wap.oschina.net/home/goweb?url=http%3A%2F%2Fwww.slony.info%2Fdownloads%2F](http://wap.oschina.net/home/goweb?url=http%3A%2F%2Fwww.slony.info%2Fdownloads%2F)
网友评论(0)

帐号：
密码：
[注册新帐号](http://wap.oschina.net/home/reg)
不能超过250个字

快速通道:[首页](http://wap.oschina.net/)|[新闻](http://wap.oschina.net/news/list)|[项目](http://wap.oschina.net/project/list)|[讨论区](http://wap.oschina.net/discuss)
开源中国社区(OsChina.NET)
2009/10/26 星期一 14:12
[进入Web版](http://www.oschina.net/)
