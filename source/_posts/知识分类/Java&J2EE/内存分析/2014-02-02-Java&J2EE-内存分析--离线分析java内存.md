---
layout: post
title: "离线分析java内存"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 内存分析
--- 

# 离线分析java内存

 如题，我这里简单说下我现在离线分析java内存的方式，所谓离线，就是需要dump出正在运行的java系统中的一些运行时堆栈数据，然后拿到线下来分析，分析可以包括内存，线程，GC等等，同时不会对正在运行的生产环境的机器造成很大的影响，对应着离线分析，当然是在线分析了，这个我在后面会尝试下，因为离线分析有些场景还是模拟不出来，需要借助LR来模拟压力，查看在线的java程序运行情况了。

 

            首先一个简单的问题，如何dump出java运行时堆栈，这个SUN就提供了很好的工具，位于JAVA_HOME/bin目录下的jmap（java memory map之意），如果需要dump出当前运行的java进程的堆栈数据，则首先需要获得该java进程的进程ID,在linux下可以使用
Java代码  [![收藏代码]()]( "收藏这段代码")

1. ps -aux  
1.   
1. ps -ef | grep java  

 

或者使用jdk自带的一个工具jps，例如

 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. /JAVA_HOME/bin/jps  

 

找到了当前运行的java进程的id后，就可以对正在运行的java进程使用jmap工具进行dump了，例如使用以下命令：

 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. JAVA_HOME/bin/jmap  -dump:format=b,file=heap.bin <pid>   

 

其中file = heap.bin的意思是dump出的文件名叫heap.bin, 当然你可以选择你喜欢的名字，我这里选择叫*.bin是为了后面使用方便，<pid>表示你需要dump的java进程的id。

这里需要注意的是，记住dump的进程是java进程，不会是jboss的进程，weblogic的进程等。dump过程中机器load可能会升高，但是在我这里测试发现load升的不是特别快，同时dump时需要的磁盘空间也比较大，例如我这里测试的几个系统，分别是500M 800M 1500M  3000M，所以确保你运行jmap命令时所在的目录中的磁盘空间足够，当然现在的系统磁盘空间都比较大。

 

