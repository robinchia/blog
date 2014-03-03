---
layout: post
title: "一个javascriptvoid(0)引发的异常 - Tech - JavaEye论坛"
categories: JS用法
tags: 
 - JS用法
--- 

# 一个javascriptvoid(0)引发的异常 - Tech - JavaEye论坛

[您还未登录 !](http://www.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://www.javaeye.com/login) [注册](http://www.javaeye.com/signup)

[![JavaEye-最棒的软件开发交流社区]( "JavaEye-最棒的软件开发交流社区")](http://www.javaeye.com/)

![]()

[论坛首页](http://www.javaeye.com/forums) → [综合技术版](http://www.javaeye.com/forums/board/Tech) →

# 一个javascript:void(0)引发的异常

[全部](http://www.javaeye.com/forums/board/Tech) [Database](http://www.javaeye.com/forums/tag/Database) [Haskell](http://www.javaeye.com/forums/tag/Haskell) [Erlang](http://www.javaeye.com/forums/tag/Erlang) [FP](http://www.javaeye.com/forums/tag/FP) [Linux](http://www.javaeye.com/forums/tag/Linux) [数据结构和算法](http://www.javaeye.com/forums/tag/algorithm)
浏览 567 次 锁定老贴子 [主题：一个javascript:void(0)引发的异常](http://www.javaeye.com/topic/290261)

精华帖 (0) :: 良好帖 (0) :: 新手帖 (0) :: 隐藏帖 (0) 作者 正文 * classicbride
* 等级: 初级会员
* [![classicbride的博客]( "classicbride的博客: classicbride")](http://classicbride.javaeye.com/)
* 文章: 41
* 积分: 30
* 来自: 北京
* ![]()
 发表时间：2008-12-10

相关文章: [ ](http://www.javaeye.com/topic/290261# "关闭")

* [Broken pipe　/ socket write error异常](http://www.javaeye.com/topic/214030 "Broken pipe　/  socket write error异常")
* [ClientAbortException原因探究](http://www.javaeye.com/topic/31800 "ClientAbortException原因探究")
* [确实冤枉好同志了](http://www.javaeye.com/topic/10392 "确实冤枉好同志了")
推荐圈子: [JSF](http://jsfgroup.group.javaeye.com/)
[更多相关推荐](http://www.javaeye.com/wiki/topic/290261)  当用户登陆时，需要用户输入验证码，在服务器端生成一张内存图像，写入response流中...
当用户点击“看不清楚，换成图片”时，利用javascript重新请求服务器端，生成新的图像... 到了这里万事大吉一切正常，在firefox下，IE7下都没有问题，但是在IE6下问题来了，无法刷新验证码，后台报错：
ClientAbortException:  java.net.SocketException: Connection reset by peer: socket write error
at org.apache.catalina.connector.OutputBuffer.doFlush(OutputBuffer.java:319)
at org.apache.catalina.connector.OutputBuffer.flush(OutputBuffer.java:288)
at org.apache.catalina.connector.CoyoteOutputStream.flush(CoyoteOutputStream.java:98)
at javax.imageio.stream.FileCacheImageOutputStream.flushBefore(FileCacheImageOutputStream.java:212)
at javax.imageio.stream.ImageInputStreamImpl.flush(ImageInputStreamImpl.java:801)
at com.sun.imageio.plugins.jpeg.JPEGImageWriter.write(JPEGImageWriter.java:1002)
google半天，说法很多，有说是因为服务器端数据没有写完时，客户端就停止了引发的... 但一直找不到问题的根本原因，继续google，终于找到原因，首先看看偶的代码
<img id="codeImage" src="validateCode.action" border="0" /> <a id="getValidateCode" href="javascript:void(0);">重新获得验证码</a>
function flashValidateCode(){ var randomStr = random(); $("#codeImage").get(0).src = "validateCode.action?randomStr=" + randomStr; } function random(){ return Math.round(Math.random() * 10000); }
当用户有点击重新获取验证码时，我监听一个onclick事件，请求服务器生成新的图像，而我又写了href="javascript:void(0);"
对了，问题就出在javascript:void(0)，<a></a>本身是一个超链接，它一个接受到了void(0)，就认为请求处理完毕了，垃圾IE6就停止了对服务器端的响应，就造成了上述的问题，所以将代码改成：
<img id="codeImage" src="validateCode.action" border="0" /> <a id="getValidateCode" href="#">重新获得验证码</a>
就o了~ 吼吼~ 这个小问题折磨我很久，日~！！ 
声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。

推荐链接

* [![taobao，赢在淘宝]( "taobao，赢在淘宝")](http://www.javaeye.com/clicks/111) [返回顶楼](http://www.javaeye.com/topic/290261#) [ ](http://classicbride.javaeye.com/ "浏览作者的博客") [ ](http://classicbride.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=classicbride "发送站内短信") [ ](http://classicbride.javaeye.com/blog/guest_book "给作者留言") 最后修改：2008-12-10

[论坛首页](http://www.javaeye.com/forums) → [综合技术版](http://www.javaeye.com/forums/board/Tech)
跳转论坛:Java编程和Java企业应用 Web前端技术：AJAX和RIA 移动编程和手机应用开发 C/C++编程 Ruby编程 Python编程 PHP编程 Flash编程 Microsoft .Net 综合技术 软件开发和项目管理 行业应用 入门讨论 招聘求职 海阔天空

* [首页](http://www.javaeye.com/)
* [新闻](http://www.javaeye.com/news)
* [论坛](http://www.javaeye.com/forums)
* [问答](http://www.javaeye.com/ask)
* [知识库](http://www.javaeye.com/wiki)
* [博客](http://www.javaeye.com/blogs)
* [圈子](http://www.javaeye.com/groups)
* [猎头](http://www.javaeye.com/hunters)
* [招聘](http://www.javaeye.com/job)
* [服务](http://www.javaeye.com/index/service)
* [搜索](http://www.javaeye.com/google_search)
* [Java](http://java.javaeye.com/)
* [Web](http://webapp.javaeye.com/)
* [Ruby](http://ruby.javaeye.com/)
* [Python](http://python.javaeye.com/)
* [敏捷](http://agile.javaeye.com/)
* [MySQL](http://mysql.javaeye.com/)
* [普元](http://primeton.javaeye.com/)
* [Dorado](http://dorado.javaeye.com/)
* [金蝶中间件](http://kingdee.javaeye.com/)
* [图书](http://book.javaeye.com/)
* [广告服务](http://www.javaeye.com/index/service)
* [JavaEye黑板报](http://webmaster.javaeye.com/)
* [关于我们](http://www.javaeye.com/index/aboutus)
* [联系我们](http://www.javaeye.com/index/contactus)
* [友情链接](http://www.javaeye.com/index/friend_links)

© 2003-2009 JavaEye.com. All rights reserved. [ 沪ICP备05023328号 ]
