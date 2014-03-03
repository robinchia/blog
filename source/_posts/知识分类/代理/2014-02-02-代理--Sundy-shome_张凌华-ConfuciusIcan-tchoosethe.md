---
layout: post
title: "Sundy-s home _ 张凌华 - Confucius I can-t choose the "
categories: 代理
tags: 
 - 代理
--- 

# Sundy's home _ 张凌华 - Confucius I can't choose the best , the best choose me

# [Sundy's home . 张凌华](http://www.lhzhang.org/)

## Confucius: I can't choose the best , the best choose me .

* [Home](http://mobidever.com/)
* [Archive](http://mobidever.com/archive.aspx)
* [Product](http://mobidever.com/page/my-product.aspx)
* [Education](http://mobidever.com/page/my-education.aspx)
* [Books](http://mobidever.com/page/Books.aspx)
* [Album](http://mobidever.com/page/my-album.aspx)
* [Contact](http://mobidever.com/contact.aspx)
* [About Me](http://mobidever.com/page/about.aspx)
* [![Feed]()Subscribe](http://www.lhzhang.org/syndication.axd)
# [GOAGENT又一个基于GAE的穿越利器](http://mobidever.com/post/2011/08/GOAGENTe58f88e4b880e4b8aae59fbae4ba8eGAEe79a84e7a9bfe8b68ae588a9e599a8.aspx)

by [sundy](http://mobidever.com/author/sundy.aspx) 8/16/2011 7:26:00 AM

### GoAgent是 一个使用Python和Google Appengine SDK编写的代理软件。部署和使用方法非常简单，不需要安装Python或者Google Appenginge SDK ，几分钟即可搞定。

### 具体步骤如下：

### GoAgent『申请与创建』

首先申请[ ](https://www.google.com/accounts/Login?hl=zh-CN)注册一个**Google App Engine**账号（[**点此注册**](http://appengine.google.com/)）。没有**Gmail**账号先注册一个， 用你的**Gmaill**账号登录。

过程详解如下：**下图：**

![]()

登录之后，自动转向**Application**注册页面，如**下图**：

![]()

接下来的页面，**输入你的手机号码**，**如下图：**

![]()

需要注意的是，**手机号码前面要+86 **格式如：**+86 13888888888**。然后等待收取手机短信，收到短信后（一串数字号码）填入下图表单，点**send**提交.

![]()

提交完成之后，**GAE**账号即被激活，然后就可以创建新的应用程序了。转入**“My Applications”**页面，点击**“Create an Application”**新建应用，如**下图**：

![]()

一个**Gmail**账户最多可以创建十个**GAE**应用。这里我们只创建一个应用就可以了。进入下一步，填写新应用的必要信息，**如下图**：

![]()

在上图中**第一处添加一个应用名称**，如**abc555**验证一下是否可用，如果通过那么**abc555**就是你的**Appid**（记住这个**id**），而abc555 .appspoft.com就是你的应用服务器地址了。**第二个空可随便填**，点击提交按钮，如果能看到下图这个页面，就说明你成功创建了一个新的应用,你也可以点击应用名称，进入控制面板进行管理。

![]()

### GoAgent『部署和使用』

### 1.申请Google Appengine并创建appid
2.下载GoAgent  [https://github.com/phus/goagent/zipball/master](https://github.com/phus/goagent/zipball/master) （新版）
3.双击server\upload.bat,输入你的appid和你的Gmail帐号和密码，就会自动上传到服务端
4.把local\proxy.ini中的your_appid 改成你申请到的appid
5.设置浏览器代理为127.0.0.1：8087
6.运行taskbar.exe  好了，现在你可以穿墙了。

![]()

**上图为第3 步截图**。（输入AppID按回车，再输入gmail帐号按回车，输入密码后再按回车键（注：输入密码时不会显示）就开始自动上传了，多等一会，上传完毕后黑窗口会自动关.

![]()

![]()

**上图为第4步的截图。**把**local\proxy.ini**中的**your_appid** 改成你申请到的**appid** (用windows的记事本也可以）

![]()

你也可登录你的GAE账户，在后台管理查看**（上图）**

**笔者建议用Firefox浏览器，再安装一个Autoproxy 插件，可以在是否使用代理选择上非常方便地切换。安装插件[https://addons.mozilla.org/zh-CN/firefox/addon/autoproxy/](https://addons.mozilla.org/zh-CN/firefox/addon/autoproxy/) 安装后，因autoproxy插件里没有goagent代理选项，须自建一个，步骤如下：
重启浏览器后，点击浏览器上方的/工具/autoproxy/代理服务器/编辑代理服务器/添加代理/然后新建一个“名称goagent  主机127.0.0.1  端口8087/确定”，然后选择代理服务器/goagent/确定。（下图）**

![]()

![]()

**运行taskbar.exe （下图）**

![]()

**启动Firefox浏览器，（autoproxy插件安装后，在浏览器的右上角或右下角有一个“福”字，点击这个字，绿色为全局代理，红色为自动判别模式）。点击[http://www.ip-adress.com/ip_tracer/](http://www.ip-adress.com/ip_tracer/)，全局模式下看到下图，穿越成功。**

![]()

GoAgent 现在越来越简单了，太容易搭建了，且速度飞快。（不建议用**IE**浏览器，在**Chrome****和Firefox**下没有任何问题，但用**IE**时常常翻不出去。）Goagent 打开SSL连接的网站，如果浏览器弹出证书无效警告，可以用这样的方法解决:导入证书：在local文件夹下的ssl文件夹有一个ca.crt证书文件；# Firefox依次操作：“首选项->高级->加密->查看证书->证书机构->导入->选择 local->sll->ca.crt 文件–>确定”，即可导入成功。（Chrome下直接双击ca.crt安装证书）
Currently rated 2.6 by 517 people

* Currently 2.601584/5 Stars.
* [1]( "Rate this 1 star out of 5")
* [2]( "Rate this 2 stars out of 5")
* [3]( "Rate this 3 stars out of 5")
* [4]( "Rate this 4 stars out of 5")
* [5]( "Rate this 5 stars out of 5")

Tags:

[Internet Communion](http://mobidever.com/category/Internet-Communion.aspx) | [Mobile & Wireless](http://mobidever.com/category/Mobile-amp3b-Wireless.aspx)

[E-mail](mailto:?subject=GOAGENT又一个基于GAE的穿越利器&body=Thought you might like this: http://www.lhzhang.org/post/2011/08/GOAGENTe58f88e4b880e4b8aae59fbae4ba8eGAEe79a84e7a9bfe8b68ae588a9e599a8.aspx) | [Kick it!](http://www.dotnetkicks.com/submit?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f08%2fGOAGENTe58f88e4b880e4b8aae59fbae4ba8eGAEe79a84e7a9bfe8b68ae588a9e599a8.aspx&title=GOAGENT%e5%8f%88%e4%b8%80%e4%b8%aa%e5%9f%ba%e4%ba%8eGAE%e7%9a%84%e7%a9%bf%e8%b6%8a%e5%88%a9%e5%99%a8) | [DZone it!](http://www.dzone.com/links/add.html?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f08%2fGOAGENTe58f88e4b880e4b8aae59fbae4ba8eGAEe79a84e7a9bfe8b68ae588a9e599a8.aspx&title=GOAGENT%e5%8f%88%e4%b8%80%e4%b8%aa%e5%9f%ba%e4%ba%8eGAE%e7%9a%84%e7%a9%bf%e8%b6%8a%e5%88%a9%e5%99%a8) | [del.icio.us](http://del.icio.us/post?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f08%2fGOAGENTe58f88e4b880e4b8aae59fbae4ba8eGAEe79a84e7a9bfe8b68ae588a9e599a8.aspx&title=GOAGENT%e5%8f%88%e4%b8%80%e4%b8%aa%e5%9f%ba%e4%ba%8eGAE%e7%9a%84%e7%a9%bf%e8%b6%8a%e5%88%a9%e5%99%a8)[Permalink](http://www.lhzhang.org/post.aspx?id=b3c84fae-92d8-4ab6-9a11-6e674e7c0359) | [Comments (0)](http://mobidever.com/post/2011/08/GOAGENTe58f88e4b880e4b8aae59fbae4ba8eGAEe79a84e7a9bfe8b68ae588a9e599a8.aspx#comment) | [Post RSS![RSS comment feed]()](http://mobidever.com/syndication.axd?post=b3c84fae-92d8-4ab6-9a11-6e674e7c0359)

# [Learn to play the Les Paul Google Doodle](http://mobidever.com/post/2011/06/Learn-to-play-the-Les-Paul-Google-Doodle.aspx)

by [sundy](http://mobidever.com/author/sundy.aspx) 6/10/2011 10:04:00 AM

After getting much positive feedback on its playable guitar to honor guitar hero Lester William Polsfuss (Les Paul), Google left its doodle in its United States site for an extra day.
In a message on [its Twitter account](http://twitter.com/#!/GOOGLE), Google cited "popular demand" in its decision to extend its latest Google Doodle for a day.
"Due to popular demand, we're leaving the Les Paul doodle up in the U.S. for an extra day. Thanks for jamming with us!" it said.
In the doodle that came out Thursday, hovering the mouse or pointing device over the strings would produce a specific note.
Paul is credited for popularizing innovations such as delay effects, phasing effects and multi-track recording.
While the doodle is accessible only in the US website, users outside the US can get to the digital guitar by by clicking on the "Go to Google.com" link.
**Inner workings**
[An article on IBN Live](http://ibnlive.in.com/news/google-keeps-les-paul-doodle-up-for-an-extra-day/158086-11.html) said the doodle was made with a combination of JavaScript, HTML5 Canvas, CSS, Flash and tools like the Google Font API, goo.gl and App Engine.
The IBN Live article added that of the last 10 Google Doodles, five included animations or were interactive.
The Google Doodle first went interactive in May 2010 to celebrate the 30th birthday of the popular Pac-Man game.
Meanwhile, musicians continued to post online their work using the Google Doodle digital guitar.
PC Magazine said its creative director Chris Phillips, a musician, managed to put together the opening to "Here Comes the Sun" by The Beatles.
"After toying around with the Doodle for a while, Chris figured out which keys corresponded to which notes. The Doodle works on all four rows of the keyboard starting at 1, Q, A, and Z. On the A row through the semicolon, you can play an octave plus two notes. With that knowledge, Chris set out to play something memorable. A Beatles song was a logical choice," it said.
The key sequence was posted [on PC Mag's site](http://www.pcmag.com/article2/0,2817,2386684,00.asp).
PC World said Phillips agreed that this is probably Google's most distracting Doodle to date.
It added Phillips agreed that Google should commemorate John Bonham with a fully interactive drum kit.
"Time will tell if Google will do that," it said.
**Playable on keyboard too**
The digital guitar can be played using the mouse to hover over the strings; or the keyboard, with the Huffington Post offering [a brief tutorial](http://www.huffingtonpost.com/2011/06/09/how-to-play-guitar-on-google_n_873895.html?ref=tw).
Each note has a corresponding numerical key, starting at the low G string:
1 = G
2 = A
3 = B
4 = C
5 = D
6 = E
7 = F#
8 = G
9 = A
10 = B
"Google guitar's keyboard function also works using corresponding letter keys down the rows of the keyboard, meaning that low G can be played using the 1, Q, A and Z keys, the low A can be played using the 2, W, S and X keys, and so on," it said.
It also invited the guitar doodle users to send URLs of their compositions, saying it will post the best songs online.** — TJD, GMA News**
Currently rated 1.7 by 121 people

* Currently 1.661157/5 Stars.
* [1]( "Rate this 1 star out of 5")
* [2]( "Rate this 2 stars out of 5")
* [3]( "Rate this 3 stars out of 5")
* [4]( "Rate this 4 stars out of 5")
* [5]( "Rate this 5 stars out of 5")

Tags:

[Game Life](http://mobidever.com/category/Game-Life.aspx)

[E-mail](mailto:?subject=Learn to play the Les Paul Google Doodle&body=Thought you might like this: http://www.lhzhang.org/post/2011/06/Learn-to-play-the-Les-Paul-Google-Doodle.aspx) | [Kick it!](http://www.dotnetkicks.com/submit?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f06%2fLearn-to-play-the-Les-Paul-Google-Doodle.aspx&title=Learn+to+play+the+Les+Paul+Google+Doodle) | [DZone it!](http://www.dzone.com/links/add.html?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f06%2fLearn-to-play-the-Les-Paul-Google-Doodle.aspx&title=Learn+to+play+the+Les+Paul+Google+Doodle) | [del.icio.us](http://del.icio.us/post?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f06%2fLearn-to-play-the-Les-Paul-Google-Doodle.aspx&title=Learn+to+play+the+Les+Paul+Google+Doodle)[Permalink](http://www.lhzhang.org/post.aspx?id=147b41e1-d5df-42ab-9e67-880a8c41c342) | [Comments (0)](http://mobidever.com/post/2011/06/Learn-to-play-the-Les-Paul-Google-Doodle.aspx#comment) | [Post RSS![RSS comment feed]()](http://mobidever.com/syndication.axd?post=147b41e1-d5df-42ab-9e67-880a8c41c342)
# [虽然喜欢这段视频，但拿孩子炒作广告很让人心酸 。](http://mobidever.com/post/2011/06/e899bde784b6e5969ce6aca2e8bf99e6aeb5e8a786e9a291efbc8ce4bd86e68bbfe5ada9e5ad90e78292e4bd9ce5b9bfe5918ae5be88e8aea9e4babae5bf83e985b8-e38082.aspx)

by [sundy](http://mobidever.com/author/sundy.aspx) 6/10/2011 9:58:00 AM

Currently rated 2.7 by 39 people

* Currently 2.666668/5 Stars.
* [1]( "Rate this 1 star out of 5")
* [2]( "Rate this 2 stars out of 5")
* [3]( "Rate this 3 stars out of 5")
* [4]( "Rate this 4 stars out of 5")
* [5]( "Rate this 5 stars out of 5")

Tags:

[Gossip](http://mobidever.com/category/Gossip.aspx)
[E-mail](mailto:?subject=虽然喜欢这段视频，但拿孩子炒作广告很让人心酸 。&body=Thought you might like this: http://www.lhzhang.org/post/2011/06/e899bde784b6e5969ce6aca2e8bf99e6aeb5e8a786e9a291efbc8ce4bd86e68bbfe5ada9e5ad90e78292e4bd9ce5b9bfe5918ae5be88e8aea9e4babae5bf83e985b8-e38082.aspx) | [Kick it!](http://www.dotnetkicks.com/submit?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f06%2fe899bde784b6e5969ce6aca2e8bf99e6aeb5e8a786e9a291efbc8ce4bd86e68bbfe5ada9e5ad90e78292e4bd9ce5b9bfe5918ae5be88e8aea9e4babae5bf83e985b8-e38082.aspx&title=%e8%99%bd%e7%84%b6%e5%96%9c%e6%ac%a2%e8%bf%99%e6%ae%b5%e8%a7%86%e9%a2%91%ef%bc%8c%e4%bd%86%e6%8b%bf%e5%ad%a9%e5%ad%90%e7%82%92%e4%bd%9c%e5%b9%bf%e5%91%8a%e5%be%88%e8%ae%a9%e4%ba%ba%e5%bf%83%e9%85%b8+%e3%80%82) | [DZone it!](http://www.dzone.com/links/add.html?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f06%2fe899bde784b6e5969ce6aca2e8bf99e6aeb5e8a786e9a291efbc8ce4bd86e68bbfe5ada9e5ad90e78292e4bd9ce5b9bfe5918ae5be88e8aea9e4babae5bf83e985b8-e38082.aspx&title=%e8%99%bd%e7%84%b6%e5%96%9c%e6%ac%a2%e8%bf%99%e6%ae%b5%e8%a7%86%e9%a2%91%ef%bc%8c%e4%bd%86%e6%8b%bf%e5%ad%a9%e5%ad%90%e7%82%92%e4%bd%9c%e5%b9%bf%e5%91%8a%e5%be%88%e8%ae%a9%e4%ba%ba%e5%bf%83%e9%85%b8+%e3%80%82) | [del.icio.us](http://del.icio.us/post?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f06%2fe899bde784b6e5969ce6aca2e8bf99e6aeb5e8a786e9a291efbc8ce4bd86e68bbfe5ada9e5ad90e78292e4bd9ce5b9bfe5918ae5be88e8aea9e4babae5bf83e985b8-e38082.aspx&title=%e8%99%bd%e7%84%b6%e5%96%9c%e6%ac%a2%e8%bf%99%e6%ae%b5%e8%a7%86%e9%a2%91%ef%bc%8c%e4%bd%86%e6%8b%bf%e5%ad%a9%e5%ad%90%e7%82%92%e4%bd%9c%e5%b9%bf%e5%91%8a%e5%be%88%e8%ae%a9%e4%ba%ba%e5%bf%83%e9%85%b8+%e3%80%82)[Permalink](http://www.lhzhang.org/post.aspx?id=41fa6b1d-ebb7-452e-9027-fa02f1f89f74) | [Comments (0)](http://mobidever.com/post/2011/06/e899bde784b6e5969ce6aca2e8bf99e6aeb5e8a786e9a291efbc8ce4bd86e68bbfe5ada9e5ad90e78292e4bd9ce5b9bfe5918ae5be88e8aea9e4babae5bf83e985b8-e38082.aspx#comment) | [Post RSS![RSS comment feed]()](http://mobidever.com/syndication.axd?post=41fa6b1d-ebb7-452e-9027-fa02f1f89f74)

# [我喜欢的Swepy输入法 - for 智能设备](http://mobidever.com/post/2011/06/e68891e5969ce6aca2e79a84Swepye8be93e585a5e6b395---for-e699bae883bde8aebee5a487.aspx)

by [sundy](http://mobidever.com/author/sundy.aspx) 6/8/2011 11:30:00 AM

### What is Swype?

Swype provides a faster and easier way to input text on any screen. With one continuous finger or stylus motion across the screen keyboard, the patented technology enables users to input words faster and easier than other data input methods—at over 40 words per minute. The application is designed to work across a variety of devices such as phones, tablets, game consoles, kiosks, televisions, virtual screens and more.
![what is swype?]()

### Simply Trace a Path

The word “quick” was generated from tracing the path shown above in a fraction of a second, by roughly aiming to pass through the letters of the word. A key advantage to Swype is that there is no need to be very accurate, enabling very rapid text entry.
Currently rated 1.0 by 1 people

* Currently 1/5 Stars.
* [1]( "Rate this 1 star out of 5")
* [2]( "Rate this 2 stars out of 5")
* [3]( "Rate this 3 stars out of 5")
* [4]( "Rate this 4 stars out of 5")
* [5]( "Rate this 5 stars out of 5")

Tags:

[E-mail](mailto:?subject=我喜欢的Swepy输入法 - for 智能设备&body=Thought you might like this: http://www.lhzhang.org/post/2011/06/e68891e5969ce6aca2e79a84Swepye8be93e585a5e6b395---for-e699bae883bde8aebee5a487.aspx) | [Kick it!](http://www.dotnetkicks.com/submit?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f06%2fe68891e5969ce6aca2e79a84Swepye8be93e585a5e6b395---for-e699bae883bde8aebee5a487.aspx&title=%e6%88%91%e5%96%9c%e6%ac%a2%e7%9a%84Swepy%e8%be%93%e5%85%a5%e6%b3%95+-+for+%e6%99%ba%e8%83%bd%e8%ae%be%e5%a4%87) | [DZone it!](http://www.dzone.com/links/add.html?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f06%2fe68891e5969ce6aca2e79a84Swepye8be93e585a5e6b395---for-e699bae883bde8aebee5a487.aspx&title=%e6%88%91%e5%96%9c%e6%ac%a2%e7%9a%84Swepy%e8%be%93%e5%85%a5%e6%b3%95+-+for+%e6%99%ba%e8%83%bd%e8%ae%be%e5%a4%87) | [del.icio.us](http://del.icio.us/post?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f06%2fe68891e5969ce6aca2e79a84Swepye8be93e585a5e6b395---for-e699bae883bde8aebee5a487.aspx&title=%e6%88%91%e5%96%9c%e6%ac%a2%e7%9a%84Swepy%e8%be%93%e5%85%a5%e6%b3%95+-+for+%e6%99%ba%e8%83%bd%e8%ae%be%e5%a4%87)[Permalink](http://www.lhzhang.org/post.aspx?id=4682e194-a6df-4e74-a61b-e32692037a30) | [Comments (0)](http://mobidever.com/post/2011/06/e68891e5969ce6aca2e79a84Swepye8be93e585a5e6b395---for-e699bae883bde8aebee5a487.aspx#comment) | [Post RSS![RSS comment feed]()](http://mobidever.com/syndication.axd?post=4682e194-a6df-4e74-a61b-e32692037a30)
# [台湾IT业Android工程师紧俏 年薪50万大陆抢人](http://mobidever.com/post/2011/06/e58fb0e6b9beITe4b89aAndroide5b7a5e7a88be5b888e7b4a7e4bf8f-e5b9b4e896aa50e4b887e5a4a7e99986e68aa2e4baba.aspx)

by [sundy](http://mobidever.com/author/sundy.aspx) 6/5/2011 2:20:05 PM

面对传统PC向移动互联网的转变，台湾IT行业的人才资源也面临一次由“硬”到“软”的转型。

不仅在岛内争夺升温，大量的台湾硬件厂商还把目光瞄向了内地——这里聚集着大量的软件研发工程师，尤其是正当潮流的Android系统平台，相比台湾，大陆研发人才储备更为丰富。

这与过去台湾仅把大陆当作代工基地和销售市场的时代，相去甚远。

“由于近年内地山寨产业的发展，事实上在基于Android平台的研发上，内地研发人员走在了台湾公司的前面。”深圳一家[平板电脑](http://tech.ifeng.com/digi/ehome/detail_2010_04/12/519029_0.shtml)厂商负责人告诉记者，台湾硬件厂商倾注太多精力在wintel架构（指windows系统和intel架构），在基于ARM和Android架构上的资源投入严重不足——尽管台湾有宏达电，但不过凤毛麟角。

台湾硬件厂商们难以回避的现实是，面对移动互联网时代的大趋势，ARM加Android的架构无疑更具优势——更快的运算速度和更低的功耗，但这需要极强的软硬件整合能力。

而这恰恰是台湾传统硬件厂商的软肋。

**由“硬”到“软”转型**

中层软硬整合正是台湾硬件厂商的软肋

今年上半年，台湾的IT制造企业集体“加码”软件研发。

仁宝总经理陈瑞聪在今年一季度法人会上表示，软件将是公司今年重点发展项目：“规模要从当前的100人，扩充到600人。”

宏碁和宏达电也纷纷表示，今年年底要招募超过1000名软件工程师。

“台湾IT业过去的优势在硬件代工，软件研发实力稍显薄弱。”半导体行业分析人士王艳辉认为，面对向移动互联网转型的大趋势，台湾IT企业需要加强软件研发领域的投入，尤其是基于Android平台的应用开发。

在过去PC时代，软件设计部分基于[微软](http://app.tech.ifeng.com/enterprise/index.php?name=%E5%BE%AE%E8%BD%AF)和[英特尔](http://app.tech.ifeng.com/enterprise/index.php?name=%E8%8B%B1%E7%89%B9%E5%B0%94)架构，相对成熟。而在功能性手机产品上，联发科的turn key模式，让下游集成商在软件研发上不需要耗费太大的精力，即可“一站式”解决。

相比之下，Android系统平台分为上层应用程式端、中层软硬整合平台和下层硬件驱动。其中，中层软硬整合正是台湾硬件厂商的软肋。

“让公司从硬件制造转向软件和服务时，对于传统的代工厂商，是个不小的挑战。”在王艳辉看来，相比之下，大陆IT业界在软件研发上，尤其是基于ARM和Android架构的研发上，近年积累迅速。

在此背景下，台湾传统硬件制造商针对软件研发人才的“争夺战”，悄然打响。本报记者了解到，6月初，仁宝在昆山基地、[富士康](http://tech.ifeng.com/it/detail_2010_04/08/514310_0.shtml)在深圳基地，近期都展开大规模软件研发人员的招聘工作。

**来大陆挖人**

**台湾硬件厂商还通过猎头公司四处挖角**

“刚和BENQ（明基）朋友聊天，台湾大厂确实在到处挖软件人才，非常缺。”6月3日下午，身在台北电脑展的深圳创智成科技有限公司总经理徐建平给记者发来短信。

据本报记者了解，除了常规招聘外，台湾硬件厂商还通过猎头公司四处挖角，其中大陆山寨厂商的Android研发工程师，也成为台湾硬件厂商觊觎的对象。

“事实上，深圳的山寨厂商，是国内最早从事Android研发的一个群体。”一位深圳原山寨厂商负责人告诉记者，早在2009年初，受山寨机市场利润持续下滑的影响，部分山寨机厂商开始基于Android的研发，以期望尽早抢占市场，拉低价格。

一个可供参考的事实是，早在2009年7月，深圳手机设计公司创扬通信就号称推出了首款基于Android平台的手机。（见本报2009年7月24日报道《深圳草莽竞跑Android：阻击Gphone入华》）

 

“由于Android系统的不稳定性，加之成本过高。事实上，绝大部分山寨厂商推出的Android手机，都没有成功。”上述山寨厂商人士认为，但这次经历为Android的研发积累了经验。

不止于此，相比较台湾硬件厂商在wintel架构倾入太多资源，而在arm和Android架构面临“船大难掉头”的困境，大陆电子企业则显得“轻装上阵”。

例如，福州瑞芯微电子是大陆最早由MP3转型Android平台研发的芯片设计公司，今年1月，瑞芯微电子推

出首个基于Android系统的智能设备提供全套解决方案RK29XX，其涵盖了包括智能手机、MID、[平板电脑](http://tech.ifeng.com/digi/ehome/detail_2010_04/12/519029_0.shtml)、电子书、网络电视、MP4等多种电子设备。

在此背景下，仁宝、联发科、Mstar纷纷在内地兴建研发机构，招募大量的本地化软件研发人才，其中Android工程师成为“抢手货”。

**紧俏的Android工程师**

**Android开发工程师呈现紧缺态势，多方争夺之下，其工资薪酬水涨船高**

“直到今天，Android研发工程师仍很抢手。”徐建平在电话中感慨。

创智成一度是深圳当地最大的主板生产商，在昙花一现的上网本时代，创智成一度占据深圳上网本市场出货量的50%以上。其后，其快速转入Android系统平板电脑的研发，是深圳当地最早一批进入Android平台研发的厂商之一。

“公司有上百人的研发团队，但Android研发人才仍旧紧俏。”徐建平告诉本报记者。

据徐分析，目前Android研发工程师呈现稀缺局面，主要有两个方面原因。其一，是Android系统上市时间不长，相关技术人才储备尚处在积累阶段；其二，是Android系统自身的不稳定性，这提高了相关的技术人员的研发难度。

事实上，内地的Android研发相比较台湾，仍走在前列。“内地Android平台开发商很多是由MP3、手机平台转型而来的，他们在移动便携式电子设备上较有优势。”徐建平分析，在开发过程中，这些平台商更擅长处理低功耗和便携方面的平衡。

相比之下，台湾大量的硬件企业，以传统PC代工起家，在基于移动互联网应用的研发方面能力不足。

“从去年开始，就有不少台湾硬件厂商，在深圳招募Android工程师。”徐建平告诉记者，Android开发工程师呈现紧缺态势，多方争夺之下，其工资薪酬水涨船高。

据本报记者了解，目前一个资深Android工程师年薪可以拿到50万左右，而一个入行两年的Android工程师，月薪可接近2万。[![]()](http://www.ifeng.com/)
Currently rated 2.3 by 54 people

* Currently 2.314815/5 Stars.
* [1]( "Rate this 1 star out of 5")
* [2]( "Rate this 2 stars out of 5")
* [3]( "Rate this 3 stars out of 5")
* [4]( "Rate this 4 stars out of 5")
* [5]( "Rate this 5 stars out of 5")

Tags:

[Mobile & Wireless](http://mobidever.com/category/Mobile-amp3b-Wireless.aspx) | [Newscaster](http://mobidever.com/category/Newscaster.aspx)

[E-mail](mailto:?subject=台湾IT业Android工程师紧俏 年薪50万大陆抢人&body=Thought you might like this: http://www.lhzhang.org/post/2011/06/e58fb0e6b9beITe4b89aAndroide5b7a5e7a88be5b888e7b4a7e4bf8f-e5b9b4e896aa50e4b887e5a4a7e99986e68aa2e4baba.aspx) | [Kick it!](http://www.dotnetkicks.com/submit?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f06%2fe58fb0e6b9beITe4b89aAndroide5b7a5e7a88be5b888e7b4a7e4bf8f-e5b9b4e896aa50e4b887e5a4a7e99986e68aa2e4baba.aspx&title=%e5%8f%b0%e6%b9%beIT%e4%b8%9aAndroid%e5%b7%a5%e7%a8%8b%e5%b8%88%e7%b4%a7%e4%bf%8f+%e5%b9%b4%e8%96%aa50%e4%b8%87%e5%a4%a7%e9%99%86%e6%8a%a2%e4%ba%ba) | [DZone it!](http://www.dzone.com/links/add.html?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f06%2fe58fb0e6b9beITe4b89aAndroide5b7a5e7a88be5b888e7b4a7e4bf8f-e5b9b4e896aa50e4b887e5a4a7e99986e68aa2e4baba.aspx&title=%e5%8f%b0%e6%b9%beIT%e4%b8%9aAndroid%e5%b7%a5%e7%a8%8b%e5%b8%88%e7%b4%a7%e4%bf%8f+%e5%b9%b4%e8%96%aa50%e4%b8%87%e5%a4%a7%e9%99%86%e6%8a%a2%e4%ba%ba) | [del.icio.us](http://del.icio.us/post?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f06%2fe58fb0e6b9beITe4b89aAndroide5b7a5e7a88be5b888e7b4a7e4bf8f-e5b9b4e896aa50e4b887e5a4a7e99986e68aa2e4baba.aspx&title=%e5%8f%b0%e6%b9%beIT%e4%b8%9aAndroid%e5%b7%a5%e7%a8%8b%e5%b8%88%e7%b4%a7%e4%bf%8f+%e5%b9%b4%e8%96%aa50%e4%b8%87%e5%a4%a7%e9%99%86%e6%8a%a2%e4%ba%ba)[Permalink](http://www.lhzhang.org/post.aspx?id=ceb68991-de86-43b1-9802-df0b2224ce41) | [Comments (0)](http://mobidever.com/post/2011/06/e58fb0e6b9beITe4b89aAndroide5b7a5e7a88be5b888e7b4a7e4bf8f-e5b9b4e896aa50e4b887e5a4a7e99986e68aa2e4baba.aspx#comment) | [Post RSS![RSS comment feed]()](http://mobidever.com/syndication.axd?post=ceb68991-de86-43b1-9802-df0b2224ce41)

# [备注一下，简短的自我介绍](http://mobidever.com/post/2011/05/e5a487e6b3a8e4b880e4b88befbc8ce7ae80e79fade79a84e887aae68891e4bb8be7bb8d.aspx)

by [sundy](http://mobidever.com/author/sundy.aspx) 5/31/2011 2:37:00 PM

[]()[]()[]()十二年软件项目开发设计经验，8年的技术管理经验，带领超过70人的本土团队及超过40人的国际化团队（俄罗斯，印度，波兰，韩国，中国），8年移动开发经验(Palm,BREW,Symbian,J2ME,WM,Android)。超过5年的企业培训经历，给中国移动，中国邮政,阿尔卡特,泰立嘉等20多家企业做过培训。现就职于某北欧公司，任Android架构师，在韩国三星手机事业部北美组从事团队管理工作 。
Currently rated 5.0 by 3 people

* Currently 5/5 Stars.
* [1]( "Rate this 1 star out of 5")
* [2]( "Rate this 2 stars out of 5")
* [3]( "Rate this 3 stars out of 5")
* [4]( "Rate this 4 stars out of 5")
* [5]( "Rate this 5 stars out of 5")

Tags:

[E-mail](mailto:?subject=备注一下，简短的自我介绍&body=Thought you might like this: http://www.lhzhang.org/post/2011/05/e5a487e6b3a8e4b880e4b88befbc8ce7ae80e79fade79a84e887aae68891e4bb8be7bb8d.aspx) | [Kick it!](http://www.dotnetkicks.com/submit?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f05%2fe5a487e6b3a8e4b880e4b88befbc8ce7ae80e79fade79a84e887aae68891e4bb8be7bb8d.aspx&title=%e5%a4%87%e6%b3%a8%e4%b8%80%e4%b8%8b%ef%bc%8c%e7%ae%80%e7%9f%ad%e7%9a%84%e8%87%aa%e6%88%91%e4%bb%8b%e7%bb%8d) | [DZone it!](http://www.dzone.com/links/add.html?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f05%2fe5a487e6b3a8e4b880e4b88befbc8ce7ae80e79fade79a84e887aae68891e4bb8be7bb8d.aspx&title=%e5%a4%87%e6%b3%a8%e4%b8%80%e4%b8%8b%ef%bc%8c%e7%ae%80%e7%9f%ad%e7%9a%84%e8%87%aa%e6%88%91%e4%bb%8b%e7%bb%8d) | [del.icio.us](http://del.icio.us/post?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f05%2fe5a487e6b3a8e4b880e4b88befbc8ce7ae80e79fade79a84e887aae68891e4bb8be7bb8d.aspx&title=%e5%a4%87%e6%b3%a8%e4%b8%80%e4%b8%8b%ef%bc%8c%e7%ae%80%e7%9f%ad%e7%9a%84%e8%87%aa%e6%88%91%e4%bb%8b%e7%bb%8d)[Permalink](http://www.lhzhang.org/post.aspx?id=1ddec803-21a7-432f-bc4c-833677ae212e) | [Comments (0)](http://mobidever.com/post/2011/05/e5a487e6b3a8e4b880e4b88befbc8ce7ae80e79fade79a84e887aae68891e4bb8be7bb8d.aspx#comment) | [Post RSS![RSS comment feed]()](http://mobidever.com/syndication.axd?post=1ddec803-21a7-432f-bc4c-833677ae212e)
# [诺基亚CEO内部备忘录曝光：承认失败承诺改变](http://mobidever.com/post/2011/02/e8afbae59fbae4ba9aCEOe58685e983a8e5a487e5bf98e5bd95e69b9de58589efbc9ae689bfe8aea4e5a4b1e8b4a5e689bfe8afbae694b9e58f98.aspx)

by [sundy](http://mobidever.com/author/sundy.aspx) 2/12/2011 3:54:00 AM

![]() 

**以下为史蒂芬·埃洛普致员工内部备忘录的内容：**

你们好：

有一个经久流传的故事，是关于在北海石油开采平台工作的一个人。有一天晚上，他被巨大的爆炸声惊醒，突然间整个石油平台都被熊熊烈火笼罩。几秒钟的时间里，他也被火光包围在中间。他努力穿过浓烟与烈火，来到了平台边缘，往下只能看到黑暗、冰冷和咆哮的大西洋海水。

随着烈火的靠近，他只有几秒钟时间做出反应。他要么站在平台上，被烈火无情地吞没，要么从30米高的石油平台跳进冰冷的海水中。这个人站立在“燃烧的平台”，他需要作出选择。

他决定跳下去。这有点出乎意料。在通常情况下，他绝不会考虑跳入冰冷的海水中。但他面对的不是通常情况，他所站立的平台已经着火。他成功跳入海中并存活下来。在被救出来之后，他说，“燃烧的平台”使得他采取了极端的行为。

我们也站在一个“燃烧的平台”，我们必须决定如何改变自己的行为。

过去几个月里，我与你们共享了我从股东、运营商、开发商、供应商以及你们那里听到的话。今天，我要共享我所学到的，以及我所坚信的。

我认识到，我们站在一个燃烧的平台。

另外，我们的爆炸不只一次，我们面对着多个炽热的高温点，使得周围的火焰越烧越旺。

例如，我们的竞争对手点燃了炽热的火焰，而且比我们预想的速度更快。苹果重新定义了智能手机，将开发人员吸引到一个封闭，但是强大的生态系统，进而摧毁了整个市场。

2008年，苹果在300美元以上手机市场的占有率为25%；到2010年市场占有率上升至61%。2010年第四季度，苹果营收额同比实现了78%的增长。苹果的成功说明，只要设计优秀，消费者愿意购买拥有出色体验的高价手机，开发人员也愿意为其开发应用。他们改变了游戏格局，而今天苹果拥有了高端市场。

其次，还有Android。在大约两年时间里，Android已经成为一个能够吸引应用开发人员、服务提供商和硬件生产商的平台。Android从高端市场着手，现在已经开始赢得中端市场，很快他们将进入100欧元以下手机的低端市场。谷歌就像地心引力一样，吸纳了行业内大量的创新。

我们不要忘记低端价格区间。2008年，联发科开始提供手机芯片组，使得中国深圳地区的生产商能够以难以想象的速度生产手机。有数据显示，这些生产商的手机占全球手机出货量的三分之一以上，在新兴市场吞食我们的市场份额。

就在竞争对手向我们的市场份额投下火焰的时候，诺基亚呢？我们落后了，我们错失了重要趋势，我们失去了时间。那个时候，我们认为可以做出正确的决定，但实际上，事后发现我们已经落后了几年。

第一款iPhone在2007年进入市场，而我们直到现在都没有一款能够接近iPhone体验的产品。Android进入我们的视野仅仅两年时间，而本周Android已经夺去了我们在智能手机出货量方面的领先地位。难以想象。

在诺基亚内部，我们有一些出色的创新源泉，但我们未能及时将其引入市场。我们认为MeeGo可以成为赢得高端智能手机市场的平台，但以目前的速度，到2011年底，我们也只能向市场推出一款MeeGo产品。

在中端市场，我们还有Symbian，它被证明在北美等成熟市场是不具有竞争力的。另外，Symbian被证明是一个开发人员越来越难以满足用户不断增长的需求的平台，这导致产品开发速度变慢，给我们在探索新硬件平台优势的过程中带来了不利因素。如果我们像过去一样继续下去，那我们必将会进一步落后，而竞争对手则进一步扩大领先优势。

在低端市场，中国的手机生产商正以远远超过我们的速度推出新产品，正如一位诺基亚员工半开玩笑地所说，“就在我们完善PowerPoint介绍的时间里”，他们就可以推出新手机。他们速度非常快，价格非常低，因此对我们构成了挑战。

真正令我们困惑的是我们甚至没有合适的武器来发起反击。我们仍然试图将每部设备的价格达到价格区间。

现在手机之间的竞争已经转变成整个生态系统的争夺，而生态系统不仅仅包括产品的硬件和软件，还包括开发人员、应用程序、电子商务、广告、搜索、社交应用、地理位置服务、统一通信等。我们的竞争对手们并不是依靠设备来夺取我们的市场份额，而是依靠整个生态系统。这意味着，我们必须决定如何打造和开发生态系统，或者加入某个生态系统。

这是我们要做出的决定之一。我们在失去市场份额的同时，还失去了思维，失去了时间。

周二，标准普尔表示有可能把诺基亚信用评级下调至“负面”。这与穆迪上周给予诺基亚的评级类似。这意味着，在接下来几周时间里，他们将对诺基亚进行分析，并决定是否下调诺基亚评级。这何信贷机构考虑做出这种改变？因为他们对我们的竞争力感到担忧。

消费者对诺基亚的青睐也在全球范围内下滑。在英国，我们的品牌喜爱程度下降至20%，比去年下降了8%。也就是说，在英国只有五分之一的消费者对诺基亚的喜爱程度超过其它品牌。而在其它一些诺基亚传统优势市场，诺基亚品牌的喜爱程度也有所下滑，例如俄罗斯、德国、印度尼西亚、阿联酋等等。

我们为何会沦落到这种地步？我们为何会在世界发展过程中落后？

这是我一直希望理解的。我认为，至少其中一部分原因要归结于诺基亚内部。我们自己将石油洒到了燃烧的平台。我认为在目前变革时代下，我们缺乏足够的可信度和领导能力来调整和指导公司。我们有一系列的失误，未能迅速做出创新，内部合作也不够理想。

诺基亚，我们的平台正在燃烧。

我们正在探索前方的道路，一条重新建立市场领先的道路。我们会在2月11日公布新的战略，这将是公司转型的一次重大努力。但我坚信，只要齐心协力，我们可以对领先者发起挑战。只要齐心协力，我们可以将未来掌握在自己手中。

燃烧的平台迫使一个人改变自己的行为，面对不确定的未来走出大胆、勇敢的一步。这样他才能够讲出自己的故事。而现在，我们遇到了同样的机遇。

史蒂芬
Currently rated 4.0 by 1 people

* Currently 4/5 Stars.
* [1]( "Rate this 1 star out of 5")
* [2]( "Rate this 2 stars out of 5")
* [3]( "Rate this 3 stars out of 5")
* [4]( "Rate this 4 stars out of 5")
* [5]( "Rate this 5 stars out of 5")

Tags:

[Mobile & Wireless](http://mobidever.com/category/Mobile-amp3b-Wireless.aspx)

[E-mail](mailto:?subject=诺基亚CEO内部备忘录曝光：承认失败承诺改变&body=Thought you might like this: http://www.lhzhang.org/post/2011/02/e8afbae59fbae4ba9aCEOe58685e983a8e5a487e5bf98e5bd95e69b9de58589efbc9ae689bfe8aea4e5a4b1e8b4a5e689bfe8afbae694b9e58f98.aspx) | [Kick it!](http://www.dotnetkicks.com/submit?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f02%2fe8afbae59fbae4ba9aCEOe58685e983a8e5a487e5bf98e5bd95e69b9de58589efbc9ae689bfe8aea4e5a4b1e8b4a5e689bfe8afbae694b9e58f98.aspx&title=%e8%af%ba%e5%9f%ba%e4%ba%9aCEO%e5%86%85%e9%83%a8%e5%a4%87%e5%bf%98%e5%bd%95%e6%9b%9d%e5%85%89%ef%bc%9a%e6%89%bf%e8%ae%a4%e5%a4%b1%e8%b4%a5%e6%89%bf%e8%af%ba%e6%94%b9%e5%8f%98) | [DZone it!](http://www.dzone.com/links/add.html?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f02%2fe8afbae59fbae4ba9aCEOe58685e983a8e5a487e5bf98e5bd95e69b9de58589efbc9ae689bfe8aea4e5a4b1e8b4a5e689bfe8afbae694b9e58f98.aspx&title=%e8%af%ba%e5%9f%ba%e4%ba%9aCEO%e5%86%85%e9%83%a8%e5%a4%87%e5%bf%98%e5%bd%95%e6%9b%9d%e5%85%89%ef%bc%9a%e6%89%bf%e8%ae%a4%e5%a4%b1%e8%b4%a5%e6%89%bf%e8%af%ba%e6%94%b9%e5%8f%98) | [del.icio.us](http://del.icio.us/post?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f02%2fe8afbae59fbae4ba9aCEOe58685e983a8e5a487e5bf98e5bd95e69b9de58589efbc9ae689bfe8aea4e5a4b1e8b4a5e689bfe8afbae694b9e58f98.aspx&title=%e8%af%ba%e5%9f%ba%e4%ba%9aCEO%e5%86%85%e9%83%a8%e5%a4%87%e5%bf%98%e5%bd%95%e6%9b%9d%e5%85%89%ef%bc%9a%e6%89%bf%e8%ae%a4%e5%a4%b1%e8%b4%a5%e6%89%bf%e8%af%ba%e6%94%b9%e5%8f%98)[Permalink](http://www.lhzhang.org/post.aspx?id=a2b9774f-86f4-4042-8ee0-0a3c0a0ace96) | [Comments (0)](http://mobidever.com/post/2011/02/e8afbae59fbae4ba9aCEOe58685e983a8e5a487e5bf98e5bd95e69b9de58589efbc9ae689bfe8aea4e5a4b1e8b4a5e689bfe8afbae694b9e58f98.aspx#comment) | [Post RSS![RSS comment feed]()](http://mobidever.com/syndication.axd?post=a2b9774f-86f4-4042-8ee0-0a3c0a0ace96)

# [Native Android Applications Including](http://mobidever.com/post/2011/02/Native-Android-Applications-Including.aspx)

by [sundy](http://mobidever.com/author/sundy.aspx) 2/6/2011 5:15:00 AM

Android phones will normally come with a suite ofpreinstalled applications including, but not limited to:

<!--[if !supportLists]-->·        <!--[endif]-->An e-mail client compatible with Gmail but notlimited to it  

<!--[if !supportLists]-->·        <!--[endif]-->An SMS management application 

<!--[if !supportLists]-->·        <!--[endif]-->A full PIM (personal information management)suite including a calendar and contacts list, both   

<!--[if !supportLists]-->o  <!--[endif]-->tightly integrated with Google’s onlineservices5

<!--[if !supportLists]-->o  <!--[endif]-->A fully featured mobile Google Maps applicationincluding StreetView, business ﬁ  nder,driving   

<!--[if !supportLists]-->o  <!--[endif]-->directions, satellite view, and trafﬁ  c conditions

<!--[if !supportLists]-->o  <!--[endif]-->A WebKit-based web browser  

<!--[if !supportLists]-->o  <!--[endif]-->An Instant Messaging Client  

<!--[if !supportLists]-->o  <!--[endif]-->A music player and picture viewer  

<!--[if !supportLists]-->o  <!--[endif]-->The Android Marketplace client for downloadingthied-party Android applications.   

<!--[if !supportLists]-->o  <!--[endif]-->The Amazon MP3 store client for purchasing DRMfree music.  

<!--[if !supportLists]-->o  <!--[endif]-->All the native applications are written in Javausing the Android SDK and are run on Dalvik.

<!--[if !supportLists]-->o  <!--[endif]-->The data stored and used by the nativeapplications — like contact details — are also available to third-

<!--[if !supportLists]-->o  <!--[endif]-->party applications. Similarly, your applicationscan handle events such as an incoming call or a new

<!--[if !supportLists]-->o  <!--[endif]-->SMS message.

<!--[if !supportLists]-->o  <!--[endif]-->The exact makeup of the applications availableon new Android phones is likely to vary based on the

<!--[if !supportLists]-->o  <!--[endif]-->hardware manufacturer and/or the phone carrieror distributor. This is especially true in the United

<!--[if !supportLists]-->o  <!--[endif]-->States, where carriers have signiﬁ  cant inﬂ uence on the software included on shipped devices.
Currently rated 1.5 by 22 people

* Currently 1.5/5 Stars.
* [1]( "Rate this 1 star out of 5")
* [2]( "Rate this 2 stars out of 5")
* [3]( "Rate this 3 stars out of 5")
* [4]( "Rate this 4 stars out of 5")
* [5]( "Rate this 5 stars out of 5")

Tags: [android](http://mobidever.com/?tag=/android)

[Mobile & Wireless](http://mobidever.com/category/Mobile-amp3b-Wireless.aspx)

[E-mail](mailto:?subject=Native Android Applications Including&body=Thought you might like this: http://www.lhzhang.org/post/2011/02/Native-Android-Applications-Including.aspx) | [Kick it!](http://www.dotnetkicks.com/submit?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f02%2fNative-Android-Applications-Including.aspx&title=Native+Android+Applications+Including) | [DZone it!](http://www.dzone.com/links/add.html?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f02%2fNative-Android-Applications-Including.aspx&title=Native+Android+Applications+Including) | [del.icio.us](http://del.icio.us/post?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f02%2fNative-Android-Applications-Including.aspx&title=Native+Android+Applications+Including)[Permalink](http://www.lhzhang.org/post.aspx?id=ba28c802-d86b-4694-8e29-c98f4c7de955) | [Comments (0)](http://mobidever.com/post/2011/02/Native-Android-Applications-Including.aspx#comment) | [Post RSS![RSS comment feed]()](http://mobidever.com/syndication.axd?post=ba28c802-d86b-4694-8e29-c98f4c7de955)
# [Nintendo to launch 3D console in spring](http://mobidever.com/post/2011/01/Nintendo-to-launch-3D-console-in-spring.aspx)

by [sundy](http://mobidever.com/author/sundy.aspx) 1/22/2011 4:54:00 PM

![Nintendo 3ds]()

Nintendo Europe president Satoru Shibata and Jonathan Ross at the 3DS preview event

Stereoscopic [3D](http://www.guardian.co.uk/technology/3d) may finally be poised to shed its image as an expensive curiosity thanks to the imminent arrival of [Nintendo](http://www.guardian.co.uk/technology/nintendo)'s [3DS](http://www.guardian.co.uk/technology/3ds), a [handheld](http://www.guardian.co.uk/technology/handheld)[games](http://www.guardian.co.uk/technology/games) console with a 3D screen that does not require glasses.

At an Amsterdam event today, hosted by Jonathan Ross, Nintendo announced that the 3DS will reach UK shops on 25 March, priced between £219 and £229. That may sound a hefty for a handheld console, but the 3DS has potential to be a game-changer.

As well as running 3D takes on some of the best-loved games franchises, it will take photographs in 3D, run 3D video, let users engage in proximity-based social networking and could bring [augmented reality](http://www.guardian.co.uk/technology/augmented-reality)(AR) to the mainstream. Some industry experts are describing it as a contender for must-have gadget of the year.

Gamers will find its charms hard to resist, despite the high price – partly explained by a 3D screen that is essentially one screen on top of another, and the use of the most powerful graphics chip ever fitted to a Nintendo handheld console.

Nintendo said that, between the launch and June, 25 3DS games will go on sale. These will include favourite franchises such as Pro Evolution Soccer, Nintendogs (this time including cats), Resident Evil, Metal Gear Solid, Driver, Ridge Racer, Splinter Cell, The Legend of Zelda and Super Street Fighter.

Yves Guillemot, the president of the games publisher Ubisoft, admitted that many 3DS games will be pricier than existing [DS](http://www.guardian.co.uk/technology/ds) games (which typically cost £29.99), but not all. "It depends on how big your game is," he said. "For the small ones, they will be the same as the DS. But if you go with lots of video, then the cartridges will need more storage so the games will become more expensive."

Price issues notwithstanding, Guillemot added: "We expect it will sell a lot faster than the DS, because the DS brought a number of new customers to the industry, like young girls who had not played before and older people via games like Dr Kawashima's Brain Training, and they will want the 3DS."

Concerns have been raised that very young children playing on the console could develop eyesight issues due to their sight not being fully formed. David Yarnton, the managing director of Nintendo UK, said: "We recommend that for under-six-year-olds, it's not advisable to play in 3D.

"With any product, when you're looking at a screen on a prolonged basis, you should have a break. And there's a parental lock, so parents can force it to play in 2D."

The console will also come with a number of AR cards which, when photographed, will activate mini-games.

Yarnton believes "it will be our most successful launch ever".
Currently rated 1.2 by 6 people

* Currently 1.166667/5 Stars.
* [1]( "Rate this 1 star out of 5")
* [2]( "Rate this 2 stars out of 5")
* [3]( "Rate this 3 stars out of 5")
* [4]( "Rate this 4 stars out of 5")
* [5]( "Rate this 5 stars out of 5")

Tags:

[Game Life](http://mobidever.com/category/Game-Life.aspx)

[E-mail](mailto:?subject=Nintendo to launch 3D console in spring&body=Thought you might like this: http://www.lhzhang.org/post/2011/01/Nintendo-to-launch-3D-console-in-spring.aspx) | [Kick it!](http://www.dotnetkicks.com/submit?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f01%2fNintendo-to-launch-3D-console-in-spring.aspx&title=Nintendo+to+launch+3D+console+in+spring) | [DZone it!](http://www.dzone.com/links/add.html?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f01%2fNintendo-to-launch-3D-console-in-spring.aspx&title=Nintendo+to+launch+3D+console+in+spring) | [del.icio.us](http://del.icio.us/post?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2011%2f01%2fNintendo-to-launch-3D-console-in-spring.aspx&title=Nintendo+to+launch+3D+console+in+spring)[Permalink](http://www.lhzhang.org/post.aspx?id=b9552149-6605-4671-b322-73d4ef2f6197) | [Comments (0)](http://mobidever.com/post/2011/01/Nintendo-to-launch-3D-console-in-spring.aspx#comment) | [Post RSS![RSS comment feed]()](http://mobidever.com/syndication.axd?post=b9552149-6605-4671-b322-73d4ef2f6197)

# [中国的文明](http://mobidever.com/post/2010/11/e4b8ade59bbde79a84e69687e6988e.aspx)

by [sundy](http://mobidever.com/author/sundy.aspx) 11/24/2010 3:14:00 AM

上海当局用了250个场馆、6个月的时间、7300万人来宣扬“城市让生活更美好”，而1幢大楼、4个小时、53位亡灵证明了这不过是一个口号。 

一个国家的文明程度，不在于能不能办奥运会，不在于能不能办世博会，能不能办亚运会，也不在于能买多少美国垃圾国债，更不在于能去国外几十亿几百亿下订单，而是在于让公民坐在家里不会被烧死、上街摆摊不会被扇耳光，走路不会被李.刚家的宝马车撞，想吃什么都不用担心会有毒。
这个世界就是，不吸烟的得肺癌，不工作的做老板，不爱国的当大官；真正的爱不能要，真正的事不能干，真正的人不能做；需要书的读不起，需要房的买不起，需要人的娶不起；有文化的留不了学，有能力的找不到活，有良知的赚不了钱。 

三聚氰胺害了那么多儿童，最后抓了几个养奶牛的；央视大火烧掉10几亿，抓了几个运烟火的；上海静安大火烧死53人，是4个电焊工的责任！跟西游记一样，有背景的妖怪都被带走了，没背景的妖怪都被乱棍打死 
豹子办了个澡堂子，包给狐狸，狐狸包给松鼠，松鼠雇几只蚂蚁搓澡接客。有一天，狮子去洗澡，掉脸盆里淹死了。。。。虎大王震怒，派警察调查情况，骂了狐狸，打了松鼠，最后，抓了8只蚂蚁。。。。因为他们，竟然没搓澡证！
Currently rated 3.9 by 11 people

* Currently 3.90909/5 Stars.
* [1]( "Rate this 1 star out of 5")
* [2]( "Rate this 2 stars out of 5")
* [3]( "Rate this 3 stars out of 5")
* [4]( "Rate this 4 stars out of 5")
* [5]( "Rate this 5 stars out of 5")

Tags:

[Gossip](http://mobidever.com/category/Gossip.aspx)

[E-mail](mailto:?subject=中国的文明&body=Thought you might like this: http://www.lhzhang.org/post/2010/11/e4b8ade59bbde79a84e69687e6988e.aspx) | [Kick it!](http://www.dotnetkicks.com/submit?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2010%2f11%2fe4b8ade59bbde79a84e69687e6988e.aspx&title=%e4%b8%ad%e5%9b%bd%e7%9a%84%e6%96%87%e6%98%8e) | [DZone it!](http://www.dzone.com/links/add.html?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2010%2f11%2fe4b8ade59bbde79a84e69687e6988e.aspx&title=%e4%b8%ad%e5%9b%bd%e7%9a%84%e6%96%87%e6%98%8e) | [del.icio.us](http://del.icio.us/post?url=http%3a%2f%2fwww.lhzhang.org%2fpost%2f2010%2f11%2fe4b8ade59bbde79a84e69687e6988e.aspx&title=%e4%b8%ad%e5%9b%bd%e7%9a%84%e6%96%87%e6%98%8e)[Permalink](http://www.lhzhang.org/post.aspx?id=fc497200-8a9a-4d8a-bc34-ae98531ab45b) | [Comments (0)](http://mobidever.com/post/2010/11/e4b8ade59bbde79a84e69687e6988e.aspx#comment) | [Post RSS![RSS comment feed]()](http://mobidever.com/syndication.axd?post=fc497200-8a9a-4d8a-bc34-ae98531ab45b)

[<< Previous posts](http://mobidever.com/?page=2)

The opinions expressed herein are my own personal opinions and do not represent my employer's view in anyway.
© Copyright 2007 - 2008 Design by [Sundy Linghua-Zhang 蜀ICP备08108648号](http://www.lhzhang.org/)

# About the author

![Name of author]() Author name
Something about me and what I do.
[E-mail me ![Send mail]()](http://www.lhzhang.org/contact.aspx)

Search
Include comments in search
# Calendar

[<<]()   June 2012   >> Mo Tu We Th Fr Sa Su 28 29 30 31 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 1 2 3 4 5 6 7 8
[View posts in large calendar](http://www.lhzhang.org/calendar/default.aspx)

# Pages

* [about](http://mobidever.com/page/about.aspx)
* [androidtraining](http://mobidever.com/page/androidtraining.aspx)
* [AnyM-C](http://mobidever.com/page/AnyM-C.aspx)
* [AnyM-H](http://mobidever.com/page/AnyM-H.aspx)
* [BMatrix](http://mobidever.com/page/BMatrix.aspx)
* [Books](http://mobidever.com/page/Books.aspx)
* [J2EE基础架构设计培训提纲](http://mobidever.com/page/J2EEe59fbae7a180e69eb6e69e84e8aebee8aea1e59fb9e8aeade68f90e7bab2.aspx)
* [Junior Mobile for HENAN MO](http://mobidever.com/page/Junior-Mobile-for-HENAN-MO.aspx)
* [MobileBackup](http://mobidever.com/page/MobileBackup.aspx)
* [MobileTable](http://mobidever.com/page/MobileTable.aspx)
* [MobileTouch](http://mobidever.com/page/MobileTouch.aspx)
* [My Education](http://mobidever.com/page/My-Education.aspx)
* [My Product](http://mobidever.com/page/My-Product.aspx)
* [Speechparrot](http://mobidever.com/page/Speechparrot.aspx)
* [Tolo](http://mobidever.com/page/Tolo.aspx)
* [zc](http://mobidever.com/page/zc.aspx)
# Recent posts

* [GOAGENT又一个基于GAE的穿越利器](http://mobidever.com/post/2011/08/GOAGENTe58f88e4b880e4b8aae59fbae4ba8eGAEe79a84e7a9bfe8b68ae588a9e599a8.aspx)Rating: 2.6 / 517
* [Learn to play the Les Paul Google Doodle](http://mobidever.com/post/2011/06/Learn-to-play-the-Les-Paul-Google-Doodle.aspx)Rating: 1.7 / 121
* [虽然喜欢这段视频，但拿孩子炒作广告很让人心酸 。](http://mobidever.com/post/2011/06/e899bde784b6e5969ce6aca2e8bf99e6aeb5e8a786e9a291efbc8ce4bd86e68bbfe5ada9e5ad90e78292e4bd9ce5b9bfe5918ae5be88e8aea9e4babae5bf83e985b8-e38082.aspx)Rating: 2.7 / 39
* [我喜欢的Swepy输入法 - for 智能设备](http://mobidever.com/post/2011/06/e68891e5969ce6aca2e79a84Swepye8be93e585a5e6b395---for-e699bae883bde8aebee5a487.aspx)Rating: 1 / 1
* [台湾IT业Android工程师紧俏 年薪50万大陆抢人](http://mobidever.com/post/2011/06/e58fb0e6b9beITe4b89aAndroide5b7a5e7a88be5b888e7b4a7e4bf8f-e5b9b4e896aa50e4b887e5a4a7e99986e68aa2e4baba.aspx)Rating: 2.3 / 54
* [备注一下，简短的自我介绍](http://mobidever.com/post/2011/05/e5a487e6b3a8e4b880e4b88befbc8ce7ae80e79fade79a84e887aae68891e4bb8be7bb8d.aspx)Rating: 5 / 3
* [诺基亚CEO内部备忘录曝光：承认失败承诺改变](http://mobidever.com/post/2011/02/e8afbae59fbae4ba9aCEOe58685e983a8e5a487e5bf98e5bd95e69b9de58589efbc9ae689bfe8aea4e5a4b1e8b4a5e689bfe8afbae694b9e58f98.aspx)Rating: 4 / 1
* [Native Android Applications Including](http://mobidever.com/post/2011/02/Native-Android-Applications-Including.aspx)Rating: 1.5 / 22
* [Nintendo to launch 3D console in spring](http://mobidever.com/post/2011/01/Nintendo-to-launch-3D-console-in-spring.aspx)Rating: 1.2 / 6
* [中国的文明](http://mobidever.com/post/2010/11/e4b8ade59bbde79a84e69687e6988e.aspx)Rating: 3.9 / 11

# Recent comments

* [Struts源码分析1：org.apache.struts.action.Action](http://mobidever.com/post/2009/08/Strutse6ba90e7a081e58886e69e901efbc9aorgapachestrutsactionAction.aspx) (3)
[cash advance](http://cashusloans.com/) wrote: Interesting ... as always - is your blog making an?[[More]](http://mobidever.com/post/2009/08/Strutse6ba90e7a081e58886e69e901efbc9aorgapachestrutsactionAction.aspx#id_5ac8bf0c-225f-41dc-8e66-e3dbb2675036)
* [MySQL on Linux Tutorial [Important Level ,]](http://mobidever.com/post/2008/03/MySQL-on-Linux-Tutorial-Important-Level-2c.aspx) (4)
[cash loans](http://cashusloans.com/) wrote: I just hope to have understood this the way it was?[[More]](http://mobidever.com/post/2008/03/MySQL-on-Linux-Tutorial-Important-Level-2c.aspx#id_b4fcac82-2a2f-42d0-88ae-be33b144e767)
* [JBoss EJB 3.0 Reference Document](http://mobidever.com/post/2008/03/JBoss-EJB-30-Reference-Document.aspx) (6)
[loans](http://cashusloans.com/) wrote: I always wanted to write in my site something like?[[More]](http://mobidever.com/post/2008/03/JBoss-EJB-30-Reference-Document.aspx#id_445ac0c4-1a9a-4cad-b475-9c539e21e856)
* [Here we go again ; why Mono doesn't suck](http://mobidever.com/post/2009/07/Here-we-go-again-ndash3b-why-Mono-doesnrsquo3bt-suck.aspx) (4)
[payday loans](http://cashusloans.com/) wrote: thanks! very helpful post!! like the template btw?[[More]](http://mobidever.com/post/2009/07/Here-we-go-again-ndash3b-why-Mono-doesnrsquo3bt-suck.aspx#id_f31f5244-0b52-4be0-90af-09c1396f3147)
* [C语言的后续 - D语言](http://mobidever.com/post/2009/07/Ce8afade8a880e79a84e5908ee7bbad---De8afade8a880.aspx) (3)
[cash loans](http://cashusloans.com/) wrote: Thank you for your help! [[More]](http://mobidever.com/post/2009/07/Ce8afade8a880e79a84e5908ee7bbad---De8afade8a880.aspx#id_5281e861-e782-403e-8b40-7cd716ddd93b)
* [GRUB Error Messages](http://mobidever.com/post/2008/03/GRUB-Error-Messages.aspx) (2)
[personal loan](http://www.flyingloans.com/) wrote: Found your site on del.icio.us today and really li?[[More]](http://mobidever.com/post/2008/03/GRUB-Error-Messages.aspx#id_d92cea8c-71b0-4218-b21b-60abf51806ee)
* [美国视频浏览量三分之一来自YouTube](http://mobidever.com/post/2008/03/e7be8ee59bbde8a786e9a291e6b58fe8a788e9878fe4b889e58886e4b98be4b880e69da5e887aaYouTube.aspx) (1)
[true blood online episodes](http://www.truebloodonlineepisodes.info/) wrote: Hey - nice blog, just looking around some blogs, s?[[More]](http://mobidever.com/post/2008/03/e7be8ee59bbde8a786e9a291e6b58fe8a788e9878fe4b889e58886e4b98be4b880e69da5e887aaYouTube.aspx#id_42423997-151b-45de-baa6-937dd978ae89)
* [Struts源码分析1：org.apache.struts.action.Action](http://mobidever.com/post/2009/08/Strutse6ba90e7a081e58886e69e901efbc9aorgapachestrutsactionAction.aspx) (3)
[loans](http://www.noteletrackloans.net/) wrote: I like your blog but how do I subscribe? [[More]](http://mobidever.com/post/2009/08/Strutse6ba90e7a081e58886e69e901efbc9aorgapachestrutsactionAction.aspx#id_0913e5bd-842b-4bb7-996a-cc2de9845bfa)
* [C语言的后续 - D语言](http://mobidever.com/post/2009/07/Ce8afade8a880e79a84e5908ee7bbad---De8afade8a880.aspx) (3)
[loans](http://www.noteletrackloans.net/) wrote: This is very detailed and informative article. Tha?[[More]](http://mobidever.com/post/2009/07/Ce8afade8a880e79a84e5908ee7bbad---De8afade8a880.aspx#id_8ffa35a5-6fb6-454c-b1b5-323a3f2e168e)
* [用 Selenium 自动化验收测试 , Robot Testing](http://mobidever.com/post/2008/03/e794a8-Selenium-e887aae58aa8e58c96e9aa8ce694b6e6b58be8af95-efbc88e8bdacefbc89.aspx) (3)
[browse](http://www.7forallskirt.info/) wrote: Interesting post I hope you dont mind if I link to?[[More]](http://mobidever.com/post/2008/03/e794a8-Selenium-e887aae58aa8e58c96e9aa8ce694b6e6b58be8af95-efbc88e8bdacefbc89.aspx#id_7d989075-abe4-41be-b069-616efb06c9d4)
# Archive

* 2011

* [August (1)](http://mobidever.com/2011/08/default.aspx)
* [June (4)](http://mobidever.com/2011/06/default.aspx)
* [May (1)](http://mobidever.com/2011/05/default.aspx)
* [February (2)](http://mobidever.com/2011/02/default.aspx)
* [January (1)](http://mobidever.com/2011/01/default.aspx)
* 2010

* [November (2)](http://mobidever.com/2010/11/default.aspx)
* [October (4)](http://mobidever.com/2010/10/default.aspx)
* [September (8)](http://mobidever.com/2010/09/default.aspx)
* [August (3)](http://mobidever.com/2010/08/default.aspx)
* [June (1)](http://mobidever.com/2010/06/default.aspx)
* [May (3)](http://mobidever.com/2010/05/default.aspx)
* [April (6)](http://mobidever.com/2010/04/default.aspx)
* [March (2)](http://mobidever.com/2010/03/default.aspx)
* [February (1)](http://mobidever.com/2010/02/default.aspx)
* [January (4)](http://mobidever.com/2010/01/default.aspx)
* 2009

* [December (7)](http://mobidever.com/2009/12/default.aspx)
* [November (9)](http://mobidever.com/2009/11/default.aspx)
* [October (9)](http://mobidever.com/2009/10/default.aspx)
* [September (4)](http://mobidever.com/2009/09/default.aspx)
* [August (13)](http://mobidever.com/2009/08/default.aspx)
* [July (9)](http://mobidever.com/2009/07/default.aspx)
* [June (8)](http://mobidever.com/2009/06/default.aspx)
* [May (1)](http://mobidever.com/2009/05/default.aspx)
* [March (1)](http://mobidever.com/2009/03/default.aspx)
* [February (2)](http://mobidever.com/2009/02/default.aspx)
* [January (3)](http://mobidever.com/2009/01/default.aspx)
* 2008

* [December (7)](http://mobidever.com/2008/12/default.aspx)
* [November (15)](http://mobidever.com/2008/11/default.aspx)
* [October (14)](http://mobidever.com/2008/10/default.aspx)
* [September (6)](http://mobidever.com/2008/09/default.aspx)
* [August (3)](http://mobidever.com/2008/08/default.aspx)
* [July (8)](http://mobidever.com/2008/07/default.aspx)
* [June (2)](http://mobidever.com/2008/06/default.aspx)
* [May (6)](http://mobidever.com/2008/05/default.aspx)
* [April (9)](http://mobidever.com/2008/04/default.aspx)
* [March (31)](http://mobidever.com/2008/03/default.aspx)

# Authors

* [![RSS feed for sundy]()](http://mobidever.com/syndication.axd?author=sundy)[sundy (210)](http://mobidever.com/author/sundy.aspx "Author: sundy")
# Tags

* [adobe](http://mobidever.com/?tag=/adobe "Tag: adobe")
* [android](http://mobidever.com/?tag=/android "Tag: android")
* [arm](http://mobidever.com/?tag=/arm "Tag: arm")
* [blog](http://mobidever.com/?tag=/blog "Tag: blog")
* [blogengine](http://mobidever.com/?tag=/blogengine "Tag: blogengine")
* [borland](http://mobidever.com/?tag=/borland "Tag: borland")
* [codegear](http://mobidever.com/?tag=/codegear "Tag: codegear")
* [deklarit](http://mobidever.com/?tag=/deklarit "Tag: deklarit")
* [edison](http://mobidever.com/?tag=/edison "Tag: edison")
* [ejb](http://mobidever.com/?tag=/ejb "Tag: ejb")
* [extension](http://mobidever.com/?tag=/extension "Tag: extension")
* [falsh](http://mobidever.com/?tag=/falsh "Tag: falsh")
* [fms](http://mobidever.com/?tag=/fms "Tag: fms")
* [funny](http://mobidever.com/?tag=/funny "Tag: funny")
* [grub](http://mobidever.com/?tag=/grub "Tag: grub")
* [jboss](http://mobidever.com/?tag=/jboss "Tag: jboss")
* [jdbc](http://mobidever.com/?tag=/jdbc "Tag: jdbc")
* [joit](http://mobidever.com/?tag=/joit "Tag: joit")
* [kfo](http://mobidever.com/?tag=/kfo "Tag: kfo")
* [linux](http://mobidever.com/?tag=/linux "Tag: linux")
* [ndsl](http://mobidever.com/?tag=/ndsl "Tag: ndsl")
* [netbeans](http://mobidever.com/?tag=/netbeans "Tag: netbeans")
* [orm](http://mobidever.com/?tag=/orm "Tag: orm")
* [ps3](http://mobidever.com/?tag=/ps3 "Tag: ps3")
* [seam](http://mobidever.com/?tag=/seam "Tag: seam")
* [silverlight](http://mobidever.com/?tag=/silverlight "Tag: silverlight")
* [software factory](http://mobidever.com/?tag=/software+factory "Tag: software factory")
* [sqlserver 2005](http://mobidever.com/?tag=/sqlserver+2005 "Tag: sqlserver 2005")
* [svn](http://mobidever.com/?tag=/svn "Tag: svn")
* [symbian](http://mobidever.com/?tag=/symbian "Tag: symbian")
* [testing](http://mobidever.com/?tag=/testing "Tag: testing")
* [welcome](http://mobidever.com/?tag=/welcome "Tag: welcome")
* [云计算](http://mobidever.com/?tag=/%e4%ba%91%e8%ae%a1%e7%ae%97 "Tag: 云计算")
* [教育](http://mobidever.com/?tag=/%e6%95%99%e8%82%b2 "Tag: 教育")
* [求职](http://mobidever.com/?tag=/%e6%b1%82%e8%81%8c "Tag: 求职")
* [自己的培训](http://mobidever.com/?tag=/%e8%87%aa%e5%b7%b1%e7%9a%84%e5%9f%b9%e8%ae%ad "Tag: 自己的培训")
* [软件工厂](http://mobidever.com/?tag=/%e8%bd%af%e4%bb%b6%e5%b7%a5%e5%8e%82 "Tag: 软件工厂")

# Categories

* [![RSS feed for C/C++/Embedded]()](http://mobidever.com/syndication.axd?category=c4ba8cdb-6db9-45b3-bcfb-f7985ca10734)[C/C++/Embedded (7)](http://mobidever.com/category/CC2b2bEmbedded.aspx "Category: C/C++/Embedded")
* [![RSS feed for Education & Consultation]()](http://mobidever.com/syndication.axd?category=856e816b-7248-4bfd-a798-dca590ded853)[Education & Consultation (16)](http://mobidever.com/category/Education--Consultation.aspx "Category: Education & Consultation")
* [![RSS feed for Electronic Commerce]()](http://mobidever.com/syndication.axd?category=03693fe8-39b2-49ee-8bfc-2a6dd9f88cee)[Electronic Commerce (19)](http://mobidever.com/category/Electronic-Commerce.aspx "Category: Electronic Commerce")
* [![RSS feed for Flash,SilverLight Coming]()](http://mobidever.com/syndication.axd?category=25767fbd-3b56-4149-acb8-0f3456414e82)[Flash,SilverLight Coming (5)](http://mobidever.com/category/Flash2cSilverLight-Coming.aspx "Category: Flash,SilverLight Coming")
* [![RSS feed for Game Life]()](http://mobidever.com/syndication.axd?category=7881ec5a-0928-4d34-b3d5-22e80b95f595)[Game Life (18)](http://mobidever.com/category/Game-Life.aspx "Category: Game Life")
* [![RSS feed for Gossip]()](http://mobidever.com/syndication.axd?category=01f734c8-d56b-465e-9c41-a7df8435e22a)[Gossip (63)](http://mobidever.com/category/Gossip.aspx "Category: Gossip")
* [![RSS feed for Internet Communion]()](http://mobidever.com/syndication.axd?category=2afd29cc-af6b-48c3-b7fe-bf78ab603f7b)[Internet Communion (8)](http://mobidever.com/category/Internet-Communion.aspx "Category: Internet Communion")
* [![RSS feed for Java & OpenSource Domain]()](http://mobidever.com/syndication.axd?category=ac8b7eb2-c845-48b3-95b1-fb869ea88c93)[Java & OpenSource Domain (27)](http://mobidever.com/category/Java--OpenSource-Domain.aspx "Category: Java & OpenSource Domain")
* [![RSS feed for Kendo]()](http://mobidever.com/syndication.axd?category=15067de6-e26e-484b-922f-181c15c8980b)[Kendo (1)](http://mobidever.com/category/Kendo.aspx "Category: Kendo")
* [![RSS feed for Microsoft Domain]()](http://mobidever.com/syndication.axd?category=cb318ab3-4d71-4f86-9a88-d5e83746ac39)[Microsoft Domain (21)](http://mobidever.com/category/Microsoft-Domain.aspx "Category: Microsoft Domain")
* [![RSS feed for Mobile & Wireless]()](http://mobidever.com/syndication.axd?category=850ee323-9a47-4400-be36-020ac21c9cb0)[Mobile & Wireless (21)](http://mobidever.com/category/Mobile--Wireless.aspx "Category: Mobile & Wireless")
* [![RSS feed for Newscaster]()](http://mobidever.com/syndication.axd?category=cc29a5bd-406a-4255-88cd-102f28a304d7)[Newscaster (55)](http://mobidever.com/category/Newscaster.aspx "Category: Newscaster")
* [![RSS feed for Software's industrialization]()](http://mobidever.com/syndication.axd?category=3f28c360-60c5-40a8-8ccd-f1aa2bdd0c2c)[Software's industrialization (16)](http://mobidever.com/category/Softwares-industrialization.aspx "Category: Software's industrialization")
* [![RSS feed for Speech & Artificial Intelligence]()](http://mobidever.com/syndication.axd?category=26403abb-2974-4bf4-a557-20561fc1239d)[Speech & Artificial Intelligence (1)](http://mobidever.com/category/Speech--Artificial-Intelligence.aspx "Category: Speech & Artificial Intelligence")
# Blogroll

* [![RSS feed for .NET slave]()](http://feeds.feedburner.com/netslave)[.NET slave](http://www.madskristensen.dk/)
* [![RSS feed for Al Nyveldt]()](http://feeds.feedburner.com/razorant)[Al Nyveldt](http://www.nyveldt.com/blog/)
* [![RSS feed for Ruslan Tur]()](http://feeds.feedburner.com/rtur)[Ruslan Tur](http://rtur.net/blog/)
* [![RSS feed for My Space Live Blog]()](http://sundyzlh.spaces.live.com/)[My Space Live Blog](http://sundyzlh.spaces.live.com/)
* [![RSS feed for DoteyDog' Blog----王瑜]()](http://www.doteydog.net/syndication.axd)[DoteyDog' Blog----王瑜](http://www.doteydog.net/)
* [![RSS feed for 李开复博客]()](http://blog.sina.com.cn/rss/1197161814.xml)[李开复博客](http://blog.sina.com.cn/kaifulee)
* [![RSS feed for veryls的电子商务物语]()](http://veryls.blog.sohu.com/)[veryls的电子商务物语](http://veryls.blog.sohu.com/)
* [![RSS feed for Renmeiti]()](http://www.renmeiti.com/)[Renmeiti](http://www.renmeiti.com/)
[Download OPML file ![OPML]()](http://www.lhzhang.org/opml.axd "Download OPML file")

# Disclaimer

The opinions expressed herein are my own personal opinions and do not represent my employer's view in anyway.
© Copyright 2012
[Sign in]()
