---
layout: post
title: "html页面中运用CSS为层(div)元素添加滚动条 - CSS - New - JavaEye论坛"
categories: CSS
tags: 
 - CSS
--- 

# html页面中运用CSS为层(div)元素添加滚动条 - CSS - New - JavaEye论坛

[您还未登录 !](http://www.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://www.javaeye.com/login) [注册](http://www.javaeye.com/signup)

[![JavaEye-最棒的软件开发交流社区]( "JavaEye-最棒的软件开发交流社区")](http://www.javaeye.com/)

![]()

[论坛首页](http://www.javaeye.com/forums) → [入门讨论版](http://www.javaeye.com/forums/board/New) → [CSS](http://www.javaeye.com/forums/tag/CSS) →

# html页面中运用CSS为层(div)元素添加滚动条

[全部](http://www.javaeye.com/forums/board/New) [入门技术](http://www.javaeye.com/forums/tag/Newbie)
浏览 2039 次 锁定老贴子 [主题：html页面中运用CSS为层(div)元素添加滚动条]()

精华帖 (0) :: 良好帖 (0) :: 新手帖 (0) :: 隐藏帖 (0) 作者 正文 * sdcyst
* 等级: ![二星会员]( "二星会员")
* [![sdcyst的博客]( "sdcyst的博客: xuxiaoming's blog")](http://sdcyst.javaeye.com/)
* 文章: 71
* 积分: 180
* 来自: 济南
* ![]()
 发表时间：2008-10-21

相关文章: [ ](http://www.javaeye.com/topic/255784# "关闭")

* [详解QQ菜单代码](http://www.javaeye.com/topic/229156 "详解QQ菜单代码")
* [IE6下兼容position:fixed的问题](http://www.javaeye.com/topic/317007 "IE6下兼容position:fixed的问题 ")
* [CSS之border margin padding](http://www.javaeye.com/topic/271099 "CSS之border margin padding")
推荐圈子: [EXT](http://ext.group.javaeye.com/)
[更多相关推荐](http://www.javaeye.com/wiki/topic/255784)

     当在页面中显示大量信息时，为了不使整个页面太长，常常将这些内容放在一个层中，然后再为这个层加上滚动条。这样即显示了全部的信息，又使得页面更加美观。下面是一个简单的带滚动条的层实现(其中涉及的一些CSS样式最好是能够参考一下CSS手册，可以使得整个层更加漂亮)。

 
Html代码

1. <html>  
1.     <head>  
1.     <title>Div Scroll</title>  
1.       
1.     <style type="text/css">  
1.     .scroll {  
1.         width: 50%;                                     /*宽度*/  
1.         height: 200px;                                  /*高度*/  
1.         color: ;                                        /*颜色*/  
1.         font-family: ;                                  /*字体*/  
1.         padding-left: 10px;                             /*层内左边距*/  
1.         padding-right: 10px;                            /*层内右边距*/  
1.         padding-top: 10px;                              /*层内上边距*/  
1.         padding-bottom: 10px;                           /*层内下边距*/  
1.         overflow-x: scroll;                             /*横向滚动条(scroll:始终出现;auto:必要时出现;具体参考CSS文档)*/  
1.         overflow-y: scroll;                             /*竖向滚动条*/  
1.           
1.         scrollbar-face-color: #D4D4D4;                  /*滚动条滑块颜色*/  
1.         scrollbar-hightlight-color: #ffffff;                /*滚动条3D界面的亮边颜色*/  
1.         scrollbar-shadow-color: #919192;                    /*滚动条3D界面的暗边颜色*/  
1.         scrollbar-3dlight-color: #ffffff;               /*滚动条亮边框颜色*/  
1.         scrollbar-arrow-color: #919192;                 /*箭头颜色*/  
1.         scrollbar-track-color: #ffffff;                 /*滚动条底色*/  
1.         scrollbar-darkshadow-color: #ffffff;                /*滚动条暗边框颜色*/  
1.     }  
1.     </style>  
1.       
1.     </head>  
1.       
1.     <body>  
1.     <div align="center">  
1.         <div class="scroll">  
1.             <!--在此添加想要显示的内容 -->  
1.         </div>  
1.     </div>  
1.       
1.     </body>  
1. </html>  
<html> <head> <title>Div Scroll</title> <style type="text/css"> .scroll { width: 50%; /*宽度*/ height: 200px; /*高度*/ color: ; /*颜色*/ font-family: ; /*字体*/ padding-left: 10px; /*层内左边距*/ padding-right: 10px; /*层内右边距*/ padding-top: 10px; /*层内上边距*/ padding-bottom: 10px; /*层内下边距*/ overflow-x: scroll; /*横向滚动条(scroll:始终出现;auto:必要时出现;具体参考CSS文档)*/ overflow-y: scroll; /*竖向滚动条*/ scrollbar-face-color: #D4D4D4; /*滚动条滑块颜色*/ scrollbar-hightlight-color: #ffffff; /*滚动条3D界面的亮边颜色*/ scrollbar-shadow-color: #919192; /*滚动条3D界面的暗边颜色*/ scrollbar-3dlight-color: #ffffff; /*滚动条亮边框颜色*/ scrollbar-arrow-color: #919192; /*箭头颜色*/ scrollbar-track-color: #ffffff; /*滚动条底色*/ scrollbar-darkshadow-color: #ffffff; /*滚动条暗边框颜色*/ } </style> </head> <body> <div align="center"> <div class="scroll"> <!--在此添加想要显示的内容 --> </div> </div> </body> </html>

 
声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。

推荐链接 [返回顶楼](http://www.javaeye.com/topic/255784#) [ ](http://sdcyst.javaeye.com/ "浏览作者的博客") [ ](http://sdcyst.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=sdcyst "发送站内短信") [ ](http://sdcyst.javaeye.com/blog/guest_book "给作者留言")  * 昔日舞曲
* 等级: 初级会员
* [![昔日舞曲的博客]( "昔日舞曲的博客: 昔日舞曲")](http://l-m1985.javaeye.com/)
* 文章: 10
* 积分: 30
* 来自: 武汉
* ![]()
 发表时间：2008-10-22

不错
挺有趣的。。。！！ [返回顶楼](http://www.javaeye.com/topic/255784#) [ ](http://l-m1985.javaeye.com/ "浏览作者的博客") [ ](http://l-m1985.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E6%98%94%E6%97%A5%E8%88%9E%E6%9B%B2 "发送站内短信") [ ](http://l-m1985.javaeye.com/blog/guest_book "给作者留言") [回帖地址](http://www.javaeye.com/topic/255784#706328 "昔日舞曲回帖:html页面中运用CSS为层(div)元素添加滚动条")

[0](http://www.javaeye.com/topic/255784# "好") [0](http://www.javaeye.com/topic/255784# "差") 请登录后投票

[论坛首页](http://www.javaeye.com/forums) → [入门讨论版](http://www.javaeye.com/forums/board/New) → [CSS](http://www.javaeye.com/forums/tag/CSS)
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

© 2003-2009 JavaEye.com. All rights reserved. [ 沪ICP备05023328号 ] ![]()
