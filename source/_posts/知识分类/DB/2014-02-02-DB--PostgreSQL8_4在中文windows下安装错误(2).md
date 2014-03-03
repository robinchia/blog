---
layout: post
title: "PostgreSQL 8_4 在中文 windows 下安装错误 (2)"
categories: DB
tags: 
 - DB
--- 

# PostgreSQL 8_4 在中文 windows 下安装错误 (2)

# [goldenhawking的专栏](http://blog.csdn.net/goldenhawking)

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

* [空间](http://hi.csdn.net/goldenhawking)
* [博客](http://blog.csdn.net/goldenhawking)
* [好友](http://hi.csdn.net/!s/friend/list/goldenhawking)
* [相册](http://hi.csdn.net/!s/album/list/goldenhawking)
* [留言](http://hi.csdn.net/!s/wall/to/goldenhawking)

用户操作[[留言]](http://hi.csdn.net/!s/wall/to/goldenhawking)  [[发消息]](http://hi.csdn.net/!s/msg/to/goldenhawking)  [[加为好友]](http://hi.csdn.net/!s/friend/add/goldenhawking) 订阅我的博客[![XML聚合]()](http://feeds.feedsky.com/csdn.net/goldenhawking)   [![FeedSky]()](http://feeds.feedsky.com/csdn.net/goldenhawking)[![订阅到鲜果]()](http://www.xianguo.com/subscribe.php?url=http://feeds.feedsky.com/csdn.net/goldenhawking)[![订阅到Google]()](http://fusion.google.com/add?feedurl=http://feeds.feedsky.com/csdn.net/goldenhawking)[![订阅到抓虾]()](http://www.zhuaxia.com/add_channel.php?url=http://feeds.feedsky.com/csdn.net/goldenhawking)goldenhawking的公告文章分类存档* [2009年08月(3)](http://blog.csdn.net/goldenhawking/archive/2009/08.aspx)
* [2008年04月(1)](http://blog.csdn.net/goldenhawking/archive/2008/04.aspx)
* [2008年02月(1)](http://blog.csdn.net/goldenhawking/archive/2008/02.aspx)
# ![原创]()  PostgreSQL 8.4 在中文 windows 下安装错误 [收藏]( "收藏到我的网摘中，并分享给我的朋友")

这个错误是在安装新版的PostgreSQL 8.4时遇到的，安装程序在 windows xp sp3下启动（简体中文），一路Next, 没注意“locale”选项，当时选择了default,next, 导致无法安装完成，提示：

Installation fails with complaint about accessing data\postgresql.conf
发现这个老兄也遇到了：

[http://www.nabble.com/BUG--4785%3A-Installation-fails-to23323059.html#a23323059](http://www.nabble.com/BUG--4785%3A-Installation-fails-to23323059.html#a23323059)

搞半天，最后发现了问题，是PSQL的全文搜索中没有简体中文支持。其实，安装的时候选择"C"就可以了。

哎，看来我和开源数据库最近老扯不清关系。

这是在nabble上的帖子。
En, I have that damm problem sloved.
Are you using a Non-English Version of Windows? for example, JPN, CHS?
If you install your PSQL in an Asian-language windows, be sure that "locale" settings should be setted manualy to a supported language, I choosed "C" for it is the first item in the combo.
this is the errmsg when you're using default local settings:
Running the post-installation/upgrade actions:
Delete the temporary scripts directory...
Write the base directory to the ini file...
Write the version number to the ini file...
Initialising the database cluster (this may take a few minutes)...
Executing cscript
Script exit code: 0
Script output:
Ensuring we can write to the data directory (using cacls):
数据无效。(Means data is unavaliable)
The files belonging to this database system will be owned by user "XXXXXXXX".
This user must also own the server process.
The database cluster will be initialized with locale Chinese_XXXXXXX.936.
initdb: could not find suitable text search configuration for locale Chinese_XXXXXXX.936
The default text search configuration will be set to "simple".
fixing permissions on existing directory D:/XXXXXX... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 32MB
creating configuration files ... ok
creating template1 database in D:/XXXXXXX/base/1 ... ok
initializing pg_authid ... FATAL:  database locale is incompatible with operating system
DETAIL:  The database was initialized with LC_COLLATE "Chinese_XXXXXXXX.936",  which is not recognized by setlocale().
HINT:  Recreate the database with another locale or install the missing locale.
child process exited with exit code 1
initdb: removing contents of data directory "D:/XXXXXX"
Granting service account access to the data directory (using cacls):
处理的目录: D:\XXXXXXXXX(Processed folder:D:\XXXXXXXXX)
initcluster.vbs ran to completion
Script stderr:
Configuring database server startup...
Executing cscript
Script exit code: 0
Script output:
startupcfg.vbs ran to completion
Script stderr:

发表于 @ 2009年08月05日　12:44:00 | [评论( loading...  )](http://blog.csdn.net/goldenhawking/archive/2009/08/05/4411446.aspx#FeedBack "评论")| [编辑](http://writeblog.csdn.net/PostEdit.aspx?entryId=4411446 "编辑")| [举报](mailto:yuexn@csdn.net?subject=Article%20Report!!!&body=Author:goldenhawking%0D%0AURL:http://blog.csdn.net/ArticleContent.aspx?UserName=goldenhawking&Entryid=4411446)| [收藏]( "收藏到我的网摘中，并分享给我的朋友")

### [旧一篇:初试My-SQL](http://blog.csdn.net/goldenhawking/archive/2009/08/05/4411395.aspx)[]()

Copyright © goldenhawkingPowered by CSDN Blog![]()
