---
layout: post
title: "5个必须掌握的maven命令"
categories: 编译脚本
tags: 
 - 编译脚本
 - maven
--- 

# 5个必须掌握的maven命令

1. mvn help:describe 你是否因为记不清某个插件有哪些goal而痛苦过,你是否因为想不起某个goal有哪些参数而苦恼,那就试试这个命令吧,它会告诉你一切的. 参数: 1. -Dplugin=pluginName 2. -Dgoal(或-Dmojo)=goalName:与-Dplugin一起使用,它会列出某个插件的goal信息,如果嫌不够详细,同样可以加 -Ddetail.(注:一个插件goal也被认为是一个 “Mojo”) 下面大家就运行mvn help:describe -Dplugin=help -Dmojo=describe感受一下吧!

2. mvn archetype:generate 你是怎么创建你的maven项目的?是不是像这样:mvn archetype:create -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.ryanote -Dartifact=common,如果你还再用的话,那你就out了,现代人都用mvn archetype:generate了,它将创建项目这件枯燥的事更加人性化,你再也不需要记那么多的archetypeArtifactId,你只需输入archetype:generate,剩下的就是做”选择题”了.

3. mvn tomcat:run 用了maven后,你再也不需要用eclipse里的tomcat来运行web项目(实际工作中经常会发现用它会出现不同步更新的情况),只需在对应目录 (如/ryanote)里运行 mvn tomat:run命令,然后就可在浏览器里运行http://localhost:8080/ryanote查看了.如果你想要更多的定制,可以在 pom.xml文件里加下面配置: 01 02 03 04 org.codehaus.mojo 05 tomcat-maven-plugin 06 07 /web 08 9090 09 10 11 12 当然你也可以在命令里加参数来实现特定的功能,下面几个比较常用: 1. 跳过测试:-Dmaven.test.skip(=true) 2. 指定端口:-Dmaven.tomcat.port=9090 3. 忽略测试失败:-Dmaven.test.failure.ignore=true 当然,如果你的其它关联项目有过更新的话,一定要在项目根目录下运行mvn clean install来执行更新,再运行mvn tomcat:run使改动生效.

4. mvnDebug tomcat:run 这条命令主要用来远程测试,它会监听远程测试用的8000端口,在eclipse里打开远程测试后,它就会跑起来了,设断点,调试,一切都是这么简单.上面提到的那几个参数在这里同样适用.

5. mvn dependency:sources 故名思义,有了它,你就不用到处找源码了,运行一下,你项目里所依赖的jar包的源码就都有了
来源： <[http://www.cnblogs.com/MyFavorite/archive/2012/03/18/2404330.html](http://www.cnblogs.com/MyFavorite/archive/2012/03/18/2404330.html)> 
