---
layout: post
title: "java线程安全总结 - 高级语言虚拟机"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# java线程安全总结 - 高级语言虚拟机

[您还未登录 !](http://hllvm.group.iteye.com/login "登录") [登录](http://hllvm.group.iteye.com/login) [注册](http://hllvm.group.iteye.com/signup)

[![ITeye3.0]( "ITeye-最棒的软件开发交流社区")](http://www.iteye.com/)

[]()

[群组首页](http://www.iteye.com/groups) → [编程语言](http://hllvm.group.iteye.com/groups/category/language) → [高级语言虚拟机](http://hllvm.group.iteye.com/) → [知识库](http://hllvm.group.iteye.com/group/wiki) → [线程安全](http://hllvm.group.iteye.com/group/wiki?category_id=304) → [java线程安全总结]()
原创作者: [jameswxx](http://www.javaeye.com/topic/806968)   阅读:9396次   评论:28条   更新时间:2011-05-26    

      最近想将java基础的一些东西都整理整理，写下来，这是对知识的总结，也是一种乐趣。已经拟好了提纲，大概分为这几个主题： java线程安全，java垃圾收集，java并发包详细介绍，java profile和jvm性能调优 。慢慢写吧。本人jameswxx原创文章，转载请注明出处，我费了很多心血，多谢了。关于java线程安全，网上有很多资料，我只想从自己的角度总结对这方面的考虑，有时候写东西是很痛苦的，知道一些东西，但想用文字说清楚，却不是那么容易。我认为要认识java线程安全，必须了解两个主要的点：java的内存模型，java的线程同步机制。特别是内存模型，java的线程同步机制很大程度上都是基于内存模型而设定的。后面我还会写java并发包的文章，详细总结如何利用java并发包编写高效安全的多线程并发程序。暂时写得比较仓促，后面会慢慢补充完善。

**浅谈java内存模型**
       不同的平台，内存模型是不一样的，但是jvm的内存模型规范是统一的。其实java的多线程并发问题最终都会反映在java的内存模型上，所谓线程安全无非是要控制多个线程对某个资源的有序访问或修改。总结java的内存模型，要解决两个主要的问题：可见性和有序性。我们都知道计算机有高速缓存的存在，处理器并不是每次处理数据都是取内存的。JVM定义了自己的内存模型，屏蔽了底层平台内存管理细节，对于java开发人员，要清楚在jvm内存模型的基础上，如果解决多线程的可见性和有序性。
       那么，何谓可见性？ 多个线程之间是不能互相传递数据通信的，它们之间的沟通只能通过共享变量来进行。Java内存模型（JMM）规定了jvm有主内存，主内存是多个线程共享的。当new一个对象的时候，也是被分配在主内存中，每个线程都有自己的工作内存，工作内存存储了主存的某些对象的副本，当然线程的工作内存大小是有限制的。当线程操作某个对象时，执行顺序如下：
 (1) 从主存复制变量到当前工作内存 (read and load)
 (2) 执行代码，改变共享变量值 (use and assign)
 (3) 用工作内存数据刷新主存相关内容 (store and write)

JVM规范定义了线程对主存的操作指令：read，load，use，assign，store，write。当一个共享变量在多个线程的工作内存中都有副本时，如果一个线程修改了这个共享变量，那么其他线程应该能够看到这个被修改后的值，这就是多线程的可见性问题。
        那么，什么是有序性呢 ？线程在引用变量时不能直接从主内存中引用,如果线程工作内存中没有该变量,则会从主内存中拷贝一个副本到工作内存中,这个过程为read-load,完成后线程会引用该副本。当同一线程再度引用该字段时,有可能重新从主存中获取变量副本(read-load-use),也有可能直接引用原来的副本(use),也就是说 read,load,use顺序可以由JVM实现系统决定。
        线程不能直接为主存中中字段赋值，它会将值指定给工作内存中的变量副本(assign),完成后这个变量副本会同步到主存储区(store-write)，至于何时同步过去，根据JVM实现系统决定.有该字段,则会从主内存中将该字段赋值到工作内存中,这个过程为read-load,完成后线程会引用该变量副本，当同一线程多次重复对字段赋值时,比如：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. for(int i=0;i<10;i++)  
1.  a++;  
for(int i=0;i<10;i++) a++;

 

线程有可能只对工作内存中的副本进行赋值,只到最后一次赋值后才同步到主存储区，所以assign,store,weite顺序可以由JVM实现系统决定。假设有一个共享变量x，线程a执行x=x+1。从上面的描述中可以知道x=x+1并不是一个原子操作，它的执行过程如下：
1 从主存中读取变量x副本到工作内存
2 给x加1
3 将x加1后的值写回主 存
如果另外一个线程b执行x=x-1，执行过程如下：
1 从主存中读取变量x副本到工作内存
2 给x减1
3 将x减1后的值写回主存
那么显然，最终的x的值是不可靠的。假设x现在为10，线程a加1，线程b减1，从表面上看，似乎最终x还是为10，但是多线程情况下会有这种情况发生：
1：线程a从主存读取x副本到工作内存，工作内存中x值为10
2：线程b从主存读取x副本到工作内存，工作内存中x值为10
3：线程a将工作内存中x加1，工作内存中x值为11
4：线程a将x提交主存中，主存中x为11
5：线程b将工作内存中x值减1，工作内存中x值为9
6：线程b将x提交到中主存中，主存中x为9
同样，x有可能为11，如果x是一个银行账户，线程a存款，线程b扣款，显然这样是有严重问题的，要解决这个问题，必须保证线程a和线程b是有序执行的，并且每个线程执行的加1或减1是一个原子操作。看看下面代码：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public class Account {  
1.   
1.     private int balance;  
1.   
1.     public Account(int balance) {  
1.         this.balance = balance;  
1.     }  
1.   
1.     public int getBalance() {  
1.         return balance;  
1.     }  
1.   
1.     public void add(int num) {  
1.         balance = balance + num;  
1.     }  
1.   
1.     public void withdraw(int num) {  
1.         balance = balance - num;  
1.     }  
1.   
1.     public static void main(String[] args) throws InterruptedException {  
1.         Account account = new Account(1000);  
1.         Thread a = new Thread(new AddThread(account, 20), "add");  
1.         Thread b = new Thread(new WithdrawThread(account, 20), "withdraw");  
1.         a.start();  
1.         b.start();  
1.         a.join();  
1.         b.join();  
1.         System.out.println(account.getBalance());  
1.     }  
1.   
1.     static class AddThread implements Runnable {  
1.         Account account;  
1.         int     amount;  
1.   
1.         public AddThread(Account account, int amount) {  
1.             this.account = account;  
1.             this.amount = amount;  
1.         }  
1.   
1.         public void run() {  
1.             for (int i = 0; i < 200000; i++) {  
1.                 account.add(amount);  
1.             }  
1.         }  
1.     }  
1.   
1.     static class WithdrawThread implements Runnable {  
1.         Account account;  
1.         int     amount;  
1.   
1.         public WithdrawThread(Account account, int amount) {  
1.             this.account = account;  
1.             this.amount = amount;  
1.         }  
1.   
1.         public void run() {  
1.             for (int i = 0; i < 100000; i++) {  
1.                 account.withdraw(amount);  
1.             }  
1.         }  
1.     }  
1. }  
public class Account { private int balance; public Account(int balance) { this.balance = balance; } public int getBalance() { return balance; } public void add(int num) { balance = balance + num; } public void withdraw(int num) { balance = balance - num; } public static void main(String[] args) throws InterruptedException { Account account = new Account(1000); Thread a = new Thread(new AddThread(account, 20), "add"); Thread b = new Thread(new WithdrawThread(account, 20), "withdraw"); a.start(); b.start(); a.join(); b.join(); System.out.println(account.getBalance()); } static class AddThread implements Runnable { Account account; int amount; public AddThread(Account account, int amount) { this.account = account; this.amount = amount; } public void run() { for (int i = 0; i < 200000; i++) { account.add(amount); } } } static class WithdrawThread implements Runnable { Account account; int amount; public WithdrawThread(Account account, int amount) { this.account = account; this.amount = amount; } public void run() { for (int i = 0; i < 100000; i++) { account.withdraw(amount); } } } }

 

第一次执行结果为10200，第二次执行结果为1060，每次执行的结果都是不确定的，因为线程的执行顺序是不可预见的。这是java同步产生的根源，synchronized关键字保证了多个线程对于同步块是互斥的，synchronized作为一种同步手段，解决java多线程的执行有序性和内存可见性，而volatile关键字之解决多线程的内存可见性问题。后面将会详细介绍。

**synchronized关键字**
        上面说了，java用synchronized关键字做为多线程并发环境的执行有序性的保证手段之一。当一段代码会修改共享变量，这一段代码成为互斥区或临界区，为了保证共享变量的正确性，synchronized标示了临界区。典型的用法如下：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. synchronized(锁){  
1.      临界区代码  
1. }   
synchronized(锁){ 临界区代码 }

 

为了保证银行账户的安全，可以操作账户的方法如下：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public synchronized void add(int num) {  
1.      balance = balance + num;  
1. }  
1. public synchronized void withdraw(int num) {  
1.      balance = balance - num;  
1. }  
public synchronized void add(int num) { balance = balance + num; } public synchronized void withdraw(int num) { balance = balance - num; }

 

刚才不是说了synchronized的用法是这样的吗：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. synchronized(锁){  
1. 临界区代码  
1. }  
synchronized(锁){ 临界区代码 }

 

那么对于public synchronized void add(int num)这种情况，意味着什么呢？其实这种情况，锁就是这个方法所在的对象。同理，如果方法是public  static synchronized void add(int num)，那么锁就是这个方法所在的class。
        理论上，每个对象都可以做为锁，但一个对象做为锁时，应该被多个线程共享，这样才显得有意义，在并发环境下，一个没有共享的对象作为锁是没有意义的。假如有这样的代码：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public class ThreadTest{  
1.   public void test(){  
1.      Object lock=new Object();  
1.      synchronized (lock){  
1.         //do something  
1.      }  
1.   }  
1. }  
public class ThreadTest{ public void test(){ Object lock=new Object(); synchronized (lock){ //do something } } }

 

lock变量作为一个锁存在根本没有意义，因为它根本不是共享对象，每个线程进来都会执行Object lock=new Object();每个线程都有自己的lock，根本不存在锁竞争。
        每个锁对象都有两个队列，一个是就绪队列，一个是阻塞队列，就绪队列存储了将要获得锁的线程，阻塞队列存储了被阻塞的线程，当一个被线程被唤醒(notify)后，才会进入到就绪队列，等待cpu的调度。当一开始线程a第一次执行account.add方法时，jvm会检查锁对象account的就绪队列是否已经有线程在等待，如果有则表明account的锁已经被占用了，由于是第一次运行，account的就绪队列为空，所以线程a获得了锁，执行account.add方法。如果恰好在这个时候，线程b要执行account.withdraw方法，因为线程a已经获得了锁还没有释放，所以线程b要进入account的就绪队列，等到得到锁后才可以执行。
一个线程执行临界区代码过程如下：
1 获得同步锁
2 清空工作内存
3 从主存拷贝变量副本到工作内存
4 对这些变量计算
5 将变量从工作内存写回到主存
6 释放锁
可见，synchronized既保证了多线程的并发有序性，又保证了多线程的内存可见性。
**生产者/消费者模式**
        生产者/消费者模式其实是一种很经典的线程同步模型，很多时候，并不是光保证多个线程对某共享资源操作的互斥性就够了，往往多个线程之间都是有协作的。
        假设有这样一种情况，有一个桌子，桌子上面有一个盘子，盘子里只能放一颗鸡蛋，A专门往盘子里放鸡蛋，如果盘子里有鸡蛋，则一直等到盘子里没鸡蛋，B专门从盘子里拿鸡蛋，如果盘子里没鸡蛋，则等待直到盘子里有鸡蛋。其实盘子就是一个互斥区，每次往盘子放鸡蛋应该都是互斥的，A的等待其实就是主动放弃锁，B等待时还要提醒A放鸡蛋。
如何让线程主动释放锁
很简单，调用锁的wait()方法就好。wait方法是从Object来的，所以任意对象都有这个方法。看这个代码片段：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. Object lock=new Object();//声明了一个对象作为锁  
1.    synchronized (lock) {  
1.        balance = balance - num;  
1.        //这里放弃了同步锁，好不容易得到，又放弃了  
1.        lock.wait();  
1. }  
Object lock=new Object();//声明了一个对象作为锁 synchronized (lock) { balance = balance - num; //这里放弃了同步锁，好不容易得到，又放弃了 lock.wait(); }

 

如果一个线程获得了锁lock，进入了同步块，执行lock.wait()，那么这个线程会进入到lock的阻塞队列。如果调用lock.notify()则会通知阻塞队列的某个线程进入就绪队列。
声明一个盘子，只能放一个鸡蛋
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. package com.jameswxx.synctest;  
1. public class Plate{  
1.   List<Object> eggs=new ArrayList<Object>();  
1.   public synchronized  Object getEgg(){  
1.      if(eggs.size()==0){  
1.         try{  
1.             wait();  
1.         }catch(InterruptedException e){  
1.         }  
1.      }  
1.   
1.     Object egg=eggs.get(0);  
1.     eggs.clear();//清空盘子  
1.     notify();//唤醒阻塞队列的某线程到就绪队列  
1.     return egg;  
1. }  
1.   
1.  public synchronized  void putEgg(Object egg){  
1.     If(eggs.size()>0){  
1.       try{  
1.          wait();  
1.       }catch(InterruptedException e){  
1.       }  
1.     }  
1.     eggs.add(egg);//往盘子里放鸡蛋  
1.     notify();//唤醒阻塞队列的某线程到就绪队列  
1.   }  
1. }  
package com.jameswxx.synctest; public class Plate{ List<Object> eggs=new ArrayList<Object>(); public synchronized Object getEgg(){ if(eggs.size()==0){ try{ wait(); }catch(InterruptedException e){ } } Object egg=eggs.get(0); eggs.clear();//清空盘子 notify();//唤醒阻塞队列的某线程到就绪队列 return egg; } public synchronized void putEgg(Object egg){ If(eggs.size()>0){ try{ wait(); }catch(InterruptedException e){ } } eggs.add(egg);//往盘子里放鸡蛋 notify();//唤醒阻塞队列的某线程到就绪队列 } }

 

声明一个Plate对象为plate，被线程A和线程B共享，A专门放鸡蛋，B专门拿鸡蛋。假设
1 开始，A调用plate.putEgg方法，此时eggs.size()为0，因此顺利将鸡蛋放到盘子，还执行了notify()方法，唤醒锁的阻塞队列的线程，此时阻塞队列还没有线程。
2 又有一个A线程对象调用plate.putEgg方法，此时eggs.size()不为0，调用wait()方法，自己进入了锁对象的阻塞队列。
3 此时，来了一个B线程对象，调用plate.getEgg方法，eggs.size()不为0，顺利的拿到了一个鸡蛋，还执行了notify()方法，唤醒锁的阻塞队列的线程，此时阻塞队列有一个A线程对象，唤醒后，它进入到就绪队列，就绪队列也就它一个，因此马上得到锁，开始往盘子里放鸡蛋，此时盘子是空的，因此放鸡蛋成功。
4 假设接着来了线程A，就重复2；假设来料线程B，就重复3。
整个过程都保证了放鸡蛋，拿鸡蛋，放鸡蛋，拿鸡蛋。

**volatile关键字**
       volatile是java提供的一种同步手段，只不过它是轻量级的同步，为什么这么说，因为volatile只能保证多线程的内存可见性，不能保证多线程的执行有序性。而最彻底的同步要保证有序性和可见性，例如synchronized。任何被volatile修饰的变量，都不拷贝副本到工作内存，任何修改都及时写在主存。因此对于Valatile修饰的变量的修改，所有线程马上就能看到，但是volatile不能保证对变量的修改是有序的。什么意思呢？假如有这样的代码：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public class VolatileTest{  
1.   public volatile int a;  
1.   public void add(int count){  
1.        a=a+count;  
1.   }  
1. }  
public class VolatileTest{ public volatile int a; public void add(int count){ a=a+count; } }

 

        当一个VolatileTest对象被多个线程共享，a的值不一定是正确的，因为a=a+count包含了好几步操作，而此时多个线程的执行是无序的，因为没有任何机制来保证多个线程的执行有序性和原子性。volatile存在的意义是，任何线程对a的修改，都会马上被其他线程读取到，因为直接操作主存，没有线程对工作内存和主存的同步。所以，volatile的使用场景是有限的，在有限的一些情形下可以使用 volatile 变量替代锁。要使 volatile 变量提供理想的线程安全,必须同时满足下面两个条件:
1)对变量的写操作不依赖于当前值。
2)该变量没有包含在具有其他变量的不变式中
volatile只保证了可见性，所以Volatile适合直接赋值的场景，如
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public class VolatileTest{  
1.   public volatile int a;  
1.   public void setA(int a){  
1.       this.a=a;  
1.   }  
1. }  
public class VolatileTest{ public volatile int a; public void setA(int a){ this.a=a; } }

 

在没有volatile声明时，多线程环境下，a的最终值不一定是正确的，因为this.a=a;涉及到给a赋值和将a同步回主存的步骤，这个顺序可能被打乱。如果用volatile声明了，读取主存副本到工作内存和同步a到主存的步骤，相当于是一个原子操作。所以简单来说，volatile适合这种场景：一个变量被多个线程共享，线程直接给这个变量赋值。这是一种很简单的同步场景，这时候使用volatile的开销将会非常小。

评论 共 28 条 请[登录](http://hllvm.group.iteye.com/login)后发表评论 []()

### 28 楼 [geyingchen](http://geyingchen.iteye.com/ "geyingchen") 2012-11-16 13:48

这样行么，如果多次调用getegg ,然后putegg,唤醒  getegg ，getegg在唤醒getegg，这样就会报错额，你notify 唤醒的又不一定是另外一个方法的线程，
### 27 楼 [hqf2009](http://hqf2009.iteye.com/ "hqf2009") 2012-09-19 15:07

受教颇多……对线程同步了解更进一步了。

### 26 楼 [lijun880312](http://lijun880312.iteye.com/ "lijun880312") 2012-09-03 14:59

楼主诸葛之思，神来之笔啊！![]()
### 25 楼 [zhuangyuann](http://zhuangyuann.iteye.com/ "zhuangyuann") 2012-07-26 19:23

收益良多，谢谢分享

### 24 楼 [bbym010](http://bbym010.iteye.com/ "bbym010") 2012-07-08 11:27

还厉害，博主，线程相关其他文章在哪
### 23 楼 [yangcheng1230](http://yangcheng1230.iteye.com/ "yangcheng1230") 2012-06-26 14:41

楼主大才，谢谢分享。

### 22 楼 [爱上边城](http://13966692733-163-com.iteye.com/ "爱上边城") 2012-06-21 14:41

![]() 线程说的真好。感谢您的慷慨奉献
### 21 楼 [tiandizhiguai](http://tiandizhiguai.iteye.com/ "tiandizhiguai") 2012-06-21 10:28

写的妙![]()

### 20 楼 [xieyaxiong](http://xieyaxiong.iteye.com/ "xieyaxiong") 2012-06-18 17:52

![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]()
### 19 楼 [358397023](http://358397023.iteye.com/ "358397023") 2012-03-30 21:28

LZ写得好啊！学习了

### 18 楼 [Stanly_xia](http://stanly-xia.iteye.com/ "Stanly_xia") 2012-03-22 15:59

写的不错，受益匪浅。谢谢……![]()
### 17 楼 [fs216](http://fs216.iteye.com/ "fs216") 2012-02-10 10:53

![]()

### 16 楼 [wyn4595735](http://wyn4595735.iteye.com/ "wyn4595735") 2012-02-06 17:03

写的真不错啊，赞
### 15 楼 [hemijing](http://hemijing.iteye.com/ "hemijing") 2011-10-22 15:33

灰常之好呀

### 14 楼 [yacht](http://noam.iteye.com/ "yacht") 2011-10-12 15:54

这个文写的太好了！![]()
### 13 楼 [qpshenggui](http://qpshenggui.iteye.com/ "qpshenggui") 2011-08-29 13:39

![]()

### 12 楼 [wubo.wb](http://tiangu.iteye.com/ "wubo.wb") 2011-08-22 13:48

很通俗易懂，让我知道还有这样一个拷贝过程，谢谢![]() ![]()
### 11 楼 [txin0814](http://txin0814.iteye.com/ "txin0814") 2011-06-23 12:41

![]()

### 10 楼 [blueheart2008](http://blueheart2008.iteye.com/ "blueheart2008") 2011-06-04 22:53

![]()
太牛了。
### 9 楼 [yangke5105](http://yangke5105.iteye.com/ "yangke5105") 2011-06-01 11:24

已收藏，3ks……

### 8 楼 [sd6733531](http://sd6733531.iteye.com/ "sd6733531") 2011-05-19 10:57

的确是好文。但是本人目前还是不能完全理解和消化,楼主有没有好书推荐下
### 7 楼 [艾青草](http://lovegrass91-yahoo-com.iteye.com/ "艾青草") 2011-04-18 11:49

拜你为师吧！

### 6 楼 [jyb2218](http://jyb2218.iteye.com/ "jyb2218") 2011-04-08 14:23

写的好啊。。。。
### 5 楼 [xbcxs](http://xbcxs.iteye.com/ "xbcxs") 2011-04-02 11:29

好文章~~~

### 4 楼 [zhao0p](http://zhao0p.iteye.com/ "zhao0p") 2011-03-15 16:23

通俗易懂~
### 3 楼 [xiaoying_honey](http://xiaoying-honey.iteye.com/ "xiaoying_honey") 2011-03-15 10:59

![]() 谢谢，受益匪浅。

### 2 楼 [lj杰](http://ljstring19851014-126-com.iteye.com/ "lj杰") 2011-01-19 16:56

很厉害，很受益额，谢谢了
### 1 楼 [star65225692](http://star65225692.iteye.com/ "star65225692") 2010-11-22 12:00

好文~~~~~~~~~~[Java读出excel文件的类](http://www.itstrike.cn/Code/095c4615-263b-45b5-a778-67ebf1aec426)
### 发表评论

[![]() 您还没有登录,请您登录后再发表评论](http://hllvm.group.iteye.com/login)

[![New-page]()](http://hllvm.group.iteye.com/group/wiki/new)

### 文章信息

[知识库: 高级语言虚拟机](http://hllvm.group.iteye.com/group/wiki/)

* 由[fantasy](http://kiral.iteye.com/ "fantasy")在2010-11-12创建
* 由[fantasy](http://kiral.iteye.com/ "fantasy")在2011-05-26更新
### 相关新闻

* [Scalaris，为Web 2.0交易服务存储数据](http://hllvm.group.iteye.com/news/4307-scalaris-as-web-2-0-services-trade-data)
* [基于Spindle的增强HTTP Spider](http://hllvm.group.iteye.com/news/1731)
* [推荐风轻扬：Java 6中的性能优化](http://hllvm.group.iteye.com/news/2823)

### 相关讨论

* [java线程安全总结](http://hllvm.group.iteye.com/topic/806990)
* [线程同步](http://hllvm.group.iteye.com/topic/164905)
* [多核线程笔记-volatile原理与技巧](http://hllvm.group.iteye.com/topic/109150)
* [自己实现的java lock](http://hllvm.group.iteye.com/topic/1068877)
* [最简单高效的tryLock](http://hllvm.group.iteye.com/topic/770080)
### 相关博客

* [java线程安全总结](http://jiangtie.iteye.com/blog/1112625)
* [jvm内存模型](http://handawei.iteye.com/blog/808485)
* [java线程同步](http://republicw.iteye.com/blog/1218723)
* [java线程同步](http://qpshenggui.iteye.com/blog/1160343)
* [java线程安全总结](http://soboer.iteye.com/blog/1312934)
* [首页](http://www.iteye.com/)
* [资讯](http://www.iteye.com/news)
* [精华](http://www.iteye.com/magazines)
* [论坛](http://www.iteye.com/forums)
* [问答](http://www.iteye.com/ask)
* [博客](http://www.iteye.com/blogs)
* [专栏](http://www.iteye.com/blogs/subjects)
* [群组](http://www.iteye.com/groups)
* [招聘](http://job.iteye.com/iteye)
* [搜索](http://www.iteye.com/search)
* [广告服务](http://hllvm.group.iteye.com/index/service)
* [ITeye黑板报](http://webmaster.iteye.com/)
* [联系我们](http://hllvm.group.iteye.com/index/contactus)
* [友情链接](http://hllvm.group.iteye.com/index/friend_links)

© 2003-2012 ITeye.com. [ [京ICP证110151号](http://www.miibeian.gov.cn/) 京公网安备110105010620 ]
百联优力(北京)投资有限公司 版权所有 ![](http://stat.iteye.com/?url=http%3A%2F%2Fhllvm.group.iteye.com%2Fgroup%2Fwiki%2F2877-synchronized-volatile&referrer=&user_id=)
