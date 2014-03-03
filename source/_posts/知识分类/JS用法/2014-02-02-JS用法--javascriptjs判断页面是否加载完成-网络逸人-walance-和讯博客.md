---
layout: post
title: "javascript js 判断页面是否加载完成 - 网络逸人 - walance - 和讯博客"
categories: JS用法
tags: 
 - JS用法
--- 

# javascript js 判断页面是否加载完成 - 网络逸人 - walance - 和讯博客

http://hexun.com/walance > [复制]() > [收藏]() | [手机看个人门户](http://hexuncom.blog.hexun.com/2029948_d.html)

[和讯博客](http://blog.hexun.com/) | [和讯首页](http://www.hexun.com/)

网络逸人

56en
[个人门户](http://hexun.com/walance/default.html)

[博客](http://walance.blog.hexun.com/)
[相册](http://walance.photo.hexun.com/)

[音乐](http://walance.music.hexun.com/)
[转帖](http://bookmark.hexun.com/walance/default.aspx)

[博揽](http://rss.hexun.com/walance/default.aspx)
[邮箱](http://mail.hexun.com/)

[朋友圈](http://hexun.com/walance/circle.html)
[好友](http://hexun.com/walance/friends.html)

[留言](http://hexun.com/walance/messageboard.html)

[进入我的家](http://i.hexun.com/myhome.html)

载入中

自定义HTML载入中... ![loading]()

快速链接

[[和讯博客]](http://blog.hexun.com/)

[[发表文章]](http://post.blog.hexun.com/walance/postarticle.aspx)
[[博客设置]](http://i.hexun.com/manage/admin_bloggeneral.aspx)

[[文章管理]](http://post.blog.hexun.com/i/myarticles.aspx?blogname=walance)
[搜索]()

[![RSS 2.0]()](http://walance.blog.hexun.com/rss2.aspx)
[![使用和讯博揽订阅]()](http://rss.hexun.com/sub.aspx?fromurl=http://walance.blog.hexun.com/rss2.aspx)
[![博客印书]()](http://i.hexun.com/yinba/index.aspx?blog=http%3a%2f%2fwalance.blog.hexun.com)

javascript js 判断页面是否加载完成 [转贴 2009-02-05 11:39:05] [![]()](http://post.blog.hexun.com/walance/postarticle.aspx?articleid=28923705 "编辑...") ![]( "删除...") 

[![我顶]()](http://cache-sidebar.blog.hexun.com/voteArticle.aspx?articleID=28923705&blogname=walance&blogid=69116 "给这篇文章投一票") 字号：大 中 小

之前看别人用的是document.onreadystatechange的方法来监听状态改变，然后用document.readyState == “complete”判断是否加载完成，代码如下：
document.onreadystatechange = subSomething;//当页面加载状态改变的时候执行这个方法.
function subSomething()
{
if(document.readyState == “complete”) //当页面加载状态为完全结束时进入
myform.submit(); //这是你的操作
} 

这种办法对一些页面简单，DOM较少的页面是好的解决方案，但是最近页面里面有嵌入flash，发现flash有自己载入东西的功能，跟页面无关，所以用之前的办法行不通啦，自己变通了一下，用setInterval来监听事件。

代码如下：

var start;
window.onload = function () {
 start = setInterval(’updateImg()’, 1000);
}
function updateImg() {
 if (document.readyState == “complete”) {
    try{
           …
           clearInterval(start);//执行成功，清除监听
    }catch(err){return true;}
 }
}

—————-2008-12-4—————
 改正一下，因为之前没有测试firefox的，所以并没有发现firefox不支持document.readyState == “complete”，firefox的加载完成事件用window.onload就可以了，所以这个代码修改如下：

var start;
window.onload = function () {
  if(document.all) {//简单判断是否是IE
    start = setInterval(’updateImg()’, 1000);
  } else {
    …//要执行的语句
  }
}
function updateImg() {
 if (document.readyState == “complete”) {
    try{
           …
           clearInterval(start);//执行成功，清除监听
    }catch(err){return true;}
 }
}

标签: [JavaScript](http://blog.hexun.com/group/commontag.aspx?searchTag=JavaScript) [![进入JavaScript吧]()](http://bar.hexun.com/PostSearchNew.aspx?sw=JavaScript&radiobutton=1)  .
所属版块[![]()](http://blogeditor.blog.hexun.com/13880302_d.html): [科技![]()](http://blog.hexun.com/class23.htm)

[[收藏到我的转帖]]() [[引用通告]](http://walance.blog.hexun.com/showtrackback.aspx?articleid=28923705)

[[推荐]](http://post.blog.hexun.com/walance/RecommendArticle.aspx?articleid=28923705&blogid=69116) [[评论]](http://walance.blog.hexun.com/28923705_d.html#comment) [[举报]](http://hexun.com/report.aspx?userid=2333141&type=3&Url=http%3a%2f%2fwalance.blog.hexun.com%2f28923705_d.html) [[打印]]()
点击数:   评论数: [](http://comment.blog.hexun.com/i/comments.aspx?blogname=walance&articleid=28923705)
[我 顶](http://cache-sidebar.blog.hexun.com/voteArticle.aspx?articleID=28923705&blogname=walance&blogid=69116) 
 ！觉得精彩就顶一下，顶的多了，文章将出现在更重要的位置上。

下一篇: [JS操作html时childNodes的替代方法[兼容IE与...](http://walance.blog.hexun.com/29318415_d.html)

上一篇: [修改2003系统默认上传文件大小](http://walance.blog.hexun.com/28900025_d.html)
[]()

发表评论

大 名:  [[登录](http://reg.hexun.com/login.aspx?gourl=http://walance.blog.hexun.com/28923705_d.html)]  [[注册成为和讯用户](http://reg.hexun.com/Register.aspx?fromhost=HX_BLOG&backurl=http://post.blog.hexun.com/registerblog.aspx)]

(不填写则显示为匿名者)
网 址:

(您的网址，可以不填)
标 题:

内 容:
字数上限为2000字

请根据下图中的字符输入验证码：

0
![验证码]()[点这里显示验证码。]()
(您的评论将有可能审核后才能发表)
[和讯个人门户](http://blog.hexun.com/) v1.0 | [和讯家园](http://i.hexun.com/default.html) | [意见反馈](http://group.hexun.com/hexunjiayuan/default.html)
