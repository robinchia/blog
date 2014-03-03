---
layout: post
title: "Maven实战（六）依赖"
categories: 编译脚本
tags: 
 - 编译脚本
 - maven
--- 

# Maven实战（六）依赖

我们项目中用到的jar包可以通过依赖的方式引入，构建项目的时候从Maven仓库下载即可。

**** 

**1. 依赖配置 
**   依赖可以声明如下： 
  
Xml代码  [![收藏代码]()]( "收藏这段代码")

1. <project>  
1.   ...  
1.   <dependencies>  
1.     <dependency>  
1.       <groupId>group-a</groupId>  
1.       <artifactId>artifact-a</artifactId>  
1.       <version>1.0</version>  
1.       <exclusions>  
1.         <exclusion>  
1.           <groupId>group-c</groupId>  
1.           <artifactId>excluded-artifact</artifactId>  
1.         </exclusion>  
1.       </exclusions>  
1.     </dependency>  
1.     <dependency>  
1.       <groupId>group-a</groupId>  
1.       <artifactId>artifact-b</artifactId>  
1.       <version>1.0</version>  
1.       <type>bar</type>  
1.       <scope>runtime</scope>  
1.     </dependency>  
1.   </dependencies>  
1. </project>  

 我们在Maven实战(二)中就遇到了依赖的概念，项目中测试需要依赖junit jar包，依赖配置如下：

Xml代码  [![收藏代码]()]( "收藏这段代码")

1. <dependencies>  
1.     <dependency>  
1.       <groupId>junit</groupId>  
1.       <artifactId>junit</artifactId>  
1.       <version>3.8.1</version>  
1.       <scope>test</scope>  
1.     </dependency>  
1.  </dependencies>  

 依赖会包含基本的groupId, artifactId,version等元素，根元素project下的dependencies可以包含一个或者多个dependency元素，以声明一个或者多个依赖。
 下面详细讲解每个依赖可以包含的元素：

 

  **groupId**,**artifactId**和**version**：依赖的基本坐标，对于任何一个依赖来说，基本坐标是最重要的，Maven根据坐标才能找到需要的依赖

 

  **type**: 依赖的类型，对应于项目坐标定义的packaging。大部分情况下，该元素不必声明，其默认值是jar

 

  **scope**: 依赖的范围，下面会进行详解

 

  **optional**: 标记依赖是否可选

 

  **exclusions**: 用来排除传递性依赖，下面会进行详解

 

  大部分依赖声明只包含基本坐标。

**2. 依赖范围**

 Maven在编译主代码的时候需要使用一套classpath,在编译和执行测试的时候会使用另一套classpath,实际运行项目的时候，又会使用一套classpath。

 依赖范围就是用来控制依赖与这三种classpath(编译classpath、测试classpath、运行classpath)的关系，Maven有以下几种依赖范围：

 

**compile**: 编译依赖范围。如果没有指定，就会默认使用该依赖范围。使用此依赖范围的Maven依赖，对于编译、测试、运行三种classpath都有效。

 

**test**: 测试依赖范围。使用此依赖范围的Maven依赖，只对于测试classpath有效，在编译主代码或者运行项目的使用时将无法使用此类依赖。典型的例子就是JUnit，它只有在编译测试代码及运行测试的时候才需要。

 

**provided**: 已提供依赖范围。使用此依赖范围的Maven依赖，对于编译和测试classpath有效，但在运行时无效。典型的例子是servlet-api，编译和测试项目的时候需要该依赖，但在运行项目的时候，由于容器已经提供，就不需要Maven重复地引入一遍。

 

**runtime**: 运行时依赖范围。使用此依赖范围的Maven依赖，对于测试和运行classpath有效，但在编译主代码时无效。典型的例子是JDBC驱动实现，项目主代码的编译只需要JDK提供的JDBC接口，只有在执行测试或者运行项目的时候才需要实现上述接口的具体JDBC驱动。

 

**system**: 系统依赖范围。该依赖与三种classpath的关系，和provided依赖范围完全一致。但是，使用system范围依赖时必须通过systemPath元素显式地指定依赖文件的路径。由于此类依赖不是通过Maven仓库解析的，而且往往与本机系统绑定，可能造成构建的不可移植，因此应该谨慎使用。systemPath元素可以引用环境变量，如：
Xml代码  [![收藏代码]()]( "收藏这段代码")

1. <dependency>  
1.     <groupId>javax.sql</groupId>  
1.     <artifactId>jdbc-stdext</artifactId>  
1.     <version>2.0</version>  
1.     <scope></scope>  
1.     <systemPath>${java.home}/lib/rt.jar</systemPath>  
1. </dependency>  

 

**import**(Maven 2.0.9及以上): 导入依赖范围。该依赖范围不会对三种classpath产生实际的影响，稍后会介绍到。

 

**3. 传递性依赖**

 下面我们看一个简单的项目，读者可从附件中下载源码

POM.xml配置如下：
Xml代码  [![收藏代码]()]( "收藏这段代码")

