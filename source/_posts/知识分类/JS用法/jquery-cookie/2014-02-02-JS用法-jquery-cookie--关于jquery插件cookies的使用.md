---
layout: post
title: "关于jquery插件cookies的使用"
categories: JS用法
tags: 
 - JS用法
 - jquery-cookie
--- 

# 关于jquery插件cookies的使用

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼]()

[招聘](http://www.iteye.com/job) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://fireflywithcat.iteye.com/login "登录") [登录](http://fireflywithcat.iteye.com/login) [注册](http://fireflywithcat.iteye.com/signup)

# [fireflywithcat](http://fireflywithcat.iteye.com/)

* [**博客**](http://fireflywithcat.iteye.com/)
* [微博](http://fireflywithcat.iteye.com/weibo)
* [相册](http://fireflywithcat.iteye.com/album)
* [收藏](http://fireflywithcat.iteye.com/link)
* [留言](http://fireflywithcat.iteye.com/blog/guest_book)
* [关于我](http://fireflywithcat.iteye.com/blog/profile)

### [关于jquery插件cookies的使用](http://fireflywithcat.iteye.com/blog/1581529) **

**博客分类：**
* [jquery](http://fireflywithcat.iteye.com/category/231916)
[cookie](http://www.iteye.com/blogs/tag/cookie)[jquery](http://www.iteye.com/blogs/tag/jquery)[html](http://www.iteye.com/blogs/tag/html) 

下面就是一个例子：不过没有设置 domain、path、hoursToLive……几个属性，所以一旦关闭浏览器就不会保存cookie了。
Html代码  [![收藏代码]()![]()]( "收藏这段代码")

1. <html>  
1. <head>  
1. <script type="text/javascript" src="jquery.js"></script> //插入js  
1. <script type="text/javascript" src="jquery.cookies.2.2.0.min.js"></script>//插入插件。注意：这个插件一定要放在jquery文件的后面，否则不会生效  
1. <script type="text/javascript">  
1. $(document).ready(function(){  
1. $(".tijiao").click(function(){   //触发button事件  
1. $.cookies.set('name',$(".userName").value);  
1. $.cookies.set('password',$(".password").value);   //把表单中得到的value存入cookie  
1. alert("已经存入cookie");  
1. });  
1. var userN=$.cookies.get( 'name' );    
1. var userP=$.cookies.get( 'password' ); //从cookie中读取数据  
1. if(userN!=""&&userN!=null&&userP!=""&&userP!=null)  
1. {$(".userName").value=userN;  
1. $(".password").value=userP;   // 赋值  
1. }  
1. else{  
1. alert("userName or password is not exist");};  
1. })  
1. </script>  
1. </head>  
1. <body>  
1. <div>  
1. <table>  
1. <tr id="name">  
1. <td><lable>用户名：</lable></td>  
1. <td><input type="text" class="userName" value=""/></td>  
1. </tr>  
1. <tr>  
1. <td><lable>密码：</lable></td>  
1. <td><input type="password1" class="password" value=""/></td>  
1. </tr>  
1. <tr>  
1. <td><input type="button" class="tijiao" value="登录"/></td>  
1. </tr>  
1. </table>  
1. </div>  
1. </body>  
1. </html>  
<html> <head> <script type="text/javascript" src="jquery.js"></script> //插入js <script type="text/javascript" src="jquery.cookies.2.2.0.min.js"></script>//插入插件。注意：这个插件一定要放在jquery文件的后面，否则不会生效 <script type="text/javascript"> $(document).ready(function(){ $(".tijiao").click(function(){ //触发button事件 $.cookies.set('name',$(".userName").value); $.cookies.set('password',$(".password").value); //把表单中得到的value存入cookie alert("已经存入cookie"); }); var userN=$.cookies.get( 'name' ); var userP=$.cookies.get( 'password' ); //从cookie中读取数据 if(userN!=""&&userN!=null&&userP!=""&&userP!=null) {$(".userName").value=userN; $(".password").value=userP; // 赋值 } else{ alert("userName or password is not exist");}; }) </script> </head> <body> <div> <table> <tr id="name"> <td><lable>用户名：</lable></td> <td><input type="text" class="userName" value=""/></td> </tr> <tr> <td><lable>密码：</lable></td> <td><input type="password1" class="password" value=""/></td> </tr> <tr> <td><input type="button" class="tijiao" value="登录"/></td> </tr> </table> </div> </body> </html>
要想设置cookie的其他属性就看这个：
Js代码  [![收藏代码]()![]()]( "收藏这段代码")

1. var newOptions = {  
1.     domain: '*.mydomain.com',//设置域  
1.     path: '/somedir',//设置路径  
1.     expiresAt: new Date( 2011, 1, 1 ),//设置cookie失效时间  
1.     hoursToLive:0.01//设置cookie存活时间  
1.     secure: true//是否保护  
1.   }  
var newOptions = { domain: '*.mydomain.com',//设置域 path: '/somedir',//设置路径 expiresAt: new Date( 2011, 1, 1 ),//设置cookie失效时间 hoursToLive:0.01//设置cookie存活时间 secure: true//是否保护 }
详情请查看：
http://code.google.com/p/cookies/wiki/Documentation#Test_if_browser_is_accepting_cookies
**1**
顶

**0**
踩

分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[Jquery获取select标签的值、文本方式](http://fireflywithcat.iteye.com/blog/1581183 "Jquery获取select标签的值、文本方式")
* 37 分钟前
* 浏览 15
* [评论(0)]()
* 分类:[编程语言](http://www.iteye.com/blogs/category/language)
* [相关推荐](http://www.iteye.com/wiki/blog/1581529)

### 评论

[]()
### 发表评论

[![]()](http://fireflywithcat.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://fireflywithcat.iteye.com/login)

[![FireFlyWithCat的博客]( "FireFlyWithCat的博客: ")](http://fireflywithcat.iteye.com/)

FireFlyWithCat

* 浏览: 986 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 北京
* ![]()
### 最近访客 [更多访客>>](http://fireflywithcat.iteye.com/blog/user_visits)

[![kamuikyo的博客]( "kamuikyo的博客: 有云的天空")](http://kamuikyo.iteye.com/)

[kamuikyo](http://kamuikyo.iteye.com/ "kamuikyo")

[![jinnianshilongnian的博客]( "jinnianshilongnian的博客: 私塾在线网 http://sishuok.com")](http://jinnianshilongnian.iteye.com/)

[jinnianshilongnian](http://jinnianshilongnian.iteye.com/ "jinnianshilongnian")
[![myjsj_2006的博客]( "myjsj_2006的博客: ")](http://myjsj-2006.iteye.com/)

[myjsj_2006](http://myjsj-2006.iteye.com/ "myjsj_2006")

[![e04610112的博客]( "e04610112的博客: ")](http://e04610112.iteye.com/)

[e04610112](http://e04610112.iteye.com/ "e04610112")

### 文章分类

* [全部博客 (13)](http://fireflywithcat.iteye.com/)
* [grails&groovy (5)](http://fireflywithcat.iteye.com/category/228603)
* [html (5)](http://fireflywithcat.iteye.com/category/228604)
* [CSS (1)](http://fireflywithcat.iteye.com/category/229570)
* [jquery (2)](http://fireflywithcat.iteye.com/category/231916)
### 社区版块

* [我的资讯](http://fireflywithcat.iteye.com/blog/news) (0)
* [我的论坛](http://fireflywithcat.iteye.com/blog/post) (0)
* [我的问答](http://fireflywithcat.iteye.com/blog/answered_problems) (0)

### 存档分类

* [2012-07](http://fireflywithcat.iteye.com/blog/monthblog/2012-07) (2)
* [2012-06](http://fireflywithcat.iteye.com/blog/monthblog/2012-06) (11)
* [更多存档...](http://fireflywithcat.iteye.com/blog/monthblog_more)
### 评论排行榜

* [在grails框架中导入groovy脚本方法（四）](http://fireflywithcat.iteye.com/blog/1566819 "在grails框架中导入groovy脚本方法（四）")

### 最新评论

* [agile_boy](http://agile-boy.iteye.com/ "agile_boy")： 不建议在Grails这样使用Groovy
[在grails框架中导入groovy脚本方法（四）](http://fireflywithcat.iteye.com/blog/1566819#bc2266078)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![]()
