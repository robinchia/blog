---
layout: post
title: "缓存算法"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_多线程
 - 内存
--- 

# 缓存算法

**缓存算法**

没有人能说清哪种缓存算法由于其他的缓存算法。（以下的几种缓存算法，有的我也理解不好，如果感兴趣，你可以Google一下 ）

**Least Frequently Used（LFU）**：

大家好，我是 LFU，我会计算为每个缓存对象计算他们被使用的频率。我会把最不常用的缓存对象踢走。

**Least Recently User（LRU）**：

我是LRU缓存算法，我把最近最少使用的缓存对象给踢走。

我总是需要去了解在什么时候，用了哪个缓存对象。如果有人想要了解我为什么总能把最近最少使用的对象踢掉，是非常困难的。

浏览器就是使用了我（LRU）作为缓存算法。新的对象会被放在缓存的顶部，当缓存达到了容量极限，我会把底部的对象踢走，而技巧就是：我会把最新被访问的缓存对象，放到缓存池的顶部。

所以，经常被读取的缓存对象就会一直呆在缓存池中。有两种方法可以实现我，array 或者是 linked list。

我的速度很快，我也可以被数据访问模式适配。我有一个大家庭，他们都可以完善我，甚至做的比我更好（我确实有时会嫉妒，但是没关系）。我家庭的一些成员包括LRU2 和 2Q，他们就是为了完善 LRU 而存在的。

**Least Recently Used 2（LRU2）**：

我是 Least Recently Used 2，有人叫我最近最少使用twice，我更喜欢这个叫法。我会把被两次访问过的对象放入缓存池，当缓存池满了之后，我会把有两次最少使用的缓存对象踢走。 因为需要跟踪对象2次，访问负载就会随着缓存池的增加而增加。如果把我用在大容量的缓存池中，就会有问题。另外，我还需要跟踪那么不在缓存的对象，因为他 们还没有被第二次读取。我比LRU好，而且是 adoptive to access 模式 。

**Two Queues（2Q）**：

我是 Two Queues；我把被访问的数据放到LRU的缓存中，如果这个对象再一次被访问，我就把他转移到第二个、更大的LRU缓存。

我踢走缓存对象是为了保持第一个缓存池是第二个缓存池的1/3。当缓存的访问负载是固定的时候，把 LRU 换成 LRU2，就比增加缓存的容量更好。这种机制使得我比 LRU2 更好，我也是 LRU 家族中的一员，而且是 adoptive to access 模式 。

**Adaptive Replacement Cache（ARC）**：

我是 ARC，有人说我是介于 LRU 和 LFU 之间，为了提高效果，我是由2个 LRU 组成，第一个，也就是 L1，包含的条目是最近只被使用过一次的，而第二个 LRU，也就是 L2，包含的是最近被使用过两次的条目。因此， L1 放的是新的对象，而 L2 放的是常用的对象。所以，别人才会认为我是介于 LRU 和 LFU 之间的，不过没关系，我不介意。

我被认为是性能最好的缓存算法之一，能够自调，并且是低负载的。我也保存着历史对象，这样，我就可以记住那些被移除的对象，同时，也让我可以看到被移除的对象是否可以留下，取而代之的是踢走别的对象。我的记忆力很差，但是我很快，适用性也强。

**Most Recently Used（MRU）**：

我是 MRU，和 LRU 是对应的。我会移除最近最多被使用的对象，你一定会问我为什么。好吧，让我告诉你，当一次访问过来的时候，有些事情是无法预测的，并且在缓存系统中找出最少最近使用的对象是一项时间复杂度非常高的运算，这就是为什么我是最好的选择。

我是数据库内存缓存中是多么的常见！每当一次缓存记录的使用，我会把它放到栈的顶端。当栈满了的时候，你猜怎么着？我会把栈顶的对象给换成新进来的对象！

**First in First out（FIFO）**：

