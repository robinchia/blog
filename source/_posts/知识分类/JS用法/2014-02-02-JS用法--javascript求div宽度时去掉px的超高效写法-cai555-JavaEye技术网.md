---
layout: post
title: "javascript 求div宽度时去掉px的超高效写法 - cai555 - JavaEye技术网"
categories: JS用法
tags: 
 - JS用法
--- 

# javascript 求div宽度时去掉px的超高效写法 - cai555 - JavaEye技术网站

[首页](http://www.javaeye.com/) [新闻](http://www.javaeye.com/news) [论坛](http://www.javaeye.com/forums) [博客](http://www.javaeye.com/blogs) [招聘](http://www.javaeye.com/job) [更多 ▼](http://cai555.javaeye.com/blog/187530#)

[问答](http://www.javaeye.com/ask) [知识库](http://www.javaeye.com/wiki) [圈子](http://www.javaeye.com/groups) [搜索](http://www.javaeye.com/google_search)

[您还未登录 !](http://cai555.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://cai555.javaeye.com/login) [注册](http://cai555.javaeye.com/signup)

# [cai555](http://cai555.javaeye.com/)

永久域名 [http://cai555.javaeye.com/](http://cai555.javaeye.com/)

[javascript获得DOM元素X,Y坐标的函数](http://cai555.javaeye.com/blog/188334 "javascript获得DOM元素X,Y坐标的函数") | [使用telnetd java类库制作守护线程](http://cai555.javaeye.com/blog/182020 "使用telnetd java类库制作守护线程")

2008-04-28

### [javascript 求div宽度时去掉px的超高效写法](http://cai555.javaeye.com/blog/187530)

关键字: javascript 求div宽度时去掉px的超高效写法
<html> <head> <style type="text/css"> div#mydiv { width: 100px; height: 100px; background-color: red; } </style> <script language="JavaScript"> function addWidth() { var mydiv = document.getElementById("mydiv"); alert(mydiv.style.width+","+parseInt(mydiv.style.width)); var curr_width = parseInt(mydiv.style.width); // removes the "px" at the end mydiv.style.width = (curr_width + 10) +"px"; } </script> </head> <body> <input type="button" value="Make Bigger" onclick="addWidth()" /> <div id="mydiv" style="width:100px"></div> </body> </html>

主要是这句： var curr_width = parseInt(mydiv.style.width); // removes the "px" at the end

是不是很有意思？

[javascript获得DOM元素X,Y坐标的函数](http://cai555.javaeye.com/blog/188334 "javascript获得DOM元素X,Y坐标的函数") | [使用telnetd java类库制作守护线程](http://cai555.javaeye.com/blog/182020 "使用telnetd java类库制作守护线程")
* 13:49
* 浏览 (1111)
* [评论](http://cai555.javaeye.com/blog/187530#comments) (0)
* [相关推荐](http://www.javaeye.com/wiki/topic/187530)

### 评论

[]()
### 发表评论

您还没有登录，请[登录](http://cai555.javaeye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![cai555的博客]( "cai555的博客: cai555")](http://cai555.javaeye.com/)

cai555

* 浏览: 81308 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 杭州
* ![]()
* [详细资料](http://cai555.javaeye.com/blog/profile) [留言簿](http://cai555.javaeye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://cai555.javaeye.com/blog/user_visits)

[![ymfans的博客]( "ymfans的博客: ymfans")](http://ymfans.javaeye.com/)

[ymfans](http://ymfans.javaeye.com/)

[![yzjklove的博客]( "yzjklove的博客: yzjklove")](http://yzjklove.javaeye.com/)

[yzjklove](http://yzjklove.javaeye.com/)
[![o_hello的博客]( "o_hello的博客: ")](http://o-hello.javaeye.com/)

[o_hello](http://o-hello.javaeye.com/)

[![qq807215177的博客]( "qq807215177的博客: ")](http://qq807215177.javaeye.com/)

[qq807215177](http://qq807215177.javaeye.com/)

### 博客分类

* [全部博客 (237)](http://cai555.javaeye.com/)
* [OpenCms (38)](http://cai555.javaeye.com/category/26348)
* [Java-Excel报表开发POI系列 (2)](http://cai555.javaeye.com/category/34020)
* [javascript (28)](http://cai555.javaeye.com/category/34663)
* [panorama (1)](http://cai555.javaeye.com/category/34758)
* [libraryh3lp (1)](http://cai555.javaeye.com/category/35973)
* [Django (2)](http://cai555.javaeye.com/category/41858)
* [java (30)](http://cai555.javaeye.com/category/43355)
* [CSS (2)](http://cai555.javaeye.com/category/43819)
* [php (14)](http://cai555.javaeye.com/category/46689)
* [Python (13)](http://cai555.javaeye.com/category/61534)
* [DataBase (18)](http://cai555.javaeye.com/category/73958)
* [OSGI-equinox (16)](http://cai555.javaeye.com/category/78172)
* [Java中的位运算 (2)](http://cai555.javaeye.com/category/79795)
* [JBPM (1)](http://cai555.javaeye.com/category/85801)
### 我的留言簿 [>>更多留言](http://cai555.javaeye.com/blog/guest_book)

* 你好啊 我把路径中的两个opencms去掉后 在online下图片显示不出来怎么办 ...
-- by [abc19840124](http://cai555.javaeye.com/blog/guest_book#12069)
* 能留个联系方式吗？我的qq是314879089
-- by [dongwushengde](http://cai555.javaeye.com/blog/guest_book#11937)
* 直接把jar包放到你的/WEB-INF/lib下面就可以了，然后就可以引用J-FT ...
-- by [cai555](http://cai555.javaeye.com/blog/guest_book#8065)

### 其他分类

* [我的收藏](http://cai555.javaeye.com/blog/favorite) (147)
* [我的论坛帖子](http://cai555.javaeye.com/blog/forum) (46)
* [我的精华良好贴](http://cai555.javaeye.com/blog/article) (0)
### 最近加入圈子

### 存档

* [2009-12](http://cai555.javaeye.com/blog/monthblog/2009-12) (3)
* [2009-11](http://cai555.javaeye.com/blog/monthblog/2009-11) (9)
* [2009-10](http://cai555.javaeye.com/blog/monthblog/2009-10) (7)
* [更多存档...](http://cai555.javaeye.com/blog/monthblog_more)
### 最新评论

* [php读取二进制流（C语言 ...](http://cai555.javaeye.com/blog/354411#comments "php读取二进制流（C语言结构体struct数据文件）")
                        
-- by [liexusong](http://liexusong.javaeye.com/)
* [Java compiler level does ...](http://cai555.javaeye.com/blog/468409#comments "Java compiler level does not match the version of the installed Java project fac")
谢谢！在项目上右键properties-->Project Facets可 ...
-- by [JustDoNow](http://justdonow.javaeye.com/)
* [POI实现插入一行操作，就 ...](http://cai555.javaeye.com/blog/202963#comments "POI实现插入一行操作，就像office excel插入一行的操作")
Util 哪个util啊
-- by [ftp51423121](http://ftp51423121.javaeye.com/)
* [Opencms 静态导出子路径的 ...](http://cai555.javaeye.com/blog/256856#comments "Opencms 静态导出子路径的设置")
正是我要找的，赞一个
-- by [qinghe3012](http://qinghe3012.javaeye.com/)
* [javamail Session.getDefa ...](http://cai555.javaeye.com/blog/212385#comments "javamail Session.getDefaultInstance和getInstance的区别")
好  谢谢了
-- by [c8234933](http://c8234933.javaeye.com/)

### 评论排行榜

* [让 php curl_exec直接返回字符串](http://cai555.javaeye.com/blog/351750 "让 php curl_exec直接返回字符串")
* [IE6环境下遭遇winow.location.href=''的跳 ...](http://cai555.javaeye.com/blog/356626 "IE6环境下遭遇winow.location.href=''的跳转bug")
* [php读取二进制流（C语言结构体struct数据 ...](http://cai555.javaeye.com/blog/354411 "php读取二进制流（C语言结构体struct数据文件）")
* [Java compiler level does not match the v ...](http://cai555.javaeye.com/blog/468409 "Java compiler level does not match the version of the installed Java project fac")
* [P2P之UDP穿透NAT的原理与实现(附源代码 ...](http://cai555.javaeye.com/blog/346565 "P2P之UDP穿透NAT的原理与实现(附源代码)")
* [![Rss]()](http://cai555.javaeye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://cai555.javaeye.com/rss)
* [![Rss_zhuaxia]()](http://www.zhuaxia.com/add_channel.php?url=http://cai555.javaeye.com/rss)
* [![Rss_xianguo]()](http://www.xianguo.com/subscribe.php?url=http://cai555.javaeye.com/rss)
* [[什么是RSS?]](http://www.google.com/search?hl=zh-CN&q=RSS)
声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2009 JavaEye.com. All rights reserved. 上海炯耐计算机软件有限公司 [ 沪ICP备05023328号 ]
