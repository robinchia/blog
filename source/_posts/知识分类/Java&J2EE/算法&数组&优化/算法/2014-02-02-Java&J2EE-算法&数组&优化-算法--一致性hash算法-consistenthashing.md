---
layout: post
title: "一致性hash算法 - consistent hashing"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 算法
--- 

# 一致性hash算法 - consistent hashing

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

# [sparkliang的专栏](http://blog.csdn.net/sparkliang)

##

* [![]()目录视图](http://blog.csdn.net/sparkliang?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/sparkliang?viewmode=list)
* [![]()订阅](http://blog.csdn.net/sparkliang/rss/list)
[专访赵霏：应把握机会 HTML5游戏开发已臻成熟](http://www.csdn.net/article/2013-07-09/2816160)        [2013年7月微软MVP当选名单揭晓](http://blog.csdn.net/blogdevteam/article/details/9226601)      [CSDN博客频道自定义摘要、图片水印、热门标签等功能上线啦](http://blog.csdn.net/csdnproduct/article/details/9226265)      [CSDN博客第二期云计算最佳博主评选](http://blog.csdn.net/blogdevteam/article/details/9136613)      []()

### [一致性hash算法 - consistent hashing]()

分类： [算法艺术](http://blog.csdn.net/sparkliang/article/category/488397)  2010-02-02 09:19 46510人阅读 [评论]()(80) [收藏]( "收藏") [举报]( "举报")
[算法](http://blog.csdn.net/tag/details.html?tag=%e7%ae%97%e6%b3%95)[cache](http://blog.csdn.net/tag/details.html?tag=cache)[object](http://blog.csdn.net/tag/details.html?tag=object)[服务器](http://blog.csdn.net/tag/details.html?tag=%e6%9c%8d%e5%8a%a1%e5%99%a8)[存储](http://blog.csdn.net/tag/details.html?tag=%e5%ad%98%e5%82%a8)[c](http://blog.csdn.net/tag/details.html?tag=c)

目录[(?)]( "系统根据文章中H1到H6标签自动生成文章目录")[[+]]( "展开")

1. [一致性 hash 算法 consistent hashing]()

1. [基本场景]()
1. [hash 算法和单调性]()
1. [consistent hashing 算法的原理]()

1. [环形hash 空间]()
1. [把对象映射到hash 空间]()
1. [把cache 映射到hash 空间]()
1. [把对象映射到cache]()
1. [考察cache 的变动]()

1. [虚拟节点]()
1. [小结]()
# []()一致性 hash 算法（ consistent hashing ）

张亮

consistent hashing 算法早在 1997 年就在论文 **[Consistent hashing and random trees](http://portal.acm.org/citation.cfm?id=258660)** 中被提出，目前在 cache 系统中应用越来越广泛；

## []()1 基本场景

比如你有 N 个 cache 服务器（后面简称 cache ），那么如何将一个对象 object 映射到 N 个 cache 上呢，你很可能会采用类似下面的通用方法计算 object 的 hash 值，然后均匀的映射到到 N 个 cache ；

hash(object)%N

一切都运行正常，再考虑如下的两种情况；

1  一个 cache 服务器 m down 掉了（在实际应用中必须要考虑这种情况），这样所有映射到 cache m 的对象都会失效，怎么办，需要把 cache m 从 cache 中移除，这时候 cache 是 N-1 台，映射公式变成了 hash(object)%(N-1) ；

2  由于访问加重，需要添加 cache ，这时候 cache 是 N+1 台，映射公式变成了 hash(object)%(N+1) ；

1 和 2 意味着什么？这意味着突然之间几乎所有的 cache 都失效了。对于服务器而言，这是一场灾难，洪水般的访问都会直接冲向后台服务器；

再来考虑第三个问题，由于硬件能力越来越强，你可能想让后面添加的节点多做点活，显然上面的 hash 算法也做不到。

  有什么方法可以改变这个状况呢，这就是 consistent hashing...

## []()2 hash 算法和单调性

Hash 算法的一个衡量指标是单调性（ Monotonicity ），定义如下：

单调性是指如果已经有一些内容通过哈希分派到了相应的缓冲中，又有新的缓冲加入到系统中。哈希的结果应能够保证原有已分配的内容可以被映射到新的缓冲中去，而不会被映射到旧的缓冲集合中的其他缓冲区。

容易看到，上面的简单 hash 算法 hash(object)%N 难以满足单调性要求。

## []()3 consistent hashing 算法的原理

consistent hashing 是一种 hash 算法，简单的说，在移除 / 添加一个 cache 时，它能够尽可能小的改变已存在 key 映射关系，尽可能的满足单调性的要求。

下面就来按照 5 个步骤简单讲讲 consistent hashing 算法的基本原理。

### []()3.1 环形hash 空间

考虑通常的 hash 算法都是将 value 映射到一个 32 为的 key 值，也即是 0~2^32-1 次方的数值空间；我们可以将这个空间想象成一个首（ 0 ）尾（ 2^32-1 ）相接的圆环，如下面图 1 所示的那样。

![circle space]()

图 1 环形 hash 空间

### []()3.2  把对象映射到hash 空间

接下来考虑 4 个对象 object1~object4 ，通过 hash 函数计算出的 hash 值 key 在环上的分布如图 2 所示。

hash(object1) = key1;

… …

hash(object4) = key4;

![object]()

图 2 4 个对象的 key 值分布

### []()3.3  把cache 映射到hash 空间

Consistent hashing 的基本思想就是将对象和 cache 都映射到同一个 hash 数值空间中，并且使用相同的 hash 算法。

假设当前有 A,B 和 C 共 3 台 cache ，那么其映射结果将如图 3 所示，他们在 hash 空间中，以对应的 hash 值排列。

hash(cache A) = key A;

… …

hash(cache C) = key C;

![cache]()

图 3 cache 和对象的 key 值分布

 

说到这里，顺便提一下 cache 的 hash 计算，一般的方法可以使用 cache 机器的 IP 地址或者机器名作为 hash 输入。

### []()3.4  把对象映射到cache

现在 cache 和对象都已经通过同一个 hash 算法映射到 hash 数值空间中了，接下来要考虑的就是如何将对象映射到 cache 上面了。

在这个环形空间中，如果沿着顺时针方向从对象的 key 值出发，直到遇见一个 cache ，那么就将该对象存储在这个 cache 上，因为对象和 cache 的 hash 值是固定的，因此这个 cache 必然是唯一和确定的。这样不就找到了对象和 cache 的映射方法了吗？！

依然继续上面的例子（参见图 3 ），那么根据上面的方法，对象 object1 将被存储到 cache A 上； object2 和 object3 对应到 cache C ； object4 对应到 cache B ；

### []()3.5  考察cache 的变动

前面讲过，通过  hash 然后求余的方法带来的最大问题就在于不能满足单调性，当 cache 有所变动时， cache 会失效，进而对后台服务器造成巨大的冲击，现在就来分析分析 consistent hashing 算法。

**3.5.1** **移除 cache**

考虑假设 cache B 挂掉了，根据上面讲到的映射方法，这时受影响的将仅是那些沿 cache B 逆时针遍历直到下一个 cache （ cache C ）之间的对象，也即是本来映射到 cache B 上的那些对象。

因此这里仅需要变动对象 object4 ，将其重新映射到 cache C 上即可；参见图 4 。

![remove]()

图 4 Cache B 被移除后的 cache 映射

**3.5.2** **添加 cache**

再考虑添加一台新的 cache D 的情况，假设在这个环形 hash 空间中， cache D 被映射在对象 object2 和 object3 之间。这时受影响的将仅是那些沿 cache D 逆时针遍历直到下一个 cache （ cache B ）之间的对象（它们是也本来映射到 cache C 上对象的一部分），将这些对象重新映射到 cache D 上即可。

 

因此这里仅需要变动对象 object2 ，将其重新映射到 cache D 上；参见图 5 。

![add]()

图 5  添加 cache D 后的映射关系

## []()4  虚拟节点

考量 Hash 算法的另一个指标是平衡性 (Balance) ，定义如下：

平衡性

平衡性是指哈希的结果能够尽可能分布到所有的缓冲中去，这样可以使得所有的缓冲空间都得到利用。

hash 算法并不是保证绝对的平衡，如果 cache 较少的话，对象并不能被均匀的映射到 cache 上，比如在上面的例子中，仅部署 cache A 和 cache C 的情况下，在 4 个对象中， cache A 仅存储了 object1 ，而 cache C 则存储了 object2 、 object3 和 object4 ；分布是很不均衡的。

为了解决这种情况， consistent hashing 引入了“虚拟节点”的概念，它可以如下定义：

“虚拟节点”（ virtual node ）是实际节点在 hash 空间的复制品（ replica ），一实际个节点对应了若干个“虚拟节点”，这个对应个数也成为“复制个数”，“虚拟节点”在 hash 空间中以 hash 值排列。

仍以仅部署 cache A 和 cache C 的情况为例，在图 4 中我们已经看到， cache 分布并不均匀。现在我们引入虚拟节点，并设置“复制个数”为 2 ，这就意味着一共会存在 4 个“虚拟节点”，  cache A1, cache A2 代表了 cache A ； cache C1, cache C2 代表了 cache C ；假设一种比较理想的情况，参见图 6 。

![virtual nodes]()

图 6  引入“虚拟节点”后的映射关系

 

此时，对象到“虚拟节点”的映射关系为：

objec1->cache A2 ； objec2->cache A1 ； objec3->cache C1 ； objec4->cache C2 ；

因此对象 object1 和 object2 都被映射到了 cache A 上，而 object3 和 object4 映射到了 cache C 上；平衡性有了很大提高。

引入“虚拟节点”后，映射关系就从 { 对象 -> 节点 } 转换到了 { 对象 -> 虚拟节点 } 。查询物体所在 cache 时的映射关系如图 7 所示。

![map]()

图 7  查询对象所在 cache

 

“虚拟节点”的 hash 计算可以采用对应节点的 IP 地址加数字后缀的方式。例如假设 cache A 的 IP 地址为 202.168.14.241 。

引入“虚拟节点”前，计算 cache A 的 hash 值：

Hash(“202.168.14.241”);

引入“虚拟节点”后，计算“虚拟节”点 cache A1 和 cache A2 的 hash 值：

Hash(“202.168.14.241#1”);   // cache A1

Hash(“202.168.14.241#2”);   // cache A2

## []()5  小结

Consistent hashing 的基本原理就是这些，具体的分布性等理论分析应该是很复杂的，不过一般也用不到。

[http://weblogs.java.net/blog/2007/11/27/consistent-hashing](http://weblogs.java.net/blog/2007/11/27/consistent-hashing)  上面有一个 java 版本的例子，可以参考。

[http://blog.csdn.net/mayongzhan/archive/2009/06/25/4298834.aspx](http://blog.csdn.net/mayongzhan/archive/2009/06/25/4298834.aspx)  转载了一个 PHP 版的实现代码。

[http://www.codeproject.com/KB/recipes/lib-conhash.aspx](http://www.codeproject.com/KB/recipes/lib-conhash.aspx) C语言版本

 

一些参考资料地址：

[http://portal.acm.org/citation.cfm?id=258660](http://portal.acm.org/citation.cfm?id=258660)

[http://en.wikipedia.org/wiki/Consistent_hashing](http://en.wikipedia.org/wiki/Consistent_hashing)

[http://www.spiteful.com/2008/03/17/programmers-toolbox-part-3-consistent-hashing/](http://www.spiteful.com/2008/03/17/programmers-toolbox-part-3-consistent-hashing/)

 [http://weblogs.java.net/blog/2007/11/27/consistent-hashing](http://weblogs.java.net/blog/2007/11/27/consistent-hashing)

[http://tech.idv2.com/2008/07/24/memcached-004/](http://tech.idv2.com/2008/07/24/memcached-004/)

[http://blog.csdn.net/mayongzhan/archive/2009/06/25/4298834.aspx](http://blog.csdn.net/mayongzhan/archive/2009/06/25/4298834.aspx)

 

 

分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")
1. 上一篇：[Map Reduce – the Free Lunch is not over?](http://blog.csdn.net/sparkliang/article/details/5272296)
1. 下一篇：[Hadoop分布式文件系统：架构和设计要点](http://blog.csdn.net/sparkliang/article/details/5279424)
查看评论[]()

70楼 [Spark-zh](http://blog.csdn.net/u011335846) 2013-07-09 20:27发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/u011335846)感谢楼主的讲解，很明了。69楼 [thomastianqq](http://blog.csdn.net/thomastianqq) 2013-07-09 17:26发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/thomastianqq)单调性是指如果已经有一些内容通过哈希分派到了相应的缓冲中，又有新的缓冲加入到系统中。哈希的结果应能够保证原有已分配的内容可以被映射到新的缓冲中去，而不会被映射到旧的缓冲集合中的其他缓冲区。
Hi,LZ，文中关于单调性的阐述是错误的。正确的应该是：哈希的结果应能够保证原有已分配的内容可以被映射到原有的或者新的缓冲中去，而不会被映射到旧的缓冲集合中的其他缓冲区。68楼 [lgfeng218](http://blog.csdn.net/lgfeng218) 2013-06-26 11:59发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/lgfeng218)通俗易懂67楼 [midstr](http://blog.csdn.net/midstr) 2013-06-18 14:54发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/midstr)通俗易懂，顶一个哈66楼 [zxh87](http://blog.csdn.net/zxh87) 2013-06-03 15:18发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/zxh87)很好，又看了一次！65楼 [fanjiabing](http://blog.csdn.net/fanjiabing) 2013-05-22 16:00发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/fanjiabing)写的很好!64楼 [mydreamongo](http://blog.csdn.net/mydreamongo) 2013-05-20 16:27发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/mydreamongo)讲的很好。。63楼 [bingmo01](http://blog.csdn.net/bingmo01) 2013-05-17 00:31发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/bingmo01)很给力。基本上看懂62楼 [perfectshark](http://blog.csdn.net/perfectshark) 2013-05-11 13:26发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/perfectshark)浅显易懂！61楼 [pymqqq](http://blog.csdn.net/pymqqq) 2013-05-04 20:18发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/pymqqq)非常好，谢谢楼主！学习了！60楼 [草原面朝大海](http://blog.csdn.net/zhaowenchaofang) 2013-04-30 22:59发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/zhaowenchaofang)假设我们已经在cache A中存储了一个object 1，然后不巧的是cacheA在之后被删除了，这时候我们需要访问object1，难道再一次进行hash，将object1存到下一个cache中吗？
如果上述成立，问题是我们如何将object1从cacheA中读取出来，因为在实际情况中，很有可能是cacheA是一下子坏掉了。Re: [总版主](http://blog.csdn.net/xuhb95083023) 2013-05-07 17:08发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/xuhb95083023)回复zhaowenchaofang：我觉得可以把OBJ存三份，分别再顺时针往后三个cache中，这样就算cache瞬间崩溃了，那么重新复制一份备份保证有三个备份即可，加CACHE也不是问题，剪切一部分obj到新增的cache上即可。59楼 [sc274491910](http://blog.csdn.net/sc274491910) 2013-04-19 18:07发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/sc274491910)加入了虚拟节点后，出现加减节点的情况，这种情况怎么权衡呢？58楼 [r4t5y6u7](http://blog.csdn.net/r4t5y6u7) 2013-04-07 13:46发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/r4t5y6u7)讲的太好了，清晰明了，浅显易懂57楼 [轻舞飘扬](http://blog.csdn.net/u010096845) 2013-03-30 17:09发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/u010096845)非常好56楼 [scncpb](http://blog.csdn.net/scncpb) 2013-03-27 15:13发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/scncpb)作者写的和老外写的一样通俗易懂55楼 [mail_f5](http://blog.csdn.net/mail_f5) 2013-02-21 17:55发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/mail_f5)图文并茂，不错54楼 [zhouxiaoqingelse](http://blog.csdn.net/zhouxiaoqingelse) 2013-02-20 18:18发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/zhouxiaoqingelse)非常清晰，感谢。53楼 [sight_](http://blog.csdn.net/sight_) 2013-01-22 11:08发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/sight_)楼主，有个问题请教一下,这个算法是如何保证后面添加的服务器能承担更多的任务，我应该如何控制"更多"这个量。例如我的服务器是分等级的，如何根据这个等级来控制hash到它上面的任务。Re: [sparkliang](http://blog.csdn.net/sparkliang) 2013-01-22 13:57发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/sparkliang)回复sight_：首先新加入的节点，将会把后继节点的部分数据分流给自己；
后面的新数据将会根据hash结果，无区别的分配到各节点上；
从我了解的来看，hash本身解决不了你说的分等级问题；hash环上的所有节点都是无差别对待的。不过对于引入虚拟节点的hash环来说，一个方法就是根据优先级来设置vnode的个数，这样也可以做到能力强的节点承担更多数据的结果，从统计意义上，实际上可能略有偏差。52楼 [林大虫](http://blog.csdn.net/lyzlovely) 2012-12-24 13:44发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/lyzlovely)讲得很清晰啊51楼 [dream1baby](http://blog.csdn.net/dream1baby) 2012-12-18 11:32发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/dream1baby)赞，学习了！50楼 [blueskyltt](http://blog.csdn.net/blueskyltt) 2012-12-03 18:30发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/blueskyltt)楼主讲的很浅显易懂，赞49楼 [syzcch](http://blog.csdn.net/syzcch) 2012-11-12 09:11发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/syzcch)讲的浅显易懂48楼 [jixianwu2009](http://blog.csdn.net/jixianwu2009) 2012-11-05 10:03发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/jixianwu2009)清晰明了47楼 [月亮床](http://blog.csdn.net/guoqingcun) 2012-10-23 18:27发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/guoqingcun)受益非浅46楼 [ju136](http://blog.csdn.net/ju136) 2012-10-16 09:32发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/ju136)good45楼 [yangqisheng](http://blog.csdn.net/yangqisheng) 2012-10-07 13:42发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/yangqisheng)楼主写的很好，一看就懂。我非常喜欢图文并茂的说明。谢谢楼主。44楼 [lcw918110](http://blog.csdn.net/lcw918110) 2012-09-05 10:14发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/lcw918110)3.5.1中，这时受影响的将仅是那些沿 cache B 逆时针遍历直到下一个 cache （ cache C ）之间的对象。其中的cache C如图所示应为 A。请楼主检查下。
谢谢楼主的博文。Re: [sparkliang](http://blog.csdn.net/sparkliang) 2012-09-05 15:42发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/sparkliang)回复lcw918110：赞，看的很仔细；逆时针是在A上，顺时针落在C上。Re: [lcw918110](http://blog.csdn.net/lcw918110) 2012-09-07 11:40发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/lcw918110)回复sparkliang：楼主，关于cache中使用的hash算法，楼主知道的还有哪些？学习学习。43楼 [renjunyi6666](http://blog.csdn.net/renjunyi6666) 2012-08-23 11:38发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/renjunyi6666)受益!谢谢42楼 [Myxiao7](http://blog.csdn.net/Myxiao7) 2012-08-17 21:05发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/Myxiao7)学习了41楼 [skywhsq1987](http://blog.csdn.net/skywhsq1987) 2012-08-02 10:35发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/skywhsq1987)ding40楼 [m_vptr](http://blog.csdn.net/m_vptr) 2012-08-02 09:25发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/m_vptr)好文章39楼 [boYwell](http://blog.csdn.net/boYwell) 2012-08-01 09:08发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/boYwell)文笔很好，赞一个38楼 [YcdoiT](http://blog.csdn.net/leejunjie2008) 2012-07-26 16:40发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/leejunjie2008)顶一个，我这种悟性的都能看懂了37楼 [大城小爱](http://blog.csdn.net/longsaiqin) 2012-07-06 19:02发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/longsaiqin)讲得很清晰，不错不错36楼 [brucest0078](http://blog.csdn.net/BruceXX) 2012-06-18 11:44发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/BruceXX)关于virtualNode 有一些问题，因为你的key在存储的时候可以这样，但是读取的时候只有原生的key去相关的内容，而不会把vNode的ip之类的带进去，所以这里只是理论呢还是已经有实践过？表示怀疑。Re: [sparkliang](http://blog.csdn.net/sparkliang) 2012-06-18 13:52发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/sparkliang)回复BruceXX：读取的时候，关vnode什么事呢。读取者事先是不知道key存在那个节点上的。35楼 [Honeybb](http://blog.csdn.net/Honeybb) 2012-06-12 09:44发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/Honeybb)最近在看这方面的文章，很迷茫。 楼主，分布式网络关于资源定位与一致性哈希算法和paxos算法的关系，请楼主明示啊！Re: [sparkliang](http://blog.csdn.net/sparkliang) 2012-06-18 13:52发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/sparkliang)回复Honeybb：一致性哈希算法和paxos算法，没什么关联34楼 [laigege](http://blog.csdn.net/laigege) 2012-06-02 09:48发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/laigege)楼主是好人33楼 [dengjm_2011](http://blog.csdn.net/dengjm_2011) 2012-05-23 16:41发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/dengjm_2011)很不错的文章，简单明了！32楼 [zcl198715](http://blog.csdn.net/zcl198715) 2012-04-29 21:34发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/zcl198715)分析的很好！赞一个！31楼 [leonkyd](http://blog.csdn.net/leonkyd) 2012-04-21 10:28发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/leonkyd)非常好的技术文章，赞一个30楼 [wumucheng](http://blog.csdn.net/wumucehng) 2012-04-06 20:04发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/wumucehng)讲得非常清晰，赞！29楼 [lqbbduck](http://blog.csdn.net/lqbbduck) 2012-03-30 00:51发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/lqbbduck)分析得很好28楼 [TCCaiWQ](http://blog.csdn.net/TCCaiWQ) 2012-03-15 16:54发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/TCCaiWQ)简洁明了27楼 [RunZhi1989](http://blog.csdn.net/RunZhi1989) 2012-03-05 17:22发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/RunZhi1989)赞，我看英文维基百科一直没看懂，博主的东西真是简洁明了。赞赞赞！26楼 [libobo5954451](http://blog.csdn.net/libobo5954451) 2012-02-10 18:02发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/libobo5954451)非常好25楼 [humingchun](http://blog.csdn.net/humingchun) 2012-01-02 19:44发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/humingchun)偶竟然看明白了 说明写的很清楚 呵呵24楼 [jmx_zhidai](http://blog.csdn.net/jmx_zhidai) 2011-11-24 16:31发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/jmx_zhidai)写的非常明白，赞一个！！！23楼 [yfk](http://blog.csdn.net/yfkiss) 2011-11-16 15:27发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/yfkiss)很赞的文章
谢谢lZ22楼 [南山道人](http://blog.csdn.net/kutpbpb_CSDN) 2011-11-16 12:47发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/kutpbpb_CSDN)顶，非常感谢！21楼 [zhaopeinow](http://blog.csdn.net/zhaopeinow) 2011-10-20 17:52发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/zhaopeinow)顶20楼 [lbqBraveheart](http://blog.csdn.net/lbqBraveheart) 2011-10-12 15:03发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/lbqBraveheart)顶19楼 [向良玉](http://blog.csdn.net/xiangliangyu2008) 2011-10-11 13:59发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/xiangliangyu2008)好文章18楼 [xgtogd](http://blog.csdn.net/xgtogd) 2011-09-25 23:26发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/xgtogd)很好17楼 [fairywell](http://blog.csdn.net/fairywell) 2011-09-12 21:54发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/fairywell)好文，图文并茂，让我一下就看明白了 ：）
Mark并收藏~~16楼 [parakpurple](http://blog.csdn.net/parakpurple) 2011-08-29 17:09发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/parakpurple)写的太棒了 深入浅出15楼 [bokee](http://blog.csdn.net/bokee) 2011-08-06 16:12发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/bokee)很清晰啊，想问下你的图是用什么工具画的？Re: [sparkliang](http://blog.csdn.net/sparkliang) 2011-08-30 09:19发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/sparkliang)回复bokee：visio啊14楼 [dahai_888](http://blog.csdn.net/dahai_888) 2011-07-16 17:12发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/dahai_888)不得了。13楼 [wayon_yang](http://blog.csdn.net/wayon_yang) 2011-05-30 11:49发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/wayon_yang)[e01]
英语不好 之前没看懂 现在清晰多了12楼 [laoyi19861011](http://blog.csdn.net/laoyi19861011) 2011-05-13 20:45发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/laoyi19861011)这么好的一篇文章居然没有推荐，csdn的编辑干嘛吃去了11楼 [chemila](http://blog.csdn.net/chemila) 2011-03-31 17:45发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/chemila)[e01]10楼 [liomao](http://blog.csdn.net/liomao) 2011-03-29 09:13发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/liomao)[e10]9楼 [xunujNext](http://blog.csdn.net/xunujNext) 2011-02-17 22:28发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/xunujNext)不得不[e01]8楼 [printfabcd](http://blog.csdn.net/printfabcd) 2010-12-17 14:51发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/printfabcd)请问虚拟节点，怎么保证均匀分布在，那个环上呢？？O(∩_∩)O~Re: [sparkliang](http://blog.csdn.net/sparkliang) 2011-03-11 15:31发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/sparkliang)回复 printfabcd：这就要由hash算法来保证了，均匀分布是概率上的均匀，当虚拟节点足够时，就能保证大概均匀了。7楼 [ccnuliu](http://blog.csdn.net/ccnuliu) 2010-06-23 00:01发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/ccnuliu)假如cache通过hash函数计算出的值 和 object通过hash函数计算出来的值是同一个hash值怎么办？
那object应该指向哪个cache?Re: [sparkliang](http://blog.csdn.net/sparkliang) 2010-06-24 09:41发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/sparkliang)回复 ccnuliu：如果碰巧相同，就是指向具有相同hash值的cache。选择的原则是cache.hash &gt;= object.hash；（当然在环的衔接处是例外）Re: [a43350860](http://blog.csdn.net/a43350860) 2011-03-05 18:27发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/a43350860)回复 sparkliang：假设我给hash环开启一扇大门，所有的值进来都必须经过这一扇门，那么我只要在这一扇大门里加一个hash关卡，我不关心你传进来的值是什么类型，是什么形态，我都是统一做hash运算，然后再分配到相应的节点上面去。6楼 [ccnuliu](http://blog.csdn.net/ccnuliu) 2010-06-23 00:01发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/ccnuliu)问你一个问题
加入cache通过hash函数计算出的值 和 object通过hash函数计算出来的值是同一个hash值怎么办？
那object应该指向哪个cache?5楼 [chengqianl](http://blog.csdn.net/chengqianl) 2010-06-02 16:53发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/chengqianl)收益了4楼 [specific_intel](http://blog.csdn.net/specific_intel) 2010-05-12 16:26发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/specific_intel)[e01] 非常清晰，谢谢！3楼 [yayaaishenghuo](http://blog.csdn.net/yayaaishenghuo) 2010-03-09 13:46发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/yayaaishenghuo)讲得太好了2楼 [匿名用户](http://blog.csdn.net/匿名用户) 2010-02-22 10:06发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/匿名用户)[e10]1楼 [匿名用户](http://blog.csdn.net/匿名用户) 2010-02-22 10:05发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/匿名用户)[e01]
您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fsparkliang%2Farticle%2Fdetails%2F5279393)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()
[![TOP]()]( "回到顶部")

个人资料

[![]( "访问我的空间")](http://my.csdn.net/sparkliang)
[sparkliang](http://my.csdn.net/sparkliang)

[]( "[加关注]") []( "[发私信]")
[![]()](http://medal.blog.csdn.net/allmedal.aspx)

* 访问：383734次
* 积分：3864分
* 排名：第1399名

* 原创：87篇
* 转载：15篇
* 译文：4篇
* 评论：395条

文章搜索

[]()

文章分类

* [C/C++语言](http://blog.csdn.net/sparkliang/article/category/488400)(25)
* [ICE相关](http://blog.csdn.net/sparkliang/article/category/806112)(2)
* [libevent分析](http://blog.csdn.net/sparkliang/article/category/660506)(14)
* [Linux](http://blog.csdn.net/sparkliang/article/category/635072)(7)
* [STL](http://blog.csdn.net/sparkliang/article/category/503089)(4)
* [Windows程序设计](http://blog.csdn.net/sparkliang/article/category/534835)(8)
* [分布式系统](http://blog.csdn.net/sparkliang/article/category/699896)(32)
* [算法艺术](http://blog.csdn.net/sparkliang/article/category/488397)(8)
* [网络程序设计](http://blog.csdn.net/sparkliang/article/category/614184)(31)
* [虚拟现实](http://blog.csdn.net/sparkliang/article/category/488399)(1)
* [软件架构](http://blog.csdn.net/sparkliang/article/category/641160)(11)
* [随笔](http://blog.csdn.net/sparkliang/article/category/488398)(8)
* [Leveldb](http://blog.csdn.net/sparkliang/article/category/1342001)(19)
文章存档

* [2013年07月](http://blog.csdn.net/sparkliang/article/month/2013/07)(1)
* [2013年06月](http://blog.csdn.net/sparkliang/article/month/2013/06)(2)
* [2013年05月](http://blog.csdn.net/sparkliang/article/month/2013/05)(1)
* [2013年04月](http://blog.csdn.net/sparkliang/article/month/2013/04)(5)
* [2013年03月](http://blog.csdn.net/sparkliang/article/month/2013/03)(7)
* [2013年02月](http://blog.csdn.net/sparkliang/article/month/2013/02)(5)
* [2012年11月](http://blog.csdn.net/sparkliang/article/month/2012/11)(1)
* [2012年10月](http://blog.csdn.net/sparkliang/article/month/2012/10)(3)
* [2012年05月](http://blog.csdn.net/sparkliang/article/month/2012/05)(1)
* [2012年02月](http://blog.csdn.net/sparkliang/article/month/2012/02)(1)
* [2011年12月](http://blog.csdn.net/sparkliang/article/month/2011/12)(2)
* [2011年11月](http://blog.csdn.net/sparkliang/article/month/2011/11)(1)
* [2011年07月](http://blog.csdn.net/sparkliang/article/month/2011/07)(1)
* [2011年06月](http://blog.csdn.net/sparkliang/article/month/2011/06)(1)
* [2011年04月](http://blog.csdn.net/sparkliang/article/month/2011/04)(2)
* [2010年10月](http://blog.csdn.net/sparkliang/article/month/2010/10)(1)
* [2010年09月](http://blog.csdn.net/sparkliang/article/month/2010/09)(1)
* [2010年07月](http://blog.csdn.net/sparkliang/article/month/2010/07)(2)
* [2010年06月](http://blog.csdn.net/sparkliang/article/month/2010/06)(7)
* [2010年05月](http://blog.csdn.net/sparkliang/article/month/2010/05)(1)
* [2010年04月](http://blog.csdn.net/sparkliang/article/month/2010/04)(5)
* [2010年03月](http://blog.csdn.net/sparkliang/article/month/2010/03)(6)
* [2010年02月](http://blog.csdn.net/sparkliang/article/month/2010/02)(6)
* [2010年01月](http://blog.csdn.net/sparkliang/article/month/2010/01)(8)
* [2009年12月](http://blog.csdn.net/sparkliang/article/month/2009/12)(14)
* [2009年11月](http://blog.csdn.net/sparkliang/article/month/2009/11)(3)
* [2009年08月](http://blog.csdn.net/sparkliang/article/month/2009/08)(1)
* [2009年07月](http://blog.csdn.net/sparkliang/article/month/2009/07)(1)
* [2009年06月](http://blog.csdn.net/sparkliang/article/month/2009/06)(2)
* [2009年04月](http://blog.csdn.net/sparkliang/article/month/2009/04)(4)
* [2009年03月](http://blog.csdn.net/sparkliang/article/month/2009/03)(2)
* [2009年01月](http://blog.csdn.net/sparkliang/article/month/2009/01)(1)
* [2008年12月](http://blog.csdn.net/sparkliang/article/month/2008/12)(6)
* [2008年11月](http://blog.csdn.net/sparkliang/article/month/2008/11)(1)

展开

阅读排行

* [一致性hash算法 - consistent hashing]( "一致性hash算法 - consistent hashing")(46507)
* [libevent源码深度剖析一](http://blog.csdn.net/sparkliang/article/details/4957667 "libevent源码深度剖析一")(38682)
* [Linux Epoll介绍和程序实例](http://blog.csdn.net/sparkliang/article/details/4770655 "Linux Epoll介绍和程序实例")(29675)
* [libevent源码深度剖析二](http://blog.csdn.net/sparkliang/article/details/4957744 "libevent源码深度剖析二")(24922)
* [libevent源码深度剖析三](http://blog.csdn.net/sparkliang/article/details/4957820 "libevent源码深度剖析三")(21004)
* [libevent源码深度剖析五](http://blog.csdn.net/sparkliang/article/details/4974876 "libevent源码深度剖析五")(14714)
* [libevent源码深度剖析PDF](http://blog.csdn.net/sparkliang/article/details/5202394 "libevent源码深度剖析PDF")(13425)
* [libevent源码深度剖析四](http://blog.csdn.net/sparkliang/article/details/4957885 "libevent源码深度剖析四")(12823)
* [libevent源码深度剖析六](http://blog.csdn.net/sparkliang/article/details/4985955 "libevent源码深度剖析六")(11796)
* [libevent源码深度剖析七](http://blog.csdn.net/sparkliang/article/details/4987751 "libevent源码深度剖析七")(10646)
评论排行

* [一致性hash算法 - consistent hashing]( "一致性hash算法 - consistent hashing")(80)
* [Linux Epoll介绍和程序实例](http://blog.csdn.net/sparkliang/article/details/4770655 "Linux Epoll介绍和程序实例")(67)
* [C/C++语言实现动态数组](http://blog.csdn.net/sparkliang/article/details/5359634 "C/C++语言实现动态数组")(21)
* [libevent源码深度剖析一](http://blog.csdn.net/sparkliang/article/details/4957667 "libevent源码深度剖析一")(20)
* [libevent源码深度剖析三](http://blog.csdn.net/sparkliang/article/details/4957820 "libevent源码深度剖析三")(15)
* [开源网络框架HPServer0.2.10版发布](http://blog.csdn.net/sparkliang/article/details/5350272 "开源网络框架HPServer0.2.10版发布")(14)
* [KMP算法真的很简单1](http://blog.csdn.net/sparkliang/article/details/5284579 "KMP算法真的很简单1")(12)
* [libevent源码深度剖析二](http://blog.csdn.net/sparkliang/article/details/4957744 "libevent源码深度剖析二")(12)
* [libevent源码深度剖析十三——libevent信号处理注意点](http://blog.csdn.net/sparkliang/article/details/5306809 "libevent源码深度剖析十三——libevent信号处理注意点")(11)
* [libevent源码深度剖析PDF](http://blog.csdn.net/sparkliang/article/details/5202394 "libevent源码深度剖析PDF")(10)

推荐文章
最新评论

* [Linux Epoll介绍和程序实例](http://blog.csdn.net/sparkliang/article/details/4770655#comments)

[xystar2012](http://blog.csdn.net/xystar2012): 代码有一个bug，在accpet后的sockfd，每次读或者写时，不能再调用epoll_ctl的EP...
* [Linux Epoll介绍和程序实例](http://blog.csdn.net/sparkliang/article/details/4770655#comments)

[zhkhhust](http://blog.csdn.net/zhkhhust): 楼主您好，我在ubuntu 下面测试时， 发现每次用 telnet 连接到 12345 端口，刚打了...
* [Linux Epoll介绍和程序实例](http://blog.csdn.net/sparkliang/article/details/4770655#comments)

[geniuslinchao](http://blog.csdn.net/geniuslinchao): 例子程序少加了stirng.h和stdlib.h两个头文件
* [一致性hash算法 - consistent hashing]()

[Spark-zh](http://blog.csdn.net/u011335846): 感谢楼主的讲解，很明了。
* [一致性hash算法 - consistent hashing]()

[thomastianqq](http://blog.csdn.net/thomastianqq): 单调性是指如果已经有一些内容通过哈希分派到了相应的缓冲中，又有新的缓冲加入到系统中。哈希的结果应能够...
* [一致性hash算法 - consistent hashing]()

[lgfeng218](http://blog.csdn.net/lgfeng218): 通俗易懂
* [一致性hash算法 - consistent hashing]()

[midstr](http://blog.csdn.net/midstr): 通俗易懂，顶一个哈
* [Epoll vs. IOCP](http://blog.csdn.net/sparkliang/article/details/4836536#comments)

[xly931](http://blog.csdn.net/xly931): @tiange0823:一般说的阻塞都是只IO操作本身是否阻塞（参见unix网络编程第六章）；epo...
* [Linux Epoll介绍和程序实例](http://blog.csdn.net/sparkliang/article/details/4770655#comments)

[qiuzhenguang](http://blog.csdn.net/qiuzhenguang): 相当好的一篇教程，学习了！多谢楼主。
* [libevent源码深度剖析PDF](http://blog.csdn.net/sparkliang/article/details/5202394#comments)

[superyongzhe](http://blog.csdn.net/superyongzhe): 谢谢分享！！！

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)[QQ客服](http://wpa.qq.com/msgrd?v=3&uin=2355263776&site=qq&menu=yes) [微博客服](http://e.weibo.com/csdnsupport/profile) [论坛反馈](http://bbs.csdn.net/forums/Service) [联系邮箱：webmaster@csdn.net](mailto:webmaster@csdn.net) 服务热线：400-600-2320京 ICP 证 070598 号北京创新乐知信息技术有限公司 版权所有世纪乐知(北京)网络技术有限公司 提供技术支持江苏乐知网络技术有限公司 提供商务支持Copyright © 1999-2012, CSDN.NET, All Rights Reserved [![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
![](http://counter.csdn.net/pv.aspx?id=24)
