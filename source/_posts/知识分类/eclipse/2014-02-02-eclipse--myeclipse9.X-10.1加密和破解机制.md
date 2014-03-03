---
layout: post
title: "myeclipse9.X-10.1加密和破解机制"
categories: eclipse
tags: 
 - eclipse
--- 

# myeclipse9.X-10.1加密和破解机制

不想了解破解机制急于破解的直接跳到最后 ‘ 具体操作 ’

myeclipse 9.1 终于出来了，有人尝鲜了，但是发现很受伤，很受伤是因为完整性验证部分，呵呵。

 

![图片]()

 

 

myeclipse 9.0 完整性校验有参数可以控制.

-Dgenuitec.honorDevMode=true

-Dosgi.dev=true

 

但是9.1取消了这个选项。上面的命令就不好用了。反编译源代码即可看差异，确实删掉了开发模式的代码。

要想跳过校验，有两种选择。

看堆栈（使用 jconsole，Java内置），可以看到

...

com.genuitec.eclipse.core.SignatureVerifier.verifyJarSignatures(SignatureVerifier.java:172)

com.genuitec.eclipse.core.CommonCore.startup(CommonCore.java:39)

...

 

1.短路 com.genuitec.eclipse.core.CommonCore中的startup()方法

2.短路 com.genuitec.eclipse.core.SignatureVerifier中的verifyJarSignatures()方法

 

仔细看反编译的源代码即可发现 短路CommonCore中的startup()方法比较麻烦，因为需要别的类包中的库，找起来比较麻烦。

所以我选择短路SignatureVerifier中的verifyJarSignatures()方法

下面的类即可，什么也不需要，这个方法什么也不做，直接替换就行，用压缩软件打开，替换相应的类即可

注意：最好编译该类的时候使用JDK1.5。高版本也可以，移植的时候可能要考虑JDK的问题。因为1.5运行不了1.6的类文件

相应的包是 Common/plug/com.genuitec.eclipse.core.common_9.0.0.me2011*.jar，包具体视情况而定.

 

 
Java代码 

1. package com.genuitec.eclipse.core;  
1.   
1. /** 
1.  * replace com.genuitec.eclipse.core.SignatureVerifier With this class<br> 
1.  * in jar com.genuitec.eclipse.core.common_9.0.0.me2011*.jar<br> 
1.  * shorcut method verifyJarSignatures()<br> 
1.  *  
1.  * @author macbookpro 
1.  */  
1. public class SignatureVerifier {  
1.     public void verifyJarSignatures() {  
1.     // do nothing ...  
1.     }  
1. }  

 
类可以自己编译，也可以用已上传的替换即可。

关于，进一步的用户licence和activecode破解，那个用9.0的破解方法即可，这部分都是一样的。
一个基于Java环境的破解程序。

***************************************************************
有人用了以上的方法，没有破解成功，之后还有校验完整性错误的弹出框，这个是因为myeclipse的检测机制我之前没有弄清楚。

myeclipse检测完整性分为两个步骤

1. myeclipse 启动校验

2. myeclipse 组件校验。

 

上面写的只是myeclipse的启动的短路，其实还有组件的检测。

 

组件校验

当myeclipse启动成功，会首先加载eclipse的核心，然后挂载myeclipse组件。

当挂载myeclipse组件的时候，就会有完整性校验了。这个就是组件校验，9.0不知道是否存在这种校验。

 

找到那个组件校验的类包有两种方法（目前能想到的比较实际的）

1. 查看堆栈。（jconsole和jstack）

   在弹出校验错误的窗口时，看堆栈，找出那个类进行的校验，然后找到该包，短路掉里面的校验逻辑。（找包的部分比较麻烦）

   然后再打开myeclipse，再找出哪个类进行的校验………. 直到没有弹出框为止，证明都找到了。

2. jar文件遍历

   编写程序，找出jar文件里的校验类。

   程序很好写，就是找出plugin目录和子目录下的jar文件里的类名是否包括SignatureVerifier类即可。

 

下面的类包和里面的类就是我通过上面两种方法共同找到的类。

Common/plugins/com.genuitec.eclipse.core.common_9.0.0.me201105301859.jar                      [com/genuitec/eclipse/core/SignatureVerifier.class]

Common/plugins/com.genuitec.eclipse.easie.core_9.0.0.me201106010603/easiecore.jar           [com/genuitec/eclipse/easie/core/SignatureVerifier.class]

Common/plugins/com.genuitec.eclipse.j2eedt.core_9.0.0.me201106292137/j2eedtcore.jar         [com/genuitec/eclipse/j2eedt/core/SignatureVerifier.class]

Common/plugins/com.genuitec.myeclipse.product_9.0.0.me201106290046/myeclipse-product.jar      [com/genuitec/myeclipse/product/SignatureVerifier.class]

 

我们需要把上面的类包中的SignatureVerifier短路掉即可，用上面SignatureVerifier类重新编译即可，注意包名。

替换的类我编写好了一份，要是自己编译麻烦的话，那就直接用附件里面的。

 

这几个类包短路掉之后，我用windows测试过没有问题。没有校验的弹出框了，基本可以解决校验问题。

替换类的方法我感觉还是比较麻烦，谁让我喜欢尝鲜了，尝鲜的朋友都知道，那就是必须的折腾。

myeclipse 9.1 破解说明

 

