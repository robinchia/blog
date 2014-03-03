---
layout: post
title: "Eclipse 调优及使用小细节"
categories: eclipse
tags: 
 - eclipse
--- 

# Eclipse 调优及使用小细节

### Eclipse 调优及使用小细节

下了个eclipse JUNO，界面有了很大改变，感觉更小清新了。不啰嗦了，直接上干货。

eclipse4.2的界面：

 ![eclipse调优及使用小细节]()

elicpse是我新装的，所以照着以前的内容一起实践了一下

**装插件，现在eclipse的插件安装很方便，从网上下载下来直接扔到dropin文件夹就行**

├─findbugs

├─jadeclipse

├─m2eclipse

├─site-1.8.7

├─spket-1.6.18

├─tomcat_3.3.0
findbug是很好用的一个代码检查工具

jad是用来反编译的工具
m2eclipse是mvn的一个插件工具

site是svn的插件工具
spket是用来搞extjs

tomcat顾名思义就是tomcat的集成工具
还有一个umlet是用来画uml图的工具

**调整内存和启动项**
加大JVM非堆内存

-vmargs -Xms128M -Xmx512M -XX:PermSize=64M -XX:MaxPermSize=128M

A 各个参数的含义

JVM初始分配的内存由-Xms指定，默认是物理内存的1/64；

JVM最大分配的内存由-Xmx指定，默认是物理内存的1/4。默认空余堆内存小于40%时，JVM就会增大堆直到-Xmx的最大限制；空余堆内 存大于70%时，JVM会减少堆直到-Xms的最小限制。因此服务器一般设置-Xms、-Xmx相等以避免在每次GC 后调整堆的大小

JVM使用-XX:PermSize设置非堆内存初始值，默认是物理内存的1/64；由XX:MaxPermSize设置最大非堆内存的大小，默认是物理内存的1/4。

B  Eclipse.ini文件的配置

-vmargs

-Xms128M

-Xmx512M

-XX:PermSize=64M

-XX:MaxPermSize=128M

实际运行的结果可以通过Eclipse中“Help”-“About Eclipse SDK”窗口里面的“Configuration Details”按钮进行查看。
参考： [http://hi.baidu.com/injava/item/9dee73d67f77fd94270ae777](http://hi.baidu.com/injava/item/9dee73d67f77fd94270ae777)

****

**取消不需要加载的模块及自动验证的配置文件**

一个系统20%的功能往往能够满足80%的需求，MyEclipse也不例外，我们在大多数时候只需要20%的系统功能，所以可以将一些不使用的模 块禁止加载启动。通过Windows - Preferences打开配置窗口，依次选择左侧的General - Startup and Shutdown，这个时候在右侧就显示出了Eclipse启动时加载的模块，可以根据自己的实际情况去除一些模块。

默认情况下MyEclipse在启动的时候会自动验证每个项目的配置文件，这是一个非常耗时的过程，可以在Preferences窗口依次选择 MyEclipse - Validation，然后在右侧的Validator列表中只保留 Manual 项就可以了。如果需要验证的时候只需要选中文件，然后右键选择 MyEclipse - Run Validation就可以了。

** eclipse设置自动提示**

默认是输入"."后出现自动提示，用于类成员的自动提示，可是有时候我们希望它能在我们输入类的首字母后就出现自动提示，可以节省大量的输入时间（虽然按alt + /会出现提示，但还是要多按一次按键，太麻烦了）。

 从Window -> preferences -> Java -> Editor -> Content assist -> Auto-Activation下，我们可以在"."号后面加入我们需要自动提示的首字幕，比如"ahiz"。

然后我们回到Eclipse的开发环境，输入"a"，提示就出现了。但是我们可以发现，这个Auto-Activation下的输入框里最多只能输 入5个字母，也许是Eclipse的开发人员担心我们输入的太多会影响性能，但计算机的性能不用白不用，所以我们要打破这个限制。

在"."后面随便输入几个字符，比如"abij"，然后回到开发环境，File -> export -> general -> preferences -> 选一个地方保存你的首选项，比如C:\a.epf

用任何文本编辑器打开a.epf，查找字符串“abij”，找到以后，替换成“abcdefghijklmnopqrstuvwxyz”，总之就是 你想怎样就怎样！！然后回到Eclipse，File -> import -> general -> preferences -> 导入刚才的a.epf文件。此时你会发现输入任何字幕都可以得到自动提示了。爽！！！

最后：自动提示弹出的时间最好改成100毫秒以下，这样会比较爽一点，不然你都完事了，自动提示才弹出来:)，不过也要看机器性能。

