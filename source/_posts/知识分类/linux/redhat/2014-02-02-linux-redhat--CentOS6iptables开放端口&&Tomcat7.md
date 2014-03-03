---
layout: post
title: "CentOS 6 iptables 开放端口&&Tomcat7"
categories: linux
tags: 
 - linux
 - redhat
--- 

# CentOS 6 iptables 开放端口&&Tomcat7

**[CentOS](http://www.linuxidc.com/topicnews.aspx?tid=14 "CentOS") 6 iptables 开放端口80 3306 22等**

#/sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT
#/sbin/iptables -I INPUT -p tcp --dport 22 -j ACCEPT
#/sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
#/sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
然后保存：
#/etc/init.d/iptables save
 
查看打开的端口：
# /etc/init.d/iptables status

极端情况

#关闭防火墙
/etc/init.d/iptables stop‍

CentOS-6.3安装配置Tomcat-7

装环境：CentOS-6.3
安装方式：源码安装
软件：apache-tomcat-7.0.29.tar.gz
下载地址：http://tomcat.apache.org/download-70.cgi

## 安装前提

系统必须已经安装配置了JDK6+，如果不会安装请参考《CentOS-6.3安装配置JDK-7》。

## 安装tomcat

将apache-tomcat-7.0.29.tar.gz文件上传到/usr/local中执行以下操作：
[root@admin    local]# cd /usr/local
[root@admin    local]# tar -zxv -f apache-tomcat-7.0.29.tar.gz         // 解压压缩包
[root@admin    local]# rm -rf apache-tomcat-7.0.29.tar.gz   // 删除压缩包
[root@admin    local]# mv apache-tomcat-7.0.29  tomcat

## 启动Tomcat

执行以下操作：
[root@admin ~]#  /usr/local/tomcat/bin/startup.sh   //启动tomcat
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR:    /usr/local/tomcat/temp
Using JRE_HOME:        /usr/java/jdk1.7.0/jre
Using CLASSPATH:          /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar

出现以上的打印信息说明已经成功启动。 

3、创建链接文件

ln –s  /usr/local/tomcat/bin//startup.sh /etc/init.d/tomcat

4、修改权限

chmod +x tomcat

5、添加启动

chkconfig –add  tomcat（add前是两个减号）

chkconfig tomcat on

6、检查

service tomcat start
