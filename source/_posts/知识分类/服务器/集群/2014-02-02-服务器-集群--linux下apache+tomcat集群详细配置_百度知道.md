---
layout: post
title: "linux下apache+tomcat集群详细配置_百度知道"
categories: 服务器
tags: 
 - 服务器
 - 集群
--- 

# linux下apache+tomcat集群详细配置_百度知道

### 分享到

* [QQ空间](http://zhidao.baidu.com/question/155502817.html#)
* [新浪微博](http://zhidao.baidu.com/question/155502817.html#)
* [百度搜藏](http://zhidao.baidu.com/question/155502817.html#)
* [人人网](http://zhidao.baidu.com/question/155502817.html#)
* [腾讯微博](http://zhidao.baidu.com/question/155502817.html#)
* [开心网](http://zhidao.baidu.com/question/155502817.html#)
* [腾讯朋友](http://zhidao.baidu.com/question/155502817.html#)
* [百度空间](http://zhidao.baidu.com/question/155502817.html#)
* [豆瓣网](http://zhidao.baidu.com/question/155502817.html#)
* [搜狐微博](http://zhidao.baidu.com/question/155502817.html#)
* [MSN](http://zhidao.baidu.com/question/155502817.html#)
* [QQ收藏](http://zhidao.baidu.com/question/155502817.html#)
* [淘宝](http://zhidao.baidu.com/question/155502817.html#)
* [百度贴吧](http://zhidao.baidu.com/question/155502817.html#)
* [搜狐白社会](http://zhidao.baidu.com/question/155502817.html#)
* [更多...](http://zhidao.baidu.com/question/155502817.html#)

[百度分享](http://zhidao.baidu.com/question/155502817.html#)

[百度首页](http://www.baidu.com/) |  [登录](https://passport.baidu.com/?login)   [注册](http://zhidao.baidu.com/question/155502817.html#)

[![百度知道]()](http://zhidao.baidu.com/)

* [新闻](http://news.baidu.com/)
* [网页](http://www.baidu.com/)
* [贴吧](http://tieba.baidu.com/)
* **知道**
* [MP3](http://mp3.baidu.com/)
* [图片](http://image.baidu.com/)
* [视频](http://video.baidu.com/)
* [百科](http://baike.baidu.com/)
* [文库](http://wenku.baidu.com/)
[帮助](http://www.baidu.com/search/zhidao_help.html) | [设置](http://zhidao.baidu.com/question/155502817.html#)

[![]()](http://www.baidu.com/adrc.php?t=00KZ00000f7WV0m0ECcg0jV6As0de0G50000000000Kb0000Tgm1Ej.THLA4nEe_Xa11P5EYIf0UWdGpv4EIsK15Hm4uhDzuAn1P164PAFWPhf0IHYkPYDznRDLnjIKrDckn1nvPWFafHNjrRDzwW6krH0zffK95ywlnAkwn7-_R-wPU77JuvkiwR-_RgGFU77lHykiwDd_RHKNU77JpvkwN7N_RgGFU7FDRg-PwDN3iL-yw7FGNbuPXNujHYPywN7GNbwPpNuDHg-ywRd4NbwiRdujHdPyfb4HNbwPpNu7HbP1U7DsyykiNDd_RyGMU7F7iykwXb-_RgGPU7FDHykwn7NVmLCsnbqgyh9PUNFJHgGWPDqRRh-uX-GoihYkph7gRH-PXbRdH-w7nY4RwyduNj0dyZGKUywZnj-2U-uYR7PpIhPVp1-rfdGlu7I2IhPVp1-2UbF4mNfsXbGVwhVWnYGJR7w7FHPD0ARqpZwYTjCEQLILIz49uvqbmi4WUvY8mv3ETA7zIA4-TMnEIZF9mvVGUhT8mgPsXjqYXgK-5HDhTv-YuNqGujYkPjm1nWm1FMNzUjdCIZwsrBtEILILQh7MUvw9QhPEUi4WUBq9Tv-9Qv9EUhIxpvq8uzqCUv4MgvVEUhT8pZwVUaudIAdxTvqdThP-5yF9pywdFMNYUNqVuywGIyYqwvq_uDdGUhNzFMNYUNqWUv4Yuy4Y5R9EUhT-nWKQUv4Mg1Dvrj03FMNYUNqWmydsmy-MUWdcUv4MFHcsivq8udqZUvkbHy-8ugchIgwVgLw-ThYqiAq8uzRznDVEUhIxP1msQHbsFMw9u1dWPHwbPWDdPBYsmWw9QHf3n10VmH-braYLPHfsuW0duHc1nAm0mLFW5HD4nHfL)[![]()](http://ishop.baidu.com/index.html)

[百度知道](http://zhidao.baidu.com/) > [电脑/网络](http://zhidao.baidu.com/browse/74?lm=2) > [程序设计](http://zhidao.baidu.com/browse/1073?lm=2) > [其他编程语言](http://zhidao.baidu.com/browse/93?lm=2)

# linux下apache+tomcat集群详细配置

2010-5-26 14:17 提问者：[ycdxg](http://passport.baidu.com/?business&aid=6&un=ycdxg#2)   | 浏览次数：2607次

2010-5-28 00:22  最佳答案

环境： 操作系统均为：CentOS 5.1 Apache2.X服务器一台：IP地址192.168.232.4；安装路径/usr/local/apache； Tomcat6服务器一台：IP地址192.168.232.5；安装路径/usr/local/tomcat； Tomcat6服务器一台：IP地址192.168.232.6；安装路径/usr/local/tomcat； 配置： Apache安装： #./configure --prefix=/usr/local/apache --enable-modules=so --enable-mods-shared=all --enable-proxy --enable-proxy-connect --enable-proxy-ftp --enable-proxy-http --enable-proxy-ajp --enable-proxy-balancer --enable-rewrite 注释：激活tomcat集群需要的 enable-proxy，enable-proxy-http，enable-proxy-connect，enable-proxy-ajp和enable-proxy-balancer，其中proxy-ajp和proxy-balancer必须依赖proxy，如果是自定义的编译除了以上几个必须的模块外，mod_status也要编译进去，切记。enable-proxy-ftp可以不编译。 #make;make install 制作Apache启动项： #cp support/apachectl /etc/rc.d/init.d/httpd #vi /etc/rc.d/init.d/httpd 添加以下内容：（包括＃号） # Startup script for the Apache Web Server # chkconfig: 2345 85 15 # description: Apache is a World Wide Web server .It is used to server # HTML files and CGI. # processname: httpd # pidfile: /usr/local/apache/log/httpd.pid # config: /usr/local/apache/conf/httpd.conf 增加服务项 #chkconfig --add httpd #chmod 755 /etc/rc.d/init.d/httpd #chkconfig --level 345 httpd on JDK安装： #chmod a+x jdk-6u4-linux-i586-rpm.bin #./jdk-6u4-linux-i586-rpm.bin JAVA环境变量设置： #vi /etc/profile 在文件最后添加以下内容： JAVA_HOME=/usr/java/jdk1.6.0_04 CLASSPATH=.:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar PATH=$JAVA_HOME/bin:$PATH CATALINA_HOME=/usr/local/tomcat export JAVA_HOME CLASSPATH PATH CATALINA_HOME 执行如下命令使环境变量生效： source /etc/profile 测试配置是否成功： java –version Tomcat安装： #wget [url][http://apache.mirror.phpchina.com/tomcat/tomcat-6/v6.0.16/bin/apache-tomcat-6.0.16.tar.gz](http://apache.mirror.phpchina.com/tomcat/tomcat-6/v6.0.16/bin/apache-tomcat-6.0.16.tar.gz)[/url] #tar zxvf apache-tomcat-6.0.16.tar.gz #mv apache-tomcat-6.0.16 /usr/local/tomcat Tomcat随机启动： #vi /etc/rc.local 添加以下内容： /usr/local/tomcat/bin/startup.sh tomcat6配置文件server.xml： 把 <!-- You should set jvmRoute to support load-balancing via AJP ie : <Engine name="Standalone" defaultHost="localhost" jvmRoute="jvm1"> --> <Engine name="Catalina" defaultHost="localhost"> 改成 <!-- You should set jvmRoute to support load-balancing via AJP ie : --> <Engine name="Standalone" defaultHost="localhost" jvmRoute="tomcatX"> <!-- <Engine name="Catalina" defaultHost="localhost"> --> 说明： 第一台tomcat就把jvmRoute="tomcat1" 第二台tomcat就把jvmRoute="tomcat2" 把 <!-- <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/> --> 去掉注释变为 <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/> ***群集详细配置*** <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster" channelSendOptions="8"> <Manager className="org.apache.catalina.ha.session.DeltaManager" expireSessionsOnShutdown="false" notifyListenersOnReplication="true"/> <Channel className="org.apache.catalina.tribes.group.GroupChannel"> <Membership className="org.apache.catalina.tribes.membership.McastService" address="228.0.0.4" port="45564" frequency="500" dropTime="3000"/> <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver" address="auto" port="4000" autoBind="100" selectorTimeout="5000" maxThreads="6"/> <Sender className="org.apache.catalina.tribes.transport.ReplicationTransmitter"> <Transport className="org.apache.catalina.tribes.transport.nio.PooledParallelSender"/> </Sender> <Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector"/> <Interceptor className="org.apache.catalina.tribes.group.interceptors.MessageDispatch15Interceptor"/> </Channel> <Valve className="org.apache.catalina.ha.tcp.ReplicationValve" filter=""/> <Valve className="org.apache.catalina.ha.session.JvmRouteBinderValve"/> <Deployer className="org.apache.catalina.ha.deploy.FarmWarDeployer" tempDir="/tmp/war-temp/" deployDir="/tmp/war-deploy/" watchDir="/tmp/war-listen/" watchEnabled="false"/> <ClusterListener className="org.apache.catalina.ha.session.JvmRouteSessionIDBinderListener"/> <ClusterListener className="org.apache.catalina.ha.session.ClusterSessionListener"/> </Cluster> 配置应用的web.xml： 在每个webapps应用中,修改配置文件web.xml文件 添加元素<distributable/> 在web.xml文件中<web-app>元素下增加以下内容： <!--此应用将与群集服务器复制Session--> <distributable/> 具体修改如下: 修改前: <?xml version="1.0" encoding="ISO-8859-1"?> <web-app xmlns="[url][http://java.sun.com/xml/ns/javaee](http://java.sun.com/xml/ns/javaee)[/url]" xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)" xsi:schemaLocation="[http://java.sun.com/xml/ns/javaee](http://java.sun.com/xml/ns/javaee) [url][http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd](http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd)[/url]" version="2.5"> </web-app> 修改后: <?xml version="1.0" encoding="ISO-8859-1"?> <web-app xmlns="[url][http://java.sun.com/xml/ns/javaee](http://java.sun.com/xml/ns/javaee)[/url]" xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)" xsi:schemaLocation="[http://java.sun.com/xml/ns/javaee](http://java.sun.com/xml/ns/javaee) [url][http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd](http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd)[/url]" version="2.5"> <!--此应用将与群集服务器复制Session--> <distributable/> </web-app> 配置apache的ajp负载均衡功能： 确保将以下Module的注释去掉 LoadModule proxy_module modules/mod_proxy.so LoadModule proxy_connect_module modules/mod_proxy_connect.so LoadModule proxy_ftp_module modules/mod_proxy_ftp.so LoadModule proxy_http_module modules/mod_proxy_http.so LoadModule proxy_ajp_module modules/mod_proxy_ajp.so LoadModule proxy_balancer_module modules/mod_proxy_balancer.so LoadModule status_module modules/mod_status.so 增加以下内容： # Proxypass Config Include conf/extra/httpd-modproxy.conf 建立文件httpd-modproxy.conf输入内容： <Location /server-status> SetHandler server-status Order Deny,Allow Deny from all Allow from all </Location> <Location /balancer-manager> SetHandler balancer-manager Order Deny,Allow Deny from all Allow from all </Location> ProxyRequests Off ProxyPass / balancer://tomcatcluster stickysession=jsessionid nofailover=On <Proxy balancer://tomcatcluster> BalancerMember [url][http://192.168.232.5:8080](http://192.168.232.5:8080/)[/url] loadfactor=1 BalancerMember [url][http://192.168.232.6:8080](http://192.168.232.6:8080/)[/url] loadfactor=2 </Proxy> 注释： ProxyRequests Off 表示启用反向代理，必须开启； ProxyPass为代理转发的Url,即将所有访问/的请求转发到群集balancer://tomcatcluster，这里为/即将所有访问/的请求转发到群集balancer://tomcatcluster的/test目录； BalancerMember为群集的成员,即群集服务器1或2,负载均衡服务器会根据均衡规则来将请求转发给BalancerMember； 调试负载均衡集群系统： 访问apache服务器的web服务：[url][http://192.168.232.4/balancer-manager](http://192.168.232.4/balancer-manager)[/url] 如果显示负载均衡有关信息则说明成功了，接着可以访问[url][http://192.168.232.4/](http://192.168.232.4/)[/url]即访问到了tomcat的应用 ***必须先启动Tomcat服务再启动Apache服务！*** 参考文档： [url][http://tomcat.apache.org/tomcat-6.0-doc/cluster-howto.html](http://tomcat.apache.org/tomcat-6.0-doc/cluster-howto.html)[/url] [url][http://tomcat.apache.org/tomcat-6.0-doc/balancer-howto.html](http://tomcat.apache.org/tomcat-6.0-doc/balancer-howto.html)[/url] [url][http://man.chinaunix.net/newsoft/ApacheMenual_CN_2.2new/mod/mod_proxy.html](http://man.chinaunix.net/newsoft/ApacheMenual_CN_2.2new/mod/mod_proxy.html)[/url] [url][http://man.chinaunix.net/newsoft/ApacheMenual_CN_2.2new/mod/mod_proxy_balancer.html](http://man.chinaunix.net/newsoft/ApacheMenual_CN_2.2new/mod/mod_proxy_balancer.html)[/url]

赞同

5 | 评论(1)
[![]()](http://passport.baidu.com/?business&aid=6&un=xie8571756#2)

[向TA求助](http://zhidao.baidu.com/question/155502817.html#)

回答者： [xie8571756](http://passport.baidu.com/?business&aid=6&un=xie8571756#2)  | [四级](http://www.baidu.com/search/zhidao_help.html#如何选择头衔)采纳率：43%

擅长领域： [医疗健康](http://zhidao.baidu.com/browse/79) [军事](http://zhidao.baidu.com/browse/211) [奥运/赛事](http://zhidao.baidu.com/browse/999) [羽毛球](http://zhidao.baidu.com/browse/996) [历史话题](http://zhidao.baidu.com/browse/761)

参加的活动： 暂时没有参加的活动

提问者对于答案的评价：
谢谢，非常详细
**相关内容**

* 2009-11-26 [linux下tomcat.apache.jdk之间是什么关系。需要配置哪些变量呢](http://zhidao.baidu.com/question/126116093.html?fr=qrl&cid=93&index=1)
* 2010-8-3 [求在linux平台下整合tomcat+apache的详细步骤！](http://zhidao.baidu.com/question/170016262.html?fr=qrl&cid=93&index=2)
* 2011-5-22 [linux下apache与tomcat整合](http://zhidao.baidu.com/question/262930090.html?fr=qrl&cid=93&index=3) ![该问题包含图片]( "该问题包含图片")
* 2009-11-30 [Linux下apache和tomcat整合](http://zhidao.baidu.com/question/118597309.html?fr=qrl&cid=93&index=4)  1
* 2010-11-1 [linux下 LAMP和apache tomcat 都是 构建服务器用的？用他们构建的...](http://zhidao.baidu.com/question/192108186.html?fr=qrl&cid=93&index=5)  2
[更多相关问题>>](http://zhidao.baidu.com/q?word=linux%CF%C2apache%2Btomcat%BC%AF%C8%BA%CF%EA%CF%B8%C5%E4%D6%C3&ct=17&pn=0&tn=ikaslist&rn=10&fr=qrl&cid=93)

查看同主题问题： [linux](http://zhidao.baidu.com/topic?ct=29&tn=iktopic&word=linux&fr=rtag&cid=93&index=1) [集群](http://zhidao.baidu.com/topic?ct=29&tn=iktopic&word=%BC%AF%C8%BA&fr=rtag&cid=93&index=2) [配置](http://zhidao.baidu.com/topic?ct=29&tn=iktopic&word=%C5%E4%D6%C3&fr=rtag&cid=93&index=3)

**等待您来回答**

* 0回答20[北京集体户口的复印件一定要本人去开么？](http://zhidao.baidu.com/question/364523566.html?push=cookie "北京集体户口的复印件一定要本人去开么？")
* 0回答[学校的集体户口，迁移证已经开出，如何落户？](http://zhidao.baidu.com/question/364512535.html?push=cookie "学校的集体户口，迁移证已经开出，如何落户？")
* 0回答[我户口在江西，老婆户口在天津北辰人才集体户口，我们现在北京工作。...](http://zhidao.baidu.com/question/364511131.html?push=cookie "我户口在江西，老婆户口在天津北辰人才集体户口，我们现在北京工作。要办理准生证该怎么办理呢？")
* 1回答5[在武汉，我是外地户口，房产证上只有我一个人的名字，能不能把我老婆...](http://zhidao.baidu.com/question/364404321.html?push=cookie "在武汉，我是外地户口，房产证上只有我一个人的名字，能不能把我老婆的集体户口落户啊？")
* 0回答[山东武城县民政局,我是本地人，他也是本地人，但户口转到学校又转到外...](http://zhidao.baidu.com/question/364391011.html?push=cookie "山东武城县民政局,我是本地人，他也是本地人，但户口转到学校又转到外地，现是集体户口，身份证是学校的")
* 0回答[我是2005年毕业来武汉工作的，户口属于公司集体户口，现在09年5月在武...](http://zhidao.baidu.com/question/364387414.html?push=cookie "我是2005年毕业来武汉工作的，户口属于公司集体户口，现在09年5月在武汉买了房子，请问怎么把户口转户口啊")
* 1回答[谁能提供江苏集体户口样本,急啊](http://zhidao.baidu.com/question/364359113.html?push=cookie "谁能提供江苏集体户口样本,急啊")
* 1回答10[我四川内江威远县新场镇的，我老公是洛阳中铁15局的集体户口，我应该...](http://zhidao.baidu.com/question/364315318.html?push=cookie "我四川内江威远县新场镇的，我老公是洛阳中铁15局的集体户口，我应该准备什么资料，特别是我老公的")
[更多等待您来回答的问题>>](http://zhidao.baidu.com/browse/?lm=17)
分享到：

[]( "分享到百度贴吧") []( "分享到新浪微博") []( "分享到腾讯微博") []( "分享到QQ空间") []( "分享到人人网") []( "分享到豆瓣网") []( "分享到Myspace")
    [ ]()

推广链接 [广东栢图广东 linux 培训 保就业 报名送高配置笔记本](http://www.baidu.com/baidu.php?url=a000000_q8Cq_KM03_q__Z0IKYLz9PMc7NEEyXyeUuRBByc_93oYC__B8RqPvH6krpSpMYJZNXlRowBddjgqhpKwi5k_5E4zeDlfWWfdQYWTB-hUUpV2GkI9mAoT.DR_aS9wx76lrijV96vzJgq_H8j-9k1QjPak8LtIz60.U1Yk0ZDqk8HQSeZ6_t1HeqZR0Zfq8XrvJzRznAkGUMN3FHcskXjwVfKGUHYzn160I1Y0u1d3Xj00Iybq0ZKGujYY0APGujY4nsKVIjYs0AVG5H00uy-b5H00Uy-b5H00mLFW5HRdPW6k)
[高起点广东 linux 培训 ,广东省重点实验基地,总投资过亿,师资,设备,教学环境一流,入.. www.btlinux.cn](http://www.baidu.com/baidu.php?url=a000000_q8Cq_KM03_q__Z0IKYLz9PMc7NEEyXyeUuRBByc_93oYC__B8RqPvH6krpSpMYJZNXlRowBddjgqhpKwi5k_5E4zeDlfWWfdQYWTB-hUUpV2GkI9mAoT.DR_aS9wx76lrijV96vzJgq_H8j-9k1QjPak8LtIz60.U1Yk0ZDqk8HQSeZ6_t1HeqZR0Zfq8XrvJzRznAkGUMN3FHcskXjwVfKGUHYzn160I1Y0u1d3Xj00Iybq0ZKGujYY0APGujY4nsKVIjYs0AVG5H00uy-b5H00Uy-b5H00mLFW5HRdPW6k)  [嘉瑞美深圳linux培训培训官方授权培训考试中心](http://www.baidu.com/baidu.php?url=a00000j3FLN_oGRWh2RodbQLI0RHquARgvYnwye98qRSNabTEsWg6EnHlUKnVFVIlqreRnH0pqmrdXLfpdCcnM-NPfmDCe0_sKSXSRrB6mi-gBxsJhyRN7NfERdH.7D_iRDsdXgphUwZGuhCc6REemMTZQjWhyAp7Wu8otN0.U1Yz0ZDqk8HQSeZ6_t1HeqZR0ZfqzXeUvhkGUMN3kXjwVfKGUHYzn160I1Y0u1d3Xj00Iybq0ZKGujYY0APGujY4nsKVIjYs0AVG5H00uy-b5H00Uy-b5H00mLFW5Hbvnjb)
[深圳linux培训首选深圳嘉瑞美 RHCE全球通用认证 金牌红帽认证讲师精心指导0755-2398.. tech.kerysmart.com](http://www.baidu.com/baidu.php?url=a00000j3FLN_oGRWh2RodbQLI0RHquARgvYnwye98qRSNabTEsWg6EnHlUKnVFVIlqreRnH0pqmrdXLfpdCcnM-NPfmDCe0_sKSXSRrB6mi-gBxsJhyRN7NfERdH.7D_iRDsdXgphUwZGuhCc6REemMTZQjWhyAp7Wu8otN0.U1Yz0ZDqk8HQSeZ6_t1HeqZR0ZfqzXeUvhkGUMN3kXjwVfKGUHYzn160I1Y0u1d3Xj00Iybq0ZKGujYY0APGujY4nsKVIjYs0AVG5H00uy-b5H00Uy-b5H00mLFW5Hbvnjb)  [信盈达linux培训,linux培训项目实战教学,包就业](http://www.baidu.com/baidu.php?url=a00000KyIYu87aBhdbO8NrFkLhaSLF4jELJBWF1zJJpaGbRTH14nzM-eo_fSBox89k60yaH-h7h3tk5FybIfR0ue-gTYy5MTc6LIDg_pbyuCWArsN1xVYiVdM4rt.7R_KPva8EmBKiOVBIsT_rrumuCyrr1GlIB6.U1Y10ZDqk8HQSeZ6_t1HeqZR0ZfqUA-8IgW73PAd0A-V5Hc1r0KL5fKM5g93n0KdpHY0TA-b5Hf0mv-b5Hb10AdY5H00pvbqn0K-pyfqn0KVpyfqn0KWThnqnWR1n1R)
[linux培训结合企业真实项目,师傅带徒弟方式教学,linux培训ARM9内核,linux培训移植20.. www.edu118.com](http://www.baidu.com/baidu.php?url=a00000KyIYu87aBhdbO8NrFkLhaSLF4jELJBWF1zJJpaGbRTH14nzM-eo_fSBox89k60yaH-h7h3tk5FybIfR0ue-gTYy5MTc6LIDg_pbyuCWArsN1xVYiVdM4rt.7R_KPva8EmBKiOVBIsT_rrumuCyrr1GlIB6.U1Y10ZDqk8HQSeZ6_t1HeqZR0ZfqUA-8IgW73PAd0A-V5Hc1r0KL5fKM5g93n0KdpHY0TA-b5Hf0mv-b5Hb10AdY5H00pvbqn0K-pyfqn0KVpyfqn0KWThnqnWR1n1R)

用户名：

密码码：

记住我的登录状态

登 录 [忘记密码](http://passport.baidu.com/?getpass_index)

[注册百度账号，遨游知识海洋](http://zhidao.baidu.com/html/user_reg.html?qb=1)

[广东栢图广东 linux 培训 保..](http://www.baidu.com/baidu.php?url=a00000KXas72HJxB1XU7F3kEfEZP2ScSmt1Sv4Abi_AGR-ZAM_Ek7Qw3T6HkO7AtmCTm-64hFI92IwtuBRsE0vmIYHbz3Y6D78MMy_SRFFXEr0pTQz7HB_-0UE9Y.DR_aS9wx76lrijV96vzJgq_H8j-9k1QjPak8LtIz60.U1Yk0ZDqk8HQSeZ6_t1HeqZR0Zfq8XrvJzRznAkGUMN3FHcskXjwVfKGUHYznjT0I1Y0u1d3Xj00Iybq0ZKGujYY0APGujY4nsKVIjYs0AVG5H00uy-b5H00Uy-b5H00mLFW5HDYP1b)
[高起点广东 linux 培训 ,广东省重点实验基地,总投资过亿,师资,设备,教学环境一流,入..
www.btlinux.cn](http://www.baidu.com/baidu.php?url=a00000KXas72HJxB1XU7F3kEfEZP2ScSmt1Sv4Abi_AGR-ZAM_Ek7Qw3T6HkO7AtmCTm-64hFI92IwtuBRsE0vmIYHbz3Y6D78MMy_SRFFXEr0pTQz7HB_-0UE9Y.DR_aS9wx76lrijV96vzJgq_H8j-9k1QjPak8LtIz60.U1Yk0ZDqk8HQSeZ6_t1HeqZR0Zfq8XrvJzRznAkGUMN3FHcskXjwVfKGUHYznjT0I1Y0u1d3Xj00Iybq0ZKGujYY0APGujY4nsKVIjYs0AVG5H00uy-b5H00Uy-b5H00mLFW5HDYP1b)
[嘉瑞美深圳linux培训培训官..](http://www.baidu.com/baidu.php?url=a00000aDniyoCRUNMgrQ61uVl2nxIbaYfO_Ch6A3GxvNUQn4rLOXlosP3bJkB6NcS9w7WQUtgALySfp0E5Btgjj3AhvfGU3smy8Au5_GVU6SEvaER6ldOR6L1bxN.7D_iRDsdXgphUwZGuhCc6REemMTZQjWhyAp7Wu8otN0.U1Yz0ZDqk8HQSeZ6_t1HeqZR0ZfqzXeUvhkGUMN3kXjwVfKGUHYznjT0I1Y0u1d3Xj00Iybq0ZKGujYY0APGujY4nsKVIjYs0AVG5H00uy-b5H00Uy-b5H00mLFW5Hm1rHRk)
[深圳linux培训首选深圳嘉瑞美 RHCE全球通用认证 金牌红帽认证讲师精心指导0755-2398..
tech.kerysmart.com](http://www.baidu.com/baidu.php?url=a00000aDniyoCRUNMgrQ61uVl2nxIbaYfO_Ch6A3GxvNUQn4rLOXlosP3bJkB6NcS9w7WQUtgALySfp0E5Btgjj3AhvfGU3smy8Au5_GVU6SEvaER6ldOR6L1bxN.7D_iRDsdXgphUwZGuhCc6REemMTZQjWhyAp7Wu8otN0.U1Yz0ZDqk8HQSeZ6_t1HeqZR0ZfqzXeUvhkGUMN3kXjwVfKGUHYznjT0I1Y0u1d3Xj00Iybq0ZKGujYY0APGujY4nsKVIjYs0AVG5H00uy-b5H00Uy-b5H00mLFW5Hm1rHRk)
[信盈达linux培训,linux培训..](http://www.baidu.com/baidu.php?url=a0000008AY_4UklMSftRV_Cf4q_fyVHTlRS6DCfAPKu4eQsBKJykMoqgIc83AlwqvX2YSZOfsWnyiLrwHbIxrl5vb_SPYNiWguCuz4a1ySGxOzrl41OKq9nQBaMY.7R_KPva8EmBKiOVBIsT_rrumuCyrr1GlIB6.U1Y10ZDqk8HQSeZ6_t1HeqZR0ZfqUA-8IgW73PAd0A-V5HcsPsKL5fKM5g93n0KdpHY0TA-b5Hf0mv-b5Hb10AdY5H00pvbqn0K-pyfqn0KVpyfqn0KWThnqPj63nHT)
[linux培训结合企业真实项目,师傅带徒弟方式教学,linux培训ARM9内核,linux培训移植20..
www.edu118.com](http://www.baidu.com/baidu.php?url=a0000008AY_4UklMSftRV_Cf4q_fyVHTlRS6DCfAPKu4eQsBKJykMoqgIc83AlwqvX2YSZOfsWnyiLrwHbIxrl5vb_SPYNiWguCuz4a1ySGxOzrl41OKq9nQBaMY.7R_KPva8EmBKiOVBIsT_rrumuCyrr1GlIB6.U1Y10ZDqk8HQSeZ6_t1HeqZR0ZfqUA-8IgW73PAd0A-V5HcsPsKL5fKM5g93n0KdpHY0TA-b5Hf0mv-b5Hb10AdY5H00pvbqn0K-pyfqn0KVpyfqn0KWThnqPj63nHT)
[![]()](http://www.baidu.com/adrc.php?t=000k00000fwWV0m0tZtg0jV6As0de0G50000000000Kb0000Tgm1Ea.THLA4nEe_Xa11P5EYIf0UWdBmy-bIy-oUhqLgvPsT6K15Hm4uhDzuAn1P164PAFWPhf0IHYkPYDznRDLnjIKrDckn1nvPWFafHNjrRDzwW6krH0zffK95ywlnAkwn7-_R-wPU77JuvkiwR-_RgGFU77lHykiwDd_RHKNU77JpvkwN7N_RgGFU7FDRg-PwDN3iL-yw7FGNbuPXNujHYPywN7GNbwPpNuDHg-ywRd4NbwiRdujHdPyfb4HNbwPpNu7HbP1U7DsyykiNDd_RyGMU7F7iykwXb-_RgGPU7FDHykwn7NVmLCsnbqgyh9PUNFJHgGWPDqRRh-uX-GoihYkph7gRH-PXbRdH-woIYdliyduNj0dyZGKUywZnj-2U-uYR7PpIhPVp1-rfdGlu7I2IhPVp1-2UbF4mNfsXbGVwhVWnYGJR7w7FHPD0ARqpZwYTjCEQLILIz49uvqbmi4WUvY8mv3ETA7zIA4-TMnEIZF9mvVGUhT8mgPsXjqYXgK-5HDhTv-YuNqGujYkPjm1nWm1FMNzUjdCIZwsrBtEILILQh7MUvw9QhPEUi4WUBq9Tv-9Qv9EUhIxpvq8uzqCUv4MgvVEUhT8pZwVUaudIAdxTvqdThP-5yF9pywdFMNYUNqVuywGIyYqwvq_uDdGUhNzFMNYUNqWUv4Yuy4Y5R9EUhT-nWKQUv4Mg1Dvrj03FMNYUNqWmydsmy-MUWdcUv4MFHcsivq8udqZUvkbHy-8ugchIgwVgLw-ThYqiAq8uzRznDVEUhIxnWTsQHcYnauYmyTqrj9-nv7hnycVrH0sPiYYuHP-QH63ryRVrAFhP1I-rHFBrAfz0APzm1YdPHb4Ps)[![]()](http://ishop.baidu.com/index.html)
[来百度推广其他编程语言](http://e.baidu.com/)
[![]()](http://zhidao.baidu.com/s/wisdom/index.html?fr=QB_zh_270)

[![]()](http://zhidao.baidu.com/s/weiwenda/index.html?fr=iknow_Qb2)
©2012 Baidu   [使用百度前必读](http://www.baidu.com/duty/) | [知道协议](http://www.baidu.com/search/zhidao_help.html#知道协议) | [百度知道开放平台](http://zhidao.baidu.com/s/open)
