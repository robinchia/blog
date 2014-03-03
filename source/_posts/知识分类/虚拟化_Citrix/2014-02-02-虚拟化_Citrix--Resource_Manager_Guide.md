---
layout: post
title: "Resource_Manager_Guide"
categories: 虚拟化_Citrix
tags: 
 - 虚拟化_Citrix
--- 

# Resource_Manager_Guide

Resource Manager Administrator’s Guide
For other guides in this document set, go to the Document Center
Resource Manager for MetaFrame® Presentation Server Citrix® MetaFrame® Presentation Server 3.0, Enterprise Edition for Windows Citrix® MetaFrame® Access Suite
Use of the product documented in this guide is subject to your prior acceptance of the End User License Agreement. Note that copies of the End User License Agreement are included in the root directory of the MetaFrame XP Server CD-ROM and in the root directory of the Components CD-ROM. Information in this document is subject to change without notice. Companies, names, and data used in examples herein are fictitious unless otherwise noted. Other than printing one copy for personal use, no part of this document may be reproduced or transmitted in any form or by any means, electronic or mechanical, for any purpose, without the express written permission of Citrix Systems, Inc. Copyright © 2004 Citrix Systems, Inc. All rights reserved. Citrix, ICA (Independent Computing Architecture), and WinFrame are registered trademarks, and Citrix Solutions Network, MetaFrame, MetaFrame XP, NFuse, Program Neighborhood, and SpeedScreen are trademarks of Citrix Systems, Inc. in the United States and other countries. RSA Encryption © 1996-1997 RSA Security Inc., All Rights Reserved. Adobe, Acrobat, and PostScript are trademarks or registered trademarks of Adobe Systems Incorporated in the U.S. and/or other countries. DB2 is a registered trademark and PowerPC is a trademark of International Business Machines Corp. in the U.S. and other countries. HP OpenView is a trademark of the Hewlett-Packard Company. Microsoft, MS-DOS, Windows, Windows NT, Win32, Outlook, ActiveX, and Active Directory are either registered trademarks or trademarks of Microsoft Corp. in the United States and/or other countries. Tivoli and NetView are registered trademarks of International Business Machines Corp. in the U.S. and other countries. Unicenter is a registered trademark of Computer Associates International, Inc. All other trademarks and registered trademarks are the property of their owners. Last Updated: January 9, 2004 (SC)
Go to Document Center
Contents 3
Contents
Chapter 1 Introduction
Overview . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 9 About this Guide . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 9 Accessing Documentation . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 10 Introducing Resource Manager . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 11 Monitoring Your Existing Server Farm . . . . . . . . . . . . . . . . . . . . . . . . . . 11 Identifying, Diagnosing, and Solving Problems. . . . . . . . . . . . . . . . . . . . 11 Gauging and Justifying Future Resource Needs . . . . . . . . . . . . . . . . . . . 12 Planning and Scaling Your Server Farm . . . . . . . . . . . . . . . . . . . . . . . . . 12 Billing Users for Resource Usage . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 12 New Features in this Release . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 12 Extended Reporting Capabilities . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 12 Extended Real Time Monitoring Capabilities . . . . . . . . . . . . . . . . . . . . . . . . . . 13 Enhanced Delegated Administration . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 14 Failed Import Alerts . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 14 License Server Connection Failure Alerts . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 14 What to Read Next . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 15
Chapter 2
Installing Resource Manager
Overview . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 17 Software Requirements . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 17 Licensing Information. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 18 Installing Resource Manager . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 18 Before You Start . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 18 Selecting a Server to Use as the Farm Metric Server. . . . . . . . . . . . . . . . 19 If You Are Upgrading from a Previous Version . . . . . . . . . . . . . . . . . . . . . . . . 19 Installing or Upgrading Resource Manager . . . . . . . . . . . . . . . . . . . . . . . . . . . . 19 Uninstalling Resource Manager . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 20 Setting Up a Summary Database . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 21 Setting a System Data Source Name . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 22 Configuring a Database Connection Server . . . . . . . . . . . . . . . . . . . . . . . . . . . . 24 Turning the Summary Database On . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 25 Turning the Summary Database Off . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 26
4
Resource Manager Administrator’s Guide
Go to Document Center
Displaying Resource Manager and its Components . . . . . . . . . . . . . . . . . . . . . . . . 27 Displaying the Main Resource Manager Screen . . . . . . . . . . . . . . . . . . . . . . . . 28 Displaying Resource Manager for the Entire Server Farm . . . . . . . . . . . . . . . . 29 Displaying Resource Manager for a Single Server . . . . . . . . . . . . . . . . . . . . . . 29 Displaying the Applications in a Server Farm . . . . . . . . . . . . . . . . . . . . . . . . . . 29 Changing the Location of Resource Manager After Installation . . . . . . . . . . . . . . 30
Chapter 3
Monitoring Servers and Applications in Real Time
Overview . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 31 Configuring Metrics on Servers. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 32 Checking the Status of Server Metrics . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 33 Customizing Server Metrics . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 34 Monitoring the Disk on a Server with Windows 2000 or Later installed 34 Configuring Application Metrics. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 35 Monitoring Your Farm. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 35 Suspending Notification of a Metric’s Status . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 36 Preparing Your System for Resource Manager Alerts . . . . . . . . . . . . . . . . . . . . . . 36 Preparing Your System for Email Alerts . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 37 Using MAPI to Send Alerts . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 38 Setting up SSL for SMTP Email Alerts . . . . . . . . . . . . . . . . . . . . . . . . . . 39 Preparing Your System for SMS Alerts . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 40 Configuring Resource Manager to Use SMS . . . . . . . . . . . . . . . . . . . . . . 40 Preparing Your System for SNMP Alerts . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 40 Getting More Information About Metrics and Monitoring. . . . . . . . . . . . . . . . . . . 41
Chapter 4
Recording the History of Servers and Applications
Overview . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 43 How Does Resource Manager Information get into a Summary Database? . . . . . 43 What Information Should I Record in the Database? . . . . . . . . . . . . . . . . . . . . . . . 45 Issues to Consider when Selecting Database Information. . . . . . . . . . . . . . . . . 45 The Benefits of Keeping Long-Term Resource Manager Information . . 45 Deciding Which Metrics Information to Store. . . . . . . . . . . . . . . . . . . . . 45 How Can Recording a Metric Affect Database Growth?. . . . . . . . . . . . . 46 Selecting Server and Application Metrics to Record in the Database. . . . . . . . 46 Using Farm-Wide Options for Reducing Summary Data . . . . . . . . . . . . . . . . . 46 Scheduling Summary Data Collection and Removal . . . . . . . . . . . . . . . . . . . . . . . 47 Ignoring Server Metrics During Periods of Low Server Activity . . . . . . . . . . . 47 Removing Unwanted Information from the Database . . . . . . . . . . . . . . . . . . . . 47
Go to Document Center
Contents 5
Setting a Purging Schedule. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 48 Setting the Summary Database Update Time . . . . . . . . . . . . . . . . . . . . . . . . . . 48 Monitoring the Status of the Summary Database . . . . . . . . . . . . . . . . . . . . . . . . . . 49 Managing the Resource Manager Summary Database . . . . . . . . . . . . . . . . . . . . . . 52 Estimating Summary Database Growth . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 53 Managing Summary Database Growth . . . . . . . . . . . . . . . . . . . . . . . . . . 54
Chapter 5
Reporting and Analyzing Resource Manager Information
Overview . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 55 Generating Reports to Analyze Data . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 56 Creating Reports on Current Activity . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 56 Reporting on the Use of Processes or Applications . . . . . . . . . . . . . . . . . . . . . . 57 Reporting on User Activity. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 59 Looking Back to a Specific Time on a Server . . . . . . . . . . . . . . . . . . . . . . . . . . 60 Creating Reports on Past Activity . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 61 Looking Back at Specific Processor Application Usage . . . . . . . . . . . . . . . . . . 61 Looking Back at Specific Users’ Activities . . . . . . . . . . . . . . . . . . . . . . . . . . . . 63 Looking Back at Activities on a Specific Server . . . . . . . . . . . . . . . . . . . . . . . . 65 How Reports from Servers in Different Time Zones or Locales are Displayed. . . 67 What if a Reporting Server Uses a Different Time Zone?. . . . . . . . . . . . . . . . . 67 What if a Reporting Server Uses a Different Language?. . . . . . . . . . . . . . . . . . 67 Estimating the Concurrent User Capacity of a Server. . . . . . . . . . . . . . . . . . . . . . . 68
Chapter 6
Billing Users for Resource Usage
Overview . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 69 Creating a Fee Profile. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 69 Organizing Users into Cost Centers for Billing. . . . . . . . . . . . . . . . . . . . . . . . . . . . 70 Producing Billing Reports . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 70 Cost Centers Report . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 70 Domain Users Report . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 70 How Information Is Presented in the Report . . . . . . . . . . . . . . . . . . . . . . . . . . . 71 User. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 71 Session Start . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 71 Session Elapsed Time. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 71 Process . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 71 CPU Time Used . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 71 Memory Used . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 71 Process Loaded Time . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 71
6
Resource Manager Administrator’s Guide
Go to Document Center
Process Active Time. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 72 Cost. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 72 Fee Profile . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 72
Chapter 7
Troubleshooting
Overview . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 73 Unexpected Behavior . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 73 Locating Servers. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 76
Chapter 8
Using the Monitoring and Reporting Extensions
Monitoring Server Metrics. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 77 Creating Reports. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 78 Unexpected Behavior . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 79
Appendix A
Default Metric Set . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .81
Overview . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 81 Default Set of Metrics . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 82 Data Store Connection Failure . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 82 Minutes . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 82 License Server Connection Failure. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 82 Minutes . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 82 Logical Disk . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 82 % Disk Time. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 82 % Free Space . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 83 Memory . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 83 Available Bytes . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 83 Pages/sec. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 83 Network Interface . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 84 Bytes Total/sec . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 84 Paging File . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 84 % Usage . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 84 Processor . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 84 % Interrupt Time . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 84 % Processor Time. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 84 System . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 85 Context Switches/sec . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 85 Terminal Services . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 85 Active Sessions. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 85
Go to Document Center
Contents 7
Inactive Sessions . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 85
Appendix B
Summary Database Schema . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .87
Overview . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 87 Database Entity Relationship Diagram . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 87 Database Entity Table Descriptions. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 89 Application History. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 89 SDB_APPHISTORY . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 89 Application Metrics . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 90 SDB_APPMETRICS . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 90 Client History . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 91 SDB_CLIENTHISTORY. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 91 Connection History . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 92 SDB_CONNECTIONHISTORY . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 92 Event Log . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 93 SDB_EVENTLOG . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 93 Administrator Configurable Server Metrics. . . . . . . . . . . . . . . . . . . . . . . . . . . . 94 SDB_METRICS . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 94 Processes . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 95 SDB_PROCESS . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 95 User Information. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 98 SDB_SESSION . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 98 Version Control. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 100 SCHEMAVERSION . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 100 Support and Look-Up Tables . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 100 LU_APPNAME . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 100 LU_CLIENT. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 101 LU_CLIENTPROPERTIES . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 101 LU_CLIENTTYPEMAPPINGS . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 102 LU_FARMNAME . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 102 LU_INSTANCE . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 103 LU_LAUNCHER . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 103 LU_METRIC . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 104 LU_METRICCOUNTER. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 105 LU_NETDOMAIN. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 105 LU_OBJECT . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 106 LU_PATH . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 106 LU_PROCESS . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 107 LU_PROCESSNAME . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 108
8
Resource Manager Administrator’s Guide
Go to Document Center
LU_SERVER . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 108 LU_SERVERNAME . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 109 LU_SERVERINF . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 110 LU_WINSTATION . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 111 SDB_SCRATCH . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 111 SDB_HEURISTICS . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 112 String Length Table . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 113 SQL Data Type Mapping. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 114
Appendix C
Glossary . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .115
Index . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 119
Go to Document Center
CHAPTER 1
Introduction
Overview
Welcome to Resource Manager for MetaFrame Presentation Server. This chapter introduces you to the documentation and to Resource Manager. Topics include: • • • • • How to use this guide Accessing documentation in general An introduction to Resource Manager What’s new in this release Details of where to find more information about Resource Manager
About this Guide
This guide is for MetaFrame administrators responsible for installing, configuring, and maintaining Resource Manager. This guide is designed to introduce you to the key concepts of Resource Manager, and to get you started quickly and easily. This chapter introduces the documentation and the Resource Manager product, and describes what’s new in this release. Subsequent chapters provide an overview of how to deploy and configure Resource Manager, and provide you with a summary of the main ways you can use it in your deployment. This guide assumes knowledge of MetaFrame Presentation Server. If you will be recording the history of server and application metrics using the summary database, you need to be familiar with Microsoft SQL server or Oracle DBMS. For detailed information on how to carry out the Resource Manager procedures outlined in this guide, see the Resource Manager Help.
10
Resource Manager Administrator’s Guide
Go to Document Center
Accessing Documentation
This administrator’s guide is part of the MetaFrame Presentation Server documentation set. The documentation set includes online guides that correspond to different features of MetaFrame Presentation Server. Online documentation is provided as Adobe Portable Document Format (PDF) files. Use the Document Center to access the complete set of online guides. The Document Center provides a single point of access to the documentation that enables you to go straight to the section of documentation that you need. The Document Center includes: • • • A list of common tasks and a link to each item of documentation. A search function that covers all the PDF guides. This is useful when you need to consult a number of different guides. Cross-references between documents. You can move between documents as often as you need using the links to other guides and the links to the Document Center.
Important To view, search, and print the PDF documentation, you need to have the Adobe Acrobat Reader 5.0.5 with Search or a later version with Search. You can download Adobe Acrobat Reader for free from Adobe Systems’ Web site at http://www.adobe.com/. If you prefer to access the guides without using the Document Center, you can navigate to the component PDF files using Windows Explorer. If you prefer to use printed documentation, you can also print each guide from Acrobat Reader. More information about Citrix documentation, and details about how to obtain further information and support, is included in Getting Started with MetaFrame Presentation Server.
Go to Document Center
Chapter 1 Introduction
11
Introducing Resource Manager
You can use Resource Manager to manage resources on single or multiple MetaFrame Presentation Server computers. Resource Manager enables you to collect, display, store, and analyze data about system performance, application or process use, and user activity. Use Resource Manager to: • • • Watch what is happening at a particular moment for a system. This is known as real-time monitoring. Analyze and report using records of system activity. You can create reports for current activities or create reports on past activities using a summary database. Create Billing reports to charge users for their use of resources using a summary database.
Resource Manager can track and store information about a wide variety of system and network processes and events. These are known as metrics. If the value of a metric falls outside normal limits, Resource Manager can inform you. During installation, Resource Manager automatically creates a set of default metrics and assigns limits to define the normal operation of each one. Tip Resource Manager can track any Windows Performance Monitor counter as a server metric. Further information on these counters is available in Appendix C of the MetaFrame Presentation Server Administrator’s Guide and the Microsoft Windows Help for Performance Monitor. You can use Resource Manager in your server farm to help you with the following tasks:
Monitoring Your Existing Server Farm
You can use Resource Manager to monitor and analyze system performance, loading, and user behavior. You can view information about an entire server farm, analyze individual servers and applications, or monitor specific aspects of performance. By fine-tuning the monitoring process, you can customize the information that Resource Manager provides to suit your specific environment.
Identifying, Diagnosing, and Solving Problems
Resource Manager can warn you about any developing problems in your environment. If a problem does occur, you can analyze the relevant data to help you decide what action to take.
12
Resource Manager Administrator’s Guide
Go to Document Center
Gauging and Justifying Future Resource Needs
You can produce reports about system usage that help you identify requirements for future resources, such as new servers or additional licenses.
Planning and Scaling Your Server Farm
By deploying Resource Manager on a pilot system, you can gauge how the server farm will cope in different possible scenarios. This will help you to scale your network, set baselines, and spot potential problems before they affect the final installation. For example, you can evaluate whether or not a particular server can support the activity of the desired number of users.
Billing Users for Resource Usage
You can produce Billing reports that use the resource usage data from the summary database and a fee profile to calculate the charges for users of the server farm. You define fee profiles to reflect different charging rates and currencies. When Resource Manager generates a Billing report, it calculates the charges by multiplying the resources used during the report period by the associated fees.
New Features in this Release
This release of Resource Manager includes the following new features and enhancements.
Extended Reporting Capabilities
The Report Center is one of the system management tools provided through the Access Suite Console. The Access Suite Console snaps into the Microsoft Management Console to provide a central location for system management tools. You can also use it to launch other consoles such as the MetaFrame Presentation Server Console. The Report Center extends Resource Manager reporting capabilities, and allows you to easily generate reports from a variety of real-time and historical data sources. A wizard helps you select the type of report, the data to be displayed, and the schedule for running the report. You can view the status of your scheduled report, and, for those that have not been run, you can adjust the report parameters. The Report Center contains several report types, for example: • Application Availability report. Lists the published applications in a farm and the percentage of time when they were available, in a planned down state, and in an unplanned down state.
Go to Document Center
Chapter 1 Introduction
13
•
Server Availability report. Lists the servers in a farm and the percentage of time when they were available, in a planned down state, and in an unplanned down state. Server Performance report. Displays, for the selected servers, the average CPU utilization, the minimum memory available, and the maximum number of concurrent connections. The report also contains details of the most heavily loaded servers. Application report. Provides details of all published applications including the servers they are published on, users and groups with access to them, and any options used by clients that connect to the applications. Application Usage report. Shows the most frequently used applications, the number of times each application has been accessed, and the maximum number of concurrent users.
•
•
•
For further details of how to use the Report Center, see the assistance provided on the Report Center screens and “Using the Monitoring and Reporting Extensions” on page 77.
Extended Real-Time Monitoring Capabilities
The Dashboard on the Access Suite Console allows you to display performance metrics for the servers in a farm in a highly visual way. You can create custom views of farms, zones, servers, and applications by using the My Views feature. My Views allows you to create and save customized views of items in your deployment. You can then see updated information about those items at any time, without having to search for them. For example, you can group related objects in folders or you can order farms, according to the frequency with which you need to access them. You may want to monitor farms located in different locations. In this case, you could use the My Views feature to create a custom view that groups each farm in a different part of the screen and then use a background graphic to identify each location. A suitable performance metric, such as CPU Load, displayed in your custom view, allows you to pinpoint problems quickly on any server in any location. Using custom views in this way helps you anticipate or identify problems with your servers or applications as soon as possible. Note that these servers must be running the MetaFrame Presentation Server Provider for Microsoft Windows Management Instrumentation. For further information, see the assistance provided on the Access Suite Console screens and “Using the Monitoring and Reporting Extensions” on page 77.
14
Resource Manager Administrator’s Guide
Go to Document Center
Enhanced Delegated Administration
Enhanced delegated administration in Resource Manager allows you to delegate the tasks of controlling applications and analyzing system and application data to custom administrators. For more information on Enhanced Delegated Administration, see the MetaFrame Presentation Server Administrator’s Guide. To simplify the configuration of custom administrators, full administrators may find it easier to first create a suitable folder hierarchy, and then create custom administrators with the necessary permissions. You create custom administrators using the Add MetaFrame Administrator wizard in the Presentation Server Console. For Resource Manager you can: • Delegate the control of Resource Manager applications to custom administrators. A Resource Manager application is an application that is not a MetaFrame Presentation Server published application but is still recognized by the Resource Manager system. Details of how to set up Resource Manager applications are provided in the Help. Determine which custom administrators can generate current reports, summary reports, or Billing reports Ensure that administrators receive alerts only for those servers for which they have responsibility
• •
Failed Import Alerts
Resource Manager warns you of failed attempts to update the summary database, and can be configured to send alerts under these circumstances. In its red state, a new icon on the Summary Database tab identifies failures to commit any record in the last attempted update to the database, and the Summary Database Configuration dialog box allows you to notify administrators of any such failure. For further information, see “Monitoring the Status of the Summary Database” on page 49 and “Setting Up a Summary Database” on page 21.
License Server Connection Failure Alerts
With the Citrix MetaFrame Access Suite Licensing feature, Citrix licensing is handled by one or more license servers. MetaFrame Presentation Server computers communicate with license servers to ensure that client sessions are licensed appropriately. A license server connection failure alert is raised when a server cannot communicate with its associated license server. This may be due to a hardware failure on the license server, the license service on the license server malfunctioning, or network problems between the license server and the MetaFrame Presentation Server computer.
Go to Document Center
Chapter 1 Introduction
15
When contact with a license server is lost, the MetaFrame Presentation Server computer lapses into a licensing grace period. Typically, the licensing grace period is 96 hours. During this period, the MetaFrame Presentation Server software is fully functional and connections to the server work normally. Contact with the license server must be reestablished before the grace period ends, or the software is reduced automatically to single user mode and only the administrator can log on to the server. For further information, see “Preparing Your System for Resource Manager Alerts” on page 36.
What to Read Next
This guide provides: • • • • Installation instructions. An overview of the tasks that you carry out using Resource Manager. For stepby-step instructions on how to carry out Resource Manager tasks see the Help. Detailed information on Resource Manager-related tasks that you carry out using other tools or products. Reference information.
Refer to Chapter 2: “Installing Resource Manager” Chapter 3: “Monitoring Servers and Applications in Real Time” Chapter 4: “Recording the History of Servers and Applications” Chapter 5: “Reporting and Analyzing Resource Manager Information” Chapter 6: “Billing Users for Resource Usage”
To find Instructions about installing Resource Manager An overview of real-time monitoring An overview of how to record the history of servers and applications using a summary database An overview of how to generate reports from stored Resource Manager information An overview of how to charge users for resource usage using summary database information Answers to common questions about Resource Manager Procedures for using the reporting and monitoring extensions
Chapter 7: “Troubleshooting” Chapter 8: “Using the Reporting and Monitoring Extensions”
16
Resource Manager Administrator’s Guide
Go to Document Center
Details about the default metrics that are configured during installation Definitions, layout, and organization of summary database schema Definitions of the technical terms used in this guide
Appendix A: “Default Metric Set” Appendix B: “Summary Database Schema” Appendix C: “Glossary”
Go to Document Center
CHAPTER 2
Installing Resource Manager
Overview
This chapter explains how to install Resource Manager. Topics include: • • • • • • • Issues to consider before you start, including software requirements and licensing information Installation instructions How to upgrade to the latest version of Resource Manager An overview of the user interface and navigation How to set up a summary database, including setting up a system data source name (DSN) and Database Connection Server How to turn the summary database on and off How to change the location of Resource Manager after installation
Software Requirements
You install Resource Manager when you install or upgrade your servers to MetaFrame Presentation Server 3.0, Enterprise Edition for Windows. If you are upgrading from a previous release of MetaFrame Presentation Server, it is not necessary for Resource Manager to have been installed previously. For guidelines about the licensing requirements for MetaFrame Presentation Server or other components, see the MetaFrame Access Suite Licensing Guide.
18
Resource Manager Administrator’s Guide
Go to Document Center
If you are going to record the history of server and application metrics using the summary database, you need one of the following DBMS packages: • • Microsoft SQL Server Versions 7 or 2000 Oracle Database Versions 7, 8i, or 9i —Or—
Licensing Information
Full Resource Manager functionality requires a MetaFrame Presentation Server Enterprise Edition license. Refer to the MetaFrame Access Suite Licensing Guide for further details.
Installing Resource Manager
Before You Start
Before you install Resource Manager, Citrix recommends that you do the following: • Ensure that the servers on which you are going to install Resource Manager meet the software requirements listed in “Software Requirements” on page 17. Resource Manager stores recent information about applications, servers, and users in a local database that can become large if the server is heavily loaded. If this may cause an issue in the future, you can select an alternative installation location for Resource Manager. See “Installing or Upgrading Resource Manager” on page 19 for more information. Ensure that each server on which Resource Manager is to be installed can connect to a data store. The data store is a database that MetaFrame Presentation Server and its components use to keep track of configuration information about the servers, applications, and configured users in the server farm. You set up a data store during MetaFrame Presentation Server installation. Resource Manager uses this data store. If you are going to use a summary database, you need to install DBMS software on a server. This computer is the database storage facility for your server farm’s summary data. It does not need to be a farm server but must be available to the farm servers through the network. Ensure that this server has enough available space to store the summary database. Refer to “Managing the Resource Manager Summary Database” on page 52.
•
•
Go to Document Center
Chapter 2 Installing Resource Manager
19
Selecting a Server to Use as the Farm Metric Server
By default, the first server on which you install Resource Manager becomes the Farm Metric Server. The Farm Metric Server interprets metrics that apply to the entire server farm (for example, application counts) and sends alerts when required. Citrix recommends that the Farm Metric Server be lightly loaded and, preferably, be a MetaFrame Presentation Server data collector. If necessary, you can change the Farm Metric Server to a different machine after installation. For more information, see the Help. The second server on which you install Resource Manager becomes the backup Farm Metric Server. If these servers will experience heavy loading or are not data collectors, specify different servers to be the Farm Metric Servers.
If You Are Upgrading from a Previous Version
If you decide to upgrade servers to MetaFrame Presentation Server over a period of time (rather than simultaneously), ensure that you upgrade the Database Connection Server first, then the main Farm Metric Server, then the backup Farm Metric Server, before upgrading other servers in the server farm. Problems may occur if another server is running a later version of Resource Manager.
Installing or Upgrading Resource Manager
Use the following procedure to install or upgrade Resource Manager. You need to follow this procedure for each server in your server farm. To install Resource Manager on a server 1. Follow the installation instructions in Chapter 4 of the MetaFrame Presentation Server Administrator’s Guide, making sure that you choose to install MetaFrame Presentation Server Enterprise Edition. 2. When the Component Selection setup screen appears, if you want to change the installation location of Resource Manager, browse to the correct location then click OK. 3. At the prompt, click Restart to restart the server. Note You need to install the Presentation Server Console on every server from which you want to administer Resource Manager servers. For instructions on how to install the Console, see “To install the Presentation Server Console on a workstation” on page 27.
20
Resource Manager Administrator’s Guide
Go to Document Center
Uninstalling Resource Manager
Important If you are uninstalling MetaFrame Presentation Server from the Resource Manager Farm Metric Server(s) or Database Connection Server for a summary database, reassign the server before uninstalling. That is, change the Farm Metric Server(s) and/or the Database Connection Servers to other Resource Manager servers before uninstalling or removing from the server farm. If you are using a summary database, Citrix recommends that you update it before removing any servers from the server farm. For details of how to update the database, see the Help. To uninstall Resource Manager from a server 1. Log off any currently connected clients and the Presentation Server Console, and exit all programs running on the server. 2. From the Start menu, select Settings > Control Panel > Add/Remove Programs. 3. Select Citrix MetaFrame Presentation Server for Windows. 4. Select Change. The Setup wizard starts. 5. On the Application Maintenance screen, select Modify, then click Next. 6. On the Component Selection screen, select Resource Manager and select Entire feature will be unavailable. 7. Select Next. The selections you have made are listed for review. 8. Click Finish. Note If you run the uninstaller after manually changing the location of Resource Manager, as described in “Changing the Location of Resource Manager After Installation” on page 30, the uninstaller does not remove the Resource Manager folder, and you must delete it manually.
Go to Document Center
Chapter 2 Installing Resource Manager
21
Setting Up a Summary Database
Before you can start using a summary database, you must do the following: 1. Install your DBMS software on a server and create a database on it. This server is the database storage facility for your server farm’s summary data. It does not need to be a farm server but needs to be available to the server farm through the network. You need to ensure that this server has enough available space to store the summary database. Refer to “Managing the Resource Manager Summary Database” on page 52. Resource Manager supports the following DBMS software: • • Microsoft SQL Server Versions 7 and 2000. Oracle Database Versions 7, 8i, and 9i. If you are using an Oracle DBMS, ensure that the character set it uses contains all the characters you use in your server farm; for example, for server and application names. This includes special characters and currency symbols.
Important When you create your summary database on the DBMS server, the DBMS access credentials you set to be used by Resource Manager must each not exceed 255 characters in length. This is irrespective of the limits of the DBMS software itself. If you are using a Microsoft SQL Server DBMS, do not use the master database for your summary database. The master database is used by SQL Server for internal functions. Using it for your summary database may cause database corruption problems. Citrix recommends that you do not install the DBMS on the Database Connection Server. 2. Set a system data source name (DSN). The system DSN stores information about how a client can connect to a database. It is required by the Database Connection Server (the database client) to be able to communicate with the summary database DBMS. Refer to “Setting a System Data Source Name” on page 22. 3. Configure a Database Connection Server. This server enables communications between the server farm and the summary database by writing data to the database and reading data from it. It should be relatively low-load for best performance. Refer to “Configuring a Database Connection Server” on page 24.
![]() 22
Resource Manager Administrator’s Guide
Go to Document Center
Setting a System Data Source Name
To set a system data source name for Microsoft SQL Server DBMS 1. Choose a server to be your Database Connection Server. 2. Open the Windows Control Panel. 3. Open the ODBC Data Source Administrator dialog box. To do this, open Administrative Tools, then open Data Sources (ODBC). 4. Click the System DSN tab. 5. Click Add. 6. In the Create New Data Source dialog box, select SQL Server. 7. Click Finish. 8. In the Microsoft SQL Server DSN Configuration dialog box, type rmsummarydatabase in the Name box. Type a description (optional), then select the server with the DBMS installed on it from the Server list. Important You must type rmsummarydatabase exactly. Any spaces or spelling errors will make the database unrecognizable to the Database Connection Server. However, the field is not case-sensitive.
Screenshot of the Microsoft SQL Server DSN Configuration Dialog Box
9. Click Next.
Go to Document Center
Chapter 2 Installing Resource Manager
23
10. Select how Microsoft SQL Server authenticates your identification so you can set up the system DSN. Either: Click With Windows NT authentication using the network login ID to use Windows NT authentication. —Or— • Click With SQL Server authentication using a login ID and password entered by the user, then select the Connect to SQL Server to obtain default settings for the additional configuration options check box and type a user name and password in the Login ID and Password boxes, respectively. •
11. Click Client Configuration. 12. In the Edit Network Library Configuration dialog box, select TCP/IP under Network libraries. 13. Click OK. 14. Click Next. 15. Select the Change the default database to check box, then select the database you created on the DBMS server from the list. 16. Click Next, then Finish. 17. In the ODBC Setup dialog box, you can click Test Data Source to confirm the DSN configuration. Click OK, then OK again to close the dialog box. 18. Click OK to close the ODBC Data Source Administrator dialog box. For more information, refer to your Windows operating system and Microsoft SQL Server documentation. To set a system data source name for Oracle DBMS Note The system DSN setup described below may be different for other Oracle versions. 1. Choose a server to be your Database Connection Server. 2. Open the Windows Control Panel. 3. Open the ODBC Data Source Administrator dialog box. To do this, open Administrative Tools, then open Data Sources (ODBC). 4. Click the System DSN tab. 5. Click Add.
24
Resource Manager Administrator’s Guide
Go to Document Center
6. In the Create New Data Source dialog box, select the Oracle ODBC Driver option. This option is available only after the Oracle Client is installed. 7. Click Finish. 8. In the Oracle ODBC Driver Configuration dialog box, type rmsummarydatabase in the Data Source Name text box. Type a description (optional). Important You must enter rmsummarydatabase exactly. Any spaces or spelling errors will make the database unrecognizable to the Database Connection Server. However, the field is not case-sensitive. 9. From the TNS Service Name list, select the global database name of the Oracle database, and type the user name in the User ID box. 10. Select Disable Microsoft Transaction Server (MTS) in the Workarounds tab. For information about why MTS needs to be disabled, refer to Microsoft Knowledge Base articles Q180190 and Q193893, available from http://www.microsoft.com. 11. Ensure that: • • The Read Only check box is cleared Enable closing cursor and Enable Results Set are selected
12. Click OK to close the Oracle ODBC Driver Configuration dialog box. Note that the dialog box shown varies between Oracle releases. For more information, refer to your Windows operating system and Oracle Database documentation.
Configuring a Database Connection Server
After you set up a system data source name (DSN) on the Database Connection Server, you need to configure this server as the Database Connection Server using the Presentation Server Console. To configure a Database Connection Server 1. In the left pane of the Console, click Resource Manager. 2. In the right pane, click the Summary Database tab. 3. Click Configure. 4. In the Summary Database Configuration dialog box, select your Database Connection Server from the Server list. Only servers running Resource Manager for MetaFrame Presentation Server appear in the list.
![]() Go to Document Center
Chapter 2 Installing Resource Manager
25
Note If a server name is dimmed, this means that the server is running an older version of Resource Manager and should not be selected. 5. Enter the DBMS access credentials in the User and Password boxes. These must match valid credentials defined within the supporting DBMS (the Oracle or Microsoft SQL Server database you are using). Note Resource Manager supports Windows NT authentication for the Microsoft SQL Server user name. The 255 character limit for the user name includes the domain name, the intervening backslash ( \ ), and the user name. 6. Click Test to check the connection to the database. You can now activate the summary database. See “Turning the Summary Database On” on page 25. You can further configure the Database Connection Server in the following ways: • • • • Configure the database update time. See “Setting the Summary Database Update Time” on page 48. Configure a database purging schedule to remove unwanted information. See “Removing Unwanted Information from the Database” on page 47. Choose the methods for sending alerts when an update to the database fails. See “Preparing Your System for Resource Manager Alerts” on page 36. Configure data collection restrictions for all farm servers. See “Using FarmWide Options for Reducing Summary Data” on page 46.
Turning the Summary Database On
To begin recording data for your database, you need to turn the summary database on after installation. When it is on, Resource Manager servers create and store information for inclusion in the summary database. To turn the summary database on 1. In the left pane of the Console, click Resource Manager. 2. In the right pane, click the Summary Database tab. Note The first icon in the Status panel is Not Configured when the summary database is off or a Database Connection Server is not configured. In this state, Resource Manager servers are not creating or storing information for inclusion in the summary database.
![]() ![]() 26
Resource Manager Administrator’s Guide
Go to Document Center
3. Click Configure. 4. In the Summary Database Configuration dialog box, select the Summary Database enabled check box. 5. Click OK, then OK again. The first icon in the Status panel is OK , meaning the summary database is on and a Database Connection Server is correctly configured and in use. In this state, Resource Manager servers are collecting information for inclusion in the database.
Turning the Summary Database Off
If you need to stop creating summary data, for example for maintenance purposes, you can turn the summary database off. CAUTION When the summary database is off, Resource Manager servers will no longer create and store information for the summary database. This may result in data loss until the summary database is turned back on. You cannot turn the summary database off for individual servers; however, you can minimize the data being contributed by choosing to ignore periods of low activity for an individual server, for example, weekends or late at night. Refer to “Ignoring Server Metrics During Periods of Low Server Activity” on page 47 for details. To turn the summary database off 1. In the left pane of the Presentation Server Console, click Resource Manager. 2. In the right pane, click the Summary Database tab. 3. Click Configure. 4. In the Summary Database Configuration dialog box, clear the Summary Database enabled check box. 5. Click OK, then OK again. The first status icon in the Status panel is Not Configured , meaning the summary database is off. In this state, Resource Manager servers are not creating or storing information for inclusion in the summary database.
Go to Document Center
Chapter 2 Installing Resource Manager
27
Displaying Resource Manager and its Components
The following procedures are designed to familiarize you with the interface so that you can quickly get up to speed with Resource Manager. The user interface for Resource Manager is integrated with the Presentation Server Console. To open the Presentation Server Console From the Start menu, choose Programs > Citrix > MetaFrame Presentation Server > Presentation Server Console. You can also open the Presentation Server Console from the Access Suite Console if the Access Suite Console has been appropriately configured. When the Console starts, log on to a server. When you are connected to a server farm, the Console displays a window with two main panes: • • The left pane shows a hierarchical list of the components of the server farm The right pane shows information about the object that is selected in the left pane
From this window you can access Resource Manager. The servers and applications that you see depend on whether you are a full administrator or a custom administrator. If you are a full administrator you have access to the entire server farm. If you are a custom administrator you can view and update only those folders of servers or applications for which the full administrator has granted you permissions. You need to install the Console on every machine from which you want to administer servers with Resource Manager installed. You can install the Console on a MetaFrame Presentation Server computer (at the time when you install MetaFrame Presentation Server itself), or on a remote machine. To install the Presentation Server Console on a workstation 1. Ensure that Resource Manager is installed on the server for which you want to view Resource Manager information. 2. Install or upgrade the Presentation Server Console on the workstation using the setup program on the MetaFrame Presentation Server CD-ROM.
![]() 28
Resource Manager Administrator’s Guide
Go to Document Center
Displaying the Main Resource Manager Screen
To display the main screen In the Presentation Server Console, either: • • In the left pane, click Resource Manager. —Or— In the right pane, double-click Resource Manager.
Screenshot of the Main Resource Manager Screen
This window displays a number of tabs that enable you to perform the following functions in Resource Manager: • • Watcher. Show a real-time list of all servers in the server farm that have an alarm state. Reports. Generate reports about: • • • • • • Current process and user activity, and recent server status. Past process, user, and server activity. These reports require a summary database to be in use.
Summary Database. Configure a summary database and see its status. Billing. Configure cost centers (fees and user groups). If you are using a summary database, generate Billing reports based on resource usage. SMS, SNMP, and E-mail. Configure automatic SMS, SNMP, or email alerts. Farm Metric Server. See the status of Farm Metric Servers and change the servers being used as Farm Metric Servers.
Go to Document Center
Chapter 2 Installing Resource Manager
29
Displaying Resource Manager for the Entire Server Farm
This view enables you to monitor all the servers in your server farm. You can gain an overall picture of the status of the server farm and spot problems as they occur. To display Resource Manager for the entire server farm 1. In the left pane of the Console, click Servers. 2. In the right pane, click the Resource Manager tab.
Displaying Resource Manager for a Single Server
To display Resource Manager for a single server 1. In the left pane of the Console, navigate to the required server. 2. In the right pane, click the Resource Manager tab. This displays all the metrics that are being monitored for that server. If a problem arises, a status icon appears to warn you.
Displaying the Applications in a Server Farm
To see which applications are available for monitoring in a server farm In the left pane of the Console, expand the Applications folder. You can use Resource Manager to monitor all published applications that are running on Resource Manager servers in the server farm. When you start using Resource Manager, all existing published applications are listed in the Applications folder. You can also monitor applications that are not published by setting them up as Resource Manager applications, and identifying the servers on which you want to monitor them. For details of how to set up Resource Manager applications, see the Help. When you set up a Resource Manager application, it is added to the list in the Applications folder. Refer to “Monitoring Your Farm” on page 35.
30
Resource Manager Administrator’s Guide
Go to Document Center
Changing the Location of Resource Manager After Installation
If necessary, you can change the location of Resource Manager after installation. Before changing the location of the Resource Manager files, you must ensure that you set the correct permissions so that the Independent Management Architecture (IMA) service can read and write files at the new location. CAUTION This procedure requires you to edit the registry. Using Registry Editor incorrectly can cause serious problems that may require you to reinstall your operating system. Citrix cannot guarantee that problems resulting from the incorrect use of Registry Editor can be solved. Use Registry Editor at your own risk. To change the location of Resource Manager 1. Stop the IMA service. See the Help for more information on how to do this. 2. Move the Resource Manager folder and all its contents to the new location. 3. Edit the registry key InstallDir in HKEY_LOCAL_MACHINE\SOFTWARE\Citrix\Citrix Resource Manager 4. Restart the IMA service. Note If you run the uninstaller after manually changing the location of Resource Manager, as described here, the uninstaller does not remove the Resource Manager folder, and you must delete it manually.
Go to Document Center
CHAPTER 3
Monitoring Servers and Applications in Real Time
Overview
Resource Manager provides you with information about a number of system and network processes and events. Status displays show this information in real time, enabling you to see the state of your system at a glance. You can monitor the following: • • The status of the servers in a server farm The number of instances of specific applications that are running in a server farm
Each item that is being monitored is referred to as a metric. A metric is a combination of: • • • The type of object that you want to monitor: a physical or logical system resource, for example a computer's hard disk. The counter to be monitored: the specific aspect of the object that you want to monitor, for example disk free space. The instance of the object: an individual example of the object or a state it needs to reach in order to be counted. For example, a computer may have more than one hard disk. In this case, the instance identifies which disk you want to examine.
When you install Resource Manager, a default set of metrics is configured automatically for each server. You can change the metrics to suit your specific environment. A set of default limits is also configured during installation for the metrics that apply to each server. You can change these limits to suit your needs.
32
Resource Manager Administrator’s Guide
Go to Document Center
When a metric’s value exceeds its defined limits, Resource Manager displays a warning or problem status icon for the metric. These are known as alarms.You can also configure Resource Manager to send messages to notify you of warnings and problems. These are known as alerts. Each server with Resource Manager installed has a Microsoft Jet Access database, in which it stores metric values and application information for the last 96 hours. By default, this database is located in: ...\Citrix Resource Manager\LocalDB\RMLocalDatabase. It is accessed when you are creating real-time graphs, displaying Server Snapshot reports, and running reports on that specific server. The IMA service reads and writes to the database periodically. The size of the database is managed automatically. When the IMA service is started on a server, the local Resource Manager database is automatically compacted every day. You can also use the Management Console for MetaFrame Access Suite to display performance metrics for the servers in a farm in a highly visual way, as described in “Extended Real-Time Monitoring Capabilities” on page 13. For further information, see the assistance provided on the Access Suite Console screens.
Configuring Metrics on Servers
When you install Resource Manager, it automatically configures a default set of metrics for each server. These default metrics are described in Appendix A. Resource Manager metrics are derived from Microsoft Windows Performance Monitor; Resource Manager can track any Windows Performance Monitor counter as a server metric. For explanations of the metrics and advice on customizing metrics, refer to Appendix C of the MetaFrame Presentation Server Administrator’s Guide and the Microsoft Windows Help for Performance Monitor. Resource Manager configures default alarm thresholds for the default metrics. Citrix recommends that you customize these metrics over time to suit your environment, and that you limit the total number of metrics being tracked on a server to 50. The default metrics provide a real-time overview of each server but, to performance tune each server, you can add more specific metrics. This set of metrics is, by default, recorded in the summary database if one is configured. For further information on how to configure metrics, see the Help.
![]() ![]() ![]() ![]() ![]() ![]() Go to Document Center
Chapter 3 Monitoring Servers and Applications in Real Time
33
Checking the Status of Server Metrics
For each server in your system, status icons show the status of each monitored condition or metric. During installation, Resource Manager automatically configures a set of limits for the metrics that apply to each server. If a metric’s value falls outside normal limits for a particular length of time (a tolerance time that you can configure), the status icons change. You may need to alter these limits to suit your specific environment. You can set alerts to be triggered when either yellow (warning) or red (problem) status occurs, and also when an alert state changes back to green (OK). See the Help for more information on setting metric limits. You can also set alerts to be sent when the status of a metric returns to green (OK) or yellow, having been previously at yellow or red. The meaning of each status icon is described below:
Represents normal operation; that is, the value of the metric falls between the set limits. Represents a warning condition. This means that a problem may be developing that will require further analysis to improve performance or to prevent the situation from becoming worse. Represents a problem condition. This often means that some action is required to provide better application or server performance. Both yellow and red indicators occur when the value for a metric falls outside the normal limits and remains there for a defined period of time. Represents a metric that is not yet active, and needs to be configured. Not Configured Sleep Represents a metric that is set to “Sleep”; that is, you indefinitely suspended notification of the metric’s status. See “Suspending Notification of a Metric’s Status” on page 36 for details. This icon is also used (for all metrics) if the server is unlicensed. Represents a metric that is set to “Snooze”; that is, you suspended notification of the metric’s status for a fixed period. See “Suspending Notification of a Metric’s Status” on page 36 for details.
OK Warning
Critical
Snooze
Resource Manager determines the status of each metric by sampling the raw performance data every 15 seconds, and updates metric status icons accordingly. The history of metrics being monitored on a server is recorded in the Resource Manager server log. For details of how to view the log, see the Help.
34
Resource Manager Administrator’s Guide
Go to Document Center
Customizing Server Metrics
If you need to change the way in which Resource Manager monitors the servers in real time, you can: • • • Change the list of metrics being monitored for a server Configure the way alerts work for individual metrics Change the list of processes that Resource Manager does not monitor
If you need to configure a new metric, you can display a graph of the current values for that metric using the Visual Threshold Configuration option, and use this to help you set the appropriate thresholds. You can also change the server that deals with farm-wide metrics. In the server farm, the Farm Metric Server interprets metrics that apply to the entire server farm (such as application counts), and sends alerts if necessary.
Monitoring the Disk on a Server with Windows 2000 or Later installed
By default, Resource Manager monitors the LogicalDisk performance object on Windows 2000 Server machines. Some installations of Windows 2000 Server, without Windows 2000 Service Pack 1 installed, do not include the LogicalDisk performance object. If you have this type of installation, no disk monitoring metric is included in the default list of metrics. You can address this in two possible ways: • You can monitor the PhysicalDisk performance object instead, by adding appropriate PhysicalDisk metrics using Resource Manager. This means that only the physical drive is monitored, not each partition, and % Disk Free Space is not monitored. You can install Windows 2000 Service Pack 1, which enables you to monitor the LogicalDisk object. This is preferable, because you can then also monitor the physical drive, partition, and % Disk Free Space.
•
Go to Document Center
Chapter 3 Monitoring Servers and Applications in Real Time
35
Configuring Application Metrics
Resource Manager monitors only one metric for each application. This is the Count metric, which maintains a count of how many instances of specific applications are running in the server farm, and notifies you if the number of instances of a monitored application reaches a defined limit. This enables you to manage application licenses. You can monitor all MetaFrame Presentation Server published applications. You can also monitor applications that have not been explicitly published, for example, because the desktop itself is published, or because you are not deploying the application through MetaFrame Presentation Server. To do this, you need to set up the application as a Resource Manager application. See the Help for more information. Resource Manager can monitor a published application only if you specified the full path name of the application in the Properties dialog box when you published the application in MetaFrame Presentation Server. When you publish an application, it is a good idea to use the Browse button to select the executable, to ensure that you use the correct full path. For full information on publishing applications, refer to the MetaFrame Presentation Server Administrator’s Guide. If an application path name also specifies command-line parameters for the program, surround the path name with double quotes (“ ”). This enables Resource Manager to distinguish between process name and argument list and monitor the published application correctly. Important You can only use Resource Manager to keep track of the usage of 32-bit applications. You cannot monitor 16-bit applications.
Monitoring Your Farm
Resource Manager enables you to monitor the status of servers and applications in your server farm in a number of ways. You can organize servers and applications in the server farm into folders and use Resource Manager to monitor each folder as a unit. You can then see an overview of the servers or applications in the folder, as well as an overview of the entire server farm. See the Help for more information on real-time monitoring. At times when you do not want to show the full status display on the screen, you can monitor servers or server folders by displaying a smaller window, called the Watcher Window.
36
Resource Manager Administrator’s Guide
Go to Document Center
Resource Manager can send alert messages to notify you under the following conditions: • • A server in the server farm unexpectedly stops operating The status of selected metrics changes: • • • • Up to red Up to yellow Down to yellow Down to green
Alerts are sent by email, SNMP message, SMS message, or more than one of these formats. The Farm Metric Server interprets metrics that apply to the entire server farm (for example, application counts) and sends alerts when required for them. Alerts are generated whenever the IMA service stops operating, including expected events such as scheduled reboots.
Suspending Notification of a Metric’s Status
You can stop Resource Manager giving you information about a specific metric. This is useful when, for example, you want to work on a problem without receiving repeated alarms. When you suspend notification, Resource Manager continues recording information about the metric's values but does not display alarms or send alerts. See the Help for more information.
Preparing Your System for Resource Manager Alerts
You can use Resource Manager to send alert messages when metrics change state. The alerts can be sent using one or more of the following methods: • • • Email messages, using MAPI or SMTP Short Message Service (SMS) text messages to cell/mobile phones Simple Network Management Protocol (SNMP) messages
To use Resource Manager to send alerts, you need to set up one or more Resource Manager servers to send the alert messages. You need to make sure that these servers have additional hardware or software to handle each type of alert you require; for example, a modem for SMS alerts and an email system for MAPI alerts. For SNMP alerts, you need to set up SNMP on every server that has one or more metrics configured to send SNMP alerts.
Go to Document Center
Chapter 3 Monitoring Servers and Applications in Real Time
37
When you have done this, you can configure settings in Resource Manager, such as who will receive the alert messages, and set up the individual server and application metrics that you want to trigger the alert messages. The alert recipients you set up will be used for all the servers in the server farm. You can modify Resource Manager alert recipients for any individual server. If you have delegated responsibility for a set of servers to a custom administrator, you may wish to prevent that administrator from receiving alerts from all the other servers in the farm. You do this by setting up the administrator’s alert contact details in the Add MetaFrame Presentation Server Administrator wizard. For each server folder you can specify whether or not the custom administrator will receive alerts for the servers in that folder. For further details, see the Help provided with the wizard.
Preparing Your System for Email Alerts
Before you can use email alerts, you must configure your system. You can use either MAPI or SMTP to generate email alerts. Select the relevant setting in the Resource Manager Properties dialog box. By default MAPI is used to send alerts. • For MAPI email alerts, you first need to choose and configure the servers in the server farm that you want to use to send the email messages. Then you create a mail profile and enable the Resource Manager MAPI email service. These servers are called MAPI Connection Servers. For SMTP email alerts, you must have an SMTP email server that is accessible from your server farm. You then specify the details of your SMTP server in Resource Manager. See the Help for more information. You can secure communication between your Resource Manager servers and your SMTP server using SSL. For details of how to set up SSL, see “Setting up SSL for SMTP Email Alerts” on page 39.
•
38
Resource Manager Administrator’s Guide
Go to Document Center
Using MAPI to Send Alerts
Selecting the MAPI Connection Servers
You need to select one or more servers in the server farm to be the MAPI Connection Servers. When email alerts are generated in the farm, they are passed to one of the configured MAPI servers, which then sends the actual email. You can configure as many MAPI Connection Servers as you wish. Alerts are sent through a randomly selected MAPI server, although if the server on which an alert is generated is also a MAPI server, that server sends the email. Each MAPI Connection Server must be able to access a mail server (for example, Microsoft Exchange Server). It must have an email client installed (for example, Microsoft Outlook) that conforms to the X-400 protocols.
Creating a Mail Profile for Resource Manager
On each MAPI Connection Server, you need to configure a mail profile for Resource Manager to use. The profile must have the same name and details on all of your MAPI Connection Servers. Citrix recommends that you give the profile a name that is easy to recognize (for example, Resource Manager). The profile is used throughout the server farm. When you create the mail profile, ensure that you include the mail system that you want to use; for example, Microsoft Exchange Server. You can also specify an address book for the profile. When you have set up the profile, it is a good idea to test that you can log on to your email system using the profile and that you can send a message. Note A user who is configured to use the email profile can log on to the email system without being prompted for logon credentials. For more information on configuring email profiles, refer to Citrix Knowledge Base article CTX333658 available at http://knowledgebase.citrix.com.
Enabling the Resource Manager MAPI Mail Service
Email alerts are managed by a service called Resource Manager Mail. This service is installed automatically on all servers on which you install Resource Manager. You need to enable the service on each MAPI Connection Server. To enable the Resource Manager Mail Service on a MAPI Connection Server 1. Open the Windows Control Panel. 2. Open the Services dialog box. To do this, open Administrative Tools, then open Services.
Go to Document Center
Chapter 3 Monitoring Servers and Applications in Real Time
39
3. In the Properties dialog box for the Resource Manager Mail service, click the General tab and ensure that the startup type for the service is set to Automatic. 4. Click the Log On tab and select This account. 5. Enter the details of the local user account, including the domain, that you want Resource Manager to use for email alerts. Ensure that you type the account and domain details exactly, or browse to the account so that you can be sure that the details you enter are correct. 6. Ensure that the Resource Manager Mail service is started.
Configuring Resource Manager to Use MAPI Email Alerts
After you set up the MAPI Connection Servers, you must set up Resource Manager to use email alerts for the server farm. For example, you need to specify the alert recipients. These settings apply to the entire server farm. See the Help for more information on MAPI email alerts.
Setting up SSL for SMTP Email Alerts
Important This section assumes that you have access to a certificate authority and an appropriate root certificate, and are familiar with the procedure of adding a certification path to a Windows server. You must also ensure that your SMTP server supports SSL, and is properly configured to use SSL in your network environment. 1. Install the certification path of your certificate authority on each Resource Manager server. In the Microsoft Management Console, add the root certificate path in the following location: Certificates (Local Computer)\Trusted Root Certification Authorities\Certificates. This step enables each server to trust certificates issued from your certificate authority. 2. In the left pane of the Presentation Server Console, click Resource Manager. In the right pane, click E-mail. In the E-mail tab: • Specify the SMTP server by typing its fully qualified domain name (FQDN). For example: smtpserver1.mydepartment.mycompany.com. Click Use SSL to send e-mail alerts. Click OK.
• •
40
Resource Manager Administrator’s Guide
Go to Document Center
Preparing Your System for SMS Alerts
To send an SMS message to a cell/mobile phone, a Resource Manager server makes a call through its modem to a number that is designated by the cell/mobile service provider. It then sends data to the service provider's computer, instructing it to send an SMS message to the cell/mobile phone of the person who is to receive the alert. If you want to use SMS alerts, ensure that at least one Resource Manager server in the server farm has a modem. This can be an analog modem or an ISDN card. You need to investigate the modem requirements of the service providers for the cell/mobile phones to which you want to send alerts. Some service providers require a specific type of modem (usually analog). Where this is the case, at least one server with that type of modem must be in the server farm before you can use SMS alerts for that service provider. If the people that you want to receive SMS alerts use a variety of service providers, you need to know the details of the gateway that Resource Manager must use to communicate with each service provider. Each provider is likely to have a different telephone number, and may employ a different protocol to carry the messages. Some service providers offer an analog line, others offer ISDN. You probably need to configure a range of numbers to call and a range of protocols to use. The alert recipients you set up will be used for all the servers in the server farm. You can modify Resource Manager alert recipients for any individual server.
Configuring Resource Manager to Use SMS
When you are sure that you have fulfilled all the requirements for SMS alerts, you can configure the way in which Resource Manager uses SMS for alerts in the server farm. For example, you need to specify the alert recipients. These settings apply to the entire server farm. You can also specify SMS alert recipients for individual servers.You need to select one or more servers in the server farm to send the SMS alerts. Such a server is called a TAPI Server. For more information on configuring Resource Manager to use SMS, see the Help.
Preparing Your System for SNMP Alerts
Resource Manager can send five different SNMP alerts: • • • trapServerDown - The Resource Manager server is down trapMetrictoGreen - The metric on the Resource Manager server has changed to green status trapMetricGreenToYellow - The metric on the Resource Manager server has changed from green to yellow status
Go to Document Center
Chapter 3 Monitoring Servers and Applications in Real Time
41
• •
trapMetricRedtoYellow - The metric on the Resource Manager server has changed from red to yellow status trapMetricToRed - The metric on the Resource Manager server has changed to red status
To receive SNMP messages, a computer on a network requires an SNMP management tool that enables it to “listen” for messages. A number of third-party commercial tools are available. You need to install and set up the Windows 2000 or Windows Server 2003 SNMP service on every server that has a metric configured to send SNMP alerts. If you want Resource Manager to send alerts for the Count metric on any applications that are running in the server farm, ensure that the Farm Metric Server has the Windows SNMP service enabled and running.
Getting More Information About Metrics and Monitoring
Resource Manager uses server performance and resource metrics derived from the Microsoft Windows Performance Monitor. For explanations of the metrics and advice on customizing metrics, refer to Appendix C of the MetaFrame Presentation Server Administrator’s Guide and the Microsoft Windows Help for Performance Monitor.
Go to Document Center
CHAPTER 4
Recording the History of Servers and Applications
Overview
This chapter explains how you can use Resource Manager to store details of server performance, application instances, and resource usage in a summary database. Topics include: • • • How Resource Manager information gets into a summary database What information you should record in a summary database Scheduling data collection for a summary database
For information on how to set up a summary database, see “Setting Up a Summary Database” on page 21.
How Does Resource Manager Information get into a Summary Database?
Each Resource Manager server creates a summarized version of its daily activity. This information is known as summary data. There are various types of summary data: • • • • Server-specific performance metrics Server-specific session information Farm-wide application metrics Farm-wide server events (for example, server-down)
Farm-wide metric and server event information is generated as summary data by the Farm Metric Server in addition to its own server-specific information and metrics. Farm-wide metrics are routed from servers to the Farm Metric Server through the zone data collector.
![]() ![]() ![]() ![]() ![]() ![]() 44
Resource Manager Administrator’s Guide
Go to Document Center
Summary data is kept in special temporary summary files that are stored locally on each server in a database-compatible format. Each hour, Resource Manager adds the summary data gathered over the previous hour to the summary files. Summary files are stored in the \\Program Files\Citrix\Citrix Resource Manager\SummaryFiles folder. On a daily basis, the summary data held by each server in the server farm is collected by the Database Connection Server. The Database Connection Server then updates the summary database. After the summary database has been updated, summary files are overwritten with new data. The following diagram represents a typical server farm utilizing the Resource Manager summary database. It shows the flow of both server-specific and farmwide summary data from the farm servers to the summary database through the Database Connection Server.
Server Farm Farm Metric Server
An Application
An application example for the purposes of demonstrating the having a Summary Database.
An Application
An application example for the purposes of demonstrating the having a Summary Database.
Database Connection Server Server
External Database
An Application
An application example for the purposes of demonstrating the having a Summary Database.
Server
Server
Legend
An Application
An application example for the purposes of demonstrating the having a Summary Database.
Farm-wide metrics Server-specific metrics Farm-wide metrics collated on the Farm Metric Server continuously (via zone data collector) Hourly summary data sent to Database Connection Server once per day Daily summary data
A Diagram Showing a Server Farm Using a Summary Database
Go to Document Center
Chapter 4 Recording the History of Servers and Applications
45
What Information Should I Record in the Database?
Resource Manager servers, by default, automatically record process-related and session-related information and server events in the summary database. The scope and detail of this information cannot be changed. A number of default metrics are also included in the database when you install Resource Manager. You can modify the default metric set or specify your own for individual or multiple servers. Note When you make changes to the summary database configuration, it can take up to 10 minutes for other Presentation Server Consoles in the farm to be updated with the new settings.
Issues to Consider when Selecting Database Information
You need to consider some important issues when selecting information to be stored in your database. These are outlined below.
The Benefits of Keeping Long-Term Resource Manager Information
Store information that will be useful; for example, if you are billing users for RAM usage, you can store process information until you create bills for it. It is a waste of summary database space to store information that is of little significance for your server farm. For example, there is little point recording the amount of application usage on a server if you don’t care what specific applications are being used.
Deciding Which Metrics Information to Store
You need to decide what metric information to store for different servers in your server farm. This is important if you have several servers contributing differing metric information to the database because it may confuse an assessment of the server farm as a whole when using summary database reports. Consider the following scenario: An administrator has selected the “LogicalDisk - % Free Space” metric to be recorded in the database for half of the servers in the server farm. At a later date, an administrator generates a report from the summary database to find the servers most in need of hard drive upgrades (least hard disk free space). Because half of the servers in the server farm are not storing this metric in the database, the administrator does not get an accurate report of the overall state of the server farm for the metric.
46
Resource Manager Administrator’s Guide
Go to Document Center
How Can Recording a Metric Affect Database Growth?
You need to consider how quickly your summary database will grow when storing your chosen metric information, and the amount of hard disk space available to the DBMS. The larger your server farm, and the more information you store, the faster your database will grow. For more information about estimating the growth of the database, see “Estimating Summary Database Growth” on page 53. To help you manage database growth, you can remove unwanted data using a purging schedule. Database purging automatically deletes records older than a specified age. See “Removing Unwanted Information from the Database” on page 47 for details.
Selecting Server and Application Metrics to Record in the Database
Whenever you add a new metric, it is automatically set to be summarized. Refer to Chapter 3 for details about how to set and use metrics. You can select server or application metrics for individual servers or applications, or use one setting for multiple servers or applications. See the Help for more information.
Using Farm-Wide Options for Reducing Summary Data
Resource Manager enables you to reduce the amount of farm-wide summary data being generated. You can use the Collection Restrictions option described in the Help to create summary data only for processes connected to, or part of, MetaFrame Presentation Server published applications or Resource Manager applications. Note Collection restrictions may affect resource billing because process data that is not stored in the summary database cannot be billed.
Go to Document Center
Chapter 4 Recording the History of Servers and Applications
47
Scheduling Summary Data Collection and Removal
Resource Manager gives you control over several important aspects of maintaining historical records in your summary database. These include: • • • Ignoring periods of low server activity Automatically deleting records from the summary database by scheduling their removal after they are stored for a specified time Setting when you want the Database Connection Server to update the summary database with the day’s summary data
The detailed procedures to follow if you want to carry out these tasks are described in the Help. The following sections provide a general overview of the tasks.
Ignoring Server Metrics During Periods of Low Server Activity
You can configure Resource Manager to ignore server metrics during periods of low server activity; for example, over weekends or late at night, where metric collection may be of little benefit and would increase database size unnecessarily. You can exclude server metrics from summary data on specific days of the week and/or during specific periods during each day. Alternatively, if you need to capture data about business processes such as overnight backups, you may prefer to set up continuous data capture. Note that session, process, application, and server event information is always recorded regardless of any settings you make. You can schedule server metric summary data collection for individual servers or use one setting for multiple servers. Any previous settings for individual servers are replaced with the new settings.
Removing Unwanted Information from the Database
Over time, your database will grow in size as it stores summary data for the server farm. Records you store in the database are kept indefinitely, by default, which can lead to a large database in a short period of time. The greater the number of servers contributing to the database, and the greater the amount of summary data being stored for each server, the faster the database will grow. You may want to keep some of the records stored in the database only for a certain length of time. For example: • If you record the percentage of CPU interrupt time so you can assess potential hardware problems or server overloading on a monthly basis, you may not require the information after assessment
48
Resource Manager Administrator’s Guide
Go to Document Center
•
If you record session and process information on your servers so you can bill users for their usage time, you may not want to keep the records after the bill is created
You can remove unwanted data from the database using a purging schedule. Purging automatically deletes records from the database after they are there for a specified period of time. You can also configure data so that it can be purged only after a bill is created for it. You can configure database purging using the following record type options: • Events • Metrics • Sessions/processes (billed) • Sessions/processes (not billed)
Setting a Purging Schedule
When you configure your purging schedule, you must specify how long the data is to be kept before being purged. The age of a piece of data is calculated by subtracting its end time from the current Database Connection Server time. Retention periods are set to “Indefinite” upon initial setup. The Indefinite setting will never purge the associated data. Important Ensure that the operating system time and date on the originating Resource Manager servers and the Database Connection Server are synchronized. This prevents data from being purged incorrectly; for example, if one of the server farm servers has the date set two days behind that of the Database Connection Server, the data from it is purged two days earlier than expected.
Setting the Summary Database Update Time
The Database Connection Server automatically updates the summary database with summary data from each Resource Manager server once per day. This is referred to as the update time. The default update time on setup is 00:00 hours (midnight). If this is inconvenient, change this to a time of day when server activity is low to prevent slow data transferal or interference with normal server farm activities. During an update, each server in the server farm first sends a request to the Database Connection Server, asking for permission to send its summary data for the day. After accepting the request, the Database Connection Server receives the server’s summary data, and then updates the summary database.
![]() Go to Document Center
Chapter 4 Recording the History of Servers and Applications
49
Note The update time is always interpreted in the time zone local to each server. Servers in different time zones will request to send their summary data at the update time in their local time zone. You can perform manual updates independently of the update time. You may want to do this, for example, if you want to generate reports on a fresh set of information. If an update is unsuccessful, this is reported on the Summary Database tab, and an alert is sent. You can also temporarily “Sleep” the Database Connection Server to stop the database from being updated. You may want to do this to perform maintenance on the database.
Monitoring the Status of the Summary Database
You can easily check the status of the summary database using the Summary Database tab. To view the summary database status and run-time activity icons 1. In the left pane of the Presentation Server Console, click Resource Manager. 2. In the right pane, click the Summary Database tab. The Status area includes indicators showing the current Database Connection Server, Farm Metric Servers, run-time process, and database import states.
Screenshot of the Summary Database Status Panel
![]() ![]() ![]() ![]() ![]() ![]() 50
Resource Manager Administrator’s Guide
Go to Document Center
The meanings of the status icons for the Database Connection Server On/Off/ Configuration indicator are:
The summary database is off or a Database Connection Server is not configured. In this state, no Resource Manager servers are creating and storing summary data for inclusion in the database. The summary database is on and the Database Connection Server is correctly configured. In this state, Resource Manager servers are creating and storing summary data for inclusion in the database. The summary database software version for the Database Connection Server is not accepted because another server in the server farm has a later version of the summary database software installed. You need to upgrade the software on the Database Connection Server. See Chapter 2, “Installing Resource Manager” for details. Note: This situation can arise only when there are mixed releases of the summary database software within a server farm.
Not Configured OK
Critical
The meanings of the status icons for the Farm Metric Server indicator are:
The primary Farm Metric Server is active and has an accepted version of the summary database software installed. The summary database software version for the backup Farm Metric Server is not accepted because another server in the server farm has a later version of the summary database software installed. You need to upgrade the software on the backup Farm Metric Server. See Chapter 2, “Installing Resource Manager” for details. Note: This situation can arise only when there are mixed releases of the summary database software within a server farm. The summary database software version for the primary Farm Metric Server is not accepted because another server in the server farm has a later version of the summary database software installed. You need to upgrade the software on the primary Farm Metric Server. See Chapter 2, “Installing Resource Manager” for details. Note: This situation can arise only when there are mixed releases of the summary database software within a server farm.
OK Warning
Critical
![]() ![]() ![]() ![]() ![]() ![]() ![]() ![]() Go to Document Center
Chapter 4 Recording the History of Servers and Applications
51
The meanings of the status icons for the Run-time indicator are:
The Database Connection Server is currently updating the database without error. A connection problem between the Database Connection Server and the DBMS that the summary database is on has occurred during a database update. Ensure the Database Connection Server user identification and password for the DBMS is correct. See Chapter 2, “Installing Resource Manager” for details. Use the Server Log for the Database Connection Server (click View Server Log) for further error information. The summary database is off or a Database Connection Server is not configured. In this state, no Resource Manager servers are creating and storing summary data for inclusion in the database. Automatic database updates have been temporarily stopped. This is known as Sleep mode. See “Temporarily stopping database updates in the Resource Manager” online Help for details. The Database Connection Server is in an idle state between database updates.
OK Critical
Not Configured
Sleep Snooze
The meanings of the status icons for the Failed Import indicator are:
The summary database is off or a Database Connection Server is not configured. No update has therefore been attempted. Data has been successfully imported. OK An error has occurred: the import was not successful. Critical
Not Configured
52
Resource Manager Administrator’s Guide
Go to Document Center
Managing the Resource Manager Summary Database
Use the following sections to help you manage how much hard disk space your summary database will require over a period of time. The size of your summary database, as it grows over time, depends on the following basic factors: • • • • • • The number of servers in the server farm The average number of processes run on a server each day The number of Resource Manager metrics you are storing in the database The average number of server events for a server each day The length of time the database records are kept The DBMS summary database transaction log Note The transaction log maintains a history of the data transactions for the summary database. Resource Manager does not automatically purge the DBMS summary database transaction log. You need to configure how the DBMS controls the transaction log to restrict its growth. See your DBMS documentation for details. Various categories of data are written to the database for the server or server farm. These are: • • • • • Process information Server metrics Session information Application metrics Server events (server-down/server-up)
Go to Document Center
Chapter 4 Recording the History of Servers and Applications
53
Estimating Summary Database Growth
You can use the following equations to formulate an idea of how much information will be stored in the summary database per day. Note that these estimates are very approximate and each server or farm may vary considerably from the example. The following schedule of data is for a Resource Manager farm of 100 servers under typical loads. Process information • • Estimate @ 600 sessions per day (with 6 processes per session) Estimate @ 140 bytes per row in process database table
Estimated total is 600 x 140 = 84,000 bytes per server per day Server metrics • • Estimate @ 15 metrics per server (summarized at hourly intervals) Estimate @ 100 bytes per row in metrics database table
Estimated total is 15 x 100 x 24 = 36,000 bytes per server per day Session information • • Estimate @ 100 sessions per server per day Estimate @ 100 bytes per row in session database table
Estimated total is 100 x 100 = 10,000 bytes per server per day Sub Total is 84,000 + 36,000 + 10,000 = 130,000 bytes per server per day. Application metrics • • Estimate @ 20 application metrics per server (summarized at hourly intervals) Estimate @ 100 bytes per row in application metric database table
Estimated total is 20 x 100 x 24 = 48,000 bytes for the farm per day Server events • • Estimate @ 1% of farm servers restarted per day Estimate @ 20 bytes per row in event log database table
Estimated total is 1 x 20 = 20 bytes for the farm per day Sub Total is 24,000 + 20 = 24,020 bytes for the farm per day GRAND TOTAL is (84,000 + 36,000 + 10,000) x 100 + 24,020 = 13.02 MB of summarized information stored in the database per day on a 100 Resource Manager server farm.
54
Resource Manager Administrator’s Guide
Go to Document Center
Managing Summary Database Growth
As described above for a typical farm, summary data stored in your database can be substantial. Unless you manage your external database appropriately, it can grow until all available storage space is used. You need to monitor the rate at which your database is growing. Do this by regularly checking how much disk space is available. You can then use this information to help you set metrics and purge schedules for your database in order to limit growth rates. Some things you might want to do when using a summary database are to: • Regularly check the available disk space on the database host computer so you can work out an average for the amount of information being stored each day. Tip You can configure your summary database DBMS to constrain database size. See the Help for instructions on how to do this. • Regularly create reports on the information you have stored and analyze which metrics are appropriate for long-term storage and historical reporting for your system. If you find you are keeping metrics in the database unnecessarily, remove them from the list being stored. • Work out how often you need to check on resource usage for each metric. You can create reports on these items on a regular basis and set up a purging schedule to remove them from the database after you create the reports. The more regular your reporting, the sooner you can purge the database of the information.
Go to Document Center
CHAPTER 5
Reporting and Analyzing Resource Manager Information
Overview
This chapter describes the reports you can produce using data that is held on each server or in a summary database. Topics include: • • • • • Descriptions of the types of reports you can produce Generating detailed reports about current activity Generating summarized reports about past activity How reports from servers in different time zones and languages are handled Estimating the concurrent user capacity of a server
Detailed instructions on how to produce each type of report are provided in the Help. You can also use the Report Center in the Access Suite Console to produce reports from a variety of real-time and historical data sources. For further details of how to use the Report Center, see Chapter 8 and the assistance provided on the Report Center screens.
56
Resource Manager Administrator’s Guide
Go to Document Center
Generating Reports to Analyze Data
Resource Manager enables you to produce two report types: • Current: These reports are generated from Resource Manager information stored in the local database on each server and can be generated either on a per server basis or for multiple servers. Information is recorded at 15 second intervals and is referred to as real-time. Summary: These reports are generated from data stored in the summary database and can be generated either on a per server basis or for multiple servers. Summary reports are less detailed than current reports; however, they can be generated for any times within the period for which data is stored in the summary database. Summary reports can be customized to include or exclude various record types. Important Summary reports include metric information only for times when data was being stored to the summary database. If you set Resource Manager to capture metric data only for certain times for a specific server, data from outside these periods is not included. All reports are displayed in a report viewer window. Reports contain navigation links to allow you to step between the top of the report and any of the tables within it. You can also print the report, or save it in HTML or comma-separated values (CSV) format, for future reference. Note Resource Manager uses a number of HTML templates to create reports. These are located on each server in the Templates subfolder of the Resource Manager folder. To avoid inconsistencies in the reports, do not edit the templates. If your number format settings use commas as decimal separators, Resource Manager replaces them with semicolons (;) when saving reports to CSV format, because commas are used specifically in this format to separate the items of data in the file.
•
Creating Reports on Current Activity
Current reports can provide detailed information about the following: • • • Statistics about current process activity or application usage in your server farm Statistics about current user activity in your server farm The status of a server at a particular moment
Go to Document Center
Chapter 5 Reporting and Analyzing Resource Manager Information
57
Reporting on the Use of Processes or Applications
You can produce a report containing information about monitored processes in the server farm or processes that are running on a specific server. The report tells you about the resource usage of the process, the times it is active and loaded, and the user(s) running it. For example, you might use this report to examine a server's details when one or more of its metrics enter an alarm state. The level of detail shown in the report depends on the options you select when you set up the report. The report has a general information section at the top, including the date and time of report generation, the report type, and the various report options. Within the report, you can click links to navigate to the different sections. The information shown in the report is as follows: • Processes The Processes table shows details about each instance of the selected process (or all processes) being run on the server by the selected user (or all users), at the time you generate the report. The table shows the following information: Name: The name of the process. Path: The location of the process on the server. This may help you distinguish between instances. Product Date: The date from the program file. This gives an idea of when the executable was created and may help you identify the process. Type: A Microsoft Windows-defined process type code. Version: The version number that is defined within the program file. Note: This information may not be present for some programs. Start Time: The date and time at which this instance of the process was loaded on the selected server. Times are shown in the local time zone of the server being reported on. % Active: The length of time that this instance of the process was active, as a percentage of the time since it was loaded on the server. User: The user name and domain of the user that is running this instance of the process.
58
Resource Manager Administrator’s Guide
Go to Document Center
•
CPU The CPU table shows CPU usage statistics for the user(s) running the process(es) at the time you generate the report. The table shows the following information: Overall CPU Utilization Kernel mode User mode CPU Utilization while active Kernel mode User mode For information about what these categories mean, consult the documentation for the server operating system. Memory The Memory table shows memory usage statistics for the user(s) running the process(es) at the time you generate the report. The table shows the following information: Working set while active Nominal working set Peak working set Peak paged pool Peak non-paged pool Peak page file usage Page faults/sec For information about what these categories mean, consult the documentation for the server operating system. Times The Times table shows process activity statistics for the user(s) running the process(es) at the time you generate the report. The table shows the following information: Time loaded: The length of time for which the processes had been loaded on the server. Time active: The length of time for which the processes had been active. Active/loaded ratio: The length of time that the processes had been active, as a percentage of the time since they were loaded on the server. Total time loaded: The total length of time for which all the processes had been loaded. Total time active: The total length of time for which all the processes had been active.
•
•
Go to Document Center
Chapter 5 Reporting and Analyzing Resource Manager Information
59
First/last recorded use: For all the processes that were running, the earliest and latest times at which a process was started. Times are shown in the local time zone of the server being reported on. • Users If you generate the report for all users, the Users table shows the user name and domain of all the users running the processes, and the servers on which they are running, at the time you generate the report. Remember, if you generated the report for a single user, then the report only includes that user.
Reporting on User Activity
You can use Resource Manager to provide information about users who have active sessions on a specific server at the time you generate the report. The user report tells you about the users' sessions and the processes they are running. Sessions and processes are shown as different sections in the report, and are described below. The report has a general information section at the top, including the date and time of report generation, the report type, and the various selections made when the report was set up. Within the report, you can click links to navigate to the different sections. The information shown in the report is as follows: • Sessions The Sessions table lists all the sessions that are being run by the selected user(s) on the selected server at the time you generate the report. The table shows the following information: User: The ID and domain of the user who is running the session. Session Name: The name of the session. Protocol: The protocol that was used for the session: for example, TCP/IP. Start Time: The date and time at which the session was started. Times are shown in the local time zone of the server being reported on. Duration: How long the session has been running. Process Count: The number of processes that are running in the session. This information is expanded in the Processes table further down the report. Processes The Processes table shows a complete list of all the processes, and the number of instances of each, run by the selected users on the selected server. The processes are listed in order of start time, giving you a picture of what the user has been doing most recently.
•
60
Resource Manager Administrator’s Guide
Go to Document Center
Looking Back to a Specific Time on a Server
If there is a problem on a particular server, you can produce a Server Snapshot report, showing its status at the time the problem occurred. You can then use this report to evaluate why the problem happened. The report tells you about users and processes utilizing the server, presents information on monitored metrics, and can be stepped back to any time within the last 96 hours. You can also generate Server Snapshot reports from the real-time graph of a server metric. See the Help for more information. Within the report, you can click links to navigate to the different sections. The information shown in the report is as follows: • Processes over 5% CPU or Memory This table shows any processes that use more than 5% of the CPU load or memory. • Users and Processes This table shows the users and the processes they ran on the selected server. The table shows the following information: User: The user name and domain of each user who had an active session on the server. Process: The name of each process that the user ran on the server. There is a separate entry for each instance of the process. Path: The location of each process instance on the server. This may help you distinguish between instances. Version: For each instance, the version number that is defined within the program file. This information may not be present for some programs. Metrics The Metrics table gives information about every metric that was monitored on the server over the 15 second period. The table shows the following information: Object: This is the category being monitored. It is a physical or logical system resource: for example, a computer's hard disk. Note The Terminal Services object is returned from Windows Terminal Server. The Inactive Sessions counter for this object includes idle sessions.
•
Go to Document Center
Chapter 5 Reporting and Analyzing Resource Manager Information
61
Instance: An individual example of the object or a state it needs to reach in order to be counted. For example, a computer may have more than one hard disk. In this case, the instance would identify which disk is being examined. Counter: This is the specific aspect of the object being monitored. For example, disk free space. Time: The date and time at which the operating system last recorded the metric. Dates are shown in dd/mm/yyyy format; times are shown in the local time zone of the server being reported on. Value: The value of the metric at the time of sampling. Some metrics are average values.
Creating Reports on Past Activity
Summary reports can provide summarized information about the following: • • • Statistics about past process activity in your server farm Statistics about past user activity in your server farm The status of a server for a specified one hour period
Important Generating more than two summary reports at a time will overburden most systems. You can also generate reports from the summary database using an external package such as Crystal Reports. To help you do this, the database schema used by the summary database is described in Appendix B. Citrix provides several Crystal Reports templates that you can use. These templates are available in several languages, and are available for download from http://www.citrix.com/download/.
Looking Back at Specific Processor Application Usage
You can use Resource Manager to provide information about the resource usage of a process, users who have run it, and on what servers it ran over a selected period. The level of detail shown in the report depends on the options you select when you set up the report. Remember, if you select a specific server and/or user, the report includes only the information related to that server or user. The options you choose depend on why you are generating the report. For example, you might include the CPU and memory statistics because you want to get an indication of the load that a program imposes on the server. Within the report, you can click links to navigate to the different sections. The information shown in the report is as follows:
62
Resource Manager Administrator’s Guide
Go to Document Center
•
CPU For processes that have finished, the CPU table shows CPU usage statistics for the user(s) who ran the selected process and the server(s) on which it ran. The table shows the following information: Overall CPU Utilization Kernel mode User mode CPU Utilization while active Kernel mode User mode For information about what these categories mean, consult the documentation for the server operating system. Memory For processes that have finished, the memory table shows memory usage statistics for the user(s) who ran the process and the server(s) on which it ran. The table shows the following information: Working set while active Nominal working set Peak working set Peak paged pool Peak non-paged pool Peak page file usage Page faults/sec For information about what these categories mean, consult the documentation for the server operating system. Times The Times table shows process activity statistics for the user(s) who ran the process and the server(s) on which it ran. The table shows the following information: Time loaded: The length of time for which the process was loaded on the server. Time active: The length of time for which the process was actively operating. Active/loaded ratio: The length of time that the process was active, as a percentage of the length of time that it was loaded on the server. Total time loaded: The total length of time for which the process was loaded, for all its different locations and versions.
•
•
Go to Document Center
Chapter 5 Reporting and Analyzing Resource Manager Information
63
Total time active: The total length of time for which the process was active, for all its different locations and versions. First/last recorded use: The earliest and latest times at which the process was started. Times are shown in the local time zone of the server requesting the report. • Users The Users table shows the user name and domain of all the user(s) who ran the process and the server(s) on which it ran. Remember, if you generate the report for a single user/server, then the report only includes that user/server. Servers The Servers table shows a list of all the servers on which the user executed the selected process. If you generate the report for a single server, this section of the report is omitted.
•
Looking Back at Specific Users’ Activities
You can use Resource Manager to provide information about a user's resource usage, sessions, and the processes used and on what servers, over a selected period. The level of detail shown in the report depends on the options you select when you set up the report. For example, you might use this report when you want to examine the activity of a specific user; for example, to analyze work patterns. Within the report, you can click links to navigate to the different sections. The following information is shown in the report: • Session Summary The Session Summary table lists all the selected user's sessions over the specified report period, on the selected server(s), and for the selected process(es). The table shows the following information: Start Time: The date and time at which the session started. Times are shown in the local time zone of the Console requesting the report. End Time: The date and time at which the session finished. Times are shown in the local time zone of the Console requesting the report. This area remains blank if the session was still running when the summary database was last updated. Duration: The length of time for which the session was run. This area remains blank if the session was still running when the summary database was last updated. Server: The server on which the session was run. Client: The name of the client machine. Where the user was using the server itself rather than a separate client device, the client machine name is shown as “Console.”
64
Resource Manager Administrator’s Guide
Go to Document Center
Published Application: If the user connected to any published applications, this column shows the names of the applications. Winstation: The Winstation or Sessionname for the session. Protocol: The protocol used for the session; for example, ICA. Process Count: The total number of processes that were run during the session. Processes: A list of the unique process names that were run during the session. For example, if the Process Count is shown as 6 and there is just one process name in the Processes column, that process was run six times. If you select the Process Summary option under Report Options when you generate the report, you will see more detailed information about each process in a Process Summary table. • Favorite Processes The Favorite Processes table lists the top 10 processes run by the selected user, on the selected server(s), and of the selected process(es). The processes are listed in descending order of most frequent use. The table shows the following information: Count: The number of times the user ran the process. Process: The name of the process. Time Loaded: The total length of time that the user ran the process. Time Active: The total length of time for which the process was actively operating (as opposed to just being loaded). % Active: The total length of time for which the process was active, as a percentage of the total time for which it was loaded. Last Use: The date and time at which the user most recently ran the process. Process Summary The Process Summary table shows a complete list of all the processes (or the selected process, if you selected a single process) run by the selected user, within the selected report period, on the selected server(s). The processes are listed in order of start time, giving a picture of what the user was doing at specific times. The table shows the following information: Start Time: The date and time at which the user started the process. Times are shown in the local time zone of the Console requesting the report. End Time: The date and time at which the process terminated. Times are shown in the local time zone of the Console requesting the report. This area remains blank if the session was still running when the summary database was last updated. Server: The server on which the user ran the process. Process Name: The name of the process.
•
Go to Document Center
Chapter 5 Reporting and Analyzing Resource Manager Information
65
Exit Code: The exit code returned for the process, indicating the status of the process when it terminated. Consult the software vendor for more information about the exit codes that are used for specific processes. % Active: The total length of time for which the process was active, as a percentage of the total time for which it was loaded. • Statistics The Statistics table shows general statistics about sessions run by the user on the selected server(s), and for the selected process(es). The table shows the following information: Session duration: The duration of all completed sessions that the user ran. The table shows the average, minimum, and maximum session duration, and the total duration of all sessions. Session duration per day: The duration of all completed sessions that the user ran, per working day. Note Working days are defined as Monday to Friday. Sessions per day: The number of sessions run per working day. Processes per day: The number of processes run per working day. Processes per session: The number of processes per session calculated from the process count in the Session Summary table, against all the sessions that the user ran. • Servers The Servers table shows a list of all the servers on which the user executed one or more processes, and the number of processes the user ran. Remember, if you have generated the report for a single server, the report includes information only for that server.
Looking Back at Activities on a Specific Server
Server Summary reports produce statistical information for a particular server for a one hour period. You can use this report to: • • See users and associated process activity for a farm server, including the process path and version. See the metrics saved to the external database for a server, including their associated object along with the metric counter and mean value. The time the metric was recorded and the value are also displayed.
66
Resource Manager Administrator’s Guide
Go to Document Center
Users and processes, and metrics are shown as two different sections in the report and are described below. The report has a general information section at the top, including the date and time of report generation, the report type, and the various selections made when the report was set up. Within the report, you can click links to navigate to the different sections. The following information is shown in the report: • Users and Processes This table shows the user(s) and the processes they ran on the selected server, over the selected hour. The table shows the following information: User: The user name and domain of each user who had an active session on the server. Process: The name of each process that was run on the server. There is a separate entry for each instance of the process. Path: The location of each process instance on the server. This may help you distinguish between instances. Version: For each instance, the version number that is defined within the program file. Process versions are available for some Win32 processes, but no Win16 or DOS processes. Metrics The Metrics table gives information about every metric that was monitored on the server over the selected hour. The table shows the following information: Object: This is the category being monitored. It is a physical or logical system resource: for example, a computer's hard disk. Note The Terminal Services object is returned from Windows Terminal Server. The Inactive Sessions counter for this object includes idle sessions. Instance: An individual example of the object or a state it needs to reach in order to be counted. For example, a computer may have more than one hard disk. In this case, the instance would identify which disk is being examined. Counter: This is the specific aspect of the object being monitored. For example, disk free space. Time: The date and time at which the operating system last recorded the metric. Dates are shown in dd/mm/yyyy format; times are shown in the local time zone of the server being reported on. Value: The average value of the metric over the selected hour.
•
Go to Document Center
Chapter 5 Reporting and Analyzing Resource Manager Information
67
How Reports from Servers in Different Time Zones or Locales are Displayed
This section describes how reports are displayed if you have servers in the server farm that are located in different time zones and/or are localized to different languages.
What if a Reporting Server Uses a Different Time Zone?
Times and dates that you enter when generating reports are understood by the system to be in the local time for the server on which you are reporting. All times and dates shown in the report tables are in the local time zone for the server being reported on, with the exception of User Summary and Process Summary reports. User Summary and Process Summary reports show session times in the local time zone of the server requesting the report. Report generation times, in the report header, show the time the report was generated in the requesting server’s local time, plus any UTC (Coordinated Universal Time) offset. Server Summary reports also show UTC offset for times in the Metrics table. An example scenario is a server farm with servers in various parts of the world. This example server farm has: • • Resource Manager servers located in New York, United States (UTC - 5 hours) Resource Manager servers in Berlin, Germany (UTC + 1 hour)
The server farm administrator generates a Server Summary report for the last six hours from a server in New York at 13:00 hours local time. The report shows a generation time of “13:00-05:00” (New York time). An event that occurred two hours previously (11:00 hours in New York) in Berlin will be shown as 16:00 hours—the local time the event occurred in Berlin.
What if a Reporting Server Uses a Different Language?
Resource Manager software supports a number of languages. In a server farm with differing language versions of Resource Manager present, reports are localized to the locale where the Console requesting the report is. In the above example, the information from Berlin (German locale) would be reported in American English (United States locale).
68
Resource Manager Administrator’s Guide
Go to Document Center
Estimating the Concurrent User Capacity of a Server
You can use the summary database to estimate the concurrent user capacity of a server. The summary database stores information concerning CPU and memory usage for various processes running on MetaFrame Presentation Server. To determine user capacity 1. Either • • Add the server to the published applications in an existing farm Create a new farm and limit user access to approximately 20 users per server. —Or—
2. Using the information in this guide, and the Help, configure and enable the summary database. 3. Ask your users to launch and use the published applications running on the server you are testing. Ensure that users continue to use the server over a suitable period of time, in order to create a record of resource usage that reflects your users’ normal working practices. 4. Create a Crystal report that queries the following: • • Average CPU and memory usage for the specific processes being assessed, per user. Average CPU and memory usage for other processes associated with a user, such as explorer.exe, ctfmon.exe, osa.exe, wfshell.exe, csrss.exe, svchost.exe, and winlogon.exe. A defined threshold. For example, no more than 90% CPU usage and/or no more than 3 GB of RAM used. A calculation to extrapolate the number of users that can be divided into the threshold given the resource usage above.
• •
In general, the longer the time for which users work on the server, the more accurate the data averages that can be collected from the summary database.
Go to Document Center
CHAPTER 6
Billing Users for Resource Usage
Overview
Resource Manager enables you to produce Billing reports based on the information stored in your summary database. Billing reports use the resource usage data from the summary database and a fee profile to calculate the charges for users of the server farm. You define fee profiles to reflect different charging rates and currencies. You can organize individual users and/or user groups into billable groups known as cost centers. You can also bill individual domain users or user groups. When Resource Manager generates a Billing report, it calculates the charges by multiplying the resources used during the report period by the associated fee. All reports are displayed in a report viewer window. You can save a report in HTML or comma-separated values (CSV) format for later printing, viewing, or inclusion in documents. The detailed steps you need to carry out to complete the tasks outlined in this chapter are provided in the Help.
Creating a Fee Profile
Before you can produce Billing reports you should configure at least one fee profile. If a Billing report is generated without using a fee profile, no costs information is provided. In each fee profile you specify a currency and a list of rates to charge for resource usage. Resources you can include in a fee profile are: • • • CPU time Session time Memory usage
70
Resource Manager Administrator’s Guide
Go to Document Center
• •
Process loaded time Process active time
Organizing Users into Cost Centers for Billing
You can organize users into cost centers. For example, to charge different departments within an organization, you create a cost center for each department. Each cost center is linked to a fee profile: it is best to create at least one fee profile before you create cost centers. The “Summary database users” cost center is a predefined cost center that cannot be removed, edited, or renamed. This cost center is defined as all users who have an entry in the summary database. Important Local user groups on farm servers that are included in cost centers can be billed only if the server in question is currently running.
Producing Billing Reports
Cost Centers Report
This type of Billing report is used to charge cost centers for their use of certain resources that are being monitored in the server farm. You need to have at least one fee profile and one cost center to generate useful Cost Center Billing reports. The report tells you about the use of various chargeable resources by the cost center(s) over a selected period. If there are users who are not members of any cost center, you can generate Billing reports for them by billing against domain users. See “Domain Users Report” on page 70 for details. You can avoid billing of system processes by configuring Resource Manager to ignore those processes. See the Help for more information about ignoring processes.
Domain Users Report
This type of Billing report is used to charge individual users or user groups within Microsoft Windows domains for their use of certain resources that are being monitored in the server farm. You need to create at least one fee profile to generate useful Domain User Billing reports. The report tells you about the use of various chargeable resources by the users over a selected period.
Go to Document Center
Chapter 6 Billing Users for Resource Usage
71
How Information Is Presented in the Report
The columns shown in the report depend on the options you choose in the Report Options dialog box. The report is laid out on a per cost center or domain user basis, and is further broken down into the sessions used over the report period. Totals for each column are displayed at the end of the report.
User
The domain user(s) or cost center(s) for which the report has been generated.
Session Start
The date and time each session began during the report period. Dates are shown in dd/mm/yyyy format; times are shown in the local time zone of the Console requesting the report, along with any UTC offset.
Session Elapsed Time
The total length of all session times during the report period. Unfinished sessions are billed for the time elapsed during the report period. Report Totals for session elapsed time at the end of the report shows the combined total of all user sessions, including unfinished sessions, during the report period.
Process
The names of the processes run by the user over the course of each session.
CPU Time Used
The CPU time used for each process over the course of each session during the report period. Report Totals for CPU time used at the end of the report shows the combined total of all CPU time used during the report period.
Memory Used
The memory used for each process (in Megabyte-minutes) over the course of each session. Report Totals for memory used at the end of the report shows the combined total of all memory used during the report period.
Process Loaded Time
The length of time each process was loaded during each session. Report Totals for process loaded time at the end of the report shows the combined total of all process loaded times during the report period.
72
Resource Manager Administrator’s Guide
Go to Document Center
Process Active Time
The length of time each process was active during each session. Report Totals for process active time at the end of the report shows the combined total of all process active times during the report period.
Cost
The amount to be charged for each process during each session. Report Totals for cost at the end of the report shows the combined total of all charges during the report period.
Fee Profile
The fee schedule and currency for chargeable resources is listed at the end of the report.
Go to Document Center
CHAPTER 7
Troubleshooting
Overview
This chapter covers some common questions that you may encounter when using Resource Manager, and offers possible solutions.
Unexpected Behavior
Q: I set up a feature in Resource Manager, but it doesn't seem to be working. What could be the problem?
Ensure the server you are monitoring was upgraded to Resource Manager for Citrix MetaFrame Presentation Server 3.0, Enterprise Edition. It is possible to use Resource Manager in an environment where some servers in the server farm are upgraded and others are not. However, if you are monitoring a server that has not been upgraded, certain aspects of the user interface will not work for that server, even if the server from which you are running the Presentation Server Console was upgraded.
Q: For a published application, Resource Manager shows the application count as zero, even though some instances of the application are running. How can I see the correct application count?
Check that you specified the full path (rather than just the application executable) in the Properties dialog box for the published application.
Q: The IMA service crashed while I was adding Resource Manager metrics to a server. Why?
If the IMA service stops working while you are adding metrics for monitoring on a server, it may be because you tried to add a metric that has already been added. If there are duplicate instances of a metric, Resource Manager cannot monitor either instance of that metric.
![]() 74
Resource Manager Administrator’s Guide
Go to Document Center
Q: Some metric values reported by Resource Manager are negative. What should I do?
Some values returned by the Windows Performance API and displayed by Resource Manager are negative. You can ignore these values.
Q: I connected to a Resource Manager server in a different time zone, and saw some apparent time discrepancies. What is happening?
All dates and times displayed in the Presentation Server Console are in the context of the server's time zone and current local time. When you connect to a server using a MetaFrame Presentation Server Client, the clock shown on your remote desktop is in the client device’s time zone. If you look in the local database, note that the times stored there are in UTC (Coordinated Universal Time), and therefore cannot be directly compared with the times displayed in the Console.
Q: My Database Connection Server is not updating the summary database. Why?
Your Database Connection Server might not be able to connect to the summary database DBMS. If your Database Connection Server cannot communicate with the summary database during a database update, in the Summary Database tab, under Status, the lower indicator changes to Critical . Your summary data is not lost; it is stored locally until the problem is rectified. Note Data that is delayed in reaching the summary database due to Database Connection Server problems is subject to purge settings once in the summary database. For example, if you cannot update the summary database for a week and some of your stored data is set to be purged after five days, when the problems are rectified and the data is stored in the summary database, it is purged at the next purge time because it is already five days old. To view any problems that have occurred 1. In the left pane of the Presentation Server Console, click Resource Manager. 2. In the right pane, click the Summary Database tab. 3. Click View Server Log. The server log shows if any problems have occurred. To address communication problems between the Database Connection Server and the summary database • Ensure the user identification password for the Database Connection Server is correct. See “Setting a System Data Source Name” on page 22 for details.
Go to Document Center
Chapter 7 Troubleshooting
75
•
If the connection problem still exists, the Database Connection Server may have failed. This is also indicated by a “Server Down” status icon in the Resource Manager Watcher window. More information about the fault is available from that server’s Resource Manager server log.
Q: I generated a Resource Manager summary report and the information I expected to find was not there. Where is it?
When you create summary reports, all information for the report is derived from records stored in the summary database. If you encounter problems with your summary reports, these may be due to the following reasons: • The Resource Manager metric you want to report on is not set to be stored in the summary database. Check that the metric in question is set to be stored in the external database for the relevant server. See “Selecting Server and Application Metrics to Record in the Database” on page 46. • The report was created during the 24-hour period between database updates. If you create several reports for the same information between the 24 hourly automatic database update times, the information in the reports will not change. If you need information on a server for periods between database updates, use Resource Manager Current reports. See Chapter 4, “Reporting and Analyzing Information”. • The report was created after the information was purged from the summary database. You must create reports on information before it is purged from the summary database. Check whether your purging schedule gives you time to create appropriate reports. See “Removing Unwanted Information from the Database” on page 47 for details about database purging schedules. • The name of a contributing Resource Manager server is changed. Each server in the server farm is identified to the summary database by the server’s network identification computer name. If you change a server’s name, existing records referencing the old name remain in the database until purged while new records for the new server name are created. Note If you change a server’s name from x to y, and rename another server in the server farm from z to x, new reports for server x are collated from data from both new server x and old server x.
76
Resource Manager Administrator’s Guide
Go to Document Center
Q: I am getting the error message: “The Farm Metric Server cannot be contacted.” What can I do?
If you see this message: • • First, find out which servers are acting as the primary Farm Metric Server and the backup Farm Metric Server. (See below.) Ensure that both the primary Farm Metric Server and the backup are operational. If either server is down, restart it.
If either server is heavily loaded, the Farm Metric Server can take some time to respond, so this error message probably indicates a time-out error. If you suspect that this might be the case, wait for a few moments, or change the Farm Metric Server and the backup to servers that are more lightly loaded.
Locating Servers
Q: How do I find out which server is currently acting as the Database Connection Server?
To identify the Database Connection Server 1. On any Resource Manager server, start the Presentation Server Console. 2. In the left pane of the Console, click Resource Manager. 3. In the right pane, click the Summary Database tab. The current Database Connection Server is shown. See the Resource Manager online Help for instructions about how to change the Database Connection Server.
Q: How do I find out which server is currently acting as the Farm Metric Server?
To identify the Farm Metric Server 1. On any Resource Manager server, start the Presentation Server Console. 2. In the left pane of the Console, click Resource Manager. 3. In the right pane, click the Farm Metric Server tab. The current Farm Metric Servers are listed.
Go to Document Center
CHAPTER 8
Using the Monitoring and Reporting Extensions
In addition to the reporting and monitoring capabilities provided by Resource Manager in the Presentation Server Console, you can also create reports and analyze your farms’ performance using two extensions to the Access Suite Console: Report Center and Dashboard. For an overview of the features provided by the extensions, see “Extended Reporting Capabilities” on page 12 and “Extended RealTime Monitoring Capabilities” on page 13. Information about installation and software requirements for the Access Suite Console is in the MetaFrame Presentation Server Administrator’s Guide. User assistance for the extensions is provided on-screen in the Access Suite Console. This chapter provides extra information that you will find helpful when using the extensions. Note that the reports available in Report Center are different from those available in Resource Manager. Similarly, Dashboard allows you to display performance metrics differently from Resource Manager. So, to familiarize yourself with all the monitoring and reporting capabilities at your disposal, explore both the Presentation Server Console (containing Resource Manager) and the Access Suite Console (containing Dashboard and Report Center).
Monitoring Server Metrics
Using Dashboard, you can get an indication of the real-time performance of your server farm by: • Displaying selected performance data for a set of servers on a bar graph. This allows you to monitor, in a highly visual way, a single set of data across a range of servers. Displaying the current status of metrics for a particular server.
•
78
Resource Manager Administrator’s Guide
Go to Document Center
To display performance data for a set of servers 1. In the tree of the Access Suite Console, select the servers whose performance data you want to display. 2. Click Display performance data > Dashboard, then select a metric. 3. If prompted, select the display size of the icons to use. A qualitative indication of the metric’s value is displayed on a small bar graph above each server icon. 4. Hold the pointer over each item to display the value of the metric. To analyze the current status of metrics for a particular server 1. Select a server. 2. Click Change Display > View Server Health.
Creating Reports
Using Report Center, you can generate reports that describe the use and availability of your servers and published applications. You can also report on the sessions, client types, and policies that are in use in your farms. Note Report Center reports do not include information about Resource Manager applications. To create a report you specify the following: • The type of report. The pre-defined report types provided should cover most of your reporting needs, but you can download the other report types from the Citrix Web site. The items to include in the report, such as servers and applications. Where and in what format you want to save the report. Whether you want to email the report to other people. How often you want to run the report.
• • • •
You can monitor the status of reports that are due to run or have already run.
Go to Document Center
Chapter 8 Using the Monitoring and Reporting Extensions
79
Unexpected Behavior
Q: Although I scheduled a report to run, no report is generated and the job’s status is now “Error.” Why?
In order to run reports that require a connection to a summary database when you are not logged on to the machine running the Report Center, your credentials must be passed to the Task Scheduler. To ensure that this occurs, edit your report’s specification and ensure that Allow saving password is checked on the Connections tab of the Data Link Properties dialog box.
Q: I suspect that a report does not contain up-to-date information. How do I rectify this?
Make sure you run discovery just before generating a report. This is important if items such as servers or applications have been added to your farm. If you don’t run discovery before generating a report, data for new items is not included. If you suspect that data is missing from your report, this could be because an item in your farm has been removed since you generated the specification on which the report is based. For example, if a server has been removed from a farm, the selection in the report includes the server, but no data is displayed for the server.
Q: I cannot save a schedule with a particular name because Report Center tells me that a schedule already exists with that name, even though it doesn’t appear in my list of schedules. Why?
Report Center creates schedules using Microsoft Task Scheduler, which you may have used to create schedules for other reasons. Either rename the Report Center schedule or locate the other schedule with the same name, and, if it’s no longer needed, delete it. Then try saving the Report Center schedule again.
Go to Document Center
APPENDIX A
Default Metric Set
Overview
This appendix describes the default set of metrics that is monitored by Resource Manager for Microsoft Windows 2000 and Windows Server 2003 operating systems. The explanations of each metric are based on the default configuration. Alternative configurations may produce alerts under different circumstances from those described here.
82
Resource Manager Administrator’s Guide
Go to Document Center
Default Set of Metrics
Data Store Connection Failure
Minutes
The number of minutes since the server last successfully connected to the data store, informing you if communications between a server and the IMA data store fail. This failure could be because: • • • The IMA data store DBMS system is down. This could be due to failure, or for maintenance. The network connection to the server with the IMA data store is down. The server with the IMA data store is down.
License Server Connection Failure
Minutes
The number of minutes since the server last successfully connected to the license server, informing you if communications between a server and the license server fail. This failure could be because: • • • A hardware failure on the license server has occurred The license service on the license server is malfunctioning Network problems exist between the license server and the MetaFrame Presentation Server computer
Logical Disk
Important Resource Manager Logical Disk metrics require that Windows Logical Disk counters are enabled. You can determine whether or not they are enabled by running the “diskperf” utility at the command line. For more information on the Logical Disk performance counters, run the “diskperf /?” command.
% Disk Time
Gives an indication of how busy the disks are. The disk can become a bottleneck for a number of reasons: • The server has too little physical memory so is “thrashing.” If thrashing is occurring, the pages/sec will also be high.
Go to Document Center
Appendix A Default Metric Set
83
•
A single user is running an application or process that makes extensive and rapid use of the disk. You can investigate such a user by running Current Process and Current User reports. Many users are performing large amounts of disk activity. The speed of the disks may be the server’s bottleneck.
•
The metric % Disk Time is calculated using a number of factors, and values above 100% are possible. If you see values of 100% disk time, the disk is in constant use. Values greater than 100% may indicate that the disk is too slow for the number of requests.
% Free Space
The server is running out of disk space. Several factors can cause this: • • • A lack of remaining disk space after installing the operating system and applications A large number of users have logged on (now or in the past) and their configuration data, settings, and files are taking up too much space A rogue process or user is consuming a large amount of disk space
Memory
Available Bytes
Informs you if too much memory is being used. This could be because: • • • Too many users are logged on. The applications that users are running are too memory hungry for the amount of memory available on the server. Some user or process is using a large amount of memory. Running a Current Process report may help you track this down.
Being short on memory could result in “thrashing.” The disk usage and paging metrics may also change to a red alarm state.
Pages/sec
A large amount of paging indicates either: • The system is low on physical memory and the disk is being used extensively as virtual memory. This can be caused by too many users being logged on, too many processes running, or a rogue process “stealing” virtual memory.
—Or—
84
Resource Manager Administrator’s Guide
Go to Document Center
•
An active process or processes are making large and frequent memory accesses.
Too much paging degrades the performance of the server for all users logged on. The Available Bytes, Disk, and % Processor Time metrics may also enter warning or danger states when a large amount of paging occurs. Short bursts of heavy paging are normal, but long periods of heavy paging seriously affect server performance.
Network Interface
Bytes Total/sec
Gives a good indication of how much network activity this server is generating or receiving. If this metric changes to yellow or red, the server is experiencing unusually high network activity and may cause a network saturation. If too many users are remotely logged on for the network card to support, this metric may change to a warning or danger state. In this situation, the bottleneck could be the network or server’s network card, which may decrease performance of users’ sessions.
Paging File
% Usage
A high page file usage usually indicates that the server’s page file size should be extended. If the Memory: Pages/sec metric is also high, it is a good idea to add more physical memory.
Processor
% Interrupt Time
The processor is spending a large amount of time responding to input and output rather than user processing. A large value for interrupt time usually indicates a hardware problem or a very busy server.
% Processor Time
A high processor time for a long period of time indicates that the processor is the bottleneck of the server, too many users are logged on, or there is a rogue user or process (use the Current Process report to investigate).
Go to Document Center
Appendix A Default Metric Set
85
System
Context Switches/sec
A large number of threads and/or processes are competing for processor time.
Terminal Services
Active Sessions
A large number of users are logged on and running applications. The server may begin running out of memory or processor time and performance for users may deteriorate. Note that current Presentation Server Console sessions are listed as “active.”
Inactive Sessions
There is a large number of disconnected sessions, which are taking up virtual memory. Remove some disconnected sessions or reduce the length of time for which disconnected sessions can persist until they are automatically removed.
Go to Document Center
APPENDIX B
Summary Database Schema
Overview
This appendix describes the layout and organization of the Resource Manager summary database schema. The summary database is a data warehouse made up of historical data imported from each Resource Manager server in the server farm. The database schema of the local database is de-normalized, whereas the data stored in the summary database is extensively normalized to save storage space. It includes: • • A diagram of database entity relationships Descriptions of each database entity table
Database Entity Relationship Diagram
The diagram on the following page shows all the tables in the schema and the links required to retrieve data. The main tables are named SDB_xxx and have a white background to their title bar. Supporting (or look-up) tables have a gray background to their title bar and are (mostly) named LU_xxx. Some supporting tables are used more than once. This reduces the space required to store the data.
![]() 88
Resource Manager Administrator’s Guide
Go to Document Center
A Diagram Showing Database Entity Relationships
Go to Document Center
Appendix B Summary Database Schema
89
Database Entity Table Descriptions
The following data types are all described using Microsoft SQL Server data types. For type mappings to other SQL databases, see the type mapping section at the end of this Appendix.
Application History
SDB_APPHISTORY
This table stores a history of published applications supported on each server.
SDB_HISTORY PK_SDB_APPHISTID FK_APPNAMEID FK_SERVERID STARTTIME ENDTIME SERVERUTCBIAS int int int date time date time int NOT NULL NOT NULL NOT NULL NOT NULL NULL NOT NULL Unique identifier for referential integrity (Primary Key) Pointer to LU_APPNAME, application name Pointer to LU_SERVER, server name Time application was supported by this server Time application no longer supported by server Bias in minutes to be subtracted from EVENTTIME to find the event time in the server's local time zone
Foreign Key(s) • • • • • • • FK_APPNAMEID FK_ SERVERID FK_ SERVERID FK_ APPNAMEID STARTTIME LU_APPNAME LU_SERVER
Unique
Support Tables Referenced
90
Resource Manager Administrator’s Guide
Go to Document Center
Application Metrics
SDB_APPMETRICS
This table stores a summary of all application metrics in a server farm.
SDB_APPMETRICS FK_APPNAMEID FK_FARMNAMEID FK_OBJECTID APPMETRICUPDATETIME APPMETRICSAMPLEPERIOD APPMETRICDATACOUNT MINAPPMETRICVALUE MAXAPPMETRICVALUE MEANAPPMETRICVALUE STDDEVAPPMETRICVALUE int int int date time int int float float float float NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL Pointer to LU_APPNAME, application name and type Pointer to LU_FARMNAME, farm name Pointer to LU_OBJECT, object name Timestamp of last application metric data point in dataset (stored in UTC) Sample period of summary record in seconds Number of data points used to summarize this row Minimum application metric value Maximum application metric value Mean application metric value Standard deviation of application metric values
Foreign Key(s) • • • FK_APPNAMEID FK_FARMNAMEID FK_OBJECTID
Go to Document Center
Appendix B Summary Database Schema
91
Unique • • • • • • • • FK_APPNAMEID FK_FARMNAMEID FK_OBJECTID APPMETRICUPDATETIME APPMETRICUPDATETIME LU_APPNAME LU_FARMNAME LU_OBJECT
Additional Indexed Columns Support Tables Referenced
Client History
SDB_CLIENTHISTORY
SDB_CLIENTHISTORY FK_SDB_SESSIONID FK_CLIENT_ID FK_CLIENTPROPERTIESID STARTTIME ENDTIME SERVERUTCBIAS int int int date time date time int NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL Pointer into SDB_SESSION for sessionID Pointer into LU_CLIENT for client name and address Pointer into LU_CLIENT_PROPERTIES for build number, version, client type Time client connected to session Time client disconnected (or time IMA service was stopped) Bias in minutes to be subtracted from time fields to adjust the time to the server's local time zone Boolean indicating whether Secure Gateway was used Pointer into LU_LAUNCHER table
USINGSG FK_LAUNCHERID
INT INT
92
Resource Manager Administrator’s Guide
Go to Document Center
Foreign Key(s) • • • • • • • • FK_SDB_SESSIONID FK_CLIENTID FK_CLIENTPROPERTIESID FK_LAUNCHERID SDB_SESSION LU_CLIENT LU_CLIENTPROPERTIES LU_LAUNCHER
Support Tables Referenced
Connection History
SDB_CONNECTIONHISTORY
This table stores connected and disconnected session counts at the time of any session connect or disconnect event for each server in the farm.
SDB_CONNECTIONHISTORY FK_SERVERID CONNECTED DISCONNECTED TIMESTAMP int int int date time NOT NULL NOT NULL NOT NULL NOT NULL Pointer to LU_SERVER, RM server name Count of connected sessions Count of disconnected sessions Bias in minutes to be subtracted from TIMESTAMP to adjust the time to the server's local time zone
Foreign Key(s) • • • FK_ SERVERID FK_ SERVERID TIMESTAMP Unique
Go to Document Center
Appendix B Summary Database Schema
93
Support Tables Referenced • LU_ SERVER
Event Log
SDB_EVENTLOG
This table stores generic IMA service up and IMA service down events that occur on a server farm.
SDB_EVENTLOG EVENTCODE FK_SERVERID EVENTTIME SERVERUTCBIAS int int date time int NOT NULL NOT NULL NOT NULL NOT NULL Generic event ID. SERVER_DOWN = 0, SERVER_UP = 1 Pointer to LU_SERVER, Resource Manager server name Timestamp of event occurrence (Date and Time) Bias in minutes to be subtracted from EVENTTIME to find the event time in the server’s local time zone
Foreign Key(s) • • • • • • FK_SERVERID EVENTTIME LU_SERVER EVENTCODE FK_SERVERID EVENTTIME Additional Indexed Columns Support Tables Referenced Unique
94
Resource Manager Administrator’s Guide
Go to Document Center
Administrator Configurable Server Metrics
SDB_METRICS
This table stores all metrics imported from each Resource Manager server in the server farm. The metric values are summarized to reduce data storage requirements.
SDB_METRICS FK_SERVERID FK_METRICID METRICUPDATETIME SERVERUTCBIAS int int date time int NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NULL Pointer to LU_SERVER, Resource Manager server name Pointer to LU_METRIC, metric description Timestamp of last metric data point in dataset (stored in UTC) Bias in minutes to be subtracted from EVENTTIME to find the event time in the server's local time zone Sample period of summary record in seconds Number of data points used to summarize this row Minimum metric value Maximum metric value Mean metric value Standard deviation of metric values Reference to folders and zone information
METRICSAMPLEPERIOD METRICDATACOUNT MINMETRICVALUE MAXMETRICVALUE MEANMETRICVALUE STDDEVMETRICVALUE FK_SERVERINFID
int int float float float float Int
Primary Key (Unique) • • • FK_SERVERID, FK_METRICID, METRICUPDATETIME FK_METRICID FK_SERVERID Foreign Key(s)
Go to Document Center
Appendix B Summary Database Schema
95
Additional Indexed Columns • • • • METRICUPDATETIME LU_METRIC LU_SERVER LU_SERVERINF Support Tables Referenced
Processes
SDB_PROCESS
This table stores process data per user.
SDB_PROCESS PK_SDB_PROCESSID FK_SERVERID FK_PROCESSID FK_USERID FK_CLIENTID FK_APPNAMEID int int int int int int NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Pointer to LU_SERVER, Resource Manager server name Pointer to LU_PROCESS Pointer to LU_USER, user name Pointer to LU_CLIENT, client name Pointer to LU_APPNAME, application name and type of the application with which the process is most closely associated. This is determined by examining each process in the process parenting hierarchy (starting with the process itself and working upwards) and comparing the process executable path with that of all published applications. If a match is found, FK_APPNAMEID reflects this; if no match is found, FK_APPNAMEID is set to reflect a blank application name. Pointer to SDB_SESSION, session data
FK_SDB_SESSIONID
int
NOT NULL
96
Resource Manager Administrator’s Guide
Go to Document Center
SDB_PROCESS PID EXITCODE int int NOT NULL NOT NULL NOT NULL NOT NULL NULL Process identifier (from operating system) The exit code returned by the executable when it completes. 259 means “Still executing” A mask indicating which processor(s) the process can use to execute itself Time the process started executing Time the process completed execution - or the time the process statistics were last updated when EXITCODE = 259 End time - Start time (in milliseconds) A summation of all monitored periods of a process where the CPU time was greater than 1% The percentage of kernel CPU time the process has used during its lifetime The percentage of user CPU time the process has used during its lifetime The percentage of user CPU that was being used during the ACTIVETIME The percentage of kernel CPU that was being used during the ACTIVETIME Sum of the average number of megabytes per minute used by the process during its lifetime The number of megabytes per minute used by the process during the ACTIVETIME The peak recorded working set of the processes at any point during its lifetime The peak page file allocated to process in bytes at any point in its lifetime The number of page faults that occurred The peak paged pool usage in bytes at any point in its lifetime
AFFINITY STARTTIME ENDTIME
int date time date time float float
TOTALTIME ACTIVETIME
NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL
KERNELUSE USERUSE USERACTIVE KERNELACTIVE MEMORY
float float float float float
MEMORYACTIVE WORKSET PAGEFILE PAGEFAULTS PAGEDPOOL
float int int int int
Go to Document Center
Appendix B Summary Database Schema
97
SDB_PROCESS NONPAGEDPOOL SESSID SERVERUTCBIAS int int int NOT NULL NOT NULL NOT NULL The peak non-paged pool usage in bytes at any point in its lifetime Matches the SESSIONID value in the SESSION table Bias, in minutes, to be subtracted from EVENTTIME to find the event time in the server's local time zone
Primary Key (Unique) • • • • • • • • • • • • • • • PK_SDB_PROCESSID FK_APPNAMEID FK_CLIENTID FK_PROCESSID FK_SERVERID FK_USERID FK_SDB_SESSIONID STARTTIME, ENDTIME FK_SERVERID, STARTTIME, PID LU_APPNAME LU_CLIENT LU_PROCESS LU_SERVER LU_USER SDB_SESSION Foreign Key(s)
Additional Indexed Columns Unique Support Tables Referenced
98
Resource Manager Administrator’s Guide
Go to Document Center
User Information
SDB_SESSION
This table stores session data per user.
SDB_SESSION PK_SDB_SESSIONID FK_USERID FK_SERVERID FK_CLIENTID FK_APPNAMEID int int int int int NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NULL NULL NOT NULL NULL NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL Start time of the first process run under the user's session (stored in UTC) End time of the final process to end as part of the session (stored in UTC) End time - Start time in milliseconds Bias in minutes to be subtracted from EVENTTIME to find the event time in the server's local time zone Bias in minutes to be subtracted from SESSIONSTART in which to find the session start-time A Session ID (generated by operating system) Sum of the total time for all completed processes Sum of the active time for all completed processes Unique identifier for referential integrity (Primary Key) Pointer to LU_USER, user name Pointer to LU_SERVER, Resource Manager server name Pointer to LU_CLIENT, client name Pointer to LU_APPNAME, application name and type for the published application with which the session was launched Pointer to LU_WINSTATION, name of the WinStation the session is connected through
FK_WINSTATIONID PROTOCOL SESSIONSTART SESSIONEND DURATION SERVERUTCBIAS
int int date time date time float int
SESSIONUTCBIAS
int
SESSIONID TOTALTIMESUM ACTIVETIMESUM
int float float
Go to Document Center
Appendix B Summary Database Schema
99
SDB_SESSION CPUTIMESUM MEMORYSUM BILLSTATUS float float int NOT NULL NOT NULL NOT NULL Sum of the CPU time for all completed processes Sum of the memory usage for all completed processes 0 = This session is not billed
Primary Key (Unique) • • • • • • • • • • • • • PK_SDB_SESSIONID FK_APPNAMEID FK_CLIENTID FK_SERVERID FK_USERID FK_WINSTATIONID STARTTIME, ENDTIME FK_SERVERID, SESSIONSTART, SESSIONID LU_APPNAME LU_CLIENT LU_SERVER LU_USER LU_WINSTATION Foreign Key(s)
Additional Indexed Columns Unique Support Tables Referenced
100
Resource Manager Administrator’s Guide
Go to Document Center
Version Control
SCHEMAVERSION
This table stores the version of the summary database schema. The version number is queried on connection by the Database Connection Server to determine if it and the summary database schema are compatible.
SCHEMAVERSION VERSION int NOT NULL Version number of database schema
Support and Look-Up Tables
LU_APPNAME
Look-up table of published application names.
LU_APPNAME PK_APPNAMEID APPNAME APPTYPE int nvarchar (256) int NOT NULL NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Application name Application type: 0 = MetaFrame Presentation Server published application, 1 = Resource Manager application
Primary Key (Unique) • • PK_APPNAMEID APPNAME, APPTYPE Unique
Go to Document Center
Appendix B Summary Database Schema
101
LU_CLIENT
Look-up table of client names.
LU_CLIENT PK_CLIENTID CLIENTNAME CLIENTADDRESSFAMILY CLIENTADDRESS int nvarchar(32) int nvarchar(20) NOT NULL NOT NULL NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Client name Client address family: 2 = AF_INET, 6 = AF_IPX Client address
Primary Key (Unique) • • PK_CLIENTID CLIENTNAME, CLIENTADDRESSFAMILY, CLIENTADDRESS Unique
LU_CLIENTPROPERTIES
Look-up table of client build, version (if available), and a reference into the LU_CLIENTTYPEMAPPINGS table.
LU_CLIENTPROPERTIES PK_CLIENTPROPERTIES FK_CLIENTTYPEID BUILD VERSION int int int nvarchar(64) NOT NULL NOT NULL NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Reference into LU_CLIENTTYPEMAPPINGS Client build number Client version, or “ “ if not available
Primary Key (Unique) • PK_CLIENTPROPERTIESID
102
Resource Manager Administrator’s Guide
Go to Document Center
Unique • • • FK_CLIENTTYPEID BUILD VERSION
LU_CLIENTTYPEMAPPINGS
Maps protocol type and client product identifier onto a client type name. Known mappings are populated when the schema is created. New ones will be added as ICA n or RDP n where n is the client product identifier.
LU_CLIENTTYPEMAPPINGS PK_CLIENTTYPEID PROTOCOL_TYPE CLIENT_TYPE int int int NOT NULL NOT NULL NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Protocol type. 0 = Console, 1 = ICA, 2 =RDP Client product identifier
CLIENTTYPENAME
nvarchar(32)
Semi-useful string (for example, ICA Win32)
Primary Key (Unique) • • • PK_CLIENTTYPEID CLIENT_TYPE PROTOCOL_TYPE Unique
LU_FARMNAME
Look-up table for server farm names. It is a support table for LU_SERVER.
LU_FARMNAME PK_FARMNAMEID FARMNAME int nvarchar (255) NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Server farm name
Go to Document Center
Appendix B Summary Database Schema
103
Primary Key (Unique) • • PK_FARMNAMEID FARMNAME Unique
LU_INSTANCE
Look-up table of instances; for example, “C:\.”
LU_INSTANCE PK_INSTANCEID INSTANCE int nvarchar (128) NOT NULL NULL Unique identifier for referential integrity (Primary Key) Instance name
Primary Key (Unique) • • PK_INSTANCEID INSTANCE Unique
LU_LAUNCHER
Look-up table of launchers, for example, Program Neighborhood Agent.
LU_LAUNCHER PK_LAUNCHERID LAUNCHER int nvarchar(128) NOT NULL NULL Unique identifier for referential integrity (Primary Key) Launcher name
Primary Key (Unique) • • PK_LAUNCHERID LAUNCHER Unique
104
Resource Manager Administrator’s Guide
Go to Document Center
LU_METRIC
Look-up table of metric definitions. This table stores look-up keys for objects, metric counters, and instances.
LU_METRIC PK_METRICID FK_OBJECTID FK_METRICCOUNTERID FK_INSTANCEID int int int int NOT NULL NOT NULL NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Pointer to LU_OBJECT, object name Pointer to LU_METRICCOUNTER, metric counter name Pointer to LU_INSTANCE, instance name
Primary Key (Unique) • • • • • • • • PK_METRICID FK_INSTANCEID FK_METRICCOUNTERID FK_OBJECTID PK_OBJECTID, FK_METRICCOUNTERID, FK_INSTANCEID LU_INSTANCE LU_METRICCOUNTER LU_OBJECT Foreign Key(s)
Unique Support Table Referenced
Go to Document Center
Appendix B Summary Database Schema
105
LU_METRICCOUNTER
Look-up table of metric counters; for example, “% Disk time.”
LU_METRICCOUNTER PK_METRICCOUNTERID METRICCOUNTER int nvarchar (128) NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Metric counter name
Primary Key (Unique) • • PK_METRICCOUNTERID METRICCOUNTER Unique
LU_NETDOMAIN
Look-up table for Network Domain names. It is a support table for LU_SERVER and LU_USER.
LU_NETDOMAIN PK_NETDOMAINID NETDOMAIN int nvar char (32) NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Network domain name
Primary Key (Unique) • • PK_NETDOMAINID NETDOMAIN Unique
106
Resource Manager Administrator’s Guide
Go to Document Center
LU_OBJECT
Look-up table of objects; for example, “Logical Disk.”
LU_OBJECT PK_OBJECTID OBJECT int nvar char (128) NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Object name
Primary Key (Unique) • • PK_OBJECTID OBJECT Unique
LU_PATH
Look-up table of application paths. This is a support table for LU_PROCESS.
LU_PATH PK_PATHID PATH int nvar char (260) NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Path
Primary Key (Unique) • • PK_PATHID PATH Unique
Go to Document Center
Appendix B Summary Database Schema
107
LU_PROCESS
Look-up table of process details.
LU_PROCESS PK_PROCESSID FK_PATHID FK_PROCESSNAMEID TYPE int int int int NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Pointer to LU_PATH, path name Pointer to LU_PROCESSNAME, executable name of the file associated with the process Type of the executable. Win32, POSIX etc. -1 indexed, where -1 = system process, 0 means Win32 application Hexadecimal version number of executable
VERSION
nvar char (24) date time
PRODUCTDATE
Timestamp of executable (by originators)
Primary Key (Unique) • • • • • • PK_PROCESSID FK_PATHID FK_PROCESSNAMEID FK_PATHID, FK_PROCESSNAMEID, TYPE, VERSION, PRODUCTDATE LU_PATH LU_PROCESSNAME Foreign Key(s)
Unique Support Tables Referenced
108
Resource Manager Administrator’s Guide
Go to Document Center
LU_PROCESSNAME
Look-up table of process names. This is a support table for LU_PROCESS.
LU_PROCESSNAME PK_PROCESSNAMEID PROCESSNAME int nvar char (255) NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Executable name of the file associated with the process
Primary Key (Unique) • • PK_PROCESSNAMEID PROCESSNAME Unique
LU_SERVER
Look-up table for Resource Manager server instances.
LU_SERVER PK_SERVERID FK_SERVERNAMEID FK_NETDOMAINID FK_FARMNAMEID UPDATETIME int int int int date time NOT NULL NOT NULL NOT NULL NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Pointer to LU_SERVERNAME, Resource Manager server name Pointer to LU_NETDOMAIN, network domain name Pointer to LU_FARMNAME, server farm name The last time summary data was written to the summary database for this particular server
Primary Key (Unique) • PK_SERVERID
Go to Document Center
Appendix B Summary Database Schema
109
Foreign Key(s) • • • • • • • FK_FARMNAMEID FK_NETDOMAINID FK_SERVERNAMEID FK_SERVERNAMEID, FK_NETDOMAINID, FK_FARMNAMEID LU_FARMNAME LU_NETDOMAIN LU_SERVERNAME
Unique Support Tables Referenced
LU_SERVERNAME
Look-up table for Resource Manager server names. It is a support table for LU_SERVER.
LU_SERVERNAME PK_SERVERNAMEID SERVERNAME int nvar char (32) NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Server name
Primary Key (Unique) • • PK_SERVERNAMEID SERVERNAME Unique
110
Resource Manager Administrator’s Guide
Go to Document Center
LU_SERVERINF
Look-up table for Resource Manager server folder and zone information.
LU_SERVERNAME PK_SERVERINFID FOLDER int nvar char (255) nvar char (128) NOT NULL NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Folder name
ZONE
Zone name
Primary Key (Unique) • • PK_SERVERINFID FK_SERVERINFID, FOLDER, ZONE Unique
LU_USER
Look-up table of user instances.
LU_USER PK_USERID FK_NETDOMAINID USERNAME int int nvar char (32) NOT NULL NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Pointer to LU_NETDOMAIN, network domain name User name
Primary Key (Unique) • • PK_USERID FK_NETDOMAINID Foreign Key(s)
Go to Document Center
Appendix B Summary Database Schema
111
Unique • • FK_NETDOMAINID, USERNAME LU_NETDOMAIN Support Tables Referenced
LU_WINSTATION
Look-up table for WinStation names.
LU_WINSTATION PK_WINSTATIONID WINSTATION int nvar char (32) NOT NULL NOT NULL Unique identifier for referential integrity (Primary Key) Name of the WinStation through which the session is connected
Primary Key (Unique) • • PK_WINSTATIONID WINSTATION Unique
SDB_SCRATCH
A cross-reference table used to identify session records contained in Billing reports so that the sessions can be updated to show they were billed.
SDB_SCRATCH CMC_ID REPORT_ID USER_ID SCRATCH_DATE FK_SDB_SESSIONID SESSION_START SESSION_DURATION date time int date time float int int NULL NULL NULL NOT NULL NOT NULL NULL NULL Identifies the Presentation Server Console that generated the report Identifies the report in question Identifies a user within the report Time that this table row was created SDB_SESSION.PK_SDB_SESSIONID session table primary key value SDB_SESSION.SESSIONSTART value SDB_SESSION.DURATION value
112
Resource Manager Administrator’s Guide
Go to Document Center
SDB_SCRATCH PROCESS_TOTAL_TIME SERVER_UTC_BIAS SERVER_NAME float int nvar char (32) NULL NULL NULL SDB_SESSION.TOTALTIMESUM value SDB_SESSION.SERVERUTCBIAS value The server that the session ran on
Primary Key (Unique) • • • • • • None None None SESSION_START FK_SDB_SESSIONID CMC_ID, REPORT_ID, USER_ID, FK_SDB_SESSIONID Foreign Key(s) Unique Additional Indexed Columns
SDB_HEURISTICS
This table holds miscellaneous data used by Resource Manager.
SDB_HEURISTICS PK_HEURISTIC nvar char (64) float NOT NULL NOT NULL Name of heuristic
HEURVALUE
Value of heuristic
Go to Document Center
Appendix B Summary Database Schema
113
String Length Table
This table lists the maximum string lengths available.
String LU_SERVERNAME\SERVERNAME LU_NETDOMAIN\NET\NETDOMAIN LU_FARMNAME\FARMNAME LU_OBJECT\OBJECT LU_METRICCOUNTER\METRICCOUNTER LU_INSTANCE\INSTANCE LU_APPNAME\APPNAME Length 32 32 256 128 128 128 39 Justification/Reference CNLEN=15 in LMCONS.H (NT Header), padded to RM1 size DNLEN=15 in LMCONS.H (NT Header), padded to RM1 size MAXLEN_FARMNAME=255 in Cconfig.H (MF header) Taken from Monitor Subsystem Taken from Monitor Subsystem Taken from Monitor Subsystem PNDATA_BROWSERNAME_LEN GTH in imacommonapplicationdefs.h (MF) UNLEN=15 in LMCONS.H (NT Header), padded to RM1 size CNLEN=15 in LMCONS.H (NT Header), padded to RM1 size ADDRESS size = 20 in Wtsapi32.h (NT Header) WINSTATIONNAME_LENGTH=32 in CTXDEF.H (MF header) MAX_PATH in WINDEF.H (NT header) MAX_PATH in WINDEF.H (NT header) imacore.h Imacommonapplicationdefs.h. Dropped one character due to technical issue with DBMS Client VERSION Client type name, for example ICA Win32, ICA Mac, RDP Win32 Client launcher, for example Program Neighborhood Agent
LU_USER\USERNAME LU_CLIENT\CLIENTNAME LU_CLIENT\CLIENTADDRESS LU_WINSTATION\WINSTATION LU_PATH\PATH LU_PROCESSNAME\PROCESSNAME LU_SERVERINF\ZONE LU_SERVERINF\FOLDERS
32 32 20 32 260 255 128 255
LU_CLIENTPROPERTIESVERSION LU_CLIENTTYPEMAPPINGS\CLIENTTYPE NAME LU_LAUNCHER
64 32 256
114
Resource Manager Administrator’s Guide
Go to Document Center
SQL Data Type Mapping
Column data types vary between different SQL databases. A list of all data types used in the summary database for all supported SQL databases follows:
Description Integer numbers Small integer numbers Floating point numbers Date and time National variable length strings (Unicode) Microsoft SQL Server int tinyint float datetime nvarchar Oracle int smallint float date varchar2
Go to Document Center
APPENDIX C
Glossary
CSV
Comma-separated values. A file format used as a portable representation of a database. Each line is one entry or record and the data fields in a record are separated by commas. Commas can be followed by spaces and/or tab characters that are ignored. If a field includes a comma, the whole field must be surrounded with double quotes. An administrator who is subordinate to a full administrator. Custom administrators cannot set up other administrator accounts and have only a subset of the permissions that a full administrator has. A Resource Manager server that writes data to, and reads data from, a summary database. A data store that centralizes configuration information about published applications, users, printers, and servers. Each server farm has a single data store. The system data source name stores information about how a client can connect to a database. It is used by a client to access a DBMS. In the case of Resource Manager, the client is the Database Connection Server. Database management system. A software interface between the database and the user. A DBMS handles user requests for database actions with provision for data security and integrity requirements.
custom administrator
Database Connection Server data store
data source name
DBMS
116
Resource Manager Administrator’s Guide
Go to Document Center
Farm Metric Server
This server interprets farm-wide metrics and then processes them as part of its summary data. Application count is an example of a farmwide metric. You can also have a backup Farm Metric Server. A set of rates to be charged for using different types of resources. Fee profiles are used for billing. An administrator who has full access to all the administrative functions and features of the server farm. Full administrators are the only administrators who are allowed to create or modify other administrator accounts. Citrix’s server-to-server infrastructure that provides robust, secure, and scalable tools for managing a server farm of any size. Among other features, IMA enables centralized platform-independent management, an ODBC-compliant data store, and a suite of management products that plug in to the Presentation Server Console. One of a series of measurable items for a server or application. You can select which metrics you want to monitor for a particular server. This occurs when Resource Manager is actively looking at the data on servers. A server or published application. An instance of a program that is being executed. An application that is not a MetaFrame Presentation Server published application but is still recognized by the Resource Manager system. A description of a database to a DBMS in the language provided by the DBMS. A schema defines aspects of the database, such as attributes (fields) and attribute parameters. To systematically eliminate old or unneeded information.
fee profile
full administrator
Independent Management Architecture (IMA)
metric
monitoring
object process Resource Manager application schema
purge
Go to Document Center
Appendix C Glossary
117
server farm
A group of servers that are managed as a single unit, and that share some form of physical connection and a single data store. A colored signal in the status display that shows the status of each metric. When a status icon in the display changes, an alarm condition occurs. An averaged calculation of metrics information recorded on a Resource Manager server once each hour. Summarized data is stored by a DBMS for reporting purposes. Coordinated Universal Time. UTC is the same time as Greenwich Mean Time (GMT), and is the reference time zone used for calculating world time zones.
status icon
summary data
UTC
Go to Document Center
119
Index
A
Acrobat Reader, requirements 10 alarms automatic alerts for 36 counter 31 instance 31 object 31 pausing notification of 36 watching for 35 alerts 36 configuring automatic alerts 36 email 37 MAPI email configuration 37 pausing 36 SMS (cell/mobile phone) 40 SMTP email 37 SNMP 40 when a server stops operating 36 applications reporting on use 57 selecting Count metric for 46 automatic alerts 36
D
data source name 21 definition 115 setting 22 setting for Microsoft SQL Server 22 setting for Oracle 23 data store definition 115 Database Connection Server configuring 24 definition 115 DBMS definition 115 installing onto a server 18, 21 requirements 18, 21 transaction log 52 default metrics 81 data store connection failure 82 logical disk 82 memory 83 network interface 84 paging file 84 processor 84 system 85 terminal services 85 default metrics set 82 dialog box Data Sources (ODBC) 22–23, 38 Microsoft SQL Server DSN Configuration 22 Oracle ODBC Driver Configuration 24 Summary Database Configuration 24
B
billing cost centers 70 domain users 70
C
cell/mobile phone (SMS) alerts 40 cost centers 70 counter, for metrics 31 CSV files definition 115 saving to 56, 69 custom administrator definition 115
E
email alerts 37 configuring in Resource Manager 39 creating a mail profile 38 MAPI Connection Server 38 Resource Manager Mail Service 38 errors fixing user identification/password conflict 74 missing summary report information 75 viewing Resource Manager server log files 74
120
Resource Manager Administrator’s Guide
Go to Document Center
metrics displaying using the Dashboard 77 Microsoft SQL Server DSN Configuration dialog box 22 mixed environments 73 modems (for SMS alerts) 40 monitoring definition 116 entire server farm 29 real-time 31 single server 29, 36
F
Farm Metric Server configuration 49 contacting 76 definition 116 fee profile 69 full administrator definition 116
G
gateways for SMS alerts 40
N
negative metric values 74 network interface metrics 84 Bytes Total/sec 84 new features 12
I
icons, meaning of 33 Independent Management Architecture, definition 116 installation changing location after installation 30 software requirements 17 instance, for metrics 31
O
object definition 116 object, for metrics 31 Oracle ODBC Driver Configuration dialog box 24
L
licensing 18 log file, server 33 logical disk metrics 82 % Disk Time 82 % Free Space 83
P
paging file metrics 84 % Usage 84 pausing alarms and alerts 36 performance monitor 41 process definition 116 Process Summary report 61 processes reporting on current 57 reporting on history of 61 processor metrics 84 % Interrupt Time 84 % Processor Time 84 profile for email alerts 38 purge definition 116 purging, summary database overview 47
M
MAPI email to configure 38 memory metrics 83 Available Bytes 83 Pages/sec 83 metric checking status of 35 configuring automatic alerts for 36 counter 31 Count, selecting applications for 46 default set of 81 definition 116 instance 31 issues to consider when selecting for summary database 45 negative values 74 object 31 selecting for summary database 46
R
real-time monitoring 31 Report Center 78 unexpected behavior 79
Go to Document Center
reports about current activity 56 about past activity 61 Billing, overview 69 creating using Report Center 78 Current Process 57 Current User 59 overview 56 Process Summary 61 saving 67 saving Billing 72 Server Snapshot 60 Server Summary 65 templates 56 User Summary 63 viewing saved Billing 72 viewing saved current and summary 67 Resource Manager application definition 116 Resource Manager Mail Service 38 Resource Manager server log, viewing 74 Resource Manager tab 28
Index
summary database DBMS supported 18 estimating size 54 growth management 54 ignoring specific times during the day 47 issues to consider when selecting metrics for 45 purging 47 size considerations 52 transaction log 52 turning off 26 turning on 25 Summary Database Configuration dialog box 24 summary database schema administrator configurable server metrics 94 application metrics 89 LU_APPNAME 100 LU_CLIENT 101 LU_FARMNAME 101 LU_INSTANCE 103 LU_LAUNCHER 103 LU_METRICCOUNTER 105 LU_NETDOMAIN 105 LU_OBJECT 106 LU_PATH 106 LU_PROCESS 107 LU_PROCESSNAME 108 LU_SERVER 108 LU_SERVERINF 110 LU_SERVERNAME 109 LU_USER 110 LU_WINSTATION 111 processes 95 SCHEMAVERSION 100 SDB_APPMETRICS 90 SDB_CLIENTHISTORY 91 SDB_EVENTLOG 93 SDB_METRICS 94 SDB_PROCESS 95 SDB_SCRATCH 111 SDB_SESSION 98 SQL data type mapping 114 string length table 113 support and look-up tables 100 user information 98 version control 100 Summary Database tab 24 suspending alarms and alerts 36 system metrics 85 Context Switches/sec 85
121
S
schema definition 116 server determining user capacity 68 monitoring single 36 reporting on recent activity of 60 server farm definition 116 server log 33 Server Summary report 65 SMS alerts 40 configuring 40 modems 40 SMTP email alerts configuring 36 SNMP alerts 40 to configure 41 status icon definition 117 status icons meaning of 33 summary data definition 117 ignoring specific times during the day 47
122
Resource Manager Administrator’s Guide
Go to Document Center
system requirements DBMS 18, 21 for email alerts 37 for SMS alerts 40 for SNMP alerts 40
T
tab Resource Manager 28 Summary Database 24 TAPI Servers 40 terminal services metrics 85 Active Sessions 85 Inactive Sessions 85 traps List of SNMP 40 troubleshooting 73
U
unexpected behavior 73 Report Center 79 uninstalling Resource Manager 20 User Summary report 63 users reporting on current 59 reporting on history of 63 UTC definition 117
