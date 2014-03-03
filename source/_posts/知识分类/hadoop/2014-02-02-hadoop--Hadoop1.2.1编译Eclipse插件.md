---
layout: post
title: "Hadoop 1.2.1编译Eclipse插件"
categories: hadoop
tags: 
 - hadoop
--- 

# Hadoop 1.2.1编译Eclipse插件

## /src/contrib/eclipse-plugin/build.xml

### 1）取消ivy-download：

[![]()](http://static.oschina.net/uploads/space/2013/0520/194613_IBLw_256028.png)

### 2）添加将要打包到plugin中的第三方jar包列表：  

01

<!-- Override jar target to specify manifest -->
 

02

<

target
 

name

=

"jar"
 

depends

=

"compile"
 

unless

=

"skip.contrib"

>
03

<

mkdir
 

dir

=

"${build.dir}/lib"

/> 

04
 
05

<!-- 自定义的修改内容：begin -->
 

06

<!--
07

<copy file="${hadoop.root}/build/hadoop-core-${version}.jar"

08

  

tofile="${build.dir}/lib/hadoop-core.jar" verbose="true"/>
09

<copy file="${hadoop.root}/build/ivy/lib/Hadoop/common/commons-cli-${commons-cli.version}.jar" 

10

  

todir="${build.dir}/lib" verbose="true"/> 
11

-->
 

12

<

copy
 

file

=

"${hadoop.root}/hadoop-core-${version}.jar"
 

tofile

=

"${build.dir}/lib/hadoop-core.jar"
 

verbose

=

"true"

/>
13

<

copy
 

file

=

"${hadoop.root}/lib/commons-cli-1.2.jar"
  

todir

=

"${build.dir}/lib"
 

verbose

=

"true"

/> 

14

<

copy
 

file

=

"${hadoop.root}/lib/commons-configuration-1.6.jar"
  

todir

=

"${build.dir}/lib"
 

verbose

=

"true"

/> 
15

<

copy
 

file

=

"${hadoop.root}/lib/commons-httpclient-3.0.1.jar"
  

todir

=

"${build.dir}/lib"
 

verbose

=

"true"

/> 

16

<

copy
 

file

=

"${hadoop.root}/lib/commons-lang-2.4.jar"
  

todir

=

"${build.dir}/lib"
 

verbose

=

"true"

/>  
17

<

copy
 

file

=

"${hadoop.root}/lib/jackson-core-asl-1.8.8.jar"
  

todir

=

"${build.dir}/lib"
 

verbose

=

"true"

/>

18

<

copy
 

file

=

"${hadoop.root}/lib/jackson-mapper-asl-1.8.8.jar"
  

todir

=

"${build.dir}/lib"
 

verbose

=

"true"

/>  
19

<!-- 自定义的修改内容：end -->
 

20

<

jar
21

jarfile

=

"${build.dir}/hadoop-${name}-${version}.jar"

22

manifest

=

"${root}/META-INF/MANIFEST.MF"

> 
23

<

fileset
 

dir

=

"${build.dir}"
 

includes

=

"classes/ lib/"

/> 

24

<

fileset
 

dir

=

"${root}"
 

includes

=

"resources/ plugin.xml"

/> 
25

</

jar

> 

26

</

target

><

span
 

style

=

"font-size:10pt;line-height:1.5;font-family:'sans serif', tahoma, verdana, helvetica;"

>  </

span

>

[![]()](http://static.oschina.net/uploads/space/2013/0520/194752_bBaX_256028.png) 

## 3.%hadoop%/src/contrib/build-contrib.xml ： 

### 1）添加hadoop的version和eclipse的eclipse.home属性：  

1

<?

xml
 

version

=

"1.0"

?>

2

<!-- Imported by contrib/*/build.xml files to share generic targets. -->
3

<

project
 

name

=

"hadoopbuildcontrib"
 

xmlns:ivy

=

"antlib:org.apache.ivy.ant"

>

4

<

property
 

name

=

"name"
 

value

=

"${ant.project.name}"

/>
5

<

property
 

name

=

"root"
 

value

=

"${basedir}"

/>

6

<

property
 

name

=

"hadoop.root"
 

location

=

"${root}/../../../"

/>
7

<!-- hadoop版本、eclipse安装路径 -->

8

<

property
 

name

=

"version"
 

value

=

"1.1.2"

/>
9

<

property
 

name

=

"eclipse.home"
 

location

=

"%eclipse%"

/>

[![]()](http://static.oschina.net/uploads/space/2013/0520/194838_mpUD_256028.png)  

### 2）取消ivy-download： 

[![]()](http://static.oschina.net/uploads/space/2013/0520/194859_Cc5J_256028.png) 
来源： <[http://my.oschina.net/vigiles/blog/132238](http://my.oschina.net/vigiles/blog/132238)>

配置/${hadoop-home}/src/contrib/eclipse-plugins/build.xml

找到

<path id=”classpath”>

然后添加

<fileset dir=”${hadoop.root}/”>
      <include name=”*.jar”/>
</fileset>

**这一步很重要，如果不添加的话会出现找不到相应的程序包，错误如下（给出部分）** ：

[javac]      Counters.Group group = counters.getGroup(groupName);
    [javac]              ^
    [javac] /home/summerdg/hadoop_src/hadoop-1.2.1/src/contrib/eclipse-plugin/src/java/org/apache/hadoop/eclipse/server/HadoopJob.java:305: 错误: 程序包Counters不存在
    [javac]      for (Counters.Counter counter : group) {
    [javac]                    ^
    [javac] /home/summerdg/hadoop_src/hadoop-1.2.1/src/contrib/eclipse-plugin/src/java/org/apache/hadoop/eclipse/dfs/DFSFile.java:74: 错误: 找不到符号
    [javac]      FileStatus fs = getDFS().getFileStatus(path);
    [javac]      ^
    [javac]  符号:  类 FileStatus
    [javac]  位置: 类 DFSFile
    [javac] 注: 某些输入文件使用或覆盖了已过时的 API。
    [javac] 注: 有关详细信息, 请使用 -Xlint:deprecation 重新编译。
    [javac] 注: 某些输入文件使用了未经检查或不安全的操作。
    [javac] 注: 有关详细信息, 请使用 -Xlint:unchecked 重新编译。
    [javac] 100 个错误

当然，出现这个错误还有个原因，就是你已经设置了这句，但你依然出现这个错误，那就是你eclipse版本的问题了。

找到
来源： <[http://www.kankanews.com/ICkengine/archives/63441.shtml](http://www.kankanews.com/ICkengine/archives/63441.shtml)> 

## 4.编辑%HADOOP_HOME%/build.xml：

### 1）修改hadoop版本号：

[![]()](http://static.oschina.net/uploads/space/2013/0520/194950_g5cw_256028.png)

### 2）取消ivy-download：

[![]()](http://static.oschina.net/uploads/space/2013/0520/195011_l8BK_256028.png) 

## 5.修改%hadoop%/src/contrib/eclipse-plugin/META-INF/MANIFEST.MF： 

修改${HADOOP_HOME}/src/contrib/eclipse-plugin/META-INF/MANIFEST.MF的Bundle-ClassPath：
1

Bundle-ClassPath: classes/,

2

 

lib/hadoop-core.jar,
3

 

lib/commons-cli-1.2.jar,

4

 

lib/commons-configuration-1.6.jar,
5

 

lib/commons-httpclient-3.0.1.jar,

6

 

lib/commons-lang-2.4.jar,
7

 

lib/jackson-core-asl-1.8.8.jar,

8

 

lib/jackson-mapper-asl-1.8.8.jar

[![]()](http://static.oschina.net/uploads/space/2013/0520/195106_mqwV_256028.png) 
 

## 进入到 /build/contrib/eclipse-plugin/执行 ant jar ：  

01

hep@hep-ubuntu:~$ 

cd
 

~/hadoop/src/contrib/eclipse-plugin

02

hep@hep-ubuntu:~/hadoop/src/contrib/eclipse-plugin$ ant jar
03

Buildfile: /home/hep/hadoop/src/contrib/eclipse-plugin/build.xml

04
 
05

check-contrib:

06
 
07

init:

08

     

[

echo

] contrib: eclipse-plugin
09
 

10

init-contrib:
11
 

12

ivy-probe-antlib:
13
 

14

ivy-init-antlib:
15
 

16

ivy-init:
17

[ivy:configure] :: Ivy 2.1.0 - 20090925235825 :: http://ant.apache.org/ivy/ ::

18

[ivy:configure] :: loading settings :: 

file
 

= /home/hep/hadoop/ivy/ivysettings.xml
19
 

20

ivy-resolve-common:
21

[ivy:resolve] :: resolving dependencies :: org.apache.hadoop

#eclipse-plugin;working@hep-ubuntu

22

[ivy:resolve]   confs: [common]
23

[ivy:resolve]   found commons-logging

#commons-logging;1.0.4 in maven2

24

[ivy:resolve]   found log4j

#log4j;1.2.15 in maven2
25

[ivy:resolve] :: resolution report :: resolve 171ms :: artifacts dl 4ms

26

    

---------------------------------------------------------------------
27

    

|                  |            modules            ||   artifacts   |

28

    

|       conf       | number| search|dwnlded|evicted|| number|dwnlded|
29

    

---------------------------------------------------------------------

30

    

|      common      |   2   |   0   |   0   |   0   ||   2   |   0   |
31

    

---------------------------------------------------------------------

32
 
33

ivy-retrieve-common:

34

[ivy:retrieve] :: retrieving :: org.apache.hadoop

#eclipse-plugin [sync]
35

[ivy:retrieve]  confs: [common]

36

[ivy:retrieve]  0 artifacts copied, 2 already retrieved (0kB/6ms)
37

[ivy:cachepath] DEPRECATED: 

'ivy.conf.file'
 

is deprecated, use 

'ivy.settings.file'
 

instead

38

[ivy:cachepath] :: loading settings :: 

file
 

= /home/hep/hadoop/ivy/ivysettings.xml
39
 

40

compile:
41

     

[

echo

] contrib: eclipse-plugin

42

    

[javac] /home/hep/hadoop/src/contrib/eclipse-plugin/build.xml:61: warning: 

'includeantruntime'
 

was not 

set

, defaulting to build.sysclasspath=last; 

set
 

to 

false
 

for
 

repeatable builds
43
 

44

jar:
45

    

[

mkdir

] Created 

dir

: /home/hep/hadoop/build/contrib/eclipse-plugin/lib

46

     

[copy] Copying 1 

file
 

to /home/hep/hadoop/build/contrib/eclipse-plugin/lib
47

     

[copy] Copying /home/hep/hadoop/hadoop-core-1.1.2.jar to /home/hep/hadoop/build/contrib/eclipse-plugin/lib/hadoop-core.jar

48

     

[copy] Copying 1 

file
 

to /home/hep/hadoop/build/contrib/eclipse-plugin/lib
49

     

[copy] Copying /home/hep/hadoop/lib/commons-cli-1.2.jar to /home/hep/hadoop/build/contrib/eclipse-plugin/lib/commons-cli-1.2.jar

50

     

[copy] Copying 1 

file
 

to /home/hep/hadoop/build/contrib/eclipse-plugin/lib
51

     

[copy] Copying /home/hep/hadoop/lib/commons-configuration-1.6.jar to /home/hep/hadoop/build/contrib/eclipse-plugin/lib/commons-configuration-1.6.jar

52

     

[copy] Copying 1 

file
 

to /home/hep/hadoop/build/contrib/eclipse-plugin/lib
53

     

[copy] Copying /home/hep/hadoop/lib/commons-httpclient-3.0.1.jar to /home/hep/hadoop/build/contrib/eclipse-plugin/lib/commons-httpclient-3.0.1.jar

54

     

[copy] Copying 1 

file
 

to /home/hep/hadoop/build/contrib/eclipse-plugin/lib
55

     

[copy] Copying /home/hep/hadoop/lib/commons-lang-2.4.jar to /home/hep/hadoop/build/contrib/eclipse-plugin/lib/commons-lang-2.4.jar

56

     

[copy] Copying 1 

file
 

to /home/hep/hadoop/build/contrib/eclipse-plugin/lib
57

     

[copy] Copying /home/hep/hadoop/lib/jackson-core-asl-1.8.8.jar to /home/hep/hadoop/build/contrib/eclipse-plugin/lib/jackson-core-asl-1.8.8.jar

58

     

[copy] Copying 1 

file
 

to /home/hep/hadoop/build/contrib/eclipse-plugin/lib
59

     

[copy] Copying /home/hep/hadoop/lib/jackson-mapper-asl-1.8.8.jar to /home/hep/hadoop/build/contrib/eclipse-plugin/lib/jackson-mapper-asl-1.8.8.jar

60

      

[jar] Building jar: /home/hep/hadoop/build/contrib/eclipse-plugin/hadoop-eclipse-plugin-1.1.2.jar
61
 

62

BUILD SUCCESSFUL
63

Total 

time

: 3 seconds

生成的插件jar就在本目录中。

把编译好的插件放入 $eclipse_home/dropins/hadoop/plugins/ (mkdir -p 创建)

启动eclipse 新建mapreduce project， 配置好map/reduce location, 就可以看到hdfs中的文件了
