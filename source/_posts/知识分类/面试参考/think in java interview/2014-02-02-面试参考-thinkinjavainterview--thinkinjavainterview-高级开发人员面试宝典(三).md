---
layout: post
title: "think in java interview-高级开发人员面试宝典(三)"
categories: 面试参考
tags: 
 - 面试参考
 - think in java interview
--- 

# think in java interview-高级开发人员面试宝典(三)

### [[置顶] think in java interview-高级开发人员面试宝典(三)]()

分类： [面经](http://blog.csdn.net/lifetragedy/article/category/1544093) 2013-08-05 13:13 2328人阅读 [评论]()(6) [收藏]( "收藏") [举报]( "举报")
# 收集自Oracle公司的10次（60道）电话面试全部问答（英语）

Q: What environment variables do I need to set on my machine in order to be able to run Java programs?
A: CLASSPATH and PATH are the two variables.
Q: Can an application have multiple classes having main method?
A: Yes it is possible. While starting the application we mention the class name to be run. The JVM will look for the Main method only in the class whose name you have mentioned. Hence there is not conflict amongst the multiple classes having main method.
Q: Can I have multiple main methods in the same class?
A: No the program fails to compile. The compiler says that the main method is already defined in the class.
Q: Do I need to import java.lang package any time? Why ?
A: No. It is by default loaded internally by the JVM.
Q Can I import same package/class twice? Will the JVM load the package twice at runtime?
A: One can import the same package or same class multiple times. Neither compiler nor JVM complains abt it. And the JVM will internally load the class only once no matter how many times you import the same class.
Q: What are Checked and UnChecked Exception?
A: A checked exception is some subclass of Exception (or Exception itself), excluding class RuntimeException and its subclasses.
Making an exception checked forces client programmers to deal with the possibility that the exception will be thrown. eg, IOException thrown by java.io.FileInputStream's read() method?
Unchecked exceptions are RuntimeException and any of its subclasses. Class Error and its subclasses also are unchecked. With an unchecked exception, however, the compiler doesn't force client programmers either to catch the
exception or declare it in a throws clause. In fact, client programmers may not even know that the exception could be thrown. eg, StringIndexOutOfBoundsException thrown by String's charAt() method? Checked exceptions must be caught at compile time. Runtime exceptions do not need to be. Errors often cannot be.
Q: What is Overriding?
A: When a class defines a method using the same name, return type, and arguments as a method in its superclass, the method in the class overrides the method in the superclass.
When the method is invoked for an object of the class, it is the new definition of the method that is called, and not the method definition from superclass. Methods may be overridden to be more public, not more private.
Q: What are different types of inner classes?
A: Nested top-level classes, Member classes, Local classes, Anonymous classes
Nested top-level classes- If you declare a class within a class and specify the static modifier, the compiler treats the class just like any other top-level class.
Any class outside the declaring class accesses the nested class with the declaring class name acting similarly to a package. eg, outer.inner. Top-level inner classes implicitly have access only to static variables.There can also be inner interfaces. All of these are of the nested top-level variety.
Member classes - Member inner classes are just like other member methods and member variables and access to the member class is restricted, just like methods and variables. This means a public member class acts similarly to a nested top-level class. The primary difference between member classes and nested top-level classes is that member classes have access to the specific instance of the enclosing class.
Local classes - Local classes are like local variables, specific to a block of code. Their visibility is only within the block of their declaration. In order for the class to be useful beyond the declaration block, it would need to implement a more publicly available interface.Because local classes are not members, the modifiers public, protected, private, and static are not usable.
Anonymous classes - Anonymous inner classes extend local inner classes one level further. As anonymous classes have no name, you cannot provide a constructor.
Q: Are the imports checked for validity at compile time? e.g. will the code containing an import such as java.lang.ABCD compile?
A: Yes the imports are checked for the semantic validity at compile time. The code containing above line of import will not compile. It will throw an error saying,can not resolve symbol
symbol : class ABCD
location: package io
import java.io.ABCD;
Q: Does importing a package imports the subpackages as well? e.g. Does importing com.MyTest.* also import com.MyTest.UnitTests.*?
A: No you will have to import the subpackages explicitly. Importing com.MyTest.* will import classes in the package MyTest only. It will not import any class in any of it's subpackage.
Q: What is the difference between declaring a variable and defining a variable?
A: In declaration we just mention the type of the variable and it's name. We do not initialize it. But defining means declaration + initialization.
e.g String s; is just a declaration while String s = new String ("abcd"); Or String s = "abcd"; are both definitions.
Q: What is the default value of an object reference declared as an instance variable?
A: null unless we define it explicitly.
Q: Can a top level class be private or protected?
A: No. A top level class can not be private or protected. It can have either "public" or no modifier. If it does not have a modifier it is supposed to have a default access.If a top level class is declared as private the compiler will complain that the "modifier private is not allowed here". This means that a top level class can not be private. Same is the case with protected.
Q: What type of parameter passing does Java support?
A: In Java the arguments are always passed by value .
Q: Primitive data types are passed by reference or pass by value?
A: Primitive data types are passed by value.
Q: Objects are passed by value or by reference?
A: Java only supports pass by value. With objects, the object reference itself is passed by value and so both the original reference and parameter copy both refer to the same object .
Q: What is serialization?
A: Serialization is a mechanism by which you can save the state of an object by converting it to a byte stream.
Q: How do I serialize an object to a file?
A: The class whose instances are to be serialized should implement an interface Serializable. Then you pass the instance to the ObjectOutputStream which is connected to a fileoutputstream. This will save the object to a file.
Q: Which methods of Serializable interface should I implement?
A: The serializable interface is an empty interface, it does not contain any methods. So we do not implement any methods.
Q: How can I customize the seralization process? i.e. how can one have a control over the serialization process?
A: Yes it is possible to have control over serialization process. The class should implement Externalizable interface. This interface contains two methods namely readExternal and writeExternal. You should implement these methods and write the logic for customizing the serialization process.
Q: What is the common usage of serialization?
A: Whenever an object is to be sent over the network, objects need to be serialized. Moreover if the state of an object is to be saved, objects need to be serilazed.
Q: What is Externalizable interface?
A: Externalizable is an interface which contains two methods readExternal and writeExternal. These methods give you a control over the serialization mechanism. Thus if your class implements this interface, you can customize the serialization process by implementing these methods.
Q: When you serialize an object, what happens to the object references included in the object?
A: The serialization mechanism generates an object graph for serialization. Thus it determines whether the included object references are serializable or not. This is a recursive process. Thus when an object is serialized, all the included objects are also serialized alongwith the original obect.
Q: What one should take care of while serializing the object?
A: One should make sure that all the included objects are also serializable. If any of the objects is not serializable then it throws a NotSerializableException.
Q: What happens to the static fields of a class during serialization?
A: There are three exceptions in which serialization doesnot necessarily read and write to the stream. These are
1. Serialization ignores static fields, because they are not part of ay particular state state.
2. Base class fields are only hendled if the base class itself is serializable.
3. Transient fields.
Q: Does Java provide any construct to find out the size of an object?
A: No there is not sizeof operator in Java. So there is not direct way to determine the size of an object directly in Java.
Q: Give a simplest way to find out the time a method takes for execution without using any profiling tool?
A: Read the system time just before the method is invoked and immediately after method returns. Take the time difference, which will give you the time taken by a method for execution.
To put it in code...
long start = System.currentTimeMillis ();
method ();
long end = System.currentTimeMillis ();
System.out.println ("Time taken for execution is " + (end - start));
Remember that if the time taken for execution is too small, it might show that it is taking zero milliseconds for execution. Try it on a method which is big enough, in the sense the one which is doing considerable amout of processing.
Q: What are wrapper classes?
A: Java provides specialized classes corresponding to each of the primitive data types. These are called wrapper classes. They are e.g. Integer, Character, Double etc.
Q: Why do we need wrapper classes?
A: It is sometimes easier to deal with primitives as objects. Moreover most of the collection classes store objects and not primitive data types. And also the wrapper classes provide many utility methods also. Because of these resons we need wrapper classes. And since we create instances of these classes we can store them in any of the collection classes and pass them around as a collection. Also we can pass them around as method parameters where a method expects an object.
Q: What are checked exceptions?
A: Checked exception are those which the Java compiler forces you to catch. e.g. IOException are checked Exceptions.
Q: What are runtime exceptions?
A: Runtime exceptions are those exceptions that are thrown at runtime because of either wrong input data or because of wrong business logic etc. These are not checked by the compiler at compile time.
Q: What is the difference between error and an exception?
A: An error is an irrecoverable condition occurring at runtime. Such as OutOfMemory error. These JVM errors and you can not repair them at runtime. While exceptions are conditions that occur because of bad input etc. e.g. FileNotFoundException will be thrown if the specified file does not exist. Or a NullPointerException will take place if you try using a null reference. In most of the cases it is possible to recover from an exception (probably by giving user a feedback for entering proper values etc.).
Q: How to create custom exceptions?
A: Your class should extend class Exception, or some more specific type thereof.
Q: If I want an object of my class to be thrown as an exception object, what should I do?
A: The class should extend from Exception class. Or you can extend your class from some more precise exception type also.
Q: If my class already extends from some other class what should I do if I want an instance of my class to be thrown as an exception object?
A: One can not do anytihng in this scenarion. Because Java does not allow multiple inheritance and does not provide any exception interface as well.
Q: How does an exception permeate through the code?
A: An unhandled exception moves up the method stack in search of a matching When an exception is thrown from a code which is wrapped in a try block followed by one or more catch blocks, a search is made for matching catch block. If a matching type is found then that block will be invoked. If a matching type is not found then the exception moves up the method stack and reaches the caller method. Same procedure is repeated if the caller method is included in a try catch block. This process continues until a catch block handling the appropriate type of exception is found. If it does not find such a block then finally the program terminates.
Q: What are the different ways to handle exceptions?
A: There are two ways to handle exceptions,
1. By wrapping the desired code in a try block followed by a catch block to catch the exceptions. and
2. List the desired exceptions in the throws clause of the method and let the caller of the method hadle those exceptions.
Q: What is the basic difference between the 2 approaches to exception handling.
1> try catch block and
2> specifying the candidate exceptions in the throws clause?
When should you use which approach?
A: In the first approach as a programmer of the method, you urself are dealing with the exception. This is fine if you are in a best position to decide should be done in case of an exception. Whereas if it is not the responsibility of the method to deal with it's own exceptions, then do not use this approach. In this case use the second approach. In the second approach we are forcing the caller of the method to catch the exceptions, that the method is likely to throw. This is often the approach library creators use. They list the exception in the throws clause and we must catch them. You will find the same approach throughout the java libraries we use.
Q: Is it necessary that each try block must be followed by a catch block?
A: It is not necessary that each try block must be followed by a catch block. It should be followed by either a catch block OR a finally block. And whatever exceptions are likely to be thrown should be declared in the throws clause of the method.
Q: If I write return at the end of the try block, will the finally block still execute?
A: Yes even if you write return as the last statement in the try block and no exception occurs, the finally block will execute. The finally block will execute and then the control return.
Q: If I write System.exit (0); at the end of the try block, will the finally block still execute?
A: No in this case the finally block will not execute because when you say System.exit (0); the control immediately goes out of the program, and thus finally never executes.
Q: How are Observer and Observable used?
A: Objects that subclass the Observable class maintain a list of observers. When an Observable object is updated it invokes the update() method of each of its observers to notify the observers that it has changed state. The Observer interface is implemented by objects that observe Observable objects.
Q: What is synchronization and why is it important?
A: With respect to multithreading, synchronization is the capability to control
the access of multiple threads to shared resources. Without synchronization, it is possible for one thread to modify a shared object while another thread is in the process of using or updating that object's value. This often leads to significant errors.
Q: How does Java handle integer overflows and underflows?
A: It uses those low order bytes of the result that can fit into the size of the type allowed by the operation.
Q: Does garbage collection guarantee that a program will not run out of memory?
A: Garbage collection does not guarantee that a program will not run out of memory. It is possible for programs to use up memory resources faster than they are garbage collected. It is also possible for programs to create objects that are not subject to garbage collection
Q: What is the difference between preemptive scheduling and time slicing?
A: Under preemptive scheduling, the highest priority task executes until it enters the waiting or dead states or a higher priority task comes into existence. Under time slicing, a task executes for a predefined slice of time and then reenters the pool of ready tasks. The scheduler then determines which task should execute next, based on priority and other factors.
Q: When a thread is created and started, what is its initial state?
A: A thread is in the ready state after it has been created and started.
Q: What is the purpose of finalization?
A: The purpose of finalization is to give an unreachable object the opportunity to perform any cleanup processing before the object is garbage collected.
Q: What is the Locale class?
A: The Locale class is used to tailor program output to the conventions of a particular geographic, political, or cultural region.
Q: What is the difference between a while statement and a do statement?
A: A while statement checks at the beginning of a loop to see whether the next loop iteration should occur. A do statement checks at the end of a loop to see whether the next iteration of a loop should occur. The do statement will always execute the body of a loop at least once.
Q: What is the difference between static and non-static variables?
A: A static variable is associated with the class as a whole rather than with specific instances of a class. Non-static variables take on unique values with each object instance.
Q: How are this() and super() used with constructors?
A: This() is used to invoke a constructor of the same class. super() is used to invoke a superclass constructor.
Q: What are synchronized methods and synchronized statements?
A: Synchronized methods are methods that are used to control access to an object. A thread only executes a synchronized method after it has acquired the lock for the method's object or class. Synchronized statements are similar to synchronized methods. A synchronized statement can only be executed after a thread has acquired the lock for the object or class referenced in the synchronized statement.
Q: What is daemon thread and which method is used to create the daemon thread?
A: Daemon thread is a low priority thread which runs intermittently in the back ground doing the garbage collection operation for the java runtime system. setDaemon method is used to create a daemon thread.
Q: Can applets communicate with each other?
A: At this point in time applets may communicate with other applets running in the same virtual machine. If the applets are of the same class, they can communicate via shared static variables. If the applets are of different classes, then each will need a reference to the same class with static variables. In any case the basic idea is to pass the information back and forth through a static variable.
An applet can also get references to all other applets on the same page using the getApplets() method of java.applet.AppletContext. Once you get the reference to an applet, you can communicate with it by using its public members.
It is conceivable to have applets in different virtual machines that talk to a server somewhere on the Internet and store any data that needs to be serialized there. Then, when another applet needs this data, it could connect to this same server. Implementing this is non-trivial.
Q: What are the steps in the JDBC connection?
A: While making a JDBC connection we go through the following steps :
Step 1 : Register the database driver by using :
Class.forName(\" driver classs for that specific database\" );
Step 2 : Now create a database connection using :
Connection con = DriverManager.getConnection(url,username,password);
Step 3: Now Create a query using :
Statement stmt = Connection.Statement(\"select * from TABLE NAME\");
Step 4 : Exceute the query :
stmt.exceuteUpdate();
Q: How does a try statement determine which catch clause should be used to handle an exception?
A: When an exception is thrown within the body of a try statement, the catch clauses of the try statement are examined in the order in which they appear. The first catch clause that is capable of handling the exceptionis executed. The remaining catch clauses are ignored.
Q: Can an unreachable object become reachable again?
A: An unreachable object may become reachable again. This can happen when the object's finalize() method is invoked and the object performs an operation which causes it to become accessible to reachable objects.
Q: What method must be implemented by all threads?
A: All tasks must implement the run() method, whether they are a subclass of Thread or implement the Runnable interface.
Q: What are synchronized methods and synchronized statements?
A: Synchronized methods are methods that are used to control access to an object. A thread only executes a synchronized method after it has acquired the lock for the method's object or class. Synchronized statements are similar to synchronized methods. A synchronized statement can only be executed after a thread has acquired the lock for the object or class referenced in the synchronized statement.
Q: What is Externalizable?
A: Externalizable is an Interface that extends Serializable Interface. And sends data into Streams in Compressed Format. It has two methods, writeExternal(ObjectOuput out) and readExternal(ObjectInput in)
Q: What modifiers are allowed for methods in an Interface?
A: Only public and abstract modifiers are allowed for methods in interfaces.
Q: What are some alternatives to inheritance?
A: Delegation is an alternative to inheritance. Delegation means that you include an instance of another class as an instance variable, and forward messages to the instance. It is often safer than inheritance because it forces you to think about each message you forward, because the instance is of a known class, rather than a new class, and because it doesn't force you to accept all the methods of the super class: you can provide only the methods that really make sense. On the other hand, it makes you write more code, and it is harder to re-use (because it is not a subclass).
Q: What does it mean that a method or field is "static"?
A: Static variables and methods are instantiated only once per class. In other words they are class variables, not instance variables. If you change the value of a static variable in a particular object, the value of that variable changes for all instances of that class.
Static methods can be referenced with the name of the class rather than the name of a particular object of the class (though that works too). That's how library methods like System.out.println() work out is a static field in the
Q: What is the difference between preemptive scheduling and time slicing?
A: Under preemptive scheduling, the highest priority task executes until it enters the waiting or dead states or a higher priority task comes into existence. Under time slicing, a task executes for a predefined slice of time and then reenters the pool of ready tasks. The scheduler then determines which task should execute next, based on priority and other factors.
Q: What is the catch or declare rule for method declarations?
A: If a checked exception may be thrown within the body of a method, the method must either catch the exception or declare it in its throws clause.

分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")
1. 上一篇：[think in java interview-高级开发人员面试宝典(二)](http://blog.csdn.net/lifetragedy/article/details/9751079)
1. 下一篇：[think in java interview-高级开发人员面试宝典(四)](http://blog.csdn.net/lifetragedy/article/details/9812419)

顶3 踩2

查看评论[]()
5楼 [柔龙笑](http://blog.csdn.net/SSH8080) 3天前 17:31发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/SSH8080)哎！英语太难啦！ 4楼 [qq1456221801](http://blog.csdn.net/qq1456221801) 6天前 19:56发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/qq1456221801)谢谢您的奉献 3楼 [Fly_fans](http://blog.csdn.net/Fly_fans) 6天前 14:31发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/Fly_fans)英文水平有点低啊。。。看不太懂 Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 6天前 16:56发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复Fly_fans：搞IT的很多英语全忘了，天天看美剧，背台词，半年可以有小成吧，相信你能提高你的英语水平的，加油。 2楼 [水哥709](http://blog.csdn.net/leon709) 2013-08-10 18:04发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/leon709)我觉得关于main方法的回答需要说清楚一点，一个类中是可以有多个main方法的，重载main是可以的，public static void main(String[] args){} 方法只能有一个，也就是程序入口方法，主线程的栈底方法只能有一个。 1楼 [yuanlong_zheng](http://blog.csdn.net/yuanlong_zheng) 2013-08-05 23:26发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/yuanlong_zheng)非常感谢！最近在复习J2ee基础知识，如您所说，copy & paste 次数多了，能力没提高，心中就慢慢怕了。。。

您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Flifetragedy%2Farticle%2Fdetails%2F9766087)
* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()

[![TOP]()]( "回到顶部")
