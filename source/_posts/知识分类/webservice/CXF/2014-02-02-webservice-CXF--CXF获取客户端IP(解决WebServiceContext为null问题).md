---
layout: post
title: "CXF获取客户端IP(解决WebServiceContext为null问题)"
categories: webservice
tags: 
 - webservice
 - CXF
--- 

# CXF获取客户端IP(解决WebServiceContext为null问题)

网络上很多文章都是这样配的：
1

2
3

4
5

6
7

8
 @Resource

private

WebServiceContext wscontext;
 

public

String getIP(){
        

MessageContext ctx = wscontext.getMessageContext();

        

HttpServletRequest request = (HttpServletRequest)ctx.ge(AbstractHTTPDestination.HTTP_REQUEST);
        

return

request.getRemoteAddr();

}

 

但是在我测试的过程中，发现如果把这段代码写在aop切点中，wscontext就是null，如果写在普通的实现类，就可以正常获取。其实很多帖子也说到null的问题，但最后都没解决。

现在在[这里](http://www.javatips.net/blog/2012/03/getting-ip-address-using-cxf)发现另一种方法，经测试完全有效:
1

2
3 Message message = PhaseInterceptorChain.getCurrentMessage();

HttpServletRequest httprequest = (HttpServletRequest)message.get(AbstractHTTPDestination.HTTP_REQUEST);
return

httprequest.getRemoteAddr();
来源： <[https://linchunyu.info/81/cxf_fetch_request_ip.html](https://linchunyu.info/81/cxf_fetch_request_ip.html)> 
