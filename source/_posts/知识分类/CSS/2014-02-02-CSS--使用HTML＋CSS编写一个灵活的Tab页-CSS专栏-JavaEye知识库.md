---
layout: post
title: "使用HTML＋CSS编写一个灵活的Tab页 - CSS专栏 - JavaEye知识库"
categories: CSS
tags: 
 - CSS
--- 

# 使用HTML＋CSS编写一个灵活的Tab页 - CSS专栏 - JavaEye知识库

[您还未登录 !](http://www.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://www.javaeye.com/login) [注册](http://www.javaeye.com/signup)

[![JavaEye3.0]( "JavaEye-最棒的软件开发交流社区")](http://www.javaeye.com/)

[![第四届 D2前端技术论坛 (12月19日·杭州)]()](http://www.d2forum.org/d2/4/)

[]()

[知识库](http://www.javaeye.com/wiki)  →  [专栏: CSS专栏](http://www.javaeye.com/wiki/CSS)  →  [使用HTML＋CSS编写一个灵活的Tab页]()
# 使用HTML＋CSS编写一个灵活的Tab页

原创作者: [downpour](http://www.javaeye.com/topic/52036)   阅读:892次   评论:0条   更新时间:2007-02-12    

最近在研究CSS，正好结合项目做了一个灵活的Tab页，使用纯HTML＋CSS实现，正好总结一下。
首先看一下预览界面：
![]()
样例HTML可以访问：http://www.demo2do.com/htmldemo/school/attendance/AttendanceGlobal.html
下面开始讲述一下完成上述页面的步骤。
1. 构建HTML
构建HTML是整个过程最基础的部分。我们构建HTML比较关键的一个原则就是“还HTML标签其本来的含义”。所以在这里，我们应该合理分析一下期望做到的HTML的结构的情况，并加以分析，选择比较合适的HTML标签，而不是采用非标准的Table布局或者充斥着大量div和class的布局方式。事实上，现在存在着一种误区，就是凡事采用了DIV＋CSS的方式进行页面编程的就是Web标准的，其实这是完全错误的观点，很容易就导致了“多div症”（divitus）或者“多类症”（classitis）。
回到正题，我们分析一下页面样式，可以将整个Tab页分成2个部分，分别是一级菜单和二级菜单，他们有类似的特点，并以横向方式排列。HTML标签中的无序列表就可以反映出这种逻辑关系。所以我们分别采用2个无序列表来表示一级菜单和二级菜单。代码如下：
Java代码

1. <div class="navg">  
1.     <div id="attendance" class="mainNavg">  
1.         <ul>  
1.             <li id="attendanceNavg"><a href="#">考勤管理</a></li>  
1.             <li id="teachNavg"><a href="#">教学管理</a></li>  
1.             <li id="communicationNavg"><a href="#">家校互通</a></li>  
1.             <li id="systemNavg"><a href="#">系统管理</a></li>  
1.         </ul>  
1.     </div>      
1.     <div id="dailyAttendance" class="secondaryNavg">  
1.         <ul>  
1.             <li id="dailyAttendanceNavg"><a href="#">当天考勤</a></li>  
1.             <li id="leaveApproveNavg"><a href="#">请假审批</a></li>  
1.             <li id="attendanceStatisticsNavg"><a href="#">考勤统计</a></li>  
1.             <li id="attendanceCollectNavg"><a href="#">考勤汇总</a></li>  
1.         </ul>  
1.     </div>  
1. </div>  
<div class="navg"> <div id="attendance" class="mainNavg"> <ul> <li id="attendanceNavg"><a href="#">考勤管理</a></li> <li id="teachNavg"><a href="#">教学管理</a></li> <li id="communicationNavg"><a href="#">家校互通</a></li> <li id="systemNavg"><a href="#">系统管理</a></li> </ul> </div> <div id="dailyAttendance" class="secondaryNavg"> <ul> <li id="dailyAttendanceNavg"><a href="#">当天考勤</a></li> <li id="leaveApproveNavg"><a href="#">请假审批</a></li> <li id="attendanceStatisticsNavg"><a href="#">考勤统计</a></li> <li id="attendanceCollectNavg"><a href="#">考勤汇总</a></li> </ul> </div> </div>
其中，2个div将菜单级别划分开。其实在以后还会有其他的功效。此时，我们不妨View一下这张页面，我们可以惊喜的发现，这张页面就想Word文档一样，是可读的，这一点我们可以在整个过程做完以后再一次验证。
![]()
2. 构建基本CSS
先简单的让ul横向排列，这里面要注意元素float之后要注意清理
然后通过分别在LI 和 A 元素上应用背景来实现主菜单样式，这里有个比较重要的地方是A这个元素变成块级元素（display: block），这样可以便于我们下面做一些处理，也能使整个菜单应用到链接样式。
而其中的line-height，恰恰可以使A中的字纵向居中。text-align使得A中的字横向居中。
Java代码

1. .navg .mainNavg UL {  
1.     margin: 0;  
1.     padding: 0;  
1.     list-style: none;  
1. }  
1. .navg .mainNavg UL LI {  
1.     float: left;      
1.     background-color: #E1E9F8;  
1.     background: url(../images/tab_right.gif) no-repeat right top;  
1.     margin: 10px 3px;  
1.     height: 25px;  
1. }  
1.   
1. .navg .mainNavg UL LI A {  
1.     display: block;  
1.     height: 25px;  
1.     padding: 0 25px;  
1.     line-height: 24px;  
1.     background-color: #E1E9F8;  
1.     background: url(../images/tab_left.gif) no-repeat left top;  
1.     text-decoration: none;  
1.     float: left;  
1.     text-align:center;  
1.     color: #fff;  
1.     font-weight: bold;    
1. }  
.navg .mainNavg UL { margin: 0; padding: 0; list-style: none; } .navg .mainNavg UL LI { float: left; background-color: #E1E9F8; background: url(../images/tab_right.gif) no-repeat right top; margin: 10px 3px; height: 25px; } .navg .mainNavg UL LI A { display: block; height: 25px; padding: 0 25px; line-height: 24px; background-color: #E1E9F8; background: url(../images/tab_left.gif) no-repeat left top; text-decoration: none; float: left; text-align:center; color: #fff; font-weight: bold; }
3. 使宽度自适应
我们在这里使用滑动门技术来做宽度自适应。下面简单介绍一下滑动门技术
简单来说，就是在LI上应用一幅大图像背景，并让这个背景居于右侧
![]()
然后在A上应用一个小图像背景，并让这个背景居于左侧，遮住大图像边缘
![]()
这样无论菜单文字内容长度怎么变，都不会破坏原来的结构了。
4. 当前菜单高亮显示
如果高亮当前页面，这个有很多种做法，最死板的是在每个页面上显式的定义类。
但是对于web项目来说，页面多数是动态的，所以这样不是最理想的方法。
我这里采用的方法是CSS选择器的灵活使用
Java代码

1. #attendance #attendanceNavg,  
1. #teach #teachNavg,  
1. #communication #communicationNavg,  
1. #system #systemNavg {  
1.     background: url(../images/tab_right_on.gif) no-repeat right top;  
1. }  
1. #attendance #attendanceNavg A,  
1. #teach #teachNavg A,  
1. #communication #communicationNavg A,  
1. #system #systemNavg A {  
1.     background: url(../images/tab_left_on.gif) no-repeat left top;  
1.     color: #0000ff;  
1. }  
#attendance #attendanceNavg, #teach #teachNavg, #communication #communicationNavg, #system #systemNavg { background: url(../images/tab_right_on.gif) no-repeat right top; } #attendance #attendanceNavg A, #teach #teachNavg A, #communication #communicationNavg A, #system #systemNavg A { background: url(../images/tab_left_on.gif) no-repeat left top; color: #0000ff; }
在<div id="attendance" class="mainNavg">的代码中，我们可以使用不同的id作为选择器，由于CSS中的选择器id的优先级将大于class，所以只要根据id配合上li上面的id，就可以达到动态选择高亮选中的目的。
事实上，由于我们的页面都是动态的，所以id可以由后台生成，这样就可以通过id的不同组合非常精巧的实现了我们的需求。
5. 小技巧
最后可能还有一个问题你在想怎么实现的，就是高亮的tab如何把下面的横线遮掉的
很简单，图片上的小技巧。将高亮的图片高度设置为25px，而普通的图片设置为24px。然后通过padding，就可以将那根横线遮去了。
我们可以使用类似的方式，把二级菜单也做出来，这里就不详细叙述了。大家可以结合源码试一下。
附件为
[css( 转)](http://www.javaeye.com/wiki/CSS/820-css(%20%E8%BD%AC) "css( 转)") | [css基础--滤镜特效](http://www.javaeye.com/wiki/CSS/822-css%E5%9F%BA%E7%A1%80--%E6%BB%A4%E9%95%9C%E7%89%B9%E6%95%88 "css基础--滤镜特效")

评论 共 0 条 [发表评论](http://www.javaeye.com/wiki/CSS/821-%E4%BD%BF%E7%94%A8HTML%EF%BC%8BCSS%E7%BC%96%E5%86%99%E4%B8%80%E4%B8%AA%E7%81%B5%E6%B4%BB%E7%9A%84Tab%E9%A1%B5#) []()
### 发表评论

### 表情图标

![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()

字体颜色: 标准深红红色橙色棕色黄色绿色橄榄青色蓝色深蓝靛蓝紫色灰色白色黑色 字体大小: 标准1 (xx-small)2 (x-small)3 (small)4 (medium)5 (large)6 (x-large)7 (xx-large) 对齐: 标准居左居中居右

提示：选择您需要装饰的文字, 按上列按钮即可添加上相应的标签
您还没有登录，请[登录](http://www.javaeye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

### 文章信息

[![CSS专栏专栏]( "CSS专栏: CSS专栏，07年将是web标准大面积推广的年份 希望和大家一起交流基于web标准的web重构之道。")](http://www.javaeye.com/wiki/CSS)  [专栏: CSS专栏](http://www.javaeye.com/wiki/CSS)

* 由[kj23](http://kj23.javaeye.com/)在2007-02-12创建
* 由[kj23](http://kj23.javaeye.com/)在2007-02-12更新

### 相关新闻

* [最常用和实用的CSS技巧](http://www.javaeye.com/news/3176 "最常用和实用的CSS技巧")
* [Firefox支持CSS3多重背景](http://www.javaeye.com/news/6504-firefox-support-for-css3-multiple-backgrounds "Firefox支持CSS3多重背景")
* [CSS Sprites + 圆角](http://www.javaeye.com/news/7417-css-sprites-fillet "CSS Sprites + 圆角")
### 相关讨论

* [使用HTML＋CSS编写一个灵活的Tab页](http://www.javaeye.com/topic/52036)
* [最简单的css滑动门技术](http://www.javaeye.com/topic/293626)
* [组织架构图（水平方向的树视图）的实现](http://www.javaeye.com/topic/119542)
* [用CSS+JQuery实现的动态导航](http://www.javaeye.com/topic/512273)
* [javascript + css 利用div的scroll属性让TAB动感十足](http://www.javaeye.com/topic/382190)

### 相关博客

* [使用HTML＋CSS编写一个灵活的Tab页](http://leowzy.javaeye.com/blog/510720)
* [使用HTML＋CSS编写一个灵活的Tab页](http://downpour.javaeye.com/blog/52036)
* [［转］使用HTML＋CSS编写一个灵活的Tab页](http://bea.javaeye.com/blog/155465)
* [(续)精彩导航](http://tiger-passion.javaeye.com/blog/122874)
* [支持IE和FF的div+css选项卡](http://lovephoenix.javaeye.com/blog/116849)
* [首页](http://www.javaeye.com/)
* [新闻](http://www.javaeye.com/news)
* [论坛](http://www.javaeye.com/forums)
* [问答](http://www.javaeye.com/ask)
* [知识库](http://www.javaeye.com/wiki)
* [博客](http://www.javaeye.com/blogs)
* [圈子](http://www.javaeye.com/groups)
* [猎头](http://www.javaeye.com/hunters)
* [招聘](http://www.javaeye.com/job)
* [服务](http://www.javaeye.com/index/service)
* [搜索](http://www.javaeye.com/google_search)

* [Java](http://java.javaeye.com/)
* [Web](http://webapp.javaeye.com/)
* [Ruby](http://ruby.javaeye.com/)
* [Python](http://python.javaeye.com/)
* [敏捷](http://agile.javaeye.com/)
* [MySQL](http://mysql.javaeye.com/)
* [普元](http://primeton.javaeye.com/)
* [Dorado](http://dorado.javaeye.com/)
* [金蝶中间件](http://kingdee.javaeye.com/)
* [图书](http://book.javaeye.com/)
* [广告服务](http://www.javaeye.com/index/service)
* [JavaEye黑板报](http://webmaster.javaeye.com/)
* [关于我们](http://www.javaeye.com/index/aboutus)
* [联系我们](http://www.javaeye.com/index/contactus)
* [友情链接](http://www.javaeye.com/index/friend_links)

© 2003-2009 JavaEye.com. All rights reserved. [ 沪ICP备05023328号 ] ![]()
