---
layout: post
title: "Android分享文稿 ( by quqi99 )"
categories: mobile
tags: 
 - mobile
 - android
--- 

# Android分享文稿 ( by quqi99 )

您还未登录！|[登录](https://passport.csdn.net/account/login)|[注册](https://passport.csdn.net/account/register)|[帮助](https://passport.csdn.net/help/faq)

* [首页](http://www.csdn.net/)
* [业界](http://news.csdn.net/)
* [移动](http://mobile.csdn.net/)
* [云计算](http://cloud.csdn.net/)
* [研发](http://sd.csdn.net/)
* [论坛](http://bbs.csdn.net/)
* [博客](http://blog.csdn.net/)
* [下载](http://download.csdn.net/)
* 
## [更多]()

# [技术并艺术着](http://blog.csdn.net/quqi99)

## 张华的技术Blog

* [![]()目录视图](http://blog.csdn.net/quqi99?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/quqi99?viewmode=list)
* [![]()订阅](http://blog.csdn.net/quqi99/rss/list)
[公告：博客新增直接引用代码功能](https://code.csdn.net/blog/12)        [专访李铁军：从医生到金山首席安全专家的转变](http://www.csdn.net/article/2013-08-06/2816471)      [CSDN博客频道自定义域名、标签搜索功能上线啦！](http://blog.csdn.net/csdnproduct/article/details/9495139)      [独一无二的职位：开源社区经理](http://blog.csdn.net/adali/article/details/9813651)

### [[置顶] Android分享文稿 ( by quqi99 )]()

分类： [Android](http://blog.csdn.net/quqi99/article/category/351802)  2011-04-06 16:50 1272人阅读 [评论]()(4) [收藏]( "收藏") [举报]( "举报")
[android](http://blog.csdn.net/tag/details.html?tag=android)[eclipse](http://blog.csdn.net/tag/details.html?tag=eclipse)[linux内核](http://blog.csdn.net/tag/details.html?tag=linux%e5%86%85%e6%a0%b8)[layout](http://blog.csdn.net/tag/details.html?tag=layout)[linux](http://blog.csdn.net/tag/details.html?tag=linux)[google](http://blog.csdn.net/tag/details.html?tag=google)

目录[(?)]( "系统根据文章中H1到H6标签自动生成文章目录")[[+]]( "展开")

1. [作者张华  发表于2011-04-06 版权声明可以任意转载转载时请务必以超链接形式标明文章原始出处和作者信息及本版权声明]()
1. [httpblogcsdnnetquqi99]()
1. [目标]()
1. [掀起你的盖头来 --Android 是什么]()

1. [Android 模拟器]()
1. [关于 Android 的八卦]()
1. [Android 架构]()
1. [Android 优势]()

1. [工欲善其事必先利其器 -- 建立 Android 开发环境]()

1. [安装JDK]()
1. [安装Android SDK 及AVD Manager]()
1. [安装 Android eclipse plugin]()
1. [配置 Virtual devices Android Emulator]()

1. [千里之行始于足下 --Android 知识结构]()

1. [Android 界面开发]()

1. [Layout]()
1. [Wiget]()
1. [一些概念]()
1. [Activity 屏幕间的跳转]()
1. [使用 Droiddraw 开发界面]()

1. [Android 数据存储]()
1. [Android 多媒体开发]()
1. [Android 网络通信]()
1. [与 google 相关的特色开发]()

1. [大学教育的失败在于理论与实践脱节 --Android 实践]()

1. [应用工程组成]()
1. [两个 Activity 之间的切换]()
                                    **Android分享文稿 ( by quqi99 )**

## []()作者：张华  发表于：2011-04-06
版权声明：可以任意转载，转载时请务必以超链接形式标明文章原始出处和作者信息及本版权声明

## []()( http://blog.csdn.net/quqi99 )

**内容目录**

目标 1

1 掀起你的盖头来--Android是什么 1

1.1 Android模拟器 2

1.2 关于Android的八卦 2

1.3 Android架构 2

1.2 Android优势 5

2 工欲善其事，必先利其器--建立Android开发环境 5

2.1 安装JDK 5

2.2 安装Android SDK及AVD Manager 5

2.3 安装 Android eclipse plugin 5

2.4 配置Virtual devices (Android Emulator) 6

3 千里之行，始于足下--Android知识结构 8

3.1 Android界面开发 8

3.1.1 Layout 8

3.1.2 Wiget 8

3.1.3 一些概念 9

3.1.4 Activity屏幕间的跳转 10

3.1.4 使用Droiddraw开发界面 10

3.2 Android数据存储 10

3.3 Android多媒体开发 10

3.4 Android网络通信 10

3.5 与google相关的特色开发 10

4 大学教育的失败在于理论与实践脱节--Android实践 10

4.1 应用工程组成 10

4.2 两个Activity之间的切换 11

# []()目标

作为一个java 程序员，在阅读了本文之后，剩下的就仅是熟悉android 的API 。

# []()1   掀起你的盖头来 --Android 是什么

Android 是 Google 公司于 2007 年 11 月宣布的基于 linux 平台的开源手机操作系统，它包括了操作系统、中间件、用户界面及一些关键应用。

Android 是基于 Java （实际运行在 dalvik 虚拟机上）并运行在 linux 内核上的操作系统。

目前，基于 NDK ，也可以用 C ＋＋、 C 开发 Android 应用 ;  也可以借助一些软件在 Android 平台上跑例如 PHP 之类的脚本程序 ; Android 已经可以安装在上网本上。

## []()1.1 Android  模拟器

启动命令：  emulator -debug avd_config -avd avd2.2

![]()

## []()1.2   关于 Android 的八卦

开放手机联盟 :

谈到 Android ，有必要首先了解开放手机联盟（ Open Handset Alliance) ，它是 Google 公司于 2007 年 11 月宣布组建的一个全球性联盟组织，这一联盟会支持 Android 操作系统及其应用软件，包括手机制造商、手机芯片厂商、移动运营商，建立了整个移动系统的生态环境。

Android 开发大赛 :

Android 的未来关键取决于手机用户多次多彩的应用程序， Google 公司为了吸引更多的开发者参与到 Android 开发中来，于 2008 年 4 月 17 日举办了奖金为 1000 万美元的 Android 开发大赛，推动了 Android 开发的应用速度。我就是那时开始接触 Android 系统的。

Android Market ：

Android Market 被定位为开放的内容分享系统，也就是应用程序超市，任何程序员开发的应用程序都可以放在 Android Market 上，供所有的 Android 手机用户下载体验并赚钱。

Android 开发者社区：

Android 开发者社区是： http://www.android.com ， 不过由于伟大的长城防火墙的原因一般访问不了。不过你可以上局域网的社区： [http://www.eoeandroid.com](http://www.eoeandroid.com/)

## []()1.3 Android  架构

[http://blog.csdn.net/quqi99/archive/2007/12/04/1916553.aspx](http://blog.csdn.net/quqi99/archive/2007/12/04/1916553.aspx)

![]()

                                  图1-1 Android系统结构图

从图 1-1 可以看出 Android 分为 4 层，从高到低分别是应用层、应用框架层、系统运行库层和 Linux 内核层。下面将对这 4 层进行简要的分析和介绍。

**1.**  **应用层**

应用是用 java 语言编写的运行在虚拟机上的程序。 Google 同时也内置了一些核心应用，如 E-mail 客户端、 SMS 短消息程序、日历、地图、浏览器等等。

**2.**   **应用框架层**

这一层是开发应用程序可以使用的框架，这样便简化了程序开发的架构设计，但是必须遵守其框架的开发原则。从图 1-1 中可以看出， Android 提供了如下一些组件：

* 
丰富而又扩展的视图（ View) ：可以用来构建应用程序，如文本框（ TextBox ）等。
* 
内容提供器（ Content Providers ）：它可以让一个应用访问另一个应用的数据，或共享它们自己的数据。
* 
资源管理器（ Resource Manager ）：提供非代码资源的访问，如本地字符串、图形和布局文件（ Layout file ）。
* 
通知管理器（ Notification Manager) ：应用可以在状态栏中显示自定义的提示信息。
* 
活动管理器（ Activity Manager) ：用来管理应用程序生命周期并提供常用的导航退回功能。
* 
窗口管理器（ Window Manager ）：管理所有的窗口程序。
* 
包管理器（ Pakage Manager ）： Android 系统内的程序管理。

我们编程过程中，具体可供使用的包有：

* 
android.app  ：提供高层的程序模型和基本的运行环境。
* 
android.content  ：包含对各种设备上的数据进行访问和发布。
* 
android.database:  通过内容提供者浏览和操作数据库。
* 
android.graphics:  底层的图形库，包含画布、颜色过滤、点、矩形、可以将绘制在屏幕上。
* 
android.location:  定位和相关服务的类。
* 
android.media  ：提供一些类管理多种音频、视频的媒体接口。
* 
android.net:  提供帮助网络访问的类，超过通常的 java.net.* 接口。
* 
android.os:  提供了系统服务、消息传输和 IPC 机制。
* 
android.opengl:  提供 OpenGL 的工具。
* 
android.provider:  提供访问 Android 内容提供者的类。
* 
android.telephony:  提供与拨打电话相关的 API 交互。
* 
android.view:  提供基础的用户界面接口框架。
* 
android.util:  涉及工具的方法。
* 
android.webkit:  默认浏览器操作接口。
* 
android.widget:  包含各种 UI 元素，在应用程序的布局中使用。

**3.**   **系统运行库（**  **C/C++**  **库以及**  **Android**  **运行库）层**

当使用 Android 应用框架时， Android 系统会通过一些 C/C++ 库来支持我们使用的各个组件。

* 
Bionic  系统 C 库： C 语言标准库，最底层的库， C 库通过 Linux 系统来调用。
* 
多媒体库（ Media Framework ）：多媒体库，基于 PacketVideo OpenCORE, 该库支持多种常见格式的音频、视频的回放和录制，以及图片，比如 MPEG4 、 MP3 、 AAC 、 AMR 、 JPG 、 PNG 。
* 
SGL  ： 2D 图形引擎库
* 
SSL  ：位于 TCP/IP 协议与各种应用协议之间，为数据通信提供支持。
* 
OpenGL ES1.0  ： 3D 效果支持
* 
SQLite  ：关系型数据库
* 
Webkit  ： WEB 浏览器引擎
* 
FreeType  ：位图（ bitmap ）和矢量（ vector)

**4.Linux**   **内核层**

Android 的核心系统服务基于 Linux2.6 内核，如安全性、内存管理、进程管理、网络协议栈和驱动模型等都依赖于该内核。

Android 更多的是需要一些与移动设备相关的驱动程序，主要的驱动如下所示：

* 
显示驱动（ Display Driver ）：基于 Linux 的帧缓冲驱动。
* 
键盘驱动（ KeyBoard Driver ）：作为输入设备的键盘驱动。
* 
Flash  内存驱动（ Flash Memory Driver ）：基于 MTD 的 Flash 驱动程序。
* 
照相机驱动（ Camera Driver ）：常用的基于 Linux 的 v412 (Video for Linux) 驱动
* 
音频驱动（ Audio Driver ）：常用的基于 ALSA （ Advanced Linux Sound Architecture) 的高级 Linux 声音体系驱动。
* 
蓝牙驱动（ Bluetooth Driver ）：基于 IEEE 802.15.1 标准的无线传输技术。
* 
WiFi  驱动：基于 IEEE 802.11 标准的驱动程序。
* 
Binder IPC  驱动： Android 的一个特殊驱动程序，具有单独的设备节点，提供进程间通信的功能。
* 
电源管理（ Power Management ）：比如电池电量等。

## []()1.2 Android  优势

# []()2   工欲善其事，必先利其器 -- 建立 Android 开发环境

在 Mac 平台中建立 Android 开发环境，需安装下列软件：

* 
JDK
* 
Android SDK (Dalvik)
* 
Eclipse + Android eclipse plugin (ADT, Andorid Developer Tool)

## []()2.1   安装JDK

注意配置JAVA_HOME 环境变量，略。

## []()2.2  安装Android SDK 及AVD Manager

下载地址：[http://developer.android.com/sdk/index.html](http://developer.android.com/sdk/index.html)

ANDROID_SDK_HOME=/home/java/android-sdk-mac_86  （由于ADT 有bug 的原因，我们会修改这个环境变量为ANDROID_SDK_HOME=/var/root ）

PATH=$ANDROID_SDK_HOME/tools:$PATH

接着运行/home/java/android-sdk-mac_86/tools/android 命令在弹出的如下图的对话框中点击”Install packages” 菜单安装Android SDK  及AVD Manager

![]()

                                                 图2-1  安装Android SDK

## []()2.3   安装  Android eclipse plugin

Update url:  [https://dl-ssl.google.com/android/eclipse/](https://dl-ssl.google.com/android/eclipse/)

注意，此时一般会报错，这时候，再将 https 换成 http 即可。

 

![]()

                               图 2-2  安装 ADT (android eclipse plugin)

## []()2.4   配置 Virtual devices (Android Emulator)

点击 eclipse 的 window → Android SDK and AVD Manager 菜单，如下图所示：

在弹出的“ Android SDK and AVD Manager“ 对话框的” Virual devices” 面板点击 New 按钮，在弹出的“ Create new Android Virtual Device(AVD) 对话框架按照下图新建一个模拟器。

![]()

                                           图 2-3  新建模拟器 AVD

注意，此时如果在 eclipse 中点” Start“ 按钮启动模拟器的话，会报下列错：

emulator: ERROR: unknown virtual device name: 'Froyo'

emulator: could not find virtual device named 'Froyo'

图2-4  在eclipse 新建的AVD 在eclipse 中start 报的错

![]()

注意：上述错误是由 ADT 插件的 BUG 所致，虽然我们前面设置了环境变量 ANDROID_SDK_HOME=/home/java/android-sdk-mac_86 ， 但是 ADT 仍然傻乎乎地将 AVD 建在了 /var/root 目录下，所以在 eclipse 中启动 AVD 时会报上述错误 。

目前，只能通过下列方法曲线救国（我没有想出更好的解决办法）

1 ）重设环境变量 ANDROID_SDK_HOME=/var/root

2 ）在eclipse 中新建模拟器（因为要用eclipse 来debug 程序，所以因为路径的bug 我们最好不要在命令行中新建模拟器，而改在eclipse 中新建模拟器）

3 ）在命令行中启动模拟器（emulator -debug avd_config -avd avd2.2 ， 因为路径的bug 问题不要在eclipse 中启动模拟器）

4 ）剩下的调试和普通的eclipse 调试是一模一样的。

另外，注意：模拟器经常会出现error:device offline 的错误，这时候需要重启模拟器。

下面我们列举一些常用的模拟器操作的命令：

* 
列出模拟器类型：android list targets
* 
创建模拟器：android create avd –target 2 –name avd2.2
* 
列出自己创建的模拟器：android list avd
* 
切换模拟器样式：在创建命令后面加上”--skin QVGA” 即可。
* 
删除模拟器：android delete avd -name avd2.2
* 
安装avk 文件：adb install /tmp/myApp.apk
* 
缷载模拟器中的apk 文件，依次输入命令：adb shell, cd data, cd app, ls ，然后再用普通的linux 命令删除即可（注意：android 的底层是基于linux 内核的，文件系统是用linux 的）

# []()3   千里之行，始于足下 --Android 知识结构

想要快速掌握 Android 开发，需要学习一些什么呢？

## []()3.1 Android  界面开发

### []()3.1.1 Layout

类似于 SWT 编程。

* 
LinearLayout:   线性布局，一行（列）只能放一个控件。
* 
RelativeLayout  ：相对布局。控件的位置都是相对位置。
* 
TableLayout  ：表单布局。这要和 TableRow 配合使用，很像 HTML 里的 table
* 
TabWidget  ：切换卡，实现标签的切换功能。
* 
FrameLayout:  里面只能放一个控件，并且不能设计这个控件的位置，控件会放到左上角。
* 
AbsoluteLayout  ：可以自定义控件的 x,y 的位置。

### []()3.1.2 Wiget

类似于 SWT 编程，常用控件如下：

* 
文本框（ TextView)
* 
列表（ ListView)
* 
提示（ Toast)
* 
编辑框（ EditText)
* 
单项选择（ RadioGroup, RadioButton)
* 
多项选择（ CheckBox)
* 
下拉表列（ Spinner)
* 
自动提示（ AutoComplete-TextView)
* 
日期与时间（ DatePicker, TimePicker)
* 
按钮（ Button)
* 
菜单（ Menu)
* 
对话框（ Dialog)
* 
图片视图（ ImageView)
* 
带图标的按钮（ ImageButton)
* 
拖动效果（ Gallery)
* 
切换图片（ ImageSwitcher)
* 
网格视图（ GridView)
* 
卷轴视图（ ScrollView)
* 
进度条（ ProgressBar)
* 
拖动条（ SeekBar)
* 
状态栏提示（ Notification, NotificationManager)
* 
对话框中的进度条（ ProgressDialog)

