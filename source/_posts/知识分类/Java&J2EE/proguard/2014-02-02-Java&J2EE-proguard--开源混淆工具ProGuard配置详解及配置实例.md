---
layout: post
title: "开源混淆工具ProGuard配置详解及配置实例"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - proguard
--- 

# 开源混淆工具ProGuard配置详解及配置实例

开源混淆工具ProGuard配置详解及配置实例

ProGuard是一个免费的java类文件压缩,优化,混淆器.它探测并删除没有使用的类,字段,方法和属性.它删除没有用的说明并使用字节码得到最大优化.它使用无意义的名字来重命名类,字段和方法.

ProGuard的作用: 
 
1.创建紧凑的代码文档是为了更快的网络传输,快速装载和更小的内存占用. 
2.创建的程序和程序库很难使用反向工程. 
3.所以它能删除来自源文件中的没有调用的代码 
4.充分利用java6的快速加载的优点来提前检测和返回java6中存在的类文件. 
 
**参数： **
 
-include {filename}    从给定的文件中读取配置参数 
-basedirectory {directoryname}    指定基础目录为以后相对的档案名称 
-injars {class_path}    指定要处理的应用程序jar,war,ear和目录 
-outjars {class_path}    指定处理完后要输出的jar,war,ear和目录的名称 
-libraryjars {classpath}    指定要处理的应用程序jar,war,ear和目录所需要的程序库文件 
-dontskipnonpubliclibraryclasses    指定不去忽略非公共的库类。 
-dontskipnonpubliclibraryclassmembers    指定不去忽略包可见的库类的成员。 
**保留选项 **
-keep {Modifier} {class_specification}    保护指定的类文件和类的成员 
-keepclassmembers {modifier} {class_specification}    保护指定类的成员，如果此类受到保护他们会保护的更好 
-keepclasseswithmembers {class_specification}    保护指定的类和类的成员，但条件是所有指定的类和类成员是要存在。 
-keepnames {class_specification}    保护指定的类和类的成员的名称（如果他们不会压缩步骤中删除） 
-keepclassmembernames {class_specification}    保护指定的类的成员的名称（如果他们不会压缩步骤中删除） 
-keepclasseswithmembernames {class_specification}    保护指定的类和类的成员的名称，如果所有指定的类成员出席（在压缩步骤之后） 
-printseeds {filename}    列出类和类的成员-keep选项的清单，标准输出到给定的文件 
 
**压缩 **
-dontshrink    不压缩输入的类文件 
-printusage {filename} 
-whyareyoukeeping {class_specification}     
 
**优化 **
-dontoptimize    不优化输入的类文件 
-assumenosideeffects {class_specification}    优化时假设指定的方法，没有任何副作用 
-allowaccessmodification    优化时允许访问并修改有修饰符的类和类的成员 
 
**混淆 **
-dontobfuscate    不混淆输入的类文件 
-printmapping {filename} 
-applymapping {filename}    重用映射增加混淆 
-obfuscationdictionary {filename}    使用给定文件中的关键字作为要混淆方法的名称 
-overloadaggressively    混淆时应用侵入式重载 
-useuniqueclassmembernames    确定统一的混淆类的成员名称来增加混淆 
-flattenpackagehierarchy {package_name}    重新包装所有重命名的包并放在给定的单一包中 
-repackageclass {package_name}    重新包装所有重命名的类文件中放在给定的单一包中 
-dontusemixedcaseclassnames    混淆时不会产生形形色色的类名 
-keepattributes {attribute_name,...}    保护给定的可选属性，例如LineNumberTable, LocalVariableTable, SourceFile, Deprecated, Synthetic, Signature, and InnerClasses. 
-renamesourcefileattribute {string}    设置源文件中给定的字符串常量
**Ant Example:**
<!-- This Ant build file illustrates how to process applications,
     by including ProGuard-style configuration options.
     Usage: ant -f applications2.xml -->
<project name="Applications" default="obfuscate" basedir="../..">
<target name="obfuscate">
  <taskdef resource="proguard/ant/task.properties"
           classpath="lib/proguard.jar" />
  <proguard>
    <!-- Specify the input jars, output jars, and library jars. -->
    -injars  in.jar
    -outjars out.jar
    -libraryjars ${java.home}/lib/rt.jar
    <!-- -libraryjars junit.jar    -->
    <!-- -libraryjars servlet.jar  -->
    <!-- -libraryjars jai_core.jar -->
    <!-- ...                       -->
    <!-- Save the obfuscation mapping to a file, and preserve line numbers. -->
    -printmapping out.map
    -renamesourcefileattribute SourceFile
    -keepattributes SourceFile,LineNumberTable
    <!-- Preserve all annotations. -->
    -keepattributes *Annotation*
    <!-- Preserve all public applications. -->
    -keepclasseswithmembers public class * {
        public static void main(java.lang.String[]);
    }
    <!-- Preserve all native method names and the names of their classes. -->
    -keepclasseswithmembernames class * {
        native &lt;methods&gt;;
    }
    <!-- Preserve the methods that are required in all enumeration classes. -->
    -keepclassmembers class * extends java.lang.Enum {
        public static **[] values();
        public static ** valueOf(java.lang.String);
    }
    <!-- Explicitly preserve all serialization members. The Serializable
         interface is only a marker interface, so it wouldn't save them.
         You can comment this out if your library doesn't use serialization.
         If your code contains serializable classes that have to be backward
         compatible, please refer to the manual. -->
    -keepclassmembers class * implements java.io.Serializable {
        static final long serialVersionUID;
        static final java.io.ObjectStreamField[] serialPersistentFields;
        private void writeObject(java.io.ObjectOutputStream);
        private void readObject(java.io.ObjectInputStream);
        java.lang.Object writeReplace();
        java.lang.Object readResolve();
    }
    <!-- Your application may contain more items that need to be preserved;
         typically classes that are dynamically created using Class.forName -->
  </proguard>
</target>
</project>
来源： <[http://www.kaiyuanba.cn/html/1/131/138/7820.htm](http://www.kaiyuanba.cn/html/1/131/138/7820.htm)> 
