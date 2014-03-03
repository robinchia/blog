---
layout: post
title: "在firefox里如何触发a中的Click方法？ Web 开发 - JavaScript - CSD"
categories: JS用法
tags: 
 - JS用法
--- 

# 在firefox里如何触发a中的Click方法？ Web 开发 - JavaScript - CSDN社区 community_csdn_net

![]()

[CSDN首页](http://www.csdn.net/) [空间](http://hi.csdn.net/) [新闻](http://news.csdn.net/) [论坛](http://bbs.csdn.net/) [Blog](http://blog.csdn.net/) [下载](http://download.csdn.net/) [读书](http://book.csdn.net/) [网摘](http://wz.csdn.net/) [搜索](http://search.csdn.net/) [.NET](http://dotnet.csdn.net/) [Java](http://java.csdn.net/) [视频](http://live.csdn.net/) [接项目](http://prj.csdn.net/) [求职](http://job.csdn.net/) [在线学习](http://www.itcast.net/) [买书](http://www.dearbook.com.cn/) [程序员](http://www.programmer.com.cn/) [通知](http://hi.csdn.net/Admin/NotifyList.aspx)

[**5-8万年薪顶级嵌入式，京沪深就业地**](http://www.uplooking.com/) [**浅谈并行编程中的任务分解模式**](http://g.csdn.net/5085045) [![]()](http://g.csdn.net/5078710)
[![CSDN社区]()](http://community.csdn.net/)

[搜索](http://topic.csdn.net/t/20060901/11/4991364.html#) [收藏]( "收藏到我的网摘中，并分享给我的朋友") [打印](http://topic.csdn.net/t/20060901/11/4991364.html#) [关闭](http://topic.csdn.net/t/20060901/11/4991364.html#)

[CSDN社区](http://community.csdn.net/) >  [Web 开发](http://community.csdn.net/Expert/ForumsList.asp?typenum=1&roomid=3) >  [JavaScript](http://community.csdn.net/Expert/ForumList.asp?typenum=1&roomid=304)
# 在firefox里如何触发<a>中的Click方法？

[楼主]()zhujiechang（小朱）2006-09-01 11:14:05 在 Web 开发 / JavaScript 提问

<a   href="javascript:test(1)"   id="a3">hello</a>  
  <a   href="javascript:test2(1)"   id="b3">hello2</a>  
  function   test(num)  
          {  
                  window.alert(num);  
          }  
          function   test2(num)  
          {  
                    //下面这句在firefox里面不能执行  
                    document.getElementById("a3").click();          
          }  
  请问怎么调用<a>的Click方法？ 问题点数：30、回复次数：7[Top](http://topic.csdn.net/t/20060901/11/4991364.html#)

### **[1 楼]()0009（夏天以南）**回复于 2006-09-01 11:36:56 得分 0

好像是没什么好的办法  
   
  垃圾FF[Top](http://topic.csdn.net/t/20060901/11/4991364.html#)

### **[2 楼]()hbhbhbhbhb1021（天外水火(我要多努力)）**回复于 2006-09-01 11:50:41 得分 0

可以实现的  
  <a   href="#"   onclick="test(1)"   id="a3">hello</a>  
  <a   href="#"   onclick="test2(1)"   id="b3">hello2</a>  
  <script   language=javascript>  
  function   test(num)  
          {  
                  window.alert(num);  
          }  
          function   test2(num)  
          {  
                    //下面这句在firefox里面不能执行  
                    document.getElementById("a3").click();          
          }  
          </script>[Top](http://topic.csdn.net/t/20060901/11/4991364.html#)

### **[3 楼]()ice_berg16（寻梦的稻草人）**回复于 2006-09-01 12:00:25 得分 *25*

<a   href="#"   onclick="test(1)"   id="a3">hello</a>  
  <a   href="#"   onclick="test2(1)"   id="b3">hello2</a>  
  <script   language="javascript">  
  <!--  
  function   test(num)  
          {  
                  window.alert(num);  
          }  
          function   test2(num)  
          {  
                    if(document.all)  
                    document.getElementById("a3").click();          
    else  
    {  
    var   evt   =   document.createEvent("MouseEvents");  
    evt.initEvent("click",   true,   true);  
    document.getElementById("a3").dispatchEvent(evt);  
    }  
          }  
   
  //-->  
  </script>[Top](http://topic.csdn.net/t/20060901/11/4991364.html#)

### **[4 楼]()zhujiechang（小朱）**回复于 2006-09-01 12:01:29 得分 0

Error:   document.getElementById("a3").click   is   not   a   function  
  Line:   106  
   
  楼上，还是不行啊。firefox中有这样的问题，有什么好的解决方案或者替代的方法？[Top](http://topic.csdn.net/t/20060901/11/4991364.html#)

### **[5 楼]()zhujiechang（小朱）**回复于 2006-09-01 12:02:52 得分 0

谢谢了   ice_berg16(寻梦的稻草人)    
   
  这样才行。[Top](http://topic.csdn.net/t/20060901/11/4991364.html#)

### **[6 楼]()hbhbhbhbhb1021（天外水火(我要多努力)）**回复于 2006-09-01 12:03:00 得分 *5*

SORRY,我的代码帖错了，把测试的代码帖上来了[Top](http://topic.csdn.net/t/20060901/11/4991364.html#)

### **[7 楼]()mingxuan3000（铭轩）**回复于 2006-09-01 12:22:19 得分 0

mark[Top](http://topic.csdn.net/t/20060901/11/4991364.html#)

### **相关问题**

### 关键词

### 得分解答快速导航

* 帖主：[zhujiechang](http://topic.csdn.net/t/20060901/11/4991364.html#Top)
* [ice_berg16](http://topic.csdn.net/t/20060901/11/4991364.html#r_36492333)
* [hbhbhbhbhb1021](http://topic.csdn.net/t/20060901/11/4991364.html#r_36492404)

### 相关链接

* [Web开发类图书](http://www.dearbook.com.cn/Book/SearchBook.aspx?sortid=13&sorttype=bigsort)

### 广告也精彩

### 反馈

请通过下述方式给我们反馈
![反馈]()

[x]() [![提问]()](http://g.csdn.net/5060169)     [网站简介](http://www.csdn.net/help/intro.htm)|[广告服务](http://www.csdn.net/help/ads.htm)|[VIP资费标准](http://www.csdn.net/help/vip.htm)|[银行汇款帐号](http://www.csdn.net/help/bankaccount.htm)|[网站地图](http://www.csdn.net/help/sitemap.htm)|[帮助](http://www.csdn.net/help/help.htm)|[联系方式](http://www.csdn.net/help/contact.htm)|[诚聘英才](http://job.csdn.net/Jobs/f9c75c9f2ad14404a604669b757b9ed0/viewcompany.aspx)|[English](http://www.csdn.net/help/english.htm)|[问题报告](http://topic.csdn.net/t/20060901/11/4991364.html#)北京创新乐知广告有限公司 版权所有, 京 ICP 证 070598 号世纪乐知(北京)网络技术有限公司 提供技术支持CSDN网站24小时值班电话：13552009689Copyright © 2000-2009, CSDN.NET, All Rights Reserved[![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