myeclipse 9.X 系列需要两个步骤：

第一步是破解Licence（所有系列都有）

第二步破解激活码activecode，（9.X系列特有）

 

Licence的算号，网上都有，这个基本没有什么问题，不需要借助其他的库，直接算法就可以。

 

关键是激活码的计算，这个比较麻烦，网上有关于9.0的破解程序，但是是exe文件的。

本人使用macbook ,要想破解起来必须装虚拟机或者bootcamp，所以对我来说比较麻烦，还有一个问题就是windows破解程序在计算activecode的时候需要systemid，但是替换成macbook的systemid之后再算的时候可能有问题。（可能有些人没这个问题）

所以自己看了myeclipse的验证逻辑，自己写了个破解。完全基于Java,任何系统都可以使用。

 

下面讲解一下myeclipse 9.X系列的注册验证问题

 

1. systemid的计算

systemid计算需要借助于其他类库

jniwrapper 关于这个可以到官方网站下载相应操作系统的类库。

 

systemid组成

“1f553430D3107d17834”

 

TypeField:1

Field:    15(f)

HostInfo:55 

SystemInfo:3430D31

MacAddress:07d1

HDSerial:7834

 

licenceCode由Licence算号算出。

 

Licence到期时间，可以由licenceCode反向转换得出，也可以自定义，只要在反向转换的时间之后就可以。

 

RSA加密解密

大家都知道myeclipse中有个publicKey.bytes文件，这个是公匙。

密匙（privateKey.bytes）在myeclipse公司手中，我们是拿不到的。

所以我们需要自己生成一个公匙和密匙，然后替换掉myeclipse中的公匙，这个就是9.0的破解过程，大家都清楚吧。

通过密匙加密，然后用公匙解出。这个也是RSA加密解密的机制。

 

激活码 是由上述三个字符串累加而成，然后通过RSA加密而成。

 

 

激活码注册验证需要通过三层验证

第一层

你输入的激活码解密出activecode（通过publicKey.bytes解出）

第二层

activecode分离出systemId,licenceCode,licenceDate

第三层

比对systemId,licenceCode,licenceDate三个参数，看是否匹配系统中的参数，即可完成验证。

9.0 的注册机在windows下很好用，但是在其他系统上却有些问题。手动填写systemid计算后的破解码有些问题。

有人使用MacOSX，所以必须借助于虚拟机或者其他的机器上来破解。在网上也找了很多，有一种方法就是使用9.0的注册机，替换掉里面生成systemid字符串（自己操作系统上的systemid，在myeclipse激活窗口能看到），然后运行那个exe文件，就可以破解。这个有些麻烦，但是也能解决问题。

有些人可能就是自己一台机器，借助不了其他的环境，那破解起来就会比较麻烦。

 9.1的破解

我们这里，使用java，可以跨平台使用，启动文件为jar文件，如果装了JDK双击即可，要是在命令行就用java -jar *.jar（当然要配置好java环境变量，我相信在这里费劲注册激活myeclipse的人java环境变量配置都是入门知识的了吧）

**具体操作**

 

![图片]()

 

第一步：Usrcode中输入任意用户名

第二步：点击systemid一次，这时候如果出现一行错误

Cannot find JNIWrapper native library (libjniwrap.so) in java.library.path:~~

不需要理会，再点击一次即可出现systemid。

第三步： 点菜单Tools->RebuildKey

第四步：点击active按钮.会在显示区域生成

LICENSE_KEY

ACTIVATION_CODE

ACTIVATION_KEY

这时候你并不需要打开myeclipse到激活页面输入。切记。next

第五步：打开菜单Tools->ReplaceJarFile，弹出文件选择对话框，到myeclipse的安装目录common文件夹下选择plugins文件夹

    点击打开，程序会卡住，不要担心，正在替换文件呢！

一会之后，会输出信息，文件已被替换

第六步：点菜单Tools->SaveProperites

OK 。打开你的myeclipse已经不需要再输入激活码什么的了。

仅供研究myeclipse9.X~~10.1加密和破解机制学习使用

我放在MSN网盘上共享，下载地址，  不需注册，刚添加了一下10.1破解的文件和源码
[https://skydrive.live.com/redir.aspx?cid=3a056e7a1777dab0&resid=3A056E7A1777DAB0!105](https://skydrive.live.com/redir.aspx?cid=3a056e7a1777dab0&resid=3A056E7A1777DAB0!105)

来源： <[http://b1.cnc.qzone.qq.com/cgi-bin/blognew/blog_output_data?uin=894528596&blogid=1317127373&styledm=cnc.qzonestyle.gtimg.cn&imgdm=cnc.qzs.qq.com&bdm=b.cnc.qzone.qq.com&mode=2&numperpage=15&blogseed=0.7196430717594922&property=GoRE×tamp=1372298123&dprefix=cnc.&g_tk=5381&ref=qzone&entertime=1372298137865](http://b1.cnc.qzone.qq.com/cgi-bin/blognew/blog_output_data?uin=894528596&blogid=1317127373&styledm=cnc.qzonestyle.gtimg.cn&imgdm=cnc.qzs.qq.com&bdm=b.cnc.qzone.qq.com&mode=2&numperpage=15&blogseed=0.7196430717594922&property=GoRE&timestamp=1372298123&dprefix=cnc.&g_tk=5381&ref=qzone&entertime=1372298137865)>
 
