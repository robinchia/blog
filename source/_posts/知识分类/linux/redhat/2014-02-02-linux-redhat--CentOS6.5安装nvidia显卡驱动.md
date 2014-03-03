---
layout: post
title: "CentOS 6.5安装nvidia显卡驱动"
categories: linux
tags: 
 - linux
 - redhat
--- 

# CentOS 6.5安装nvidia显卡驱动

* 
1 
* 
根据nvidia显卡的具体型号，从官方网站下载驱动，我的是NVIDIA-Linux-x86-319.82.run
* 
http://www.geforce.cn/drivers
* 
2安装编译环境：gcc、kernel-devel、kernel-headers

[root@localhost ~]# yum -y install gcc kernel-devel kernel-headers
* 
3

修改/etc/modprobe.d/blacklist.conf 文件，以阻止 nouveau 模块的加载

方法： 添加blacklist nouveau，注释掉blacklist nvidiafb

修改后内容如下：

#

# Listing a module here prevents the hotplug scripts from loading it.

# Usually that'd be so that some other driver will bind it instead,

# no matter which driver happens to get probed first.  Sometimes user

# mode tools can also control driver binding.

#

# Syntax: see modprobe.conf(5).

#

# watchdog drivers

blacklist i8xx_tco

# framebuffer drivers

blacklist nouveau

# ISDN - see bugs 154799, 159068

blacklist hisax

blacklist hisax_fcpcipnp

# sound drivers

blacklist snd-pcsp

# I/O dynamic configuration support for s390x (bz #563228)

blacklist chsc_sch
* 
4

重新建立initramfs image文件

[root@localhost ~]# mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r).img.bak

[root@localhost ~]# dracut /boot/initramfs-$(uname -r).img $(uname -r)
* 
5

修改/etc/inittab，使系统开机进入init 3文本模式:

将最后一行“id:5:initdefault:”修改成“id:3:initdefault:”（不包含引号）

注释：5代表系统启动时默认进入x-window图形界面，3代表默认进入终端模式。
* 
6

阻止kernel加载nouveau模块（非必要操作）

修改/boot/grub/grub.conf，在kernel行的末尾加上 rdblacklist=nouveau vga=792
* 
7

重启

[root@localhost ~]# reboot now

## 安装NVIDIA驱动

1. 
输入root和password，进入根用户模式下,确保nouveau kernel driver没有被加载

[root@localhost ~]# lsmod | grep nouveau
1. 
进入驱动程序所在目录,开始安装

[root@localhost ~]# chmod +x NVIDIA-Linux-x86-331.20.run

[root@localhost ~]# ./NVIDIA-Linux-x86-331.20.run

安装过程中，根据提示选择accept，yes 或 OK，即可完成安装：

     如果提示有旧驱动，询问是否删除旧驱动，选Yes；

     如果提示缺少某某模块（modules），询问是否上网下载，选no；

     如果提示编译模块，询问是否进行编译，选ok；

     如果提示将要修改xorg.conf，询问是否允许，选Yes；

     接下来就是等待安装完成。
1. 
修改/etc/inittab，使系统开机进入init 5图形界面模式

将最后一行“id:3:initdefault:”修改成“id:5:initdefault:”
1. 
重启

[root@localhost ~]#reboot now

当看到Nvidia的logo后，安装成功,登陆后在系统- 首选项里可以看到NVIDIA X Server Settings菜单，可以查看基本信息及进行一些设置
