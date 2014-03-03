---
layout: post
title: "使用ibatis操作数据库的封装"
categories: 框架汇总
tags: 
 - 框架汇总
 - ORM
 - ibatis
--- 

# 使用ibatis操作数据库的封装

**[使用ibatis操作数据库的封装](http://blog.csdn.net/boaifeng/article/details/5966940 "[置顶]使用ibatis操作数据库的封装")******

近期刚进入公司，也是本人的第一份正式工作，公司使用的ORM框架是ibatis，下面代码是对batis dao的一个封装，主要继承自spring的SqlMapClientDaoSupport，负责为单个Entity 提供CRUD操作的IBatis DAO基类，使用该基类，可以减少不少代码量。

[view plain](http://blog.csdn.net/boaifeng/article/details/5966940 "view plain")[copy to clipboard](http://blog.csdn.net/boaifeng/article/details/5966940 "copy to clipboard")[print](http://blog.csdn.net/boaifeng/article/details/5966940 "print")[?](http://blog.csdn.net/boaifeng/article/details/5966940 "?")

1.  package com.nfschina.utils.dao.ibatis  

2.    

3.  import java.io.Serializable;  

4.  import java.sql.Connection;  

5.  import java.sql.ResultSet;  

6.  import java.sql.SQLException;  

7.  import java.util.HashMap;  

8.  import java.util.List;  

9.  import java.util.Map;  

10.   

11. import org.apache.commons.beanutils.PropertyUtils;  

12. import org.apache.commons.lang.StringUtils;  

13. import org.springframework.util.Assert;  

14.   

15. import com.ibatis.sqlmap.engine.impl.SqlMapClientImpl;  

16. import com.ibatis.sqlmap.engine.mapping.sql.stat.StaticSql;  

17. import com.ibatis.sqlmap.engine.mapping.statement.MappedStatement;  

18.   

19. importcom.nfschina.utils.BaseException;  

20. import com.nfschina.utils.DataPage;  

21.   

22. import org.springframework.orm.ibatis.support.SqlMapClientDaoSupport  

23.   

24. /** 

25.  * IBatis Dao的泛型基类. 

26.  * 继承于Spring的SqlMapClientDaoSupport,提供分页函数和若干便捷查询方法，并对返回值作了泛型类型转换. 

27.  */  

28. @SuppressWarnings("unchecked")  

29. public class IBatisGenericDao extends SqlMapClientDaoSupport {  

30.   

31.  public static final String POSTFIX_INSERT = ".insert";  

32.   

33.  public static final String POSTFIX_UPDATE = ".update";  

34.   

35.  public static final String POSTFIX_DELETE = ".delete";  

36.   

37.  public static final String POSTFIX_DELETE_PRIAMARYKEY = ".deleteByPrimaryKey";  

38.   

39.  public static final String POSTFIX_SELECT = ".select";  

40.   

41.  public static final String POSTFIX_SELECTMAP = ".selectByMap";  

42.   

43.  public static final String POSTFIX_SELECTSQL = ".selectBySql";  

44.   

45.  public static final String POSTFIX_COUNT = ".count";  

46.   

47.  /** 

48.   * 根据ID获取对象 

49.   *  

50.   * @throws BaseException 

51.   * @throws SQLException  

52.   */  

53.  public <T> T get(Class<T> entityClass, Serializable id) throws BaseException, SQLException {  

54.   

55.   T o = (T) getSqlMapClient().queryForObject(entityClass.getName() + POSTFIX_SELECT, id);  

56.   if (o == null)  

57.    throw new BaseException(BaseException.DATA_NOTFOUND, "未找到实体: " + id);  

58.   return o;  

59.  }  

60.   

61.  /** 

62.   * 获取全部对象 

63.   * @throws SQLException  

64.   */  

65.  public <T> List<T> getAll(Class<T> entityClass) throws SQLException {  

66.   return getSqlMapClient().queryForList(entityClass.getName() + POSTFIX_SELECT, null);  

67.  }  

68.   

69.  /** 

70.   * 新增对象 

71.   * @throws SQLException  

72.   */  

73.  public void insert(Object o) throws SQLException {  

74.   getSqlMapClient().insert(o.getClass().getName() + POSTFIX_INSERT, o);  

75.  }  

76.   

77.  /** 

78.   * 保存对象 

79.   * @throws SQLException  

80.   */  

81.  public int update(Object o) throws SQLException {  

82.   return getSqlMapClient().update(o.getClass().getName() + POSTFIX_UPDATE, o);  

83.  }  

84.   

85.  /** 

86.   * 删除对象 

87.   * @throws SQLException  

88.   */  

89.  public int remove(Object o) throws SQLException {  

90.   return getSqlMapClient().delete(o.getClass().getName() + POSTFIX_DELETE, o);  

91.  }  

92.   

93.  /** 

94.   * 根据ID删除对象 

95.   * @throws SQLException  

96.   */  

97.  public <T> int removeById(Class<T> entityClass, Serializable id) throws SQLException {  

98.   return getSqlMapClient().delete(entityClass.getName() + POSTFIX_DELETE_PRIAMARYKEY, id);  

99.  }  

100.     

101.    /** 

102.     * map查询. 

103.     *  

104.     * @param map 

105.     *            包含各种属性的查询 

106.     * @throws SQLException  

107.     */  

108.    public <T> List<T> find(Class<T> entityClass, Map<String, Object> map) throws SQLException {  

109.     if (map == null)  

110.      return this.getSqlMapClient().queryForList(entityClass.getName() + POSTFIX_SELECT, null);  

111.     else {  

112.      map.put("findBy", "True");  

113.      return this.getSqlMapClient()  

114.        .queryForList(entityClass.getName() + POSTFIX_SELECTMAP, map);  

115.     }  

116.    }  

117.     

118.    /** 

119.     * sql 查询. 

120.     *  

121.     * @param sql 

122.     *            直接sql的语句(需要防止注入式攻击) 

123.     * @throws SQLException  

124.     */  

125.    public <T> List<T> find(Class<T> entityClass, String sql) throws SQLException {  

126.     Assert.hasText(sql);  

127.     if (StringUtils.isEmpty(sql))  

128.      return this.getSqlMapClient().queryForList(entityClass.getName() + POSTFIX_SELECT, null);  

129.     else  

130.      return this.getSqlMapClient()  

131.        .queryForList(entityClass.getName() + POSTFIX_SELECTSQL, sql);  

132.    }  

133.     

134.    /** 

135.     * 根据属性名和属性值查询对象. 

136.     *  

137.     * @return 符合条件的对象列表 

138.     * @throws SQLException  

139.     */  

140.    public <T> List<T> findBy(Class<T> entityClass, String name, Object value) throws SQLException {  

141.     Assert.hasText(name);  

142.     Map<String, Object> map = new HashMap<String, Object>();  

143.     map.put(name, value);  

144.     return find(entityClass, map);  

145.    }  

146.     

147.    /** 

148.     * 根据属性名和属性值查询对象. 

149.     *  

150.     * @return 符合条件的唯一对象 

151.     */  

152.    public <T> T findUniqueBy(Class<T> entityClass, String name, Object value) {  

153.     Assert.hasText(name);  

154.     Map<String, Object> map = new HashMap<String, Object>();  

155.     try {  

156.      PropertyUtils.getProperty(entityClass.newInstance(), name);  

157.      map.put(name, value);  

158.      map.put("findUniqueBy", "True");  

159.      return (T) getSqlMapClient().queryForObject(entityClass.getName() + POSTFIX_SELECTMAP,  

160.        map);  

161.     } catch (Exception e) {  

162.      logger.error("Error when propertie on entity," + e.getMessage(), e.getCause());  

163.      return null;  

164.     }  

165.     

166.    }  

167.     

168.    /** 

169.     * 根据属性名和属性值以Like AnyWhere方式查询对象. 

170.     * @throws SQLException  

171.     */  

172.    public <T> List<T> findByLike(Class<T> entityClass, String name, String value) throws SQLException {  

173.     Assert.hasText(name);  

174.     Map<String, Object> map = new HashMap<String, Object>();  

175.     map.put(name, value);  

176.     map.put("findLikeBy", "True");  

177.     return getSqlMapClient().queryForList(entityClass.getName() + POSTFIX_SELECTMAP, map);  

178.     

179.    }  

180.     

181.    /** 

182.     * 判断对象某些属性的值在数据库中不存在重复 

183.     *  

184.     * @param tableName 

185.     *            数据表名字 

186.     * @param names 

187.     *            在POJO里不能重复的属性列表,以逗号分割 如"name,loginid,password" <br> 

188.     *            FIXME how about in different schema? 

189.     */  

190.    public boolean isNotUnique(Object entity, String tableName, String names) {  

191.     try {  

192.      String primarykey;  

193.      Connection con = getSqlMapClient().getCurrentConnection();  

194.      ResultSet dbMetaData = con.getMetaData().getPrimaryKeys(con.getCatalog(), null, tableName);  

195.      dbMetaData.next();  

196.      if (dbMetaData.getRow() > 0) {  

197.       primarykey = dbMetaData.getString(4);  

198.       if (names.indexOf(primarykey) > -1)  

199.        return false;  

200.      } else {  

201.       return true;  

202.      }  

203.     

204.     } catch (SQLException e) {  

205.      logger.error(e.getMessage(), e);  

206.      return false;  

207.     }  

208.     return false;  

209.    }  

210.     

211.    /** 

212.     * 分页查询函数，使用PaginatedList. 

213.     *  

214.     * @param pageNo 

215.     *            页号,从0开始. 

216.     * @throws SQLException 

217.     */  

218.    @SuppressWarnings("rawtypes")  

219.    public DataPage pagedQuery(String sqlName, HashMap<String, Object> hashMap, Integer pageNo, Integer pageSize)  

220.      throws SQLException {  

221.     

222.     if (pageNo == null || pageSize == null) {  

223.      List list = getSqlMapClient().queryForList(sqlName, hashMap);  

224.      if (list == null || list.size() == 0) {  

225.       return new DataPage();  

226.      } else {  

227.       return new DataPage(0, list.size(), list.size(), list);  

228.      }  

229.     } else {  

230.      Assert.hasText(sqlName);  

231.      Assert.isTrue(pageNo >= 1, "pageNo should start from 1");  

232.      // Count查询   

233.      Integer totalCount = (Integer) getSqlMapClient().queryForObject(sqlName + ".Count", hashMap);  

234.     

235.      if (totalCount < 1) {  

236.       return new DataPage();  

237.      }  

238.     

239.      // 实际查询返回分页对象   

240.      int startIndex = DataPage.getStartOfPage(pageNo, pageSize);  

241.      hashMap.put("startIndex", startIndex);  

242.      hashMap.put("pageSize", pageSize);  

243.      List list = getSqlMapClient().queryForList(sqlName, hashMap);  

244.     

245.      return new DataPage(startIndex, totalCount, pageSize, list);  

246.     }  

247.    }  

248.     

249.    public String getMappedSQL(String sqlName) {  

250.     String sql = null;  

251.     

252.     SqlMapClientImpl sqlmap = (SqlMapClientImpl) getSqlMapClient();  

253.     

254.     MappedStatement stmt = sqlmap.getMappedStatement(sqlName);  

255.     StaticSql staticSql = (StaticSql) stmt.getSql();  

256.     sql = staticSql.getSql(null, null);  

257.     return sql;  

258.    }  

259.   }  

package com.nfschina.utils.dao.ibatis import java.io.Serializable; import java.sql.Connection; import java.sql.ResultSet; import java.sql.SQLException; import java.util.HashMap; import java.util.List; import java.util.Map; import org.apache.commons.beanutils.PropertyUtils; import org.apache.commons.lang.StringUtils; import org.springframework.util.Assert; import com.ibatis.sqlmap.engine.impl.SqlMapClientImpl; import com.ibatis.sqlmap.engine.mapping.sql.stat.StaticSql; import com.ibatis.sqlmap.engine.mapping.statement.MappedStatement; importcom.nfschina.utils.BaseException; import com.nfschina.utils.DataPage; import org.springframework.orm.ibatis.support.SqlMapClientDaoSupport /** * IBatis Dao的泛型基类. * 继承于Spring的SqlMapClientDaoSupport,提供分页函数和若干便捷查询方法，并对返回值作了泛型类型转换. */ @SuppressWarnings("unchecked") public class IBatisGenericDao extends SqlMapClientDaoSupport { public static final String POSTFIX_INSERT = ".insert"; public static final String POSTFIX_UPDATE = ".update"; public static final String POSTFIX_DELETE = ".delete"; public static final String POSTFIX_DELETE_PRIAMARYKEY = ".deleteByPrimaryKey"; public static final String POSTFIX_SELECT = ".select"; public static final String POSTFIX_SELECTMAP = ".selectByMap"; public static final String POSTFIX_SELECTSQL = ".selectBySql"; public static final String POSTFIX_COUNT = ".count"; /** * 根据ID获取对象 * * @throws BaseException * @throws SQLException */ public <T> T get(Class<T> entityClass, Serializable id) throws BaseException, SQLException { T o = (T) getSqlMapClient().queryForObject(entityClass.getName() + POSTFIX_SELECT, id); if (o == null) throw new BaseException(BaseException.DATA_NOTFOUND, "未找到实体: " + id); return o; } /** * 获取全部对象 * @throws SQLException */ public <T> List<T> getAll(Class<T> entityClass) throws SQLException { return getSqlMapClient().queryForList(entityClass.getName() + POSTFIX_SELECT, null); } /** * 新增对象 * @throws SQLException */ public void insert(Object o) throws SQLException { getSqlMapClient().insert(o.getClass().getName() + POSTFIX_INSERT, o); } /** * 保存对象 * @throws SQLException */ public int update(Object o) throws SQLException { return getSqlMapClient().update(o.getClass().getName() + POSTFIX_UPDATE, o); } /** * 删除对象 * @throws SQLException */ public int remove(Object o) throws SQLException { return getSqlMapClient().delete(o.getClass().getName() + POSTFIX_DELETE, o); } /** * 根据ID删除对象 * @throws SQLException */ public <T> int removeById(Class<T> entityClass, Serializable id) throws SQLException { return getSqlMapClient().delete(entityClass.getName() + POSTFIX_DELETE_PRIAMARYKEY, id); } /** * map查询. * * @param map * 包含各种属性的查询 * @throws SQLException */ public <T> List<T> find(Class<T> entityClass, Map<String, Object> map) throws SQLException { if (map == null) return this.getSqlMapClient().queryForList(entityClass.getName() + POSTFIX_SELECT, null); else { map.put("findBy", "True"); return this.getSqlMapClient() .queryForList(entityClass.getName() + POSTFIX_SELECTMAP, map); } } /** * sql 查询. * * @param sql * 直接sql的语句(需要防止注入式攻击) * @throws SQLException */ public <T> List<T> find(Class<T> entityClass, String sql) throws SQLException { Assert.hasText(sql); if (StringUtils.isEmpty(sql)) return this.getSqlMapClient().queryForList(entityClass.getName() + POSTFIX_SELECT, null); else return this.getSqlMapClient() .queryForList(entityClass.getName() + POSTFIX_SELECTSQL, sql); } /** * 根据属性名和属性值查询对象. * * @return 符合条件的对象列表 * @throws SQLException */ public <T> List<T> findBy(Class<T> entityClass, String name, Object value) throws SQLException { Assert.hasText(name); Map<String, Object> map = new HashMap<String, Object>(); map.put(name, value); return find(entityClass, map); } /** * 根据属性名和属性值查询对象. * * @return 符合条件的唯一对象 */ public <T> T findUniqueBy(Class<T> entityClass, String name, Object value) { Assert.hasText(name); Map<String, Object> map = new HashMap<String, Object>(); try { PropertyUtils.getProperty(entityClass.newInstance(), name); map.put(name, value); map.put("findUniqueBy", "True"); return (T) getSqlMapClient().queryForObject(entityClass.getName() + POSTFIX_SELECTMAP, map); } catch (Exception e) { logger.error("Error when propertie on entity," + e.getMessage(), e.getCause()); return null; } } /** * 根据属性名和属性值以Like AnyWhere方式查询对象. * @throws SQLException */ public <T> List<T> findByLike(Class<T> entityClass, String name, String value) throws SQLException { Assert.hasText(name); Map<String, Object> map = new HashMap<String, Object>(); map.put(name, value); map.put("findLikeBy", "True"); return getSqlMapClient().queryForList(entityClass.getName() + POSTFIX_SELECTMAP, map); } /** * 判断对象某些属性的值在数据库中不存在重复 * * @param tableName * 数据表名字 * @param names * 在POJO里不能重复的属性列表,以逗号分割 如"name,loginid,password" <br> * FIXME how about in different schema? */ public boolean isNotUnique(Object entity, String tableName, String names) { try { String primarykey; Connection con = getSqlMapClient().getCurrentConnection(); ResultSet dbMetaData = con.getMetaData().getPrimaryKeys(con.getCatalog(), null, tableName); dbMetaData.next(); if (dbMetaData.getRow() > 0) { primarykey = dbMetaData.getString(4); if (names.indexOf(primarykey) > -1) return false; } else { return true; } } catch (SQLException e) { logger.error(e.getMessage(), e); return false; } return false; } /** * 分页查询函数，使用PaginatedList. * * @param pageNo * 页号,从0开始. * @throws SQLException */ @SuppressWarnings("rawtypes") public DataPage pagedQuery(String sqlName, HashMap<String, Object> hashMap, Integer pageNo, Integer pageSize) throws SQLException { if (pageNo == null || pageSize == null) { List list = getSqlMapClient().queryForList(sqlName, hashMap); if (list == null || list.size() == 0) { return new DataPage(); } else { return new DataPage(0, list.size(), list.size(), list); } } else { Assert.hasText(sqlName); Assert.isTrue(pageNo >= 1, "pageNo should start from 1"); // Count查询 Integer totalCount = (Integer) getSqlMapClient().queryForObject(sqlName + ".Count", hashMap); if (totalCount < 1) { return new DataPage(); } // 实际查询返回分页对象 int startIndex = DataPage.getStartOfPage(pageNo, pageSize); hashMap.put("startIndex", startIndex); hashMap.put("pageSize", pageSize); List list = getSqlMapClient().queryForList(sqlName, hashMap); return new DataPage(startIndex, totalCount, pageSize, list); } } public String getMappedSQL(String sqlName) { String sql = null; SqlMapClientImpl sqlmap = (SqlMapClientImpl) getSqlMapClient(); MappedStatement stmt = sqlmap.getMappedStatement(sqlName); StaticSql staticSql = (StaticSql) stmt.getSql(); sql = staticSql.getSql(null, null); return sql; } }

 

 

[view plain](http://blog.csdn.net/boaifeng/article/details/5966940 "view plain")[copy to clipboard](http://blog.csdn.net/boaifeng/article/details/5966940 "copy to clipboard")[print](http://blog.csdn.net/boaifeng/article/details/5966940 "print")[?](http://blog.csdn.net/boaifeng/article/details/5966940 "?")

1.  package com.nfschina.utils.dao.ibatis;  

2.    

3.  import java.io.Serializable;  

4.  import java.lang.reflect.InvocationTargetException;  

5.  import java.sql.SQLException;  

6.  import java.util.List;  

7.  import java.util.Map;  

8.    

9.  import org.apache.commons.beanutils.PropertyUtils;  

10. import org.apache.commons.lang.StringUtils;  

11. import org.springframework.orm.ObjectRetrievalFailureException;  

12.   

13. import com.nfschina.utils.BaseException;  

14. import com.nfschina.utils.GenericsUtils;  

15.   

16. import   

17. /** 

18.  * 负责为单个Entity 提供CRUD操作的IBatis DAO基类. 

19.  * <p/> 

20.  * 子类只要在类定义时指定所管理Entity的Class, 即拥有对单个Entity对象的CRUD操作. 

21.  *  

22.  * <pre> 

23.  * public class UserManagerIbatis extends IBatisEntityDao<User> { 

24.  * } 

25.  * </pre> 

26.  */  

27. public class IBatisEntityDao<T> extends IBatisGenericDao {  

28.   

29.  /** 

30.   * DAO所管理的Entity类型. 

31.   */  

32.  protected Class<T> entityClass;  

33.   

34.  protected String primaryKeyName;  

35.   

36.  /** 

37.   * 在构造函数中将泛型T.class赋给entityClass. 

38.   */  

39.  @SuppressWarnings("unchecked")  

40.  public IBatisEntityDao() {  

41.   entityClass = (Class<T>) GenericsUtils.getSuperClassGenricType(getClass());  

42.  }  

43.   

44.  /** 

45.   * 根据属性名和属性值查询对象. 

46.   *  

47.   * @return 符合条件的对象列表 

48.   * @throws SQLException  

49.   */  

50.  public List<T> findBy(String name, Object value) throws SQLException {  

51.   return findBy(getEntityClass(), name, value);  

52.  }  

53.    

54.  /** 

55.   * 根据属性的名值对查询对象 

56.   * @param map 

57.   * @return 

58.   * @throws SQLException  

59.   */  

60.  public List<T> find(Map<String, Object> map) throws SQLException{  

61.   return find(getEntityClass(), map);  

62.  }  

63.    

64.  /** 

65.   * 根据属性的名值对查询唯一对象 

66.   * @param map 

67.   * @return 

68.   * @throws SQLException 

69.   */  

70.  public T findUniqueByMap(Map<String, Object> map) throws SQLException{  

71.   List<T> list = find(getEntityClass(), map);  

72.   if(list == null || list.size() <= 0){  

73.    return null;  

74.   }   

75.   return list.get(0);  

76.  }  

77.   

78.  /** 

79.   * 根据属性名和属性值以Like AnyWhere方式查询对象. 

80.   * @throws SQLException  

81.   */  

82.  public List<T> findByLike(String name, String value) throws SQLException {  

83.   return findByLike(getEntityClass(), name, value);  

84.  }  

85.   

86.  /** 

87.   * 根据属性名和属性值查询单个对象. 

88.   *  

89.   * @return 符合条件的唯一对象 

90.   */  

91.  public T findUniqueBy(String name, Object value) {  

92.   return findUniqueBy(getEntityClass(), name, value);  

93.  }  

94.   

95.  /** 

96.   * 根据ID获取对象. 

97.   *  

98.   * @throws BaseException 

99.   * @throws SQLException  

100.     */  

101.    public T get(Serializable id) throws BaseException, SQLException {  

102.     return get(getEntityClass(), id);  

103.    }  

104.     

105.    /** 

106.     * 获取全部对象. 

107.     * @throws SQLException  

108.     */  

109.    public List<T> getAll() throws SQLException {  

110.     return getAll(getEntityClass());  

111.    }  

112.     

113.    /** 

114.     * 取得entityClass. 

115.     * <p/> 

116.     * JDK1.4不支持泛型的子类可以抛开Class<T> entityClass,重载此函数达到相同效果。 

117.     */  

118.    protected Class<T> getEntityClass() {  

119.     return entityClass;  

120.    }  

121.     

122.    public String getPrimaryKeyName() {  

123.     if (StringUtils.isEmpty(primaryKeyName))  

124.      primaryKeyName = "id";  

125.     return primaryKeyName;  

126.    }  

127.     

128.    protected Object getPrimaryKeyValue(Object o) throws NoSuchMethodException, IllegalAccessException,  

129.      InvocationTargetException, InstantiationException {  

130.     return PropertyUtils.getProperty(entityClass.newInstance(), getPrimaryKeyName());  

131.    }  

132.     

133.    /** 

134.     * 根据ID移除对象. 

135.     * @throws SQLException  

136.     */  

137.    public int removeById(Serializable id) throws SQLException {  

138.     return removeById(getEntityClass(), id);  

139.    }  

140.     

141.    /** 

142.     * 保存对象. 为了实现IEntityDao 我在内部使用了insert和upate 2个方法. 

143.     * @throws SQLException  

144.     */  

145.    public void saveOrUpdate(Object o) throws SQLException {  

146.     Object primaryKey;  

147.     try {  

148.      primaryKey = getPrimaryKeyValue(o);  

149.     } catch (Exception e) {  

150.      throw new ObjectRetrievalFailureException(entityClass, e);  

151.     }  

152.     

153.     if (primaryKey == null)  

154.      insert(o);  

155.     else  

156.      update(o);  

157.    }  

158.     

159.    public void setPrimaryKeyName(String primaryKeyName) {  

160.     this.primaryKeyName = primaryKeyName;  

161.    }  

162.     

163.    public String getIdName(Class<?> clazz) {  

164.     return "id";  

165.    }  

166.   }  

package com.nfschina.utils.dao.ibatis; import java.io.Serializable; import java.lang.reflect.InvocationTargetException; import java.sql.SQLException; import java.util.List; import java.util.Map; import org.apache.commons.beanutils.PropertyUtils; import org.apache.commons.lang.StringUtils; import org.springframework.orm.ObjectRetrievalFailureException; import com.nfschina.utils.BaseException; import com.nfschina.utils.GenericsUtils; import /** * 负责为单个Entity 提供CRUD操作的IBatis DAO基类. * <p/> * 子类只要在类定义时指定所管理Entity的Class, 即拥有对单个Entity对象的CRUD操作. * * <pre> * public class UserManagerIbatis extends IBatisEntityDao<User> { * } * </pre> */ public class IBatisEntityDao<T> extends IBatisGenericDao { /** * DAO所管理的Entity类型. */ protected Class<T> entityClass; protected String primaryKeyName; /** * 在构造函数中将泛型T.class赋给entityClass. */ @SuppressWarnings("unchecked") public IBatisEntityDao() { entityClass = (Class<T>) GenericsUtils.getSuperClassGenricType(getClass()); } /** * 根据属性名和属性值查询对象. * * @return 符合条件的对象列表 * @throws SQLException */ public List<T> findBy(String name, Object value) throws SQLException { return findBy(getEntityClass(), name, value); } /** * 根据属性的名值对查询对象 * @param map * @return * @throws SQLException */ public List<T> find(Map<String, Object> map) throws SQLException{ return find(getEntityClass(), map); } /** * 根据属性的名值对查询唯一对象 * @param map * @return * @throws SQLException */ public T findUniqueByMap(Map<String, Object> map) throws SQLException{ List<T> list = find(getEntityClass(), map); if(list == null || list.size() <= 0){ return null; } return list.get(0); } /** * 根据属性名和属性值以Like AnyWhere方式查询对象. * @throws SQLException */ public List<T> findByLike(String name, String value) throws SQLException { return findByLike(getEntityClass(), name, value); } /** * 根据属性名和属性值查询单个对象. * * @return 符合条件的唯一对象 */ public T findUniqueBy(String name, Object value) { return findUniqueBy(getEntityClass(), name, value); } /** * 根据ID获取对象. * * @throws BaseException * @throws SQLException */ public T get(Serializable id) throws BaseException, SQLException { return get(getEntityClass(), id); } /** * 获取全部对象. * @throws SQLException */ public List<T> getAll() throws SQLException { return getAll(getEntityClass()); } /** * 取得entityClass. * <p/> * JDK1.4不支持泛型的子类可以抛开Class<T> entityClass,重载此函数达到相同效果。 */ protected Class<T> getEntityClass() { return entityClass; } public String getPrimaryKeyName() { if (StringUtils.isEmpty(primaryKeyName)) primaryKeyName = "id"; return primaryKeyName; } protected Object getPrimaryKeyValue(Object o) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException { return PropertyUtils.getProperty(entityClass.newInstance(), getPrimaryKeyName()); } /** * 根据ID移除对象. * @throws SQLException */ public int removeById(Serializable id) throws SQLException { return removeById(getEntityClass(), id); } /** * 保存对象. 为了实现IEntityDao 我在内部使用了insert和upate 2个方法. * @throws SQLException */ public void saveOrUpdate(Object o) throws SQLException { Object primaryKey; try { primaryKey = getPrimaryKeyValue(o); } catch (Exception e) { throw new ObjectRetrievalFailureException(entityClass, e); } if (primaryKey == null) insert(o); else update(o); } public void setPrimaryKeyName(String primaryKeyName) { this.primaryKeyName = primaryKeyName; } public String getIdName(Class<?> clazz) { return "id"; } }

 

 
