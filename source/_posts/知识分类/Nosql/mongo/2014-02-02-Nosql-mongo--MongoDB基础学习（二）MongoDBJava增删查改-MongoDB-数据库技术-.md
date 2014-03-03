---
layout: post
title: "MongoDB基础学习（二）MongoDB Java增删查改 - MongoDB - 数据库技术 -"
categories: Nosql
tags: 
 - Nosql
 - mongo
--- 

# MongoDB基础学习（二）MongoDB Java增删查改 - MongoDB - 数据库技术 - 9SSSD.COM

[9SSSD首页](http://www.9sssd.com/)

* [导航![](http://./MongoDB基础学习（二）MongoDB Java增删查改 - MongoDB - 数据库技术 - 9SSSD.COM_files/MenuDownArrow.gif)](http://www.9sssd.com/map)

* [新闻](http://www.9sssd.com/news)
* [.NET技术](http://dotnet.9sssd.com/)
* [Java技术](http://java.9sssd.com/)
* [Web开发](http://web.9sssd.com/)
* [编程语言](http://lang.9sssd.com/)
* [数据库技术](http://database.9sssd.com/)
* [移动开发](http://mobile.9sssd.com/)
* [9SSSD搜索](http://search.9sssd.com/)
[![RSS](http://./MongoDB基础学习（二）MongoDB Java增删查改 - MongoDB - 数据库技术 - 9SSSD.COM_files/rss.png)](http://www.9sssd.com/rss/database)

* [加入收藏]()
* [设为首页]()

[![9SSSD首页](http://./MongoDB基础学习（二）MongoDB Java增删查改 - MongoDB - 数据库技术 - 9SSSD.COM_files/9sssdLogo.png)](http://www.9sssd.com/)

热门搜索： [MongoDB学习](http://search.9sssd.com/s?w=MongoDB%e5%ad%a6%e4%b9%a0&sid=103)[索引](http://search.9sssd.com/s?w=%e7%b4%a2%e5%bc%95&sid=103)[驱动](http://search.9sssd.com/s?w=%e9%a9%b1%e5%8a%a8&sid=103)
* [**数据库技术**](http://database.9sssd.com/)
* [MS SQLServer](http://database.9sssd.com/mssql)
*
* [MySQL](http://database.9sssd.com/mysql)
*
* [Oracle](http://database.9sssd.com/oracle)
*
* [SyBase](http://database.9sssd.com/sybase)
*
* [DB2](http://database.9sssd.com/db2)
*
* [SQLite](http://database.9sssd.com/sqlite)
*
* [PostgreSQL](http://database.9sssd.com/postgresql)
*
* [MongoDB](http://database.9sssd.com/mongodb)

您的位置: [首页](http://www.9sssd.com/)*>*[数据库技术](http://database.9sssd.com/)*>*[MongoDB](http://database.9sssd.com/mongodb)*>*MongoDB基础学习（二）MongoDB Java增删查改

# MongoDB基础学习（二）MongoDB Java增删查改

2012-09-30 00:32 来源：博客园 作者：archie2010 字号：T|T

[**摘要**]本文介绍MongoDB Java增加、删除、查询、修改数据记录，并提供详细的示例代码供参考。

## 相关资料

1、MongoDB for Java的驱动包

[https://github.com/mongodb/mongo-java-driver/downloads](https://github.com/mongodb/mongo-java-driver/downloads)

2、在线文档

[http://www.mongodb.org/display/DOCS/Java+Language+Center](http://www.mongodb.org/display/DOCS/Java+Language+Center)

## 操作

1、查询某张表（在MongoDB中称之为集合）的所有数据

**Java代码DBTest.java**
[View Row Code](http://database.9sssd.com/mongodb/art/633#)1package com.archie.mongodb;23import java.net.UnknownHostException;45import com.mongodb.DB;6import com.mongodb.DBCollection;7import com.mongodb.DBCursor;8import com.mongodb.DBObject;9import com.mongodb.Mongo;10import com.mongodb.MongoException;1112/**13* 查询指定数据库指定DBCollection集合中的所有数据14* @author archie201015*16* since 2012-9-29 下午10:40:2117*/18public class DBTest {19public static void main(String[] args) throws UnknownHostException,20MongoException {21/**22Mongo实例代表了一个数据库连接池23* Mongo mg = new Mongo("localhost");24Mongo mg = new Mongo("localhost", 27017);25*/26Mongo mg = new Mongo();2728// 获取名为“dbtest”的数据库对象29DB db = mg.getDB("dbtest");30// 查询该库中所有的集合 (相当于表)31for (String name : db.getCollectionNames()) {32System.out.println("Collection Name: " + name);33}34DBCollection users = db.getCollection("emp");35// 查询集合中所有的数据36DBCursor cur = users.find();37System.out.println("Record Count:" + cur.count());38while (cur.hasNext()) {39DBObject object = cur.next();40System.out.println(object);41// 取出对象中列表为字段名为'uname'和'upwd'的数据42System.out.println("uname:" + object.get("uname") + "\tupwd:"43+ object.get("upwd"));44}45}46}

运行结果：

![MongoDB Java增删查改](http://./MongoDB基础学习（二）MongoDB Java增删查改 - MongoDB - 数据库技术 - 9SSSD.COM_files/12993448667640348583425.png)

2、对指定DBCollection集合的CRUD操作

**Java代码DBUtil.java**
[View Row Code](http://database.9sssd.com/mongodb/art/633#)1package com.archie.mongodb;23import java.net.UnknownHostException;45import com.mongodb.DB;6import com.mongodb.DBCollection;7import com.mongodb.Mongo;89/**10* 获得DBCollection集合的工具类11* @author archie201012*13* since 2012-9-29 下午10:54:4214*/15public class DBUtil {1617public static Mongo mg=null;18public static DB db=null;19public static DBCollection collection;2021/**22* 获得DBCollection对象23* @param dbName24* @param colName25* @return26*/27public static DBCollection getDBCollection(String dbName,String colName){28if(mg==null){29try {30mg=new Mongo();31} catch (UnknownHostException e) {32e.printStackTrace();33}34}35if(db==null){36db=mg.getDB(dbName);37}38return db.getCollection(colName);39}40}

**CRUDTest.java**

[View Row Code](http://database.9sssd.com/mongodb/art/633#)1package com.archie.mongodb;23import com.mongodb.BasicDBObject;4import com.mongodb.DBCollection;5import com.mongodb.DBCursor;6import com.mongodb.DBObject;78/**9* 对指定DBCollection集合的CRUD操作10* @author archie201011*12* since 2012-9-29 下午10:51:2413*/14public class CRUDTest {15/**16* 增加17* @param obj18*/19public static void add(DBObject obj){20DBCollection coll=DBUtil.getDBCollection("dbtest", "emp");21coll.insert(obj);22}23/**24* 删除25* @param obj26*/27public static void delete(DBObject obj){28DBCollection coll=DBUtil.getDBCollection("dbtest", "emp");29coll.remove(obj);30}31/**32* 查询33*/34public static void query(){35DBCollection coll=DBUtil.getDBCollection("dbtest", "emp");36// 查询集合中所有的数据37DBCursor cur = coll.find();38System.out.println("Record Count:" + cur.count());39while (cur.hasNext()) {40DBObject object = cur.next();41System.out.println(object);42// 取出对象中列表为'uname'和'upwd'的数据43System.out.println("uname:" + object.get("uname") + "\tupwd:"44+ object.get("upwd")+"\t_id:"+object.get("_id"));45}46}47/**48* 修改49*/50public static void modify(DBObject orig,DBObject update){51DBCollection coll=DBUtil.getDBCollection("dbtest", "emp");52coll.update(orig, update, true, false);53}54public static void main(String[] args) {55DBObject empObj=new BasicDBObject();56empObj.put("uname", "teddy");57empObj.put("upwd", "123456");58//添加59add(empObj);60query();616263DBObject updateObj=new BasicDBObject();64updateObj.put("uname", "teddy");65updateObj.put("upwd", "3333");66//更新67modify(new BasicDBObject("uname","teddy"),updateObj);68System.out.println("-----------------------修改后-------------------");69query();7071//删除72delete(new BasicDBObject("uname","teddy"));73System.out.println("-----------------------删除后-------------------");74query();75}76}

运行效果：

![MongoDB Java增删查改](http://./MongoDB基础学习（二）MongoDB Java增删查改 - MongoDB - 数据库技术 - 9SSSD.COM_files/12993448791626553560004.png)

[**示例代码下载**](http://sourceforge.net/projects/mongodbl1/files/)

* 分享到：
* [QQ微博]( "分享到QQ微博")
* [QQ空间]( "分享到QQ空间")
* [新浪微博]( "分享到新浪微博")
* [白社会]( "分享到搜狐白社会")
* [人人网]( "分享到人人网")
* [开心网]( "分享到开心网")
* [豆瓣网]( "分享到豆瓣网")
* [谷歌书签]( "分享到谷歌书签")
* [百度搜藏]( "分享到百度搜藏")
## 相关文章：

* [MongoDB基础学习（三）MongoDB shell 命令行的使用](http://database.9sssd.com/mongodb/art/695)*2012-10-07*
* [MongoDB基础学习（一）安装配置](http://database.9sssd.com/mongodb/art/632)*2012-09-30*
* [MongoDB学习(六) MongoDB索引用法和效率分析](http://database.9sssd.com/mongodb/art/553)*2012-09-11*
* [MongoDB学习 (五) MongoDB文件存取操作](http://database.9sssd.com/mongodb/art/552)*2012-09-11*
* [MongoDB学习(四) 用MongoDB的文档结构描述数据关系](http://database.9sssd.com/mongodb/art/551)*2012-09-11*
* [MongoDB学习(三) MVC模式下通过Jqgrid表格操作MongoDB数据](http://database.9sssd.com/mongodb/art/550)*2012-09-11*
热门搜索: [MongoDB学习](http://search.9sssd.com/s?w=MongoDB%e5%ad%a6%e4%b9%a0&sid=103)

上一篇：[MongoDB基础学习（一）安装配置](http://database.9sssd.com/mongodb/art/632)
下一篇：[MongoDB基础学习（三）MongoDB shell 命令行的使用](http://database.9sssd.com/mongodb/art/695)

## [最新推荐](http://database.9sssd.com/mongodb/recommend)

* [MongoDB基础学习（二）MongoDB Java增删查改]()

## [热门文章](http://database.9sssd.com/mongodb/hot)

* [MongoDB基础学习（三）MongoDB shell 命令行的使用](http://database.9sssd.com/mongodb/art/695)
## 阅读排行

* 24小时排行
* [总排行](http://database.9sssd.com/mongodb/click)

* [MongoDB基础学习（一）安装配置](http://database.9sssd.com/mongodb/art/632)

* [MongoDB基础学习（二）MongoDB Java增删查改]()
* [MongoDB源码阅读 BSON源码分析](http://database.9sssd.com/mongodb/art/805)
* [MongoDB试用及Java的CRUD](http://database.9sssd.com/mongodb/art/556)
* [MongoDB学习 (五) MongoDB文件存取操作](http://database.9sssd.com/mongodb/art/552)
* [MongoDB基础学习（一）安装配置](http://database.9sssd.com/mongodb/art/632)
* [MongoDB基础学习（三）MongoDB shell 命令行的使用](http://database.9sssd.com/mongodb/art/695)
* [MongoDB学习(六) MongoDB索引用法和效率分析](http://database.9sssd.com/mongodb/art/553)
* [MongoDB学习(三) MVC模式下通过Jqgrid表格操作MongoDB数据](http://database.9sssd.com/mongodb/art/550)
* [Memcache 及 Mongodb 介绍](http://database.9sssd.com/mongodb/art/554)
* [MongoDB学习(四) 用MongoDB的文档结构描述数据关系](http://database.9sssd.com/mongodb/art/551)
* [**返回顶部**](http://database.9sssd.com/mongodb/art/633# "返回顶部")

[关于9SSSD](http://www.9sssd.com/about) | [联系我们](http://www.9sssd.com/contact) | [广告服务](http://www.9sssd.com/ad) | [友情链接](http://database.9sssd.com/links) | [网站导航](http://www.9sssd.com/map)

Copyright © 2010 - 2011 9SSSD. All Rights Reserved
9SSSD.COM版权所有

苏ICP备12063653号
[![](http://./MongoDB基础学习（二）MongoDB Java增删查改 - MongoDB - 数据库技术 - 9SSSD.COM_files/21.gif)](http://tongji.baidu.com/hm-web/welcome/ico?s=6ef62b4ec47ec08cd3325e2cad1a2cb3)
