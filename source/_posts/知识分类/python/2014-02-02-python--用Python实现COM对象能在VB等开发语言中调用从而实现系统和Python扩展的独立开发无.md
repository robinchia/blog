---
layout: post
title: "用Python实现COM对象 能在VB等开发语言中调用 从而实现系统和Python扩展的独立开发 无"
categories: python
tags: 
 - python
--- 

# 用Python实现COM对象 能在VB等开发语言中调用 从而实现系统和Python扩展的独立开发 无缝集成

用Python实现COM对象 能在VB等开发语言中调用 从而实现系统和Python扩展的独立开发 无缝集成

作者：马维峰 李林 王晓蕊

        可以通过在组件式GIS开发中集成Python来提高开发效率和质量。Python可以在GIS系统开发中编写数据的导入导出、处理、分析等模块，以及应用 系统的业务逻辑层和科学研究中的空间分析、地学建模等模块。Python和组件式GIS可以通过PythonCOM实现的Python的COM接口来集 成，在VB等开发语言中调用使用Python开发的COM服务器组件，从而实现了GIS系统和Python扩展的独立开发，无缝集成……
**1.1 初识Python**
随着计算机和地理信息技术的飞速发展，GIS理论与应用的逐渐成熟，组件式技术已逐渐成为GIS 软件的主流，改变了传统集成式GIS 平台的工作模式，更适合用户进行二次开发以及与MIS、OA等其它系统的有机集成。代表性的组件式GIS有ERSI的ArcGIS和北京超图的 SuperMap Object。SuperMap Object是由北京超图公司和中科院地理与资源所开发研制的新一代大型组件式地理信息系统平台。

       由于大多数的脚本语言具有语法简单，易于学习，解释性、无需编译，易于部署和维护 等优点，与直接使用静态编程语言C、C++、VB等语言相比，使用动态脚本语言开发效率更高，开发和维护难度较低，作为系统扩展和快速开发语言具有一般静 态编程语言不具有的优势。通过脚本语言扩展或集成已有系统，在很多软件平台中得到了广泛应用。在ArcGIS、SuperMap Object等基于COM的组件式地理信息系统平台中，可以通过Python等支持COM的脚本语言对系统进行功能扩展，集成其他程序，编写数据的导入导 出、处理、分析等模块，特别是在应用系统开发的业务逻辑层（中间层）和科学研究中的空间分析、地学建模等方面更具优势。
