---
layout: post
title: "Using CAS without the CAS login screen - Central A"
categories: CAS
tags: 
 - CAS
 - without+cas+login+screen
--- 

# Using CAS without the CAS login screen - Central Authentication Service - Jasig Wiki

* [Skip to content](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen#title-heading)
* [Skip to breadcrumbs](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen#breadcrumbs)
* [Skip to header menu](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen#header-menu-bar)
* [Skip to action menu](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen#navigation)
* [Skip to quick search](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen#quick-search-query)
Quick Search

* [Browse](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen#)

* [Pages](https://wiki.jasig.org/pages/listpages.action?key=CAS "Browse pages in the Central Authentication Service space ( Type 'g' then 's' )")
* [Blog](https://wiki.jasig.org/pages/viewrecentblogposts.action?key=CAS "Browse blogs in the Central Authentication Service space")
* [Labels](https://wiki.jasig.org/labels/listlabels-heatmap.action?key=CAS "Browse labels in the Central Authentication Service space")
* [Attachments](https://wiki.jasig.org/spaces/listattachmentsforspace.action?key=CAS "Browse attachments in the Central Authentication Service space")
* [Mail](https://wiki.jasig.org/mail/archive/viewmailarchive.action?key=CAS "Browse mail in the Central Authentication Service space")
* [Advanced](https://wiki.jasig.org/spaces/viewspacesummary.action?key=CAS "Browse additional space functions in the Central Authentication Service space")
* [Activity](https://wiki.jasig.org/spaces/usage/report.action?key=CAS)

* [What’s New](http://docs.atlassian.com/confluence/docs-41/whatsnew/iframe)
* [Space Directory](https://wiki.jasig.org/spacedirectory/view.action "Browse the Confluence space directory")
* [Feed Builder](https://wiki.jasig.org/dashboard/configurerssfeed.action "Create your custom RSS feed.")
* [Keyboard Shortcuts](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen# "View available keyboard shortcuts ( Type '?' )")
* [Confluence Gadgets](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen# "Browse gadgets provided by Confluence")
* [Log In](https://wiki.jasig.org/login.action?os_destination=%2Fdisplay%2FCAS%2FUsing%2BCAS%2Bwithout%2Bthe%2BCAS%2Blogin%2Bscreen)
* [Sign Up](https://wiki.jasig.org/signup.action)

1. [Dashboard](https://wiki.jasig.org/dashboard.action "Go to Dashboard ( Type 'g' then 'd' )")
1. [Central Authentication Service](https://wiki.jasig.org/display/CAS)
1. **…**
1. [CASifying Applications](https://wiki.jasig.org/display/CAS/CASifying+Applications)
1. [Using CAS without the CAS login screen]()

* [Tools](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen#)

* [Attachments (0)](https://wiki.jasig.org/pages/viewpageattachments.action?pageId=5225 "View Attachments ( Type 't' )")
* [Page History](https://wiki.jasig.org/pages/viewpreviousversions.action?pageId=5225)
* [Restrictions](https://wiki.jasig.org/pages/viewinfo.action?pageId=5225 "Edit restrictions")

* [Info](https://wiki.jasig.org/pages/viewinfo.action?pageId=5225)
* [Link to this Page…](https://wiki.jasig.org/pages/viewinfo.action?pageId=5225 "Link to this Page ( Type 'k' )")
* [View in Hierarchy](https://wiki.jasig.org/pages/listpages-dirview.action?key=CAS&openId=5225#selectedPageInHierarchy)
* [View Source](https://wiki.jasig.org/plugins/viewsource/viewpagesrc.action?pageId=5225)

# [![]()](https://wiki.jasig.org/display/CAS)  [Using CAS without the CAS login screen]()

[Skip to end of metadata](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen#page-metadata-end)

* [Page restrictions apply](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen# "Page restrictions apply. Click the lock icon to view or edit the restriction.")
* Added by [Andrew Petro](https://wiki.jasig.org/display/~awp9), last edited by [Hendrik Brummermann](https://wiki.jasig.org/display/~brummermann) on Jul 26, 2011  ([view change](https://wiki.jasig.org/pages/diffpages.action?pageId=5225&originalId=52495191))
* [show comment](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen#) [hide comment](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen#)
**Comment:** Corrected links that should have been relative instead of absolute.
[Go to start of metadata](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen#page-metadata-start)

This page is for documenting how to use CAS without using the CAS login screen.

## Why not to do this

One of the advantages CAS brings to the table is a single authentication user experience across all applications using CAS. The user doesn't have to wonder whether he should be giving his password to any particular application, because no applications other than CAS receive the password.

Implementing this hack results in applications other than CAS having access to actual usernames and passwords. This is a bad thing. It reduces the security value CAS is trying to give you.

## Existing features

The CAS gateway feature allows your application to attempt to use CAS single sign on to authenticate the user without interrupting the user experience to display a CAS login screen. If no SSO session already exists, CAS simply redirects back to your service without displaying the login screen.

## Alternative approaches

An alternative to your application rendering its own login screen is to get CAS Server to render the login screen you would have wanted to render. [WIND](https://wiki.jasig.org/display/CAS/Columbia) goes this route. This is on the feature list for CAS 3.

A relatively easy option (for portals, at least) is to proxy the HTML of the CAS login page and display it inline in the portal page.  The proxied form should still submit to CAS itself.  You can tweak the CAS jsps to use different CSS styles depending on the User Agent or Remote Host, to provide a modified (e.g. smaller) login screen.

## Using CAS without the login screen

Suppose you really do want your service to be handling the user's username and password.

First, produce your custom form. When your custom form submits, the recipient of the submit needs to do the following:

### Validate the username and password

Somehow, you need to figure out whether this username and password are correct.

One thing to do would be to generate an HTTP request for CAS login, parse out a valid login ticket, then issue a request for CAS login posting the login ticket, username, and password. Parse the response. If it indicates a success, the username and password are valid. Otherwise not.

Another thing to do would be to go directly against whatever it is that CAS is using to validate usernames and passwords.

If the username and password didn't match, redisplay your form. If they did match, then either 1) you're done because all you wanted to do was authenticate the user, or 2) now you need to establish the CAS SSO session in the web browser and obtain a Service ticket.

### Establish the SSO session and possibly get a Proxy Ticket

Again, get a login ticket and the jsessionid. Then, generate a redirect to the browser placing the sesisonid, login ticket, the username, and the password on the CAS login URL to which you're redirecting:
[?](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen#)https:

//cas.example.com/cas/login;jsessionid=XXXXXXXX?username=XXXXXXX&password=XXXXXXX&lt=XXXXXX

Specify the service to be your application where you want to receive the service ticket.

CAS will set the Single Sign On cookie so the user will have a CAS SSO session established.

Since the username, password, and login ticket are good, CAS will redirect back to your service with the service ticket. Grab the service ticket and validate it in the normal way (presumably using a CAS client). This is your opportunity to obtain a Proxy Ticket.

Note: This only works as long as the user has not visited the CAS webpage before. Otherwise he or she got a different sessionid cookie which has priority over the sessionid in the url.

## Experience Reports 

[Here is an experience report](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen) on how an UI-less mode for CAS has been achieved using javascript only on the client side. Passwords are only handed over to the SSL secured CAS service, not to a third party.
Labels:

None

[Edit Labels](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+CAS+login+screen# "Edit Labels ( Type 'l' )")
Powered by a free **Atlassian Confluence Open Source Project License** granted to Java Architectures Special Interest Group. [Evaluate Confluence today](http://www.atlassian.com/c/conf/11461).
This Confluence installation runs a Free Gliffy License - [Evaluate Gliffy](http://www.gliffy.com/products/confluence-plugin/) for your Wiki!

* Powered by [Atlassian Confluence](http://www.atlassian.com/software/confluence) 4.1.5, the [Enterprise Wiki](http://www.atlassian.com/software/confluence/tour/enterprise-wiki.jsp)
* Printed by Atlassian Confluence 4.1.5, the Enterprise Wiki.
*  ·  [Report a bug](http://jira.atlassian.com/secure/BrowseProject.jspa?id=10470)
*  ·  [Atlassian News](http://www.atlassian.com/about/connected.jsp?s_kwcid=Confluence-stayintouch)

[]()[]()[]()
