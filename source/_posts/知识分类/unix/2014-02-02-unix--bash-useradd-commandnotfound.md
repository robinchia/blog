---
layout: post
title: "bash- useradd -command not found"
categories: unix
tags: 
 - unix
--- 

# bash: useradd :command not found

## bash: useradd :command not found

在网上查了一下,发现应该如下操作：
利用su - ，而不是su进入root，然后再敲useradd test1,这样就OK了.
注意：是su - 而不是su，因为su是只取得ROOT的权限，su - 是取得ROOT的权限后还执行ROOT的PROFILE来取得ROOT的环境变量。

加了这个减号的目的是使环境变量和欲转换的用户相同、不加-是取得用户的临时权限
来源： <[http://hi.baidu.com/peng123/item/0751b74478ccefeabdf451f0](http://hi.baidu.com/peng123/item/0751b74478ccefeabdf451f0)> 
对于bash:useradd:command not found错误

在命令行中添加用户时显示bash:useradd:command not found的错误，在网上查了一下资料。
思路：
        在UNIX系统里面，每个系统用户都由自己的环境变量来定义自己登录上来的shell、终端类型、路径等。Linux下Bshell用户登录后执行主目录下的.bash_profile，Cshell用户执行.cshrc_profile文件。
        当以普通用户登录主机，而此用户的环境里没有定义系统命令所在的路径，如/usr/bin，/usr/sbin等；或在一些情况下TELNET上主机后也会遗失环境变量。
解决方法：
       1.在绝对路径/usr/sbin中执行；
       2.用root用户执行命令。用“su -”可以取得root用户的权限和环境（注：是“su -”不是“su”，因为“su”只取得root的权限，“su -”取得root权限后还执行root的profile来取得root的环境变量）
       3.如果确定要使用非root用户的当前用户来执行命令，需要把系统路径加到该用户的.bash_profile或者.cshrc_profile文件中去
附：
      useradd指令的用法
       作用：账户建立或更新使用者的信息
       语法：useradd  <选项>  用户名
       常用参数：
       1. -c comment
          新账号password档的说明栏
       2. -d home_dir
          新账号的家目录路径
       3. -e expire_date
          账号的截止日期，格式为MM/DD/YY
       4. -f inactive_days
          账号过期几日后永久封停。当值为0时账号立即被封停，当值为-1时则是关闭这个功能，默认为-1
       5. -g initial_group
          group名称或以数字来做为使用者登陆的起始组。组名必须是存在的名称
       6. -G group,[...]
          定义用户所属的其他组，可以有多个
       7. -m [-k skeleton_dir]
          用户家目录如果不存在则自动建立。如果使用-k选项，skeleton_dir内的文档会被复制到家目录下，而/etc/skel目录下的文档也会被复制过来
       8. -M
          强制不建立用户家目录，即使/etc/login.defs设定要建立家目录
       9. -s
          使用者登陆后使用的shell名称。预设是不填写，这样系统会指定预设的登陆shell
       10. -u
          手动设定用户的ID值，ID值必须是唯一的。
      
      userdel指令的用法

来源： <[http://blog.163.com/bhjbhj@yeah/blog/static/6792210920128351534109/](http://blog.163.com/bhjbhj@yeah/blog/static/6792210920128351534109/)>
 