### []()3.1.3   一些概念

Android 应用程序由 4 个模块组成： Activity, Intent, Content Provider, Service.

* 
Activity  ，你可以把它看作 JSP ，代表用户所能看到的活动（也就是屏幕啦），但是比 JSP 可能还多些带动作的东西，如监听系统事件（按键事件，触摸屏事件），也用户显示指定的 View, 启动其他的 Activity 等。
* 
Intent, Intent  用户 Activity 与 Activity 之间的切换，它包括两个重要的部分：动作和动作对应的数据。典型的动作类型有 MAIN,VIEW,PICK,EDIT 等，而动作对应的数据则以 URI 的形式表示。你可以把它看成是 JSP 页面之间的一次跳转或重定向。如要查看某人的联系方式，需要创建一个动作类型为 VIEW 的 Intent ，以及一个表示这个人的 URI 。有三种类型的 Intent:

1)  通过 Intent 切换屏幕，可以带数据

2  ）通过 Intent 来启动一个 Service

3  ）通过 Intent 来广播一个事件

* 
Intent filter,   如果说 Intent 是一个有效请求，一个 Intent Filter 则用于描述一个 Activity( 或者 Intent Receiver ）能够操作哪些 Intent 。例如，一个 Activity 要显示一个人的联系方式时，需要声明一个 Intent Filter ，这个 Intent Filter 要知道怎么去处理 VIEW 动作和表示一个人的 URI ，它一般在 AndroidManifest.xml 中定义。
* 
Broadcast intent Receiver  介绍，可以使用 BroadcaseReceiver 来让应用对一个外部的事件做出响应， BroadcaseReceiver 不能生成 UI 对用户是透明的。既可在 AndroidManifest.xml 中定义，也可使用 Content.registerReceiver() 进行注册。各种应用还可以通过使用 Content.sendBroadcast() 将它们自己的 intent broadcasts 广播给其它应用程序。
* 
Content Provider,  用于提供内容，如访问数据库。 值得一提的是，在 Android 当数据是应用私有的，而 Content Provider 就是来解决两个应用之间交换数据滴。一个 Content Provider 类实现了一组标准的方法接口（如 query, insert, update, delete) ，从而将自己的数据暴露出去。
* 
Service  ，是一个生命周期长且没有用户界面的程序。通过 startService(Intent intent) 可以启动一个 Service ，通过 Context.bindService() 可以绑定一个 Service 。

