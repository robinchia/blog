---
layout: post
title: "firefox中的ajax同步传输--谌俊"
categories: JS用法
tags: 
 - JS用法
--- 

# firefox中的ajax同步传输--谌俊

[![]()](http://www.jingzhengli.cn/) [![效益型网络营销方案]()](http://www.jingzhengli.com/solution/tigao.html) ![]()[网络营销方案](http://www.jingzhengli.com/solution.html)

![]()[网站优化方案](http://www.jingzhengli.cn/youhua.htm)
![]()[网络营销培训](http://www.jingzhengli.cn/peixun.htm)

   [新竞争力首页](http://www.jingzhengli.cn/index.htm) | [公司博客首页](http://www.jingzhengli.cn/Blog/index.html) | [个人首页](http://www.jingzhengli.cn/Blog/cj/index.html) | [相册](http://www.jingzhengli.cn/Blog/cj/cmd.html?uid=36&do=album) | [标签](http://www.jingzhengli.cn/Blog/cj/cmd.html?uid=36&do=tags)谌俊
当前位置：[新竞争力首页](http://www.jingzhengli.cn/index.htm) > [公司博客首页](http://www.jingzhengli.cn/Blog/index.html) > 谌俊

firefox中的ajax同步传输

    以前很少使用到ajax同步传输，也没有注意到其在firefox中的表现！前两周因项目需要，使用到了ajax同步传输，但在测试中发现一般使用的程序firefox中的ajax同步传输无法执行。相同代码，异步状态在firefox下正常，但无法实现需要的功能！
<script language="javascript" type="text/javascript">
function GetData()
{ 
    var result;
    var xmlhttp = create_XML_object();
    xmlhttp.open("POST", "/test.aspx", false);
    xmlhttp.onreadystatechange=function() 
    {
        if (xmlhttp.readyState==4)
        {
            result = xmlhttp.responseText;
        }
    }
    xmlhttp.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    xmlhttp.send(null);
    return result;
}
</script>

    以上基础的ajax调用代码，ajax异步传输在所有浏览器正常，ajax同步传输在IE等其他浏览器中正常，在firefox中的ajax同步传输不会起作用！
    究其原因，就是firefox经常打起的标准大旗（可害苦了不少人），firefox中对ajax同步请求是不调用状态改变函数onreadystatechange的，firefox中的ajax同步传输则在xmlhttp.send(null)之后直接使用xmlhttp.responseText便可获取ajax同步传输返回值！那么我们在JS中就先判断浏览器类型，然后调用不同的代码实现ajax同步传输，代码如下：

<script language="javascript" type="text/javascript">
function getOs()
{
   var OsObject = "";
   if(navigator.userAgent.indexOf("MSIE")>0) {
        return "MSIE";       //IE浏览器
   }
   if(isFirefox=navigator.userAgent.indexOf("Firefox")>0){
        return "Firefox";     //Firefox浏览器
   }
   if(isSafari=navigator.userAgent.indexOf("Safari")>0) {
        return "Safari";      //Safan浏览器
   }
   if(isCamino=navigator.userAgent.indexOf("Camino")>0){
        return "Camino";   //Camino浏览器
   }
   if(isMozilla=navigator.userAgent.indexOf("Gecko/")>0){
        return "Gecko";    //Gecko浏览器
   }
}
function GetData()
{ 
    var result;
    var xmlhttp = create_XML_object();
    xmlhttp.open("POST", "/test.aspx", false);
    var btype=getOs();
    if(btype!="Firefox")
    {
        xmlhttp.onreadystatechange=function() 
        {
            if (xmlhttp.readyState==4)
            {
                result = xmlhttp.responseText;
            }
        }
    }
    xmlhttp.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    xmlhttp.send(null);
    if(btype=="Firefox")
    {
        result = xmlhttp.responseText;
    }
    return result;
}
</script>

    大功告成，实现在firefox中的ajax同步传输！
* 谌俊 发表于 2009-9-7 15:59:37
* [ [阅读全文](http://www.jingzhengli.cn/Blog/cj/1043.html#) | [回复(0)](http://www.jingzhengli.cn/Blog/cj/1043.html#cmt) | [编辑](http://www.jingzhengli.cn/Blog/user_post.asp?logid=1043) ]

* 上一篇：[location.href不跳转的解决办法](http://www.jingzhengli.cn/Blog/cj/1039.html)
[]()

## 发表评论：

昵称：

密码：

标题：

公告栏

管理博客
日志分类

[![]()](http://www.jingzhengli.cn/Blog/cj/rss2.xml)
最新日志

* [firefox中的ajax同步传输](http://www.jingzhengli.cn/Blog/cj/1043.html "发表于2009-9-7 15:59:37")
* [location.href不跳转的解](http://www.jingzhengli.cn/Blog/cj/1039.html "发表于2009-9-2 14:06:32")
* [小型编辑器(jquery)](http://www.jingzhengli.cn/Blog/cj/1032.html "发表于2009-8-17 14:46:00")
* [HTML生成PDF(c#)](http://www.jingzhengli.cn/Blog/cj/1030.html "发表于2009-8-14 17:22:53")
* [SQL Server 索引结构及其使](http://www.jingzhengli.cn/Blog/cj/1023.html "发表于2009-8-5 14:52:39")
* [SQLServer索引结构及其使用（](http://www.jingzhengli.cn/Blog/cj/1022.html "发表于2009-8-5 14:46:13")
* [SQLServer索引结构及其使用（](http://www.jingzhengli.cn/Blog/cj/1021.html "发表于2009-8-5 14:43:00")
* [SQL Server 索引结构及其使](http://www.jingzhengli.cn/Blog/cj/1020.html "发表于2009-8-5 14:37:39")
* [查看sql语句执行时间](http://www.jingzhengli.cn/Blog/cj/1019.html "发表于2009-8-5 14:02:21")
* [测试SQL语句执行时间](http://www.jingzhengli.cn/Blog/cj/1018.html "发表于2009-8-5 14:01:01")
最新回复

最新留言
* [::签写留言::](http://www.jingzhengli.cn/Blog/cj/message.html#cmt)

友情链接
[新竞争力-谌俊的博客](http://www.jingzhengli.cn/blog/cj/index.html)

信息统计
* 日志总数:16
* 评论数量:0
* 留言数量:0
* 访问次数:
* [加为好友](http://www.jingzhengli.cn/Blog/user_friends.asp?action=saveadd&friendname=谌俊)　[发送短信]()

“新竞争力”是深圳市竞争力科技有限公司的注册商标
深圳市竞争力科技有限公司 版权所有
电话:86-755-26502263 　Email:[info@jingzhengli.cn](mailto:info@jingzhengli.cn)
