---
layout: post
title: "存取之美 —— HashMap原理、源码、实践"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 数组&集合
--- 

# 存取之美 —— HashMap原理、源码、实践

[首页](http://www.javaeye.com/) [新闻](http://www.javaeye.com/news) [论坛](http://www.javaeye.com/forums) [问答](http://www.javaeye.com/ask) [博客](http://www.javaeye.com/blogs) [招聘](http://www.javaeye.com/job) [更多 ▼](http://grunt1223.javaeye.com/blog/544497#)

[专栏](http://www.javaeye.com/wiki) [圈子](http://www.javaeye.com/groups) [搜索](http://www.javaeye.com/search)

[您还未登录 !](http://grunt1223.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://grunt1223.javaeye.com/login) [注册](http://grunt1223.javaeye.com/signup)

# [小白的博客](http://grunt1223.javaeye.com/)

永久域名 [http://grunt1223.javaeye.com/](http://grunt1223.javaeye.com/)

[54顶](http://grunt1223.javaeye.com/blog/544497#)
[9踩](http://grunt1223.javaeye.com/blog/544497#)

[利用方法拦截器优化Ibatis更新策略 —— ...](http://grunt1223.javaeye.com/blog/545776 "利用方法拦截器优化Ibatis更新策略  —— 基于POJO、CGLIB、SPRING AOP")

2009-12-09

### [存取之美 —— HashMap原理、源码、实践](http://grunt1223.javaeye.com/blog/544497)

**文章分类:[Java编程](http://www.javaeye.com/blogs/category/java)**
HashMap是一种十分常用的数据结构，作为一个应用开发人员，对其原理、实现的加深理解有助于更高效地进行数据存取。本文所用的jdk版本为1.5。
**使用HashMap**
《Effective JAVA》中认为，99%的情况下，当你覆盖了equals方法后，请务必覆盖hashCode方法。默认情况下，这两者会采用Object的“原生”实现方式，即：
protected native int hashCode(); public boolean equals(Object obj) { return (this == obj); }
hashCode方法的定义用到了native关键字，表示它是由C或C++采用较为底层的方式来实现的，你可以认为它返回了该对象的内存地址；而缺省equals则认为，只有当两者引用同一个对象时，才认为它们是相等的。如果你只是覆盖了equals()而没有重新定义hashCode()，在读取HashMap的时候，除非你使用一个与你保存时引用完全相同的对象作为key值，否则你将得不到该key所对应的值。
另一方面，你应该尽量避免使用“可变”的类作为HashMap的键。如果你将一个对象作为键值并保存在HashMap中，之后又改变了其状态，那么HashMap就会产生混乱，你所保存的值可能丢失（尽管遍历集合可能可以找到）。可参考*[http://www.ibm.com/developerworks/cn/java/j-jtp02183/](http://www.ibm.com/developerworks/cn/java/j-jtp02183/)*
**HashMap存取机制**
Hashmap实际上是一个数组和链表的结合体，利用数组来模拟一个个桶（类似于Bucket Sort）以快速存取不同hashCode的key，对于相同hashCode的不同key，再调用其equals方法从List中提取出和key所相对应的value。
JAVA中hashMap的初始化主要是为initialCapacity和loadFactor这两个属性赋值。前者表示hashMap中用来区分不同hash值的key空间长度，后者是指定了当hashMap中的元素超过多少的时候，开始自动扩容，。默认情况下initialCapacity为16，loadFactor为0.75，它表示一开始hashMap可以存放16个不同的hashCode，当填充到第12个的时候，hashMap会自动将其key空间的长度扩容到32，以此类推；这点可以从源码中看出来：
void addEntry(int hash, K key, V value, int bucketIndex) { Entry<K,V> e = table[bucketIndex]; table[bucketIndex] = new Entry<K,V>(hash, key, value, e); if (size++ >= threshold) resize(2 * table.length); }
而每当hashMap扩容后，内部的每个元素存放的位置都会发生变化（因为元素的最终位置是其hashCode对key空间长度取模而得），因此resize方法中又会调用transfer函数，用来重新分配内部的元素；这个过程成为rehash，是十分消耗性能的，因此在可预知元素的个数的情况下，一般应该避免使用缺省的initialCapacity，而是通过构造函数为其指定一个值。例如我们可能会想要将数据库查询所得1000条记录以某个特定字段（比如ID）为key缓存在hashMap中，为了提高效率、避免rehash，可以直接指定initialCapacity为2048。
另一个值得注意的地方是，hashMap其key空间的长度一定为2的N次方，这一点可以从一下源码中看出来：
int capacity = 1; while (capacity < initialCapacity) capacity <<= 1;
即使我们在构造函数中指定的initialCapacity不是2的平方数，capacity还是会被赋值为2的N次方。
为什么Sun Microsystem的工程师要将hashMap key空间的长度设为2的N次方呢？这里参考R.W.Floyed给出的衡量散列思想的三个标准：

一个好的hash算法的计算应该是非常快的
一个好的hash算法应该是冲突极小化
如果存在冲突,应该是冲突均匀化
为了将各元素的hashCode保存至长度为Length的key数组中，一般采用取模的方式，即index = hashCode % Length。不可避免的，存在多个不同对象的hashCode被安排在同一位置，这就是我们平时所谓的“冲突”。如果仅仅是考虑元素均匀化与冲突极小化，似乎应该将Length取为素数（尽管没有明显的理论来支持这一点，但数学家们通过大量的实践得出结论，对素数取模的产生结果的无关性要大于其它数字）。为此，Craig Larman and Rhett Guthrie《Java Performence》中对此也大加抨击。为了弄清楚这个问题，Bruce Eckel（Thinking in JAVA的作者）专程采访了java.util.hashMap的作者Joshua Bloch，并将他采用这种设计的原因放到了网上（[*http://www.roseindia.net/javatutorials/javahashmap.shtml*](http://grunt1223.javaeye.com/blog/<em>http://www.roseindia.net/javatutorials/javahashmap.shtml</em>)） 。
上述设计的原因在于，取模运算在包括JAVA在内的大多数语言中的效率都十分低下，而当除数为2的N次方时，取模运算将退化为最简单的位运算，其效率明显提升（按照Bruce Eckel给出的数据，大约可以提升5～8倍） 。看看JDK中是如何实现的：
static int indexFor(int h, int length) { return h & (length-1); }
当key空间长度为2的N次方时，计算hashCode为h的元素的索引可以用简单的与操作来代替笨拙的取模操作！假设某个对象的hashCode为35（二进制为100011），而hashMap采用默认的initialCapacity（16），那么indexFor计算所得结果将会是100011 & 1111 = 11，即十进制的3，是不是恰好是35 Mod 16。
上面的方法有一个问题，就是它的计算结果仅有对象hashCode的低位决定，而高位被统统屏蔽了；以上面为例，19（10011）、35（100011）、67（1000011）等就具有相同的结果。针对这个问题， Joshua Bloch采用了“防御性编程”的解决方法，在使用各对象的hashCode之前对其进行二次Hash，参看JDK中的源码：
static int hash(Object x) { int h = x.hashCode(); h += ~(h << 9); h ^= (h >>> 14); h += (h << 4); h ^= (h >>> 10); return h; }
采用这种旋转Hash函数的主要目的是让原有hashCode的高位信息也能被充分利用，且兼顾计算效率以及数据统计的特性，其具体的原理已超出了本文的领域。
加快Hash效率的另一个有效途径是编写良好的自定义对象的HashCode，String的实现采用了如下的计算方法：
for (int i = 0; i < len; i++) { h = 31*h + val[off++]; } hash = h;
这种方法HashCode的计算方法可能最早出现在Brian W. Kernighan和Dennis M. Ritchie的《The C Programming Language》中，被认为是性价比最高的算法（又被称为times33算法，因为C中乘数常量为33，JAVA中改为31），实际上，包括List在内的大多数的对象都是用这种方法计算Hash值。
另一种比较特殊的hash算法称为布隆过滤器，它以牺牲细微精度为代价，换来存储空间的大量节俭，常用于诸如判断用户名重复、是否在黑名单上等等，可以参考李开复的数学之美系列第13篇（*[http://googlechinablog.com/2006/08/blog-post.html](http://googlechinablog.com/2006/08/blog-post.html)*）
**Fail-Fast机制**
众所周知，HashMap不是线程安全的集合类。但在某些容错能力较好的应用中，如果你不想仅仅因为1%的可能性而去承受hashTable的同步开销，则可以考虑利用一下HashMap的Fail-Fast机制，其具体实现如下：
Entry<K,V> nextEntry() { if (modCount != expectedModCount) throw new ConcurrentModificationException(); …… }
其中modCount为HashMap的一个实例变量，并且被声明为volatile，表示任何线程都可以看到该变量被其它线程修改的结果（根据JVM内存模型的优化，每一个线程都会存一份自己的工作内存，此工作内存的内容与本地内存并非时时刻刻都同步，因此可能会出现线程间的修改不可见的问题） 。使用Iterator开始迭代时，会将modCount的赋值给expectedModCount，在迭代过程中，通过每次比较两者是否相等来判断HashMap是否在内部或被其它线程修改。HashMap的大多数修改方法都会改变ModCount，参考下面的源码：
public V put(K key, V value) { K k = maskNull(key); int hash = hash(k); int i = indexFor(hash, table.length); for (Entry<K,V> e = table[i]; e != null; e = e.next) { if (e.hash == hash && eq(k, e.key)) { V oldValue = e.value; e.value = value; e.recordAccess(this); return oldValue; } } modCount++; addEntry(hash, k, value, i); return null; }
以put方法为例，每次往HashMap中添加元素都会导致modCount自增。其它诸如remove、clear方法也都包含类似的操作。
从上面可以看出，HashMap所采用的Fail-Fast机制本质上是一种乐观锁机制，通过检查状态——没有问题则忽略——有问题则抛出异常的方式，来避免线程同步的开销，下面给出一个在单线程环境下发生Fast-Fail的例子：
class Test { public static void main(String[] args) { java.util.HashMap<Object,String> map=new java.util.HashMap<Object,String>(); map.put(new Object(), "a"); map.put(new Object(), "b"); java.util.Iterator<Object> it=map.keySet().iterator(); while(it.hasNext()){ it.next(); map.put("", ""); System.out.println(map.size()); } }
运行上面的代码会抛出java.util.ConcurrentModificationException，因为在迭代过程中修改了HashMap内部的元素导致modCount自增。若将上面代码中 map.put(new Object(), "b") 这句注释掉，程序会顺利通过，因为此时HashMap中只包含一个元素，经过一次迭代后已到了尾部，所以不会出现问题，也就没有抛出异常的必要了。
在通常并发环境下，还是建议采用同步机制。这一般通过对自然封装该映射的对象进行同步操作来完成。如果不存在这样的对象，则应该使用 Collections.synchronizedMap 方法来“包装”该映射。最好在创建时完成这一操作，以防止意外的非同步访问。
**LinkedHashMap**
遍历HashMap所得到的数据是杂乱无章的，这在某些情况下客户需要特定遍历顺序时是十分有用的。比如，这种数据结构很适合构建 LRU 缓存。调用 put 或 get 方法将会访问相应的条目（假定调用完成后它还存在）。putAll 方法以指定映射的条目集合迭代器提供的键-值映射关系的顺序，为指定映射的每个映射关系生成一个条目访问。Sun提供的J2SE说明文档特别规定任何其他方法均不生成条目访问，尤其，collection 集合类的操作不会影响底层映射的迭代顺序。
LinkedHashMap的实现与 HashMap 的不同之处在于，前者维护着一个运行于所有条目的双重链接列表。此链接列表定义了迭代顺序，该迭代顺序通常就是集合中元素的插入顺序。该类定义了header、before与after三个属性来表示该集合类的头与前后“指针”，其具体用法类似于数据结构中的双链表，以删除某个元素为例：
private void remove() { before.after = after; after.before = before; }
实际上就是改变前后指针所指向的元素。
显然，由于增加了维护链接列表的开支，其性能要比 HashMap 稍逊一筹，不过有一点例外：LinkedHashMap的迭代所需时间与其的所包含的元素成比例；而HashMap 迭代时间很可能开支较大，因为它所需要的时间与其容量（分配给Key空间的长度）成比例。一言以蔽之，随机存取用HashMap，顺序存取或是遍历用LinkedHashMap。
LinkedHashMap还重写了removeEldestEntry方法以实现自动清除过期数据的功能，这在HashMap中是无法实现的，因为后者其内部的元素是无序的。默认情况下，LinkedHashMap中的removeEldestEntry的作用被关闭，其具体实现如下：
protected boolean removeEldestEntry(Map.Entry<K,V> eldest) { return false; }
可以使用如下的代码覆盖removeEldestEntry：
private static final int MAX_ENTRIES = 100; protected boolean removeEldestEntry(Map.Entry eldest) { return size() > MAX_ENTRIES; }
它表示，刚开始，LinkedHashMap中的元素不断增长；当它内部的元素超过MAX_ENTRIES（100）后，每当有新的元素被插入时，都会自动删除双链表中最前端（最旧）的元素，从而保持LinkedHashMap的长度稳定。
缺省情况下，LinkedHashMap采取的更新策略是类似于队列的FIFO，如果你想实现更复杂的更新逻辑比如LRU（最近最少使用） 等，可以在构造函数中指定其accessOrder为true，因为的访问元素的方法（get）内部会调用一个“钩子”，即recordAccess，其具体实现如下：
void recordAccess(HashMap<K,V> m) { LinkedHashMap<K,V> lm = (LinkedHashMap<K,V>)m; if (lm.accessOrder) { lm.modCount++; remove(); addBefore(lm.header); } }
上述代码主要实现了这样的功能：如果accessOrder被设置为true，则每次访问元素时，都将该元素移至headr的前面，即链表的尾部。将removeEldestEntry与accessOrder一起使用，就可以实现最基本的内存缓存，具体代码可参考[*http://bluepopopo.javaeye.com/blog/180236*](http://grunt1223.javaeye.com/blog/<em>http://bluepopopo.javaeye.com/blog/180236</em>)。
**WeakHashMap**
99%的JAVA教材教导我们不要去干预JVM的垃圾回收机制，但JAVA中确实存在着与其密切相关的四种引用：强引用、软引用、弱引用以及幻象引用。
JAVA中默认的HashMap采用的是采用类似于强引用的强键来管理的，这意味着即使作为key的对象已经不存在了（指没有任何一个引用指向它），也仍然会保留在HashMap中，在某些情况下（例如内存缓存）中，这些过期的条目可能会造成内存泄漏等问题。
WeakHashMap采用的策略是，只要作为key的对象已经不存在了（超出生命周期），就不会阻止垃圾收集器清空此条目，即使当前机器的内存并不紧张。不过，由于GC是一个优先级很低的线程，因此不一定会很快发现那些只具有弱引用的对象，除非你显示地调用它，可以参考下面的例子：
public static void main(String[] args) { Map<String, String>map = new WeakHashMap<String, String>(); map.put(new String("Alibaba"), "alibaba"); while (map.containsKey("Alibaba")) { try { Thread.sleep(500); } catch (InterruptedException ignored) { } System.out.println("Checking for empty"); System.gc(); }
上述代码输出一次Checking for empty就退出了主线程，意味着GC在最近的一次垃圾回收周期中清除了new String(“Alibaba”),同时WeakHashMap也做出了及时的反应，将该键对应的条目删除了。如果将map的类型改为HashMap的话，由于其内部采用的是强引用机制，因此即使GC被显示调用，map中的条目依然存在，程序会不断地打出Checking for empty字样。另外，在使用WeakHashMap的情况下，若是将
map.put(new String("Alibaba"), "alibaba");
改为
map.put("Alibaba", "alibaba");
程序还是会不断输出Checking for empty。这与前面我们分析的WeakHashMap的弱引用机制并不矛盾，因为JVM为了减小重复创建和维护多个相同String的开销，其内部采用了蝇量模式（《JAVA与模式》），此时的“Alibaba”是存放在常量池而非堆中的，因此即使没有对象指向“Alibaba”，它也不会被GC回收。弱引用特别适合以下对象：占用大量内存，但通过垃圾回收功能回收以后很容易重新创建。
介于HashMap和WeakHashMap之中的是SoftHashMap，它所采用的软引用的策略指的是，垃圾收集器并不像其收集弱可及的对象一样尽量地收集软可及的对象，相反，它只在真正 “需要” 内存时才收集软可及的对象。软引用对于垃圾收集器来说是一种“睁一只眼，闭一只眼”方式，即 “只要内存不太紧张，我就会保留该对象。但是如果内存变得真正紧张了，我就会去收集并处理这个对象。” 就这一点看，它其实要比WeakHashMap更适合于实现缓存机制。遗憾的是，JAVA中并没有实现相关的SoftHashMap类（Apache和Google提供了第三方的实现），但它却是提供了两个十分重要的类java.lang.ref.SoftReference以及ReferenceQueue，可以在对象应用状态发生改变是得到通知，可以参考com.alibaba.common.collection.SofthashMap中processQueue方法的实现:
private ReferenceQueue queue = new ReferenceQueue(); ValueCell vc; Map hash = new HashMap(initialCapacity, loadFactor); …… while ((vc = (ValueCell) queue.poll()) != null) { if (vc.isValid()) { hash.remove(vc.key); } else { valueCell.dropped--; } } }
processQueue方法会在几乎所有SoftHashMap的方法中被调用到，JVM会通过ReferenceQueue的poll方法通知该对象已经过期并且当前的内存现状需要将它释放，此时我们就可以将其从hashMap中剔除。事实上，默认情况下，Alibaba的MemoryCache所使用的就是SoftHashMap。

[**54**
顶](http://grunt1223.javaeye.com/blog/544497#)[**9**
踩](http://grunt1223.javaeye.com/blog/544497#)
[利用方法拦截器优化Ibatis更新策略 —— ...](http://grunt1223.javaeye.com/blog/545776 "利用方法拦截器优化Ibatis更新策略  —— 基于POJO、CGLIB、SPRING AOP")

* 09:46
* 浏览 (5374)
* [评论](http://grunt1223.javaeye.com/blog/544497#comments) (16)
* 分类: [算法与数据结构](http://grunt1223.javaeye.com/category/89020)
* [相关推荐](http://www.javaeye.com/wiki/topic/544497)
### 评论

[]()

16 楼 [beneo](http://beneo.javaeye.com/) 2010-06-18   [引用](http://grunt1223.javaeye.com/blog/544497#)

是不是叫做Floyed的人都是神啊
15 楼 [usiboy](http://usiboy.javaeye.com/) 2009-12-27   [引用](http://grunt1223.javaeye.com/blog/544497#)

这篇文章分析的很透彻，无论是从算法的角度还是从应用的层面都讲的挺详细的，本人也在研究HashMap的写法，但不足的是少了一点HashMap的设计模式，从《Effective Java》这本书来看，也提到了一点HashMap的模式，我最不明白的是Entry的设计，希望能和楼主一起讨论这个Entry设计的目的。

14 楼 [dvaknheo](http://dvaknheo.javaeye.com/) 2009-12-15   [引用](http://grunt1223.javaeye.com/blog/544497#)

有没有碰到过 hash 碰撞的诡异现象？
13 楼 [liuxuejin](http://liuxuejin.javaeye.com/) 2009-12-15   [引用](http://grunt1223.javaeye.com/blog/544497#)

写得真好，我现在正在研究这个东西！

12 楼 [longhua828](http://longhua828.javaeye.com/) 2009-12-14   [引用](http://grunt1223.javaeye.com/blog/544497#)

真强，本人第一次发帖，好不容易通过了论坛发帖规则测试，太难了
11 楼 [kellersoon](http://kellersoon.javaeye.com/) 2009-12-12   [引用](http://grunt1223.javaeye.com/blog/544497#)

好文章， 感觉下边这段很巧妙![]()
static int indexFor(int h, int length) { return h & (length-1); }

10 楼 [liusu](http://liusu.javaeye.com/) 2009-12-11   [引用](http://grunt1223.javaeye.com/blog/544497#)

Test '%' and '&'
package test; /** * Why HashMap use operation '&' replace '%' ? * * Test: * % Cost 2844ms * & Cost 921ms * * So. * @author seany1 * */ public class ModolTest { public static void main(String[] args) { int base = 1 << 4; int all = 100000000; System.out.println(base); String hashTest = new String("Test hash"); long start = System.currentTimeMillis(); for (int i = 0; i < all; i++) { int x = hashTest.hashCode() % base; } long end = System.currentTimeMillis(); System.out.println("'%' Cost " + (end - start) + "ms"); start = System.currentTimeMillis(); for (int i = 0; i < all; i++) { int x = hashTest.hashCode() & base; } end = System.currentTimeMillis(); System.out.println("'&' Cost " + (end - start) + "ms"); } }
9 楼 [andyu2008](http://ardenyu.javaeye.com/) 2009-12-11   [引用](http://grunt1223.javaeye.com/blog/544497#)

看完了，楼主写的的确很不错！

8 楼 [Aguo](http://halzhang.javaeye.com/) 2009-12-11   [引用](http://grunt1223.javaeye.com/blog/544497#)

very well![]()
7 楼 [J-catTeam](http://j-catteam.javaeye.com/) 2009-12-10   [引用](http://grunt1223.javaeye.com/blog/544497#)

为什么都要说hashcode就是地址呢···
呵呵
~散列码

6 楼 [challengerjiang](http://challengerjiang.javaeye.com/) 2009-12-10   [引用](http://grunt1223.javaeye.com/blog/544497#)

真的很好哦![]()
5 楼 [idealab](http://idealab.javaeye.com/) 2009-12-10   [引用](http://grunt1223.javaeye.com/blog/544497#)

![]()

4 楼 [enhydra](http://enhydra.javaeye.com/) 2009-12-10   [引用](http://grunt1223.javaeye.com/blog/544497#)

真不错 
3 楼 [lijiecong](http://lijiecong.javaeye.com/) 2009-12-09   [引用](http://grunt1223.javaeye.com/blog/544497#)

写的非常好，数学之美不是李开复写的：）

2 楼 [猫尾摆摆](http://wh-mimi.javaeye.com/) 2009-12-09   [引用](http://grunt1223.javaeye.com/blog/544497#)

研究的很透彻了，对旋转hash没明白，呵呵
1 楼 [mylazygirl](http://mylazygirl.javaeye.com/) 2009-12-09   [引用](http://grunt1223.javaeye.com/blog/544497#)

写的不错~~

### 发表评论

您还没有登录，请[登录](http://grunt1223.javaeye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![grunt1223的博客]( "grunt1223的博客: 小白的博客")](http://grunt1223.javaeye.com/)

grunt1223

* 浏览: 9713 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 杭州
* ![]()
* [详细资料](http://grunt1223.javaeye.com/blog/profile) [留言簿](http://grunt1223.javaeye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://grunt1223.javaeye.com/blog/user_visits)

[![mmBlue的博客]( "mmBlue的博客: ")](http://mmblue.javaeye.com/)

[mmBlue](http://mmblue.javaeye.com/)

[![nickevin的博客]( "nickevin的博客: Less is more")](http://nickevin.javaeye.com/)

[nickevin](http://nickevin.javaeye.com/)
[![yingmu0591的博客]( "yingmu0591的博客: yingmu0591")](http://yingmu0591.javaeye.com/)

[yingmu0591](http://yingmu0591.javaeye.com/)

[![xlongbuilder的博客]( "xlongbuilder的博客: 不跟蛤蟆讨论大海的故事")](http://xlongbuilder.javaeye.com/)

[xlongbuilder](http://xlongbuilder.javaeye.com/)

### 博客分类

* [全部博客 (5)](http://grunt1223.javaeye.com/)
* [算法与数据结构 (1)](http://grunt1223.javaeye.com/category/89020)
* [设计模式 (3)](http://grunt1223.javaeye.com/category/89031)
* [Spring、Ibatis、Hibernate (1)](http://grunt1223.javaeye.com/category/89223)
* [企业级应用开发 (0)](http://grunt1223.javaeye.com/category/89474)
* [产品设计 (0)](http://grunt1223.javaeye.com/category/89475)
* [心路历程 (0)](http://grunt1223.javaeye.com/category/89476)
### 其他分类

* [我的收藏](http://grunt1223.javaeye.com/blog/favorite) (6)
* [我的论坛主题贴](http://grunt1223.javaeye.com/blog/topic) (0)
* [我的所有论坛贴](http://grunt1223.javaeye.com/blog/post) (3)
* [我的精华良好贴](http://grunt1223.javaeye.com/blog/article) (0)

### 最近加入圈子
### 存档

* [2009-12](http://grunt1223.javaeye.com/blog/monthblog/2009-12) (5)
* [更多存档...](http://grunt1223.javaeye.com/blog/monthblog_more)

### 最新评论

* [存取之美 —— HashMap原 ...](http://grunt1223.javaeye.com/blog/544497#comments "存取之美 —— HashMap原理、源码、实践")
是不是叫做Floyed的人都是神啊
-- by [beneo](http://beneo.javaeye.com/)
* [设计模式初学者教程（下）](http://grunt1223.javaeye.com/blog/551287#comments "设计模式初学者教程（下）")
正在读图中的1，2，4，5这4本书。
-- by [Durian](http://duriun-yahoo-cn.javaeye.com/)
* [设计模式初学者教程（下）](http://grunt1223.javaeye.com/blog/551287#comments "设计模式初学者教程（下）")
写得很有意思，谢谢这篇文章。
-- by [xmiangui](http://xmiangui.javaeye.com/)
* [存取之美 —— HashMap原 ...](http://grunt1223.javaeye.com/blog/544497#comments "存取之美 —— HashMap原理、源码、实践")
这篇文章分析的很透彻，无论是从算法的角度还是从应用的层面都讲的挺详细的，本人也在研 ...
-- by [usiboy](http://usiboy.javaeye.com/)
* [设计模式初学者教程（下）](http://grunt1223.javaeye.com/blog/551287#comments "设计模式初学者教程（下）")
很好的诙谐授课方式！！！
-- by [hqm1988](http://hqm1988.javaeye.com/)
### 评论排行榜

* [存取之美 —— HashMap原理、源码、实践](http://grunt1223.javaeye.com/blog/544497 "存取之美 —— HashMap原理、源码、实践")
* [设计模式初学者教程（上）](http://grunt1223.javaeye.com/blog/549122 "设计模式初学者教程（上）")
* [设计模式初学者教程（下）](http://grunt1223.javaeye.com/blog/551287 "设计模式初学者教程（下）")
* [设计模式的定义、设计原则及案例](http://grunt1223.javaeye.com/blog/549893 "设计模式的定义、设计原则及案例")
* [利用方法拦截器优化Ibatis更新策略 —— ...](http://grunt1223.javaeye.com/blog/545776 "利用方法拦截器优化Ibatis更新策略  —— 基于POJO、CGLIB、SPRING AOP")

* [![Rss]()](http://grunt1223.javaeye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://grunt1223.javaeye.com/rss)
* [![Rss_xianguo]()](http://www.xianguo.com/subscribe.php?url=http://grunt1223.javaeye.com/rss)
* [[什么是RSS?]](http://www.google.com/search?hl=zh-CN&q=RSS)
声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2010 JavaEye.com. All rights reserved. 上海炯耐计算机软件有限公司 [ 沪ICP备05023328号 ]
