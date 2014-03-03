---
layout: post
title: "DWR如何获得返回对象 list Map Set list"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_集合
--- 

# DWR如何获得返回对象 list Map Set list

**DWR****如何获得返回对象****list Map Set list.add(JavaBean)**

**1****、调用没有返回值和参数的JAVA方法**

1.1、dwr.xml的配置
<dwr>
<allow>
<create creator="new" javascript="testClass" >
<param name="class" value="/com.dwr.TestClass" />
<include method="testMethod1"/>
</create>
</allow>
</dwr>
<allow>标签中包括可以暴露给javascript访问的东西。
<create>标签中指定javascript中可以访问的java类，并定义DWR应当如何获得要进行远程的类的实例。creator="new"属性指定java类实例的生成方式，new意味着DWR应当调用类的默认构造函数来获得实例，其他的还有spring方式，通过与IOC容器Spring进行集成来获得实例等等。javascript=" testClass "属性指定javascript代码访问对象时使用的名称。
<param>标签指定要公开给javascript的java类名。
<include>标签指定要公开给javascript的方法。不指定的话就公开所有方法。
<exclude>标签指定要防止被访问的方法。
1.2、javascript中调用
首先，引入javascript脚本
<script src='dwr/interface/ testClass.js'></script>
<script src="/dwr/engine.js"></script>
<script src="/dwr/util.js"></script>
其中TestClass.js是dwr根据配置文件自动生成的，engine.js和util.js是dwr自带的脚本文件。
其次，编写调用java方法的javascript函数
Function callTestMethod1(){
       testClass.testMethod1();
}
**2****、调用有简单返回值的java方法**

**2.1****、dwr.xml的配置********配置同1.1
<dwr>
<allow>
<create creator="new" javascript="testClass" >
<param name="class" value="/com.dwr.TestClass" />
<include method="testMethod2"/>
</create>
</allow>
</dwr>
2.2、javascript中调用
首先，引入javascript脚本
其次，编写调用java方法的javascript函数和接收返回值的回调函数
Function callTestMethod2(){
       testClass.testMethod2(callBackFortestMethod2);
}
Function callBackFortestMethod2(data){
      //其中date接收方法的返回值
      //可以在这里对返回值进行处理和显示等等
alert("the return value is " + data);
}
其中callBackFortestMethod2是接收返回值的回调函数**

**3****、调用有简单参数的java方法**

**3.1****、dwr.xml的配置********配置同1.1
<dwr>
<allow>
<create creator="new" javascript="testClass" >
<param name="class" value="/com.dwr.TestClass" />
<include method="testMethod3"/>
</create>
</allow>
</dwr>
3.2、javascript中调用
首先，引入javascript脚本
其次，编写调用java方法的javascript函数
Function callTestMethod3(){
                  //定义要传到java方法中的参数
       var data;
       //构造参数
       data = “test String”;
       testClass.testMethod3(data);
}**

**4****、调用返回JavaBean的java方法****4.1****、dwr.xml的配置****<dwr>
<allow>
<create creator="new" javascript="testClass" >
<param name="class" value="/com.dwr.TestClass" />
<include method="testMethod4"/>
</create>
<convert converter="bean" match=""com.dwr.TestBean">
                   <param name="include" value="username,password" />
</convert>
</allow>
</dwr>
<creator>****标签负责公开用于Web远程的类和类的方法，<convertor>标签则负责这些方法的参数和返回类型。convert元素的作用是告诉DWR在服务器端Java 对象表示和序列化的JavaScript之间如何转换数据类型。DWR自动地在Java和JavaScript表示之间调整简单数据类型。这些类型包括Java原生类型和它们各自的封装类表示，还有String、Date、数组和集合类型。DWR也能把JavaBean转换成JavaScript 表示，但是出于安全性的原因，要求显式的配置，<convertor>标签就是完成此功能的。converter="bean"属性指定转换的方式采用JavaBean命名规范，match=""com.dwr.TestBean"属性指定要转换的javabean名称，<param>标签指定要转换的JavaBean属性。
4.2、javascript中调用
首先，引入javascript脚本
其次，编写调用java方法的javascript函数和接收返回值的回调函数
Function callTestMethod4(){
       testClass.testMethod4(callBackFortestMethod4);
}
Function callBackFortestMethod4(data){
      //其中date接收方法的返回值
//对于JavaBean返回值，有两种方式处理
              //不知道属性名称时，使用如下方法
            for(var property in data){
               alert("property:"+property);
               alert(property+":"+data[property]);
            }
//知道属性名称时，使用如下方法
            alert(data.username);
            alert(data.password);
}
其中callBackFortestMethod4是接收返回值的回调函数**

