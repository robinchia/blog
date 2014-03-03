---
layout: post
title: "rdesktop安装和使用"
categories: linux
tags: 
 - linux
 - redhat
--- 

# rdesktop安装和使用

* 
到官方网站去下载最新版的rdesktop,
[http://www.rdesktop.org](http://www.rdesktop.org/)
，目前最新版本为：rdesktop 1.5.0,Source(211KB)
* 
[root@lvdbing software]# tar -xvzf rdesktop-1.5.0.tar.gz （解压）
* 
[root@lvdbing software]# cd rdesktop-1.5.0;　./configure
* 
[root@lvdbing rdesktop-1.5.0]# make;　make install　(编译，安装)　              注：在安装CentOS时，记得要把开发工具包给安装上，否则可能安装不成功
* 
[root@lvdbing rdesktop-1.5.0]# ls -l /usr/local/bin/rdesktop
-rwxr-xr-x 1 root root 182708 Jan 14 14:00 /usr/local/bin/rdesktop　(确认安装)

最后：测试连接

* 
编写选择脚本　(因为运行的windows服务器不‍只一台)
* 
[root@lvdbing Desktop]# cat SelectHost.sh
#!/bin/bash
echo "Select Rmote Hostname:"
echo "1): 192.168.1.30"
echo "2): 192.168.1.40"
echo "3): 192.168.1.50"
echo ""
echo "Please select remote host :"
read host
case "$host" in
1)      rdesktop -f -a 16 192.168.1.30
        ;;
2)      rdesktop -f -a 16 192.168.1.40
        ;;
3)      rdesktop -f -a 16 192.168.1.50
        ;;
*)      echo "Error,Please select 1,2 or 3: "
        ;;
esac
#其中选项-f为全屏显示,-a 16 为显示像素为16, 192.168.1.30为主机名
* 
使用Ctrl+Alt+Enter可以退出全屏.
* 
连接效果图　
