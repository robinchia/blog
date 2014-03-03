---
layout: post
title: "Java字节码工具ASM在Web Service开发中的应用"
categories: Java&J2EE
tags: 
 - Java&J2EE
--- 

# Java字节码工具ASM在Web Service开发中的应用

在基于 JAX-WS 标准的 web services 的开发中，不少实际场景都是希望采用自底向上的开发方式， 即基于已有的 Java bean 来创建 web services 。WebSphere Application Server ( 以下简称 WAS ) 提供了命令行的工具 wsgen 和相对应的 Ant task 来支持这种开发过程，而且这两个工具比较适合大型项目的自动化构建。 这两个工具的使用前提是 Java bean 中事先添加有 web services 的 Annotation 标注，而在现有的业务系统中，class 文件一般是不带 Annotation 的，这就需要开发人员去修改现有的代码，以手工方式添加 Annotation，但这样带来的工作量太大且容易出错。本文介绍了一种解决途径，可以不用修改源代码，而是利用字节码工具 ASM 直接修改 class 文件， 在字节码文件中自动注入 Annotation ，然后再利用 wsgen 工具就可以很方便地生成 web services 应用。 本文同时也总结了使用 ASM 的一些实践。

本文首先介绍了和 ASM 使用相关的四种文件，以及它们之间的相互转化。然后结合 web services 开发实例，介绍了使用 wsgen 开发时遇到的实际问题，如何写一个 ASM 的适配器去修改现有 class，最终如何产生 web services 文件。

ASM 是一个多用途的 Java 字节码操控和分析框架。它能以二进制的形式直接修改现有的类或动态生成的类。 ASM 提供了常用的转换和分析算法，是一种允许用户方便的组装定制化的复杂转换和代码分析工具。 ASM 也提供和其它的字节码工具类似的功能，但 ASM 的重点是使用的简单和高性能。由于 ASM 在设计实现的时候尽可能 的小的内存占用和提供更高的性能；因此在动态系统中使用 ASM 是很有优势的。ASM 作为一种轻量级、高性能的 Java 字节码操控和分析框架，设计了一种更有效的方法、提供更好的性能和内存占用。今天， ASM 在许多领域都有应用，并已成为事实上的字节码处理框架标准。

[问题的引入]()

先写一个不带 web services 标注的 SayHelloImpl 类。
[**清单 1. 不带 web services 的 SayHelloImpl 类**]()1

public
 

class
 

SayHelloImpl {

2

 

public
 

String sayHello(String s) {
3

   

return
 

“Hello: ” + s ;

4

 

}
5

}

再用 wsgen 来创建 web services，wsgen 的使用如下：
[**清单 2. wsgen 实例**]()1

wsgen.exe – 

cp
 

bin – s output – d output – r output – wsdl ibm.was.asm.SayHelloImpl我们将会遇到如下错误： 
[**清单 3. 根据不带 web services 标注的类创建 web services 时遇到的错误信息**]() 
注释处理过程中遇到问题；

......
com.sun.tools.internal.ws.processor.modeler.ModelerException: [failed to localiz

e] A web service endpoint could not be found()
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.o

nError(WebServiceAP.java:215)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.b

uildModel(WebServiceAP.java:322)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.p

rocess(WebServiceAP.java:256)
        at com.sun.mirror.apt.AnnotationProcessors$CompositeAnnotationProcessor.

process(AnnotationProcessors.java:60)

注意：我们通常是在 class 前使用 @WebService， 在方法前使用 @WebMethod 来将一个类标注为 web services 的。WAS 6.1 的 web services 功能部件包和 WAS 7 都支持基于 JAX-WS 标准 的 web services 的开发， 在 WAS 安装目录的 bin 目录下提供了命令行工具 wsgen，wsgen 支持自底向上的开发方式。 下面是 wsgen 的用法：
[**清单 4. wsgen 命令格式**]() 
注 wsgen [options]  [SEI]

主要选项：

* -d 指定生成的 class 文件位置；
* -s 指定生成的 Java 源文件位置；
* -r 指定生成的 resources 文件位置，如 wsdl，xsd；
* -cp 指定服务实现类所在的位置；
* -wsdl，-servicename，-portname 三个参数指定生成的 wsdl 文件中的 service 和 port 的名称。

SEI(Service Endpoint Interface) 是一个 endpoint implementation class，不能是一个 interface。所以首先要开发一个 endpoint 的实现类，如本例中的 SayHelloImpl 。用 @WebService 声明 web services ，然后将它编译，才能提供给 wsgen 来创建 web services 。

[四种文件之间的转换]()

下面介绍本文要用到的四个概念（Java Source，Java Class，ASM Code，ASM Source）

* Java Source: 即我们通常编写的 Java 源文件；
* Java Class: Java 源文件编译后的字节码文件；
* ASM Source : 类似于对 class 反编译后的源文件，也就是 "textual byte code"，但比原始的 Java Source 可读性要差， 参见清单 5。
* ASM Code: 指读写 class 文件的 ASM 程序代码。需要用到 ASM 提供的 API， 参见清单 6。
[**清单 5. SayHelloImpl 类对应的 ASM Source**]()01

// class version 50.0 (50)

02

// access flags 33
03

public
 

class
 

com/ibm/was/asm/SayHelloImpl {

04
 
05

 

// access flags 1

06

 

public
  

<init>()V
07

   

ALOAD 

0

08

   

INVOKESPECIAL java/lang/Object. <init> ()V
09

   

RETURN

10

   

MAXSTACK = 

1
11

   

MAXLOCALS = 

1

12
 
13

 

// access flags 1

14

 

public
 

sayHello(Ljava/lang/String;)Ljava/lang/String;
15

   

NEW java/lang/StringBuilder

16

   

DUP
17

   

LDC 

"Hello:"

18

   

INVOKESPECIAL java/lang/StringBuilder. <init> (Ljava/lang/String;)V
19

   

ALOAD 

1

20

   

INVOKEVIRTUAL java/lang/StringBuilder.append (Ljava/lang/String;)
21

   

Ljava/lang/StringBuilder;

22

   

INVOKEVIRTUAL java/lang/StringBuilder.toString ()Ljava/lang/String;
23

   

ARETURN

24

   

MAXSTACK = 

3
25

   

MAXLOCALS = 

2

26

}[**清单 6. 生成 SayHelloImpl.class 的 ASM Code**]()[view source](http://www.oschina.net/question/129540_28430#viewSource "view source")[print](http://www.oschina.net/question/129540_28430#printSource "print")[?](http://www.oschina.net/question/129540_28430#about "?")

01

package
 

asm.com.ibm.was.asm;

02

import
 

java.util.*;
03

import
 

org.objectweb.asm.*;

04

import
 

org.objectweb.asm.attrs.*;
05

public
 

class
 

SayHelloImplDump 

implements
 

Opcodes {

06

 

public
 

static
 

byte

[] dump () 

throws
 

Exception {
07

 

ClassWriter cw = 

new
 

ClassWriter(

0

);

08

 

FieldVisitor fv;
09

 

MethodVisitor mv;

10

 

AnnotationVisitor av0;
11
 

12

 

cw.visit(V1_6, ACC_PUBLIC + ACC_SUPER, 

"com/ibm/was/asm/SayHelloImpl"

,
13

 

null

, 

"java/lang/Object"

, 

null

);

14
 
15

 

{

16

 

mv = cw.visitMethod(ACC_PUBLIC, 

"<init>"

, 

"()V"

, 

null

, 

null

);
17

 

mv.visitCode();

18

 

mv.visitVarInsn(ALOAD, 

0

);
19

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/Object"

, 

"<init>"

, 

"()V"

);

20

 

mv.visitInsn(RETURN);
21

 

mv.visitMaxs(

1

, 

1

);

22

 

mv.visitEnd();
23

 

}

24

 

{
25

 

mv = cw.visitMethod(ACC_PUBLIC, 

"sayHello"

,

26

  

"(Ljava/lang/String;)Ljava/lang/String;"

, 

null

, 

null

);
27

 

mv.visitCode();

28

 

mv.visitTypeInsn(NEW, 

"java/lang/StringBuilder"

);
29

 

mv.visitInsn(DUP);

30

 

mv.visitLdcInsn(

"Hello:"

);
31

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/StringBuilder"

,

32

 

"<init>"

, 

"(Ljava/lang/String;)V"

);
33

 

mv.visitVarInsn(ALOAD, 

1

);

34

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
35

 

"append"

, 

"(Ljava/lang/String;)Ljava/lang/StringBuilder;"

);

36

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
37

 

"toString"

, 

"()Ljava/lang/String;"

);

38

 

mv.visitInsn(ARETURN);
39

 

mv.visitMaxs(

3

, 

2

);

40

 

mv.visitEnd();
41

 

}

42

 

cw.visitEnd();
43

 

return
 

cw.toByteArray();

44

 

}
45

}

图 1 描述了 Java Source 文件、Java Class 文件、ASM Code 文件和 ASM Source 文件之间的转换关系。
[**图 1. 四种文件的转换图**]() 
[![图 1. 四种文件的转换图]()](http://static.oschina.net/uploads/img/201109/26204312_QD65.jpg) 

如图 -1 所示，Java Source 文件通过 Javac 可以转换为 Java Class 文件。而相应的 Java Class 文件通过 Java 反编译器工具可转换为 Java Source 文件；

通 过 ASM Code 去创建和修改 Java Class 需要对 ASM API 比较熟悉才行，一个常见的问题时，怎么用 ASM Code 生成一个我们希望的 class 文件， 也就是说， 给定了 Java Class， 怎么得到其对应的 ASM Code 呢？ 所幸的是， ASM 框架为我们提供了 ASMifierClassVisitor 工具来产生 Java Class 对应的 ASM Code。代码如下：
[**清单 7. 给定 class 文件，通过 ASMifierClassVisitor 获得其对应的 ASM Code**]()01

public
 

void
 

showClassASMProcess() {

02

  

ClassReader cr;
03

  

final
 

String n = SayHelloImpl.

class

.getName();

04

       
 
05

  

try

{

06

    

cr = 

new
 

ClassReader(n);                  
07

    

cr.accept(

new
 

ASMifierClassVisitor(

new

PrintWriter(System.out)),

08

    

new
 

Attribute[

0

],
09

    

ClassReader.SKIP_DEBUG);

10

               
 
11

  

} 

catch
 

(Exception e) {

12

    

e.printStackTrace();
13

 

}

14

}

另外一个重要的转换就是， 给定了 Java Class 文件， 如何查看它的 ASM Source ？ 这点对于我们验证 ASM Code 生成的 class 是否正确很有用。ASM 提供了另外一个工具 TraceClassVisitor， 来获得一个 Java Class 对应的 ASM Source。代码如下：
[**清单 8. 利用 TraceClassVisitor 查看 class 文件的 ASM Source**]()01

//file 是 class 文件的全路径名

02

public
 

void
 

showClassSource(String file) {
03

  

FileInputStream is ;

04

  

ClassReader cr; 
05

       
 

06

  

try
 

{
07

      

is = 

new
 

FileInputStream(file);     

08

      

cr = 

new
 

ClassReader(is);
09

      

TraceClassVisitor trace = 

new
 

TraceClassVisitor(

new

PrintWriter(System.out));

10

      

cr.accept(trace, ClassReader.SKIP_DEBUG);
11

           
 

12

      

is.close() ;
13

  

} 

catch
 

(Exception e) {

14

      

e.printStackTrace() ;
15

 

}

16

}
17

{

18

    

e.printStackTrace();
19

 

}

20

}

[ASM 类注入]()

ASM 类注入是指修改一个现有的 class 文件，在其中加入自己的代码。按通常思维，我们需要利用 ASM 提供的 reader 类去读取所要修改的类文件， 找到要修改的地方，如方法名或属性名，然后在该处插入自己的代码或修改现有的代码，这种思路将使问题复杂化。 ASM 为我们提供了访问类文件的 Visitor 模式，来遍历一个 class 文件，Visitor 是一个实现 ClassVisitor 接口的类。 要注入一个 class 文件，先要找到一个合适的 Visitor 做向导， ASM 提供了好几种 Visitor， 最常用的是 ClassWriter。同时我们需要提供一个适配器类 ClassAdapter， Visitor 会带着这个 Adapter 一起去遍历， 然后在遍历过程中回调 Adapter 提供的方法。我们在 Adapter 的这些方法中就可以实现我们的修改和定制逻辑。当然，如果你不想做任何修改，那 Visitor 遍历完后将得到一个和被遍历的 class 完全一样的拷贝。
[**清单 9. 类注入的完整过程**]()01

public
 

void
 

addAnnotationToExistingClass() {

02

 

FileInputStream is ;
03

 

ClassReader cr;          

04

 

try
 

{        
05

   

String classfile = 

"E:\\SayHelloImpl.class"
 

; 

// 待遍历的类

06

   

showClassSource(classfile) ;  

// 打印该类的 ASM source
07

           
 

08

   

is = 

new
 

FileInputStream(classfile);       
09

   

cr = 

new
 

ClassReader(is);

10

   

// 此处我们使用 ClassWriter 做 Visitor
11

   

ClassWriter cw = 

new
 

ClassWriter(ClassWriter.COMPUTE_MAXS);

12

// 给 Visitor 提供一个 Adapter
13

   

AddAnnotationAdapter adapter = 

new
 

AddAnnotationAdapter(cw);           

14

   

cr.accept(adapter, ClassReader.SKIP_DEBUG);            
15

   
 

16

   

// 遍历完后，生成的 class 保存在字节数组中
17

   

byte

[] b = cw.toByteArray();

18

           
 
19

   

try
 

{

20

        

// 将字节数组输出到文件中
21

        

// 为了不至于覆盖掉原有的 SayHelloImpl.class 文件，我们将结果输出到 _SayHelloImpl.class

22

     

FileOutputStream fout = 

new
 

FileOutputStream(

"E:\\_SayHelloImpl.class"

);
23

     

fout.write(b) ;

24

 

fout.flush();
25

     

fout.close() ;

26

   

} 

catch
 

(Exception e) {
27

     

e.printStackTrace() ;

28

   

}
29

           
 

30

   

// 验证 _SayHelloImpl.class 是否包含我们注入的代码
31

   

showClassSource(

"E:\\_SayHelloImpl.class"

) ;

32

} 

catch
 

(Exception e) {
33

     

e.printStackTrace();

34

} 
35

}

[web services 标记注入过程]()

我们已经知道了如何往 class 中注入自己的代码，那对于一个已有的 Java bean，怎么往里注入 @WebService 和 @WebMethod 呢？根据上面我们讲的转换关系，我们可以先写一个带 annotation 的 Java Source，编译得到 Java Class， 再得到其 ASM Code。下面是利用 ASM 给一个不带任何 Annotation 的 class 添加 @WebService 和 @WebMethod 的步骤。

1. 首先在 SayHelloImpl.java 中添加 @WebService 和 @WebMethod： 
[**清单 10. 带 web services 标注的 SayHelloImpl 类**]()1

@WebService

2

public
 

class
 

SayHelloImpl {
3

 

@WebMethod

4

 

public
 

String sayHello(String s) {
5

   

return
 

"Hello: "
 

+ s ;

6

 

}
7

}
1. 通过 ASMifierClassVisitor 工具获得 SayHelloImpl class 的 ASM Code，见清单 6。
1. 写一个 ClassAdapter 类， ClassAdapter 其实也是一个 Visitor。根据 ASM Code 的提示， 我们就可以知道如何利用 ASM API 去获得我们希望的 class 文件。下面是 Adapter 的示例代码。 
[**清单 11. 添加 Annotation 的 Adapter 类**]()01

public
 

class
 

AddAnnotationAdapter 

extends
 

ClassAdapter 

implements
 

