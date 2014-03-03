---
layout: post
title: "Java Socket连接池"
categories: 连接池
tags: 
 - 连接池
--- 

# Java Socket连接池

1：SocketAdapter类，此类继承了socket，重载了socket类的close方法，目的是当用户关闭socket的时候，我们并不关闭它只是放在连接池内部。
package com.tarena.socketpool;
import java.net.*;
import java.io.IOException;
/**
* <p>socket连接的简单实现</p>
* <p>Description: </p>
* <p>Copyright: Copyright Tarena(c) 2005</p>
* <p>Company: Tarena</p>
* @author chengxing
* @version 1.0
*/
public class ConnectionAdapter extends Socket{
/**
* 连接状态
*/
private boolean status=true;
/**
* 默认的构造函数
*/
public ConnectionAdapter() {
super();
}
public ConnectionAdapter(String host,int port)throws UnknownHostException,IOException{
super(host,port);
}
/**
* 判断此连接是否空闲
* @return boolean 空闲返回ture,否则false
*/
public boolean isFree(){
return status;
}
/**
* 当使用此连接的时候设置状态为false（忙碌）
*/
public void setBusy(){
this.status=false;
}
/**
* 当客户端关闭连接的时候状态设置为true(空闲）
*/
public void close(){
System.out.println(Close : set the status is free );
status=true;
}
public void destroy(){
//Close socket connection
close();
// System.out.println(Close success );
}
}
第二个类连接管理器。
package com.tarena.socketpool;

import java.lang.reflect.*;
import java.util.Properties;
/**
* <p>连接管理器</p>
* <p>Copyright: Copyright Tarena(c) 2005</p>
* <p>Company: Tarena</p>
* @author chengxing
* @version 1.0
*/
public class ConnectionManager {
//测试程序默认的连接池实现类
public static final String PROVIDER_CLASS=com.tarena.socketpool.MyConnectionProvider;
//测试程序的默认ip
public static final String HOST=127.0.0.1;
//测试程序的默认端口号
public static final String PORT=9880;
/**
* 注册钩子程序的静态匿名块
*/
static {
//增加钩子控制资源的释放周期
Runtime runtime = Runtime.getRuntime();
Class c = runtime.getClass();
try {
Method m = c.getMethod(addShutdownHook, new Class[] { Thread.class } );
m.invoke(runtime, new Object[] { new ShutdownThread() });
}
catch (NoSuchMethodException e) {
e.printStackTrace();
// Ignore -- the user might not be running JDK 1.3 or later.
}
catch (Exception e) {
e.printStackTrace();
}
}
/**
* 默认的构造函数
*/
public ConnectionManager() {
}
/**
* 得到并初始化一个连接池
* 连接池的实现类通过系统参数来传递进来，通过命令行-DConnectionProvider=YourImplClass
* 如果没有指定的实现的话，则采用系统默认的实现类
* 通过命令行传入的参数列表如下
* 对方主机名-DHost=192.168.0.200
* 对方端口号　-DPort=9880
* 最小连接数 -DMax_style='font-size:10px'0
* 最大连结数　-DMin_style='font-size:14px'0
* 以上的值可以改变，但是参数不能改变，
* 最大连结数和最小连接数可以省略，默认值分别为２０和１０
* @return ConnectionProvider
*/
public static ConnectionProvider getConnectionProvider()throws Exception{
String provider_class=System.getProperty(ConnectionProvider);
if(provider_class==null)provider_class=PROVIDER_CLASS;

String host=System.getProperty(Host);
if(host==null)host=HOST;

String port=System.getProperty(port);
if(port==null)port=PORT;

String max_size=System.getProperty(Max_size);
String min_size=System.getProperty(Min_size);

Properties pro=new Properties();
pro.setProperty(ConnectionProvider.SERVER_IP,host);
pro.setProperty(ConnectionProvider.SERVER_PORT,port);
if(max_size!=null)pro.setProperty(ConnectionProvider.MAX_SIZE,max_size);
if(min_size!=null)pro.setProperty(ConnectionProvider.MIN_SIZE,min_size);
//通过反射得到实现类
System.out.println(provider_class);
System.out.flush();
Class provider_impl=Class.forName(provider_class);
//由于是单子模式，采用静态方法回调
Method m=provider_impl.getMethod(newInstance,new Class[]{java.util.Properties.class});
ConnectionProvider provider=null;
try{
provider = (ConnectionProvider) m.invoke(provider_impl, new Object[]{pro});
}catch(Exception e){
e.printStackTrace();
}

return provider;
}
/**
*
* <p>一个钩子的线程: 在程序结束的时候调用注销连接池</p>
* <p>Description: </p>
* <p>Copyright: Copyright Tarena(c) 2005</p>
* <p>Company: Tarena</p>
* @author chengxing
* @version 1.0
*/
private static class ShutdownThread extends Thread {
public void run() {
try{
ConnectionProvider provider = ConnectionManager.getConnectionProvider();
if (provider != null) {
provider.destroy();
}
}catch(Exception e){
e.printStackTrace();
}
}
}

}

第三个类，连接池的接口定义
package com.tarena.socketpool;

import java.net.*;
import java.util.*;
import java.io.IOException;

