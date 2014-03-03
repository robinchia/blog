---
layout: post
title: "windows下安装ipython"
categories: python
tags: 
 - python
--- 

# windows下安装ipython

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

[弧光守望者-itfanr](http://itfanr.duapp.com/ "弧光守望者-itfanr - 又一个 WordPress 站点")

* [关于](http://itfanr.duapp.com/?page_id=5)
* [在线markdown](http://itfanr.duapp.com/?page_id=84)

* [订阅关注]()

* [腾讯微博](http://t.qq.com/itfanr)
* [新浪微博](http://weibo.com/itfan)
### 订阅到：

[Google Reader](http://fusion.google.com/add?feedurl=http://itfanr.duapp.com/?feed=rss2) [QQ邮箱](http://mail.qq.com/cgi-bin/feed?u=http://itfanr.duapp.com/?feed=rss2) [鲜果](http://www.xianguo.com/subscribe.php?url=http://itfanr.duapp.com/?feed=rss2) [抓虾](http://www.zhuaxia.com/add_channel.php?url=http://itfanr.duapp.com/?feed=rss2)

### 订阅地址：

### 邮件订阅：
* [用户登陆]()

### 用户名：

### 密码：

[找回密码](http://itfanr.duapp.com/wp-login.php?action=lostpassword)
# [windows下安装ipython](http://itfanr.duapp.com/?p=343 "猛击查看 windows下安装ipython 的详细内容")

分享到：[]( "分享到新浪微博")[]( "分享到QQ空间")[]( "分享到腾讯微博")[]( "分享到人人网")[]( "分享到开心网")[]( "分享到复制网址")更多[0]( "累计分享0次")

2013-08-13   分类：[Python](http://itfanr.duapp.com/?cat=28 "查看Python中的全部文章")[1条评论]( "查看 windows下安装ipython 的评论")63次浏览

![]()

看了好多博客和官方文档，感觉windows下安装ipython很复杂。后来发现其实很简单。

首先在[python官网](http://www.python.org/getit/)下载python installer。建议安装**python 2.x**版本，因为python 3.x版本和2.x版本不是一个体系，网上资料较少。安装后记得将**python.exe**所在目录设置为PATH路径。

然后下载**ex_setup.py**这个包管理工具，官网是[https://pypi.python.org/pypi/ez_setup](https://pypi.python.org/pypi/ez_setup)，但是建议下载[这个](http://peak.telecommunity.com/dist/ez_setup.py)。下载得到**ez_setup.py**文件。然后cd到该文件所在目录，然后执行：
[view source]( "view source")

1

python ez_setup.py

然后输入安装[pip包管理工具](https://pypi.python.org/pypi/pip)：

[view source]( "view source")

1

easy_install pip

最后通过pip安装ipython：

[view source]( "view source")

1

pip install ipython

在CMD命令行输入ipython

Enjoy it 。

如果是更新ipython，当然是先uninstall，然后install了
[view source]( "view source")

1

pip uninstall ipython

建议下载console2 [http://sourceforge.net/projects/console/](http://sourceforge.net/projects/console/) 替代windows自带的命令行。

Console is a Windows console window enhancement. Console features include: multiple tabs, text editor-like text selection, different background types, alpha and color-key transparency, configurable font, different window styles

参考：

[1]. [http://www.v2ex.com/t/78747](http://www.v2ex.com/t/78747#reply9)

![]()

### 本文作者：[itfanr](http://itfanr.duapp.com/?author=1 "查看itfanr所有文章")
分享到：[]( "分享到新浪微博")[]( "分享到QQ空间")[]( "分享到腾讯微博")[]( "分享到人人网")[]( "分享到开心网")[]( "分享到复制网址")更多[0]( "累计分享0次")

继续查看有关 [console2](http://itfanr.duapp.com/?tag=console2)，[ez_setup](http://itfanr.duapp.com/?tag=ez_setup)，[ipython](http://itfanr.duapp.com/?tag=ipython)，[pip](http://itfanr.duapp.com/?tag=pip)，[python](http://itfanr.duapp.com/?tag=python-2)，[windows](http://itfanr.duapp.com/?tag=windows) 的文章

### 与本文相关的文章

* 暂无相关文章！
 []()

[喜欢取消喜欢]()

[最新]()[最早]()[最热]()

* [1条评论]()

* [![_moxie_]()](http://weibo.com/sovey "_moxie_")

[_moxie_](http://weibo.com/sovey)

ipython原来是这么个东西啊，我也装个试试

8月14日[回复]()[顶]()[转发]()[举报]()
[1]()[]()

社交帐号登录:
* [微博](http://wp-duapp.duoshuo.com/login/weibo/?sso=1&redirect_uri=http%3A%2F%2Fitfanr.duapp.com%2Fwp-login.php%3Faction%3Dduoshuo_login&redirect_to%3Dhttp%3A%2F%2Fitfanr.duapp.com%2F%3Fp%3D343)
* [QQ](http://wp-duapp.duoshuo.com/login/qq/?sso=1&redirect_uri=http%3A%2F%2Fitfanr.duapp.com%2Fwp-login.php%3Faction%3Dduoshuo_login&redirect_to%3Dhttp%3A%2F%2Fitfanr.duapp.com%2F%3Fp%3D343)
* [人人](http://wp-duapp.duoshuo.com/login/renren/?sso=1&redirect_uri=http%3A%2F%2Fitfanr.duapp.com%2Fwp-login.php%3Faction%3Dduoshuo_login&redirect_to%3Dhttp%3A%2F%2Fitfanr.duapp.com%2F%3Fp%3D343)
* [豆瓣](http://wp-duapp.duoshuo.com/login/douban/?sso=1&redirect_uri=http%3A%2F%2Fitfanr.duapp.com%2Fwp-login.php%3Faction%3Dduoshuo_login&redirect_to%3Dhttp%3A%2F%2Fitfanr.duapp.com%2F%3Fp%3D343)
* [更多»]()

* [开心](http://wp-duapp.duoshuo.com/login/kaixin/?sso=1&redirect_uri=http%3A%2F%2Fitfanr.duapp.com%2Fwp-login.php%3Faction%3Dduoshuo_login&redirect_to%3Dhttp%3A%2F%2Fitfanr.duapp.com%2F%3Fp%3D343)
* [网易](http://wp-duapp.duoshuo.com/login/netease/?sso=1&redirect_uri=http%3A%2F%2Fitfanr.duapp.com%2Fwp-login.php%3Faction%3Dduoshuo_login&redirect_to%3Dhttp%3A%2F%2Fitfanr.duapp.com%2F%3Fp%3D343)
* [搜狐](http://wp-duapp.duoshuo.com/login/sohu/?sso=1&redirect_uri=http%3A%2F%2Fitfanr.duapp.com%2Fwp-login.php%3Faction%3Dduoshuo_login&redirect_to%3Dhttp%3A%2F%2Fitfanr.duapp.com%2F%3Fp%3D343)
* [百度](http://wp-duapp.duoshuo.com/login/baidu/?sso=1&redirect_uri=http%3A%2F%2Fitfanr.duapp.com%2Fwp-login.php%3Faction%3Dduoshuo_login&redirect_to%3Dhttp%3A%2F%2Fitfanr.duapp.com%2F%3Fp%3D343)
* [谷歌](http://wp-duapp.duoshuo.com/login/google/?sso=1&redirect_uri=http%3A%2F%2Fitfanr.duapp.com%2Fwp-login.php%3Faction%3Dduoshuo_login&redirect_to%3Dhttp%3A%2F%2Fitfanr.duapp.com%2F%3Fp%3D343)
[![]()]()

发布

[]( "插入表情")

[弧光守望者-itfanr正在使用多说](http://duoshuo.com/)
1. [_moxie_](http://weibo.com/sovey) on [2013 年 8 月 14 日 at 10:17]() said:

ipython原来是这么个东西啊，我也装个试试

### 聚合文章

* [java字节流和字符流](http://itfanr.duapp.com/?p=364)
* [七牛上传文件小工具v0.1](http://itfanr.duapp.com/?p=356)
* [C#解析json数据](http://itfanr.duapp.com/?p=354)
* [windows下安装ipython](http://itfanr.duapp.com/?p=343)
* [专业的Windows鼠标右键菜单管理工具](http://itfanr.duapp.com/?p=341)
* [ubuntu编译安装vim7.4](http://itfanr.duapp.com/?p=337)
### 活跃读者

* ![]()

3+[_moxie_](http://weibo.com/sovey)
* ![]()

1+[疯狂的眼球](http://weibo.com/ailvme)
* ![]()

1+[summving](http://weibo.com/lordson)

### 最新评论

* [*>*![]()**summving：**学习了](http://itfanr.duapp.com/?p=316#comment-15 " python中的__name__ == ")
* [*>*![]()**_moxie_：**ipython原来是这么个东西啊，我也装个试试]( "windows下安装ipython上的评论")
* [*>*![]()**_moxie_：**我还没看，留存了。。](http://itfanr.duapp.com/?p=300#comment-13 "Viterbi算法上的评论")
* [*>*![]()**疯狂的眼球：**又一位代码大神~~](http://itfanr.duapp.com/?p=291#comment-10 "C语言利用中心极限定理产生高斯白噪声上的评论")
* [*>*![]()**_moxie_：**不错不错，感谢推荐](http://itfanr.duapp.com/?p=300#comment-9 "Viterbi算法上的评论")
* [*>*![]()**_moxie_：**嗯，花钱不？](http://itfanr.duapp.com/?p=105#comment-7 "我放弃在wordpress使用wordpress markdown插件上的评论")
  

© 2013 [弧光守望者-itfanr](http://itfanr.duapp.com/) 版权所有.    Theme [D7-Simple](http://www.kilvn.com/d7-simple/) & [大前端](http://www.daqianduan.com/).     由[Wordpress](http://cn.wordpress.org/)内核驱动.

[]( "回顶部")[]( "发评论")