Opcodes{

02

  

//private String annotationDesc;
03

  

private
 

boolean
 

isAnnotationPresent;

04
 
05

  

public
 

AddAnnotationAdapter(ClassVisitor cv) {

06

     

super

(cv);
07

  

}

08
 
09

  

@Override

10

  

public
 

void
 

visit(

int
 

version, 

int
 

access, String name, String signature,
11

    

String superName, String[] interfaces) {

12

        
 
13

    

cv.visit(V1_6, ACC_PUBLIC + ACC_SUPER, name, signature, superName,

14

    

interfaces);
15

    

AnnotationVisitor av0;

16

    

av0 = cv.visitAnnotation(

"Ljavax/jws/WebService;"

, 

true

);
17

    

av0.visitEnd();

18

  

}
19
 

20

  

@Override
21

  

public
 

AnnotationVisitor visitAnnotation(String desc, 

boolean
 

visible) {

22

    

return
 

cv.visitAnnotation(desc, visible);
23

  

}

24
 
25

  

@Override

26

  

public
 

void
 

visitInnerClass(String name, String outerName,
27

    

String innerName, 

int
 

access) {

28

    

cv.visitInnerClass(name, outerName, innerName, access);
29

  

}

30
 
31

  

@Override

32

  

public
 

FieldVisitor visitField(

int
 

access, String name, String desc,
33

    

String signature, Object value) {

34

    

return
 

cv.visitField(access, name, desc, signature, value);
35

  

}

36
 
37

  

@Override

38

  

public
 

MethodVisitor visitMethod(

int
 

access, String name, String desc,
39

    

String signature, String[] exceptions) {   

40

         

MethodVisitor mv  = cv.visitMethod(access, name, desc, signature,
41

          

exceptions);

42

         

if
 

(!name.equals(

"<init>"

)) {
43

          

AnnotationVisitor av0;

44

          

av0 = mv.visitAnnotation(

"Ljavax/jws/WebMethod;"

, 

true

);
45

          

av0.visitEnd();

46

         

}
47

         

return
 

mv;

48

     

}
49
 

50

  

@Override
51

  

public
 

void
 

visitEnd() {

52

    

cv.visitEnd();
53

  

}

54
 
55

 

}

最后，我们采用上面讲的类注入办法，即可得到一个带 @WebService 和 @WebMethod 标注的 class 文件。再通过 wsgen 工具即可成功创建 web services，该命令将会在 src 目录下生成 web services 描述文件：SayHelloImplService.wsdl，SayHelloImplService_schema1.xsd，同时生成了 2 个 JAX-WS 文件，分别为：SayHello.java，SayHelloResponse.java。

当然，在实际应用中，我们可能需要将一个 class 的某些特定方法发布为 web services，这就需要我们对 Adapter 做进一步的改造，在 visitMethod() 方法中根据方法名称等参数做进一步的处理。

[结束语]()

本文简要介绍了 ASM 字节码工具及其在 web services 开发中的应用。web services 的使用已经越来越广泛， 但在改造遗留系统过程中会遇到不少问题，本文针对其中一个主要问题给出了解决办法，该方法无须对现有系统做大量的改动， 即可将现存的 Java bean 转化成 web services 。本文对那些正考虑迁移到 JAX-WS 编程模型上的项目有一定的参考价值。
来源： <[http://www.oschina.net/question/129540_28430](http://www.oschina.net/question/129540_28430)> 

在基于 JAX-WS 标准的 web services 的开发中，不少实际场景都是希望采用自底向上的开发方式， 即基于已有的 Java bean 来创建 web services 。WebSphere Application Server ( 以下简称 WAS ) 提供了命令行的工具 wsgen 和相对应的 Ant task 来支持这种开发过程，而且这两个工具比较适合大型项目的自动化构建。 这两个工具的使用前提是 Java bean 中事先添加有 web services 的 Annotation 标注，而在现有的业务系统中，class 文件一般是不带 Annotation 的，这就需要开发人员去修改现有的代码，以手工方式添加 Annotation，但这样带来的工作量太大且容易出错。本文介绍了一种解决途径，可以不用修改源代码，而是利用字节码工具 ASM 直接修改 class 文件， 在字节码文件中自动注入 Annotation ，然后再利用 wsgen 工具就可以很方便地生成 web services 应用。 本文同时也总结了使用 ASM 的一些实践。

本文首先介绍了和 ASM 使用相关的四种文件，以及它们之间的相互转化。然后结合 web services 开发实例，介绍了使用 wsgen 开发时遇到的实际问题，如何写一个 ASM 的适配器去修改现有 class，最终如何产生 web services 文件。

ASM 是一个多用途的 Java 字节码操控和分析框架。它能以二进制的形式直接修改现有的类或动态生成的类。 ASM 提供了常用的转换和分析算法，是一种允许用户方便的组装定制化的复杂转换和代码分析工具。 ASM 也提供和其它的字节码工具类似的功能，但 ASM 的重点是使用的简单和高性能。由于 ASM 在设计实现的时候尽可能 的小的内存占用和提供更高的性能；因此在动态系统中使用 ASM 是很有优势的。ASM 作为一种轻量级、高性能的 Java 字节码操控和分析框架，设计了一种更有效的方法、提供更好的性能和内存占用。今天， ASM 在许多领域都有应用，并已成为事实上的字节码处理框架标准。

[问题的引入]()

先写一个不带 web services 标注的 SayHelloImpl 类。
[**清单 1. 不带 web services 的 SayHelloImpl 类**]()1

public
 

class
 

SayHelloImpl {

2

 

public
 

String sayHello(String s) {
3

   

return
 

“Hello: ” + s ;

4

 

}
5

}

再用 wsgen 来创建 web services，wsgen 的使用如下：
[**清单 2. wsgen 实例**]()1

wsgen.exe – 

cp
 

bin – s output – d output – r output – wsdl ibm.was.asm.SayHelloImpl我们将会遇到如下错误： 
[**清单 3. 根据不带 web services 标注的类创建 web services 时遇到的错误信息**]() 
注释处理过程中遇到问题；

......
com.sun.tools.internal.ws.processor.modeler.ModelerException: [failed to localiz

e] A web service endpoint could not be found()
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.o

nError(WebServiceAP.java:215)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.b

uildModel(WebServiceAP.java:322)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.p

rocess(WebServiceAP.java:256)
        at com.sun.mirror.apt.AnnotationProcessors$CompositeAnnotationProcessor.

process(AnnotationProcessors.java:60)

注意：我们通常是在 class 前使用 @WebService， 在方法前使用 @WebMethod 来将一个类标注为 web services 的。WAS 6.1 的 web services 功能部件包和 WAS 7 都支持基于 JAX-WS 标准 的 web services 的开发， 在 WAS 安装目录的 bin 目录下提供了命令行工具 wsgen，wsgen 支持自底向上的开发方式。 下面是 wsgen 的用法：
[**清单 4. wsgen 命令格式**]() 
注 wsgen [options]  [SEI]

主要选项：

* -d 指定生成的 class 文件位置；
* -s 指定生成的 Java 源文件位置；
* -r 指定生成的 resources 文件位置，如 wsdl，xsd；
* -cp 指定服务实现类所在的位置；
* -wsdl，-servicename，-portname 三个参数指定生成的 wsdl 文件中的 service 和 port 的名称。

SEI(Service Endpoint Interface) 是一个 endpoint implementation class，不能是一个 interface。所以首先要开发一个 endpoint 的实现类，如本例中的 SayHelloImpl 。用 @WebService 声明 web services ，然后将它编译，才能提供给 wsgen 来创建 web services 。

[四种文件之间的转换]()

下面介绍本文要用到的四个概念（Java Source，Java Class，ASM Code，ASM Source）

* Java Source: 即我们通常编写的 Java 源文件；
* Java Class: Java 源文件编译后的字节码文件；
* ASM Source : 类似于对 class 反编译后的源文件，也就是 "textual byte code"，但比原始的 Java Source 可读性要差， 参见清单 5。
* ASM Code: 指读写 class 文件的 ASM 程序代码。需要用到 ASM 提供的 API， 参见清单 6。
[**清单 5. SayHelloImpl 类对应的 ASM Source**]()01

// class version 50.0 (50)

02

// access flags 33
03

public
 

class
 

com/ibm/was/asm/SayHelloImpl {

04
 
05

 

// access flags 1

06

 

public
  

<init>()V
07

   

ALOAD 

0

08

   

INVOKESPECIAL java/lang/Object. <init> ()V
09

   

RETURN

10

   

MAXSTACK = 

1
11

   

MAXLOCALS = 

1

12
 
13

 

// access flags 1

14

 

public
 

sayHello(Ljava/lang/String;)Ljava/lang/String;
15

   

NEW java/lang/StringBuilder

16

   

DUP
17

   

LDC 

"Hello:"

18

   

INVOKESPECIAL java/lang/StringBuilder. <init> (Ljava/lang/String;)V
19

   

ALOAD 

1

20

   

INVOKEVIRTUAL java/lang/StringBuilder.append (Ljava/lang/String;)
21

   

Ljava/lang/StringBuilder;

22

   

INVOKEVIRTUAL java/lang/StringBuilder.toString ()Ljava/lang/String;
23

   

ARETURN

24

   

MAXSTACK = 

3
25

   

MAXLOCALS = 

2

26

}[**清单 6. 生成 SayHelloImpl.class 的 ASM Code**]()01

package
 

asm.com.ibm.was.asm;

02

import
 

java.util.*;
03

import
 

org.objectweb.asm.*;

04

import
 

org.objectweb.asm.attrs.*;
05

public
 

class
 

SayHelloImplDump 

implements
 

Opcodes {

06

 

public
 

static
 

byte

[] dump () 

throws
 

Exception {
07

 

ClassWriter cw = 

new
 

ClassWriter(

0

);

08

 

FieldVisitor fv;
09

 

MethodVisitor mv;

10

 

AnnotationVisitor av0;
11
 

12

 

cw.visit(V1_6, ACC_PUBLIC + ACC_SUPER, 

"com/ibm/was/asm/SayHelloImpl"

,
13

 

null

, 

"java/lang/Object"

, 

null

);

14
 
15

 

{

16

 

mv = cw.visitMethod(ACC_PUBLIC, 

"<init>"

, 

"()V"

, 

null

, 

null

);
17

 

mv.visitCode();

18

 

mv.visitVarInsn(ALOAD, 

0

);
19

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/Object"

, 

"<init>"

, 

"()V"

);

20

 

mv.visitInsn(RETURN);
21

 

mv.visitMaxs(

1

, 

1

);

22

 

mv.visitEnd();
23

 

}

24

 

{
25

 

mv = cw.visitMethod(ACC_PUBLIC, 

"sayHello"

,

26

  

"(Ljava/lang/String;)Ljava/lang/String;"

, 

null

, 

null

);
27

 

mv.visitCode();

28

 

mv.visitTypeInsn(NEW, 

"java/lang/StringBuilder"

);
29

 

mv.visitInsn(DUP);

30

 

mv.visitLdcInsn(

"Hello:"

);
31

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/StringBuilder"

,

32

 

"<init>"

, 

"(Ljava/lang/String;)V"

);
33

 

mv.visitVarInsn(ALOAD, 

1

);

34

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
35

 

"append"

, 

"(Ljava/lang/String;)Ljava/lang/StringBuilder;"

);

36

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
37

 

"toString"

, 

"()Ljava/lang/String;"

);

38

 

mv.visitInsn(ARETURN);
39

 

mv.visitMaxs(

3

, 

2

);

40

 

mv.visitEnd();
41

 

}

42

 

cw.visitEnd();
43

 

return
 

cw.toByteArray();

44

 

}
45

}

图 1 描述了 Java Source 文件、Java Class 文件、ASM Code 文件和 ASM Source 文件之间的转换关系。
[**图 1. 四种文件的转换图**]() 
[![图 1. 四种文件的转换图]()]() 

如图 -1 所示，Java Source 文件通过 Javac 可以转换为 Java Class 文件。而相应的 Java Class 文件通过 Java 反编译器工具可转换为 Java Source 文件；

通 过 ASM Code 去创建和修改 Java Class 需要对 ASM API 比较熟悉才行，一个常见的问题时，怎么用 ASM Code 生成一个我们希望的 class 文件， 也就是说， 给定了 Java Class， 怎么得到其对应的 ASM Code 呢？ 所幸的是， ASM 框架为我们提供了 ASMifierClassVisitor 工具来产生 Java Class 对应的 ASM Code。代码如下：
[**清单 7. 给定 class 文件，通过 ASMifierClassVisitor 获得其对应的 ASM Code**]()[view source](http://www.oschina.net/question/129540_28430#viewSource "view source")[print](http://www.oschina.net/question/129540_28430#printSource "print")[?](http://www.oschina.net/question/129540_28430#about "?")

01

public
 

void
 

showClassASMProcess() {

02

  

ClassReader cr;
03

  

final
 

String n = SayHelloImpl.

class

.getName();

04

       
 
05

  

try

{

06

    

cr = 

new
 

ClassReader(n);                  
07

    

cr.accept(

new
 

ASMifierClassVisitor(

new

PrintWriter(System.out)),

08

    

new
 

Attribute[

0

],
09

    

ClassReader.SKIP_DEBUG);

10

               
 
11

  

} 

catch
 

(Exception e) {

12

    

e.printStackTrace();
13

 

}

14

}

另外一个重要的转换就是， 给定了 Java Class 文件， 如何查看它的 ASM Source ？ 这点对于我们验证 ASM Code 生成的 class 是否正确很有用。ASM 提供了另外一个工具 TraceClassVisitor， 来获得一个 Java Class 对应的 ASM Source。代码如下：
[**清单 8. 利用 TraceClassVisitor 查看 class 文件的 ASM Source**]()01

//file 是 class 文件的全路径名

02

public
 

void
 

showClassSource(String file) {
03

  

FileInputStream is ;

04

  

ClassReader cr; 
05

       
 

06

  

try
 

{
07

      

is = 

new
 

FileInputStream(file);     

08

      

cr = 

new
 

ClassReader(is);
09

      

TraceClassVisitor trace = 

new
 

TraceClassVisitor(

new

PrintWriter(System.out));

10

      

cr.accept(trace, ClassReader.SKIP_DEBUG);
11

           
 

12

      

is.close() ;
13

  

} 

catch
 

(Exception e) {

14

      

e.printStackTrace() ;
15

 

}

16

}
17

{

18

    

e.printStackTrace();
19

 

}

20

}

[ASM 类注入]()

ASM 类注入是指修改一个现有的 class 文件，在其中加入自己的代码。按通常思维，我们需要利用 ASM 提供的 reader 类去读取所要修改的类文件， 找到要修改的地方，如方法名或属性名，然后在该处插入自己的代码或修改现有的代码，这种思路将使问题复杂化。 ASM 为我们提供了访问类文件的 Visitor 模式，来遍历一个 class 文件，Visitor 是一个实现 ClassVisitor 接口的类。 要注入一个 class 文件，先要找到一个合适的 Visitor 做向导， ASM 提供了好几种 Visitor， 最常用的是 ClassWriter。同时我们需要提供一个适配器类 ClassAdapter， Visitor 会带着这个 Adapter 一起去遍历， 然后在遍历过程中回调 Adapter 提供的方法。我们在 Adapter 的这些方法中就可以实现我们的修改和定制逻辑。当然，如果你不想做任何修改，那 Visitor 遍历完后将得到一个和被遍历的 class 完全一样的拷贝。
[**清单 9. 类注入的完整过程**]()01

public
 

void
 

addAnnotationToExistingClass() {

02

 

FileInputStream is ;
03

 

ClassReader cr;          

04

 

try
 

{        
05

   

String classfile = 

"E:\\SayHelloImpl.class"
 

; 

// 待遍历的类

06

   

showClassSource(classfile) ;  

// 打印该类的 ASM source
07

           
 

08

   

is = 

new
 

FileInputStream(classfile);       
09

   

cr = 

new
 

ClassReader(is);

10

   

// 此处我们使用 ClassWriter 做 Visitor
11

   

ClassWriter cw = 

new
 

ClassWriter(ClassWriter.COMPUTE_MAXS);

12

// 给 Visitor 提供一个 Adapter
13

   

AddAnnotationAdapter adapter = 

new
 

AddAnnotationAdapter(cw);           

14

   

cr.accept(adapter, ClassReader.SKIP_DEBUG);            
15

   
 

16

   

// 遍历完后，生成的 class 保存在字节数组中
17

   

byte

[] b = cw.toByteArray();

18

           
 
19

   

try
 

{

20

        

// 将字节数组输出到文件中
21

        

// 为了不至于覆盖掉原有的 SayHelloImpl.class 文件，我们将结果输出到 _SayHelloImpl.class

22

     

FileOutputStream fout = 

new
 

FileOutputStream(

"E:\\_SayHelloImpl.class"

);
23

     

fout.write(b) ;

24

 

fout.flush();
25

     

fout.close() ;

26

   

} 

catch
 

(Exception e) {
27

     

e.printStackTrace() ;

28

   

}
29

           
 

30

   

// 验证 _SayHelloImpl.class 是否包含我们注入的代码
31

   

showClassSource(

"E:\\_SayHelloImpl.class"

) ;

32

} 

catch
 

(Exception e) {
33

     

e.printStackTrace();

34

} 
35

}

