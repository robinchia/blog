---
layout: post
title: "直接拿来用！超实用的Java数组技巧攻略"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 数组&集合
--- 

# 直接拿来用！超实用的Java数组技巧攻略

# 直接拿来用！超实用的Java数组技巧攻略

本文分享了关于Java数组最顶级的11大方法，帮助你解决工作流程问题，无论是运用在团队环境或是在私人项目中，你都可以直接拿来用！ 

**0.  声明一个数组（Declare an array）** 

**[js]** [view plain](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "view plain")[copy](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "copy")

1. String[] aArray = new String[5];  
1. String[] bArray = {"a","b","c", "d", "e"};  
1. String[] cArray = new String[]{"a","b","c","d","e"};  
**1.  在Java中输出一个数组（Print an array in Java）**

**[js]** [view plain](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "view plain")[copy](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "copy")

1. int[] intArray = { 1, 2, 3, 4, 5 };  
1. String intArrayString = Arrays.toString(intArray);  
1.    
1. // print directly will print reference value  
1. System.out.println(intArray);  
1. // [I@7150bd4d  
1.    
1. System.out.println(intArrayString);  
1. // [1, 2, 3, 4, 5]  
**2. 从数组中创建数组列表（**Create an ArrayList from an array**）**

**[js]** [view plain](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "view plain")[copy](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "copy")

1. String[] stringArray = { "a", "b", "c", "d", "e" };  
1. ArrayList<String> arrayList = new ArrayList<String>(Arrays.asList(stringArray));  
1. System.out.println(arrayList);  
1. // [a, b, c, d, e]  
**3. 检查数组中是否包含特定值（Check if an array contains a certain value）**

**[js]** [view plain](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "view plain")[copy](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "copy")

1. String[] stringArray = { "a", "b", "c", "d", "e" };  
1. boolean b = Arrays.asList(stringArray).contains("a");  
1. System.out.println(b);  
1. // true  
**4. 连接两个数组（ Concatenate two arrays）**

**[js]** [view plain](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "view plain")[copy](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "copy")

1. int[] intArray = { 1, 2, 3, 4, 5 };  
1. int[] intArray2 = { 6, 7, 8, 9, 10 };  
1. // Apache Commons Lang library  
1. int[] combinedIntArray = ArrayUtils.addAll(intArray, intArray2);  
**5. 声明一个数组内链（Declare an array inline ）**

**[js]** [view plain](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "view plain")[copy](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "copy")

1. method(new String[]{"a", "b", "c", "d", "e"});  

**6. 将数组元素加入到一个独立的字符串中（Joins the elements of the provided array into a single String）**

**[js]** [view plain](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "view plain")[copy](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "copy")

1. // containing the provided list of elements  
1. // Apache common lang  
1. String j = StringUtils.join(new String[] { "a", "b", "c" }, ", ");  
1. System.out.println(j);  
1. // a, b, c  
**7. 将数组列表转换成一个数组 （Covnert an ArrayList to an array）** 

**[js]** [view plain](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "view plain")[copy](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "copy")

1. String[] stringArray = { "a", "b", "c", "d", "e" };  
1. ArrayList<String> arrayList = new ArrayList<String>(Arrays.asList(stringArray));  
1. String[] stringArr = new String[arrayList.size()];  
1. arrayList.toArray(stringArr);  
1. for (String s : stringArr)  
1.     System.out.println(s);  
**8. 将数组转换成一个集合（Convert an array to a set）** 

**[js]** [view plain](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "view plain")[copy](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "copy")

1. Set<String> set = new HashSet<String>(Arrays.asList(stringArray));  
1. System.out.println(set);  
1. //[d, e, b, c, a]  
**9. 反向数组（Reverse an array）**

**[js]** [view plain](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "view plain")[copy](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "copy")

1. int[] intArray = { 1, 2, 3, 4, 5 };  
1. ArrayUtils.reverse(intArray);  
1. System.out.println(Arrays.toString(intArray));  
1. //[5, 4, 3, 2, 1]  
**10. 删除数组元素（Remove element of an array）**

**[js]** [view plain](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "view plain")[copy](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "copy")

1. int[] intArray = { 1, 2, 3, 4, 5 };  
1. int[] removed = ArrayUtils.removeElement(intArray, 3);//create a new array  
1. System.out.println(Arrays.toString(removed));  
**One more – convert int to byte array** 

**[js]** [view plain](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "view plain")[copy](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays# "copy")

1. byte[] bytes = ByteBuffer.allocate(4).putInt(8).array();  
1.    
1. for (byte t : bytes) {  
1.    System.out.format("0x%x ", t);  
1. }  
来源： <[http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays](http://www.csdn.net/article/2013-09-16/2816947-methods-for-java-arrays)> 

# [Top 10 Methods for Java Arrays](http://www.programcreek.com/2013/09/top-10-methods-for-java-arrays/)
The following are top 10 methods for Java Array. They are the most voted questions from stackoverflow.

**0. Declare an array**
String[] aArray = new String[5];String[] bArray = {"a","b","c", "d", "e"};String[] cArray = new String[]{"a","b","c","d","e"};

**1. Print an array in Java**

int[] intArray = { 1, 2, 3, 4, 5 };String intArrayString = Arrays.toString(intArray); // print directly will print reference valueSystem.out.println(intArray);// [I@7150bd4d System.out.println(intArrayString);// [1, 2, 3, 4, 5]

**2. Create an ArrayList from an array**

String[] stringArray = { "a", "b", "c", "d", "e" };ArrayList<String> arrayList = new ArrayList<String>(Arrays.asList(stringArray));System.out.println(arrayList);// [a, b, c, d, e]

**3. Check if an array contains a certain value**

String[] stringArray = { "a", "b", "c", "d", "e" };boolean b = Arrays.asList(stringArray).contains("a");System.out.println(b);// true

**4. Concatenate two arrays**

int[] intArray = { 1, 2, 3, 4, 5 };int[] intArray2 = { 6, 7, 8, 9, 10 };// Apache Commons Lang libraryint[] combinedIntArray = ArrayUtils.addAll(intArray, intArray2);

**5. Declare an array inline**

method(new String[]{"a", "b", "c", "d", "e"});

**6. Joins the elements of the provided array into a single String**

// containing the provided list of elements// Apache common langString j = StringUtils.join(new String[] { "a", "b", "c" }, ", ");System.out.println(j);// a, b, c

**7. Covnert an ArrayList to an array**

String[] stringArray = { "a", "b", "c", "d", "e" };ArrayList<String> arrayList = new ArrayList<String>(Arrays.asList(stringArray));String[] stringArr = new String[arrayList.size()];arrayList.toArray(stringArr);for (String s : stringArr) System.out.println(s);

**8. Convert an array to a set**

Set<String> set = new HashSet<String>(Arrays.asList(stringArray));System.out.println(set);//[d, e, b, c, a]

**9. Reverse an array**

int[] intArray = { 1, 2, 3, 4, 5 };ArrayUtils.reverse(intArray);System.out.println(Arrays.toString(intArray));//[5, 4, 3, 2, 1]

**10. Remove element of an array**

int[] intArray = { 1, 2, 3, 4, 5 };int[] removed = ArrayUtils.removeElement(intArray, 3);//create a new arraySystem.out.println(Arrays.toString(removed));

**One more – convert int to byte array**

byte[] bytes = ByteBuffer.allocate(4).putInt(8).array(); for (byte t : bytes) { System.out.format("0x%x ", t);}
来源： <[http://www.programcreek.com/2013/09/top-10-methods-for-java-arrays/](http://www.programcreek.com/2013/09/top-10-methods-for-java-arrays/)> 
