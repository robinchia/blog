---
layout: post
title: "haproxy 解决 多主机session共享问题 的三种方法"
categories: 服务器
tags: 
 - 服务器
 - haproxy
--- 

# haproxy 解决 多主机session共享问题 的三种方法

haproxy 解决 多主机session共享问题 的三种方法

 
1 session知识储备
2 haproxy三种方法保持客户端session一致
3 实验环境及结构
4 安装配置及管理
5 本实验中使用到相同的index.php代码
6 联系方法及扩展阅读
感谢 不就是要我命 QQ 47034839 提供测试主机 
1 session知识储备
Session是由应用服务器维持的一个服务器端的存储空间，用户在连接服务器时，会由服务器生成一个唯一的SessionID,用该SessionID 为标识符来存取服务器端的Session存储空间。
而SessionID这一数据则是保存到客户端，用Cookie保存的，用户提交页面时，会将这一 SessionID提交到服务器端，来存取Session数据。
服务器也通过URL重写的方式来传递SessionID的值，因此不是完全依赖Cookie。如果客户端Cookie禁用，则服务器可以自动通过重写URL的方式来保存Session的值，并且这个过程对程序员透明。
php.ini 里几个session相关值的 其它的值请参考《PHP与Mysql5程序设计》
session.use_cookies = 1  #表示 服务端和客户端交互session是通过cookie的方式 默认值
session.name = 9ai9     #默认值是PHPSESSID 我这里改成9ai9是为了和默认值区别
session.cache_limiter = nocache #此设置确保对每个请求，在可能提供缓存的版本前，先请求发送到最初的服务器。这个值联系到下文中 cookie识别中的相关参数
2 haproxy三种方法保持客户端session一致
  2.1 用户IP 识别 
  
