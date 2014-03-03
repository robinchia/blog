---
layout: post
title: "Maven 构建生命周期"
categories: 编译脚本
tags: 
 - 编译脚本
 - maven
--- 

# Maven 构建生命周期

## 构建生命周期是什么？

构建生命周期阶段的目标是要执行的顺序是一个良好定义的序列。在此阶段的生命周期中的一个阶段。
作为一个例子，一个典型的[Maven](http://www.yiibai.com/maven) 构建生命周期是由下列顺序的阶段：
PhaseHandles描述prepare-resourcesresource copyingResource copying can be customized in this phase.compilecompilationSource code compilation is done in this phase.packagepackagingThis phase creates the JAR / WAR package as mentioned in packaging in POM.xml.installinstallationThis phase installs the package in local / remote maven repository.

总是有可用于注册必须执行一个特定的阶段之前或之后的目标的前处理和后阶段。
当Maven开始建立一个项目，它通过定义序列的阶段步骤和执行注册的每个阶段的目标。 Maven的有以下三种标准的生命周期：

* 
clean
* 
default(or build)
* 
site

目标代表一个特定的任务，这有助于项目的建设和管理。它可以被绑定到零个或多个生成阶段。一个没有绑定到任何构建阶段的目标的构建生命周期可以执行直接调用。
执行的顺序取决于目标和构建阶段被调用的顺序。例如，考虑下面的命令。清洁和包装参数的构建阶段，而依赖：复制的依赖关系是一个目标。
mvn clean dependency:copy-dependencies package

在这里，清洁的阶段，将首先执行，然后的依赖关系：复制依赖性的目标将被执行，并终于将执行包阶段。

## 清洁生命周期

当我们执行命令mvn clean命令后，Maven的调用清洁的生命周期由以下几个阶段组成的。

* 
pre-clean
* 
clean
* 
post-clean

Maven 清洁目标（clean:clean）被绑定的清洁干净的生命周期阶段。clean:clean 目标删除删除build目录下的输出构建。因此，当mvn clean 命令执行时，Maven会删除编译目录。

提到清洁的生命周期在上述阶段的目标，我们可以自定义此行为。
在下面的示例中，我们将附加 maven-antrun-plugin:run目标的预清洁，清洁和清洁后的阶段。这将使我们能够调用的信息显示的清洁生命周期的各个阶段。
我们已经创建了一个pom.xml在 C:\ MVN\ 项目文件夹中。
<project xmlns="http://maven.apache.org/POM/4.0.0"

   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0

   http://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>

<groupId>com.companyname.projectgroup</groupId>
<artifactId>project</artifactId>

<version>1.0</version>
<build>

<plugins>
   <plugin>

   <groupId>org.apache.maven.plugins</groupId>
   <artifactId>maven-antrun-plugin</artifactId>

   <version>1.1</version>
   <executions>

      <execution>
         <id>id.pre-clean</id>

         <phase>pre-clean</phase>
         <goals>

            <goal>run</goal>
         </goals>

         <configuration>
            <tasks>

               <echo>pre-clean phase</echo>
            </tasks>

         </configuration>
      </execution>

      <execution>
         <id>id.clean</id>

         <phase>clean</phase>
         <goals>

          <goal>run</goal>
         </goals>

         <configuration>
            <tasks>

               <echo>clean phase</echo>
            </tasks>

         </configuration>
      </execution>

      <execution>
         <id>id.post-clean</id>

         <phase>post-clean</phase>
         <goals>

            <goal>run</goal>
         </goals>

         <configuration>
            <tasks>

               <echo>post-clean phase</echo>
            </tasks>

         </configuration>
      </execution>

   </executions>
   </plugin>

</plugins>
</build>

</project>

现在，打开命令控制台，到该文件夹包含的pom.xml和执行以下mvncommand。

C:\MVN\project>mvn post-clean

Maven将开始处理和显示清洁生命周期的所有阶段

[INFO] Scanning for projects...

[INFO] ------------------------------------------------------------------
[INFO] Building Unnamed - com.companyname.projectgroup:project:jar:1.0

[INFO]    task-segment: [post-clean]
[INFO] ------------------------------------------------------------------

[INFO] [antrun:run {execution: id.pre-clean}]
[INFO] Executing tasks

     [echo] pre-clean phase
[INFO] Executed tasks

[INFO] [clean:clean {execution: default-clean}]
[INFO] [antrun:run {execution: id.clean}]

[INFO] Executing tasks
     [echo] clean phase

[INFO] Executed tasks
[INFO] [antrun:run {execution: id.post-clean}]

[INFO] Executing tasks
     [echo] post-clean phase

[INFO] Executed tasks
[INFO] ------------------------------------------------------------------

[INFO] BUILD SUCCESSFUL
[INFO] ------------------------------------------------------------------

[INFO] Total time: < 1 second
[INFO] Finished at: Sat Jul 07 13:38:59 IST 2012

[INFO] Final Memory: 4M/44M
[INFO] ------------------------------------------------------------------

你可以尝试调整mvn清洁的命令，该命令将显示前干净清洁，什么都不会被执行为清洁后阶段。

## **默认（或生成）的生命周期**

这是主要的Maven的生命周期，用于构建应用程序。它有以下23个阶段。
Lifecycle Phase描述validateValidates whether project is correct and all necessary information is available to complete the build process.initializeInitializes build state, for example set propertiesgenerate-sourcesGenerate any source code to be included in compilation phase.process-sourcesProcess the source code, for example, filter any value.generate-resourcesGenerate resources to be included in the package.process-resourcesCopy and process the resources into the destination directory, ready for packaging phase.compileCompile the source code of the project.process-classesPost-process the generated files from compilation, for example to do bytecode enhancement/optimization on Java classes.generate-test-sourcesGenerate any test source code to be included in compilation phase.process-test-sourcesProcess the test source code, for example, filter any values.test-compileCompile the test source code into the test destination directory.process-test-classesProcess the generated files from test code file compilation.testRun tests using a suitable unit testing framework(Junit is one).prepare-packagePerform any operations necessary to prepare a package before the actual packaging.packageTake the compiled code and package it in its distributable format, such as a JAR, WAR, or EAR file.pre-integration-testPerform actions required before integration tests are executed. For example, setting up the required environment.integration-testProcess and deploy the package if necessary into an environment where integration tests can be run.pre-integration-testPerform actions required after integration tests have been executed. For example, cleaning up the environment.verifyRun any check-ups to verify the package is valid and meets quality criterias.installInstall the package into the local repository, which can be used as a dependency in other projects locally.deployCopies the final package to the remote repository for sharing with other developers and projects.

There are few important concepts related to Maven Lifecycles which are wroth to mention:

* 
When a phase is called via Maven command, for example mvn compile, only phases upto and including that phase will execute.
* 
Different maven goals will be bound to different phases of Maven lifecycle depending upon the type of packaging (JAR / WAR / EAR).

在下面的示例中，我们将附加的Maven的antrun插件：运行目标构建生命周期的几个阶段。这将使我们能够回显的短信显示的生命周期的各个阶段。
我们已经更新了pom.xml文件在C：\MVN\项目文件夹中。
<project xmlns="http://maven.apache.org/POM/4.0.0"

  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0

  http://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>

<groupId>com.companyname.projectgroup</groupId>
<artifactId>project</artifactId>

<version>1.0</version>
<build>

<plugins>
<plugin>

<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-antrun-plugin</artifactId>

<version>1.1</version>
<executions>

   <execution>
      <id>id.validate</id>

      <phase>validate</phase>
      <goals>

         <goal>run</goal>       </goals>
      <configuration>

         <tasks>
            <echo>validate phase</echo>

         </tasks>
      </configuration>

   </execution>
   <execution>

      <id>id.compile</id>
      <phase>compile</phase>

      <goals>
         <goal>run</goal>

      </goals>
      <configuration>

         <tasks>
            <echo>compile phase</echo>

         </tasks>
      </configuration>

   </execution>
   <execution>

      <id>id.test</id>
      <phase>test</phase>

      <goals>
         <goal>run</goal>

      </goals>
      <configuration>

         <tasks>
            <echo>test phase</echo>

         </tasks>
      </configuration>

   </execution>
   <execution>

         <id>id.package</id>
         <phase>package</phase>

         <goals>
            <goal>run</goal>

         </goals>
         <configuration>

         <tasks>
            <echo>package phase</echo>

         </tasks>
      </configuration>

   </execution>
   <execution>

      <id>id.deploy</id>
      <phase>deploy</phase>

      <goals>
         <goal>run</goal>

      </goals>
      <configuration>

      <tasks>
         <echo>deploy phase</echo>

      </tasks>
      </configuration>

   </execution>
</executions>

</plugin>
</plugins>

</build>
</project>

现在，打开命令控制台，进入该文件夹包含pom.xml和执行以下mvn命令。

C:\MVN\project>mvn compile

最多编译阶段，Maven将开始构建生命周期阶段的处理和显示。

[INFO] Scanning for projects...

[INFO] ------------------------------------------------------------------
[INFO] Building Unnamed - com.companyname.projectgroup:project:jar:1.0

[INFO]    task-segment: [compile]
[INFO] ------------------------------------------------------------------

[INFO] [antrun:run {execution: id.validate}]
[INFO] Executing tasks

     [echo] validate phase
[INFO] Executed tasks

[INFO] [resources:resources {execution: default-resources}]
[WARNING] Using platform encoding (Cp1252 actually) to copy filtered resources,

i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory C:\MVN\project\src\main\resources

[INFO] [compiler:compile {execution: default-compile}]
[INFO] Nothing to compile - all classes are up to date

[INFO] [antrun:run {execution: id.compile}]
[INFO] Executing tasks

     [echo] compile phase
[INFO] Executed tasks

[INFO] ------------------------------------------------------------------
[INFO] BUILD SUCCESSFUL

[INFO] ------------------------------------------------------------------
[INFO] Total time: 2 seconds

[INFO] Finished at: Sat Jul 07 20:18:25 IST 2012
[INFO] Final Memory: 7M/64M

[INFO] ------------------------------------------------------------------

## **网站的生命周期**

Maven的网站插件通常用于创建新的文档，创建报告，部署网站等。
阶段

 

* 
pre-site
* 
site
* 
post-site
* 
site-deploy

在下面的示例中，我们将附加maven-antrun-plugin:run目标网站的生命周期的所有阶段。这将使我们能够呼应的短信显示的生命周期的各个阶段。
我们已经更新了pom.xml文件在C：\ MVN\项目文件夹中。
<project xmlns="http://maven.apache.org/POM/4.0.0"

  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0

  http://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>

<groupId>com.companyname.projectgroup</groupId>
<artifactId>project</artifactId>

<version>1.0</version>
<build>

<plugins>
<plugin>

<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-antrun-plugin</artifactId>

<version>1.1</version>
   <executions>

      <execution>
         <id>id.pre-site</id>

         <phase>pre-site</phase>
         <goals>

            <goal>run</goal>
         </goals>

         <configuration>
            <tasks>

               <echo>pre-site phase</echo>
            </tasks>

         </configuration>
      </execution>

      <execution>
         <id>id.site</id>

         <phase>site</phase>
         <goals>

         <goal>run</goal>
         </goals>

         <configuration><tasks>
               <echo>site phase</echo>

            </tasks>
         </configuration>

      </execution>
      <execution>

         <id>id.post-site</id>
         <phase>post-site</phase>

         <goals>
            <goal>run</goal>

         </goals>
         <configuration>

            <tasks>
               <echo>post-site phase</echo>

            </tasks>
         </configuration>

      </execution>
      <execution>

         <id>id.site-deploy</id>
         <phase>site-deploy</phase>

         <goals>
            <goal>run</goal>

         </goals>
         <configuration>

            <tasks>
               <echo>site-deploy phase</echo>

            </tasks>
         </configuration>

      </execution>
   </executions>

</plugin>
</plugins>

</build>
</project>

现在，打开命令控制台，进入该文件夹包含pom.xml和执行以下mvn命令。

C:\MVN\project>mvn site

Maven将开始处理和显示最多的网站网站的生命周期阶段的阶段。

[INFO] Scanning for projects...

[INFO] ------------------------------------------------------------------
[INFO] Building Unnamed - com.companyname.projectgroup:project:jar:1.0

[INFO]    task-segment: [site]
[INFO] ------------------------------------------------------------------

[INFO] [antrun:run {execution: id.pre-site}]
[INFO] Executing tasks

     [echo] pre-site phase
[INFO] Executed tasks

[INFO] [site:site {execution: default-site}]
[INFO] Generating "About" report.

[INFO] Generating "Issue Tracking" report.
[INFO] Generating "Project Team" report.

[INFO] Generating "Dependencies" report.
[INFO] Generating "Project Plugins" report.

[INFO] Generating "Continuous Integration" report.
[INFO] Generating "Source Repository" report.

[INFO] Generating "Project License" report.
[INFO] Generating "Mailing Lists" report.

[INFO] Generating "Plugin Management" report.
[INFO] Generating "Project Summary" report.

[INFO] [antrun:run {execution: id.site}]
[INFO] Executing tasks

     [echo] site phase
[INFO] Executed tasks

[INFO] ------------------------------------------------------------------
[INFO] BUILD SUCCESSFUL

[INFO] ------------------------------------------------------------------
[INFO] Total time: 3 seconds

[INFO] Finished at: Sat Jul 07 15:25:10 IST 2012
[INFO] Final Memory: 24M/149M

[INFO] -
来源： <[http://www.yiibai.com/maven/maven_build_life_cycle.html](http://www.yiibai.com/maven/maven_build_life_cycle.html)> 
