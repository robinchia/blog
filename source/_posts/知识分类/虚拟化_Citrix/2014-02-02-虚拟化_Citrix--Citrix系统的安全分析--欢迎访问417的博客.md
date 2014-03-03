---
layout: post
title: "Citrix 系统的安全分析--欢迎访问417的博客"
categories: 虚拟化_Citrix
tags: 
 - 虚拟化_Citrix
--- 

# Citrix 系统的安全分析--欢迎访问417的博客

[首页](http://www.bokee.com/) | [博客群](http://group.bokee.com/) | [公社](http://blogs.bokee.com/) | [专栏](http://column.bokee.com/) | [论坛](http://bbs.bokee.com/) | [图片](http://photo.bokee.com/) | [资讯](http://news.bokee.com/) | [注册](http://reg.bokee.com/account/web/register.jsp) | [帮助](http://help.bokee.com:8086/help/index.html) | [博客联播](http://lianbo.booso.com/) | [随机访问](http://ping.bokee.com:81/memcm/random.b)

# [欢迎访问417的博客](http://www417.bokee.com/index.html)

[特别的一晚](http://www417.bokee.com/5962997.html "上一篇")- -| [回首页](http://www417.bokee.com/index.html) | [2007年索引](http://www417.bokee.com/catalog_2007.html) | - -[Pic-2](http://www417.bokee.com/6103623.html "下一篇")
## Citrix 系统的安全分析

**关键词**： [Citrix](http://tag.bokee.com/tag/Citrix)                                          

// 很久以前翻译的一篇文章，2004年发给黑防了。

编者按：虚拟工作平台已开始的从单纯的透过网页传播信息，变化到利用Internet、Interanet、Extranet的基础建设来达到合作、交易及知识管理的目的。这项新的网页环境最基本需求之一是安全的配置、重要的营业应用软件。不相容的整合导致无法即时分享重要的信息，造成时间、金钱及附加价值的损失。这是为什么成千上万个大型企业团体选择在全球具有领导地位的Citrix的主要原因。下文介绍的是Citrix中不可忽略的一个因素——安全！

Citrix 系统的安全分析
原作者/Brian Madden
翻译/417
一、介绍
Citrix系统是一款广泛流行的远程桌面控制程序，类似于 Microsoft 的 Terminal Services，但是原理不同，Microsoft Terminal Services 使用的是 RDP(Remote Desktop Protocol) 协议，而 Citrix 使用的是 ICA (Independent Computing Architecture)协议。(实际上 Microsoft 买断了 Citrix 的另一项技术 Citrix MultiWin，并使它可以与 Windows 同内核工作，以支持多用户并行会话，在 Windows 2000 操作系统中才被正式命名为RDP。Citrix 的产品和技术已被全球超过2000万的用户所采纳，其中包括世界财富100强的所有企业和公司，世界财富500强的企业中亦有85%选用 Citrix 的产品。Citrix 的客户包括 MCI Worldcom、Qantas、朗讯、北方电信、摩托罗拉、诺基亚、香港电信、JPMorgan、雀巢公司、壳牌集团、UCLA（加利福尼亚大学洛杉矶分校）、香港大学等等——译者注)
在本文中，我将简要介绍Citrix是怎样工作的，和如何更好的配置Citrix使用者权限。
要声明的是，作者是Citrix MetaFrame XP的高级技术顾问之一，但并不是Citrix的管理员，以下有些想法并不完美，不过本文的重点并不是这些，请根据具体情况。如果有错误可以联系 [wirepair@roguemail.net](mailto:wirepair@roguemail.net)。
二、Citrix 的工作方式
我列在这里的是几种Citrix可能被用到的解决方式：
1、Citrix MetaFrame
Citrix MetaFrame 有三个不同的版本：XPs，XPa及 XPe。分别适合不同的环境使用。其中 XPe 是完全安全版，包括一些不同于其他版本的管理选项。XPa 和 XPe 则稍微少一点。在此篇文章中，我们只讨论 XPe，但是其中的大部分功能都能在其他版本上应用。Citrix 默认使用 1494 端口并且只和使用了 Citrix ICA 加密协议的客户端通信。
2、Citrix NFuse/Citrix 安全网关
Citrix NFuse 允许管理员锁定程序而只能通过 Web 浏览器通信。Citrix NFuse 默认是安装的 IIS 5.0 上的，但是在这篇文章中，我们将试着把它安装在 Apache 上。当然也许读者对在 IIS 5.0 上的安装/配置/管理 Citrix NFuse 更感性趣，我们稍后会涉及到这个问题。下面谈到的应用若没有特别指出则全部使用 SSL 128 位加密。
Citrix NFuse 默认安装情况下的远程权限规则允许管理员执行 Citrix 安全网关。如果管理员适当的配置了 NFuse，远程用户将则不能直接通过 Citrix Server。所有通过 Citrix NFuse 服务器和安全网关的连接将被过滤。如图1所示：

![ ]()

图1
三、以攻击者的角度思考问题
根据简图，我们发现使用者不能获得一个和 Citrix Server 直接的连接，但是可以通过 DMZ 到 Internal network。
在默认安装的 NFuse 并且没有安全网关配合的情况下，使用者接触到 NFuse Web Server,之后一旦发出了请求并确认，使用者将获得一个直接连接 Citrix Server 的通道，你可以看出这个有个问题，恶意使用者可以在和 Citrix Server 连接好的情况下，再进行先前的请求扫描，通过搜集请求列表之后建立自己的 .ICA 文件，里面包含了他们指定的信息。这就意味着，如果当前的情况是在网络边界，就有两个漏洞可以通过防火墙。一个是 IIS，一个是 Citrix。
.ICA 文件是一种基于文本结构的文档，包含所有的配置信息。这个文件一般是给最终用户使用，在安装完 Citrix 客户端以后，双击这个文件就可以自动连接 .ICA 文件中指定的服务器。
如果 NFuse 被使用的情况下通常就不需要 .ICA 文件了。假设 A 用户打开 Web 浏览器来到 NFuse Web 服务器，将使用他在 NT 域中的账号。如果成功登陆，A 用户将得到一张他 Citrix Farm 上有权使用的程序列表。
有多种方法可以得到运行在 Citrix 主机上的远程桌面，最近 Ian Vitek 发布了一些很有用的 PERL 脚本工具。我最常用的扫描 Citrix 请求的工具在这个可以下载：
[http://www.cqure.net/itools01.html](http://www.cqure.net/itools01.html)
这个工具列出了远程机器上允许的请求，得到这些列表之后我们就可以通过修改 .ICA 文件中的请求信息。再这之后，你就可以尝试针对一些容易猜到的账号进行攻击，我的经验是尝试攻击一些专门用来做备份工作的账号。
（如果上面的程序在你的机器上不能很好的工作，你可以试试这个:http://sh0dan.org/files/pubappbrute.tar.gz）
假设你已经做好一切准备，只是没有好运气的话，你可以尝试 GUEST 账号，虽然通常情况下GUEST没有足够的登陆权限。
从 .ICA 文件入手来看：
[WFClient]
Version=2
TcpBrowserAddress=ip.ip.ip.ip

[ApplicationServers]
word=

[word]
Address=word
InitialProgram=#word
ClientAudio=Off
Compress=On
TWIMode=On
DesiredHRES=800
DesiredVRES=600
DesiredColor=4
TransportDriver=TCP/IP
WinStationDriver=ICA 3.0

通过分析看来Citrix在运行时似乎寻找了 [word] 中 InitialProgram 的数据。如果我们把这个数据修改成cmd.exe 或 explorer.exe 会怎么样？呵呵，幸运的是我们真的可以指定运行这个程序，我曾利用这个方法无数次的绕过了登陆检查。只要确定程序是有效的存在的我们就可以运行它。现在我们已经在Citrix有了一个远程 shell。如果你运行的是 explorer.exe，那么你就打开了一个真正的桌面。我希望你熟悉如何提升自己的权限，在这里我就不多说了。另外，即使我以 guest 身份进入系统，我也可以运行只有管理员才可以运行的工具。很明显，这是一个严重的漏洞，你可以在网络上发现大量的弱口令用户。
如果他们使用 NFuse 和 Citrix Secure Gateway 过滤了所有通过防火墙的连接。这样你修改 .ICA 文件就没用了。但是我们仍有很多的机会获得远程桌面。一般情况下 Citrix 管理员经常在办公室远程操作，太好了，如果你有权远程使用 excel ，做个一个 vbscript 当到启动菜单里面，就算你像我一样懒，只是用 IE 浏览网页的话，我们也可以得到 C:\winnt\explorer.exe，其他 Microsoft Word 软件都可以做到。实际上决大多数情况下，我们可以通过 帮助-->帮助主题-->跳至URL 填写 cmd.exe 你可以下载它。如果你的远程机器上有写权限，修改你的.ICA 文件，哈哈，你又可以得到一个 shell 了。
类似的方法还有很多，你可以打开进程管理器启动一个新的进程，或者利用大部分程序中 打开-->浏览 获得一个 shell,如果你可以浏览远程机器，还可以从默认的配置文件目录：
NT4 ： %systemroot%\profiles\username\Application Data\
Windows 2000 ： C:\Documents and Settings\username\Application Data\
将里面的 .ICA 文件复制过来，覆盖你相应目录下的文件，之后打开 Citrix Program Neighborhood 你会发现你有权向所有已经连接的用户发出修改密码的消息。
四、安全的Citrix
还好这里有很多资料来提醒管理员加固Citrix，我曾经花费几周的时间试图找出一个应用与安全相平衡的安全策略：
1、首先正确配置 NFuse / Citrix Secure Gateway
2、确定 IIS/Apache 已经打了最新补丁，并且在 DMZ 的保护中，或者使用 NTLM 认证
3、如果可能，要求远程用户使用 SecureID 认证方式（原有的 ICA 认证方式，被发现存在弱加密算法漏洞——译者注）
4、使用其它的浏览器取代 IE
5、建立一个组，把所有Citrix用户放到这个组里面，禁止他们访问 cmd.exe, [ftp://ftp.exe/](ftp://ftp.exe/), tftp.exe, rcp.exe, net.exe,command.com, iexplorer.exe 等可能对系统有危害的权限(在安全与应用的平衡之间选择)
6、给你的Citrix打上最新的补丁
7、禁止 winhelp32 的访问，设置 Internet 选项禁止下载,禁止使用进程管理器
8、如果可能，设置 Citrix Connection Configuration—>ica-tcp—>client settings —>选择必须
五、写在最后
翻译完着篇文章发现 Citrix系统很类似现在网吧广泛应用的类似虚拟桌面的程序，在研究了应用比较广泛的几款网吧管理程序后发现，此类程序都并非建立的系统内核基础上，很容易利用 Windows 本身的机制绕过，这才是这类软件漏洞产生的根本原因。

名词解析：
DMZ：De-Militarized Zone，非军事区，是防火墙的一个特性。它可以使某台特定计算机向互连网开放。有些应用程序需要开通多个TCP/IP接口。而DMZ就可以为微机实现这些功能。

【作者: [417]()】【访问统计:  】【2007年01月4日 星期四 20:18】【[注册](http://reg.bokee.com/account/web/register.jsp)】【[打印]()】

[
### 搜索

]()   [![Google]()]()
[
### Trackback

]()

你可以使用这个链接引用该篇文章 http://publishblog.blogchina.com/blog/tb.b?diaryID=6017226

[
### 回复

]()
验证码：    ![]() 评论内容：
**************作者已禁止回复功能**************
[2003-2004 BOKEE.COM All rights reserved](http://blog.bokee.com/)
[Powered by BlogDriver 2.1](http://www.blogdriver.com/)
