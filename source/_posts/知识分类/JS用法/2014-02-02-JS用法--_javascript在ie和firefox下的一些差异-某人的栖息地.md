---
layout: post
title: "_ javascript在ie和firefox下的一些差异 - 某人的栖息地"
categories: JS用法
tags: 
 - JS用法
--- 

# _ javascript在ie和firefox下的一些差异 - 某人的栖息地

# [某人的栖息地](http://www.ooso.net/)

« [PriadoBlender可支持php-gtk2](http://www.ooso.net/archives/364)

[MySQL Proxy的Alpha版本发布](http://www.ooso.net/archives/366) »

## [javascript在ie和firefox下的一些差异](http://www.ooso.net/archives/362 "Permanent link to javascript在ie和firefox下的一些差异")

2007-07-31 @ 08:40:30 · 作者 [Volcano](http://www.ooso.net/archives/author/volcano/ "Posts by Volcano") · 归类于 [FIREFOX](http://www.ooso.net/category/firefox "View all posts in FIREFOX"), [JAVASCRIPT](http://www.ooso.net/category/javascript "View all posts in JAVASCRIPT")
## 你可能会感兴趣的内容

* [jQuerify书签](http://www.ooso.net/archives/417 "jQuerify书签")
* [用greasemonkey生成土豆的豆单下载清单](http://www.ooso.net/archives/400 "用greasemonkey生成土豆的豆单下载清单")
* [用来看新浪新闻的greasemonkey脚本](http://www.ooso.net/archives/302 "用来看新浪新闻的greasemonkey脚本")
* [Firebug 1.0 beta is out](http://www.ooso.net/archives/268 "Firebug 1.0 beta is out")
* [检测javascript的内存泄漏](http://www.ooso.net/archives/254 "检测javascript的内存泄漏")

javascript在ie和[firefox](http://www.ooso.net/index.php?tag=firefox)下,运行结果有一些差异。下面把最近碰到的情况做个记录，以后也会不断补充以备忘。

## object操作

* firefox:可支持

[PLAIN TEXT](http://www.ooso.net/archives/362#)
CODE:

1. var obj = { 'key' : 'aaa', }
1. ie:不支持

[PLAIN TEXT](http://www.ooso.net/archives/362#)
CODE:

1. var obj = { 'key' : 'aaa', }

会报javascript错误,最后的"**,**"必须去掉

## [javascript](http://www.ooso.net/index.php?tag=javascript)对select元素的option操作
1. firefox:可直接设置

[PLAIN TEXT](http://www.ooso.net/archives/362#)
CODE:

1. option.text = 'foooooooo';
1. ie:只能设置

[PLAIN TEXT](http://www.ooso.net/archives/362#)
CODE:

1. option.innerHTML = 'fooooooo';

## 删除一个select的option
1. firefox:可以

[PLAIN TEXT](http://www.ooso.net/archives/362#)
CODE:

1. select.options.remove(selectedIndex);
1. ie7:可以用

[PLAIN TEXT](http://www.ooso.net/archives/362#)
CODE:

1. select.options[i] = null;
1. ie6:需要写

[PLAIN TEXT](http://www.ooso.net/archives/362#)
CODE:

1. select.options[i].outerHTML = null;

真是万恶的浏览器。

作者: [Volcano](http://www.ooso.net/archives/author/volcano/ "Posts by Volcano") 发表于July 31, 2007 at 8:40 am

[版权信息](http://creativecommons.org/licenses/by/3.0/deed.zh): **可以任意转载, 转载时请务必以超链接形式标明文章[原始出处](http://www.ooso.net/archives/362)和[作者信息](http://www.ooso.net/)及此声明**

[永久链接 - http://www.ooso.net/archives/362](http://www.ooso.net/archives/362 "永久链接到 javascript在ie和firefox下的一些差异")

### Tags: [FIREFOX](http://www.ooso.net/tag/firefox),[ie](http://www.ooso.net/tag/ie),[JAVASCRIPT](http://www.ooso.net/tag/javascript)

## 4 条评论 [»](http://www.ooso.net/archives/362#postcomment "跳转到评论表单")

1. ![]()

### [PerfectWorks](http://www.ttyc.com.cn/) 于 2007-07-31 @ 13:08:25 [留言](http://www.ooso.net/archives/362#comment-14274 "永久链接到本条评论")：

浏览器确实让人很头疼
昨天调试一个程序FF正常IE报错，是因为一个参数列表
opintions = {
opt1 = var1,
}
后面的那个逗号，IE解析不正常……
调试了半小时
1. ![]()

### [volcano](http://www.ooso.net/) 于 2007-07-31 @ 14:18:00 [留言](http://www.ooso.net/archives/362#comment-14276 "永久链接到本条评论")：

感谢补充，我以前也碰到过，只是这会没想起来。
1. ![]()

### [柠檬园主](http://3rgb.com/) 于 2007-11-24 @ 11:41:17 [留言](http://www.ooso.net/archives/362#comment-17901 "永久链接到本条评论")：

所以说，还是用一些好的JS类库省事一些。比如jQuery。
但IE确实是在JS方面恶心到家了，最近在做的一个项目要大量用到JS，
很多时候都是FF不报错的情况下，IE一个劲错，原因是经常少个；之类的。。。。莫名。。。。
1. ![]()

### [阿草哥](http://www.qbencao.com/) 于 2008-04-18 @ 10:25:14 [留言](http://www.ooso.net/archives/362#comment-22182 "永久链接到本条评论")：

这些差异真的郁闷啊 有的开发者平时做的时候只用了IE 到时候发布测试才发现ff也不行 回头再改

[RSS 为此帖反馈评论](http://www.ooso.net/archives/362/feed) · [反向跟踪 网站](http://www.ooso.net/archives/362/trackback)

## 留条评论

作者 (required)

邮箱 (required)

网站

 
* 
## Search
* 
## 本站其它搜索结果
* 
## 分类

* [AJAX](http://www.ooso.net/category/ajax "View all posts filed under AJAX")
* [COMMON](http://www.ooso.net/category/common "View all posts filed under COMMON")
* [FIREFOX](http://www.ooso.net/category/firefox "View all posts filed under FIREFOX")
* [FLASH](http://www.ooso.net/category/flash "View all posts filed under FLASH")
* [GOOGLE](http://www.ooso.net/category/google "View all posts filed under GOOGLE")
* [JAVASCRIPT](http://www.ooso.net/category/javascript "View all posts filed under JAVASCRIPT")
* [MYSQL](http://www.ooso.net/category/mysql "View all posts filed under MYSQL")
* [PEAR](http://www.ooso.net/category/pear "View all posts filed under PEAR")
* [PHP](http://www.ooso.net/category/php "View all posts filed under PHP")
* [WORDPRESS](http://www.ooso.net/category/wordpress "View all posts filed under WORDPRESS")
* 
## 谁来过这里
* 
## Meta

* [Log in](http://www.ooso.net/wp-login.php)
* [RSS Feed](http://feed.ooso.net/ "Syndicate this site using RSS")
* [Friendfeed](https://friendfeed.com/diablo)
* [订阅到 抓虾](http://www.zhuaxia.com/add_channel.php?url=http%3A//feed.ooso.net)
* [订阅到 鲜果](http://www.xianguo.com/subscribe.php?url=http%3A//feed.ooso.net)
* [订阅到 Google Reader](http://www.google.com/reader/preview/*/feed/http://feed.ooso.net)
*

Powered by [**WordPress**](http://wordpress.org/ "Powered by WordPress, state-of-the-art semantic personal publishing platform.")
Copyright © 2002 - 2008 ooso.net
