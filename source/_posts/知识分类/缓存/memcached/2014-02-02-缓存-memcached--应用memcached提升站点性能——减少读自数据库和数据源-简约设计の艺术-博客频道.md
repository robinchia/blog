---
layout: post
title: "应用 memcached 提升站点性能——减少读自数据库和数据源 - 简约设计の艺术 - 博客频道 "
categories: 缓存
tags: 
 - 缓存
 - memcached
--- 

# 应用 memcached 提升站点性能——减少读自数据库和数据源 - 简约设计の艺术 - 博客频道 - CSDN.NET

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

### [应用 memcached 提升站点性能——减少读自数据库和数据源]()

分类： [Cache](http://blog.csdn.net/DL88250/article/category/768467) [Memcached](http://blog.csdn.net/DL88250/article/category/768466) [Data-Structrue/Algorithms](http://blog.csdn.net/DL88250/article/category/259943) [Open Source](http://blog.csdn.net/DL88250/article/category/296292) [Architecture Design](http://blog.csdn.net/DL88250/article/category/362353)  2010-12-21 09:14 4720人阅读 [评论]()(4) [收藏]( "收藏") [举报]( "举报")
[memcached](http://blog.csdn.net/tag/details.html?tag=memcached)[数据库](http://blog.csdn.net/tag/details.html?tag=%e6%95%b0%e6%8d%ae%e5%ba%93)[存储](http://blog.csdn.net/tag/details.html?tag=%e5%ad%98%e5%82%a8)[服务器](http://blog.csdn.net/tag/details.html?tag=%e6%9c%8d%e5%8a%a1%e5%99%a8)[ibm](http://blog.csdn.net/tag/details.html?tag=ibm)[websphere](http://blog.csdn.net/tag/details.html?tag=websphere)

目录[(?)]( "系统根据文章中H1到H6标签自动生成文章目录")[[+]]( "展开")

1. [简介]()
1. [基础知识]()
1. [何时使用 memcached]()
1. [键名称空间和值]()
1. [填充并使用 memcached]()
1. []()
1. [弹性和可用性]()
1. [分配缓存]()
1. [如何能不使用 memcached]()

1. [memcached 不是一个数据库]()
1. [不要缓存数据库行或文件]()
1. [memcached 并不安全]()

1. [不要限制自己]()
1. [结束语]()
1. [参考资料]()
1. [关于作者]()
![Memcached Banner]()

[Martin Brown](http://www.ibm.com/developerworks/cn/opensource/os-memcached/#author1), 自由撰稿人, Freelance Developer
Martin Brown 成为专业作家已有七年多的时间了。他是题材广泛的众多书籍和文章的作者。他的专业技术涉及各种开发语言和平台 —— Perl、Python、Java™、JavaScript、Basic、Pascal、Modula-2、C、C++、Rebol、Gawk、 Shellscript、Windows、Solaris、Linux®、BeOS、Mac OS/X 等等，还涉及 Web 编程、系统管理和集成。Martin 是 Microsoft® 的主题专家（SME），并且是 ServerWatch.com、LinuxToday.com 和 IBM developerWorks 的定期投稿人，他还是 Computerworld、The Apple Blog 和其他站点的正式博客。您可以通过他的 Web 站点 : [http://www.mcslp.com](http://www.mcslp.com/) 与他联系。

**简介：** 开源 *memcached* 工具是一个用来存储常用信息的缓存，有了它，您便无需从缓慢的资源，比如磁盘或数据库，加载（并处理）信息了。该工具可部署在专用的情况下，也可作为用完 现有环境内的多余内存的一种方法。尽管 memcached 十分简便，但有时它仍被不当使用，或被用在错误的环境类型中。在本文中，了解使用 memcached 的最佳时机。

## []()[简介]()

memcached 常被用来加速应用程序的处理，在这里，我们将着重于介绍将它部署于应用程序和环境中的最佳实践。这包括应该存储或不应存储哪些、如何处理数据的灵活分布以 及如何调节用来更新 memcached 和所存储数据的方法。我们还将介绍对高可用性的解决方案的支持，比如 IBM WebSphere® eXtreme Scale。

所有的应用程序，特别是很多 web 应用程序都需要优化它们访问客户机和将信息返回至客户机的速度。可是，通常，返回的都是相同的信息。从数据源（数据库或文件系统）加载数据十分低效，若是每次想要访问该信息时都运行相同的查询，就尤显低效。

虽然很多 web 服务器都可被配置成使用缓存发回信息，但那与大多数应用程序的动态特性无法相适。而这正是 memcached 的用武之地。它提供了一个通用的内存存储器，可保存任何东西，包括本地语言的对象，这就让您可以存储各种各样的信息并可以从诸多的应用程序和环境访问这些信息。

## []()[基础知识]()

memcached 是一个开源项目，旨在利用多个服务器内的多余 RAM 来充当一个可存放经常被访问信息的内存缓存。这里的关键是使用了术语*缓存*：memcached 为加载自他处的信息提供的是内存中的暂时存储。

比如，考虑这样一个典型的基于 web 的应用程序。即便是一个动态网站可能也会有一些组件或信息常量是贯穿页面整个生命周期的。在一个博客站点内，针对单个 blog post 的类别列表不大可能在页面查看间经常性地变更。每次都通过一个对数据库的查询加载此信息相对比较昂贵，特别是在数据没有更改的情况下，就更是如此。从图 1 可以看到一个博客站点内可被缓存的页面分区。

[**图 1. 一个典型的博客页面内的可缓存元素**]()
![此图显示了可缓存的 blog 元素及其布局：顶部是 Page Header，左边是当前的 Current post lists，右边是 About、Post History、External Links 和 Category list]()

将这种结构放在 blog 站点的其他元素，poster 信息、注释 — 设置 blog post 本身 — 进行推断，可以看出为了显示主页的内容很可能需要发生 10-20 次数据库查询和格式化。 每天对数百甚至数千的的页面查看重复此过程，那么您的服务器和应用程序执行的查询要远远多于为了显示页面内容所需执行的查询。

通过使用 memcached，可以将加载自数据库的格式化信息存储为一种可直接用在 Web 页面上的格式。并且由于信息是从 RAM 而不是通过数据库和其他处理从磁盘加载的，所以对信息的访问几乎是瞬时的。

再强调一下，memcached 是一个用来存储常用信息的缓存，有了它，您便无需从缓慢的资源，比如磁盘或数据库，加载并处理信息了。

对 memcached 的接口是通过网络连接提供的。这意味着您可以在多个客户机间共享单个的 memcached 服务器（或多个服务器，如本文稍后所示的）。这个网络接口非常迅速，并且为了改善性能，服务器会故意不支持身份验证或安全性通信。但这不应限制部署选项。 memcached 服务器应该存在于您网络的*内部*。网络接口的实用性以及可以部署多个 memcached 实例的简便性让您可以使用多个机器上的*多余* RAM 来提高您缓存的整体大小。

memcached 的存储方法是一个简单的键/值对，类似于很多语言内的散列或关联数组。通过提供键和值来将信息存储到 memcached 内，通过按特定的键请求信息来恢复信息。

信息会无限期地保留在缓存内，除非发生如下的情况：

1. **为缓存分配的内存耗尽** — 在这种情况下，memcached 使用 LRU（最近最少使用）方法从此缓存删除条目。最近未曾使用的条目会从此缓存中先删除，最旧的最先访问。
1. **条目被明确删除** — 总是可以从此缓存内删除条目。
1. **条目过期失效** — 各条目均有一个有效的期限以便针对此键存储的信息在过于陈旧时可从缓存中清除这些条目。

上述这些情况可以与您应用程序的逻辑综合使用以便确保缓存内的信息是最新的。有了这些基础知识后，让我们来看看在应用程序内如何能最好地利用 memcached。

## []()[何时使用 memcached]()

在使用 memcached 改进应用程序性能时，可以对一些关键的过程和步骤进行修改。

在加载信息时，典型的场景如图 2 所示。

[**图 2. 加载要显示的信息的典型顺序**]()
![此图显示了从加载数据到处理/格式化数据再到发送数据至客户机的流程]()

一般而言，这些步骤是：

1. 执行一个或多个查询来从数据库加载信息
1. 格式化适合于显示（或进一步处理）的信息
1. 使用或显示格式化了的数据

在使用 memcached 时，为配合这个缓存，可对应用程序的逻辑进行稍许修改：

* 尽量从缓存加载信息
* 如果存在，使用信息的被缓存版本
* 如果它不存在：

1. 执行一个或多个查询来从数据库加载信息
1. 格式化适合于显示或进一步处理的信息
1. 将信息存储到缓存内
1. 使用格式化了的数据

图 3 是对这些步骤的总结。

[**图 3. 在使用 memcached 时加载适合于显示的信息**]()
![此图显示了如果所请求的数据位于缓存内，它就会跳过所有处理步骤，节省了时间]()

数据加载成为了至多三个步骤的一个过程，从缓存加载数据或从数据库（视情况而定）加载数据并存储在缓存内。

当这个过程首次发生时，数据将正常地从数据库或其他数据源加载，然后再存储到 memcached 内。当下一次访问此信息时，它就会从 memcached 拉出，而不是从数据库加载，节省了时间和 CPU 循环。

问题的另一个方面是要确保如果更改了要存储在 memcached 内的信息，在更新后端信息的同时还要更新 memcached 的版本。这会让图 4 内所示的这个典型顺序发生稍许变化，如 [图 5](http://www.ibm.com/developerworks/cn/opensource/os-memcached/#f5) 所示。

[**图 4. 在一个典型的应用程序内更新或存储数据**]()
![此图显示了从更新数据到处理/格式化数据再到发送更新了的数据至客户机的流程]()

图 5 显示了使用 memcached 后发生了变化的流程。

[**图 5. 在使用 memcached 时更新或存储数据**]()
![此图显示了拓展了的流程：从更新数据到处理/格式化数据，再到将数据存储在 memcached 内，最后到发送更新了的数据至客户机]()

比如，仍以博客站点为例，在博客系统更新数据库内的类别列表时，更新应该遵循如下顺序：

1. 更新数据库内的类别列表
1. 格式化信息
1. 将信息存储到 memcached 内
1. 将信息返回至客户机

memcached 内的存储操作是原子的，所以信息的更新不会让客户机只获得部分数据；它们获得的或者是老版本，或者是新版本。

对于大多数应用程序，这两个操作是您惟一需要注意的。在访问他人使用的数据时，它会自动被添加到这个缓存内，而且如果对该数据进行了更改，此缓存内也会自动进行更新。

## []()[键、名称空间和值]()

memcached 另一个需要重点考虑的因素是如何组织和命名存储在缓存内的这些数据。从之前博客站点的例子中，不难看出需要使用一种一致的命名结构以便您能加载博客类别、历史和其他信息，然后再在加载信息（并更新缓存）时或者在更新数据（同样也要更新缓存）时使用。

使用的何种具体的命名系统特定于应用程序，但通常可以使用一种与现有应用程序类似的结构，并且这种结构很可能基于某种惟一识别符。当从数据库拉出信息或在整理信息集时，就会发生这种情况。

以 blog post 为例，可以在一个具有键

category-list
的项中存储类别列表。与此 post ID 对应的单个 post，比如

blogpost-29
相关的值都可以使用，而该项的注释则可以存储在

blogcomments-29
内，其中 *29* 就是这个 blog post 的 ID。这样一来， 您就可以将各种各样的信息存储在缓存内，使用不同的前缀来标识这些信息。

memcached 键/值存储的简便性（以及安全性的缺乏）意味着如果您想要在使用同一个 memcached 服务器的同时支持多个应用程序，那么就可以考虑使用其他格式的量词来标识数据属于某种特定的应用程序。比如，可以添加像

blogapp:blogpost-29
这样的应用程序前缀。这些键是没有格式的，所以可以使用任何字符串作为键的名称。

在存储值的方面，应该确保存储在缓存内的信息适合于您的应用程序。比如，对于这个博客系统，您可能想要存储被博客应用程序使用的对象以便格式化博客信息，而不是原始的 HTML。如果同一个基础结构用在应用程序内的多个地方，这一点更具实用性。

大多数语言的接口，包括 Java™、Perl、PHP 等，都能串行化语言对象以便存储在 memcached 内。这就让您可以存储并随后从内存存储恢复全部对象，而不是在您的应用程序内手动重构它们。 很多对象，或它们使用的结构，都基于某种散列或数组结构。对于跨语言的环境，比如在 JSP 环境和 JavaScript 环境间共享相同信息，可以使用一种架构中立的格式，比如 JavaScript Object Notation (JSON) 甚或 XML。

## []()[填充并使用 memcached]()

作为一种开源产品以及一种最初开发用来工作于现有开源环境内的产品，memcached 受大量环境和平台支持。与 memcached 服务器通信的接口有很多，并常常具有针对所有语言的多个实现。参见 [参考资料](http://www.ibm.com/developerworks/cn/opensource/os-memcached/resources.html) 以获得常用的库和工具箱。

要列出所有受支持的接口和环境不太可能，但它们均支持 memcached 协议提供的基础 API。这些描述已经被简化并应用在不同语言的上下文内，在这些语言中，使用不同的值可指示错误。主要的函数有：

* 
get(key)
— 从存储了特定键的 memcached 获得信息。 如果键不存在，就返回错误。
* 
set(key, value [, expiry])
— 使用缓存内的标识符键存储这个特定的值。如果键已经存在，那么它就会被更新。期满时间的单位为秒，并且如果值小于 30 天 (30*24*60*60)，那么就用作相对时间，如果值大于 30 天，那么就用作绝对时间 (epoch)。
* 
add(key, value [, expiry])
— 如果键不存在就将这个键添加到缓存内，如果键已经存在就返回错误。如果您想要显式地添加一个新键而又不会因它已经存在而更新它，那么这个函数将十分有用。
* 
replace(key, value [, expiry])
— 更新此特定键的值，如果键不存在就返回一个错误。
* 
delete(key [, time])
— 从缓存中删除此键/值对。如果您提供一个时间，那么添加具有此键的一个新值就会被阻塞这个特定的时期。超时让您可以确保此值总是可以重新读取自您的数据中心。
* 
incr(key [, value]
) — 为特定的键增 1 或特定的值。只适用于数值。
* 
decr(key [, value])
— 为特定的键减 1 或特定的值，只适用于数值。
* 
flush_all
— 让缓存内的所有当前条目无效（或到期失效）。

比如，在 Perl 内，基本 set 操作可以如清单 1 所示的那样处理。

[**清单 1. Perl 内的基本 set 操作**]()
use Cache::Memcached;
my $cache = new Cache::Memcached {
'servers' => [
'localhost:11211',
],
};
$cache->set('mykey', 'myvalue');

 

Ruby 内的相同的基本操作如清单 2 所示。

[**清单 2. Ruby 内的基本 set 操作**]()
require 'memcache'
memc = MemCache::new '192.168.0.100:11211'
memc["mykey"] = "myvalue"

 

在两个例子中可以看到相同的基本结构：设置 memcached 服务器，然后分配或设置值。其他的接口也可用，包括适合于 Java 技术的那些接口，让您可以在 WebSphere 应用程序内使用 memcached。memcached 接口类允许将 Java 对象直接序列化到 memcached 以便于存储和加载复杂的结构。当在像 WebSphere 这样的环境内进行部署时，有两个事情非常重要：服务的弹性（在 memcached 不可用时如何做）以及如何提高缓存存储量来改进在使用多个应用程序服务器或在使用像 WebSphere eXtreme Scale 这样的环境时的性能。我们接下来就来看看这两个问题。

## []()

## []()[弹性和可用性]()

有关 memcached 最常见的一个问题是：“若缓存不可用了，会发生什么情况呢？”正如之前章节中明示的，缓存内的信息不应该成为信息的的惟一资源。必须要能够从其他位置加载存储在缓存内的数据。

虽然，无法从缓存访问信息将会减缓应用程序的性能，但它不应该阻止应用程序的运转。可能会发生这样几个场景：

1. 如果 memcached 服务宕掉，应用程序应该回退到从原始数据源加载信息并对信息进行显示所需的格式化。此应用程序还应继续尝试在 memcached 内加载和存储信息。
1. 一旦 memcached 服务器恢复可用，应用程序就应该自动尝试存储数据。没有必要强制重载已缓存了的数据，可以使用标准的访问来用信息加载和填充缓存。最终，缓存将会被最常用的数据重新填充。

再次重申，memcached 是信息的缓存但并非惟一的数据源。memcached 服务器不可用不应该是应用程序的终结，虽然这意味着在 memcached 服务器恢复正常之前性能会有所降低。实际上，memcached 服务器相对简单，并且虽然不是绝对无故障的，但它的简单性的结果就是它很少会出错。

## []()[分配缓存]()

memcached 服务器只是网络上针对一些键存储值的一个缓存。如果有多台机器，那么很自然地会想要在所有多余机器上设置一个 memcached 的实例来提供一个超大的联网 RAM 缓存存储。

有了这个想法后，还有一种想当然是需要使用某种分配或复制机制来在机器之间复制键/值对。这种方式的问题是如果这么做反而会减少可用的 RAM 缓存，而不是增加。如图 6 所示，可以看出这里有三个应用程序服务器，每个服务器都可以访问一个 memcached 实例。

[**图 6. 多重 memcached 实例的不正确使用**]()
![此图显示了 memcached 的三个独立的 1-GB 实例，支持三个应用程序服务器，各产出 1 GB 的缓存空间]()

尽管每个 memcached 实例都是 1 GB 的大小（产生 3 GB 的 RAM 缓存），但如果每个应用程序服务器只有其自己的缓存（或者在 memcached 之间存在着数据的复制），那么整个安装也仍只能有 1 GB 的缓存在每个实例间复制。

由于 memcached 通过一个网络接口提供信息，因此单个的客户机可以从它所能访问的任何一个 memcached 实例访问数据。如果数据没有跨每个实例被复制，那么最终在每个应用程序服务器上，就可以有 3 GB 的 RAM 缓存可用，如图 7 所示。

[**图 7. 多重 memcached 实例的正确使用**]()
![此图显示了三个交互的 1-GB memcached 实例，支持三个应用程序服务器，导致总体 3 GB 的共享缓存空间]()

这个方法的问题是选择哪个服务器来储存键/值对，以及当想要重新获得一个值时，如何决定要与哪个 memcached 服务器对话。问题的解决方案就是忽略复杂的东西，比如查找表，或是寄望 memcached 服务器来为您处理这个过程。而 memcached 客户机则必须要力求简单。

memcached 客户机不必决定此信息，它只需对在存储信息时指定的键使用一个简单的散列算法。当想要从一列 memcached 服务器存储或获取信息时，memcached 客户机就会用一个一致的散列算法从这个键获取一个数值。举个例子，键

mykey
被转换成数值

23875
。是保存还是获取信息无关紧要，这个键将总是被用作惟一标识符来从 memcached 服务器加载，因此在本例中，“mykey” 散列转化后对应的值总是

23875
。

如果有两个服务器，那么 memcached 客户机将对这个数值进行一个简单的运算（例如，系数）来决定它应将此值存储在第一个还是第二个配置了的 memcached 实例上。

当存储一个值时，客户机会从这个键确定出散列值以及它原来存储在哪个服务器上。当获取一个值时，客户机会从这个键确定出相同的散列值并会选择相同的服务器来获取信息。

如果在每个应用程序服务器上使用的是相同的服务器列表（并且顺序相同），那么当需要保存或检索同一个键时，每个应用程序服务器都将选择同一个 服务器。现在，在这个例子中，有 3GB 的 memcached 空间可以共享，而不是同一个 1 GB 的空间的复制，这就带来了更多的可用缓存，并很有可能会提高有多个用户情况下的应用程序的性能。

这个过程也有其复杂性（比如当一个服务器不可用时会怎样），更多信息，请参见相关文档（参见 [参考资料](http://www.ibm.com/developerworks/cn/opensource/os-memcached/resources.html)）。

## []()[如何能不使用 memcached]()

尽管 memcached 很简单，但 memcached 实例有时候还是会被不正确地使用。

### []()[memcached 不是一个数据库]()

最常见的 memcached 误用就是把它用作一个数据存储，而不是一个缓存。memcached 的首要目的就是加快数据的响应时间，否则数据从其他数据源构建或恢复需要很长时间。一个典型的例子就是从一个数据库中恢复信息，特别是在信息显示给用户前 需要对信息进行格式化或处理的时候。Memcached 被设计用来将信息存储在内存中以避免每次在数据需要恢复时重复执行相同的任务。

切不可将 memcached 用作运行应用程序所需信息的惟一信息源；数据应总是可以从其他信息源获取。此外，要记住 memcached 只是一个键/值的存储。不能在数据上执行查询，或者对内容进行迭代来提取信息。应该使用它来存储数据块或对象以备批量使用。

### []()[不要缓存数据库行或文件]()

虽然可以使用 memcached 存储加载自数据库的数据行，但这实际上是查询缓存，并且大多数数据库都提供各自的查询缓存的机制。其他的对象，比如文件系统的图像或文件的情况与此相同。很多应用程序和 web 服务器针对此类工作已经有了一些很好的解决方案。

如果在加载和格式化后，使用它来存储全部信息块，就可以从 memcached 获得更多的实用工具和性能上的改善。仍以我们的博客站点为例，存储信息的最佳点是在将博客类别格式化为对象，甚至是在格式化成 HTML 后。博客页面的构造可通过从 memcached 加载各个组件（比如 blog post、category list、post history 等）并将完成的 HTML 写回至客户机实现。

### []()[memcached 并不安全]()

为了确保最佳性能，memcached 并未提供任何形式的安全性，没有身份验证，也没有加密。这意味着对 memcached 服务器的访问应该这么处理：一是通过将它们放到应用程序部署环境相同的私有侧，二是如果安全性是必须的，那么就使用 UNIX® socket 并只允许当前主机上的应用程序访问此 memcached 服务器。

这多少牺牲了一些灵活性和弹性，以及跨网络上的多台机器共享 RAM 缓存的能力，但这是在目前的情况下确保 memcached 数据安全性的惟一一种解决方案。

## []()[不要限制自己]()

除了不应该使用 memcached 实例的情况外，memcached 的灵活性不应忽视。由于 memcached 与应用程序处于相同的架构水平，所以很容易集成并连接到它。并且更改应用程序以便利用 memcached 也并不复杂。此外，由于 memcached 只是一个缓存，所以在出现问题时它不会停止应用程序的执行。如果使用正确的话，它所做的是减轻其余服务器基础设施的负载（减少对数据库和数据源的读操 作），这意味着无需更多的硬件就可以支持更多的客户机。

但请记住，它仅仅是个缓存！

## []()[结束语]()

在本文中，我们了解了 memcached 以及如何最佳地使用它。我们看到了信息如何存储、如何选择合理的键以及如何选择要存储的信息。我们还讨论了所有 memcached 用户都要遇到的一些关键的部署问题，包括多服务器的使用、当 memcached 实例消亡时该怎么做，以及（也许最为重要的）在哪些情况下不能使用 memcached。

作为一种开源的应用程序并且是目的简单而直白的应用程序，memcached 的功能和实用性均来自于这种简单性。通过为信息提供巨大的 RAM 存储空间、让它在网络上可用，然后再让它可通过各种不同的接口和语言访问到，memcached 可被集成到多种多样的安装和环境中。

 

## []()[参考资料]()

**学习**

* [MySQL memcached 文档](http://dev.mysql.com/doc/refman/5.1/en/ha-memcached.html) 提供了很多有关如何在一个典型的数据库部署环境内使用 memcached 的信息。
* 通过体验 [IBM solidDB 产品家族](http://www.ibm.com/developerworks/cn/data/products/soliddb/)，了解 IBM 的商业缓存解决方案 solidDB®。
* 随时关注 developerWorks [技术活动](http://www.ibm.com/developerworks/cn/offers/techbriefings/)和[网络广播](http://www.ibm.com/developerworks/cn/swi/)。
* 查阅最近将在全球举办的面向 IBM 开放源码开发人员的研讨会、交易展览、网络广播和其他 [活动](http://www.ibm.com/developerworks/views/opensource/events.jsp)。
* 访问 developerWorks [Open source 专区](http://www.ibm.com/developerworks/cn/opensource/)获得丰富的 how-to 信息、工具和项目更新以及[最受欢迎的文章和教程](http://www.ibm.com/developerworks/cn/opensource/best2009/index.html)，帮助您用开放源码技术进行开发，并将它们与 IBM 产品结合使用。
* 查看免费的 [developerWorks On demand 演示](http://www.ibm.com/developerworks/offers/lp/demos/)，观看并了解 IBM 及开源技术和产品功能。

**获得产品和技术**

* [memcached.org](http://memcached.org/) 提供了有关 memcached 以及如何下载并安装它的信息。
* [Cache::Memcached interface for Perl](http://search.cpan.org/%7Ebradfitz/Cache-Memcached-1.28/) 提供了一个广泛的接口。
* 对于 Java 技术，可使用 [com.danga.MemCached 类](http://whalin.com/memcached/)，它会提供一些额外的故障转换及多实例扩展。
* 使用 [IBM 产品评估试用版软件](http://www.ibm.com/developerworks/cn/downloads/) 改进您的下一个开发项目，这些软件可以通过下载获得。
* 下载 [IBM 产品评估试用版软件](http://www.ibm.com/developerworks/cn/downloads/) 或 [IBM SOA Sandbox for People](http://www.ibm.com/developerworks/cn/downloads/soasandbox/people/)，并开始使用来自 DB2®、Lotus®、Rational®、Tivoli® 和 WebSphere® 的应用程序开发工具和中间件产品。

**讨论**

* 参与 [developerWorks 博客](http://www.ibm.com/developerworks/blogs) 加入 developerWorks 社区。
* 欢迎加入 [My developerWorks 中文社区](http://www.ibm.com/developerworks/cn/mydeveloperworks/)。

## []()[关于作者]()

[]()Martin Brown 成为专业作家已有七年多的时间了。他是题材广泛的众多书籍和文章的作者。他的专业技术涉及各种开发语言和平台 —— Perl、Python、Java™、JavaScript、Basic、Pascal、Modula-2、C、C++、Rebol、Gawk、 Shellscript、Windows、Solaris、Linux®、BeOS、Mac OS/X 等等，还涉及 Web 编程、系统管理和集成。Martin 是 Microsoft® 的主题专家（SME），并且是 ServerWatch.com、LinuxToday.com 和 IBM developerWorks 的定期投稿人，他还是 Computerworld、The Apple Blog 和其他站点的正式博客。您可以通过他的 Web 站点 : [http://www.mcslp.com](http://www.mcslp.com/) 与他联系。

转自：http://www.ibm.com/developerworks/cn/opensource/os-memcached
本文是使用 [B3log Solo](http://b3log-solo.googlecode.com/) 从 [简约设计の艺术](http://88250.b3log.org/) 进行同步发布的

原文地址：[http://88250.b3log.org/articles/2010/12/21/1292901251292.html](http://88250.b3log.org/articles/2010/12/21/1292901251292.html)

分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")
* 上一篇：[腾讯对外发布微博开放平台 API](http://blog.csdn.net/dl88250/article/details/6081379)
* 下一篇：[谁更胜一筹：技术解析 Google App Engine 和 Amazon EC2](http://blog.csdn.net/dl88250/article/details/6091854)
查看评论[]()

4楼 [fang00y](http://blog.csdn.net/fang00y) 2011-01-19 17:04发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/fang00y)[e03][e03][e03]3楼 [Rain](http://blog.csdn.net/yuanyuan110_l) 2010-12-23 20:24发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/yuanyuan110_l)[e01]打算在现在的产品里用用2楼 [xjbx](http://blog.csdn.net/xjbx) 2010-12-23 15:45发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/xjbx)[e03]1楼 [ajsfo](http://blog.csdn.net/ajsfo) 2010-12-23 10:10发表 [[回复]]( "回复")  [[引用]]( "引用") [[举报]]( "举报")[![]()](http://blog.csdn.net/ajsfo)[e01]楼主发现了绝好的东西。
您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fdl88250%2Farticle%2Fdetails%2F6088742)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()
[![TOP]()]( "回到顶部")

个人资料

[![]( "访问我的空间")](http://my.csdn.net/DL88250)
[88250](http://my.csdn.net/DL88250)

[]( "[加关注]") []( "[发私信]")

* 访问：4219340次
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
