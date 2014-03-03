---
layout: post
title: "Advanced HTTPClient Info"
categories: Httpclient
tags: 
 - Httpclient
--- 

# Advanced HTTPClient Info

# Advanced HTTPClient Info

## Contents

* [Proxies](http://www.innovation.ch/java/HTTPClient/advanced_info.html#proxies)
* [Timeouts](http://www.innovation.ch/java/HTTPClient/advanced_info.html#timeouts)
* [Output Streams](http://www.innovation.ch/java/HTTPClient/advanced_info.html#out_streams)
* [Contexts](http://www.innovation.ch/java/HTTPClient/advanced_info.html#contexts)
* [Persistent Connections (Keep-Alive's)](http://www.innovation.ch/java/HTTPClient/advanced_info.html#pers_con)
* [Closing Connections](http://www.innovation.ch/java/HTTPClient/advanced_info.html#close_con)
* [Pipelining](http://www.innovation.ch/java/HTTPClient/advanced_info.html#pipelining)
* [Relationship between HTTPConnection instances and TCP connections](http://www.innovation.ch/java/HTTPClient/advanced_info.html#conn_details)
* [Multithreading](http://www.innovation.ch/java/HTTPClient/advanced_info.html#threading)
* [HttpURLConnection](http://www.innovation.ch/java/HTTPClient/advanced_info.html#urlcon)
* [Protocol Version](http://www.innovation.ch/java/HTTPClient/advanced_info.html#prot_vers)
* [Modules](http://www.innovation.ch/java/HTTPClient/advanced_info.html#modules)
* [Java System Properties recognized by HTTPClient](http://www.innovation.ch/java/HTTPClient/advanced_info.html#properties)
* [HTTP Header Fields](http://www.innovation.ch/java/HTTPClient/advanced_info.html#headers)
* [HTTP (and related) Specifications](http://www.innovation.ch/java/HTTPClient/advanced_info.html#specs)

## [Proxies]()

Support for proxies (including SOCKS) is fully implemented. However, using proxies in applets is subject to a number of security restrictions (see [security](http://www.innovation.ch/java/HTTPClient/security.html) for more information on the various security policies and the consequences that arise from them). If you are using an http proxy then use the [HTTPConnection.setProxyServer()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setProxyServer(java.lang.String, int)) method to set the default proxy for all new HTTPConnections; [HTTPConnection.setCurrentProxy()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setCurrentProxy(java.lang.String, int)) can be used to set a proxy for the current HTTPConnection only. You can also manipulate a list of hosts for which no proxy is to be used with the methods [HTTPConnection.dontProxyFor()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#dontProxyFor(java.lang.String)) and [HTTPConnection.doProxyFor()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#doProxyFor(java.lang.String)). Example:
HTTPConnection.setProxyServer("my.proxy.dom", 8008); HTTPConnection.dontProxyFor("localhost"); HTTPConnection.dontProxyFor(".mycompany.com"); AuthorizationInfo.addBasicAuthorization("my.proxy.dom", 8008, realm, user, passwd); ... HTTPConnection con = new HTTPConnection(...);This will cause all connections to use the proxy (and automatically authenticate with it) except when connecting to localhost or any other machine within the company.

If you are using SOCKS then the method to use is [setSocksServer()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setSocksServer(java.lang.String)). Note that both an http proxy and a SOCKS proxy can be set at the same time, in which case a request is sent via the SOCKS server to the proxy server, which in turn relays the request to the desired destination.

Some proxies will proxy for protocols other than http using http to contact the proxy itself. If you have such a proxy then you can use the HTTPClient to do requests for other protocols through the proxy. To do this you need to create an HTTPConnection to the proxy itself (i.e. *don't* use [setCurrentProxy()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setCurrentProxy(java.lang.String, int)) or [setProxyServer()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setProxyServer(java.lang.String, int))) and specify the full URL of the file/article/whatever in the Get(), Put(), etc. Example: if you want to retrieve the file /pub/README via ftp from rtfm.mit.edu then you could use something like:
HTTPConnection proxy = new HTTPConnection("my.proxy.dom", 8000); HTTPResponse resp = proxy.Get("ftp://rtfm.mit.edu/pub/README"); ...

## [Timeouts]()

Sometimes one doesn't want to wait (almost) forever until a connection is established to the server or until the server answers. In this case a timeout can be set using the methods [HTTPConnection.setDefaultTimeout()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setDefaultTimeout(int)) and [HTTPConnection.setTimeout()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setTimeout(int)). Setting a timeout will cause the client to limit the time it will spend trying to get the hosts IP-address and establishing a connection with the server, and it will also set the timeout on the socket while reading the response.

**Note:** if a timeout occurs while reading the response (i.e. an InterruptedIOException is thrown) then doing another

read()
may or may not lose data, depending on what streams have been pushed on the response input stream. In general all HTTPClient streams are restartable, but those from the JDK aren't (e.g GZIPInputStream). This means that if such a stream has been pushed onto the stack (typically when a compression content-coding or transfer-coding was used) and an InterruptedIOException occurs then chances are the response data will be corrupted.

## [Output Streams]()

All request methods which may send data (such as POST and PUT) take either a byte[] or an [HttpOutputStream](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HttpOutputStream.html). Using the latter may be beneficial when sending large amounts of data because it streams the data directly over the socket in many cases. However, restrictions on many servers may prevent the streaming, necessitating buffering of the data by the HttpOutputStream. The class documentation gives the full details, but in summary here are the things to consider: if you know the length of the data beforehand then use the appropriate constructor and you will be able to stream the data to any server. If you do not know the length beforehand then you may encounter problems, in which case you'll need to set the java system property HTTPClient.dontChunkRequests to true, and all data will be buffered by the client and only sent when the stream is close()'d.

Various modules (such as the RedirectionModule and AuthorizationModule) need to resend certain requests in order to do their job. However, if the request used an HttpOutputStream then they cannot do so; in such cases the application will have to handle those statuses itself. In order to partially solve this problem, an application may set the java system property HTTPClient.deferStreamed to true. Requests which need resending (and which used an HttpOutputStream) are then remembered by the modules and the [retry flag](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPResponse.html#retryRequest()) in the response is set to true. The application can then check this flag, and if it's true just blindly resend the request; the modules will detect the resend and apply the appropriate modifications (such as adding the necessary Authorization header field, or changing the request-uri to a new location).

## [Contexts]()

By default all authorization info, cookies, permanent redirection lists, etc. are shared between all instance of HTTPConnection. However, there are cases where one wants to simulate multiple independent clients or users within the same application. To enable this the HTTPClient a supports a concept called contexts. Each HTTPConnection instance has an associated context, which can be any Object at all, and which can be set using ([HTTPConnection.setContext()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setContext(java.lang.Object))). All instances of HTTPConnection which have the same context will share all info on cookies, authorization info, etc. This is done by having each [module](http://www.innovation.ch/java/HTTPClient/advanced_info.html#modules) which keeps information on behalf of the application (such as the cookie module, the authorization module and the redirection module) use a separate list for each context. By setting the context for each HTTPConnection instance appropriately, you can control exactly which instances share data and which don't.

Note that you can't have more than one context per HTTPConnection, i.e. you need at least one separate instance of HTTPConnection for each client.

Example: suppose you want to simulate 3 independent clients, each having two HTTPConnection instances. Then you could do something like:
HTTPConnection cl1_con1 = new HTTPConnection(...); HTTPConnection cl1_con2 = new HTTPConnection(...); cl1_con1.setContext("client1"); cl1_con2.setContext("client1"); HTTPConnection cl2_con1 = new HTTPConnection(...); HTTPConnection cl2_con2 = new HTTPConnection(...); cl2_con1.setContext("client2"); cl2_con2.setContext("client2"); HTTPConnection cl3_con1 = new HTTPConnection(...); HTTPConnection cl3_con2 = new HTTPConnection(...); cl3_con1.setContext("client3"); cl3_con2.setContext("client3");

As mentioned above, the context may be any object at all. So, if in the above example you were going to run each client in a separate thread then you could use the thread itself as the context (e.g.

con.setContext(Thread.currentThread());
). If no context is set a default context is used which is the same for all HTTPConnection's. Therefore applications which don't need this feature can just ignore it.

## [Persistent Connections (Keep-Alive's)]()

The Hypertext Transfer Protocol originally allowed only one request per TCP connection. However, establishing a TCP connection is fairly expensive time wise, so that some implementors of HTTP/1.0 added so called Keep-Alive's to keep a connection open after a request was completed and to allow further requests to be made over that connection. Unfortunately, this was not well defined and is broken in the face of proxies. HTTP/1.1 defines persistent connections correctly and even makes them the default.

The HTTPClient will by default try to keep a connection alive for as many requests as possible, both when talking to HTTP/1.0 and HTTP/1.1 servers. To disable persistent connections you can specify a Connection header with the value close. Example:
NVPair[] def_hdrs = { new NVPair("Connection", "close") }; con.setDefaultHeaders(def_hdrs);

This will disable persistent connections for all future request (unless overridden by a connection header on the request method call).

Keeping the socket connection to the server open after a request is fine as long as another request follows within a short period of time. However when you are done you should let the library know by passing the above

"Connection: close"
header with the last request. Furthermore, to limit the length of time the connection will be held open a timer is started after each request which will close the socket connection if no further requests arrive within the next 60 seconds.

Note that most of this is transparent as far as the functioning of the requests is concerned; the only differences you will notice is in the time required for a request to be sent. Also note that persistent connections are only done within the context of a given instance of HTTPConnection; so if you create two instances both pointing at the same server then they will create separate connections to the server.

## [Closing Connections]()

The HTTPClient will close a socket as soon as it has determined that no more data will arrive on the socket or that any outstanding data is to be discarded. However, this is not always easy to do, and in at least one common case the client will keep a connection open indefinitely unless a simple precaution is taken (see below).

A socket is closed when one of the five following conditions occurs:

* an exception occurs during read or write.
* the connection timeout triggers and no responses are outstanding (note this is *not* the timeout set by [HTTPConnection.setTimeout()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setTimeout(int)), but an internal, hard coded 60 second timeout).
* all response streams on the connection have been closed, up to and including a terminal response.
* [stop()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#stop()) is invoked on the HTTPConnection.
* the stream demultiplexer is finalized.

A "terminal" response is a response for which client has determined that the connection should be closed after it has been fully received. This includes the server sending a

"Connection: close"
(in the case of an HTTP/1.1 server) or not sending a

Connection: keep-alive
(in the case of an HTTP/1.0 server) with the response, the response having no Content-length and no self-delimiting body, or the receipt of certain error status codes.

As can be seen from above, a basic condition for the connection to be closed is that the response streams have been closed. This is necessary because the client can't distinguish between a slow application which takes its time to read the response and an application which is not interested in the response. Failing to close the response streams can lead to a lot of open connections hanging around, which will eventually cause you to run out of file descriptors (on a Unix box - socket descriptors on Windoze). Therefore it is important that you **always close the response stream** of every response. This can be done either by invoking

resp.getData()
or invoking

close()
on the stream from

resp.getInputStream()
(where resp is the HTTPResponse). If you are not interested in the response data you can just do a

resp.getInputStream().close()
.

## [Pipelining]()

If the connection is kept open across requests then the requests may be pipelined. Pipelining here means that a new request is sent before the response to a previous request is received. Since this can obviously enhance performance by reducing the overall round-trip time for a series of requests to the same server, the HTTPClient has been written to support pipelining (at the expense of some extra code to keep track of the outstanding requests).

The programming model is always the same: for every request you send you get a response back which contains the headers and data of the servers response. Now, to support pipelining, the fields in the response aren't necessarily filled in yet when the HTTPResponse object is returned to the caller (i.e. the actual response headers and data haven't been read off the net), but the first call to any method in the response (e.g. a getStatusCode()) will wait till the response has actually been read and parsed. Also any previous requests will be forced to read their responses if they have not already done so (so e.g. if you send two consecutive requests and receive responses r1 and r2, calling

r2.getHeader("Content-type")
will first force the complete response r1 to be read before reading the response r2). All this should be completely transparent, except for the fact that invoking a method on one response may sometimes take a few seconds to complete, while the same method on a different response will return immediately with the desired info.

## [Relationship between HTTPConnection instances and TCP connections]()

The relationship between an HTTPConnection instance and actual TCP connections (instances of java.net.Socket) is as follows: each HTTPConnection instance creates its own sockets, i.e. sockets are not shared between instances. Sockets are created on demand, just before a request is sent (so creating an HTTPConnection instance does *not* immediately cause a TCP connection to be opened). A new socket is only created if either the current socket (if any) has been closed, or the previous request/response over it has determined that it will be closed at the end that response.

Some implications of the above are that 1) applications can control the number of parallel TCP connections to some degree by creating multiple instances of HTTPConnection pointing to the same server, but 2) that control is not absolute: if the server does not support keep-alive's and the application sends requests before having the read the response to the previous request, then you may end up with multiple parallel TCP connections from the same HTTPConnection instance.

## [Multithreading]()

The HTTPClient is completely multithread safe (or at least should be ;-). A single HTTPConnection instance may be shared between multiple threads, multiple requests may be run concurrently on a single or multiple HTTPConnection instances, and you may even have multiple threads reading on a single response (though I'm not sure that the latter makes much sense). Because a number of parameters are on a per HTTPConnection instance basis (such as the module list, proxy settings, default headers, etc), care should be taken when modifying these parameters if the HTTPConnection instance is shared among multiple threads.

## [HttpURLConnection]()

The [HttpURLConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HttpURLConnection.html) class provides a java.net.HttpURLConnection compatible interface to the HTTPClient. It exists mainly to allow the HTTPClient to be used as a direct plug-in replacement for Sun's HttpClient (part of the JDK); if you are writing HTTPClient specific code, then I recommend using the HTTPClient's "native" API, i.e. HTTPConnection et. al.

The HttpURLConnection follows the model of java.net.URLConnection and java.net.HttpURLConnection in that each instance is only capable of doing a single request (i.e. a new instance must be created for each request, usually via

URL.openConnection()
or

URL.openStream()
). To enable reuse of connections, HttpURLConnection manages a pool of HTTPConnection's internally. Different instances of HttpURLConnection will use the same HTTPConnection if they have the same host and port. This way socket connections are reused as much as possible (i.e. you still get to take advantage of the persistent connections capability). In addition, [connect()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HttpURLConnection.html#connect()) will only send the request but not read or parse the response; only when a method such as

getInputStream()
,

getResponseCode()
, etc, is invoked is the response read and parsed. This, together with the reuse of HTTPConnection's, allows requests to be pipelined by explicitly invoking

connect()
. Example:
URLConnection con1 = url1.openConnection(); con1.connect(); // sends first request URLConnection con2 = url2.openConnection(); con2.connect(); // sends second request InputStream is1 = con1.getInputStream(); // waits for first response InputStream is2 = con2.getInputStream(); // waits for second response

## [Protocol Version]()

The request protocol version sent is always HTTP/1.1, except in a few circumstances when a broken server is encountered, in which case the version sent reverts to HTTP/1.0. An HTTP/0.9 request is never sent.

The protocol version returned by the server is used to select between different mechanisms for persistent connections. If the server advertises itself as being HTTP/1.1 compliant then HTTP/1.1 persistent connections are used; otherwise HTTP/1.0 keep-alive's are used (the difference is the tokens used in the Connection header for signaling persistence and the end of a connection). Apart from this, the only other distinction made between talking to an HTTP/1.0 or an HTTP/1.1 server is in how request are automatically retried (retried requests with a body need to use slightly different mechanisms for determining how long to wait after sending the headers, before sending the body).

## [Modules]()

A number of functions of the HTTPClient, such as authorization and redirection handling, are broken out into [modules](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPClientModule.html). Modules can be dynamically added and removed, thereby allowing certain functionality to be disabled or new functionality to be added without having to modify any of the core code. Each connection has a list of modules it will use to process a request and response. When a request method is invoked (such as

Get()
or

Post()
) the request is first assembled into a [Request](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/Request.html) instance. Then the [request handler](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPClientModule.html#requestHandler(HTTPClient.Request,HTTPClient.Response[])) of each module is invoked in turn with this request. This handler may modify the request (such as add headers) or even generate a response directly (such as a cache might do). Only after all handlers have been invoked (and none of them generated a response) is the request actually sent over the wire. Similarly, when a response is read the [response handlers](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPClientModule.html#responsePhase1Handler(HTTPClient.Response,HTTPClient.RoRequest)) in each module are invoked in turn. They may do certain things based on the status code (such as the redirection module) or the headers (such as the cookie module), modify the response, or even generate a new request (such as in the redirection and authentication modules). If a new request is generated the process starts from the top again.

The currently supplied modules are the AuthorizationModule, the RedirectionModule, the ContentEncodingModule, the TransferEncodingModule, the CookieModule, the ContentMD5Module, the RetryModule and the DefaultModule. These are explained in more detail further down.

Modules can be dynamically added and removed to tailor the request and response processing desired. The methods [HTTPConnection.addDefaultModule()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#addDefaultModule(java.lang.Class, int)), [HTTPConnection.removeDefaultModule()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#removeDefaultModule(java.lang.Class)) and [HTTPConnection.getDefaultModules()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#getDefaultModules()) manipulate and return the list of default modules which is used when a new [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html) is created. Similarly, the methods [HTTPConnection.addModule()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#addModule(java.lang.Class, int)), [HTTPConnection.removeModule()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#removeModule(java.lang.Class)) and [HTTPConnection.getModules()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#getModules()) manipulate and return the list of current modules for an HTTPConnection instance. Note that for the modules which are not public you must get their Class objects via Class.forName() (e.g. to remove the redirection module, use

con.removeModule(Class.forName("HTTPClient.RedirectionModule"))
).

The default list of modules is initialized from the java system property HTTPClient.Modules. This property must be a "|" separated list of class names. If this property is not set it defaults to all the classes listed below. Normally if during class initialization any module in the list does not exist or cannot be instantiated then an Error is thrown. However, if this is being used in an unsigned applet then the error is suppressed. This way applets can limit the modules loaded over the net by simply not providing them (remember, they can't set system properties due to security restrictions).

You may create your own modules and add them using the above methods. Any module you write must implement the [HTTPClientModule](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPClientModule.html) interface. See the API docs more info. **Note:** this interface may change. If you write a module and find the interface insufficient or difficult, please contact me. Also, if you write a module which you think might be of general usefulness and would like to make it freely available, let me know.

Here is a short description of each module.

### [AuthorizationModule]()

Authorization briefly described in [Getting Started](http://www.innovation.ch/java/HTTPClient/getting_started.html#auth). As mentioned, this module will handle both server and proxy authorization requests (status codes 401 and 407). In addition to the 'Basic' and 'Digest' authorization schemes, the AuthorizationModule can be made to handle other schemes as well, so long as they are not "too exotic" (i.e. they follow the simple challenge-response mechanism outlined in the http specs); this is done by setting your own [AuthorizationHandler](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/AuthorizationHandler.html). Of course, if you need to something more sophisticated you can always plug in your own authorization module.

When confronted with an authorization request the authorization module will query all known authorization info for a possible candidate (the match must be for the host, port, scheme and realm). If no suitable info is found, or if the server rejects any info found, an authorization handler is called to try and get the necessary info from the user; if the user does not give any information, or if the information she gives is also rejected, then the retrying is terminated and the last failure status returned to the caller. The default handler currently only understands requests for the 'Basic' and 'Digest' authorization schemes; you may however set your own handler via the [AuthorizationInfo.setAuthHandler()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/AuthorizationInfo.html#setAuthHandler(HTTPClient.AuthorizationHandler)) method. The handler given must implement the [AuthorizationHandler](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/AuthorizationHandler.html) interface. To disable the handler completely give null for the handler.

A server (or proxy) may send multiple authorization challenges in the response, in which case the above algorithm is modified to go through the list of challenges in the same order as they were sent, trying to get authorization info for each challenge in the list and going to the next challenge if either no info was found or the server rejects that info. If the end of the list is reached without achieving authorization then the authorization handler is called on each challenge (in the same order) until either an authorization request is successful, the authorization handler returns null (e.g. when the Cancel button in the default popup box is activated) or the list is exhausted, in which case the response to the last failed request is returned.

Whenever the [default authorization handler](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/DefaultAuthHandler.html) needs to ask the user for a username and password, it queries the current [AuthorizationPrompter](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/AuthorizationPrompter.html). This is responsible for popping up a dialog box or otherwise acquiring the necessary info. A default prompter is provided which will pop up a simple dialog box, if the AWT is running, or use a command line prompt if it isn't and the currently running system is Unix or VMS; you may set your own prompter using [DefaultAuthHandler.setAuthorizationPrompter()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/DefaultAuthHandler.html#setAuthorizationPrompter(HTTPClient.AuthorizationPrompter)). To prevent the prompter from being queried (e.g. to prevent the popup box from appearing) you can either set the prompter to null, or use [con.setAllowUserInteraction(false)](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setAllowUserInteraction(boolean)).

### [RedirectionModule]()

This module handles the redirection status codes 301, 302, 303, 305 and 307. 301 and 307 responses are only redirected if the request method was GET or HEAD; this is because redirecting, say, a POST blindly might lead to undesired behaviour, as the circumstances leading to the POST might have changed. 302 and 303 are treated identically: the new request to the new location is done using GET (this is what many cgi's expect - they are basically directing you to a prefabricated answer). 305 is only honored if the connection is not already using a proxy.

This module also keeps a list of permanently redirected URL's (status code 301) and will preemptively redirect requests for these. This list is volatile (i.e. it will be lost when the application exits).

### [CookieModule](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/CookieModule.html)

This module implements cookies as defined by [Netscape's cookie spec](http://home.netscape.com/newsref/std/cookie_spec.html) and the latest [HTTP State Management Mechanism](http://www.ietf.org/rfc/rfc2965.txt) spec. During response processing it intercepts and parses the Set-Cookie and Set-Cookie2 header fields (removing them from the response in the process), and during request processing it adds all eligible cookies to the requests.

Because of privacy issues surrounding cookies, whenever the server tries to set a cookie a [policy handler](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/CookiePolicyHandler.html) is invoked to see whether this cookie should be accepted. The default handler brings up a popup describing the cookie and allowing the user to accept or reject it; the user may also summarily accept or reject cookies from whole domains. You may substitute your own policy handler using the [setCookiePolicyHandler()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/CookieModule.html#setCookiePolicyHandler(HTTPClient.CookiePolicyHandler)) method. If you set the handler to null then all cookies will be silently accepted. If you do not want any cookies to be accepted then either remove the CookieModule from the list of modules or set your own handler which always returns false. The handler must implement the [CookiePolicyHandler](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/CookiePolicyHandler.html) interface.

The CookieModule provides methods to view the currently stored cookies, and to add and remove cookies from that list. If you want to manually provide additional cookies for a request then you must add them to the CookieModule's internal list - any Cookie header fields you create yourself and pass to a request will be removed by the CookieModule.

The CookieModule will save and restore cookies at the start and end of the application only if the java system property HTTPClient.cookies.save is set to true. All cookies which are meant to persist across invocations are then saved to a file when the application exits and read from that file when the CookieModule is loaded. The format of the file is a serialized Hashtable of Cookies. The name of file can controlled through the system property HTTPClient.cookies.jar. If this is not set, a system dependent name is used:

* Unix: $HOME/.httpclient_cookies
* Mac: System Folder:Preferences:HTTPClientCookies
* OS/2: $HOME/.httpclient_cookies
* Win95: $JAVA_HOME/.httpclient_cookies
* WinNT: $HOME/.httpclient_cookies
(where $HOME is the value of the java system property "user.home", and $JAVA_HOME is the value of the java system property "java.home").

### [ContentEncodingModule]()

Servers may apply various content encodings to the content. The most widely used encodings are compressions: gzip, compress, and deflate. This module will handle these three content encodings by pushing an appropriate decoding stream; this means that the data read from [getInputStream()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPResponse.html#getInputStream()) will be the clear text. The Content-Encoding header is also modified appropriately.

You might want to consider disabling this module if you are using the HTTPClient for things like web-copying where storing the compressed document makes sense.

### [TransferEncodingModule]()

This is similar to the ContentEncoding module except that it applies to transfer encodings. It also handles the gzip, compress, and deflate encodings by pushing an appropriate decoding stream onto the response input stream and modifies the Transfer-Encoding header appropriately.

### [ContentMD5Module]()

Some servers may generate a Content-MD5 header which contains an MD5 hash of the message body (after any content encoding, but before any transport encoding is applied). If this header is present this module will push a stream which calculates the MD5 hash of the body. When the stream is closed or the end of the data is reached the calculated hash is compared to the one in the Content-MD5 header and if they don't match an IOException is thrown.

### [DefaultModule]()

This handles the response status codes 408 (request timeout) and 411 (length required).

### [RetryModule]()

This module is special. It is responsible for automatically retrying requests which were aborted due to an IOException on the socket. It is unlike other modules in that it is closely tied in to the core code, instead of just manipulating the request and response structures as other modules do. The code in this module could have been put in with the rest of the core code, but moving it to a module has the advantage that this automatic retrying of requests may be disabled using the standard mechanism of removing modules.

### Ordering the Modules

The handlers in the modules are invoked in the order the modules are placed in the list. Because of certain constraints between modules this order is important. The default order for the supplied modules is:

1. RetryModule
1. CookieModule
1. RedirectionModule
1. AuthorizationModule
1. DefaultModule
1. TransferEncodingModule
1. ContentMD5Module
1. ContentEncodingModule
However the constraints impose only a partial ordering, so that the above order may be changed as long as the following restrictions are observed:

* The RetryModule *must* be first. It catches a special RetryException, which is thrown by the core code. If it were not the first module it would not be able to catch this exception.
* The TransferEncoding module must precede any module which handles the response message (such as the ContentEncoding module and the ContentMD5Module). There are a number of reasons for this, but basically it's because all the headers refer to the message before transport encoding is applied.
* The ContentMD5 module must precede the ContentEncoding module, as the hash is calculated for the encoded message.

## [Java System Properties recognized by HTTPClient]()

There are a number of java system properties which are used by the HTTPClient. Most are documented somewhere in the api docs. Some of the properties may contain a list of elements, in which case the elements are separated by vertical bars ("|"). White space is ignored, except that a "| |" produces an empty element whereas "||" is treated like a single delimiter (i.e. "|").

The properties are read once, when the class that uses them is loaded (i.e. they're read in the class' static initializer). This means they need to be set either on the command line via the -D option, as in
java -DHTTPClient.dontChunkRequests=true MyAppor early in your application before the class is first accessed: System.getProperties().put("HTTPClient.dontChunkRequests", "true"); ... HTTPConnection.setDefaultHeaders(...);

Here is summary of all properties recognized:

http.proxyHostRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Used to specify the http proxy to use. See [setProxyServer()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setProxyServer(java.lang.String, int)) for more info. This is the same property that is used by Sun's JDK 1.1 (and later).http.proxyPortRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Used to specify the http proxy to use. See [setProxyServer()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setProxyServer(java.lang.String, int)) for more info. This is the same property that is used by Sun's JDK 1.1 (and later).proxySetRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Obsolete. Used by Sun's JDK 1.0.2. If http.proxyHost is not set and proxySet is true, then the default proxy is set using the values in proxyHost and proxyPort.proxyHostRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Obsolete. Used by Sun's JDK 1.0.2. If http.proxyHost is not set and proxySet is true, then the default proxy is set using the values in proxyHost and proxyPort.proxyPortRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Obsolete. Used by Sun's JDK 1.0.2. If http.proxyHost is not set and proxySet is true, then the default proxy is set using the values in proxyHost and proxyPort.HTTPClient.nonProxyHostsRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Used to specify a list hosts for which no http proxy is to be used. See [dontProxyFor()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#dontProxyFor(java.lang.String)) for more info.http.nonProxyHostsRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Used to specify a list hosts for which no http proxy is to be used. See [dontProxyFor()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#dontProxyFor(java.lang.String)) for more info. This is the same property that is used by Sun's JDK 1.1 (and later).HTTPClient.socksHostRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Used to specify the SOCKS proxy host. See [setSocksServer()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setSocksServer(java.lang.String, int, int)) for more info.HTTPClient.socksPortRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Used to specify the SOCKS proxy port. See [setSocksServer()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setSocksServer(java.lang.String, int, int)) for more info.HTTPClient.socksVersionRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Used to specify the SOCKS proxy version. See [setSocksServer()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#setSocksServer(java.lang.String, int, int)) for more info.HTTPClient.ModulesRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Used to define the default list of modules. See [HTTPConnection.addDefaultModule()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html#addDefaultModule(java.lang.Class, int)) for more info.HTTPClient.disable_pipeliningRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Used to disable all pipelining. This should never be needed, but you may encounter a server which displays problems when pipelining requests. Setting this property to true will cause the HTTPClient to stall each request until the headers from the response to the previous request have been received and parsed.HTTPClient.disableKeepAlivesRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Used to disable keep-alive's. This exactly equivalent to (and is implemented as)

con.setDefaultHeaders(new NVPair[] { new NVPair("Connection", "close") })
.HTTPClient.forceHTTP_1.0Read by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Used to force the client to pretend it's only capable of HTTP/1.0. Setting this to true causes all requests to be sent as if the server could only handle HTTP/1.0, and causes the HTTP-version in the request-line to be sent as HTTP/1.0.HTTPClient.dontChunkRequestsRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). Used to prevent the client from using the chunked transfer encoding on requests. Setting this to true causes all data written to an HttpOutputStream (that was not constructed with a content length) to be buffered and only sent when the stream is closed.HTTPClient.deferStreamedRead by [HTTPConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPConnection.html). When a request is sent which uses an [HttpOutputStream](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HttpOutputStream.html), and handling of the response by modules would require the module to resend the request, then modules have to punt because they do not have the request data (i.e. the data written to the output stream). In order to make things easier on the application, an application can set this property to true and then check the [retry flag](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HTTPResponse.html#retryRequest()) in the response for whether it needs to resend the request. If the value is false, the retry flag will always be false. The default is false in order to prevent memory leaks in applications which aren't prepared for it.HTTPClient.dontTimeoutRespBodyRead by RespInputStream. Used to restore pre-V0.3-3 behaviour of disabling any timeout while reading the response body. Setting this to true causes read()'s on the response input stream to block indefinitely until some data arrives or the connection closes, irrespective of whether a timeout has been set or not.HTTPClient.cookies.hosts.acceptRead by [CookieModule](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/CookieModule.html). Used to initialize the list of hosts and domains from which to always accept cookies. See [setCookiePolicyHandler()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/CookieModule.html#setCookiePolicyHandler(HTTPClient.CookiePolicyHandler)) for more info.HTTPClient.cookies.hosts.rejectRead by [CookieModule](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/CookieModule.html). Used to initialize the list of hosts and domains from which to always reject cookies. See [setCookiePolicyHandler()](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/CookieModule.html#setCookiePolicyHandler(HTTPClient.CookiePolicyHandler)) for more info.HTTPClient.cookies.saveRead by [CookieModule](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/CookieModule.html). If set to true then the CookieModule will read stored cookie at startup time, and save cookies that should persist at application exit time.HTTPClient.cookies.jarRead by [CookieModule](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/CookieModule.html). This specifies the cookie jar to use for reading and saving cookies. If not set, a system dependent name is used.HTTPClient.log.fileRead by [Log](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/Log.html). If defined, this specifies the name of the file to which log messages are to be written. By default log messages are written to System.err.HTTPClient.log.maskRead by [Log](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/Log.html). If defined, this specifies which debug messages should be logged. The value is a number which is the bitwise OR ('|') of the values of the facilities for which logging should be enabled. E.g. the value -1 enables logging for all facilities. By default logging is disabled.HTTPClient.HttpURLConnection.AllowUIRead by [HttpURLConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HttpURLConnection.html). If set to true then set the defaultAllowUserInteraction to true. By default this is set to false in java.net.URLConnection.http.agentRead by [HttpURLConnection](http://www.innovation.ch/java/HTTPClient/api/HTTPClient/HttpURLConnection.html). If set then the "User-Agent" header is set to this property's value.

## [HTTP Header Fields]()

All request methods accept optional header fields to be sent with the request. Here are a list of possible request and response header fields as defined in the HTTP/1.1 spec. I have added some comments to some of them, but for further info I recommend getting the [specs](http://www.innovation.ch/java/HTTPClient/advanced_info.html#specs) (every header field is described in a paragraph of its own in the spec, so you can read just the part that interests you and ignore the rest).

### Request Header Fields

* Cache-Control
* Connection - used for persistent connections; need not be set by applications
* Date - use only in PUT/POST; even then optional
* Keep-Alive - used for persistent connections (HTTP/1.0 only); set by the HTTPClient as needed.
* Pragma
* Transfer-Encoding - set by the client as necessary
* Upgrade
* Via

* Accept
* Accept-Charset
* Accept-Encoding - set by the ContentEncoding module
* Accept-Language
* Authorization - generated by HTTPClient as necessary
* From
* Host - set by HTTPClient; may be overridden by application
* If-Modified-Since - the server returns 304 if the document was not modified
* If-Match - the server returns 412 if the match fails
* If-NoneMatch - the server returns 304 if the match succeeds
* If-Range
* If-Unmodified-Since - the server returns 412 if modified
* Max-Forwards - used with TRACE and OPTIONS
* Proxy-Authorization - generated by HTTPClient as necessary
* Range
* Referer
* User-Agent - generated by HTTPClient; can be modified

* Allow - only with PUT
* Content-Base
* Content-Encoding - examples: gzip, compress, deflate
* Content-Language
* Content-Length - set by HTTPClient
* Content-MD5
* Content-Range
* Content-Type

### Response Header Fields

* Cache-Control
* Connection - used for persistent connections
* Date - date that the document was delivered (**not** when it was last modified)
* Keep-Alive - used for persistent connections (HTTP/1.0 only)
* Pragma
* Transfer-Encoding - HTTPClient handles chunked in the core code; gzip and deflate are handled by the TransferEncoding module.
* Upgrade - sent with a 101 response;
* Via - carries a list of proxies that were involved in returning the request; especially interesting for TRACE and OPTIONS requests.

* Age - from caches
* Location - sent with a redirection status code; since HTTPClient automatically handles 301, 302, 303, 305 and 307 status codes this field is seldom seen.
* Proxy-Authenticate - read by HTTPClient
* Public
* Retry-After - optionally sent with a 503 status
* Vary
* Warning
* WWW-Authenticate - read by HTTPClient

* Accept-Ranges
* Allow - with a 405 status
* Content-Base
* Content-Encoding - gzip, deflate, and compress are handled by the ContentEncoding module.
* Content-Language
* Content-Length
* Content-Location
* Content-MD5 - handled by the ContentMD5 module.
* Content-Range
* Content-Type
* ETag
* Expires - the date when the document expires
* Last-Modified - the date the document was last modified

# [Further Reading]()

![*]() [General HTTP Info at W3C](http://www.w3.org/Protocols/)![*]() [HTTP/1.0 Spec (RFC 1945)](http://www.ietf.org/rfc/rfc1945.txt)![*]() [HTTP/1.1 Spec (RFC 2616)](http://www.ietf.org/rfc/rfc2616.txt)![*]() [Digest Authentication Spec (RFC 2617)](http://www.ietf.org/rfc/rfc2617.txt)

[![[HTTPClient]]()](http://www.innovation.ch/java/HTTPClient/index.html)

Ronald Tschalär / 6. May 2001 / [ronald@innovation.ch](mailto:ronald@innovation.ch).
