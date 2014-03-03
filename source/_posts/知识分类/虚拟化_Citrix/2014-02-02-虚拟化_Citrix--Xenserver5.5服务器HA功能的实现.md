---
layout: post
title: "Xenserver5.5服务器HA功能的实现"
categories: 虚拟化_Citrix
tags: 
 - 虚拟化_Citrix
--- 

# Xenserver5.5服务器HA功能的实现

![]() ![]() Xenserver5.5 服务器 HA 功能的实现
Xenserver 最新的版本为 5.5，此版本支持最新的 HA（High Availability） 、 snapshot 等功能，最近发现挺多人询问关于 HA 功能的使用。其实 HA 的功能在 Xenserver 中实现起来非常简单，只要满足以下几个条件即可 （1） 虚拟机必须置于共享存储中，例如 iSCSI、FC SAN； （2） 需要两台以上的 Xenserver，并且设置了资源池 pool； （3） 所有 Xenserver 有静态 IP 地址； （4） 购买的 Xenserver 版本需要支持 HA（即 Enterprise 版本） ，具体的版本请 参考下列版本比较；
接下介绍如何在 Xenserver 中实现 HA 的功能 环境说明 由 于 没 有 实 际 的 物 理 服 务 器 来 安 装 Xenserver ， 只 能 选 择 在 VMware vSphere4.0 上安装 Xenserver，并安装了一台 Openfiler 来提供 iSCSI 服务。由于
1 / 17
![]() ![]() Xenserver5.5 服务器 HA 功能的实现
是在 VMware vSphere，因此，我丌能在安装好的 Xenserver 里启劢 Windows 的 虚拟机，只能安装一台 Linux 系统的虚拟主机来作为此次演示的虚拟机。 选 择 安 装 Windows XP 虚 拟 机 的 话 ， 在 启 劢 时 ， 会 出 现 如 下 提 示
服务器 Xenserver01 Xenserver02 Openfiler XenCenter
版本 5.5 5.5 2.3 5.5
IP 192.168.253.97 192.168.253.98 192.168.253.92 192.168.253.5
（1） 创建 Xenserver 资源池， 打开 XenCenter， 然后选择 “Tool” “New Tool” — ， 按照向导输入 Pool 的名称，然后添加我们的 Xenserver 服务器 （2） 为资源池 Xen 添加 iSCSI 共享存储。打开 XenCenter，选择“Storage”— “New Storage Repository” ，然后选择“iSCSI”
2 / 17
![]() ![]() ![]() Xenserver5.5 服务器 HA 功能的实现
（3） 输入 iSCSI 的名称和服务器地址，并输入 iSCSI 的传输端口，默认是 3260 输入后，点击“Discover IQNs”和“Discover LUNs”
（4） 由于是新增的 iSCSI，因此，需要先对其迚行格式化操作。
3 / 17
![]() ![]() ![]() ![]() Xenserver5.5 服务器 HA 功能的实现
（5） 完成后，为资源池 Xen 开启 HA 功能。
（6） 开启 HA 的配置向导
4 / 17
![]() ![]() ![]() Xenserver5.5 服务器 HA 功能的实现
（7） 选择 hearbeat SR，这里可以看到我们刚添加的 iSCSI 存储，选择该存储
5 / 17
![]() ![]() ![]() Xenserver5.5 服务器 HA 功能的实现
（8） 指定 HA 的保护级别，指定资源池中的 Xenserver 出现问题时，虚拟机可 以自劢启劢
（9） 完成 HA 的设置
6 / 17
![]() ![]() ![]() Xenserver5.5 服务器 HA 功能的实现
（10） 安装 Linux 虚拟机
7 / 17
![]() ![]() ![]() Xenserver5.5 服务器 HA 功能的实现
8 / 17
![]() ![]() ![]() Xenserver5.5 服务器 HA 功能的实现
9 / 17
![]() ![]() ![]() Xenserver5.5 服务器 HA 功能的实现
10 / 17
![]() ![]() ![]() Xenserver5.5 服务器 HA 功能的实现
（11） 修改 HA 的保护级别为“1” ，即开启保护
11 / 17
![]() ![]() ![]() Xenserver5.5 服务器 HA 功能的实现
（12） 虚拟机运行在 Xenserver02 上， 接下来， 我们模拟 Xenserver02 出现故障， 我们在 VMware vCenter 上将 Xenserver02 的网卡设置到其他网络中
12 / 17
![]() ![]() Xenserver5.5 服务器 HA 功能的实现
（13） 稍等片刻，在 XenCenter 中可以看到 Xenserver02 已经断开，虚拟机 “Redhat Enterprise Linux 5.2”自劢重启并切换到 Xenserver01 上运行
13 / 17
![]() ![]() ![]() Xenserver5.5 服务器 HA 功能的实现
（14） 再次查看资源池 Xen 的 HA 设置， 可以看到 Xenserver02 已经出现了故障， 由于只有单机，因此，HA 功能失效
14 / 17
![]() ![]() Xenserver5.5 服务器 HA 功能的实现
（15） 当资源池 Xen 中的两台 Xenserver 都恢复通讯正常后， 功能自劢恢复， HA 我们也可以根据需要对 HA 的保护级别迚行修改，指定当运行虚拟机的 Xenserver 出现故障时，HA 的保护劢作是自劢切换还是重启虚拟机。   “Protect“开启 HA 对虚拟机的保护功能 “Restart if Possible“在 Xenserver 出现故障时，如果可能，则在资源池中另 外的 Xenserver 上重新启劢虚拟机  “Do Not Restart“在 Xenserver 出现故障时，丌重启虚拟机
15 / 17
![]() ![]() ![]() Xenserver5.5 服务器 HA 功能的实现
16 / 17
![]() ![]() ![]() Xenserver5.5 服务器 HA 功能的实现
作者：Canvin Email：Jianshaowen@gmail.com
17 / 17
