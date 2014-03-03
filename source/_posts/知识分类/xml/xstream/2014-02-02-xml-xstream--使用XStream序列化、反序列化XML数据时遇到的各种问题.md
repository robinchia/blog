---
layout: post
title: "使用XStream序列化、反序列化XML数据时遇到的各种问题"
categories: xml
tags: 
 - xml
 - xstream
--- 

# 使用XStream序列化、反序列化XML数据时遇到的各种问题

现在参与的项目是一个纯Application Server，整个Server都是自己搭建的，使用JMS消息实现客户端和服务器的交互，交互的数据格式采用XML。说来惭愧，开始为了赶进度，所有XML消息都是使用字符串拼接的，而XML的解析则是使用DOM方式查找的。我很早就看这些代码不爽了，可惜一直没有时间去重构，最近项目加了几个人，而且美国那边也开始渐渐的把这个项目开发的控制权交给我们了，所以我开始有一些按自己的方式开发的机会了。因而最近动手开始重构这些字符串拼接的代码。

对XML到Java Bean的解析框架，熟悉一点的只有Digester和XStream，Digester貌似只能从XML文件解析成Java Bean对象，所以只能选择XStream来做了，而且同组的其他项目也有在用XStream。一直听说XStream的使用比较简单，而且我对ThoughtWorks这家公司一直比较有好感，所以还以为引入XStream不会花太多时间，然而使用以后才发现XStream并没有想象的你那么简单。不过这个也有可能是因为我不想改变原来的XML数据格式，而之前的XML数据格式的设计自然不会考虑到如何便利的使用XStream。因而记录在使用过程中遇到的问题，供后来人参考，也为自己以后如果打算开其源码提供参考。废话就到这里了，接下来步入正题。

首先对于简单的引用，XStream使用起来确实比较简单，比如自定义标签的属性、使用属性和使用子标签的定义等：
@XStreamAlias("request")
public class XmlRequest1 {
    private static XStream xstream;
    static {
        xstream = new XStream();
        xstream.autodetectAnnotations(true);
    }
   
    @XStreamAsAttribute
    private String from;
   
    @XStreamAsAttribute
    @XStreamAlias("calculate-method")
    private String calculateMethod;
   
    @XStreamAlias("request-time")
    private Date requestTime;
 
    @XStreamAlias("input-files")
    private List<InputFileInfo> inputFiles;
   
    public static String toXml(XmlRequest1 request) {
        StringWriter writer = new StringWriter();
        writer.append(Constants.XML_HEADER);
        xstream.toXML(request, writer);
        return writer.toString();
    }
    public static XmlRequest1 toInstance(String xmlContent) {
        return (XmlRequest1)xstream.fromXML(xmlContent);
}
![]()
    @XStreamAlias("input-file")
    public static class InputFileInfo {
        private String type;
        private String fileName;
        ![]()
    }
    public static void main(String[] args) {
        XmlRequest1 request = buildXmlRequest();
        System.out.println(XmlRequest1.toXml(request));
    }
    private static XmlRequest1 buildXmlRequest() {
        ![]()
    }
}

 对以上Request定义，我们可以得到如下结果：

<?xml version="1.0" encoding="UTF-8"?>
<request from="levin@host" calculate-method="advanced">
 <request-time>2012-11-28 17:11:54.664 UTC</request-time>
 <input-files>
    <input-file>
      <type>DATA</type>
      <fileName>data.2012.11.29.dat</fileName>
    </input-file>
    <input-file>
      <type>CALENDAR</type>
      <fileName>calendar.2012.11.29.dat</fileName>
    </input-file>
 </input-files>
</request>

可惜这个世界不会那么清净，这个格式有些时候貌似并不符合要求，比如request-time的格式、input-files的格式，我们实际需要的格式是这样的：

<?xml version="1.0" encoding="UTF-8"?>
<request from="levin@host" calculate-method="advanced">
 <request-time>20121128T17:51:05</request-time>
 <input-file type="DATA">data.2012.11.29.dat</input-file>
 <input-file type="CALENDAR">calendar.2012.11.29.dat</input-file>
