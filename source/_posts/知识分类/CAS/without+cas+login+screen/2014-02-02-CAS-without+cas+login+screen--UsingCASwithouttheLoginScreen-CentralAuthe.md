---
layout: post
title: "Using CAS without the Login Screen - Central Authe"
categories: CAS
tags: 
 - CAS
 - without+cas+login+screen
--- 

# Using CAS without the Login Screen - Central Authentication Service - Jasig Wiki

* [Skip to content](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#title-heading)
* [Skip to breadcrumbs](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#breadcrumbs)
* [Skip to header menu](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#header-menu-bar)
* [Skip to action menu](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#navigation)
* [Skip to quick search](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#quick-search-query)
Quick Search

* [Browse](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#)

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
* [Keyboard Shortcuts](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen# "View available keyboard shortcuts ( Type '?' )")
* [Confluence Gadgets](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen# "Browse gadgets provided by Confluence")
* [Log In](https://wiki.jasig.org/login.action?os_destination=%2Fdisplay%2FCAS%2FUsing%2BCAS%2Bwithout%2Bthe%2BLogin%2BScreen)
* [Sign Up](https://wiki.jasig.org/signup.action)

1. [Dashboard](https://wiki.jasig.org/dashboard.action "Go to Dashboard ( Type 'g' then 'd' )")
1. [Central Authentication Service](https://wiki.jasig.org/display/CAS)
1. **…**
1. [Home](https://wiki.jasig.org/display/CAS/Home)
1. [Using CAS without the Login Screen]()

* [Tools](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#)

* [Attachments (0)](https://wiki.jasig.org/pages/viewpageattachments.action?pageId=3604523 "View Attachments ( Type 't' )")
* [Page History](https://wiki.jasig.org/pages/viewpreviousversions.action?pageId=3604523)
* [Restrictions](https://wiki.jasig.org/pages/viewinfo.action?pageId=3604523 "Edit restrictions")

* [Info](https://wiki.jasig.org/pages/viewinfo.action?pageId=3604523)
* [Link to this Page…](https://wiki.jasig.org/pages/viewinfo.action?pageId=3604523 "Link to this Page ( Type 'k' )")
* [View in Hierarchy](https://wiki.jasig.org/pages/listpages-dirview.action?key=CAS&openId=3604523#selectedPageInHierarchy)
* [View Source](https://wiki.jasig.org/plugins/viewsource/viewpagesrc.action?pageId=3604523)

# [![]()](https://wiki.jasig.org/display/CAS)  [Using CAS without the Login Screen]()

[Skip to end of metadata](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#page-metadata-end)

* [Page restrictions apply](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen# "Page restrictions apply. Click the lock icon to view or edit the restriction.")
* Added by [Konrad Wulf](https://wiki.jasig.org/display/~jacomac), last edited by [newacct](https://wiki.jasig.org/display/~newacct) on Aug 28, 2010  ([view change](https://wiki.jasig.org/pages/diffpages.action?pageId=3604523&originalId=52495897))
* [show comment](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#) [hide comment](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#)
**Comment:** Corrected links that should have been relative instead of absolute.
[Go to start of metadata](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#page-metadata-start)

## Motivation

We wanted to be more flexible in the use of the login UI, so e.g. wanted to embed it in several places as a small panel. Moreover, we wanted to understand CAS as a pure service, not having to maintain layout information twice. Also, we wanted to support both, login postings from another site and direct login at the CAS server as a fallback.

## Proposed Solutions that do not work 

There have been two proposals, which we haven't found to be suitable:

* AJAX: Ajax requests are sandboxed and that sandbox is even more strict than the two-dot rule applied to cookies: It really has to be exactly the same server name in order to work
* IFrames: In this [posting to the cas-dev mailing list](http://tp.its.yale.edu/pipermail/cas-dev/2006-October/001451.html), it has been suggested to use an iFrame to embed a login panel on a remote site. This has two limitations:

* How to get rid of the Frame after login? in the default view a successful login would lead to redirect only in that frame. If you use target="parent" in the from tag, how do you display errors then?
* You would have limited layout possibilities.

 So for our requirements, none of the proposed solutions were adequate.

## Our Solution

We decided to use Javascript triggered redirects, which allows us to use this mechanism also on static html pages (e.g. CMS based contents). While loading we request a login ticket, then the login form displays and then it submits to the CAS login URL. If Errors are present at CAS we redirect to the referring page, adding a parameter "error_message" to that request.

### Client side 

Here is the static remote login form we use:
[?](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#)<!DOCTYPE html PUBLIC

"-//W3C//DTD HTML 4.01 Transitional//EN"

"[http://www.w3.org/TR/html4/loose.dtd"](http://www.w3.org/TR/html4/loose.dtd)

>

<html>
<head>

<meta http-equiv=

"Content-Type"

content=

"text/html; charset=ISO-8859-1"

>
<title>Test remote Login using JS</title>

<script type=

"text/javascript"

>
function prepareLoginForm() {

    

$(

'myLoginForm'

).action = casLoginURL;
    

$(

"lt"

).value = loginTicket;

}
 

function checkForLoginTicket() {
    

var loginTicketProvided =

false

;

    

var query   =

''

;
        

casLoginURL =

'[https://pcdt057:8443/cas/login'](https://pcdt057:8443/cas/login')

;

        

thisPageURL =

'[https://pcdt057:8443/cas/test-remote-login-js+error.html'](https://pcdt057:8443/cas/test-remote-login-js+error.html')

;
        

casLoginURL +=

'?login-at='

+ encodeURIComponent (thisPageURL);

 
    

query   = window.location.search;

    

query   = query.substr (

1

);
 

 
    

var param   =

new

Array();

    

//var value = new Array();
    

var temp    =

new

Array();

    

param   = query.split (

'&'

);
 

    

i =

0

;
    

while

(param[i]) {

        

temp        = param[i].split (

'='

);
        

if

(temp[

0

] ==

'lt'

) {

            

loginTicket = temp[

1

];
            

loginTicketProvided =

true

;

        

}
        

if

(temp[

0

] ==

'error_message'

) {

                

error = temp[

1

];
            

}

        

i++;
    

}

    

if

(!loginTicketProvided) {
        

location.href = casLoginURL +

'&get-lt=true'

;

    

}
}

 
function $(id) {

    

return

document.getElementById(id);
}

var loginTicket;
var error;

var casLoginURL;
var thisPageURL;

 
checkForLoginTicket();

onload = prepareLoginForm;
</script>

</head>
<body>

<h2>Test remote Login using JS</h2>
<form id=

"myLoginForm"

action=

""

method=

"post"

>

<input type=

"hidden"

value=

"submit"

name=

"_eventId"

/>
<table>

<tr>
    

<td id=

"txt_error"

colspan=

"2"

>

 
    

<script type=

"text/javascript"

language=

"javascript"

>

    

<!--
    

if

( error ) {

        

error = decodeURIComponent (error);
        

document.write (error);

    

}
    

//-->

    

</script>
 

    

</td>
</tr>

<tr>
    

<td>Benutzer:</td>

    

<td><input type=

"text"

value=

"konrad"

name=

"username"

></td>
</tr>

<tr>
    

<td>Password:</td>

    

<td><input type=

"text"

value=

"konrad"

name=

"password"

></td>
</tr>

<tr>
    

<td>Login Ticket:</td>

    

<td><input type=

"text"

name=

"lt"

id=

"lt"

value=

""

></td>
</tr>

<tr>
    

<td>Service:</td>

    

<td><input type=

"text"

name=

"service"

value=

"[http://quiz.jacomac.de"](http://quiz.jacomac.de/)

></td>
</tr>

<tr>
    

<td align=

"right"

colspan=

"2"

><input type=

"submit"

/></td>

</tr>
</table>

</form>
</body>

</html>

### CAS side

Adapting only the views and introducing if-else statements there meant having logic that belongs to the controllers in the view. Therefore we decided to inject our add-on into the spring web flow. Here are the changes we have made to login-webflow.xml (changes highlighted in blue):
[?](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#)<?xml version=

"1.0"

encoding=

"UTF-8"

?>

<flow xmlns=

"[http://www.springframework.org/schema/webflow"](http://www.springframework.org/schema/webflow)
    

xmlns:xsi=

"[http://www.w3.org/2001/XMLSchema-instance"](http://www.w3.org/2001/XMLSchema-instance)

    

xsi:schemaLocation="
              

http:

//www.springframework.org/schema/webflow

              

http:

//www.springframework.org/schema/webflow/spring-webflow-1.0.xsd">

<start-state idref="provideLoginTicketToRemoteRequestor" />

<action-state id="provideLoginTicketToRemoteRequestor">
<action bean="provideLoginTicketToRemoteRequestorAction" />
<transition on="loginTicketRequested" to="viewRedirectToRequestor" />
<transition on="continue" to="automaticCookiePathSetter" />
</action-state>

<view-state id="viewRedirectToRequestor" view="bmRedirectToRequestorView">
<transition on="submit" to="bindAndValidate" />
</view-state>
[?](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#)<action-state id=

"automaticCookiePathSetter"

>

        

<action bean=

"automaticCookiePathSetterAction"

/>
        

<transition on=

"success"

            

to=

"ticketGrantingTicketExistsCheckAction"

/>
    

</action-state>

 
    

<action-state id=

"ticketGrantingTicketExistsCheckAction"

>

        

<action bean=

"ticketGrantingTicketExistsAction"

/>
        

<transition on=

"ticketGrantingTicketExists"

            

to=

"hasServiceCheck"

/>
        

<transition on=

"noTicketGrantingTicketExists"

            

to=

"gatewayRequestCheck"

/>
    

</action-state>

 
    

<action-state id=

"gatewayRequestCheck"

>

        

<action bean=

"gatewayRequestCheckAction"

/>
        

<transition on=

"gateway"

to=

"redirect"

/>

        

<transition on=

"authenticationRequired"

to=

"viewLoginForm"

/>
    

</action-state>

 
    

<action-state id=

"hasServiceCheck"

>

        

<action bean=

"hasServiceCheckAction"

/>
        

<transition on=

"authenticatedButNoService"

            

to=

"viewGenericLoginSuccess"

/>
        

<transition on=

"hasService"

to=

"renewRequestCheck"

/>

    

</action-state>
 

    

<action-state id=

"renewRequestCheck"

>
        

<action bean=

"renewRequestCheckAction"

/>

        

<transition on=

"authenticationRequired"

to=

"viewLoginForm"

/>
        

<transition on=

"generateServiceTicket"

            

to=

"generateServiceTicket"

/>
    

</action-state>

 
    

<!--

        

<action-state id=

"startAuthenticate"

>
        

<action bean=

"x509Check"

/>

        

<transition on=

"success"

to=

"sendTicketGrantingTicket"

/>
        

<transition on=

"error"

to=

"viewLoginForm"

/>

        

</action-state>
    

-->

    

<view-state id=

"viewLoginForm"

view=

"casLoginView"

>
        

<transition on=

"submit"

to=

"bindAndValidate"

/>

    

</view-state>
 

    

<action-state id=

"bindAndValidate"

>
        

<action bean=

"authenticationViaFormAction"

/>

        

<transition on=

"success"

to=

"submit"

/>
        

<transition on=

"error"

to=

"viewLoginForm"

/>

    

</action-state>
 

    

<action-state id=

"submit"

>
        

<action bean=

"authenticationViaFormAction"

method=

"submit"

/>

        

<transition on=

"warn"

to=

"warn"

/>
        

<transition on=

"success"

to=

"sendTicketGrantingTicket"

/>

        

<transition on=

"error"

to=

"viewLoginForm"

/>

<transition on="errorForRemoteRequestor" to="viewRedirectToRequestor" />
[?](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#)</action-state>

 
    

<action-state id=

"sendTicketGrantingTicket"

>

        

<action bean=

"sendTicketGrantingTicketAction"

/>
        

<transition on=

"success"

to=

"serviceCheck"

/>

    

</action-state>
 

    

<action-state id=

"serviceCheck"

>
        

<action bean=

"hasServiceCheckAction"

/>

        

<transition on=

"authenticatedButNoService"
            

to=

"viewGenericLoginSuccess"

/>

        

<transition on=

"hasService"

to=

"generateServiceTicket"

/>
    

</action-state>

 
    

<action-state id=

"generateServiceTicket"

>

        

<action bean=

"generateServiceTicketAction"

/>
        

<transition on=

"success"

to=

"warn"

/>

        

<transition on=

"error"

to=

"viewLoginForm"

/>
        

<transition on=

"gateway"

to=

"redirect"

/>

    

</action-state>
 

    

<!--
        

The

"warn"

action makes the determination of whether to redirect directly to the requested

        

service or display the

"confirmation"

page to go back to the server.
    

-->

    

<action-state id=

"warn"

>
        

<action bean=

"warnAction"

/>

        

<transition on=

"redirect"

to=

"redirect"

/>
        

<transition on=

"warn"

to=

"showWarningView"

/>

    

</action-state>
 

    

<!--
        

the

"viewGenericLogin"

is the end state

for

when a user attempts to login without coming directly from a service.

        

They have only initialized their single-sign on session.
    

-->

    

<end-state id=

"viewGenericLoginSuccess"
        

view=

"casLoginGenericSuccessView"

/>

 
 

    

<!--
        

The

"showWarningView"

end state is the end state

for

when the user has requested privacy settings (to be

"warned"

) to be turned on.  It delegates to a

        

view defines in default_views.properties that display the

"Please click here to go to the service."

message.
    

-->

    

<end-state id=

"showWarningView"

view=

"casLoginConfirmView"

/>
 

    

<!--
        

The

"redirect"

end state allows CAS to properly end the workflow

while

still redirecting

        

the user back to the service required.
    

-->

    

<end-state id=

"redirect"
        

view=

"externalRedirect:${externalContext.requestParameterMap['service']}${requestScope.ticket == null ? '' : (externalContext.requestParameterMap['service'].indexOf('?') != -1 ? '&amp;' : '?') + 'ticket=' + requestScope.ticket}"

/>

 
    

<global-transitions>

        

<transition to=

"viewServiceErrorView"
            

on-exception=

"org.jasig.cas.services.UnauthorizedServiceException"

/>

    

</global-transitions>
</flow>

##  Problems encountered and suggested solutions

The solution works well, but it has a very dirty edge: introducing a new event in the submit method that allows us to distinguish between a login posting from the CAS Site and another site. Here we had to redeclare the *submit* method in *org.jasig.cas.web.flow.AuthenticationViaFormAction* (changes marked in blue):
[?](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#)[...]        

try

{

            

final

String ticketGrantingTicketId =

this

.centralAuthenticationService
                

.createTicketGrantingTicket(credentials);

            

ContextUtils.addAttribute(context,
                

AbstractLoginAction.REQUEST_ATTRIBUTE_TICKET_GRANTING_TICKET,

                

ticketGrantingTicketId);
            

setWarningCookie(response, warn);

            

return

success();
        

}

catch

(

final

TicketException e) {

            

populateErrorsInstance(context, e);

            // START: ChangesBusinessMart: check, whether the posting has been sent from a remote server
            String myServerName = request.getLocalName();
            String referrer = request.getParameter("login-at");
            if (referrer != null && referrer.indexOf(myServerName) == -1) {
                return result("errorForRemoteRequestor");
            }

[?](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#)return

error();

        

}
[...]

 This could have been avoided by

1. making this submit method overridable (non-final) to add new events in a derived class. Or
1. to make such a distinction of events in the standard CAS distribution.

Maybe this can be accomplished somehow in future versions of CAS?

## Listing of the other files mentioned in this solution 

provideLoginTicketToRemoteRequestorAction -> introduces a new parameter get-lt to signal that a new login ticket shall be issued to the HTTP Referer:
[?](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#)package

de.businessmart.sso.cas.flow;

 
import

javax.servlet.http.HttpServletRequest;

import

javax.servlet.http.HttpServletResponse;
 

import

org.jasig.cas.web.flow.util.ContextUtils;
import

org.springframework.beans.factory.InitializingBean;

import

org.springframework.webflow.action.AbstractAction;
import

org.springframework.webflow.core.collection.MutableAttributeMap;

import

org.springframework.webflow.execution.Event;
import

org.springframework.webflow.execution.RequestContext;

 
import

de.businessmart.sso.cas.CasUtility;

 
/**

 

* Opens up the CAS web flow to allow external retrieval of a login ticket.
 

* @author konrad.wulf

 

*
 

*/

public

class

ProvideLoginTicketToRemoteRequestorAction

extends

AbstractAction
{

 
    

@Override

    

protected

Event doExecute(RequestContext context)

throws

Exception
    

{

        

final

HttpServletRequest request = ContextUtils.getHttpServletRequest(context);
 

        

if

(request.getParameter(

"get-lt"

) !=

null

&& request.getParameter(

"get-lt"

).equalsIgnoreCase(

"true"

)) {
            

return

result(

"loginTicketRequested"

);

        

}
        

return

result(

"continue"

);

    

}
 

}

viewRedirectToRequestor actually does the redirects to the referrer:

[?](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#)<%

@page

import

=

"de.businessmart.sso.cas.CasUtility"

%>

<%@ taglib prefix=

"c"

uri=

"[http://java.sun.com/jsp/jstl/core"](http://java.sun.com/jsp/jstl/core)

%>
<%@ taglib prefix=

"spring"

uri=

"[http://www.springframework.org/tags"](http://www.springframework.org/tags)

%>

<% String separator =

""

;
String referrer = request.getHeader(

"Referer"

);

referrer = CasUtility.resetUrl(referrer);
if

(referrer !=

null

&& referrer.length() >

0

) {

    

separator =(referrer.indexOf(

"?"

) > -

1

)?

"&"

:

"?"

; %>
<html>

    

<head>
        

<script>

               

var redirectURL =

"<%= referrer + separator %>lt=${flowExecutionKey}"

;
        

<spring:hasBindErrors name=

"credentials"

>

            

redirectURL +=

'&error_message='

+ encodeURIComponent (

'<c:forEach var="error" items="${errors.allErrors}"><spring:message code="${error.code}" text="${error.defaultMessage}" /></c:forEach>'

);
        

</spring:hasBindErrors>

            

window.location.href = redirectURL;
       

</script>

    

</head>
    

<body></body>

</html><%
}

else
{

    

out.print(

"You better know what to do here."

);
} %>

And for completeness, here is the CasUtility that removes obsolete parameters from the query string:
[?](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#)package

de.businessmart.sso.cas;

 
public

class

CasUtility {

    

/**
     

* Removes the previously attached GET parameters "lt" and "error_message" to be able to send new ones.

     

* @param casUrl
     

* @return

     

*/
    

public

static

String resetUrl ( String casUrl) {

        

String cleanedUrl;
        

String[] paramsToBeRemoved =

new

String[]{

"lt"

,

"error_message"

,

"get-lt"

};

        

cleanedUrl = removeHttpGetParameters(casUrl, paramsToBeRemoved);
        

return

cleanedUrl;

    

}
 

 
    

/**

     

* Removes selected HTTP GET parameters from a given URL
     

* @param casUrl

     

* @param paramsToBeRemoved
     

* @return

     

*/
    

public

static

String removeHttpGetParameters (String casUrl, String[] paramsToBeRemoved) {

        

String cleanedUrl = casUrl;
        

if

(casUrl !=

null

)

        

{
            

// check if there is any query string at all

            

if

(casUrl.indexOf(

"?"

) == -

1

)
            

{

                

return

casUrl;
            

}

else

            

{
                

// determine the start and end position of the parameters to be removed

                

int

startPosition, endPosition;
                

boolean

containsOneOfTheUnwantedParams =

false

;

                

for

(String paramToBeErased : paramsToBeRemoved)
                

{

                    

startPosition = -

1

;
                    

endPosition = -

1

;

                    

if

(cleanedUrl.indexOf(

"?"

+ paramToBeErased +

"="

) > -

1

)
                    

{

                        

startPosition = cleanedUrl.indexOf(

"?"
                                

+ paramToBeErased +

"="

) +

1

;

                    

}

else

if

(cleanedUrl.indexOf(

"&"

+ paramToBeErased +

"="

) > -

1

)
                    

{

                        

startPosition = cleanedUrl.indexOf(

"&"
                                

+ paramToBeErased +

"="

) +

1

;

                    

}
                    

if

(startPosition > -

1

)

                    

{
                        

int

temp = cleanedUrl.indexOf(

"&"

, startPosition);

                        

endPosition = (temp > -

1

) ? temp +

1

: cleanedUrl
                                

.length();

                        

// remove that parameter, leaving the rest untouched
                        

cleanedUrl = cleanedUrl.substring(

0

, startPosition)

                                

+ cleanedUrl.substring(endPosition);
                        

containsOneOfTheUnwantedParams =

true

;

                    

}
                

}

 
                

// wenn nur noch das Fragezeichen vom query string übrig oder am schluss ein "&", dann auch dieses entfernen

                

if

(cleanedUrl.endsWith(

"?"

) || cleanedUrl.endsWith(

"&"

))
                

{

                    

cleanedUrl = cleanedUrl.substring(

0

,
                            

cleanedUrl.length() -

1

);

                

}
 

                

// erst zurückgeben, wenn wir auch überprüft haben, ob nicht ein parameter mehrfach angegeben wurde...
                

if

(!containsOneOfTheUnwantedParams)

                    

return

casUrl;
                

else

                    

cleanedUrl = removeHttpGetParameters(cleanedUrl, paramsToBeRemoved);
            

}

        

}
        

return

cleanedUrl;

 
    

}

 
}
Labels:

None

[Edit Labels](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen# "Edit Labels ( Type 'l' )")

## [8 Comments](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#)

[Hide/Show Comments](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen# "Hide/Show Comments")

1. ![User icon: battags]( "battags")

Apr 25, 2007
### [Scott Battaglia](https://wiki.jasig.org/display/~battags)

The IFRAME method actually does work. As stated in emails on the CAS developer's list, it requires replacing the default view with a JavaScript redirect view.

This allows the following:
(a) errors will continue to be displayed correctly (in the iframe)
(b) redirects the browser correctly
(c) usernames and passwords are never collected on the client application

* [Permalink](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen?focusedCommentId=3604556#comment-3604556 "Permanent link to this comment")
* ![User icon: albert4cas]( "albert4cas")

Jul 09, 2007
### [Alberto Mozzone](https://wiki.jasig.org/display/~albert4cas)

Hi there,
I made the changes described here, but I'm stuck with these problems:
- Where the file "viewRedirectToRequestor.jsp" must be placed ?
- There isn't the definition of bean with class "ProvideLoginTicketToRemoteRequestorAction" in "applicationContext.xml".
- After adding it, I get the exception:

2007-07-09 17:08:18,773 ERROR [org.apache.catalina.core.ContainerBase.[Catalina].[auth.domain.com].[/].[cas]] - Servlet.service() for servlet cas threw exception
java.lang.InstantiationException
    at sun.reflect.InstantiationExceptionConstructorAccessorImpl.newInstance(InstantiationExceptionConstructorAccessorImpl.java:30)

And CAS shows its generic exception page "CAS is unavailable".

Where am I wrong ?

Somebody can help, please ?

Thanks in advance.

Alberto

* [Permalink](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen?focusedCommentId=6619787#comment-6619787 "Permanent link to this comment")
* ![User icon: netstokedev@gmail.com]( "netstokedev@gmail.com")

Dec 07, 2007
### [Kevin McKee](https://wiki.jasig.org/display/~netstokedev@gmail.com)

A much simpler and elegant version is to not deal with getting a login ticket at all and to simply auto submit the CAS form via Javascript if a param such as 'auto' is submitted to the login form along with the service, username, and password parameters.

Example of casLoginView.jsp:
[?](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen#) <%

// If we recieve the 'auto' parameter as 'true' we attempt to
// auto populate the form from the request variables and submit it

String auto = request.getParameter(

"auto"

);
if

(auto !=

null

&& auto.equals(

"true"

)) {

%>
<script language=

"javascript"

>

 function doAutoLogin() {
  document.forms[

0

].username.value =

'<%=request.getParameter("username")%>'

;

  document.forms[

0

].password.value =

'<%=request.getParameter("password")%>'

;
  document.forms[

0

].submit();

 }
 

// Add the auto login function to the onload event

 addLoadEvent(doAutoLogin);
</script>

<% 
}

%>    

No fuss, no muss. CAS will then redirect back to your service url with a ticket that is ready to be validated. This comes in really handy if you are doing something like creating users in your system and then auto logging them in after the creation process.

I know purists will hate the fact that we put a scriptlet in there (i'm not too wild about it either), but it is alot easier than all the redirects involved for grabbing a login ticket just to submit credentials.

Just a note, you will also want to change the name of the submit button from 'submit' or else Javascript will have a fit. Hope this helps.

* [Permalink](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen?focusedCommentId=8716750#comment-8716750 "Permanent link to this comment")
* ![User icon: jacomac]( "jacomac")

Jun 06, 2008
### [Konrad Wulf](https://wiki.jasig.org/display/~jacomac)

Here's a tardy response to Alberto Mozzone's post. Sorry for not having answered any earlier. But better late than never ![(wink)]() It still seems to be of interest:

I have forgotten to mention in this article that the view bmRedirectToRequestorView needs to be defined in the default_views.properties file, which is located at "/WEB-INF/classes". The following entries need to be added there:

### Redirect with login ticket view
bmRedirectToRequestorView.(class)=org.springframework.web.servlet.view.JstlView
bmRedirectToRequestorView.url=/WEB-INF/view/jsp/default/ui/bmRedirectToRequestorView.jsp

Moreover, Alberto was also quite right that the bean "provideLoginTicketToRemoteRequestorAction" has to be defined somewhere. I did that in /WEB-INF/cas-servlet.xml:

id="provideLoginTicketToRemoteRequestorAction"
class="de.businessmart.sso.cas.flow.ProvideLoginTicketToRemoteRequestorAction" />

With these 2 little additions the solution description should be complete.

* [Permalink](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen?focusedCommentId=13570147#comment-13570147 "Permanent link to this comment")
* ![User icon: gauravsaxena]( "gauravsaxena")

Oct 13, 2010
### [gaurav saxena](https://wiki.jasig.org/display/~gauravsaxena)

Hi,

This was a nice article. But i need to do the same with that latest version of CAS i.e. 3.4 . Can you please provide me the steps for the same. Atleast the stucture for login-webflow.xml as it is now using spring-webflow-2.0.xsd .

Thanks in advance.

Gaurav Saxena

* [Permalink](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen?focusedCommentId=37946049#comment-37946049 "Permanent link to this comment")

1. ![User icon: eholderman]( "eholderman")

Feb 02, 2011
### [Ed Holderman](https://wiki.jasig.org/display/~eholderman)

I have the same question.  Does anyone have any tips on doing this in CAS 3.4.x and is there now a different way of accomplishing it with the new version?  My end goal is to alter the Liferay login portlet to submit credentials to CAS over https.  Out of the box IFrame portlets would hard code the service URL, breaking deep link capabilities.  Thank you - Ed

* [Permalink](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen?focusedCommentId=39813346#comment-39813346 "Permanent link to this comment")
* ![User icon: rdemuth]( "rdemuth")

Jun 13, 2011
### [Robert DeMuth](https://wiki.jasig.org/display/~rdemuth)

Looking to do the same thing in CAS 3.4.6 (using the Maven2 overlay method).  It looks like login-webflow.xml has changed dramatically since this page was created.

* [Permalink](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen?focusedCommentId=46137905#comment-46137905 "Permanent link to this comment")

1. ![User icon: patino@fordham.edu]( "patino@fordham.edu")

Oct 17, 2011
### [jpats](https://wiki.jasig.org/display/~patino@fordham.edu)

Hi, I just want to find out if you were able to have this working. If you did, really would like to find out how you did it. Thanks in advance.

* [Permalink](https://wiki.jasig.org/display/CAS/Using+CAS+without+the+Login+Screen?focusedCommentId=50857235#comment-50857235 "Permanent link to this comment")
Powered by a free **Atlassian Confluence Open Source Project License** granted to Java Architectures Special Interest Group. [Evaluate Confluence today](http://www.atlassian.com/c/conf/11461).
This Confluence installation runs a Free Gliffy License - [Evaluate Gliffy](http://www.gliffy.com/products/confluence-plugin/) for your Wiki!

* Powered by [Atlassian Confluence](http://www.atlassian.com/software/confluence) 4.1.5, the [Enterprise Wiki](http://www.atlassian.com/software/confluence/tour/enterprise-wiki.jsp)
* Printed by Atlassian Confluence 4.1.5, the Enterprise Wiki.
*  ·  [Report a bug](http://jira.atlassian.com/secure/BrowseProject.jspa?id=10470)
*  ·  [Atlassian News](http://www.atlassian.com/about/connected.jsp?s_kwcid=Confluence-stayintouch)

[]()[]()[]()
