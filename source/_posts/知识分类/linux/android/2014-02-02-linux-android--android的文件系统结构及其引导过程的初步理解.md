---
layout: post
title: "android的文件系统结构及其引导过程的初步理解"
categories: linux
tags: 
 - linux
 - android
--- 

# android的文件系统结构及其引导过程的初步理解

1、android文件系统的结构
android源码编译后得到system.img,ramdisk.img,userdata.img映像文件。其中， ramdisk.img是emulator的文件系统，system.img包括了主要的包、库等文件，userdata.img包括了一些用户数据，emulator加载这3个映像文件后，会把 system和 userdata分别加载到 ramdisk文件系统中的system和 userdata目录下。因此，我们可以把ramdisk.img里的所有文件复制出来，system.img和userdata.img分别解压到 ramdisk文件系统中的system和 userdata目录下。

 

ramdisk映像是一个最基础的小型文件系统，它包括了初始化系统所需要的全部核心文件，例如:初始化init进程以及init.rc（可以用于设置很多系统的参数）等文件。如果你您希望了解更多关于此文件的信息可以参考以下网址：
[http://git.source.android.com/?p=kernel/common.git;a=blob;f=Documentation/filesystems/ramfs-rootfs-initramfs.txt](http://git.source.android.com/?p=kernel/common.git;a=blob;f=Documentation/filesystems/ramfs-rootfs-initramfs.txt)
以下是一个典型的ramdisk中包含的文件列表：

./init.trout.rc
./default.prop
./proc
./dev
./init.rc
./init
./sys
./init.goldfish.rc
./sbin
./sbin/adbd
./system
./data

2、分离android文件系统出来
system.img,ramdisk.img,userdata.img映像文件是采用cpio打包、gzip压缩的，可以通过file命令验证：
file ramdisk.img，输出：
ramdisk.img: gzip compressed data, from Unix, last modified: Wed Mar 18 17:16:10 2009
Android源码编译后除了生成system.img，userdata.img之外还生成system和 userdata文件夹，因此不需要解压它们。Android源码编译后还生成root文件夹，其实root下的文件与 ramdisk.img 里的文件是一样的，不过这里还是介绍怎样把 ramdisk.img解压出来:
将ramdisk.img复制一份到任何其他目录下，将其名称改为ramdisk.img.gz，并使用命令
gunzip ramdisk.img.gz
然后新建一个文件夹，叫ramdisk吧，进入，输入命令
cpio -i -F ../ramdisk.img
这下，你就能看见并操作ramdisk里面的内容了。
然后把Android源码编译后生成的system和 userdata里的文件复制到 ramdisk/system和 ramdisk/userdata下。这样就得到一个文件系统了。
3、使用网络文件系统方式挂载android文件系统
因此，我们需要建立/nfsroot目录，再建立/nfsroot/androidfs目录，把刚才的android文件系统改名为androidfs，并链接到/nfsroot/androidfs
4、android内核引导文件系统
android内核挂载/nfsroot/androidfs之后，根据init.rc,init.goldfish.rc来初始化并装载系统库、程序等直到开机完成。init.rc脚本包括了文件系统初始化、装载的许多过程。init.rc的工作主要是：
1）设置一些环境变量
2）创建system、sdcard、data、cache等目录
3）把一些文件系统mount到一些目录去，如，mount tmpfs tmpfs /sqlite_stmt_journals
4）设置一些文件的用户群组、权限
5）设置一些线程参数
6）设置TCP缓存大小
