---
layout: post
title: "XSD文件知识汇总"
categories: xml
tags: 
 - xml
 - xsd
--- 

# XSD文件知识汇总

[什么是XML Schema]()

XML Schema如同DTD一样是负责定义和描述XML文档的结构和内容模式。它可以定义XML文档中存在哪些元素和元素之间的关系，并且可以定义元素和属性的数据类型。

XML Schema本身是一个XML文档，它符合XML语法结构。可以用通用的XML解析器解析它。
![]()
![]() ![]()
[****](http://www.ibm.com/developerworks/cn/xml/x-cert/part6/index.html#main)

[为什么要使用Schema]()

我们前面已经使用DTD来定义一个XML的结构和数据类型,那为什么还要Schema呢?

因DTD有着不少缺陷：

1) DTD是基于正则表达式的，描述能力有限； 
2) DTD没有数据类型的支持，在大多数应用环境下能力不足； 
3) DTD的约束定义能力不足，无法对XML实例文档作出更细致的语义限制； 
4) DTD的结构不够结构化，重用的代价相对较高； 
5) DTD并非使用XML作为描述手段，而DTD的构建和访问并没有标准的编程接口，无法使用标准的编程方式进行DTD维护。

而XML Schema正是针对这些DTD的缺点而设计的，XML Schema的优点:

1) XML Schema基于XML,没有专门的语法 
2) XML可以象其他XML文件一样解析和处理 
3) XML Schema支持一系列的数据类型(int、float、Boolean、date等) 
4) XML Schema提供可扩充的数据模型。 
5) XML Schema支持综合命名空间 
6) XML Schema支持属性组。
![]()
![]() ![]()
[****](http://www.ibm.com/developerworks/cn/xml/x-cert/part6/index.html#main)

[一个简单的XML Schema文档]()
![]() 

在这个Schema里面定义了一个元素:quantity,它的类型是nonNegativeInteger(非负整数),xmlns是Schema的命名空间，这在前面第3部分已经叙述过了。

下面的XML片段是合法的:
<quantity>5</quantity>

下面的XML片段是非法的:
<quantity>-4</quantiy> ![]()
[****](http://www.ibm.com/developerworks/cn/xml/x-cert/part6/index.html#main)

[Schema中的类型]()

Schema中主要包括三种部件：元素(element)、属性(attribute)、注释(notation)。

这三种基本的部件还能组合成以下的部件：

a)类型定义部件： 简单类型和复合类型 
b)组部件 
c)属性组部件

[简单类型]()
![]() 

XML Schema中定义了一些内建的数据类型，这些类型可以用来描述元素的内容和属性值。

一个元素中如果仅仅包含数字、字符串或其他数据，但不包括子元素，这种被称为简单类型。

