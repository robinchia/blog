---
layout: post
title: "配置AD、CA、SSL，绑定keystore"
categories: 域
tags: 
 - 域
--- 

# 配置AD、CA、SSL，绑定keystore

# [风林火山](http://blog.csdn.net/freewind88)

## 其疾如风，其徐如林，侵掠如火，不动如山，难知如阴，动如雷震 ——《孙子兵法》

* [条新通知](http://hi.csdn.net/space-notice.html)
* [登录](http://passport.csdn.net/UserLogin.aspx)
* [注册](http://passport.csdn.net/CSDNUserRegister.aspx)
* [欢迎](http://hi.csdn.net/)
* [退出](http://writeblog.csdn.net/Signout.aspx)
* [我的博客](http://blog.csdn.net/)
* [配置](http://writeblog.csdn.net/configure.aspx)
* [写文章](http://writeblog.csdn.net/PostEdit.aspx)
* [文章管理](http://writeblog.csdn.net/PostList.aspx)
* [博客首页](http://blog.csdn.net/)

*
* 全站 当前博客
*

* [空间](http://hi.csdn.net/freewind88)
* [博客](http://blog.csdn.net/freewind88)
* [好友](http://hi.csdn.net/!s/friend/list/freewind88)
* [相册](http://hi.csdn.net/!s/album/list/freewind88)
* [留言](http://hi.csdn.net/!s/wall/to/freewind88)

用户操作[[留言]](http://hi.csdn.net/!s/wall/to/freewind88)  [[发消息]](http://hi.csdn.net/!s/msg/to/freewind88)  [[加为好友]](http://hi.csdn.net/!s/friend/add/freewind88) 订阅我的博客[![XML聚合]()](http://feeds.feedsky.com/csdn.net/freewind88)   [![FeedSky]()](http://feeds.feedsky.com/csdn.net/freewind88)[![订阅到鲜果]()](http://www.xianguo.com/subscribe.php?url=http://feeds.feedsky.com/csdn.net/freewind88)[![订阅到Google]()](http://fusion.google.com/add?feedurl=http://feeds.feedsky.com/csdn.net/freewind88)[![订阅到抓虾]()](http://www.zhuaxia.com/add_channel.php?url=http://feeds.feedsky.com/csdn.net/freewind88)[[编辑]](http://writeblog.csdn.net/configure.aspx)freewind88的公告自己在Computer Science方面的所学，与大家一起交流[[编辑]](http://writeblog.csdn.net/EditCategories.aspx?catID=1)文章分类* [![(RSS)]()](http://blog.csdn.net/freewind88/category/237967.aspx/rss)[C/C++](http://blog.csdn.net/freewind88/category/237967.aspx)
* [![(RSS)]()](http://blog.csdn.net/freewind88/category/221073.aspx/rss)[Delphi](http://blog.csdn.net/freewind88/category/221073.aspx)
* [![(RSS)]()](http://blog.csdn.net/freewind88/category/328710.aspx/rss)[Domino](http://blog.csdn.net/freewind88/category/328710.aspx)
* [![(RSS)]()](http://blog.csdn.net/freewind88/category/12650.aspx/rss)[Java](http://blog.csdn.net/freewind88/category/12650.aspx)
* [![(RSS)]()](http://blog.csdn.net/freewind88/category/221072.aspx/rss)[Linux](http://blog.csdn.net/freewind88/category/221072.aspx)
* [![(RSS)]()](http://blog.csdn.net/freewind88/category/441413.aspx/rss)[Portlet](http://blog.csdn.net/freewind88/category/441413.aspx)
* [![(RSS)]()](http://blog.csdn.net/freewind88/category/238231.aspx/rss)[WebSphere](http://blog.csdn.net/freewind88/category/238231.aspx)
* [![(RSS)]()](http://blog.csdn.net/freewind88/category/328722.aspx/rss)[Windows](http://blog.csdn.net/freewind88/category/328722.aspx)
* [![(RSS)]()](http://blog.csdn.net/freewind88/category/12651.aspx/rss)[软件工程](http://blog.csdn.net/freewind88/category/12651.aspx)
* [![(RSS)]()](http://blog.csdn.net/freewind88/category/237966.aspx/rss)[数据库](http://blog.csdn.net/freewind88/category/237966.aspx)
* [![(RSS)]()](http://blog.csdn.net/freewind88/category/214719.aspx/rss)[网格计算](http://blog.csdn.net/freewind88/category/214719.aspx)
* [![(RSS)]()](http://blog.csdn.net/freewind88/category/272363.aspx/rss)[移动开发](http://blog.csdn.net/freewind88/category/272363.aspx)存档* [2008年08月(1)](http://blog.csdn.net/freewind88/archive/2008/08.aspx)
* [2008年07月(2)](http://blog.csdn.net/freewind88/archive/2008/07.aspx)
* [2007年10月(1)](http://blog.csdn.net/freewind88/archive/2007/10.aspx)
* [2007年09月(4)](http://blog.csdn.net/freewind88/archive/2007/09.aspx)
* [2007年08月(3)](http://blog.csdn.net/freewind88/archive/2007/08.aspx)
* [2007年01月(5)](http://blog.csdn.net/freewind88/archive/2007/01.aspx)
* [2006年11月(2)](http://blog.csdn.net/freewind88/archive/2006/11.aspx)
* [2006年10月(1)](http://blog.csdn.net/freewind88/archive/2006/10.aspx)
* [2006年09月(3)](http://blog.csdn.net/freewind88/archive/2006/09.aspx)
* [2006年08月(1)](http://blog.csdn.net/freewind88/archive/2006/08.aspx)
* [2006年07月(1)](http://blog.csdn.net/freewind88/archive/2006/07.aspx)
* [2006年06月(2)](http://blog.csdn.net/freewind88/archive/2006/06.aspx)
* [2004年05月(5)](http://blog.csdn.net/freewind88/archive/2004/05.aspx)
# ![原创]()  配置AD、CA、SSL，绑定keystore [收藏]( "收藏到我的网摘中，并分享给我的朋友")

在这就简单的介绍一下配置过程，未提到的设置基本就都采用默认即可。
1）安装AD:
开始 -> 运行 -> dcpromote
域名: testad.com.cn
NT域名: ldap
即 Fully Qualified Domain Name (FQDN) 为 ldap.testad.com.cn
注意，一定要先安装 IIS , 再安装 CA.
2）安装 IIS:
开始-> 程序 -> 管理工具 -> 配置您的服务器向导 -> 应用服务器 (IIS, ASP.NET)
进入 http:// ldap.testad.com.cn /iisstart.htm 表示安装成功.
3）安装CA:
开始-> 设置 -> 控制面板-> 添加或删除程序 ->添加/删除Windows组件 -> 证书服务
选择 企业根CA
共用名称 CA: testca
进入 http:// ldap.testad.com.cn /CertSrv 表示安装成功.
4）生成证书请求:
开始->程序->管理工具-> Internet 信息服务 (IIS) 管理器 -> Internet信息服务-> (本地计算机) -> 网站
->  右键点选 默认网站 -> 属性 ->选择 "目录安全性" -> 服务器证书
->新建证书 -> 准备证书，但稍后发送
公共名称最好设置为 ldap.testad.com.cn, 这是给使用者连ssl 的 站点.
最后产生证书请求文件 , 默认为c:\certreq.txt
5）在CA上请求证书:
进入 http:// ldap.testad.com.cn /CertSrv
按 申请一个证书 -> 高级证书申请
-> 使用 base64 编码的 CMC 或 PKCS #10 文件提交一个证书申请，或使用 base64 编码的 PKCS #7 文件续订证书申请。
使用 记事本 打开 c:\certreq.txt , copy c:\certreq.txt 内容贴至 保存的申请:
证书模板 选择 Web 服务器, 按 提交
然后点选 下载证书 , 将 certnew.cer 储存至 c:\certnew.cer
6）安装证书:
开始->程序->管理工具-> Internet 信息服务 (IIS) 管理器 -> Internet信息服务-> (本地计算机) -> 网站
->  右键点选默认网站->属性 -> 选择 "目录安全性" ->服务器证书
->处理挂起的请求，安装证书 -> 路径和文件名: c:\certnew.cer
网站SSL 端口: 443
7）将 CA 证书 加入至keystore 里:
进入 http:// ldap.testad.com.cn /CertSrv
点选 下载一个 CA 证书，证书链或 CRL
点选 下载 CA 证书, 然后下载并改名为 c:\ca_cert.cer
安装CA后LDAP服务器C盘根目录会生成一文件ldap.testad.com.cn_testca.crt
然后执行 命令:
keytool -import -keystore "c:/testca.keystore" -file "ldap.testad.com.cn_testca.crt" -storepass "changeit"
keytool -import -keystore "c:/testca.keystore" -alias mkey -file "c:/ca_cert.cer" -storepass "changeit"
出现 Trusted this certificate? 按 "y" 即新增成功.
但经过个人的测试，其实只需使用ldap.testad.com.cn_testca.crt这个证书就可以连接上SSL AD 636。

发表于 @ 2007年08月17日　14:29:00 | [评论( loading...  )](http://blog.csdn.net/freewind88/archive/2007/08/17/1748225.aspx#FeedBack "评论")| [编辑](http://writeblog.csdn.net/PostEdit.aspx?entryId=1748225 "编辑")| [举报](mailto:webmaster@csdn.net?subject=Article Report!!!&body=Author:freewind88URL:http://blog.csdn.net/ArticleContent.aspx?UserName=freewind88&Entryid=1748225)| [收藏]( "收藏到我的网摘中，并分享给我的朋友")

### [旧一篇:Java添加、修改MS AD用户密码](http://blog.csdn.net/freewind88/archive/2007/08/16/1746349.aspx) | [新一篇:在Domino中修改AD密码](http://blog.csdn.net/freewind88/archive/2007/08/18/1749624.aspx)

[查看最新精华文章 请访问博客首页](http://blog.csdn.net/)相关文章[]()

* 发表评论
 
* 表 情：
* [![顶]( "顶")](http://blog.csdn.net/freewind88/archive/2007/08/17/1748225.aspx#) [![砸]( "砸")](http://blog.csdn.net/freewind88/archive/2007/08/17/1748225.aspx#) [![棒]( "棒")](http://blog.csdn.net/freewind88/archive/2007/08/17/1748225.aspx#) [![大笑]( "大笑")](http://blog.csdn.net/freewind88/archive/2007/08/17/1748225.aspx#) [![愤怒]( "愤怒")](http://blog.csdn.net/freewind88/archive/2007/08/17/1748225.aspx#) [![大哭]( "大哭")](http://blog.csdn.net/freewind88/archive/2007/08/17/1748225.aspx#) [![疑问]( "疑问")](http://blog.csdn.net/freewind88/archive/2007/08/17/1748225.aspx#) [![汗]( "汗")](http://blog.csdn.net/freewind88/archive/2007/08/17/1748225.aspx#) [![呕吐]( "呕吐")](http://blog.csdn.net/freewind88/archive/2007/08/17/1748225.aspx#) [![送花]( "送花")](http://blog.csdn.net/freewind88/archive/2007/08/17/1748225.aspx#)

* 评论内容：
*
* 用 户 名：
* [登录]() [注册](http://passport.csdn.net/CSDNUserRegister.aspx) 匿名评论

* 验 证 码：
* [![验证码]()]() [重新获得验证码]()

*
*![]()