[web services 标记注入过程]()

我们已经知道了如何往 class 中注入自己的代码，那对于一个已有的 Java bean，怎么往里注入 @WebService 和 @WebMethod 呢？根据上面我们讲的转换关系，我们可以先写一个带 annotation 的 Java Source，编译得到 Java Class， 再得到其 ASM Code。下面是利用 ASM 给一个不带任何 Annotation 的 class 添加 @WebService 和 @WebMethod 的步骤。

1. 首先在 SayHelloImpl.java 中添加 @WebService 和 @WebMethod： 
[**清单 10. 带 web services 标注的 SayHelloImpl 类**]()1

@WebService

2

public
 

class
 

SayHelloImpl {
3

 

@WebMethod

4

 

public
 

String sayHello(String s) {
5

   

return
 

"Hello: "
 

+ s ;

6

 

}
7

}
1. 通过 ASMifierClassVisitor 工具获得 SayHelloImpl class 的 ASM Code，见清单 6。
1. 写一个 ClassAdapter 类， ClassAdapter 其实也是一个 Visitor。根据 ASM Code 的提示， 我们就可以知道如何利用 ASM API 去获得我们希望的 class 文件。下面是 Adapter 的示例代码。 
[**清单 11. 添加 Annotation 的 Adapter 类**]()01

public
 

class
 

AddAnnotationAdapter 

extends
 

ClassAdapter 

implements
 

Opcodes{

02

  

//private String annotationDesc;
03

  

private
 

boolean
 

isAnnotationPresent;

04
 
05

  

public
 

AddAnnotationAdapter(ClassVisitor cv) {

06

     

super

(cv);
07

  

}

08
 
09

  

@Override

10

  

public
 

void
 

visit(

int
 

version, 

int
 

access, String name, String signature,
11

    

String superName, String[] interfaces) {

12

        
 
13

    

cv.visit(V1_6, ACC_PUBLIC + ACC_SUPER, name, signature, superName,

14

    

interfaces);
15

    

AnnotationVisitor av0;

16

    

av0 = cv.visitAnnotation(

"Ljavax/jws/WebService;"

, 

true

);
17

    

av0.visitEnd();

18

  

}
19
 

20

  

@Override
21

  

public
 

AnnotationVisitor visitAnnotation(String desc, 

boolean
 

visible) {

22

    

return
 

cv.visitAnnotation(desc, visible);
23

  

}

24
 
25

  

@Override

26

  

public
 

void
 

visitInnerClass(String name, String outerName,
27

    

String innerName, 

int
 

access) {

28

    

cv.visitInnerClass(name, outerName, innerName, access);
29

  

}

30
 
31

  

@Override

32

  

public
 

FieldVisitor visitField(

int
 

access, String name, String desc,
33

    

String signature, Object value) {

34

    

return
 

cv.visitField(access, name, desc, signature, value);
35

  

}

36
 
37

  

@Override

38

  

public
 

MethodVisitor visitMethod(

int
 

access, String name, String desc,
39

    

String signature, String[] exceptions) {   

40

         

MethodVisitor mv  = cv.visitMethod(access, name, desc, signature,
41

          

exceptions);

42

         

if
 

(!name.equals(

"<init>"

)) {
43

          

AnnotationVisitor av0;

44

          

av0 = mv.visitAnnotation(

"Ljavax/jws/WebMethod;"

, 

true

);
45

          

av0.visitEnd();

46

         

}
47

         

return
 

mv;

48

     

}
49
 

50

  

@Override
51

  

public
 

void
 

visitEnd() {

52

    

cv.visitEnd();
53

  

}

54
 
55

 

}

最后，我们采用上面讲的类注入办法，即可得到一个带 @WebService 和 @WebMethod 标注的 class 文件。再通过 wsgen 工具即可成功创建 web services，该命令将会在 src 目录下生成 web services 描述文件：SayHelloImplService.wsdl，SayHelloImplService_schema1.xsd，同时生成了 2 个 JAX-WS 文件，分别为：SayHello.java，SayHelloResponse.java。

当然，在实际应用中，我们可能需要将一个 class 的某些特定方法发布为 web services，这就需要我们对 Adapter 做进一步的改造，在 visitMethod() 方法中根据方法名称等参数做进一步的处理。

[结束语]()

本文简要介绍了 ASM 字节码工具及其在 web services 开发中的应用。web services 的使用已经越来越广泛， 但在改造遗留系统过程中会遇到不少问题，本文针对其中一个主要问题给出了解决办法，该方法无须对现有系统做大量的改动， 即可将现存的 Java bean 转化成 web services 。本文对那些正考虑迁移到 JAX-WS 编程模型上的项目有一定的参考价值。在基于 JAX-WS 标准的 web services 的开发中，不少实际场景都是希望采用自底向上的开发方式， 即基于已有的 Java bean 来创建 web services 。WebSphere Application Server ( 以下简称 WAS ) 提供了命令行的工具 wsgen 和相对应的 Ant task 来支持这种开发过程，而且这两个工具比较适合大型项目的自动化构建。 这两个工具的使用前提是 Java bean 中事先添加有 web services 的 Annotation 标注，而在现有的业务系统中，class 文件一般是不带 Annotation 的，这就需要开发人员去修改现有的代码，以手工方式添加 Annotation，但这样带来的工作量太大且容易出错。本文介绍了一种解决途径，可以不用修改源代码，而是利用字节码工具 ASM 直接修改 class 文件， 在字节码文件中自动注入 Annotation ，然后再利用 wsgen 工具就可以很方便地生成 web services 应用。 本文同时也总结了使用 ASM 的一些实践。

本文首先介绍了和 ASM 使用相关的四种文件，以及它们之间的相互转化。然后结合 web services 开发实例，介绍了使用 wsgen 开发时遇到的实际问题，如何写一个 ASM 的适配器去修改现有 class，最终如何产生 web services 文件。

ASM 是一个多用途的 Java 字节码操控和分析框架。它能以二进制的形式直接修改现有的类或动态生成的类。 ASM 提供了常用的转换和分析算法，是一种允许用户方便的组装定制化的复杂转换和代码分析工具。 ASM 也提供和其它的字节码工具类似的功能，但 ASM 的重点是使用的简单和高性能。由于 ASM 在设计实现的时候尽可能 的小的内存占用和提供更高的性能；因此在动态系统中使用 ASM 是很有优势的。ASM 作为一种轻量级、高性能的 Java 字节码操控和分析框架，设计了一种更有效的方法、提供更好的性能和内存占用。今天， ASM 在许多领域都有应用，并已成为事实上的字节码处理框架标准。

[问题的引入]()

先写一个不带 web services 标注的 SayHelloImpl 类。
[**清单 1. 不带 web services 的 SayHelloImpl 类**]()1

public
 

class
 

SayHelloImpl {

2

 

public
 

String sayHello(String s) {
3

   

return
 

“Hello: ” + s ;

4

 

}
5

}

再用 wsgen 来创建 web services，wsgen 的使用如下：
[**清单 2. wsgen 实例**]()1

wsgen.exe – 

cp
 

bin – s output – d output – r output – wsdl ibm.was.asm.SayHelloImpl我们将会遇到如下错误： 
[**清单 3. 根据不带 web services 标注的类创建 web services 时遇到的错误信息**]() 
注释处理过程中遇到问题；

......
com.sun.tools.internal.ws.processor.modeler.ModelerException: [failed to localiz

e] A web service endpoint could not be found()
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.o

nError(WebServiceAP.java:215)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.b

uildModel(WebServiceAP.java:322)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.p

rocess(WebServiceAP.java:256)
        at com.sun.mirror.apt.AnnotationProcessors$CompositeAnnotationProcessor.

process(AnnotationProcessors.java:60)

注意：我们通常是在 class 前使用 @WebService， 在方法前使用 @WebMethod 来将一个类标注为 web services 的。WAS 6.1 的 web services 功能部件包和 WAS 7 都支持基于 JAX-WS 标准 的 web services 的开发， 在 WAS 安装目录的 bin 目录下提供了命令行工具 wsgen，wsgen 支持自底向上的开发方式。 下面是 wsgen 的用法：
[**清单 4. wsgen 命令格式**]() 
注 wsgen [options]  [SEI]

主要选项：

* -d 指定生成的 class 文件位置；
* -s 指定生成的 Java 源文件位置；
* -r 指定生成的 resources 文件位置，如 wsdl，xsd；
* -cp 指定服务实现类所在的位置；
* -wsdl，-servicename，-portname 三个参数指定生成的 wsdl 文件中的 service 和 port 的名称。

SEI(Service Endpoint Interface) 是一个 endpoint implementation class，不能是一个 interface。所以首先要开发一个 endpoint 的实现类，如本例中的 SayHelloImpl 。用 @WebService 声明 web services ，然后将它编译，才能提供给 wsgen 来创建 web services 。

[四种文件之间的转换]()

下面介绍本文要用到的四个概念（Java Source，Java Class，ASM Code，ASM Source）

* Java Source: 即我们通常编写的 Java 源文件；
* Java Class: Java 源文件编译后的字节码文件；
* ASM Source : 类似于对 class 反编译后的源文件，也就是 "textual byte code"，但比原始的 Java Source 可读性要差， 参见清单 5。
* ASM Code: 指读写 class 文件的 ASM 程序代码。需要用到 ASM 提供的 API， 参见清单 6。
[**清单 5. SayHelloImpl 类对应的 ASM Source**]()01

// class version 50.0 (50)

02

// access flags 33
03

public
 

class
 

com/ibm/was/asm/SayHelloImpl {

04
 
05

 

// access flags 1

06

 

public
  

<init>()V
07

   

ALOAD 

0

08

   

INVOKESPECIAL java/lang/Object. <init> ()V
09

   

RETURN

10

   

MAXSTACK = 

1
11

   

MAXLOCALS = 

1

12
 
13

 

// access flags 1

14

 

public
 

sayHello(Ljava/lang/String;)Ljava/lang/String;
15

   

NEW java/lang/StringBuilder

16

   

DUP
17

   

LDC 

"Hello:"

18

   

INVOKESPECIAL java/lang/StringBuilder. <init> (Ljava/lang/String;)V
19

   

ALOAD 

1

20

   

INVOKEVIRTUAL java/lang/StringBuilder.append (Ljava/lang/String;)
21

   

Ljava/lang/StringBuilder;

22

   

INVOKEVIRTUAL java/lang/StringBuilder.toString ()Ljava/lang/String;
23

   

ARETURN

24

   

MAXSTACK = 

3
25

   

MAXLOCALS = 

2

26

}[**清单 6. 生成 SayHelloImpl.class 的 ASM Code**]()01

package
 

asm.com.ibm.was.asm;

02

import
 

java.util.*;
03

import
 

org.objectweb.asm.*;

04

import
 

org.objectweb.asm.attrs.*;
05

public
 

class
 

SayHelloImplDump 

implements
 

Opcodes {

06

 

public
 

static
 

byte

[] dump () 

throws
 

Exception {
07

 

ClassWriter cw = 

new
 

ClassWriter(

0

);

08

 

FieldVisitor fv;
09

 

MethodVisitor mv;

10

 

AnnotationVisitor av0;
11
 

12

 

cw.visit(V1_6, ACC_PUBLIC + ACC_SUPER, 

"com/ibm/was/asm/SayHelloImpl"

,
13

 

null

, 

"java/lang/Object"

, 

null

);

14
 
15

 

{

16

 

mv = cw.visitMethod(ACC_PUBLIC, 

"<init>"

, 

"()V"

, 

null

, 

null

);
17

 

mv.visitCode();

18

 

mv.visitVarInsn(ALOAD, 

0

);
19

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/Object"

, 

"<init>"

, 

"()V"

);

20

 

mv.visitInsn(RETURN);
21

 

mv.visitMaxs(

1

, 

1

);

22

 

mv.visitEnd();
23

 

}

24

 

{
25

 

mv = cw.visitMethod(ACC_PUBLIC, 

"sayHello"

,

26

  

"(Ljava/lang/String;)Ljava/lang/String;"

, 

null

, 

null

);
27

 

mv.visitCode();

28

 

mv.visitTypeInsn(NEW, 

"java/lang/StringBuilder"

);
29

 

mv.visitInsn(DUP);

30

 

mv.visitLdcInsn(

"Hello:"

);
31

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/StringBuilder"

,

32

 

"<init>"

, 

"(Ljava/lang/String;)V"

);
33

 

mv.visitVarInsn(ALOAD, 

1

);

34

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
35

 

"append"

, 

"(Ljava/lang/String;)Ljava/lang/StringBuilder;"

);

36

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
37

 

"toString"

, 

"()Ljava/lang/String;"

);

38

 

mv.visitInsn(ARETURN);
39

 

mv.visitMaxs(

3

, 

2

);

40

 

mv.visitEnd();
41

 

}

42

 

cw.visitEnd();
43

 

return
 

cw.toByteArray();

44

 

}
45

}

图 1 描述了 Java Source 文件、Java Class 文件、ASM Code 文件和 ASM Source 文件之间的转换关系。
[**图 1. 四种文件的转换图**]() 
[![图 1. 四种文件的转换图]()]() 

如图 -1 所示，Java Source 文件通过 Javac 可以转换为 Java Class 文件。而相应的 Java Class 文件通过 Java 反编译器工具可转换为 Java Source 文件；

通 过 ASM Code 去创建和修改 Java Class 需要对 ASM API 比较熟悉才行，一个常见的问题时，怎么用 ASM Code 生成一个我们希望的 class 文件， 也就是说， 给定了 Java Class， 怎么得到其对应的 ASM Code 呢？ 所幸的是， ASM 框架为我们提供了 ASMifierClassVisitor 工具来产生 Java Class 对应的 ASM Code。代码如下：
[**清单 7. 给定 class 文件，通过 ASMifierClassVisitor 获得其对应的 ASM Code**]()[view source](http://www.oschina.net/question/129540_28430#viewSource "view source")[print](http://www.oschina.net/question/129540_28430#printSource "print")[?](http://www.oschina.net/question/129540_28430#about "?")

01

public
 

void
 

showClassASMProcess() {

02

  

ClassReader cr;
03

  

final
 

String n = SayHelloImpl.

class

.getName();

04

       
 
05

  

try

{

06

    

cr = 

new
 

ClassReader(n);                  
07

    

cr.accept(

new
 

ASMifierClassVisitor(

new

PrintWriter(System.out)),

08

    

new
 

Attribute[

0

],
09

    

ClassReader.SKIP_DEBUG);

10

               
 
11

  

} 

catch
 

(Exception e) {

12

    

e.printStackTrace();
13

 

}

14

}

另外一个重要的转换就是， 给定了 Java Class 文件， 如何查看它的 ASM Source ？ 这点对于我们验证 ASM Code 生成的 class 是否正确很有用。ASM 提供了另外一个工具 TraceClassVisitor， 来获得一个 Java Class 对应的 ASM Source。代码如下：
[**清单 8. 利用 TraceClassVisitor 查看 class 文件的 ASM Source**]()01

//file 是 class 文件的全路径名

02

public
 

void
 

showClassSource(String file) {
03

  

FileInputStream is ;

04

  

ClassReader cr; 
05

       
 

06

  

try
 

{
07

      

is = 

new
 

FileInputStream(file);     

08

      

cr = 

new
 

ClassReader(is);
09

      

TraceClassVisitor trace = 

new
 

TraceClassVisitor(

new

PrintWriter(System.out));

10

      

cr.accept(trace, ClassReader.SKIP_DEBUG);
11

           
 

12

      

is.close() ;
13

  

} 

catch
 

(Exception e) {

14

      

e.printStackTrace() ;
15

 

}

16

}
17

{

18

    

e.printStackTrace();
19

 

}

20

}

[ASM 类注入]()

