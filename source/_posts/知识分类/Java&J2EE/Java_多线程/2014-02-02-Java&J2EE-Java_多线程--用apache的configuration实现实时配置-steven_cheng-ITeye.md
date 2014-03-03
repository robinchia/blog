---
layout: post
title: "用apache的configuration实现实时配置 - steven_cheng - ITeye"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_多线程
--- 

# 用apache的configuration实现实时配置 - steven_cheng - ITeye技术网站

[首页](http://www.iteye.com/) [新闻](http://www.iteye.com/news) [论坛](http://www.iteye.com/forums) [问答](http://www.iteye.com/ask) [博客](http://www.iteye.com/blogs) [招聘](http://www.iteye.com/job) [更多 ▼](http://steven-cheng.iteye.com/blog/70634#)

[专栏](http://www.iteye.com/wiki)  [群组](http://www.iteye.com/groups) [搜索](http://www.iteye.com/search)

[您还未登录 !](http://steven-cheng.iteye.com/login "登录") [我的应用](http://www.iteye.com/all) [登录](http://steven-cheng.iteye.com/login) [注册](http://steven-cheng.iteye.com/signup)

# [steven_cheng](http://steven-cheng.iteye.com/)

永久域名 [http://steven-cheng.iteye.com](http://steven-cheng.iteye.com/)

[异常处理](http://steven-cheng.iteye.com/blog/87667 "异常处理")

2006-03-24

### [用apache的configuration实现实时配置]()

关键字: java 开源
apache下commons有一个configeration包，对于做配置很方便，尤其是实时热配置。可以自动监测到配置文件的更改而reload配置文件。在项目中使用所以进行了一下封装。
java 代码

1. public class DefaultRealTimeXMLConfiger {   
1.        
1.     private static Log logger = LogFactory.getLog(DefaultRealTimeXMLConfiger.class);   
1.        
1.     private String fileName;   
1.        
1.     private long reloadPeriod;   
1.        
1.     private XMLConfiguration config;   
1.        
1.     public void init()   
1.     {   
1.         String filePath = GlobalConfigerImpl.getConfDir()+"/"+fileName;   
1.         logger.debug("will config with XML file["+filePath+"]");   
1.            
1.         File file = new File(filePath);   
1.         if (!file.exists() || !file.isFile()) {   
1.             logger.error(" can't find file[" + filePath + "]");   
1.             throw new IllegalArgumentException("config error! can't find file[" + filePath + "]");   
1.         }   
1.         this.init(file);   
1.     }   
1.        
1.     public void init(File file) {   
1.         try {   
1.             config = new XMLConfiguration(file);   
1.             FileChangedReloadingStrategy fs = new FileChangedReloadingStrategy();   
1.             fs.setConfiguration(config);   
1.                
1.             if(this.reloadPeriod>0)   
1.             {   
1.                 fs.setRefreshDelay(this.reloadPeriod);   
1.             }   
1.             config.setReloadingStrategy(fs);   
1.                
1.         } catch (ConfigurationException e) {   
1.             logger.error("error! configer error["+file.getPath()+"]");   
1.             logger.error(e);   
1.             e.printStackTrace();   
1.         }   
1.     }   
1.   
1.     public Object getProperty(String name) {   
1.         Object s = this.config.getProperty(name);   
1.         return s;   
1.     }   
1.   
1.     public String getString(String name) {   
1.         Object s = this.config.getProperty(name);   
1.         String result = null;   
1.         if (s != null)   
1.             result = (String) s;   
1.   
1.         return result;   
1.     }   
1.   
1.     public String[] getStringArray(String name) {   
1.         String[] target = this.config.getStringArray(name);   
1.            
1.         return target;   
1.     }   
1.        
1.   
1.     /**  
1.      * @return Returns the fileName.  
1.      */  
1.     public String getFileName() {   
1.         return fileName;   
1.     }   
1.   
1.     /**  
1.      * @param fileName The fileName to set.  
1.      */  
1.     public void setFileName(String fileName) {   
1.         this.fileName = fileName;   
1.     }   
1.   
1.     /**  
1.      * @return Returns the reloadPeriod.  
1.      */  
1.     public long getReloadPeriod() {   
1.         return reloadPeriod;   
1.     }   
1.   
1.     /**  
1.      * @param reloadPeriod The reloadPeriod to set.  
1.      */  
1.     public void setReloadPeriod(long reloadPeriod) {   
1.         this.reloadPeriod = reloadPeriod;   
1.     }   
1. }  
[异常处理](http://steven-cheng.iteye.com/blog/87667 "异常处理")

* 06:34
* 浏览 (988)
* [评论](http://steven-cheng.iteye.com/blog/70634#comments) (0)
* 分类: [java](http://steven-cheng.iteye.com/category/12345)
* [相关推荐](http://www.iteye.com/wiki/topic/70634)
### 评论

[]()

### 发表评论

### 表情图标

![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()![]()

字体颜色: 标准深红红色橙色棕色黄色绿色橄榄青色蓝色深蓝靛蓝紫色灰色白色黑色 字体大小: 标准1 (xx-small)2 (x-small)3 (small)4 (medium)5 (large)6 (x-large)7 (xx-large) 对齐: 标准居左居中居右

提示：选择您需要装饰的文字, 按上列按钮即可添加上相应的标签

您还没有登录，请[登录](http://steven-cheng.iteye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![steven_cheng的博客]( "steven_cheng的博客: steven_cheng")](http://steven-cheng.iteye.com/)

steven_cheng

* 浏览: 6938 次
* 来自: 北京
* ![]()
* [详细资料](http://steven-cheng.iteye.com/blog/profile) [留言簿](http://steven-cheng.iteye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://steven-cheng.iteye.com/blog/user_visits)

[![Loudyn的博客]( "Loudyn的博客: ")](http://loudyn.iteye.com/)

[Loudyn](http://loudyn.iteye.com/)

[![hepeng19861212的博客]( "hepeng19861212的博客: 火柴天堂")](http://hepeng19861212.iteye.com/)

[hepeng19861212](http://hepeng19861212.iteye.com/)
[![osacar的博客]( "osacar的博客: ")](http://osacar.iteye.com/)

[osacar](http://osacar.iteye.com/)

[![kevin_gzhz的博客]( "kevin_gzhz的博客: kevin_gzhz")](http://kevin-gzhz.iteye.com/)

[kevin_gzhz](http://kevin-gzhz.iteye.com/)

### 博客分类

* [全部博客 (8)](http://steven-cheng.iteye.com/)
* [默认类别 (0)](http://steven-cheng.iteye.com/category/4287)
* [java (6)](http://steven-cheng.iteye.com/category/12345)
* [单元测试和TDD (0)](http://steven-cheng.iteye.com/category/24850)
* [freemarker (1)](http://steven-cheng.iteye.com/category/48191)
* [linux (1)](http://steven-cheng.iteye.com/category/73657)
* [restlet (1)](http://steven-cheng.iteye.com/category/75601)
### 我的留言簿 [>>更多留言](http://steven-cheng.iteye.com/blog/guest_book)

* steven_cheng 写道restlet那片原文是你的吗，如果是给个原文连接。 ...
-- by [whaosoft](http://steven-cheng.iteye.com/blog/guest_book#6151)

### 其他分类

* [我的收藏](http://steven-cheng.iteye.com/blog/favorite) (15)
* [我的代码](http://steven-cheng.iteye.com/blog/code_favorite) (0)
* [我的论坛主题帖](http://steven-cheng.iteye.com/blog/topic) (1)
* [我的所有论坛帖](http://steven-cheng.iteye.com/blog/post) (8)
* [我的精华良好帖](http://steven-cheng.iteye.com/blog/article) (0)
* [我解决的问题](http://steven-cheng.iteye.com/blog/solution) (1)
### 最近加入群组

* [Restlet](http://restlet.group.iteye.com/)
* [FreeMarker](http://freemarker.group.iteye.com/)

### 存档

* [2010-02](http://steven-cheng.iteye.com/blog/monthblog/2010-02) (1)
* [2009-09](http://steven-cheng.iteye.com/blog/monthblog/2009-09) (2)
* [2009-07](http://steven-cheng.iteye.com/blog/monthblog/2009-07) (1)
* [更多存档...](http://steven-cheng.iteye.com/blog/monthblog_more)
### 评论排行榜

* [![Rss]()](http://steven-cheng.iteye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://steven-cheng.iteye.com/rss)

声明：ITeye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2011 ITeye.com. All rights reserved. [ 京ICP证110151号 ]
![]()
