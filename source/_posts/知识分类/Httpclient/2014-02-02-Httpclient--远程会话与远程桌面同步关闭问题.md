---
layout: post
title: "远程会话与远程桌面同步关闭问题"
categories: Httpclient
tags: 
 - Httpclient
--- 

# 远程会话与远程桌面同步关闭问题

  [专家博客](http://blog.zxbc.cn/) | [论坛](http://bbs.zxbc.cn/) | [下载](http://down.zxbc.cn/) | [视频](http://sp.zxbc.cn/) | [网络](http://www.zxbc.cn/html/network/) | [导航](http://dh.zxbc.cn/) | [Windows](http://www.zxbc.cn/html/windows/) | [IT认证](http://www.zxbc.cn/html/itrz/) | [书籍](http://down.zxbc.cn/shuji/) | [开源](http://www.zxbc.cn/opensource/) | [方案](http://www.zxbc.cn/Solutions/) | [站内导航](http://www.zxbc.cn/html/wzdh/) [![]()](http://www.zxbc.cn/plus/rssmap.html) [![中国自学编程网]()](http://www.zxbc.cn/) [**资讯**](http://www.zxbc.cn/html/news/)[动态](http://www.zxbc.cn/html/news/cmsnews/)[业界](http://www.zxbc.cn/html/ITnews/)  [**语言**](http://www.zxbc.cn/html/kfyy/ "编程语言")[C语言](http://www.zxbc.cn/html/Csutdy/)[C++](http://www.zxbc.cn/html/cjj/)[C#](http://www.zxbc.cn/html/cshorp/)[VB](http://www.zxbc.cn/html/vb/)  [**开发**](http://www.zxbc.cn/html/rjkf/)[ERP](http://www.zxbc.cn/html/erp/)[项目管理](http://www.zxbc.cn/html/pm/)[软件工程](http://www.zxbc.cn/html/rjgc/)  [**数据库**](http://www.zxbc.cn/html/sjk/)[MSSQL](http://www.zxbc.cn/html/SQLServer/)[Oracle](http://www.zxbc.cn/html/Oracle/) [Mysql](http://www.zxbc.cn/html/MySQL/)  [**WEB**](http://www.zxbc.cn/html/webkaifa/)[ASP](http://www.zxbc.cn/html/article/)[PHP](http://www.zxbc.cn/html/PHP/)[ASP.NET](http://www.zxbc.cn/html/aspnet/) [人物](http://www.zxbc.cn/html/renwu/)[商业](http://www.zxbc.cn/html/sydt/)[通讯](http://www.zxbc.cn/html/tongxun/) [.NET](http://www.zxbc.cn/html/dnet/)[JAVA](http://www.zxbc.cn/html/java/)[Delphi](http://www.zxbc.cn/html/DELPHI/)[算法](http://www.zxbc.cn/html/sf/) [游戏](http://www.zxbc.cn/html/yxkf/)[嵌入式](http://www.zxbc.cn/html/qrskf/)[测 试](http://www.zxbc.cn/html/yyzh/)[移动开发](http://www.zxbc.cn/html/3gwl/) [IBM-DB2](http://www.zxbc.cn/html/DB2/) [Sybase](http://www.zxbc.cn/html/Sybase/) [ACCESS](http://www.zxbc.cn/html/Access/)[VFP](http://www.zxbc.cn/html/VFP/) [XML](http://www.zxbc.cn/html/xml/)[HTML](http://www.zxbc.cn/html/HEML/)[CSS+DIV](http://www.zxbc.cn/html/article/webprog/)[JSP](http://www.zxbc.cn/html/jsp2/)  [中国自学编程网](http://www.zxbc.cn/) > [Windows](http://www.zxbc.cn/html/Windows/) > [网络技巧](http://www.zxbc.cn/html/oswl/) > 正文 ![]()   []() **远程会话与远程桌面同步关闭问题** 来源：中国自学编程网   发布日期：2008-06-03   在日常操作中，网管员往往哪个需要通过远程桌面在主机中进行下载、文件安装、程序运行等等操作，这时，即使管理员推出远程桌面我们也希望这些操作照常进行，不要中断，不过在实际情况中却有很多情况不是这样，只要管理员退出这些操作就会同步中断，给网管员的操作带来了很大的不便。从理论上来说，在对服务器主机进行远程控制时，网络管理员只要不对服务器系统执行系统注销操作或重新启动操作，只是简单地单击远程桌面连接窗口右上角处的关闭按钮时，那些通过远程控制方式启动运行的应用程序还应该继续以后台方式运行，并不会跟随远程桌面连接窗口的关闭而同步关闭。但为什么会出现同步关不的问题呢，经过笔者的观察，这主要是设置的问题，只要我们进行了正确的设置，就完全可以避免此种情况的发生。现在，本文就将该故障的详细排除过程贡献出来：

首先看看在远程登录服务器系统时使用的帐号设置是否正确，如果登录帐号的权限不够或者属性参数设置不当的话，那么就容易出现远程会话同步关闭的现象。假设网络管理员以系统管理员帐号“administrator”来远程登录服务器系统的，在检查“administrator”帐号的设置正确性时，我们可以在服务器系统中用鼠标右键单击桌面上的“我的电脑”图标，从弹出的右键菜单中执行“管理”命令，打开服务器系统的计算机管理界面。在该界面的左侧显示区域，用鼠标依次展开“系统工具”/“本地用户和用户组”/“用户”分支选项，在对应“用户”分支选项的右侧显示区域中，选中目标登录帐号“administrator”，并用鼠标右键单击该帐号，再执行右键菜单中的“属性”命令，打开对应帐号的属性设置界面；单击该设置界面中的“会话”选项卡，在对应的选项设置界面中我们会看到“空闲会话限制”、“活动会话限制”、“结束已断开的会话”、“当达到会话极限或连接中断时如何操作的设置”等几个参数（如图1所示）。其中“空闲会话限制”选项是在服务器系统中没有进行任何操作时所要设置的一项参数，“活动会话限制”选项是用来限制服务器系统中活动连接持续使用时间的一种参数，“结束已断开的会话”选项是用来强行关闭某个会话连接的一项参数。为了避免远程会话同步关闭现象的发生，我们必须在这里将上面的各项参数全部修改为“从不”，以确保远程会话不会被服务器系统强行关闭。

 

   

![图1]( "图1")

图1

    
    其次检查服务器系统中的终端服务配置参数是否正确，如果终端服务器模式设置不当的话，也可能引起远程会话同步关闭的现象。打开服务器系统的“开始”菜单，从中依次选择“程序”/“管理工具”/“终端服务配置”选项，进入终端服务配置界面，选中该界面左侧显示区域的“服务器设置”选项，并在对应该选项的右侧显示区域我们就能看到终端服务器模式究竟是什么了，要是发现终端服务器模式不是“应用程序服务器”模式时，我们必须及时修改过来，并且还需要在这里将“活动桌面”功能启用起来（如图2所示）。
   

   

![图2]( "图2")

图2

    
    下面我们还要对远程终端服务属性界面中的一些参数进行检查，并且这里的参数设置优先级一般要高于系统登录帐号属性界面中的参数设置。在进行这种检查时，我们可以先按照前面的操作打开服务器系统的终端服务配置界面，之后依次选中“终端服务配置”/“连接”选项，在对应该选项右侧显示区域中用鼠标右键单击“RDP-TCP”选项，从弹出的快捷菜单中执行“属性”命令，打开远程终端服务属性设置界面（如图3所示）；在该设置界面中我们看到了“替代用户设置”这个选项参数，要是将该选项参数选中的话，那么我们之前在系统登录帐号属性界面中设置的各项参数都将不能发挥作用，所有参数都会自动按照远程终端服务属性设置界面中的参数进行处理，要是没有选中“替代用户设置”这个选项参数时，那么我们之前在系统登录帐号属性界面中设置的各项参数才能生效。而且，“替代用户设置”设置项下面，我们同样也会看到“空闲会话限制”、“活动会话限制”、“结束已断开的会话”、“当达到会话极限或连接中断时如何操作的设置”等几个参数，我们可以根据工作需要进行有针对性设置就行了。

 

   

![图3]( "图3")

图3

    
    网络管理员在远程控制单位服务器系统时，发现这里的“替代用户设置”选项被意外选中了，同时“结束已断开的会话”参数也被设置为了1分钟，这也是为什么网络管理员通过远程桌面连接在服务器系统中启动某个会话连接后，单击远程桌面连接窗口中的关闭按钮时该会话连接也会同步关闭的原因。找到故障原因后，问题就很好解决了，网络管理员在这里将“结束已断开的会话”参数设置为了“从不”后，就立即解决了服务器远程会话同步关闭故障。  ![]() 相关文章  关于 **远程会话与远程桌面同步关闭问题** ·[ADSl浅析](http://www.zxbc.cn/html/20080530/51611.html)
·[Windows 2000 Server DNS维护一点通](http://www.zxbc.cn/html/20080530/51701.html)
·[用"连接共享"实现ADSL共享上网](http://www.zxbc.cn/html/20080530/51959.html)
·[通过NETBIOS实现信息收集与渗透(1)](http://www.zxbc.cn/html/20080530/51612.html)
·[未公开的Windows网络工具](http://www.zxbc.cn/html/20080530/51636.html)
·[去除Windows 2000的默认共享](http://www.zxbc.cn/html/20080530/51991.html) **新闻动态** ·[网络电视突遇监管风暴 腾讯QQlive停播部份节目](http://www.zxbc.cn/html/20070419/1254.html)
 ·[熊猫烧香病毒案告破 8犯罪嫌疑人被抓获](http://www.zxbc.cn/html/20070419/1111.html) ·[第三届中国(南京)国际软件产品博览会](http://www.zxbc.cn/html/20070616/22989.html)
 ·[百度将推新一代营销平台“我的营销中心”](http://www.zxbc.cn/html/20080527/51220.html) ·[微软收购企业群组通讯软件商Parlano](http://www.zxbc.cn/html/20070903/26868.html)
 ·[Sun和微软加深合作成立了Sun/微软协作中心](http://www.zxbc.cn/html/20080311/32227.html) ·[大企业领袖“高薪低能排行榜”](http://www.zxbc.cn/html/20070510/16550.html)
 ·[入侵省内多所高校计算机系统，制贩假文凭被捕](http://www.zxbc.cn/html/20070913/26946.html) ·[戴尔代工厂被曝出违反《劳动法》有关规定](http://www.zxbc.cn/html/20071127/29814.html)
 ·[互联网公司应主动"锻炼"积极"过冬"](http://www.zxbc.cn/html/20081121/68047.html)   **栏目推荐**   ![]() ![]() ·[让PXE无盘工作站使用固定的IP地址](http://www.zxbc.cn/html/20080530/51542.html)
·[网络数据库的复制和同步(2)](http://www.zxbc.cn/html/20080530/51569.html)
·[需要密码的Windows XP系统的共享文件夹](http://www.zxbc.cn/html/20080530/51974.html)
·[巧设IP地址便于简化管理网络](http://www.zxbc.cn/html/20081112/67722.html)
·[利用Win XP自带工具实现远程管理](http://www.zxbc.cn/html/20080530/51891.html)
·[在网络服务中避免远程引用](http://www.zxbc.cn/html/20080530/51768.html)   **专家博客**   ![]() ![]() **程序人生**   [![编程的首要原则是？]()](http://www.zxbc.cn/html/20090524/71367.html) [编程的首要原则是？](http://www.zxbc.cn/html/20090524/71367.html) [![北漂IT人如何崛起网络间]()](http://www.zxbc.cn/html/20080829/64957.html) [北漂IT人如何崛起网络](http://www.zxbc.cn/html/20080829/64957.html) ·[JBoss创始人Marc Fleury的成功职业生涯](http://www.zxbc.cn/html/20070511/16558.html)
·[怎样才能成为高手?](http://www.zxbc.cn/html/20080813/64330.html)
·[亲历惊心48小时,抢救35亿交易数据](http://www.zxbc.cn/html/20081225/69262.html)
·[微软架构师谈编程语言发展（一）](http://www.zxbc.cn/html/20070804/25703.html)
·[IT人你除了IT还能干什么？](http://www.zxbc.cn/html/20070519/20020.html)
·[电子工程师也是十余年了](http://www.zxbc.cn/html/20070521/20077.html) [关于站点](http://www.zxbc.cn/html/gyzd/) - [联系我们](http://www.zxbc.cn/html/gywm/) - [版权隐私](http://www.zxbc.cn/html/bqys/) -  - [返回顶部](http://www.zxbc.cn/html/20080603/56461.html#top)  中国自学编程网版权所有 本站原创文章，未经授权禁止转载或建立镜象站点 E-Mail:520it@163.com Tel:15802529892 Copyright 2006-2008 Zxbc.cn Corporation 中国自学编程网倡导软件开发学习文化，崇尚软件开源和共享，致力于帮助编程学习者在各自专业领域取得成功！
