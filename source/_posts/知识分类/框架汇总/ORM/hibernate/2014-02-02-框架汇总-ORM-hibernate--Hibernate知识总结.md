---
layout: post
title: "Hibernate知识总结"
categories: 框架汇总
tags: 
 - 框架汇总
 - ORM
 - hibernate
--- 

# Hibernate知识总结

[]()

# [Li-boy 奋斗的蜗牛](http://libooy.diandian.com/)

* [投稿](http://libooy.diandian.com/submit)
* [私信](http://libooy.diandian.com/inbox)
* [存档](http://libooy.diandian.com/archive)
* [随机文章](http://libooy.diandian.com/random)
* [关于]()
![Li-boy 奋斗的蜗牛]()

### 关于我

一天进步一点点！！！

26 Mar

# Hibernate知识总结

![]()

****

**1.Hibernate持久化对象的生命周期 (状态)  **

(1) 瞬态（自由态） (2) 持久态 (3) 托管（游离态）

**1.1自由态**

   持久化对象的自由态，指的是对象在内存中存在，但是在数据库中并

没有数据与其关联。比如Student student = new Student()，这里

的student对象就是一个自由态的持久化对象。

**1.2持久态**

   持久态指的是持久化对象处于由Hibernate管理的状态，这种状态下

持久化对象的变化将会被同步到数据库中。

session.beginTransaction();

User user = new User();

user.setUserName("James");

user.setUserPwd("123");

session.save(user);

session.getTransaction().commit();

**1.3游离态**

处于持久态的对象，在其对应的Session实例关闭后，此时对象迚入

游离态。也就是说Session实例是持久态对象的宿主环境，一旦宿主

环境失效，那么持久态对象迚入游离状态。

session.beginTransaction();

User user = new User();

user.setUserName("James");

user.setUserPwd("123");

Integer id = (Integer) session.save(user);

user.setUserPwd("456");

session.getTransaction().commit();

user.setUserPwd("789");

**游离态和自由态的区别**

区别就在于游离态对象可以再次与Session迚行关联而成为持久态对

象。

session.beginTransaction();

User user = new User();

user.setUserName("James");

user.setUserPwd("123");

Integer id = (Integer) session.save(user);

user.setUserPwd("456");

session.getTransaction().commit();

Session session2 = HibernateUtil.getSessionFactory().getCurrentS

session2.beginTransaction();

user.setUserPwd("789");

session2.update(user);

session2.getTransaction().commit();

自由态对象在数据库中没有数据与其对应，但是游离态对象在数据库

中有数据与其对应，只不过当前对象不在Session环境中而已。从对

象的是否有主键值可以做简单的判断。

session.beginTransaction();

User user = new User();

user.setUserName("James");

user.setUserPwd("123");

System.out.println(user.getId());

Integer id = (Integer) session.save(user);

session.getTransaction().commit();

System.out.println(user.getId());

如果我自己创建一个对象，并且给主键属性赋值，该值还在数据库中

存在，当前对象的状态不也是游离态了？

* 在Hibernate中根据主键判断对象是自由态还是游离态只是判断的

一个参考点，在Hibernate中还有更复杂的机制来判断一个对象的

状态，比如对象的version等等。

回到自由态

session.beginTransaction();

User user = (User) session.load(User.class, 120);

session.delete(user);

session.getTransaction().commit();

三种状态的转换 :

![]()

**load和get方法**

相同点：

   get和load方法都是利用对象的主键值获取相应的对象，并可以使对

象处于持久状态。

不同点：

   load方法获取对象时不会立即执行查询操作，而是在第一次使用对象

是再去执行查询操作。如果查询的对象在数据库中不存在，load方法

返回值不会为null，在第一次使用时抛出

org.hibernate.ObjectNotFoundException异常。

   使用get方法获取对象时会立即执行查询操作，并且对象在数据库中

不存在时返回null值。

**save和persist方法**

相同点：

save和persist方法都是将持久化对象保存到数据库中

区别：

sava方法成功执行后，返回持久化对象的ID

persist方法成功执行后，不会返回持久化对象的ID，persist方法是

JPA中推荐使用的方法  

**save和update方法**

save方法是将自由态的对象迚行保存。

update方法是将游离态的对象迚行保存。

update和saveOrUpdate方法

   如果一个对象是游离态戒持久态，对其执行update方法后会将对象

的修改同步到数据库中，如果该对象是自由态，则执行update方法

是没有作用的。

   在执行saveOrUpdate方法时该方法会自动判断对象的状态，如果为

自由态则执行save操作，如果为游离态戒持久态则执行update操作。

**update和merge方法**

   如果持久化对象在数据库中存在，使用merge操作时迚行同步操作。

如果对象在数据库不存在，merge对象则迚行保存操作。

   如果对象是游离状态，经过update操作后，对象转换为持久态。但

是经过merge操作后，对象状态依然是游离态。

**saveOrUpdate和merge方法**

saveOrUpdate方法和merge方法的区别在于如果session中存在两

个主键值相同的对象，迚行saveOrUpdate操作时会有异常抛出。这

时必须使用merge迚行操作。

session.beginTransaction();

User user = new User();

user.setId(3);

user.setUserName("aaaaaaaa");

user.setUserPwd("123123");

User user2 = (User) session.get(User.class, 3);

session.saveOrUpdate(user);//ERROR

session.getTransaction().commit();

clear方法和flush方法

clear方法是将Session中对象全部清除，当前在Session中的对象由

持久态转换为游离态。flush方法则是将持久态对象的更改同步到数据

库中。

session.beginTransaction();

User user = (User) session.get(User.class, 3);

user.setPassword("111");

session.flush();  

session.getTransaction().commit();

**2.Hibernate查询**

**2.1 HQL**

   HQL（Hibernate Query Language）提供了丰富灵活的查询方式，

使用HQL进行查询也是Hibernate官方推荐使用的查询方式。

   HQL在语法结构上和SQL语句十分的相同，所以可以很快的上手进行

使用。使用HQL需要用到Hibernate中的Query对象，该对象丏门执

行HQL方式的操作。

**1.查询所有**

session.beginTransaction();

String hql = "from User";

Query query = session.createQuery(hql);

List<User> userList = query.list();

for(User user:userList){

 System.out.println(user.getUserName());

}

session.getTransaction().commit();

**2.where**

session.beginTransaction();

String hql = "from User where userName = 'James'";

Query query = session.createQuery(hql);

List<User> userList = query.list();

for(User user:userList){

 System.out.println(user.getUserName());

}

session.getTransaction().commit();

在HQL中where语句中使用的是持久化对象的属性名，比如上面示例

中的userName。当然在HQL中也可以使用别名：

String hql = "from User as u where u.userName = 'James'";

**3.过滤条件**

在where语句中还可以使用各种过滤条件，如：=、<>、<、>、>=

、<=、between、not between、in、not in、is、like、and、or

等。

– from Student where age > 20;

– from Student where age between 20 and 30;

– from Student where name is null;

– from Student where name like ‘小%’;

– from Student where name like ‘小%’ and age < 30

**4.获取一个不完整对象**

**一列：**

session.beginTransaction();

String hql = "select userName from User"

Query query = session.createQuery(hql);

List nameList = query.list();

for(Object obj:nameList){

 System.out.println(obj);

}

session.getTransaction().commit();

**5.两列或多列：**

session.beginTransaction();

String hql = "select userName,userPwd from User";

Query query = session.createQuery(hql);

List nameList = query.list();

for(Object obj:nameList){

Object[] array = (Object[]) obj;

System.out.println("name:" + array[0]);

System.out.println("pwd:" + array[1]);

}

session.getTransaction().commit();

**6.统计和分组查询**

session.beginTransaction();

String hql = "select count(*),max(id) from User";

Query query = session.createQuery(hql);

List nameList = query.list();

for(Object obj:nameList){

Object[] array = (Object[]) obj;

System.out.println("count:" + array[0]);

System.out.println("max:" + array[1]);

}

session.getTransaction().commit();

**7.更多写法…**

消除重复： select distinct name from Student;

最大： select max(age) from Student;

行数： select count(age),age from Student group by age;

排序： from Student order by age;

**HQL占位符**

session.beginTransaction();

String hql = "from User where userName = ?";

Query query = session.createQuery(hql);

query.setString(0, "James");

List<User> userList = query.list();

for(User user:userList){

 System.out.println(user.getUserName());

}

session.getTransaction().commit();

**HQL引用占位符**

session.beginTransaction();

String hql = "from User where userName = :name";

Query query = session.createQuery(hql);

query.setParameter("name", "James");

List<User> userList = query.list();

for(User user:userList){

 System.out.println(user.getUserName());

}

session.getTransaction().commit();

**HQL分页**

session.beginTransaction();

String hql = "from User";

Query query = session.createQuery(hql);

**query.setFirstResult(0);**

**query.setMaxResults(2);**

List<User> userList = query.list();

for(User user:userList){

 System.out.println(user.getUserName());

}

session.getTransaction().commit();

**3.Criteria查询**

   Criteria对象提供了一种面向对象的方式查询数据库。Criteria对象需

要使用Session对象来获得。

一个Criteria对象表示对一个持久化类的查询。

**1.查询所有**

session.beginTransaction();

Criteria c = session.createCriteria(User.class);

List<User> userList = c.list();

for(User user:userList){

 System.out.println(user.getUserName());

}

session.getTransaction().commit();

**2.Where**

session.beginTransaction();

Criteria c = session.createCriteria(User.class);

c.add(Restrictions.eq("userName", "James"));

List<User> userList = c.list();

for(User user:userList){

 System.out.println(user.getUserName());

}

session.getTransaction().commit();

**3.Restrictions对象**

![]()

**where...and .... 语句**

session.beginTransaction();

**Criteria c = session.createCriteria(User.class)**

**c.add(Restrictions.like("userName", "J"));**

**c.add(Restrictions.eq("id", 120));**

List<User> userList = c.list();

for(User user:userList){

 System.out.println(user.getUserName());

}

session.getTransaction().commit();

**where...or .... 语句**

session.beginTransaction();

Criteria c = session.createCriteria(User.class);

**c.add(Restrictions.or(Restrictions.eq("userName", "James"),**

** Restrictions.eq("userName", "Alex")));**

List<User> userList = c.list();

for(User user:userList){

System.out.println(user.getUserName());

}

session.getTransaction().commit();

 

**获取唯一的记录**

session.beginTransaction();

Criteria c = session.createCriteria(User.class);

c.add(Restrictions.eq("id", 120));

User user = (User) c.uniqueResult();

System.out.println(user.getUserName());

session.getTransaction().commit();

**分页**

session.beginTransaction();

Criteria c = session.createCriteria(User.class);

c.setFirstResult(0);

c.setMaxResults(5);

List<User> userList = c.list();

for(User user:userList){

 System.out.println(user.getUserName());

}

session.getTransaction().commit();

**分组与统计**

session.beginTransaction();

Criteria c = session.createCriteria(User.class);

c.setProjection(Projections.sum("id"));

Object obj = c.uniqueResult();

System.out.println(obj);

session.getTransaction().commit();

Projections对象

![]()

**多个统计与分组**

session.beginTransaction();

Criteria c = session.createCriteria(User.class);

**ProjectionList projectionList = Projections.projectionList();**

**projectionList.add(Projections.sum("id"));**

**projectionList.add(Projections.min("id"));**

**c.setProjection(projectionList);**

Object[] obj = (Object[]) c.uniqueResult();

System.out.println("sum:" + obj[0]);

System.out.println("min:" + obj[1]);

session.getTransaction().commit();

**排序**

session.beginTransaction();

Criteria c = session.createCriteria(User.class);

**c.addOrder(Order.desc("id"));**

List<User> list = c.list();

for(User user : list){

 System.out.println(user.getUserName());

}

session.getTransaction().commit();

**4.使用原生SQL查询**

**1.查询出后必须封装才可以用：**

session.beginTransaction();

String sql = "select id,username,userpwd from t_user";

List list = session.createSQLQuery(sql).list();

for(Object item : list){

Object[] rows = (Object[]) item;

System.out.println("id:" + rows[0] + "username:"  

 + rows[1] + "userpwd:" + rows[2]);

}

session.getTransaction().commit();

**2.查询出一个集合可以直接用对象操作：**

session.beginTransaction();

String sql = "select id,username,userpwd from t_user";

SQLQuery query = session.createSQLQuery(sql).addEntity(User.class);

List<User> list = query.list();

for(User user : list){

 System.out.println(user.getUserName());

}

session.getTransaction().commit();

**3.查询出一个对象可以直接用对象操作**：

session.beginTransaction();

String sql = "select id,username,userpwd from t_user where id = 2";

SQLQuery query = session.createSQLQuery(sql).addEntity(User.class);

User user = (User) query.uniqueResult();

System.out.println(user.getUserName());

session.getTransaction().commit();

[hibernate](http://libooy.diandian.com/?tag=hibernate "hibernate")
[喜欢]() [热度 (4)]()

[分享]()

[]()
[返回首页](http://libooy.diandian.com/)

[]()
[上一篇](http://libooy.diandian.com/post/2012-03-28/14606048)

[下一篇](http://libooy.diandian.com/post/2012-03-26/18813130)
### [关于]( "关于")

一天进步一点点！！！

### 导航

* [RSS订阅](http://libooy.diandian.com/rss)
* [随机文章](http://libooy.diandian.com/random)
* [存档](http://libooy.diandian.com/archive)
* [私信](http://libooy.diandian.com/inbox "私信")
* [投稿](http://libooy.diandian.com/submit "投稿")
### 页面

* [首页](http://libooy.diandian.com/)

* © [Li-boy 奋斗的蜗牛](http://libooy.diandian.com/ "Li-boy 奋斗的蜗牛")
* Powered by [点点](http://www.diandian.com/ "点点网")
* Themed by [吹衣轻飏](http://www.yiyifly.com/ "舟遥遥以轻飏,风飘飘而吹衣")