我是先进先出，我是一个低负载的算法，并且对缓存对象的管理要求不高。我通过一个队列去跟踪所有的缓存对象，最近最常用的缓存对象放在后面，而更早的缓存对象放在前面，当缓存容量满时，排在前面的缓存对象会被踢走，然后把新的缓存对象加进去。我很快，但是我并不适用。

**Second Chance**：

大家好，我是 second chance，我是通过FIFO修改而来的，被大家叫做 second chance 缓存算法，我比 FIFO 好的地方是我改善了 FIFO 的成本。我是 FIFO 一样也是在观察队列的前端，但是很FIFO的立刻踢出不同，我会检查即将要被踢出的对象有没有之前被使用过的标志（1一个bit表示），没有没有被使用 过，我就把他踢出；否则，我会把这个标志位清除，然后把这个缓存对象当做新增缓存对象加入队列。你可以想象就这就像一个环队列。当我再一次在队头碰到这个 对象时，由于他已经没有这个标志位了，所以我立刻就把他踢开了。我在速度上比FIFO快。

**CLock**

我是Clock，一个更好的FIFO，也比 second chance更好。因为我不会像second chance那样把有标志的缓存对象放到队列的尾部，但是也可以达到second chance的效果。

我持有一个装有缓存对象的环形列表，头指针指向列表中最老的缓存对象。当缓存miss发生并且没有新的缓存空间时，我会问问指针指向的缓存对象的标 志位去决定我应该怎么做。如果标志是0，我会直接用新的缓存对象替代这个缓存对象；如果标志位是1，我会把头指针递增，然后重复这个过程，知道新的缓存对 象能够被放入。我比second chance更快。

**Simple time-based**：

我是 simple time-based 缓存算法，我通过绝对的时间周期去失效那些缓存对象。对于新增的对象，我会保存特定的时间。我很快，但是我并不适用。

**Extended time-based expiration**：

我是 extended time-based expiration 缓存算法，我是通过相对时间去失效缓存对象的；对于新增的缓存对象，我会保存特定的时间，比如是每5分钟，每天的12点。

**Sliding time-based expiration**：

我是 sliding time-based expiration，与前面不同的是，被我管理的缓存对象的生命起点是在这个缓存的最后被访问时间算起的。我很快，但是我也不太适用。

好了！听了那么多缓存算法的自我介绍，其他的缓存算法还考虑到了下面几点：

成本。如果缓存对象有不同的成本，应该把那些难以获得的对象保存下来。
容量。如果缓存对象有不同的大小，应该把那些大的缓存对象清除，这样就可以让更多的小缓存对象进来了。
时间。一些缓存还保存着缓存的过期时间。电脑会失效他们，因为他们已经过期了。
根据缓存对象的大小而不管其他的缓存算法可能是有必要的。

**Random Cache**：

我是随机缓存，我随意的替换缓存实体，没人敢抱怨。你可以说那个被替换的实体很倒霉。通过这些行为，我随意的去处缓存实体。我比FIFO机制好，在某些情况下，我甚至比 LRU 好，但是，通常LRU都会比我好。

看看缓存元素（缓存实体）
public class CacheElement {

private Object objectValue;

private Object objectKey;

private int index;

private int hitCount;

// getters and setters

}

这个缓存实体拥有缓存的key和value，这个实体的数据结构会被以下所有缓存算法用到。

缓存算法的公用代码
public final synchronized void addElement(Object key,Object value) {

int index;
Object obj;

// get the entry from the table
obj = table.get(key);

// If we have the entry already in our table
then get it and replace only its value.
if (obj != null) {
CacheElement element;

element = (CacheElement) obj;
element.setObjectValue(value);
element.setObjectKey(key);

return;
}
}

上面的代码会被所有的缓存算法实现用到。这段代码是用来检查缓存元素是否在缓存中了，如果是，我们就替换它，但是如果我们找不到这个key对应的缓存，我们会怎么做呢？那我们就来深入的看看会发生什么吧！

现场访问

