---
layout: post
title: "CentOS6.0+下安装NVIDIA显卡驱动"
categories: linux
tags: 
 - linux
 - redhat
--- 

# CentOS6.0+下安装NVIDIA显卡驱动

‍linux（CentOS6.0+）下安装NVIDIA显卡驱动，disable The Nouveau kernel driver

(2013-03-31 13:49:23)

安装动因：安装本驱动程序之前，my pc 显卡风扇声音较大。最近实在忍不住了，才从师兄哪里找到了原因。原来是centos系统内核中用的是nouveau驱动程序（而my pc 的显卡是nvidia的GT 220），此驱动与nvidia完全无关。

开始安装时，按照网上的博客的说明，退出x-window界面，进入文本终端模式。运行 # sh nvidia-driver-version.run可是出现了如下错误：

ERROR: The Nouveau kernel driver is currently in use by your system. This

  driver is incompatible with the NVIDIA driver, and must be disabled

  before proceeding. Please consult the NVIDIA driver README and your

  Linux distribution's documentation for details on how to correctly

  disable the Nouveau kernel driver.

WARNING: The modprobe configuration file to disable Nouveau,

  /etc/modprobe.d/nvidia-installer-disable-nouveau.conf, is already

  present. Please be sure you have rebooted your system since that file

  was written. If you have rebooted, then Nouveau may be enabled for

  other reasons, such as being included in the system initial ramdisk or

  in your X configuration file. Please consult the NVIDIA driver README

  and your Linux distribution's documentation for details on how to

  correctly disable the Nouveau kernel driver.

ERROR: Installation has failed. Please see the file

  '/var/log/nvidia-installer.log' for details. You may find suggestions

  on fixing installation problems in the README available on the Linux.

 解决的办法是：先禁用nouveau kernel driver，再安装NVIDIA驱动。

在安装之前，到nvidia官方网站下载相应的驱动程序。

我的是：NVIDIA-Linux-x86_64-310.40.run

一、关闭nouveau

       1. 把nouveau kernel driver加入黑名单：

           # vi  /etc/modprobe.d/blacklist.conf  在后面追加 blacklist nouveau

       2. 重新建立initramfs image file：

           首先备份initramfs image file

               # mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r).img.bak

           其次重新建立initramfs image file

               # dracut -v /boot/initramfs-$(uname -r).img $(uname -r)

       3. 重启系统至文本终端模式

           # vi /etc/inittab

           最后一行“id:5:initdefault:”，将5修改成3

           （5代表系统启动时默认进入x-window图形界面，3代表默认进入终端模式。我的是CentOS6.4系统）

           # reboot

       4. 输入root和password，进入根用户模式下

           # lsmod | grep nouveau        确保nouveau kernel driver没有被加载

二、安装NVIDIA驱动

       1. # ./path/to/NVIDIA-Linux-x86_64-310.40.run

           或

           # sh NVIDIA-Linux-x86_64-310.40.run

           根据提示选择accept，yes 或 OK，即可完成安装！

       2. 修改配置文件，默认进入x-window模式

           # vi /etc/inittab

           最后一行id:3:initdefault:     将前面修改后的3，改回5，保存退出。

 
