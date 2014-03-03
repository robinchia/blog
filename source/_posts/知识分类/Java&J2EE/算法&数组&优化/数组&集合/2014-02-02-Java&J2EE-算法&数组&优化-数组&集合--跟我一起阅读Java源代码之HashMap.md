---
layout: post
title: "跟我一起阅读Java源代码之HashMap"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 数组&集合
--- 

# 跟我一起阅读Java源代码之HashMap

最近闲的很，想和大家一起学习并讨论下Java的一些源代码以及其实现的数据结构，

不是什么高水平的东西，有兴趣的随便看看

 

1. 为什么要用Map，以HashMap为例

    很多时候我们有这样的需求，我们需要将数据成键值对的方式存储起来，根据key来获取value(value可以是简单值，也可以是自定义对象)

    当然用对象数组也能实现这个目的，查找时可以遍历数组，比较关键字来获取对应的value

    从性能上来讲，遍历大数组会消耗性能

    从API易用性来讲，需要自己实现查找的逻辑

    所以用HashMap是必要的    

 

2. HashMap的数据结构是怎么样的

 

    我一直对HashMap的内部结构很好奇，看了源码之后发现他是用散列实现的，即基于hashcode

    大体思想是这样的

    1. 首先建立一个数组用来存取数据，假设我们定义一个Object[] table用来存取map的value

这个很容易理解，key存在哪里呢？暂时我不想存储key

    2. 获得key的hashcode经过一定算法转成一个整数

        index，这个index的取值范围必须是0=<index<table.length，然后我将其作为数组元素的下标

        比如执行这样的操作：table[index] = value;

        这样存储的问题解决了

    3. 如何通过key去获取这个value呢

        这个太简单了，首先获取key的hashcode，然后通过刚才一样的算法得出元素下标index

        然后value = table[index]

 

简单的HashTable实现如下

 

Java代码  [![收藏代码]()]( "收藏这段代码")

1. public class SimpleHashMap {  
1.   
1.     private Object[] table;  
1.   
1.     public SimpleHashMap() {  
1.         table = new Object[10];  
1.     }  
1.   
1.     public Object get(Object key) {  
1.         int index = indexFor(hash(key.hashCode()), 10);  
1.         return table[index];  
1.     }  
1.   
1.     public void put(Object key, Object value) {  
1.         int index = indexFor(hash(key.hashCode()), 10);  
1.         table[index] = value;  
1.     }  
1.   
1.     /** 
1.      * 通过hash code 和table的length得到对应的数组下标 
1.      *  
1.      * @param h 
1.      * @param length 
1.      * @return 
1.      */  
1.     static int indexFor(int h, int length) {  
1.         return h & (length - 1);  
1.     }  
1.   
1.     /** 
1.      * 通过一定算法计算出新的hash值 
1.      *  
1.      * @param h 
1.      * @return 
1.      */  
1.     static int hash(int h) {  
1.         h ^= (h >>> 20) ^ (h >>> 12);  
1.         return h ^ (h >>> 7) ^ (h >>> 4);  
1.     }  
1.       
1.       
1.     public static void main(String[] args){  
1.         SimpleHashMap hashMap = new SimpleHashMap();  
1.         hashMap.put("key", "value");  
1.         System.out.println(hashMap.get("key"));  
1.     }  
1. }  
 

这个简单的例子大概描述了散列实现hashmap的过程

但是还很不成熟，我发现至少存在以下两个问题

1. hashmap的size是固定的

