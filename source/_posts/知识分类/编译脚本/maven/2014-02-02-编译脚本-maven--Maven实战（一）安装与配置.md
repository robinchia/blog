---
layout: post
title: "Maven实战（一）安装与配置"
categories: 编译脚本
tags: 
 - 编译脚本
 - maven
--- 

# Maven实战（一）安装与配置

**1. 简介** 
  Maven是基于项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具. 
如果你已经有十次输入同样的Ant targets来编译你的代码、jar或者war、生成javadocs，你一定会自问，是否有一个重复性更少却能同样完成该工作的方 法。 Maven便提供了这样一种选择，将你的注意力从作业层转移到项目管理层。Maven项目已经能够知道如何构建和捆绑代码，运行测试，生成文档并宿主项目网页 
**2.核心价值** 
   *** 简单** 
      Maven 暴露了一组一致、简介的操作接口，能帮助团队成员从原来的高度自定义的、复杂的构建系统中解脱出来，使用Maven现有的成熟的、稳定的组件也能简   化构建系统的复杂度。 
   *** 交流与反馈** 
      与版本控制系统结合后，多有人都能执行最新的构建并快速得到反馈。此外，自动生成的项目报告也能帮助成员了解项目的状态，促进团队的交流。 
   *** 测试驱动开发** 
**      **TDD强调测试先行，所有产品都应该由测试用例覆盖。而测试是maven生命周期的最重要组成部分之一，并且Maven有现成的成熟插件支持业界流行的测试框架，如Junit和TestNG。
   *** 快速构建**
    只需要一些配置，之后用一条简单的命令就能让Maven帮你清理、编译、测试、打包、部署，然后得到最终产品。[/size] 
   *** 持续集成** 
**      **更加方便的持续集成 
   *** 富有信息的工作区** 
**2.主要内容** 
   我将会发表一系列课程来讲解Maven的应用，基于Maven3.0，主要内容如下： 
   1）安装和配置 
   2）Maven使用入门 
   3）坐标和依赖 
   4）Maven仓库 
   5)  生命周期和插件 
   6）聚合与继承 
   7）使用Nexus创建私服 
   8）使用Maven进行测试 
   9）m2eclipse的使用 
   10）自动部署maven项目 
   11）使用Hudson进行持续集成 
**3. 安装好JDK **
    以JDK1.5以上为例 
**4. Maven 的下载
**   下载地址：[http://maven.apache.org/download.html](http://maven.apache.org/download.html) 
**5.Maven安装** 
   将下载到的文件解压到指定目录即可，如：C:\maven\apache-maven-3.0.4 
**6.环境变量的配置
**
    在系统环境变量中新增如下环境变量 
    M2_HOME:  Maven的安装目录，如：C:\maven\apache-maven-3.0.4 
    M2:  %M2_HOME%\bin 
    并在path中添加%M2%，这样便可以在任何路径中执行mvn命令
**7. 检测安装是否成功 
**
    Cmd窗口执行命令：mvn –v 
    得到如下图所示结果： 
![]()
 
   

 8**.设置代理**

**  **有时候你所在的公司基于安全因素考虑，要求你使用通过安全认证的代理访问因特网。这时就需要为Maven配置HTTP代理。

   在目录~/.m2/setting.xml文件中编辑如下（如果没有该文件，则复制$M2_HOME/conf/setting.xml）：
Xml代码  [![收藏代码]()]( "收藏这段代码")

1. <proxies>  
1.     <proxy>  
1.       <id>optional</id>  
1.       <active>true</active>  
1.       <protocol>http</protocol>  
1.       <username>proxyuser</username>  
1.       <password>proxypass</password>  
1.       <host>proxy.host.net</host>  
1.       <port>80</port>  
1.       <nonProxyHosts>local.net|some.host.com</nonProxyHosts>  
1.     </proxy>      
1.  </proxies>  

 

来源： <[http://tangyanbo.iteye.com/blog/1502578](http://tangyanbo.iteye.com/blog/1502578)> 
