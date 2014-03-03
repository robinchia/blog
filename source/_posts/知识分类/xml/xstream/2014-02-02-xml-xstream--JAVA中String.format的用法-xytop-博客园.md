---
layout: post
title: "JAVA中String.format的用法 - xytop - 博客园"
categories: xml
tags: 
 - xml
 - xstream
--- 

# JAVA中String.format的用法 - xytop - 博客园

[]()    ![]()

[xytop](http://www.cnblogs.com/xytop/)
随笔- 2  文章- 0  评论- 0 

[博客园](http://www.cnblogs.com/)  [首页](http://www.cnblogs.com/xytop/)  [新随笔](http://www.cnblogs.com/xytop/admin/EditPosts.aspx?opt=1)  [联系](http://space.cnblogs.com/msg/send/xytop)  [管理](http://www.cnblogs.com/xytop/admin/EditPosts.aspx)  [订阅](http://www.cnblogs.com/xytop/rss) [![订阅]()](http://www.cnblogs.com/xytop/rss)
# [JAVA中String.format的用法]()

## JAVA String.format 方法使用介绍

1.对整数进行格式化：%[index$][标识][最小宽度]转换方式
        我们可以看到，格式化字符串由4部分组成，其中%[index$]的含义我们上面已经讲过，[最小宽度]的含义也很好理解，就是最终该整数转化的字符串最少包含多少位数字。我们来看看剩下2个部分的含义吧：
![]()标识： 
![]()'-'    在最小宽度内左对齐，不可以与“用0填充”同时使用
![]()'#'    只适用于8进制和16进制，8进制时在结果前面增加一个0，16进制时在结果前面增加0x
![]()'+'    结果总是包括一个符号（一般情况下只适用于10进制，若对象为BigInteger才可以用于8进制和16进制）
![]()'  '    正值前加空格，负值前加负号（一般情况下只适用于10进制，若对象为BigInteger才可以用于8进制和16进制）
![]()'0'    结果将用零来填充
![]()','    只适用于10进制，每3位数字之间用“，”分隔
![]()'('    若参数是负数，则结果中不添加负号而是用圆括号把数字括起来（同‘+’具有同样的限制）
![]()
![]()转换方式：
![]()d-十进制   o-八进制   x或X-十六进制

        上面的说明过于枯燥，我们来看几个具体的例子。需要特别注意的一点是：大部分标识字符可以同时使用。

![]()        System.out.println(String.format("%1$,09d", -3123));
![]()        System.out.println(String.format("%1$9d", -31));
![]()        System.out.println(String.format("%1$-9d", -31));
![]()        System.out.println(String.format("%1$(9d", -31));
![]()        System.out.println(String.format("%1$#9x", 5689));
![]()
![]()//结果为：
![]()//-0003,123
![]()//      -31
![]()//-31      
![]()//     (31)
![]()//   0x1639

2.对浮点数进行格式化：%[index$][标识][最少宽度][.精度]转换方式
        我们可以看到，浮点数的转换多了一个“精度”选项，可以控制小数点后面的位数。

![]()标识： 
![]()'-'    在最小宽度内左对齐，不可以与“用0填充”同时使用
![]()'+'    结果总是包括一个符号
![]()'  '    正值前加空格，负值前加负号
![]()'0'    结果将用零来填充
![]()','    每3位数字之间用“，”分隔（只适用于fgG的转换）
![]()'('    若参数是负数，则结果中不添加负号而是用圆括号把数字括起来（只适用于eEfgG的转换）
![]()
![]()转换方式：
![]()'e', 'E'  --  结果被格式化为用计算机科学记数法表示的十进制数
![]()'f'          --  结果被格式化为十进制普通表示方式
![]()'g', 'G'    --  根据具体情况，自动选择用普通表示方式还是科学计数法方式
![]()'a', 'A'    --   结果被格式化为带有效位数和指数的十六进制浮点数

3.对字符进行格式化：
        对字符进行格式化是非常简单的，c表示字符，标识中'-'表示左对齐，其他就没什么了。
4.对百分比符号进行格式化：
        看了上面的说明，大家会发现百分比符号“%”是特殊格式的一个前缀。那么我们要输入一个百分比符号该怎么办呢？肯定是需要转义字符的,但是要注意的是，在这里转义字符不是“\”，而是“%”。换句话说，下面这条语句可以输出一个“12%”：
System.out.println(String.format("%1$d%%", 12));
5.取得平台独立的行分隔符：
        System.getProperty("line.separator")可以取得平台独立的行分隔符，但是用在format中间未免显得过于烦琐了。于是format函数自带了一个平台独立的行分隔符那就是String.format("%n")。
6.对日期类型进行格式化：
         以下日期和时间转换的后缀字符是为 't' 和 'T' 转换定义的。这些类型相似于但不完全等同于那些由 GNU date 和 POSIX strftime(3c) 定义的类型。提供其他转换类型是为了访问特定于 Java 的功能（如将 'L' 用作秒中的毫秒）。
以下转换字符用来格式化时间：
'H'     24 小时制的小时，被格式化为必要时带前导零的两位数，即 00 - 23。
'I'     12 小时制的小时，被格式化为必要时带前导零的两位数，即 01 - 12。
'k'     24 小时制的小时，即 0 - 23。
'l'     12 小时制的小时，即 1 - 12。
'M'     小时中的分钟，被格式化为必要时带前导零的两位数，即 00 - 59。
'S'     分钟中的秒，被格式化为必要时带前导零的两位数，即 00 - 60 （"60" 是支持闰秒所需的一个特殊值）。
'L'     秒中的毫秒，被格式化为必要时带前导零的三位数，即 000 - 999。
'N'     秒中的毫微秒，被格式化为必要时带前导零的九位数，即 000000000 - 999999999。
'p'     特定于语言环境的 上午或下午 标记以小写形式表示，例如 "am" 或 "pm"。使用转换前缀 'T' 可以强行将此输出转换为大写形式。
'z'     相对于 GMT 的 RFC 822 格式的数字时区偏移量，例如 -0800。
'Z'     表示时区缩写形式的字符串。Formatter 的语言环境将取代参数的语言环境（如果有）。
's'     自协调世界时 (UTC) 1970 年 1 月 1 日 00:00:00 至现在所经过的秒数，即 Long.MIN_VALUE/1000 与 Long.MAX_VALUE/1000 之间的差值。
'Q'     自协调世界时 (UTC) 1970 年 1 月 1 日 00:00:00 至现在所经过的毫秒数，即 Long.MIN_VALUE 与 Long.MAX_VALUE 之间的差值。
以下转换字符用来格式化日期：
'B'     特定于语言环境的月份全称，例如 "January" 和 "February"。
'b'     特定于语言环境的月份简称，例如 "Jan" 和 "Feb"。
'h'     与 'b' 相同。
'A'     特定于语言环境的星期几全称，例如 "Sunday" 和 "Monday"
'a'     特定于语言环境的星期几简称，例如 "Sun" 和 "Mon"
'C'     除以 100 的四位数表示的年份，被格式化为必要时带前导零的两位数，即 00 - 99
'Y'     年份，被格式化为必要时带前导零的四位数（至少），例如，0092 等于格里高利历的 92 CE。
'y'     年份的最后两位数，被格式化为必要时带前导零的两位数，即 00 - 99。
'j'     一年中的天数，被格式化为必要时带前导零的三位数，例如，对于格里高利历是 001 - 366。
'm'     月份，被格式化为必要时带前导零的两位数，即 01 - 13。
'd'     一个月中的天数，被格式化为必要时带前导零两位数，即 01 - 31
'e'     一个月中的天数，被格式化为两位数，即 1 - 31。
以下转换字符用于格式化常见的日期/时间组合。
'R'     24 小时制的时间，被格式化为 "%tH:%tM"
'T'     24 小时制的时间，被格式化为 "%tH:%tM:%tS"。
'r'     12 小时制的时间，被格式化为 "%tI:%tM:%tS %Tp"。上午或下午标记 ('%Tp') 的位置可能与语言环境有关。
'D'     日期，被格式化为 "%tm/%td/%ty"。
'F'     ISO 8601 格式的完整日期，被格式化为 "%tY-%tm-%td"。
'c'     日期和时间，被格式化为 "%ta %tb %td %tT %tZ %tY"，例如 "Sun Jul 20 16:17:00 EDT 1969"。

分类: [JAVA](http://www.cnblogs.com/xytop/category/152381.html)

绿色通道：[好文要顶]()[关注我]()[收藏该文]()[与我联系](http://space.cnblogs.com/msg/send/xytop)
[xytop](http://home.cnblogs.com/u/xytop/)
[关注 - 3](http://home.cnblogs.com/u/xytop/followees)
[粉丝 - 0](http://home.cnblogs.com/u/xytop/followers)

[+加关注]()

0

0
(请您对文章做出评价)

[»](http://www.cnblogs.com/xytop/archive/2010/09/16/1827865.html) 下一篇：[IIS无法启动](http://www.cnblogs.com/xytop/archive/2010/09/16/1827865.html "发布于2010-09-16 10:30")

posted @ 2008-08-26 23:57 [xytop](http://www.cnblogs.com/xytop/) 阅读(1680) [评论(0)](http://www.cnblogs.com/xytop/archive/2008/08/26/1277125.html#commentform) [编辑](http://www.cnblogs.com/xytop/archive/2008/08/26/1277125.html#) [收藏](http://www.cnblogs.com/xytop/archive/2008/08/26/1277125.html#)
 ![]()

注册用户登录后才能发表评论，请 [登录](http://passport.cnblogs.com/login.aspx?ReturnUrl=http%3a%2f%2fwww.cnblogs.com%2fxytop%2farchive%2f2008%2f08%2f26%2f1277125.html%3flogin%3d1%23commentform) 或 [注册](http://passport.cnblogs.com/register.aspx?ReturnUrl=http%3a%2f%2fwww.cnblogs.com%2fxytop%2farchive%2f2008%2f08%2f26%2f1277125.html%23Bottom2)，[返回博客园首页](http://www.cnblogs.com/)。

**最新IT新闻**:
· [MSN 中文网大改版，附介绍](http://news.cnblogs.com/n/103808/)
· [英特尔宣布Ultrabook](http://news.cnblogs.com/n/103807/)
· [Win8集成Xbox 360传言再起 或采用会员制](http://news.cnblogs.com/n/103806/)
· [京东自种大米上市销售 刘强东称绝对无毒](http://news.cnblogs.com/n/103805/)
· [2011上半年电商务10大事件盘点](http://news.cnblogs.com/n/103804/)
» [更多新闻...](http://news.cnblogs.com/ "IT新闻")
**知识库最新文章**:
[从一个职校走出来的高级程序员](http://kb.cnblogs.com/page/100785/)
[MySQL与NoSQL - SQL与NoSQL的融合](http://kb.cnblogs.com/page/100521/)
[分布式系统部署、监控与进程管理的几重境界](http://kb.cnblogs.com/page/100594/)
[谁做了程序员眼中的程序员](http://kb.cnblogs.com/page/100991/)
[复杂是大敌](http://kb.cnblogs.com/page/100468/)
 » [更多知识库文章...](http://news.cnblogs.com/)

网站导航: [博客园首页](http://www.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [我的园子](http://home.cnblogs.com/)  [闪存](http://home.cnblogs.com/ing/)  [程序员招聘](http://job.cnblogs.com/)  [博问](http://home.cnblogs.com/q/)
[![]()](http://www.china-pub.com/STATIC07/1104/jsj_bangding_110421.asp?ref=bky)
[China-pub 计算机图书网上专卖店！6.5万品种2-8折！](http://www.china-pub.com/itbook/)
[China-Pub 计算机绝版图书按需印刷服务](http://www.china-pub.com/static07/0901/zh_jueba_090121.asp)

简洁版式：[JAVA中String.format的用法](http://archive.cnblogs.com/a/1277125/)

Copyright ©2011 xytop

昵称：[xytop](http://www.cnblogs.com/xytop/)
粉丝：[0](http://home.cnblogs.com/u/xytop/followers/)
关注：[3](http://home.cnblogs.com/u/xytop/followees/)
[+加关注]()

[<]( "Go to the previous month")2008年8月[>]( "Go to the next month")日一二三四五六272829303112345678910111213141516171819202122232425[26](http://www.cnblogs.com/xytop/archive/2008/8/26.html)2728293031123456
### 搜索

 

 

### 常用链接

* [我的随笔](http://www.cnblogs.com/xytop/MyPosts.html)
* [我的空间](http://home.cnblogs.com/xytop/)
* [我的短信](http://space.cnblogs.com/msg/recent)
* [我的评论](http://www.cnblogs.com/xytop/MyComments.html)
* [更多链接](http://www.cnblogs.com/xytop/archive/2008/08/26/1277125.html#)
* [我的参与](http://www.cnblogs.com/xytop/OtherPosts.html "我发表过评论的随笔")
* [我的新闻](http://www.cnblogs.com/xytop/MyNews.html)
* [我的标签](http://www.cnblogs.com/xytop/tag/)
### 我的标签

* [iis无法启动](http://www.cnblogs.com/xytop/tag/iis%E6%97%A0%E6%B3%95%E5%90%AF%E5%8A%A8/)(1)
* [IIS启动报错](http://www.cnblogs.com/xytop/tag/IIS%E5%90%AF%E5%8A%A8%E6%8A%A5%E9%94%99/)(1)

# 随笔分类

* [C#(1)](http://www.cnblogs.com/xytop/category/152382.html)[![]()](http://www.cnblogs.com/xytop/archive/2008/08/26/1277125.html#)
* [JAVA(1)](http://www.cnblogs.com/xytop/category/152381.html)[![]()](http://www.cnblogs.com/xytop/archive/2008/08/26/1277125.html#)
* [JSP](http://www.cnblogs.com/xytop/category/152383.html)[![]()](http://www.cnblogs.com/xytop/archive/2008/08/26/1277125.html#)

# 随笔档案

* [2010年9月 (1)](http://www.cnblogs.com/xytop/archive/2010/09.html)
* [2008年8月 (1)](http://www.cnblogs.com/xytop/archive/2008/08.html)
### 最新评论[![]( "RSS订阅最最新评论")](http://www.cnblogs.com/xytop/CommentsRSS.aspx "RSS订阅最最新评论")

### 阅读排行榜

* [1. JAVA中String.format的用法(1680)]()
* [2. IIS无法启动(64)](http://www.cnblogs.com/xytop/archive/2010/09/16/1827865.html)
### 评论排行榜

* [1. IIS无法启动(0)](http://www.cnblogs.com/xytop/archive/2010/09/16/1827865.html)
* [2. JAVA中String.format的用法(0)]()
