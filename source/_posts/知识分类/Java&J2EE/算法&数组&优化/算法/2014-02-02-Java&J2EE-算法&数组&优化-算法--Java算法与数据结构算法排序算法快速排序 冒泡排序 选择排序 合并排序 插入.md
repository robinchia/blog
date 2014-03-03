---
layout: post
title: "Java算法与数据结构 算法 排序算法 快速排序   冒泡排序   选择排序   合并排序   插入"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 算法
--- 

# Java算法与数据结构 算法 排序算法 快速排序   冒泡排序   选择排序   合并排序   插入排序     数据结构 单向链表   栈

### [Java算法与数据结构](http://tangyanbo.iteye.com/blog/1470412)**

**算法**

****

**排序算法**

****

****

****

[快速排序](http://tangyanbo.iteye.com/admin/blogs/1470417)

[冒泡排序](http://tangyanbo.iteye.com/admin/blogs/1470870)

[选择排序](http://tangyanbo.iteye.com/admin/blogs/1470871)

[合并排序](http://tangyanbo.iteye.com/admin/blogs/1470872)

[插入排序](http://tangyanbo.iteye.com/admin/blogs/1472367)

**数据结构**

[单向链表](http://tangyanbo.iteye.com/admin/blogs/1472811) 

[栈](http://tangyanbo.iteye.com/admin/blogs/1472830)

         

### [快速排序](http://tangyanbo.iteye.com/blog/1470417)**

**博客分类：** 
* [排序算法](http://tangyanbo.iteye.com/category/214584)

**交换排序法->快速排序 **
快速排序使用分治法策略来把一个序列分为两个子序列 
算法步骤： 
1. 从数列中挑出一个元素，称为 "基准"（pivot） 
2. 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面 （相同的数可以到任一边）。在这个分割结束之后，该基准就处于数列的中间位置。这个称为分割（partition）操作 
3. 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序 
* 比较复杂度：O(n㏒n) 
* 交换(赋值)复杂度：O(n㏒n) 
* 优点：比一般的排序都要快 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. public static void quickSort(Integer[] array) {     
1.     if (array == null || array.length== 0) {     
1.          return;     
1.     }     
1.     quickSort(array,0,array.length-1);           
1. }  

 

Java代码  [![收藏代码]()]( "收藏这段代码")

1. private static void quickSort(Integer[] array, final int start, final int end){  
1.         //数组长度<=1退出  
1.         if(start>=end){  
1.             return;  
1.         }  
1.         //数组长度==2,比较两个元素的大小  
1.         if(end-start==1){  
1.             if(array[start]>array[end]){  
1.                 swap(array,start,end);  
1.             }  
1.             return;  
1.         }  
1.           
1.         //用来进行比较的数  
1.         int compareNumber = array[start];  
1.         int middlePosition = 0;  
1.         int i = start;  
1.         int j = end;  
1.         for(;;i++,j--){  
1.               
1.             //从数组首端开始迭代(不包括compareNumber)，如果数组的数<compareNumber，不做移动  
1.             while(array[i]<compareNumber&&i<j){  
1.                 i++;  
1.             }         
1.             //从数组尾端迭代，如果数组的数>=compareNumber，不做移动  
1.               
1.             while(array[j]>compareNumber&&i<j){  
1.                 j--;  
1.             }             
1.               
1.             if(i>=j){  
1.                 if(array[j]>compareNumber){  
1.                     middlePosition = j;  
1.                 }else{  
1.                     middlePosition = (j+1);  
1.                 }  
1.                 break;  
1.             }  
1.             //从数组首端开始迭代，得到大于compareNumber的数array[i]，此时从尾端迭代直至得到<=compareNumber的数  
1.             //array[j],交换这两个数的位置，然后继续迭代  
1.             swap(array,i,j);  
1.         }  
1.         //递归排序  
1.         quickSort(array,start,middlePosition-1);  
1.         quickSort(array,middlePosition,end);  
1.     }  

 

Java代码  [![收藏代码]()]( "收藏这段代码")

1. public static void swap(Object[] array,int a,int b){  
1.     Object temp = array[a];  
1.     array[a] = array[b];  
1.     array[b] = temp;  
1. }  
 
### [冒泡排序](http://tangyanbo.iteye.com/blog/1470870)**

**博客分类：** 
* [排序算法](http://tangyanbo.iteye.com/category/214584)
[数据结构](http://www.iteye.com/blogs/tag/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)

**交换排序->冒泡排序 **
算法步骤： 
1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。 
2. 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。 
3. 针对所有的元素重复以上的步骤，除了最后一个。 
4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. public static void bubbleSort(Integer[] array){  
1.         for(int i=array.length-1;i>=0;i--){  
1.             for(int j=0;j<i;j++){  
1.                 if(array[j]>array[j+1]){  
1.                     swap(array, j, j+1);  
1.                 }  
1.             }  
1.         }  
1.     }  
Java代码  [![收藏代码]()]( "收藏这段代码")

1. public static void swap(Object[] array,int a,int b){  
1.         Object temp = array[a];  
1.         array[a] = array[b];  
1.         array[b] = temp;  
1.     }  
来源： <[http://tangyanbo.iteye.com/blog/1470870](http://tangyanbo.iteye.com/blog/1470870)> 
### [选择排序](http://tangyanbo.iteye.com/blog/1470871)**

**博客分类：** 
* [排序算法](http://tangyanbo.iteye.com/category/214584)
[数据结构](http://www.iteye.com/blogs/tag/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)

**选择排序法->选择排序** 
算法步骤： 
1. 未排序序列中找到最小元素，存放到排序序列的起始位置 
2. 再从剩余未排序元素中继续寻找最小元素，然后放到排序序列末尾 
3. 以此类推,直到所有元素均排序完毕 
比较复杂度：n(n-1)/2 
交换(赋值)复杂度：n-1 
优点：相比冒泡排序来讲，交换的次数减少了 
缺点：相对快速排序，比较次数仍然是n² 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. public static void selectionSort(Integer[] array){  
1.         for(int i=0;i<array.length-1;i++){  
1.             //最小数存放位置  
1.             int minPosition = i;  
1.             for(int j=i+1;j<array.length;j++){  
1.                 if(array[j]<array[minPosition]){  
1.                     //新的最小数位置  
1.                     minPosition = j;  
1.                 }  
1.             }  
1.             //找到剩余元素中的最小数，并将其交换至起始位置  
1.             swap(array, i, minPosition);  
1.         }  
1.           
1.     }  
Java代码  [![收藏代码]()]( "收藏这段代码")

1. public static void swap(Object[] array,int a,int b){  
1.         Object temp = array[a];  
1.         array[a] = array[b];  
1.         array[b] = temp;  
1.     }  
来源： <[http://tangyanbo.iteye.com/blog/1470871](http://tangyanbo.iteye.com/blog/1470871)> 
### [插入排序](http://tangyanbo.iteye.com/blog/1472367)**

**博客分类：** 
* [排序算法](http://tangyanbo.iteye.com/category/214584)

插入排序 
算法步骤： 
1.从第一个元素开始，该元素可以认为已经被排序 
2.取出下一个元素a，在已经排序的元素序列中从后向前扫描 
3.如果已排序中的元素b大于a，则将元素b后移一个位置 
4.重复步骤3，直到找到已排序的元素x小于或者等于元素a 
5.将元素a插入到x的后面 
6.重复步骤2~5 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. public static void insertionSort(Integer[] array){  
1.         for(int i=1;i<array.length;i++){  
1.             //待插入的数据  
1.             Integer toBeInsertedValue = array[i];  
1.             int j;  
1.             for(j=i;j>0;j--){  
1.                 if(array[j-1]>toBeInsertedValue){  
1.                     //将比toBeInsertedValue大的元素全部后移  
1.                     array[j]=array[j-1];  
1.                     continue;  
1.                 }  
1.                 break;  
1.             }  
1.             //插入新元素  
1.             array[j]=toBeInsertedValue;  
1.         }  
1.     }  
来源： <[http://tangyanbo.iteye.com/blog/1472367](http://tangyanbo.iteye.com/blog/1472367)> 
