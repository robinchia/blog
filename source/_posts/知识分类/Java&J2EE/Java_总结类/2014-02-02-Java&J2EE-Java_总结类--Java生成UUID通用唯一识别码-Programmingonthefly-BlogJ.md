---
layout: post
title: "Java生成UUID通用唯一识别码 - Programming on the fly - BlogJ"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# Java生成UUID通用唯一识别码 - Programming on the fly - BlogJava

[Programming on the fly](http://www.blogjava.net/Werther/)
Live as if you were to die tomorrow. Learn as if you were to live forever.

[BlogJava](http://www.blogjava.net/)   [首页](http://www.blogjava.net/Werther/)   [新随笔](http://www.blogjava.net/Werther/admin/EditPosts.aspx?opt=1) [联系](http://www.blogjava.net/Werther/contact.aspx?id=1)   [聚合](http://www.blogjava.net/Werther/rss)[![]()](http://www.blogjava.net/Werther/rss)   [管理](http://www.blogjava.net/Werther/admin/EditPosts.aspx)

随笔-170  评论-150  文章-11  trackbacks-0
[Java生成UUID通用唯一识别码](http://www.blogjava.net/Werther/archive/2009/12/14/305925.html)UUID含义是通用唯一识别码 (Universally Unique Identifier)，这 是一个软件建构的标准，也是被开源软件基金会 (Open Software Foundation, OSF) 的组织在分布式计算环境 (Distributed Computing Environment, DCE) 领域的一部份。UUID 的目的，是让分布式系统中的所有元素，都能有唯一的辨识资讯，而不需要透过中央控制端来做辨识资讯的指定。如此一来，每个人都可以建立不与其它人冲突的 UUID。在这样的情况下，就不需考虑数据库建立时的名称重复问题。目前最广泛应用的 UUID，即是微软的 Microsoft's Globally Unique Identifiers (GUIDs)，而其他重要的应用，则有 Linux ext2/ext3 档案系统、LUKS 加密分割区、GNOME、KDE、Mac OS X 等等。

以下是具体生成UUID的例子：

view plaincopy to clipboardprint?

package test;

import java.util.UUID;

public class UUIDGenerator {

public UUIDGenerator() {

}

public static String getUUID() {

UUID uuid = UUID.randomUUID();

String str = uuid.toString();

// 去掉"-"符号

String temp = str.substring(0, + str.substring(9, 13) + str.substring(14, 18) + str.substring(19, 23) + str.substring(24);

return str+","+temp;

}

//获得指定数量的UUID

public static String[] getUUID(int number) {

if (number < 1) {

return null;

}

String[] ss = new String[number];

for (int i = 0; i < number; i++) {

ss[i] = getUUID();

}

return ss;

}

public static void main(String[] args) {

String[] ss = getUUID(10);

for (int i = 0; i < ss.length; i++) {

System.out.println("ss["+i+"]====="+ss[i]);

}

}

}

package test;

import java.util.UUID;

public class UUIDGenerator {

public UUIDGenerator() {

}

public static String getUUID() {

UUID uuid = UUID.randomUUID();

String str = uuid.toString();

// 去掉"-"符号

String temp = str.substring(0, + str.substring(9, 13) + str.substring(14, 18) + str.substring(19, 23) + str.substring(24);

return str+","+temp;

}
//获得指定数量的UUID

public static String[] getUUID(int number) {

if (number < 1) {

return null;

}

String[] ss = new String[number];

for (int i = 0; i < number; i++) {

ss[i] = getUUID();

}

return ss;

}

public static void main(String[] args) {

String[] ss = getUUID(10);

for (int i = 0; i < ss.length; i++) {

System.out.println("ss["+i+"]====="+ss[i]);

}

}

}

结果：

view plaincopy to clipboardprint?

ss[0]=====4cdbc040-657a-4847-b266-7e31d9e2c3d9,4cdbc040657a4847b2667e31d9e2c3d9

ss[1]=====72297c88-4260-4c05-9b05-d28bfb11d10b,72297c8842604c059b05d28bfb11d10b

ss[2]=====6d513b6a-69bd-4f79-b94c-d65fc841ea95,6d513b6a69bd4f79b94cd65fc841ea95

ss[3]=====d897a7d3-87a3-4e38-9e0b-71013a6dbe4c,d897a7d387a34e389e0b71013a6dbe4c

ss[4]=====5709f0ba-31e3-42bd-a28d-03485b257c94,5709f0ba31e342bda28d03485b257c94

ss[5]=====530fbb8c-eec9-48d1-ae1b-5f792daf09f3,530fbb8ceec948d1ae1b5f792daf09f3

ss[6]=====4bf07297-65b2-45ca-b905-6fc6f2f39158,4bf0729765b245cab9056fc6f2f39158

ss[7]=====6e5a0e85-b4a0-485f-be54-a758115317e1,6e5a0e85b4a0485fbe54a758115317e1

ss[8]=====245accec-3c12-4642-967f-e476cef558c4,245accec3c124642967fe476cef558c4

ss[9]=====ddd4b5a9-fecd-446c-bd78-63b70bb500a1,ddd4b5a9fecd446cbd7863b70bb500a1

ss[0]=====4cdbc040-657a-4847-b266-7e31d9e2c3d9,4cdbc040657a4847b2667e31d9e2c3d9

ss[1]=====72297c88-4260-4c05-9b05-d28bfb11d10b,72297c8842604c059b05d28bfb11d10b

ss[2]=====6d513b6a-69bd-4f79-b94c-d65fc841ea95,6d513b6a69bd4f79b94cd65fc841ea95

ss[3]=====d897a7d3-87a3-4e38-9e0b-71013a6dbe4c,d897a7d387a34e389e0b71013a6dbe4c

ss[4]=====5709f0ba-31e3-42bd-a28d-03485b257c94,5709f0ba31e342bda28d03485b257c94

ss[5]=====530fbb8c-eec9-48d1-ae1b-5f792daf09f3,530fbb8ceec948d1ae1b5f792daf09f3

ss[6]=====4bf07297-65b2-45ca-b905-6fc6f2f39158,4bf0729765b245cab9056fc6f2f39158

ss[7]=====6e5a0e85-b4a0-485f-be54-a758115317e1,6e5a0e85b4a0485fbe54a758115317e1

ss[8]=====245accec-3c12-4642-967f-e476cef558c4,245accec3c124642967fe476cef558c4

ss[9]=====ddd4b5a9-fecd-446c-bd78-63b70bb500a1,ddd4b5a9fecd446cbd7863b70bb500a1

可以看出，UUID 是指在一台机器上生成的数字，它保证对在同一时空中的所有机器都是唯一的。通常平台会提供生成的API。按照开放软件基金会(OSF)制定的标准计算，用到了以太网卡地址、纳秒级时间、芯片ID码和许多可能的数字

UUID由以下几部分的组合：

（1）当前日期和时间，UUID的第一个部分与时间有关，如果你在生成一个UUID之后，过几秒又生成一个UUID，则第一个部分不同，其余相同。

（2）时钟序列

（3）全局唯一的IEEE机器识别号，如果有网卡，从网卡MAC地址获得，没有网卡以其他方式获得。

UUID的唯一缺陷在于生成的结果串会比较长。关于UUID这个标准使用最普遍的是微软的GUID(Globals Unique Identifiers)。在ColdFusion中可以用CreateUUID()函数很简单的生成UUID，其格式为：xxxxxxxx-xxxx- xxxx-xxxxxxxxxxxxxxxx(8-4-4-16)，其中每个 x 是 0-9 或 a-f 范围内的一个十六进制的数字。而标准的UUID格式为：xxxxxxxx-xxxx-xxxx-xxxxxx-xxxxxxxxxx (8-4-4-4-12)，可以从cflib [下载](http://download.chinaitlab.com/)CreateGUID() UDF进行转换。

使用UUID的好处在分布式的软件系统中（比如：DCE/RPC, COM+,CORBA）就能体现出来，它能保证每个节点所生成的标识都不会重复，并且随着WEB服务等整合技术的发展，UUID的优势将更加明显。根据使用的特定机制，UUID不仅需要保证是彼此不相同的，或者最少也是与公元3400年之前其他任何生成的通用惟一标识符有非常大的区别。

通用惟一标识符还可以用来指向大多数的可能的物体。微软和其他一些软件公司都倾向使用全球惟一标识符（GUID），这也是通用惟一标识符的一种类型，可用来指向组建对象模块对象和其他的软件组件。第一个通用惟一标识符是在网罗计算机系统（NCS）中创建，并且随后成为开放软件基金会（OSF）的分布式计算环境（DCE）的组件。
posted on 2009-12-14 17:19 [Werther](http://www.blogjava.net/Werther/) 阅读(466) [评论(1)](http://www.blogjava.net/Werther/archive/2009/12/14/305925.html#Post)  [编辑](http://www.blogjava.net/Werther/admin/EditPosts.aspx?postid=305925)  [收藏](http://www.blogjava.net/Werther/AddToFavorite.aspx?id=305925) 所属分类: [10.Java](http://www.blogjava.net/Werther/category/38034.html)![]()

[]()
**评论:**

[#](http://www.blogjava.net/Werther/archive/2009/12/14/305925.html#305943 "permalink: re: Java生成UUID通用唯一识别码 ") []()re: Java生成UUID通用唯一识别码 []()2009-12-14 19:58 | [天独](http://www.blogjava.net/kick191/)
在去掉“-”可以用replaceAll这个方法  [回复](http://www.blogjava.net/Werther/archive/2009/12/14/305925.html#post)  [更多评论](http://www.blogjava.net/comment?author=%e5%a4%a9%e7%8b%ac "查看该作者发表过的评论")
[]()  []()
[IT新闻](http://news.cnblogs.com/)  [新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [Chrome OS专题](http://kb.cnblogs.com/zt/chrome/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/Werther/archive/2009/12/14/305925.html&SourceURL=/Werther/archive/2009/12/14/305925.html)       [使用Ctrl+Enter键可以直接提交] [Windows 7专题](http://kb.cnblogs.com/zt/windows7/)  IT新闻：
· [微软将推云计算迁移工具](http://news.cnblogs.com/n/53055/)
· [广电总局明年加大查处力度 VeryCD有望逃过一劫](http://news.cnblogs.com/n/53054/)
· [史玉柱携巨人重返珠海](http://news.cnblogs.com/n/53053/)
· [谷歌2009年15件大事 先后2次裁员](http://news.cnblogs.com/n/53052/)
· [谷歌和Facebook推出短域名goo.gl和Fb.me](http://news.cnblogs.com/n/53051/)

博客园首页随笔：
· [汉诺塔(Hanoi) C#解法](http://www.cnblogs.com/gsrdell/archive/2009/12/15/1624437.html)
· [计算机中的颜色VI——从色相值到纯色的快速计算](http://www.cnblogs.com/grenet/archive/2009/12/15/1624243.html)
· [反编译Silverlight项目](http://www.cnblogs.com/jv9/archive/2009/12/15/1622853.html)
· [程序之外的事情 (Part 1 - Speech)](http://www.cnblogs.com/cathsfz/archive/2009/12/15/1624229.html)
· [Visual Studio 2010 Ultimate敏捷测试驱动开发](http://www.cnblogs.com/xiaoyin_net/archive/2009/12/15/1624220.html)
招聘信息：
· [.NET 网站程序开发工程师(龙宽网络信息技术（北京）有限公司)](http://job.cnblogs.com/offer/3308/)
· [技术部经理(河南互惠信息咨询有限责任公司)](http://job.cnblogs.com/offer/2823/)
· [.NET(C#)程序员(北京赛优科技有限公司)](http://job.cnblogs.com/offer/3775/)
· [.net平台研发工程师(北京易车互联信息技术有限公司)](http://job.cnblogs.com/offer/2429/)
· [C++开发工程师(空中网)](http://job.cnblogs.com/offer/3334/) 在知识库中查看：
[Java生成UUID通用唯一识别码](http://kb.cnblogs.com/b/305925/) 网站导航:

[博客园](http://www.cnblogs.com/)   [IT新闻](http://news.cnblogs.com/)   [个人主页](http://home.cnblogs.com/)   [博客生活](http://www.cnweblog.com/)   [IT博客网](http://www.cnitblog.com/)   [C++博客](http://www.cppblog.com/)   [博客园社区](http://space.cnblogs.com/) [管理](http://www.blogjava.net/Werther/archive/2009/12/14/305925.html?opt=admin) 相关文章:

* [Java生成UUID通用唯一识别码](http://www.blogjava.net/Werther/archive/2009/12/14/305925.html)
* [instanceof 运算符的用法](http://www.blogjava.net/Werther/archive/2009/11/25/303561.html)
* [不同方式遍历Map集合](http://www.blogjava.net/Werther/archive/2009/11/23/303281.html)
* [页面显示的处理!](http://www.blogjava.net/Werther/archive/2009/11/19/302891.html)
* [logic:present 和 logic:empty标签](http://www.blogjava.net/Werther/archive/2009/11/10/301808.html)
* [分治算法](http://www.blogjava.net/Werther/archive/2009/11/06/301416.html)
* [Java正则表达式应用总结](http://www.blogjava.net/Werther/archive/2009/10/17/298665.html)
* [Post和Get的区别](http://www.blogjava.net/Werther/archive/2009/08/06/290155.html)
* [迭代器模式(Iterator pattern)](http://www.blogjava.net/Werther/archive/2009/08/05/289995.html)
* [利用Ant和XDoclet自动产生映射文件例子](http://www.blogjava.net/Werther/archive/2009/08/04/289872.html)  

# I'm reading...

Java 60![]()
Head  First SQL![]()
Apache Tomcat5![]()
If you need these books,pls send me emails.
Email:kunpeng.niu@163.com [<]( "Go to the previous month") 2009年12月 [>]( "Go to the next month") 日 一 二 三 四 五 六 29 30 1 2 3 4 5 6 7 8 9 10 11 12 13 [14](http://www.blogjava.net/Werther/archive/2009/12/14.html) 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 1 2 3 4 5 6 7 8 9

### 留言簿(6)

* [给我留言](http://www.blogjava.net/Werther/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/Werther/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/Werther/admin/MyMessages.aspx)

# 随笔分类(154)

* [10.Java(67)](http://www.blogjava.net/Werther/category/38034.html)[![]( "Subscribe to 10.Java(67)")](http://www.blogjava.net/Werther/category/38034.html/rss "Subscribe to 10.Java(67)")
* [11.JavaScript(2)](http://www.blogjava.net/Werther/category/38696.html)[![]( "Subscribe to 11.JavaScript(2)")](http://www.blogjava.net/Werther/category/38696.html/rss "Subscribe to 11.JavaScript(2)")
* [12.English(3)](http://www.blogjava.net/Werther/category/38199.html)[![]( "Subscribe to 12.English(3)")](http://www.blogjava.net/Werther/category/38199.html/rss "Subscribe to 12.English(3)")
* [13.AJAX(2)](http://www.blogjava.net/Werther/category/39061.html)[![]( "Subscribe to 13.AJAX(2)")](http://www.blogjava.net/Werther/category/39061.html/rss "Subscribe to 13.AJAX(2)")
* [14.Oracle(2)](http://www.blogjava.net/Werther/category/39062.html)[![]( "Subscribe to 14.Oracle(2)")](http://www.blogjava.net/Werther/category/39062.html/rss "Subscribe to 14.Oracle(2)")
* [15.SQL Server(21)](http://www.blogjava.net/Werther/category/37767.html)[![]( "Subscribe to 15.SQL Server(21)")](http://www.blogjava.net/Werther/category/37767.html/rss "Subscribe to 15.SQL Server(21)")
* [16.MySQL(5)](http://www.blogjava.net/Werther/category/39063.html)[![]( "Subscribe to 16.MySQL(5)")](http://www.blogjava.net/Werther/category/39063.html/rss "Subscribe to 16.MySQL(5)")
* [17.Windows(4)](http://www.blogjava.net/Werther/category/38035.html)[![]( "Subscribe to 17.Windows(4)")](http://www.blogjava.net/Werther/category/38035.html/rss "Subscribe to 17.Windows(4)")
* [18.Other(32)](http://www.blogjava.net/Werther/category/37791.html)[![]( "Subscribe to 18.Other(32)")](http://www.blogjava.net/Werther/category/37791.html/rss "Subscribe to 18.Other(32)")
* [19.Eclipse(1)](http://www.blogjava.net/Werther/category/39064.html)[![]( "Subscribe to 19.Eclipse(1)")](http://www.blogjava.net/Werther/category/39064.html/rss "Subscribe to 19.Eclipse(1)")
* [20.Struts(5)](http://www.blogjava.net/Werther/category/39065.html)[![]( "Subscribe to 20.Struts(5)")](http://www.blogjava.net/Werther/category/39065.html/rss "Subscribe to 20.Struts(5)")
* [21.Hibernate(4)](http://www.blogjava.net/Werther/category/39066.html)[![]( "Subscribe to 21.Hibernate(4)")](http://www.blogjava.net/Werther/category/39066.html/rss "Subscribe to 21.Hibernate(4)")
* [22.Spring(4)](http://www.blogjava.net/Werther/category/39067.html)[![]( "Subscribe to 22.Spring(4)")](http://www.blogjava.net/Werther/category/39067.html/rss "Subscribe to 22.Spring(4)")
* [23.Tomcat](http://www.blogjava.net/Werther/category/39068.html)[![]( "Subscribe to 23.Tomcat")](http://www.blogjava.net/Werther/category/39068.html/rss "Subscribe to 23.Tomcat")
* [24.Weblogic](http://www.blogjava.net/Werther/category/39069.html)[![]( "Subscribe to 24.Weblogic")](http://www.blogjava.net/Werther/category/39069.html/rss "Subscribe to 24.Weblogic")
* [25.UML(2)](http://www.blogjava.net/Werther/category/40732.html)[![]( "Subscribe to 25.UML(2)")](http://www.blogjava.net/Werther/category/40732.html/rss "Subscribe to 25.UML(2)")

# 随笔档案(179)

* [2009年12月 (1)](http://www.blogjava.net/Werther/archive/2009/12.html)
* [2009年11月 (8)](http://www.blogjava.net/Werther/archive/2009/11.html)
* [2009年10月 (4)](http://www.blogjava.net/Werther/archive/2009/10.html)
* [2009年9月 (1)](http://www.blogjava.net/Werther/archive/2009/09.html)
* [2009年8月 (9)](http://www.blogjava.net/Werther/archive/2009/08.html)
* [2009年7月 (14)](http://www.blogjava.net/Werther/archive/2009/07.html)
* [2009年6月 (20)](http://www.blogjava.net/Werther/archive/2009/06.html)
* [2009年5月 (26)](http://www.blogjava.net/Werther/archive/2009/05.html)
* [2009年4月 (41)](http://www.blogjava.net/Werther/archive/2009/04.html)
* [2009年3月 (40)](http://www.blogjava.net/Werther/archive/2009/03.html)
* [2009年2月 (15)](http://www.blogjava.net/Werther/archive/2009/02.html)

# 文章档案(1)

* [2009年11月 (1)](http://www.blogjava.net/Werther/archives/2009/11.html)

# 新闻档案(5)

* [2009年9月 (2)](http://www.blogjava.net/Werther/news/2009/09.html)
* [2009年6月 (2)](http://www.blogjava.net/Werther/news/2009/06.html)
* [2009年4月 (1)](http://www.blogjava.net/Werther/news/2009/04.html)

# 相册

* [My Books](http://www.blogjava.net/Werther/gallery/39963.html)

# 1.Java Official Website

* [30.Sun Official Website](http://java.sun.com/)
* [31.Sun China](http://cn.sun.com/)
* [32.Java SE API 6](http://java.sun.com/javase/6/docs/api/index.html)
* [33.Eclipse Offcial Website](http://www.eclipse.org/)
* [34.Apache](http://www.apache.org/)

# 2.Java Study Website

* [40.CSDN](http://www.csdn.com/)
* [41.Java2s](http://www.java2s.com/)
* [42.ChinaItLab](http://www.chinaitlab.com/)
* [43.Hono Web](http://www.honoweb.com/java.php)
* [44.MATRIX CHINA](http://www.matrix.org.cn/main.shtml)
* [45.JAVA 2000](http://www.java2000.net/)
* [46.Java Source Code Examlpe](http://www.javadb.com/)
* [47.Java乐园](http://www.javaly.cn/)
* [48.xdoclet documentation](http://xdoclet.sourceforge.net/olddocs/)
* [49.open-source](http://www.open-open.com/)

# 3.Java Technic Website

* [50.J2me develop](http://www.j2medev.com/Index.html)
* [51.J2MEChina](http://www.j2me.com.cn/bbs/)
* [52.JavaEye](http://www.javaeye.com/)

# 4.Java Video Website

* [60.尚学堂](http://www.verycd.com/topics/93279/)
* [61.浪曦視頻](http://www.verycd.com/topics/249195/)

# 5.Database Website

* [70.TT Database](http://www.searchdatabase.com.cn/index.htm)
* [71.MySQL](http://www.mysql.com/)
* [72.MySql Help](http://dev.mysql.com/doc/refman/5.1/zh/index.html)
* [73.My SQL-TCR](http://www.mysql-tcr.com/)

# 6.Bookshop Website

* [80.卓越](http://www.amazon.cn/?source=2009hao123famousdaohang)
* [81.當當網](http://www.dangdang.com.cn/)
* [82.ITBOOK](http://www.itbook.com.cn/)
* [83.第二書店](http://www.dearbook.com.cn/)
* [84.中國互動出版網](http://www.china-pub.com/)
* [85.IT坊](http://www.ithov.com/)

# 7.English Website

* [90.KeKe English](http://www.kekenet.com/)
* [91.HengXing English](http://www.hxen.com/)
* [92.Dream English](http://www.xaqing.cn/Index.html)
* [93.WangWang English](http://www.wwenglish.com/)
* [94.Ebigear](http://www.ebigear.com/)

# 8.Friends Link

* [100.Robin's Java World](http://www.blogjava.net/fastzch/)
* [101.隔叶黄莺 The Blog of Unmi](http://www.blogjava.net/Unmi/)
* [102.熔岩](http://lavasoft.blog.51cto.com/)
* [103.李先靜](http://blog.csdn.net/absurd)[![]( "Subscribe to 103.李先靜")](http://blog.csdn.net/absurd "Subscribe to 103.李先靜")
* [104.Leo](http://blog.csdn.net/jobchanceleo)
* [105.闵祖平](http://tonyandjava.s155.eatj.com/)
* [106.Fish](http://www.cnblogs.com/wayne-ivan/)
* [107..Net世界](http://www.itstrike.cn/)
* [107.陈皓](http://hi.csdn.net/haoel)
* [108.VFP 十三豆](http://blog.csdn.net/apple_8180)

# 9.Other Web

* [120.天际网](http://ceping.tianji.com/eval01)
* [121.中國網管聯盟](http://bitscn.com/)
* [122.查查吧](http://www.chachaba.com/)

### 积分与排名

* 积分 - 80344
* 排名 - 172

### 最新评论 [![]()](http://www.blogjava.net/Werther/CommentsRSS.aspx)

* [1. re: Java生成UUID通用唯一识别码](http://www.blogjava.net/Werther/archive/2009/12/14/305925.html#305943)
* 在去掉“-”可以用replaceAll这个方法
* --天独
* [2. re: 不同方式遍历Map集合](http://www.blogjava.net/Werther/archive/2009/12/03/303281.html#304589)
* 多學點！
* --征服者
* [3. re: Spring多数据源解决方案 [未登录]](http://www.blogjava.net/Werther/archive/2009/12/01/288643.html#304368)
* 如果各个dataSource上的数据库结构不一样的话，这个方法是不能解决问题的
* --懒猫
* [4. re: 页面显示的处理!](http://www.blogjava.net/Werther/archive/2009/11/19/302891.html#302955)
* @月亮的太阳
在开发过程中,对一些数据的处理,尽量在后台处理.每个人有每个人的习惯.
* --Werther
* [5. re: 页面显示的处理!](http://www.blogjava.net/Werther/archive/2009/11/19/302891.html#302956)
* 页面只是为了显示数据.
* --Werther

### 阅读排行榜

* [1. Java正则表达式的解释说明 (12131)](http://www.blogjava.net/Werther/archive/2009/06/10/281198.html)
* [2. 实战体会Java多线程编程精要 (3009)](http://www.blogjava.net/Werther/archive/2009/07/21/287656.html)
* [3. Spring多数据源解决方案 (2306)](http://www.blogjava.net/Werther/archive/2009/07/27/288643.html)
* [4. 一个最简单的Socket通信例子 (1780)](http://www.blogjava.net/Werther/archive/2009/05/05/268905.html)
* [5. 操作java数组的常用工具类(1630)](http://www.blogjava.net/Werther/archive/2009/04/06/264145.html)

### 评论排行榜

* [1. hibernate的11大优势 (14)](http://www.blogjava.net/Werther/archive/2009/06/18/283091.html)
* [2. 区分Action, Service 和 Dao功能 (8)](http://www.blogjava.net/Werther/archive/2009/05/14/270676.html)
* [3. 让MyEclipse也具有强大的提示功能 (8)](http://www.blogjava.net/Werther/archive/2009/04/21/266819.html)
* [4. Spring事务配置的五种方式 (6)](http://www.blogjava.net/Werther/archive/2009/04/20/266584.html)
* [5. JSF与Struts的比较(6)](http://www.blogjava.net/Werther/archive/2009/04/28/267821.html)
Powered by: [博客园](http://www.cnblogs.com/) 模板提供：[沪江博客](http://blog.hjenglish.com/) Copyright ©2009 Werther
