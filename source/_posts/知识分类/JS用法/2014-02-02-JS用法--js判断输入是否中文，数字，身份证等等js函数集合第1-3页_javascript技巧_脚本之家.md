---
layout: post
title: "js判断输入是否中文，数字，身份证等等js函数集合第1-3页_javascript技巧_脚本之家"
categories: JS用法
tags: 
 - JS用法
--- 

# js判断输入是否中文，数字，身份证等等js函数集合第1-3页_javascript技巧_脚本之家

脚 本 之 家 www.jb51.net

* [源码下载](http://www.jb51.net/codes/index.html)
* [软件下载](http://www.jb51.net/softs/index.html)
* [服务器常用软件](http://www.jb51.net/s.htm)
* [最近更新](http://www.jb51.net/new/new_1.htm)
* [繁体中文]()

[![脚本之家]()](http://www.jb51.net/ "脚本之家")
* [首页](http://www.jb51.net/)
* [网页制作](http://www.jb51.net/web/index.html "网页制作")
* [脚本专栏](http://www.jb51.net/list/index_96.htm "脚本专栏")
* [网络编程](http://www.jb51.net/list/index_1.htm "网络编程")
* [数据库](http://www.jb51.net/list/index_104.htm "数据库")
* [脚本下载](http://www.jb51.net/jiaoben/index.html "脚本下载")
* [程序教学](http://www.jb51.net/cms/index.html "cms使用教程")
* [电子书籍](http://www.jb51.net/books/index.html "电子书籍")
* [平面设计](http://www.jb51.net/pingmian/index.html "平面设计")
* [媒体动画](http://www.jb51.net/media/index.html "媒体动画")
* [模板下载](http://www.jb51.net/moban/index.html "模板下载")
* [操作系统](http://www.jb51.net/os/index.html "操作系统")
* [网站运营](http://www.jb51.net/yunying/index.html "网站运营")

* [**主机租用**](http://www.cnkuai.cn/GetSpace.htm "主机租用,服务器租用,主机托管,服务器托管")
* [**域名查询**](http://www.cnkuai.cn/domain.htm "域名查询,域名注册,域名申请,域名")
* [基础知识](http://www.jb51.net/list/list_61_1.htm "基础知识")
* [javascript类库](http://www.jb51.net/list/list_23_1.htm "javascript类库")
* [表单特效](http://www.jb51.net/list/list_22_1.htm "表单特效")
* [广告代码](http://www.jb51.net/list/list_18_1.htm "广告代码")
* [网页特效](http://www.jb51.net/list/list_43_1.htm "网页特效")
* [黑客性质](http://www.jb51.net/list/list_26_1.htm "黑客性质")
* [javascript技巧](http://www.jb51.net/list/list_222_1.htm "javascript技巧")
页面导航： [首页](http://www.jb51.net/index.htm) → [网络编程](http://www.jb51.net/list/index_1.htm "网络编程") → [JavaScript](http://www.jb51.net/list/list_3_1.htm "JavaScript") → [javascript技巧](http://www.jb51.net/list/list_222_1.htm "javascript技巧") → 正文内容 js判断

# js判断输入是否中文，数字，身份证等等js函数集合第1/3页

发布：dxy 字体：[[增加]() [减小]()] 类型：转载

收集的比较多，建议大家查找搜寻，常用的js判断函数
*
判断指定的内容是否为空，若为空则弹出 警告框
*/
function isEmpty(theValue, strMsg){
if(theValue==""){
alert(strMsg+"不能为空!");
return true;
}
return false;
}
/*
中文判断函数，允许生僻字用英文“*”代替
返回true表示是符合条件，返回false表示不符合
*/
function isChinese(str){
var badChar ="ABCDEFGHIJKLMNOPQRSTUVWXYZ";
badChar += "abcdefghijklmnopqrstuvwxyz";
badChar += "0123456789";
badChar += " "+"　";//半角与全角空格
badChar += "`~!@#$%^&()-_=+]\\|:;\"\\'<,>?/";//不包含*或.的英文符号
if(""==str){
return false;
}
for(var i=0;i var c = str.charAt(i);//字符串str中的字符
if(badChar.indexOf(c) > -1){
return false;
}
}
return true;
}
/*
数字判断函数，返回true表示是全部数字，返回false表示不全部是数字
*/
function isNumber(str){
if(""==str){
return false;
}
var reg = /\D/;
return str.match(reg)==null;
}
/*
判断给定的字符串是否为指定长度的数字
是返回true，不是返回false
*/
function isNumber_Ex(str,len){
if(""==str){
return false;
}
if(str.length!=len){
return false;
}
if(!isNumber(str)){
return false;
}
return true;
}
/*
money判断函数，允许第一位为"-"来表示欠钱
返回true表示格式正确，返回false表示格式错误
*/
function isMoney(str){
if(""==str){
return false;
}
for(var i=0;i var c = str.charAt(i);
if(i==0){
if(c!="-"&&(c<"0"||c>"9")){
return false;
}else if(c=="-"&&str.length==1){
return false;
}
}else if(c < "0" || c > "9"){
return false;
}
}
return true;
}
/*
英文判断函数，返回true表示是全部英文，返回false表示不全部是英文
*/
function isLetter(str){
if(""==str){
return false;
}
for(var i=0;i var c = str.charAt(i);
if((c<"a"||c>"z")&&(c<"A"||c>"Z")){
return false;
}
}
return true;
}
/*
空格判断，当包含有空格返回false，当不包含一个空格返回true
""不能被判断
*/
function notInSpace(str){
if(""==str){
return false;
}
var badChar =" ";
badChar += "　";
for(var i=0;i var c = str.charAt(i);//字符串str中的字符
if(badChar.indexOf(c) > -1){
return false;
}
}
return true;
}
/*
发票号判断函数，返回true表示是发票号，返回false表示不符合规范
*/
function isFPH(str){
if(""==str){
return false;
}
for(var i=0;i var c = str.charAt(i);
if((c < "0" || c > "9") && (c!="-")&&(c!=",")){
return false;
}
}
return true;
}
当前1/3页 [**1**](http://www.jb51.net/article/15804.htm)[2](http://www.jb51.net/article/15804_2.htm)[3](http://www.jb51.net/article/15804_3.htm)[下一页](http://www.jb51.net/article/15804_2.htm)

**Tags：**[js](http://img.jb51.net/tag/js/1.htm "搜索关于js的文章") [中文](http://img.jb51.net/tag/ÖÐÎÄ/1.htm "搜索关于中文的文章") [数字](http://img.jb51.net/tag/Êý×Ö/1.htm "搜索关于数字的文章") [身份证](http://img.jb51.net/tag/Éí·ÝÖ¤/1.htm "搜索关于身份证的文章")
[复制链接发给好友]( "复制本文链接发给你QQ/MSN上的好友")[收藏本文]()[打印本文]()[关闭本文]()[返回首页](http://www.jb51.net/index.htm)

上一篇：[js判断是否有中文的脚本_js判断中文方法集合](http://www.jb51.net/article/15802.htm "js判断是否有中文的脚本_js判断中文方法集合")

下一篇：[Javascript 中介者模式实例](http://www.jb51.net/article/21463.htm)
## 同类文章

* [IE和firefox浏览器的event事件兼容性汇总](http://www.jb51.net/article/21278.htm "IE和firefox浏览器的event事件兼容性汇总")
* [javascript的indexOf忽略大小写的方法](http://www.jb51.net/article/15445.htm "javascript的indexOf忽略大小写的方法")
* [图片预载入](http://www.jb51.net/article/1348.htm "图片预载入")
* [JS是否可以跨文件同时控制多个iframe页面的应用技](http://www.jb51.net/article/13164.htm "JS是否可以跨文件同时控制多个iframe页面的应用技巧")
* [经常用的图片在容器中的水平垂直居中实例](http://www.jb51.net/article/10279.htm "经常用的图片在容器中的水平垂直居中实例")
* [cloneNode实现表格增加删除效果](http://www.jb51.net/article/4841.htm "cloneNode实现表格增加删除效果")
* [JavaScript 组件之旅（四）：测试 JavaScript 组](http://www.jb51.net/article/20635.htm "JavaScript 组件之旅（四）：测试 JavaScript 组件")
* [JavaScript使用prototype定义对象类型(转)[](http://www.jb51.net/article/5485.htm "JavaScript使用prototype定义对象类型(转)[")
* [javascript 常见的闭包问题的解决办法](http://www.jb51.net/article/20775.htm "javascript 常见的闭包问题的解决办法")
* [Javascript下的keyCode键码值表](http://www.jb51.net/article/9335.htm "Javascript下的keyCode键码值表")
* [可以拖动的div 实现代码](http://www.jb51.net/article/18737.htm "可以拖动的div 实现代码")
* [动态修改DOM 里面的 id 属性的弊端分析](http://www.jb51.net/article/15660.htm "动态修改DOM 里面的 id 属性的弊端分析")
* [Valerio 发布了 Mootools](http://www.jb51.net/article/1094.htm "Valerio 发布了 Mootools")
* [JS input 数字验证代码](http://www.jb51.net/article/19431.htm "JS input 数字验证代码")
* [JavaScript窗口功能指南之在窗口中书写内容](http://www.jb51.net/article/417.htm "JavaScript窗口功能指南之在窗口中书写内容")
* [Javascript结合css实现网页换肤功能](http://www.jb51.net/article/20701.htm "Javascript结合css实现网页换肤功能")

## 文章评论

共有 ****位脚本之家网友发表了评论[我来说两句](http://img.jb51.net/comment.asp?id=15804)

### 最 近 更 新

* [JavaScript中的new的使用方法与注意事项](http://www.jb51.net/article/9998.htm "JavaScript中的new的使用方法与注意事项")
* [javascript 获取图片颜色](http://www.jb51.net/article/17651.htm "javascript 获取图片颜色")
* [用JS控制表格的逐行变色的表单](http://www.jb51.net/article/13534.htm "用JS控制表格的逐行变色的表单")
* [javascript RadioButtonList获取选中值](http://www.jb51.net/article/17678.htm "javascript RadioButtonList获取选中值")
* [用javascript操作xml](http://www.jb51.net/article/4179.htm "用javascript操作xml")
* [随机显示经典句子或诗歌的javascript脚本](http://www.jb51.net/article/10710.htm "随机显示经典句子或诗歌的javascript脚本")
* [window.open被浏览器拦截后的自定义提示效](http://www.jb51.net/article/12788.htm "window.open被浏览器拦截后的自定义提示效果代码")
* [JavaScript talbe表中指定位置插入一行的](http://www.jb51.net/article/18502.htm "JavaScript talbe表中指定位置插入一行的实现代码 脚本之家修正版")
* [javascript引用对象的方法代码](http://www.jb51.net/article/11007.htm "javascript引用对象的方法代码")
* [使用自定义setTimeout和setInterval使之可](http://www.jb51.net/article/17859.htm "使用自定义setTimeout和setInterval使之可以传递参数和对象参数")
### 热 点 排 行

* [清除网页历史记录，屏蔽后退按钮](http://www.jb51.net/article/16884.htm "清除网页历史记录，屏蔽后退按钮!")
* [js刷新页面方法大全](http://www.jb51.net/article/14397.htm "js刷新页面方法大全")
* [Div+CSS+JS树型菜单，可刷新](http://www.jb51.net/article/425.htm "Div+CSS+JS树型菜单，可刷新")
* [eval(function(p,a,c,k,e,d)系列](http://www.jb51.net/article/9705.htm "eval(function(p,a,c,k,e,d)系列解密javascript程序")
* [JavaScript 节点操作 以及DOMDoc](http://www.jb51.net/article/13051.htm "JavaScript 节点操作 以及DOMDocument属性和方法")
* [JavaScript实现Sleep函数的代码](http://www.jb51.net/article/7508.htm "JavaScript实现Sleep函数的代码")
* [javascript小技巧 超强推荐](http://www.jb51.net/article/583.htm "javascript小技巧  超强推荐")
* [JavaScript修改css样式style](http://www.jb51.net/article/14178.htm "JavaScript修改css样式style")
* [在线游戏大家来找茬II](http://www.jb51.net/article/1335.htm "在线游戏大家来找茬II")
* [身份证号码前六位所代表的省,市,](http://www.jb51.net/article/9386.htm "身份证号码前六位所代表的省,市,区, 以及地区编码下载")

[关于本站](http://www.jb51.net/about.htm) - [广告合作](http://www.jb51.net/support.htm) - [联系我们](http://www.jb51.net/linkus.htm) - [免责声明](http://www.jb51.net/sm.htm) - [网站地图](http://www.jb51.net/sitemap.htm) - [意见反馈](http://www.jb51.net/do/plus/guestbook/index.php) - [返回顶部](http://www.jb51.net/article/15804.htm#top)

欢迎转载我们的原创作品，转载请注明出处。[苏ICP备06043639号](http://www.miibeian.gov.cn/)
Copyright © 2006-2010 www.jb51.net online services. All rights reserved. .
QQ群1:14624678 QQ群2:36345889或业务QQ:461478385
