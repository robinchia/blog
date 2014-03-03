---
layout: post
title: "maven 配置篇 之pom.xml"
categories: 编译脚本
tags: 
 - 编译脚本
 - maven
--- 

# maven 配置篇 之pom.xml

## maven 配置篇 之pom.xml(一）

    说完了settings.xml配置，下来说一下maven2的主要配置pom.xml
**什么是pom?**
    pom作为项目对象模型。通过xml表示maven项目，使用pom.xml来实现。主要描述了项目：包括配置文件；开发者需要遵循的规则，缺陷管理系统，组织和licenses，项目的url，项目的依赖性，以及其他所有的项目相关因素。
**快速察看：**
xml 代码

 

1. <project>  
1.   <modelVersion>4.0.0<!---->modelVersion>  
1.   
1.   <!---->  
1.   <groupId>...<!---->groupId>  
1.   <artifactId>...<!---->artifactId>  
1.   <version>...<!---->version>  
1.   <packaging>...<!---->packaging>  
1.   <dependencies>...<!---->dependencies>  
1.   <parent>...<!---->parent>  
1.   <dependencyManagement>...<!---->dependencyManagement>  
1.   <modules>...<!---->modules>  
1.   <properties>...<!---->properties>  
1.   
1.   <!---->  
1.   <build>...<!---->build>  
1.   <reporting>...<!---->reporting>  
1.   
1.   <!---->  
1.   <name>...<!---->name>  
1.   <description>...<!---->description>  
1.   <url>...<!---->url>  
1.   <inceptionYear>...<!---->inceptionYear>  
1.   <licenses>...<!---->licenses>  
1.   <organization>...<!---->organization>  
1.   <developers>...<!---->developers>  
1.   <contributors>...<!---->contributors>  
1.   
1.   <!---->  
1.   <issueManagement>...<!---->issueManagement>  
1.   <ciManagement>...<!---->ciManagement>  
1.   <mailingLists>...<!---->mailingLists>  
1.   <scm>...<!---->scm>  
1.   <prerequisites>...<!---->prerequisites>  
1.   <repositories>...<!---->repositories>  
1.   <pluginRepositories>...<!---->pluginRepositories>  
1.   <distributionManagement>...<!---->distributionManagement>  
1.   <profiles>...<!---->profiles>  
1. <!---->project>  
**基本内容：**
    POM包括了所有的项目信息。
maven 相关：
pom定义了最小的maven2元素，允许groupId,artifactId,version。所有需要的元素

* groupId:项目或者组织的唯一标志，并且配置时生成的路径也是由此生成，如org.codehaus.mojo生成的相对路径为：/org/codehaus/mojo
* artifactId: 项目的通用名称
* version:项目的版本
* packaging: 打包的机制，如pom, jar, maven-plugin, ejb, war, ear, rar, par
* classifier: 分类
**POM关系：**
主要为依赖，继承，合成
**  依赖关系：**
 

xml 代码
 

1. <dependencies>  
1.     <dependency>  
1.       <groupId>junit<!---->groupId>  
1.       <artifactId>junit<!---->artifactId>  
1.       <version>4.0<!---->version>  
1.       <type>jar<!---->type>  
1.       <scope>test<!---->scope>  
1.       <optional>true<!---->optional>  
1.     <!---->dependency>  
1.     ...  
1.   <!---->dependencies>  

* groupId, artifactId, version:描述了依赖的项目唯一标志
可以通过以下方式进行安装：
 
* 使用以下的命令安装：
* mvn install:install-file –Dfile=non-maven-proj.jar –DgroupId=some.group –DartifactId=non-maven-proj –Dversion=1
* 创建自己的库,并配置，使用deploy:deploy-file
* 设置此依赖范围为system，定义一个系统路径。不提倡。

* type:相应的依赖产品包形式，如jar，war
* scope:用于限制相应的依赖范围，包括以下的几种变量：
* compile ：默认范围，用于编译
* provided：类似于编译，但支持你期待jdk或者容器提供，类似于classpath
* runtime:在执行时，需要使用
* test:用于test任务时使用
* system:需要外在提供相应得元素。通过systemPath来取得

* systemPath: 仅用于范围为system。提供相应的路径
* optional: 标注可选，当项目自身也是依赖时。用于连续依赖时使用
**   独占性  **  
   外在告诉maven你只包括指定的项目，不包括相关的依赖。此因素主要用于解决版本冲突问题
 

xml 代码
 