/**
*
* <p>定义的抽象类，所有的子类必须单子模式去实现，
* 统一方法为public ConnectionProvider newInstance();
* 连接提供器的抽象接口，每一个实现它的子类最好都是JAVABEAN，
* 这样它的方法就可以是被外界控制</p>
* @see JiveBeanInfo
* <p>Copyright: Copyright Tarena(c) 2005</p>
* <p>Company: Tarena</p>
* @author chengxing
* @version 1.0
*/
public interface
ConnectionProvider {
public static final String SERVER_IP = SERVER_IP_ADDRESS;
public static final String SERVER_PORT = SERVER_IP_PORT;
public static final String MAX_SIZE = MAX_SIZE;
public static final String MIN_SIZE = MIN_SIZE;

/**
*判断连接池内是否有连接
* @return true 有连接返回true,否则返回false
*/
public boolean isPooled();

/**
* 当此方法被调用的时候提供一个 socket
* @see Socket
* @return Socket a Connection object.
*/
public Socket getConnection() throws java.net.SocketException;

/**
* 连接池初始化
*/
public void init() throws UnknownHostException, IOException;

/**
* 连接池重新启动
*/
public void restart() throws UnknownHostException, IOException;

/**
* 注销连接池
*/
public void destroy();
}
第四个类MyConnectionProvider，自己写的一个连接池的简单实现
package com.tarena.socketpool;

import java.util.*;
import java.net.*;
import java.net.SocketException;
import java.io.IOException;

/**
*
* <p>这是一个连接管理器的简单实现</p>
* <p>Description: implements the Interface ConnectionProvider</p>
* <p>Copyright: Copyright Tarena(c) 2005</p>
* <p>Company: Tarena</p>
* @author chengxing
* @version 1.0
*/
public class MyConnectionProvider
implements ConnectionProvider {

private Properties pro = null;
private static ConnectionProvider provider = null;
private static Object object_lock = new Object();
private String ip;
private String port;

/**
* 默认的最大连接数
*/
private int max_size = 20;

/**
* 默认的最小连接数
*/
private int min_size = 10;

/**
* Socket connection池数组
*/
private ConnectionAdapter[] socketpool = null;

/**
* 构造对象的时候初始化连接池
* @throws UnknownHostException 未知的主机异常
* @throws IOException
*/
private MyConnectionProvider(Properties pro) throws UnknownHostException,
IOException {
ip = pro.getProperty(SERVER_IP);
port = pro.getProperty(SERVER_PORT);
String max_size_s = pro.getProperty(MAX_SIZE);
String min_size_s = pro.getProperty(MIN_SIZE);
if (max_size_s != null) {
max_size = Integer.parseInt(max_size_s);
}
if (min_size_s != null) {
min_size = Integer.parseInt(min_size_s);
}

init(); //构造对象的时候初始化连接池
}

/**
* 判断是否已经池化
* @return boolean 如果池化返回ture,反之返回false
*/
public boolean isPooled() {
if (socketpool != null) {
return true;
}
else return false;
}

/**
*返回一个连接
* @return a Connection object.
*/
public Socket getConnection() {
Socket s = null;
for (int i = 0; i < socketpool.length; i++) {
if (socketpool[i] != null) {
//如果有空闲的连接，返回一个空闲连接，如果没有，继续循环
if (socketpool[i].isFree()) {
s = socketpool[i];
return s;
}
else continue;
}
else { //如果连接为空，证明超过最小连接数，重新生成连接
try {
s = socketpool[i] = new ConnectionAdapter(ip, Integer.parseInt(port));
}
catch (Exception e) {
//never throw
}
}
}
//如果连接仍旧为空的话，则超过了最大连接数
if (s == null) {
try { //生成普通连接，由客户端自行关闭，释放资源，不再由连接池管理
s = new Socket(ip, Integer.parseInt(port));
}
catch (Exception e) { //此异常永远不会抛出
}
}
return s;
}

/**
* 初始化连接池
* @throws UnknownHostException 主机ip找不到
* @throws IOException 此端口号上无server监听
*/
public void init() throws UnknownHostException, IOException {

socketpool = new ConnectionAdapter[max_size];
for (int i = 0; i < min_size; i++) {
socketpool[i] = new ConnectionAdapter(ip, Integer.parseInt(port));
System.out.print( . );
}
System.out.println();
System.out.println(System init success ....);
}

/**
* 重新启动连接池
* @throws UnknownHostException
* @throws IOException
*/
public void restart() throws UnknownHostException, IOException {
destroy();
init();
}

/**
* 注销此连接池
*/
public void destroy() {
for (int i = 0; i < socketpool.length; i++) {
if (socketpool[i] != null) {
ConnectionAdapter adapter = (ConnectionAdapter) socketpool[i];
adapter.destroy();
System.out.print( . );
}
}
System.out.println(\ndestory success ....);
}
/**
* 静态方法，生成此连接池实现的对象
* @param pro Properties 此连接池所需要的所有参数的封装
* @throws UnknownHostException 主机无法找到
* @throws IOException 与服务器无法建立连接
* @return ConnectionProvider 返回父类ConnectionProvider
*/
public static ConnectionProvider newInstance(java.util.Properties pro) throws
UnknownHostException, IOException {
if (provider == null) {
synchronized (object_lock) {
if (provider == null) {
provider = new MyConnectionProvider(pro);
}
}
}
return provider;
}
/**
*设置系统属性 通过封装系统properties对象来封装所需要的不同值
* SERVER_IP，SERVER_PORT，MAX_SIZE,MIN_SIZE等父类定义的不同的参数
* @param pro Properties 传进来的系统属性
*/
public void setProperties(Properties pro) {
this.pro = pro;
}
}

posted on 2008-04-26 16:51 [马森](http://www.cnblogs.com/pony/) 阅读(661) [评论(0)](http://www.cnblogs.com/pony/archive/2008/04/26/1172288.html#Post) [编辑](http://www.cnblogs.com/pony/admin/EditPosts.aspx?postid=1172288) [收藏](http://www.cnblogs.com/pony/AddToFavorite.aspx?id=1172288) [网摘](http://www.cnblogs.com/pony/archive/2008/04/26/1172288.html#)
