---
layout: post
title: "centos6.5 安装fcitx"
categories: linux
tags: 
 - linux
 - redhat
--- 

# centos6.5 安装fcitx

    因为选择的是最小安装，所有需要安装很多包啦
**一、首先把ibus卸载啦**
yum remove ibus
**二、****Fcitx，依赖于：**
1、安装intltool

2、可以用 yum install xxx软件包名，来提前安装它们，解决依赖问题。（这一步很重要！）
yum  install
perl-XML-Parser
libtoolize
gettext
gettext-devel
libXft
libXft-devel
libXpm
libXpm-devel  
automake
autoconf
libXtst-devel
gtk+-devel
gcc
zlib-devel
libpng-devel  
gtk2-devel
glib-devel
缺少libxml2

安装libxml2-devel
**三、下载安装fcitx
**

1、下载Fcitx最新版本的源码包：wget http://www.fcitx.org/download/fcitx-3.6.3.tar.bz2
2、解压缩到 /usr/src 目录下：tar jxvf fcitx-3.6.3.tar.bz2 -C /usr/src
3、进入目录：cd /usr/src/fcitx-3.6.3
4、生成”.configure“文件： ./autogen.sh
5、开始编译： ./configure
6. 正式安装：
make
make install
7. 测试下是否安装成功：输入fcitx -h，如果安装成功，应该能得到帮助文件的，如下：
[root@CentOS ~]# fcitx -h
Usage: fcitx [OPTION]
-d run as daemon(default)
-D don’t run as daemon
-c (re)create config file in home directory and then exit
-n[im name] run as specified name
-v display the version information and exit
-h display this help and exit
**三、配置Fcitx为默认输入法**
方法一：（推荐此法！）
1. 新建配置文件 vim /etc/X11/xinit/xinput.d/fcitx.conf ，内容为：
XIM=fcitx
XIM_PROGRAM=/usr/local/bin/fcitx
XIM_ARGS=”-d”
GTK_IM_MODULE=fcitx
QT_IM_MODULE =fcitx
2. 然后在/etc/alternatives/目录下，将符号链接xinputrc删除，重新建一个：
mv /etc/alternatives/xinputrc /etc/alternatives/xinputrc.bak
rm –rf /etc/alternatives/xinputrc
ln -s /etc/X11/xinit/xinput.d/fcitx.conf /etc/alternatives/xinputrc
注：如果你使用的桌面是英文环境的，还需要在使用用户的用户目录.bashrc配置文件里添加如下内容：
export LANG=”zh_CN.UTF-8″
export LC_CTYPE=”zh_CN.UTF-8″
export XIM=fcitx
export XIM_PROGRAM=fcitx
export GTK_IM_MODULE=xim
export XMODIFIERS=”@im=fcitx”
方法二：（此法在CentOS 5.3下可以，在5.5里有问题。）
1. 新建配置文件：vim /etc/X11/xinit/xinput.d/fcitx，内容为：
XIM=fcitx
XIM_PROGRAM=fcitx
GTK_IM_MOUDLE=fcitx
QT_IM_MOUDLE=fcitx
保存退出，重启电脑
2. 查询Fcitx是否开机运行。终端下输入：fcitx，应该是提示：Start FCITX error. Another XIM daemon named SCIM is running？这样就对了，直接到”4“
3. 如果没任何提示，则：ln -s /etc/X11/xinit/Xinput.d/fcitx /$HOME/.xinputrc
4. 输入： fcitx -nb ，即可看到输入框
默认fcitx启动后，是在后台运行的，因此看不到输入框，用上面的命令就可以调出来了。
ctrl+空格 切换输入法。
配置fcitx输入法修改vim ~/.fcitx/config文件中的相应偏好设置。
注意：如果你的输入法安装了，但是又不能按ctrl+space杂办，是因为你还缺少啦一个库文件
yum install gtk2-immodule-xim
安装好就可以使用啦
******四、卸载方法**
进入目录：cd /usr/src/fcitx-3.6.3
卸载：make uninstall
附：我的fcitx配置文件如下 vim ~/.fcitx/config
**五、配置文件**
[程序]
显示字体(中)=宋体
显示字体(英)=Courier
显示字体大小=20
主窗口字体大小=12
是否使用AA字体=1
[输出]
数字后跟半角符号=1
Enter键行为=1
分号输入英文=0
大写字母输入英文=1
联想方式禁止翻页=1
LumaQQ支持=0
[界面]
候选词个数=9
主窗口是否使用3D界面=0
输入条使用3D界面=1
主窗口隐藏模式=0
是否自动隐藏输入条=0
#输入条固定宽度仅适用于码表输入法，0表示不固定宽度
输入条固定宽度=600
序号后加点=0
显示打字速度=1
光标色=92 210 131
主窗口背景色=220 220 220
主窗口线条色=100 180 255
主窗口输入法名称色=170 170 170 150 200 150 0 0 255
输入窗背景色=240 240 240
输入窗提示色=255 0 0
输入窗用户输入色=0 0 255
输入窗序号色=200 0 0
输入窗第一个候选字色=0 150 100
#该颜色值只用于拼音中的用户自造词
输入窗用户词组色=0 0 255
输入窗提示编码色=100 100 255
#五笔、拼音的单字/系统词组均使用该颜色
输入窗其它文本色=0 0 0
输入窗线条色=100 200 255
输入窗箭头色=255 150 255
虚拟键盘窗背景色=220 220 220
虚拟键盘窗字母色=80 0 0
虚拟键盘窗符号色=0 0 0
#除了“中英文快速切换键”外，其它的热键均可设置为两个，中间用空格分隔
[热键]
打开/关闭输入法=CTRL_SPACE
#中英文快速切换键 可以设置为L_CTRL R_CTRL L_SHIFT R_SHIFT
中英文快速切换键=L_CTRL
双击中英文切换=0
击键时间间隔=250
光标跟随=CTRL_K
GBK支持=CTRL_M
联想支持=CTRL_L
反查拼音=CTRL_ALT_E
全半角=SHIFT_SPACE
中文标点=ALT_SPACE
上一页=-
下一页==
第二三候选词选择键=SHIFT
[输入法]
使用拼音=1
使用双拼=0
使用区位=1
使用码表=1
提示词库中的词组=0
[拼音]
使用全拼=0
拼音自动组词=1
保存自动组词=0
增加拼音常用字=CTRL_8
删除拼音常用字=CTRL_7
删除拼音用户词组=CTRL_DELETE
#拼音以词定字键，等号后面紧接键，不要有空格
拼音以词定字键=[]
#重码调整方式说明：0–>不调整 1–>快速调整 2–>按频率调整
拼音单字重码调整方式=2
拼音词组重码调整方式=1
拼音常用词重码调整方式=0
是否模糊an和ang=0
是否模糊en和eng=0
是否模糊ian和iang=0
是否模糊in和ing=0
是否模糊ou和u=0
是否模糊uan和uang=0
是否模糊c和ch=0
是否模糊f和h=0
是否模糊l和n=0
是否模糊s和sh=0
是否模糊z和zh=0
