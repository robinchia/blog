---
layout: post
title: "adb命令详解"
categories: linux
tags: 
 - linux
 - android
--- 

# adb命令详解

ADB全称Android Debug Bridge, 是android sdk里的一个工具, 用这个工具可以直接操作管理android模拟器或者真实的andriod设备(如G1手机).
它的主要功能有:
    * 运行设备的shell(命令行)
    * 管理模拟器或设备的端口映射
    * 计算机和设备之间上传/下载文件
    * 将本地apk软件安装至模拟器或android设备
ADB是一个 客户端-服务器端 程序, 其中客户端是你用来操作的电脑, 服务器端是android设备..
先说安装方法, 电脑上需要安装客户端. 客户端包含在sdk里. 设备上不需要安装, 只需要在手机上打开选项settings-applications-development-USB debugging.
对于Mac和Linux用户, 下载好的sdk解压后, 可以放~或者任意目录. 然后修改~/.bash_profile文件, 设置运行环境指向sdk的tools目录.
具体是打开~/.bash_profile文件(如果没有此文件也可以自行添加), 在里面加入一行:
export PATH=${PATH}:<你的sdk目录>/tools
然后就可以使用adb命令了.
嫌安装麻烦的同学其实也可以省去上面安装步骤, 直接输入完整路径来使用命令。
对于windows xp用户, 需要先安装usb驱动 android_usb_windows.zip, 然后如果你只打算使用adb而不想下载整个sdk的话, 可以下载这个单独的adb工具包 adb_win.zip 下载后解压, 把里面 adb.exe 和 AdbWinApi.dll 两个文件放到系统盘的 windows/system32 文件夹里就可以了
现在说下ADB常用的几个命令
查看设备
    * adb devices
这个命令是查看当前连接的设备, 连接到计算机的android设备或者模拟器将会列出显示
安装软件
    * adb install <apk文件路径>
这个命令将指定的apk文件安装到设备上.
卸载软件
    * adb uninstall <软件名>
    * adb uninstall -k <软件名>
如果加 -k 参数,为卸载软件但是保留配置和缓存文件.
登录设备shell
    * adb shell
    * adb shell <command命令>
这个命令将登录设备的shell.
后面加<command命令>将是直接运行设备命令, 相当于执行远程命令
从电脑上发送文件到设备
    * adb push <本地路径> <远程路径>
用push命令可以把本机电脑上的文件或者文件夹复制到设备(手机)
从设备上下载文件到电脑
    * adb pull <远程路径> <本地路径>
用pull命令可以把设备(手机)上的文件或者文件夹复制到本机电脑
显示帮助信息
    * adb help
这个命令将显示帮助信息
这里还有一个英文版的：
在DOS下输入以下命令基本可以完成刷机任务,一些常用命令解释如下:
    adb devices - 列出连接到电脑的ADB设备(也就是手机),一般显示出手机P/N码.如果没有显示出来则手机与电脑没有连接上.
    adb install <packagename.apk> – 安装手机软件到手机中,如:adb install qq2009.apk.
    adb remount – 重新打开手机写模式(刷机模式).
    adb push <localfile> <location on your phone> - 传送文件到手机中,如:adb push recovery.img /sdcard/recovery.img,将本地目录中的recovery.img文件传送手机的SD卡中并取同样的文件名.
    adb pull <location on your phone> <localfile> - 传送手机的文件到本地目录(和上命令相反).
    adb shell <command> - 让手机执行命令,<command>就是手机执行的命令.如: adb shell flash_image recovery /sd-card/recovery-RAv1.0G.img,执行将recovery-RAv1.0G.img写入到recovery 区中.
我们刷recovery时一般按下顺序执行:
    adb shell mount -a
    adb push recovery-RAv1.0G.img /system/recovery.img
    adb push recovery-RAv1.0G.img /sdcard/recovery-RAv1.0G.img
    adb shell flash_image recovery /sdcard/recovery-RAv1.0G.img reboot
其它的自己灵活运用了.
ADB命令详解:
Android Debug Bridge version 1.0.20
-d 　- directs command to the only connected USB device　returns an error if more than one USB device is　present.
-e  　- directs command to the only running emulator.returns an error if more than one emulator is running.
-s <serial number>            – directs command to the USB device or emulator withthe given serial number
-p <product name or path>     – simple product name like ‘sooner’, or　a relative/absolute path to a product　out directory like ‘out/target/product/sooner’.
If -p is not specified, the ANDROID_PRODUCT_OUT　environment variable is used, which must　be an absolute path.
devices 　 – list all connected devices
device commands:
adb push <local> <remote>    – copy file/dir to device
adb pull <remote> <local>    – copy file/dir from device
adb sync [ <directory> ]     – copy host->device only if changed　(see ‘adb help all’)
adb shell                    – run remote shell interactively
adb shell <command>          – run remote shell command
adb emu <command>            – run emulator console command
adb logcat [ <filter-spec> ] – View device log
adb forward <local> <remote> – forward socket connections
forward specs are one of:
    tcp:<port>
    localabstract:<unix domain socket name>
    localreserved:<unix domain socket name>
    localfilesystem:<unix domain socket name>
    dev:<character device name>
    jdwp:<process pid> (remote only)
adb jdwp　 – list PIDs of processes hosting a JDWP transport
adb install [-l] [-r] <file> – push this package file to the device and install it
(‘-l’ means forward-lock the app)
(‘-r’ means reinstall the app, keeping its data)
adb uninstall [-k] <package> – remove this app package from the device
(‘-k’ means keep the data and cache directories)
adb bugreport                – return all information from the device　that should be included in a bug report.
adb help                     – show this help message
adb version                  – show version num
DATAOPTS:
(no option)                   – don’t touch the data partition
-w                           – wipe the data partition
-d                           – flash the data partition
scripting:
adb wait-for-device          – block until device is online
adb start-server             – ensure that there is a server running
adb kill-server              – kill the server if it is running
adb get-state                – prints: offline | bootloader | device
adb get-serialno             – prints: <serial-number>
adb status-window            – continuously print device status for a specified device
adb remount                  – remounts the /system partition on the device re
ad-write
adb root                     – restarts adb with root permissions
networking:
adb ppp <tty> [parameters]   – Run PPP over USB.
Note: you should not automatically start a PDP connection.
<tty> refers to the tty for PPP stream. Eg. dev:/dev/omap_csmi_tty1
[parameters] – Eg. defaultroute debug dump local notty usepeerdns
adb sync notes: adb sync [ <directory> ]
<localdir> can be interpreted in several ways:
- If <directory> is not specified, both /system and /data partitions will be updated.
- If it is “system” or “data”, only the corresponding partition　is updated.
