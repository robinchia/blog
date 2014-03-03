---
layout: post
title: "Maven实战（五）坐标详解"
categories: 编译脚本
tags: 
 - 编译脚本
 - maven
--- 

# Maven实战（五）坐标详解

1.为什么要定义Maven坐标 
     在我们开发Maven项目的时候，需要为其定义适当的坐标，这是Maven强制要求的。在这个基础上，其他Maven项目才能应用该项目生成的构件。 
2.Maven坐标详解

     Maven坐标为各种构件引入了秩序，任何一个构件都必须明确定义自己的坐标，而一组Maven坐标是通过一些元素定义的，它们是groupId,artifactId,version,packaging,class-sifer。下面是一组坐标定义：

  
Xml代码  [![收藏代码]()]( "收藏这段代码")

1. <groupId>com.mycompany.app</groupId>  
1.   <artifactId>my-app</artifactId>  
1.   <packaging>jar</packaging>  
1.  <version>0.0.1-SNAPSHOT</version>  

 下面讲解一下各个坐标元素：

 

**groupId** ：定义当前Maven项目隶属的实际项目。首先，Maven项目和实际项目不一定是一对一的关系。比如SpringFrameWork这一实际项目，其对应的Maven项目会有很多，如spring-core,spring-context等。这是由于Maven中模块的概念，因此，一个实际项目往往会被划分成很多模块。其次，groupId不应该对应项目隶属的组织或公司。原因很简单，一个组织下会有很多实际项目，如果groupId只定义到组织级别，而后面我们会看到，artifactId只能定义Maven项目（模块），那么实际项目这个层次将难以定义。最后，groupId的表示方式与Java包名的表达方式类似，通常与域名反向一一对应。

 

**artifactId **: 该元素定义当前实际项目中的一个Maven项目（模块），推荐的做法是使用实际项目名称作为artifactId的前缀。比如上例中的my-app。

 

**version** : 该元素定义Maven项目当前的版本

 

**packaging** ：定义Maven项目打包的方式，首先，打包方式通常与所生成构件的文件扩展名对应，如上例中的packaging为jar,最终的文件名为my-app-0.0.1-SNAPSHOT.jar。也可以打包成war, ear等。当不定义packaging的时候，Maven 会使用默认值jar

 

**classifier**: 该元素用来帮助定义构建输出的一些附件。附属构件与主构件对应，如上例中的主构件为my-app-0.0.1-SNAPSHOT.jar,该项目可能还会通过一些插件生成如my-app-0.0.1-SNAPSHOT-javadoc.jar,my-app-0.0.1-SNAPSHOT-sources.jar, 这样附属构件也就拥有了自己唯一的坐标
来源： <[http://tangyanbo.iteye.com/blog/1503946](http://tangyanbo.iteye.com/blog/1503946)> 
