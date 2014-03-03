---
layout: post
title: "VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程"
categories: mac
tags: 
 - mac
--- 

# VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程

之前论坛里的[**莱茵哈特**](http://my.zol.com.cn/mybbs/raychaw/)网友发过一帖：[**VMware虚拟机安装MAC OS X Mountain Lion详细图文教程**](http://diybbs.zol.com.cn/1/34037_629.html)，这个帖子发的时候还是VMware8，现在的最新版已经更新到了9.01版本，而且Mountain Lion也更新到了10.8.2版本，加上看到帖子后面很多网友安装失败，所以写一个虚拟机安装MAC OS的升级版教程，同时也更新了所用的系统和软件，以及修正了部分错误并简化了安装步骤。
**VMware虚拟机安装Mac OS X Mountain Lion 10.8.2所需文件：**
1、Vmware 9.01版下载：[**点击进入**](http://diybbs.zol.com.cn/15/225_142169.html)
2、Vmware 9.01版汉化文件：[**点击进入**](http://diybbs.zol.com.cn/15/225_142170.html)
3、VMware Workstation 破解安装mac os补丁：[**点击进入**](http://diybbs.zol.com.cn/1/34037_700.html)
4、Mac OS X Mountain Lion 10.8.2下载：[**点击进入**](http://diybbs.zol.com.cn/1/34037_701.html)
5、虚拟机VMTOOLS darwin：[**点击进入**](http://diybbs.zol.com.cn/1/34037_702.html)（非必须，官方完整版虚拟机已包含）
**具体步骤：**
1、安装VMware虚拟机，安装后，复制汉化包中的文件到虚拟机安装目录中覆盖同名文件，如果提示无法覆盖，在任务管理器中结束vm开头的几个进程后重试覆盖
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片1]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415508)
2、下载并解压vm mac OS补丁，在windows文件夹中右键点击【install.cmd】文件，选择window7管理员运行，完成后重启一下电脑
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片2]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415315)
3、新建虚拟机
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片3]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415316)
4、选择自定义（高级），下一步
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片4]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415317)
5、直接下一步
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片5]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415318)
6、选我以后再安装操作系统，然后下一步
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片6]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415319)
7、选中APPLE MAC OS X，然后选择10.8 64bit
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片7]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415320)
8、设置一个虚拟机名字，名字可以随便起，然后设置一个存放虚拟机文件的目录，记住放到一个新的文件夹中，不要乱放，安装后会释放很多文件
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片8]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415321)
9、设置CPU的数量
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片9]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415322)
10、设置内存，别设置的太小
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片10]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415323)
11、ADSL猫直接连的选择NAT，局域网选择桥接
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片11]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415324)
12、下面几步直接选择继续
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片12]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415325)
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片13]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415326)
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片14]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415327)
13、选择单个文件存储虚拟磁盘，尽量别低于40G，然后记得选择下面的：单个文件存储虚拟磁盘
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片15]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415328)
14、直接选择继续
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片16]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415329)
15、点编完成，现在虚拟机就算创建完成了，下面即使一些对这个虚拟机的设置以及装系统了
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片17]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415330)
16、点编辑虚拟机设置
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片18]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415505)
17、点击硬盘---高级----设置为SCSI 0:8
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片19]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415332)
18、删除软驱
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片20]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415333)
19、勾选显示器中的3D图形加速
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片21]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415334)
20、点击CD/DVD，选择使用ISO映像文件，点击右侧的浏览
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片22]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415335)
21、上面提供的是cdr格式的懒人版安装包，所以默认是看不到的，选择所有文件就可以看到了，选中，然后确定关闭虚拟机编辑
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片23]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415506)
22、点击打开虚拟机电源，就可以开始苹果系统的安装了
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片24]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415336)
23、选择简体中文
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片25]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415337)
24、在这一步的时候不要着急点继续，点击上面的实用工具，选择磁盘工具
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片26]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415339)
25、选中磁盘，在分区布局处可以选择分几个分区，我一般都是分一个，白苹果也是就一个分区
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片27]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415340)
26、选择好分区后，在右边的名称输入名字，如果是分多个区的话，设置一下下面的大小，然后但应用创建分区
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片28]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415341)
27、在弹出的对话框中选择分区
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片29]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415342)
28、刚才分完区后，左侧会出现你刚才设置的名字的一个分区，点中，，右边的标签选择抹掉，然后右下角选择抹掉，抹掉后点击左上角的红色按钮关闭窗口
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片30]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415343)
29、点击继续
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片31]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415344)
30、点击我同意
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片32]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415345)
31、点击一个刚才分的区，出现绿色向下箭头后，点击安装
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片33]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415346)
32、安装中，我这显示需要21分钟左右
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片34]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415347)
33、安装成功后，会提示重启，10秒钟后会自动重启
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片35]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415348)
34、选中中国，继续
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片36]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415349)
35、直接继续
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片37]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415350)
36、点击以后
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片38]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415351)
37、直接点继续
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片39]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415352)
38、点击不使用
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片40]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415353)
39、如果有Apple ID输入，没有跳过
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片41]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415354)
40、点继续
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片42]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415355)
41、点同意
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片43]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415356)
42、输入帐号、密码等点继续
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片44]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415357)
43、选择中国的城市，然后继续
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片45]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415358)
44、可以注册，我都是跳过
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片46]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415359)
45、现在MAC OS的苹果系统就算是安装完了，点击【开始使用Mac】
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片47]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415360)
47、安装完成，进入桌面
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程图片48]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415361)

 
来源： <[【VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程】-MAC OS论坛-ZOL中关村在线](http://diybbs.zol.com.cn/1/34037_699.html)> 

 
安装VMware Tools Darwin：
VMware Tools是VMware虚拟机中自带的一种增强工具，相当于VirtualBox中的增强功能（Sun VirtualBox Guest Additions），是VMware提供的增强虚拟显卡和硬盘性能、以及同步虚拟机与主机时钟的驱动程序。只有在VMware虚拟机中安装好了VMware Tools，才能实现主机与虚拟机之间的文件共享，同时可支持自由拖拽的功能，鼠标也可在虚拟机与主机之前自由移动（不用再按ctrl+alt），且虚拟机屏幕也可实现全屏化。
1、点击虚拟机-设置
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程回复图片50970]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415583)
2、选择CD/DVD，点击右侧的浏览，然后选择最开始提供的VMware Tools Darwin的ISO文件【下载后需解压】
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程回复图片50971]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415584)
3、桌面右上角会出现一个VMware Tools的图标，双击这个图标
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程回复图片50972]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415585)
4、在弹出的安装窗口中点击安装VMware Tools
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程回复图片50973]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415586)
5、点击继续
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程回复图片50974]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415587)
6、继续
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程回复图片50975]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415588)
8、选择一个安装的分区，然后点继续
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程回复图片50976]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415591)
9、点安装
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程回复图片50977]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415595)
10、输入系统密码
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程回复图片50978]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415596)
11、点击继续安装
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程回复图片50979]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415597)
12、安装完成后，点击重新启动
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程回复图片509710]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415598)
13、重启后，鼠标就可以在实机和虚拟机中自由的移动，分辨率也可以设置了：
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程回复图片509711]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415599)
来源： <[【VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程】-MAC OS论坛-ZOL中关村在线](http://diybbs.zol.com.cn/1/34037_699.html)> 安装错误的解决方法【随时更新】
1、如果提示下图，点击重新启动，重新安装即可
[![VMware9虚拟机安装MAC OS X Mountain Lion 10.8.2详细图文教程回复图片50980]( "点击查看大图")](http://diybbs.zol.com.cn/tips/show_pic.php?picid=7415615)
