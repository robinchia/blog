---
layout: post
title: "HttpClient - HttpClient Logging Practices"
categories: Httpclient
tags: 
 - Httpclient
--- 

# HttpClient - HttpClient Logging Practices

[![Apache Software Foundation]()](http://httpcomponents.apache.org/)[![HttpClient]()](http://hc.apache.org/httpclient-3.x/)

Last published: 08 February 2008 | Doc for 3.1
### Overview

* [Features](http://hc.apache.org/httpclient-3.x/features.html)
* [News](http://hc.apache.org/httpclient-3.x/news.html)
* [Status](http://hc.apache.org/status.html)
* [Download](http://hc.apache.org/downloads.cgi)
* [Wiki](http://wiki.apache.org/jakarta-httpclient/ "External Link")

### User Guide

* [Overview](http://hc.apache.org/httpclient-3.x/userguide.html)
* [Authentication Guide](http://hc.apache.org/httpclient-3.x/authentication.html)
* [Character Encodings](http://hc.apache.org/httpclient-3.x/charencodings.html)
* [Cookies](http://hc.apache.org/httpclient-3.x/cookies.html)
* [Exception Handling](http://hc.apache.org/httpclient-3.x/exception-handling.html)
* **[Logging Guide](http://hc.apache.org/httpclient-3.x/logging.html)**
* [Methods](http://hc.apache.org/httpclient-3.x/methods.html)
* [Optimization Guide](http://hc.apache.org/httpclient-3.x/performance.html)
* [Preference Architecture](http://hc.apache.org/httpclient-3.x/preference-api.html)
* [Redirects Handling](http://hc.apache.org/httpclient-3.x/redirects.html)
* [Sample Code](http://svn.apache.org/viewvc/httpcomponents/oac.hc3x/trunk/src/examples/ "External Link")
* [SSL Guide](http://hc.apache.org/httpclient-3.x/sslguide.html)
* [Threading](http://hc.apache.org/httpclient-3.x/threading.html)
* [Trouble Shooting](http://hc.apache.org/httpclient-3.x/troubleshooting.html)
* [Tutorial](http://hc.apache.org/httpclient-3.x/tutorial.html)
### ASF

* [Foundation](http://www.apache.org/foundation/ "External Link")
* [Sponsor Apache](http://www.apache.org/foundation/sponsorship.html "External Link")
* [Thanks](http://www.apache.org/foundation/thanks.html "External Link")

### Project Documentation

* [About](http://hc.apache.org/httpclient-3.x/index.html)
* [Project Info](http://hc.apache.org/httpclient-3.x/project-info.html)
* [Project Reports](http://hc.apache.org/httpclient-3.x/maven-reports.html)
* [Development Process](http://hc.apache.org/httpclient-3.x/development-process.html)
### Legend

* External Link
* Opens in a new window
[![Built by Maven]()](http://maven.apache.org/ "Built by Maven")

[]()

## Logging Practices

Being a library HttpClient is not to dictate which logging framework the user has to use. Therefore *HttpClient* utilizes the logging interface provided by the [Commons Logging](http://commons.apache.org/logging/ "External Link") package. *Commons Logging* provides a simple and generalized [log interface](http://commons.apache.org/logging/commons-logging-1.0.4/docs/apidocs/ "External Link") to various logging packages. By using *Commons Logging*, *HttpClient* can be configured for a variety of different logging behaviours. That means the user will have to make a choice which logging framework to use. By default *Commons Logging* supports the following logging frameworks:

* [Log4J](http://logging.apache.org/log4j/docs/index.html "External Link")
* [java.util.logging](http://java.sun.com/j2se/1.4.2/docs/api/java/util/logging/package-summary.html "External Link")
* [SimpleLog](http://commons.apache.org/logging/commons-logging-1.0.4/docs/apidocs/org/apache/commons/logging/impl/SimpleLog.html "External Link") (internal to *Commons Logging*)
By implementing some simple interfaces *Commons Logging* can be extended to support basically any other custom logging framework. *Commons Logging* tries to automatically discover the logging framework to use. If it fails to select the expected one, you must configure *Commons Logging* by hand. Please refer to the *Commons Logging* documentation for more information.

*HttpClient* performs two different kinds of logging: the standard context logging used within each class, and wire logging.
[]()

### Context Logging

Context logging contains information about the internal operation of HttpClient as it performs HTTP requests. Each class has its own log named according to the class's fully qualified name. For example the class

HttpClient
has a log named

org.apache.commons.httpclient.HttpClient
. Since all classes follow this convention it is possible to configure context logging for all classes using the single log named

org.apache.commons.httpclient
.

[]()

### Wire Logging

The wire log is used to log all data transmitted to and from servers when executing HTTP requests. This log should only be enabled to debug problems, as it will produce an extremely large amount of log data, some of it in binary format.

Because the content of HTTP requests is usually less important for debugging than the HTTP headers, these two types of data have been separated into different wire logs. The content log is

httpclient.wire.content
and the header log is

httpclient.wire.header
.

[]()

## Configuration Examples

*Commons Logging* can delegate to a variety of loggers for processing the actual output. Below are configuration examples for *Commons Logging*, *Log4j* and *java.util.logging*.
[]()

### Commons Logging Examples

*Commons Logging* comes with a basic logger called

SimpleLog
. This logger writes all logged messages to

System.err
. The following examples show how to configure *Commons Logging* via system properties to use

SimpleLog
.

**Note:** The system properties must be set before a reference to any *Commons Logging* class is made.

Enable header wire + context logging - **Best for Debugging**
System.setProperty("org.apache.commons.logging.Log", "org.apache.commons.logging.impl.SimpleLog");
System.setProperty("org.apache.commons.logging.simplelog.showdatetime", "true");
System.setProperty("org.apache.commons.logging.simplelog.log.httpclient.wire.header", "debug");
System.setProperty("org.apache.commons.logging.simplelog.log.org.apache.commons.httpclient", "debug");

Enable full wire(header and content) + context logging
System.setProperty("org.apache.commons.logging.Log", "org.apache.commons.logging.impl.SimpleLog");
System.setProperty("org.apache.commons.logging.simplelog.showdatetime", "true");
System.setProperty("org.apache.commons.logging.simplelog.log.httpclient.wire", "debug");
System.setProperty("org.apache.commons.logging.simplelog.log.org.apache.commons.httpclient", "debug");

Enable just context logging
System.setProperty("org.apache.commons.logging.Log", "org.apache.commons.logging.impl.SimpleLog");
System.setProperty("org.apache.commons.logging.simplelog.showdatetime", "true");
System.setProperty("org.apache.commons.logging.simplelog.log.org.apache.commons.httpclient", "debug");

[]()

### Log4j Examples

The simplest way to configure [Log4j](http://logging.apache.org/log4j/ "External Link") is via a log4j.properties file. *Log4j* will automatically read and configure itself using a file named log4j.properties when it's present at the root of the application classpath. Below are some *Log4j* configuration examples.

**Note:** *Log4j* is not included in the *HttpClient* distribution.

Enable header wire + context logging - **Best for Debugging**
log4j.rootLogger=INFO, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%c] %m%n
log4j.logger.httpclient.wire.header=DEBUG
log4j.logger.org.apache.commons.httpclient=DEBUG

Enable full wire(header and content) + context logging
log4j.rootLogger=INFO, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%c] %m%n
log4j.logger.httpclient.wire=DEBUG
log4j.logger.org.apache.commons.httpclient=DEBUG

Log wire to file + context logging
log4j.rootLogger=INFO
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%c] %m%n
log4j.appender.F=org.apache.log4j.FileAppender
log4j.appender.F.File=wire.log
log4j.appender.F.layout=org.apache.log4j.PatternLayout
log4j.appender.F.layout.ConversionPattern =%5p [%c] %m%n
log4j.logger.httpclient.wire=DEBUG, F
log4j.logger.org.apache.commons.httpclient=DEBUG, stdout

Enable just context logging
log4j.rootLogger=INFO, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%c] %m%n
log4j.logger.org.apache.commons.httpclient=DEBUG

Note that the default configuration for Log4J is very inefficient as it causes all the logging information to be generated but not actually sent anywhere. The Log4J manual is the best reference for how to configure Log4J. It is available at [http://logging.apache.org/log4j/docs/manual.html](http://logging.apache.org/log4j/docs/manual.html "External Link")
[]()

### java.util.logging Examples

Since JDK 1.4 there has been a package [java.util.logging](http://java.sun.com/j2se/1.4.2/docs/api/java/util/logging/package-summary.html "External Link") that provides a logging framework similar to *Log4J*. By default it reads a config file from

$JAVA_HOME/jre/lib/logging.properties
which looks like this (comments stripped):
handlers=java.util.logging.ConsoleHandler
.level=INFO
java.util.logging.FileHandler.pattern = %h/java%u.log
java.util.logging.FileHandler.limit = 50000
java.util.logging.FileHandler.count = 1
java.util.logging.FileHandler.formatter = java.util.logging.XMLFormatter
java.util.logging.ConsoleHandler.level = INFO
java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter
com.xyz.foo.level = SEVERE
To customize logging a custom

logging.properties
file should be created in the project directory. The location of this file must be passed to the JVM as a system property. This can be done on the command line like so: $JAVA_HOME/java -Djava.util.logging.config.file=$HOME/myapp/logging.properties -classpath $HOME/myapp/target/classes com.myapp.MainAlternatively [LogManager#readConfiguration(InputStream)](http://java.sun.com/j2se/1.4.2/docs/api/java/util/logging/LogManager.html#readConfiguration(java.io.InputStream) "External Link") can be used to pass it the desired configuration.

Enable header wire + context logging - **Best for Debugging**
.level=INFO
handlers=java.util.logging.ConsoleHandler
java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter
httpclient.wire.header.level=FINEST
org.apache.commons.httpclient.level=FINEST

Enable full wire(header and content) + context logging
.level=INFO
handlers=java.util.logging.ConsoleHandler
java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter
httpclient.wire.level=FINEST
org.apache.commons.httpclient.level=FINEST

Enable just context logging
.level=INFO
handlers=java.util.logging.ConsoleHandler
java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter
org.apache.commons.httpclient.level=FINEST

More detailed information is available from the [Java Logging documentation](http://java.sun.com/j2se/1.4.2/docs/guide/util/logging/overview.html "External Link").
© 2001-2008, Apache Software Foundation
