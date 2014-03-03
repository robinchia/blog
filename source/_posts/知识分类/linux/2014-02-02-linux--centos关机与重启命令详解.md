---
layout: post
title: "centos关机与重启命令详解"
categories: linux
tags: 
 - linux
--- 

# centos关机与重启命令详解

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

# [xautjzd小天地](http://blog.csdn.net/jiangzhengdong)

## 不积跬步无以至千里 ，不积小流无以成江海

* [![]()目录视图](http://blog.csdn.net/jiangzhengdong?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/jiangzhengdong?viewmode=list)
* [![]()订阅](http://blog.csdn.net/jiangzhengdong/rss/list)
[《这些年，我们读过的技术经典图书》主题有奖征文](http://blog.csdn.net/blogdevteam/article/details/9819385)        [专访李铁军：从医生到金山首席安全专家的转变](http://www.csdn.net/article/2013-08-06/2816471)      [独一无二的职位：开源社区经理](http://blog.csdn.net/adali/article/details/9813651)        [CSDN博客第三期云计算最佳博主评选](http://blog.csdn.net/blogdevteam/article/details/10389969)

### [centos关机与重启命令详解]()

分类： [Linux](http://blog.csdn.net/jiangzhengdong/article/category/1110445)  2012-10-02 14:06 3091人阅读 [评论]()(0) [收藏]( "收藏") [举报]( "举报")
[centos](http://blog.csdn.net/tag/details.html?tag=centos)[linux](http://blog.csdn.net/tag/details.html?tag=linux)[signal](http://blog.csdn.net/tag/details.html?tag=signal)[login](http://blog.csdn.net/tag/details.html?tag=login)[工作](http://blog.csdn.net/tag/details.html?tag=%e5%b7%a5%e4%bd%9c)[windows](http://blog.csdn.net/tag/details.html?tag=windows)

Linux centos关机与重启命令详解与实战

Linux centos重启命令：

* 1、reboot
* 2、shutdown -r now 立刻重启(root用户使用)
* 3、shutdown -r 10 过10分钟自动重启(root用户使用)
* 4、shutdown -r 20:35 在时间为20:35时候重启(root用户使用)

如果是通过shutdown命令设置重启的话，可以用shutdown -c命令取消重启

Linux centos关机命令：

* 1、halt 立刻关机
* 2、poweroff 立刻关机
* 3、shutdown -h now 立刻关机(root用户使用)
* 4、shutdown -h 10 10分钟后自动关机

如果是通过shutdown命令设置关机的话，可以用shutdown -c命令取消重启

1.shutdown

shutdown命令安全地将系统关机。 有些用户会使用直接断掉电源的方式来关闭linux，

这是十分危险的。因为linux与windows不同，其后台运行着许多进程，所以强制关机可能

会导致进程的数据丢失﹐使系统处于不稳定的状态﹐甚至在有的系统中会损坏硬件设备。

而在系统关机前使用shutdown命令﹐系统管理员会通知所有登录的用户系统将要关闭。

并且login指令会被冻结﹐即新的用户不能再登录。直接关机或者延迟一定的时间才关机

都是可能的﹐还可能重启。这是由所有进程〔process〕都会收到系统所送达的信号〔signal〕

决定的。这让像vi之类的程序有时间储存目前正在编辑的文档﹐而像处理邮件〔mail〕和

新闻〔news〕的程序则可以正常地离开等等。

shutdown执行它的工作是送信号〔signal〕给init程序﹐要求它改变runlevel。

Runlevel 0被用来停机〔halt〕﹐runlevel 6是用来重新激活〔reboot〕系统﹐

而runlevel 1则是被用来让系统进入管理工作可以进行的状态﹔这是预设的﹐假定没有-h也

没有-r参数给shutdown。要想了解在停机〔halt〕或者重新开机〔reboot〕过程中做了哪些

动作﹐你可以在这个文件/etc/inittab里看到这些runlevels相关的资料。

shutdown 参数说明:

[-t] 在改变到其它runlevel之前﹐告诉init多久以后关机。

[-r] 重启计算器。

[-k] 并不真正关机﹐只是送警告信号给每位登录者〔login〕。

[-h] 关机后关闭电源〔halt〕。

[-n] 不用init﹐而是自己来关机。不鼓励使用这个选项﹐而且该选项所产生的后果往

往不总是你所预期得到的。

[-c] cancel current process取消目前正在执行的关机程序。所以这个选项当然没有

时间参数﹐但是可以输入一个用来解释的讯息﹐而这信息将会送到每位使用者。

[-f] 在重启计算器〔reboot〕时忽略fsck。

[-F] 在重启计算器〔reboot〕时强迫fsck。

[-time] 设定关机〔shutdown〕前的时间。

2.halt—-最简单的关机命令

其实halt就是调用shutdown -h。halt执行时﹐杀死应用进程﹐执行sync系统调用﹐

文件系统写操作完成后就会停止内核。

参数说明:

[-n] 防止sync系统调用﹐它用在用fsck修补根分区之后﹐以阻止内核用老版本的超

级块〔superblock〕覆盖修补过的超级块。

[-w] 并不是真正的重启或关机﹐只是写wtmp〔/var/log/wtmp〕纪录。

[-d] 不写wtmp纪录〔已包含在选项[-n]中〕。

[-f] 没有调用shutdown而强制关机或重启。

[-i] 关机〔或重启〕前﹐关掉所有的网络接口。

[-p] 该选项为缺省选项。就是关机时调用poweroff。

3.reboot

reboot的工作过程差不多跟halt一样﹐不过它是引发主机重启﹐而halt是关机。它

的参数与halt相差不多。

4.init

init是所有进程的祖先﹐它的进程号始终为1﹐所以发送TERM信号给init会终止所有的

用户进程﹑守护进程等。shutdown 就是使用这种机制。init定义了8个运行级别(runlevel)，

init 0为关机﹐init 1为重启。关于init可以长篇大论﹐这里就不再叙述。另外还有

telinit命令可以改变init的运行级别﹐比如﹐telinit -iS可使系统进入单用户模式﹐

并且得不到使用shutdown时的信息和等待时间。

linux如何修改root管理员密码

以root 身份登录(SSH操作)

输入 passwd 命令 就可以看到提示输入新密码了
分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")

* 上一篇：[CentOS在VirtualBox下安装没有图形界面的解决方法](http://blog.csdn.net/jiangzhengdong/article/details/8036553)
* 下一篇：[Virtualbox运行CentOS报错:cannot access the kernel driver的解决](http://blog.csdn.net/jiangzhengdong/article/details/8036640)
查看评论[]()

  暂无评论
您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fjiangzhengdong%2Farticle%2Fdetails%2F8036594)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()
[![TOP]()]( "回到顶部")

个人资料

[![]( "访问我的空间")](http://my.csdn.net/jiangzhengdong)
[jiangzhengdong](http://my.csdn.net/jiangzhengdong)

[]( "[加关注]") []( "[发私信]")
[![]()](http://medal.blog.csdn.net/allmedal.aspx)

* 访问：52541次
* 积分：1623分
* 排名：第5984名

* 原创：106篇
* 转载：50篇
* 译文：1篇
* 评论：15条

文章搜索

[]()

文章分类

* [Linux](http://blog.csdn.net/jiangzhengdong/article/category/1110445)(37)
* [Algorithms](http://blog.csdn.net/jiangzhengdong/article/category/1330382)(2)
* [C](http://blog.csdn.net/jiangzhengdong/article/category/1359389)(18)
* [MyEclipse](http://blog.csdn.net/jiangzhengdong/article/category/1265876)(1)
* [Spring](http://blog.csdn.net/jiangzhengdong/article/category/1168363)(5)
* [Hibernate](http://blog.csdn.net/jiangzhengdong/article/category/1154857)(10)
* [struts2](http://blog.csdn.net/jiangzhengdong/article/category/1141894)(8)
* [Database](http://blog.csdn.net/jiangzhengdong/article/category/1154227)(11)
* [java开发](http://blog.csdn.net/jiangzhengdong/article/category/1112675)(6)
* [asp.net MVC3](http://blog.csdn.net/jiangzhengdong/article/category/1113521)(21)
* [Web实用功能](http://blog.csdn.net/jiangzhengdong/article/category/1110458)(15)
* [Javascript/CSS](http://blog.csdn.net/jiangzhengdong/article/category/1120411)(19)
* [tomcat](http://blog.csdn.net/jiangzhengdong/article/category/1151564)(1)
* [SVN](http://blog.csdn.net/jiangzhengdong/article/category/1241663)(2)
* [windows系统相关知识](http://blog.csdn.net/jiangzhengdong/article/category/1182720)(4)
* [网络协议](http://blog.csdn.net/jiangzhengdong/article/category/1255782)(3)
文章存档

* [2013年07月](http://blog.csdn.net/jiangzhengdong/article/month/2013/07)(11)
* [2013年06月](http://blog.csdn.net/jiangzhengdong/article/month/2013/06)(7)
* [2013年05月](http://blog.csdn.net/jiangzhengdong/article/month/2013/05)(1)
* [2013年04月](http://blog.csdn.net/jiangzhengdong/article/month/2013/04)(13)
* [2013年03月](http://blog.csdn.net/jiangzhengdong/article/month/2013/03)(18)
* [2013年02月](http://blog.csdn.net/jiangzhengdong/article/month/2013/02)(4)
* [2013年01月](http://blog.csdn.net/jiangzhengdong/article/month/2013/01)(10)
* [2012年12月](http://blog.csdn.net/jiangzhengdong/article/month/2012/12)(1)
* [2012年11月](http://blog.csdn.net/jiangzhengdong/article/month/2012/11)(6)
* [2012年10月](http://blog.csdn.net/jiangzhengdong/article/month/2012/10)(22)
* [2012年09月](http://blog.csdn.net/jiangzhengdong/article/month/2012/09)(5)
* [2012年08月](http://blog.csdn.net/jiangzhengdong/article/month/2012/08)(15)
* [2012年07月](http://blog.csdn.net/jiangzhengdong/article/month/2012/07)(4)
* [2012年06月](http://blog.csdn.net/jiangzhengdong/article/month/2012/06)(3)
* [2012年05月](http://blog.csdn.net/jiangzhengdong/article/month/2012/05)(18)
* [2012年04月](http://blog.csdn.net/jiangzhengdong/article/month/2012/04)(14)
* [2012年03月](http://blog.csdn.net/jiangzhengdong/article/month/2012/03)(6)

展开

推荐文章
最新评论

* [Highcharts基本设置](http://blog.csdn.net/jiangzhengdong/article/details/8456224#comments)

[jiangzhengdong](http://blog.csdn.net/jiangzhengdong): 可能是吧，感觉写博客的水平确实不是很高，有待提高吧
* [Highcharts基本设置](http://blog.csdn.net/jiangzhengdong/article/details/8456224#comments)

[zhangxitab](http://blog.csdn.net/zhangxitab): 不是很清楚
* [关于Asp.Net Mvc3.0 使用KindEditor4.0 上传图片与文件](http://blog.csdn.net/jiangzhengdong/article/details/7477573#comments)

[jobs123456](http://blog.csdn.net/jobs123456): 为何文件一直在上传中？用批量上传时，上传成功了，但还是显示上传失败。
* [发现highcharts之highstock中一个小bug](http://blog.csdn.net/jiangzhengdong/article/details/8716995#comments)

[nigelyyz](http://blog.csdn.net/nigelyyz): 我要补充一下，我也发现了这个问题。当你的series超过两条且是通过chartObject .ser...
* [CentOS在VirtualBox下安装没有图形界面的解决方法](http://blog.csdn.net/jiangzhengdong/article/details/8036553#comments)

[northcamel](http://blog.csdn.net/northcamel): 原来是这样。。。
* [CentOS在VirtualBox下安装没有图形界面的解决方法](http://blog.csdn.net/jiangzhengdong/article/details/8036553#comments)

[HelloWorld90](http://blog.csdn.net/HelloWorld90): 正解！！！
* [Virtualbox运行CentOS报错:cannot access the kernel driver的解决](http://blog.csdn.net/jiangzhengdong/article/details/8036640#comments)

[DONG_HAO1208](http://blog.csdn.net/DONG_HAO1208): 谢谢，很清楚
* [Microsoft JScript 运行时错误: 对象不支持此属性或方法](http://blog.csdn.net/jiangzhengdong/article/details/7893019#comments)

[renought](http://blog.csdn.net/renought): 表示5分钟就发现不兼容。换了个 1.7的路过。
* [ASP.NET MVC3 ModelState.IsValid为false的问题](http://blog.csdn.net/jiangzhengdong/article/details/7417200#comments)

[zltcplu](http://blog.csdn.net/zltcplu): 正好遇到了这个问题，谢谢！
* [CKFinder使用过程中出现的错误总结](http://blog.csdn.net/jiangzhengdong/article/details/7414398#comments)

[仰望那天空](http://blog.csdn.net/chenbin_90): 感谢博主！！！我也碰到这个问题了，帮了大忙了！

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)[QQ客服](http://wpa.qq.com/msgrd?v=3&uin=2355263776&site=qq&menu=yes) [微博客服](http://e.weibo.com/csdnsupport/profile) [论坛反馈](http://bbs.csdn.net/forums/Service) [联系邮箱：webmaster@csdn.net](mailto:webmaster@csdn.net) 服务热线：400-600-2320京 ICP 证 070598 号北京创新乐知信息技术有限公司 版权所有世纪乐知(北京)网络技术有限公司 提供技术支持江苏乐知网络技术有限公司 提供商务支持Copyright © 1999-2012, CSDN.NET, All Rights Reserved [![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
![](http://counter.csdn.net/pv.aspx?id=24)
