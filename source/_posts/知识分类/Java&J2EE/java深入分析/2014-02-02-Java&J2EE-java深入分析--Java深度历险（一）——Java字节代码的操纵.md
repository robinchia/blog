---
layout: post
title: "Java深度历险（一）——Java字节代码的操纵"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - java深入分析
--- 

# Java深度历险（一）——Java字节代码的操纵

编者按】Java作为业界应用最为广泛的语言之一，深得众多软件厂商和开发者的推崇，更是被包括Oracle在内的众多JCP成员积极地推动发展。但是对于Java语言的深度理解和运用，毕竟是很少会有人涉及的话题。InfoQ中文站特地邀请IBM高级工程师成富为大家撰写这个《Java深度历险》专栏，旨在就Java的一些深度和高级特性分享他的经验。

在一般的Java应用开发过程中，开发人员使用Java的方式比较简单。打开惯用的IDE，编写Java源代码，再利用IDE提供的功能直接运行Java 程序就可以了。这种开发模式背后的过程是：开发人员编写的是Java源代码文件（.java），IDE会负责调用Java的编译器把Java源代码编译成平台无关的字节代码（byte code），以类文件的形式保存在磁盘上（.class）。Java虚拟机（JVM）会负责把Java字节代码加载并执行。Java通过这种方式来实现其“[编写一次，到处运行（Write once, run anywhere）](http://en.wikipedia.org/wiki/Write_once,_run_anywhere)” 的目标。Java类文件中包含的字节代码可以被不同平台上的JVM所使用。Java字节代码不仅可以以文件形式存在于磁盘上，也可以通过网络方式来下载，还可以只存在于内存中。JVM中的类加载器会负责从包含字节代码的字节数组（byte[]）中定义出Java类。在某些情况下，可能会需要动态的生成 Java字节代码，或是对已有的Java字节代码进行修改。这个时候就需要用到本文中将要介绍的相关技术。首先介绍一下如何动态编译Java源文件。

## 动态编译Java源文件

在一般情况下，开发人员都是在程序运行之前就编写完成了全部的Java源代码并且成功编译。对有些应用来说，Java源代码的内容在运行时刻才能确定。这个时候就需要动态编译源代码来生成Java字节代码，再由JVM来加载执行。典型的场景是很多算法竞赛的在线评测系统（如[PKU JudgeOnline](http://poj.org/)），允许用户上传Java代码，由系统在后台编译、运行并进行判定。在动态编译Java源文件时，使用的做法是直接在程序中调用Java编译器。

[JSR 199](http://www.jcp.org/en/jsr/detail?id=199)引入了Java编译器API。如果使用JDK 6的话，可以通过此API来动态编译Java代码。比如下面的代码用来动态编译最简单的Hello World类。该Java类的代码是保存在一个字符串中的。
public class CompilerTest {

   public static void main(String[] args) throws Exception {     
      String source = "public class Main { public static void main(String[] args) {System.out.println(\"Hello World!\");} }";

      JavaCompiler compiler = ToolProvider.getSystemJavaCompiler();
      StandardJavaFileManager fileManager = compiler.getStandardFileManager(null, null, null);

      StringSourceJavaObject sourceObject = new CompilerTest.StringSourceJavaObject("Main", source);
      Iterable< extends JavaFileObject> fileObjects = Arrays.asList(sourceObject);

      CompilationTask task = compiler.getTask(null, fileManager, null, null, null, fileObjects);
      boolean result = task.call();

      if (result) {
         System.out.println("编译成功。");

      }
   }

 
   static class StringSourceJavaObject extends SimpleJavaFileObject {

 
      private String content = null;

      public StringSourceJavaObject(String name, String content) ??throws URISyntaxException {
         super(URI.create("string:///" + name.replace('.','/') + Kind.SOURCE.extension), Kind.SOURCE);

         this.content = content;
      }

 
      public CharSequence getCharContent(boolean ignoreEncodingErrors) ??throws IOException {

         return content;
      }

   }
}

如果不能使用JDK 6提供的Java编译器API的话，可以使用JDK中的工具类[com.sun.tools.javac.Main](http://download.oracle.com/javase/1.5.0/docs/tooldocs/solaris/javac.html)，不过该工具类只能编译存放在磁盘上的文件，类似于直接使用javac命令。
 

另外一个可用的工具是[Eclipse JDT Core](http://www.eclipse.org/jdt/core/)提供的编译器。这是Eclipse Java开发环境使用的增量式Java编译器，支持运行和调试有错误的代码。该编译器也可以单独使用。[Play框架](http://www.playframework.org/)在内部使用了JDT的编译器来动态编译Java源代码。在开发模式下，Play框架会定期扫描项目中的Java源代码文件，一旦发现有修改，会自动编译 Java源代码。因此在修改代码之后，刷新页面就可以看到变化。使用这些动态编译的方式的时候，需要确保JDK中的tools.jar在应用的 CLASSPATH中。

下面介绍一个例子，是关于如何在Java里面做四则运算，比如求出来(3+4)*7-10的值。一般的做法是分析输入的运算表达式，自己来模拟计算过程。考虑到括号的存在和运算符的优先级等问题，这样的计算过程会比较复杂，而且容易出错。另外一种做法是可以用[JSR 223](http://jcp.org/en/jsr/detail?id=223)引入的脚本语言支持，直接把输入的表达式当做JavaScript或是JavaFX脚本来执行，得到结果。下面的代码使用的做法是动态生成Java源代码并编译，接着加载Java类来执行并获取结果。这种做法完全使用Java来实现。
private static double calculate(String expr) throws CalculationException  {

   String className = "CalculatorMain";
   String methodName = "calculate";

   String source = "public class " + className
      + " { public static double " + methodName + "() { return " + expr + "; } }";

      //省略动态编译Java源代码的相关代码，参见上一节
   boolean result = task.call();

   if (result) {
      ClassLoader loader = Calculator.class.getClassLoader();

      try {           
         Class<?> clazz = loader.loadClass(className);

         Method method = clazz.getMethod(methodName, new Class<?>[] {});
         Object value = method.invoke(null, new Object[] {});

         return (Double) value;
      } catch (Exception e) {

         throw new CalculationException("内部错误。");       
      }   

   } else {
      throw new CalculationException("错误的表达式。");   

   }
}

上面的代码给出了使用动态生成的Java字节代码的基本模式，即通过类加载器来加载字节代码，创建Java类的对象的实例，再通过Java反射API来调用对象中的方法。

## Java字节代码增强

Java 字节代码增强指的是在Java字节代码生成之后，对其进行修改，增强其功能。这种做法相当于对应用程序的二进制文件进行修改。在很多Java框架中都可以见到这种实现方式。Java字节代码增强通常与Java源文件中的注解（annotation）一块使用。注解在Java源代码中声明了需要增强的行为及相关的元数据，由框架在运行时刻完成对字节代码的增强。Java字节代码增强应用的场景比较多，一般都集中在减少冗余代码和对开发人员屏蔽底层的实现细节上。用过[JavaBeans](http://download.oracle.com/javase/tutorial/javabeans/index.html)的人可能对其中那些必须添加的getter/setter方法感到很繁琐，并且难以维护。而通过字节代码增强，开发人员只需要声明Bean中的属性即可，getter/setter方法可以通过修改字节代码来自动添加。用过[JPA](http://www.oracle.com/technetwork/articles/javaee/jpa-137156.html)的人，在调试程序的时候，会发现[实体类](http://download.oracle.com/javaee/5/tutorial/doc/bnbqa.html)中被添加了一些额外的 域和方法。这些域和方法是在运行时刻由JPA的实现动态添加的。字节代码增强在[面向方面编程（AOP）](http://en.wikipedia.org/wiki/Aspect-oriented_programming)的一些实现中也有使用。

在讨论如何进行字节代码增强之前，首先介绍一下表示一个Java类或接口的字节代码的组织形式。
类文件 {

   0xCAFEBABE，小版本号，大版本号，常量池大小，常量池数组，
   访问控制标记，当前类信息，父类信息，实现的接口个数，实现的接口信息数组，域个数，

   域信息数组，方法个数，方法信息数组，属性个数，属性信息数组
}

如上所示，一个类或接口的字节代码使用的是一种松散的组织结构，其中所包含的内容依次排列。对于可能包含多个条目的内容，如所实现的接口、域、方法和属性等，是以数组来表示的。而在数组之前的是该数组中条目的个数。不同的内容类型，有其不同的内部结构。对于开发人员来说，直接操纵包含字节代码的字节数组的话，开发效率比较低，而且容易出错。已经有不少的开源库可以对字节代码进行修改或是从头开始创建新的Java类的字节代码内容。这些类库包括[ASM](http://asm.ow2.org/)、[cglib](http://cglib.sourceforge.net/)、[serp](http://serp.sourceforge.net/)和[BCEL](http://jakarta.apache.org/bcel/)等。使用这些类库可以在一定程度上降低增强字节代码的复杂度。比如考虑下面一个简单的需求，在一个Java类的所有方法执行之前输出相应的日志。熟悉AOP的人都知道，可以用一个前增强（before advice）来解决这个问题。如果使用ASM的话，相关的代码如下：

ClassReader cr = new ClassReader(is);

ClassNode cn = new ClassNode();
cr.accept(cn, 0);

for (Object object : cn.methods) {   
   MethodNode mn = (MethodNode) object;  

   if ("<init>".equals(mn.name) || "<clinit>".equals(mn.name)) {       
      continue;   

   }   
   InsnList insns = mn.instructions;   

   InsnList il = new InsnList();  
   il.add(new FieldInsnNode(GETSTATIC, "java/lang/System", "out", "Ljava/io/PrintStream;"));   

   il.add(new LdcInsnNode("Enter method -> " + mn.name));  
   il.add(new MethodInsnNode(INVOKEVIRTUAL, "java/io/PrintStream", "println", "(Ljava/lang/String;)V"));   

   insns.insert(il);  mn.maxStack += 3;
}

ClassWriter cw = new ClassWriter(0);
cn.accept(cw);

byte[] b = cw.toByteArray();

从[ClassWriter](http://asm.ow2.org/asm33/javadoc/user/org/objectweb/asm/ClassWriter.html)就可以获取到包含增强之后的字节代码的字节数组，可以把字节代码写回磁盘或是由类加载器直接使用。上述示例中，增强部分的逻辑比较简单，只是遍历Java类中的所有方法并添加对System.out.println方法的调用。在字节代码中，Java方法体是由一系列的指令组成的。而要做的是生成调用System.out.println方法的指令，并把这些指令插入到指令集合的最前面。ASM对这些指令做了抽象，不过熟悉全部的指令比较困难。ASM提供了一个工具类[ASMifierClassVisitor](http://asm.ow2.org/asm33/javadoc/user/org/objectweb/asm/util/ASMifierClassVisitor.html)，可以打印出Java类的字节代码的结构信息。当需要增强某个类的时候，可以先在源代码上做出修改，再通过此工具类来比较修改前后的字节代码的差异，从而确定该如何编写增强的代码。

对类文件进行增强的时机是需要在Java源代码编译之后，在JVM执行之前。比较常见的做法有：

* 由IDE在完成编译操作之后执行。如Google App Engine的Eclipse插件会在编译之后运行[DataNucleus](http://www.datanucleus.org/)来对实体类进行增强。
* 在构建过程中完成，比如通过Ant或Maven来执行相关的操作。
* 实现自己的Java类加载器。当获取到Java类的字节代码之后，先进行增强处理，再从修改过的字节代码中定义出Java类。
* 通过JDK 5引入的java.lang.instrument包来完成。

## java.lang.instrument

由于存在着大量对Java字节代码进行修改的需求，[JDK 5](http://download.oracle.com/javase/1.5.0/docs/api/java/lang/instrument/package-summary.html)引入了java.lang.instrument包并在[JDK 6](http://download.oracle.com/javase/6/docs/api/java/lang/instrument/package-summary.html)中得到了进一步的增强。基本的思路是在JVM启动的时候添加一些代理（agent）。每个代理是一个jar包，其清单（manifest）文件中会指定一个代理类。这个类会包含一个premain方法。JVM在启动的时候会首先执行代理类的premain方法，再执行Java程序本身的main方法。在 premain方法中就可以对程序本身的字节代码进行修改。JDK 6中还允许在JVM启动之后动态添加代理。java.lang.instrument包支持两种修改的场景，一种是重定义一个Java类，即完全替换一个 Java类的字节代码；另外一种是转换已有的Java类，相当于前面提到的类字节代码增强。还是以前面提到的输出方法执行日志的场景为例，首先需要实现[java.lang.instrument.ClassFileTransformer](http://download.oracle.com/javase/6/docs/api/java/lang/instrument/ClassFileTransformer.html)接口来完成对已有Java类的转换。
static class MethodEntryTransformer implements ClassFileTransformer {

   public byte[] transform(ClassLoader loader, String className,
     Class<?> classBeingRedefined, ?ProtectionDomain protectionDomain, byte[] classfileBuffer)

     throws  IllegalClassFormatException {
        try {

           ClassReader cr = new ClassReader(classfileBuffer);
           ClassNode cn = new ClassNode();           

           //省略使用ASM进行字节代码转换的代码           
           ClassWriter cw = new ClassWriter(0);

           cn.accept(cw);
           return cw.toByteArray();      

        } catch (Exception e){           
           return null;

        }
   }

}

有了这个转换类之后，就可以在代理的premain方法中使用它。

public static void premain(String args, Instrumentation inst) {   

   inst.addTransformer(new MethodEntryTransformer());
}

把该代理类打成一个jar包，并在jar包的清单文件中通过Premain-Class声明代理类的名称。运行Java程序的时候，添加JVM启动参数-javaagent:myagent.jar。这样的话，JVM会在加载Java类的字节代码之前，完成相关的转换操作。

## 总结

操纵Java字节代码是一件很有趣的事情。通过它，可以很容易的对二进制分发的Java程序进行修改，非常适合于性能分析、调试跟踪和日志记录等任务。另外一个非常重要的作用是把开发人员从繁琐的Java语法中解放出来。开发人员应该只需要负责编写与业务逻辑相关的重要代码。对于那些只是因为语法要求而添加的，或是模式固定的代码，完全可以将其字节代码动态生成出来。字节代码增强和源代码生成是不同的概念。源代码生成之后，就已经成为了程序的一部分，开发人员需要去维护它：要么手工修改生成出来的源代码，要么重新生成。而字节代码的增强过程，对于开发人员是完全透明的。妥善使用Java字节代码的操纵技术，可以更好的解决某一类开发问题。

## 参考资料

* [Java字节代码格式](http://java.sun.com/docs/books/jvms/second_edition/html/ClassFile.doc.html)
* [Java 6.0 Compiler API](http://www.javabeat.net/articles/73-the-java-60-compiler-api-1.html)
* [深入探讨Java类加载器](http://www.ibm.com/developerworks/cn/java/j-lo-classloader/index.html)

感谢[张凯峰](http://www.infoq.com/cn/bycategory.action?authorName=%E5%BC%A0%E5%87%AF%E5%B3%B0)对本文的审校。

来源： <[http://www.infoq.com/cn/articles/cf-java-byte-code](http://www.infoq.com/cn/articles/cf-java-byte-code)>
 
