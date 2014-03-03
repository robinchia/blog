---
layout: post
title: "jsp-include与include的区别"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_常用
--- 

# jsp:include与include的区别

（题外话：引入的文件如果是jsp则应定义为***.jspf文件，如果其他文件可定义为***.inc文件，即include file。而且<%@ inlucde page="" %>除了可以引jspf还可以引servlet——很重要）
近日做一项目要用到JSP动态包含JSP，本想肯定很简单，但不想这么复杂，而且目前还没有求到好的答案，问题如下：
#文件：one.jsp
1

2
3 <%!

String var1=

"China"

;
%>
#文件 two.jsp
1

2
3

4
 <%!

String var1=

"America"

;
String var2=

"England"

;

%>
#文件 three.jsp
1

2
3

4
5

6
7

8
9

10
11

12
 <%

int j=1;
if

(j==1){

%>
<%@ include file=

"one.jsp"

%>

<%
}

else

{

%>
<%@ include file=

"two.jsp"

%>

<%}%>
<%=var1%>

<%=var2%>
执行three.jsp会出什么结果？
a.编译错误
b.显示China England
很多人理所当然的觉得肯定是a,因为j=1所以只包含one.jsp,two.jsp不会包含进来，但答案是b,上机测试就知道。
为什么？
因为@include要先于jsp的其他代码执行，所以两个文件都会被包含进来！
－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－
如果你说用jsp:include可以解决问题，好，把three.jsp改成如下：
＃文件 three.jsp
1

2
3

4
5

6
7

8
9

10
11 <%

int j=1;
String includeFile=

""

;

if

(j==1){
includeFile =

"one.jsp"

;

}

else

{
includeFile =

"two.jsp"

;

}
%>

<jsp:include page=′<%=includeFile%>′ />
<%=var1%>
－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－
结果是什么？
a.编译错误
b.显示 China
是b吗，不，是a,编译错误！提示var1未定义。
为什么？因为jsp:include是动态包含，相当于把包含文件与被包含文件分开编译。
－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－
现把include的问题总结如下：
【引用 gfzhx(小小)的话】
    动态包含相当于作了一个页面跳转，也就是相当于重新访问了另一个servlet，所以当然会提示变量没有定义，你想一想，一个类中申明了一个变量，第二个类直接是用这个变量，你说可以吗？其实你的情况和这个例子很像，这就是动态包含，而静态包含你可以看一看jsp编译后的java文件就知道了，它是直接将你包含的页面直接包含进去，然后再编译的。所以你的问题采用静态包含就可以了。不过不管怎么，还是不推荐采用这种形式，会给程序造成很多问题，至少比较难以维护了，可以说是一种不好的编程风格。建议采用其他方法解决问题。
【引用 xiao_yuer(小鱼儿)的话】
要使用引入文件中定义的变量，只能用@include指令。
也就是<%@ include file="one.jsp" %>,但这在一般情况下都不是动态的，是在jsp页面第一次编译时，把它导入的。而jsp编译后，这两个文件再作修改很多jsp服务器都不会侦测到，因为包含这两的jsp的jsp文件本身并没有发生变化。但很奇怪，weblogic6好像可以。你可以试试，不过不要抱太大希望，因为你这种要求不是很合理。向你这种情况，完全应该引入一个java类，这个类中定义一些变量（按你的说法都应该算是常量了，jsp取出来直接用而不会修改它再存回去），然后再jsp中得到那个类的实例，来进行处理。那样如果你要修改这些常量的值，就修改java类，而不用修改jsp.
【自己的:-))】
@include包含是静态包含，是把被包含文件加入到包含文件中然后进行编译，所以这种包含与解释执行的语言很象（例如php)，而且JSP中@开头的语句都要先于其他语句执行，所以如上，用if.else来判断然后包含是不行的，所以以前如果是做PHP这种解释语言的人会觉得不适应。
jsp:include是既可以静态包含又可以动态包含，与@include不同的是，jsp:include没有@include那样的优先权，即不是现于其他语句执行的，所以jsp:include可以又选择性的包含。不过更重要的一点是，用jsp:include相当于编译两个不同的文件，所以如果被包含文件中仅仅是显示某些东西（例如被包含文件是纯HTML）的话，这种情况下，用jsp:include和@include来包含文件的效果是一样的，但如果要用jsp:include来显示被包含文件中定义的变量就不行了（为什么？见上面的引用吧，就不赘述了）。
【感谢】
gfzhx(小小)、xiao_yuer(小鱼儿)
来源： <[http://www.wudongqi.com/article/529.htm](http://www.wudongqi.com/article/529.htm)> 