### []()3.1.4 Activity  屏幕间的跳转

Android 程序无外乎就是一个个屏幕的切换，通过解析各种 Intent ，从一个屏幕导航到另一个屏幕是很简单的。当向前导航时， Activity 将会调用 startActivity(Intent myIntent) 方法。然后，系统会在所有已安装的应用程度中定义的 IntentFilter 查找 ，找到最匹配 myIntent 的 Intent 对应的 Activity 。新的 Activity 接收到 myIntent 的通知后，开始运行。

这里假设有两个 Activity ，一个是 A, 一个是 B, 从屏幕跳到屏幕 B ，代码如下所示：

Intent intent = new Intent(A.this, B.class);

startActivity(intent);

### []()3.1.4   使用 Droiddraw 开发界面

可参见文档《 droiddraw + android + vi 可视化设计器》

安装步骤如下：

1 ）启动模拟器

android list avd

emulator -debug avd-config -avd avd2.2

2 ）下载 AnDroidDraw.apk 并安装它， adb install /tmp/AnDroidDraw.apk

3 ）安装端口转发规则： adb forward tcp:6100 tcp:7100

4 ）可点击 Generate 生成 XML 布局代码，也可点击“ Project—send GUI to device” 在模拟器上查看效果

## []()3.2 Android  数据存储

