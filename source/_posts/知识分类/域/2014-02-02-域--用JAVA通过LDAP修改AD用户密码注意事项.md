---
layout: post
title: "用JAVA通过LDAP修改AD用户密码注意事项"
categories: 域
tags: 
 - 域
--- 

# 用JAVA通过LDAP修改AD用户密码注意事项

[![]()](http://woaidong.com/;jsessionid=4C48D8C4E183EC8E371AB028066D4371)[我爱动](http://woaidong.com/archiver/;jsessionid=4C48D8C4E183EC8E371AB028066D4371) » [日志](http://woaidong.com/archiver/method-blogs.shtml;jsessionid=4C48D8C4E183EC8E371AB028066D4371)
特别提醒： 你现在看到的是帖子的纯文字版, [查看完整版本](http://woaidong.com/blog/method-detail-id-2278.shtml;jsessionid=4C48D8C4E183EC8E371AB028066D4371)

2009年1月14日 **用JAVA通过LDAP修改AD用户密码注意事项**

最近要用java来修改windows 2003的Active Directory(简称AD)上的用户，包括新增、修改、删除，普通的操作这里就不说了，网上有一大堆的资料，这里记述一下本人操作过程中遇到的问题及解决方法。
通过ldap来修改AD的用户信息，除了修改密码外，其他的都可以使用非安全的连接进行操作，也就是可以不走SSL连接来操作，注意AD的普通端口是389，SSL端口是636。
当使用SSL连接修改密码时，需要在连接端安装证书，怎么获取证书，本人使用网上其他人介绍的证书方法，全是无法成功，都是出现 unable to find valid certification path to requested target 错误，最终经一朋友提示，使用其他方法获取到正确的证书，具体如下：
1、安装证书服务。最好不要在域控制器上安装证书服务，而是使用另外一台机器加入到域中，且在这台机器上使用域账户来安装证书服务。证书服务的安装没什么特别的注意，请参考其他人的文章。
2、获取客户端证书。别人都是通过下载证书的方式来获取证书，但是我通过这种方式就是无法成功修改密码，也都是提示 unable to find valid certification path to requested target 这个错误。我的操作方法为，用IE通过SSL直接连接ldap服务器(也就是安装AD的那台机器)，使用636端口，类似于 https://192.168.0.111:636 ，连接的时候会提示安装证书，这时候把这个证书保存下来，即为需要的客户端证书。
3、连接。得到证书后，我在连接的时候成功了，但在修改AD用户密码的时候还是报错，但这次的错误为 javax.naming.OperationNotSupportedException: [LDAP: error code 53 - 0000052D: SvcErr: DSID-031A0FC0, problem 5003 (WILL_NOT_PERFORM), data 0 ,操作不支持错误，在这个问题了转了好久都没解决，后来无意中看到一篇文章讲到AD中的密码策略问题，才想起来windows2003中AD中的默认密码策略有长度限制和复杂性要求，所以才导致出现OperationNotSupportedException异常，记得在域控制器上查看自己的域策略。
后记：由于水平原因，就因为证书的问题搞了两天多才搞定，其实通过调整域策略可以使用修改密码不需要走SSL通道的，但这都是旁门左道，希望写下这篇文章对大家有所帮助。
[友情链接](http://woaidong.com/include/about/links.jhtml;jsessionid=4C48D8C4E183EC8E371AB028066D4371 "友情链接")[关于我爱动](http://woaidong.com/include/about/about_us.jhtml;jsessionid=4C48D8C4E183EC8E371AB028066D4371 "关于我们")[加入我爱动](http://woaidong.com/include/about/join.jhtml;jsessionid=4C48D8C4E183EC8E371AB028066D4371 "加入我们")[联系我们](http://woaidong.com/include/about/contact.jhtml;jsessionid=4C48D8C4E183EC8E371AB028066D4371 "联系我们")[免责条款](http://woaidong.com/include/about/mz.jhtml;jsessionid=4C48D8C4E183EC8E371AB028066D4371 "免责声明")  [浙ICP备08105846号](http://www.miibeian.gov.cn/)Copyright ©  [我爱动](http://woaidong.com/;jsessionid=4C48D8C4E183EC8E371AB028066D4371),2008-2009. All Rights Reserved [archiver](http://woaidong.com/archiver/;jsessionid=4C48D8C4E183EC8E371AB028066D4371)
