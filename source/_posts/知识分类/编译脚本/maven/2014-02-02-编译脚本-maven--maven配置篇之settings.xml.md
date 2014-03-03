---
layout: post
title: "maven 配置篇 之 settings.xml"
categories: 编译脚本
tags: 
 - 编译脚本
 - maven
--- 

# maven 配置篇 之 settings.xml

    maven2 比起maven1 来说，需要配置的文件少多了，主要集中在pom.xml和settings.xml中。
    先来说说settings.xml，settings.xml对于maven来说相当于全局性的配置，用于所有的项目。在maven2中存在两个 settings.xml，一个位于maven2的安装目录conf下面，作为全局性配置。对于团队设置，保持一致的定义是关键，所以 maven2/conf下面的settings.xml就作为团队共同的配置文件。保证所有的团队成员都拥有相同的配置。当然对于每个成员，都需要特殊的 自定义设置，如用户信息，所以另外一个settings.xml就作为本地配置。默认的位置为：${user.dir} /.m2/settings.xml目录中（${user.dir} 指windows 中的用户目录）。
    settings.xml基本结构如下：
   

xml 代码
 

1. <settings xmlns="http://maven.apache.org/POM/4.0.0"  
1.           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
1.           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0  
1.                                http://maven.apache.org/xsd/settings-1.0.0.xsd">  
1.   <localRepository/>  
1.   <interactiveMode/>  
1.   <usePluginRegistry/>  
1.   <offline/>  
1.   <pluginGroups/>  
1.   <servers/>  
1.   <mirrors/>  
1.   <proxies/>  
1.   <profiles/>  
1.   <activeProfiles/>  
1. </settings>  
简单介绍一下几个主要的配置因素：
**localRepository：**表示本地库的保存位置，也就是maven2主要的jar保存位置，默认在${user.dir}/.m2/repository，如果需要另外设置，就换成其他的路径。
**offline：**如果不想每次编译，都去查找远程中心库，那就设置为true。当然前提是你已经下载了必须的依赖包。
**Servers**
   在POM中的 distributionManagement元素定义了开发库。然而，特定的username和pwd不能使用于pom.xml，所以通过此配置来保存server信息
 

xml 代码
 

1. <servers>  
1.    <server>  
1.      <id>server001</id>  
1.      <username>my_login</username>  
1.      <password>my_password</password>  
1.      <privateKey>${usr.home}/.ssh/id_dsa</privateKey>  
1.      <passphrase>some_passphrase</passphrase>  
1.      <filePermissions>664</filePermissions>  
1.      <directoryPermissions>775</directoryPermissions>  
1.      <configuration></configuration>  
1.    </server>  
1.  </servers>   

* id:server 的id,用于匹配distributionManagement库id，比较重要。
* username, password:用于登陆此服务器的用户名和密码
* privateKey, passphrase：设置private key，以及passphrase
* filePermissions, directoryPermissions：当库文件或者目录创建后，需要使用权限进行访问。参照unix文件许可，如664和775
**Mirrors**
表示镜像库，指定库的镜像，用于增加其他库
 

xml 代码
 

1. <mirrors>  
1.    <mirror>  
1.      <id>planetmirror.com</id>  
1.      <name>PlanetMirror Australia</name>  
1.      <url>http://downloads.planetmirror.com/pub/maven2</url>  
1.      <mirrorOf>central</mirrorOf>  
1.    </mirror>  
1.  </mirrors>  

* id,name:唯一的标志，用于区别镜像
* url:镜像的url
* mirrorOf：此镜像指向的服务id
**Proxies**
此设置，主要用于无法直接访问中心的库用户配置。
 

xml 代码
 

