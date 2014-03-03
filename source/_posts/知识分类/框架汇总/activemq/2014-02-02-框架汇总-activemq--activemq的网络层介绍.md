---
layout: post
title: "activemq的网络层介绍"
categories: 框架汇总
tags: 
 - 框架汇总
 - activemq
--- 

# activemq的网络层介绍

#

### 概要

activemq是一个apache的顶级项目，其实现了[JMS规范](http://www.goldendoc.org/2011/08/jms_spec_message/ "JMS规范介绍(1) JMS消息")，作为一个开源的JMS实现，activemq已经在很多地方得到了应用。同时，开源小组在研究JMS实现的时候，也选择了activemq作为研究对象，希望能够读其源码，让[开源小组](http://www.goldendoc.org/ "开源学习小组：黄金档")更好的明白JMS规范和实现。

在学习activemq的时候，我们发现还不能一下子就进去学习JMS规范的实现，而是需要了解其底层代码，包括其对网络连接的处理等等。所以，这篇文章就学习了[activemq的网络层实现](http://www.goldendoc.org/2011/09/activemq-network-process/ "activemq的网络层实现")。

### activemq网络层概念

activemq 是支持多协议的，因此，把单一协议的server抽象成更高一层，就很有意义，请看下面这张图：
[![]()](http://www.goldendoc.org/wp-content/uploads/2011/09/activemq%E7%BD%91%E7%BB%9C%E8%BF%9E%E6%8E%A51.png)

在activemq中，差不多每种概念都有生命周期（见Service接口）。
这张图十分简单，实际中当然比这个要复杂。下面针对每个概念进行叙述。

### Transport

Transport 应该算是很底层的概念，他封装了 Socket，职责是发送和接受连接方的信息。
同时，在Transport中，增加了同步和异步的概念。
Transport拥有一个TransportListener。

### TransportListener

顾名思义，TransportListener 就是监听 Transport 事件的发生以及做出一定的反映。
在activemq中，正是利用了 TransportListener 对 Command 进行处理 —— Command 是 activemq 网络传递的形式，每个请求都会被反序列化成一个 Command。

### TransportServer

就是一个server实例，他会处理外部进来的Socket，进而封装成 Transport 。此 Server 拥有一个 TransportAcceptListener 。

### TransportAcceptListener

负责 Accept Transport，将 Transport 封装成 Connection，从而开启 Connection 的生命周期。
这里需要注意的是：TransportServer 将 Socket 封装成 Transport，而 TransportAcceptListener 将 Transport 封装成 Connection。

### Connection

可以理解成业务层面上的连接。 Connection 会给相应的 Transport 设置 TransportListener ，从而获取底层 Socket 的活动情况，进行业务的处理。

### Connector

Connector就是较高层次的抽象，它代表对外的连接器，每个连接器都可以支持不同的协议。
Connector 通过给 TransportServer 设置 TransportAcceptListener ，从而能够控制 Transport 并产生相应的 Connection。

### 总结

总体来说，两个Listener是连接 Conncetor与TransportServer 和 Connection和Transport 的关键概念。
至于为什么要分成 Connector ， TransportServer 这两层，我想原因应该就是开头提到的： activemq需要支持多协议，因此 Connector 抽象起来，可以当作是针对不同协议的统一行为；而 TransportServer 就是针对各协议的具体实现。 对于 Connection,Transport 来说， Connector 是与 Connection 统一层次， Transport 是与 TransportServer 统一层次。
如果读者有时间，还可以查看[开源小组](http://www.goldendoc.org/ "开源小组")当时分析的[activemq网络连接序列图](http://www.goldendoc.org/wp-content/uploads/2011/09/activemq%E7%BD%91%E7%BB%9C%E5%A4%84%E7%90%86%E6%B5%81%E7%A8%8B.png)（是基于TcpTransportServer的，图像有点大，需要耐心看）

![]()
来源： <[http://www.goldendoc.org/2011/09/activemq-network-process/](http://www.goldendoc.org/2011/09/activemq-network-process/)>

本文会把主要篇幅集中在这几个组件的创建时期，运行时期来讲解，最后会总结一下这几个组建使用的线程情况。一下的分析是基于 bio 的（nio 也是在这个结构当中，只是行为是基于 nio 特点的）。在[activemq的网络层介绍（一）](http://www.goldendoc.org/2011/09/activemq-network-process/ "activemq的网络层介绍（一）")也讲过，activemq中很多组件都实现了 Service 接口，而 Service 接口的作用就是赋予实现者有生命周期的意义。

### TransportConnector

TransportConnector 是 Connector 的实现，是基于 Transport 的 Connector（目前activemq的Connector实现，也就是基于Transport的）。其内部依赖了 TransportServer ，这个也容易理解，因为这个 Connector 是基于 Transport 的。因此 TransportServer 也是其所重点依赖的组件。 值得注意的是 TransportServer 的 AcceptListener 也是在 TransportConnector 中指定的，具体见下文。

### TransportConnector 的创建

TransportConnector 的创建比较简单，直接接受传入进来的 TransportServer （关于 TransportServer 的创建，详见下文）。

### TransportConnector 的运行

TransportConnector 是一个 Service，有自己的生命周期。因此，在 activemq 开始的时候，其生命周期，会被启动（具体表现为被调用 start 方法）。

在 TransportConnector 的 start 方法中，可以很明显的看到两个主要的逻辑：为 TransportServer 设置一个匿名的 AcceptListener ，调用 TransportServer 启动。

对于 TransportConnector 为 TransportServer 设置的 AcceptListener，其逻辑主要是当 TransportServer 接收到传输进来的 Transport（由 TransportServer 将 Socket 封装成 Transport）之后，将 Transport 封装成 Connection ，并且启动 Connection 的生命周期。这里也不难理解：在[activemq网络层介绍（一）](http://www.goldendoc.org/2011/09/activemq-network-process/ "activemq的网络层介绍（一）")中也讲过， Connector 是与 Connection 一个层次的，TransportServer 与 Transport 是一个层次的。因此，将 AcceptListener 的匿名实现放在 TransportConnector 中，也就很好理解了。

### Connection

在 TransportServer 的 AcceptListener 收到传输进来的 Transport 之后，就创建了 Connection（实际类型是 TransportConnection），然后为 Connection 开启生命周期。
但一个Socket是从什么时候传进来，进而被封装成 Transport 的，会在下文有描述。现在让我们跟着 TransportConnection ，看看他是如何被创建的，以及在它的生命周期中，会做什么事情。

### TransportConnection 的创建

TransportConnection 是在 AcceptListener 收到 TransportServer 传进来的 Transport 之后创建的。可以想象，TransportConnection 是封装了 Transport 的。同时， Transport 的 TransportListener ，也是在 TransportConnection 的构造方法中进行了创建。此 TransportListener 的逻辑就是服务一个 Command (Command是 activemq 实例之间交互的基本对象，任何请求和响应都被activemq 序列化成一个 Command)，在得到响应的时候，进行分发（响应其实也是一个Command）。

### TransportConnection 的运行

在开始说 TransportConnection 的生命周期之前，可以先说一下 activemq 的 Task 接口，此接口有一个 iterate 遍历方法，activemq 一般对这个接口的用法都是使用一个线程去跑 iterater 方法，直到 iterater 返回 false 。一般的，iterater 方法里面都是在重复的做一段逻辑，只不过是这个逻辑实施的对象不一样而已。TransportConnection 正是实现了此接口。

在 TransportConnection 的 start 方法中，主要做了两件事情：创建一个 taskRunner ，以做好跑 Task 的准备（就是在另起线程跑自己的 Iiterater 方法）；调用 Transport 的 Start 方法，开启 Transport 的生命周期。

在 TransportConnection 自己的 Iiterater 方法中，就是到一个队列中取出 Command，并且使用 Transport 的 oneWay，将 Command 发送出去。

### TransportServer

按理说要接下来讲 Transport，但如果不先把 TransportServer 弄清出的话， Transport 还是有些难以理解的。因为是 TransportServer 将 接受到的 Socket 封装成 Transport。这里说的是 TransportServer 的实现， TcpTransportServer。

### TcpTransportServer 的创建

TransportServer 是由 TransportFactory 创建的。TransportFactory，不同的 TransportFactory 创建出不同的 TransportServer，而 TcpTransportServer 则是由 TcpTransportFactory 创建的。关于 TransportFactory 如何创建相应的 TransportServer，activemq使用了一套基于分析参数和配置文件来实现，说起来很高深，其实很朴实的，有兴趣也可以了解一下，这里不做深入。

### TcpTransportServer 的运行

这里主要使用 TcpTransportServer，TcpTransportServer 生命周期，是通过 TransportConnector 来开启的，这点在上面已经有所讲述。

这里要说一下 TransportServerThreadSupport，此类的作用是支持 TransportServer 能够开启另外一个线程，只要实现了 TransportServer Runnable，在 start 的时候，就能够开启另外一个线程，这也是 TransportServerThreadSupport 命令的含义。同样的机制也出现在 TransportThreadSupport 中。

现在来看一下 TcpTransportServer 的 start ：
TcpTransportServer 会开启两个线程，包括：
1. 实现了Runnable的线程，此线程的工作是不停的 accept 外部的 socket，然后将 socket 扔到一个队列中。简单来说，此线程是一个 acceptor。
2. 另外一个线程的工作是，从队列中取出 socket ，并且将 WireFormat（ WireFormat 是在 activemq 中负责序列化和发序列化的组件） 和socket 封装成一个 Transport ，然后将创建好的 Transport 通知到 AcceptListener 中。

### Transport

这里也主要讲 TcpTransport。

### TcpTransport 的创建

上面已经讲到，TcpTransport 是在 TcpTransportServer 中创建，并扔给了 AcceptListener 。并且，TcpTransport 也支持 TransportThreadSupport 的形式，可以另起一个线程。

### TcpTransport 的运行

TcpTransport 开始执行之后，专注于做两个事情：
1. 初始化 socket，并且把 socket 的流给设置起来。
2. 另起一个线程，利用 wireFormat 将 socket 传过来的字节序列转化成一个 command ，然后将 Command 扔给 transportListener 。transportListener 所做的事情就是进行业务处理，然后将得到的 response （也是一个 command）通过 tcpTransport 的 oneway 传给对方。

这样，这个网络通讯的过程就完成了。下面我们来总结一下 TcpTransportServer 的线程模型。

### TcpTransportServer 的线程模型

如图所示：
[![activemq网络线程模型]()](http://www.goldendoc.org/wp-content/uploads/2011/09/activemq%E6%A8%A1%E5%9E%8B.jpg)

activemq网络线程模型

### 总结

从上图中可以看出，accpetor 和 sockethandler 线程是在整个 transportServer 中的，而 ThreadPool 中的线程不会进行消息的发送，相反，当需要发送消息的时候，总是会创建新的线程去发送。这也是为了 ThreadPool 能将自己的线程利用率提高。因为这是 bio 的网络，如果消息的发送是利用线程池中的线程来发送的，那么很有可能线程池中的线程会在发送的时候阻塞，因为对方的网络不可预知。因此，新建一个新的线程，利用新建的线程发送消息，则是很明智的选择。
来源： <[http://www.goldendoc.org/2011/09/activemq-network-process-2/](http://www.goldendoc.org/2011/09/activemq-network-process-2/)> 

 
