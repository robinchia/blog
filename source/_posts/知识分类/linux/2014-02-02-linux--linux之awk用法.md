---
layout: post
title: "linux之awk用法"
categories: linux
tags: 
 - linux
--- 

# linux之awk用法

awk是一个非常棒的数字处理工具。相比于sed常常作用于一整行的处理，awk则比较倾向于将一行分为数个“字段”来处理。运行效率高，而且代码简单，对格式化的文本处理能力超强。先来一个例子：
文件a，统计文件a的第一列中是浮点数的行的浮点数的平均值。用awk来实现只需要一句话就可以搞定
$cat a
1.021 33
1#.ll   44
2.53 6
ss    7
awk 'BEGIN{total = 0;len = 0} {if($1~/^[0-9]+\.[0-9]*/){total += $1; len++}} END{print total/len}' a
（分析：$1~/^[0-9]+\.[0-9]*/表示$1与“/ /”里面的正则表达式进行匹配，若匹配，则total加上$1，且len自增，即数目加1.“^[0-9]+\.[0-9]*”是个正则表达式，“^[0-9]”表示以数字开头，“\.”是转义的意思，表示“.”为小数点的意思。“[0-9]*”表示0个或多个数字）

awk的一般语法格式为：
awk [-参数 变量] 'BEGIN{初始化}条件类型1{动作1}条件类型2{动作2}。。。。END{后处理}'
其中：BEGIN和END中的语句分别在开始读取文件（in_file）之前和读取完文件之后发挥作用，可以理解为初始化和扫尾。
**（1）参数说明：**
 -F re：允许awk更改其字段分隔符
      -v var=$v 把v值赋值给var，如果有多个变量要赋值，那么就写多个-v，每个变量赋值对应一个-v
e.g. 要打印文件a的第num行到num+num1行之间的行，
awk -v num=$num -v num1=$num1 'NR==num,NR==num+num1{print}' a
-f progfile：允许awk调用并执行progfile程序文件，当然progfile必须是一个符合awk语法的程序文件。

**（2）awk内置变量：**
**ARGC**    命令行参数的个数
**ARGV  ** 命令行参数数组
**ARGIND** 当前被处理文件的ARGV标志符
e.g 有两个文件a 和b
awk '{if(ARGIND==1){print "处理a文件"} if(ARGIND==2){print "处理b文件"}}' a b
文件处理的顺序是先扫描完a文件，再扫描b文件

**NR 　　**已经读出的记录数
**FNR**   　当前文件的记录数
上面的例子也可以写成这样：
awk 'NR==FNR{print "处理文件a"} NR > FNR{print "处理文件b"}' a b
输入文件a和b，由于先扫描a，所以扫描a的时候必然有NR==FNR，然后扫描b的时候，FNR从1开始计数，而NR则接着a的行数继续计数，所以NR > FNR

e.g 要显示文件的第10行至第15行
awk 'NR==10,NR==15{print}' a

**FS 　　**输入字段分隔符（缺省为:space:），相当于-F选项
awk -F ':' '{print}' a    和   awk 'BEGIN{FS=":"}{print}' a 是一样的

**OFS**输出字段分隔符（缺省为:space:）
awk -F ':' 'BEGIN{OFS=";"}{print $1,$2,$3}' b
如果cat b为
1:2:3
4:5:6
那么把OFS设置成";"后就会输出
1;2;3
4;5;6
（小注释：awk把分割后的第1、2、3个字段用$1,$2,$3...表示，$0表示整个记录（一般就是一整行））

**NF**：当前记录中的字段个数
awk -F ':' '{print NF}' b的输出为
3
3
表明b的每一行用分隔符":"分割后都3个字段
可以用NF来控制输出符合要求的字段数的行，这样可以处理掉一些异常的行
awk -F ':' '{if (NF == 3)print}' b

**RS**：输入记录分隔符，缺省为"\n"
缺省情况下，awk把一行看作一个记录；如果设置了RS，那么awk按照RS来分割记录
例如，如果文件c，cat c为
hello world; I want to go swimming tomorrow;hiahia
运行 awk 'BEGIN{ RS = ";" } {print}' c 的结果为
hello world
I want to go swimming tomorrow
hiahia
合理的使用RS和FS可以使得awk处理更多模式的文档，例如可以一次处理多行，例如文档d cat d的输出为
1 2
3 4 5
6 7
8 9 10
11 12

hello
每个记录使用空行分割，每个字段使用换行符分割，这样的awk也很好写
awk 'BEGIN{ FS = "\n"; RS = ""} {print NF}' d 输出
2
3
1

**ORS**：输出记录分隔符，缺省为换行符，控制每个print语句后的输出符号
awk 'BEGIN{ FS = "\n"; RS = ""; ORS = ";"} {print NF}' d 输出
2;3;1
**（3）awk读取shell中的变量**
可以使用-v选项实现功能
     $b=1
     $cat f
     apple
$awk -v var=$b '{print var, $var}' f
1 apple
至于有没有办法把awk中的变量传给shell呢，这个问题我是这样理解的。shell调用awk实际上是fork一个子进程出来，而子进程是无法向父进程传递变量的，除非用重定向（包括管道）
a=$(awk '{print $b, '$b'}' f)
$echo $a
apple 1

****（4）**输出重定向**

awk的输出重定向类似于shell的重定向。重定向的目标文件名必须用双引号引用起来。
$awk '$4 >=70 {print $1,$2 > "destfile" }' filename
$awk '$4 >=70 {print $1,$2 >> "destfile" }' filename

**（5）awk中调用shell命令：**

1)使用**管道**
awk中的管道概念和shell的管道类似，都是使用"|"符号。如果在awk程序中打开了管道，必须先关闭该管道才能打开另一个管道。也就是说一次只能打开一个管道。shell命令必须被双引号引用起来。“如果打算再次在awk程序中使用某个文件或管道进行读写，则可能要先关闭程序，因为其中的管道会保持打开状态直至脚本运行结束。注意，管道一旦被打开，就会保持打开状态直至awk退出。因此END块中的语句也会收到管道的影响。（可以在END的第一行关闭管道）”
awk中使用管道有两种语法，分别是：
awk output | shell input
shell output | awk input

对于awk output | shell input来说，shell接收awk的输出，并进行处理。需要注意的是，awk的output是先缓存在pipe中，等输出完毕后再调用shell命令 处理，shell命令只处理一次，而且处理的时机是“awk程序结束时，或者管道关闭时（需要显式的关闭管道）”
$awk '/west/{count++} {printf "%s %s\t\t%-15s\n", $3,$4,$1 | "sort +1"} END{close "sort +1"; printf "The number of sales pers in the western"; printf "region is " count "." }' datafile （解释：/west/{count++}表示与“wes”t进行匹配，若匹配，则count自增）
printf函数用于将输出格式化并发送给管道。所有输出集齐后，被一同发送给sort命令。必须用与打开时完全相同的命令来关闭管道(sort +1)，否则END块中的语句将与前面的输出一起被排序。此处的sort命令只执行一次。

在shell output | awk input中awk的input只能是getline函数。shell执行的结果缓存于pipe中，再传送给awk处理，如果有多行数据，awk的getline命令可能调用多次。
来源： <[http://www.cnblogs.com/dong008259/archive/2011/12/06/2277287.html](http://www.cnblogs.com/dong008259/archive/2011/12/06/2277287.html)> 
