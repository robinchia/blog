---
layout: post
title: "python安全管理子进程-subprocess"
categories: python
tags: 
 - python
--- 

# python安全管理子进程-subprocess

[您还未登录 !](http://www.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://www.javaeye.com/login) [注册](http://www.javaeye.com/signup)

[![JavaEye-最棒的软件开发交流社区]( "JavaEye-最棒的软件开发交流社区")](http://www.javaeye.com/)

![]()

[论坛首页](http://www.javaeye.com/forums) → [Python编程版](http://www.javaeye.com/forums/board/Python) → [python](http://www.javaeye.com/forums/tag/python) →

# python安全管理子进程-subprocess

[全部](http://www.javaeye.com/forums/board/Python) [python](http://www.javaeye.com/forums/tag/python) [django](http://www.javaeye.com/forums/tag/django) [GAE](http://www.javaeye.com/forums/tag/GAE)
浏览 4130 次 锁定老贴子 [主题：python安全管理子进程-subprocess](http://www.javaeye.com/topic/406623)

精华帖 (0) :: 良好帖 (0) :: 新手帖 (0) :: 隐藏帖 (0) 作者 正文 * pypy
* 等级: ![一星会员]( "一星会员")
* [![pypy的博客]( "pypy的博客: python mplayer ffmpeg vp6")](http://pypy.javaeye.com/)
* 性别: ![]( "男")
* 文章: 19
* 积分: 100
* 来自: 上海
* ![]()
    发表时间：2009-06-11   最后修改：2010-04-03

[<](http://www.javaeye.com/topic/406623#) [>](http://www.javaeye.com/topic/406623#) 猎头职位:

相关文章: [ ](http://www.javaeye.com/topic/406623# "关闭")

* [django,性能测试，以及对fastcgi下进程模型和线程模型的分析](http://www.javaeye.com/topic/267429 "django,性能测试，以及对fastcgi下进程模型和线程模型的分析")
* [用python写了一个跟踪路由的小东西](http://www.javaeye.com/topic/550804 "用python写了一个跟踪路由的小东西")
* [发个python2.6+wxPython+wxGlade实现的简单telnet客户端](http://www.javaeye.com/topic/451635 "发个python2.6+wxPython+wxGlade实现的简单telnet客户端")
推荐圈子: [Python](http://onlypython.group.javaeye.com/)
[更多相关推荐](http://www.javaeye.com/wiki/topic/406623)
经常会用到python去调用外部 工具或者命令去干活
有的时候子进程并不按预期退出
比如，子进程由于某种原因挂在那里，
这时候也许，我们有这样一种需求：需要父进程对子进程有监控动作，即，超过一定的时间，就不再等待子进程自己退出，而是去kill子进程，回收资源
以下会列出几张实现方法
1.os.system
[http://docs.python.org/library/os.html](http://docs.python.org/library/os.html)
Python 2.5.2 (r252:60911, Jan 4 2009, 17:40:26) [GCC 4.3.2] on linux2 Type "help", "copyright", "credits" or "license" for more information. >>> import os >>> os.system("date") Wed Jun 10 19:34:23 CST 2009 0 >>>
其实是执行 linux shell 命令
$ date Wed Jun 10 19:36:02 CST 2009
缺点：
A. os.system() 是新起一个shell去干活的，对系统的开销比较大
B. 获得输出等信息比较麻烦,不能与外部命令或工具交互
C. 无法控制，（如果调用的外部命令，挂死或者执行时间很长），主进程无法控制os.system(), 因为调用os.system(cmd) 调用进程会block， until os.system() 自己退出
2.commands
[url]
http://docs.python.org/library/commands.html[/url]
tommy@lab3:~$ python Python 2.5.2 (r252:60911, Jan 4 2009, 17:40:26) [GCC 4.3.2] on linux2 Type "help", "copyright", "credits" or "license" for more information. >>> import commands >>> dir(commands) ['__all__', '__builtins__', '__doc__', '__file__', '__name__', 'getoutput', 'getstatus', 'getstatusoutput', 'mk2arg', 'mkarg'] >>> commands.getoutput("date") 'Wed Jun 10 19:39:57 CST 2009' >>> >>> commands.getstatusoutput("date") (0, 'Wed Jun 10 19:40:41 CST 2009')
优点：
A. 容易获得外部命令的输出，已经退出状态
缺点：
同os.system()中的B,C
3.subprocess
[http://docs.python.org/library/subprocess.html](http://docs.python.org/library/subprocess.html)
tommy@lab3:~$ python Python 2.5.2 (r252:60911, Jan 4 2009, 17:40:26) [GCC 4.3.2] on linux2 Type "help", "copyright", "credits" or "license" for more information. >>> import subprocess >>> dir(subprocess) ['CalledProcessError', 'MAXFD', 'PIPE', 'Popen', 'STDOUT', '__all__', '__builtins__', '__doc__', '__file__', '__name__', '_active', '_cleanup', '_demo_posix', '_demo_windows', 'call', 'check_call', 'errno', 'fcntl', 'gc', 'list2cmdline', 'mswindows', 'os', 'pickle', 'select', 'signal', 'sys', 'traceback', 'types'] >>> Popen = subprocess.Popen(["date"]) Wed Jun 10 19:48:41 CST 2009 >>> Popen.pid 24723 >>>
优点：
看文档吧，可以支持和子进程交互等等
虽然 python2.6中的subprocess模块增加了
kill()
terminate()
来控制子进程退出
但是在实际的使用过程中会发现
如果子进程并不是自己退出，而是调用 kill()/terminate() 给子进程发信退出
通过 top 或者 ps -A 看到，子进程的确是释放资源了，但是却变成了 zombie（僵尸进程）
于是分析 subprocess.py模块
1201 1202 def send_signal(self, sig): 1203 """Send a signal to the process 1204 """ 1205 os.kill(self.pid, sig) 1206 1207 def terminate(self): 1208 """Terminate the process with SIGTERM 1209 """ 1210 self.send_signal(signal.SIGTERM) 1211 1212 def kill(self): 1213 """Kill the process with SIGKILL 1214 """ 1215 self.send_signal(signal.SIGKILL)
程序仅仅是 调用 os.kill(self.pid, sig) 向子进程发送了一个信号后，标准subprocess.py库 父进程并没有显示区 wait() 子进程，导致了 zombie（僵尸进程） 的生成
所以问题找到，
修改subprocess.py模块，显然不妥，
那就封装一下(继承subprocess)，
我是用这个subprocess去调用mencoder 做批量转码，所以为子进程超时，要有很好控制，
具体实现见附件
显示的封装成两个函数
1.
shell_2_tty(_cmd=cmds, _cwd=None, _timeout=10*60)
# _cmd 是要执行的外面命令行，要是一个 list， 如果是str，shell=True，会启动一个新的shell去干活的，这样，不利于进程的控制
# _cwd 是执行这个命令行前，cd到这个路径下面，这个，对我的用应很重要，如果不需要可以用默认值
# _timeout 这个是主角，设置超时时间（秒单位），从真重执行命令行开始计时，墙上时间超过 _timeout后，父进程会kill掉子进程，回收资源，并避免产生 zombie（僵尸进程）
# 并将调用的命令行输出，直接输出到stdout,即是屏幕的终端上，
（如果对输出比较讨厌，可以将 stdout = open("/dev/null", "w"), stderr=open("/dev/null"),等等）
2.
shell_2_tempfile(_cmd=cmds, _cwd=None, _timeout=10)
类同1，主要是增加，对命令行的输出，捕获，并返回给父进程，留作分析
* [td_shell.zip](http://dl.javaeye.com/topics/download/90954018-7a28-32d3-b4f6-edc17e5ebfaa) (1.8 KB)
* 下载次数: 101

声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。
推荐链接

* [返回顶楼](http://www.javaeye.com/topic/406623#) [ ](http://pypy.javaeye.com/ "浏览作者的博客") [ ](http://pypy.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=pypy "发送站内短信") [ ](http://pypy.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=pypy "关注作者")  * taupo
* 等级: 初级会员
* [![taupo的博客]( "taupo的博客: ")](http://taupo.javaeye.com/)
* 性别: ![]( "男")
* 文章: 554
* 积分: 10
* 来自: 重庆
* ![]()
    发表时间：2009-06-11  

开个新线程不行吗？
用进程是不是太那个了？ [返回顶楼](http://www.javaeye.com/topic/406623#) [ ](http://taupo.javaeye.com/ "浏览作者的博客") [ ](http://taupo.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=taupo "发送站内短信") [ ](http://taupo.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=taupo "关注作者") [回帖地址](http://www.javaeye.com/topic/406623#1050610 "taupo回帖:python安全管理子进程-subprocess")

[0](http://www.javaeye.com/topic/406623# "好") [0](http://www.javaeye.com/topic/406623# "差") 请登录后投票  * pypy
* 等级: ![一星会员]( "一星会员")
* [![pypy的博客]( "pypy的博客: python mplayer ffmpeg vp6")](http://pypy.javaeye.com/)
* 性别: ![]( "男")
* 文章: 19
* 积分: 100
* 来自: 上海
* ![]()
    发表时间：2009-06-11  

当然，用线程是可以实现这个需求的，
比如，需要调用外部命令的时候，就新起一个线程去干活，
主线程去监控子线程，如果发现子线程超时，就发信号给子线程退出，
实际的操作的时候，你会发现，线程很难退出
这个，可能是我线程用的不好的原因吧，
并且这个需求是要在 SMP 机器上干活，还是需要进程，可以被调度到其他CPU上干活，（GIL）限制 [返回顶楼](http://www.javaeye.com/topic/406623#) [ ](http://pypy.javaeye.com/ "浏览作者的博客") [ ](http://pypy.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=pypy "发送站内短信") [ ](http://pypy.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=pypy "关注作者") [回帖地址](http://www.javaeye.com/topic/406623#1050758 "pypy回帖:python安全管理子进程-subprocess")

[0](http://www.javaeye.com/topic/406623# "好") [0](http://www.javaeye.com/topic/406623# "差") 请登录后投票  * fanzy618
* 等级: 初级会员
* [![fanzy618的博客]( "fanzy618的博客: ")](http://fanzy618.javaeye.com/)
* 性别: ![]( "男")
* 文章: 19
* 积分: 40
* 来自: 北京
* ![]()
    发表时间：2009-06-25   最后修改：2009-06-25

执行terminate（）以后手动调用一下wait()不行吗？
如果使用2.6，有个一个新的库multiprocessing，非常强大，模仿threading的接口可以很容易的实现多进程的程序
from multiprocessing import Process def f(): print 'hello world!' if __name__ == '__main__': Process(target=f).start() [返回顶楼](http://www.javaeye.com/topic/406623#) [ ](http://fanzy618.javaeye.com/ "浏览作者的博客") [ ](http://fanzy618.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=fanzy618 "发送站内短信") [ ](http://fanzy618.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=fanzy618 "关注作者") [回帖地址](http://www.javaeye.com/topic/406623#1067751 "fanzy618回帖:python安全管理子进程-subprocess")

[0](http://www.javaeye.com/topic/406623# "好") [0](http://www.javaeye.com/topic/406623# "差") 请登录后投票  * pypy
* 等级: ![一星会员]( "一星会员")
* [![pypy的博客]( "pypy的博客: python mplayer ffmpeg vp6")](http://pypy.javaeye.com/)
* 性别: ![]( "男")
* 文章: 19
* 积分: 100
* 来自: 上海
* ![]()
    发表时间：2009-06-26   最后修改：2009-06-26

fanzy618 写道

执行terminate（）以后手动调用一下wait()不行吗？
如果使用2.6，有个一个新的库multiprocessing，非常强大，模仿threading的接口可以很容易的实现多进程的程序
from multiprocessing import Process def f(): print 'hello world!' if __name__ == '__main__': Process(target=f).start()
在附件里面，显示的写上了，wait()
def terminate(self): """Terminate the process with SIGTERM """ self.send_signal(signal.SIGTERM) def kill(self): """Kill the process with SIGKILL """ self.send_signal(signal.SIGKILL) def wait(self): """ wait child exit signal, """ self.Popen.wait() def free_child(self): """kill process by pid """ try: self.terminate() self.kill() self.wait() except: pass
兄弟提到的 multiprocessing 模块，的确很强大，
以后在程序中，小试下，
还有，现在很多服务器端的python版本还是比较低的,(centOS5.3)
基本上都是维持在
[tommy@lab4 log]$ python Python 2.4.3 (#1, Jan 21 2009, 01:11:33) [GCC 4.1.2 20071124 (Red Hat 4.1.2-42)] on linux2 Type "help", "copyright", "credits" or "license" for more information. >>>
所以，还需要自行安装一些模块的，比如（multiprocessing）等 [返回顶楼](http://www.javaeye.com/topic/406623#) [ ](http://pypy.javaeye.com/ "浏览作者的博客") [ ](http://pypy.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=pypy "发送站内短信") [ ](http://pypy.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=pypy "关注作者") [回帖地址](http://www.javaeye.com/topic/406623#1068781 "pypy回帖:python安全管理子进程-subprocess")

[0](http://www.javaeye.com/topic/406623# "好") [0](http://www.javaeye.com/topic/406623# "差") 请登录后投票  * fanzy618
* 等级: 初级会员
* [![fanzy618的博客]( "fanzy618的博客: ")](http://fanzy618.javaeye.com/)
* 性别: ![]( "男")
* 文章: 19
* 积分: 40
* 来自: 北京
* ![]()
    发表时间：2009-06-26  

multiprocessing有个2.4/2.5的逆移植版
http://code.google.com/p/python-multiprocessing/
不过在2.4版上还是Popen用的多一些。 [返回顶楼](http://www.javaeye.com/topic/406623#) [ ](http://fanzy618.javaeye.com/ "浏览作者的博客") [ ](http://fanzy618.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=fanzy618 "发送站内短信") [ ](http://fanzy618.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=fanzy618 "关注作者") [回帖地址](http://www.javaeye.com/topic/406623#1068890 "fanzy618回帖:python安全管理子进程-subprocess")

[0](http://www.javaeye.com/topic/406623# "好") [0](http://www.javaeye.com/topic/406623# "差") 请登录后投票  * mikeandmore
* 等级: 初级会员
* [![mikeandmore的博客]( "mikeandmore的博客: ")](http://mikeandmore.javaeye.com/)
* 性别: ![]( "男")
* 文章: 588
* 积分: 0
* 来自: 杭州
* ![]()
    发表时间：2009-06-26  

taupo 写道

开个新线程不行吗？
用进程是不是太那个了？
GIL [返回顶楼](http://www.javaeye.com/topic/406623#) [ ](http://mikeandmore.javaeye.com/ "浏览作者的博客") [ ](http://mikeandmore.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=mikeandmore "发送站内短信") [ ](http://mikeandmore.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=mikeandmore "关注作者") [回帖地址](http://www.javaeye.com/topic/406623#1069578 "mikeandmore回帖:python安全管理子进程-subprocess")

[0](http://www.javaeye.com/topic/406623# "好") [0](http://www.javaeye.com/topic/406623# "差") 请登录后投票  * swanky_yao
* 等级: 初级会员
* [![swanky_yao的博客]( "swanky_yao的博客: javaEye Fans")](http://swanky-yao.javaeye.com/)
* 性别: ![]( "男")
* 文章: 21
* 积分: 90
* 来自: 北京
* ![]()
    发表时间：2010-04-02  

好贴，不过补充一点：subprocess还有个好处就是会自动地加载系统环境变量。 [返回顶楼](http://www.javaeye.com/topic/406623#) [ ](http://swanky-yao.javaeye.com/ "浏览作者的博客") [ ](http://swanky-yao.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=swanky_yao "发送站内短信") [ ](http://swanky-yao.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=swanky_yao "关注作者") [回帖地址](http://www.javaeye.com/topic/406623#1433232 "swanky_yao回帖:python安全管理子进程-subprocess")

[0](http://www.javaeye.com/topic/406623# "好") [0](http://www.javaeye.com/topic/406623# "差") 请登录后投票  * pypy
* 等级: ![一星会员]( "一星会员")
* [![pypy的博客]( "pypy的博客: python mplayer ffmpeg vp6")](http://pypy.javaeye.com/)
* 性别: ![]( "男")
* 文章: 19
* 积分: 100
* 来自: 上海
* ![]()
    发表时间：2010-04-03   最后修改：2010-04-03

对的，自动加载父进程的环境变量，
环境变量&子进程执行的当前目录都是可以根据需要设置的
环境变量env自然就不用说了，有时候使用subprocess启动的子进程可能会要求在特定的目录下执行
这个时候就需要正确的设置cwd
[http://docs.python.org/library/subprocess.html](http://docs.python.org/library/subprocess.html)
class subprocess.Popen(args, bufsize=0, executable=None, stdin=None, stdout=None, stderr=None, preexec_fn=None, close_fds=False, shell=False, cwd=None, env=None, universal_newlines=False, startupinfo=None, creationflags=0) [返回顶楼](http://www.javaeye.com/topic/406623#) [ ](http://pypy.javaeye.com/ "浏览作者的博客") [ ](http://pypy.javaeye.com/blog/profile "浏览作者资料") [ ](http://app.javaeye.com/messages/new?message%5Breceiver_name%5D=pypy "发送站内短信") [ ](http://pypy.javaeye.com/blog/guest_book "给作者留言") [ ](http://app.javaeye.com/feed?subscription%5Bsubscribed_user_name%5D=pypy "关注作者") [回帖地址](http://www.javaeye.com/topic/406623#1435053 "pypy回帖:python安全管理子进程-subprocess")

[0](http://www.javaeye.com/topic/406623# "好") [0](http://www.javaeye.com/topic/406623# "差") 请登录后投票

[论坛首页](http://www.javaeye.com/forums) → [Python编程版](http://www.javaeye.com/forums/board/Python) → [python](http://www.javaeye.com/forums/tag/python)
跳转论坛:Java编程和Java企业应用 Web前端技术 移动编程和手机应用开发 C/C++编程 Ruby编程 Python编程 PHP编程 Flash编程和RIA Microsoft .Net 综合技术 软件开发和项目管理 行业应用 入门讨论 招聘求职 海阔天空

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
