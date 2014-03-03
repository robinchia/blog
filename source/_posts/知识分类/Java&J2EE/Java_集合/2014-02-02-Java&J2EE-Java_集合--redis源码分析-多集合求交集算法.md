---
layout: post
title: "redis源码分析-多集合求交集算法"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_集合
--- 

# redis源码分析-多集合求交集算法

源码redis源码过程中学习了它内部多个set求交集的算法，觉得挺不错的，实现也相对简单。

不过要发挥这个算法的效果，前提是保证集合是**散列型**的，比如hash，set。 对链表或者数组等集合求交集方法后面介绍。

先来看看redis中多个set求交集的主要部分。
/* Sort sets from the smallest to largest, this will improve our     * algorithm's performace */    qsort(dv,setsnum,sizeof(dict*),qsortCompareSetsByCardinality);

 
di= dictGetIterator(dv[0]); while((de = dictNext(di)) != NULL) {

 
robj*ele; for (j = 1; j &lt; setsnum; j++) if (dictFind(dv[j],dictGetEntryKey(de)) == NULL) break; if (j != setsnum) continue; /* at least one set does not contain the member */

 
ele= dictGetEntryKey(de); if (!dstkey) {

 
addReplyBulk(c,ele);

 
cardinality++; } else {

 
dictAdd(dstset-&gt;ptr,ele,NULL);

 
incrRefCount(ele); } }

看代码比较难理解，redis里的set数据结构本质就是个dict(hash表), 所以可以利用本身就做过hash这点，交集算法主要步骤u如下：

1、对多个set集合根据集合大小排序，最小的排最前面，下标0，

2、得到最小set集合的迭代器，后面需要

3、遍历第二步得到的迭代器，本质上就是遍历最小的set集合中所有的元素，然后把每一步遍历得到的元素去其他set集合里查看是否存在，只要发现在一个集合中不存在就直接跳出，因为对其他集合(下标0到N)比较是按前面排序的顺序，即从第二小的集合比较，到最大集合。这样可以最大程度的提前跳出比较。当这个元素在其他集合都存在的时候，说明这个元素是交集中的值。

redis求交的大致思路如此。但是如果我们的集合是数组类的，那么要使用这种算法，需要先把所有数组集合的元素都计算hash值，这样做根本没必要，反而得不偿失。

我对这类集合求交算法如下(java代码)：

 
 Collections.sort(sort_list); //第一步和redis的交集算法一样，也是根据集合大小从小到大的排序

// hashMap 求交

HashMap<Long, Integer> hashMap = new HashMap<Long, Integer>();// 我需要再创建一个hashMap

boolean first = true; //是否是最小集合

List<Long> list = null;

for (int i = 0; i < sort_list.size(); i++) {

int index = sort_list.get(i).getIndex();// 得到集合的下标

list = values[index].getLongList();//真正的多个集合是通过values[]保存的，这里根据上面的下标就得到了这个集合

for (int j = 0; j < list.size(); j++) {//遍历这个集合

if (first) {//如果是最小的集合，直接将元素插入到前面的hashmap中，value值为1，表示出现一次

hashMap.put(list.get(j), 1);

} else {//当不是最小集合的时候，取出元素先判断hashmap里是否存在，不存在的话就根本不需要做无谓的插入，这里的插入前提是之前一直有同样的数据存在，但hashmap存在这个值，并不表示是有效值，比如第一次出现了，但是后面的集合没有出现，所以需要一个value的计数器。

if (hashMap.containsKey(list.get(j))) {

int temp = hashMap.get(list.get(j));

hashMap.put(list.get(j), temp + 1);//如果存在这个元素，则给对应的value+1

}

}

}

first = false;//第一次遍历后，就标记为false

}

/**

 * 得到交集后的结果，这个需要遍历之前的hashmap，判断value值等于集合的数量就表示这个是交集中的值

 */

List<String> result = new ArrayList();

Entry<Long, Integer> entry = null;

for (Iterator iterator = hashMap.entrySet().iterator(); iterator

.hasNext();) {

entry = (Entry<Long, Integer>) iterator.next();

if (entry.getValue() >= keyN.length) {// 多个集合的交集

result.add(entry.getKey());//把结果保存到结果集中

}

}
上面的算法其实没有必要对集合做排序，只需要得出最小的集合就可以了。

两个算法的相似处是对比较都做了优化。redis的交集算法，从最小集合开始比较，使判断提前跳出，不做无谓的比较。

第二个算法也是做了排序，从最小集合开始，首先就把这个hashmap的大小限制住了，后面不管有多大的集合，都不可能超过hashmap的范围。其次减少了无谓的数据插入，只要在之前有这个元素才做插入。

用哪种交集算法还要考虑自己的数据特点。单论两种算法，不管是在空间和时间上，前者都比后者强。但是一般的集合并不是散列类型的数据，那么还是用第二种吧，再根据自己的数据特点做优化。
来源： <[http://www.yiihsia.com/2011/04/redis%e6%ba%90%e7%a0%81%e5%88%86%e6%9e%90-%e5%a4%9a%e9%9b%86%e5%90%88%e6%b1%82%e4%ba%a4%e9%9b%86%e7%ae%97%e6%b3%95/](http://www.yiihsia.com/2011/04/redis%e6%ba%90%e7%a0%81%e5%88%86%e6%9e%90-%e5%a4%9a%e9%9b%86%e5%90%88%e6%b1%82%e4%ba%a4%e9%9b%86%e7%ae%97%e6%b3%95/)> 
