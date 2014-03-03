---
layout: post
title: "Dom 学习总结及其实例 - daomul - 博客园"
categories: JS用法
tags: 
 - JS用法
--- 

# Dom 学习总结及其实例 - daomul - 博客园

# [Dom 学习总结及其实例](http://www.cnblogs.com/daomul/archive/2013/04/23/3029203.html)

1、   重新导航到指定的地址：navigate("[http://www.cnblogs.com/daomul/](http://www.cnblogs.com/daomul/)");

2、

 （1、*setInterval每隔一段时间执行指定的代码，第一个参数为代码的字符串，第二个参数为间隔时间（单位毫秒），返回值为定时器的标识。如：  

              setInterval("alert('hello')",5000);

*clearInterval取消setInterval的定时执行，相当于Timer中的Enabled=False。因为setInterval可以设定多个定时，所以clearInterval要指定清除那个定时器的标识，即setInterval的返回值。

var intervalld= setInterval("alert('hello')",5000);
clearInterval(intervalld);

（2、setTimeout也是定时执行，但是不像setInterval那样是定时执行，而是设定时间后只执行一次，clearTimeout也是清除定时。
       很好区分：Interval是定时；Timeout是超时之意。

            var timeoutld=setTimeout("alert('hello')",2000);
（3、案例：实现标题栏走马灯的效果，也就是浏览器的标题文字每隔500ms向右滚动一下
![]()![]()跑马灯效果

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 function scroll() { 7 var title = document.title; 8 var first = title.charAt(0); 9 var last = title.substring(1, title.length);10                 document.title = last+first;11             }12             setInterval("scroll()",500);13 </script>14 </head>15 <body>16 17 </body>18 </html>

3、

   （1、onload:网页加载完毕时触发，浏览器是一边下载文档、一边解析执行，可能会出现JavaScript执行时需要操作某个元素，这个元素还没有加载，如果这样就要把操作的代码放到body的onload事件中，或者可以把JavaScript放到元素之后。元素的onload事件是元素自己加载完毕时触发，而body 里的onload才是全部加载完成。
   （2、onunload：网页关闭（或者离开）后触发。onbeforeunload：窗口离开（比如前进、后退、关闭之前）就会弹出确认消息。如：                                                     <body onbeforeunload="Window.event.returnValue='真的要放弃发贴退出吗？'"> 

4、

     除了有特有的属性之外，当然还有通用的HTML元素的事件：onclick(单击)、ondblclick(双击)、onkeydown(按键按下)、onkeyup(按键释放)、onkeypress(点击按键)、onmousedown(鼠标按下)、onmousemove(鼠标移动)、onmouseout(鼠标离开元素范围)、
onmouseover(鼠标移动到元素范围)、onmouseup(鼠标按键释放)等。

5、window对象的属性

（1、window.location.href="[http://www.sina.com.cn](http://www.sina.com.cn/)",重新导向新的地址，和navigate方法效果一样。window.location.reload()刷新页面。
（2、window.event是非常重要的属性，用来获得发生事件时的信息，事件不局限于window对象的事件，所有元素的事件都可以通过event属性取到相关信息。
         **a**、altKey属性，boot类型，表示发生事件时alt键是否被按下，类似的还有ctrlKey、shiftKey属性
![]()![]()windows事件样例

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 </head> 6 <body> 7 <input type="button" value="href" onclick="alert(location.href)" /> 8 <input type="button" value="重定向" onclick="location.href='页面1.htm'" /> 9 <input type="button" value="点击" onclick="if(window.event.ctrlKey){alert('按下了Ctrl')}else{alert('普通点击')}" /><a10 href="http://www.baidu.com" onclick="alert('禁止访问！');window.event.returnValue=false;"></a>11 <form action="daomul.aspx">12 <input type="submit" value="提交" onclick="alert('数据有问题!');window.event.returnValue=false;" />13 </form>14 </body>15 </html>

 

**b**、 clientX、clientY 发生事件时鼠标在客户区（浏览器界页面内）的坐标；screenX、screenY发生事件时鼠标在屏幕上的坐标；offsetX、offsetY发生事件时鼠标相对于事件源（按钮button内）的坐标。
**c**、returnValue属性，如果将returnValue设置为false，就会取消默认事件的处理。
**d**、srcElement:获得事件源对象
**e**、KeyCode：发生时间时的按键值
**f**、button：发生时间时鼠标的按键，1为左键，2为右键，3为左右键同时按。

         <body onmousedown="if(event.button==2){alert('禁止复制')}">

6、clipboardData对象，对粘贴板的操作。clearData("Text")清空粘贴板；getData("Text")读取粘贴板的值，返回值为粘贴板中的内容；setData("Text",val),设置粘贴板中的值。

（1、当复制的时候body的oncopy方法被触发，直接return false就是禁止复制。<body oncopy="alert('禁止复制！');return false;">
（2、很多元素也有oncopy、onpaste事件。

 例子1：禁止复制
                    <body oncopy="alert('禁止复制！');return false;">
 例子2：给粘贴板赋值：复制地址给好友

<input type="button" value="分享本页给好友" onclick="clipboardData.setData('Text','我发现一个很实用的网站，计算机方面的!'+ 
location.href);alert(' 已经将地址放到粘贴板中，赶快通过QQ传递给你的好友吧！');" />

 例子3：禁止粘贴到文本框

请输入您的手机号码：<input type="text" />
请您再次输入手机号码：<input type="text" onpaste="alert('为保证您充值到正确的手机号，请勿粘贴手机号，请直接填！');return false;"/>:

 例子4：复制时附带内容

在网站中复制文章的时候，为了防止那些拷贝党不添加文章来源，自动在复制的内容后添加版权声明。

function modifyClipboard(){

   clipboardData.setData('Text',clipboardData.getData('Text')+'本文来自博客园技术专区，转载请注明来源。'+location.href);
}
oncopy="setTimeout('modifyClipboard()',100)"。

用户复制动作发生0.1秒以后再去修改粘贴板中的内容。100ms只是一个经常取值，写1000、10、50、20……都行。不能直接在oncopy中执行对粘贴板的操作，因此设定定时器，0.1秒以后执行，这样就不再oncopy的执行调用栈上了。

7、页面前进、后退：history操作历史记录

window.history.back()后退；
window.history.forward()前进。也可以用window.history.go(-1)表前进；window.history.go(1)表后退。

实例1： 

<body>这里是第2页<a href="javascript:window.history.back()">后退</a><input type="button" value="后退" onclick="window.history.go(-1)" />
</body>

8、document属性（最复杂的属性）document是window对象的一个属性，因为使用window对象成员的时候可以省略window,所以一般直接写document。

（1、write:向文档中写入内容。writeln和write差不多，只不过最后添加一个回车
（2、<input type="button" value="点击" onclick="document.write('<font color=blue>您好</font>')" />
（3、在onclick等事件中写的代码会冲掉页面中的内容，只有在页面加载过程中write才会与原有内容融合在一起
（4、<script type="text/javascript">
                 document.write("<font color=red>您好</font>");
        </script>

案例1：
![]()![]()getElementById

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>getElementById</title> 5 <script type="text/javascript"> 6 //不建议直接通过id操作元素，而是通过getElementById根据元素的Id获得对象 7 function btnClick() { 8 //alert(textbox1.value); 9 var txt = document.getElementById("textbox1");10             alert(txt.value);11         }12 function btnClick2() {13 //alert(form1.textbox2.value);//要显示表单里的信息需添加表单名称14 var txt = document.getElementById("textbox2");15             alert(txt.value);16         }17 </script>18 </head>19 <body>20 <input type="text" id="textbox1" />21 <input type="button" value="点击我" onclick="btnClick()" />22 <form action="页面1.htm" id="form1">23 <input type="text" id="textbox2" />24 <input type="button" value="显示刚输入的内容" onclick="btnClick2()" />25 </form>26 </body>27 </html>

案例2：

![]()![]()getElementByName

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>getElementByName的例子</title> 5 <script type="text/javascript"> 6 function btnClick() { 7 var radios = document.getElementsByName("gender"); 8 /*//在JS中for(var r in radios)不像C#中的foreach,并不会遍历每个元素,而是遍历的key 9             for (var r in radios) {10             alert(r.value);11             }*/12 for (var i = 0; i < radios.length; i++) {13 var radio = radios[i];14                 alert(radio.value);15             }16         }17 function btnClick2() {18 var inputs = document.getElementsByTagName("input");19 for (var i = 0; i < inputs.length; i++) {20 var input = inputs(i);21                 input.value = "welcome to my room";22             }23         }24 </script>25 </head>26 <body>27 <input type="radio" name="gender" value="男" />男28 <input type="radio" name="gender" value="女" />女29 <input type="radio" name="gender" value="保密" />保密30 <input type="button" value="click" onclick="btnClick()" />31 <br />32 <input type="text" /><br />33 <input type="text" /><br />34 <input type="text" /><br />35 <input type="text" /><br />36 <input type="text" /><br />37 <input type="button" value="bytagname" onclick="btnClick2()" />38 </body>39 </html>

案例3：