今天的专题很特殊，因为我们有特殊的客人，事实上他们是我们想要听的与会者，但是首先，先介绍一下我们的客人：Random Cache，FIFO Cache。让我们从 Random Cache开始。

看看随机缓存的实现
public final synchronized void addElement(Object key,Object value) {

int index;
Object obj;

obj = table.get(key);

if (obj != null) {
CacheElement element;

// Just replace the value.
element = (CacheElement) obj;
element.setObjectValue(value);
element.setObjectKey(key);

return;
}

// If we haven’t
filled the cache yet, put it at the end.
if (!isFull()) {
index = numEntries;
++numEntries;
} else {
// Otherwise, replace a random entry.
index = (int) (cache.length * random.nextFloat());
table.remove(cache[index].getObjectKey());
}

cache[index].setObjectValue(value);
cache[index].setObjectKey(key);
table.put(key, cache[index]);
}

看看FIFO缓存算法的实现

public final synchronized void addElement(Object
key,Object value) {
int index;
Object obj;

obj = table.get(key);

if (obj != null) {
CacheElement element;

// Just replace the value.
element = (CacheElement) obj;
element.setObjectValue(value);
element.setObjectKey(key);

return;
}

// If we haven’t filled the cache yet, put it at the end.
if (!isFull()) {
index = numEntries;
++numEntries;
} else {
// Otherwise, replace the current pointer, entry with the new one
index = current;
// in order to make Circular FIFO
if (++current >= cache.length)
current = 0;

table.remove(cache[index].getObjectKey());
}

cache[index].setObjectValue(value);
cache[index].setObjectKey(key);
table.put(key, cache[index]);
}

看看LFU缓存算法的实现

public synchronized Object getElement(Object key) {

Object obj;

obj = table.get(key);

if (obj != null) {
CacheElement element = (CacheElement) obj;
element.setHitCount(element.getHitCount() + 1);
return element.getObjectValue();
}
return null;

}

public final synchronized void addElement(Object key, Object value) {

Object obj;

obj = table.get(key);

if (obj != null) {
CacheElement element;

// Just replace the value.
element = (CacheElement) obj;
element.setObjectValue(value);
element.setObjectKey(key);

return;
}

if (!isFull()) {

index = numEntries;
++numEntries;
} else {
CacheElement element = removeLfuElement();
index = element.getIndex();
table.remove(element.getObjectKey());
}

cache[index].setObjectValue(value);
cache[index].setObjectKey(key);
cache[index].setIndex(index);
table.put(key, cache[index]);
}

public CacheElement removeLfuElement() {

CacheElement[] elements = getElementsFromTable();
CacheElement leastElement = leastHit(elements);
return leastElement;
}

public static CacheElement leastHit(CacheElement[] elements) {

CacheElement lowestElement = null;
for (int i = 0; i < elements.length; i++) {
CacheElement element = elements[i];
if (lowestElement == null) {
lowestElement = element;

} else {
if (element.getHitCount() < lowestElement.getHitCount()) { lowestElement = element; }

}

}

return lowestElement;

}

最重点的代码，就应该是 leastHit 这个方法，这段代码就是把 hitCount 最低的元素找出来，然后删除，给新进的缓存元素留位置。 看看LRU缓存算法实现

private void moveToFront(int index) {

int nextIndex, prevIndex;

if(head != index) {

nextIndex = next[index];

prevIndex = prev[index]; // Only the head has a prev entry that is an invalid index so // we don’t check. next[prevIndex] = nextIndex; // Make sure index is valid. If it isn’t, we’re at the tail // and don’t set prev[next]. if(nextIndex >= 0)
prev[nextIndex] = prevIndex;
else
tail = prevIndex;

prev[index] = -1;
next[index] = head;
prev[head] = index;
head = index;
}
}