</request>

对不同Date格式的支持可以是用Converter实现，在XStream中默认使用自己实现的DateConverter，它支持的格式是：yyyy-MM-dd HH:mm:ss.S 'UTC'，然而我们现在需要的格式是yyyy-MM-dd’T’HH:mm:ss，如果使用XStream直接注册DateConverter，可以使用配置自己的DateConverter，但是由于DateConverter的构造函数的定义以及@XStreamConverter的构造函数参数的支持方式的限制，貌似DateConverter不能很好的支持注解方式的注册，因而我时间了一个自己的DateConverter以支持注解：

public class LevinDateConverter extends DateConverter {
    public LevinDateConverter(String dateFormat) {
        super(dateFormat, new String[] { dateFormat });
    }
}

在requestTime字段中需要加入以下注解定义：

@XStreamConverter(value=LevinDateConverter.class, strings={"yyyyMMdd'T'HH:mm:ss"})
@XStreamAlias("request-time")
private Date requestTime;

对集合类，XStream提供了@XStreamImplicit注解，以将集合中的内容摊平到上一层XML元素中，其中itemFieldName的值为其使用的标签名，此时InputFileInfo类中不需要@XStreamAlias标签的定义：

@XStreamImplicit(itemFieldName="input-file")
private List<InputFileInfo> inputFiles;

对InputFileInfo中的字段，type作为属性很容易，只要为它加上@XStreamAsAttribute注解即可，而将fileName作为input-file标签的一个内容字符串，则需要使用ToAttributedValueConverter，其中Converter的参数为需要作为字符串内容的字段名：

@XStreamConverter(value=ToAttributedValueConverter.class, strings={"fileName"})
public static class InputFileInfo {
    @XStreamAsAttribute
    private String type;
private String fileName;
![]()
}

XStream对枚举类型的支持貌似不怎么好，默认注册的EnumSingleValueConverter只是使用了Enum提供的name()和静态的valueOf()方法将enum转换成String或将String转换回enum。然而有些时候XML的字符串和类定义的enum值并不完全匹配，最常见的就是大小写的不匹配，此时需要写自己的Converter。在这种情况下，我一般会在enum中定义一个name属性，这样就可以自定义enum的字符串表示。比如有TimePeriod的enum：

public enum TimePeriod {
    MONTHLY("monthly"), WEEKLY("weekly"), DAILY("daily");
   
    private String name;
   
    public String getName() {
        return name;
    }
   
    private TimePeriod(String name) {
        this.name = name;
    }
   
    public static TimePeriod toEnum(String timePeriod) {
        try {
            return Enum.valueOf(TimePeriod.class, timePeriod);
        } catch(Exception ex) {
            for(TimePeriod period : TimePeriod.values()) {
                if(period.getName().equalsIgnoreCase(timePeriod)) {
                    return period;
                }
            }
            throw new IllegalArgumentException("Cannot convert <" + timePeriod + "> to TimePeriod enum");
        }
    }
}

我们可以编写以下Converter以实现对枚举类型的更宽的容错性：

public class LevinEnumSingleNameConverter extends EnumSingleValueConverter {
    private static final String CUSTOM_ENUM_NAME_METHOD = "getName";
    private static final String CUSTOM_ENUM_VALUE_OF_METHOD = "toEnum";
   
    private Class<? extends Enum<?>> enumType;
 
    public LevinEnumSingleNameConverter(Class<? extends Enum<?>> type) {
        super(type);
        this.enumType = type;
    }
 
    @Override
    public String toString(Object obj) {
        Method method = getCustomEnumNameMethod();
        if(method == null) {
            return super.toString(obj);
        } else {
            try {
                return (String)method.invoke(obj, (Object[])null);
            } catch(Exception ex) {
                return super.toString(obj);
            }
        }
    }
 
    @Override
    public Object fromString(String str) {
        Method method = getCustomEnumStaticValueOfMethod();
        if(method == null) {
            return enhancedFromString(str);
        }
        try {
            return method.invoke(null, str);
        } catch(Exception ex) {
            return enhancedFromString(str);
        }
    }
   
