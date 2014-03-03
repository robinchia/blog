---
layout: post
title: "XStream - Annotations Tutorial"
categories: xml
tags: 
 - xml
 - xstream
--- 

# XStream - Annotations Tutorial

[![XStream]()](http://xstream.codehaus.org/index.html)

# Annotations Tutorial

## Motivation

Sometimes it can get tedious to call all those XStream aliases/register converter methods or you might simply like the new trend on configuring POJOs: Java annotations.

This tutorial will show you how to use some of the annotations provided by XStream in order to make configuration easier. Let's start with a custom Message class:
package com.thoughtworks.xstream; package com.thoughtworks.xstream; public class RendezvousMessage { private int messageType; public RendezvousMessage(int messageType) { this.messageType = messageType; } }

Let's code the XStream calls which generate the XML file:

package com.thoughtworks.xstream; public class Tutorial { public static void main(String[] args) { XStream stream = new XStream(); RendezvousMessage msg = new RendezvousMessage(15); System.out.println(stream.toXML(msg)); } }

Results in the following XML:

<com.thoughtworks.xstream.RendezvousMessage> <messageType>15</messageType> </com.thoughtworks.xstream.RendezvousMessage>

## Aliasing Annotation

The most basic annotation is the one responsible for type and field aliasing: @XStreamAlias. Let's annotate both our type and field and run the tutorial method again:
@XStreamAlias("message") class RendezvousMessage { @XStreamAlias("type") private int messageType; public RendezvousMessage(int messageType) { this.messageType = messageType; } }

In some strange way, the result is the same. What happened here? XStream does not read this annotation by default as it would be impossible to deserialize the XML code. Therefore we need to tell XStream to read the annotations from this type:

public static void main(String[] args) { XStream stream = new XStream(); xstream.processAnnotations(RendezvousMessage.class); RendezvousMessage msg = new RendezvousMessage(15); System.out.println(stream.toXML(msg)); }

Note that we have called the processAnnotations method of XStream. This method registers all aliases annotations in the XStream instance passed as first argument. You may also use the overloaded version of this method taking an array of types. The resulting XML is now what we have expected:

<message> <type>15</type> </message>

If you let XStream process the annotations of a type, it will also process all annotations of the related types i.e. all super types, implemented interfaces, the class types of the members and all their generic types.

## Implicit Collections

Let's add a List of content to our RendezvousMessage. We desire the same functionality obtained with implicit collections:
@XStreamAlias("message") class RendezvousMessage { @XStreamAlias("type") private int messageType; private List<String> content; public RendezvousMessage(int messageType, String ... content) { this.messageType = messageType; this.content = Arrays.asList(content); } }

public static void main(String[] args) { XStream stream = new XStream(); xstream.processAnnotations(RendezvousMessage.class); RendezvousMessage msg = new RendezvousMessage(15, "firstPart","secondPart"); System.out.println(stream.toXML(msg)); }

The resulting XML shows the collection name before its elements:
<message> <type>15</type> <content class="java.util.Arrays$ArrayList"> <a class="string-array"> <string>firstPart</string> <string>secondPart</string> </a> </content> </message>

This is not what we desire therefore we will annotate the content list to be recognized as an implicit collection:

@XStreamAlias("message") class RendezvousMessage { @XStreamAlias("type") private int messageType; @XStreamImplicit private List<String> content; public RendezvousMessage(int messageType, String... content) { this.messageType = messageType; this.content = Arrays.asList(content); } }

Resulting in an XML which ignores the field name (content) of the list:

<message> <type>15</type> <a class="string-array"> <string>firstPart</string> <string>secondPart</string> </a> </message>

We are almost there... we still want to remove the 'a' tag, and define each content part with the tag 'part'. In order to do so, let's add another attribute to our implicit collection annotation. The attribute field defines the name of the tag used for data contained inside this collection:

@XStreamAlias("message") class RendezvousMessage { @XStreamAlias("type") private int messageType; @XStreamImplicit(itemFieldName="part") private List<String> content; public RendezvousMessage(int messageType, String... content) { this.messageType = messageType; this.content = Arrays.asList(content); } }

Resulting in a cleaner XML:

<message> <type>15</type> <part>firstPart</part> <part>secondPart</part> </message>

## Local Converters

Let's create another attribute which defines the timestamp when the message was created:
@XStreamAlias("message") class RendezvousMessage { @XStreamAlias("type") private int messageType; @XStreamImplicit(itemFieldName="part") private List<String> content; private Calendar created = new GregorianCalendar(); public RendezvousMessage(int messageType, String... content) { this.messageType = messageType; this.content = Arrays.asList(content); } }

Resulting in the following xml:

<message> <type>15</type> <part>firstPart</part> <part>secondPart</part> <created> <time>1154097812245</time> <timezone>America/Sao_Paulo</timezone> </created> </message>

Now we face the following problem: We want to use a custom converter locally for this Calendar, but only for this Calendar, this exact field in this exact type. Easy... let's annotate it with the custom converter annotation:

@XStreamAlias("message") class RendezvousMessage { @XStreamAlias("type") private int messageType; @XStreamImplicit(itemFieldName="part") private List<String> content; @XStreamConverter(SingleValueCalendarConverter.class) private Calendar created = new GregorianCalendar(); public RendezvousMessage(int messageType, String... content) { this.messageType = messageType; this.content = Arrays.asList(content); } }

Let's create the custom converter:

public class SingleValueCalendarConverter implements Converter { public void marshal(Object source, HierarchicalStreamWriter writer, MarshallingContext context) { Calendar calendar = (Calendar) source; writer.setValue(String.valueOf(calendar.getTime().getTime())); } public Object unmarshal(HierarchicalStreamReader reader, UnmarshallingContext context) { GregorianCalendar calendar = new GregorianCalendar(); calendar.setTime(new Date(Long.parseLong(reader.getValue()))); return calendar; } public boolean canConvert(Class type) { return type.equals(GregorianCalendar.class); } }

And we end up with the converter being used and generating the following XML:

<message> <type>15</type> <part>firstPart</part> <part>secondPart</part> <created>1154097812245</created> </message>

## Attributes

The client may asks for the type tag to be an attribute inside the message tag, as follows:
<message type="15"> <part>firstPart</part> <part>secondPart</part> <created>1154097812245</created> </message>

All you need to do is add the @XStreamAsAttribute annotation:

@XStreamAlias("message") class RendezvousMessage { @XStreamAlias("type") @XStreamAsAttribute private int messageType; @XStreamImplicit(itemFieldName="part") private List<String> content; @XStreamConverter(SingleValueCalendarConverter.class) private Calendar created = new GregorianCalendar(); public RendezvousMessage(int messageType, String... content) { this.messageType = messageType; this.content = Arrays.asList(content); } }

## Omitting Fields

Sometimes a class may contain elements that should not be part of the resulting XML. In our case we may now drop the 'messageType', since we are only interested at the content. This is easy using the @XStreamOmitField annotation:
@XStreamAlias("message") class RendezvousMessage { @XStreamOmitField private int messageType; @XStreamImplicit(itemFieldName="part") private List<String> content; @XStreamConverter(SingleValueCalendarConverter.class) private Calendar created = new GregorianCalendar(); public RendezvousMessage(int messageType, String... content) { this.messageType = messageType; this.content = Arrays.asList(content); } }

The resulting XML does not contain the type of the message anymore:

<message> <part>firstPart</part> <part>secondPart</part> <created>1154097812245</created> </message>

## Auto-detect Annotations

Until now we have always told you, that you have to call processAnnotation to configure the XStream instance with the present annotations in the different classes. However, this is only half the truth. You can run XStream also in a lazy mode, where it auto-detects the annotations while processing the object graph and configure the XStream instance on-the-fly:
package com.thoughtworks.xstream; public class Tutorial { public static void main(String[] args) { XStream stream = new XStream(); xstream.autodetectAnnotations(true); RendezvousMessage msg = new RendezvousMessage(15); System.out.println(stream.toXML(msg)); } }

The resulting XML will look as expected! Nevertheless you have to understand the implications, therefore some words of warning:

1. **Chicken-and-egg problem**

An XStream instance caches all class types processed for annotations. Every time XStream converts an object it will in auto-detection mode first process the object's type and all the types related (as explained [above](http://xstream.codehaus.org/annotations-tutorial.html#Aliasing)). Therefore it is no problem to serialize an object graph into XML, since XStream will know of all types in advance. This is no longer true at deserialization time. XStream has to know the alias to turn it into the proper type, but it can find the annotation for the alias only if it has processed the type in advance. Therefore deserialization will fail if the type has not already been processed either by having called XStream's processAnnotations method or by already having serialized this type. However, @XStreamAlias is the only annotation that may fail in this case.
1. **Concurrency**

XStream is not thread-safe while it is configured, thread-safety is only guaranteed during marshalling and unmarshalling. However an annotation is defining a change in configuration that is now applied while object marshalling is processed. Therefore will the auto-detection mode turn XStream's marshalling process in a thread-unsafe operation any you may run under certain circumstances into concurrency problems.
1. **Exceptions**

XStream uses a well-defined exception hierarchy. Normally an InitializationException is only thrown while XStream is configured. If annotations are processed on the fly they can be thrown obviously also in a marshalling process.
1. **Performance**

In auto-detection mode XStream will have to examine any unknown class type for annotations. This will slow down the marshalling process until all processed types have been examined once.

Please note, that any call to XStream.processAnnotations will turn off the auto-detection mode.

## Summing up

The XStream annotations support might help you configuring your class mappings in some ways, as the custom configuration will appear in your types, but might not be the solution for other problems, i.e. when you need to map the same type to two different XML 'standards'. Others might claim that the configuration should be clearly stated in a Java class and not mixed with your model, its up to you to pick the best approach in your case: Annotations or direct method calls to the XStream instance. Annotations do not provide more functionality, but may improve convenience.
# Software

* [About XStream](http://xstream.codehaus.org/index.html)
* [News](http://xstream.codehaus.org/news.html)
* [Change History](http://xstream.codehaus.org/changes.html)
* [About Versioning](http://xstream.codehaus.org/versioning.html)

# Evaluating XStream

* [Two Minute Tutorial](http://xstream.codehaus.org/tutorial.html)
* [Object references](http://xstream.codehaus.org/graphs.html)
* [Tweaking the Output](http://xstream.codehaus.org/manual-tweaking-output.html)
* [License](http://xstream.codehaus.org/license.html)
* [Download](http://xstream.codehaus.org/download.html)
* [References](http://xstream.codehaus.org/references.html)
* [Statistics](http://www.ohloh.net/projects/3459)
# Using XStream

* [Architecture Overview](http://xstream.codehaus.org/architecture.html)
* [Converters](http://xstream.codehaus.org/converters.html)
* [Frequently Asked Questions](http://xstream.codehaus.org/faq.html)
* [Users' Mailing List](http://xstream.codehaus.org/list-user.html)
* [JavaDoc Core](http://xstream.codehaus.org/javadoc/index.html)
* [JavaDoc Benchmark](http://xstream.codehaus.org/benchmark-javadoc/index.html)
* [Reporting Issues](http://xstream.codehaus.org/issues.html)

# Tutorials

* [Two Minute Tutorial](http://xstream.codehaus.org/tutorial.html)
* [Alias Tutorial](http://xstream.codehaus.org/alias-tutorial.html)
* Annotations Tutorial
* [Converter Tutorial](http://xstream.codehaus.org/converter-tutorial.html)
* [Object Streams Tutorial](http://xstream.codehaus.org/objectstream.html)
* [Persistence API Tutorial](http://xstream.codehaus.org/persistence-tutorial.html)
* [JSON Tutorial](http://xstream.codehaus.org/json-tutorial.html)
# Developing XStream

* [How to Contribute](http://xstream.codehaus.org/how-to-contribute.html)
* [Developers' Mailing List](http://xstream.codehaus.org/list-dev.html)
* [Development Team](http://xstream.codehaus.org/team.html)
* [Source Repository](http://xstream.codehaus.org/repository.html)
* [Continuous Integration](http://bamboo.ci.codehaus.org/browse/XSTREAM)
