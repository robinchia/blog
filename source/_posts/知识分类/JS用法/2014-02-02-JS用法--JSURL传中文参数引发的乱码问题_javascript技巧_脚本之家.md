---
layout: post
title: "JS URL传中文参数引发的乱码问题_javascript技巧_脚本之家"
categories: JS用法
tags: 
 - JS用法
--- 

# JS URL传中文参数引发的乱码问题_javascript技巧_脚本之家

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
页面导航： [首页](http://www.jb51.net/index.htm) → [网络编程](http://www.jb51.net/list/index_1.htm "网络编程") → [JavaScript](http://www.jb51.net/list/list_3_1.htm "JavaScript") → [javascript技巧](http://www.jb51.net/list/list_222_1.htm "javascript技巧") → 正文内容 url中文参数 乱码

# JS URL传中文参数引发的乱码问题

发布：dxy 字体：[[增加]() [减小]()] 类型：转载

今天的项目中碰到了一个乱码问题，从JS里传URL到服务器，URL中有中文参数，服务器里读出的中文参数来的全是“？”，查了网上JS编码相关资料得以解决。
解决方法如下：
1、在JS里对中文参数进行两次转码
复制代码 代码如下:

var login_name = document.getElementById("loginname").value;
login_name = encodeURI(login_name);
login_name = encodeURI(login_name);
2、在服务器端对参数进行解码
复制代码 代码如下:

String loginName = ParamUtil.getString(request, "login_name");
loginName = java.net.URLDecoder.decode(loginName,"UTF-8");
在使用url进行参数传递时，经常会传递一些中文名的参数或URL地址，在后台处理时会发生转换错误。在有些传递页面使用GB2312，而在接收页面使用UTF8，这样接收到的参数就可能会与原来发生不一致。使用服务器端的urlEncode函数编码的URL，与使用客户端javascript的encodeURI函数编码的URL，结果就不一样。
javaScript中的编码方法：
escape() 方法：
采用ISO Latin字符集对指定的字符串进行编码。所有的空格符、标点符号、特殊字符以及其他非ASCII字符都将被转化成%xx格式的字符编码（xx等于该字符在字符集表里面的编码的16进制数字）。比如，空格符对应的编码是%20。unescape方法与此相反。不会被此方法编码的字符： @ * / +
英文解释：MSDN JScript Reference: The escape method returns a string value (in Unicode format) that contains the contents of [the argument]. All spaces, punctuation, accented characters, and any other non-ASCII characters are replaced with %xx encoding, where xx is equivalent to the hexadecimal number representing the character. For example, a space is returned as "%20."
Edge Core Javascript Guide: The escape and unescape functions let you encode and decode strings. The escape function returns the hexadecimal encoding of an argument in the ISO Latin character set. The unescape function returns the ASCII string for the specified hexadecimal encoding value.
encodeURI() 方法：把URI字符串采用UTF-8编码格式转化成escape格式的字符串。不会被此方法编码的字符：! @ # $& * ( ) = : / ; ? + '
英文解释：MSDN JScript Reference: The encodeURI method returns an encoded URI. If you pass the result to decodeURI, the original string is returned. The encodeURI method does not encode the following characters: ":", "/", ";", and "?". Use encodeURIComponent to encode these characters. Edge Core Javascript Guide: Encodes a Uniform Resource Identifier (URI) by replacing each instance of certain characters by one, two, or three escape sequences representing the UTF-8 encoding of the character
encodeURIComponent() 方法：把URI字符串采用UTF-8编码格式转化成escape格式的字符串。与encodeURI()相比，这个方法将对更多的字符进行编码，比如 / 等字符。所以如果字符串里面包含了URI的几个部分的话，不能用这个方法来进行编码，否则 / 字符被编码之后URL将显示错误。不会被此方法编码的字符：! * ( )
英文解释：MSDN JScript Reference: The encodeURIComponent method returns an encoded URI. If you pass the result to decodeURIComponent, the original string is returned. Because the encodeURIComponent method encodes all characters, be careful if the string represents a path such as /folder1/folder2/default.html. The slash characters will be encoded and will not be valid if sent as a request to a web server. Use the encodeURI method if the string contains more than a single URI component. Mozilla Developer Core Javascript Guide： Encodes a Uniform Resource Identifier (URI) component by replacing each instance of certain characters by one, two, or three escape sequences representing the UTF-8 encoding of the character.
因此，对于中文字符串来说，如果不希望把字符串编码格式转化成UTF-8格式的（比如原页面和目标页面的charset是一致的时候），只需要使用escape。如果你的页面是GB2312或者其他的编码，而接受参数的页面是UTF-8编码的，就要采用encodeURI或者encodeURIComponent。
另外，encodeURI/encodeURIComponent是在javascript1.5之后引进的，escape则在javascript1.0版本就有。
英文注释：The escape() method does not encode the + character which is interpreted as a space on the server side as well as generated by forms with spaces in their fields. Due to this shortcoming, you should avoid use of escape() whenever possible. The best alternative is usually encodeURIComponent().Use of the encodeURI() method is a bit more specialized than escape() in that it encodes for URIs [REF] as opposed to the querystring, which is part of a URL. Use this method when you need to encode a string to be used for any resource that uses URIs and needs certain characters to remain un-encoded. Note that this method does not encode the ' character, as it is a valid character within URIs.Lastly, the encodeURIComponent() method should be used in most cases when encoding a single component of a URI. This method will encode certain chars that would normally be recognized as special chars for URIs so that many components may be included. Note that this method does not encode the ' character, as it is a valid character within URIs.

**Tags：**[url](http://img.jb51.net/tag/url/1.htm "搜索关于url的文章") [中文参数](http://img.jb51.net/tag/ÖÐÎÄ²ÎÊý/1.htm "搜索关于中文参数的文章") [乱码](http://img.jb51.net/tag/ÂÒÂë/1.htm "搜索关于乱码的文章")
[复制链接发给好友]( "复制本文链接发给你QQ/MSN上的好友")[收藏本文]()[打印本文]()[关闭本文]()[返回首页](http://www.jb51.net/index.htm)

上一篇：[FF IE兼容性的修改小结](http://www.jb51.net/article/19845.htm "FF IE兼容性的修改小结")

下一篇：[Javascript 中介者模式实例](http://www.jb51.net/article/21463.htm)
## 同类文章

* [Javascript 学习笔记 错误处理](http://www.jb51.net/article/19448.htm "Javascript 学习笔记 错误处理")
* [IE与firefox下Dhtml的一些区别小结](http://www.jb51.net/article/21210.htm "IE与firefox下Dhtml的一些区别小结")
* [js变量作用域及可访问性的探讨](http://www.jb51.net/article/4908.htm "js变量作用域及可访问性的探讨")
* [javascript 表格排序和表头浮动效果(扩展SortTab](http://www.jb51.net/article/17662.htm "javascript 表格排序和表头浮动效果(扩展SortTable)")
* [javascript 冒号 使用说明](http://www.jb51.net/article/18465.htm "javascript 冒号 使用说明")
* [效率高的Javscript字符串替换函数的benchmark](http://www.jb51.net/article/15353.htm "效率高的Javscript字符串替换函数的benchmark")
* [Javascript 获取字符串字节数的多种方法](http://www.jb51.net/article/18398.htm "Javascript 获取字符串字节数的多种方法")
* [简单的加密css地址防止别人下载你的CSS文件的方法](http://www.jb51.net/article/20450.htm "简单的加密css地址防止别人下载你的CSS文件的方法")
* [教你如何解密js/vbs/vbscript加密的编码异处理小](http://www.jb51.net/article/14972.htm "教你如何解密js/vbs/vbscript加密的编码异处理小结")
* [服务器安全设置的几个注册表设置](http://www.jb51.net/article/10638.htm "服务器安全设置的几个注册表设置")
* [js 深拷贝函数](http://www.jb51.net/article/16706.htm "js 深拷贝函数")
* [js实现右下角窗口弹出窗口效果](http://www.jb51.net/article/15676.htm "js实现右下角窗口弹出窗口效果")
* [JS的递增/递减运算符和带操作的赋值运算符的等价](http://www.jb51.net/article/13086.htm "JS的递增/递减运算符和带操作的赋值运算符的等价式")
* [如何用JS取得网址中的文件名](http://www.jb51.net/article/587.htm "如何用JS取得网址中的文件名")
* [javascript 支持页码格式的分页类](http://www.jb51.net/article/21326.htm "javascript 支持页码格式的分页类")
* [javascript模仿msgbox提示效果代码](http://www.jb51.net/article/14733.htm "javascript模仿msgbox提示效果代码")

## 文章评论

共有 ****位脚本之家网友发表了评论[我来说两句](http://img.jb51.net/comment.asp?id=19850)

### 最 近 更 新

* [javascript 点击整页变灰的效果（可做退出](http://www.jb51.net/article/13345.htm "javascript 点击整页变灰的效果（可做退出效果）。")
* [使两个iframe的高度与内容自适应,且相等](http://www.jb51.net/article/4870.htm "使两个iframe的高度与内容自适应,且相等")
* [javascript 自动转到命名锚记](http://www.jb51.net/article/17070.htm "javascript 自动转到命名锚记")
* [Javascript 定时器调用传递参数的方法](http://www.jb51.net/article/20880.htm "Javascript 定时器调用传递参数的方法")
* [无间断滚动marquee的详细用法解析](http://www.jb51.net/article/592.htm "无间断滚动marquee的详细用法解析")
* [Js之软键盘实现(js源码)](http://www.jb51.net/article/6605.htm "Js之软键盘实现(js源码)")
* [document.createRange实例](http://www.jb51.net/article/2835.htm "document.createRange实例")
* [javascript 连连看代码出炉](http://www.jb51.net/article/18800.htm "javascript 连连看代码出炉")
* [JavaScript容错例外处理](http://www.jb51.net/article/14718.htm "JavaScript容错例外处理")
* [js实现双击单元格变成文本输入框效果代码](http://www.jb51.net/article/14274.htm "js实现双击单元格变成文本输入框效果代码")
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

[关于本站](http://www.jb51.net/about.htm) - [广告合作](http://www.jb51.net/support.htm) - [联系我们](http://www.jb51.net/linkus.htm) - [免责声明](http://www.jb51.net/sm.htm) - [网站地图](http://www.jb51.net/sitemap.htm) - [意见反馈](http://www.jb51.net/do/plus/guestbook/index.php) - [返回顶部](http://www.jb51.net/article/19850.htm#top)

欢迎转载我们的原创作品，转载请注明出处。[苏ICP备06043639号](http://www.miibeian.gov.cn/)
Copyright © 2006-2010 www.jb51.net online services. All rights reserved. .
QQ群1:14624678 QQ群2:36345889或业务QQ:461478385
