---
layout: post
title: "Apache + Tomcat 2集群 负载平衡(Linux环境) - Derek.Guo BLOG"
categories: 服务器
tags: 
 - 服务器
 - 集群
--- 

# Apache + Tomcat 2集群 负载平衡(Linux环境) - Derek.Guo BLOG - BlogJava

[**Derek.Guo BLOG**](http://www.blogjava.net/envoydada/)

[BlogJava](http://www.blogjava.net/)   [首页](http://www.blogjava.net/envoydada/)   [新随笔](http://www.blogjava.net/envoydada/admin/EditPosts.aspx?opt=1) [联系](http://www.blogjava.net/envoydada/contact.aspx?id=1)   [聚合](http://www.blogjava.net/envoydada/rss)[![]()](http://www.blogjava.net/envoydada/rss)   [管理](http://www.blogjava.net/envoydada/admin/EditPosts.aspx)

随笔-82  评论-31  文章-0  trackbacks-0
[Apache + Tomcat*2集群 负载平衡(Linux环境)]()

Apache + Tomcat*2集群 负载平衡（Linux环境）

说明：一台apache主机，两台tomcat主机

安装JDK、安装Apache、安装Tomcat、配置Apache代理、配置Tomcat集群

一、安装JDK（所有运行Tomcat主机，即web服务器）
  1.下载JDK的bin包，如jdk-1_5_0_02-linux-i586.rpm.bin ，给其添加执行权限，执行#./jdk-1_5_0_02-linux-i586.rpm.bin , 在

当前目录生成rpm安装包，同样给其添加执行权限。 再执行 #rpm -ivh jdk-1_5_0_02-linux-i586.rpm 出现安装协议 按<Enter>接受

即可。
  2.设置环境变量 #vi /etc/profile  在其最后加入
        JAVA_HOME =/ usr / java / jdk1. 5 .0_02
        CLASSPATH = .:$JAVA_HOME / lib:$JAVA_HOME / jre / lib
        PATH = $PATH:$JAVA_HOME / bin:$JAVA_HOME / jre / bin
        export JAVA_HOME CLASSPATH PATH

     保存退出
  3.要使JDK在所有的用户中使用，可以这样：vi /etc/profile.d/java.sh在新的java.sh中输入以下内容：
        #set java environment
        JAVA_HOME=/usr/java/jdk1.5.0_02
        CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
        PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin
        export JAVA_HOME CLASSPATH PATH
     保存退出，然后给java.sh分配权限：chmod 755 /etc/profile.d/java.sh

二、安装Apache（访问代理主机）
  1.下载apache源代码 [http://archive.apache.org/dist/httpd/httpd-2.2.2.tar.gz](http://archive.apache.org/dist/httpd/httpd-2.2.2.tar.gz)

 解压缩 tar fvxz httpd-2.2.2.tar.gz

  2.进入解压后的目录。进行配置：
. / configure  -- prefix =/ usr / apache  -- enable - module = most  -- enable - proxy  -- enable - proxy - ajp  -- enable - forward  -- enable - proxy - connect  -- enable - proxy - http  -- enable - so  -- enable - deflate  -- enable - headers  -- enable - include

上面的配置，用到了其他一些模块，说不定以后会用到，如支持ssi的include模块。

  3.编译（编译如果不成功，确认一下你的linux是否安装有编译所需要的c环境和其他需要的类库）
    make

  4.安装 make install

  5.进入/usr/apache目录，运行apache  ./apachectl -k start

      运行apache后，浏览一下是否运行正常。

    关闭apache ./apachectl -k stop

   6.把apache作为linux的启动就运行服务程序
     执行如下操作：cp /usr/apache/bin/apachectl /etc/rc.d/init.d/httpd
     确认linux以前安装的httpd（apache）不需要了，你可覆盖掉以前apache的httpd文件。
     chkconfig --add httpd
     运行linux的setup，把httpd服务默认设定为自动运行。
     到现在，你就可用另一种方式来启动、关闭apache了。如service httpd start

三、安装tomcat（Web服务器）
    1.下载jakarta-tomcat-5.5.20.tar.gz
      tar zxf jakarta-tomcat-5.5.20.tar.gz 解压文件 （如解压到/usr/local/）
    2.设置环境变量 #vi /etc/profile  添加
  CATALINA_HOME=/usr/local/jakarta-tomcat-5.5.30
  export CATALINA_HOME
      保存退出

    3.修改JVM内存：/bin/catalina.sh 文件
        在下# ----- Execute The Requested Command -----------------
            # Bugzilla 37848: only output this if we have a TTY
              if [ $have_tty -eq 1 ]; then
                 echo "Using CATALINA_BASE:   $CATALINA_BASE"
                 echo "Using CATALINA_HOME:   $CATALINA_HOME"
                 echo "Using CATALINA_TMPDIR: $CATALINA_TMPDIR"
                if [ "$1" = "debug" -o "$1" = "javac" ] ; then
                    echo "Using JAVA_HOME:       $JAVA_HOME"
                else
                   echo "Using JRE_HOME:       $JRE_HOME"
                fi
               fi
            添加以下内容：
                CATALINA_OPTS = " $CATALINA_OPTS -Xms256m -Xmx512m -XX:PermSize=32m  -XX:MaxPermSize=128m $JPDA_OPTS "
                JAVA_OPTS = " $JAVA_OPTS -Djava.awt.headless=true "
                echo  " Using CATALINA_OPTS: $CATALINA_OPTS "
                echo  " Using JAVA_OPTS: $JAVA_OPTS "

    4.运行/usr/local/jakarta-tomcat-5.5.30/bin/startup.sh 启动tomcat服务器 测试是否正常
   
  

四、配置apache代理(适用mod_proxy_ajp.so)
    编辑apache配置文件 #vi /usr/apache/conf/httpd.conf
    1.配置proxy_ajp
 #加载解析模块（windows下，或linux采用动态加载模式下需配置。前面我们的linux编译时把下面的模块嵌入到了apache中 

       ，所以不用再加载）
 LoadModule proxy_module modules/mod_proxy.so
 LoadModule proxy_ajp_module modules/mod_proxy_ajp.so
    2.配置文件添加
 ProxyPass  /  balancer: // tomcatcluster/ lbmethod=byrequests stickysession=JSESSIONID nofailover=on timeout=5  maxattempts = 3
 ProxyPassReverse  /  balancer: // tomcatcluster/
  < Proxy balancer: // tomcatcluster >
    BalancerMember ajp: // 192.168.40.15:8009 smax=2 loadfactor=1  route=tomca t1
    BalancerMember ajp: // 192.168.71.106:8009 smax=2 loadfactor=2  route=tomc at2
  </ Proxy >

 

        以上说明请参见mod_proxy中文手册 [http://www.6bee.com/tech/ApacheMenu/mod/mod_proxy.html](http://www.6bee.com/tech/ApacheMenu/mod/mod_proxy.html)
    3.其他说明

 1、apache对tomcat的支持历史：apache第2.1版本后，内置了proxy_ajp，而jk2已经没人开发了，jk则支持到apache的

2.0.58版本。
proxy_ajp配置较简单，但可配置性还不如jk2，主要表现在proxy_ajp目前只支持配置到目录，还不支持对文件名称的pattern模式匹

配（即还不能定义到只对jsp文件起作用）。

 2、因为proxy_ajp的配置，还不支持对文件名称的pattern模式匹配，所以你要特别注意：
——尽量把jsp和静态文件和图片路径分不同的目录来管理；
——对于静态文件和图片路径，如/images，你可用“ProxyPass /images !”来禁止ProxyPass，从而来让apache来直接处理图片的请

求。
——关于apache的ssi（即shtml，include）与tomcat的集成时，shtml文件不能处于ProxyPass的控制下（即不能在ProxyPass目录）

，而shtml调用的jsp须在ProxyPass有效控制下；

五、配置Tomcat负载均衡、集群
    1.修改tomcat 的 conf/server.xml 的<Engine>     
    去掉注释<Engine name="Standalone" defaultHost="localhost" jvmRoute="tomcat1">
        jvmRoute是tomcat路由标示，由此区分两台tomcat主机，那么第二台就改为
            <Engine name="Standalone" defaultHost="localhost" jvmRoute="tomcat2">
    加上注释<Engine name="Catalina" defaultHost="localhost">
   
    2.修改tomcat 的 conf/server.xml 的<Connector> 
    去掉注释<Connector port="8009"
               enableLookups="false" redirectPort="8443" debug="0"
               protocol="AJP/1.3" />   

    3.修改tomcat 的 conf/server.xml 的<Cluster>   
    <!--  
         < Cluster className = " org.apache.catalina.cluster.tcp.SimpleTcpCluster "
                 managerClassName = " org.apache.catalina.cluster.session.DeltaManager "
                 expireSessionsOnShutdown = " false "
                 useDirtyFlag = " true "
                 notifyListenersOnReplication = " true " >
             < Membership 
                className = " org.apache.catalina.cluster.mcast.McastService "
                mcastAddr = " 228.0.0.4 "
                mcastPort = " 45564 "
                mcastFrequency = " 500 "
                mcastDropTime = " 3000 " />
             < Receiver 
                className = " org.apache.catalina.cluster.tcp.ReplicationListener "
                tcpListenAddress = " auto "
                tcpListenPort = " 4001 "
                tcpSelectorTimeout = " 100 "
                tcpThreadCount = " 6 " />
             < Sender
                className = " org.apache.catalina.cluster.tcp.ReplicationTransmitter "
                replicationMode = " pooled "
                ackTimeout = " 5000 " />
             < Valve className = " org.apache.catalina.cluster.tcp.ReplicationValve "
                   filter = " .*\.gif;.*\.js;.*\.jpg;.*\.png;.*\.htm;.*\.html;.*\.css;.*\.txt; " />
                   
             < Deployer className = " org.apache.catalina.cluster.deploy.FarmWarDeployer "
                      tempDir = " /tmp/war-temp/ "
                      deployDir = " /tmp/war-deploy/ "
                      watchDir = " /tmp/war-listen/ "
                      watchEnabled = " false " />
                      
             < ClusterListener className = " org.apache.catalina.cluster.session.ClusterSessionListener " />
         </ Cluster >
         -->

把上面的注释拿掉 就ok 了！

4.在每个webapps应用中,修改web.xml文件 添加元素<distributable/>
最后完工，重启tomcat,apahce测试平衡负载，新建jsp页面
<%
Runtime lRuntime = Runtime.getRuntime();
out.println("*** BEGIN MEMORY STATISTICS ***<br/>");
out.println("Free  Memory: "+lRuntime.freeMemory()/1024/1024+"M<br/>");
out.println("Max   Memory: "+lRuntime.maxMemory()/1024/1024+"M<br/>");
out.println("Total Memory: "+lRuntime.totalMemory()/1024/1024+"M<br/>");
out.println("Available Processors : "+lRuntime.availableProcessors()+"<br/>");
out.println("*** END MEMORY STATISTICS ***");
%>
<br>
<%= request.getSession().getId() %>
放入到两台tomcat的ROOT目录中测试
再测试集群(session复制)
posted on 2006-11-15 11:06 [Derek.Guo](http://www.blogjava.net/envoydada/) 阅读(3196) [评论(0)](http://www.blogjava.net/envoydada/archive/2006/11/15/81196.html#Post)  [编辑](http://www.blogjava.net/envoydada/admin/EditPosts.aspx?postid=81196)  [收藏](http://www.blogjava.net/envoydada/AddToFavorite.aspx?id=81196) 所属分类: [Linux/Unix](http://www.blogjava.net/envoydada/category/32100.html) ![]()

[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [找优秀程序员，就在博客园](http://job.cnblogs.com/)
IT新闻：
· [开源Tizen系统视频泄露 外观似诺基亚MeeGo](http://news.cnblogs.com/n/128015/)
· [Opera推出电视应用商店 提供HTML5应用](http://news.cnblogs.com/n/128014/)
· [CES 2012 开展前演讲：电子设备消费将达 1 万亿美元](http://news.cnblogs.com/n/128013/)
· [分析师称苹果借转售NAND闪存获数十亿美元利润](http://news.cnblogs.com/n/128012/)
· [爱创会：火花，火焰，火光](http://news.cnblogs.com/n/128011/)   [博客园](http://www.cnblogs.com/)  [博问](http://q.cnblogs.com/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/envoydada/archive/2006/11/15/81196.html&SourceURL=/envoydada/archive/2006/11/15/81196.html)       [使用Ctrl+Enter键可以直接提交]    推荐职位：
· [北京C++游戏服务端研发工程师(武神世纪网络)](http://job.cnblogs.com/Offer/18582/)
· [知识库技术编辑(博客园)](http://job.cnblogs.com/offer/16502/)
· [北京Web系统工程师(嘉康利中国)](http://job.cnblogs.com/offer/18594/)
· [.NET 高级软件开发工程师(5173.com)](http://job.cnblogs.com/offer/13272/)
· [中高级.NET程序员(沪江网)](http://job.cnblogs.com/offer/10223/)
· [石家庄高级.NET工程师（月薪6K-8K）(盛安德科技)](http://job.cnblogs.com/Offer/13754/)

博客园首页随笔：
· [再论 Time stamp counter](http://www.cnblogs.com/ralphjzhang/archive/2012/01/09/2317463.html)
· [我在ZZ这八年](http://www.cnblogs.com/default/archive/2012/01/09/2317506.html)
· [写在2012里的2011总结](http://www.cnblogs.com/phphuaibei/archive/2012/01/09/2317484.html)
· [高效程序员秘籍（8）：养成使用网络笔记本、网络文件同步工具的习惯](http://www.cnblogs.com/west-link/archive/2012/01/09/2317477.html)
· [Ext.get()与Ext.fly()之区别](http://www.cnblogs.com/judgelee/archive/2012/01/09/2317474.html)
知识库：
· [持续集成之“Everything is code”](http://kb.cnblogs.com/page/127846/)
· [持续集成之“软件自我识别”](http://kb.cnblogs.com/page/127845/)
· [持续集成之戏说Check-in Dance](http://kb.cnblogs.com/page/127843/)
· [什么是闭包，我的理解](http://kb.cnblogs.com/page/111014/)
· [什么是闭包(Closure)？](http://kb.cnblogs.com/page/111780/)  网站导航:

[博客园](http://www.cnblogs.com/ "程序员的网上家园")   [IT新闻](http://news.cnblogs.com/)   [知识库](http://kb.cnblogs.com/)   [C++博客](http://www.cppblog.com/)   [程序员招聘](http://job.cnblogs.com/)   [管理](http://www.blogjava.net/envoydada/archive/2006/11/15/81196.html?opt=admin) 相关文章:

* [使用 sendfile() 提升网络文件发送性能](http://www.blogjava.net/envoydada/archive/2011/12/29/367529.html)
* [Web服务器性能/压力测试工具http_load、webbench、ab、Siege(转)](http://www.blogjava.net/envoydada/archive/2011/03/15/346300.html)
* [liunx下安装Subversion](http://www.blogjava.net/envoydada/archive/2010/04/08/317711.html)
* [Window下配置SVN服务器与客户端(转)](http://www.blogjava.net/envoydada/archive/2008/07/29/218323.html)
* [Solaris系统进程的查看和管理](http://www.blogjava.net/envoydada/archive/2008/06/11/207028.html)
* [Solaris 操作系统的动态跟踪工具Dtrace](http://www.blogjava.net/envoydada/archive/2007/09/29/149640.html)
* [Linux自启Tomcat,Solaris自启glassfish](http://www.blogjava.net/envoydada/archive/2007/08/01/133838.html)
* [JProfiler远程监控Tomcat](http://www.blogjava.net/envoydada/archive/2007/07/30/133285.html)
* [Glassfish的安装](http://www.blogjava.net/envoydada/archive/2007/07/24/132080.html)
* [启动Solaris10-webmin](http://www.blogjava.net/envoydada/archive/2007/07/10/129316.html)  

[<]( "Go to the previous month")2006年11月[>]( "Go to the next month")日一二三四五六293031123456[7](http://www.blogjava.net/envoydada/archive/2006/11/07.html)[8](http://www.blogjava.net/envoydada/archive/2006/11/08.html)9[10](http://www.blogjava.net/envoydada/archive/2006/11/10.html)11121314[15](http://www.blogjava.net/envoydada/archive/2006/11/15.html)[16](http://www.blogjava.net/envoydada/archive/2006/11/16.html)1718192021222324252627282930123456789

### 留言簿(7)

* [给我留言](http://www.blogjava.net/envoydada/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/envoydada/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/envoydada/admin/MyMessages.aspx)

# 随笔分类(80)

* [Cache(2)](http://www.blogjava.net/envoydada/category/45572.html)[![]( "Subscribe to Cache(2)")](http://www.blogjava.net/envoydada/category/45572.html/rss "Subscribe to Cache(2)")
* [Database(9)](http://www.blogjava.net/envoydada/category/32101.html)[![]( "Subscribe to Database(9)")](http://www.blogjava.net/envoydada/category/32101.html/rss "Subscribe to Database(9)")
* [Java(51)](http://www.blogjava.net/envoydada/category/32099.html)[![]( "Subscribe to Java(51)")](http://www.blogjava.net/envoydada/category/32099.html/rss "Subscribe to Java(51)")
* [Linux/Unix(14)](http://www.blogjava.net/envoydada/category/32100.html)[![]( "Subscribe to Linux/Unix(14)")](http://www.blogjava.net/envoydada/category/32100.html/rss "Subscribe to Linux/Unix(14)")
* [NoSqlDB(4)](http://www.blogjava.net/envoydada/category/45571.html)[![]( "Subscribe to NoSqlDB(4)")](http://www.blogjava.net/envoydada/category/45571.html/rss "Subscribe to NoSqlDB(4)")

# 技术网站

* [54chen](http://www.54chen.com/)
* [Coreseek中文全文检索](http://www.coreseek.com/)
* [Dominic-Blog](http://dominic.blog.chinaunix.net/)
* [GFlot](http://code.google.com/p/gflot/)
* GWT charting library http://repository.jboss.org/maven2/ca/nanometrics/gflot/1.0.0/
* [GWT Showcase](http://gwt.google.com/samples/Showcase/Showcase.html)
* [JavaCC、解析树和 XQuery 语法](http://www.ibm.com/developerworks/cn/xml/x-javacc/part1/)
* [Mongodb手册](http://www.mongodb.org/display/DOCS/Manual)
* [MySQL 5.1参考手册](http://doc.mysql.cn/mysql5/refman-5.1-zh.html-chapter/)
* [Mysql部落](http://www.mysqlsystems.com/)
* [Nginx 的中文维基](http://wiki.nginx.org/NginxChs)
* [Redis](http://redis.io/)
* [Sphinxsearch](http://sphinxsearch.com/)
* [spymemcached](http://code.google.com/p/spymemcached/)
* A simple, asynchronous, single-threaded memcached client written in java.
* [Spymemcached](http://code.google.com/p/spymemcached/)
* [Tigase](http://www.tigase.org/)
* 轻量高性能JABBER/XMPP服务器，带GWT开发的客户端
* [Tomcat 系统架构与设计模式](http://www.ibm.com/developerworks/cn/java/j-lo-tomcat1/index.html)
* [xmemcached](http://code.google.com/p/xmemcached/)
* Extreme performance modern memcached client for java
* [播布客](http://www.boobooke.com/bbs/index.php)
* [百度文库浏览器分析及实现](http://blog.csdn.net/chinull/archive/2010/03/17/5390830.aspx)
* [红联Linux](http://www.linuxdiyf.com/)

### 积分与排名

* 积分 - 107622
* 排名 - 186

### 最新随笔

* [1. 使用 sendfile() 提升网络文件发送性能](http://www.blogjava.net/envoydada/archive/2011/12/29/367529.html)
* [2. Web服务器性能/压力测试工具http_load、webbench、ab、Siege(转)](http://www.blogjava.net/envoydada/archive/2011/03/15/346300.html)
* [3. Magent：Memcached集群代理](http://www.blogjava.net/envoydada/archive/2010/07/13/325947.html)
* [4. Mongodb Import Export Tools](http://www.blogjava.net/envoydada/archive/2010/07/05/325305.html)
* [5. Mongodb dbshell Reference](http://www.blogjava.net/envoydada/archive/2010/07/05/325266.html)
* [6. 转mongodb入门](http://www.blogjava.net/envoydada/archive/2010/06/23/324266.html)
* [7. Mongodb Dynamic querys select](http://www.blogjava.net/envoydada/archive/2010/06/23/324255.html)
* [8. mysql常用的hint](http://www.blogjava.net/envoydada/archive/2010/04/08/317718.html)
* [9. Mysql innodb引擎优化](http://www.blogjava.net/envoydada/archive/2010/04/08/317716.html)
* [10. J2SE6 分析工具](http://www.blogjava.net/envoydada/archive/2010/04/08/317713.html)
* [11. liunx下安装Subversion](http://www.blogjava.net/envoydada/archive/2010/04/08/317711.html)
* [12. ORACLE 中dbms_stats的使用](http://www.blogjava.net/envoydada/archive/2009/02/07/253698.html)
* [13. Memcached 剖析(转)](http://www.blogjava.net/envoydada/archive/2008/09/28/231708.html)
* [14. Window下配置SVN服务器与客户端(转)](http://www.blogjava.net/envoydada/archive/2008/07/29/218323.html)
* [15. Oracle 10g Recycle Bin](http://www.blogjava.net/envoydada/archive/2008/06/18/208808.html)
* [16. Oracle中分区表的使用](http://www.blogjava.net/envoydada/archive/2008/06/16/208221.html)
* [17. ORACLE CTXCAT-CATSEARCH](http://www.blogjava.net/envoydada/archive/2008/06/12/207472.html)
* [18. ORACLE JOBS](http://www.blogjava.net/envoydada/archive/2008/06/12/207288.html)
* [19. Oracle 分区表(Partition)](http://www.blogjava.net/envoydada/archive/2008/06/11/207090.html)
* [20. Solaris系统进程的查看和管理](http://www.blogjava.net/envoydada/archive/2008/06/11/207028.html)

### 最新评论 [![]()](http://www.blogjava.net/envoydada/CommentsRSS.aspx)

* [1. re: Hibernate 本地SQL查询SQLQuery](http://www.blogjava.net/envoydada/archive/2011/07/13/106322.html#354291)
* 不错,很受用
* --happytjn
* [2. re: DES加密](http://www.blogjava.net/envoydada/archive/2010/05/21/46964.html#321538)
* 评论内容较长,点击标题查看
* --woxiangbo
* [3. re: JAVA缩放图片（转贴）](http://www.blogjava.net/envoydada/archive/2010/04/07/138549.html#317660)
* 希望能用
* --moguji
* [4. re: Hibernate 本地SQL查询SQLQuery[未登录]](http://www.blogjava.net/envoydada/archive/2009/10/20/106322.html#299083)
* 很好，谢谢啦
* --xx
* [5. re: Hibernate 本地SQL查询SQLQuery](http://www.blogjava.net/envoydada/archive/2009/09/30/106322.html#296990)
* 很不错啊， 谢谢。。
* --carvin

### 阅读排行榜

* [1. Hibernate 本地SQL查询SQLQuery(11576)](http://www.blogjava.net/envoydada/archive/2007/03/26/106322.html)
* [2. Hibernate批量更新和批量删除(8941)](http://www.blogjava.net/envoydada/archive/2005/09/13/12894.html)
* [3. Spring+Hibernate+Struts(4960)](http://www.blogjava.net/envoydada/archive/2006/05/10/45438.html)
* [4. JProfiler远程监控Tomcat(4646)](http://www.blogjava.net/envoydada/archive/2007/07/30/133285.html)
* [5. Spring DataSource注入(3902)](http://www.blogjava.net/envoydada/archive/2006/11/07/79516.html)
* [6. Spring+hibernate分页查询(3677)](http://www.blogjava.net/envoydada/archive/2007/03/06/102152.html)
* [7. Spring Hibernate 模板实现分页(3500)](http://www.blogjava.net/envoydada/archive/2006/08/28/66182.html)
* [8. Java调用Linux命令(3496)](http://www.blogjava.net/envoydada/archive/2007/05/08/115877.html)
* [9. Hibernate one-to-many学习笔记(3431)](http://www.blogjava.net/envoydada/archive/2006/09/07/68308.html)
* [10. Apache + Tomcat*2集群 负载平衡(Linux环境)(3196)]()
* [11. ORACLE 中dbms_stats的使用(3084)](http://www.blogjava.net/envoydada/archive/2009/02/07/253698.html)
* [12. java虚拟机参数详解(3009)](http://www.blogjava.net/envoydada/archive/2007/07/23/131803.html)
* [13. WEB定时器-Timer(2151)](http://www.blogjava.net/envoydada/archive/2006/04/26/43355.html)
* [14. 工具分析GC日志(2014)](http://www.blogjava.net/envoydada/archive/2007/07/30/133329.html)
* [15. Struts中logic:iterate标记的使用(1871)](http://www.blogjava.net/envoydada/archive/2005/09/11/12654.html)
* [16. Tomcat内存配置(1823)](http://www.blogjava.net/envoydada/archive/2006/11/10/80309.html)
* [17. JAVA访问LDAP(1709)](http://www.blogjava.net/envoydada/archive/2007/05/09/116195.html)
* [18. Tomcat5.0连接池(1697)](http://www.blogjava.net/envoydada/archive/2005/09/11/12655.html)
* [19. Hibernate-Extension和Middlegen-Hibernate(1692)](http://www.blogjava.net/envoydada/archive/2005/09/11/12679.html)
* [20. Spring配置总结(1663)](http://www.blogjava.net/envoydada/archive/2007/07/20/131410.html)
* [21. EL表达式(1341)](http://www.blogjava.net/envoydada/archive/2006/08/30/66559.html)
* [22. JAVA缩放图片（转贴）(1326)](http://www.blogjava.net/envoydada/archive/2007/08/22/138549.html)
* [23. Hibernate主键生成方式(1285)](http://www.blogjava.net/envoydada/archive/2006/04/20/42153.html)
* [24. JAVA的RSS阅读器(1276)](http://www.blogjava.net/envoydada/archive/2006/08/15/63743.html)
* [25. Tomcat 通过数据库验证的配置方法(BASIC,FORM).(1274)](http://www.blogjava.net/envoydada/archive/2006/11/07/79585.html)
* [26. hibernate二级缓存攻略 Ehcache(转贴)(1224)](http://www.blogjava.net/envoydada/archive/2006/12/15/87862.html)
* [27. Oracle中分区表的使用(1180)](http://www.blogjava.net/envoydada/archive/2008/06/16/208221.html)
* [28. Hibernate3.0批量更新和批量删除(1175)](http://www.blogjava.net/envoydada/archive/2006/03/15/35434.html)
* [29. GC调优(1162)](http://www.blogjava.net/envoydada/archive/2007/07/20/131433.html)
* [30. Hibernate属性延迟加载(1122)](http://www.blogjava.net/envoydada/archive/2007/09/20/146797.html)
* [31. Struts常用标签(1120)](http://www.blogjava.net/envoydada/archive/2006/03/28/37791.html)
* [32. RedHat终端中文乱码解决(1077)](http://www.blogjava.net/envoydada/archive/2006/10/12/74758.html)
* [33. 数据库性能 常用SQL(1070)](http://www.blogjava.net/envoydada/archive/2007/12/27/170920.html)
* [34. Spring 定时器(927)](http://www.blogjava.net/envoydada/archive/2006/06/05/50423.html)
* [35. DES加密(869)](http://www.blogjava.net/envoydada/archive/2006/05/19/46964.html)
* [36. Log4j配置(849)](http://www.blogjava.net/envoydada/archive/2006/01/18/28446.html)
* [37. Glassfish的安装(785)](http://www.blogjava.net/envoydada/archive/2007/07/24/132080.html)
* [38. Java调用windows程序(747)](http://www.blogjava.net/envoydada/archive/2006/05/11/45645.html)
* [39. Hibernate3支持DetachedCriteria(转贴)(718)](http://www.blogjava.net/envoydada/archive/2007/06/08/122881.html)
* [40. Oracle 分区表(Partition)(705)](http://www.blogjava.net/envoydada/archive/2008/06/11/207090.html)
Powered by: [博客园](http://www.cnblogs.com/) 模板提供：[沪江博客](http://blog.hjenglish.com/) Copyright ©2012 Derek.Guo

MSN:envoydada@hotmail.com QQ:34935442
