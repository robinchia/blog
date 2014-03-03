---
layout: post
title: "DIV align=名称 - DIV style=名称， DIV id=名称 有什么区别？怎么用？_"
categories: DIV
tags: 
 - DIV
--- 

# DIV align=名称 , DIV style=名称， DIV id=名称 有什么区别？怎么用？_百度知道

[百度首页](http://www.baidu.com/) | [百度知道](http://zhidao.baidu.com/) | [登录](http://passport.baidu.com/?login&tpl=ik)

[![]()](http://zhidao.baidu.com/question/29818029.html#) ![关闭]() [](http://zhidao.baidu.com/question/29818029.html#)
[![百度知道]()](http://zhidao.baidu.com/) [新闻](http://news.baidu.com/ns?cl=2&rn=20&tn=news&word=div%20style&t=1)   [网页](http://www.baidu.com/s?cl=3&wd=div%20style)   [贴吧](http://tieba.baidu.com/f?kw=div%20style&t=4)   知道   [MP3](http://mp3.baidu.com/m?tn=baidump3&ct=134217728&lm=-1&word=div%20style&t=2)   [图片](http://image.baidu.com/i?tn=baiduimage&ct=201326592&lm=-1&cl=2&word=div%20style&t=3)   [视频](http://video.baidu.com/v?ct=301989888&rn=20&pn=0&db=0&s=22&word=div%20style)   [百科](http://baike.baidu.com/w?ct=17&lm=0&tn=baiduWikiSearch&pn=0&rn=10&word=div%20style)

   [帮助](http://www.baidu.com/search/zhidao_help.html) [设置]()

[](http://ma.baidu.com/ma/rcv/click.php?t=uv-b5HDhTv-b5HcLP10sFMIGujYkFhVGujYkFhqsULnqniuhUWdGpv4EIzudThsqpZwYTaR1fiRzwBRzwhPCTh-1IAd9Tz4Bmy-bIi4WUvY-nbmhTv3qP1DhT1Y1n101P1TzmhDsryPhn1PWFMFsULnqniubIjd8iAPvUhCvIh3kUWPfn0)

[百度知道](http://zhidao.baidu.com/) > [电脑/网络](http://zhidao.baidu.com/browse/74?lm=2) > [互联网](http://zhidao.baidu.com/browse/88?lm=2)

[添加到搜藏](http://cang.baidu.com/do/add) 已解决

<DIV align=名称 ,<DIV style=名称，<DIV id=名称 有什么区别？怎么用？

![]() 悬赏分：20 -  解决时间：2007-7-13 09:55
<DIV id= <DIV class= 上边的我知道怎么用什么意思 下边的就不知道了 <DIV align= <DIV style=

提问者： [xyq0313](http://passport.baidu.com/?business&aid=6&un=xyq0313#2) - [一级](http://www.baidu.com/search/zhidao_help.html#如何选择头衔)
最佳答案

1.<DIV align=名称 作用:水平对齐方式 名称:center,left,right,justify 分别表示 居中,左对齐,右对齐,两端对齐 2.<DIV style=CSS语句 作用:设置样式 CSS语句,比如: <DIV style="border:1px solid #F4BF20;padding:5px 0px 5px 0px ;margin-bottom:12px;"></div> 3.<DIV id=名称 作用:赋予ID值 名称:这名称可以自己定,比如myDiv,然后通过document.getElementById("myDiv")可以获得这个DIV层对象,从而对它进行动态操作. 另外,如果这个id名称在CSS中被定义的话,这个DIV层将会套用该CSS样式,比如CSS中有这样的定义: <style type="css/text"> #myDiv {float:left;margin-left:10px;background:#FFFFFF;} </style> 4.<DIV class=名称 作用:套用CSS样式 这里的名称一般在CSS中有定义,否则没有意义. 比如<div class=div1>则在CSS中有div1所对应的样式: <style type="css/text"> .div1 {margin:13px 0 7px 0;height:1px;} /* 注意,这里是".div1",不是"#div1","#"表示ID号,"."表示class类 */ </style> --------------------- 3,4的区别是: <DIV id= <DIV class= 通过id可以较好地获取DIV层,以便对它进行操作,可以在定议的ID设置CSS 通过CSS可以套用CSS样式,CSS样式不仅在网页中应用,在其它文件中也有用到,比如XML方面. 在CSS中定义为： <style type="css/text"> #idName {} .className {} </style> ---------------- 区别 <DIV align= <DIV style= <div align="center"> 相当于<div style="text-align: center;"> 也就是说align="center"只是HTML里的标记, 通过CSS的样式（style）完全可以实现,而且CSS样式的功能远不止"将区块对齐"。

 **34**

回答者： [haoyeeh](http://passport.baidu.com/?business&aid=6&un=haoyeeh#2) - [五级](http://www.baidu.com/search/zhidao_help.html#如何选择头衔)  ![]() 2007-7-2 12:15
[我来评论>>](http://zhidao.baidu.com/remark/29818029.html)

提问者对于答案的评价：
非常感谢大家   []()

相关内容

• [<Div id = "divPage" align=center style = "display: 'none' ">什么意思](http://zhidao.baidu.com/question/19858461.html?fr=qrl&cid=88&index=1&fr2=query)  2007-2-14 • [谁知道div_box.style.text-align = "center";在js里的正确写法](http://zhidao.baidu.com/question/127719522.html?fr=qrl&cid=88&index=2&fr2=query)  1  2009-12-2 • [<div class="altbg2" style="height:10px;padding:3px 5px 0px 0px;text-align:right;"> 这段代码翻一下](http://zhidao.baidu.com/question/71412442.html?fr=qrl&cid=88&index=3&fr2=query)  2008-10-24 • [IE打开网站时出现<div style='display:none'id="xml_2并全部黑掉](http://zhidao.baidu.com/question/30818624.html?fr=qrl&cid=88&index=4&fr2=query)  2007-7-22 • [关于javascript标签 <div id="c1" style="position:absolute;left:20;top:-20;](http://zhidao.baidu.com/question/54286023.html?fr=qrl&cid=88&index=5&fr2=query)  6  2008-5-27   [更多关于div style的问题>>](http://zhidao.baidu.com/q?word=div%20style&ct=17&pn=0&tn=ikaslist&rn=10&fr=qrl&cid=88&fr2=query) 查看同主题问题： [align](http://zhidao.baidu.com/topic?ct=29&tn=iktopic&word=align&fr=rtag&cid=88&index=1&fr2=query) [style](http://zhidao.baidu.com/topic?ct=29&tn=iktopic&word=style&fr=rtag&cid=88&index=2&fr2=query) [名称](http://zhidao.baidu.com/topic?ct=29&tn=iktopic&word=%C3%FB%B3%C6&fr=rtag&cid=88&index=3&fr2=query) [区别](http://zhidao.baidu.com/topic?ct=29&tn=iktopic&word=%C7%F8%B1%F0&fr=rtag&cid=88&index=4&fr2=query)
![]()
等待您来回答

* [稳态指标：动态指标；](http://zhidao.baidu.com/question/129470296.html?fr=newQuestion "稳态指标：动态指标；")
* [我刚装的DLINK路由器，但是只有一台机子可以上网，还需要拨号，局域网中却有两台电脑](http://zhidao.baidu.com/question/129470238.html?fr=newQuestion "我刚装的DLINK路由器，但是只有一台机子可以上网，还需要拨号，局域网中却有两台电脑")
* [苍井的资源！谢谢！](http://zhidao.baidu.com/question/129470235.html?fr=newQuestion "苍井的资源！谢谢！")
* [QQ农场好友 想偷菜吗？加我](http://zhidao.baidu.com/question/129470177.html?fr=newQuestion "QQ农场好友 想偷菜吗？加我")
* [我家的电信路由器坏了](http://zhidao.baidu.com/question/129470173.html?fr=newQuestion "我家的电信路由器坏了")
* [360网站问题](http://zhidao.baidu.com/question/129470164.html?fr=newQuestion "360网站问题")
* [如何计算IPV4地址可以容纳多少台主机](http://zhidao.baidu.com/question/129470130.html?fr=newQuestion "如何计算IPV4地址可以容纳多少台主机")
* [帮忙开通QQ牧场 .谢谢](http://zhidao.baidu.com/question/129470122.html?fr=newQuestion "帮忙开通QQ牧场 .谢谢")
©2009 Baidu