1. <dependencies>  
1.     <dependency>  
1.       <groupId>org.apache.maven<!---->groupId>  
1.       <artifactId>maven-embedder<!---->artifactId>  
1.       <version>2.0<!---->version>  
1.       <exclusions>  
1.         <exclusion>  
1.           <groupId>org.apache.maven<!---->groupId>  
1.           <artifactId>maven-core<!---->artifactId>  
1.         <!---->exclusion>  
1.       <!---->exclusions>  
1.     <!---->dependency>  
表示项目maven-embedder需要项目maven-core，但我们不想引用maven-core
**继承关系**
    另一个强大的变化,maven带来的是项目继承。主要的设置：
定义父项目
xml 代码

 

1. <project>  
1.   <modelVersion>4.0.0<!---->modelVersion>  
1.   <groupId>org.codehaus.mojo<!---->groupId>  
1.   <artifactId>my-parent<!---->artifactId>  
1.   <version>2.0<!---->version>  
1.   <packaging>pom<!---->packaging>  
1. <!---->project>  
    packaging 类型，需要pom用于parent和合成多个项目。我们需要增加相应的值给父pom，用于子项目继承。主要的元素如下：

* 依赖型
* 开发者和合作者
* 插件列表
* 报表列表
* 插件执行使用相应的匹配ids
* 插件配置
* 子项目配置
xml 代码

 

1. <project>  
1.   <modelVersion>4.0.0<!---->modelVersion>  
1.   <parent>  
1.     <groupId>org.codehaus.mojo<!---->groupId>  
1.     <artifactId>my-parent<!---->artifactId>  
1.     <version>2.0<!---->version>  
1.     <relativePath>../my-parent<!---->relativePath>  
1.   <!---->parent>  
1.   <artifactId>my-project<!---->artifactId>  
1. <!---->project>  
relativePath可以不需要，但是用于指明parent的目录，用于快速查询。
**dependencyManagement：**
用于父项目配置共同的依赖关系，主要配置依赖包相同因素，如版本，scope。
**合成（或者多个模块）**
    一个项目有多个模块，也叫做多重模块，或者合成项目。
如下的定义：
xml 代码

 

1. <project>  
1.   <modelVersion>4.0.0<!---->modelVersion>  
1.   <groupId>org.codehaus.mojo<!---->groupId>  
1.   <artifactId>my-parent<!---->artifactId>  
1.   <version>2.0<!---->version>  
1.   <modules>  
1.     <module>my-project1<module>  
1.     <module>my-project2<module>  
1.   <!---->modules>  
1. <!---->project>  
**build 设置**
    主要用于编译设置，包括两个主要的元素，build和report
**  build**
    主要分为两部分，基本元素和扩展元素集合
注意：包括项目build和profile build
xml 代码

 

1. <project>  
1.   <!---->  
1.   <build>...<!---->build>  
1.   <profiles>  
1.     <profile>  
1.       <!---->  
1.       <build>...<!---->build>  
1.     <!---->profile>  
1.   <!---->profiles>  
1. <!---->project>  
基本元素
xml 代码

 

1. <build>  
1.   <defaultGoal>install<!---->defaultGoal>  
1.   <directory>${basedir}/target<!---->directory>  
1.   <finalName>${artifactId}-${version}<!---->finalName>  
1.   <filters>  
1.     <filter>filters/filter1.properties<!---->filter>  
1.   <!---->filters>  
1.   ...  
1. <!---->build>  

* defaultGoal: 定义默认的目标或者阶段。如install
* directory: 编译输出的目录
* finalName: 生成最后的文件的样式
* filter: 定义过滤，用于替换相应的属性文件，使用maven定义的属性。设置所有placehold的值
**资源(resources)**
    你项目中需要指定的资源。如spring配置文件,log4j.properties
xml 代码

 