以上是在java进程还存活的时候进行的dump，有的时候我们的java进程crash后，会生成一个core.pid文件，这个core.pid文件还不能直接被我们的java 内存分析工具使用，需要将其转换为java 内存分析工具可以读的文件（例如使用jmap工具dump出的heap.bin文件就是很多java 内存分析工具可以读的文件格式）。将core.pid文件转换为jmap工具dump出的文件格式还可以继续使用jmap工具，这个的说明可以见我前几篇中的一个转载（[Create Java heapdumps with the help of core dumps](http://dikar.iteye.com/blog/643196)），这里我在补充点

 

 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. jmap -heap:format=b [java binary] [core dump file]  
1.   
1.   
1. 64位下可以指定使用64位模式  
1.   
1. jmap -d64  -dump:format=b [java binary] [core dump file]  

 

需要说明一下，使用jmap转换core.pid文件时，当文件格式比较大时，可能大于2G的时候就不能执行成功（我转换3G文件大小的时候没有成功）而报出

 

Error attaching to core file: Can't attach to the core file

 

查过sun的bug库中，这个bug还没有被修复，我想还是由于32位下用户进程寻址大小限制在2G的范围内引起的，在64位系统和64位jdk版本中，转换3G文件应该没有什么大的问题（有机会有环境得需要测试下）。如果有兴趣分析jmap转换不成功的同学，可以使用如下命令来分析跟踪命令的执行轨迹，例如使用

 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. strace  jmap -heap:format=b [java binary] [core dum  
1. p]  

 对于strace的命令的说明，同样可以参考我前几篇文章中的一个[ strace命令用法](http://dikar.iteye.com/blog/643201)

同时对于core.pid文件的调试我也补充一下, 其中>>表示命令提示符

 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. >>gdb JAVA_HOME/bin/java  core.pid  
1.   
1. >>bt  

 

bt后就可以看到生成core.pid文件时，系统正在执行的一个操作，例如是哪个so文件正在执行等。

 

好了说了这么多，上面都是怎么生成java 运行期DUMP文件的，接下来我们就进入分析阶段，为了分析这个dump出的文件，需要将这个文件弄到你的分析程序所在的机器上，例如可以是windows上，linux上，这个和你使用的分析工具以及使用的操作系统有关。不管使用什么系统，总是需要把生产环境下打出的dump文件搞到你的分析机器上，由于dump出的文件经常会比较大，例如达到2G，这么大的文件不是很好的从生产环境拉下来，因此使用FTP的方式把文件拖到分析机器上，同时由于单个文件很大，因此为了快速的将文件下载到分析机器，我们可以使用分而治之的思想，先将文件切割为小文件下载，然后在合并为一个大文件即可，还好linux提供了很方便的工具，例如使用如下命令

 
Java代码  [![收藏代码]()]( "收藏这段代码")

1. $ split -b 300m heap.bin  
1.   
1.  $ cat x* > heap.bin  

在上面的 split 命令行中的 “300m” 表示分割后的每个文件为 300MB，“heap.bin” 为待分割的dump文件，分割后的文件自动命名为 xaa，xab，xac等 
cat 命令可将这些分割后的文件合并为一个文件，例如将所有x开头的文件合并为heap.bin

 

如果我们是利用一个中间层的FTP服务器来保存数据的，那么我们还需要连接这个FTP服务器把合并后的文件拉下来，在windows下我推荐使用一个工具，速度很快而且简单，

winscp   http://winscp.net/eng/docs/lang:chs

好了分析的文件终于经过一翻周折到了你的分析机器上，现在我们就可以使用分析工具来分析这个dump出的程序了，这里我主要是分析内存的问题，所以我说下我选择的内存分析工具，我这里使用的是开源的由SAP 和IBM 支持的一个内存分析工具

# Memory Analyzer (MAT)

http://www.eclipse.org/mat/

 

我建议下载 Stand-alone Eclipse RCP 版本，不要装成eclipse的插件，因为这个分析起来还是很耗内存。****

 

下载好了，解压开来就可以直接使用了（基于eclipse的），打开以后，在菜单栏中选择打开文件，选择你刚刚的dump文件，然后一路的next就可以了，最后你会看到一个报告，这个报告里会告诉你可能的内存泄露的点，以及内存中对象的一个分布，关于mat的使用请参考官方说明，当然你也可以自己徜徉在学习的海洋中 。

 

对于dump文件的分析还可以使用jdk中提供的一个jhat工具来查看，不过这个很耗内存，而且默认的内存大小不够，还需要增加参数设置内存大小才能分析出，不过我看了下分析出的结果不是很满意，而且这个用起来很慢。还是推荐使用mat 。
来源： <[http://dikar.iteye.com/blog/643436](http://dikar.iteye.com/blog/643436)> 

 摘文：[Create Java heapdumps with the help of core dumps](http://dikar.iteye.com/blog/643196)

转载自：http://devops-abyss.blogspot.com/2010/03/create-java-heapdumps-with-help-of-core.html

 

Hi,
for some time I had the problem, that taking Java heap dumps with jmap took too long. When one of my tomcats crashed by an OutOfMemoryException, I had no time to do a heap dump because it took some hours and the server had to be back online.
Now I found a sollution to my problem. The initial idea came from [this](https://proxy.evlit.net/browse.php?u=http%3A%2F%2Fserverfault.com%2Fquestions%2F110153%2Fhow-to-reliably-take-java-heap-dumps%2F12545&b=4#125451) post. It had a solution for Solaris, but with some googling and try and error I found a solution for linux too.

1. create a core dump of your java process with gdb
gdb --pid=[java pid] 
gcore [file name] 
detach 
quit
1. restart the tomcat or do whatever you like with the java process
1. attach jmap to the core dump and create a Java heap dump
jmap -heap:format=b [java binary] [core dump file]
1. analyze your Java heap dump with your prefered tool

 When you get the following error in step three:
Error attaching to core file: Can't attach to the core file

This might help:
In my case the error apeared because I used the wrong java binary in the jmap call. When you are not sure about your java binary, open the core dump with gdb:

gdb --core=[core dump file]

You will get an output similar to this one:

GNU gdb 6.6 Copyright (C) 2006 Free Software Foundation, Inc. GDB is free software, covered by the GNU General Public License, and you are welcome to change it and/or distribute copies of it under certain conditions. Type "show copying" to see the conditions. There is absolutely no warranty for GDB.  Type "show warranty" for details. This GDB was configured as "i586-suse-linux"... (no debugging symbols found) Using host libthread_db library "/lib/libthread_db.so.1". 
warning: core file may not match specified executable file. (no debugging symbols found) Failed to read a valid object file image from memory. Core was generated by `/opt/tomcat/bin/jsvc'. #0  0xffffe410 in _start ()

 What you are looking for is in this line:

Core was generated by `/opt/tomcat/bin/jsvc'.

 Call jmap with this binary and you will get a heapdump.

来源： <[http://dikar.iteye.com/blog/643196](http://dikar.iteye.com/blog/643196)> 
 
