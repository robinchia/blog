---
layout: post
title: "线性表分析及Java实现"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - java深入分析
--- 

# 线性表分析及Java实现

**[线性表分析及Java实现](http://java-mzd.iteye.com/blog/826059)**

**文章分类****:[综合技术](http://www.iteye.com/blogs/category/tech)**

      数据结构中的线性表，对应着Collection中的List接口。

      在本节中，我们将做以下三件事

            第一。我们先来看看线性表的特征

            第二，自己用JAVA实现List

            第三，对比的线性表、链式表性能，以及自己的List性能与JDKList性能对比

 

    ** ****线性表特征：**** **

            第一，一个特定的线性表，应该是用来存放特定的某一个类型的元素的（元素的“同一性”）

            第二， 除第一个元素外，其他每一个元素有且仅有一个直接前驱；除最后一个元素外，其他每一个元素有且仅有一个             直接后继（元素的“序偶性”）

            第二， 元素在线性表中的“下标”唯一地确定该元素在表中的相对位置（元素的“索引性”）

       又，一.线性表只是数据的一种逻辑结构，其具体存储结构可以为顺序存储结构和链式储存结构来完成，对应可以得到顺序表和链表，

            二.对线性表的入表和出表顺序做一定的限定，可以得到特殊的线性表，栈(FILO)和队列（FIFO）

 

 

**   ****自己实现线性表之顺序表**

             思路：

                1. 顺序表因为采用顺序存储形式，所以内部使用数组来存储数据

                2.因为存储的具体对象类型不一定，所以采用泛型操作

                3.数组操作优点：1.通过指针快速定位到下表，查询快速

                               缺点：1.数组声明时即需要确定数组大小。当操作中超过容量时，则需要重新声明数组，并且复制当前所有数据

                                        2.当需要在中间进行插入或者删除时，则需要移动大量元素（size-index个）

  具体实现代码如下

**Java****代码**** **** **[**![收藏代码]()**]( ""收藏这段代码" ")****
1.   /** 

2.    * 自己用数组实现的线性表 

3.    */  

4.   **public** **class** ArrayList<E> {  

5.       Object[] data = **null**;// 用来保存此队列中内容的数组  

6.       **int** current;// 保存当前为第几个元素的指标  

7.       **int** capacity;// 表示数组大小的指标  

8.          

9.       /** 

10.      * 如果初始化时，未声明大小，则默认为10 

11.      */  

12.     **public** ArrayList() {  

13.         **this**(10);  

14.     }  

15.   

16.     /** 

17.      * 初始化线性表，并且声明保存内容的数组大小 

18.      * @param initalSize 

19.      */  

20.     **public** ArrayList(**int** initalSize) {  

21.         **if** (initalSize < 0) {  

22.             **throw** **new** RuntimeException("数组大小错误:" + initalSize);  

23.         } **else** {  

24.             **this**.data = **new** Object[initalSize];  

25.             **this**.current = 0;  

26.             capacity = initalSize;  

27.         }  

28.     }  

29.   

30.     /** 

31.      * 添加元素的方法 添加前，先确认是否已经满了 

32.      * @param e 

33.      * @return 

34.      */  

35.     **public** **boolean** add(E e) {  

36.         ensureCapacity(current);// 确认容量  

37.         **this**.data[current] = e;  

38.         current++;  

39.         **return** **true**;  

40.     }  

41.   

42.     /** 

43.      * 确认系统当前容量是否满足需要,如果满足，则不执行操作 如果不满足，增加容量 

44.      * @param cur 当前个数 

45.      */  

46.     **private** **void** ensureCapacity(**int** cur) {  

47.         **if** (cur == capacity) {  

48.             // 如果达到容量极限，增加10的容量，复制当前数组  

49.             **this**.capacity = **this**.capacity + 10;  

50.             Object[] newdata = **new** Object[capacity];  

51.             **for** (**int** i = 0; i < cur; i++) {  

52.                 newdata[i] = **this**.data[i];  

53.             }  

54.             **this**.data = newdata;  

55.         }  

56.     }  

57.   

58.     /** 

59.      * 得到指定下标的数据 

60.      * @param index 

61.      * @return 

62.      */  

63.     **public** E get(**int** index) {  

64.         validateIndex(index);  

65.         **return** (E) **this**.data[index];  

66.     }  

67.        

68.    /** 

69.     * 返回当前队列大小 

70.     * @return 

71.     */  

72.     **public** **int** size() {  

73.         **return** **this**.current;  

74.     }  

75.   

76.     /** 

77.      * 更改指定下标元素的数据为e 

78.      * @param index  

79.      * @param e 

80.      * @return 

81.      */  

82.     **public** **boolean** set(**int** index, E e) {  

83.         validateIndex(index);  

84.         **this**.data[index] = e;  

85.         **return** **true**;  

86.     }  

87.      

88.     /** 

89.      *  验证当前下标是否合法，如果不合法，抛出运行时异常 

90.      * @param index 下标 

91.      */  

92.     **private** **void** validateIndex(**int** index) {  

93.         **if** (index < 0 || index > current) {  

94.             **throw** **new** RuntimeException("数组index错误：" + index);  

95.         }  

96.     }  

97.   

98.     /** 

99.      * 在指定下标位置处插入数据e 

100.      * @param index 下标 

101.      * @param e 需要插入的数据 

102.      * @return  

103.      */  

104.     **public** **boolean** insert(**int** index, E e) {  

105.         validateIndex(index);  

106.         Object[] tem = **new** Object[capacity];// 用一个临时数组作为备份  

107.         //开始备份数组  

108.         **for** (**int** i = 0; i < current; i++) {  

109.             **if** (i < index) {  

110.                 tem[i] = **this**.data[i];  

111.             }**else** **if**(i==index){  

112.                 tem[i]=e;  

113.             }**else** **if**(i>index){  

114.                 tem[i]=**this**.data[i-1];  

115.             }  

116.         }  

117.         **this**.data=tem;  

118.         **return** **true**;  

119.     }  

120. <br><br>/**<br>  * 删除指定下标出的数据<br>    * @param index<br>  * @return<br>   */<br> **public** **boolean** delete(**int** index){<br>       validateIndex(index);<br>       Object[] tem = **new** Object[capacity];// 用一个临时数组作为备份<br>      //开始备份数组<br>        for (int i = 0; i < current; i++) {<br>          if (i < index) {<br>             tem[i] = this.data[i];<br>          }else if(i==index){<br>             tem[i]=this.data[i+1];<br>          }else if(i>index){<br>               tem[i]=this.data[i+1];<br>          }<br>       }<br>       this.data=tem;<br>      return true;<br>    }<br><br>}  

 

  ** ****自己实现线性表之链表**

         思路：1.链表采用链式存储结构，在内部只需要将一个一个结点链接起来。（每个结点中有关于此结点下一个结点的引用）

         链表操作优点：1.，因为每个结点记录下个结点的引用，则在进行插入和删除操作时，只需要改变对应下标下结点的引用即可

                     缺点：1.要得到某个下标的数据，不能通过下标直接得到，需要遍历整个链表。

 

 

  实现代码如下

**Java****代码**** **** **[**![收藏代码]()**]( ""收藏这段代码" ")****
1.   /** 

2.    * 自己用链式存储实现的线性表 

3.    */  

4.   **public** **class** LinkedList<E> {  

5.     

6.       **private** Node<E> header = **null**;// 头结点  

7.       **int** size = 0;// 表示数组大小的指标  

8.     

9.       **public** LinkedList() {  

10.         **this**.header = **new** Node<E>();  

11.     }  

12.   

13.     **public** **boolean** add(E e) {  

14.         **if** (size == 0) {  

15.             header.e = e;  

16.         } **else** {  

17.             // 根据需要添加的内容，封装为结点  

18.             Node<E> newNode = **new** Node<E>(e);  

19.             // 得到当前最后一个结点  

20.             Node<E> last = getNode(size-1);  

21.             // 在最后一个结点后加上新结点  

22.             last.addNext(newNode);  

23.         }  

24.         size++;// 当前大小自增加1  

25.         **return** **true**;  

26.     }  

27.   

28.     **public** **boolean** insert(**int** index, E e) {  

29.         Node<E> newNode = **new** Node<E>(e);  

30.         // 得到第N个结点  

31.         Node<E> cNode = getNode(index);  

32.         newNode.next = cNode.next;  

33.         cNode.next = newNode;  

34.         size++;  

35.         **return** **true**;  

36.   

37.     }  

38.   

39.     /** 

40.      * 遍历当前链表，取得当前索引对应的元素 

41.      *  

42.      * @return 

43.      */  

44.     **private** Node<E> getNode(**int** index) {  

45.         // 先判断索引正确性  

46.         **if** (index > size || index < 0) {  

47.             **throw** **new** RuntimeException("索引值有错：" + index);  

48.         }  

49.         Node<E> tem = **new** Node<E>();  

50.         tem = header;  

51.         **int** count = 0;  

52.         **while** (count != index) {  

53.             tem = tem.next;  

54.             count++;  

55.         }  

56.         **return** tem;  

57.     }  

58.   

59.     /** 

60.      * 根据索引，取得该索引下的数据 

61.      *  

62.      * @param index 

63.      * @return 

64.      */  

65.     **public** E get(**int** index) {  

66.         // 先判断索引正确性  

67.         **if** (index >= size || index < 0) {  

68.             **throw** **new** RuntimeException("索引值有错：" + index);  

69.         }  

70.         Node<E> tem = **new** Node<E>();  

71.         tem = header;  

72.         **int** count = 0;  

73.         **while** (count != index) {  

74.             tem = tem.next;  

75.             count++;  

76.         }  

77.         E e = tem.e;  

78.         **return** e;  

79.     }  

80.   

81.     **public** **int** size() {  

82.         **return** size;  

83.     }  

84.   

85.     /** 

86.      * 设置第N个结点的值 

87.      *  

88.      * @param x 

89.      * @param e 

90.      * @return 

91.      */  

92.     **public** **boolean** set(**int** index, E e) {  

93.         // 先判断索引正确性  

94.         **if** (index > size || index < 0) {  

95.             **throw** **new** RuntimeException("索引值有错：" + index);  

96.         }  

97.         Node<E> newNode = **new** Node<E>(e);  

98.         // 得到第x个结点  

99.         Node<E> cNode = getNode(index);  

100.         cNode.e = e;  

101.         **return** **true**;  

102.     }  

103.   

104.     /** 

105.      * 用来存放数据的结点型内部类 

106.      */  

107.     **class** Node<e> {  

108.         **private** E e;// 结点中存放的数据  

109.   

110.         Node() {  

111.         }  

112.   

113.         Node(E e) {  

114.             **this**.e = e;  

115.         }  

116.   

117.         Node<E> next;// 用来指向该结点的下一个结点  

118.   

119.         // 在此结点后加一个结点  

120.         **void** addNext(Node<E> node) {  

121.             next = node;  

122.         }  

123.     }  

124.   

125. }  

 

**自己实现线性表之栈**

         栈是限定仅允许在表的同一端（通常为“表尾”）进行插入或删除操作的线性表。

         允许插入和删除的一端称为栈顶(top)，另一端称为栈底(base)
         特点：后进先出 (LIFO)或，先进后出（FILO）

 

         因为栈是限定线的线性表，所以，我们可以调用前面两种线性表，只需要对出栈和入栈操作进行设定即可

    具体实现代码

**Java****代码**** **** **[**![收藏代码]()**]( ""收藏这段代码" ")****
1.   /** 

2.    * 自己用数组实现的栈 

3.    */  

4.   **public** **class** ArrayStack<E> {  

5.         **private** ArrayList<E> list=**new** ArrayList<E>();//用来保存数据线性表<br>    private  int size;//表示当前栈元素个数  

6.         /** 

7.          * 入栈操作 

8.          * @param e 

9.          */  

10.       **public** **void** push(E e){  

11.           list.add(e);  

12.           size++;  

13.       }  

14.        

15.       /** 

16.        * 出栈操作 

17.        * @return 

18.        */  

19.       **public** E pop(){  

20.          E e= list.get(size-1);  

21.          size--;  

22.          **return** e;  

23.       }  

24.   

25. }  

 

 至于用链表实现栈，则只需要把保存数据的顺序表改成链表即可，此处就不给出代码了

 

**自己实现线性表之队列**

        与栈类似

        队列是只允许在表的一端进行插入，而在另一端删除元素的线性表。

        在队列中，允许插入的一端叫队尾（rear），允许删除的一端称为队头(front)。
        特点：先进先出 (FIFO)、后进后出 (LILO)

 

       同理，我们也可以调用前面两种线性表，只需要对队列的入队和出队方式进行处理即可

 

**Java****代码**** **** **[**![收藏代码]()**]( ""收藏这段代码" ")****
1.   **package** cn.javamzd.collection.List;  

2.     

3.   /** 

4.    * 用数组实现的队列 

5.    */  

6.   **public** **class** ArrayQueue<E> {  

7.       **private** ArrayList<E> list = **new** ArrayList<E>();// 用来保存数据的队列  

8.       **private** **int** size;// 表示当前栈元素个数  

9.     

10.     /** 

11.      * 入队 

12.      * @param e 

13.      */  

14.     **public** **void** EnQueue(E e) {  

15.         list.add(e);  

16.         size++;  

17.     }  

18.   

19.     /** 

20.      * 出队 

21.      * @return 

22.      */  

23.     **public** E DeQueue() {  

24.         **if** (size > 0) {  

25.             E e = list.get(0);  

26.             list.delete(0);  

27.             **return** e;  

28.         }**else**{  

29.             **throw** **new** RuntimeException("已经到达队列顶部");  

30.         }  

31.     }  

32. }  

 

 

       
**对比线性表和链式表**
         前面已经说过顺序表和链式表各自的特点，这里在重申一遍

         数组操作优点：1.通过指针快速定位到下标，查询快速

                     缺点：1.数组声明时即需要确定数组大小。当操作中超过容量时，则需要重新声明数组，并且复制当前所有数据

                              2.当需要在中间进行插入或者删除时，则需要移动大量元素（size-index个）    

 

 

         链表操作优点：1.，因为每个结点记录下个结点的引用，则在进行插入和删除操作时，只需要改变对应下标下结点的引用即可

                     缺点：1.要得到某个下标的数据，不能通过下标直接得到，需要遍历整个链表。

 

         现在，我们通过进行增删改查操作来感受一次其效率的差异

         **思路**：通过两个表，各进行大数据量操作（2W）条数据的操作，记录操作前系统时间，操作后系统时间，得出操作时间

  实现代码如下

 

**Java****代码**** **** **[**![收藏代码]()**]( ""收藏这段代码" ")****
1.   **package** cn.javamzd.collection.List;  

2.     

3.   **public** **class** Test {  

4.     

5.       /** 

6.        * @param args 

7.        */  

8.       **public** **static** **void** main(String[] args) {  

9.           //测试自己实现的ArrayList类和Linkedlist类添加20000个数据所需要的时间  

10.         ArrayList<String> al = **new** ArrayList<String>();  

11.         LinkedList<String> ll = **new** LinkedList<String>();  

12.         Long aBeginTime=System.currentTimeMillis();//记录BeginTime  

13.         **for**(**int** i=0;i<30000;i++){  

14.             al.add("now"+i);  

15.         }  

16.         Long aEndTime=System.currentTimeMillis();//记录EndTime  

17.         System.out.println("arrylist  add time--->"+(aEndTime-aBeginTime));  

18.         Long lBeginTime=System.currentTimeMillis();//记录BeginTime  

19.         **for**(**int** i=0;i<30000;i++){  

20.             ll.add("now"+i);  

21.         }  

22.         Long lEndTime=System.currentTimeMillis();//记录EndTime  

23.         System.out.println("linkedList add time---->"+(lEndTime-lBeginTime));  

24.           

25.         //测试JDK提供的ArrayList类和LinkedList类添加20000个数据所需要的世界  

26.         java.util.ArrayList<String> sal=**new** java.util.ArrayList<String>();  

27.         java.util.LinkedList<String> sll=**new** java.util.LinkedList<String>();  

28.         Long saBeginTime=System.currentTimeMillis();//记录BeginTime  

29.         **for**(**int** i=0;i<30000;i++){  

30.             sal.add("now"+i);  

31.         }  

32.         Long saEndTime=System.currentTimeMillis();//记录EndTime  

33.         System.out.println("JDK arrylist  add time--->"+(saEndTime-saBeginTime));  

34.         Long slBeginTime=System.currentTimeMillis();//记录BeginTime  

35.         **for**(**int** i=0;i<30000;i++){  

36.             sll.add("now"+i);  

37.         }  

38.         Long slEndTime=System.currentTimeMillis();//记录EndTime  

39.         System.out.println("JDK linkedList add time---->"+(slEndTime-slBeginTime));  

40.     }  

41.   

42. }  

  得到测试结果如下： 

arrylist add time--->446
linkedList add time---->9767
JDK arrylist add time--->13
JDK linkedList add time---->12

        由以上数据，我们可知：

**           1.JDK****中的****ArrayList****何****LinkedList****在添加数据时的性能，其实几乎是没有差异的**

           2.我们自己写的List的性能和JDK提供的List的性能还是存在巨大差异的

           3.我们使用链表添加操作，花费的时间是巨大的，比ArrayList都大几十倍

 

      第三条显然是跟我们最初的设计不相符的，按照我们最初的设想，链表的添加应该比顺序表更省时

      查看我们写的源码，可以发现：

      我们每次添加一个数据时，都需要遍历整个表，得到表尾，再在表尾添加，这是很不科学的

 

     ** ****现改进如下**：设立一个Node<E>类的成员变量end来指示表尾，这样每次添加时，就不需要再重新遍历得到表尾

      改进后add()方法如下

 

**Java****代码**** **** **[**![收藏代码]()**]( ""收藏这段代码" ")****
1.   **public** **boolean** add(E e) {  

2.       **if** (size == 0) {  

3.           header.e = e;  

4.       } **else** {  

5.           // 根据需要添加的内容，封装为结点  

6.           Node<E> newNode = **new** Node<E>(e);  

7.           //在表尾添加元素  

8.           last.addNext(newNode);  

9.                                           //将表尾指向当前最后一个元素  

10.         last = newNode;  

11.     }  

12.     size++;// 当前大小自增加1  

13.     **return** **true**;  

14. }  

 

       ArrayList添加的效率和JDK中对比起来也太低

       分析原因为：

       每次扩大容量时，扩大量太小，需要进行的复制操作太多

       现在改进如下：

       每次扩大，则扩大容量为当前的三倍，此改进仅需要更改ensureCapacity()方法中的一行代码，此处就不列出了。

 

改进后，再次运行添加元素测试代码，结果如下：

 
arrylist add time--->16
linkedList add time---->8
JDK arrylist add time--->7
JDK linkedList add time---->7

 虽然还有改进的空间，但是显然，我们的效果已经大幅度改进了，而且也比较接近JDK了

 

 

接下来测试插入操作的效率

  我们只需要将测试代码中的添加方法(add())改成插入方法(insert(int index,E e)),为了使插入次数尽可能多，我们把index都设置为0

测试结果如下：

 
arrylist inset time--->17
linkedList inset time---->13
JDK arrylist inset time--->503
JDK linkedList inset time---->11

**多次测试，发现我们写的****ArrayList****在插入方法的效率都已经超过****JDK****了，而且也接近****LinkedLst****了。撒花！！！**

 

接下来测试删除、得到下标等等操作就不一一列出来了（只需要改变每次调用的方法即可）

 

 

 

 

恩，本来想今晚把所有的集合框架实现都写一下的

但是不知不觉这都又2点了

明早还得去蓝杰上课

果断先睡吧

敬请大家期待我明日大作------------静态/动态查找表的实现，动态查找表查找/加入算法的JAVA实现，Hash表的实现

 

good night

 

url: http://java-mzd.iteye.com/blog/826059
