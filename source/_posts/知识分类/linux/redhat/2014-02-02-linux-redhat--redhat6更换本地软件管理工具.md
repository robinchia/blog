---
layout: post
title: "redhat6更换本地软件管理工具"
categories: linux
tags: 
 - linux
 - redhat
--- 

# redhat6更换本地软件管理工具

[root@redhat6 robin]# yum install gnome-packagekit

Loaded plugins: aliases, auto-update-debuginfo, changelog, dellsysid,

              : downloadonly, fastestmirror, filter-data, fs-snapshot, keys,

              : list-data, local, merge-conf, post-transaction-actions, presto,

              : priorities, product-id, protectbase, ps, refresh-packagekit,

              : remove-with-leaves, rpm-warm-cache, security, show-leaves,

              : subscription-manager, tmprepo, tsflags, upgrade-helper, verify,

              : versionlock

This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.

Loading mirror speeds from cached hostfile

 * elrepo: elrepo.org

 * epel: mirrors.yun-idc.com

 * jpackage: sunsite.informatik.rwth-aachen.de

 * rpmforge: mirror.oscc.org.my

 * rpmfusion-nonfree-updates: ftp.sjtu.edu.cn

_local                                                   | 2.9 kB     00:00 ... 

_local/primary_db                                        |  89 kB     00:00 ... 

Skipping filters plugin, no data

246 packages excluded due to repository priority protections

0 packages excluded due to repository protections

Setting up Install Process

Resolving Dependencies

Skipping filters plugin, no data

--> Running transaction check

---> Package gnome-packagekit.x86_64 0:2.28.3-7.el6 will be installed

--> Processing Dependency: PackageKit-device-rebind >= 0.5.0 for package: gnome-packagekit-2.28.3-7.el6.x86_64

--> Running transaction check

---> Package PackageKit-device-rebind.x86_64 0:0.5.8-21.el6 will be installed

--> Finished Dependency Resolution

Dependencies Resolved

================================================================================

 Package                       Arch        Version              Repository

                                                                           Size

================================================================================

Installing:

 gnome-packagekit              x86_64      2.28.3-7.el6         base      2.5 M

Installing for dependencies:

 PackageKit-device-rebind      x86_64      0.5.8-21.el6         base       95 k

Transaction Summary

================================================================================

Install       2 Package(s)

Total download size: 2.6 M

Installed size: 8.1 M

Is this ok [y/N]: y

Downloading Packages:

Setting up and reading Presto delta metadata

Processing delta metadata

Package(s) data still to download: 2.6 M

(1/2): PackageKit-device-rebind-0.5.8-21.el6.x86_64.rpm  |  95 kB     00:00     

(2/2): gnome-packagekit-2.28.3-7.el6.x86_64.rpm          | 2.5 MB     00:14     

--------------------------------------------------------------------------------

Total                                           182 kB/s | 2.6 MB     00:14     

Running rpm_check_debug

Running Transaction Test

Transaction Test Succeeded

Running Transaction

  Installing : PackageKit-device-rebind-0.5.8-21.el6.x86_64                 1/2 

  Installing : gnome-packagekit-2.28.3-7.el6.x86_64                         2/2 

  Verifying  : gnome-packagekit-2.28.3-7.el6.x86_64                         1/2 

  Verifying  : PackageKit-device-rebind-0.5.8-21.el6.x86_64                 2/2 

Installed:

  gnome-packagekit.x86_64 0:2.28.3-7.el6                                        

Dependency Installed:

  PackageKit-device-rebind.x86_64 0:0.5.8-21.el6                                

Complete!

New leaves:

  gnome-packagekit.x86_64

替换本地yum源weiCentOS6的163源，见《Redhat 使用CentOS的yum源进行升级或软件安装（最新6.0-6.4，不含最新版6.5）》

在yum makecache后

执行：
yum remove PackageKit

[root@redhat6 robin]# yum update gnome-packagekit

