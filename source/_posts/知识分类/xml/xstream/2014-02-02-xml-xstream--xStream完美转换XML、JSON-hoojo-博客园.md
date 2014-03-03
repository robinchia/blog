---
layout: post
title: "xStream完美转换XML、JSON - hoojo - 博客园"
categories: xml
tags: 
 - xml
 - xstream
--- 

# xStream完美转换XML、JSON - hoojo - 博客园

# [xStream完美转换XML、JSON]()

**xStream****框架******

xStream可以轻易的将Java对象和xml文档相互转换，而且可以修改某个特定的属性和节点名称，而且也支持json的转换；

前面有介绍过json-lib这个框架，在线博文：[http://www.cnblogs.com/hoojo/archive/2011/04/21/2023805.html](http://www.cnblogs.com/hoojo/archive/2011/04/21/2023805.html)

以及Jackson这个框架，在线博文：[http://www.cnblogs.com/hoojo/archive/2011/04/22/2024628.html](http://www.cnblogs.com/hoojo/archive/2011/04/22/2024628.html)

它们都完美支持JSON，但是对xml的支持还不是很好。一定程度上限制了对Java对象的描述，不能让xml完全体现到对Java对象的描述。这里将会介绍xStream对JSON、XML的完美支持。xStream不仅对XML的转换非常友好，而且提供annotation注解，可以在JavaBean中完成对xml节点、属性的描述。以及对JSON也支持，只需要提供相关的JSONDriver就可以完成转换。

**一、****准备工作******

1、 下载jar包、及官方资源

xStream的jar下载地址：

[https://nexus.codehaus.org/content/repositories/releases/com/thoughtworks/xstream/xstream-distribution/1.3.1/xstream-distribution-1.3.1-bin.zip](https://nexus.codehaus.org/content/repositories/releases/com/thoughtworks/xstream/xstream-distribution/1.3.1/xstream-distribution-1.3.1-bin.zip)

官方的示例很全，官方参考示例：[http://xstream.codehaus.org/tutorial.html](http://xstream.codehaus.org/tutorial.html)

添加xstream-1.3.1.jar文件到工程中，就可以开始下面的工作；需要的jar如下：

[![clip_image002]( "clip_image002")](http://images.cnblogs.com/cnblogs_com/hoojo/201104/201104221846024160.jpg)

2、 测试用例代码
package com.hoo.test;

import java.io.IOException;
import java.io.ObjectInputStream;

import java.io.ObjectOutputStream;
import java.io.StringReader;

import java.io.Writer;
import java.util.ArrayList;

import java.util.HashMap;
import java.util.Iterator;

import java.util.List;
import java.util.Map;

import java.util.Set;
import org.codehaus.jettison.json.JSONException;

import org.junit.After;
import org.junit.Before;

import org.junit.Test;
import com.hoo.entity.Birthday;

import com.hoo.entity.Classes;
import com.hoo.entity.ListBean;

import com.hoo.entity.Student;
import com.thoughtworks.xstream.XStream;

import com.thoughtworks.xstream.io.HierarchicalStreamWriter;
import com.thoughtworks.xstream.io.json.JettisonMappedXmlDriver;

import com.thoughtworks.xstream.io.json.JsonHierarchicalStreamDriver;
import com.thoughtworks.xstream.io.json.JsonWriter;

/**
* <b>function:</b>Java对象和XML字符串的相互转换

* jar-lib-version: xstream-1.3.1
* @author hoojo

* @createDate Nov 27, 2010 12:15:15 PM
* @file XStreamTest.java

* @package com.hoo.test
* @project WebHttpUtils

* @blog http://blog.csdn.net/IBM_hoojo
* @email hoojo_@126.com

* @version 1.0
*/

@SuppressWarnings("unchecked")
public class XStreamTest {

private XStream xstream = null;
private ObjectOutputStream out = null;

private ObjectInputStream in = null;
private Student bean = null;

/**
* <b>function:</b>初始化资源准备

* @author hoojo
* @createDate Nov 27, 2010 12:16:28 PM

*/
@Before

public void init() {
try {

xstream = new XStream();
//xstream = new XStream(new DomDriver()); // 需要xpp3 jar

} catch (Exception e) {
e.printStackTrace();

}
bean = new Student();

bean.setAddress("china");
bean.setEmail("jack@email.com");

bean.setId(1);
bean.setName("jack");

Birthday day = new Birthday();
day.setBirthday("2010-11-22");

bean.setBirthday(day);
}

/**
* <b>function:</b>释放对象资源

* @author hoojo
* @createDate Nov 27, 2010 12:16:38 PM

*/
@After

public void destory() {
xstream = null;

bean = null;
try {

if (out != null) {
out.flush();

out.close();
}

if (in != null) {
in.close();

}
} catch (IOException e) {

e.printStackTrace();
}

System.gc();
}

public final void fail(String string) {
System.out.println(string);

}
public final void failRed(String string) {

System.err.println(string);
}

}

通过XStream对象的toXML方法就可以完成Java对象到XML的转换，toXML方法还有2个相同签名的方法，需要传递一个流。然后通过流来完成xml信息的输出。

3、 需要的JavaBean
package com.hoo.entity;

public class Student {
private int id;

private String name;
private String email;

private String address;
private Birthday birthday;

//getter、setter
public String toString() {

return this.name + "#" + this.id + "#" + this.address + "#" + this.birthday + "#" + this.email;
}

}

****

**二、****Java****转换成****XML**

1、 JavaBean转换XM
/**

* <b>function:</b>Java对象转换成XML字符串
* @author hoojo

* @createDate Nov 27, 2010 12:19:01 PM
*/

@Test
public void writeBean2XML() {

try {
fail("------------Bean->XML------------");

fail(xstream.toXML(bean));
fail("重命名后的XML");

//类重命名
//xstream.alias("account", Student.class);

//xstream.alias("生日", Birthday.class);
//xstream.aliasField("生日", Student.class, "birthday");

//xstream.aliasField("生日", Birthday.class, "birthday");
//fail(xstream.toXML(bean));

//属性重命名
xstream.aliasField("邮件", Student.class, "email");

//包重命名
xstream.aliasPackage("hoo", "com.hoo.entity");

fail(xstream.toXML(bean));
} catch (Exception e) {

e.printStackTrace();
}

}

看结果中的第一份xml内容，是没有经过然后修改或重命名的文档，按照原样输出。文档中的第二份文档的package经过重命名，email属性也经过重命名以及类名也可以进行重命名的。

运行后结果如下：
------------Bean->XML------------

<com.hoo.entity.Student>
<id>1</id>

<name>jack</name>
<email>jack@email.com</email>

<address>china</address>
<birthday>

<birthday>2010-11-22</birthday>
</birthday>

</com.hoo.entity.Student>
重命名后的XML

<hoo.Student>
<id>1</id>

<name>jack</name>
<邮件>jack@email.com</邮件>

<address>china</address>
<birthday>

<birthday>2010-11-22</birthday>
</birthday>

</hoo.Student>

2、 将List集合转换成xml文档

/**

* <b>function:</b>将Java的List集合转换成XML对象
* @author hoojo

* @createDate Nov 27, 2010 12:20:07 PM
*/

@Test
public void writeList2XML() {

try {
//修改元素名称

xstream.alias("beans", ListBean.class);
xstream.alias("student", Student.class);

fail("----------List-->XML----------");
ListBean listBean = new ListBean();

listBean.setName("this is a List Collection");
List<Object> list = new ArrayList<Object>();

list.add(bean);
list.add(bean);//引用bean

//list.add(listBean);//引用listBean，父元素
bean = new Student();

bean.setAddress("china");
bean.setEmail("tom@125.com");

bean.setId(2);
bean.setName("tom");

Birthday day = new Birthday("2010-11-22");
bean.setBirthday(day);

list.add(bean);
listBean.setList(list);

//将ListBean中的集合设置空元素，即不显示集合元素标签
//xstream.addImplicitCollection(ListBean.class, "list");

//设置reference模型
//xstream.setMode(XStream.NO_REFERENCES);//不引用

xstream.setMode(XStream.ID_REFERENCES);//id引用
//xstream.setMode(XStream.XPATH_ABSOLUTE_REFERENCES);//绝对路径引用

//将name设置为父类（Student）的元素的属性
xstream.useAttributeFor(Student.class, "name");

xstream.useAttributeFor(Birthday.class, "birthday");
//修改属性的name

xstream.aliasAttribute("姓名", "name");
xstream.aliasField("生日", Birthday.class, "birthday");

fail(xstream.toXML(listBean));
} catch (Exception e) {

e.printStackTrace();
}

}

上面的代码运行后，结果如下：

----------List-->XML----------

<beans id="1">
<name>this is a List Collection</name>

<list id="2">
<student id="3" 姓名="jack">

<id>1</id>
<email>jack@email.com</email>

<address>china</address>
<birthday id="4" 生日="2010-11-22"/>

</student>
<student reference="3"/>

<student id="5" 姓名="tom">
<id>2</id>

<email>tom@125.com</email>
<address>china</address>

<birthday id="6" 生日="2010-11-22"/>
</student>

</list>
</beans>

如果不加xstream.addImplicitCollection(ListBean.**class**, "list");

这个设置的话，会出现一个List节点包裹着Student节点元素。添加addImplicitCollection可以忽略这个list节点元素。那么上面的list节点就不存在，只会在beans元素中出现name、student这2个xml元素标签；

setMode是设置相同的对象的引用方式，如果设置XStream.NO_REFERENCES就是不引用，会输出2分相同的Student元素。如果是XStream.ID_REFERENCES会引用相同的那个对象的id属性，如果是XStream.XPATH_ABSOLUTE_REFERENCES引用，那么它将显示xpath路径。上面采用的id引用，<student reference="3"/>这个引用了id=3的那个student标签元素；

useAttributeFor是设置某个节点显示到父节点的属性中，也就是将指定class中的指定属性，在这个class元素节点的属性中显示。

如：<student><name>hoojo</name></student>

设置好后就是这样的结果：<student name=”hoojo”></student>

aliasAttribute是修改属性名称。

3、 在JavaBean中添加Annotation注解进行重命名设置

先看看JavaBean的代码
package com.hoo.entity;

import java.util.Arrays;
import java.util.Calendar;

import java.util.GregorianCalendar;
import java.util.List;

import com.thoughtworks.xstream.annotations.XStreamAlias;
import com.thoughtworks.xstream.annotations.XStreamAsAttribute;

import com.thoughtworks.xstream.annotations.XStreamConverter;
import com.thoughtworks.xstream.annotations.XStreamImplicit;

import com.thoughtworks.xstream.annotations.XStreamOmitField;
@XStreamAlias("class")

public class Classes {
/*

* 设置属性显示
*/

@XStreamAsAttribute
@XStreamAlias("名称")

private String name;
/*

* 忽略
*/

@XStreamOmitField
private int number;

@XStreamImplicit(itemFieldName = "Students")
private List<Student> students;

@SuppressWarnings("unused")
@XStreamConverter(SingleValueCalendarConverter.class)

private Calendar created = new GregorianCalendar();
public Classes(){}

public Classes(String name, Student... stu) {
this.name = name;

this.students = Arrays.asList(stu);
}

//getter、setter
}

SingleValueCalendarConverter.java这个是一个类型转换器

package com.hoo.entity;

import java.util.Calendar;
import java.util.Date;

import java.util.GregorianCalendar;
import com.thoughtworks.xstream.converters.Converter;

import com.thoughtworks.xstream.converters.MarshallingContext;
import com.thoughtworks.xstream.converters.UnmarshallingContext;

import com.thoughtworks.xstream.io.HierarchicalStreamReader;
import com.thoughtworks.xstream.io.HierarchicalStreamWriter;

public class SingleValueCalendarConverter implements Converter {
public void marshal(Object source, HierarchicalStreamWriter writer,

MarshallingContext context) {
Calendar calendar = (Calendar) source;

writer.setValue(String.valueOf(calendar.getTime().getTime()));
}

public Object unmarshal(HierarchicalStreamReader reader,
UnmarshallingContext context) {

GregorianCalendar calendar = new GregorianCalendar();
calendar.setTime(new Date(Long.parseLong(reader.getValue())));

return calendar;
}

@SuppressWarnings("unchecked")
public boolean canConvert(Class type) {

return type.equals(GregorianCalendar.class);
}

}

再看看测试用例代码

@Test

public void writeList2XML4Annotation() {
try {

failRed("---------annotation Bean --> XML---------");
Student stu = new Student();

stu.setName("jack");
Classes c = new Classes("一班", bean, stu);

c.setNumber(2);
//对指定的类使用Annotation

//xstream.processAnnotations(Classes.class);
//启用Annotation

//xstream.autodetectAnnotations(true);
xstream.alias("student", Student.class);

fail(xstream.toXML(c));
} catch (Exception e) {

e.printStackTrace();
}

}

当启用annotation或是对某个特定的类启用annotation时，上面的classes这个类才有效果。如果不启用annotation，运行后结果如下：

---------annotation Bean --> XML---------

<com.hoo.entity.Classes>
<name>一班</name>

<number>2</number>
<students class="java.util.Arrays$ArrayList">

<a class="student-array">
<student>

<id>1</id>
<name>jack</name>

<email>jack@email.com</email>
<address>china</address>

<birthday>
<birthday>2010-11-22</birthday>

</birthday>
</student>

<student>
<id>0</id>

<name>jack</name>
</student>

</a>
</students>

<created>
<time>1303292056718</time>

<timezone>Asia/Shanghai</timezone>
</created>

</com.hoo.entity.Classes>

当启用annotation后xstream.processAnnotations(Classes.class)，结果如下：

---------annotation Bean --> XML---------

<class 名称="一班">
<Students>

<id>1</id>
<name>jack</name>

<email>jack@email.com</email>
<address>china</address>

<birthday>
<birthday>2010-11-22</birthday>

</birthday>
</Students>

<Students>
<id>0</id>

<name>jack</name>
</Students>

<created>1303292242937</created>
</class>

4、 Map集合转换xml文档

/**

* <b>function:</b>Java Map集合转XML
* @author hoojo

* @createDate Nov 27, 2010 1:13:26 PM
*/

@Test
public void writeMap2XML() {

try {
failRed("---------Map --> XML---------");

Map<String, Student> map = new HashMap<String, Student>();
map.put("No.1", bean);//put

bean = new Student();
bean.setAddress("china");

bean.setEmail("tom@125.com");
bean.setId(2);

bean.setName("tom");
Birthday day = new Birthday("2010-11-22");

bean.setBirthday(day);
map.put("No.2", bean);//put

bean = new Student();
bean.setName("jack");

map.put("No.3", bean);//put
xstream.alias("student", Student.class);

xstream.alias("key", String.class);
xstream.useAttributeFor(Student.class, "id");

xstream.useAttributeFor("birthday", String.class);
fail(xstream.toXML(map));

} catch (Exception e) {
e.printStackTrace();

}
}

运行后结果如下：

---------Map --> XML---------

<map>
<entry>

<key>No.3</key>
<student id="0">

<name>jack</name>
</student>

</entry>
<entry>

<key>No.1</key>
<student id="1">

<name>jack</name>
<email>jack@email.com</email>

<address>china</address>
<birthday birthday="2010-11-22"/>

</student>
</entry>

<entry>
<key>No.2</key>

<student id="2">
<name>tom</name>

<email>tom@125.com</email>
<address>china</address>

<birthday birthday="2010-11-22"/>
</student>

</entry>
</map>

5、 用OutStream输出流写XML

/**

* <b>function:</b>用OutStream输出流写XML
* @author hoojo

* @createDate Nov 27, 2010 1:13:48 PM
*/

@Test
public void writeXML4OutStream() {

try {
out = xstream.createObjectOutputStream(System.out);

Student stu = new Student();
stu.setName("jack");

Classes c = new Classes("一班", bean, stu);
c.setNumber(2);

failRed("---------ObjectOutputStream # JavaObject--> XML---------");
out.writeObject(stu);

out.writeObject(new Birthday("2010-05-33"));
out.write(22);//byte

out.writeBoolean(true);
out.writeFloat(22.f);

out.writeUTF("hello");
} catch (Exception e) {

e.printStackTrace();
}

}

使用输出流后，可以通过流对象完成xml的构建，即使没有JavaBean对象，你可以用流来构建一个复杂的xml文档，运行后结果如下：

---------ObjectOutputStream # JavaObject--> XML---------

<object-stream>
<com.hoo.entity.Student>

<id>0</id>
<name>jack</name>

</com.hoo.entity.Student>
<com.hoo.entity.Birthday>

<birthday>2010-05-33</birthday>
</com.hoo.entity.Birthday>

<byte>22</byte>
<boolean>true</boolean>

<float>22.0</float>
<string>hello</string>

</object-stream>

****

**三、****XML****内容转换****Java****对象******

1、 用InputStream将XML文档转换成java对象
/**

* <b>function:</b>用InputStream将XML文档转换成java对象
* 需要额外的jar xpp3-main.jar

* @author hoojo
* @createDate Nov 27, 2010 1:14:52 PM

*/
@Test

public void readXML4InputStream() {
try {

String s = "<object-stream><com.hoo.entity.Student><id>0</id><name>jack</name>" +
"</com.hoo.entity.Student><com.hoo.entity.Birthday><birthday>2010-05-33</birthday>" +

"</com.hoo.entity.Birthday><byte>22</byte><boolean>true</boolean><float>22.0</float>" +
"<string>hello</string></object-stream>";

failRed("---------ObjectInputStream## XML --> javaObject---------");
StringReader reader = new StringReader(s);

in = xstream.createObjectInputStream(reader);
Student stu = (Student) in.readObject();

Birthday b = (Birthday) in.readObject();
byte i = in.readByte();

boolean bo = in.readBoolean();
float f = in.readFloat();

String str = in.readUTF();
System.out.println(stu);

System.out.println(b);
System.out.println(i);

System.out.println(bo);
System.out.println(f);

System.out.println(str);
} catch (Exception e) {

e.printStackTrace();
}

}

读取后，转换的Java对象，结果如下：

---------ObjectInputStream## XML --> javaObject---------

jack#0#null#null#null
2010-05-33

22
true

22.0
hello

2、 将xml文档转换成Java对象

/**

* <b>function:</b>将XML字符串转换成Java对象
* @author hoojo

* @createDate Nov 27, 2010 2:39:06 PM
*/

@Test
public void readXml2Object() {

try {
failRed("-----------Xml >>> Bean--------------");

Student stu = (Student) xstream.fromXML(xstream.toXML(bean));
fail(stu.toString());

List<Student> list = new ArrayList<Student>();
list.add(bean);//add

Map<String, Student> map = new HashMap<String, Student>();
map.put("No.1", bean);//put

bean = new Student();
bean.setAddress("china");

bean.setEmail("tom@125.com");
bean.setId(2);

bean.setName("tom");
Birthday day = new Birthday("2010-11-22");

bean.setBirthday(day);
list.add(bean);//add

map.put("No.2", bean);//put
bean = new Student();

bean.setName("jack");
list.add(bean);//add

map.put("No.3", bean);//put
failRed("==========XML >>> List===========");

List<Student> studetns = (List<Student>) xstream.fromXML(xstream.toXML(list));
fail("size:" + studetns.size());//3

for (Student s : studetns) {
fail(s.toString());

}
failRed("==========XML >>> Map===========");

Map<String, Student> maps = (Map<String, Student>) xstream.fromXML(xstream.toXML(map));
fail("size:" + maps.size());//3

Set<String> key = maps.keySet();
Iterator<String> iter = key.iterator();

while (iter.hasNext()) {
String k = iter.next();

fail(k + ":" + map.get(k));
}

} catch (Exception e) {
e.printStackTrace();

}
}

运行后结果如下：

-----------Xml >>> Bean--------------

jack#1#china#2010-11-22#jack@email.com
==========XML >>> List===========

size:3
jack#1#china#2010-11-22#jack@email.com

tom#2#china#2010-11-22#tom@125.com
jack#0#null#null#null

==========XML >>> Map===========
size:3

No.3:jack#0#null#null#null
No.1:jack#1#china#2010-11-22#jack@email.com

No.2:tom#2#china#2010-11-22#tom@125.com

怎么样，成功的完成XML到JavaBean、List、Map的转换，更多对象转换还需要大家一一尝试。用法类似~这里就不一样赘述。

**四、****XStream****对****JSON****的支持******

xStream对JSON也有非常好的支持，它提供了2个模型驱动。用这2个驱动可以完成Java对象到JSON的相互转换。使用JettisonMappedXmlDriver驱动，将Java对象转换成json，需要添加jettison.jar

1、 用JettisonMappedXmlDriver完成Java对象到JSON的转换
/**

* <b>function:</b>XStream结合JettisonMappedXmlDriver驱动，转换Java对象到JSON
* 需要添加jettison jar

* @author hoojo
* @createDate Nov 27, 2010 1:23:18 PM

*/
@Test

public void writeEntity2JETTSON() {
failRed("=======JettisonMappedXmlDriver===JavaObject >>>> JaonString=========");

xstream = new XStream(new JettisonMappedXmlDriver());
xstream.setMode(XStream.NO_REFERENCES);

xstream.alias("student", Student.class);
fail(xstream.toXML(bean));

}

运行后结果如下：

=======JettisonMappedXmlDriver===JavaObject >>>> JaonString=========

{"student":{"id":1,"name":"jack","email":"jack@email.com","address":"china","birthday":[{},"2010-11-22"]}}

JSON的转换和XML的转换用法一样，只是创建XStream需要传递一个参数，这个参数就是xml到JSON映射转换的驱动。这里会降到两个驱动，分别是JettisonMappedXmlDriver、JsonHierarchicalStreamDriver。

2、 JsonHierarchicalStreamDriver完成Java对象到JSON的转换
/**

* <b>function:</b>用XStream结合JsonHierarchicalStreamDriver驱动
* 转换java对象为JSON字符串

* @author hoojo
* @createDate Nov 27, 2010 1:16:46 PM

*/
@Test

public void writeEntiry2JSON() {
failRed("======JsonHierarchicalStreamDriver====JavaObject >>>> JaonString=========");

xstream = new XStream(new JsonHierarchicalStreamDriver());
//xstream.setMode(XStream.NO_REFERENCES);

xstream.alias("student", Student.class);
failRed("-------Object >>>> JSON---------");

fail(xstream.toXML(bean));
//failRed("========JsonHierarchicalStreamDriver==删除根节点=========");

//删除根节点
xstream = new XStream(new JsonHierarchicalStreamDriver() {

public HierarchicalStreamWriter createWriter(Writer out) {
return new JsonWriter(out, JsonWriter.DROP_ROOT_MODE);

}
});

//xstream.setMode(XStream.NO_REFERENCES);
xstream.alias("student", Student.class);

fail(xstream.toXML(bean));
}

运行后结果如下：

======JsonHierarchicalStreamDriver====JavaObject >>>> JaonString=========

-------Object >>>> JSON---------
{"student": {

"id": 1,
"name": "jack",

"email": "jack@email.com",
"address": "china",

"birthday": {
"birthday": "2010-11-22"

}
}}

{
"id": 1,

"name": "jack",
"email": "jack@email.com",

"address": "china",
"birthday": {

"birthday": "2010-11-22"
}

}

使用JsonHierarchicalStreamDriver转换默认会给转换后的对象添加一个根节点，但是在构建JsonHierarchicalStreamDriver驱动的时候，你可以重写createWriter方法，删掉根节点。

看上面的结果，一个是默认带根节点的JSON对象，它只是将类名作为一个属性，将对象作为该属性的一个值。而另一个没有带根属性的JSON就是通过重写createWriter方法完成的。

3、 将List集合转换成JSON字符串
@Test

public void writeList2JSON() {
failRed("======JsonHierarchicalStreamDriver====JavaObject >>>> JaonString=========");

JsonHierarchicalStreamDriver driver = new JsonHierarchicalStreamDriver();
xstream = new XStream(driver);

//xstream = new XStream(new JettisonMappedXmlDriver());//转换错误
//xstream.setMode(XStream.NO_REFERENCES);

xstream.alias("student", Student.class);
List<Student> list = new ArrayList<Student>();

list.add(bean);//add
bean = new Student();

bean.setAddress("china");
bean.setEmail("tom@125.com");

bean.setId(2);
bean.setName("tom");

Birthday day = new Birthday("2010-11-22");
bean.setBirthday(day);

list.add(bean);//add
bean = new Student();

bean.setName("jack");
list.add(bean);//add

fail(xstream.toXML(list));
//failRed("========JsonHierarchicalStreamDriver==删除根节点=========");

//删除根节点
xstream = new XStream(new JsonHierarchicalStreamDriver() {

public HierarchicalStreamWriter createWriter(Writer out) {
return new JsonWriter(out, JsonWriter.DROP_ROOT_MODE);

}
});

xstream.alias("student", Student.class);
fail(xstream.toXML(list));

}

运行后结果如下

======JsonHierarchicalStreamDriver====JavaObject >>>> JaonString=========

##{"list": [
{

"id": 1,
"name": "jack",

"email": "jack@email.com",
"address": "china",

"birthday": {
"birthday": "2010-11-22"

}
},

{
"id": 2,

"name": "tom",
"email": "tom@125.com",

"address": "china",
"birthday": {

"birthday": "2010-11-22"
}

},
{

"id": 0,
"name": "jack"

}
]}

#[
{

"id": 1,
"name": "jack",

"email": "jack@email.com",
"address": "china",

"birthday": {
"birthday": "2010-11-22"

}
},

{
"id": 2,

"name": "tom",
"email": "tom@125.com",

"address": "china",
"birthday": {

"birthday": "2010-11-22"
}

},
{

"id": 0,
"name": "jack"

}
]

上面的list1是使用JsonHierarchicalStreamDriver 转换的，当然你也可以使用JettisonMappedXmlDriver驱动进行转换；用JettisonMappedXmlDriver转换后，你会发现格式不同而且没有根属性。

4、 Map转换json
@Test

public void writeMap2JSON() {
failRed("======JsonHierarchicalStreamDriver==== Map >>>> JaonString=========");

xstream = new XStream(new JsonHierarchicalStreamDriver());
//xstream = new XStream(new JettisonMappedXmlDriver());

xstream.alias("student", Student.class);
Map<String, Student> map = new HashMap<String, Student>();

map.put("No.1", bean);//put
bean = new Student();

bean.setAddress("china");
bean.setEmail("tom@125.com");

bean.setId(2);
bean.setName("tom");

bean.setBirthday(new Birthday("2010-11-21"));
map.put("No.2", bean);//put

bean = new Student();
bean.setName("jack");

map.put("No.3", bean);//put
fail(xstream.toXML(map));

//failRed("========JsonHierarchicalStreamDriver==删除根节点=========");
//删除根节点

xstream = new XStream(new JsonHierarchicalStreamDriver() {
public HierarchicalStreamWriter createWriter(Writer out) {

return new JsonWriter(out, JsonWriter.DROP_ROOT_MODE);
}

});
xstream.alias("student", Student.class);

fail(xstream.toXML(map));
}

运行后结果如下：

======JsonHierarchicalStreamDriver==== Map >>>> JaonString=========

{"map": [
[

"No.3",
{

"id": 0,
"name": "jack"

}
],

[
"No.1",

{
"id": 1,

"name": "jack",
"email": "jack@email.com",

"address": "china",
"birthday": {

"birthday": "2010-11-22"
}

}
],

[
"No.2",

{
"id": 2,

"name": "tom",
"email": "tom@125.com",

"address": "china",
"birthday": {

"birthday": "2010-11-21"
}

}
]

]}
[

[
"No.3",

{
"id": 0,

"name": "jack"
}

],
[

"No.1",
{

"id": 1,
"name": "jack",

"email": "jack@email.com",
"address": "china",

"birthday": {
"birthday": "2010-11-22"

}
}

],
[

"No.2",
{

"id": 2,
"name": "tom",

"email": "tom@125.com",
"address": "china",

"birthday": {
"birthday": "2010-11-21"

}
}

]
]

5、 将JSON转换java对象

/**

* <b>function:</b>JsonHierarchicalStreamDriver可以将简单的json字符串转换成java对象，list、map转换不成功；
* JsonHierarchicalStreamDriver读取JSON字符串到java对象出错

* @author hoojo
* @createDate Nov 27, 2010 1:22:26 PM

* @throws JSONException
*/

@Test
public void readJSON2Object() throws JSONException {

String json = "{\"student\": {" +
"\"id\": 1," +

"\"name\": \"haha\"," +
"\"email\": \"email\"," +

"\"address\": \"address\"," +
"\"birthday\": {" +

"\"birthday\": \"2010-11-22\"" +
"}" +

"}}";
//JsonHierarchicalStreamDriver读取JSON字符串到java对象出错，但JettisonMappedXmlDriver可以

xstream = new XStream(new JettisonMappedXmlDriver());
xstream.alias("student", Student.class);

fail(xstream.fromXML(json).toString());
//JettisonMappedXmlDriver转换List集合出错，但JsonHierarchicalStreamDriver可以转换正确

//JettisonMappedXmlDriver 转换的字符串 {"list":{"student":[{"id":1,"name":"haha","email":"email","address":"address","birthday":[{},"2010-11-22"]}]},"student":{"id":2,"name":"tom","email":"tom@125.com","address":"china","birthday":[{},"2010-11-22"]}}
json = "{\"list\": [{" +

"\"id\": 1," +
"\"name\": \"haha\"," +

"\"email\": \"email\"," +
"\"address\": \"address\"," +

"\"birthday\": {" +
"\"birthday\": \"2010-11-22\"" +

"}" +
"},{" +

"\"id\": 2," +
"\"name\": \"tom\"," +

"\"email\": \"tom@125.com\"," +
"\"address\": \"china\"," +

"\"birthday\": {" +
"\"birthday\": \"2010-11-22\"" +

"}" +
"}]}";

System.out.println(json);//用js转换成功
List list = (List) xstream.fromXML(json);

System.out.println(list.size());//0好像转换失败
}

运行后结果如下：

haha#1#address#2010-11-22#email

{"list": [{"id": 1,"name": "haha","email": "email","address": "address","birthday": {"birthday": "2010-11-22"}},
{"id": 2,"name": "tom","email": "tom@125.com","address": "china","birthday": {"birthday": "2010-11-22"}}]}

0

JSON到Java的转换是fromXML方法。

作者：[hoojo](http://hoojo.cnblogs.com/)
出处：[http://www.cnblogs.com/hoojo/archive/2011/04/22/2025197.html](http://hoojo.cnblogs.com/)
blog：[http://blog.csdn.net/IBM_hoojo](http://blog.csdn.net/IBM_hoojo/)
本文版权归作者和博客园共有，欢迎转载，但未经作者同意必须保留此段声明，且在文章页面明显位置给出原文连接，否则保留追究法律责任的权利。
[版权所有，转载请注明出处](http://hoojo.cnblogs.com/) [本文出自：  http://www.cnblogs.com/hoojo/archive/2011/04/22/2025197.html](http://hoojo.cnblogs.com/)
