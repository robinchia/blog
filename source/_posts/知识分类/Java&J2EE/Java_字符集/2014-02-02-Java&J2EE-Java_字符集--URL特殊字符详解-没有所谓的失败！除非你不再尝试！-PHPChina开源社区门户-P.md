---
layout: post
title: "URL特殊字符详解 - 没有所谓的失败！除非你不再尝试！ - PHPChina 开源社区门户 - P"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_字符集
--- 

# URL特殊字符详解 - 没有所谓的失败！除非你不再尝试！ - PHPChina 开源社区门户 - Powered by X-Space

**没有所谓的失败！除非你不再尝试！**

[copy]( "复制地址") [Bookmark](http://smarty.blog.phpchina.com/ "加入收藏") http://smarty.blog.phpchina.com

* [好友](http://www.phpchina.com/?uid-26354-action-spacelist-type-friend)
* [论坛](http://www.phpchina.com/?uid-26354-action-spacelist-type-bbs)
* [留言](http://www.phpchina.com/?uid-26354-action-viewpro)

[空间管理](http://www.phpchina.com/batch.manage.php?uid=26354) 您的位置: [PHPChina 开源社区门户](http://www.phpchina.com/) » [没有所谓的失败！除非你不再尝试！](http://www.phpchina.com/?uid-26354) » [日志](http://www.phpchina.com/?uid-26354-action-spacelist-type-blog)

每天都在进步一点点，在这里我将记下我的点点滴滴，希望和大家一起进步，一起学习，一起共同成长PHPer之路！ PHPer成长群(49052575)欢迎各界精英加入，互相学习，互补不足。空间不再更新：如有需要请进 http://www.phpcake.cn
# URL特殊字符详解

[上一篇](http://www.phpchina.com/batch.common.php?action=viewspace&op=up&itemid=32317&uid=26354) / [下一篇](http://www.phpchina.com/batch.common.php?action=viewspace&op=next&itemid=32317&uid=26354)  2008-04-11 19:48:57 / 个人分类：[url特殊字符](http://www.phpchina.com/?uid-26354-action-spacelist-type-blog-itemtypeid-3538)
[查看( 386 )](http://www.phpchina.com/html/54/26354-32317.html#xspace-tracks) / [评论( 2 )](http://www.phpchina.com/html/54/26354-32317.html#xspace-itemreply) / [评分( 1 / 0 )](http://www.phpchina.com/html/54/26354-32317.html#xspace-itemform)

有些符号在URL中是不能直接传递的，如果要在URL中传递这些特殊符号，那么就要使用他们的编码了。编码的格式为：%加字符的ASCII码，即一个百分号%，后面跟对应字符的ASCII（16进制）码值。例如 空格的编码值是”%20″。
下表中列出了一些URL特殊符号及编码
十六进制值 1. + URL 中+号表示空格 %2B 2. 空格 URL中的空格可以用+号或者编码 %20 3. / 分隔目录和子目录 %2F 4. ? 分隔实际的 URL 和参数 %3F 5. % 指定特殊字符 %25 6. # 表示书签 %23 7. & URL 中指定的参数间的分隔符 %26 8. = URL 中指定参数的值 %3D

例：要传递字符串“this%is#te=st&o k?+/”作为参数t传给te.asp，则URL可以是:
te.asp?t=this%25is%23te%3Dst%26o%20k%3F%2B%2F 或者
te.asp?t=this%25is%23te%3Dst%26o+k%3F%2B%2F （空格可以用%20或+代替）java中URL 的编码和解码函数
java.net.URLEncoder.encode(String s)和java.net.URLDecoder.decode(String s);
注：encode(String s)[**方法**]()过期了，现在一般用encode(String s, “GBK”);

在javascrīpt 中URL 的编码和解码函数
[**escape**]()(String s)和[**unescape**]()(String s) ;
### 相关阅读:

* [encodeURIComponent应用](http://www.phpchina.com/?uid-26354-action-viewspace-itemid-20379) ([design_dd](http://www.phpchina.com/?uid-26354), 2007-12-06)
* [FLEAPHP 的一些基础学习连接](http://www.phpchina.com/?uid-50821-action-viewspace-itemid-30743) ([brucehawking](http://www.phpchina.com/?uid-50821), 2008-3-30)

[导入论坛]() [收藏]() [分享给好友]() [管理](http://www.phpchina.com/batch.manage.php?itemid=32317) [举报]()

TAG: [url](http://www.phpchina.com/?action-tag-tagname-url) [escape](http://www.phpchina.com/?action-tag-tagname-escape) [unescape](http://www.phpchina.com/?action-tag-tagname-unescape) [url特殊字符](http://www.phpchina.com/?action-tag-tagname-url%CC%D8%CA%E2%D7%D6%B7%FB)
[![没有所谓的失败！除非你不再尝试！]()](http://www.phpchina.com/?uid-26354) [引用]() [删除]() [design_dd](http://www.phpchina.com/?uid-26354)   /   2008-04-19 16:11:49 谢谢支持^_^ ![]() [引用]() [删除]() 废墟   /   2008-04-17 21:36:42 评 1 分
不错。。怎么没人顶啊  。。这些人 就知道看 也不顶。。

[查看全部评论]()

[-5]() [-3]() [-1]() [-]() [+1]() [+3]() [+5]()

评分：0

我来说两句

显示全部

![:loveliness:]() ![:handshake]() ![:victory:]() ![:funk:]() ![:time:]() ![:kiss:]() ![:call:]() ![:hug:]() ![:lol]() ![:'(]() ![:Q]() ![:L]() ![;P]() ![:$]() ![:P]() ![:o]() ![:@]() ![:D]() ![:(]() ![:)]()

内容

昵称

加入事件

提交评论

![design_dd]()

[design_dd](http://www.phpchina.com/?uid-26354-action-viewpro-showpro-1)

### 用户菜单

* [给我留言](http://www.phpchina.com/?uid-26354-action-viewpro)
* [加入好友]()
* [发短消息](http://bbs.phpchina.com/pm.php?action=send&uid=26354)
* [我的介绍](http://www.phpchina.com/?uid-26354-action-viewpro-showpro-1)
* [论坛资料](http://bbs.phpchina.com/viewpro.php?uid=26354)
* [空间管理](http://www.phpchina.com/batch.manage.php?uid=26354)
### 标题搜索

### 我的存档

* [2008年04月](http://www.phpchina.com/?uid-26354-action-spacelist-starttime-1206979200-endtime-1209571200)
* [2008年03月](http://www.phpchina.com/?uid-26354-action-spacelist-starttime-1204300800-endtime-1206979200)
* [2008年02月](http://www.phpchina.com/?uid-26354-action-spacelist-starttime-1201795200-endtime-1204300800)
* [2008年01月](http://www.phpchina.com/?uid-26354-action-spacelist-starttime-1199116800-endtime-1201795200)
* [2007年12月](http://www.phpchina.com/?uid-26354-action-spacelist-starttime-1196438400-endtime-1199116800)
* [2007年11月](http://www.phpchina.com/?uid-26354-action-spacelist-starttime-1193846400-endtime-1196438400)
* [2007年10月](http://www.phpchina.com/?uid-26354-action-spacelist-starttime-1191168000-endtime-1193846400)
* [2007年09月](http://www.phpchina.com/?uid-26354-action-spacelist-starttime-1188576000-endtime-1191168000)
* [2007年08月](http://www.phpchina.com/?uid-26354-action-spacelist-starttime-1185897600-endtime-1188576000)
* [2007年07月](http://www.phpchina.com/?uid-26354-action-spacelist-starttime-1183219200-endtime-1185897600)
* [2007年06月](http://www.phpchina.com/?uid-26354-action-spacelist-starttime-1180627200-endtime-1183219200)
* [2007年05月](http://www.phpchina.com/?uid-26354-action-spacelist-starttime-1177948800-endtime-1180627200)
### 数据统计

* 访问量: 30236
* 日志数: 69
* 建立时间: 2007-05-14
* 更新时间: 2008-04-11

### RSS订阅

* [![RSS订阅]()](http://www.phpchina.com/?uid-26354-action-rss-type-blog)

[清空Cookie](http://www.phpchina.com/batch.login.php?action=logout) - [联系我们](mailto:PHPChina) - [PHPChina 开源社区门户](http://www.phpchina.com/) - [交流论坛](http://bbs.phpchina.com/) - [空间列表](http://www.phpchina.com/?action/spaces) - [站点存档](http://www.phpchina.com/archiver/) - [升级自己的空间](http://www.phpchina.com/?action/register)

Powered by [**X-Space**](http://www.supesite.com/) *4.0 UC* © 2001-2008 [Comsenz Inc.](http://www.comsenz.com/)
[京ICP备07504665号](http://www.miibeian.gov.cn/)
[Open Toolbar]()
