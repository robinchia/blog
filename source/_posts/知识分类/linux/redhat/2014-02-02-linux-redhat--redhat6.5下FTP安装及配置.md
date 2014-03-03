---
layout: post
title: "redhat 6.5下FTP安装及配置"
categories: linux
tags: 
 - linux
 - redhat
--- 

# redhat 6.5下FTP安装及配置

**一、****FTP****的安装**

1、检测是否安装了FTP:[root@localhost ~]# rpm -q vsftpd 

如果安装了会显示版本信息:

[root@localhost ~]# vsftpd-2.0.5-16.el5_5.1 
否则显示:[root@localhost ~]# package vsftpd is not installed 
2、如果没安装FTP，运行yum install vsftpd命令
[root@localhost ~]# yum install vsftpd 
[root@localhost ~]# 
3、完成ftp安装后，将/etc/vsftpd/user_list文件和/etc/vsftpd/ftpusers文件中的root这一行注释掉
# root 
4、执行以下命令
#setsebool -P ftpd_disable_trans=1
修改/etc/vsftpd/vsftpd.conf,在最后一行处添加local_root=/
5、重启ftp进程   #service vsftpd restart 

注：每次修改过ftp相关的配置文件，都需要重启ftp进程来生效。
ftp服务器就可以使用了。

*********************************************************************
**二、****vsftpd****的配置文件说明：**
vsftpd.ftpusers：位于/etc目录下。它指定了哪些用户账户不能访问FTP服务器，例如root等。
vsftpd.user_list：位于/etc目录下。该文件里的用户账户在默认情况下也不能访问FTP服务器，仅当vsftpd .conf配置文件里启用userlist_enable=NO选项时才允许访问。
vsftpd.conf：位于/etc/vsftpd目录下。来自定义用户登录控制、用户权限控制、超时设置、服务器功能选项、服务器性能选项、服务器响应消息等FTP服务器的配置。
(1)用户登录控制
anonymous_enable=YES，允许匿名用户登录。
no_anon_password=YES，匿名用户登录时不需要输入密码。
local_enable=YES，允许本地用户登录。
deny_email_enable=YES，可以创建一个文件保存某些匿名电子邮件的黑名单，以防止这些人使用Dos攻击。
banned_email_file=/etc/vsftpd.banned_emails，当启用deny_email_enable功能时，所需的电子邮件黑名单保存路径(默认为/etc/vsftpd.banned_emails)。
(2)用户权限控制
write_enable=YES，开启全局上传权限。
local_umask=022，本地用户的上传文件的umask设为022(系统默认是077，一般都可以改为022)。
anon_upload_enable=YES，允许匿名用户具有上传权限，很明显，必须启用write_enable=YES，才可以使用此项。同时我们还必须建立一个允许ftp用户可以读写的目录(前面说过，ftp是匿名用户的映射用户账号)。
anon_mkdir_write_enable=YES，允许匿名用户有创建目录的权利。
chown_uploads=YES，启用此项，匿名上传文件的属主用户将改为别的用户账户，注意，这里建议不要指定root账号为匿名上传文件的属主用户！
chown_username=whoever，当启用chown_uploads=YES时，所指定的属主用户账号，此处的whoever自然要用合适的用户账号来代替。
chroot_list_enable=YES，可以用一个列表限定哪些本地用户只能在自己目录下活动，如果chroot_local_user=YES，那么这个列表里指定的用户是不受限制的。
chroot_list_file=/etc/vsftpd.chroot_list，如果chroot_local_user=YES，则指定该列表(chroot_local_user)的保存路径(默认是/etc/vsftpd.chroot_list)。
nopriv_user=ftpsecure，指定一个安全用户账号，让FTP服务器用作完全隔离和没有特权的独立用户。这是vsftpd系统推荐选项。
async_abor_enable=YES，强烈建议不要启用该选项，否则将可能导致出错！
ascii_upload_enable=YES；ascii_download_enable=YES，默认情况下服务器会假装接受ASCⅡ模式请求但实际上是忽略这样的请求，启用上述的两个选项可以让服务器真正实现ASCⅡ模式的传输。
注意：启用ascii_download_enable选项会让恶意远程用户们在ASCⅡ模式下用“SIZE/big/file”这样的指令大量消耗FTP服务器的I/O资源。
这些ASCⅡ模式的设置选项分成上传和下载两个，这样我们就可以允许ASCⅡ模式的上传(可以防止上传脚本等恶意文件而导致崩溃)，而不会遭受拒绝服务攻击的危险。
(3)用户连接和超时选项
idle_session_timeout=600，可以设定默认的空闲超时时间，用户超过这段时间不动作将被服务器踢出。
data_connection_timeout=120，设定默认的数据连接超时时间。
(4)服务器日志和欢迎信息
dirmessage_enable=YES，允许为目录配置显示信息，显示每个目录下面的message_file文件的内容。
ftpd_banner=Welcome to blah FTP service，可以自定义FTP用户登录到服务器所看到的欢迎信息。
xferlog_enable=YES，启用记录上传/下载活动日志功能。
xferlog_file=/var/log/vsftpd.log，可以自定义日志文件的保存路径和文件名，默认是/var/log/vsftpd.log。
anonymous_enable=YES允许匿名登录local_enable=YES允许本地用户登录
write_enable=YES开放本地用户写权限
local_umask=022设置本地用户生成文件的掩码为022
#anon_upload_enable=YES此项设置允许匿名用户上传文件
#anon_mkdir_write_enable=YES开启匿名用户的写和创建目录的权限
dirmessage_enable=YES当切换到目录时，显示该目录下的.message隐藏文件的内容
xferlog_enable=YES激活上传和下载日志
connect_from_port_20=YES启用FTP数据端口的连接请求
#chown_uploads=YES是否具有上传权限.用户由chown_username参数指定。
#chown_username=whoever指定拥有上传文件权限的用户。此参数与chown_uploads联用。
#xferlog_file=/var/log/vsftpd.log
xferlog_std_format=YES使用标准的ftpd xferlog日志格式
#idle_session_timeout=600此设置将在用户会话空闲10分钟后被中断
#data_connection_timeout=120将在数据连接空闲2分钟后被中断
#ascii_upload_enable=YES启用上传的ASCII传输方式
#ascii_download_enable=YES启用下载的ASCII传输方式
#ftpd_banner=Welcome to blah FTP service 设置用户连接服务器后显示消息
#deny_email_enable=NO此参数默认值为NO。当值为YES时，拒绝使用banned_email_file参数指定文件中所列出的e-mail地址用户登录。
#banned_email_file=/etc/vsftpd.banned_emails指定包含拒绝的e-mail地址的文件.
#chroot_list_enable=YES设置本地用户登录后不能切换到自家目录以外的别的目录
#chroot_list_file=/etc/vsftpd.chroot_list
#ls_recurse_enable=YES
pam_service_name=vsftpd设置PAM认证服务的配置文件名称，该文件存放在/etc/pam.d/
userlist_enable=YES此项配置/etc/vsftpd.user_list中指定的用户也不能访问服务器，若添加userlist_deny=No,则仅仅/etc/vsftpd.user_list文件中的用户可以访问，其他用户都不可以访问服务器。如过userlist_enable=NO,userlist_deny=YES,则指定使文件/etc/vsftpd.user_list中指定的用户不可以访问服务器，其他本地用户可以访问服务器。
listen=YES指明VSFTPD以独立运行方式启动
tcp_wrappers=YES在VSFTPD中使用TCP_Wrappers远程访问控制机制，默认值为YES

