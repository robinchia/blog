---
layout: post
title: "XStream 学习笔记 - 枫情的Blog - BlogJava"
categories: xml
tags: 
 - xml
 - xstream
--- 

# XStream 学习笔记 - 枫情的Blog - BlogJava

  [枫情的Blog](http://www.blogjava.net/zlkn2005/)
程序员不光有自己的程序还有自己的生活。 风雨情     ### 常用链接

* [我的随笔](http://www.blogjava.net/zlkn2005/MyPosts.html)
* [我的评论](http://www.blogjava.net/zlkn2005/MyComments.html)
* [我的参与](http://www.blogjava.net/zlkn2005/OtherPosts.html)
* [最新评论](http://www.blogjava.net/zlkn2005/RecentComments.html)

### 留言簿(1)

* [给我留言](http://www.blogjava.net/zlkn2005/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/zlkn2005/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/zlkn2005/admin/MyMessages.aspx)

# 随笔档案

* [2005年12月 (1)](http://www.blogjava.net/zlkn2005/archive/2005/12.html)

# 我MSN的个人空间

* [枫情之家](http://spaces.msn.com/members/zlkn2005/)

### 搜索

*
*  

### 积分与排名

* 积分 - 3689
* 排名 - 2224

### 最新评论 [![]()](http://www.blogjava.net/zlkn2005/CommentsRSS.aspx)

* [1. re: XStream 学习笔记](http://www.blogjava.net/zlkn2005/archive/2008/05/12/24240.html#200051)
* 评论内容较长,点击标题查看
* --jadar
* [2. re: XStream 学习笔记](http://www.blogjava.net/zlkn2005/archive/2008/03/03/24240.html#183462)
* 呵呵.在JAVA里面,INT类型的变量本来默认值就是0,你没有给它值,它也会默认将0作为这个变量的初始值....@xmlspy2004
* --心无痕
* [3. re: XStream 学习笔记](http://www.blogjava.net/zlkn2005/archive/2005/12/19/24240.html#24616)
* 评论内容较长,点击标题查看
* --xmlspy2004
* [4. re: XStream 学习笔记](http://www.blogjava.net/zlkn2005/archive/2005/12/16/24240.html#24242)
* 好
* --fanta
Powered by: [博客园](http://www.cnblogs.com/)
模板提供：[沪江博客](http://blog.hjenglish.com/) [BlogJava](http://www.blogjava.net/) | [首页](http://www.blogjava.net/zlkn2005/) | [发新随笔](http://www.blogjava.net/zlkn2005/admin/EditPosts.aspx?opt=1) | [发新文章](http://www.blogjava.net/EnterMyBlog.aspx?NewArticle=1) | [联系](http://www.blogjava.net/zlkn2005/contact.aspx?id=1) | [聚合](http://www.blogjava.net/zlkn2005/rss)[![]()](http://www.blogjava.net/zlkn2005/rss) | [管理](http://www.blogjava.net/zlkn2005/admin/EditPosts.aspx)

# [XStream 学习笔记]()

**XStream**

 

**使用XStream****的初衷**

 

研究和使用XStream的原因是我在项目中的一个预研。在项目中需要应用到对XML文件的管理和配置，因此需要一个能够将对象保存为XML的工具库，在这里有多种方法实现，我也研究并进行了比对，比如与Zeus工具的比对，与Java自身的XML工具库的比对等。在这里，我就描述下我的XStream学习过程和研究结果。

 

**XStream****简单介绍**

 

XStream是一个开源项目，一套简单实用的类库，用于序列化对象与XML对象之间的相互转换。将XML文件内容解析为一个对象或将一个对象序列化为XML文件。

XStream可以用于JDK1.3以上的版本使用，我是在JDK1.5下使用它的。

XStream的相关信息可以到http://xstream.codehaus.org/下查看，它有专门的JavaDoc，可以方便的阅读Xstream的函数及方法。

XStream中主要的类为XStream，它用于序列化对象与XML 对象之间的相互转换。简单的使用它就可以解决很多问题。

XStream中主要的方法也是我用的比较多的是fromXML()和toXML()。

fromXML用于从XML中将对象解析出来。

toXML用于将对象序列化为XML文件。

在XStream中我还使用HierarchicalStreamWriter，HierarchicalStreamReader，createObjectInputStream()，createObjectOutputStream()，主要是用于对象的输入输出。

下面我们来研究下XStream的工作方式。

 

**XStream****的实例——将一个序列化对象转化为XML****对象。**

 

一，创建XStream对象。

XStream xstream=new XStream();

用默认构造器构造了一个名为xstream的XStream的对象。默认构造器所使用XML解析库为Xpp3库，XPP3是一种运行效率非常高的XML全解析实现。

 

二，创建需要序列化的对象。

比如这个类就叫PrintUnit。

构造也比较简单，一个简单的JavaBean

       public class PrintUnit

       {

              Private String a;

              Private String b;

              Private String c;

             

              Public PrintUnit(){}

             

              Public setA(String a)

              {

                     this.a=a;

              }

 

              Public getA()

              {

                     return a;

              }

 

              Public setB(String b)

              {

                     this.b=b;

              }

 

              Public getB()

              {

                     return b;

              }

 

              Public setC(String c)

              {

                     This.c=c;

              }

 

              Public getC()

              {

                     Return c;

              }

       }

 

在例子中使用这个JavaBean。

创建并初始化PrintUnit。

PrintUnit pu=new PrintUnit();

pu.setA("A11");

pu.setB("B22");

pu.setC("C33");

 

三，创建Writer。

创建一个输出流，至于怎么输出我发现可以使用多种方法，其实原理是一样的。

在这里就不得不提到HierarchicalStreamWriter,HierarchicalStreamWriter是一个接口，从字面上意思来说它是有等级的输入流。同样在XStream中也有不少这个接口的实现类用于输出。我现在所用过的有CompactWriter和PrettyPrintWriter这2个。

我是这样做的：

String str="stream.xml"; //本目录下的一个名为stream的XML文件

PrintWriter pw=new PrintWriter(str);//创建一个PrintWriter对象，用于输出。

之后选用一个HierarchicalStreamWriter的实现类来创建输出。

选用CompactWriter创建：

CompactWriter cw=new CompactWriter(pw);

选用PrettyPrintWriter创建：

PrettyPrintWriter ppw=new PrettyPrintWriter(pw);

两者所使用的方法都是很简单的。

CompactWriter与PrettyPrintWriter的区别在于，以CompactWriter方法输出的为连续的没有分隔的XML文件，而用PrettyPrintWriter方法输出的为有分隔有一定格式的XML文件。

以CompactWriter方式生成的XML文件：

 

<object-stream><PrintUnit><a>A11</a><b>B22</b><c>C33</c></PrintUnit></object-stream>

 

以PrettyPrintWriter方式生成的XML文件：

       <object-stream>

             <PrintUnit>

                  <a>A11</a>

                  <b>B22</b>

                  <c>C33</c>

             </PrintUnit>

       </object-stream>

 

       我想大家能很容易的分辨出它们的差异。

 

       四，输出操作      

以上步骤完成后就可以做输出操作了，XStream的输出方式有多种：toXML方式，ObjectOutputStream方式，marshal方式以及一些我尚未发现的一些其它方式。

先说下我所使用的方式它们各自的不同点，从工作原理上说它们是相似的，但是做法各不相同。

toXML()方法，本身toXML的方法就有2种：

第一种:java.lang.String toXML(java.lang.Object obj)

将对象序列化为XML格式并保存到一个String对象中。

第二种:void toXML(java.lang.Object obj, java.io.Writer out)

将对象序列化为XML格式后以Writer输出到某个地方存储。

我所使用的是第二种方式，使用前面已经做好的Pw就可以实现输出，它其实很简单不需要再去做其它定义，只需要一个PrintWriter对象和需要序列化的Object即可。

直接调用xstream.toXML(printUnit,pw);就能输出XML文件,在这里是输出到该目录下的stream.xml中。这里的输出都是覆盖性的，不是末尾添加形式。

使用ObjectOutputStream方式，简单说它就是生成一个对象输出流。

ObjectOutputStream obj_out = xstream.createObjectOutputStream(ppw);

使用XStream的createObjectOutputStream方法创建一个ObjectOutputStream对象，用于XML的输出。这里使用的是PrettyPrintWriter的方式。   之后调用writerObject方法既可，使用方法与其它输出流类似。

obj_out.writeObject(pu);

obj_out.close();

使用marshal方式，其实marshal方法和toXML方法是相同的。在调用toXML方法进行输出时，在XStream内部是需要调用marshal方法的，然后它再去调用对象marshallingStrategy的marshal方法。所以做toXML其实和marshal是相同的，在这里只是想更加说明它的工作方式。

 

使用 void marshal(java.lang.Object obj, HierarchicalStreamWriter writer)方法。

延续上面的例子，在这里可以这样写：xstream.marshal(pu,ppw);

 

需要注意的是，和toXML不同的是参数，一个是PrintWriter对象一个则是PrettyPrintWriter对象。因为marshal中需要

HierarchicalStreamWriter，而PrettyPrintWriter则是实现了HierarchicalStreamWriter接口的实现类。

 

结果和toXML是相同的。

 

五，结果：

       <object-stream>

             <PrintUnit>

                  <a>A11</a>

                  <b>B22</b>

                  <c>C33</c>

             </PrintUnit>

       </object-stream>

 

经过以上5步的操作既可将一个序列化对象转化为XML对象。

 

 

**toXML****内部调用图：**

** **

![XStream.gif]()toXML操作时的内部调用图，自己随意画的。有些没有详细说明。

 

 

**XStream****的实例——将XML****文件转化为一个对象**

 

通过上面的一个例子不难看出XStream简便性，既然有了输出就一定会有输入。

输入方我们将会使用ObjectInputStream。

与输出相同我们需要有一个XStream对象，暂且名为xstream。之后需要读取的XML文件地址目录信息。沿用上面的例子。

String inputStr="xstream.xml";

XStream xstream=new XStream();

我们需要通过对象流进行输入操作，所以需要FileReader和BufferedReader。

FileReader fr=new FileReader(inputStr);

BufferedReader br=new BufferedReader(fr);

创建对象输入流

ObjectInputStream obj_input=xstream.createObjectInputStream(br);

创建对象，还是使用PrintUnit这个对象。

PrintUnit pu2;

通过ObjectInputStream中的readObject()方法将对象从XML文件中读取出来。

pu2=(PrintUnit)obj_input.readObject();

获取值：

System.out.println(pu2.getB());

控制台：

B22 

 

从整个输入的过程来看，是一个文件的读取，将其中的对象数据取出来，然后再对这个对象数据进行操作。内容也比较简单通过ObjectInputStream输入对象。

通过以上的输入输出例子，我想大家应该很容易就能理解XStream是如何实现的。

 

**FomXML**

 

上面使用的是以ObjectInputStream的方式进行XML与对象之间进行转换的。下面我将使用XStream中的fromXML（）方法进行转换。

首先在使用fromXML我发现一个问题，它必须使用正确的解析方式或输出方式对应的输入方式才可以正常解析读取文件，这个问题有点怪，不过确实存在，当我使用前面ObjectOutputStream方式输出的XML文件,用fromXML（）解析读取时，它会报错。

错误信息：

Exception in thread "main" com.thoughtworks.xstream.alias.CannotResolveClassException: object$stream : object$stream

信息内容为：不能解析这个文件。我认为它和输出方式有关，因为上面例子中使用的是ObjectOutputStream，当我反过来做了一个实验后也证明了这一点。

实验大致内容：使用toXML()方法输出XML文件，使用ObjectInputStream解析，发现会在读取的时候抛出CannotResolveClassException异常。

错误信息：

Exception in thread "main" com.thoughtworks.xstream.alias.CannotResolveClassException:

a : a

       因此我认为在解析文件的时候必须先要确定这个文件是由什么方式生成的，然后在解析它，对于使用Dom,Dom4j,XPP等不同方式解析尚未尝试。以上测试是在默认的基础上实验的，默认为XPP3的解析器。

 

       使用fromXML的方法。

      

       public java.lang.Object fromXML(java.lang.String xml)

       public java.lang.Object fromXML(java.io.Reader xml)

public java.lang.Object fromXML(java.lang.String xml,java.lang.Object root)

public java.lang.Object fromXML(java.io.Reader xml,java.lang.Object root)

      

例子：

       PrintUnit puTwo=(PrintUnit)xstream.fromXML(xml);

      

这里的xml必须是使用toXML()生成出来的。对于Reader没有太多的要求。

 

 

**XStream****与****Java.Bean****中****XML****工具的比较******

 

       XStream主要作用是将序列化的对象转化为一个XML文件或将XML文件解析为一个对象。当然并非只有它可以做到，很多其它工具一样可以，在Java中存在这样两个类XMLDecoder和XMLEncoder，它们是在Java.Bean包下的，它们的作用是将JavaBean转化为XML或将XML文件转化为一个Java Bean。

       XMLDecoder是通过一个输入流将对象从输入流中取出并转化为一个实例的方法。它所需要的就是一个输入流及一个转化过程。

 

       XMLDecoder的实例：

 

       String fileStr=”xstream.xml”;//XML文件，在本目录下，延用上次使用文件。

       ObjectInputStream in=new ObjectInputStream(new FileInputStream(fileStr));//创建一个ObjectInputStream用于输入。

       XMLDecoder xmld=new XMLDecoder(in);//创建一个XMLDecoder对象。

       延用前面所使用PrintUnit这个Bean。

       PrintUnit pu=(PrintUnit)xmld.readObject();//通过XMLDecoder中的readObject方法获得PrintUnit对象。

如果获取到了这个对象那么pu中将有它的值a=A11,b=B22,c=C33。整个过程最好放try

…catch中去，能够捕获一些如：文件不存在等异常。

       从操作方式上看XMLDecoder似乎不比XStream差多少，同样是可以通过ObjectInputStream获取XML文件中的对象。它们的差异就是解析的方式不同，XMLDecoder是使用Java自带的XML解析方式，而XStream则是可以自定义的，它可以使用多中方式进行解析。这些是我个人所发现的一些不同点。

 

       XMLEncoder是通过一个输出流将对象序列化并输出为XML文件。它所需要的是一个输出流及一个输出方式。

 

       XMLEncoder的实例：

 

       String fileStr=”xstream.xml”;//定义一个输入的目标文件。

       ObjectOutputStream out=new ObjectOutputStream(new FileOutputStream(fileStr));//创建一个对象输出流。

       XMLEncoder xmle=new XMLEncoder(out);//创建一个XMLEncoder对象。

       延用前面所使用PrintUnit这个Bean。

//创建并初始化PrintUnit对象。

PrintUnit pu=new PrintUnit();

pu.setA(“AAA”);

pu.setB(“BBB”);

pu.setC(“CCC”);

 

       xmle.writeObject(pu);//使用XMLEncode的writeObject方法输出pu

       xmle.flush();//刷新

       xmle.close();//关闭输出流

 

       从上面的代码不难看出，使用XMLEncode方式将对象序列化并输出也是很方便的，简单调用writeObject方法能将普通Bean输出为XML文件。

      

       XML文件的内容：

 

�_ <?xml version="1.0" encoding="UTF-8"?>

<java version="1.5.0" class="java.beans.XMLDecoder">

 <object class="test.PrintUnit">

  <void property="a">

   <string>AAA</string>

  </void>

  <void property="b">

   <string>BBB</string>

  </void>

  <void property="c">

   <string>CCC</string>

  </void>

 </object>

w   </java>

 

       不知道是我哪里没有处理，还是实际并不是像我想象的哪么简单，使用XMLEncoder所输出的XML文件中有一定的问题，虽然它很详细，比起XStream所生成的更多，包括了XML和Java的版本看上去更像是个完整的XML文件，不过再细看它们两生成的XML格式内容，完全不同，这个我想就是它们最大的区别。这让我想到了很多内容：工作方式，解析器，转换方式等。大家有没发现在开始和结束都存在一些乱码数据，难道在XMLEncoder输出过程中或数据转换中内容已经存在“脏”数据了？还是我所使用的输出方式存在问题？哎…一个又一个问题出现了。我想我需要再进一步的研究和学习才能得到答案。

       不过尽管有这个那个的问题，使用Java本身自带的XML工具还是一样很实用的，读取和输出一样可用，操作也很灵活。因此我觉得在某些场合使用特定的工具可能会更好，利用XMLEncoder和XMLDecoder同样可以解决一些问题。

我的这个使用XMLDecoder和XMLEncoder的序列化格式输出暂研究到这里。
枫情·太子爷
2005年12月16日

发表于 2005-12-16 16:22 [枫情·太子爷](http://www.blogjava.net/zlkn2005/) 阅读(3625) [评论(4)](http://www.blogjava.net/zlkn2005/archive/2005/12/16/24240.html#Post)  [编辑](http://www.blogjava.net/zlkn2005/admin/EditPosts.aspx?postid=24240)  [收藏](http://www.blogjava.net/zlkn2005/AddToFavorite.aspx?id=24240)

 
![]()

[]()

[]()

[评论]()

[]()

[]()

[]()[#](http://www.blogjava.net/zlkn2005/archive/2005/12/16/24240.html#24242 "permalink: re: XStream 学习笔记") []()re: XStream 学习笔记  [回复](http://www.blogjava.net/zlkn2005/archive/2005/12/16/24240.html#post)  [更多评论](http://www.blogjava.net/comment?author=fanta "查看该作者发表过的评论")  []()  []()
好

[fanta](http://www.blogjava.net/fanscial/) 评论于 2005-12-16 16:45
[#](http://www.blogjava.net/zlkn2005/archive/2005/12/16/24240.html#24616 "permalink: re: XStream 学习笔记") []()re: XStream 学习笔记  [回复](http://www.blogjava.net/zlkn2005/archive/2005/12/16/24240.html#post)  [更多评论](http://www.blogjava.net/comment?author=xmlspy2004 "查看该作者发表过的评论")  []()  []()

还有很多问题，比如：
当bean的字段为int类型，如果这个字段没有值，它默认输出是0
而实际应用中我们需要的是<intField></intField>.
类似的问题还很多，都需要你去处理。
但处理来处理去，你会发现，自己做的修改很多，还不如自己手动写xml的String。
[xmlspy2004]() 评论于 2005-12-19 12:42

[#](http://www.blogjava.net/zlkn2005/archive/2005/12/16/24240.html#183462 "permalink: re: XStream 学习笔记") []()re: XStream 学习笔记  [回复](http://www.blogjava.net/zlkn2005/archive/2005/12/16/24240.html#post)  [更多评论](http://www.blogjava.net/comment?author=%e5%bf%83%e6%97%a0%e7%97%95 "查看该作者发表过的评论")  []()  []()

呵呵.在JAVA里面,INT类型的变量本来默认值就是0,你没有给它值,它也会默认将0作为这个变量的初始值....@xmlspy2004
[心无痕](http://www.blogjava.net/xinwuhen/) 评论于 2008-03-03 14:39
[#](http://www.blogjava.net/zlkn2005/archive/2005/12/16/24240.html#200051 "permalink: re: XStream 学习笔记") []()re: XStream 学习笔记[]()  [回复](http://www.blogjava.net/zlkn2005/archive/2005/12/16/24240.html#post)  [更多评论](http://www.blogjava.net/comment?author=jadar "查看该作者发表过的评论")  []()  []()

出现：Exception in thread "main" com.thoughtworks.xstream.alias.CannotResolveClassException:
的可能原因是:调用xStream.alias("PrintUnit ",PrintUnit.class) 时写错了，
尤其是第一个参数，要跟xml中的大小写一致！
good Luck！
[jadar]() 评论于 2008-05-12 16:52
 

[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  
[]() [找优秀程序员，就在博客园](http://job.cnblogs.com/)
[网易有道诚聘CRM研发工程师](http://job.cnblogs.com/offer/12368/)
[锦江国际诚聘Java高级软件工程师](http://job.cnblogs.com/offer/11576/)
[福州几维网络诚聘Java服务端程序员](http://job.cnblogs.com/offer/12493/)
IT新闻：
· [百度LOGO控烟日变脸：李彦宏签名倡导无烟](http://news.cnblogs.com/n/103732/)
· [高毛利驱动线上线下T恤战：B2C卖19元成本10元](http://news.cnblogs.com/n/103731/)
· [美女子森林中用iPhone意外拍到大脚野人视频](http://news.cnblogs.com/n/103730/)
· [欧盟延长2起并购案交易审查期限：涉及希捷等](http://news.cnblogs.com/n/103727/)
· [联通呼吁成立操作系统联盟 三大运营商有望联手](http://news.cnblogs.com/n/103726/)   [博客园](http://www.cnblogs.com/)  [博问](http://home.cnblogs.com/q/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/zlkn2005/archive/2005/12/16/24240.html&SourceURL=/zlkn2005/archive/2005/12/16/24240.html)       [使用Ctrl+Enter键可以直接提交]    推荐职位：
· [[急聘] ASP.NET 软件高级开发工程师(北京网笑信息)](http://job.cnblogs.com/offer/12638/)
· [ASP.NET中高级软件工程师(联展壹步科技)](http://job.cnblogs.com/offer/11670/)
· [中高级前端网页设计师(联展壹步科技)](http://job.cnblogs.com/offer/13073/)
· [网页美工设计(联展壹步科技)](http://job.cnblogs.com/offer/11671/)
· [北京.NET 研发工程师 (北京捷报数据)](http://job.cnblogs.com/offer/5914/)
· [(北京).NET软件开发工程师(北京龙达)](http://job.cnblogs.com/offer/12527/)
· [厦门高级.NET软件工程师(服务于美国Amazon)](http://job.cnblogs.com/offer/11584/)
· [高级Web页面前端开发工程师(新蛋中国)](http://job.cnblogs.com/offer/10723/)

博客园首页随笔：
· [一个网站的诞生- MagicDict开发总结5 [检索候补列表的生成]](http://www.cnblogs.com/TextEditor/archive/2011/05/31/2063172.html)
· [基于Silverlight的快速开发框架RapidSL之开源](http://www.cnblogs.com/guozili/archive/2011/05/31/2063957.html)
· [你不可不知的Mango — 开发者篇（3）](http://www.cnblogs.com/twodays/archive/2011/05/31/whats-new-in-mango-for-dev-3.html)
· [巧用Silverlight之Path](http://www.cnblogs.com/Wendy_Yu/archive/2011/05/31/2063720.html)
· [60款很酷的 jQuery 幻灯片演示和下载](http://www.cnblogs.com/lhb25/archive/2011/05/31/2056103.html)
知识库：
· [谁做了程序员眼中的程序员](http://kb.cnblogs.com/page/100991/)
· [复杂是大敌](http://kb.cnblogs.com/page/100468/)
· [告诉你一些DBA求职面试技巧](http://kb.cnblogs.com/page/100657/)
· [模型驱动开发 —在RUP与Agile之间找到平衡点](http://kb.cnblogs.com/page/100350/)
· [总结一下领域模型的验证（附代码下载）](http://kb.cnblogs.com/page/103569/) 最简洁阅读版式：
[XStream 学习笔记](http://archive.cnblogs.com/b/24240/) 网站导航:

[博客园](http://www.cnblogs.com/ "程序员的网上家园")   [IT新闻](http://news.cnblogs.com/)   [知识库](http://kb.cnblogs.com/)   [博客生活](http://www.cnweblog.com/)   [IT博客网](http://www.cnitblog.com/)   [C++博客](http://www.cppblog.com/)   [博问](http://space.cnblogs.com/q/ "IT问答")   [管理](http://www.blogjava.net/zlkn2005/archive/2005/12/16/24240.html?opt=admin)   
