---
layout: post
title: "CSS经典实用技巧10招 - 南昌分站专版 - CSDN_net程序员地方俱乐部 - Powered"
categories: CSS
tags: 
 - CSS
--- 

# CSS经典实用技巧10招 - 南昌分站专版 - CSDN_net程序员地方俱乐部 - Powered by Discuz!

[CSDN首页](http://www.csdn.net/)| [空间](http://hi.csdn.net/)| [新闻](http://news.csdn.net/)| [论坛](http://bbs.csdn.net/)| [群组](http://groups.csdn.net/)| [BLOG](http://blog.csdn.net/)| [文档](http://dev.csdn.net/)| [下载](http://download.csdn.net/)| [读书](http://book.csdn.net/)| [网摘](http://wz.csdn.net/)| [视频](http://live.csdn.net/)| [开源](http://gforge.osdn.net.cn/)| [书店](http://www.dearbook.com.cn/)| [程序员](http://www.programmer.com.cn/)| [项目交易](http://prj.csdn.net/)| [培训](http://training.csdn.net/)

## [![CSDN.net程序员地方俱乐部]()](http://club.csdn.net/index.php "CSDN.net程序员地方俱乐部")

[注册](http://club.csdn.net/register.php) [登录](http://club.csdn.net/logging.php?action=login)

* [会员](http://club.csdn.net/member.php?action=list)
* [标签](http://club.csdn.net/tag.php)
* [统计](http://club.csdn.net/stats.php)
* [帮助](http://club.csdn.net/faq.php)
[CSDN.net程序员地方俱乐部](http://club.csdn.net/index.php) » [南昌分站专版](http://club.csdn.net/forumdisplay.php?fid=25&page=1) » CSS经典实用技巧10招

[‹‹ 上一主题](http://club.csdn.net/redirect.php?fid=25&tid=5620&goto=nextoldset) | [下一主题 ››](http://club.csdn.net/redirect.php?fid=25&tid=5620&goto=nextnewset)[![发新话题]( "发新话题")](http://club.csdn.net/post.php?action=newthread&fid=25&extra=page%3D1) [![]()](http://club.csdn.net/post.php?action=reply&fid=25&tid=5620&extra=page%3D1)

* [发新话题](http://club.csdn.net/post.php?action=newthread&fid=25&extra=page%3D1)
* [发布投票](http://club.csdn.net/post.php?action=newthread&fid=25&extra=page%3D1&special=1)
* [发布商品](http://club.csdn.net/post.php?action=newthread&fid=25&extra=page%3D1&special=2)
* [发布悬赏](http://club.csdn.net/post.php?action=newthread&fid=25&extra=page%3D1&special=3)
* [发布活动](http://club.csdn.net/post.php?action=newthread&fid=25&extra=page%3D1&special=4)
* [发布辩论](http://club.csdn.net/post.php?action=newthread&fid=25&extra=page%3D1&special=5)
* [发布视频](http://club.csdn.net/post.php?action=newthread&fid=25&extra=page%3D1&special=6)
[打印](http://club.csdn.net/viewthread.php?action=printable&tid=5620)

# [[论坛新帖]](http://club.csdn.net/forumdisplay.php?fid=25&filter=type&typeid=33) CSS经典实用技巧10招

[huangruifang](http://club.csdn.net/space.php?uid=3976)

![]()

*新手上路*

![Rank: 1]()

* [发短消息](http://club.csdn.net/viewthread.php?tid=5620&extra=page%3D1###)
* [加为好友](http://club.csdn.net/my.php?item=buddylist&newbuddyid=3976&buddysubmit=yes)
* 当前在线
 **1#** *大* *中* *小* 发表于 2009-12-14 09:10  [只看该作者](http://club.csdn.net/viewthread.php?tid=5620&page=1&authorid=3976)

## CSS经典实用技巧10招

一.使用css缩写
使用缩写可以帮助减少你CSS文件的大小，更加容易阅读。css缩写的主要规则请参看《常用css缩写语法总结》，这里就不展开描述。
二.明确定义单位，除非值为0
忘记定义尺寸的单位是CSS新手普遍的错误。在HTML中你可以只写width="100"，但是在CSS中，你必须给一个准确的单位，比如:width: 100px width:100em。只有两个例外情况可以不定义单位:行高和0值。除此以外，其他值都必须紧跟单位，注意，不要在数值和单位之间加空格。
三.区分大小写
当在XHTML中使用CSS，CSS里定义的元素名称是区分大小写的。为了避免这种错误，我建议所有的定义名称都采用小写。
class和id的值在HTML和XHTML中也是区分大小写的，如果你一定要大小写混合写，请仔细确认你在CSS的定义和XHTML里的标签是一致的。
四.取消class和id前的元素限定
当你写给一个元素定义class或者id，你可以省略前面的元素限定，因为ID在一个页面里是唯一的，而clas s可以在页面中多次使用。你限定某个元素毫无意义。例如:
div#content { }
fieldset.details { }
可以写成
#content { }
.details { }
这样可以节省一些字节。
五.默认值
通常padding的默认值为0，background-color的默认值是transparent。但是在不同的浏览器默认值可能不同。如果怕有冲突，可以在样式表一开始就先定义所有元素的margin和padding值都为0，象这样:
* {
margin:0;
padding:0;
}
六.不需要重复定义可继承的值
CSS中，子元素自动继承父元素的属性值，象颜色、字体等，已经在父元素中定义过的，在子元素中可以直接继承，不需要重复定义。但是要注意，浏览器可能用一些默认值覆盖你的定义。
七.最近优先原则
如果对同一个元素的定义有多种，以最接近(最小一级)的定义为最优先，例如有这么一段代码
Update: Lorem ipsum dolor set
在CSS文件中，你已经定义了元素p，又定义了一个class"update"
p {
margin:1em 0;
font-size:1em;
color:#333;
}
.update {
font-weight:bold;
color:#600;
}
这两个定义中，class="update"将被使用，因为class比p更近。你可以查阅W3C的《 Calculating a selector’s specificity》 了解更多。
八.多重class定义
一个标签可以同时定义多个class。例如:我们先定义两个样式，第一个样式背景为#666;第二个样式有10 px的边框。
.one{width:200px;background:#666;}
.two{border:10px solid #F00;}
在页面代码中，我们可以这样调用
这样最终的显示效果是这个div既有#666的背景，也有10px的边框。是的，这样做是可以的，你可以尝试一下。
九.使用子选择器(descendant selectors)
CSS初学者不知道使用子选择器是影响他们效率的原因之一。子选择器可以帮助你节约大量的class定义。我们来看下面这段代码:
Item 1>
Item 1
Item 1
这段代码的CSS定义是:
div#subnav ul { }
div#subnav ul li.subnavitem { }
div#subnav ul li.subnavitem a.subnavitem { }
div#subnav ul li.subnavitemselected { }
div#subnav ul li.subnavitemselected a.subnavitemselected { }
你可以用下面的方法替代上面的代码
Item 1
Item 1
Item 1
样式定义是:
#subnav { }
#subnav li { }
#subnav a { }
#subnav .sel { }
#subnav .sel a { }
用子选择器可以使你的代码和CSS更加简洁、更加容易阅读。
十.不需要给背景图片路径加引号
为了节省字节，我建议不要给背景图片路径加引号，因为引号不是必须的。例如:
background:url("images
margin:0 auto;
}
但是IE5/Win不能正确显示这个定义，我们采用一个非常有用的技巧来解决:用text-align属性。就象这样:
body {
text-align:center;
} UID 3976  帖子 183  精华 [0](http://club.csdn.net/digest.php?authorid=3976)  积分 0  阅读权限 255  在线时间 26 小时  注册时间 2009-11-25  最后登录 2009-12-15 

[查看详细资料](http://club.csdn.net/space.php?uid=3976) **TOP**

[]()[http://club.csdn.net/space.php?uid=4129](http://club.csdn.net/space.php?uid=4129)

![]()

*新手上路*

![Rank: 1]()

* [发短消息](http://club.csdn.net/viewthread.php?tid=5620&extra=page%3D1###)
* [加为好友](http://club.csdn.net/my.php?item=buddylist&newbuddyid=4129&buddysubmit=yes)
* 当前离线
 **2#** *大* *中* *小* 发表于 2009-12-14 13:46  [只看该作者](http://club.csdn.net/viewthread.php?tid=5620&page=1&authorid=4129)

## 很好

总结的很好！ UID 4129  帖子 1  精华 [0](http://club.csdn.net/digest.php?authorid=4129)  积分 0  阅读权限 255  在线时间 0 小时  注册时间 2009-12-14  最后登录 2009-12-14 

[查看详细资料](http://club.csdn.net/space.php?uid=4129) **TOP**
[skyscraping](http://club.csdn.net/space.php?uid=4140)

![]()

*新手上路*

![Rank: 1]()

* [发短消息](http://club.csdn.net/viewthread.php?tid=5620&extra=page%3D1###)
* [加为好友](http://club.csdn.net/my.php?item=buddylist&newbuddyid=4140&buddysubmit=yes)
* 当前离线
 **3#** *大* *中* *小* 发表于 2009-12-14 22:08  [只看该作者](http://club.csdn.net/viewthread.php?tid=5620&page=1&authorid=4140)

总结得非常好，也很有用，学习了。。。 UID 4140  帖子 1  精华 [0](http://club.csdn.net/digest.php?authorid=4140)  积分 0  阅读权限 255  在线时间 0 小时  注册时间 2009-12-14  最后登录 2009-12-14 

[查看详细资料](http://club.csdn.net/space.php?uid=4140) **TOP**

[]()[xiaolong0211](http://club.csdn.net/space.php?uid=3175)

![]()

*新手上路*

![Rank: 1]()

* [发短消息](http://club.csdn.net/viewthread.php?tid=5620&extra=page%3D1###)
* [加为好友](http://club.csdn.net/my.php?item=buddylist&newbuddyid=3175&buddysubmit=yes)
* 当前离线
 **4#** *大* *中* *小* 发表于 2009-12-14 22:16  [只看该作者](http://club.csdn.net/viewthread.php?tid=5620&page=1&authorid=3175)

学习了，谢谢~~ UID 3175  帖子 2  精华 [0](http://club.csdn.net/digest.php?authorid=3175)  积分 0  阅读权限 255  在线时间 0 小时  注册时间 2009-8-21  最后登录 2009-12-14 

[查看详细资料](http://club.csdn.net/space.php?uid=3175) **TOP**
[‹‹ 上一主题](http://club.csdn.net/redirect.php?fid=25&tid=5620&goto=nextoldset) | [下一主题 ››](http://club.csdn.net/redirect.php?fid=25&tid=5620&goto=nextnewset)[![发新话题]( "发新话题")](http://club.csdn.net/post.php?action=newthread&fid=25&extra=page%3D1) [![]()](http://club.csdn.net/post.php?action=reply&fid=25&tid=5620&extra=page%3D1)

* [控制面板首页](http://club.csdn.net/memcp.php)
* [编辑个人资料](http://club.csdn.net/memcp.php?action=profile)
* [积分交易](http://club.csdn.net/memcp.php?action=credits)
* [积分记录](http://club.csdn.net/memcp.php?action=creditslog)
* [公众用户组](http://club.csdn.net/memcp.php?action=usergroups)
当前时区 GMT+8, 现在时间是 2009-12-15 09:34

[清除 Cookies](http://club.csdn.net/member.php?action=clearcookies&formhash=23178fa1) - [联系我们](mailto:admin@your.com) - [CSDN.NET](http://www.csdn.net/) - [Archiver](http://club.csdn.net/archiver/) - [WAP](http://club.csdn.net/wap/) - TOP
[![Discuz!]()](http://www.discuz.net/ "Powered by Discuz!")

Powered by **[Discuz!](http://www.discuz.net/)** *6.1.0* © 2001-2008 [Comsenz Inc.](http://www.comsenz.com/)

Processed in 0.028199 second(s), 7 queries.
