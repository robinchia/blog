---
layout: post
title: "Ehcache配置文件加载方式 - crazyinsomnia的空间 - 开源中国社区"
categories: 缓存
tags: 
 - 缓存
 - ehcache
--- 

# Ehcache配置文件加载方式 - crazyinsomnia的空间 - 开源中国社区

[开源中国社区](http://www.oschina.net/ "开源中国社区首页")

10月20日杭州OSC源创会 [正在报名](http://www.oschina.net/question/28_72721) 中
* [软件](http://www.oschina.net/project)
* [讨论](http://www.oschina.net/question)
* [代码](http://www.oschina.net/code/list)
* [资讯](http://www.oschina.net/news)
* [博客](http://www.oschina.net/blog)
* [Android](http://www.oschina.net/android)
* [招聘](http://www.oschina.net/job)

当前访客身份： 游客 [ [登录](http://www.oschina.net/home/login?goto_page=http%3A%2F%2Fmy.oschina.net%2Fcrazyinsomnia) | [加入开源中国](http://www.oschina.net/home/reg) ]  [你有*0*新留言](http://www.oschina.net/home/login?goto_page=http%3A%2F%2Fmy.oschina.net%2Fcrazyinsomnia "进入我的留言箱")

软件 代码 讨论区 新闻 博客

软件

* [软件](http://my.oschina.net/crazyinsomnia/blog/3552#1)
* [代码](http://my.oschina.net/crazyinsomnia/blog/3552#2)
* [讨论区](http://my.oschina.net/crazyinsomnia/blog/3552#3)
* [新闻](http://my.oschina.net/crazyinsomnia/blog/3552#4)
* [博客](http://my.oschina.net/crazyinsomnia/blog/3552#5)
[![crazyinsomnia]( "crazyinsomnia")](http://my.oschina.net/crazyinsomnia)  [crazyinsomnia](http://my.oschina.net/crazyinsomnia "男")  ![]( "男") [关注此人]( "成为TA的粉丝")

[关注(10)](http://my.oschina.net/crazyinsomnia/fellow) [粉丝(16)](http://my.oschina.net/crazyinsomnia/fans) [积分(47)](http://www.oschina.net/question/3307_20931 "查看OSCHINA积分规则")

青春是人生的实验课，错也错的很值得！！
[.发送留言]() [.请教问题](http://www.oschina.net/question/ask?user=30362)

**博客分类**

* [Spring](http://my.oschina.net/crazyinsomnia/blog?catalog=62638)(1)
* [Freemarker](http://my.oschina.net/crazyinsomnia/blog?catalog=29443)(1)
* [java](http://my.oschina.net/crazyinsomnia/blog?catalog=22602)(12)
* [Jquery](http://my.oschina.net/crazyinsomnia/blog?catalog=22687)(2)
* [Linux](http://my.oschina.net/crazyinsomnia/blog?catalog=28068)(3)
* [Velocity](http://my.oschina.net/crazyinsomnia/blog?catalog=24660)(1)
* [Hibernate](http://my.oschina.net/crazyinsomnia/blog?catalog=27928)(1)
* [Struts2](http://my.oschina.net/crazyinsomnia/blog?catalog=27635)(6)
* [Htmlparser](http://my.oschina.net/crazyinsomnia/blog?catalog=28479)(1)
* [数据库](http://my.oschina.net/crazyinsomnia/blog?catalog=25478)(2)
* [CSS+Html+Javascript](http://my.oschina.net/crazyinsomnia/blog?catalog=24533)(5)
* [工作日志](http://my.oschina.net/crazyinsomnia/blog?catalog=13879)(0)
* [日常记录](http://my.oschina.net/crazyinsomnia/blog?catalog=13880)(3)
* [转贴的文章](http://my.oschina.net/crazyinsomnia/blog?catalog=13881)(0)
**阅读排行**

1. [1. WebApplicationContext : org.springframework.web.context.ContextLoaderListener作用](http://my.oschina.net/crazyinsomnia/blog/3370)
1. [2. hibernate学习笔记：hibernate中的Cache管理](http://my.oschina.net/crazyinsomnia/blog/2231)
1. [3. Document.location.href和document.location.replace的区别](http://my.oschina.net/crazyinsomnia/blog/3490)
1. [4. web.xml 中的listener、 filter、servlet 加载顺序及其详解](http://my.oschina.net/crazyinsomnia/blog/12518)
1. [5. jquery的extend和fn.extend](http://my.oschina.net/crazyinsomnia/blog/2233)
1. [6. Struts2中的ActionContext](http://my.oschina.net/crazyinsomnia/blog/3400)
1. [7. Struts2执行流程【图】](http://my.oschina.net/crazyinsomnia/blog/3848)
1. [8. Ehcache配置文件加载方式]()

**最新评论**

* [@lfsfxy9](http://my.oschina.net/u/242075)：相同的内容，有必要重复一遍吗？！ [查看»](http://my.oschina.net/action/tweet/go?obj=269119659&type=18&user=242075)
* [@被风遗忘](http://my.oschina.net/chengjiansunboy)：引用来自“senge”的评论 CacheManager manager... [查看»](http://my.oschina.net/action/tweet/go?obj=268835524&type=18&user=221603)
* [@senge](http://my.oschina.net/senge)：CacheManager manager = new CacheManager(url);... [查看»](http://my.oschina.net/action/tweet/go?obj=264235413&type=18&user=106591)
* [@哥不是传说](http://my.oschina.net/laddygaga)： [查看»](http://my.oschina.net/action/tweet/go?obj=259653898&type=18&user=111291)
* [@红薯](http://my.oschina.net/javayou)：不需要 from dual 啦， select now();... [查看»](http://my.oschina.net/action/tweet/go?obj=254022309&type=18&user=12)
* [@crazyinsomnia](http://my.oschina.net/crazyinsomnia)：嘻嘻，是啊！：） [查看»](http://my.oschina.net/action/tweet/go?obj=254017055&type=18&user=30362)
* [@红薯](http://my.oschina.net/javayou)：通过URL加载，这个说法不太对哦 getClass().get... [查看»](http://my.oschina.net/action/tweet/go?obj=254015753&type=18&user=12)
**访客统计**

* 今日访问：21
* 昨日访问：22
* 本周访问：86
* 本月访问：175
* 所有访问：21437

[空间](http://my.oschina.net/crazyinsomnia) » [博客](http://my.oschina.net/crazyinsomnia/blog) » [java](http://my.oschina.net/crazyinsomnia/blog?catalog=22602) » 博客正文

# ![]() Ehcache配置文件加载方式

*0*人收藏此文章,  [我要收藏]()   发表于2年前(2010-04-01 00:00) , 已有**924**次阅读 共**[4](http://my.oschina.net/crazyinsomnia/blog/3552#comments)**个评论

会在classpath路径下找ehcache.xml配置文件

CacheManager manager = new CacheManager();   

也可以根据相对文件路径来加载配置文件

CacheManager manager = new CacheManager(“src/ehcache.xml”); .

通过相对与类路径的位置加载   

URL url = getClass().getResource(“/ehcache.xml”); 

CacheManager manager = new CacheManager(url);

通过流加载

InputSream fis = new FileInputStream(new File(“src/config/ehcache.xml”).getAbsolutePath());
声明：OSCHINA 博客文章版权属于作者，受法律保护。未经作者同意不得转载。

* [« Linux总结----VI文件编辑器](http://my.oschina.net/crazyinsomnia/blog/3547 "上一篇：Linux总结----VI文件编辑器")
* [MySQL获得系统时间 »](http://my.oschina.net/crazyinsomnia/blog/3562 "下一篇：MySQL获得系统时间")

开源中国-程序员在线工具：[API文档大全(120+)](http://www.osctools.net/apidocs) [JS在线编辑演示](http://www.osctools.net/jsbin) [二维码](http://www.osctools.net/qr) [更多>>](http://www.osctools.net/)
[]( "分享到新浪微博") []( "分享到腾讯微博") []( "分享到开心网") []( "分享到人人网") []( "分享到豆瓣") [](http://www.jiathis.com/share?uid=1567353) [0]()     [顶]()已有 *0*人顶
## []()共有 4 条网友评论

* [![红薯]( "红薯")](http://my.oschina.net/javayou) 1楼：[红薯](http://my.oschina.net/javayou) 发表于 2010-04-01 07:39 [回复此评论]()

通过URL加载，这个说法不太对哦 getClass().getResource(....) 主要是从类路径中加载
* [![crazyinsomnia]( "crazyinsomnia")](http://my.oschina.net/crazyinsomnia) 2楼：[crazyinsomnia](http://my.oschina.net/crazyinsomnia) 发表于 2010-04-01 09:27 [回复此评论]()

嘻嘻，是啊！：）
* [![senge]( "senge")](http://my.oschina.net/senge) 3楼：[senge](http://my.oschina.net/senge) 发表于 2011-11-13 17:37 [回复此评论]()

CacheManager manager = new CacheManager(url);
url 是WEB－INF　下面的配置文件　怎么加载不进来啊！
* [![被风遗忘]( "被风遗忘")](http://my.oschina.net/chengjiansunboy) 4楼：[被风遗忘](http://my.oschina.net/chengjiansunboy) 发表于 2012-08-05 22:39 [回复此评论]()

### 引用来自“senge”的评论

CacheManager manager = new CacheManager(url);
url 是WEB－INF　下面的配置文件　怎么加载不进来啊！
应该是读取的位置不对的吧.

文明上网，理性发言
[]()

![]() 文明上网，理性发言  [回到页首](http://my.oschina.net/crazyinsomnia/blog/3552#) | [回到评论列表](http://my.oschina.net/crazyinsomnia/blog/3552#comments)

**[关闭]()相关文章阅读**

* 2012/07/29 [Nagios的配置文件](http://my.oschina.net/u/615185/blog/69693 "Nagios的配置文件")
* 2012/06/04 [读取properties格式的配置文件...](http://my.oschina.net/chaoren8/blog/60918 "读取properties格式的配置文件")
* 2012/01/29 [更新了VIM的配置-_vimc（2012年1月版...](http://my.oschina.net/superoscar/blog/39502 "更新了VIM的配置-_vimc（2012年1月版）")
* 2012/08/14 [hadoop配置文件](http://my.oschina.net/u/617085/blog/72692 "hadoop配置文件")
* 2012/06/04 [编写调用配置文件](http://my.oschina.net/bsnfei/blog/60900 "编写调用配置文件")

© 开源中国社区(OsChina.NET) | [关于我们](http://www.oschina.net/home/about) | [广告联系](mailto:oschina.net@gmail.com) | [@新浪微博](http://weibo.com/oschina2010) | [开源中国手机版](http://m.oschina.net/) | 粤ICP备12009483号-3

[]()[]()[]()
![]()
