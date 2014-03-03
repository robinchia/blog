---
layout: post
title: "13_7_5 key可以重复的Map集合：IdentityHashMap"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_集合
--- 

# 13_7_5 key可以重复的Map集合：IdentityHashMap

[51CTO首页](http://www.51cto.com/) | [新闻](http://news.51cto.com/) | [专题](http://www.51cto.com/col/35) | [论坛](http://bbs.51cto.com/) | [博客](http://blog.51cto.com/) | [技术圈](http://g.51cto.com/) | [读书](http://book.51cto.com/) | [技术频道](http://www.51cto.com/col/35/)| [CIO](http://www.cioage.com/)| [存储](http://www.watchstor.com/)| [地图](http://www.51cto.com/about/map.htm) | [English](http://www.51cto.com/about/aboutus_e.html)

* [组网建网](http://network.51cto.com/)
* [网络安全](http://netsecurity.51cto.com/)
* [服务器](http://server.51cto.com/)
* [操作系统](http://os.51cto.com/)
* [虚拟化](http://virtual.51cto.com/)
* [开发](http://developer.51cto.com/)
* [资讯前沿](http://www.cioage.com/news)
* [业界观察](http://www.cioage.com/insight)
* [应用体验](http://www.cioage.com/exp)
* [杀手技术](http://www.cioage.com/tech)

* [新闻资讯](http://news.watchstor.com/)
* [技术中心](http://tech.watchstor.com/)
* [互动视频](http://video.watchstor.com/)
* [专题汇聚](http://special.watchstor.com/)
您所在的位置： [首页](http://www.51cto.com/) > [读书频道](http://book.51cto.com/) > [设计开发](http://book.51cto.com/col/1221/) > [Java系列](http://book.51cto.com/col/1223/) >

### 13.7.5 key可以重复的Map集合：IdentityHashMap

[http://book.51cto.com/](http://book.51cto.com/)  2009-08-03 09:31  李兴华  清华大学出版社  [我要评论(0)](http://www.51cto.com/php/feedbackt.php?id=141100)
* 摘要：《Java开发实战经典》第13章Java类集，本章将对Java类集进行完整的介绍，针对于一些较为常用的操作也将进行深入的讲解。本节为大家介绍key可以重复的Map集合：IdentityHashMap。
* 标签：[IdentityHashMap](http://www.51cto.com/php/search.php?keyword=IdentityHashMap)  [Java](http://www.51cto.com/php/search.php?keyword=Java)  [Java开发实战经典](http://www.51cto.com/php/search.php?keyword=Java%BF%AA%B7%A2%CA%B5%D5%BD%BE%AD%B5%E4)
*

**13.7.5  key可以重复的Map集合：IdentityHashMap**

之前所讲解的所有Map操作中key的值是不能重复的，例如，HashMap操作时key是不能重复的，如果重复则肯定会覆盖之前的内容，如下代码所示。

范例：Map中的key不允许重复，重复就是覆盖
1. package org.lxh.demo13.mapdemo;  
1. import java.util.HashMap;  
1. import java.util.Iterator;  
1. import java.util.Map;  
1. import java.util.Set;  
1. class Person {                              
// 定义Person类  
1.     private String name;                   
// 定义name属性  
1.     private int age;                        
// 定义age属性  
1.     public Person(String name, int age) {   
// 通过构造方法为属性赋值  
1.         this.name = name;                   
// 为name属性赋值  
1.         this.age = age;                     
// 为age属性赋值  
1.     }  
1.     public boolean equals(Object obj) {    
// 覆写equals()方法  
1.         if (this == obj) {                  
// 判断地址是否相等  
1.             return true;                    
// 返回true表示同一对象  
1.         }  
1.         if (!(obj instanceof Person)) {     
// 传递进来的不是本类的对象  
1.             return false;                   
// 返回false表示不是同一对象  
1.         }  
1.         Person p = (Person) obj;            
// 进行向下转型  
1.         if (this.name.equals(p.name) &&
this.age == p.age) {  
1.             return true ;                   
// 属性依次比较，相等返回true  
1.         }else{  
1.             return false ;                  
// 属性内容不相等，返回false  
1.         }  
1.     }  
1.     public int hashCode(){                    
// 覆写hashCode()方法  
1.         return this.name.hashCode() * this.age ;  
// 计算公式  
1.     }  
1.     public String toString() {                    
// 覆写toString()方法  
1.         return "姓名：" + this.name + "；年龄：" 
+ this.age;   // 返回信息  
1.     }  
1. }   
1. public class IdentityHashMapDemo01 {  
1.     public static void main(String[] args) {  
1.         Map<Person, String> map = null;            
// 声明Map对象，指定  
1. 泛型类型  
1.         map = new HashMap<Person, String>();         
// 实例化Map对象  
1.         map.put(new Person("张三", 30), "zhangsan_1");  
// 增加内容  
1.         map.put(new Person("张三", 30), "zhangsan_2");   
// 增加内容，key重复  
1.         map.put(new Person("李四", 31), "lisi");     
// 增加内容  
1.         Set<Map.Entry<Person, String>> allSet = null;  
// 声明一个Set集合  
1.         allSet = map.entrySet();                  
// 将Map接口实例变为  
1. Set接口实例  
1.         Iterator<Map.Entry<Person, String>> 
iter = null;    // 声明Iterator  
1. 对象  
1.         iter = allSet.iterator();              
// 实例化Iterator  
1. 对象  
1.         while (iter.hasNext()) {               
// 迭代输出  
1.             Map.Entry<Person, String> me = 
iter.next();// 每个对象都是Map.   
1. Entry实例  
1.             System.out.println(me.getKey()   
1.                     + " --> " + me.getValue());  
// 输出key和value  
1.         }  
1.     }  
1. } 

程序运行结果：

1. 姓名：李四；年龄：31 --> lisi  
1. 姓名：张三；年龄：30 --> zhangsan_2 

从程序的运行结果中可以发现，第二个内容覆盖了第一个内容，所以此时可以使用Identity HashMap。使用此类时只要地址不相等（key1！=key2），就表示不是重复的key，可以添加到集合中。

范例：使用IdentityHashMap修改程序
1. package org.lxh.demo13.mapdemo;  
1. import java.util.IdentityHashMap;  
1. import java.util.Iterator;  
1. import java.util.Map;  
1. import java.util.Set;  
1. class Person {  
1.     // 此类与之前定义一样，此处不再列出  
1. }  
1. public class IdentityHashMapDemo02 {  
1.     public static void main(String[] args) {  
1.         Map<Person, String> map = null;        
// 声明Map对象，指定  
1. 泛型类型  
1.         map = new IdentityHashMap<Person, String>(); 
// 实例化Map对象  
1.         map.put(new Person("张三", 30), "zhangsan_1"); 
// 增加内容  
1.         map.put(new Person("张三", 30), "zhangsan_2");  
// 增加内容，key重复  
1.         map.put(new Person("李四", 31), "lisi");    
// 增加内容  
1.         Set<Map.Entry<Person, String>> allSet = 
null;   // 声明一个Set集合  
1.         allSet = map.entrySet();               
// 将Map接口实例变为  
1. Set接口实例  
1.         Iterator<Map.Entry<Person, String>> 
iter = null;// 声明Iterator对象  
1.         iter = allSet.iterator();            
// 实例化Iterator  
1. 对象  
1.         while (iter.hasNext()) {               
// 迭代输出  
1.             Map.Entry<Person, String> me = 
iter.next();// 每个对象都是Map.  
1. Entry实例  
1.             System.out.println(me.getKey()   
1.                     + " --> " + me.getValue());  
// 输出key和value  
1.         }  
1.     }  
1. } 

程序运行结果：

1. 姓名：张三；年龄：30 --> zhangsan_2  
1. 姓名：张三；年龄：30 --> zhangsan_1  
1. 姓名：李四；年龄：31 --> lisi 

从程序的运行结果中可以发现，现在的key允许重复，只要两个对象的地址不相等即可。

【责任编辑：[云霞](mailto:limian@51cto.com) TEL：（010）68476606】
[**回书目**](http://book.51cto.com/art/200907/140657.htm)   [**上一节**](http://book.51cto.com/art/200908/141099.htm)   [**下一节**](http://book.51cto.com/art/200908/141101.htm)
上一篇： [13.7.4 Map接口的使用注意事项（2）](http://book.51cto.com/art/200908/141099.htm) 下一篇： [13.8 SortedMap接口](http://book.51cto.com/art/200908/141101.htm)

 

## [频道推荐](http://book.51cto.com/click/1223)

[更多>>](http://book.51cto.com/click/1223)

* ·[自己动手写搜索引擎](http://book.51cto.com/art/200910/159525.htm "自己动手写搜索引擎")
* ·[1.1.2 编写代码（15分钟）](http://book.51cto.com/art/200910/159539.htm "1.1.2  编写代码（15分钟）")
* ·[1.1.1 准备工作环境（10分钟）](http://book.51cto.com/art/200910/159538.htm "1.1.1  准备工作环境（10分钟）")
## [热点标签](http://www.51cto.com/)

[刀片服务器](http://server.51cto.com/Blade "刀片服务器专区")   [云计算](http://cloud.51cto.com/ "云计算频道全新上线")   [ARP攻防](http://netsecurity.51cto.com/art/200609/31897.htm "ARP攻击与防御")   [思科培训](http://training.51cto.com/cisco "思科培训技术专区")  
## [全站热点](http://www.51cto.com/)

[更多>>](http://www.51cto.com/)

[](http://netsecurity.51cto.com/art/200609/31897.htm "ARP攻击防范与解决方案")

[ARP攻击防范与..](http://netsecurity.51cto.com/art/200609/31897.htm "ARP攻击防范与解决方案")

[](http://training.51cto.com/art/200804/71335.htm "2009年下半年软考最新试题与答案")

[2009年下半年..](http://training.51cto.com/art/200804/71335.htm "2009年下半年软考最新试题与答案")
* ·[网络工程师须知：30个经典的路由问题](http://network.51cto.com/art/200910/159165.htm "网络工程师须知：30个经典的路由问题")
* ·[图解天河一号 中国最快的计算机](http://server.51cto.com/server-159912.htm "图解天河一号 中国最快的计算机")
* ·[Ubuntu 9.10 功能截图](http://os.51cto.com/art/200910/159488.htm "Ubuntu 9.10 功能截图")
* ·[Google网页工具包(GWT)是Web开发的未来？](http://developer.51cto.com/art/200910/159138.htm "Google网页工具包(GWT)是Web开发的未来？")
* ·[Ubuntu 9.10 桌面版发布 今日开始下载](http://os.51cto.com/art/200910/159486.htm "Ubuntu 9.10 桌面版发布 今日开始下载")

## [技术人](http://fellow.51cto.com/)

[更多>>](http://fellow.51cto.com/)
[![]()](http://fellow.51cto.com/art/200907/140965.htm "IT十大死对头：Linux单挑Windows 谷歌对抗所有人")

[IT十大死对头：L..](http://fellow.51cto.com/art/200907/140965.htm "IT十大死对头：Linux单挑Windows 谷歌对抗所有人")

[![]()](http://job.51cto.com/art/200907/140484.htm "程序员面试技巧：全程大揭秘")

[程序员面试技巧..](http://job.51cto.com/art/200907/140484.htm "程序员面试技巧：全程大揭秘")

* ·[腾讯“小三”事件追踪](http://job.51cto.com/art/200910/159867.htm "腾讯“小三”事件追踪")
* ·[反盗版联盟逼宫迅雷:两阵营险些大打出手](http://job.51cto.com/art/200910/159848.htm "反盗版联盟逼宫迅雷:两阵营险些大打出手")
* ·[网管技能水平考试教材发布 开启网管人才培养新..](http://training.51cto.com/art/200911/160298.htm "网管技能水平考试教材发布 开启网管人才培养新篇章")
* ·[没有技术怎么办 任正非的人才技术兼收之道](http://fellow.51cto.com/art/200911/160116.htm "没有技术怎么办 任正非的人才技术兼收之道")
* ·[开心网起诉千橡不正当竞争庭审文字记录](http://job.51cto.com/art/200910/159521.htm "开心网起诉千橡不正当竞争庭审文字记录")
## [读书](http://book.51cto.com/)

[更多>>](http://book.51cto.com/)

* ·[监控](http://book.51cto.com/art/200910/157013.htm "监控")
* ·[Python在Unix和Linux系统管理中的应用](http://book.51cto.com/art/200910/156835.htm "Python在Unix和Linux系统管理中的应用")
* ·[锦绣蓝图：怎样规划令人流连忘返的网站（第2版）](http://book.51cto.com/art/200910/155790.htm "锦绣蓝图：怎样规划令人流连忘返的网站（第2版）")
* ·[Scala编程](http://book.51cto.com/art/200910/156006.htm "Scala编程")
## [优秀博文](http://blog.51cto.com/)

[更多>>](http://blog.51cto.com/)

## [最新热帖](http://bbs.51cto.com/hotthreads.php)

[更多>>](http://bbs.51cto.com/hotthreads.php)

   

Copyright©2005-2009 51CTO.COM 版权所有