ASM 类注入是指修改一个现有的 class 文件，在其中加入自己的代码。按通常思维，我们需要利用 ASM 提供的 reader 类去读取所要修改的类文件， 找到要修改的地方，如方法名或属性名，然后在该处插入自己的代码或修改现有的代码，这种思路将使问题复杂化。 ASM 为我们提供了访问类文件的 Visitor 模式，来遍历一个 class 文件，Visitor 是一个实现 ClassVisitor 接口的类。 要注入一个 class 文件，先要找到一个合适的 Visitor 做向导， ASM 提供了好几种 Visitor， 最常用的是 ClassWriter。同时我们需要提供一个适配器类 ClassAdapter， Visitor 会带着这个 Adapter 一起去遍历， 然后在遍历过程中回调 Adapter 提供的方法。我们在 Adapter 的这些方法中就可以实现我们的修改和定制逻辑。当然，如果你不想做任何修改，那 Visitor 遍历完后将得到一个和被遍历的 class 完全一样的拷贝。
[**清单 9. 类注入的完整过程**]()01

public
 

void
 

addAnnotationToExistingClass() {

02

 

FileInputStream is ;
03

 

ClassReader cr;          

04

 

try
 

{        
05

   

String classfile = 

"E:\\SayHelloImpl.class"
 

; 

// 待遍历的类

06

   

showClassSource(classfile) ;  

// 打印该类的 ASM source
07

           
 

08

   

is = 

new
 

FileInputStream(classfile);       
09

   

cr = 

new
 

ClassReader(is);

10

   

// 此处我们使用 ClassWriter 做 Visitor
11

   

ClassWriter cw = 

new
 

ClassWriter(ClassWriter.COMPUTE_MAXS);

12

// 给 Visitor 提供一个 Adapter
13

   

AddAnnotationAdapter adapter = 

new
 

AddAnnotationAdapter(cw);           

14

   

cr.accept(adapter, ClassReader.SKIP_DEBUG);            
15

   
 

16

   

// 遍历完后，生成的 class 保存在字节数组中
17

   

byte

[] b = cw.toByteArray();

18

           
 
19

   

try
 

{

20

        

// 将字节数组输出到文件中
21

        

// 为了不至于覆盖掉原有的 SayHelloImpl.class 文件，我们将结果输出到 _SayHelloImpl.class

22

     

FileOutputStream fout = 

new
 

FileOutputStream(

"E:\\_SayHelloImpl.class"

);
23

     

fout.write(b) ;

24

 

fout.flush();
25

     

fout.close() ;

26

   

} 

catch
 

(Exception e) {
27

     

e.printStackTrace() ;

28

   

}
29

           
 

30

   

// 验证 _SayHelloImpl.class 是否包含我们注入的代码
31

   

showClassSource(

"E:\\_SayHelloImpl.class"

) ;

32

} 

catch
 

(Exception e) {
33

     

e.printStackTrace();

34

} 
35

}

[web services 标记注入过程]()

我们已经知道了如何往 class 中注入自己的代码，那对于一个已有的 Java bean，怎么往里注入 @WebService 和 @WebMethod 呢？根据上面我们讲的转换关系，我们可以先写一个带 annotation 的 Java Source，编译得到 Java Class， 再得到其 ASM Code。下面是利用 ASM 给一个不带任何 Annotation 的 class 添加 @WebService 和 @WebMethod 的步骤。

1. 首先在 SayHelloImpl.java 中添加 @WebService 和 @WebMethod： 
[**清单 10. 带 web services 标注的 SayHelloImpl 类**]()1

@WebService

2

public
 

class
 

SayHelloImpl {
3

 

@WebMethod

4

 

public
 

String sayHello(String s) {
5

   

return
 

"Hello: "
 

+ s ;

6

 

}
7

}
1. 通过 ASMifierClassVisitor 工具获得 SayHelloImpl class 的 ASM Code，见清单 6。
1. 写一个 ClassAdapter 类， ClassAdapter 其实也是一个 Visitor。根据 ASM Code 的提示， 我们就可以知道如何利用 ASM API 去获得我们希望的 class 文件。下面是 Adapter 的示例代码。 
[**清单 11. 添加 Annotation 的 Adapter 类**]()01

public
 

class
 

AddAnnotationAdapter 

extends
 

ClassAdapter 

implements
 

Opcodes{

02

  

//private String annotationDesc;
03

  

private
 

boolean
 

isAnnotationPresent;

04
 
05

  

public
 

AddAnnotationAdapter(ClassVisitor cv) {

06

     

super

(cv);
07

  

}

08
 
09

  

@Override

10

  

public
 

void
 

visit(

int
 

version, 

int
 

access, String name, String signature,
11

    

String superName, String[] interfaces) {

12

        
 
13

    

cv.visit(V1_6, ACC_PUBLIC + ACC_SUPER, name, signature, superName,

14

    

interfaces);
15

    

AnnotationVisitor av0;

16

    

av0 = cv.visitAnnotation(

"Ljavax/jws/WebService;"

, 

true

);
17

    

av0.visitEnd();

18

  

}
19
 

20

  

@Override
21

  

public
 

AnnotationVisitor visitAnnotation(String desc, 

boolean
 

visible) {

22

    

return
 

cv.visitAnnotation(desc, visible);
23

  

}

24
 
25

  

@Override

26

  

public
 

void
 

visitInnerClass(String name, String outerName,
27

    

String innerName, 

int
 

access) {

28

    

cv.visitInnerClass(name, outerName, innerName, access);
29

  

}

30
 
31

  

@Override

32

  

public
 

FieldVisitor visitField(

int
 

access, String name, String desc,
33

    

String signature, Object value) {

34

    

return
 

cv.visitField(access, name, desc, signature, value);
35

  

}

36
 
37

  

@Override

38

  

public
 

MethodVisitor visitMethod(

int
 

access, String name, String desc,
39

    

String signature, String[] exceptions) {   

40

         

MethodVisitor mv  = cv.visitMethod(access, name, desc, signature,
41

          

exceptions);

42

         

if
 

(!name.equals(

"<init>"

)) {
43

          

AnnotationVisitor av0;

44

          

av0 = mv.visitAnnotation(

"Ljavax/jws/WebMethod;"

, 

true

);
45

          

av0.visitEnd();

46

         

}
47

         

return
 

mv;

48

     

}
49
 

50

  

@Override
51

  

public
 

void
 

visitEnd() {

52

    

cv.visitEnd();
53

  

}

54
 
55

 

}

最后，我们采用上面讲的类注入办法，即可得到一个带 @WebService 和 @WebMethod 标注的 class 文件。再通过 wsgen 工具即可成功创建 web services，该命令将会在 src 目录下生成 web services 描述文件：SayHelloImplService.wsdl，SayHelloImplService_schema1.xsd，同时生成了 2 个 JAX-WS 文件，分别为：SayHello.java，SayHelloResponse.java。

当然，在实际应用中，我们可能需要将一个 class 的某些特定方法发布为 web services，这就需要我们对 Adapter 做进一步的改造，在 visitMethod() 方法中根据方法名称等参数做进一步的处理。

[结束语]()

本文简要介绍了 ASM 字节码工具及其在 web services 开发中的应用。web services 的使用已经越来越广泛， 但在改造遗留系统过程中会遇到不少问题，本文针对其中一个主要问题给出了解决办法，该方法无须对现有系统做大量的改动， 即可将现存的 Java bean 转化成 web services 。本文对那些正考虑迁移到 JAX-WS 编程模型上的项目有一定的参考价值。在基于 JAX-WS 标准的 web services 的开发中，不少实际场景都是希望采用自底向上的开发方式， 即基于已有的 Java bean 来创建 web services 。WebSphere Application Server ( 以下简称 WAS ) 提供了命令行的工具 wsgen 和相对应的 Ant task 来支持这种开发过程，而且这两个工具比较适合大型项目的自动化构建。 这两个工具的使用前提是 Java bean 中事先添加有 web services 的 Annotation 标注，而在现有的业务系统中，class 文件一般是不带 Annotation 的，这就需要开发人员去修改现有的代码，以手工方式添加 Annotation，但这样带来的工作量太大且容易出错。本文介绍了一种解决途径，可以不用修改源代码，而是利用字节码工具 ASM 直接修改 class 文件， 在字节码文件中自动注入 Annotation ，然后再利用 wsgen 工具就可以很方便地生成 web services 应用。 本文同时也总结了使用 ASM 的一些实践。

本文首先介绍了和 ASM 使用相关的四种文件，以及它们之间的相互转化。然后结合 web services 开发实例，介绍了使用 wsgen 开发时遇到的实际问题，如何写一个 ASM 的适配器去修改现有 class，最终如何产生 web services 文件。

ASM 是一个多用途的 Java 字节码操控和分析框架。它能以二进制的形式直接修改现有的类或动态生成的类。 ASM 提供了常用的转换和分析算法，是一种允许用户方便的组装定制化的复杂转换和代码分析工具。 ASM 也提供和其它的字节码工具类似的功能，但 ASM 的重点是使用的简单和高性能。由于 ASM 在设计实现的时候尽可能 的小的内存占用和提供更高的性能；因此在动态系统中使用 ASM 是很有优势的。ASM 作为一种轻量级、高性能的 Java 字节码操控和分析框架，设计了一种更有效的方法、提供更好的性能和内存占用。今天， ASM 在许多领域都有应用，并已成为事实上的字节码处理框架标准。

[问题的引入]()

先写一个不带 web services 标注的 SayHelloImpl 类。
[**清单 1. 不带 web services 的 SayHelloImpl 类**]()1

public
 

class
 

SayHelloImpl {

2

 

public
 

String sayHello(String s) {
3

   

return
 

“Hello: ” + s ;

4

 

}
5

}

再用 wsgen 来创建 web services，wsgen 的使用如下：
[**清单 2. wsgen 实例**]()1

wsgen.exe – 

cp
 

bin – s output – d output – r output – wsdl ibm.was.asm.SayHelloImpl我们将会遇到如下错误： 
[**清单 3. 根据不带 web services 标注的类创建 web services 时遇到的错误信息**]() 
注释处理过程中遇到问题；

......
com.sun.tools.internal.ws.processor.modeler.ModelerException: [failed to localiz

e] A web service endpoint could not be found()
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.o

nError(WebServiceAP.java:215)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.b

uildModel(WebServiceAP.java:322)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.p

rocess(WebServiceAP.java:256)
        at com.sun.mirror.apt.AnnotationProcessors$CompositeAnnotationProcessor.

process(AnnotationProcessors.java:60)

注意：我们通常是在 class 前使用 @WebService， 在方法前使用 @WebMethod 来将一个类标注为 web services 的。WAS 6.1 的 web services 功能部件包和 WAS 7 都支持基于 JAX-WS 标准 的 web services 的开发， 在 WAS 安装目录的 bin 目录下提供了命令行工具 wsgen，wsgen 支持自底向上的开发方式。 下面是 wsgen 的用法：
[**清单 4. wsgen 命令格式**]() 
注 wsgen [options]  [SEI]

主要选项：

* -d 指定生成的 class 文件位置；
* -s 指定生成的 Java 源文件位置；
* -r 指定生成的 resources 文件位置，如 wsdl，xsd；
* -cp 指定服务实现类所在的位置；
* -wsdl，-servicename，-portname 三个参数指定生成的 wsdl 文件中的 service 和 port 的名称。

SEI(Service Endpoint Interface) 是一个 endpoint implementation class，不能是一个 interface。所以首先要开发一个 endpoint 的实现类，如本例中的 SayHelloImpl 。用 @WebService 声明 web services ，然后将它编译，才能提供给 wsgen 来创建 web services 。

[四种文件之间的转换]()

下面介绍本文要用到的四个概念（Java Source，Java Class，ASM Code，ASM Source）

* Java Source: 即我们通常编写的 Java 源文件；
* Java Class: Java 源文件编译后的字节码文件；
* ASM Source : 类似于对 class 反编译后的源文件，也就是 "textual byte code"，但比原始的 Java Source 可读性要差， 参见清单 5。
* ASM Code: 指读写 class 文件的 ASM 程序代码。需要用到 ASM 提供的 API， 参见清单 6。
[**清单 5. SayHelloImpl 类对应的 ASM Source**]()01

// class version 50.0 (50)

02

// access flags 33
03

public
 

class
 

com/ibm/was/asm/SayHelloImpl {

04
 
05

 

// access flags 1

06

 

public
  

<init>()V
07

   

ALOAD 

0

08

   

INVOKESPECIAL java/lang/Object. <init> ()V
09

   

RETURN

10

   

MAXSTACK = 

1
11

   

MAXLOCALS = 

1

12
 
13

 

// access flags 1

14

 

public
 

sayHello(Ljava/lang/String;)Ljava/lang/String;
15

   

NEW java/lang/StringBuilder

16

   

DUP
17

   

LDC 

"Hello:"

18

   

INVOKESPECIAL java/lang/StringBuilder. <init> (Ljava/lang/String;)V
19

   

ALOAD 

1

20

   

INVOKEVIRTUAL java/lang/StringBuilder.append (Ljava/lang/String;)
21

   

Ljava/lang/StringBuilder;

22

   

INVOKEVIRTUAL java/lang/StringBuilder.toString ()Ljava/lang/String;
23

   

ARETURN

24

   

MAXSTACK = 

3
25

   

MAXLOCALS = 

2

26

}[**清单 6. 生成 SayHelloImpl.class 的 ASM Code**]()01

package
 

asm.com.ibm.was.asm;

02

import
 

java.util.*;
03

import
 

org.objectweb.asm.*;

04

import
 

org.objectweb.asm.attrs.*;
05

public
 

class
 

SayHelloImplDump 

implements
 

Opcodes {

06

 

public
 

static
 

byte

[] dump () 

throws
 

Exception {
07

 

ClassWriter cw = 

new
 

ClassWriter(

0

);

08

 

FieldVisitor fv;
09

 

MethodVisitor mv;

10

 

AnnotationVisitor av0;
11
 

12

 

cw.visit(V1_6, ACC_PUBLIC + ACC_SUPER, 

"com/ibm/was/asm/SayHelloImpl"

,
13

 

null

, 

"java/lang/Object"

, 

null

);

14
 
15

 

{

16

 

mv = cw.visitMethod(ACC_PUBLIC, 

"<init>"

, 

"()V"

, 

null

, 

null

);
17

 

mv.visitCode();

18

 

mv.visitVarInsn(ALOAD, 

0

);
19

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/Object"

, 

"<init>"

, 

"()V"

);

20

 

mv.visitInsn(RETURN);
21

 

mv.visitMaxs(

1

, 

1

);

22

 

mv.visitEnd();
23

 

}

24

 

{
25

 

mv = cw.visitMethod(ACC_PUBLIC, 

"sayHello"

,

26

  

"(Ljava/lang/String;)Ljava/lang/String;"

, 

null

, 

null

);
27

 

mv.visitCode();

28

 

mv.visitTypeInsn(NEW, 

"java/lang/StringBuilder"

);
29

 

mv.visitInsn(DUP);

30

 

mv.visitLdcInsn(

"Hello:"

);
31

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/StringBuilder"

,

32

 

"<init>"

, 

"(Ljava/lang/String;)V"

);
33

 

mv.visitVarInsn(ALOAD, 

1

);

34

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
35

 

"append"

, 

"(Ljava/lang/String;)Ljava/lang/StringBuilder;"

);

36

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
37

 

"toString"

, 

"()Ljava/lang/String;"

);

38

 

mv.visitInsn(ARETURN);
39

 

mv.visitMaxs(

3

, 

2

);

40

 

mv.visitEnd();
41

 

}

42

 

cw.visitEnd();
43

 

return
 

cw.toByteArray();

44

 

}
45

}

图 1 描述了 Java Source 文件、Java Class 文件、ASM Code 文件和 ASM Source 文件之间的转换关系。
[**图 1. 四种文件的转换图**]() 
[![图 1. 四种文件的转换图]()]() 

如图 -1 所示，Java Source 文件通过 Javac 可以转换为 Java Class 文件。而相应的 Java Class 文件通过 Java 反编译器工具可转换为 Java Source 文件；

