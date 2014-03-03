---
layout: post
title: "css( 转) - CSS专栏 - JavaEye知识库"
categories: CSS
tags: 
 - CSS
--- 

# css( 转) - CSS专栏 - JavaEye知识库

[您还未登录 !](http://www.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://www.javaeye.com/login) [注册](http://www.javaeye.com/signup)

[![JavaEye3.0]( "JavaEye-最棒的软件开发交流社区")](http://www.javaeye.com/)

[![第四届 D2前端技术论坛 (12月19日·杭州)]()](http://www.d2forum.org/d2/4/)

[]()

[知识库](http://www.javaeye.com/wiki)  →  [专栏: CSS专栏](http://www.javaeye.com/wiki/CSS)  →  [css( 转)]()
# css( 转)

原创作者: [Ping](http://www.javaeye.com/topic/52493)   阅读:757次   评论:0条   更新时间:2007-02-06    

[**一.使用css缩写**
]()

引用内容：
使用缩写可以帮助减少你CSS文件的大小，更加容易阅读。css缩写的主要规则如下：
颜色
16进制的色彩值，如果每两位的值相同，可以缩写一半，例如：
#000000可以缩写为#000;#336699可以缩写为#369;
盒尺寸
通常有下面四种书写方法:
property:value1; 表示所有边都是一个值value1；
property:value1 value2; 表示top和bottom的值是value1,right和left的值是value2 
property:value1 value2 value3; 表示top的值是value1，right和left的值是value2，bottom的值是value3 
property:value1 value2 value3 value4; 四个值依次表示top,right,bottom,left 
方便的记忆方法是顺时针，上右下左。具体应用在margin和padding的例子如下：
margin:1em 0 2em 0.5em; 
边框(border)
边框的属性如下：
border-width:1px; 
border-style:solid; 
border-color:#000; 
可以缩写为一句：border:1px solid #000; 
语法是border:width style color; 
背景(Backgrounds)
背景的属性如下：
background-color:#f00; 
background-image:url(background.gif); 
background-repeat:no-repeat; 
background-attachment:fixed; 
background-position:0 0; 
可以缩写为一句：background:#f00 url(background.gif) no-repeat fixed 0 0; 
语法是background:color image repeat attachment position;
你可以省略其中一个或多个属性值，如果省略，该属性值将用浏览器默认值，默认值为：
color: transparent 
image: none 
repeat: repeat 
attachment: scroll
position: 0% 0%
字体(fonts)
字体的属性如下：
font-style:italic; 
font-variant:small-caps; 
font-weight:bold; 
font-size:1em; 
line-height:140%; 
font-family:”Lucida Grande”,sans-serif; 
可以缩写为一句：font:italic small-caps bold 1em/140% “Lucida Grande”,sans-serif;
注意，如果你缩写字体定义，至少要定义font-size和font-family两个值。
列表(lists)
取消默认的圆点和序号可以这样写list-style:none;,
list的属性如下:
list-style-type:square; 
list-style-position:inside; 
list-style-image:url(image.gif); 
可以缩写为一句：list-style:square inside url(image.gif);
**二.明确定义单位，除非值为0**
忘记定义尺寸的单位是CSS新手普遍的错误。在HTML中你可以只写width=”100″，但是在CSS中，你必须给一个准确的单位，比如：width: 100px width:100em。只有两个例外情况可以不定义单位：行高和0值。除此以外，其他值都必须紧跟单位，注意，不要在数值和单位之间加空格。
**三.区分大小写**
当在XHTML中使用CSS，CSS里定义的元素名称是区分大小写的。为了避免这种错误，我建议所有的定义名称都采用小写。
class和id的值在HTML和XHTML中也是区分大小写的，如果你一定要大小写混合写，请仔细确认你在CSS的定义和XHTML里的标签是一致的。
**四.取消class和id前的元素限定**
当你写给一个元素定义class或者id，你可以省略前面的元素限定，因为ID在一个页面里是唯一的，而clas s可以在页面中多次使用。你限定某
个元素毫无意义。例如：
div#content { /* declarations */ }
fieldset.details { /* declarations */ } 
可以写成
#content { /* declarations */ }
.details { /* declarations */ } 
这样可以节省一些字节。
**五.默认值**
通常padding的默认值为0，background-color的默认值是transparent。但是在不同的浏览器默认值可能不同。如果怕有冲突，可以在样式表一
开始就先定义所有元素的margin和padding值都为0，象这样：
* {
margin:0;
padding:0;
} 
**六.不需要重复定义可继承的值**
CSS中，子元素自动继承父元素的属性值，象颜色、字体等，已经在父元素中定义过的，在子元素中可以直接继承，不需要重复定义。但是要注意，浏览器可能用一些默认值覆盖你的定义。
**七.最近优先原则**
如果对同一个元素的定义有多种，以最接近(最小一级)的定义为最优先，例如有这么一段代码
Update: Lorem ipsum dolor set
在CSS文件中，你已经定义了元素p，又定义了一个class”update”
p {
margin:1em 0;
font-size:1em;
color:#333;
}
.update {
font-weight:bold;
color:#600;
} 
这两个定义中，class=”update”将被使用，因为class比p更近。你可以查阅W3C的Calculating a selector’s specificity
**八.多重class定义**
一个标签可以同时定义多个class。例如：我们先定义两个样式，第一个样式背景为#666；第二个样式有10 px的边框。
.one{width:200px;background:#666;}
.two{border:10px solid #F00;} 
在页面代码中，我们可以这样调用，这样最终的显示效果是这个div既有#666的背景，也有10px的边框。是的，这样做是可以的，你可以尝试一下。
**九.使用子选择器(descendant selectors)**
CSS初学者不知道使用子选择器是影响他们效率的原因之一。子选择器可以帮助你节约大量的class定义。我们来看下面这段代码：
Item 1>
Item 1 
Item 1 
这段代码的CSS定义是：
div#subnav ul { /* Some styling */ }
div#subnav ul li.subnavitem { /* Some styling */ }
div#subnav ul li.subnavitem a.subnavitem { /* Some styling */ }
div#subnav ul li.subnavitemselected { /* Some styling */ }
div#subnav ul li.subnavitemselected a.subnavitemselected { /* Some styling */ } 
你可以用下面的方法替代上面的代码
Item 1 
Item 1 
Item 1 
样式定义是：
#subnav { /* Some styling */ }
#subnav li { /* Some styling */ }
#subnav a { /* Some styling */ }
#subnav .sel { /* Some styling */ }
#subnav .sel a { /* Some styling */ } 
用子选择器可以使你的代码和CSS更加简洁、更加容易阅读。
**十.不需要给背景图片路径加引号**
为了节省字节，我建议不要给背景图片路径加引号，因为引号不是必须的。例如：
background:url("images/***.gif") #333; 
可以写为
background:url(images/***.gif) #333; 
如果你加了引号，反而会引起一些浏览器的错误。
**十一.组选择器(Group selectors)**
当一些元素类型、class或者id都有共同的一些属性，你就可以使用组选择器来避免多次的重复定义。这可以节省不少字节。 
例如：定义所有标题的字体、颜色和margin，你可以这样写：
h1,h2,h3,h4,h5,h6 {
font-family:"Lucida Grande,Lucida,Arial,Helvetica,sans-serif";
color:#333;
margin:1em 0;
} 
如果在使用时，有个别元素需要定义独立样式，你可以再加上新的定义，可以覆盖老的定义，例如：
h1 { font-size:2em; }
h2 { font-size:1.6em; } 
**十二.用正确的顺序指定链接的样式**
当你用CSS来定义链接的多个状态样式时，要注意它们书写的顺序，正确的顺序是：:link :visited :hover :active。抽取第一个字母是”
LVHA”，你可以记忆成”LoVe HAte”(喜欢讨厌)。为什么这么定义，可以参考Eric Meyer的《Link Specificity》。
如果你的用户需要用键盘来控制，需要知道当前链接的焦点，你还可以定义:focus属性。:focus属性的效果也取决与你书写的位置，如果你希
望聚焦元素显示:hover效果，你就把:focus写在:hover前面；如果你希望聚焦效果替代:hover效果，你就把:focus放在:hover后面。
**十三.清除浮动**
一个非常常见的CSS问题，定位使用浮动的时候，下面的层被浮动的层所覆盖，或者层里嵌套的子层超出了外层的范围。
通常的解决办法是在浮动层后面添加一个额外元素，例如一个div或者一个br，并且定义它的样式为clear: both。这个办法有一点牵强，幸运
的是还有一个好办法可以解决，参看这篇文章《How To Clear Floats Without Structural Markup》
上面2种方法可以很好解决浮动超出的问题，但是如果当你真的需要对层或者层里的对象进行clear的时候怎么办？一种简单的方法就是用
overflow属性，这个方法最初的发表在《Simple Clearing of Floats》，又在《Clearance》和《Super simple clearing floats》中被广泛
讨论。
上面那一种clear方法更适合你，要看具体的情况，这里不再展开论述。另外关于float的应用，一些优秀的文章已经说得很清楚，推荐你阅读
：>、>和>
**十四.横向居中(centering)**
这是一个简单的技巧，但是值得再说一遍，因为我看见太多的新手问题都是问这个：CSS如何横向居中？你需要定义元素的宽，并且定义横向的margin，如果你的布局包含在一个层(容器)中，就象这样：
你可以这样定义使它横向居中：
#wrap {
width:760px; /* 修改为你的层的宽度 */
margin:0 auto;
} 
但是IE5/Win不能正确显示这个定义，我们采用一个非常有用的技巧来解决：用text-align属性。就象这样：
body {
text-align:center;
}
#wrap {
width:760px; /* 修改为你的层的宽度 */
margin:0 auto;
text-align:left;
} 
第一个body的text-align:center; 规则定义IE5/Win中body的所有元素居中(其他浏览器只是将文字居中) ，第二个text-align:left;是将
#warp中的文字居左。
**十五.导入(Import)和隐藏CSS**
因为老版本浏览器不支持CSS，一个通常的做法是使用@import技巧来把CSS隐藏起来。例如：
@import url("main.css"); 
然而，这个方法对IE4不起作用，这让我很是头疼了一阵子。后来我用这样的写法：
@import "main.css"; 
这样就可以在IE4中也隐藏CSS了，呵呵，还节省了5个字节呢
**十六.针对IE的优化**
有些时候，你需要对IE浏览器的bug定义一些特别的规则，这里有太多的CSS技巧(hacks)，我只使用其中的两种方法，不管[微软](http://www.microsoft.com/china/)在发布的IE7 
beta版里是否更好的支持CSS，这两种方法都是最安全的。
1.注释的方法
(a)在IE中隐藏一个CSS定义，你可以使用子选择器(child selector):
body p {
/* 定义内容 */
}
(b)下面这个写法只有IE浏览器可以理解(对其他浏览器都隐藏)
* html p {
/* declarations */
}
(c)还有些时候，你希望IE/Win有效而IE/Mac隐藏，你可以使用”反斜线”技巧：
/* \*/
* html p {
declarations
}
/* */
2.条件注释(conditional comments)的方法
另外一种方法，我认为比CSS　Hacks更加经得起考验就是采用[微软](http://www.microsoft.com/china/)的私有属性条件注释(conditional comments)。用这个方法你可以给IE单独
定义一些样式，而不影响主样式表的定义。
**十七.调试技巧：层有多大？**
当调试CSS发生错误，你就要象排版工人，逐行分析CSS代码。我通常在出问题的层上定义一个背景颜色，这样就能很明显看到层占据多大空间
。有些人建议用border，一般情况也是可以的，但问题是，有时候border 会增加元素的尺寸，border-top和boeder-bottom会破坏纵向margin
的值，所以使用background更加安全些。 
另外一个经常出问题的属性是outline。outline看起来象border，但不会影响元素的尺寸或者位置。只有少数浏览器支持outline属性，我所知
道的只有Safari、OmniWeb、和Opera。
**十八.CSS代码书写样式**
在写CSS代码的时候，对于缩进、断行、空格，每个人有每个人的书写习惯。在经过不断实践后，我决定采用下面这样的书写样式：
selector1,
selector2 {
property:value;
} 
当使用联合定义时，我通常将每个选择器单独写一行，这样方便在CSS文件中找到它们。在最后一个选择器和大括号{之间加一个空格，每个定
义也单独写一行，分号直接在属性值后，不要加空格。
我习惯在每个属性值后面都加分号，虽然规则上允许最后一个属性值后面可以不写分号，但是如果你要加新样式时容易忘记补上分号而产生错
误，所以还是都加比较好。
最后，关闭的大括号}单独写一行。
空格和换行有助与阅读。
一个CSS的在线参考手册,可以在线编辑然后看效果.
[http://www.webucator.com/resources/css/reference.html](http://www.webucator.com/resources/css/reference.html)
CSS.0中文在线手册：[http://goaler.tengyi.cn/css2.0/](http://goaler.tengyi.cn/css2.0/)
[使用HTML＋CSS编写一个灵活的Tab页](http://www.javaeye.com/wiki/CSS/821-%E4%BD%BF%E7%94%A8HTML%EF%BC%8BCSS%E7%BC%96%E5%86%99%E4%B8%80%E4%B8%AA%E7%81%B5%E6%B4%BB%E7%9A%84Tab%E9%A1%B5 "使用HTML＋CSS编写一个灵活的Tab页")

评论 共 0 条 [发表评论](http://www.javaeye.com/wiki/CSS/820-css(%20%E8%BD%AC)#) []()
### 发表评论

### 表情图标

![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()

字体颜色: 标准深红红色橙色棕色黄色绿色橄榄青色蓝色深蓝靛蓝紫色灰色白色黑色 字体大小: 标准1 (xx-small)2 (x-small)3 (small)4 (medium)5 (large)6 (x-large)7 (xx-large) 对齐: 标准居左居中居右

提示：选择您需要装饰的文字, 按上列按钮即可添加上相应的标签
您还没有登录，请[登录](http://www.javaeye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

### 文章信息

[![CSS专栏专栏]( "CSS专栏: CSS专栏，07年将是web标准大面积推广的年份 希望和大家一起交流基于web标准的web重构之道。")](http://www.javaeye.com/wiki/CSS)  [专栏: CSS专栏](http://www.javaeye.com/wiki/CSS)

* 由[kj23](http://kj23.javaeye.com/)在2007-02-06创建
* 由[kj23](http://kj23.javaeye.com/)在2007-02-06更新

### 相关新闻

* [E3.Table发布, 集成了ext grid](http://www.javaeye.com/news/2893 "E3.Table发布, 集成了ext grid")
* [【翻译】Java EE 6体系结构的变革](http://www.javaeye.com/news/5547-translation-java-ee-6-architecture-changes "【翻译】Java EE 6体系结构的变革")
* [E3平台之 E3.ID 1.0 发布](http://www.javaeye.com/news/2128 "E3平台之 E3.ID 1.0 发布")
### 相关讨论

* [构建一个通用的Action 类,请大家谈谈自己的看法和经验](http://www.javaeye.com/topic/101346)
* [在 <#list>中实现动态换行](http://www.javaeye.com/topic/322152)
* [投简历无回音，请大家帮我看看简历，谢谢](http://www.javaeye.com/topic/355773)
* [***** JavaScript 面向对象 之 对象全解 [半原创] *****](http://www.javaeye.com/topic/345641)
* [如何构建一个插件框架](http://www.javaeye.com/topic/366158)

### 相关博客

* [Richfaces 控件使用](http://gaolixu.javaeye.com/blog/365192)
* [ActionScript 3中的类反射](http://lixinye0123.javaeye.com/blog/318177)
* [jCharts用户指南翻译第六章 轴图表](http://yinxiangbing.javaeye.com/blog/336393)
* [开发RSS聚合的开源框架](http://benbenming.javaeye.com/blog/347620)
* [《Extjs实战》节选：ColumnLayout的使用方法](http://chenxueyong.javaeye.com/blog/338610)
* [首页](http://www.javaeye.com/)
* [新闻](http://www.javaeye.com/news)
* [论坛](http://www.javaeye.com/forums)
* [问答](http://www.javaeye.com/ask)
* [知识库](http://www.javaeye.com/wiki)
* [博客](http://www.javaeye.com/blogs)
* [圈子](http://www.javaeye.com/groups)
* [猎头](http://www.javaeye.com/hunters)
* [招聘](http://www.javaeye.com/job)
* [服务](http://www.javaeye.com/index/service)
* [搜索](http://www.javaeye.com/google_search)

* [Java](http://java.javaeye.com/)
* [Web](http://webapp.javaeye.com/)
* [Ruby](http://ruby.javaeye.com/)
* [Python](http://python.javaeye.com/)
* [敏捷](http://agile.javaeye.com/)
* [MySQL](http://mysql.javaeye.com/)
* [普元](http://primeton.javaeye.com/)
* [Dorado](http://dorado.javaeye.com/)
* [金蝶中间件](http://kingdee.javaeye.com/)
* [图书](http://book.javaeye.com/)
* [广告服务](http://www.javaeye.com/index/service)
* [JavaEye黑板报](http://webmaster.javaeye.com/)
* [关于我们](http://www.javaeye.com/index/aboutus)
* [联系我们](http://www.javaeye.com/index/contactus)
* [友情链接](http://www.javaeye.com/index/friend_links)

© 2003-2009 JavaEye.com. All rights reserved. [ 沪ICP备05023328号 ] ![]()