如同图中元素quantity就是一个简单类型。它的元素内容必须是非负整数，不包括任何属性和子元素。
<quantity>some</quantity> ![]()
[****](http://www.ibm.com/developerworks/cn/xml/x-cert/part6/index.html#main)

[所有内建的简单类型]()

原始类型
string,boolean,decimal,float,double,duration

datetime,time,date,gYearMonth,gYear,gMonthDay,
dDay,gMonth,hexBinary,base64Binary,any URI,QName

NOTATION

衍生类型(括号中为基类型)
normalizedString(string),language(tonken),token(normalizedString)

NMTOKEN(token),Name(token),NCName(Name),ID(NCName),IDREF(NCName)
IDREFS(list of IDREF),ENTITY(NCName),ENTITIES(list of ENTITY)

integer(decimal),nonPositiveInteger(integer),
negativeInteger(noPositiveInteger),long(integer),int(long),

short(int),byte(short),nonNegativeInteger(integer)
unsignedLong(nonNegativeInteger),unsignedInt(unsignedLong),

unsignedShort(unsignedInt),unsignedByte(unsignedShort),
positiveInteger(nonNegativeInteger) ![]()
[****](http://www.ibm.com/developerworks/cn/xml/x-cert/part6/index.html#main)

[创建简单类型]()

![]() 

图中我们先创建了一个简单类型:quantityType，它是从integer继承过来的，minInclusive和maxInclusive定义了它的最小值2和最大值5。最后我们定义元素quantity的类型为quantityType。
正确：    <quantity>3</quantity>

错误：    <quantity>10</quantity>
<qauntity>aaa</quantity>

使用restriction我们可以限制只能接受一定数值或者只能接受一定文字，
基本方面：equal,ordered,bounded,cardinality,numeric

限制方面：length,minLength,maxLength
pattern,enumeration

whiteSpace
maxInclusive,maxExclusive,minInclusive,minExclusive

totalDigits,fractionDigits ![]()
[****](http://www.ibm.com/developerworks/cn/xml/x-cert/part6/index.html#main)

[简单类型的例子 1]()
![]() 

这个SKU的类型的取值：3个数字后面根着一个连字号接着跟着两个大写的英文字母。

pattern后面跟的是正则表达式。有关正则表达式的语法请参阅其他书籍。
正确:    <ourSKU>123-AB</ourSKU>

错误:    <ourSKU>abc-AB</ourSKU>
<ourSKU>123-ab</ourSKU>

[简单类型的例子 2]()
![]() 

这是一个用来描述美国州名的类型USState，通过enumeration来列出所有州名，取值时就只能取里面列出的州名。

<!-- and so on ...-> 这是一个注释语句。
正确：    <statename>AK</statename>

错误：    <statename>Alaska</statename>

[列表类型]()
![]() 

list可以用来定义列表类型，listOfIntType这个类型被定义为一个Integer的列表，元素listOfMyInt的值可以几个整数，他们之间用空格隔开。
正确： <listOfMyInt>1 5 15037 95977 95945</listOfMyInt>

错误： <listOfMyInt>1 3 abc</listOfMyInt>

[联合类型]()
![]() 

图中用union来定义了一个联合类型，里面的成员类型包括USState和listOfMyIntType,应用了联合类型的元素的值可以是这些原子类型或列表类型中的一个类型的实例，但是一个元素实例不能同时包含两个类型。
正确：    <zips>CA</zips>

<zips>95630 95977 95945</zips>
<zips>AK</zips>

错误：    <zips>CA 95630</zips>
[**匿名类型定义**]()
![]() 

前面我们在定义元素类型时总是先定义一个数据类型，然后再把元素的type设成新定义的数据类型。如果这个新的数据类型只会用一次，我们就可以直接设置在元素定义里面，而不用另外来设置。如图中元素quantity的类型就是一个从1到99的整数。

这种新的类型没有自己的名字的定义方法我们称之为匿名类型定义。

[复合类型]()
![]() 

前面我们所讲到的都是属于简单类型，即元素里面只有内容，不再包括属性或者其它元素。接下来我们要让元素里面包含属性和其它元素，称之为复合类型。

图中我们用complexType表示这是一个复合类型（这里我们是用匿名类型定义的）。simpleContent表示这个元素下面不包括子元素，extension表示这个元素值是decimal的，attribute来设置它的一个属性currency，类型为string.
正确：<internationalPrice currency="EUR">423.46</internationalPrice>
![]()
![]() ![]()
[****](http://www.ibm.com/developerworks/cn/xml/x-cert/part6/index.html#main)

[混合内容]()
![]() 

同样，我们采用了匿名类型方式来定义一个元素salutation。我们注意到在complexType后面多了一个mixed="true"，这表明这是一个混合类型：里面既有元素本身的内容，又有其它子元素。name元素就是salutation的子元素。
正确：    <salutation>Dear Mr.<name>Robert Smith</name>.</salutation>

错误：    <salutation>Dear Mr.</salutation>

sequence表示子元素出现的顺序要和schema里面的顺序一样。我们在后面还会讲到和sequence对应的choice和all两种方式。

[空内容]()
![]() 

有的时候元素根本没有内容，他的内容模型是空。为了定义内容是空的类型，我们可以通过这样的方式：首先我们定义一个元素，它只能包含子元素而不能包含元素内容，然后我们又不定义任何子元素，依靠这样的方式，我们就能够定义出内容模型为空的元素。

图中complexConet表示只包含子元素，然后我们定义了两个属性currency和value，但是却不定义任何子元素。
正确：

<internationalPrice currency="EUR" value="/423.46"/>
错误：

<internationalPrice currency="EUR" value="/423.46">
Here is a mistake!

</interanationPrice>

还要更简洁的方法定义：
<xsd:element name="internationalPrice">

<xsd:complexType>
<xsd:attribute name="currency" type="xsd:string"/>

<xsd:attribute name="value" type="xsd:decimal"/>
</xsd:complexType>

</xsd:element>

因为一个不带有simpleContent 或者complexContent的复合类型定义，会被解释为带有类型定义为anyType的complexContent，这是一个默认的速记方法，所以这个简洁的语法可以在模式处理器中工作。

[anyType]()
![]() 

一个anyType类型不以任何形式约束其包含的内容。我们可以象使用其他类型一样使用anyType，如图第一个语句，这个方式声明的元素是不受约束的。所以元素的值可以为423.46，也可以为任何其他的字符序列，或者甚至是字符和元素的混合。实际上，anyType是默认类型，所以上面的可以被重写为第二个语句。

如果需要表示不受约束的元素内容，举例来说在元素包含散文，其中可能需要嵌入标签来支持国际化的表示，那么默认的声明(无约束)或者有些微约束的形式会很合适。

[注释]()
![]() 

为了方便其他读者和应用来理解模式文档，XML Schema提供了三个元素用来注释。
annotation

documentation
appinfo

图中，我们在documentation元素中放置了一个基本的模式描述和版权信息，这是放置适合人阅读的信息的推荐位置。我们推荐你在任何的documentation元素中使用xml:lang属性来表示这些描述信息使用的语言。
![]()
[****](http://www.ibm.com/developerworks/cn/xml/x-cert/part6/index.html#main)

[构造内容模型]()
![]() 

图中，我们在purchaseOrderType定义中引入两个元素组定义,购买订单就可以有两种选择来描述地址：第一种是包含彼此独立的送货地址和收款地址，第二种情况则是仅包含一个简单的地址，这个地址即是送货地址也是收款地址.

对于choice组元素而言，在实例中仅仅允许出现这个组中的一个子内容。对于图中的例子而言，第一个子内容是一个内部group元素，引用以shipAndBill命名的元素组，这个元素组由元素序列shipTo、billTo组成。第二个子内容为singleUSAddress。因此，在一个实例文档中，purchaseOrder元素必须，要么包含一个billTo元素和一个shipTo元素，要么包含一个singleUSAddress元素。

choice组后面跟着的是comment和items元素声明。元素和组的声明都是sequence 组的子内容。这样定义的效果是comment和items元素必须按顺序跟在地址元素后面。

在内容模型中被命名或未被命名的元素组(分别由group、choice、sequence、all所表现)可以带有minOccurs 和maxOccurs属性

[属性组]()
![]() 

我们可以建立一个被命名的属性组来包含所有item元素所期望的属性，并且在item元素声明中通过名字来引用这个属性组ItemDeleivery

通过这种方法来使用属性组，可以提高模式文档的可读性，同时也便于更新模式文档。这是因为一个属性组能够在一个地方定义和编辑，同时能够在多个定义和声明中被引用。注意到一个属性组可以包含其他属性组，同时还要注意到属性组的声明和引用必须在复合类型定义的最后。

[空值(Nil)]()
![]() 

XML Schema 空值机制包括一个空值信号。换句话说，作为元素内容而言，并没有没有真正的空值，代之的是一个说明元素的内容是空值的属性。为了显示这点，我们修改shipDate元素的声明，这样空值就能够被明确地告知用户了。
<xsd:element name="shipDate" type="xsd:date" nillable="true"/>

为了在实例文档中明确的表示shipDate有一个空值，我们可以设置nil属性为真：
<shipDate xsi:nil="true"></shipDate>

**Xml Schema****的用途**

1．  定义一个Xml文档中都有什么元素

2．  定义一个Xml文档中都会有什么属性

3．  定义某个节点的都有什么样的子节点，可以有多少个子节点，子节点出现的顺序

4．  定义元素或者属性的数据类型

5．  定义元素或者属性的默认值或者固定值

**Xml Schema****的根元素：**

<?xml version="1.0"?>

<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" 表示数据类型等定义来自w3

targetNamespace="http://www.w3schools.com" 表示文档中要定义的元素来自什么命名空间

xmlns="http://www.w3schools.com"表示此文档的默认命名空间是什么

elementFormDefault="qualified"> 表示要求xml文档的每一个元素都要有命名空间指定

……定义主体部分……

</xs:schema>

**如何定义一个简单元素**

<xs:element  此处表示要定义一个元素

name=”color” 表示要定义元素的名称

type=”xs:string”  表示要定义元素的数据类型

default=”red” 表示定义元素的默认值

fixed=”red”/> 表示要定义元素的固定值，此元素只可以取“red”值

以上定义了一个简单元素，元素实例：<color>red</color>

**如何定义一个属性**

<xs:attribute

         name=”birthday” 表示要定义属性的名字

         type=”xs:date” 表示要定义属性的数据类型

         default=”2001-01-11” 表示要定义属性的默认值

         fixed=”2001-01-11” 表示要定义属性的固定值

         use=”required”/> 表示此属性是否是必须指定的，即如果不指定就不符合Schema，默认没有use=”required”属性表示属性可有可无

**如何定义元素或者属性值的限制**

1．最大值最小值限制

<xs:element name="age">

<xs:simpleType>

<xs:restriction base="xs:integer">

<xs:minInclusive value="0"/> 大于等于0，<xs: minExclusive>表示最小值但是不包括指定值

 <xs:maxInclusive value="120"/> 小于等于120，<xs: maxExclusive>

</xs:restriction>

</xs:simpleType>

</xs:element>

2．枚举限制，指只能在指定的几个值中取值

         <xs:element name="car" type="carType"/>

<xs:simpleType name="carType">

  <xs:restriction base="xs:string">

    <xs:enumeration value="Audi"/>

    <xs:enumeration value="Golf"/>

    <xs:enumeration value="BMW"/>

  </xs:restriction>

</xs:simpleType>

3．模式（pattern）限制 ，指字符串的格式必须满足制定的匹配模式
例子
 
说明 <xs:element name="letter">

<xs:simpleType>
  <xs:restriction base="xs:string">

    <xs:pattern value="[a-z]"/>
  </xs:restriction>

</xs:simpleType>

</xs:element>
 
表示只能在小写字母中取一个值 <xs:element name="initials">

<xs:simpleType>
  <xs:restriction base="xs:string">

    <xs:pattern value="[A-Z][A-Z][A-Z]"/>
  </xs:restriction>

</xs:simpleType>
</xs:element> 
表示必须是三个大写字母 <xs:element name="initials">

<xs:simpleType>
  <xs:restriction base="xs:string">

    <xs:pattern value="[a-zA-Z][a-zA-Z][a-zA-Z]"/>
  </xs:restriction>

</xs:simpleType>
</xs:element> 
表示必须是三个字母，可以是大写或小写的 <xs:element name="choice">

<xs:simpleType>
  <xs:restriction base="xs:string">

    <xs:pattern value="[xyz]"/>
  </xs:restriction>

</xs:simpleType>

</xs:element>
 
表示必须是xyz中的一个 <xs:element name="prodid">

<xs:simpleType>
  <xs:restriction base="xs:integer">

    <xs:pattern value="[0-9][0-9][0-9][0-9][0-9]"/>
  </xs:restriction>

</xs:simpleType>
</xs:element> 
表示数字的范围是0-99999 <xs:element name="letter">

<xs:simpleType>
  <xs:restriction base="xs:string">

    <xs:pattern value="([a-z])*"/>
  </xs:restriction>

</xs:simpleType>
</xs:element> 
表示必须是0或者多个小写字符组成的序列 <xs:element name="letter">

<xs:simpleType>
  <xs:restriction base="xs:string">

    <xs:pattern value="([a-z][A-Z])+"/>
  </xs:restriction>

</xs:simpleType>
</xs:element> 
表示必须是多个字母。 <xs:element name="gender">

<xs:simpleType>
  <xs:restriction base="xs:string">

    <xs:pattern value="male|female"/>
  </xs:restriction>

</xs:simpleType>
</xs:element> 
表示是male或者female中的一个 <xs:element name="password">

<xs:simpleType>
  <xs:restriction base="xs:string">

    <xs:pattern value="[a-zA-Z0-9]{8}"/>
  </xs:restriction>

</xs:simpleType>
</xs:element> 
表示必须是8个字母数字字符

4．字符串长度的限制

<xs:element name="password">

<xs:simpleType>

  <xs:restriction base="xs:string">

    <xs:length value="8"/>

  </xs:restriction>

</xs:simpleType>

</xs:element>

长度必须是8。

<xs:element name="password">

<xs:simpleType>

  <xs:restriction base="xs:string">

    <xs:minLength value="5"/>

    <xs:maxLength value="8"/>

  </xs:restriction>

</xs:simpleType>

</xs:element>

表示长度在5-8之间

6． 对于空白字符的限制
示例
 
说明 <xs:element name="address">

<xs:simpleType>
  <xs:restriction base="xs:string">

    <xs:whiteSpace value="preserve"/>
  </xs:restriction>

</xs:simpleType>

</xs:element>
 
保留原样，表示xml处理器不会移除或者替换任何空白字符 <xs:element name="address">

<xs:simpleType>
  <xs:restriction base="xs:string">

    <xs:whiteSpace value="replace"/>
  </xs:restriction>

</xs:simpleType>
</xs:element> 
指回车，换行，Tab都会被替换成空格处理 <xs:element name="address">

<xs:simpleType>
  <xs:restriction base="xs:string">

    <xs:whiteSpace value="collapse"/>
  </xs:restriction>

</xs:simpleType>
</xs:element> 
去掉多于一个空格，和html中处理方式相同

 

**如何定义复杂类型******

复杂类型是指定义元素中包含属性或者子元素的类型

1． 定义只包含子元素的复杂类型

<xs:element name="person">

  <xs:complexType>

    <xs:sequence>

      <xs:element name="firstname" type="xs:string"/>

      <xs:element name="lastname" type="xs:string"/>

    </xs:sequence>

  </xs:complexType>

</xs:element>

2． 定义只包含属性的复杂类型

<xs:element name="product" type="prodtype"/>

<xs:complexType name="prodtype">

  <xs:attribute name="prodid" type="xs:positiveInteger"/>

</xs:complexType>

3． 定义只包含内容的复杂类型

<xs:element name="shoesize" type="shoetype"/>

<xs:complexType name="shoetype">

  <xs:simpleContent>

    <xs:extension base="xs:integer">

      <xs:attribute name="country" type="xs:string" />

    </xs:extension>

  </xs:simpleContent>

</xs:complexType>

4． 定义包含内容和子元素混合的复杂类型

<xs:element name="letter">

  <xs:complexType** mixed="true"**>

    <xs:sequence>

      <xs:element name="name" type="xs:string"/>

      <xs:element name="orderid" type="xs:positiveInteger"/>

      <xs:element name="shipdate" type="xs:date"/>

    </xs:sequence>

  </xs:complexType>

</xs:element>

以上定义对应的Xml

<letter>

Dear Mr.<name>John Smith</name>.

Your order <orderid>1032</orderid>

will be shipped on <shipdate>2001-07-13</shipdate>.

</letter>

5． 定义包含属性和子元素的复杂类型

 

**使用指示器******

在Xsd中的指示器包括

1． 顺序指示器

1） All

指示子元素可以以任何顺序出现，并且每一个元素都必须出现一次

<xs:element name="person">

  <xs:complexType>

    <xs:all>

      <xs:element name="firstname" type="xs:string"/>

      <xs:element name="lastname" type="xs:string"/>

    </xs:all>

  </xs:complexType>

</xs:element>

2） Choice

指示子元素中可以出现一个或者另一个

<xs:element name="person">

  <xs:complexType>

    <xs:choice>

      <xs:element name="employee" type="employee"/>

      <xs:element name="member" type="member"/>

    </xs:choice>

  </xs:complexType>

</xs:element>

3） Sequence

指示子元素必须按照顺序出现

<xs:element name="person">

  <xs:complexType>

    <xs:sequence>

      <xs:element name="firstname" type="xs:string"/>

      <xs:element name="lastname" type="xs:string"/>

    </xs:sequence>

  </xs:complexType>

</xs:element>

2． 出现次数指示器minOccurs，maxOccurs

<xs:element name="person">

  <xs:complexType>

    <xs:sequence>

      <xs:element name="full_name" type="xs:string"/>

      <xs:element name="child_name" type="xs:string"

      maxOccurs="10" minOccurs="0"/>

    </xs:sequence>

  </xs:complexType>

</xs:element>

3． 组指示器（group Indicators）

用来定义相关的一组元素

<xs:group name="persongroup">

  <xs:sequence>

    <xs:element name="firstname" type="xs:string"/>

    <xs:element name="lastname" type="xs:string"/>

    <xs:element name="birthday" type="xs:date"/>

  </xs:sequence>

</xs:group>

<xs:element name="person" type="personinfo"/>

<xs:complexType name="personinfo">

  <xs:sequence>

    <xs:group ref="persongroup"/>

    <xs:element name="country" type="xs:string"/>

  </xs:sequence>

   </xs:complexType>

用来定义一组相关的属性

<xs:attributeGroup name="personattrgroup">

  <xs:attribute name="firstname" type="xs:string"/>

  <xs:attribute name="lastname" type="xs:string"/>

  <xs:attribute name="birthday" type="xs:date"/>

</xs:attributeGroup>

<xs:element name="person">

  <xs:complexType>

    <xs:attributeGroup ref="personattrgroup"/>

  </xs:complexType>

</xs:element>

**Any****关键字******

表示可以有任意元素

<xs:element name="person">

  <xs:complexType>

    <xs:sequence>

      <xs:element name="firstname" type="xs:string"/>

      <xs:element name="lastname" type="xs:string"/>

      <xs:any minOccurs="0"/>

    </xs:sequence>

  </xs:complexType>

</xs:element>

**anyAttribute****关键字******

<xs:element name="person">

  <xs:complexType>

    <xs:sequence>

      <xs:element name="firstname" type="xs:string"/>

      <xs:element name="lastname" type="xs:string"/>

    </xs:sequence>

    <xs:anyAttribute/>

  </xs:complexType>

</xs:element>

**substitutionGroup****关键字******

表示某一个元素和另一个替代元素定义相同

<xs:element name="name" type="xs:string"/>

<xs:element name="navn" substitutionGroup="name"/>

<xs:complexType name="custinfo">

  <xs:sequence>

    <xs:element ref="name"/>

  </xs:sequence>

</xs:complexType><xs:element name="customer" type="custinfo"/>

<xs:element name="kunde" substitutionGroup="customer"/>

 

摘自

[http://www.onlyblog.com/blog2/linxiaoya/9613.html](http://www.onlyblog.com/blog2/linxiaoya/9613.html)

[http://www.cnblogs.com/yukaizhao/archive/2007/03/25/xsd_tutorial.html](http://www.cnblogs.com/yukaizhao/archive/2007/03/25/xsd_tutorial.html)
来源： <[http://blog.csdn.net/begtostudy/article/details/3218310](http://blog.csdn.net/begtostudy/article/details/3218310)> 
