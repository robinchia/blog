---
layout: post
title: "Git与Github入门资料 ← 阳志平的个人网站  技术 (2)"
categories: git&github
tags: 
 - git&github
--- 

# Git与Github入门资料 ← 阳志平的个人网站 技术 (2)

# [阳志平的个人网站::技术](http://www.yangzhiping.com/tech/ "阳志平的个人网站::技术")

* [文章存档](http://www.yangzhiping.com/tech/past.html)
* [回到首页](http://www.yangzhiping.com/)

# Git与Github入门资料

## Git主要优势及安装

git，一个非常强大的版本管理工具。[Github](http://www.github.com/)则是一个基于Git的日益流行的开源项目托管库。Git与svn的最大区别是，它的使用流程不需要联机，可以先将对代码的修改，评论，保存在本机。等上网之后，再实时推送过去。同时它创建分支与合并分支更容易，推送速度也更快，配合Github提交需求也更容易。

git的入门，稍微有点麻烦，需要在本机创建一个ssh的钥匙，其他的则海空天空了。windows下可以参考[这篇教程](http://hi.baidu.com/mcspring/blog/item/171b1e38986d39fab211c71b.html)，Mac等更多教程则可以参考[Github官方](http://help.github.com/win-set-up-git/)。

## Git全局设置

下载并安装Git
git config --global user.name "Your Name" git config --global user.email youremail@email.com

## 将Ｇit项目与Github建立联系

mkdir yourgithubproject cd yourgithubproject git init touch README git add README git commit -m 'first commit' git remote add origin git@github.com:yourgithubname/yourgithubproject.git git push origin master

## 导入现有的Git仓库

cd existing_git_repo git remote add origin git@github.com:yourgithubname/yourgithubproject.git git push origin master

## 导入现有的Subversion仓库

[点击此处](http://help.github.com/import-from-subversion/)

## git最主要的命令

git --help

The most commonly used git commands are:

add Add file contents to the index bisect Find by binary search the change that introduced a bug branch List, create, or delete branches checkout Checkout a branch or paths to the working tree clone Clone a repository into a new directory commit Record changes to the repository diff Show changes between commits, commit and working tree, etc fetch Download objects and refs from another repository grep Print lines matching a pattern init Create an empty git repository or reinitialize an existing one log Show commit logs merge Join two or more development histories together mv Move or rename a file, a directory, or a symlink pull Fetch from and merge with another repository or a local branch push Update remote refs along with associated objects rebase Forward-port local commits to the updated upstream head reset Reset current HEAD to the specified state rm Remove files from the working tree and from the index show Show various types of objects status Show the working tree status tag Create, list, delete or verify a tag object signed with GPG

## 第一次提交的时候

git push yourgithubproject maste

## 日常提交常用命令

git add . git commit -a -m"some files" git push yourgithubproject

## Textmate的Git Bundles

![参考]( "Textmate的Git Bundles")

## 更多参考

* [Pro Git](http://progit.org/book/)，[中文版](http://progit.org/2010/06/09/pro-git-zh.html)
* [Peepcode的Git教程](http://peepcode.com/products/git)
* [Git cheat sheets](http://help.github.com/git-cheat-sheets/)

本作品采用[知识共享署名-非商业性使用-禁止演绎 3.0 Unported许可协议](http://creativecommons.org/licenses/by-nc-nd/3.0/)进行许可。
25 November 2010

分享到： []( "分享到新浪微博") []( "分享到腾讯微博") []( "分享到人人网") []( "分享到Instapaper") [更多](http://www.jiathis.com/share)
![DISQUS]()![...]()

* [](http://www.yangzhiping.com/tech/git.html# "更多社区信息")
* [Disqus](http://www.yangzhiping.com/tech/git.html#)

* [登录](http://www.yangzhiping.com/tech/git.html#)
* [About Disqus](http://disqus.com/)

* [喜欢](http://www.yangzhiping.com/tech/git.html# "I like this page")
* [Dislike](http://www.yangzhiping.com/tech/git.html# "I don't like this page")
* 
* [![]()](http://disqus.com/loo2k/)
* and 1 other liked this.

### Glad you liked it. Would you like to share?

Facebook

Twitter

* [Share](http://www.yangzhiping.com/tech/git.html#)
* [No thanks](http://www.yangzhiping.com/tech/git.html#)

Sharing this page …
Thanks! [Close](http://www.yangzhiping.com/tech/git.html#)
[登录](http://www.yangzhiping.com/tech/git.html#)

### 添加新的评论

![]()

* 以什么身份发表 …
* Image
*

排序 受欢迎的   排序 best rating   排序 后发表在前   排序 先发表在前

### 显示 2 评论

*
* [![]()](http://disqus.com/google-2f926cdf4c90bb949573659bf911e7a6/)

Huayi Duan  1 comment collapsed  [Collapse](http://www.yangzhiping.com/tech/git.html# "Collapse thread") [Expand](http://www.yangzhiping.com/tech/git.html# "Expand thread")

这个陈述有问题吧。Git与svn的最大区别是，它的使用流程不需要联机，可以先将对代码的修改，评论，保存在本机。等上网之后，再实时推送过去。
* A [喜欢](http://www.yangzhiping.com/tech/git.html#)
* [回复](http://www.yangzhiping.com/tech/git.html#)

* [5 月前](http://www.yangzhiping.com/tech/git.html#comment-600500053 "Link to comment by Huayi Duan")
* [1 喜欢](http://www.yangzhiping.com/tech/git.html#)
* [F](http://www.yangzhiping.com/tech/git.html#)
*
*
* [![]()](http://disqus.com/guest/c689eb38a3c4c5a1341d2287b165fb9c/)

Jid  1 comment collapsed  [Collapse](http://www.yangzhiping.com/tech/git.html# "Collapse thread") [Expand](http://www.yangzhiping.com/tech/git.html# "Expand thread")

的
* A [喜欢](http://www.yangzhiping.com/tech/git.html#)
* [回复](http://www.yangzhiping.com/tech/git.html#)

* [4 月前](http://www.yangzhiping.com/tech/git.html#comment-629347515 "Link to comment by Jid")
* [0 喜欢](http://www.yangzhiping.com/tech/git.html#)
* [F](http://www.yangzhiping.com/tech/git.html#)
*
* [M *通过邮件订阅*](http://www.yangzhiping.com/tech/git.html#)
* [S *RSS*](http://ouyang.disqus.com/gitgithub/latest.rss)

引用通告网址
     <div id="footer"> <address> <span class="copyright"> Content by <a href="http://www.yangzhiping.com/">阳志平</a> (<a rel="licence" href="http://creativecommons.org/licenses/by-nc-nd/3.0/">Some rights reserved</a>) </span> <span class="engine"> Powered by <a href="http://github.com/mreid/jekyll/" title="A static, minimalist CMS">Jekyll</a> and <a href="http://mark.reid.name/">Mark Reid</a> </span> </address> </div> </div> <script type="text/javascript"> var _gaq = _gaq || []; _gaq.push(['_setAccount', 'UA-6781501-12']); _gaq.push(['_trackPageview']); (function() { var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true; ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js'; var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s); })(); </script> </body> </html>
