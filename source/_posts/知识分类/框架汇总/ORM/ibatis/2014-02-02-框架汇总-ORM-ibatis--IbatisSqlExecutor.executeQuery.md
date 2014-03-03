---
layout: post
title: "Ibatis SqlExecutor.executeQuery"
categories: 框架汇总
tags: 
 - 框架汇总
 - ORM
 - ibatis
--- 

# Ibatis SqlExecutor.executeQuery

# [小猫的窝棚]()

##

* [![]()目录视图]()
* [![]()摘要视图]()
* [![]()订阅]()
[公告：新版下载频道介绍之二——上传和下载资源页面介绍](http://blog.csdn.net/csdnproduct/article/details/6692206)                 [bShare分享，迅速提升10倍流量](http://g.csdn.net/5193964)

### [Ibatis SqlExecutor.executeQuery(ZZ)]( "Ibatis SqlExecutor.executeQuery(ZZ)")

分类： [Java]()  2008-07-28 10:37 688人阅读 [评论]()(1) [收藏]( "收藏") [举报]( "举报")
 
一直以来ibatis的分页都是通过滚动ResultSet实现的，应该算是逻辑分页吧。逻辑分页虽然能很干净地独立于特定数据库，但效率在多数情况下不及特定数据库支持的物理分页，而hibernate的分页则是直接组装sql，充分利用了特定数据库的分页机制，效率相对较高。本文讲述的就是如何在不重新编译ibatis源码的前提下，为ibatis引入hibernate式的物理分页机制。 基本思路就是找到ibatis执行sql的地方，截获sql并重新组装sql。通过分析ibatis源码知道，最终负责执行sql的类是com.ibatis.sqlmap.engine.execution.SqlExecutor，此类没有实现任何接口，这多少有点遗憾，因为接口是相对稳定契约，非大的版本更新，接口一般是不会变的，而类就相对易变一些，所以这里的代码只能保证对当前版本（2.1.7）的ibatis有效。下面是SqlExecutor执行查询的方法： /** * Long form of the method to execute a query * * @param request - the request scope * @param conn - the database connection * @param sql - the SQL statement to execute * @param parameters - the parameters for the statement * @param skipResults - the number of results to skip * @param maxResults - the maximum number of results to return * @param callback - the row handler for the query * * @throws SQLException - if the query fails */ public void executeQuery(RequestScope request, Connection conn, String sql, Object[] parameters, int skipResults, int maxResults, RowHandlerCallback callback) throws SQLException { ErrorContext errorContext = request.getErrorContext(); errorContext.setActivity("executing query"); errorContext.setObjectId(sql); PreparedStatement ps = null; ResultSet rs = null; try { errorContext.setMoreInfo("Check the SQL Statement (preparation failed)."); Integer rsType = request.getStatement().getResultSetType(); if (rsType != null) { ps = conn.prepareStatement(sql, rsType.intValue(), ResultSet.CONCUR_READ_ONLY); } else { ps = conn.prepareStatement(sql); } Integer fetchSize = request.getStatement().getFetchSize(); if (fetchSize != null) { ps.setFetchSize(fetchSize.intValue()); } errorContext.setMoreInfo("Check the parameters (set parameters failed)."); request.getParameterMap().setParameters(request, ps, parameters); errorContext.setMoreInfo("Check the statement (query failed)."); ps.execute(); rs = getFirstResultSet(ps); if (rs != null) { errorContext.setMoreInfo("Check the results (failed to retrieve results)."); handleResults(request, rs, skipResults, maxResults, callback); } // clear out remaining results while (ps.getMoreResults()); } finally { try { closeResultSet(rs); } finally { closeStatement(ps); } } } 其中handleResults(request, rs, skipResults, maxResults, callback)一句用于处理分页，其实此时查询已经执行完毕，可以不必关心handleResults方法，但为清楚起见，下面来看看handleResults的实现： private void handleResults(RequestScope request, ResultSet rs, int skipResults, int maxResults, RowHandlerCallback callback) throws SQLException { try { request.setResultSet(rs); ResultMap resultMap = request.getResultMap(); if (resultMap != null) { // Skip Results if (rs.getType() != ResultSet.TYPE_FORWARD_ONLY) { if (skipResults > 0) { rs.absolute(skipResults); } } else { for (int i = 0; i < skipResults; i++) { if (!rs.next()) { break; } } } // Get Results int resultsFetched = 0; while ((maxResults == SqlExecutor.NO_MAXIMUM_RESULTS || resultsFetched < maxResults) && rs.next()) { Object[] columnValues = resultMap.resolveSubMap(request, rs).getResults(request, rs); callback.handleResultObject(request, columnValues, rs); resultsFetched++; } } } finally { request.setResultSet(null); } } 此处优先使用的是ResultSet的absolute方法定位记录，是否支持absolute取决于具体数据库驱动，但一般当前版本的数据库都支持该方法，如果不支持则逐条跳过前面的记录。由此可以看出如果数据库支持absolute，则ibatis内置的分页策略与特定数据库的物理分页效率差距就在于物理分页查询与不分页查询在数据库中的执行效率的差距了。因为查询执行后读取数据前数据库并未把结果全部返回到内存，所以本身在存储占用上应该差距不大，如果都使用索引，估计执行速度也差不太多。 继续我们的话题。其实只要在executeQuery执行前组装sql，然后将其传给executeQuery，并告诉handleResults我们不需要逻辑分页即可。拦截executeQuery可以采用aop动态实现，也可直接继承SqlExecutor覆盖executeQuery来静态地实现，相比之下后者要简单许多，而且由于SqlExecutor没有实现任何接口，比较易变，动态拦截反到增加了维护的工作量，所以我们下面来覆盖executeQuery： package com.aladdin.dao.ibatis.ext; import java.sql.Connection; import java.sql.SQLException; import org.apache.commons.logging.Log; import org.apache.commons.logging.LogFactory; import com.aladdin.dao.dialect.Dialect; import com.ibatis.sqlmap.engine.execution.SqlExecutor; import com.ibatis.sqlmap.engine.mapping.statement.RowHandlerCallback; import com.ibatis.sqlmap.engine.scope.RequestScope; public class LimitSqlExecutor extends SqlExecutor { private static final Log logger = LogFactory.getLog(LimitSqlExecutor.class); private Dialect dialect; private boolean enableLimit = true; public Dialect getDialect() { return dialect; } public void setDialect(Dialect dialect) { this.dialect = dialect; } public boolean isEnableLimit() { return enableLimit; } public void setEnableLimit(boolean enableLimit) { this.enableLimit = enableLimit; } @Override public void executeQuery(RequestScope request, Connection conn, String sql, Object[] parameters, int skipResults, int maxResults, RowHandlerCallback callback) throws SQLException { if ((skipResults != NO_SKIPPED_RESULTS || maxResults != NO_MAXIMUM_RESULTS) && supportsLimit()) { sql = dialect.getLimitString(sql, skipResults, maxResults); if(logger.isDebugEnabled()){ logger.debug(sql); } skipResults = NO_SKIPPED_RESULTS; maxResults = NO_MAXIMUM_RESULTS; } super.executeQuery(request, conn, sql, parameters, skipResults, maxResults, callback); } public boolean supportsLimit() { if (enableLimit && dialect != null) { return dialect.supportsLimit(); } return false; } } 其中： skipResults = NO_SKIPPED_RESULTS; maxResults = NO_MAXIMUM_RESULTS; 告诉handleResults不分页（我们组装的sql已经使查询结果是分页后的结果了），此处引入了类似hibenate中的数据库方言接口Dialect，其代码如下： package com.aladdin.dao.dialect; public interface Dialect { public boolean supportsLimit(); public String getLimitString(String sql, boolean hasOffset); public String getLimitString(String sql, int offset, int limit); } 下面为Dialect接口的MySQL实现： package com.aladdin.dao.dialect; public class MySQLDialect implements Dialect { protected static final String SQL_END_DELIMITER = ";"; public String getLimitString(String sql, boolean hasOffset) { return new StringBuffer(sql.length() + 20).append(trim(sql)).append( hasOffset ? " limit ?,?" : " limit ?") .append(SQL_END_DELIMITER).toString(); } public String getLimitString(String sql, int offset, int limit) { sql = trim(sql); StringBuffer sb = new StringBuffer(sql.length() + 20); sb.append(sql); if (offset > 0) { sb.append(" limit ").append(offset).append(',').append(limit) .append(SQL_END_DELIMITER); } else { sb.append(" limit ").append(limit).append(SQL_END_DELIMITER); } return sb.toString(); } public boolean supportsLimit() { return true; } private String trim(String sql) { sql = sql.trim(); if (sql.endsWith(SQL_END_DELIMITER)) { sql = sql.substring(0, sql.length() - 1 - SQL_END_DELIMITER.length()); } return sql; } } 接下来的工作就是把LimitSqlExecutor注入ibatis中。我们是通过spring来使用ibatis的，所以在我们的dao基类中执行注入，代码如下： package com.aladdin.dao.ibatis; import java.io.Serializable; import java.util.List; import org.springframework.orm.ObjectRetrievalFailureException; import org.springframework.orm.ibatis.support.SqlMapClientDaoSupport; import com.aladdin.dao.ibatis.ext.LimitSqlExecutor; import com.aladdin.domain.BaseObject; import com.aladdin.util.ReflectUtil; import com.ibatis.sqlmap.client.SqlMapClient; import com.ibatis.sqlmap.engine.execution.SqlExecutor; import com.ibatis.sqlmap.engine.impl.ExtendedSqlMapClient; public abstract class BaseDaoiBatis extends SqlMapClientDaoSupport { private SqlExecutor sqlExecutor; public SqlExecutor getSqlExecutor() { return sqlExecutor; } public void setSqlExecutor(SqlExecutor sqlExecutor) { this.sqlExecutor = sqlExecutor; } public void setEnableLimit(boolean enableLimit) { if (sqlExecutor instanceof LimitSqlExecutor) { ((LimitSqlExecutor) sqlExecutor).setEnableLimit(enableLimit); } } public void initialize() throws Exception { if (sqlExecutor != null) { SqlMapClient sqlMapClient = getSqlMapClientTemplate() .getSqlMapClient(); if (sqlMapClient instanceof ExtendedSqlMapClient) { ReflectUtil.setFieldValue(((ExtendedSqlMapClient) sqlMapClient) .getDelegate(), "sqlExecutor", SqlExecutor.class, sqlExecutor); } } } ... } 其中的initialize方法执行注入，稍后会看到此方法在spring Beans 配置中指定为init-method。由于sqlExecutor是com.ibatis.sqlmap.engine.impl.ExtendedSqlMapClient的私有成员，且没有公开的set方法，所以此处通过反射绕过java的访问控制，下面是ReflectUtil的实现代码： package com.aladdin.util; import java.lang.reflect.Field; import java.lang.reflect.Method; import java.lang.reflect.Modifier; import org.apache.commons.logging.Log; import org.apache.commons.logging.LogFactory; public class ReflectUtil { private static final Log logger = LogFactory.getLog(ReflectUtil.class); public static void setFieldValue(Object target, String fname, Class ftype, Object fvalue) { if (target == null || fname == null || "".equals(fname) || (fvalue != null && !ftype.isAssignableFrom(fvalue.getClass()))) { return; } Class clazz = target.getClass(); try { Method method = clazz.getDeclaredMethod("set" + Character.toUpperCase(fname.charAt(0)) + fname.substring(1), ftype); if (!Modifier.isPublic(method.getModifiers())) { method.setAccessible(true); } method.invoke(target, fvalue); } catch (Exception me) { if (logger.isDebugEnabled()) { logger.debug(me); } try { Field field = clazz.getDeclaredField(fname); if (!Modifier.isPublic(field.getModifiers())) { field.setAccessible(true); } field.set(target, fvalue); } catch (Exception fe) { if (logger.isDebugEnabled()) { logger.debug(fe); } } } } }<PRE>

 

到此剩下的就是通过Spring将sqlExecutor注入BaseDaoiBatis中了，下面是Spring Beans配置文件： <?xml version="1.0" encoding="UTF-8"?> <BEANS> <!-- Transaction manager for a single JDBC DataSource --> <BEAN class=org.springframework.jdbc.datasource.DataSourceTransactionManager id=transactionManager> <property name="dataSource"> <REF bean="dataSource" /> </property> </bEAN> <!-- SqlMap setup for iBATIS Database Layer --> <BEAN class=org.springframework.orm.ibatis.SqlMapClientFactoryBean id=sqlMapClient> <property name="configLocation"> <VALUE>classpath:/com/aladdin/dao/ibatis/sql-map-config.xml</vALUE> </property> <property name="dataSource"> <REF bean="dataSource" /> </property> </bEAN> <BEAN class=com.aladdin.dao.ibatis.ext.LimitSqlExecutor id=sqlExecutor> <property name="dialect"> <BEAN class=com.aladdin.dao.dialect.MySQLDialect /> </property> </bEAN> <BEAN class=com.aladdin.dao.ibatis.BaseDaoiBatis id=baseDao init-method="initialize" abstract="true"> <property name="dataSource"> <REF bean="dataSource" /> </property> <property name="sqlMapClient"> <REF bean="sqlMapClient" /> </property> <property name="sqlExecutor"> <REF bean="sqlExecutor" /> </property> </bEAN> <BEAN class=com.aladdin.dao.ibatis.UserDaoiBatis id=userDao parent="baseDao" /> <BEAN class=com.aladdin.dao.ibatis.RoleDaoiBatis id=roleDao parent="baseDao" /> <BEAN class=com.aladdin.dao.ibatis.ResourceDaoiBatis id=resourceDao parent="baseDao" /> </bEANS><PRE>

 

此后就可以通过调用org.springframework.orm.ibatis.SqlMapClientTemplate的 public List queryForList(final String statementName, final Object parameterObject, final int skipResults, final int maxResults) throws DataAccessException 或 public PaginatedList queryForPaginatedList(final String statementName, final Object parameterObject, final int pageSize) throws DataAccessException 得到分页结果了。建议使用第一个方法，第二个方法返回的是PaginatedList，虽然使用简单，但是其获得指定页的数据是跨过我们的dao直接访问ibatis的，不方便统一管理。 <PRE>

 

* 上一篇：[Table的一些不常用的属性(ZZ)](http://blog.csdn.net/fulianglove/article/details/2708480)
* 下一篇：[海量数据库的查询优化(ZZ)](http://blog.csdn.net/fulianglove/article/details/2722784)
查看评论[]()

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场

个人资料

[![]()]( "我的博客主页")
fulianglove

* 访问：19467次
* 积分：496分
* 排名：第13741名

* 原创：22篇
* 转载：32篇
* 译文：0篇
* 评论：24条

文章搜索

[]()
文章分类

* [.NET](http://blog.csdn.net/fulianglove/article/category/404780)(3)
* [Java](http://blog.csdn.net/fulianglove/article/category/400749)(24)
* [JavaScript](http://blog.csdn.net/fulianglove/article/category/417945)(9)
* [Rails/Ruby](http://blog.csdn.net/fulianglove/article/category/403582)(0)
* [Spring](http://blog.csdn.net/fulianglove/article/category/403569)(1)
* [sql](http://blog.csdn.net/fulianglove/article/category/400748)(5)
* [数据库](http://blog.csdn.net/fulianglove/article/category/440985)(1)

文章存档

* [2009年02月](http://blog.csdn.net/fulianglove/article/month/2009/02)(1)
* [2009年01月](http://blog.csdn.net/fulianglove/article/month/2009/01)(2)
* [2008年12月](http://blog.csdn.net/fulianglove/article/month/2008/12)(2)
* [2008年10月](http://blog.csdn.net/fulianglove/article/month/2008/10)(6)
* [2008年09月](http://blog.csdn.net/fulianglove/article/month/2008/09)(5)
* [2008年08月](http://blog.csdn.net/fulianglove/article/month/2008/08)(9)
* [2008年07月](http://blog.csdn.net/fulianglove/article/month/2008/07)(10)
* [2008年06月](http://blog.csdn.net/fulianglove/article/month/2008/06)(14)
* [2008年05月](http://blog.csdn.net/fulianglove/article/month/2008/05)(5)
阅读排行

* [sqlserver中判断表是否存在]( "sqlserver中判断表是否存在") (2489)
* [js获取鼠标点击位置(很旧很旧。。。)]( "js获取鼠标点击位置(很旧很旧。。。)") (1487)
* [Response下载Content－ty...]( "Response下载Content－type：HTTP相应的Content类型(转贴)") (1330)
* [js利用object标签显示动态图片]( "js利用object标签显示动态图片") (1089)
* [Spring+Ibatis集成]( "Spring+Ibatis集成 ") (910)
* [EHCache使用(收集ZZ)]( "EHCache使用(收集ZZ)") (894)
* [window.print()使用]( "window.print()使用") (808)
* [JSP中文件下载(转贴)]( "JSP中文件下载(转贴) ") (745)
* [java LinkedList分析(ZZ...]( "java LinkedList分析(ZZ)") (690)
* [Ibatis SqlExecutor.e...]( "Ibatis SqlExecutor.executeQuery(ZZ)") (687)

评论排行

* [如何正确的使用Java序列化技术(转贴)]( "如何正确的使用Java序列化技术(转贴)") (5)
* [Oracle,Sql Server, M...]( "Oracle,Sql Server, MySql, DB2使用sql分页(ZZ)") (4)
* [js获取鼠标点击位置(很旧很旧。。。)]( "js获取鼠标点击位置(很旧很旧。。。)") (3)
* [js利用object标签显示动态图片]( "js利用object标签显示动态图片") (2)
* [sqlserver中判断表是否存在]( "sqlserver中判断表是否存在") (2)
* [JSP中文件下载(转贴)]( "JSP中文件下载(转贴) ") (2)
* [重复提交解决办法]( "重复提交解决办法") (1)
* [动态为表格添加行]( "动态为表格添加行") (1)
* [JS 事件注册]( "JS 事件注册") (1)
* [EHCache使用(收集ZZ)]( "EHCache使用(收集ZZ)") (1)
最新评论

* []()
* []()
* []()
* []()
* [不错，]()
* []()
* []()
* []()
* [怎么不写了？]()
* [SELECT *FROM pe...]( "SELECT *FROM person WHERE uid LIKE ? OR name LIKE ? limit " (currentPage-1)*lineSize "," lineSizemysql这样，sql2005该怎样表示啊？？？")

.NET

* [AjaxPage--Asp.Net2.0中的Callback机制](http://www.cnblogs.com/abei108/archive/2006/09/12/422125.html)
JavaScript

* [ExtJs](http://www.ajaxjs.com/docs/docs/)

java技术链接

* [[Java原型数据类型在分布式Cache中的同步处理]](http://scottding.blog.chinajavaworld.com/entry.jspa?id=2992)
* [JAVA NIO入门](https://www6.software.ibm.com/developerworks/cn/education/java/j-nio/tutorial/index.html)
* [HTML DOM Object 对象](http://www.w3school.com.cn/htmldom/dom_obj_object.asp)
* [孟宪会之精彩世界[.Net版]](http://dotnet.aspx.cc/ShowList.aspx?id=1&page=1)
* [java读取配置文件的几种方法](http://hbcui1984.javaeye.com/blog/56496)
* [HTML DOM Object 对象(2)](http://hi.baidu.com/fanyouan/blog/item/b1743e2e2e2bc5504fc226dd.html)
* [Digester学习笔记(一)BY竹笋炒肉](http://www.infomall.cn/cgi-bin/mallgate/20040514/http://hedong.3322.org/archives/000333.html)
* [Digester学习笔记(二)BY竹笋炒肉](http://www.infomall.cn/cgi-bin/mallgate/20040514/http://hedong.3322.org/archives/000336.html)
* [Digester学习笔记(三)BY竹笋炒肉](http://www.infomall.cn/cgi-bin/mallgate/20040514/http://hedong.3322.org/archives/000337.html)
* [博客文章BY竹笋炒肉](http://www.infomall.cn/cgi-bin/mallgate/20040514/http://hedong.3322.org/archives.html)
* [Struts处理请求全过程](http://gemini.javaeye.com/blog/163747)
* [IBM developerWorks 中国](http://www-128.ibm.com/developerworks/cn/)
* [利用java操作Excel文件](http://www.javaeye.com/topic/55844)
* [获取Java文件路径](http://www.blogjava.net/i369/articles/182041.html)
* [Spring+Ibatis集成开发实例](http://blog.csdn.net/apicescn/archive/2007/12/04/1916551.aspx)
Protocol Buffers

* [北极冰仔部落格--[第一个 Protocol Buffers 小程序：电话本]](http://hellobmw.com/archives/protocol-buffers-based-addressbook.html)
* [Protocol Buffer Basics: Java](http://code.google.com/apis/protocolbuffers/docs/javatutorial.html)