    private Method getCustomEnumNameMethod() {
        try {
            return enumType.getMethod(CUSTOM_ENUM_NAME_METHOD, (Class<?>[])null);
        } catch(Exception ex) {
            return null;
        }
    }
   
    private Method getCustomEnumStaticValueOfMethod() {
        try {
            Method method = enumType.getMethod(CUSTOM_ENUM_VALUE_OF_METHOD, (Class<?>[])null);
            if(method.getModifiers() == Modifier.STATIC) {
                return method;
            }
            return null;
        } catch(Exception ex) {
            return null;
        }
    }
   
    private Object enhancedFromString(String str) {
        try {
            return super.fromString(str);
        } catch(Exception ex) {
            for(Enum<?> item : enumType.getEnumConstants()) {
                if(item.name().equalsIgnoreCase(str)) {
                    return item;
                }
            }
            throw new IllegalStateException("Cannot converter <" + str + "> to enum <" + enumType + ">");
        }
    }
}

如下方式使用即可：

@XStreamAsAttribute
@XStreamAlias("time-period")
@XStreamConverter(value=LevinEnumSingleNameConverter.class)
private TimePeriod timePeriod;

对double类型，貌似默认的DoubleConverter实现依然不给力，它不支持自定义的格式，比如我们想在序列化的时候用一下格式：” ###,##0.0########”，此时又需要编写自己的Converter：

public class FormatableDoubleConverter extends DoubleConverter {
    private String pattern;
    private DecimalFormat formatter;
   
    public FormatableDoubleConverter(String pattern) {
        this.pattern = pattern;
        this.formatter = new DecimalFormat(pattern);
    }
   
    @Override
    public String toString(Object obj) {
        if(formatter == null) {
            return super.toString(obj);
        } else {
            return formatter.format(obj);
        }
    }
   
    @Override
    public Object fromString(String str) {
        try {
            return super.fromString(str);
        } catch(Exception ex) {
            if(formatter != null) {
                try {
                    return formatter.parse(str);
                } catch(Exception e) {
                    throw new IllegalArgumentException("Cannot parse <" + str + "> to double value", e);
                }
            }
            throw new IllegalArgumentException("Cannot parse <" + str + "> to double value", ex);
        }
    }
   
    public String getPattern() {
        return pattern;
    }
}

使用方式和之前的Converter类似：

@XStreamAsAttribute
@XStreamConverter(value=FormatableDoubleConverter.class, strings={"###,##0.0########"})
private double value;

最后，还有两个XStream没法实现的，或者说我没有找到一个更好的实现方式的场景。**第一种场景是****XStream****不能很好的处理对象组合问题：**

在面向对象编程中，一般尽量的倾向于抽取相同的数据成一个类，而通过组合的方式构建整个数据结构。比如Student类中有name、address，Address是一个类，它包含city、code、street等信息，此时如果要对Student对象做如下格式序列化：
<student name=”Levin”>
 <city>shanghai</city>
 <street>zhangjiang</street>
 <code>201203</code>
</student>

貌似我没有找到可以实现的方式，XStream能做是在中间加一层address标签。对这种场景的解决方案，一种是将Address中的属性平摊到Student类中，另一种是让Student继承自Address类。不过貌似这两种都不是比较理想的办法。

**第二种场景是XStream****不能很好的处理多态问题：**

比如我们有一个Trade类，它可能表示不同的产品：
public class Trade {
    private String tradeId;
    private Product product;
![]()
}
abstract class Product {
    private String name;
    public Product(String name) {
        this.name = name;
}
![]()
}
class FX extends Product {
    private double ratio;
    public FX() {
        super("fx");
    }
    ![]()
}
class Future extends Product {
    private double maturity;
    public Future() {
        super("future");
    }
    ![]()
}

通过一些简单的设置，我们能得到如下XML格式：

<trades>
 <trade trade-id="001">
    <product class="levin.xstream.blog.FX" name="fx" ratio="0.59"/>
 </trade>
 <trade trade-id="002">
    <product class="levin.xstream.blog.Future" name="future" maturity="2.123"/>
 </trade>
</trades>

