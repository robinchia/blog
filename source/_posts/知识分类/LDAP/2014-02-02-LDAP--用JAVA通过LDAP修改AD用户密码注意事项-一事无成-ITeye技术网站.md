---
layout: post
title: "用JAVA通过LDAP修改AD用户密码注意事项 - 一事无成 - ITeye技术网站"
categories: LDAP
tags: 
 - LDAP
--- 

# 用JAVA通过LDAP修改AD用户密码注意事项 - 一事无成 - ITeye技术网站

[首页](http://www.iteye.com/) [新闻](http://www.iteye.com/news) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [招聘](http://www.iteye.com/job) [更多 ▼](http://wls981.iteye.com/blog/316012#)

[专栏](http://www.iteye.com/wiki)  [群组](http://www.iteye.com/groups) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://wls981.iteye.com/login "登录") [我的应用](http://www.iteye.com/all) [登录](http://wls981.iteye.com/login) [注册](http://wls981.iteye.com/signup)

# [一事无成](http://wls981.iteye.com/)

永久域名 [http://wls981.iteye.com](http://wls981.iteye.com/)

[实模式->保护模式->实模式 切换的注意事项](http://wls981.iteye.com/blog/443237 "实模式->保护模式->实模式 切换的注意事项") | [Installanywhere 2008 VP1 Enterprise 破解 ...](http://wls981.iteye.com/blog/256955 "Installanywhere 2008 VP1 Enterprise 破解 分享")

2009-01-14

### [用JAVA通过LDAP修改AD用户密码注意事项]()

**文章分类:[Java编程](http://www.iteye.com/blogs/category/java)**
    最近要用java来修改windows 2003的Active Directory(简称AD)上的用户，包括新增、修改、删除，普通的操作这里就不说了，网上有一大堆的资料，这里记述一下本人操作过程中遇到的问题及解决方法。
    通过ldap来修改AD的用户信息，除了修改密码外，其他的都可以使用非安全的连接进行操作，也就是可以不走SSL连接来操作，注意AD的普通端口是389，SSL端口是636。
    当使用SSL连接修改密码时，需要在连接端安装证书，怎么获取证书，本人使用网上其他人介绍的证书方法，全是无法成功，都是出现 unable to find valid certification path to requested target  错误，最终经一朋友提示，使用其他方法获取到正确的证书，具体如下：
    1、安装证书服务。在ldap服务器上安装证书服务。证书服务的安装没什么特别的注意，请参考其他人的文章。
   
    2、获取客户端证书。别人都是通过下载证书的方式来获取证书，但是我通过这种方式就是无法成功修改密码，也都是提示 unable to find valid certification path to requested target 这个错误。我的操作方法为，用IE通过SSL直接连接ldap服务器(也就是安装AD的那台机器)，使用636端口，类似于  https://192.168.0.111:636  ，连接的时候会提示安装证书，这时候把这个证书保存下来，即为需要的客户端证书。
    3、连接。得到证书后，我在连接的时候成功了，但在修改AD用户密码的时候还是报错，但这次的错误为  javax.naming.OperationNotSupportedException: [LDAP: error code 53 - 0000052D: SvcErr: DSID-031A0FC0, problem 5003 (WILL_NOT_PERFORM), data 0    ,操作不支持错误，在这个问题了转了好久都没解决，后来无意中看到一篇文章讲到AD中的密码策略问题，才想起来windows2003中AD中的默认密码策略有长度限制和复杂性要求，所以才导致出现OperationNotSupportedException异常，记得在域控制器上查看自己的域策略。
后记：由于水平原因，就因为证书的问题搞了两天多才搞定，其实通过调整域策略可以使用修改密码不需要走SSL通道的，但这都是旁门左道，希望写下这篇文章对大家有所帮助。

* [ldap.rar](http://dl.iteye.com/topics/download/ad8784f9-0680-309e-b0ba-dba4f94c6611) (6 KB)
* 下载次数: 73
[实模式->保护模式->实模式 切换的注意事项](http://wls981.iteye.com/blog/443237 "实模式->保护模式->实模式 切换的注意事项") | [Installanywhere 2008 VP1 Enterprise 破解 ...](http://wls981.iteye.com/blog/256955 "Installanywhere 2008 VP1 Enterprise 破解 分享")

* 10:03
* 浏览 (4409)
* [评论](http://wls981.iteye.com/blog/316012#comments) (6)
* 分类: [java](http://wls981.iteye.com/category/53640)
* [相关推荐](http://www.iteye.com/wiki/topic/316012)
### 评论

[]()

6 楼 [wls981](http://wls981.iteye.com/) 2009-09-02   [引用](http://wls981.iteye.com/blog/316012#)

其实只要是AD中提供的对象，你都是可以修改的，group如果是AD提供的对象，那肯定也是可以修改的，修改方法应该跟用户类似。
5 楼 [Leecupn](http://leecupn.iteye.com/) 2009-08-21   [引用](http://wls981.iteye.com/blog/316012#)

to wls981:
谢谢，问题我前几天已经解决了，确实是证书没导入。
请问一下，除了对用户编辑外，我可以对用户组，也就是group进行增、删操作吗？这是一个比较严峻的问题啊
期待回复。

4 楼 [wls981](http://wls981.iteye.com/) 2009-08-19   [引用](http://wls981.iteye.com/blog/316012#)

to Leecupn:
你证书正确的导入到JDK里了吗？连接的时候相应的证书路径是否指定正确呢？
3 楼 [Leecupn](http://leecupn.iteye.com/) 2009-08-17   [引用](http://wls981.iteye.com/blog/316012#)

楼主，我获取证书并安装完成了之后，为什么还是报：
javax.naming.CommunicationException: simple bind failed: 192.168.136.202:636 [Root exception is javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException:PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target]
异常呢？？
怎样才能与好好你交流下？
期待回复，谢谢！

2 楼 [wls981](http://wls981.iteye.com/) 2009-08-09   [引用](http://wls981.iteye.com/blog/316012#)

具体的操作方法网上有很多代码你可以找找看，只要注意证书正确即可。
1 楼 [zhangfeiii](http://zhangfeiii.iteye.com/) 2009-05-08   [引用](http://wls981.iteye.com/blog/316012#)

我們公司B/S架構的系統，目前需要直接在界面上操作AD用戶信息(比如用戶帳號停用 / 用戶密碼Update等)，請問能否詳細敘述下技術層面，謝謝！

### 发表评论

### 表情图标

![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()

字体颜色: 标准深红红色橙色棕色黄色绿色橄榄青色蓝色深蓝靛蓝紫色灰色白色黑色 字体大小: 标准1 (xx-small)2 (x-small)3 (small)4 (medium)5 (large)6 (x-large)7 (xx-large) 对齐: 标准居左居中居右

提示：选择您需要装饰的文字, 按上列按钮即可添加上相应的标签

您还没有登录，请[登录](http://wls981.iteye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![wls981的博客]( "wls981的博客: 一事无成")](http://wls981.iteye.com/)

wls981

* 浏览: 7786 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 桂林
* ![]()
* [详细资料](http://wls981.iteye.com/blog/profile) [留言簿](http://wls981.iteye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://wls981.iteye.com/blog/user_visits)

[![lrg30067的博客]( "lrg30067的博客: lrg30067")](http://lrg30067.iteye.com/)

[lrg30067](http://lrg30067.iteye.com/)

[![lhlinux的博客]( "lhlinux的博客: lhlinux")](http://lhlinux.iteye.com/)

[lhlinux](http://lhlinux.iteye.com/)
[![爱问知识人的博客]( "爱问知识人的博客: ")](http://wangyiywc-163-com.iteye.com/)

[爱问知识人](http://wangyiywc-163-com.iteye.com/)

### 博客分类

* [全部博客 (10)](http://wls981.iteye.com/)
* [java (4)](http://wls981.iteye.com/category/53640)
* [汇编 (4)](http://wls981.iteye.com/category/75275)
* [oracle (1)](http://wls981.iteye.com/category/152148)
### 我的留言簿 [>>更多留言](http://wls981.iteye.com/blog/guest_book)

* http://dl.iteye.com/topics/download/ad878 ...
-- by [wls981](http://wls981.iteye.com/blog/guest_book#30502)
* 好厉害 【用JAVA通过LDAP修改AD用户密码注意事项一】 java来修改wi ...
-- by [小刀飞李](http://wls981.iteye.com/blog/guest_book#29097)

### 其他分类

* [我的收藏](http://wls981.iteye.com/blog/favorite) (0)
* [我的代码](http://wls981.iteye.com/blog/code_favorite) (0)
* [我的论坛主题帖](http://wls981.iteye.com/blog/topic) (4)
* [我的所有论坛帖](http://wls981.iteye.com/blog/post) (14)
* [我的精华良好帖](http://wls981.iteye.com/blog/article) (0)
### 最近加入群组

### 存档

* [2011-04](http://wls981.iteye.com/blog/monthblog/2011-04) (1)
* [2011-03](http://wls981.iteye.com/blog/monthblog/2011-03) (2)
* [2010-10](http://wls981.iteye.com/blog/monthblog/2010-10) (1)
* [更多存档...](http://wls981.iteye.com/blog/monthblog_more)
### 评论排行榜

* [Ext.data.ScriptTagProxy在ie6中报参数无 ...](http://wls981.iteye.com/blog/981072 "Ext.data.ScriptTagProxy在ie6中报参数无效")
* [weblogic 10在windows下换64位JDK后的本 ...](http://wls981.iteye.com/blog/981088 "weblogic 10在windows下换64位JDK后的本地IO库")
* [oracle stream 目标库上的触发器不执行](http://wls981.iteye.com/blog/998028 "oracle stream 目标库上的触发器不执行")
* [spring的aop中切入点的定义注意](http://wls981.iteye.com/blog/790233 "spring的aop中切入点的定义注意")

* [![Rss]()](http://wls981.iteye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://wls981.iteye.com/rss)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 ]
![]()