采用 SQLLite 嵌入式数据库，略。

## []()3.3 Android  多媒体开发

音乐、视频、相机、闹钟

## []()3.4 Android  网络通信

HTTP 通信可采用：

* 
java.net.*
* 
com.android.*

Socket 通信

Webkit 应用

WiFi 、蓝牙

## []()3.5   与 google 相关的特色开发

Google Map

# []()4   大学教育的失败在于理论与实践脱节 --Android 实践

## []()4.1   应用工程组成

                                         ![]()

* 
src,   源文件
* 
gen/R.java,   是 eclipse 自动生成的，是对资源文件的索引， res 目录发生变化，此处会自动更新
* 
assets,   放置多媒体
* 
res  目录，放资源文件， drawable 子目录放图片资源， layout 子目录放布局文件， values 子目录放置字符串（ string.xml ）、颜色（ colors.xml ）、数组（ arrays.xml ）
* 
AndroidManifest.xml,  应用的配置文件，如应用的名称、 Activity 、 Service 以及 reciever 等

## []()4.2   两个 Activity 之间的切换

![]()

                              图 新建 Andorid 工程

如上图所示，新建一个名为 AndroidTest 的 android project （注意：上述没给图的步骤直接按默认的下一步就行了）。

代码如下：

main.xml  布局文件是：

