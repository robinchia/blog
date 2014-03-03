---
layout: post
title: "HttpClient接收数据的问题"
categories: Httpclient
tags: 
 - Httpclient
--- 

# HttpClient接收数据的问题

# [没有的专栏](http://blog.csdn.net/devoteinjava)

##

* [登录](http://passport.csdn.net/UserLogin.aspx)
* [注册](http://passport.csdn.net/CSDNUserRegister.aspx)
* [欢迎](http://hi.csdn.net/)
* [退出](http://writeblog.csdn.net/Signout.aspx)
* [我的博客](http://blog.csdn.net/)
* [配置](http://writeblog.csdn.net/configure.aspx)
* [写文章](http://writeblog.csdn.net/PostEditPlain.aspx)
* [文章管理](http://writeblog.csdn.net/PostList.aspx)
* [博客首页](http://blog.csdn.net/)

*
* 全站 当前博客
*

* [空间](http://hi.csdn.net/devoteinjava)
* [博客](http://blog.csdn.net/devoteinjava)
* [好友](http://hi.csdn.net/!s/friend/list/devoteinjava)
* [相册](http://hi.csdn.net/!s/album/list/devoteinjava)
* [留言](http://hi.csdn.net/!s/wall/to/devoteinjava)

用户操作[[留言]](http://hi.csdn.net/!s/wall/to/devoteinjava)  [[发消息]](http://hi.csdn.net/!s/msg/to/devoteinjava)  [[加为好友]](http://hi.csdn.net/!s/friend/add/devoteinjava) 订阅我的博客[![XML聚合]()](http://feeds.feedsky.com/csdn.net/devoteinjava)   [![FeedSky]()](http://feeds.feedsky.com/csdn.net/devoteinjava)[![订阅到鲜果]()](http://www.xianguo.com/subscribe.php?url=http://feeds.feedsky.com/csdn.net/devoteinjava)[![订阅到Google]()](http://fusion.google.com/add?feedurl=http://feeds.feedsky.com/csdn.net/devoteinjava)[![订阅到抓虾]()](http://www.zhuaxia.com/add_channel.php?url=http://feeds.feedsky.com/csdn.net/devoteinjava)devoteinjava的公告文章分类存档* [2008年01月(1)](http://blog.csdn.net/devoteinjava/archive/2008/01.aspx)
* [2007年06月(5)](http://blog.csdn.net/devoteinjava/archive/2007/06.aspx)
* [2007年05月(1)](http://blog.csdn.net/devoteinjava/archive/2007/05.aspx)
* [2007年04月(7)](http://blog.csdn.net/devoteinjava/archive/2007/04.aspx)
* [2006年09月(1)](http://blog.csdn.net/devoteinjava/archive/2006/09.aspx)
* [2006年08月(2)](http://blog.csdn.net/devoteinjava/archive/2006/08.aspx)
* [2006年05月(3)](http://blog.csdn.net/devoteinjava/archive/2006/05.aspx)
* [2006年04月(4)](http://blog.csdn.net/devoteinjava/archive/2006/04.aspx)
* [2006年03月(7)](http://blog.csdn.net/devoteinjava/archive/2006/03.aspx)
* [2006年02月(3)](http://blog.csdn.net/devoteinjava/archive/2006/02.aspx)
* [2006年01月(5)](http://blog.csdn.net/devoteinjava/archive/2006/01.aspx)
* [2005年12月(2)](http://blog.csdn.net/devoteinjava/archive/2005/12.aspx)
* [2005年11月(1)](http://blog.csdn.net/devoteinjava/archive/2005/11.aspx)
* [2005年10月(4)](http://blog.csdn.net/devoteinjava/archive/2005/10.aspx)
* [2005年09月(7)](http://blog.csdn.net/devoteinjava/archive/2005/09.aspx)
* [2005年08月(11)](http://blog.csdn.net/devoteinjava/archive/2005/08.aspx)
* [2005年07月(2)](http://blog.csdn.net/devoteinjava/archive/2005/07.aspx)
# ![原创]()  HttpClient接收数据的问题 [收藏]( "收藏到我的网摘中，并分享给我的朋友")

背景：
竞价大厅和支持系统进行项目的部署和竞价数据回调时，涉及到了传输XML文件。
发送端采用的是HttpClient技术，将XML格式的String作为Http的Body发送。

原先的做法：
接收端通过Jsp的request对象的getInputStream()方法获取输入流，然后用read()方法,将输入流按照byte读取，存入byte[]，然后还原成对应编码的String，并进行解析。

大厅升级时，遇到的问题：
由于涉及多产品，发送端的数据量大大增加。如果发现String长度如果超过4K多，就会报告XML解析报错。
逐一调试分析，发现错误的核心是inputstream.read()方法,该方法只接收到了4096个byte。
使用inputstream.read(byte[], int off, int len )方法指定长度也不行，还是只读4096个字节。

解决：
增加一个缓冲，采用BufferedReader读取inputstream，然后针对BufferedReader逐行读取，则接收完全正常。

原因分析:
没发现inputStream读取有长度限制；写了个读file的Demo测试inputStream的read方法，再次确定不存在长度的问题。
对inputStream作2次read（），发现第二次是从第4097个字节开始往下读，看来问题和缓冲相关。
翻翻HttpClient的源代码，基本确定原因是：HttpClient的传输协议是对Socket进行了封装，而默认Socket的缓冲大小4096。Socket缓冲满了就flush,发送给客户端，那么如果接收端没有用缓冲接收，一次最大只能解收到4096个字节。

发表于 @ 2005年10月31日　15:28:00 | [评论( loading...  )](http://blog.csdn.net/devoteinjava/archive/2005/10/31/520014.aspx#FeedBack "评论")| [编辑](http://writeblog.csdn.net/PostEdit.aspx?entryId=520014 "编辑")| [举报](mailto:yuexn@csdn.net?subject=Article%20Report!!!&body=Author:devoteinjava%0D%0AURL:http://blog.csdn.net/ArticleContent.aspx?UserName=devoteinjava&Entryid=520014)| [收藏]( "收藏到我的网摘中，并分享给我的朋友")

### [旧一篇:产品列表完毕](http://blog.csdn.net/devoteinjava/archive/2005/10/21/510671.aspx) | [新一篇:出差记事](http://blog.csdn.net/devoteinjava/archive/2005/11/25/536796.aspx)[]()

Copyright © devoteinjavaPowered by CSDN Blog![]()