通 过 ASM Code 去创建和修改 Java Class 需要对 ASM API 比较熟悉才行，一个常见的问题时，怎么用 ASM Code 生成一个我们希望的 class 文件， 也就是说， 给定了 Java Class， 怎么得到其对应的 ASM Code 呢？ 所幸的是， ASM 框架为我们提供了 ASMifierClassVisitor 工具来产生 Java Class 对应的 ASM Code。代码如下：
[**清单 7. 给定 class 文件，通过 ASMifierClassVisitor 获得其对应的 ASM Code**]()[view source](http://www.oschina.net/question/129540_28430#viewSource "view source")[print](http://www.oschina.net/question/129540_28430#printSource "print")[?](http://www.oschina.net/question/129540_28430#about "?")

01

public
 

void
 

showClassASMProcess() {

02

  

ClassReader cr;
03

  

final
 

String n = SayHelloImpl.

class

.getName();

04

       
 
05

  

try

{

06

    

cr = 

new
 

ClassReader(n);                  
07

    

cr.accept(

new
 

ASMifierClassVisitor(

new

PrintWriter(System.out)),

08

    

new
 

Attribute[

0

],
09

    

ClassReader.SKIP_DEBUG);

10

               
 
11

  

} 

catch
 

(Exception e) {

12

    

e.printStackTrace();
13

 

}

14

}

另外一个重要的转换就是， 给定了 Java Class 文件， 如何查看它的 ASM Source ？ 这点对于我们验证 ASM Code 生成的 class 是否正确很有用。ASM 提供了另外一个工具 TraceClassVisitor， 来获得一个 Java Class 对应的 ASM Source。代码如下：
[**清单 8. 利用 TraceClassVisitor 查看 class 文件的 ASM Source**]()01

//file 是 class 文件的全路径名

02

public
 

void
 

showClassSource(String file) {
03

  

FileInputStream is ;

04

  

ClassReader cr; 
05

       
 

06

  

try
 

{
07

      

is = 

new
 

FileInputStream(file);     

08

      

cr = 

new
 

ClassReader(is);
09

      

TraceClassVisitor trace = 

new
 

TraceClassVisitor(

new

PrintWriter(System.out));

10

      

cr.accept(trace, ClassReader.SKIP_DEBUG);
11

           
 

12

      

is.close() ;
13

  

} 

catch
 

(Exception e) {

14

      

e.printStackTrace() ;
15

 

}

16

}
17

{

18

    

e.printStackTrace();
19

 

}

20

}

[ASM 类注入]()

ASM 类注入是指修改一个现有的 class 文件，在其中加入自己的代码。按通常思维，我们需要利用 ASM 提供的 reader 类去读取所要修改的类文件， 找到要修改的地方，如方法名或属性名，然后在该处插入自己的代码或修改现有的代码，这种思路将使问题复杂化。 ASM 为我们提供了访问类文件的 Visitor 模式，来遍历一个 class 文件，Visitor 是一个实现 ClassVisitor 接口的类。 要注入一个 class 文件，先要找到一个合适的 Visitor 做向导， ASM 提供了好几种 Visitor， 最常用的是 ClassWriter。同时我们需要提供一个适配器类 ClassAdapter， Visitor 会带着这个 Adapter 一起去遍历， 然后在遍历过程中回调 Adapter 提供的方法。我们在 Adapter 的这些方法中就可以实现我们的修改和定制逻辑。当然，如果你不想做任何修改，那 Visitor 遍历完后将得到一个和被遍历的 class 完全一样的拷贝。
[**清单 9. 类注入的完整过程**]()01

public
 

void
 

addAnnotationToExistingClass() {

02

 

FileInputStream is ;
03

 

ClassReader cr;          

04

 

try
 

{        
05

   

String classfile = 

"E:\\SayHelloImpl.class"
 

; 

// 待遍历的类

06

   

showClassSource(classfile) ;  

// 打印该类的 ASM source
07

           
 

08

   

is = 

new
 

FileInputStream(classfile);       
09

   

cr = 

new
 

ClassReader(is);

10

   

// 此处我们使用 ClassWriter 做 Visitor
11

   

ClassWriter cw = 

new
 

ClassWriter(ClassWriter.COMPUTE_MAXS);

12

// 给 Visitor 提供一个 Adapter
13

   

AddAnnotationAdapter adapter = 

new
 

AddAnnotationAdapter(cw);           

14

   

cr.accept(adapter, ClassReader.SKIP_DEBUG);            
15

   
 

16

   

// 遍历完后，生成的 class 保存在字节数组中
17

   

byte

[] b = cw.toByteArray();

18

           
 
19

   

try
 

{

20

        

// 将字节数组输出到文件中
21

        

// 为了不至于覆盖掉原有的 SayHelloImpl.class 文件，我们将结果输出到 _SayHelloImpl.class

22

     

FileOutputStream fout = 

new
 

FileOutputStream(

"E:\\_SayHelloImpl.class"

);
23

     

fout.write(b) ;

24

 

fout.flush();
25

     

fout.close() ;

26

   

} 

catch
 

(Exception e) {
27

     

e.printStackTrace() ;

28

   

}
29

           
 

30

   

// 验证 _SayHelloImpl.class 是否包含我们注入的代码
31

   

showClassSource(

"E:\\_SayHelloImpl.class"

) ;

32

} 

catch
 

(Exception e) {
33

     

e.printStackTrace();

34

} 
35

}

[web services 标记注入过程]()

我们已经知道了如何往 class 中注入自己的代码，那对于一个已有的 Java bean，怎么往里注入 @WebService 和 @WebMethod 呢？根据上面我们讲的转换关系，我们可以先写一个带 annotation 的 Java Source，编译得到 Java Class， 再得到其 ASM Code。下面是利用 ASM 给一个不带任何 Annotation 的 class 添加 @WebService 和 @WebMethod 的步骤。

1. 首先在 SayHelloImpl.java 中添加 @WebService 和 @WebMethod： 
[**清单 10. 带 web services 标注的 SayHelloImpl 类**]()1

@WebService

2

public
 

class
 

SayHelloImpl {
3

 

@WebMethod

4

 

public
 

String sayHello(String s) {
5

   

return
 

"Hello: "
 

+ s ;

6

 

}
7

}
1. 通过 ASMifierClassVisitor 工具获得 SayHelloImpl class 的 ASM Code，见清单 6。
1. 写一个 ClassAdapter 类， ClassAdapter 其实也是一个 Visitor。根据 ASM Code 的提示， 我们就可以知道如何利用 ASM API 去获得我们希望的 class 文件。下面是 Adapter 的示例代码。 
[**清单 11. 添加 Annotation 的 Adapter 类**]()01

public
 

class
 

AddAnnotationAdapter 

extends
 

ClassAdapter 

implements
 

Opcodes{

02

  

//private String annotationDesc;
03

  

private
 

boolean
 

isAnnotationPresent;

04
 
05

  

public
 

AddAnnotationAdapter(ClassVisitor cv) {

06

     

super

(cv);
07

  

}

08
 
09

  

@Override

10

  

public
 

void
 

visit(

int
 

version, 

int
 

access, String name, String signature,
11

    

String superName, String[] interfaces) {

12

        
 
13

    

cv.visit(V1_6, ACC_PUBLIC + ACC_SUPER, name, signature, superName,

14

    

interfaces);
15

    

AnnotationVisitor av0;

16

    

av0 = cv.visitAnnotation(

"Ljavax/jws/WebService;"

, 

true

);
17

    

av0.visitEnd();

18

  

}
19
 

20

  

@Override
21

  

public
 

AnnotationVisitor visitAnnotation(String desc, 

boolean
 

visible) {

22

    

return
 

cv.visitAnnotation(desc, visible);
23

  

}

24
 
25

  

@Override

26

  

public
 

void
 

visitInnerClass(String name, String outerName,
27

    

String innerName, 

int
 

access) {

28

    

cv.visitInnerClass(name, outerName, innerName, access);
29

  

}

30
 
31

  

@Override

32

  

public
 

FieldVisitor visitField(

int
 

access, String name, String desc,
33

    

String signature, Object value) {

34

    

return
 

cv.visitField(access, name, desc, signature, value);
35

  

}

36
 
37

  

@Override

38

  

public
 

MethodVisitor visitMethod(

int
 

access, String name, String desc,
39

    

String signature, String[] exceptions) {   

40

         

MethodVisitor mv  = cv.visitMethod(access, name, desc, signature,
41

          

exceptions);

42

         

if
 

(!name.equals(

"<init>"

)) {
43

          

AnnotationVisitor av0;

44

          

av0 = mv.visitAnnotation(

"Ljavax/jws/WebMethod;"

, 

true

);
45

          

av0.visitEnd();

46

         

}
47

         

return
 

mv;

48

     

}
49
 

50

  

@Override
51

  

public
 

void
 

visitEnd() {

52

    

cv.visitEnd();
53

  

}

54
 
55

 

}

最后，我们采用上面讲的类注入办法，即可得到一个带 @WebService 和 @WebMethod 标注的 class 文件。再通过 wsgen 工具即可成功创建 web services，该命令将会在 src 目录下生成 web services 描述文件：SayHelloImplService.wsdl，SayHelloImplService_schema1.xsd，同时生成了 2 个 JAX-WS 文件，分别为：SayHello.java，SayHelloResponse.java。

当然，在实际应用中，我们可能需要将一个 class 的某些特定方法发布为 web services，这就需要我们对 Adapter 做进一步的改造，在 visitMethod() 方法中根据方法名称等参数做进一步的处理。

[结束语]()

本文简要介绍了 ASM 字节码工具及其在 web services 开发中的应用。web services 的使用已经越来越广泛， 但在改造遗留系统过程中会遇到不少问题，本文针对其中一个主要问题给出了解决办法，该方法无须对现有系统做大量的改动， 即可将现存的 Java bean 转化成 web services 。本文对那些正考虑迁移到 JAX-WS 编程模型上的项目有一定的参考价值。在基于 JAX-WS 标准的 web services 的开发中，不少实际场景都是希望采用自底向上的开发方式， 即基于已有的 Java bean 来创建 web services 。WebSphere Application Server ( 以下简称 WAS ) 提供了命令行的工具 wsgen 和相对应的 Ant task 来支持这种开发过程，而且这两个工具比较适合大型项目的自动化构建。 这两个工具的使用前提是 Java bean 中事先添加有 web services 的 Annotation 标注，而在现有的业务系统中，class 文件一般是不带 Annotation 的，这就需要开发人员去修改现有的代码，以手工方式添加 Annotation，但这样带来的工作量太大且容易出错。本文介绍了一种解决途径，可以不用修改源代码，而是利用字节码工具 ASM 直接修改 class 文件， 在字节码文件中自动注入 Annotation ，然后再利用 wsgen 工具就可以很方便地生成 web services 应用。 本文同时也总结了使用 ASM 的一些实践。

本文首先介绍了和 ASM 使用相关的四种文件，以及它们之间的相互转化。然后结合 web services 开发实例，介绍了使用 wsgen 开发时遇到的实际问题，如何写一个 ASM 的适配器去修改现有 class，最终如何产生 web services 文件。

ASM 是一个多用途的 Java 字节码操控和分析框架。它能以二进制的形式直接修改现有的类或动态生成的类。 ASM 提供了常用的转换和分析算法，是一种允许用户方便的组装定制化的复杂转换和代码分析工具。 ASM 也提供和其它的字节码工具类似的功能，但 ASM 的重点是使用的简单和高性能。由于 ASM 在设计实现的时候尽可能 的小的内存占用和提供更高的性能；因此在动态系统中使用 ASM 是很有优势的。ASM 作为一种轻量级、高性能的 Java 字节码操控和分析框架，设计了一种更有效的方法、提供更好的性能和内存占用。今天， ASM 在许多领域都有应用，并已成为事实上的字节码处理框架标准。

[问题的引入]()

先写一个不带 web services 标注的 SayHelloImpl 类。
[**清单 1. 不带 web services 的 SayHelloImpl 类**]()1

public
 

class
 

SayHelloImpl {

2

 

public
 

String sayHello(String s) {
3

   

return
 

“Hello: ” + s ;

4

 

}
5

}

再用 wsgen 来创建 web services，wsgen 的使用如下：
[**清单 2. wsgen 实例**]()1

wsgen.exe – 

cp
 

bin – s output – d output – r output – wsdl ibm.was.asm.SayHelloImpl我们将会遇到如下错误： 
[**清单 3. 根据不带 web services 标注的类创建 web services 时遇到的错误信息**]() 
注释处理过程中遇到问题；

......
com.sun.tools.internal.ws.processor.modeler.ModelerException: [failed to localiz

e] A web service endpoint could not be found()
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.o

nError(WebServiceAP.java:215)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.b

uildModel(WebServiceAP.java:322)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.p

rocess(WebServiceAP.java:256)
        at com.sun.mirror.apt.AnnotationProcessors$CompositeAnnotationProcessor.

process(AnnotationProcessors.java:60)

注意：我们通常是在 class 前使用 @WebService， 在方法前使用 @WebMethod 来将一个类标注为 web services 的。WAS 6.1 的 web services 功能部件包和 WAS 7 都支持基于 JAX-WS 标准 的 web services 的开发， 在 WAS 安装目录的 bin 目录下提供了命令行工具 wsgen，wsgen 支持自底向上的开发方式。 下面是 wsgen 的用法：
[**清单 4. wsgen 命令格式**]() 
注 wsgen [options]  [SEI]

主要选项：

* -d 指定生成的 class 文件位置；
* -s 指定生成的 Java 源文件位置；
* -r 指定生成的 resources 文件位置，如 wsdl，xsd；
* -cp 指定服务实现类所在的位置；
* -wsdl，-servicename，-portname 三个参数指定生成的 wsdl 文件中的 service 和 port 的名称。

SEI(Service Endpoint Interface) 是一个 endpoint implementation class，不能是一个 interface。所以首先要开发一个 endpoint 的实现类，如本例中的 SayHelloImpl 。用 @WebService 声明 web services ，然后将它编译，才能提供给 wsgen 来创建 web services 。

[四种文件之间的转换]()

下面介绍本文要用到的四个概念（Java Source，Java Class，ASM Code，ASM Source）

* Java Source: 即我们通常编写的 Java 源文件；
* Java Class: Java 源文件编译后的字节码文件；
* ASM Source : 类似于对 class 反编译后的源文件，也就是 "textual byte code"，但比原始的 Java Source 可读性要差， 参见清单 5。
* ASM Code: 指读写 class 文件的 ASM 程序代码。需要用到 ASM 提供的 API， 参见清单 6。
[**清单 5. SayHelloImpl 类对应的 ASM Source**]()01

// class version 50.0 (50)

02

// access flags 33
03

public
 

class
 

com/ibm/was/asm/SayHelloImpl {

04
 
05

 

// access flags 1

06

 

public
  

<init>()V
07

   

ALOAD 

0

08

   

INVOKESPECIAL java/lang/Object. <init> ()V
09

   

RETURN

10

   

MAXSTACK = 

1
11

   

MAXLOCALS = 

1

12
 
13

 

// access flags 1

14

 

public
 

sayHello(Ljava/lang/String;)Ljava/lang/String;
15

   

NEW java/lang/StringBuilder

16

   

DUP
17

   

LDC 

"Hello:"

18

   

INVOKESPECIAL java/lang/StringBuilder. <init> (Ljava/lang/String;)V
19

   

ALOAD 

1

20

   

INVOKEVIRTUAL java/lang/StringBuilder.append (Ljava/lang/String;)
21

   

Ljava/lang/StringBuilder;

22

   

INVOKEVIRTUAL java/lang/StringBuilder.toString ()Ljava/lang/String;
23

   

ARETURN

24

   

MAXSTACK = 

3
25

   

MAXLOCALS = 

2

26

}[**清单 6. 生成 SayHelloImpl.class 的 ASM Code**]()01

package
 

asm.com.ibm.was.asm;

02

import
 

java.util.*;
03

import
 

org.objectweb.asm.*;

04

import
 

org.objectweb.asm.attrs.*;
05

public
 

class
 

SayHelloImplDump 

implements
 

