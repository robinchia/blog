---
layout: post
title: "firefox ie 区别相关 - lu_huanling的专栏 - CSDN博客"
categories: JS用法
tags: 
 - JS用法
--- 

# firefox ie 区别相关 - lu_huanling的专栏 - CSDN博客

# [lu_huanling的专栏](http://blog.csdn.net/lu_huanling)

##

* [登录](http://passport.csdn.net/UserLogin.aspx?from=http%3A%2F%2Fblog.csdn.net%2Flu_huanling%2Farchive%2F2008%2F12%2F05%2F3453723.aspx)
* [注册](http://passport.csdn.net/CSDNUserRegister.aspx)
* [欢迎](http://hi.csdn.net/)
* [退出](http://writeblog.csdn.net/Signout.aspx)
* [我的博客](http://blog.csdn.net/)
* [配置](http://writeblog.csdn.net/configure.aspx)
* [写文章](http://writeblog.csdn.net/PostEdit.aspx)
* [文章管理](http://writeblog.csdn.net/PostList.aspx)
* [博客首页](http://blog.csdn.net/)

*
* 全站 当前博客
*

* [空间](http://hi.csdn.net/lu_huanling)
* [博客](http://blog.csdn.net/lu_huanling)
* [好友](http://hi.csdn.net/!s/friend/list/lu_huanling)
* [相册](http://hi.csdn.net/!s/album/list/lu_huanling)
* [留言](http://hi.csdn.net/!s/wall/to/lu_huanling)

用户操作 [[留言]](http://hi.csdn.net/!s/wall/to/lu_huanling)  [[发消息]](http://hi.csdn.net/!s/msg/to/lu_huanling)  [[加为好友]](http://hi.csdn.net/!s/friend/add/lu_huanling)  路焕玲ID：[lu_huanling](http://hi.csdn.net/lu_huanling)[![路焕玲]()](http://hi.csdn.net/lu_huanling)共*691*次访问，排名*2万外*，[好友](http://hi.csdn.net/!s/friend/list/lu_huanling)*5*人，[关注者](http://hi.csdn.net/!s/follow/list/lu_huanling)*7*人。路焕玲的文章原创 6 篇翻译 0 篇转载 1 篇评论 0 篇  订阅我的博客 [![XML聚合]()](http://feeds.feedsky.com/csdn.net/lu_huanling)   [![FeedSky]()](http://feeds.feedsky.com/csdn.net/lu_huanling) [![订阅到鲜果]()](http://www.xianguo.com/subscribe.php?url=http://feeds.feedsky.com/csdn.net/lu_huanling) [![订阅到Google]()](http://fusion.google.com/add?feedurl=http://feeds.feedsky.com/csdn.net/lu_huanling) [![订阅到抓虾]()](http://www.zhuaxia.com/add_channel.php?url=http://feeds.feedsky.com/csdn.net/lu_huanling)  [[编辑]](http://writeblog.csdn.net/configure.aspx)lu_huanling的公告 [[编辑]](http://writeblog.csdn.net/EditCategories.aspx?catID=1)文章分类 * [![(RSS)]()](http://blog.csdn.net/lu_huanling/category/466861.aspx/rss)[javaScript 相關](http://blog.csdn.net/lu_huanling/category/466861.aspx "javaScript ")
* [![(RSS)]()](http://blog.csdn.net/lu_huanling/category/456949.aspx/rss)[学习,工作](http://blog.csdn.net/lu_huanling/category/456949.aspx "You are never too old to study .") 存档 * [2008年12月(5)](http://blog.csdn.net/lu_huanling/archive/2008/12.aspx)
* [2008年09月(4)](http://blog.csdn.net/lu_huanling/archive/2008/09.aspx)
# ![原创]()  firefox ie 区别相关 [收藏]( "收藏到我的网摘中，并分享给我的朋友")

 

常见firefox不支持的JavaScript问题
2008-11-25 12:35 <a href="#" onclick="ChildNode(this);">aaa</a>要改为
<a href="#" onclick="ChildNode(event);">aaa</a>
无法取得this对象，要用以下方法来取得。
function ChildNode(e)
{
var evt = e ? e : (window.event ? window.event : null); //此方法为了在firefox中的兼容
var node = evt.srcElement ? evt.srcElement : evt.target; //evt.target在火狐上才能识别用的。
selectNode = node.getAttribute("nodeId").toString();
}
nodeId属性不支持，要node.getAttribute("nodeId");
还有var+=elements[i].innerText在firefox中无识别，用elements[i].innerHTML来支持即可。
------------------------------------------------------------------------------------------------
//这是一个访问下拉框的方法，注意ele.option()；中的圆括号firefox不支持，只能用[];才行。
var ele = document.getElementById('bizName');
idv = ele.option[ele.selectedIndex].title;
---------------------------------------------------------------------------
//在火狐中的地址栏输入：about:config，会出现火狐的参数配置设置，
---------------------------------------------------------------------------------
document.all在火狐中无法被识别，用document.getElementById,document.getElementByName等来替换即可。
----------------------------------------------------------------------------------------
//文件浏览的文本内容清理方法；unselectalbe：用于设置只读属性。on/off：两个值。
<input type="file" name="pic" id="pic" onchange="checkpic(this);" UNSELECTABLE="on"/>
function checkpic(here)
{
var reg_pic=/\w+(\.gif|\.jpg){1}/;
if(!reg_pic.test(here.value))
{
alert("");
here.outerHTML += "";//用于清除浏览框中的内容,here.value="";是无法执行的。IE支持这个方法
here.value = ""; //IE不支持这个属性，firefor却支持。
//在赋值时要注意outerHTML+=，value用=。
return;
}
}
//用来清除file中的内容；
<input type="file" id="file1"/><input type="button" onclick="addfile();"/>
function addfile(){
document.all('file1').select();
document.selection.clear()
}
----------------------------------------------------
//用来判断是IE或者FireFox
//用来判断浏览器的类型。
function IsBrowser()
{
var isBrowser ;
if(window.ActiveXObject){
isBrowser = "IE";
}else if(window.XMLHttpRequest){
isBrowser = "FireFox";
}
return isBrowser;
}
//在firefox中firstChild方法无效，用childNodes[]来代替。
var tableobj = document.getElementById('products');
var rvobj = document.getElementById('sto');
var delall = document.getElementById('delall');
if(IsIE == "IE")
{
tableobj.firstChild.removeChild(rvobj);
tableobj.firstChild.removeChild(delall);
}
if(IsIE == "FireFox")
{
tableobj.childNodes[1].removeChild(rvobj);
tableobj.childNodes[1].removeChild(delall);
}
出现这firstChild无法读取问题
<table>
<tbody>
<tr>
<td></td>
</tr>
</tbody>
</table>
这样的话firefox中无tableobj.firstChild就读取为空。
要这样<table><tbody><tr><td></td></tr></tbody></table>
在firefox中tableobj.firstChild就可以读取出<tbody>来，
所以在firefox中空白的也算一个节点。（要特别注意）
----------------------------IE不兼容问题----------------------------------------------
在程序中需要动态的创建一个复选框并在页面上显示，但是用document.getElementsByName()取的时候却取不到，
经测试，在firefox和opera中是完全能够取到的，看来又是ie的问题了
又试着创建了一个div，还是取不到，看来不光是表单元素有这个问题
解决方式：用document.getElementsByTagName
---------------------------------------------------------------------------
//会有打印的效果document.execCommand('print'),window.print();也有同样的效果。window.print()会在FireFox中兼容，而
document.exeCommand('print');会在FireFox有不兼容问题。
----------------------------------------------------------------------------------------------
//这种写法在firefox中不支持会有错误出现。
function document.onkeydown
{
var er = event.srcElement;
if(event.keyCode == 13)
document.getElementId('subform').click();
}
//只能这样写
document.onkeydown = function()
{
var er = event.srcElement;
if(event.keyCode == 13)
document.getElementId('subform').click();
}
--------------------------------------------------------------------------------------------------------
//eval的使用。IE中是可以用来取对象的一种方法，FireFox不支持这个。
<input type="text" id="submitText"/>
function subEval()
{
//IE中可以用来取得对象
var whileEl = eval('submitText');
alert(whileEl.type);
}
-----------------------------------------------------------------------------------------------------
//all在IE中支持，火狐不支持的，用elements可以两个都支持。
function clearForm(input){
var form = input.form;
if(form != null && form != undefined){
//var elements = form.all; IE支持。读取表单下的元素。
var elements = form.elements;
for(var i=0;i<elements.length;i++){
with(elements[i]){
if(elements[i].type == undefined || name == input.name)
continue;
if(type == 'text'){
value = '';
}else if(type == 'radio'){
checked = false;
}else if(type == 'select-one'){
selectedIndex = 0;
}
}
}
}
this.checked = true;
}
-----------------------------------------------------------------------------------------------------
//火狐上的用调试的小问题。alert();的使用
alert();当里面没有参数时会在火狐中无法运行，IE可以。
alert('');有参数火狐才会执行，在火狐调试时要特别注意。
-----------------------------------------------------------------------------------------------------
1)event
event.srcElement从字面上可以看出来有以下关键字：事件,源 他的意思就是：当前事件的源，
我们可以调用他的各种属性 就像:document.getElementById("")这样的功能，
经常有人问 firefox 下的 event.srcElement 怎么用，在此详细说明：
IE下,event对象有srcElement属性,但是没有target属性;Firefox下,event对象有target属性,但是没有srcElement属性.但他们的作用是相当的，即：
firefox 下的 event.target = IE 下的 event.srcElement
解决方法:使用obj(obj = event.srcElement ? event.srcElement : event.target;)来代替IE下的event.srcElement或者Firefox下的event.target.
http://www.firefox.hk
IE 中可以直接使用 event 对象，而 FF 中则不可以，解决方法之一如下：
var theEvent = window.event || arguments.callee.caller.arguments[0];
第二种是将 event 作为参数来传递:
function xxx(e){var theEvent = window.event || e;}
srcElement 和 target
在 IE 中 srcElement 表示产生事件的源，比如是哪个按钮触发的 onclick 事件，FF 中则是 target。
var theEvent = window.event || arguments.callee.caller.arguments[0];
var srcElement = theEvent.srcElement;
if (!srcElement)
{
srcElement = theEvent.target;
}
例子：
document.onclick = function(e){
var theEvent = window.event || e;
var srcElement = theEvent.srcElement;
if (!srcElement) {
srcElement = theEvent.target;
}
}
function clickAction(){
var theEvent = window.event || arguments.callee.caller.arguments[0];
var srcElement = theEvent.srcElement;
if (!srcElement) {
srcElement = theEvent.target;
}
// do something;
}
function clickAction(e){
var theEvent = window.event || e;
var srcElement = theEvent.srcElement;
if (!srcElement) {
srcElement = theEvent.target;
}
// do something;
}
event.keyCode 和event.which
FF不支持window.event.keyCode，代替着是event.which
列子：
//在网页上面屏蔽tab键的代码
document.onkeydown = function (e){
var theEvent = window.event || e;
var code = theEvent.keyCode || theEvent.which;
if(code == 9){
return false;
}
}
http://hi.baidu.com/myweb2/blog/item/041fb9943e644e1cd21b70ad.html
2)document.all
document.all是ie在dom标准确立之前的一个得到元素的一个集合，根据id和name，的一个元素大集合，后来DOM标准确定了， getElementById逐渐慢慢取代了all对象集的地位，但是firefox为了兼容一些为ie写的使用document.all的脚本，不得已，加入了document.all支持，但是也不支持if(document.all)判断，并且在有正确xhtml的doctype下会屏蔽使用 document.all
3)event
window.event //IE
e //FF
e = window.event || e
3)判断页面加载完成
IE: document.onreadystatechange=function(){document.readyState=="complete"}
FF: document.addEventListener("DOMContentLoaded",handle,false)
当某一事件被触发时需要执行某个函数，在IE下可用attachEvent，在FF下则要用addEventListener。
attachEvent()有两个参数，第一个是事件名称，第二个是需执行的函数；
addEventListener()有三个参数，第一个是事件名称，但与IE事件不同的是，事件不带"on",比如"onsubmit"在这里应为"submit"，第二个是需执行的函数，第三个参数为布尔值；
4)设置容器位置 left、top
IE：可以不用加单位px
FF：一定要加单位 px
-------------------------------------------------------------------------------------------
//一种用来输入整数的方法。
IsInt:<input type="text" onkeyup="isInt(event);">
//是否整型
function isInt(e)
{
//keyCode:IE支持，which:FF支持。
var theEvent = window.event || e;
var code = theEvent.keyCode || theEvent.which;
if(code<48 || code>57)
{
//alert(code);//srcElement:IE支持，target:FF支持
var val = e.srcElement ? e.srcElement : e.target;
val.value = val.value.substring(0,val.value.length-1);
}
}
---------------------------------------------------------------------------------------------
// "||"：也可以用来赋值，在FF中没有window.event，要对象赋对象。isInt(event);
function isInt(e)
{
var oEvent = e || window.event; //用来判断是IE或者FF，并赋值给对象。
var oTarget = oEvent.target || oEvent.srcElement; //用来取IE或者FF的对象。[http://hi.baidu.com/yuanxiaozi/blog/item/a3a6d235693dde1691ef39c8.html](http://hi.baidu.com/yuanxiaozi/blog/item/a3a6d235693dde1691ef39c8.html)

发表于 @ 2008年12月05日　16:08:00 | [评论( 0  )](http://blog.csdn.net/lu_huanling/archive/2008/12/05/3453723.aspx#FeedBack "评论")| [编辑](http://writeblog.csdn.net/PostEdit.aspx?entryId=3453723 "编辑")| [举报](mailto:webmaster@csdn.net?subject=Article%20Report!!!&body=Author:lu_huanling%0D%0AURL:http://blog.csdn.net/ArticleContent.aspx?UserName=lu_huanling&Entryid=3453723)| [收藏]( "收藏到我的网摘中，并分享给我的朋友")

### [旧一篇:firefox ie 区别](http://blog.csdn.net/lu_huanling/archive/2008/12/05/3453482.aspx) | [新一篇:有关 arguments](http://blog.csdn.net/lu_huanling/archive/2008/12/12/3502026.aspx)

相关文章[](http://blog.csdn.net/lu_huanling/archive/2008/12/05/3453723.aspx#) []()

给lu_huanling的留言只有注册用户才能发表评论！[登录](http://passport.csdn.net/member/UserLogin.aspx?from=http://blog.csdn.net/lu_huanling/archive/2008/12/05/3453723.aspx)[注册](http://passport.csdn.net/CSDNUserRegister.aspx)*![]()*

* 姓   名：
* 校验码：
![]()

Copyright © lu_huanling Powered by CSDN Blog
![]() ![]()

[![]()]()

[“CSDN作协”招募作家！](http://blog.csdn.net/!subject/writer_zm.html)精彩推荐 *[博客专家](http://blog.csdn.net/MoreExpert.html) [作家协会](http://zuoxie.blog.csdn.net/)*

* [对技术团队沟通产生冲突之我见](http://blog.csdn.net/changemyself/archive/2009/12/10/4980121.aspx)
* [透明度设置](http://blog.csdn.net/cheng5128/archive/2009/12/10/4982893.aspx)
* [郁郁乡思](http://blog.csdn.net/downmoon/archive/2009/12/08/4967639.aspx)
* [3DXI重写过的3Ds Max 导出插件代码放出](http://blog.csdn.net/Nhsoft/archive/2009/12/10/4982661.aspx)
![]()
