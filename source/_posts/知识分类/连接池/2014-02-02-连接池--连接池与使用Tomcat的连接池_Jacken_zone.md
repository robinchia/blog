---
layout: post
title: "连接池与使用Tomcat的连接池 _ Jacken_zone"
categories: 连接池
tags: 
 - 连接池
--- 

# 连接池与使用Tomcat的连接池 _ Jacken_zone

## [连接池与使用Tomcat的连接池]()

Filed under: [AppServ](http://www.jacken.com.cn/category/it-technology/appserv/ "显示AppServ的所有日志"), [IT Technology](http://www.jacken.com.cn/category/it-technology/ "显示IT Technology的所有日志"), [JDBC](http://www.jacken.com.cn/category/it-technology/jdbc-java/ "显示JDBC的所有日志") |

Posted on 12月 13th, 2007 由 Jacken
**What is Connection Pool？看图~~**

1)存放Connection对象的容器；
2)减少连接数据库的开销；
3)程序请求连接时，在Connection Pool中取连接；
4)连接使用完后，放回Connection Pool,不释放；
5)Connection Pool对连接进行管理：计数、监控连接状态；

![ConnectionPool-JPool]()

**自己写个连接池？**
**?**一般情况下不要使用自己写的连接池，很多应用提供连接池，它们的更好更安全更专业…

DbConfig.java
下载: [DbConfig.java](http://www.jacken.com.cn/wp-content/plugins/coolcode/coolcode.php?p=73&download=DbConfig.java)

package cn.com.jacken.JPool.javabeans;
public class DbConfig {
private String jdbcDriver;
private String url;
private String userName;
private String password;
public String getJdbcDriver() {
return jdbcDriver;
}
public void setJdbcDriver(String jdbcDriver) {
this.jdbcDriver = jdbcDriver;
}
public String getUrl() {
return url;
}
public void setUrl(String url) {
this.url = url;
}
public String getUserName() {
return userName;
}
public void setUserName(String userName) {
this.userName = userName;
}
public String getPassword() {
return password;
}
public void setPassword(String password) {
this.password = password;
}
}

IJConnectionPool.java
下载: [IJConnectionPool.java](http://www.jacken.com.cn/wp-content/plugins/coolcode/coolcode.php?p=73&download=IJConnectionPool.java)

package cn.com.jacken.JPool;
import java.sql.Connection;
import java.util.Hashtable;
import cn.com.jacken.JPool.javabeans.DbConfig;
public abstract class IJConnectionPool{
protected Hashtable<Connection, String> connectionContainer = newHashtable<Connection, String>();
public abstract void init(int count, DbConfig config) throws Exception;
public abstract Connection getConnection() throws Exception;
public abstract void returnConnection(Connection conn);
}

JConnectionPoolImpl.java
下载: [JConnectionPoolImpl.java](http://www.jacken.com.cn/wp-content/plugins/coolcode/coolcode.php?p=73&download=JConnectionPoolImpl.java)

package cn.com.jacken.JPool;
import java.sql.Connection;
import java.sql.DriverManager;
import cn.com.jacken.JPool.javabeans.DbConfig;
public class JConnectionPoolImplextendsIJConnectionPool {
public JConnectionPoolImpl(int count, DbConfig config) throws Exception {
this.init(count, config);
}
@Override
publicConnectiongetConnection() throws Exception {
Connection conn = null;
Object[] objList = this.connectionContainer.keySet().toArray();
for (Object obj : objList) {
String value = this.connectionContainer.get("obj");
// 判断状态是否为FREE
if (true == value.equals("FREE")) {
// 如果当前Connection状态确实为FREE
conn = (Connection) obj;
// 将实例的状态置为BUSY
this.connectionContainer.put(conn, "BUSY");
break;
}
}
if (null == conn) {
Exception e = new Exception("没有空闲的Connection,请稍候再试!");
throwe;
}
// 返回实例给客户代码
return conn;
}
@Override
publicvoid init(int count, DbConfig config) throws Exception {
// 循环的添加Connection实例到Hashtable中
for (int i = 0; i < count; i++) {
// 产生Connection实例
Class.forName(config.getJdbcDriver());
Connection conn = DriverManager.getConnection(config.getUrl(),
config.getUserName(), config.getPassword());
// 将Connection实例添加到Hashtable中
this.connectionContainer.put(conn, "FREE");
}
}
@Override
publicvoid returnConnection(Connection conn) {
// 将实例的状态置为FREE
this.connectionContainer.put(conn, "FREE");
}
}

**使用Tomcat的连接池**

1，配置连接池：在conf目录下的context.xml中，添加一项(可以配置多项)：
下载: [context.xml](http://www.jacken.com.cn/wp-content/plugins/coolcode/coolcode.php?p=73&download=context.xml)

<Context>
<WatchedResource>WEB-INF/web.xml</WatchedResource>
<!--
name为资源的JNDI查找的名字;
type:资源(一般不变);
maxActive:最大活动链接，如果使用的链接到达这个数目，其后所有的请求连接需要等待；
maxIdle：最大空闲链接，如果超过这个数目，连接池会烧毁多出来的链接；
maxWait：等待时间，请求超过这个时间会抛出异常；
driverClassName：JDBC驱动程序，必须先将其JDBC Driver复制至%TomcatHome%/common/lib下；
username：数据库帐户；
password：数据库帐户密码；
url：URL值
-->
<Resource name="jacken" type="javax.sql.DataSource" maxActive="4"
maxIdle="2" maxWait="5000"
driverClassName="com.mysql.jdbc.Driver"
username="root" password=""
url="jdbc:mysql://localhost:3306/cart" />
</Context>

2，将数据库驱动添加到Tomcat的classPath中或者复制到%TomcatHome%/common/lib下.

3，在代码中使用连接池，获得Connection 实例.
//......
Connection conn = null;
// 获得连接池环境
Contextctx = newInitialContext();
// 获得数据源
DataSource ds = (DataSource) ctx.lookup("java:comp/env/jacken");
// 从数据源中 获得Connection实例
conn = ds.getConnection();
// 执行SQL
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery(sql);
//......
