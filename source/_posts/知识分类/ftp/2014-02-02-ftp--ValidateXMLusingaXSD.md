---
layout: post
title: "Validate XML using a XSD"
categories: ftp
tags: 
 - ftp
--- 

# Validate XML using a XSD

1、xsd文件源;

SchemaFactory: newSchema(Source schema)

/*    Parser object is: com.sun.org.apache.xerces.internal.jaxp.SAXParserImpl@c9ba38    Start document:     Start element: local name: PHONEBOOK qname: PHONEBOOK uri:     Characters:       Start element: local name: PERSON qname: PERSON uri:     Characters:        Start element: local name: NAME qname: NAME uri:     Attributes:      Name : firstName      Type : CDATA      Value: Joe      Name : lastName      Type : CDATA      Value: Yin    Characters: Joe    Characters:  Yin    End element: local name: NAME qname: NAME uri:     Characters:        Start element: local name: EMAIL qname: EMAIL uri:     Characters: joe@yourserver.com    End element: local name: EMAIL qname: EMAIL uri:     Characters:        Start element: local name: TELEPHONE qname: TELEPHONE uri:     Characters: 202-999-9999    End element: local name: TELEPHONE qname: TELEPHONE uri:     Characters:        Start element: local name: WEB qname: WEB uri:     Characters: www.java2s.com    End element: local name: WEB qname: WEB uri:     Characters:       End element: local name: PERSON qname: PERSON uri:     Characters:       End element: local name: PHONEBOOK qname: PHONEBOOK uri:     End document:      */    **import **java.io.StringReader;    **import **javax.xml.XMLConstants;    **import **javax.xml.parsers.ParserConfigurationException;    **import **javax.xml.parsers.SAXParser;    **import **javax.xml.parsers.SAXParserFactory;    **import **javax.xml.transform.sax.SAXSource;    **import **javax.xml.validation.SchemaFactory;    **import **org.xml.sax.Attributes;    **import **org.xml.sax.InputSource;    **import **org.xml.sax.SAXException;    **import **org.xml.sax.SAXParseException;    **import **org.xml.sax.helpers.DefaultHandler;    **public class **MainClass {      **public static ****void **main(String args[])**throws **Exception {        SAXParserFactory spf = SAXParserFactory.newInstance();        SAXParser parser = **null**;        spf.setNamespaceAware(**true**);        **try **{         SchemaFactory sf =                         SchemaFactory.newInstance(XMLConstants.W3C_XML_SCHEMA_NS_URI);          spf.setSchema(sf.newSchema(**new **SAXSource(**new **InputSource(**new **StringReader(schemaString)))));         parser = spf.newSAXParser();        }        **catch**(SAXException e) {          e.printStackTrace(System.err);          System.exit(1);            }         **catch**(ParserConfigurationException e) {          e.printStackTrace(System.err);          System.exit(1);            }        MySAXHandler handler = **new **MySAXHandler();         System.out.println(schemaString);        parser.parse(**new **InputSource(**new **StringReader(xmlString)), handler);      }      **static **String xmlString = "<?xml version=\"1.0\"?>" +          "<note>" +          "<to>rtoName</to>" +          "<from>FromName</from>" +          "<heading>Info</heading>" +          "<body>Message Body</body>" +          "</note>";            **static **String schemaString ="<?xml version=\"1.0\"?>" +          "<xs:schema xmlns:xs=\"http://www.w3.org/2001/XMLSchema\"" +          " targetNamespace=\"http://www.java2s.com\"" +          " xmlns=\"http://www.java2s.com\"" +          " elementFormDefault=\"qualified\">" +          "<xs:element name=\"note\">" +          "<xs:complexType>" +          "<xs:sequence>" +          "<xs:element name=\"to\" type=\"xs:string\"/>" +          "<xs:element name=\"from\" type=\"xs:string\"/>" +          "<xs:element name=\"heading\" type=\"xs:string\"/>" +          "<xs:element name=\"body\" type=\"xs:string\"/>" +          "</xs:sequence>" +                          "</xs:complexType>" +                          "</xs:element>" +                          "</xs:schema>";    }    **class **MySAXHandler **extends **DefaultHandler {      **public ****void **startDocument() {        System.out.println("Start document: ");      }            **public ****void **endDocument()  {        System.out.println("End document: ");      }            **public ****void **startElement(String uri, String localName, String qname,                                                                    Attributes attr)      {        System.out.println("Start element: local name: " + localName + " qname: "                                                             + qname + " uri: "+uri);        **int **attrCount = attr.getLength();        **if**(attrCount>0) {          System.out.println("Attributes:");           **for**(**int **i = 0 ; i<attrCount ; i++) {            System.out.println("  Name : " + attr.getQName(i));             System.out.println("  Type : " + attr.getType(i));             System.out.println("  Value: " + attr.getValue(i));           }        }       }            **public ****void **endElement(String uri, String localName, String qname) {        System.out.println("End element: local name: " + localName + " qname: "                                                             + qname + " uri: "+uri);      }            **public ****void **characters(**char**[] ch, **int **start, **int **length) {        System.out.println("Characters: " + **new **String(ch, start, length));      }      **public ****void **ignorableWhitespace(**char**[] ch, **int **start, **int **length) {        System.out.println("Ignorable whitespace: " + **new **String(ch, start, length));      }      **public ****void **startPrefixMapping(String prefix, String uri) {        System.out.println("Start \"" + prefix + "\" namespace scope. URI: " + uri);       }      **public ****void **endPrefixMapping(String prefix) {        System.out.println("End \"" + prefix + "\" namespace scope.");       }      **public ****void **warning(SAXParseException spe) {        System.out.println("Warning at line "+spe.getLineNumber());        System.out.println(spe.getMessage());      }      **public ****void **fatalError(SAXParseException spe) **throws **SAXException {        System.out.println("Fatal error at line "+spe.getLineNumber());        System.out.println(spe.getMessage());        **throw **spe;      }    }

