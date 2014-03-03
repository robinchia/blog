---
layout: post
title: "探究 Singleton 设计模式"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# 探究 Singleton 设计模式

**探究 Singleton 设计模式（构建分布式应用程序）**

来源：[www.microsoft.com](http://www.microsoft.com)
来源：[UML软件工程组织](http://uml.org.cn/sjms/200506211.htm)

**摘要：**

讨论 Singleton 设计模式（指示如何以及何时创建对象的创造性模式）及其在 Microsoft .NET 框架中的有效使用。

**内容**

[简介](http://sheneyan.com/tech/article/patterns/singleton.html#i1#i1)

[Singleton 模式](http://sheneyan.com/tech/article/patterns/singleton.html#i2#i2)

[结论](http://sheneyan.com/tech/article/patterns/singleton.html#i3#i3)

**简介**

在开发软件应用程序过程中，随着应用程序的开发，会出现重复性的模式。 随着整个软件系统的开发，很多相同的模式会逐渐显现出来。

这种重复性模式概念在其他应用中是非常明显的。 汽车制造就是一种此类应用。 很多不同的汽车型号使用相同的子构件，包括大多数基本部件（例如，灯泡和紧固零件）以及较大的构件（例如，底盘和发动机）。

在住宅建筑中，重复性模式概念适用于螺丝和螺钉以及整体总体建筑物配电系统。 无论组建的小组是为了开发新的汽车设计还是新的建筑物设计，它其通常不必没有考虑到以前已解决的问题。 如果设计和建筑住宅的小组必须重新构思和设计房子的每一个组成部分，则整个过程所花的时间比现在要长得多。 门高或灯开关功能等许多设计决策（例如，门高或灯开关功能）很容易理解。 房为满足给房子不同部分提供洗手功能的要求，房屋设计师不必重新设计和重新建造不同类型的输供水和蓄水设施：，以便达到为房子不同部分提供洗手功能的要求： 标准水槽以及标准的热水和冷水输入接头和排水输出接头是很容易理解非常常见的房屋建筑构件。 可以将重复性模式概念反复应用于我们周围的几乎每样东西上，包括软件。

汽车和住宅建筑示例有助于在软件设计和构造中体现某些一般性的抽象概念。 易于理解且明确定义的通用功能部件的概念是设计模式的源动力，它也是其他两篇设计模式文章探究工厂设计模式和探究观察者设计模式的重点。 这些模式几乎涵盖了面向对象的软件设计的各个方面，包括对象创建、对象交互和对象生存期。 在本文中，我们将讨论 Singleton 模式，它包含在创造性模式系列中。

创造性模式指示如何以及何时创建对象。 很多实例需要只能通过创造性方法解决的特殊行为，而不是在创建实例后强制实施所需的行为。 此类行为要求最好的例子之一包含在 Singleton 模式中。 Singleton 模式在 Design Patterns: Elements of Reusable Software 这一经典参考书目中有正式的定义，该书的作者包括 Erich Gamma、Richard Helm、Ralph Johnson 和 John Vlissides（也称为四人组或 GoF）。 在 Design Patterns 中，此模式是最简单也是使用最广泛的模式之一。 但是，正如我们将会看到的一样，在实现此模式时可能会出现一些问题。 本文试图通过 Singleton 模式的多个早期实现来从头开始分析 Singleton 模式，以及如何在 Microsoft_ .NET 应用程序开发中发挥其最佳用途。

**Singleton****模式**

按照 Design Patterns 中的定义，Singleton 模式的用途是 "ensure a class has only one instance, and provide a global point of access to it"（确保每个类只有一个实例，并提供它的全局访问点）。

它可以解决什么问题，或者换句话说，我们使用它的动机是什么？ 几乎在每个应用程序中，都需要有一个从中进行全局访问和维护某种类型数据的区域。 在面向对象的 (OO) 系统中也有这种情况，在此类系统中，在任何给定时间只应运行一个类或某个类的一组预定义数量的实例。 例如，当使用某个类来维护增量计数器时，此简单的计数器类需要跟踪在多个应用程序领域中使用的整数值。 此类需要能够增加该计数器并返回当前的值。 对于这种情况，所需的类行为应该仅使用一个类实例来维护该整数，而不是使用其它类实例来维护该整数。

最初，人们可能会试图将计数器类实例只作为静态全局变量来创建。 这是一种通用的方法，但实际上只解决一部分问题；它解决了全局可访问性问题，但没有采取任何措施来确保在任何给定的时间只运行一个类实例。 应该由类本身来负责只使用一个类实例，而不是由类用户来负责。 应该始终不要让类用户来监视和控制运行的类实例的数量。

所需要的是使用某种方法来控制如何创建类实例，然后确保在任何给定的时间只创建一个类实例。 这会确切地给我们提供所需的行为，并使客户端不必了解任何类细节。

**逻辑模型**

Singleton 模型非常简单直观。 （通常）只有一个 Singleton 实例。 客户端通过一个已知的访问点来访问 Singleton 实例。 在这种情况下，客户端是一个需要访问唯一 Singleton 实例的对象。 图 1 以图形方式显示此关系。

![图 1. Singleton 模式逻辑模型]()
图1. Singleton模式逻辑模型

**物理模型**

Singleton 模式的物理模型也是非常简单的。 但是，随着时间的推移，实现 Singleton 的方式也略有不同。 让我们看一下原始的 GoF Singleton 实现。 图 2 显示按 Design Patterns 所定义的原始 Singleton 模式的 UML 模型。

![图 2. Design Patterns 中的 Singleton 模式物理模型]()
图2. Design Patterns中的Singleton模式物理模型

我们看到的是一个简单的类图表，显示有一个 Singleton 对象的私有静态属性以及返回此相同属性的公共方法 Instance()。 这实际上是 Singleton 的核心。 还有其他一些属性和方法，用于说明在该类上允许执行的其他操作。 为了便于此次讨论，让我们将重点放在实例属性和方法上。

客户端仅通过实例方法来访问任何 Singleton 实例。 此处没有定义创建实例的方式。 我们还希望能够控制如何以及何时创建实例。 在 OO 开发中，通常可以在类的构造函数中最好地处理特殊对象的创建行为。 这种情况也不例外。 我们可以做的是，定义我们何时以及如何构造类实例，然后禁止任何客户端直接调用该构造函数。 这是在 Singleton 构造中始终使用的方法。 让我们看一下 Design Patterns 中的原始示例。 通常，将下面所示的 C++ Singleton 示例实现代码示例视为 Singleton 的默认实现。 本示例已移植到很多其他编程语言中，通常它在任何地方的形式与此几乎相同。

**C++ Singleton****示例实现代码**

**Language****：****cpp****, parsed in: 0.004 seconds, using GeSHi 1.0.7.12**

1.  // Declaration

2.  **class** Singleton {

3.  **public**:

4.      static Singleton* Instance();

5.  **protected**:

6.      Singleton();

7.  **private**:

8.      static Singleton* _instance;

9.  }

10.  

11. // Implementation

12. Singleton* Singleton::_instance = 0;

13.  

14. Singleton* Singleton::Instance() {

15.     if (_instance == 0) {

16.         _instance = new Singleton;

17.     }

18.     return _instance;

19. }

20.  

让我们先花点时间分析一下此代码。 该简单类有一个成员变量，此变量是指向该类自身的指针。 注意，构造函数是受保护的，并且只有公共方法才是实例方法。 在实例方法实现中，有一个控制块 (if)，它检查成员变量是否已初始化，如果没有的话，则创建一个新实例。 控制块中这种惰性初始化意味着仅在第一次调用 Instance() 方法时初始化或创建 Singleton 实例。 对于很多应用程序，这种方法效果很好。 但对于多线程应用程序，这种方法证明具有潜在危险的副作用。 如果两个线程同时进入控制块，则可能会创建该成员变量的两个实例。 要解决这一问题，您可能想只将重要部分放在控制块周围以确保线程安全。 如果您这样做，则将对实例方法的所有调用进行序列化处理，并且可能会对性能产生不利影响（取决于应用程序）。 正是由于这个原因，创建了此模式的另一个版本，它使用某种称为双重检验机制的功能。 下一个代码示例显示使用 Java 语法的双重检验锁定。

**使用 Java 语法的双重检验锁定 Singleton 代码**

**Language****：****java, parsed in: 0.011 seconds, using GeSHi 1.0.7.12**

1.   // C++ port to Java

2.   **class** Singleton

3.   {

4.       **public** **static** Singleton Instance() {

5.           if (_instance == **null**) {

6.               **synchronized** (**Class**.forName("Singleton")) {

7.                   if (_instance == **null**) {

8.                       _instance = **new** Singleton();

9.                   }

10.               }

11.          }

12.          **return** _instance;     

13.      }

14.      **protected** Singleton() {}

15.      **private** **static** Singleton _instance = **null**;

16.  }

17.   

在使用 Java 语法的双重检验锁定 Singleton 代码示例中，我们直接将 C++ 代码移植到 Java 代码，以便利用 Java 关键部分块（已同步）。 主要差别是不再有单独的声明和实现部分，没有指针数据类型，并且采用了新的双重检验机制。 双重检验发生在第一个 IF 块上。 如果成员变量为空，则执行进入关键部分块，该块再次双重检验该成员变量。 仅在通过此最终测试后，才会实例化该成员变量。 一般来说，两个线程无法使用这种方法创建两个类实例。 另外，因为在第一次检查时没有出现线程阻塞，所以对此方法的大多数调用不会由于必须进入锁定而导致性能下降。 目前，在实现 Singleton 模式时，很多 Java 应用程序中都广泛使用这种方法。 这种方法很巧妙，但也有瑕疵。 某些优化编译器可以将惰性初始化代码优化掉或对其重新进行排序，并且会重新产生线程安全问题。 有关更深入的解释，请参阅 "The Double-Check Locking is Broken" Declaration。

另一种试图解决此问题的方法可能是，在成员变量声明中使用 volatile 关键字。 这应该告诉编译器不要对代码重新排序，并且放弃优化。 目前，这是唯一建议的 JVM 内存模型，并且不会立即解决该问题。

实现 Singleton 的最好方法是什么？ 最终（而不是碰巧），Microsoft .NET 框架解决了所有这些问题，从而更易于实现 Singleton，却不会产生我们目前讨论的不利副作用。 .NET 框架以及 C# 语言允许我们在必要时通过替换语言关键字，将上述的 Java 语法移植到 C# 语法。 因此，Singleton 代码变为以下内容：

**以 C# 编码的双重检验锁定**

**Language****：****csharp****, parsed in: 0.005 seconds, using GeSHi 1.0.7.12**

1. // Port to C#

2.  class Singleton

3.  {

4.      public static Singleton Instance() {

5.          if (_instance == null) {

6.              lock ([typeof](http://www.google.com/search?q=typeof+msdn.microsoft.com)(Singleton)) {

7.                  if (_instance == null) {

8.                      _instance = [new](http://www.google.com/search?q=new+msdn.microsoft.com) Singleton();

9.                  }

10.             }

11.         }

12.         return _instance;     

13.     }

14.     protected Singleton() {}

15.     private static volatile Singleton _instance = null;

16. }

17.  

此处，我们替换了锁定关键字来执行关键部分块，使用 typeof 操作并添加 volatile 关键字，以确保没有对代码进行优化程序重新排序。 虽然此代码或多或少是 GoF Singleton 模式的直接移植，但它可达到我们的目的，并且我们可获得所需的行为。 此代码还说明了将 C++ 移植到 Java 和将 Java 移植到 C# 代码的一些相似之处和主要差别。 但是，正如任何代码移植一样，通常目标语言或平台的一些优点可能在移植过程中失去。 需要做的就是对代码重构，以便利用新目标语言或平台的功能。

在前面的每个代码示例中，Singleton 的原始实现随时间的推移而发生变化，以解决在每个新模式实现中发现的问题。 一些问题（例如，线程安全）要求对大多数实现进行更改，以满足在目前应用程序中日益增长的需要并解决演变发展问题。 .NET 在应用程序开发中提供了一个演变步骤。 可以在“框架”级别解决前面示例中出现的很多亟待解决的问题，而不是在实现级别解决。 虽然上一个示例显示了一个使用 .NET 框架和 C# 的有效 Singleton 类，但只需更好地利用 .NET 框架本身就可以大大简化此代码。 以下示例使用 .NET，它是一个松散地基于原始 GoF 模式的最小限度的 Singleton 类，并且仍然可获得类似的行为。

**.NET Singleton****示例**

**Language****：****csharp****, parsed in: 0.002 seconds, using GeSHi 1.0.7.12**

1. // .NET Singleton

2. sealed class Singleton

3. {

4.     private Singleton() {}

5.     public static readonly Singleton Instance = [new](http://www.google.com/search?q=new+msdn.microsoft.com) Singleton();

6. }

7.  

此版本已大大简化并且更加直观。 它仍然是 Singleton 吗？ 让我们看一下更改了哪些内容，然后再做决定。 我们修改了要密封的类本身（该类密封后是不可继承的），删除了惰性初始化代码，删除了 Instance() 方法，并且对 _instance 变量做了大量的修改。 对 _instance 变量所做的更改包括修改对公共方法的访问级别，将变量标记为只读，以及在声明时初始化该变量。 此处，我们可以直接定义所需的行为，而不关心实现的潜在有害的副作用。 那么，使用惰性初始化有什么优点以及使用多个线程有什么危险呢？ 在 .NET 框架中内置了所有正确的行为。 让我们先看第一种情况：惰性初始化。

最初使用惰性初始化的主要原因是要获取仅在第一次调用 Instance() 方法中创建实例的行为，还因为 C++ 规范中具有某种开放性，并不定义静态变量的确切初始化顺序。 要在 C++ 中获得所需的 Singleton 行为，必须采用涉及使用惰性初始化的运算方法。 我们真正关心的是在第一次（在该情况下）调用实例属性中创建该实例，还是在此调用之前创建该实例的，并且类中的静态变量是否有已定义的初始化顺序。 对于 .NET 框架，这就是我们获取的行为。 在 JIT 过程中，当（且仅当）任何方法使用静态属性时，“框架”将初始化此静态属性。 如果没有使用该属性，则不会创建实例。 更准确地说，在 JIT 过程中发生的事情就是，在任何调用方使用该类的任何静态成员时构造和加载该类。 在这种情况下，结果是相同的。

那么，线程安全初始化呢？ “框架”也解决了这一问题。 “框架”内部保证静态类型初始化的线程安全。 换句话说，在上面的示例中，只创建一个 Singleton 类实例。 还要注意，用于保存类实例的属性字段称为实例。 此选项更好地说明了，在本文中的讨论过程中，此值是类的实例。 在“框架”本身中，虽然使用的属性名称称为值，但有多个类使用此类型的 Singleton。 概念完全相同。

对类所做的其他更改意味着禁止划分子类。 添加密封类修饰符可确保不会将该类划分为子类。 GoF Singleton 模式详细介绍了试图对 Singleton 划分子类所产生的问题，该划分通常并不是小事。 在大多数情况下，可以很容易地开发没有父类的 Singleton，并且添加划分子类功能会增加通常根本不需要的新的复杂性级别。 随着复杂性的提高，测试、培训和文档编制等所需的时间也会增加。 通常，除非绝对必要，否则您不希望提高任何代码的复杂性。

让我们看一下如何使用 Singleton。 使用我们最初的计数器的有关动机的概念，我们可以创建一个简单的 Singleton 计数器类并说明我们将如何使用它。 图 3 显示了 UML 类说明将包含什么内容。

![图 3. UML 类图表]()
图 3. UML 类图表

相应的类实现代码以及示例客户端使用如下所示。

**示例 Singleton 使用**

**Language****：****csharp****, parsed in: 0.007 seconds, using GeSHi 1.0.7.12**

1. sealed class SingletonCounter {

2. public static readonly SingletonCounter Instance =

3. [new](http://www.google.com/search?q=new+msdn.microsoft.com) SingletonCounter();

4. private long Count = 0;

5. private SingletonCounter() {}

6. public long NextValue() {

7. return ++Count;

8. }

9. }

10.  

11. class SingletonClient {

12. [STAThread]

13. static void Main() {

14. for (int i=0; i&lt;20; i++) {

15. Console.WriteLine("Next singleton value: {0}",

16. SingletonCounter.Instance.NextValue());

17. }

18. }

19. }

20.  

此处，我们还创建了一个 Singleton 类来维护具有 long 类型的增量计数。 客户端是一个简单的控制台应用程序，它显示计数器类的 20 个值。 虽然此示例极其简单，但它却说明了如何使用 .NET 来实现 Singleton，然后将其用在应用程序中。

**小结**

Singleton 设计模式是一个非常有用的机制，可用于在面向对象的应用程序中提供单个对象访问点。 无论使用的是什么实现，该模式提供一个大家所熟知的概念，以便其在设计和开发小组之间方便地进行共享。 但是，正如我们所发现的一样，注意到这些实现有多大差异及其潜在的副作用也是非常重要的。 .NET 框架为模式实现者在设计所需的功能类型方面提供了很大的帮助，实现者无需处理本文中所讨论的很多副作用。 在正确实现后，可以证实模式的最初目的的有效性。

设计模式是非常有用的软件设计概念，可使小组将重点放在提供最佳类型的应用程序上，而不考虑它们是什么应用程序。 关键在于正确而有效地使用设计模式，目前有很多关于将设计模式用于 Microsoft .NET 方面的 MSDN 系列文档，其中介绍了如何正确而有效地使用设计模式。

 
