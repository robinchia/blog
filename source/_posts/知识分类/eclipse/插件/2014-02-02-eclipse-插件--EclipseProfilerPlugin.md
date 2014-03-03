---
layout: post
title: "Eclipse Profiler Plugin"
categories: eclipse
tags: 
 - eclipse
 - 插件
--- 

# Eclipse Profiler Plugin

# Eclipse Profiler Plugin

This is a plugin for the Eclipse platform which allows java code profiling. [Project](http://sourceforge.net/projects/eclipsecolorer)

## License

CPL.

## General note

1.) if you run the remote profiler, the filter settings in the eclipse environment are not taken into account. Instead you must provide the filter settings in the start command of your application server. You do this by adding an environment variable __PROFILER_PACKAGE_FILTER to the startup command of your aplication, see below. 2.) Setting up the profiler package filters (in general) You must define the environment variable __PROFILER_PACKAGE_FILTER as shown below in the examples for tomcat, jboss, weblogic and resin. General important Note: The environment variable contains following parts: the application starting class (__A__) inclusion filters (__P__) exclusion filters (__M__) You must provide exactly one starting class, but you can have multiple inclusion and exclusion filters. The parts MUST be separated by the OS specific path.separator, i.e. ":" for unix or ";" for WINDOWS platforms) example package filter setting for WINDOWS: -D__PROFILER_PACKAGE_FILTER=__A__org.apache.catalina.startup.Bootstrap;__M__sun.;__P__my.company.classes. example package filter setting for UNIX/LINUX: -D__PROFILER_PACKAGE_FILTER=__A__org.apache.catalina.startup.Bootstrap:__M__sun.:__P__my.company.classes. In the examples below we provide the WINDOWS style. Please take care to use the LINUX/UNIX style if that aplies to you.

## Win32 installation

Copy ProfilerDLL.dll from root plugin folder into bin folder of your JRE installation. You can skip this step, plugin will ask you and copy DLL into your JRE\BIN when you will start profiling local application inside of Eclipse first time. It will also check, that you have in JRE\BIN same DLL as in plugin directory.

## Linux installation

Profiler has native part compiled with gcc 3.2, but if you have old gcc or libraries you can build native part yourself. Extract files from native\profiler_linux.tgz and look at script "m". This is example of compilation script. Change it as needed for you OS.
See also profile_cpu/profile_heap for examples of start line for cpu and heap profiling and r_cpu/r_heap as example how to use them.

## Usage

Profiler plugin creates additional kind of launch configuration in Run menu. Profiler tab allows the user to define packages which shouldn't be instrumented, thus, all time usage will be referred to calling methods.
You can specify refresh rate, i.e. how often plugin will read statistics from prolifing JVM.
You can also set method, how to get time of enter in method and leave. There are two methods: fast, but usefull only if you have one active thread, which tries to use all CPU; or slow, which use JVMPI function GetCurrentThreadCpuTime() and allows detect how much of CPU was used by thread. However with this method profiled program runs about 3 times slower than with fast method.
![]()
**Fig. 1. Launch configuration.**

Profiler supports inclusive (grren) and exlusive (red) filters. You can noy only specify what packages should be excluded from instrumentation, but also what packages should be included. Usefull, if you want to profile only your own classes. You can move filters up and down using buttons or drag and drop.
![]()
**Fig. 1.1. Drag filters.**

Plugin allows remote profiling with "Remote Profiler" launch configuration. Notice, that remote profiling is supported only in "run" mode, i.e. when you create launch configuration using "Run|Run..." menu item.
You need to start remote application with special switches like: java -XrunProfilerDLL -Xbootclasspath/a:jakarta-regexp.jar;profiler_trace.jar;commons-lang.jar -D__PROFILER_PACKAGE_FILTER=__M__sun.;__M__com.sun. -D__PROFILER_USE_PACKAGE_FILTER=1 Here __M__ prefix used for exclusive filter, __P__ can be used for inclusive filter. And you should use __A__ for class with method "main".
![]()
**Fig. 2. Remote launch configuration.**

## Tomcat CPU profiling

Profiler was tested with jakarta-tomcat-4.1.12. Add after lines in catalina.bat:
set _EXECJAVA=%_RUNJAVA%
set MAINCLASS=org.apache.catalina.startup.Bootstrap
set ACTION=start
set SECURITY_POLICY_FILE=
set DEBUG_OPTS=
set JPDA=
following line:
set JAVA_OPTS=-XrunProfilerDLL:1 -Xbootclasspath/a:jakarta-regexp.jar;profiler_trace.jar;commons-lang.jar -D__PROFILER_PACKAGE_FILTER=__A__%MAINCLASS%;__M__sun.;__M__com.sun.;__M__java.;__M__javax.;__M__org.apache. -D__PROFILER_TIMING_METHOD=1
Then copy jar's: commons-lang.jar jakarta-regexp.jar profiler_trace.jar to bin directory of tomcat. Now you can start tomcat using startup.bat and it will gather statistics for you. Next step is configuring Eclipse. You should create remote launch configuration and set host address as needed. This is all. Launch it, plugin will connect to your Tomcat and show you some statistics.