Opcodes {

06

 

public
 

static
 

byte

[] dump () 

throws
 

Exception {
07

 

ClassWriter cw = 

new
 

ClassWriter(

0

);

08

 

FieldVisitor fv;
09

 

MethodVisitor mv;

10

 

AnnotationVisitor av0;
11
 

12

 

cw.visit(V1_6, ACC_PUBLIC + ACC_SUPER, 

"com/ibm/was/asm/SayHelloImpl"

,
13

 

null

, 

"java/lang/Object"

, 

null

);

14
 
15

 

{

16

 

mv = cw.visitMethod(ACC_PUBLIC, 

"<init>"

, 

"()V"

, 

null

, 

null

);
17

 

mv.visitCode();

18

 

mv.visitVarInsn(ALOAD, 

0

);
19

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/Object"

, 

"<init>"

, 

"()V"

);

20

 

mv.visitInsn(RETURN);
21

 

mv.visitMaxs(

1

, 

1

);

22

 

mv.visitEnd();
23

 

}

24

 

{
25

 

mv = cw.visitMethod(ACC_PUBLIC, 

"sayHello"

,

26

  

"(Ljava/lang/String;)Ljava/lang/String;"

, 

null

, 

null

);
27

 

mv.visitCode();

28

 

mv.visitTypeInsn(NEW, 

"java/lang/StringBuilder"

);
29

 

mv.visitInsn(DUP);

30

 

mv.visitLdcInsn(

"Hello:"

);
31

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/StringBuilder"

,

32

 

"<init>"

, 

"(Ljava/lang/String;)V"

);
33

 

mv.visitVarInsn(ALOAD, 

1

);

34

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
35

 

"append"

, 

"(Ljava/lang/String;)Ljava/lang/StringBuilder;"

);

36

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
37

 

"toString"

, 

"()Ljava/lang/String;"

);

38

 

mv.visitInsn(ARETURN);
39

 

mv.visitMaxs(

3

, 

2

);

40

 

mv.visitEnd();
41

 

}

42

 

cw.visitEnd();
43

 

return
 

cw.toByteArray();

44

 

}
45

}

图 1 描述了 Java Source 文件、Java Class 文件、ASM Code 文件和 ASM Source 文件之间的转换关系。
[**图 1. 四种文件的转换图**]() 
[![图 1. 四种文件的转换图]()]() 

如图 -1 所示，Java Source 文件通过 Javac 可以转换为 Java Class 文件。而相应的 Java Class 文件通过 Java 反编译器工具可转换为 Java Source 文件；

通 过 ASM Code 去创建和修改 Java Class 需要对 ASM API 比较熟悉才行，一个常见的问题时，怎么用 ASM Code 生成一个我们希望的 class 文件， 也就是说， 给定了 Java Class， 怎么得到其对应的 ASM Code 呢？ 所幸的是， ASM 框架为我们提供了 ASMifierClassVisitor 工具来产生 Java Class 对应的 ASM Code。代码如下：
[**清单 7. 给定 class 文件，通过 ASMifierClassVisitor 获得其对应的 ASM Code**]()[view source](http://www.oschina.net/question/129540_28430#viewSource "view source")[print](http://www.oschina.net/question/129540_28430#printSource "print")[?](http://www.oschina.net/question/129540_28430#about "?")

01

public
 

void
 

showClassASMProcess() {

02

  

ClassReader cr;
03

  

final
 

String n = SayHelloImpl.

class

.getName();

04

       
 
05

  

try

{

06

    

cr = 

new
 

ClassReader(n);                  
07

    

cr.accept(

new
 

ASMifierClassVisitor(

new

PrintWriter(System.out)),

08

    

new
 

Attribute[

0

],
09

    

ClassReader.SKIP_DEBUG);

10

               
 
11

  

} 

catch
 

(Exception e) {

12

    

e.printStackTrace();
13

 

}

14

}

另外一个重要的转换就是， 给定了 Java Class 文件， 如何查看它的 ASM Source ？ 这点对于我们验证 ASM Code 生成的 class 是否正确很有用。ASM 提供了另外一个工具 TraceClassVisitor， 来获得一个 Java Class 对应的 ASM Source。代码如下：
[**清单 8. 利用 TraceClassVisitor 查看 class 文件的 ASM Source**]()01

//file 是 class 文件的全路径名

02

public
 

void
 

showClassSource(String file) {
03

  

FileInputStream is ;

04

  

ClassReader cr; 
05

       
 

06

  

try
 

{
07

      

is = 

new
 

FileInputStream(file);     

08

      

cr = 

new
 

ClassReader(is);
09

      

TraceClassVisitor trace = 

new
 

TraceClassVisitor(

new

PrintWriter(System.out));

10

      

cr.accept(trace, ClassReader.SKIP_DEBUG);
11

           
 

12

      

is.close() ;
13

  

} 

catch
 

(Exception e) {

14

      

e.printStackTrace() ;
15

 

}

16

}
17

{

18

    

e.printStackTrace();
19

 

}

20

}

[ASM 类注入]()

ASM 类注入是指修改一个现有的 class 文件，在其中加入自己的代码。按通常思维，我们需要利用 ASM 提供的 reader 类去读取所要修改的类文件， 找到要修改的地方，如方法名或属性名，然后在该处插入自己的代码或修改现有的代码，这种思路将使问题复杂化。 ASM 为我们提供了访问类文件的 Visitor 模式，来遍历一个 class 文件，Visitor 是一个实现 ClassVisitor 接口的类。 要注入一个 class 文件，先要找到一个合适的 Visitor 做向导， ASM 提供了好几种 Visitor， 最常用的是 ClassWriter。同时我们需要提供一个适配器类 ClassAdapter， Visitor 会带着这个 Adapter 一起去遍历， 然后在遍历过程中回调 Adapter 提供的方法。我们在 Adapter 的这些方法中就可以实现我们的修改和定制逻辑。当然，如果你不想做任何修改，那 Visitor 遍历完后将得到一个和被遍历的 class 完全一样的拷贝。
[**清单 9. 类注入的完整过程**]()01

public
 

void
 

addAnnotationToExistingClass() {

02

 

FileInputStream is ;
03

 

ClassReader cr;          

04

 

try
 

{        
05

   

String classfile = 

"E:\\SayHelloImpl.class"
 

; 

// 待遍历的类

06

   

showClassSource(classfile) ;  

// 打印该类的 ASM source
07

           
 

08

   

is = 

new
 

FileInputStream(classfile);       
09

   

cr = 

new
 

ClassReader(is);

10

   

// 此处我们使用 ClassWriter 做 Visitor
11

   

ClassWriter cw = 

new
 

ClassWriter(ClassWriter.COMPUTE_MAXS);

12

// 给 Visitor 提供一个 Adapter
13

   

AddAnnotationAdapter adapter = 

new
 

AddAnnotationAdapter(cw);           

14

   

cr.accept(adapter, ClassReader.SKIP_DEBUG);            
15

   
 

16

   

// 遍历完后，生成的 class 保存在字节数组中
17

   

byte

[] b = cw.toByteArray();

18

           
 
19

   

try
 

{

20

        

// 将字节数组输出到文件中
21

        

// 为了不至于覆盖掉原有的 SayHelloImpl.class 文件，我们将结果输出到 _SayHelloImpl.class

22

     

FileOutputStream fout = 

new
 

FileOutputStream(

"E:\\_SayHelloImpl.class"

);
23

     

fout.write(b) ;

24

 

fout.flush();
25

     

fout.close() ;

26

   

} 

catch
 

(Exception e) {
27

     

e.printStackTrace() ;

28

   

}
29

           
 

30

   

// 验证 _SayHelloImpl.class 是否包含我们注入的代码
31

   

showClassSource(

"E:\\_SayHelloImpl.class"

) ;

32

} 

catch
 

(Exception e) {
33

     

e.printStackTrace();

34

} 
35

}

[web services 标记注入过程]()

我们已经知道了如何往 class 中注入自己的代码，那对于一个已有的 Java bean，怎么往里注入 @WebService 和 @WebMethod 呢？根据上面我们讲的转换关系，我们可以先写一个带 annotation 的 Java Source，编译得到 Java Class， 再得到其 ASM Code。下面是利用 ASM 给一个不带任何 Annotation 的 class 添加 @WebService 和 @WebMethod 的步骤。

1. 首先在 SayHelloImpl.java 中添加 @WebService 和 @WebMethod： 
[**清单 10. 带 web services 标注的 SayHelloImpl 类**]()1

@WebService

2

public
 

class
 

SayHelloImpl {
3

 

@WebMethod

4

 

public
 

String sayHello(String s) {
5

   

return
 

"Hello: "
 

+ s ;

6

 

}
7

}
1. 通过 ASMifierClassVisitor 工具获得 SayHelloImpl class 的 ASM Code，见清单 6。
1. 写一个 ClassAdapter 类， ClassAdapter 其实也是一个 Visitor。根据 ASM Code 的提示， 我们就可以知道如何利用 ASM API 去获得我们希望的 class 文件。下面是 Adapter 的示例代码。 
[**清单 11. 添加 Annotation 的 Adapter 类**]()01

public
 

class
 

AddAnnotationAdapter 

extends
 

ClassAdapter 

implements
 

Opcodes{

02

  

//private String annotationDesc;
03

  

private
 

boolean
 

isAnnotationPresent;

04
 
05

  

public
 

AddAnnotationAdapter(ClassVisitor cv) {

06

     

super

(cv);
07

  

}

08
 
09

  

@Override

10

  

public
 

void
 

visit(

int
 

version, 

int
 

access, String name, String signature,
11

    

String superName, String[] interfaces) {

12

        
 
13

    

cv.visit(V1_6, ACC_PUBLIC + ACC_SUPER, name, signature, superName,

14

    

interfaces);
15

    

AnnotationVisitor av0;

16

    

av0 = cv.visitAnnotation(

"Ljavax/jws/WebService;"

, 

true

);
17

    

av0.visitEnd();

18

  

}
19
 

20

  

@Override
21

  

public
 

AnnotationVisitor visitAnnotation(String desc, 

boolean
 

visible) {

22

    

return
 

cv.visitAnnotation(desc, visible);
23

  

}

24
 
25

  

@Override

26

  

public
 

void
 

visitInnerClass(String name, String outerName,
27

    

String innerName, 

int
 

access) {

28

    

cv.visitInnerClass(name, outerName, innerName, access);
29

  

}

30
 
31

  

@Override

32

  

public
 

FieldVisitor visitField(

int
 

access, String name, String desc,
33

    

String signature, Object value) {

34

    

return
 

cv.visitField(access, name, desc, signature, value);
35

  

}

36
 
37

  

@Override

38

  

public
 

MethodVisitor visitMethod(

int
 

access, String name, String desc,
39

    

String signature, String[] exceptions) {   

40

         

MethodVisitor mv  = cv.visitMethod(access, name, desc, signature,
41

          

exceptions);

42

         

if
 

(!name.equals(

"<init>"

)) {
43

          

AnnotationVisitor av0;

44

          

av0 = mv.visitAnnotation(

"Ljavax/jws/WebMethod;"

, 

true

);
45

          

av0.visitEnd();

46

         

}
47

         

return
 

mv;

48

     

}
49
 

50

  

@Override
51

  

public
 

void
 

visitEnd() {

52

    

cv.visitEnd();
53

  

}

54
 
55

 

}

最后，我们采用上面讲的类注入办法，即可得到一个带 @WebService 和 @WebMethod 标注的 class 文件。再通过 wsgen 工具即可成功创建 web services，该命令将会在 src 目录下生成 web services 描述文件：SayHelloImplService.wsdl，SayHelloImplService_schema1.xsd，同时生成了 2 个 JAX-WS 文件，分别为：SayHello.java，SayHelloResponse.java。

当然，在实际应用中，我们可能需要将一个 class 的某些特定方法发布为 web services，这就需要我们对 Adapter 做进一步的改造，在 visitMethod() 方法中根据方法名称等参数做进一步的处理。

[结束语]()

本文简要介绍了 ASM 字节码工具及其在 web services 开发中的应用。web services 的使用已经越来越广泛， 但在改造遗留系统过程中会遇到不少问题，本文针对其中一个主要问题给出了解决办法，该方法无须对现有系统做大量的改动， 即可将现存的 Java bean 转化成 web services 。本文对那些正考虑迁移到 JAX-WS 编程模型上的项目有一定的参考价值。在基于 JAX-WS 标准的 web services 的开发中，不少实际场景都是希望采用自底向上的开发方式， 即基于已有的 Java bean 来创建 web services 。WebSphere Application Server ( 以下简称 WAS ) 提供了命令行的工具 wsgen 和相对应的 Ant task 来支持这种开发过程，而且这两个工具比较适合大型项目的自动化构建。 这两个工具的使用前提是 Java bean 中事先添加有 web services 的 Annotation 标注，而在现有的业务系统中，class 文件一般是不带 Annotation 的，这就需要开发人员去修改现有的代码，以手工方式添加 Annotation，但这样带来的工作量太大且容易出错。本文介绍了一种解决途径，可以不用修改源代码，而是利用字节码工具 ASM 直接修改 class 文件， 在字节码文件中自动注入 Annotation ，然后再利用 wsgen 工具就可以很方便地生成 web services 应用。 本文同时也总结了使用 ASM 的一些实践。

本文首先介绍了和 ASM 使用相关的四种文件，以及它们之间的相互转化。然后结合 web services 开发实例，介绍了使用 wsgen 开发时遇到的实际问题，如何写一个 ASM 的适配器去修改现有 class，最终如何产生 web services 文件。

ASM 是一个多用途的 Java 字节码操控和分析框架。它能以二进制的形式直接修改现有的类或动态生成的类。 ASM 提供了常用的转换和分析算法，是一种允许用户方便的组装定制化的复杂转换和代码分析工具。 ASM 也提供和其它的字节码工具类似的功能，但 ASM 的重点是使用的简单和高性能。由于 ASM 在设计实现的时候尽可能 的小的内存占用和提供更高的性能；因此在动态系统中使用 ASM 是很有优势的。ASM 作为一种轻量级、高性能的 Java 字节码操控和分析框架，设计了一种更有效的方法、提供更好的性能和内存占用。今天， ASM 在许多领域都有应用，并已成为事实上的字节码处理框架标准。

[问题的引入]()

先写一个不带 web services 标注的 SayHelloImpl 类。
[**清单 1. 不带 web services 的 SayHelloImpl 类**]()1

public
 

class
 

SayHelloImpl {

2

 

public
 

String sayHello(String s) {
3

   

return
 

“Hello: ” + s ;

4

 

}
5

}

再用 wsgen 来创建 web services，wsgen 的使用如下：
[**清单 2. wsgen 实例**]()1

wsgen.exe – 

cp
 

bin – s output – d output – r output – wsdl ibm.was.asm.SayHelloImpl我们将会遇到如下错误： 
[**清单 3. 根据不带 web services 标注的类创建 web services 时遇到的错误信息**]() 
注释处理过程中遇到问题；

......
com.sun.tools.internal.ws.processor.modeler.ModelerException: [failed to localiz

e] A web service endpoint could not be found()
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.o

nError(WebServiceAP.java:215)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.b

uildModel(WebServiceAP.java:322)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.p

rocess(WebServiceAP.java:256)
        at com.sun.mirror.apt.AnnotationProcessors$CompositeAnnotationProcessor.

process(AnnotationProcessors.java:60)

注意：我们通常是在 class 前使用 @WebService， 在方法前使用 @WebMethod 来将一个类标注为 web services 的。WAS 6.1 的 web services 功能部件包和 WAS 7 都支持基于 JAX-WS 标准 的 web services 的开发， 在 WAS 安装目录的 bin 目录下提供了命令行工具 wsgen，wsgen 支持自底向上的开发方式。 下面是 wsgen 的用法：
[**清单 4. wsgen 命令格式**]() 
注 wsgen [options]  [SEI]

主要选项：

* -d 指定生成的 class 文件位置；
* -s 指定生成的 Java 源文件位置；
* -r 指定生成的 resources 文件位置，如 wsdl，xsd；
* -cp 指定服务实现类所在的位置；
* -wsdl，-servicename，-portname 三个参数指定生成的 wsdl 文件中的 service 和 port 的名称。

SEI(Service Endpoint Interface) 是一个 endpoint implementation class，不能是一个 interface。所以首先要开发一个 endpoint 的实现类，如本例中的 SayHelloImpl 。用 @WebService 声明 web services ，然后将它编译，才能提供给 wsgen 来创建 web services 。

[四种文件之间的转换]()

下面介绍本文要用到的四个概念（Java Source，Java Class，ASM Code，ASM Source）

