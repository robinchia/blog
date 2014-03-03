---
layout: post
title: "Struts2-XWork 安全漏洞及解决办法"
categories: 安全
tags: 
 - 安全
 - Java
--- 

# Struts2-XWork 安全漏洞及解决办法

[您还未登录 !](http://www.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://www.javaeye.com/login) [注册](http://www.javaeye.com/signup)

[![JavaEye-最棒的软件开发交流社区]( "JavaEye-最棒的软件开发交流社区")](http://www.javaeye.com/)

[论坛首页](http://www.javaeye.com/forums) → [Java编程和Java企业应用版](http://www.javaeye.com/forums/board/Java) → [Struts](http://www.javaeye.com/forums/tag/Struts) →

# Struts2/XWork 安全漏洞及解决办法

[全部](http://www.javaeye.com/forums/board/Java) [Hibernate](http://www.javaeye.com/forums/tag/Hibernate) [Spring](http://www.javaeye.com/forums/tag/Spring) [Struts](http://www.javaeye.com/forums/tag/Struts) [iBATIS](http://www.javaeye.com/forums/tag/iBATIS) [企业应用](http://www.javaeye.com/forums/tag/J2EE) [设计模式](http://www.javaeye.com/forums/tag/Design-Pattern) [DAO](http://www.javaeye.com/forums/tag/DAO) [领域模型](http://www.javaeye.com/forums/tag/Object-Domain) [OO](http://www.javaeye.com/forums/tag/OO) [Tomcat](http://www.javaeye.com/forums/tag/Tomcat) [SOA](http://www.javaeye.com/forums/tag/SOA) [JBoss](http://www.javaeye.com/forums/tag/JBoss) [Swing](http://www.javaeye.com/forums/tag/Swing) [Java综合](http://www.javaeye.com/forums/tag/Java)
[](http://www.javaeye.com/forums/39/topics/720209/posts/new "发表回复")

[myApps快速开发平台，配置即开发、所见即所得，节约85%工作量](http://www.javaeye.com/clicks/324)

« 上一页 1 [2](http://www.javaeye.com/topic/720209?page=2) [3](http://www.javaeye.com/topic/720209?page=3) … [6](http://www.javaeye.com/topic/720209?page=6) [7](http://www.javaeye.com/topic/720209?page=7) [下一页 »](http://www.javaeye.com/topic/720209?page=2)
浏览 7631 次
 [主题：Struts2/XWork 安全漏洞及解决办法](http://www.javaeye.com/topic/720209)

**该帖已经被评为精华帖** 作者 正文 * my_corner
* 等级: ![一星会员]( "一星会员")
* [![my_corner的博客]( "my_corner的博客: 我的角落")](http://my-corner.javaeye.com/)
* 文章: 96
* 积分: 130
* 来自: 北京
* ![]()
    发表时间：2010-07-24   最后修改：2010-07-27

[<](http://www.javaeye.com/topic/720209#) [>](http://www.javaeye.com/topic/720209#) 猎头职位:

相关文章: [ ](http://www.javaeye.com/topic/720209# "关闭")

* [Struts2/XWork < 2.2.0 Remote Command Execution Vulnerability 临时解决方法](http://www.javaeye.com/topic/713659 "Struts2/XWork < 2.2.0 Remote Command Execution Vulnerability 临时解决方法")
* [教你如何在java程序中调用本地应用程序](http://www.javaeye.com/topic/24780 "教你如何在java程序中调用本地应用程序")
* [struts2 中同一个action的实现中对应多个input的处理方法](http://www.javaeye.com/topic/413816 "struts2 中同一个action的实现中对应多个input的处理方法")
推荐圈子: [struts2](http://struts2.group.javaeye.com/)
[更多相关推荐](http://www.javaeye.com/wiki/topic/720209)
exploit-db网站在7月14日爆出了一个Struts2的远程执行任意代码的漏洞。
漏洞名称：Struts2/XWork < 2.2.0 Remote Command Execution Vulnerability
相关介绍：

* http://www.exploit-db.com/exploits/14360/
* http://sebug.net/exploit/19954/
Struts2的核心是使用的webwork框架，处理 action时通过调用底层的getter/setter方法来处理http的参数，它将每个http参数声明为一个ONGL（这里是ONGL的介绍）语句。当我们提交一个http参数：
?user.address.city=Bishkek&user['favoriteDrink']=kumys
ONGL将它转换为：
action.getUser().getAddress().setCity("Bishkek") action.getUser().setFavoriteDrink("kumys")
这是通过ParametersInterceptor（参数过滤器）来执行的，使用用户提供的HTTP参数调用 ValueStack.setValue()。
为了防范篡改服务器端对象，XWork的ParametersInterceptor不允许参数名中出现“#”字符，但如果使用了Java的 unicode字符串表示\u0023，攻击者就可以绕过保护，修改保护Java方式执行的值：
此处代码有破坏性，请在测试环境执行，严禁用此种方法进行恶意攻击
?('\u0023_memberAccess[\'allowStaticMethodAccess\']')(meh)=true&(aaa)(('\u0023context[\'xwork.MethodAccessor.denyMethodExecution\']\u003d\u0023foo')(\u0023foo\u003dnew%20java.lang.Boolean("false")))&(asdf)(('\u0023rt.exit(1)')(\u0023rt\u003d@java.lang.Runtime@getRuntime()))=1
转义后是这样：
?('#_memberAccess['allowStaticMethodAccess']')(meh)=true&(aaa)(('#context['xwork.MethodAccessor.denyMethodExecution']=#foo')(#foo=new%20java.lang.Boolean("false")))&(asdf)(('#rt.exit(1)')(#rt=@java.lang.Runtime@getRuntime()))=1
OGNL处理时最终的结果就是java.lang.Runtime.getRuntime().exit(1);
类似的可以执行java.lang.Runtime.getRuntime().exec("rm –rf /root")，只要有权限就可以删除任何一个目录。
目前尝试了3个解决方案：
**1.升级到struts2.2版本。**
这个可以避免这个问题，但是struts开发团队没有release这个版本（包括最新的2.2.1版本都没有release），经我测试发现新版本虽然解决了上述的漏洞，但是新的问题是strus标签出问题了。
<s:bean id="UserUtil" name="cn.com.my_corner.util.UserUtil"></s:bean> <s:property value="#UserUtil.getType().get(cType.toString())" />
这样的标签在struts2.0中是可以使用的，但是新版中就不解析了，原因就是“#”的问题导致的，补了漏洞，正常的使用也用不了了。
所以sebug网站上的建议升级到2.2版本是不可行的。
**2.struts参数过滤。**
<interceptor-ref name="params"> <param name="excludeParams">.*\\u0023.*</param> </interceptor-ref>
这个可以解决漏洞问题，缺点是工作量大，每个项目都得改struts配置文件。如果项目里，是引用的一个类似global.xml的配置文件，工作量相应减少一些。
**3.在前端请求进行过滤。**
比如在ngnix，apache进行拦截，参数中带有\u0023的一律视为攻击，跳转到404页面或者别的什么页面。这样做的一个前提就是没人把#号转码后作为参数传递。
目前来看后两种是比较有效的方法，采用第三种方法比较简便。是否有另外的解决办法，欢迎大家讨论。
我并没有在windows环境下测试，有同学在windows下没有试验成功，这并不能说明windows下就没有风险可能是我们的参数或者什么地方有问题而已。既然漏洞的确存在，咱们就要重视对吧。欢迎大家测试，是否windows下漏洞不能执行成功。
声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。

推荐链接

* [![adobe]( "adobe")](http://www.javaeye.com/clicks/373) [返回顶楼](http://www.javaeye.com/topic/720209#) [ ](http://my-corner.javaeye.com/ "浏览作者的博客") [ ](http://my-corner.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=my_corner "发送站内短信") [ ](http://my-corner.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=my_corner "关注作者")  * my_corner
* 等级: ![一星会员]( "一星会员")
* [![my_corner的博客]( "my_corner的博客: 我的角落")](http://my-corner.javaeye.com/)
* 文章: 96
* 积分: 130
* 来自: 北京
* ![]()
    发表时间：2010-07-24  

这三种方法，均已通过测试 [返回顶楼](http://www.javaeye.com/topic/720209#) [ ](http://my-corner.javaeye.com/ "浏览作者的博客") [ ](http://my-corner.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=my_corner "发送站内短信") [ ](http://my-corner.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=my_corner "关注作者") [回帖地址](http://www.javaeye.com/topic/720209#1593723 "my_corner回帖:Struts2/XWork 安全漏洞及解决办法")

[0](http://www.javaeye.com/topic/720209# "好") [0](http://www.javaeye.com/topic/720209# "差") 请登录后投票  * KimShen
* 等级: 初级会员
* [![KimShen的博客]( "KimShen的博客: 马丁大人,您也就这种把戏了.得。您继续自我感觉良好去吧")](http://kimshen.javaeye.com/)
* 文章: 168
* 积分: 0
* 来自: 上海
* ![]()
    发表时间：2010-07-24  

我们也是用的struts2,突然看到 特此登陆 谢谢LZ提醒.这个漏洞可以定义为破坏级了 [返回顶楼](http://www.javaeye.com/topic/720209#) [ ](http://kimshen.javaeye.com/ "浏览作者的博客") [ ](http://kimshen.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=KimShen "发送站内短信") [ ](http://kimshen.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=KimShen "关注作者") [回帖地址](http://www.javaeye.com/topic/720209#1594228 "KimShen回帖:Struts2/XWork 安全漏洞及解决办法")

[0](http://www.javaeye.com/topic/720209# "好") [0](http://www.javaeye.com/topic/720209# "差") 请登录后投票  * rustlingwind
* 等级: 初级会员
* [![rustlingwind的博客]( "rustlingwind的博客: 马鸣风萧萧")](http://rustlingwind.javaeye.com/)
* 文章: 6
* 积分: 30
* 来自: 北京
* ![]()
    发表时间：2010-07-25  

貌似 struts2 现在已经基本没人维护了。已经没有生命力的项目，现在还有人用？ [返回顶楼](http://www.javaeye.com/topic/720209#) [ ](http://rustlingwind.javaeye.com/ "浏览作者的博客") [ ](http://rustlingwind.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=rustlingwind "发送站内短信") [ ](http://rustlingwind.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=rustlingwind "关注作者") [回帖地址](http://www.javaeye.com/topic/720209#1594572 "rustlingwind回帖:Struts2/XWork 安全漏洞及解决办法")

[0](http://www.javaeye.com/topic/720209# "好") [0](http://www.javaeye.com/topic/720209# "差") 请登录后投票  * swanky_yao
* 等级: 初级会员
* [![swanky_yao的博客]( "swanky_yao的博客: javaEye Fans")](http://swanky-yao.javaeye.com/)
* 文章: 21
* 积分: 90
* 来自: 北京
* ![]()
    发表时间：2010-07-25  

靠 这么严重 我还如此看好它... [返回顶楼](http://www.javaeye.com/topic/720209#) [ ](http://swanky-yao.javaeye.com/ "浏览作者的博客") [ ](http://swanky-yao.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=swanky_yao "发送站内短信") [ ](http://swanky-yao.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=swanky_yao "关注作者") [回帖地址](http://www.javaeye.com/topic/720209#1594599 "swanky_yao回帖:Struts2/XWork 安全漏洞及解决办法")

[0](http://www.javaeye.com/topic/720209# "好") [0](http://www.javaeye.com/topic/720209# "差") 请登录后投票  * my_corner
* 等级: ![一星会员]( "一星会员")
* [![my_corner的博客]( "my_corner的博客: 我的角落")](http://my-corner.javaeye.com/)
* 文章: 96
* 积分: 130
* 来自: 北京
* ![]()
    发表时间：2010-07-25  

rustlingwind 写道

貌似 struts2 现在已经基本没人维护了。已经没有生命力的项目，现在还有人用？
没人维护？老兄，错了吧。struts2是webwork交给apache基金会重新包装出来的，核心代码还是webwork。你是说的webwork没人维护了吧。 [返回顶楼](http://www.javaeye.com/topic/720209#) [ ](http://my-corner.javaeye.com/ "浏览作者的博客") [ ](http://my-corner.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=my_corner "发送站内短信") [ ](http://my-corner.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=my_corner "关注作者") [回帖地址](http://www.javaeye.com/topic/720209#1594638 "my_corner回帖:Struts2/XWork 安全漏洞及解决办法")

[0](http://www.javaeye.com/topic/720209# "好") [0](http://www.javaeye.com/topic/720209# "差") 请登录后投票  * my_corner
* 等级: ![一星会员]( "一星会员")
* [![my_corner的博客]( "my_corner的博客: 我的角落")](http://my-corner.javaeye.com/)
* 文章: 96
* 积分: 130
* 来自: 北京
* ![]()
    发表时间：2010-07-25  

swanky_yao 写道

靠 这么严重 我还如此看好它...
呵呵，哪有绝对安全的东西呀，这都是随着时间逐渐暴露出来的。当然，主要是无聊的人太多了，找漏洞 [返回顶楼](http://www.javaeye.com/topic/720209#) [ ](http://my-corner.javaeye.com/ "浏览作者的博客") [ ](http://my-corner.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=my_corner "发送站内短信") [ ](http://my-corner.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=my_corner "关注作者") [回帖地址](http://www.javaeye.com/topic/720209#1594643 "my_corner回帖:Struts2/XWork 安全漏洞及解决办法")

[0](http://www.javaeye.com/topic/720209# "好") [0](http://www.javaeye.com/topic/720209# "差") 请登录后投票  * my_corner
* 等级: ![一星会员]( "一星会员")
* [![my_corner的博客]( "my_corner的博客: 我的角落")](http://my-corner.javaeye.com/)
* 文章: 96
* 积分: 130
* 来自: 北京
* ![]()
    发表时间：2010-07-25  

KimShen 写道

我们也是用的struts2,突然看到 特此登陆 谢谢LZ提醒.这个漏洞可以定义为破坏级了
呵呵，![]() [返回顶楼](http://www.javaeye.com/topic/720209#) [ ](http://my-corner.javaeye.com/ "浏览作者的博客") [ ](http://my-corner.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=my_corner "发送站内短信") [ ](http://my-corner.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=my_corner "关注作者") [回帖地址](http://www.javaeye.com/topic/720209#1594655 "my_corner回帖:Struts2/XWork 安全漏洞及解决办法")

[0](http://www.javaeye.com/topic/720209# "好") [1](http://www.javaeye.com/topic/720209# "差") 请登录后投票  * 三问飞絮
* 等级: 初级会员
* [![三问飞絮的博客]( "三问飞絮的博客: 三问飞絮")](http://simon-fish.javaeye.com/)
* 文章: 25
* 积分: 50
* 来自: 福建
* ![]()
    发表时间：2010-07-25  

my_corner 写道

swanky_yao 写道

靠 这么严重 我还如此看好它...
呵呵，哪有绝对安全的东西呀，这都是随着时间逐渐暴露出来的。当然，主要是无聊的人太多了，找漏洞
这种无聊倒是精神可嘉！ [返回顶楼](http://www.javaeye.com/topic/720209#) [ ](http://simon-fish.javaeye.com/ "浏览作者的博客") [ ](http://simon-fish.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E4%B8%89%E9%97%AE%E9%A3%9E%E7%B5%AE "发送站内短信") [ ](http://simon-fish.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E4%B8%89%E9%97%AE%E9%A3%9E%E7%B5%AE "关注作者") [回帖地址](http://www.javaeye.com/topic/720209#1594732 "三问飞絮回帖:Struts2/XWork 安全漏洞及解决办法")

[0](http://www.javaeye.com/topic/720209# "好") [1](http://www.javaeye.com/topic/720209# "差") 请登录后投票  * my_corner
* 等级: ![一星会员]( "一星会员")
* [![my_corner的博客]( "my_corner的博客: 我的角落")](http://my-corner.javaeye.com/)
* 文章: 96
* 积分: 130
* 来自: 北京
* ![]()
    发表时间：2010-07-25  

三问飞絮 写道

my_corner 写道

swanky_yao 写道

靠 这么严重 我还如此看好它...
呵呵，哪有绝对安全的东西呀，这都是随着时间逐渐暴露出来的。当然，主要是无聊的人太多了，找漏洞
这种无聊倒是精神可嘉！
呵呵，攻击别人满足自己的虚荣呗。一般都是练手的，不过也有花钱攻击竞争对手的。
![]()  扯得有点远了。
大家还有什么更好的方法，分享…… [返回顶楼](http://www.javaeye.com/topic/720209#) [ ](http://my-corner.javaeye.com/ "浏览作者的博客") [ ](http://my-corner.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=my_corner "发送站内短信") [ ](http://my-corner.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=my_corner "关注作者") [回帖地址](http://www.javaeye.com/topic/720209#1594751 "my_corner回帖:Struts2/XWork 安全漏洞及解决办法")

[0](http://www.javaeye.com/topic/720209# "好") [0](http://www.javaeye.com/topic/720209# "差") 请登录后投票

[](http://www.javaeye.com/forums/39/topics/720209/posts/new "发表回复")

« 上一页 1 [2](http://www.javaeye.com/topic/720209?page=2) [3](http://www.javaeye.com/topic/720209?page=3) … [6](http://www.javaeye.com/topic/720209?page=6) [7](http://www.javaeye.com/topic/720209?page=7) [下一页 »](http://www.javaeye.com/topic/720209?page=2)
[论坛首页](http://www.javaeye.com/forums) → [Java编程和Java企业应用版](http://www.javaeye.com/forums/board/Java) → [Struts](http://www.javaeye.com/forums/tag/Struts)
跳转论坛:Java编程和Java企业应用 Web前端技术 移动编程和手机应用开发 C/C++编程 Ruby编程 Python编程 PHP编程 Flash编程和RIA Microsoft .Net 综合技术 软件开发和项目管理 行业应用 入门讨论 招聘求职 海阔天空

* [北京: JavaEye猎头诚聘Java搜索工程师](http://www.javaeye.com/jobs/712 "北京:JavaEye猎头诚聘Java搜索工程师")
* [浙江: VMKID诚聘Java开发工程师(J2SE)（Client端方](http://www.javaeye.com/jobs/779 "浙江:VMKID诚聘Java开发工程师(J2SE)（Client端方向）")
* [北京: 去哪儿旅游搜索引擎诚聘java开发工程师](http://www.javaeye.com/jobs/523 "北京:去哪儿旅游搜索引擎诚聘java开发工程师")
* [北京: 掌中魔力诚聘java资深工程师](http://www.javaeye.com/jobs/769 "北京:掌中魔力诚聘java资深工程师")
* [北京: chinacache诚聘Java应用架构师](http://www.javaeye.com/jobs/728 "北京:chinacache诚聘Java应用架构师")
* [北京: glodon诚聘Eclipse插件开发工程师](http://www.javaeye.com/jobs/839 "北京:glodon诚聘Eclipse插件开发工程师")
* [北京: 网易（NETEASE）诚聘高级Java开发工程师](http://www.javaeye.com/jobs/800 "北京:网易（NETEASE）诚聘高级Java开发工程师")
* [福建: 福州几维网络诚聘游戏服务端程序员(Java)](http://www.javaeye.com/jobs/804 "福建:福州几维网络诚聘游戏服务端程序员(Java)")
* [北京: Laszlo Systems 诚聘高级Java程序员](http://www.javaeye.com/jobs/817 "北京:Laszlo Systems 诚聘高级Java程序员")
* [上海: BShare诚聘资深Java Web开发工程师](http://www.javaeye.com/jobs/838 "上海:BShare诚聘资深Java Web开发工程师")
* [广东: 穆迪moody's诚聘Java Developer](http://www.javaeye.com/jobs/794 "广东:穆迪moody's诚聘Java Developer")
* [上海: 上海汉得诚聘高级基础架构研发工程师——IDE](http://www.javaeye.com/jobs/825 "上海:上海汉得诚聘高级基础架构研发工程师——IDE方向")
* [上海: 恺英网络诚聘JS工程师](http://www.javaeye.com/jobs/682 "上海:恺英网络诚聘JS工程师")
* [北京: 上海幽幽诚聘手机网游服务器端开发工程师](http://www.javaeye.com/jobs/806 "北京:上海幽幽诚聘手机网游服务器端开发工程师")
* [北京: chinacache诚聘高级Java开发工程师（后台应用](http://www.javaeye.com/jobs/726 "北京:chinacache诚聘高级Java开发工程师（后台应用）")

* [首页](http://www.javaeye.com/)
* [新闻](http://www.javaeye.com/news)
* [论坛](http://www.javaeye.com/forums)
* [问答](http://www.javaeye.com/ask)
* [专栏](http://www.javaeye.com/wiki)
* [博客](http://www.javaeye.com/blogs)
* [圈子](http://www.javaeye.com/groups)
* [招聘](http://www.javaeye.com/job)
* [服务](http://www.javaeye.com/index/service)
* [搜索](http://www.javaeye.com/search)
* [Java](http://java.javaeye.com/)
* [Web](http://webapp.javaeye.com/)
* [Ruby](http://ruby.javaeye.com/)
* [Python](http://python.javaeye.com/)
* [敏捷](http://agile.javaeye.com/)
* [MySQL](http://mysql.javaeye.com/)
* [润亁报表](http://runqian.javaeye.com/)
* [图书](http://book.javaeye.com/)
* [广告服务](http://www.javaeye.com/index/service)
* [JavaEye黑板报](http://webmaster.javaeye.com/)
* [关于我们](http://www.javaeye.com/index/aboutus)
* [联系我们](http://www.javaeye.com/index/contactus)
* [友情链接](http://www.javaeye.com/index/friend_links)

© 2003-2010 JavaEye.com. 上海炯耐计算机软件有限公司版权所有 [ [沪ICP备05023328号](http://www.miibeian.gov.cn/) ]
