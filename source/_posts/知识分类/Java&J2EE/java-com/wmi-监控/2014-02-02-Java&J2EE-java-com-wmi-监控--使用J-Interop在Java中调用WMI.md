---
layout: post
title: "使用 J-Interop 在 Java 中调用WMI"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - java-com
 - wmi-监控
--- 

# 使用 J-Interop 在 Java 中调用WMI

[我的空间](http://simpleframework.net/space.html) ![]()|[邀请](http://simpleframework.net/invite.html) |[登录]()|[注册](http://simpleframework.net/regist.html) [ ](http://simpleframework.net/)  简单就是最好的，java web 开发，simple 让你化繁为简 !

[首页](http://simpleframework.net/index.html)[]()[新闻资讯](http://simpleframework.net/news.html)[]()[博客](http://simpleframework.net/blog.html)[]()[论坛](http://simpleframework.net/bbs.html)[]()[Simple市场](http://simpleframework.net/market.html)[]()[全面介绍](http://simpleframework.net/doc/d2.html)[]()[simple3演示](http://demo.simpleframework.net/simple3)[]()[simple4演示](http://simple4.simpleframework.net/simple4)[]()[My Portal](http://simpleframework.net/home.html)

![]()[SimpleFramework开发者必读](http://simpleframework.net/news/v/27953.html)

[![]()](http://simpleframework.net/space/176.html)

[赵老师的博客](http://simpleframework.net/blog/176.html)

* [全部](http://simpleframework.net/blog.html)
* [推荐](http://simpleframework.net/blog.html?t=recommended)
* [关注](http://simpleframework.net/$resource/default/login/jsp/location.jsp)
* [我的博客](http://simpleframework.net/$resource/default/login/jsp/location.jsp)

[关注]() ([1]())

[博客](http://simpleframework.net/blog.html)»[赵老师](http://simpleframework.net/blog/176.html)»显示全文

使用 J-Interop 在 Java 中调用WMI
5评/3355阅

发表于: 2011-06-16 07:12

**有关****WMI****的小知识**

[**Windows****管理规范（****WMI****）**](http://msdn.microsoft.com/en-us/library/aa394582(VS.85).aspx)是微软对来自[分布式管理任务组](http://en.wikipedia.org/wiki/Distributed_Management_Task_Force "Distributed Management Task Force")（**DMTF**）的[基于Web的企业管理](http://en.wikipedia.org/wiki/Web-Based_Enterprise_Management "Web-Based Enterprise Management")（WBEM）和[通用信息模型](http://en.wikipedia.org/wiki/Common_Information_Model_(computing) "Common Information Model (computing)")（CIM）标准的实现。WMI用于访问Windows系统、应用、网络、设备等组件，并管理它们。连接到一台机器通过**DCOM**进行管理。因此，有关**DCOM**的小知识将有助于本文的理解。你可以到MSDN了解有关WMI的更多细节。

### **J-Interop**

市场上有一些在使用 JAVA 调用 WMI 的好库，包括 [J-Interop](http://www.j-interop.org/index.html)、[JACOB-Project](http://sourceforge.net/projects/jacob-project/) 和 [J-Integra](http://j-integra.intrinsyc.com/products/com/)。其中，我更喜欢J-Interop，因为它是完全免费和开源的API。它提供了没有任何依赖的纯DCOM桥，完全用Java编写的没有任何JNI代码。

### **使用****WMI****管理****Windows****服务**

现在，来看一个使用JAVA调用WMI的例子。这个例子利用J-Interop的API使用Win32_Service类解释WMI操作，将启动和停止在这个例子中的窗口服务。

### **步骤****1****：连接到****WBEM****服务**

下面的代码示例显示了使用J-Interop如何初始化DCOM会话，并连接到远程DCOM服务使。它使用SWbemLocator对象连接到**SWbemServices**，**SWbemServices**对象提供对本地或远程计算机WMI的访问，它调用“ConnectServer”方法连接到**SWbemServices****。在本例中，*提供管理员级别的用户连接到远程计算机。***
JISessiondcomSession=JISession.createSession(domainName,userName,password); dcomSession.useSessionSecurity(false);   JIComServercomServer=newJIComServer(valueOf("WbemScripting.SWbemLocator"),hostIP,dcomSession); IJIDispatchwbemLocator=(IJIDispatch)narrowObject(comServer.createInstance().queryInterface(IID)); //parameterstoconnecttoWbemScripting.SWbemLocator Object[]params=newObject[]{                           newJIString(hostIP),//strServer                           newJIString(win32_namespace),//strNamespace                           JIVariant.OPTIONAL_PARAM(),//strUser                           JIVariant.OPTIONAL_PARAM(),//strPassword                           JIVariant.OPTIONAL_PARAM(),//strLocale                           JIVariant.OPTIONAL_PARAM(),//strAuthority                           newInteger(0),//iSecurityFlags                           JIVariant.OPTIONAL_PARAM()//objwbemNamedValueSet                           };   JIVariantresults[]=wbemLocator.callMethodA("ConnectServer",params); IJIDispatchwbemServices=(IJIDispatch)narrowObject(results[0].getObjectAsComObject());

（**domainName**=远程计算机域名，**hostIP**=远程计算机IP地址,**用户名**=管理员级别的用户，**密码**=密码）

### **第****2****步：获取****Win32_Service****实例**

一旦你获得对**SWbemServices**对象的引用，就可以调用这个类的任何方法。其中**WbemServices.InstancesOf****方法获得任何****Win32****类的实例。**

也可以使用WMI查询语言(WQL)达到同样的目的，如下所示：
finalintRETURN_IMMEDIATE=0x10; finalintFORWARD_ONLY=0x20; Object[]params=newObject[]{ newJIString("SELECT*FROMWin32_Service"), JIVariant.OPTIONAL_PARAM(), newJIVariant(newInteger(RETURN_IMMEDIATE+FORWARD_ONLY)) }; JIVariant[]servicesSet=wbemServices.callMethodA("ExecQuery",params); IJIDispatchwbemObjectSet=(IJIDispatch)narrowObject(servicesSet[0].getObjectAsComObject());

### **第三步：执行方法**

现在，已得到**Win32_Service**类的实例，可采用下述代码来调用同一类的方法，因为，它返回多个服务实例，要列举它们以便获取IJIDispatcher服务。
JIVariant newEnumvariant = wbemObjectSet.get("_NewEnum"); IJIComObject enumComObject = newEnumvariant.getObjectAsComObject(); IJIEnumVariant enumVariant = (IJIEnumVariant) narrowObject(enumComObject.queryInterface(IJIEnumVariant.IID));   Object[] elements = enumVariant.next(1); JIArray aJIArray = (JIArray) elements[0];   JIVariant[] array = (JIVariant[]) aJIArray.getArrayInstance(); for (JIVariant variant : array) {     IJIDispatch wbemObjectDispatch = (IJIDispatch) narrowObject(variant.getObjectAsComObject());       JIVariant returnStatus = wbemObjectDispatch.callMethodA("StopService");       System.out.println(returnStatus.getObjectAsInt()); }

现在，下面的代码显示了一个使用WMI启动和停止Windows服务的完整Java类。

packagecom.wmi.windows;   importstaticorg.jinterop.dcom.core.JIProgId.valueOf; importstaticorg.jinterop.dcom.impls.JIObjectFactory.narrowObject; importstaticorg.jinterop.dcom.impls.automation.IJIDispatch.IID; importjava.util.logging.Level; importorg.jinterop.dcom.common.JIException; importorg.jinterop.dcom.common.JIRuntimeException; importorg.jinterop.dcom.common.JISystem; importorg.jinterop.dcom.core.IJIComObject; importorg.jinterop.dcom.core.JIArray; importorg.jinterop.dcom.core.JIComServer; importorg.jinterop.dcom.core.JISession; importorg.jinterop.dcom.core.JIString; importorg.jinterop.dcom.core.JIVariant; importorg.jinterop.dcom.impls.automation.IJIDispatch; importorg.jinterop.dcom.impls.automation.IJIEnumVariant;   publicclassServiceManager{            privatestaticStringdomainName="";          privatestaticStringuserName="administrator";          privatestaticStringpassword="";          privatestaticStringhostIP="127.0.0.1";          privatestaticfinalStringwin32_namespace="ROOT\\CIMV2";            privatestaticfinalintSTOP_SERVICE=0;          privatestaticfinalintSTART_SERVICE=1;            privateJISessiondcomSession=null;          /**          *          *@paramargs          */          publicstaticvoidmain(String[]args){                  ServiceManagermanager=newServiceManager();                   manager.stopService(domainName,hostIP,userName,password,"MySql");//stopsaservicenamedMySql          }          /**          *Startsagivenserviceifitsstopped.          *          *@paramdomainName          *@paramhostname          *@paramusername          *@parampassword          *@paramserviceName          */    publicvoidstartService(StringdomainName,Stringhostname,Stringusername,Stringpassword,StringserviceName){                  execute(domainName,hostname,username,password,serviceName,START_SERVICE);          }          /**          *Stopsagivenserviceifitsrunning.          *          *@paramdomainName          *@paramhostname          *@paramusername          *@parampassword          *@paramserviceName          */     publicvoidstopService(StringdomainName,Stringhostname,Stringusername,Stringpassword,StringserviceName){                  execute(domainName,hostname,username,password,serviceName,STOP_SERVICE);          }          /**          *Starts/Stopsagivenserviceofremotemachineashostname.          *          *@paramdomainName          *@paramhostname          *@paramusername          *@parampassword          *@paramserviceName          *@paramaction          */ publicvoidexecute(StringdomainName,Stringhostname,Stringusername,Stringpassword,StringserviceName,intaction){                    try{                           IJIDispatchwbemServices=createCOMServer();                             finalintRETURN_IMMEDIATE=0x10;                           finalintFORWARD_ONLY=0x20;                           Object[]params=newObject[]{                                             newJIString("SELECT*FROMWin32_ServiceWHEREName='"+serviceName+"'"),                                             JIVariant.OPTIONAL_PARAM(),                                             newJIVariant(newInteger(RETURN_IMMEDIATE+FORWARD_ONLY))                           };                           JIVariant[]servicesSet=wbemServices.callMethodA("ExecQuery",params);                           IJIDispatchwbemObjectSet=(IJIDispatch)narrowObject(servicesSet[0].getObjectAsComObject());                             JIVariantnewEnumvariant=wbemObjectSet.get("_NewEnum");                           IJIComObjectenumComObject=newEnumvariant.getObjectAsComObject();                    IJIEnumVariantenumVariant=(IJIEnumVariant)narrowObject(enumComObject.queryInterface(IJIEnumVariant.IID));                             Object[]elements=enumVariant.next(1);                           JIArrayaJIArray=(JIArray)elements[0];                             JIVariant[]array=(JIVariant[])aJIArray.getArrayInstance();                           for(JIVariantvariant:array){                                    IJIDispatchwbemObjectDispatch=(IJIDispatch)narrowObject(variant.getObjectAsComObject());                                      //Printobjectastext.                                    JIVariant[]v=wbemObjectDispatch.callMethodA("GetObjectText_",newObject[]{1});                                    System.out.println(v[0].getObjectAsString().getString());                                      //StartorStoptheservice                                    StringmethodToInvoke=(action==START_SERVICE)?"StartService":"StopService";                                    JIVariantreturnStatus=wbemObjectDispatch.callMethodA(methodToInvoke);                           System.out.println("Returnstatus:"+returnStatus.getObjectAsInt());//ifreturncode=0success.Seemsdnformoredetailsaboutthemethod.                           }                  }catch(Exceptione){                           e.printStackTrace();                  }finally{                           if(dcomSession!=null){                                    try{                                             JISession.destroySession(dcomSession);                                    }catch(Exceptionex){                                             ex.printStackTrace();                                    }                           }                  }          }          /**          *InitializeDCOMsessionandconnecttoSWBEMserviceongivenhostmachine.          *@return          */          privateIJIDispatchcreateCOMServer(){                  JIComServercomServer;                  try{                           JISystem.getLogger().setLevel(Level.WARNING);                           JISystem.setAutoRegisteration(true);                           dcomSession=JISession.createSession(domainName,userName,password);                           dcomSession.useSessionSecurity(false);                           comServer=newJIComServer(valueOf("WbemScripting.SWbemLocator"),hostIP,dcomSession);                             IJIDispatchwbemLocator=(IJIDispatch)narrowObject(comServer.createInstance().queryInterface(IID));                           //parameterstoconnecttoWbemScripting.SWbemLocator                           Object[]params=newObject[]{                                             newJIString(hostIP),//strServer                                             newJIString(win32_namespace),//strNamespace                                             JIVariant.OPTIONAL_PARAM(),//strUser                                             JIVariant.OPTIONAL_PARAM(),//strPassword                                             JIVariant.OPTIONAL_PARAM(),//strLocale                                             JIVariant.OPTIONAL_PARAM(),//strAuthority                                             newInteger(0),//iSecurityFlags                                             JIVariant.OPTIONAL_PARAM()//objwbemNamedValueSet                           };                           JIVariantresults[]=wbemLocator.callMethodA("ConnectServer",params);                           IJIDispatchwbemServices=(IJIDispatch)narrowObject(results[0].getObjectAsComObject());                           returnwbemServices;                  }catch(JIExceptionjie){                           System.out.println(jie.getMessage());                           jie.printStackTrace();                  }catch(JIRuntimeExceptionjire){                           jire.printStackTrace();                  }catch(Exceptione){                           e.printStackTrace();                  }finally{                           if(dcomSession!=null){                                    try{                                             JISession.destroySession(dcomSession);                                    }catch(JIExceptione){                                             e.printStackTrace();                                    }                           }                  }                  returnnull;          } }

【原文】[http://nikunjp.wordpress.com/](http://nikunjp.wordpress.com/)
评论 (共 5 条评论)

匿名 (219.142.23.106), 475天前

j-interop 远程收集WIN7 的eventlog日志怎么不行啊，求助赵老师
[回复]()|[支持]()|[反对]()
匿名 (219.142.23.106), 475天前

是啊，win7怎么不行呢。。
[回复]()|[支持]()|[反对]()

匿名 (59.40.119.215), 497天前

xp的开始Remote Register 服务就可以了。 win7还在研究中
[回复]()|[支持]()|[反对]()
匿名 (219.133.0.1), 501天前

win7平台为什么会用不了呢？ 我也测试过了，xp的机器可以成功运行。
有些xp的也不行，我觉得可能是有什么配置项没有配置而导致。
[回复]()|[支持]()|[反对]()

匿名 (111.4.219.247), 722天前

我测试报错：The system cannot find the file specified. win7平台。
[回复]()|[支持]()|[反对]() (共5, 显示全部)/

[表情]() 发表评论

![]()

[看不清楚，再换一张]() [高级模式]()

相关主题

[JAVA问题定位技术](http://simpleframework.net/blog/v/32903.html) 2/3333

[sword](http://simpleframework.net/space/26397.html), 799天前

[Java序列化和克隆](http://simpleframework.net/blog/v/18467.html) 0/1568

[书生](http://simpleframework.net/space/534.html), 870天前
[Java EE6中的装饰器，高级用法](http://simpleframework.net/blog/v/46888.html) 0/1163

[赵老师](http://simpleframework.net/space/176.html), 733天前

[通过 Java 编程处理 XML 服务定义](http://simpleframework.net/blog/v/16596.html) 1/1691

[书生](http://simpleframework.net/space/534.html), 877天前
[解析Java软件开发中的五种认识误区](http://simpleframework.net/blog/v/34389.html) 0/1178

[书生](http://simpleframework.net/space/534.html), 792天前

[Java内省和反射机制三步曲之(2)反射](http://simpleframework.net/blog/v/23210.html) 0/1409

[书生](http://simpleframework.net/space/534.html), 854天前

一周点击排行
[留言](http://simpleframework.net/guestbook.html)|[联系我们](http://simpleframework.net/contact.html)|[版权声明](http://simpleframework.net/copyright.html)|[关于](http://simpleframework.net/about.html)

simpleframework.net Copyright ©2011 版权所有

Powered by SimpleFramework [ 0.156 s ]

*
* 数据装载中...
*