haroxy 将用户IP经过hash计算后 指定到固定的真实服务器上（类似于nginx 的IP hash 指令）
配置指令        balance source
实例访问[http://sourceip.9ai9.net:8080](http://sourceip.9ai9.net:8080/)
  2.2 cookie 识别  
haproxy 将WEB服务端发送给客户端的cookie中插入(或添加加前缀)haproxy定义的后端的服务器COOKIE ID。
配置指令例举  cookie  SESSION_COOKIE  insert indirect nocache
[http://cookie.9ai9.net:8080](http://cookie.9ai9.net:8080/)
用firebug可以观察到用户的请求头的cookie里 有类似" Cookie 9ai9=0bc588656ca05ecf7588c65f9be214f5; SESSION_COOKIE=12" SESSION_COOKIE=12就是haproxy添加的内容
  2.3 session 识别  
haproxy 将后端服务器产生的session和后端服务器标识存在haproxy中的一张表里。客户端请求时先查询这张表。
配置指令例举 appsession 9ai9 len 64 timeout 5h request-learn 
注意 9ai9 这个值替换成 你的php.ini 里session.name的值。
实例访问 [http://appsession.9ai9.net:8080](http://appsession.9ai9.net:8080/)
  2.4 只做简单轮询对比 
实例访问 [http://nosession.9ai9.net:8080](http://nosession.9ai9.net:8080/)
3 实验环境及结构
CentOS 5.3 64
haproxy  113.106.185.245
WEB1 REALsrv_70  184.82.239.70
WEB2  REALsrv_120 220.162.237.120  
4 安装配置及管理
useradd -M -s /sbin/nologin haproxy
wget [http://haproxy.1wt.eu/download/1.4/src/haproxy-1.4.13.tar.gz](http://haproxy.1wt.eu/download/1.4/src/haproxy-1.4.13.tar.gz)
tar zxvf haproxy-1.4.13.tar.gz 
cd haproxy-1.4.13
make TARGET=linux26 PREFIX=/usr/local/haproxy install
mkdir /usr/local/haproxy/conf
vim /usr/local/haproxy/conf/haproxy.cfg

1. global
1.         log     127.0.0.1 local0 info 
1.         maxconn 4096
1.         user    haproxy
1.         group   haproxy
1.         daemon
1.         nbproc  1
1.         pidfile /var/run/haproxy.pid
1. defaults
1.         mode    http
1.         maxconn         2000
1.         contimeout      5000
1.         clitimeout      30000
1.         srvtimeout      30000
1.         option          httplog
1.         option          redispatch
1.         option          abortonclose
1.         retries         3
1. listen admin_stats
1.         bind 113.106.185.245:443
1.         mode http
1.         log 127.0.0.1 local0 err
1.         stats   uri     /qhappy_stats
1.         stats   realm   9ai9.net\ Qhappy
1.         stats   auth    qhappy:qhappy
1.         stats   refresh   5s 
1. listen site_status
1.         bind 113.106.185.245:445
1.         mode http
1.         log  127.0.0.1 local0 err
1.         monitor-uri     /site_status
1. frontend  WEB_SITE
1.         bind    0.0.0.0:8080
1.         mode    http
1.         log     global
1.         option  httplog
1.         option  httpclose
1.         option  forwardfor
1.         acl     COOKIE          hdr_reg(host)   -i ^(cookie.9ai9.net)
1.         acl     SOURCE          hdr_reg(host)   -i ^(sourceip.9ai9.net)
1.         acl     APPSESSION      hdr_reg(host)   -i ^(appsession.9ai9.net)
1.         acl     NOSESSION       hdr_reg(host)   -i ^(nosession.9ai9.net)
1.         use_backend COOKIE_srv          if COOKIE
1.         use_backend SOURCE_srv          if SOURCE
1.         use_backend APPSESSION_srv      if APPSESSION
1.         use_backend NOSESSION_srv       if NOSESSION
1. #        default_backend ai_server
1. backend COOKIE_srv
1.         mode    http
1.         cookie  SESSION_COOKIE  insert indirect nocache
1.        server REALsrv_70       184.82.239.70:80 cookie 11 check inter 1500 rise 3 fall 3 weight 1
1.        server REALsrv_120      220.162.237.120:80 cookie 12 check inter 1500 rise 3 fall 3 weight 1
1. backend SOURCE_srv
1.         mode    http
1.        balance source
1.        server REALsrv_70       184.82.239.70:80 cookie 11 check inter 1500 rise 3 fall 3 weight 1
1.        server REALsrv_120      220.162.237.120:80 cookie 12 check inter 1500 rise 3 fall 3 weight 1
1. backend APPSESSION_srv
1.        mode    http
1.        appsession 9ai9 len 64 timeout 5h request-learn
1.        server REALsrv_70       184.82.239.70:80 cookie 11 check inter 1500 rise 3 fall 3 weight 1
1.        server REALsrv_120      220.162.237.120:80 cookie 12 check inter 1500 rise 3 fall 3 weight 1
1.
1. backend NOSESSION_srv
1.        mode    http
1.         balance roundrobin
1.        server REALsrv_70       184.82.239.70:80 cookie 11 check inter 1500 rise 3 fall 3 weight 1
1.        server REALsrv_120      220.162.237.120:80 cookie 12 check inter 1500 rise 3 fall 3 weight 1
1. backend ai_server
1.        mode    http
1.        balance roundrobin
1.        cookie  SERVERID
1.        server REALsrv_70 184.82.239.70:80 cookie 2 check inter 1500 rise 3 fall 3 weight 1
1.        server REALsrv_120 220.162.237.120:80 cookie 1 check inter 1500 rise 3 fall 3 weight 1
*复制代码*
haproxy 启动重启等管理脚本
cd /etc/init.d/
wget [http://www.9ai9.net/download/shell/haproxy](http://www.9ai9.net/download/shell/haproxy)
chmod 755 haproxy
chkconfig --add haproxy
使用方法 你懂的
/etc/init.d/haproxy {start|stop|status|checkconfig|restart|try-restart|reload|force-reload}
5 本实验中使用到相同的index.php代码 如下
1. <?php
1. session_start();
1. $_SESSION['time'] =date("Y:m:d:H:s",time());
1. echo "本次访问时间"."<font color=red>".$_SESSION['time']."</font>"."<br>";
1. echo "访问的服务器地址是"."<font color=red>".$_SERVER['SERVER_ADDR']."</font>"."<br>";
1. echo "访问的服务器域名是"."<font color=red>".$_SERVER['SERVER_NAME']."</font>"."<br>";
1. echo "SESSIONNAME是"."<font color=red>".session_name()."</font>"."<br>";
1. echo "SESSIONID是"."<font color=red>".session_id()."</font>"."<br>";
1. ?>
*复制代码*6 联系方法及扩展阅读
笔者 水煮鱼@溢  微博 [http://t.qq.com/cllxy1234](http://t.qq.com/cllxy1234) 欢迎收听
haproxy 官网 [http://haproxy.1wt.eu/download/1.4/doc/configuration.txt](http://haproxy.1wt.eu/download/1.4/doc/configuration.txt)
董旗宇  [http://www.9ai9.net/download/art/HAProxy](http://www.9ai9.net/download/art/HAProxy)配置使用说明.pdf   
刘天斯  [http://blog.liuts.com/post/223/](http://blog.liuts.com/post/223/)
linuxtone [http://bbs.linuxtone.org/thread-73-1-1.html](http://bbs.linuxtone.org/thread-73-1-1.html)

来源： <[http://bbs.linuxtone.org/thread-9526-1-1.html](http://bbs.linuxtone.org/thread-9526-1-1.html)>
 
