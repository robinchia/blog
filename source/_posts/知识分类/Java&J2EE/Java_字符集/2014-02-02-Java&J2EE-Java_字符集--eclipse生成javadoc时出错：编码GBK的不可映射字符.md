---
layout: post
title: "eclipse生成javadoc时出错：编码GBK的不可映射字符"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_字符集
--- 

# eclipse生成javadoc时出错：编码GBK的不可映射字符

[首页](http://www.javaeye.com/) [新闻](http://www.javaeye.com/news) [论坛](http://www.javaeye.com/forums) [问答](http://www.javaeye.com/ask) [博客](http://www.javaeye.com/blogs) [招聘](http://www.javaeye.com/job) [更多 ▼](http://huibin.javaeye.com/blog/458902#)

[专栏](http://www.javaeye.com/wiki) [文摘](http://www.javaeye.com/articles) [圈子](http://www.javaeye.com/groups) [搜索](http://www.javaeye.com/google_search)

[您还未登录 !](http://huibin.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://huibin.javaeye.com/login) [注册](http://huibin.javaeye.com/signup)

# [子夜轻风](http://huibin.javaeye.com/)

永久域名 [http://huibin.javaeye.com/](http://huibin.javaeye.com/)

[java中的注释规范](http://huibin.javaeye.com/blog/458904 "java中的注释规范") | [异常备忘：java.lang.UnsupportedClassVers ...](http://huibin.javaeye.com/blog/458898 "异常备忘：java.lang.UnsupportedClassVersionError: Bad version number in .class file ")

2009-08-31

### [eclipse生成javadoc时出错：编码GBK的不可映射字符](http://huibin.javaeye.com/blog/458902)
 
![]()
 由于java源代码是用的UTF-8编码，[Eclipse](http://618119.com/tag/eclipse "标签 eclipse 下的日志")中默认编码是GB18030，因此，在生成javadoc的时候，需要手工指定一下编码和字符集。
主菜单–>Project–>Generate javadoc–>next–>next–> 在 “Extra javadoc options”下面的文本框中填入 ” -encoding UTF-8 -charset UTF-8 “.

![]()

* [![]( "点击查看原始大小图片")](http://dl.javaeye.com/upload/attachment/141029/2ea27dbc-56c4-30c6-a6ef-02c2e51464ce.jpg)
* 大小: 105.4 KB

* [![]( "点击查看原始大小图片")](http://dl.javaeye.com/upload/attachment/141031/894c3327-9d49-3a43-ae8a-8bf134a62389.jpg)
* 大小: 50.3 KB

* [查看图片附件](http://huibin.javaeye.com/blog/458902#)
[java中的注释规范](http://huibin.javaeye.com/blog/458904 "java中的注释规范") | [异常备忘：java.lang.UnsupportedClassVers ...](http://huibin.javaeye.com/blog/458898 "异常备忘：java.lang.UnsupportedClassVersionError: Bad version number in .class file ")

* 08:30
* 浏览 (275)
* [评论](http://huibin.javaeye.com/blog/458902#comments) (1)
* 分类: [JAVA](http://huibin.javaeye.com/category/71100)
* [相关推荐](http://www.javaeye.com/wiki/topic/458902)
### 评论

[]()

1 楼 [zyhui98](http://zyhui98.javaeye.com/) 2009-12-17   [引用](http://huibin.javaeye.com/blog/458902#)

太好了，今天就遇到了这个问题。lz很厉害！不过
有下面的问题
正在构造 Javadoc 信息...
E:\JAVA\Gwap-struts2\src\cn\com\tarena\ecport\listener\HibernateListener.java:3: 软件包 javax.servlet 不存在
import javax.servlet.ServletContextEvent;

### 发表评论

您还没有登录，请[登录](http://huibin.javaeye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![huibin的博客]( "huibin的博客: 子夜轻风")](http://huibin.javaeye.com/)

huibin

* 浏览: 14372 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 郑州
* ![]()
* [详细资料](http://huibin.javaeye.com/blog/profile) [留言簿](http://huibin.javaeye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://huibin.javaeye.com/blog/user_visits)

[![Asdpboy的博客]( "Asdpboy的博客: ")](http://asdpboy.javaeye.com/)

[Asdpboy](http://asdpboy.javaeye.com/)

[![tmgs2001的博客]( "tmgs2001的博客: tmgs2001")](http://tmgs2001.javaeye.com/)

[tmgs2001](http://tmgs2001.javaeye.com/)
[![yufu863的博客]( "yufu863的博客: ")](http://yufu863.javaeye.com/)

[yufu863](http://yufu863.javaeye.com/)

[![john_chen的博客]( "john_chen的博客: john_chen")](http://john-chen.javaeye.com/)

[john_chen](http://john-chen.javaeye.com/)

### 博客分类

* [全部博客 (140)](http://huibin.javaeye.com/)
* [JAVA (24)](http://huibin.javaeye.com/category/71100)
* [ORACLE (10)](http://huibin.javaeye.com/category/71101)
* [HIBERNATE (0)](http://huibin.javaeye.com/category/71102)
* [SPRING (23)](http://huibin.javaeye.com/category/71103)
* [STRUTS (1)](http://huibin.javaeye.com/category/71104)
* [OTHERS (0)](http://huibin.javaeye.com/category/71106)
* [MYSQL (3)](http://huibin.javaeye.com/category/71105)
* [Struts2 (10)](http://huibin.javaeye.com/category/75140)
* [JS (22)](http://huibin.javaeye.com/category/75141)
* [Tomcat (1)](http://huibin.javaeye.com/category/75184)
* [DWR (0)](http://huibin.javaeye.com/category/75185)
* [JQuery (4)](http://huibin.javaeye.com/category/75186)
* [JBoss (0)](http://huibin.javaeye.com/category/75187)
* [SQL SERVER (0)](http://huibin.javaeye.com/category/75188)
* [XML (6)](http://huibin.javaeye.com/category/77537)
* [生活 (2)](http://huibin.javaeye.com/category/79793)
* [JSP (1)](http://huibin.javaeye.com/category/82603)
* [CSS (1)](http://huibin.javaeye.com/category/86849)
* [word (1)](http://huibin.javaeye.com/category/88039)
* [MyEclipse (1)](http://huibin.javaeye.com/category/92891)
* [JSTL (1)](http://huibin.javaeye.com/category/93014)
* [JEECMS (2)](http://huibin.javaeye.com/category/93212)
* [Freemarker (6)](http://huibin.javaeye.com/category/93680)
* [页面特效 (1)](http://huibin.javaeye.com/category/93796)
* [EXT (1)](http://huibin.javaeye.com/category/93798)
* [Web前端 js库 (1)](http://huibin.javaeye.com/category/93800)
* [JSON http://www.json.org (3)](http://huibin.javaeye.com/category/94851)
* [代码收集 (1)](http://huibin.javaeye.com/category/94771)
* [电脑常识 (1)](http://huibin.javaeye.com/category/94044)
* [MD5加密 (0)](http://huibin.javaeye.com/category/94875)
* [Axis (0)](http://huibin.javaeye.com/category/94894)
* [Grails (1)](http://huibin.javaeye.com/category/94899)
* [浏览器 (1)](http://huibin.javaeye.com/category/95327)
* [js调试工具 (1)](http://huibin.javaeye.com/category/95334)
* [WEB前端 (3)](http://huibin.javaeye.com/category/99235)
* [JDBC (2)](http://huibin.javaeye.com/category/99262)
### 我的相册

[![]()](http://huibin.javaeye.com/album)QQ截图未命名_conew1.png
[共 1 张](http://huibin.javaeye.com/album)

### 其他分类

* [我的收藏](http://huibin.javaeye.com/blog/favorite) (121)
* [我的书籍](http://huibin.javaeye.com/blog/pdf) (2)
* [我的论坛主题贴](http://huibin.javaeye.com/blog/topic) (0)
* [我的所有论坛贴](http://huibin.javaeye.com/blog/post) (0)
* [我的精华良好贴](http://huibin.javaeye.com/blog/article) (0)
### 最近加入圈子

* [osworkflow](http://osworkflow.group.javaeye.com/)
* [Hibernate](http://hibernate.group.javaeye.com/)
* [struts2](http://struts2.group.javaeye.com/)
* [javascript研究小组](http://hzjavaeyer.group.javaeye.com/)
* [http://jbpm.group.javaeye.com/](http://jbpm.group.javaeye.com/)

### 存档

* [2010-03](http://huibin.javaeye.com/blog/monthblog/2010-03) (34)
* [2010-02](http://huibin.javaeye.com/blog/monthblog/2010-02) (9)
* [2010-01](http://huibin.javaeye.com/blog/monthblog/2010-01) (33)
* [更多存档...](http://huibin.javaeye.com/blog/monthblog_more)
### 最新评论

* [eclipse生成javadoc时出 ...](http://huibin.javaeye.com/blog/458902#comments "eclipse生成javadoc时出错：编码GBK的不可映射字符")
太好了，今天就遇到了这个问题。lz很厉害！不过有下面的问题正在构造 Javad ...
-- by [zyhui98](http://zyhui98.javaeye.com/)

### 评论排行榜

* [eclipse生成javadoc时出错：编码GBK的不 ...](http://huibin.javaeye.com/blog/458902 "eclipse生成javadoc时出错：编码GBK的不可映射字符")
* [mysql limit 使用方法](http://huibin.javaeye.com/blog/569857 "mysql limit 使用方法")
* [日期控件](http://huibin.javaeye.com/blog/435717 "日期控件")
* [Java调用Oracle存储过程](http://huibin.javaeye.com/blog/569871 "Java调用Oracle存储过程")
* [WEB前端工具](http://huibin.javaeye.com/blog/618771 "WEB前端工具")
* [![Rss]()](http://huibin.javaeye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://huibin.javaeye.com/rss)
* [![Rss_xianguo]()](http://www.xianguo.com/subscribe.php?url=http://huibin.javaeye.com/rss)
* [[什么是RSS?]](http://www.google.com/search?hl=zh-CN&q=RSS)
声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2009 JavaEye.com. All rights reserved. 上海炯耐计算机软件有限公司 [ 沪ICP备05023328号 ]
