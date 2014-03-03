---
layout: post
title: "git如何merge github forked repository里的代码更新    haoji"
categories: git&github
tags: 
 - git&github
--- 

# git如何merge github forked repository里的代码更新 haojii

# [haojii](http://www.haojii.com/ "haojii")

## 一个专注于技术的IT男

Search

### Main menu

[Skip to primary content](http://www.haojii.com/2011/08/how-to-git-merge-from-forked-repository/#content "Skip to primary content")

[Skip to secondary content](http://www.haojii.com/2011/08/how-to-git-merge-from-forked-repository/#secondary "Skip to secondary content")
* [首页](http://www.haojii.com/ "首页")
* [关于](http://www.haojii.com/about/)
* [玩具](http://www.haojii.com/toys/)

### Post navigation

[← Previous](http://www.haojii.com/2011/08/gmail-notification-build-upon-arduino-and-ruby/) [Next →](http://www.haojii.com/2011/08/tip-download-a-bitcomet-only-torrent-file-with-thunder/)

# git如何merge github forked repository里的代码更新?

Posted on [2011/08/20]( "21:49")  by  [Jacky](http://www.haojii.com/author/admin/ "View all posts by Jacky")

问题是这样的，github里有个项目ruby-gmail，我需要从fork自同一个项目的另一个repository拿一些Bug Fix的代码
link1：[https://github.com/dcparker/ruby-gmail](https://github.com/dcparker/ruby-gmail) （原作者dcparker的repository）
link2：[https://github.com/jihao/ruby-gmail](https://github.com/jihao/ruby-gmail) （我从link1 fork的repository）
link3：[https://github.com/geoffyoungs/ruby-gmail](https://github.com/geoffyoungs/ruby-gmail) （geoffyoungs 从link1 fork的repository，然后他有些Bug修改，但是没被merge回原作者的link1的repository）

也就我git clone repository到本地后，发现link3有我想要的代码，我要把link3上的改动merge到我的repository上，避免我花精力改相同的bug

**git如何merge github forked repository里的更新?**
**具体做法是下面三步，以前没用git这么搞过，知道之后其实蛮简单**
**
1. >git remote add geoffyoungs http://github.com/geoffyoungs/ruby-gmail.git    2. >git fetch geoffyoungs    3. >git merge geoffyoungs/master
**

本地的repository看上去是这样的：

>git remote -v    geoffyoungs http://github.com/geoffyoungs/ruby-gmail.git (fetch)    geoffyoungs http://github.com/geoffyoungs/ruby-gmail.git (push)    origin http://jihao@github.com/jihao/ruby-gmail.git (fetch)    origin http://jihao@github.com/jihao/ruby-gmail.git (push)    >git branch -a    * master    remotes/geoffyoungs/gh-pages    remotes/geoffyoungs/master    remotes/origin/HEAD -> origin/master    remotes/origin/adimircolen-master    remotes/origin/gh-pages    remotes/origin/master
[]()

想要保存喜欢过的文章吗？立即关联或[创建无觅帐号](http://www.wumii.com/reg/binding?ext=true)？

[](http://www.wumii.com/reg/authapp?app=SINA&ext=true&c=false) [](http://www.wumii.com/reg/authapp?app=QQ&ext=true&c=false) [不再提示！]()

[0](http://www.wumii.com/item/6FtFNbPv)
![正在加载推荐文章]()

其他文章：

* [git stash](http://www.haojii.com/2012/04/git-stash/)
* [基于arduino和ruby的gmail邮件提醒](http://www.haojii.com/2011/08/gmail-notification-build-upon-arduino-and-ruby/)
* [空虚,贴一段代码](http://www.haojii.com/2011/05/code-snip/)
* [如何写出可维护的面向对象的JavaScript代码?](http://www.haojii.com/2011/07/javascript-oo/)

[无觅](http://www.wumii.com/widget/relatedItems "无觅相关文章插件")
This entry was posted in [小问题](http://www.haojii.com/category/questions-answers/ "查看 小问题 中的全部文章") and tagged [git](http://www.haojii.com/tag/git/), [github](http://www.haojii.com/tag/github/), [gmail](http://www.haojii.com/tag/gmail/) by [Jacky](http://www.haojii.com/author/admin/). Bookmark the [permalink]( "Permalink to git如何merge github forked repository里的代码更新?").

## 2 thoughts on “git如何merge github forked repository里的代码更新?”

1. ![]()[Christopher Meng](http://cicku.me/) on [2012/07/13 at 13:03](http://www.haojii.com/2011/08/how-to-git-merge-from-forked-repository/comment-page-1/#comment-2619) said:

什么时候直接网页操作就好了。。。
1. ![]()lsff on [2013/01/07 at 10:38](http://www.haojii.com/2011/08/how-to-git-merge-from-forked-repository/comment-page-1/#comment-3914) said:

哇，这技术博客太漂亮了，博主自己弄得么？
### 发表评论 [取消回复](http://www.haojii.com/2011/08/how-to-git-merge-from-forked-repository/#respond)

电子邮件地址不会被公开。 必填项已用 * 标注

姓名 *

电子邮件 *

站点

评论

您可以使用这些 HTML 标签和属性：

<a href="" title=""> <abbr title=""> <acronym title=""> <b> <blockquote cite=""> <cite> <code> <del datetime=""> <em> <i> <q cite=""> <strike> <strong>
[Proudly powered by WordPress](http://wordpress.org/ "Semantic Personal Publishing Platform")

[![无觅相关文章插件，快速提升流量]()](http://www.wumii.com/widget/relatedItems.htm)
