---
layout: post
title: "加速你的Hibernate引擎（上）"
categories: 框架汇总
tags: 
 - 框架汇总
 - ORM
 - hibernate
--- 

# 加速你的Hibernate引擎（上）

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

# [简约设计の艺术](http://blog.csdn.net/DL88250)

## 一个自由程序员的琐碎

* [![]()目录视图](http://blog.csdn.net/DL88250?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/DL88250?viewmode=list)
* [![]()订阅](http://blog.csdn.net/DL88250/rss/list)
[公告：博客新增直接引用代码功能](https://code.csdn.net/blog/12)        [专访谭海燕：移动互联网开发的那些事](http://www.csdn.net/article/2013-07-24/2816320)      [CSDN博客频道自定义摘要、图片水印、热门标签等功能上线啦](http://blog.csdn.net/csdnproduct/article/details/9226265)      [CSDN博客第二期云计算最佳博主评选](http://blog.csdn.net/blogdevteam/article/details/9136613)      []()

### [加速你的Hibernate引擎（上）]()

分类： [ORM](http://blog.csdn.net/DL88250/article/category/742694) [Hibernate Framework](http://blog.csdn.net/DL88250/article/category/336870) [Java Persistence API](http://blog.csdn.net/DL88250/article/category/362100) [JavaEE](http://blog.csdn.net/DL88250/article/category/742693)  2010-11-03 09:55 5540人阅读 [评论]()(8) [收藏]( "收藏") [举报]( "举报")
目录[(?)]( "系统根据文章中H1到H6标签自动生成文章目录")[[+]]( "展开")

1. [引言]()
1. [Hibernate性能调优]()
1. [监控和剖析]()

1. [监控SQL生成]()
1. [查看Hibernate统计]()
1. [剖析]()

1. [调优技术]()

1. [业务规则与设计调优]()
1. [继承映射调优]()

1. [每个类层次一张表]()
1. [每个子类一张表]()
1. [每个具体类一张表]()
1. [使用隐式多态实现每个具体类一张表]()

1. [领域对象调优]()

1. [POJO调优]()
1. [POJO之间关联的调优]()

1. [连接池调优]()
1. [事务和并发的调优]()

![Hibernate Logo]()![InfoQ Logo]()

[Hibernate](http://www.hibernate.org/)是 最流行的对象关系映射（ORM）引擎之一，它提供了数据持久化和查询服务。在你的项目中引入Hibernate并让它跑起来是很容易的。但是，要让它跑得 好却是需要很多时间和经验的。通过我们的使用Hibernate 3.3.1和Oracle 9i的能源项目中的一些例子，本文涵盖了很多Hibernate调优技术。其中还提供了一些掌握Hibernate调优技术所必需的数据库知识。

我们假设读者对Hibernate有一个基本的了解。如果一个调优方法在Hibernate 参考文档（下文简称HRD）或其他调优文章中有详细描述，我们仅提供一个对该文档的引用并从不同角度对其做简单说明。我们关注于那些行之有效，但又缺乏文档的调优方法。

作者 **[Yongjun Jiao and Stewart Clark](http://www.infoq.com/cn/author/Yongjun-Jiao-and-Stewart-Clark)** 译者**[丁雪丰](http://www.infoq.com/cn/author/%E4%B8%81%E9%9B%AA%E4%B8%B0)** 发布于 2010年10月26日 上午12时0分

## []()1.引言

[Hibernate](http://www.hibernate.org/)是最流行的对象关系映射（ORM）引擎之一，它提供了数据持久化和查询服务。在你的项目中引入Hibernate并让它跑起来是很容易的。但是，要让它跑得好却是需要很多时间和经验的。通过我们的使用Hibernate 3.3.1和Oracle 9i的能源项目中的一些例子，本文涵盖了很多Hibernate调优技术。其中还提供了一些掌握Hibernate调优技术所必需的数据库知识。

我们假设读者对Hibernate有一个基本的了解。如果一个调优方法在Hibernate 参考文档（下文简称HRD）或其他调优文章中有详细描述，我们仅提供一个对该文档的引用并从不同角度对其做简单说明。我们关注于那些行之有效，但又缺乏文档的调优方法。

## []()2.Hibernate性能调优

调优是一个迭代的、持续进行的过程，涉及软件开发生命周期（SDLC）的所有阶段。在一个典型的使用Hibernate进行持久化的Java EE应用程序中，调优会涉及以下几个方面：

* 业务规则调优
* 设计调优
* Hibernate调优
* Java GC调优
* 应用程序容器调优
* 底层系统调优，包括数据库和OS。

没有一套精心设计的方案就去进行以上调优是非常耗时的，而且很可能收效甚微。好的调优方法的重要部分是为调优内容划分优先级。可以用Pareto定律（又称“80/20法则”）来解释这一点，即通常80%的应用程序性能改善源自头20%的性能问题[5]。

相比基于磁盘和网络的访问，基于内存和CPU的访问能提供更低的延迟和更高的吞吐量。这种基于IO的Hibernate调优与底层系统IO部分的调优应该优先于基于CPU和内存的底层系统GC、CPU和内存部分的调优。
**范例1**

我们调优了一个选择电流的HQL查询，把它从30秒降到了1秒以内。如果我们在垃圾回收方面下功夫，可能收效甚微——也许只有几毫秒或者最多几秒，相比HQL的改进，GC方面的改善可以忽略不计。

好的调优方法的另一个重要部分是决定何时优化[4]。

积极优化的提倡者主张开始时就进行调优，例如在业务规则和设计阶段，在整个SDLC都持续进行优化，因为他们认为后期改变业务规则和重新设计代价太大。

另一派人提倡在SDLC末期进行调优，因为他们抱怨前期调优经常会让设计和编码变得复杂。他们经常引用Donald Knuth的名言“*过早优化是万恶之源*”[6]。

为了平衡调优和编码需要一些权衡。根据笔者的经验，适当的前期调优能带来更明智的设计和细致的编码。很多项目就失败在应用程序调优上，因为上面提到的“过早优化”阶段在被引用时脱离了上下文，而且相应的调优不是被推迟得太晚就是投入资源过少。

但是，要做很多前期调优也不太可能，因为没有经过剖析，你并不能确定应用程序的瓶颈究竟在何处，应用程序一般都是这样演化的。

对我们的多线程企业级应用程序的剖析也表现出大多数应用程序平均只有20-50%的CPU使用率。剩余的CPU开销只是在等待数据库和网络相关的IO。

基于上述分析，我们得出这样一个结论，结合业务规则和设计的Hibernate调优在Pareto定律中20%的那个部分，相应的它们的优先级更高。

一种比较实际的做法是：

1. 识别出主要瓶颈，可以预见其中多数是Hibernate、业务规则和设计方面的（其数量视你的调优目标而定；但三到五个是不错的开端）。
1. 修改应用程序以便消除这些瓶颈。
1. 测试应用程序，然后重复步骤1，直到达到你的调优目标为止。

你能在Jack Shirazi的《Java Performance Tuning》 [7]一书中找到更多关于性能调优阶段的常见建议。

下面的章节中，我们会按照调优的大致顺序（列在前面的通常影响最大）去解释一些特定的调优技术。

## []()3. 监控和剖析

没有对Hibernate应用程序的有效监控和剖析，你无法得知性能瓶颈以及何处需要调优。

### []()3.1.1 监控SQL生成

尽管使用Hibernate的主要目的是将你从直接使用SQL的痛苦中解救出来，为了对应用程序进行调优，你必须知道Hibernate生成了哪些 SQL。JoeSplosky在他的《The Law of Leaky Abstractions》一文中详细描述了这个问题。

你可以在log4j中将**org.hibernate.SQL**包的日志级别设为DEBUG，这样便能看到生成的所有SQL。你还可以将其他包的日志级别设为DEBUG，甚至TRACE来定位一些性能问题。

### []()3.1.2 查看Hibernate统计

如果开启**hibernate.generate.statistics**，Hibernate会导出实体、集合、会话、二级缓存、查询和会话工厂的统计信息，这对通过**SessionFactory.getStatistics()**进行的调优很有帮助。为了简单起见，Hibernate还可以使用MBean“**org.hibernate.jmx.StatisticsService**”通过JMX来导出统计信息。你可以在这个网站找到配置范例 。

### []()3.1.3 剖析

一个好的剖析工具不仅有利于Hibernate调优，还能为应用程序的其他部分带来好处。然而，大多数商业工具（例如JProbe [10]）都很昂贵。幸运的是Sun/Oracle的JDK1.6自带了一个名为“Java VisualVM” [11]的调试接口。虽然比起那些商业竞争对手，它还相当基础，但它提供了很多调试和调优信息。

## []()4. 调优技术

### []()4.1 业务规则与设计调优

尽管业务规则和设计调优并不属于Hibernate调优的范畴，但此处的决定对后面Hibernate的调优有很大影响。因此我们特意指出一些与Hibernate调优有关的点。

在业务需求收集与调优过程中，你需要知道：

* 数据获取特性包括引用数据（reference data）、只读数据、读分组（read group）、读取大小、搜索条件以及数据分组和聚合。
* 数据修改特性包括数据变更、变更组、变更大小、无效修改补偿、数据库（所有变更都在一个数据库中或在多个数据库中）、变更频率和并发性，以及变更响应和吞吐量要求。
* 数据关系，例如关联（association）、泛化（generalization）、实现（realization）和依赖（dependency）。

基于业务需求，你会得到一个最优设计，其中决定了应用程序类型（是OLTP还是数据仓库，亦或者与其中某一种比较接近）和分层结构（将持久层和服务 层分离还是合并），创建领域对象（通常是POJO），决定数据聚合的地方（在数据库中进行聚合能利用强大的数据库功能，节省网络带宽；但是除了像 COUNT、SUM、AVG、MIN和MAX这样的标准聚合，其他的聚合通常不具有移植性。在应用服务器上进行聚合允许你应用更复杂的业务逻辑；但你需要 先在应用程序中载入详细的数据）。
**范例2**

分析员需要查看一个取自大数据表的电流ISO（Independent System Operator）聚合列表。最开始他们想要显示大多数字段，尽管数据库能在1分钟内做出响应，应用程序也要花30分钟将1百万行数据加载到前端UI。经 过重新分析，分析员保留了14个字段。因为去掉了很多可选的高聚合度字段，从剩下的字段中进行聚合分组返回的数据要少很多，而且大多数情况下的数据加载时 间也缩小到了可接受的范围内。

**范例3**

过24个“非标准”（shaped，表示每小时都可以有自己的电量和价格；如果所有24小时的电量和价格相同，我们称之为“标准”）小时会修改小时电流交易，其中包括2个属性：每小时电量和价格。起初我们使用Hibernate的*select-before-update*特性，就是更新24行数据需要24次选择。因为我们只需要2个属性，而且如果不修改电量或价格的话也没有业务规则禁止无效修改，我们就关闭了*select-before-update*特性，避免了24次选择。

### []()4.2继承映射调优

尽管继承映射是领域对象的一部分，出于它的重要性我们将它单独出来。HRD [1]中的[第9章“继承映射”](http://docs.jboss.org/hibernate/stable/core/reference/en/html/inheritance.html)已经说得很清楚了，所以我们将关注SQL生成和针对每个策略的调优建议。

以下是HRD中范例的类图：

![]()[]()

### []()4.2.1 每个类层次一张表

只需要一张表，一条多态查询生成的SQL大概是这样的：
select id, payment_type, amount, currency, rtn, credit_card_type **from** payment

针对具体子类（例如CashPayment）的查询生成的SQL是这样的：

select id, amount, currency **from** payment **where** payment_type=’CASH’

这样做的优点包括只有一张表、查询简单以及容易与其他表进行关联。第二个查询中不需要包含其他子类中的属性。所有这些特性让该策略的性能调优要比其他策略容易得多。这种方法通常比较适合数据仓库系统，因为所有数据都在一张表里，不需要做表连接。

主要的缺点整个类层次中的所有属性都挤在一张大表里，如果有很多子类特有的属性，数据库中就会有太多字段的取值为null，这为当前基于行的数据库 （使用基于列的DBMS的数据仓库处理这个会更好些）的SQL调优增加了难度。除非进行分区，否则唯一的数据表会成为热点，OLTP系统通常在这方面都不 太好。

### []()4.2.2每个子类一张表

需要4张表，多态查询生成的SQL如下：
select id, payment_type, amount, currency, rtn, credit_card type,
        case when c.payment_id **is** **not** **null** **then** 1
     when ck.payment_id **is** **not** **null** **then** 2
     when cc.payment_id **is** **not** **null** **then** 3
     when p.id **is** **not** **null** **then** 0 **end** **as** clazz
**from** payment p **left** **join** cash_payment c **on** p.id=c.payment_id **left join**
   cheque_payment ck **on** p.id=ck.payment_id **left** **join**
   credit_payment cc **on** p.id=cc.payment_id;

针对具体子类（例如CashPayment）的查询生成的SQL是这样的：

select id, payment_type, amount, currency
**from** payment p **left** **join** cash_payment c **on** p.id=c.payment_id;

优点包括数据表比较紧凑（没有不需要的可空字段），数据跨三个子类的表进行分区，容易使用超类的表与其他表进行关联。紧凑的数据表可以针对基于行的 数据库做存储块优化，让SQL执行得更好。数据分区增加了数据修改的并发性（除了超类，没有热点），OLTP系统通常会更好些。

同样的，第二个查询不需要包含其他子类的属性。

缺点是在所有策略中它使用的表和表连接最多，SQL语句稍显复杂（看看Hibernate动态鉴别器的长CASE子句）。相比单张表，数据库要花更多时间调优数据表连接，数据仓库在使用该策略时通常不太理想。

因为不能跨超类和子类的字段来建立复合索引，如果需要按这些列进行查询，性能会受影响。任何子类数据的修改都涉及两张表：超类的表和子类的表。

### []()4.2.3每个具体类一张表

涉及三张或更多的表，多态查询生成的SQL是这样的：
select p.id, p.amount, p.currency, p.rtn, p. credit_card_type, p.clazz
**from** (**select** id, amount, currency, **null** **as** rtn,**null** **as** credit_card type,
1 **as** clazz **from** cash_payment **union** **all**
**select** id, amount, **null** **as** currency, rtn,**null** **as** credit_card type,
2 **as** clazz **from** cheque_payment **union** **all**
**select** id, amount, **null** **as** currency, **null** **as** rtn,credit_card type,
3 **as** clazz **from** credit_payment) p;

针对具体子类（例如CashPayment）的查询生成的SQL是这样的：

select id, payment_type, amount, currency **from** cash_payment;

优点和上面的“每个子类一张表”策略相似。因为超类通常是抽象的，所以具体的三张表是必须的[开头处说的3张或更多的表是必须的]，任何子类的数据修改只涉及一张表，运行起来更快。

缺点是SQL（from子句和union all子查询）太复杂。但是大多数数据库对此类SQL的调优都很好。

如果一个类想和Payment超类关联，数据库无法使用引用完整性（referential integrity）来实现它；必须使用触发器来实现它。这对数据库性能有些影响。

### []()4.2.4使用隐式多态实现每个具体类一张表

只需要三张表。对于Payment的多态查询生成三条独立的SQL语句，每个对应一个子类。Hibernate引擎通过Java反射找出Payment的所有三个子类。

具体子类的查询只生成该子类的SQL。这些SQL语句都很简单，这里就不再阐述了。

它的优点和上节类似：紧凑数据表、跨三个具体子类的数据分区以及对子类任意数据的修改都只涉及一张表。

缺点是用三条独立的SQL语句代替了一条联合SQL，这会带来更多网络IO。Java反射也需要时间。假设如果你有一大堆领域对象，你从最上层的Object类进行隐式选择查询，那该需要多长时间啊！

根据你的映射策略制定合理的选择查询并非易事；这需要你仔细调优业务需求，基于特定的数据场景制定合理的设计决策。

以下是一些建议：

* 设计细粒度的类层次和粗粒度的数据表。细粒度的数据表意味着更多数据表连接，相应的查询也会更复杂。
* 如非必要，不要使用多态查询。正如上文所示，对具体类的查询只选择需要的数据，没有不必要的表连接和联合。
* “每个类层次一张表”对有高并发、简单查询并且没有共享列的OLTP系统来说是个不错的选择。如果你想用数据库的引用完整性来做关联，那它也是个合适的选择。
* “每个具体类一张表”对有高并发、复杂查询并且没有共享列的OLTP系统来说是个不错的选择。当然你不得不牺牲超类与其他类之间的关联。
* 采用混合策略，例如“每个类层次一张表”中嵌入“每个子类一张表”，这样可以利用不同策略的优势。随着你项目的进化，如果你要反复重新映射，那你可能也会采用该策略。
* “使用隐式多态实现每个具体类一张表”这种做法并不推荐，因为其配置过于繁缛、使用“any”元素的复杂关联语法和隐式查询的潜在危险性。
**范例4**

下面是一个交易描述应用程序的部分领域类图：

![]()

开始时，项目只有GasDeal和少数用户，它使用“每个类层次一张表”。

OilDeal和ElectricityDeal是后期产生更多业务需求后加入的。没有改变映射策略。但是ElectricityDeal有太多自己的属性，因此有很多电相关的可空字段加入了Deal表。因为用户量也在增长，数据修改变得越来越慢。

重新设计时我们使用了两张单独的表，分别针对气/油和电相关的属性。新的映射混合了“每个类层次一张表”和“每个子类一张表”。我们还重新设计了查询，以便允许针对具体交易子类进行选择，消除不必要的列和表连接。

### []()4.3 领域对象调优

基于**4.1****节**中对业务规则和设计的调优，你得到了一个用POJO来表示的领域对象的类图。我们建议：

### []()4.3.1 POJO调优

* 从读写数据中将类似引用这样的只读数据和以读为主的数据分离出来。
只读数据的二级缓存是最有效的，其次是以读为主的数据的非严格读写。将只读POJO标识为不可更改的（immutable）也是一个调优点。如果一个服务层方法只处理只读数据，可以将它的事务标为只读，这是优化Hibernate和底层JDBC驱动的一个方法。
* 细粒度的POJO和粗粒度的数据表。
基于数据的修改并发量和频率等内容来分解大的POJO。尽管你可以定义一个粒度非常细的对象模型，但粒度过细的表会导致大量表连接，这对数据仓库来说是不能接受的。
* 优先使用非final的类。
Hibernate只会针对非final的类使用CGLIB代理来实现延时关联获取。如果被关联的类是final的，Hibernate会一次加载所有内容，这对性能会有影响。
* 使用业务键为分离（detached）实例实现equals()和hashCode()方法。
在多层系统中，经常可以在分离对象上使用乐观锁来提升系统并发性，达到更高的性能。
* 定义一个版本或时间戳属性。
乐观锁需要这个字段来实现长对话（应用程序事务）[译注：session译为会话，conversion译为对话，以示区别]。
* 优先使用组合POJO。
你的前端UI经常需要来自多个不同POJO的数据。你应该向UI传递一个组合POJO而不是独立的POJO以获得更好的网络性能。
有两种方式在服务层构建组合POJO。一种是在开始时加3.2载所有需要的独立POJO，随后抽取需要的属性放入组合POJO；另一种是使用HQL投影，直接从数据库中选择需要的属性。
如果其他地方也要查找这些独立POJO，可以把它们放进二级缓存以便共享，这时第一种方式更好；其他情况下第二种方式更好。

### []()4.3.2 POJO之间关联的调优

* 如果可以用one-to-one、one-to-many或many-to-one的关联，就不要使用many-to-many。
* many-to-many关联需要额外的映射表。
尽管你的Java代码只需要处理两端的POJO，但查询时，数据库需要额外地关联映射表，修改时需要额外的删除和插入。
* 单向关联优先于双向关联。
由于many-to-many的特性，在双向关联的一端加载对象会触发另一端的加载，这会进一步触发原始端加载更多的数据，等等。
one-to-many和many-to-one的双向关联也是类似的，[当你从多端（子实体）定位到一端（父实体）]()。
这样的来回加载很耗时，而且可能也不是你所期望的。
* 不要为了关联而定义关联；只在你需要一起加载它们时才这么做，这应该由你的业务规则和设计来决定（见**范例****5**）。
另外，你要么不定义任何关联，要么在子POJO中定义一个值类型的属性来表示父POJO的ID（另一个方向也是类似的）。
* 集合调优
如果集合排序逻辑能由底层数据库实现，就使用“order-by”属性来代替“sort”，因为通常数据库在这方面做得比你好。
集合可以是值类型的（元素或组合元素），也可以是实体引用类型的（one-to-many或many-to-many关联）。对引用类型集合的调优主要是调优获取策略。对于值类型集合的调优，HRD [1]中的20.5节“理解集合性能”已经做了很好的阐述。
* 获取策略调优。请见**4.7****节的范例****5**。
**范例5**

我们有一个名为ElectricityDeals的核心POJO用于描述电的交易。从业务角度来看，它有很多many-to-one关联，例如和 Portfolio、Strategy和Trader等的关联。因为引用数据十分稳定，它们被缓存在前端，能基于其ID属性快速定位到它们。

为了有好的加载性能，ElectricityDeal只映射元数据，即那些引用POJO的值类型ID属性，因为在需要时，可以在前端通过portfolioKey从缓存中快速查找Portfolio：
name=*"portfolioKey"* column=*"PORTFOLIO_ID"*type=*"*integer*"*/>

这种隐式关联避免了数据库表连接和额外的字段选择，降低了数据传输的大小。

### []()4.4 连接池调优

由于创建物理数据库连接非常耗时，你应该始终使用连接池，而且应该始终使用生产级连接池而非Hibernate内置的基本连接池算法。

通常会为Hibernate提供一个有连接池功能的数据源。Apache DBCP的BasicDataSource[13]是一个流行的开源生产级数据源。大多数数据库厂商也实现了自己的兼容JDBC 3.0的连接池。举例来说，你也可以使用Oracle ReaApplication Cluster [15]提供的JDBC连接池[14]以获得连接的负载均衡和失败转移。

不用多说，你在网上能找到很多关于连接池调优的技术，因此我们只讨论那些大多数连接池所共有的通用调优参数：

* 最小池大小：连接池中可保持的最小连接数。
* 最大池大小：连接池中可以分配的最大连接数。
如果应用程序有高并发，而最大池大小又太小，连接池就会经常等待。相反，如果最小池大小太大，又会分配不需要的连接。
* 最大空闲时间：连接池中的连接被物理关闭前能保持空闲的最大时间。
* 最大等待时间：连接池等待连接返回的最大时间。该参数可以预防失控事务（runaway transaction）。
* 验证查询：在将连接返回给调用方前用于验证连接的SQL查询。这是因为一些数据库被配置为会杀掉长时间空闲的连接，网络或数据库相关的异常也可能会杀死连接。为了减少此类开销，连接池在空闲时会运行该验证。

### []()4.5事务和并发的调优

短数据库事务对任何高性能、高可扩展性的应用程序来说都是必不可少的。你使用表示对话请求的会话来处理单个工作单元，以此来处理事务。

考虑到工作单元的范围和事务边界的划分，有3中模式：

* **每次操作一个会话。**每次数据库调用需要一个新会话和事务。因为真实的业务事务通常包含多个此类操作和大量小事务，这一般会引起更多数据库活动（主要是数据库每次提交需要将变更刷新到磁盘上），影响应用程序性能。这是一种反模式，不该使用它。
* **使用分离对象，每次请求一个会话。**每次客户端请求有一个新会话和一个事务，使用Hibernate的“当前会话”特性将两者关联起来。
在一个多层系统中，用户通常会发起长对话（或应用程序事务）。大多数时间我们使用Hibernate的自动版本和分离对象来实现乐观并发控制和高性能。
* **带扩展（或长）会话的每次对话一会话。**在一个也许会跨多个事务的长对话中保持会话开启。尽管这能把你从重新关联中解脱出来，但会话可能会内存溢出，在高并发系统中可能会有旧数据。

你还应该注意以下几点。 

* 如果不需要JTA就用本地事务，因为JTA需要更多资源，比本地事务更慢。就算你有多个数据源，除非有跨多个数据库的事务，否则也不需要 JTA。在最后的一个场景下，可以考虑在每个数据源中使用本地事务，使用一种类似“Last Resource Commit Optimization”[16]的技术（见下面的**范例****6**）。
* 如果不涉及数据变更，将事务标记为只读的，就像**4.3.1****节**提到的那样。
* 总是设置默认事务超时。保证在没有响应返回给用户时，没有行为不当的事务会完全占有资源。这对本地事务也同样有效。
* 如果Hibernate不是独占数据库用户，乐观锁会失效，除非创建数据库触发器为其他应用程序对相同数据的变更增加版本字段值。
**范例6**

我们的应用程序有多个在大多数情况下只和数据库“A”打交道的服务层方法；它们偶尔也会从数据库“B”中获取只读数据。因为数据库“B”只提供只读数据，我们对这些方法在这两个数据库上仍然使用本地事务。

服务层上有一个方法设计在两个数据库上执行数据变更。以下是伪代码：
//Make sure a local transaction on database A exists
@Transactional (readOnly=**false**, propagation=Propagation.*REQUIRED*)
**public** **void** saveIsoBids() {
//it participates in the above annotated local transaction
insertBidsInDatabaseA();
//it runs in its own local transaction on database B
insertBidRequestsInDatabaseB(); //must be the last operation

因为**insertBidRequestsInDatabaseB()**是saveIsoBids ()中的最后一个方法，所以只有下面的场景会造成数据不一致：

在saveIsoBids()执行返回时，数据库“A”的本地事务提交失败。

但是，就算saveIsoBids()使用JTA，在两阶段提交（2PC）的第二个提交阶段失败的时候，你还是会碰到数据不一致。因此如果你能处理好上述的数据不一致性，而且不想为了一个或少数几个方法引入JTA的复杂性，你应该使用本地事务。

（未完待续）

**关于作者**

**Yongjun Jiao**是SunGard Consulting Services的技术主管。过去10年中他一直是专业软件开发者，他的专长包括Java SE、Java EE、Oracle和应用程序调优。他最近的关注点是高性能计算，包括内存数据网格、并行计算和网格计算。

**Stewart Clark**是SunGard Consulting Services的负责人。过去15年中他一直是专业软件开发者和项目经理，他的专长包括Java核心编程、Oracle和能源交易。

[译注：由于原文较长，中译版分两次发布]

**查看英文原文：**[Revving Up Your Hibernate Engine](http://www.infoq.com/articles/hibernate_tuning)

转自：http://www.infoq.com/cn/articles/hibernate_tuning
本文是使用 [B3log Solo](http://b3log-solo.googlecode.com/) 从 [简约设计の艺术](http://b3log-88250.appspot.com/) 进行同步发布的

原文地址：[http://b3log-88250.appspot.com/articles/2010/11/03/1288788936673.html](http://b3log-88250.appspot.com/articles/2010/11/03/1288788936673.html)
分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")

* 下一篇：[今天正式搬家过来CSDN了~~~~](http://blog.csdn.net/dl88250/article/details/1426527)
查看评论[]()

8楼 [pettery2](http://blog.csdn.net/pettery2) 2010-12-01 22:41发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/pettery2)[e01]7楼 [Duke147](http://blog.csdn.net/Duke147) 2010-11-09 19:05发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/Duke147)[e01]6楼 [tangweiwei0000](http://blog.csdn.net/tangweiwei0000) 2010-11-08 09:53发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/tangweiwei0000)[e01]5楼 [TAOCLEE](http://blog.csdn.net/TAOCLEE) 2010-11-07 14:29发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/TAOCLEE)路漫漫其修远喜4楼 [codeshuo](http://blog.csdn.net/codeshuo) 2010-11-06 22:17发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/codeshuo)[e01]
但我看不太懂啊。。。还得加把劲。3楼 [lupengji](http://blog.csdn.net/lupengji) 2010-11-06 18:28发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/lupengji)[e01]2楼 [liuwu513](http://blog.csdn.net/liuwu513) 2010-11-05 16:01发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/liuwu513)[e01]
这一部分太重要了1楼 [skytalemcc](http://blog.csdn.net/skytalemcc) 2010-11-05 09:47发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/skytalemcc)[e01]
您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fdl88250%2Farticle%2Fdetails%2F5985750)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()
[![TOP]()]( "回到顶部")

个人资料

[![]( "访问我的空间")](http://my.csdn.net/DL88250)
[88250](http://my.csdn.net/DL88250)

[]( "[加关注]") []( "[发私信]")

* 访问：4219337次
* 积分：41528分
* 排名：第20名

* 原创：1195篇
* 转载：326篇
* 译文：42篇
* 评论：2834条

文章搜索

[]()

文章分类

* [Adoration](http://blog.csdn.net/dl88250/article/category/359483)(7)
* [Agile Develeopment](http://blog.csdn.net/dl88250/article/category/361991)(25)
* [Alipay](http://blog.csdn.net/dl88250/article/category/817011)(2)
* [Android](http://blog.csdn.net/dl88250/article/category/785718)(2)
* [Apache](http://blog.csdn.net/dl88250/article/category/735896)(2)
* [Architecture Design](http://blog.csdn.net/dl88250/article/category/362353)(18)
* [B3log](http://blog.csdn.net/dl88250/article/category/727396)(31)
* [Beyond](http://blog.csdn.net/dl88250/article/category/772464)(2)
* [BeyondTrack](http://blog.csdn.net/dl88250/article/category/466930)(8)
* [Book](http://blog.csdn.net/dl88250/article/category/792935)(1)
* [BPEL](http://blog.csdn.net/dl88250/article/category/531019)(7)
* [Buzz](http://blog.csdn.net/dl88250/article/category/737397)(2)
* [C# &amp; .Net](http://blog.csdn.net/dl88250/article/category/273193)(36)
* [C/C++](http://blog.csdn.net/dl88250/article/category/259942)(60)
* [Cache](http://blog.csdn.net/dl88250/article/category/768467)(4)
* [CAS &amp; SAML &amp; SSO](http://blog.csdn.net/dl88250/article/category/445355)(7)
* [Chinasb](http://blog.csdn.net/dl88250/article/category/785583)(1)
* [Chrome](http://blog.csdn.net/dl88250/article/category/830731)(1)
* [Code Name:l0y0l](http://blog.csdn.net/dl88250/article/category/279627)(4)
* [Compile Principles](http://blog.csdn.net/dl88250/article/category/323229)(8)
* [Data-Structrue/Algorithms](http://blog.csdn.net/dl88250/article/category/259943)(41)
* [Database](http://blog.csdn.net/dl88250/article/category/307128)(31)
* [Design Patterns](http://blog.csdn.net/dl88250/article/category/318447)(22)
* [DHTML](http://blog.csdn.net/dl88250/article/category/688714)(27)
* [Eclipse](http://blog.csdn.net/dl88250/article/category/304165)(36)
* [EJB 3.x](http://blog.csdn.net/dl88250/article/category/472355)(24)
* [English](http://blog.csdn.net/dl88250/article/category/371604)(4)
* [EverBox](http://blog.csdn.net/dl88250/article/category/772184)(1)
* [Fiddlededee](http://blog.csdn.net/dl88250/article/category/259944)(87)
* [FireFox](http://blog.csdn.net/dl88250/article/category/798286)(2)
* [FreeMarker](http://blog.csdn.net/dl88250/article/category/823430)(1)
* [GAE](http://blog.csdn.net/dl88250/article/category/731730)(32)
* [Game](http://blog.csdn.net/dl88250/article/category/509270)(4)
* [GFW](http://blog.csdn.net/dl88250/article/category/794103)(1)
* [Git](http://blog.csdn.net/dl88250/article/category/744371)(5)
* [GlassFish](http://blog.csdn.net/dl88250/article/category/790487)(5)
* [Go](http://blog.csdn.net/dl88250/article/category/755357)(4)
* [Google](http://blog.csdn.net/dl88250/article/category/750293)(19)
* [Guice](http://blog.csdn.net/dl88250/article/category/801964)(1)
* [Hibernate Framework](http://blog.csdn.net/dl88250/article/category/336870)(30)
* [HTML5](http://blog.csdn.net/dl88250/article/category/763988)(2)
* [HttpClient](http://blog.csdn.net/dl88250/article/category/735897)(1)
* [IoC/DI](http://blog.csdn.net/dl88250/article/category/801962)(2)
* [J2EE/JavaEE](http://blog.csdn.net/dl88250/article/category/321666)(123)
* [J2SE/JavaSE](http://blog.csdn.net/dl88250/article/category/273192)(174)
* [Java](http://blog.csdn.net/dl88250/article/category/732513)(63)
* [Java Persistence API](http://blog.csdn.net/dl88250/article/category/362100)(25)
* [Java Server Faces](http://blog.csdn.net/dl88250/article/category/359121)(40)
* [JavaEE](http://blog.csdn.net/dl88250/article/category/742693)(14)
* [JavaEE Security](http://blog.csdn.net/dl88250/article/category/461374)(3)
* [JavaFX](http://blog.csdn.net/dl88250/article/category/363012)(25)
* [JavaScript](http://blog.csdn.net/dl88250/article/category/570523)(7)
* [JBoss Seam](http://blog.csdn.net/dl88250/article/category/450848)(26)
* [jBPM](http://blog.csdn.net/dl88250/article/category/531020)(8)
* [JDK 7](http://blog.csdn.net/dl88250/article/category/786045)(3)
* [Joke](http://blog.csdn.net/dl88250/article/category/735349)(3)
* [JPA](http://blog.csdn.net/dl88250/article/category/814017)(2)
* [jQuery](http://blog.csdn.net/dl88250/article/category/782163)(1)
* [jsdoc](http://blog.csdn.net/dl88250/article/category/758468)(1)
* [jsoup](http://blog.csdn.net/dl88250/article/category/757521)(3)
* [JSR-299](http://blog.csdn.net/dl88250/article/category/510376)(21)
* [JSR-330](http://blog.csdn.net/dl88250/article/category/801963)(2)
* [Life in Programming](http://blog.csdn.net/dl88250/article/category/273771)(153)
* [LivaPlayer](http://blog.csdn.net/dl88250/article/category/298242)(14)
* [Mathematics](http://blog.csdn.net/dl88250/article/category/365496)(9)
* [Maven](http://blog.csdn.net/dl88250/article/category/735535)(8)
* [Maven 2](http://blog.csdn.net/dl88250/article/category/461289)(18)
* [Memcached](http://blog.csdn.net/dl88250/article/category/768466)(1)
* [MultiMediia](http://blog.csdn.net/dl88250/article/category/295040)(17)
* [Music](http://blog.csdn.net/dl88250/article/category/483612)(16)
* [My Linux](http://blog.csdn.net/dl88250/article/category/278915)(165)
* [NetBeans](http://blog.csdn.net/dl88250/article/category/345108)(225)
* [Network Engineering](http://blog.csdn.net/dl88250/article/category/300046)(63)
* [node.js](http://blog.csdn.net/dl88250/article/category/836154)(1)
* [Open Source](http://blog.csdn.net/dl88250/article/category/296292)(261)
* [Optimization](http://blog.csdn.net/dl88250/article/category/821590)(1)
* [Oracle](http://blog.csdn.net/dl88250/article/category/786046)(6)
* [ORM](http://blog.csdn.net/dl88250/article/category/742694)(4)
* [OSGi](http://blog.csdn.net/dl88250/article/category/364737)(18)
* [PHP](http://blog.csdn.net/dl88250/article/category/743945)(9)
* [Portal &amp; Portlet](http://blog.csdn.net/dl88250/article/category/445013)(8)
* [Python](http://blog.csdn.net/dl88250/article/category/833677)(1)
* [Quartz](http://blog.csdn.net/dl88250/article/category/755291)(1)
* [Regular Expression](http://blog.csdn.net/dl88250/article/category/302630)(16)
* [Ruby &amp; Rails](http://blog.csdn.net/dl88250/article/category/361363)(19)
* [SCA](http://blog.csdn.net/dl88250/article/category/435004)(1)
* [Shell Programming](http://blog.csdn.net/dl88250/article/category/310293)(15)
* [Software Engineering](http://blog.csdn.net/dl88250/article/category/287867)(42)
* [Software Quality Assurance](http://blog.csdn.net/dl88250/article/category/481570)(2)
* [Software Testing](http://blog.csdn.net/dl88250/article/category/318452)(15)
* [Spring Framework](http://blog.csdn.net/dl88250/article/category/336869)(25)
* [StoneAgeDict](http://blog.csdn.net/dl88250/article/category/363915)(28)
* [Struts Framework](http://blog.csdn.net/dl88250/article/category/454387)(3)
* [Subversion](http://blog.csdn.net/dl88250/article/category/469638)(10)
* [SWT/JFace/RCP](http://blog.csdn.net/dl88250/article/category/319645)(25)
* [SyntaxHighlighter](http://blog.csdn.net/dl88250/article/category/747162)(1)
* [System Analyst exam](http://blog.csdn.net/dl88250/article/category/299913)(13)
* [Tencent](http://blog.csdn.net/dl88250/article/category/767001)(1)
* [TestNG](http://blog.csdn.net/dl88250/article/category/799655)(2)
* [TeX/LaTeX](http://blog.csdn.net/dl88250/article/category/357256)(9)
* [Text Categorization](http://blog.csdn.net/dl88250/article/category/260439)(9)
* [Ubuntu](http://blog.csdn.net/dl88250/article/category/742035)(3)
* [UML Modeling](http://blog.csdn.net/dl88250/article/category/318451)(12)
* [VI/VIM](http://blog.csdn.net/dl88250/article/category/832030)(1)
* [Web](http://blog.csdn.net/dl88250/article/category/794848)(4)
* [Web Browser](http://blog.csdn.net/dl88250/article/category/798287)(2)
* [Web Service](http://blog.csdn.net/dl88250/article/category/475410)(5)
* [Web UI Design](http://blog.csdn.net/dl88250/article/category/324813)(23)
* [Wicket](http://blog.csdn.net/dl88250/article/category/803421)(5)
* [Windows](http://blog.csdn.net/dl88250/article/category/267361)(54)
* [Wine](http://blog.csdn.net/dl88250/article/category/753760)(1)
* [Workflow &amp; BPM](http://blog.csdn.net/dl88250/article/category/475341)(17)
* [にほんごのべんきょう](http://blog.csdn.net/dl88250/article/category/368591)(4)
* [li](http://blog.csdn.net/dl88250/article/category/848178)(0)
* [B3log Announcement](http://blog.csdn.net/dl88250/article/category/911428)(3)
* [B3log Solo](http://blog.csdn.net/dl88250/article/category/1290033)(1)
文章存档

* [2013年05月](http://blog.csdn.net/dl88250/article/month/2013/05)(1)
* [2012年11月](http://blog.csdn.net/dl88250/article/month/2012/11)(1)
* [2012年08月](http://blog.csdn.net/dl88250/article/month/2012/08)(1)
* [2012年02月](http://blog.csdn.net/dl88250/article/month/2012/02)(1)
* [2011年10月](http://blog.csdn.net/dl88250/article/month/2011/10)(1)
* [2011年08月](http://blog.csdn.net/dl88250/article/month/2011/08)(1)
* [2011年07月](http://blog.csdn.net/dl88250/article/month/2011/07)(1)
* [2011年06月](http://blog.csdn.net/dl88250/article/month/2011/06)(7)
* [2011年05月](http://blog.csdn.net/dl88250/article/month/2011/05)(6)
* [2011年04月](http://blog.csdn.net/dl88250/article/month/2011/04)(9)
* [2011年03月](http://blog.csdn.net/dl88250/article/month/2011/03)(17)
* [2011年02月](http://blog.csdn.net/dl88250/article/month/2011/02)(27)
* [2011年01月](http://blog.csdn.net/dl88250/article/month/2011/01)(13)
* [2010年12月](http://blog.csdn.net/dl88250/article/month/2010/12)(16)
* [2010年11月](http://blog.csdn.net/dl88250/article/month/2010/11)(29)
* [2010年10月](http://blog.csdn.net/dl88250/article/month/2010/10)(16)
* [2010年09月](http://blog.csdn.net/dl88250/article/month/2010/09)(17)
* [2010年08月](http://blog.csdn.net/dl88250/article/month/2010/08)(10)
* [2010年07月](http://blog.csdn.net/dl88250/article/month/2010/07)(10)
* [2010年06月](http://blog.csdn.net/dl88250/article/month/2010/06)(13)
* [2010年05月](http://blog.csdn.net/dl88250/article/month/2010/05)(12)
* [2010年04月](http://blog.csdn.net/dl88250/article/month/2010/04)(25)
* [2010年03月](http://blog.csdn.net/dl88250/article/month/2010/03)(13)
* [2010年02月](http://blog.csdn.net/dl88250/article/month/2010/02)(10)
* [2010年01月](http://blog.csdn.net/dl88250/article/month/2010/01)(13)
* [2009年12月](http://blog.csdn.net/dl88250/article/month/2009/12)(17)
* [2009年11月](http://blog.csdn.net/dl88250/article/month/2009/11)(9)
* [2009年10月](http://blog.csdn.net/dl88250/article/month/2009/10)(13)
* [2009年09月](http://blog.csdn.net/dl88250/article/month/2009/09)(9)
* [2009年08月](http://blog.csdn.net/dl88250/article/month/2009/08)(13)
* [2009年07月](http://blog.csdn.net/dl88250/article/month/2009/07)(13)
* [2009年06月](http://blog.csdn.net/dl88250/article/month/2009/06)(27)
* [2009年05月](http://blog.csdn.net/dl88250/article/month/2009/05)(13)
* [2009年04月](http://blog.csdn.net/dl88250/article/month/2009/04)(18)
* [2009年03月](http://blog.csdn.net/dl88250/article/month/2009/03)(17)
* [2009年02月](http://blog.csdn.net/dl88250/article/month/2009/02)(15)
* [2009年01月](http://blog.csdn.net/dl88250/article/month/2009/01)(23)
* [2008年12月](http://blog.csdn.net/dl88250/article/month/2008/12)(19)
* [2008年11月](http://blog.csdn.net/dl88250/article/month/2008/11)(34)
* [2008年10月](http://blog.csdn.net/dl88250/article/month/2008/10)(27)
* [2008年09月](http://blog.csdn.net/dl88250/article/month/2008/09)(27)
* [2008年08月](http://blog.csdn.net/dl88250/article/month/2008/08)(19)
* [2008年07月](http://blog.csdn.net/dl88250/article/month/2008/07)(18)
* [2008年06月](http://blog.csdn.net/dl88250/article/month/2008/06)(10)
* [2008年05月](http://blog.csdn.net/dl88250/article/month/2008/05)(31)
* [2008年04月](http://blog.csdn.net/dl88250/article/month/2008/04)(23)
* [2008年03月](http://blog.csdn.net/dl88250/article/month/2008/03)(55)
* [2008年02月](http://blog.csdn.net/dl88250/article/month/2008/02)(78)
* [2008年01月](http://blog.csdn.net/dl88250/article/month/2008/01)(76)
* [2007年12月](http://blog.csdn.net/dl88250/article/month/2007/12)(13)
* [2007年11月](http://blog.csdn.net/dl88250/article/month/2007/11)(28)
* [2007年10月](http://blog.csdn.net/dl88250/article/month/2007/10)(33)
* [2007年09月](http://blog.csdn.net/dl88250/article/month/2007/09)(21)
* [2007年08月](http://blog.csdn.net/dl88250/article/month/2007/08)(68)
* [2007年07月](http://blog.csdn.net/dl88250/article/month/2007/07)(113)
* [2007年06月](http://blog.csdn.net/dl88250/article/month/2007/06)(65)
* [2007年05月](http://blog.csdn.net/dl88250/article/month/2007/05)(83)
* [2007年04月](http://blog.csdn.net/dl88250/article/month/2007/04)(43)
* [2007年03月](http://blog.csdn.net/dl88250/article/month/2007/03)(22)
* [2007年02月](http://blog.csdn.net/dl88250/article/month/2007/02)(74)
* [2007年01月](http://blog.csdn.net/dl88250/article/month/2007/01)(78)
* [2006年12月](http://blog.csdn.net/dl88250/article/month/2006/12)(48)

展开

阅读排行

* [当代 IT 大牛排行榜](http://blog.csdn.net/dl88250/article/details/3852624 "当代 IT 大牛排行榜")(438472)
* [NetBeans 时事通讯（刊号 # 34 - Nov 11, 2008）](http://blog.csdn.net/dl88250/article/details/3280698 "NetBeans 时事通讯（刊号 # 34 - Nov 11, 2008）")(294762)
* [Seam 2.1.1.GA 发布！](http://blog.csdn.net/dl88250/article/details/3604772 "Seam 2.1.1.GA 发布！")(240265)
* [JBoss Seam 框架下的单元测试](http://blog.csdn.net/dl88250/article/details/3765750 "JBoss Seam 框架下的单元测试")(220053)
* [20个漂亮xp桌面主题](http://blog.csdn.net/dl88250/article/details/1636841 "20个漂亮xp桌面主题")(72125)
* [了解 NoSQL 的必读资料](http://blog.csdn.net/dl88250/article/details/5191092 "了解 NoSQL 的必读资料")(65897)
* [初学UML之-------用例图](http://blog.csdn.net/dl88250/article/details/1826713 "初学UML之-------用例图")(64870)
* [数学符号大全](http://blog.csdn.net/dl88250/article/details/4027668 "数学符号大全")(38964)
* [Web Beans (JSR-299): Q&amp;A with Specification Lead Gavin King](http://blog.csdn.net/dl88250/article/details/3753193 "Web Beans (JSR-299): Q&amp;A with Specification Lead Gavin King ")(30472)
* [《星际争霸》成为大学课程之一](http://blog.csdn.net/dl88250/article/details/3862541 "《星际争霸》成为大学课程之一")(26977)
评论排行

* [十二个理由让你不得不期待 Ubuntu10.10](http://blog.csdn.net/dl88250/article/details/5931333 "十二个理由让你不得不期待 Ubuntu10.10")(106)
* [2009 年个人回忆与总结](http://blog.csdn.net/dl88250/article/details/5310751 "2009 年个人回忆与总结")(91)
* [Linux下的千千静听——LivaPlayer](http://blog.csdn.net/dl88250/article/details/1587054 "Linux下的千千静听——LivaPlayer")(69)
* [20个漂亮xp桌面主题](http://blog.csdn.net/dl88250/article/details/1636841 "20个漂亮xp桌面主题")(49)
* [初学UML之-------用例图](http://blog.csdn.net/dl88250/article/details/1826713 "初学UML之-------用例图")(48)
* [书评：简洁代码──敏捷软件工艺指南](http://blog.csdn.net/dl88250/article/details/4303524 "书评：简洁代码──敏捷软件工艺指南 ")(44)
* [程序员的幽默](http://blog.csdn.net/dl88250/article/details/5100894 "程序员的幽默")(43)
* [准备入职支付宝](http://blog.csdn.net/dl88250/article/details/6386620 "准备入职支付宝")(39)
* [HTML 5 WebSocket 示例](http://blog.csdn.net/dl88250/article/details/5118301 "HTML 5 WebSocket 示例")(38)
* [NetBeans IDE 6.9 Beta 发布](http://blog.csdn.net/dl88250/article/details/5518605 "NetBeans IDE 6.9 Beta 发布")(38)

推荐文章
最新评论

* [GAE Java 应用性能优化](http://blog.csdn.net/DL88250/article/details/6174582#comments)

[wilder2000](http://blog.csdn.net/wilder2000): mark
* [基于 JSF＋Spring + JPA 构建敏捷的Web应用[88250原创]](http://blog.csdn.net/DL88250/article/details/2075637#comments)

[喝冰开水](http://blog.csdn.net/az690236414): 把源码发给我还吗？690236414@qq.com谢谢了，最好是我能加你好友，好相互交流
* [数学符号大全](http://blog.csdn.net/DL88250/article/details/4027668#comments)

[低调小一](http://blog.csdn.net/zinss26914): 多谢了，转载了，楼主辛苦
* [java.util.logging日志功能使用快速入门](http://blog.csdn.net/DL88250/article/details/1843813#comments)

[ccssddnnbbookkee](http://blog.csdn.net/ccssddnnbbookkee): 很详细，谢谢
* [使用 CAS 在 Tomcat 中实现单点登录](http://blog.csdn.net/DL88250/article/details/2799522#comments)

[hooqee](http://blog.csdn.net/hooqee): 不错！
* [哪本书是对程序员最有影响、每个程序员都该阅读的书？](http://blog.csdn.net/DL88250/article/details/6227988#comments)

[起飞---为梦想而飞](http://blog.csdn.net/yxm0603): 我一本都没看过，需要好好看看
* [Single SignOn - Integrating Liferay With CAS Server](http://blog.csdn.net/DL88250/article/details/2794943#comments)

[logoc](http://blog.csdn.net/logoc): 咱能，一天到晚不转来转去吗。还他妈的专家
* [暂时告别 CSDN 博客，移居 GAE（http://88250.b3log.org）](http://blog.csdn.net/DL88250/article/details/6612090#comments)

[bubble](http://blog.csdn.net/vbubble): 顶b3blog 对搜索引擎优化方面做的如何
* [加入中国 HTML5 研究小组](http://blog.csdn.net/DL88250/article/details/6208677#comments)

[簡約_Billy](http://blog.csdn.net/vicns): 想您致敬，
* [Java 开源博客——B3log Solo 0.5.5 正式版发布了！](http://blog.csdn.net/DL88250/article/details/8227475#comments)

[簡約_Billy](http://blog.csdn.net/vicns):

Code snips

* [Java Code examples](http://www.java2s.com/)
* [HTML代码示例](http://www.blabla.cn/index.html)
* [C 代码示例](http://www.java2s.com/Code/Cpp/CatalogCpp.htm)
E-books

* [中国 IT 实验室](http://www.chinaitlab.com/)
* [中文电子书网](http://shop.apabi.com/index/index.aspx)
* [网络中国 - E 书](http://book.httpcn.com/)
* [CSDN 下载频道](http://download.csdn.net/)
* [偶要雷锋 - 分享社区](http://www.51leifeng.net/index.php)

Linux/Ubuntu

* [Gnome-Look](http://www.gnome-look.org/)
* [Ubuntu 中文官方论坛](http://forum.ubuntu.org.cn/)
* [deviantART Search](http://search.deviantart.com/)
* [GetDeb](http://www.getdeb.net/)
* [KDE-Look](http://www.kde-look.org/)
* [LinuxToy](http://linuxtoy.org/)
* [Compiz Themes](http://www.compiz-themes.org/)
* [ChinaUnix](http://www.chinaunix.net/)
* [Compiz-Fusion](http://www.compiz-fusion.org/)
My friends

* [光光的Blog~](http://yhuag.blogbus.com/)
* [ZY Tough](http://hi.baidu.com/zy_tough)
* [师傅 dorainm](http://dorainm.cublog.cn/)
* [Eleven 的专栏](http://blog.csdn.net/elevenxl)
* [Meteor 的专栏](http://blog.csdn.net/meteorlWJ)
* [Vanessa 的小窝](http://blog.csdn.net/Vanessa219)
* [秋歌的专栏](http://blog.csdn.net/herian/)
* [金秋风采](http://user.qzone.qq.com/769626482/)
* [eleven-china](http://www.eleven-china.com/)
* [野地的枯草](http://blog.csdn.net/debug_today)
* [狼猫窝](http://hi.baidu.com/%C0%C7%C6%C6%C0%CB)
* [云南科软](http://www.co-soft.info/)
* [88250 @ Solo](http://88250.b3log.org/) ([RSS](http://88250.b3log.org/blog-articles-feed.do))

My projects

* [BeyondTrack @ Java.net](https://beyondtrack.dev.java.net/)
* [LivaPlayer](http://sourceforge.net/projects/livaplayer/)
* [Drop](http://kenai.com/projects/drop/)
* [B3log Solo](http://b3log-solo.googlecode.com/)
* [B3log Latke](http://latke.googlecode.com/)
Super stars :-)

* [Don Knuth's Home Page](http://www-cs-staff.stanford.edu/~knuth/index.html)
* [Martin Fowler](http://www.martinfowler.com/)
* [Alan Turing](http://www.turing.org.uk/turing/)
* [Uncle Bob (Robert C. Martin)](http://blog.objectmentor.com/articles/category/uncle-bobs-blatherings)
* [Bjarne Stroustrup's Homepage](http://www.research.att.com/~bs/)
* [Richard Stallman's Home Page](http://www.stallman.org/)

Technologies

* [IBM 软件技术](http://www-128.ibm.com/developerworks/cn/opensource/)
* [CSDN](http://www.csdn.net/)
* [UML 官方](http://www.uml.org/)
* [Eclipse.org](http://www.eclipse.org/)
* [Apache Software](http://www.apache.org/)
* [LEX & YACC Page](http://dinosaur.compilertools.net/)
* [Java 开源大全](http://www.open-open.com/)
* [JBoss.org](http://labs.jboss.com/)
* [PHP 官方](http://www.php.net/)
* [Springframework.org](http://www.springframework.org/)
* [NetBeans 中文社区](http://www.netbeans.org/index_zh_CN.html)
* [SourceForge.net](http://www.sourceforge.net/)
* [JavaWorld@TW](http://www.javaworld.com.tw/)
* [hibernate.org](http://www.hibernate.org/)
* [Extreme Programming](http://www.extremeprogramming.org/)
* [Ruby on Rails](http://www.rubyonrails.org/)
* [Ruby 中文社区论坛](http://www.ruby-lang.org.cn/forums/)
* [JavaFX Script Reference](http://openjfx.java.sun.com/current-build/doc/)
* [JavaFX Home](http://www.javafx.com/)
* [Open Source Initiative](http://www.opensource.org/)
* [Facelets DevDoc](https://facelets.dev.java.net/nonav/docs/dev/docbook.html)
* [Testng.org](http://www.testng.org/)
* [java-source](http://java-source.net/)
* [JavaEye](http://www.javaeye.com/)
* [NetBeans Dream Team](http://wiki.netbeans.org/NetBeansDreamTeam)
* [InfoQ](http://www.infoq.com/)
* [OpenEBS](https://open-esb.dev.java.net/)
* [JBoss Seam](http://www.seamframework.org/)
* [J 道](http://www.jdon.com/)
* [HTML 4.01 Spec](http://www.w3.org/TR/html4/)
* [歇歇脚](http://xiexiejiao.cn/)
* [HTTP 1.1 Status Code](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)
* [开源中国社区](http://www.oschina.net/)
* [HTML5 研究小组](http://www.mhtml5.com/)

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)[QQ客服](http://wpa.qq.com/msgrd?v=3&uin=2355263776&site=qq&menu=yes) [微博客服](http://e.weibo.com/csdnsupport/profile) [论坛反馈](http://bbs.csdn.net/forums/Service) [联系邮箱：webmaster@csdn.net](mailto:webmaster@csdn.net) 服务热线：400-600-2320京 ICP 证 070598 号北京创新乐知信息技术有限公司 版权所有世纪乐知(北京)网络技术有限公司 提供技术支持江苏乐知网络技术有限公司 提供商务支持Copyright © 1999-2012, CSDN.NET, All Rights Reserved [![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
![](http://counter.csdn.net/pv.aspx?id=24)