Loaded plugins: aliases, auto-update-debuginfo, changelog, dellsysid,

              : downloadonly, fastestmirror, filter-data, fs-snapshot, keys,

              : list-data, local, merge-conf, post-transaction-actions, presto,

              : priorities, product-id, protectbase, ps, remove-with-leaves,

              : rpm-warm-cache, security, show-leaves, subscription-manager,

              : tmprepo, tsflags, upgrade-helper, verify, versionlock

This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.

Loading mirror speeds from cached hostfile

 * elrepo: elrepo.org

 * epel: mirrors.yun-idc.com

 * jpackage: mirror.ibcp.fr

 * rpmforge: mirror.oscc.org.my

 * rpmfusion-nonfree-updates: ftp.sjtu.edu.cn

Skipping filters plugin, no data

246 packages excluded due to repository priority protections

0 packages excluded due to repository protections

Setting up Update Process

No Packages marked for Update

[root@redhat6 robin]# yum remove PackageKit

Loaded plugins: aliases, auto-update-debuginfo, changelog, dellsysid,

              : downloadonly, fastestmirror, filter-data, fs-snapshot, keys,

              : list-data, local, merge-conf, post-transaction-actions, presto,

              : priorities, product-id, protectbase, ps, remove-with-leaves,

              : rpm-warm-cache, security, show-leaves, subscription-manager,

              : tmprepo, tsflags, upgrade-helper, verify, versionlock

This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.

Setting up Remove Process

Resolving Dependencies

--> Running transaction check

---> Package PackageKit.x86_64 0:0.5.8-21.el6 will be erased

--> Processing Dependency: PackageKit = 0.5.8-21.el6 for package: PackageKit-glib-0.5.8-21.el6.x86_64

--> Processing Dependency: PackageKit >= 0.5.0 for package: gnome-packagekit-2.28.3-7.el6.x86_64

--> Running transaction check

---> Package PackageKit-glib.x86_64 0:0.5.8-21.el6 will be erased

--> Processing Dependency: libpackagekit-glib2.so.12()(64bit) for package: PackageKit-gstreamer-plugin-0.5.8-21.el6.x86_64

--> Processing Dependency: PackageKit-glib = 0.5.8-21.el6 for package: PackageKit-gstreamer-plugin-0.5.8-21.el6.x86_64

--> Processing Dependency: PackageKit-glib = 0.5.8-21.el6 for package: PackageKit-gtk-module-0.5.8-21.el6.x86_64

--> Processing Dependency: PackageKit-glib = 0.5.8-21.el6 for package: PackageKit-device-rebind-0.5.8-21.el6.x86_64

---> Package gnome-packagekit.x86_64 0:2.28.3-7.el6 will be erased

--> Running transaction check

---> Package PackageKit-device-rebind.x86_64 0:0.5.8-21.el6 will be erased

---> Package PackageKit-gstreamer-plugin.x86_64 0:0.5.8-21.el6 will be erased

---> Package PackageKit-gtk-module.x86_64 0:0.5.8-21.el6 will be erased

--> Finished Dependency Resolution

Dependencies Resolved

================================================================================

 Package

      Arch   Version

                  Repository                                               Size

================================================================================

Removing:

 PackageKit

      x86_64 0.5.8-21.el6

                  @anaconda-RedHatEnterpriseLinux-201311111358.x86_64/6.5 2.3 M

Removing for dependencies:

 PackageKit-device-rebind

      x86_64 0.5.8-21.el6

                  @anaconda-RedHatEnterpriseLinux-201311111358.x86_64/6.5 231 k

 PackageKit-glib

      x86_64 0.5.8-21.el6

                  @anaconda-RedHatEnterpriseLinux-201311111358.x86_64/6.5 734 k

 PackageKit-gstreamer-plugin

      x86_64 0.5.8-21.el6

                  @anaconda-RedHatEnterpriseLinux-201311111358.x86_64/6.5 232 k

 PackageKit-gtk-module

      x86_64 0.5.8-21.el6

                  @anaconda-RedHatEnterpriseLinux-201311111358.x86_64/6.5 231 k

 gnome-packagekit

      x86_64 2.28.3-7.el6

                  @anaconda-RedHatEnterpriseLinux-201311111358.x86_64/6.5 7.9 M

