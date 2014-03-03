---
layout: post
title: "Hash表分析以及Java实现"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - java深入分析
--- 

# Hash表分析以及Java实现

**[Hash表分析以及Java实现](http://java-mzd.iteye.com/blog/827523)**

**文章分类****:[综合技术](http://www.iteye.com/blogs/category/tech)**

       这篇博客主要探讨**Hash****表**中的一些原理/概念，及根据这些原理/概念，自己设计一个用来存放/查找数据的Hash表，并且与JDK中的HashMap类进行比较。

我们分一下七个步骤来进行。 

**一。****   Hash****表概念**

**二****.      Hash****构造函数的方法，及适用范围**

**三****.       Hash****处理冲突方法，各自特征**

**四****.       Hash****查找过程**

**五****.       ****实现一个使用****Hash****存数据的场景****-------Hash****查找算法，插入算法**

**六****.       JDK****中****HashMap****的实现**

**七****.       Hash****表与****HashMap****的对比，性能分析**

 

 **一。****   Hash****表概念**** **

               在查找表中我们已经说过，在Hash表中，**记录在表中的位置和其关键字之间存在着一种确定的关系**。这样       我们就能预先知道所查关键字在表中的位置，从而直接通过下标找到记录。使ASL趋近与0.

 

              1) **  **哈希(Hash)函数是一个映象，即： 将关键字的集合映射到某个地址集合上，它的设置很灵活，只要这个地       址集合的大小不超出允许范围即可；

             2)  由于哈希函数是一个压缩映象，因此，在一般情况下，很容易产生“冲突”现象，即： key1¹ key2，而  f            (key1) = f(key2)。

              3).  只能尽量减少冲突而不能完全避免冲突，这是因为通常关键字集合比较大，其元素包括所有可能的关键字，       而地址集合的元素仅为哈希表中的地址值

 

       在构造这种特殊的“查找表” 时，除了需要选择一个**“****好****”(****尽可能少产生冲突****)**的哈希函数之外；还需要找到一      种**“****处理冲突****”** 的方法。

 

**二****.     Hash****构造函数的方法，及适用范围**

§    **直接定址法**

§    **数字分析法**

§    **平方取中法**

§    **折叠法**

§    **除留余数法**

§    **随机数法**      

 

      （1）直接定址法：

                哈希函数为关键字的线性函数，H(key) = key 或者 H(key) = a ´ key + b

              **此法仅适合于**：地址集合的大小 = = 关键字集合的大小，其中a和b为常数。

     （2）数字分析法：

             假设关键字集合中的每个关键字都是由 s 位数字组成 (u1, u2, …, us)，分析关键字集中的全体，                  并从中提取分布均匀的若干位或它们的组合作为地址。

             **此法适于:**能预先估计出全体关键字的每一位上各种数字出现的频度。

     （3）平方取中法：

               以关键字的平方值的中间几位作为存储地址。求“关键字的平方值” 的目的是“扩大差别” ，同                    时平方值的中间各位又能受到整个关键字中各位的影响。

             **此法适于:**关键字中的每一位都有某些数字重复出现频度很高的现象。

     （4）折叠法：

            将关键字分割成若干部分，然后取它们的叠加和为哈希地址。两种叠加处理的方法：移位叠加:将分                割后的几部分低位对齐相加；间界叠加:从一端沿分割界来回折叠，然后对齐相加。

            **此法适于：**关键字的数字位数特别多。

     （5）除留余数法：

             设定哈希函数为:H(key) = key MOD p   ( p≤m )，其中， m为表长，p 为不大于 m 的素数，或                 是不含 20 以下的质因子

     （6）随机数法：

           设定哈希函数为:H(key) = Random(key)其中，Random 为伪随机函数

           **此法适于：**对长度不等的关键字构造哈希函数。

 

         实际造表时，采用何种构造哈希函数的方法取决于建表的关键字集合的情况(包括关键字的范围和形态)，以及哈希表    长度（哈希地址范围），**总的原则是使产生冲突的可能性降到尽可能地小。**

**三****.       Hash****处理冲突方法，各自特征**

 **“****处理冲突” 的实际含义是：为产生冲突的关键字寻找下一个哈希地址。**

§    **  ****开放定址法**

§    **  ****再哈希法**

§    **  ****链地址法**

      （1）开放定址法：

               为产生冲突的关键字地址 H(key) 求得一个地址序列： H0, H1, H2, …, Hs  1≤s≤m-1，Hi = ( H(key)                 +di  ) MOD m，其中： i=1, 2, …, s，H(key)为哈希函数;m为哈希表长;

 

      （2）链地址法：
