---
layout: post
title: "通过网络使用adb"
categories: linux
tags: 
 - linux
 - android
--- 

# 通过网络使用adb

在adb的说明文档中提到：

    “An ADB transport models a connection between the ADB server and one device
    or emulator. There are currently two kinds of transports:
       - USB transports, for physical devices through USB
       - Local transports, for emulators running on the host, connected to
         the server through TCP”

    大意是说，在物理设备上，adb是通过USB连接到设备上的，而在模拟器上，adb是通过TCP协议连接到设备上的。实际上在物理设备上，也可以让adb 通过TCP协议来连接设备（当然前提条件是你的设备要有网口）。首先看一下下面这段源代码，出自system/core/adb/adb.c，第921 行：

   

   /* for the device, start the usb transport if the
        ** android usb device exists and "service.adb.tcp"
        ** is not set, otherwise start the network transport.
        */
    property_get("service.adb.tcp.port", value, "0");
    if (sscanf(value, "%d", &port) == 1 && port > 0) {
        // listen on TCP port specified by service.adb.tcp.port property
        local_init(port);
    } else if (access("/dev/android_adb", F_OK) == 0) {
        // listen on USB
        usb_init();
    } else {
        // listen on default port
        local_init(ADB_LOCAL_TRANSPORT_PORT);
    }

    分析上述代码可以发现，在adbd启动时首先检查是否设置了service.adb.tcp.port，如果设置了，就是使用TCP作为连接方式；如果没 设置，就去检查是否有adb的USB设备（dev/android_adb)，如果有就用USB作为连接方式；如果没有USB设备，则还是用TCP作为连 接方式。

    因此只需要在启动adbd之前设置service.adb.tcp.port，就可以让adbd选则TCP模式，也就可以通过网络来连接adb了。这需要修改init.rc文件。如果不想修改，也可以在系统启动之后，在控制台上执行下列命令：

    #stop adbd

    #set service.adb.tcp.port 5555

    #start adbd

    这样就可以在主机上通过下列命令来连接设备了：

    adb connetc <ip-of-device>:5555
