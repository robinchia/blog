---
layout: post
title: "Android 2.3 代码混淆proguard技术介绍"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - proguard
--- 

# Android 2.3 代码混淆proguard技术介绍

[Android 2.3 代码混淆proguard技术介绍](http://blog.csdn.net/zengyangtech/article/details/6127600)

由于各种反编译工具的泛滥，作为Android程序员在2.3版本以前只能通过手动添加proguard来实现代码混淆

 

proguard这个工具是一个java代码混淆的工具

 

在2.3版本的sdk中 我们可以看到在android-sdk-windows/tools/下面多了一个proguard文件夹

google已经把proguard技术放在了android sdk里面 可以通过正常的编译方式也能实现代码混淆了

 

![]()

可以看见新建一个工程里面有default.properties和proguard.cfg

 

默认的default.properties代码如下

**[c-sharp]** [view plain](http://blog.csdn.net/Zengyangtech/article/details/6127600# "view plain")[copy](http://blog.csdn.net/Zengyangtech/article/details/6127600# "copy")

1. # This file is automatically generated by Android Tools.  
1. # Do not modify this file -- YOUR CHANGES WILL BE ERASED!  
1. #  
1. # This file must be checked in Version Control Systems.  
1. #  
1. # To customize properties used by the Ant build system use,  
1. # "build.properties", and override values to adapt the script to your  
1. # project structure.  
1. # Project target.  
1. target=android-9  
 

我们可以看到proguard.cfg已经帮我们写好了优化代码脚本

**[c-sharp]** [view plain](http://blog.csdn.net/Zengyangtech/article/details/6127600# "view plain")[copy](http://blog.csdn.net/Zengyangtech/article/details/6127600# "copy")

1. -optimizationpasses 5  
1. -dontusemixedcaseclassnames  
1. -dontskipnonpubliclibraryclasses  
1. -dontpreverify  
1. -verbose  
1. -optimizations !code/simplification/arithmetic,!field/*,!class/merging/*  
1. -keep public class * extends android.app.Activity  
1. -keep public class * extends android.app.Application  
1. -keep public class * extends android.app.Service  
1. -keep public class * extends android.content.BroadcastReceiver  
1. -keep public class * extends android.content.ContentProvider  
1. -keep public class com.android.vending.licensing.ILicensingService  
1. -keepclasseswithmembernames class * {  
1.     native <methods>;  
1. }  
1. -keepclasseswithmembernames class * {  
1.     public <init>(android.content.Context, android.util.AttributeSet);  
1. }  
1. -keepclasseswithmembernames class * {  
1.     public <init>(android.content.Context, android.util.AttributeSet, int);  
1. }  
1. -keepclassmembers enum * {  
1.     public static **[] values();  
1.     public static ** valueOf(java.lang.String);  
1. }  
1. -keep class * implements android.os.Parcelable {  
1.   public static final android.os.Parcelable$Creator *;  
1. }  
 

从脚本中可以看到，混淆中保留了继承自Activity、Service、Application、BroadcastReceiver、ContentProvider等基本组件以及com.android.vending.licensing.ILicensingService

 

并保留了所有的Native变量名及类名，所有类中部分以设定了固定参数格式的构造函数，枚举等等。(详细信息请参考<proguard_path>/examples中的例子及注释。)

 

接下来 按照google帮助文档里说的

**[c-sharp]** [view plain](http://blog.csdn.net/Zengyangtech/article/details/6127600# "view plain")[copy](http://blog.csdn.net/Zengyangtech/article/details/6127600# "copy")

1. To enable ProGuard so that it runs as part of an Ant or Eclipse build, set the proguard.config property in the <project_root>/default.properties file. The path can be an absolute path or a path relative to the project's root.  
 

所以我们修改default.properties file

加上一句

proguard.config=proguard.cfg

如下

**[c-sharp]** [view plain](http://blog.csdn.net/Zengyangtech/article/details/6127600# "view plain")[copy](http://blog.csdn.net/Zengyangtech/article/details/6127600# "copy")

1. # This file is automatically generated by Android Tools.  
1. # Do not modify this file -- YOUR CHANGES WILL BE ERASED!  
1. #  
1. # This file must be checked in Version Control Systems.  
1. #  
1. # To customize properties used by the Ant build system use,  
1. # "build.properties", and override values to adapt the script to your  
1. # project structure.  
1. # Project target.  
1. target=android-9  
1. proguard.config=proguard.cfg  
 

 

然后正常的编译签名即可

 

然后用Android Tools生成一个发布的apk即可

![]()

 

然后用反编译工具查看dex文件

最后导出反编译之后的混淆代码如下图

 

![]()

 

 

是不是很轻松加愉快!希望各位程序员都能保护好自己的Android代码!
来源： <[http://blog.csdn.net/Zengyangtech/article/details/6127600](http://blog.csdn.net/Zengyangtech/article/details/6127600)> 
