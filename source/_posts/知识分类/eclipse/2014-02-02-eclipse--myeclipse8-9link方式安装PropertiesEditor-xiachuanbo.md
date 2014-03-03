---
layout: post
title: "myeclipse8-9 link方式安装PropertiesEditor - xiachuanbo"
categories: eclipse
tags: 
 - eclipse
--- 

# myeclipse8,9 link方式安装PropertiesEditor - xiachuanbo2007sz的专栏 - 博客频道 - CSDN.NET

您还未登录！|[登录](http://passport.csdn.net/account/login)|[注册](http://passport.csdn.net/account/register)|[帮助](http://passport.csdn.net/help/faq)

* [CSDN首页](http://www.csdn.net/)
* [资讯](http://news.csdn.net/)
* [论坛](http://bbs.csdn.net/)
* [博客](http://blog.csdn.net/)
* [下载](http://download.csdn.net/)
* [搜索](http://so.csdn.net/)
* 
## [更多]()

* [CTO俱乐部](http://cto.csdn.net/)
* [学生大本营](http://student.csdn.net/)
* [培训充电](http://edu.csdn.net/)
* [移动开发](http://mobile.csdn.net/)
* [软件研发](http://sd.csdn.net/)
* [云计算](http://cloud.csdn.net/)
* [程序员](http://www.programmer.com.cn/)
* [ITeye](http://www.iteye.com/)
* [TUP](http://tup.csdn.net/)

# [xiachuanbo2007sz的专栏](http://blog.csdn.net/xiachuanbo2007sz)

##

* [![]()目录视图](http://blog.csdn.net/xiachuanbo2007sz?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/xiachuanbo2007sz?viewmode=list)
* [![]()订阅](http://blog.csdn.net/xiachuanbo2007sz/rss/list)
[“第四届中国云计算大会”最新重磅嘉宾抢先报！](http://blog.csdn.net/blogdevteam/article/details/7419530)         [博客频道4月技术图书有奖试读火爆开启](http://blog.csdn.net/blogdevteam/article/details/7415936)        [免费下载《SKC易云解决方案》](http://ad-apac.doubleclick.net/clk;253093956;76972838;f?http://www.ibm.com/systems/cn/ads/2012q1_bcfc.shtml?crs=apch_cit1_20120206_1328527126975&cm=b&cr=csdn&ct=CN2DG13W&ck=csdn&cmp=CN2DG)
[2012年7月微软MVP申请开始啦！](http://blog.csdn.net/blogdevteam/article/details/7414502)           [CSDN十大风云博客专栏评选结果公布！](http://event.blog.csdn.net/TopColumn/awardlist.aspx)            [CSDN博客皮肤评选活动火爆开启！](http://event.blog.csdn.net/BlogSkin/Vote.aspx)

### [myeclipse8,9 link方式安装PropertiesEditor]()

分类： [java web](http://blog.csdn.net/xiachuanbo2007sz/article/category/640912)  2011-08-21 13:55 194人阅读 [评论](http://blog.csdn.net/xiachuanbo2007sz/article/details/6705978#comments)(0) [收藏]( "收藏") [举报](http://blog.csdn.net/xiachuanbo2007sz/article/details/6705978#report "举报")
步骤：

1.先下载 PropertiesEditor 插件  最新版本5.3.3
下载地址：[http://zh.sourceforge.jp/projects/propedit/downloads/40156/jp.gr.java_conf.ussiy.app.propedit_5.3.3.zip/](http://zh.sourceforge.jp/projects/propedit/downloads/40156/jp.gr.java_conf.ussiy.app.propedit_5.3.3.zip/)

日本的网站，速度有点慢，如果没有下载，点击（[jp.gr.java_conf.ussiy.app.propedit_5.3.3.zip](http://zh.sourceforge.jp/frs/redir.php?m=jaist&f=%2Fpropedit%2F40156%2Fjp.gr.java_conf.ussiy.app.propedit_5.3.3.zip).）

如图

![]()

2. 配置插件

解压文件夹

![]()

 

在myeclipse安装目录 Genuitec\MyEclipse-8.6\dropins文件夹下创建link文件

![]()

 

编写link文件，格式： path=插件文件路径

path=D:\\myeclipse8.6\\Genuitec\\MyEclipse-8.6\\myplugins\\propedit

![]()

 

配置完毕重启 myeclipse  找到对应的资源文件 右键open with 选择propertiesEditor打开

![]()

效果如下

![]()

 

可以设置默认属性文件打开方式 window ->Perferences->General->Editors->File Associations 找到 *.properties  选择 PropertiesEditor点default  如图

 ![]()

点击ok 完成操作 ！

 其他插件也可以上述link 安装！

 

 

分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")
* 上一篇：[Myeclipse 9正式版 下载](http://blog.csdn.net/xiachuanbo2007sz/article/details/6397790)
* 下一篇：[Quartz 调度，添加，修改，删除 任务](http://blog.csdn.net/xiachuanbo2007sz/article/details/6926556)
查看评论[]()

  暂无评论
您还没有登录,请[[登录]](http://passport.csdn.net/account/login?from=http%3A%2F%2Fblog.csdn.net%2Fxiachuanbo2007sz%2Farticle%2Fdetails%2F6705978)或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fxiachuanbo2007sz%2Farticle%2Fdetails%2F6705978)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()

个人资料

[![]( "访问我的空间")](http://my.csdn.net/xiachuanbo2007sz)
[xiachuanbo2007sz](http://my.csdn.net/xiachuanbo2007sz)

[](http://blog.csdn.net/xiachuanbo2007sz/article/details/6705978# "[加关注]") [](http://my.csdn.net/my/letter/send/xiachuanbo2007sz "[发私信]")

* 访问：1315次
* 积分：132分
* 排名：千里之外

* 原创：12篇
* 转载：2篇
* 译文：0篇
* 评论：3条

文章搜索

[]()
文章分类

* [AJAX](http://blog.csdn.net/xiachuanbo2007sz/article/category/640926)(0)
* [J2EE 框架](http://blog.csdn.net/xiachuanbo2007sz/article/category/640915)(3)
* [java web](http://blog.csdn.net/xiachuanbo2007sz/article/category/640912)(6)
* [Java 基础](http://blog.csdn.net/xiachuanbo2007sz/article/category/640906)(2)
* [JavaScript](http://blog.csdn.net/xiachuanbo2007sz/article/category/640923)(1)

文章存档

* [2012年02月](http://blog.csdn.net/xiachuanbo2007sz/article/month/2012/02)(1)
* [2011年12月](http://blog.csdn.net/xiachuanbo2007sz/article/month/2011/12)(2)
* [2011年11月](http://blog.csdn.net/xiachuanbo2007sz/article/month/2011/11)(1)
* [2011年08月](http://blog.csdn.net/xiachuanbo2007sz/article/month/2011/08)(1)
* [2011年05月](http://blog.csdn.net/xiachuanbo2007sz/article/month/2011/05)(1)
* [2010年07月](http://blog.csdn.net/xiachuanbo2007sz/article/month/2010/07)(1)
* [2010年02月](http://blog.csdn.net/xiachuanbo2007sz/article/month/2010/02)(1)
* [2010年01月](http://blog.csdn.net/xiachuanbo2007sz/article/month/2010/01)(6)

展开
阅读排行

* [Myeclipse 9正式版 下载](http://blog.csdn.net/xiachuanbo2007sz/article/details/6397790 "Myeclipse 9正式版 下载") (404)
* [myeclipse8,9 link方式...]( "myeclipse8,9  link方式安装PropertiesEditor") (193)
* [Quartz 调度，添加，修改，删除 任...](http://blog.csdn.net/xiachuanbo2007sz/article/details/6926556 "Quartz 调度，添加，修改，删除 任务") (69)
* [session 会话时间长短设置](http://blog.csdn.net/xiachuanbo2007sz/article/details/5146484 "session 会话时间长短设置") (61)
* [关于request.getParamet...](http://blog.csdn.net/xiachuanbo2007sz/article/details/7082327 "关于request.getParameterMap()取值") (33)
* [使用javamail发送邮件](http://blog.csdn.net/xiachuanbo2007sz/article/details/5146391 "使用javamail发送邮件") (26)
* [java环境变量配置](http://blog.csdn.net/xiachuanbo2007sz/article/details/5146318 "java环境变量配置") (25)
* [Struts2 -- session的使...](http://blog.csdn.net/xiachuanbo2007sz/article/details/5285267 "Struts2 -- session的使用") (25)
* [JavaScript判断浏览器类型及版本](http://blog.csdn.net/xiachuanbo2007sz/article/details/5777697 "JavaScript判断浏览器类型及版本") (21)
* [tomcat 虚拟目录设置](http://blog.csdn.net/xiachuanbo2007sz/article/details/5146461 "tomcat 虚拟目录设置") (20)

评论排行

* [java环境变量配置](http://blog.csdn.net/xiachuanbo2007sz/article/details/5146318 "java环境变量配置") (1)
* [常用的jdbc连接字符串](http://blog.csdn.net/xiachuanbo2007sz/article/details/5146378 "常用的jdbc连接字符串") (1)
* [使用javamail发送邮件](http://blog.csdn.net/xiachuanbo2007sz/article/details/5146391 "使用javamail发送邮件") (1)
* [MD5加密](http://blog.csdn.net/xiachuanbo2007sz/article/details/7082339 "MD5加密") (0)
* [关于request.getParamet...](http://blog.csdn.net/xiachuanbo2007sz/article/details/7082327 "关于request.getParameterMap()取值") (0)
* [Quartz 调度，添加，修改，删除 任...](http://blog.csdn.net/xiachuanbo2007sz/article/details/6926556 "Quartz 调度，添加，修改，删除 任务") (0)
* [myeclipse8,9 link方式...]( "myeclipse8,9  link方式安装PropertiesEditor") (0)
* [Myeclipse 9正式版 下载](http://blog.csdn.net/xiachuanbo2007sz/article/details/6397790 "Myeclipse 9正式版 下载") (0)
* [JavaScript判断浏览器类型及版本](http://blog.csdn.net/xiachuanbo2007sz/article/details/5777697 "JavaScript判断浏览器类型及版本") (0)
* [Struts2 -- session的使...](http://blog.csdn.net/xiachuanbo2007sz/article/details/5285267 "Struts2 -- session的使用") (0)
推荐文章

最新评论

* [常用的jdbc连接字符串](http://blog.csdn.net/xiachuanbo2007sz/article/details/5146378#comments)

lingling_1026:
* [java环境变量配置](http://blog.csdn.net/xiachuanbo2007sz/article/details/5146318#comments)

lingling_1026:
* [使用javamail发送邮件](http://blog.csdn.net/xiachuanbo2007sz/article/details/5146391#comments)

lingling_1026:
      ![]() ![]()

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)北京创新乐知信息技术有限公司 版权所有, 京 ICP 证 070598 号世纪乐知(北京)网络技术有限公司 提供技术支持江苏乐知网络技术有限公司 提供商务支持![]() Email:webmaster@csdn.netCopyright © 1999-2012, CSDN.NET, All Rights Reserved[![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