![]()![]()getElementByTagName

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>getElementByTagName</title> 5 <script type="text/javascript"> 6 function initEvent() { 7 var inputs = document.getElementsByTagName("input"); 8 for (var i = 0; i < inputs.length; i++) { 9 var input = inputs[i];10                 input.onclick = btnClick;11             }12         }13 function btnClick() {14 var inputs = document.getElementsByTagName("input");15 for (var i = 0; i < inputs.length; i++) {16 var input = inputs[i];17 //window.event.srcElement//取得引发事件的控件18 if (input == window.event.srcElement) {19                     input.value = "哈哈"; //以下五个按钮，点到的那个就变为“哈哈”，其余都为“呜呜”，点“呜呜”就变为“哈哈”20                 }21 else {22                     input.value = "呜呜";23                 }24             }25         }26 </script>27 </head>28 <body onload="initEvent()">29 <input type="button" value="欢迎进入" />30 <input type="button" value="欢迎进入" />31 <input type="button" value="欢迎进入" />32 <input type="button" value="欢迎进入" />33 <input type="button" value="欢迎进入" />34 </body>35 </html>

案例4：

![]()![]()阅读协议等待计时器

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6  7 var leftTime = 10; 8 var TimeID; 9 function scroll() {//alert(222);10 var btn = document.getElementById("btnAgree");11 //如果网页速度非常慢的话，可能定时器运行的时候控件还没有加载！12 if (btn) {//alert(11);13 if (leftTime <= 0) {14                         btn.value = "同意";15                         btn.disabled = "";16                         clearInterval(TimeID);17                     }18 else {19                         btn.value = "请阅读协议，同意（还剩下"+leftTime+"秒）";20                         leftTime--;21                     }22                 }23             }24             TimeID=setInterval("scroll()", 1000);25 </script>26 </head>27 <body>28 <textarea id="TextArea1" cols="20" rows="2"></textarea>29 <input id="btnAgree" type="button" value="同意" disabled="disabled"/>30 </body>31 </html>

案例5：

![]()![]()美女时钟

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 //var now = new Date();不能放在这，否则取得时间不变 7 function Fill2Len(i) { 8 if (i < 10) { 9 return "0" + i;10             }11 else {12 return i;13             }14         }15 function Refresh() {16 var imgMM = document.getElementById("imgMM");17 if (!imgMM) {18 return; //没加载就直接return19             }20 var now = new Date(); //每次刷新时取得时间21 var filename = Fill2Len(now.getHours()) + "_" + Fill2Len(now.getSeconds()) + ".jpg";22             imgMM.src = "MMClock/" + filename;23         }24         setInterval("Refresh()", 1000);25 </script>26 </head>27 <body onload="Refresh()">28 <img id="imgMM" src="" />29 </body>30 </html>

案例6：

![]()![]()五角星评分

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 function indexOf(arr,element) { 7 for (var i = 0; i < arr.length; i++) { 8 if (arr[i] == element) { 9 return i;10                     }11                 }12 return -1;13             }14 15 function myload() {16 //获得table下面的所有td17 var table1 = document.getElementById("NewTalbeId");18 var td1 = table1.getElementsByTagName("td");19 for (var i = 0; i < td1.length; i++) {20                     td1[i].onclick = onmyBlur;21 //鼠标移到上面变手势22                     td1[i].style.cursor = "pointer";23                 }24             }25 26 function onmyBlur() {27 var table1 = document.getElementById("NewTalbeId");28 var td1 = table1.getElementsByTagName("td");29 var index = indexOf(td1, this);30 for (var i = 0; i <= index; i++) {31                     td1[i].style.background = "yellow";32                 }33 for (var i = index + 1; i < td1.length; i++) {34                     td1[i].style.background = "white";35                 }36             }37 </script>38 </head>39 <body onload="myload()">40 <table border="0" cellpadding="0" cellspacing="0" id="NewTalbeId">41 <tr>42 <td>☆</td><td>☆</td><td>☆</td><td>☆</td><td>☆</td>43 </tr>44 </table>45 </body>46 </html>

 

9、Dom的动态创建

     （1、document.write只能在页面加载过程中才能动态创建。
     （2、可以调用document的createElement方法来创建具有指定标签的DOM对象，然后通过调用某个元素的appendChild方法将新创建元素添加到相应的元素下
![]()![]()动态创建按钮

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 function clickButton() { 7 var divMain = document.getElementById("divMain"); //获得层 8 var btn = document.createElement("input"); //创建一个标签名为input的元素（这时还没添加到任何元素上，所以没有显示） 9             btn.type = "button";10             btn.value = "新动态按钮";11             divMain.appendChild(btn); //将动态产生的元素添加到divMain层（容器）12         }13 </script>14 </head>15 <body>16 <div id="divMain"></div>17 <input type="button" value="OK" onclick="clickButton()" />18 </body>19 </html>

10、innerText与innerHTML

     （1、几乎所有DOM元素都有innerText与innerHTML属性（注意大小写），分别是元素标签内内容的文本表示形式和HTML源代码，这两个属性是可读可写的。

                 <a href="[http://cnblog.com/daomul/](http://blog.sina.com.cn/)" id="link1">博<font color="red"/>客</font>园</a>
                  <input type="button" value="innerXXX" onclick="alert(document.getElementById('link1').innerText);
                                                                                        alert(document.getElementById('link1').innerHTML);" />

     （2、用innerHTML也可以替代createElement,属于简单、粗放型、后果自负的创建。

function createInput() {
            var divMain = document.getElementById('divMain');
           divMain.innerHTML="<a href='[http://cnblog.com/daomul/](http://blog.sina.com.cn/)'>daomul的博客</a>";
}

 11、浏览器兼容性问题：

       （1、IE6、IE7对table.appendChlid("tr")的支持和IE8不一样，用insertRow、 insertCell来代替或者为表格添加tbody，然后向tbody中添加tr。个人比较喜欢          

              下面的Insert方式，它不是不兼容火狐浏览器，它也是基本兼容的了，只是 (FF)不支持innerText，但是支持innerHtml。

   实例：动态增加双行网站列表

    
![]()![]()采用InsertRow和insertCell

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>动态创建表格的兼容性</title> 5 <script type="text/javascript"> 6  7 function LoadData() {//方法一 8 var data = { "百度": "http://www.baidu.com", "新浪": "http://www.sina.com", "淘宝": "http://www.taobao.com" }; 9 var tableLinks2 = document.getElementById("tableLinks2");10 for (var key in data) {11 var value = data[key];12 var tr = tableLinks2.insertRow(-1); //在表格最后插入一行，并且返回插入行的对象。联想Winform中TreeNode node=treeNode.AddNode("hello")13 14 var td1 = tr.insertCell(-1); //在tr中增加一个td，并且返回增加的td，然后td中放入内容15                 td1.innerText = key; //FF不支持innerText16 var td2 = tr.insertCell(-1);17                 td2.innerHTML = "<a href='" + value + "'>" + value + "</a>"; //也可以用createElement18             }19         }20 </script>21 </head>22 <body>23 <table id="tableLinks">24 <tbody>25 </tbody>26 </table>27 <table id="tableLinks2">28 </table>29 <input type="button" value="非兼容性加载数据" onclick="LoadData()" />30 </body>31 </html>

![]()![]()采用appendChild方法

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>动态创建表格的兼容性</title> 5 <script type="text/javascript"> 6  7  8 function LoadDataGood() {//方法二 9 var data = { "百度": "http://www.baidu.com", "新浪": "http://www.sina.com", "淘宝": "http://www.taobao.com" };10 var tableLinks = document.getElementById("tableLinks");11 for (var key in data) {12 var value = data[key];13 var tr = document.createElement("tr");14 15 var td1 = document.createElement("td"); //先创建td,在td中放入内容，再加入tr16                 td1.innerText = key;17                 tr.appendChild(td1);18 19 var td2 = document.createElement("td");20                 td2.innerHTML = "<a href='" + value + "'>" + value + "</a>";21                 tr.appendChild(td2);22 23                 tableLinks.tBodies[0].appendChild(tr); //并且在<table>标签里要加上<tbody></tbody>24             }25         } //动态产生的元素，查看源代码是看不到的。但可以通过DebugBar->Dom->Document->HTML可以看到26 27 28 29 </script>30 </head>31 <body>32 <table id="tableLinks">33 <tbody>34 </tbody>35 </table>36 <table id="tableLinks2">37 </table>38 <input type="button" value="兼容性  加载数据" onclick="LoadDataGood()" />39 </body>40 </html>

 12、事件冒泡

       如过元素A嵌套在元素B中，那么A被点击不仅A的onclick事件会被触发，B也会，顺序是由内而外。

 13、事件中的this

      除了可以使用event.srcElement在事件响应函数中，this表示发生事件的控件。只有在事件响应函数才能使用this获得发生事件的控件，在事件响应函数调用的函数  中不能使用，如果要使用则要将this传递给函数或者使用event.srcElement. 
  案例： 
![]()![]()this事件

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 <script type="text/javascript"> 7 function btnClick3() {//在事件响应函数调用的函数里就不能通过this获得事件对象了 8             alert(this.value);//弹出undefined 9         }10 function btnClick4(btn) {//在事件响应函数中将this传过来就可以11             alert(btn.value);12         }13 function btnClick5() {//this与event.srcElement语义是不一样的，this就表示当前监听事件的这个对象，event.srcElement是引发事件的对象14             alert(event.srcElement.value);15         }16 </script>17 </head>18 <body>19 <input type="button" value="Logan" onclick="alert(event.srcElement.value)" />20 <input type="button" value="Logan2" onclick="alert(this.value)" />21 <input type="button" value="Logan3" onclick="btnClick3()" />22 <input type="button" value="Logan4" onclick="btnClick4(this)" />23 <input type="button" value="Logan5" onclick="btnClick5()" />24 </body>25 </html>

14、Dom修改样式

     （1、Dom中修改css样式，必须是通过className而不是class(可能是因为作为关键字)

            例如：onclick="document.getElementById('htmldivID').className='cssID'; "

            例如：获得body的css样式id：document.body.className

    （2、单独修改样式的属性使用“style.属性名”，不过要注意存在“-”的属性，因为javascript中横线代表减号

 案例1：ontextBlur()是myload的响应函数，而不是被响应函数调用的函数，所以可以用this来获得
![]()![]()修改Dom样式-鼠标移离

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 function myload() {//alert(1); 7 var texts = document.getElementsByTagName("textarea"); 8 for (var i = 0; i < texts.length; i++) { 9 //alert(2);10 var text = texts[i];11                     text.onblur = ontextBlur;12                 }13             }14 15 function ontextBlur() {16 //alert(3);17 if (this.value <= 0) {18 //alert(4);19 this.style.background = "red";20                 }21 else {22 this.style.background = "write";23                 }24             }25 </script>26 </head>27 <body onload="myload()">28 <textarea id="TextArea1" cols="20" rows="2" ></textarea>29 </body>30 </html>

案例2：评分

![]()![]()评分

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 function indexOf(arr,element) { 7 for (var i = 0; i < arr.length; i++) { 8 if (arr[i] == element) { 9 return i;10                     }11                 }12 return -1;13             }14 15 function myload() {16 //获得table下面的所有td17 var table1 = document.getElementById("NewTalbeId");18 var td1 = table1.getElementsByTagName("td");19 for (var i = 0; i < td1.length; i++) {20                     td1[i].onclick = onmyBlur;21 //鼠标移到上面变手势22                     td1[i].style.curser = "pointer";23                 }24             }25 26 function onmyBlur() {27 var table1 = document.getElementById("NewTalbeId");28 var td1 = table1.getElementsByTagName("td");29 var index = indexOf(td1, this);30 for (var i = 0; i <= index; i++) {31                     td1[i].style.background = "yellow";32                 }33 for (var i = index + 1; i < td1.length; i++) {34                     td1[i].style.background = "white";35                 }36             }37 </script>38 </head>39 <body onload="myload()">40 <table border="0" cellpadding="0" cellspacing="0" id="NewTalbeId">41 <tr>42 <td>☆</td><td>☆</td><td>☆</td><td>☆</td><td>☆</td>43 </tr>44 </table>45 </body>46 </html>

 15、编程控制层

（1、元素的position样式值：static（无定位，显示在默认位置）、absolute(绝对位置)、fixed（相对于窗口的固定定位，位置不会随着浏览器的滚动而变化，IE6不支持）、relative(相对元素默认位置的定位)。如果要通过代码修改元素的坐标则一般使用absolute，然后修改元素的top（上边缘距离）、left（左边缘距离）两个样式值。

 （2、createElement的两种用法，注意innerHTML的问题，

       var input=document.createElement("<a href=‘[http://www.cnblogs.com/daomul](http://www.cnblogs.com/daomul)’>文本</a>")文本部分不会被识别

（3、设定一些DOm元素属性名的特殊属性：例如

       label.setAttribute("for","username");  label.setAttribute("aaa","111");

案例1：高级选项的显示隐藏
![]()![]()高级选项的显示隐藏

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 function showSomething(sb) { 7 var div1 = document.getElementById("div1"); 8 if (sb.checked) { 9                     div1.style.display = "";10                 }11 else {12                     div1.style.display = "none";13                 }14             }15 </script>16 </head>17 <body>18 <input type="checkbox" id="checkin" onclick="showSomething(this)"/><label for="">显示高级选项</label>19 <div id="div1" style="display:none"> 20             显示被隐藏的高级选项21 </div>22 </body>23 </html>

 案例2：鼠标移动（1）

![]()![]()鼠标移动图片

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6             document.onmousemove = function () { 7 var x = window.event.clientX; 8 var y = window.event.clientY; 9 var div1 = document.getElementById("div1");10 11 if (!div1) { return; }12                 div1.style.left = x;13                 div1.style.top = y;14             }15 </script>16 </head>17 <body>18 <div id="div1" style="position:absolute"> 19 <img src="images\DSCF0762.JPG" alt="Alternate Text" style="width:100px;height:100px"/> 20 <br/>美女与你同在21 </div>22 </body>23 </html>

 案例3：鼠标移动显示信息（2）

![]()![]()鼠标移动显示信息

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <style type="text/css"> 6            .mydiv 7 { 8               border-style:dotted; 9               border-width:2px;10               border-color:Green;11               background-color:Gray;12 }13 </style>14 <script type="text/javascript">15 function InitLoad() {16 var links=document.getElementsByTagName("a"

);
 17 for (var i = 0; i < links.length; i++) {18                   links[i].onmouseover = linkMouseOver;19                   links[i].onmouseout=linkMouseOut;20               }21           }22 function linkMouseOut() {23 var div2 = document.getElementsByTagName("div");24 for (var i = 0; i < div2.length; i++) {25 if (div2[i].className == "mydiv") {26 27                       document.body.removeChild(div2[i]); //remove的是div2[i]不是div228 //div2.style.dispaly = "none"; //只是隐藏没有消去29                   }30               }31           }32 33 function linkMouseOver() {34 var div1 = document.createElement("div");35               div1.className = "mydiv";36               div1.style.position = "absolute";37               div1.style.left = window.event.clientX;38               div1.style.top = window.event.clientY;39               div1.innerHTML = "nimei nimie nimei nimei ";40 41               document.body.appendChild(div1);42           }43 44 45 </script>46 </head>47 <body onload="InitLoad()">48 <a href="#">daomul</a>49 <a href="#">ddddd</a>50 <a href="#">aaaaa</a>51 <a href="#">bbbbb</a>52 </body>53 </html>

 案例4：小图片显示大图片

![]()![]()小图片显示大图片

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢(小图片显示大图片1)</title> 5 <script type="text/javascript"> 6 function InitLoad() { 7 var data = { "images/1小.bmp": ["images/1.JPG", "zzz", "21"], 8 "images/2小.bmp": ["images/2.jpg", "yyy", "22"], 9 "images/3小.bmp": ["images/3.JPG", "xxx", "23"]10                 };11 for(var smallImg in data){12 //创建13 var smallIm=document.createElement("img");14 //属性(因为图片的路径在存入和取出的过程中不同，15 //所以还是自定义属性，)16                     smallIm.src = smallImg;17                     smallIm.style.width = "100px";18                     smallIm.style.height = "100px";19                     smallIm.setAttribute("a1",data[smallImg][0]);20                     smallIm.setAttribute("a2",data[smallImg][1]);21                     smallIm.setAttribute("a3",data[smallImg][2]);22 23 //方法24                     smallIm.onmouseover = function () {25 //26                         document.getElementById("imgId").src = this.getAttribute("a1");27                         document.getElementById("NameId").innerHTML = this.getAttribute("a2");28                         document.getElementById("AgeId").innerHTML = this.getAttribute("a3");29 //显示DIV30 var div1 = document.getElementById("showDivId");31                         div1.style.left = window.event.clientX;32                         div1.style.top = window.event.clientY;33                         div1.style.display = '';34 //alert(1);35                     };36 //关联绑定37                     document.body.appendChild(smallIm);  38                 }39           }40 41 </script>42 </head>43 <body onload="InitLoad()">44 <div style="display:none;position:absolute;" id="showDivId">45 <img src="" alt="error Pic ,please call the Admin"  id="imgId" style="width:200px;height:200px;"/>46 <p id="NameId"></p>47 <p id="AgeId"></p>48 </div>49 </body>50 </html>

 案例5：评分控件改进

![]()![]()评分控件改进

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢(评分改进2)</title> 5 <script type="text/javascript"> 6 function indexOf(arr,element) { 7 for (var i = 0; i < arr.length; i++) { 8 if(arr[i]==element){ 9 return i;10                    }11                 }12 return -1;13             }14 15 16 function InitLoad() {17 var table1 = document.getElementById("myTable"); 18 var td1 = table1.getElementsByTagName("td");19 for (var i = 0; i < td1.length; i++) {20                     td1[i].style.cursor = 'pointer';21                     td1[i].onmouseover = function () {22 23 var table1 = document.getElementById("myTable");24 var td1 = table1.getElementsByTagName("td");25 var index = indexOf(td1, this);26 for (var i = 0; i <= index; i++) {27                             td1[i].innerText = "★";28                         }29 for (var i = index + 1; i < td1.length; i++) {30                             td1[i].innerText = "☆";31                         }32                     };33                 }34           }35 36 </script>37 </head>38 <body onload="InitLoad()">39 <table border="0" cellpadding="0" cellspacing="0" id="myTable">40 <tr>41 <td>☆42 </td>43 <td>☆44 </td>45 <td>☆46 </td>47 <td>☆48 </td>49 <td>☆50 </td>51 </tr>52 </table>53 </body>54 </html>

案例6：搜索框关键字搜索

![]()![]()搜索框关键字搜索

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢(文本框关键字3)</title> 5 <script type="text/javascript"> 6 function inputBlur(keyword) { 7 if (keyword.value.length <= 0) { 8                 keyword.value = "请输入搜索关键词"; 9                 keyword.style.color = 'Gray';10             }11         }12 function inputFocus(keyword) {13 //var keyword = document.getElementById("keyword");把this传给两个方法，省去了getElementById依赖id14 if (keyword.value == "请输入搜索关键词") {15                 keyword.value = "";16                 keyword.style.color = 'Black';17             }18         }19 </script>20 </head>21 <body onload="InitLoad()">22 <input onblur="inputBlur(this)" onfocus="inputFocus(this)" value="请输入搜索关键词" style="color: Gray" />23 <input type="button" value="搜索" />24 </body>25 </html>

16、Form表单： Form 对象是表单的 Dom 对象。

 方法： submit() 提交表单，但是不会触发 onsubmit 事件。 实现 autopost ，也就是焦点离开控件以后页面立即提交，而不是 只有提交 submit 按钮以后才提交，当光标离开的时候触发 onblur 事件，在 onblur 中调用 form 的 submit 方法。在点击 submit 后 form 的 onsubmit 事件被触发 ，在 onsubmit 中可以 进行数据校验，数据有问题， 返回 false 即可取消提交。

 案例1：
![]()![]()Form表单

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢(Form表单提交4)</title> 5 <script type="text/javascript"> 6  7 </script> 8 </head> 9 <body onload="InitLoad()">10 <form action="default.aspx" id="form1" onsubmit="return false;">11 <!--通过click函数"欺骗"其他按钮触发-->12 <input type="button" id="btn1" onclick="alert('触发了其他button了')" value="按钮" />13 <input type="button" onclick="document.getElementById('btn1').click()" value="同样提交" />14 <!--因为return false，所以不可提交表单，但是调用submit(),却可以触发方法-->15 <input type="submit" onclick="document.getElementById('form1').submit()" value="可提交的submit" />16 <input type="submit" value="不可提交的submit" />17 <!--类似于asp.net中的autopostback(相同的还有文本框输入后移开的onblur方法的submit调用)-->18 <select id="Select1" onchange="document.getElementById('form1').submit()">19 <option>111</option>20 <option>222</option>21 <option>333</option>22 <option>444</option>23 </select>24 </form>25 </body>26 </html>

17、正则表达式 

 *JavaScript 中创建正则表达式类的方法： 
            var regex = new RegExp("\\d{5}")  或者   var regex = /\d{5}/ 
           / 表达式 / 是 JavaScript 中专门为简化正则表达式编写而提供的语法， 
           写在 // 中的正则表达式就不用管转义符了。 
 *RegExp 对象的方法： 
      **  test(str) 判断字符串 str 是否匹配正则表达式，相当于 IsMatch 
          var regex = /.+@.+/; 
         alert(regex.test("a@b.com")); 
         alert(regex.test("ab.com")); 
     **  exec(str) 进行搜索匹配，返回值为匹配结果 ( * ) 
     **  compile 编译表达式，提高运行速度。   ( * ) 
*String 对象中提供了一些与正则表达式相关的方法，相当于对于 
               RegExp 类的包装，简化调用： 
              match(regexp) ,相当于调用 exec 

        var s = "aaa@163.com"; 
        var regex = /(.+)@(.+)/; 
        var match = s.match(regex); 
        alert(RegExp.$1 + " ，服务器： " + RegExp.$2); 

案例1：
![]()![]()正则表达式

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 var s = "4444@163.com"; 7 var regex = /(.+)@(.+)/; 8             s.match(regex); 9             alert(RegExp.$1); //取得第一组即@前面的部分10             alert(RegExp.$2); //取得@后面的部分11 </script>12 </head>13 <body>14 </body>15 </html>

 18、不同浏览器的差异

<!--
（1、appendChild，insertCell，px
（2、获取网页中哪个元素触发事件
IE中使用srcElement
FireFox使用target
（3、使用Dom获取标签文本
IE中使用innerText
FireFox使用textContent
（4、动态网页绑定
Id:attachEvent
FireFox:addEventListener
（5、不同浏览器css样式支持不同：
哀悼网页使用的css只有IE才支持，FF不支持
（6、JQuery进行封装：可在不同浏览器中进行封装
解决不同浏览器中Dom的不同-->

 19、键盘码操作以及金融框案例:     

案例1：

财务相关系统中涉及到金额的文本框有如下要求：

 *进入金额文本文本框不使用中文输入法 不能输入非数字 焦点在文本框中时文本框左对齐；焦点离开文本框时文本框右对齐，显示千 分位   禁用输入法： style="ime-mode:disabled" 

*禁止键入非法值，只有这些才能被键入 (k == 9) || (k == 13) ||
(k==46)||(k==8)||(k==189)||(k==109)||(k==190)||(k==110)|| (k>=48 &&
k<=57)||(k>=96 && k<=105)||(k>=37 && k<=40) 。 onkeydown="return
numonKeyDown()" 不要写成 onkeydown="numonKeyDown()" 区分事件响应函数 和事件响应函数调用的函数。

* 禁止粘贴 ( 伟大的 Tester) ， <input onpaste="return false;" ，太暴力，应该只是禁止粘贴非法值。在 onpaste 中通过 clipboardData.getData('Text')   
   取到粘贴板中的值，然后遍历每个字符，看是否是合法的值，如果全部是合法值才允许粘贴，只要有一个非法值就禁止粘贴。 charAt 、 charCodeAt添加千分位   的方法

* 焦点在的时候左对齐没有千分位，焦点不在时右对齐千分位：this.style.textAlign='right'
![]()![]()金融文本框设置

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3  4 <head> 5 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 6 <script type="text/javascript"> 7  8 //非数字 9 function numonKeyDown() {10 var k = window.event.keyCode;11 return isValidNum(k);12         }13 //判断k是否为合法的Ascii 14 function isValidNum(k) {15 return ((k == 9) || (k == 13) || (k == 46) || (k == 8) || (k == 189) || (k == 109) || (k == 190) || (k == 110) || (k >= 48 && k <= 57) || (k >= 96 && k <= 105) || (k >= 37 && k <= 40));16         }17 //添加千分位 18 function commafy(n) {  19 //var re = /\d{1,3}(?=(\d{3})+$)/g; 20 //var n1 = n.replace(/^(\d+)/((\.\d+)?)$/,function(s,s1,s2){return s1.replace(re,"$,")+s2;}); 21             re = /(\d{1,3})(?=(\d{3})+(?:$|\D))/g;22             n1 = n.replace(re, "$1,")23 return n1;24         }25 //处理粘贴板数据 26 function numPaste() { 27 var text = window.clipboardData.getData("Text");28 for (var i = 0; i < text.length; i++) {29 var asc = text.charCodeAt(i);    //charAt→"3",charCodeAt()取到的是ASCII码 30 if (!isValidNum(asc)) { //遇到一个非法值就认为要粘贴的内容是非法的return false 31 return false;32                 }33             }34         } 35 </script>36 </head>37 <body onkeydown="if(window.event.keyCode==13){window.event.keyCode=9;}">38     不能输入非数字:39 <input type="text" style="text-align: right" onkeydown="var k=window.event.keyCode; if((k == 9) || (k == 13) || (k==46)||(k==8)||(k==189)||(k==109)||(k==190)||(k==110)|| (k>=48 && k<=57)||(k>=96 && k<=105)||(k>=37 && k<=40)){}else{return false;}" />40 <br />41     禁用输入法:42 <input type="text" style="text-align: right; ime-mode: disabled;" />43 <br />44     不能输入和粘贴非数字:45 <input type="text" style="text-align: right;" onpaste="return numPaste()" onkeydown="return numonKeyDown()" />46 <br />47     添加去掉千分位:48 <input id="t" type="text" value="95654784.75" 49         onblur="this.value=commafy(this.value);this.style.textAlign='right';"50         onfocus="this.style.textAlign='left';this.value=this.value.replace(/,/g,'')" />51 <br />52 </body>53 </html>

![]()![]()省市选择

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <!--  4 案例：实现省市选择界面。请选择省的处理，从后向前删。  5 --> 6 <head> 7 <title>欢迎来到daomul的博客，欢迎再来，谢谢（省市选择）</title> 8 <script type="text/javascript"> 9 var data = { "山东": ["济南", "青岛", "潍坊"], "河南": ["郑州", "洛阳", "三门峡"], "辽宁": ["沈阳", "鞍山", "大连"] };10 function loadProv() {11 var prov = document.getElementById("prov");12 for (var key in data) {13 var option = document.createElement("option");14                 option.value = key;15                 option.innerText = key;16                 prov.appendChild(option);17             }18         }19 function provChange() {20 var prov = document.getElementById("prov");21 var city = document.getElementById("city");22 /* 先清除旧的市列表 23                   可以采取这种方式city.option.length=0; 24                   遍历select的所有子节点，如果从前往后删，每次都会有漏网之鱼，因为有重新排号的问题 25                   从后向前删就没顺序问题了 */26 for (var i = city.childNodes.length - 1; i >= 0; i--) {27 var option = city.childNodes[i];28                 city.removeChild(option);29             }30 31 var provName = prov.value;32 var cities = data[provName]; //取出的内容["济南", "青岛", "潍坊"] 33 for (var i = 0; i < cities.length; i++) {34 var option = document.createElement("option");35                 option.value = cities[i];36                 option.innerText = cities[i];37                 city.appendChild(option);38             }39         } 40 </script>41 </head>42 <body onload="loadProv()">43 <select id="prov" onchange="provChange()">44 <option value="请选择省">--请选择省--</option>45 </select>46 <select id="city">47 </select>48 </body>49 </html>

 案例3：复选框实现全选、全不选、反选
![]()![]()复选框选择

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢（歌曲选择）</title> 5 <script type="text/javascript"> 6 function selAll() { 7 var div1 = document.getElementById("playlist"); 8 var input1 = div1.getElementsByTagName("input"); 9 for (var i = 0; i < input1.length; i++) {10 if (input1[i].type == "checkbox") {11                     input1[i].checked = "checked";12                 }13             }14         }15 function deSelAll() {16 var div1 = document.getElementById("playlist");17 var input1 = div1.getElementsByTagName("input");18 for (var i = 0; i < input1.length; i++) {19 if (input1[i].type == "checkbox") {20                     input1[i].checked = "";21                 }22             }23         }24 function reverseSel() {25 var div1 = document.getElementById("playlist");26 var input1 = div1.getElementsByTagName("input");27 for (var i = 0; i < input1.length; i++) {28 if (input1[i].type == "checkbox") {29 if (input1[i].checked == true) {30                         input1[i].checked = false;31                     }32 else{33                         input1[i].checked = true;34                     }35                 }36             }37         }38 </script>39 </head>40 <body>41 <div id="playlist">42 <input type="checkbox" id="p1" /><label for="p1">leaving into the dog</label><br />43 <input type="checkbox" id="p2" /><label for="p1">take a shower</label><br />44 <input type="checkbox" id="p3" /><label for="p1">fire heart</label><br />45 <input type="checkbox" id="p4" /><label for="p1">china</label><br />46 <p>47 <input type="button" onclick="selAll()" value="全选" />48 <input type="button" onclick="deSelAll()" value="全不选" />49 <input type="button" onclick="reverseSel()" value="反选" />50 </p>51 </div>52 </body>53 </html>

案例4：权限选择

![]()![]()权限选择

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢（权限选择）</title> 5 <script type="text/javascript"> 6 function moveSelected(selectedSrc, selectedLast) { 7 for (var i = selectedSrc.childNodes.length - 1; i >= 0; i--) { 8 var newOption = selectedSrc.childNodes[i]; 9 if (newOption.selected == true) {10                     selectedSrc.removeChild(newOption);11                     newOption.selected = false;12                     selectedLast.appendChild(newOption);13                 }14             }15         }16 function moveAll(selectedSrc, selectedLast) {17 for (var i = selectedSrc.childNodes.length - 1; i >= 0; i--) {18 var newOption = selectedSrc.childNodes[i];19                 selectedSrc.removeChild(newOption);20                 selectedLast.appendChild(newOption);21             }22         }23 </script>24 </head>25 <body>26 <select style="float:left;width:15%;height:100px;" id="select1" multiple="multiple"> 27 <option>添加</option> 28 <option>删除</option> 29 <option>修改</option> 30 <option>查询</option> 31 <option>打印</option> 32 </select> 33 <div style="float:left"> 34 <input style="float:left;width:100%;" type="button" value=">" onclick="moveSelected(document.getElementById('select1'),document.getElementById('select2'))" /> 35 <input style="float:left;width:100%;" type="button" value="<" onclick="moveSelected(document.getElementById('select2'),document.getElementById('select1'))" /> 36 <input style="float:left;width:100%;" type="button" value=">>" onclick="moveAll(document.getElementById('select1'),document.getElementById('select2'))" /> 37 <input style="float:left;width:100%;" type="button" value="<<" onclick="moveAll(document.getElementById('select2'),document.getElementById('select1'))" /> 38 </div> 39 <select style="float:left;width:15%;height:100px;" id="select2" multiple="multiple"></select>40 </body>41 </html>

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

分类: [javascript](http://www.cnblogs.com/daomul/category/419507.html)

绿色通道： [好文要顶]() [关注我]() [收藏该文]()[与我联系](http://space.cnblogs.com/msg/send/daomul) [![]()]( "分享至新浪微博")
[daomul](http://home.cnblogs.com/u/daomul/)
[关注 - 1](http://home.cnblogs.com/u/daomul/followees)
[粉丝 - 43](http://home.cnblogs.com/u/daomul/followers)

[+加关注]()

4

0
(请您对文章做出评价)

[«](http://www.cnblogs.com/daomul/archive/2013/04/17/3021929.html) 上一篇：[ADO.NET- 基础总结及实例](http://www.cnblogs.com/daomul/archive/2013/04/17/3021929.html "发布于2013-04-17 23:54")
[»](http://www.cnblogs.com/daomul/archive/2013/04/26/3037737.html) 下一篇：[JQuery 学习总结及实例](http://www.cnblogs.com/daomul/archive/2013/04/26/3037737.html "发布于2013-04-26 00:29")

posted @ 2013-04-23 00:36 [daomul](http://www.cnblogs.com/daomul/) 阅读(925) 评论(3) [编辑](http://www.cnblogs.com/daomul/admin/EditPosts.aspx?postid=3029203) [收藏]()

1、   重新导航到指定的地址：navigate("[http://www.cnblogs.com/daomul/](http://www.cnblogs.com/daomul/)");

2、

 （1、*setInterval每隔一段时间执行指定的代码，第一个参数为代码的字符串，第二个参数为间隔时间（单位毫秒），返回值为定时器的标识。如：  

              setInterval("alert('hello')",5000);

*clearInterval取消setInterval的定时执行，相当于Timer中的Enabled=False。因为setInterval可以设定多个定时，所以clearInterval要指定清除那个定时器的标识，即setInterval的返回值。

var intervalld= setInterval("alert('hello')",5000);
clearInterval(intervalld);

（2、setTimeout也是定时执行，但是不像setInterval那样是定时执行，而是设定时间后只执行一次，clearTimeout也是清除定时。
       很好区分：Interval是定时；Timeout是超时之意。

            var timeoutld=setTimeout("alert('hello')",2000);
（3、案例：实现标题栏走马灯的效果，也就是浏览器的标题文字每隔500ms向右滚动一下
![]()![]()跑马灯效果

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 function scroll() { 7 var title = document.title; 8 var first = title.charAt(0); 9 var last = title.substring(1, title.length);10                 document.title = last+first;11             }12             setInterval("scroll()",500);13 </script>14 </head>15 <body>16 17 </body>18 </html>

3、

   （1、onload:网页加载完毕时触发，浏览器是一边下载文档、一边解析执行，可能会出现JavaScript执行时需要操作某个元素，这个元素还没有加载，如果这样就要把操作的代码放到body的onload事件中，或者可以把JavaScript放到元素之后。元素的onload事件是元素自己加载完毕时触发，而body 里的onload才是全部加载完成。
   （2、onunload：网页关闭（或者离开）后触发。onbeforeunload：窗口离开（比如前进、后退、关闭之前）就会弹出确认消息。如：                                                     <body onbeforeunload="Window.event.returnValue='真的要放弃发贴退出吗？'"> 

4、

     除了有特有的属性之外，当然还有通用的HTML元素的事件：onclick(单击)、ondblclick(双击)、onkeydown(按键按下)、onkeyup(按键释放)、onkeypress(点击按键)、onmousedown(鼠标按下)、onmousemove(鼠标移动)、onmouseout(鼠标离开元素范围)、
onmouseover(鼠标移动到元素范围)、onmouseup(鼠标按键释放)等。

5、window对象的属性

（1、window.location.href="[http://www.sina.com.cn](http://www.sina.com.cn/)",重新导向新的地址，和navigate方法效果一样。window.location.reload()刷新页面。
（2、window.event是非常重要的属性，用来获得发生事件时的信息，事件不局限于window对象的事件，所有元素的事件都可以通过event属性取到相关信息。
         **a**、altKey属性，boot类型，表示发生事件时alt键是否被按下，类似的还有ctrlKey、shiftKey属性
![]()![]()windows事件样例

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title></title> 5 </head> 6 <body> 7 <input type="button" value="href" onclick="alert(location.href)" /> 8 <input type="button" value="重定向" onclick="location.href='页面1.htm'" /> 9 <input type="button" value="点击" onclick="if(window.event.ctrlKey){alert('按下了Ctrl')}else{alert('普通点击')}" /><a10 href="http://www.baidu.com" onclick="alert('禁止访问！');window.event.returnValue=false;"></a>11 <form action="daomul.aspx">12 <input type="submit" value="提交" onclick="alert('数据有问题!');window.event.returnValue=false;" />13 </form>14 </body>15 </html>

 

**b**、 clientX、clientY 发生事件时鼠标在客户区（浏览器界页面内）的坐标；screenX、screenY发生事件时鼠标在屏幕上的坐标；offsetX、offsetY发生事件时鼠标相对于事件源（按钮button内）的坐标。
**c**、returnValue属性，如果将returnValue设置为false，就会取消默认事件的处理。
**d**、srcElement:获得事件源对象
**e**、KeyCode：发生时间时的按键值
**f**、button：发生时间时鼠标的按键，1为左键，2为右键，3为左右键同时按。

         <body onmousedown="if(event.button==2){alert('禁止复制')}">

6、clipboardData对象，对粘贴板的操作。clearData("Text")清空粘贴板；getData("Text")读取粘贴板的值，返回值为粘贴板中的内容；setData("Text",val),设置粘贴板中的值。

（1、当复制的时候body的oncopy方法被触发，直接return false就是禁止复制。<body oncopy="alert('禁止复制！');return false;">
（2、很多元素也有oncopy、onpaste事件。

 例子1：禁止复制
                    <body oncopy="alert('禁止复制！');return false;">
 例子2：给粘贴板赋值：复制地址给好友

<input type="button" value="分享本页给好友" onclick="clipboardData.setData('Text','我发现一个很实用的网站，计算机方面的!'+ 
location.href);alert(' 已经将地址放到粘贴板中，赶快通过QQ传递给你的好友吧！');" />

 例子3：禁止粘贴到文本框

请输入您的手机号码：<input type="text" />
请您再次输入手机号码：<input type="text" onpaste="alert('为保证您充值到正确的手机号，请勿粘贴手机号，请直接填！');return false;"/>:

 例子4：复制时附带内容

在网站中复制文章的时候，为了防止那些拷贝党不添加文章来源，自动在复制的内容后添加版权声明。

function modifyClipboard(){

   clipboardData.setData('Text',clipboardData.getData('Text')+'本文来自博客园技术专区，转载请注明来源。'+location.href);
}
oncopy="setTimeout('modifyClipboard()',100)"。

用户复制动作发生0.1秒以后再去修改粘贴板中的内容。100ms只是一个经常取值，写1000、10、50、20……都行。不能直接在oncopy中执行对粘贴板的操作，因此设定定时器，0.1秒以后执行，这样就不再oncopy的执行调用栈上了。

7、页面前进、后退：history操作历史记录

window.history.back()后退；
window.history.forward()前进。也可以用window.history.go(-1)表前进；window.history.go(1)表后退。

实例1： 

<body>这里是第2页<a href="javascript:window.history.back()">后退</a><input type="button" value="后退" onclick="window.history.go(-1)" />
</body>

8、document属性（最复杂的属性）document是window对象的一个属性，因为使用window对象成员的时候可以省略window,所以一般直接写document。

（1、write:向文档中写入内容。writeln和write差不多，只不过最后添加一个回车
（2、<input type="button" value="点击" onclick="document.write('<font color=blue>您好</font>')" />
（3、在onclick等事件中写的代码会冲掉页面中的内容，只有在页面加载过程中write才会与原有内容融合在一起
（4、<script type="text/javascript">
                 document.write("<font color=red>您好</font>");
        </script>

案例1：
![]()![]()getElementById

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>getElementById</title> 5 <script type="text/javascript"> 6 //不建议直接通过id操作元素，而是通过getElementById根据元素的Id获得对象 7 function btnClick() { 8 //alert(textbox1.value); 9 var txt = document.getElementById("textbox1");10             alert(txt.value);11         }12 function btnClick2() {13 //alert(form1.textbox2.value);//要显示表单里的信息需添加表单名称14 var txt = document.getElementById("textbox2");15             alert(txt.value);16         }17 </script>18 </head>19 <body>20 <input type="text" id="textbox1" />21 <input type="button" value="点击我" onclick="btnClick()" />22 <form action="页面1.htm" id="form1">23 <input type="text" id="textbox2" />24 <input type="button" value="显示刚输入的内容" onclick="btnClick2()" />25 </form>26 </body>27 </html>

案例2：

![]()![]()getElementByName

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>getElementByName的例子</title> 5 <script type="text/javascript"> 6 function btnClick() { 7 var radios = document.getElementsByName("gender"); 8 /*//在JS中for(var r in radios)不像C#中的foreach,并不会遍历每个元素,而是遍历的key 9             for (var r in radios) {10             alert(r.value);11             }*/12 for (var i = 0; i < radios.length; i++) {13 var radio = radios[i];14                 alert(radio.value);15             }16         }17 function btnClick2() {18 var inputs = document.getElementsByTagName("input");19 for (var i = 0; i < inputs.length; i++) {20 var input = inputs(i);21                 input.value = "welcome to my room";22             }23         }24 </script>25 </head>26 <body>27 <input type="radio" name="gender" value="男" />男28 <input type="radio" name="gender" value="女" />女29 <input type="radio" name="gender" value="保密" />保密30 <input type="button" value="click" onclick="btnClick()" />31 <br />32 <input type="text" /><br />33 <input type="text" /><br />34 <input type="text" /><br />35 <input type="text" /><br />36 <input type="text" /><br />37 <input type="button" value="bytagname" onclick="btnClick2()" />38 </body>39 </html>

案例3：

![]()![]()getElementByTagName

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>getElementByTagName</title> 5 <script type="text/javascript"> 6 function initEvent() { 7 var inputs = document.getElementsByTagName("input"); 8 for (var i = 0; i < inputs.length; i++) { 9 var input = inputs[i];10                 input.onclick = btnClick;11             }12         }13 function btnClick() {14 var inputs = document.getElementsByTagName("input");15 for (var i = 0; i < inputs.length; i++) {16 var input = inputs[i];17 //window.event.srcElement//取得引发事件的控件18 if (input == window.event.srcElement) {19                     input.value = "哈哈"; //以下五个按钮，点到的那个就变为“哈哈”，其余都为“呜呜”，点“呜呜”就变为“哈哈”20                 }21 else {22                     input.value = "呜呜";23                 }24             }25         }26 </script>27 </head>28 <body onload="initEvent()">29 <input type="button" value="欢迎进入" />30 <input type="button" value="欢迎进入" />31 <input type="button" value="欢迎进入" />32 <input type="button" value="欢迎进入" />33 <input type="button" value="欢迎进入" />34 </body>35 </html>

案例4：

![]()![]()阅读协议等待计时器

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6  7 var leftTime = 10; 8 var TimeID; 9 function scroll() {//alert(222);10 var btn = document.getElementById("btnAgree");11 //如果网页速度非常慢的话，可能定时器运行的时候控件还没有加载！12 if (btn) {//alert(11);13 if (leftTime <= 0) {14                         btn.value = "同意";15                         btn.disabled = "";16                         clearInterval(TimeID);17                     }18 else {19                         btn.value = "请阅读协议，同意（还剩下"+leftTime+"秒）";20                         leftTime--;21                     }22                 }23             }24             TimeID=setInterval("scroll()", 1000);25 </script>26 </head>27 <body>28 <textarea id="TextArea1" cols="20" rows="2"></textarea>29 <input id="btnAgree" type="button" value="同意" disabled="disabled"/>30 </body>31 </html>

案例5：

![]()![]()美女时钟

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 //var now = new Date();不能放在这，否则取得时间不变 7 function Fill2Len(i) { 8 if (i < 10) { 9 return "0" + i;10             }11 else {12 return i;13             }14         }15 function Refresh() {16 var imgMM = document.getElementById("imgMM");17 if (!imgMM) {18 return; //没加载就直接return19             }20 var now = new Date(); //每次刷新时取得时间21 var filename = Fill2Len(now.getHours()) + "_" + Fill2Len(now.getSeconds()) + ".jpg";22             imgMM.src = "MMClock/" + filename;23         }24         setInterval("Refresh()", 1000);25 </script>26 </head>27 <body onload="Refresh()">28 <img id="imgMM" src="" />29 </body>30 </html>

案例6：

![]()![]()五角星评分

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 function indexOf(arr,element) { 7 for (var i = 0; i < arr.length; i++) { 8 if (arr[i] == element) { 9 return i;10                     }11                 }12 return -1;13             }14 15 function myload() {16 //获得table下面的所有td17 var table1 = document.getElementById("NewTalbeId");18 var td1 = table1.getElementsByTagName("td");19 for (var i = 0; i < td1.length; i++) {20                     td1[i].onclick = onmyBlur;21 //鼠标移到上面变手势22                     td1[i].style.cursor = "pointer";23                 }24             }25 26 function onmyBlur() {27 var table1 = document.getElementById("NewTalbeId");28 var td1 = table1.getElementsByTagName("td");29 var index = indexOf(td1, this);30 for (var i = 0; i <= index; i++) {31                     td1[i].style.background = "yellow";32                 }33 for (var i = index + 1; i < td1.length; i++) {34                     td1[i].style.background = "white";35                 }36             }37 </script>38 </head>39 <body onload="myload()">40 <table border="0" cellpadding="0" cellspacing="0" id="NewTalbeId">41 <tr>42 <td>☆</td><td>☆</td><td>☆</td><td>☆</td><td>☆</td>43 </tr>44 </table>45 </body>46 </html>

 

9、Dom的动态创建

     （1、document.write只能在页面加载过程中才能动态创建。
     （2、可以调用document的createElement方法来创建具有指定标签的DOM对象，然后通过调用某个元素的appendChild方法将新创建元素添加到相应的元素下
![]()![]()动态创建按钮

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 function clickButton() { 7 var divMain = document.getElementById("divMain"); //获得层 8 var btn = document.createElement("input"); //创建一个标签名为input的元素（这时还没添加到任何元素上，所以没有显示） 9             btn.type = "button";10             btn.value = "新动态按钮";11             divMain.appendChild(btn); //将动态产生的元素添加到divMain层（容器）12         }13 </script>14 </head>15 <body>16 <div id="divMain"></div>17 <input type="button" value="OK" onclick="clickButton()" />18 </body>19 </html>

10、innerText与innerHTML

     （1、几乎所有DOM元素都有innerText与innerHTML属性（注意大小写），分别是元素标签内内容的文本表示形式和HTML源代码，这两个属性是可读可写的。

                 <a href="[http://cnblog.com/daomul/](http://blog.sina.com.cn/)" id="link1">博<font color="red"/>客</font>园</a>
                  <input type="button" value="innerXXX" onclick="alert(document.getElementById('link1').innerText);
                                                                                        alert(document.getElementById('link1').innerHTML);" />

     （2、用innerHTML也可以替代createElement,属于简单、粗放型、后果自负的创建。

function createInput() {
            var divMain = document.getElementById('divMain');
           divMain.innerHTML="<a href='[http://cnblog.com/daomul/](http://blog.sina.com.cn/)'>daomul的博客</a>";
}

 11、浏览器兼容性问题：

       （1、IE6、IE7对table.appendChlid("tr")的支持和IE8不一样，用insertRow、 insertCell来代替或者为表格添加tbody，然后向tbody中添加tr。个人比较喜欢          

              下面的Insert方式，它不是不兼容火狐浏览器，它也是基本兼容的了，只是 (FF)不支持innerText，但是支持innerHtml。

   实例：动态增加双行网站列表

    
![]()![]()采用InsertRow和insertCell

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>动态创建表格的兼容性</title> 5 <script type="text/javascript"> 6  7 function LoadData() {//方法一 8 var data = { "百度": "http://www.baidu.com", "新浪": "http://www.sina.com", "淘宝": "http://www.taobao.com" }; 9 var tableLinks2 = document.getElementById("tableLinks2");10 for (var key in data) {11 var value = data[key];12 var tr = tableLinks2.insertRow(-1); //在表格最后插入一行，并且返回插入行的对象。联想Winform中TreeNode node=treeNode.AddNode("hello")13 14 var td1 = tr.insertCell(-1); //在tr中增加一个td，并且返回增加的td，然后td中放入内容15                 td1.innerText = key; //FF不支持innerText16 var td2 = tr.insertCell(-1);17                 td2.innerHTML = "<a href='" + value + "'>" + value + "</a>"; //也可以用createElement18             }19         }20 </script>21 </head>22 <body>23 <table id="tableLinks">24 <tbody>25 </tbody>26 </table>27 <table id="tableLinks2">28 </table>29 <input type="button" value="非兼容性加载数据" onclick="LoadData()" />30 </body>31 </html>

![]()![]()采用appendChild方法

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>动态创建表格的兼容性</title> 5 <script type="text/javascript"> 6  7  8 function LoadDataGood() {//方法二 9 var data = { "百度": "http://www.baidu.com", "新浪": "http://www.sina.com", "淘宝": "http://www.taobao.com" };10 var tableLinks = document.getElementById("tableLinks");11 for (var key in data) {12 var value = data[key];13 var tr = document.createElement("tr");14 15 var td1 = document.createElement("td"); //先创建td,在td中放入内容，再加入tr16                 td1.innerText = key;17                 tr.appendChild(td1);18 19 var td2 = document.createElement("td");20                 td2.innerHTML = "<a href='" + value + "'>" + value + "</a>";21                 tr.appendChild(td2);22 23                 tableLinks.tBodies[0].appendChild(tr); //并且在<table>标签里要加上<tbody></tbody>24             }25         } //动态产生的元素，查看源代码是看不到的。但可以通过DebugBar->Dom->Document->HTML可以看到26 27 28 29 </script>30 </head>31 <body>32 <table id="tableLinks">33 <tbody>34 </tbody>35 </table>36 <table id="tableLinks2">37 </table>38 <input type="button" value="兼容性  加载数据" onclick="LoadDataGood()" />39 </body>40 </html>

 12、事件冒泡

       如过元素A嵌套在元素B中，那么A被点击不仅A的onclick事件会被触发，B也会，顺序是由内而外。

 13、事件中的this

      除了可以使用event.srcElement在事件响应函数中，this表示发生事件的控件。只有在事件响应函数才能使用this获得发生事件的控件，在事件响应函数调用的函数  中不能使用，如果要使用则要将this传递给函数或者使用event.srcElement. 
  案例： 
![]()![]()this事件

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 <script type="text/javascript"> 7 function btnClick3() {//在事件响应函数调用的函数里就不能通过this获得事件对象了 8             alert(this.value);//弹出undefined 9         }10 function btnClick4(btn) {//在事件响应函数中将this传过来就可以11             alert(btn.value);12         }13 function btnClick5() {//this与event.srcElement语义是不一样的，this就表示当前监听事件的这个对象，event.srcElement是引发事件的对象14             alert(event.srcElement.value);15         }16 </script>17 </head>18 <body>19 <input type="button" value="Logan" onclick="alert(event.srcElement.value)" />20 <input type="button" value="Logan2" onclick="alert(this.value)" />21 <input type="button" value="Logan3" onclick="btnClick3()" />22 <input type="button" value="Logan4" onclick="btnClick4(this)" />23 <input type="button" value="Logan5" onclick="btnClick5()" />24 </body>25 </html>

14、Dom修改样式

     （1、Dom中修改css样式，必须是通过className而不是class(可能是因为作为关键字)

            例如：onclick="document.getElementById('htmldivID').className='cssID'; "

            例如：获得body的css样式id：document.body.className

    （2、单独修改样式的属性使用“style.属性名”，不过要注意存在“-”的属性，因为javascript中横线代表减号

 案例1：ontextBlur()是myload的响应函数，而不是被响应函数调用的函数，所以可以用this来获得
![]()![]()修改Dom样式-鼠标移离

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 function myload() {//alert(1); 7 var texts = document.getElementsByTagName("textarea"); 8 for (var i = 0; i < texts.length; i++) { 9 //alert(2);10 var text = texts[i];11                     text.onblur = ontextBlur;12                 }13             }14 15 function ontextBlur() {16 //alert(3);17 if (this.value <= 0) {18 //alert(4);19 this.style.background = "red";20                 }21 else {22 this.style.background = "write";23                 }24             }25 </script>26 </head>27 <body onload="myload()">28 <textarea id="TextArea1" cols="20" rows="2" ></textarea>29 </body>30 </html>

案例2：评分

![]()![]()评分

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 function indexOf(arr,element) { 7 for (var i = 0; i < arr.length; i++) { 8 if (arr[i] == element) { 9 return i;10                     }11                 }12 return -1;13             }14 15 function myload() {16 //获得table下面的所有td17 var table1 = document.getElementById("NewTalbeId");18 var td1 = table1.getElementsByTagName("td");19 for (var i = 0; i < td1.length; i++) {20                     td1[i].onclick = onmyBlur;21 //鼠标移到上面变手势22                     td1[i].style.curser = "pointer";23                 }24             }25 26 function onmyBlur() {27 var table1 = document.getElementById("NewTalbeId");28 var td1 = table1.getElementsByTagName("td");29 var index = indexOf(td1, this);30 for (var i = 0; i <= index; i++) {31                     td1[i].style.background = "yellow";32                 }33 for (var i = index + 1; i < td1.length; i++) {34                     td1[i].style.background = "white";35                 }36             }37 </script>38 </head>39 <body onload="myload()">40 <table border="0" cellpadding="0" cellspacing="0" id="NewTalbeId">41 <tr>42 <td>☆</td><td>☆</td><td>☆</td><td>☆</td><td>☆</td>43 </tr>44 </table>45 </body>46 </html>

 15、编程控制层

（1、元素的position样式值：static（无定位，显示在默认位置）、absolute(绝对位置)、fixed（相对于窗口的固定定位，位置不会随着浏览器的滚动而变化，IE6不支持）、relative(相对元素默认位置的定位)。如果要通过代码修改元素的坐标则一般使用absolute，然后修改元素的top（上边缘距离）、left（左边缘距离）两个样式值。

 （2、createElement的两种用法，注意innerHTML的问题，

       var input=document.createElement("<a href=‘[http://www.cnblogs.com/daomul](http://www.cnblogs.com/daomul)’>文本</a>")文本部分不会被识别

（3、设定一些DOm元素属性名的特殊属性：例如

       label.setAttribute("for","username");  label.setAttribute("aaa","111");

案例1：高级选项的显示隐藏
![]()![]()高级选项的显示隐藏

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 function showSomething(sb) { 7 var div1 = document.getElementById("div1"); 8 if (sb.checked) { 9                     div1.style.display = "";10                 }11 else {12                     div1.style.display = "none";13                 }14             }15 </script>16 </head>17 <body>18 <input type="checkbox" id="checkin" onclick="showSomething(this)"/><label for="">显示高级选项</label>19 <div id="div1" style="display:none"> 20             显示被隐藏的高级选项21 </div>22 </body>23 </html>

 案例2：鼠标移动（1）

![]()![]()鼠标移动图片

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6             document.onmousemove = function () { 7 var x = window.event.clientX; 8 var y = window.event.clientY; 9 var div1 = document.getElementById("div1");10 11 if (!div1) { return; }12                 div1.style.left = x;13                 div1.style.top = y;14             }15 </script>16 </head>17 <body>18 <div id="div1" style="position:absolute"> 19 <img src="images\DSCF0762.JPG" alt="Alternate Text" style="width:100px;height:100px"/> 20 <br/>美女与你同在21 </div>22 </body>23 </html>

 案例3：鼠标移动显示信息（2）

![]()![]()鼠标移动显示信息

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <style type="text/css"> 6            .mydiv 7 { 8               border-style:dotted; 9               border-width:2px;10               border-color:Green;11               background-color:Gray;12 }13 </style>14 <script type="text/javascript">15 function InitLoad() {16 var links=document.getElementsByTagName("a"

);
 17 for (var i = 0; i < links.length; i++) {18                   links[i].onmouseover = linkMouseOver;19                   links[i].onmouseout=linkMouseOut;20               }21           }22 function linkMouseOut() {23 var div2 = document.getElementsByTagName("div");24 for (var i = 0; i < div2.length; i++) {25 if (div2[i].className == "mydiv") {26 27                       document.body.removeChild(div2[i]); //remove的是div2[i]不是div228 //div2.style.dispaly = "none"; //只是隐藏没有消去29                   }30               }31           }32 33 function linkMouseOver() {34 var div1 = document.createElement("div");35               div1.className = "mydiv";36               div1.style.position = "absolute";37               div1.style.left = window.event.clientX;38               div1.style.top = window.event.clientY;39               div1.innerHTML = "nimei nimie nimei nimei ";40 41               document.body.appendChild(div1);42           }43 44 45 </script>46 </head>47 <body onload="InitLoad()">48 <a href="#">daomul</a>49 <a href="#">ddddd</a>50 <a href="#">aaaaa</a>51 <a href="#">bbbbb</a>52 </body>53 </html>

 案例4：小图片显示大图片

![]()![]()小图片显示大图片

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢(小图片显示大图片1)</title> 5 <script type="text/javascript"> 6 function InitLoad() { 7 var data = { "images/1小.bmp": ["images/1.JPG", "zzz", "21"], 8 "images/2小.bmp": ["images/2.jpg", "yyy", "22"], 9 "images/3小.bmp": ["images/3.JPG", "xxx", "23"]10                 };11 for(var smallImg in data){12 //创建13 var smallIm=document.createElement("img");14 //属性(因为图片的路径在存入和取出的过程中不同，15 //所以还是自定义属性，)16                     smallIm.src = smallImg;17                     smallIm.style.width = "100px";18                     smallIm.style.height = "100px";19                     smallIm.setAttribute("a1",data[smallImg][0]);20                     smallIm.setAttribute("a2",data[smallImg][1]);21                     smallIm.setAttribute("a3",data[smallImg][2]);22 23 //方法24                     smallIm.onmouseover = function () {25 //26                         document.getElementById("imgId").src = this.getAttribute("a1");27                         document.getElementById("NameId").innerHTML = this.getAttribute("a2");28                         document.getElementById("AgeId").innerHTML = this.getAttribute("a3");29 //显示DIV30 var div1 = document.getElementById("showDivId");31                         div1.style.left = window.event.clientX;32                         div1.style.top = window.event.clientY;33                         div1.style.display = '';34 //alert(1);35                     };36 //关联绑定37                     document.body.appendChild(smallIm);  38                 }39           }40 41 </script>42 </head>43 <body onload="InitLoad()">44 <div style="display:none;position:absolute;" id="showDivId">45 <img src="" alt="error Pic ,please call the Admin"  id="imgId" style="width:200px;height:200px;"/>46 <p id="NameId"></p>47 <p id="AgeId"></p>48 </div>49 </body>50 </html>

 案例5：评分控件改进

![]()![]()评分控件改进

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢(评分改进2)</title> 5 <script type="text/javascript"> 6 function indexOf(arr,element) { 7 for (var i = 0; i < arr.length; i++) { 8 if(arr[i]==element){ 9 return i;10                    }11                 }12 return -1;13             }14 15 16 function InitLoad() {17 var table1 = document.getElementById("myTable"); 18 var td1 = table1.getElementsByTagName("td");19 for (var i = 0; i < td1.length; i++) {20                     td1[i].style.cursor = 'pointer';21                     td1[i].onmouseover = function () {22 23 var table1 = document.getElementById("myTable");24 var td1 = table1.getElementsByTagName("td");25 var index = indexOf(td1, this);26 for (var i = 0; i <= index; i++) {27                             td1[i].innerText = "★";28                         }29 for (var i = index + 1; i < td1.length; i++) {30                             td1[i].innerText = "☆";31                         }32                     };33                 }34           }35 36 </script>37 </head>38 <body onload="InitLoad()">39 <table border="0" cellpadding="0" cellspacing="0" id="myTable">40 <tr>41 <td>☆42 </td>43 <td>☆44 </td>45 <td>☆46 </td>47 <td>☆48 </td>49 <td>☆50 </td>51 </tr>52 </table>53 </body>54 </html>

案例6：搜索框关键字搜索

![]()![]()搜索框关键字搜索

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢(文本框关键字3)</title> 5 <script type="text/javascript"> 6 function inputBlur(keyword) { 7 if (keyword.value.length <= 0) { 8                 keyword.value = "请输入搜索关键词"; 9                 keyword.style.color = 'Gray';10             }11         }12 function inputFocus(keyword) {13 //var keyword = document.getElementById("keyword");把this传给两个方法，省去了getElementById依赖id14 if (keyword.value == "请输入搜索关键词") {15                 keyword.value = "";16                 keyword.style.color = 'Black';17             }18         }19 </script>20 </head>21 <body onload="InitLoad()">22 <input onblur="inputBlur(this)" onfocus="inputFocus(this)" value="请输入搜索关键词" style="color: Gray" />23 <input type="button" value="搜索" />24 </body>25 </html>

16、Form表单： Form 对象是表单的 Dom 对象。

 方法： submit() 提交表单，但是不会触发 onsubmit 事件。 实现 autopost ，也就是焦点离开控件以后页面立即提交，而不是 只有提交 submit 按钮以后才提交，当光标离开的时候触发 onblur 事件，在 onblur 中调用 form 的 submit 方法。在点击 submit 后 form 的 onsubmit 事件被触发 ，在 onsubmit 中可以 进行数据校验，数据有问题， 返回 false 即可取消提交。

 案例1：
![]()![]()Form表单

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢(Form表单提交4)</title> 5 <script type="text/javascript"> 6  7 </script> 8 </head> 9 <body onload="InitLoad()">10 <form action="default.aspx" id="form1" onsubmit="return false;">11 <!--通过click函数"欺骗"其他按钮触发-->12 <input type="button" id="btn1" onclick="alert('触发了其他button了')" value="按钮" />13 <input type="button" onclick="document.getElementById('btn1').click()" value="同样提交" />14 <!--因为return false，所以不可提交表单，但是调用submit(),却可以触发方法-->15 <input type="submit" onclick="document.getElementById('form1').submit()" value="可提交的submit" />16 <input type="submit" value="不可提交的submit" />17 <!--类似于asp.net中的autopostback(相同的还有文本框输入后移开的onblur方法的submit调用)-->18 <select id="Select1" onchange="document.getElementById('form1').submit()">19 <option>111</option>20 <option>222</option>21 <option>333</option>22 <option>444</option>23 </select>24 </form>25 </body>26 </html>

17、正则表达式 

 *JavaScript 中创建正则表达式类的方法： 
            var regex = new RegExp("\\d{5}")  或者   var regex = /\d{5}/ 
           / 表达式 / 是 JavaScript 中专门为简化正则表达式编写而提供的语法， 
           写在 // 中的正则表达式就不用管转义符了。 
 *RegExp 对象的方法： 
      **  test(str) 判断字符串 str 是否匹配正则表达式，相当于 IsMatch 
          var regex = /.+@.+/; 
         alert(regex.test("a@b.com")); 
         alert(regex.test("ab.com")); 
     **  exec(str) 进行搜索匹配，返回值为匹配结果 ( * ) 
     **  compile 编译表达式，提高运行速度。   ( * ) 
*String 对象中提供了一些与正则表达式相关的方法，相当于对于 
               RegExp 类的包装，简化调用： 
              match(regexp) ,相当于调用 exec 

        var s = "aaa@163.com"; 
        var regex = /(.+)@(.+)/; 
        var match = s.match(regex); 
        alert(RegExp.$1 + " ，服务器： " + RegExp.$2); 

案例1：
![]()![]()正则表达式

1 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"> 2 <html> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 5 <script type="text/javascript"> 6 var s = "4444@163.com"; 7 var regex = /(.+)@(.+)/; 8             s.match(regex); 9             alert(RegExp.$1); //取得第一组即@前面的部分10             alert(RegExp.$2); //取得@后面的部分11 </script>12 </head>13 <body>14 </body>15 </html>

 18、不同浏览器的差异

<!--
（1、appendChild，insertCell，px
（2、获取网页中哪个元素触发事件
IE中使用srcElement
FireFox使用target
（3、使用Dom获取标签文本
IE中使用innerText
FireFox使用textContent
（4、动态网页绑定
Id:attachEvent
FireFox:addEventListener
（5、不同浏览器css样式支持不同：
哀悼网页使用的css只有IE才支持，FF不支持
（6、JQuery进行封装：可在不同浏览器中进行封装
解决不同浏览器中Dom的不同-->

 19、键盘码操作以及金融框案例:     

案例1：

财务相关系统中涉及到金额的文本框有如下要求：

 *进入金额文本文本框不使用中文输入法 不能输入非数字 焦点在文本框中时文本框左对齐；焦点离开文本框时文本框右对齐，显示千 分位   禁用输入法： style="ime-mode:disabled" 

*禁止键入非法值，只有这些才能被键入 (k == 9) || (k == 13) ||
(k==46)||(k==8)||(k==189)||(k==109)||(k==190)||(k==110)|| (k>=48 &&
k<=57)||(k>=96 && k<=105)||(k>=37 && k<=40) 。 onkeydown="return
numonKeyDown()" 不要写成 onkeydown="numonKeyDown()" 区分事件响应函数 和事件响应函数调用的函数。

* 禁止粘贴 ( 伟大的 Tester) ， <input onpaste="return false;" ，太暴力，应该只是禁止粘贴非法值。在 onpaste 中通过 clipboardData.getData('Text')   
   取到粘贴板中的值，然后遍历每个字符，看是否是合法的值，如果全部是合法值才允许粘贴，只要有一个非法值就禁止粘贴。 charAt 、 charCodeAt添加千分位   的方法

* 焦点在的时候左对齐没有千分位，焦点不在时右对齐千分位：this.style.textAlign='right'
![]()![]()金融文本框设置

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3  4 <head> 5 <title>欢迎来到daomul的博客，欢迎再来，谢谢</title> 6 <script type="text/javascript"> 7  8 //非数字 9 function numonKeyDown() {10 var k = window.event.keyCode;11 return isValidNum(k);12         }13 //判断k是否为合法的Ascii 14 function isValidNum(k) {15 return ((k == 9) || (k == 13) || (k == 46) || (k == 8) || (k == 189) || (k == 109) || (k == 190) || (k == 110) || (k >= 48 && k <= 57) || (k >= 96 && k <= 105) || (k >= 37 && k <= 40));16         }17 //添加千分位 18 function commafy(n) {  19 //var re = /\d{1,3}(?=(\d{3})+$)/g; 20 //var n1 = n.replace(/^(\d+)/((\.\d+)?)$/,function(s,s1,s2){return s1.replace(re,"$,")+s2;}); 21             re = /(\d{1,3})(?=(\d{3})+(?:$|\D))/g;22             n1 = n.replace(re, "$1,")23 return n1;24         }25 //处理粘贴板数据 26 function numPaste() { 27 var text = window.clipboardData.getData("Text");28 for (var i = 0; i < text.length; i++) {29 var asc = text.charCodeAt(i);    //charAt→"3",charCodeAt()取到的是ASCII码 30 if (!isValidNum(asc)) { //遇到一个非法值就认为要粘贴的内容是非法的return false 31 return false;32                 }33             }34         } 35 </script>36 </head>37 <body onkeydown="if(window.event.keyCode==13){window.event.keyCode=9;}">38     不能输入非数字:39 <input type="text" style="text-align: right" onkeydown="var k=window.event.keyCode; if((k == 9) || (k == 13) || (k==46)||(k==8)||(k==189)||(k==109)||(k==190)||(k==110)|| (k>=48 && k<=57)||(k>=96 && k<=105)||(k>=37 && k<=40)){}else{return false;}" />40 <br />41     禁用输入法:42 <input type="text" style="text-align: right; ime-mode: disabled;" />43 <br />44     不能输入和粘贴非数字:45 <input type="text" style="text-align: right;" onpaste="return numPaste()" onkeydown="return numonKeyDown()" />46 <br />47     添加去掉千分位:48 <input id="t" type="text" value="95654784.75" 49         onblur="this.value=commafy(this.value);this.style.textAlign='right';"50         onfocus="this.style.textAlign='left';this.value=this.value.replace(/,/g,'')" />51 <br />52 </body>53 </html>

![]()![]()省市选择

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <!--  4 案例：实现省市选择界面。请选择省的处理，从后向前删。  5 --> 6 <head> 7 <title>欢迎来到daomul的博客，欢迎再来，谢谢（省市选择）</title> 8 <script type="text/javascript"> 9 var data = { "山东": ["济南", "青岛", "潍坊"], "河南": ["郑州", "洛阳", "三门峡"], "辽宁": ["沈阳", "鞍山", "大连"] };10 function loadProv() {11 var prov = document.getElementById("prov");12 for (var key in data) {13 var option = document.createElement("option");14                 option.value = key;15                 option.innerText = key;16                 prov.appendChild(option);17             }18         }19 function provChange() {20 var prov = document.getElementById("prov");21 var city = document.getElementById("city");22 /* 先清除旧的市列表 23                   可以采取这种方式city.option.length=0; 24                   遍历select的所有子节点，如果从前往后删，每次都会有漏网之鱼，因为有重新排号的问题 25                   从后向前删就没顺序问题了 */26 for (var i = city.childNodes.length - 1; i >= 0; i--) {27 var option = city.childNodes[i];28                 city.removeChild(option);29             }30 31 var provName = prov.value;32 var cities = data[provName]; //取出的内容["济南", "青岛", "潍坊"] 33 for (var i = 0; i < cities.length; i++) {34 var option = document.createElement("option");35                 option.value = cities[i];36                 option.innerText = cities[i];37                 city.appendChild(option);38             }39         } 40 </script>41 </head>42 <body onload="loadProv()">43 <select id="prov" onchange="provChange()">44 <option value="请选择省">--请选择省--</option>45 </select>46 <select id="city">47 </select>48 </body>49 </html>

 案例3：复选框实现全选、全不选、反选
![]()![]()复选框选择

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢（歌曲选择）</title> 5 <script type="text/javascript"> 6 function selAll() { 7 var div1 = document.getElementById("playlist"); 8 var input1 = div1.getElementsByTagName("input"); 9 for (var i = 0; i < input1.length; i++) {10 if (input1[i].type == "checkbox") {11                     input1[i].checked = "checked";12                 }13             }14         }15 function deSelAll() {16 var div1 = document.getElementById("playlist");17 var input1 = div1.getElementsByTagName("input");18 for (var i = 0; i < input1.length; i++) {19 if (input1[i].type == "checkbox") {20                     input1[i].checked = "";21                 }22             }23         }24 function reverseSel() {25 var div1 = document.getElementById("playlist");26 var input1 = div1.getElementsByTagName("input");27 for (var i = 0; i < input1.length; i++) {28 if (input1[i].type == "checkbox") {29 if (input1[i].checked == true) {30                         input1[i].checked = false;31                     }32 else{33                         input1[i].checked = true;34                     }35                 }36             }37         }38 </script>39 </head>40 <body>41 <div id="playlist">42 <input type="checkbox" id="p1" /><label for="p1">leaving into the dog</label><br />43 <input type="checkbox" id="p2" /><label for="p1">take a shower</label><br />44 <input type="checkbox" id="p3" /><label for="p1">fire heart</label><br />45 <input type="checkbox" id="p4" /><label for="p1">china</label><br />46 <p>47 <input type="button" onclick="selAll()" value="全选" />48 <input type="button" onclick="deSelAll()" value="全不选" />49 <input type="button" onclick="reverseSel()" value="反选" />50 </p>51 </div>52 </body>53 </html>

案例4：权限选择

![]()![]()权限选择

1 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 2 <html xmlns="http://www.w3.org/1999/xhtml"> 3 <head> 4 <title>欢迎来到daomul的博客，欢迎再来，谢谢（权限选择）</title> 5 <script type="text/javascript"> 6 function moveSelected(selectedSrc, selectedLast) { 7 for (var i = selectedSrc.childNodes.length - 1; i >= 0; i--) { 8 var newOption = selectedSrc.childNodes[i]; 9 if (newOption.selected == true) {10                     selectedSrc.removeChild(newOption);11                     newOption.selected = false;12                     selectedLast.appendChild(newOption);13                 }14             }15         }16 function moveAll(selectedSrc, selectedLast) {17 for (var i = selectedSrc.childNodes.length - 1; i >= 0; i--) {18 var newOption = selectedSrc.childNodes[i];19                 selectedSrc.removeChild(newOption);20                 selectedLast.appendChild(newOption);21             }22         }23 </script>24 </head>25 <body>26 <select style="float:left;width:15%;height:100px;" id="select1" multiple="multiple"> 27 <option>添加</option> 28 <option>删除</option> 29 <option>修改</option> 30 <option>查询</option> 31 <option>打印</option> 32 </select> 33 <div style="float:left"> 34 <input style="float:left;width:100%;" type="button" value=">" onclick="moveSelected(document.getElementById('select1'),document.getElementById('select2'))" /> 35 <input style="float:left;width:100%;" type="button" value="<" onclick="moveSelected(document.getElementById('select2'),document.getElementById('select1'))" /> 36 <input style="float:left;width:100%;" type="button" value=">>" onclick="moveAll(document.getElementById('select1'),document.getElementById('select2'))" /> 37 <input style="float:left;width:100%;" type="button" value="<<" onclick="moveAll(document.getElementById('select2'),document.getElementById('select1'))" /> 38 </div> 39 <select style="float:left;width:15%;height:100px;" id="select2" multiple="multiple"></select>40 </body>41 </html>

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

分类: [javascript](http://www.cnblogs.com/daomul/category/419507.html)

绿色通道： [好文要顶]() [关注我]() [收藏该文]()[与我联系](http://space.cnblogs.com/msg/send/daomul) [![]()]( "分享至新浪微博")
[daomul](http://home.cnblogs.com/u/daomul/)
[关注 - 1](http://home.cnblogs.com/u/daomul/followees)
[粉丝 - 43](http://home.cnblogs.com/u/daomul/followers)

[+加关注]()

4

0
(请您对文章做出评价)

[«](http://www.cnblogs.com/daomul/archive/2013/04/17/3021929.html) 上一篇：[ADO.NET- 基础总结及实例](http://www.cnblogs.com/daomul/archive/2013/04/17/3021929.html "发布于2013-04-17 23:54")
[»](http://www.cnblogs.com/daomul/archive/2013/04/26/3037737.html) 下一篇：[JQuery 学习总结及实例](http://www.cnblogs.com/daomul/archive/2013/04/26/3037737.html "发布于2013-04-26 00:29")
