---
layout: post
title: "Python学习笔记"
categories: python
tags: 
 - python
--- 

# Python学习笔记

# (Michael) Xiaofeng Yang

M.Sc. Candidate
[School of Computing](http://www.cs.queensu.ca/)
[Queen's University](http://www.queensu.ca/)

* [Home](http://michael-yxf.appspot.com/)
* [Research](http://michael-yxf.appspot.com/research)
* [WikiNotes](http://michael-yxf.appspot.com/wiki)
* [Blog](http://michael-yxf.appspot.com/blog)
* [Album](http://michael-yxf.appspot.com/album)
# Python学习笔记

[Python介绍](http://michael-yxf.appspot.com/wiki/notes/python.html#sec1)[程序设计](http://michael-yxf.appspot.com/wiki/notes/python.html#sec2)[基础语法](http://michael-yxf.appspot.com/wiki/notes/python.html#sec3)[变量](http://michael-yxf.appspot.com/wiki/notes/python.html#sec4)[运算符](http://michael-yxf.appspot.com/wiki/notes/python.html#sec5)[参数](http://michael-yxf.appspot.com/wiki/notes/python.html#sec6)[语句控制](http://michael-yxf.appspot.com/wiki/notes/python.html#sec7)[函数](http://michael-yxf.appspot.com/wiki/notes/python.html#sec8)[数据结构](http://michael-yxf.appspot.com/wiki/notes/python.html#sec9)[字符串](http://michael-yxf.appspot.com/wiki/notes/python.html#sec10)[列表](http://michael-yxf.appspot.com/wiki/notes/python.html#sec11)[字典](http://michael-yxf.appspot.com/wiki/notes/python.html#sec12)[元组](http://michael-yxf.appspot.com/wiki/notes/python.html#sec13)[面向对象](http://michael-yxf.appspot.com/wiki/notes/python.html#sec14)[基础](http://michael-yxf.appspot.com/wiki/notes/python.html#sec15)[对象属性](http://michael-yxf.appspot.com/wiki/notes/python.html#sec16)[对象方法](http://michael-yxf.appspot.com/wiki/notes/python.html#sec17)[运算符重载](http://michael-yxf.appspot.com/wiki/notes/python.html#sec18)[对象继承](http://michael-yxf.appspot.com/wiki/notes/python.html#sec19)[高级编程](http://michael-yxf.appspot.com/wiki/notes/python.html#sec20)[正则表达式](http://michael-yxf.appspot.com/wiki/notes/python.html#sec21)[文件处理](http://michael-yxf.appspot.com/wiki/notes/python.html#sec22)[XML 编程](http://michael-yxf.appspot.com/wiki/notes/python.html#sec23)[网络编程](http://michael-yxf.appspot.com/wiki/notes/python.html#sec24)[数据库](http://michael-yxf.appspot.com/wiki/notes/python.html#sec25)[常用标准库](http://michael-yxf.appspot.com/wiki/notes/python.html#sec26)[参考资料](http://michael-yxf.appspot.com/wiki/notes/python.html#sec27)

## []()Python介绍

* Python 是一种解释性语言，程序是被解释器来解析执行的。
* 版本信息：$ python -V
* python 大小写敏感
* python 帮助文档设置:

前提是安装python2.5-doc （名字因版本不同可不同）
env PATHONDOCS=/usr/share/doc/python2.5 # python >>> help(print) ## 打印print函数信息 
在python中，可以定义模块或函数的帮助文档，用三引号表示 """ ....... """, 然后调用函数或模块的 __doc__ 属性

* 测试程序:
#! env python print "hello world"

* 文件编码
#! env python #-*- coding: UTF-8 -*-

* 一些不错的句子摘录：
Program testing can be used to show the presence of bugs, but never to show their absence! ---- Edsger W. DijkstraFinding a hard bug requires reading, running, ruminating, and sometimes retreating. If you get stuck on one of these activities, try the others.

## []()程序设计

### []()基础语法

* 注释语句用

#
表示
* 使用数学公式要先导入

math
模块
* 
None
是一种特殊值，代表无效值，注意与

"None"
不同，后者表示值为

None
的字符串，前者无值。
* 注意

==
和

===
的区别：前者表示比较值的大小是否等同，后者用来测试是否为同一对象。
* 读取用户输入数据：

input=raw_input("Please enter a value:")
* 读取到的值存储在字符串中，如上所示的

input
，如果期待的是一个整型数值，则需要进行显示转换:

int(input)
.
* Python里的两个boolean值：

True
和

False
* 判断某变量是否属于指定的数据类型：

isinstance(var, type)
* 长整型数据表示：

2L
* 字符串:

* 在python里，单引号和双引号是完全相同的
* """ : 表示中间的单引号和双引号都不会被解析，字符串可以换行
* unicode字符串要加前缀u或U
* 自然字符串用r前缀表示: r"Newlines are indicated by \n"
* 转义符:

* \' 表示单引号'
* \出现在行末，表示字符串在下一行继续，不表示换行
* 标识符命名:

* 首字母为大小写字母

(a-zA-Z)
或

"_"
* 其他字母由大小写字母

(a-zA-Z)，"_"
和数字

(0-9)
组成
* 大小写敏感
* 语句：

* 多条语句在同一行用";"分隔
* 一条语句在多行用"\"来分隔
* 缩进：

在python里，不能随便缩进。缩进一次代表进入一个block，类似于进入 "{}"语句
* exec:

执行一段python代码
* eval:

计算表示式
* assert:

条件声明，当fail时，会抛出AssertionError
* 空值:

python里的空值为None

### []()变量

* 变量:

不需要声明和定义数据类型，可直接使用
* 声明全局变量:
global x

### []()运算符

特殊需要记住的:

//
表示取整除，返回商的整数部分, 其余都一样

* and 和 or 运算符:

在python里，and 和 or 运算符并不返回布尔值，而是返回最后计算的运算子，例如:
'a' and 'b' 返回 'b' 'a' or 'b' 返回 'a'

* python里没有

++, --
运算符，采用

+=, -=
代替实现功能。

### []()参数

* 同Java语言一样，Python的参数传递是值传递，即传递的是参数值的拷贝。

### 参数类型

* 函数参数:

多个参数跟在函数后面用","分隔，例如:
lang='python' print 'this is a test for', lang

* 
print
语句结尾加逗号

,
表示分行语句。
* 默认参数：只有在形参表末尾的那些参数可以有默认参数值，即你不能在声明函数形参的时候，先声明有默认值的形参而后声明没有默认值的形参

### 参数传递

* 对数组或字典类型的数据传递用*和**前缀来表示，例如*argv，则所有参数传递作为数组类型存储在argv变量里，如果是**argv，则作为键值对存储在argv中.
* 每个传递给程序的命令行参数都以列表的形式存储在 sys.argv里面:

sys.argv[0] 表示文件名本身，

sys.argv[1]...
表示真正传递给脚本的参数对于更复杂的通过参数选项来传递参数值，用 gotopt 模块来实现:
opts, args = getopt.getopt(argv, "hg:d", ["help", "grammar="])

### GetOpt功能

* 接收3个参数: 参数列表(argv)， 短标志选项(h, g:, d)，长标志选项(help, grammar=)
* 返回1个元组 (tuple) opts 和参数列表 args:
opts: (flag, argument)，flag为选项名，如 -d, --help，argument存储其对应的参数值，如果没有，则为None args: 剩余的其他参数，如果没有，则为None

* 短标志说明:
g: 表示 -g 选项后必须跟一个参数

* 长标志说明:
grammar= 表示 --grammar 选项后必须跟一个参数 
长标志和短标志参数的顺序要保持一致，允许缺少某个参数选项，只要顺序一致就行。

### []()语句控制

* for 循环:
for var in array:

* 控制语句：

* python里没有switch ... case 语句，使用if...elif...else语句
* True和False被称为布尔类型。你可以分别把它们等效地理解为值1和0。
* while循环语句可以跟一个else从句
* 异常语句:

* try...except: 捕捉异常

* try ... except 可以跟 else 语句
* try...finally: 异常发生时总是要执行的语句
* raise: 引发异常
* 超类: Error 和 Exception

### []()函数

* lambda 函数:

用lambda算子创建新的函数对象，例如:
def base_fun(n): return lambda s:s*n //new_fun 
定义一个函数叫

base_fun
，该函数执行的结果返回一个新的函数

new_fun
，参数是

s
，返回的结果是

s*n
比如

new_fun = base_fun(2)
，则返回的函数

new_fun(s)
为:

s*2
然后执行

new_fun(5)
，则返回

5*2=10

* glob 函数:

glob采用通配符来获取所有条件匹配的文件或文件夹，存储在返回的list中，例如:
import glob glob.glob("/home/michael/*/*.mp3") 
获取"/home/michael"目录及其子目录下的所有mp3文件列表

## []()数据结构

### []()字符串

* 字符串是不可改变对象 immutable object
* 常用方法:
str.upper() str.find(ch, [startindex], [endindex]) str.strip()

* 遍历运算符

in
:
for letter in word: ......

* 
in
还可用来判断属组中是否含有某元素:
if letter in word: .... if letter not in word: ....

* string 模块:
string.lowercase: 所有的小写字母

### []()列表

* 列表的引用:

* 列表的赋值语句不创建拷贝
* 拷贝可以通过列表的slice来建立
* 列表是可改变对象
* 
range
方法产生常用连续数字列表
range(1,5) => [1,2,3,4] range(5) => [1,2,3,4] range(1,5,2) => [1,3]

* 运算符：
+ : 连接多个list * : 重复列表多次

* 列表切割：
t = ['a', 'b', 'c', 'd', 'e', 'f'] t[1:3] => ['b', 'c'] t[:3] => ['a', 'b', 'c'] t[3:] => ['d', 'e', 'f'] t[:] => ['a', 'b', 'c', 'd', 'e', 'f']

* 列表方法：
t1 = ['a', 'b', 'c'] t2 = ['d', 'e'] t1.append(t2): 添加列表元素，参数可以为另一个列表 ['a', 'b', 'c', ['d', 'e']] t1 = ['a', 'b', 'c'] t2 = ['d', 'e'] t1.extend(t2) => ['a', 'b', 'c', 'd', 'e']

* 字符串转换成列表
l = list(s) l = s.split() l = s.split(delimiter) #默认的分隔符为空格

* 列表转换成字符串
delimiter.join(l)

* 列表函数
li.append(), li.insert(), li.extend(), li.index() li.remove(), li.pop()

### []()字典

* 构造：内建函数

dict()
创建一个空词典对象
* 
in
操作符用来判断是否存在某个键值

key
* 返回所有的值:

dict.values()
* 
get
方法：

dict.get('key', [default])

**NOTE**: 在词典对象中，只有具有

hashtable
方法的对象才能作为key值，所以必须是

immutable
的对象，比如

int, float, string, tuple
。例如

list, dictionary
就不能作为key。
* 基于dictionary的字符串格式化:

* %(key)s 表示在dictionary中对应key键值的value，例如params = {'pwd':'pass', 'pwd2':'pass2'}
"%(pwd)s is not correct." % params 
返回 "pass is not correct."

* 字典函数
del d[index]

* locals 和 globals:

* locals()函数返回局部变量的dictionary的一个拷贝
* globals()函数返回实际的全局变量的dictionary，修改其中的值会影响全局变量

### []()元组

元组数据(Tuple)是不可更改对象(immutable)，通常用于返回多个变量值，例如

return firstname, lastname
.

* 元组里可以使用字符串和列表里的操作
t = tuple('abcd') => ('a', 'b', 'c', 'd') t[1:3] => ('b', 'c')

* 元组赋值非常灵活，比如两个变量值的交换:
a, b = b, a

* 元组在用于返回多个值时非常有用:
quot, rem = divmod(7,3) quot => 2 rem => 1

* 词典对象

items
方法的返回值就是一组元组列表
d = {'a':0, 'b':1, 'c':2} t = d.items() => [('a',0), ('b', 1), ('c', 2)]

* 元组与词典对象的转换：
d = dict(t)

* 元组数据可以作为

dictionary
对象的键

key
dic[last, first] = number for last, first in dic: print first, last, dic[last, first]

* 元组函数
tuple只读, tuple没有函数方法

## []()面向对象

### []()基础

* 包的导入:

包其实就是一个普通的目录，即文件夹，但其特征就是位于该目录下的__init__.py文件; 没有此文件的目录仅仅是一个普通文件夹
* self:

类的方法比较特殊，必须加上self参数，等同于Java里的this，指代当前对象本身
* import <module> as <m>:

在程序中简写模块的名称
* 类属性和数据属性：

数据属性定义在 __init__ 中

类属性定义在类的空间里，类似于Java中的static数据

### []()对象属性

* 浅拷贝 (shallow copy):

copy.copy(obj)
* 深拷贝 (deep copy):

copy.deepcopy(obj)
* 判断对象是否含有某个属性:
>>> hasattr(obj, 'attributename')

* 对象的属性信息都存储在该对象的一个

dictionary
里:
>>> print p.__dict__ {'y':4, 'x':3}

该例子表示对象

p
中含有属性

y
和

x
，其值分别为4和3。

def print_attributes(obj): for attr in obj.__dict__: print attr, getattr(obj, attr)

### []()对象方法

* 特殊方法:

* __init__: 初始化类的方法，等同于Java里的constructor
* __del__: 类似于Java里的desconstructor, 类不再被使用时执行，但无法确定在什么时候被运行
* __str__: 类对象的表示，用于print对象时的输出，等同于Java中的toString()方法
* __lt__(self, other): 用于与其他对象的比较，如果小于other对象，则返回True

* __getitem__(self, key): 定义此函数后，可以对对象调用其<o>[key]来返回对应于key的item
* __len__(self): 对序列对象使用内建的len()函数，比如在对象内定义了__len__(self) 函数后，可以直接调用对象的len()函数
* __: python里用__前缀表示私有变量, __privatevar，用_前缀表示只在类或对象内使用的变量
* repr():

返回对象的规范字符串表示，效果和反引号`` 一样, 和str()的不同是，str()是用在print对象时的输出字符串，repr()和``是直接返回表示对象的字符串，如果两个值一样的话，其关系如以下所示:
print <obj> == print repr(<obj>) == print `<obj>`

* 私有方法:

python中私有方法是在方法名前加上前缀 "__"来表示，从外部无法直接调用方法

调用私有方法可以通过

_<class><method>
来调用，例如类Test里定义了一个私有方法叫

__method()
，从外部调用就是

_Test__method

虽然可以这么调用私有方法，但建议从不采用这种方法，否则就违背了私有方法存在的目的

* 对象的内置函数:
type(), str(), dir() dir函数返回对象的所有属性，字段，方法 三个函数都归结到了内置模块 __builtin__ 中了，所以等同于调用 __builtin__.type(), __builtin__.str()

函数与方法的区别：

* 函数指过程性的语句，方法指属于某个类对象的函数，定义的方式不一样
* 函数指代的纯函数，和类没有任何关系
* 方法定义的第一个参数必须是该方法所属的类对象，即关键字表示的

this
* 函数定义:
class Time: ...... def print_time(time): print '%.2d:%.2d:%.2d' % (time.hour, time.minute, time.second)

* 方法定义:
class Time: .... def print_time(self): print '%.2d:%.2d:%.2d' % (self.hour, self.minute, self.second)

* 在方法调用时，参数

self
被省略掉。
* 
__str__(self)
方法:用于打印该对象所显示的对象信息
* 
__repr__(self)
方法:用于

eval
某个对象产生的结果

### []()运算符重载

* 重载

+
，通过定义

__add(self, other)__
方法来实现
* 
__radd__(self, other)__
方法用于当对象出现于

+
的右边。
* 当对象实现了

__add()__
方法时，

sum()
方法就适用于该对象。
* 
__cmp__
用于定义对对象的大小比较。
* 当对象实现了

__cmp__
方法后，可以采用

sort()
方法来对对象进行排序。

### []()对象继承

* 继承定义:
class Hand(Deck): ....

## []()高级编程

### []()正则表达式

* re.sub
import re s = 'abcdef' re.sub(<reg_pattern>,"",s): 
re模块用sub方法来替代符合正则表达的字符串，在指定正则表达式时，为了减小"转义字符灾难"的影响，可以加上r前缀，这样在表达式中的所有字符串不需要进一步转义，比如:
re.sub(r'\bROAD$', 'RD.', s)就比re.sub('\\bROAD$', 'RD.', s) 清晰多了

* re.search(pattern, 'string')

松散正则表达: re.search(pattern, 'string', re.VERBOSE)
pattern = """ ^ # beginning of string M{0,4} #thousands - 0 to 4 M's $ # end of string """ re.compile(r'regpattern').search(s) re.compile(r'regpattern', re.VERBOSE).search(s)

### []()文件处理

* 打开文件:

>>> fin=open('file.txt')
* 读取：
fin.readline()

* 写入：
fin.write(linetext)

* 绝对路径:

os.path.abspath('memo.txt')
* 判断路径是否存在:

os.path.exists('memo.txt')
* 获取当前路径:

os.getcwd()
* 判断文件或目录：
os.path.isfile() os.path.isdir()

* 列出所有文件：

os.listdir(cwd)

### []()XML 编程

### 解析XML文件

>>> from xml.dom import minidom >>> xmldoc = minidom.parse(filename)xmldoc 返回一个

Document
对象， 该对象是

Node
的子类。从

Node
继承下来的方法有:

childNodes, firstChild, lastChild, toxml
。

### []()网络编程

* HTTP 状态代码
200 - OK 404 - Not Found 301 - 永久重定向 302 - 临时重定向 304 - 从上次访问后没有修改

### urllib 模块

urllib 模块依赖于 httplib 模块，可以通过设置 httplib 模块属性打开调试选项: httplib.HTTPConnection.debuglevel = 1

使用方法：

urllib.open()

用minidom.parse函数处理各种类型文件:

web页面: urlopen() 本地文件: open() 字符串: StringIO()

stdout, stderr
都是文件类型对象，但是只能

写入
操作，不能读出。

stdin
是只读的文件类型对象。

### urllib2 模块

用

urllib2
模块来获取HTTP资源分为三个步骤完成:

* 构建

Request
对象:

request = urllib2.Request(res)
* 构建

opener
处理器:

opener = urllib2.build_opener()
* 用构建的处理器去获取

Request
对象：

feeddata = opener.open(request)

Request
对象通过

add_header
方法来更改HTTP头数据的属性值。

feeddata.headers
是一个

dictionary
对象。

检查Web服务有没有更新所使用的不同头数据：

Last-Modified
:

If-Modified-Since

Etag
:

If-None-Match

### []()数据库

### anydbm模块

anydbm
模块是以键值对

key:value
来存储数据的，可以如同对

dictionary
数据类型一样进行操作:
>>> import anydbm >>> db = anydbm.open('dbname.db', 'c') >>> db['keyname'] = 'value of the key' >>> print db['keyname'] 
anydbm
的局限性是键值都必须为字符串类型。

### Pickling模块

Pickling
可以存储对象数据
>>> import pickle >>> t = [1, 2, 3] >>> pickle.dumps(t) '(lp0\nI1\naI2\naI3\na.' >>> t2 = pickle.loads(t) >>> print t2 [1, 2, 3]

### []()常用标准库

### sys

sys.argv[0] 指向程序名 sys.version sys.stdout/sys.stdin/sys.stderr

### os

os.name: 平台名字, windows下返回"NT", linux下返回"posix" os.getcwd(): 返回当前的工作目录 os.getenv(), os.putenv(): 读取和设置当前环境变量 os.listdir(): 返回指定目录下的所有文件和目录 os.remove(): 删除一个文件 os.system(): 执行一个shell命令 os.linesep: 当前平台上的行终止符, linux下返回'\n', windows下返回'\r\n' os.path.split(): 返回一个路径的目录名和文件名 (<dir>,<file>) os.path.isfile(), os.path.isdir(): 检验给出的路径是文件夹还是文件 os.path.exists(): 检验给出的路径是否存在

## []()参考资料

* Byte of Python
* Dive into Python
* Think like a Python Programmer
* Core Python Programming
*Last Updated: 2010-02-23 []()Michael Yang*

Success is a Science
CopyRight© 2008, 2009, 2010 Michael Yang
[![gmail]()](mailto:michael.yxf@gmail.com)