**5****、调用有JavaBean参数的java方法****5.1****、dwr.xml的配置
****配置同4.1
<dwr>
<allow>
<create creator="new" javascript="testClass" >
<param name="class" value="/com.dwr.TestClass" />
<include method="testMethod5"/>
</create>
<convert converter="bean" match="com.dwr.TestBean">
                   <param name="include" value="username,password" />
</convert>
</allow>
</dwr>
5.2、javascript中调用
首先，引入javascript脚本
其次，编写调用java方法的javascript函数
Function callTestMethod5(){
                  //定义要传到java方法中的参数
       var data;
       //构造参数，date实际上是一个object
       data = { username:"user", password:"password" }
       testClass.testMethod5(data);
}**

**6****、调用返回List、Set或者Map的java方法****6.1****、dwr.xml的配置********配置同4.1
<dwr>
<allow>
<create creator="new" javascript="testClass" >
<param name="class" value="/com.dwr.TestClass" />
<include method="testMethod6"/>
</create>
<convert converter="bean" match="com.dwr.TestBean">
<param name="include" value="username,password" />
</convert>
</allow>
</dwr>
注意：如果List、Set或者Map中的元素均为简单类型（包括其封装类）或String、Date、数组和集合类型，则不需要<convert>标签。
6.2、javascript中调用(以返回List为例，List的元素为TestBean)
首先，引入javascript脚本
其次，编写调用java方法的javascript函数和接收返回值的回调函数
Function callTestMethod6(){
       testClass.testMethod6(callBackFortestMethod6);
}
Function callBackFortestMethod6(data){
      //其中date接收方法的返回值
//对于JavaBean返回值，有两种方式处理
              //不知道属性名称时，使用如下方法
            for(var i=0;i<data.length;i++){
for(var property in data){
                   alert("property:"+property);
                   alert(property+":"+data[property]);
                }
}
//知道属性名称时，使用如下方法
for(var i=0;i<data.length;i++){
                alert(data.username);
                alert(data.password);
}
}**

**7****、调用有List、Set或者Map参数的java方法**

**7.1****、dwr.xml的配置****<dwr>
<allow>
<create creator="new" javascript="testClass" >
<param name="class" value="/com.dwr.TestClass" />
<include method="testMethod7"/>
</create>
<convert converter="bean" match="com.dwr.TestBean">
<param name="include" value="username,password" />
</convert>
</allow>
<signatures>
<![CDATA[
import java.util.List;
import com.dwr.TestClass;
import com.dwr.TestBean;
TestClass.testMethod7(List<TestBean>);
]]>
</signatures>
</dwr>
<signatures>****标签是用来声明java方法中List、Set或者Map参数所包含的确切类，以便java代码作出判断。
7.2、javascript中调用(以返回List为例，List的元素为TestBean)
首先，引入javascript脚本
其次，编写调用java方法的javascript函数
Function callTestMethod7(){
//定义要传到java方法中的参数
       var data;
       //构造参数，date实际上是一个object数组，即数组的每个元素均为object
data = [
                       {
                          username:"user1",
                          password:"password2"
                       },
                       {
                          username:"user2",
                          password:" password2"
                       }
                   ];
       testClass.testMethod7(data);
}
注意：
1、对于第6种情况，如果java方法的返回值为Map，则在接收该返回值的javascript回调函数中如下处理：
function callBackFortestMethod(data){
            //其中date接收方法的返回值
            for(var property in data){
                   var bean = data[property];
                   alert(bean.username);
                   alert(bean.password);
               }
}
2、对于第7种情况，如果java的方法的参数为Map（假设其key为String，value为TestBean），则在调用该方法的javascript函数中用如下方法构造要传递的参数：
function callTestMethod (){
               //定义要传到java方法中的参数
               var data;
               //构造参数，date实际上是一个object，其属性名为Map的key，属性值为Map的value
               data = {
                          "key1":{
                              username:"user1",
                             password:"password2"
                          },
                          "key2":{
                             username:"user2",
                             password:" password2"
                          }
                      };
               testClass.testMethod(data);
}
并且在dwr.xml中增加如下的配置段
<signatures>
<![CDATA[
import java.util.List;
import com.dwr.TestClass;
import com.dwr.TestBean;
TestClass.testMethod7(Map<String,TestBean>);
]]>
</signatures>
3、由以上可以发现，对于java方法的返回值为List(Set)的情况，DWR将其转化为Object数组，传递个javascript；对于java方法的返回值为Map的情况，DWR将其转化为一个Object，其中Object的属性为原Map的key值，属性值为原Map相应的value值。
4、如果java方法的参数为List(Set)和Map的情况，javascript中也要根据3种所说，构造相应的javascript数据来传递到java中。**

