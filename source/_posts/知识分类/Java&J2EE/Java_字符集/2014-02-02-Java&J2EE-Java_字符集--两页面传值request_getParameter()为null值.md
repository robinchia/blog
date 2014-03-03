---
layout: post
title: "两页面传值request_getParameter()为null值"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_字符集
--- 

# 两页面传值request_getParameter()为null值

![]()

[**写词条，拿礼品—中国移动Wiki嘉年华**](http://events.csdn.net/wiki/index.html) [**浅谈并行编程中的任务分解模式**](http://g.csdn.net/5085045) [![]()](http://g.csdn.net/5078710) [![CSDN社区]()](http://community.csdn.net/)

[搜索](http://topic.csdn.net/t/20050119/21/3739242.html#) [收藏]( "收藏到我的网摘中，并分享给我的朋友") [打印](http://topic.csdn.net/t/20050119/21/3739242.html#) [关闭](http://topic.csdn.net/t/20050119/21/3739242.html#)

[CSDN社区](http://community.csdn.net/) >  [Java](http://community.csdn.net/Expert/ForumsList.asp?typenum=1&roomid=54) >  [Web 开发](http://community.csdn.net/Expert/ForumList.asp?typenum=1&roomid=5409)
# 两页面传值request.getParameter()为null值

[楼主]()sole_lodestar（弱势群体应该怎么办）2005-01-19 21:36:02 在 Java / Web 开发 提问

为什么在第二个页面使用request.getParameter()得不到上一页面提交过来的数据？  
   
  第一个页面：  
   
  <%@   page   contentType="text/html;charset=GBK"   errorPage="../include/error.jsp"%>  
  <HTML>  
  <BODY   BGCOLOR="white">  
   
  <H1>import_data</H1>  
  <HR>  
   
  <FORM   name="form1"   METHOD="POST"   ACTION="import_data_post.jsp"   ENCTYPE="multipart/form-data">  
  会计帐号：  
  <input   value="aaa"   type="text"   name="userName"   size="40"><br>  
  密码：  
  <input   value="aaa"   type="password"   name="userPass"   size="40"><br>  
  请选择要更新的表  
  <select   name="myTable">  
  <option   value="0">选择更新表</option>  
  <option   value="EXWCMWAGE"   selected>职工工资表</option>  
  <option   value="EXWCMITEM">项目资金表</option>  
  </select><br>  
  请选择要导入的文件：  
        <INPUT   TYPE="FILE"   NAME="myFile"   SIZE="50"><BR>  
        <INPUT   TYPE="SUBMIT"   VALUE="导入">  
        <INPUT   TYPE="reset"   VALUE="初始">  
  </FORM>  
   
  </BODY>  
  </HTML>  
   
  第二个页面：  
   
  <%@   page   contentType="text/html;charset=GBK"   %>  
  <%@   page   language="java"   import="java.sql.*"   %>  
  <%@   page   import="java.io.File"   %>  
  <%@   page   import="java.util.Date"   %>  
  <%@   page   import="java.io.*"   %>  
  <%@   page   import="java.lang.*"   %>  
  <%@   page   import="java.util.*"   %>  
  <%@   page   import="jxl.*"   %>  
  <%      
  //定义所有变量  
  String   userName   =   ""; //会计帐号  
  String   userPass   =   ""; //密码  
  String   myFile   =   ""; //数据源文件  
  String   myTable   =   ""; //所要更新的表  
   
   
  //取得提交数据  
  userName   =   request.getParameter("userName");  
  userPass   =   request.getParameter("userPass");  
  myFile   =   request.getParameter("myFile");  
  myTable   =   request.getParameter("myTable");  
                    ………………  
  问题点数：20、回复次数：11[Top](http://topic.csdn.net/t/20050119/21/3739242.html#)

### **[1 楼]()jFresH_MaN（十一月的萧邦－夜曲）**回复于 2005-01-19 21:42:56 得分 *20*

因为里面有一个上传文件，如果还有一般的字符串请求就得不到了  
  <INPUT   TYPE="FILE"   NAME="myFile"   SIZE="50">[Top](http://topic.csdn.net/t/20050119/21/3739242.html#)

### **[2 楼]()sole_lodestar（弱势群体应该怎么办）**回复于 2005-01-19 21:45:09 得分 0

那该怎么解决呢？[Top](http://topic.csdn.net/t/20050119/21/3739242.html#)

### **[3 楼]()sole_lodestar（弱势群体应该怎么办）**回复于 2005-01-19 21:49:52 得分 0

中间曾经把form的name属性改为form1,就可以了。可后来对第二页面修改后，又得不到值了。不知道为什么[Top](http://topic.csdn.net/t/20050119/21/3739242.html#)

### **[4 楼]()jFresH_MaN（十一月的萧邦－夜曲）**回复于 2005-01-19 21:50:34 得分 0

单独提交上传文件  
  不过现在一般都是使用jspsmartupload来上传文件，那样就没问题了[Top](http://topic.csdn.net/t/20050119/21/3739242.html#)

### **[5 楼]()sole_lodestar（弱势群体应该怎么办）**回复于 2005-01-19 21:54:33 得分 0

我并不是要上传文件，而是要在第二页面读取该文件内容。只要把我要上传的文件的完整路径（包括文件名）提交给第二个页面就可以了。[Top](http://topic.csdn.net/t/20050119/21/3739242.html#)

### **[6 楼]()sole_lodestar（弱势群体应该怎么办）**回复于 2005-01-19 21:55:23 得分 0

我并不是要上传文件，而是要在第二页面读取该文件内容。只要把我选中的文件的完整路径（包括文件名）提交给第二个页面就可以了。[Top](http://topic.csdn.net/t/20050119/21/3739242.html#)

### **[7 楼]()jFresH_MaN（十一月的萧邦－夜曲）**回复于 2005-01-19 21:58:24 得分 0

这样啊？  
  那你把ENCTYPE="multipart/form-data"去掉[Top](http://topic.csdn.net/t/20050119/21/3739242.html#)

### **[8 楼]()sole_lodestar（弱势群体应该怎么办）**回复于 2005-01-19 22:00:38 得分 0

嗯，对了。什么原因？  
  谢谢[Top](http://topic.csdn.net/t/20050119/21/3739242.html#)

### **[9 楼]()xuerenrenmin（）**回复于 2005-01-19 22:03:30 得分 0

把第一个页面改为  
  <%@   page   contentType="text/html;charset=GBK"   errorPage="../include/error.jsp"%>  
  <HTML>  
  <script   language="javascript">  
  function   Open_Post(){  
  var   userName=form1.UserName.value;  
  window.open("../import_data_post.jsp?userName="+userName);  
   
  }  
   
  </script>  
  <BODY   BGCOLOR="white">  
   
  <H1>import_data</H1>  
  <HR>  
   
  <FORM   name="form1"   METHOD="POST"   ACTION="import_data_post.jsp"   ENCTYPE="multipart/form-data">  
  会计帐号：  
  <input   value="aaa"   type="text"   name="userName"   size="40"><br>  
  密码：  
  <input   value="aaa"   type="password"   name="userPass"   size="40"><br>  
  请选择要更新的表  
  <select   name="myTable">  
  <option   value="0">选择更新表</option>  
  <option   value="EXWCMWAGE"   selected>职工工资表</option>  
  <option   value="EXWCMITEM">项目资金表</option>  
  </select><br>  
  请选择要导入的文件：  
        <INPUT   TYPE="FILE"   NAME="myFile"   SIZE="50"><BR>  
        <INPUT   TYPE="button"   VALUE="导入"   onclick="OpenPost()">  
        <INPUT   TYPE="reset"   VALUE="初始">  
  </FORM>  
   
  </BODY>  
  </HTML>  
   
  应当没有问题了[Top](http://topic.csdn.net/t/20050119/21/3739242.html#)

### **[10 楼]()jFresH_MaN（十一月的萧邦－夜曲）**回复于 2005-01-19 22:03:40 得分 0

原来提交得方式就把那个文件作为流来传输了，普通字符串就传不过去了！  
  去掉之后默认都是传递字符串，所以只把那个文本框里面得字符串提交过去！[Top](http://topic.csdn.net/t/20050119/21/3739242.html#)

### **[11 楼]()sole_lodestar（弱势群体应该怎么办）**回复于 2005-01-19 22:05:38 得分 0

送分啰[Top](http://topic.csdn.net/t/20050119/21/3739242.html#)

### **相关问题**

* [为什么getParameter取到的值都是null](http://topic.csdn.net/t/20050724/12/4164362.html)
* [关于传null值????](http://topic.csdn.net/t/20050816/10/4211151.html)
* [关于null值问题](http://topic.csdn.net/t/20010228/16/76866.html)
* [如何判断null值](http://topic.csdn.net/t/20020705/14/852564.html)
* [null值比较的问题](http://topic.csdn.net/t/20050815/19/4210371.html)
* [null 值查询的问题](http://topic.csdn.net/t/20051209/18/4449644.html)
* [为什么值为null？](http://topic.csdn.net/t/20060318/16/4623353.html)
* [http://topic.csdn.net/t/20050603/15/4057199.html](http://topic.csdn.net/t/20050603/15/4057199.html)
* [Request取值问题..............](http://topic.csdn.net/t/20050220/16/3793439.html)
* [怎样给指针赋值为NULL?](http://topic.csdn.net/t/20011003/18/310852.html)

### 关键词

### 得分解答快速导航

* 帖主：[sole_lodestar](http://topic.csdn.net/t/20050119/21/3739242.html#Top)
* [jFresH_MaN](http://topic.csdn.net/t/20050119/21/3739242.html#r_27296656)

### 相关链接

* [CSDN Java频道](http://java.csdn.net/)
* [Java类图书](http://www.dearbook.com.cn/Book/SearchBook.aspx?sortid=4&sorttype=smallsort)
* [Java类源码下载](http://www.codechina.net/resource/sort.php/21)

### 广告也精彩

### 反馈

请通过下述方式给我们反馈
![反馈]()

[x]() [![提问]()](http://g.csdn.net/5060169)
