---
layout: post
title: "Scala Tour"
categories: scala
tags: 
 - scala
--- 

# Scala Tour

5月1日是劳动的日子，笔者做了一个学习Scala精彩特性的网站[Scala-Tour](http://zh.scala-tour.com/ "scala-tour")。在学习Scala是时候，遇到很多令人激动的特性，主要函数式编程和并发。相比下Java已经老态龙钟，步履躇跚。或许Scala不会成为替代Java语言，但的确给后来者设立了标杆。所以做了这个网站，顺着一个一个例子，由浅入深，由表及里。逐渐学会Scala,尽管不会因此成为一个熟练Scala的开发者，但是对函数式编程的也会相当了然。这篇文章精选了Scala-Tour上了一些章节，想快速了解的朋友可以看看这篇文章，当然想详细看就上上[Scala-Tour](http://zh.scala-tour.com/ "scala-tour")吧。
![]( "Scala")

### 不再需要Close

在Java里面，使用完资源(文件句柄，数据库连接)等之后，必须手动Close。否则发生泄漏后，程序只有被迫重启。Scala可以通过函数式实现自动close。
import scala.reflect.io.File

import java.util.Scanner
 

def withScanner(f: File, op: Scanner => Unit) = {
    val scanner = new Scanner(f.bufferedReader)

    try {
        op(scanner)

    } finally {
        scanner.close()

    }
}

 
withScanner(File("/proc/self/stat"),

    scanner => println("pid is " + scanner.next()))

这个例子是从/proc/self/stat文件中读取当前进程的pid。withScanner封装了try-finally块，所以调用者不用再close。

### 按名称传递参数

我们熟悉的参数传递方式是按值传递。按名称传递的方式，可以理解为直接传递参数名字，等到实际调用的时候，再去取值。在Java代码中，往往充斥着if(log.isDebug()){log.debug(...)}这样语句。之前的if调用是很有必要的，因为在之后的debug语句中往往有字符串拼接的操作。在不需要打Log的时候，字符串拼接也有可能发生异常抛出。而Scala可以通过按名称传递解决这个问题，这样就不再需要if(log.isDebug())这样的语句了。
val logEnable = false

 
def log(msg: => String) =

    if (logEnable) println(msg)
 

val MSG = "programing is running"
 

log(MSG + 1 / 0)

### 鸭子类型

“走起来像鸭子，叫起来像鸭子，就是鸭子。”这个例子中使用{ def close(): Unit }作为参数类型。因此任何含有close()的函数的类都可以作为参数。这样的做法比使用接口要好很多，因为可以不引入任何依赖。这个withClose方法单独编译，随处使用。
def withClose(closeAble: { def close(): Unit }, op: { def close(): Unit } => Unit) {

    try {
        op(closeAble)

    } finally {
        closeAble.close()

    }
}

 
class Connection {

    def close() = println("close Connection")
}

 
val conn: Connection = new Connection()

withClose(conn, conn =>
    println("do something with Connection"))

### Trait

Traits就像是有函数体的Interface，使用with关键字来混入。单个Traits就像是一块乐高积木，一个插件。就像下面的JsonAble，当使用一个对象的时候，可以随时随地把它插在他上面。这个对接就具备了toJson的能力。不用创建一个类，或者写组合的代码，非常干脆。这样也可以使代码有很高的正交性。不再会为了一个很小的需求，去修改一个被广泛使用的类。
trait ForEachAble[A] {

  def iterator: java.util.Iterator[A]
  def foreach(f: A => Unit) = {

    val iter = iterator
    while (iter.hasNext)

      f(iter.next)
  }

}
 

trait JsonAble {
  def toJson() =

    scala.util.parsing.json.JSONFormat.defaultFormatter(this)
}

 
val list = new java.util.ArrayList[Int]() with ForEachAble[Int]

list.add(1); list.add(2)
 

println("For each: "); list.foreach(x => println(x))
//println("Json: " + list.toJson())

### 函数式真正的威力

通过将函数作为参数，可以使程序极为简洁。 函数式除了能简化代码外，更重要的是他关注的是Input和Output，函数本身没有副作用。 就是Unix pipeline一样，简单的命令可以组合在一起。 List的filter方法接受一个过滤函数，返回一个新的List 如果你喜欢Unix pipeline的方式，你一定也会喜欢函数式编程。 这个例子是用函数式的代码模拟“cat file | grep 'warn' | grep '2013' | wc”的行为。相比于Ruby等动态语言,这威力来自于科学而不是魔法
val file = List("warn 2013 msg", "warn 2012 msg", "error 2013 msg", "warn 2013 msg")

 
println("cat file | grep 'warn' | grep '2013' | wc : "

    + file.filter(_.contains("warn")).filter(_.contains("2013")).size)

### 再见 NullException

每个Java程序员都被NullException折磨过。因为Java中每个对象都可能为Null,所以要么到处检查null的问题，要么到处try/cache。
Scala提供了Option机制来解决，代码中不断检查null的问题。这个例子包装了getProperty方法，使其返回一个Option。 这样就可以不再漫无目的地null检查。只要Option类型的值即可。使用pattern match来检查是常见做法。也可以使用getOrElse来提供当为None时的默认值。给力的是Option还可以看作是最大长度为1的List，List的强大功能都可以使用。
不是每个对象都可以为Null了，只有Option可以为None。这样的做法显示区分了可能为Null的情况，可以和NullException说再见了。
def getProperty(name: String): Option[String] = {

  val value = System.getProperty(name)
  if (value != null) Some(value) else None

}
 

val osName = getProperty("os.name")
 

osName match {
  case Some(value) => println(value)

  case _ => println("none")
}

 
println(osName.getOrElse("none"))

 
osName.foreach(print _)

 

### 并行集合

这个例子是访问若干URL。但确可以并行访问，比非并行的做法可以快一倍。要想让访问并行，只要调用List.par就可以了。
val urls = List("http://scala-lang.org",

  "https://github.com/yankay/scala-tour")
 

def fromURL(url: String) = scala.io.Source.fromURL(url)
  .getLines().mkString("\n")

 
val t = System.currentTimeMillis()

urls.par.map(fromURL(_))
println("time: " + (System.currentTimeMillis - t) + "ms")

是不是非常的简单？并行集合支持大部分集合的功能。不增加程序复杂性，却能大幅提高并发的能力。

### 远程Actor

Actor是并发模型，也使用于分布式。这个例子创建一个时间服务器，通过alive来监听TCP端口，register来注册自己。调用时通过select创建client。其余使用方式和普通Actor一样。
将单机并发和分布式抽象成一种模型。简化了程序复杂性。虽然多核编程并不广泛，但调用外部接口的情况越来越多。Actor模型非常适用于这样的异步环境。
import scala.actors.remote.RemoteActor._

import scala.actors.Actor._
import scala.actors.remote.Node

 
val port = 31241

 
val echoServer = actor {

  alive(port)
  register('echoServer, self)

  loop {
    react {

      case msg => {
        reply("replay " + msg)

      }
    }

  }
}

 
val timeServerClient = select(Node("127.0.0.1", port), 'echoServer)

 
timeServerClient !? "hi" match {

  case replay: String => println(replay)
}

### 抽取器

抽取器可以进行解构。这个例子是构建一个Email抽取器，只要实现unapply函数就可以了。
Scala的正则表达式会自带抽取器，可以抽取出一个List。List里的元素是匹配()里的表达式。
抽取器很有用，短短的例子里就有两处使用抽取器：

* 通过 case user :: do main :: Nil 来解构List
* 通过 case Email(user, domain) 来解构Email。
import scala.util.matching.Regex

 
object Email {

  def unapply(str: String) = new Regex("""(.*)@(.*)""")
    .unapplySeq(str).get match {

    case user :: domain :: Nil => Some(user, domain)
    case _ => None

  }
}

 
"user@domain.com" match {

  case Email(user, domain) => println(user + "@" + domain)
}

### DSL

DSL是Scala最强大武器，可以使一些描述性代码变得极为简单。这个例子是使用DSL生成JSON。Scala很多看似是语言级的特性也是用DSL做到的。
自己编写DSL有点复杂，但使用起来非常方便。这样可以使Scala可以嵌入XML，嵌入Json，嵌入SQL。而其他语言中这些都只是字符串而已。
import org.json4s._

import org.json4s.JsonDSL._
import org.json4s.jackson.JsonMethods._

import java.util.Date
 

case class Twitter(id: Long, text: String, publishedAt: Option[java.util.Date])
 

var twitters = Twitter(1, "hello scala", Some(new Date())) ::
  Twitter(2, "I like scala tour", None) :: Nil

 
var json = ("twitters"

  -> twitters.map(
    t => ("id" -> t.id)

      ~ ("text" -> t.text)
      ~ ("published_at" -> t.publishedAt.toString())))

 
println(pretty(render(json)))

### Simple Build Tool

SBT是Scala的最佳编译工具，在他的帮助下，你甚至不需要安装除JRE外的任何东西，来开发Scala。
例如你想在自己的机器上执行[Scala-Tour](http://zh.scala-tour.com/ "scala-tour"),可以执行下面的命令
#Linux/Mac(compile & run):

git clone https://github.com/yankay/scala-tour-zh.git
cd scala-tour-zh

./sbt/sbt stage
./target/start

 
#Windows(can only compile):

git clone https://github.com/yankay/scala-tour-zh.git
cd scala-tour-zh

sbt\sbt stage

### 结语

这几个例子精选自[Scala-Tour](http://zh.scala-tour.com/ "scala-tour")，这个网站中还有介绍了很多其他好的特性，比如模式匹配和隐式转换，就不逐一介绍了。这个项目Host在[GitHub](https://github.com/yankay/scala-tour/ "GitHub")上，如果你也有精彩的用法的话，大家交流交流吧。
Share the post "Scala Tour - 精选"

* [Weibo](http://service.weibo.com/share/share.php?url=http%3A%2F%2Fwww.yankay.com%2Fscala-tour-choiceness%2F "Share this article on Weibo")
