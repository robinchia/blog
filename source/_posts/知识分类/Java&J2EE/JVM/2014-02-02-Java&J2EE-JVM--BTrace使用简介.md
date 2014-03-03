---
layout: post
title: "BTrace使用简介"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# BTrace使用简介

# [BlueDavy之技术blog](https://blog.bluedavy.com/)

{互联网，OSGi，Java, High Scalability, High Performance,HA}

* [Home](https://blog.bluedavy.com/)
* [About](https://blog.bluedavy.com/?page_id=2)
* [Photos](https://blog.bluedavy.com/?page_id=81)

## [BTrace使用简介](https://blog.bluedavy.com/?p=185 "BTrace使用简介")

Nov 11

[bluedavy](http://bluedavy.com/ "Visit bluedavy’s website")[jvm](https://blog.bluedavy.com/?cat=13 "View all posts in jvm") [btrace](https://blog.bluedavy.com/?tag=btrace) [13 Comments]( "Comment on BTrace使用简介")
很多时候在online的应用出现问题时，很多时候我们需要知道更多的程序的运行细节，但又不可能在开发的时候就把程序中所有的运行细节都打印到日志上，通常这个时候能采取的就是修改代码，重新部署，然后再观察，但这种方法对于online应用来说不是很好，另外一方面如果碰到不好改的代码，例如引用的其他的外部的包什么的，就很麻烦了，BTrace就是一个可以在不改代码、不重启应用的情况下，动态的查看程序运行细节的工具，其官方网站在此：[http://kenai.com/projects/btrace/](http://kenai.com/projects/btrace/)，在这篇blog中，就来看看如何用BTrace来动态的监测方法的一些运行细节状况。
BTrace通过动态的挂接用java写的代码到运行时上来获取到一些运行细节，例如典型的使用btrace的方法为：
btrace -cp [btrace的jar所在的路径，默认为btrace/build下] [pid] [需要运行的java代码]
例如一段这样的代码：
[?]()1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
17

18
19

20
21

22
23

24
25

26
import

java.util.Random;

public

class

Case1{
 

   

public

static

void

main(String[] args)

throws

Exception{
      

Random random=

new

Random();

      

CaseObject object=

new

CaseObject();
      

boolean

result=

true

;

      

while

(result){
         

result=object.execute(random.nextInt(

1000

));

         

Thread.sleep(

1000

);
      

}

   

}
 

}
public

class

CaseObject{

 
   

private

static

int

sleepTotalTime=

0

;  

 
   

public

boolean

execute(

int

sleepTime)

throws

Exception{

       

System.out.println(

"sleep: "

+sleepTime);
       

sleepTotalTime+=sleepTime;

       

Thread.sleep(sleepTime);
       

return

true

;

   

}
 

}

如在程序运行的情况下，想知道调用CaseObject的execute方法的以下几种情况，在BTrace中可以这么做：
1、调用此方法时传入的是什么参数，返回的是什么值，当时sleepTotalTime是多少？
BTrace脚本如下：

[?]()1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
    

import

static

com.sun.btrace.BTraceUtils.*;

import

com.sun.btrace.annotations.*;
 

@BTrace

public

class

TraceMethodArgsAndReturn{
   

@OnMethod

(

      

clazz=

"CaseObject"

,
      

method=

"execute"

,

      

location=

@Location

(Kind.RETURN)
   

)

   

public

static

void

traceExecute(

@Self

CaseObject instance,

int

sleepTime,

@Return

boolean

result){
     

println(

"call CaseObject.execute"

);

     

println(strcat(

"sleepTime is:"

,str(sleepTime)));
     

println(strcat(

"sleepTotalTime is:"

,str(get(field(

"CaseObject"

,

"sleepTotalTime"

),instance))));

     

println(strcat(

"return value is:"

,str(result)));
   

}

}

然后直接执行btrace -cp btrace/build [pid] TraceMethodArgsAndReturn.java就可以了。
当程序中调用到caseobject的execute方法时，就会在btrace的console中输出相应的信息。
2、execute方法执行耗时是多久？
BTrace脚本如下：

[?]()1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
17

18
19

20
21

22
23

24
    

import

static

com.sun.btrace.BTraceUtils.*;

import

com.sun.btrace.annotations.*;
 

@BTrace

public

class

TraceMethodExecuteTime{
   
 

   

@TLS

static

long

beginTime;
 

   

@OnMethod

(
      

clazz=

"CaseObject"

,

      

method=

"execute"
   

)

   

public

static

void

traceExecuteBegin(){
     

beginTime=timeMillis();

   

}
 

   

@OnMethod

(
      

clazz=

"CaseObject"

,

      

method=

"execute"

,
      

location=

@Location

(Kind.RETURN)

   

)
   

public

static

void

traceExecute(

int

sleepTime,

@Return

boolean

result){

      

println(strcat(strcat(

"CaseObject.execute time is:"

,str(timeMillis()-beginTime)),

"ms"

));
   

}

}

3、谁调用了execute方法？
BTrace脚本如下：

[?]()1

2
3

4
5

6
7

8
9

10
11

12
13     

import

static

com.sun.btrace.BTraceUtils.*;

import

com.sun.btrace.annotations.*;
 

@BTrace

public

class

TraceMethodCallee{
   

@OnMethod

(

      

clazz=

"CaseObject"

,
      

method=

"execute"

   

)
   

public

static

void

traceExecute(){

     

println(

"who call CaseObject.execute :"

);
     

jstack();

   

}
}

4、有没有人调用CaseObject中的哪一行代码？
BTrace脚本如下：

[?]()1

2
3

4
5

6
7

8
9

10
11

12
    

import

static

com.sun.btrace.BTraceUtils.*;

import

com.sun.btrace.annotations.*;
 

@BTrace

public

class

TraceMethodLine{
   

@OnMethod

(

      

clazz=

"CaseObject"

,
      

location=

@Location

(value=Kind.LINE,line=

5

)

   

)
   

public

static

void

traceExecute(

@ProbeClassName

String pcn,

@ProbeMethodName

String pmn,

int

line){

     

println(strcat(strcat(strcat(

"call "

,pcn),

"."

),pmn));
   

}

}

从上面可看出，在有了BTrace后，要动态的跟踪代码的运行细节还是非常爽的，更多的细节的操作请大家查看BTrace的[User Guide](http://kenai.com/projects/btrace/pages/UserGuide)。

[GC悲观策略之Serial GC篇](https://blog.bluedavy.com/?p=170) [学习JVM的References](https://blog.bluedavy.com/?p=187)

### 13 Comments *([+add yours?]())*

1. 
![]() [ikbear](http://www.helishi.net/)
**Nov 11, 2010** @ 17:41:12
收藏了。
1. 
![](https://secure.gravatar.com/avatar/4a2de3195dfd282b2f0ea420dc7a6a7a?s=48&d=<path_to_url>&r=G) pandonix
**Nov 11, 2010** @ 20:58:54
关于方法的执行时间和返回值，有个问题请教，如果调用量非常大的方法，有没有类似染色日志这样的功能：只需要打印参数符合某种条件的调用情况？
1. 
![](https://secure.gravatar.com/avatar/5267242305332fe1affd9c4788191e1d?s=48&d=<path_to_url>&r=G) [bluedavy](http://bluedavy.com/)
**Nov 12, 2010** @ 11:42:06
![:)]() ，这个自己可以在脚本里判断下…
1. 
![](https://secure.gravatar.com/avatar/bd17360b1eb0b26b89a19de9cebed4f9?s=48&d=<path_to_url>&r=G) vitty
**Nov 14, 2010** @ 14:09:22
不知道这个对被监测程序的性能影响有多大？不过真的很爽呢。
1. 
![](https://secure.gravatar.com/avatar/5267242305332fe1affd9c4788191e1d?s=48&d=<path_to_url>&r=G) [bluedavy](http://bluedavy.com/)
**Nov 14, 2010** @ 19:53:38
据使用来看，影响很小。
1. 
![](https://secure.gravatar.com/avatar/baad2156c4a21abc487fb92bbd53a174?s=48&d=<path_to_url>&r=G) bobo
**Nov 16, 2010** @ 10:16:58
学习,哈哈
1. 
![](https://secure.gravatar.com/avatar/5ef11dda80e7ce522810093bd8a791eb?s=48&d=<path_to_url>&r=G) xuanyuanzhiyuan
**Dec 21, 2010** @ 15:37:22
学习了 ：）
1. 
![](https://secure.gravatar.com/avatar/4a2de3195dfd282b2f0ea420dc7a6a7a?s=48&d=<path_to_url>&r=G) pandonix
**Dec 27, 2010** @ 23:07:55
运行第一个例子：TraceMethodArgsAndReturn.java报错：
TraceMethodArgsAndReturn.java:10: 找不到符号
符号： 类 CaseObject
位置： 类 TraceMethodArgsAndReturn
把@Self CaseObject instance修改成为@Self Object self，并且去掉println(strcat(“sleepTotalTime is:”,str(get(field(“CaseObject”,”sleepTotalTime”),instance))));
就可以了，但是楼主希望获取sleepTotalTime的需求就满足不鸟了。还不太明白@Self的含义，再看看纠结的文档吧
1. 
![](https://secure.gravatar.com/avatar/4a2de3195dfd282b2f0ea420dc7a6a7a?s=48&d=<path_to_url>&r=G) pandonix
**Dec 27, 2010** @ 23:44:37
找到原因了，是CaseObject不在classthpath中，如果用到了@Self CaseObject ，编译TraceMethodArgsAndReturn.java时会去找CaseObject类。
简单的将CaseObject.java放到跟TraceMethodArgsAndReturn.java同一目录即可
1. 
![](https://secure.gravatar.com/avatar/6a8f8285a42a76f5e47159c9f09a7c4f?s=48&d=<path_to_url>&r=G) 光夏
**Jan 23, 2011** @ 11:34:36
使用TLS得注意递归调用的情况吧
1. 
![]() victorcai
**Jul 19, 2011** @ 15:03:18
请问各位大侠btrace能否在windows xp上使用,是否需要设置环境变量,我在windows xp上每次使用都失败
### Leave a Reply

[Cancel]()

Name (required)

Mail (required)

Website

![CAPTCHA Image](https://blog.bluedavy.com/wp-content/plugins/si-captcha-for-wordpress/captcha/securimage_show.php?si_form_id=com&prefix=2KNUhEvWsRaEKSqu "CAPTCHA Image")

[![Refresh Image]()]( "Refresh Image")

CAPTCHA Code *

<style type='text/css'>#submit {display:none;}</style><br /> <input name="submit" type="submit" id="submit-alt" tabindex="6" value="Submit Comment" />

July 2013 M T W T F S S [« Mar](https://blog.bluedavy.com/?m=201303 "View posts for March 2013")     1234567 891011121314 15161718192021 22232425262728 293031  

### Categories

* [Java](https://blog.bluedavy.com/?cat=63 "View all posts filed under Java") (10)
* [jvm](https://blog.bluedavy.com/?cat=13 "View all posts filed under jvm") (19)
* [NoSQL](https://blog.bluedavy.com/?cat=56 "View all posts filed under NoSQL") (7)
* [SOA](https://blog.bluedavy.com/?cat=4 "View all posts filed under SOA") (1)
* [书:分布式Java应用](https://blog.bluedavy.com/?cat=11 "关于《分布式Java应用：基础与实践》书部分章节的公开、书内容的纠错以及补充完善。") (5)
* [互联网](https://blog.bluedavy.com/?cat=6 "View all posts filed under 互联网") (5)
* [产品总结](https://blog.bluedavy.com/?cat=46 "View all posts filed under 产品总结") (1)
* [优化案例](https://blog.bluedavy.com/?cat=18 "View all posts filed under 优化案例") (1)
* [圆桌交流](https://blog.bluedavy.com/?cat=12 "View all posts filed under 圆桌交流") (2)
* [容量规划](https://blog.bluedavy.com/?cat=8 "View all posts filed under 容量规划") (2)
* [迁户口](https://blog.bluedavy.com/?cat=97 "View all posts filed under 迁户口") (1)
* [高可用](https://blog.bluedavy.com/?cat=52 "View all posts filed under 高可用") (1)
* [高并发](https://blog.bluedavy.com/?cat=3 "View all posts filed under 高并发") (3)
* [高性能](https://blog.bluedavy.com/?cat=1 "关于性能优化方面的一些东西。") (2)
### Recent Comments

* [JVM调优 | code1（code1.riaos.com）](http://code1.riaos.com/?p=5030138) on [说说MaxTenuringThreshold这个参数](https://blog.bluedavy.com/?p=70&cpage=1#comment-16520)
* [JVM调优 | architecture（riaos.com）](http://architecture1.riaos.com/?p=3063358) on [说说MaxTenuringThreshold这个参数](https://blog.bluedavy.com/?p=70&cpage=1#comment-16519)
* [bluedavy](http://bluedavy.com/) on [一个Java应用频繁抛异常导致cpu us诡异现象的案例](https://blog.bluedavy.com/?p=409&cpage=1#comment-16462)
* xiaobo on [一个Java应用频繁抛异常导致cpu us诡异现象的案例](https://blog.bluedavy.com/?p=409&cpage=1#comment-16460)
* [bluedavy](http://bluedavy.com/) on [一个Java应用频繁抛异常导致cpu us诡异现象的案例](https://blog.bluedavy.com/?p=409&cpage=1#comment-16459)

### Tags

[btrace](https://blog.bluedavy.com/?tag=btrace "2 topics") [c1](https://blog.bluedavy.com/?tag=c1 "1 topic") [c2](https://blog.bluedavy.com/?tag=c2 "1 topic") [Deflater](https://blog.bluedavy.com/?tag=deflater "1 topic") [facebook](https://blog.bluedavy.com/?tag=facebook "2 topics") [gc](https://blog.bluedavy.com/?tag=gc "4 topics") [gc tuning](https://blog.bluedavy.com/?tag=gc-tuning "2 topics") [Grizzly](https://blog.bluedavy.com/?tag=grizzly "2 topics") [HBase](https://blog.bluedavy.com/?tag=hbase "6 topics") [hotspot](https://blog.bluedavy.com/?tag=hotspot "1 topic") [Inflater](https://blog.bluedavy.com/?tag=inflater "1 topic") [interpreter](https://blog.bluedavy.com/?tag=interpreter "1 topic") [javac](https://blog.bluedavy.com/?tag=javac "1 topic") [java code generation](https://blog.bluedavy.com/?tag=java-code-generation "1 topic") [JavaOne](https://blog.bluedavy.com/?tag=javaone "4 topics") [javaone general technical session](https://blog.bluedavy.com/?tag=javaone-general-technical-session "1 topic") [java代码执行](https://blog.bluedavy.com/?tag=java%e4%bb%a3%e7%a0%81%e6%89%a7%e8%a1%8c "1 topic") [Java 并发](https://blog.bluedavy.com/?tag=java-%e5%b9%b6%e5%8f%91 "1 topic") [jit](https://blog.bluedavy.com/?tag=jit "1 topic") [jvm](https://blog.bluedavy.com/?tag=jvm "12 topics") [memory management](https://blog.bluedavy.com/?tag=memory-management "1 topic") [Native Memory Leak](https://blog.bluedavy.com/?tag=native-memory-leak "1 topic") [NoSQL](https://blog.bluedavy.com/?tag=nosql "2 topics") [oom](https://blog.bluedavy.com/?tag=oom "1 topic") [oracle keynote](https://blog.bluedavy.com/?tag=oracle-keynote "1 topic") [pessimism policy](https://blog.bluedavy.com/?tag=pessimism-policy "1 topic") [references](https://blog.bluedavy.com/?tag=references "1 topic") [RPC](https://blog.bluedavy.com/?tag=rpc "2 topics") [serial gc](https://blog.bluedavy.com/?tag=serial-gc "1 topic") [SOA](https://blog.bluedavy.com/?tag=soa "2 topics") [sun jdk](https://blog.bluedavy.com/?tag=sun-jdk "1 topic") [sun jdk oom](https://blog.bluedavy.com/?tag=sun-jdk-oom "1 topic") [Web容量规划的艺术](https://blog.bluedavy.com/?tag=web%e5%ae%b9%e9%87%8f%e8%a7%84%e5%88%92%e7%9a%84%e8%89%ba%e6%9c%af "1 topic") [yuanzhuo](https://blog.bluedavy.com/?tag=yuanzhuo "1 topic") [书:分布式Java应用](https://blog.bluedavy.com/?tag=%e4%b9%a6 "1 topic") [书评](https://blog.bluedavy.com/?tag=%e4%b9%a6%e8%af%84 "1 topic") [互联网技术](https://blog.bluedavy.com/?tag=%e4%ba%92%e8%81%94%e7%bd%91%e6%8a%80%e6%9c%af "1 topic") [交流](https://blog.bluedavy.com/?tag=%e4%ba%a4%e6%b5%81 "1 topic") [内存管理](https://blog.bluedavy.com/?tag=%e5%86%85%e5%ad%98%e7%ae%a1%e7%90%86 "1 topic") [分布式Java应用](https://blog.bluedavy.com/?tag=%e5%88%86%e5%b8%83%e5%bc%8fjava%e5%ba%94%e7%94%a8 "2 topics") [圆桌交流](https://blog.bluedavy.com/?tag=%e5%9c%86%e6%a1%8c%e4%ba%a4%e6%b5%81 "1 topic") [容量规划](https://blog.bluedavy.com/?tag=%e5%ae%b9%e9%87%8f%e8%a7%84%e5%88%92 "2 topics") [悲观策略](https://blog.bluedavy.com/?tag=%e6%82%b2%e8%a7%82%e7%ad%96%e7%95%a5 "3 topics") [服务框架](https://blog.bluedavy.com/?tag=%e6%9c%8d%e5%8a%a1%e6%a1%86%e6%9e%b6 "1 topic") [硅谷公司](https://blog.bluedavy.com/?tag=%e7%a1%85%e8%b0%b7%e5%85%ac%e5%8f%b8 "1 topic")
### 订阅

### 推荐书籍
### My Book

[![]()](http://book.douban.com/subject/4848587/ "分布式Java应用：基础与实践") [![]()](http://book.douban.com/subject/3843896/ "OSGi原理与最佳实践")
© [BlueDavy之技术blog](https://blog.bluedavy.com/) 2013

[Icons](http://icondock.com/) & [Wordpress Theme](http://www.ndesign-studio.com/wp-themes) by [N.Design](http://www.ndesign-studio.com/)
