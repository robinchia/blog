---
layout: post
title: "基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客"
categories: 缓存
tags: 
 - 缓存
 - ehcache
--- 

# 基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客

### 分享到

* [QQ空间](http://passover.blog.51cto.com/#)
* [新浪微博](http://passover.blog.51cto.com/#)
* [百度搜藏](http://passover.blog.51cto.com/#)
* [人人网](http://passover.blog.51cto.com/#)
* [腾讯微博](http://passover.blog.51cto.com/#)
* [开心网](http://passover.blog.51cto.com/#)
* [腾讯朋友](http://passover.blog.51cto.com/#)
* [百度空间](http://passover.blog.51cto.com/#)
* [豆瓣网](http://passover.blog.51cto.com/#)
* [搜狐微博](http://passover.blog.51cto.com/#)
* [百度新首页](http://passover.blog.51cto.com/#)
* [QQ收藏](http://passover.blog.51cto.com/#)
* [和讯微博](http://passover.blog.51cto.com/#)
* [我的淘宝](http://passover.blog.51cto.com/#)
* [百度贴吧](http://passover.blog.51cto.com/#)
* [更多...](http://passover.blog.51cto.com/#)

[百度分享](http://passover.blog.51cto.com/#)

[![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/head_blog_sibebar_logo.gif)](http://blog.51cto.com/)[51CTO首页](http://www.51cto.com/)![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/top_bg_xian.gif)[51CTO博客](http://blog.51cto.com/)![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/top_bg_xian.gif)[我的博客](http://home.51cto.com/?reback=http%253A%252F%252Fpassover.blog.51cto.com%252F2431658%252F486709)![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/top_bg_xian.gif) [搜索](http://blog.51cto.com/search.php) [每日博报]()

![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/top_shequ.gif)社区：[论坛](http://bbs.51cto.com/)[博客](http://blog.51cto.com/)[下载](http://down.51cto.com/)[读书](http://book.51cto.com/)[更多![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/nav_ico1.gif)]()

[![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/dia.gif "今天领无忧币了吗？")](http://home.51cto.com/index.php?s=/Home/index)[登录](http://home.51cto.com/?reback=http%253A%252F%252Fpassover.blog.51cto.com%252F2431658%252F486709)[注册](http://ucenter.51cto.com/reg_01.php?reback=http%253A%252F%252Fpassover.blog.51cto.com%252F2431658%252F486709)
* [家园](http://home.51cto.com/)
* [微博](http://t.51cto.com/)
* [博客](http://blog.51cto.com/)
* [论坛](http://bbs.51cto.com/)
* [下载](http://down.51cto.com/)
* [自测](http://selftest.51cto.com/)
* [门诊](http://doctor.51cto.com/)
* [周刊](http://blog.51cto.com/newsletter/)
* [读书](http://book.51cto.com/)
* [技术圈](http://g.51cto.com/)
* [知道](http://zhidao.51cto.com/)

[![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/blogLogo01.png)](http://blog.51cto.com/blogcommend)

[
# passover【毕成功的博客】

](http://passover.blog.51cto.com/)

[http://passover.blog.51cto.com](http://passover.blog.51cto.com/)  [【复制】]() [](http://passover.blog.51cto.com/rss.php?uid=2431658)[【订阅】](http://passover.blog.51cto.com/rss.php?uid=2431658)

[原创](http://passover.blog.51cto.com/2431658/o "察看passover所有原创文章"):94[翻译](http://passover.blog.51cto.com/2431658/t "察看passover所有翻译文章"):2[转载](http://passover.blog.51cto.com/2431658/c "察看passover所有转载文章"):21

[博 客](http://passover.blog.51cto.com/)|[图库](http://passover.blog.51cto.com/pic/)|[写博文](http://passover.blog.51cto.com/addblog.php)|[帮 助](http://51ctoblog.blog.51cto.com/all/26414/4)
* [首页](http://passover.blog.51cto.com/#)|
* [Java](http://passover.blog.51cto.com/2431658/d-1)|
* [Game](http://passover.blog.51cto.com/2431658/d-15)|
* [Open Source](http://passover.blog.51cto.com/2431658/d-11)|
* [Design](http://passover.blog.51cto.com/2431658/d-5)|
* [Agile](http://passover.blog.51cto.com/2431658/d-4)|
* [Manage](http://passover.blog.51cto.com/2431658/d-9)|
* [Linux](http://passover.blog.51cto.com/2431658/d-7)|
* [Database](http://passover.blog.51cto.com/2431658/d-6)|
* [Server](http://passover.blog.51cto.com/2431658/d-16)|
* [Tool](http://passover.blog.51cto.com/2431658/d-13)|
* [Experience](http://passover.blog.51cto.com/2431658/d-10)|
* [Grails](http://passover.blog.51cto.com/2431658/d-3)|
* [Scala](http://passover.blog.51cto.com/2431658/d-2)|
* [Other](http://passover.blog.51cto.com/2431658/d-14)

## **passover** 的BLOG

![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/avatar.php)

[头像升级了？来看看>>](http://51ctoblog.blog.51cto.com/26414/928875)
[写留言](http://home.51cto.com/index.php?s=/space/2431658#message)[邀请进圈子](http://g.51cto.com/addgroup.php?uid=)[发消息](http://home.51cto.com/index.php?s=/Notify/write/uid/2431658) [加友情链接](http://passover.blog.51cto.com/)[进家园 加好友](http://home.51cto.com/index.php?s=/space/2431658)

## 博客统计信息

[**51CTO推荐博客**](http://blog.51cto.com/blogcommend)
用户名：passover
文章数：117
评论数：174
访问量：162194
无忧币：1682
[博客积分](http://51ctoblog.blog.51cto.com/26414/5591)：2780
[博客等级](http://51ctoblog.blog.51cto.com/26414/5591)：7
注册日期：2010-11-16
[距离博客No.1争夺赛结束还有 10 天](http://blog.51cto.com/active/no1/ing.html)
## 热门文章

* [按能力发薪酬？还是按贡..](http://passover.blog.51cto.com/2431658/531075)
* [利用Jsoup解析HTML](http://passover.blog.51cto.com/2431658/484673)
* [利用nginx+tomcat+memcac..](http://passover.blog.51cto.com/2431658/648182)
* [初试超轻量级actor框架—..](http://passover.blog.51cto.com/2431658/517931)
* [基于EHCache实现缓存去重]()
* [SolrJ学习的一点总结](http://passover.blog.51cto.com/2431658/568972)
* [代理IP自动切换的方法](http://passover.blog.51cto.com/2431658/502232)
* [Java游戏引擎libgdx的简介](http://passover.blog.51cto.com/2431658/803094)

## 搜索BLOG文章
## 我的技术圈([**2**](http://passover.blog.51cto.com/mygroup.php))

[更多>>](http://home.51cto.com/apps/group/index.php?s=/Index/index/)

* [SOA讨论与实践](http://g.51cto.com/soa)  
* [Scala](http://g.51cto.com/scala)  

## 最近访客

* [![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/avatar(1).php)](http://5985626.blog.51cto.com/)

[yyxxxn](http://5985626.blog.51cto.com/)
* [![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/avatar(2).php)](http://vnet111.blog.51cto.com/)

[vnet111](http://vnet111.blog.51cto.com/)
* [![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/avatar(3).php)](http://tntdba.blog.51cto.com/)

[vbbb625](http://tntdba.blog.51cto.com/)
* [![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/avatar(4).php)](http://2739597.blog.51cto.com/)

[naxxmm](http://2739597.blog.51cto.com/)
* [![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/avatar(5).php)](http://2951265.blog.51cto.com/)

[mumab..](http://2951265.blog.51cto.com/)
* [![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/avatar(6).php)](http://4733437.blog.51cto.com/)

[wangj..](http://4733437.blog.51cto.com/)
* [![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/avatar(7).php)](http://1711891.blog.51cto.com/)

[hjdon..](http://1711891.blog.51cto.com/)
* [![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/avatar(8).php)](http://5920597.blog.51cto.com/)

[睡着了](http://5920597.blog.51cto.com/)
* [![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/avatar(9).php)](http://fallenleaves.blog.51cto.com/)

[李惟忠](http://fallenleaves.blog.51cto.com/)
* [![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/avatar(10).php)](http://2798724.blog.51cto.com/)

[李大玉](http://2798724.blog.51cto.com/)
* [![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/avatar(11).php)](http://51ctohome.blog.51cto.com/)

[51cto..](http://51ctohome.blog.51cto.com/)
* [![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/avatar(12).php)](http://glories.blog.51cto.com/)

[glories](http://glories.blog.51cto.com/)
## 最新评论

* [**bbaobelief**](http://bbaobelief.blog.51cto.com/)：[你们是window啊，360修复一些漏洞，..](http://passover.blog.51cto.com/2431658/578817)
* [**Aquarius_**](http://aquarius.blog.51cto.com/)：[呵呵，感谢博主分享了](http://passover.blog.51cto.com/2431658/1004429)
* [匿名]aaaa：[极其的不靠谱，配置起访问一直报400](http://passover.blog.51cto.com/2431658/648182)
* [**passover**](http://passover.blog.51cto.com/)：[回复 whocares: pm管理员就能拿到..](http://passover.blog.51cto.com/2431658/836918)
* [匿名]whocares：[没有邀请码，等了半年都拿不到，郁闷啊](http://passover.blog.51cto.com/2431658/836918)
* [匿名]33：[3333发达省份是地方](http://passover.blog.51cto.com/2431658/487411)

## 51CTO推荐博文

[更多>>](http://blog.51cto.com/artcommend)

* [RSA加密解密相关 前端js加密，服..](http://johnchina.blog.51cto.com/4390273/1017894 "RSA加密解密相关 前端js加密，服务端java解密")
* [struts2.0中struts.xml配置文件详解](http://mrwlh.blog.51cto.com/2238823/1017728 "struts2.0中struts.xml配置文件详解 ")
* [快速构建Windows 8风格应用16-Set..](http://wzk89.blog.51cto.com/1660752/1017949 "快速构建Windows 8风格应用16-SettingContract原理及构建")
* [如何处理ORA-00376错误的恢复问题](http://2745881.blog.51cto.com/2735881/1017921 "如何处理ORA-00376错误的恢复问题")
* [ajax效果模拟——隐藏的iframe无..](http://woshixy.blog.51cto.com/5637578/1018125 "ajax效果模拟——隐藏的iframe无刷新效果")
* [用javascript做的一个简单的乒乓..](http://zhanfeng.blog.51cto.com/6109904/1018081 "用javascript做的一个简单的乒乓球游戏")
* [Rails开发细节《五》Migrations ..](http://virusswb.blog.51cto.com/115214/1019513 "Rails开发细节《五》Migrations 数据迁移")
* [SQLServer存储过程和ADO.NET访问..](http://yisuowushinian.blog.51cto.com/4241271/1016527 "SQLServer存储过程和ADO.NET访问存储过程")
* [快速构建Windows 8风格应用15-Sha..](http://wzk89.blog.51cto.com/1660752/1017700 "快速构建Windows 8风格应用15-ShareContract构建")
* [网络工作室暑假后第二次培训资料..](http://yisuowushinian.blog.51cto.com/4241271/1016524 "网络工作室暑假后第二次培训资料（SQLServer存储过程和ADO.NET访问存储过程）整理（一）")
* [MyBatis多参数传递之注解方式示例](http://legend2011.blog.51cto.com/3018495/1015003 "MyBatis多参数传递之注解方式示例")
## 友情链接

* [51CTO博客开发](http://51ctoblog.blog.51cto.com/ "51CTO博客开发") ![本周内更新](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/tubiao3.gif)

[2012年10月MVP名单公布啦！](http://51ctomvp.blog.51cto.com/192695/1019865)[周刊：从需求看《IT人员应聘建议》](http://blog.51cto.com/newsletter/175/)[【有奖门诊】探索式测试的奥秘](http://doctor.51cto.com/develop-271.html)

[博主的更多文章>>](http://passover.blog.51cto.com/all/2431658)

[![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/tj.png)](http://blog.51cto.com/artcommend)

![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/artType01.jpg) 基于EHCache实现缓存去重 2011-01-27 16:15:10
标签：[缓存](http://blog.51cto.com/tagindex.php?keyword=%BB%BA%B4%E6) [职场](http://blog.51cto.com/tagindex.php?keyword=%D6%B0%B3%A1) [休闲](http://blog.51cto.com/tagindex.php?keyword=%D0%DD%CF%D0) [ehcache](http://blog.51cto.com/tagindex.php?keyword=ehcache)

     由于近期的工作主要集中在数据处理上，而性能问题时而暴露出来，我对需要处理的数据进行了一下简单的分析，发现存在大量的重复数据，这自然让我想到了去建立一个二级缓存把曾经处理过的数据缓存起来，避免重复处理。我们业务上其实就是对最近处理过的数据重复出现几率比较高，所以有一个几百兆的内存空间用LRU的策略进行去重应该就足够了。

    其实可以选择的方案有很多，初步筛选了一下，我决定在对Java支持度比较好且应用广泛的OSCache和EHCache中选一个。上了官网一查，发现OSCache在几年前就停止更新了，而EHCache则一直有公司在维护，所以自然选定了后者。官方文档的地址是[http://www.ehcache.org/documentation/index.html](http://www.ehcache.org/documentation/index.html)。

    先让同事研究了一下，弄的是1.7.0的版本，这个版本比较好的是对其他包的依赖很少，很快就把自己的demo建立起来了。然后上官网下载了最新的版本ehcache-core-2.3.1-distribution，里面其实就多了两个slf4j的包，原来的代码一行没动就可以运行起来了。这里新版本加入的内容比较多，jar包就是1.7.0的3倍大，看了下官方文档的说明，主要加入的就是对分布式的支持，当然还有很多新特性。新特性以后慢慢研究吧，我目前也用不到什么高级功能，既然新版本使用起来也很方便，那就用这个好了。

    这里简单解释一下，我们原先想试一下他提供的FIFO和LRU的策略，结果刚开始测试输出的结果和预期居然不一致，官方文档上也没看到相应的解释。经过反复测试，感觉他不是严格按照这个策略来，可能是算法有些问题吧。

    另外补充一下参数设置的经验。

1. 对于存储对象个数的设置：由于配置文件中只能指定maxElementsInMemory，这就会有可能存入的对象太多而超出VM的heap大小，当然你可以通过jvm参数增大heap大小，但这总还是有可能溢出。这里可以把maxElementsInMemory值设置到一个比较安全的大小，自己预先测试一下最好。如果内存仍然存不下你需要存的对象个数，那么可以开启overflowToDisk来增加可以存储的Element个数。这里要注意一下，EHCache不会自动帮助你去把内存对象写入到磁盘，当超过maxElementsInMemory程序会自动把更多的部分开始往硬盘写，但是内存的对象其实并没有清出去，这时需要手动使用Cache.flush()方法来把内存对象清出去。 对于是否需要用磁盘扩充缓存，这个还是根据自己应用判断。
1. 重建上一次运行的缓存：这个需求肯定比较普遍，我们当然不希望一旦程序退出，整个缓存就要重建了。开启diskPersistent功能，只要使用的是CacheManager单例模式，下一次启动的时候就会调用上一次运行的缓存。比较麻烦的是写入磁盘的时间还是要自己调用 Cache.flush()方法。如果仅仅考虑到程序重启的话，我建议这里把diskStore写入到一个ramfs，这样性能就更高了，但重启电脑的话就不得不重建缓存了。
1. 索引的建立：如果你测试的maxElementsOnDisk量比较大的话，我本地设置成100000，那么你会在临时文件目录下看到有index文件，这个文件显然是对磁盘上的文件建立索引，加快查询速度。所以这个索引是否建立是EHCache自动帮你做的，你不用操心了。
1. 写入缓存的速度：在本地不断调整参数，惊讶的发现有时写入缓存的速度奇慢。因为开启了 diskPersistent功能，程序运行时经常会卡在backOffIfDiskSpoolFull方法上，作用是If the disk store spool is full wait a short time to give it a chance to catch up。这个参数对应的是diskSpoolBufferSizeMB，默认是30MB，如果存储的对象比较多，强烈建议调大，或者直接把 diskPersistent关掉。
1. 缓存内容写入磁盘的速度：如果你把maxElementsOnDisk参数配置的远小于maxElementsInMemory值的话，你会发现速度又变得很慢，这是因为程序一直要去找到底哪些内容应该写入磁盘。建议这 maxElementsOnDisk值的配置应该不小于 maxElementsInMemory，其实正常用应该也会这么配置，只是我测试无聊试了下出问题了。
1. 内存Element数量不能控制：一旦开启了 diskPersistent，惊讶的发现居然设置的 maxElementsInMemory无效了，内存的 Element数量一直在增长。反复测试，问题稳定重现，又找不到解释，无奈之下给官方提了个bug，等答复吧，汗...

 

     这里还是给出测试代码，主要用到的就是一个ehcache.xml的配置文件，定义了Cache的具体策略。程序调用的时候非常方便，就是典型key/value的方式。

1. <?xml version="1.0" encoding="UTF-8"?> 
1. <ehcache> 
1.  
1.     <diskStore path="java.io.tmpdir" /> 
1.      
1.     <defaultCache  
1.         maxElementsInMemory="10000"  
1.         eternal="false" 
1.         timeToIdleSeconds="120"  
1.         timeToLiveSeconds="120"  
1.         overflowToDisk="true" 
1.         diskPersistent="false"  
1.         diskExpiryThreadIntervalSeconds="120" 
1.         memoryStoreEvictionPolicy="LRU"  
1.     /> 
1.          
1.     <cache name="sample"          
1.         maxElementsInMemory="5"   
1.         maxElementsOnDisk = "5"  
1.         eternal="false"   
1.         timeToIdleSeconds="1440"   
1.         diskPersistent="false"  
1.         timeToLiveSeconds="2880"   
1.         overflowToDisk="false"  
1.         memoryStoreEvictionPolicy="FIFO"  
1.         statistics="true"  
1.     /> 
1.  
1. </ehcache> 

    以下就是测试代码：
1. **package ehcache; **
1. ** **
1. **import org.apache.log4j.Logger; **
1. ** **
1. **import net.sf.ehcache.Cache; **
1. **import net.sf.ehcache.CacheManager; **
1. **import net.sf.ehcache.Element; **
1. ** **
1. **public class EhcacheTest { **
1. **     **
1. **    private static final Logger logger = Logger.getLogger(EhcacheTest.class); **
1. **     **
1. **    private static Cache sampleCache = null; **
1. **     **
1. **    public static void main(String[] args) { **
1. **        init(); **
1. **        test(); **
1. **    } **
1. ** **
1. **    private static void test() { **
1. **        logger.info(sampleCache.getMemoryStoreEvictionPolicy()); **
1. **        for(int i=0; i<10; i++){ **
1. **            //写入缓存 **
1. **            sampleCache.put(new Element(i, "v" + i)); **
1. **            //打印当前缓存的所有值 **
1. **            logger.info(sampleCache.getKeys()); **
1. **            //读取缓存 **
1. **            Element e = sampleCache.get(i); **
1. **            logger.info(e.getValue()); **
1. **        } **
1. **        //打印命中统计 **
1. **        logger.info(sampleCache.getStatistics()); **
1. **    } **
1. **     **
1. **    private static void init() { **
1. **        CacheManager manager = CacheManager.create(); **
1. **//      manager.addCache("sample"); //已经在配置文件定义过了 **
1. **        sampleCache = manager.getCache("sample"); 
**
1. **    } **
1. ** **
1. **} **

 
![分享至](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/ssk.png) [](http://passover.blog.51cto.com/# "分享到新浪微博") [](http://passover.blog.51cto.com/# "分享到QQ空间") [](http://passover.blog.51cto.com/# "分享到腾讯微博") [](http://passover.blog.51cto.com/# "分享到百度搜藏") [](http://passover.blog.51cto.com/# "分享到人人网") [](http://passover.blog.51cto.com/# "分享到QQ收藏") [](http://passover.blog.51cto.com/# "分享到豆瓣网") [](http://passover.blog.51cto.com/# "分享到百度空间") [](http://passover.blog.51cto.com/# "分享到MSN") ** 更多 [0](http://passover.blog.51cto.com/# "累计分享0次") [![一键收藏，随时查看，分享好友！](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/favorite_add_small.gif)]( "一键收藏，随时查看，分享好友！")

![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/icon01.png) [姜周、倔强的土豆]() 2人 了这篇文章

类别：[Open Source](http://passover.blog.51cto.com/2431658/d-11)┆[技术圈]()(0)┆阅读(3391)┆评论(2) ┆ [推送到技术圈](http://passover.blog.51cto.com/#)┆[返回首页](http://passover.blog.51cto.com/)
上一篇 [利用Jsoup解析HTML](http://passover.blog.51cto.com/2431658/484673 "利用Jsoup解析HTML") 下一篇 [初试Redis的感受](http://passover.blog.51cto.com/2431658/487411 "初试Redis的感受")

## 相关文章

* [EhCache构建Hibernate二级缓存](http://wujuxiang.blog.51cto.com/2250829/422377 "EhCache构建Hibernate二级缓存")
* [CentOS 5.5环境下安装配置Varnish](http://kerry.blog.51cto.com/172631/402923 "CentOS 5.5环境下安装配置Varnish")
* [如何删除系统漏洞修复的缓存文件夹](http://sdbaby.blog.51cto.com/149645/303987 "如何删除系统漏洞修复的缓存文件夹")
* [网站性能优化之应用程序缓存-初篇](http://8807131.blog.51cto.com/2425232/435135 "网站性能优化之应用程序缓存-初篇")
* [域账号缓存登陆的故障排除一例](http://rickyfang.blog.51cto.com/1213/130032 "域账号缓存登陆的故障排除一例")
* [ASP.NET数据缓存](http://yueyuanyuan.blog.51cto.com/1342062/337873 "ASP.NET数据缓存")
[]()

## 文章评论

 []()

[1楼]    ![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/reply.gif)  [**姜周**](http://jiangzhou.blog.51cto.com/) [回复](http://passover.blog.51cto.com/#)

2011-01-28 14:44:20
支持一个！
[]()

[2楼]    ![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/reply.gif)  [**倔强的土豆**](http://juejiangdetudou.blog.51cto.com/) [回复](http://passover.blog.51cto.com/#)

2012-08-24 17:55:22
不错，顶了。
 
 []()

[]()

## 发表评论            

[2013年1月MVP申请公告[截止10月15日]](http://51ctomvp.blog.51cto.com/192695/1019565)
昵  称： [登录](http://home.51cto.com/index.php?reback=http%253A%252F%252Fpassover.blog.51cto.com%252F2431658%252F486709)  [快速注册](http://ucenter.51cto.com/reg_01.php?reback=http://blog.51cto.com) 验证码： ![]()

点击图片可刷新验证码请点击后输入验证码[博客过2级，无需填写验证码](http://51ctoblog.blog.51cto.com/26414/5591) 内  容：

同时赞一个
返回顶部

Copyright By 51CTO.COM 版权所有
[![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/copyright.jpg)](http://blog.51cto.com/)
[![](http://./基于EHCache实现缓存去重 - passover【毕成功的博客】 - 51CTO技术博客_files/pic.gif)](http://www.cnzz.com/stat/website.php?web_id=4274540 "站长统计")
