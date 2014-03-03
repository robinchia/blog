---
layout: post
title: "Maven实战（二）构建简单Maven项目"
categories: 编译脚本
tags: 
 - 编译脚本
 - maven
--- 

# Maven实战（二）构建简单Maven项目

上一节讲了maven的安装和配置，这一节我们来学习一下创建一个简单的Maven项目

1. 用Maven 命令创建一个简单的Maven项目

在cmd中运行如下命令：
C代码  [![收藏代码]()]( "收藏这段代码")

1. mvn archetype:generate   
1. -DgroupId=com.mycompany.app   
1. -DartifactId=my-app-simple  
1.  -Dversion=1.0   
1. -DarchetypeArtifactId=maven-archetype-quickstart  

 即可在当前目录创建一个简单的maven项目，当然创建的时候会从Maven库中下载相关的依赖，耐心等待即可。

maven的大致结构如下：
Java代码  [![收藏代码]()]( "收藏这段代码")

1. my-app  
1. |-- pom.xml  
1. `-- src  
1.     |-- main  
1.     |   |-- java  
1.     |   |   `-- com  
1.     |   |       `-- mycompany  
1.     |   |           `-- app  
1.     |   |               `-- App.java  
1.     |   `-- resources  
1.     |       `-- META-INF  
1.     |           `-- application.properties  
1.     `-- test  
1.         `-- java  
1.             `-- com  
1.                 `-- mycompany  
1.                     `-- app  
1.                         `-- AppTest.java  

 

  **src/main/java** : java源文件存放位置

   **src/main/resource** : resource资源，如配置文件等

   **src/test/java** : 测试代码源文件存放位置

 

2.简单POM.xml

 打开项目即可看到pom.xml
Xml代码  [![收藏代码]()]( "收藏这段代码")

1. <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
1.   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">  
1.   <modelVersion>4.0.0</modelVersion>  
1.   <groupId>com.mycompany.app</groupId>  
1.   <artifactId>my-app-simple</artifactId>  
1.   <packaging>jar</packaging>  
1.   <version>1.0</version>  
1.   <name>my-app-simple</name>  
1.   <url>http://maven.apache.org</url>  
1.   <dependencies>  
1.     <dependency>  
1.       <groupId>junit</groupId>  
1.       <artifactId>junit</artifactId>  
1.       <version>3.8.1</version>  
1.       <scope>test</scope>  
1.     </dependency>  
1.   </dependencies>  
1. </project>  

 这段代码中最重要的是包含groupId, artifactId 和 version 的三行。这三个元素定义了一个项目基本的坐标

** **

**groupId** 定义了项目属于哪个组，这个组往往和项目所在的组织或公司存在关联。譬如在googlecode上建立了一个名为myapp的项目，那么groupId就应该是com.googlecode.myapp

 

**artifactId** 定义了当前Maven项目在组织中唯一的ID, 可以理解为项目中的模块, 模块为Maven中最小单位构件

****

**version** 项目的版本

 

   

3.运行简单Maven命令

 我们已经创建了最简单的Maven项目，下面我们来执行一些简单的构建命令

 

**  编译： compile**

在cmd中，将目录切换到my-app-simple下，执行mvn clean compile

build success之后我们会在my-app-simple下看到新增了一个target目录，该目录下存放项目编译后的文件，如.class文件

 

**  清理： clean**

cmd目录my-app-simple下执行命令 mvn clean

会将target文件删除，即清理项目，该命令可以结合其他命令运行

 

**  测试: test**

cmd目录my-app-simple下执行命令 mvn test

会执行src/test/java 下的Junit 测试代码

当然在执行测试之前会自动执行编译命令，运行结果如下图：

![]()
 

 **打包: package**

 cmd目录my-app-simple下执行命令 mvn package

 会将项目打成jar包，并放在target目录中

 执行此命令之前会先执行编译和测试命令

 

 **安装：install **

 cmd目录my-app-simple下执行命令 mvn install

 会将项目jar包安装到本地仓库中，以便其他项目使用

执行此命令之前会先执行编译，测试，打包命令 
来源： <[http://tangyanbo.iteye.com/blog/1503489](http://tangyanbo.iteye.com/blog/1503489)> 
