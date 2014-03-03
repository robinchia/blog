---
layout: post
title: "Java调用windows下某些程序"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# Java调用windows下某些程序

[首页](http://www.javaeye.com/) [新闻](http://www.javaeye.com/news) [论坛](http://www.javaeye.com/forums) [问答](http://www.javaeye.com/ask) [博客](http://www.javaeye.com/blogs) [招聘](http://www.javaeye.com/job) [更多 ▼](http://nee.javaeye.com/blog/146233#)

[专栏](http://www.javaeye.com/wiki) [文摘](http://www.javaeye.com/articles) [圈子](http://www.javaeye.com/groups) [搜索](http://www.javaeye.com/google_search)

[您还未登录 !](http://nee.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://nee.javaeye.com/login) [注册](http://nee.javaeye.com/signup)

# [Lyon & Carol](http://nee.javaeye.com/)

永久域名 [http://nee.javaeye.com/](http://nee.javaeye.com/)

[JavaScript FSO属性大全](http://nee.javaeye.com/blog/147213 "JavaScript FSO属性大全") | [在Eclipse状态栏上增加JVM内存用量指示器](http://nee.javaeye.com/blog/82247 "在Eclipse状态栏上增加JVM内存用量指示器")

2007-12-05

### [Java调用windows下某些程序](http://nee.javaeye.com/blog/146233)
Java是种跨平台的语言，我们经常碰到需要通过Java调用windows下某些程序。有些第三方厂商如（ANT），也提供了调用windows下可执行程序的方法，但我们往往需要调用一些批处理命令。而Java却不提供。这里，我采用一种变相的调用方法，使得Java能调用批处理命令。
前期准备
Quick Batch File (De)Compiler
将任何BAT、CMD批处理脚本编译为EXE文件。
1、运行exe 文件
Java JDK里已经提供了调用的方法，不在累赘，代码如下。
try { 　　String command = "notepad"; 　　Process child = 　　Runtime.getRuntime().exec(command); 　　} catch (IOException e) 　　{ 　　}
2、运行 bat（批处理） 文件
Java对批处理文件还不支持。刚开始一直在研究Java如何调用批处理文件，始终找不到解决方法。后来只好绕过批处理，考虑如何将批处理转换为exe可执行文件。然后再通过Java调用可执行文件。
在Google上搜索一下，找到Quick Batch File (De)Compiler，可以将任何BAT、CMD批处理脚本编译为EXE文件。使用了一下，果然可以。
Quick Batch File (De)Compiler使用非常简单：
Quickbfc 文件名.bat 文件名.exe（将批处理命令编译为可执行文件）
quickbfd 文件名.exe 文件名.bat（将可执行文件反编译为批处理命令）
然后，我们再按第一种方法通过Java 调用，即可。
本文来自：http://www.linuxpk.com/46886.html

[JavaScript FSO属性大全](http://nee.javaeye.com/blog/147213 "JavaScript FSO属性大全") | [在Eclipse状态栏上增加JVM内存用量指示器](http://nee.javaeye.com/blog/82247 "在Eclipse状态栏上增加JVM内存用量指示器")
* 16:46
* 浏览 (579)
* [评论](http://nee.javaeye.com/blog/146233#comments) (0)
* 分类: [Java技术](http://nee.javaeye.com/category/9074)
* [相关推荐](http://www.javaeye.com/wiki/topic/146233)

### 评论

[]()
### 发表评论

您还没有登录，请[登录](http://nee.javaeye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![kyonee的博客]( "kyonee的博客: Lyon & Carol")](http://nee.javaeye.com/)

kyonee

* 浏览: 15981 次
* 来自: ...
* ![]()
* [详细资料](http://nee.javaeye.com/blog/profile) [留言簿](http://nee.javaeye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://nee.javaeye.com/blog/user_visits)

[![txws.zx的博客]( "txws.zx的博客: ")](http://txws-zx.javaeye.com/)

[txws.zx](http://txws-zx.javaeye.com/)

[![a3x60的博客]( "a3x60的博客: ")](http://a3x60.javaeye.com/)

[a3x60](http://a3x60.javaeye.com/)
[![zhang_yingjie的博客]( "zhang_yingjie的博客: 凤舞九天")](http://zhang-yingjie-qq-com.javaeye.com/)

[zhang_yingjie](http://zhang-yingjie-qq-com.javaeye.com/)

[![shgavin的博客]( "shgavin的博客: ")](http://shgavin.javaeye.com/)

[shgavin](http://shgavin.javaeye.com/)

### 博客分类

* [全部博客 (21)](http://nee.javaeye.com/)
* [Java技术 (10)](http://nee.javaeye.com/category/9074)
* [DBMS (3)](http://nee.javaeye.com/category/9075)
* [Delphi (0)](http://nee.javaeye.com/category/9076)
* [Movie & DVD (0)](http://nee.javaeye.com/category/9077)
* [摄影 (0)](http://nee.javaeye.com/category/9078)
* [下载 (0)](http://nee.javaeye.com/category/9079)
* [其他 (8)](http://nee.javaeye.com/category/9080)
### 我的相册

[![]()](http://nee.javaeye.com/album)1
[共 5 张](http://nee.javaeye.com/album)

### 其他分类

* [我的收藏](http://nee.javaeye.com/blog/favorite) (8)
* [我的论坛主题贴](http://nee.javaeye.com/blog/topic) (0)
* [我的所有论坛贴](http://nee.javaeye.com/blog/post) (1)
* [我的精华良好贴](http://nee.javaeye.com/blog/article) (0)
### 最近加入圈子

### 存档

* [2009-03](http://nee.javaeye.com/blog/monthblog/2009-03) (2)
* [2008-12](http://nee.javaeye.com/blog/monthblog/2008-12) (2)
* [2008-08](http://nee.javaeye.com/blog/monthblog/2008-08) (1)
* [更多存档...](http://nee.javaeye.com/blog/monthblog_more)
### 最新评论

* [Chrome启动参数及地址栏功 ...](http://nee.javaeye.com/blog/350299#comments "Chrome启动参数及地址栏功能")
怎么里面连--app参数都没有说呢？
-- by [徐风子](http://iswind.javaeye.com/)
* [Hibernate读写Blob/Clob ...](http://nee.javaeye.com/blog/48574#comments "Hibernate读写Blob/Clob类型数据")
补充:读写BLOB                                 ...
-- by [kyonee](http://nee.javaeye.com/)

### 评论排行榜
* [![Rss]()](http://nee.javaeye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://nee.javaeye.com/rss)
* [![Rss_xianguo]()](http://www.xianguo.com/subscribe.php?url=http://nee.javaeye.com/rss)
* [[什么是RSS?]](http://www.google.com/search?hl=zh-CN&q=RSS)
声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2010 JavaEye.com. All rights reserved. 上海炯耐计算机软件有限公司 [ 沪ICP备05023328号 ]
