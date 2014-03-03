---
layout: post
title: "新浪微博如何实现 SSO 的分析 - 企业应用 - Java - ITeye论坛"
categories: CAS
tags: 
 - CAS
--- 

# 新浪微博如何实现 SSO 的分析 - 企业应用 - Java - ITeye论坛

[您还未登录 !](http://www.iteye.com/login "登录") [登录](http://www.iteye.com/login) [注册](http://www.iteye.com/signup)

[![ITeye-最棒的软件开发交流社区]( "ITeye-最棒的软件开发交流社区")](http://www.iteye.com/)

[论坛首页](http://www.iteye.com/forums) → [Java企业应用论坛](http://www.iteye.com/forums/board/Java) →

# 新浪微博如何实现 SSO 的分析

[全部](http://www.iteye.com/forums/board/Java) [Hibernate](http://www.iteye.com/forums/tag/Hibernate) [Spring](http://www.iteye.com/forums/tag/Spring) [Struts](http://www.iteye.com/forums/tag/Struts) [iBATIS](http://www.iteye.com/forums/tag/iBATIS) [企业应用](http://www.iteye.com/forums/tag/%E4%BC%81%E4%B8%9A%E5%BA%94%E7%94%A8) [Lucene](http://www.iteye.com/forums/tag/Lucene) [SOA](http://www.iteye.com/forums/tag/SOA) [Java综合](http://www.iteye.com/forums/tag/Java%E7%BB%BC%E5%90%88) [设计模式](http://www.iteye.com/forums/tag/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F) [Tomcat](http://www.iteye.com/forums/tag/Tomcat) [OO](http://www.iteye.com/forums/tag/OO) [JBoss](http://www.iteye.com/forums/tag/JBoss)
« 上一页 1 [2](http://www.iteye.com/topic/1039052?page=2) [3](http://www.iteye.com/topic/1039052?page=3) [下一页 »](http://www.iteye.com/topic/1039052?page=2)

浏览 14772 次 锁定老帖子 [主题：新浪微博如何实现 SSO 的分析]()

**该帖已经被评为精华帖** 作者 正文 * denger
* 等级: ![四星会员]( "四星会员")
* [![denger的博客]( "denger的博客: Coding for fun.      ")](http://denger.iteye.com/)
* 性别: ![]( "男")
* 文章: 176
* 积分: 400
* 来自: 北京
* ![]()
    发表时间：2011-05-10   最后修改：2011-06-25

[<](http://www.iteye.com/topic/1039052#) [>](http://www.iteye.com/topic/1039052#)  猎头职位: [上海:  【上海】外资企业高新诚聘web开发工程师](http://www.iteye.com/jobs/2245)

相关文章: [ ](http://www.iteye.com/topic/1039052# "关闭")

* [CAS 之 跨域 Ajax 登录实践](http://www.iteye.com/topic/1111931 "CAS 之 跨域 Ajax 登录实践")
* [跨域的单点登录](http://www.iteye.com/topic/21770 "跨域的单点登录")
* [【原创】CAS调研总结](http://www.iteye.com/topic/544899 "【原创】CAS调研总结")
推荐群组: [liferay](http://liferay.group.iteye.com/)
[更多相关推荐](http://www.iteye.com/wiki/topic/1039052)
[企业应用](http://www.iteye.com/forums/tag/%E4%BC%81%E4%B8%9A%E5%BA%94%E7%94%A8)      最近在使用sina微博时，经常性交替使用　weibo.com 和 t.sina.cm.cn进入我的微博。发现当我在 t.sina.com.cn中登录之后，直接切换至weibo.com，这时候在 weibo.com是已经登录的，当我在 weibo.com进行注销之后，再切换至 t.sina.com.cn，这时候在 t.sina.com.cn也已经是注销的状态了。
     对于SSO的实现方案及其机制，早已经不是什么新鲜的技术了，从微软为.net提供的passport机制到java中开源的JBoss SSO、Oracle OpenSSO及经典的 Yale CAS等等之类的开源或一些商业SSO中间件都不失为作为单点登录实现的选择。当然一些企业也会选择自己实现一套适合自己轻量级方案，如采用SESSIONID转递或SESSION同步复制之类的。　可以看得出SSO的价值也是具大的，就拿sina来说吧，增加 weibo.com域名之后，对于用户来说来说没有任何影响，即使你在 t.sina.com.cn中进行登录，可以无缝在两域名之间随意切换，对于它推广weibo.com无非是大大的益处。
    由于近年来一直在使用 Yale的CAS作为SSO的方案，觉得 SINA的SSO与Yale-CAS有很多异曲同工之妙，于是便对SINA的SSO进行分析，其中的细节处理还是很值的学习的。当然，由于分析看到的SINA SSO处理都只是一些表现或表面上的东西，再加上其大部分关键的sso js都已经被压缩，及SERVER端的实现机制也只是靠自己的经验及结合CAS的的一些原理进行猜测。其实本文应该叫　<CAS SSO与SINA SSO的实现对比分析>更比较贴切。
  
    好吧，进入正题。

* Sina SSO之分析篇
    首先是进入 t.sina.com.cn，提交用户名及密码进行登录，通过 Firebug可以看到它通过类似Aajx POST到了 http://login.sina.com.cn/sso/login.php?client=ssologin.js(v1.3.12)，如下图所示：
    ![]()
    不难看出，其 http://login.sina.com.cn/sso/login.php 就是类似是 CAS 中的 Server，对sina的所有应用系统提供的统一登录入口。上面的参数中有一个service参数，了解 CAS的GG应该知道 cas 在登录的时候除了username 和 password同样也有一个 service 参数，其CAS该参数含义是子应用系统的服务名标识及登录成功之后所跳转的地址。当然，sina这里使用了 "miniblog"作为微博的服务名，估计他在sso-server端对 miniblog 与登录成功之后的地址进行映射，如 miniblog=http://t.sina.com.cn/，这样就避免了CAS-client中转入service= decodeURIComponent('http://t.sina.com.cn')之类的做法了。
    这里的登录与CAS做法一致，将登录验证提交至统一的认证中心进行验证处理，从而避免了跨子域和全域的问题。　验证成功之后路转的路径就是service所向的地址，验证失败之后则返回至当前登录页。下面就SSO中的一些登录方面的核心问题做一些分析，看看SINA和CAS分别是如何处理的：
     　**1.如何授权某个子系统允许其在sso-server进行登录验证呢，类似cas-server中的login-ticket;**
        对于cas来说，在首次进入  /cas/login页时， [会产生一个一次性的login-ticket](http://denger.iteye.com/blog/809170)，也就是说在提交登录验证前必须向服务器请求一个login-ticket，在登录提交时，需要将用户名及密码以及login-ticket进行提交至 cas-server端，cas-server端确定login-ticket有效后才会对用户名及密码进行认证。
        看看sina如何处理的吧，继续看firebug:
        ![]()       以上截图是当我首次进行 t.sina.com.cn时，通过 ajax/jsonp的方式发起的一个请求，可以看到返回的callback函数中的 json 串中包含了 nonce:"SXK19N"的属性，参数名的汉译是“一次”或“一次性”的意思，估计这里的 nonce就是login-ticket，为再一次确实，我再试着提交登录看看，看它是否将该参数POST过去：
       ![]()
       果然不出所料， nonce:"SXK19N"作为参数提交过去了，证明所猜测的应该是正确的。
**2.比如验证码跨域跨服务器导致从session无法获取的问题，我们曾经遇到过;**
        貌似sina登录没有涉及到验证码之类的东西，当你多次登录失败之后，它采用的是“您的登录过于频繁，请稍后再试吧”，这种方案确实比验证码要好的多，而且还避免了上面的说的问题。
     **3. 当我登录失败了，/sso/login.php 如何将登录的错误信息返回给 t.sina.com.cn并让它进行显示呢，如果我登录成功了/sso/login.php 通过什么方式通知t.sina.com.cn呢，因为它这里使用的是ajax方式登录？**
       对于这方面，cas的处理是将错误信息以参数的方式返回给 client-login，如登录失败，重定向地址： http://cas-client.com?errocode=0，如果登录成功，则直接 重定向至 service 中的url，并生成ST给客户端，表示其已经在cas-server登录成功了。
       看看sina如何处理的吧，随便输入一个用户名密码，提交登录，继续通过firebug看看它的处理过程:
![]( "点击查看原始大小图片")
        再看看t.sina.com.cn 中的html内容的变化：
        ![]()
       
  以上图1中发生了两次请求，第一次登录验证是访问 sso认证中心，它所返回response是一个html内容，第二次请求的地址：　http://t.sina.com.cn/ajaxlogin.php　framelogin=1&callback=parent.sinaSSOController.feedBackUrlCallBack&retcode=4038&reason=%B5%C7%C2）
  再结合以上图2信息，看到 html 中发生了变化，创建了一个　id=ssoLoginFrame 的iframe，于是便可以得出，sina 的登录并非原生的ajax方式，而是通过创建iframe来模拟提交不刷新的登录。也就是说，当用户点击登录提交时，这时候它会通过js创建iframe，将登录提效至该iframe中。
         既然已经知道它登录是提交到iframe中，而非ajax方式，那么对于以上截图1中两个请求为什么返回的都是HTML内容就很容易解释了。再回到上面的问题，/sso/login是如何通知t.sina.com.cn登录失败了呢? 首先在以上第一个截图中返回的 HTML包含了一段 javascript:
       

Javascript代码  [![收藏代码]()![]()]( "收藏这段代码")

1. location.replace("http://t.sina.com.cn/ajaxlogin.php?framelogin=1&callback=parent.sinaSSOController.feedBackUrlCallBack&retcode=4038&reason=%B5%C7%C2%BC%B3%A%BC%B3%A2%CA%D4%B4%CE%CA%FD%B9%FD%D3%DA%C6%B5%B7%B1%A3%AC%C7%EB%C9%D4%BA%F3%D4%D9%B5%C7%C2%BC");  
location.replace("http://t.sina.com.cn/ajaxlogin.php?framelogin=1&callback=parent.sinaSSOController.feedBackUrlCallBack&retcode=4038&reason=%B5%C7%C2%BC%B3%A%BC%B3%A2%CA%D4%B4%CE%CA%FD%B9%FD%D3%DA%C6%B5%B7%B1%A3%AC%C7%EB%C9%D4%BA%F3%D4%D9%B5%C7%C2%BC");
         location.replace的意思与location.href类似，同样都是改变当前的URL地址,具体区别及做法可以参考[这里](http://blog.csdn.net/fangxinggood/archive/2006/02/21/604916.aspx)及[这里](http://developer.apple.com/internet/webcontent/iframe.html)。需要注意的这里所说的通过location.replace改变当前的URL其它并非改变t.sina.com.cn的地址，而是第二个截图里iframe中src的地址，因为这段HTML是在iframe中输出的。
       在  locaiton.replace 的地址中包含了一个 retcode 及 reason参数，估计这就是当前登录的错误信息。在上面第一个截图的第二个请求实际就是在iframe 中进行的 location.replace操作后的跳转地址。关键看它输出的html内容：
  

Html代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <html><head>  
1. <script language='javascript'>  
1.  parent.sinaSSOController.feedBackUrlCallBack({"result":false,"errno":"4038","reason":"\u767b\u5f55\u5c1d\u8bd5\u6b21\u6570\u8fc7\u4e8e\u9891\u7e41\uff0c\u8bf7\u7a0d\u540e\u518d\u767b\u5f55"});</script></head><body></body></html>null  
<html><head> <script language='javascript'> parent.sinaSSOController.feedBackUrlCallBack({"result":false,"errno":"4038","reason":"\u767b\u5f55\u5c1d\u8bd5\u6b21\u6570\u8fc7\u4e8e\u9891\u7e41\uff0c\u8bf7\u7a0d\u540e\u518d\u767b\u5f55"});</script></head><body></body></html>null
      这段js是在 iframe中执行的，所以可以通过 parent 进行访问 t.sina.com.cn中的js，可以肯定 parent.sinaSSOController.feedBackUrlCallBack 就是告诉 t.sina.com.cn 当前已经登录失败了，并且将错误信息传至该入该callback了。至此，已经完成了 /sso/login.php 对 t.sina.com.cn的信息传送。　新浪果然是有一手呀，在CAS中AJAX登录一直都是一个问题，而sina它巧妙的通过iframe+callback 进行实现了。
      接着，再看看它对于登录成功之后如何通知 t.sina.com.cn的吧，先看看登录成功之后 sina-sso-server 会做什么，看firebug截图：
       ![]( "点击查看原始大小图片")
       重点在于 set-Cookie: tgc=TGT-MTc4NTc0NzM0Mw==-1305003116-ja-D51B2EB107B79FC50D8CA424BFE08907;  哈哈，熟悉CAS的应该会很熟悉这个，没想到SINA的TGT与CAS的TGT不但参数命名，居然连生成的规则也一模一样，估计sina肯定是参考了 cas 的实现机制。关于TGT是什么或其作用可以参考：[CAS总结之Ticket篇](http://zhenkm0507.iteye.com/blog/546805)。另外还有一个就是当登录成功之后，sina-sso-server会将用户登陆名等等放在sina.com.cn根域的cookie中。
       然后再看看登录成功之后 sina-sso-server所返回的response内容：
       ![]()
       以下是从以上摘取JS部分：
      

Javascript代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <script>  
1. try{sinaSSOController.setCrossDomainUrlList({"retcode":0,"arrURL":["http:\/\/weibo.com\/sso\/crosdom?action=login&savestate=1305607916"]});}catch(e){}try{sinaSSOController.crossDomainAction('login',function(){location.replace('http://t.sina.com.cn/ajaxlogin.php?framelogin=1&callback=parent.sinaSSOController.feedBackUrlCallBack&retcode=0');});}catch(e){}  
1. </script>  
<script> try{sinaSSOController.setCrossDomainUrlList({"retcode":0,"arrURL":["http:\/\/weibo.com\/sso\/crosdom?action=login&savestate=1305607916"]});}catch(e){}try{sinaSSOController.crossDomainAction('login',function(){location.replace('http://t.sina.com.cn/ajaxlogin.php?framelogin=1&callback=parent.sinaSSOController.feedBackUrlCallBack&retcode=0');});}catch(e){} </script>
      首先再次声明，以上firebug截图中的请求处理，并非 AJAX，而是在 t.sina.com.cn中放了一个iframe，输出的 reponse都会至iframe当中.    
      以上的js主要重点在于:
     

Javascript代码  [![收藏代码]()![]()]( "收藏这段代码")

1. location.replace('http://t.sina.com.cn/ajaxlogin.php?framelogin=1&callback=parent.sinaSSOController.feedBackUrlCallBack&retcode=0')  
location.replace('http://t.sina.com.cn/ajaxlogin.php?framelogin=1&callback=parent.sinaSSOController.feedBackUrlCallBack&retcode=0')
      还是通过设置当前iframe中src地址，再看看跳转至http://t.sina.com.cn/ajaxlogin.php后的response内容吧：
      ![]()
      返回用户信息(从cookie中获取的)，并且还是类似上面的做法，通过　parent.sinaSSOController.feedBackUrlCallBack回调t.sina.com.cn中的js，告诉它这个用户已经登录成功了。
      于是t.sina.com.cn便进行跳转至　t.sina.com.cn/dengers 中，从而实现登录。
      ![]()
      整体的处理流程如下：
      ![]()
      **4. 当我在t.sina.com.cn中登录后，切换至weibo.com，weibo.com我应该也是已经登录的，如何做到呢？**
       对于这个问题，CAS中的处理就是，当我进入 weibo.com的时候，马上跳转至  /cas/login，然后在login中判断cookie是否存在TGT，如果存在，并确定其有效性后，则认为你已经登录，并为你生成一个ST，将ST作为ticket参数使其重定向至 weibo.com?ticket=TG-xxxx 并登录。
      看看sina怎么处理的吧，首先我直接在t.sina.com.cn登录成功。然后再新建一个选项卡，输入 weibo.com：
      ![]( "点击查看原始大小图片")
      可以看得出，当我进入　weibo.com之后，sina并没有直接进入 weibo.com的主页，而是马上重定向至:  http://login.sina.com.cn/sso/login.php?url=http%3A%2F%2Fweibo.com%2F&_rand=1305008634.5127&gateway=1&service=miniblog&useticket=1&returntype=META 　与cas的做法确实一致。　再看看该 login.php的Response 信息，主要是JS：
      

Js代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <script type="text/javascript" language="javascript">  
1. location.replace("http://weibo.com/sso/login.php?url=http%3A%2F%2Fweibo.com%2F&ticket=ST-MTc4NTc0NzM0Mw==-1305008634-ja-694BA43623A3F72999AE7129A0572048&retcode=0");  
1. </script>  
<script type="text/javascript" language="javascript"> location.replace("http://weibo.com/sso/login.php?url=http%3A%2F%2Fweibo.com%2F&ticket=ST-MTc4NTc0NzM0Mw==-1305008634-ja-694BA43623A3F72999AE7129A0572048&retcode=0"); </script>
      看到这里之后，不得不怀疑 SINA　的 SSO　是不是用的就是 CAS　啊！！不但连 TGT 参数名一样，连ST规则及参数名也一模一样，其处理机制也十分相似。
      到这里之后就与 CAS　的处理一样了，就不详细写了，可以参考 CAS相关文章。
──────────
PS:由于在分析过程中里面的很多SSO关键JS都压缩了，所以难免会存在误差。　不过SINA的SSO很多细节方面确实处理的很好，作为互联网应用的话，如果单纯的只是把 CAS　DOWNLOAD　下来，然后直接配配就用的话很多方面的处理还是很不到位的。　有时间我把我们CAS参考 SINA　调整一下。
     到这里，不得不说的一个事情就是，之前在[分析淘宝cookie如何跨域获取](http://www.iteye.com/topic/1000776)时，大家都说出了一个taobao的jsonp实际存在一定的安全隐患。后面那个淘宝的GG看到之后加入Refer的判断。而现在，在分析的过程中发现新浪也有这样的问题，可以尝试一下，随便在本地建立一个html，引入jquery，然后使用下面的JS，就可以获取到sina中的登录邮箱名等信息，前提是你需要先在sina中登录：
Js代码  [![收藏代码]()![]()]( "收藏这段代码")

1. $.ajax({url: 'http://t.sina.com.cn/ajaxlogin.php?framelogin=0&callback=?&retcode=0', dataType:'jsonp',  
1.                 success:function(data){  
1.                     alert(data.userinfo.userid);  
1.                 }});  
$.ajax({url: 'http://t.sina.com.cn/ajaxlogin.php?framelogin=0&callback=?&retcode=0', dataType:'jsonp', success:function(data){ alert(data.userinfo.userid); }});
* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/0047/8286/31cd9585-6269-3a37-b8f4-2b8d02e4729a.png)
* 大小: 50.6 KB

* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/0047/8303/e1584505-a33b-31f0-bd37-45d01091e95e.png)
* 大小: 26.4 KB

* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/0047/8305/d4482ff9-9dc3-3ac5-ad7d-c7f994f30ed3.png)
* 大小: 37.5 KB

* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/0047/8580/77c647c4-5c36-3dcd-910f-c90a201cfe71.png)
* 大小: 40 KB

* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/0047/8597/7f3a898c-6211-3cdd-acfe-f940615a73e8.png)
* 大小: 134.6 KB

* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/0047/8602/966dd896-d63f-3622-b78c-ee23370baaed.png)
* 大小: 56 KB

* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/0047/8608/30cd2444-e47c-3b5c-876f-e357a5a617f3.png)
* 大小: 29 KB

* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/0047/8615/ddda1c2e-b441-3e66-930f-6c06a2d2bfa1.png)
* 大小: 35.9 KB

* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/0047/8620/f660b8c0-67b0-31c0-ae8c-27fa88491198.png)
* 大小: 103.5 KB

* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/0047/8636/77438b2b-c573-3bba-8853-682ecdd3380d.png)
* 大小: 110.8 KB

* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/0047/9000/e43d4e20-665b-342e-a693-62bd388c9597.png)
* 大小: 134.6 KB

* [![]( "点击查看原始大小图片")](http://dl.iteye.com/upload/attachment/0047/9002/61d8dccb-90d7-3c8e-b9c0-d58bd48c902e.png)
* 大小: 85.2 KB

* [查看图片附件](http://www.iteye.com/topic/1039052#)

声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。
推荐链接

* [想进外企，出国，跳槽，找工作？英语不好,快来充电吧](http://www.iteye.com/clicks/716)
* [](http://www.iteye.com/clicks/609)
* [世界级研发人员汇聚深圳！](http://www.iteye.com/clicks/770) [返回顶楼](http://www.iteye.com/topic/1039052#) [ ](http://denger.iteye.com/ "浏览作者的博客") [ ](http://denger.iteye.com/blog/profile "浏览作者资料") [ ](http://my.iteye.com/messages/new?message%5Breceiver_name%5D=denger "发送站内短信") [ ](http://denger.iteye.com/blog/guest_book "给作者留言") [ ](http://my.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=denger "关注作者")  * 苍山洱海
* 等级: 初级会员
* [![苍山洱海的博客]( "苍山洱海的博客: ")](http://yangkun0318-126-com.iteye.com/)
* 性别: ![]( "男")
* 文章: 95
* 积分: 0
* 来自: 北京
* ![]()
    发表时间：2011-05-10  

好文章。分析的很详实。慢慢看。 [返回顶楼](http://www.iteye.com/topic/1039052#) [ ](http://yangkun0318-126-com.iteye.com/ "浏览作者的博客") [ ](http://yangkun0318-126-com.iteye.com/blog/profile "浏览作者资料") [ ](http://my.iteye.com/messages/new?message%5Breceiver_name%5D=%E8%8B%8D%E5%B1%B1%E6%B4%B1%E6%B5%B7 "发送站内短信") [ ](http://yangkun0318-126-com.iteye.com/blog/guest_book "给作者留言") [ ](http://my.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=%E8%8B%8D%E5%B1%B1%E6%B4%B1%E6%B5%B7 "关注作者") [回帖地址](http://www.iteye.com/topic/1039052#2095803 "苍山洱海回帖:新浪微博如何实现 SSO 的分析")

[0](http://www.iteye.com/topic/1039052# "好") [0](http://www.iteye.com/topic/1039052# "差") 请登录后投票  * ln1058
* 等级: 初级会员
* [![ln1058的博客]( "ln1058的博客: 这是一段很长很长的旅程,流尽所有的泪水永无止境,我奋力地穿越苦惘和迷墙,在我的路上寻找生命的意义... ")](http://ln1058.iteye.com/)
* 性别: ![]( "男")
* 文章: 43
* 积分: 40
* 来自: 北京
* ![]()
    发表时间：2011-05-11  

我怎么觉得没必要这么麻烦呢。
如果是我来做，直接通过cookie来判断用户是否为登录状态，然后在进行相关的URL跳转。
这种网站对于用户的安全性都不高，真有必要这么麻烦吗？ [返回顶楼](http://www.iteye.com/topic/1039052#) [ ](http://ln1058.iteye.com/ "浏览作者的博客") [ ](http://ln1058.iteye.com/blog/profile "浏览作者资料") [ ](http://my.iteye.com/messages/new?message%5Breceiver_name%5D=ln1058 "发送站内短信") [ ](http://ln1058.iteye.com/blog/guest_book "给作者留言") [ ](http://my.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=ln1058 "关注作者") [回帖地址](http://www.iteye.com/topic/1039052#2096212 "ln1058回帖:新浪微博如何实现 SSO 的分析")

[0](http://www.iteye.com/topic/1039052# "好") [0](http://www.iteye.com/topic/1039052# "差") 请登录后投票  * denger
* 等级: ![四星会员]( "四星会员")
* [![denger的博客]( "denger的博客: Coding for fun.      ")](http://denger.iteye.com/)
* 性别: ![]( "男")
* 文章: 176
* 积分: 400
* 来自: 北京
* ![]()
    发表时间：2011-05-11   最后修改：2011-05-11

ln1058 写道

我怎么觉得没必要这么麻烦呢。
如果是我来做，直接通过cookie来判断用户是否为登录状态，然后在进行相关的URL跳转。
实际上说起来确实是这样，CAS和SINA也是这样的，直接从cookie判断用户是否已经登录，如果已经登录进行相关 URL的跳转。但是，cookie只会存储在一个域名下(login.sina.com.cn) ， sina很多方面都采用 AJAX/JSONP　或 iframe　的方式，因为基本上全部都是跨全域的"AJAX"方式登录的，所以很多地方处理起来并不是那么方便。
另外一个还要考虑的SSO的灵活和通用信，一个的系统或新的域名整合过来，增加或修改达到最少，这些在设计SSO中都是需要考虑、封装的，而sina这方面确实封装的还可以，整个SSO的处理逻辑也很清楚。他参考了CAS的设计方案或者说直接使用的就是CAS然后进行二次开发的。
引用

这种网站对于用户的安全性都不高，真有必要这么麻烦吗？
新浪有着上亿的微博用户噢....　就你一句话说他安全性不高，他就不高了？ [返回顶楼](http://www.iteye.com/topic/1039052#) [ ](http://denger.iteye.com/ "浏览作者的博客") [ ](http://denger.iteye.com/blog/profile "浏览作者资料") [ ](http://my.iteye.com/messages/new?message%5Breceiver_name%5D=denger "发送站内短信") [ ](http://denger.iteye.com/blog/guest_book "给作者留言") [ ](http://my.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=denger "关注作者") [回帖地址](http://www.iteye.com/topic/1039052#2096237 "denger回帖:新浪微博如何实现 SSO 的分析")

[0](http://www.iteye.com/topic/1039052# "好") [0](http://www.iteye.com/topic/1039052# "差") 请登录后投票  * fangin
* 等级: 初级会员
* [![fangin的博客]( "fangin的博客: ")](http://fangin.iteye.com/)
* 性别: ![]( "男")
* 文章: 52
* 积分: 30
* 来自: 北京
* ![]()
    发表时间：2011-05-11  

ln1058 写道

我怎么觉得没必要这么麻烦呢。
如果是我来做，直接通过cookie来判断用户是否为登录状态，然后在进行相关的URL跳转。
这种网站对于用户的安全性都不高，真有必要这么麻烦吗？
你不能因为用户禁用cookie就登陆不上去啊 [返回顶楼](http://www.iteye.com/topic/1039052#) [ ](http://fangin.iteye.com/ "浏览作者的博客") [ ](http://fangin.iteye.com/blog/profile "浏览作者资料") [ ](http://my.iteye.com/messages/new?message%5Breceiver_name%5D=fangin "发送站内短信") [ ](http://fangin.iteye.com/blog/guest_book "给作者留言") [ ](http://my.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=fangin "关注作者") [回帖地址](http://www.iteye.com/topic/1039052#2096261 "fangin回帖:新浪微博如何实现 SSO 的分析")

[0](http://www.iteye.com/topic/1039052# "好") [0](http://www.iteye.com/topic/1039052# "差") 请登录后投票  * denger
* 等级: ![四星会员]( "四星会员")
* [![denger的博客]( "denger的博客: Coding for fun.      ")](http://denger.iteye.com/)
* 性别: ![]( "男")
* 文章: 176
* 积分: 400
* 来自: 北京
* ![]()
    发表时间：2011-05-11   最后修改：2011-05-11

fangin 写道

ln1058 写道

我怎么觉得没必要这么麻烦呢。
如果是我来做，直接通过cookie来判断用户是否为登录状态，然后在进行相关的URL跳转。
这种网站对于用户的安全性都不高，真有必要这么麻烦吗？
你不能因为用户禁用cookie就登陆不上去啊
你说的没错，不过遗憾的是 sina当你禁用了cookie你确实登录不上去。貌似连iteye也是这样。 [返回顶楼](http://www.iteye.com/topic/1039052#) [ ](http://denger.iteye.com/ "浏览作者的博客") [ ](http://denger.iteye.com/blog/profile "浏览作者资料") [ ](http://my.iteye.com/messages/new?message%5Breceiver_name%5D=denger "发送站内短信") [ ](http://denger.iteye.com/blog/guest_book "给作者留言") [ ](http://my.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=denger "关注作者") [回帖地址](http://www.iteye.com/topic/1039052#2096274 "denger回帖:新浪微博如何实现 SSO 的分析")

[0](http://www.iteye.com/topic/1039052# "好") [0](http://www.iteye.com/topic/1039052# "差") 请登录后投票  * aweber
* 等级: 初级会员
* [![aweber的博客]( "aweber的博客: ")](http://aweber.iteye.com/)
* 性别: ![]( "男")
* 文章: 3
* 积分: 30
* 来自: 福建
* ![]()
    发表时间：2011-05-11  

亮点是 iframe调用父窗口js函数的解决方法。居然在当前域名下整了一个跳转页面，在跳转页面里面调用父窗口地址。NB。
我之前都是 在子iframe里面再包含一个iframe，简称孙iframe，孙iframe和父窗口同域名，也就是跳转页面，在孙iframe里面调用父窗口的函数 [返回顶楼](http://www.iteye.com/topic/1039052#) [ ](http://aweber.iteye.com/ "浏览作者的博客") [ ](http://aweber.iteye.com/blog/profile "浏览作者资料") [ ](http://my.iteye.com/messages/new?message%5Breceiver_name%5D=aweber "发送站内短信") [ ](http://aweber.iteye.com/blog/guest_book "给作者留言") [ ](http://my.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=aweber "关注作者") [回帖地址](http://www.iteye.com/topic/1039052#2096367 "aweber回帖:新浪微博如何实现 SSO 的分析")

[0](http://www.iteye.com/topic/1039052# "好") [0](http://www.iteye.com/topic/1039052# "差") 请登录后投票  * xiang37
* 等级: 初级会员
* [![xiang37的博客]( "xiang37的博客: 一步既出，无需罣碍，前行便是")](http://xiva.iteye.com/)
* 性别: ![]( "男")
* 文章: 22
* 积分: 40
* 来自: 南京
* ![]()
    发表时间：2011-05-11  

支持一个，最近在研究sso，现在想先做个cas的实例出来。 [返回顶楼](http://www.iteye.com/topic/1039052#) [ ](http://xiva.iteye.com/ "浏览作者的博客") [ ](http://xiva.iteye.com/blog/profile "浏览作者资料") [ ](http://my.iteye.com/messages/new?message%5Breceiver_name%5D=xiang37 "发送站内短信") [ ](http://xiva.iteye.com/blog/guest_book "给作者留言") [ ](http://my.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=xiang37 "关注作者") [回帖地址](http://www.iteye.com/topic/1039052#2096404 "xiang37回帖:新浪微博如何实现 SSO 的分析")

[0](http://www.iteye.com/topic/1039052# "好") [0](http://www.iteye.com/topic/1039052# "差") 请登录后投票  * tengfei3003
* 等级: 初级会员
* [![tengfei3003的博客]( "tengfei3003的博客: tengfei3003")](http://tengfei3003.iteye.com/)
* 性别: ![]( "男")
* 文章: 9
* 积分: 40
* 来自: 南京
* ![]()
    发表时间：2011-05-11  

分析透彻，好好研究研究 [返回顶楼](http://www.iteye.com/topic/1039052#) [ ](http://tengfei3003.iteye.com/ "浏览作者的博客") [ ](http://tengfei3003.iteye.com/blog/profile "浏览作者资料") [ ](http://my.iteye.com/messages/new?message%5Breceiver_name%5D=tengfei3003 "发送站内短信") [ ](http://tengfei3003.iteye.com/blog/guest_book "给作者留言") [ ](http://my.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=tengfei3003 "关注作者") [回帖地址](http://www.iteye.com/topic/1039052#2096616 "tengfei3003回帖:新浪微博如何实现 SSO 的分析")

[0](http://www.iteye.com/topic/1039052# "好") [0](http://www.iteye.com/topic/1039052# "差") 请登录后投票  * jeho0815
* 等级: 初级会员
* [![jeho0815的博客]( "jeho0815的博客: ")](http://jeho0815.iteye.com/)
* 性别: ![]( "男")
* 文章: 21
* 积分: 30
* 来自: 深圳
* ![]()
    发表时间：2011-05-11  

看的有点困难啊，实力太差了！ [返回顶楼](http://www.iteye.com/topic/1039052#) [ ](http://jeho0815.iteye.com/ "浏览作者的博客") [ ](http://jeho0815.iteye.com/blog/profile "浏览作者资料") [ ](http://my.iteye.com/messages/new?message%5Breceiver_name%5D=jeho0815 "发送站内短信") [ ](http://jeho0815.iteye.com/blog/guest_book "给作者留言") [ ](http://my.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=jeho0815 "关注作者") [回帖地址](http://www.iteye.com/topic/1039052#2097028 "jeho0815回帖:新浪微博如何实现 SSO 的分析")

[0](http://www.iteye.com/topic/1039052# "好") [0](http://www.iteye.com/topic/1039052# "差") 请登录后投票

« 上一页 1 [2](http://www.iteye.com/topic/1039052?page=2) [3](http://www.iteye.com/topic/1039052?page=3) [下一页 »](http://www.iteye.com/topic/1039052?page=2)
[论坛首页](http://www.iteye.com/forums) → [Java企业应用版](http://www.iteye.com/forums/board/Java)
跳转论坛:移动开发技术 Web前端技术 Java企业应用 编程语言技术 综合技术 入门技术 招聘求职 海阔天空

* [吉林: 用友汽车软件诚聘高级Java工程师工作地点：长](http://www.iteye.com/jobs/2137 "吉林:用友汽车软件诚聘高级Java工程师工作地点：长春、北京")
* [广东: 用友汽车软件诚聘中级JAVA工程师 工作地点：](http://www.iteye.com/jobs/2144 "广东:用友汽车软件诚聘中级JAVA工程师 工作地点：广州")
* [上海: 宝尊电商诚聘JAVA中级软件工程师](http://www.iteye.com/jobs/2343 "上海:宝尊电商诚聘JAVA中级软件工程师")
* [北京: 都乐保诚聘Java高级开发工程师](http://www.iteye.com/jobs/2426 "北京:都乐保诚聘Java高级开发工程师")
* [陕西: ThoughtWorks诚聘西安Senior Java Applicatio](http://www.iteye.com/jobs/1144 "陕西:ThoughtWorks诚聘西安Senior Java Application Developer")
* [上海: 用友汽车软件诚聘项目经理](http://www.iteye.com/jobs/2078 "上海:用友汽车软件诚聘项目经理")
* [上海: 爱可生技术诚聘高级JAVA工程师](http://www.iteye.com/jobs/2106 "上海:爱可生技术诚聘高级JAVA工程师")
* [上海: 用友汽车软件诚聘初级java开发工程师（UAP平](http://www.iteye.com/jobs/2141 "上海:用友汽车软件诚聘初级java开发工程师（UAP平台）")
* [上海: 宝尊电商诚聘JAVA高级软件工程师](http://www.iteye.com/jobs/2344 "上海:宝尊电商诚聘JAVA高级软件工程师")
* [浙江: 阿里巴巴诚聘高级Java工程师](http://www.iteye.com/jobs/1628 "浙江:阿里巴巴诚聘高级Java工程师")
* [浙江: 阿里巴巴诚聘C++开发工程师(搜索引擎和广告](http://www.iteye.com/jobs/1849 "浙江:阿里巴巴诚聘C++开发工程师(搜索引擎和广告引擎方向，偏底层)")
* [吉林: 用友汽车软件诚聘java开发工程师 工作地点：](http://www.iteye.com/jobs/2136 "吉林:用友汽车软件诚聘java开发工程师 工作地点：长春、北京")
* [上海: 用友汽车软件诚聘中级java程序员（工作地点：](http://www.iteye.com/jobs/2143 "上海:用友汽车软件诚聘中级java程序员（工作地点：上海安亭）")
* [上海: 用友汽车软件诚聘高级Java程序员](http://www.iteye.com/jobs/2064 "上海:用友汽车软件诚聘高级Java程序员")
* [北京: 都乐保诚聘Java开发工程师](http://www.iteye.com/jobs/2427 "北京:都乐保诚聘Java开发工程师")
* [云南: 云南远信诚聘高级软件工程师(J2EE)](http://www.iteye.com/jobs/2321 "云南:云南远信诚聘高级软件工程师(J2EE) ")
* [上海: 用友汽车软件诚聘高级java开发工程师（UAP平](http://www.iteye.com/jobs/2140 "上海:用友汽车软件诚聘高级java开发工程师（UAP平台）")
* [广东: ★路人甲★诚聘高级Java程序员 广州](http://www.iteye.com/jobs/2437 "广东:★路人甲★诚聘高级Java程序员 广州")
* [云南: 云南远信诚聘软件工程师（Java）](http://www.iteye.com/jobs/2297 "云南:云南远信诚聘软件工程师（Java）")
* [浙江: 全麦电子商务诚聘J2EE系统架构师](http://www.iteye.com/jobs/2358 "浙江:全麦电子商务诚聘J2EE系统架构师")

* [首页](http://www.iteye.com/)
* [资讯](http://www.iteye.com/news)
* [精华](http://www.iteye.com/magazines)
* [论坛](http://www.iteye.com/forums)
* [问答](http://www.iteye.com/ask)
* [博客](http://www.iteye.com/blogs)
* [专栏](http://www.iteye.com/blogs/subjects)
* [群组](http://www.iteye.com/groups)
* [招聘](http://www.iteye.com/job)
* [搜索](http://www.iteye.com/search)
* [广告服务](http://www.iteye.com/index/service)
* [ITeye黑板报](http://webmaster.iteye.com/)
* [联系我们](http://www.iteye.com/index/contactus)
* [友情链接](http://www.iteye.com/index/friend_links)

© 2003-2012 ITeye.com. [ [京ICP证110151号](http://www.miibeian.gov.cn/) 京公网安备110105010620 ]
百联优力(北京)投资有限公司 版权所有 ![]()