**======================================================================================**

[]()**Scripting Introduction**

**DWR****根据dwr.xml生成和Java代码类似的Javascript代码。**

**相对而言Java同步调用，创建与Java代码匹配的Ajax远程调用接口的最大挑战来至与实现Ajax的异步调用特性。**

**DWR****通过引入回调函数来解决这个问题，当结果被返回时，DWR会调用这个函数。**

**有两种推荐的方式来使用DWR实现远程方法调用。可以通过把回调函数放在参数列表里，也可以把回调函数放到元数据对象里。**

**当然也可以把回调函数做为第一个参数，但是不建议使用这种方法。因为这种方法在处理**[自动处理http对象](http://wiki.javascud.org/display/dwrcn/Accessing+Servlet+Objects "Accessing Servlet Objects")**时(查看"Alternative Method")上会有问题。这个方法主要是为向下兼容而存在的。**

[]()**简单的回调函数**

**假设你有一个这样的Java方法：**

**public class Remote {    public String getData(int index) { ... }}**

**我们可以在Javascript中这样使用：**

**<script type="text/javascript"    src="[WEBAPP]/dwr/interface/Remote.js"> </script><script type="text/javascript"    src="[WEBAPP]/dwr/engine.js"> </script>...function handleGetData(str) {  alert(str);}Remote.getData(42, handleGetData);**

**42****是Java方法getData()的一个参数。**

**此外你也可以使用这种减缩格式：**

**Remote.getData****(42, function(str) { alert(str); });**

[]()**调用元数据对象(Meta-Data)**

**另外一种语法时使用"调用元数据对象"来指定回调函数和其他的选项。上面的例子可以写成这样：**

**Remote.getData****(42, {  callback:function(str) { alert(str); }});**

**这种方法有很多优点：易于阅读，更重要的指定额外的调用选项。**

[]()**超时和错误处理**

**在回调函数的元数据中你可以指定超时和错误的处理方式。例如：**

**Remote.getData****(42, {  callback:function(str) { alert(str); },  timeout:5000,  errorHandler:function(message) { alert("Oops: " + message); }});**

[]()**查找回调函数**

**有些情况下我们很难区分各种回调选项(记住，Javascript是不支持函数重载的)。例如：**

**Remote.method****({ timeout:3 }, { errorHandler:somefunc });**

**这两个参数之一是bean的参数，另一个是元数据对象，但是我们不能清楚的告诉DWR哪个是哪个。为了可以跨浏览器，我们假定null == undefined。 所以当前的情况，规则是：**

* **如果第一个或最后一个是一个函数，那么它就是回调函数，没有元数据对象，并且其他参数都是Java的方法参数。**
* **另外，如果最后一个参数是一个对象，这个对象中有一个callback成员，并且它是个函数，那么这个对象就是元数据对象，其他的都是Java方法参数。**
* **另外，如果第一个参数是 null ，我们就假设没有回调函数，并且其他的都是Java方法参数。尽管如此，我们会检查最后一个参数是不是null，如果是就发出警告。**
* **最后如果最后一个参数是null，那么就没有callback函数。**
* **另外，发出错误信号是个糟糕的请求格式。**

[]()**创造一个与Java对象匹配的Javascript对象**

**假设你有这样的Java方法：**

**public class Remote {  public void setPerson(Person p) {    this.person = p;  }}**

**Person****对象的结构是这样的：**

**public Person {  private String name;  private int age;  private Date[] appointments;  // getters and setters ...}**

**那么你可以在Javascript中这样写：**

**var****p = {  name:"Fred Bloggs",  age:42,  appointments:[ new Date(), new Date("1 Jan 2008") ]};Remote.setPerson(p);**

**在Javascript没有出现的字段，在Java中就不会被设置。**

**因为setter都是返回'void'，我们就不需要使用callback函数了。如果你想要一个返回void的服务端方法的完整版，你也可以加上callback函数。很明显DWR不会向它传递任何参数。**

 