**三、举例建立一个名为****test****的账户并进行配置**

根据实际情况对FTP进行配置后，下面举例介绍建立一个FTP账户并进行简单的配置：

1、创建一个账号为test的账户：

#mkdir /tmp/test                                    //首先创建好目录

#adduser -d /tmp/test  -g ftp -s /sbin/nologin test //-s /sbin/nologin是让其不能登陆系统，-d是指定用户目录为/opt/srsman，即该账户只能登陆ftp，却不能用做登陆系统用。

#passwd test

Changing password for user beinan.//接下来会出现让你设置新的密码

New password: 

Retype new password: 
passwd: all authentication tokens updated successfully

创建账户成功！

‍

2、限制用户目录，不得改变目录到上级

修改/etc/vsftpd/vsftpd.conf
将这两行
#chroot_list_enable=YES
#chroot_list_file=/etc/vsftpd.chroot_list
注释去掉
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chroot_list

新增一个文件: /etc/vsftpd/chroot_list 
内容写需要限制的用户名：
test

重新启动vsftpd

#service vsftpd restart

3、最后为了防止服务器由于断电、重启等现象发生，导致ftp进程在开机后未启动，将其添加到开机启动文件中：

(1)找到/etc/rc.local文件

(2)打开该文件，在最后一行添加：service vsftpd start

(3)保存，退出

4、通过在“我的电脑”中输入ftp：//192.168.179.30(填该ftp服务器ip地址)进入ftp服务器，输入设置好的账户登陆即可。
