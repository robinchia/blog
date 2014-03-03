---
layout: post
title: "超越MySQL：三个流行MySQL分支的对比(摘录) - Speeddsy的专栏 - 博客频道 - "
categories: DB
tags: 
 - DB
 - mariadb
--- 

# 超越MySQL：三个流行MySQL分支的对比(摘录) - Speeddsy的专栏 - 博客频道 - CSDN.NET

您还未登录！|[登录](https://passport.csdn.net/account/login)|[注册](https://passport.csdn.net/account/register)|[帮助](https://passport.csdn.net/help/faq)

* [首页](http://www.csdn.net/)
* [业界](http://news.csdn.net/)
* [移动](http://mobile.csdn.net/)
* [云计算](http://cloud.csdn.net/)
* [研发](http://sd.csdn.net/)
* [论坛](http://bbs.csdn.net/)
* [博客](http://blog.csdn.net/)
* [下载](http://download.csdn.net/)
* 
## [更多]()

# [Speeddsy的专栏](http://blog.csdn.net/Speeddsy)

## 我的博客我说行！

* [![]()目录视图](http://blog.csdn.net/Speeddsy?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/Speeddsy?viewmode=list)
* [![]()订阅](http://blog.csdn.net/Speeddsy/rss/list)
[博客专家信息更新登记表](http://surveies.csdn.net/survey/comein/532)        [社区专家谈 12306](http://blog.csdn.net/blogdevteam/article/details/8572108)      [CSDN社区程序员回乡见闻活动火爆开始！](http://bbs.csdn.net/topics/390373496)
[专访周家安：我的十年编程自学之路](http://www.csdn.net/article/2013-02-26/2814263)        [2013年全国百所高校巡讲讲师招募](http://blog.csdn.net/csdnstudent/article/details/8077932)

### [超越MySQL：三个流行MySQL分支的对比(摘录)]()

分类： [数据库](http://blog.csdn.net/Speeddsy/article/category/1166947)  2012-06-16 16:32 178人阅读 [评论](http://blog.csdn.net/speeddsy/article/details/7669150#comments)(0) [收藏]( "收藏") [举报](http://blog.csdn.net/speeddsy/article/details/7669150#report "举报")
[mysql](http://blog.csdn.net/tag/details.html?tag=mysql)[引擎](http://blog.csdn.net/tag/details.html?tag=%e5%bc%95%e6%93%8e)[产品](http://blog.csdn.net/tag/details.html?tag=%e4%ba%a7%e5%93%81)[server](http://blog.csdn.net/tag/details.html?tag=server)[存储](http://blog.csdn.net/tag/details.html?tag=%e5%ad%98%e5%82%a8)[扩展](http://blog.csdn.net/tag/details.html?tag=%e6%89%a9%e5%b1%95)

**三个流行MySQL分支：Drizzle、MariaDB和Percona Server（包括XtraDB引擎）**

**XtraDB**

XtraDB是一款独立的产品，但它仍被认为是MySQL的一个分支。XtraDB实际上是基于MySQL的数据库的一个存储引擎。XtraDB被认为是已成为MySQL一部分的标准MyISAM和InnoDB的一个额外存储引擎。

XtraDB分支有另一个目标，即成为InnoDB存储引擎的简单替代，这样用户就可以轻松地切换其存储引擎，无需更改任何现有的应用程序代码。XtraDB必须能够向后兼容InnoDB，以提供它们想要添加的所有新功能和改进。它们实现了此目标。

XtraDB的速度有多快？我找到的一个性能测试表明：与内置的MySQL 5.1 InnoDB 引擎相比，它每分钟可处理2.7倍的事务。（请参见参考资料）。速度显然是一个不可以忽略的因素，在考虑替代产品时更是如此。

**Percona**

与内置的MySQL存储引擎相比，XtraDB提供了一些极大的改进，但它不是一款独立产品，也无法轻松放入现有MySQL安装。因此，如果您想使用这款新引擎，则必须使用提供它的产品。采用Percona Server的一个很好的理由是，利用XtraDB引擎来尽可能地减少代码更改。

下面是Percona Server的声明，该声明来自它们自己的网站：

* 可扩展性：处理更多事务；在强大的服务器上进行扩展
* 性能：使用了XtraDB的Percona Server速度非常快
* 可靠性：避免损坏，提供崩溃安全(crash-safe)复制
* 管理：在线备份，在线表格导入/导出
* 诊断：高级分析和检测
* 灵活性：可变的页面大小，改进的缓冲池管理

Percona Server的一个缺点是他们自己管理代码，不接受外部开发人员的贡献，以这种方式确保他们对产品中所包含功能的控制。

**MariaDB**

另一款提供了XtraDB存储引擎的产品是MariaDB产品。它与Percona产品非常类似，但是提供了更多底层代码更改，试图提供比标准MySQL更多的性能改进

MariaDB提供了MySQL提供的标准存储引擎，即MyISAM和InnoDB。因此，实际上，可以将它视为MySQL的扩展集，它不仅提供MySQL提供的所有功能，还提供其他功能。MariaDB还声称自己是MySQL的替代，因此从MySQL切换到MariaDB时，无需更改任何基本代码即可安装它。

**Drizzle**

本文介绍的最后一款产品是Drizzle。与之前介绍的两款产品不同，Drizzle与MySQL有很大差别，甚至声称它们不是MySQL的替代产品。他们期望对MySQL进行一些重大更改，想要提供一种出色的解决方案来解决高可用性问题，即使这意味着要更改我们已经习惯了的MySQL的各个方面。

是不是所有人都应该使用Drizzle呢？等等，正如Drizzle反复指出的那样，它与MySQL不兼容。因此，如果您现在使用的是MySQL平台，那么需要重写大量代码，才能使Drizzle在您的环境中正常工作

尽管需要额外的工作才能让它运行，但它并不像Percona或MariaDB那样快速且易于使用。我之所以介绍Drizzle，是因为尽管目前它可能不是您的选择，但几年之后，它很可能会成为一些人的选择。因为本文的目标是提高您对未来使用的工具的认识，所以这是向您介绍此产品的好机会。许多领先的DB专家相信Drizzle将成为未来5年内高可用性数据库安装的选择。
分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")

* 上一篇：[Windows7下MySQL5.5.20免安装版的配置](http://blog.csdn.net/speeddsy/article/details/7667621)
* 下一篇：[eclipse 配置知识](http://blog.csdn.net/speeddsy/article/details/7669425)
查看评论[]()

  暂无评论
您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fspeeddsy%2Farticle%2Fdetails%2F7669150)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()
[![TOP]()](http://blog.csdn.net/speeddsy/article/details/7669150# "回到顶部")

个人资料

[![]( "访问我的空间")](http://my.csdn.net/Speeddsy)
[Speeddsy](http://my.csdn.net/Speeddsy)

[]( "[加关注]") []( "[发私信]")

* 访问：2619次
* 积分：58分
* 排名：千里之外

* 原创：1篇
* 转载：13篇
* 译文：0篇
* 评论：2条

文章分类

* [互联网](http://blog.csdn.net/speeddsy/article/category/1277200)(0)
* [数据库](http://blog.csdn.net/speeddsy/article/category/1166947)(6)
* [java](http://blog.csdn.net/speeddsy/article/category/1167373)(5)
* [c/c++](http://blog.csdn.net/speeddsy/article/category/1167164)(3)
* [其它](http://blog.csdn.net/speeddsy/article/category/1187660)(1)
文章存档

* [2012年11月](http://blog.csdn.net/speeddsy/article/month/2012/11)(1)
* [2012年07月](http://blog.csdn.net/speeddsy/article/month/2012/07)(1)
* [2012年06月](http://blog.csdn.net/speeddsy/article/month/2012/06)(12)

文章搜索

[]()
阅读排行

* [福昕pdf套件注册码激活](http://blog.csdn.net/speeddsy/article/details/7751270 "福昕pdf套件注册码激活")(640)
* [codeblocks 使用汇总](http://blog.csdn.net/speeddsy/article/details/7661602 "codeblocks 使用汇总")(418)
* [MySQL5.5 配置文件 my.ini](http://blog.csdn.net/speeddsy/article/details/7667611 "MySQL5.5 配置文件 my.ini")(327)
* [30 Best Eclipse Plugins](http://blog.csdn.net/speeddsy/article/details/7677726 "30 Best Eclipse Plugins")(248)
* [eclipse保存个人设置preference](http://blog.csdn.net/speeddsy/article/details/7662626 "eclipse保存个人设置preference")(200)
* [超越MySQL：三个流行MySQL分支的对比(摘录)]( "超越MySQL：三个流行MySQL分支的对比(摘录)")(177)
* [Windows7下MySQL5.5.20免安装版的配置](http://blog.csdn.net/speeddsy/article/details/7667621 "Windows7下MySQL5.5.20免安装版的配置")(110)
* [MySQL Workbench的使用教程](http://blog.csdn.net/speeddsy/article/details/7660647 "MySQL Workbench的使用教程")(105)
* [You can't specify target table 'sc1' for update in FROM clause](http://blog.csdn.net/speeddsy/article/details/7694735 "You can't specify target table 'sc1' for update in FROM clause")(93)
* [eclipse 配置知识](http://blog.csdn.net/speeddsy/article/details/7669425 "eclipse 配置知识")(79)

评论排行

* [MySQL Workbench的使用教程](http://blog.csdn.net/speeddsy/article/details/7660647 "MySQL Workbench的使用教程")(1)
* [eclipse保存个人设置preference](http://blog.csdn.net/speeddsy/article/details/7662626 "eclipse保存个人设置preference")(1)
* [福昕pdf套件注册码激活](http://blog.csdn.net/speeddsy/article/details/7751270 "福昕pdf套件注册码激活")(0)
* [You can't specify target table 'sc1' for update in FROM clause](http://blog.csdn.net/speeddsy/article/details/7694735 "You can't specify target table 'sc1' for update in FROM clause")(0)
* [30 Best Eclipse Plugins](http://blog.csdn.net/speeddsy/article/details/7677726 "30 Best Eclipse Plugins")(0)
* [10款常用Java测试工具](http://blog.csdn.net/speeddsy/article/details/7677667 "10款常用Java测试工具")(0)
* [jdk与jre的区别](http://blog.csdn.net/speeddsy/article/details/7669609 "jdk与jre的区别")(0)
* [eclipse 配置知识](http://blog.csdn.net/speeddsy/article/details/7669425 "eclipse 配置知识")(0)
* [超越MySQL：三个流行MySQL分支的对比(摘录)]( "超越MySQL：三个流行MySQL分支的对比(摘录)")(0)
* [Windows7下MySQL5.5.20免安装版的配置](http://blog.csdn.net/speeddsy/article/details/7667621 "Windows7下MySQL5.5.20免安装版的配置")(0)
推荐文章

最新评论

* [MySQL Workbench的使用教程](http://blog.csdn.net/Speeddsy/article/details/7660647#comments)

[随便啦](http://blog.csdn.net/lnas01): 请问这篇文章怎么转载啊。。。
* [eclipse保存个人设置preference](http://blog.csdn.net/Speeddsy/article/details/7662626#comments)

[greateryang](http://blog.csdn.net/greateryang): 好用，此文章推荐寻找此方法的同志都看看

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)京 ICP 证 070598 号北京创新乐知信息技术有限公司 版权所有世纪乐知(北京)网络技术有限公司 提供技术支持江苏乐知网络技术有限公司 提供商务支持![]() 联系邮箱：webmaster(at)csdn.netCopyright © 1999-2012, CSDN.NET, All Rights Reserved [![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
![]()
