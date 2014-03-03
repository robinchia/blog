---
layout: post
title: "java nio网络编程的一点心得"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - NIO
--- 

# java nio网络编程的一点心得

前几日用java nio写了一个[tcp端口转发小工具](http://code.google.com/p/marlon-tools/source/browse/tools/tcpmon/TCPMonitorSelect.java)，还颇费周折，其中一个原因在于网上资料很混乱，不少还是错误的。这篇文章中我会以一个EchoServer作为例子。先看[《Java网络编程》](http://book.douban.com/subject/1438754/)中的写法，这也是在网上颇为常见的一个写法。
Java代码  [![收藏代码](http://marlonyao.iteye.com/images/icon_star.png)![]()]( "收藏这段代码")

1. public class EchoServer {  
1.     public static int DEFAULT_PORT = 7777;  
1.   
1.     public static void main(String[] args) throws IOException {  
1.         System.out.println("Listening for connection on port " + DEFAULT_PORT);  
1.   
1.         Selector selector = Selector.open();  
1.         initServer(selector);  
1.   
1.         while (true) {  
1.             selector.select();  
1.   
1.             for (Iterator<SelectionKey> itor = selector.selectedKeys().iterator(); itor.hasNext();) {  
1.                 SelectionKey key = (SelectionKey) itor.next();  
1.                 itor.remove();  
1.                 try {  
1.                     if (key.isAcceptable()) {  
1.                         ServerSocketChannel server = (ServerSocketChannel) key.channel();  
1.                         SocketChannel client = server.accept();  
1.                         System.out.println("Accepted connection from " + client);  
1.                         client.configureBlocking(false);  
1.                         SelectionKey clientKey = client.register(selector, SelectionKey.OP_WRITE|SelectionKey.OP_READ);  
1.                         ByteBuffer buffer = ByteBuffer.allocate(100);  
1.                         clientKey.attach(buffer);  
1.                     }  
1.                     if (key.isReadable()) {  
1.                         SocketChannel client = (SocketChannel) key.channel();  
1.                         ByteBuffer buffer = (ByteBuffer) key.attachment();  
1.                         client.read(buffer);  
1.                     }  
1.                     if (key.isWritable()) {  
1.                         // System.out.println("is writable...");  
1.                         SocketChannel client = (SocketChannel) key.channel();  
1.                         ByteBuffer buffer = (ByteBuffer) key.attachment();  
1.                         buffer.flip();  
1.                         client.write(buffer);  
1.                         buffer.compact();  
1.                     }  
1.                 } catch (IOException e) {  
1.                     key.cancel();  
1.                     try { key.channel().close(); } catch (IOException ioe) { }  
1.                 }  
1.             }  
1.         }  
1.     }  
1.   
1.     private static void initServer(Selector selector) throws IOException,  
1.             ClosedChannelException {  
1.         ServerSocketChannel serverChannel = ServerSocketChannel.open();  
1.         ServerSocket ss = serverChannel.socket();  
1.         ss.bind(new InetSocketAddress(DEFAULT_PORT));  
1.         serverChannel.configureBlocking(false);  
1.         serverChannel.register(selector, SelectionKey.OP_ACCEPT);  
1.     }  
1. }  

public class EchoServer {

    public static int DEFAULT_PORT = 7777;
 

    public static void main(String[] args) throws IOException {
        System.out.println("Listening for connection on port " + DEFAULT_PORT);

 
        Selector selector = Selector.open();

        initServer(selector);
 

        while (true) {
            selector.select();

 
            for (Iterator<SelectionKey> itor = selector.selectedKeys().iterator(); itor.hasNext();) {

                SelectionKey key = (SelectionKey) itor.next();
                itor.remove();

                try {
                    if (key.isAcceptable()) {

                        ServerSocketChannel server = (ServerSocketChannel) key.channel();
                        SocketChannel client = server.accept();

                        System.out.println("Accepted connection from " + client);
                        client.configureBlocking(false);

                        SelectionKey clientKey = client.register(selector, SelectionKey.OP_WRITE|SelectionKey.OP_READ);
                        ByteBuffer buffer = ByteBuffer.allocate(100);

                        clientKey.attach(buffer);
                    }

                    if (key.isReadable()) {
                        SocketChannel client = (SocketChannel) key.channel();

                        ByteBuffer buffer = (ByteBuffer) key.attachment();
                        client.read(buffer);

                    }
                    if (key.isWritable()) {

                        // System.out.println("is writable...");
                        SocketChannel client = (SocketChannel) key.channel();

                        ByteBuffer buffer = (ByteBuffer) key.attachment();
                        buffer.flip();

                        client.write(buffer);
                        buffer.compact();

                    }
                } catch (IOException e) {

                    key.cancel();
                    try { key.channel().close(); } catch (IOException ioe) { }

                }
            }

        }
    }

 
    private static void initServer(Selector selector) throws IOException,

            ClosedChannelException {
        ServerSocketChannel serverChannel = ServerSocketChannel.open();

        ServerSocket ss = serverChannel.socket();
        ss.bind(new InetSocketAddress(DEFAULT_PORT));

        serverChannel.configureBlocking(false);
        serverChannel.register(selector, SelectionKey.OP_ACCEPT);

    }
}
上面的代码很典型，运行结果似乎也是正确的。
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. marlon$ java EchoServer&  
1. --> Listening for connection on port 7777  
1. marlon$ telnet localhost 7777  
1. --> Accepted connection from java.nio.channels.SocketChannel[connected local=/127.0.0.1:7777 remote=/127.0.0.1:65030]  
1. hello  
1. --> hello  
1. world  
1. -->world  

marlon$ java EchoServer&

--> Listening for connection on port 7777
marlon$ telnet localhost 7777

--> Accepted connection from java.nio.channels.SocketChannel[connected local=/127.0.0.1:7777 remote=/127.0.0.1:65030]
hello

--> hello
world

-->world
但是如果你这时top用看一下发现服务器进程CPU占用到95%以上，如果取消掉32行的注释，服务器会不断地输出"is writable..."，这是为什么呢？让我们来分析当第一个客户端连接上时发生什么情况。

1. 在连接之前，服务器第11行：selector.select()处阻塞。当阻塞时，内核会将这个进程调度至休眠状态，此时基本不耗CPU。
1. 当客户端发起一个连接时，服务器检测到客户端连接，selector.select()返回。selector.selectedKeys()返回已就绪的SelectionKey的集合，在这种情况下，它只包含一个key，也就是53行注册的acceptable key。服务器开始运行17-25行的代码，server.accept()返回代码客户端连接的socket，第22行在socket上注册OP_READ和OP_WRITE，表示当socket可读或者可写时就会通知selector。
1. 接着服务器又回到第11行，尽管这时客户端还没有任何输入，但这时selector.select()不会阻塞，因为22行在socket注册了写操作，而socket只要send buffer不满就可以写，刚开始send buffer为空，socket总是可以写，于是server.select()立即返回，包含在22行注册的key。由于这个key可写，所以服务器会运行31-38行的代码，但是这时buffer为空，client.write(buffer)没有向socket写任何东西，立即返回0。
1. 接着服务器又回到第11行，由于客户端连接socket可以写，这时selector.select()会立即返回，然后运行31-38行的代码，像步骤3一样，由于buffer为空，服务器没有干任何事又返回到第11行，这样不断循环，服务器却实际没有干事情，却耗大量的CPU。
从上面的分析可以看出问题在于我们在没有数据可写时就在socket上注册了OP_WRITE，导致服务器浪费大量CPU资源，解决办法是**只有数据可以写时才注册OP_WRITE操作**。上面的版本还不只浪费CPU那么简单，它还可能导致潜在的死锁。虽然死锁在我的机器上没有发生，对于这个简单的例子似乎也不大可能发生在别的机器上，但是在对于复杂的情况，比如我写的端口转发工具中就发生了，这还依赖于jdk的实现。对于上面的EchoServer，出现死锁的场景是这样的：

1. 假设服务器已经启动，并且已经有一个客户端与它相连，此时正如上面的分析，服务器在不断地循环做无用功。这时用户在客户端输入"hello"。
1. 当服务器运行到第11行：selector.select()时，这时selector.selectedKeys()会返回一个代表客户端连接的key，显然这时客户端socket是既可读又可写，但jdk却并不保证能够检测到两种状态。如果它检测到key既可读又可写，那么服务器会执行26-38行的代码。如果只检测到可读，那么服务器会执行26-30行的代码。如果只检测到可写，那么会执行31－38行的代码。对于前两种情况，不会造成死锁，因为当执行完29行，buffer会读到用户输入的内容，下次再运行到36行就可以将用户输入内容echo回。但是对最后一种情况，服务器完全忽略了客户端发过来的内容，如果每次selector.select()都只能检测到socket可写，那么服务器永远不能将echo回客户端输入的内容。
避免死锁的一个简单方法就是**不要在同一个socket同时注册多个操作**。对于上面的EchoServer来说就是不要同时注册OP_READ和OP_WRITE，要么只注册OP_READ，要么只注册OP_WRITE。下面的EchoServer修正了以上的错误：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public static void main(String[] args) throws IOException {  
1.     System.out.println("Listening for connection on port " + DEFAULT_PORT);  
1.   
1.     Selector selector = Selector.open();  
1.     initServer(selector);  
1.   
1.     while (true) {  
1.         selector.select();  
1.   
1.         for (Iterator<SelectionKey> itor = selector.selectedKeys().iterator(); itor.hasNext();) {  
1.             SelectionKey key = (SelectionKey) itor.next();  
1.             itor.remove();  
1.             try {  
1.                 if (key.isAcceptable()) {  
1.                     ServerSocketChannel server = (ServerSocketChannel) key.channel();  
1.                     SocketChannel client = server.accept();  
1.                     System.out.println("Accepted connection from " + client);  
1.                     client.configureBlocking(false);  
1.                     SelectionKey clientKey = client.register(selector, SelectionKey.OP_READ);  
1.                     ByteBuffer buffer = ByteBuffer.allocate(100);  
1.                     clientKey.attach(buffer);  
1.                 } else if (key.isReadable()) {  
1.                     SocketChannel client = (SocketChannel) key.channel();  
1.                     ByteBuffer buffer = (ByteBuffer) key.attachment();  
1.                     int n = client.read(buffer);  
1.                     if (n > 0) {  
1.                         buffer.flip();  
1.                         key.interestOps(SelectionKey.OP_WRITE);     // switch to OP_WRITE  
1.                     }  
1.                 } else if (key.isWritable()) {  
1.                     System.out.println("is writable...");  
1.                     SocketChannel client = (SocketChannel) key.channel();  
1.                     ByteBuffer buffer = (ByteBuffer) key.attachment();  
1.                     client.write(buffer);  
1.                     if (buffer.remaining() == 0) {  // write finished, switch to OP_READ  
1.                         buffer.clear();  
1.                         key.interestOps(SelectionKey.OP_READ);  
1.                     }  
1.                 }  
1.             } catch (IOException e) {  
1.                 key.cancel();  
1.                 try { key.channel().close(); } catch (IOException ioe) { }  
1.             }  
1.         }  
1.     }  
1. }  

    public static void main(String[] args) throws IOException {

        System.out.println("Listening for connection on port " + DEFAULT_PORT);
 

        Selector selector = Selector.open();
        initServer(selector);

 
        while (true) {

            selector.select();
 

            for (Iterator<SelectionKey> itor = selector.selectedKeys().iterator(); itor.hasNext();) {
                SelectionKey key = (SelectionKey) itor.next();

                itor.remove();
                try {

                    if (key.isAcceptable()) {
                        ServerSocketChannel server = (ServerSocketChannel) key.channel();

                        SocketChannel client = server.accept();
                        System.out.println("Accepted connection from " + client);

                        client.configureBlocking(false);
                        SelectionKey clientKey = client.register(selector, SelectionKey.OP_READ);

                        ByteBuffer buffer = ByteBuffer.allocate(100);
                        clientKey.attach(buffer);

                    } else if (key.isReadable()) {
                        SocketChannel client = (SocketChannel) key.channel();

                        ByteBuffer buffer = (ByteBuffer) key.attachment();
                        int n = client.read(buffer);

                        if (n > 0) {
                            buffer.flip();

                            key.interestOps(SelectionKey.OP_WRITE);        // switch to OP_WRITE
                        }

                    } else if (key.isWritable()) {
                        System.out.println("is writable...");

                        SocketChannel client = (SocketChannel) key.channel();
                        ByteBuffer buffer = (ByteBuffer) key.attachment();

                        client.write(buffer);
                        if (buffer.remaining() == 0) {    // write finished, switch to OP_READ

                            buffer.clear();
                            key.interestOps(SelectionKey.OP_READ);

                        }
                    }

                } catch (IOException e) {
                    key.cancel();

                    try { key.channel().close(); } catch (IOException ioe) { }
                }

            }
        }

    }
主要变化，在第19行接受客户端连接时只注册OP_READ操作，第28行当读到数据时才切换到OP_WRITE操作，第35-38行，当写操作完成时再切换到OP_READ操作。由于一个key同时只能执行一个操作，我将原来三个并行if换成了if...else。
上面的代码不够优雅，它将处理服务器Socket和客户连接Socket的代码搅在一起，对于简单的EchoServer这样做没什么问题，当服务器变得复杂，使用命令模式将它们分开变显得非常必要。首先创建一个接口来抽象对SelectionKey的处理。
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. interface Handler {  
1.     void execute(Selector selector, SelectionKey key);  
1. }  

    interface Handler {

        void execute(Selector selector, SelectionKey key);
    }
再来看main函数：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public static void main(String[] args) throws IOException {  
1.     System.out.println("Listening for connection on port " + DEFAULT_PORT);  
1.   
1.     Selector selector = Selector.open();  
1.     initServer(selector);  
1.   
1.     while (true) {  
1.         selector.select();  
1.   
1.         for (Iterator<SelectionKey> itor = selector.selectedKeys().iterator(); itor.hasNext();) {  
1.             SelectionKey key = (SelectionKey) itor.next();  
1.             itor.remove();  
1.             Handler handler = (Handler) key.attachment();  
1.             handler.execute(selector, key);  
1.         }  
1.     }  
1. }  
1.   
1. private static void initServer(Selector selector) throws IOException,  
1.         ClosedChannelException {  
1.     ServerSocketChannel serverChannel = ServerSocketChannel.open();  
1.     ServerSocket ss = serverChannel.socket();  
1.     ss.bind(new InetSocketAddress(DEFAULT_PORT));  
1.     serverChannel.configureBlocking(false);  
1.     SelectionKey serverKey = serverChannel.register(selector, SelectionKey.OP_ACCEPT);  
1.     serverKey.attach(new ServerHandler());  
1. }  

    public static void main(String[] args) throws IOException {

        System.out.println("Listening for connection on port " + DEFAULT_PORT);
 

        Selector selector = Selector.open();
        initServer(selector);

 
        while (true) {

            selector.select();
 

            for (Iterator<SelectionKey> itor = selector.selectedKeys().iterator(); itor.hasNext();) {
                SelectionKey key = (SelectionKey) itor.next();

                itor.remove();
                Handler handler = (Handler) key.attachment();

                handler.execute(selector, key);
            }

        }
    }

 
    private static void initServer(Selector selector) throws IOException,

            ClosedChannelException {
        ServerSocketChannel serverChannel = ServerSocketChannel.open();

        ServerSocket ss = serverChannel.socket();
        ss.bind(new InetSocketAddress(DEFAULT_PORT));

        serverChannel.configureBlocking(false);
        SelectionKey serverKey = serverChannel.register(selector, SelectionKey.OP_ACCEPT);

        serverKey.attach(new ServerHandler());
    }
main函数非常简单，迭代SelectionKey，对每个key的attachment为Handler，调用它的execute的方法，不用管它是服务器Socket还是客户Socket。注意initServer方法将serverKey附加了一个ServerHandler。下面是ServerHandler的代码：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. class ServerHandler implements Handler {  
1.     public void execute(Selector selector, SelectionKey key) {  
1.         ServerSocketChannel server = (ServerSocketChannel) key.channel();  
1.         SocketChannel client = null;  
1.         try {  
1.             client = server.accept();  
1.             System.out.println("Accepted connection from " + client);  
1.         } catch (IOException e) {  
1.             e.printStackTrace();  
1.             return;  
1.         }  
1.           
1.         SelectionKey clientKey = null;  
1.         try {  
1.             client.configureBlocking(false);  
1.             clientKey = client.register(selector, SelectionKey.OP_READ);  
1.             clientKey.attach(new ClientHandler());  
1.         } catch (IOException e) {  
1.             if (clientKey != null)  
1.                 clientKey.cancel();  
1.             try { client.close(); } catch (IOException ioe) { }  
1.         }  
1.     }  
1. }  

    class ServerHandler implements Handler {

        public void execute(Selector selector, SelectionKey key) {
            ServerSocketChannel server = (ServerSocketChannel) key.channel();

            SocketChannel client = null;
            try {

                client = server.accept();
                System.out.println("Accepted connection from " + client);

            } catch (IOException e) {
                e.printStackTrace();

                return;
            }

 
            SelectionKey clientKey = null;

            try {
                client.configureBlocking(false);

                clientKey = client.register(selector, SelectionKey.OP_READ);
                clientKey.attach(new ClientHandler());

            } catch (IOException e) {
                if (clientKey != null)

                    clientKey.cancel();
                try { client.close(); } catch (IOException ioe) { }

            }
        }

    }
ServerHandler接收连接，为每个客户Socket注册OP_READ操作，返回的clientKey附加上ClientHandler。
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. class ClientHandler implements Handler {  
1.     private ByteBuffer buffer;  
1.       
1.     public ClientHandler() {  
1.         buffer = ByteBuffer.allocate(100);  
1.     }  
1.       
1.     public void execute(Selector selector, SelectionKey key) {  
1.         try {  
1.             if (key.isReadable()) {  
1.                 readKey(selector, key);  
1.             } else if (key.isWritable()) {  
1.                 writeKey(selector, key);  
1.             }  
1.         } catch (IOException e) {  
1.             key.cancel();  
1.             try { key.channel().close(); } catch (IOException ioe) { }  
1.         }  
1.     }  
1.       
1.     private void readKey(Selector selector, SelectionKey key) throws IOException {  
1.         SocketChannel client = (SocketChannel) key.channel();  
1.         int n = client.read(buffer);  
1.         if (n > 0) {  
1.             buffer.flip();  
1.             key.interestOps(SelectionKey.OP_WRITE);     // switch to OP_WRITE  
1.         }  
1.     }  
1.       
1.     private void writeKey(Selector selector, SelectionKey key) throws IOException {  
1.         // System.out.println("is writable...");  
1.         SocketChannel client = (SocketChannel) key.channel();  
1.         client.write(buffer);  
1.         if (buffer.remaining() == 0) {  // write finished, switch to OP_READ  
1.             buffer.clear();  
1.             key.interestOps(SelectionKey.OP_READ);  
1.         }  
1.     }  
1. }  

    class ClientHandler implements Handler {

        private ByteBuffer buffer;
 

        public ClientHandler() {
            buffer = ByteBuffer.allocate(100);

        }
 

        public void execute(Selector selector, SelectionKey key) {
            try {

                if (key.isReadable()) {
                    readKey(selector, key);

                } else if (key.isWritable()) {
                    writeKey(selector, key);

                }
            } catch (IOException e) {

                key.cancel();
                try { key.channel().close(); } catch (IOException ioe) { }

            }
        }

 
        private void readKey(Selector selector, SelectionKey key) throws IOException {

            SocketChannel client = (SocketChannel) key.channel();
            int n = client.read(buffer);

            if (n > 0) {
                buffer.flip();

                key.interestOps(SelectionKey.OP_WRITE);        // switch to OP_WRITE
            }

        }
 

        private void writeKey(Selector selector, SelectionKey key) throws IOException {
            // System.out.println("is writable...");

            SocketChannel client = (SocketChannel) key.channel();
            client.write(buffer);

            if (buffer.remaining() == 0) {    // write finished, switch to OP_READ
                buffer.clear();

                key.interestOps(SelectionKey.OP_READ);
            }

        }
    }
这个代码没有什么新内容，只是将根据key是可读还可写拆分为两个方法，代码结构显得更清晰。对于EchoServer，这么做确实有些过度工程，对于稍微复杂一点的服务器这么做是很值得的。
代码：[EchoServer.java](http://pastebin.com/de64ZzUy), [EchoServer2.java](http://pastebin.com/fFy0Uhbm), [EchoServer3.java](http://pastebin.com/DRMT4LdJ)
参考：

1. [The Rox Java NIO Tutorial](http://rox-xmlrpc.sourceforge.net/niotut/)
1. [Architecture of a Highly Scalable NIO-Based Server](http://today.java.net/pub/a/today/2007/02/13/architecture-of-highly-scalable-nio-server.html)
