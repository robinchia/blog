---
layout: post
title: "8种Nosql数据库系统对比"
categories: Nosql
tags: 
 - Nosql
--- 

# 8种Nosql数据库系统对比

导读：Kristóf Kovács 是一位软件架构师和咨询顾问，他最近发布了一片对比各种类型[nosql数据库](http://blog.jobbole.com/1344/ "8种Nosql数据库系统对比")的文章。

虽然SQL数据库是非常有用的工具，但经历了15年的一支独秀之后垄断即将被打破。这只是时间问题：被迫使用关系数据库，但最终发现不能适应需求的情况不胜枚举。

但是[NoSQL](http://blog.jobbole.com/1344/ "8种Nosql数据库系统对比")数据库之间的不同，远超过两 SQL数据库之间的差别。这意味着软件架构师更应该在项目开始时就选择好一个适合的 NoSQL数据库。针对这种情况，这里对[Cassandra](http://cassandra.apache.org/)、[Mongodb](http://www.mongodb.org/)、[CouchDB](http://couchdb.apache.org/)、[Redis](http://redis.io/)、 [Riak](http://www.basho.com/Riak.html)、[Membase](http://www.couchbase.org/membase)、[Neo4j](http://neo4j.org/) 和 [HBase](http://hbase.apache.org/) 进行了比较：

（编注1：NoSQL：是一项全新的数据库革命性运动，NoSQL的拥护者们提倡运用非关系型的数据存储。现今的计算机体系结构在数据存储方面要求具 备庞大的水平扩 展性，而NoSQL致力于改变这一现状。目前Google的 BigTable 和Amazon 的Dynamo使用的就是NoSQL型数据库。 参见[NoSQL词条](http://zh.wikipedia.org/zh/NoSQL)。）

 

**1. CouchDB**

* 所用语言： Erlang
* 特点：DB一致性，易于使用
* 使用许可： Apache
* 协议： HTTP/REST
* 双向数据复制，
* 持续进行或临时处理，
* 处理时带冲突检查，
* 因此，采用的是master-master复制（见编注2）
* MVCC – 写操作不阻塞读操作
* 可保存文件之前的版本
* Crash-only（可靠的）设计
* 需要不时地进行数据压缩
* 视图：嵌入式 映射/减少
* 格式化视图：列表显示
* 支持进行服务器端文档验证
* 支持认证
* 根据变化实时更新
* 支持附件处理
* 因此， CouchApps（独立的 js应用程序）
* 需要 jQuery程序库

 

**最佳应用场景：**适用于数据变化较少，执行预定义查询，进行数据统计的应用程序。适用于需要提供数据版本支持的应用程序。

**例如：** CRM、CMS系统。 master-master复制对于多站点部署是非常有用的。

（编注2：master-master复制：是一种数据库同步方法，允许数据在一组计算机之间共享数据，并且可以通过小组中任意成员在组内进行数据更新。）

 

**2. Redis**

* 所用语言：C/C++
* 特点：运行异常快
* 使用许可： BSD
* 协议：类 Telnet
* 有硬盘存储支持的内存数据库，
* 但自2.0版本以后可以将数据交换到硬盘（注意， 2.4以后版本不支持该特性！）
* Master-slave复制（见编注3）
* 虽然采用简单数据或以键值索引的哈希表，但也支持复杂操作，例如 ZREVRANGEBYSCORE。
* INCR & co （适合计算极限值或统计数据）
* 支持 sets（同时也支持 union/diff/inter）
* 支持列表（同时也支持队列；阻塞式 pop操作）
* 支持哈希表（带有多个域的对象）
* 支持排序 sets（高得分表，适用于范围查询）
* Redis支持事务
* 支持将数据设置成过期数据（类似快速缓冲区设计）
* Pub/Sub允许用户实现消息机制

 

**最佳应用场景：**适用于数据变化快且数据库大小可遇见（适合内存容量）的应用程序。

**例如：**股票价格、数据分析、实时数据搜集、实时通讯。

（编注3：Master-slave复制：如果同一时刻只有一台服务器处理所有的复制请求，这被称为 Master-slave复制，通常应用在需要提供高可用性的服务器集群。）

 

**3. MongoDB**

* 所用语言：C++
* 特点：保留了SQL一些友好的特性（查询，索引）。
* 使用许可： AGPL（发起者： Apache）
* 协议： Custom, binary（ BSON）
* Master/slave复制（支持自动错误恢复，使用 sets 复制）
* 内建分片机制
* 支持 javascript表达式查询
* 可在服务器端执行任意的 javascript函数
* update-in-place支持比CouchDB更好
* 在数据存储时采用内存到文件映射
* 对性能的关注超过对功能的要求
* 建议最好打开日志功能（参数 –journal）
* 在32位操作系统上，数据库大小限制在约2.5Gb
* 空数据库大约占 192Mb
* 采用 GridFS存储大数据或元数据（不是真正的文件系统）

 

**最佳应用场景：**适用于需要动态查询支持；需要使用索引而不是 map/reduce功能；需要对大数据库有性能要求；需要使用 CouchDB但因为数据改变太频繁而占满内存的应用程序。

**例如：**你本打算采用 MySQL或 PostgreSQL，但因为它们本身自带的预定义栏让你望而却步。

 

**4. Riak**

* 所用语言：Erlang和C，以及一些Javascript
* 特点：具备容错能力
* 使用许可： Apache
* 协议： HTTP/REST或者 custom binary
* 可调节的分发及复制(N, R, W)
* 用 JavaScript or Erlang在操作前或操作后进行验证和安全支持。
* 使用JavaScript或Erlang进行 Map/reduce
* 连接及连接遍历：可作为图形数据库使用
* 索引：输入元数据进行搜索（1.0版本即将支持）
* 大数据对象支持（ Luwak）
* 提供“开源”和“企业”两个版本
* 全文本搜索，索引，通过 Riak搜索服务器查询（ beta版）
* 支持Masterless多站点复制及商业许可的 SNMP监控

 

**最佳应用场景：**适用于想使用类似 Cassandra（类似Dynamo）数据库但无法处理 bloat及复杂性的情况。适用于你打算做多站点复制，但又需要对单个站点的扩展性，可用性及出错处理有要求的情况。

例如：销售数据搜集，工厂控制系统；对宕机时间有严格要求；可以作为易于更新的 web服务器使用。

**5. Membase**

* 所用语言： Erlang和C
* 特点：兼容 Memcache，但同时兼具持久化和支持集群
* 使用许可： Apache 2.0
* 协议：分布式缓存及扩展
* 非常快速（200k+/秒），通过键值索引数据
* 可持久化存储到硬盘
* 所有节点都是唯一的（ master-master复制）
* 在内存中同样支持类似分布式缓存的缓存单元
* 写数据时通过去除重复数据来减少 IO
* 提供非常好的集群管理 web界面
* 更新软件时软无需停止数据库服务
* 支持连接池和多路复用的连接代理

 

**最佳应用场景：**适用于需要低延迟数据访问，高并发支持以及高可用性的应用程序

例如：低延迟数据访问比如以广告为目标的应用，高并发的 web 应用比如网络游戏（例如 Zynga）

 

**6. Neo4j**

* 所用语言： Java
* 特点：基于关系的图形数据库
* 使用许可： GPL，其中一些特性使用 AGPL/商业许可
* 协议： HTTP/REST（或嵌入在 Java中）
* 可独立使用或嵌入到 Java应用程序
* 图形的节点和边都可以带有元数据
* 很好的自带web管理功能
* 使用多种算法支持路径搜索
* 使用键值和关系进行索引
* 为读操作进行优化
* 支持事务（用 Java api）
* 使用 Gremlin图形遍历语言
* 支持 Groovy脚本
* 支持在线备份，高级监控及高可靠性支持使用 AGPL/商业许可

 

**最佳应用场景：**适用于图形一类数据。这是 Neo4j与其他nosql数据库的最显著区别

例如：社会关系，公共交通网络，地图及网络拓谱

 

**7. Cassandra**

* 所用语言： Java
* 特点：对大型表格和 Dynamo支持得最好
* 使用许可： Apache
* 协议： Custom, binary (节约型)
* 可调节的分发及复制(N, R, W)
* 支持以某个范围的键值通过列查询
* 类似大表格的功能：列，某个特性的列集合
* 写操作比读操作更快
* 基于 Apache分布式平台尽可能地 Map/reduce
* 我承认对 Cassandra有偏见，一部分是因为它本身的臃肿和复杂性，也因为 Java的问题（配置，出现异常，等等）

 

**最佳应用场景：**当使用写操作多过读操作（记录日志）如果每个系统组建都必须用 Java编写（没有人因为选用 Apache的软件被解雇）

例如：银行业，金融业（虽然对于金融交易不是必须的，但这些产业对数据库的要求会比它们更大）写比读更快，所以一个自然的特性就是实时数据分析

 

**8. HBase**

（配合 ghshephard使用）

* 所用语言： Java
* 特点：支持数十亿行X上百万列
* 使用许可： Apache
* 协议：HTTP/REST （支持 [Thrift](http://www.jobbole.com/entry.php/73)，见编注4）
* 在 BigTable之后建模
* 采用分布式架构 Map/reduce
* 对实时查询进行优化
* 高性能 Thrift网关
* 通过在server端扫描及过滤实现对查询操作预判
* 支持 XML, Protobuf, 和binary的HTTP
* Cascading, hive, and pig source and sink modules
* 基于 Jruby（ JIRB）的shell
* 对配置改变和较小的升级都会重新回滚
* 不会出现单点故障
* 堪比MySQL的随机访问性能

 

**最佳应用场景：**适用于偏好BigTable:)并且需要对大数据进行随机、实时访问的场合。

例如： Facebook消息数据库（更多通用的用例即将出现）

编注4：Thrift 是一种接口定义语言，为多种其他语言提供定义和创建服务，[由Facebook开发并开源](http://blog.jobbole.com/73/)。

当然，所有的系统都不只具有上面列出的这些特性。这里我仅仅根据自己的观点列出一些我认为的重要特性。与此同时，技术进步是飞速的，所以上述的内容肯定需要不断更新。我会尽我所能地更新这个列表。
来源： <[http://blog.jobbole.com/1344/](http://blog.jobbole.com/1344/)> 

# **Cassandra** vs **MongoDB** vs **CouchDB** vs **Redis** vs **Riak** vs**HBase** vs **Couchbase** vs **Neo4j** vs **Hypertable** vs**ElasticSearch** vs **Accumulo** vs **VoltDB** vs **Scalaris**comparison
 

(Yes it's a long title, since people kept asking me to write about this and that too :) I do when it has a point.)

While SQL databases are insanely useful tools, their monopoly in the last decades is coming to an end. And it's just time: I can't even count the things that were forced into relational databases, but never really fitted them. (That being said, relational databases will always be the best for the stuff that has relations.)

But, the differences between NoSQL databases are much bigger than ever was between one SQL database and another. This means that it is a bigger responsibility on [software architects](http://kkovacs.eu/resume) to choose the appropriate one for a project right at the beginning.

In this light, here is a comparison of [Cassandra](http://cassandra.apache.org/), [Mongodb](http://www.mongodb.org/), [CouchDB](http://couchdb.apache.org/), [Redis](http://redis.io/), [Riak](http://basho.com/riak/), [Couchbase (ex-Membase)](http://www.couchbase.org/membase), [Hypertable](http://hypertable.org/), [ElasticSearch](http://www.elasticsearch.org/), [Accumulo](http://accumulo.apache.org/), [VoltDB](http://voltdb.com/), [Kyoto Tycoon](http://fallabs.com/kyototycoon/), [Scalaris](https://code.google.com/p/scalaris/), [Neo4j](http://neo4j.org/) and [HBase](http://hbase.apache.org/):

### The most popular ones

## MongoDB (2.2)

* **Written in:** C++
* **Main point:** Retains some friendly properties of SQL. (Query, index)
* **License:** AGPL (Drivers: Apache)
* **Protocol:** Custom, binary (BSON)
* Master/slave replication (auto failover with replica sets)
* Sharding built-in
* Queries are javascript expressions
* Run arbitrary javascript functions server-side
* Better update-in-place than CouchDB
* Uses memory mapped files for data storage
* Performance over features
* Journaling (with --journal) is best turned on
* On 32bit systems, limited to ~2.5Gb
* An empty database takes up 192Mb
* GridFS to store big data + metadata (not actually an FS)
* Has geospatial indexing
* Data center aware

**Best used:** If you need dynamic queries. If you prefer to define indexes, not map/reduce functions. If you need good performance on a big DB. If you wanted CouchDB, but your data changes too much, filling up disks.

**For example:** For most things that you would do with MySQL or PostgreSQL, but having predefined columns really holds you back.

## Riak (V1.2)

* **Written in:** Erlang & C, some JavaScript
* **Main point:** Fault tolerance
* **License:** Apache
* **Protocol:** HTTP/REST or custom binary
* Stores blobs
* Tunable trade-offs for distribution and replication
* Pre- and post-commit hooks in JavaScript or Erlang, for validation and security.
* Map/reduce in JavaScript or Erlang
* Links & link walking: use it as a graph database
* Secondary indices: but only one at once
* Large object support (Luwak)
* Comes in "open source" and "enterprise" editions
* Full-text search, indexing, querying with Riak Search
* In the process of migrating the storing backend from "Bitcask" to Google's "LevelDB"
* Masterless multi-site replication replication and SNMP monitoring are commercially licensed

**Best used:** If you want something Dynamo-like data storage, but no way you're gonna deal with the bloat and complexity. If you need very good single-site scalability, availability and fault-tolerance, but you're ready to pay for multi-site replication.

**For example:** Point-of-sales data collection. Factory control systems. Places where even seconds of downtime hurt. Could be used as a well-update-able web server.
## CouchDB (V1.2)

* **Written in:** Erlang
* **Main point:** DB consistency, ease of use
* **License:** Apache
* **Protocol:** HTTP/REST
* Bi-directional (!) replication,
* continuous or ad-hoc,
* with conflict detection,
* thus, master-master replication. (!)
* MVCC - write operations do not block reads
* Previous versions of documents are available
* Crash-only (reliable) design
* Needs compacting from time to time
* Views: embedded map/reduce
* Formatting views: lists & shows
* Server-side document validation possible
* Authentication possible
* Real-time updates via '_changes' (!)
* Attachment handling
* thus, [CouchApps](http://couchapp.org/) (standalone js apps)

**Best used:** For accumulating, occasionally changing data, on which pre-defined queries are to be run. Places where versioning is important.

**For example:** CRM, CMS systems. Master-master replication is an especially interesting feature, allowing easy multi-site deployments.

## Redis (V2.4)

* **Written in:** C/C++
* **Main point:** Blazing fast
* **License:** BSD
* **Protocol:** Telnet-like
* Disk-backed in-memory database,
* Currently without disk-swap (VM and Diskstore were abandoned)
* Master-slave replication
* Simple values or hash tables by keys,
* but [complex operations](http://redis.io/commands) like ZREVRANGEBYSCORE.
* INCR & co (good for rate limiting or statistics)
* Has sets (also union/diff/inter)
* Has lists (also a queue; blocking pop)
* Has hashes (objects of multiple fields)
* Sorted sets (high score table, good for range queries)
* Redis has transactions (!)
* Values can be set to expire (as in a cache)
* Pub/Sub lets one implement messaging (!)

**Best used:** For rapidly changing data with a foreseeable database size (should fit mostly in memory).

**For example:** Stock prices. Analytics. Real-time data collection. Real-time communication. And wherever you used memcached before.

### Clones of Google's Bigtable

## HBase (V0.92.0)

* **Written in:** Java
* **Main point:** Billions of rows X millions of columns
* **License:** Apache
* **Protocol:** HTTP/REST (also Thrift)
* Modeled after Google's BigTable
* Uses Hadoop's HDFS as storage
* Map/reduce with Hadoop
* Query predicate push down via server side scan and get filters
* Optimizations for real time queries
* A high performance Thrift gateway
* HTTP supports XML, Protobuf, and binary
* Jruby-based (JIRB) shell
* Rolling restart for configuration changes and minor upgrades
* Random access performance is like MySQL
* A cluster consists of several different types of nodes

**Best used:** Hadoop is probably still the best way to run Map/Reduce jobs on huge datasets. Best if you use the Hadoop/HDFS stack already.

**For example:** Search engines. Analysing log data. Any place where scanning huge, two-dimensional join-less tables are a requirement.

## Cassandra (1.2)

* **Written in:** Java
* **Main point:** Best of BigTable and Dynamo
* **License:** Apache
* **Protocol:** Thrift & custom binary CQL3
* Tunable trade-offs for distribution and replication (N, R, W)
* Querying by column, range of keys (Requires indices on anything that you want to search on)
* BigTable-like features: columns, column families
* Can be used as a distributed hash-table, with an "SQL-like" language, CQL (but no JOIN!)
* Data can have expiration (set on INSERT)
* Writes can be much faster than reads (when reads are disk-bound)
* Map/reduce possible with Apache Hadoop
* All nodes are similar, as opposed to Hadoop/HBase
* Cross-datacenter replication

**Best used:** When you write more than you read (logging). If every component of the system must be in Java. ("No one gets fired for choosing Apache's stuff.")

**For example:** Banking, financial industry (though not necessarily for financial transactions, but these industries are much bigger than that.) Writes are faster than reads, so one natural niche is data analysis.
## Hypertable (0.9.6.5)

* **Written in:** C++
* **Main point:** A faster, smaller HBase
* **License:** GPL 2.0
* **Protocol:** Thrift, C++ library, or HQL shell
* Implements Google's BigTable design
* Run on Hadoop's HDFS
* Uses its own, "SQL-like" language, HQL
* Can search by key, by cell, or for values in column families.
* Search can be limited to key/column ranges.
* Sponsored by Baidu
* Retains the last N historical values
* Tables are in namespaces
* Map/reduce with Hadoop

**Best used:** If you need a better HBase.

**For example:** Same as HBase, since it's basically a replacement: Search engines. Analysing log data. Any place where scanning huge, two-dimensional join-less tables are a requirement.

## Accumulo (1.4)

* **Written in:** Java and C++
* **Main point:** A BigTable with Cell-level security
* **License:** Apache
* **Protocol:** Thrift
* Another BigTable clone, also runs of top of Hadoop
* Cell-level security
* Bigger rows than memory are allowed
* Keeps a memory map outside Java, in C++ STL
* Map/reduce using Hadoop's facitlities (ZooKeeper & co)
* Some server-side programming

**Best used:** If you need a different HBase.

**For example:** Same as HBase, since it's basically a replacement: Search engines. Analysing log data. Any place where scanning huge, two-dimensional join-less tables are a requirement.

### Special-purpose

## Neo4j (V1.5M02)

* **Written in:** Java
* **Main point:** Graph database - connected data
* **License:** GPL, some features AGPL/commercial
* **Protocol:** HTTP/REST (or embedding in Java)
* Standalone, or embeddable into Java applications
* Full ACID conformity (including durable data)
* Both nodes and relationships can have metadata
* Integrated pattern-matching-based query language ("Cypher")
* Also the "Gremlin" graph traversal language can be used
* Indexing of nodes and relationships
* Nice self-contained web admin
* Advanced path-finding with multiple algorithms
* Indexing of keys and relationships
* Optimized for reads
* Has transactions (in the Java API)
* Scriptable in Groovy
* Online backup, advanced monitoring and High Availability is AGPL/commercial licensed

**Best used:** For graph-style, rich or complex, interconnected data. Neo4j is quite different from the others in this sense.

**For example:** For searching routes in social relations, public transport links, road maps, or network topologies.

## ElasticSearch (0.20.1)

* **Written in:** Java
* **Main point:** Advanced Search
* **License:** Apache
* **Protocol:** JSON over HTTP (Plugins: Thrift, memcached)
* Stores JSON documents
* Has versioning
* Parent and children documents
* Documents can time out
* Very versatile and sophisticated querying, scriptable
* Write consistency: one, quorum or all
* Sorting by score (!)
* Geo distance sorting
* Fuzzy searches (approximate date, etc) (!)
* Asynchronous replication
* Atomic, scripted updates (good for counters, etc)
* Can maintain automatic "stats groups" (good for debugging)
* Still depends very much on only one developer (kimchy).

**Best used:** When you have objects with (flexible) fields, and you need "advanced search" functionality.

**For example:** A dating service that handles age difference, geographic location, tastes and dislikes, etc. Or a leaderboard system that depends on many variables.

### The "long tail"
(Not widely known, but definitely worthy ones)

## Couchbase (ex-Membase) (2.0)

* **Written in:** Erlang & C
* **Main point:** Memcache compatible, but with persistence and clustering
* **License:** Apache
* **Protocol:** memcached + extensions
* Very fast (200k+/sec) access of data by key
* Persistence to disk
* All nodes are identical (master-master replication)
* Provides memcached-style in-memory caching buckets, too
* Write de-duplication to reduce IO
* Friendly cluster-management web GUI
* Connection proxy for connection pooling and multiplexing (Moxi)
* Incremental map/reduce
* Cross-datacenter replication

**Best used:** Any application where low-latency data access, high concurrency support and high availability is a requirement.

**For example:** Low-latency use-cases like ad targeting or highly-concurrent web apps like online gaming (e.g. Zynga).

## Scalaris (0.5)

* **Written in:** Erlang
* **Main point:** Distributed P2P key-value store
* **License:** Apache
* **Protocol:** Proprietary & JSON-RPC
* In-memory (disk when using Tokyo Cabinet as a backend)
* Uses YAWS as a web server
* Has transactions (an adapted Paxos commit)
* Consistent, distributed write operations
* From CAP, values Consistency over Availability (in case of network partitioning, only the bigger partition works)

**Best used:** If you like Erlang and wanted to use Mnesia or DETS or ETS, but you need something that is accessible from more languages (and scales much better than ETS or DETS).

**For example:** In an Erlang-based system when you want to give access to the DB to Python, Ruby or Java programmers.

## VoltDB (2.8.4.1)

* **Written in:** Java
* **Main point:** Fast transactions and repidly changing data
* **License:** GPL 3
* **Protocol:** Proprietary
* In-memory **relational** database.
* Can export data into Hadoop
* Supports ANSI SQL
* Stored procedures in Java
* Cross-datacenter replication

**Best used:** Where you need to act fast on massive amounts of incoming data.

**For example:** Point-of-sales data analysis. Factory control systems.

## Kyoto Tycoon (0.9.56)

* **Written in:** C++
* **Main point:** A lightweight network DBM
* **License:** GPL
* **Protocol:** HTTP (TSV-RPC or REST)
* Based on Kyoto Cabinet, Tokyo Cabinet's successor
* Multitudes of storage backends: Hash, Tree, Dir, etc (everything from Kyoto Cabinet)
* Kyoto Cabinet can do 1M+ insert/select operations per sec (but Tycoon does less because of overhead)
* Lua on the server side
* Language bindings for C, Java, Python, Ruby, Perl, Lua, etc
* Uses the "visitor" pattern
* Hot backup, asynchronous replication
* background snapshot of in-memory databases
* Auto expiration (can be used as a cache server)

**Best used:** When you want to choose the backend storage algorithm engine very precisely. When speed is of the essence.

**For example:** Caching server. Stock prices. Analytics. Real-time data collection. Real-time communication. And wherever you used memcached before.

Of course, all these systems have much more features than what's listed here. I only wanted to list the key points that I base my decisions on. Also, development of all are very fast, so things are bound to change.
来源： <[http://kkovacs.eu/cassandra-vs-mongodb-vs-couchdb-vs-redis](http://kkovacs.eu/cassandra-vs-mongodb-vs-couchdb-vs-redis)> 
 
