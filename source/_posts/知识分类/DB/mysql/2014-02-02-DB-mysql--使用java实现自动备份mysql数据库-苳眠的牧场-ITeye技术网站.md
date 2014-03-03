---
layout: post
title: "使用java实现自动备份mysql数据库 - 苳眠的牧场 - ITeye技术网站"
categories: DB
tags: 
 - DB
 - mysql
--- 

# 使用java实现自动备份mysql数据库 - 苳眠的牧场 - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼](http://wangjie2013.iteye.com/blog/1858541#)

[招聘](http://job.iteye.com/iteye) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://wangjie2013.iteye.com/login "登录") [登录](http://wangjie2013.iteye.com/login) [注册](http://wangjie2013.iteye.com/signup)

# [苳眠的牧场](http://wangjie2013.iteye.com/)

* [**博客**](http://wangjie2013.iteye.com/)
* [微博](http://wangjie2013.iteye.com/weibo)
* [相册](http://wangjie2013.iteye.com/album)
* [收藏](http://wangjie2013.iteye.com/link)
* [留言](http://wangjie2013.iteye.com/blog/guest_book)
* [关于我](http://wangjie2013.iteye.com/blog/profile)

### [使用java实现自动备份mysql数据库]() **

**博客分类：**
* [mysql](http://wangjie2013.iteye.com/category/276804)
[mysql](http://www.iteye.com/blogs/tag/mysql)[数据库](http://www.iteye.com/blogs/tag/%E6%95%B0%E6%8D%AE%E5%BA%93)[备份](http://www.iteye.com/blogs/tag/%E5%A4%87%E4%BB%BD)[java](http://www.iteye.com/blogs/tag/java) 

       Last modified：2013-05-02 16:55:01

      **********************************************

       在实际应用中，定时备份数据库是一件非常重要的工作，下面是关于利用java程序实现数据库自动调用的方法，其实也不一定非要用java语言了，只要原理会了，大家大可使用其他语言来实现。话不多说，下面就来演示一下如何**自动备份mysql下的abc数据库**：

 

1，在java API中为我们提供了一个Runtime类，它可以用来调用一些程序，比如notepad.exe,cmd.exe...

具体怎么回事，想了解的同学去看API吧，下面是实现代码：Backup.java

 
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. import java.util.Calendar;  
1. import java.util.Date;  
1. import java.text.SimpleDateFormat;  
1. import java.io.IOException;  
1. import java.io.PrintStream;  
1.   
1. public class Backup   
1. {  
1.     public static void main(String[] args)   
1.     {  
1.         Runtime runtime = Runtime.getRuntime();  
1.         Calendar calendar = Calendar.getInstance();  
1.         SimpleDateFormat dateFormat = new SimpleDateFormat("yyyyMMdd_HHmmss");  
1.         String currentTime = dateFormat.format(calendar.getTime());  
1.         Process p = null;  
1.         PrintStream print = null;  
1.         StringBuilder buf = new StringBuilder();  
1.         for(String a : args){  
1.             buf.append(a);  
1.             buf.append(" ");  
1.         }  
1.         String databases = buf.toString();  
1.         System.out.println(databases);  
1.         try{  
1.             p = runtime.exec("cmd /c mysqldump -uroot -p1234 -B "+databases+">"+currentTime+".sql.bak");  
1.         }catch (IOException e){  
1.             if( p != null ){  
1.                 p.destroy();  
1.             }  
1.             try{  
1.                 print = new PrintStream(currentTime+"_backup_err.log");  
1.                 dateFormat.applyPattern("yyyy-MM-dd HH:mm:ss");  
1.                 currentTime = dateFormat.format(calendar.getTime());  
1.                 print.println(currentTime+"  backup failed.");  
1.                 e.printStackTrace(print);  
1.                 print.flush();  
1.             }catch (IOException e2){  
1.   
1.             }finally{  
1.                 if(print!=null){  
1.                     print.close();  
1.                 }  
1.             }  
1.         }  
1.     }  
1. }  
import java.util.Calendar; import java.util.Date; import java.text.SimpleDateFormat; import java.io.IOException; import java.io.PrintStream; public class Backup { public static void main(String[] args) { Runtime runtime = Runtime.getRuntime(); Calendar calendar = Calendar.getInstance(); SimpleDateFormat dateFormat = new SimpleDateFormat("yyyyMMdd_HHmmss"); String currentTime = dateFormat.format(calendar.getTime()); Process p = null; PrintStream print = null; StringBuilder buf = new StringBuilder(); for(String a : args){ buf.append(a); buf.append(" "); } String databases = buf.toString(); System.out.println(databases); try{ p = runtime.exec("cmd /c mysqldump -uroot -p1234 -B "+databases+">"+currentTime+".sql.bak"); }catch (IOException e){ if( p != null ){ p.destroy(); } try{ print = new PrintStream(currentTime+"_backup_err.log"); dateFormat.applyPattern("yyyy-MM-dd HH:mm:ss"); currentTime = dateFormat.format(calendar.getTime()); print.println(currentTime+" backup failed."); e.printStackTrace(print); print.flush(); }catch (IOException e2){ }finally{ if(print!=null){ print.close(); } } } } }

 

2，将以上java程序编译后得到Backup.class文件；

 

 

 3，创建批处理文件mytask.bat: 
Bat代码  [![收藏代码]()![]()]( "收藏这段代码")

1. @echo off  
1. cd c:\backup_wj  
1. rem 这里提倡使用绝度路径，并且如果绝度路径中有空格，记得用  
1. rem 引号将路径括起来！  
1. rem 可以将abc替换为其他数据库，多多个数据库，并用空格隔开  
1. "D:\Program Files\Java\jdk1.6.0_31\bin\java.exe" Backup abc  
@echo off cd c:\backup_wj rem 这里提倡使用绝度路径，并且如果绝度路径中有空格，记得用 rem 引号将路径括起来！ rem 可以将abc替换为其他数据库，多多个数据库，并用空格隔开 "D:\Program Files\Java\jdk1.6.0_31\bin\java.exe" Backup abc

 

 3.1：根据评论我又发现了一个新的方法，不需要使用java，直接写一个批处理文件就可以实现自动备份功能，读者可以直接使用下面的批处理文件代替原来的批处理文件，不需要在使用java了，然后按照下面的步骤制订计划任务就可以了，代码如下：
Bat代码  [![收藏代码]()![]()]( "收藏这段代码")

1. mysqldump -uroot -p1234 -B abc > %date:~0,4%%date:~5,2%%date:~8,2%_%time:~0,2%%time:~3,2%%time:~6,2%.sql.bak  
mysqldump -uroot -p1234 -B abc > %date:~0,4%%date:~5,2%%date:~8,2%_%time:~0,2%%time:~3,2%%time:~6,2%.sql.bak

  

4，将Backup.class、mytask.bat放到同一个目录下，比如我就放在了c:\backup_wj目录下

 

 

5，在win7下配置任务计划，下面照着步骤一步一步走就行了：

 

 

5.1：在控制面板下找到“管理工具”——》“任务计划程序”

![]( "点击查看原始大小图片")
 

![]()
 

5.2：选择“任务计划程序库”——》“创建基本任务”

![]( "点击查看原始大小图片")
 

5.3：填写任务名称和描述

![]( "点击查看原始大小图片")
 

 

5.4：下面根据向导的描述操作就可以了；

 

![]()
 
![]( "点击查看原始大小图片")
 
![]()
 

5.5：点击“浏览”选择批处理文件“mytask.bat”；

 

![]( "点击查看原始大小图片")
 

 

![]()
 

 

 
![]( "点击查看原始大小图片")
 
![]( "点击查看原始大小图片")
 

5.6：点击“完成”后，选中autoBackup，点击“运行”测试一下：

 

![]()
 

是不是有一个黑窗口一闪而过，好的那么打开你存放backup.bat的目录看一看，是不是生成了一个备份文件呢？

下面是我的，不过我们的备份文件已经生成了4次，从文件名就可以看出生成的时间。

![]()
 

那么其中的原理大家都会了吧，其实就是让我们的操作系统定时去执行一个批处理文件“Backup.bat”,而这个批处理文件会去执行一个.class文件，这是一个java可执行文件，它会调用runtime.exec()方法，在命令行下执行一段命令，这段命令就是备份数据库的命令。

注意：对于备份的时间，我们可以设置得更加精细，通过“编辑触发器”中的“重复任务间隔”我们可以精确到每5分钟重复执行一次。具体怎么设置大家可以根据实际情况来设置。

![]( "点击查看原始大小图片")
 
* [![]( "点击查看原始大小图片")]()
* 大小: 65.5 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 53.9 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 79.5 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 39 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 34.2 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 30.9 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 31.8 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 34.1 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 52.6 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 36.3 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 48.2 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 64.3 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 44.2 KB

* [![]( "点击查看原始大小图片")]()
* 大小: 83.7 KB

* [查看图片附件](http://wangjie2013.iteye.com/blog/1858541#)

**7**
顶

**5**
踩

分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")
[关于工资的三个秘密](http://wangjie2013.iteye.com/blog/1856897 "关于工资的三个秘密")

* 23 小时前
* 浏览 1571
* [评论(7)](http://wangjie2013.iteye.com/blog/1858541#comments)
* 分类:[数据库](http://www.iteye.com/blogs/category/database)
* [相关推荐](http://www.iteye.com/wiki/blog/1858541)
### 评论

[]()

7 楼 [kjj](http://docs.iteye.com/ "kjj") 4 小时前  

如果是java，我以为你要一条条读取，原来回头来调用mysqldump ，直接at搞定算了！
6 楼 [wangjie2013](http://wangjie2013.iteye.com/ "wangjie2013") 16 小时前  

freezingsky 写道

mysql自身就可以实现了，实在没必要用其他的任何语言来附加，有种多余的感觉 。
mysql提供指令自动备份了吗？怎么做呀？？

5 楼 [freezingsky](http://freezingsky.iteye.com/ "freezingsky") 16 小时前  

mysql自身就可以实现了，实在没必要用其他的任何语言来附加，有种多余的感觉 。
4 楼 [wangjie2013](http://wangjie2013.iteye.com/ "wangjie2013") 19 小时前  

RamosLi 写道

一行命令就能搞定： at xx:xx /every:date mysqldump xxx
命令格式自己查吧 at/?
那么如果要实现每天多次备份是不是执行多次at命令，更改为不同的时间就可以了呀？
还有，，，，怎么停止呢？

3 楼 [RamosLi](http://ramosli.iteye.com/ "RamosLi") 19 小时前  

一行命令就能搞定： at xx:xx /every:date mysqldump xxx
命令格式自己查吧 at/?
2 楼 [wangjie2013](http://wangjie2013.iteye.com/ "wangjie2013") 20 小时前  

cgs1999 写道

使用java有点多余，其实可以新建一个批处理文件backup.bat，内容如下：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. mysqldump -uroot -p1234 -B abc>%date:~0,4%%date:~5,2%%date:~8,2%_%time:~0,2%%time:~3,2%%time:~6,2%.sql.bak  
mysqldump -uroot -p1234 -B abc>%date:~0,4%%date:~5,2%%date:~8,2%_%time:~0,2%%time:~3,2%%time:~6,2%.sql.bak
同样使用系统的“计划任务”实现定时备份
哈哈 你还真想的出来！哈哈 我本来也想过看看有系统有没有提供获取完整时间的方法，就查到date和time，不过没想到你拼出来了，哈哈。

1 楼 [cgs1999](http://cgs1999.iteye.com/ "cgs1999") 22 小时前  

使用java有点多余，其实可以新建一个批处理文件backup.bat，内容如下：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. mysqldump -uroot -p1234 -B abc>%date:~0,4%%date:~5,2%%date:~8,2%_%time:~0,2%%time:~3,2%%time:~6,2%.sql.bak  
mysqldump -uroot -p1234 -B abc>%date:~0,4%%date:~5,2%%date:~8,2%_%time:~0,2%%time:~3,2%%time:~6,2%.sql.bak
同样使用系统的“计划任务”实现定时备份

### 发表评论

[![]()](http://wangjie2013.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://wangjie2013.iteye.com/login)

[![wangjie2013的博客]( "wangjie2013的博客: 苳眠的牧场")](http://wangjie2013.iteye.com/)

wangjie2013

* 浏览: 5001 次
* 性别: ![Icon_minigender_1]( "男")
* ![]()
### 最近访客 [更多访客>>](http://wangjie2013.iteye.com/blog/user_visits)

[![小网客的博客]( "小网客的博客: 小网客")](http://smallnetvisitor.iteye.com/)

[小网客](http://smallnetvisitor.iteye.com/ "小网客")

[![GoodWell的博客]( "GoodWell的博客: GoodWell")](http://goodwell.iteye.com/)

[GoodWell](http://goodwell.iteye.com/ "GoodWell")
[![haiwuyanlzh的博客]( "haiwuyanlzh的博客: haiwuyanlzh")](http://haiwuyanlzh.iteye.com/)

[haiwuyanlzh](http://haiwuyanlzh.iteye.com/ "haiwuyanlzh")

[![hubert_bubert的博客]( "hubert_bubert的博客: ")](http://hubert-bubert.iteye.com/)

[hubert_bubert](http://hubert-bubert.iteye.com/ "hubert_bubert")

### 文章分类

* [全部博客 (21)](http://wangjie2013.iteye.com/)
* [JAVA (14)](http://wangjie2013.iteye.com/category/252443)
* [C (1)](http://wangjie2013.iteye.com/category/273482)
* [数据结构和算法 (0)](http://wangjie2013.iteye.com/category/273484)
* [jQuery (0)](http://wangjie2013.iteye.com/category/275854)
* [系统 (1)](http://wangjie2013.iteye.com/category/271730)
* [读书 (3)](http://wangjie2013.iteye.com/category/255333)
* [转载 (1)](http://wangjie2013.iteye.com/category/276592)
* [mysql (1)](http://wangjie2013.iteye.com/category/276804)
### 社区版块

* [我的资讯](http://wangjie2013.iteye.com/blog/news) (0)
* [我的论坛](http://wangjie2013.iteye.com/blog/post) (6)
* [我的问答](http://wangjie2013.iteye.com/blog/answered_problems) (0)

### 存档分类

* [2013-05](http://wangjie2013.iteye.com/blog/monthblog/2013-05) (1)
* [2013-04](http://wangjie2013.iteye.com/blog/monthblog/2013-04) (18)
* [2013-03](http://wangjie2013.iteye.com/blog/monthblog/2013-03) (1)
* [更多存档...](http://wangjie2013.iteye.com/blog/monthblog_more)
### 评论排行榜

* [试读《征服C指针》](http://wangjie2013.iteye.com/blog/1842689 "试读《征服C指针》")
* [单例设计模式](http://wangjie2013.iteye.com/blog/1844815 "单例设计模式")

### 最新评论

* [wangjie2013](http://wangjie2013.iteye.com/ "wangjie2013")： s1318601 写道讲下优点和适用范围及实际情况呗建议不错， ...
[单例设计模式](http://wangjie2013.iteye.com/blog/1844815#bc2307168)
* [s1318601](http://s1318601.iteye.com/ "s1318601")： 讲下优点和适用范围及实际情况呗
[单例设计模式](http://wangjie2013.iteye.com/blog/1844815#bc2307049)
* [wangjie2013](http://wangjie2013.iteye.com/ "wangjie2013")： yekui 写道买就不用了 ，直接去CSDN上去下就好了。地址 ...
[试读《征服C指针》](http://wangjie2013.iteye.com/blog/1842689#bc2307034)
* [wangjie2013](http://wangjie2013.iteye.com/ "wangjie2013")： yekui 写道只是只有存在你脑子里才是有价值的，并不是说买来 ...
[试读《征服C指针》](http://wangjie2013.iteye.com/blog/1842689#bc2307033)
* [yekui](http://yekui.iteye.com/ "yekui")： 只是只有存在你脑子里才是有价值的，并不是说买来的书，知识就能自 ...
[试读《征服C指针》](http://wangjie2013.iteye.com/blog/1842689#bc2306899)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2012 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![]()

This ad is supporting your extension Allow Right-Click: [More info]() | [Privacy Policy]() | [Hide on this page](http://wangjie2013.iteye.com/blog/1858541#)
