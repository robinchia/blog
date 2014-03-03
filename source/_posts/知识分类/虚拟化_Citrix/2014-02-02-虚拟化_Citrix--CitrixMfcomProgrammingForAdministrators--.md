---
layout: post
title: "Citrix Mfcom Programming For Administrators--"
categories: 虚拟化_Citrix
tags: 
 - 虚拟化_Citrix
--- 

# Citrix Mfcom Programming For Administrators--

[SlideShare]( "SlideShare")

* [Home]( "SlideShare")
* [Browse]( "Browse Presentations")
* [My Slidespace]( "Your SlideShare Profile")
* **[Upload]( "Upload PowerPoint, OpenOffice, Keynote or PDF files")**
* [Community]( "Groups, Events, Contests, Karaoke")
* [Widgets]( "Widgets for blogs, websites, MySpace, Facebook, and other social media")

[Search]( "Search for, view and download presentations")

# Citrix Mfcom Programming For Administrators

*
* [Share]( "Email this presentation to your contacts")
* [Favorite]( "Save to favorites")
* [Favorited]( "Saved to your favorites") [X]( "Remove from favorites")
* [Get File]( "Download this presentation")
* [More...]( "Add this presentation to a Group or Event")
*  
Loading...

Flash Player 9 (or above) is needed to view presentations.
We have detected that you do not have it on your computer. To install it, go [here](http://www.adobe.com/shockwave/download/download.cgi?P1_Prod_Version=ShockwaveFlash "Install Adobe Flash Player").

* Post to
* [Blogger](http://www.slideshare.net/share/blogspot/595751 "Post to Blogger")
* [WordPress]( "Post to WordPress")
* [Twitter](http://www.slideshare.net/share/tweet/595751/Check%20out%20this%20SlideShare%20Presentation%20:%20Citrix%20Mfcom%20Programming%20For%20Administrators "Post to Twitter")
* [Facebook](http://www.slideshare.net/share/facebook/595751/Citrix%20Mfcom%20Programming%20For%20Administrators "Post to Facebook")
* [Delicious](http://delicious.com/save?url=http%3A%2F%2Fwww.slideshare.net%2Fvishalganeriwala%2Fcitrix-mfcom-programming-for-administrators-presentation&title=Citrix+Mfcom+Programming+For+Administrators&jump=yes&noui&share=yes&partner=slideshare "Bookmark on Delicious")
*

* [more share options]( "Post to More Networks")
* Embed    For WordPress.com
Without related presentations
## [0 comments]()

[Post a comment]()

Post a comment

[![]()]()

[![]()]()
[Embed Video]( "Just include the embed code in the comment. We support embeds from Youtube, Google Videos, BlipTV, Metacafe, Viddler, Yahoo Videos and several other approved sites only.")  Email:
Subscribe to follow-up comments  [Unsubscribe from followup comments]()       Edit your comment   [Cancel]()

## [Notes on slide 1]()

## 2 Favorites

* [ ]( "Add as my contact") [![fpschultze](http://public.slidesharecdn.com/images/userImage_SMALL.gif) Frank-Peter Schultze]( "fpschultze"), Consultant  favorited this 3 months ago

**Tags** **[citrix]()** **[mfcom]()**
* [ ]( "Add as my contact") [![AmitRanjan](http://cdn.slidesharecdn.com/profile-photo-AmitRanjan?1224137016) Amit Ranjan]( "AmitRanjan | I head SlideShare's New Delhi office.  


"), CoFounder & COO  at SlideShare,  favorited this 11 months ago
[more]()    []()

## Citrix Mfcom Programming For Administrators - Presentation Transcript

1. Citrix MFCOM Programming for Administrators October 2007
1. 100 Applications 1000 Users 100,000 Clicks
1. Introductions

* Instructors

* Nitin Desai

* Vishal Ganeriwala

* Facilitators

* Fred Liu

* Tom Kludy

* Jeff Reed
* Agenda MFCOM Basics Start Using MFCOM Labs In Action Future And Takeaways
* Agenda Agenda item number 1 Start Using MFCOM Labs In Action Future And Takeaways MFCOM Basics
* What is MFCOM?

* COM-based management interface

* Runs on every Citrix Presentation Server™ box

* Scriptable and COM programming language support

* Remote execution
* Work Smarter and Faster Using MFCOM Integration with third-party software applications 3 Information reporting 2 Automate administration tasks 1
* How Does MFCOM Work ? MFCOM IMA LHC DCOM RPC SAL SAL SAL Data Store Data Collector Citrix Presentation Server Windows Machine SAL : Subsystem Access Layer
* Common MFCOM Objects Zones Farm Apps Servers Sessions Policies Servers Users Sessions Apps Processes VCs
* Agenda Agenda item number 1 MFCOM Basics Labs In Action Future and Takeaways Start Using MFCOM
* Today’s Setup For Class Demos DCOM Port 2512 Client Machine MPS SDK Lab1CPS CPS 4.5 VM Lab2CPS CPS 4.5 VM DCOM IMA Farm Name: SDKDemo Data Store: Lab1CPS Zone Data Collector: Lab1CPS Servers : Lab1CPS & Lab2CPS
* How Can I Get Started?

* Download SDK from: http://support.citrix.com/page.jspa?pageID=devCenter

* Browse Script Repository http:// support.citrix.com/kb/category.jspa?categoryID =645&subCategoryID=645
* Setting Up Client and Server Machines

* Register the remote Citrix Presentation Server using: mfreg <servername>

* Use dcomcnfg to change the Impersonation level from identity to impersonate on Windows XP

* W2K3+SP1 CPS Server - Add remote MFCOM user to the DCOM Users Group

* Make sure you have administrative privileges for the Citrix Farm
* Sample Examples
* MPSSDK Documentation

* MPSSDK Help System

* MFCOM browser

* http:// www.jasonconger.com/ShowPost.aspx?strID =9686d808-19c0-4bad-a577-02d85f597a8d
* Displaying Farm Name

* Use of VBScriptTemplate.wsf to add code

* Use of MetaFrameFarm Object and its methods
* Navigating MFCOM …

* New version of interfaces with different releases

* MetaFrameFarm object : IMetaFrameFarm6 (CPS4.0) inherits from IMetaFrameFarm5 (CPS3.0)

* Scripts connect to the latest interfaces

* C++ - Use QueryInterface

* Drill down to appropriate level of object.

* MetaFrameServer Object:

* IMetaFrameServer

* IMetaFrameWinServer
Finding the relevant interface
* Agenda Agenda item number 1 MFCOM Basics Start Using MFCOM Future And Takeaways Labs In Action
* What Will You Learn?

* When and how to use MFCOM

* Navigating the documentation

* Using major MFCOM objects

* Auditing the farm

* Automating Citrix Presentation Server™ management tasks
* Labs: Commonly Done Presentation Server Administration Tasks

* Lab 1: Servers and Applications

* Lab 2: Create a Load Evaluator

* Lab 3: Take the Server Offline

* Extra Credit:

* Printing
* Auditing Using Enumerations

* Object associations as a powerful tool for auditing
Zones Farm Apps Servers Sessions Sessions
* Lab 1 : Servers and Applications

* List servers and applications in the Farm.

* List Applications on each server.

* List Servers for each application

* Use of MetaFrameServer and MetaFrameApplication methods/properties.
* Simple Administration Tasks

* Creating new entities in the farm

* Create an Object in MFCOM

* Initialize the necessary data

* SaveData – Saving data to IMA Datastore

* Changing the configuration settings for the existing entities

* Create an Object (CreateObject method)

* Initialization – Initialize or set methods

* LoadData – Loading data from IMA Datastore

* Change the settings

* SaveData – Save data back to IMA Datastore
* Lab2 : Create a Load Evaluator

* Create a Load Evaluator

* Set the name and description

* Create a LMRule for CPU utilization

* Create a LMRules collection and add a rule

* Set Load Evaluator Rules and save the data

* Use of MetaFrameLoadEvaluator, MetaFrameLMRule and MetaFrameLMRules methods/properties.
* Inside MFCOM

* COM Free threading model

* Error reporting

* Limited error codes as HRESULT

* Exceptions in. NET

* MFCOM user

* Current user vs. other user (runas)

* MFCOM impersonates before calling IMA
* Complex Management Tasks

* Connecting more than one entity in the farm

* Series of tasks with proper ordering

* Repetitive pattern

* Examples

* Take the server offline

* Migrate applications from one farm to another

* Remove the server from N applications
* Lab3 : Taking The Server Offline

* How to take the server offline for maintenance purpose gracefully?

* Steps -

* Prevent the new connections

* Communicate to the users

* Let the connections drain

* Logoff the sessions after timeout period

* Use of MetaFrameLoadEvaluator, MetaFrameSession and MetaFrameServer methods/properties.
* Advanced MFCOM Usage

* Multi farm management

* Create objects on the multiple remote servers

* C# - Activator method

* VBScript - CreateObject((&quot;MetaFrameCOM.MetaFrameFarm&quot;, ServerName1)

* Event handling

* Server, Application and Folder Events

* Create, update (rename), move and delete

* MetaFrameFarmEvent enum, IMetaFrameEventQueue , CreateEventQueue2 method in MetaFrameFarm
* Extra Credit : Printing

* List printer drivers on every server

* Use of MetaFrameServer and MetaFramePrinterDriver Methods
* Agenda Agenda item number 1 MFCOM Basics Start Using MFCOM Labs In Action Future and Takeaways
* CPSSDK

* Scalability

* Highly scalable in large farm environment

* Usability

* Consolidation of interfaces and methods

* Enhanced consistency in object behavior

* Easy multi-farm management

* Better .Net Support

* .NET assembly

* .NET versioning and generics
Next-generation Presentation Server Management SDK
* How Does CPSSDK Work ? IMACOM IMA LHC DCOM RPC SAL SAL SAL Data Store Data Collector Citrix Presentation Server Windows Machine CPSSDK Client Cached Data Chunky Calls SAL : Subsystem Access Layer
* Work Smarter and Faster Start Using MFCOM Today !
* Before you leave…

* Overall conference survey is available online at www.citrixiforum.com starting Wednesday, October 24 (please provide feedback)

* Download workshop materials from www.citrixiforum.com starting Monday, October 29

* Leftover workshops handouts can be found at the HOT Assistance Center Desk
*  
* Data Type: LMRuleSchedule Start time End time Bit 15-0 Bit 31-16 Half hour Bit 4 Not used Bit 7-5 Day of week Hour (in military time format) Bit 3-0 Bit 15-8

[+]( "Add as my contact") [![vishalganeriwala](http://public.slidesharecdn.com/images/userImage_SMALL.gif)vishalganeriwala]( "vishalganeriwala"), 12 months ago

Embed  [custom]()   Without related presentations
For WordPress.com

1343 views, 2 favs, 2 embeds [more stats]()
This is a basic presentation done at Citrix IForum [more]()

This is a basic presentation done at Citrix IForum 2007 event. It is a good overview for anyone getting started with Citrix MFCOM [less]()

## [Related Presentations]()

* [![Cloud Computing And Citrix C3]()  Cloud Computing And Citrix C3]( "Cloud Computing And Citrix C3")
* [![Drupal CDN integration: easier, more flexible and faster!]()  Drupal CDN integration: easier, mo…]( "Drupal CDN integration: easier, more flexible and faster!")
* [![]()  ![Top 10 Items Found By Citrix Consulting On Assessments]()  Top 10 Items Found By Citrix Consu…]( "Top 10 Items Found By Citrix Consulting On Assessments")
* [![eG Citrix Monitor]()  eG Citrix Monitor]( "eG Citrix Monitor")
* [![How to Get to Second Base with Your CDN]()  How to Get to Second Base with You…]( "How to Get to Second Base with Your CDN")
* [![Faster & more flexible CDN integration]()  Faster & more flexible CDN integra…]( "Faster & more flexible CDN integration")
* [![Let's make your CDN with RUBY]()  Let's make your CDN with RUBY]( "Let's make your CDN with RUBY")
* [![XS Japan 2008 Citrix Japanese]()  XS Japan 2008 Citrix Japanese]( "XS Japan 2008 Citrix Japanese")
* [![Бесплатная виртуализация Citrix XenServer для компаний]()  Бесплатная виртуа�…]( "Бесплатная виртуализация Citrix XenServer для компаний")

## [More by user]()

[View all presentations from this user]( "View all presentations from this user")
## More Info

© All Rights Reserved
Go to [text version]()

* **Total Views** 1343

* 1328 on SlideShare
* 15 from embeds
* **Comments** 0
* **Favorites** 2
* **Downloads** 34
**Most viewed embeds**

* 9 views on http://localhost.citrix.com
* 6 views on http://ftlwvishalg02:8080

[more]()

**All embeds**

* 9 views on http://localhost.citrix.com
* 6 views on http://ftlwvishalg02:8080

[less]()

* Also on [LinkedIn](http://www.linkedin.com/opensocialInstallation/preview?_ch_panel_id=1&_applicationId=1200&trk=p_slideshare)
* Uploaded via [SlideShare]()
* Uploaded as [Microsoft PowerPoint]()

[Flagged as inappropriate Flag as inappropriate]()   Flag as innappropriate

Select your reason for flagging this presentation as inappropriate. If needed, use the [feedback]( "FeedBack") form to let us know more details.

None Pornographic Copyright Violation Defamatory Illegal/Unlawful Other Terms Of Service Violation   [Cancel]()
## Categories

* **[]()**
* **[Technology]()**
Automotive Books Business & Mgmt Career Design Education Entertainment Fashion & Beauty Finance Gadgets & Reviews Health & Medicine How-to & DIY Humor Investor Relations News & Politics Pets Photos Real Estate Spiritual Sports Technology Templates & Forms Travel   [Cancel]()

## Tags

* **[cdn]()**
* **[mfcom]()**
* **[citrix]()**
* **[xenapp]()**
-->

[Search]( "Search for, view and download presentations")

[RSS Feed]()

**What's new?**

[]()[Participate in the SlideShare World's Best Presentation Contest 2009...](http://blog.slideshare.net/2009/08/03/worlds-best-presentation-contest-is-back-and-more-social-than-ever/)

* **Info**
* [About]()
* [Blog](http://blog.slideshare.net)
* [Press]()
* [Advertise]()
* [Jobs]()

* **Using SlideShare**
* [Quick Tour]()
* [Terms of Use]()
* [Privacy Policy]() & [DMCA]()
* [Community Guidelines]()

* **Help**
* [Feedback / Contact]()
* [FAQ]()
* [Forum](http://getsatisfaction.com/slideshare)

* **Explore**
* [Find your Friends]()
* [Events]()
* [Developers & API]()
* [Karaoke](http://www.slidesharetoys.com/)
* [Make a Webinar]()
* [SlideShare+YouTube]( "Add YouTube videos to your slides")

* **Slideshare Outside**
* [Linkedin App](http://www.linkedin.com/opensocialInstallation/preview?_ch_panel_id=1&_applicationId=1200 "SlideShare for LinkedIn")
* [Facebook App](http://apps.facebook.com/slideshare "SlideShare for Facebook")
* [SlideShare in PowerPoint](http://www.slideshare.net/developers/apps/pptribbon "SlideShare Plugin for PowerPoint 2007")
* [SlideShare Mobile]( "SlideShare Mobile")

© 2009 SlideShare Inc. All Rights Reserved

* Favorited! Add tags?   [Cancel]()
* Edit your favorites   [Cancel]()
* Send to your Group / Event  Select Group / Event
Add your message  [Cancel]()