![http://dl.iteye.com/upload/attachment/355453/2a1ac1de-80ef-33d5-a120-9a8f07dbf3e9.jpg]()

             将所有哈希地址相同的记录都链接在同一链表中。

 

      （3）再哈希法：

               方法：构造若干个哈希函数，当发生冲突时，根据另一个哈希函数计算下一个哈希地址，直到冲突不再发                  生。即：Hi=Rhi(key)     i=1,2,……k，其中：Rhi——不同的哈希函数，特点：计算时间增加

 **四****.       Hash****查找过程**

 
![http://dl.iteye.com/upload/attachment/355455/a946abd8-ba7b-3e4e-b5a5-b94059e086ac.png]()

**  **      对于给定值 K,计算哈希地址 i = H(K)，若 r[i] = NULL  则查找不成功，若 r[i].key = K  则查找成功， 否则 “求     下一地址 Hi” ，直至r[Hi] = NULL  (查找不成功)  或r[Hi].key = K  (查找成功) 为止。

 

 **五****.       ****实现一个使用****Hash****存数据的场景****-------Hash****查找算法，插入算法**

         假设我们要设计的是一个用来保存中南大学所有在校学生个人信息的数据表。因为在校学生数量也不是特别巨大(8W?)，每个学生的学号是唯一的,因此，我们可以简单的应用直接定址法，声明一个10W大小的数组，每个学生的学号作为主键。然后每次要添加或者查找学生，只需要根据需要去操作即可。

      但是，显然这样做是**很脑残**的。这样做系统的可拓展性和复用性就非常差了，比如有一天人数超过10W了？如果是用来保存别的数据呢？或者我只需要保存20条记录呢？声明大小为10W的数组显然是太浪费了的。

 

     如果我们是用来保存大数据量（比如银行的用户数，4大的用户数都应该有3-5亿了吧？），这时候我们计算出来的HashCode就很可能会有冲突了， 我们的系统应该有“处理冲突”的能力，此处我们**通过挂链法****“****处理冲突****”**。

 

     如果我们的数据量非常巨大，并且还持续在增加，如果我们仅仅只是通过挂链法来处理冲突，可能我们的链上挂了上万个数据后，这个时候再通过静态搜索来查找链表，显然性能也是非常低的。所以我们的系统应该还能实现自动扩容，**当容量达到某比例后，即自动扩容，使装载因子保存在一个固定的水平上**。

 

综上所述，我们对这个Hash容器的基本要求应该有如下几点：

             **满足****Hash****表的查找要求（废话）**

**             ****能支持从小数据量到大数据量的自动转变（自动扩容）**

**             ****使用挂链法解决冲突**

 

 

好了，既然都分析到这一步了，咱就闲话少叙，直接开始上代码吧。

 

**Java****代码**** **** **[**![收藏代码]()**]( ""收藏这段代码" ")****
1.   **package** cn.javamzd.collection.search;  

2.     

3.   **public** **class** MyMap<K, V> {  

4.       **private** **int** size;// 当前容量  

5.       **private** **static** **int** INIT_CAPACITY = 16;// 默认容量  

6.       **private** Entry<K, V>[] container;// 实际存储数据的数组对象  

7.       **private** **static** **float** LOAD_FACTOR = 0.75f;// 装载因子  

8.       **private** **int** max;// 能存的最大的数=capacity*factor  

9.     

10.     // 自己设置容量和装载因子的构造器  

11.     **public** MyMap(**int** init_Capaticy, **float** load_factor) {  

12.         **if** (init_Capaticy < 0)  

13.             **throw** **new** IllegalArgumentException("Illegal initial capacity: "  

14.                     + init_Capaticy);  

15.         **if** (load_factor <= 0 || Float.isNaN(load_factor))  

16.             **throw** **new** IllegalArgumentException("Illegal load factor: "  

17.                     + load_factor);  

18.         **this**.LOAD_FACTOR = load_factor;  

19.         max = (**int**) (init_Capaticy * load_factor);  

20.         container = **new** Entry[init_Capaticy];  

21.     }  

22.   

23.     // 使用默认参数的构造器  

24.     **public** MyMap() {  

25.         **this**(INIT_CAPACITY, LOAD_FACTOR);  

26.     }  

27.   

28.     /** 

29.      * 存 

30.      *  

31.      * @param k 

32.      * @param v 

33.      * @return 

34.      */  

35.     **public** **boolean** put(K k, V v) {  

36.         // 1.计算K的hash值  

37.         // 因为自己很难写出对不同的类型都适用的Hash算法，故调用JDK给出的hashCode()方法来计算hash值  

38.         **int** hash = k.hashCode();  

39.         //将所有信息封装为一个Entry  

40.         Entry<K,V> temp=**new** Entry(k,v,hash);  

41.             **if**(setEntry(temp, container)){  

42.                 // 大小加一  

43.                 size++;  

44.                 **return** **true**;  

45.             }  

46.             **return** **false**;  

47.     }  

48.   

49.   

50.     /** 

51.      * 扩容的方法 

52.      *  

53.      * @param newSize 

54.      *            新的容器大小 

55.      */  

56.     **private** **void** reSize(**int** newSize) {  

57.         // 1.声明新数组  

58.         Entry<K, V>[] newTable = **new** Entry[newSize];  

59.         max = (**int**) (newSize * LOAD_FACTOR);  

60.         // 2.复制已有元素,即遍历所有元素，每个元素再存一遍  

61.         **for** (**int** j = 0; j < container.length; j++) {  

62.             Entry<K, V> entry = container[j];  

63.             //因为每个数组元素其实为链表，所以…………  

64.             **while** (**null** != entry) {  

65.                 setEntry(entry, newTable);  

66.                 entry = entry.next;  

67.             }  

68.         }  

69.         // 3.改变指向  

70.         container = newTable;  

71.           

72.     }  

73.       

74.     /** 

75.      *将指定的结点temp添加到指定的hash表table当中 

76.      * 添加时判断该结点是否已经存在 

77.      * 如果已经存在，返回false 

78.      * 添加成功返回true 

79.      * @param temp 

80.      * @param table 

81.      * @return 

82.      */  

83.     **private** **boolean** setEntry(Entry<K,V> temp,Entry[] table){  

84.         // 根据hash值找到下标  

85.         **int** index = indexFor(temp.hash, table.length);  

86.         //根据下标找到对应元素  

87.         Entry<K, V> entry = table[index];  

88.         // 3.若存在  

89.         **if** (**null** != entry) {  

90.             // 3.1遍历整个链表，判断是否相等  

91.             **while** (**null** != entry) {  

92.                 //判断相等的条件时应该注意，除了比较地址相同外，引用传递的相等用equals()方法比较  

93.                 //相等则不存，返回false  

94.                 **if** ((temp.key == entry.key||temp.key.equals(entry.key)) && temp.hash == entry.hash&&(temp.value==entry.value||temp.value.equals(entry.value))) {  

95.                     **return** **false**;  

96.                 }  

97.                 //不相等则比较下一个元素  

98.                 **else** **if** (temp.key != entry.key && temp.value != entry.value) {  

99.                         //到达队尾，中断循环  

100.                         **if**(**null**==entry.next){  

101.                             **break**;  

102.                         }  

103.                         // 没有到达队尾，继续遍历下一个元素  

104.                         entry = entry.next;  

105.                 }  

106.             }  

107.             // 3.2当遍历到了队尾，如果都没有相同的元素，则将该元素挂在队尾  

108.             addEntry2Last(entry,temp);  

109.                   

110.         }  

111.         // 4.若不存在,直接设置初始化元素  

112.         setFirstEntry(temp,index,table);  

113.         **return** **true**;  

114.     }  

115.       

116.     **private** **void** addEntry2Last(Entry<K, V> entry, Entry<K, V> temp) {  

117.         **if** (size > max) {  

118.             reSize(container.length * 4);  

119.         }  

120.         entry.next=temp;  

121.           

122.     }  

123.   

124.     /** 

125.      * 将指定结点temp，添加到指定的hash表table的指定下标index中 

126.      * @param temp 

127.      * @param index 

128.      * @param table 

129.      */  

130.     **private** **void** setFirstEntry(Entry<K, V> temp, **int** index, Entry[] table) {  

131.         // 1.判断当前容量是否超标，如果超标，调用扩容方法  

132.         **if** (size > max) {  

133.             reSize(table.length * 4);  

134.         }  

135.         // 2.不超标，或者扩容以后，设置元素  

136.         table[index] = temp;  

137.         //！！！！！！！！！！！！！！！  

138.         //因为每次设置后都是新的链表，需要将其后接的结点都去掉  

139.          //NND，少这一行代码卡了哥哥7个小时（代码重构）  

140.         temp.next=**null**;  

141.     }  

142.   

143.     /** 

144.      * 取 

145.      *  

146.      * @param k 

147.      * @return 

148.      */  

149.     **public** V get(K k) {  

150.         Entry<K, V> entry = **null**;  

151.         // 1.计算K的hash值  

152.         **int** hash = k.hashCode();  

153.         // 2.根据hash值找到下标  

154.         **int** index = indexFor(hash, container.length);  

155.         // 3。根据index找到链表  

156.         entry = container[index];  

157.         // 3。若链表为空，返回null  

158.         **if** (**null** == entry) {  

159.             **return** **null**;  

160.         }  

161.         // 4。若不为空，遍历链表，比较k是否相等,如果k相等，则返回该value  

162.         **while** (**null** != entry) {  

163.             **if** (k == entry.key||entry.key.equals(k)) {  

164.                 **return** entry.value;  

165.             }  

166.             entry = entry.next;  

167.         }  

168.         // 如果遍历完了不相等，则返回空  

169.         **return** **null**;  

170.   

171.     }  

172.   

173.     /** 

174.      * 根据hash码，容器数组的长度,计算该哈希码在容器数组中的下标值 

175.      *  

176.      * @param hashcode 

177.      * @param containerLength 

178.      * @return 

179.      */  

180.     **public** **int** indexFor(**int** hashcode, **int** containerLength) {  

181.         **return** hashcode & (containerLength - 1);  

182.   

183.     }  

184.   

185.     /** 

186.      * 用来实际保存数据的内部类,因为采用挂链法解决冲突，此内部类设计为链表形式 

187.      *  

188.      * @param <K>key 

189.      * @param <V> 

190.      *            value 

191.      */  

192.     **class** Entry<K, V> {  

193.         Entry<K, V> next;// 下一个结点  

194.         K key;// key  

195.         V value;// value  

196.         **int** hash;// 这个key对应的hash码，作为一个成员变量，当下次需要用的时候可以不用重新计算  

197.   

198.         // 构造方法  

199.         Entry(K k, V v, **int** hash) {  

200.             **this**.key = k;  

201.             **this**.value = v;  

202.             **this**.hash = hash;  

203.   

204.         }  

205.   

206.         //相应的getter()方法  

207.   

208.     }  

209. }  

 

 代码中有相当清楚的注释了

 

在文章的最后这里，我要强烈的宣泄下感情

MLGBD，本来以为分析的挺到位了，写出这个东西也就最多需要个把小时吧

结果因为通宵作业，脑袋运转不灵

硬是花了哥三个小时才写出了

好不容易些出来了

我日

看着代码比较混乱

然后就对代码重构了下

把逻辑抽象清楚，进行重构就花了个多小时

好不容易构造好了

就开始了TMD的一直报错了----------大数据量测试时到大概5000就死循环了

各种调试，各种分析都觉得没错误

 最后花了哥7个小时终于找出来了

我擦

 

第一次初始化加的时候，因为每个元素的next都是空的

而扩充容量resize()时，因为冲突处理是链式结构的

当将他们重新hash添加的时候，重复的这些鸟元素的next是有元素的

一定要设置为null

 

 

**七****.****性能分析：**

      1.因为冲突的存在，其查找长度不可能达到O(1)

      2哈希表的平均查找长度是装载因子a 的函数，而不是 n 的函数。

      3.用哈希表构造查找表时，可以选择一个适当的装填因子 a ，使得平均查找长度限定在某个范围内。

  
   最后给出我们这个HashMap的性能

  测试代码

**Java****代码**** **** **[**![收藏代码]()**]( ""收藏这段代码" ")****
1.   **public** **class** Test {  

2.     

3.       **public** **static** **void** main(String[] args) {  

4.           MyMap<String, String> mm = **new** MyMap<String, String>();   

5.           Long aBeginTime=System.currentTimeMillis();//记录BeginTime  

6.           **for**(**int** i=0;i<1000000;i++){  

7.           mm.put(""+i, ""+i*100);  

8.           }  

9.           Long aEndTime=System.currentTimeMillis();//记录EndTime  

10.         System.out.println("insert time-->"+(aEndTime-aBeginTime));  

11.           

12.         Long lBeginTime=System.currentTimeMillis();//记录BeginTime  

13.         mm.get(""+100000);  

14.         Long lEndTime=System.currentTimeMillis();//记录EndTime  

15.         System.out.println("seach time--->"+(lEndTime-lBeginTime));  

16.     }  

17. }  

  100W个数据时，全部存储时间为1S多一点，而**搜寻时间为****0 **

insert time-->1536
seach time--->0

 

 

好了，牢骚发完了

本来今天想写个**有关大访问量处理的一些基本概念**的文章

全泡汤了,明天写吧

 

url: http://java-mzd.iteye.com/blog/827523
