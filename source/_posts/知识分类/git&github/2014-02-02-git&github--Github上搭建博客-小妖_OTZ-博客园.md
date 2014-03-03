---
layout: post
title: "Github上搭建博客 - 小妖_OTZ - 博客园"
categories: git&github
tags: 
 - git&github
--- 

# Github上搭建博客 - 小妖_OTZ - 博客园

[]()

[午饭](http://www.cnblogs.com/sunwufan/)

* [博客园](http://www.cnblogs.com/)
* [首页](http://www.cnblogs.com/sunwufan/)
* [博问](http://q.cnblogs.com/)
* [闪存](http://home.cnblogs.com/ing/)
* [新随笔](http://www.cnblogs.com/sunwufan/admin/EditPosts.aspx?opt=1)
* [联系](http://space.cnblogs.com/msg/send/%e5%b0%8f%e5%a6%96.OTZ)
* [订阅](http://www.cnblogs.com/sunwufan/rss)
* [管理](http://www.cnblogs.com/sunwufan/admin/EditPosts.aspx)
随笔-38 文章-0 评论-3 

# [Github上搭建博客](http://www.cnblogs.com/sunwufan/archive/2012/10/16/2726225.html)

本文将详细介绍如何在Windows系统下搭建博客，并发布到GitHub Pages上，以及后期博客发布后的更新工作....

### **一、博客整体效果**

**         搭建博客效果**：[http://www.sunfanwu.com/](http://www.sunfanwu.com/)（绑定域名后的效果，未绑定则调转到[sunfanwu.github.com](http://sunfanwu.github.com/)）

**         网站成本：**域名注册[http://www.sunfanwu.com/](http://www.sunfanwu.com/) 50元（可不要）

**         服  务 器：**免费托管在GitHub个人主页上（无数据库，但有整个网站备份。由于GitHub网站性质，博客源码文档为所有人可见）。

**         博客类型：**静态博客，无数据库，博客通过html编写。

**         本地软件：[msysgit](http://code.google.com/p/msysgit/downloads/list)、[ruby](http://rubyforge.org/frs/download.php/75127/rubyinstaller-1.9.2-p290.exe)、[DevKit](https://github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe)、[python](http://www.activestate.com/activepython/downloads)。**

**         备       注：**GitHub为开源项目网站，请不要上传过大的且无意义的附件（如视频等）。

### **二、搭建本地环境**

       为了在Github上使用Octopress，需要首先配置一下本地环境：

       安装Git，下载[msysgit](http://code.google.com/p/msysgit/downloads/list)，安装可参考[官方文档](https://help.github.com/articles/set-up-git)。（Git分布式版本管理系统，在此主要是用来操作GitHub。不推荐使用Git 客户端—github for windows）

       然后安装Ruby，[Octopress 官方文档](http://octopress.org/docs/setup/)中指定的 Ruby 版本是 1.9.2，下载 [rubyinstaller-1.9.2-p290.exe](http://rubyforge.org/frs/download.php/75127/rubyinstaller-1.9.2-p290.exe)，安装时记得选中“Add Ruby executables to your PATH”。

       为了检查Git、ruby是否已加入到PATH中，可在 Windows 的cmd窗口中执行以下命令：
path

       接着安装Devkit，选择下载 4.5.2 版本：[DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe](https://github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe)，下载完成后，将其解压到如 E:DevKit，然后在win的cmd窗口中执行如下命令进行安装：

E:
cd E:DevKit
ruby dk.rb init
ruby dk.rb install

        （选装）最后安装python，主要是博客代码加亮模块需要python环境的支持，[下载2.7版](http://www.activestate.com/activepython/downloads)，安装完以后，在Windows的cmd窗口中执行：

         本地环境配置结束。

### 三、更新本地环境配置

 

       为了支持中文UTF-8编码，对Windows环境变量配置如下：

 
LANG=zh_CN.UTF-8
LC_ALL=zh_CN.UTF-8

      也可在直接在Windows的cmd窗口下运行命令：

set LANG=zh_CN.UTF-8
set LC_ALL=zh_CN.UTF-8

        更新gem的更新源，ruby的官方更新源经常被河蟹，换成国内的更新源，这样速度就快多了，变更如下：

gem sources -a http://ruby.taobao.org/
gem sources -r http://rubygems.org/
gem sources -l

        最后一个命令可查看更改后的更新源列表。

### 四、下载并配置Octopress

        首先下载Octopress源码，可以使用下面git命令（具体应用可参见git官方文档）下载，也可直接在Octopress Github库中下载octopress的zip包（[点击下载](https://nodeload.github.com/imathis/octopress/zipball/master)），然后将下载的压缩包解压到E盘根目录，修改解压后的文件夹名称为 octopress。
E:
git clone git://github.com/imathis/octopress.git  octopress

         然后更新 Octopress 的gem更新源：进到 E:octopress 目录，用文本编辑器（例如记事本）打开文件Gemfile，将里面source “http://rubygems.org/”改为source “http://ruby.taobao.org/”。

         最后安装Octopress的依赖项，在Windows的CMD窗口输入以下命令：
E:
cd octopress
gem install bundler
bundle install

### 五、新建Github Repositories

       登录[Github](https://github.com/)，假设你的用户名是username，首先要新建一个命名为 **username.github.com**的Repo，命名必须是这个格式，如果不这样命名的话，在运行命令 rake setup_github_pages  之后不能够自动创建后面提到的master和source 分支，而是作为普通仓库生成 gh-pages 分支。

       创建Repo（如果没用过GitHub，需先注册账户），如下图：

![image]()

       Repo的设置，如下图：

 ![20121016155745]()

### 六、发布Octopress到Github

       1、打开Windows下的命令窗口，进入到Octopress所在的目录，输入命令：
rake setup_github_pages

        按照提示输入刚才新建的Repo地址，类似：[git@github.com:用户名/用户](mailto:git@github.com:用户名/用户)[名.github.com.git](mailto:git@github.com)。

![在Github上搭建Octopress博客03]()

       2、接着输入命令：
rake install

rake generate

rake preview

     其中rake install是安装Octopress默认主题的，rake gnerate是生成静态页面的，这两个命令是必须运行的，而rake preview则是用来本地浏览的（运行时看屏幕上提示，按Ctrl+C并输入Y来终止批处理操作），运行后打开浏览器，输入 [http://localhost:4000/](http://localhost:4000/) 就可以看到如下的界面了，不想预览的话也可以不运行，直接进入下一步。

![在Github上搭建Octopress博客04]()

      3、将博客发布到Github上，输入下面命令：
rake deploy

     **备注：此处遇到错误，请看问题一。**

     这样，生成的内容将会自动发布到master分支，并且可以使用 [http://用户名.github.com](http://用户名.github.com/) 访问内容。

![在Github上搭建Octopress博客05]()

        4、别忘了把所有源文件发布到 source 分支下面：
git add .

git commit -m “your message”

git push origin source

        至此，所有发布完成，接下来就是对博客的设置了。

### 七、Ocotpress博客配置

          更改下面的配置后，还需要运行 rake generate、rake deploy等等命令的。

      1、默认的博客运行成功的话，就需要按照自己的要求对博客配置进行修改了，主要是修改Octopress根目录下的主配置文件_config.yml。
url:  http://username.github.com                 # 博客地址

title:                                                       # 博客标题
subtitle:                                                  # 副标题
author:                                                   # 作者
simple_search:  http://www.google.com.hk/search     # 搜索引擎
description:                                                            # 关于博客的描述
subscribe_rss:  /atom.xml                  # Rss订阅地址, 默认是  /atom.xml
subscribe_email:                               # 提供Email订阅的地址
email:                                              # Rss订阅的Email地址

root:  /               # 博客路径，默认是“/“，如果你打算在子目录中，记得修改这个路径
permalink: /blog/:year/:month/:day/:title/           # 文章的固定链接形式

     2、更换主题

         主题位于 octopress/.theme 目录下，默认主题为 classic。 如果需要更改主题（可在网上查找），下载后将主题也放在.theme目录下即可，如果主题名字为blog_theme，那么安装主题时输入以下命令即可：
rake install ['blog_theme']

### 八、绑定域名

        Github Pages绑定域名，需要在Octopress/source目录下建个无后缀的CNAME文本文件，文件内容就是你的域名，例如：
www.sunfanwu.com

        然后进入你的域名管理网站，修改A纪录为：“207.97.227.245” ，或者 CNAME 指向 username.github.com，下面就等着解析生效了。

**九、更新博客**（发布日志将在下一篇进行详细说明，敬请期待....）

        由于每次写博客都通过html格式比较麻烦，所以此处推荐使用[Windows Live Writer](http://baike.baidu.com/view/1049077.htm)本地编辑。

        1.编辑完成，选择以“源代码”形式查看,复制文本。

        2.在本地“octopress/public/blog/”找到需要更新的日志,通过记事本软件打开此html。

        3.找到<div class="entry-content"></div>，将复制的代码粘贴进去，保存关闭。

        4.通过之前说过的“rake deploy”命令就可以发布了。（发布之前可“rake preview”本地查看一下~）

       **备注：**如有图片的话，一般做法还需将图片保存到对应文件夹。此处可以先在“博客园”等网站通过[Windows Live Writer](http://baike.baidu.com/view/1049077.htm)发布博客，再将源码复制。即省去图片调用。又可做到一篇博客不同地方迅速发布，保留备份。

**问题一：发布时git出现提示：**
Agent admitted failure to sign using the key. Permission denied (publickey)
fatal: The remote end hung up unexpectedly

   github的[官方文档](http://help.github.com/linux-set-up-git/)

#1. Check for SSH keys. $ cd ~/.ssh #2. Backup and remove existing SSH keys. $ lsLists all the subdirectories in the current directory $ mkdir key_backupmakes a subdirectory called "key_backup" in the current directory $ cp id_rsa* key_backupCopies the contents of the id_rsa directory into key_backup $ rm id_rsa* #3. Generate a new SSH key. $ ssh-keygen -t rsa -C "your_email@youremail.com" #4. Add your SSH key to GitHub. #5. Test everything out. $ ssh -T [git@github.com](mailto:git@github.com)

     大概意思就是你先在本地建立自己的SSH密码对，然后将公钥上传到github。进行上传服务时输入密码即可（cmd输入密码是为不可见状态，输入好后点击“回车”即可）。

参考文章：

            教程参考： [http://xuhehuan.com/783.html](http://xuhehuan.com/783.html) （80%为参考此处进行整理）。

                          [http://mrzhang.me/blog/blog-equals-github-plus-octopress.html](http://mrzhang.me/blog/blog-equals-github-plus-octopress.html "http://mrzhang.me/blog/blog-equals-github-plus-octopress.html")

                          [http://caok.github.com/blog/2012/06/24/install-octopress-to-write-blog/](http://caok.github.com/blog/2012/06/24/install-octopress-to-write-blog/ "http://caok.github.com/blog/2012/06/24/install-octopress-to-write-blog/")

            octopress官方建立github主页参考： [http://octopress.org/docs/deploying/github/](http://octopress.org/docs/deploying/github/ "http://octopress.org/docs/deploying/github/")

posted @ 2012-10-16 16:10 [小妖.OTZ](http://www.cnblogs.com/sunwufan/) 阅读(2654) 评论(0) [编辑](http://www.cnblogs.com/sunwufan/admin/EditPosts.aspx?postid=2726225) [收藏](http://www.cnblogs.com/sunwufan/archive/2012/10/16/2726225.html#)
![]()

[刷新评论]()[刷新页面](http://www.cnblogs.com/sunwufan/archive/2012/10/16/2726225.html#)[返回顶部](http://www.cnblogs.com/sunwufan/archive/2012/10/16/2726225.html#top)

[博客园首页](http://www.cnblogs.com/ "程序员的网上家园")[博问](http://q.cnblogs.com/ "程序员问答社区")[新闻](http://news.cnblogs.com/ "IT新闻")[闪存](http://home.cnblogs.com/ing/)[程序员招聘](http://job.cnblogs.com/)[知识库](http://kb.cnblogs.com/)

### 公告
Copyright ©2013 小妖.OTZ
