---
layout: post
title: "java.net.SocketException  Too many open files 问题的解"
categories: 服务器
tags: 
 - 服务器
 - linux文件打开
--- 

# java.net.SocketException Too many open files 问题的解决办法 - R&D--梦想在这里成真 R&D--夢を実現しましょう  R&D--Dreams Come Ture - IT博客

[R&D--梦想在这里成真
 R&D--夢を実現しましょう
  R&D--Dreams Come Ture](http://www.cnitblog.com/rd416/)
   **努力 我们将梦想变为现实**

[IT博客](http://www.cnitblog.com/)  ::  [首页](http://www.cnitblog.com/rd416/)  ::  [新随笔](http://www.cnitblog.com/rd416/admin/EditPosts.aspx?opt=1)  ::  [联系](http://www.cnitblog.com/rd416/contact.aspx?id=1)  ::  [聚合](http://www.cnitblog.com/rd416/Rss.aspx) [![]()](http://www.cnitblog.com/rd416/Rss.aspx)  ::  [管理](http://www.cnitblog.com/rd416/admin/EditPosts.aspx)
posts - 134,  comments - 20,  trackbacks - 0

[<]( "Go to the previous month")2009年2月[>]( "Go to the next month")日一二三四五六25262728293031123456[7](http://www.cnitblog.com/rd416/archive/2009/02/07.html)89[10](http://www.cnitblog.com/rd416/archive/2009/02/10.html)1112131415161718192021222324[25](http://www.cnitblog.com/rd416/archive/2009/02/25.html)2627[28](http://www.cnitblog.com/rd416/archive/2009/02/28.html)1234567

### 常用链接

* [我的随笔](http://www.cnitblog.com/rd416/MyPosts.html)
* [我的评论](http://www.cnitblog.com/rd416/MyComments.html)
* [我参与的随笔](http://www.cnitblog.com/rd416/OtherPosts.html)

### 留言簿(1)

* [给我留言](http://www.cnitblog.com/rd416/Contact.aspx?id=1)
* [查看公开留言](http://www.cnitblog.com/rd416/default.aspx?opt=msg)
* [查看私人留言](http://www.cnitblog.com/rd416/admin/MyMessages.aspx)

# 随笔分类

* [AJAX(5)](http://www.cnitblog.com/rd416/category/5693.html)[![]( "Subscribe to AJAX(5)")](http://www.cnitblog.com/rd416/category/5693.html/rss "Subscribe to AJAX(5)")
* [ASP.NET(13)](http://www.cnitblog.com/rd416/category/5694.html)[![]( "Subscribe to ASP.NET(13)")](http://www.cnitblog.com/rd416/category/5694.html/rss "Subscribe to ASP.NET(13)")
* [C#(13)](http://www.cnitblog.com/rd416/category/5724.html)[![]( "Subscribe to C#(13)")](http://www.cnitblog.com/rd416/category/5724.html/rss "Subscribe to C#(13)")
* [CSS(4)](http://www.cnitblog.com/rd416/category/5698.html)[![]( "Subscribe to CSS(4)")](http://www.cnitblog.com/rd416/category/5698.html/rss "Subscribe to CSS(4)")
* [HTML(9)](http://www.cnitblog.com/rd416/category/5696.html)[![]( "Subscribe to HTML(9)")](http://www.cnitblog.com/rd416/category/5696.html/rss "Subscribe to HTML(9)")
* [JAVA(39)](http://www.cnitblog.com/rd416/category/6468.html)[![]( "Subscribe to JAVA(39)")](http://www.cnitblog.com/rd416/category/6468.html/rss "Subscribe to JAVA(39)")
* [Javascript(15)](http://www.cnitblog.com/rd416/category/5695.html)[![]( "Subscribe to Javascript(15)")](http://www.cnitblog.com/rd416/category/5695.html/rss "Subscribe to Javascript(15)")
* [Linux(6)](http://www.cnitblog.com/rd416/category/5916.html)[![]( "Subscribe to Linux(6)")](http://www.cnitblog.com/rd416/category/5916.html/rss "Subscribe to Linux(6)")
* [Orcal(1)](http://www.cnitblog.com/rd416/category/8268.html)[![]( "Subscribe to Orcal(1)")](http://www.cnitblog.com/rd416/category/8268.html/rss "Subscribe to Orcal(1)")
* [Ruby(1)](http://www.cnitblog.com/rd416/category/5931.html)[![]( "Subscribe to Ruby(1)")](http://www.cnitblog.com/rd416/category/5931.html/rss "Subscribe to Ruby(1)")
* [SQL(5)](http://www.cnitblog.com/rd416/category/5692.html)[![]( "Subscribe to SQL(5)")](http://www.cnitblog.com/rd416/category/5692.html/rss "Subscribe to SQL(5)")
* [XML(3)](http://www.cnitblog.com/rd416/category/5697.html)[![]( "Subscribe to XML(3)")](http://www.cnitblog.com/rd416/category/5697.html/rss "Subscribe to XML(3)")
* [感想(1)](http://www.cnitblog.com/rd416/category/5691.html)[![]( "Subscribe to 感想(1)")](http://www.cnitblog.com/rd416/category/5691.html/rss "Subscribe to 感想(1)")
* [技术资源(16)](http://www.cnitblog.com/rd416/category/5722.html)[![]( "Subscribe to 技术资源(16)")](http://www.cnitblog.com/rd416/category/5722.html/rss "Subscribe to 技术资源(16)")
* [精品转载(61)](http://www.cnitblog.com/rd416/category/7565.html)[![]( "Subscribe to 精品转载(61)")](http://www.cnitblog.com/rd416/category/7565.html/rss "Subscribe to 精品转载(61)")

# 随笔档案

* [2010年2月 (1)](http://www.cnitblog.com/rd416/archive/2010/02.html)
* [2009年11月 (14)](http://www.cnitblog.com/rd416/archive/2009/11.html)
* [2009年10月 (1)](http://www.cnitblog.com/rd416/archive/2009/10.html)
* [2009年8月 (3)](http://www.cnitblog.com/rd416/archive/2009/08.html)
* [2009年7月 (2)](http://www.cnitblog.com/rd416/archive/2009/07.html)
* [2009年4月 (4)](http://www.cnitblog.com/rd416/archive/2009/04.html)
* [2009年3月 (5)](http://www.cnitblog.com/rd416/archive/2009/03.html)
* [2009年2月 (4)](http://www.cnitblog.com/rd416/archive/2009/02.html)
* [2009年1月 (3)](http://www.cnitblog.com/rd416/archive/2009/01.html)
* [2008年12月 (2)](http://www.cnitblog.com/rd416/archive/2008/12.html)
* [2008年11月 (13)](http://www.cnitblog.com/rd416/archive/2008/11.html)
* [2008年10月 (17)](http://www.cnitblog.com/rd416/archive/2008/10.html)
* [2008年9月 (14)](http://www.cnitblog.com/rd416/archive/2008/09.html)
* [2008年8月 (4)](http://www.cnitblog.com/rd416/archive/2008/08.html)
* [2008年7月 (5)](http://www.cnitblog.com/rd416/archive/2008/07.html)
* [2007年11月 (4)](http://www.cnitblog.com/rd416/archive/2007/11.html)
* [2007年8月 (5)](http://www.cnitblog.com/rd416/archive/2007/08.html)
* [2007年7月 (33)](http://www.cnitblog.com/rd416/archive/2007/07.html)

# 文章分类

* [AJAX](http://www.cnitblog.com/rd416/category/5684.html)[![]( "Subscribe to AJAX")](http://www.cnitblog.com/rd416/category/5684.html/rss "Subscribe to AJAX")
* [ASP.NET](http://www.cnitblog.com/rd416/category/5688.html)[![]( "Subscribe to ASP.NET")](http://www.cnitblog.com/rd416/category/5688.html/rss "Subscribe to ASP.NET")
* [C#](http://www.cnitblog.com/rd416/category/5689.html)[![]( "Subscribe to C#")](http://www.cnitblog.com/rd416/category/5689.html/rss "Subscribe to C#")
* [CSS](http://www.cnitblog.com/rd416/category/5686.html)[![]( "Subscribe to CSS")](http://www.cnitblog.com/rd416/category/5686.html/rss "Subscribe to CSS")
* [HTML](http://www.cnitblog.com/rd416/category/5690.html)[![]( "Subscribe to HTML")](http://www.cnitblog.com/rd416/category/5690.html/rss "Subscribe to HTML")
* [Javascript](http://www.cnitblog.com/rd416/category/5685.html)[![]( "Subscribe to Javascript")](http://www.cnitblog.com/rd416/category/5685.html/rss "Subscribe to Javascript")
* [SQL](http://www.cnitblog.com/rd416/category/5683.html)[![]( "Subscribe to SQL")](http://www.cnitblog.com/rd416/category/5683.html/rss "Subscribe to SQL")
* [XML](http://www.cnitblog.com/rd416/category/5687.html)[![]( "Subscribe to XML")](http://www.cnitblog.com/rd416/category/5687.html/rss "Subscribe to XML")

# 相册

* [開発リーダー：宋金城](http://www.cnitblog.com/rd416/gallery/5702.html)

### 搜索

*
*  

### 最新评论 [![]()](http://www.cnitblog.com/rd416/CommentsRSS.aspx)

* [1. re: java.net.SocketException: Too many open files 问题的解决办法[未登录]](http://www.cnitblog.com/rd416/archive/2011/11/24/47724.html#76429)
* 不错
* --11
* [2. re: 超好用的css2.0様式表手帳](http://www.cnitblog.com/rd416/archive/2011/04/02/29766.html#73245)
* hkhk
* --xiong
* [3. re: java.net.SocketException: Too many open files 问题的解决办法](http://www.cnitblog.com/rd416/archive/2011/03/08/47724.html#73009)
* 212121
* --212
* [4. re: JAVA读取xml文件中节点值(转)](http://www.cnitblog.com/rd416/archive/2010/10/29/53733.html#70697)
* 怎么不见xml文档呢？
* --menglinxi
* [5. re: 一个比较好用的HashMap的遍历方法](http://www.cnitblog.com/rd416/archive/2010/08/12/47231.html#68079)
* 评论内容较长,点击标题查看
* --lurenjia

### 阅读排行榜

* [1. java.net.SocketException: Too many open files 问题的解决办法(6623)](http://www.cnitblog.com/rd416/archive/2008/08/07/47724.aspx)
* [2. JAVA读取xml文件中节点值(转)(2230)](http://www.cnitblog.com/rd416/archive/2009/01/13/53733.aspx)
* [3. HashMap排序问题(2169)](http://www.cnitblog.com/rd416/archive/2008/08/01/47408.aspx)
* [4. java中的String.split() 中“|”作为分隔符的问题和数组长度问题(1985)](http://www.cnitblog.com/rd416/archive/2008/07/29/47244.aspx)
* [5. GWT开发的8个忠告(转载)(1984)](http://www.cnitblog.com/rd416/archive/2009/01/15/53784.aspx)

### 评论排行榜

* [1. 用javacsv API 来操作csv文件 (4)](http://www.cnitblog.com/rd416/archive/2008/07/29/47248.aspx)
* [2. java 全角半角转换函数(2)](http://www.cnitblog.com/rd416/archive/2008/07/29/47236.aspx)
* [3. java.net.SocketException: Too many open files 问题的解决办法(2)](http://www.cnitblog.com/rd416/archive/2008/08/07/47724.aspx)
* [4. Ajax中Session的使用方法(2)](http://www.cnitblog.com/rd416/archive/2007/07/12/29769.aspx)
* [5. JAVA开发中的乱码问题(1)](http://www.cnitblog.com/rd416/archive/2007/11/01/35707.aspx)
[java.net.SocketException: Too many open files 问题的解决办法](http://www.cnitblog.com/rd416/archive/2008/08/07/47724.html)
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

 
以上只是我的个人见解，希望对大家有所帮助。
posted on 2008-08-07 09:23 [TRE-China R&D](http://www.cnitblog.com/rd416/) 阅读(6624) [评论(2)](http://www.cnitblog.com/rd416/archive/2011/11/24/47724.html#Post)  [编辑](http://www.cnitblog.com/rd416/admin/EditPosts.aspx?postid=47724) [收藏](http://www.cnitblog.com/rd416/AddToFavorite.aspx?id=47724) [引用](http://www.cnitblog.com/rd416/services/trackbacks/47724.aspx) 所属分类: [Linux](http://www.cnitblog.com/rd416/category/5916.html) 、[JAVA](http://www.cnitblog.com/rd416/category/6468.html) ![]()

[]()  []()

[Feedback]()

[]()

[]()[#](http://www.cnitblog.com/rd416/archive/2011/11/24/47724.html#73009 "permalink: re: java.net.SocketException: Too many open files 问题的解决办法") []()re: java.net.SocketException: Too many open files 问题的解决办法
2011-03-08 11:10 | [212](http://0.0.4.188/) 212121  [回复](http://www.cnitblog.com/rd416/archive/2011/11/24/47724.html#post)  [更多评论](http://www.cnitblog.com/comment?author=212 "查看该作者发表过的评论")
[]()  []()
[#](http://www.cnitblog.com/rd416/archive/2011/11/24/47724.html#76429 "permalink: re: java.net.SocketException: Too many open files 问题的解决办法[未登录]") []()re: java.net.SocketException: Too many open files 问题的解决办法[未登录][]()

2011-11-24 12:58 | [11](http://0.0.0.1/)
不错   [回复](http://www.cnitblog.com/rd416/archive/2011/11/24/47724.html#post)  [更多评论](http://www.cnitblog.com/comment?author=11 "查看该作者发表过的评论")
[]()  []()
[刷新评论列表]()

[]() [博问 - 解决您的IT难题](http://q.cnblogs.com/)
IT新闻：
· [印度35美元平板机出师不利](http://news.cnblogs.com/n/132335/)
· [Firefox领先Windows，Chrome领先Linux](http://news.cnblogs.com/n/132334/)
· [物理是一个NP-hard问题](http://news.cnblogs.com/n/132333/)
· [优酷网与CCTV6电影网再签署一年合作协议](http://news.cnblogs.com/n/132332/)
· [Bottlenose：将数据可视化融入社交网络运营中](http://news.cnblogs.com/n/132331/)  [博客园首页](http://www.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [IT问答](http://home.cnblogs.com/q/)  [程序员招聘](http://job.cnblogs.com/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 * ![]() 内容(提交失败后,可以通过“恢复上次提交”恢复刚刚提交的内容)

请输入评论内容 Remember Me?   [登录](http://www.cnitblog.com/login.aspx?ReturnURL=http://www.cnitblog.com/rd416/archive/2011/11/24/47724.html&SourceURL=/rd416/archive/2011/11/24/47724.html)  [使用高级评论](http://www.cnitblog.com/rd416/archive/2011/11/24/47724.html?login=1#Post)  [新用户注册](http://www.cnitblog.com/RequireRegister.aspx)  [返回页首](http://www.cnitblog.com/rd416/archive/2011/11/24/47724.html#Top)  [恢复上次提交](http://www.cnitblog.com/rd416/archive/2011/11/24/47724.html#Post)       [使用Ctrl+Enter键可以直接提交]  [![]()](http://q.cnblogs.com/)  博客园首页随笔：
· [游戏运营技术之---玩家关系管理（PRM）--〉推进虚拟营销](http://www.cnblogs.com/yuyang-DataAnalysis/archive/2012/02/22/2363735.html)
· [建立完善的日期定义表](http://www.cnblogs.com/vvian/archive/2012/02/22/2363867.html)
· [《人月神话》摘要](http://www.cnblogs.com/quanzi/archive/2012/02/22/2347270.html)
· [C++标准编程：虚函数与内联](http://www.cnblogs.com/phquan/archive/2012/02/22/2363834.html)
· [莫蹭网：“黑客”怎样伪造wifi钓你(Windows平台)](http://www.cnblogs.com/darklx/archive/2012/02/22/2363627.html) [博客园](http://www.cnblogs.com/)   [IT新闻](http://news.cnblogs.com/)   [BlogJava](http://www.blogjava.net/)   [博客生活](http://www.cnweblog.com/)   [C++博客](http://www.cppblog.com/)   [PHP博客](http://www.phpweblog.net/)   相关文章:

* [linux下文件夹压缩（专载）](http://www.cnitblog.com/rd416/archive/2009/03/25/55725.html)
* [linux VI常用命令](http://www.cnitblog.com/rd416/archive/2009/01/16/53814.html)
* [Linux命令(日语版)](http://www.cnitblog.com/rd416/archive/2008/10/14/50232.html)
* [Linux知识宝库](http://www.cnitblog.com/rd416/archive/2008/10/14/50166.html)
* [java.net.SocketException: Too many open files 问题的解决办法](http://www.cnitblog.com/rd416/archive/2008/08/07/47724.html)
* [很好的Linux笔记（摘录）](http://www.cnitblog.com/rd416/archive/2007/08/16/31970.html)

Powered by:
[IT博客](http://www.cnitblog.com/) [![]()](http://blogs.clearscreen.com/migs)
Copyright ©2012 TRE-China R&D
