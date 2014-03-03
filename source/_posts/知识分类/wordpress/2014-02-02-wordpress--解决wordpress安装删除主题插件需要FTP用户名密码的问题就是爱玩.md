---
layout: post
title: "解决wordpress安装删除主题插件需要FTP用户名密码的问题   就是爱玩"
categories: wordpress
tags: 
 - wordpress
--- 

# 解决wordpress安装删除主题插件需要FTP用户名密码的问题 就是爱玩

[![就是爱玩]( "就是爱玩")](http://94iw.com/)

免费资源分享，网络编程技术

* [首页](http://94iw.com/)
* [免费空间](http://94iw.com/freehost)
* [技术教程]()

* [电脑技巧](http://94iw.com/pcskill)
* [建站技术](http://94iw.com/webskill)
* [网络编程](http://94iw.com/webcode)
* [建站工具](http://94iw.com/webtools)
* [爱玩手机](http://94iw.com/mobile)
* [WordPress]()

* [技巧](http://94iw.com/wordpress/wordpress-skill)
* [主题](http://94iw.com/wordpress/wordpress-theme)
* [插件](http://94iw.com/wordpress/wordpress-plugin)
* [销售业务](http://94iw.com/sales)

* []()
* [订阅本站](http://94iw.com/feed "订阅本站")

« [android手机代理翻墙设置教程](http://94iw.com/android-mobile-agent-tutorial)

[WordPress获取制定指定分类文章数量](http://94iw.com/get-category-count) »

# 解决wordpress安装删除主题插件需要FTP用户名密码的问题

VPS 安装wordpress后，在后台自动升级时，或者更新、删除主题或者插件的时候，如果提示需要输入FTP账户信息，然而即使我们正确输入了FTP用户名 和密码也无法完成升级，这个是服务器端的权限设置问题，不是用户的问题。如果你是管理员，也遇到了这样的问题（新手）下面提供几个方法以供大家参考：

### 一、如果使用虚拟主机

方法1、可以在wp-config.php里加入下面代码:
1
2
3define("FS_METHOD", "direct");
define("FS_CHMOD_DIR", 0777);
define("FS_CHMOD_FILE", 0777);

方法2、拷贝下面的代码到wp-config.php中的?>之前

1
2
3
4
5/** Override default file permissions */
if(is_admin()) {
add_filter('filesystem_method',create_function('$a','return "direct";'));
define('FS_CHMOD_DIR', 0751);
}

方法3、修改FTP相关信息之后，拷贝代码到wp-config.php的?>之前

1
2
3
4
5
6//*added ftp login credentials to avoid the annoying prompt asking for login info every time I wanted to upgrade a plugin*
define('FTP_HOST', 'ftp.yoursite.com');
define('FTP_USER', 'Your_FTP_Username');
define('FTP_PASS', 'Your_FTP_password');
//*If you can use a SSL connection set this to true*
define('FTP_SSL', true);

### 二、如果使用独立服务器或VPS，可以修改网站所在目录属性：

1
2chmod -R 755 /home/wwwroot
chown -R www /home/wwwroot

其实出现这个的问题就是Apache/Nginx的执行身份非文件属主身份。

解决方法：
假设你的wordpress安装目录为/home/wwwroot/wordpress
执行：
1chown -R www /home/wwwroot/wordpress

执行上面的命令就可以将/home/wwwroot/wordpress下所有文件的属主改为www，

“www”换成你自己的ftp用户名，“/home/wwwroot/wordpress”换成你自己的wordpress安装目。

这样就可以解决自动更新必须填FTP的问题。

注意：

1，必须是把wordpress程序文件上传到空间以后再执行该命令，顺序不能颠倒；

2，添加完虚拟主机以后，也必须把wordpress程序文件上传到空间以后，再执行该命令才有效！

若出现了这个问题，不仅后台安装不了插件或主题，在ftp的wordpress目录下也是上传不了文件的，（折磨了我很久。。。）

via：[www.byncc.com/04-kloxowp.html](http://www.byncc.com/04-kloxowp.html)
[wordpress](http://94iw.com/tag/wordpress-2 "wordpress (7 篇文章)")

版权所有：[解决wordpress安装删除主题插件需要FTP用户名密码的问题]() | [就是爱玩](http://94iw.com/)
本文链接：[http://94iw.com/wordpress-ftp-password]()
版权声明：除非注明，本站所有文章皆为原创，转载请以链接形式标明本文地址

这篇文章由 [就是爱玩](http://94iw.com/author/admin "查看 就是爱玩 的所有文章 ") 于 2011年8月20日 16:49 发表在 [技巧](http://94iw.com/wordpress/wordpress-skill "技巧 (5 篇文章)")
您可以订阅 [RSS 2.0](http://94iw.com/wordpress-ftp-password/feed "RSS 2.0") 也可以发表 [评论](http://94iw.com/wordpress-ftp-password#commentform) 或 [引用](http://94iw.com/wordpress-ftp-password/trackback) 到您的网站
[打印](http://94iw.com/wordpress-ftp-password?print=1)

* [评论 （4）](http://94iw.com/wordpress-ftp-password#comments)
* [相关文章](http://94iw.com/wordpress-ftp-password#related-posts)
* ![安问]()

[#1](http://94iw.com/wordpress-ftp-password#comment-105) 由 [安问](http://askender.com/)  发表于 1 年前
嗯，多谢啦~写的挺好
 []()

[回复](http://94iw.com/wordpress-ftp-password?replytocom=105#respond) [引用](http://94iw.com/wordpress-ftp-password#commentform)

* ![就是爱玩]()

[#2](http://94iw.com/wordpress-ftp-password#comment-108) 由 **就是爱玩**  发表于 1 年前
客气了
 []()

[回复](http://94iw.com/wordpress-ftp-password?replytocom=108#respond) [引用](http://94iw.com/wordpress-ftp-password#commentform)
* ![洋洋]()

[#3](http://94iw.com/wordpress-ftp-password#comment-897) 由 [洋洋](http://o51k.com/)  发表于 1 年前
kloxo自动安装里就不会出现这种情况。
 []()

[回复](http://94iw.com/wordpress-ftp-password?replytocom=897#respond) [引用](http://94iw.com/wordpress-ftp-password#commentform)
* ![小文]()

[#4](http://94iw.com/wordpress-ftp-password#comment-3674) 由 [小文](http://weobo.com.javinwei/)  发表于 2 月前
我根据管理的方法操作，但是有的数据加上去无法再浏览网页了，我用的是SF空间，在之后我发现了一个有趣的事是，即使需要输入FTP账户密码设置了777权限但是还是无法安装，显示错误是：无法连接到 FTP 服务器 web.sf.net:21 但是我通过实验发现端口出现了问题，我使用的是22端口，但是WP自动默认的是21端口，但是我不知道如何修改端口，若管理知道如何修改可以帮助我，我非常感谢！
 []()

[回复](http://94iw.com/wordpress-ftp-password?replytocom=3674#respond) [引用](http://94iw.com/wordpress-ftp-password#commentform)
* ![]()

输入评论内容

您可以使用这些 HTML 标签：

<a>

<abbr>

<acronym>

<b>

<blockquote>

<cite>

<code>

<del>

<em>

<i>

<q>

<strike>

<strong>

![:ymy:]() ![:wink:]() ![:see:]() ![:sbq:]() ![:roll:]() ![:mrgreen:]() ![:lol:]() ![:idea:]() ![:gl:]() ![:gg:]() ![:evil:]() ![:cry:]() ![:cool:]() ![:arrow:]() ![:?:]() ![:-x]() ![:-o]() ![:-P]() ![:-D]() ![:-@]() ![:-?]() ![:)]() ![:(]() ![:!:]() ![:!!!:]()
* [订阅本文评论](http://94iw.com/wordpress-ftp-password/feed)
1. [wordpress更换空间和域名方法](http://94iw.com/methods-on-space-and-domain-name-wordpress "固定连接：wordpress更换空间和域名方法")
1. [主题 mystique 3.1 中文语言包下载](http://94iw.com/theme-mystique-3-1-chinese-language-pack "固定连接：主题 mystique 3.1 中文语言包下载")
1. [mystique 3.0.9 中文语言包](http://94iw.com/mystique-3-0-9-chinese-language-pack "固定连接：mystique 3.0.9 中文语言包")
1. [TinyMCE Xiami Music音乐插件](http://94iw.com/tinymce-xiami-music "固定连接：TinyMCE Xiami Music音乐插件")
1. [利用.htaccess文件加速wordpress](http://94iw.com/htaccess-accelerate-wordpress "固定连接：利用.htaccess文件加速wordpress")
1. [WordPress获取制定指定分类文章数量](http://94iw.com/get-category-count "固定连接：WordPress获取制定指定分类文章数量")

* [搜索](http://94iw.com/wordpress-ftp-password#)
* * [日历](http://94iw.com/wordpress-ftp-password#tab-atom-calendar-2)
* [归档](http://94iw.com/wordpress-ftp-password#tab-atom-archives-3)
* [标签](http://94iw.com/wordpress-ftp-password#tab-atom-tag-cloud-3)
* [最近的评论](http://94iw.com/wordpress-ftp-password#tab-atom-recent-comments-5)
* [登录](http://94iw.com/wordpress-ftp-password#tab-atom-login-2)
[« 八](http://94iw.com/2012/08 "查看文章 八月 2012")

### 2013 年一月

周日 周一 周二 周三 周四 周五 周六  12345 6789101112 13141516171819 20212223242526 2728293031  

* [八月 2012 (1)](http://94iw.com/2012/08)
* [七月 2012 (2)](http://94iw.com/2012/07)
* [六月 2012 (7)](http://94iw.com/2012/06)
* [一月 2012 (3)](http://94iw.com/2012/01)
* [十月 2011 (6)](http://94iw.com/2011/10)
* [九月 2011 (27)](http://94iw.com/2011/09)
* [八月 2011 (36)](http://94iw.com/2011/08)
[**cpanel**](http://94iw.com/tag/cpanel "34 个话题") [DirectAdmin](http://94iw.com/tag/directadmin "4 个话题") [freehost](http://94iw.com/tag/freehost-2 "1 个话题") [gofreeserve](http://94iw.com/tag/gofreeserve "1 个话题") [guru-host](http://94iw.com/tag/guru-host "1 个话题") [host-stage](http://94iw.com/tag/host-stage "2 个话题") [instantfreesite](http://94iw.com/tag/instantfreesite "1 个话题") [**iPanel**](http://94iw.com/tag/ipanel "8 个话题") [mystique](http://94iw.com/tag/mystique "2 个话题") [orangeserve](http://94iw.com/tag/orangeserve "1 个话题") [**php空间**](http://94iw.com/tag/php%e7%a9%ba%e9%97%b4 "10 个话题") [Positive Hosting](http://94iw.com/tag/positive-hosting "1 个话题") [pos机](http://94iw.com/tag/pos%e6%9c%ba "3 个话题") [squareserve](http://94iw.com/tag/squareserve "1 个话题") [SSH](http://94iw.com/tag/ssh "4 个话题") [VistaPanel](http://94iw.com/tag/vistapanel "4 个话题") [**WHM**](http://94iw.com/tag/whm "14 个话题") [**wordpress**](http://94iw.com/tag/wordpress-2 "7 个话题") [主题汉化](http://94iw.com/tag/%e4%b8%bb%e9%a2%98%e6%b1%89%e5%8c%96 "2 个话题") [**代理**](http://94iw.com/tag/%e4%bb%a3%e7%90%86 "6 个话题") [便民支付](http://94iw.com/tag/%e4%be%bf%e6%b0%91%e6%94%af%e4%bb%98 "3 个话题") [**免费空间**](http://94iw.com/tag/%e5%85%8d%e8%b4%b9%e7%a9%ba%e9%97%b4 "47 个话题") [全能空间](http://94iw.com/tag/%e5%85%a8%e8%83%bd%e7%a9%ba%e9%97%b4 "2 个话题") [**即时激活**](http://94iw.com/tag/%e5%8d%b3%e6%97%b6%e6%bf%80%e6%b4%bb "5 个话题") [建站工具](http://94iw.com/tag/webtools "2 个话题") [**拉卡拉**](http://94iw.com/tag/%e6%8b%89%e5%8d%a1%e6%8b%89 "6 个话题") [收款宝](http://94iw.com/tag/%e6%94%b6%e6%ac%be%e5%ae%9d "4 个话题") [**无限空间**](http://94iw.com/tag/%e6%97%a0%e9%99%90%e7%a9%ba%e9%97%b4 "7 个话题") [稳定空间](http://94iw.com/tag/%e7%a8%b3%e5%ae%9a%e7%a9%ba%e9%97%b4 "2 个话题") [**翻墙**](http://94iw.com/tag/%e7%bf%bb%e5%a2%99 "6 个话题")

* [![就是爱玩]()  就是爱玩 仪表盘>外观>Mystique>内容选项>首页选项>缩略图 勾选就OK了 3 天前](http://94iw.com/theme-mystique-3-1-chinese-language-pack/comment-page-3#comment-9314 "在 主题 mystique 3.1 中文语言包下载")
* [![名孙山]()  名孙山 请问万能的博主，首页标题前的预览缩略图怎么设置啊？实在搞不懂啊？ 3 天前](http://94iw.com/theme-mystique-3-1-chinese-language-pack/comment-page-3#comment-9268 "在 主题 mystique 3.1 中文语言包下载")
* [![YY]()  YY 我是通过在.htaccess中设置php_flag output_buffering on实现的。 2 周前](http://94iw.com/cpanel-panel-space-custom-php-ini/comment-page-1#comment-5109 "在 cPanel面板空间自定义php.ini")
* [![35分类目录]()  35分类目录 35分类目录，免费收录各种网站，免费提供各种网站友情链接交换，免费收录各种原创站长资讯，可带外链！ http://www.356688.com/ 2 周前](http://94iw.com/idcbuster/comment-page-1#comment-5058 "在 IdcBuster(1G 50G)国人提供美国DirectAdmin免费空间")
* [![网赚客]()  网赚客 相当不错的一个活动啊 1 月前](http://94iw.com/churpchurp/comment-page-1#comment-4579 "在 邀请朋友注册Churp Churp，获得丰厚奖励")
[显示更多](http://94iw.com/wordpress-ftp-password#)
您好游客，如果您已注册，请在下面登录

记住我

[忘记密码](http://94iw.com/wp-login.php?action=lostpassword)
[注册账号](http://94iw.com/sign-up/)
* ### 随机文章

* [serversfree(10G 100G)美国免费空间](http://94iw.com/serversfree "
 空间参数：
 
 	10000 M空......  2011.09.16")
* [0fees提供300m免费稳定php空间](http://94iw.com/0fees "
 免费虚拟主机功能
 我们为您提供最先......  2011.08.12")
* [host-stage免费100M全能空间](http://94iw.com/host-stage "
 空间特点：
 
 	100M磁盘空间......  2011.08.22")
* [IdcBuster(1G 50G)国人提供美国DirectAdmin免费空间](http://94iw.com/idcbuster "
 空间餐宿：
 
 	1024M空间
......  2012.08.11")
* [AWARDSPACE德国250M免费PHP空间申请教程](http://94iw.com/awardspace "
 空间参数：
 
 	250M磁盘空间......  2011.08.28")
* [使用CloudFlare免费CDN加速你的wordpress](http://94iw.com/free-use-cloudflare-sdn-accelerate-your-wordpress "什么是 CDN:
 CDN的全称是Con......  2011.09.24")
* [cubehosts(5G 50G)美国cpanel免费空间](http://94iw.com/cubehosts "
 空间参数：
 
 	5000M空间
......  2011.09.9")
* [simplefreeweb美国无限cPanel空间](http://94iw.com/simplefreeweb "
 空间参数：
 
 	无限空间
 	无......  2011.09.16")
* [解决wordpress安装删除主题插件需要FTP用户名密码的问题]( "VPS 安装wordpress后，在后台......  2011.08.20")
* [产品介绍](http://94iw.com/lakala-products "
  
 
 公司简介&nbs......  2012.06.17")
* ### 热评文章
* ### 热门标签

[0fees](http://94iw.com/tag/0fees "1 个话题") [aaa logo](http://94iw.com/tag/aaa-logo "1 个话题") [cpanel](http://94iw.com/tag/cpanel "34 个话题") [DirectAdmin](http://94iw.com/tag/directadmin "4 个话题") [EclipsePHP](http://94iw.com/tag/eclipsephp "1 个话题") [extract](http://94iw.com/tag/extract "1 个话题") [freehost](http://94iw.com/tag/freehost-2 "1 个话题") [gofreeserve](http://94iw.com/tag/gofreeserve "1 个话题") [guru-host](http://94iw.com/tag/guru-host "1 个话题") [host-stage](http://94iw.com/tag/host-stage "2 个话题") [infinite serve](http://94iw.com/tag/infinite-serve "1 个话题") [instantfreesite](http://94iw.com/tag/instantfreesite "1 个话题") [iPanel](http://94iw.com/tag/ipanel "8 个话题") [ip地址](http://94iw.com/tag/ip%e5%9c%b0%e5%9d%80 "1 个话题") [kilu.de](http://94iw.com/tag/kilu-de "1 个话题") [koolserve](http://94iw.com/tag/koolserve "1 个话题") [logo制作](http://94iw.com/tag/logo%e5%88%b6%e4%bd%9c "1 个话题") [mystique](http://94iw.com/tag/mystique "2 个话题") [orangeserve](http://94iw.com/tag/orangeserve "1 个话题") [php查ip](http://94iw.com/tag/php%e6%9f%a5ip "1 个话题") [php空间](http://94iw.com/tag/php%e7%a9%ba%e9%97%b4 "10 个话题") [Positive Hosting](http://94iw.com/tag/positive-hosting "1 个话题") [pos机](http://94iw.com/tag/pos%e6%9c%ba "3 个话题") [squareserve](http://94iw.com/tag/squareserve "1 个话题") [SSH](http://94iw.com/tag/ssh "4 个话题") [TrixieHost](http://94iw.com/tag/trixiehost "1 个话题") [VistaPanel](http://94iw.com/tag/vistapanel "4 个话题") [WHM](http://94iw.com/tag/whm "14 个话题") [wordpress](http://94iw.com/tag/wordpress-2 "7 个话题") [主题汉化](http://94iw.com/tag/%e4%b8%bb%e9%a2%98%e6%b1%89%e5%8c%96 "2 个话题") [代理](http://94iw.com/tag/%e4%bb%a3%e7%90%86 "6 个话题") [便民支付](http://94iw.com/tag/%e4%be%bf%e6%b0%91%e6%94%af%e4%bb%98 "3 个话题") [免费空间](http://94iw.com/tag/%e5%85%8d%e8%b4%b9%e7%a9%ba%e9%97%b4 "47 个话题") [全能空间](http://94iw.com/tag/%e5%85%a8%e8%83%bd%e7%a9%ba%e9%97%b4 "2 个话题") [即时激活](http://94iw.com/tag/%e5%8d%b3%e6%97%b6%e6%bf%80%e6%b4%bb "5 个话题") [在线解压](http://94iw.com/tag/%e5%9c%a8%e7%ba%bf%e8%a7%a3%e5%8e%8b "1 个话题") [建站工具](http://94iw.com/tag/webtools "2 个话题") [拉卡拉](http://94iw.com/tag/%e6%8b%89%e5%8d%a1%e6%8b%89 "6 个话题") [收款宝](http://94iw.com/tag/%e6%94%b6%e6%ac%be%e5%ae%9d "4 个话题") [无限空间](http://94iw.com/tag/%e6%97%a0%e9%99%90%e7%a9%ba%e9%97%b4 "7 个话题") [测试](http://94iw.com/tag/%e6%b5%8b%e8%af%95 "1 个话题") [稳定空间](http://94iw.com/tag/%e7%a8%b3%e5%ae%9a%e7%a9%ba%e9%97%b4 "2 个话题") [纯真ip](http://94iw.com/tag/%e7%ba%af%e7%9c%9fip "1 个话题") [翻墙](http://94iw.com/tag/%e7%bf%bb%e5%a2%99 "6 个话题") [远程下载](http://94iw.com/tag/%e8%bf%9c%e7%a8%8b%e4%b8%8b%e8%bd%bd "1 个话题")

[主题](http://94iw.com/wordpress/wordpress-theme "查看 主题 下的所有文章") (2)
[免费空间](http://94iw.com/freehost "查看 免费空间 下的所有文章") (49)
[建站工具](http://94iw.com/webtools "查看 建站工具 下的所有文章") (4)
[建站技术](http://94iw.com/webskill "查看 建站技术 下的所有文章") (4)
[技巧](http://94iw.com/wordpress/wordpress-skill "查看 技巧 下的所有文章") (5)
[插件](http://94iw.com/wordpress/wordpress-plugin "查看 插件 下的所有文章") (1)
[爱玩手机](http://94iw.com/mobile "查看 爱玩手机 下的所有文章") (2)
[电脑技巧](http://94iw.com/pcskill "查看 电脑技巧 下的所有文章") (5)
[网络编程](http://94iw.com/webcode "查看 网络编程 下的所有文章") (3)
[销售业务](http://94iw.com/sales "查看 销售业务 下的所有文章") (7)

WP-标签云 by [94iw.com](http://94iw.com/) and [就是爱玩](http://94iw.com/) 要求 [Flash Player](http://www.macromedia.com/go/getflashplayer) 9 or better.
* ### 可爱仓鼠
* ### 付费链接

* [阿迪达斯官方网站专卖](http://brand.xiu.com/1002.html)
* [企业网站托管](http://www.webshi.com.cn/)
* [体育明星](http://www.jackport520.com/)
* [系统之家](http://www.xpxtzj.com/)
* [广场舞大全](http://www.gcwdq.com/)
* [QQ空间秀](http://www.qqkongjianxiu.com/)
* [最极限UCD沙龙](http://www.zuijixian.com/)
* [给我留言](http://94iw.com/message)
* [联系站长](http://94iw.com/contact)
* [关于本站](http://94iw.com/about)
* [友情链接](http://94iw.com/links)

本站程序由 [Wordpress](http://wordpress.org/) 驱动 | 主题由 [digitalnature](http://digitalnature.eu/) 设计 | 版权为 [就是爱玩](http://94iw.com/ "就是爱玩") 所有

[网站统计](http://www.51.la/?4422386 "51.la 专业、免费、强健的访问统计") ![]()  <a href="http://www.51.la/?4422386" target="_blank"><img alt="我要啦免费统计" src="http://img.users.51.la/4422386.asp" style="border:none" /></a>
[回到页顶](http://94iw.com/wordpress-ftp-password#page)
