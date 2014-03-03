---
layout: post
title: "探索java多线程（连载）1 守护线程 - ikon - BlogJava"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_多线程
--- 

# 探索java多线程（连载）1 守护线程 - ikon - BlogJava

# [ikon](http://www.blogjava.net/ikon/)

posts - 1, comments - 0, trackbacks - 0, articles - 1   [BlogJava](http://www.blogjava.net/) :: [首页](http://www.blogjava.net/ikon/) :: [新随笔](http://www.blogjava.net/ikon/admin/EditPosts.aspx?opt=1) :: [联系](http://www.blogjava.net/ikon/contact.aspx?id=1) :: [聚合](http://www.blogjava.net/ikon/rss) [![]()](http://www.blogjava.net/ikon/rss) :: [管理](http://www.blogjava.net/ikon/admin/EditPosts.aspx) ![]()

### 日历

[<]( "Go to the previous month")2011年3月[>]( "Go to the next month")日一二三四五六2728123456789101112131415161718192021[22](http://www.blogjava.net/ikon/archive/2011/03/22.html)232425262728293031123456789

![]()

### 常用链接

* [我的随笔](http://www.blogjava.net/ikon/MyPosts.html)
* [我的评论](http://www.blogjava.net/ikon/MyComments.html)
* [我的参与](http://www.blogjava.net/ikon/OtherPosts.html)
![]()

### 留言簿

* [给我留言](http://www.blogjava.net/ikon/Contact.aspx?id=1)
* [查看公开留言](http://www.blogjava.net/ikon/default.aspx?opt=msg)
* [查看私人留言](http://www.blogjava.net/ikon/admin/MyMessages.aspx)

![]()

### 随笔档案

* [2011年3月 (1)](http://www.blogjava.net/ikon/archive/2011/03.html)
![]()

### 搜索

*  

![]()

### 最新评论 [![]()](http://www.blogjava.net/ikon/CommentsRSS.aspx) ## [探索java多线程（连载）1 守护线程]()

Posted on 2011-03-22 19:25 [ikon](http://www.blogjava.net/ikon/) 阅读(1692) [评论(0)](http://www.blogjava.net/ikon/archive/2011/03/22/346738.html#Post)  [编辑](http://www.blogjava.net/ikon/admin/EditPosts.aspx?postid=346738)  [收藏](http://www.blogjava.net/ikon/AddToFavorite.aspx?id=346738) ![]()

      在java中有一类线程，专门在后台提供服务，此类线程无需显式关闭，当程序结束了，它也就结束了，这就是守护线程 daemon thread。如果还有非守护线程的线程在执行，它就不会结束。      守护线程有何用处呢？让我们来看个实践中的例子。

      在我们的系统中经常应用各种配置文件（黑名单，禁用词汇），当修改配置文件后，一般要重启服务，系统才能够加载；当重启服务的代价比较高的情况下，这种加载方式不能满足我们的要求，这个时候守护线程该发挥它的作用了，它可以实时加载你的配置文件，无需重启。（当然，相当重要的配置文件，不推荐实时加载）
![]()package com.ikon.thread.daemon;
![]()
![]()import java.io.File;
![]()import java.util.*;
![]()
![]()![]()/** *//**
![]() * 文件监测
![]() * @author ikon99999
![]() * 
![]() */
![]()![]()public abstract class FileWatchdog extends Thread ![](){
![]()
![]() 
![]()  static final public long DEFAULT_DELAY = 20*1000; 
![]() 
![]()  
![]()  protected HashMap fileList;
![]() 
![]()  protected long delay = DEFAULT_DELAY; 
![]()  
![]()  boolean warnedAlready = false;
![]()  
![]()  boolean interrupted = false;
![]()
![]()  public static class Entity
![]()![]()  ![](){
![]()        File file;
![]()        long lastModify;
![]()        Entity(File file,long lastModify)
![]()![]()        ![](){
![]()            this.file = file;
![]()            this.lastModify = lastModify;
![]()        }
![]()  }
![]()  
![]()![]()  protected  FileWatchdog() ![](){
![]()      fileList = new HashMap ();
![]()    setDaemon(true);
![]()  }
![]()
![]() 
![]()![]()  public  void setDelay(long delay) ![](){
![]()    this.delay = delay;
![]()  }
![]()
![]()  public void addFile(File file)
![]()![]()  ![](){
![]()        fileList.put(file.getAbsolutePath(),new Entity(file,file.lastModified()));     
![]()  }
![]()  
![]()  public boolean contains(File file)
![]()![]()  ![](){
![]()        if( fileList.get(file.getAbsolutePath()) != null) return true;
![]()        else return false;
![]()  }
![]()  
![]()  abstract   protected   void doOnChange(File file);
![]()
![]()![]()  protected  void checkAndConfigure() ![](){
![]()      HashMap map = (HashMap)fileList.clone(); 
![]()      Iterator it = map.values().iterator();
![]()      
![]()      while( it.hasNext())
![]()![]()      ![](){
![]()          
![]()            Entity entity = (Entity)it.next();
![]()            
![]()            boolean fileExists;
![]()![]()            try ![](){
![]()              fileExists = entity.file.exists();
![]()            } catch(SecurityException  e) 
![]()![]()            ![](){
![]()              System.err.println ("Was not allowed to read check file existance, file:["+ entity.file .getAbsolutePath() +"].");
![]()              interrupted = true; 
![]()              return;
![]()            }
![]()
![]()            if(fileExists) 
![]()![]()            ![](){
![]()                
![]()              long l = entity.file.lastModified(); // this can also throw a SecurityException
![]()![]()              if(l > entity.lastModify) ![](){           // however, if we reached this point this
![]()                    entity.lastModify = l;              // is very unlikely.
![]()                    newThread(entity.file);
![]()              }
![]()            }
![]()            else 
![]()![]()            ![](){
![]()                System.err.println ("["+entity.file .getAbsolutePath()+"] does not exist.");
![]()            }
![]()      }
![]()  }
![]()  
![]()  private void newThread(File file)
![]()![]()  ![](){
![]()      class MyThread extends Thread
![]()![]()      ![](){
![]()            File f;
![]()            public MyThread(File f)
![]()![]()            ![](){
![]()                this.f = f;
![]()            }
![]()            
![]()            public void run()
![]()![]()            ![](){
![]()                doOnChange(f);
![]()            }
![]()      }
![]()      
![]()      MyThread mt = new MyThread(file);
![]()      mt.start();
![]()  }
![]()
![]()  public  void run() 
![]()![]()  ![](){    
![]()![]()    while(!interrupted) ![](){
![]()![]()      try ![](){
![]()        Thread.currentThread().sleep(delay);
![]()![]()      } catch(InterruptedException e) ![](){
![]()    // no interruption expected
![]()      }
![]()      checkAndConfigure();
![]()    }
![]()  }
![]()}
![]()

    FileWatchdog是个抽象类，本身是线程的子类；在构造函数中设置为守护线程；
    此类用hashmap维护着一个文件和最新修改时间值对，checkAndConfigure()方法用来检测哪些文件的修改时间更新了，如果发现文件更新了则调用doOnChange方法来完成监测逻辑；doOnChange方法是我们需要实现的；看下面关于一个黑名单服务的监测服务：
      

 1![]()package com.ikon.thread.daemon;
 2![]()
 3![]()import java.io.File;
 4![]()
 5![]()![]()/** *//**
 6![]() * 黑名单服务
 7![]() * @author ikon99999
 8![]() * 2011-3-21
 9![]() */
10![]()![]()public class BlacklistService ![](){
11![]()    private File configFile = new File("c:/blacklist.txt");
12![]()    
13![]()![]()    public void init() throws Exception![](){
14![]()        loadConfig();
15![]()        ConfigWatchDog dog = new ConfigWatchDog();
16![]()        dog.setName("daemon_demo_config_watchdog");//a
17![]()        dog.addFile(configFile);//b
18![]()        dog.start();//c
19![]()    }
20![]()    
21![]()![]()    public void loadConfig()![](){
22![]()![]()        try![](){
23![]()            Thread.sleep(1*1000);//d
24![]()        
25![]()            System.out.println("加载黑名单");
26![]()![]()        }catch(InterruptedException ex)![](){
27![]()            System.out.println("加载配置文件失败！");
28![]()        }
29![]()    }
30![]()        
31![]()![]()    public File getConfigFile() ![](){
32![]()        return configFile;
33![]()    }
34![]()
35![]()![]()    public void setConfigFile(File configFile) ![](){
36![]()        this.configFile = configFile;
37![]()    }
38![]()
39![]()
40![]()![]()    private class ConfigWatchDog extends FileWatchdog![](){
41![]()        
42![]()        @Override
43![]()![]()        protected void doOnChange(File file) ![](){
44![]()            System.out.println("文件"+file.getName()+"发生改变，重新加载");
45![]()            loadConfig();
46![]()        }
47![]()        
48![]()    }
49![]()    
50![]()![]()    public static void main(String[] args) throws Exception ![](){
51![]()        BlacklistService service = new BlacklistService();
52![]()        service.init();
53![]()        
54![]()        Thread.sleep(60*60*1000);//e
55![]()    }
56![]()}
57![]()
        ConfigWatchDog内部类实现了doOnChange(File file)方法，当文件被修改后，watchdog调用doOnChange方法完成重新加载操作；
        在blackservice的init方法中初始化watchdog线程；
        d：模拟文件加载耗时
        e：主要是防止主线程退出；
        其实上面的FileWatchdog就是取自log4j；
        

[新用户注册](http://www.blogjava.net/RequireRegister.aspx)  [刷新评论列表]()  

[]() [找优秀程序员，就在博客园](http://job.cnblogs.com/)
[网易有道诚聘CRM研发工程师](http://job.cnblogs.com/offer/12368/)
[锦江国际诚聘Java高级软件工程师](http://job.cnblogs.com/offer/11576/)
[福州几维网络诚聘Java服务端程序员](http://job.cnblogs.com/offer/12493/)
IT新闻：
· [开放，开放，开放 —— 垄断](http://news.cnblogs.com/n/101664/)
· [GNOME讨论放弃支持非Linux操作系统](http://news.cnblogs.com/n/101663/)
· [Chrome 13将隐藏地址栏](http://news.cnblogs.com/n/101662/)
· [联想：USB 3.0将在2012年成为主流](http://news.cnblogs.com/n/101661/)
· [意法半导体 CEO ：诺基亚 Windows Phone 将采用 U8500 双核芯片](http://news.cnblogs.com/n/101659/)   [博客园](http://www.cnblogs.com/)  [博问](http://home.cnblogs.com/q/)  [IT新闻](http://news.cnblogs.com/)  [Java程序员招聘](http://job.cnblogs.com/cate-java_programmer/) 标题  请输入标题 姓名  请输入你的姓名 主页 请输入验证码 验证码 *  ![]() 内容(请不要发表任何与政治相关的内容) 请输入评论内容 Remember Me?   [登录](http://www.blogjava.net/login.aspx?ReturnURL=http://www.blogjava.net/ikon/archive/2011/03/22/346738.html&SourceURL=/ikon/archive/2011/03/22/346738.html)       [使用Ctrl+Enter键可以直接提交]    推荐职位：
· [北京.NET 研发工程师 (北京捷报数据)](http://job.cnblogs.com/offer/5914/)
· [(北京).NET软件开发工程师(北京龙达)](http://job.cnblogs.com/offer/12527/)
· [厦门高级.NET软件工程师(服务于美国Amazon)](http://job.cnblogs.com/offer/11584/)
· [高级Web页面前端开发工程师(新蛋中国)](http://job.cnblogs.com/offer/10723/)
· [厦门Java服务端程序员(福州几维网络)](http://job.cnblogs.com/offer/12493/)
· [.NET 高级软件开发工程师 (新蛋中国)](http://job.cnblogs.com/offer/9051/)
· [北京ASP.NET 工程师（月薪12k）(北京盛安德)](http://job.cnblogs.com/offer/12439/)
· [厦门C#游戏客户端程序员 (福州几维网络)](http://job.cnblogs.com/offer/12492/)

博客园首页随笔：
· [字符编码浅谈（二）](http://www.cnblogs.com/kqingchao/archive/2011/05/20/character-encoding-2.html)
· [Windows Phone 7编程实践—必应地图导航](http://www.cnblogs.com/xuesong/archive/2011/05/20/2051892.html)
· [绕死你不偿命的UNICODE、_UNICODE、__TEXT、__T、_T、_TEXT、TEXT宏](http://www.cnblogs.com/ini_always/archive/2011/05/20/2050517.html)
· [学习笔记：JAVA RMI远程方法调用简单实例](http://www.cnblogs.com/leslies2/archive/2011/05/20/2051844.html)
· [关于CellSet转DataTable的改进方案](http://www.cnblogs.com/bobomouse/archive/2011/05/20/2051846.html)
知识库：
· [程序员的本质](http://kb.cnblogs.com/page/101423/)
· [Scrum之成败——从自身案例说起](http://kb.cnblogs.com/page/101345/)
· [清除代码异味](http://kb.cnblogs.com/page/101321/)
· [详解.NET程序集的加载规则](http://kb.cnblogs.com/page/101198/)
· [如何通过ildasm/ilasm修改assembly的IL代码](http://kb.cnblogs.com/page/101162/) 最简洁阅读版式：
[探索java多线程（连载）1 守护线程](http://archive.cnblogs.com/b/346738/) 网站导航:

[博客园](http://www.cnblogs.com/ "程序员的网上家园")   [IT新闻](http://news.cnblogs.com/)   [知识库](http://kb.cnblogs.com/)   [博客生活](http://www.cnweblog.com/)   [IT博客网](http://www.cnitblog.com/)   [C++博客](http://www.cppblog.com/)   [博问](http://space.cnblogs.com/q/ "IT问答")   [管理](http://www.blogjava.net/ikon/archive/2011/03/22/346738.html?opt=admin)    Powered by:
[BlogJava](http://www.blogjava.net/)
Copyright © ikon
