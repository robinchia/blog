---
layout: post
title: "SmartAuditor"
categories: 虚拟化_Citrix
tags: 
 - 虚拟化_Citrix
--- 

# SmartAuditor

![]() ![]() Citrix Presentation Server SmartAuditor
Joël Stöcker Senior Systems Engineer EMEA, AVG
1 | © 2007 Citrix Systems, Inc.—All rights reserved.
Session Agenda
• What is SmartAuditor? • Technical Overview • Demo
2 | © 2007 Citrix Systems, Inc.—All rights reserved.
What is SmartAuditor?
An exclusive feature of Platinum Edition that uses flexible policies to trigger recordings of Presentation Server sessions automatically for later playback
• SmartAuditor leverages Presentation Server to provide:
• • • •
Policy-based recording of an ICA session & associated session information Managed recording & logging of ICA sessions to persistent storage Ability to search a catalog of recorded ICA sessions Playback of recorded ICA sessions for detailed analysis
• Analogy: SmartAuditor is like a Digital Video Recorder
3 | © 2007 Citrix Systems, Inc.—All rights reserved.
SmartAuditor
For improved compliance and risk management
• Enhanced auditing for regulatory compliance
• Monitor activity involving sensitive data • Record administrators screen to a video log for change management of critical systems
• Powerful application session recording for risk mitigation and eDiscovery support
• Capture screen updates, including mouse-clicks and keystrokes, to a video file • Configure monitoring of a specific user, application or server • Trusted digitally signed recording
• Accelerated problem resolution
• Replay actual screen activity at exact moment of application failure for fast troubleshooting
Advanced
Enterprise
Platinum
4 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() ![]() ![]() SmartAuditor – How It Works
Configure Capture Audit
Configure
Administrator selects which users, applications and servers to monitor Efficiently stores on-screen activity to video file in a central location and digitally signs each recording Administrative, helpdesk, and auditing teams can search the archive and review captured activity using an easy-to-use player
Capture
Audit
Select triggers for automatic capture
5 | © 2007 Citrix Systems, Inc.—All rights reserved.
System auto-captures and archives activity
Review captured activity
![]() ![]() ![]() Advantages of SmartAuditor technology
Capture screen updates to video Quickly prepare video evidence for litigation Digitally signed account of application usage
File
Record application failure for faster analysis
Monitor suspicious activity
6 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() SmartAuditor Architectural Overview
7 | © 2007 Citrix Systems, Inc.—All rights reserved.
Architecture Overview
Unsecure Network
SmartAuditor Agent
Secure Datacenter
SmartAuditor Server SmartAuditor Player
0 Configure policies 1 Connection 2 Policy 2 Data
Establish ICA
1
2
2
PS Farm
Verify recording
DB
4
Send Session
0
3
5
Log Session Data 3 and Write to Storage
3rd Party Archive Solution
Storage SmartAuditor Policy Console
4 Data
Retrieve Session
Presentation Server Clients
5 Archive files
8 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() SmartAuditor recording and logging
1. User accesses server-side virtualized
application running on Presentation Server
2 3
SmartAuditor Server
2. SmartAuditor agent begins recording
the session while it checks with SmartAuditor server whether session should be recorded
3. Agent continues recording session 4. SmartAuditor server stores metadata
and session recording
1
Presentation Server Client Presentation Server with SmartAuditor Agent Metadata and recording stream
4
9 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() Playback
1. 2. 3. 4. 5. 6. Authorized personnel accesses the player Initiate a search of the metadata records Search results are returned Selects recording of interest Retrieves log file from persistent storage Plays log file within playback window
2 3 5 6 4 1
SmartAuditor Server
10 | © 2007 Citrix Systems, Inc.—All rights reserved.
SmartAuditor Player
Security Auditor
![]() ![]() SmartAuditor Technical Overview
11 | © 2007 Citrix Systems, Inc.—All rights reserved.
Content
• Components in detail • Design Considerations and Scalability • Security • POC Best Practices • Installation • Configuration • Let's take a look…
12 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() Components in Detail
13 | © 2007 Citrix Systems, Inc.—All rights reserved.
Architecture Overview
Unsecure Network
SmartAuditor Agent
Secure Datacenter
SmartAuditor Server SmartAuditor Player
1 Connection 2 Policy
Establish ICA
Verify recording
1
2
2
Presentation Server Farm
DB
4
2 Data
Send Session
3
5
Log Session Data 3 and Write to Storage
3rd Party Archive Solution
Storage SmartAuditor Policy Console
4 Data
Retrieve Session
Presentation Server Clients
5 Archive files
14 | © 2007 Citrix Systems, Inc.—All rights reserved.
Administration - SmartAuditor Policy Console
• The SmartAuditor Policy Console provides the ability to manage policies related to recording of ICA sessions • The SmartAuditor Policy Console is implemented as an MMC Console Snap-in • Installation pre-requisites (verified before installation):
• Windows XP, Vista or Windows Server 2003 (this can be a Presentation Server) • Microsoft .NET Framework 2.0
15 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() SmartAuditor Agent
• The SmartAuditor Agent is the component which will be installed on the Presentation Server(s) • Responsible for recording session data • The SmartAuditor Driver is installed as part of the agent and is responsible for gathering session recording data
• This component will only be exposed in the Admin guide and other troubleshooting docs
• Presentation Server Platinum License is required • Installation pre-requisites
• • • • Presentation Server 4.5 with Hotfix Rollup Pack 1 Windows Server 2003 Microsoft .NET Framework 2.0 Microsoft Message Queuing (MSMQ)
Capture
16 | © 2007 Citrix Systems, Inc.—All rights reserved.
SmartAuditor Agent - Overview
3
Get session metadata from MFCOM and various Win32 functions To SmartAuditor Broker
4
MFCOM Win32 API VDs VDs VDs WDICA WDICA WDICA PD TD PD /TD PD / /TD SA Driver SA Agent
To SmartAuditor Storage Manager (MSMQ)
5
6
IOCTLs to WDICA
Recording data and session information
2
To client
Recording data and TS/CPS session ID
1
17 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() ![]() Administration - SmartAuditor Server
• SmartAuditor Broker
• Installed as an IIS 6.0 (or later)/ASP.NET 2.0 hosted web application • Responsible for communicating with the SmartAuditor Database to enforce policy query decisions and communicating with the SmartAuditor Player to manage access to session recordings
• SmartAuditor Storage Manager
• Installed as a Windows service • Tasks include:
• • • • Writing Session Recording data to disk Writes metadata to database Generates digital signatures Records performance data (use perfmon)
Configure
• Installation pre-requisites (verified before installation):
• Windows Server 2003 • Microsoft .NET Framework 2.0 • Microsoft Message Queuing (MSMQ)
18 | © 2007 Citrix Systems, Inc.—All rights reserved.
Administration - SmartAuditor Server
Temporary Restore Directory
To SmartAuditor Agent
IIS 6.0/ASP.NET 2.0 SmartAuditor Broker
4
1 3
Master File Master File Master Storage File Storage Storage SmartAuditor Database
SmartAuditor Storage Manager
2
From SmartAuditor Agent (MSMQ)
19 | © 2007 Citrix Systems, Inc.—All rights reserved.
Administration - SmartAuditor Database
• The Storage Manager writes session recording file metadata and policies in the SmartAuditor Database
• Can co-exist on SQL Server with other databases • Database schema installer will create appropriate login and user security settings • SQL Server can be clustered • Availability features such as replication and mirroring are supported
• Installation pre-requisites for Database:
• Windows 2000 or 2003 Server • Runs on any edition of SQL Server 2005, including SQL Server 2005 Express Edition SP1 • SP2 has not been tested, so is not supported at this time • Microsoft .NET Framework 2.0
20 | © 2007 Citrix Systems, Inc.—All rights reserved.
Administration - SmartAuditor Database
• Holds session metadata, policies and custom event metadata • Session metadata is approximately 1KB per recording • Custom event metadata is also approximately 1KB per recording • All string data stored as UTF-16 • Indexes optimized for player searches
SmartAuditor Database SmartAuditor Storage Manager
21 | © 2007 Citrix Systems, Inc.—All rights reserved.
SmartAuditor Data storage
• Session recording data is stored on a central file store in flat files
• Multiple directories can be defined • Storage Manager will distribute files over directories
• Directories are created by year, month and day
• E.g. E:\Recordings\2007\10\30
• SmartAuditor leverages Citrix ICA protocol to enable session recordings file sizes to be much smaller than other video protocols • Files sizes are dependent upon the graphical content of the application
22 | © 2007 Citrix Systems, Inc.—All rights reserved.
SmartAuditor Player
• The application used to replay Presentation Server session recordings captured by the SmartAuditor software • Only interacts with SmartAuditor Broker component
• Annotation and Bookmarks can be set and are stored locally on the machine
• Search option for metadata
• E.g. date/time, user, application, server, etc
Audit
• Installation pre-requisites
• Windows XP, Vista or Windows Server 2003 • Microsoft .NET Framework 2.0
23 | © 2007 Citrix Systems, Inc.—All rights reserved.
Other Components and Utilities
• SmartAuditor Authorization Console
• A utility that enables SmartAuditor Server administrators to add users to pre-determined user roles
• SmartAuditor Custom Event API
• API for the SmartAuditor software which enables ISVs to inject custom data through a thirdparty application into a session recording
• SmartAuditor Player SDK
• An SDK for use by ISVs to write third-party SmartAuditor Player extensions which can display custom event data injected into the recorded session using the SmartAuditor Custom Event API
• icldb
• A command line utility that enables you to run queries and perform maintenance of the SmartAuditor Database
• iclstat
• A command line utility for the SmartAuditor Server that enables you to view metadata information about a session recording file
24 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() Design Considerations and Scalability
25 | © 2007 Citrix Systems, Inc.—All rights reserved.
Single-Server Scenario
Presentation Server 4.5 Platinum Edition Security Auditor Workstation
SmartAuditor Agent SmartAuditor Server SmartAuditor Database SmartAuditor Policy Console
SmartAuditor Player
26 | © 2007 Citrix Systems, Inc.—All rights reserved.
Two-Server Scenario
Presentation Server 4.5 Platinum Edition SmartAuditor Server Security Auditor Workstation
SmartAuditor Agent
SmartAuditor Server SmartAuditor Database SmartAuditor Policy Console
SmartAuditor Player
27 | © 2007 Citrix Systems, Inc.—All rights reserved.
Multiple-Server Scenario
Presentation Server 4.5 Platinum Edition SmartAuditor Server Database Server
SmartAuditor Agent Management Workstation
SmartAuditor Server Security Auditor Workstation
SmartAuditor Database
Storage (e.g. SAN, NAS)
SmartAuditor Policy Console
28 | © 2007 Citrix Systems, Inc.—All rights reserved.
SmartAuditor Player
Single Server Scalabilty
• Current SSS testing show about a 1-5% overhead for SmartAuditor
PS 4.5 FP 1 SmartAuditor
29 | © 2007 Citrix Systems, Inc.—All rights reserved.
SmartAuditor Storage
Estimated Recording Size Storage Needs
< 200GB for 10,000 users
* Estimated file size for 8 hour recording. Typical interaction = e.g. Microsoft Outlook Non-Stop interaction = e.g. Microsoft Excel
* Assumes 50% typical and 50% non-stop interaction sessions with a duration of 8 hours. All calculations use estimated recording sizes.
30 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() Security
31 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() SmartAuditor Security
• SmartAuditor is designed to have the most secure configuration out-of-the-box
32 | © 2007 Citrix Systems, Inc.—All rights reserved.
Security
• SmartAuditor Agent
• Non-administrator users have no means of disabling recording or accessing recorded data • All Presentation Server configuration settings for SmartAuditor are in the registry with strong ACL security; nonadministrators cannot even view settings
• SmartAuditor Server
• Recordings can be digitally signed with a certificate • The SmartAuditor Storage Manager service is required to run as “Local System” but has all unused privileges removed • The SmartAuditor Broker runs as “Network Service” (i.e., less privileged than “Local System”) and has all unused privileges removed
33 | © 2007 Citrix Systems, Inc.—All rights reserved.
Security (cont’d)
• Communications
• SSL communication option is recommended for both the control channel (i.e., to the SmartAuditor Broker) and data channel (i.e., MSMQ) • Client authentication to server is Windows Authentication only
• SmartAuditor Player
• Only authorized users are able to search or download recorded session files • Playback Protection: encrypts recorded session files before downloading to the player • Download of files using the “Live Playback” feature is less secure than viewing a completed recording as the live downloaded data is not signed nor encrypted using playback protection
34 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() SmartAuditor POC Best Practices
35 | © 2007 Citrix Systems, Inc.—All rights reserved.
Things to know for Proof Of Concept setup
• If no certificate used, disable HTTPS in multiple places (see Admin Guide Page 68) • If database instance not designated correctly during installation, must uninstall and reinstall • Cannot use autorun to install from a network share • SQL Server Express 2005 works fine for database • Does not integrate with Desktop Server
36 | © 2007 Citrix Systems, Inc.—All rights reserved.
Things to know for POC setup (cont’d)
• Hotfix PSE450R01W2K3011 required • Default policy is do not record • Workgroup works for single server; not supported for multiple servers • When auditing based on application, only first application is used for decision
• May need to silo app or disable session sharing
• A 10.15 ICA recording cannot be played with the Player built on 10.1 client
37 | © 2007 Citrix Systems, Inc.—All rights reserved.
Keep in Mind . . .
• Disable SmartAuditor on servers where recording is not required
• Initially starts to record until policy dictates otherwise
• Initial popup can be customized, including language • Can configure multiple farms to use a single SmartAuditor server; SmartAuditor Agent can only point to one server at a time • Large files are rolled over based on configured file size or duration (whichever occurs first) • Not recommended to publish the Player as a CPS application because of high memory requirements
38 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() Installation
39 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() Installation
40 | © 2007 Citrix Systems, Inc.—All rights reserved.
Installation Order
1. CPS • HRP1, Platinum license, and set to Platinum edition • Hotfix PSE450R01W2K3011 2. SmartAuditor Server • Install SQL Server 2005 (full or Express) • Install and configure SmartAuditor Administration
• Database • Server • Policy Console
3. SmartAuditor Agent • Install and configure SmartAuditor Agent 4. Workstation • Install and configure SmartAuditor Player
41 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() Configuration
42 | © 2007 Citrix Systems, Inc.—All rights reserved.
Basic Configuration SmartAuditor Server
• SmartAuditor Server Properties:
• Set file storage location (if different from default) • Optional configuration:
• • • • Set signing certificate Modify roll-over thresholds Modify playback settings Modify notification settings
• SmartAuditor Authorization Console:
• Assign correct users to the Player, PolicyQuery and Policy Administrator roles
• SmartAuditor Policy Console:
• Create recording policy (if required) • Set active recording policy
43 | © 2007 Citrix Systems, Inc.—All rights reserved.
Basic Configuration SmartAuditor Agent
• SmartAuditor Agent Properties: • Enable session recording (default) • Optional configuration:
• Custom event recording • Connection settings: • SmartAuditor Server • MSMQ Protocol/port • MSMQ message life • SmartAuditor Broker Protocol/port
44 | © 2007 Citrix Systems, Inc.—All rights reserved.
Basic Configuration SmartAuditor Player
• SmartAuditor Player options: • Add Server • Optional configuration:
• Seek response time • Player search settings • Player Cache settings
45 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() Let’s take a look...
46 | © 2007 Citrix Systems, Inc.—All rights reserved.
![]() ![]() Reference Documentation
• This slide deck • SmartAuditor Administrator’s Guide
• CTX113599
http://support.citrix.com/article/CTX113599
• Built-in SmartAuditor player help • Knowledge Base articles
• Building a Highly Scalable SmartAuditor Server [CTX114799] • Citrix SmartAuditor Database Maintenance How to Archive and Restore Session Recordings [CTX114818] • Troubleshooting Recording Issues in Citrix SmartAuditor [CTX114819]
47 | © 2007 Citrix Systems, Inc.—All rights reserved.
48 | © 2007 Citrix Systems, Inc.—All rights reserved.
49 | © 2007 Citrix Systems, Inc.—All rights reserved.