参考 ：[http://www.whhpaccp.com/hpaccp/studyjava_08243792.html#](http://www.whhpaccp.com/hpaccp/studyjava_08243792.html#)

**设置java的环境变量及其配置**

这个内容本来是最开始就应该操作的，这里算是插播一下，位置在 我的电脑-属性-高级-系统变量-

JAVA_HOME ：JAVA的JDK安装的目录C:\Program Files\Java\jdk1.6.0_21

PATH:最前面追加 .;%JAVA_HOME%\bin;

**myeclipse启动tomcat console中没有信息提示的情况**

这个应该是最开始用eclipse犯的错误，各种system.out，但是tomcat的console一直没有内容，其实你只要检查一下将Console中的“Open Console”切换成“Java Stack Trace Console”试试。

参考：[http://menglimengwai.iteye.com/blog/370043](http://menglimengwai.iteye.com/blog/370043)

**eclipse常用的快捷键**
【ALT+/】此快捷键为用户编辑的好帮手，能为用户提供内容的辅助，不要为记不全方法和属性名称犯愁，当记不全类、方法和属性的名字时，多体验一下【ALT+/】快捷键带来的好处吧。

【Ctrl+O】显示类中方法和属性的大纲，能快速定位类的方法和属性，在查找Bug时非常有用。

【Ctrl+/】快速添加注释，能为光标所在行或所选定行快速添加注释或取消注释，在调试的时候可能总会需要注释一些东西或取消注释，现在好了，不需要每行进行重复的注释。

【Ctrl+D】删除当前行，这也是笔者的最爱之一，不用为删除一行而按那么多次的删除键。

【Ctrl+M】窗口最大化和还原，用户在窗口中进行操作时，总会觉得当前窗口小（尤其在编写代码时），现在好了，试试【Ctrl+M】快捷键。

【Ctrl+K】、【Ctrl++Shift+K】快速向下和向上查找选定的内容，从此不再需要用鼠标单击查找对话框了。

【Ctrl+Shift+T】查找工作空间（Workspace）构建路径中的可找到Java类文件，不要为找不到类而痛苦，而且可以使用“*”、“？”等通配符。

【Ctrl+Shift+R】和【Ctrl+Shift+T】对应，查找工作空间（Workspace）中的所有文件（包括Java文件），也可以使用通配符。

【Ctrl+Shift+G】查找类、方法和属性的引用。这是一个非常实用的快捷键，例如要修改引用某个方法的代码，可以通过【Ctrl+Shift+G】快捷键迅速定位所有引用此方法的位置。

【Ctrl+Shift+O】快速生成import，当从网上拷贝一段程序后，不知道如何import进所调用的类，试试【Ctrl+Shift+O】快捷键，一定会有惊喜。

【Ctrl+Shift+F】格式化代码，书写格式规范的代码是每一个程序员的必修之课，当看见某段代码极不顺眼时，选定后按【Ctrl+Shift+F】快捷键可以格式化这段代码，如果不选定代码则默认格式化当前文件（Java文件）。

【ALT+Shift+W】查找当前文件所在项目中的路径，可以快速定位浏览器视图的位置，如果想查找某个文件所在的包时，此快捷键非常有用（特别在比较大的项目中）。

【Ctrl+L】定位到当前编辑器的某一行，对非Java文件也有效。

【Alt+←】、【Alt+→】后退历史记录和前进历史记录，在跟踪代码时非常有用，用户可能查找了几个有关联的地方，但可能记不清楚了，可以通过这两个快捷键定位查找的顺序。

【F3】快速定位光标位置的某个类、方法和属性。

【F4】显示类的继承关系，并打开类继承视图。

【Ctrl+Shift+B】：在当前行设置断点或取消设置的断点。

【F11】：调试最后一次执行的程序。

【Ctrl+F11】：运行最后一次执行的程序。

【F5】：跟踪到方法中，当程序执行到某方法时，可以按【F5】键跟踪到方法中。

【F6】：单步执行程序。

【F7】：执行完方法，返回到调用此方法的后一条语句。

【F8】：继续执行，到下一个断点或程序结束。

【Ctrl+C】：复制。

【Ctrl+X】：剪切。

【Ctrl+V】：粘贴。

【Ctrl+S】：保存文件。

【Ctrl+Z】：撤销。

【Ctrl+Y】：重复。

【Ctrl+F】：查找。

【Ctrl+F6】：切换到下一个编辑器。

【Ctrl+Shift+F6】：切换到上一个编辑器。

【Ctrl+F7】：切换到下一个视图。

【Ctrl+Shift+F7】：切换到上一个视图。

【Ctrl+F8】：切换到下一个透视图。

【Ctrl+Shift+F8】：切换到上一个透视图。

------------------------------------------------------------------------------------

1. edit->content Assist - > add      Alt+/ 代码关联

2. Window -> Next Editor -> add    Ctrl+Tab 切换窗口

3. Run/Debug Toggle Line Breakpoint -> add Ctrl+` 在调试的时候 增删断点

4. Source-> Surround with try/catch Block -> Ctrl+Shift+v 增加try catch 框框

5. Source -> Generate Getters and Setters -> Ctrl+Shift+. 增加get set 方法

------------------------------------------------------------------------------------

Alt+/ 代码助手完成一些代码的插入(但一般和输入法有冲突,可以修改输入法的热键,也可以暂用Alt+/来代替)

Ctrl+1:光标停在某变量上，按Ctrl+1键，可以提供快速重构方案。选中若干行，按Ctrl+1键可将此段代码放入for、while、if、do或try等代码块中。

双击左括号（小括号、中括号、大括号），将选择括号内的所有内容。

Alt+Enter 显示当前选择资源(工程,or 文件 or文件)的属性

Ctrl+K:将光标停留在变量上，按Ctrl+K键可以查找到下一个同样的变量

Ctrl+Shift+K:和Ctrl+K查找的方向相反

Ctrl+E 快速显示当前Editer的下拉列表(如果当前页面没有显示的用黑体表示)

Ctrl+Shift+E 显示管理当前打开的所有的View的管理器(可以选择关闭,激活等操作)

Ctrl+Q 定位到最后编辑的地方

Ctrl+L 定位在某行 (对于程序超过100的人就有福音了)

Ctrl+M 最大化当前的Edit或View (再按则反之)

Ctrl+/ 注释当前行,再按则取消注释

Ctrl+T 快速显示当前类的继承结构

Ctrl+Shift-T: 打开类型（Open type）。如果你不是有意磨洋工，还是忘记通过源码树（source tree）打开的方式吧。

Ctrl+O:在代码中打开类似大纲视图的小窗口

Ctrl+鼠标停留:可以显示类和方法的源码

Ctrl+H:打开搜索窗口

Ctrl+/(小键盘) 折叠当前类中的所有代码

Ctrl+×(小键盘) 展开当前类中的所有代码

-----------Ctrl+Shift 系列-----------

Ctrl+Shift+F 格式化当前代码

Ctrl+Shift+X 把当前选中的文本全部变味小写

Ctrl+Shift+Y 把当前选中的文本全部变为小写

Ctrl+Shift+O:快速地导入import

Ctrl+Shift+R:打开资源 open Resource

F3:打开声明该引用的文件

F4:打开类型层次结构

F5:单步跳入

F6:单步跳过

F7:单步跳出

F8:继续，如果后面没有断点，程序将运行完

Ctrl+D: 删除当前行

Ctrl+Alt+↓ 复制当前行到下一行(复制增加)

Ctrl+Alt+↑ 复制当前行到上一行(复制增加)

Alt+↓ 当前行和下面一行交互位置(特别实用,可以省去先剪切,再粘贴了)

Alt+↑ 当前行和上面一行交互位置(同上)

Shift+Enter 在当前行的下一行插入空行(这时鼠标可以在当前行的任一位置,不一定是最后)

Ctrl+Shift+Enter 在当前行插入空行(原理同上条)

Alt+← 前一个编辑的页面

Alt+→ 下一个编辑的页面(当然是针对上面那条来说了)

Ctrl+Shift+S:保存全部

Ctrl+W 关闭当前Editer

Ctrl+Shift+F4 关闭所有打开的Editer

Ctrl+Shift+G: 在workspace中搜索引用

Ctrl+Shift+P 定位到对于的匹配符(譬如{}) (从前面定位后面时,光标要在匹配符里面,后面到前面,则反之)

Ctrl+J 正向增量查找(按下Ctrl+J后,你所输入的每个字母编辑器都提供快速匹配定位到某个单词,如果没有,则在stutes line中显示没有找到了,查一个单词时,特别实用,这个功能Idea两年前就有了)

Ctrl+Shift+J 反向增量查找(和上条相同,只不过是从后往前查)
来源： <[http://www.open-open.com/lib/view/open1348033080365.html](http://www.open-open.com/lib/view/open1348033080365.html)> 
