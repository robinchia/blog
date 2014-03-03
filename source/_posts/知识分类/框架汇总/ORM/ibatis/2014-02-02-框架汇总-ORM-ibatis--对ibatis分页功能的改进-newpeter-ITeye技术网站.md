---
layout: post
title: "对ibatis分页功能的改进 - newpeter - ITeye技术网站"
categories: 框架汇总
tags: 
 - 框架汇总
 - ORM
 - ibatis
--- 

# 对ibatis分页功能的改进 - newpeter - ITeye技术网站

[首页](http://www.iteye.com/) [资讯](http://www.iteye.com/news) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [群组](http://www.iteye.com/groups) [更多 ▼](http://newpeter.iteye.com/blog/323135#)

[招聘](http://www.iteye.com/job) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://newpeter.iteye.com/login "登录") [我的应用](http://www.iteye.com/all) [登录](http://newpeter.iteye.com/login) [注册](http://newpeter.iteye.com/signup)

# [newpeter](http://newpeter.iteye.com/)

永久域名 [http://newpeter.iteye.com/](http://newpeter.iteye.com/)

[当前比较全的Linux各种版本简介](http://newpeter.iteye.com/blog/323137 "当前比较全的Linux各种版本简介") | [常用正则表达式](http://newpeter.iteye.com/blog/323130 "常用正则表达式")

2009-02-04

### [对ibatis分页功能的改进](http://newpeter.iteye.com/blog/323135) **

[iBATIS](http://www.iteye.com/blogs/tag/iBATIS)[SQL](http://www.iteye.com/blogs/tag/SQL)[Hibernate](http://www.iteye.com/blogs/tag/Hibernate)[DAO](http://www.iteye.com/blogs/tag/DAO)[EXT](http://www.iteye.com/blogs/tag/EXT)
 

今天无意间看到了一篇关于这方面的文章，觉得是网上改进ibatis分页方面比较好的文章，这里转摘一下，希望能让更多的人用的到，也希望别人能把更好的解决方案贡献出来！
使ibatis支持hibernate式的物理分页

一直以来ibatis的分页都是通过滚动ResultSet实现的，应该算是逻辑分页吧。逻辑分页虽然能很干净地独立于特定数据库，但效率在多数情 况下不及特定数据库支持的物理分页，而hibernate的分页则是直接组装sql，充分利用了特定数据库的分页机制，效率相对较高。本文讲述的就是如何 在不重新编译ibatis源码的前提下，为ibatis引入hibernate式的物理分页机制。

基本思路就是找到ibatis执行sql的地方，截获sql并重新组装sql。通过分析ibatis源码知道，最终负责执行sql的类是 com.ibatis.sqlmap.engine.execution.SqlExecutor，此类没有实现任何接口，这多少有点遗憾，因为接口是相 对稳定契约，非大的版本更新，接口一般是不会变的，而类就相对易变一些，所以这里的代码只能保证对当前版本（2.1.7）的ibatis有效。下面是 SqlExecutor执行查询的方法：

 

 
/** * Long form of the method to execute a query * * @param request - the request scope * @param conn - the database connection * @param sql - the SQL statement to execute * @param parameters - the parameters for the statement * @param skipResults - the number of results to skip * @param maxResults - the maximum number of results to return * @param callback - the row handler for the query * * @throws SQLException - if the query fails */ public void executeQuery(RequestScope request, Connection conn, String sql, Object[] parameters, int skipResults, int maxResults, RowHandlerCallback callback) throws SQLException { ErrorContext errorContext = request.getErrorContext(); errorContext.setActivity("executing query"); errorContext.setObjectId(sql); PreparedStatement ps = null; ResultSet rs = null; try { errorContext.setMoreInfo("Check the SQL Statement (preparation failed)."); Integer rsType = request.getStatement().getResultSetType(); if (rsType != null) { ps = conn.prepareStatement(sql, rsType.intValue(), ResultSet.CONCUR_READ_ONLY); } else { ps = conn.prepareStatement(sql); } Integer fetchSize = request.getStatement().getFetchSize(); if (fetchSize != null) { ps.setFetchSize(fetchSize.intValue()); } errorContext.setMoreInfo("Check the parameters (set parameters failed)."); request.getParameterMap().setParameters(request, ps, parameters); errorContext.setMoreInfo("Check the statement (query failed)."); ps.execute(); rs = getFirstResultSet(ps); if (rs != null) { errorContext.setMoreInfo("Check the results (failed to retrieve results)."); handleResults(request, rs, skipResults, maxResults, callback); } // clear out remaining results while (ps.getMoreResults()); } finally { try { closeResultSet(rs); } finally { closeStatement(ps); } } }

 

 其中handleResults(request, rs, skipResults, maxResults, callback)一句用于处理分页，其实此时查询已经执行完毕，可以不必关心handleResults方法，但为清楚起见，下面来看看 handleResults的实现：

 
private void handleResults(RequestScope request, ResultSet rs, int skipResults, int maxResults, RowHandlerCallback callback) throws SQLException { try { request.setResultSet(rs); ResultMap resultMap = request.getResultMap(); if (resultMap != null) { // Skip Results if (rs.getType() != ResultSet.TYPE_FORWARD_ONLY) { if (skipResults > 0) { rs.absolute(skipResults); } } else { for (int i = 0; i < skipResults; i++) { if (!rs.next()) { break; } } } // Get Results int resultsFetched = 0; while ((maxResults == SqlExecutor.NO_MAXIMUM_RESULTS || resultsFetched < maxResults) && rs.next()) { Object[] columnValues = resultMap.resolveSubMap(request, rs).getResults(request, rs); callback.handleResultObject(request, columnValues, rs); resultsFetched++; } } } finally { request.setResultSet(null); } }

 此处优先使用的是ResultSet的absolute方法定位记录，是否支持absolute取决于具体数据库驱动，但一般当前版本的数据库都支 持该方法，如果不支持则逐条跳过前面的记录。由此可以看出如果数据库支持absolute，则ibatis内置的分页策略与特定数据库的物理分页效率差距 就在于物理分页查询与不分页查询在数据库中的执行效率的差距了。因为查询执行后读取数据前数据库并未把结果全部返回到内存，所以本身在存储占用上应该差距 不大，如果都使用索引，估计执行速度也差不太多。

 

继续我们的话题。其实只要在executeQuery执行前组装sql，然后将其传给executeQuery，并告诉handleResults 我们不需要逻辑分页即可。拦截executeQuery可以采用aop动态实现，也可直接继承SqlExecutor覆盖executeQuery来静态 地实现，相比之下后者要简单许多，而且由于SqlExecutor没有实现任何接口，比较易变，动态拦截反到增加了维护的工作量，所以我们下面来覆盖 executeQuery：

 
package com.aladdin.dao.ibatis.ext; import java.sql.Connection; import java.sql.SQLException; import org.apache.commons.logging.Log; import org.apache.commons.logging.LogFactory; import com.aladdin.dao.dialect.Dialect; import com.ibatis.sqlmap.engine.execution.SqlExecutor; import com.ibatis.sqlmap.engine.mapping.statement.RowHandlerCallback; import com.ibatis.sqlmap.engine.scope.RequestScope; public class LimitSqlExecutor extends SqlExecutor { private static final Log logger = LogFactory.getLog(LimitSqlExecutor.class); private Dialect dialect; private boolean enableLimit = true; public Dialect getDialect() { return dialect; } public void setDialect(Dialect dialect) { this.dialect = dialect; } public boolean isEnableLimit() { return enableLimit; } public void setEnableLimit(boolean enableLimit) { this.enableLimit = enableLimit; } @Override public void executeQuery(RequestScope request, Connection conn, String sql, Object[] parameters, int skipResults, int maxResults, RowHandlerCallback callback) throws SQLException { if ((skipResults != NO_SKIPPED_RESULTS || maxResults != NO_MAXIMUM_RESULTS) && supportsLimit()) { sql = dialect.getLimitString(sql, skipResults, maxResults); if(logger.isDebugEnabled()){ logger.debug(sql); } skipResults = NO_SKIPPED_RESULTS; maxResults = NO_MAXIMUM_RESULTS; } super.executeQuery(request, conn, sql, parameters, skipResults, maxResults, callback); } public boolean supportsLimit() { if (enableLimit && dialect != null) { return dialect.supportsLimit(); } return false; } }

 

 其中：

 
skipResults = NO_SKIPPED_RESULTS; maxResults = NO_MAXIMUM_RESULTS;

 

 告诉handleResults不分页（我们组装的sql已经使查询结果是分页后的结果了），此处引入了类似hibenate中的数据库方言接口Dialect，其代码如下：

 
package com.aladdin.dao.dialect; public interface Dialect { public boolean supportsLimit(); public String getLimitString(String sql, boolean hasOffset); public String getLimitString(String sql, int offset, int limit); }

 下面为Dialect接口的MySQL实现：package com.aladdin.dao.ibatis;

import java.io.Serializable; import java.util.List; import org.springframework.orm.ObjectRetrievalFailureException; import org.springframework.orm.ibatis.support.SqlMapClientDaoSupport; import com.aladdin.dao.ibatis.ext.LimitSqlExecutor; import com.aladdin.domain.BaseObject; import com.aladdin.util.ReflectUtil; import com.ibatis.sqlmap.client.SqlMapClient; import com.ibatis.sqlmap.engine.execution.SqlExecutor; import com.ibatis.sqlmap.engine.impl.ExtendedSqlMapClient; public abstract class BaseDaoiBatis extends SqlMapClientDaoSupport { private SqlExecutor sqlExecutor; public SqlExecutor getSqlExecutor() { return sqlExecutor; } public void setSqlExecutor(SqlExecutor sqlExecutor) { this.sqlExecutor = sqlExecutor; } public void setEnableLimit(boolean enableLimit) { if (sqlExecutor instanceof LimitSqlExecutor) { ((LimitSqlExecutor) sqlExecutor).setEnableLimit(enableLimit); } } public void initialize() throws Exception { if (sqlExecutor != null) { SqlMapClient sqlMapClient = getSqlMapClientTemplate() .getSqlMapClient(); if (sqlMapClient instanceof ExtendedSqlMapClient) { ReflectUtil.setFieldValue(((ExtendedSqlMapClient) sqlMapClient) .getDelegate(), "sqlExecutor", SqlExecutor.class, sqlExecutor); } } } ... }

 

 

 

分享到： [![]()]( "分享到新浪微博") [![]()]( "分享到腾讯微博")
[当前比较全的Linux各种版本简介](http://newpeter.iteye.com/blog/323137 "当前比较全的Linux各种版本简介") | [常用正则表达式](http://newpeter.iteye.com/blog/323130 "常用正则表达式")

* 23:31
* [评论](http://newpeter.iteye.com/blog/323135#comments) / 浏览 (1 / 2852)
* [相关推荐](http://www.iteye.com/wiki/blog/323135)
### 评论

[]()

1 楼 [vasuer](http://vasuer.iteye.com/) 2010-08-29   [引用](http://newpeter.iteye.com/blog/323135#)

te![]()

### 发表评论

您还没有登录，请[登录](http://newpeter.iteye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![newpeter的博客]( "newpeter的博客: newpeter")](http://newpeter.iteye.com/)

newpeter

* 浏览: 16997 次
* 性别: ![Icon_minigender_1]( "男")
* 来自: 深圳
* ![]()
* [详细资料](http://newpeter.iteye.com/blog/profile) [留言簿](http://newpeter.iteye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://newpeter.iteye.com/blog/user_visits)

[![ankonlcy的博客]( "ankonlcy的博客: ")](http://ankonlcy.iteye.com/)

[ankonlcy](http://ankonlcy.iteye.com/)

[![amazingstar的博客]( "amazingstar的博客: ")](http://amazingstar.iteye.com/)

[amazingstar](http://amazingstar.iteye.com/)
[![woozyangel的博客]( "woozyangel的博客: ")](http://woozyangel.iteye.com/)

[woozyangel](http://woozyangel.iteye.com/)

[![jdTangYin的博客]( "jdTangYin的博客: ")](http://jdtangyin.iteye.com/)

[jdTangYin](http://jdtangyin.iteye.com/)

### 博客分类

* [全部博客 (16)](http://newpeter.iteye.com/)
### 我的留言簿 [>>更多留言](http://newpeter.iteye.com/blog/guest_book)

### 其他分类

* [我的收藏](http://newpeter.iteye.com/blog/favorite) (6)
* [我的代码](http://newpeter.iteye.com/blog/code_favorite) (0)
* [我的论坛主题帖](http://newpeter.iteye.com/blog/topic) (2)
* [我的所有论坛帖](http://newpeter.iteye.com/blog/post) (5)
* [我的精华良好帖](http://newpeter.iteye.com/blog/article) (0)
### 最近加入群组

### 存档

* [2009-02](http://newpeter.iteye.com/blog/monthblog/2009-02) (13)
* [2009-01](http://newpeter.iteye.com/blog/monthblog/2009-01) (1)
* [2007-05](http://newpeter.iteye.com/blog/monthblog/2007-05) (2)
* [更多存档...](http://newpeter.iteye.com/blog/monthblog_more)
* [![Rss]()](http://newpeter.iteye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://newpeter.iteye.com/rss)
声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 ]
