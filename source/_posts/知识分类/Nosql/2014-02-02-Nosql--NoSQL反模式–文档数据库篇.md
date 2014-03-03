---
layout: post
title: "NoSQL反模式 – 文档数据库篇"
categories: Nosql
tags: 
 - Nosql
--- 

# NoSQL反模式 – 文档数据库篇

* [首页](http://www.yankay.com/)

* [关于](http://www.yankay.com/about/)

# [我自然](http://www.yankay.com/)

## 颜开的博客

## NoSQL反模式 - 文档数据库篇

作者: [颜开](http://www.yankay.com/author/admin/ "查看 颜开 的所有文章") 日期: 2013 年 1 月 25 日  [发表评论]( "发表评论？") (14) [查看评论]( "查看评论？")

我们设计关系数据库Schema的都有一套完整的方案，而NoSQL却没有这些。半年前笔者读了本《SQL反模式》的书，觉得非常好。就开始留意，对于NoSQL是否也有反模式？好的反模式可以在我们设计Schema告诉哪里是陷阱和悬崖。NoSQL宣传的时候往往宣称是SchemaLess的，这会让人误解其不需要设计Schema。但如果不意识到设计Schema的必要，陷阱就在一直在黑暗中等着我们。这篇文章就总结一些别人的，也有自己犯过的深痛的设计Schema错误。

NoSQL数据库最主流的有文档数据库，列存数据库，键值数据库。三者分别有代表作MongoDB，HBase和Redis。如果将NoSQL比作兵器的话，可以这样(MySQL是典型的关系型数据库，一样参与比较)： [![]( "Nosql兵器谱")](http://yankaycom-wordpress.stor.sinaapp.com/uploads/2013/01/NosqlImage.png)

* **MySQL**产生年代较早，而且随着LAMP大潮得以成熟。尽管其没有什么大的改进，但是新兴的互联网使用的最多的数据库。就像传统的菜刀，结构简单，几百年没有改进。但是不妨碍产生各式各样的刀法，只要有一把，就能胜任厨房里的大部分事务。MySQL也是一样，核心已经稳定。但是切库，分表，备份，监控，等等手段一应俱全。
* **MongoDB**是个新生事物，提供更灵活的Schema，Capped Collection，异步提交，地理位置索引等五花十色的功能。就像瑞士军刀，不但可以当刀用，还可以开瓶盖，剪指甲。但是他也不比MySQL强，因为还缺乏时间的磨砺。一是系统本身的稳定性，二是开发，运维需要更多经验才能流行。
* **HBase**是个仗势欺人的大象兵。依仗着Hadoop的生态环境，可以有很好的扩展性。但是就像象兵一样，使用者需要养一头大象(Hadoop),才能驱使他。
* **Redis**是键值存储的代表，功能最简单。提供随机数据存储。就像一根棒子一样，没有多余的构造。但是也正是因此，他的伸缩性特别好。就像悟空手里的金箍棒，大可捅破天，小能成缩成针。

### 文档数据库的得失

关系模型试图将数据库模型和数据库实现分开，让开发者可以脱离底层很好的操作数据。但笔者以为关系模型在一些应用场景下有弱点，现在已经不得不面对。

* ******SQL****弱点一：必须支持Join**。因为数据不能够有重复。所以使用范式的关系模型会不可避免的大量Join。如果参与Join的是一张比内存小的表还好。但是如果大表Join或者表分布在多台机器上的话，Join就是性能的噩梦。
* ****SQL弱点二：**计算和存储耦合**。关系模型作为统一的数据模型既可以用于数据分析，也可以用于在线业务。但这两者一个强调高吞吐，一个强调低延时，已经演化出完全不同的架构。用同一套模型来抽象显然是不合适的。Hadoop针对的就是计算的部分。MongoDB,Redis等针对在线业务。两者都抛弃了关系模型。

针对这两个梦魇。文档数据库如MongoDB的的主要目的是** 提供更丰富的数据结构来抛弃Join来适应在线业务**。当然也不是MongoDB完全不能用Join，不能拿来做数据分析，讨论这个只是见仁见智的问题。所以文档数据库并不比关系数据库强大，由于对Join的弱支持，功能会弱许多。设计关系模型的时候，通常只需要考虑好数据直接的关系，定义数据模型。而设计文档数据库模型的时候，还需要考虑应用如何使用。因此设计好一个的文档数据库Schema比设计关系模型更加的困难。除此之外，由于文档数据库事务的支持也是比较弱，一般NoSQL只会提供一个行锁。这也给设计Schema更加增加了难度。对于文档数据库的使用有很多需要注意的地方，本文只关注模型设计的部分。

### 反模式一：惯性思维/沿用关系模型

关系模型是数据存储的经典模型，使用数据模型范式的好处非常的明显。但是由于文档数据库不支持Join(包括和外键息息相关的外键约束)等特性，习惯性的沿用关系模型有的时候会出现问题。需要利用起文档数据库提供的丰富的数据模型来应对。

值得一提的是文档数据库的设计和关系模型不同，是灵活多样的。对于同一个情形，可以设计出有多种能够工作的模型，没有绝对意义上最好的模型。

下图是关系模型和文档模型的对比。
[![]( "关系模型 VS 文档模型")](http://yankaycom-wordpress.stor.sinaapp.com/uploads/2013/01/r-d1.png)

关系模型 VS 文档模型

这个一个博客的数据模型，有Blog,User等表。左侧是关系模型，右侧是文档模型。这个文档模型并不是完全合理，可以作为“正反两面教材”在下文不断阐述。

**问题一：存在描述多对多的关系表**
症状：文档数据库中存储在有纯粹的关系表，例如：
id user_id blog_id 0 0 0 1 0 1

这样的表就算在关系模型中也是不妥的，因为这个ID非常的多余，可以用联合主键来解决。但是在文档数据库中，由于必须强制单主键，不得不采取这样的设计。

坏处：

1. 破坏数据完备性。由于ID是主键，在数据模型上没有约束来保证不出现重复的user_id,blog_id对。一旦数据出现重复，更新删除都是问题。
1. 索引过多。由于是关系表，必须在user_id和blog_id上面分别建一个索引。影响性能。

解决方案：
使用文档数据库典型的处理多对多的办法。不是建立一张关系表，而是在其中一个文档(如User)中，加入一个List字段。
user_id user_name blog_id[] …… 0 Jake 0,1 …… 1 Rose 1,2 ……

 

**问题二:没有区分"一对多关系"和“多对一关系”**
症状：关系模型不区分“一对多”和“多对一”，对于文档数据库来讲，关系模型只有“多对一”。就像这张Comment表：
comment_id user_id content …… 0 0 “NoSQL反模式是好文章” …… 1 0 “是啊” ……

如果整个模型都是这样的“多对一”关系就需要反思了。

坏处：

1. 额外索引。如果客户端已知user_id,需要获得User信息和Comment信息，需要执行两次查询。其中一次查询需要使用索引。并且要在客户端自己Join。这样可能有潜在性能问题。

解决方案：
问题的核心在于是已知user_id查询两张表，还是已知comment_id查询两张表。如果是已知comment_id这样的设计就是合理的，但是如果是已知user_id来查询，把关系放在user表里的设计更合理一些。
user_id user_name comment_id[] …… 0 Jake 0,1 …… 1 Rose 1,2 ……

这样的设计，就可以避免一个索引。同理，对于多对多也是一样的，通过合理的安排字段的位置可以避免索引。

正确使用的场合：

关系型模型是非常成功的数据模型，合理的沿用是非常好的。但是由于文档数据库的特点，需要适当的调整，这样得出的数据模型，尽管性能不是最优，但是有最好的灵活性。并且也有利于和关系数据库转换。

### 反模式二：处处引用客户端Join

症状：数据库设计中充满了xx_id的字端，在查询的时候需要大量的手动Join操作。就涉及到了这个反模式。正如上面提到的博客的关系模型，如果已知blog_id查询comments，需要至少执行3次查询，并且手动Join。

坏处：

1. 手动Join，麻烦且易出错。文档数据库不支持Join且没有外键保证。因此需要在客户端Join，这样的操作对于软件开发来讲是比较繁琐的。由于没有外键保证，因此不能保证取得的ID在数据库里面是有数据的。在处理的时候需要不断判断，容易出错。
1. 多次查询。如果引用过多，查询的时候需要多次查询才能查到足够的数据。本来文档数据库是很快的，但是由于多次查询，给数据库增加了压力，获取全部数据的时间也会增加。
1. 事务处理繁琐。文档数据库一般不支持一般意义上事务，只支持行锁。如果文档数据库有给多个连接。在插入的时候，事务的处理就是噩梦。在文档数据库中使用事务，需要使用行锁，在进行大量的处理。太过繁琐，感兴趣的读者可以搜一下。

解决方案：
适当使用内联数据结构。由于文档数据库支持更复杂的数据结构可以将引用转换为内联的数据，而不用新建一张表。这样做可以解决上面的一些问题，是一个推荐的方案。就像上面博客的例子一样。将五张表简化成了两张表。那什么时候使用内联呢？一般认为
* 使用内联可以解决读性能问题，明显减少Query的次数的时候。
* 可以简化数据模型，化简表之间的关系，而同时不会影响灵活性的时候。
* 事务可以得到简化为单行事务的时候

正确使用的场合：

范式化的使用场景，文档数据库会被多个应用使用。由于数据库设计无法估计多个应用现在及将来的查询情况，需要极大的灵活性。在这个时候，使用引用比内联靠谱。

### 反模式三 滥用内联后患无穷

**问题一 妨碍到查询的内联**
症状：频繁查询一些内联字段，丢弃其他字段。

坏处：

1. 无ID约束：使用内联字段和引用不同，是没有ID约束的。因此不能通过ID(主键)来管理，如果经常需要单独操作内联对象会非常不便。
1. 索引泛滥：如果以内联字段为条件进行查询，需要建立索引。有可能造成索引泛滥。
1. 性能浪费：大部分文档数据库的实现是按行存储的，也就意味着，尽管只查询一个字段，但是DB需要将整行从磁盘中取出。如果字段够小，文档够大，是很不合算的。

解决方案：
如果出现以上的症结，就可以考虑使用引用代替内联了。内联特性主要的用途在于提高性能，如果出现性能不升反降，那就没有意义了。如果对性能有很强烈的要求，可以考虑使用重复数据，同样的数据即在内联字段中也在引用的表里面。这样可以结合内联和引用的性能优势。缺点是数据出现重复，维护会比较麻烦。

**问题二 无限膨胀的内联**
症状：List,Map类型的内联字段不断膨胀，而且没有限制。就像前面提到的Blog的内联字段Comment。如果对每一篇Blog的Comment数量没有限制的话，Comment会无限膨胀。轻则影响性能，重则插入失败。
Blog_id content Comment[] …… 0 “…” “NoSQL反模式是好文章”, “是啊”,”无限增长中”… ……

坏处：

1. 插入失败。文档数据库的每条记录都有最大大小，并且也有推荐最佳的大小。一般不会超过4M。就像刚刚提到的例子，如果是篇热门的博文的话，评论的大小很容易就超过4M。届时文档将无法更新，新的评论无法插入。
1. 性能拖油瓶。由于内联字段膨胀，其大小将远远超过其他部分，影响其他部分的性能表现。并且因此导致该记录大小频繁变化，对档数据库的数据文件内部可能因此产生很多碎片。

解决方案：
设定最大数目或者使用引用。还是Blog和Comment的例子，可以将Comment从Blog中剥离出成一张表。如果考虑到性能，可以在Blog表中新建一个字段如最近的评论。这样既保证了性能，又能够预防膨胀。
Blog_id content last_five_comment[] …… 0 “…” “NoSQL反模式是好文章”, “是啊”,”最多5条”… ……

 

**问题三 无法维护的内联**
症状：DBA想单独维护内联字段，但无法做到。

坏处：

1. 权限管理难。数据库的权限管理的最小粒度是表。如果使用内联技术，就意味着内联部分必须和其他字段用同一个权限来管理。没有办法在DB级别隐藏。
1. 切表难。如果发现一张表的庞大需要切表。这个时候就比较纠结了。如果一刀切，partion Key的选择；索引的失效都会成为问题。如果觉得拆为两张表，就会很好操作的话，就是内联的过度使用了 。
1. 备份难。关系数据库中每张表可以有不同的备份策略。但是如果内联起来，这样的备份就做不到了。
解决办法：

设计数据库模型的时候需要考量之后的维护操作，尤其是内联的字段需不需要单独的维护。需要和运维商量。如果对内联的字段有单独维护的要求，可以拆分出来作为引用。

 
**问题四 盯死应用的内联**
症状：应用可以非常好的运行在数据库上。但是当新的应用接入的时候会很麻烦。因为设计数据模型的时候考虑到了查询。所以当有新应用，新查询接入的时候，就会难于使用原有的模型。

坏处：

1. 新应用接入难。当新的应用试图使用同一个数据库的时候，接入比较困难。因为查询时不同的，需要调整数据模型才能适应。但是调整模型又会影响原有应用。
1. 集成难。不同的关系型数据库可以集成在一起，共同使用。但是对于文档数据库，虽然功能上可以互补，但是由于内联数据结构的差异，也比较难于集成。
1. ETL难。现在大部分的数据分析系统使用的是关系模型，就连Hadoop虽然不用关系模型，但是其上的Hive的常用工具也是按关系模型设计的。

解决方案：

使用范式设计数据库，即用引用代替内联。或者在使用内联的时候，给每个内联对象一个全局唯一的Key，保证其和关系模型直接可以存在映射关系，这样可以提高数据模型的灵活性。如Blog表：
Blog_id content Comment[] …… 0 “…” [{"id"=1,"content"=“NoSQL反模式是好文章”}, {"id"=2,"content"=“是啊”}…] ……

这样的设计既可以利用到内联的好处，又能将其和关系模型映射起来。确定是需要手动维护comment_id，保证其全局唯一性。

 

### 反模式四：在线计算

症状：有一些运行时间很长的Query,由于有聚合计算，索引也不能解决。随着数据量的增长，逐渐成为性能瓶颈。

坏处：

1. 影响用户体验。在线业务中，如果一个查询大于4s，用户体验会急剧下降。按主键和按索引的查询都能满足要求。但是聚合操作往往需要扫描全表或者大量的数据，随着数据量的增加，查询时间会变长，用户不可容忍。
1. 影响数据库性能。长查询的坏处数不清。在线上应用中，如果出现长查询，可能会霸占数据的大部分资源，包括IO，连接，CPU等等。导致其他很好的查询，轻则性能也下降，重者无法使用数据库。长查询可以称之为DB杀手。

解决方案：
首先要权衡，这个聚合操作是不是必要的，必须实时完成。如果没有必要实时完成的话，可以采取离线操作的方案。在夜深人静的时候，跑一个长查询，将结果缓存起来，给第二天使用。如果必须实时完成，则可以新建一个字段，用“incr”这样的操作，在运行的时候，实时聚合结果。而不是查询的时候执行一次长查询。如果逻辑比较复杂，或者觉得大量“incr”操作给数据库系统带来了压力，可以使用Storm之类的实时数据处理框架。总之，要慎用长查询。

### 反模式五：把内联Map对象的Key当作ID用

症状：文档数据库支持内联Map类型。将其中Map的Key当作数据库的主键来用。
Blog_id content Comment{} …… 0 “…” {"1"=“NoSQL反模式是好文章”, "2"=“是啊”} ……

这个反模式很容易犯，因为在编程语言中Map数据结构就是这么用的。但是对于数据库模型来说，这是不折不扣的反模式。

坏处：

1. 无法通过数据库做各种(><=)查询。对于关系型数据库来说，虽然数据结构可以很灵活，但查询的时候都是按层次的。比如comment.id，comment.content。也就是说其Map类型中的Key可以理解为属性名的，而不是用作ID。因此一旦这样使用，就脱离的数据库管制，无法使用各种查询功能。
1. 无法通过索引查询。文档数据可建立索引是需要列名的。比如comment.id。而这样的数据结构没有固定的列名，因此无法建立索引。

解决方案：
使用数组+Map来解决。如：
Blog_id content Comment[] …… 0 “…” [{"id"=1,"content"=“NoSQL反模式是好文章”}, {"id"=2,"content"=“是啊”}…] ……

这样，就可以使用comment.id作为索引，也可以使用数据库的查询功能。简单有效。Map类型中的Key是属性名，Value是属性值。这样的用法是文档数据库数据模型的本意，因此其提供的各种功能才能利用上。否则就无法使用。

 

### 反模式六：不合理的ID

症状：使用String甚至更复杂数据结构作为的ID，或者全部使用数据库提供的自生成ID。如：
id(该ID系系统自生成） Blog_id content …… 0 0 ... ……

坏处:

1. ID混乱。如果使用数据库提供的自生成ID，同时表中还有一个类似有主键含义的Blog_id，这样很不好，容易造成逻辑混乱。由于文档数据库不支持ID的重命名，习惯关系数据库做法的人可能会再建立一个自己的逻辑ID字段。这是没有必要的。
1. 索引庞大，性能低下。ID是数据库的非常重要的部分。ID的长度将决定索引(包括主键的索引)的大小，直接影响到数据库性能。如果索引比内存小，性能会很好。但一旦索引大小超过内存，出现数据交换，性能会急剧下降。一个Long占8字节，一个20个字符的UTF8 String占用约60个字节。相差10倍之巨，不能不考虑。

解决方案：
尽量使用有一定意义的字段做ID，并且不在其他字段中重复出现。不使用复杂的数据类型做ID，只使用int,long或者系统提供的主键类型做ID。

### 文档数据库的反模式总结

阐述了这么多的反模式，下面有个一览表，涵盖了上面所有的反模式。这个一览表，是按照文档数据库模型建立的。是个文档数据库模型的例子。
ID 反模式名 问题 0 存在描述多对多的关系表 [{ID：00
症状：文档数据库中存储在有纯粹的关系表
坏处：[破坏数据完备性，索引过多]
解决方案：加入一个List字段
},{
ID：01
症状：关系模型不区分“一对多”和“多对一”
坏处：额外索引
解决方案：合理的安排字段的位置
}] 1 处处引用客户端Join [{
ID：10
症状：查询的时候需要大量的手动Join操作
坏处：[手动Join，多次查询, 事务处理繁琐]
解决方案：适当使用内联数据结构。
}] 2 滥用内联后患无穷 [{
ID：20
症状：频繁查询一些内联字段，丢弃其他字段
坏处：[无ID约束，索引泛滥, 性能浪费]
解决方案：使用引用代替内联了,允许重复数据
},{
ID：21
症状：List,Map类型的内联字段不断膨胀，而且没有限制
坏处：[插入失败, 性能拖油瓶]
解决方案：设定最大数目或者使用引用。
},{
ID：22
症状：DBA想单独维护内联字段，但无法做到
坏处：[权限管理难, 切表难, 备份难]
解决方案：设计数据库模型的时候需要考量之后的维护操作
},{
ID：23
症状：应用可以非常好的运行在数据库上。但是当新的应用接入的时候会很麻烦。内联盯死了应用
坏处：[新应用接入难, 集成难, ETL难]
解决方案：使用范式设计数据库，即用引用代替内联。保证其和关系模型直接可以存在映射关系
}] 3 在线计算 [{
ID：30
症状：有一些运行时间很长的Query, 逐渐成为性能瓶颈。
坏处：[影响用户体验，影响数据库性能]
解决方案：取消不必要的聚合操作. 运行的时候，实时聚合结果.使用第三方实时或非实时工具。如Hadoop，Storm.
}] 4 把内联Map对象的Key当作ID用 [{
ID：40
症状：文档数据库支持内联Map类型。将其中Map的Key当作数据库的主键来用。
坏处：[无法通过数据库做各种(><=)查询，无法通过索引查询]
解决方案：使用数组+Map来解决。
}] 5 不合理的ID [{
ID：50
症状：用String甚至更复杂数据结构作为的ID，或者全部使用数据库提供的自生成ID。
坏处：[ID混乱，索引庞大]
解决方案：尽量使用有一定意义的字段做ID。不使用复杂的数据类型做ID。
}]

本文试图总结了笔者知道的重要的文档数据库的反模式。现在关于NoSQL数据模型设计模式的讨论才刚刚起步，将来也许会逐渐自成体系。对于列数据库和Key-Value的反模式，笔者等到有了足够积累的时候，再和大家分享。

Share the post "NoSQL反模式 - 文档数据库篇"

* [Weibo](http://service.weibo.com/share/share.php?url=http%3A%2F%2Fwww.yankay.com%2Fnosql-anti-pattern-document%2F "Share this article on Weibo")
[每日心得](http://www.yankay.com/category/%e6%8a%80%e6%9c%af/%e6%af%8f%e6%97%a5%e5%bf%83%e5%be%97/ "查看 每日心得 中的全部文章")[NoSQL](http://www.yankay.com/tag/nosql/), [反模式](http://www.yankay.com/tag/%e5%8f%8d%e6%a8%a1%e5%bc%8f/), [文档数据库](http://www.yankay.com/tag/%e6%96%87%e6%a1%a3%e6%95%b0%e6%8d%ae%e5%ba%93/)

[← 2012年学习小结](http://www.yankay.com/2012%e5%b9%b4%e5%ad%a6%e4%b9%a0%e5%b0%8f%e7%bb%93/)

[Scala Tour - 精选 →](http://www.yankay.com/scala-tour-choiceness/)

[发表评论？]( "发表评论？")

## 14 条评论。

1. ![]() [Heroic](http://heroicyang.com/) [2013 年 1 月 27 日 在 下午 2:32]()

如果采用内联，数据的同步更新方面怎么做呢？比如：User和Blog，Blog采用内联User的一些信息，以便在显示Blog列表时不用再去查询User。但是当User更新了昵称或头像等Blog内联的信息时，该怎么处理呢？需要更新内联信息吗？实时更新？延时更新？（应用的首页就是加载Blog列表）
[回复](http://www.yankay.com/nosql-anti-pattern-document/?replytocom=4583#respond)
1. ![]() [Heroic](http://heroicyang.com/) [2013 年 1 月 28 日 在 上午 12:05]()

想请教一下关于内联设计的数据同步更新问题。比如User和Blog，Blog文档中内联一个简单的User信息结构，如用户名、昵称、头像，以便在首页加载所有的Blog列表时不用再查询User。那如果User的信息更新了该怎么处理呢？是同时更新到Blog的内联文档，还是延时更新？或者说不更新？
[回复](http://www.yankay.com/nosql-anti-pattern-document/?replytocom=4591#respond)

* ![]() 颜开 [2013 年 2 月 13 日 在 下午 7:33]()

如果会导致**重复数据**就不建立这样内联，可以内联UserID。除非当使用UserID的时候，性能会差到不可以接受。这个时候可以考虑延迟更新，不更新之类的更复杂的设计。
[回复](http://www.yankay.com/nosql-anti-pattern-document/?replytocom=4665#respond)
* ![]() vincent [2013 年 1 月 28 日 在 上午 9:30]()

我们项目中用到了mongodb和redis,HBase的确是大象兵,一般公司养不起,mongodb应该更好的利用array或称list,但发现我们还是存在一个问题:使用关系数据库的思想去设计mongodb结构...
[回复](http://www.yankay.com/nosql-anti-pattern-document/?replytocom=4594#respond)

* ![]() Filix [2013 年 3 月 17 日 在 下午 10:39]()

关系模型最大的优点就是，在关系不变的情况下可以适应以后各种需求。
所以，我觉得在mongodb的模型设计中，采用关系模型并不是什么坏事。
文中作者也提到了，各种内嵌式设计都只是对当前问题的解决方法。当我们的需求变化时，蛋疼的事就来了。

所以，我是墙头草，两种设计结合比较好。
[回复](http://www.yankay.com/nosql-anti-pattern-document/?replytocom=5087#respond)
* ![]() clarkorz [2013 年 2 月 14 日 在 上午 11:41]()

内容不错，但我有很多疑问。因为每个反模式只交代了问题，没有交代场景，所以我觉得解决方案对不上。比如反模式一的问题一，关系表的存在肯定是个问题，但是解决该问题的方案要看是什么场景。例如一个可能的场景是“用户点击author浏览该autor发布的blog文章（含内容）分页展示每页10条”，此时用你给出的解决方案只能得到该author发布的所有blog文章的bolg_id，要达到取得10个文章内容的目的，还要再去blog集合中查询1次（或10次）。这个场景我认为把user_id放在blog文档里更合适（甚至可以把user的name、email等重要且不易变的信息也一同放进blog文档里）。这就变成了问题二，必须在blog集合中为user_id增加索引，按照你对问题二的解决方案，把blog_id放在user文档里，只查一次db也只能取得用户信息和一堆blog_id，要取得blog内容还是要再查db。而且，没有经过压力测试也不好说给user_id增加索引会引起多大的性能问题，如果在可接受范围内那也没问题。
所以我觉得就我给出的场景来说，问题二可以不必解决。有必要把和user关联的blog_id及comment_id都放进user文档里的场景，我暂时还没有想到，如果你有例子可以举一个么？：）
[回复](http://www.yankay.com/nosql-anti-pattern-document/?replytocom=4668#respond)

* ![]() 颜开 [2013 年 2 月 16 日 在 下午 12:49]()

谢谢你的留言。
你说的对。对于问题二，我觉得除了加一个索引外，还有一个可能的隐患是分区的困难。因为这张表需要按blog_id,user_id查询，而分区的时候只能选择一个Key。
您觉得呢？
[回复](http://www.yankay.com/nosql-anti-pattern-document/?replytocom=4673#respond)
* ![]() 颜开 [2013 年 2 月 16 日 在 下午 1:09]()

的确，在场景不够明晰的环境下，这些问题之间甚至会自相冲突。我也觉得把blog_id，comment_id都放在user里的场景是很少的。
[回复](http://www.yankay.com/nosql-anti-pattern-document/?replytocom=4674#respond)
* ![]() Cloud [2013 年 2 月 22 日 在 上午 4:15]()

我感觉对于内联内容的更新问题，可以只内联不需要更新的部分，比如用户名称，这种基本不会经常变化的。或者内联用户头像，发表过的评论不随用户profile更新头像，这样还可以让用户想起来自己当时发表评论时使用的头像，这在一些社交网络中会有不错的效果，不过见仁见智了。
[回复](http://www.yankay.com/nosql-anti-pattern-document/?replytocom=4690#respond)
* ![]() [Larry](http://larry-wang.com/) [2013 年 4 月 15 日 在 下午 3:54]()

从王信文的博客上看到你的链接。呵呵，写的很好。我是王小龙，有空交换个链接，嘿嘿。
[回复](http://www.yankay.com/nosql-anti-pattern-document/?replytocom=5648#respond)

* ![]() 颜开 [2013 年 5 月 2 日 在 下午 9:23]()

好啊
[回复](http://www.yankay.com/nosql-anti-pattern-document/?replytocom=5892#respond)
* [电视棒](http://www.xp500.com/Software/multimedia/onlinetv/13002.html) - trackback on 2013 年 5 月 20 日 在 下午 10:35
* [苹果电视棒](http://www.xhfz.net/thread-2843-1-1.html) - trackback on 2013 年 6 月 13 日 在 下午 12:21
* [NoSQL反模式 – 文档数据库篇 - RYAN 大叔](http://www.ryands.net/?p=182) - pingback on 2013 年 6 月 29 日 在 上午 10:37
### 发表评论 [取消回复]()

昵称

邮箱

网址

[![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]()

注意 - 你可以用以下 HTML tags and attributes:

<a href="" title=""> <abbr title=""> <acronym title=""> <b> <blockquote cite=""> <cite> <code> <del datetime=""> <em> <i> <q cite=""> <strike> <strong>

### Trackbacks and Pingbacks:

* [电视棒](http://www.xp500.com/Software/multimedia/onlinetv/13002.html) - Trackback on 2013/05/20/ 22:35
* [苹果电视棒](http://www.xhfz.net/thread-2843-1-1.html) - Trackback on 2013/06/13/ 12:21
* [NoSQL反模式 – 文档数据库篇 - RYAN 大叔](http://www.ryands.net/?p=182) - Pingback on 2013/06/29/ 10:37
[RSS 订阅](http://www.yankay.com/feed/ "RSS 订阅") [微博](http://weibo.com/yankaycom "微博")

### 近期文章

* [Scala Tour - 精选](http://www.yankay.com/scala-tour-choiceness/ "Scala Tour - 精选")
* [NoSQL反模式 - 文档数据库篇](http://www.yankay.com/nosql-anti-pattern-document/ "NoSQL反模式 - 文档数据库篇")
* [2012年学习小结](http://www.yankay.com/2012%e5%b9%b4%e5%ad%a6%e4%b9%a0%e5%b0%8f%e7%bb%93/ "2012年学习小结")
* [Go-简洁的并发](http://www.yankay.com/go-clear-concurreny/ "Go-简洁的并发")
* [Google Spanner原理- 全球级的分布式数据库](http://www.yankay.com/google-spanner%e5%8e%9f%e7%90%86-%e5%85%a8%e7%90%83%e7%ba%a7%e7%9a%84%e5%88%86%e5%b8%83%e5%bc%8f%e6%95%b0%e6%8d%ae%e5%ba%93/ "Google Spanner原理- 全球级的分布式数据库")
* [Google Dremel 原理 - 如何能3秒分析1PB](http://www.yankay.com/google-dremel-rationale/ "Google Dremel 原理 - 如何能3秒分析1PB")
* [Java使用"指针"快速比较字节](http://www.yankay.com/java-fast-byte-comparison/ "Java使用"指针"快速比较字节")
* [HBase介绍PPT](http://www.yankay.com/hbase-slider/ "HBase介绍PPT")
* [Amazon EBS架构猜想](http://www.yankay.com/amazon-ebs%e6%9e%b6%e6%9e%84%e7%8c%9c%e6%83%b3/ "Amazon EBS架构猜想")
* [Linux大文件传输](http://www.yankay.com/linux%e5%a4%a7%e6%96%87%e4%bb%b6%e4%bc%a0%e8%be%93/ "Linux大文件传输")

### 标签

[Android](http://www.yankay.com/tag/android/ "1 个话题") [apache](http://www.yankay.com/tag/apache/ "1 个话题") [arch](http://www.yankay.com/tag/arch/ "1 个话题") [BigTable](http://www.yankay.com/tag/bigtable/ "1 个话题") [Common Cloud](http://www.yankay.com/tag/common-cloud/ "1 个话题") [eclipse](http://www.yankay.com/tag/eclipse/ "2 个话题") [Facebook](http://www.yankay.com/tag/facebook/ "2 个话题") [FriendMap](http://www.yankay.com/tag/friendmap/ "1 个话题") [Gae](http://www.yankay.com/tag/gae/ "3 个话题") [GaeS](http://www.yankay.com/tag/gaes/ "5 个话题") [GaeVfs](http://www.yankay.com/tag/gaevfs/ "1 个话题") [GDP](http://www.yankay.com/tag/gdp/ "1 个话题") [GGG](http://www.yankay.com/tag/ggg/ "1 个话题") [Giant Global Graph](http://www.yankay.com/tag/giant-global-graph/ "1 个话题") [Google Reader](http://www.yankay.com/tag/google-reader/ "1 个话题") [Google Reader Play](http://www.yankay.com/tag/google-reader-play/ "1 个话题") [Hadoop](http://www.yankay.com/tag/hadoop/ "2 个话题") [Hbase](http://www.yankay.com/tag/hbase/ "2 个话题") [HTML5](http://www.yankay.com/tag/html5/ "2 个话题") [Java](http://www.yankay.com/tag/java/ "4 个话题") [JPA2.0](http://www.yankay.com/tag/jpa2-0/ "1 个话题") [Jsa4j](http://www.yankay.com/tag/jsa4j/ "2 个话题") [latex](http://www.yankay.com/tag/latex/ "2 个话题") [Linux](http://www.yankay.com/tag/linux/ "2 个话题") [maven](http://www.yankay.com/tag/maven/ "1 个话题") [NoSQL](http://www.yankay.com/tag/nosql/ "5 个话题") [TortoiseHg](http://www.yankay.com/tag/tortoisehg/ "2 个话题") [Ubuntu](http://www.yankay.com/tag/ubuntu/ "2 个话题") [中文](http://www.yankay.com/tag/%e4%b8%ad%e6%96%87/ "2 个话题") [中科杯](http://www.yankay.com/tag/%e4%b8%ad%e7%a7%91%e6%9d%af/ "1 个话题") [乱码](http://www.yankay.com/tag/%e4%b9%b1%e7%a0%81/ "2 个话题") [优化](http://www.yankay.com/tag/%e4%bc%98%e5%8c%96/ "2 个话题") [动态规划](http://www.yankay.com/tag/%e5%8a%a8%e6%80%81%e8%a7%84%e5%88%92/ "1 个话题") [同学](http://www.yankay.com/tag/%e5%90%8c%e5%ad%a6/ "1 个话题") [图书馆](http://www.yankay.com/tag/%e5%9b%be%e4%b9%a6%e9%a6%86/ "1 个话题") [在水滩](http://www.yankay.com/tag/%e5%9c%a8%e6%b0%b4%e6%bb%a9/ "1 个话题") [数据库](http://www.yankay.com/tag/%e6%95%b0%e6%8d%ae%e5%ba%93/ "1 个话题") [文件](http://www.yankay.com/tag/%e6%96%87%e4%bb%b6/ "1 个话题") [模板](http://www.yankay.com/tag/%e6%a8%a1%e6%9d%bf/ "1 个话题") [江苏杯](http://www.yankay.com/tag/%e6%b1%9f%e8%8b%8f%e6%9d%af/ "1 个话题") [知识图](http://www.yankay.com/tag/%e7%9f%a5%e8%af%86%e5%9b%be/ "1 个话题") [立志](http://www.yankay.com/tag/%e7%ab%8b%e5%bf%97/ "1 个话题") [维基百科](http://www.yankay.com/tag/%e7%bb%b4%e5%9f%ba%e7%99%be%e7%a7%91/ "1 个话题") [视频](http://www.yankay.com/tag/%e8%a7%86%e9%a2%91/ "1 个话题") [酒](http://www.yankay.com/tag/%e9%85%92/ "1 个话题")
### 我的应用

* [Scala Tour](http://zh.scala-tour.com/ "精彩的Scala之旅")
* [趣味应用-微八卦](http://weibagua.cloudfoundry.com/)

### 相关组织

* [EMC中国研究院](http://qing.weibo.com/emclabschina "EMC中国研究院")
* [nosqlfan](http://blog.nosqlfan.com/)
### 友情链接

* [Layne Peng的博客](http://laynepeng.cloudfoundry.com/)
* [爱陶](http://www.iiira.com/)
* [王小龙的博客](http://larry-wang.com/)
* [高晟](http://loveharbor.net/)
版权所有 © 2013 我自然 | Powered by [WordPress](http://wordpress.org/) and [zBench](http://zww.me/)
↑ [回到顶部]( "Back to top")
