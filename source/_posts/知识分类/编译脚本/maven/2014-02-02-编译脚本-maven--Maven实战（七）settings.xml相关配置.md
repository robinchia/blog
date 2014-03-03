---
layout: post
title: "Maven实战（七）settings.xml相关配置"
categories: 编译脚本
tags: 
 - 编译脚本
 - maven
--- 

# Maven实战（七）settings.xml相关配置

**一、简介**

settings.xml对于maven来说相当于全局性的配置，用于所有的项目，当Maven运行过程中的各种配置，例如pom.xml，不想绑定到一个固定的project或者要分配给用户时，我们使用settings.xml中的settings元素来确定这些配置。这包含了本地仓库位置，远程仓库服务器以及认证信息等。

 

settings.xml存在于两个地方：

1.安装的地方：$M2_HOME/conf/settings.xml

2.用户的目录：${user.home}/.m2/settings.xml

 

前者又被叫做全局配置，后者被称为用户配置。如果两者都存在，它们的内容将被合并，并且用户范围的配置优先。

平时配置时优先选择用户目录的settings.xml

下面是settings下的顶层元素的一个概览：

 
1

2
3

4
5

6
7

8
9

10
11

12
13

14
15<

settings
 

xmlns

=

"http://maven.apache.org/SETTINGS/1.0.0"

         

xmlns:xsi

=

"http://www.w3.org/2001/XMLSchema-instance"
         

xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0

            

http://maven.apache.org/xsd/settings-1.0.0.xsd">
    

<

localRepository

/>

    

<

interactiveMode

/>
    

<

usePluginRegistry

/>

    

<

offline

/>
    

<

pluginGroups

/>

    

<

servers

/>
    

<

mirrors

/>

    

<

proxies

/>
    

<

profiles

/>

    

<

activeProfiles

/>
</

settings

>

 

**二、简单值**

localRepository：这个值是构建系统的本地仓库的路径。默认的值是${user.home}/.m2/repository.如果一个系统想让所有登陆的用户都用同一个本地仓库的话，这个值是极其有用的。

interactiveMode：如果Maven要试图与用户交互来得到输入就设置为true，否则就设置为false，默认为true。

usePluginRegistry：如果Maven使用${user.home}/.m2/plugin-registry.xml来管理plugin的版本，就设置为true，默认为false。

offline：如果构建系统要在离线模式下工作，设置为true，默认为false。如果构建服务器因为网络故障或者安全问题不能与远程仓库相连，那么这个设置是非常有用的。

 

**三、PluginGroups（插件组）**

这个元素包含了一系列pluginGroup元素，每个又包含了一个groupId。当一个plugin被使用，而它的groupId没有被提供的时候，这个列表将被搜索。这个列表自动的包含了org.apache.maven.plugins和org.codehaus.mojo。

 
1

2
3

4
5

6
7

8
9

10
<

settings
 

xmlns

=

"http://maven.apache.org/SETTINGS/1.0.0"

          

xmlns:xsi

=

"http://www.w3.org/2001/XMLSchema-instance"
          

xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0

            

http://maven.apache.org/xsd/settings-1.0.0.xsd">
    

...

    

<

pluginGroups

>
        

<

pluginGroup

>org.mortbay.jetty</

pluginGroup

>

    

</

pluginGroups

>
    

...

</

settings

>

 

**四、Servers（服务器）**

 1. 定义jar包下载的Maven仓库

   2. 定义部署服务器

 

 
1

2
3

4
5

6
7

8
9

10
11

12
<

servers

>

    

<

server

>
        

<

id

>tomcat</

id

>

        

<

username

>bruce</

username

>
        

<

password

>password</

password

>

    

</

server

>
    

<

server

>

        

<

id

>shiyue</

id

>
        

<

username

>admin</

username

>

        

<

password

>password</

password

>
    

</

server

>

  

</

servers

>

tomcat: 部署服务器

shiyue: Mave私服

 

**五、Mirrors（镜像）**

指定仓库的地址，则默认从指定的镜像下载jar包及插件
1

2
3

4
5

6
7

8
9<

mirrors

>

                                                                                                
 
     

<

mirror

>

      

<

id

>mirrorId</

id

>
      

<

mirrorOf

>*</

mirrorOf

>

      

<

name

>Human Readable Name for this Mirror.</

name

>
      

<

url

>http://host:port/nexus-2.1.2/content/groups/public</

url

>

    

</

mirror

>
  

</

mirrors

>

 

**六、Proxies（代理）**

有时候你所在的公司基于安全因素考虑，要求你使用通过安全认证的代理访问因特网。这时就需要为Maven配置HTTP代理。

 
1

2
3

4
5

6
7

8
9

10
11

12
<

proxies

>

    

<

proxy

>
      

<

id

>optional</

id

>

      

<

active

>true</

active

>
      

<

protocol

>http</

protocol

>

      

<

username

>proxyuser</

username

>
      

<

password

>proxypass</

password

>

      

<

host

>proxy.host.net</

host

>
      

<

port

>80</

port

>

      

<

nonProxyHosts

>local.net|some.host.com</

nonProxyHosts

>
    

</

proxy

>

 

</

proxies

>

参考：[http://maven.apache.org/settings.html](http://maven.apache.org/settings.html)

来源： <[http://tangyanbo.iteye.com/blog/1971257](http://tangyanbo.iteye.com/blog/1971257)> 
