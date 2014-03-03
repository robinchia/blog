---
layout: post
title: "squid 优化 - Java 小试 - ITeye技术网站"
categories: 服务器
tags: 
 - 服务器
 - Squid
--- 

# squid 优化 - Java 小试 - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼](http://sech.iteye.com/blog/1154909#)

[招聘](http://job.iteye.com/iteye) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://sech.iteye.com/login "登录") [登录](http://sech.iteye.com/login) [注册](http://sech.iteye.com/signup)

# [Java 小试](http://sech.iteye.com/)

* [**博客**](http://sech.iteye.com/)
* [微博](http://sech.iteye.com/weibo)
* [相册](http://sech.iteye.com/album)
* [收藏](http://sech.iteye.com/link)
* [留言](http://sech.iteye.com/blog/guest_book)
* [关于我](http://sech.iteye.com/blog/profile)

### [squid 优化]() **

**博客分类：**
* [squid](http://sech.iteye.com/category/170576)
 

 
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. maximum_object_size 是 能cache最大的文件大小。对应wmv，rm文件，建议设置为32768 kB  
1. maximum_object_size_in_memory 是在内存中cache的最大文件大小。  
1. cache_mem 是SQUID可用到的最大内存。经实践，4G内存的服务器用2G；超过2G导致SQUID运行不稳  
1.   
1. 首先要分析SQUID所cache内容：  
1.   
1. 运行  
1.   
1. squidclient -p 80 cache_object://localhost/info  
1.   
1. 能看到如下内容：  
1.   
1. Storage Swap size: 7549104 KB  
1. Storage Mem size: 418804 KB  
1. Mean Object Size: 160.46 KB  
1.   
1. Mean Object Size是平均内容大小，一般要把maximum_object_size_in_memory设置成离它最近的128的倍数。在这个例子中maximum_object_size_in_memory 的值应该是256kB。  
1.   
1. cache_mem 一般设置成服务器内存的一半或更多，只要运行过程中LINUX没有使用SWAP就可以。  
1.   
1. 再就是按业务分SQUID。  
1. 比如某个论坛，用户能上载图片和视频；当然我们要把上载的图片、视频放在单独的域名上，比如img.example.com, video.example.com；这两个域名只提供静态文件服务。  
1.   
1. 根据统计，图片的平均大小在100KB，视频的平均大小在4M，差别是很大，应该建两个squid分别作图片和视频的CACHE。图片SQUID的 maximum_object_size_in_memory 设置为256KB，视频的SQUID的maximum_object_size_in_memory设置为8196KB。  
maximum_object_size 是 能cache最大的文件大小。对应wmv，rm文件，建议设置为32768 kB maximum_object_size_in_memory 是在内存中cache的最大文件大小。 cache_mem 是SQUID可用到的最大内存。经实践，4G内存的服务器用2G；超过2G导致SQUID运行不稳 首先要分析SQUID所cache内容： 运行 squidclient -p 80 cache_object://localhost/info 能看到如下内容： Storage Swap size: 7549104 KB Storage Mem size: 418804 KB Mean Object Size: 160.46 KB Mean Object Size是平均内容大小，一般要把maximum_object_size_in_memory设置成离它最近的128的倍数。在这个例子中maximum_object_size_in_memory 的值应该是256kB。 cache_mem 一般设置成服务器内存的一半或更多，只要运行过程中LINUX没有使用SWAP就可以。 再就是按业务分SQUID。 比如某个论坛，用户能上载图片和视频；当然我们要把上载的图片、视频放在单独的域名上，比如img.example.com, video.example.com；这两个域名只提供静态文件服务。 根据统计，图片的平均大小在100KB，视频的平均大小在4M，差别是很大，应该建两个squid分别作图片和视频的CACHE。图片SQUID的 maximum_object_size_in_memory 设置为256KB，视频的SQUID的maximum_object_size_in_memory设置为8196KB。

 

 
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. squid 有内存缓存和磁盘缓存两级缓存。通常来说，只要是专门给 squid 用的机器，内存缓存都建议开得比较大，大内存缓存总是有好处的嘛。但是注意不要使得系统开始吃 swap，像Linux这样一开始吃 swap 性能就下降比较严重的系统尤其要注意。  
1.   
1. 这个程度需要自己试验确定。通常 1G 内存的Linux机器用来跑 squid，内存缓存可以开到 512M。  
1.   
1. 有些libc比较差的平台，例如比较老的 freebsd 系统，其 malloc 函数的质量不高，可能会造成比较多的内存碎片，导致squid 运行一段时间以后分配不出来内存挂掉。这时候推荐在编译时候使用 dlmalloc package。即使如此，仍然要再缩小 squid 的内存缓存，以防不幸发生。  
1.   
1. 磁盘缓存的情况比较复杂，squid 有 ufs, aufs, coss, diskd, null 五种存储后端。其中 ufs, aufs, diskd 都是在文件系统上面保存很多小文件, coss 是 squid 自己实现了一个简单的文件系统，可以使用一个大文件或者一个磁盘设备来存储。 null 则是给不想要磁盘缓存的情况准备的。coss 看起来好像比较拽，但是以前试验并不足够稳定，因此并不推荐使用。剩下的三种存储方式，具体选择哪种需要根据操作系统的特性来进行。  
1.   
1. ufs 是最传统的存储方式。我们知道 squid 是一个单进程的程序，它使用 ufs 存储后端时，直接在进程里面读写文件。这是一种很简单的方式，缺点是当读写磁盘被阻塞的时候，squid 不能够处理请求，会造成服务质量波动比较大。  
1.   
1. 因此出现了 aufs 和 diskd 两种存储后端。原理都是 squid 主服务循环不负责读写文件，而是通过消息队列或者 tcp/pipe 连接将数据传送给其他的线程(aufs)/进程(diskd)，然后其他线程/进程进行读写。很显然，这两种存储方式有一定的通信开销，因此不一定就比 ufs 好，需要具体问题具体分析。  
1. 前面说到， ufs/aufs/diskd 都是在文件系统上面存储很多小文件，因此文件系统本身的特性严重影响了 squid 缓存的性能。对于 Linux，强烈推荐用 reiserfs 等适合处理小文件的文件系统，bsd 则至少要打开 softupdate 以及 dirhash 等一切对很多小文件有好处的选项。在比较新的系统上面，reiserfs 等文件系统的性能已经足够优越，通常 ufs 就已经可以应付需要。对于一些老系统，使用 aufs 或者 diskd 是比较好的选择，如果系统的线程库比较好(如Linux,Solaris)，那么使用 aufs，否则 diskd。也有一些例外情况，比如多 cpu 的 Linux 2.6 系统，线程库很优秀，虽然 ufs 本身已经比较快了，但是 squid 单进程无法利用另外的 cpu，不如使用 aufs，让另外的 cpu 也可以起到一些作用，aufs 在编译的时候可以选择使用几个读写线程。这个个人觉得稍微超过 cpu 个数就可以了，但是并没有实际测试过。  
1.   
1. 磁盘缓存开多大? 这个问题没有固定答案。需要经过试验来确定，一般来说开大一些没有太大问题，只要你的硬盘足够。  
squid 有内存缓存和磁盘缓存两级缓存。通常来说，只要是专门给 squid 用的机器，内存缓存都建议开得比较大，大内存缓存总是有好处的嘛。但是注意不要使得系统开始吃 swap，像Linux这样一开始吃 swap 性能就下降比较严重的系统尤其要注意。 这个程度需要自己试验确定。通常 1G 内存的Linux机器用来跑 squid，内存缓存可以开到 512M。 有些libc比较差的平台，例如比较老的 freebsd 系统，其 malloc 函数的质量不高，可能会造成比较多的内存碎片，导致squid 运行一段时间以后分配不出来内存挂掉。这时候推荐在编译时候使用 dlmalloc package。即使如此，仍然要再缩小 squid 的内存缓存，以防不幸发生。 磁盘缓存的情况比较复杂，squid 有 ufs, aufs, coss, diskd, null 五种存储后端。其中 ufs, aufs, diskd 都是在文件系统上面保存很多小文件, coss 是 squid 自己实现了一个简单的文件系统，可以使用一个大文件或者一个磁盘设备来存储。 null 则是给不想要磁盘缓存的情况准备的。coss 看起来好像比较拽，但是以前试验并不足够稳定，因此并不推荐使用。剩下的三种存储方式，具体选择哪种需要根据操作系统的特性来进行。 ufs 是最传统的存储方式。我们知道 squid 是一个单进程的程序，它使用 ufs 存储后端时，直接在进程里面读写文件。这是一种很简单的方式，缺点是当读写磁盘被阻塞的时候，squid 不能够处理请求，会造成服务质量波动比较大。 因此出现了 aufs 和 diskd 两种存储后端。原理都是 squid 主服务循环不负责读写文件，而是通过消息队列或者 tcp/pipe 连接将数据传送给其他的线程(aufs)/进程(diskd)，然后其他线程/进程进行读写。很显然，这两种存储方式有一定的通信开销，因此不一定就比 ufs 好，需要具体问题具体分析。 前面说到， ufs/aufs/diskd 都是在文件系统上面存储很多小文件，因此文件系统本身的特性严重影响了 squid 缓存的性能。对于 Linux，强烈推荐用 reiserfs 等适合处理小文件的文件系统，bsd 则至少要打开 softupdate 以及 dirhash 等一切对很多小文件有好处的选项。在比较新的系统上面，reiserfs 等文件系统的性能已经足够优越，通常 ufs 就已经可以应付需要。对于一些老系统，使用 aufs 或者 diskd 是比较好的选择，如果系统的线程库比较好(如Linux,Solaris)，那么使用 aufs，否则 diskd。也有一些例外情况，比如多 cpu 的 Linux 2.6 系统，线程库很优秀，虽然 ufs 本身已经比较快了，但是 squid 单进程无法利用另外的 cpu，不如使用 aufs，让另外的 cpu 也可以起到一些作用，aufs 在编译的时候可以选择使用几个读写线程。这个个人觉得稍微超过 cpu 个数就可以了，但是并没有实际测试过。 磁盘缓存开多大? 这个问题没有固定答案。需要经过试验来确定，一般来说开大一些没有太大问题，只要你的硬盘足够。  

 

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. A. 数据反馈  
1.   
1. 康神教导我们：反馈是做一切事情的基础，优化也不例外。那么具体看一些什么反馈数据呢？很遗憾，这个问题没有固定的答案，基本上需要具体问题具体分析。具体来说，用 cacti 之类的看 squid snmp 数据是第一步。主要的数据有：  
1.   
1. * BHR (Byte Hit Rate)/RHR (Request Hit Rate)，这两个分别表示有多少数据量/请求数是被  
1. squid hit cache 的。要特别注意的是，squid hit cache 并不等于 squid 不往主服务器发请求。具体情况在  
1. squid 2.5/2.6 里面参看 isTcpHit() 函数。举个例子，如果 RHR 显示 90%，那么可能有 20% 的请求 squid 还是往主服务器发送了请求询问它过期的 cache 内容是否有变化的，只不过主服务器的回应是没有变化。主服务器的判断一般需要一次数据库查询或者文件系统的 stat 调用。高负荷服务器到最后如果瓶颈在主服务器磁盘 I/O，这些貌似 HIT 的请求也会对主服务器造成一定量的冲击。单纯盲目追求高 BHR/RHR 是不可靠的。  
1.   
1. * Service Time，这个是各类请求的回应时间，但是这个时间受到外部网络速度的影响，所以也不一定代表真实的性能。另外，如果出现 Miss Service Time 比别的几个 Service Time 指标高出很多，表示去主服务器的请求耗时比较长，一般是代表瓶颈在主服务器那里，但这也不是绝对的。如果 squid 服务器磁盘 I/O  
1. 性能跟不上或者 CPU 不够强劲（squid 2 核心是可怜的单进程），那么也可能是 squid 本身的调度出了问题。  
1.   
1. * Storage，在特定情况下可以估算 squid 每天缓存多少东西，在配置缓存大小等问题的时候会有帮助。  
1.   
1. 总的来说，squid snmp 数据是需要根据具体情况分析的，而且 squid snmp 的数据也不多，有时候往往需要配合别的监控来综合分析问题，比方网络出入流量，服务器的各类性能分析，主服务器和 squid access  
1. log。有一次我在某站 squid 调整了一个参数，结果那天 squid 的反应奇好，BHR 更是上了前所未有的98%。但是后来反复推敲发现这个参数并没有作用，结合各类 log 分析发现是因为那天 frjj 贴了新照片，而且被盗链，导致巨大比例的 frjj 照片的请求被 squid 有效的缓存，squid 网络出流量激增也支持这个结论。说了半天，中心意思是高负荷服务器的 squid 优化是一个长期而艰巨的任务，某些参数的优化需要几天的数据才能得出有意义的结论，所以需要有足够的耐心和针对实际情况的系统化的优化步骤。  
1.   
1. B. 缓存策略  
1.   
1. 康神教导我们：一般来说，（缓存策略）如果后端不是配置很麻烦，建议还是在后端做，前端的配置修改大多数都是违背 http 协议的，如果出现问题，也比较难排查。  
1.   
1. HTTP 缓存协议比较权威的可以参考 RF2616 第十三章，特别是 13.2 和 13.3 节。具体实现可以参考比方 Firefox 代码 nsHttpResponseHead.cpp 的 ComputeCurrentAge() 和 ComputeFreshnessLifetime() 函数看看各类情况的处理方式。  
1.   
1. mod_expires 的配置就需要深刻理解这些基本概念，否则可能反而会增加请求数。如果没有特别的理由，静态文件的过期时间一般是设置为 access time 加上一定量的时间。这个一定量的时间由具体情况决定。比如网站建设初期，各类静态文件可能需要比较短的过期时间以方便网站更新；而一旦美工敲定图片，图片的过期时间可以大胆的设置为几个月。在配置完成以后如果没有很大的把握也可以实际浏览一下分析请求序列看是否浏览器端和 squid 服务器都做到了有效的缓存，特别注意 cache 相关的请求和回复头，包括 squid 提供的 X-Cache 头。  
1.   
1. 另外，虽然违反 HTTP 协议的 squid 配置一般都不推荐，但是具体到细节上，这也不是绝对的原则。下面举例说说必须违反 HTTP 协议的情况。  
1.   
1. * 使用 javascript 做镜像网站测速，一般实现方式是从各个镜像站下载一个图片看哪一个最快。最理想的情况是图片在浏览器端不要缓存（以便下次准确测速），但是这个请求又完全没必要打到主服务器上。那么可以在 squid 里针对这个图片 url 配置强制缓存 refresh_pattern reload-into-ims ignore-reload。当然这个例子很土鳖，只是举个例子。  
1.   
1. * reload_into_ims （这里说的是 squid 的配置参数，不是 refresh_pattern 里面的 option）。这个参数虽然违反 HTTP 协议但是对大部分网站来说是可以设置为 on 的，只要后端服务器对 If-Modified-Since 头的判断正确并且没有潜在安全问题即可。  
1.   
1. * 浏览器 F5 刷新和 javascript 的 location.reload() 刷新可能会重新请求所有的网页内嵌元素并且可能带 no-cache 请求头，一般来说 reload_into_ims 设置成 on 已经足够保证对主服务器不造成冲击，但是如果有必要可能还是需要在 squid 配置强制缓存。  
1.   
1. * 针对土鳖客户端的优化。比如早期的 fterm 预览图片会发送 Pragma: no-cache 的请求头，这势必导致所有 fterm 预览图片的请求如数全部打在后端服务器上，所以解决方法是 squid 这里做手脚针对这类 url 配置强制缓存。一个细节问题是如果不能缓存的图片（比方有察看权限限制的）和能缓存的图片的 url 结构完全一样，那么在 squid 强制缓存这类 url 的话又会有潜在的安全问题，这里涉及到后面会讲到的网站结构优化，针对这个问题需要修改网站的代码以明确区分这两类 url。还有另外一个例子是早期的 firefox 发送 XMLHttpRequest 请求也会发送 no-cache 的头，后来的版本改了。当年这一类 ajax 请求的 url 也是需要配置强制缓存的。  
1.   
1. * 最后一个问题是，如果在特殊情况下必须同时在后端服务器发送 Expires 头，并且又在 squid 中配置这类 url 的 refresh_pattern，那么需要特别小心。比如，如果 squid 强制缓存时间比 mod_expires 配置的过期时间长，那么可能造成 squid 发送已经过期的内容，导致浏览器本来可以有效缓存的内容却需要不断的向服务器检查更新。  
1.   
1. 最后，有些后端服务器没办法配置 mod_expires。这可能是因为没有配置权限，也可能是因为后端服务器软件太土鳖，总之这样的情况下就必须用 squid 配置 refresh_pattern 了。  
1.   
1. C. 网站代码及结构优化  
1. 康神教导我们：很多 squid 优化（的文章）只限于在 squid 参数和系统参数上面的调整。但是这个实在只是细枝末节的事情，只要不是太弱智的配置导致无法缓存，squid 的性能不会有太大差距。网站优化一般来说也是属于这种类型的优化，对于主服务器负荷瓶颈在磁盘 I/O，或者网络瓶颈是大量大图片文件的情况，优化网站 html 结构可能对性能提升没有半点作用。不过即便如此，有一个为 squid 考虑的网站结构，可以使得 squid 服务器的配置比较容易，也可以比较容易的实现多 squid 业务分拆，有的时候业务分拆并不是为了性能，而是为了更好的分析问题以便进一步优化网站。下面简要说说有可能提高性能的网站代码优化。  
1.   
1. * 减少页面大小。这个问题实在是到处都有好文章，我就不详细说了。常见技巧是分离 css/js 到单独文件减少动态主页面大小同时保证静态内容有效缓存；页面 layout 设计尽量使用 div+css；有大量冗余 html 元素的部分使用 javascript 来输出。最后这个页面 javascript 化可能需要考虑搜索引擎优化 (SEO) 的问题，总的来说需要在减少流量和 SEO 之间寻找一个好的平衡点，这个只有做网站的人自己最清楚。  
1.   
1. * 减少同一份数据的不同表现形式。大量使用 ajax 的站点有时候考虑 SEO 往往要重写一套给搜索引擎看的页面，这势必导致 squid 这里要存两套页面。但是如果功力足够还是可以做到大部分页面重用。举例来说，网站可能希望用户读文章不切换页面而使用 XMLHttpRequest 载入，这个就可以在 <a href 写上文章内容的页面以便搜索引擎扒站同时也允许用户在新窗口打开这个文章，而 onclick 事件则触发 XMLHttpRequest 载入页面并分析显示内容。只要代码写的足够漂亮，这里用一个文章页面就可以实现所有的功能。  
1.   
1. * 标准化 url。这个可以算前一条的补充。写网站如果不小心，可能同一个资源会有不同的 url。比方某篇文章，从主页进去的 url 是 article?bid=3&id=50，从搜索结果进去却是 article?id=50&bid=3。这样两个页面，不但影响外部搜索引擎排名（自己和自己打架），更会影响 squid 效率，因为 squid 需要单独存这两类页面。  
1.   
1. * 网站建设初期充分考虑到将来的 squid 优化。举例来说，很多网站都在页面带用户登录信息显示，这样的页面如果不使用 javascript 技巧就完全不可以在 squid 这里 cache。而实际上，如果这些动态内容可以在 javascript 里面通过 cookie 判断出来，那么完全可以用 javascript 来写。这方面的细节工作做得越好，就有越多的页面可以被 squid 安全的缓存。当然这方面的优化有时候也只有网站运行起来才能发现，维护网站的时候多分析 log，多观察，就可以发现这些细小的可以优化的地方，水滴石穿，大量小细节的优化也可以带来可观的性能提升。  
1.   
1. D. 一些杂问题  
1.   
1. * 同步 squid 和主服务器的时钟。从原理上说即使主服务器、squid 以及浏览器端的时钟都不同步，应该也不会造成缓存策略上的问题，但是为了防止诡异问题的发生，还是配置一下 squid 和主服务器的 ntpd 为好。ntp 是一个极轻量级的协议，现在网络上 ntpd server 也遍地都是，保证服务器时钟准确到 1秒之内也可以保证别的一些程序的事务处理逻辑。  
1.   
1. * 密切注意搜索引擎的动向。有一些搜索引擎做的比较弱智，有的时候会突然发很多请求过来。搜索引擎扒站很容易扒到冷僻内容，所以即使请求量只是普通浏览用户 请求量的零头，也可能会对主服务器造成冲击。大部分搜索引擎还是比较守规矩的，甚至有些搜索引擎公司还可以与他们接触配置扒站方案。不老实的搜索引擎可以 通过 squid 或者主服务器 log 找出来，特别不老实的可能 iptables 都能发现。解决方法比如可以针对搜索引擎 user agent 判断，或者干脆 iptables 咔嚓掉。  
1.   
1. * Cache replacement policy，对大论坛站点，虽然 lru 算法占用 cpu 较低，但是 service time 可能会不如带 dynamic aging 的算法稳定。据观察，lru 算法在运行几天之后的早晨如果突然碰到大量新请求，新请求会很难进入 cache，或者进入了也很快被踢出，导致非常容易形成恶性正反馈拖垮后台服务器。但是假如每天清晨清 cache，并且保证磁盘 cache 的量稍大于每天能存下来的量，那么 lru 算法应该也不会比别的算法差（事实上什么算法都一样了）。当然这只是我的一家之言，一般来说这个问题还是需要根据 squid 服务器性能和网站具体情况多次反复试验选择最合适的算法。一般来说小规模和超大规模的站点优化这个参数可能不会有什么显著的性能提升，所以不建议耗费太多时间优化这个。  
1.   
1. * 一定时间清理 cache 并重启 squid。这个有可能只是 squid 2.5 并且是高负荷破机器上需要考虑的一个方案。某站曾经有段时间每天高峰期 Miss Service Time 都会飙升，但是主服务器却没有超负荷的现象，最后推测可能是 squid 自己调度的问题。后来每三天清理 cache 并重启 squid 似乎大大减少了这种现象。后据权威人士批复，这个可能是因为 squid cache replacement 算法过于古老，不适应高速更新的大型论坛所致。  
1.   
1. * 多域名宣传的服务器。如果网站允许有多个域名但是所有的域名都指向同一个网站，那么要注意 squid  
1. 不要配置成多域名模式，否则它会把每个域名的 cache 都分开处理，导致效率低下而且不能有效利用缓存存储空间。题外话，单个网站宣传多个域名也会影响搜索引擎排名等等，所以本质上也是不推荐这么做的。  
1.   
1. * maximum_object_size_in_memory，maximum_object_size 这两个参数的配置也是具体问题具体分析的。具体到某站上，常见的大文件就是附件了，由于附件最大允许大小是 5120 KB，所以 maximum_object_size 配置了 5123 KB 以保证即使最大的附件加上各 HTTP 头也能有效的被缓存。最早这个参数是配置成 5120 KB 的，然后曾经有一次发生 BHR 骤降的情况，后来分析发现是有人贴了 5120 KB  
1. RAR 分卷压缩的李宇春同学的视频，而且恰好又有无数玉米下载，大量这类请求因为超容所以都压到了主服务器上。另外 maximum_object_size_in_memory 的配置需要考虑网站具体情况和 squid 服务器的性能，这也需要实际试验出来。  
1.   
1. 最后废话一句，现在硬件降价很快，给机群升级扩容提升硬件性能往往比找个猪头优化 squid 配置更有效果，而且见效也快。所以在 squid 大配置没有问题的前提下，有钱的话应该首选硬件优化方案而不是软件细节优化。  
A. 数据反馈 康神教导我们：反馈是做一切事情的基础，优化也不例外。那么具体看一些什么反馈数据呢？很遗憾，这个问题没有固定的答案，基本上需要具体问题具体分析。具体来说，用 cacti 之类的看 squid snmp 数据是第一步。主要的数据有： * BHR (Byte Hit Rate)/RHR (Request Hit Rate)，这两个分别表示有多少数据量/请求数是被 squid hit cache 的。要特别注意的是，squid hit cache 并不等于 squid 不往主服务器发请求。具体情况在 squid 2.5/2.6 里面参看 isTcpHit() 函数。举个例子，如果 RHR 显示 90%，那么可能有 20% 的请求 squid 还是往主服务器发送了请求询问它过期的 cache 内容是否有变化的，只不过主服务器的回应是没有变化。主服务器的判断一般需要一次数据库查询或者文件系统的 stat 调用。高负荷服务器到最后如果瓶颈在主服务器磁盘 I/O，这些貌似 HIT 的请求也会对主服务器造成一定量的冲击。单纯盲目追求高 BHR/RHR 是不可靠的。 * Service Time，这个是各类请求的回应时间，但是这个时间受到外部网络速度的影响，所以也不一定代表真实的性能。另外，如果出现 Miss Service Time 比别的几个 Service Time 指标高出很多，表示去主服务器的请求耗时比较长，一般是代表瓶颈在主服务器那里，但这也不是绝对的。如果 squid 服务器磁盘 I/O 性能跟不上或者 CPU 不够强劲（squid 2 核心是可怜的单进程），那么也可能是 squid 本身的调度出了问题。 * Storage，在特定情况下可以估算 squid 每天缓存多少东西，在配置缓存大小等问题的时候会有帮助。 总的来说，squid snmp 数据是需要根据具体情况分析的，而且 squid snmp 的数据也不多，有时候往往需要配合别的监控来综合分析问题，比方网络出入流量，服务器的各类性能分析，主服务器和 squid access log。有一次我在某站 squid 调整了一个参数，结果那天 squid 的反应奇好，BHR 更是上了前所未有的98%。但是后来反复推敲发现这个参数并没有作用，结合各类 log 分析发现是因为那天 frjj 贴了新照片，而且被盗链，导致巨大比例的 frjj 照片的请求被 squid 有效的缓存，squid 网络出流量激增也支持这个结论。说了半天，中心意思是高负荷服务器的 squid 优化是一个长期而艰巨的任务，某些参数的优化需要几天的数据才能得出有意义的结论，所以需要有足够的耐心和针对实际情况的系统化的优化步骤。 B. 缓存策略 康神教导我们：一般来说，（缓存策略）如果后端不是配置很麻烦，建议还是在后端做，前端的配置修改大多数都是违背 http 协议的，如果出现问题，也比较难排查。 HTTP 缓存协议比较权威的可以参考 RF2616 第十三章，特别是 13.2 和 13.3 节。具体实现可以参考比方 Firefox 代码 nsHttpResponseHead.cpp 的 ComputeCurrentAge() 和 ComputeFreshnessLifetime() 函数看看各类情况的处理方式。 mod_expires 的配置就需要深刻理解这些基本概念，否则可能反而会增加请求数。如果没有特别的理由，静态文件的过期时间一般是设置为 access time 加上一定量的时间。这个一定量的时间由具体情况决定。比如网站建设初期，各类静态文件可能需要比较短的过期时间以方便网站更新；而一旦美工敲定图片，图片的过期时间可以大胆的设置为几个月。在配置完成以后如果没有很大的把握也可以实际浏览一下分析请求序列看是否浏览器端和 squid 服务器都做到了有效的缓存，特别注意 cache 相关的请求和回复头，包括 squid 提供的 X-Cache 头。 另外，虽然违反 HTTP 协议的 squid 配置一般都不推荐，但是具体到细节上，这也不是绝对的原则。下面举例说说必须违反 HTTP 协议的情况。 * 使用 javascript 做镜像网站测速，一般实现方式是从各个镜像站下载一个图片看哪一个最快。最理想的情况是图片在浏览器端不要缓存（以便下次准确测速），但是这个请求又完全没必要打到主服务器上。那么可以在 squid 里针对这个图片 url 配置强制缓存 refresh_pattern reload-into-ims ignore-reload。当然这个例子很土鳖，只是举个例子。 * reload_into_ims （这里说的是 squid 的配置参数，不是 refresh_pattern 里面的 option）。这个参数虽然违反 HTTP 协议但是对大部分网站来说是可以设置为 on 的，只要后端服务器对 If-Modified-Since 头的判断正确并且没有潜在安全问题即可。 * 浏览器 F5 刷新和 javascript 的 location.reload() 刷新可能会重新请求所有的网页内嵌元素并且可能带 no-cache 请求头，一般来说 reload_into_ims 设置成 on 已经足够保证对主服务器不造成冲击，但是如果有必要可能还是需要在 squid 配置强制缓存。 * 针对土鳖客户端的优化。比如早期的 fterm 预览图片会发送 Pragma: no-cache 的请求头，这势必导致所有 fterm 预览图片的请求如数全部打在后端服务器上，所以解决方法是 squid 这里做手脚针对这类 url 配置强制缓存。一个细节问题是如果不能缓存的图片（比方有察看权限限制的）和能缓存的图片的 url 结构完全一样，那么在 squid 强制缓存这类 url 的话又会有潜在的安全问题，这里涉及到后面会讲到的网站结构优化，针对这个问题需要修改网站的代码以明确区分这两类 url。还有另外一个例子是早期的 firefox 发送 XMLHttpRequest 请求也会发送 no-cache 的头，后来的版本改了。当年这一类 ajax 请求的 url 也是需要配置强制缓存的。 * 最后一个问题是，如果在特殊情况下必须同时在后端服务器发送 Expires 头，并且又在 squid 中配置这类 url 的 refresh_pattern，那么需要特别小心。比如，如果 squid 强制缓存时间比 mod_expires 配置的过期时间长，那么可能造成 squid 发送已经过期的内容，导致浏览器本来可以有效缓存的内容却需要不断的向服务器检查更新。 最后，有些后端服务器没办法配置 mod_expires。这可能是因为没有配置权限，也可能是因为后端服务器软件太土鳖，总之这样的情况下就必须用 squid 配置 refresh_pattern 了。 C. 网站代码及结构优化 康神教导我们：很多 squid 优化（的文章）只限于在 squid 参数和系统参数上面的调整。但是这个实在只是细枝末节的事情，只要不是太弱智的配置导致无法缓存，squid 的性能不会有太大差距。网站优化一般来说也是属于这种类型的优化，对于主服务器负荷瓶颈在磁盘 I/O，或者网络瓶颈是大量大图片文件的情况，优化网站 html 结构可能对性能提升没有半点作用。不过即便如此，有一个为 squid 考虑的网站结构，可以使得 squid 服务器的配置比较容易，也可以比较容易的实现多 squid 业务分拆，有的时候业务分拆并不是为了性能，而是为了更好的分析问题以便进一步优化网站。下面简要说说有可能提高性能的网站代码优化。 * 减少页面大小。这个问题实在是到处都有好文章，我就不详细说了。常见技巧是分离 css/js 到单独文件减少动态主页面大小同时保证静态内容有效缓存；页面 layout 设计尽量使用 div+css；有大量冗余 html 元素的部分使用 javascript 来输出。最后这个页面 javascript 化可能需要考虑搜索引擎优化 (SEO) 的问题，总的来说需要在减少流量和 SEO 之间寻找一个好的平衡点，这个只有做网站的人自己最清楚。 * 减少同一份数据的不同表现形式。大量使用 ajax 的站点有时候考虑 SEO 往往要重写一套给搜索引擎看的页面，这势必导致 squid 这里要存两套页面。但是如果功力足够还是可以做到大部分页面重用。举例来说，网站可能希望用户读文章不切换页面而使用 XMLHttpRequest 载入，这个就可以在 <a href 写上文章内容的页面以便搜索引擎扒站同时也允许用户在新窗口打开这个文章，而 onclick 事件则触发 XMLHttpRequest 载入页面并分析显示内容。只要代码写的足够漂亮，这里用一个文章页面就可以实现所有的功能。 * 标准化 url。这个可以算前一条的补充。写网站如果不小心，可能同一个资源会有不同的 url。比方某篇文章，从主页进去的 url 是 article?bid=3&id=50，从搜索结果进去却是 article?id=50&bid=3。这样两个页面，不但影响外部搜索引擎排名（自己和自己打架），更会影响 squid 效率，因为 squid 需要单独存这两类页面。 * 网站建设初期充分考虑到将来的 squid 优化。举例来说，很多网站都在页面带用户登录信息显示，这样的页面如果不使用 javascript 技巧就完全不可以在 squid 这里 cache。而实际上，如果这些动态内容可以在 javascript 里面通过 cookie 判断出来，那么完全可以用 javascript 来写。这方面的细节工作做得越好，就有越多的页面可以被 squid 安全的缓存。当然这方面的优化有时候也只有网站运行起来才能发现，维护网站的时候多分析 log，多观察，就可以发现这些细小的可以优化的地方，水滴石穿，大量小细节的优化也可以带来可观的性能提升。 D. 一些杂问题 * 同步 squid 和主服务器的时钟。从原理上说即使主服务器、squid 以及浏览器端的时钟都不同步，应该也不会造成缓存策略上的问题，但是为了防止诡异问题的发生，还是配置一下 squid 和主服务器的 ntpd 为好。ntp 是一个极轻量级的协议，现在网络上 ntpd server 也遍地都是，保证服务器时钟准确到 1秒之内也可以保证别的一些程序的事务处理逻辑。 * 密切注意搜索引擎的动向。有一些搜索引擎做的比较弱智，有的时候会突然发很多请求过来。搜索引擎扒站很容易扒到冷僻内容，所以即使请求量只是普通浏览用户 请求量的零头，也可能会对主服务器造成冲击。大部分搜索引擎还是比较守规矩的，甚至有些搜索引擎公司还可以与他们接触配置扒站方案。不老实的搜索引擎可以 通过 squid 或者主服务器 log 找出来，特别不老实的可能 iptables 都能发现。解决方法比如可以针对搜索引擎 user agent 判断，或者干脆 iptables 咔嚓掉。 * Cache replacement policy，对大论坛站点，虽然 lru 算法占用 cpu 较低，但是 service time 可能会不如带 dynamic aging 的算法稳定。据观察，lru 算法在运行几天之后的早晨如果突然碰到大量新请求，新请求会很难进入 cache，或者进入了也很快被踢出，导致非常容易形成恶性正反馈拖垮后台服务器。但是假如每天清晨清 cache，并且保证磁盘 cache 的量稍大于每天能存下来的量，那么 lru 算法应该也不会比别的算法差（事实上什么算法都一样了）。当然这只是我的一家之言，一般来说这个问题还是需要根据 squid 服务器性能和网站具体情况多次反复试验选择最合适的算法。一般来说小规模和超大规模的站点优化这个参数可能不会有什么显著的性能提升，所以不建议耗费太多时间优化这个。 * 一定时间清理 cache 并重启 squid。这个有可能只是 squid 2.5 并且是高负荷破机器上需要考虑的一个方案。某站曾经有段时间每天高峰期 Miss Service Time 都会飙升，但是主服务器却没有超负荷的现象，最后推测可能是 squid 自己调度的问题。后来每三天清理 cache 并重启 squid 似乎大大减少了这种现象。后据权威人士批复，这个可能是因为 squid cache replacement 算法过于古老，不适应高速更新的大型论坛所致。 * 多域名宣传的服务器。如果网站允许有多个域名但是所有的域名都指向同一个网站，那么要注意 squid 不要配置成多域名模式，否则它会把每个域名的 cache 都分开处理，导致效率低下而且不能有效利用缓存存储空间。题外话，单个网站宣传多个域名也会影响搜索引擎排名等等，所以本质上也是不推荐这么做的。 * maximum_object_size_in_memory，maximum_object_size 这两个参数的配置也是具体问题具体分析的。具体到某站上，常见的大文件就是附件了，由于附件最大允许大小是 5120 KB，所以 maximum_object_size 配置了 5123 KB 以保证即使最大的附件加上各 HTTP 头也能有效的被缓存。最早这个参数是配置成 5120 KB 的，然后曾经有一次发生 BHR 骤降的情况，后来分析发现是有人贴了 5120 KB RAR 分卷压缩的李宇春同学的视频，而且恰好又有无数玉米下载，大量这类请求因为超容所以都压到了主服务器上。另外 maximum_object_size_in_memory 的配置需要考虑网站具体情况和 squid 服务器的性能，这也需要实际试验出来。 最后废话一句，现在硬件降价很快，给机群升级扩容提升硬件性能往往比找个猪头优化 squid 配置更有效果，而且见效也快。所以在 squid 大配置没有问题的前提下，有钱的话应该首选硬件优化方案而不是软件细节优化。

  squid反向代理作web加速器时需要关注的系统性能因素主要是：

Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. 一. 硬件：  
1. 1.大内存（最重要，影响最大）；快速大硬盘（第二要素，更多缓存，看网站实际数据量了，要快速的，最好是1万转以上的，如sas10K或西部数据的猛禽系列）；CPU（较为次要，影响不大，特别是多核处理器基本没用）。   
1. 2.建议匹配配置：每G磁盘空间需要32M内存。这样，512M内存的系统，能支持16G的磁盘缓存。你的情况当然会不同。  
1. 内存需求依赖于如下事实：缓存目标大小，CPU体系（32位或64位），同时在线的用户数量，和你使用的特殊功能。估算：建立一个有足够磁盘空间，可存储3-7天web流量数据的系统。如带宽1M，则需要约3600*1M的数据缓存（3.5G），如果一天提供8小时有效访问，则需要缓存10-28G（看重复情况了）。  
1. 但Squid官方网站说法：squid使用内存表索引硬盘缓存内容，硬盘内容/内存索引=177，但要同时考虑到squid程序内存，cache_mem，硬盘缓冲cache等占用的内存。  
1. 因此，我的估算：2G内存的系统，使用1.5G内存作squid索引，对应硬盘150G。 3.关于硬盘说明：requests  per  second  =  1000/seek  time/硬盘数，一块硬盘是比较准确的，多块硬盘就不好说了。一定要用random-seek  time小的盘，而随机寻道时间短意味着转速要快，越快其随机寻道时间越短！ 4.关于Swap：  
1. 毫不犹豫地关闭swap，squid是个大进程，使用swap只能使性能下降  
1. 二. 适合的操作系统：  
1. 能够支持posix线程实现异步io的操作系统，如：linux2.6内核的系统  
1. 三. 适合的文件系统：  
1. reisfer文件系统，处理大量小文件（一般的网页缓存都是小文件），性能最佳四. 每个squid对应专门应用，写明httpd_accel_host避免dns查询，dns查询很消耗时间五. 配置尽量使用IP，不用域名，加快访问速度（如多台缓存服务器/后台服务器等）  
一. 硬件： 1.大内存（最重要，影响最大）；快速大硬盘（第二要素，更多缓存，看网站实际数据量了，要快速的，最好是1万转以上的，如sas10K或西部数据的猛禽系列）；CPU（较为次要，影响不大，特别是多核处理器基本没用）。 2.建议匹配配置：每G磁盘空间需要32M内存。这样，512M内存的系统，能支持16G的磁盘缓存。你的情况当然会不同。 内存需求依赖于如下事实：缓存目标大小，CPU体系（32位或64位），同时在线的用户数量，和你使用的特殊功能。估算：建立一个有足够磁盘空间，可存储3-7天web流量数据的系统。如带宽1M，则需要约3600*1M的数据缓存（3.5G），如果一天提供8小时有效访问，则需要缓存10-28G（看重复情况了）。 但Squid官方网站说法：squid使用内存表索引硬盘缓存内容，硬盘内容/内存索引=177，但要同时考虑到squid程序内存，cache_mem，硬盘缓冲cache等占用的内存。 因此，我的估算：2G内存的系统，使用1.5G内存作squid索引，对应硬盘150G。 3.关于硬盘说明：requests per second = 1000/seek time/硬盘数，一块硬盘是比较准确的，多块硬盘就不好说了。一定要用random-seek time小的盘，而随机寻道时间短意味着转速要快，越快其随机寻道时间越短！ 4.关于Swap： 毫不犹豫地关闭swap，squid是个大进程，使用swap只能使性能下降 二. 适合的操作系统： 能够支持posix线程实现异步io的操作系统，如：linux2.6内核的系统 三. 适合的文件系统： reisfer文件系统，处理大量小文件（一般的网页缓存都是小文件），性能最佳四. 每个squid对应专门应用，写明httpd_accel_host避免dns查询，dns查询很消耗时间五. 配置尽量使用IP，不用域名，加快访问速度（如多台缓存服务器/后台服务器等）  

 

 
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. BBS，论坛是典型动态内容，要保证内容更新及时的同时，提高访问速度，降低数据库负担不是个简单任务。经实践发现如下办法取得很好效果：  
1.   
1. 1) 配置SQUID，对动态内容强制CACHE，用到的配置参数是refresh_pattern  
1.   
1. refresh_pattern ^/forum/viewthread.php 1440 1000% 1440 ignore-reload  
1.   
1. /forum/viewthread.php的内容将强制保持1天  
1.   
1. 2) 修改论坛程序在用户回复帖子后，向SQUID发送PURGE命令清除相应帖子的页面CACHE，保证失效性  
1. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~实现过这一功能,但是有时候生效,有时候无效,还未进一步查明原因.(Edit by Sean)  
1.   
1. 3) 有些频繁更新的页面可以不CACHE，用no_cache参数  
1.   
1. acl no_forum_cache urlpath_regex ^/forum/forumdisplay.php  
1. no_cache DENY no_forum_cache  
BBS，论坛是典型动态内容，要保证内容更新及时的同时，提高访问速度，降低数据库负担不是个简单任务。经实践发现如下办法取得很好效果： 1) 配置SQUID，对动态内容强制CACHE，用到的配置参数是refresh_pattern refresh_pattern ^/forum/viewthread.php 1440 1000% 1440 ignore-reload /forum/viewthread.php的内容将强制保持1天 2) 修改论坛程序在用户回复帖子后，向SQUID发送PURGE命令清除相应帖子的页面CACHE，保证失效性 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~实现过这一功能,但是有时候生效,有时候无效,还未进一步查明原因.(Edit by Sean) 3) 有些频繁更新的页面可以不CACHE，用no_cache参数 acl no_forum_cache urlpath_regex ^/forum/forumdisplay.php no_cache DENY no_forum_cache
分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[Linux 常用命令](http://sech.iteye.com/blog/1155576 "Linux 常用命令") | [configure: error: ...No recognized SSL/T ...](http://sech.iteye.com/blog/1149197 "configure: error: ...No recognized SSL/TLS toolkit detected")
* 2011-08-22 13:23
* 浏览 1359
* [评论(0)](http://sech.iteye.com/blog/1154909#comments)
* 分类:[开源软件](http://www.iteye.com/blogs/category/opensource)
* [相关推荐](http://www.iteye.com/wiki/blog/1154909)

### 评论

[]()
### 发表评论

[![]()](http://sech.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://sech.iteye.com/login)

[![sech的博客]( "sech的博客: Java 小试")](http://sech.iteye.com/)

sech

* 浏览: 265919 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 青岛
* ![]()
### 最近访客 [更多访客>>](http://sech.iteye.com/blog/user_visits)

[![506328730的博客]( "506328730的博客: ")](http://506328730.iteye.com/)

[506328730](http://506328730.iteye.com/ "506328730")

[![s_pippen的博客]( "s_pippen的博客: ")](http://s-pippen.iteye.com/)

[s_pippen](http://s-pippen.iteye.com/ "s_pippen")
[![dylinshi126的博客]( "dylinshi126的博客: ")](http://dylinshi126.iteye.com/)

[dylinshi126](http://dylinshi126.iteye.com/ "dylinshi126")

[![win2011ccc的博客]( "win2011ccc的博客: ")](http://win2011ccc.iteye.com/)

[win2011ccc](http://win2011ccc.iteye.com/ "win2011ccc")

### 文章分类

* [全部博客 (228)](http://sech.iteye.com/)
* [JAVA (17)](http://sech.iteye.com/category/23417)
* [jsf (11)](http://sech.iteye.com/category/23352)
* [WEB (30)](http://sech.iteye.com/category/24437)
* [Linux (30)](http://sech.iteye.com/category/24439)
* [Ajax (2)](http://sech.iteye.com/category/24885)
* [Ruby (2)](http://sech.iteye.com/category/28269)
* [Oracle (7)](http://sech.iteye.com/category/26111)
* [SAP (1)](http://sech.iteye.com/category/26629)
* [Groovy (4)](http://sech.iteye.com/category/26883)
* [struts (5)](http://sech.iteye.com/category/29544)
* [Hibernate (4)](http://sech.iteye.com/category/31494)
* [iPhone (43)](http://sech.iteye.com/category/141706)
* [数据库 (21)](http://sech.iteye.com/category/27119)
* [项目管理 (1)](http://sech.iteye.com/category/57369)
* [正则表达式 (1)](http://sech.iteye.com/category/36730)
* [squid (5)](http://sech.iteye.com/category/170576)
* [编程相关 (7)](http://sech.iteye.com/category/24722)
* [杂 谈 (7)](http://sech.iteye.com/category/27259)
* [点点滴滴 (1)](http://sech.iteye.com/category/24710)
* [养生之道 (1)](http://sech.iteye.com/category/24958)
* [开心一笑 (5)](http://sech.iteye.com/category/25422)
* [电脑应用 (4)](http://sech.iteye.com/category/25853)
* [我的文章 (1)](http://sech.iteye.com/category/25984)
* [openssl (1)](http://sech.iteye.com/category/170063)
* [Apache (5)](http://sech.iteye.com/category/171094)
* [bind (1)](http://sech.iteye.com/category/180236)
* [Mac (2)](http://sech.iteye.com/category/188721)
### 社区版块

* [我的资讯](http://sech.iteye.com/blog/news) (0)
* [我的论坛](http://sech.iteye.com/blog/post) (21)
* [我的问答](http://sech.iteye.com/blog/answered_problems) (0)

### 存档分类

* [2013-04](http://sech.iteye.com/blog/monthblog/2013-04) (1)
* [2013-03](http://sech.iteye.com/blog/monthblog/2013-03) (2)
* [2013-01](http://sech.iteye.com/blog/monthblog/2013-01) (3)
* [更多存档...](http://sech.iteye.com/blog/monthblog_more)
### 最新评论

* [java_xiaoyi](http://easyperson.iteye.com/ "java_xiaoyi")： ...
[[SQLServer]传入的表格格式数据流(TDS)远程过程调用(RPC)协议流不正确](http://sech.iteye.com/blog/173671#bc2276342)
* [mliqiong](http://mliqiong.iteye.com/ "mliqiong")： 您好！针对第9条，我有个不同的看法，描述如下：9.WHERE后 ...
[不会使用索引，导致全表扫描情况](http://sech.iteye.com/blog/347849#bc2263034)
* [lily200825](http://lily200825.iteye.com/ "lily200825")： 好久,没看这么基础的东西了,总结得不错.
[JAVA程序员面试33问,你能回答多少题?](http://sech.iteye.com/blog/346311#bc2238145)
* [success_feel](http://success-feel.iteye.com/ "success_feel")： 非常感谢帖主
[[SQLServer]传入的表格格式数据流(TDS)远程过程调用(RPC)协议流不正确](http://sech.iteye.com/blog/173671#bc2228842)
* [forgetallaboutyou](http://forgetallaboutyou.iteye.com/ "forgetallaboutyou")：      
[男人使用说明书](http://sech.iteye.com/blog/155006#bc2222549)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2012 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![]()