1. <project>  
1.   <build>  
1.     ...  
1.     <resources>  
1.       <resource>  
1.         <targetPath>META-INF/plexus<!---->targetPath>  
1.         <filtering>false<!---->filtering>  
1.         <directory>${basedir}/src/main/plexus<!---->directory>  
1.         <includes>  
1.           <include>configuration.xml<!---->include>  
1.         <!---->includes>  
1.         <excludes>  
1.           <exclude>**/*.properties<!---->exclude>  
1.         <!---->excludes>  
1.       <!---->resource>  
1.     <!---->resources>  
1.     <testResources>  
1.       ...  
1.     <!---->testResources>  
1.     ...  
1.   <!---->build>  
1. <!---->project>  

* resources: resource的列表，用于包括所有的资源
* targetPath: 指定目标路径，用于放置资源，用于build
* filtering: 是否替换资源中的属性placehold
* directory: 资源所在的位置
* includes: 样式，包括那些资源
* excludes: 排除的资源
* testResources: 测试资源列表
**插件**
  在build时，执行的插件，比较有用的部分，如使用jdk 5.0编译等等
xml 代码

 

1. <project>  
1.   <build>  
1.     ...  
1.     <plugins>  
1.       <plugin>  
1.         <groupId>org.apache.maven.plugins<!---->groupId>  
1.         <artifactId>maven-jar-plugin<!---->artifactId>  
1.         <version>2.0<!---->version>  
1.         <extensions>false<!---->extensions>  
1.         <inherited>true<!---->inherited>  
1.         <configuration>  
1.           <classifier>test<!---->classifier>  
1.         <!---->configuration>  
1.         <dependencies>...<!---->dependencies>  
1.         <executions>...<!---->executions>  
1.       <!---->plugin>  
1.     <!---->plugins>  
1.   <!---->build>  
1. <!---->project>  

* extensions: true or false，是否装载插件扩展。默认false
* inherited: true or false，是否此插件配置将会应用于poms，那些继承于此的项目
* configuration: 指定插件配置
* dependencies: 插件需要依赖的包
* executions: 用于配置execution目标，一个插件可以有多个目标。
如下：
   

xml 代码
 

1. <plugin>  
1.         <artifactId>maven-antrun-plugin<!---->artifactId>  
1.   
1.         <executions>  
1.           <execution>  
1.             <id>echodir<!---->id>  
1.             <goals>  
1.               <goal>run<!---->goal>  
1.             <!---->goals>  
1.             <phase>verify<!---->phase>  
1.             <inherited>false<!---->inherited>  
1.             <configuration>  
1.               <tasks>  
1.                 <echo>Build Dir: ${project.build.directory}<!---->echo>  
1.               <!---->tasks>  
1.             <!---->configuration>  
1.           <!---->execution>  
1.         <!---->executions>  
1.       <!---->plugin>  
  说明：

* id:规定execution 的唯一标志
* goals: 表示目标
* phase: 表示阶段，目标将会在什么阶段执行
* inherited: 和上面的元素一样，设置false maven将会拒绝执行继承给子插件
* configuration: 表示此执行的配置属性
**插件管理**
    pluginManagement：插件管理以同样的方式包括插件元素，用于在特定的项目中配置。所有继承于此项目的子项目都能使用。主要定义插件的共同元素
**扩展元素集合**
主要包括以下的元素：
**Directories**
用于设置各种目录结构，如下：
 

xml 代码
 

1. <build>  
1.     <sourceDirectory>${basedir}/src/main/java<!---->sourceDirectory>  
1.     <scriptSourceDirectory>${basedir}/src/main/scripts<!---->scriptSourceDirectory>  
1.     <testSourceDirectory>${basedir}/src/test/java<!---->testSourceDirectory>  
1.     <outputDirectory>${basedir}/target/classes<!---->outputDirectory>  
1.     <testOutputDirectory>${basedir}/target/test-classes<!---->testOutputDirectory>  
1.     ...  
1.   <!---->build>  
**Extensions**
表示需要扩展的插件，必须包括进相应的build路径。
xml 代码

 

1. <project>  
1.   <build>  
1.     ...  
1.     <extensions>  
1.       <extension>  
1.         <groupId>org.apache.maven.wagon<!---->groupId>  
1.         <artifactId>wagon-ftp<!---->artifactId>  
1.         <version>1.0-alpha-3<!---->version>  
1.       <!---->extension>  
1.     <!---->extensions>  
1.     ...  
1.   <!---->build>  
1. <!---->project>  
**Reporting**
    用于在site阶段输出报表。特定的maven 插件能输出相应的定制和配置报表。
 

xml 代码
 

1. <reporting>  
1.     <plugins>  
1.       <plugin>  
1.         <outputDirectory>${basedir}/target/site<!---->outputDirectory>  
1.         <artifactId>maven-project-info-reports-plugin<!---->artifactId>  
1.         <reportSets>  
1.           <reportSet><!---->reportSet>  
1.         <!---->reportSets>  
1.       <!---->plugin>  
1.     <!---->plugins>  
1.   <!---->reporting>  
**Report Sets**
    用于配置不同的目标，应用于不同的报表
xml 代码

 

1. <reporting>  
1.     <plugins>  
1.       <plugin>  
1.         ...  
1.         <reportSets>  
1.           <reportSet>  
1.             <id>sunlink<!---->id>  
1.             <reports>  
1.               <report>javadoc<!---->report>  
1.             <!---->reports>  
1.             <inherited>true<!---->inherited>  
1.             <configuration>  
1.               <links>  
1.                 <link>http://java.sun.com/j2se/1.5.0/docs/api/<!---->link>  
1.               <!---->links>  
1.             <!---->configuration>  
1.           <!---->reportSet>  
1.         <!---->reportSets>  
1.       <!---->plugin>  
1.     <!---->plugins>  
1.   <!---->reporting> 

[maven 配置篇 之pom.xml（二)](http://zyl.iteye.com/blog/41761)
**更多的项目信息**

* name:项目除了artifactId外，可以定义多个名称
* description: 项目描述
* url: 项目url
* inceptionYear:创始年份
**Licenses**
xml 代码

 

1. <licenses>  
1.   <license>  
1.     <name>Apache 2<!---->name>  
1.     <url>http://www.apache.org/licenses/LICENSE-2.0.txt<!---->url>  
1.     <distribution>repo<!---->distribution>  
1.     <comments>A business-friendly OSS license<!---->comments>  
1.   <!---->license>  
1. <!---->licenses> 
**Organization**
配置组织信息
 

xml 代码
 

1. <organization>  
1.     <name>Codehaus Mojo<!---->name>  
1.     <url>http://mojo.codehaus.org<!---->url>  
1.   <!---->organization>  
**Developers**
配置开发者信息
xml 代码

 

1. <developers>  
1.     <developer>  
1.       <id>eric<!---->id>  
1.       <name>Eric<!---->name>  
1.       <email>eredmond@codehaus.org<!---->email>  
1.       <url>http://eric.propellors.net<!---->url>  
1.       <organization>Codehaus<!---->organization>  
1.       <organizationUrl>http://mojo.codehaus.org<!---->organizationUrl>  
1.       <roles>  
1.         <role>architect<!---->role>  
1.         <role>developer<!---->role>  
1.       <!---->roles>  
1.       <timezone>-6<!---->timezone>  
1.       <properties>  
1.         <picUrl>http://tinyurl.com/prv4t<!---->picUrl>  
1.       <!---->properties>  
1.     <!---->developer>  
1.   <!---->developers>  
**Contributors**
 

xml 代码
 

1. <contributors>  
1.    <contributor>  
1.      <name>Noelle<!---->name>  
1.      <email>some.name@gmail.com<!---->email>  
1.      <url>http://noellemarie.com<!---->url>  
1.      <organization>Noelle Marie<!---->organization>  
1.      <organizationUrl>http://noellemarie.com<!---->organizationUrl>  
1.      <roles>  
1.        <role>tester<!---->role>  
1.      <!---->roles>  
1.      <timezone>-5<!---->timezone>  
1.      <properties>  
1.        <gtalk>some.name@gmail.com<!---->gtalk>  
1.      <!---->properties>  
1.    <!---->contributor>  
1.  <!---->contributors>  
**环境设置**
**Issue Management**
    定义相关的bug跟踪系统，如bugzilla,testtrack,clearQuest等
 

xml 代码
 

1. <issueManagement>  
1.     <system>Bugzilla<!---->system>  
1.     <url>http://127.0.0.1/bugzilla<!---->url>  
1.   <!---->issueManagement>  
**Continuous Integration Management**
连续整合管理，基于triggers或者timings
 

xml 代码
 

1. <ciManagement>  
1.    <system>continuum<!---->system>  
1.    <url>http://127.0.0.1:8080/continuum<!---->url>  
1.    <notifiers>  
1.      <notifier>  
1.        <type>mail<!---->type>  
1.        <sendOnError>true<!---->sendOnError>  
1.        <sendOnFailure>true<!---->sendOnFailure>  
1.        <sendOnSuccess>false<!---->sendOnSuccess>  
1.        <sendOnWarning>false<!---->sendOnWarning>  
1.        <configuration><address>continuum@127.0.0.1<!---->address><!---->configuration>  
1.      <!---->notifier>  
1.    <!---->notifiers>  
1.  <!---->ciManagement>  
**Mailing Lists**
 

xml 代码
 

1. <mailingLists>  
1.    <mailingList>  
1.      <name>User List<!---->name>  
1.      <subscribe>user-subscribe@127.0.0.1<!---->subscribe>  
1.      <unsubscribe>user-unsubscribe@127.0.0.1<!---->unsubscribe>  
1.      <post>user@127.0.0.1<!---->post>  
1.      <archive>http://127.0.0.1/user/<!---->archive>  
1.      <otherArchives>  
1.        <otherArchive>http://base.google.com/base/1/127.0.0.1<!---->otherArchive>  
1.      <!---->otherArchives>  
1.    <!---->mailingList>  
1.  <!---->mailingLists>  
**SCM**
  软件配置管理，如cvs 和svn
 

xml 代码
 

1. <scm>  
1.     <connection>scm:svn:http://127.0.0.1/svn/my-project<!---->connection>  
1.     <developerConnection>scm:svn:https://127.0.0.1/svn/my-project<!---->developerConnection>  
1.     <tag>HEAD<!---->tag>  
1.     <url>http://127.0.0.1/websvn/my-project<!---->url>  
1.   <!---->scm>  
**Repositories**
配置同setting.xml中的开发库
**Plugin Repositories**
配置同 repositories
**Distribution Management**
用于配置分发管理，配置相应的产品发布信息,主要用于发布，在执行mvn deploy后表示要发布的位置
**1 配置到文件系统**
xml 代码

 

1. <distributionManagement>  
1. <repository>  
1. <id>proficio-repository<!---->id>  
1. <name>Proficio Repository<!---->name>  
1. <url>file://${basedir}/target/deploy<!---->url>  
1. <!---->repository>  
1. <!---->distributionManagement>  
**2 使用ssh2配置**
xml 代码

 

1. <distributionManagement>  
1. <repository>  
1. <id>proficio-repository<!---->id>  
1. <name>Proficio Repository<!---->name>  
1. <url>scp://sshserver.yourcompany.com/deploy<!---->url>  
1. <!---->repository>  
1. <!---->distributionManagement>  
**3 使用sftp配置**
xml 代码

 

1. <distributionManagement>  
1. <repository>  
1. <id>proficio-repository<!---->id>  
1. <name>Proficio Repository<!---->name>  
1. <url>sftp://ftpserver.yourcompany.com/deploy<!---->url>  
1. <!---->repository>  
1. <!---->distributionManagement>  
**4 使用外在的ssh配置**
    编译扩展用于指定使用wagon外在ssh提供，用于提供你的文件到相应的远程服务器。
xml 代码

 

1. <distributionManagement>  
1. <repository>  
1. <id>proficio-repository<!---->id>  
1. <name>Proficio Repository<!---->name>  
1. <url>scpexe://sshserver.yourcompany.com/deploy<!---->url>  
1. <!---->repository>  
1. <!---->distributionManagement>  
1. <build>  
1. <extensions>  
1. <extension>  
1. <groupId>org.apache.maven.wagon<!---->groupId>  
1. <artifactId>wagon-ssh-external<!---->artifactId>  
1. <version>1.0-alpha-6<!---->version>  
1. <!---->extension>  
1. <!---->extensions>  
1. <!---->build>  
**5 使用ftp配置**
xml 代码

 

1. <distributionManagement>  
1. <repository>  
1. <id>proficio-repository<!---->id>  
1. <name>Proficio Repository<!---->name>  
1. <url>ftp://ftpserver.yourcompany.com/deploy<!---->url>  
1. <!---->repository>  
1. <!---->distributionManagement>  
1. <build>  
1. <extensions>  
1. <extension>  
1. <groupId>org.apache.maven.wagon<!---->groupId>  
1. <artifactId>wagon-ftp<!---->artifactId>  
1. <version>1.0-alpha-6<!---->version>  
1. <!---->extension>  
1. <!---->extensions>  
1. <!---->build>  
repository 对应于你的开发库，用户信息通过settings.xml中的server取得
**Profiles**
类似于settings.xml中的profiles，增加了几个元素，如下的样式：
 

xml 代码
 

1. <profiles>  
1.     <profile>  
1.       <id>test<!---->id>  
1.       <activation>...<!---->activation>  
1.       <build>...<!---->build>  
1.       <modules>...<!---->modules>  
1.       <repositories>...<!---->repositories>  
1.       <pluginRepositories>...<!---->pluginRepositories>  
1.       <dependencies>...<!---->dependencies>  
1.       <reporting>...<!---->reporting>  
1.       <dependencyManagement>...<!---->dependencyManagement>  
1.       <distributionManagement>...<!---->distributionManagement>  
1.     <!---->profile>  
1.   <!---->profiles>  
 