## Tomcat heap profiling

Profiling heap is almost same as CPU profiling, but you should use following line:
set JAVA_OPTS=-XrunProfilerDLL:3,10,0 -D__PROFILER_PROFILE_HEAP=1 -Xbootclasspath/a:jakarta-regexp.jar;profiler_trace.jar;commons-lang.jar -D__PROFILER_PACKAGE_FILTER=__A__%MAINCLASS%;__M__ -D__PROFILER_TIMING_METHOD=1

## JBoss profiling

Profiler was tested with jboss-3.0.6_tomcat-4.1.18. Add following line in your bin/run.bat (directly after echo off):
set JAVA_OPTS=-XrunProfilerDLL:1 -Xbootclasspath/a:jakarta-regexp.jar;profiler_trace.jar;commons-lang.jar -D__PROFILER_PACKAGE_FILTER=__A__org.jboss.Main;__M__sun.;__M__com.sun.;__M__java.;__M__javax. -D__PROFILER_TIMING_METHOD=1
Then copy jar's: commons-lang.jar jakarta-regexp.jar profiler_trace.jar to bin directory of JBoss. Now you can start JBoss using run.bat and it will gather statistics for you. Next step is configuring Eclipse. You should create remote launch configuration and set host address as needed. This is all. Launch it, plugin will connect to your JBoss and show you some statistics.

## WebLogic profiling

Profiler was tested with WebLogic 8.1, installed in "c:/bea". WebLogic has its own JRE's, so you will need to copy ProfilerDLL.dll manually to C:\bea\jdk141_02\jre\bin. For profiling examples, change file C:\bea\weblogic81\samples\domains\examples\startExamplesServer.cmd, line where JAVA_OPTIONS is defined:
set JAVA_OPTIONS=-XrunProfilerDLL:1 -Xbootclasspath/a:jakarta-regexp.jar;profiler_trace.jar;commons-lang.jar -D__PROFILER_PACKAGE_FILTER=__A__weblogic.Server;__M__sun.;__M__com.sun.;__M__java.;__M__javax.;__M__weblogic. -D__PROFILER_TIMING_METHOD=1
Then copy jar's: commons-lang.jar jakarta-regexp.jar profiler_trace.jar to "C:\bea\weblogic81\samples\domains\examples". Now you can start WebLogic examplex using startExamplesServer.cmd, or shortcut in Windows menu, and it will gather statistics for you. Next step is configuring Eclipse. You should create remote launch configuration and set host address as needed. This is all. Launch it, plugin will connect to your WebLogic and show you some statistics.

## Resin profiling

Profiler was tested with Resin-ee 2.1.10. Create batch file with following content in "bin":
httpd.exe -J-XrunProfilerDLL:1 -Xbootclasspath/a:c:/prof/jakarta-regexp.jar;c:/prof/profiler_trace.jar;c:/prof/commons-lang.jar -D__PROFILER_PACKAGE_FILTER=__A__com.caucho.server.http.HttpServer;__M__sun.;__M__com.sun.;__M__java.;__M__javax. -D__PROFILER_TIMING_METHOD=1
Then create directory "prof" in disc "C" and copy jar's: commons-lang.jar jakarta-regexp.jar profiler_trace.jar. Now you can start your batch file and it will gather statistics for you. Next step is configuring Eclipse. You should create remote launch configuration and set host address as needed. This is all. Launch it, plugin will connect to your Resin and show you some statistics.

## Options

Profiler supports several options via -DXXX.
Options Description __PROFILER_PACKAGE_FILTER Contains list of packages to include or exclude.
Here __P__ - inclusive pattern, __M__ - exclusive pattern.
And __A__ - application start class.
Examples:
Include only ru.* and de.* packages: __P__ru.;__P__de.
Exclude system packages: __A__org.jboss.Main;__M__sun.;__M__com.sun.;__M__java.;__M__javax.
If at least one inclusive pattern used, only classes accepted by inclusive patterns will be instrumented. __PROFILER_TIMING_METHOD Specifies timing method - how to measure elapsed time.
0 - fast, System.currentTimeMillis(), good for application without sleeps and waits.
1 - precise, thread aware, slow, good for multithreaded applications.
2 - sampling profiling, very fast, good for long runned processes. __PROFILER_PROFILE_HEAP 1, if HEAP profiling should be used. __PROFILER_AUTO_START 0 - don't start automatically.
1 (default) - start automatically. __PROFILER_START_ON_METHOD Start method name. When applications tries to enter in this method profiling starts (see __PROFILER_AUTO_START, it should be 0).
Example: -D__PROFILER_START_ON_METHOD=ru.nlmk.train.Main.mainLoop __PROFILER_PAUSE_ON_METHOD 1, if you need pause profiling when __PROFILER_START_ON_METHOD leaved. __PROFILER_INSTRUMENT_SYSTEM_CLASSES 1, if you need instrument additional system classes, like java.lang.String, etc. Expensive! __PROFILER_WAIT_FRONTEND_CONNECT 1, if you need to wait, until frontend (plugin) will connect. (0 by default).

