---
layout: post
title: "adb命令拷贝文件"
categories: linux
tags: 
 - linux
 - android
--- 

# adb命令拷贝文件

To copy dirs, it seems you can use    

adb pull <remote> <local>
if you    want to copy file/dir from device,    and

adb push <local> <remote>
to    copy file/dir to device.    Alternatively, just to copy a file, you can use a simple    trick:

cat source_file >    dest_file
. 

Note that this does not work for user-inaccessible paths.

To edit files, I have not found a    simple solution, just some possible    workarounds. Try [this](http://benno.id.au/blog/2007/11/14/android-busybox), it seems    you can (after the setup) use it to    edit files like

busybox vi    <filename>
. Nano seems to be [possible to use](http://forum.xda-developers.com/showthread.php?t=556944) to.
