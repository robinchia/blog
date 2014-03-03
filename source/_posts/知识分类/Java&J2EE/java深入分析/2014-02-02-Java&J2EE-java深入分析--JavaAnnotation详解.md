---
layout: post
title: "Java Annotation详解"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - java深入分析
--- 

# Java Annotation详解

**元数据的作用**

如果要对于元数据的作用进行分类，目前还没有明确的定义，不过我们可以根据它所起的作用，大致可分为三类：

l          编写文档：通过代码里标识的元数据生成文档。

l          代码分析：通过代码里标识的元数据对代码进行分析。

l          编译检查：通过代码里标识的元数据让编译器能实现基本的编译检查。

 

**基本内置注释**

    **@Override** 注释能实现编译时检查，你可以为你的方法添加该注释，以声明该方法是用于覆盖父类中的方法。如果该方法不是覆盖父类的方法，将会在编译时报错。例如我们为某类重写toString() 方法却写成了tostring() ，并且我们为该方法添加了@Override 注释；

     **@Deprecated** 的作用是对不应该在使用的方法添加注释，当编程人员使用这些方法时，将会在编译时显示提示信息，它与javadoc 里的 @deprecated 标记有相同的功能，准确的说，它还不如javadoc @deprecated ，因为它不支持参数，

注意：要了解详细信息，请使用 -Xlint:deprecation 重新编译。

    **@SuppressWarnings** 与前两个注释有所不同，你需要添加一个参数才能正确使用，这些参数值都是已经定义好了的，我们选择性的使用就好了，参数如下：

 

deprecation   使用了过时的类或方法时的警告

unchecked  执行了未检查的转换时的警告，例如当使用集合时没有用泛型 (Generics) 来指定集合保存的类型

fallthrough   当 Switch 程序块直接通往下一种情况而没有 Break 时的警告

path   在类路径、源文件路径等中有不存在的路径时的警告

serial 当在可序列化的类上缺少 serialVersionUID 定义时的警告

finally    任何 finally 子句不能正常完成时的警告

all 关于以上所有情况的警告

 

注意：要了解详细信息，请使用 -Xlint:unchecked 重新编译。

**** 

**定制注释类型**

    好的，让我们创建一个自己的注释类型（annotation type ）吧。它类似于新创建一个接口类文件，但为了区分，我们需要将它声明为@interface, 如下例：

public @interface NewAnnotation {

 

}

 

**使用定制的注释类型**

    我们已经成功地创建好一个注释类型NewAnnotation ，现在让我们来尝试使用它吧，如果你还记得本文的第一部分，那你应该知道他是一个标记注释，使用也很容易，如下例：

public class AnnotationTest {

 

    @NewAnnotation

    public static void main(String[] args) {

   

    }

}

 

**添加变量**

    J2SE 5.0 里，我们了解到内置注释@SuppressWarnings() 是可以使用参数的，那么自定义注释能不能定义参数个数和类型呢？答案是当然可以，但参数类型只允许为基本类型、String 、Class 、枚举类型等，并且参数不能为空。我们来扩展NewAnnotation ，为之添加一个String 类型的参数，示例代码如下：

public @interface NewAnnotation {

 

    String value();

}

    使用该注释的代码如下：正如你所看到的，该注释的使用有两种写法，这也是在之前的文章里所提到过的。如果你忘了这是怎么回事，那就再去翻翻吧。

public class AnnotationTest {

 

    @NewAnnotation("Just a Test.")

    public static void main(String[] args) {

        sayHello();

    }

   

    @NewAnnotation(value="Hello NUMEN.")

    public static void sayHello() {

        // do something

    }

}

 

**为变量赋默认值**

    我们对Java 自定义注释的了解正在不断的增多，不过我们还需要更过，在该条目里我们将了解到如何为变量设置默认值，我们再对NewAnnotaion 进行修改，看看它会变成什么样子，不仅参数多了几个，连类名也变了。但还是很容易理解的，我们先定义一个枚举类型，然后将参数设置为该枚举类型，并赋予默认值。

public @interface Greeting {

 

    public enum FontColor {RED, GREEN, BLUE};

 

    String name();

 

    String content();

   

    FontColor fontColor() default FontColor.BLUE;

}

 

**限定注释使用范围**

    当我们的自定义注释不断的增多也比较复杂时，就会导致有些开发人员使用错误，主要表现在不该使用该注释的地方使用。为此，Java 提供了一个ElementType 枚举类型来控制每个注释的使用范围，比如说某些注释只能用于普通方法，而不能用于构造函数等。下面是Java 定义的ElementType 枚举：

