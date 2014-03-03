---
layout: post
title: "Java集合框架之小结"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_集合
--- 

# Java集合框架之小结

1、Java容器类库的简化图，下面是集合类库更加完备的图。包括抽象类和遗留构件（不包括Queue的实现）：
![]()

2、ArrayList初始化时不可指定容量，如果以new ArrayList()方式创建时，初始容量为10个；如果以new ArrayList(Collection c)初始化时，容量为c.size()*1.1，即增加10%的容量；当向ArrayList中添加一个元素时，先进行容器的容量调整，如果容量不够时，则增加至原来的1.5倍加1，再然后把元素加入到容器中，即以原始容量的0.5倍比率增加。

3、Vector：初始化时容量可以设定，如果以new Vector()方式创建时，则初始容量为10，超过容量时以2倍容量增加。如果以new Vector(Collection c)方式创建时，初始容量为c.size()*1.1，超过时以2倍容量增加。如果以new Vector(int initialCapacity, int capacityIncrement)，则以capacityIncrement容量增加。

 

4、集合特点：

* List：保证以某种特定插入顺序来维护元素顺序，即保持插入的顺序，另外元素可以重复。
* ArrayList：是用数组实现的，读取速度快，插入与删除速度慢（因为插入与删除时要移动后面的元素），适合于随机访问。
* Vector：功能与ArrayList几乎相同，也是以数组实现，添加，删除，读取，设置都是基于线程同步的。
* LinkedList：双向链表来实现，删除与插入速度快，读取速度较慢，因为它读取时是从头向尾（如果节点在链的前半部分），或尾向头（如果节点在链的后半部分）查找元素。因此适合于元素的插入与删除操作。
* Set：维持它自己的内部排序，随机访问不具有意义。另外元素不可重复。
* HashSet：是最常用的，查询速度最快，因为 内部以HashMap来实现，所以插入元素不能保持插入次序。
* LinkedHashSet：继承了HashSet，保持元素的插入次序，因为内部使用LinkedHashMap实现，所以能保持元素插入次序。
* TreeSet：基于TreeMap，生成一个总是处于排序状态的set，它实现了SortedSet接口，内部以 TreeMap来实现
* TreeMap：键以某种排序规则排序，内部以red-black（红-黑）树数据结构实现，实现了SortedMap接口，具体可参《[RED-BLACK(红黑)树的实现TreeMap源码阅读](http://jiangzhengjun.iteye.com/admin/blogs/562408) 》
* HashMap: 以哈希表数据结构实现，查找对象时通过哈希函数计算其位置，它是为快速查询而设计的，其内部定义了一个hash表数组（Entry[] table），元素会通过哈希转换函数将元素的哈希地址转换成数组中存放的索引，如果有冲突，则使用散列链表的形式将所有相同哈希地址的元素串起来，可能通过查看HashMap.Entry的源码它是一个单链表结构。
* Hashtable:也是以哈希表数据结构实现的，解决冲突时与HashMap也一样也是采用了散列链表的形式，不过性能比HashMap要低。
* LinkedHashMap:继承HashMap，内部实体LinkedHashMap.Entry继承自HashMap.Entry，LinkedHashMap.Entry在HashMap.Entry的基础上新增了两个实体引用（Entry before, after），这样实体可以相互串链起来形成链，并且在LinkedHashMap中就定义了一个头节点（Entry header）用来指向循环双向链的第一个元素（通过after指向）与最后一个元素（通过before指向）。在添加一个元素时，先会通过父类HashMap将元素加入到hash表数组里，然后再会在链尾（header.before指向位置）添加（当然这一过程只是调整LinkedHashMap.Entry对象内部的before, after而已，而不是真真创建一个什么新的链表结构向里加那样）；删除先从hash表数组中删除，再将被删除的元素彻底的从双向链中断开。其实在链中添加与删除操作与LinkedList是一样的，可以参考《[Java集合框架之LinkedList及ListIterator实现源码分析](http://jiangzhengjun.iteye.com/admin/blogs/553199) 》

5、Hashtable和HashMap的区别：

* Hashtable中的方法是同步的，而HashMap中的方法在缺省情况下是非同步的。在多线程应用程序中，我们应该使用Hashtable；而对于HashMap，则需要额外的同步机制。但HashMap的同步问题可通过Collections的一个静态方法得到解决：Map Collections.synchronizedMap(Map m)，当然与可以自己在使用地方加锁。

* 在HashMap中，可以允许null作为键，且只可以有一个，否则覆盖，但可以有一个或多个值为null。因为当get()方法返回null值时，即可以表示 HashMap中没有该键，也可以表示该键所对应的值为null，所以HashMap不能由get()方法来判断否存在某个键，而应该用containsKey()方法来判断；而Hashtable不允许null键与null值。

* HashTable使用Enumeration，HashMap使用Iterator。

* Hashtable是Dictionary的子类，HashMap是Map接口的一个实现类；

* HashTable中hash table数组默认大小是11，增加的方式是 int newCapacity = oldCapacity * 2 + 1;,即增加至2倍（而不是2倍加1，因为扩容是在增加元素前进行的，在扩容后会将新增元素放入容器中）。HashMap中hash数组的默认大小是16，而且一定是2的多少次方;另外两者的默认负载因子都是0.75。

* 求哈希地址与哈希地址转hash数组（Entry table[]）索引方法不同：

HashTable直接使用对象的hashCode：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. int hash = key.hashCode();//直接使用键的hashCode方法求哈希值  
1. //哈希地址转hash数组索引，先使用最大正int数与，这样将负转正数，再与数组长度求模得到存入的hash数组索引位置  
1. int index = (hash & 0x7FFFFFFF) % tab.length;  

int hash = key.hashCode();//直接使用键的hashCode方法求哈希值

//哈希地址转hash数组索引，先使用最大正int数与，这样将负转正数，再与数组长度求模得到存入的hash数组索引位置
int index = (hash & 0x7FFFFFFF) % tab.length;

而HashMap重新计算hash值，而且用位运算&代替求模：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. int hash = hash(k);  
1. int i = indexFor(hash, table.length);  
1.   
1. static int hash(Object x) {  
1. //以键本身的hash码为基础求哈希地址，但看不懂是什么意思  
1. int h = x.hashCode();  
1. h += ~(h << 9);  
1. h ^= (h >>> 14);  
1. h += (h << 4);  
1. h ^= (h >>> 10);  
1. return h;  
1. }  
1. static int indexFor(int h, int length) {  
1. return h & (length-1);//将哈希地址转换成哈希数组中存入的索引号  
1. }  

int hash = hash(k);

int i = indexFor(hash, table.length);
 

static int hash(Object x) {
//以键本身的hash码为基础求哈希地址，但看不懂是什么意思

int h = x.hashCode();
h += ~(h << 9);

h ^= (h >>> 14);
h += (h << 4);

h ^= (h >>> 10);
return h;

}
static int indexFor(int h, int length) {

return h & (length-1);//将哈希地址转换成哈希数组中存入的索引号
}

  HashMap实现图：

![]()
 

6、集合中键值是否允许null小结

* List：可以有多个null，可以有重复值。
* HashSet：能插入一个null（因为内部是以 HashMap实现 ），忽略不插入重复元素。
* TreeSet：不能插入null （因为内部是以 TreeMap 实现 ） ，元素不能重复，如果待插入的元素存在，则忽略不插入，对元素进行排序。
* HashMap：允许一个null键与多个null值，若重复键，则覆盖以前值。
* TreeMap：不允许null键(实际上可以插入一个null键，如果这个Map里只有一个元素是不会报错的，因为一个元素时没有进行排序操作，也就不会报空指针异常，但如果插入第二个时就会立即报错)，但允许多个null值，覆盖已有键值。
* HashTable：不允许null键与null值(否则运行进报空指针异常)。也会覆盖以重复值。基于线程同步。

7、对List的选择：

* 对于随机查询与迭代遍历操作，数组比所有的容器都要快。
* 从中间的位置插入和删除元素，LinkedList要比ArrayList快，特别是删除操作。
* Vector通常不如ArrayList快，则且应该避免使用，它目前仍然存在于类库中的原因是为了支持过去的代码。
* 最佳实践：将ArrayList作为默认首选，只有当程序的性能因为经常从list中间进行插入和删除而变差的时候，才去选择LinkedList。当然了，如果只是使用固定数量的元素，就应该选择数组了。

8、对Set的选择：

* HashSet的性能总比TreeSet好(特别是最常用的添加和查找元素操作)。
* TreeSet存在的唯一原因是，它可以维持元素的排序状态，所以只有当你需要一个排好序的Set时，才应该使用TreeSet。
* 对于插入操作，LinkedHashSet比HashSet略微慢一点：这是由于维护链表所带来额外开销造成的。不过，因为有了链表，遍历LinkedHashSet会比HashSet更快。

9、对Map的选择：

* Hashtable和HashMap的效率大致相同(通常HashMap更快一点，所以HashMap有意取代Hashtable)。
* TreeMap通常比HashMap慢，因为要维护排序。
* HashMap正是为快速查询而设计的。
* LinkedHashMap比HashMap慢一点，因为它维护散列数据结构的同时还要维护链表。

10、Stack基于线程安全，Stack类是用Vector来实现的(public　class Stack extends Vector)，但最好不要用集合API里的这个实现栈，因为它继承于Vector，本就是一个错误的设计，应该是一个组合的设计关系。

 

11、Iterator对ArrayList(LinkedList)的操作限制：

* 刚实例化的迭代器如果还没有进行后移(next)操作是不能马上进行删除与修改操作的。
* 可以用ListIterator对集合连续添加与修改，但不能连续删除。
* 进行添加操作后是不能立即进行删除与修改操作的。
* 进行删除操作后可以进行添加，但不能进行修改操作。
* 进行修改后是可以立即进行删除与添加操作的。

12、当以自己的对象做为**HashMap、HashTable、LinkedHashMap、HashSet 、LinkedHashSet** 的键时，一定要重写**hashCode** ()与**equals** ()方法，因为Object的hashCode()是返回内存地址，且equals()方法也是比较内存地址，所以当要在这些hash集合中查找时，如果是另外new出的新对象是查不到的，除非重写这两个方法。因为AbstractMap类的containsKey(Object key)方法实现如下：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. if (e.hash == hash && eq(k, e.key))//先比对hashcode，再使用equals  
1.     return true;  
1.   
1. static boolean eq(Object x, Object y) {  
1.     return x == y || x.equals(y);  
1. }  

if (e.hash == hash && eq(k, e.key))//先比对hashcode，再使用equals

    return true;
 

static boolean eq(Object x, Object y) {
    return x == y || x.equals(y);

}

String对象是可以准确做为键的，因为已重写了这两个方法。

 

因此，Java中的集合框架中的哈希是以一个对象查找另外一个对象，所以重写hasCode与equals方法很重要。
13、重写hashCode()与equals()这两个方法是针对哈希类，至于其它集合，如果要用public boolean contains(Object o)或containsValue(Object value)查找时，只需要实现equals()方法即可，他们都只使用对象的 equals方法进行比对，没有使用 hashCode方法。
14、TreeMap/TreeSet：放入其中的元素一定要具有自然比较能力(即要实现java.lang.Comparable接口)或者在构造TreeMap/TreeSet时传入一个比较器(实现java.util.Comparator接口)，如果在创建时没有传入比较器，而放入的元素也没有自然比较能力时，会出现类型转换错误(因为在没有较器时，会试着转成Comparable型)。

两种比较接口：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. //自然比较器  
1. public interface java.lang.Comparable {  
1.     public int compareTo(Object o);  
1. }  
1.   
1. public interface java.util.Comparator {  
1.     int compare(Object o1, Object o2);  
1.     boolean equals(Object obj);  
1. }  

//自然比较器

public interface java.lang.Comparable {
    public int compareTo(Object o);

}
 

public interface java.util.Comparator {
    int compare(Object o1, Object o2);

    boolean equals(Object obj);
}

 

15、Collection或Map的同步控制：可以使用Collections类的相应静态方法来包装相应的集合类，使他们具线程安全，如**public**static Collection **synchronizedCollection** (Collection c)方法实质返回的是包装后的SynchronizedCollection子类，当然你也可以使用Collections的synchronizedList、synchronizedMap、synchronizedSet方法来获取不同的经过包装了的同步集合，其代码片断：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public class Collections {  
1.   
1.     //...  
1.   
1.     static Collection synchronizedCollection(Collection c, Object mutex) {  
1.         return new SynchronizedCollection(c, mutex);  
1.     }  
1.   
1.     public static List synchronizedList(List list) {  
1.         //...  
1.     }  
1.   
1.     static Set synchronizedSet(Set s, Object mutex) {  
1.         //...  
1.     }  
1.   
1.     public static Map synchronizedMap(Map m) {  
1.         return new SynchronizedMap(m);  
1.     }  
1.   
1.     //...  
1.     static class SynchronizedCollection implements Collection, Serializable {  
1.   
1.         Collection c; // 对哪个集合进行同步（包装）  
1.         Object mutex; // 对象锁，可以自己设置  
1.   
1.         //...  
1.         SynchronizedCollection(Collection c, Object mutex) {  
1.             this.c = c;  
1.             this.mutex = mutex;  
1.         }  
1.   
1.         public int size() {  
1.             synchronized (mutex) {  
1.                 return c.size();  
1.             }  
1.         }  
1.   
1.         public boolean isEmpty() {  
1.             synchronized (mutex) {  
1.                 return c.isEmpty();  
1.             }  
1.         }  
1.         //...  
1.     }  
1.   
1.     static class SynchronizedList extends SynchronizedCollection implements List {  
1.   
1.         List list;  
1.   
1.         SynchronizedList(List list, Object mutex) {  
1.             super(list, mutex);  
1.             this.list = list;  
1.         }  
1.   
1.         public Object get(int index) {  
1.             synchronized (mutex) {  
1.                 return list.get(index);  
1.             }  
1.         }  
1.         //...  
1.     }  
1.   
1.     static class SynchronizedSet extends SynchronizedCollection implements Set {  
1.         SynchronizedSet(Set s) {  
1.             super(s);  
1.         }  
1.         //...  
1.     }  
1.     //...  
1. }  

public class Collections {

 
    //...

 
    static Collection synchronizedCollection(Collection c, Object mutex) {

        return new SynchronizedCollection(c, mutex);
    }

 
    public static List synchronizedList(List list) {

        //...
    }

 
    static Set synchronizedSet(Set s, Object mutex) {

        //...
    }

 
    public static Map synchronizedMap(Map m) {

        return new SynchronizedMap(m);
    }

 
    //...

    static class SynchronizedCollection implements Collection, Serializable {
 

        Collection c; // 对哪个集合进行同步（包装）
        Object mutex; // 对象锁，可以自己设置

 
        //...

        SynchronizedCollection(Collection c, Object mutex) {
            this.c = c;

            this.mutex = mutex;
        }

 
        public int size() {

            synchronized (mutex) {
                return c.size();

            }
        }

 
        public boolean isEmpty() {

            synchronized (mutex) {
                return c.isEmpty();

            }
        }

        //...
    }

 
    static class SynchronizedList extends SynchronizedCollection implements List {

 
        List list;

 
        SynchronizedList(List list, Object mutex) {

            super(list, mutex);
            this.list = list;

        }
 

        public Object get(int index) {
            synchronized (mutex) {

                return list.get(index);
            }

        }
        //...

    }
 

    static class SynchronizedSet extends SynchronizedCollection implements Set {
        SynchronizedSet(Set s) {

            super(s);
        }

        //...
    }

    //...
}

由包装的代码可以看出只是把原集合的相应方法放在同步块里调用罢了。

 

16、通过迭代器修改集合结构
在使用迭代器遍历集合时，我们不能通过集合本身来修改集合的结构（添加、删除），只能通过迭代器来操作，下面是拿对HashMap删除操作的测试，其它集合也是这样：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public static void main(String[] args) {  
1.  Map map = new HashMap();  
1.  map.put(1, 1);  
1.  map.put(2, 3);  
1.  Set entrySet = map.entrySet();  
1.  Iterator it = entrySet.iterator();  
1.  while (it.hasNext()) {  
1.   Entry entry = (Entry) it.next();  
1.   /* 
1.    * 可以通过迭代器来修改集合结构，但前提是要在已执行过 next 或 
1.    * 前移操作，否则会抛异常：IllegalStateException 
1.    */  
1.   // it.remove();  
1.   /* 
1.    * 抛异常：ConcurrentModificationException 因为通过 迭代 器操 
1.    * 作时，不能使用集合本身来修 
1.    * 改集合的结构 
1.    */  
1.   // map.remove(entry.getKey());  
1.  }  
1.  System.out.println(map);  
1. }  

public static void main(String[] args) {

Map map = new HashMap();
map.put(1, 1);

map.put(2, 3);
Set entrySet = map.entrySet();

Iterator it = entrySet.iterator();
while (it.hasNext()) {

  Entry entry = (Entry) it.next();
  /*

   * 可以通过迭代器来修改集合结构，但前提是要在已执行过 next 或
   * 前移操作，否则会抛异常：IllegalStateException

   */
  // it.remove();

  /*
   * 抛异常：ConcurrentModificationException 因为通过 迭代 器操

   * 作时，不能使用集合本身来修
   * 改集合的结构

   */
  // map.remove(entry.getKey());

}
System.out.println(map);

}

 