http://www.java2s.com/Code/JavaAPI/javax.xml.validation/SchemaFactorynewSchemaSourceschema.htm

另外，可参考：

import java.io.IOException; // SAX import javax.xml.parsers.SAXParser; import javax.xml.parsers.SAXParserFactory; import org.xml.sax.XMLReader; //SAX and external XSD import javax.xml.transform.Source; import javax.xml.transform.stream.StreamSource; import javax.xml.validation.SchemaFactory; import javax.xml.parsers.ParserConfigurationException; import org.xml.sax.ErrorHandler; import org.xml.sax.SAXException; import org.xml.sax.SAXParseException; import org.xml.sax.InputSource; public class XMLUtils {   private XMLUtils() {}      // validate SAX and external XSD    public static boolean validateWithExtXSDUsingSAX(String xml, String xsd)    throws ParserConfigurationException, IOException    {     try {       SAXParserFactory factory = SAXParserFactory.newInstance();      factory.setValidating(false);        factory.setNamespaceAware(true);       SchemaFactory schemaFactory = SchemaFactory.newInstance("http://www.w3.org/2001/XMLSchema");       SAXParser parser = null;       try {          factory.setSchema(schemaFactory.newSchema(new Source[] {new StreamSource( xsd )}));          parser = factory.newSAXParser();       }       catch (SAXException se) {         System.out.println("SCHEMA : " + se.getMessage());  // problem in the XSD itself         return false;       }              XMLReader reader = parser.getXMLReader();       reader.setErrorHandler(           new ErrorHandler() {             public void warning(SAXParseException e) throws SAXException {               System.out.println("WARNING: " + e.getMessage()); // do nothing             }             public void error(SAXParseException e) throws SAXException {               System.out.println("ERROR : " + e.getMessage());               throw e;             }             public void fatalError(SAXParseException e) throws SAXException {               System.out.println("FATAL : " + e.getMessage());               throw e;             }           }           );       reader.parse(new InputSource(xml));       return true;     }         catch (ParserConfigurationException pce) {       throw pce;     }      catch (IOException io) {       throw io;     }     catch (SAXException se){       return false;   } } public static void main (String args[]) throws Exception{      System.out.println         (XMLUtils.validateWithExtXSDUsingSAX             ("c:/temp/howto.xml", "c:/temp/howto.xsd"));     /*       output :                true     */              } }

