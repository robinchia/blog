---
layout: post
title: "本教程为在win7系统中安装苹果 Mountain Lion 双系统的图文教程"
categories: mac
tags: 
 - mac
--- 

# 本教程为在win7系统中安装苹果 Mountain Lion 双系统的图文教程

**本教程为在win7系统中安装苹果 Mountain Lion 双系统的图文教程， 如果仅是想要安装苹果尝鲜，建议在虚拟机中安装，请参考：**[**VMware虚拟机安装MAC OS X Mountain Lion详细图文教程**](http://diybbs.zol.com.cn/1/34037_629.html)
如果对PC机安装黑苹果有兴趣的朋友，请详细观看本教程，以免带来不必要的麻烦。
**在安装苹果MAC OS X双系统之前，确认BIOS可以开启AHCI(必须)**
**安装苹果MAC OS X双系统步骤：
**A、工具准备
B、制作维护盘
C、分区并写入维护盘镜像MacPE
D、安装windows版变色龙
E、变色龙引导制作完整安装盘
F、变色龙引导完整安装盘安装Lion系统
**A、工具准备**
1. Win7下软件准备（A-E下载：[Windows下相关软件.rar](http://bbsdown.zol.com.cn/down.php?aid=585552)(大小7138k,下载次数:41830)，F：[**下载地址**](http://xiazai.zol.com.cn/detail/42/418614.shtml)）
a. Windows版Chameleon
b. HFS+Explorer、JRE
c. TransMac9.1
d. [硬盘](http://detail.zol.com.cn/hard_drives_index/subcate2_list_1.html)安装助手
e. DiskGenius
f. JRE
2. 安装盘文件准备（下载地址：[安装盘所需文件准备.part1.rar](http://bbsdown.zol.com.cn/down.php?aid=585554)(大小15729k,下载次数:30937) [安装盘所需文件准备.part2.rar](http://bbsdown.zol.com.cn/down.php?aid=585555)(大小13237k,下载次数:24621)）
a. MAC OS X 系统安装文件
b. 根目录放置Extra文件夹，其目录结构如下：
Extra/dsdt.aml (非必需, 假如碰到AppleACPIPlatform.kext、IOPCIFamily.kext的错误请尝试加上自己的dsdt)
Extra/org.chameleon.Boot.plist （1105版变色龙起，之前版本请用com.apple.Boot.plist）
Extra/smbios.plist （非必需）
Extra/Extensions/AppleACPIPS2Nub.kext
Extra/Extensions/ApplePS2Controller.kext
Extra/Extensions/FakeSMC.kext
Extra/Extensions/NullCPUPowerManagement.kext
c. App、Utilities.plist、InstallerMenuAdditions.plist和Frameworks & PrivateFrameworks(制作维护盘需要)。
d. Mac OS X 镜像（下载：[InstallESD.rar](http://bbsdown.zol.com.cn/down.php?aid=585553)(大小43k,下载次数:6923)）
**B、制作维护盘**
1. 安装jre，之后安装HFS+Explorer.exe。
2. 用7-zip提取InstallESD-10.8.dmg至InstallESD-10.8文件夹，之后会在InstallESD-10.8InstallMacOSX.pkg下得到InstallESD.dmg
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片1]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657219)
3. 提取InstallESD.dmg。打开HFS+Explorer, 点file-load file system from file…，载入刚才解压得到的InstallESD-10.8InstallMacOSX.pkgInstallESD.dmg，弹出对话框选择 Apple_HFS 后点确定
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片2]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657220)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片3]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657221)
4. 提取BaseSystem.dmg、mach_kernel及Packages文件夹，提取到Temp目录，弹出对话框选是。
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片4]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657222)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片5]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657223)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片6]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657224)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片7]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657225)
5. 用HFSExplorer载入BaseSystem.dmg，打开菜单Tools-Create disk image...重新打包建立新的可写盘dmg，命名为BaseSystem-Install，保存在Temp文件夹下面
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片8]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657226)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片9]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657227)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片10]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657228)
6. 替换Osinstall，放置Extra、mach_kernal、及 Packages 文件夹。打开TransMac9.1，菜单file-open disk image，打开刚才保存的BaseSystem-Install.dmg文件
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片11]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657229)
a. 把Extra文件夹及mach_kernel拖拽复制至TransMac窗口根目录HFS+ Volume文件夹下
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片12]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657230)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片13]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657231)
b. 把OSInstall拖拽至System/Library/PrivateFrameworks/Install.framework/Frameworks/OSInstall.framework/Versions/A/文件夹下替换原文件
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片14]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657232)
c. 删除System/Installation/下的Packages快捷方式并新建一个Packages文件夹，并把刚提取的Packages下的OSInstall.mpkg文件拖拽拷贝至Packages文件夹。我们这里不拷贝整个Packages到System/Installation/下，不然会提示空间不足无法拷贝完。
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片15]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657233)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片16]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657234)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片17]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657235)
7. 实际上进行到上一步，就完成了大家常说的懒人版的制作过程。剩下Packages文件夹该怎么办呢，大家常用的方法是先用硬盘助手把镜像写入硬盘，再安装MacDriver后进win系统反复一堆操作之后打开mac盘去拷贝替换这些文件。这里不使用这个方法，我们会给这个dmg添加三个软件：Finder、Chameleon Wizard和invisibliX。这样我们之后增删文件及安装mac版变色龙等操作就不再需要win系统，直接引导维护盘就可以进行之后操作。下面我们多做几步：
a. 将Finder拖拽复制到System／Library／CoreServices目录内(Finder要对应系统版本，这里是10.8)：[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片18]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657236)
b. 将Chameleon Wizard和invisibliX拖拽复制到/Applications/Utilities下
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片19]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657237)
c. 解压 Frameworks & PrivateFrameworks.7z后到Frameworks & PrivateFrameworks文件夹，拖拽复制SystemLibraryFrameworks下的Collaboration.framework和AddressBook.framework到dmg的对应目录，拖拽复制SystemLibraryPrivateFrameworks下的PhoneNumbers.framework和InternetAccounts.framework到dmg的对应目录(Frameworks & PrivateFrameworks同样对应系统版本，这里是10.8)：
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片20]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657238)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片21]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657239)
d. 将InstallerMenuAdditions.plist和Utilities.plist分别拖拽复制到dmg的/System/Installation/CDIS/Mac OS X Installer.app/Contents/Resources/和/System/Installation/CDIS/Mac OS X Utilities.app/Contents/Resources/目录替换原文件(作用是在menubar上添加Finder和软件的菜单，并可以在安装器和实用工具之间来回切换，大家可以参考这两个文件的内容照猫画虎，若用用记事本修改，保存时需用utf-8编码。另外只有少数几个软件可以在安装盘下运行)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片22]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657240)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片23]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657241)
这样类似PE的dmg维护盘镜像便制作完成，体积不大，可写入U盘或者[移动硬盘](http://detail.zol.com.cn/move_disk_index/subcate274_list_1.html)的一个分区，作增改文件维护之用。我们称之为MacPE、维护盘都可以，也可以叫它没有packages的懒人版，且在接下来加入Packages文件夹制作完整安装盘的操作过程也不再依靠Win环境和MacDriver，仅依靠该dmg镜像即可。
**C、分区并写入维护盘镜像MacPE**
说在前面，建议新手或者不细心的朋友再第一次做分区及写镜像操作时在移动硬盘上进行，若你的机器没办法从移动硬盘引导，则建议在进行分区及后续操作之前先用DiskGenius备份硬盘的分区表，之后把分区表保存在本机之外的地方(若你的WinPE是32位就用32位的DiskGenius来备份，64位DiskGenius备份出来的可能用不了)：
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片24]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657242)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片25]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657243)
分区概况：
1.5G - PE维护盘(BaseSystem-Install.dmg)5G - 完整安装盘(BaseSystem-Install.dmg+Packages)10G - Lion系统(若你打算把Lion作为常用系统的话建议分60G以上)
先保证有30G左右的空闲空间，我这里的例子，已有的主分区不超过2个，100M保留空间已占用一个主分区(如果已经有3个主分区，那新压缩的分区都只能是逻辑分区不能转为主分区，或者你无需努力尝试转为主分区，逻辑分区亦可，在逻辑分区上安装后可能需要借助WinPE来修复Win系统的引导及激活Win分区为活动分区，这里不表。注：转换为逻辑分区请不要把win系统盘转为逻辑分区了，如果你实在转不了或者完全没有头绪，建议转主分区这几个过程略过即可)。
a. 桌面，我的[电脑](http://detail.zol.com.cn/desktop_pc_index/subcate27_list_1.html)右键选择管理，打开的界面选择磁盘管理。从最后一个分区开始压缩，我们这里先压缩25G左右给安装盘和Lion系统盘，你也可以多压缩一些能多分一个区来给mac系统保存数据。
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片26]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657244)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片27]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657245)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片28]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657246)
b. 在未分配上点右键选 新建简单卷，分配1.5G给维护盘，自动分配一个驱动器号，不格式化。
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片29]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657247)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片30]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657248)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片31]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657249)
在剩余空间分配5G左右给安装盘，剩下的都分给Lion系统，不分配驱动器号，不格式化
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片32]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657251)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片33]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657252)
c. 打开DiskGenius，以我的操作为例，先把E盘转为逻辑分区，然后把在刚才分出来的20G左右分区上点右键选择“转换为主分区”，最后点保存更改(注意：转换为逻辑分区请不要把win系统盘转为逻辑分区了，如果你实在转不了或者完全没有头绪，建议转主分区这几个过程略过即可)。
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片34]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657333)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片35]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657254)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片36]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657255)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片37]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657256)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片38]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657257)
作为示例，这样分出来的分区图貌似不大好看，不过目的达到了。我的建议是如果想重新来过的话，弄掉那100M隐藏分区，硬盘分区的时候前面可以准备两三个主分区，这样扩展分区就排在后面。
d. 以管理员方式运行硬盘安装助手，选择刚才制作的BaseSystem-Install.dmg，勾掉红圈的地方，目标分区选刚分出来的安装分区，点开始后不要做其他操作，等待日志出现Change partition type to AF: Success. All done, have fun!
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片39]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657258)
e. 打开DiskGenius，修改刚才三个盘的分区参数，手动输入AF，确认后点保存更改。
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片40]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657259)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片41]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657260)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片42]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657261)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片43]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657262)
**D、Windows版变色龙安装**
管理员方式运行Chameleon Install 1806 for win下的Chameleon Install.exe，点安装。如果有100M隐藏分区，要先打开磁盘管理给该分区分配一个盘符，然后点安装，安装后再删掉100M分区盘符
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片44]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657263)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片45]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657264)
**E、变色龙引导制作完整安装盘**
安装完变色龙之后重启，选择chameleon选项
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片46]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657265)
选择最后一个苹果图标的选项，底部boot处可以输入一些命令，比如输入-v，可以命令行方式引导方便我们查看可能碰到的问题，若什么都不输则以图形方式引导，之后按回车继续
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片47]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657266)
之后按回车继续
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片48]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657267)
等待一点时间，会引导至如下界面
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片49]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657268)
下拉选择框内选择一种语言之后继续
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片50]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657269)
在menubar上点“实用工具”，选择“磁盘工具”
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片51]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657415)
文件-菜单-打开磁盘映象，找到BaseSystem-Install.dmg并打开
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片52]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657416)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片53]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657417)
先选择之前的5G分区，切换到抹掉标签，格式改为Mac OS扩展(日志式)，然后抹掉
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片54]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657418)
接下来点选恢复标签，Mac OS X Base System设为源磁盘，之前的5G为目的磁盘，确认无误后点恢复按钮，弹出对话框选抹掉
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片55]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657419)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片56]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657422)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片57]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657423)
接下来会把Packages文件夹拷贝至该安装盘分区内
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片58]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657424)
关闭磁盘工具后在menubar上点“实用工具”，选择“Finder…”，menubar上会显示Finder菜单，文件下拉选择“新建Finder窗口”
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片59]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657426)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片60]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657427)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片61]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657428)
菜单，选Finder偏好设置，边栏标签里勾选“电脑”，进入如图所示目录设置显示选项
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片62]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657430)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片63]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657431)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片64]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657432)
勾选项目简介后方便识别刚写入的安装盘，在安装盘上点右键显示简介，把名称更改为Lion 10.7.3 Install方便我们识别
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片65]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657433)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片66]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657435)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片67]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657437)
新建一个Finder窗口，找到最开始提取出来的Packages文件夹，拷贝到Lion 10.7.3 Install/System/Installation路径，覆盖原文件夹
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片68]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657438)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片69]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657439)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片70]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657440)
至此，完整的安装盘已经制作完毕。重启引导进入Lion系统安装过程
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片71]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657441)
**F、变色龙引导完整安装盘安装Lion系统**
与E步骤引导过程一样，这里选择Lion 10.7.3 Install引导系统安装，若要看命令行方式，则在左下角输入-v，方便我们观察引导过程中如果出现失败可以知道大概出在什么问题上
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片72]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657442)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片73]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657443)
进入安装界面后，选择实用工具下的磁盘工具，选择分给Lion的分区，设置如图，然后抹掉，之后关闭磁盘工具
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片74]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657444)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片75]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657447)
安装器中点继续
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片76]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657448)
选择Lion分区，单击安装，然后等待10分钟左右
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片77]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657450)
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片78]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657453)
安装完毕，重启
[![Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程图片79]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=6657455)
**论坛火爆热帖推荐：**
**[U盘安装Windows8教程](http://diybbs.zol.com.cn/1/34036_894.html)
****[Win8 硬盘安装过程图解](http://diybbs.zol.com.cn/1/34036_1212.html)
**[**Windows 8 RTM正式版试用**](http://diybbs.zol.com.cn/1/34036_1214.html)
[**Windows 8 Enterprise N RTM正式版下载（32位+64位+简体、繁体语言包下载）**](http://diybbs.zol.com.cn/1/34036_1211.html)

来源： <[【Win7下安装苹果MAC OS X Mountain Lion 双系统详细图文教程】-MAC OS论坛-ZOL中关村在线](http://diybbs.zol.com.cn/1/34037_630.html)>
 
