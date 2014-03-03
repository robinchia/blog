---
layout: post
title: "Oracle 连接错误;ORA-27101 shared memory realm does not"
categories: DB
tags: 
 - DB
 - oracle
 - oracle-
--- 

# Oracle 连接错误;ORA-27101 shared memory realm does not exist讨论第2页 - PostgreSQL - Tech - ITeye论坛

[您还未登录 !](http://www.iteye.com/login "登录") [我的应用](http://www.iteye.com/all) [登录](http://www.iteye.com/login) [注册](http://www.iteye.com/signup)

[![ITeye-最棒的软件开发交流社区]( "ITeye-最棒的软件开发交流社区")](http://www.iteye.com/)

[论坛首页](http://www.iteye.com/forums) → [综合技术版](http://www.iteye.com/forums/board/Tech) → [PostgreSQL](http://www.iteye.com/forums/tag/PostgreSQL) →

# Oracle 连接错误;ORA-27101: shared memory realm does not exist

[全部](http://www.iteye.com/forums/board/Tech) [Database](http://www.iteye.com/forums/tag/Database) [Haskell](http://www.iteye.com/forums/tag/Haskell) [Erlang](http://www.iteye.com/forums/tag/Erlang) [FP](http://www.iteye.com/forums/tag/FP) [Linux](http://www.iteye.com/forums/tag/Linux) [数据结构和算法](http://www.iteye.com/forums/tag/algorithm) [mysql](http://www.iteye.com/forums/tag/mysql) [oracle](http://www.iteye.com/forums/tag/OracleDB) [DB2](http://www.iteye.com/forums/tag/DB2) [SQLServer](http://www.iteye.com/forums/tag/SQLServer) [PostgreSQL](http://www.iteye.com/forums/tag/PostgreSQL) [MacOSX](http://www.iteye.com/forums/tag/MacOSX) [Unix](http://www.iteye.com/forums/tag/Unix) [编程综合](http://www.iteye.com/forums/tag/Technology) [OS](http://www.iteye.com/forums/tag/OS)
[](http://www.iteye.com/forums/43/topics/774091/posts/new "发表回复")

[最成熟稳定甘特图控件,支持Java和.Net](http://www.iteye.com/clicks/593)

[« 上一页](http://www.iteye.com/topic/774091?page=1) [1](http://www.iteye.com/topic/774091?page=1) 2 下一页 »
浏览 7550 次
 [主题：Oracle 连接错误;ORA-27101: shared memory realm does not exist](http://www.iteye.com/topic/774091)

精华帖 (0) :: 良好帖 (0) :: 新手帖 (0) :: 隐藏帖 (0) 作者 正文 * someone
* 等级: ![二星会员]( "二星会员")
* [![someone的博客]( "someone的博客: someone")](http://someone.iteye.com/)
* 性别: ![]( "男")
* 文章: 179
* 积分: 180
* 来自: 中国
* ![]()
    发表时间：2010-09-30  

[<](http://www.iteye.com/topic/774091?page=2#) [>](http://www.iteye.com/topic/774091?page=2#) 猎头职位:好多年前也遇到过这个错误，当时的解决办法是
把oracle的服务的登录用户改为某个域用户（这个域用户在该机器的管理员组），
然后重新启动就可以了。
根本原因未知。 [返回顶楼](http://www.iteye.com/topic/774091?page=2#) [ ](http://someone.iteye.com/ "浏览作者的博客") [ ](http://someone.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=someone "发送站内短信") [ ](http://someone.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=someone "关注作者") [回帖地址](http://www.iteye.com/topic/774091?page=2#1694858 "someone回帖:Oracle 连接错误;ORA-27101: shared memory realm does not exist")

[0](http://www.iteye.com/topic/774091?page=2# "好") [0](http://www.iteye.com/topic/774091?page=2# "差") 请登录后投票  * TeddyWang
* 等级: 初级会员
* [![TeddyWang的博客]( "TeddyWang的博客: 哦嘛咪嘛咪哄~~~")](http://teddywang.iteye.com/)
* 性别: ![]( "男")
* 文章: 76
* 积分: 30
* 来自: 深圳
* ![]()
    发表时间：2010-09-30  

原因是没有那个instance，好好确认一下PATH、ORACLE_SID、ORACLE_HOME的値 [返回顶楼](http://www.iteye.com/topic/774091?page=2#) [ ](http://teddywang.iteye.com/ "浏览作者的博客") [ ](http://teddywang.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=TeddyWang "发送站内短信") [ ](http://teddywang.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=TeddyWang "关注作者") [回帖地址](http://www.iteye.com/topic/774091?page=2#1695121 "TeddyWang回帖:Oracle 连接错误;ORA-27101: shared memory realm does not exist")

[0](http://www.iteye.com/topic/774091?page=2# "好") [0](http://www.iteye.com/topic/774091?page=2# "差") 请登录后投票  * zgdhj95
* 等级: 初级会员
* [![zgdhj95的博客]( "zgdhj95的博客: zgdhj95")](http://zgdhj95.iteye.com/)
* 文章: 21
* 积分: 52
* 来自: ...
* ![]()
    发表时间：2010-10-05  

用下面的法子应该可以：
a) 开始-运行，输入 sqlplus /nolog，回车
b) 输入 conn sys/password@orcl as sysdba，回车
c) 输入startup 回车
然后等数据库启动结束，就应该好了。 [返回顶楼](http://www.iteye.com/topic/774091?page=2#) [ ](http://zgdhj95.iteye.com/ "浏览作者的博客") [ ](http://zgdhj95.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=zgdhj95 "发送站内短信") [ ](http://zgdhj95.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=zgdhj95 "关注作者") [回帖地址](http://www.iteye.com/topic/774091?page=2#1697197 "zgdhj95回帖:Oracle 连接错误;ORA-27101: shared memory realm does not exist")

[0](http://www.iteye.com/topic/774091?page=2# "好") [0](http://www.iteye.com/topic/774091?page=2# "差") 请登录后投票  * kzwang
* 等级: 初级会员
* [![kzwang的博客]( "kzwang的博客: kzwang")](http://kzwang.iteye.com/)
* 性别: ![]( "男")
* 文章: 2
* 积分: 60
* 来自: 乌鲁木齐
* ![]()
    发表时间：2010-10-07  

数据库没有启动
startup
就好了 [返回顶楼](http://www.iteye.com/topic/774091?page=2#) [ ](http://kzwang.iteye.com/ "浏览作者的博客") [ ](http://kzwang.iteye.com/blog/profile "浏览作者资料") [ ](http://app.iteye.com/messages/new?message%5Breceiver_name%5D=kzwang "发送站内短信") [ ](http://kzwang.iteye.com/blog/guest_book "给作者留言") [ ](http://app.iteye.com/feed?subscription%5Bsubscribed_user_name%5D=kzwang "关注作者") [回帖地址](http://www.iteye.com/topic/774091?page=2#1698425 "kzwang回帖:Oracle 连接错误;ORA-27101: shared memory realm does not exist")

[0](http://www.iteye.com/topic/774091?page=2# "好") [0](http://www.iteye.com/topic/774091?page=2# "差") 请登录后投票

[](http://www.iteye.com/forums/43/topics/774091/posts/new "发表回复")

[« 上一页](http://www.iteye.com/topic/774091?page=1) [1](http://www.iteye.com/topic/774091?page=1) 2 下一页 »
[论坛首页](http://www.iteye.com/forums) → [综合技术版](http://www.iteye.com/forums/board/Tech) → [PostgreSQL](http://www.iteye.com/forums/tag/PostgreSQL)
跳转论坛:Java编程和Java企业应用 Web前端技术 移动编程和手机应用开发 C/C++编程 Ruby编程 Python编程 PHP编程 Flash编程和RIA Microsoft .Net 综合技术 软件开发和项目管理 行业应用 入门讨论 招聘求职 海阔天空

* [北京: 国政通诚聘互联网软件开发工程师](http://www.iteye.com/jobs/1889 "北京:国政通诚聘互联网软件开发工程师")
* [北京: ThoughtWorks诚聘Architect](http://www.iteye.com/jobs/1139 "北京:ThoughtWorks诚聘Architect")
* [浙江: CSST研究院杭州分院诚聘研究员](http://www.iteye.com/jobs/1213 "浙江:CSST研究院杭州分院诚聘研究员")
* [浙江: CSST研究院杭州分院诚聘Erlang开发工程师/部](http://www.iteye.com/jobs/1209 "浙江:CSST研究院杭州分院诚聘Erlang开发工程师/部门经理")

* [首页](http://www.iteye.com/)
* [资讯](http://www.iteye.com/news)
* [论坛](http://www.iteye.com/forums)
* [问答](http://www.iteye.com/ask)
* [博客](http://www.iteye.com/blogs)
* [群组](http://www.iteye.com/groups)
* [招聘](http://www.iteye.com/job)
* [搜索](http://www.iteye.com/search)
* [招贤纳士](http://www.iteye.com/index/recruit)
* [广告服务](http://www.iteye.com/index/service)
* [ITeye黑板报](http://webmaster.iteye.com/)
* [联系我们](http://www.iteye.com/index/contactus)
* [友情链接](http://www.iteye.com/index/friend_links)

© 2003-2011 ITeye.com. [ [京ICP证110151号](http://www.miibeian.gov.cn/) ]
