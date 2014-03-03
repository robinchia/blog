---
layout: post
title: "PowerDesinger逆向数据库物理模型及关系图 - 既然来了就要多看几篇博客， 因为每一篇都适"
categories: 网上资料url集
tags: 
 - 网上资料url集
--- 

# PowerDesinger逆向数据库物理模型及关系图 - 既然来了就要多看几篇博客， 因为每一篇都适合你。如果收藏重复看，你一定能感受到我的正能量。 - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼](http://zz563143188.iteye.com/blog/1829068#)

[招聘](http://job.iteye.com/iteye) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://zz563143188.iteye.com/login "登录") [登录](http://zz563143188.iteye.com/login) [注册](http://zz563143188.iteye.com/signup)

# [既然来了就要多看几篇博客， 因为每一篇都适合你。如果收藏重复看，你一定能感受到我的正能量。](http://zz563143188.iteye.com/)

* [**博客**](http://zz563143188.iteye.com/)
* [微博](http://zz563143188.iteye.com/weibo)
* [相册](http://zz563143188.iteye.com/album)
* [收藏](http://zz563143188.iteye.com/link)
* [留言](http://zz563143188.iteye.com/blog/guest_book)
* [关于我](http://zz563143188.iteye.com/blog/profile)

### [PowerDesinger逆向数据库物理模型及关系图]() **

**博客分类：**
* [编程工具](http://zz563143188.iteye.com/category/270761)

 利用PowerDesinger生成的数据库物理模型及关系图

**下载地址：** [http://pan.baidu.com/share/link?shareid=372668&uk=4076915866](http://pan.baidu.com/share/link?shareid=372668&uk=4076915866)    

 
在[数据库建模](http://baike.baidu.com/view/280978.htm)的过程中，需要运用PowerDesigner进行[数据库设计](http://baike.baidu.com/view/8268.htm)，这个不但让人直观的理解模型，而且可以充分的利用数据库技术，优化数据库的设计。第一次用PowerDesigner并不感到很陌生，里面与SQLServer建立[数据库](http://baike.baidu.com/view/1088.htm)差不多。

[2][]()其次就是[ER图](http://baike.baidu.com/view/1622963.htm)，在[数据库系统概论](http://baike.baidu.com/view/1051126.htm)中有涉及到，这个实体关系图中，一个实体对于一个表，实体、属性与联系是进行[系统设计](http://baike.baidu.com/view/170106.htm)时要考虑的三个[要素](http://baike.baidu.com/view/290205.htm)，也是一个好的[数据库设计](http://baike.baidu.com/view/8268.htm)的核心。
正文：

利用PowerDesinger逆向数据物理模型及关系图，第一步是建立ODBC数据源，然后再利用powerdesinger连接ODBC数据源管理数据。
步骤：

1. 首先建立odbc数据源，开始->控制面板->管理工具->数据源(odbc)->系统DSN->添加数 据库驱动(本人是用的oracle。所以就使用 oracle in oraDb10g_home驱动), 建立好数据源测试成功即可。
![]( "点击查看原始大小图片")

2.添加myo数据源,添加数据源以后测试连接是否成功。
![]()

![]()
3.通过powerdesinger逆向工程->数据库得到数据库物理模型。

![]( "点击查看原始大小图片")
4.选择数据库类型，建立ＯＤＢＣ连接

![]( "点击查看原始大小图片")
5.配置数据源，可能已经存在的数据源，也可以新建数据源

![]( "点击查看原始大小图片")
6.如果已存在ｍｙｏ数据源则不需要新建，如果没有则需要新建数据源。配置后测试数据库连接后，即可逆向数据库模型。

![]()
7.powerdesinger显示逆向的所有表，并即将生成物理模型和关系实体图。

![]( "点击查看原始大小图片")
8.显示出表的结构信息，并可以重新编译生成。

![]( "点击查看原始大小图片")
9.powerdesinger为我们建立好数据库关系图，看上去是不是很专业。

 
![]( "点击查看原始大小图片")

 
* [![]( "点击查看原始大小图片")](http://dl2.iteye.com/upload/attachment/0081/6042/5b5c8c72-6caf-3cd8-84cc-e43955abca9c.jpg)
* 大小: 127 KB

* [![]( "点击查看原始大小图片")](http://dl2.iteye.com/upload/attachment/0081/6044/cd7270ad-6376-30e7-a051-90028d7c9bf6.jpg)
* 大小: 84.4 KB

* [![]( "点击查看原始大小图片")](http://dl2.iteye.com/upload/attachment/0081/6046/284695e5-2e12-3212-8ba5-d8fb24a9e436.jpg)
* 大小: 77.7 KB

* [![]( "点击查看原始大小图片")](http://dl2.iteye.com/upload/attachment/0081/6048/118eb2ef-2a81-3449-baec-bf3950972574.jpg)
* 大小: 129.9 KB

* [![]( "点击查看原始大小图片")](http://dl2.iteye.com/upload/attachment/0081/6050/d0f2cbd9-fd23-3bcf-8ccb-ff032c31f3c7.jpg)
* 大小: 84.7 KB

* [![]( "点击查看原始大小图片")](http://dl2.iteye.com/upload/attachment/0081/6052/513c14f3-7257-35ef-9fee-7fc6465be432.jpg)
* 大小: 137.2 KB

* [![]( "点击查看原始大小图片")](http://dl2.iteye.com/upload/attachment/0081/6054/dc6a4056-e2a7-361f-9c2e-d5600869f21f.jpg)
* 大小: 94.7 KB

* [![]( "点击查看原始大小图片")](http://dl2.iteye.com/upload/attachment/0081/6056/9b27e97c-15be-3f13-adf6-b16ca1c05440.jpg)
* 大小: 105.1 KB

* [![]( "点击查看原始大小图片")](http://dl2.iteye.com/upload/attachment/0081/6058/a3409e43-62cf-33fb-9aad-fc215e99813a.jpg)
* 大小: 259.4 KB

* [![]( "点击查看原始大小图片")](http://dl2.iteye.com/upload/attachment/0081/6060/a2065747-f46f-3aeb-8e21-5ceceb9bde3a.jpg)
* 大小: 269 KB

* [查看图片附件](http://zz563143188.iteye.com/blog/1829068#)

**3**
顶

**1**
踩

分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")
[好用的需求文档管理工具Telelogic DOORS](http://zz563143188.iteye.com/blog/1830393 "好用的需求文档管理工具Telelogic DOORS") | [数据库生成数据字典工具(PDMREAD)图解](http://zz563143188.iteye.com/blog/1828557 "数据库生成数据字典工具(PDMREAD)图解")

* 2013-03-14 08:52
* 浏览 1517
* [评论(0)](http://zz563143188.iteye.com/blog/1829068#comments)
* 分类:[编程语言](http://www.iteye.com/blogs/category/language)
* [相关推荐](http://www.iteye.com/wiki/blog/1829068)
### 评论

[]()

### 发表评论

[![]()](http://zz563143188.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://zz563143188.iteye.com/login)

[![zz563143188的博客]( "zz563143188的博客: 既然来了就要多看几篇博客， 因为每一篇都适合你。如果收藏重复看，你一定能感受到我的正能量。")](http://zz563143188.iteye.com/)

zz563143188

* 浏览: 232530 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 珠海
* ![]()
### 最近访客 [更多访客>>](http://zz563143188.iteye.com/blog/user_visits)

[![ausra0714的博客]( "ausra0714的博客: ")](http://ausra0714.iteye.com/)

[ausra0714](http://ausra0714.iteye.com/ "ausra0714")

[![basil1292的博客]( "basil1292的博客: basil1292")](http://basil1292.iteye.com/)

[basil1292](http://basil1292.iteye.com/ "basil1292")
[![tw_loverr的博客]( "tw_loverr的博客: ")](http://tw-loverr.iteye.com/)

[tw_loverr](http://tw-loverr.iteye.com/ "tw_loverr")

[![anyeeye的博客]( "anyeeye的博客: anyeeye")](http://anyeeye.iteye.com/)

[anyeeye](http://anyeeye.iteye.com/ "anyeeye")

### 博客专栏

[![77fc734c-2f95-3224-beca-6b8da12debc8]()](http://www.iteye.com/blogs/subjects/zl19861124) [编程工具介绍](http://www.iteye.com/blogs/subjects/zl19861124 "编程工具介绍")
浏览量：71600 [![D9710da2-8a00-3ae6-a084-547a11afab81]()](http://www.iteye.com/blogs/subjects/zl563143188) [Spring Mvc实战(...](http://www.iteye.com/blogs/subjects/zl563143188 "Spring Mvc实战(必带源码)")
浏览量：71858 [![D3f88135-07de-3968-a0f0-d2f13428c267]()](http://www.iteye.com/blogs/subjects/zl52013145) [项目开发经验](http://www.iteye.com/blogs/subjects/zl52013145 "项目开发经验")
浏览量：94503
### 文章分类

* [全部博客 (58)](http://zz563143188.iteye.com/)
* [java (7)](http://zz563143188.iteye.com/category/213003)
* [生活小结 (0)](http://zz563143188.iteye.com/category/216667)
* [工作 (4)](http://zz563143188.iteye.com/category/232472)
* [Spring (7)](http://zz563143188.iteye.com/category/269165)
* [web (8)](http://zz563143188.iteye.com/category/269712)
* [数据库 (4)](http://zz563143188.iteye.com/category/270760)
* [编程工具 (8)](http://zz563143188.iteye.com/category/270761)
* [编程经验 (18)](http://zz563143188.iteye.com/category/271839)
* [代码 (2)](http://zz563143188.iteye.com/category/273789)
* [设计模式 (0)](http://zz563143188.iteye.com/category/271644)
* [数据结构算法 (0)](http://zz563143188.iteye.com/category/273130)
* [网络编程 (0)](http://zz563143188.iteye.com/category/273260)
* [服务器 (0)](http://zz563143188.iteye.com/category/273328)
* [java基础 (0)](http://zz563143188.iteye.com/category/274045)
* [UMl (0)](http://zz563143188.iteye.com/category/274147)

### 社区版块

* [我的资讯](http://zz563143188.iteye.com/blog/news) (0)
* [我的论坛](http://zz563143188.iteye.com/blog/post) (0)
* [我的问答](http://zz563143188.iteye.com/blog/answered_problems) (1)
### 存档分类

* [2013-05](http://zz563143188.iteye.com/blog/monthblog/2013-05) (5)
* [2013-04](http://zz563143188.iteye.com/blog/monthblog/2013-04) (22)
* [2013-03](http://zz563143188.iteye.com/blog/monthblog/2013-03) (29)
* [更多存档...](http://zz563143188.iteye.com/blog/monthblog_more)

### 评论排行榜

* [Spring mvc+hibernate+freemarker(开源项目 ...](http://zz563143188.iteye.com/blog/1825168 "Spring mvc+hibernate+freemarker(开源项目)")
* [从程序员到CTO的Java技术路线图](http://zz563143188.iteye.com/blog/1877266 "从程序员到CTO的Java技术路线图")
* [推荐一款好用的笔记管理软件(Evernote)](http://zz563143188.iteye.com/blog/1830965 "推荐一款好用的笔记管理软件(Evernote)")
* [比较全的OA系统功能模块列表](http://zz563143188.iteye.com/blog/1860248 "比较全的OA系统功能模块列表 ")
* [Java开发中的23种设计模式详解](http://zz563143188.iteye.com/blog/1847029 "Java开发中的23种设计模式详解")
### 最新评论

* [zz563143188](http://zz563143188.iteye.com/ "zz563143188")： mikzhang 写道我觉得CTO未必这些全会当然不用全会，具 ...
[从程序员到CTO的Java技术路线图](http://zz563143188.iteye.com/blog/1877266#bc2313623)
* [mikzhang](http://mikzhang.iteye.com/ "mikzhang")： 我觉得CTO未必这些全会
[从程序员到CTO的Java技术路线图](http://zz563143188.iteye.com/blog/1877266#bc2313614)
* [zz563143188](http://zz563143188.iteye.com/ "zz563143188")： tuweijie 写道[color=red][/color][ ...
[从程序员到CTO的Java技术路线图](http://zz563143188.iteye.com/blog/1877266#bc2313613)
* [zz563143188](http://zz563143188.iteye.com/ "zz563143188")： cloudzb 写道LZ厉害！现在还是下层的人，表示压力三大！ ...
[从程序员到CTO的Java技术路线图](http://zz563143188.iteye.com/blog/1877266#bc2313612)
* [zz563143188](http://zz563143188.iteye.com/ "zz563143188")： zhongxiaweimian 写道哎 要学的东西还很多呢根据 ...
[从程序员到CTO的Java技术路线图](http://zz563143188.iteye.com/blog/1877266#bc2313611)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2012 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![]()
