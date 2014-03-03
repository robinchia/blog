---
layout: post
title: "淘宝面试题：如何充分利用多核CPU，计算很大的List中所有整数的和 - Java综合 - Java"
categories: 面试参考
tags: 
 - 面试参考
 - 题目
--- 

# 淘宝面试题：如何充分利用多核CPU，计算很大的List中所有整数的和 - Java综合 - Java - JavaEye论坛

[您还未登录 !](http://www.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://www.javaeye.com/login) [注册](http://www.javaeye.com/signup)

[![JavaEye-最棒的软件开发交流社区]( "JavaEye-最棒的软件开发交流社区")](http://www.javaeye.com/)

[论坛首页](http://www.javaeye.com/forums) → [Java编程和Java企业应用版](http://www.javaeye.com/forums/board/Java) → [Java综合](http://www.javaeye.com/forums/tag/Java) →

# 淘宝面试题：如何充分利用多核CPU，计算很大的List中所有整数的和

[全部](http://www.javaeye.com/forums/board/Java) [Hibernate](http://www.javaeye.com/forums/tag/Hibernate) [Spring](http://www.javaeye.com/forums/tag/Spring) [Struts](http://www.javaeye.com/forums/tag/Struts) [iBATIS](http://www.javaeye.com/forums/tag/iBATIS) [企业应用](http://www.javaeye.com/forums/tag/J2EE) [设计模式](http://www.javaeye.com/forums/tag/Design-Pattern) [DAO](http://www.javaeye.com/forums/tag/DAO) [领域模型](http://www.javaeye.com/forums/tag/Object-Domain) [OO](http://www.javaeye.com/forums/tag/OO) [Tomcat](http://www.javaeye.com/forums/tag/Tomcat) [SOA](http://www.javaeye.com/forums/tag/SOA) [JBoss](http://www.javaeye.com/forums/tag/JBoss) [Swing](http://www.javaeye.com/forums/tag/Swing) [Java综合](http://www.javaeye.com/forums/tag/Java)
[](http://www.javaeye.com/forums/39/topics/711162/posts/new "发表回复")

[myApps快速开发平台，配置即开发、所见即所得，节约85%工作量](http://www.javaeye.com/clicks/324)

« 上一页 1 [2](http://www.javaeye.com/topic/711162?page=2) [3](http://www.javaeye.com/topic/711162?page=3) … [15](http://www.javaeye.com/topic/711162?page=15) [16](http://www.javaeye.com/topic/711162?page=16) [下一页 »](http://www.javaeye.com/topic/711162?page=2)
浏览 24475 次
 [主题：淘宝面试题：如何充分利用多核CPU，计算很大的List中所有整数的和](http://www.javaeye.com/topic/711162)

**该帖已经被评为良好帖** 作者 正文 * 飞雪无情
* 等级: ![一星会员]( "一星会员")
* [![飞雪无情的博客]( "飞雪无情的博客: 简简单单")](http://flysnow.javaeye.com/)
* 文章: 50
* 积分: 110
* 来自: 河南
* ![]()
    发表时间：2010-07-12   最后修改：2010-07-14

[<](http://www.javaeye.com/topic/711162#) [>](http://www.javaeye.com/topic/711162#) 猎头职位:

相关文章: [ ](http://www.javaeye.com/topic/711162# "关闭")

* [java并发编程-Executor框架](http://www.javaeye.com/topic/366591 "java并发编程-Executor框架")
* [更好的把握线程<一>:Thread (线程)介绍](http://www.javaeye.com/topic/202329 "更好的把握线程<一>:Thread (线程)介绍")
* [Fork/Join点点滴滴之一CyclicAction](http://www.javaeye.com/topic/406000 "Fork/Join点点滴滴之一CyclicAction")
推荐圈子: [lucene爱好者](http://lucene-group.group.javaeye.com/)
[更多相关推荐](http://www.javaeye.com/wiki/topic/711162)
**永久链接：[http://flysnow.javaeye.com/blog/711162](http://flysnow.javaeye.com/blog/711162)**
引用

前几天在网上看到一个淘宝的面试题：有一个很大的整数list，需要求这个list中所有整数的和，写一个可以充分利用多核CPU的代码，来计算结果。
**一：分析题目**
从题中可以看到“很大的List”以及“充分利用多核CPU”，这就已经充分告诉我们要采用多线程(任务)进行编写。具体怎么做呢？大概的思路就是分割List，每一小块的List采用一个线程(任务)进行计算其和，最后等待所有的线程(任务)都执行完后就可得到这个“很大的List”中所有整数的和。
**二：具体分析和技术方案**
既然我们已经决定采用多线程(任务)，并且还要分割List，每一小块的List采用一个线程(任务)进行计算其和，那么我们必须要等待所有的线程(任务)完成之后才能得到正确的结果，那么怎么才能保证“等待所有的线程(任务)完成之后输出结果呢”？这就要靠java.util.concurrent包中的CyclicBarrier类了。它是一个同步辅助类，它允许一组线程(任务)互相等待，直到到达某个公共屏障点 (common barrier point)。在涉及一组固定大小的线程(任务)的程序中，这些线程(任务)必须不时地互相等待，此时 CyclicBarrier 很有用。简单的概括其适应场景就是：当一组线程(任务)并发的执行一件工作的时候，必须等待所有的线程(任务)都完成时才能进行下一个步骤。具体技术方案步骤如下：

* 分割List，根据采用的线程(任务)数平均分配，即list.size()/threadCounts。
* 定义一个记录“很大List”中所有整数和的变量sum，采用一个线程(任务)处理一个分割后的子List，计算子List中所有整数和(subSum)，然后把和(subSum)累加到sum上。
* 等待所有线程(任务)完成后输出总和(sum)的值。
示意图如下：
![]()
**三：详细编码实现**
代码中有很详细的注释，这里就不解释了。
/** * 计算List中所有整数的和<br> * 采用多线程，分割List计算 * @author 飞雪无情 * @since 2010-7-12 */ public class CountListIntegerSum { private long sum;//存放整数的和 private CyclicBarrier barrier;//障栅集合点(同步器) private List<Integer> list;//整数集合List private int threadCounts;//使用的线程数 public CountListIntegerSum(List<Integer> list,int threadCounts) { this.list=list; this.threadCounts=threadCounts; } /** * 获取List中所有整数的和 * @return */ public long getIntegerSum(){ ExecutorService exec=Executors.newFixedThreadPool(threadCounts); int len=list.size()/threadCounts;//平均分割List //List中的数量没有线程数多（很少存在） if(len==0){ threadCounts=list.size();//采用一个线程处理List中的一个元素 len=list.size()/threadCounts;//重新平均分割List } barrier=new CyclicBarrier(threadCounts+1); for(int i=0;i<threadCounts;i++){ //创建线程任务 if(i==threadCounts-1){//最后一个线程承担剩下的所有元素的计算 exec.execute(new SubIntegerSumTask(list.subList(i*len,list.size()))); }else{ exec.execute(new SubIntegerSumTask(list.subList(i*len, len*(i+1)>list.size()?list.size():len*(i+1)))); } } try { barrier.await();//关键，使该线程在障栅处等待，直到所有的线程都到达障栅处 } catch (InterruptedException e) { System.out.println(Thread.currentThread().getName()+":Interrupted"); } catch (BrokenBarrierException e) { System.out.println(Thread.currentThread().getName()+":BrokenBarrier"); } exec.shutdown(); return sum; } /** * 分割计算List整数和的线程任务 * @author lishuai * */ public class SubIntegerSumTask implements Runnable{ private List<Integer> subList; public SubIntegerSumTask(List<Integer> subList) { this.subList=subList; } public void run() { long subSum=0L; for (Integer i : subList) { subSum += i; } synchronized(CountListIntegerSum.this){//在CountListIntegerSum对象上同步 sum+=subSum; } try { barrier.await();//关键，使该线程在障栅处等待，直到所有的线程都到达障栅处 } catch (InterruptedException e) { System.out.println(Thread.currentThread().getName()+":Interrupted"); } catch (BrokenBarrierException e) { System.out.println(Thread.currentThread().getName()+":BrokenBarrier"); } System.out.println("分配给线程："+Thread.currentThread().getName()+"那一部分List的整数和为：\tSubSum:"+subSum); } } }
有人可能对barrier=new CyclicBarrier(threadCounts+1);//创建的线程数和主线程main有点不解，不是采用的线程(任务)数是threadCounts个吗？怎么为CyclicBarrier设置的给定数量的线程参与者比我们要采用的线程数多一个呢？答案就是这个多出来的一个用于控制main主线程的，主线程也要等待，它要等待其他所有的线程完成才能输出sum值，这样才能保证sum值的正确性，如果main不等待的话，那么结果将是不可预料的。
/** * 计算List中所有整数的和测试类 * @author 飞雪无情 * @since 2010-7-12 */ public class CountListIntegerSumMain { /** * @param args */ public static void main(String[] args) { List<Integer> list = new ArrayList<Integer>(); int threadCounts = 10;//采用的线程数 //生成的List数据 for (int i = 1; i <= 1000000; i++) { list.add(i); } CountListIntegerSum countListIntegerSum=new CountListIntegerSum(list,threadCounts); long sum=countListIntegerSum.getIntegerSum(); System.out.println("List中所有整数的和为:"+sum); } }
**四：总结**
本文主要通过一个淘宝的面试题为引子，介绍了并发的一点小知识，主要是介绍通过CyclicBarrier同步辅助器辅助多个并发任务共同完成一件工作。Java SE5的java.util.concurrent引入了大量的设计来解决并发问题，使用它们有助于我们编写更加简单而健壮的并发程序。
**附mathfox提到的ExecutorService.invokeAll()方法的实现**
这个不用自己控制等待，invokeAll执行给定的任务，当所有任务完成时，返回保持任务状态和结果的 Future 列表。sdh5724也说用了同步，性能不好。这个去掉了同步，根据返回结果的 Future 列表相加就得到总和了。/** * 使用ExecutorService的invokeAll方法计算 * @author 飞雪无情 * */ public class CountSumWithCallable { /** * @param args * @throws InterruptedException * @throws ExecutionException */ public static void main(String[] args) throws InterruptedException, ExecutionException { int threadCounts =19;//使用的线程数 long sum=0; ExecutorService exec=Executors.newFixedThreadPool(threadCounts); List<Callable<Long>> callList=new ArrayList<Callable<Long>>(); //生成很大的List List<Integer> list = new ArrayList<Integer>(); for (int i = 0; i <= 1000000; i++) { list.add(i); } int len=list.size()/threadCounts;//平均分割List //List中的数量没有线程数多（很少存在） if(len==0){ threadCounts=list.size();//采用一个线程处理List中的一个元素 len=list.size()/threadCounts;//重新平均分割List } for(int i=0;i<threadCounts;i++){ final List<Integer> subList; if(i==threadCounts-1){ subList=list.subList(i*len,list.size()); }else{ subList=list.subList(i*len, len*(i+1)>list.size()?list.size():len*(i+1)); } //采用匿名内部类实现 callList.add(new Callable<Long>(){ public Long call() throws Exception { long subSum=0L; for(Integer i:subList){ subSum+=i; } System.out.println("分配给线程："+Thread.currentThread().getName()+"那一部分List的整数和为：\tSubSum:"+subSum); return subSum; } }); } List<Future<Long>> futureList=exec.invokeAll(callList); for(Future<Long> future:futureList){ sum+=future.get(); } exec.shutdown(); System.out.println(sum); } }
**一些感言**
这篇文章是昨天夜里11点多写好的，我当时是在网上看到了这个题目，就做了一下分析，写了实现代码，由于水平有限，难免有bug，这里感谢xifo等人的指正。这些帖子从发表到现在不到24小时的时间里创造了近9000的浏览次数，回复近100，这是我没有想到的，javaeye很久没这么疯狂过啦。这不是因为我的算法多好，而是因为这个题目、这篇帖子所体现出的意义。大家在看完这篇帖子后不光指正错误，还对方案进行了改进，关键是思考，人的思维是无穷的，只要我们善于发掘，善于思考，总能想出一些意想不到的方案。
从算法看，或者从题目场景对比代码实现来看，或许不是一篇很好的帖子，但是我说这篇帖子是很有意义的，方案也是在很多场景适用，有时我们可以假设这不是计算和，而是把数据写到一个个的小文件里，或者是分割进行网络传输等等，都有一定的启发，特别是回帖中的讨论。
单说一下回帖，我建议进来的人尽量看完所有的回帖，因为这里是很多人集思广益的精华，这里有他们分析问题，解决问题的思路，还有每个人提到的解决方案，想想为什么能用?为什么不能用?为什么好?为什么不好?
**我一直相信：讨论是解决问题、提高水平的最佳方式！**
* [![]( "点击查看原始大小图片")]()
* 大小: 95.8 KB

* [查看图片附件](http://www.javaeye.com/topic/711162#)

声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。
推荐链接

* [![adobe]( "adobe")](http://www.javaeye.com/clicks/373) [返回顶楼](http://www.javaeye.com/topic/711162#) [ ](http://flysnow.javaeye.com/ "浏览作者的博客") [ ](http://flysnow.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E9%A3%9E%E9%9B%AA%E6%97%A0%E6%83%85 "发送站内短信") [ ](http://flysnow.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E9%A3%9E%E9%9B%AA%E6%97%A0%E6%83%85 "关注作者")  * coffeefeel
* 等级: 初级会员
* [![coffeefeel的博客]( "coffeefeel的博客: ")](http://coffeefeel.javaeye.com/)
* 文章: 1
* 积分: 30
* 来自: 北京
* ![]()
    发表时间：2010-07-13  

飞雪无情 写道

synchronized(CountListIntegerSum.this){//在CountListIntegerSum对象上同步
sum+=subSum;
}
实际效果还是顺序累加的？ [返回顶楼](http://www.javaeye.com/topic/711162#) [ ](http://coffeefeel.javaeye.com/ "浏览作者的博客") [ ](http://coffeefeel.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=coffeefeel "发送站内短信") [ ](http://coffeefeel.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=coffeefeel "关注作者") [回帖地址](http://www.javaeye.com/topic/711162#1576458 "coffeefeel回帖:淘宝面试题：如何充分利用多核CPU，计算很大的List中所有整数的和")

[0](http://www.javaeye.com/topic/711162# "好") [0](http://www.javaeye.com/topic/711162# "差") 请登录后投票  * 飞雪无情
* 等级: ![一星会员]( "一星会员")
* [![飞雪无情的博客]( "飞雪无情的博客: 简简单单")](http://flysnow.javaeye.com/)
* 文章: 50
* 积分: 110
* 来自: 河南
* ![]()
    发表时间：2010-07-13  

coffeefeel 写道

飞雪无情 写道

synchronized(CountListIntegerSum.this){//在CountListIntegerSum对象上同步
sum+=subSum;
}
实际效果还是顺序累加的？
这个List不管分多少段，最终总要合并到一起。因为这个累加是每个线程(任务)执行的，所以累加不是顺序的，哪个线程（任务）先完成计算分割List部分的和，哪个线程（任务）就先进行累加。 [返回顶楼](http://www.javaeye.com/topic/711162#) [ ](http://flysnow.javaeye.com/ "浏览作者的博客") [ ](http://flysnow.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E9%A3%9E%E9%9B%AA%E6%97%A0%E6%83%85 "发送站内短信") [ ](http://flysnow.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E9%A3%9E%E9%9B%AA%E6%97%A0%E6%83%85 "关注作者") [回帖地址](http://www.javaeye.com/topic/711162#1576506 "飞雪无情回帖:淘宝面试题：如何充分利用多核CPU，计算很大的List中所有整数的和")

[0](http://www.javaeye.com/topic/711162# "好") [0](http://www.javaeye.com/topic/711162# "差") 请登录后投票  * p2bl
* 等级: 初级会员
* [![p2bl的博客]( "p2bl的博客: ")](http://p2bl.javaeye.com/)
* 文章: 19
* 积分: 40
* 来自: 南京
* ![]()
    发表时间：2010-07-13  

有点一个jvm里的mapreduce的意思 [返回顶楼](http://www.javaeye.com/topic/711162#) [ ](http://p2bl.javaeye.com/ "浏览作者的博客") [ ](http://p2bl.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=p2bl "发送站内短信") [ ](http://p2bl.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=p2bl "关注作者") [回帖地址](http://www.javaeye.com/topic/711162#1576571 "p2bl回帖:淘宝面试题：如何充分利用多核CPU，计算很大的List中所有整数的和")

[1](http://www.javaeye.com/topic/711162# "好") [0](http://www.javaeye.com/topic/711162# "差") 请登录后投票  * 飞雪无情
* 等级: ![一星会员]( "一星会员")
* [![飞雪无情的博客]( "飞雪无情的博客: 简简单单")](http://flysnow.javaeye.com/)
* 文章: 50
* 积分: 110
* 来自: 河南
* ![]()
    发表时间：2010-07-13  

p2bl 写道

有点一个jvm里的mapreduce的意思
呵呵。。这个和那个差的远，即使有点那么个意思也是冰山一角。呵呵。 [返回顶楼](http://www.javaeye.com/topic/711162#) [ ](http://flysnow.javaeye.com/ "浏览作者的博客") [ ](http://flysnow.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E9%A3%9E%E9%9B%AA%E6%97%A0%E6%83%85 "发送站内短信") [ ](http://flysnow.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E9%A3%9E%E9%9B%AA%E6%97%A0%E6%83%85 "关注作者") [回帖地址](http://www.javaeye.com/topic/711162#1576599 "飞雪无情回帖:淘宝面试题：如何充分利用多核CPU，计算很大的List中所有整数的和")

[0](http://www.javaeye.com/topic/711162# "好") [0](http://www.javaeye.com/topic/711162# "差") 请登录后投票  * hrsvici412
* 等级: 初级会员
* [![hrsvici412的博客]( "hrsvici412的博客: 乐在其中")](http://hrsvici412.javaeye.com/)
* 文章: 32
* 积分: 30
* 来自: 上海
* ![]()
    发表时间：2010-07-13  

学到好多线程的东西，请问 你的分析的图，是用什么工具画的！ [返回顶楼](http://www.javaeye.com/topic/711162#) [ ](http://hrsvici412.javaeye.com/ "浏览作者的博客") [ ](http://hrsvici412.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=hrsvici412 "发送站内短信") [ ](http://hrsvici412.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=hrsvici412 "关注作者") [回帖地址](http://www.javaeye.com/topic/711162#1576749 "hrsvici412回帖:淘宝面试题：如何充分利用多核CPU，计算很大的List中所有整数的和")

[0](http://www.javaeye.com/topic/711162# "好") [0](http://www.javaeye.com/topic/711162# "差") 请登录后投票  * mathfox
* 等级: 初级会员
* [![mathfox的博客]( "mathfox的博客: mathfox")](http://mathfox.javaeye.com/)
* 文章: 208
* 积分: 40
* 来自: 北京
* ![]()
    发表时间：2010-07-13  

引用

本文主要通过一个淘宝的面试题为引子，介绍了并发的一点小知识，主要是介绍通过CyclicBarrier同步辅助器辅助多个并发任务共同完成一件工作。Java SE5的java.util.concurrent引入了大量的设计来解决并发问题，使用它们有助于我们编写更加简单而健壮的并发程序。
不用这么麻烦吧
用ExecutorService.invokeAll就行了吧。 [返回顶楼](http://www.javaeye.com/topic/711162#) [ ](http://mathfox.javaeye.com/ "浏览作者的博客") [ ](http://mathfox.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=mathfox "发送站内短信") [ ](http://mathfox.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=mathfox "关注作者") [回帖地址](http://www.javaeye.com/topic/711162#1576760 "mathfox回帖:淘宝面试题：如何充分利用多核CPU，计算很大的List中所有整数的和")

[1](http://www.javaeye.com/topic/711162# "好") [0](http://www.javaeye.com/topic/711162# "差") 请登录后投票  * numen_wlm
* 等级: 初级会员
* [![numen_wlm的博客]( "numen_wlm的博客: numen_wlm")](http://numen.javaeye.com/)
* 文章: 48
* 积分: 12
* 来自: 上海
* ![]()
    发表时间：2010-07-13  

有时间研究下CyclicBarrier里面是怎么处理的。 [返回顶楼](http://www.javaeye.com/topic/711162#) [ ](http://numen.javaeye.com/ "浏览作者的博客") [ ](http://numen.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=numen_wlm "发送站内短信") [ ](http://numen.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=numen_wlm "关注作者") [回帖地址](http://www.javaeye.com/topic/711162#1576763 "numen_wlm回帖:淘宝面试题：如何充分利用多核CPU，计算很大的List中所有整数的和")

[0](http://www.javaeye.com/topic/711162# "好") [0](http://www.javaeye.com/topic/711162# "差") 请登录后投票  * xifo
* 等级: 初级会员
* [![xifo的博客]( "xifo的博客: xifo")](http://xifo.javaeye.com/)
* 文章: 20
* 积分: 43
* 来自: 安徽
* ![]()
    发表时间：2010-07-13   最后修改：2010-07-13

思路是对的，可惜算法有问题，大多数情况下计算结果不正确。
不信的话，试试把这一行
for (int i = 1; i <= 1000000; i++)
换成
for (int i = 0; i <= 1000000; i++)
如果计算正确，两个计算结果应该一样，实际上计算结果会少一百万，原因是分割那里没有考虑尾数。 [返回顶楼](http://www.javaeye.com/topic/711162#) [ ](http://xifo.javaeye.com/ "浏览作者的博客") [ ](http://xifo.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=xifo "发送站内短信") [ ](http://xifo.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=xifo "关注作者") [回帖地址](http://www.javaeye.com/topic/711162#1576785 "xifo回帖:淘宝面试题：如何充分利用多核CPU，计算很大的List中所有整数的和")

[1](http://www.javaeye.com/topic/711162# "好") [0](http://www.javaeye.com/topic/711162# "差") 请登录后投票  * 飞雪无情
* 等级: ![一星会员]( "一星会员")
* [![飞雪无情的博客]( "飞雪无情的博客: 简简单单")](http://flysnow.javaeye.com/)
* 文章: 50
* 积分: 110
* 来自: 河南
* ![]()
    发表时间：2010-07-13  

hrsvici412 写道

学到好多线程的东西，请问 你的分析的图，是用什么工具画的！
用的是Edraw [返回顶楼](http://www.javaeye.com/topic/711162#) [ ](http://flysnow.javaeye.com/ "浏览作者的博客") [ ](http://flysnow.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=%E9%A3%9E%E9%9B%AA%E6%97%A0%E6%83%85 "发送站内短信") [ ](http://flysnow.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=%E9%A3%9E%E9%9B%AA%E6%97%A0%E6%83%85 "关注作者") [回帖地址](http://www.javaeye.com/topic/711162#1576813 "飞雪无情回帖:淘宝面试题：如何充分利用多核CPU，计算很大的List中所有整数的和")

[0](http://www.javaeye.com/topic/711162# "好") [0](http://www.javaeye.com/topic/711162# "差") 请登录后投票

[](http://www.javaeye.com/forums/39/topics/711162/posts/new "发表回复")

« 上一页 1 [2](http://www.javaeye.com/topic/711162?page=2) [3](http://www.javaeye.com/topic/711162?page=3) … [15](http://www.javaeye.com/topic/711162?page=15) [16](http://www.javaeye.com/topic/711162?page=16) [下一页 »](http://www.javaeye.com/topic/711162?page=2)
[论坛首页](http://www.javaeye.com/forums) → [Java编程和Java企业应用版](http://www.javaeye.com/forums/board/Java) → [Java综合](http://www.javaeye.com/forums/tag/Java)
跳转论坛:Java编程和Java企业应用 Web前端技术 移动编程和手机应用开发 C/C++编程 Ruby编程 Python编程 PHP编程 Flash编程和RIA Microsoft .Net 综合技术 软件开发和项目管理 行业应用 入门讨论 招聘求职 海阔天空

* [北京: Laszlo Systems 诚聘高级Java程序员](http://www.javaeye.com/jobs/817 "北京:Laszlo Systems 诚聘高级Java程序员")
* [浙江: 网易（NETEASE）诚聘java开发工程师](http://www.javaeye.com/jobs/823 "浙江:网易（NETEASE）诚聘java开发工程师")
* [上海: 上海汉得诚聘高级基础架构研发工程师——IDE](http://www.javaeye.com/jobs/825 "上海:上海汉得诚聘高级基础架构研发工程师——IDE方向")
* [上海: BShare诚聘资深Java Web开发工程师](http://www.javaeye.com/jobs/838 "上海:BShare诚聘资深Java Web开发工程师")
* [北京: chinacache诚聘Java应用架构师](http://www.javaeye.com/jobs/728 "北京:chinacache诚聘Java应用架构师")
* [广东: 穆迪moody's诚聘Java Developer](http://www.javaeye.com/jobs/794 "广东:穆迪moody's诚聘Java Developer")
* [北京: 网易（NETEASE）诚聘高级Java开发工程师](http://www.javaeye.com/jobs/800 "北京:网易（NETEASE）诚聘高级Java开发工程师")
* [福建: 福州几维网络诚聘游戏服务端程序员(Java)](http://www.javaeye.com/jobs/804 "福建:福州几维网络诚聘游戏服务端程序员(Java)")
* [北京: 掌中魔力诚聘java资深工程师](http://www.javaeye.com/jobs/769 "北京:掌中魔力诚聘java资深工程师")
* [北京: chinacache诚聘高级Java开发工程师（后台应用](http://www.javaeye.com/jobs/726 "北京:chinacache诚聘高级Java开发工程师（后台应用）")
* [北京: JavaEye猎头诚聘Java搜索工程师](http://www.javaeye.com/jobs/712 "北京:JavaEye猎头诚聘Java搜索工程师")
* [北京: 去哪儿旅游搜索引擎诚聘java开发工程师](http://www.javaeye.com/jobs/523 "北京:去哪儿旅游搜索引擎诚聘java开发工程师")
* [上海: 恺英网络诚聘JS工程师](http://www.javaeye.com/jobs/682 "上海:恺英网络诚聘JS工程师")
* [北京: 上海幽幽诚聘手机网游服务器端开发工程师](http://www.javaeye.com/jobs/806 "北京:上海幽幽诚聘手机网游服务器端开发工程师")
* [浙江: VMKID诚聘Java开发工程师(J2SE)（Client端方](http://www.javaeye.com/jobs/779 "浙江:VMKID诚聘Java开发工程师(J2SE)（Client端方向）")
* [北京: glodon诚聘Eclipse插件开发工程师](http://www.javaeye.com/jobs/839 "北京:glodon诚聘Eclipse插件开发工程师")

* [首页](http://www.javaeye.com/)
* [新闻](http://www.javaeye.com/news)
* [论坛](http://www.javaeye.com/forums)
* [问答](http://www.javaeye.com/ask)
* [专栏](http://www.javaeye.com/wiki)
* [博客](http://www.javaeye.com/blogs)
* [圈子](http://www.javaeye.com/groups)
* [招聘](http://www.javaeye.com/job)
* [服务](http://www.javaeye.com/index/service)
* [搜索](http://www.javaeye.com/search)
* [Java](http://java.javaeye.com/)
* [Web](http://webapp.javaeye.com/)
* [Ruby](http://ruby.javaeye.com/)
* [Python](http://python.javaeye.com/)
* [敏捷](http://agile.javaeye.com/)
* [MySQL](http://mysql.javaeye.com/)
* [润亁报表](http://runqian.javaeye.com/)
* [图书](http://book.javaeye.com/)
* [广告服务](http://www.javaeye.com/index/service)
* [JavaEye黑板报](http://webmaster.javaeye.com/)
* [关于我们](http://www.javaeye.com/index/aboutus)
* [联系我们](http://www.javaeye.com/index/contactus)
* [友情链接](http://www.javaeye.com/index/friend_links)

© 2003-2010 JavaEye.com. 上海炯耐计算机软件有限公司版权所有 [ [沪ICP备05023328号](http://www.miibeian.gov.cn/) ]
