---
layout: post
title: "JDOM-XPATH编程指南"
categories: xml
tags: 
 - xml
 - jdom
--- 

# JDOM-XPATH编程指南

[![IBM®]()](http://www.ibm.com/cn/) ![]() [![跳转到主要内容]()](http://www.ibm.com/developerworks/cn/xml/x-jdom/#main)     **中国** [[选择](http://www.ibm.com/developerworks/cn/country/)]      [使用条款](http://www.ibm.com/legal/cn/)   ![]() ![]()   ![Select a scope:]() dW 全部内容 -----------------   AIX and UNIX   Information management   Lotus   Rational   WebSphere -----------------   Architecture   Grid computing   Java 技术   Linux   Multicore acceleration   Open source   Security   SOA & Web services   Web development   XML ----------------- IBM 全部内容 ![Search for:]()      []()        [首页](http://www.ibm.com/cn/)      [产品](http://www.ibm.com/products/cn/)      [服务与解决方案](http://www.ibm.com/servicessolutions/cn/)      [支持与下载](http://www.ibm.com/support/cn/)      [个性化服务](http://www.ibm.com/account/cn/)      [ ](http://www.ibm.com/developerworks/cn/) [developerWorks
中国](http://www.ibm.com/developerworks/cn/) [本文内容包括：](http://www.ibm.com/developerworks/cn/xml/x-jdom/#) ![]() [前言](http://www.ibm.com/developerworks/cn/xml/x-jdom/#0) ![]() [XPATH速成篇](http://www.ibm.com/developerworks/cn/xml/x-jdom/#1) ![]() [JDOM修炼篇](http://www.ibm.com/developerworks/cn/xml/x-jdom/#2) ![]() [获得并安装JDOM](http://www.ibm.com/developerworks/cn/xml/x-jdom/#3) ![]() [用JDOM解析XML](http://www.ibm.com/developerworks/cn/xml/x-jdom/#4) ![]() [JDOM+XPATH进阶篇](http://www.ibm.com/developerworks/cn/xml/x-jdom/#5) ![]() [结语](http://www.ibm.com/developerworks/cn/xml/x-jdom/#6) ![]() [参考资料](http://www.ibm.com/developerworks/cn/xml/x-jdom/#resources) ![]() [关于作者](http://www.ibm.com/developerworks/cn/xml/x-jdom/#author) ![]() [对本文的评价](http://www.ibm.com/developerworks/cn/xml/x-jdom/#rate) ![]() ![]()
**相关链接：** ![]() [XML 技术文档库](http://www.ibm.com/developerworks/cn/views/xml/libraryview.jsp) ![]() ![]() [![跳转到主要内容]()]() ![]() ![]()
[developerWorks 中国](http://www.ibm.com/developerworks/cn/)  >  [XML](http://www.ibm.com/developerworks/cn/xml/)  >![]()

# JDOM/XPATH编程指南

![]() ![developerWorks]() ![]() ![]() 文档选项 ![]() ![]() 
未显示需要 JavaScript 的文档选项

级别： 初级

[薛谷雨](http://www.ibm.com/developerworks/cn/xml/x-jdom/#author) ([mailto:rainight@126.com?subject=JDOM/XPATH编程指南](mailto:rainight@126.com?subject=JDOM/XPATH编程指南)), 高级JAVA工程师, NORDSAN信息科技开发有限公司

2004 年 5 月 01 日
本文分别介绍了 JDOM 和 XPATH，以及结合两者进行 XML 编程带来的好处。

[前言]()

XML是一种优秀的数据打包和数据交换的形式，在当今XML大行于天下，如果没有听说过它的大名，那可真是孤陋寡闻了。用XML描述数据的优势显而易见，它具有结构简单，便于人和机器阅读的双重功效，并弥补了关系型数据对客观世界中真实数据描述能力的不足。W3C组织根据技术领域的需要，制定出了XML的格式规范，并相应的建立了描述模型，简称DOM。各种流行的程序设计语言都纷纷根据这一模型推出了自己的XML解析器，在JAVA世界里，APACHE组织开发的XERCES应该是流行最广功能最为强大的XML解析器之一。但是由于W3C在设计DOM模型时，并不是针对某一种语言而设计，因此为了通用性，加入了许多繁琐而不必要的细节 ，使JAVA程序员在开发XML的应用程序过程中感到不甚方便，因此JDOM作为一种新型的XML解析器横空出世，它不遵循DOM模型，建立了自己独立的一套JDOM模型(注意JDOM决不是DOM扩展，虽然名字差不多，但两者是平行的关系)，并提供功能强大使用方便的类库，使JAVA程序员可以更为高效的开发自己的XML应用程序，并极大的减少了代码量，因此它很快得到了业内的认可，如JBUILDER这样的航空母舰级的重磅产品都以JDOM为XML解析引擎，足见其名不虚传。

有了XML数据的描述标准，人们自然就会想到应该有一种查询语言可以在XML中查找任意节点的数据，就像SQL语句可以在关系性数据库中执行查询操作一样，于是XQUERY和XPATH顺应潮流，应运而生。由于XQUERY较为复杂，使用不甚方便，XPATH渐渐成为主流，我们只需对XPATH进行学习，便可以应付所有的查询要求。在JDOM发布的最新的V1.0bata10版中，已经加入了对XPATH的支持，这无疑是令开发者十分激动的。

学会JDOM和XPATH，你便不再是XML的入门者，在未来的开发生涯中，就像特种兵的多用匕首，为你披荆斩棘，助你勇往直前。闲言少叙，学习还要脚踏实地，从头开始。
![]()
![]() ![]()
![]()
 [**回页首**](http://www.ibm.com/developerworks/cn/xml/x-jdom/#main)

[XPATH速成篇]()

XPATH遵循文档对象模型(DOM)的路径格式，由于每个XML文档都可以看成是一棵拥有许多结点的树，每个结点可以是以下七个类型之一：根（root）、元素（element）、属性（attribute）、正文（text）、命名空间（namespace）、处理指令（processing instruction）和注释（comment）。XPATH的基本语法由表达式构成。在计算表达式的值之后产生一个对象，这种对象有以下四种基本类型：节点集合、布尔型、数字型和字符串型 。XPATH基本上和在文件系统中寻找文件类似，如果路径是以"/"开头的，就表明该路径表示的是一个绝对路径，这和在UNIX系统中关于文件路径的定义是一致的。以"//"开头则表示在文档中的任意位置查找。

不谈泛泛的理论，学习XPATH还要从实例学起最为快捷，并有助于你举一反三。

下面的样例XML文档，描述了某台电脑中硬盘的基本信息(根节点<HD>代表硬盘，<disk>标签代表硬盘分区，从它的name属性可以看出有两个盘符名称为"C"和"D"的分区；每个分区下都包含<capacity>,<directories><files>三个节点，分别代表了分区的空间大小、目录数量、所含文件个数):
<?xml version="1.0" encoding="UTF-8"?> <HD> <disk name="C"> <capacity>8G</capacity> <directories>200</directories> <files>1580</files> </disk> <disk name="D"> <capacity>10G</capacity> <directories>500</directories> <files>3000</files> </disk> </HD>

你在XML文档中使用位置路径表达式来查找信息，这些表达式有很多种组成方式。

结点元素的查找是你将要碰到的最频繁的查找方式。在上面这个XML文档例子中，根HD包含disk结点。你可以使用路径来查找这些结点，用正斜杠（/）来分隔子结点，返回所有与模式相匹配的元素。下面的XPATH 语句返回所有的disk元素：

/HD/disk

"*"代表"全部"的意思。/HD/* 代表HD下的全部节点。

下面的XPATH将返回任意节点下的名称为disk的全部节点：

//disk

下面的XPATH将返回名称为disk，name属性为'C'的全部节点：

/HD/disk[@name='C']

节点的附加元素，比如属性，函数等都要用方括号扩起来，属性前面要加上@号

下面的XPATH将返回文件个数为1580的files节点：

/HD/disk/files[text()='1580']

大家注意到上面包含一个text()，这就是XPATH的一个函数，它的功能是取出当前节点的文本。

下面的XPATH将返回文件个数为1580的分区：

/HD/disk/files[text()='1580']/parent::*

最后的parent::*表示这个元素的所有的父节点的集合。

XPATH中一些有用的函数：
string **concat** (string, string, string*) 联接两个字符串 boolean **starts-with** (string, string) 判断某字符串是否以另一字符串开头 boolean **contains** (string, string) 判断某字符串是否包含另一字符串 string **substring** (string, number, number) 取子字符串 number **string-length** (string) 测字符串长度 number **sum** (node-set) 求和 number **floor** (number) 求小于此数的最大整数值 number **ceiling** (number) 求大于此数最小整数值

XPATH具有丰富的表达功能，上面这些已经基本够用，在你做项目中就会发现根据实际情况有许多查询需求，你应该参考本文最后提供的W3C发布的关于XAPH的官方资料进行查阅，我在这里只起一个抛砖引玉的作用，在下面的章节中，我们的应用范例将不会超出上面提到的这些内容，如果你对XPATH感兴趣，应该在读完本文后，查找相关资料和书籍进行深入学习。

![]()
![]() ![]()
![]()
 [**回页首**](http://www.ibm.com/developerworks/cn/xml/x-jdom/#main)

[JDOM修炼篇]()

用过XERCES的程序员都会感到，有时候用一句话就可以说清楚的事，当用XERCES的API来实现时，要三四行程序。
![]()
![]() ![]()
![]()
 [**回页首**](http://www.ibm.com/developerworks/cn/xml/x-jdom/#main)

[获得并安装JDOM]()

在 [http://www.jdom.org/](http://www.jdom.org/)可以下载JDOM的最新版本，将压缩包中的jdom.jar及lib目录下的全部jar包加入到classpath就可以了。
![]()
![]() ![]()
![]()
 [**回页首**](http://www.ibm.com/developerworks/cn/xml/x-jdom/#main)

[用JDOM解析XML]()

JDOM模型的全部类都在org.jdom.*这个包里，org.jdom.input.*这个包里包含了JDOM的解析器，其中的DOMBuilder的功能是将DOM模型的Document解析成JDOM模型的Document；SAXBuilder的功能是从文件或流中解析出符合JDOM模型的XML树。由于我们的上面提到的XML样例存储在一个名称为sample.xml的文件中，很显然我们应该采用后者作为解析工具。下面程序演示了jdom的基本功能，即解析一个xml文档，并挑选一些内容输出到屏幕上。
import java.util.*; import org.jdom.*; import org.jdom.input.SAXBuilder; public class Sample1 { public static void main(String[] args) throws Exception{ SAXBuilder sb=new SAXBuilder(); Document doc=sb.build("sample.xml"); Element root=doc.getRootElement(); List list=root.getChildren("disk"); for(int i=0;i<list.size();i++){ Element element=(Element)list.get(i); String name=element.getAttributeValue("name"); String capacity=element.getChildText("capacity"); String directories=element.getChildText("directories"); String files=element.getChildText("files"); System.out.println("磁盘信息:"); System.out.println("分区盘符:"+name); System.out.println("分区容量:"+capacity); System.out.println("目录数:"+directories); System.out.println("文件数:"+files); System.out.println("-----------------------------------"); } } }

程序的输出结果：
磁盘信息: 分区盘符:C 分区容量:8G 目录数:200 文件数:1580 ----------------------------------- 磁盘信息: 分区盘符:D 分区容量:10G 目录数:500 文件数:3000 -----------------------------------

这段程序采用了传统的解析方式，一级一级的从根节点到子节点逐个采集我们所需要的数据，中规中矩。试想如果这个树足够深，我们想取第5 0层第三个节点的数据（夸张了点，呵呵），那将是一场噩梦！下面的内容将轻松化解你的这一痛苦。
![]()
![]() ![]()
![]()
 [**回页首**](http://www.ibm.com/developerworks/cn/xml/x-jdom/#main)

[JDOM+XPATH进阶篇]()

说了那么多JDOM和XPATH的好处，终于到了英雄有用武之地的时候了。

JDOM的关于XPATH的api在org.jdom.xpath这个包里。看看这个包下，只有一个类，JDOM就是如此简洁，什么事都不故弄玄虚的搞得那么复杂。这个类中的核心的api主要是两个selectNodes()和selectSingleNode()。前者根据一个xpath语句返回一组节点；后者根据一个xpath语句返回符合条件的第一个节点。

下面的程序我们用JDOM+XPATH实现了上一个程序同样的功能，你可以从中学到不少运用XPATH 的知识：
import java.util.*; import org.jdom.*; import org.jdom.input.SAXBuilder; import org.jdom.xpath.XPath; public class Sample2 { public static void main(String[] args) throws Exception { SAXBuilder sb = new SAXBuilder(); Document doc = sb.build("sample.xml"); Element root = doc.getRootElement(); List list = XPath.selectNodes(root, "/HD/disk"); for (int i = 0; i > list.size(); i++) { Element disk_element = (Element) list.get(i); String name = disk_element.getAttributeValue("name"); String capacity = ( (Text) XPath.selectSingleNode(disk_element, "//disk[@name='" + name + "']/capacity/text()")).getTextNormalize(); String directories = ( (Text) XPath.selectSingleNode(disk_element, "//disk[@name='" + name + "']/directories/text()")).getTextNormalize(); String files = ( (Text) XPath.selectSingleNode(disk_element, "//disk[@name='" + name + "']/files/text()")).getTextNormalize(); System.out.println("磁盘信息:"); System.out.println("分区盘符:" + name); System.out.println("分区容量:" + capacity); System.out.println("目录数:" + directories); System.out.println("文件数:" + files); System.out.println("-----------------------------------"); } } }

输出结果：
磁盘信息: 分区盘符:C 分区容量:8G 目录数:200 文件数:1580 ----------------------------------- 磁盘信息: 分区盘符:D 分区容量:10G 目录数:500 文件数:3000 -----------------------------------
![]()
![]() ![]()
![]()
 [**回页首**](http://www.ibm.com/developerworks/cn/xml/x-jdom/#main)

[结语]()

技术在日新月异的发展。永远没有学过后，便可以一劳永逸的技术。XML的发展一日千里。W3C作为INTERNET方面的权威组织指导着互联网技术的发展方向。新技术的出现大都围绕着W3C制订的标准，但往往有些“旁门左道”的另类功法却能产生惊人的杀伤力。JDOM就是这众多旁门中的一朵奇葩。就像J2EE大行其道的今天，有许多开源组织仍旧在默默的打造着自己的独家兵器，谁又能说在不久的将来，他们不会成为划时代的创造呢? 君不见Hibernate的兴起正在有力的震撼着J2EE中EJB架构的基石。只要是成型的框架，必然有薄弱的软肋。新的技术只要能攻入对方这一弱点，便可在业界站一席之地。本文只起抛砖引玉的作用，相信读者在吃过这道快餐之后，一定会发现窗外有更美丽的风景等待我们去游历。

[参考资料]()

* W3C发布的关于XPATH的权威文档请访问 [http://www.w3.org/TR/2002/WD-DOM-Level-3-XPath-20020328](http://www.w3.org/TR/2002/WD-DOM-Level-3-XPath-20020328)
* JDOM官方网站可以下载最新JDOM类库 [http://www.jdom.org/](http://www.jdom.org/)

[关于作者]()
![]() ![]() 
薛谷雨是NORDSAN(北京)信息科技开发有限公司高级JAVA研发工程师，正致力于企业级异构数据交换的服务器产品的研发，在J2EE和WEB SERVICE方面有较为丰富的开发经验，你可以通过 [mailto:rainight@126.com?cc=](mailto:rainight@126.com?cc=)与他取得联系。

[对本文的评价]()
![]() 太差！ (1) 需提高 (2) 一般；尚可 (3) 好文章 (4) 真棒！(5)
**建议？**
  ![]()

![]()
![]()
 [**回页首**](http://www.ibm.com/developerworks/cn/xml/x-jdom/#main)
 ![]()IBM 公司保留在 developerWorks 网站上发表的内容的著作权。未经IBM公司或原始作者的书面明确许可，请勿转载。如果您希望转载，请通过 [提交转载请求表单](https://www.ibm.com/developerworks/secure/reprintreq.jsp?domain=dwchina) 联系我们的编辑团队。     [关于 IBM](http://www.ibm.com/cn/ibm/index.shtml)      [隐私条约](http://www.ibm.com/cn/ibm/privacy/index.shtml)      [联系 IBM](http://www.ibm.com/contact/cn/)      [使用条款](http://www.ibm.com/legal/cn/zh/)  ![]()
