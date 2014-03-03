---
layout: post
title: "XStream - Frequently Asked Questions"
categories: xml
tags: 
 - xml
 - xstream
--- 

# XStream - Frequently Asked Questions

[![XStream]()](http://xstream.codehaus.org/index.html)

# Frequently Asked Questions

1. [Compatibility](http://xstream.codehaus.org/faq.html#Compatibility)
1. [Serialization](http://xstream.codehaus.org/faq.html#Serialization)
1. [XML specifics](http://xstream.codehaus.org/faq.html#XML)
1. [JSON specifics](http://xstream.codehaus.org/faq.html#JSON)
1. [Comparison to other products](http://xstream.codehaus.org/faq.html#Other_Products)
1. [Scalability](http://xstream.codehaus.org/faq.html#Scalability)
1. [Uses of XStream](http://xstream.codehaus.org/faq.html#Uses)

# Compatibility

## Which JDK is required to use XStream?

1.3 or later.

## Does XStream behave differently across different JVMs?

XStream has two modes of operation: **Pure Java** and **Enhanced**. In pure Java mode, XStream behaves in the same way across different JVMs, however its features are limited to what reflection allows, meaning it cannot serialize certain classes or fields. In **enhanced** mode, XStream does not have these limitations, however this mode of operation is not available to all JVMs.

## Which JVMs allow XStream to operate in enhanced mode?

Currently on the Sun, Apple, HP, IBM and Blackdown 1.4 JVMs and onwards. Also for Hitachi, SAP and Diablo from 1.5 and onwards. Support for BEA JRockit starting with R25.1.0. For all other JVMs, XStream should be used in pure Java mode.

## What are the advantages of using enhanced mode over pure Java mode?

Feature Pure Java Enhanced Mode Public classes Yes Yes Non public classes No Yes Static inner classes Yes Yes Non-static inner classes No Yes Anonymous inner classes No Yes With default constructor Yes Yes Without default constructor No Yes Private fields Yes Yes Final fields Yes >= JDK 1.5 Yes

## Can I use XStream in an Android application?

XStream does work in Android 1.0, but has limited capabilities. XStream does not support enhanced mode for Android. Additionally Android reports itself as implementation of JDK specification 0.9, so it is not possible to write into final fields. Android also does not include the java.beans package. Therefore you cannot use the JavaBeanConverter.

## Why does XStream fail on Harmony?

Since JDK 5 it is possible according the Java specification to write into final fields using reflection. This is not yet supported by Harmony and therefore the PureJavaReflectionProvider fails. We have also already investigated into enhanced mode in Harmony, but the Harmony JVM currently crashes running the unit tests. It is simply not yet ready.

## Are there plans to provide enhanced mode support to other JVMs?

Yes. [Let us know](http://xstream.codehaus.org/list-user.html) which JVM you would like supported.

## When should I use XStream not in enhanced mode?

Running XStream in a secured environment can prevent XStream from running in enhanced mode. This is especially true when running XStream in an applet. You may also try to use the JavaBeanConverter as alternative to the ReflectionConverter running in enhanced or pure Java mode.

## Which permissions does XStream need when running with an active SecurityManager?

This depends on the mode XStream is running in. Refer to the [SecurityManagerTest](http://svn.xstream.codehaus.org/browse/xstream/trunk/xstream/src/test/com/thoughtworks/acceptance/SecurityManagerTest.java) for details.

## Why does XStream 1.2 no longer read XML generated with XStream 1.1.x?

The architecture in XStream has slightly changed. Starting with XStream 1.2 the [HierarchicalStreamDriver](http://xstream.codehaus.org/javadoc/com/thoughtworks/xstream/io/HierarchicalStreamDriver.html) implementation is responsible to ensure that XML tags and attributes are valid names in XML, in XStream 1.1.x this responsibility was part of the ClassMapper implementations. Under some rare circumstances this will result in an unreadable XML due to the different processing order in the workflow of such problematic tag names.

You can run XStream in 1.1 compatibility mode though:
XStream xstream = new XStream(new XppDriver(new XStream11XmlFriendlyReplacer())) { protected boolean useXStream11XmlFriendlyMapper() { return true; } };

## XStream 1.3 ignores suddenly annotated converters (@XStreamConverter and @XStreamConverters)?

XStream treats now all annotations the same and therefore it no longer auto-detects any annotation by default. You can configure XStream to run in auto-detection mode, but be aware if the [implications](http://xstream.codehaus.org/annotations-tutorial.html#AutoDetect). As alternative you might register the deprecated AnnotationReflectionConverter, that was used for XStream pre 1.3.x, but as drawback the functionality to register a local converter with XStream.registerLocalConverter will no longer work.

## XStream 1.3 suddenly has a different field order?

Yes. This was announced with the last 1.2.x release and was done to support the type inheritance of XML schemas. However, XStream is delivered with the [XStream12FieldKeySorter](http://xstream.codehaus.org/javadoc/com/thoughtworks/xstream/io/HierarchicalStreamDriver.html) that can be used to [sort the fields](http://xstream.codehaus.org/faq.html#Serialization_sort_fields) according XStream 1.2.2.

# Serialization

## Which class types can be serialized by XStream?

In contrast to the JDK XStream is not tied to a marker interface to serialize a class. XStream ships with some specialized [converters](http://xstream.codehaus.org/converters.html), but will use reflection by default for "unknown" classes to examine, read and write the class' data. Therefore XStream can handle quite any class, especially the ones referred as POJO (Plain Old Java Object).

However, some types of classes exist with typical characteristics, that cannot be handled - at least not out of the box:

1. Objects that are based on threads or thread local data: Thread, Timer, ThreadLocal and so on. These classes keep different data for different threads and there's no possibility to recreate a thread in a generic way nor recreating thread specific data. There might be special use cases, but this will always involve a custom converter where threads can be recreated in a specific way tied to that use case.
1. Class types that are based on generated classes. Such types have often names that are unique to the current process and will have no meaning in a different process. A custom converter might help to write the appropriate data into the serializing stream to be able to recreate a equivalent class at deserialization time.
1. Types that keep and use system resources like file handles, sockets, pipes and so on. ClassLoader, FileInputStream, FileOutputStream, Socket and so on. To deserialize such a class the converter must be able to claim the appropriate resource from the system again. With the help of a custom converter this might be possible, but with the reflection converter the deserialized class might refer a system resource that is no longer valid or belongs to somebody else. Behavior is undefined then.
1. A very special case of such allocated system resources are those classes that keep handles to system memory directly, because they are partly implemented native. It is known to be true for the Linux version of Sun's JDK that the BufferedImage references some system specific types of the JDK that themselves have member fields with such memory handles. While it is possible at first sight to serialize and deserialize a BufferedImage, the reflection converter will also duplicate the memory handle. As a result the JVM might crash easily because of freeing unallocated memory or freeing the same memory twice. It might be possible to create a custom converter, but the data structure is really complex in this area and nobody has been investigating so far to such an extent. However, **do not use the reflection converter for these types! You have been warned!**

## How do I specify that a field should not be serialized?

Make it

transient
, specify it with

XStream.omitField()
or annotate it with @XStreamOmitField

## How do I initialize a transient field at deserialization?

XStream uses the same mechanism as the JDK serialization. Example:
class ThreadAwareComponent { private transient ThreadLocal component; // ... private Object readResolve() { component = new ThreadLocal(); return this; } }

## XStream is not calling the default constructor during deserialization.

This is, in fact, the same case as above. XStream uses the same mechanism as the JDK serialization. When using the enhanced mode with the optimized reflection API, it does not invoke the default constructor. The solution is to implement the readResolve method as above:
class MyExecutor { private Object readResolve() { // do what you need to do here System.out.println("After instantiating MyExecutor"); // at the end returns itself return this; } }

## What do serialized collections look like?

See example for the [CollectionConverter](http://xstream.codehaus.org/converters.html#java.util).

Note, that it is possible to configure XStream to omit the container element toys using implicit collections.

## Do my classes have to implement Serializable if XStream is to serialize them?

No.

## Can dynamic proxies be serialized?

Yes.

## Can CGLIB proxies be serialized?

Only limitedly. A proxy generated with the CGLIB Enhancer is supported, if the proxy uses either a factory or only one callback. Then it is possible to recreate the proxy instance at unmarshalling time. Starting with XStream 1.3.1 CGLIB support is no longer automatically installed because of possible classloader problems and side-effects, because of incompatible ASM versions. You can enable CGLIB support with:
XStream xstream = new XStream() { protected MapperWrapper wrapMapper(MapperWrapper next) { return new CGLIBMapper(next); } }; xstream.registerConverter(new CGLIBEnhancedConverter(xstream.getMapper(), xstream.getReflectionProvider()));

## CGLIBEnhancedConverter fails at initialization with ExceptionInInitializerError

This is not a problem of XStream. You have incompatible ASM versions in your classpath. CGLIB 2.1.x and below is based on ASM 1.5.x which is incompatible to newer versions that are used by common packages like Hibernate, Groovy or Guice. Check your dependencies and ensure that you are using either using cglib-nodep-2.x.jar instead of cglib-2.x.jar or update to cglib-2.2.x that depends on ASM 3.1. However, the *nodep* version contains a copy of the ASM classes with private packages and will therefore not raise class incompatibilities at all.

## Serialization fails with NoSuchMethodError: net.sf.cglib.proxy.Enhancer.isEnhanced(Ljava/lang/Class;)Z

XStream uses this method to detect a CGLIB-enhanced proxy. Unfortunately the method is not available in the cglib-2.0 version. Since this version is many years old and the method is available starting with cglib-2.0.1, please consider an upgrade of the dependency, it works usually smoothly.

## My attributes are interpreted by XStream itself and cause unexpected behavior

XStream's generic converters and the marshalling strategies use a number of attributes on their own. Especially the attributes named *id*, *class* and *reference* are likely to cause such collisions. Main reason is XStream's history, because originally user defined attributes were not supported and all attribute were system generated. Starting with XStream 1.3.1 you can redefine those attributes to allow the names to be used for your own ones. The following snippet defines XStream to use different system attributes for *id* and *class* while the field *id* of YourClass is written into the attribute *class*:
XStream xstream = new XStream() { xstream.useAttributeFor(YourClass.class, "id"); xstream.aliasAttribute("class", "id"); xstream.aliasSystemAttribute("type", "class"); xstream.aliasSystemAttribute("refid", "id");

## Can I select the field order in which XStream serializes objects?

Yes. XStream's ReflectionConverter uses the defined field order by default. You can override it by using an specific FieldKeySorter:
SortableFieldKeySorter sorter = new SortableFieldKeySorter(); sorter.registerFieldOrder(MyType.class, new String[] { "firstToSerialize", "secondToSerialize", "thirdToSerialize" }); xstream = new XStream(new Sun14ReflectionProvider(new FieldDictionary(sorter)));

## How does XStream deal with newer versions of classes?

* If a new field is added to the class, deserializing an old version will leave the field uninitialized.
* If a field is removed from the class, deserializing an old version that contains the field will cause an exception. Leaving the field in place but declaring it as transient will avoid the exception, but XStream will not try to deserialize it.
* If a class is renamed, aliases can be used to create an abstraction between the name used in XML and the real class name.
* If a field is renamed, this should be treated as adding a field and removing a field.

For more advanced class migrations, you may

* have to do custom pre-processing of the XML before sending it to XStream (for example, with XSLT or DOM manipulations)
* declare new fields as transient
* implement your own converter, that can handle the situation
* add a readResolve() method to your class, that initializes the object accordingly
* implement a custom mapper to ignore unknown fields automatically (see acceptance test CustomMapperTest.testCanBeUsedToOmitUnexpectedElements())

Future versions of XStream will include features to make these type of migrations easier.

## How does XStream cope with isolated class loaders?

Serializing an object graph is never a problem, even if the classes of those objects have been loaded by a different class loader. The situation changes completely at deserialization time. In this case you must set the class loader to use with:
xstream.setClassLoader(yourClassLoader);

Although XStream caches a lot of type related information to gain speed, it keeps those information in tables with weak references that should be cleaned by the garbage collector when the class loader is freed.

Note, that this call should be made quite immediately after creating the XStream and before any other configuration is done. Otherwise configuration based on special types might refer classes loaded with the wrong classloader.

# XML specifics

## Why does XStream not respect the encoding in the XML declaration?

XStream architecture is based on IO Readers and Writers, while the XML declaration is the responsibility of XML parsers. XStream all [HierarchicalStreamDriver](http://xstream.codehaus.org/javadoc/com/thoughtworks/xstream/io/HierarchicalStreamDriver.html) implementation respect the encoding since version 1.3, but only if you provide an [InputStream](http://java.sun.com/j2se/1.4.2/docs/api/java/io/InputStream.html). If XStream consumes a [Reader](http://java.sun.com/j2se/1.4.2/docs/api/java/io/Reader.html) you have to initialize the reader with the appropriate encoding yourself, since it is now the reader's task to perform the encoding and no XML parser can change the encoding of a Reader and any encoding definition in the XML header will be ignored.

## Why does XStream not write an XML declaration?

XStream is designed to write XML snippets, so you can embed its output into an existing stream or string. You can write the XML declaration yourself into the Writer before using it to call XStream.toXML(writer).

## Why does XStream not write XML in UTF-8?

XStream does no character encoding by itself, it relies on the configuration of the underlying XML writer. By default it uses its own PrettyPrintWriter which writes into the default encoding of the current locale. To write UTF-8 you have to provide a [Writer](http://java.sun.com/j2se/1.4.2/docs/api/java/io/Writer.html) with the appropriate encoding yourself.

## Why do field names suddenly have double underscores in the generated XML?

XStream maps Java class names and field names to XML tags or attributes. Unfortunately this mapping cannot be 1:1, since some identifiers of Java contain a dollar sign which is invalid in XML names. Therefore XStream uses an [XmlFriendlyReplacer](http://xstream.codehaus.org/javadoc/com/thoughtworks/xstream/io/xml/XmlFriendlyReplacer.html) to replace this character with a replacement. By default this replacement uses an underscore and therefore the replacer must escape the underscore itself also. You may provide a different configured instance of the XmlFriendlyReplacer or a complete own implementation, but you must ensure, that the resulting names are valid for XML.

## XStream fails to unmarshal my given XML and I do not know why?

By default XStream is written for persistence i.e. it will read the XML it can write. If you have to transform a given XML into an object graph, you should go the other way round. Use XStream to transfer your objects into XML. If the written XML matches your schema, XStream is also able to read it. This way is much easier, since you can spot the differences in the XML much more easy than to interpret the exceptions XStream will throw if it cannot match the XML into your objects.

## My parser claims the &#x0; character to be invalid, but it was written with XStream!

Your parser is basically right! A character of value 0 is not valid as part of XML according the XML specification (see version [1.0](http://www.w3.org/TR/2006/REC-xml-20060816/#charsets) or [1.1](http://www.w3.org/TR/2006/REC-xml11-20060816/#charsets)), neither directly nor as character entity nor within CDATA. But not every parser respects this part of the specification (e.g. Xpp3 will ignore it and read character entities). If you expect such characters in your strings and you do not use the Xpp3 parser, you should consider to use a converter that writes the string as byte array in Base64 code. As alternative you may force the [PrettyPrintWriter](http://xstream.codehaus.org/javadoc/com/thoughtworks/xstream/io/xml/PrettyPrintWriter.html) or derived writers to be XML 1.0 or 1.1. compliant, i.e. in this mode a StreamException is thrown.

## My parser claims a control character to be invalid, but it was written with XStream!

Your parser is probably right! Control characters are only valid as part of XML 1.1. You should add an XML header declaring this version or use a parser that does not care about this part of the specification (e.g. Xpp3 parser).

## Why is my element not written as XML attribute although I have configured it?

You can only write types as attributes that are represented as a single String value and are handled therefore by SingleValueConverter implementations. If your type is handled by a Converter implementation, the configuration of XStream to write an attribute (using XStream.useAttributeFor() or @XStreamAsAttribute) is simply ignored.

## Why are whitespace characters missing in my attribute values after deserialization?

This is part of the XML specification and a required functionality for any XML parser called [attribute value normalization](http://www.w3.org/TR/2006/REC-xml-20060816/#AVNormalize). It cannot be influenced by XStream. Do not use attributes if your values contain leading or trailing whitespaces, other whitespaces than blanks, or sequences of whitespaces.

## Why does XStream not have any namespace support?

Not every XML parser supports namespaces and not every XML parser that supports namespaces can be configured within XStream to use those. Basically namespaces must be supported individually for the different XML parsers and the only support for namespaces that has currently been implemented in XStream is for the StAX paser. Therefore use and configure the StaxDriver of XStream to use namespaces.

# JSON specifics

## Why are there two JSON driver implementations?

As always, first for historical reasons! Main difference is that the [JettisonMappedXmlDriver](http://xstream.codehaus.org/javadoc/com/thoughtworks/xstream/io/json/JettisonMappedXmlDriver.html) is a thin wrapper around [Jettison](http://jettison.codehaus.org/) in combination with the [StaxDriver](http://xstream.codehaus.org/javadoc/com/thoughtworks/xstream/io/xml/StaxDriver.html), while the [JsonHierarchicalStreamDriver](http://xstream.codehaus.org/javadoc/com/thoughtworks/xstream/io/json/JsonHierarchicalStreamDriver.html) uses an own more flexible implementation, but can only be used to generate JSON, deserialization is not implemented.

## Why is it not possible to deserialize a JSON string starting with an array?

XStream's implementation to deserialize JSON is based on Jettison and StAX. Jettison implements a XMLStreamReader of StaX and transforms the processed JSON virtually into XML first. However, if the JSON string starts with an array it is not possible for Jettison to create a valid root element, since it has no name.

## XStream fails to unmarshal my JSON string and I do not know why?

Deserialization of JSON is currently done by Jettison, that transforms the JSON string into a StAX stream. XStream itself does nothing know about the JSON format here. If your JSON string reaches some kind of complexity and you do not know how to design your Java objects and configure XStream to match those, you should have a look at the intermediate XML that is processed by XStream in the end. This might help to identify the problematic spots. Also consider then [marshalling your Java objects into XML first](http://xstream.codehaus.org/faq.html#XML_unmarshalling_fails). You can use following code to generate the XML:
String json = "{\"string\": \"foo\"}"; HierarchicalStreamDriver driver = new JettisonMappedXmlDriver(); StringReader reader = new StringReader(json); HierarchicalStreamReader hsr = driver.createReader(reader); StringWriter writer = new StringWriter(); new HierarchicalStreamCopier().copy(hsr, new PrettyPrintWriter(writer)); writer.close(); System.out.println(writer.toString());

## What limitations has XStream's JSON support?

JSON represents a very simple data model for easy data transfer. Especially it has no equivalent for XML attributes. Those are written with a leading "@" character, but this is not always possible without violating the syntax (e.g. for array types). Those may silently dropped (and makes it therefore difficult to implement deserialization). References are another issue in the serialized object graph, since JSON has no possibility to express such a construct. You should therefore always set the NO_REFERENCES mode of XStream.

## Why are there invalid characters in my JSON representation?

The JSON spec requires any JSON string to be in UTF-8 encoding. However, XStream ensures this only if you provide an InputStream or an OutputStream. If you provide a Reader or Writer you have to ensure this requirement on your own.

## The generated JSON is invalid, it contains a dash in the label!

Well, no, the JSON is valid! Please check yourself with the [JSON syntax checker](http://www.jslint.com/). However, some JavaScript libraries silently assume that the JSON labels are valid JavaScript identifiers, because JavaScript supports a convenient way to address an element, **if** the label is a valid JavaScript identifier:
var json = {"label": "foo", "label-with-dash": "bar"}; var fooVar = json.label; // works for labels that are JavaScript identifiers var barVar = json["label-with-dash"]; // using an array index works always

As alternative you may wrap the JsonWriter and replace any dash with an underscore:

HierarchicalStreamDriver driver = new JsonHierarchicalStreamDriver() { public HierarchicalStreamWriter createWriter(Writer out) { return new WriterWrapper(super.createWriter(out)) { public void startNode(String name) { startNode(name, null); } public void startNode(String name, Class clazz) { wrapped.startNode(name.replace('-', '_'), clazz); } } } }; XStream xstream = new XStream(driver);

# Comparison to other products

## How does XStream compare to java.beans.XMLEncoder?

XStream is designed for serializing objects using internal fields, whereas [XMLEncoder](http://java.sun.com/j2se/1.4.2/docs/api/java/beans/XMLEncoder.html) is designed for serializing JavaBeans using public API methods (typically in the form of

getXXX()
,

setXXX()
,

addXXX()
and

removeXXX()
methods.

## How does XStream compare to JAXB (Java API for XML Binding)?

JAXB is a Java binding tool. It generates Java code from a schema and you are able to transform from those classes into XML matching the processed schema and back. Note, that you cannot use your own objects, you have to use what is generated.

# Scalability

## Is XStream thread safe?

Yes. Once the XStream instance has been created and configured, it may be shared across multiple threads allowing objects to be serialized/deserialized concurrently (unless you enable the auto.detection and processing of annotations). Actually the creation and initialization of XStream is quite expensive, therefore it is recommended to keep the XStream instance itself.

## How much memory does XStream consume?

This cannot be answered in general, but following topics have impact on the memory:

1. XML parser technology in use: You should use a streaming parser like Xpp3 or StAX. DOM-based parsers process the complete XML and create their document model in memory before the first converter of XStream is called.
1. Your object model: Is it necessary to keep the complete object graph in memory at once. As alternative you might use [object streams](http://xstream.codehaus.org/objectstream.html) or write custom converters that can load and save objects of your object model on the fly without adding them to the object graph physically. As example see the implementation of the [XmlArrayList](http://xstream.codehaus.org/javadoc/com/thoughtworks/xstream/persistence/XmlArrayList.html) in combination with the [FileStreamStrategy](http://xstream.codehaus.org/javadoc/com/thoughtworks/xstream/persistence/FileStreamStrategy.html) to keep parts of the object graph separate.
1. References: By default XStream supports references to the same object in an object graph. This implies that XStream keeps track of all serialized and deserialized objects internally. These references are kept with WeakReferences, so that the memory can be freed as soon as nobody references these objects anymore.
1. XML values: Any tag and attribute value that is converted into a Java String in the object graph will use the same String instance.
1. XStream caches: To increase performance XStream caches quite a lot like classes, converters to use, aliasing, tag names. All those caches make usage of WeakReferences or will exist only while marshalling one object graph resp. unmarshalling one input stream.

## Can the performance of XStream be increased?

XStream is a generalizing library, it inspects and handles your types on the fly. Therefore it will normally be slower than a piece of optimized Java code generated out of a schema. However, it is possible to increase the performance anyway:

* Write custom converters for those of your types that occur very often in your XML.
* Keep a configured XStream instance for multiple usage. Creation and initialization is quite expensive compared to the overhead of XStream when calling marshall or unmarshal.
* Use Xpp3 or StAX parsers.

Note, you should never try to optimize code for performance simply because you **believe** that you have detected a bottle neck. Always use proper tools like a profiler to verify where your hotspots are and whether your optimization was really successful or not.

# Uses of XStream

## Is XStream a data binding tool?

No. It is a serialization tool.

## Can XStream generate classes from XSD?

No. For this kind of work a data binding tool such as [XMLBeans](http://xmlbeans.apache.org/) is appropriate.

## Why is there no SaxReader?

XStream works on a stream-based parser model, while SAX is event-based. The stream based model implies, that the caller consumes the individual tokens from the XML parser on demand, while in an event-based model the parser controls the application flow on its own and will use callbacks to support client processing. The different architecture makes it therefore impossible for XStream to use an event-driven XML parser.
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
* Frequently Asked Questions
* [Users' Mailing List](http://xstream.codehaus.org/list-user.html)
* [JavaDoc Core](http://xstream.codehaus.org/javadoc/index.html)
* [JavaDoc Benchmark](http://xstream.codehaus.org/benchmark-javadoc/index.html)
* [Reporting Issues](http://xstream.codehaus.org/issues.html)

# Tutorials

* [Two Minute Tutorial](http://xstream.codehaus.org/tutorial.html)
* [Alias Tutorial](http://xstream.codehaus.org/alias-tutorial.html)
* [Annotations Tutorial](http://xstream.codehaus.org/annotations-tutorial.html)
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