Python是一门解释性、面向对象、动态语义特征的高层语言（Lutz，2001）。Python具有高层的内建数据结构，动态类型和动态绑定，丰 富的标准和第三方扩展库（包括科学计算、统计分析、可视化等模块），简单而易于阅读的语法，使其非常适合作快速应用开发来开发新系统、扩展已有系统，或作 为胶水语言来连接、集成已有部件。有关Python的信息和语法等请参考Python官方网站（[http://www.python.org](http://www.python.org/)）。
本文将介绍如何应用Python来扩展SuperMap Object等组件式GIS的思路、方法和优势。
**1.2****应用Python来扩展组件式GIS**
**1.2.1 Python的集成方式**
在一般的编程语言中，集成或调用Python有以下几种方式：（1）在C或C++语言中，可以利用Python提供的接口，调用Python的特定功 能（Rossum，2003）；（2）通过数据文件方式，通过操作系统的管道方式，直接调用Python执行环境和功能模块完成特定功能；（3）在 Windows环境下，可以使用PythonCOM扩展模块将Python编写的对象包装成COM对象，供支持COM的编程语言调用 （Hammond，2000）。其中（1）和（3）种方法为“无缝”集成，方法（2）为松散集成。
本文将通过PythonCOM扩展模块来完成与SuperMap Object相互调用。PythonCOM是由ActiveState公司开发的一个Python与COM、ASP集成的扩展模块，有关PythonCOM的信息请参考ActiveState公司网站（[http://www.activestate.com](http://www.activestate.com/)）。以下将详细介绍如何创建一个可以被其他编程语言调用的Python模块，以及此模块如何与SuperMap Object交互完成特定的任务。
**1.2.2****应用Python[]()实现COM对象**
使用Python实现一个COM对象首先需要定义一个Python类，实现需要的属性和方法；其次需要应用PythonCOM扩展，实现 ProgID、CLSID等属性，在注册表里注册这个Python类，使其成为一个可以被调用的COM对象。下面将通过一个简单的例子来说明这个过程。
我们来设计一个可以计算用户表达式的COM对象，以下是代码：
# SimpleEVAL.py – 表达式求解
class PythonEVAL:
_public_methods_ = [ 'MyEval' ]
_reg_progid_ = "PythonGISDemos.EVAL"
# Guid of this COM Searver
_reg_clsid_ = "{6288B5B7-870F-494E-A4F0-99868729804E}"
def MyEval(self, source, my_x):
x = my_x
return eval(source)
# 自注册部分
if __name__=='__main__':
print "Registering COM server..."
import win32com.server.register
win32com.server.register.UseCommandLine(PythonEVAL)

以上的文件定义有3个特殊的属性字段，分别是：
_public_methods_
可以被调用的方法列表，本代码只有一个方法“MyEval”。
_reg_progid_
对象的名称，用来调用这个对象时使用。
_reg_clsid_
COM对象的CLSID，可以使用“pythoncom.CreateGuid()”这个方法来生成，也可以使用别的工具。
MyEval为我们定义的方法，参数为一个含“x”的表达式（字符串），和x的具体值，然后调用这个方法，返回计算后的值。后面的部分为自注册模块， 如果是在Python的交互环境下直接调用此模块，则运行这段代码。将这个文件保存在“Python23\Lib\site-packages”这个目录 下，首先运行一次，使其在注册表里注册这个COM组件。
这个对象可以应用在某些需要用户给出具体的表达式进行分析计算的模块中。在编写这个类的过程中，可以在Python的交互环境下边编写边测试，有助于验证算法和逻辑的正确性，保证开发效率和质量。
Python开发的COM对象也可以通过命令行进行注册和反注册。注册一个对象：“Python.exe YourServer.py”；反注册一个对象：“Python.exe YourServer.py –unregister”。需要说明的是这个对象一定要有以上程序的注册部分这一段代码。
PythonCOM还定义了除了“_public_methods_”以外其他一些特殊的属性字段，可以描述COM对象的其他特性。在代码中，还可以 通过“raise COMException("the error info")”这样的语句来进行错误处理。对于使用Python创建COM对象的其他细节问题请参考PythonCOM相关文档。
**1.2.3****调用Python实现的COM对象**
调用Python编写的COM对象和调用其他COM对象方法一样，以下将通过VB代码来实例说明其调用方法。
在VB中新建一个工程，在缺省窗体上放置3个文本框和一个按钮，在Form_Load事件里编写对COM对象的初始化代码，在按钮点击事件里，编写COM对象调用的代码，在Form_Unload事件里，销毁这个对象。全部代码如下：

Public myServer As Object
Private Sub Command1_Click()
Text3.Text=myServer.MyEval(Text1.Text,Int(Text2.Text))
End Sub
Private Sub Form_Load()
Set myServer = CreateObject("PythonGISDemos.EVAL")
End Sub
Private Sub Form_Unload(Cancel As Integer)
Set myServer = Nothing
End Sub

整个代码和调用其他COM对象没有什么差别。以下是程序运行的界面，对x=2，表达式为“3*x – x*x”进行求解，结果是2。
![http://www.gisforum.net/magazine/050415/images/picrdjs05.gif]()
图 1 VB中调用Python编写的COM对象的运行结果
我们可以把这段代码插入使用SuperMap Object等组件式GIS，开发语言为VB或其他语言的开发项目中需要用户给出具体的表达式进行分析计算的模块中。需要说明的是，对于参数传递，数据类 型，对象调用等使用COM开发需要注意的问题，在这里一样需要注意。另外有两个问题需要注意，第一，对于VB开发环境，VB是大小写不敏感的，而 Python是区分大小写的；第二是Unicode的问题，COM传递的字符串都是Unicode，[]()**在Python中**处理使用之前要根据需要转换为Python的字符串。
**1.3 应用实例**
下面将使用Python来实现一个最小二乘拟合的模块。以下是代码：

# LeastSq.py – 最小二乘拟合
from scipy import *
from scipy.optimize import leastsq

class PythonSq:
_public_methods_ = ['AddX','AddY','leastsq','GetP']
_reg_progid_ = "PythonGISDemos.LeastSq"
# Guid of this COM Searver
_reg_clsid_ = "{6278B5B7-870F-494E-A4F0-92368724804E}"
#程序中需要拟合的X和Y的值
x=[]
y=[]
#输入X和Y的值，由于一般语言没有对应的序列等数据结构，所以逐个输入
def AddX(self, xx):
self.x.append(xx)
def AddY(self, yy):
self.y.append(yy)
#求解剩余变量
def residuals(self, p, y, x):
a, b, c = p
y = array(y) #将序列转换为矩阵
x = array(x)
err = y - (a*x*x + b*x + c)
return err
#求解参数
def leastsq(self):
m_p = [1.0,1.0,1.0]
self.p = leastsq(self.residuals,m_p,args=(self.y,self.x))
#求返回值
def GetP(self, index):
return self.p[0][index]
# 自注册部分（略）

程序代码的说明见注释。程序使用了SciPy扩展模块optimize模块（对于非线性方程，进行回归分析建议使用本方法，SciPy另外专门有线性代数模块（构建于ATLAS LAPACK和BLAS），可以进行线性代数运算，对于线性回归，具有更好的速度和稳定性。） （http://www.scipy.org），代码量很短，核心代码仅10余行，主要处理了输入和输出，定义了剩余变量的求解方式，主要的最小二次拟合 的代码仅1行，直接调用了optimize模块的leastsq方法。如果要改变拟合方程式，只需在residuals方法内修改“err = y - (a*x*x + b*x + c)”括号内的部分即可。数据通过逐个输入方式输入，然后由Python读进其List中（如果需要传递大数据量的数据，笔者建议使用外部文件方式，由 VB等客户程序创建一个数据文件，然后将此文件路径传递给Python的COM组件，由Python读入（Python具有非常简单的文件读取和写入方式），构建为内部的List、Array或Matrix。） 。
在VB或其他语言中新建一个工程，测试此COM对象，以下为VB6下一个简单的测试代码：

Dim objSq As Object
Set objSq = CreateObject("PythonGISDemos.LeastSq")
Dim i As Integer
'初始化回归值
For i = 0 To 100
objSq.AddX (CSng(i))
objSq.AddY (CSng(i) ^ 2 + Rnd())
Next
'调用回归方法
objSq.leastsq
MsgBox "A = " & objSq.GetP(0)
MsgBox "B = " & objSq.GetP(1)
MsgBox "C = " & objSq.GetP(2)
Set objSq = Nothing

对于非线性方程，需要给出参数的初始值（leastsq方法内的m_p的值）。我们也可以使用前面举例介绍过的“eval”函数，编写一个可以对任意 表达式进行求解的最小二次拟合模块。对于代码的编写，可以首先在Python的交互式开发环境和IDE（Python自带的IDLE，PythonWin 等）下，边编写边测试，确定语法和逻辑没有错误后，即可完成模块，然后在其他环境下测试和使用，可以提高开发效率和质量。
**1.4****结论**
通过PythonCOM接口，在基于COM的组件式GIS（例如SuperMap Object，ArcGIS）以及可以调用COM组件进行扩展的GIS工具的开发和扩展中集成Python，应用Python来开发部分功能组件具有以下优势：
1) 充分发挥Python动态语言解释性、语法简单、易于阅读、面向对象、动态语义、具有高层的内建数据结构等优势，可以提高开发效率和开发质量，缩短开发周期；
2) 基于COM的组件式开发保证了GIS系统的开发和Python扩展的开发的独立性，对于Python对象开发的组件，可以在任意时刻随意修改（包括编译打包后），不会影响GIS组件对其的调用；
3) 通过COM接口的调用和集成实现了Python和GIS工具的无缝集成；
4) Python优秀的科学计算、统计分析、可视化等扩展模块（SciPy，见[http://www.scipy.org](http://www.scipy.org/)）可以方便的应用在GIS的数据处理、空间分析、地学模型分析等领域；
5) Python丰富的标准库和第三方扩展库可以在GIS与MIS、OA等应用性系统的集成开发中发挥积极的作用。

**附：**本文所列的代码都在Python和VB等环境下测试通过，测试软件环 境（Windows XP，VB 6.0，Python 2.3 + SciPy 0.3）。文中代码都略去了错误处理和捕获，在实际应用中，应该使用适当的错误处理机制。程序中有关Python、SciPy扩展模块、COM、VB等的 语法和使用方法，请参考其官方网站和相应文档。