1. <proxies>  
1.    <proxy>  
1.      <id>myproxy</id>  
1.      <active>true</active>  
1.      <protocol>http</protocol>  
1.      <host>proxy.somewhere.com</host>  
1.      <port>8080</port>  
1.      <username>proxyuser</username>  
1.      <password>somepassword</password>  
1.      <nonProxyHosts>*.google.com|ibiblio.org</nonProxyHosts>  
1.    </proxy>  
1.  </proxies>  

* id:代理的标志
* active：是否激活代理
* protocol, host, port:protocol://host:port 代理
* username, password：用户名和密码
* nonProxyHosts: 不需要代理的host
**Profiles**
  类似于pom.xml中的profile元素，主要包括activation,repositories,pluginRepositories 和properties元素
  刚开始接触的时候，可能会比较迷惑，其实这是maven2中比较强大的功能。从字面上来说，就是个性配置。
  单独定义profile后，并不会生效，需要通过满足条件来激活。
 **repositories 和pluginRepositories**
 定义其他开发库和插件开发库。对于团队来说，肯定有自己的开发库。可以通过此配置来定义。
 如下的配置，定义了本地开发库，用于release 发布。
   

xml 代码
 

1. <repositories>  
1.         <repository>  
1.           <id>repo-local</id>  
1.        <name>Internal 开发库</name>  
1.        <url>http://192.168.0.2:8082/repo-local</url>  
1.           <releases>  
1.             <enabled>true</enabled>  
1.             <updatePolicy>never</updatePolicy>  
1.             <checksumPolicy>warn</checksumPolicy>  
1.           </releases>  
1.           <snapshots>  
1.             <enabled>false</enabled>  
1.           </snapshots>  
1.           <layout>default</layout>  
1.         </repository>  
1.       </repositories>  
1.       <pluginRepositories>  
1.     <pluginRepository>  
1.     <id>repo-local</id>  
1.     <name>Internal 开发库</name>  
1.     <url>http://192.168.0.2:8082/repo-local</url>  
1.     <releases>  
1.             <enabled>true</enabled>  
1.             <updatePolicy>never</updatePolicy>  
1.             <checksumPolicy>warn</checksumPolicy>  
1.     </releases>  
1.     <snapshots>  
1.     <enabled>false</enabled>  
1.     </snapshots>  
1.     <layout>default</layout>  
1.     </pluginRepository>  
1.     </pluginRepositories>  
releases, snapshots:每个产品的版本的Release或者snapshot(注：release和snapshot的区别，release一般是比较稳定的版本，而snapshot基本上不稳定，只是作为快照）
**properties**
  maven 的properties作为placeholder值，如ant的properties。
包括以下的5种类型值：

1. env.X，返回当前的环境变量
1. project.x:返回pom中定义的元素值，如project.version
1. settings.x：返回settings.xml中定义的元素
1. java 系统属性：所有经过java.lang.System.getProperties()返回的值
1. x：用户自己设定的值
**Activation**
  用于激活此profile
 

xml 代码
 

1. <activation>  
1.         <activeByDefault>false</activeByDefault>  
1.         <jdk>1.5</jdk>  
1.         <os>  
1.           <name>Windows XP</name>  
1.           <family>Windows</family>  
1.           <arch>x86</arch>  
1.           <version>5.1.2600</version>  
1.         </os>  
1.         <property>  
1.           <name>mavenVersion</name>  
1.           <value>2.0.3</value>  
1.         </property>  
1.         <file>  
1.           <exists>${basedir}/file2.properties</exists>  
1.           <missing>${basedir}/file1.properties</missing>  
1.         </file>  
1.       </activation>  

* jdk:如果匹配指定的jdk版本，将会激活
* os:操作系统
* property：如果maven能检测到相应的属性
* file: 用于判断文件是否存在或者不存在
除了使用activation来激活profile，同样可以通过activeProfiles来激活
**Active Profiles**
表示激活的profile,通过profile id来指定。
 

xml 代码
 

1. <activeProfiles>  
1.     <activeProfile>env-test</activeProfile> 指定的profile id  
1.   </activeProfiles>  
