---
layout: post
title: "XStream使用方法总结附实例代码_JSP编程_脚本之家"
categories: xml
tags: 
 - xml
 - xstream
--- 

# XStream使用方法总结附实例代码_JSP编程_脚本之家

页面导航： [首页](http://www.jb51.net/index.htm) → [网络编程](http://www.jb51.net/list/index_1.htm "网络编程") → [JSP编程](http://www.jb51.net/list/list_83_1.htm "JSP编程") → 正文内容 XStream 实例 

# XStream使用方法总结附实例代码

发布：dxy 字体：[[增加]() [减小]()] 类型：转载 

XStream是一个Java对象和XML相互转换的工具，很好很强大。提供了所有的基础类型、数组、集合等类型直接转换的支持。因此XML常用于数据交换、对象序列化（这种序列化和Java对象的序列化技术有着本质的区别）。
XStream中的核心类就是XStream类，一般来说，熟悉这个类基本就够用了，如果你用的更多，估计是你设计有问题，否则不需要。 
XStream对象相当Java对象和XML之间的转换器，转换过程是双向的。创建XSteam对象的方式很简单，只需要new XStream()即可。 
Java到xml，用toXML()方法。 
Xml到Java，用fromXML()方法。 
在没有任何设置默认情况下，java到xml的映射，是java成员名对应xml的元素名，java类的全名对应xml根元素的名字。而实际中，往往是xml和java类都有了，要完成相互转换，必须进行别名映射。 
别名配置包含三种情况： 
1、类别名，用alias(String name, Class type)。 
2、类成员别名，用aliasField(String alias, Class definedIn, String fieldName) 
3、类成员作为属性别名，用 aliasAttribute(Class definedIn, String attributeName, String alias)，单独命名没有意义，还要通过useAttributeFor(Class definedIn, String fieldName) 应用到某个类上。 
别名的配置是非常重要的，但是其中有些细节问题很重要，在例子中会专门做详细说明。 
另外还有不太常用的方法： 
addImplicitCollection(Class ownerType, String fieldName)，去掉集合类型生成xml的父节点。 
registerConverter(Converter converter) ，注册一个转换器。 
如果你的xml很大，或者为了安全性，以流的方式传输，那么XStream也提供丰富的API， 
使用起来也非常简便。目前还用不到，暂不考虑。 
如果这些基本的操作还不能满足你应用的需求，XStream提供丰富的扩展点。你可以实现自己的转换器。还可以利用XStream完成更负责的功能，比如输出其他非xml格式的数据，还可以输出html，还支持XML Dom类型数据，这些应用起来稍微复杂些。当然这些不是XStream应用的重点，也不用理会，真正需要的时候再查看API和源码研究研究。 
XStream的优点很多，但是也有一些小bug，比如在定义别名中的下划线“_”转换为xml后会变成“__”这个符号，很变态。因此，尽量避免在别名中实用任何符号，却是需要下划线的时候，可以考虑实用连接符“-”，这个没有问题。 
另外，我们的Java Bean中，常常有一些常量，在转换过程，XStream也会将这些常量转换过去，形成常量的xml节点，这显然不是想要的结果，对于常量字段，就不做转换了。 
下面给出一个非常典型的而且实用的例子，作为对总结的补充： 
package test; 
import java.util.List; 
/** 
* Created by IntelliJ IDEA.<br> 
* <b>User</b>: leizhimin<br> 
* <b>Date</b>: 2008-5-22 21:10:13<br> 
* <b>Note</b>: Please add comment here! 
*/ 
public class Person { 
private String name; 
private String age; 
private Profile profile; 
private List<Address> addlist; 
public Person(String name, String age, Profile profile, List<Address> addlist) { 
this.name = name; 
this.age = age; 
this.profile = profile; 
this.addlist = addlist; 
} 
public String toString() { 
return "Person{" + 
"name='" + name + '\'' + 
", age='" + age + '\'' + 
", profile=" + profile + 
", addlist=" + addlist + 
'}'; 
} 
} 
package test; 
import java.sql.Date; 
/** 
* Created by IntelliJ IDEA.<br> 
* <b>User</b>: leizhimin<br> 
* <b>Date</b>: 2008-5-22 21:10:32<br> 
* <b>Note</b>: Please add comment here! 
*/ 
public class Profile { 
private String job; 
private String tel; 
private String remark; 
public Profile(String job, String tel, String remark) { 
this.job = job; 
this.tel = tel; 
this.remark = remark; 
} 
public String toString() { 
return "Profile{" + 
"job='" + job + '\'' + 
", tel='" + tel + '\'' + 
", remark='" + remark + '\'' + 
'}'; 
} 
} 
package test; 
/** 
* Created by IntelliJ IDEA.<br> 
* <b>User</b>: leizhimin<br> 
* <b>Date</b>: 2008-5-22 21:10:22<br> 
* <b>Note</b>: Please add comment here! 
*/ 
public class Address { 
private String add; 
private String zipcode; 
public Address(String add, String zipcode) { 
this.add = add; 
this.zipcode = zipcode; 
} 
public String toString() { 
return "Address{" + 
"add='" + add + '\'' + 
", zipcode='" + zipcode + '\'' + 
'}'; 
} 
} 
package test; 
import com.thoughtworks.xstream.XStream; 
import java.util.List; 
import java.util.ArrayList; 
/** 
* Created by IntelliJ IDEA.<br> 
* <b>User</b>: leizhimin<br> 
* <b>Date</b>: 2008-5-22 21:10:47<br> 
* <b>Note</b>: XStream学习[http://lavasoft.blog.51cto.com] 
*/ 
public class TestXStream { 
public static void main(String args[]) { 
test(); 
} 
public static void test() { 
System.out.println("----------XStream学习:http://lavasoft.blog.51cto.com----------"); 
//目标对象 
Address address1 = new Address("郑州市经三路", "450001"); 
Address address2 = new Address("西安市雁塔路", "710002"); 
List<Address> addList = new ArrayList<Address>(); 
addList.add(address1); 
addList.add(address2); 
Profile profile = new Profile("软件工程师", "13512129933", "备注说明"); 
Person person = new Person("熔岩", "27", profile, addList); 
//转换装配 
XStream xStream = new XStream(); 
/************** 设置类别名 ****************/ 
xStream.alias("PERSON", test.Person.class); 
xStream.alias("PROFILE", test.Profile.class); 
xStream.alias("ADDRESS", test.Address.class); 
output(1, xStream, person); 
/************* 设置类成员的别名 ***************/ 
//设置Person类的name成员别名Name 
xStream.aliasField("Name", Person.class, "name"); 
/*[注意] 设置Person类的profile成员别名PROFILE,这个别名和Profile类的别名一致, 
* 这样可以保持XStream对象可以从profile成员生成的xml片段直接转换为Profile成员, 
* 如果成员profile的别名和Profile的别名不一致,则profile成员生成的xml片段不可 
* 直接转换为Profile对象,需要重新创建XStream对象,这岂不给自己找麻烦? */ 
xStream.aliasField("PROFILE", test.Person.class, "profile"); 
xStream.aliasField("ADDLIST", test.Person.class, "addlist"); 
xStream.aliasField("Add", test.Address.class, "add"); 
xStream.aliasField("Job", test.Profile.class, "job"); 
output(2, xStream, person); 
/******* 设置类成员为xml一个元素上的属性 *******/ 
xStream.useAttributeFor(Address.class, "zipcode"); 
/************* 设置属性的别名 ***************/ 
xStream.aliasAttribute(test.Address.class, "zipcode", "Zipcode"); 
output(3, xStream, person); 
/************* 将xml转为java对象 ******×****/ 
String person_xml = "<PERSON>\n" + 
" <Name>熔岩</Name>\n" + 
" <age>27</age>\n" + 
" <PROFILE>\n" + 
" <Job>软件工程师</Job>\n" + 
" <tel>13512129933</tel>\n" + 
" <remark>备注说明</remark>\n" + 
" </PROFILE>\n" + 
" <ADDLIST>\n" + 
" <ADDRESS Zipcode=\"450001\">\n" + 
" <Add>郑州市经三路</Add>\n" + 
" </ADDRESS>\n" + 
" <ADDRESS Zipcode=\"710002\">\n" + 
" <Add>西安市雁塔路</Add>\n" + 
" </ADDRESS>\n" + 
" </ADDLIST>\n" + 
"</PERSON>"; 
String profile_xml = " <PROFILE>\n" + 
" <Job>软件工程师</Job>\n" + 
" <tel>13512129933</tel>\n" + 
" <remark>备注说明</remark>\n" + 
" </PROFILE>"; 
String address_xml = " <ADDRESS Zipcode=\"710002\">\n" + 
" <Add>西安市雁塔路</Add>\n" + 
" </ADDRESS>"; 
//同样实用上面的XStream对象xStream 
System.out.println(xStream.fromXML(person_xml).toString()); 
System.out.println(xStream.fromXML(profile_xml).toString()); 
System.out.println(xStream.fromXML(address_xml).toString()); 
} 
public static void output(int i, XStream xStream, Object obj) { 
String xml = xStream.toXML(obj); 
System.out.println(">>>第[ " + i + "]次输出\n"); 
System.out.println(xml + "\n"); 
} 
} 
----------XStream学习:http://lavasoft.blog.51cto.com---------- 
>>>第[ 1]次输出 
<PERSON> 
<name>熔岩</name> 
<age>27</age> 
<profile> 
<job>软件工程师</job> 
<tel>13512129933</tel> 
<remark>备注说明</remark> 
</profile> 
<addlist> 
<ADDRESS> 
<add>郑州市经三路</add> 
<zipcode>450001</zipcode> 
</ADDRESS> 
<ADDRESS> 
<add>西安市雁塔路</add> 
<zipcode>710002</zipcode> 
</ADDRESS> 
</addlist> 
</PERSON> 
>>>第[ 2]次输出 
<PERSON> 
<Name>熔岩</Name> 
<age>27</age> 
<PROFILE> 
<Job>软件工程师</Job> 
<tel>13512129933</tel> 
<remark>备注说明</remark> 
</PROFILE> 
<ADDLIST> 
<ADDRESS> 
<Add>郑州市经三路</Add> 
<zipcode>450001</zipcode> 
</ADDRESS> 
<ADDRESS> 
<Add>西安市雁塔路</Add> 
<zipcode>710002</zipcode> 
</ADDRESS> 
</ADDLIST> 
</PERSON> 
>>>第[ 3]次输出 
<PERSON> 
<Name>熔岩</Name> 
<age>27</age> 
<PROFILE> 
<Job>软件工程师</Job> 
<tel>13512129933</tel> 
<remark>备注说明</remark> 
</PROFILE> 
<ADDLIST> 
<ADDRESS Zipcode="450001"> 
<Add>郑州市经三路</Add> 
</ADDRESS> 
<ADDRESS Zipcode="710002"> 
<Add>西安市雁塔路</Add> 
</ADDRESS> 
</ADDLIST> 
</PERSON> 
Person{name='熔岩', age='27', profile=Profile{job='软件工程师', tel='13512129933', remark='备注说明'}, addlist=[Address{add='郑州市经三路', zipcode='450001'}, Address{add='西安市雁塔路', zipcode='710002'}]} 
Profile{job='软件工程师', tel='13512129933', remark='备注说明'} 
Address{add='西安市雁塔路', zipcode='710002'} 
Process finished with exit code 0 
在实际中，类的属性很多，嵌套层次也很复杂，如果仅仅使用XStream原生API来硬编码设置别名等属性，显得太生硬也难以维护。完全可以考虑通过一个xml配置文件来定义所有用到的类的别名定义（包括其成员），然后，通过读取配置构建一个XStream的工厂，在用到时候直接去取，而不是让实用者组装。我目前的一个项目中，就是这么实现的，效果非常的好。 
下面我给出针对上面提出的问题一个解决方案： 
思想：考虑做一个过滤器，在xml转java之前，在Java转xml之后，应用这个过滤器。这个过滤器提供将xml中的“__”替换为“-”，并且将xml中的不需要的节点剔除。 
在过滤之前，我实现了个转换器装配，这一步通过xml来配置，并在java中获取。 
代码就省略了，这一步很灵活，关键看你的应用了。 
为了能过滤xml，我们需要用Dom4j递归遍历xml文档。下面一些算法代码： 
//递归算法：遍历配置文件，找出所有有效的xpath 
private static void recursiveElement(Element element) { 
List<Element> elements = element.elements(); 
validXPathList.add(element.getPath()); 
if (elements.size() == 0) { 
//没有子元素 
} else { 
//有子元素 
for (Iterator<Element> it = elements.iterator(); it.hasNext();) { 
//递归遍历 
recursiveElement(it.next()); 
} 
} 
} 
//递归算法：遍历xml，标识无效的元素节点 
private static void recursiveFixElement(Element element) { 
List<Element> elements = element.elements(); 
if (!validXPathList.contains(element.getPath())) { 
element.addAttribute("delete", "true"); 
} 
if (elements.size() == 0) { 
//没有子元素 
} else { 
//有子元素 
for (Iterator<Element> it = elements.iterator(); it.hasNext();) { 
Element e = it.next(); 
if (!validXPathList.contains(e.getPath())) { 
e.addAttribute("delete", "true"); 
} 
//递归遍历 
recursiveFixElement(e); 
} 
} 
} 
/** 
* 过滤器接口方法，转换不规范字符，剔除无效节点 
* 
* @param xmlStr 要过滤的xml 
* @return 符合转换器要求的xml 
*/ 
public static String filter(String xmlStr) { 
Document document = null; 
try { 
document = DocumentHelper.parseText(xmlStr.replaceAll("__", "_")); 
//递归的调用：标记要剔除的xml元素 
recursiveFixElement(document.getRootElement()); 
List<Node> nodeList = document.selectNodes("//@delete"); 
for (Node node : nodeList) { 
node.getParent().detach(); //剔除xml元素 
} 
} catch (DocumentException e) { 
System.out.println(e.getMessage()); 
e.printStackTrace(); 
} 
return document.asXML(); 
} 

-

**Tags：**[XStream](http://img.jb51.net/tag/XStream/1.htm "搜索关于XStream的文章") [实例](http://img.jb51.net/tag/%CA%B5%C0%FD/1.htm "搜索关于实例的文章")
[复制链接]( "复制本文链接发给你QQ/MSN上的好友")[收藏本文]()[打印本文]()[关闭本文]()[返回首页](http://www.jb51.net/index.htm) 

上一篇：[下载完成后页面不自动关闭的方法](http://www.jb51.net/article/13684.htm "下载完成后页面不自动关闭的方法")

下一篇：[java+sql2005 随机抽取试题的代码](http://www.jb51.net/article/27084.htm)
[](http://s.jb51.net/)

## 相关文章

* 
[java struts常见错误以及原因分析](http://www.jb51.net/article/16686.htm "java struts常见错误以及原因分析")
* 
[指南：想成为一个JSP网站程序员吗？](http://www.jb51.net/article/2657.htm "指南：想成为一个JSP网站程序员吗？")
* 
[实例讲解JSP Model2体系结构（上）](http://www.jb51.net/article/2544.htm "实例讲解JSP Model2体系结构（上）")
* 
[jsp JFreeChart使用心得与例子](http://www.jb51.net/article/16653.htm "jsp JFreeChart使用心得与例子")
* 
[JSP的内部对象](http://www.jb51.net/article/2665.htm "JSP的内部对象")
* 
[jsp Hibernate入门教程](http://www.jb51.net/article/16478.htm "jsp Hibernate入门教程")
* 
[使用JavaBean创建您的网上日历本(2)](http://www.jb51.net/article/2572.htm "使用JavaBean创建您的网上日历本(2)")
* 
[jsp中文乱码 jsp mysql 乱码的解决方法](http://www.jb51.net/article/13160.htm "jsp中文乱码 jsp mysql 乱码的解决方法")
**友情提醒**：本站文件的解压密码：[www.jb51.net](http://www.jb51.net) (请使用最新的winrar)[](http://www.tuidc.com/ "腾佑科技")

## 文章评论

共有 **0 ** 位脚本之家网友发表了评论[我来说两句](http://img.jb51.net/comment.asp?id=14542)

[](http://idc567.com/)  [](http://www.geisnic.com/)

### 最 近 更 新
* 
[为Java应用程序添加退出事件响应](http://www.jb51.net/article/2800.htm "为Java应用程序添加退出事件响应")
* 
[我认为JSP有问题(下)](http://www.jb51.net/article/2531.htm "我认为JSP有问题(下)")
* 
[jsp 页面上图片分行输出小技巧](http://www.jb51.net/article/17863.htm "jsp 页面上图片分行输出小技巧")
* 
[JSP教程(五)-JSP Actions的使用下](http://www.jb51.net/article/2527.htm "JSP教程(五)-JSP Actions的使用下")
* 
[也谈用JSP实现新郎、sohu新闻系统的技术。](http://www.jb51.net/article/2618.htm "也谈用JSP实现新郎、sohu新闻系统的技术。")
* 
[Java开源项目Hibernate](http://www.jb51.net/article/2770.htm "Java开源项目Hibernate")
* 
[一个可以防止刷新的JSP计数器](http://www.jb51.net/article/2470.htm "一个可以防止刷新的JSP计数器")
* 
[面向对象编程,我的思想(5)](http://www.jb51.net/article/2712.htm "面向对象编程,我的思想(5)")
* 
[JAVA/JSP学习系列之二(Tomcat安装)](http://www.jb51.net/article/2510.htm "JAVA/JSP学习系列之二(Tomcat安装)")
* 
[JDBC板块精华整理20051226](http://www.jb51.net/article/5296.htm "JDBC板块精华整理20051226")

### 热 点 排 行
* 
[FCKeditor使用方法(FCKeditor_2.](http://www.jb51.net/article/15792.htm "FCKeditor使用方法(FCKeditor_2.6.3)详细使用说明")
* 
[struts2+spring+hibernate分页代](http://www.jb51.net/article/15976.htm "struts2+spring+hibernate分页代码[比较多]")
* 
[jsp JFreeChart使用心得与例子](http://www.jb51.net/article/16653.htm "jsp JFreeChart使用心得与例子")
* 
[搭建EXTJS和STRUTS2框架(ext和st](http://www.jb51.net/article/21204.htm "搭建EXTJS和STRUTS2框架(ext和struts2简单实例)")
* 
[jsp生成静态页面的方法](http://www.jb51.net/article/6740.htm "jsp生成静态页面的方法")
* 
[Jsp页面实现文件上传下载类代码](http://www.jb51.net/article/13388.htm "Jsp页面实现文件上传下载类代码")
* 
[一个实用的JSP分页代码](http://www.jb51.net/article/15974.htm "一个实用的JSP分页代码")
* 
[JSP连接Access数据库](http://www.jb51.net/article/6742.htm "JSP连接Access数据库")
* 
[XStream使用方法总结附实例代码]( "XStream使用方法总结附实例代码")
* 
[response.setHeader参数、用法的](http://www.jb51.net/article/16437.htm "response.setHeader参数、用法的介绍")

### Js与CSS工具

* 
[CSS在线压缩格式化(中文)](http://www.jb51.net/tools/cssyasuo.shtml)
* 
[css 格式化整理工具(英文)](http://www.codebeautifier.com/)
* 
[CSS整形格式化](http://www.jb51.net/csstidy/css_optimiser.php?lang=zh)
* 
[JavaScript 格式化整理工具](http://tools.jb51.net/tools/js_geshihua.asp)
* 
[jsbeautifier Js格式化整理工具(英文)](http://jsbeautifier.org/)
* 
[php 格式化整理工具(英文)](http://www.phpformatter.com/)
* 
[HTML/JS互相转换工具](http://www.jb51.net/tools/html-js.htm)
* 
[javascript pack加密压缩工具](http://www.jb51.net/tools/packer.htm)
* 
[JS Minifier压缩](http://www.jb51.net/tools/jsmin/index.htm)
* 
[JS混淆工具](http://www.jb51.net/tools/JShunxiao.htm)
* 
[在线JS脚本校验器错误](http://www.jb51.net/tools/js_Debug.htm)
* 
[JavaScript 正则表达式在线测试工具](http://tools.jb51.net/tools/regex.asp)
### 代码转换工具

* 
[Base64编码加密](http://www.jb51.net/tools/base64.htm)
* 
[Escape加解密](http://tools.jb51.net//tools/Escape.asp)
* 
[HTML/UBB代码转换](http://tools.jb51.net/html_ubb.asp)
* 
[GB2312/BIG5繁简字转换](http://www.jb51.net/tools/fanjianzhi.htm)
* 
[经典小工具集 数字转换](http://www.jb51.net/tools/jingdian.shtml)
* 
[HTML多功能代码转换器](http://www.jb51.net/tools/htmlto.htm)
* 
[迅雷快车加/解密](http://www.jb51.net/tools/xunleijm.htm)
* 
[汉字转换拼音](http://tools.jb51.net/tools/pinyin.asp)
[](http://www.enkj.com/) [](http://www.tuidc.com/)

 

 

 

 
[](http://www.xunbiz.com/click/click.asp?str=44717c87b8d8fc3a3e18797345b90434)

-

* 
[CSS在线压缩格式化(中文)](http://www.jb51.net/tools/cssyasuo.shtml)
* 
[css 格式化整理工具(英文)](http://www.codebeautifier.com)
* 
[CSS整形格式化](http://www.jb51.net/csstidy/css_optimiser.php?lang=zh)
* 
[JavaScript 格式化整理工具](http://tools.jb51.net/tools/js_geshihua.asp)
* 
[jsbeautifier Js格式化整理工具(英文)](http://jsbeautifier.org)
* 
[php 格式化整理工具(英文)](http://www.phpformatter.com)
* 
[HTML/JS互相转换工具](http://www.jb51.net/tools/html-js.htm)
* 
[javascript pack加密压缩工具](http://www.jb51.net/tools/packer.htm)
* 
[JS Minifier压缩](http://www.jb51.net/tools/jsmin/index.htm)
* 
[JS混淆工具](http://www.jb51.net/tools/JShunxiao.htm)
* 
[在线JS脚本校验器错误](http://www.jb51.net/tools/js_Debug.htm)
* 
[JavaScript 正则表达式在线测试工具](http://tools.jb51.net/tools/regex.asp)

* 
[Base64编码加密](http://www.jb51.net/tools/base64.htm)
* 
[Escape加解密](http://tools.jb51.net//tools/Escape.asp)
* 
[HTML/UBB代码转换](http://tools.jb51.net/html_ubb.asp)
* 
[GB2312/BIG5繁简字转换](http://www.jb51.net/tools/fanjianzhi.htm)
* 
[经典小工具集 数字转换](http://www.jb51.net/tools/jingdian.shtml)
* 
[HTML多功能代码转换器](http://www.jb51.net/tools/htmlto.htm)
* 
[迅雷快车加/解密](http://www.jb51.net/tools/xunleijm.htm)
* 
[汉字转换拼音](http://tools.jb51.net/tools/pinyin.asp)

 
