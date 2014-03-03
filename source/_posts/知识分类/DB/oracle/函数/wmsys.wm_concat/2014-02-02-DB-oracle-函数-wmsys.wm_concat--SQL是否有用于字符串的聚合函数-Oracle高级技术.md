---
layout: post
title: "SQL是否有用于字符串的聚合函数 - Oracle   高级技术"
categories: DB
tags: 
 - DB
 - oracle
 - 函数
 - wmsys.wm_concat
--- 

# SQL是否有用于字符串的聚合函数 - Oracle 高级技术

您还未登录！|[登录](http://passport.csdn.net/UserLogin.aspx)|[注册](http://passport.csdn.net/CSDNUserRegister.aspx)|[帮助](http://passport.csdn.net/help/faq)

* [CSDN首页](http://www.csdn.net/)
* [资讯](http://news.csdn.net/)
* [论坛](http://bbs.csdn.net/)
* [博客](http://blog.csdn.net/)
* [下载](http://download.csdn.net/)
* [搜索](http://so.csdn.net/)
* 
## [更多]()

* [CTO俱乐部](http://cto.csdn.net/)
* [学生大本营](http://student.csdn.net/)
* [培训充电](http://edu.csdn.net/)
* [移动开发](http://mobile.csdn.net/)
* [软件研发](http://sd.csdn.net/)
* [云计算](http://cloud.csdn.net/)
* [程序员](http://www.programmer.com.cn/)
* [TUP](http://tup.csdn.net/)

[![]()](http://topic.csdn.net/u/20110919/11/40ca7c15-3d93-4150-8f44-1c7cf345a475.html#)

* []()
[CSDN](http://www.csdn.net/)-[CSDN社区](http://community.csdn.net/)-[Oracle](http://forum.csdn.net/BList/Oracle/)-[高级技术](http://forum.csdn.net/SList/Oracle_Technology//)

* [管理菜单]()

* [生成帖子](http://forum.csdn.net/PointForum/BuildTopic.aspx?topicId=40ca7c15-3d93-4150-8f44-1c7cf345a475&postDate=2011-09-19+11%3a17%3a53&return=http%3A%2F%2Ftopic.csdn.net%2Fu%2F20110919%2F11%2F40ca7c15-3d93-4150-8f44-1c7cf345a475.html)
* [置顶]()
* [推荐]()
* [取消推荐]()
* [锁定]()
* [解锁]()
* [移动]()
* [编辑]()
* [删除]()
* [帖子加分]()
* [帖子高亮]()
* [取消高亮]()
* [结  帖](http://forum.csdn.net/PointForum/Manage/TopicManageView.aspx?forumID=4d4e3fd6-f23d-4bf7-b10f-1fb66ca785fa&topicID=40ca7c15-3d93-4150-8f44-1c7cf345a475&date=2011-09-19+11%3a17%3a53&v=13)
* [发  帖](http://forum.csdn.net/PointForum/Forum/PostTopic.aspx?forumID=4d4e3fd6-f23d-4bf7-b10f-1fb66ca785fa)
* [回  复](http://topic.csdn.net/u/20110919/11/40ca7c15-3d93-4150-8f44-1c7cf345a475.html#r_achor)
# [收藏]() 不显示删除回复显示所有回复显示星级回复显示得分回复  []()SQL是否有用于字符串的聚合函数*[问题点数:40分]*

 * [![]()](http://hi.csdn.net/zhengyunQ)
* [zhengyunQ](http://hi.csdn.net/zhengyunQ)
* (zhengyunQ)
*
* 等　级：![]()
* 结帖率：100.00%
* *楼主*发表于：2011-09-19 11:17:53 设有SUM_STR()这样一个类似SUM()的聚合函数，功能如：
select sum_str(name,',')from class group by type;
可以把name用‘,’连接按type分组查出来。
有这样的聚合函数不？还有聚合函数是否可以自定义？
--by zhengyun * [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20110919/11/40ca7c15-3d93-4150-8f44-1c7cf345a475.html#top)
回复次数：4    * [![canhui87用户头像]( "canhui87用户自定义头像")](http://hi.csdn.net/canhui87)
* [canhui87](http://hi.csdn.net/canhui87)
* (canhui87)
*
* 等　级：![]()
*#1楼 得分：0回复于：2011-09-19 11:21:05 []() 搜一下sys_connect_by_path或者wmsys.wm_concat()* [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20110919/11/40ca7c15-3d93-4150-8f44-1c7cf345a475.html#top)
精华推荐：[收集网易数据库笔试题，欢迎大家拍砖](http://topic.csdn.net/u/20090302/18/83ca65bd-1abb-4800-abfc-cb6d6ccec5bd.html)  * [![jimmylin040用户头像]( "jimmylin040用户自定义头像")](http://hi.csdn.net/jimmylin040)
* [jimmylin040](http://hi.csdn.net/jimmylin040)
* (尘轻飞扬)
*
* 等　级：![]()
*#2楼 得分：0回复于：2011-09-19 11:22:10 []() 9i有一个函数stragg功能应该类似。
不过10g不能用。
10g的话，可以用connect by来做，例子
SQL codeselect t.code,substr(max(sys_connect_by_path(t.sz,',')),2) sz from (select code,sz,row_number()over(partition by code order by sz) rn from (select * from prdsz where rownum<100)) t start with t.rn=1 connect by t.code=prior t.code and t.rn-1=prior t.rn group by t.code;* [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20110919/11/40ca7c15-3d93-4150-8f44-1c7cf345a475.html#top)
精华推荐：[其他论坛Oracle版都是大版，火热的很，为何感觉csdn的Oracle版有些冷清？？？？](http://topic.csdn.net/u/20080925/09/efd09bfc-cd90-4fa0-b7c9-fedcf9875896.html)  * [![xiaobn_cn用户头像]( "xiaobn_cn用户自定义头像")](http://hi.csdn.net/xiaobn_cn)
* [xiaobn_cn](http://hi.csdn.net/xiaobn_cn)
* (xiaobn)
*
* 等　级：![]()
* 2#3楼 得分：0回复于：2011-09-19 11:24:03 []() oracle 10g以上版本有对应的函数：
wm_concat(列名)
这个函数不能指定分隔符，只能以逗号分隔
oracle可以自定义聚合函数* [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20110919/11/40ca7c15-3d93-4150-8f44-1c7cf345a475.html#top)
精华推荐：[经典SQL语句收集（ORACLE）](http://topic.csdn.net/u/20090302/13/1f448418-a3e3-41a9-9788-be03872471bb.html)  * [![lxyzxq2008用户头像]( "lxyzxq2008用户自定义头像")](http://hi.csdn.net/lxyzxq2008)
* [lxyzxq2008](http://hi.csdn.net/lxyzxq2008)
* (阳阳)
*
* 等　级：![]()
*#4楼 得分：0回复于：2011-09-19 11:33:47 []() 10g以后可以用wm_concat这个函数阿，行转列，逗号隔离，应该就是你需要的了* [对我有用]()[0]
* [丢个板砖]()[0]
* [引用]()
* [举报]()
* [管理]()
* [TOP](http://topic.csdn.net/u/20110919/11/40ca7c15-3d93-4150-8f44-1c7cf345a475.html#top)
精华推荐：[oracle面试题目总结---（300分相赠）！](http://topic.csdn.net/u/20080731/21/4b8eb01e-0523-4898-be3f-9cddc3b3a5b3.html)

* [管理菜单]()

* [生成帖子](http://forum.csdn.net/PointForum/BuildTopic.aspx?topicId=40ca7c15-3d93-4150-8f44-1c7cf345a475&postDate=2011-09-19+11%3a17%3a53&return=)
* [置顶]()
* [推荐]()
* [取消推荐]()
* [锁定]()
* [解锁]()
* [移动]()
* [编辑]()
* [删除]()
* [帖子加分]()
* [帖子高亮]()
* [取消高亮]()
* [结  帖](http://forum.csdn.net/PointForum/Manage/TopicManageView.aspx?forumID=4d4e3fd6-f23d-4bf7-b10f-1fb66ca785fa&topicID=40ca7c15-3d93-4150-8f44-1c7cf345a475&date=2011-09-19+11%3a17%3a53&v=13)
* [发  帖](http://forum.csdn.net/PointForum/Forum/PostTopic.aspx?forumID=4d4e3fd6-f23d-4bf7-b10f-1fb66ca785fa)
* [回  复](http://topic.csdn.net/u/20110919/11/40ca7c15-3d93-4150-8f44-1c7cf345a475.html#r_achor)
[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)北京创新乐知信息技术有限公司 版权所有, 京 ICP 证 070598 号世纪乐知(北京)网络技术有限公司 提供技术支持江苏乐知网络技术有限公司 提供商务支持![]() Email:webmaster@csdn.netCopyright © 1999-2011, CSDN.NET, All Rights Reserved[![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010) ![]()

![]()
[[关闭]](http://topic.csdn.net/u/20110919/11/40ca7c15-3d93-4150-8f44-1c7cf345a475.html#)

[[关闭]](http://topic.csdn.net/u/20110919/11/40ca7c15-3d93-4150-8f44-1c7cf345a475.html#)
