---
layout: post
title: "XMLHttpRequest的POST中文表单问题解决方案 - esffor - JavaEye技术"
categories: Httpclient
tags: 
 - Httpclient
--- 

# XMLHttpRequest的POST中文表单问题解决方案 - esffor - JavaEye技术网站

[首页](http://www.javaeye.com/) [新闻](http://www.javaeye.com/news) [论坛](http://www.javaeye.com/forums) [博客](http://www.javaeye.com/blogs) [招聘](http://www.javaeye.com/job) [更多 ▼](http://esffor.javaeye.com/blog/96320#)

[问答](http://www.javaeye.com/ask) [知识库](http://www.javaeye.com/wiki) [圈子](http://www.javaeye.com/groups) [搜索](http://www.javaeye.com/google_search)

[您还未登录 !](http://esffor.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://esffor.javaeye.com/login) [注册](http://esffor.javaeye.com/signup)

# [esffor](http://esffor.javaeye.com/)

永久域名 [http://esffor.javaeye.com/](http://esffor.javaeye.com/)

[Hibernate优化查询性能手段](http://esffor.javaeye.com/blog/96019 "Hibernate优化查询性能手段") | [今天开始，回到Csdn Blog中](http://esffor.javaeye.com/blog/96485 "今天开始，回到Csdn Blog中")

2007-03-15

### [XMLHttpRequest的POST中文表单问题解决方案](http://esffor.javaeye.com/blog/96320)
XMLHttpRequest的POST中文表单问题解决方案

由于XMLHttpRequest POST的内容是用UTF-8编码，所以在服务端要先把request的编码改为UTF-8.
而且客户端post的表单是x-www-form-urlencoded的，所以也要对post的内容进行编码encodeURIComponent()函数
escape() 只是为 ASCII字符 做转换工作，转换成的 %unnnn 这样的码，如果要用更多的字符如 UTF-8字符库
就一定要用 encodeURIComponent() 或 encodeURI() 转换才可以成 %nn%nn
还有
escape() 不编码这些字符:  @*/+
encodeURI() 不编码这些字符:  [!@#$&*()=:/;?+'](mailto:!@)
encodeURIComponent() 不编码这些字符:  !*()'

还是推荐使用encodeURIComponent()函数来编码比较好。

代码如下：

在客户端的js脚本
<script>
function myXMLHttpRequest(){
 var xmlHttp = false;
 try {
  xmlHttp = new ActiveXObject("Msxml2.XMLHTTP");
 } catch (e) {
  try {
   xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
  } catch (e2) {
   xmlHttp = false;
  }
 }
 if (!xmlHttp && typeof XMLHttpRequest != 'undefined') {
  xmlHttp = new XMLHttpRequest();
 }
 return xmlHttp;
}

  content = "user="+**encodeURIComponent("大发")**;
  xmlHttp.Open("POST", "doc.jsp", false);
  xmlHttp.setRequestHeader("Content-Length",content.length);
  xmlHttp.setRequestHeader("CONTENT-TYPE","application/x-www-form-urlencoded");
  xmlHttp.Send(content);
</script>

JSP

<[%@page](mailto:%@page) language="java" contentType="text/html;charset=gbk"%>
<%
 **request.setCharacterEncoding("UTF-8");**
 System.out.println(request.getParameter("user"));
%>
 

[Hibernate优化查询性能手段](http://esffor.javaeye.com/blog/96019 "Hibernate优化查询性能手段") | [今天开始，回到Csdn Blog中](http://esffor.javaeye.com/blog/96485 "今天开始，回到Csdn Blog中")
* 18:05
* 浏览 (841)
* [评论](http://esffor.javaeye.com/blog/96320#comments) (0)
* [相关推荐](http://www.javaeye.com/wiki/topic/96320)

### 评论

[]()
### 发表评论

您还没有登录，请[登录](http://esffor.javaeye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![esffor的博客]( "esffor的博客: esffor")](http://esffor.javaeye.com/)

esffor

* 浏览: 242558 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 北京
* ![]()
* [详细资料](http://esffor.javaeye.com/blog/profile) [留言簿](http://esffor.javaeye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://esffor.javaeye.com/blog/user_visits)

[![xiehx的博客]( "xiehx的博客: ")](http://xiehx.javaeye.com/)

[xiehx](http://xiehx.javaeye.com/)

[![jpacm的博客]( "jpacm的博客: ")](http://jpacm.javaeye.com/)

[jpacm](http://jpacm.javaeye.com/)
[![shaochen的博客]( "shaochen的博客: ")](http://shaochen.javaeye.com/)

[shaochen](http://shaochen.javaeye.com/)

[![only_java的博客]( "only_java的博客: 用心学java")](http://only-java.javaeye.com/)

[only_java](http://only-java.javaeye.com/)

### 博客分类

* [全部博客 (628)](http://esffor.javaeye.com/)
* [Hibernate相关 (6)](http://esffor.javaeye.com/category/16050)
* [Spring相关 (6)](http://esffor.javaeye.com/category/16051)
* [JSF相关 (0)](http://esffor.javaeye.com/category/16052)
* [JSP/Servlet相关 (0)](http://esffor.javaeye.com/category/16053)
### 其他分类

* [我的收藏](http://esffor.javaeye.com/blog/favorite) (1)
* [我的论坛帖子](http://esffor.javaeye.com/blog/forum) (1)
* [我的精华良好贴](http://esffor.javaeye.com/blog/article) (0)

### 最近加入圈子
### 存档

* [2008-02](http://esffor.javaeye.com/blog/monthblog/2008-02) (25)
* [2008-01](http://esffor.javaeye.com/blog/monthblog/2008-01) (12)
* [2007-12](http://esffor.javaeye.com/blog/monthblog/2007-12) (105)
* [更多存档...](http://esffor.javaeye.com/blog/monthblog_more)

### 最新评论

* [hibernate查询方法选择（ ...](http://esffor.javaeye.com/blog/168307#comments "hibernate查询方法选择（List()与Iterator()）")
43385607 写道我觉得你说的有错误，list会利用二级缓存，但是前提是开启查 ...
-- by [yzzh9](http://547600.javaeye.com/)
* [JAVA中实现四舍五入的两种 ...](http://esffor.javaeye.com/blog/96158#comments "JAVA中实现四舍五入的两种方法")
这两种有什么区别吗？
-- by [有话给鬼说](http://luckpig.javaeye.com/)
* [Hibernate乐观锁实现之Ve ...](http://esffor.javaeye.com/blog/168243#comments "Hibernate乐观锁实现之Version")
好文章，非常感谢！
-- by [SpringJava](http://springjava.javaeye.com/)
* [使用循环遍历Set](http://esffor.javaeye.com/blog/96214#comments "使用循环遍历Set")
,我晕
-- by [还有也许](http://yanzhenwei.javaeye.com/)
* [使用javamail发送SMTP验证 ...](http://esffor.javaeye.com/blog/96304#comments "使用javamail发送SMTP验证邮件")
我也遇到这种情况，而且用其他邮箱发也直接进入了垃圾邮件箱，不知道怎么搞的？？
-- by [highping](http://highping.javaeye.com/)
### 评论排行榜

* [![Rss]()](http://esffor.javaeye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://esffor.javaeye.com/rss)
* [![Rss_zhuaxia]()](http://www.zhuaxia.com/add_channel.php?url=http://esffor.javaeye.com/rss)
* [![Rss_xianguo]()](http://www.xianguo.com/subscribe.php?url=http://esffor.javaeye.com/rss)
* [[什么是RSS?]](http://www.google.com/search?hl=zh-CN&q=RSS)
声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2009 JavaEye.com. All rights reserved. 上海炯耐计算机软件有限公司 [ 沪ICP备05023328号 ]