* Java Source: 即我们通常编写的 Java 源文件；
* Java Class: Java 源文件编译后的字节码文件；
* ASM Source : 类似于对 class 反编译后的源文件，也就是 "textual byte code"，但比原始的 Java Source 可读性要差， 参见清单 5。
* ASM Code: 指读写 class 文件的 ASM 程序代码。需要用到 ASM 提供的 API， 参见清单 6。
[**清单 5. SayHelloImpl 类对应的 ASM Source**]()01

// class version 50.0 (50)

02

// access flags 33
03

public
 

class
 

com/ibm/was/asm/SayHelloImpl {

04
 
05

 

// access flags 1

06

 

public
  

<init>()V
07

   

ALOAD 

0

08

   

INVOKESPECIAL java/lang/Object. <init> ()V
09

   

RETURN

10

   

MAXSTACK = 

1
11

   

MAXLOCALS = 

1

12
 
13

 

// access flags 1

14

 

public
 

sayHello(Ljava/lang/String;)Ljava/lang/String;
15

   

NEW java/lang/StringBuilder

16

   

DUP
17

   

LDC 

"Hello:"

18

   

INVOKESPECIAL java/lang/StringBuilder. <init> (Ljava/lang/String;)V
19

   

ALOAD 

1

20

   

INVOKEVIRTUAL java/lang/StringBuilder.append (Ljava/lang/String;)
21

   

Ljava/lang/StringBuilder;

22

   

INVOKEVIRTUAL java/lang/StringBuilder.toString ()Ljava/lang/String;
23

   

ARETURN

24

   

MAXSTACK = 

3
25

   

MAXLOCALS = 

2

26

}[**清单 6. 生成 SayHelloImpl.class 的 ASM Code**]()01

package
 

asm.com.ibm.was.asm;

02

import
 

java.util.*;
03

import
 

org.objectweb.asm.*;

04

import
 

org.objectweb.asm.attrs.*;
05

public
 

class
 

SayHelloImplDump 

implements
 

Opcodes {

06

 

public
 

static
 

byte

[] dump () 

throws
 

Exception {
07

 

ClassWriter cw = 

new
 

ClassWriter(

0

);

08

 

FieldVisitor fv;
09

 

MethodVisitor mv;

10

 

AnnotationVisitor av0;
11
 

12

 

cw.visit(V1_6, ACC_PUBLIC + ACC_SUPER, 

"com/ibm/was/asm/SayHelloImpl"

,
13

 

null

, 

"java/lang/Object"

, 

null

);

14
 
15

 

{

16

 

mv = cw.visitMethod(ACC_PUBLIC, 

"<init>"

, 

"()V"

, 

null

, 

null

);
17

 

mv.visitCode();

18

 

mv.visitVarInsn(ALOAD, 

0

);
19

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/Object"

, 

"<init>"

, 

"()V"

);

20

 

mv.visitInsn(RETURN);
21

 

mv.visitMaxs(

1

, 

1

);

22

 

mv.visitEnd();
23

 

}

24

 

{
25

 

mv = cw.visitMethod(ACC_PUBLIC, 

"sayHello"

,

26

  

"(Ljava/lang/String;)Ljava/lang/String;"

, 

null

, 

null

);
27

 

mv.visitCode();

28

 

mv.visitTypeInsn(NEW, 

"java/lang/StringBuilder"

);
29

 

mv.visitInsn(DUP);

30

 

mv.visitLdcInsn(

"Hello:"

);
31

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/StringBuilder"

,

32

 

"<init>"

, 

"(Ljava/lang/String;)V"

);
33

 

mv.visitVarInsn(ALOAD, 

1

);

34

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
35

 

"append"

, 

"(Ljava/lang/String;)Ljava/lang/StringBuilder;"

);

36

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
37

 

"toString"

, 

"()Ljava/lang/String;"

);

38

 

mv.visitInsn(ARETURN);
39

 

mv.visitMaxs(

3

, 

2

);

40

 

mv.visitEnd();
41

 

}

42

 

cw.visitEnd();
43

 

return
 

cw.toByteArray();

44

 

}
45

}

图 1 描述了 Java Source 文件、Java Class 文件、ASM Code 文件和 ASM Source 文件之间的转换关系。
[**图 1. 四种文件的转换图**]() 
[![图 1. 四种文件的转换图]()]() 

如图 -1 所示，Java Source 文件通过 Javac 可以转换为 Java Class 文件。而相应的 Java Class 文件通过 Java 反编译器工具可转换为 Java Source 文件；

通 过 ASM Code 去创建和修改 Java Class 需要对 ASM API 比较熟悉才行，一个常见的问题时，怎么用 ASM Code 生成一个我们希望的 class 文件， 也就是说， 给定了 Java Class， 怎么得到其对应的 ASM Code 呢？ 所幸的是， ASM 框架为我们提供了 ASMifierClassVisitor 工具来产生 Java Class 对应的 ASM Code。代码如下：
[**清单 7. 给定 class 文件，通过 ASMifierClassVisitor 获得其对应的 ASM Code**]()01

public
 

void
 

showClassASMProcess() {

02

  

ClassReader cr;
03

  

final
 

String n = SayHelloImpl.

class

.getName();

04

       
 
05

  

try

{

06

    

cr = 

new
 

ClassReader(n);                  
07

    

cr.accept(

new
 

ASMifierClassVisitor(

new

PrintWriter(System.out)),

08

    

new
 

Attribute[

0

],
09

    

ClassReader.SKIP_DEBUG);

10

               
 
11

  

} 

catch
 

(Exception e) {

12

    

e.printStackTrace();
13

 

}

14

}

另外一个重要的转换就是， 给定了 Java Class 文件， 如何查看它的 ASM Source ？ 这点对于我们验证 ASM Code 生成的 class 是否正确很有用。ASM 提供了另外一个工具 TraceClassVisitor， 来获得一个 Java Class 对应的 ASM Source。代码如下：
[**清单 8. 利用 TraceClassVisitor 查看 class 文件的 ASM Source**]()01

//file 是 class 文件的全路径名

02

public
 

void
 

showClassSource(String file) {
03

  

FileInputStream is ;

04

  

ClassReader cr; 
05

       
 

06

  

try
 

{
07

      

is = 

new
 

FileInputStream(file);     

08

      

cr = 

new
 

ClassReader(is);
09

      

TraceClassVisitor trace = 

new
 

TraceClassVisitor(

new

PrintWriter(System.out));

10

      

cr.accept(trace, ClassReader.SKIP_DEBUG);
11

           
 

12

      

is.close() ;
13

  

} 

catch
 

(Exception e) {

14

      

e.printStackTrace() ;
15

 

}

16

}
17

{

18

    

e.printStackTrace();
19

 

}

20

}

[ASM 类注入]()

ASM 类注入是指修改一个现有的 class 文件，在其中加入自己的代码。按通常思维，我们需要利用 ASM 提供的 reader 类去读取所要修改的类文件， 找到要修改的地方，如方法名或属性名，然后在该处插入自己的代码或修改现有的代码，这种思路将使问题复杂化。 ASM 为我们提供了访问类文件的 Visitor 模式，来遍历一个 class 文件，Visitor 是一个实现 ClassVisitor 接口的类。 要注入一个 class 文件，先要找到一个合适的 Visitor 做向导， ASM 提供了好几种 Visitor， 最常用的是 ClassWriter。同时我们需要提供一个适配器类 ClassAdapter， Visitor 会带着这个 Adapter 一起去遍历， 然后在遍历过程中回调 Adapter 提供的方法。我们在 Adapter 的这些方法中就可以实现我们的修改和定制逻辑。当然，如果你不想做任何修改，那 Visitor 遍历完后将得到一个和被遍历的 class 完全一样的拷贝。
[**清单 9. 类注入的完整过程**]()01

public
 

void
 

addAnnotationToExistingClass() {

02

 

FileInputStream is ;
03

 

ClassReader cr;          

04

 

try
 

{        
05

   

String classfile = 

"E:\\SayHelloImpl.class"
 

; 

// 待遍历的类

06

   

showClassSource(classfile) ;  

// 打印该类的 ASM source
07

           
 

08

   

is = 

new
 

FileInputStream(classfile);       
09

   

cr = 

new
 

ClassReader(is);

10

   

// 此处我们使用 ClassWriter 做 Visitor
11

   

ClassWriter cw = 

new
 

ClassWriter(ClassWriter.COMPUTE_MAXS);

12

// 给 Visitor 提供一个 Adapter
13

   

AddAnnotationAdapter adapter = 

new
 

AddAnnotationAdapter(cw);           

14

   

cr.accept(adapter, ClassReader.SKIP_DEBUG);            
15

   
 

16

   

// 遍历完后，生成的 class 保存在字节数组中
17

   

byte

[] b = cw.toByteArray();

18

           
 
19

   

try
 

{

20

        

// 将字节数组输出到文件中
21

        

// 为了不至于覆盖掉原有的 SayHelloImpl.class 文件，我们将结果输出到 _SayHelloImpl.class

22

     

FileOutputStream fout = 

new
 

FileOutputStream(

"E:\\_SayHelloImpl.class"

);
23

     

fout.write(b) ;

24

 

fout.flush();
25

     

fout.close() ;

26

   

} 

catch
 

(Exception e) {
27

     

e.printStackTrace() ;

28

   

}
29

           
 

30

   

// 验证 _SayHelloImpl.class 是否包含我们注入的代码
31

   

showClassSource(

"E:\\_SayHelloImpl.class"

) ;

32

} 

catch
 

(Exception e) {
33

     

e.printStackTrace();

34

} 
35

}

[web services 标记注入过程]()

我们已经知道了如何往 class 中注入自己的代码，那对于一个已有的 Java bean，怎么往里注入 @WebService 和 @WebMethod 呢？根据上面我们讲的转换关系，我们可以先写一个带 annotation 的 Java Source，编译得到 Java Class， 再得到其 ASM Code。下面是利用 ASM 给一个不带任何 Annotation 的 class 添加 @WebService 和 @WebMethod 的步骤。

1. 首先在 SayHelloImpl.java 中添加 @WebService 和 @WebMethod： 
[**清单 10. 带 web services 标注的 SayHelloImpl 类**]()1

@WebService

2

public
 

class
 

SayHelloImpl {
3

 

@WebMethod

4

 

public
 

String sayHello(String s) {
5

   

return
 

"Hello: "
 

+ s ;

6

 

}
7

}
1. 通过 ASMifierClassVisitor 工具获得 SayHelloImpl class 的 ASM Code，见清单 6。
1. 写一个 ClassAdapter 类， ClassAdapter 其实也是一个 Visitor。根据 ASM Code 的提示， 我们就可以知道如何利用 ASM API 去获得我们希望的 class 文件。下面是 Adapter 的示例代码。 
[**清单 11. 添加 Annotation 的 Adapter 类**]()01

public
 

class
 

AddAnnotationAdapter 

extends
 

ClassAdapter 

implements
 

Opcodes{

02

  

//private String annotationDesc;
03

  

private
 

boolean
 

isAnnotationPresent;

04
 
05

  

public
 

AddAnnotationAdapter(ClassVisitor cv) {

06

     

super

(cv);
07

  

}

08
 
09

  

@Override

10

  

public
 

void
 

visit(

int
 

version, 

int
 

access, String name, String signature,
11

    

String superName, String[] interfaces) {

12

        
 
13

    

cv.visit(V1_6, ACC_PUBLIC + ACC_SUPER, name, signature, superName,

14

    

interfaces);
15

    

AnnotationVisitor av0;

16

    

av0 = cv.visitAnnotation(

"Ljavax/jws/WebService;"

, 

true

);
17

    

av0.visitEnd();

18

  

}
19
 

20

  

@Override
21

  

public
 

AnnotationVisitor visitAnnotation(String desc, 

boolean
 

visible) {

22

    

return
 

cv.visitAnnotation(desc, visible);
23

  

}

24
 
25

  

@Override

26

  

public
 

void
 

visitInnerClass(String name, String outerName,
27

    

String innerName, 

int
 

access) {

28

    

cv.visitInnerClass(name, outerName, innerName, access);
29

  

}

30
 
31

  

@Override

32

  

public
 

FieldVisitor visitField(

int
 

access, String name, String desc,
33

    

String signature, Object value) {

34

    

return
 

cv.visitField(access, name, desc, signature, value);
35

  

}

36
 
37

  

@Override

38

  

public
 

MethodVisitor visitMethod(

int
 

access, String name, String desc,
39

    

String signature, String[] exceptions) {   

40

         

MethodVisitor mv  = cv.visitMethod(access, name, desc, signature,
41

          

exceptions);

42

         

if
 

(!name.equals(

"<init>"

)) {
43

          

AnnotationVisitor av0;

44

          

av0 = mv.visitAnnotation(

"Ljavax/jws/WebMethod;"

, 

true

);
45

          

av0.visitEnd();

46

         

}
47

         

return
 

mv;

48

     

}
49
 

50

  

@Override
51

  

public
 

void
 

visitEnd() {

52

    

cv.visitEnd();
53

  

}

54
 
55

 

}

最后，我们采用上面讲的类注入办法，即可得到一个带 @WebService 和 @WebMethod 标注的 class 文件。再通过 wsgen 工具即可成功创建 web services，该命令将会在 src 目录下生成 web services 描述文件：SayHelloImplService.wsdl，SayHelloImplService_schema1.xsd，同时生成了 2 个 JAX-WS 文件，分别为：SayHello.java，SayHelloResponse.java。

当然，在实际应用中，我们可能需要将一个 class 的某些特定方法发布为 web services，这就需要我们对 Adapter 做进一步的改造，在 visitMethod() 方法中根据方法名称等参数做进一步的处理。

[结束语]()

本文简要介绍了 ASM 字节码工具及其在 web services 开发中的应用。web services 的使用已经越来越广泛， 但在改造遗留系统过程中会遇到不少问题，本文针对其中一个主要问题给出了解决办法，该方法无须对现有系统做大量的改动， 即可将现存的 Java bean 转化成 web services 。本文对那些正考虑迁移到 JAX-WS 编程模型上的项目有一定的参考价值。在基于 JAX-WS 标准的 web services 的开发中，不少实际场景都是希望采用自底向上的开发方式， 即基于已有的 Java bean 来创建 web services 。WebSphere Application Server ( 以下简称 WAS ) 提供了命令行的工具 wsgen 和相对应的 Ant task 来支持这种开发过程，而且这两个工具比较适合大型项目的自动化构建。 这两个工具的使用前提是 Java bean 中事先添加有 web services 的 Annotation 标注，而在现有的业务系统中，class 文件一般是不带 Annotation 的，这就需要开发人员去修改现有的代码，以手工方式添加 Annotation，但这样带来的工作量太大且容易出错。本文介绍了一种解决途径，可以不用修改源代码，而是利用字节码工具 ASM 直接修改 class 文件， 在字节码文件中自动注入 Annotation ，然后再利用 wsgen 工具就可以很方便地生成 web services 应用。 本文同时也总结了使用 ASM 的一些实践。

本文首先介绍了和 ASM 使用相关的四种文件，以及它们之间的相互转化。然后结合 web services 开发实例，介绍了使用 wsgen 开发时遇到的实际问题，如何写一个 ASM 的适配器去修改现有 class，最终如何产生 web services 文件。

ASM 是一个多用途的 Java 字节码操控和分析框架。它能以二进制的形式直接修改现有的类或动态生成的类。 ASM 提供了常用的转换和分析算法，是一种允许用户方便的组装定制化的复杂转换和代码分析工具。 ASM 也提供和其它的字节码工具类似的功能，但 ASM 的重点是使用的简单和高性能。由于 ASM 在设计实现的时候尽可能 的小的内存占用和提供更高的性能；因此在动态系统中使用 ASM 是很有优势的。ASM 作为一种轻量级、高性能的 Java 字节码操控和分析框架，设计了一种更有效的方法、提供更好的性能和内存占用。今天， ASM 在许多领域都有应用，并已成为事实上的字节码处理框架标准。

[问题的引入]()

先写一个不带 web services 标注的 SayHelloImpl 类。
[**清单 1. 不带 web services 的 SayHelloImpl 类**]()1

public
 

class
 

SayHelloImpl {

2

 

public
 

String sayHello(String s) {
3

   

return
 

“Hello: ” + s ;

4

 

}
5

}

再用 wsgen 来创建 web services，wsgen 的使用如下：
[**清单 2. wsgen 实例**]()1

