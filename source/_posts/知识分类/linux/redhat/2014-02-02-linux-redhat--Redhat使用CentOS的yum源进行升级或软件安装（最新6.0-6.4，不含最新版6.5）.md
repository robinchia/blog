---
layout: post
title: "Redhat 使用CentOS的yum源进行升级或软件安装（最新6.0-6.4，不含最新版6.5）"
categories: linux
tags: 
 - linux
 - redhat
--- 

# Redhat 使用CentOS的yum源进行升级或软件安装（最新6.0-6.4，不含最新版6.5）

Redhat默认的源不但速度不给力，而且软件版本陈旧，今天试着将Redhat默认源替换为CentOS的163源，发现速度能达到2M/s左右，而且版本都比较新，非常给力，与大家分享！

（目前可以使用CentOS0-6.3软件仓库的软件）
1. 删除原有的yum源

    # rpm -aq | grep yum|xargs rpm -e --nodeps
2.下载新的yum安装包，这里使用centos-6

    # wget http://mirror.centos.org/centos/6/os/x86_64/Packages/python-iniparse-0.3.1-2.1.el6.noarch.rpm
    # wget http://mirror.centos.org/centos/6/os/x86_64/Packages/yum-metadata-parser-1.1.2-16.el6.x86_64.rpm

    # wget http://mirror.centos.org/centos/6/os/x86_64/Packages/yum-3.2.29-40.el6.centos.noarch.rpm
    # wget http://mirror.centos.org/centos/6/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.30-14.el6.noarch.rpm

3.安装yum软件包
    #rpm -ivh python-iniparse-0.3.1-2.1.el6.noarch.rpm

    # rpm -ivh yum-metadata-parser-1.1.2-16.el6.x86_64.rpm
    #rpm -ivh yum-3.2.29-40.el6.centos.noarch.rpm yum-plugin-fastestmirror-1.1.30-14.el6.noarch.rpm

4. 更改yum源
在/etc/yum.repos.d/新建文件centos.repo,内容如下：

    #########################################################################
    # CentOS-Base.repo

    #
    # The mirror system uses the connecting IP address of the client and the

    # update status of each mirror to pick mirrors that are updated to and
    # geographically close to the client. You should use this for CentOS updates

    # unless you are manually picking other mirrors.
    #

    # If the mirrorlist= does not work for you, as a fall back you can try the
    # remarked out baseurl= line instead.

    #
    #

    [base]
    name=CentOS-6 - Base - 163.com

    baseurl=http://mirrors.163.com/centos/6/os/$basearch/
    #mirrorlist=http://mirrorlist.centos.org/?release=6&arch=$basearch&repo=os

    gpgcheck=1
    gpgkey=http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-6

    #released updates
    [updates]

    name=CentOS-6 - Updates - 163.com
    baseurl=http://mirrors.163.com/centos/6/updates/$basearch/

    #mirrorlist=http://mirrorlist.centos.org/?release=6&arch=$basearch&repo=updates
    gpgcheck=1

    gpgkey=http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-6
    #additional packages that may be useful

    [extras]
    name=CentOS-6 - Extras - 163.com

    baseurl=http://mirrors.163.com/centos/6/extras/$basearch/
    #mirrorlist=http://mirrorlist.centos.org/?release=6&arch=$basearch&repo=extras

    gpgcheck=1
    gpgkey=http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-6

    #additional packages that extend functionality of existing packages
    [centosplus]

    name=CentOS-6 - Plus - 163.com
    baseurl=http://mirrors.163.com/centos/6/centosplus/$basearch/

    #mirrorlist=http://mirrorlist.centos.org/?release=6&arch=$basearch&repo=centosplus
    gpgcheck=1

    enabled=0
    gpgkey=http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-6

    #contrib - packages by Centos Users
    [contrib]

    name=CentOS-6 - Contrib - 163.com
    baseurl=http://mirrors.163.com/centos/6/contrib/$basearch/

    #mirrorlist=http://mirrorlist.centos.org/?release=6&arch=$basearch&repo=contrib
    gpgcheck=1

    enabled=0
    gpgkey=http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-6

    #########################################################################
或者下载文件CentOS6-Base-163.repo：

wget [http://mirrors.163.com/.help/CentOS6-Base-163.repo](http://mirrors.163.com/.help/CentOS6-Base-163.repo)
将其中的

baseurl=http://mirrors.163.com/centos/$releasever/contrib/$basearch/

#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib
的$releasever 修改为 6

mv CentOS6-Base-163.repo /etc/yum.repos.d/CentOS-Base.repo
5.清理yum缓存

    # yum clean all
    # yum makecache #将服务器上的软件包信息缓存到本地,以提高搜索安装软件的速度

    # yum install software_name #测试新源是否可用
6.更新系统

    #yum update -y
问题：

（1）如果出现域名不能解析的情况，添加nameserver 8.8.8.8到/etc/resolv.conf中即可。
