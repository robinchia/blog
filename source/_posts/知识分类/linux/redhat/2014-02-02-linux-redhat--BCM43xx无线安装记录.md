---
layout: post
title: "BCM43xx无线安装记录"
categories: linux
tags: 
 - linux
 - redhat
--- 

# BCM43xx无线安装记录

大致按这个

[http://blog.csdn.net/airfer/article/details/8812784](http://blog.csdn.net/airfer/article/details/8812784)或

[http://wiki.centos.org/zh/HowTos/Laptops/Wireless/Broadcom?action=show](http://wiki.centos.org/zh/HowTos/Laptops/Wireless/Broadcom?action=show)来做。

注意make失败照这个[http://www.cnblogs.com/zhuangzebo/p/3158493.html](http://www.cnblogs.com/zhuangzebo/p/3158493.html)

来处理，也就是make API=WEXT

最后要注意将冲突的模块加blacklist，按这个来做[http://blog.sina.com.cn/s/blog_4b75b002010161n3.html](http://blog.sina.com.cn/s/blog_4b75b002010161n3.html)

 

cd /usr/local/src/hybrid-wl/

make clean

make  API=WEXT

make install
我执行的是第二个链接：

# 以 Broadcom Corporation BCM4311, BCM4312, BCM4313, BCM4321, BCM4322, BCM43224, BCM43225, BCM43227 和 BCM43228 为基础的无线网络卡

![<!>]( "<!>") CentOS 对这些芯片组并没有原生的支持。

这页的英文版本现时由 [Miloš Blažević](http://wiki.centos.org/MilosBlazevic) 维护。
![]( "attachment:ArtWork/WikiDesign/icon-admonition-alert.png")

**注意：**自驱动程序 5.100.82.38 于 2010 年 12 月发行至现有的最新版本（5.100.82.112），暂时仍未有数据确定这些驱动程序在 CentOS 5 上对声称支持的无线芯片组是否有效。因此，假若 [ELRepo 的 kmod-wl 页](http://elrepo.org/tiki/wl-kmod)不能助你设置无联机网，你可考虑采用另一个方法，就是从[这里](http://archive.ubuntu.com/ubuntu/pool/multiverse/b/broadcom-sta/broadcom-sta_5.60.48.36.orig.tar.gz)、[这里](http://archive.ubuntu.com/ubuntu/pool/multiverse/b/broadcom-sta/broadcom-sta_5.10.91.9.3.orig.tar.gz)或付出小量金钱从[这里](http://members.driverguide.com/driver/detail.php?driverid=1601449)下载旧版驱动程序，然后按照以下指南进行编译。另外，烦请反馈你对 Broadcom 无线芯片的经验，好让此指南能保持更新及不断改善。![]( "attachment:ArtWork/WikiDesign/icon-admonition-info.png")

**注：**基于这个 Broadcam 驱动程序的极度限制性条款，ELRepo 软件库的开发者放弃以 rpm 组件来提供它 —— 因此这份文件被创建的目的是要提供一个全面的驱动程序安装说明。![]( "attachment:ArtWork/WikiDesign/icon-admonition-info.png")

**注：**本作者至今只测试了 Broadcom 的 BCM4311 及 BCM4312 芯片组。

Contents

1. [以 Broadcom Corporation BCM4311, BCM4312, BCM4313, BCM4321, BCM4322, BCM43224, BCM43225, BCM43227 和 BCM43228 为基础的无线网络卡](http://wiki.centos.org/zh/HowTos/Laptops/Wireless/Broadcom?action=show#head-e6d7ee96336c7d433ac1c29de6d8abcf0b5cf9b7)

1. [第 1 步：辨认无线网络芯片及安装时依赖的组件](http://wiki.centos.org/zh/HowTos/Laptops/Wireless/Broadcom?action=show#head-37e5a48b460ced40a5a6676720dbfc0fbc452849)
1. [第 2 步：下载并解压 Broadcom 驱动程序的压缩档](http://wiki.centos.org/zh/HowTos/Laptops/Wireless/Broadcom?action=show#head-f9127743dc7076bb4fbc9f729147646c35937c38)
1. [第 3 步：编译 Broadcom 驱动模块](http://wiki.centos.org/zh/HowTos/Laptops/Wireless/Broadcom?action=show#head-2700ac07dd839f031bc3da66d7bc2c91ef45de72)
1. [第 4 步上：将驱动模块装入内核中](http://wiki.centos.org/zh/HowTos/Laptops/Wireless/Broadcom?action=show#head-338f081e6ff8fb0208a53acdad261ec95e181b77)
1. [第 4 步下：在开机时将驱动模块装入内核中](http://wiki.centos.org/zh/HowTos/Laptops/Wireless/Broadcom?action=show#head-9d0b5fc97e48d03b2f8f9be52c00e82f14d70965)
1. [附录 A：在 CentOS 5.6 上编译（最新的？）驱动](http://wiki.centos.org/zh/HowTos/Laptops/Wireless/Broadcom?action=show#head-3219f249363c7f864138ea279aff3a091f6f2a0b)

若要安装以 Broadcom BCM4311、BCM4312、BCM4313、BCM4321 或 BCM4322 为基础的无线网络卡，请遵照以下的步骤：

## 第 1 步：辨认无线网络芯片及安装时依赖的组件

首先，请确定你是位「拥有 Broadcom BCM43xx 无线网络卡的幸运儿」：

[user@host ~]$ /sbin/lspci | grep Broadcom0b:00.0 Network controller: Broadcom Corporation BCM4312 802.11a/b/g (rev 01)

辨认完无线网络芯片型号之后，请确定你不会欠缺编译及安装时所需的组件：

[root@host]# yum install kernel-headers kernel-devel gcc

当然，假若你要为 Xen 内核（kernel-xen）编译驱动程序，你必须安装 kernel-xen-devel 而不是 kernel-devel。

## 第 2 步：下载并解压 Broadcom 驱动程序的压缩档

请从 [Broadcom 的官方网站](http://www.broadcom.com/support/802.11/linux_sta.php)下载 Broadcom BCM43xx 的 linux 驱动程序压缩档**（此驱动程序已无效 —— 请参阅本页顶部的警告）**到你的机器并将它解压到 /usr/local/src/hybrid-wl，请随你所需将这个目录的拥有者改为无特权的用户：

[root@host ~]# mkdir -p /usr/local/src/hybrid-wl[root@host hybrid-wl]# cd /usr/local/src/hybrid-wl[root@host hybrid-wl]# tar xvfz /path/to/the/tarball/hybrid-portsrc-x86_64-v5.10.91.9.3.tar.gz（下载档的名称）[root@host hybrid-wl]# chown -R someuser.somegroup /usr/local/src/hybrid-wl

![]( "attachment:ArtWork/WikiDesign/icon-admonition-idea.png")

**注：为什么不随便将它解压到一个位置并保留缺省的拥有者？**
原因是上面的做法会把驱动模块的源代码保留在系统上 —— 在你放置它们的位置 —— 好让你可以随时按需要创建驱动程序（譬如：你将内核升了级 —— 因为驱动模块永远根据某个内核来编译），还有，就是你可以用无特权的用户来编译！

## 第 3 步：编译 Broadcom 驱动模块

驱动模块可以这样编译：

[user@host hybrid-wl]$ make -C /lib/modules/`uname -r`/build/ M=`pwd`

请留意引号（也就反引号）。

现在你很可能会获得一个错误信息，而不是一个编译好的驱动模块（实际上，本作者仍未遇过这个信息以外的情况）。这则信息的内容大致上是：

make: Entering directory `/usr/src/kernels/2.6.18-164.el5-x86_64'  LD      /tmp/hybrid/hybrid/hybrid/built-in.o  CC [M]  /tmp/hybrid/hybrid/hybrid/src/wl/sys/wl_linux.oIn file included from /tmp/hybrid/hybrid/hybrid/src/wl/sys/wl_linux.c:20:/tmp/hybrid/hybrid/hybrid/src/include/typedefs.h:70: error: conflicting types for ‘bool’include/linux/types.h:36: error: previous declaration of ‘bool’ was heremake[1]: *** [/tmp/hybrid/hybrid/hybrid/src/wl/sys/wl_linux.o] Error 1make: *** [_module_/tmp/hybrid/hybrid/hybrid] Error 2make: Leaving directory `/usr/src/kernels/2.6.18-164.el5-x86_64'

正如你所见，**typedefs.h** 这个文件的第 70 行出了一个问题。要解决它，请将第 70 行的代码改为注释，好让它变成：

/*#ifndef TYPEDEF_BOOLtypedef  unsigned char  bool;#endif*/

你亦可以通过在标头档加入以下内容（勿论这一行是否已经存在）来简单地解决这个问题：

#define TYPEDEF_BOOL

现在，请尝试再次编译驱动模块：

[user@host hybrid-wl]$ make -C /lib/modules/`uname -r`/build/ M=`pwd`

编译器的输出大致上是这样：

make: Entering directory `/usr/src/kernels/2.6.18-164.el5-x86_64'  CC [M]  /tmp/hybrid/hybrid/hybrid/src/wl/sys/wl_linux.o  CC [M]  /tmp/hybrid/hybrid/hybrid/src/wl/sys/wl_iw.o  CC [M]  /tmp/hybrid/hybrid/hybrid/src/shared/linux_osl.o  LD [M]  /tmp/hybrid/hybrid/hybrid/wl.o  Building modules, stage 2.  MODPOST  CC      /tmp/hybrid/hybrid/hybrid/wl.mod.o  LD [M]  /tmp/hybrid/hybrid/hybrid/wl.komake: Leaving directory `/usr/src/kernels/2.6.18-164.el5-x86_64'

一旦这个模块被建成，你便可以删除不必要的符号：

[user@host hybrid-wl]$ strip --strip-debug wl.ko

你会发现驱动模块的文件尺寸会明显地缩小（由 2.2MB 降至 1.5MB）。而且，你的驱动模块仍能正常运作 ![;)]( ";)")

## 第 4 步上：将驱动模块装入内核中

当你成功地编译了驱动模块后，你便可以将它装入内核中，并设置在开机时自动装入这个驱动程序（要这样做，你必须利用 root 的权限）。当然，做这一切之先，你必须从内核删除现在的无线驱动模块（假如有的话）：
[root@host ~]# rmmod bcm43xx[root@host ~]# rmmod b43[root@host ~]# rmmod b43legacy[root@host ~]# rmmod ndiswrapper

现在我们装入驱动模块：

[root@host hybrid-wl]# insmod wl.ko

如果这一步失败了（有不少这样的报告，但是，就作者本身还没有发生过类似的情况），并伴有如下的提示信息：

insmod: error inserting 'wl.ko': -1 Unknown symbol in module

首先尝试构建模块依赖：

root@host ~]# depmod `uname -r`

之后再装载驱动模块：

[root@host hybrid-wl]# modprobe wl

假若模块的装入再次失败，请尝试先以手动方式插入 **ieee802.11_crypt_tkip** 这个依赖性模块，然后才继续装入 **wl** 这个驱动模块：
[root@host hybrid-wl]# modprobe ieee80211_crypt_tkip[root@host hybrid-wl]# modprobe wl

假如你在无线驱动程序以外没有应用 ndiswrapper 这个内核模块，你可以删除它，但这并非必需的。

## 第 4 步下：在开机时将驱动模块装入内核中

首先，请将驱动模块的文件复制到一个可以让内核找到它的地方：

[root@host hybrid-wl]# cp -vi /usr/local/src/hybrid-wl/wl.ko /lib/modules/`uname -r`/extra/

这样做是为了与其它已经／将会从 kmod 组件安装的外置模块（例如：fuse、ntfs-3g、等）保持一贯性。

按着，请执行：

[root@host ~]# depmod $(uname -r)

以便能创建一个模块的互赖性清单。

编辑 **/etc/modprobe.d/blacklist** 这个文件并加入以下内容：

blacklist bcm43xxblacklist ndiswrapperblacklist b43blacklist b43legacy

通过这样做，你可以避免这些模块在开机时被装入内核中。此外，假如你在 **/etc/modprobe.conf** 内有一行是指定无线界面的驱动程序，例如：

alias eth1 bcm43xx 或alias eth1 b43 或alias eth1 b43legacy

请将这行注释掉：

#alias eth1 bcm43xx 或#alias eth1 b43 或#alias eth1 b43legacy

并为你的无线网络卡加入新的驱动程序别名：

alias eth1 wl

这一切都假设你的无线网络界面设备档是 eth1。

现在，请编辑 **/etc/modprobe.d/modprobe.conf.dist** 这个文件并加入以下内容（某些情况下，以下内容或许不是必须的）：
alias ieee80211_crypt_tkip ieee80211_crypt_tkipalias eth1 wl

现在你的驱动应该在每次开机时都会被装入（当然除了在你安装了新内核之后，到时你必须依照以上步骤将它重新编译）。

![]( "attachment:ArtWork/WikiDesign/icon-admonition-alert.png")

**注意：** 这个驱动程序**不能**跨越内核升级（意即当你更新内核后用新内核开机，你必须重做以上步骤）。正因为这个原因，你要将压缩档的内容放置在 /usr/local/src/hybrid-wl 内，并更改目录及内容的拥有者。

![]( "attachment:ArtWork/WikiDesign/icon-admonition-info.png")

**注：**成功安装驱动程序后，无联机网的新手经常会汇报 **Error for wireless request "Set Encode" (8B2A): SET failed on device...** 等问题。最简便的解决方法就是 [设置 NetworkManager 守护服务](http://wiki.centos.org/HowTos/Laptops/NetworkManager)替换 **network** 守护服务来管理你的网络连接。

## 附录 A：在 CentOS 5.6 上编译（最新的？）驱动

显然，对最新发布的驱动来说，并不仅仅存在功能上的问题。同时，另一个 ' 编译时' 问题在 CentOS 5.6 上也浮现出来，首先注释掉广为人知的 TYPEDEF_BOOL 部分，开始我们的编译过程：
[milos@host hybrid-new]$ make -C /lib/modules/2.6.18-238.5.1.el5/build M=`pwd`make: Entering directory `/usr/src/kernels/2.6.18-238.5.1.el5-x86_64'  LD /tmp/hybrid-new/built-in.o  CC [M] /tmp/hybrid-new/src/shared/linux_osl.oIn file included from /tmp/hybrid-new/src/shared/linux_osl.c:17:/tmp/hybrid-new/src/include/typedefs.h:86: error: conflicting types for ‘bool’include/linux/types.h:36: error: previous declaration of ‘bool’ was hereIn file included from /tmp/hybrid-new/src/shared/linux_osl.c:19:/tmp/hybrid-new/src/include/linuxver.h:88: error: redefinition of typedef ‘work_func_t’include/linux/workqueue.h:22: error: previous declaration of ‘work_func_t’ was heremake[1]: *** [/tmp/hybrid-new/src/shared/linux_osl.o] Error 1make: *** [_module_/tmp/hybrid-new] Error 2make: Leaving directory `/usr/src/kernels/2.6.18-238.5.1.el5-x86_64'

注释掉 /usr/local/src/hybrid/src/include/linuxver.h 文件中有问题的第 88 行：

typedef void (*work_func_t)(void *work);

就可以轻松解决这个问题。

移除了上文提到的行之后，驱动的编译很顺利。坦诚的讲，我至今还没有尝试编译过早先版本的驱动，这也是我现在提出这个问题的根本原因。

Translation of revision 19
 

​和剩下的屏蔽地址：
9. 注意:移除所有其它的BROADCOM 无线设备驱动
# lsmod | grep "b43\|ssb\|bcma\|wl"
如果以下任何一种驱动存在，移除它:
# rmmod b43
# rmmod ssb
# rmmod bcma
# rmmod wl
移除后再
#lsmod | grep "b43\|ssb\|bcma\|wl"
此时，没有任何显示，将这些有冲突的驱动加入黑名单
# echo "blacklist ssb" >> /etc/modprobe.d/blacklist.conf
# echo "blacklist bcma" >> /etc/modprobe.d/blacklist.conf
# echo "blacklist b43" >> /etc/modprobe.d/blacklist.conf
10. 加入驱动
# depmod -a
# modprobe wl
11. #lsmod | grep "b43\|ssb\|bcma\|wl"
显示有 新加入的wl驱动，则驱动安装成功
12. 写如开机启动
echo modprobe wl >> /etc/rc.local
