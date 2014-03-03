---
layout: post
title: "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了"
categories: hadoop
tags: 
 - hadoop
--- 

# Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了

原文出处： [钱五哥の共享空间](http://blog.sina.com.cn/s/blog_53a5366c0101dp0q.html)

今天参加了3个keynotes，42个session中的8个，和一大堆厂商讨论技术，真是信息大爆炸的一天。

[![Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了]( "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")](http://cdn2.jobbole.com/2013/06/53a5366c4e020b68545d6amp.jpg "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")

Hadoop从诞生到今年已经有7个年头，今年出现了很多新的变化：

1、Hadoop被公认是一套行业大数据标准开源软件，在分布式环境下提供了海量数据的处理能力（Gartner）。几乎所有主流厂商都围绕Hadoop开发工具、开源软件、商业化工具和技术服务。今年大型IT公司，如EMC、Microsoft、Intel、Teradata、Cisco都明显增加了Hadoop方面的投入，Teradata还公开展示了一个一体机；另一方面创业型Hadoop公司层出不穷，这次看到的几个是Sqrrl、Wandisco、GridGain、InMobi等等，都推出了开源的或者商用的软件。

[![Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了]( "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")](http://cdn2.jobbole.com/2013/06/53a5366c4e020b79dcc7bamp.jpg "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")

2、Hadoop生态系统丰富多彩，但是核心已经被Cloudera、HortonWorks牢牢掌控，基本上没有撼动之可能。今年Hortonworks的宣传是100% open source，Cloudera只好干着急，谁叫他不开放Cloudera Enterprise Manager的源代码呢？Hortonworks介绍Ambari的时候，会场至少5个Cloudera的工程师在仔细聆听，有个小伙不停地在iPad上面速记，竞争可见一斑，个人估计，Cloudera早晚将Enterprise Manager开源。Hortonworks目前Ambari的committer是20+，Contributor 50+，后一个数字可能有些水，但是第一个是没有问题的。目前每天有update，1.25版本比1.0x版本明显好用了。其他大小厂商的生存之道就是搞插件，如Wandisco、vmware、mellanox、GridGain，而且插件均是不用修改内核的外挂 – 这些厂商是没有能力动内核的，持续投入可能会有一些作用，如vmware，但是一线hadoop厂商是绝不会松手的。

3、Hadoop 2.0转型基本上无可阻挡。Hortonworks的VPArun在介绍Tez的时候，给出了很多有趣的ppt，主旨就是一个：MapReduce已经是昨日黄花，Yarn将是未来并行计算的基础设施。我自己还没有使用Yarn，但是Hortonworks已经围绕Yarn开发了很多工具，尤其是Tez，这个玩意可以提升查询计划的执行时间，PIG和Hive将被改写并重装上阵。Hortonworks虽然没有搞出来Impala，但是从更底层的技术上包围Impala，两个老大的布局和较量始终没有停止。

[![Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了]( "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")](http://cdn2.jobbole.com/2013/06/53a5366c4e020b9af392eamp.jpg "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")

[![Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了]( "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")](http://cdn2.jobbole.com/2013/06/53a5366c4e020bac324d8amp.jpg "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")

4、SQL over Hadoop是一个重要的技术趋势。去年Hadoop World时，MPP还吹嘘自己如何牛X。但是Google发布了Dremel和PowerDrill，EMC搞出来HAWQ，Cloudera搞出来Impala之后，所有的MPP都开始反思自己的技术路线。和Parccel技术人员（感觉是售前）讨论了一下，她找出一张卡片说Parccel速度是Hive的100X，领先Impala10年。我感觉这个说话很快就会失灵，首先是Hive的优化一直没有停止，Hortonworks搞出来Tez、Stinger（与Facebook合作）。虽然MPP领先Hadoop很多年，根据80：20原则，如果hadoopSQL只做用户需要的20%特性，那么这个差距最多2年，2年内，hadoopSQL将在部分领域超越MPP。MPP企业的出路就是学习HAWQ。列存储也是推陈出新，近期主要是ORC（MS和Hortonworks合作）、Parquet（Twitter和Cloudera合作），有木有看出来两个巨头PK的身影？有木有看到抱团PK？这些技术在测试中均显示出很大的优势

[![Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了]( "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")](http://cdn2.jobbole.com/2013/06/53a5366c4e020bc314b2bamp.jpg "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")

[![Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了]( "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")](http://cdn2.jobbole.com/2013/06/53a5366c4e020bd3a31e7amp.jpg "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")

[![Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了]( "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")](http://cdn2.jobbole.com/2013/06/53a5366c4e020be5d05c3amp.jpg "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")

5、IT和开源单位合作广泛。这个不仅仅存在IT厂商和开源之间，实际上开源之间也在密切合作。不太清楚合作的内部信息，但是基本上有两种模式：产品/软件交叉集成（含管理系统集成）；合作开发和推广。在技术方面就要求软件有很好的架构，提供开放的接口，这一点Ambari的设计和俺对HT的要求一模一样，可以俺未能如愿，而Amabri已经开发了好几个版本。

6、技术上看，大数据和云的整合也是一个选项（注意，不是趋势，而是选项）。今年新增了OpenStack相关议题，一些集成商和厂商也提出了云上Hadoop的适用场景。这个并不是适用于所有人，但是部分用户可以因此获益。Netflix是一个典型的例子，他们的实例都在AWS上面，显然他们的hadoop是基于虚拟机的，和一个Netflix小伙子（日本人）交流，他们大约有2000个虚拟实例，基于EMR，并开发了Gennie管理系统。

[![Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了]( "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")](http://cdn2.jobbole.com/2013/06/53a5366c4e020bf6e8c61amp.jpg "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")

要睡觉了，4小时后还有一场信息大爆炸！贴一张在宾馆小院乘凉，看到的小松鼠吧，也就距离我5米不到，真要赞一声美帝的环境！

[![Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了]( "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")](http://cdn2.jobbole.com/2013/06/53a5366c07cd01360d282amp.jpg "Hadoop Summit 2013见闻：看完基本了解整个Hadoop生态圈格局和趋势了")

相关信息：

1. [Hadoop Summit 2013 Day1：BOF&Meetup](http://blog.sina.com.cn/s/blog_53a5366c0101doch.html "http://blog.sina.com.cn/s/blog_53a5366c0101doch.html")
![1 vote, average: 5.00 out of 5]( "1 vote, average: 5.00 out of 5")![1 vote, average: 5.00 out of 5]( "1 vote, average: 5.00 out of 5")![1 vote, average: 5.00 out of 5]( "1 vote, average: 5.00 out of 5")![1 vote, average: 5.00 out of 5]( "1 vote, average: 5.00 out of 5")![1 vote, average: 5.00 out of 5]( "1 vote, average: 5.00 out of 5") (***1** 个评分，平均: **5.00***)

![Loading ...]( "Loading ...") Loading ...
