---
layout: post
title: "httpClient中文乱码问题解决（wap提交）收藏 - kangojian的专栏 - 博客频道 "
categories: Httpclient
tags: 
 - Httpclient
--- 

# httpClient中文乱码问题解决（wap提交）收藏 - kangojian的专栏 - 博客频道 - CSDN.NET

您还未登录！|[登录](https://passport.csdn.net/account/login)|[注册](https://passport.csdn.net/account/register)|[帮助](https://passport.csdn.net/help/faq)

* [首页](http://www.csdn.net/)
* [业界](http://news.csdn.net/)
* [移动](http://mobile.csdn.net/)
* [云计算](http://cloud.csdn.net/)
* [研发](http://sd.csdn.net/)
* [论坛](http://bbs.csdn.net/)
* [博客](http://blog.csdn.net/)
* [下载](http://download.csdn.net/)
* 
## [更多]()

# [kangojian的专栏](http://blog.csdn.net/kangojian)

##

* [![]()目录视图](http://blog.csdn.net/kangojian?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/kangojian?viewmode=list)
* [![]()订阅](http://blog.csdn.net/kangojian/rss/list)
[公告：CSDN博客频道推出博客导出工具（开源）](http://blog.csdn.net/sq_zhuyi/article/details/7924776)      [公告：CSDN博客频道推出文章目录功能](http://blog.csdn.net/csdnproduct/article/details/7987178)     [CSDN产品客服新浪微博正式上线](http://blog.csdn.net/csdnproduct/article/details/7964475)
[移动开发者大会最新议题发布6折抢票最后3天](http://mdcc.csdn.net/)                 [“第一次亲密接触”—项目遇到的问题有奖征文活动](http://blog.csdn.net/blogdevteam/article/details/7954317)

### [httpClient中文乱码问题解决（wap提交）收藏]()

2008-06-24 14:55 547人阅读 [评论](http://blog.csdn.net/kangojian/article/details/2582161#comments)(0) [收藏]( "收藏") [举报](http://blog.csdn.net/kangojian/article/details/2582161#report "举报")
我在尝试着直接将中文改变为utf-8的字符串直接写入，失败后！以为是网络传输中应该是iso-8859-1方式传输的，然后将中文转为该编码格式，还是失败后，看httpclient源代码发现：重写postmethod中的getrequestcharset()方法，虽然源代码中该方法动态的设置编码格式，但是好像并没有很好的执行！在重写后，问题解决！ package pro; import java.io.IOException; import org.apache.commons.httpclient.HttpClient; import org.apache.commons.httpclient.HttpException; import org.apache.commons.httpclient.HttpStatus; import org.apache.commons.httpclient.NameValuePair; import org.apache.commons.httpclient.methods.PostMethod; public class simulateWebAction {     public static void main(String[] args) throws IOException {         // TODO Auto-generated method stub         String url = “*******”;         PostMethod postMethod = new UTF8PostMethod(url);         StringBuilder origin = new StringBuilder();         origin.setLength(0);                 HttpClient httpClient = new HttpClient(); //        getMethod.getParams().setParameter(HttpMethodParams.RETRY_HANDLER, new DefaultHttpMethodRetryHandler());                 NameValuePair a = new NameValuePair("a","**");         NameValuePair q = new NameValuePair("q","**");         NameValuePair[] param = new NameValuePair[]{a,q};                 postMethod.setRequestBody(param);         try { //            执行getMethod             int statusCode = httpClient.executeMethod(postMethod);             if (statusCode != HttpStatus.SC_OK) {                 System.err.println("Method failed: "+ postMethod.getStatusLine());             }else{ //                读内容                 System.out.println(postMethod.getResponseBodyAsString());             }                         } catch (HttpException e) {                 // TODO Auto-generated catch block                 e.printStackTrace();             } catch (IOException e) {                 // TODO Auto-generated catch block                 e.printStackTrace();             }finally{                 postMethod.releaseConnection();             }        }         public static class UTF8PostMethod extends PostMethod{         public UTF8PostMethod(String url){             super(url);         }         @Override         public String getRequestCharSet() {             //return super.getRequestCharSet();             return "UTF-8";         }     } }

分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")
* 上一篇：[HTTP 1.1 状态码与它们的用途(ZT)- -](http://blog.csdn.net/kangojian/article/details/2582124)
* 下一篇：[struts2.0----菜鸟日记一](http://blog.csdn.net/kangojian/article/details/2597273)
查看评论[]()

  暂无评论
您还没有登录,请[[登录]](http://passport.csdn.net/account/login?from=http%3A%2F%2Fblog.csdn.net%2Fkangojian%2Farticle%2Fdetails%2F2582161)或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fkangojian%2Farticle%2Fdetails%2F2582161)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()
[![TOP]()](http://blog.csdn.net/kangojian/article/details/2582161# "回到顶部")

个人资料

[![]( "访问我的空间")](http://my.csdn.net/kangojian)
[kangojian](http://my.csdn.net/kangojian)

[](http://blog.csdn.net/kangojian/article/details/2582161# "[加关注]") [](http://my.csdn.net/my/letter/send/kangojian "[发私信]")

* 访问：87731次
* 积分：2367分
* 排名：第2242名

* 原创：127篇
* 转载：103篇
* 译文：2篇
* 评论：36条

文章搜索

[]()
文章分类

* [Apache](http://blog.csdn.net/kangojian/article/category/496276)(15)
* [java Spider](http://blog.csdn.net/kangojian/article/category/502638)(2)
* [java 算法](http://blog.csdn.net/kangojian/article/category/509900)(13)
* [javaAPI](http://blog.csdn.net/kangojian/article/category/496279)(56)
* [Js](http://blog.csdn.net/kangojian/article/category/496277)(1)
* [Linux](http://blog.csdn.net/kangojian/article/category/583377)(2)
* [Luncece](http://blog.csdn.net/kangojian/article/category/496278)(1)
* [luncece](http://blog.csdn.net/kangojian/article/category/502640)(1)
* [mysql](http://blog.csdn.net/kangojian/article/category/496281)(25)
* [spring](http://blog.csdn.net/kangojian/article/category/496280)(12)
* [数据层](http://blog.csdn.net/kangojian/article/category/702233)(4)
* [杂谈](http://blog.csdn.net/kangojian/article/category/567162)(19)
* [设计模式](http://blog.csdn.net/kangojian/article/category/683710)(4)

文章存档

* [2012年03月](http://blog.csdn.net/kangojian/article/month/2012/03)(1)
* [2012年02月](http://blog.csdn.net/kangojian/article/month/2012/02)(2)
* [2012年01月](http://blog.csdn.net/kangojian/article/month/2012/01)(5)
* [2011年11月](http://blog.csdn.net/kangojian/article/month/2011/11)(1)
* [2011年10月](http://blog.csdn.net/kangojian/article/month/2011/10)(5)
* [2011年09月](http://blog.csdn.net/kangojian/article/month/2011/09)(4)
* [2011年08月](http://blog.csdn.net/kangojian/article/month/2011/08)(5)
* [2011年05月](http://blog.csdn.net/kangojian/article/month/2011/05)(5)
* [2011年04月](http://blog.csdn.net/kangojian/article/month/2011/04)(2)
* [2011年03月](http://blog.csdn.net/kangojian/article/month/2011/03)(5)
* [2011年02月](http://blog.csdn.net/kangojian/article/month/2011/02)(8)
* [2011年01月](http://blog.csdn.net/kangojian/article/month/2011/01)(4)
* [2010年12月](http://blog.csdn.net/kangojian/article/month/2010/12)(4)
* [2010年11月](http://blog.csdn.net/kangojian/article/month/2010/11)(4)
* [2010年10月](http://blog.csdn.net/kangojian/article/month/2010/10)(1)
* [2010年08月](http://blog.csdn.net/kangojian/article/month/2010/08)(4)
* [2010年07月](http://blog.csdn.net/kangojian/article/month/2010/07)(16)
* [2010年06月](http://blog.csdn.net/kangojian/article/month/2010/06)(3)
* [2010年05月](http://blog.csdn.net/kangojian/article/month/2010/05)(2)
* [2010年04月](http://blog.csdn.net/kangojian/article/month/2010/04)(5)
* [2010年03月](http://blog.csdn.net/kangojian/article/month/2010/03)(2)
* [2010年02月](http://blog.csdn.net/kangojian/article/month/2010/02)(7)
* [2010年01月](http://blog.csdn.net/kangojian/article/month/2010/01)(7)
* [2009年12月](http://blog.csdn.net/kangojian/article/month/2009/12)(2)
* [2009年11月](http://blog.csdn.net/kangojian/article/month/2009/11)(10)
* [2009年10月](http://blog.csdn.net/kangojian/article/month/2009/10)(3)
* [2009年09月](http://blog.csdn.net/kangojian/article/month/2009/09)(2)
* [2009年08月](http://blog.csdn.net/kangojian/article/month/2009/08)(8)
* [2009年07月](http://blog.csdn.net/kangojian/article/month/2009/07)(7)
* [2009年06月](http://blog.csdn.net/kangojian/article/month/2009/06)(1)
* [2009年05月](http://blog.csdn.net/kangojian/article/month/2009/05)(5)
* [2009年04月](http://blog.csdn.net/kangojian/article/month/2009/04)(1)
* [2009年03月](http://blog.csdn.net/kangojian/article/month/2009/03)(11)
* [2009年02月](http://blog.csdn.net/kangojian/article/month/2009/02)(1)
* [2009年01月](http://blog.csdn.net/kangojian/article/month/2009/01)(4)
* [2008年12月](http://blog.csdn.net/kangojian/article/month/2008/12)(2)
* [2008年11月](http://blog.csdn.net/kangojian/article/month/2008/11)(11)
* [2008年10月](http://blog.csdn.net/kangojian/article/month/2008/10)(9)
* [2008年09月](http://blog.csdn.net/kangojian/article/month/2008/09)(8)
* [2008年08月](http://blog.csdn.net/kangojian/article/month/2008/08)(6)
* [2008年07月](http://blog.csdn.net/kangojian/article/month/2008/07)(6)
* [2008年06月](http://blog.csdn.net/kangojian/article/month/2008/06)(10)
* [2008年05月](http://blog.csdn.net/kangojian/article/month/2008/05)(11)
* [2008年04月](http://blog.csdn.net/kangojian/article/month/2008/04)(7)
* [2008年03月](http://blog.csdn.net/kangojian/article/month/2008/03)(5)

展开
阅读排行

* [•Oracle 9i企业版官方下载 9.2.0.1.0](http://blog.csdn.net/kangojian/article/details/3122483 "•Oracle 9i企业版官方下载 9.2.0.1.0 ")(6936)
* [Java中的invoke使用](http://blog.csdn.net/kangojian/article/details/3216884 "Java中的invoke使用")(4194)
* [java 超简单 生成json与解析](http://blog.csdn.net/kangojian/article/details/2991316 "java 超简单 生成json与解析")(3731)
* [spring配置文件详解](http://blog.csdn.net/kangojian/article/details/3238699 "spring配置文件详解")(3037)
* [mysql #1349 - View's SELECT contains a subquery in the FROM clause 错误](http://blog.csdn.net/kangojian/article/details/2801200 "mysql  #1349 - View's SELECT contains a subquery in the FROM clause  错误")(2801)
* [在windows xp下,一块网卡绑定多个ip,设置多个网络连接](http://blog.csdn.net/kangojian/article/details/3095274 "在windows xp下,一块网卡绑定多个ip,设置多个网络连接")(2062)
* [hibernate 级联 必须注意的问题](http://blog.csdn.net/kangojian/article/details/2808001 "hibernate 级联 必须注意的问题")(1650)
* [Ext（一） 登录+验证码](http://blog.csdn.net/kangojian/article/details/3410763 "Ext（一） 登录+验证码")(1469)
* [struts2.0----菜鸟日记二](http://blog.csdn.net/kangojian/article/details/2597586 "struts2.0----菜鸟日记二")(1330)
* [java 内存监控使用](http://blog.csdn.net/kangojian/article/details/5955677 "java 内存监控使用")(1320)

评论排行

* [Ext（一） 登录+验证码](http://blog.csdn.net/kangojian/article/details/3410763 "Ext（一） 登录+验证码")(5)
* [Java中的invoke使用](http://blog.csdn.net/kangojian/article/details/3216884 "Java中的invoke使用")(5)
* [java nio Selector的使用－客户端](http://blog.csdn.net/kangojian/article/details/5711358 "java nio Selector的使用－客户端")(5)
* [网页快照 java 实现](http://blog.csdn.net/kangojian/article/details/6207464 "网页快照 java 实现")(5)
* [•Oracle 9i企业版官方下载 9.2.0.1.0](http://blog.csdn.net/kangojian/article/details/3122483 "•Oracle 9i企业版官方下载 9.2.0.1.0 ")(4)
* [java博客灌水器完成](http://blog.csdn.net/kangojian/article/details/3624788 "java博客灌水器完成")(1)
* [Why use a ProtocolCodecFilter](http://blog.csdn.net/kangojian/article/details/5823357 "Why use a ProtocolCodecFilter")(1)
* [java算法之冒泡](http://blog.csdn.net/kangojian/article/details/3853955 "java算法之冒泡")(1)
* [java IO 概念误区---------同步/异步与阻塞/非阻塞的区别](http://blog.csdn.net/kangojian/article/details/5710977 "java IO 概念误区---------同步/异步与阻塞/非阻塞的区别 ")(1)
* [java NIO 与IO的比较](http://blog.csdn.net/kangojian/article/details/5710877 "java NIO 与IO的比较")(1)
推荐文章

最新评论

* [java NIO 与IO的比较](http://blog.csdn.net/kangojian/article/details/5710877#comments)

liu149339750: 下载地址不可用！！
* [网页快照 java 实现](http://blog.csdn.net/kangojian/article/details/6207464#comments)

yangyawei2013: DJNativeSwing
* [网页快照 java 实现](http://blog.csdn.net/kangojian/article/details/6207464#comments)

jinyan1218: 能把chrriis包提供出来不？
* [java IO 概念误区---------同步/异步与阻塞/非阻塞的区别](http://blog.csdn.net/kangojian/article/details/5710977#comments)

wanghuichun8000: 不错
* [网页快照 java 实现](http://blog.csdn.net/kangojian/article/details/6207464#comments)

ncx1259988: 你把jar包提供出来啊
* [深入理解java的finalize](http://blog.csdn.net/kangojian/article/details/5443220#comments)

qin0701: 想看对象的状态转换图。
* [网页快照 java 实现](http://blog.csdn.net/kangojian/article/details/6207464#comments)

zq86813: 运行都报错。
* [java nio Selector的使用－客户端](http://blog.csdn.net/kangojian/article/details/5711358#comments)

yun2223: 顶一下。能不能也发一份代码到给我啊。。。eye上要下个代码还要注册以后等三天。。。397864905...
* [java nio Selector的使用－客户端](http://blog.csdn.net/kangojian/article/details/5711358#comments)

huanhuanyunzhou: ffsadfsdf
* [java nio Selector的使用－客户端](http://blog.csdn.net/kangojian/article/details/5711358#comments)

huanhuanyunzhou: rtyrtyryryrytr
      ![]()

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)京 ICP 证 070598 号北京创新乐知信息技术有限公司 版权所有![]() 联系邮箱：webmaster@csdn.netCopyright © 1999-2012, CSDN.NET, All Rights Reserved [![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
### [![close]()]()

[![]()](http://2012amap.csdn.net/)![]()
