---
layout: post
title: "JQuery 学习总结及实例"
categories: JS用法
tags: 
 - JS用法
 - Jquery
--- 

# JQuery 学习总结及实例

# [JQuery 学习总结及实例](http://www.cnblogs.com/daomul/archive/2013/04/26/3037737.html)

1、JQuery简介
      普通JavaScript的缺点：每种控件的操作方式不统一，不同浏览器下有区别，要编写跨浏览器的程序非常麻烦。因此出现了很多对JavaScript的封装库，比如Prototype、Dojo、ExtJS、JQuery等，这些库对JavaScript进行了封装，简化了开发。这些库是对JavaScript的封装，也就是咱们调用JQuery的一句函数，JQuery内部这句函数帮我们调用JavaScript中的代码几十句，因为JQuery就是JavaScript语法写的一些函数类，内部仍然是调用JavaScript实现的，所以并不是代替JavaScript的。使用JQuery的代码、编写JQuery的扩展插件等仍然需要JavaScript的技术，Jquery本身就是一堆JavaScript函数。

 
           （1、Jquery是最火的JavaScript库，已集成到VS2010，MS的Ajax toolkit和JQuery结合也是最方便，JQuery的扩展插件也是非常多。

           （2、JQuery的优点：尺寸小、使用简单方便（Write Less, Do More，吃得少干得多。

                     链式编程（$("#div1").draggble().show().hide().fly()）、
                     隐式迭代  （自动对于多个元素进行迭代方法调用））、

                    屏蔽浏览器差异跨浏览器兼容性好（IE 6.0+, FF 2+, Safari 3.0+, Opera 9.0+, Chrome）、插件丰富、 开源、免费。
           （3、VS中JavaScript、JQuery的自动完成功能：在VS2010中直接有，VS008需要安装VisualStudio 和VS90SP1-KB958502-x86补丁会更强更好用, 下

                   载地址见备注。然后引用jquery-1.4.1.js，jquery-1.4.1-vsdoc.js放到同目录下，不需要在页面引用。
           （4、vsdoc是vs2008sp1以后增加的一个技术，将js文件对应的vsdoc（相当于js库提供的方法的说明库）放到和js一起，就有会第三方js的自动提示的功能

 
2、简单的JQuery之Ready

 
     （1、注册事件的函数，和普通的dom不一样，不需要在元素上标记on**这样的事件。

$(document).ready(function(){

           alert("加载完毕！");
       });

（2、当页面Dom元素加载完毕时执行代码，可以简写为：
        $(function(){

           alert("加载完毕！");
      });

      （3、和onload类似，但是onload只能注册一次（没有C#中的+=机制），后注册的取代先注册的，而ready则可以多次注册都会被执行。
                    window.onload=function(){alert("1")};window.onload=function(){alert("2")};//结果只弹出2

      （4、JQuery的ready和Dom的onload的区别(*)：onload是所有Dom元素创建完毕、图片、Css等都加载完毕后才被触发，而ready则是Dom元素创建完毕后就被触发，这样可以提高网页的响应速度。在jQuery中也可以用$(window).load()来实现onload那种事件调用的时机。
           $(function(){alert("1111");});//简写方式

 
3、JQuery的函数

    $.map(array,fu) 得到函数的返回值和$.each(array,fn)调用函数处理没有返回值

![]()![]()map函数和each函数

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 6 <script type="text/javascript"> 7 /*$.map(array,fu) 8         var arr1 = [1, 2, 3]; 9         var arrOne = $.map(arr1, function (item) { return item * 2; });10         alert(arrOne);*/11 12 /*$.each(array,fn)*/13 var arr2 = [1, 2, 3];14 //$.each(arr2, function (key, item) { alert(key+" and "+item); });//两个参数返回的是key+值15 //$.each(arr2, function (item) { alert(item); });//一个参数返回的是key16         $.each(arr2, function () { alert(this); });//没有参数自觉返回值17 </script>18 </head>19 <body>20 21 </body>22 </html>

