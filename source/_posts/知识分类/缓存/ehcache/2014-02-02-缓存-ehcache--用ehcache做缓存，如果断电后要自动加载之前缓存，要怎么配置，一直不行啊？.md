---
layout: post
title: "用ehcache做缓存，如果断电后要自动加载之前缓存，要怎么配置，一直不行啊？"
categories: 缓存
tags: 
 - 缓存
 - ehcache
--- 

# 用ehcache做缓存，如果断电后要自动加载之前缓存，要怎么配置，一直不行啊？

* [首页](http://www.oschina.net/)
* [开源软件](http://www.oschina.net/project)
* [讨论区](http://www.oschina.net/question)
* [代码分享](http://www.oschina.net/code/list)
* [资讯](http://www.oschina.net/news)
* [博客](http://www.oschina.net/blog)
* [Android](http://www.oschina.net/android)
* [招聘](http://www.oschina.net/job)

当前访客身份：游客 [ [登录](http://www.oschina.net/home/login) | [加入开源中国](http://www.oschina.net/home/reg) ]

当前位置：[讨论区](http://www.oschina.net/question) » [技术问答](http://www.oschina.net/question?catalog=1) » [EhCache](http://www.oschina.net/p/ehcache)

软件 代码 讨论区 新闻 博客
[![zwt]( "zwt")](http://my.oschina.net/zwtlong)

# [用ehcache做缓存，如果断电后要自动加载之前缓存，要怎么配置，一直不行啊？]()

[zwt](http://my.oschina.net/zwtlong) 发表于 5-3 13:09 5个月前, [4](http://www.oschina.net/question/146494_52050#answers)回/600阅, 最后回答: 1个月前
**[讨论区](http://www.oschina.net/question) » [技术问答](http://www.oschina.net/question?catalog=1)**

【杭州】开源中国-源创会第十三期开始报名 [我要报名»](http://www.oschina.net/question/28_72721)
在使用ehcache  如果碰到意外情况，断电或服务器挂掉，我希望重启后 ehcache 能自动加载之前的缓存，但一直试都不行，不知道要怎么配置；

目前在重启服务器的时候会提示：

警告: The index for data file stake.data is out of date, probably due to an unclean shutdown. Deleting index file stake.index

目前配置：
[view source](http://www.oschina.net/question/146494_52050#viewSource "view source")[print](http://www.oschina.net/question/146494_52050#printSource "print")[?](http://www.oschina.net/question/146494_52050#about "?")

01

<

diskStore

path

=

"java.io.tmpdir"

/>

02
 
03

      

<

defaultCache

04

            

maxElementsInMemory

=

"10000"
05

            

eternal

=

"false"

06

            

timeToIdleSeconds

=

"43200"
07

            

timeToLiveSeconds

=

"43200"

08

            

overflowToDisk

=

"true"
09

            

diskPersistent

=

"true"

10

            

diskExpiryThreadIntervalSeconds

=

"120"
11

            

memoryStoreEvictionPolicy

=

"LFU"

12

            

/>

**标签：** [EhCache](http://www.oschina.net/question/tag/ehcache "Java缓存框架 EhCache") [Java](http://www.oschina.net/question/tag/java "编程语言 Java")
[我想问同样的问题]()  共*1*个人想要问同样的问题 [补充话题说明»]()

**分享到** []( "分享到新浪微博")[]( "分享到腾讯微博")**

[收藏]( "收藏此话题")
**

6
**

[举报]()
**

[踩]( "踩：这问题不知道在说什么，或者没什么用") 0 | [顶]( "顶：这问题很有用或者很清晰明了") 0
**
## [按评价排序](http://www.oschina.net/question/146494_52050#answers) | [显示最新答案](http://www.oschina.net/question/146494_52050?sort=time#answers) | [回页面顶部](http://www.oschina.net/question/146494_52050#top)  []()共有*4*个答案 [我要回答»](http://www.oschina.net/question/146494_52050#answerform)

* [![zwt]( "zwt")](http://my.oschina.net/zwtlong)

[zwt](http://my.oschina.net/zwtlong) 回答于 2012-05-03 13:32

[举报]()
已经解决，在ehcache初始化之前

System.setProperty("net.sf.ehcache.enableShutdownHook","true");

请看 [http://stackoverflow.com/questions/2373431/ehcache-disk-store-unclean-shutdown](http://stackoverflow.com/questions/2373431/ehcache-disk-store-unclean-shutdown)

 

 
[有帮助]( "这是一个好答案，能解决问题")*(0)* | [没帮助]( "这答案无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此答案](http://www.oschina.net/question/answer?question=52050&answer=207929)
* [![samsamsam]( "samsamsam")](http://my.oschina.net/u/558914)

[samsamsam](http://my.oschina.net/u/558914) 回答于 2012-05-14 16:29

[举报]()
我想请教，我按照

System.setProperty("net.sf.ehcache.enableShutdownHook","true");

这个方法试过还是The index for data file is out of date。。。。。
**--- 共有 1 条评论 ---**

* [![zwt]( "zwt")](http://my.oschina.net/zwtlong)  你要在加载ehcache之前设置环境变量，设置成功后看会输出log (4个月前 by zwt) [回复]()

[有帮助]( "这是一个好答案，能解决问题")*(0)* | [没帮助]( "这答案无法解决问题，或者模糊不清")*(0)* | [评论]()*(1)* | [引用此答案](http://www.oschina.net/question/answer?question=52050&answer=216080)
* [![fbfan520]( "fbfan520")](http://my.oschina.net/u/732846)

[fbfan520](http://my.oschina.net/u/732846) 回答于 2012-09-05 14:00

[举报]()
 System.setProperty("net.sf.ehcache.enableShutdownHook","true");请问这句加在哪里
**--- 共有 1 条评论 ---**

* [![zwt]( "zwt")](http://my.oschina.net/zwtlong)  在ehcache初始化之前 (1个月前 by zwt) [回复]()

[有帮助]( "这是一个好答案，能解决问题")*(0)* | [没帮助]( "这答案无法解决问题，或者模糊不清")*(0)* | [评论]()*(1)* | [引用此答案](http://www.oschina.net/question/answer?question=52050&answer=297289)
* [![fbfan520]( "fbfan520")](http://my.oschina.net/u/732846)

[fbfan520](http://my.oschina.net/u/732846) 回答于 2012-09-07 13:35

[举报]()
在ehcache初始化之前是什么意思？ public static CacheManager manager = CacheManager.create("D:\\XML\\ehcache.xml");
    public static Cache diskOnlyEternalCache = null;
    public static Cache diskEternalCache = null;
    public static Cache disk7DayCache = null;
    public static Cache diskSyn7DayCache = null;
    public static Cache disk1DayCache = null;

 

    private CacheManageService() {

    }

    public synchronized static Cache getDiskOnlyEternalCache() {

        if (diskOnlyEternalCache == null) {
            diskOnlyEternalCache = manager.getCache("diskOnlyEternalCache");
        }
        return diskOnlyEternalCache;
    }
像这个程序我加在哪里？
[有帮助]( "这是一个好答案，能解决问题")*(0)* | [没帮助]( "这答案无法解决问题，或者模糊不清")*(0)* | [评论]()*(0)* | [引用此答案](http://www.oschina.net/question/answer?question=52050&answer=298897)

[![非会员用户]( "非会员用户")](http://www.oschina.net/question/$link.user($g_user))   []( "粗体(Ctrl+B)")[]( "斜体(Ctrl+I)")[]( "下划线(Ctrl+U)")[]( "删除线")[]( "删除格式")[]( "编号")[]( "项目符号")[]( "文字颜色")[]( "文字背景")[]( "字体")[]( "文字大小")[]( "超级链接")[]( "取消超级链接")[]( "插入表情")[]( "插入程序代码或脚本")[]( "图片")[]( "插入Flash")[]( "引用某段文字")[]( "全选")[]( "HTML代码")[]( "关于")
[回答案顶部](http://www.oschina.net/question/146494_52050#answers) | [回页面顶部](http://www.oschina.net/question/146494_52050#top)
有什么技术问题吗？ [我要提问](http://www.oschina.net/question/ask)

**[全部(3)...](http://my.oschina.net/zwtlong/?ft=bbs&scope=2&showme=1)*zwt*的其他问题**

* [oschina 评论盖楼是怎么实现的？](http://www.oschina.net/question/146494_68822 "oschina 评论盖楼是怎么实现的？")(1回/97阅,1个月前)
* [extremetable 页数过多的情况显示怎么处理](http://www.oschina.net/question/146494_49003 "extremetable 页数过多的情况显示怎么处理")(0回/50阅,6个月前)
**类似的话题**

* [ehcache并发的问题。](http://www.oschina.net/question/257044_65201 "ehcache并发的问题。")(2回/151阅,1个月前)
* [EhCache cluster 不能复制](http://www.oschina.net/question/584947_60156 "EhCache cluster 不能复制")(2回/121阅,3个月前)
* [EHCache RMI同步](http://www.oschina.net/question/731217_67787 "EHCache RMI同步")(2回/98阅,1个月前)
* [ehcache缓存失效问题](http://www.oschina.net/question/1705_22910 "ehcache缓存失效问题")(4回/539阅,1年前)
* [请教ehcache的notifyElementExpired的事件用法](http://www.oschina.net/question/911_51821 "请教ehcache的notifyElementExpired的事件用法")(7回/138阅,5个月前)
* [EhCache 缓存嵌入对象问题](http://www.oschina.net/question/113136_43994 "EhCache 缓存嵌入对象问题")(7回/317阅,7个月前)
* [ehcache如何增量加入缓存](http://www.oschina.net/question/109139_16386 "ehcache如何增量加入缓存")(1回/410阅,1年前)
* [ehcache分布式同步问题](http://www.oschina.net/question/254302_44560 "ehcache分布式同步问题")(3回/360阅,7个月前)
* [ehcache如何设置输出打印日志](http://www.oschina.net/question/102297_12816 "ehcache如何设置输出打印日志")(4回/574阅,1年前)
* [Ehcache 2.4.1 Search 问题！！！！](http://www.oschina.net/question/138851_19238 "Ehcache 2.4.1 Search 问题！！！！")(3回/251阅,1年前)
* [Ehcache配置文件中各项参数](http://www.oschina.net/question/35932_3865 "Ehcache配置文件中各项参数")(1回/722阅,2年前)
* [在ehcache中如何配置多个diskStore](http://www.oschina.net/question/575002_57827 "在ehcache中如何配置多个diskStore")(2回/81阅,3个月前)
* [ehcache2.5缓存问题，求解？？？？？？](http://www.oschina.net/question/106591_47184 "ehcache2.5缓存问题，求解？？？？？？")(5回/294阅,6个月前)
* [跑EhCache 集群演示程序读cache的值为null](http://www.oschina.net/question/112732_14500 "跑EhCache 集群演示程序读cache的值为null")(0回/166阅,1年前)
* [用了红薯大哥的EhCache缓存代码，调试后出错，各位大侠能否给一个些建议？](http://www.oschina.net/question/203545_45215 "用了红薯大哥的EhCache缓存代码，调试后出错，各位大侠能否给一个些建议？")(7回/425阅,6个月前)
* [Ehcache gzipFilter](http://www.oschina.net/question/37023_40508 "Ehcache gzipFilter")(3回/250阅,7个月前)

© 开源中国社区(OsChina.NET) | [关于我们](http://www.oschina.net/home/about) | [广告联系](mailto:oschina.net@gmail.com) | [@新浪微博](http://weibo.com/oschina2010) | [开源中国手机版](http://m.oschina.net/) | 粤ICP备12009483号-3

[]()[]()[]()
![]()