Transaction Summary

================================================================================

Remove        6 Package(s)

Installed size: 12 M

Is this ok [y/N]: y

Downloading Packages:

Running rpm_check_debug

Running Transaction Test

Transaction Test Succeeded

Running Transaction

  Erasing    : gnome-packagekit-2.28.3-7.el6.x86_64                         1/6 

  Erasing    : PackageKit-device-rebind-0.5.8-21.el6.x86_64                 2/6 

  Erasing    : PackageKit-gstreamer-plugin-0.5.8-21.el6.x86_64              3/6 

  Erasing    : PackageKit-gtk-module-0.5.8-21.el6.x86_64                    4/6 

  Erasing    : PackageKit-0.5.8-21.el6.x86_64                               5/6 

  Erasing    : PackageKit-glib-0.5.8-21.el6.x86_64                          6/6 

Loading mirror speeds from cached hostfile

 * elrepo: elrepo.org

 * epel: mirrors.yun-idc.com

 * jpackage: mirror.ibcp.fr

 * rpmforge: mirror.oscc.org.my

 * rpmfusion-nonfree-updates: ftp.sjtu.edu.cn

246 packages excluded due to repository priority protections

0 packages excluded due to repository protections

  Verifying  : PackageKit-gstreamer-plugin-0.5.8-21.el6.x86_64              1/6 

  Verifying  : gnome-packagekit-2.28.3-7.el6.x86_64                         2/6 

  Verifying  : PackageKit-device-rebind-0.5.8-21.el6.x86_64                 3/6 

  Verifying  : PackageKit-gtk-module-0.5.8-21.el6.x86_64                    4/6 

  Verifying  : PackageKit-0.5.8-21.el6.x86_64                               5/6 

  Verifying  : PackageKit-glib-0.5.8-21.el6.x86_64                          6/6 

Removed:

  PackageKit.x86_64 0:0.5.8-21.el6                                              

Dependency Removed:

  PackageKit-device-rebind.x86_64 0:0.5.8-21.el6                                

  PackageKit-glib.x86_64 0:0.5.8-21.el6                                         

  PackageKit-gstreamer-plugin.x86_64 0:0.5.8-21.el6                             

  PackageKit-gtk-module.x86_64 0:0.5.8-21.el6                                   

  gnome-packagekit.x86_64 0:2.28.3-7.el6                                        

Complete!

然后，执行yum install PackageKit

[root@redhat6 robin]# yum install PackageKit

Loaded plugins: aliases, auto-update-debuginfo, changelog, dellsysid,

              : downloadonly, fastestmirror, filter-data, fs-snapshot, keys,

              : list-data, local, merge-conf, post-transaction-actions, presto,

              : priorities, product-id, protectbase, ps, remove-with-leaves,

              : rpm-warm-cache, security, show-leaves, subscription-manager,

              : tmprepo, tsflags, upgrade-helper, verify, versionlock

This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.

Loading mirror speeds from cached hostfile

 * elrepo: elrepo.org

 * epel: mirrors.yun-idc.com

 * jpackage: mirror.ibcp.fr

 * rpmforge: mirror.oscc.org.my

 * rpmfusion-nonfree-updates: ftp.sjtu.edu.cn

Skipping filters plugin, no data

246 packages excluded due to repository priority protections

0 packages excluded due to repository protections

Setting up Install Process

Resolving Dependencies

Skipping filters plugin, no data

--> Running transaction check

---> Package PackageKit.x86_64 0:0.5.8-21.el6 will be installed

--> Processing Dependency: PackageKit-yum-plugin = 0.5.8-21.el6 for package: PackageKit-0.5.8-21.el6.x86_64

--> Processing Dependency: PackageKit-yum = 0.5.8-21.el6 for package: PackageKit-0.5.8-21.el6.x86_64

