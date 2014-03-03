---
layout: post
title: "centos libxml2"
categories: linux
tags: 
 - linux
 - redhat
--- 

# centos libxml2

##            Introduction to libx‍ml2

The libxml2 package contains          libraries and utilities used for parsing XML files.

This package is known to build and work properly using an LFS-7.4          platform.

### Package Information

* 
Download (HTTP): [http://xmlsoft.org/sources/libxml2-2.9.1.tar.gz](http://xmlsoft.org/sources/libxml2-2.9.1.tar.gz)
* 
Download (FTP): [ftp://xmlsoft.org/libxml2/libxml2-2.9.1.tar.gz](ftp://xmlsoft.org/libxml2/libxml2-2.9.1.tar.gz)
* 
Download MD5 sum: 9c0cfef285d5c4a5c80d00904ddab380
* 
Download size: 5.0 MB
* 
Estimated disk space required: 100 MB
* 
Estimated build time: 0.6 SBU

### Additional Downloads

* 
Optional Testsuite: [http://www.w3.org/XML/Test/xmlts20130923.tar.gz](http://www.w3.org/XML/Test/xmlts20130923.tar.gz)                - This enables **make                check** to do complete testing.

### libxml2 Dependencies

### Recommended

[Python-2.7.6](http://www.linuxfromscratch.org/blfs/view/svn/general/python2.html "Python-2.7.6") (to build and install a          Python library module,          additionally it is required to run the full suite of tests)

![[Note]](http://www.linuxfromscratch.org/blfs/view/svn/images/note.png)          

### Note

Some packages which utilize libxml2 (such as GNOME Doc Utils) need the Python module installed to function properly            and some packages (such as MesaLib) will not build properly if            the Python module is not            available.

User Notes: [http://wiki.linuxfromscratch.org/blfs/wiki/libxml2](http://wiki.linuxfromscratch.org/blfs/wiki/libxml2)        

## Installation of libxml2

If you downloaded the testsuite, issue the following command:
tar xf ../xmlts20130923.tar.gz

       

Install libxml2 by running the          following commands:
./configure --prefix=/usr --disable-static --with-history && make

       

To test the results, issue: **make          check**.

Now, as the

root
user:
make install

     

## Command Explanations

*
--disable-static
*: This          switch prevents installation of static versions of the libraries.

--with-history
: This switch enables          Readline support when running          **xmlcatalog** or          **xmllint** in shell          mode.

## Contents

**Installed Programs:**              xml2-config, xmlcatalog and              xmllint            

**Installed Libraries:**              libxml2.so and optionally, the              libxml2mod.so Python              module            

**Installed Directories:**              /usr/include/libxml2,              /usr/share/doc/libxml2-2.9.1,              /usr/share/doc/libxml2-python-2.9.1 and              /usr/share/gtk-doc/html/libxml2            

### Short Descriptions

[]()**xml2-config**                  

determines the compile and linker flags that should be                    used to compile and link programs that use

libxml2
.[]()**xmlcatalog**                  

is used to monitor and manipulate XML and SGML catalogs.[]()**xmllint**                  

parses XML files and outputs reports (based upon options)                    to detect errors in XML coding.[]()

libxml2.so
                 

provides functions for programs to parse files that use                    the XML format.                   
