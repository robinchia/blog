---
layout: post
title: "深入理解HashMap"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 数组&集合
--- 

# 深入理解HashMap

[您还未登录 !](http://www.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://www.javaeye.com/login) [注册](http://www.javaeye.com/signup)

[![JavaEye-最棒的软件开发交流社区]( "JavaEye-最棒的软件开发交流社区")](http://www.javaeye.com/)

[论坛首页](http://www.javaeye.com/forums) → [Java编程和Java企业应用版](http://www.javaeye.com/forums/board/Java) → [Java综合](http://www.javaeye.com/forums/tag/Java) →

# 深入理解HashMap

[全部](http://www.javaeye.com/forums/board/Java) [Hibernate](http://www.javaeye.com/forums/tag/Hibernate) [Spring](http://www.javaeye.com/forums/tag/Spring) [Struts](http://www.javaeye.com/forums/tag/Struts) [iBATIS](http://www.javaeye.com/forums/tag/iBATIS) [企业应用](http://www.javaeye.com/forums/tag/J2EE) [设计模式](http://www.javaeye.com/forums/tag/Design-Pattern) [DAO](http://www.javaeye.com/forums/tag/DAO) [领域模型](http://www.javaeye.com/forums/tag/Object-Domain) [OO](http://www.javaeye.com/forums/tag/OO) [Tomcat](http://www.javaeye.com/forums/tag/Tomcat) [SOA](http://www.javaeye.com/forums/tag/SOA) [JBoss](http://www.javaeye.com/forums/tag/JBoss) [Swing](http://www.javaeye.com/forums/tag/Swing) [Java综合](http://www.javaeye.com/forums/tag/Java)
[](http://www.javaeye.com/forums/39/topics/539465/posts/new "发表回复")

[myApps快速开发平台，配置即开发、所见即所得，节约85%工作量](http://www.javaeye.com/clicks/324)

« 上一页 1 [2](http://www.javaeye.com/topic/539465?page=2) [3](http://www.javaeye.com/topic/539465?page=3) … [8](http://www.javaeye.com/topic/539465?page=8) [9](http://www.javaeye.com/topic/539465?page=9) [下一页 »](http://www.javaeye.com/topic/539465?page=2)
浏览 17789 次
 [主题：深入理解HashMap](http://www.javaeye.com/topic/539465)

**该帖已经被评为精华帖** 作者 正文 * annegu
* 等级: ![三星会员]( "三星会员")
* [![annegu的博客]( "annegu的博客: annegu")](http://annegu.javaeye.com/)
* 文章: 38
* 积分: 360
* 来自: 杭州
* ![]()
    发表时间：2009-12-02   最后修改：2010-05-16

[<](http://www.javaeye.com/topic/539465#) [>](http://www.javaeye.com/topic/539465#) 猎头职位:  [安徽:  合肥,杭州,苏州：诚聘java架构师]()

相关文章: [ ](http://www.javaeye.com/topic/539465# "关闭")

* [一道面试题所牵涉到的JAVA知识(一)](http://www.javaeye.com/topic/662935 "一道面试题所牵涉到的JAVA知识(一)")
* [《算法导论》读书笔记7 (散列表)](http://www.javaeye.com/topic/570646 "《算法导论》读书笔记7 (散列表)")
* [ConcurrentHashMap之实现细节](http://www.javaeye.com/topic/344876 "ConcurrentHashMap之实现细节")
推荐圈子: [JAVA 3T](http://javagp.group.javaeye.com/)
[更多相关推荐](http://www.javaeye.com/wiki/topic/539465)
/**
    *@author annegu
    *@date 2009-12-02
    */
Hashmap是一种非常常用的、应用广泛的数据类型，最近研究到相关的内容，就正好复习一下。网上关于hashmap的文章很多，但到底是自己学习的总结，就发出来跟大家一起分享，一起讨论。
1、hashmap的数据结构
要知道hashmap是什么，首先要搞清楚它的数据结构，在java编程语言中，最基本的结构就是两种，一个是数组，另外一个是模拟指针（引用），所有的数据结构都可以用这两个基本结构来构造的，hashmap也不例外。Hashmap实际上是一个数组和链表的结合体（在数据结构中，一般称之为“链表散列“），请看下图（横排表示数组，纵排表示数组元素【实际上是一个链表】）。
![]()
从图中我们可以看到一个hashmap就是一个数组结构，当新建一个hashmap的时候，就会初始化一个数组。我们来看看java代码：
/** * The table, resized as necessary. Length MUST Always be a power of two. * FIXME 这里需要注意这句话，至于原因后面会讲到 */ transient Entry[] table;
static class Entry<K,V> implements Map.Entry<K,V> { final K key; V value; final int hash; Entry<K,V> next; .......... }
        上面的Entry就是数组中的元素，它持有一个指向下一个元素的引用，这就构成了链表。
         当我们往hashmap中put元素的时候，先根据key的hash值得到这个元素在数组中的位置（即下标），然后就可以把这个元素放到对应的位置中了。如果这个元素所在的位子上已经存放有其他元素了，那么在同一个位子上的元素将以链表的形式存放，新加入的放在链头，最先加入的放在链尾。从hashmap中get元素时，首先计算key的hashcode，找到数组中对应位置的某一元素，然后通过key的equals方法在对应位置的链表中找到需要的元素。从这里我们可以想象得到，如果每个位置上的链表只有一个元素，那么hashmap的get效率将是最高的，但是理想总是美好的，现实总是有困难需要我们去克服，哈哈~
2、hash算法
我们可以看到在hashmap中要找到某个元素，需要根据key的hash值来求得对应数组中的位置。如何计算这个位置就是hash算法。前面说过hashmap的数据结构是数组和链表的结合，所以我们当然希望这个hashmap里面的元素位置尽量的分布均匀些，尽量使得每个位置上的元素数量只有一个，那么当我们用hash算法求得这个位置的时候，马上就可以知道对应位置的元素就是我们要的，而不用再去遍历链表。
所以我们首先想到的就是把hashcode对数组长度取模运算，这样一来，元素的分布相对来说是比较均匀的。但是，“模”运算的消耗还是比较大的，能不能找一种更快速，消耗更小的方式那？java中时这样做的，
static int indexFor(int h, int length) { return h & (length-1); }
首先算得key得hashcode值，然后跟数组的长度-1做一次“与”运算（&）。看上去很简单，其实比较有玄机。比如数组的长度是2的4次方，那么hashcode就会和2的4次方-1做“与”运算。很多人都有这个疑问，为什么hashmap的数组初始化大小都是2的次方大小时，hashmap的效率最高，我以2的4次方举例，来解释一下为什么数组大小为2的幂时hashmap访问的性能最高。
         看下图，左边两组是数组长度为16（2的4次方），右边两组是数组长度为15。两组的hashcode均为8和9，但是很明显，当它们和1110“与”的时候，产生了相同的结果，也就是说它们会定位到数组中的同一个位置上去，这就产生了碰撞，8和9会被放到同一个链表上，那么查询的时候就需要遍历这个链表，得到8或者9，这样就降低了查询的效率。同时，我们也可以发现，当数组长度为15的时候，hashcode的值会与14（1110）进行“与”，那么最后一位永远是0，而0001，0011，0101，1001，1011，0111，1101这几个位置永远都不能存放元素了，空间浪费相当大，更糟的是这种情况中，数组可以使用的位置比数组长度小了很多，这意味着进一步增加了碰撞的几率，减慢了查询的效率！
![]()
          所以说，当数组长度为2的n次幂的时候，不同的key算得得index相同的几率较小，那么数据在数组上分布就比较均匀，也就是说碰撞的几率小，相对的，查询的时候就不用遍历某个位置上的链表，这样查询效率也就较高了。
          说到这里，我们再回头看一下hashmap中默认的数组大小是多少，查看源代码可以得知是16，为什么是16，而不是15，也不是20呢，看到上面annegu的解释之后我们就清楚了吧，显然是因为16是2的整数次幂的原因，在小数据量的情况下16比15和20更能减少key之间的碰撞，而加快查询的效率。
所以，在存储大容量数据的时候，最好预先指定hashmap的size为2的整数次幂次方。就算不指定的话，也会以大于且最接近指定值大小的2次幂来初始化的，代码如下(HashMap的构造方法中)：
// Find a power of 2 >= initialCapacity int capacity = 1; while (capacity < initialCapacity) capacity <<= 1;
3、hashmap的resize
       当hashmap中的元素越来越多的时候，碰撞的几率也就越来越高（因为数组的长度是固定的），所以为了提高查询的效率，就要对hashmap的数组进行扩容，数组扩容这个操作也会出现在ArrayList中，所以这是一个通用的操作，很多人对它的性能表示过怀疑，不过想想我们的“均摊”原理，就释然了，而在hashmap数组扩容之后，最消耗性能的点就出现了：原数组中的数据必须重新计算其在新数组中的位置，并放进去，这就是resize。
         那么hashmap什么时候进行扩容呢？当hashmap中的元素个数超过数组大小*loadFactor时，就会进行数组扩容，loadFactor的默认值为0.75，也就是说，默认情况下，数组大小为16，那么当hashmap中元素个数超过16*0.75=12的时候，就把数组的大小扩展为2*16=32，即扩大一倍，然后重新计算每个元素在数组中的位置，而这是一个非常消耗性能的操作，所以如果我们已经预知hashmap中元素的个数，那么预设元素的个数能够有效的提高hashmap的性能。比如说，我们有1000个元素new HashMap(1000), 但是理论上来讲new HashMap(1024)更合适，不过上面annegu已经说过，即使是1000，hashmap也自动会将其设置为1024。 但是new HashMap(1024)还不是更合适的，因为0.75*1000 < 1000, 也就是说为了让0.75 * size > 1000, 我们必须这样new HashMap(2048)才最合适，既考虑了&的问题，也避免了resize的问题。
4、key的hashcode与equals方法改写
在第一部分hashmap的数据结构中，annegu就写了get方法的过程：首先计算key的hashcode，找到数组中对应位置的某一元素，然后通过key的equals方法在对应位置的链表中找到需要的元素。所以，hashcode与equals方法对于找到对应元素是两个关键方法。
Hashmap的key可以是任何类型的对象，例如User这种对象，为了保证两个具有相同属性的user的hashcode相同，我们就需要改写hashcode方法，比方把hashcode值的计算与User对象的id关联起来，那么只要user对象拥有相同id，那么他们的hashcode也能保持一致了，这样就可以找到在hashmap数组中的位置了。如果这个位置上有多个元素，还需要用key的equals方法在对应位置的链表中找到需要的元素，所以只改写了hashcode方法是不够的，equals方法也是需要改写滴~当然啦，按正常思维逻辑，equals方法一般都会根据实际的业务内容来定义，例如根据user对象的id来判断两个user是否相等。
在改写equals方法的时候，需要满足以下三点：
(1) 自反性：就是说a.equals(a)必须为true。
(2) 对称性：就是说a.equals(b)=true的话，b.equals(a)也必须为true。
(3) 传递性：就是说a.equals(b)=true，并且b.equals(c)=true的话，a.equals(c)也必须为true。
通过改写key对象的equals和hashcode方法，我们可以将任意的业务对象作为map的key(前提是你确实有这样的需要)。
总结：
        本文主要描述了HashMap的结构，和hashmap中hash函数的实现，以及该实现的特性，同时描述了hashmap中resize带来性能消耗的根本原因，以及将普通的域模型对象作为key的基本要求。尤其是hash函数的实现，可以说是整个HashMap的精髓所在，只有真正理解了这个hash函数，才可以说对HashMap有了一定的理解。
这是hashmap第一篇，主要讲了一下hashmap的数据结构和计算hash的算法。接下去annegu还会写第二篇，主要讲讲LinkedHashMap和LRUHashMap。先做个预告，呵呵~
* [![]( "点击查看原始大小图片")]()
* 大小: 78.6 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 62.9 KB

* [查看图片附件](http://www.javaeye.com/topic/539465#)

声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。
推荐链接

* [![adobe]( "adobe")](http://www.javaeye.com/clicks/373) [返回顶楼](http://www.javaeye.com/topic/539465#) [ ](http://annegu.javaeye.com/ "浏览作者的博客") [ ](http://annegu.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=annegu "发送站内短信") [ ](http://annegu.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=annegu "关注作者")  * luobo25
* 等级: 初级会员
* [![luobo25的博客]( "luobo25的博客: ")](http://luobo25.javaeye.com/)
* 文章: 2
* 积分: 0
* 来自: 北京
* ![]()
    发表时间：2009-12-03  

当length=2^n时，hashcode & (length-1) == hashcode % length，难怪这么巧![]() ，楼主的研究有价值 [返回顶楼](http://www.javaeye.com/topic/539465#) [ ](http://luobo25.javaeye.com/ "浏览作者的博客") [ ](http://luobo25.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=luobo25 "发送站内短信") [ ](http://luobo25.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=luobo25 "关注作者") [回帖地址](http://www.javaeye.com/topic/539465#1277844 "luobo25回帖:深入理解HashMap")

[0](http://www.javaeye.com/topic/539465# "好") [0](http://www.javaeye.com/topic/539465# "差") 请登录后投票  * mycybyb
* 等级: 初级会员
* [![mycybyb的博客]( "mycybyb的博客: mycybyb")](http://mycybyb.javaeye.com/)
* 文章: 126
* 积分: 30
* 来自: 沈阳
* ![]()
    发表时间：2009-12-03  

写的很好。
不过，以前学数据结构的时候都学过。 [返回顶楼](http://www.javaeye.com/topic/539465#) [ ](http://mycybyb.javaeye.com/ "浏览作者的博客") [ ](http://mycybyb.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=mycybyb "发送站内短信") [ ](http://mycybyb.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=mycybyb "关注作者") [回帖地址](http://www.javaeye.com/topic/539465#1277918 "mycybyb回帖:深入理解HashMap")

[0](http://www.javaeye.com/topic/539465# "好") [0](http://www.javaeye.com/topic/539465# "差") 请登录后投票  * 火星来客
* 等级: 初级会员
* [![火星来客的博客]( "火星来客的博客: ")](http://azhang-tudou-com.javaeye.com/)
* 文章: 26
* 积分: 30
* 来自: 火星
* ![]()
    发表时间：2009-12-03  

mycybyb 写道

写的很好。
不过，以前学数据结构的时候都学过。
楼上应该没有看楼主的这篇文章，数据结构中应该不会有JDK中map的hash函数的实现说明，而楼主详细的分析了jdk中的hashmap中hash函数真正的原理和价值所在，网上HashMap的文章一堆一堆的，但是没有人能说清楚h&length-1的奥妙所在 [返回顶楼](http://www.javaeye.com/topic/539465#) [ ](http://azhang-tudou-com.javaeye.com/ "浏览作者的博客") [ ](http://azhang-tudou-com.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E7%81%AB%E6%98%9F%E6%9D%A5%E5%AE%A2 "发送站内短信") [ ](http://azhang-tudou-com.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E7%81%AB%E6%98%9F%E6%9D%A5%E5%AE%A2 "关注作者") [回帖地址](http://www.javaeye.com/topic/539465#1277933 "火星来客回帖:深入理解HashMap")

[0](http://www.javaeye.com/topic/539465# "好") [0](http://www.javaeye.com/topic/539465# "差") 请登录后投票  * mycybyb
* 等级: 初级会员
* [![mycybyb的博客]( "mycybyb的博客: mycybyb")](http://mycybyb.javaeye.com/)
* 文章: 126
* 积分: 30
* 来自: 沈阳
* ![]()
    发表时间：2009-12-03   最后修改：2009-12-03

火星来客 写道

mycybyb 写道

写的很好。
不过，以前学数据结构的时候都学过。
楼上应该没有看楼主的这篇文章，数据结构中应该不会有JDK中map的hash函数的实现说明，而楼主详细的分析了jdk中的hashmap中hash函数真正的原理和价值所在，网上HashMap的文章一堆一堆的，但是没有人能说清楚h&length-1的奥妙所在
没看能说好吗？
我说写的很好，说的就是h & length -1这个。
其他的，都是常识了。 [返回顶楼](http://www.javaeye.com/topic/539465#) [ ](http://mycybyb.javaeye.com/ "浏览作者的博客") [ ](http://mycybyb.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=mycybyb "发送站内短信") [ ](http://mycybyb.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=mycybyb "关注作者") [回帖地址](http://www.javaeye.com/topic/539465#1277985 "mycybyb回帖:深入理解HashMap")

[0](http://www.javaeye.com/topic/539465# "好") [0](http://www.javaeye.com/topic/539465# "差") 请登录后投票  * 火星来客
* 等级: 初级会员
* [![火星来客的博客]( "火星来客的博客: ")](http://azhang-tudou-com.javaeye.com/)
* 文章: 26
* 积分: 30
* 来自: 火星
* ![]()
    发表时间：2009-12-03   最后修改：2009-12-03

mycybyb 写道

火星来客 写道

mycybyb 写道

写的很好。
不过，以前学数据结构的时候都学过。
楼上应该没有看楼主的这篇文章，数据结构中应该不会有JDK中map的hash函数的实现说明，而楼主详细的分析了jdk中的hashmap中hash函数真正的原理和价值所在，网上HashMap的文章一堆一堆的，但是没有人能说清楚h&length-1的奥妙所在
没看能说好吗？
我说写的很好，说的就是h & length -1这个。
其他的，都是常识了。
恩，同意，其他确实是常识，只是从我的面试结果来看，没有常识的程序员比较多。 [返回顶楼](http://www.javaeye.com/topic/539465#) [ ](http://azhang-tudou-com.javaeye.com/ "浏览作者的博客") [ ](http://azhang-tudou-com.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E7%81%AB%E6%98%9F%E6%9D%A5%E5%AE%A2 "发送站内短信") [ ](http://azhang-tudou-com.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E7%81%AB%E6%98%9F%E6%9D%A5%E5%AE%A2 "关注作者") [回帖地址](http://www.javaeye.com/topic/539465#1278000 "火星来客回帖:深入理解HashMap")

[0](http://www.javaeye.com/topic/539465# "好") [0](http://www.javaeye.com/topic/539465# "差") 请登录后投票  * dennis_zane
* 等级: ![四钻会员]( "四钻会员")
* [![dennis_zane的博客]( "dennis_zane的博客: 庄周梦蝶")](http://dennis-zane.javaeye.com/)
* 文章: 1350
* 积分: 1968
* 来自: 杭州
* ![]()
    发表时间：2009-12-03  

火星来客 写道

mycybyb 写道

写的很好。
不过，以前学数据结构的时候都学过。
楼上应该没有看楼主的这篇文章，数据结构中应该不会有JDK中map的hash函数的实现说明，而楼主详细的分析了jdk中的hashmap中hash函数真正的原理和价值所在，网上HashMap的文章一堆一堆的，但是没有人能说清楚h&length-1的奥妙所在
h&length-1，这个也算是常识吧，对数学或者位运算了解点就知道了。 [返回顶楼](http://www.javaeye.com/topic/539465#) [ ](http://dennis-zane.javaeye.com/ "浏览作者的博客") [ ](http://dennis-zane.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=dennis_zane "发送站内短信") [ ](http://dennis-zane.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=dennis_zane "关注作者") [回帖地址](http://www.javaeye.com/topic/539465#1278031 "dennis_zane回帖:深入理解HashMap")

[0](http://www.javaeye.com/topic/539465# "好") [2](http://www.javaeye.com/topic/539465# "差") 请登录后投票  * 火星来客
* 等级: 初级会员
* [![火星来客的博客]( "火星来客的博客: ")](http://azhang-tudou-com.javaeye.com/)
* 文章: 26
* 积分: 30
* 来自: 火星
* ![]()
    发表时间：2009-12-03   最后修改：2009-12-03

dennis_zane 写道

h&length-1，这个也算是常识吧，对数学或者位运算了解点就知道了。
&运算本身当然是常识，任何一本计算机图书中几乎都会提到，但是将其和：
int capacity = 1; while (capacity < initialCapacity) capacity <<= 1;
组合起来使用，得到%的效果，但是性能比%高，这一点我没有想到，所以这一点上对我个人来说没有将其视为常识。 [返回顶楼](http://www.javaeye.com/topic/539465#) [ ](http://azhang-tudou-com.javaeye.com/ "浏览作者的博客") [ ](http://azhang-tudou-com.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E7%81%AB%E6%98%9F%E6%9D%A5%E5%AE%A2 "发送站内短信") [ ](http://azhang-tudou-com.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E7%81%AB%E6%98%9F%E6%9D%A5%E5%AE%A2 "关注作者") [回帖地址](http://www.javaeye.com/topic/539465#1278051 "火星来客回帖:深入理解HashMap")

[0](http://www.javaeye.com/topic/539465# "好") [0](http://www.javaeye.com/topic/539465# "差") 请登录后投票  * yuyee
* 等级: 初级会员
* [![yuyee的博客]( "yuyee的博客: 一往无前")](http://yuyee.javaeye.com/)
* 文章: 618
* 积分: 0
* 来自: 杭州
* ![]()
    发表时间：2009-12-03  

![]() 的确蛮仔细的 [返回顶楼](http://www.javaeye.com/topic/539465#) [ ](http://yuyee.javaeye.com/ "浏览作者的博客") [ ](http://yuyee.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=yuyee "发送站内短信") [ ](http://yuyee.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=yuyee "关注作者") [回帖地址](http://www.javaeye.com/topic/539465#1278116 "yuyee回帖:深入理解HashMap")

[0](http://www.javaeye.com/topic/539465# "好") [0](http://www.javaeye.com/topic/539465# "差") 请登录后投票  * lwyx2000
* 等级: 初级会员
* [![lwyx2000的博客]( "lwyx2000的博客: ")](http://lwyx2000.javaeye.com/)
* 文章: 5
* 积分: 30
* 来自: 福建省
* ![]()
    发表时间：2009-12-03  

很不错！！ [返回顶楼](http://www.javaeye.com/topic/539465#) [ ](http://lwyx2000.javaeye.com/ "浏览作者的博客") [ ](http://lwyx2000.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=lwyx2000 "发送站内短信") [ ](http://lwyx2000.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=lwyx2000 "关注作者") [回帖地址](http://www.javaeye.com/topic/539465#1278160 "lwyx2000回帖:深入理解HashMap")

[0](http://www.javaeye.com/topic/539465# "好") [0](http://www.javaeye.com/topic/539465# "差") 请登录后投票

[](http://www.javaeye.com/forums/39/topics/539465/posts/new "发表回复")

« 上一页 1 [2](http://www.javaeye.com/topic/539465?page=2) [3](http://www.javaeye.com/topic/539465?page=3) … [8](http://www.javaeye.com/topic/539465?page=8) [9](http://www.javaeye.com/topic/539465?page=9) [下一页 »](http://www.javaeye.com/topic/539465?page=2)
[论坛首页](http://www.javaeye.com/forums) → [Java编程和Java企业应用版](http://www.javaeye.com/forums/board/Java) → [Java综合](http://www.javaeye.com/forums/tag/Java)
跳转论坛:Java编程和Java企业应用 Web前端技术 移动编程和手机应用开发 C/C++编程 Ruby编程 Python编程 PHP编程 Flash编程和RIA Microsoft .Net 综合技术 软件开发和项目管理 行业应用 入门讨论 招聘求职 海阔天空

* [北京: 去哪儿旅游搜索引擎诚聘java开发工程师](http://www.javaeye.com/jobs/523 "北京:去哪儿旅游搜索引擎诚聘java开发工程师")
* [北京: 掌中魔力诚聘java资深工程师](http://www.javaeye.com/jobs/769 "北京:掌中魔力诚聘java资深工程师")
* [北京: glodon诚聘Eclipse插件开发工程师](http://www.javaeye.com/jobs/839 "北京:glodon诚聘Eclipse插件开发工程师")
* [北京: chinacache诚聘高级Java开发工程师（后台应用](http://www.javaeye.com/jobs/726 "北京:chinacache诚聘高级Java开发工程师（后台应用）")
* [北京: chinacache诚聘Java应用架构师](http://www.javaeye.com/jobs/728 "北京:chinacache诚聘Java应用架构师")
* [浙江: VMKID诚聘Java开发工程师(J2SE)（Client端方](http://www.javaeye.com/jobs/779 "浙江:VMKID诚聘Java开发工程师(J2SE)（Client端方向）")
* [上海: 恺英网络诚聘JS工程师](http://www.javaeye.com/jobs/682 "上海:恺英网络诚聘JS工程师")
* [上海: 上海汉得诚聘高级基础架构研发工程师——IDE](http://www.javaeye.com/jobs/825 "上海:上海汉得诚聘高级基础架构研发工程师——IDE方向")
* [北京: Laszlo Systems 诚聘高级Java程序员](http://www.javaeye.com/jobs/817 "北京:Laszlo Systems 诚聘高级Java程序员")
* [福建: 福州几维网络诚聘游戏服务端程序员(Java)](http://www.javaeye.com/jobs/804 "福建:福州几维网络诚聘游戏服务端程序员(Java)")
* [北京: JavaEye猎头诚聘Java搜索工程师](http://www.javaeye.com/jobs/712 "北京:JavaEye猎头诚聘Java搜索工程师")
* [北京: 网易（NETEASE）诚聘高级Java开发工程师](http://www.javaeye.com/jobs/800 "北京:网易（NETEASE）诚聘高级Java开发工程师")
* [北京: 上海幽幽诚聘手机网游服务器端开发工程师](http://www.javaeye.com/jobs/806 "北京:上海幽幽诚聘手机网游服务器端开发工程师")
* [上海: BShare诚聘资深Java Web开发工程师](http://www.javaeye.com/jobs/838 "上海:BShare诚聘资深Java Web开发工程师")
* [广东: 穆迪moody's诚聘Java Developer](http://www.javaeye.com/jobs/794 "广东:穆迪moody's诚聘Java Developer")

* [首页](http://www.javaeye.com/)
* [新闻](http://www.javaeye.com/news)
* [论坛](http://www.javaeye.com/forums)
* [问答](http://www.javaeye.com/ask)
* [专栏](http://www.javaeye.com/wiki)
* [博客](http://www.javaeye.com/blogs)
* [圈子](http://www.javaeye.com/groups)
* [招聘](http://www.javaeye.com/job)
* [服务](http://www.javaeye.com/index/service)
* [搜索](http://www.javaeye.com/search)
* [Java](http://java.javaeye.com/)
* [Web](http://webapp.javaeye.com/)
* [Ruby](http://ruby.javaeye.com/)
* [Python](http://python.javaeye.com/)
* [敏捷](http://agile.javaeye.com/)
* [MySQL](http://mysql.javaeye.com/)
* [润亁报表](http://runqian.javaeye.com/)
* [图书](http://book.javaeye.com/)
* [广告服务](http://www.javaeye.com/index/service)
* [JavaEye黑板报](http://webmaster.javaeye.com/)
* [关于我们](http://www.javaeye.com/index/aboutus)
* [联系我们](http://www.javaeye.com/index/contactus)
* [友情链接](http://www.javaeye.com/index/friend_links)

© 2003-2010 JavaEye.com. 上海炯耐计算机软件有限公司版权所有 [ [沪ICP备05023328号](http://www.miibeian.gov.cn/) ] ![](http://stat.javaeye.com/?url=file:///D:/Documents/My Knowledge/Temp/5def100f-8e7c-4831-a431-99fb717e95db_128.htm&referrer=&logged_in=no)
