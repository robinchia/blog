---
layout: post
title: "PreparedStatement 批量更新-插入数据到Oracle - 生活在爪洼岛上 - ITe"
categories: DB
tags: 
 - DB
 - oracle
 - oracle-
--- 

# PreparedStatement 批量更新,插入数据到Oracle - 生活在爪洼岛上 - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [精华](http://www.iteye.com/magazines) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [专栏](http://www.iteye.com/blogs/subjects) [群组](http://www.iteye.com/groups) [更多 ▼](http://zhoujingxian.iteye.com/blog/752742#)

[招聘](http://www.iteye.com/job) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://zhoujingxian.iteye.com/login "登录") [登录](http://zhoujingxian.iteye.com/login) [注册](http://zhoujingxian.iteye.com/signup)

# [生活在爪洼岛上](http://zhoujingxian.iteye.com/)

* [**博客**](http://zhoujingxian.iteye.com/)
* [微博](http://zhoujingxian.iteye.com/weibo)
* [相册](http://zhoujingxian.iteye.com/album)
* [收藏](http://zhoujingxian.iteye.com/link)
* [留言](http://zhoujingxian.iteye.com/blog/guest_book)
* [关于我](http://zhoujingxian.iteye.com/blog/profile)

### [PreparedStatement 批量更新,插入数据到Oracle]() **

**博客分类：**
* [database](http://zhoujingxian.iteye.com/category/74527)
[Oracle](http://www.iteye.com/blogs/tag/Oracle)[SQL](http://www.iteye.com/blogs/tag/SQL)[WAP](http://www.iteye.com/blogs/tag/WAP)

批量更新,插入代码  [![收藏代码]()![]()]( "收藏这段代码")

1. /**  
1.  * 更新数据库已有的customer信息  
1.  * @param List<CustomerBean>  
1.  * @return   
1.  */  
1. public int updateExistsInfo(List<CustomerBean> updateList){  
1.       
1.     //查询的SQL语句  
1.     String sql = "update t_customer set LICENSE_KEY=?,CORPORATE_NAME=?,INTEGRATED_CLASSIFICATION=?,BOSSHEAD=?," +  
1.             "CONTACT_PHONE=?,ORDER_FREQUENCY=?,CONTACT_ADDRESS=?,USER_ID=? where CUSTOMER_ID=?" ;  
1.       
1.     //插入需要的数据库对象  
1.     Connection conn = null;  
1.     PreparedStatement pstmt = null;  
1.   
1.     try  {            
1.         conn = new DBSource().getConnection();  
1.   
1.         //设置事务属性  
1.         conn.setAutoCommit(false);  
1.           
1.         pstmt = conn.prepareStatement(sql,ResultSet.TYPE_SCROLL_INSENSITIVE,ResultSet.CONCUR_READ_ONLY);              
1.   
1.         for(CustomerBean cbean : updateList){  
1.             pstmt.setString(1, cbean.getLicense_key());  
1.             pstmt.setString(2, cbean.getCorporate_name());  
1.             pstmt.setString(3, cbean.getIntegrated_classification());  
1.             pstmt.setString(4, cbean.getBosshead());  
1.             pstmt.setString(5, cbean.getContact_phone());  
1.             pstmt.setString(6, cbean.getOrder_frequency());  
1.             pstmt.setString(7, cbean.getContact_address());  
1.             pstmt.setInt   (8, cbean.getUser_id());  
1.             pstmt.setInt   (9, cbean.getCustomer_id());  
1.               
1.             pstmt.addBatch();  
1.               
1.         }  
1.         int[] tt = pstmt.executeBatch();  
1.         System.out.println("update : " + tt.length);  
1.   
1.         //提交，设置事务初始值  
1.         conn.commit();  
1.         conn.setAutoCommit(true);  
1.   
1.         //插入成功，返回  
1.         return tt.length;  
1.   
1.     }catch(SQLException ex){  
1.         try{  
1.             //提交失败，执行回滚操作  
1.             conn.rollback();  
1.   
1.         }catch (SQLException e) {  
1.             e.printStackTrace();  
1.             System.err.println("updateExistsInfo回滚执行失败!!!");  
1.         }  
1.   
1.         ex.printStackTrace();  
1.         System.err.println("updateExistsInfo执行失败");  
1.   
1.         //插入失败返回标志0  
1.         return 0;  
1.   
1.     }finally {  
1.         try{  
1.             //关闭资源  
1.             if(pstmt != null)pstmt.close();  
1.             if(conn != null)conn.close();  
1.               
1.         }catch (SQLException e) {  
1.             e.printStackTrace();  
1.             System.err.println("资源关闭失败!!!");  
1.         }  
1.     }  
1. }   
1.   
1. /**  
1.  * 插入数据中没有的customer信息  
1.  * @param List<CustomerBean>  
1.  * @return   
1.  */  
1. public int insertNewInfo(List<CustomerBean> insertList){  
1.               
1.     //查询的SQL语句  
1.     String sql = "insert into t_customer(CUSTOMER_ID," +  
1.             "LICENSE_KEY,CORPORATE_NAME,INTEGRATED_CLASSIFICATION,BOSSHEAD,CONTACT_PHONE," +  
1.             "ORDER_FREQUENCY,CONTACT_ADDRESS,USER_ID,CUSTOMER_NUM,CUSTOMER_CODING," +  
1.             "INVESTIGATION_TIME,SMS_REC_FLAG,WAP_FLAG,PRICE_GATHERING_FLAG,SOCIETY_STOCK_FLAG," +  
1.             "REGION_TYPE)" +  
1.             "VALUES(CUSTOMER.NEXTVAL," +  
1.             "?,?,?,?,?," +  
1.             "?,?,?,?,?," +  
1.             "TO_DATE(?,'YYYY-MM-DD'),?,0,0,0," +  
1.             "?)" ;  
1.       
1.     //插入需要的数据库对象  
1.     Connection conn = null;  
1.     PreparedStatement pstmt = null;  
1.   
1.     try  {            
1.         conn = new DBSource().getConnection();  
1.   
1.         //设置事务属性  
1.         conn.setAutoCommit(false);  
1.           
1.         pstmt = conn.prepareStatement(sql,ResultSet.TYPE_SCROLL_INSENSITIVE,ResultSet.CONCUR_READ_ONLY);              
1.   
1.         for(CustomerBean cbean : insertList){  
1.             pstmt.setString(1, cbean.getLicense_key());  
1.             pstmt.setString(2, cbean.getCorporate_name());  
1.             pstmt.setString(3, cbean.getIntegrated_classification());  
1.             pstmt.setString(4, cbean.getBosshead());  
1.             pstmt.setString(5, cbean.getContact_phone());  
1.             pstmt.setString(6, cbean.getOrder_frequency());  
1.             pstmt.setString(7, cbean.getContact_address());  
1.             pstmt.setInt(8, cbean.getUser_id());  
1.             pstmt.setString(9, "gyyc00000");//  
1.             pstmt.setString(10, "95000000");//  
1.             pstmt.setString(11, getToday());  
1.             pstmt.setInt(12, cbean.getSms_rec_flag());  
1.             pstmt.setInt(13, cbean.getRegion_type());  
1.               
1.   
1.             pstmt.addBatch();  
1.   
1.         }  
1.         int[] tt = pstmt.executeBatch();  
1.         System.out.println("insert : " + tt.length);  
1.   
1.         //提交，设置事务初始值  
1.         conn.commit();  
1.         conn.setAutoCommit(true);  
1.   
1.         //插入成功，返回  
1.         return tt.length;  
1.   
1.     }catch(SQLException ex){  
1.         try{  
1.             //提交失败，执行回滚操作  
1.             conn.rollback();  
1.   
1.         }catch (SQLException e) {  
1.             e.printStackTrace();  
1.             System.err.println("insertNewInfo回滚执行失败!!!");  
1.         }  
1.   
1.         ex.printStackTrace();  
1.         System.err.println("insertNewInfo执行失败");  
1.   
1.         //插入失败返回标志0  
1.         return 0;  
1.   
1.     }finally {  
1.         try{  
1.             //关闭资源  
1.             if(pstmt != null)pstmt.close();  
1.             if(conn != null)conn.close();  
1.               
1.         }catch (SQLException e) {  
1.             e.printStackTrace();  
1.             System.err.println("资源关闭失败!!!");  
1.         }  
1.     }  
1. }   
/** * 更新数据库已有的customer信息 * @param List<CustomerBean> * @return */ public int updateExistsInfo(List<CustomerBean> updateList){ //查询的SQL语句 String sql = "update t_customer set LICENSE_KEY=?,CORPORATE_NAME=?,INTEGRATED_CLASSIFICATION=?,BOSSHEAD=?," + "CONTACT_PHONE=?,ORDER_FREQUENCY=?,CONTACT_ADDRESS=?,USER_ID=? where CUSTOMER_ID=?" ; //插入需要的数据库对象 Connection conn = null; PreparedStatement pstmt = null; try { conn = new DBSource().getConnection(); //设置事务属性 conn.setAutoCommit(false); pstmt = conn.prepareStatement(sql,ResultSet.TYPE_SCROLL_INSENSITIVE,ResultSet.CONCUR_READ_ONLY); for(CustomerBean cbean : updateList){ pstmt.setString(1, cbean.getLicense_key()); pstmt.setString(2, cbean.getCorporate_name()); pstmt.setString(3, cbean.getIntegrated_classification()); pstmt.setString(4, cbean.getBosshead()); pstmt.setString(5, cbean.getContact_phone()); pstmt.setString(6, cbean.getOrder_frequency()); pstmt.setString(7, cbean.getContact_address()); pstmt.setInt (8, cbean.getUser_id()); pstmt.setInt (9, cbean.getCustomer_id()); pstmt.addBatch(); } int[] tt = pstmt.executeBatch(); System.out.println("update : " + tt.length); //提交，设置事务初始值 conn.commit(); conn.setAutoCommit(true); //插入成功，返回 return tt.length; }catch(SQLException ex){ try{ //提交失败，执行回滚操作 conn.rollback(); }catch (SQLException e) { e.printStackTrace(); System.err.println("updateExistsInfo回滚执行失败!!!"); } ex.printStackTrace(); System.err.println("updateExistsInfo执行失败"); //插入失败返回标志0 return 0; }finally { try{ //关闭资源 if(pstmt != null)pstmt.close(); if(conn != null)conn.close(); }catch (SQLException e) { e.printStackTrace(); System.err.println("资源关闭失败!!!"); } } } /** * 插入数据中没有的customer信息 * @param List<CustomerBean> * @return */ public int insertNewInfo(List<CustomerBean> insertList){ //查询的SQL语句 String sql = "insert into t_customer(CUSTOMER_ID," + "LICENSE_KEY,CORPORATE_NAME,INTEGRATED_CLASSIFICATION,BOSSHEAD,CONTACT_PHONE," + "ORDER_FREQUENCY,CONTACT_ADDRESS,USER_ID,CUSTOMER_NUM,CUSTOMER_CODING," + "INVESTIGATION_TIME,SMS_REC_FLAG,WAP_FLAG,PRICE_GATHERING_FLAG,SOCIETY_STOCK_FLAG," + "REGION_TYPE)" + "VALUES(CUSTOMER.NEXTVAL," + "?,?,?,?,?," + "?,?,?,?,?," + "TO_DATE(?,'YYYY-MM-DD'),?,0,0,0," + "?)" ; //插入需要的数据库对象 Connection conn = null; PreparedStatement pstmt = null; try { conn = new DBSource().getConnection(); //设置事务属性 conn.setAutoCommit(false); pstmt = conn.prepareStatement(sql,ResultSet.TYPE_SCROLL_INSENSITIVE,ResultSet.CONCUR_READ_ONLY); for(CustomerBean cbean : insertList){ pstmt.setString(1, cbean.getLicense_key()); pstmt.setString(2, cbean.getCorporate_name()); pstmt.setString(3, cbean.getIntegrated_classification()); pstmt.setString(4, cbean.getBosshead()); pstmt.setString(5, cbean.getContact_phone()); pstmt.setString(6, cbean.getOrder_frequency()); pstmt.setString(7, cbean.getContact_address()); pstmt.setInt(8, cbean.getUser_id()); pstmt.setString(9, "gyyc00000");// pstmt.setString(10, "95000000");// pstmt.setString(11, getToday()); pstmt.setInt(12, cbean.getSms_rec_flag()); pstmt.setInt(13, cbean.getRegion_type()); pstmt.addBatch(); } int[] tt = pstmt.executeBatch(); System.out.println("insert : " + tt.length); //提交，设置事务初始值 conn.commit(); conn.setAutoCommit(true); //插入成功，返回 return tt.length; }catch(SQLException ex){ try{ //提交失败，执行回滚操作 conn.rollback(); }catch (SQLException e) { e.printStackTrace(); System.err.println("insertNewInfo回滚执行失败!!!"); } ex.printStackTrace(); System.err.println("insertNewInfo执行失败"); //插入失败返回标志0 return 0; }finally { try{ //关闭资源 if(pstmt != null)pstmt.close(); if(conn != null)conn.close(); }catch (SQLException e) { e.printStackTrace(); System.err.println("资源关闭失败!!!"); } } }

 

Notice:

//设置事务属性
conn.setAutoCommit(false);
pstmt = conn.prepareStatement(sql,ResultSet.TYPE_SCROLL_INSENSITIVE,ResultSet.CONCUR_READ_ONLY);
for(CustomerBean cbean : updateList){
pstmt.setString(1, cbean.getLicense_key());
...    
    pstmt.addBatch();
}
int[] tt = pstmt.executeBatch();
System.out.println("update : " + tt.length);
//提交，设置事务初始值
conn.commit();
conn.setAutoCommit(true);
...
分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")

[[面试技巧]如何向你的面试官“发问”(转 ...](http://zhoujingxian.iteye.com/blog/753295 "[面试技巧]如何向你的面试官“发问”(转)") | [最后期限中的经典管理名录](http://zhoujingxian.iteye.com/blog/748049 "最后期限中的经典管理名录")
* 2010-09-01 15:34:23
* 浏览 668
* [评论(0)](http://zhoujingxian.iteye.com/blog/752742#comments)
* 分类:[数据库](http://www.iteye.com/blogs/category/database)
* [相关推荐](http://www.iteye.com/wiki/blog/752742)

### 评论

[]()
### 发表评论

[![]()](http://zhoujingxian.iteye.com/login)[您还没有登录,请您登录后再发表评论](http://zhoujingxian.iteye.com/login)

[![zjx2388的博客]( "zjx2388的博客: 生活在爪洼岛上")](http://zhoujingxian.iteye.com/)

zjx2388

* 浏览: 211663 次
* 性别: ![Icon_minigender_2]( "女")
* 来自: 北京
* ![]()
### 最近访客 [更多访客>>](http://zhoujingxian.iteye.com/blog/user_visits)

[![danrise的博客]( "danrise的博客: danrise")](http://danrise.iteye.com/)

[danrise](http://danrise.iteye.com/)

[![lijianlee的博客]( "lijianlee的博客: ")](http://lijianlee.iteye.com/)

[lijianlee](http://lijianlee.iteye.com/)
[![yslflsy的博客]( "yslflsy的博客: ")](http://yslflsy.iteye.com/)

[yslflsy](http://yslflsy.iteye.com/)

[![ws_nihao的博客]( "ws_nihao的博客: ")](http://ws-nihao.iteye.com/)

[ws_nihao](http://ws-nihao.iteye.com/)

### 文章分类

* [全部博客 (451)](http://zhoujingxian.iteye.com/)
* [J2SE (95)](http://zhoujingxian.iteye.com/category/74522)
* [J2EE (93)](http://zhoujingxian.iteye.com/category/74523)
* [database (66)](http://zhoujingxian.iteye.com/category/74527)
* [by-talk (33)](http://zhoujingxian.iteye.com/category/74528)
* [JavaScript (48)](http://zhoujingxian.iteye.com/category/74529)
* [Tools/Software (48)](http://zhoujingxian.iteye.com/category/74530)
* [Page (12)](http://zhoujingxian.iteye.com/category/95008)
* [Linux (7)](http://zhoujingxian.iteye.com/category/181884)
* [职场 (18)](http://zhoujingxian.iteye.com/category/121837)
* [Android (4)](http://zhoujingxian.iteye.com/category/127946)
* [网络编程 (4)](http://zhoujingxian.iteye.com/category/136921)
* [认证考试 (16)](http://zhoujingxian.iteye.com/category/136922)
* [IELTS (2)](http://zhoujingxian.iteye.com/category/140062)
* [Portal服务器 (1)](http://zhoujingxian.iteye.com/category/173880)
* [Portlet容器 (1)](http://zhoujingxian.iteye.com/category/173881)
* [Portlet 的区别 (1)](http://zhoujingxian.iteye.com/category/173882)
* [Carefx_relate (2)](http://zhoujingxian.iteye.com/category/174652)
* [Linux sub= (0)](http://zhoujingxian.iteye.com/category/181886)
* [GWT (1)](http://zhoujingxian.iteye.com/category/185363)
* [面试题 (3)](http://zhoujingxian.iteye.com/category/212290)
### 社区版块

* [我的资讯](http://zhoujingxian.iteye.com/blog/news) (0)
* [我的论坛](http://zhoujingxian.iteye.com/blog/post) (24)
* [我解决的问题](http://zhoujingxian.iteye.com/blog/solution) (0)

### 存档分类

* [2012-04](http://zhoujingxian.iteye.com/blog/monthblog/2012-04) (3)
* [2012-03](http://zhoujingxian.iteye.com/blog/monthblog/2012-03) (4)
* [2011-10](http://zhoujingxian.iteye.com/blog/monthblog/2011-10) (10)
* [更多存档...](http://zhoujingxian.iteye.com/blog/monthblog_more)
### 评论排行榜

* [AMF,RTMP,RTMPT,RTMPS(转)](http://zhoujingxian.iteye.com/blog/1021222 "AMF,RTMP,RTMPT,RTMPS(转)")
* [jdk与jre的区别](http://zhoujingxian.iteye.com/blog/1114759 "jdk与jre的区别 ")

### 最新评论

* [zjx2388](http://zhoujingxian.iteye.com/)： yilv99 写道汗。。。<option value=& ...
[jstl中下拉列表有默认值的两种html写法](http://zhoujingxian.iteye.com/blog/503275#bc2252345)
* [yilv99](http://yilv99.iteye.com/)： 汗。。。<option value="一类&q ...
[jstl中下拉列表有默认值的两种html写法](http://zhoujingxian.iteye.com/blog/503275#bc2252227)
* [java_ganbin](http://java-ganbin.iteye.com/)： ...
[JAVA生成简单的随机字符串(a-zA-Z0-9)](http://zhoujingxian.iteye.com/blog/793082#bc2234807)
* [xlblank](http://xlblank.iteye.com/)：          // 把时间转换成整型 public St ...
[java中replaceAll()](http://zhoujingxian.iteye.com/blog/440809#bc2234564)
* [zdfeng](http://zdfeng.iteye.com/)： 我喜欢，java看着就是爽，虽然是android，基础，还是 ...
[Android 下载文件及写入SD卡](http://zhoujingxian.iteye.com/blog/859597#bc2221052)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 京公网安备110105010620 ]
![]()
