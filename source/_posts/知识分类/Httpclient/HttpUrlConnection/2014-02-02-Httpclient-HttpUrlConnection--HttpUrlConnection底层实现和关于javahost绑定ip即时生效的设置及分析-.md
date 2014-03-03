---
layout: post
title: "HttpUrlConnection底层实现和关于java host绑定ip即时生效的设置及分析 - "
categories: Httpclient
tags: 
 - Httpclient
 - HttpUrlConnection
--- 

# HttpUrlConnection底层实现和关于java host绑定ip即时生效的设置及分析 - 程序员之路 - 博客频道 - CSDN.NET

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

# [程序员之路](http://blog.csdn.net/zhongweijian)

## 求各种编程相关兼职

* [![]()目录视图](http://blog.csdn.net/zhongweijian?viewmode=contents)
* [![]()摘要视图](http://blog.csdn.net/zhongweijian?viewmode=list)
* [![]()订阅](http://blog.csdn.net/zhongweijian/rss/list)
[“移动开发那点事”——主题征文活动](http://blog.csdn.net/blogdevteam/article/details/8055079)    [浓缩六届精华，国内大数据领域最纯粹技术盛会](http://hbtc2012.hadooper.cn/)     [CSDN高校俱乐部专家巡讲讲师招募](http://blog.csdn.net/liushizao/article/details/8012164)
[移动开发者大会最新议题发布，八折抢票！](http://mdcc.csdn.net/)     [新版论坛全民公测！](http://blog.csdn.net/csdnproduct/article/details/8071931)    [2012年10月当选微软MVP的CSDN会员名单揭晓！](http://blog.csdn.net/blogdevteam/article/details/8052186)

### [HttpUrlConnection底层实现和关于java host绑定ip即时生效的设置及分析]()

分类： [java](http://blog.csdn.net/zhongweijian/article/category/640345)  2012-05-31 13:49 202人阅读 [评论](http://blog.csdn.net/zhongweijian/article/details/7619453#comments)(0) [收藏]( "收藏") [举报](http://blog.csdn.net/zhongweijian/article/details/7619453#report "举报")
**[java]** [view plain](http://blog.csdn.net/zhongweijian/article/details/7619453# "view plain")[copy](http://blog.csdn.net/zhongweijian/article/details/7619453# "copy")[print](http://blog.csdn.net/zhongweijian/article/details/7619453# "print")[?](http://blog.csdn.net/zhongweijian/article/details/7619453# "?")

1. 最近有个需求需要对于获取URL页面进行host绑定并且立即生效，在java里面实现可以用代理服务器来实现：因为在测试环境下可能需要通过绑定来访问测试环境的应用  
1. 实现代码如下：  
1.    
1.     public static String getResponseText(String queryUrl,String host,String ip) { //queryUrl，完整的url，host和ip需要绑定的host和ip  
1.        InputStream is = null;  
1.        BufferedReader br = null;  
1.        StringBuffer res = new StringBuffer();  
1.        try {  
1.            HttpURLConnection httpUrlConn = null;  
1.            URL url = new URL(queryUrl);  
1.            if(ip!=null){  
1.                String str[] = ip.split("\\.");  
1.                byte[] b =new byte[str.length];  
1.                for(int i=0,len=str.length;i<len;i++){  
1.                    b[i] = (byte)(Integer.parseInt(str[i],10));  
1.                }  
1.                 Proxy proxy = new Proxy(Proxy.Type.HTTP,  
1.                 new InetSocketAddress(InetAddress.getByAddress(b), 80));  //b是绑定的ip，生成proxy代理对象，因为http底层是socket实现，  
1.                 httpUrlConn = (HttpURLConnection) url  
1.                 .openConnection(proxy);  
1.            }else{  
1.                 httpUrlConn = (HttpURLConnection) url  
1.                         .openConnection();   
1.            }  
1.            httpUrlConn.setRequestMethod("GET");  
1.            httpUrlConn.setDoOutput(true);  
1.            httpUrlConn.setConnectTimeout(2000);  
1.            httpUrlConn.setReadTimeout(2000);  
1.            httpUrlConn.setDefaultUseCaches(false);  
1.            httpUrlConn.setUseCaches(false);  
1.    
1.            is = httpUrlConn.getInputStream();  
1.    
1.    
1. 那么底层对于proxy对象到底是怎么处理，底层的socket实现到底怎么样，带着这个疑惑看了下jdk的rt.jar对于这块的处理  
1. httpUrlConn = (HttpURLConnection) url.openConnection(proxy)  
1.    
1. java.net.URL类里面的openConnection方法：  
1. public URLConnection openConnection(Proxy proxy){  
1.    …  
1.    return handler.openConnection(this, proxy); Handler是sun.net.www.protocol.http.Handler.java类，继承java.net. URLStreamHandler.java类，用来处理http连接请求响应的。  
1. }  
1.    
1. Handler的方法：  
1. protected java.net.URLConnection openConnection(URL u, Proxy p)  
1.         throws IOException {  
1.         return new HttpURLConnection(u, p, this);  
1.     }  
1.    
1. 只是简单的生成sun.net.www.protocl.http.HttpURLConnection对象，并进行初始化  
1. protected HttpURLConnection(URL u, Proxy p, Handler handler) {  
1.         super(u);  
1.         requests = new MessageHeader();  请求头信息生成类  
1.         responses = new MessageHeader(); 响应头信息解析类  
1.         this.handler = handler;   
1.         instProxy = p;  代理服务器对象  
1.         cookieHandler = (CookieHandler)java.security.AccessController.doPrivileged(  
1.             new java.security.PrivilegedAction() {  
1.             public Object run() {  
1.                 return CookieHandler.getDefault();  
1.             }  
1.         });  
1.         cacheHandler = (ResponseCache)java.security.AccessController.doPrivileged(  
1.             new java.security.PrivilegedAction() {  
1.             public Object run() {  
1.                 return ResponseCache.getDefault();  
1.             }  
1.         });  
1.     }  
1.     
1.    
1. 最终在httpUrlConn.getInputStream();才进行socket连接，发送http请求，解析http响应信息。具体过程如下：  
1.    
1. sun.net.www.protocl.http.HttpURLConnection.java的getInputStream方法：  
1.    
1. public synchronized InputStream getInputStream() throws IOException {  
1.      
1.      ...socket连接  
1.      connect();  
1.      ...  
1.      ps = (PrintStream)http.getOutputStream(); 获得输出流，打开连接之后已经生成。  
1.    
1.        if (!streaming()) {  
1.              writeRequests();  输出http请求头信息  
1.        }  
1.      ...  
1.      http.parseHTTP(responses, pi, this);  解析响应信息  
1.                 if(logger.isLoggable(Level.FINEST)) {  
1.                     logger.fine(responses.toString());  
1.                 }  
1.                 inputStream = http.getInputStream();  获得输入流  
1. }  
1.    
1. 其中connect()调用方法链：  
1. plainConnect(){  
1. ...  
1.                 Proxy p = null;  
1.                 if (sel != null) {  
1.                     URI uri = sun.net.www.ParseUtil.toURI(url);  
1.                     Iterator<Proxy> it = sel.select(uri).iterator();  
1.                     while (it.hasNext()) {  
1.                         p = it.next();  
1.                         try {  
1.                             if (!failedOnce) {  
1.                                 http = getNewHttpClient(url, p, connectTimeout);  
1. ...  
1. }  
1.    
1. getNewHttpClient(){  
1. ...  
1.         return HttpClient.New(url, p, connectTimeout, useCache);  
1. ...  
1. }  
1.    
1. 下面跟进去最终建立socket连接的代码：  
1. sun.net.www.http.HttpClient.java的openServer()方法建立socket连接：  
1.    
1.     protected synchronized void openServer() throws IOException {  
1.             ...  
1.             if ((proxy != null) && (proxy.type() == Proxy.Type.HTTP)) {  
1.                 sun.net.www.URLConnection.setProxiedHost(host);  
1.                 if (security != null) {  
1.                     security.checkConnect(host, port);  
1.                 }  
1.                 privilegedOpenServer((InetSocketAddress) proxy.address());最终socket连接的是设置的代理服务器的地址，  
1.             ...  
1. }  
1.    
1.     private synchronized void privilegedOpenServer(final InetSocketAddress server)  
1.          throws IOException  
1.     {  
1.         try {  
1.             java.security.AccessController.doPrivileged(  
1.                 new java.security.PrivilegedExceptionAction() {  
1.                 public Object run() throws IOException {  
1.                     openServer(server.getHostName(), server.getPort());  注意openserver函数  这里的server的getHostName是设置的代理服务器，（ip或者hostname，如果是host绑定设置的代理服务器的ip，那么这里getHostName出来的就是ip地址，可以去查看InetSocketAddress类的getHostName方法）  
1.                     return null;  
1.                 }  
1.             });  
1.         } catch (java.security.PrivilegedActionException pae) {  
1.             throw (IOException) pae.getException();  
1.         }  
1.     }  
1.    
1.    public void openServer(String server, int port) throws IOException {  
1.         serverSocket = doConnect(server, port);  生成的Socket连接对象  
1.         try {  
1.             serverOutput = new PrintStream(  
1.                 new BufferedOutputStream(serverSocket.getOutputStream()),  
1.                                          false, encoding);   生成输出流，  
1.         } catch (UnsupportedEncodingException e) {  
1.             throw new InternalError(encoding+" encoding not found");  
1.         }  
1.         serverSocket.setTcpNoDelay(true);  
1.     }  
1.    
1.    
1. protected Socket doConnect (String server, int port)  
1.     throws IOException, UnknownHostException {  
1.         Socket s;  
1.         if (proxy != null) {  
1.             if (proxy.type() == Proxy.Type.SOCKS) {  
1.                 s = (Socket) AccessController.doPrivileged(  
1.                                new PrivilegedAction() {  
1.                                    public Object run() {  
1.                                        return new Socket(proxy);  
1.                                    }});  
1.             } else  
1.                 s = new Socket(Proxy.NO_PROXY);  
1.         } else  
1.             s = new Socket();  
1.         // Instance specific timeouts do have priority, that means  
1.         // connectTimeout & readTimeout (-1 means not set)  
1.         // Then global default timeouts  
1.         // Then no timeout.  
1.         if (connectTimeout >= 0) {  
1.             s.connect(new InetSocketAddress(server, port), connectTimeout);  
1.         } else {  
1.             if (defaultConnectTimeout > 0) {  
1.                 s.connect(new InetSocketAddress(server, port), defaultConnectTimeout);//连接到代理服务器，看下面Socket类的connect方法代码  
1.             } else {  
1.                 s.connect(new InetSocketAddress(server, port));  
1.             }  
1.         }  
1.         if (readTimeout >= 0)  
1.             s.setSoTimeout(readTimeout);  
1.         else if (defaultSoTimeout > 0) {  
1.             s.setSoTimeout(defaultSoTimeout);  
1.         }  
1.         return s;  
1. }  
1.    
1. 上面的new InetSocketAddress(server, port)这里会涉及到java DNS cache的处理，  
1.    
1.       public InetSocketAddress(String hostname, int port) {  
1.         if (port < 0 || port > 0xFFFF) {  
1.             throw new IllegalArgumentException("port out of range:" + port);  
1.         }  
1.         if (hostname == null) {  
1.             throw new IllegalArgumentException("hostname can't be null");  
1.         }  
1.         try {  
1.             addr = InetAddress.getByName(hostname);  //这里会有java DNS缓存的处理，先从缓存取hostname绑定的ip地址，如果取不到再通过OS的DNS cache机制去取，取不到再从DNS服务器上取。  
1.         } catch(UnknownHostException e) {  
1.             this.hostname = hostname;  
1.             addr = null;  
1.         }  
1.         this.port = port;  
1.     }  
1.    
1.    
1.    
1. 当然最终的Socket.java的connect方法  
1. java.net.socket  
1.               
1.    public void connect(SocketAddress endpoint, int timeout) throws IOException {  
1.         if (endpoint == null)  
1.              
1.         if (timeout < 0)  
1.           throw new IllegalArgumentException("connect: timeout can't be negative");  
1.    
1.         if (isClosed())  
1.             throw new SocketException("Socket is closed");  
1.    
1.         if (!oldImpl && isConnected())  
1.             throw new SocketException("already connected");  
1.    
1.         if (!(endpoint instanceof InetSocketAddress))  
1.             throw new IllegalArgumentException("Unsupported address type");  
1.    
1.         InetSocketAddress epoint = (InetSocketAddress) endpoint;  
1.    
1.         SecurityManager security = System.getSecurityManager();  
1.         if (security != null) {  
1.             if (epoint.isUnresolved())  
1.                 security.checkConnect(epoint.getHostName(),  
1.                                       epoint.getPort());  
1.             else  
1.                 security.checkConnect(epoint.getAddress().getHostAddress(),  
1.                                       epoint.getPort());  
1.         }  
1.         if (!created)  
1.             createImpl(true);  
1.         if (!oldImpl)  
1.             impl.connect(epoint, timeout);  
1.         else if (timeout == 0) {  
1.             if (epoint.isUnresolved())  //如果没有设置SocketAddress的ip地址，则用域名去访问  
1.                 impl.connect(epoint.getAddress().getHostName(),  
1.                              epoint.getPort());  
1.             else  
1.                 impl.connect(epoint.getAddress(), epoint.getPort());  最终socket连接的是设置的SocketAddress的ip地址，  
1.         } else  
1.             throw new UnsupportedOperationException("SocketImpl.connect(addr, timeout)");  
1.         connected = true;  
1.         /* 
1.          * If the socket was not bound before the connect, it is now because 
1.          * the kernel will have picked an ephemeral port & a local address 
1.          */  
1.         bound = true;  
1.     }  
1.    
1.    
1.    
1. 我们再看下通过socket来发送HTTP请求的处理代码，也就是sun.net.www.protocl.http.HttpURLConnection.java的getInputStream方法中调用的writeRequests()方法：   
1. private void writeRequests() throws IOException {  这段代码就是封装http请求的头请求信息，通过socket发送出去  
1.         /* print all message headers in the MessageHeader 
1.          * onto the wire - all the ones we've set and any 
1.          * others that have been set 
1.          */  
1.         // send any pre-emptive authentication  
1.         if (http.usingProxy) {  
1.             setPreemptiveProxyAuthentication(requests);  
1.         }  
1.         if (!setRequests) {  
1.    
1.             /* We're very particular about the order in which we 
1.              * set the request headers here.  The order should not 
1.              * matter, but some careless CGI programs have been 
1.              * written to expect a very particular order of the 
1.              * standard headers.  To name names, the order in which 
1.              * Navigator3.0 sends them.  In particular, we make *sure* 
1.              * to send Content-type: <> and Content-length:<> second 
1.              * to last and last, respectively, in the case of a POST 
1.              * request. 
1.              */  
1.             if (!failedOnce)  
1.                 requests.prepend(method + " " + http.getURLFile()+" "  +  
1.                                  httpVersion, null);  
1.             if (!getUseCaches()) {  
1.                 requests.setIfNotSet ("Cache-Control", "no-cache");  
1.                 requests.setIfNotSet ("Pragma", "no-cache");  
1.             }  
1.             requests.setIfNotSet("User-Agent", userAgent);  
1.             int port = url.getPort();  
1.             String host = url.getHost();  
1.             if (port != -1 && port != url.getDefaultPort()) {  
1.                 host += ":" + String.valueOf(port);  
1.             }  
1.             requests.setIfNotSet("Host", host);  
1.             requests.setIfNotSet("Accept", acceptString);  
1.    
1.             /* 
1.              * For HTTP/1.1 the default behavior is to keep connections alive. 
1.              * However, we may be talking to a 1.0 server so we should set 
1.              * keep-alive just in case, except if we have encountered an error 
1.              * or if keep alive is disabled via a system property 
1.              */  
1.    
1.             // Try keep-alive only on first attempt  
1.             if (!failedOnce && http.getHttpKeepAliveSet()) {  
1.                 if (http.usingProxy) {  
1.                     requests.setIfNotSet("Proxy-Connection", "keep-alive");  
1.                 } else {  
1.                     requests.setIfNotSet("Connection", "keep-alive");  
1.                 }  
1.             } else {  
1.                 /* 
1.                  * RFC 2616 HTTP/1.1 section 14.10 says: 
1.                  * HTTP/1.1 applications that do not support persistent 
1.                  * connections MUST include the "close" connection option 
1.                  * in every message 
1.                  */  
1.                 requests.setIfNotSet("Connection", "close");  
1.             }  
1.             // Set modified since if necessary  
1.             long modTime = getIfModifiedSince();  
1.             if (modTime != 0 ) {  
1.                 Date date = new Date(modTime);  
1.                 //use the preferred date format according to RFC 2068(HTTP1.1),  
1.                 // RFC 822 and RFC 1123  
1.                 SimpleDateFormat fo =  
1.                   new SimpleDateFormat ("EEE, dd MMM yyyy HH:mm:ss 'GMT'", Locale.US);  
1.                 fo.setTimeZone(TimeZone.getTimeZone("GMT"));  
1.                 requests.setIfNotSet("If-Modified-Since", fo.format(date));  
1.             }  
1.             // check for preemptive authorization  
1.             AuthenticationInfo sauth = AuthenticationInfo.getServerAuth(url);  
1.             if (sauth != null && sauth.supportsPreemptiveAuthorization() ) {  
1.                 // Sets "Authorization"  
1.                 requests.setIfNotSet(sauth.getHeaderName(), sauth.getHeaderValue(url,method));  
1.                 currentServerCredentials = sauth;  
1.             }  
1.    
1.             if (!method.equals("PUT") && (poster != null || streaming())) {  
1.                 requests.setIfNotSet ("Content-type",  
1.                         "application/x-www-form-urlencoded");  
1.             }  
1.    
1.             if (streaming()) {  
1.                 if (chunkLength != -1) {  
1.                     requests.set ("Transfer-Encoding", "chunked");  
1.                 } else {  
1.                     requests.set ("Content-Length", String.valueOf(fixedContentLength));  
1.                 }  
1.             } else if (poster != null) {  
1.                 /* add Content-Length & POST/PUT data */  
1.                 synchronized (poster) {  
1.                     /* close it, so no more data can be added */  
1.                     poster.close();  
1.                     requests.set("Content-Length",  
1.                                  String.valueOf(poster.size()));  
1.                 }  
1.             }  
1.    
1.             // get applicable cookies based on the uri and request headers  
1.             // add them to the existing request headers  
1.             setCookieHeader();  
1. …  
1. }  
1.    
1.    
1. 再来看看把socket响应信息解析为http的响应信息的代码：  
1. sun.net.www.http.HttpClient.java的parseHTTP方法：  
1. private boolean parseHTTPHeader(MessageHeader responses, ProgressSource pi, HttpURLConnection httpuc)  
1.     throws IOException {  
1.         /* If "HTTP/*" is found in the beginning, return true.  Let 
1.          * HttpURLConnection parse the mime header itself. 
1.          * 
1.          * If this isn't valid HTTP, then we don't try to parse a header 
1.          * out of the beginning of the response into the responses, 
1.          * and instead just queue up the output stream to it's very beginning. 
1.          * This seems most reasonable, and is what the NN browser does. 
1.          */  
1.    
1.         keepAliveConnections = -1;  
1.         keepAliveTimeout = 0;  
1.    
1.         boolean ret = false;  
1.         byte[] b = new byte[8];  
1.    
1.         try {  
1.             int nread = 0;  
1.             serverInput.mark(10);  
1.             while (nread < 8) {  
1.                 int r = serverInput.read(b, nread, 8 - nread);  
1.                 if (r < 0) {  
1.                     break;  
1.                 }  
1.                 nread += r;  
1.             }  
1.             String keep=null;  
1.             ret = b[0] == 'H' && b[1] == 'T'  
1.                     && b[2] == 'T' && b[3] == 'P' && b[4] == '/' &&  
1.                 b[5] == '1' && b[6] == '.';  
1.             serverInput.reset();  
1.             if (ret) { // is valid HTTP - response started w/ "HTTP/1."  
1.                 responses.parseHeader(serverInput);  
1.    
1.                 // we've finished parsing http headers  
1.                 // check if there are any applicable cookies to set (in cache)  
1.                 if (cookieHandler != null) {  
1.                     URI uri = ParseUtil.toURI(url);  
1.                     // NOTE: That cast from Map shouldn't be necessary but  
1.                     // a bug in javac is triggered under certain circumstances  
1.                     // So we do put the cast in as a workaround until  
1.                     // it is resolved.  
1.                     if (uri != null)  
1.                         cookieHandler.put(uri, (Map<java.lang.String,java.util.List<java.lang.String>>)responses.getHeaders());  
1.                 }  
1.    
1.                 /* decide if we're keeping alive: 
1.                  * This is a bit tricky.  There's a spec, but most current 
1.                  * servers (10/1/96) that support this differ in dialects. 
1.                  * If the server/client misunderstand each other, the 
1.                  * protocol should fall back onto HTTP/1.0, no keep-alive. 
1.                  */  
1.                 if (usingProxy) { // not likely a proxy will return this  
1.                     keep = responses.findValue("Proxy-Connection");  
1.                 }  
1.                 if (keep == null) {  
1.                     keep = responses.findValue("Connection");  
1.                 }  
1.                 if (keep != null && keep.toLowerCase().equals("keep-alive")) {  
1.                     /* some servers, notably Apache1.1, send something like: 
1.                      * "Keep-Alive: timeout=15, max=1" which we should respect. 
1.                      */  
1.                     HeaderParser p = new HeaderParser(  
1.                             responses.findValue("Keep-Alive"));  
1.                     if (p != null) {  
1.                         /* default should be larger in case of proxy */  
1.                         keepAliveConnections = p.findInt("max", usingProxy?50:5);  
1.                         keepAliveTimeout = p.findInt("timeout", usingProxy?60:5);  
1.                     }  
1.                 } else if (b[7] != '0') {  
1.                     /* 
1.                      * We're talking 1.1 or later. Keep persistent until 
1.                      * the server says to close. 
1.                      */  
1.                     if (keep != null) {  
1.                         /* 
1.                          * The only Connection token we understand is close. 
1.                          * Paranoia: if there is any Connection header then 
1.                          * treat as non-persistent. 
1.                          */  
1.                         keepAliveConnections = 1;  
1.                     } else {  
1.                         keepAliveConnections = 5;  
1.                     }  
1.                 }  
1. ……  
1. }  
1.    
1.    
1. 对于java.net包的http，ftp等各种协议的底层实现，可以参考rt.jar下面的几个包的代码：  
1. sun.net.www.protocl下的几个包。  
1.    
1.    
1. 在http client中也可以设置代理：  
1.                HostConfiguration conf = new HostConfiguration();  
1.                conf.setHost(host);  
1.                conf.setProxy(ip, 80);  
1.                statusCode = httpclient.executeMethod(conf,getMethod);  
1.    
1. httpclient自己也是基于socket封装的http处理的库。底层代理的实现是一样的。  
1.    
1.    
1. 另外一种不设置代理，通过反射修改InetAddress的cache也是ok的。但是这种方法非常不推荐，不要使用，因为对于proxy代理服务器概念了解不清楚，最开始还使用这种方法，  
1. public static void jdkDnsNoCache(final String host, final String ip)  
1.            throws SecurityException, NoSuchFieldException,  
1.            IllegalArgumentException, IllegalAccessException {  
1.        if (StringUtils.isBlank(host)) {  
1.            return;  
1.        }  
1.        final Class clazz = java.net.InetAddress.class;  
1.        final Field cacheField = clazz.getDeclaredField("addressCache");  
1.        cacheField.setAccessible(true);  
1.        final Object o = cacheField.get(clazz);  
1.        Class clazz2 = o.getClass();  
1.        final Field cacheMapField = clazz2.getDeclaredField("cache");  
1.        cacheMapField.setAccessible(true);  
1.        final Map cacheMap = (Map) cacheMapField.get(o);  
1.        AccessController.doPrivileged(new PrivilegedAction() {  
1.            public Object run() {  
1.               try {  
1.                   synchronized (o) {// 同步是必须的,因为o可能会有多个线程同时访问修改。  
1.                      // cacheMap.clear();//这步比较关键，用于清除原来的缓存  
1. //                   cacheMap.remove(host);  
1.                      if (!StringUtils.isBlank(ip)) {  
1.                          InetAddress inet = InetAddress.getByAddress(host,IPUtil.int2byte(ip));  
1.                          InetAddress addressstart = InetAddress.getByName(host);  
1.                          Object cacheEntry = cacheMap.get(host);  
1.                          cacheMap.put(host,newCacheEntry(inet,cacheEntry));  
1. //                       cacheMap.put(host,newCacheEntry(newInetAddress(host, ip)));  
1.                      }else{  
1.                          cacheMap.remove(host);  
1.                      }  
1. //                   System.out.println(getStaticProperty(  
1. //                          "java.net.InetAddress", "addressCacheInit"));  
1.                      // System.out.println(invokeStaticMethod("java.net.InetAddress","getCachedAddress",new  
1.                      // Object[]{host}));  
1.                   }  
1.               } catch (Throwable te) {  
1.                   throw new RuntimeException(te);  
1.               }  
1.               return null;  
1.            }  
1.        });  
1.        final Map cacheMapafter = (Map) cacheMapField.get(o);  
1.        System.out.println(cacheMapafter);  
1.    
1.     }  
1.    
1. 关于java中对于DNS的缓存设置可以参考：  
1. 1.在${java_home}/jre/lib/secuiry/java.secuiry文件，修改下面为   
1.   networkaddress.cache.negative.ttl=0   DNS解析不成功的缓存时间  
1. networkaddress.cache.ttl=0    DNS解析成功的缓存的时间  
1. 2.jvm启动时增加下面两个启动环境变量  
1.   -Dsun.net.inetaddr.ttl=0  
1.       -Dsun.net.inetaddr.negative.ttl=0  
1.    
1.    
1. 如果在java程序中使用，可以这么设置设置：  
1.     java.security.Security.setProperty("networkaddress.cache.ttl" , "0");  
1.         java.security.Security.setProperty("networkaddress.cache.negative.ttl" , "0");  
1.    
1.    还有几篇文档链接可以查看：  
1.        http://www.rgagnon.com/javadetails/java-0445.html  
1. http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=6247501  
1.    
1.   linux下关于ＯＳ　ＤＮＳ设置的几个文件是  
1. /etc/resolve.conf  
1. /etc/nscd.conf  
1. /etc/nsswitch.conf  
1.    
1. http://www.linuxfly.org/post/543/  
1. http://linux.die.net/man/5/nscd.conf  
1. http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_:_Ch18_:_Configuring_DNS  
1. http://linux.die.net/man/5/nscd.conf  
最近有个需求需要对于获取URL页面进行host绑定并且立即生效，在java里面实现可以用代理服务器来实现：因为在测试环境下可能需要通过绑定来访问测试环境的应用 实现代码如下： public static String getResponseText(String queryUrl,String host,String ip) { //queryUrl，完整的url，host和ip需要绑定的host和ip InputStream is = null; BufferedReader br = null; StringBuffer res = new StringBuffer(); try { HttpURLConnection httpUrlConn = null; URL url = new URL(queryUrl); if(ip!=null){ String str[] = ip.split("\\."); byte[] b =new byte[str.length]; for(int i=0,len=str.length;i<len;i++){ b[i] = (byte)(Integer.parseInt(str[i],10)); } Proxy proxy = new Proxy(Proxy.Type.HTTP, new InetSocketAddress(InetAddress.getByAddress(b), 80)); //b是绑定的ip，生成proxy代理对象，因为http底层是socket实现， httpUrlConn = (HttpURLConnection) url .openConnection(proxy); }else{ httpUrlConn = (HttpURLConnection) url .openConnection(); } httpUrlConn.setRequestMethod("GET"); httpUrlConn.setDoOutput(true); httpUrlConn.setConnectTimeout(2000); httpUrlConn.setReadTimeout(2000); httpUrlConn.setDefaultUseCaches(false); httpUrlConn.setUseCaches(false); is = httpUrlConn.getInputStream(); 那么底层对于proxy对象到底是怎么处理，底层的socket实现到底怎么样，带着这个疑惑看了下jdk的rt.jar对于这块的处理 httpUrlConn = (HttpURLConnection) url.openConnection(proxy) java.net.URL类里面的openConnection方法： public URLConnection openConnection(Proxy proxy){ … return handler.openConnection(this, proxy); Handler是sun.net.www.protocol.http.Handler.java类，继承java.net. URLStreamHandler.java类，用来处理http连接请求响应的。 } Handler的方法： protected java.net.URLConnection openConnection(URL u, Proxy p) throws IOException { return new HttpURLConnection(u, p, this); } 只是简单的生成sun.net.www.protocl.http.HttpURLConnection对象，并进行初始化 protected HttpURLConnection(URL u, Proxy p, Handler handler) { super(u); requests = new MessageHeader(); 请求头信息生成类 responses = new MessageHeader(); 响应头信息解析类 this.handler = handler; instProxy = p; 代理服务器对象 cookieHandler = (CookieHandler)java.security.AccessController.doPrivileged( new java.security.PrivilegedAction() { public Object run() { return CookieHandler.getDefault(); } }); cacheHandler = (ResponseCache)java.security.AccessController.doPrivileged( new java.security.PrivilegedAction() { public Object run() { return ResponseCache.getDefault(); } }); } 最终在httpUrlConn.getInputStream();才进行socket连接，发送http请求，解析http响应信息。具体过程如下： sun.net.www.protocl.http.HttpURLConnection.java的getInputStream方法： public synchronized InputStream getInputStream() throws IOException { ...socket连接 connect(); ... ps = (PrintStream)http.getOutputStream(); 获得输出流，打开连接之后已经生成。 if (!streaming()) { writeRequests(); 输出http请求头信息 } ... http.parseHTTP(responses, pi, this); 解析响应信息 if(logger.isLoggable(Level.FINEST)) { logger.fine(responses.toString()); } inputStream = http.getInputStream(); 获得输入流 } 其中connect()调用方法链： plainConnect(){ ... Proxy p = null; if (sel != null) { URI uri = sun.net.www.ParseUtil.toURI(url); Iterator<Proxy> it = sel.select(uri).iterator(); while (it.hasNext()) { p = it.next(); try { if (!failedOnce) { http = getNewHttpClient(url, p, connectTimeout); ... } getNewHttpClient(){ ... return HttpClient.New(url, p, connectTimeout, useCache); ... } 下面跟进去最终建立socket连接的代码： sun.net.www.http.HttpClient.java的openServer()方法建立socket连接： protected synchronized void openServer() throws IOException { ... if ((proxy != null) && (proxy.type() == Proxy.Type.HTTP)) { sun.net.www.URLConnection.setProxiedHost(host); if (security != null) { security.checkConnect(host, port); } privilegedOpenServer((InetSocketAddress) proxy.address());最终socket连接的是设置的代理服务器的地址， ... } private synchronized void privilegedOpenServer(final InetSocketAddress server) throws IOException { try { java.security.AccessController.doPrivileged( new java.security.PrivilegedExceptionAction() { public Object run() throws IOException { openServer(server.getHostName(), server.getPort()); 注意openserver函数 这里的server的getHostName是设置的代理服务器，（ip或者hostname，如果是host绑定设置的代理服务器的ip，那么这里getHostName出来的就是ip地址，可以去查看InetSocketAddress类的getHostName方法） return null; } }); } catch (java.security.PrivilegedActionException pae) { throw (IOException) pae.getException(); } } public void openServer(String server, int port) throws IOException { serverSocket = doConnect(server, port); 生成的Socket连接对象 try { serverOutput = new PrintStream( new BufferedOutputStream(serverSocket.getOutputStream()), false, encoding); 生成输出流， } catch (UnsupportedEncodingException e) { throw new InternalError(encoding+" encoding not found"); } serverSocket.setTcpNoDelay(true); } protected Socket doConnect (String server, int port) throws IOException, UnknownHostException { Socket s; if (proxy != null) { if (proxy.type() == Proxy.Type.SOCKS) { s = (Socket) AccessController.doPrivileged( new PrivilegedAction() { public Object run() { return new Socket(proxy); }}); } else s = new Socket(Proxy.NO_PROXY); } else s = new Socket(); // Instance specific timeouts do have priority, that means // connectTimeout & readTimeout (-1 means not set) // Then global default timeouts // Then no timeout. if (connectTimeout >= 0) { s.connect(new InetSocketAddress(server, port), connectTimeout); } else { if (defaultConnectTimeout > 0) { s.connect(new InetSocketAddress(server, port), defaultConnectTimeout);//连接到代理服务器，看下面Socket类的connect方法代码 } else { s.connect(new InetSocketAddress(server, port)); } } if (readTimeout >= 0) s.setSoTimeout(readTimeout); else if (defaultSoTimeout > 0) { s.setSoTimeout(defaultSoTimeout); } return s; } 上面的new InetSocketAddress(server, port)这里会涉及到java DNS cache的处理， public InetSocketAddress(String hostname, int port) { if (port < 0 || port > 0xFFFF) { throw new IllegalArgumentException("port out of range:" + port); } if (hostname == null) { throw new IllegalArgumentException("hostname can't be null"); } try { addr = InetAddress.getByName(hostname); //这里会有java DNS缓存的处理，先从缓存取hostname绑定的ip地址，如果取不到再通过OS的DNS cache机制去取，取不到再从DNS服务器上取。 } catch(UnknownHostException e) { this.hostname = hostname; addr = null; } this.port = port; } 当然最终的Socket.java的connect方法 java.net.socket public void connect(SocketAddress endpoint, int timeout) throws IOException { if (endpoint == null) if (timeout < 0) throw new IllegalArgumentException("connect: timeout can't be negative"); if (isClosed()) throw new SocketException("Socket is closed"); if (!oldImpl && isConnected()) throw new SocketException("already connected"); if (!(endpoint instanceof InetSocketAddress)) throw new IllegalArgumentException("Unsupported address type"); InetSocketAddress epoint = (InetSocketAddress) endpoint; SecurityManager security = System.getSecurityManager(); if (security != null) { if (epoint.isUnresolved()) security.checkConnect(epoint.getHostName(), epoint.getPort()); else security.checkConnect(epoint.getAddress().getHostAddress(), epoint.getPort()); } if (!created) createImpl(true); if (!oldImpl) impl.connect(epoint, timeout); else if (timeout == 0) { if (epoint.isUnresolved()) //如果没有设置SocketAddress的ip地址，则用域名去访问 impl.connect(epoint.getAddress().getHostName(), epoint.getPort()); else impl.connect(epoint.getAddress(), epoint.getPort()); 最终socket连接的是设置的SocketAddress的ip地址， } else throw new UnsupportedOperationException("SocketImpl.connect(addr, timeout)"); connected = true; /* * If the socket was not bound before the connect, it is now because * the kernel will have picked an ephemeral port & a local address */ bound = true; } 我们再看下通过socket来发送HTTP请求的处理代码，也就是sun.net.www.protocl.http.HttpURLConnection.java的getInputStream方法中调用的writeRequests()方法： private void writeRequests() throws IOException { 这段代码就是封装http请求的头请求信息，通过socket发送出去 /* print all message headers in the MessageHeader * onto the wire - all the ones we've set and any * others that have been set */ // send any pre-emptive authentication if (http.usingProxy) { setPreemptiveProxyAuthentication(requests); } if (!setRequests) { /* We're very particular about the order in which we * set the request headers here. The order should not * matter, but some careless CGI programs have been * written to expect a very particular order of the * standard headers. To name names, the order in which * Navigator3.0 sends them. In particular, we make *sure* * to send Content-type: <> and Content-length:<> second * to last and last, respectively, in the case of a POST * request. */ if (!failedOnce) requests.prepend(method + " " + http.getURLFile()+" " + httpVersion, null); if (!getUseCaches()) { requests.setIfNotSet ("Cache-Control", "no-cache"); requests.setIfNotSet ("Pragma", "no-cache"); } requests.setIfNotSet("User-Agent", userAgent); int port = url.getPort(); String host = url.getHost(); if (port != -1 && port != url.getDefaultPort()) { host += ":" + String.valueOf(port); } requests.setIfNotSet("Host", host); requests.setIfNotSet("Accept", acceptString); /* * For HTTP/1.1 the default behavior is to keep connections alive. * However, we may be talking to a 1.0 server so we should set * keep-alive just in case, except if we have encountered an error * or if keep alive is disabled via a system property */ // Try keep-alive only on first attempt if (!failedOnce && http.getHttpKeepAliveSet()) { if (http.usingProxy) { requests.setIfNotSet("Proxy-Connection", "keep-alive"); } else { requests.setIfNotSet("Connection", "keep-alive"); } } else { /* * RFC 2616 HTTP/1.1 section 14.10 says: * HTTP/1.1 applications that do not support persistent * connections MUST include the "close" connection option * in every message */ requests.setIfNotSet("Connection", "close"); } // Set modified since if necessary long modTime = getIfModifiedSince(); if (modTime != 0 ) { Date date = new Date(modTime); //use the preferred date format according to RFC 2068(HTTP1.1), // RFC 822 and RFC 1123 SimpleDateFormat fo = new SimpleDateFormat ("EEE, dd MMM yyyy HH:mm:ss 'GMT'", Locale.US); fo.setTimeZone(TimeZone.getTimeZone("GMT")); requests.setIfNotSet("If-Modified-Since", fo.format(date)); } // check for preemptive authorization AuthenticationInfo sauth = AuthenticationInfo.getServerAuth(url); if (sauth != null && sauth.supportsPreemptiveAuthorization() ) { // Sets "Authorization" requests.setIfNotSet(sauth.getHeaderName(), sauth.getHeaderValue(url,method)); currentServerCredentials = sauth; } if (!method.equals("PUT") && (poster != null || streaming())) { requests.setIfNotSet ("Content-type", "application/x-www-form-urlencoded"); } if (streaming()) { if (chunkLength != -1) { requests.set ("Transfer-Encoding", "chunked"); } else { requests.set ("Content-Length", String.valueOf(fixedContentLength)); } } else if (poster != null) { /* add Content-Length & POST/PUT data */ synchronized (poster) { /* close it, so no more data can be added */ poster.close(); requests.set("Content-Length", String.valueOf(poster.size())); } } // get applicable cookies based on the uri and request headers // add them to the existing request headers setCookieHeader(); … } 再来看看把socket响应信息解析为http的响应信息的代码： sun.net.www.http.HttpClient.java的parseHTTP方法： private boolean parseHTTPHeader(MessageHeader responses, ProgressSource pi, HttpURLConnection httpuc) throws IOException { /* If "HTTP/*" is found in the beginning, return true. Let * HttpURLConnection parse the mime header itself. * * If this isn't valid HTTP, then we don't try to parse a header * out of the beginning of the response into the responses, * and instead just queue up the output stream to it's very beginning. * This seems most reasonable, and is what the NN browser does. */ keepAliveConnections = -1; keepAliveTimeout = 0; boolean ret = false; byte[] b = new byte[8]; try { int nread = 0; serverInput.mark(10); while (nread < 8) { int r = serverInput.read(b, nread, 8 - nread); if (r < 0) { break; } nread += r; } String keep=null; ret = b[0] == 'H' && b[1] == 'T' && b[2] == 'T' && b[3] == 'P' && b[4] == '/' && b[5] == '1' && b[6] == '.'; serverInput.reset(); if (ret) { // is valid HTTP - response started w/ "HTTP/1." responses.parseHeader(serverInput); // we've finished parsing http headers // check if there are any applicable cookies to set (in cache) if (cookieHandler != null) { URI uri = ParseUtil.toURI(url); // NOTE: That cast from Map shouldn't be necessary but // a bug in javac is triggered under certain circumstances // So we do put the cast in as a workaround until // it is resolved. if (uri != null) cookieHandler.put(uri, (Map<java.lang.String,java.util.List<java.lang.String>>)responses.getHeaders()); } /* decide if we're keeping alive: * This is a bit tricky. There's a spec, but most current * servers (10/1/96) that support this differ in dialects. * If the server/client misunderstand each other, the * protocol should fall back onto HTTP/1.0, no keep-alive. */ if (usingProxy) { // not likely a proxy will return this keep = responses.findValue("Proxy-Connection"); } if (keep == null) { keep = responses.findValue("Connection"); } if (keep != null && keep.toLowerCase().equals("keep-alive")) { /* some servers, notably Apache1.1, send something like: * "Keep-Alive: timeout=15, max=1" which we should respect. */ HeaderParser p = new HeaderParser( responses.findValue("Keep-Alive")); if (p != null) { /* default should be larger in case of proxy */ keepAliveConnections = p.findInt("max", usingProxy?50:5); keepAliveTimeout = p.findInt("timeout", usingProxy?60:5); } } else if (b[7] != '0') { /* * We're talking 1.1 or later. Keep persistent until * the server says to close. */ if (keep != null) { /* * The only Connection token we understand is close. * Paranoia: if there is any Connection header then * treat as non-persistent. */ keepAliveConnections = 1; } else { keepAliveConnections = 5; } } …… } 对于java.net包的http，ftp等各种协议的底层实现，可以参考rt.jar下面的几个包的代码： sun.net.www.protocl下的几个包。 在http client中也可以设置代理： HostConfiguration conf = new HostConfiguration(); conf.setHost(host); conf.setProxy(ip, 80); statusCode = httpclient.executeMethod(conf,getMethod); httpclient自己也是基于socket封装的http处理的库。底层代理的实现是一样的。 另外一种不设置代理，通过反射修改InetAddress的cache也是ok的。但是这种方法非常不推荐，不要使用，因为对于proxy代理服务器概念了解不清楚，最开始还使用这种方法， public static void jdkDnsNoCache(final String host, final String ip) throws SecurityException, NoSuchFieldException, IllegalArgumentException, IllegalAccessException { if (StringUtils.isBlank(host)) { return; } final Class clazz = java.net.InetAddress.class; final Field cacheField = clazz.getDeclaredField("addressCache"); cacheField.setAccessible(true); final Object o = cacheField.get(clazz); Class clazz2 = o.getClass(); final Field cacheMapField = clazz2.getDeclaredField("cache"); cacheMapField.setAccessible(true); final Map cacheMap = (Map) cacheMapField.get(o); AccessController.doPrivileged(new PrivilegedAction() { public Object run() { try { synchronized (o) {// 同步是必须的,因为o可能会有多个线程同时访问修改。 // cacheMap.clear();//这步比较关键，用于清除原来的缓存 // cacheMap.remove(host); if (!StringUtils.isBlank(ip)) { InetAddress inet = InetAddress.getByAddress(host,IPUtil.int2byte(ip)); InetAddress addressstart = InetAddress.getByName(host); Object cacheEntry = cacheMap.get(host); cacheMap.put(host,newCacheEntry(inet,cacheEntry)); // cacheMap.put(host,newCacheEntry(newInetAddress(host, ip))); }else{ cacheMap.remove(host); } // System.out.println(getStaticProperty( // "java.net.InetAddress", "addressCacheInit")); // System.out.println(invokeStaticMethod("java.net.InetAddress","getCachedAddress",new // Object[]{host})); } } catch (Throwable te) { throw new RuntimeException(te); } return null; } }); final Map cacheMapafter = (Map) cacheMapField.get(o); System.out.println(cacheMapafter); } 关于java中对于DNS的缓存设置可以参考： 1.在${java_home}/jre/lib/secuiry/java.secuiry文件，修改下面为 networkaddress.cache.negative.ttl=0 DNS解析不成功的缓存时间 networkaddress.cache.ttl=0 DNS解析成功的缓存的时间 2.jvm启动时增加下面两个启动环境变量 -Dsun.net.inetaddr.ttl=0 -Dsun.net.inetaddr.negative.ttl=0 如果在java程序中使用，可以这么设置设置： java.security.Security.setProperty("networkaddress.cache.ttl" , "0"); java.security.Security.setProperty("networkaddress.cache.negative.ttl" , "0"); 还有几篇文档链接可以查看： http://www.rgagnon.com/javadetails/java-0445.html http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=6247501 linux下关于ＯＳ　ＤＮＳ设置的几个文件是 /etc/resolve.conf /etc/nscd.conf /etc/nsswitch.conf http://www.linuxfly.org/post/543/ http://linux.die.net/man/5/nscd.conf http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_:_Ch18_:_Configuring_DNS http://linux.die.net/man/5/nscd.conf

分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")
1. 上一篇：[jetty源码阅读总结1](http://blog.csdn.net/zhongweijian/article/details/7619440)
1. 下一篇：[关于jetty和webx对于HttpServletResponse getWriter和getOutputStream的处理](http://blog.csdn.net/zhongweijian/article/details/7619460)
查看评论[]()

  暂无评论
您还没有登录,请[[登录]](http://passport.csdn.net/account/login?from=http%3A%2F%2Fblog.csdn.net%2Fzhongweijian%2Farticle%2Fdetails%2F7619453)或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fzhongweijian%2Farticle%2Fdetails%2F7619453)

* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()
[![TOP]()](http://blog.csdn.net/zhongweijian/article/details/7619453# "回到顶部")

个人资料

[![]( "访问我的空间")](http://my.csdn.net/zhongweijian)
[zhongweijian](http://my.csdn.net/zhongweijian)

[](http://blog.csdn.net/zhongweijian/article/details/7619453# "[加关注]") [](http://my.csdn.net/my/letter/send/zhongweijian "[发私信]")
[![]()](http://medal.blog.csdn.net/allmedal.aspx)

* 访问：57737次
* 积分：2164分
* 排名：第2692名

* 原创：158篇
* 转载：3篇
* 译文：2篇
* 评论：30条

文章搜索

[]()
文章分类

* [acm](http://blog.csdn.net/zhongweijian/article/category/589017)(8)
* [flash as](http://blog.csdn.net/zhongweijian/article/category/657598)(1)
* [flex](http://blog.csdn.net/zhongweijian/article/category/606568)(18)
* [java](http://blog.csdn.net/zhongweijian/article/category/640345)(69)
* [java web开发](http://blog.csdn.net/zhongweijian/article/category/586565)(12)
* [linux](http://blog.csdn.net/zhongweijian/article/category/644484)(7)
* [web框架](http://blog.csdn.net/zhongweijian/article/category/657599)(4)
* [数据库](http://blog.csdn.net/zhongweijian/article/category/610388)(11)
* [脚本语言](http://blog.csdn.net/zhongweijian/article/category/611805)(2)
* [c&c++](http://blog.csdn.net/zhongweijian/article/category/1158398)(2)
* [nosql](http://blog.csdn.net/zhongweijian/article/category/1158401)(4)
* [jetty&tomcat](http://blog.csdn.net/zhongweijian/article/category/1158414)(4)
* [开源框架](http://blog.csdn.net/zhongweijian/article/category/1158428)(27)
* [架构](http://blog.csdn.net/zhongweijian/article/category/1202606)(1)

文章存档

* [2012年10月](http://blog.csdn.net/zhongweijian/article/month/2012/10)(1)
* [2012年09月](http://blog.csdn.net/zhongweijian/article/month/2012/09)(11)
* [2012年08月](http://blog.csdn.net/zhongweijian/article/month/2012/08)(14)
* [2012年07月](http://blog.csdn.net/zhongweijian/article/month/2012/07)(9)
* [2012年06月](http://blog.csdn.net/zhongweijian/article/month/2012/06)(41)
* [2012年05月](http://blog.csdn.net/zhongweijian/article/month/2012/05)(20)
* [2011年07月](http://blog.csdn.net/zhongweijian/article/month/2011/07)(1)
* [2010年06月](http://blog.csdn.net/zhongweijian/article/month/2010/06)(1)
* [2010年05月](http://blog.csdn.net/zhongweijian/article/month/2010/05)(2)
* [2010年04月](http://blog.csdn.net/zhongweijian/article/month/2010/04)(1)
* [2010年03月](http://blog.csdn.net/zhongweijian/article/month/2010/03)(6)
* [2010年02月](http://blog.csdn.net/zhongweijian/article/month/2010/02)(2)
* [2010年01月](http://blog.csdn.net/zhongweijian/article/month/2010/01)(10)
* [2009年12月](http://blog.csdn.net/zhongweijian/article/month/2009/12)(15)
* [2009年11月](http://blog.csdn.net/zhongweijian/article/month/2009/11)(4)
* [2009年10月](http://blog.csdn.net/zhongweijian/article/month/2009/10)(8)
* [2009年09月](http://blog.csdn.net/zhongweijian/article/month/2009/09)(10)
* [2009年08月](http://blog.csdn.net/zhongweijian/article/month/2009/08)(7)

展开
阅读排行

* [HTTP超文本传输协议-HTTP/1.1中文版](http://blog.csdn.net/zhongweijian/article/details/8002034 "HTTP超文本传输协议-HTTP/1.1中文版")(3067)
* [flex使用lineChart和DateTimeAxis实现时序图](http://blog.csdn.net/zhongweijian/article/details/4692368 "flex使用lineChart和DateTimeAxis实现时序图")(3046)
* [flex柱状图和折线图的混合图使用](http://blog.csdn.net/zhongweijian/article/details/4692668 "flex柱状图和折线图的混合图使用")(2486)
* [jython安装和使用](http://blog.csdn.net/zhongweijian/article/details/4742549 "jython安装和使用")(1644)
* [flex图表坐标轴样式设置](http://blog.csdn.net/zhongweijian/article/details/5381953 "flex图表坐标轴样式设置")(1387)
* [flex图表数据动态更新效果示例](http://blog.csdn.net/zhongweijian/article/details/5113532 "flex图表数据动态更新效果示例")(1255)
* [html5 新特性支持的浏览器检测](http://blog.csdn.net/zhongweijian/article/details/8002208 "html5 新特性支持的浏览器检测")(1207)
* [flex画直线示例](http://blog.csdn.net/zhongweijian/article/details/5047080 "flex画直线示例")(1094)
* [flash as3.0动态添加属性和方法](http://blog.csdn.net/zhongweijian/article/details/5339155 "flash as3.0动态添加属性和方法")(1062)
* [lucene3使用示例](http://blog.csdn.net/zhongweijian/article/details/7622675 "lucene3使用示例")(998)

评论排行

* [flex柱状图和折线图的混合图使用](http://blog.csdn.net/zhongweijian/article/details/4692668 "flex柱状图和折线图的混合图使用")(4)
* [Error parsing XML. org.xml.sax.SAXParseException: Element type "sqlMapConfig" must be declared出错解决方法](http://blog.csdn.net/zhongweijian/article/details/4651329 "Error parsing XML. org.xml.sax.SAXParseException: Element type "sqlMapConfig" must be declared出错解决方法")(2)
* [大数加减乘除求根源码](http://blog.csdn.net/zhongweijian/article/details/4517251 "大数加减乘除求根源码")(2)
* [关于jetty和webx对于HttpServletResponse getWriter和getOutputStream的处理](http://blog.csdn.net/zhongweijian/article/details/7619460 "关于jetty和webx对于HttpServletResponse getWriter和getOutputStream的处理")(2)
* [jython安装和使用](http://blog.csdn.net/zhongweijian/article/details/4742549 "jython安装和使用")(2)
* [jfreechart图片无法显示问题](http://blog.csdn.net/zhongweijian/article/details/4504272 "jfreechart图片无法显示问题")(2)
* [java 缓存框架java caching system使用示例](http://blog.csdn.net/zhongweijian/article/details/5160136 "java 缓存框架java caching system使用示例")(2)
* [linux c++连接mysql示例](http://blog.csdn.net/zhongweijian/article/details/4865966 "linux c++连接mysql示例")(2)
* [oracle 定长字段查询问题 ，ibatis 与pl/sql查询的char类型字段查询不同](http://blog.csdn.net/zhongweijian/article/details/4494972 " oracle 定长字段查询问题 ，ibatis 与pl/sql查询的char类型字段查询不同")(2)
* [flex 实现横向树状图](http://blog.csdn.net/zhongweijian/article/details/5069509 "flex 实现横向树状图")(1)
推荐文章

最新评论

* [lucene3.6.0的查询条件分析](http://blog.csdn.net/zhongweijian/article/details/7622693#comments)

main_xtgjfge: 不错——
* [java内存分配之 Thread Local Allocation Buffer](http://blog.csdn.net/zhongweijian/article/details/8010745#comments)

chenshufei2: 多个线程不是共享内存共享数据吗，只有进程才有独享的对象吗。，那怎么理解jvm为了优化，给每个线程分配...
* [linux c++连接mysql示例](http://blog.csdn.net/zhongweijian/article/details/4865966#comments)

zhongweijian: 呵呵，弄错了，
* [linux c++连接mysql示例](http://blog.csdn.net/zhongweijian/article/details/4865966#comments)

Meteora7: 楼主，换行好像是“\n” 吧
* [jetty源码阅读总结1](http://blog.csdn.net/zhongweijian/article/details/7619440#comments)

chancewang001: 很好，学习了
* [关于jetty和webx对于HttpServletResponse getWriter和getOutputStream的处理](http://blog.csdn.net/zhongweijian/article/details/7619460#comments)

zhongweijian: webx是做了个adapter能够解决的。
* [关于jetty和webx对于HttpServletResponse getWriter和getOutputStream的处理](http://blog.csdn.net/zhongweijian/article/details/7619460#comments)

yj504535760: 还是没有解决这个问题吗
* [jython安装和使用](http://blog.csdn.net/zhongweijian/article/details/4742549#comments)

iNexus: 试了执行文件的部分。好用，谢谢！
* [大数加减乘除求根源码](http://blog.csdn.net/zhongweijian/article/details/4517251#comments)

he381835877: 减法有问题啊~>>2 , >>25 >> 329 结果是-696？
* [Error parsing XML. org.xml.sax.SAXParseException: Element type "sqlMapConfig" must be declared出错解决方法](http://blog.csdn.net/zhongweijian/article/details/4651329#comments)

lendar_2010: @ruelang:多了 Config
      ![]()

[公司简介](http://www.csdn.net/company/about.html)|[招贤纳士](http://www.csdn.net/company/recruit.html)|[广告服务](http://www.csdn.net/company/marketing.html)|[银行汇款帐号](http://www.csdn.net/company/account.html)|[联系方式](http://www.csdn.net/company/contact.html)|[版权声明](http://www.csdn.net/company/statement.html)|[法律顾问](http://www.csdn.net/company/layer.html)|[问题报告](mailto:webmaster@csdn.net)京 ICP 证 070598 号北京创新乐知信息技术有限公司 版权所有![]() 联系邮箱：webmaster(at)csdn.netCopyright © 1999-2012, CSDN.NET, All Rights Reserved [![GongshangLogo]()](http://www.hd315.gov.cn/beian/view.asp?bianhao=010202001032100010)
