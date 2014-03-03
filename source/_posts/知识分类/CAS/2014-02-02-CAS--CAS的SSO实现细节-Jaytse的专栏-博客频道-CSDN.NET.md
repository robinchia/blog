---
layout: post
title: "CAS 的SSO实现细节 - Jaytse的专栏 - 博客频道 - CSDN.NET"
categories: CAS
tags: 
 - CAS
--- 

# CAS 的SSO实现细节 - Jaytse的专栏 - 博客频道 - CSDN.NET

您还未登录！|[登录](http://passport.csdn.net/account/login)|[注册](http://passport.csdn.net/account/register)|[帮助](http://passport.csdn.net/help/faq)

* [CSDN首页](http://www.csdn.net/)
* [资讯](http://news.csdn.net/)
* [论坛](http://bbs.csdn.net/)
* [博客](http://blog.csdn.net/)
* [下载](http://download.csdn.net/)
* [搜索](http://so.csdn.net/)
* 
## [更多]()

* [CTO俱乐部](http://cto.csdn.net/)
* [学生大本营](http://student.csdn.net/)
* [培训充电](http://edu.csdn.net/)
* [移动开发](http://mobile.csdn.net/)
* [软件研发](http://sd.csdn.net/)
* [云计算](http://cloud.csdn.net/)
* [程序员](http://www.programmer.com.cn/)
* [ITeye](http://www.iteye.com/)
* [TUP](http://tup.csdn.net/)

# [Jaytse的专栏](http://blog.csdn.net/jaytse)

## 欢迎来到小米的CSDN

* [![]()目录视图](http://blog.csdn.net/jaytse?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/jaytse?viewmode=list)
* [![]()订阅](http://blog.csdn.net/jaytse/rss/list)
[用开源IaaS构建自己的云——OpenStack征稿启事](http://cloud.csdn.net/a/20120620/2806805.html)                   [CSDN社区7月"畅谈加班 赢程序员杂志"活动火爆上线！！](http://topic.csdn.net/u/20120709/15/2e6511e3-e34f-41d7-9f71-a47bb4f8c9fa.html)
[2012年7月当选微软MVP的CSDN会员名单揭晓！](http://blog.csdn.net/blogdevteam/article/details/7712568)                    [CSDN账号全站整合公告](http://topic.csdn.net/u/20120704/14/c98b3641-359f-4bea-b111-21c409db8819.html)                [不用买彩票，就有408万！](http://adclk.thinkmedia.cn/clk/pid=2000/media=CSDN.CN/place=1Clt1/size=760x90)

### [CAS 的SSO实现细节]()

分类： [J2EE](http://blog.csdn.net/jaytse/article/category/168314)  2008-07-25 16:07 629人阅读 [评论](http://blog.csdn.net/jaytse/article/details/2710567#comments)(0) [收藏]( "收藏") [举报](http://blog.csdn.net/jaytse/article/details/2710567#report "举报")
 

实验步骤

1.       访问partner1 (CAS Client 1)的index.jsp

2.       访问partner2 (CAS Client 2)的index.jsp

3.       访问partner1的debug.jsp

4.       访问[www.test.com:443/cas-server/logout](http://www.test.com:443/cas-server/logout) (CAS Server)

 

讨论

步骤一

http://p.blog.csdn.net/images/p_blog_csdn_net/jaytse/EntryImages/20080725/p1.JPG

1.       请求partner1的index.jsp

2.       发现请求页面为受限页面，REDIRECT 到CAS-Server端进行认证

3.       返回CAS Server登录页面

4.       输入用户名、密码，Form表单提交

5.       CAS Server端进行认证，REDIRECT到Partner端，此时CAS-Server端为Partner1生成ST，并在浏览器记录CAS Server的Cookie

6.       Partner端获取到ST后，到CAS Server端去验证，返回认证信息

7.       若通过认证（1.0为yes， 2.0为XML格式验证），Partner返回请求的页面，并在浏览器记录Partner的Cookie

 

步骤二

http://p.blog.csdn.net/images/p_blog_csdn_net/jaytse/EntryImages/20080725/p2.JPG

1.       请求Partner2 的index.jsp

2.       发现请求页面为受限页面，REDIRECT 到CAS-Server端进行认证。浏览器的Redirect带着将已经被记录了的通过认证的CAS Server的Cookie一同到CAS-Server认证

3.       认证通过，REDIRECT到Partner2，此时CAS Server端为Partner2生成ST。

4.       Partner 2获取到ST后，到CAS Server端去验证，返回认证信息

5.       若通过认证，Partner2返回请求的页面，并在浏览器记录Partner2的Cookie

 

步骤三

http://p.blog.csdn.net/images/p_blog_csdn_net/jaytse/EntryImages/20080725/p3.JPG

1.       请求Partner 1的debug.jsp（带有Partner 1的Cookie信息）

2.       返回debug.jsp

 

步骤四

http://p.blog.csdn.net/images/p_blog_csdn_net/jaytse/EntryImages/20080725/p4.JPG

1.       请求logout

2.       CAS Server通知Partner 1和Partner 2

3.       返回CAS Server的logout页面

 

备注

Redirect分成2个步骤：

1.       response发送给浏览器返回信息，通知浏览器创建新的请求

2.       请求新的页面

 

Cookie中携带的信息：

1.       域名

2.       路径
当域名和路径名全部相同的时候，浏览器产生请求才把对应的信息一同携带给服务器

分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")
* 上一篇：[CAS学习笔记](http://blog.csdn.net/jaytse/article/details/2710041)
* 下一篇：[Solaris10 cron使用](http://blog.csdn.net/jaytse/article/details/2710594)
查看评论[]()

  暂无评论
您还没有登录,请[[登录]](http://passport.csdn.net/account/login?from=http%3A%2F%2Fblog.csdn.net%2Fjaytse%2Farticle%2Fdetails%2F2710567)或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fjaytse%2Farticle%2Fdetails%2F2710567)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()

个人资料

[![]( "访问我的空间")](http://my.csdn.net/jaytse)
[jaytse](http://my.csdn.net/jaytse)

[](http://blog.csdn.net/jaytse/article/details/2710567# "[加关注]") [](http://my.csdn.net/my/letter/send/jaytse "[发私信]")

* 访问：68320次
* 积分：1431分
* 排名：第4313名

* 原创：68篇
* 转载：30篇
* 译文：0篇
* 评论：46条

文章搜索

[]()
文章分类

* [C Programming Language](http://blog.csdn.net/jaytse/article/category/168316)(4)
* [database](http://blog.csdn.net/jaytse/article/category/358000)(4)
* [Design Pattern](http://blog.csdn.net/jaytse/article/category/168313)(0)
* [Framework](http://blog.csdn.net/jaytse/article/category/168317)(0)
* [J2EE](http://blog.csdn.net/jaytse/article/category/168314)(21)
* [J2SE](http://blog.csdn.net/jaytse/article/category/168315)(12)
* [System](http://blog.csdn.net/jaytse/article/category/358001)(4)
* [心情点滴](http://blog.csdn.net/jaytse/article/category/168318)(42)

文章存档

* [2010年05月](http://blog.csdn.net/jaytse/article/month/2010/05)(1)
* [2010年04月](http://blog.csdn.net/jaytse/article/month/2010/04)(1)
* [2010年01月](http://blog.csdn.net/jaytse/article/month/2010/01)(4)
* [2009年12月](http://blog.csdn.net/jaytse/article/month/2009/12)(1)
* [2009年11月](http://blog.csdn.net/jaytse/article/month/2009/11)(1)
* [2009年10月](http://blog.csdn.net/jaytse/article/month/2009/10)(2)
* [2009年05月](http://blog.csdn.net/jaytse/article/month/2009/05)(4)
* [2008年12月](http://blog.csdn.net/jaytse/article/month/2008/12)(3)
* [2008年11月](http://blog.csdn.net/jaytse/article/month/2008/11)(2)
* [2008年09月](http://blog.csdn.net/jaytse/article/month/2008/09)(4)
* [2008年08月](http://blog.csdn.net/jaytse/article/month/2008/08)(2)
* [2008年07月](http://blog.csdn.net/jaytse/article/month/2008/07)(11)
* [2008年05月](http://blog.csdn.net/jaytse/article/month/2008/05)(1)
* [2008年04月](http://blog.csdn.net/jaytse/article/month/2008/04)(1)
* [2008年01月](http://blog.csdn.net/jaytse/article/month/2008/01)(5)
* [2007年10月](http://blog.csdn.net/jaytse/article/month/2007/10)(1)
* [2007年09月](http://blog.csdn.net/jaytse/article/month/2007/09)(1)
* [2007年08月](http://blog.csdn.net/jaytse/article/month/2007/08)(2)
* [2007年07月](http://blog.csdn.net/jaytse/article/month/2007/07)(2)
* [2007年03月](http://blog.csdn.net/jaytse/article/month/2007/03)(1)
* [2006年11月](http://blog.csdn.net/jaytse/article/month/2006/11)(2)
* [2006年10月](http://blog.csdn.net/jaytse/article/month/2006/10)(4)
* [2006年09月](http://blog.csdn.net/jaytse/article/month/2006/09)(3)
* [2006年08月](http://blog.csdn.net/jaytse/article/month/2006/08)(3)
* [2006年07月](http://blog.csdn.net/jaytse/article/month/2006/07)(2)
* [2006年06月](http://blog.csdn.net/jaytse/article/month/2006/06)(4)
* [2006年05月](http://blog.csdn.net/jaytse/article/month/2006/05)(1)
* [2006年04月](http://blog.csdn.net/jaytse/article/month/2006/04)(2)
* [2006年03月](http://blog.csdn.net/jaytse/article/month/2006/03)(12)
* [2006年02月](http://blog.csdn.net/jaytse/article/month/2006/02)(2)
* [2006年01月](http://blog.csdn.net/jaytse/article/month/2006/01)(6)
* [2005年12月](http://blog.csdn.net/jaytse/article/month/2005/12)(6)
* [2005年08月](http://blog.csdn.net/jaytse/article/month/2005/08)(1)

展开
阅读排行

* [CAS学习笔记](http://blog.csdn.net/jaytse/article/details/2710041 "CAS学习笔记")(6331)
* [IBM 几次电话面试的总结](http://blog.csdn.net/jaytse/article/details/578083 "IBM 几次电话面试的总结")(4022)
* [Openfire Connection Manager 配置](http://blog.csdn.net/jaytse/article/details/4210625 "Openfire Connection Manager 配置")(3454)
* [MSN协议分析以及Java实现MSN登陆](http://blog.csdn.net/jaytse/article/details/3291578 "MSN协议分析以及Java实现MSN登陆")(2666)
* [利用HTTP协议获取163的联系人列表(1)](http://blog.csdn.net/jaytse/article/details/3452981 "利用HTTP协议获取163的联系人列表(1)")(2190)
* [利用HTTP协议获取163的联系人列表(3)](http://blog.csdn.net/jaytse/article/details/3453123 "利用HTTP协议获取163的联系人列表(3)")(2029)
* [WAS中关于命令行部署EAR](http://blog.csdn.net/jaytse/article/details/632843 "WAS中关于命令行部署EAR")(1788)
* [Axis试用小记（－）](http://blog.csdn.net/jaytse/article/details/565439 "Axis试用小记（－）")(1719)
* [我的这半年---离职和求职](http://blog.csdn.net/jaytse/article/details/1536991 "我的这半年---离职和求职")(1537)
* [“老板”的故事](http://blog.csdn.net/jaytse/article/details/659098 "“老板”的故事")(1511)

评论排行

* [Openfire Connection Manager 配置](http://blog.csdn.net/jaytse/article/details/4210625 "Openfire Connection Manager 配置")(6)
* [利用HTTP协议获取163的联系人列表(1)](http://blog.csdn.net/jaytse/article/details/3452981 "利用HTTP协议获取163的联系人列表(1)")(6)
* [[转载] 沦丧——交大校友的真实职场经历和体会](http://blog.csdn.net/jaytse/article/details/5627289 "[转载] 沦丧——交大校友的真实职场经历和体会")(4)
* [CAS学习笔记](http://blog.csdn.net/jaytse/article/details/2710041 "CAS学习笔记")(4)
* [逝去的2009（四）](http://blog.csdn.net/jaytse/article/details/5274598 "逝去的2009（四）")(4)
* [IBM CSDL的几个月](http://blog.csdn.net/jaytse/article/details/995743 "IBM CSDL的几个月")(3)
* [用 openssl 签发CA](http://blog.csdn.net/jaytse/article/details/2710925 "用 openssl 签发CA")(2)
* [IBM 几次电话面试的总结](http://blog.csdn.net/jaytse/article/details/578083 "IBM 几次电话面试的总结")(2)
* [逝去的2009（三）](http://blog.csdn.net/jaytse/article/details/5216774 "逝去的2009（三）")(2)
* [A week went again](http://blog.csdn.net/jaytse/article/details/643466 "A week went again")(2)
推荐文章

最新评论

* [Openfire Connection Manager 配置](http://blog.csdn.net/jaytse/article/details/4210625#comments)

taizans: good..
* [Openfire Connection Manager 配置](http://blog.csdn.net/jaytse/article/details/4210625#comments)

xueixin: 我的bin/cmanager.sh 启动不起来咋回事啊，能够提供一些详细的解决方案吗？ 我的qq67...
* [Openfire Connection Manager 配置](http://blog.csdn.net/jaytse/article/details/4210625#comments)

nongyan90: 你说的这些对我很有用，谢谢了！
* [Openfire Connection Manager 配置](http://blog.csdn.net/jaytse/article/details/4210625#comments)

nongyan90: author ，我们很关心图啊
* [Openfire Connection Manager 配置](http://blog.csdn.net/jaytse/article/details/4210625#comments)

leibf: 所谓胡图在哪里
* [CAS学习笔记](http://blog.csdn.net/jaytse/article/details/2710041#comments)

if_else123: 学习了，正在找单点登录方面的资料，虽然没有这么做，但还是支持楼主。
* [CAS学习笔记](http://blog.csdn.net/jaytse/article/details/2710041#comments)

jaytse: 呵呵，我基本上没有看懂你的问题；但我想可能是spring的事情吧；1.5比1.4多了泛型和其他扩展，...
* [Openfire Connection Manager 配置](http://blog.csdn.net/jaytse/article/details/4210625#comments)

liu282713097:
* [利用HTTP协议获取163的联系人列表(1)](http://blog.csdn.net/jaytse/article/details/3452981#comments)

tianguowei123: 你好，你写的那篇关于《利用HTTP协议获取163的联系人列表》三个系列，但是现在又改了，有空帮忙分析...
* [[转载] 沦丧——交大校友的真实职场经历和体会](http://blog.csdn.net/jaytse/article/details/5627289#comments)

lwchw: 工作的不入操作的，这年头完全颠倒了。
设计模式

* [设计模式(Patterns in Java)](http://dev.csdn.net/article/68/68538.shtm)

我的相册

* [Google相册](http://picasaweb.google.com/xjtujay)
* [QQ相册](http://xiaoyou.qq.com/index.php?mod=photo&u=c265e4bd629300c5dfe5aad01ee2601cc513190cdf90331d)
学生时代

* [东北电力学院](http://www.neiep.edu.cn/)
* [西安交通大学](http://www.xjtu.edu.cn/)
* [香港理工大学](http://www.polyu.edu.hk/)

衣食父母

* [广州傲思（已经倒了）](http://www.gzoas.com/)
* [IBM CSDL](http://www-900.ibm.com/cn/cdl/infomagmt/)
* [中科院软件所](http://www.iscas.ac.cn/)
* [中山大学](http://www.sysu.edu.cn/)
* [香港大学](http://www.hku.hk/)
友情支持

* [yevv's blog](http://blog.csdn.net/yevv)
* [our ifa's blog](http://ourifa.bokee.com/)
      ![]()

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)京 ICP 证 070598 号北京创新乐知信息技术有限公司 版权所有![]() 联系邮箱：webmaster@csdn.netCopyright © 1999-2012, CSDN.NET, All Rights Reserved [![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
