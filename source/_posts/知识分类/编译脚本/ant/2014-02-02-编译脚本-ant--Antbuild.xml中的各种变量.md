---
layout: post
title: "Ant build.xml中的各种变量"
categories: 编译脚本
tags: 
 - 编译脚本
 - ant
--- 

# Ant build.xml中的各种变量

Ant build.xml中的各种变量

Ant环境变量分为四种：

1.      build.properties文件中定义的变量

2.      build.xml文件中定义的变量，

3.      windows系统默认自带的环境变量，

4.      windows系统SET的环境变量。

1，3，4都是为2提供变量支持

 

基础储备：

Builid.xml开头一般是固定形式如下：

<!--变量设置 :name工程名 basedir相对根目录，为以后创建目录做参照 . 表示当前目录-->

<project name="project_name" basedir="." default="task_name" xmlns:ivy="antlib:fr.jayasoft.ivy.ant">

      <!-- 变量设置 -->

      <!-- <property environment="env"/> 必须放在最前面，可以确保能使用到编译平台的环境变量 -->

      <!-- <property name="project.root" value="${basedir}" /> 必须放在第二句，在build.properties中不需要再设置此属性 -->

      <property environment="env" />

      <property name="project.root" value="${basedir}" />

<--以上两句一是引用环境变量声明，二是去定根目录，为后来的目录结构奠定基础-->

      <!—下句是import进ant属性配置文件，properties文件里存放基本的配置变量。该变量可以在build.xml中直接引用 -->

      <property file="build.properties" />

<--上句是引用外部文件-->

 

 

 

l        build.properties定义的变量

build.properties定义变量非常的方便只要paramname=paranamevalue的形式

具体一下形式：

#直接定义

rel.dir=rel

project.name=some_project_name

project.revision=1.1.0

#间接引用build.properties中定义的变量

publish.dir=${rel.dir}/${project.revision}

#间接引用build.xml中定义的变量

deploy.exploded.dir=${project.root}/dist/${project.name}

deploy.ear.dir=${project.root}/dist/weblogic

#引用系统环境变量，注意要加前缀env.这个已经在build.xml文件中声明了

lib.wls.dir=${env.WL_HOME}/server/lib

weblogic.jar=${lib.wls.dir}/weblogic.jar

 

 

l        Build.xml定义的变量

build.xml定义的变量又称为属性。

定义形式<property name="some_name"  value="some_value" />

Value中可以引用：

Build.xml前面定义的变量param 引用形式：${paramname}

系统SET的环境变量，通过${env.paramname}来引用

windows系统自带环境变量，直接用{param.name}引用

 

l        windows系统默认自带环境变量  

直接用{param.name}引用

 

${user.home}环境变量

user.home路径，linux下为/home/，windows下一般为C:Document and Settings。其中为当前用户名。也可以在Ant中利用系统环境变量结合进行设置，这样更为灵活。windwos下的环境变量为HOMEPATH，linux下为HOME。

 

${user.user}环境变量

这个可以在ant中直接引用，表示当前机器的用户名。

 

l        windows系统SET的环境变量

<property environment="env" />通过该语句引进系统环境变量；一般该语句放在project的第一条。

通过${env.paramname}来引用

 

至此Ant中的变量都搞清楚了，也就是学习Ant的第一步走通了。这一步通了，读biuld.xml文件豁然开朗了。
来源： <[http://blog.csdn.net/hittata/article/details/4744653](http://blog.csdn.net/hittata/article/details/4744653)> 
 

## [命令行和ant脚本的参数传递](http://www.blogjava.net/zhyiwww/archive/2011/09/02/357823.html)

比如在执行build.xml的某些任务时候，需要从外面的命令行传递参数给ant脚本。
可以通过以下的方式进行参数传入：
ant -f ../../build.xml idc.$type.$ismenu.war -Dparent_version=$parent_version -Dson_version=$son_version
使用方法：   
    在build.xml文件定义如下属性：
   <property name="parent.version" value="${parent_version}" />
   <property name="son.version" value="${son_version}" />

 
来源： <[http://www.blogjava.net/zhyiwww/archive/2011/09/02/357823.html](http://www.blogjava.net/zhyiwww/archive/2011/09/02/357823.html)> 