<?  xml     version  =  *"1.0"*      encoding  =  *"utf-8"*   ?>

<  LinearLayout    xmlns:android  =  *"http://schemas.android.com/apk/res/android"*

android:orientation  =  *"vertical"*

android:layout_width  =  *"fill_parent"*

android:layout_height  =  *"fill_parent"*

>

<  TextView

android:id  =  *"@+id/text"*

android:layout_width  =  *"fill_parent"*

android:layout_height  =  *"wrap_content"*

android:text  =  *"@string/hello"*

/>

<  Button    android:layout_height  =  *"wrap_content"*     android:layout_width  =  *"wrap_content"*     android:id  =  *"@+id/button"*     android:text  =  *"Button"*   ></  Button  >

</  LinearLayout  >

Androidmanifest.xml  文件要添加：

<  activity     android:name  =  *".Activity1"*   ></  activity  >

<  activity     android:name  =  *".Activity2"*   ></  activity  >

**package**    com.quqi;

**import**    android.app.Activity;

**import**    android.content.Intent;

**import**    android.os.Bundle;

**import**    android.view.View;

**import**    android.view.View.OnClickListener;

**import**    android.widget.Button;

**import**    android.widget.TextView;

/**

*  **@author**      huazhang

*  **@date**    20110316

*/

**public**      **class**    AndroidTest   **extends**    Activity

{

/** Called when the activity is first created. */

@Override

**public**      **void**    onCreate(Bundle savedInstanceState)

{

**super**   .onCreate(savedInstanceState);

setContentView(R.layout. *main*   );

TextView text = (TextView) findViewById(R.id. *text*   );

text.setText( "here is main activity"  );

Button button = (Button) findViewById(R.id. *button*   );

button.setText( "next"  );

OnClickListener listener =  **new**    OnClickListener() {

**public**      **void**    onClick(View v)

{

Intent intent2 =  **new**    Intent(AndroidTest.  **this**   , Activity2.  **class**   );

intent2.putExtra( "data"  ,   "zhanghua"  );

startActivity(intent2);

}

};

button.setOnClickListener(listener);

}

}

package com.quqi;

import android.app.Activity;

import android.content.Intent;

import android.os.Bundle;

import android.view.View;

import android.view.View.OnClickListener;

import android.widget.Button;

import android.widget.TextView;

/**

* @author huazhang

* @date 20110316

*/

public class Activity2 extends Activity

