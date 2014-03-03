---
layout: post
title: "httpclient中文乱码问题解决方法（收藏）_bingxuelian的空间_百度空间"
categories: Httpclient
tags: 
 - Httpclient
--- 

# httpclient中文乱码问题解决方法（收藏）_bingxuelian的空间_百度空间

![]()

### 分享到

* [QQ空间](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)
* [新浪微博](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)
* [百度搜藏](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)
* [人人网](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)
* [腾讯微博](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)
* [开心网](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)
* [腾讯朋友](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)
* [更多...](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)

[百度分享](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)

[](http://hi.baidu.com/)

* [广场](http://hi.baidu.com/index)
* [登录](http://hi.baidu.com/go/login)
*
* [注册](http://hi.baidu.com/go/reg?pmfrom=qingtop)
[关注此空间](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)
# [bingxuelian的空间](http://hi.baidu.com/new/rtgguumtrjjmqsr)

安然学习的地方！冰雪莲！

2010-07-21 12:50

## httpclient中文乱码问题解决方法（收藏）

这里，介绍一种解决抓取后网页内容显示为乱码的办法。
前几天，在抓取某网站的信息时(http://www.99sj.com/Price/Price/Default.aspx)，第一次碰到了这种应用下的乱码问题。于是上网查了一下，提供的解决办法大致有两种：
１>　　private static final String CONTENT_CHARSET = "GBK";
httpClient.getParams().setContentCharset("UTF-8");
or
httpClient.getParams().setParameter(HttpMethodParams.HTTP_CONTENT_CHARSET, ＣONTENT_CHARSET);
2>　　private static final String CONTENTTYPE = " text/html; charset=GBK";
getMethod.setRequestHeader("Content-Type", CONTENTTYPE);
测试了，没有任何效果（换成UTF-8也不行）。也用了String result = new String(pageSrc.getBytes("UTF-8"),"GBK")，依然无效。
在焦头烂额时想到了以前在学校时经常用的一句话：找问题要会追根溯源。仔细想想，字符串里面的文本内容也是通过文件流获取的，既然转换字符串字符编码不起作用，那可以设置文件流的默认编码吗？查了jdk，是可行的。
private static final String CHARSET = "UTF-8";
InputStream ins = getMethod.getResponseBodyAsStream();
//按指定的字符集构建文件流
BufferedReader br = new BufferedReader(new InputStreamReader(ins,CHARSET));
StringBuffer sbf = new StringBuffer();
String line = null;
while ((line = br.readLine()) != null)
{
sbf.append(line);
}
/** 回收资源 */
br.close();
getMethod.releaseConnection();
/** 页面源文件 */
pageSource = sbf.toString();
问题解决，^_^。这里的CHARSET要根据实际情况设置
[#搜索引擎--爬虫技术](http://hi.baidu.com/tag/%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E--%E7%88%AC%E8%99%AB%E6%8A%80%E6%9C%AF/feeds)

[举报](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#) 浏览(303) [评论](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#) [转载](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)

[](http://hi.baidu.com/rtgguumtrjjmqsr/item/4c5e943cbb3be59eb80c03a5 "上一篇")

[](http://hi.baidu.com/rtgguumtrjjmqsr/item/9259a4973bc07ddb1f4271a5 "下一篇")
评论

 同时评论给 

 同时评论给原文作者 

[ 发布 ](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)

500/0

[收起](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)|[查看更多](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#reply)

[帮助中心](http://hi.baidu.com/go/show/introduce) | [空间客服](http://tieba.baidu.com/p/1781683739) | [投诉中心](http://tousu.baidu.com/hi/add) | [空间协议](http://www.baidu.com/search/hi_contract.html)

©2012 Baidu
内容很精彩，关注此空间？[](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)

[![bingxuelian666]()](http://hi.baidu.com/new/rtgguumtrjjmqsr "bingxuelian666")

[bingxuelian666](http://hi.baidu.com/new/rtgguumtrjjmqsr)

性别：女年龄：0岁

现居：[陕西西安]( "陕西 西安 ")

粉丝：[37](http://hi.baidu.com/rtgguumtrjjmqsr/qfriend/show/fans)关注：[47](http://hi.baidu.com/rtgguumtrjjmqsr/qfriend/show/follow)
[关注](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)[暂不](http://hi.baidu.com/rtgguumtrjjmqsr/item/7c49ab87b2b3ef804414cfa2#)
