---
layout: post
title: "jdom解析xml - - JavaEye技术网站"
categories: xml
tags: 
 - xml
 - jdom
--- 

# jdom解析xml - - JavaEye技术网站

[首页](http://www.javaeye.com/) [新闻](http://www.javaeye.com/news) [论坛](http://www.javaeye.com/forums) [问答](http://www.javaeye.com/ask) [博客](http://www.javaeye.com/blogs) [招聘](http://www.javaeye.com/job) [更多 ▼](http://wuhongyu.javaeye.com/blog/361842#)

[专栏](http://www.javaeye.com/wiki) [文摘](http://www.javaeye.com/articles) [圈子](http://www.javaeye.com/groups) [搜索](http://www.javaeye.com/google_search)

[您还未登录 !](http://wuhongyu.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://wuhongyu.javaeye.com/login) [注册](http://wuhongyu.javaeye.com/signup)

# [wuhongyu](http://wuhongyu.javaeye.com/)

永久域名 [http://wuhongyu.javaeye.com/](http://wuhongyu.javaeye.com/)

[利用Apache Commons Email发送邮件](http://wuhongyu.javaeye.com/blog/362972 "利用Apache Commons Email发送邮件") | [定期备份web工程，压缩为.zip文件](http://wuhongyu.javaeye.com/blog/361148 "定期备份web工程，压缩为.zip文件")

2009-04-05

### [jdom解析xml](http://wuhongyu.javaeye.com/blog/361842)
    xml是一种广为使用的可扩展标记语言，java中解析xml的方式有很多，最常用的像jdom、dom4j、sax等等。前两天刚好有个程序需要解析xml，就学了下jdom，写了个小例子，这里做个学习笔记。

 

    要使用jdom解析xml文件，需要下载jdom的包，我使用的是jdom-1.1。解压之后，将lib文件夹下的.jar文件以及build文件夹下的jdom.jar拷贝到工程文件夹下，然后就可以使用jdom操作xml文件了。

 

    一、读取xml文件

 

    假设有这样一个xml文件：
<?xml version="1.0" encoding="UTF-8"?> <sys-config> <jdbc-info> <driver-class-name>oracle.jdbc.driver.OracleDriver</driver-class-name> <url>jdbc:oracle:thin:@localhost:1521:database</url> <user-name>why</user-name> <password>why</password> </jdbc-info> <provinces-info> <province id="hlj" name="黑龙江"> <city id="harb">哈尔滨</city> <city id="nj">嫩江</city> </province> <province id="jl" name="吉林"></province> </provinces-info> </sys-config>

   

    首先，用 org.jdom.input.SAXBuilder 这个类取得要操作的xml文件，会返回一个 org.jdom.Document 对象，这里需要做一下异常处理。然后，取得这个xml文件的根节点，org.jdom.Element 代表xml文件中的一个节点，取得跟节点后，便可以读取xml文件中的信息。利用 org.jdom.xpath.XPath 可以取得xml中的任意制定的节点中的信息。

    例如，要取得上面文件中的 <jdbc-info> 下的 <driver-class-name> 中的内容，先取得这个节点Element driverClassNameElement = (Element)XPath.selectSingleNode(rootEle, "//sys-config/jdbc-info/driver-class-name")，注意，根节点前要使用两个 "/" ，然后，用 driverClassNameElement.getText() 便可以取得这个节点下的信息。

    如果一个节点下有多个名称相同的子节点，可以用XPath.selectNodes()方法取得多个子节点的List，遍历这个List就可以操作各个子节点的内容了。

    下面是我写的读取上面xml文件的例子，比起文字描述更直观一些吧：
package com.why.jdom; import java.io.IOException; import java.util.Iterator; import java.util.List; import org.jdom.input.SAXBuilder; import org.jdom.xpath.XPath; import org.jdom.Document; import org.jdom.Element; import org.jdom.JDOMException; public class ReadXML { /** * @param args */ public static void main(String[] args) { SAXBuilder sax = new SAXBuilder(); try { Document doc = sax.build("src/config.xml"); Element rootEle = doc.getRootElement(); Element driverClassNameElement = (Element)XPath.selectSingleNode(rootEle, "//sys-config/jdbc-info/driver-class-name"); String driverClassName = driverClassNameElement.getText(); System.out.println("driverClassName = " + driverClassName); List provinceList = XPath.selectNodes(rootEle, "//sys-config/provinces-info/province"); for(Iterator it = provinceList.iterator();it.hasNext();){ Element provinceEle = (Element)it.next(); String proId = provinceEle.getAttributeValue("id"); String proName = provinceEle.getAttributeValue("name"); System.out.println("provinceId = " + proId + " provinceName = " + proName); List cityEleList = (List)provinceEle.getChildren("city"); for(Iterator cityIt = cityEleList.iterator();cityIt.hasNext();){ Element cityEle = (Element)cityIt.next(); String cityId = cityEle.getAttributeValue("id"); String cityName = cityEle.getText(); System.out.println(" cityId = " + cityId + " cityName = " + cityName); } } } catch (JDOMException e) { // TODO 自动生成 catch 块 e.printStackTrace(); } catch (IOException e) { // TODO 自动生成 catch 块 e.printStackTrace(); } } }

 

 

    二、写xml文件

   

    写xml文件与读取xml文件的操作类似，利用 org.jdom.output.XMLOutputter 就可以将处理好的xml输出到文件了。可以设置文件的编码方式，不过一般使用UTF-8就可以了。代码如下：

 
package com.why.jdom; import java.io.FileNotFoundException; import java.io.FileOutputStream; import java.io.IOException; import org.jdom.Document; import org.jdom.Element; import org.jdom.output.XMLOutputter; public class WriteXML { /** * @param args */ public static void main(String[] args) { // TODO 自动生成方法存根 Element rootEle = new Element("sys-config"); Element provincesEle = new Element("provinces-info"); Element provinceEle = new Element("province"); provinceEle.setAttribute("id","hlj"); provinceEle.setAttribute("name","黑龙江省"); Element cityEle1 = new Element("city"); cityEle1.setAttribute("id","harb"); cityEle1.addContent("哈尔滨"); Element cityEle2 = new Element("city"); cityEle2.setAttribute("id","nj"); cityEle2.addContent("嫩江"); provinceEle.addContent(cityEle1); provinceEle.addContent(cityEle2); provincesEle.addContent(provinceEle); rootEle.addContent(provincesEle); Document doc = new Document(rootEle); XMLOutputter out = new XMLOutputter(); // out.setFormat(Format.getCompactFormat().setEncoding("GBK"));//设置文件编码，默认为UTF-8 String xmlStr = out.outputString(doc); System.out.println(xmlStr); try { out.output(doc, new FileOutputStream("c:/test.xml")); } catch (FileNotFoundException e) { // TODO 自动生成 catch 块 e.printStackTrace(); } catch (IOException e) { // TODO 自动生成 catch 块 e.printStackTrace(); } } }

 

[利用Apache Commons Email发送邮件](http://wuhongyu.javaeye.com/blog/362972 "利用Apache Commons Email发送邮件") | [定期备份web工程，压缩为.zip文件](http://wuhongyu.javaeye.com/blog/361148 "定期备份web工程，压缩为.zip文件")
* 16:57
* 浏览 (511)
* [评论](http://wuhongyu.javaeye.com/blog/361842#comments) (0)
* 分类: [java 学习笔记](http://wuhongyu.javaeye.com/category/88898)
* [相关推荐](http://www.javaeye.com/wiki/topic/361842)

### 评论

[]()
### 发表评论

您还没有登录，请[登录](http://wuhongyu.javaeye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![wuhongyu的博客]( "wuhongyu的博客: ")](http://wuhongyu.javaeye.com/)

wuhongyu

* 浏览: 3811 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 哈尔滨
* ![]()
* [详细资料](http://wuhongyu.javaeye.com/blog/profile) [留言簿](http://wuhongyu.javaeye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://wuhongyu.javaeye.com/blog/user_visits)

[![athenaer的博客]( "athenaer的博客: ")](http://athenaer.javaeye.com/)

[athenaer](http://athenaer.javaeye.com/)

[![gogo123150的博客]( "gogo123150的博客: gogo123150")](http://gogo123150.javaeye.com/)

[gogo123150](http://gogo123150.javaeye.com/)
[![godlewis的博客]( "godlewis的博客: godlewis")](http://godlewis.javaeye.com/)

[godlewis](http://godlewis.javaeye.com/)

[![kuailewuhai的博客]( "kuailewuhai的博客: ")](http://kuailewuhai.javaeye.com/)

[kuailewuhai](http://kuailewuhai.javaeye.com/)

### 博客分类

* [全部博客 (15)](http://wuhongyu.javaeye.com/)
* [java 学习笔记 (14)](http://wuhongyu.javaeye.com/category/88898)
### 其他分类

* [我的收藏](http://wuhongyu.javaeye.com/blog/favorite) (6)
* [我的论坛主题贴](http://wuhongyu.javaeye.com/blog/topic) (15)
* [我的所有论坛贴](http://wuhongyu.javaeye.com/blog/post) (4)
* [我的精华良好贴](http://wuhongyu.javaeye.com/blog/article) (0)

### 最近加入圈子
### 存档

* [2010-02](http://wuhongyu.javaeye.com/blog/monthblog/2010-02) (1)
* [2010-01](http://wuhongyu.javaeye.com/blog/monthblog/2010-01) (1)
* [2009-12](http://wuhongyu.javaeye.com/blog/monthblog/2009-12) (2)
* [更多存档...](http://wuhongyu.javaeye.com/blog/monthblog_more)

### 最新评论

* [JAVA调用系统命令或可执行 ...](http://wuhongyu.javaeye.com/blog/461477#comments "JAVA调用系统命令或可执行程序")
谢谢分享啊！
-- by [xiaoqing20](http://xiaoqing20.javaeye.com/)
* [JAVA生成MD5校验码及算法 ...](http://wuhongyu.javaeye.com/blog/435455#comments "JAVA生成MD5校验码及算法实现")
很牛X的,不过好像在哪见过
-- by [toopoo](http://toopoo.javaeye.com/)
### 评论排行榜

* [JAVA调用系统命令或可执行程序](http://wuhongyu.javaeye.com/blog/461477 "JAVA调用系统命令或可执行程序")
* [JAVA生成MD5校验码及算法实现](http://wuhongyu.javaeye.com/blog/435455 "JAVA生成MD5校验码及算法实现")
* [新家入住](http://wuhongyu.javaeye.com/blog/357891 "新家入住")
* [在eclipse中建立EJB工程](http://wuhongyu.javaeye.com/blog/370541 "在eclipse中建立EJB工程")
* [jdom解析xml](http://wuhongyu.javaeye.com/blog/361842 "jdom解析xml")

* [![Rss]()](http://wuhongyu.javaeye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://wuhongyu.javaeye.com/rss)
* [![Rss_xianguo]()](http://www.xianguo.com/subscribe.php?url=http://wuhongyu.javaeye.com/rss)
* [[什么是RSS?]](http://www.google.com/search?hl=zh-CN&q=RSS)
声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2009 JavaEye.com. All rights reserved. 上海炯耐计算机软件有限公司 [ 沪ICP备05023328号 ]
