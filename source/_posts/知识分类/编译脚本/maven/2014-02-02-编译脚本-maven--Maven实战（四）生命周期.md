---
layout: post
title: "Maven实战（四）生命周期"
categories: 编译脚本
tags: 
 - 编译脚本
 - maven
--- 

# Maven实战（四）生命周期

**1. 三套生命周期** 
    Maven拥有三套相互独立的生命周期，它们分别为clean，default和site。 
每个生命周期包含一些阶段，这些阶段是有顺序的，并且后面的阶段依赖于前面的阶段，用户和Maven最直接的交互方式就是调用这些生命周期阶段。 
以clean生命周期为例，它包含的阶段有pre-clean, clean 和 post clean。当用户调用pre-clean的时候，只有pre-clean得以执行，当用户调用clean的时候，pre-clean和clean阶段会得以顺序执行；当用户调用post-clean的时候，pre-clean,clean,post-clean会得以顺序执行。 
较之于生命周期阶段的前后依赖关系，三套生命周期本身是相互独立的，用户可以仅仅调用clean生命周期的某个阶段，或者仅仅调用default生命周期的某个阶段，而不会对其他生命周期产生任何影响。 
**2. clean 生命周期**

      clean生命周期的目的是清理项目，它包含三个阶段：

     1）**pre-clean** 执行一些清理前需要完成的工作。

     2）**clean** 清理上一次构建生成的文件。

     3）**post-clean** 执行一些清理后需要完成的工作。

 

**3. default 生命周期**

       default生命周期定义了真正构件时所需要执行的所有步骤，它是生命周期中最核心的部分，它包含的阶段如下：

       1） validate 验证项目是否正确和所有需要的相关资源是否可用

       2） initialize 初始化构建

       3） generate-sources

       4)   process-sources 处理源代码

       5） generate-resources 

       6)   process-resources 处理项目主资源文件。对src/main/resources目录的内容进行变量替换等工作后，复制到项目输出的主classpath目录中。

       7） compile 编译项目的主源代码

       8） process-classes

       9)   generate-test-sources

       10) process-test-sources 处理项目测试资源文件

       11）generate-test-resources

       12)  process-test-resources 处理测试的资源文件

       13）test-compile 编译项目的测试代码

       14）process-test-classes

       15)  test 使用单元测试框架运行测试，测试代码不会被打包或部署

       16）prepare-package 做好打包的准备

       17）package 接受编译好的代码，打包成可发布的格式

       18)  pre-integration-test

       19)  integration-test

       20)  post integration-test

       21)  verify

       22)  install 将包安装到Maven本地仓库，供本地其他Maven项目使用

       23）deploy 将最终的包复制到远程仓库，供其他开发人员和Maven项目使用

       

 

**4. site 生命周期**

      site生命周期的目的是建立和发布项目站点，Maven能够基于POM所包含的信息，自动生成一个友好的站点，方便团队交流和发布项目信息。该生命周期包含如下阶段：

      1）pre-site 执行一些在生成项目站点之前需要完成的工作

      2）site 生成项目站点文档

      3）post-site 执行一些在生成项目站点之后需要完成的工作

      4）site-deploy 将生成的项目站点发布到服务器上
来源： <[http://tangyanbo.iteye.com/blog/1503890](http://tangyanbo.iteye.com/blog/1503890)> 
