---
layout: post
title: "java实现double的精确计算和四舍五入"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# java实现double的精确计算和四舍五入

# [遗忘的角落](http://blog.csdn.net/a9529lty)

##

* [登录](http://passport.csdn.net/UserLogin.aspx)
* [注册](http://passport.csdn.net/CSDNUserRegister.aspx)
* [欢迎](http://hi.csdn.net/)
* [退出](http://writeblog.csdn.net/Signout.aspx)
* [我的博客](http://blog.csdn.net/)
* [配置](http://writeblog.csdn.net/configure.aspx)
* [写文章](http://writeblog.csdn.net/PostEdit.aspx)
* [文章管理](http://writeblog.csdn.net/PostList.aspx)
* [博客首页](http://blog.csdn.net/)

*
* 全站 当前博客
*

* [空间](http://hi.csdn.net/a9529lty)
* [博客](http://blog.csdn.net/a9529lty)
* [好友](http://hi.csdn.net/!s/friend/list/a9529lty)
* [相册](http://hi.csdn.net/!s/album/list/a9529lty)
* [留言](http://hi.csdn.net/!s/wall/to/a9529lty)

用户操作[[留言]](http://hi.csdn.net/!s/wall/to/a9529lty)  [[发消息]](http://hi.csdn.net/!s/msg/to/a9529lty)  [[加为好友]](http://hi.csdn.net/!s/friend/add/a9529lty) 订阅我的博客[![XML聚合]()](http://feeds.feedsky.com/csdn.net/a9529lty)   [![FeedSky]()](http://feeds.feedsky.com/csdn.net/a9529lty)[![订阅到鲜果]()](http://www.xianguo.com/subscribe.php?url=http://feeds.feedsky.com/csdn.net/a9529lty)[![订阅到Google]()](http://fusion.google.com/add?feedurl=http://feeds.feedsky.com/csdn.net/a9529lty)[![订阅到抓虾]()](http://www.zhuaxia.com/add_channel.php?url=http://feeds.feedsky.com/csdn.net/a9529lty)[[编辑]](http://writeblog.csdn.net/configure.aspx)a9529lty的公告我的QQ是297453997欢迎大家加我一起讨论问题[[编辑]](http://writeblog.csdn.net/EditCategories.aspx?catID=1)文章分类* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/354401.aspx/rss)[Ajax](http://blog.csdn.net/a9529lty/category/354401.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/352346.aspx/rss)[C#](http://blog.csdn.net/a9529lty/category/352346.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/526658.aspx/rss)[Eclipse](http://blog.csdn.net/a9529lty/category/526658.aspx "Eclipse")
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/354338.aspx/rss)[Hibernate](http://blog.csdn.net/a9529lty/category/354338.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/352340.aspx/rss)[Java](http://blog.csdn.net/a9529lty/category/352340.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/352342.aspx/rss)[Javascript](http://blog.csdn.net/a9529lty/category/352342.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/356239.aspx/rss)[Jsf](http://blog.csdn.net/a9529lty/category/356239.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/356238.aspx/rss)[Jsp](http://blog.csdn.net/a9529lty/category/356238.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/600868.aspx/rss)[Lua](http://blog.csdn.net/a9529lty/category/600868.aspx "Lua ")
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/431500.aspx/rss)[Mysql](http://blog.csdn.net/a9529lty/category/431500.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/352341.aspx/rss)[Netbeans](http://blog.csdn.net/a9529lty/category/352341.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/353973.aspx/rss)[Oracle](http://blog.csdn.net/a9529lty/category/353973.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/403584.aspx/rss)[Sql](http://blog.csdn.net/a9529lty/category/403584.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/430229.aspx/rss)[Struts](http://blog.csdn.net/a9529lty/category/430229.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/354337.aspx/rss)[Tomcat](http://blog.csdn.net/a9529lty/category/354337.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/593722.aspx/rss)[基础知识(面试专用)](http://blog.csdn.net/a9529lty/category/593722.aspx "面试专用")
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/449114.aspx/rss)[日语](http://blog.csdn.net/a9529lty/category/449114.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/507814.aspx/rss)[算法](http://blog.csdn.net/a9529lty/category/507814.aspx)
* [![(RSS)]()](http://blog.csdn.net/a9529lty/category/352351.aspx/rss)[正则表达式](http://blog.csdn.net/a9529lty/category/352351.aspx)[[编辑]](http://writeblog.csdn.net/ArticleList.aspx)收藏[正则表达式](http://blog.csdn.net/a9529lty/category/352350.aspx)存档* [2009年10月(7)](http://blog.csdn.net/a9529lty/archive/2009/10.aspx)
* [2009年09月(10)](http://blog.csdn.net/a9529lty/archive/2009/09.aspx)
* [2009年07月(4)](http://blog.csdn.net/a9529lty/archive/2009/07.aspx)
* [2009年06月(6)](http://blog.csdn.net/a9529lty/archive/2009/06.aspx)
* [2009年05月(9)](http://blog.csdn.net/a9529lty/archive/2009/05.aspx)
* [2009年04月(16)](http://blog.csdn.net/a9529lty/archive/2009/04.aspx)
* [2009年03月(9)](http://blog.csdn.net/a9529lty/archive/2009/03.aspx)
* [2009年02月(4)](http://blog.csdn.net/a9529lty/archive/2009/02.aspx)
* [2009年01月(17)](http://blog.csdn.net/a9529lty/archive/2009/01.aspx)
* [2008年12月(34)](http://blog.csdn.net/a9529lty/archive/2008/12.aspx)
* [2008年11月(23)](http://blog.csdn.net/a9529lty/archive/2008/11.aspx)
* [2008年10月(16)](http://blog.csdn.net/a9529lty/archive/2008/10.aspx)
* [2008年09月(13)](http://blog.csdn.net/a9529lty/archive/2008/09.aspx)
* [2008年08月(21)](http://blog.csdn.net/a9529lty/archive/2008/08.aspx)
* [2008年07月(20)](http://blog.csdn.net/a9529lty/archive/2008/07.aspx)
* [2008年06月(1)](http://blog.csdn.net/a9529lty/archive/2008/06.aspx)
* [2008年05月(12)](http://blog.csdn.net/a9529lty/archive/2008/05.aspx)
* [2008年04月(2)](http://blog.csdn.net/a9529lty/archive/2008/04.aspx)
* [2008年03月(3)](http://blog.csdn.net/a9529lty/archive/2008/03.aspx)
* [2008年01月(4)](http://blog.csdn.net/a9529lty/archive/2008/01.aspx)
* [2007年12月(51)](http://blog.csdn.net/a9529lty/archive/2007/12.aspx)
# java实现double的精确计算和四舍五入 [收藏]( "收藏到我的网摘中，并分享给我的朋友")

****
**Java中的简单浮点数类型float和double不能够进行运算。不光是Java，在其它很多编程语言中也有这样的问题。在大多数情况下，计算的结果是准确的，但是多试几次（可以做一个循环）就可以试出类似上面的错误。现在终于理解为什么要有BCD码了。
这个问题相当严重，如果你有9.999999999999元，你的计算机是不会认为你可以购买10元的商品的。
在有的编程语言中提供了专门的货币类型来处理这种情况，但是Java没有。现在让我们看看如何解决这个问题。
四舍五入
我们的第一个反应是做四舍五入。Math类中的round方法不能设置保留几位小数，我们只能象这样（保留两位）：
public double round(double value){
    return Math.round(value*100)/100.0;
}
非常不幸，上面的代码并不能正常工作，给这个方法传入4.015它将返回4.01而不是4.02，如我们在上面看到的
4.015*100=401.49999999999994
因此如果我们要做到精确的四舍五入，不能利用简单类型做任何运算
java.text.DecimalFormat也不能解决这个问题：
System.out.println(new java.text.DecimalFormat("0.00").format(4.025));
输出是4.02**
**java 代码

import java.math.BigDecimal; public class Arith { private static final int DEF_DIV_SCALE = 10; private Arith() { } /** * 提供精确的加法运算。 * @param v1 被加数 * @param v2 加数 * @return 两个参数的和 */ public static double add(double v1,double v2) { BigDecimal b1 = new BigDecimal(Double.toString(v1)); BigDecimal b2 = new BigDecimal(Double.toString(v2)); return b1.add(b2).doubleValue(); } /** * 提供精确的减法运算。 * @param v1 被减数 * @param v2 减数 * @return 两个参数的差 */ public static double sub(double v1,double v2) { BigDecimal b1 = new BigDecimal(Double.toString(v1)); BigDecimal b2 = new BigDecimal(Double.toString(v2)); return b1.subtract(b2).doubleValue(); } /** * 提供精确的乘法运算。 * @param v1 被乘数 * @param v2 乘数 * @return 两个参数的积 */ public static double mul(double v1,double v2) { BigDecimal b1 = new BigDecimal(Double.toString(v1)); BigDecimal b2 = new BigDecimal(Double.toString(v2)); return b1.multiply(b2).doubleValue(); } /** * 提供（相对）精确的除法运算，当发生除不尽的情况时，精确到 * 小数点以后10位，以后的数字四舍五入。 * @param v1 被除数 * @param v2 除数 * @return 两个参数的商 */ public static double div(double v1,double v2) { return div(v1,v2,DEF_DIV_SCALE); } /** * 提供（相对）精确的除法运算。当发生除不尽的情况时，由scale参数指 * 定精度，以后的数字四舍五入。 * @param v1 被除数 * @param v2 除数 * @param scale 表示表示需要精确到小数点以后几位。 * @return 两个参数的商 */ public static double div(double v1,double v2,int scale) { if(scale<0) { throw new IllegalArgumentException("The scale must be a positive integer or zero"); } BigDecimal b1 = new BigDecimal(Double.toString(v1)); BigDecimal b2 = new BigDecimal(Double.toString(v2)); return b1.divide(b2,scale,BigDecimal.ROUND_HALF_UP).doubleValue(); } /** * 提供精确的小数位四舍五入处理。 * @param v 需要四舍五入的数字 * @param scale 小数点后保留几位 * @return 四舍五入后的结果 */ public static double round(double v,int scale){ if(scale<0) { throw new IllegalArgumentException("The scale must be a positive integer or zero"); } BigDecimal b = new BigDecimal(Double.toString(v)); BigDecimal one = new BigDecimal("1"); return b.divide(one,scale,BigDecimal.ROUND_HALF_UP).doubleValue(); } }
**

发表于 @ 2009年05月18日　23:07:00 | [评论( loading...  )](http://blog.csdn.net/a9529lty/archive/2009/05/18/4199684.aspx#FeedBack "评论")| [编辑](http://writeblog.csdn.net/PostEdit.aspx?entryId=4199684 "编辑")| [举报](mailto:webmaster@csdn.net?subject=Article%20Report!!!&body=Author:a9529lty%0D%0AURL:http://blog.csdn.net/ArticleContent.aspx?UserName=a9529lty&Entryid=4199684)| [收藏]( "收藏到我的网摘中，并分享给我的朋友")

### [旧一篇:淘宝生成预览图片](http://blog.csdn.net/a9529lty/archive/2009/05/13/4179695.aspx) | [新一篇:Oracle层次查询和分析函数](http://blog.csdn.net/a9529lty/archive/2009/05/27/4221509.aspx)[]()

Copyright © a9529ltyPowered by CSDN Blog![]()