2. 如果不同的key通过hashcode得出的index相同呢，这样的情况是存在的，如何解决？
来源： <[http://tangyanbo.iteye.com/blog/1755636](http://tangyanbo.iteye.com/blog/1755636)> 由于table的大小是有限的，而key的集合范围是无限大的，所以寄希望于hashcode散落，肯定会出现多个key散落在同一个数组下标下面，

因此我们要引入另外一个概念，将key和value同时存入table[index]中，即将key和value构成一个对象放在table[index]，而且可能存放多个，他们的key对应的index相同，但是key本身不同

 

现在我们就该讨论以什么样的方式存储这些散落在同一个数组下标的元素

可以考虑数组？

也可以考虑链表存储

源码里面是用链表存储的，其实我也没明白这两种方式在这里有什么区别

，感觉无论在检索和存储上都是差不多的效率，

检索都是需要遍历的方式，而存储也可以是顺序的

 

那这个问题留给大家吧。

我们来实现链式存储的方式，首先定义一个链表数据结构Entry：

Java代码  [![收藏代码]()]( "收藏这段代码")

1. public class Entry<K, V> {  
1.     //存储key  
1.     final K key;  
1.     //存储value  
1.     V value;  
1.     //存储指向下一个节点的指针  
1.     Entry<K, V> next;  
1.     //存储key映射的hash  
1.     final int hash;  
1. }  

 

新的EntryHashMap实现方式

Java代码  [![收藏代码]()]( "收藏这段代码")

1. public class EntryHashMap<K, V> {  
1.   
1.     transient Entry[] table;  
1.   
1.     transient int size;  
1.   
1.     public V put(K key, V value) {  
1.         //计算出新的hash  
1.         int hash = hash(key.hashCode());  
1.         //计算出数组小标i  
1.         int i = indexFor(hash, table.length);  
1.         //遍历table[i]，如果table[i]没有与新加入的key相等的，则新加入  
1.         //一个value到table[i]中的entry，否则将新的value覆盖旧的value并返回旧的value  
1.         for (Entry<K, V> e = table[i]; e != null; e = e.next) {  
1.             Object k;  
1.             if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {  
1.                 V oldValue = e.value;  
1.                 e.value = value;  
1.                 return oldValue;  
1.             }  
1.         }  
1.         addEntry(hash, key, value, i);  
1.         return null;  
1.     }  
1.   
1.     public void addEntry(int hash, K key, V value, int bucketIndex) {  
1.         Entry<K, V> e = table[bucketIndex];  
1.         //将新的元素插入链表前端  
1.         table[bucketIndex] = new Entry<>(hash, key, value, e);  
1.         size++;  
1.     }  
1.   
1.     /** 
1.      * 通过hash code 和table的length得到对应的数组下标 
1.      *  
1.      * @param h 
1.      * @param length 
1.      * @return 
1.      */  
1.     static int indexFor(int h, int length) {  
1.         return h & (length - 1);  
1.     }  
1.   
1.     /** 
1.      * 通过一定算法计算出新的hash值 
1.      *  
1.      * @param h 
1.      * @return 
1.      */  
1.     static int hash(int h) {  
1.         h ^= (h >>> 20) ^ (h >>> 12);  
1.         return h ^ (h >>> 7) ^ (h >>> 4);  
1.     }  
1. }  

 

来源： <[http://tangyanbo.iteye.com/blog/1756074](http://tangyanbo.iteye.com/blog/1756074)> 为什么要用链表而不是数组

链表的作用有如下两点好处

1. remove操作时效率高，只维护指针的变化即可，无需进行移位操作

2. 重新散列时，原来散落在同一个槽中的元素可能会被散落在不同的地方，对于数组需要进行移位操作，而链表只需维护指针

 

今天研究下数组长度不够时的处理办法

table为散列数组

1. 首先定义一个不可修改的静态变量存储table的初始大小 DEFAULT_INITIAL_CAPACITY

2. 定义一个全局变量存储table的实际元素长度，size

3. 定义一个全局变量存储临界点，即元素的size>=threshold这个临界点时，扩大table的容量

4. 因为index是根据hash和table的长度计算得到的，所以还需要重新对所有元素进行散列

 

实现如下：

Java代码  [![收藏代码]()]( "收藏这段代码")

1. package sourcecoderead.collection.map;  
1.   
1.   
1. public class EntryHashMap<K, V> {  
1.   
1.     /** 初始容量 */  
1.     static final int DEFAULT_INITIAL_CAPACITY = 16;  
1.   
1.     static final float DEFAULT_LOAD_FACTOR = 0.75f;  
1.   
1.     /** 下次扩容的临界值 */  
1.     int threshold;  
1.   
1.     transient int size;  
1.   
1.     final float loadFactor;  
1.   
1.     transient Entry[] table;  
1.   
1.     public EntryHashMap() {  
1.         this.loadFactor = DEFAULT_LOAD_FACTOR;  
1.         threshold = (int) (DEFAULT_INITIAL_CAPACITY * DEFAULT_LOAD_FACTOR);  
1.         table = new Entry[DEFAULT_INITIAL_CAPACITY];  
1.     }  
1.   
1.     public V put(K key, V value) {  
1.         // 计算出新的hash  
1.         int hash = hash(key.hashCode());  
1.         // 计算出数组小标i  
1.         int i = indexFor(hash, table.length);  
1.         // 遍历table[i]，如果table[i]没有与新加入的key相等的，则新加入  
1.         // 一个value到table[i]中的entry，否则将新的value覆盖旧的value并返回旧的value  
1.         for (Entry<K, V> e = table[i]; e != null; e = e.next) {  
1.             Object k;  
1.             if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {  
1.                 V oldValue = e.value;  
1.                 e.value = value;  
1.                 return oldValue;  
1.             }  
1.         }  
1.         addEntry(hash, key, value, i);  
1.         return null;  
1.     }  
1.   
1.     public V get(K key) {  
1.         // 计算出新的hash  
1.         int hash = hash(key.hashCode());  
1.         // 计算出数组小标i  
1.         int i = indexFor(hash, table.length);  
1.         for (Entry<K, V> e = table[i]; e != null; e = e.next) {  
1.             Object k;  
1.             if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {  
1.                 return e.value;  
1.             }  
1.         }  
1.         return null;  
1.     }  
1.   
1.     private void addEntry(int hash, K key, V value, int bucketIndex) {  
1.         Entry<K, V> e = table[bucketIndex];  
1.         // 将新的元素插入链表前端  
1.         table[bucketIndex] = new Entry<>(hash, key, value, e);  
1.         if (size++ >= threshold)  
1.             resize(2 * table.length);  
1.     }  
1.   
1.     void resize(int newCapacity) {  
1.         Entry[] oldTable = table;  
1.         int oldCapacity = oldTable.length;  
1.         Entry[] newTable = new Entry[newCapacity];  
1.         transfer(newTable);  
1.         table = newTable;  
1.         threshold = (int) (newCapacity * loadFactor);  
1.     }  
1.   
1.     void transfer(Entry[] newTable) {  
1.         Entry[] src = table;  
1.         int newCapacity = newTable.length;  
1.         for (int j = 0; j < src.length; j++) {  
1.             Entry<K, V> e = src[j];  
1.             if (e != null) {  
1.                 src[j] = null;  
1.                 do {  
1.                     Entry<K, V> next = e.next;  
1.                     int i = indexFor(e.hash, newCapacity);  
1.                     e.next = newTable[i];  
1.                     newTable[i] = e;  
1.                     e = next;  
1.                 } while (e != null);  
1.             }  
1.         }  
1.     }  
1.   
1.     /** 
1.      * 通过hash code 和table的length得到对应的数组下标 
1.      *  
1.      * @param h 
1.      * @param length 
1.      * @return 
1.      */  
1.     static int indexFor(int h, int length) {  
1.         return h & (length - 1);  
1.     }  
1.   
1.     /** 
1.      * 通过一定算法计算出新的hash值 
1.      *  
1.      * @param h 
1.      * @return 
1.      */  
1.     static int hash(int h) {  
1.         h ^= (h >>> 20) ^ (h >>> 12);  
1.         return h ^ (h >>> 7) ^ (h >>> 4);  
1.     }  
1.   
1.     public static void main(String[] args) {  
1.         EntryHashMap<String, String> hashMap = new EntryHashMap<String, String>();  
1.         hashMap.put("key", "value");  
1.         System.out.println(hashMap.get("key"));  
1.     }  
1. }  
 
来源： <[http://tangyanbo.iteye.com/blog/1756536](http://tangyanbo.iteye.com/blog/1756536)> 
