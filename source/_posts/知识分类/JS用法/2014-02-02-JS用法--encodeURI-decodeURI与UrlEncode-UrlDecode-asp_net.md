---
layout: post
title: "encodeURI-decodeURI与UrlEncode-UrlDecode - asp_net "
categories: JS用法
tags: 
 - JS用法
--- 

# encodeURI-decodeURI与UrlEncode-UrlDecode - asp_net - lzyox

  [博客首页](http://blog.chinaunix.net/) [注册](http://blog.chinaunix.net/register.php) [建议与交流](http://bbs.chinaunix.net/forumdisplay.php?fid=51) [排行榜](http://blog.chinaunix.net/top/) [加入友情链接](http://blog.chinaunix.net/u3/94369/)  ![]() [推荐](http://blog.chinaunix.net/u2/star.php?blogid=94369 "给此博客推荐值") [投诉](http://blog.chinaunix.net/u2/complaint.php?blogid=94369 "投诉此博客") 搜索：  [帮助](http://blog.chinaunix.net/help/)  **lzyox**
 I'm willing to be an ox serving the country all my life.   [lzyox.cublog.cn](http://lzyox.cublog.cn/) 
* [管理博客](http://control.cublog.cn/)
* [发表文章](http://control.cublog.cn/article_new.php)
* [留言](http://blog.chinaunix.net/u3/94369/guestbook.html)
* [收藏夹](http://blog.chinaunix.net/u3/94369/links.html)

* [· 网络技术](http://blog.chinaunix.net/u3/94369/links_18659.html)
* [博客圈](http://blog.chinaunix.net/u3/94369/group.html)
* [音乐](http://blog.chinaunix.net/u3/94369/music.html)
* [相册](http://blog.chinaunix.net/u3/94369/photo.html)
* [文章](http://blog.chinaunix.net/u3/94369/article.html)

* [· asp](http://blog.chinaunix.net/u3/94369/article_109275.html)
* [· asp.net](http://blog.chinaunix.net/u3/94369/article_108876.html)
* [· CSharp](http://blog.chinaunix.net/u3/94369/article_108878.html)
* [· 协议](http://blog.chinaunix.net/u3/94369/article_108881.html)
* [· 数码](http://blog.chinaunix.net/u3/94369/article_109281.html)
* [· 数据库](http://blog.chinaunix.net/u3/94369/article_109282.html)
* [· 手机](http://blog.chinaunix.net/u3/94369/article_109283.html)
* [· 网络技术](http://blog.chinaunix.net/u3/94369/article_109290.html)
* [· javascript](http://blog.chinaunix.net/u3/94369/article_108880.html)
* [· linux](http://blog.chinaunix.net/u3/94369/article_111594.html)
* [· oracle](http://blog.chinaunix.net/u3/94369/article_109276.html)
* [· php](http://blog.chinaunix.net/u3/94369/article_119444.html)
* [· sqlserver](http://blog.chinaunix.net/u3/94369/article_109278.html)
* [· unix](http://blog.chinaunix.net/u3/94369/article_108872.html)
* [· vb](http://blog.chinaunix.net/u3/94369/article_108874.html)
* [· vc](http://blog.chinaunix.net/u3/94369/article_108873.html)
* [· web技术](http://blog.chinaunix.net/u3/94369/article_109277.html)
* [· windows](http://blog.chinaunix.net/u3/94369/article_108882.html)
* [· 其他](http://blog.chinaunix.net/u3/94369/article_109279.html)
* [· 安全杀毒](http://blog.chinaunix.net/u3/94369/article_108883.html)
* [· 编程技术经验](http://blog.chinaunix.net/u3/94369/article_108884.html)
* [· 汇编](http://blog.chinaunix.net/u3/94369/article_111595.html)
* [首页](http://blog.chinaunix.net/u3/94369/index.html)    ![]() **关于作者** ![]( "收起")   [![]()](http://blog.chinaunix.net/u3/94369/) || <<   >> ||   **我的分类** ![]( "收起")      ![]()  ![]()  ![]() ![]()  ![]() **encodeURI/decodeURI与UrlEncode/UrlDecode**   **摘要：**
关于encodeURI,标准似乎是这么定义的: 

"如果有空格就用%20代替，如果有其它字符就用%ASCII代替，如果有汉字等四个字节的字符，就用两个%ASCII来代替"

然而MS向来标新立异,继[encodeURI之URL中文参数问题](http://www.cnblogs.com/walkingboy/archive/2007/01/26/encodeURIAndFormAction_Chinese.html)之后,encodeURI的噩梦继续袭来......

**一、该死的空格**

最近做两个页面的数据交换.由PageA发起Ajax请求到PageB,PageB从数据库读取数据返回给PageA,由于怕中间有特殊字符会导致js失败,所以使用了UrlEncode进行URI编码,再在客户端进行decodeURI解码.

结果发现空格无法被正确识别，UrlEncode将空格编码为+，而decodeURI只识别20%表示的空格。初步判断UrlEncode的编码格式和encodeURI不一致，为了验证这个看法，于是提取了键盘上的一些特殊符号进行编码比对：

 

字符:        ~    ! @    # $     % ^ & * ()_ +    - =UrlEncode: %7e ! %40 %23 %24 %25 %5e %26 * ()_ %2b - %3dencodeURI: ~    ! @    # $     %25 %5E & * ()_ +    - =字符:        {    }    [    ] \ | ' ; :    "    / ? . , < >UrlEncode: %7b %7d %5b %5d %5c %7c ' %3b %3a %22 %2f %3f . %2c %3c %3eencodeURI: %7B %7D %5B %5D %5C %7C ' ; :    %22 / ? . , %3C %3E

 

这些比较可以看出,两个的编码确实存在比较大区别上,特别是对于特殊字符的处理上面.

由此相当，在[encodeURI之URL中文参数问题](http://www.cnblogs.com/walkingboy/archive/2007/01/26/encodeURIAndFormAction_Chinese.html)中自己所认为了，asp.net对form的action进行了2次编码的判断应该是错的,其实并没有进行2次编码,只是asp.net在接受到encodeURI编码的action之后,利用UrlDecode进行解码,然后再次用UrlEncode进行编码写入Html中,由于编码格式不一致,所以Postback之后的URI,js就无法使用decodeURI进行正确解码.

由此可以知道,如果你用encodeURI编码的字符串,是可以通过UrlDecode解码出来的,也就是说UrlDecode可以识别encodeURI(js)和UrlEncode(c#)两个编码格式.可以想到,MS在设计这个类库的时候,已经考虑到了会接受到encodeURI的编码,按常理来想的话,既然考虑到了解码,自然会考虑到编码,也即UrlEncode应该提供可以编码成decodeURI可以解码的格式.可别的是,我一直无法找到这个方式.不知道是设计者给我们开的一个小玩笑,还是留下点瑕疵好让我们燃起编程的激情,不至于对千篇一律的Code工作感到厌倦,残念......

**二、让SP来得更猛烈些吧**

因为存在这个编码的不一致性,导致如果你的程序需要做比较多的Server-Client数据沟通的话,只能通过其他途径(json,xml等非URI),即使只是一个简单的字符串,你也需要增加许多额外的数据以满足你的格式.

一如MS的很多软件一样,SP满天飞,看来我也只好自己进行SP了.

 

分析下编码中差异,基本都集中在特殊字符的处理上,对于中文的处理貌似一致的(目前还没有测试出差异).于是定下了"利用encodeURI/decodeURI处理中文字符,其他的进行手工处理"的方案,修改了下之前的js代码:

 

KINN.Util.EncodeURI = function(unzipStr,isCusEncode){    if(isCusEncode){        var zipArray = new Array();        var zipstr = "";        var lens = new Array();        for(var i=0;i<unzipStr.length;i++){         var ac = unzipStr.charCodeAt(i);         zipstr += ac;         lens = lens.concat(ac.toString().length);        }        zipArray = zipArray.concat(zipstr);        zipArray = zipArray.concat(lens.join("O"));        return zipArray.join("N");    }else{        //return encodeURI(unzipStr);        var zipstr="";        var strSpecial="!\"#$%&'()*+,/:;<=>?[]^`{|}~%";        var tt= "";        for(var i=0;i<unzipStr.length;i++){            var chr = unzipStr.charAt(i);            var c=KINN.Util.StringToAscii(chr);            tt += chr+":"+c+"n";            if(parseInt("0x"+c) > 0x7f){                 zipstr+=encodeURI(unzipStr.substr(i,1));            }else{                 if(chr==" ")                    zipstr+="+";                 else if(strSpecial.indexOf(chr)!=-1)                    zipstr+="%"+c.toString(16);                 else                    zipstr+=chr;                }            }        return zipstr;    }}KINN.Util.DecodeURI = function(zipStr,isCusEncode){    if(isCusEncode){        var zipArray = zipStr.split("N");        var zipSrcStr = zipArray[0];        var zipLens;        if(zipArray[1]){            zipLens = zipArray[1].split("O");            }else{            zipLens.length = 0;        }                var uzipStr = "";                for(var j=0;j<zipLens.length;j++){            var charLen = parseInt(zipLens[j]);            uzipStr+= String.fromCharCode(zipSrcStr.substr(0,charLen));            zipSrcStr = zipSrcStr.slice(charLen,zipSrcStr.length);        }                return uzipStr;    }else{        //return decodeURI(zipStr);        var uzipStr="";        for(var i=0;i<zipStr.length;i++){            var chr = zipStr.charAt(i);            if(chr == "+"){                 uzipStr+=" ";            }else if(chr=="%"){                 var asc = zipStr.substring(i+1,i+3);                 if(parseInt("0x"+asc)>0x7f){                     uzipStr+=decodeURI("%"+asc.toString()+zipStr.substring(i+3,i+9).toString()); ;                     i+=8;                 }else{                     uzipStr+=KINN.Util.AsciiToString(parseInt("0x"+asc));                     i+=2;                 }            }else{                 uzipStr+= chr;            }        }        return uzipStr;    }}KINN.Util.StringToAscii = function(str){    return str.charCodeAt(0).toString(16);}KINN.Util.AsciiToString = function(asccode){    return String.fromCharCode(asccode);}

 

 

**三、浪子语：**
很奇怪，为什么Asp.net老是存在某些小毛病小问题,不知道是设计者忽略了,还是真的为了改善我们程序员苦闷的编程生活?

java里面的编码就是和encodeURI一样的.

或许标准就是用来打破,IE如此,ASP.NET依然如此...... 发表于： 2009-04-13，修改于： 2009-04-13 10:36，已浏览475次，有评论0条 [推荐](http://blog.chinaunix.net/u2/star.php?blogid=94369&artid=1899042 "推荐这篇文章") [投诉](http://blog.chinaunix.net/u2/complaint.php?blogid=94369&artid=1899042 "投诉这篇文章") ![]()  ![]()![]()  ![]()
![]()  ![]() **网友评论**
![]()  ![]() 发表评论 ![]()  ![]()![]()  ![]()
