---
layout: post
title: "linux下adb工具的安装"
categories: linux
tags: 
 - linux
 - android
--- 

# linux下adb工具的安装

第一步：启动开发板，进入android系统后，在linux终端输入lsusb命令查询USB总线上的设备，比如我这里查询结果如下：

我这里是:
Bus 007 Device 009: ID 18d1:4e12

第二步：下载最新的android SDK并解压到某目录，下载地址：
[http://developer.android.com/sdk/index.html](http://developer.android.com/sdk/index.html)
截至目前最新的SDK为android-sdk_r12-linux_x86.tgz
解压出来的名称为android-sdk-linux_x86
进入下面目录：
cd android-sdk-linux_x86/tools/
./android update adb

第三步：创建一个新的udev规则的文件，在/etc/udev/rules.d路径下，新建名为imx-android.rules的文件，编辑内容如下：
vim /etc/udev/rules.d/50-android.rules

文件里添加如下配置参数:

#Acer      0502

SUBSYSTEM=="usb", SYSFS{idVendor}=="0502", MODE="0666"

#Dell     413c

SUBSYSTEM=="usb", SYSFS{idVendor}=="413c", MODE="0666"

#Foxconn     0489

SUBSYSTEM=="usb", SYSFS{idVendor}=="0489", MODE="0666"

#Garmin-Asus     091E

SUBSYSTEM=="usb", SYSFS{idVendor}=="091e", MODE="0666"

#HTC     0bb4

SUBSYSTEM=="usb", SYSFS{idVendor}=="0bb4", MODE="0666"

#Huawei     12d1

SUBSYSTEM=="usb", SYSFS{idVendor}=="12d1", MODE="0666"

#Kyocera     0482

SUBSYSTEM=="usb", SYSFS{idVendor}=="0482", MODE="0666"

#LG     1004

SUBSYSTEM=="usb", SYSFS{idVendor}=="1004", MODE="0666"

#Motorola     22b8

SUBSYSTEM=="usb", SYSFS{idVendor}=="22b8", MODE="0666"

#Nvidia     0955

SUBSYSTEM=="usb", SYSFS{idVendor}=="0955", MODE="0666"

#Pantech     10A9

SUBSYSTEM=="usb", SYSFS{idVendor}=="10A9", MODE="0666"

#Samsung     04e8

SUBSYSTEM=="usb", SYSFS{idVendor}=="04e8", MODE="0666"

#Sharp     04dd

SUBSYSTEM=="usb", SYSFS{idVendor}=="04dd", MODE="0666"

#Sony Ericsson     0fce

SUBSYSTEM=="usb", SYSFS{idVendor}=="0fce", MODE="0666"

#ZTE     19D2

SUBSYSTEM=="usb", SYSFS{idVendor}=="19D2", MODE="0666

#MEIZU     18d1

SUBSYSTEM=="usb", SYSFS{idVendor}=="18d1", MODE="0666

但是这上面的ID，并不能包括所有。
解决办法是你可以使用lsusb命令查看你的USB ID

第四步：在/etc/profile中声明adb的路径:

export PATH=/usr/local/android-sdk-linux/platform-tools:$PATH

第五步：重启ADB
adb kill-server
adb start-server

第六步：使用adb devices命令查找设备：

[root@redhat6 platform-tools]# adb devices

List of devices attached

353BCHHRB44F    device

至此，安装成功。

你可以进去设备了，用adb shell，可以操作你到android了。