package java.lang.annotation;

 

public enum ElementType {

  TYPE,         // Class, interface, or enum (but not annotation)

  FIELD,        // Field (including enumerated values)

  METHOD,       // Method (does not include constructors)

  PARAMETER,        // Method parameter

  CONSTRUCTOR,      // Constructor

  LOCAL_VARIABLE,   // Local variable or catch clause

  ANNOTATION_TYPE,  // Annotation Types (meta-annotations)

  PACKAGE       // Java package

}

    下面我们来修改Greeting 注释，为之添加限定范围的语句，这里我们称它为目标（Target ）使用方法也很简单，如下：

 

@Target( { ElementType.METHOD, ElementType.CONSTRUCTOR })

public @interface Greeting {

}

正如上面代码所展示的，我们只允许Greeting 注释标注在普通方法和构造函数上，使用在包申明、类名等时，会提示错误信息。

 

**注释保持性策略**

public enum RetentionPolicy {

  SOURCE,// Annotation is discarded by the compiler

  CLASS,// Annotation is stored in the class file, but ignored by the VM

  RUNTIME// Annotation is stored in the class file and read by the VM

}

    RetentionPolicy 的使用方法与ElementType 类似，简单代码示例如下：

@Retention(RetentionPolicy.RUNTIME)

@Target( { ElementType.METHOD, ElementType.CONSTRUCTOR })

 

**文档化功能**

    Java 提供的Documented 元注释跟Javadoc 的作用是差不多的，其实它存在的好处是开发人员可以定制Javadoc 不支持的文档属性，并在开发中应用。它的使用跟前两个也是一样的，简单代码示例如下：

@Documented

@Retention(RetentionPolicy.RUNTIME)

@Target( { ElementType.METHOD, ElementType.CONSTRUCTOR })

public @interface Greeting {

}

 

值得大家注意的是，如果你要使用@Documented 元注释，你就得为该注释设置RetentionPolicy.RUNTIME 保持性策略。为什么这样做，应该比较容易理解，这里就不提了。

[****]() 

[**标注继承**]()

[继承应该是Java 提供的最复杂的一个元注释了，它的作用是控制注释是否会影响到子类，简单代码示例如下：]()

[@Inherited]()

[@Documented]()

[@Retention(RetentionPolicy.RUNTIME)]()

[@Target( { ElementType.METHOD, ElementType.CONSTRUCTOR })]()

[public @interface Greeting {]()

[}]()

[]() 

[**读取注释信息**]()

[    ]()[当我们想读取某个注释信息时，我们是在运行时通过反射来实现的，如果你对元注释还有点印象，那你应该记得我们需要将保持性策略设置为RUNTIME ，也就是说只有注释标记了@Retention(RetentionPolicy.RUNTIME)]()[的，我们才能通过反射来获得相关信息，下面的例子我们将沿用前面几篇文章中出现的代码，并实现读取AnnotationTest 类所有方法标记的注释并打印到控制台。好了，我们来看看是如何实现的吧：]()