## CPU profiling

Profiler supports CPU profiling and basic function for heap profiling. The profiler collects invocation count and direct time statistics for every method. Direct time is the amount of time used by selected method for execute. Also a total time value displayed in calling tree for selected thread. The total time means a time used by selected method and by all the methods it had called.

Only instrumentation profiling method is supported because of its precise. The overhead expenses reflects at speed decreasing app. 5 times to normal.

## How it works

The profiler subscribes for JVPMI event JVMPI_EVENT_CLASS_LOAD_HOOK which called during every class loading. The profiler modifies loading byte-code by adding profiler's method call "enter" in the beginning of the every method of loading class and at the end of it - "leave". "Enter" profiler's method notes the time of entering to profiling method. "Leave" makes method leaving timestamp and calculates a difference between enter and leave timestamps. This difference means total time for this method call. Direct time calculates as total time of this method minus total time of the all methods called by it. The profiles makes some time corrections for the time spent by profiler.

The profiler implemented as an Eclipse perspective with following views: Threads, Packages, Classes, Methods, Thread methods, Thread call tree, Heap.

**Threads.**
Shows a list with all threads (alive or dead) in the current process. Here you can pause/resume refreshing of statistics in views, pause/resume all threads in profilied program, clear all statistics or run GC in profiled JVM.
![]()
**Fig. 3. Threads View.**

**Call graph.**
Profiler uses Draw2D for displaying call graph of thread. All methods shown in several columns, column depends on level, on which this method was called. Inside one column methods sorted by direct time used. Each node in graph has color from red to dark gray, depending on direct time. Methods with maximum time use have red color, and methods, which almost don't use time, have grey color. Each method has hint with detailed information. You can double click on method to open it in editor.
Lines between method present call from source method to target. Lines from one level to directly next have black color, to next - blue, inside one level - red, and from backward calls - green. Width of line depeneds on how much of time use in this call.
![]()
**Fig. 3.1. Call graph.**
You can also see hit for call. ![]()
**Fig. 3.2. Call hint.**
As you can see, full graph looks fairly complex, but you can select part of it. Press mouse on some method and select then button "Show callers", "Show calles" or "Show caller and callees". You can see something like this: ![]()
**Fig. 3.3. Callers.**
![]()
**Fig. 3.4. Callees.**
![]()
**Fig. 3.5. Callers and callees.**
You can double click on call line for opening editor with source method with highlighting places, where it calls target method. ![]()
**Fig. 3.6. Show calls.**

**Packages.**
Shows a list with all methods with class hierarchy from package. This allows to determine packages which used the most of CPU time. In this view (and in Classes and Methods views also) the user can see in gray color the methods with unmodified parameters since last update. The user can hide such kind of methods by applying appropriate filters.
Here:
Name - name of package/class/method
Inv. - invocation count
% - percent of all invocations
Time - direct time used
% - percent of total time
Time/Inv. - average time used for one invocation
Total time - total time used directly by method and by all methods it calls
Inst. time - time used for instrumentation of class
![]()
**Fig. 4. Packages View.**

You can add package or class to filter by pressing right mose button on element in table and selecting menu item. Filter can be added to launch configuration filter (will be used in next profiling, if you will active it) or to view filters (will be activated right now).
![]()
**Fig. 4.1. Add filter.**

**Classes.**
Shows a list of all class methods with method hierarchy from class. This allows to determine classes with the most CPU time usage.
![]()
**Fig. 5. Classes View.**

**Filters.**
Allows to define which kind of methods can be shown.
![]()
**Fig. 6. Filters.**
You can save configured name patterns (inclusive or exalusive) with some name and description, then you will able to select them in check list box. ![]()
**Fig. 6.1. New filter.**
Later you can change this user defined filter. ![]()
**Fig. 6.2. Edit filter.**

**Methods.**
Shows the statistics for all methods of the current process.
![]()
**Fig. 7. All Methods View.**

**Thread methods.**
Shows methods statictics for thread selected in Threads View.
![]()
**Fig. 8. Thread Methods View.**

**Thread tree.**
Shows a methods invocation tree for thread selected in Threads View. Here red square used for highlighted methods (from thread methods context menu) and green dot used for all other methods.
![]()
**Fig. 9. Thread Tree View.**

**Inverted thread tree.**
Shows a methods invocation tree for thread selected in Threads View starting from leaves. This allows you fastly detect, that some leaf method uses much of total time and see, what methods it is called.
![]()
**Fig. 9.1. Inverted thread Tree View.**

**Heap.**
Shows heap usage graph: total (green), used (blue) and free (yellow) heap.
![Heap View.]()
**Fig. 10. Heap View.**

Here is an overall view of the profiler perspective. You can see a method HashMap.put opened in source code editor with highlighted lines of code which has the maximum hit count (size of annotation depends on hit count). You can open source code editor by choosing menu item "Open method in editor" in context menu.
![]()
**Fig. 11. Perspective.**

[![SourceForge Logo]()](http://sourceforge.net/)
