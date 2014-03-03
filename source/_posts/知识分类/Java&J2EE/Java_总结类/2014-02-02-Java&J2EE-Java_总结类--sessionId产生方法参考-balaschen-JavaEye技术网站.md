---
layout: post
title: "sessionId产生方法参考 - balaschen - JavaEye技术网站"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# sessionId产生方法参考 - balaschen - JavaEye技术网站

[首页](http://www.javaeye.com/) [新闻](http://www.javaeye.com/news) [论坛](http://www.javaeye.com/forums) [问答](http://www.javaeye.com/ask) [博客](http://www.javaeye.com/blogs) [招聘](http://www.javaeye.com/job) [更多 ▼](http://balaschen.javaeye.com/blog/83882#)

[专栏](http://www.javaeye.com/wiki) [文摘](http://www.javaeye.com/articles) [圈子](http://www.javaeye.com/groups) [搜索](http://www.javaeye.com/google_search)

[您还未登录 !](http://balaschen.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://balaschen.javaeye.com/login) [注册](http://balaschen.javaeye.com/signup)

# [balaschen](http://balaschen.javaeye.com/)

永久域名 [http://balaschen.javaeye.com/](http://balaschen.javaeye.com/)

[rad rail plugin](http://balaschen.javaeye.com/blog/72498 "rad rail plugin") | [Spring Hibernate SessionFactory配置](http://balaschen.javaeye.com/blog/81611 "Spring Hibernate SessionFactory配置")

2007-05-28

### [sessionId产生方法参考](http://balaschen.javaeye.com/blog/83882)
tomcat
java 代码

1. protected synchronized String generateSessionId() {   
1.   
1.         byte random[] = new byte[16];   
1.   
1.         // Render the result as a String of hexadecimal digits   
1.         StringBuffer result = new StringBuffer();   
1.         int resultLenBytes = 0;   
1.         while (resultLenBytes < this.sessionIdLength) {   
1.             getRandomBytes(random);   
1.             random = getDigest().digest(random);   
1.             for (int j = 0;   
1.                     j < random.length && resultLenBytes < this.sessionIdLength;   
1.                     j++) {   
1.                 byte b1 = (byte) ((random[j] & 0xf0) >> 4);   
1.                 byte b2 = (byte) (random[j] & 0x0f);   
1.                 if (b1 < 10)   
1.                     result.append((char) ('0' + b1));   
1.                 else  
1.                     result.append((char) ('A' + (b1 - 10)));   
1.                 if (b2 < 10)   
1.                     result.append((char) ('0' + b2));   
1.                 else  
1.                     result.append((char) ('A' + (b2 - 10)));   
1.                 resultLenBytes++;   
1.             }   
1.         }   
1.         return (result.toString());   
1.   
1.     }  

 

hibernate uuid：
java 代码

1. public abstract class AbstractUUIDGenerator implements IdentifierGenerator {   
1.   
1.     private static final int IP;   
1.     static {   
1.         int ipadd;   
1.         try {   
1.             ipadd = BytesHelper.toInt( InetAddress.getLocalHost().getAddress() );   
1.         }   
1.         catch (Exception e) {   
1.             ipadd = 0;   
1.         }   
1.         IP = ipadd;   
1.     }   
1.     private static short counter = (short) 0;   
1.     private static final int JVM = (int) ( System.currentTimeMillis() >>> 8 );   
1.   
1.     public AbstractUUIDGenerator() {   
1.     }   
1.   
1.     /**  
1.      * Unique across JVMs on this machine (unless they load this class  
1.      * in the same quater second - very unlikely)  
1.      */  
1.     protected int getJVM() {   
1.         return JVM;   
1.     }   
1.   
1.     /**  
1.      * Unique in a millisecond for this JVM instance (unless there  
1.      * are > Short.MAX_VALUE instances created in a millisecond)  
1.      */  
1.     protected short getCount() {   
1.         synchronized(AbstractUUIDGenerator.class) {   
1.             if (counter<0) counter=0;   
1.             return counter++;   
1.         }   
1.     }   
1.   
1.     /**  
1.      * Unique in a local network  
1.      */  
1.     protected int getIP() {   
1.         return IP;   
1.     }   
1.   
1.     /**  
1.      * Unique down to millisecond  
1.      */  
1.     protected short getHiTime() {   
1.         return (short) ( System.currentTimeMillis() >>> 32 );   
1.     }   
1.     protected int getLoTime() {   
1.         return (int) System.currentTimeMillis();   
1.     }   
1.   
1.   
1. }   
1.   
1. public class UUIDHexGenerator extends AbstractUUIDGenerator implements Configurable {   
1.   
1.     private String sep = "";   
1.   
1.     protected String format(int intval) {   
1.         String formatted = Integer.toHexString(intval);   
1.         StringBuffer buf = new StringBuffer("00000000");   
1.         buf.replace( 8-formatted.length(), 8, formatted );   
1.         return buf.toString();   
1.     }   
1.   
1.     protected String format(short shortval) {   
1.         String formatted = Integer.toHexString(shortval);   
1.         StringBuffer buf = new StringBuffer("0000");   
1.         buf.replace( 4-formatted.length(), 4, formatted );   
1.         return buf.toString();   
1.     }   
1.   
1.     public Serializable generate(SessionImplementor session, Object obj) {   
1.         return new StringBuffer(36)   
1.             .append( format( getIP() ) ).append(sep)   
1.             .append( format( getJVM() ) ).append(sep)   
1.             .append( format( getHiTime() ) ).append(sep)   
1.             .append( format( getLoTime() ) ).append(sep)   
1.             .append( format( getCount() ) )   
1.             .toString();   
1.     }   
1.   
1.     public void configure(Type type, Properties params, Dialect d) {   
1.         sep = PropertiesHelper.getString("separator", params, "");   
1.     }   
1.   
1.     public static void main( String[] args ) throws Exception {   
1.         Properties props = new Properties();   
1.         props.setProperty("separator", "/");   
1.         IdentifierGenerator gen = new UUIDHexGenerator();   
1.         ( (Configurable) gen ).configure(Hibernate.STRING, props, null);   
1.         IdentifierGenerator gen2 = new UUIDHexGenerator();   
1.         ( (Configurable) gen2 ).configure(Hibernate.STRING, props, null);   
1.   
1.         for ( int i=0; i<10; i++) {   
1.             String id = (String) gen.generate(null, null);   
1.             System.out.println(id);   
1.             String id2 = (String) gen2.generate(null, null);   
1.             System.out.println(id2);   
1.         }   
1.   
1.     }   
1.   
1. }   
[rad rail plugin](http://balaschen.javaeye.com/blog/72498 "rad rail plugin") | [Spring Hibernate SessionFactory配置](http://balaschen.javaeye.com/blog/81611 "Spring Hibernate SessionFactory配置")

* 13:41
* 浏览 (870)
* [评论](http://balaschen.javaeye.com/blog/83882#comments) (0)
* [相关推荐](http://www.javaeye.com/wiki/topic/83882)
### 评论

[]()

### 发表评论

您还没有登录，请[登录](http://balaschen.javaeye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![balaschen的博客]( "balaschen的博客: balaschen")](http://balaschen.javaeye.com/)

balaschen

* 浏览: 69884 次
* 性别: ![Icon_minigender_1]( "男")
* ![]()
* [详细资料](http://balaschen.javaeye.com/blog/profile) [留言簿](http://balaschen.javaeye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://balaschen.javaeye.com/blog/user_visits)

[![wangzhongjie的博客]( "wangzhongjie的博客: wangzhongjie")](http://wangzhongjie.javaeye.com/)

[wangzhongjie](http://wangzhongjie.javaeye.com/)

[![yqwan的博客]( "yqwan的博客: ")](http://yqwan.javaeye.com/)

[yqwan](http://yqwan.javaeye.com/)
[![icecityman的博客]( "icecityman的博客: ")](http://icecityman.javaeye.com/)

[icecityman](http://icecityman.javaeye.com/)

[![chilongxph的博客]( "chilongxph的博客: ")](http://chilongxph.javaeye.com/)

[chilongxph](http://chilongxph.javaeye.com/)

### 博客分类

* [全部博客 (51)](http://balaschen.javaeye.com/)
* [生活 (0)](http://balaschen.javaeye.com/category/25461)
* [ror (3)](http://balaschen.javaeye.com/category/22646)
* [综合 (18)](http://balaschen.javaeye.com/category/13760)
* [spring (2)](http://balaschen.javaeye.com/category/14027)
* [MVC (3)](http://balaschen.javaeye.com/category/25457)
* [测试 (3)](http://balaschen.javaeye.com/category/25458)
* [hibernate (1)](http://balaschen.javaeye.com/category/25459)
* [系统(安装配置) (3)](http://balaschen.javaeye.com/category/25460)
* [asp.net (3)](http://balaschen.javaeye.com/category/38240)
### 其他分类

* [我的收藏](http://balaschen.javaeye.com/blog/favorite) (156)
* [我的论坛主题贴](http://balaschen.javaeye.com/blog/topic) (63)
* [我的所有论坛贴](http://balaschen.javaeye.com/blog/post) (203)
* [我的精华良好贴](http://balaschen.javaeye.com/blog/article) (3)

### 最近加入圈子

* [JBoss SEAM](http://seam.group.javaeye.com/)
* [函数式编程の道](http://wfp.group.javaeye.com/)
### 存档

* [2008-08](http://balaschen.javaeye.com/blog/monthblog/2008-08) (2)
* [2008-02](http://balaschen.javaeye.com/blog/monthblog/2008-02) (1)
* [2008-01](http://balaschen.javaeye.com/blog/monthblog/2008-01) (5)
* [更多存档...](http://balaschen.javaeye.com/blog/monthblog_more)

### 最新评论

* [FreeMarker解析字符串模板](http://balaschen.javaeye.com/blog/51591#comments "FreeMarker解析字符串模板")
Map valuesMap = HashMap(); valuesMap.put( ...
-- by [抛出异常的爱](http://loveexception.javaeye.com/)
* [添加用户、修改ad密码](http://balaschen.javaeye.com/blog/89107#comments "添加用户、修改ad密码")
是的，确实是证书没正确导入。后来我解决了。请问，我能不能添加或删除一个Group ...
-- by [Leecupn](http://leecupn.javaeye.com/)
* [添加用户、修改ad密码](http://balaschen.javaeye.com/blog/89107#comments "添加用户、修改ad密码")
应该是证书没正确导入到keyStord指定的位置
-- by [balaschen](http://balaschen.javaeye.com/)
* [添加用户、修改ad密码](http://balaschen.javaeye.com/blog/89107#comments "添加用户、修改ad密码")
你这明显是SSL没配置好。
-- by [balaschen](http://balaschen.javaeye.com/)
* [添加用户、修改ad密码](http://balaschen.javaeye.com/blog/89107#comments "添加用户、修改ad密码")
你好，我用你的方法，但是不知道怎么搞的，报以下异常: Exception in ...
-- by [Leecupn](http://leecupn.javaeye.com/)
### 评论排行榜

* [![Rss]()](http://balaschen.javaeye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://balaschen.javaeye.com/rss)
* [![Rss_xianguo]()](http://www.xianguo.com/subscribe.php?url=http://balaschen.javaeye.com/rss)
* [[什么是RSS?]](http://www.google.com/search?hl=zh-CN&q=RSS)

声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2009 JavaEye.com. All rights reserved. 上海炯耐计算机软件有限公司 [ 沪ICP备05023328号 ]
