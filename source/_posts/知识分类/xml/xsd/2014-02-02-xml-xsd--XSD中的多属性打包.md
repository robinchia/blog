---
layout: post
title: "XSD中的多属性打包"
categories: xml
tags: 
 - xml
 - xsd
--- 

# XSD中的多属性打包

定义XSD的时候，除了元素外还有属性；在使用XML时属性也占有相当大的比例，由此可见属性十分重要。例如，定义一本图书的XML元素有图书名称、出版社、作者、价格等。以作者为例，可以将其联系方式作为一种属性，如qq、msn、tel，如图1.18所示。
[![说明: http://images.51cto.com/files/uploadimg/20110728/103912698.jpg]()](http://images.51cto.com/files/uploadimg/20110728/103912698.jpg)  图1.18  XSD中的多属性打包

使用xs:attributegGroup定义一个属性组，设置name="att"（为组命名），然后在xs:attributeGroup内部定义属性组的成员，成员数量没有限制。代码如下：

1.  **<?xml** version="1.0" encoding="UTF-8"**?>** 

2.  <!--声明文档版本与字符编码方式--> 

3.  **<xs:schema** xmlns:xs="http://www.w3.org/2001/XMLSchema" 

4.  <!--声明文档的命名空间--> 

5.          targetNamespace=[http://www.mingrisoft.com](http://www.mingrisoft.com/) 
xmlns="http://www.mingrisoft.com" 

6.          elementFormDefault="qualified"**>** 

7.          **<xs:element** name="book"**>** 

8.              **<xs:complexType>** 

9.              <!--声明复杂类型--> 

10.                 **<xs:sequence>** 

11.                 <!--声明元素的排列顺序--> 

12.                     **<xs:element** name="name" type="xs:string" **/>** 

13.                     **<xs:element** name="publisher" type="xs:string" **/>** 

14.                     **<xs:element** name="company" type="xs:string" **/>** 

15.                     **<xs:element** name="author"**>** 

16.                         **<xs:complexType>** 

17.                             **<xs:simpleContent>** 

18.                                 **<xs:extension** base="xs:string"**>** 

19.                                     **<xs:attributeGroup** 
ref="att"**></xs:attributeGroup>** 

20.                                 **</xs:extension>** 

21.                             **</xs:simpleContent>** 

22.                         **</xs:complexType>** 

23.                     **</xs:element>** 

24.                     **<xs:element** name="ISBN" type="xs:string" **/>** 

25.                     **<xs:element** name="price" type="xs:double" **/>** 

26.                     **<xs:element** name="url" type="xs:string" **/>** 

27.                 **</xs:sequence>** 

28.             **</xs:complexType>** 

29.         **</xs:element>** 

30.         **<xs:attributeGroup** name="att"**>** 

31.         <!--定义一个属性组--> 

32.             **<xs:attribute** name="tel" type="xs:string" 
use="required"**></xs:attribute>** 

33.             **<xs:attribute** name="qq" type="
xs:integer"**></xs:attribute>** 

34.             **<xs:attribute** name="msn" type="
xs:string"**></xs:attribute>** 

35.         **</xs:attributeGroup>** 

36. **</xs:schema>** 

（1）建立XSD文档，声明文档根元素和命名空间。声明一个根节点book，添加复杂类型xs:cimplexType以及book下的子元素（name、publisher、company、ISBN、price和url），为各个子元素添加xs:string类型限定。

1.  **<?xml** version="1.0" encoding="UTF-8"**?>** 

2.  <!--声明文档版本与字符编码方式--> 

3.  **<xs:schema** xmlns:xs="http://www.w3.org/2001/XMLSchema" 

4.  <!--声明文档的命名空间--> 

5.          targetNamespace=[http://www.mingrisoft.com](http://www.mingrisoft.com/)
 xmlns="http://www.mingrisoft.com" 

6.          elementFormDefault="qualified"**>** 

7.          **<xs:element** name="book"**>** 

8.              **<xs:complexType>** 

9.                  **<xs:sequence>** 

10.                 <!--声明元素的排列顺序--> 

11.                     **<xs:element** name="name" type="xs:string" **/>** 

12.                     **<xs:element** name="publisher" type="xs:string" **/>** 

13.                     **<xs:element** name="company" type="xs:string" **/>** 

14.                     **<xs:element** name="ISBN" type="xs:string" **/>** 

15.                     **<xs:element** name="price" type="xs:double" **/>** 

16.                     **<xs:element** name="url" type="xs:string" **/>** 

17.                 **</xs:sequence>** 

18.             **</xs:complexType>** 

19.         **</xs:element>** 

20. **</xs:schema>** 

（2）在company下面添加一个元素author，然后在xs:simpleType上扩展xs:simpleType和xs:extension元素，在xs:extension上为author内容声明属性xs:string。

1.  **<xs:element** name="author"**>** 

2.              **<xs:complexType>** 

3.              <!--声明复杂类型--> 

4.                  **<xs:simpleContent>** 

5.                  **</xs:simpleContent>** 

6.              **</xs:complexType>** 

7.  **</xs:element>** 

（3）在xs:schema内部和book根节点平级的地方，声明一个xs:attributeGroup，设置其name="att"；在xs:attributeGroup内部声明3个属性，分别为tel、qq和msn，形成一个具有3个属性的属性组。

1.  **<xs:attributeGroup** name="att"**>** 

2.  <!--声明一个属性组--> 

3.      **<xs:attribute** name="tel" type="xs:string" 
use="required"**></xs:attribute>** 

4.      **<xs:attribute** name="qq" type="xs:integer"**></xs:attribute>** 

5.      **<xs:attribute** name="msn" type="xs:string"**></xs:attribute>** 

6.  **</xs:attributeGroup>** 

（4）在声明author 的地方，扩展xs:extension元素，在其内部声明一个xs:attributeGroup引用，引用刚才声明的属性组。

1.  **<xs:extension** base="xs:string"**>** 

2.              **<xs:attributeGroup** ref="att"**></xs:attributeGroup>** 

3.              <!--引入属性组--> 

4.  **</xs:extension>** 

心法领悟018：author属性组中的属性。

在本实例中为author添加了一个属性组，内部有3个属性tel、qq、msn，这样是为了更好地说明xs:attributeGroup的使用方式，但一般不建议这么定义XML，最好的方式是把tel、qq和msn定义成author的子元素。

 