作为数据文件，对Java类的定义显然是不合理的，因而简单一些，我们可以编写自己的Converter将class属性从product中去除：

xstream.registerConverter(new ProductConverter(
        xstream.getMapper(), xstream.getReflectionProvider()));
 
    public ProductConverter(Mapper mapper, ReflectionProvider reflectionProvider) {
        super(mapper, reflectionProvider);
    }
   
    @Override
    public boolean canConvert(@SuppressWarnings("rawtypes") Class type) {
        return Product.class.isAssignableFrom(type);
    }
 
    @Override
    protected Object instantiateNewInstance(HierarchicalStreamReader reader, UnmarshallingContext context) {
        Object currentObject = context.currentObject();
        if(currentObject != null) {
            return currentObject;
        }
       
        String name = reader.getAttribute("name");
        if("fx".equals(name)) {
            return reflectionProvider.newInstance(FX.class);
        } else if("future".equals(name)) {
            return reflectionProvider.newInstance(Future.class);
        }
        throw new IllegalStateException("Cannot convert <" + name + "> product");
    }
}

在所有Production上定义@XStreamAlias(“product”)注解。这时的XML输出结果为：

<trades>
 <trade trade-id="001">
    <product name="fx" ratio="0.59"/>
 </trade>
 <trade trade-id="002">
    <product name="future" maturity="2.123"/>
 </trade>
</trades>

然而如果有人希望XML的输出结果如下呢?

<trades>
 <trade trade-id="001">
    <fx ratio="0.59"/>
 </trade>
 <trade trade-id="002">
    <future maturity="2.123"/>
 </trade>
</trades>

大概找了一下，可能可以定义自己的Mapper来解决，不过XStream的源码貌似比较复杂，没有时间深究这个问题，留着以后慢慢解决吧。

**补充：**

对Map类型数据，XStream默认使用以下格式显示：
<map class="linked-hash-map">
    <entry>
      <string>key1</string>
      <string>value1</string>
    </entry>
    <entry>
      <string>key2</string>
      <string>value2</string>
    </entry>
 </map>

 

但是对一些简单的Map，我们希望如下显示：
 <map>
    <entry key="key1" value="value1"/>
    <entry key="key2" value="value2"/>
 </map>

对这种需求需要通过编写Converter解决，继承自MapConverter，覆盖以下函数，这里的Map默认key和value都是String类型，如果他们不是String类型，需要另外添加逻辑：
@SuppressWarnings("rawtypes")
@Override
public void marshal(Object source, HierarchicalStreamWriter writer,
        MarshallingContext context) {
    Map map = (Map) source;
    for (Iterator iterator = map.entrySet().iterator(); iterator.hasNext();) {
        Entry entry = (Entry) iterator.next();
        ExtendedHierarchicalStreamWriterHelper.startNode(writer, mapper()
                .serializedClass(Map.Entry.class), entry.getClass());
 
        writer.addAttribute("key", entry.getKey().toString());
        writer.addAttribute("value", entry.getValue().toString());
        writer.endNode();
    }
}
 
@Override
@SuppressWarnings({ "unchecked", "rawtypes" })
protected void putCurrentEntryIntoMap(HierarchicalStreamReader reader,
        UnmarshallingContext context, Map map, Map target) {
    Object key = reader.getAttribute("key");
    Object value = reader.getAttribute("value");
 
    target.put(key, value);
}

但是只是使用Converter，得到的结果多了一个class属性：
 <map class="linked-hash-map">
    <entry key="key1" value="value1"/>
    <entry key="key2" value="value2"/>
 </map>

在XStream中，如果定义的字段是一个父类或接口，在序列化是会默认加入class属性以确定反序列化时用的类，为了去掉这个class属性，可以定义默认的实现类来解决（虽然感觉这种解决方案不太好，但是目前还没有找到更好的解决方案）。

**
*xstream.addDefaultImplementation(LinkedHashMap.class, Map.class);*

来源： <[http://www.blogjava.net/DLevin/archive/2012/11/30/392240.html](http://www.blogjava.net/DLevin/archive/2012/11/30/392240.html)>
 
