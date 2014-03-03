---
layout: post
title: "[BlackHat专题]PwnieAwards 2012奖项提名- FreebuF.COM"
categories: 安全
tags: 
 - 安全
 - 无线
--- 

# [BlackHat专题]PwnieAwards 2012奖项提名- FreebuF.COM

* [首页](http://www.freebuf.com/)
* [资讯](http://www.freebuf.com/category/news)
* [工具](http://www.freebuf.com/category/tools)
* [文章](http://www.freebuf.com/category/articles)

* [WEB安全](http://www.freebuf.com/category/articles/web)
* [无线安全](http://www.freebuf.com/category/articles/wireless)
* [系统安全](http://www.freebuf.com/category/articles/system)
* [数据库安全](http://www.freebuf.com/category/articles/database)
* [终端安全](http://www.freebuf.com/category/articles/terminal)
* [其他](http://www.freebuf.com/category/articles/others-articles)
* [漏洞](http://www.freebuf.com/category/vuls)
* [招聘](http://www.freebuf.com/category/jobs)
* [周边](http://www.freebuf.com/category/others)
* [聚合](http://plant.freebuf.com/)
* [社区](http://club.freebuf.com/)
* ![]()[投稿](http://www.freebuf.com/newsend)

[登陆](http://www.freebuf.com/wp-login.php?redirect_to=http%3A%2F%2Fwww.freebuf.com%2Fnews%2Fblackhat-news%2F5088.html "登陆")[注册](http://www.freebuf.com/wp-login.php?action=register)

[![freebuf]()](http://www.freebuf.com/)

[新浪微博](http://www.weibo.com/freebuf "收听新浪微博") [腾讯微博](http://t.qq.com/freebuf "收听腾讯微博")[订阅我们](http://www.freebuf.com/feed "订阅我们")

### [BlackHat专题]PwnieAwards 2012奖项提名

[](http://www.freebuf.com/news/blackhat-news/5088.html#)[jly@12](http://www.freebuf.com/author/jly12 "由 jly@12 发布") @ [Black Hat专题](http://www.freebuf.com/category/news/blackhat-news "查看 Black Hat专题 中的全部文章") 2012-07-25 共 **2441** 人围观 

# 第六届[The Pwnie Award](http://pwnies.com/)将在[BlackHat USA](http://www.blackhat.com/usa/)会议上公布最终结果。

In the 2012 edition 奖项共9大类:

Pwnie for Best Server-Side Bug 最佳服务端漏洞 Pwnie for Best Client-Side Bug 最佳客户端漏洞 Pwnie for Best Privilege Escalation Bug 最佳提权漏洞 Pwnie for Most Innovative Research 最具创新价值研究 Pwnie for Lamest Vendor Response 最烂回应厂商 Pwnie for Best Song 最佳歌曲 Pwnie for Most Epic FAIL 最测漏失败奖 Pwnie for Epic Ownage 最霸气成功奖

**获得提名的有以下漏洞：**

### Pwnie for Best Server-Side Bug 最佳服务端漏洞

纵观所有奖项，我的票投给了WordPress Timthumb 插件 ‘timthumb’ 缓存目录任意文件上传漏洞，因为这个漏洞的传播及最终影响面非常之大。

**oracle Database Server ‘TNS Listener’远程数据投毒漏洞 (CVE-2012-1675)**
Oracle Database Server在实现上存在可允许攻击者向远程“TNS Listener”组件处理的数据投毒的漏洞。攻击者可利用此漏洞将数据库服务器的合法“TNS Listener”组件中的数据转向到攻击者控制的系统，导致控制远程组件的数据库实例，造成组件和合法数据库之间的攻击者攻击、会话劫持或拒绝服务攻击。
此漏洞由Joxean Koret在2008年发现，甲骨文公司在4月发布声明称该漏洞已修复，但是Joxean Koret随后释放的漏洞细节，证明该漏洞并未修复。

**ProFTPD响应池释放后重用代码执行漏洞 (**[**CVE-2011-4130**](http://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2011-4130)**)**
ProFTPD 1.3.3g之前版本的Response API中存在释放后使用漏洞。远程认证用户可借助包含FTP数据传输错误的向量执行任意代码。

**Mysql认证绕过漏洞(**[**CVE-2012-2122**](http://eromang.zataz.com/2012/06/12/cve-2012-2122-oracle-mysql-authentication-bypass-password-dump-metasploit-demo/)**)**
MariaDB 5.1.62, 5.2.12、5.3.6、5.5.23之前版本和MySQL 5.1.63、5.5.24、5.6.6之前版本在用户验证的处理上存在安全漏洞，可能导致攻击者无需知道正确口令就能登录到MySQL服务器。
用户连接到MariaDB/MySQL后，应用会计算和比较令牌值，由于错误的转换，即使memcmp()返回非零值，也可能出现错误的比较，造成MySQL/MariaDB误认为密码是正确的，因为协议使用的是随机字符串，该Bug发生的几率为1/256。MySQL的版本是否受影响取决于程序的编译方式，很多版本（包括官方提供的二进制文件）并不受此漏洞的影响。
该漏洞相信大家已经很熟悉了，**之前freebuf****也详细报道过**，详见[地址](http://www.freebuf.com/vuls/3815.html)

**WordPress缩略图插件Timthumb缓存目录任意文件上传漏洞(CVE-2011-4106)**
WordPress Timthumb提供的Timthumb.php不正确处理$allowedSites变量，提交特殊的主机名可上传和执行timthumb缓存目录中的任意PHP代码。该漏洞是2011年8月份左右披露出来的，当时影响面非常广，几百万的wp站点被黑，而利用起来也非常简单。在第三方网站上写上一句话木马，然后提交的url里面包含该地址就可以了，成功利用之后会将该木马下载到缓存目录。

**Pinkie Pie’s Pwnium Exploit**

在2012年的3月份的Pwnium黑客大赛结束前的几个小时，一位名叫Pinkie Pie的小伙子赢得了为发现Chrome漏洞而设立的60000美金的奖金。他一口气利用三个不同的0day写出的exp成功打破了Chrome Sandbox的神话。在这之后，谷歌几乎立即发布了Chrome稳定更新版以解决这一问题。 据Wired报道，发现Chrome漏洞的Pinkie Pie并不想透露自己的名字，因为他的老板并不支持他的这一活动。

**Sergey Glazunov’s Pwnium Exploit (**[**CVE-2011-3046**](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2011-3046)**)**
Sergey Glazunov也是在2012年的Pwnium黑客大赛中发现了chrome的至少14个bug，可以成功绕过沙盒机制来执行任意代码。

[](http://blog.chromium.org/2012/06/tale-of-two-pwnies-part-2.html)

**MS11-087: 未知win32k.sys驱动TrueType字体解析引擎漏洞(CVE 2011-3402)**
CVE 2011-3402，2011年12月MS11-087中已提供了补丁，Duqu恶意软件广泛地利用此漏洞。
 
**Flash BitmapData.histogram()信息泄露 (CVE 2012-0769)**
Google安全小组Fermin J. Serna 发现了CVE 2012-0769漏洞，并在APSB12-05中修复了此漏洞。
 
**iOS绕过代码签名漏洞(CVE 2011-3442)**
Accuvant实验室的Charlie Miller发现了该漏洞，并在iOS 5.0.1版本中修复了此漏洞。
该漏洞是一个绕过代码签名漏洞，第三方可在动态加载，执行代码及流氓软件中利用此漏洞。
 
**Pwnie for Best Privilege Escalation Bug****最佳提权漏洞**

我将投票给“Xen Intel x64 SYSRET指令权限提升漏洞”，因为该漏洞同时敲响了产品和供应商的警钟。
 
**Xen Intel x64 SYSRET指令权限提升漏洞(CVE-2012-0217)**
Rafal Wojtczuk发现了此漏洞并报给了厂商。成功地验证了[fail0verflow](http://fail0verflow.com/blog/2012/cve-2012-0217-intel-sysret-freebsd.html)在2012-07-05日提供的该漏洞。
点击这儿是该攻击的验证视频。
 
**iOS HFS目录文件整型溢出漏洞(**[**CVE-2012-0642**](http://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0642)**)**
pod2g发现了此漏洞，在Absinthe iOS 5.0/5.0.1存在此漏洞。
 
**MS11-098: Windows内核异常处理漏洞(**[**CVE-2011-2018**](http://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2011-2018)**)**
 CVE-2011-2018, [MS11-098](http://technet.microsoft.com/en-us/security/bulletin/ms11-098)中已提供了补丁，Mateusz “j00ru” Jurczyk发现了此漏洞并报告给厂商。

**VMware高带宽后门ROM重写权限提升漏洞(**[**CVE-2012-1515**](http://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-1515)**)**
[CVE-2012-1515](http://technet.microsoft.com/en-us/security/bulletin/ms12-042), MS12-042和VMSA-2012-0006.2中已提供补丁，Derek Soeder发现了此漏洞并报告给厂商。

[](http://technet.microsoft.com/en-us/security/bulletin/ms12-042)

### ![]() **版权声明：转载请注明出自:** [FreebuF.COM](http://www.freebuf.com/)

* [赞: 3]()
* [踩: 0]()
您已评价！

  分享到： [新浪微博](http://v.t.sina.com.cn/share/share.php?url=http://www.freebuf.com/news/blackhat-news/5088.html&title=[BlackHat%E4%B8%93%E9%A2%98]PwnieAwards%202012%E5%A5%96%E9%A1%B9%E6%8F%90%E5%90%8D "分享到新浪微博") [腾讯微博](http://v.t.qq.com/share/share.php?url=http://www.freebuf.com/news/blackhat-news/5088.html&title=[BlackHat%E4%B8%93%E9%A2%98]PwnieAwards%202012%E5%A5%96%E9%A1%B9%E6%8F%90%E5%90%8D "分享到腾讯微博") [豆瓣]( "分享到豆瓣") [人人网](http://share.renren.com/share/buttonshare.do?link=http://www.freebuf.com/news/blackhat-news/5088.html&title=[BlackHat%E4%B8%93%E9%A2%98]PwnieAwards%202012%E5%A5%96%E9%A1%B9%E6%8F%90%E5%90%8D "分享到人人网") [开心网](http://www.kaixin001.com/repaste/share.php?rtitle=[BlackHat%E4%B8%93%E9%A2%98]PwnieAwards%202012%E5%A5%96%E9%A1%B9%E6%8F%90%E5%90%8D&rurl=http://www.freebuf.com/news/blackhat-news/5088.html%20title=) [QZone](http://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?url=http://www.freebuf.com/news/blackhat-news/5088.html&title=[BlackHat%E4%B8%93%E9%A2%98]PwnieAwards%202012%E5%A5%96%E9%A1%B9%E6%8F%90%E5%90%8D "分享到QQ空间") [facebook](http://facebook.com/share.php?u=http://www.freebuf.com/news/blackhat-news/5088.html&title=[BlackHat%E4%B8%93%E9%A2%98]PwnieAwards%202012%E5%A5%96%E9%A1%B9%E6%8F%90%E5%90%8D "分享到facebook") [twitter](http://twitter.com/share?url==http://www.freebuf.com/news/blackhat-news/5088.html&title=[BlackHat%E4%B8%93%E9%A2%98]PwnieAwards%202012%E5%A5%96%E9%A1%B9%E6%8F%90%E5%90%8D "分享到twitter")

### 相关文章

* [BlackHat大会后的WLAN统计报告](http://www.freebuf.com/news/blackhat-news/5266.html)

**4**条评论 **1052**人已围观
* [[BlackHat2012工具]Google Hacking Diggity工具集](http://www.freebuf.com/tools/5246.html)

**4**条评论 **1646**人已围观
* [传P0sixninja在黑帽大会上出售了越狱漏洞](http://www.freebuf.com/news/blackhat-news/5233.html)

**1**条评论 **1236**人已围观
* [GPS缺陷可让黑客跟踪智能手机用户，甚至完全接管移动设备](http://www.freebuf.com/news/blackhat-news/5149.html)

**1**条评论 **1310**人已围观
* [[7月27日]BlackHat精彩内涵图集（每日更新）](http://www.freebuf.com/news/blackhat-news/5097.html)

**10**条评论 **3116**人已围观
﻿

## 4 Comments [发表评论](http://www.freebuf.com/news/blackhat-news/5088.html#respond)

1. ![]() phper [](http://www.freebuf.com/news/blackhat-news/5088.html#comment-1532) 2012-07-25

最测漏失败奖。。神级翻译。。
[支持]()(3)[反对]() (0)
[回复](http://www.freebuf.com/news/blackhat-news/5088.html?replytocom=1532#respond)
1. ![]() purpose [](http://www.freebuf.com/news/blackhat-news/5088.html#comment-1534) 2012-07-25

去年靠Timthumb这个漏洞收获了不少鸡 ![:lol:]()
[支持]()(1)[反对]() (0)
[回复](http://www.freebuf.com/news/blackhat-news/5088.html?replytocom=1534#respond)
1. [![]()](http://www.freebuf.com/?author=4) [thanks ](http://www.freebuf.com/?author=4)![]( "核心成员") (6级) 手提酱油瓶从天而降 [](http://www.freebuf.com/news/blackhat-news/5088.html#comment-1536) 2012-07-25

![:cool:]() jly@12翻译辛苦了
[支持]()(1)[反对]() (0)
[回复](http://www.freebuf.com/news/blackhat-news/5088.html?replytocom=1536#respond)
1. [![]()](http://www.freebuf.com/?author=131) [Crow ](http://www.freebuf.com/?author=131)![]( "核心成员") (5级)  [](http://www.freebuf.com/news/blackhat-news/5088.html#comment-1549) 2012-07-26

Pwnie for Most Epic FAIL 最测漏失败奖 ![:mrgreen:]()
[支持]()(0)[反对]() (0)
[回复](http://www.freebuf.com/news/blackhat-news/5088.html?replytocom=1549#respond)
昵称
(必须)您当前尚未登录。[登陆](http://www.freebuf.com/wp-login.php?redirect_to=http%3A%2F%2Fwww.freebuf.com%2Fnews%2Fblackhat-news%2F5088.html "登陆")？注册

邮箱
必须(保密)
网址

发表言论前，请滑动滚动条解锁
[表情]()[插代码]()[粗体]()[斜体]()[删除线]()[下划线]()[引用]()[链接]()[插图]()

[![]()]( "mrgreen")[![]()]( "razz")[![]()]( "sad")[![]()]( "smile")[![]()]( "oops")[![]()]( "grin")[![]()]( "eek")[![]()]( "???")[![]()]( "cool")[![]()]( "lol")[![]()]( "mad")[![]()]( "twisted")[![]()]( "roll")[![]()]( "wink")[![]()]( "idea")[![]()]( "arrow")[![]()]( "neutral")[![]()]( "cry")[![]()]( "?")[![]()]( "evil")[![]()]( "shock")[![]()]( "!")
正在提交, 请稍候...

#

[取消]()   有人回复时邮件通知我

### **热门评论****Hot Comments**

[phper](http://www.freebuf.com/news/blackhat-news/5088.html#comment-1532) @ 2012-07-25 17:29:18

最测漏失败奖。。神级翻译。。
[支持]() (3) [反对]() (0) [purpose](http://www.freebuf.com/news/blackhat-news/5088.html#comment-1534) @ 2012-07-25 17:34:07

去年靠Timthumb这个漏洞收获了不少鸡 ![:lol:]()
[支持]() (1) [反对]() (0) [thanks](http://www.freebuf.com/news/blackhat-news/5088.html#comment-1536) @ 2012-07-25 19:17:12

![:cool:]() jly@12翻译辛苦了
[支持]() (1) [反对]() (0)

### **最新发布****New Posts**

* [QQ漂流瓶飘来的可能是愿望，也有可能是XSS！](http://www.freebuf.com/articles/web/5373.html "QQ漂流瓶飘来的可能是愿望，也有可能是XSS！")
* [卡巴斯基寻民间牛人帮助破解高斯(Gauss)神秘Payload](http://www.freebuf.com/news/5378.html "卡巴斯基寻民间牛人帮助破解高斯(Gauss)神秘Payload")
* [盲注（Blind SQL Injection）工具—BBQSQL v1.0.0](http://www.freebuf.com/tools/5369.html "盲注（Blind SQL Injection）工具—BBQSQL v1.0.0")
* [计算机内存在关机后仍会泄露信息](http://www.freebuf.com/news/5364.html "计算机内存在关机后仍会泄露信息")
* [34款Firefox渗透测试插件](http://www.freebuf.com/tools/5361.html "34款Firefox渗透测试插件")
* [BackTrack5 R3发布](http://www.freebuf.com/tools/5363.html "BackTrack5 R3发布")
* [浅析无线网络数据窥探技术](http://www.freebuf.com/articles/wireless/5351.html "浅析无线网络数据窥探技术")
* [性骚扰是黑客文化的一部分？](http://www.freebuf.com/news/5358.html "性骚扰是黑客文化的一部分？")
* [freebuf安全周刊8月6日-8月12日](http://www.freebuf.com/others/5357.html "freebuf安全周刊8月6日-8月12日")
* [一个“神奇”的工具：把 JavaScript 代码转为 ()[]{}!+ 字符](http://www.freebuf.com/tools/5352.html "一个“神奇”的工具：把 JavaScript 代码转为 ()[]{}!+ 字符")
All of the material and the informations available on freebuf. com is for informative purpose. freebuf.com declines all responsibilities from the damage that may be done of it, consequently it does not encourage who might use it for illegal purposes.

### FREEBUF.COM

[免责声明](http://www.freebuf.com/dis) [关于我们](http://www.freebuf.com/others/864.html) [建议反馈](mailto:freebuf@gmail.com) [联系我们](http://www.freebuf.com/contact) [我要投稿](http://www.freebuf.com/newsend)

Copyright @ 2012 WWW.FREEBUF.COM All Rights Reserved []()

FreebuF.COM

用户名

密码

Remember Me

[Register](http://www.freebuf.com/wp-login.php?action=register) | [Lost your password?](http://www.freebuf.com/wp-login.php?action=lostpassword "Password Lost and Found")
Register

用户名

邮箱

密码(至少6位)

验证码:
看不清？[点击更换]()

![看不清?点击更换]( "看不清?点击更换")

[登陆](http://www.freebuf.com/wp-login.php) | [忘记密码？](http://www.freebuf.com/wp-login.php?action=lostpassword "Password Lost and Found")
Reset Password

Username or E-mail:

[登陆](http://www.freebuf.com/wp-login.php)| [注册](http://www.freebuf.com/wp-login.php?action=register)
[](http://www.freebuf.com/news/blackhat-news/5088.html#prev "previous")[](http://www.freebuf.com/news/blackhat-news/5088.html#next "next")

prev
next