1. <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
1.   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
1.   <modelVersion>4.0.0</modelVersion>  
1.   
1.   <groupId>com.mycompany.app</groupId>  
1.   <artifactId>my-app-simple</artifactId>  
1.   <version>0.0.1-SNAPSHOT</version>  
1.   <packaging>jar</packaging>  
1.   
1.   <name>my-app-simple</name>  
1.   <url>http://maven.apache.org</url>  
1.   
1.   <properties>  
1.     <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
1.   </properties>  
1.   
1.   <dependencies>  
1.     <dependency>  
1.       <groupId>junit</groupId>  
1.       <artifactId>junit</artifactId>  
1.       <version>3.8.1</version>  
1.       <scope>test</scope>  
1.     </dependency>  
1.       
1.      <dependency>  
1.       <groupId>org.springframework</groupId>  
1.       <artifactId>spring-core</artifactId>  
1.       <version>2.5.6</version>  
1.     </dependency>  
1.   </dependencies>  
1. </project>  

 我们可以看到此项目引入依赖junit和spring-core，我们可以在Maven仓库中查找spring-core构件，如图：

![]()

点击POM我们会看到该文件包含了一个commons-logging依赖：
Xml代码  [![收藏代码]()]( "收藏这段代码")

1. <dependency>  
1.   <groupId>commons-logging</groupId>   
1.   <artifactId>commons-logging</artifactId>   
1.   <version>1.1.1</version>   
1. </dependency>  

 
 那么该依赖会传递到当前项目中，这就是依赖的传递性，打开项目查看Maven dependencies:

![]()
 

 

**4. 可选依赖**

 有时候我们不想让依赖传递，那么可配置该依赖为可选依赖，将元素optional设置为true即可,例如：
Xml代码  [![收藏代码]()]( "收藏这段代码")

1. <dependency>  
1.   <groupId>commons-logging</groupId>   
1.   <artifactId>commons-logging</artifactId>   
1.   <version>1.1.1</version>   
1.   <optional>true<optional>  
1. </dependency>  

那么依赖该项目的另以项目将不会得到此依赖的传递

 

** 5. 排除依赖**

****

****

       当我们引入第三方jar包的时候，难免会引入传递性依赖，有些时候这是好事，然而有些时候我们不需要其中的一些传递性依赖

比如上例中的项目，我们不想引入传递性依赖commons-logging，我们可以使用exclusions元素声明排除依赖，exclusions可以包含一个或者多个exclusion子元素，因此可以排除一个或者多个传递性依赖。需要注意的是，声明exclusions的时候只需要groupId和artifactId，而不需要version元素，这是因为只需要groupId和artifactId就能唯一定位依赖图中的某个依赖。换句话说，Maven解析后的依赖中，不可能出现groupId和artifactId相同，但是version不同的两个依赖。

 如下是一个排除依赖的例子：
Xml代码  [![收藏代码]()]( "收藏这段代码")

1. <dependency>    
1.      <groupId>org.springframework</groupId>  
1.      <artifactId>spring-core</artifactId>  
1.      <version>2.5.6</version>  
1.      <exclusions>  
1.            <exclusion>      
1.                 <groupId>commons-logging</groupId>          
1.                 <artifactId>commons-logging</artifactId>  
1.            </exclusion>  
1.      </exclusions>  
1. </dependency>  

  

** 5. 依赖归类**

 如果我们项目中用到很多关于Spring Framework的依赖，它们分别是org.springframework:spring-core:2.5.6, org.springframework:spring-beans:2.5.6,org.springframework:spring-context:2.5.6,它们都是来自同一项目的不同模块。因此，所有这些依赖的版本都是相同的，而且可以预见，如果将来需要升级Spring Framework，这些依赖的版本会一起升级。因此，我们应该在一个唯一的地方定义版本，并且在dependency声明引用这一版本，这一在Spring Framework升级的时候只需要修改一处即可。
Xml代码  [![收藏代码]()]( "收藏这段代码")

1. <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
1.     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
1.     <modelVersion>4.0.0</modelVersion>  
1.   
1.     <groupId>com.mycompany.app</groupId>  
1.     <artifactId>my-app-simple</artifactId>  
1.     <version>0.0.1-SNAPSHOT</version>  
1.     <packaging>jar</packaging>  
1.     <name>my-app-simple</name>  
1.     <properties>  
1.         <springframework.version>2.5.6</springframework.version>  
1.     </properties>  
1.   
1.     <dependencies>  
1.         <dependency>  
1.             <groupId>junit</groupId>  
1.             <artifactId>junit</artifactId>  
1.             <version>3.8.1</version>  
1.             <scope>test</scope>  
1.         </dependency>  
1.   
1.         <dependency>  
1.             <groupId>org.springframework</groupId>  
1.             <artifactId>spring-core</artifactId>  
1.             <version>${springframework.version}</version>  
1.         </dependency>  
1.         <dependency>  
1.             <groupId>org.springframework</groupId>  
1.             <artifactId>spring-beans</artifactId>  
1.             <version>${springframework.version}</version>             
1.         </dependency>  
1.     </dependencies>  
1. </project>  

 

**6. 在Eclipse中管理依赖**

安装好m2eclipse之后（第2课有详细讲解）就可以用eclipse来管理依赖。

如图，在该项目的pom.xml中点击Dependency Hierarchy可以看到依赖树:

![]()
 

 点击Dependencies可以添加新的依赖，点击选择一个依赖，点击remove可以删除，点击Add可以新增一个依赖，如图：
![]()
 

如下图，搜素org.springframework（此处是从Maven中心仓库进行搜索），选择你想要的模块和版本，点击OK即可：

 
![]()
来源： <[http://tangyanbo.iteye.com/blog/1503957](http://tangyanbo.iteye.com/blog/1503957)> 