[public class AnnotationIntro {]()

[]() 

[    public static void main(String[] args) throws Exception {]()

[]() 

[        Method[] methods = Class.forName(]()

[                "com.gelc.annotation.demo.customize.AnnotationTest")]()

[                .getDeclaredMethods();]()

[        Annotation[] annotations;]()

[]() 

[        for (Method method : methods) {]()

[            annotations = method.getAnnotations();]()

[            for (Annotation annotation : annotations) {]()

[                System.out.println(method.getName() + " : "]()

[                        + annotation.annotationType().getName());]()

[            }]()

 

********************************************************************************

 

Annotation(注解)

Annotation对于程序运行没有影响,它的目的在于对编译器或分析工具说明程序的某些信息,您可以
在包,类,方法,域成员等加上Annotation.每一个Annotation对应于一个实际的Annotation类型.

1  限定Override父类方法@Override
  java.lang.Override是J2SE5.0中标准的Annotation类型之一,它对编译器说明某个方法必须
  是重写父类中的方法.编译器得知这项信息后,在编译程序时如果发现被@Override标示的方法
  并非重写父类中的方法,就会报告错误.
  例,如果在定义新类时想要重写Object类的toString()方法,可能会写成这样:
  public class CustomClass{
    public String ToString(){
      return "customObject";
    }
  }
  在编写toString()方法时,因为输入错误或其他的疏忽,将之写成ToString()了,编译这个类时
  并不会出现任何的错误,编译器不会知道您是想重写toString()方法,只会以为是定义了一个新
  的ToString()方法.
  可以使用java.lang.Override这个Annotation类型,在方法上加一个@Override的Annotation
  这可以告诉编译器现在定义的这个方法,必须是重写父类中的同包方法.
  public class CustomClass{
    @Override
    public String toString(){
      return "coustomObject";
    }
  }
  java.lang.Override是一个Marker Annotation,简单地说就是用于标示的Annotation，Annotation
  名称本身表示了要给工具程序的信息。
  
  Annotation类型与Annotation实际上是有区分的，Annotation是Annotation类型的实例，例如
  @Override是个Annotation，它是java.lang.Override类型的一个实例，一个文件中可以有很多
  个@Override，但它们都是属于java.lang.Override类型。
  
2  标示方法为Deprecated @Deprecated
  java.lang.Deprecated也是J2SE5.0中标准的Annotation类型之一。它对编译器说明某个方法已经不
  建议使用。如果有开发人员试图使用或重写被@Deprecated标示的方法，编译器必须提出警告信息。
  例:
  public class Something{
    @Deprecated
    public Something getSomething(){
      return new Something();
    }
  }
  如果有人试图在继承这个类后重写getSomething()方法，或是在程序中调用getSomething()方法，
  则编译时会有警告出现。
  java.lang.Deprecated也是一个Marker Annotation简单地说就是用于标示。
  
3  抑制编译器警告 @SuppressWarnings
  java.lang.SuppressWarnings也是J2SE5.0中标准的Annotation类型之一，它对编译器说明某个方法
  中若有警告信息，则加以抑制，不用在编译完成后出现警告。
  例:
  public class SomeClass2{
    @SuppressWarnings(value={"unchecked"});
    public void doSomething(){
      Map map = new HashMap();
      map.put("some","thing");
    }
  }
  这样，编译器将忽略unchecked的警告，您也可以指定忽略多个警告:
  @SuppressWarnings(value={"unchecked","deprecation"});
  @SuppressWarnings是所谓的Single-Value Annotation，因为这样的Annotation只有一个成员，称为
  value成员，可在使用Annotation时作额外的信息指定。
  
  
  
自定义Annotation类型
  可以自定义Annotation类型，并使用这些自定义的Annotation类型在程序代码中使用Annotation，这些
  Annotation将提供信息给程序代码分析工具。
  首先来看看如何定义Marker Annotation，也就是Annotation名称本身即提供信息。对于程序分析工具来
  说，主要是检查是否有Marker Annotation的出现,并做出对应的动作。要定义一个Annotation所需的动作
  ,就类似于定义一个接口，只不过使用的是@interface。
  例:
  public @interface Debug{}
  
  由于是一个Marker Annotation，所以没有任何成员在Annotation定义中。编译完成后，就可以在程序代码
  中使用这个Annotation。
  public class SomeObject{
    @Debug
    public void doSomething(){
     ......
    }
  }
  稍后可以看到如何在Java程序中取得Annotation信息(因为要使用Java程序取得信息，所以还要设置
  meta-annotation,稍后会谈到）
  
  接着来看看如何定义一个Single-Value Annotation，它只有一个Value成员。
  例:
  public @interface UnitTest{
    String value();
  }
  实际上定义了value()方法，编译器在编译时会自动产生一个value的域成员,接着在使用UnitTest 
  Annotation时要指定值。如：
  public class MathTool{
   @UnitTest("GCD")
   public static int gcdOf(int num1,int num2){
     ...............
   }
  }
  @UnitTest("GCD")实际上是@UnitTest(value="GCD")的简便写法,value也可以是数组值。如：
  public @interface FunctionTest{
    String[] value();
  }
  在使用时，可以写成@FunctionTest({"method1","method2"})这样的简便形式。或是
  @FunctionTest(value={"method1","method2"})这样的详细形式.
  
  也可以对value成员设置默认值,使用default关键词即可。
  例:
  public @interface UnitTest2{
    String value() default "noMethod";
  }
  这样如果使用@UnitTest2时没有指定value值，则value默认就是noMethod.
  
  
  也可以为Annotation定义额外的成员，以提供额外的信息给分析工具，如：
  public @interface Process{
    public enum Current{NONE,REQUIRE,ANALYSIS,DESIGN,SYSTEM};
    Current current() default Current.NONE;
    String tester();
    boolean ok();
  }
  运用:
  public class Application{
    @process(
      current = Process.Current.ANALYSIS,
      tester = "Justin Lin",
      ok = true
    )
    public void doSomething(){
      ...........
    }
  }
  当使用@interface自行定义Annotation类型时，实际上是自动继承了
  java.lang.annotation接口，并由编译器自动完成其他产生的细节，并且在定义Annotation类型时,
  不能继承其他的Annotation类型或接口.
  定义Annotation类型时也可以使用包机制来管理类。由于范例所设置的包都是onlyfun.caterpillar,
  所以可以直接使用Annotation类型名称而不指定包名，但如果是在别的包下使用这些自定义的Annotation
  ，记得使用import告诉编译器类型的包们置。
  如:
  import onlyfun.caterpillar.Debug;
  public class Test{
    @Debug
    public void doTest(){
      ......
    }
  }
  或是使用完整的Annotation名称.如:
  public class Test{
    @onlyfun.caterpillar.Debug
    public void doTest(){
      ......
    }
  }
  
  
  
meta-annotation
  所谓neta-annotation就是Annotation类型的数据，也就是Annotation类型的Annotation。在定义
  Annotation类型时，为Annotation类型加上Annotation并不奇怪，这可以为处理Annotation类型
  的分析工具提供更多的信息。
  
1  告知编译器如何处理annotation @Retention
  java.lang.annotation.Retention类型可以在您定义Annotation类型时，指示编译器该如何对待自定
  义的Annotation类型，编译器默认会将Annotation信息留在.class文件中，但不被虚拟机读取，而仅用
  于编译器或工具程序运行时提供信息。
  
  在使用Retention类型时，需要提供java.lang.annotation.RetentionPolicy的枚举类型。
  RetentionPolicy的定义如下所示:
  package java.lang.annotation;
  public enum RetentionPolicy{
    SOURCE,//编译器处理完Annotation信息后就没有事了
    CLASS,//编译器将Annotation存储于class文件中，默认
    RUNTIME //编译器将Annotation存储于class文件中，可由VM读入

  }
  
  RetentionPolicy为SOURCE的例子是@SuppressWarnings,这个信息的作用仅在编译时期告知
  编译器来抑制警告，所以不必将这个信息存储在.class文件中。
  
  RetentionPolicy为RUNTIME的时机，可以像是您使用Java设计一个程序代码分析工具，您必须让VM能读出
  Annotation信息，以便在分析程序时使用，搭配反射机制，就可以达到这个目的。\
  
  J2SE6.0的java.lang.reflect.AnnotatedElement接口中定义有4个方法:
  public Annotation getAnnotation(Class annotationType)
  public Annotation[] getAnnotations();
  public Annotation[] getDeclaredAnnotations()
  public boolean isAnnotationPresent(Class annotationType);
  
  Class,Constructor,field,Method,Package等类，都实现了AnnotatedElement接口，所以可以从这些
  类的实例上，分别取得标示于其上的Annotation与相关信息。由于在执行时读取Annotation信息，所以定
  义Annotation时必须设置RetentionPolicy为RUNTIME,也就是可以在VM中读取Annotation信息。
  例:
  package onlyfun.caterpillar;
  import java.lang.annotation.Retention;
  import java.lang.annotation.RetentionPllicy;
  
  @Retention(RetentionPolicy.RUNTIME)
  public @interface SomeAnnotation{
    String value();
    String name();
  }
  由于RetentionPolicy为RUNTIME，编译器在处理SomeAnnotation时，会将Annotation及给定的相关信息
  编译至.class文件中，并设置为VM可以读出Annotation信息。接下来：
  package onlyfun.caterpillar;
  public class SomeClass3{
    @SomeAnotation{
      value="annotation value1",
      name="annotation name1"
    }
    public void doSomething(){
      ......
    }
  }
  
  
  现在假设要设计一个源代码分析工具来分析所设计的类，一些分析时所需的信息已经使用Annotation标示于类
  中了，可以在执行时读取这些Annotation的相关信息。例:
  
  package onlyfun.caterpillar;
  
  import java.lang.annotation.Annotation;
  import java.lang.reflect.Method;
  
  public class AnalysisApp{
    public static void main(String [] args) throws NoSuchMethodException{
      Class<SomeClass3> c = SomeClass3.class;
      //因为SomeAnnotation标示于doSomething()方法上
      //所以要取得doSomething()方法的Method实例
      Method method = c.getMethod("doSomething");
      //如果SomeAnnotation存在
      if(method.isAnnotationPresent(SomeAnnotation.class){
        System.out.println("找到@SomeAnnotation");
        //取得SomeAnnotation
        SomeAnnotation annotation = method.getAnnotation(SomeAnnotation.class);
        //取得vlaue成员值
        System.out.println(annotation.value);
        //取得name成员值
        System.out.println(annotation.name());
      }else{
        System.out.println("找不到@SomeAnnotation");
      }
      //取得doSomething()方法上所有的Annotation
      Annotation[] annotations = method.getAnnotations();
      //显示Annotation名称
      for(Annotation annotation : annotations){
        System.out.println("Annotation名称:"+annotation.annotationType().getName());
      }
    }
  }
  若Annotation标示于方法上，就要取得方法的Method代表实例，同样的，如果Annotation标示于类或包上,
  就要分别取得类的Class代表的实例或是包的Package代表的实例。之后可以使用实例上的getAnnotation()
  等相关方法，以测试是否可取得Annotation或进行其他操作。
  
2  限定annotation使用对象 @Target
  在定义Annotation类型时，使用java.lang.annotation.Target可以定义其适用的时机，在定义时要指定
  java.lang.annotation.ElementType的枚举值之一。
  public enum elementType{
    TYPE,//适用class,interface,enum
    FIELD,//适用于field
    METHOD,//适用于method
    PARAMETER,//适用method上之parameter
    CONSTRUCTOR,//适用constructor
    LOCAL_VARIABLE,//适用于区域变量
    ANNOTATION_TYPE,//适用于annotation类型
    PACKAGE,//适用于package
  }
  
  举例，假设定义Annotation类型时，要限定它只能适用于构造函数与方法成员，则:
  package onlyfun.caterpillar;
  
  import java.lang.annotation.Target;
  import java.lang.annotation.ElementType;
  
  @Target({ElementType.CONSTRUCTOR,ElementType.METHOD})
  public @interface MethodAnnotation{}
  
  将MethodAnnotation标示于方法之上，如：
  
  public class SomeoneClass{
    @onlyfun.caterpillar.MethodAnnotation
    public void doSomething(){
      ......
    }
  }
  
3  要求为API文件的一部分 @Documented
 
  在制作Java Doc文件时，并不会默认将Annotation的数据加入到文件中.Annnotation用于标示程序代码以便
  分析工具使用相关信息，有时Annotation包括了重要的信息，您也许会想要在用户制作Java Doc文件的同时,
  也一并将Annotation的信息加入到API文件中。所以在定义Annotation类型时，可以使用
  java.lang.annotation.Documented.例:
  
  package onlyfun.caterpillar;
  
  import java.lang.annotation.Documented;
  import java.lang.annotation.Retention;
  import java.lang.annotation.RetentionPolicy;
  
  @Documented
  @Retention(RetentionPolicy.RUNTIME)
  public @interface TwoAnnotation{}
  
  使用java.lang.annotation.Documented为定义的Annotation类型加上Annotation时，必须同时使用Retention
  来指定编译器将信息加入.class文件，并可以由VM读取，也就是要设置RetentionPolicy为RUNTIME。接着可以使
  用这个Annotation，并产生Java Doc文件,这样可以看到文件中包括了@TwoAnnotation的信息.
  
4  子类是否可以继承父类的annotation @Inherited
  在定义Annotation类型并使用于程序代码上后，默认父类中的Annotation并不会被继承到子类中。可以在定义
  Annotation类型时加上java.lang.annotation.Inherited类型的Annotation，这让您定义的Annotation类型在
  被继承后仍可以保留至子类中。
  例:
  package onlyfun.caterpillar;
  
  import java.lang.annotation.Retention;
  import java.lang.annotation.RetentionPolicy;
  import java.lang.annotation.Inherited;
  
  @Retention(RetentionPolicy.RUNTIME)
  @Inherited
  public @interface ThreeAnnotation{
    String value();
    String name();
  }
  可以在下面的程序中使用@ThreeAnnotation:
  public class SomeoneClass{
    @onlyfun.caterpillar.ThreeAnnotation(
      value = "unit",
      name = "debug1"
    )
    public void doSomething(){
      .....
    }
  }
  如果有一个类继承了SomeoneClass类，则@ThreeAnnotation也会被继承下来。
来源： <[http://djjchobits.iteye.com/blog/569000](http://djjchobits.iteye.com/blog/569000)> 
