---
layout: post
title: "用Ant生成Jar文件时在manifest中加入打包时间以及删除多级目录"
categories: 编译脚本
tags: 
 - 编译脚本
 - ant
--- 

# 用Ant生成Jar文件时在manifest中加入打包时间以及删除多级目录

用tstamp和manifest实现。

<target name="release" depends="copy" description="build run JAR">

  <tstamp>  

<format property="TODAY" pattern="yyyy-MM-dd HH:mm:ss" />  

</tstamp>  

<jar  destfile="${build}/lib/DataCollectServer.jar">

<fileset dir="${build}/bin">‍

<exclude name="**/*.xml" />

<exclude name="**/*.properties" />

<exclude name="wrapper*" />

<exclude name="service*" />

<exclude name="conf" />

<exclude name="**/*.xsd"/>

</fileset>

<manifest >  

<attribute name="Built-By" value="${user.name}" />  

<section name="common">  

<attribute name="Specification-Title" value="DataCollectServer" />  

<attribute name="Specification-Version" value="${version}" />  

<attribute name="Specification-Vendor" value="${organization}" />  

<attribute name="Built-Date" value="${TODAY}" />  

</section>  

</manifest> 

</jar>

</target>

anifest 的基本信息,<manifest>标签中可以定义以下属性：
file 创建或更新的文件
mode(default replace)  "update" 或 "replace" 中的一个，
encoding(defaults UTF-8)  更新时读取已存在文件的编码
内嵌 <attribute> 标签属性
name 属性名，须匹配[A-Za-z0-9][A-Za-z0-9-_]*.
value 属性值
内嵌 <section> 可以有<attribute> 标签属性
下面就一些常见的属性名作下说明
Manifest-Version  用来定义 manifest 文件的版本
Created-By  声明该文件的生成者，一般该属性是由 jar 命令行工具生成
Signature-Version  定义 jar 文件的签名版本
Class-Path  应用程序或者类装载器使用该值来构建内部的类搜索路径
Main-Class 定义 jar 文件的入口类，该类必须是一个可执行的类，一旦定义了该属性即可通过 java -jar x.jar 来运行该jar文件
Implementation-Title  定义了扩展实现的标题
Implementation-Version  定义扩展实现的版本
Implementation-Vendor  定义扩展实现的组织  
Implementation-Vendor-Id  定义扩展实现的组织的标识
Implementation-URL  定义该扩展包的下载地址(URL)
Specification-Title  定义扩展规范的标题
Specification-Version  定义扩展规范的版本
Specification-Vendor  声明了维护该规范的组织
Sealed  定义jar文件是否封存，值可以是 true 或者 false

Ant删除多级目录：

一个小功能，但是花了几个小时。记录下来。

<project name="folder-delete" default="clear-files" basedir=".">  

      <target name="clear-files">  

        <echo message="${basedir}"/>  

        <delete includeemptydirs="true" verbose="true" failonerror="false">  

          <fileset dir="${basedir}/content">  

        <include name="apps/av_template*/**"/>  

          </fileset>  

        </delete>  

      </target>  

    </project>

执行ant clear-files 将删除当前工程目录下的content/apps/下面的所有av_template*为目录名的目录。

注意：

1.failonerror="false"可以保证如果要删除的目录不存在，也不会报错

2.verbose="true" 能够看到详细信息，便于调试

3.dirset不能用在delete内部，只能用fileset

4.虽然是要删除目录，但是av_template*/后面必须加上**