4、JQuery对象和Dom对象
         （1、$('#div1')得到的就是jQuery对象，jQuery对象只能调用jQuery对象封装的方法，不能调用Dom对象的方法，Dom对象也不能调用jQuery对象的方法，所

                 以 alert($('#div1').innerHTML是错的，因为innerHTML是DOM对象的属性。
         （2、Array是JS语言本身的对象，不是Dom对象，因此不需要转换为Jquery对象才能用

         （3、将Dom对象转换为JQuery对象的方法，$(dom对象)；当调用jQuery没有封装的方法的时候必须用Dom对象，转换方法：vardomobj = jqobj[0]或者
                  vardomobj=jqobj.get(0)

         （4、在引用外部js的Script标签内不能再写js代码，引用外部js的Script标签也不能用简写方法闭合。

![]()![]()JQuery对象和Dom对象

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 6 <script type="text/javascript"> 7 /*JQuery 对象*/ 8 //$(function () { alert($('#div1').html()); }); 9 10 /*Dom对象转化为JQuery对象*/11 //$(function () { var dom1 = document.getElementById("div1"); $(dom1).html("<font color='black'>测试black</font>"); });12 13 /*JQuery对象转化为Dom对象 +一个[0]*/14 //$(function () { var jquery1 = $(div1)[0]; alert(jquery1.innerHTML); });15 16 /*JQuery 修改样式css 还有val*/17 //$(function () { $('#div1').css("background", "gray"); });//css两个参数是设置值18 //$(function () { alert($('#div1').css("background")); }); //css一个参数是取值19 //$(function () { $('#myInput').val(new Date()); }); //val一个参数是设置值20 //$(function () { alert($('#myInput').val()); }); //val没有参数是取值21 22 </script>23 </head>24 <body>25 <div id="div1">26 <font color="red">测试red</font>27 <input type="text" name="name" value="kong" id="myInput" />28 </div>29 </body>30 </html>

5、JQuery 选择器

    （1、$(“#div1”).html();

    （2、$("TagName")来获取所有指定标签名的jQuery对象，相当于getElementsByTagName：

               例如获得所有的P：$("p").html("我们都是P");

     （3、标签选择器，拥有样式的标签选择器
             ☆ 多条件选择器：$("p,div,span.menuitem")，同时选择p标签、div标签和拥有menuitem样式的span标签元素（类似于CSS选择器）

             ☆ 注意选择器表达式中的空格不能多不能少。易错！
             ☆ 层次选择器：

☆☆$("div li")获取div下的所有li元素（后代，子、子的子……）
☆☆$("div > li")获取div下的直接li子元素

☆☆$(".menuitem +div")获取样式名为menuitem之后的第一个div元素（不常用）
☆☆$(".menuitem ~div")获取样式名为menuitem之后所有的div元素（不常用）
    

 案例1：

![]()![]()JQuery选择器1

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 6 7 <script type="text/javascript"> 8         $(function () { 9             $('.test').click(10 function () {11                     alert($(this).text());12                 }13              ); 14         });15 </script>16 <style type="text/css"> 17          .test{ background-color:Red} 18 </style>19 </head>20 <body>21 <div id="div1">22 <p class="test">23             test1</p>24 <p class="test">25             test2</p>26 <p class="test">27             test3</p>28 </div>29 </body>30 </html>

6、JQuery的迭代
      如何判断对象是否存在，jQuery选择器返回的是一个对象数组(数组中的每个对象还是Dom对象)，调用text()、html()、click()之类方法的时候其实是对数组中每个元素迭代调用每个方法，因此即使通过id选择的元素不存在也不会报错，如果需要判断指定的id是否存在，应该写：

 
if($("#btn1").length <= 0) {

                alert("id为btn1的元素不存在！");
}
 7、节点遍历

        （1、  next() 方法用于获取节点之后的挨着的第一个同辈元素， 
        （2、$(".menuitem").next("div") 、 nextAll() 方法用于获取节点之后的所  有同辈元素， $(".menuitem").nextAll("div")  prev 、 prevAll 之前的元素   
        （3、siblings() 方法用于获取所有同辈元素，$(".menuitem").siblings("li")   siblings 、next 等所有能传递选择器的地方能够使用的语法都和 $() 语法一样。

![]()![]()节点遍历

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 6 <script type="text/javascript"> 7 /*next() 和nextAll()的用法 8         $(function () { 9             $("div").click(function () {10             //alert($(this).next().text());11             //alert($(this).next("p").text());12             //alert($(this).nextAll().text());13             //alert($(this).nextAll("div").text());14         });15         });*/16 /*高亮显示所以next*/17 //$(function () { $("div").click(function () { $(this).nextAll("div").css("background","red"); }); });18 /*高亮显示自己而已*/19 //$(function () { $("div").click(function () { $(this).css("background", "red"); $(this).siblings().css("background","white"); }); });20         $(function () { $("div").click(function () { $(this).css("background", "red").siblings().css("background", "white"); }); });21 22 23 </script>24 </head>25 <body>26 <div>27         aa</div>28 <div>29         bb</div>30 <div>31         cc</div>32 <p>33         p1</p>34 <p>35         p2</p>36 <div>37         dd</div>38 <div>39         ee</div>40 </body>41 </html>

8、链式编程

      链式编程就是对象一棒棒向下传，能不能正确传下来要看返回值，html()不能传，prevAll().nextAll()也传错。

               $("#tableRating td").click(function() {$(this).prevAll().next().html("a"); });//经典！

案例1：样式的增删：addClass和removeClass
![]()![]()添加样式和移除样式

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <style type="text/css"> 6          .menuitem{background-color:Yellow; } 7          .highlight { background-color: Red;} 8 </style> 9 <script src="js/jquery-1.4.2.js" type="text/javascript"></script>10 <script type="text/javascript">11         $(function () { $(".menuitem").click(function () { $(this).addClass("highlight").siblings().removeClass("highlight"); }); });12 13 </script>14 </head>15 <body>16 <p class="menuitem">111111</p><br /> 17 <p class="menuitem">111111</p><br /> 18 <p class="menuitem">111111</p><br />19 </body>20 </html>

案例2：五角星评分

![]()![]()五角星评分

1 /* 2             $(function() { 3             $("#ratings td").html("<img src='images/star_off.gif' />") 4             .mouseover(function() { $("#ratings td").html("<img src='images/star_on.gif' />"); $(this).nextAll().html("<img src='images/star_off.gif' />"); }); 5 6             }); 7             */ 8 9             $(function() { 10             $("#ratings td").html("<img src='images/star_off.gif' />") 11                 .mouseover(function() { $("#ratings td").html("<img src='images/star_on.gif' />") 12                 .siblings().html("<img src='images/star_on.gif' />"); 13                 $(this).nextAll().html("<img src='images/star_off.gif' />"); 14             }); 15 16             }); 17 18 19 <table id=ratings> 20 <tr><td></td><td></td><td></td><td></td><td></td></tr> 21 </table>

 9、基本过滤选择器 
           （1、:first  选取第一个元素。 $("div:first") 选取第一个 <div> 
           （2、:last  选取最后一个元素。 $("div:last") 选取最后一个 <div> 
           （3、:not( 选择器 )  选取不满足 " 选择器 " 条件的元素， 
                      $("input:not(.myClass)") 选取样式名不是 myClass 的 <input> 
           （4、:even 、 :odd ，选取索引是奇数、偶数的元素： $("input:even") 选 
                      取索引是奇数的 <input> 
           （5、:eq( 索引序号 ) 、 :gt( 索引序号 ) 、 :lt( 索引序号 )  选取索引等于、大于、小于索引序号的元素，比如 $("input:lt(5)") 选取索引小于 5 的 <input>         

           （6、$(":header") 选取所有的 h1 …… h6 元素（ * ） 
                  $("div:animated") 选取正在执行动画的 <div> 元素。   （ * ）
案例1：

        $("#table1 tr:last").css("color", "red");
        $("#table1 tr:gt(0):lt(3)").css("color", "red");//lt(3)是从gt(0)后得到的新序列中的序号，不要写成lt(4);
        $("#table1 tr:gt(0):even").css("background", "red"); //表头不参与"正文表格的奇数行是红色背景",所以gt(0)去掉表头

10、相对选择器

       不仅可以使用选择器进行进行绝对定位，还可以进行相对定位， （双重选择）

       只要在 $() 指定第二个参数，第二个参数为相对的元素 .  $("ul", $(this)).css("background", "red");  //ul下面的+包含本身=ul下面的本身
![]()![]()View Code

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 6 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 7 <script type="text/javascript"> 8         $(function () { 9             $("#t1 tr").click(function () {10                 $("td", $(this)).css("background", "Yellow");11             });12         });13 </script>14 </head>15 <body>16 <table id="t1"> 17 <tr><td>姓名</td><td>成绩</td></tr> 18 <tr><td>tom</td><td>100</td></tr> 19 <tr><td>lucy</td><td>99</td></tr> 20 <tr><td>jim</td><td>95</td></tr> 21 <tr><td>david</td><td>85</td></tr> 22 <tr><td>candy</td><td>84</td></tr> 23 <tr><td>平均分</td><td>90</td></tr> 24 </table> 25 </body>26 </html>

11、属性过滤选择器： 
        （1、$("div[id]") 选取有 id 属性的 <div> 
        （2、 $("div[title=test]") 选取 title 属性为 " test " 的 <div> ， JQuery 中没有对getElementsByName 进行封装，用 $("input[name=abc]")  
        （3、$("div[title!=test]") 选取 title 属性不为 " test " 的 <div> 
               还可以选择开头、结束、包含等，条件还可以复合。（ * ）

![]()![]()属性过滤

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 6 <script type="text/javascript"> 7         $(function () { 8             $("input[value=显示选中项]").click(function () { 9                 alert($("input:checked").val());10             });11             $("input[type=checkbox]").click(function () {12                 $("input:checked").css("background-color", "Red"); 13                 alert($("input:checked").css("background-color"));14             });15 16         });17 18 </script>19 </head>20 <body>21 <input type="checkbox" value="北京" />北京<br />22 <input type="checkbox" value="南京" />南京<br />23 <input type="checkbox" value="东京" />东京<br />24 <input type="checkbox" value="西安" />西安<br />25 <input type="checkbox" value="开封" />开封<br />26 <input type="button" value="显示选中项" /><br />27 </body>28 </html>

 12、表单对象选择器

        $("#form1:enabled") 选取 id 为 form1 的表单内所有启用的元素 
     $("#form1:disabled") 选取 id 为 form1 的表单内所有禁用的元素 
     $("input:checked") 选取所有选中的元素（ Radio 、 CheckBox ） 
     $("select:selected") 选取所有选中的选项元素（下拉列表）
![]()![]()表单对象

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 6 <script type="text/javascript"> 7         $(function () { 8             $("input[name=names]").click(function () { 9 var names = $("input[name=names]:checked");10 var msg = $("#msgNames");//ID记得加#11 var array = new Array();12 13 //$(value)是将Dom对象转化为JQuery对象,即将元素的value值更新到key上 14                 names.each(function (key, values) {15                     array[key] = $(values).val();16                 });17 18                 msg.text("共选中" + names.length + "个,他们是：" + array.join(","));19             });20         });21 </script>22 </head>23 <body>24 <input type="checkbox" name="names" value="tom" />tom25 <input type="checkbox" name="names" value="jim" />jim26 <input type="checkbox" name="names" value="lily" />lily27 <p id="msgNames">28 </p>29 </body>30 </html>

 13、JQuery的Dom操作

   （1 、使用 html() 方法读取或者设置元素的 innerHTML ：
                  alert($("a:first").html());
                  $("a:first").html("hello"); 
      （2 、使用 text() 方法读取或者设置元素的 innerText ：
                  alert($("a:first").text());
                  $("a:first").text("hello");
      （3 、 使用 attr() 方法读取或者设置元素的属性，对于JQuery没有封装的属性（所有浏览器没有差异的属性）用 attr 进行操作。
                  alert($("a:first").attr("href"));
                  $("a:first").attr("href", "[http://www.rupeng.com](http://www.rupeng.com/)");        
      （4 、使用 removeAttr 删除属性。删除的属性在源代码中看不到，这是和清空属性的区别 

 14、动态创建Dom节点

       （1、使用 $(html 字符串) 来创建 Dom 节点，并且返回一个 jQuery 对象，然后调用 append 等方法将新创建的节点添加到 Dom 中：
          例子：var link = $("<a href='http://www.baidu.com'> 百度 </a>");
               $("div:first").append(link);
    （2、$() 创建的就是一个 jQuery 对象，可以完全进行操作
             var link = $("<a href='http://www.baidu.com'> 百度 </a>");
              link.text("百度");
              $("div:first").append(link); 

 
![]()![]()动态添加Dom对象

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 6 <script type="text/javascript"> 7 /*$(function () { 8             var link = "<a href='http://www.cnblogs.com/daomul' >myBlog</a>"; 9             $("#myTable").append(link);10         });*/11 12         $(function () {13 var data = { "daomul": "http://www.cnblogs.com/daomul", "bokeyuan": "http://www.cnblogs.com" };14             $.each(data, function (key, value) {15 var tr = "<tr><td>" + key + "</td><td><a href=" + value + ">"+key+"</a></td></tr>";16                 $("#myTable").append(tr);17             });18         });19 </script>20 </head>21 <body>22 <table border="0" cellpadding="0" cellspacing="0" id="myTable">23 <tr>24 <td>25 </td>26 </tr>27 </table>28 </body>29 </html>

  
    （3、append 方法用来在元素的末尾追加元素。

         //$("#select1 option:selected").remove().appendTo($("#select2")) ;
            $("#select1 option:selected").appendTo($("#select2")) ;
         prepend ，在元素的开始添加元素。
         after ，在元素之后添加元素（添加兄弟）
         before ：在元素之前添加元素（添加兄弟）

15、删除节点
     （1、remove() 删除选择的节点
               案例：清空 ul 中的项， $("ul li.testitem").remove();  删除 ul 下 li 中有 testitem 样式的元素。    
           remove 方法的返回值是被删除的节点对象，还可以继续使用被删除的节点。比如重新添加到其他节点下 
                  var lis = $("#ulSite li").remove(); 
                  $("#ulSite2").append(lis);     
     （2、remove掉后再重新移动： var items = $("#select1 option:selected").remove();  $("#select2").append(items); 
                           更狠的： $("#select1 option:selected").appendTo($("#select2")) 
![]()![]()权限管理

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 6 <script type="text/javascript"> 7         $(function () { 8             $("#moveToRight").click(function () { 9 var item = $("#select1 option:selected").remove();10                 item.attr("selected", false);11                 $("#select2").append(item);12             });13             $("#moveToLeft").click(function () {14 var item = $("#select2 option:selected").remove();15                 item.attr("selected", false);16                 $("#select1").append(item);17             });18             $("#AllToRight").click(function () {19 var item = $("#select1 option").remove();20                 $("#select2").append(item);21             });22             $("#AllToLeft").click(function () {23 var item = $("#select2 option").remove();24                 $("#select1").append(item);25             });26 27         });28 </script>29 </head>30 <body>31 <select style="float: left; width: 15%; height: 100px" id="select1" multiple="multiple">32 <option>添加</option>33 <option>删除</option>34 <option>修改</option>35 <option>查询</option>36 <option>打印</option>37 </select>38 <div style="float: left; width: 15%">39 <input style="float: left; width: 100%;" type="button" id="moveToRight" value=">" />40 <input style="float: left; width: 100%;" type="button" id="moveToLeft" value="<" />41 <input style="float: left; width: 100%;" type="button" id="AllToRight" value=">>" />42 <input style="float: left; width: 100%;" type="button" id="AllToLeft" value="<<" />43 </div>44 <select style="float: left; width: 15%; height: 100px" id="select2" multiple="multiple">45 </select>46 </body>47 </html>

     （3、empty() 是将节点清空，不像 remove 那样还可以添加到其他元素中。 

 16、Dom实例改编

案例1：加法计算器
![]()![]()加法计算器

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 6 <script type="text/javascript"> 7         $(function () { 8             $("#eq").click(function () { 9 var text1 = $("#txt1").val();//有参数则说明是设置值10 var text2 = $("#txt2").val();11 var txt3 = parseInt(text1, 10) + parseInt(text2, 10);12                 $("#txt3").val(txt3);13             });14         });15 </script>16 </head>17 <body>18 <input type="text" id="txt1" />+ 19 <input type="text" id="txt2" /> 20 <input type="button" id="eq" value="=" /> 21 <input type="text" id="txt3" />22 </body>23 </html>

案例2：全选全部选按钮

![]()![]()全选全不选按钮

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 6 <script type="text/javascript"> 7         $(function () { 8             $("#selAll").click(function () { 9                 $("#playlist input").attr("checked", true);10             });11             $("#unselAll").click(function () {12                 $("#playlist input").attr("checked", false);13             });14             $("#reverse").click(function () {15                 $("#playlist input").each(function () {16 //关键语句！！！17                     $(this).attr("checked", !$(this).attr("checked"));18                 });19             });20         });21 </script>22 </head>23 <body>24 <div id="playlist"> 25 <input type="checkbox" />1111111111<br /> 26 <input type="checkbox" />22222222222<br /> 27 <input type="checkbox" />33333333333<br /> 28 <input type="checkbox" />4444444444<br /> 29 <input type="checkbox" />444<br /> 30 </div> 31 <input type="button" value="全选" id="selAll" /> 32 <input type="button" value="全不选" id="unselAll" /> 33 <input type="button" value="反选" id="reverse" /> 34 35 </body>36 </html>

案例3：倒计时注册页面

![]()![]()倒计时注册页面

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 6 <script type="text/javascript"> 7 var InteruptID; 8 var stopSecond=10; 9         $(function () {10             $("#btnReg").attr("disabled", true);11             InteruptID = setInterval("stopTimeRole()",1000);12         });13 14 function stopTimeRole() {15 if (stopSecond <= 0) {16                 $("#btnReg").attr("disabled", false);17                 $("#btnReg").val("同意");18                 clearInterval(InteruptID);19 return;20             }21             stopSecond--;22             $("#btnReg").val("同意，还剩下"+stopSecond+"秒，请阅读注册事项");23         }24 </script>25 </head>26 <body>27 <input type="button" id="btnReg" value="同意" />28 </body>29 </html>

案例4：球队选择

![]()![]()球队选择

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 6 <script type="text/javascript"> 7         $(function () { 8             $("#myUL li").css("cursor", "pointer").mousemove(function () { 9                 $(this).css("background", "red").siblings().css("background", "white");10             }).click(function () {11                 $(this).css("background","white").appendTo("#newUL");12             });13         });14 </script>15 </head>16 <body>17 <ul id="myUL">18 <li>中国</li>19 <li>美国</li>20 <li>日本</li>21 <li>新加坡</li>22 <li>意大利</li>23 <li>法国</li>24 <li>德国</li>25 </ul>26 <ul id="newUL">27 </ul>28 </body>29 </html>

17、节点操作

        （1、替换节点： $("br").replaceWith("<hr/>");
             例子：将 <br/> 替换为 <hr/> ：$("br").replaceWith("<hr/>");
        （2、包裹节点 ：wrap() 方法用来将所有元素逐个用指定标签包裹：
                $("b").wrap("<font color='red'></font>")  将所有粗体字红色显示  

18、样式操作

    （1、获取样式   attr("class") ，设置样式 attr("class","myclass myclass2 myclass3") ，追加样式 addClass("myclass")( 不影响其他样式 ) ，
           移除样式 removeClass("myclass") ，切换样式（如果存在样式则去掉样式，如果没有样式则添加样式） toggleClass("myclass") ，
           判断是否存在样式： hasClass("myclass")

      例子：开关灯：$(document.body).toggleClass(”night“); 
19、练习

练习1：黑白切换，设置body中的颜色切换Filter

![]()![]()黑白切换

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>黑白切换</title> 5 <style type="text/css"> 6         .blackwhite{filter:Gray;} 7 </style> 8 9 <script src="js/jquery-1.4.2.js" type="text/javascript"></script>10 <script type="text/javascript">11         $(function () {12             $("#btn").click(function () {13                 $(document.body).toggleClass("blackwhite");14             });15         });16 </script>17 </head>18 <body>19 <input type="button" value="切换" id="btn" /><br />20 <img src="images/DSCF3362.JPG" alt=""/>21 </body>22 </html>

练习2：聚集控件高亮：$("body  *") ，选择器 * 表示所有类型的控件
![]()![]()聚集控件高亮

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <!-- 5    练习：聚焦控件的高亮显示。 颜色定义为 class 样式。  $("body  * ") ，选择器 * 表示 所有类型的控件。 6 --> 7 8 <title>聚集控件高亮</title> 9 <style type="text/css"> 10         .highlight{background-color:Gray;} 11 </style> 12 13 <script src="js/jquery-1.4.2.js" type="text/javascript"></script>14 <script type="text/javascript">15         $(function () {16             $("body * ").click(function () {17                 $(this).addClass("highlight").siblings().removeClass("highlight");18             });19         });20 </script>21 </head>22 <body>23 <input type="text" /> 24 <div> 25         daomul 26 </div> 27 <p> 28         http://www.cnblogs.com/daomul/29 </p> 30 31 </body>32 </html>

练习3：搜索框

![]()![]()搜索框效果

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <!-- 5    练习：聚焦控件的高亮显示。 颜色定义为 class 样式。  $("body  * ") ，选择器 * 表示 所有类型的控件。 6 --> 7 8 <title>搜索框效果</title> 9 10 <!--练习：搜索框效果。焦点放入控件，如果文本框中的值是 " 请输入关键词 " ，那么 11         将文本清空，并且颜色设置为黑色。如果焦点离开控件，如果文本框中是空值， 12         那么将文本框填充为 " 请输入关键词 " ，颜色设置为灰色（ Gray ）。 颜色定义为 13         class 样式。-->14 15 <style type="text/css"> 16         .Graycolor{background-color:Gray;} 17 </style> 18 19 <script src="js/jquery-1.4.2.js" type="text/javascript"></script>20 <script type="text/javascript">21         $(function () {22             $("#myInput").addClass("Graycolor")23             .focus(function () {24 if ($(this).val() == "请输入关键词") {25                     $(this).val("").removeClass("Graycolor");26                 }27             })28             .blur(function () {29 if ($(this).val() == "") {30                     $(this).val("请输入关键词").addClass("Graycolor");31                 }32             });33         });34 </script>35 </head>36 <body>37 <input type="text" value="请输入关键词" id="myInput"/> 38 39 </body>40 </html>

20、RadioButton的操作

      （1、取得 RadioButton 的选中值 ：$("input[name=gender]:checked").val()
          <input id="Radio2" checked="checked" name="gender"  type="radio" value=" 男 " /> 男
          <input  id="Radio1" checked="checked" name="gender"  type="radio" value=" 女 " /> 女 
          <input id="Radio3" checked="checked" name="gender" type="radio"  value=" 未知 " /> 未知 </p> 
     （2、设置 RadioButton 的选中值： $("input[name=gender]").val([" 女 "]);
                                              或者 $(":radio[name=gender]").val([" 女 "]); 
            注意 val 中参数的 [] 不能省略
![]()![]()RadioButton

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <!-- 5    练习：聚焦控件的高亮显示。 颜色定义为 class 样式。  $("body  * ") ，选择器 * 表示 所有类型的控件。 6 --> 7 <title>RadioButton</title> 8 9 <style type="text/css">10 11 </style>12 <script src="js/jquery-1.4.2.js" type="text/javascript"></script>13 <script type="text/javascript">14         $(function () {15             $("#getValue").click(function () {16                 alert($("input[name=gender]:checked").val());17             });18             $("#setValue").click(function () {19                 $("input[name=gender]").val(["女"]);20                 $(":checkbox").val(["篮球", "冰球"]);21                 $(":checkbox[value=羽毛球]").attr("checked", true); 22             });23         });24 </script>25 </head>26 <body>27 <input name="gender" type="radio" value="男" />男<br />28 <input name="gender" type="radio" value="女" />女<br />29 <input name="gender" type="radio" value="保密" />保密<br />30 <input type="checkbox" value="篮球" />篮球<br /> 31 <input type="checkbox" value="足球" />足球<br /> 32 <input type="checkbox" value="羽毛球" />羽毛球<br /> 33 <input type="checkbox" value="冰球" />冰球<br />34 <input type="button" value="设值" id="setValue" />35 <input type="button" value="取值" id="getValue" />36 </body>37 </html>

 
      （3、对 RadioButton 的选择技巧对于 CheckBox 和 Select 列表框也适用。除了可以使用 val 批量设置 RadioButton 、 CheckBox 等的选中以
             外，还可以设定 checked 属性来单独设置控件的选中状态  
                                $("#btn1").attr("checked",true)

 
 21、JQuery事件

        
       （1、事件绑定： $("#btn").bind("click",function(){}) ，每次都这么调用太麻烦，所以 jQuery 可以用 $("#btn").click(function(){}) 来进行简化 
       （2、合成事件 hover ， hover(enterfn,leavefn) ，当鼠标放在元素上时调用enterfn 方法，当鼠标离开元素的时候调用 leavefn 方法

![]()![]()合成事件hover

1 <head> 2 <title>合成事件_hover</title> 3 4 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 5 <script type="text/javascript"> 6         $(function() { 7 /* 8             $("p").mouseenter(function() { 9                 $(this).text("客官来了！"); 10             }) 11             .mouseleave(function() { 12                 $(this).text("大爷慢走！"); 13             }); 14 */ 15             $("p").hover(function() { $(this).text("来了") }, function() { $(this).text("慢走")}); 16         }); 17 </script> 18 </head> 19 <body> 20 <p>你好！</p> 21 </body>

      （3、事件冒泡： JQuery 中也像 JavaScript 一样是事件冒泡。如果想获得事件相关的信息，只要给响应的匿名函数增加一个参数： e ，   
                             e 就是调用事件对象的 stopPropagation() 方法终止冒泡。 e. stopPropagation();
                  $("tr").click(function(e) { alert("tr 被点击 "); e.stopPropagation(); });// 注意函数的参数是 e 
      （4、阻止默认行为：有的元素有默认行为，比如超链接点击后会转向新链接、提交按钮默认会提交表单，
                                 如果想阻止默认行为只要调用事件对象的preventDefault() 方法和 window.event.returnValue=false 效果一样。
                  $("a").click(function(e) { alert(" 所有超链接暂时全部禁止点击 "); 
                                                     e.preventDefault(); });
![]()![]()事件绑定

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <!-- 5    练习：聚焦控件的高亮显示。 颜色定义为 class 样式。  $("body  * ") ，选择器 * 表示 所有类型的控件。 6 --> 7 <title>RadioButton</title> 8 9 <style type="text/css">10 11 </style>12 <script src="js/jquery-1.4.2.js" type="text/javascript"></script>13 <script type="text/javascript">14         $(function () {15 //$("#p1").click(function (e) { alert(e.pageX + ":" + e.pageY); });16 //$("#tr1").click(function (e) { });17 18             $("#p1").click(function (e) { alert("下面是p的："); alert($(this).html()); alert($(e.target).html()); });19             $("#tr1").click(function (e) { alert("下面是tr的："); alert($(this).html()); alert($(e.target).html()); });20         }); 21 22 </script>23 </head>24 <body>25 <table id="t1"> 26 <tr id="tr1"> 27 <td id="td1"> 28 <p id="p1">nihao</p> 29 </td> 30 </tr> 31 </table> 32 </body>33 </html>

 

 22、事件其他（ * ）
      （1、属性： pageX 、 pageY 、 target获得触发事件的元素 ( 冒泡的起始，和this不一样) which如果是鼠标事件获得按键（1左键，2中键，3右键）。 
             altKey 、 shiftKey 、 ctrlKey 获得 alt 、shift、ctrl 是否按下，为bool值。 keyCode 、 charCode 属性发生事件时的keyCode （键盘码，小
            盘的1和主键盘的 keyCode 不一样）、charCode （ ASC 码）。   
      （2、移除事件绑定： bind() 方法即可移除元素上所有绑定的事件，如果 unbind("click") 则只移除 click 事件的绑定。 bind:+= ； unbind:-=  
      （3、一次性事件：如果绑定的事件只想执行一次随后立即 unbind 可以使用 one() 方法进行事件绑定： 
                       $(":button").one("click", function() { alert("点了"); }); 
 23、鼠标

    （1、获得发生事件时鼠标的位置
                $(document).mousemove(function(e) {
                       document.title = e.pageX + "," + e.pageY;
                 });
    （2、在 mousemove 、 click 等事件的匿名响应函数中如果指定一个参数 e ，那么就可以从 e 读取发生事件时的一些信息，比如对mousemove 等鼠标事件   来
           说，就可以读取 e.pageX 、 e.pageY 来获得发生事件时鼠标在页面的坐标。 
 案例1：跟着鼠标飞

![]()![]()跟着鼠标飞

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 6 <style type="text/css"> 7 8 </style> 9 <script src="js/jquery-1.4.2.js" type="text/javascript"></script>10 <script type="text/javascript">11         $(function () {12             $(document).mousemove(function (e) {13                 $("#fly").css("left", e.pageX).css("top",e.pageY);14             });15         });16 17 </script>18 </head>19 <body>20 <div id="fly" style="position:absolute"> 21 <img src="images/DSCF3362.JPG" style="width:100px;height:100px;" alt=""/> 22 </div> 23 </body>24 </html>

案例2：点击小图显示详情

![]()![]()点击小图显示详情

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <style type="text/css"> 6 7 </style> 8 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 9 <script type="text/javascript">10 var data = { "images/1小.bmp": ["images/1.JPG", "zzz", "21"],11 "images/2小.bmp": ["images/2.jpg", "yyy", "22"],12 "images/3小.bmp": ["images/3.JPG", "xxx", "23"]13         };14         $(function () {15             $.each(data, function (key, value) {16 var smallPic = $("<img src='" + key + "' style='width:100px;height:100px;'></img>");17 //给小图加上三个额外的属性18                 smallPic.attr("bigPci", value[0]);19                 smallPic.attr("Name", value[1]);20                 smallPic.attr("Height", value[2]);21                 smallPic.mouseover(function (e) {22                     $("#ditailImg").attr("src", $(this).attr("bigPci"));23                     $("#detailName").text("Name:"+$(this).attr("Name"));24                     $("#detailHeight").text("Height:"+$(this).attr("Height"));25                     $("#details").css("left", e.pageX).css("top", e.pageY).css("display", "");26                 });27                 $("body").append(smallPic);28             });29             $("#closeDetails").click(function () {30                 $("#details").css("display","none");31             });32         });33 34 </script>35 </head>36 <body>37 <div style="display: none; position: absolute;" id="details">38 <img id="ditailImg" src="" style="width:200px;height:200px;"/>39 <p id="detailName">40 </p>41 <p id="detailHeight">42 </p>43 <p><input id="closeDetails" type="button" value="关闭" /></p>44 </div>45 </body>46 </html>

24、动画及QQ风格控件
     （1、show() 、 hide() 方法会显示、隐藏元素。用 toggle() 方法在显示、隐藏之间切换 
                     $(":button[value=show]").click(function() { $("div").show(); });
                     $(":button[value=hide]").click(function() { $("div").hide(); });
    （2、 如果 show 、 hide 方法不带参数则是立即显示、立即隐藏，如果指定速度参数则会用指定时间进行动态显示、隐藏，单位为毫秒 ，也可以使用三个内置的速

            度： fast （200 毫秒）、 normal （400 毫秒）、 slow （600 毫秒）， jQuery 动画函数中需要速度的地方一般也可以使用这个三个值。

![]()![]()模拟QQTab效果

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <style type="text/css"> 6         ul{list-style-type:none;} 7         .header{background-color:Green;} 8         .body{border-color:Blue;border-style:solid;border-width:5px;} 9 </style>10 <script src="js/jquery-1.4.2.js" type="text/javascript"></script>11 <script type="text/javascript">12 //有body样式的li13         $(function () {14             $("#qq li:odd").addClass("body"); //偶数行15             $("#qq li:even").addClass("header").click(function () {//奇数行16                 $(this).next("li.body").show("fast").siblings("li.body").hide("fast");17             });18 //初始打开第一个表头19             $("#qq li:first").click();20         });21 </script>22 </head>23 <body>24 <h1 style="margin-left:50px">QQTab效果</h1> 25 <ul id="qq" style="width:30%"> 26 <li>我的好友</li> 27 <li>mm<br />baba<br />mama<br /></li> 28 <li>我的女友<br /></li> 29 <li>gemen<br /></li> 30 <li>陌生人</li> 31 <li>meinv<br />shuaige<br /></li> 32 </ul> 33 34 </body>35 </html>

 25、JQuery Cookie 使用 
      （1、使用方法， Cookie 保存的是键值对
           *1 、添加对 jquery.cookie.js
           *2 、设置值， $.cookie(' 名字 ', ' 值 ') 。 cookie 中保存的值都是文本。
           *3 、读取值， var v=$.cookie(' 名字 ')
                      alert($.cookie(" 用户名 "));
                      $.cookie(" 用户名 ","tom"); 在同域名的另外一个页面中也能读取到。 
      （2、设置值的时候还可以指定第三个参数， $.cookie(' 名字 ', ' 值 ', { expires: 7, path: '/',domain: 'itcast.cn', secure: true }) ，  
              expires 表示要求浏览器保留 Cookie 几天，这个值只是给浏览器的建议，可能没到时间就已经被清除了。可以实现 " 勾选 【 记录我的用户
              名 10 天 】 " 的功能。如果不设定 expires 在浏览器关闭以后就清除， options 参数用哪个设置哪个  
 案例：记住背景色

![]()![]()记住背景色

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 <style type="text/css"> 6 </style> 7 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 8 <script src="js/jquery.cookie.js" type="text/javascript"></script> 9 <script type="text/javascript">10 if ($.cookie("UserName")) { //读取上次记录的用户名 11             $("username").val($.cookie("UserName"));12         }13         $("#btnLogin").click(function () {14             $.cookie("UserName", $("#username").val())//用户填写的用户名保存到Cookie中 15         });16 17         $(function () {18             $("#tableColor td").click(function () {19 var bgColor = $(this).css("background-color");20                 $(document.body).css("background-color", bgColor);21                 $.cookie("backcolor", bgColor, {expires:2});22             });23 24 if ($.cookie("backcolor")) {25                 $(document.body).css("background-color", $.cookie("backcolor"));26             }27         });28 29 </script>30 </head>31 <body>32 <table id="tableColor">33 <tr>34 <td style="background-color: Red">35                 红色36 </td>37 <td style="background-color: Green">38                 绿色39 </td>40 <td style="background-color: Yellow">41                 黄色42 </td>43 </tr>44 </table>45 <input type="text" id="username" />46 <input type="button" value="登录" id="btnLogin" />47 </body>48 </html>

26、JQueryUI

 下载地址：[http://jqueryui.com/](http://jqueryui.com/)下发包
![]()![]()View Code

1 <head> 2 <title>JQueryUI的使用</title> 3 <script src="js/jquery-1.4.2.js" type="text/javascript"></script> 4 <link href="css/ui-lightness/jquery-ui-1.8.16.custom.css" rel="stylesheet" type="text/css" /> 5 <script src="js/jquery-ui-1.8.16.custom.min.js" type="text/javascript"></script> 6 7 <script type="text/javascript"> 8         $(function() { 9             $("#mydialog").dialog(); 10             $("#testtab").tabs(); 11         }); 12 </script> 13 </head> 14 <body> 15 <div id="mydialog">你好，我是对话框</div> 16 <div id="testtab"> 17 <ul> 18 <li><a href="#tabBasic">基本设置</a></li> 19 <li><a href="#tabAdv">高级设置</a></li> 20 </ul> 21 <div id="tabBasic">用户名：<input type="text"/></div> 22 <div id="tabAdv">刷新频率：<input type="text" /></div> 23 </div> 24 </body>

 

 

 

分类: [jQuery](http://www.cnblogs.com/daomul/category/419511.html)

绿色通道： [好文要顶]() [关注我]() [收藏该文]()[与我联系](http://space.cnblogs.com/msg/send/daomul) [![]()]( "分享至新浪微博")
[daomul](http://home.cnblogs.com/u/daomul/)
[关注 - 1](http://home.cnblogs.com/u/daomul/followees)
[粉丝 - 43](http://home.cnblogs.com/u/daomul/followers)

[+加关注]()

32

0
(请您对文章做出评价)

[«](http://www.cnblogs.com/daomul/archive/2013/04/23/3029203.html)上一篇：[Dom 学习总结及其实例](http://www.cnblogs.com/daomul/archive/2013/04/23/3029203.html "发布于2013-04-23 00:36")
[»](http://www.cnblogs.com/daomul/archive/2013/04/29/3046449.html)下一篇：[.NET运用AJAX 总结及其实例](http://www.cnblogs.com/daomul/archive/2013/04/29/3046449.html "发布于2013-04-29 18:32")
