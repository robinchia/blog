---
layout: post
title: "perftools查看堆外内存并解决hbase内存溢出"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - JVM
--- 

# perftools查看堆外内存并解决hbase内存溢出

    最近线上运行的hbase发现分配了16g内存，但是实际使用了22g，堆外内存达到6g。感觉非常诡异。堆外内存用一般的工具很难查看，可以通过google-perftools来跟踪：
[http://code.google.com/p/google-perftools/downloads/list](http://code.google.com/p/google-perftools/downloads/list)
    它的原理是在java应用程序运行时，当调用malloc时换用它的libtcmalloc.so，这样就能做一些统计了

* 下载[http://download.savannah.gnu.org/releases/libunwind/libunwind-0.99-beta.tar.gz](http://download.savannah.gnu.org/releases/libunwind/libunwind-0.99-beta.tar.gz)，configure;make;sudo make install
* 下载[http://google-perftools.googlecode.com/files/google-perftools-1.8.1.tar.gz](http://google-perftools.googlecode.com/files/google-perftools-1.8.1.tar.gz), configure --prefix=/home/user/perftools;make;sudo make install
* 在应用程序启动前加入：export LD_PRELOAD=/home/hadoop/perftools/lib/libtcmalloc.so以及export HEAPPROFILE=/home/user/perftools/test
* 修改lc_config:sudo vi /etc/ld.so.conf.d/usr_local_lib.conf，加入/usr/local/lib(libunwind的lib所在目录)
* 执行sudo /sbin/ldconfig，使libunwind生效
* 启动应用程序，此时会在/home/user/perftools/下看到诸如test_pid.xxxx.heap的heap文件，可使用bin/pprof --text $JAVA_HOME/bin/java test_pid.xxxx.heap来查看
    通过perftools查看到以下内容：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. Total: 3263.2 MB  
1.   3145.2  96.4%  96.4%   3145.2  96.4% zcalloc  
1.     83.8   2.6%  99.0%     83.8   2.6% os::malloc  
1.     30.0   0.9%  99.9%     30.0   0.9% init  
1.      2.2   0.1%  99.9%      2.2   0.1% ObjectSynchronizer::omAlloc  
1.      1.0   0.0% 100.0%   3144.1  96.4% Java_java_util_zip_Deflater_init  
1.      0.6   0.0% 100.0%      0.7   0.0% readCEN  

Total: 3263.2 MB

  3145.2  96.4%  96.4%   3145.2  96.4% zcalloc
    83.8   2.6%  99.0%     83.8   2.6% os::malloc

    30.0   0.9%  99.9%     30.0   0.9% init
     2.2   0.1%  99.9%      2.2   0.1% ObjectSynchronizer::omAlloc

     1.0   0.0% 100.0%   3144.1  96.4% Java_java_util_zip_Deflater_init
     0.6   0.0% 100.0%      0.7   0.0% readCEN
    可见调用了java.util.zip.Deflater占用绝大多数。了解到这个deflater存在无法释放内存的bug，于是编写btrace查看是否进入了这个函数：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. import static com.sun.btrace.BTraceUtils.*;  
1. import com.sun.btrace.annotations.*;  
1.   
1. import java.nio.ByteBuffer;  
1. import java.lang.Thread;  
1.   
1. @BTrace public class TestRegion1{  
1.    @OnMethod(  
1.       clazz="java.util.zip.Deflater",  
1.       method="deflate"  
1.    )  
1.    public static void traceCacheBlock(){  
1. println("deflate?");  
1.    }  
1. }  

import static com.sun.btrace.BTraceUtils.*;

import com.sun.btrace.annotations.*;
 

import java.nio.ByteBuffer;
import java.lang.Thread;

 
@BTrace public class TestRegion1{

   @OnMethod(
      clazz="java.util.zip.Deflater",

      method="deflate"
   )

   public static void traceCacheBlock(){
println("deflate?");

   }
}
    发现果然在不停调用这行代码。应该如何办呢？
由于deflater是gzip需要使用的代码，查看用户创建的表，发现COMPRESSOR设置的是GZ，尝试调整为LZO，结果发现btrace无法进入上述代码，再通过perftools查看时，堆内存不再申请，完全不再申请...
小插曲，perftools的作者是个老实人，提供了zip版下载，但是不提供安装文件，原因？在README中有以下一段话：
Html代码  [![收藏代码]()![]()]( "收藏这段代码")

1. I don't know very much about how to install DLLs on Windows, so you'll  
1. have to figure out that part for yourself.  

I don't know very much about how to install DLLs on Windows, so you'll

have to figure out that part for yourself.