wsgen.exe – 

cp
 

bin – s output – d output – r output – wsdl ibm.was.asm.SayHelloImpl我们将会遇到如下错误： 
[**清单 3. 根据不带 web services 标注的类创建 web services 时遇到的错误信息**]() 
注释处理过程中遇到问题；

......
com.sun.tools.internal.ws.processor.modeler.ModelerException: [failed to localiz

e] A web service endpoint could not be found()
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.o

nError(WebServiceAP.java:215)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.b

uildModel(WebServiceAP.java:322)
        at com.sun.tools.internal.ws.processor.modeler.annotation.WebServiceAP.p

rocess(WebServiceAP.java:256)
        at com.sun.mirror.apt.AnnotationProcessors$CompositeAnnotationProcessor.

process(AnnotationProcessors.java:60)

注意：我们通常是在 class 前使用 @WebService， 在方法前使用 @WebMethod 来将一个类标注为 web services 的。WAS 6.1 的 web services 功能部件包和 WAS 7 都支持基于 JAX-WS 标准 的 web services 的开发， 在 WAS 安装目录的 bin 目录下提供了命令行工具 wsgen，wsgen 支持自底向上的开发方式。 下面是 wsgen 的用法：
[**清单 4. wsgen 命令格式**]() 
注 wsgen [options]  [SEI]

主要选项：

* -d 指定生成的 class 文件位置；
* -s 指定生成的 Java 源文件位置；
* -r 指定生成的 resources 文件位置，如 wsdl，xsd；
* -cp 指定服务实现类所在的位置；
* -wsdl，-servicename，-portname 三个参数指定生成的 wsdl 文件中的 service 和 port 的名称。

SEI(Service Endpoint Interface) 是一个 endpoint implementation class，不能是一个 interface。所以首先要开发一个 endpoint 的实现类，如本例中的 SayHelloImpl 。用 @WebService 声明 web services ，然后将它编译，才能提供给 wsgen 来创建 web services 。

[四种文件之间的转换]()

下面介绍本文要用到的四个概念（Java Source，Java Class，ASM Code，ASM Source）

* Java Source: 即我们通常编写的 Java 源文件；
* Java Class: Java 源文件编译后的字节码文件；
* ASM Source : 类似于对 class 反编译后的源文件，也就是 "textual byte code"，但比原始的 Java Source 可读性要差， 参见清单 5。
* ASM Code: 指读写 class 文件的 ASM 程序代码。需要用到 ASM 提供的 API， 参见清单 6。
[**清单 5. SayHelloImpl 类对应的 ASM Source**]()01

// class version 50.0 (50)

02

// access flags 33
03

public
 

class
 

com/ibm/was/asm/SayHelloImpl {

04
 
05

 

// access flags 1

06

 

public
  

<init>()V
07

   

ALOAD 

0

08

   

INVOKESPECIAL java/lang/Object. <init> ()V
09

   

RETURN

10

   

MAXSTACK = 

1
11

   

MAXLOCALS = 

1

12
 
13

 

// access flags 1

14

 

public
 

sayHello(Ljava/lang/String;)Ljava/lang/String;
15

   

NEW java/lang/StringBuilder

16

   

DUP
17

   

LDC 

"Hello:"

18

   

INVOKESPECIAL java/lang/StringBuilder. <init> (Ljava/lang/String;)V
19

   

ALOAD 

1

20

   

INVOKEVIRTUAL java/lang/StringBuilder.append (Ljava/lang/String;)
21

   

Ljava/lang/StringBuilder;

22

   

INVOKEVIRTUAL java/lang/StringBuilder.toString ()Ljava/lang/String;
23

   

ARETURN

24

   

MAXSTACK = 

3
25

   

MAXLOCALS = 

2

26

}[**清单 6. 生成 SayHelloImpl.class 的 ASM Code**]()01

package
 

asm.com.ibm.was.asm;

02

import
 

java.util.*;
03

import
 

org.objectweb.asm.*;

04

import
 

org.objectweb.asm.attrs.*;
05

public
 

class
 

SayHelloImplDump 

implements
 

Opcodes {

06

 

public
 

static
 

byte

[] dump () 

throws
 

Exception {
07

 

ClassWriter cw = 

new
 

ClassWriter(

0

);

08

 

FieldVisitor fv;
09

 

MethodVisitor mv;

10

 

AnnotationVisitor av0;
11
 

12

 

cw.visit(V1_6, ACC_PUBLIC + ACC_SUPER, 

"com/ibm/was/asm/SayHelloImpl"

,
13

 

null

, 

"java/lang/Object"

, 

null

);

14
 
15

 

{

16

 

mv = cw.visitMethod(ACC_PUBLIC, 

"<init>"

, 

"()V"

, 

null

, 

null

);
17

 

mv.visitCode();

18

 

mv.visitVarInsn(ALOAD, 

0

);
19

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/Object"

, 

"<init>"

, 

"()V"

);

20

 

mv.visitInsn(RETURN);
21

 

mv.visitMaxs(

1

, 

1

);

22

 

mv.visitEnd();
23

 

}

24

 

{
25

 

mv = cw.visitMethod(ACC_PUBLIC, 

"sayHello"

,

26

  

"(Ljava/lang/String;)Ljava/lang/String;"

, 

null

, 

null

);
27

 

mv.visitCode();

28

 

mv.visitTypeInsn(NEW, 

"java/lang/StringBuilder"

);
29

 

mv.visitInsn(DUP);

30

 

mv.visitLdcInsn(

"Hello:"

);
31

 

mv.visitMethodInsn(INVOKESPECIAL, 

"java/lang/StringBuilder"

,

32

 

"<init>"

, 

"(Ljava/lang/String;)V"

);
33

 

mv.visitVarInsn(ALOAD, 

1

);

34

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
35

 

"append"

, 

"(Ljava/lang/String;)Ljava/lang/StringBuilder;"

);

36

 

mv.visitMethodInsn(INVOKEVIRTUAL, 

"java/lang/StringBuilder"

,
37

 

"toString"

, 

"()Ljava/lang/String;"

);

38

 

mv.visitInsn(ARETURN);
39

 

mv.visitMaxs(

3

, 

2

);

40

 

mv.visitEnd();
41

 

}

42

 

cw.visitEnd();
43

 

return
 

cw.toByteArray();

44

 

}
45

}

图 1 描述了 Java Source 文件、Java Class 文件、ASM Code 文件和 ASM Source 文件之间的转换关系。
[**图 1. 四种文件的转换图**]() 
[![图 1. 四种文件的转换图]()]() 

如图 -1 所示，Java Source 文件通过 Javac 可以转换为 Java Class 文件。而相应的 Java Class 文件通过 Java 反编译器工具可转换为 Java Source 文件；

通 过 ASM Code 去创建和修改 Java Class 需要对 ASM API 比较熟悉才行，一个常见的问题时，怎么用 ASM Code 生成一个我们希望的 class 文件， 也就是说， 给定了 Java Class， 怎么得到其对应的 ASM Code 呢？ 所幸的是， ASM 框架为我们提供了 ASMifierClassVisitor 工具来产生 Java Class 对应的 ASM Code。代码如下：
[**清单 7. 给定 class 文件，通过 ASMifierClassVisitor 获得其对应的 ASM Code**]()01

public
 

void
 

showClassASMProcess() {

02

  

ClassReader cr;
03

  

final
 

String n = SayHelloImpl.

class

.getName();

04

       
 
05

  

try

{

06

    

cr = 

new
 

ClassReader(n);                  
07

    

cr.accept(

new
 

ASMifierClassVisitor(

new

PrintWriter(System.out)),

08

    

new
 

Attribute[

0

],
09

    

ClassReader.SKIP_DEBUG);

10

               
 
11

  

} 

catch
 

(Exception e) {

12

    

e.printStackTrace();
13

 

}

14

}

另外一个重要的转换就是， 给定了 Java Class 文件， 如何查看它的 ASM Source ？ 这点对于我们验证 ASM Code 生成的 class 是否正确很有用。ASM 提供了另外一个工具 TraceClassVisitor， 来获得一个 Java Class 对应的 ASM Source。代码如下：
[**清单 8. 利用 TraceClassVisitor 查看 class 文件的 ASM Source**]()01

//file 是 class 文件的全路径名

02

public
 

void
 

showClassSource(String file) {
03

  

FileInputStream is ;

04

  

ClassReader cr; 
05

       
 

06

  

try
 

{
07

      

is = 

new
 

FileInputStream(file);     

08

      

cr = 

new
 

ClassReader(is);
09

      

TraceClassVisitor trace = 

new
 

TraceClassVisitor(

new

PrintWriter(System.out));

10

      

cr.accept(trace, ClassReader.SKIP_DEBUG);
11

           
 

12

      

is.close() ;
13

  

} 

catch
 

(Exception e) {

14

      

e.printStackTrace() ;
15

 

}

16

}
17

{

18

    

e.printStackTrace();
19

 

}

20

}

[ASM 类注入]()

ASM 类注入是指修改一个现有的 class 文件，在其中加入自己的代码。按通常思维，我们需要利用 ASM 提供的 reader 类去读取所要修改的类文件， 找到要修改的地方，如方法名或属性名，然后在该处插入自己的代码或修改现有的代码，这种思路将使问题复杂化。 ASM 为我们提供了访问类文件的 Visitor 模式，来遍历一个 class 文件，Visitor 是一个实现 ClassVisitor 接口的类。 要注入一个 class 文件，先要找到一个合适的 Visitor 做向导， ASM 提供了好几种 Visitor， 最常用的是 ClassWriter。同时我们需要提供一个适配器类 ClassAdapter， Visitor 会带着这个 Adapter 一起去遍历， 然后在遍历过程中回调 Adapter 提供的方法。我们在 Adapter 的这些方法中就可以实现我们的修改和定制逻辑。当然，如果你不想做任何修改，那 Visitor 遍历完后将得到一个和被遍历的 class 完全一样的拷贝。
[**清单 9. 类注入的完整过程**]()01

public
 

void
 

addAnnotationToExistingClass() {

02

 

FileInputStream is ;
03

 

ClassReader cr;          

04

 

try
 

{        
05

   

String classfile = 

"E:\\SayHelloImpl.class"
 

; 

// 待遍历的类

06

   

showClassSource(classfile) ;  

// 打印该类的 ASM source
07

           
 

08

   

is = 

new
 

FileInputStream(classfile);       
09

   

cr = 

new
 

ClassReader(is);

10

   

// 此处我们使用 ClassWriter 做 Visitor
11

   

ClassWriter cw = 

new
 

ClassWriter(ClassWriter.COMPUTE_MAXS);

12

// 给 Visitor 提供一个 Adapter
13

   

AddAnnotationAdapter adapter = 

new
 

AddAnnotationAdapter(cw);           

14

   

cr.accept(adapter, ClassReader.SKIP_DEBUG);            
15

   
 

16

   

// 遍历完后，生成的 class 保存在字节数组中
17

   

byte

[] b = cw.toByteArray();

18

           
 
19

   

try
 

{

20

        

// 将字节数组输出到文件中
21

        

// 为了不至于覆盖掉原有的 SayHelloImpl.class 文件，我们将结果输出到 _SayHelloImpl.class

22

     

FileOutputStream fout = 

new
 

FileOutputStream(

"E:\\_SayHelloImpl.class"

);
23

     

fout.write(b) ;

24

 

fout.flush();
25

     

fout.close() ;

26

   

} 

catch
 

(Exception e) {
27

     

e.printStackTrace() ;

28

   

}
29

           
 

30

   

// 验证 _SayHelloImpl.class 是否包含我们注入的代码
31

   

showClassSource(

"E:\\_SayHelloImpl.class"

) ;

32

} 

catch
 

(Exception e) {
33

     

e.printStackTrace();

34

} 
35

}

[web services 标记注入过程]()

我们已经知道了如何往 class 中注入自己的代码，那对于一个已有的 Java bean，怎么往里注入 @WebService 和 @WebMethod 呢？根据上面我们讲的转换关系，我们可以先写一个带 annotation 的 Java Source，编译得到 Java Class， 再得到其 ASM Code。下面是利用 ASM 给一个不带任何 Annotation 的 class 添加 @WebService 和 @WebMethod 的步骤。

1. 首先在 SayHelloImpl.java 中添加 @WebService 和 @WebMethod： 
[**清单 10. 带 web services 标注的 SayHelloImpl 类**]()1

@WebService

2

public
 

class
 

SayHelloImpl {
3

 

@WebMethod

4

 

public
 

String sayHello(String s) {
5

   

return
 

"Hello: "
 

+ s ;

6

 

}
7

}
1. 通过 ASMifierClassVisitor 工具获得 SayHelloImpl class 的 ASM Code，见清单 6。
1. 写一个 ClassAdapter 类， ClassAdapter 其实也是一个 Visitor。根据 ASM Code 的提示， 我们就可以知道如何利用 ASM API 去获得我们希望的 class 文件。下面是 Adapter 的示例代码。 
[**清单 11. 添加 Annotation 的 Adapter 类**]()01

public
 

class
 

AddAnnotationAdapter 

extends
 

ClassAdapter 

implements
 

Opcodes{

02

  

//private String annotationDesc;
03

  

private
 

boolean
 

isAnnotationPresent;

04
 
05

  

public
 

AddAnnotationAdapter(ClassVisitor cv) {

06

     

super

(cv);
07

  

}

08
 
09

  

@Override

10

  

public
 

void
 

visit(

int
 

version, 

int
 

access, String name, String signature,
11

    

String superName, String[] interfaces) {

12

        
 
13

    

cv.visit(V1_6, ACC_PUBLIC + ACC_SUPER, name, signature, superName,

14

    

interfaces);
15

    

AnnotationVisitor av0;

16

    

av0 = cv.visitAnnotation(

"Ljavax/jws/WebService;"

, 

true

);
17

    

av0.visitEnd();

18

  

}
19
 

20

  

@Override
21

  

public
 

AnnotationVisitor visitAnnotation(String desc, 

boolean
 

visible) {

22

    

return
 

cv.visitAnnotation(desc, visible);
23

  

}

24
 
25

  

@Override

26

  

public
 

void
 

visitInnerClass(String name, String outerName,
27

    

String innerName, 

int
 

access) {

28

    

cv.visitInnerClass(name, outerName, innerName, access);
29

  

}

30
 
31

  

@Override

32

  

public
 

FieldVisitor visitField(

int
 

access, String name, String desc,
33

    

String signature, Object value) {

34

    

return
 

cv.visitField(access, name, desc, signature, value);
35

  

}

36
 
37

  

@Override

38

  

public
 

MethodVisitor visitMethod(

int
 

access, String name, String desc,
39

    

String signature, String[] exceptions) {   

40

         

MethodVisitor mv  = cv.visitMethod(access, name, desc, signature,
41

          

exceptions);

42

         

if
 

(!name.equals(

"<init>"

)) {
43

          

AnnotationVisitor av0;

44

          

av0 = mv.visitAnnotation(

"Ljavax/jws/WebMethod;"

, 

true

);
45

          

av0.visitEnd();

46

         

}
47

         

return
 

mv;

48

     

}
49
 

50

  

@Override
51

  

public
 

void
 

visitEnd() {

52

    

cv.visitEnd();
53

  

}

54
 
55

 

}

最后，我们采用上面讲的类注入办法，即可得到一个带 @WebService 和 @WebMethod 标注的 class 文件。再通过 wsgen 工具即可成功创建 web services，该命令将会在 src 目录下生成 web services 描述文件：SayHelloImplService.wsdl，SayHelloImplService_schema1.xsd，同时生成了 2 个 JAX-WS 文件，分别为：SayHello.java，SayHelloResponse.java。

当然，在实际应用中，我们可能需要将一个 class 的某些特定方法发布为 web services，这就需要我们对 Adapter 做进一步的改造，在 visitMethod() 方法中根据方法名称等参数做进一步的处理。

[结束语]()

本文简要介绍了 ASM 字节码工具及其在 web services 开发中的应用。web services 的使用已经越来越广泛， 但在改造遗留系统过程中会遇到不少问题，本文针对其中一个主要问题给出了解决办法，该方法无须对现有系统做大量的改动， 即可将现存的 Java bean 转化成 web services 。本文对那些正考虑迁移到 JAX-WS 编程模型上的项目有一定的参考价值。