--> Processing Dependency: PackageKit-gtk-module = 0.5.8-21.el6 for package: PackageKit-0.5.8-21.el6.x86_64

--> Processing Dependency: PackageKit-glib = 0.5.8-21.el6 for package: PackageKit-0.5.8-21.el6.x86_64

--> Processing Dependency: libpackagekit-glib2.so.12()(64bit) for package: PackageKit-0.5.8-21.el6.x86_64

--> Running transaction check

---> Package PackageKit-glib.x86_64 0:0.5.8-21.el6 will be installed

---> Package PackageKit-gtk-module.x86_64 0:0.5.8-21.el6 will be installed

---> Package PackageKit-yum.x86_64 0:0.5.8-21.el6 will be installed

---> Package PackageKit-yum-plugin.x86_64 0:0.5.8-21.el6 will be installed

--> Finished Dependency Resolution

Dependencies Resolved

================================================================================

 Package                     Arch         Version              Repository  Size

================================================================================

Installing:

 PackageKit                  x86_64       0.5.8-21.el6         base       526 k

Installing for dependencies:

 PackageKit-glib             x86_64       0.5.8-21.el6         base       221 k

 PackageKit-gtk-module       x86_64       0.5.8-21.el6         base        95 k

 PackageKit-yum              x86_64       0.5.8-21.el6         base       156 k

 PackageKit-yum-plugin       x86_64       0.5.8-21.el6         base        92 k

Transaction Summary

================================================================================

Install       5 Package(s)

Total download size: 1.1 M

Installed size: 3.9 M

Is this ok [y/N]: y

Downloading Packages:

Setting up and reading Presto delta metadata

Processing delta metadata

Package(s) data still to download: 1.1 M

(1/5): PackageKit-0.5.8-21.el6.x86_64.rpm                | 526 kB     00:01     

(2/5): PackageKit-glib-0.5.8-21.el6.x86_64.rpm           | 221 kB     00:00     

(3/5): PackageKit-gtk-module-0.5.8-21.el6.x86_64.rpm     |  95 kB     00:01     

(4/5): PackageKit-yum-0.5.8-21.el6.x86_64.rpm            | 156 kB     00:00     

(5/5): PackageKit-yum-plugin-0.5.8-21.el6.x86_64.rpm     |  92 kB     00:00     

--------------------------------------------------------------------------------

Total                                           148 kB/s | 1.1 MB     00:07     

Running rpm_check_debug

Running Transaction Test

Transaction Test Succeeded

Running Transaction

  Installing : PackageKit-yum-0.5.8-21.el6.x86_64                           1/5 

  Installing : PackageKit-gtk-module-0.5.8-21.el6.x86_64                    2/5 

  Installing : PackageKit-glib-0.5.8-21.el6.x86_64                          3/5 

  Installing : PackageKit-0.5.8-21.el6.x86_64                               4/5 

  Installing : PackageKit-yum-plugin-0.5.8-21.el6.x86_64                    5/5 

  Verifying  : PackageKit-yum-plugin-0.5.8-21.el6.x86_64                    1/5 

  Verifying  : PackageKit-yum-0.5.8-21.el6.x86_64                           2/5 

  Verifying  : PackageKit-gtk-module-0.5.8-21.el6.x86_64                    3/5 

  Verifying  : PackageKit-glib-0.5.8-21.el6.x86_64                          4/5 

  Verifying  : PackageKit-0.5.8-21.el6.x86_64                               5/5 

Installed:

  PackageKit.x86_64 0:0.5.8-21.el6                                              

Dependency Installed:

  PackageKit-glib.x86_64 0:0.5.8-21.el6                                         

  PackageKit-gtk-module.x86_64 0:0.5.8-21.el6                                   

  PackageKit-yum.x86_64 0:0.5.8-21.el6                                          

  PackageKit-yum-plugin.x86_64 0:0.5.8-21.el6                                   

Complete!

之后，在执行：yum install gnome-packagekit

即完成替换了。
