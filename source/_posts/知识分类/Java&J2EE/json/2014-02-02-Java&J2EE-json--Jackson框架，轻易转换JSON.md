---
layout: post
title: "Jackson 框架，轻易转换JSON"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - json
--- 

# Jackson 框架，轻易转换JSON

[]()

[hoojo](http://www.cnblogs.com/hoojo/)
学习在于积累：滴水可以石穿！ 学而不思则罔，思而不学则殆！
[博客园](http://www.cnblogs.com/)   [首页](http://www.cnblogs.com/hoojo/)   [博问](http://q.cnblogs.com/)   [闪存](http://home.cnblogs.com/ing/)   [新随笔](http://www.cnblogs.com/hoojo/admin/EditPosts.aspx?opt=1)   [联系](http://space.cnblogs.com/msg/send/hoojo)   [订阅](http://www.cnblogs.com/hoojo/rss)[![订阅]()](http://www.cnblogs.com/hoojo/rss)  [管理](http://www.cnblogs.com/hoojo/admin/EditPosts.aspx)

随笔-139  评论-674  文章-3  trackbacks-0

# [Jackson 框架，轻易转换JSON](http://www.cnblogs.com/hoojo/archive/2011/04/22/2024628.html)

Jackson可以轻松的将Java对象转换成json对象和xml文档，同样也可以将json、xml转换成Java对象。

前面有介绍过json-lib这个框架，在线博文：[http://www.cnblogs.com/hoojo/archive/2011/04/21/2023805.html](http://www.cnblogs.com/hoojo/archive/2011/04/21/2023805.html)

相比json-lib框架，Jackson所依赖的jar包较少，简单易用并且性能也要相对高些。而且Jackson社区相对比较活跃，更新速度也比较快。

**一、****准备工作******

1、 下载依赖库jar包

Jackson的jar all下载地址：[http://jackson.codehaus.org/1.7.6/jackson-all-1.7.6.jar](http://jackson.codehaus.org/1.7.6/jackson-all-1.7.6.jar)

然后在工程中导入这个jar包即可开始工作

官方示例：[http://wiki.fasterxml.com/JacksonInFiveMinutes](http://wiki.fasterxml.com/JacksonInFiveMinutes)

因为下面的程序是用junit测试用例运行的，所以还得添加junit的jar包。版本是junit-4.2.8

如果你需要转换xml，那么还需要stax2-api.jar

2、 测试类基本代码如下
package com.hoo.test; import java.io.IOException;import java.io.StringWriter;import java.util.ArrayList;import java.util.HashMap;import java.util.Iterator;import java.util.LinkedHashMap;import java.util.List;import java.util.Map;import java.util.Set;import org.codehaus.jackson.JsonEncoding;import org.codehaus.jackson.JsonGenerationException;import org.codehaus.jackson.JsonGenerator;import org.codehaus.jackson.JsonParseException;import org.codehaus.jackson.map.JsonMappingException;import org.codehaus.jackson.map.ObjectMapper;import org.codehaus.jackson.node.JsonNodeFactory;import org.codehaus.jackson.xml.XmlMapper;import org.junit.After;import org.junit.Before;import org.junit.Test;import com.hoo.entity.AccountBean; /*** <b>function:</b>Jackson 将java对象转换成JSON字符串，也可以将JSON字符串转换成java对象* jar-lib-version: jackson-all-1.6.2* jettison-1.0.1* @author hoojo* @createDate 2010-11-23 下午04:54:53* @file JacksonTest.java* @package com.hoo.test* @project Spring3* @blog http://blog.csdn.net/IBM_hoojo* @email hoojo_@126.com* @version 1.0*/@SuppressWarnings("unchecked")public class JacksonTest {private JsonGenerator jsonGenerator = null;private ObjectMapper objectMapper = null;private AccountBean bean = null;@Beforepublic void init() {bean = new AccountBean();bean.setAddress("china-Guangzhou");bean.setEmail("hoojo_@126.com");bean.setId(1);bean.setName("hoojo");objectMapper = new ObjectMapper();try {jsonGenerator = objectMapper.getJsonFactory().createJsonGenerator(System.out, JsonEncoding.UTF8);} catch (IOException e) {e.printStackTrace();}}@Afterpublic void destory() {try {if (jsonGenerator != null) {jsonGenerator.flush();}if (!jsonGenerator.isClosed()) {jsonGenerator.close();}jsonGenerator = null;objectMapper = null;bean = null;System.gc();} catch (IOException e) {e.printStackTrace();}}}

3、 所需要的JavaEntity

package com.hoo.entity; public class AccountBean {private int id;private String name;private String email;private String address;private Birthday birthday;//getter、setter@Overridepublic String toString() {return this.name + "#" + this.id + "#" + this.address + "#" + this.birthday + "#" + this.email;}}

Birthday

package com.hoo.entity; public class Birthday {private String birthday;public Birthday(String birthday) {super();this.birthday = birthday;} //getter、setter public Birthday() {}@Overridepublic String toString() {return this.birthday;}}

**二、****Java****对象转换成****JSON**

1、 JavaBean(Entity/Model)转换成JSON
/*** <b>function:</b>将java对象转换成json字符串* @author hoojo* @createDate 2010-11-23 下午06:01:10*/@Testpublic void writeEntityJSON() {try {System.out.println("jsonGenerator");//writeObject可以转换java对象，eg:JavaBean/Map/List/Array等jsonGenerator.writeObject(bean);System.out.println();System.out.println("ObjectMapper");//writeValue具有和writeObject相同的功能objectMapper.writeValue(System.out, bean);} catch (IOException e) {e.printStackTrace();}}

运行后结果如下：

jsonGenerator{"address":"china-Guangzhou","name":"hoojo","id":1,"birthday":null,"email":"hoojo_@126.com"}ObjectMapper{"address":"china-Guangzhou","name":"hoojo","id":1,"birthday":null,"email":"hoojo_@126.com"}

上面分别利用JsonGenerator的writeObject方法和ObjectMapper的writeValue方法完成对Java对象的转换，二者传递的参数及构造的方式不同；JsonGenerator的创建依赖于ObjectMapper对象。也就是说如果你要使用JsonGenerator来转换JSON，那么你必须创建一个ObjectMapper。但是你用ObjectMapper来转换JSON，则不需要JSONGenerator。

objectMapper的writeValue方法可以将一个Java对象转换成JSON。这个方法的参数一，需要提供一个输出流，转换后可以通过这个流来输出转换后的内容。或是提供一个File，将转换后的内容写入到File中。当然，这个参数也可以接收一个JSONGenerator，然后通过JSONGenerator来输出转换后的信息。第二个参数是将要被转换的Java对象。如果用三个参数的方法，那么是一个Config。这个config可以提供一些转换时的规则，过指定的Java对象的某些属性进行过滤或转换等。

2、 将Map集合转换成Json字符串
/*** <b>function:</b>将map转换成json字符串* @author hoojo* @createDate 2010-11-23 下午06:05:26*/@Testpublic void writeMapJSON() {try {Map<String, Object> map = new HashMap<String, Object>();map.put("name", bean.getName());map.put("account", bean);bean = new AccountBean();bean.setAddress("china-Beijin");bean.setEmail("hoojo@qq.com");map.put("account2", bean);System.out.println("jsonGenerator");jsonGenerator.writeObject(map);System.out.println("");System.out.println("objectMapper");objectMapper.writeValue(System.out, map);} catch (IOException e) {e.printStackTrace();}}

转换后结果如下：

jsonGenerator{"account2":{"address":"china-Beijin","name":null,"id":0,"birthday":null,"email":"hoojo@qq.com"},"name":"hoojo","account":{"address":"china-Guangzhou","name":"hoojo","id":1,"birthday":null,"email":"hoojo_@126.com"}}objectMapper{"account2":{"address":"china-Beijin","name":null,"id":0,"birthday":null,"email":"hoojo@qq.com"},"name":"hoojo","account":{"address":"china-Guangzhou","name":"hoojo","id":1,"birthday":null,"email":"hoojo_@126.com"}}

3、 将List集合转换成json

/*** <b>function:</b>将list集合转换成json字符串* @author hoojo* @createDate 2010-11-23 下午06:05:59*/@Testpublic void writeListJSON() {try {List<AccountBean> list = new ArrayList<AccountBean>();list.add(bean);bean = new AccountBean();bean.setId(2);bean.setAddress("address2");bean.setEmail("email2");bean.setName("haha2");list.add(bean);System.out.println("jsonGenerator");//list转换成JSON字符串jsonGenerator.writeObject(list);System.out.println();System.out.println("ObjectMapper");//用objectMapper直接返回list转换成的JSON字符串System.out.println("1###" + objectMapper.writeValueAsString(list));System.out.print("2###");//objectMapper list转换成JSON字符串objectMapper.writeValue(System.out, list);} catch (IOException e) {e.printStackTrace();}}

结果如下：

jsonGenerator[{"address":"china-Guangzhou","name":"hoojo","id":1,"birthday":null,"email":"hoojo_@126.com"},{"address":"address2","name":"haha2","id":2,"birthday":null,"email":"email2"}]ObjectMapper1###[{"address":"china-Guangzhou","name":"hoojo","id":1,"birthday":null,"email":"hoojo_@126.com"},{"address":"address2","name":"haha2","id":2,"birthday":null,"email":"email2"}]2###[{"address":"china-Guangzhou","name":"hoojo","id":1,"birthday":null,"email":"hoojo_@126.com"},{"address":"address2","name":"haha2","id":2,"birthday":null,"email":"email2"}]

外面就是多了个[]中括号；同样Array也可以转换，转换的JSON和上面的结果是一样的，这里就不再转换了。~.~

4、下面来看看jackson提供的一些类型，用这些类型完成json转换；如果你使用这些类型转换JSON的话，那么你即使没有JavaBean(Entity)也可以完成复杂的Java类型的JSON转换。下面用到这些类型构建一个复杂的Java对象，并完成JSON转换。
@Testpublic void writeOthersJSON() {try {String[] arr = { "a", "b", "c" };System.out.println("jsonGenerator");String str = "hello world jackson!";//bytejsonGenerator.writeBinary(str.getBytes());//booleanjsonGenerator.writeBoolean(true);//nulljsonGenerator.writeNull();//floatjsonGenerator.writeNumber(2.2f);//charjsonGenerator.writeRaw("c");//StringjsonGenerator.writeRaw(str, 5, 10);//StringjsonGenerator.writeRawValue(str, 5, 5);//StringjsonGenerator.writeString(str);jsonGenerator.writeTree(JsonNodeFactory.instance.POJONode(str));System.out.println();//ObjectjsonGenerator.writeStartObject();//{jsonGenerator.writeObjectFieldStart("user");//user:{jsonGenerator.writeStringField("name", "jackson");//name:jacksonjsonGenerator.writeBooleanField("sex", true);//sex:truejsonGenerator.writeNumberField("age", 22);//age:22jsonGenerator.writeEndObject();//}jsonGenerator.writeArrayFieldStart("infos");//infos:[jsonGenerator.writeNumber(22);//22jsonGenerator.writeString("this is array");//this is arrayjsonGenerator.writeEndArray();//]jsonGenerator.writeEndObject();//}AccountBean bean = new AccountBean();bean.setAddress("address");bean.setEmail("email");bean.setId(1);bean.setName("haha");//complex ObjectjsonGenerator.writeStartObject();//{jsonGenerator.writeObjectField("user", bean);//user:{bean}jsonGenerator.writeObjectField("infos", arr);//infos:[array]jsonGenerator.writeEndObject();//}} catch (Exception e) {e.printStackTrace();}}

运行后，结果如下：

jsonGenerator"aGVsbG8gd29ybGQgamFja3NvbiE=" true null 2.2c world jac worl "hello world jackson!" "hello world jackson!"{"user":{"name":"jackson","sex":true,"age":22},"infos":[22,"this is array"]}{"user":{"address":"address","name":"haha","id":1,"birthday":null,"email":"email"},"infos":["a","b","c"]}

怎么样？构造的json字符串和输出的结果是一致的吧。关键看懂用JSONGenerator提供的方法，完成一个Object的构建。

**三、****JSON****转换成****Java****对象******

1、 将json字符串转换成JavaBean对象
@Testpublic void readJson2Entity() {String json = "{\"address\":\"address\",\"name\":\"haha\",\"id\":1,\"email\":\"email\"}";try {AccountBean acc = objectMapper.readValue(json, AccountBean.class);System.out.println(acc.getName());System.out.println(acc);} catch (JsonParseException e) {e.printStackTrace();} catch (JsonMappingException e) {e.printStackTrace();} catch (IOException e) {e.printStackTrace();}}

很简单，用到了ObjectMapper这个对象的readValue这个方法，这个方法需要提供2个参数。第一个参数就是解析的JSON字符串，第二个参数是即将将这个JSON解析吃什么Java对象，Java对象的类型。当然，还有其他相同签名方法，如果你有兴趣可以一一尝试使用方法，当然使用的方法和当前使用的方法大同小异。运行后，结果如下：

hahahaha#1#address#null#email

2、 将json字符串转换成List<Map>集合

/*** <b>function:</b>json字符串转换成list<map>* @author hoojo* @createDate 2010-11-23 下午06:12:01*/@Testpublic void readJson2List() {String json = "[{\"address\": \"address2\",\"name\":\"haha2\",\"id\":2,\"email\":\"email2\"},"+"{\"address\":\"address\",\"name\":\"haha\",\"id\":1,\"email\":\"email\"}]";try {List<LinkedHashMap<String, Object>> list = objectMapper.readValue(json, List.class);System.out.println(list.size());for (int i = 0; i < list.size(); i++) {Map<String, Object> map = list.get(i);Set<String> set = map.keySet();for (Iterator<String> it = set.iterator();it.hasNext();) {String key = it.next();System.out.println(key + ":" + map.get(key));}}} catch (JsonParseException e) {e.printStackTrace();} catch (JsonMappingException e) {e.printStackTrace();} catch (IOException e) {e.printStackTrace();}}

尝试过将上面的JSON转换成List，然后List中存放AccountBean，但结果失败了。但是支持Map集合。因为你转成List.class，但是不知道List存放何种类型。只好默然Map类型。因为所有的对象都可以转换成Map结合，运行后结果如下：

2address:address2name:haha2id:2email:email2address:addressname:hahaid:1email:email

3、 Json字符串转换成Array数组，由于上面的泛型转换不能识别到集合中的对象类型。所有这里用对象数组，可以解决这个问题。只不过它不再是集合，而是一个数组。当然这个不重要，你可以用Arrays.asList将其转换成List即可。

/*** <b>function:</b>json字符串转换成Array* @author hoojo* @createDate 2010-11-23 下午06:14:01*/@Testpublic void readJson2Array() {String json = "[{\"address\": \"address2\",\"name\":\"haha2\",\"id\":2,\"email\":\"email2\"},"+"{\"address\":\"address\",\"name\":\"haha\",\"id\":1,\"email\":\"email\"}]";try {AccountBean[] arr = objectMapper.readValue(json, AccountBean[].class);System.out.println(arr.length);for (int i = 0; i < arr.length; i++) {System.out.println(arr[i]);}} catch (JsonParseException e) {e.printStackTrace();} catch (JsonMappingException e) {e.printStackTrace();} catch (IOException e) {e.printStackTrace();}}

运行后的结果：

2haha2#2#address2#null#email2haha#1#address#null#email

4、 Json字符串转换成Map集合

/*** <b>function:</b>json字符串转换Map集合* @author hoojo* @createDate Nov 27, 2010 3:00:06 PM*/@Testpublic void readJson2Map() {String json = "{\"success\":true,\"A\":{\"address\": \"address2\",\"name\":\"haha2\",\"id\":2,\"email\":\"email2\"},"+"\"B\":{\"address\":\"address\",\"name\":\"haha\",\"id\":1,\"email\":\"email\"}}";try {Map<String, Map<String, Object>> maps = objectMapper.readValue(json, Map.class);System.out.println(maps.size());Set<String> key = maps.keySet();Iterator<String> iter = key.iterator();while (iter.hasNext()) {String field = iter.next();System.out.println(field + ":" + maps.get(field));}} catch (JsonParseException e) {e.printStackTrace();} catch (JsonMappingException e) {e.printStackTrace();} catch (IOException e) {e.printStackTrace();}}

运行后结果如下：

3success:trueA:{address=address2, name=haha2, id=2, email=email2}B:{address=address, name=haha, id=1, email=email}

**四、****Jackson****对****XML****的支持******

Jackson也可以完成java对象到xml的转换，转换后的结果要比json-lib更直观，不过它依赖于stax2-api.jar这个jar包。
/*** <b>function:</b>java对象转换成xml文档* 需要额外的jar包 stax2-api.jar* @author hoojo* @createDate 2010-11-23 下午06:11:21*/@Testpublic void writeObject2Xml() {//stax2-api-3.0.2.jarSystem.out.println("XmlMapper");XmlMapper xml = new XmlMapper();try {//javaBean转换成xml//xml.writeValue(System.out, bean);StringWriter sw = new StringWriter();xml.writeValue(sw, bean);System.out.println(sw.toString());//List转换成xmlList<AccountBean> list = new ArrayList<AccountBean>();list.add(bean);list.add(bean);System.out.println(xml.writeValueAsString(list));//Map转换xml文档Map<String, AccountBean> map = new HashMap<String, AccountBean>();map.put("A", bean);map.put("B", bean);System.out.println(xml.writeValueAsString(map));} catch (JsonGenerationException e) {e.printStackTrace();} catch (JsonMappingException e) {e.printStackTrace();} catch (IOException e) {e.printStackTrace();}}

运行上面的方法，结果如下：

XmlMapper<unknown><address>china-Guangzhou</address><name>hoojo</name><id>1</id><birthday/><email>hoojo_@126.com</email></unknown><unknown><unknown><address>china-Guangzhou</address><name>hoojo</name><id>1</id><birthday/><email>hoojo_@126.com</email></unknown><email><address>china-Guangzhou</address><name>hoojo</name><id>1</id><birthday/><email>hoojo_@126.com</email></email></unknown><unknown><A><address>china-Guangzhou</address><name>hoojo</name><id>1</id><birthday/><email>hoojo_@126.com</email></A><B><address>china-Guangzhou</address><name>hoojo</name><id>1</id><birthday/><email>hoojo_@126.com</email></B></unknown>

看结果，根节点都是unknown 这个问题还没有解决，由于根节点没有转换出来，所有导致解析xml到Java对象，也无法完成。

* 作者：[hoojo](http://hoojo.cnblogs.com/)
出处：[http://www.cnblogs.com/hoojo/archive/2011/04/22/2024628.html](http://hoojo.cnblogs.com/)
blog：[http://blog.csdn.net/IBM_hoojo](http://blog.csdn.net/IBM_hoojo/)
本文版权归作者和博客园共有，欢迎转载，但未经作者同意必须保留此段声明，且在文章页面明显位置给出原文连接，否则保留追究法律责任的权利。
[版权所有，转载请注明出处](http://hoojo.cnblogs.com/) [本文出自： http://www.cnblogs.com/hoojo/archive/2011/04/22/2024628.html](http://hoojo.cnblogs.com/)
[![分享道]()版权所有，欢迎转载，转载请注明出处，谢谢](http://hoojo.cnblogs.com/)

顶
[Top]()

收藏
关注

评论
分类: [JavaEE](http://www.cnblogs.com/hoojo/category/276244.html), [JavaSE](http://www.cnblogs.com/hoojo/category/276247.html), [Others](http://www.cnblogs.com/hoojo/category/276254.html), [JSON&XML](http://www.cnblogs.com/hoojo/category/295538.html)

标签: [JavaEE](http://www.cnblogs.com/hoojo/tag/JavaEE/), [JavaSE](http://www.cnblogs.com/hoojo/tag/JavaSE/), [JSON](http://www.cnblogs.com/hoojo/tag/JSON/), [XML转换](http://www.cnblogs.com/hoojo/tag/XML%E8%BD%AC%E6%8D%A2/), [JSON转换](http://www.cnblogs.com/hoojo/tag/JSON%E8%BD%AC%E6%8D%A2/)
绿色通道： [好文要顶]() [关注我]() [收藏该文]()[与我联系](http://space.cnblogs.com/msg/send/hoojo) [![]()]( "分享至新浪微博")

[![]()](http://home.cnblogs.com/u/hoojo/)

[hoojo](http://home.cnblogs.com/u/hoojo/)
[关注 - 1](http://home.cnblogs.com/u/hoojo/followees)
[粉丝 - 475](http://home.cnblogs.com/u/hoojo/followers)

[+加关注]()

7

0
(请您对文章做出评价)

[«](http://www.cnblogs.com/hoojo/archive/2011/04/21/2023805.html) 上一篇：[JSON-lib框架，转换JSON、XML不再困难](http://www.cnblogs.com/hoojo/archive/2011/04/21/2023805.html "发布于2011-04-21 17:24")
[»](http://www.cnblogs.com/hoojo/archive/2011/04/22/2025197.html) 下一篇：[xStream完美转换XML、JSON](http://www.cnblogs.com/hoojo/archive/2011/04/22/2025197.html "发布于2011-04-22 18:46")

posted on 2011-04-22 10:45 [hoojo](http://www.cnblogs.com/hoojo/) 阅读(37975) 评论(16) [编辑](http://www.cnblogs.com/hoojo/admin/EditPosts.aspx?postid=2024628) [收藏]()

**评论:**

[#1楼]()[]() 2011-04-22 13:29 | [Dawnxu](http://www.cnblogs.com/xnet/) [ ](http://space.cnblogs.com/msg/send/Dawnxu "发送站内短消息")
这个很不错，可惜android下用不上..

[支持(0)]()[反对(0)]()
  
[#2楼]()[]()[楼主] 2011-04-22 13:50 | [hoojo](http://www.cnblogs.com/hoojo/) [ ](http://space.cnblogs.com/msg/send/hoojo "发送站内短消息")
[@]( "查看所回复的评论")Dawnxu
怎么用不上，可以将转换后的字符串当做参数传递过去，这样在接收的地方再进行反序列化。应该是可以的！

[支持(0)]()[反对(0)]()
http://pic.cnitblog.com/face/u151517.jpg  

[#3楼]()[]() 2011-12-27 09:31 | [dearxiaofan](http://home.cnblogs.com/u/364867/) [ ](http://space.cnblogs.com/msg/send/dearxiaofan "发送站内短消息")
很不错的文章，我们android项目数据获取的框架用的就是这个。学习了。~

[支持(0)]()[反对(0)]()
  
[#4楼]()[]() 2012-02-28 15:25 | [7-up](http://home.cnblogs.com/u/380104/) [ ](http://space.cnblogs.com/msg/send/7-up "发送站内短消息")
文章写得很详细，但我还是不太理解java对象要怎么设计
比如下面的一段json：
{"error":0,"data":{"name":"ABC","age":20,"phone":{"home":"abc","mobile":"def"},"friends":[{"name":"DEF","phone":{"home":"hij","mobile":"klm"}},{"name":"GHI","phone":{"home":"nop","mobile":"qrs"}}]},"other":{"nickname":[]}}
我要怎样才能将其反序列？
再问一个问题对于一个未知的json有没有通用方法将其反序列，还是说一定要自己构造相应的java对象，才能实现反序列化呢？

[支持(0)]()[反对(0)]()
  

[#5楼]()[]()[楼主] 2012-03-01 09:20 | [hoojo](http://www.cnblogs.com/hoojo/) [ ](http://space.cnblogs.com/msg/send/hoojo "发送站内短消息")
[@]( "查看所回复的评论")7-up
String json = {"error":0,"data":{"name":"ABC","age":20,"phone":{"home":"abc","mobile":"def"},"friends":[{"name":"DEF","phone":{"home":"hij","mobile":"klm"}},{"name":"GHI","phone":{"home":"nop","mobile":"qrs"}}]},"other":{"nickname":[]}}
Map<String, Map<String, Object>> maps = objectMapper.readValue(json, Map.class);
这样就可以反序列化了，是一个map对象，map对象中存放的是LinkHashMap、LinkList对象，不一定要封装成Java对象。

[支持(0)]()[反对(0)]()
http://pic.cnitblog.com/face/u151517.jpg  
[#6楼]()[]() 2012-03-03 15:21 | [7-up](http://home.cnblogs.com/u/380104/) [ ](http://space.cnblogs.com/msg/send/7-up "发送站内短消息")
[@]( "查看所回复的评论")hoojo
谢谢楼主帮忙，问题已解决

[支持(0)]()[反对(0)]()
  

[#7楼]()[]() 2012-03-16 13:33 | [fanjifeng](http://home.cnblogs.com/u/387171/) [ ](http://space.cnblogs.com/msg/send/fanjifeng "发送站内短消息")
你好！楼主！ 我看你的博文，但是对于Jackson对XML的支持其中需要使用stax2-api-3.0.2.jar包，我下载该包后，发现没有org.codehaus.jackson.xml.XmlMapper;，能否请楼主给个下载地址，或者发下包，我邮箱fjfzsm@sina.com

[支持(0)]()[反对(0)]()
  
[#8楼]()[]()[楼主] 2012-03-20 09:52 | [hoojo](http://www.cnblogs.com/hoojo/) [ ](http://space.cnblogs.com/msg/send/hoojo "发送站内短消息")
[@]( "查看所回复的评论")fanjifeng
是否在jackson-all-1.7.6.jar这个包中

[支持(0)]()[反对(0)]()
http://pic.cnitblog.com/face/u151517.jpg  

[#9楼]()[]() 2012-03-20 22:37 | [fanjifeng](http://home.cnblogs.com/u/387171/) [ ](http://space.cnblogs.com/msg/send/fanjifeng "发送站内短消息")
我查看了jackson-all-1.7.6.jar, jackson-all-1.9.5.jar 都没有org.codehaus.jackson.xml.XmlMapper类，而且没有org.codehaus.jackson.xml包路径。

[支持(0)]()[反对(0)]()
  
[#10楼]()[]()[楼主] 2012-03-21 09:12 | [hoojo](http://www.cnblogs.com/hoojo/) [ ](http://space.cnblogs.com/msg/send/hoojo "发送站内短消息")
[@]( "查看所回复的评论")fanjifeng
jackson-all-1.6.2.jar有这个类，新版本的可能去掉了

[支持(0)]()[反对(0)]()
http://pic.cnitblog.com/face/u151517.jpg  

[#11楼]()[]() 2012-07-19 11:38 | [梦里六月的飞雪](http://home.cnblogs.com/u/429109/) [ ](http://space.cnblogs.com/msg/send/%e6%a2%a6%e9%87%8c%e5%85%ad%e6%9c%88%e7%9a%84%e9%a3%9e%e9%9b%aa "发送站内短消息")
这篇博客很不错，而且在很多地方也看到相同文章，我找到这篇文章，下载了，jackson-all-
1.7.6.jar 实现了，实现了，除转换为xml之外的其他所有功能，但是转换为xml一直不成功，很是
纠结，一直找不懂XmlMapper这个类，第一天下午没搞定，然后第二天上午还是在google上查资料，
终于 在 [http://www.cowtowncoder.com/blog/archives/2012/03/entry_467.html](http://www.cowtowncoder.com/blog/archives/2012/03/entry_467.html) 上面找到了些头
绪 ，jackson-dataformat-xml [https://github.com/FasterXML/jackson-dataformat-xml](https://github.com/FasterXML/jackson-dataformat-xml) ，在这
个页面上下载了 jackson-dataformat-xml-2.0.2.jar， 终于找到 XmlMapper 了，但是 放到
classpath下怎么也不能导入，后来才知道，他依赖的jar是 jackson-core-2.0.4.jar、jackson-
databind-2.0.4.jar、jackson-annotations-2.0.4.jar ，然后 在加上 stax2-api-3.1.1.jar ，
终于测试成功了，原来的 jackson-all-1.7.6.jar ,从classpath移除，加入新的包后，全部测试通
过，博主的案例将的很好，很详细，如果还解决不了 ，fmx35699@163.com ,我可以测试通过的源码
给大家，

[支持(1)]()[反对(0)]()
  
[#12楼]()[]() 2012-07-19 12:36 | [梦里六月的飞雪](http://home.cnblogs.com/u/429109/) [ ](http://space.cnblogs.com/msg/send/%e6%a2%a6%e9%87%8c%e5%85%ad%e6%9c%88%e7%9a%84%e9%a3%9e%e9%9b%aa "发送站内短消息")
[@]( "查看所回复的评论")fanjifeng
照我的解决方案试下，就可以了，jackson一直更新，而且更新很快，现在的最新2.04了，换下jar试试

[支持(0)]()[反对(0)]()
  

[#13楼]()[]() 2012-08-26 22:38 | [余波可](http://home.cnblogs.com/u/302736/) [ ](http://space.cnblogs.com/msg/send/%e4%bd%99%e6%b3%a2%e5%8f%af "发送站内短消息")
[@]( "查看所回复的评论")梦里六月的飞雪
我遇到和你一样的问题，XmlMapper这个类没有找到。其他的测试都通过了 。我按你的解决方案 “原来的 jackson-all-1.7.6.jar ,从classpath移除，加入新的包jackson-core-2.0.4.jar、jackson-databind-2.0.4.jar、jackson-annotations-2.0.4.jar后，stax2-api-3.1.1.jar 也加入了， 之前测试通过的反而测试失败了。特求一份源码。
我的邮箱：timyuheng@163.com 谢谢先

[支持(0)]()[反对(0)]()
  
[#14楼]()[]() 2013-02-16 01:46 | [kingdelee](http://home.cnblogs.com/u/496311/) [ ](http://space.cnblogs.com/msg/send/kingdelee "发送站内短消息")
博主你好哈~
我自己尝试了一下用jackson2和1都没有成功，能否在您的博客中提供一份工程源码下载参考一下或者电邮给我呢，感激不尽啊！！！
kingdelee@gmail.com

[支持(0)]()[反对(0)]()
  

[#15楼]()[]() 2013-04-12 15:14 | [苏城](http://home.cnblogs.com/u/293701/) [ ](http://space.cnblogs.com/msg/send/%e8%8b%8f%e5%9f%8e "发送站内短消息")
好文，学习了。。

[支持(0)]()[反对(0)]()
  
[#16楼]()[]()27053592013/6/15 11:55:09 2013-06-15 11:55 | [章小慢](http://home.cnblogs.com/u/538879/) [ ](http://space.cnblogs.com/msg/send/%e7%ab%a0%e5%b0%8f%e6%85%a2 "发送站内短消息")
没有发现umknow 情况

[支持(0)]()[反对(0)]()
  
[刷新评论]()[刷新页面]()[返回顶部]()

注册用户登录后才能发表评论，请 [登录]() 或 [注册]()，[访问](http://www.cnblogs.com/)网站首页。
[博客园首页](http://www.cnblogs.com/ "程序员的网上家园")[博问](http://q.cnblogs.com/ "程序员问答社区")[新闻](http://news.cnblogs.com/ "IT新闻")[闪存](http://home.cnblogs.com/ing/)[程序员招聘](http://job.cnblogs.com/)[知识库](http://kb.cnblogs.com/)

**最新IT新闻**:
· [消息称金庸将联合完美和畅游对大陆市场侵权游戏展开维权](http://news.cnblogs.com/n/185261/)
· [Google 主义 vs 苹果主义](http://news.cnblogs.com/n/185260/)
· [移动开发者爱金钱，更爱成就感](http://news.cnblogs.com/n/185258/)
· [“厕所休闲网站”Backlabel](http://news.cnblogs.com/n/185257/)
· [蒂法竟然不是第一 细数玩家心中20位红粉知己](http://news.cnblogs.com/n/185256/)
» [更多新闻...](http://news.cnblogs.com/ "IT新闻")

**最新知识库文章**:
· [我现在是这样编程的](http://kb.cnblogs.com/page/180323/)
· [如何做到 jQuery-free？](http://kb.cnblogs.com/page/178891/)
· [如何成为一位优秀的创业CEO](http://kb.cnblogs.com/page/185000/)
· [十年前的Java企业应用开发世界](http://kb.cnblogs.com/page/184977/)
· [专注做好一件事](http://kb.cnblogs.com/page/184935/)
» [更多知识库文章...](http://kb.cnblogs.com/)
### About Me

![]()
网名：hoojo
E-mail：[hoojo_@126.com](mailto:hoojo_@126.com)
专注于Java，现从事电警卡口、智能交通、电子警察、数字城市等应用开发，擅长JavaEE、Flex、ActionScript及Web前端HTML、CSS、JavaScript、ExtJS、jQuery、Mootools等开发。对常用开源框架有一定的认识和见解。

### 版权声明

hoojo 所有文章遵循[创作共用版权协议](http://creativecommons.org/licenses/by-nc-sa/2.5/cn/)，要求**署名、非商业、保持一致**。在满足[创作共用版权协议](http://creativecommons.org/licenses/by-nc-sa/2.5/cn/)的基础上可以转载，但请以超链接形式注明出处。

### 访客统计

* [undefined]()

Powered By [Clicki.cn](http://www.clicki.cn/)

### 订阅统计

[![订阅我的博客]()](http://feed.feedsky.com/hoojo "订阅我的博客")

### 订阅文章

[![抓虾]()](http://www.zhuaxia.com/add_channel.php?url=http://feed.feedsky.com/hoojo)
[![google reader]()](http://fusion.google.com/add?feedurl=http://feed.feedsky.com/hoojo)
[![netvibes]()](http://www.netvibes.com/subscribe.php?url=http://feed.feedsky.com/hoojo)
[![my yahoo]()](http://add.my.yahoo.com/rss?url=http://feed.feedsky.com/hoojo)
[![bloglines]()](http://www.bloglines.com/sub/http://feed.feedsky.com/hoojo)
[![鲜果]()](http://www.xianguo.com/subscribe.php?url=http://feed.feedsky.com/hoojo)
[![哪吒]()](http://inezha.com/add?url=http://feed.feedsky.com/hoojo)
[![有道]()](http://reader.youdao.com/b.do?keyfrom=feedsky&url=http://feed.feedsky.com/hoojo)
[![QQ邮箱]()](http://mail.qq.com/cgi-bin/feed?u=http://feed.feedsky.com/hoojo)
     [![订阅内容到手机]()](http://wap.feedsky.com/hoojo)

### 订阅历史

### [IT 新闻](http://news.cnblogs.com/)

* [淘宝出台针对低价引流平台限制新政 称无意干掉”导购网站](http://news.cnblogs.com/n/185262/) 4分钟前
* [消息称金庸将联合完美和畅游对大陆市场侵权游戏展开维权](http://news.cnblogs.com/n/185261/) 5分钟前
* [Google 主义 vs 苹果主义](http://news.cnblogs.com/n/185260/) 16分钟前
* [移动开发者爱金钱，更爱成就感](http://news.cnblogs.com/n/185258/) 49分钟前
* [厕所休闲网站”Backlabel](http://news.cnblogs.com/n/185257/) 50分钟前
昵称：[hoojo](http://home.cnblogs.com/u/hoojo/)
园龄：[3年](http://home.cnblogs.com/u/hoojo/ "入园时间：2010-08-09")
粉丝：[475](http://home.cnblogs.com/u/hoojo/followers/)
关注：[1](http://home.cnblogs.com/u/hoojo/followees/)

[+加关注]()

[<]()2011年4月[>]()日一二三四五六27282930311234567891011121314[15](http://www.cnblogs.com/hoojo/archive/2011/04/15.html)161718[19](http://www.cnblogs.com/hoojo/archive/2011/04/19.html)20[21](http://www.cnblogs.com/hoojo/archive/2011/04/21.html)[22](http://www.cnblogs.com/hoojo/archive/2011/04/22.html)2324[25](http://www.cnblogs.com/hoojo/archive/2011/04/25.html)[26](http://www.cnblogs.com/hoojo/archive/2011/04/26.html)[27](http://www.cnblogs.com/hoojo/archive/2011/04/27.html)28[29](http://www.cnblogs.com/hoojo/archive/2011/04/29.html)301234567
### 搜索

 

 

### 常用链接

* [我的随笔](http://www.cnblogs.com/hoojo/MyPosts.html)
* [我的评论](http://www.cnblogs.com/hoojo/MyComments.html)
* [我的参与](http://www.cnblogs.com/hoojo/OtherPosts.html "我发表过评论的随笔")
* [最新评论](http://www.cnblogs.com/hoojo/RecentComments.html)
* [我的标签](http://www.cnblogs.com/hoojo/tag/)

### 最新随笔

* [1. 软件设计之UML—UML中的六大关系](http://www.cnblogs.com/hoojo/p/uml_design.html)
* [2. Spring 整合 Flex （BlazeDS）无法从as对象 到 Java对象转换的异常：org.springframework.beans.ConversionNotSupportedException: Failed to convert property value of type 'java.util.Date' to required type 'java.sql.Timestamp' for property 'wfsj'; nested exception is java.lang.Ill](http://www.cnblogs.com/hoojo/p/3196252.html)
* [3. ActiveMQ 即时通讯服务 浅析](http://www.cnblogs.com/hoojo/p/active_mq_jms_apache_activeMQ.html)
* [4. ant 使用指南](http://www.cnblogs.com/hoojo/archive/2013/06/14/java_ant_project_target_task_run.html)
* [5. Eclipse下的Java反编译插件 查看源代码不再困难](http://www.cnblogs.com/hoojo/archive/2013/04/12/3016516.html)
* [6. 基于开源 Openfire 聊天服务器 - 开发Openfire聊天记录插件](http://www.cnblogs.com/hoojo/archive/2013/03/29/openfire_plugin_chatlogs_plugin_.html)
* [7. 跟我一步一步开发自己的Openfire插件](http://www.cnblogs.com/hoojo/archive/2013/03/07/2947502.html)
* [8. cnblogs博文浏览[推荐、Top、评论、关注、收藏]利器代码片段](http://www.cnblogs.com/hoojo/archive/2013/03/04/2942591.html)
* [9. 谈谈个人关于程序开发中，“零配置”和“有配置”的看法](http://www.cnblogs.com/hoojo/archive/2012/10/31/2747838.html)
* [10. Lucene 基础理论](http://www.cnblogs.com/hoojo/archive/2012/09/06/2672891.html)

### 我的标签

* [JavaEE](http://www.cnblogs.com/hoojo/tag/JavaEE/)(36)
* [DataBase](http://www.cnblogs.com/hoojo/tag/DataBase/)(31)
* [JavaSE](http://www.cnblogs.com/hoojo/tag/JavaSE/)(20)
* [Oracle](http://www.cnblogs.com/hoojo/tag/Oracle/)(17)
* [WebService](http://www.cnblogs.com/hoojo/tag/WebService/)(16)
* [框架整合](http://www.cnblogs.com/hoojo/tag/%E6%A1%86%E6%9E%B6%E6%95%B4%E5%90%88/)(15)
* [Spring](http://www.cnblogs.com/hoojo/tag/Spring/)(15)
* [Axis2 WebService](http://www.cnblogs.com/hoojo/tag/Axis2 WebService/)(11)
* [笔记](http://www.cnblogs.com/hoojo/tag/%E7%AC%94%E8%AE%B0/)(11)
* [XML](http://www.cnblogs.com/hoojo/tag/XML/)(9)
* [更多](http://www.cnblogs.com/hoojo/tag/)

### 随笔分类(387)

* [Ajax【富客户端技术】(13)](http://www.cnblogs.com/hoojo/category/276233.html)
* [DataBase(18)](http://www.cnblogs.com/hoojo/category/276234.html)
* [Design Pattern(1)](http://www.cnblogs.com/hoojo/category/276270.html)
* [DWR(1)](http://www.cnblogs.com/hoojo/category/276235.html)
* [ExtJS(6)](http://www.cnblogs.com/hoojo/category/276236.html)
* [Flex/ActionScript(4)](http://www.cnblogs.com/hoojo/category/276237.html)
* [FrameWork Integration(21)](http://www.cnblogs.com/hoojo/category/276269.html)
* [GXT](http://www.cnblogs.com/hoojo/category/276238.html)
* [Hibernate(3)](http://www.cnblogs.com/hoojo/category/276239.html)
* [HTML/CSS(12)](http://www.cnblogs.com/hoojo/category/276240.html)
* [IDE/Utils(3)](http://www.cnblogs.com/hoojo/category/276241.html)
* [IT WorkPlace【IT职场】](http://www.cnblogs.com/hoojo/category/276243.html)
* [JavaEE(68)](http://www.cnblogs.com/hoojo/category/276244.html)
* [JavaME(1)](http://www.cnblogs.com/hoojo/category/276245.html)
* [JavaScript(9)](http://www.cnblogs.com/hoojo/category/276246.html)
* [JavaSE(26)](http://www.cnblogs.com/hoojo/category/276247.html)
* [JDBC(2)](http://www.cnblogs.com/hoojo/category/276248.html)
* [jQuery(6)](http://www.cnblogs.com/hoojo/category/276249.html)
* [JSON&XML(15)](http://www.cnblogs.com/hoojo/category/295538.html)
* [Jsp/Servlet(5)](http://www.cnblogs.com/hoojo/category/276250.html)
* [Lucene/Compass/Solr(2)](http://www.cnblogs.com/hoojo/category/411091.html)
* [Ms SQL Server(10)](http://www.cnblogs.com/hoojo/category/276251.html)
* [MyBatis(6)](http://www.cnblogs.com/hoojo/category/294148.html)
* [MySQL(3)](http://www.cnblogs.com/hoojo/category/305964.html)
* [Openfire/XMPP(11)](http://www.cnblogs.com/hoojo/category/380029.html)
* [Oracle(17)](http://www.cnblogs.com/hoojo/category/276252.html)
* [Others(27)](http://www.cnblogs.com/hoojo/category/276254.html)
* [RCP【富客户端技术】(16)](http://www.cnblogs.com/hoojo/category/276255.html)
* [RIA 【富互联网程序】(15)](http://www.cnblogs.com/hoojo/category/276256.html)
* [SE FTS(3)](http://www.cnblogs.com/hoojo/category/276271.html)
* [Spring(16)](http://www.cnblogs.com/hoojo/category/276257.html)
* [Struts(2)](http://www.cnblogs.com/hoojo/category/276258.html)
* [Struts2.x(6)](http://www.cnblogs.com/hoojo/category/276259.html)
* [UML(1)](http://www.cnblogs.com/hoojo/category/504221.html)
* [WebService(36)](http://www.cnblogs.com/hoojo/category/276011.html)
* [XmlHttpRequest(2)](http://www.cnblogs.com/hoojo/category/276261.html)
* [业界动态](http://www.cnblogs.com/hoojo/category/271627.html)
* [娱乐人生](http://www.cnblogs.com/hoojo/category/276272.html)

### 随笔档案(139)

* [2013年8月 (1)](http://www.cnblogs.com/hoojo/archive/2013/08.html)
* [2013年7月 (1)](http://www.cnblogs.com/hoojo/archive/2013/07.html)
* [2013年6月 (2)](http://www.cnblogs.com/hoojo/archive/2013/06.html)
* [2013年4月 (1)](http://www.cnblogs.com/hoojo/archive/2013/04.html)
* [2013年3月 (3)](http://www.cnblogs.com/hoojo/archive/2013/03.html)
* [2012年10月 (1)](http://www.cnblogs.com/hoojo/archive/2012/10.html)
* [2012年9月 (2)](http://www.cnblogs.com/hoojo/archive/2012/09.html)
* [2012年8月 (4)](http://www.cnblogs.com/hoojo/archive/2012/08.html)
* [2012年7月 (7)](http://www.cnblogs.com/hoojo/archive/2012/07.html)
* [2012年6月 (3)](http://www.cnblogs.com/hoojo/archive/2012/06.html)
* [2012年5月 (4)](http://www.cnblogs.com/hoojo/archive/2012/05.html)
* [2012年3月 (1)](http://www.cnblogs.com/hoojo/archive/2012/03.html)
* [2012年2月 (6)](http://www.cnblogs.com/hoojo/archive/2012/02.html)
* [2012年1月 (4)](http://www.cnblogs.com/hoojo/archive/2012/01.html)
* [2011年10月 (2)](http://www.cnblogs.com/hoojo/archive/2011/10.html)
* [2011年9月 (4)](http://www.cnblogs.com/hoojo/archive/2011/09.html)
* [2011年8月 (2)](http://www.cnblogs.com/hoojo/archive/2011/08.html)
* [2011年7月 (10)](http://www.cnblogs.com/hoojo/archive/2011/07.html)
* [2011年6月 (8)](http://www.cnblogs.com/hoojo/archive/2011/06.html)
* [2011年5月 (24)](http://www.cnblogs.com/hoojo/archive/2011/05.html)
* [2011年4月 (12)](http://www.cnblogs.com/hoojo/archive/2011/04.html)
* [2011年3月 (19)](http://www.cnblogs.com/hoojo/archive/2011/03.html)
* [2011年1月 (2)](http://www.cnblogs.com/hoojo/archive/2011/01.html)
* [2010年12月 (16)](http://www.cnblogs.com/hoojo/archive/2010/12.html)

### 文章档案(3)

* [2010年12月 (1)](http://www.cnblogs.com/hoojo/archives/2010/12.html)
* [2010年11月 (2)](http://www.cnblogs.com/hoojo/archives/2010/11.html)

### 相册

* [博文图库](http://www.cnblogs.com/hoojo/gallery/276187.html)

### 关注博客

* [WinGeek[Web前端工程师]](http://blog.csdn.net/WinGeek)

### 我的链接

* [1K JS](http://js1k.com/)
* [InfoQ](http://www.infoq.com/cn)
* [My CSDN Blog](http://blog.csdn.net/IBM_hoojo)[![]()]()
* [我的BlogJava](http://hoojo.blogjava.net/)[![]()]()

### 积分与排名

* 积分 - 189629
* 排名 - 523

### 最新评论

* [1. Re:MyBatis3整合Spring3、SpringMVC3](http://www.cnblogs.com/hoojo/archive/2011/04/15/2016324.html#2749206)
* [@]( "查看所回复的评论")hoojo
谢谢楼主的解答。我知道是这样用着比较方便，只是不明白问什么非要定义一个接口才行。后面又去看了一些官网的文档，现在明白了。MapperScannerConfigurer自动去生成代理类有两种方法，一种是基于接口的，一种是基于注解的。楼主的是采用基于接口的，我这样理解没错吧？
* --zhangteng
* [2. Re:软件设计之UML&amp;mdash;UML中的六大关系](http://www.cnblogs.com/hoojo/p/uml_design.html#2749039)
* 楼主的文档都很不错，理论+实践
* --以琴为铭
* [3. Re:MyBatis3整合Spring3、SpringMVC3](http://www.cnblogs.com/hoojo/archive/2011/04/15/2016324.html#2748810)
* [@]( "查看所回复的评论")墨迹的默默
接口中的方法在xml配置文件中没有找到对应的方法，检查命名空间和Mapper的路径是否一致，方法名及其返回值、参数列表是否相同。
* --hoojo
* [4. Re:MyBatis3整合Spring3、SpringMVC3](http://www.cnblogs.com/hoojo/archive/2011/04/15/2016324.html#2748809)
* [@]( "查看所回复的评论")zhangteng
不是规定的，就是方便。我想你也不想每添加一个Mapper接口都在xml文件中配一个Bean吧。哪种方法简单，一看便知。你认为呢？
* --hoojo
* [5. Re:Solr开发文档](http://www.cnblogs.com/hoojo/archive/2011/10/21/2220431.html#2747286)
* 写的很详细，非常好的资料。
* --naijgnorus
* [6. Re:MyBatis3整合Spring3、SpringMVC3](http://www.cnblogs.com/hoojo/archive/2011/04/15/2016324.html#2745700)
* 楼主，你文章中写道用扫描的方式自动为mapper生产代理类，不用每个都配置一个bean。这种方法必须定义一个SqlMapper，所有的mapper再去继承它，这是为什么呢？是MyBatis规定的吗？
* --zhangteng
* [7. Re:Eclipse下的Java反编译插件 查看源代码不再困难](http://www.cnblogs.com/hoojo/archive/2013/04/12/3016516.html#2744875)
* [@]( "查看所回复的评论")hoojo
可以查看源码，但是单步调试进入反编译出来的源码时查看不了那些源码的变量值。
* --StrikeW
* [8. Re:基于开源 Openfire 聊天服务器 - 开发Openfire聊天记录插件](http://www.cnblogs.com/hoojo/archive/2013/03/29/openfire_plugin_chatlogs_plugin_.html#2744789)
* 兄弟，把你这个插件的源码，发给我吧，wlly216@163.com 谢谢
* --星月磊子
* [9. Re:跟我一步一步开发自己的Openfire插件](http://www.cnblogs.com/hoojo/archive/2013/03/07/2947502.html#2744763)
* 果然是我打包的jar有问题，现在解决了，谢谢。
* --nash
* [10. Re:四、handler的作用及特性](http://www.cnblogs.com/hoojo/archive/2010/12/20/1911370.html#2744743)
* 非常，用来作为日志记录！
不过，中文好像显示的是乱码！
* --冰玉翔龙
* [11. Re:MySQL 学习笔记 一](http://www.cnblogs.com/hoojo/archive/2011/06/20/2085390.html#2744497)
* 支持一下，入门都的福音~~
* --_cc
* [12. Re:JavaScript/jQuery、HTML、CSS 构建 Web IM 远程及时聊天通信程序](http://www.cnblogs.com/hoojo/archive/2012/08/13/2635779.html#2743550)
* 您好~~最近我也在做这个web聊天，抽空给我发下源码啊！楼主好人~！834596798@qq.com
* --以琴为铭
* [13. Re:跟我一步一步开发自己的Openfire插件](http://www.cnblogs.com/hoojo/archive/2013/03/07/2947502.html#2743460)
* [@]( "查看所回复的评论")nash
应该会被自动解压的，是不是你的插件打包有问题呀，检查下。
* --hoojo
* [14. Re:基于开源 Openfire 聊天服务器 - 开发Openfire聊天记录插件](http://www.cnblogs.com/hoojo/archive/2013/03/29/openfire_plugin_chatlogs_plugin_.html#2743458)
* [@]( "查看所回复的评论")星月磊子
你需要自己修改插件中的SQL语句，然后再打包成插件。因为我这里是Oracle的数据库
* --hoojo
* [15. Re:基于开源 Openfire 聊天服务器 - 开发Openfire聊天记录插件](http://www.cnblogs.com/hoojo/archive/2013/03/29/openfire_plugin_chatlogs_plugin_.html#2743455)
* [@]( "查看所回复的评论")星月磊子
个别语句可能不行，你需要自己修改插件中的SQL语句，然后再打包成插件。
* --hoojo
* [16. Re:Eclipse下的Java反编译插件 查看源代码不再困难](http://www.cnblogs.com/hoojo/archive/2013/04/12/3016516.html#2743453)
* [@]( "查看所回复的评论")StrikeW
可以的 你不能进入class看懂源代码吗？
* --hoojo
* [17. Re:Eclipse下的Java反编译插件 查看源代码不再困难](http://www.cnblogs.com/hoojo/archive/2013/04/12/3016516.html#2742665)
* 感谢你的分享，不过我在debug时还是无法查看变量的值，请问你可以吗？
* --StrikeW
* [18. Re:基于开源 Openfire 聊天服务器 - 开发Openfire聊天记录插件](http://www.cnblogs.com/hoojo/archive/2013/03/29/openfire_plugin_chatlogs_plugin_.html#2742658)
* com.hoo.openfire.chat.logs.DbChatLogsManager - chatLogs add exception: {}
我用的SQL Server数据库 插入聊天记录的时候出现上边的异常，难道不支持SQL Server
* --星月磊子
* [19. Re:基于开源 Openfire 聊天服务器 - 开发Openfire聊天记录插件](http://www.cnblogs.com/hoojo/archive/2013/03/29/openfire_plugin_chatlogs_plugin_.html#2742645)
* 兄弟，你那个插件支持SQL Server数据库么
* --星月磊子
* [20. Re:软件设计之UML&amp;mdash;UML中的六大关系](http://www.cnblogs.com/hoojo/p/uml_design.html#2741756)
* 总结的不错呀
* --左右互搏
* [21. Re:五、CXF WebService整合Spring](http://www.cnblogs.com/hoojo/archive/2011/03/30/1999563.html#2741614)
* 你好，按照你的例子演示了一遍，在使用客户端调用时出现这种异常：Caused by: org.apache.cxf.interceptor.Fault: Response was of unexpected text/html ContentType. Incoming portion of HTML stream直接通过url访问 http://localhost:8080/cxf-server...
* --easytour-c
* [22. Re:软件设计之UML&amp;mdash;UML中的六大关系](http://www.cnblogs.com/hoojo/p/uml_design.html#2741355)
* 好久没用了，快还给教师了
* --誤人子弟
* [23. Re:软件设计之UML&amp;mdash;UML中的六大关系](http://www.cnblogs.com/hoojo/p/uml_design.html#2741214)
* 看到这个，以前学过工作后一直没用用到实践中，对UML知识的一个回顾，谢享。
* --潇洒一回
* [24. Re:软件设计之UML&amp;mdash;UML中的六大关系](http://www.cnblogs.com/hoojo/p/uml_design.html#2741206)
* mark
* --tauruswu
* [25. Re:跟我一步一步开发自己的Openfire插件](http://www.cnblogs.com/hoojo/archive/2013/03/07/2947502.html#2740964)
* openfire 版本3.8.3
* --nash

### 阅读排行榜

* [1. SQL Server 存储过程(73923)](http://www.cnblogs.com/hoojo/archive/2011/07/19/2110862.html)
* [2. SQL Server 触发器(64639)](http://www.cnblogs.com/hoojo/archive/2011/07/20/2111316.html)
* [3. JSON-lib框架，转换JSON、XML不再困难(42287)](http://www.cnblogs.com/hoojo/archive/2011/04/21/2023805.html)
* [4. xStream完美转换XML、JSON(38479)](http://www.cnblogs.com/hoojo/archive/2011/04/22/2025197.html)
* [5. Jackson 框架，轻易转换JSON(37974)](http://www.cnblogs.com/hoojo/archive/2011/04/22/2024628.html)
* [6. 五、CXF WebService整合Spring(35181)](http://www.cnblogs.com/hoojo/archive/2011/03/30/1999563.html)
* [7. 【MongoDB for Java】Java操作MongoDB(33658)](http://www.cnblogs.com/hoojo/archive/2011/06/02/2068665.html)
* [8. Openfire 的安装和配置(24974)](http://www.cnblogs.com/hoojo/archive/2012/05/17/2506769.html)
* [9. jQuery 获取屏幕高度、宽度(21017)](http://www.cnblogs.com/hoojo/archive/2012/02/16/2354663.html)
* [10. Solr开发文档(16250)](http://www.cnblogs.com/hoojo/archive/2011/10/21/2220431.html)
* [11. MyBatis3整合Spring3、SpringMVC3(14369)](http://www.cnblogs.com/hoojo/archive/2011/04/15/2016324.html)
* [12. mongoDB 入门指南、示例(13582)](http://www.cnblogs.com/hoojo/archive/2011/06/01/2066426.html)
* [13. FreeMarker整合Spring 3(12726)](http://www.cnblogs.com/hoojo/archive/2011/04/19/2020551.html)
* [14. Spring REST(12123)](http://www.cnblogs.com/hoojo/archive/2011/06/10/2077422.html)
* [15. Struts2、Spring、Hibernate整合ExtJS(10766)](http://www.cnblogs.com/hoojo/archive/2011/01/07/1929577.html)
* [16. Ehcache 整合Spring 使用页面、对象缓存(10326)](http://www.cnblogs.com/hoojo/archive/2012/07/12/2587556.html)
* [17. Axis1.x WebService开发指南—目录索引(9678)](http://www.cnblogs.com/hoojo/archive/2010/12/20/1911349.html)
* [18. Spring整合CXF，发布RSETful 风格WebService(9463)](http://www.cnblogs.com/hoojo/archive/2012/07/23/2605219.html)
* [19. MySQL 学习笔记 一(9176)](http://www.cnblogs.com/hoojo/archive/2011/06/20/2085390.html)
* [20. JavaScript/jQuery、HTML、CSS 构建 Web IM 远程及时聊天通信程序(9119)](http://www.cnblogs.com/hoojo/archive/2012/08/13/2635779.html)
* [21. SQL Server T-SQL高级查询(9018)](http://www.cnblogs.com/hoojo/archive/2011/07/16/2108129.html)
* [22. 四、CXF WebService中传递复杂类型对象(8987)](http://www.cnblogs.com/hoojo/archive/2011/03/30/1999523.html)
* [23. SpringMVC 中整合JSON、XML视图一(8823)](http://www.cnblogs.com/hoojo/archive/2011/04/29/2032571.html)
* [24. IE6、IE7、IE8的CSS、JS兼容(8690)](http://www.cnblogs.com/hoojo/archive/2011/01/13/1934373.html)
* [25. MyBatis3整合Spring3的Transaction事务处理(8224)](http://www.cnblogs.com/hoojo/archive/2011/04/15/2017447.html)

### 评论排行榜

* [1. MyBatis3整合Spring3、SpringMVC3(70)](http://www.cnblogs.com/hoojo/archive/2011/04/15/2016324.html)
* [2. JavaScript/jQuery、HTML、CSS 构建 Web IM 远程及时聊天通信程序(38)](http://www.cnblogs.com/hoojo/archive/2012/08/13/2635779.html)
* [3. cnblogs博文浏览[推荐、Top、评论、关注、收藏]利器代码片段(37)](http://www.cnblogs.com/hoojo/archive/2013/03/04/2942591.html)
* [4. SQL Server 存储过程(35)](http://www.cnblogs.com/hoojo/archive/2011/07/19/2110862.html)
* [5. SQL Server T-SQL高级查询(26)](http://www.cnblogs.com/hoojo/archive/2011/07/16/2108129.html)
* [6. 跟我一步一步开发自己的Openfire插件(24)](http://www.cnblogs.com/hoojo/archive/2013/03/07/2947502.html)
* [7. FreeMarker整合Spring 3(22)](http://www.cnblogs.com/hoojo/archive/2011/04/19/2020551.html)
* [8. 基于开源 Openfire 聊天服务器 - 开发Openfire聊天记录插件(21)](http://www.cnblogs.com/hoojo/archive/2013/03/29/openfire_plugin_chatlogs_plugin_.html)
* [9. Spring整合DWR comet 实现无刷新 多人聊天室(21)](http://www.cnblogs.com/hoojo/archive/2011/06/08/2075201.html)
* [10. 【MongoDB for Java】Java操作MongoDB(19)](http://www.cnblogs.com/hoojo/archive/2011/06/02/2068665.html)
* [11. Struts2、Spring、Hibernate整合ExtJS(19)](http://www.cnblogs.com/hoojo/archive/2011/01/07/1929577.html)
* [12. 五、CXF WebService整合Spring(17)](http://www.cnblogs.com/hoojo/archive/2011/03/30/1999563.html)
* [13. Jackson 框架，轻易转换JSON(16)](http://www.cnblogs.com/hoojo/archive/2011/04/22/2024628.html)
* [14. SQL Server 开发指南(14)](http://www.cnblogs.com/hoojo/archive/2011/07/21/2112559.html)
* [15. SQL Server 触发器(14)](http://www.cnblogs.com/hoojo/archive/2011/07/20/2111316.html)
* [16. 谈谈个人关于程序开发中，“零配置”和“有配置”的看法(14)](http://www.cnblogs.com/hoojo/archive/2012/10/31/2747838.html)
* [17. Java 多线程断点下载文件(13)](http://www.cnblogs.com/hoojo/archive/2011/09/30/2196767.html)
* [18. SQL Server 事务、异常和游标(13)](http://www.cnblogs.com/hoojo/archive/2011/07/19/2110325.html)
* [19. JavaScript/jQuery WebIM 及时聊天通信工具 本地客户端(12)](http://www.cnblogs.com/hoojo/archive/2012/06/18/2553886.html)
* [20. 二、CXF 入门示例(12)](http://www.cnblogs.com/hoojo/archive/2011/03/29/1998909.html)
* [21. 移动应用（手机应用）开发IM聊天程序解决方案(11)](http://www.cnblogs.com/hoojo/archive/2012/07/31/2616896.html)
* [22. JSON-lib框架，转换JSON、XML不再困难(11)](http://www.cnblogs.com/hoojo/archive/2011/04/21/2023805.html)
* [23. Solr开发文档(10)](http://www.cnblogs.com/hoojo/archive/2011/10/21/2220431.html)
* [24. SQL Server 索引和视图(10)](http://www.cnblogs.com/hoojo/archive/2011/07/18/2109291.html)
* [25. mongoDB 入门指南、示例(9)](http://www.cnblogs.com/hoojo/archive/2011/06/01/2066426.html)

### 推荐排行榜

* [1. cnblogs博文浏览[推荐、Top、评论、关注、收藏]利器代码片段(153)](http://www.cnblogs.com/hoojo/archive/2013/03/04/2942591.html)
* [2. SQL Server 存储过程(57)](http://www.cnblogs.com/hoojo/archive/2011/07/19/2110862.html)
* [3. SQL Server 触发器(34)](http://www.cnblogs.com/hoojo/archive/2011/07/20/2111316.html)
* [4. SQL Server T-SQL高级查询(22)](http://www.cnblogs.com/hoojo/archive/2011/07/16/2108129.html)
* [5. 跟我一步一步开发自己的Openfire插件(15)](http://www.cnblogs.com/hoojo/archive/2013/03/07/2947502.html)
* [6. xStream完美转换XML、JSON(14)](http://www.cnblogs.com/hoojo/archive/2011/04/22/2025197.html)
* [7. JSON-lib框架，转换JSON、XML不再困难(14)](http://www.cnblogs.com/hoojo/archive/2011/04/21/2023805.html)
* [8. SQL Server 事务、异常和游标(13)](http://www.cnblogs.com/hoojo/archive/2011/07/19/2110325.html)
* [9. mongoDB 入门指南、示例(10)](http://www.cnblogs.com/hoojo/archive/2011/06/01/2066426.html)
* [10. JavaScript/jQuery、HTML、CSS 构建 Web IM 远程及时聊天通信程序(10)](http://www.cnblogs.com/hoojo/archive/2012/08/13/2635779.html)
* [11. SQL Server 索引和视图(9)](http://www.cnblogs.com/hoojo/archive/2011/07/18/2109291.html)
* [12. 【MongoDB for Java】Java操作MongoDB(9)](http://www.cnblogs.com/hoojo/archive/2011/06/02/2068665.html)
* [13. Solr开发文档(9)](http://www.cnblogs.com/hoojo/archive/2011/10/21/2220431.html)
* [14. SQL Server Transact-SQL 编程(8)](http://www.cnblogs.com/hoojo/archive/2011/07/15/2107740.html)
* [15. MyBatis3整合Spring3、SpringMVC3(8)](http://www.cnblogs.com/hoojo/archive/2011/04/15/2016324.html)
* [16. Jackson 框架，轻易转换JSON(7)](http://www.cnblogs.com/hoojo/archive/2011/04/22/2024628.html)
* [17. JavaScript/jQuery WebIM 及时聊天通信工具 本地客户端(7)](http://www.cnblogs.com/hoojo/archive/2012/06/18/2553886.html)
* [18. SQL Server 开发指南(7)](http://www.cnblogs.com/hoojo/archive/2011/07/21/2112559.html)
* [19. Lucene 基础理论(6)](http://www.cnblogs.com/hoojo/archive/2012/09/06/2672891.html)
* [20. MyBatis3整合Spring3的Transaction事务处理(6)](http://www.cnblogs.com/hoojo/archive/2011/04/15/2017447.html)
* [21. Openfire 的安装和配置(6)](http://www.cnblogs.com/hoojo/archive/2012/05/17/2506769.html)
* [22. jQuery 获取屏幕高度、宽度(5)](http://www.cnblogs.com/hoojo/archive/2012/02/16/2354663.html)
* [23. 谈谈个人关于程序开发中，“零配置”和“有配置”的看法(5)](http://www.cnblogs.com/hoojo/archive/2012/10/31/2747838.html)
* [24. Struts2、Spring、Hibernate整合ExtJS(5)](http://www.cnblogs.com/hoojo/archive/2011/01/07/1929577.html)
* [25. Spring REST(5)](http://www.cnblogs.com/hoojo/archive/2011/06/10/2077422.html)

Powered by: [博客园](http://www.cnblogs.com/) 模板提供：[沪江博客](http://blog.hjenglish.com/) Copyright ©2013 hoojo