{

/** Called when the activity is first created. */

@Override

public void onCreate(Bundle savedInstanceState)

{

super.onCreate(savedInstanceState);

setContentView(R.layout.main);

String date = "";

Bundle extras = getIntent().getExtras();

if (extras != null)

{

date = extras.getString("data");

}

TextView text = (TextView) findViewById(R.id.text);

text.setText("here is activity2, " + date);

Button button = (Button) findViewById(R.id.button);

button.setText("return");

OnClickListener listener = new OnClickListener() {

public void onClick(View v)

{

Intent intent1 = new Intent(Activity2.this, AndroidTest.class);

startActivity(intent1);

}

};

button.setOnClickListener(listener);

}

}

 

再剩下的事情就应该只是熟悉API了，待续...

分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")
* 上一篇：[深入理解各JEE服务器Web层集群原理 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/6292472)
* 下一篇：[Shell编程注意点 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/6305253)
查看评论[]()

4楼 [zxlxiaoyi](http://blog.csdn.net/zxlxiaoyi) 2011-12-13 21:09发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/zxlxiaoyi)写的很详细。3楼 [MI_cool](http://blog.csdn.net/MI_cool) 2011-07-03 10:12发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/MI_cool)这个总结的真不错2楼 [mapengfei00099](http://blog.csdn.net/mapengfei00099) 2011-04-18 09:09发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/mapengfei00099)好好，请继续更新谢谢！！！1楼 [liuwenhan999](http://blog.csdn.net/liuwenhan999) 2011-04-14 14:09发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/liuwenhan999)唯一的感触是只剩下api了[e01]
您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fquqi99%2Farticle%2Fdetails%2F6305061)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()
[![TOP]()]( "回到顶部")

个人资料

