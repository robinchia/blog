---
layout: post
title: "java字符串的各种编码转换"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_字符集
--- 

# java字符串的各种编码转换

# [Java世界](http://www.blogjava.net/rabbit/)

学习笔记

### 导航

* [BlogJava](http://www.blogjava.net/)
* [首页](http://www.blogjava.net/rabbit/)
* [新随笔](http://www.blogjava.net/rabbit/admin/EditPosts.aspx?opt=1)
* [联系](http://www.blogjava.net/rabbit/contact.aspx?id=1)
* [聚合](http://www.blogjava.net/rabbit/rss)[![]()](http://www.blogjava.net/rabbit/rss)
* [管理](http://www.blogjava.net/rabbit/admin/EditPosts.aspx)
[<]( "Go to the previous month") 2008年3月 [>]( "Go to the next month") 日 一 二 三 四 五 六 24 25 26 27 28 29 1 2 3 4 5 6 [7](http://www.blogjava.net/rabbit/archive/2008/03/07.html) [8](http://www.blogjava.net/rabbit/archive/2008/03/08.html) 9 10 [11](http://www.blogjava.net/rabbit/archive/2008/03/11.html) [12](http://www.blogjava.net/rabbit/archive/2008/03/12.html) 13 [14](http://www.blogjava.net/rabbit/archive/2008/03/14.html) 15 16 17 18 19 [20](http://www.blogjava.net/rabbit/archive/2008/03/20.html) 21 22 23 [24](http://www.blogjava.net/rabbit/archive/2008/03/24.html) 25 26 [27](http://www.blogjava.net/rabbit/archive/2008/03/27.html) [28](http://www.blogjava.net/rabbit/archive/2008/03/28.html) 29 30 31 1 2 3 4 5

### 留言簿(6)

* [给我留言](http://www.blogjava.net/rabbit/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/rabbit/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/rabbit/admin/MyMessages.aspx)

### 随笔档案

* [2010年1月 (3)](http://www.blogjava.net/rabbit/archive/2010/01.html)
* [2009年7月 (2)](http://www.blogjava.net/rabbit/archive/2009/07.html)
* [2009年5月 (2)](http://www.blogjava.net/rabbit/archive/2009/05.html)
* [2009年1月 (1)](http://www.blogjava.net/rabbit/archive/2009/01.html)
* [2008年12月 (2)](http://www.blogjava.net/rabbit/archive/2008/12.html)
* [2008年10月 (1)](http://www.blogjava.net/rabbit/archive/2008/10.html)
* [2008年8月 (1)](http://www.blogjava.net/rabbit/archive/2008/08.html)
* [2008年7月 (2)](http://www.blogjava.net/rabbit/archive/2008/07.html)
* [2008年6月 (9)](http://www.blogjava.net/rabbit/archive/2008/06.html)
* [2008年5月 (1)](http://www.blogjava.net/rabbit/archive/2008/05.html)
* [2008年3月 (9)](http://www.blogjava.net/rabbit/archive/2008/03.html)
* [2008年2月 (9)](http://www.blogjava.net/rabbit/archive/2008/02.html)
* [2008年1月 (1)](http://www.blogjava.net/rabbit/archive/2008/01.html)
* [2007年12月 (4)](http://www.blogjava.net/rabbit/archive/2007/12.html)
* [2007年11月 (8)](http://www.blogjava.net/rabbit/archive/2007/11.html)
* [2007年10月 (16)](http://www.blogjava.net/rabbit/archive/2007/10.html)

### 文章档案

* [2007年11月 (8)](http://www.blogjava.net/rabbit/archives/2007/11.html)
* [2007年10月 (1)](http://www.blogjava.net/rabbit/archives/2007/10.html)

### 阅读排行榜

* [1. 怎么查看端口占用情况?(22478)](http://www.blogjava.net/rabbit/archive/2008/03/12/185559.html)
* [2. java字符串的各种编码转换 (14630)](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html)
* [3. 如何导入.sql文件到mysql中？(7494)](http://www.blogjava.net/rabbit/archive/2008/03/08/184760.html)
* [4. 怎样用Eclipse生成jar文件(2261)](http://www.blogjava.net/rabbit/archive/2008/02/25/181961.html)
* [5. Linux下解压rar文件(1151)](http://www.blogjava.net/rabbit/archive/2008/06/17/208623.html)

### 评论排行榜

* [1. java字符串的各种编码转换 (13)](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html)
* [2. 怎么查看端口占用情况?(12)](http://www.blogjava.net/rabbit/archive/2008/03/12/185559.html)
* [3. 怎样用Eclipse生成jar文件(2)](http://www.blogjava.net/rabbit/archive/2008/02/25/181961.html)
* [4. 如何解决呢？请教。(2)](http://www.blogjava.net/rabbit/archive/2007/10/15/152922.html)
* [5. 单子（Singleton）设计模式(2)](http://www.blogjava.net/rabbit/archive/2007/10/22/154968.html)
### 常用链接

* [我的随笔](http://www.blogjava.net/rabbit/MyPosts.html)
* [我的评论](http://www.blogjava.net/rabbit/MyComments.html)
* [我的参与](http://www.blogjava.net/rabbit/OtherPosts.html)
* [最新评论](http://www.blogjava.net/rabbit/RecentComments.html)

### 统计

* 随笔 - 71
* 文章 - 9
* 评论 - 48
* 引用 - 0

### 积分与排名

* 积分 - 64465
* 排名 - 229

### 天籁村

### 新华网

### 雅虎

### 最新评论 [![]()](http://www.blogjava.net/rabbit/CommentsRSS.aspx)

* [1. re: java字符串的各种编码转换 [未登录]](http://www.blogjava.net/rabbit/archive/2010/02/11/189009.html#312565)
* 谢了
* --lbom
* [2. re: 怎么查看端口占用情况?](http://www.blogjava.net/rabbit/archive/2009/12/31/185559.html#307931)
* 万分感谢！
* --yjm
* [3. re: java字符串的各种编码转换 [未登录]](http://www.blogjava.net/rabbit/archive/2009/12/07/189009.html#305021)
* 谢谢
* --dong
* [4. re: java字符串的各种编码转换 [未登录]](http://www.blogjava.net/rabbit/archive/2009/12/06/189009.html#304945)
* 评论内容较长,点击标题查看
* --wolf
* [5. re: 怎么查看端口占用情况?](http://www.blogjava.net/rabbit/archive/2009/11/15/185559.html#302431)
* 太谢谢你了，， 呵呵帮我解决了 很大的问题
* --常霖

## [java字符串的各种编码转换](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html)

import java.io.UnsupportedEncodingException;

/**
 * 转换字符串的编码
 */
public class ChangeCharset {
 /** 7位ASCII字符，也叫作ISO646-US、Unicode字符集的基本拉丁块 */
 public static final String US_ASCII = "US-ASCII";
 /** ISO 拉丁字母表 No.1，也叫作 ISO-LATIN-1 */
 public static final String ISO_8859_1 = "ISO-8859-1";
 /** 8 位 UCS 转换格式 */
 public static final String UTF_8 = "UTF-8";
 /** 16 位 UCS 转换格式，Big Endian（最低地址存放高位字节）字节顺序 */
 public static final String UTF_16BE = "UTF-16BE";
 /** 16 位 UCS 转换格式，Little-endian（最高地址存放低位字节）字节顺序 */
 public static final String UTF_16LE = "UTF-16LE";
 /** 16 位 UCS 转换格式，字节顺序由可选的字节顺序标记来标识 */
 public static final String UTF_16 = "UTF-16";
 /** 中文超大字符集 */
 public static final String GBK = "GBK";

 /**
  * 将字符编码转换成US-ASCII码
  */
 public String toASCII(String str) throws UnsupportedEncodingException{
  return this.changeCharset(str, US_ASCII);
 }
 /**
  * 将字符编码转换成ISO-8859-1码
  */
 public String toISO_8859_1(String str) throws UnsupportedEncodingException{
  return this.changeCharset(str, ISO_8859_1);
 }
 /**
  * 将字符编码转换成UTF-8码
  */
 public String toUTF_8(String str) throws UnsupportedEncodingException{
  return this.changeCharset(str, UTF_8);
 }
 /**
  * 将字符编码转换成UTF-16BE码
  */
 public String toUTF_16BE(String str) throws UnsupportedEncodingException{
  return this.changeCharset(str, UTF_16BE);
 }
 /**
  * 将字符编码转换成UTF-16LE码
  */
 public String toUTF_16LE(String str) throws UnsupportedEncodingException{
  return this.changeCharset(str, UTF_16LE);
 }
 /**
  * 将字符编码转换成UTF-16码
  */
 public String toUTF_16(String str) throws UnsupportedEncodingException{
  return this.changeCharset(str, UTF_16);
 }
 /**
  * 将字符编码转换成GBK码
  */
 public String toGBK(String str) throws UnsupportedEncodingException{
  return this.changeCharset(str, GBK);
 }
 
 /**
  * 字符串编码转换的实现方法
  * @param str  待转换编码的字符串
  * @param newCharset 目标编码
  * @return
  * @throws UnsupportedEncodingException
  */
 public String changeCharset(String str, String newCharset)
   throws UnsupportedEncodingException {
  if (str != null) {
   //用默认字符编码解码字符串。
   byte[] bs = str.getBytes();
   //用新的字符编码生成字符串
   return new String(bs, newCharset);
  }
  return null;
 }
 /**
  * 字符串编码转换的实现方法
  * @param str  待转换编码的字符串
  * @param oldCharset 原编码
  * @param newCharset 目标编码
  * @return
  * @throws UnsupportedEncodingException
  */
 public String changeCharset(String str, String oldCharset, String newCharset)
   throws UnsupportedEncodingException {
  if (str != null) {
   //用旧的字符编码解码字符串。解码可能会出现异常。
   byte[] bs = str.getBytes(oldCharset);
   //用新的字符编码生成字符串
   return new String(bs, newCharset);
  }
  return null;
 }

 public static void main(String[] args) throws UnsupportedEncodingException {
  ChangeCharset test = new ChangeCharset();
  String str = "This is a 中文的 String!";
  System.out.println("str: " + str);
  String gbk = test.toGBK(str);
  System.out.println("转换成GBK码: " + gbk);
  System.out.println();
  String ascii = test.toASCII(str);
  System.out.println("转换成US-ASCII码: " + ascii);
  gbk = test.changeCharset(ascii,ChangeCharset.US_ASCII, ChangeCharset.GBK);
  System.out.println("再把ASCII码的字符串转换成GBK码: " + gbk);
  System.out.println();
  String iso88591 = test.toISO_8859_1(str);
  System.out.println("转换成ISO-8859-1码: " + iso88591);
  gbk = test.changeCharset(iso88591,ChangeCharset.ISO_8859_1, ChangeCharset.GBK);
  System.out.println("再把ISO-8859-1码的字符串转换成GBK码: " + gbk);
  System.out.println();
  String utf8 = test.toUTF_8(str);
  System.out.println("转换成UTF-8码: " + utf8);
  gbk = test.changeCharset(utf8,ChangeCharset.UTF_8, ChangeCharset.GBK);
  System.out.println("再把UTF-8码的字符串转换成GBK码: " + gbk);
  System.out.println();
  String utf16be = test.toUTF_16BE(str);
  System.out.println("转换成UTF-16BE码:" + utf16be);
  gbk = test.changeCharset(utf16be,ChangeCharset.UTF_16BE, ChangeCharset.GBK);
  System.out.println("再把UTF-16BE码的字符串转换成GBK码: " + gbk);
  System.out.println();
  String utf16le = test.toUTF_16LE(str);
  System.out.println("转换成UTF-16LE码:" + utf16le);
  gbk = test.changeCharset(utf16le,ChangeCharset.UTF_16LE, ChangeCharset.GBK);
  System.out.println("再把UTF-16LE码的字符串转换成GBK码: " + gbk);
  System.out.println();
  String utf16 = test.toUTF_16(str);
  System.out.println("转换成UTF-16码:" + utf16);
  gbk = test.changeCharset(utf16,ChangeCharset.UTF_16LE, ChangeCharset.GBK);
  System.out.println("再把UTF-16码的字符串转换成GBK码: " + gbk);
  String s = new String("中文".getBytes("UTF-8"),"UTF-8");
  System.out.println(s);
 }
}
------------------------------------------------------------------------------------------------------------------
        java中的String类是按照unicode进行编码的，当使用String(byte[] bytes, String encoding)构造字符串时，encoding所指的是bytes中的数据是按照那种方式编码的，而不是最后产生的String是什么编码方式，换句话说，是让系统把bytes中的数据由encoding编码方式转换成unicode编码。如果不指明，bytes的编码方式将由jdk根据操作系统决定。

        当我们从文件中读数据时，最好使用InputStream方式，然后采用String(byte[] bytes, String encoding)指明文件的编码方式。不要使用Reader方式，因为Reader方式会自动根据jdk指明的编码方式把文件内容转换成unicode编码。

        当我们从数据库中读文本数据时，采用ResultSet.getBytes()方法取得字节数组，同样采用带编码方式的字符串构造方法即可。

ResultSet rs;
bytep[] bytes = rs.getBytes();
String str = new String(bytes, "gb2312");

不要采取下面的步骤。

ResultSet rs;
String str = rs.getString();
str = new String(str.getBytes("iso8859-1"), "gb2312");

        这种编码转换方式效率底。之所以这么做的原因是，ResultSet在getString()方法执行时，默认数据库里的数据编码方式为iso8859-1。系统会把数据依照iso8859-1的编码方式转换成unicode。使用str.getBytes("iso8859-1")把数据还原，然后利用new String(bytes, "gb2312")把数据从gb2312转换成unicode，中间多了好多步骤。

        从HttpRequest中读参数时，利用reqeust.setCharacterEncoding()方法设置编码方式，读出的内容就是正确的了。

posted on 2008-03-27 15:03 [Rabbit](http://www.blogjava.net/rabbit/) 阅读(14630) [评论(13)](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#Post)  [编辑](http://www.blogjava.net/rabbit/admin/EditPosts.aspx?postid=189009)  [收藏](http://www.blogjava.net/rabbit/AddToFavorite.aspx?id=189009)
![]()

[]()[]()

[
### 评论

### [#](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#241299 "permalink: re: java字符串的各种编码转换 ") []()re: java字符串的各种编码转换 2008-11-19 10:47 [邀月](http://www.blogjava.net/downmoon/)

]()

感谢分享  [回复](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#post)  [更多评论](http://www.blogjava.net/comment?author=%e9%82%80%e6%9c%88 "查看该作者发表过的评论") []()  []()

### [#](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#241392 "permalink: re: java字符串的各种编码转换 [未登录]") []()re: java字符串的各种编码转换 [未登录] 2008-11-19 15:21 [rabbit]()

谢谢支持！  [回复](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#post)  [更多评论](http://www.blogjava.net/comment?author=rabbit "查看该作者发表过的评论") []()  []()

### [#](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#243964 "permalink: re: java字符串的各种编码转换 ") []()re: java字符串的各种编码转换 2008-12-02 16:44 [3分毒]()

顶~  [回复](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#post)  [更多评论](http://www.blogjava.net/comment?author=3%e5%88%86%e6%af%92 "查看该作者发表过的评论") []()  []()

### [#](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#261045 "permalink: re: java字符串的各种编码转换 ") []()re: java字符串的各种编码转换 2009-03-20 15:27 [清闲散人]()

inputstreamReader 可以直接指定编码的……  [回复](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#post)  [更多评论](http://www.blogjava.net/comment?author=%e6%b8%85%e9%97%b2%e6%95%a3%e4%ba%ba "查看该作者发表过的评论") []()  []()

### [#](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#261558 "permalink: re: java字符串的各种编码转换 [未登录]") []()re: java字符串的各种编码转换 [未登录] 2009-03-23 18:13 [yxw]()

相当的有用啊，感谢分享  [回复](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#post)  [更多评论](http://www.blogjava.net/comment?author=yxw "查看该作者发表过的评论") []()  []()

### [#](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#288167 "permalink: re: java字符串的各种编码转换 ") []()re: java字符串的各种编码转换 2009-07-24 11:24 [1111]()

有些编码不能直接转换的吧!
  [回复](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#post)  [更多评论](http://www.blogjava.net/comment?author=1111 "查看该作者发表过的评论") []()  []()

### [#](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#288194 "permalink: re: java字符串的各种编码转换 [未登录]") []()re: java字符串的各种编码转换 [未登录] 2009-07-24 13:55 [Rabbit]()

具体情况具体分析，不一定全部适用。  [回复](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#post)  [更多评论](http://www.blogjava.net/comment?author=Rabbit "查看该作者发表过的评论") []()  []()

### [#](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#293606 "permalink: re: java字符串的各种编码转换 ") []()re: java字符串的各种编码转换 2009-09-02 15:46 [Greale]()

"当使用String(byte[] bytes, String encoding)构造字符串时，encoding所指的是bytes中的数据是按照那种方式编码的，而不是最后产生的String是什么编码方"
完全错误,不要误导读者.  [回复](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#post)  [更多评论](http://www.blogjava.net/comment?author=Greale "查看该作者发表过的评论") []()  []()

### [#](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#293686 "permalink: re: java字符串的各种编码转换 [未登录]") []()re: java字符串的各种编码转换 [未登录] 2009-09-03 08:34 [Rabbit]()

@Greale
请加以测试给出结论，请查看String str = new String(bytes, "gb2312");这个字符类的原代码即可得出答案。  [回复](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#post)  [更多评论](http://www.blogjava.net/comment?author=Rabbit "查看该作者发表过的评论") []()  []()

### [#](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#301420 "permalink: re: java字符串的各种编码转换 ") []()re: java字符串的各种编码转换 2009-11-06 13:24 [tayoto]()

简直是不负责任呀......全是错的.......敢不敢看文档再发  [回复](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#post)  [更多评论](http://www.blogjava.net/comment?author=tayoto "查看该作者发表过的评论") []()  []()

### [#](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#304945 "permalink: re: java字符串的各种编码转换 [未登录]") []()re: java字符串的各种编码转换 [未登录] 2009-12-06 22:28 [wolf]()

@Greale
我来说句，下面这个来自jdk的文档，
String(byte[] bytes, String charsetName)
构造一个新的 String，方法是使用指定的字符集解码指定的字节数组
rabbit是对的。  [回复](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#post)  [更多评论](http://www.blogjava.net/comment?author=wolf "查看该作者发表过的评论") []()  []()

### [#](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#305021 "permalink: re: java字符串的各种编码转换 [未登录]") []()re: java字符串的各种编码转换 [未登录] 2009-12-07 16:47 [dong]()

谢谢  [回复](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#post)  [更多评论](http://www.blogjava.net/comment?author=dong "查看该作者发表过的评论") []()  []()

### [#](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#312565 "permalink: re: java字符串的各种编码转换 [未登录]") []()re: java字符串的各种编码转换 [未登录][]() 2010-02-11 10:24 [lbom]()

谢了  [回复](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html#post)  [更多评论](http://www.blogjava.net/comment?author=lbom "查看该作者发表过的评论") []()  []()
[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [淘宝网诚聘资深J2ME开发工程师](http://job.cnblogs.com/offer/6509/)
IT新闻：
· [Novell推新虚拟设备 简化向Windows 7迁移](http://news.cnblogs.com/n/59823/)
· [原淘宝商城负责人黄若加盟当当网任COO](http://news.cnblogs.com/n/59822/)
· [微软的“三屏一云”计划导致SQL CE数据库消失？](http://news.cnblogs.com/n/59820/)
· [苹果招聘LTE技术经理 或推4G版iPhone](http://news.cnblogs.com/n/59818/)
· [金山将砸2亿开发网游 今年推新款作品](http://news.cnblogs.com/n/59815/)
专题：[Android](http://kb.cnblogs.com/zt/android/)  [iPad](http://kb.cnblogs.com/zt/iPad/)  [jQuery](http://kb.cnblogs.com/zt/jquery/)  [Chrome OS](http://kb.cnblogs.com/zt/chrome/)    [博客园首页](http://www.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [知识库](http://kb.cnblogs.com/)  [学英语](http://a4.yeshj.com/rd/34143/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html&SourceURL=/rabbit/archive/2008/03/27/189009.html)       [使用Ctrl+Enter键可以直接提交]   [每天10分钟，轻松学英语](http://a4.yeshj.com/rd/34138/)  推荐职位：
· [飞信服务器端高级.NET开发工程师(新媒传信)](http://job.cnblogs.com/offer/6318/)
· [.NET飞信官网开发工程师(新媒传信)](http://job.cnblogs.com/offer/6319/)
· [.NET技术开发总监(广州衣酷)](http://job.cnblogs.com/offer/6189/)
· [Web前端研发工程师(百度)](http://job.cnblogs.com/offer/6492/)
· [.NET初级程序员 (北京安人)](http://job.cnblogs.com/offer/6123/)
· [.NET中级程序员 (北京安人)](http://job.cnblogs.com/offer/6124/)
· [C++开发工程师(沪江网)](http://job.cnblogs.com/zt/yeshj/#offer3)
· [前端开发工程师(沪江网)](http://job.cnblogs.com/zt/yeshj/#offer2)

博客园首页随笔：
· [界面开发(五)--- 界面优化](http://www.cnblogs.com/zhjp11/archive/2010/03/26/1697195.html)
· [项目“结项期”中如何改善开发VS测试效率的一点想法](http://www.cnblogs.com/guilin_gavin/archive/2010/03/26/1697193.html)
· [团队基础生成自动化流程之最佳实践总论(III) – 重写生成号产生机制](http://www.cnblogs.com/sun/archive/2010/03/26/1697182.html)
· [Phase Change Memory初步](http://www.cnblogs.com/superymk/archive/2010/03/26/1697171.html)
· [发布一款基于C#的机器视觉库](http://www.cnblogs.com/foamliu/archive/2010/03/26/1697167.html)
知识库：
· [如何明智选择数据库平台](http://kb.cnblogs.com/page/59817/)
· [我们的原型设计方法](http://kb.cnblogs.com/page/59812/)
· [写商业计划书——团队里该有谁？by Angels Den](http://kb.cnblogs.com/page/59807/)
· [google vs.yahoo:谁的社会化媒体策略更胜一筹?](http://kb.cnblogs.com/page/59802/)
· [淘心得——浅谈淘宝网的社会化之路](http://kb.cnblogs.com/page/59798/)  网站导航:

[博客园](http://www.cnblogs.com/)   [IT新闻](http://news.cnblogs.com/)   [个人主页](http://home.cnblogs.com/)   [博客生活](http://www.cnweblog.com/)   [IT博客网](http://www.cnitblog.com/)   [C++博客](http://www.cppblog.com/)   [博客园社区](http://space.cnblogs.com/) [管理](http://www.blogjava.net/rabbit/archive/2008/03/27/189009.html?opt=admin)   

Powered by:
[BlogJava](http://www.blogjava.net/)
Copyright © Rabbit
