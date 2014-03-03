---
layout: post
title: "Connecting Through Windows Firewall (Windows)"
categories: 虚拟化_Citrix
tags: 
 - 虚拟化_Citrix
--- 

# Connecting Through Windows Firewall (Windows)

[Home](http://msdn.microsoft.com/en-us/ "Home") [Library](http://msdn.microsoft.com/en-us/library "Library") [Learn](http://msdn.microsoft.com/en-us/bb188199.aspx "Learn") [Downloads](http://msdn.microsoft.com/en-us/aa570309.aspx "Downloads") [Support](http://msdn.microsoft.com/en-us/aa570318.aspx "Support") [Community](http://msdn.microsoft.com/en-us/aa497440.aspx "Community") [Sign in](https://login.live.com/login.srf?wa=wsignin1.0&rpsnv=11&ct=1295403418&rver=6.0.5276.0&wp=MCLBI&wlcxt=msdn%24msdn%24msdn&wreply=http:%2F%2Fmsdn.microsoft.com%2Fen-us%2Flibrary%2Faa389286.aspx&lc=1033&cb=&id=254354 "Sign in")| [中国（简体中文）](http://msdn.microsoft.com/en-us/library/preferences/locale/?returnurl=%252fen-us%252flibrary%252faa389286.aspx "中国（简体中文）")| [Preferences](http://msdn.microsoft.com/en-us/library/preferences/experience/?returnurl=%252fen-us%252flibrary%252faa389286.aspx "Preferences")

[![Search]( "Search")](http://msdn.microsoft.com/en-us/library/aa389286.aspx#)

![]()

[MSDN Library](http://msdn.microsoft.com/en-us/library/ms123401.aspx "MSDN Library")
![]()

[Windows Development](http://msdn.microsoft.com/en-us/library/ee663300(v=VS.85).aspx "Windows Development")
![]()

[Administration and Management](http://msdn.microsoft.com/en-us/library/ee663258(v=VS.85).aspx "Administration and Management")
![]()

[Windows Management Instrumentation](http://msdn.microsoft.com/en-us/library/aa394582(v=VS.85).aspx "Windows Management Instrumentation")
![]()

[Using WMI](http://msdn.microsoft.com/en-us/library/aa393964(v=VS.85).aspx "Using WMI")
![]()

[Creating WMI Clients](http://msdn.microsoft.com/en-us/library/cc512236(v=VS.85).aspx "Creating WMI Clients")
![]()

[Connecting to WMI on a Remote Computer](http://msdn.microsoft.com/en-us/library/aa389290(v=VS.85).aspx "Connecting to WMI on a Remote Computer")

![]()
[Connecting Through Windows Firewall](http://msdn.microsoft.com/en-us/library/aa389286(v=VS.85).aspx "Connecting Through Windows Firewall") ![Separator]()

Community Content

[![Avatar]()](http://msdn.microsoft.com/en-us/library/aa389286.aspx#CommunityContent "User")

* Add code samples and tips to enhance this topic.
[More...](http://msdn.microsoft.com/en-us/library/aa389286.aspx#CommunityContent "More...")
[![Expand]( "Expand") ![Minimize]( "Minimize")](http://msdn.microsoft.com/en-us/library/aa389286.aspx#)

[![MSDN]( "MSDN")](http://msdn.microsoft.com/en-us/library/default.aspx)![]()

# Connecting Through Windows Firewall

The [Windows Firewall](http://msdn.microsoft.com/en-us/library/aa364727.aspx) (formerly known as Internet Connection Firewall) service and Distributed Component Object Model (DCOM) can cause access denied errors (such as an "RPC Server Unavailable" error - 0x800706ba) when your remote computers and accounts, used for remote connections, are not properly configured.

**Note**  Content in this topic applies to Windows XP with Service Pack 2 (SP2) and Windows Server 2003 with Service Pack 1 (SP1). For information that applies to Windows Vista, see [Connecting to WMI Remotely Starting with Vista](http://msdn.microsoft.com/en-us/library/aa822854.aspx).
**Windows Server 2003 with SP1:  **The Windows Firewall is not enabled by default. **Windows XP with SP2:  **The Windows Firewall is enabled by default.

Resetting the firewall settings will enable the firewall—regardless of the platform.

**Windows Server 2003 and Windows XP:  **Windows Firewall is not available. Use Internet Connection Firewall.

When obtaining data from a remote computer, WMI must establish a DCOM connection from Computer A (the local computer) to Computer B (the remote computer)—this is shown in the diagram as Connection 1. To establish this connection, both Windows Firewall and DCOM on Computer B must be configured appropriately. The configuration must be done locally on Computer B either by changing the Group Policy settings, by executing NETSH commands, or by executing a script locally. Windows Firewall does not support any remote configuration. The following sections describe how to configure Connection 1 and Connection 2 using NETSH commands and the Group Policy editor. For more information about configuring the connections with a script, see [**http://www.microsoft.com/technet/community/columns/scripts/sg1104.mspx#EJAA**](http://go.microsoft.com/FWLink/?LinkId=84425).

The following diagram shows the relationship of WMI, the Windows Firewall, and DCOM when a script or another WMI client makes an asynchronous call to obtain data from WMI. Synchronous and semisynchronous calls only make Connection 1. Connection 2 occurs only with asynchronous calls. If the script or application made an [*asynchronous*](http://msdn.microsoft.com/en-us/library/aa390790.aspx#wmi.gloss_asynchronous_method) call, Connection 2 from Computer B to Computer A delivers the results. This delivery is the callback to the [*sink*](http://msdn.microsoft.com/en-us/library/aa390836.aspx#wmi.gloss_sink). When possible, semisynchronous calls should be made instead of asynchronous calls. The performance of semisynchronous calls is almost as good as asynchronous calls and semisynchronous calls are more secure.

 
![WMI client making an asynchronous call to obtain data from WMI]( "WMI client making an asynchronous call to obtain data from WMI")

**WMI client making an asynchronous call to obtain data from WMI**

 

To successfully connect from Computer A to Computer B when the Windows Firewall is enabled on Computer B, some configuration of firewall settings is necessary. The following procedure helps you configure Connection 1.

![Aa389286.wedge(en-us,VS.85).gif]( "Aa389286.wedge(en-us,VS.85).gif")**To configure Connection 1**

1. Ensure that the user account that is on Computer A is a local administrator on Computer B.

If the user account that is on Computer A is not an administrator on Computer B, but the user account has Remote Enable permission on Computer B, then the user must also be given DCOM Remote Launch and Remote Activation privileges on Computer B by running **Dcomcnfg.exe** at the command prompt. For more information, see the remote launch and activation permissions procedure in [Securing a Remote WMI Connection](http://msdn.microsoft.com/en-us/library/aa393266.aspx). The 0x80070005 error occurs when this privilege is not set. For more information, see [Access to WMI Namespaces](http://msdn.microsoft.com/en-us/library/aa822575.aspx).
1. Allow for remote administration on Computer B.

You can use either the Group Policy editor (Gpedit.msc) or a script to enable the Windows Firewall: Allow remote administration exception, or use a netsh firewall command at the command prompt to allow for remote administration on Computer B. The following command enables this feature.

**netsh firewall set service****RemoteAdmin****enable**

If you would rather use the Group Policy editor than the NETSH commands above, use the following steps in the Group Policy editor (Gpedit.msc) to enable "Allow Remote Administration" on Computer B.

1. Under the **Local Computer Policy** heading, double-click **Computer Configuration**.
1. Double-click **Administrative Templates**, **Network**, **Network Connections**, and then **Windows Firewall**.
1. If the computer is in the domain, then double-click **Domain Profile**; otherwise, double-click **Standard Profile**.
1. Click **Windows Firewall: Allow remote administration exception**.
1. On the **Action** menu, select **Properties**.
1. Click **Enable**, and then click **OK**.

The connection from Computer B to Computer A (Connection 2 in the diagram above) is only required when the client script or application makes an asynchronous call to the remote computer. When possible, semisynchronous calls should be made instead of asynchronous calls. The call performance of semisynchronous calls is almost as good as asynchronous calls and semisynchronous calls are more secure. For more information, see [Calling a Method](http://msdn.microsoft.com/en-us/library/aa384832.aspx) or [Querying WMI](http://msdn.microsoft.com/en-us/library/aa392903.aspx). The following procedure helps you configure Connection 2.

![Aa389286.wedge(en-us,VS.85).gif]( "Aa389286.wedge(en-us,VS.85).gif")**To configure Connection 2**

1. If the Windows Firewall is enabled on Computer A, enable the "Allow Remote Administration" exception (step 2 in the procedure above) and open the DCOM port TCP 135 on Computer A. If this port is not open, the error 0x800706ba will occur. You can open port 135 using the following command.

**netsh firewall add portopening****protocol=****tcp****port=****135****name=***DCOM_TCP135*
1. Add the client application or script, which contains the sink for callback to the Windows Firewall Exceptions List on Computer A. If the client is a script or a MMC snap-in, the sink is often Unsecapp.exe. For these connections, add %windir%\system32\wbem\unsecapp.exe to the Windows Firewall application exceptions list. You can add Unsecapp.exe with the following command.

**netsh firewall add allowedprogram****program=****%windir%\system32\wbem\unsecapp.exe****name=****UNSECAPP**

Generally a C++ application has a sink written for the application. In that case, the application sink should be added to the firewall exceptions.
1. If Computer B is either a member of WORKGROUP or is in a different domain that is untrusted by Computer A, then Connection 2 is created as an Anonymous connection. An anonymous connection fails with either the 0x80070005 error or the 0x8007000e error unless Anonymous connections are given the DCOM Remote Access permission on Computer A. The steps to grant DCOM remote access permissions are listed in [Securing a Remote WMI Connection](http://msdn.microsoft.com/en-us/library/aa393266.aspx).

### []()Related Topics

[Connecting to WMI on a Remote Computer](http://msdn.microsoft.com/en-us/library/aa389290.aspx)[Connecting Between Different Operating Systems](http://msdn.microsoft.com/en-us/library/aa389284.aspx)[Securing a Remote WMI Connection](http://msdn.microsoft.com/en-us/library/aa393266.aspx)[Creating Processes Remotely](http://msdn.microsoft.com/en-us/library/aa389769.aspx)[Setting Security on an Asynchronous Call](http://msdn.microsoft.com/en-us/library/aa393614.aspx)[Setting Security on an Asynchronous Call in VBScript](http://msdn.microsoft.com/en-us/library/aa393615.aspx)

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation feedback [wmisdk\wmi]:  Connecting Through Windows Firewall  RELEASE: (1/6/2011)&body=PRIVACY STATEMENTThe SDK team uses the feedback submitted to improve the SDK documentation. We do not use your email address for any other purpose. We will remove your email address from our system after the issue you are reporting has been resolved. While we are working to resolve this issue, we may send you an email message to request more information about your feedback. After the issues have been addressed, we may send you an email message to let you know that your feedback has been addressed.For more information about Microsoft's privacy policy, see http://privacy.microsoft.com/en-us/default.aspx. "Send comments about this topic to Microsoft")

Build date: 1/6/2011
Community Content [Add](http://msdn.microsoft.com/en-us/library/community/add/aa389286.aspx "Add")

[![Annotations]()](http://msdn.microsoft.com/en-us/library/community-edits.rss?topic=aa389286|en-us|VS.85 "Annotations") [FAQ](http://msdn.microsoft.com/en-us/library/community-msdnwikifaq.aspx "FAQ")
© 2011 Microsoft Corporation. All rights reserved. [Terms of Use](http://msdn.microsoft.com/cc300389.aspx) | [Trademarks](http://www.microsoft.com/library/toolbar/3.0/trademarks/en-us.mspx) | [Privacy Statement](http://www.microsoft.com/info/privacy.mspx) | [Feedback ![Feedback]()](http://msdn.microsoft.com/en-us/library/aa389286.aspx#footerLink "Feedback")

Feedback

[x]()

Tell us about your experience...

Did the page load quickly?
Yes  No

Do you like the page design?
Yes  No

How useful is this topic?
![]()

Tell us more
Enter description here.

![DCSIMG]()

[![]()](http://www.omniture.com/ "Web Analytics")
![Page view tracker]()