[![]( "访问我的空间")](http://my.csdn.net/quqi99)
[quqi99](http://my.csdn.net/quqi99)

[]( "[加关注]") []( "[发私信]")
[![]()](http://medal.blog.csdn.net/allmedal.aspx)

* 访问：198660次
* 积分：3337分
* 排名：第1895名

* 原创：146篇
* 转载：23篇
* 译文：0篇
* 评论：123条

文章搜索

[]()

文章分类

* [VM / Cloud](http://blog.csdn.net/quqi99/article/category/875141)(7)
* [Middleware / Java AppServer](http://blog.csdn.net/quqi99/article/category/557281)(13)
* [Seach Engine](http://blog.csdn.net/quqi99/article/category/328029)(5)
* [Linux / Unix / Shell](http://blog.csdn.net/quqi99/article/category/674417)(24)
* [J2SE / JEE](http://blog.csdn.net/quqi99/article/category/328188)(40)
* [DB / NoSQL](http://blog.csdn.net/quqi99/article/category/347580)(9)
* [Architecture](http://blog.csdn.net/quqi99/article/category/803236)(0)
* [Android](http://blog.csdn.net/quqi99/article/category/351802)(3)
* [Life](http://blog.csdn.net/quqi99/article/category/803239)(4)
* [Other](http://blog.csdn.net/quqi99/article/category/689016)(9)
* [OpenStack](http://blog.csdn.net/quqi99/article/category/1112756)(37)
* [Python](http://blog.csdn.net/quqi99/article/category/1139084)(1)
* [C / C++](http://blog.csdn.net/quqi99/article/category/1167554)(2)
* [Networking](http://blog.csdn.net/quqi99/article/category/1490633)(1)
文章存档

* [2013年08月](http://blog.csdn.net/quqi99/article/month/2013/08)(4)
* [2013年07月](http://blog.csdn.net/quqi99/article/month/2013/07)(13)
* [2013年06月](http://blog.csdn.net/quqi99/article/month/2013/06)(6)
* [2013年05月](http://blog.csdn.net/quqi99/article/month/2013/05)(2)
* [2013年04月](http://blog.csdn.net/quqi99/article/month/2013/04)(2)
* [2013年03月](http://blog.csdn.net/quqi99/article/month/2013/03)(3)
* [2013年02月](http://blog.csdn.net/quqi99/article/month/2013/02)(1)
* [2013年01月](http://blog.csdn.net/quqi99/article/month/2013/01)(9)
* [2012年12月](http://blog.csdn.net/quqi99/article/month/2012/12)(3)
* [2012年11月](http://blog.csdn.net/quqi99/article/month/2012/11)(2)
* [2012年08月](http://blog.csdn.net/quqi99/article/month/2012/08)(1)
* [2012年07月](http://blog.csdn.net/quqi99/article/month/2012/07)(1)
* [2012年06月](http://blog.csdn.net/quqi99/article/month/2012/06)(7)
* [2012年05月](http://blog.csdn.net/quqi99/article/month/2012/05)(2)
* [2012年04月](http://blog.csdn.net/quqi99/article/month/2012/04)(6)
* [2012年03月](http://blog.csdn.net/quqi99/article/month/2012/03)(2)
* [2012年02月](http://blog.csdn.net/quqi99/article/month/2012/02)(1)
* [2011年12月](http://blog.csdn.net/quqi99/article/month/2011/12)(1)
* [2011年09月](http://blog.csdn.net/quqi99/article/month/2011/09)(5)
* [2011年08月](http://blog.csdn.net/quqi99/article/month/2011/08)(2)
* [2011年06月](http://blog.csdn.net/quqi99/article/month/2011/06)(2)
* [2011年04月](http://blog.csdn.net/quqi99/article/month/2011/04)(5)
* [2011年03月](http://blog.csdn.net/quqi99/article/month/2011/03)(2)
* [2011年01月](http://blog.csdn.net/quqi99/article/month/2011/01)(4)
* [2010年12月](http://blog.csdn.net/quqi99/article/month/2010/12)(1)
* [2010年07月](http://blog.csdn.net/quqi99/article/month/2010/07)(1)
* [2010年05月](http://blog.csdn.net/quqi99/article/month/2010/05)(1)
* [2010年04月](http://blog.csdn.net/quqi99/article/month/2010/04)(2)
* [2010年03月](http://blog.csdn.net/quqi99/article/month/2010/03)(2)
* [2010年02月](http://blog.csdn.net/quqi99/article/month/2010/02)(4)
* [2010年01月](http://blog.csdn.net/quqi99/article/month/2010/01)(6)
* [2009年12月](http://blog.csdn.net/quqi99/article/month/2009/12)(6)
* [2009年11月](http://blog.csdn.net/quqi99/article/month/2009/11)(5)
* [2009年10月](http://blog.csdn.net/quqi99/article/month/2009/10)(6)
* [2009年09月](http://blog.csdn.net/quqi99/article/month/2009/09)(1)
* [2009年08月](http://blog.csdn.net/quqi99/article/month/2009/08)(1)
* [2009年06月](http://blog.csdn.net/quqi99/article/month/2009/06)(4)
* [2009年03月](http://blog.csdn.net/quqi99/article/month/2009/03)(1)
* [2008年11月](http://blog.csdn.net/quqi99/article/month/2008/11)(1)
* [2008年10月](http://blog.csdn.net/quqi99/article/month/2008/10)(2)
* [2008年08月](http://blog.csdn.net/quqi99/article/month/2008/08)(1)
* [2008年06月](http://blog.csdn.net/quqi99/article/month/2008/06)(3)
* [2008年04月](http://blog.csdn.net/quqi99/article/month/2008/04)(1)
* [2008年03月](http://blog.csdn.net/quqi99/article/month/2008/03)(1)
* [2008年02月](http://blog.csdn.net/quqi99/article/month/2008/02)(1)
* [2008年01月](http://blog.csdn.net/quqi99/article/month/2008/01)(3)
* [2007年12月](http://blog.csdn.net/quqi99/article/month/2007/12)(4)
* [2007年11月](http://blog.csdn.net/quqi99/article/month/2007/11)(1)
* [2007年10月](http://blog.csdn.net/quqi99/article/month/2007/10)(4)
* [2007年08月](http://blog.csdn.net/quqi99/article/month/2007/08)(5)
* [2007年07月](http://blog.csdn.net/quqi99/article/month/2007/07)(6)
* [2007年06月](http://blog.csdn.net/quqi99/article/month/2007/06)(1)
* [2007年05月](http://blog.csdn.net/quqi99/article/month/2007/05)(8)

展开

阅读排行

* [Android学习笔记 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1916553 "Android学习笔记 ( by quqi99 )")(22132)
* [java.lang.NoClassDefFoundError: org/apache/commons/io/output/DeferredFileOutputStream](http://blog.csdn.net/quqi99/article/details/1680647 "java.lang.NoClassDefFoundError: org/apache/commons/io/output/DeferredFileOutputStream")(17851)
* [建立openstack quantum开发环境](http://blog.csdn.net/quqi99/article/details/7433285 "建立openstack quantum开发环境")(6747)
* [在linux中安装字库( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1755642 "在linux中安装字库( by quqi99 )")(5151)
* [Android手机客户端与Servlet交换数据(by quqi99)](http://blog.csdn.net/quqi99/article/details/1930304 "Android手机客户端与Servlet交换数据(by quqi99)")(5140)
* [ReentrantLock与synchronized的区别 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/5298017 "ReentrantLock与synchronized的区别 ( by quqi99 )")(4864)
* [个人户口档案转移笔记（适用北京集体户口）](http://blog.csdn.net/quqi99/article/details/5604732 "个人户口档案转移笔记（适用北京集体户口）")(4802)
* [Magnolia学习笔记（一个基于JSR170的内容管理系统） ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1624225 "Magnolia学习笔记（一个基于JSR170的内容管理系统） ( by quqi99 )")(4342)
* [JSpider学习笔记 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1624218 "JSpider学习笔记 ( by quqi99 )")(4149)
* [Plone学习笔记 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/3099945 "Plone学习笔记 ( by quqi99 )")(4057)
评论排行

* [Android学习笔记 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1916553 "Android学习笔记 ( by quqi99 )")(21)
* [java.lang.NoClassDefFoundError: org/apache/commons/io/output/DeferredFileOutputStream](http://blog.csdn.net/quqi99/article/details/1680647 "java.lang.NoClassDefFoundError: org/apache/commons/io/output/DeferredFileOutputStream")(18)
* [Android手机客户端与Servlet交换数据(by quqi99)](http://blog.csdn.net/quqi99/article/details/1930304 "Android手机客户端与Servlet交换数据(by quqi99)")(12)
* [个人户口档案转移笔记（适用北京集体户口）](http://blog.csdn.net/quqi99/article/details/5604732 "个人户口档案转移笔记（适用北京集体户口）")(8)
* [Magnolia学习笔记（一个基于JSR170的内容管理系统） ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1624225 "Magnolia学习笔记（一个基于JSR170的内容管理系统） ( by quqi99 )")(7)
* [使用itext生成word格式的报表(by quqi99)](http://blog.csdn.net/quqi99/article/details/2591768 "使用itext生成word格式的报表(by quqi99)")(5)
* [在linux中安装字库( by quqi99 )](http://blog.csdn.net/quqi99/article/details/1755642 "在linux中安装字库( by quqi99 )")(4)
* [Android分享文稿 ( by quqi99 )]( "Android分享文稿 ( by quqi99 )")(4)
* [OpenDaylight学习 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/9156497 "OpenDaylight学习 ( by quqi99 )")(3)
* [使用jacob生成word(by quqi99)](http://blog.csdn.net/quqi99/article/details/2590703 "使用jacob生成word(by quqi99)")(3)

推荐文章
最新评论

* [OpenDaylight学习 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/9156497#comments)

[quqi99](http://blog.csdn.net/quqi99): hi whoeversucks, 谢谢你的实时信息，非常有用，我已经更新到博客里了。另外，问个问题，...
* [OpenDaylight学习 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/9156497#comments)

[whoeversucks](http://blog.csdn.net/whoeversucks): 注意，OpenDayLight Controller和OSCP实际上2个独立的SDN控制器项目（分别...
* [给Linux虚机扩充硬盘空间 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/9102745#comments)

[quqi99](http://blog.csdn.net/quqi99): hi dalinhuang, 谢谢你的回复，你给的这个方法是只适合LVM场景的啊，我没有使用LVM。
* [给Linux虚机扩充硬盘空间 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/9102745#comments)

[dalinhuang](http://blog.csdn.net/dalinhuang): 给根（/）扩充的步骤：（以你的virtualbox并使用LVM为例）1. 新增一块虚拟硬盘，给虚机。...
* [OpenDaylight学习 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/9156497#comments)

[piaochenping](http://blog.csdn.net/piaochenping): 你好，为什么我安装时老是出现这个错误呢？ Failed to execute goal org.co...
* [OpenStack CI测试之devstack-gate ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/7659905#comments)

[quqi99](http://blog.csdn.net/quqi99): @sunyilong2012: 这种错误应该是差模块吧，可以单独安装一下试试, sudo pip i...
* [Fedora 16上源码建立pydev + eclipse的OpenStack开发环境笔记草稿 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/7411091#comments)

[quqi99](http://blog.csdn.net/quqi99): openstack因为用到了一些linux特有的东西，如iptables，所以目前只能跑在linux...
* [Fedora 16上源码建立pydev + eclipse的OpenStack开发环境笔记草稿 ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/7411091#comments)

[javaerss](http://blog.csdn.net/javaerss): 大神...看哭了，为此特地跑去下载fedora 16来做实验。之前用ubuntu下用eclipse ...
* [玩转play framework ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/6576375#comments)

[nanfu08](http://blog.csdn.net/nanfu08): 你能看得清，如果只是自己看的话我没话说，这样的文字叫人怎么读？？
* [OpenStack CI测试之devstack-gate ( by quqi99 )](http://blog.csdn.net/quqi99/article/details/7659905#comments)

[dragonsun](http://blog.csdn.net/sunyilong2012): 您好，我在做这个测试的时候遇到了无法导入statsd的问题，请问您有解决的方法吗？+ /home/j...

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)[QQ客服](http://wpa.qq.com/msgrd?v=3&uin=2355263776&site=qq&menu=yes) [微博客服](http://e.weibo.com/csdnsupport/profile) [论坛反馈](http://bbs.csdn.net/forums/Service) [联系邮箱：webmaster@csdn.net](mailto:webmaster@csdn.net) 服务热线：400-600-2320京 ICP 证 070598 号北京创新乐知信息技术有限公司 版权所有世纪乐知(北京)网络技术有限公司 提供技术支持江苏乐知网络技术有限公司 提供商务支持Copyright © 1999-2012, CSDN.NET, All Rights Reserved [![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
![](http://counter.csdn.net/pv.aspx?id=24)
