---
layout: post
title: "为什么要使用集合框架？ - - ITeye技术网站"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# 为什么要使用集合框架？ - - ITeye技术网站

[首页](http://www.iteye.com/) [新闻](http://www.iteye.com/news) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [招聘](http://www.iteye.com/job) [更多 ▼](http://java-mzd.iteye.com/blog/1002387#)

[专栏](http://www.iteye.com/wiki)  [群组](http://www.iteye.com/groups) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://java-mzd.iteye.com/login "登录") [我的应用](http://www.iteye.com/all) [登录](http://java-mzd.iteye.com/login) [注册](http://java-mzd.iteye.com/signup)

# [java-mzd](http://java-mzd.iteye.com/)

永久域名 [http://java-mzd.iteye.com](http://java-mzd.iteye.com/)

[13顶](http://java-mzd.iteye.com/blog/1002387#)
[5踩](http://java-mzd.iteye.com/blog/1002387#)

[淘宝武汉*面试归来](http://java-mzd.iteye.com/blog/1004784 "淘宝武汉*面试归来") | [湖南大学生软件联盟官网建设](http://java-mzd.iteye.com/blog/945822 "湖南大学生软件联盟官网建设")

2011-04-14

### [为什么要使用集合框架？]()

**文章分类:[Java编程](http://www.iteye.com/blogs/category/java)**
  题序：**很多时候，我们专心研究一个东西的时候，往往忘记了我们最初的目的是什么。**

        曾经研究过那么久的Java集合框架，为了搞清里面的细节，甚至都跑去重新买了一本数据结构，终于知道了线性表，知道了树，知道了查找表。也自己动手实现了ArrayList，LinkedList，HashMap等。

        今天在公交车上，突然想到“**我们为什么要使用****Java****集合框架呢？**”竟然一时语塞，半天想不起来，也说不出个所以然呢。顿时悲从中来啊。还是决定再次好好复习一把，现总结如下：

**    PS.还是那句老话，如果这些问题您都能轻松解答，没必要再浪费时间看下去了。**

* **Question one：我们为什么要使用集合框架？**
* **Question two:关于ArrayList 和Vector  （HashMap和HashTable）PS**
* **Question three：总体把握，集合框架的继承图**
* **Question four：set 于map的关系（见李兴华书）**
* **Question five：关于set（底层实现，关于TreeSet，关于排序，关于比较对象相等）**
* **Question six： 已经存在了那么多的动态结构，为什么需要hash表？**
* **Question seven：TreeMap 底层实现**

 

Question One：我们为什么使用集合框架？

  大家还记得我们为什么要使用数组嘛？

  当我们需要保持一组一样（类型相同）的元素的时候，我们应该使用一个容器来保存，数组就是这样一个容器。

  那么，数组的缺点是什么呢？

  数组一旦定义，长度将不能再变化。

  然而在我们的开发实践中，经常需要保存一些变长的数据集合，于是，我们需要一些能够**动态增长长度的容器**来保存我们的数据。

  而我们需要对数据的保存的逻辑可能各种各样，于是就有了各种各样的数据结构。**我们将数据结构在****Java中实现，于是就有了我们的集合框架。**

 

Question Two: List和Vector，HashMap和 HashTable 的区别在哪里呢？

前者都是在JDK1.2后推出的，在前者中，因为采用异步处理方式，性能更高。

（其实就是删掉了后者中操作数据{add ,remove等}时的线程同步锁，这样，效率更高了，但是却不再是线程安全的了，要线程安全，必须用户在程序中使用时自己控制。）

 

Question Three：集合框架继承图

本来想自己画个简图的，结果悲催的Rational Rose一直在抽风，也没时间去弄那个了。就网上Down了一个，暂且用着吧

                                       ![]()
 

从这个图中，我们可以发现：Collection 接口包括List和Set两个子接口（其实还有Queue和Sorted两个子接口）

另外，也验证了我们前面说的，Array和Collection都是用来保存数据的容器。

Question Four：Set 和 Map 有什么关系？

从Question Three的继承图中，我们很明显的发现：Map虽然不像Set那样属 于 Collection，但是Set和Map的继承图是如此的相似。那么，他们之间到底有什么样的 关系呢？

从逻辑上来说：首先，如果我们只考察Map中的key，那么显然，这个key的集合就是一 个set：不能重复，无序。（Map中有keyset()的方法，能直接得到key的set）另外一 个层面，如果我们 将Map中的key何value当做一个整体，那么，显然，这个时候 的Map其实就是一个Set（在Map的实现中，都是通过一个内部类来将key和value当做 一个整体entity），

从代码角度本身角度来看，无论是HashSet还是TreeSet其实使用的都是对应 的Tree来实现的，我们用一个默认的Object，这样就可以存了。

而事实上Set的底层实现中，我们也可以发现，都是用对应的Map来实现的，只是，在每次存key-value 对的时候，都默认给了一个Object 类作为Value的默认值，

  

Question five：关于set（底层实现，关于TreeSet，关于排序，关于比较对象相等）

我们知道，Set是无序的，同时又是不能重复的，那么，这种不能重复性，是如何保证的呢？ 其实这个问题问的还是不到位，Question Four已经说了，底层是由Map实现的，那么都是由Map来实现的。

**为了唯一性，重点是能识别相同的元素（如何判断相等）******

在Java中，=号，表示内存地址相等，即是指向堆内存的同一引用，那么此时，显然两个元素是相等的。

当不是指向堆内存的同一引用时。**我们就需要重写Object类的equals()****方法了**。在此，我们判断，如果我们需要比较的各个属性相等的话，那么则可认为这两个对象相等（关于属性相等的比较与此类似）

 

Question six： 已经存在了那么多的动态结构，为什么需要Hash表？

其实这个问题其实问的不恰当，应该是，有了诸如顺序表和有序表这样的静态表，诸如二叉排序树 平衡二叉树这样的动态表,为什么还需要Hash表呢?

因为记录在表中的**位置**和它的**关键字**之间**不存在**一个确定的关系。**查找的过程**为**给定值**依次和关键字集合中各个**关键字**进行**比较**。**查找的效率**取决于和给定值**进行比较**的关键字**个数**。

Question Seven: TreeMap 底层实现

TreeMap 底层由红黑树（Red-Black tree）实现。因为此部分内容包括算法分析，具体代码实现以及性能分析，内容比较多，所以本部分内容稍后专门用一个日志来推出。（形式类似HashMap那篇日志）

 

* [![]( "点击查看原始大小图片")]()
* 大小: 4.4 KB

* [查看图片附件](http://java-mzd.iteye.com/blog/1002387#)
[**13**
顶](http://java-mzd.iteye.com/blog/1002387#)[**5**
踩](http://java-mzd.iteye.com/blog/1002387#)

[淘宝武汉*面试归来](http://java-mzd.iteye.com/blog/1004784 "淘宝武汉*面试归来") | [湖南大学生软件联盟官网建设](http://java-mzd.iteye.com/blog/945822 "湖南大学生软件联盟官网建设")
* 05:05
* 浏览 (3292)
* [评论](http://java-mzd.iteye.com/blog/1002387#comments) (7)
* [相关推荐](http://www.iteye.com/wiki/topic/1002387)

### 评论

[]()

7 楼 [scholers](http://scholers.iteye.com/) 2011-05-09   [引用](http://java-mzd.iteye.com/blog/1002387#)

重写了equals,那么必须要重写hashcode
否则在set,map中会出错，
这个也是一个规定！
6 楼 [hastune](http://hastune.iteye.com/) 2011-04-22   [引用](http://java-mzd.iteye.com/blog/1002387#)

s929498110 写道

真的啊
打开JDK下面的src里面的源码看看、 一切就豁然开朗了
这些集合都有自己的实现机制、 了解了实现机制，他们的优缺点就一目了然了
没必要从别人对这些东西的看法理解中寻找属于自己的看法理解
源码看起来很容易的、 主要是其中有的算法需要琢磨琢磨
个人觉得需要琢磨的是怎么用好这些就行。算法的话，大可不必了。
源码有些地方也不太简单。原作者的一些思路还是可以琢磨一下吧。
如果需要自己写个容器的话，还是得想想算法吧。

5 楼 [s929498110](http://s929498110.iteye.com/) 2011-04-22   [引用](http://java-mzd.iteye.com/blog/1002387#)

真的啊
打开JDK下面的src里面的源码看看、 一切就豁然开朗了
这些集合都有自己的实现机制、 了解了实现机制，他们的优缺点就一目了然了
没必要从别人对这些东西的看法理解中寻找属于自己的看法理解
源码看起来很容易的、 主要是其中有的算法需要琢磨琢磨
4 楼 [yaoohfox](http://yaoohfox.iteye.com/) 2011-04-21   [引用](http://java-mzd.iteye.com/blog/1002387#)

写的不错。

3 楼 [Durian](http://duriun-yahoo-cn.iteye.com/) 2011-04-19   [引用](http://java-mzd.iteye.com/blog/1002387#)

看文章，lz不是计算机专业
2 楼 [java_mzd](http://java-mzd.iteye.com/) 2011-04-14   [引用](http://java-mzd.iteye.com/blog/1002387#)

悲剧了 写道

看来楼主没用好集合框架，看看源代码就知道了，及时它部提供，我们自己也要写，他提供了那就正好用他提供的，不用自己山寨写了
  呵呵，我水平不高，不会用。
也就明白明白道理，研究研究源码，自己写一写，检验自己懂不懂罢了。
高人面前，不敢称懂。

1 楼 [悲剧了](http://feisuwoniu.iteye.com/) 2011-04-14   [引用](http://java-mzd.iteye.com/blog/1002387#)

看来楼主没用好集合框架，看看源代码就知道了，及时它部提供，我们自己也要写，他提供了那就正好用他提供的，不用自己山寨写了
### 发表评论

### 表情图标

![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()

字体颜色: 标准深红红色橙色棕色黄色绿色橄榄青色蓝色深蓝靛蓝紫色灰色白色黑色 字体大小: 标准1 (xx-small)2 (x-small)3 (small)4 (medium)5 (large)6 (x-large)7 (xx-large) 对齐: 标准居左居中居右

提示：选择您需要装饰的文字, 按上列按钮即可添加上相应的标签

您还没有登录，请[登录](http://java-mzd.iteye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![java_mzd的博客]( "java_mzd的博客: ")](http://java-mzd.iteye.com/)

java_mzd

* 浏览: 70748 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 长沙
* ![]()
* [详细资料](http://java-mzd.iteye.com/blog/profile) [留言簿](http://java-mzd.iteye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://java-mzd.iteye.com/blog/user_visits)

[![zhxing的博客]( "zhxing的博客: ヾ孤星随缘ツ  http://t.sina.com.cn/samzhxing")](http://zhxing.iteye.com/)

[zhxing](http://zhxing.iteye.com/)

[![java_suddy的博客]( "java_suddy的博客: “平凡”的思想")](http://java-suddy.iteye.com/)

[java_suddy](http://java-suddy.iteye.com/)
[![liuxinglanyue的博客]( "liuxinglanyue的博客: liuxinglanyue")](http://liuxinglanyue.iteye.com/)

[liuxinglanyue](http://liuxinglanyue.iteye.com/)

[![libo_591的博客]( "libo_591的博客: 让更多的人站在巨人的肩膀上")](http://libo-591.iteye.com/)

[libo_591](http://libo-591.iteye.com/)

### 博客分类

* [全部博客 (43)](http://java-mzd.iteye.com/)
* [数据结构----------JAVA类集 (5)](http://java-mzd.iteye.com/category/133623)
* [TCP/IP (7)](http://java-mzd.iteye.com/category/153171)
### 我的留言簿 [>>更多留言](http://java-mzd.iteye.com/blog/guest_book)

* 楼主 你工作多久了。。。。
-- by [fanmingxing](http://java-mzd.iteye.com/blog/guest_book#39913)
* 写的不错，学习，谢谢！
-- by [chenge2k](http://java-mzd.iteye.com/blog/guest_book#39693)
* 看了LZ的文章，发现工作快两年的我，就像是一块浮起来的木头，真的很惭愧！也不怪我拿 ...
-- by [GoTiger](http://java-mzd.iteye.com/blog/guest_book#39618)

### 其他分类

* [我的收藏](http://java-mzd.iteye.com/blog/favorite) (23)
* [我的代码](http://java-mzd.iteye.com/blog/code_favorite) (0)
* [我的论坛主题帖](http://java-mzd.iteye.com/blog/topic) (3)
* [我的所有论坛帖](http://java-mzd.iteye.com/blog/post) (37)
* [我的精华良好帖](http://java-mzd.iteye.com/blog/article) (0)
### 最近加入群组

* [Android](http://android.group.iteye.com/)

### 存档

* [2011-05](http://java-mzd.iteye.com/blog/monthblog/2011-05) (2)
* [2011-04](http://java-mzd.iteye.com/blog/monthblog/2011-04) (7)
* [2011-03](http://java-mzd.iteye.com/blog/monthblog/2011-03) (2)
* [更多存档...](http://java-mzd.iteye.com/blog/monthblog_more)
### 评论排行榜

* [腾讯、淘宝、金山网络，实习生我该何去何从](http://java-mzd.iteye.com/blog/1050043 "腾讯、淘宝、金山网络，实习生我该何去何从")
* [淘宝、金山网络，百感交集](http://java-mzd.iteye.com/blog/1050926 "淘宝、金山网络，百感交集")
* [TCP/IP传输层，你懂多少？](http://java-mzd.iteye.com/blog/1007577 "TCP/IP传输层，你懂多少？")
* [淘宝武汉*面试归来](http://java-mzd.iteye.com/blog/1004784 "淘宝武汉*面试归来")
* [开源软件？自由软件？免费软件？你了解多少 ...](http://java-mzd.iteye.com/blog/862787 "开源软件？自由软件？免费软件？你了解多少？")

* [![Rss]()](http://java-mzd.iteye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://java-mzd.iteye.com/rss)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 ]
![]()