[http://www.rgagnon.com/javadetails/java-0669.html](http://www.rgagnon.com/javadetails/java-0669.html)

IBM教程上有这个，但是我测试失败：

[](http://www.ibm.com/developerworks/cn/xml/tips/x-tipvalschm/#ibm-pcon)

## SAX 示例

[清单 2](http://www.ibm.com/developerworks/cn/xml/tips/x-tipvalschm/#code2)演示了如何使用 JAXP 1.2 中的新特性，通过 SAX 解析器来验证文档。要使用 SAX 解析器来验证文档，请执行以下操作：

1. 
创建一个          

SAXParserFactory
对象。
1. 
将 namespace-aware 和 validating 特性设置为 true。
1. 
获取          

SAXParser
对象。
1. 
设置模式语言和模式源的特性（这是 JAXP 1.2 和模式中新出现的）。
1. 
解析文档。解析器必须有权访问          

ErrorHandler
对象。

### 清单 2. ValidateSAX.java 演示了 JAXP 1.2

package org.ananas.tips; import java.io.*; import org.xml.sax.*; import javax.xml.parsers.*; public class ValidateSAX {    public static String SCHEMA_LANGUAGE =       "http://java.sun.com/xml/jaxp/properties/schemaLanguage",                         XML_SCHEMA =       "http://www.w3.org/2001/XMLSchema",                         SCHEMA_SOURCE =       "http://java.sun.com/xml/jaxp/properties/schemaSource";    public final static void main(String[] args)       throws IOException, SAXException, ParserConfigurationException    {       if(args.length < 2)       {          System.err.println("usage is:");          System.err.println("   java -jar tips.jar -validatesax "                             + "input.xml schema.xsd");          return;       }       File input = new File(args[0]),            schema = new File(args[1]);       SAXParserFactory factory = SAXParserFactory.newInstance();       factory.setNamespaceAware(true);       factory.setValidating(true);       SAXParser parser = factory.newSAXParser();       try       {          parser.setProperty(SCHEMA_LANGUAGE,XML_SCHEMA);          parser.setProperty(SCHEMA_SOURCE,schema);       }       catch(SAXNotRecognizedException x)       {          System.err.println("Your SAX parser is not JAXP 1.2 compliant.");       }       parser.parse(input,new ErrorPrinter());    } }

[http://www.ibm.com/developerworks/cn/xml/tips/x-tipvalschm/](http://www.ibm.com/developerworks/cn/xml/tips/x-tipvalschm/)

有时间到话，可以在测试。

列下jaxp 1.2教程：

This release includes XML data and example programs showing how to use JAXP  to process XML. Additional examples can be found on the [http://xml.apache.org](http://xml.apache.org) site.

* 
[Sample XML Files](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/docs/samples.html#files)
* 
[Printing a DOM Tree](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/docs/samples.html#DOMEcho)
* 
[SAX Program to Count Tags](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/docs/samples.html#SAXTagCount)
* 
[Schema Examples](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/docs/samples.html#Schema)

The example programs include a cross-platform [ant](http://jakarta.apache.org/ant/index.html) build file that can be used to build and run the example. Ant is a build tool similar to

make
on Unix and

nmake
on WindowsNT that is also an XML application. To use [ant](http://jakarta.apache.org/ant/index.html), download it from the[website](http://jakarta.apache.org/site/binindex.html) and read the install docs. Alternatively, you can also view the ant

build.xml
file to see what needs to be done to manually compile and run an example program on your platform.
**Note:**
 The ant utility uses the value of the

JAVA_HOME
environment  variable to determine which Java platform it uses to compile and run the  sample scripts. Make sure that variable points to the version you intend to  use. If using version 1.4, make sure that the JAXP jar files are installed,  as described in the [Release Notes.](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/docs/ReleaseNotes.html)

[]()

### Sample XML Files

A handful of sample XML files have been provided in the "samples" subdirectory.  Note that the links may not work depending on your browser environment. Please  look in ../samples/data if the links do not display in your browser.

* 
A simple [Purchase Order](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/samples/data/book-order.xml)    (

book-order.xml
) suggesting how an on-line business might    send and receive data.
* 
The original [XML    specification](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/samples/data/REC-xml-19980210.xml) (

REC-xml-19980210.xml
) was written in XML.    With a prepublication version of [its    Document Type Definition (DTD) file](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/samples/data/spec.dtd) (

spec.dtd
), it's    included here as a rather sophisticated example of how DTDs are used.
* 
Courtesy of Fuji Xerox, a short XML file in Japanese, using Japanese    tags. This is a [weekly    report](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/samples/data/weekly-euc-jp.xml) (

weekly-euc-jp.xml
) with [its DTD](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/samples/data/weekly-euc-jp.dtd)    (

weekly-euc-jp.dtd
), which can be validated.
* 
From a large collection of XML sample documents at the [sunsite.unc.edu](ftp://sunsite.unc.edu/pub/sun-info/standards/xml/eg/)    FTP server, [The Two Gentlemen of    Verona](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/samples/data/two_gent.xml) (

two_gent.xml
) and [The Tragedy of Richard the Third](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/samples/data/rich_iii.xml)    (

rich_iii.xml
). These include [their DTD](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/samples/data/play.dtd)  (

play.dtd
).
* 
A simple example showing the use of [XML namespaces](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/samples/data/namespace.xml)    (

namespace.xml
).
* 
A [personal-schema.xml](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/samples/data/personal-schema.xml)    file containing some data that can be validated using its schema    definition, [personal.xsd](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/samples/data/personal.xsd).

[]()

### Printing a DOM Tree

One of the first things many programmers want to know is how to read an XML file and generate a DOM Document object from it. Use the [DOMEcho example](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/samples/DOMEcho/DOMEcho.java) to learn how to do this in three steps. The important lines are:
    // Step 1: create a DocumentBuilderFactory and setNamespaceAware     DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();     dbf.setNamespaceAware(true);     // Step 2: create a DocumentBuilder     DocumentBuilder db = dbf.newDocumentBuilder();     // Step 3: parse the input file to get a Document object     Document doc = db.parse(new File(filename));

The program also gives an example of using an error handler and of setting optional configuration options, such as validation. Finally, this program helps you understand how DOM works by showing you the structure and contents of a DOM tree.

[]()

### SAX Program to Count Tags

The [SAXLocalNameCount](http://people.apache.org/%7Eedwingo/jaxp-ri-1.2.0-fcs/samples/SAXLocalNameCount/SAXLocalNameCount.java)program counts the number of unique element local names in an XML document, ignoring the namespace name for simplicity. This example also shows one way to turn on DTD or XSD validation and how to use a SAX ErrorHandler.

There are several ways to parse a document using SAX and JAXP. We show one approach here. The first step is to bootstrap a parser. There are two ways: one is to use only the SAX API, the other is to use the JAXP utility classes in the javax.xml.parsers package. We use the second approach here because at the time of this writing it probably is the most portable solution for a JAXP compatible parser. After bootstrapping a parser/XMLReader, there are several ways to begin a parse. In this example, we use the SAX API.

### []()Schema Examples

Both of the sample programs include an option (-xsd) that lets you  validate the incoming document using XML Schema, instead of the document's DTD.  In addition, they include an -xsdss option that lets you specify the  "schema source" (the file that defines the schema for the document).

Both programs define the following constants:
    static final String JAXP_SCHEMA_LANGUAGE =         "http://java.sun.com/xml/jaxp/properties/schemaLanguage";     static final String W3C_XML_SCHEMA =         "http://www.w3.org/2001/XMLSchema";     static final String JAXP_SCHEMA_SOURCE =         "http://java.sun.com/xml/jaxp/properties/schemaSource";

The schema language property defines the language the schema is written in.  The W3C XML Schema language is specified in these examples. The schema source  property directs the parser to a schema to use, regardless of any schema pointer  that the XML instance document may contain.

This code is abstracted from the SAX example:
    SAXParserFactory spf = SAXParserFactory.newInstance();     // Set namespaceAware to true to get a parser that corresponds to     // the default SAX2 namespace feature setting.  This is necessary     // because the default value from JAXP 1.0 was defined to be false.     spf.setNamespaceAware(true);     spf.setValidating(true);     SAXParser saxParser = spf.newSAXParser();     // Set the schema language if necessary     try {        saxParser.setProperty(JAXP_SCHEMA_LANGUAGE, W3C_XML_SCHEMA);     } catch (SAXNotRecognizedException x) {         // This can happen if the parser does not support JAXP 1.2         ...     }     ...    saxParser.setProperty(JAXP_SCHEMA_SOURCE, new File(schemaSource));

And here is the code abstracted from the DOM example:

    DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();     // Set namespaceAware to true to get a DOM Level 2 tree with nodes     // containing namesapce information.  This is necessary because the     // default value from JAXP 1.0 was defined to be false.     dbf.setNamespaceAware(true);     dbf.setValidating(true);     try {        dbf.setAttribute(JAXP_SCHEMA_LANGUAGE, W3C_XML_SCHEMA);     } catch (IllegalArgumentException x) {         // This can happen if the parser does not support JAXP 1.2         ...     }     // Specify other factory configuration settings    dbf.setAttribute(JAXP_SCHEMA_SOURCE, new File(schemaSource));     ...     DocumentBuilder db = dbf.newDocumentBuilder();

Note that the values are used to modify the SAX *parser,* using

setProperty(),
but they are used to modify the DOM parser*factory*, using

setAttribute()
.

[http://people.apache.org/~edwingo/jaxp-ri-1.2.0-fcs/docs/samples.html](http://people.apache.org/~edwingo/jaxp-ri-1.2.0-fcs/docs/samples.html)

另外一种，用 Validating Parser 

 Using the Validating Parser

By now, you have done a lot of experimenting with the nonvalidating parser. It's time to have a look at the validating parser to find out what happens when you use it to parse the sample presentation.

You need to understand about two things about the validating parser at the outset:

*
* 
A schema or document type definition (DTD) is required.

* 
Because the schema or DTD is present, the

ignorableWhitespace
method is invoked whenever possible.

### Configuring the Factory

The first step is to modify the Echo program so that it uses the validating parser instead of the nonvalidating parser.

Note: The code in this section is contained in

[Echo10.java](http://docs.oracle.com/javaee/1.4/tutorial/examples/jaxp/sax/samples/Echo10.java)
.

To use the validating parser, make the following highlighted changes:
public static void main(String argv[]) {   if (argv.length != 1) {     ...   }   // Use the default (non-validating) parser  // Use the validating parser  SAXParserFactory factory = SAXParserFactory.newInstance();   factory.setValidating(true);  try {     ...

Here, you configure the factory so that it will produce a validating parser when

newSAXParser
is invoked. To configure it to return a namespace-aware parser, you can also use

setNamespaceAware(true)
. Sun's implementation supports any combination of configuration options. (If a combination is not supported by a particular implementation, it is required to generate a factory configuration error.)

### Validating with XML Schema

Although a full treatment of XML Schema is beyond the scope of this tutorial, this section shows you the steps you take to validate an XML document using an existing schema written in the XML Schema language. (To learn more about XML Schema, you can review the online tutorial, XML Schema Part 0: Primer, at

[http://www.w3.org/TR/xmlschema-0/](http://www.w3.org/TR/xmlschema-0/)
. You can also examine the sample programs that are part of the JAXP download. They use a simple XML Schema definition to validate personnel data stored in an XML file.)

Note: There are multiple schema-definition languages, including RELAX NG, Schematron, and the W3C "XML Schema" standard. (Even a DTD qualifies as a "schema," although it is the only one that does not use XML syntax to describe schema constraints.) However, "XML Schema" presents us with a terminology challenge. Although the phrase "XML Schema schema" would be precise, we'll use the phrase "XML Schema definition" to avoid the appearance of redundancy.

To be notified of validation errors in an XML document, the parser factory must be configured to create a validating parser, as shown in the preceding section. In addition, the following must be true:

*
* 
The appropriate properties must be set on the SAX parser.
* 
The appropriate error handler must be set.
* 
The document must be associated with a schema.

### Setting the SAX Parser Properties

It's helpful to start by defining the constants you'll use when setting the properties:
static final String JAXP_SCHEMA_LANGUAGE =     "http://java.sun.com/xml/jaxp/properties/schemaLanguage"; static final String W3C_XML_SCHEMA =     "http://www.w3.org/2001/XMLSchema";

Next, you configure the parser factory to generate a parser that is namespace-aware as well as validating:
...   SAXParserFactory factory = SAXParserFactory.newInstance();   factory.setNamespaceAware(true);  factory.setValidating(true);

You'll learn more about namespaces in [Validating with XML Schema](http://docs.oracle.com/javaee/1.4/tutorial/doc/JAXPDOM8.html#wp76446). For now, understand that schema validation is a namespace-oriented process. Because JAXP-compliant parsers are not namespace-aware by default, it is necessary to set the property for schema validation to work.

The last step is to configure the parser to tell it which schema language to use. Here, you use the constants you defined earlier to specify the W3C's XML Schema language:
saxParser.setProperty(JAXP_SCHEMA_LANGUAGE, W3C_XML_SCHEMA);

In the process, however, there is an extra error to handle. You'll take a look at that error next.

### Setting Up the Appropriate Error Handling

In addition to the error handling you've already learned about, there is one error that can occur when you are configuring the parser for schema-based validation. If the parser is not 1.2-compliant and therefore does not support XML Schema, it can throw a

SAXNotRecognizedException
.

To handle that case, you wrap the

setProperty()
statement in a

try
/

catch
block, as shown in the code highlighted here:
... SAXParser saxParser = factory.newSAXParser();try {  saxParser.setProperty(JAXP_SCHEMA_LANGUAGE, W3C_XML_SCHEMA); }  catch (SAXNotRecognizedException x) {  // Happens if the parser does not support JAXP 1.2   ...}...

### Associating a Document with a Schema

Now that the program is ready to validate the data using an XML Schema definition, it is only necessary to ensure that the XML document is associated with one. There are two ways to do that:

*
* 
By including a schema declaration in the XML document

* 
By specifying the schema to use in the application

Note: When the application specifies the schema to use, it overrides any schema declaration in the document.

To specify the schema definition in the document, you create XML such as this:
<documentRoot  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xsi:noNamespaceSchemaLocation='YourSchemaDefinition.xsd' >   ...

The first attribute defines the XML namespace (

xmlns
) prefix,

xsi
, which stands for XML Schema instance. The second line specifies the schema to use for elements in the document that do not have a namespace prefix--that is, for the elements you typically define in any simple, uncomplicated XML document.

Note: You'll learn about namespaces in [Validating with XML Schema](http://docs.oracle.com/javaee/1.4/tutorial/doc/JAXPDOM8.html#wp76446). For now, think of these attributes as the "magic incantation" you use to validate a simple XML file that doesn't use them. After you've learned more about namespaces, you'll see how to use XML Schema to validate complex documents that use them. Those ideas are discussed in [Validating with Multiple Namespaces](http://docs.oracle.com/javaee/1.4/tutorial/doc/JAXPDOM8.html#wp63997).

You can also specify the schema file in the application:
static final String JAXP_SCHEMA_SOURCE =     "http://java.sun.com/xml/jaxp/properties/schemaSource"; ... SAXParser saxParser = spf.newSAXParser(); ...saxParser.setProperty(JAXP_SCHEMA_SOURCE,     new File(schemaSource));

Now that you know how to use an XML Schema definition, we'll turn to the kinds of errors you can see when the application is validating its incoming data. To do that, you'll use a document type definition (DTD) as you experiment with validation.

### Experimenting with Validation Errors

To see what happens when the XML document does not specify a DTD, remove the

DOCTYPE
statement from the XML file and run the Echo program on it.

Note: The output shown here is contained in

[Echo10-01.txt](http://docs.oracle.com/javaee/1.4/tutorial/examples/jaxp/sax/samples/Echo10-01.txt)
. (The browsable version is

[Echo10-01.html](http://docs.oracle.com/javaee/1.4/tutorial/examples/jaxp/sax/samples/Echo10-01.html)
.)

The result you see looks like this:
<?xml version='1.0' encoding='UTF-8'?> ** Parsing error, line 9, uri .../slideSample01.xml   Document root element "slideshow", must match DOCTYPE root  "null"

Note: This message was generated by the JAXP 1.2 libraries. If you are using a different parser, the error message is likely to be somewhat different.

This message says that the root element of the document must match the element specified in the

DOCTYPE
declaration. That declaration specifies the document's DTD. Because you don't yet have one, it's value is null. In other words, the message is saying that you are trying to validate the document, but no DTD has been declared, because no

DOCTYPE
declaration is present.

So now you know that a DTD is a requirement for a valid document. That makes sense. What happens when you run the parser on your current version of the slide presentation, with the DTD specified?

Note: The output shown here is produced using

[slideSample07.xml](http://docs.oracle.com/javaee/1.4/tutorial/examples/xml/samples/slideSample07.xml)
, as described in [Referencing Binary Entities](http://docs.oracle.com/javaee/1.4/tutorial/doc/IntroXML3.html#wp68336). The output is contained in

[Echo10-07.txt](http://docs.oracle.com/javaee/1.4/tutorial/examples/jaxp/sax/samples/Echo10-07.txt)
. (The browsable version is

[Echo10-07.html](http://docs.oracle.com/javaee/1.4/tutorial/examples/jaxp/sax/samples/Echo10-07.html)
.)

This time, the parser gives a different error message:
  ** Parsing error, line 29, uri file:...   The content of element type "slide" must match  "(image?,title,item*)

This message says that the element found at line 29 (

<item>
) does not match the definition of the

<slide>
element in the DTD. The error occurs because the definition says that the

slide
element requires a

title
. That element is not optional, and the copyright slide does not have one. To fix the problem, add a question mark to make

title
an optional element:
<!ELEMENT slide (image?, title?, item*)>

Now what happens when you run the program?

Note: You could also remove the copyright slide, producing the same result shown next, as reflected in

[Echo10-06.txt](http://docs.oracle.com/javaee/1.4/tutorial/examples/jaxp/sax/samples/Echo10-06.txt)
. (The browsable version is

[Echo10-06.html](http://docs.oracle.com/javaee/1.4/tutorial/examples/jaxp/sax/samples/Echo10-06.html)
.)

The answer is that everything runs fine until the parser runs into the

<em>
tag contained in the overview slide. Because that tag is not defined in the DTD, the attempt to validate the document fails. The output looks like this:
  ...   ELEMENT: <title>   CHARS:   Overview   END_ELM: </title>   ELEMENT: <item>   CHARS:   Why ** Parsing error, line 28, uri: ...Element "em" must be declared.org.xml.sax.SAXParseException: ... ...

The error message identifies the part of the DTD that caused validation to fail. In this case it is the line that defines an

item
element as

(#PCDATA | item)
.

As an exercise, make a copy of the file and remove all occurrences of

<em>
from it. Can the file be validated now? (In the next section, you'll learn how to define parameter entries so that we can use XHTML in the elements we are defining as part of the slide presentation.)

### Error Handling in the Validating Parser

It is important to recognize that the only reason an exception is thrown when the file fails validation is as a result of the error-handling code you entered in the early stages of this tutorial. That code is reproduced here:
public void error(SAXParseException e) throws SAXParseException {   throw e;}

If that exception is not thrown, the validation errors are simply ignored. Try commenting out the line that throws the exception. What happens when you run the parser now?

In general, a SAX parsing error is a validation error, although you have seen that it can also be generated if the file specifies a version of XML that the parser is not prepared to handle. Remember that your application will not generate a validation exception unless you supply an error handler such as the one here. 

[http://docs.oracle.com/javaee/1.4/tutorial/doc/JAXPSAX9.html](http://docs.oracle.com/javaee/1.4/tutorial/doc/JAXPSAX9.html)
