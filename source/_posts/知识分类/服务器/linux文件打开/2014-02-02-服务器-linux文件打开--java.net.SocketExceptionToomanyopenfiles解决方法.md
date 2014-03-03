---
layout: post
title: "java.net.SocketException  Too many open files解决方法 "
categories: 服务器
tags: 
 - 服务器
 - linux文件打开
--- 

# java.net.SocketException Too many open files解决方法 糊涂虫

[](http://www.hutud.com/ "首  页")

* [关于我](http://www.hutud.com/about-me)

[]()

[](http://www.hutud.com/ "糊涂虫")

[登录]()

登录
  下次自动登录
夜深了!  2012年2月22日 星期三
[![潍坊oa-中硕为中小企业信息化提供解决方案]()](http://www.hutud.com/archives/101 "潍坊oa-中硕为中小企业信息化提供解决方案")

[详细内容](http://www.hutud.com/archives/101 "潍坊oa-中硕为中小企业信息化提供解决方案")
## [潍坊oa-中硕为中小企业信息化提供](http://www.hutud.com/archives/101 "Permalink to 潍坊oa-中硕为中小企业信息化提供解决方案")

[![CSS隐藏文字的方法]()](http://www.hutud.com/archives/97 "CSS隐藏文字的方法")

[详细内容](http://www.hutud.com/archives/97 "CSS隐藏文字的方法")
## [CSS隐藏文字的方法](http://www.hutud.com/archives/97 "Permalink to CSS隐藏文字的方法")
[![PHP magic_quotes_gpc的详细使用方法]()](http://www.hutud.com/archives/92 "PHP magic_quotes_gpc的详细使用方法")

[详细内容](http://www.hutud.com/archives/92 "PHP magic_quotes_gpc的详细使用方法")
## [PHP magic_quotes_gpc的详细使](http://www.hutud.com/archives/92 "Permalink to PHP magic_quotes_gpc的详细使用方法")

[![phpweb修改分页信息，去掉 右下角的 首页 尾页信息]()](http://www.hutud.com/archives/82 "phpweb修改分页信息，去掉 右下角的 首页 尾页信息")

[详细内容](http://www.hutud.com/archives/82 "phpweb修改分页信息，去掉 右下角的 首页 尾页信息")
## [phpweb修改分页信息，去掉 右下角](http://www.hutud.com/archives/82 "Permalink to phpweb修改分页信息，去掉 右下角的 首页 尾页信息")

[]( "返回顶部") []( "查看留言") []( "转到底部")
现在的位置: [首页](http://www.hutud.com/ "返回首页") ＞[linux](http://www.hutud.com/archives/category/linux "查看 linux 中的全部文章"), [lnmp](http://www.hutud.com/archives/category/lnmp "查看 lnmp 中的全部文章")＞正文

[RSS](http://feed.feedsky.com/zmingcx "RSS")
[小]() [中]() [大]()

[上篇](http://www.hutud.com/archives/21) [下篇](http://www.hutud.com/archives/35)

java.net.SocketException: Too many open files解决方法

2011年12月06日  ⁄ [linux](http://www.hutud.com/archives/category/linux "查看 linux 中的全部文章"), [lnmp](http://www.hutud.com/archives/category/lnmp "查看 lnmp 中的全部文章")  ⁄ [暂无评论](http://www.hutud.com/archives/8#respond "《java.net.SocketException: Too many open files解决方法》上的评论")
最近随着网站访问量的提高把web服务器移到linux下了，在移服务器的第二天，tomcat频繁的报

java.net.SocketException: Too many open files错误，错误日志达到了100多兆，郁闷了，windows上运行了很长

时间都没出现这个错误，后来才知道linux对进程的打开文件数是有限制的。

用命令ulimit -a查看

[root@test security]# ulimit -a
core file size        (blocks, -c) 0
data seg size         (kbytes, -d) unlimited
file size             (blocks, -f) unlimited
max locked memory     (kbytes, -l) unlimited
max memory size       (kbytes, -m) unlimited
open files (-n) 1024
pipe size          (512 bytes, -p) 8
stack size            (kbytes, -s) 8192
cpu time             (seconds, -t) unlimited
max user processes            (-u) 7168
virtual memory        (kbytes, -v) unlimited
[root@test security]#
通过以上命令，我们可以看到open files 的最大数为1024

对于并发量比较大的网站这个限制是有些捉襟见肘的，所以我通过这个命令

ulimit -n 4096
把打开文件数的上限设为了4096，这下好了，项目又稳定了

没想到过两天后又重新出这个错误了，郁闷，两个小时报一次，报之后就挂掉了

在重新用ulimit -a查看，发现open files (-n) 1024 又变回了1024了，

报这个错误就在我那次登陆更新之后又报的，原来ulimit -n 4096 命令只能临时的改变open files 的值，当

重新登陆后又会恢复，所以需要永久设置open files 的值才行啊，
用ulimit -n 修改open files 总是不能保持。所以用下面一个简单的办法更好些。修改/etc/security/limits.conf 添加如下一行：

* - nofile 1006154

修改/etc/pam.d/login添加如下一行

session required /lib/security/pam_limits.so

这次永久修改后程序就再没那个问题了，一直稳定运行。

另外遇到这个问题这后还需要检查我们的程序对于操作io的流是否在操作完之后关闭，这才是从最更本上的解决。

 

 

-----------------------------------------------------------------------------------

linux 上tomcat 服务器抛出socket异常“文件打开太多”的问题
java.net.SocketException: Too many open files
at java.net.PlainSocketImpl.socketAccept(Native Method)
at java.net.PlainSocketImpl.accept(PlainSocketImpl.java:384)
at java.net.ServerSocket.implAccept(ServerSocket.java:450)
at java.net.ServerSocket.accept(ServerSocket.java:421)
at org.apache.tomcat.util.net.DefaultServerSocketFactory.acceptSocket(DefaultServerSocketFactory.java:60)
at org.apache.tomcat.util.net.PoolTcpEndpoint.acceptSocket(PoolTcpEndpoint.java:407)
at org.apache.tomcat.util.net.LeaderFollowerWorkerThread.runIt(LeaderFollowerWorkerThread.java:70)
at org.apache.tomcat.util.threads.ThreadPool$ControlRunnable.run(ThreadPool.java:684)
at java.lang.Thread.run(Thread.java:595)

原本以为是tomcat的配置或是应用本身的问题，"谷歌"一把后才发现，该问题的根本原因是由于系统文件资源的限制导致的。

具体可以参考[http://www.bea.com.cn/support_pattern/Too_Many_Open_Files_Pattern.html](http://www.bea.com.cn/support_pattern/Too_Many_Open_Files_Pattern.html)
的说明。具体的解决方式可以参考一下：
1。ulimit -a 查看系统目前资源限制的设定。
   [root@test security]# umlimit -a
-bash: umlimit: command not found
[root@test security]# ulimit -a
core file size        (blocks, -c) 0
data seg size         (kbytes, -d) unlimited
file size             (blocks, -f) unlimited
max locked memory     (kbytes, -l) unlimited
max memory size       (kbytes, -m) unlimited
open files                    (-n) 1024
pipe size          (512 bytes, -p) 8
stack size            (kbytes, -s) 8192
cpu time             (seconds, -t) unlimited
max user processes            (-u) 7168
virtual memory        (kbytes, -v) unlimited
[root@test security]#
通过以上命令，我们可以看到open files 的最大数为1024
那么我们可以通过一下命令修改该参数的最大值
2. ulimit -n 4096
[root@test security]# ulimit -n 4096
[root@test security]# ulimit -a
core file size        (blocks, -c) 0
data seg size         (kbytes, -d) unlimited
file size             (blocks, -f) unlimited
max locked memory     (kbytes, -l) unlimited
max memory size       (kbytes, -m) unlimited
open files                    (-n) 4096
pipe size          (512 bytes, -p) 8
stack size            (kbytes, -s) 8192
cpu time             (seconds, -t) unlimited
max user processes            (-u) 7168
virtual memory        (kbytes, -v) unlimited

这样我们就修改了系统在同一时间打开文件资源的最大数，基本解决以上问题。

以上部分是查找网络上的解决方法。设置了之后段时间内有作用。

后来仔细想来，问题还是要从根本上解决，于是把以前的代码由认真地看了一遍。终于找到了，罪魁祸首。

在读取文件时，有一些使用的BufferedReader 没有关闭。导致文件一直处于打开状态。造成资源的严重浪费。

修改之后的简单代码如下：
public void test(){
    BufferedReader reader =null;
    try{
        reader = 读取文件;
        String line = "";
        while( ( ine=reader.readLine())!=null){
           其他操作
        }

    } catch (IOException e){
        System.out.println(e);
    } finally{  
         if(reader !=null){
                try {
                    reader.close();
                } catch (IOException e) {
                      e.printStackTrace();
                }
          }
    }

}

 

[返回]()
![]()

### 作者: [糊涂虫](http://www.hutud.com/archives/author/admin "由 糊涂虫 发布")
  该日志由 糊涂虫 于79 天前发表在[linux](http://www.hutud.com/archives/category/linux "查看 linux 中的全部文章"), [lnmp](http://www.hutud.com/archives/category/lnmp "查看 lnmp 中的全部文章")分类下， 你可以[发表评论](http://www.hutud.com/archives/8#respond)，并在保留[原文地址]()及作者的情况下[引用](http://www.hutud.com/archives/8/trackback)到你的网站或博客。
转载请注明: [java.net.SocketException: Too many open files解决方法 | 糊涂虫]( "本文固定链接 http://www.hutud.com/archives/8")[+复制链接](http://www.hutud.com/archives/8#)

【上篇】[Linux系统利用Crontab命令实现定时重启](http://www.hutud.com/archives/21)
【下篇】[中小型的软件公司发展模式总结](http://www.hutud.com/archives/10)
### 您可能还会对这些文章感兴趣！

1. [linux 挂装u盘](http://www.hutud.com/archives/53)
1. [linux干掉进程的方法](http://www.hutud.com/archives/35)
1. [Linux系统利用Crontab命令实现定时重启](http://www.hutud.com/archives/21)

[![中小型的软件公司发展模式总结]()](http://www.hutud.com/archives/10 "中小型的软件公司发展模式总结")

[![变成了有车一族了，纪念一下 (2011-06-04 17:03:55)]()](http://www.hutud.com/archives/72 "变成了有车一族了，纪念一下 (2011-06-04 17:03:55)")
[![PHP magic_quotes_gpc的详细使用方法]()](http://www.hutud.com/archives/92 "PHP magic_quotes_gpc的详细使用方法")

[![phpweb修改分页信息，去掉 右下角的 首页 尾页信息]()](http://www.hutud.com/archives/82 "phpweb修改分页信息，去掉 右下角的 首页 尾页信息")

### 给我留言

[点击这里取消回复。](http://www.hutud.com/archives/8#respond) [留言无头像?](http://en.gravatar.com/ "去申请一个自己的Gravatar全球通用头像")

昵称 *

邮箱 *

网址

正在提交, 请稍候...

#

[![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]() [![]()]()
[插入图片]()

![使用新浪微博登陆]()

有人回复时邮件通知我

* [最新文章](http://www.hutud.com/archives/8#tab-widget1)
* [本月排行](http://www.hutud.com/archives/8#tab-widget2)
* [分类目录](http://www.hutud.com/archives/8#tab-widget3)
* [潍坊oa-中硕为中小企业信息化提供解](http://www.hutud.com/archives/101 "详细阅读 潍坊oa-中硕为中小企业信息化提供解决方案")
* [CSS隐藏文字的方法](http://www.hutud.com/archives/97 "详细阅读 CSS隐藏文字的方法")
* [PHP magic_quotes_gpc的详细使用](http://www.hutud.com/archives/92 "详细阅读 PHP magic_quotes_gpc的详细使用方法")
* [phpweb修改分页信息，去掉 右下角的](http://www.hutud.com/archives/82 "详细阅读 phpweb修改分页信息，去掉 右下角的 首页 尾页信息")
* [听故事，学人生](http://www.hutud.com/archives/79 "详细阅读 听故事，学人生")
* [变成了有车一族了，纪念一下 (2011-](http://www.hutud.com/archives/72 "详细阅读 变成了有车一族了，纪念一下 (2011-06-04 17:03:55)")
* [【程序人生】有时候错误只是种提醒](http://www.hutud.com/archives/66 "详细阅读 【程序人生】有时候错误只是种提醒")
* [Java打印程序设计全攻略(摘自：Ja](http://www.hutud.com/archives/63 "详细阅读 Java打印程序设计全攻略(摘自：Java研究组织）")
* [软件创新大赛有感](http://www.hutud.com/archives/56 "详细阅读 软件创新大赛有感")
* [linux 挂装u盘](http://www.hutud.com/archives/53 "详细阅读 linux 挂装u盘")

* [phpweb修改分页信息，去掉 右下角的](http://www.hutud.com/archives/82 "phpweb修改分页信息，去掉 右下角的 首页 尾页信息 (0条评论)")
* [linux干掉进程的方法](http://www.hutud.com/archives/35 "linux干掉进程的方法 (0条评论)")
* [phpweb修改 单页面的title](http://www.hutud.com/archives/38 "phpweb修改 单页面的title (0条评论)")
* [PHP magic_quotes_gpc的详细使用](http://www.hutud.com/archives/92 "PHP magic_quotes_gpc的详细使用方法 (0条评论)")
* [分析物流业务为何没有签合同成功](http://www.hutud.com/archives/41 "分析物流业务为何没有签合同成功 (0条评论)")
* [潍坊oa-中硕为中小企业信息化提供解](http://www.hutud.com/archives/101 "潍坊oa-中硕为中小企业信息化提供解决方案 (0条评论)")
* [Tomcat 下配置一个ip绑定多个域名(](http://www.hutud.com/archives/45 "Tomcat 下配置一个ip绑定多个域名(静态的) (0条评论)")
* [彼尔盖茨的十句话,绝对让你改变一生](http://www.hutud.com/archives/48 " 彼尔盖茨的十句话,绝对让你改变一生 (0条评论)")

* [discuz](http://www.hutud.com/archives/category/discuz "查看 discuz 下的所有文章")
* [java](http://www.hutud.com/archives/category/java "查看 java 下的所有文章")
* [linux](http://www.hutud.com/archives/category/linux "查看 linux 下的所有文章")
* [lnmp](http://www.hutud.com/archives/category/lnmp "查看 lnmp 下的所有文章")
* [mysql](http://www.hutud.com/archives/category/mysql "查看 mysql 下的所有文章")
* [php](http://www.hutud.com/archives/category/php "查看 php 下的所有文章")
* [phpweb](http://www.hutud.com/archives/category/phpweb "查看 phpweb 下的所有文章")
* [seo](http://www.hutud.com/archives/category/seo "查看 seo 下的所有文章")
* [创业路上](http://www.hutud.com/archives/category/business "发展过程中的经验 总结 与 弯路")
* [日志](http://www.hutud.com/archives/category/blogs "查看 日志 下的所有文章")

### 最活跃的读者

### 网站统计

日志总数：27篇

评论总数：11条

分类总数：14个

标签总数：0个

友情链接：1个

网站运行：210天
最后更新：2012年2月22日
* [关于我](http://www.hutud.com/about-me)

## [返回首页](http://www.hutud.com/ "糊涂虫")

Copyright © 2011-2012 糊涂虫  保留所有权利.   基于[WordPress](http://wordpress.org/ "WordPress.org") 技术创建   Theme by [Robin](http://zmingcx.com/)

[×]( "关闭")

[![]()](http://feed.feedsky.com/zmingcx "欢迎订阅本站")
[新浪微博]( "分享到新浪微博") [腾讯微博]( "分享到腾讯微博")
