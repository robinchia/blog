---
layout: post
title: "Groovy创始人：Java面临终结 Scala将取而代之 - CSDN Java频道"
categories: scala
tags: 
 - scala
--- 

# Groovy创始人：Java面临终结 Scala将取而代之 - CSDN Java频道

您还未登录！|[登录](https://passport.csdn.net/account/login)|[注册](https://passport.csdn.net/account/register)|[帮助](https://passport.csdn.net/help/faq)

* [首页](http://www.csdn.net/)
* [业界](http://news.csdn.net/)
* [移动](http://mobile.csdn.net/)
* [云计算](http://cloud.csdn.net/)
* [研发](http://sd.csdn.net/)
* [论坛](http://bbs.csdn.net/)
* [博客](http://blog.csdn.net/)
* [下载](http://download.csdn.net/)
* 
## [更多]()

[![CSDN Java频道]()](http://news.csdn.net/)您的位置：[CSDN 首页](http://www.csdn.net/) > [Java频道](http://java.csdn.net/) > 正文
# Groovy创始人：Java面临终结 Scala将取而代之

2010-08-11 16:22 | 1976次阅读 | 【已有29条评论】[发表评论]()

来源：51CTO | [收藏到我的网摘]( "收藏到我的网摘中，并分享给我的朋友")

Groovy创始人James Strachan前日在其[博客](http://macstrac.blogspot.com/2009/04/scala-as-long-term-replacement-for.html)（地址在Blogspot，未架好梯子前请勿随便点击）上发表了一篇文章，题目为《[Scala as the long term replacement for java/javac?](http://macstrac.blogspot.com/2009/04/scala-as-long-term-replacement-for.html) 》。以下是正文部分的翻译：

不要误解我的意思——我在过去的这十来年里写了无数的Java代码，并且坚信它相对C++和Smalltalk来说是一个巨大的进步（当然，很多其它语言也很有帮助，像JavaScript，Ruby，Groovy，Python等）。但是我还是一直期待着能有javac的替代者出现。我甚至还自创了一门[语言](http://groovy.codehaus.org/)（编者注：此处指Groovy）好让我暂时满足一下这种期望。

Java是一种令人惊叹的复杂语言（它的语法规范长达600页，我怀疑到底有没有人能真正理解它），它有自动装箱(Autoboxing)，空指针异常(NPE)往往就是这时抛出的。其中的基本类型(primitive type)，字符串/文字/缓冲器/集合类(collections)以及数组缺乏多态性，以至于处理任何数据结构都需要冗长的语法；而且，由于Bean属性和对闭包支持的缺失（甚至在JDK 7里也仍然还不支持），这会让你的代码里充满了 try/catch/finally 这些语句（除非你使用框架和新的自定义API）。除了这些还有好多数不清的麻烦问题。Java倒是有类型推断（type inference）功能但却不用，使得我们要多输/读如此大量的代码。

这个问题在没有Java7后变得更加紧迫 （在Snorcle之后它变得更加重要：我不知道javac是不是要被jdkc 取而代之了？）。所以我猜javac可能已经走到了尽头，它看起来根本就没有什么进展或简化了。

那么，从长久来看，谁能取代javac 呢？当然，像Ruby，Groovy，Python，还有JavaScript这些动态语言在过去几年里很受欢迎——很多人喜欢他们。

**我认为将来可能替代javac的就是Scala 。**它实在太让我印象深刻了。我甚至可以诚实地说，如果有人在2003年把Martin Odersky，Lex Spoon以及Bill Venners写的那本《Programming in Scala》拿给我看了的话，那我根本就不会再去发明Groovy了。

那么，为什么我会看好Scala呢？Scala是静态类型的，它可以被编译成与Java同样快速的字节码，所以它的速度与Java不分上下（有时快一点，有时慢一点）。你可以看看 Scala 在与 groovy 或jruby一起进行测试时表现有多好。注意：速度并不是我们追求的唯一目标——有时候我们可能宁肯让代码慢上十倍，也要写得简洁一点；但是如果要取代javac，速度当然还是很重要的。

Scala已有类型推理(type inference)功能，因此它和Ruby/Groovy一样简洁，但是它完全是静态类型的。这是很有好处的，它使得理解代码、阅读代码以及编写文档都简单多了。在任何片段(token)/方法/符号上点击，你都可以跳转到相应的代码或文档中去浏览。不需要打那些怪异的补丁，也不用操心谁什么时候新增了一个方法——这对于那些需要一个团队一起长期开发的大项目是很有好处的。Scala似乎已经实现了动态语言(dynamic language)的那种简洁，而实际上它是完全静态类型的。所以，我根本不需要去记哪些魔术方法可用——或是在shell里运行脚本来查看这些对象——IDE/编译器在你编辑代码时就已经知道这些了。

Scala已经提供了对高阶函数和闭包的支持，另外还支持序列解析（sequence comprehensions) ，这样你就可以很容易用Scala写出漂亮简洁的代码。Scala还把函数式和面向对象的编程思想很好地统一到了一种语言里，它比Java要明显简单一些（虽然它的类型体系(type system)和泛型(generics)需要花费差不同一个数量级的时间去理解，但是，它通常是框架开发者才需要考虑的问题，应用程序开发人员并不需要涉及）。它也使得从传统的面向对象/Java编程模式向函数式编程的转变变得更加容易——这对于编写并行或异步程序的开发人员尤其意义重大（这是因为现在芯片的主频已经达到了数个GHz，很难再有提升了；而芯片集成的核心数则在快速增长。51CTO之前曾发布过哪种语言将统治多核时代 再看函数式语言特性一文，对于函数式语言在多核时代的潜力做了相当深入的分析）。你可以在最开始用面向对象的方法编程，然后当你需要它的好处时，就可以迁移到用不变状态(immutable state)函数式编程正变得越来越重要，因为我们总是希望能把问题变简单，并且在一个更高的层次上解决它（如闭包，高阶函数，模式匹配，单子(monad)等），同时我们还需要通过不变状态(immutable state)实现并发和异步。

Scala也有适当的混入(mixin)（特性(trait)） ，所以你不必去摆弄面向对象编程的缺陷来获得模块化的代码。如果你确实需要一些鸭子类型(duck typing)，Scala甚至能为你提供构造类型(structural type)。

最让我印象深刻的一点就是它的核心语法极其精练简洁（它的语法手册只有大概Java的四分之一），但是其方式却更加强大和灵活，而且非常容易通过库来扩展，添加新的语义和功能。可以看看这个例子：Scala Actors。因此它非常适合用于创建嵌入式DSL或外部DSL 。有了它以后就真没必要再用Java，XPath ，XSLT，XQuery，JSP，JSTL，EL和SQL这些东西了——你可以在各种各样的场合使用DSL。

Scala确实需要花点时间去习惯——我承认第一次我看Scala时并不觉得顺眼——用了Java之后你就会习惯用一堆冗长的代码来做一点点事，刚开始时我们也都不会一看到几个符号就觉得有多惊讶。（我花了好长一段时间才习惯Scala里用_作“通配符”，因为在Scala里是用作标识符/方法）。

如果你一直在写Java，那么最开始确实会觉得Scala很不一样（如在声明方法/变量/参数时在类型或标识符上加上阶，虽然那样做的原因是为了能更方便地略去一多余的类型信息）。

例如，在Java中的写法：

List< String> list = new ArrayList< String>()

在Scala中的写法：

val list = new List[String]

或者，如果你要指定确切的类型的话：

val list : List[String] = new List[String]

但是如果你坚持用上一段时间，Scala优美的一在很快就显现出来。它对Java里的许多地方进行了简化，让你可以用非常简洁的代码就描述出意图，而不用花上大段代码去实现细节——同时还为你提供了一条迁移到函数式编程的不错途径，这对于编写并发和分布式程序是非常有利的。

我强烈建议你学习一下Scala：以开放的心态看看（当你的思维转过来后）你是否能发现它的美丽之处。

**一些Scala资料的链接和在线演示文档**

> 我强烈推荐由 Martin Odersky，Lex Spoon 和 Bill Venners编写的《[Programming in Scala](http://www.amazon.com/Programming-Scala-Comprehensive-Step-step/dp/0981531601/ref=sr_1_1?ie=UTF8&s=books&qid=1240563267&sr=8-1)》 一书。它非常好地介绍了Scala的特点以及设计时的选择。这本书相当厚，但是你可以先跳着读，必要时再深入细节。

> 《[O'Reilly Scala book](http://programming-scala.labs.oreilly.com/)》这本书我只跳着读了一点，但是看起来也非常不错。

> 如果你想在短时间内就知道个大概语法，那么可以看看《[Tour of Scala](http://www.scala-lang.org/node/104)》。不过看了之后你也还得花上一些时间来真正理解为什么它跟Java会有这样那样的不同。

> Martin Odersky 的JavaOne 2008上[关于Scala的演说](http://developers.sun.com/learning/javaoneonline/2008/pdf/TS-5165.pdf)

>[Jonas Bonér](http://jonasboner.com/)在[Real-World Scala](http://www.slideshare.net/jboner/pragmatic-real-world-scala-45-min-presentation)上作的报告

> Gert's的关于他如何创建 [Apache Camel](http://camel.apache.org/) [DSL for Scala](http://camel.apache.org/scala-dsl.html) 的介绍

> 用于[JDBC类型安全查询](http://szeiger.de/blog/2008/12/21/a-type-safe-database-query-dsl-for-scala/)的一个Scala版LINQ，顺便再了解下[DBC](http://scala.sygneca.com/libs/dbc)。

> 一份非常不错的报告，介绍了把[Scala 和OSGi](http://osgilook.com/2009/05/08/osgi-on-scala/)与DSL结合使用

> 如何使用[Scala和XML](http://www.ibm.com/developerworks/library/x-scalaxml/) （ 语言里已经自带了处理XML，XPath ， XSLT， XQuery的简洁语法）

> [Scala的例子](http://www.scala-lang.org/docu/files/ScalaByExample.pdf)

> [Scala快速参考表](http://www.geekontheloose.com/images/stories/programming/Scala_Cheatsheet.pdf)

> 这个例子显示了如何创建的[bean风格的属性](http://www.scala-lang.org/node/29)（或C ＃风格的getter函数）

> 创建一个[用Lift实现的聊天演示程序](http://liftweb.blip.tv/)或查看[Lift网站](http://liftweb.net/)上的更详细介绍

如果你还有一些空余时间的话，这些视频资料也非常不错

> Bill Venners所发表的[The Feel of Scala](http://www.parleys.com/display/PARLEYS/Home#talk=27131945;slide=1;title=The Feel Of Scala)

> Lex Spoon所作的[Scala： 把未来的语言带到JVM里来](http://www.infoq.com/presentations/jaoo-spoon-scala)

**好用的Scala框架和库**

> [liftweb](http://liftweb.net/) ：Scala的rails

> [语言规范](http://code.google.com/p/specs/)和[ScalaTest for BDD](http://www.artima.com/scalatest/)以及其它一些入门测试(literate testing)能让你体会到类型安全的DSL对于编写IDE友好 的简洁代码有多大帮助。

> [scalaz](http://code.google.com/p/scalaz/) 是一个很有用的例程库。

> 用[HTTP /JSON服务进行调度](http://databinder.net/dispatch/Guide)

另外，顺便说一下，对于那些像我一样一喜欢JAXRS的，现在可以通过jersey-lift模块使用lift模板和Jersey了。

作为这的实例，你可以看看RestMQ，这是一个我最近也参与了的开源项目，它旨在为面向消息的中间件提供REST风格的API和Web控制台，它也是基于JAXRS（Jersey），Scala，Lift构建的。

至于开发工具方面，有Ant/Maven插件，它带有一个交互式Scala控制台（REPL）和一个用于IDEA的IDE插件，还有Eclipse，NetBeans，以及TextMate，Emacs这些通用编辑器，都可以供你选择。在IDE插件的丰富程度上与Java还是有差距的，但是这些工具所提供的代码导航和自动补全功能还是很有用的。

我试用过NetBeans，Eclipse和IDEA这几个IDE上的插件，它们都各有优劣。看起来，Scala的追随者也因为这些工具分裂成了几派。如果要代码导航和自动补全，那我发现IDEA非常不错。当你打开一个Maven pom.xml，它好像就能非常好地自动解析代码，找出Scala源，那样你就能很方便地在任意的类型/方法以及它们对应的文档/源代码中跳转浏览。（通常你必须在运行/调试任务里手动添加Scala）。不过IDEA在错误代码高亮上并不是最好的。在作上一些弥补后，它们都能变得与对应的Java工具一样好用。试试这几个工具吧，找出你最习惯的那个。

**Scala Nit**

任何一种语言都有你喜欢的一面，也有你不是那么热衷的一面。Scala给你的最初印象可能确实是符号太多了点，但是你并不需要使用所有的这些符号——如果你喜欢的话，你可以继续沿用很多Java里的东西。但我想到了那个时候最好还是用符号来实现“特殊任务”以避免标识符冲突。

我对嵌套的引入声明不太感冒，使用_root_.java.util..List来把一个”全局“引入和相对引入区别开来。我还更愿意使用子引入。例如，如果你引入了com.acme.cheese.model.Foo，然后，为了引入model.impl.FooImpl，我就更喜欢用一个明确的相对前缀，就像：import _.impl.FooImpl。这对简化任务有一些好处，对于保持和Scala的简洁性就更有帮助了。

然而，和Java里大把的毛病相比，再考虑到Scala的优美，简洁和强大，Scala的这一些负面因素和根本不算什么了。

**结语：**

既然 MrJava（[Adam Bien](http://www.adam-bien.com/roller/abien/entry/java_net_javaone_which_programming)），MrJRuby（[Charles Nutter](http://blog.headius.com/2009/04/future-part-one.html)） 和 MrGroovy（作者本人） 都认为Scala将会是javac的的替代者，那肯定是有些原因的。那你还在等什么呢？赶快去买《Programming in Scala》 或 《O'Reilly Scala book》一探究竟吧！

这篇博文发布后，立刻有很多Scala，Groovy和Java开发者进行了回复。Scala的创始人Martin Odersky也对这篇文章发表了自己的赞赏之词。以下是Martin的留言：

James，感谢你的认可！这对我来说意义重大。我相信，如果我们一起努力向Java开发者们展示现在在JVM上更加美好的语言选择，我们大家都会因此而得到好处。感谢你在这方面带了个好头。

根据我对Groovy的了解（很可惜的，我的了解没有你了解Scala那样多），它看起来并非是意图填补同一块领域的。Groovy的吸引力在于它是一个语法接近Java的动态脚本语言。Scala的吸引力在于它是一个强类型的，静态的，结合函数式和面向对象的语言。

此外还有很多精彩的评论，51CTO译者对这些评论进行了一些筛选，挑出部分翻译如下：

**Scala的体验**

去年，我在做一些调查项目时把Scala引进到了我的小Java车间里。

如今Scala成为了我们最主要的编程语言。

通过使用Scala，我们现在可以构建类型系统(type system)，跟踪总结以前所做项目的经验教训，并用它来替代我们过去以模型为导向(model-driven)的开发方式。然后，我们利用函数分发(pass around functions)的特性来改进组件的参数化。

总之，对于建立可重用的组件，Scala提供了一套比Java更好的机制。

**C#和Java？**

我觉得你可以去看看C#。它解决了你在Java中遇到的许多问题。如果你不喜欢微软的话，就可以试试.NET的开源替代版本Mono。

**有关Scala和F#**

其实，在.NET平台里与Scala对应的语言并不是C#，而是F#。不管什么时候，我都更倾向于使用Scala，而不是F#，原因如下：

1 ）在F#文化里，面向对象看起来并不重要。在所有讲F#的书里，都必定有一章介绍类，然后，剩余部分就是专门讲解函数式。相比之下，Odersky在发明Scala时，并没有照搬Java的这一套机制，而是通过对象类型、特性(trait）、增强的可见性规则(visibility rule)等概念扩展并超过了Java的这一套机制。Scala使得像我这样有根深蒂固面向对象思想的开发人员觉得很舒适，它提供的函数式语法特性让我可以用来把代码变得非常简洁。

2 ）F#比Scala看起来更接近人类语言，初看起来这似乎是好事。不幸的是，由于开发者很少需要写类型说明(type annotation)，大部分代码里也都没写，这就使得代码变得更加难于理解。在Scala里，至少要声明参数类型，而且最好也声明一个方法的返回类型，除了那些一目了然的情况。

3 ）F#一直力求尽量往OCAML的语法靠拢，所以它在语法也真是没有什么创新之处。而Scala则是博取众长，吸纳了各种语言的优点。此外，它还让人感觉有些机制并不是必须的，而是为了让开发者更好地表达意图而加入的。通过加入隐式转换(implicit conversion)，析取器(extractor)这些功能，Martin从我这里得到了很大的帮助。

4 ）在我看来，Java程序员学会Scala比从C#到F#的过渡要容易得多。大体上来说，原因是Java程序员不需要花很大的代价入门，Scala可以直接被当作一门少了些模板(boilerplate)的Java使用。当程序员渐渐熟练后，他就可以开始发掘函数式编程的威力了。在其它任何的面向对象/函数式编程语言里我都找不出可以这样过渡学习的。

**Groovy盖棺定论了？**

James，我一直在留意你的博客，这篇文章写得棒极了，堪称高超。你发过一份声明说不会再继续把Groovy开发得更强大了（51CTO编者注：James Strachan在写这篇博文之前很久已经离开了Groovy开发团队），这份声明影响力很大，而且几乎可以说是给Groovy盖棺定论了。

我们有一个面向最终用户的数据处理软件，然后我们选择的是Groovy （而没用Jython和JRuby ）来作为实现各种功能扩展（从对变量编写公式到编写脚本）的途径。你们在开发Groovy所写的代码很多都是粘合代码(glue code)(对核心语言起补充作用）。我们充分利用Groovy所支持的这些特性与MS Office产品和Web服务进行整合。我真的希望，如果你们的开发团队更中意Scala的话，也请尽量让我们到时候在Scala里也能用上这些有用的库。

**James Strachan对上文的回复**

我不认为任何一种主要的JVM语言会消失，肯定会一直有一大帮人继续维护Groovy, Jruby, Cojure, Jython, Rhino等。

JVM中最大的一点好处就是这些语言很容易共存，重用另一种语言的代码也非常容易。因此，只要相信大众的选择，就不用担心会选错开发语言。

而且我也并不认为Scala会是Ruby/Groovy/Fan这些动态语言的替代者；大多数情况下性能还是很重要的。对于一个快速、静态类型的编译器来说，过去Java显然是第一选择——但是现在，Scala才是首选——这是因为Java已经显出老态了。（它可能永远也不会支持闭包，永远也不会考虑支持类型推断等新特性）。

自从发现了类型推断的威力之后，我实际上越来越觉得动态类型(就是很简洁的代码实现功能）的动机变得越来越难以琢磨了。比如说，你可以用Scala写一些脚本，它就会像Ruby/Groovy一样进入”读取-执行-打印 循环“(Read-Evaluate-Print Loop, REPL)。

但是我发这篇文章的目的并不是要挑起Scala拥护者和Ruby/Grovy/Clojure/JavaScript这些动态语言支持者之间的战争——我只是想让被Java一叶障目的开发者们意识到，这个世上已经有了比Java更好的静态类型语言：这门语言有他们所想要的全部功能（还附带有Java最需要增强的功能）。所有这一切，都能在这门语言里用简洁、优美的代码表示出来（尽管这门语言和Java确定有些不太一样，并且需要你经历一个学习曲线）。

**附录：有关Scala编程语言的其他言论**

> Java的不足可以比作大量的毛疣，那么同样在Scala中，这些地方正是表现了Scala的美、简化和强大。——James Strachan

> 在一个社区（java.net booth）举办的和James Gosling对话会议上，一个与会者问了一个非常有意思的问题：“除了Java，现在你会把哪种语言运行于JVM之上？”。答案是惊人地快速简洁：Scala。——Java之父James Gosling

> 我必须说Scala看起来是是现在Java王座的继承人。其他在JVM的语言看起来不可能有Scala那样的能力来取代Java，Scala背后的推动力是无可置疑的。Scala还不是一个动态语言，但是它有许多流行动态语言的特性，例如它的灵活富类型系统，稀疏和简洁的语法，函数式语言和面向对象范式的完美结合。Scala的缺点：“太复杂”或者“太丰富”，但这些可以通过编码规范很好避免，从而构建更健壮的编辑器和工具，以及指导多语言开发者明白如何更好地使用Scala。Scala是JVM上静态语言的重生，它也像JRuby那样延伸平台的性能，这些都是Java做不到的。——JRuby核心开发者Charles Nutter

**【CSDN小百科】**

![]()

James Strachan

James Strachan，Groovy创始人，是Apache ActiveMQ、Apache Camel、Apache ServiceMix等开源项目创始人之一。现工作于FuseSource。

原文链接：[http://developer.51cto.com/art/200907/134785.htm](http://developer.51cto.com/art/200907/134785.htm)

【 [发表评论]() 29条 】[![]()]() [![]()]( "分享到QQ微博")
* 
* [![CSDN]()](http://weibo.com/csdn)
* [CSDN](http://weibo.com/csdn)
* 全球最大中文IT社区
* 
* [![CSDN移动频道]()](http://weibo.com/csdnmobile)
* [CSDN移动频道](http://weibo.com/csdnmobile)
* 专注于移动应用开发者的创优和创富
* 
* [![CSDN云计算频道]()](http://weibo.com/csdncloud)
* [CSDN云计算频道](http://weibo.com/csdncloud)
* 做领先的云计算技术传媒
* 
* [![程序员杂志]()](http://weibo.com/programmermag)
* [程序员杂志](http://weibo.com/programmermag)
* 面向软件开发者及管理者的专业月刊
* 
* [![CSDN蒋涛]()](http://weibo.com/csdncto)
* [CSDN蒋涛](http://weibo.com/csdncto)
* CSDN和《程序员》创始人
* 
* [![刘江]()](http://weibo.com/turingbook)
* [刘江](http://weibo.com/turingbook)
* CSDN&《程序员》总编

[]()
相关文章 [敏捷宣言创始人：十年之后看“修炼”](http://java.csdn.net/a/20100805/277863.html "于2009年9月11日召开的敏捷中国大会2009上，极限编程创始人Kent Beck、敏捷宣言创始人之一Dave Thomas，国际敏捷权威专家") [JavaEye创始人：为什么要用非关系数据库？](http://java.csdn.net/a/20100729/277476.html "随着互联网web2.0网站的兴起，非关系型的数据库现在成了一个极其热门的新领域，非关系数据库产品的发展非常迅速") [**Go语言创始人对Java、C++的复杂性不满**](http://java.csdn.net/a/20100727/277294.html "谷歌高管Rob Pike 在OSCON 开源大会上打开了简化式编程语言新议题 今天的商业级编程语言--尤其是C++和Java--太过复杂而") [Groovy 1.7.3发布 值得关注的新功能](http://java.csdn.net/a/20100621/267701.html "基于JVM的新型编程语言Groovy近日发布了1.7.3版本。在新版中，使用的安全性更加可靠，对properties文件静态更新等等都") [IDC：Oracle面临Java管理挑战](http://java.csdn.net/a/20100420/263957.html "Java之父James Gosling本周离开了Oracle，扔掉了Oracle赋予给他的客户端软件集团CTO的职位，使聚光灯再次转向现已归属数据")  [Groovy框架 Grails 1.2 发布](http://java.csdn.net/a/20100127/258596.html "Grails是一套用于快速Web应用开发的开源框架，它基于Groovy编程语言，并构建于Spring、Hibernate和其它标准Java 框架之上，")
网友评论(共29条评论).. Groovy创始人：Java面临终结 Scala将取而代之

* [![]()](http://my.csdn.net/deping_chen)deping_chen 2013-05-11 10:32:28

这么多大牛推荐，必有可取之处啊。

[回复(0)]() [支持(0)]() [反对(1)]() [举报(0)]() | 0条回复..
* [![]()](http://my.csdn.net/kuailelxl)kuailelxl 2010-08-12 19:16:33

我的第一反应是，把.NET当空气了。
看了前面几段，基本上看不懂啥意思，也许他说的非常专业的技术名词，但是在商业领域上，目前JAVA除了速度慢点外没发现其他大缺点，尤其是在可读性上，简化了功能的java在可读性上要比C/C++以及C#强大许多。

[回复(8)]() [支持(1)]() [反对(2)]() [举报(0)]() | 8条回复..

* [![]()](http://my.csdn.net/FrankHB1989)FrankHB1989 2010-11-08 22:35:03

批量培养民工的成本减低了，似乎是优点，但也有可能是缺点……

[回复(2)]() [支持(1)]() [反对(0)]() [举报(0)]()
* [![]()](http://my.csdn.net/luoxiafei)luoxiafei 2010-11-23 12:53:47 *引用内容*FrankHB1989 2010-11-08 22:35:03批量培养民工的成本减低了，似乎是优点，但也有可能是缺点……..

你还别看不起民工，谁不是从代码民工来的？难道你脑子天生威武牛逼的一塌糊涂，生来就能为卫星写代码。做人做事能别带色不？

[回复(1)]() [支持(1)]() [反对(2)]() [举报(0)]()
* [![]()](http://my.csdn.net/FrankHB1989)FrankHB1989 2010-11-24 20:13:22 *引用内容*luoxiafei 2010-11-23 12:53:47你还别看不起民工，谁不是从代码民工来的？难道你脑子天生威武牛逼的一塌糊涂，生来就能为卫星写代码。做人做事能别带色不？..

不要把初学者和代码民工混为一谈。

[回复(0)]() [支持(3)]() [反对(0)]() [举报(0)]()
* [![]()](http://my.csdn.net/dda_dda_dda)dda_dda_dda 2010-12-29 14:18:08 *引用内容*FrankHB1989 2010-11-08 22:35:03批量培养民工的成本减低了，似乎是优点，但也有可能是缺点……..

编程语言只不过是一种表达方式，和你用户普通话，英语说话没有什么区别。会写程序即不说明你聪明，也不会说明你笨。重要的是你是否能够用程序实现你的想法，能够做出什么的作品。马云不会写程序，但是可以弄一个淘宝，很多学习了大量语言的人，可能连一个最简单的作品都没有。

[回复(1)]() [支持(0)]() [反对(1)]() [举报(0)]()
* [![]()](http://my.csdn.net/FrankHB1989)FrankHB1989 2010-12-31 16:51:50 *引用内容*dda_dda_dda 2010-12-29 14:18:08编程语言只不过是一种表达方式，和你用户普通话，英语说话没有什么区别。会写程序即不说明你聪明，也不会说明你笨。重要的是你是..

学习语言仅仅就只是学习使用语言么？于是会说普通话就让中文系的全喝西北风？
学习语言从来都不是学习语言本身。对于自然语言，文化背景是重要组成部分，因为在某些情况下可能极大地影响语义。对于程序设计语言，语言的设计、实现、历史背景都是重要且复杂的知识（当然对你而言有用没用另当别论），不会因为你无视而不存在。
认识到语言是工具，不应被工具束缚而阻碍你需要利用工具解决的问题——你了解了一个常识；认为语言只是工具——你无视了另一个常识。

[回复(2)]() [支持(1)]() [反对(0)]() [举报(0)]()
* [![]()](http://my.csdn.net/sytu_hzj)sytu_hzj 2011-01-01 18:28:49

可读性比C#强？你傻B了吧，我用了三年C#，一年Java，没发现Java什么地方可读性比C#强

[回复(0)]() [支持(3)]() [反对(4)]() [举报(0)]()
* [![]()](http://my.csdn.net/kevin_zhang1983)kevin_zhang1983 2012-08-17 16:43:10 *引用内容*FrankHB1989 2010-12-31 16:51:50学习语言仅仅就只是学习使用语言么？于是会说普通话就让中文系的全喝西北风？
学习语言从来都不是学习语言本身。对于..

部分同意ls观点，语言就是个工具，用来赚钱即可，对于自己钻研的语言也别装高端了， 各种语言就是谋生手段， 大家都是混饭吃。 不用扯那些忽悠外行的人的东西，还什么文化背景，想吐的心都有了。

[回复(0)]() [支持(0)]() [反对(0)]() [举报(0)]()
* [![]()](http://my.csdn.net/z742698)z742698 2012-11-13 17:12:24 *引用内容*FrankHB1989 2010-12-31 16:51:50学习语言仅仅就只是学习使用语言么？于是会说普通话就让中文系的全喝西北风？
学习语言从来都不是学习语言本身。对于..

初学者和民工。中文系的人也是从会说普通话开始的。
这家伙搞C++的，自以为脱离民工阶层，属于高端应用领域。其实眼界不过还局限在自己的语言中了。需求和发展都看不到的给人打工的可怜虫罢了。

[回复(0)]() [支持(0)]() [反对(0)]() [举报(0)]()
* [![]()](http://my.csdn.net/xiazaihaoma123)xiazaihaoma123 2011-03-14 14:27:58

那些什么代码优美性能优越对你没作用，最重要的能让你拿上更高的工资，或者让你技术更上一个层次。至于别的不多说。

[回复(0)]() [支持(1)]() [反对(0)]() [举报(0)]() | 0条回复..
* [![]()](http://my.csdn.net/xiazaihaoma123)xiazaihaoma123 2011-03-14 14:26:14

工作用java， 非得把C搞熟练 ，这什么新语言一天出一个，妈的谁沙比才跟风 除非真的你有眼光，这语言马上会火能让你拿高薪，那就去学吧。不然你是浪费时间打酱油，个人意见别扔砖头。

[回复(0)]() [支持(1)]() [反对(0)]() [举报(0)]() | 0条回复..
* [![]()](http://my.csdn.net/marqio)marqio 2010-08-12 16:29:45

相信有98%的遗留系统（java开发的）需要升级呢，他们才不愿去用新的语言开发新的系统，宁愿少花点钱去升级维护，重要的适用，没有新的高级需求是不会考虑的。语言的发展也是需要客户的认可的，你用新语言开发的系统再好，客户就是不愿意使用有屁用。

[回复(3)]() [支持(5)]() [反对(3)]() [举报(0)]() | 3条回复..

* [![]()](http://my.csdn.net/lixkyx)lixkyx 2010-10-31 21:33:40

Scala的好处是它可以与Java无缝衔接，遗留系统你可以不必管它，在新增或者修改部分用上Scala就好了。

[回复(1)]() [支持(0)]() [反对(0)]() [举报(0)]()
* [![]()](http://my.csdn.net/lifeofxiaoxin)lifeofxiaoxin 2010-11-23 18:21:41 *引用内容*lixkyx 2010-10-31 21:33:40Scala的好处是它可以与Java无缝衔接，遗留系统你可以不必管它，在新增或者修改部分用上Scala就好了。..

真的?

[回复(1)]() [支持(0)]() [反对(0)]() [举报(0)]()
* [![]()](http://my.csdn.net/lixkyx)lixkyx 2011-01-22 11:05:11 *引用内容*lifeofxiaoxin 2010-11-23 18:21:41真的?..

不敢100%保证无缝衔接，但是绝大多数情况下是没有问题的。Scala本来就是工作在JVM上的，增加的Scala部分与原有的Java部分不冲突。而且，Scala专门内置了与Java的互操作部分。

[回复(0)]() [支持(0)]() [反对(0)]() [举报(0)]()
* [![]()](http://my.csdn.net/vargs)vargs 2011-01-12 15:09:31

java刚上路这无议于晴天霹雳...

[回复(0)]() [支持(0)]() [反对(0)]() [举报(0)]() | 0条回复..
* [![]()](http://my.csdn.net/ajjpool)ajjpool 2010-12-09 12:49:57

晕，保护自己

[回复(0)]() [支持(0)]() [反对(0)]() [举报(0)]() | 0条回复..
* [![]()](http://my.csdn.net/hansonjan)hansonjan 2010-09-02 10:19:30

看了此文,才真知道是天外有天.呵呵!
其实,本人在用java的时候也对那一堆堆的try{}catch(Exception ex){}感到无语.
看到有这么好的语言出来.需要时再好好学习一下.呵呵!

[回复(1)]() [支持(0)]() [反对(0)]() [举报(0)]() | 1条回复..

* [![]()](http://my.csdn.net/lifeofxiaoxin)lifeofxiaoxin 2010-11-23 18:22:24

一直抛出就行了.管它什么.我都是这么干的.哈哈.

[回复(0)]() [支持(1)]() [反对(0)]() [举报(0)]()
* [![]()](http://my.csdn.net/FrankHB1989)FrankHB1989 2010-11-08 22:33:44

mixin能避免“面向对象编程的缺陷”？这叫“有根深蒂固面向对象思想”？
这家伙不是被“纯OOP”洗脑了就是基础没学好。虽然从语气看起来也许比偏执狂James Gosling要靠谱一些。

[回复(1)]() [支持(1)]() [反对(0)]() [举报(0)]() | 1条回复..

* [![]()](http://my.csdn.net/lifeofxiaoxin)lifeofxiaoxin 2010-11-23 18:20:58

我感觉你最不靠谱,你写几篇让我们欣赏下呗.

[回复(0)]() [支持(0)]() [反对(0)]() [举报(0)]()
* [![]()](http://my.csdn.net/simpleframework)simpleframework 2010-08-13 14:44:21

国内javaweb框架
code.google.com/p/simpleframework/

[回复(0)]() [支持(0)]() [反对(1)]() [举报(0)]() | 0条回复..
* [![]()](http://my.csdn.net/SunJava2)SunJava2 2010-08-13 10:39:17

如何用Scala访问Oracle数据库？

[回复(0)]() [支持(0)]() [反对(0)]() [举报(0)]() | 0条回复..
* [![]()](http://my.csdn.net/guojun828)guojun828 2010-08-12 23:59:47

立刻开始Scala的旅程。。。

[回复(0)]() [支持(0)]() [反对(0)]() [举报(0)]() | 0条回复..
* [![]()](http://my.csdn.net/chengyingqi0512)chengyingqi0512 2010-08-12 20:48:14

这还得等上一段时间呢

[回复(0)]() [支持(0)]() [反对(0)]() [举报(0)]() | 0条回复..
* [![]()](http://my.csdn.net/Lee_Dewyze)Lee_Dewyze 2010-08-11 21:34:55

编程语言都是相通的 学会了Java学其他面向对象的语言很会很容易上手 我还是认为Java的前景还是很好的

[回复(1)]() [支持(5)]() [反对(2)]() [举报(0)]() | 1条回复..

* [![]()](http://my.csdn.net/mapeiqi2009)mapeiqi2009 2010-08-12 17:26:01

这是个商业问题，主要是为了维护现有的java代码，要转成另一种语言的话，花费的成本太大了！
而不是什么语言相通的问题....

[回复(0)]() [支持(1)]() [反对(0)]() [举报(0)]()
* [![]()](http://my.csdn.net/gaolinwu)gaolinwu 2010-08-12 00:47:27

支持上面的说法

[回复(0)]() [支持(2)]() [反对(1)]() [举报(0)]() | 0条回复..
[第一页]()[上一页]()[1]()[下一页]()[最末页]()

****[发表评论]()/共29条评论**..
[你还没有登录，请先登录](http://passport.csdn.net/UserLogin.aspx?from=http://java.csdn.net/a/20100811/278062.html)验证码： ![]( "看不清楚，点击换一张") 匿名评论(无需注册)

**请您注意**
·自觉遵守：爱国、守法、自律、真实、文明的原则
·尊重网上道德，遵守《全国人大常委会关于维护互联网安全的决定》及中华人民共和国其他各项有关法律法规
·严禁发表危害国家安全，破坏民族团结、国家宗教政策和社会稳定，含侮辱、诽谤、教唆、淫秽等内容的作品
·承担一切因您的行为而直接或间接导致的民事或刑事法律责任
·您在CSDN新闻评论发表的作品，CSDN有权在网站内保留、转载、引用或者删除
·参与本评论即表明您已经阅读并接受上述条款
****回复**
[你还没有登录，请先登录](http://passport.csdn.net/UserLogin.aspx?from=http://java.csdn.net/a/20100811/278062.html)验证码： ![]( "看不清楚，点击换一张") 匿名评论(无需注册)

# [更多](http://news.csdn.net/)本周热点排行

# 精彩专题

* [![]()第七届IDC大会](http://special.csdn.net/2013idc/index.html/1)
* [![](http://news.csdn.net/a/uploads/2010/04/20/20100420-204756-pic1.jpg)中国软件创富蓝海](http://subject.csdn.net/drsh/)
# [更多](http://blog.csdn.net/)推荐博文

[桂林.NET俱乐部运营经验分享](http://blog.csdn.net/softmse/archive/2010/01/10/5169315.aspx)[马拉多纳与程序员转型](http://blog.csdn.net/jinxfei/archive/2009/09/11/4544470.aspx)[如何给Ubuntu装备汉字库？](http://blog.csdn.net/yuanmeng001/archive/2009/09/16/4556909.aspx)[Java复习笔记-第7天](http://blog.csdn.net/sabic/archive/2010/01/18/5209254.aspx)[企业信息化，到底是虚热还是真正热？](http://blog.csdn.net/david_lv/archive/2009/09/11/4543324.aspx)[电梯里引发的运营思考](http://blog.csdn.net/mayongzhan/archive/2010/01/07/5153920.aspx)[Linux游戏产业迎来新曙光](http://blog.csdn.net/yuanmeng001/archive/2010/01/08/5155220.aspx)[应用程序开发随想](http://blog.csdn.net/edhroyal/archive/2009/09/12/4546957.aspx)[2010年百度被黑谷歌退出中国市场不为人知的内幕](http://blog.csdn.net/Seawelly/archive/2010/01/18/5206121.aspx)[SQLite多线程写锁文件解决方案](http://blog.csdn.net/skywalker256/archive/2009/09/17/4561065.aspx)

[公司简介](http://www.csdn.net/company/about.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)北京创新乐知信息技术有限公司 版权所有, 京 ICP 证 070598 号世纪乐知(北京)网络技术有限公司 提供技术支持![]()Copyright © 1999-2011, CSDN.NET, All Rights Reserved[![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010) ![](http://counter.csdn.net/pv.aspx?id=45) <a href="http://www.linezing.com"><img src="http://img.tongji.linezing.com/1273337/tongji.gif"/></a>
