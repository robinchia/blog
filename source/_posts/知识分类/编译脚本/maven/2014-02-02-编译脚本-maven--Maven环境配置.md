---
layout: post
title: "Maven环境配置"
categories: 编译脚本
tags: 
 - 编译脚本
 - maven
--- 

# Maven环境配置

Maven 是基于[Java](http://www.yiibai.com/java) 的工具，所以第一要求是JDK有安装在你的机器上。

## 系统要求

JDK1.5 or above.Memoryno minimum requirement.Disk Spaceno minimum requirement.Operating Systemno minimum requirement.

## Step 1 - 验证Java已经在你的机器上安装

现在，打开控制台，然后执行下面的java命令。
OSTaskCommandWindowsOpen Command Consolec:\> java -versionLinuxOpen Command Terminal$ java -versionMacOpen Terminalmachine:~ joseph$ java -version

让我们来验证所有操作系统的输出：

OSOutputWindowsjava version "1.6.0_21" 
Java(TM) SE Runtime Environment (build 1.6.0_21-b07)
Java HotSpot(TM) Client VM (build 17.0-b17, mixed mode, sharing)Linuxjava version "1.6.0_21" 
Java(TM) SE Runtime Environment (build 1.6.0_21-b07)
Java HotSpot(TM) Client VM (build 17.0-b17, mixed mode, sharing)Macjava version "1.6.0_21" 
Java(TM) SE Runtime Environment (build 1.6.0_21-b07)
Java HotSpot(TM)64-Bit Server VM (build 17.0-b17, mixed mode, sharing)

如果没有安装Java，安装Java软件开发工具包（SDK）[http://www.oracle.com/technetwork/java/javase/downloads/index.htmll](http://www.oracle.com/technetwork/java/javase/downloads/index.htmll)。我们假设在本教程中安装的版本Java1.6.0_21。

## Step 2: 设置Java环境

设置JAVA_HOME环境变量指向的基目录位置在您的机器上已安装Java。例如：
OSOutputWindowsSet the environment variable JAVA_HOME to C:\Program Files\Java\jdk1.6.0_21Linuxexport JAVA_HOME=/usr/local/java-currentMacexport JAVA_HOME=/Library/Java/Home

添加到系统路径中的Java编译器的位置。

OSOutputWindowsAppend the string ;C:\Program Files\Java\jdk1.6.0_21\bin to the end of the system variable, Path.Linuxexport PATH=$PATH:$JAVA_HOME/bin/Macnot required

验证Java安装使用java-version命令解释上述。

## Step 3: 下载Maven档案

从[http://maven.apache.org/download.htmll ](http://maven.apache.org/download.htmll)下载Maven2.2.1
OSArchive nameWindowsapache-maven-2.0.11-bin.zipLinuxapache-maven-2.0.11-bin.tar.gzMacapache-maven-2.0.11-bin.tar.gz

## Step 4: 提取Maven的存档

解压压缩包，希望安装Maven2.2.1的目录。从归档文件中将创建的子目录中的Apache Maven2.2.1。
OSLocation (can be different based on your installation)WindowsC:\Program Files\Apache Software Foundation\apache-maven-2.2.1Linux/usr/local/apache-mavenMac/usr/local/apache-maven

## Step 5: 设置Maven的环境变量

添加 M2_HOME，M2，MAVEN_OPTS 环境变量。
OSOutputWindowsSet the environment variables using system properties. 

M2_HOME=C:\Program Files\Apache Software Foundation\apache-maven-2.2.1

M2=%M2_HOME%\bin

MAVEN_OPTS=-Xms256m -Xmx512mLinuxOpen command terminal and set environment variables.

export M2_HOME=/usr/local/apache-maven/apache-maven-2.2.1

export M2=%M2_HOME%\bin

export MAVEN_OPTS=-Xms256m -Xmx512mMacOpen command terminal and set environment variables.

export M2_HOME=/usr/local/apache-maven/apache-maven-2.2.1

export M2=%M2_HOME%\bin

export MAVEN_OPTS=-Xms256m -Xmx512m

## Step 6: Maven bin目录位置添加到系统路径

现在，附加M2变量系统路径
OSOutputWindowsAppend the string ;%M2% to the end of the system variable, Path.Linuxexport PATH=$M2:$PATHMacexport PATH=$M2:$PATH

## Step 8: 验证Maven安装

现在打开控制台，执行以下mvn命令
OSTaskCommandWindowsOpen Command Consolec:\> mvn --versionLinuxOpen Command Terminal$ mvn --versionMacOpen Terminalmachine:~ joseph$ mvn --version

最后，验证上述的命令的输出，这应该是如下的内容：

OSOutputWindowsApache Maven 2.2.1 (r801777; 2009-08-07 00:46:01+0530)
Java version: 1.6.0_21 
Java home: C:\Program Files\Java\jdk1.6.0_21\jreLinuxApache Maven 2.2.1 (r801777; 2009-08-07 00:46:01+0530)
Java version: 1.6.0_21 
Java home: C:\Program Files\Java\jdk1.6.0_21\jreMacApache Maven 2.2.1 (r801777; 2009-08-07 00:46:01+0530) 
Java version: 1.6.0_21 
Java home: C:\Program Files\Java\jdk1.6.0_21\jre
来源： <[http://www.yiibai.com/maven/maven_environment_setup.html](http://www.yiibai.com/maven/maven_environment_setup.html)> 