public final synchronized void addElement(Object key, Object value) {
int index;
Object obj;

obj = table.get(key);

if(obj != null) {
CacheElement entry;

// Just replace the value, but move it to the front.
entry = (CacheElement)obj;
entry.setObjectValue(value);
entry.setObjectKey(key);

moveToFront(entry.getIndex());

return;
}

// If we haven’t filled the cache yet, place in next available spot
// and move to front.
if(!isFull()) {
if(_numEntries > 0) {
prev[_numEntries] = tail;
next[_numEntries] = -1;
moveToFront(numEntries);
}
++numEntries;
} else {
// We replace the tail of the list.
table.remove(cache[tail].getObjectKey());
moveToFront(tail);
}

cache[head].setObjectValue(value);
cache[head].setObjectKey(key);
table.put(key, cache[head]);
}

这段代码的逻辑如 LRU算法 的描述一样，把再次用到的缓存提取到最前面，而每次删除的都是最后面的元素。

结论

我们已经看到 LFU缓存算法 和 LRU缓存算法的实现方式，至于如何实现，采用数组还是 LinkedHashMap，都由你决定，不够我一般是小的缓存容量用数组，大的用LinkedHashMap。
来源： <[http://www.yiihsia.com/2011/01/%e7%bc%93%e5%ad%98%e7%ae%97%e6%b3%95/](http://www.yiihsia.com/2011/01/%e7%bc%93%e5%ad%98%e7%ae%97%e6%b3%95/)> 
Intro to Caching,Caching algorithms and caching frameworks part 2

Introduction:
In this part we are going to show how to implement some of the famous replacement algorithms as we mentioned in [part 1](http://www.jtraining.com/blogs/intro-to-caching-caching-algorithms-and-caching-frameworks.html), the code in this article is just for demonstration purpose which means you will have to do some extra effort if you want to make use of it in your application (if you are going to build your own implementation and wont use any caching frameworks)

The Leftover policy:

After programmer 1 read the article he proceeded to review the comments on this article, one of these comments were talking about leftover policy, which is named “Random Cache”

Random Cache:

I am random cache, I replace any cache entry I want, I just do that and no one can complain about that, you can say the unlucky entry, by doing this I remove any overhead of tracking references or so, am better than FIFO policy, in some cases I perform even better than LRU but in general LRU is better than me.

It is comment time:

While programmer 1 was reading the rest of the comments, he found very interesting comment about implementation of some of the famous replacement policies, actually it was a link to the commenter site which has the actual implementation so programmer 1 clicked the link and here what he got:

Meet the Cache Element:
public class CacheElement {
private Object objectValue;
private Object objectKey;
private int index;
private int
hitCount;
.
. // getters and setters
.
}

This is the cache entry which will use to hold the key and the value; this will be used in all the cache algorithms implementation

Common Code for All Caches:
public final synchronized void addElement(Object key,Object value) {
int index;
Object obj;
// get the entry from the table
obj = table.get(key);
// If we have the entry already in our table
then get it and replace only its value.
if (obj != null) {
CacheElement
element;
element = (CacheElement) obj;
element.setObjectValue(value);
element.setObjectKey(key);
return;
}
}

The above code will be common for all our implementation; it is about checking if the cacheElemnet already exists in our cache, if so then we just need to place its value and we don’t need to make anything else but what if we didn’t find it ? Then we will have to dig deeper and see what will happen below.

The Talk Show:

Today’s episode is a special episode , we have special guests , they are in fact compotators we are going to hear what everyone has to say but first lets introduce our guests:

Random Cache, FIFO Cache

Let’s start with the Random Cache.

Meet Random Cache implementation:
public final synchronized void addElement(Object key,Object value) {
int index;
Object obj;
obj = table.get(key);
if (obj
!= null) {
CacheElement element;
// Just replace the value.
element = (CacheElement) obj;
element.setObjectValue(value);
element.setObjectKey(key);
return;
}
// If we haven't
filled the cache yet, put it at the end.
if (!isFull()) {
index =
numEntries;
++numEntries;
} else {
// Otherwise, replace a random
entry.
index = (int) (cache.length * random.nextFloat());
table.remove(cache[index].getObjectKey());
}
cache[index].setObjectValue(value);
cache[index].setObjectKey(key);
table.put(key, cache[index]);
}

Analyzing Random Cache Code (Talk show):

In today’s show the Random Cache is going to explain the code line by line and here we go.
I will go straight to the main point; if I am not full then I will place the new entry that the client requested at the end of the cache (in case there is a cache miss).

I do this by getting the number of entries that resides in the cache and assign it to index (which will be the index of the current entry the client is adding) after that I increment the number of entries.
if (!isFull()) {
index = numEntries;
++numEntries;
}

If I don’t have enough room for the current entry, I will have to kick out a random entry (totally random, bribing isn’t allowed).

In order to get the random entry, I will use the random util. shipped with java to generate a random index and ask the cache to remove the entry that its index equal to the generated index.
else {
// Otherwise, replace a random entry.
index = (int) (cache.length * random.nextFloat());
table.remove(cache[index].getObjectKey());
}

At the end I just place the entry -either the cache was full or no- in the cache.

cache[index].setObjectValue(value);
cache[index].setObjectKey(key);
table.put(key, cache[index]);

Magnifying the Code:

It is said that when you look at stuff from a near view it is better to understand it, so that’s why we have a magnifying glass and we are going to magnify the code to get more near to it (and maybe understand it more).

Cache entries in the same voice: hi ho, hi ho, into cache we go.

New cache entry: excuse me; I have a question! (Asking a singing old cache entry near to him)

Old cache entry: go ahead.

New cache entry: I am new here and I don’t understand my role exactly, how will the algorithm handle us?

Old cache entry: cache! (Instead of man!), you remind me of myself when I was new (1st time I was added to the cache), I used to ask questions like that, let me show you what will happen.
![]()

Meet FIFO Cache Implementation:

public final synchronized void addElement(Object
key,Object value) {
int index;
Object obj;
obj = table.get(key);
if (obj != null) {
CacheElement element;
// Just replace the
value.
element = (CacheElement) obj;
element.setObjectValue(value);
element.setObjectKey(key);
return;
}
// If we haven't
filled the cache yet, put it at the end.
if (!isFull()) {
index =
numEntries;
++numEntries;
} else {
// Otherwise, replace the current
pointer, entry with the new one
index = current;
// in order to make
Circular FIFO
if (++current >= cache.length)
current = 0;
table.remove(cache[index].getObjectKey());
}
cache[index].setObjectValue(value);
cache[index].setObjectKey(key);
table.put(key, cache[index]);
}

 

Analyzing FIFO Cache Code (Talk show):

After Random Cache, audience went crazy for random cache, which made FIFO a little bit jealous so FIFO started talking and said:

When there is no more rooms for the new cache entry , I will have to kick out the entry at the front (the one came first) as I work in a circular queue like manner, by default the current position is at the beginning of the queue(points to the beginning of the queue).

I assign current value to index (index of the current entry) and then check to see if the incremented current greater than or equals to the cache length(coz I want to reset current –pointer- position to the beginning of the queue) ,if so then I will set current to zero again ,after that I just kick the entry at the index position (Which is the first entry in the queue now) and place the new entry.
else {
// Otherwise, replace the current pointer,
which takes care of
// FIFO in a circular fashion.
index = current;
if (++current >= cache.length)
current = 0;
table.remove(cache[index].getObjectKey());
}
cache[index].setObjectValue(value);
cache[index].setObjectKey(key);
table.put(key, cache[index]);

 

Magnifying the Code:

Back to our magnifying glass we can observe the following actions happening to our entries

 
![]()Conclusion:

As we have seen in this article how to implement the FIFO replacement policy and also Random replacement policy, in the upcoming articles we will try to take our magnifying glass and magnify LFU, LRU replacement policy, till then stay tuned ;)

来源： <[http://www.jtraining.com/component/content/article/35-jtraining-blog/137.html](http://www.jtraining.com/component/content/article/35-jtraining-blog/137.html)> 
