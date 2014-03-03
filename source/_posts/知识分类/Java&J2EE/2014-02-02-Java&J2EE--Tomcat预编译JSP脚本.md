---
layout: post
title: "Tomcat 预编译JSP 脚本"
categories: Java&J2EE
tags: 
 - Java&J2EE
--- 

# Tomcat 预编译JSP 脚本

参考：

The Apache Jakarta Tomcat 5.5 Servlet/JSP Container Jasper 2 JSP Engine How To

[http://jakarta.apache.org/tomcat/tomcat-5.5-doc/jasper-howto.html](http://jakarta.apache.org/tomcat/tomcat-5.5-doc/jasper-howto.html)

jspc

[http://ant.apache.org/manual/OptionalTasks/jspc.html](http://ant.apache.org/manual/OptionalTasks/jspc.html)

用Tomcat进行预编译的ant脚本如下：

build.properties的内容为：

tomcat.home=D:/Tomcat 5.5
webapp.name=blh
webapp.path=D:/Tomcat 5.5/webapps/blh

build.xml的内容为：
1. <?xml version="1.0" encoding="GBK"?>
1. <project name="WebApp Precompilation JSP to Java to Class to Jar" basedir="." default="help">
1.  <property file="build.properties"/>
1.  <target name="all" depends="jsp2java,java2class,class2jar,clear"/>
1.  <target name="help">
1.   <echo message="显示功能列表"/>
1.   <echo message="jsp2java  通过JspC将JSP转换成Java源代码"/>
1.   <echo message="java2class 将转换后的Java源代码进行编译成class文件"/>
1.   <echo message="class2jar 将编译后的class文件打包"/>
1.   <echo message="clear  清理现场"/>
1.  </target>
1.  <target name="jsp2java">
1.   <taskdef classname="org.apache.jasper.JspC" name="jsp2java">
1.    <classpath id="jsp2java.classpath">
1.     <fileset dir="${tomcat.home}/bin">
1.      <include name="*.jar"/>
1.     </fileset>
1.     <fileset dir="${tomcat.home}/server/lib">
1.      <include name="*.jar"/>
1.     </fileset>
1.     <fileset dir="${tomcat.home}/common/lib">
1.      <include name="*.jar"/>
1.     </fileset>
1.    </classpath>
1.   </taskdef>
1.   <!-- 注意JSP文件要设置为UTF-8编码 -->
1.   <jsp2java classpath="jsp2java.classpath" javaEncoding="UTF-8" validateXml="false" uriroot="${webapp.path}" webXmlFragment="${webapp.path}/WEB-INF/webJSP.xml" outputDir="${webapp.path}/WEB-INF/JspC/src"/>
1.  </target>
1.  <target name="java2class">
1.   <mkdir dir="${webapp.path}/WEB-INF/JspC/classes"/>
1.   <!-- 同样Java文件要设置为UTF-8编码 -->
1.   <javac srcdir="${webapp.path}/WEB-INF/JspC/src" destdir="${webapp.path}/WEB-INF/JspC/classes" encoding="UTF-8" optimize="off" debug="on" failonerror="false" excludes="**/*.smap">
1.    <classpath id="java2class.classpath">
1.     <pathelement location="${webapp.path}/WEB-INF/classes"/>
1.     <fileset dir="${webapp.path}/WEB-INF/lib">
1.      <include name="*.jar"/>
1.     </fileset>
1.     <pathelement location="${tomcat.home}/common/classes"/>
1.     <fileset dir="${tomcat.home}/common/lib">
1.      <include name="*.jar"/>
1.     </fileset>
1.     <pathelement location="${tomcat.home}/shared/classes"/>
1.     <fileset dir="${tomcat.home}/shared/lib">
1.      <include name="*.jar"/>
1.     </fileset>
1.     <fileset dir="${tomcat.home}/bin">
1.      <include name="*.jar"/>
1.     </fileset>
1.    </classpath>
1.    <include name="**"/>
1.    <exclude name="tags/**" />
1.   </javac>
1.  </target>
1.  <target name="class2jar">
1.   <mkdir dir="${webapp.path}/WEB-INF/lib"/>
1.   <jar basedir="${webapp.path}/WEB-INF/JspC/classes" jarfile="${webapp.path}/WEB-INF/lib/${webapp.name}JSP.jar"/>
1.  </target>
1.  <target name="clear">
1.   <delete dir="${webapp.path}/WEB-INF/JspC/classes"/>
1.   <delete dir="${webapp.path}/WEB-INF/JspC/src"/>
1.   <delete dir="${webapp.path}/WEB-INF/JspC"/>
1.  </target>
1. </project>
****

只需要设置好Ant的path环境变量，然后修改build.properties。执行ant all命令即可。
生成好的jar文件是{$webappname}JSP.jar。
在做为产品发布的时候，只需要你的类jar包和JSP预编译的包放到WEB-INF/lib/目录下即可，如${webappname}.jar和JSP预编译的包${webappname}JSP.jar；
然后删除掉你的所有的预编过的JSP源文件；
并且${webapp.path}/WEB-INF/webJSP.xml里的servlet映射，添加到${webapp.path}/WEB-INF/web.xml中。

来源： <[http://blog.csdn.net/terry_f/article/details/3725382](http://blog.csdn.net/terry_f/article/details/3725382)> 
