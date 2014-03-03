---
layout: post
title: "用java删除文件夹里的所有文件"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# 用java删除文件夹里的所有文件

# [Java on Line](http://www.blogjava.net/mrcmd/)

和java的日子！

  [BlogJava](http://www.blogjava.net/) :: [首页](http://www.blogjava.net/mrcmd/) :: [新随笔](http://www.blogjava.net/mrcmd/admin/EditPosts.aspx?opt=1) :: [联系](http://www.blogjava.net/mrcmd/contact.aspx?id=1) :: [聚合](http://www.blogjava.net/mrcmd/rss) [![]()](http://www.blogjava.net/mrcmd/rss) :: [管理](http://www.blogjava.net/mrcmd/admin/EditPosts.aspx) :: ![]()   8 随笔 :: 0 文章 :: 8 评论 :: 0 Trackbacks

[<]( "Go to the previous month") 2007年10月 [>]( "Go to the next month") 日 一 二 三 四 五 六 30 1 2 3 4 5 6 7 8 9 10 11 [12](http://www.blogjava.net/mrcmd/archive/2007/10/12.html) 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 1 2 3 4 5 6 7 8 9 10

### 公告

谢谢您的关注！！！

### 留言簿(1)

* [给我留言](http://www.blogjava.net/mrcmd/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/mrcmd/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/mrcmd/admin/MyMessages.aspx)

### 随笔分类(8)

* [Java基础(5)](http://www.blogjava.net/mrcmd/category/25205.html) [(rss)](http://www.blogjava.net/mrcmd/category/25205.html/rss "Subscribe to Java基础(5)")
* [Java收集](http://www.blogjava.net/mrcmd/category/25267.html) [(rss)](http://www.blogjava.net/mrcmd/category/25267.html/rss "Subscribe to Java收集")
* [个人随笔(1)](http://www.blogjava.net/mrcmd/category/25194.html) [(rss)](http://www.blogjava.net/mrcmd/category/25194.html/rss "Subscribe to 个人随笔(1)")
* [开发收集(2)](http://www.blogjava.net/mrcmd/category/25833.html) [(rss)](http://www.blogjava.net/mrcmd/category/25833.html/rss "Subscribe to 开发收集(2)")

### 随笔档案(8)

* [2008年4月 (2)](http://www.blogjava.net/mrcmd/archive/2008/04.html)
* [2008年3月 (1)](http://www.blogjava.net/mrcmd/archive/2008/03.html)
* [2007年10月 (1)](http://www.blogjava.net/mrcmd/archive/2007/10.html)
* [2007年9月 (1)](http://www.blogjava.net/mrcmd/archive/2007/09.html)
* [2007年8月 (3)](http://www.blogjava.net/mrcmd/archive/2007/08.html)

### 相关连接

* [expert](http://expert.blog.51cto.com/)
* [struts2学习](http://www.blogjava.net/max/)
* [Sun中国技术社区](http://developers.sun.com.cn/)
* [我的Blog](http://hi.baidu.com/mrcmd)
* [日期控件](http://www.my97.net/dp/demo/index.htm)

### 搜索

*
*  

### 最新评论 [![]()](http://www.blogjava.net/mrcmd/CommentsRSS.aspx)

* [1. re: 用java压缩文件夹/文件](http://www.blogjava.net/mrcmd/archive/2009/03/06/138963.html#258116)
* 非常感谢!
* --sem
* [2. re: 用java解压文件夹](http://www.blogjava.net/mrcmd/archive/2009/02/13/139126.html#254531)
* 就是太慢!!!请问怎样才能快点!!!
* --huzheng
* [3. re: 用java压缩文件夹/文件](http://www.blogjava.net/mrcmd/archive/2008/10/12/138963.html#233897)
* 可以集成进jdk了
* --xh
* [4. re: 用java解压文件夹](http://www.blogjava.net/mrcmd/archive/2008/10/06/139126.html#232741)
* 非常好用，顶
* --re
* [5. re: 用java解压文件夹](http://www.blogjava.net/mrcmd/archive/2008/07/02/139126.html#212188)
* 错误的
* --1111111
[用java删除文件夹里的所有文件](http://www.blogjava.net/mrcmd/archive/2007/10/12/139003.html)

![]()import java.io.File;
![]()
![]()public class Test
![]()![]()![](){
![]()![]()   public static void main(String args[])![](){
![]()       Test t = new Test();
![]()       delFolder("c:/bb");
![]()       System.out.println("deleted![]()");
![]()}
![]()
![]()//删除文件夹
![]()//param folderPath 文件夹完整绝对路径
![]()
![]()![]()     public static void delFolder(String folderPath) ![](){
![]()![]()     try ![](){
![]()        delAllFile(folderPath); //删除完里面所有内容
![]()        String filePath = folderPath;
![]()        filePath = filePath.toString();
![]()        java.io.File myFilePath = new java.io.File(filePath);
![]()        myFilePath.delete(); //删除空文件夹
![]()![]()     } catch (Exception e) ![](){
![]()       e.printStackTrace(); 
![]()     }
![]()}
![]()
![]()//删除指定文件夹下所有文件
![]()//param path 文件夹完整绝对路径
![]()![]()   public static boolean delAllFile(String path) ![](){
![]()       boolean flag = false;
![]()       File file = new File(path);
![]()![]()       if (!file.exists()) ![](){
![]()         return flag;
![]()       }
![]()![]()       if (!file.isDirectory()) ![](){
![]()         return flag;
![]()       }
![]()       String[] tempList = file.list();
![]()       File temp = null;
![]()![]()       for (int i = 0; i < tempList.length; i++) ![](){
![]()![]()          if (path.endsWith(File.separator)) ![](){
![]()             temp = new File(path + tempList[i]);
![]()![]()          } else ![](){
![]()              temp = new File(path + File.separator + tempList[i]);
![]()          }
![]()![]()          if (temp.isFile()) ![](){
![]()             temp.delete();
![]()          }
![]()![]()          if (temp.isDirectory()) ![](){
![]()             delAllFile(path + "/" + tempList[i]);//先删除文件夹里面的文件
![]()             delFolder(path + "/" + tempList[i]);//再删除空文件夹
![]()             flag = true;
![]()          }
![]()       }
![]()       return flag;
![]()     }
![]()}
posted on 2007-10-12 16:19 [陈东](http://www.blogjava.net/mrcmd/) 阅读(4139) [评论(0)](http://www.blogjava.net/mrcmd/archive/2007/10/12/139003.html#Post)  [编辑](http://www.blogjava.net/mrcmd/admin/EditPosts.aspx?postid=139003)  [收藏](http://www.blogjava.net/mrcmd/AddToFavorite.aspx?id=139003) 所属分类: [Java基础](http://www.blogjava.net/mrcmd/category/25205.html)![]()

[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [淘宝网诚聘资深J2ME开发工程师](http://job.cnblogs.com/offer/6509/)
IT新闻：
· [WiFi网络不稳定 业界称iPad恐难扎根国内](http://news.cnblogs.com/n/61117/)
· [福布斯：中兴超越摩托罗拉成全球第五大手机商](http://news.cnblogs.com/n/61116/)
· [报告称谷歌正在内测桌面版Google Voice](http://news.cnblogs.com/n/61115/)
· [苹果iPad热度不减 5天销量已达到60万部](http://news.cnblogs.com/n/61114/)
· [苹果明年或推小号iPad 采用5英寸屏幕](http://news.cnblogs.com/n/61113/)
专题：[Android](http://kb.cnblogs.com/zt/android/)  [iPad](http://kb.cnblogs.com/zt/iPad/)  [jQuery](http://kb.cnblogs.com/zt/jquery/)  [Chrome OS](http://kb.cnblogs.com/zt/chrome/)    [博客园首页](http://www.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [知识库](http://kb.cnblogs.com/)  [学英语](http://a4.yeshj.com/rd/34143/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/mrcmd/archive/2007/10/12/139003.html&SourceURL=/mrcmd/archive/2007/10/12/139003.html)       [使用Ctrl+Enter键可以直接提交]   [每天10分钟，轻松学英语](http://a4.yeshj.com/rd/34138/)  推荐职位：
· [飞信服务器端高级.NET开发工程师(新媒传信)](http://job.cnblogs.com/offer/6318/)
· [.NET飞信官网开发工程师(新媒传信)](http://job.cnblogs.com/offer/6319/)
· [Web前端研发工程师(百度)](http://job.cnblogs.com/offer/6492/)
· [C++开发工程师(沪江网)](http://job.cnblogs.com/zt/yeshj/#offer3)
· [前端开发工程师(沪江网)](http://job.cnblogs.com/zt/yeshj/#offer2)
· [产品经理(沪江网)](http://job.cnblogs.com/zt/yeshj/#offer6)
· [运维工程师(沪江网)](http://job.cnblogs.com/zt/yeshj/#offer9)

博客园首页随笔：
· [群发“站内信”的实现（续）](http://www.cnblogs.com/grenet/archive/2010/04/09/1708008.html)
· [数据库索引白话篇](http://www.cnblogs.com/guilin_gavin/archive/2010/04/09/1707980.html)
· [18个不常见的C#关键字，您使用过几个？](http://www.cnblogs.com/zhuqil/archive/2010/04/09/UnCommon-Csharp-keywords-A-Look.html)
· [windows下信号机制的学习](http://www.cnblogs.com/ringofthec/archive/2010/04/09/1707926.html)
· [团队基础生成自动化流程之最佳实践(VI) - 系统模块化条件编译](http://www.cnblogs.com/sun/archive/2010/04/08/1707893.html)
知识库：
· [如何做网页设计的10个小窍门](http://kb.cnblogs.com/page/61095/)
· [社区媒体和网站的九个关键性界面特征](http://kb.cnblogs.com/page/61042/)
· [从零到十亿，创业企业家如何迈向成功？](http://kb.cnblogs.com/page/60937/)
· [如何精简用户界面](http://kb.cnblogs.com/page/60924/)
· [每天进步一点点，一个月后，一年后，十年后，百年后...](http://kb.cnblogs.com/page/60806/)  网站导航:

[博客园](http://www.cnblogs.com/)   [IT新闻](http://news.cnblogs.com/)   [个人主页](http://home.cnblogs.com/)   [博客生活](http://www.cnweblog.com/)   [IT博客网](http://www.cnitblog.com/)   [C++博客](http://www.cppblog.com/)   [博客园社区](http://space.cnblogs.com/) [管理](http://www.blogjava.net/mrcmd/archive/2007/10/12/139003.html?opt=admin) 相关文章:

* [JDBC连接各种数据库方法](http://www.blogjava.net/mrcmd/archive/2008/04/16/193476.html)
* [将一个字符串里的数字，分割后进行排序](http://www.blogjava.net/mrcmd/archive/2008/03/07/184379.html)
* [用java删除文件夹里的所有文件](http://www.blogjava.net/mrcmd/archive/2007/10/12/139003.html)
* [用java解压文件夹](http://www.blogjava.net/mrcmd/archive/2007/08/24/139126.html)
* [用java压缩文件夹/文件](http://www.blogjava.net/mrcmd/archive/2007/08/24/138963.html)  

Powered by:
[BlogJava](http://www.blogjava.net/)
Copyright © 陈东
