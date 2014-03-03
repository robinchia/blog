---
layout: post
title: "强制指定源更新firefox"
categories: linux
tags: 
 - linux
 - redhat
--- 

# 强制指定源更新firefox

Centos6.5下到firefox经常菜单假死，故使用另外的源更新。

使用remi源：
## Install Remi repository for RHEL/CentOS 6.5/6.4/6.3/6.2/6.1/6.0 ### wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm # rpm -Uvh remi-release-6.rpm## Check Availability of Firefox 26 in RHEL/CentOS 6.5/6.4/6.3/6.2/6.1/6.0 and Fedora 15/14  ### yum --enablerepo=remi list firefox## Install or Update Firefox 26 in RHEL/CentOS 6.5/6.4/6.3/6.2/6.1/6.0  ### yum --enablerepo=remi install firefox

这是在未安装到情况下

更新firefox：
## Update Firefox 26 in RHEL/CentOS 6.5/6.4/6.3/6.2/6.1/6.0  ### yum --enablerepo=remi update firefox
