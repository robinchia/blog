---
layout: post
title: "think in java interview-高级开发人员面试宝典(八)"
categories: 面试参考
tags: 
 - 面试参考
 - think in java interview
--- 

# think in java interview-高级开发人员面试宝典(八)

### [[置顶] think in java interview-高级开发人员面试宝典(八)]()

分类： [面经](http://blog.csdn.net/lifetragedy/article/category/1544093) 2013-08-26 22:05 2026人阅读 [评论]()(23) [收藏]( "收藏") [举报]( "举报")
目录[(?)]( "系统根据文章中H1到H6标签自动生成文章目录")[[+]]( "展开")

1. [Java IO流的复习]()
1. [InputFromConsole]()
1. [ListDir]()
1. [ListMyDirWithSubDir]()
1. [InputStreamDemo]()
1. [OutputStreamDemo]()
1. [CopyFile]()
1. [BufferInputStreamDemo]()
1. [ByteArrayDemo]()
1. [RandomAccess]()
1. [PipeStream]()

1. [SendMessage]()
1. [TestPipeStream]()
1. [Serializable的IO操作]()

1. [Person]()
1. [SerializablePersonToFile]()

面经出了7套，收到许多读者的Email，有许多人说了，这些基础知识是不是为了后面进一步的”通向架构师的道路“做准备的？

对的，你们没有猜错，就是这样的，我一直在酝酿后面的”通向架构师的道路“如何开章。

说实话，我已经在肚子里准备好的后面的”通向架构师的道路“的内容自己觉得如果一下子全拿出来的话，很多人吃不消，因为架构越来越复杂，用到的知识越来越多，而且很多都是各知识点的混合应用。

所以，先以这几套面经来铺路，我们把基础打实了，才能把大楼造的更好。因为，一个架构师首先他是一个程序员，他的基础知识必须非常的扎实，API对于架构师来说已经不太需要eclipse的code insight(即在eclipse编辑器里打一个小点点就可以得到后面的函数），尤其是一些常用的JAVA API来说，是必须熟记于心的。

下面我们继续来几天面经，顺带便复习一下JAVA和数据库的一些基础。

# []()Java IO流的复习

大家平时J2EE写多了，JAVA的IO操作可能都已经生疏了，面试时如果来上这么几道，是不是有点”其实这个问题很简单，可是我就是想不起来“的感觉啊？

呵呵！

JAVA的IO操作太多，我这边挑腾迅，盛大和百度的几道面试题，并整理出答案来供大家参考。

# []()InputFromConsole

这个最简单不过了，从console接受用户输入的字符，如和用户有交互的命令行。

如果你不复习的话，嘿嘿，还真答不出，来看：

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. public class InputFromConsole {
1.
1. /**
1. * @param args
1. */
1. public static void main(String[] args) throws Exception {
1. int a = 0;
1. byte[] input = new byte[1024];
1. System.in.read(input);
1. System.out.println("your input is: " + new String(input));
1.
1. }
1.
1. }

package org.sky.io;

public class InputFromConsole {
/**

* @param args
*/

public static void main(String[] args) throws Exception {
int a = 0;

byte[] input = new byte[1024];
System.in.read(input);

System.out.println("your input is: " + new String(input));
}

}

# []()ListDir

列出给出路径下所有的目录，包括子目录

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. import java.io.*;
1.
1. public class ListMyDir {
1.
1. /**
1. * @param args
1. */
1. public static void main(String[] args) {
1. String fileName = "D:" + File.separator + "tomcat2";
1. File f = new File(fileName);
1. File[] fs = f.listFiles();
1. for (int i = 0; i < fs.length; i++) {
1. System.out.println(fs[i].getName());
1. }
1.
1. }
1.
1. }

package org.sky.io;

import java.io.*;
public class ListMyDir {

/**
* @param args

*/
public static void main(String[] args) {

String fileName = "D:" + File.separator + "tomcat2";
File f = new File(fileName);

File[] fs = f.listFiles();
for (int i = 0; i < fs.length; i++) {

System.out.println(fs[i].getName());
}

}
}

咦，上面这个程序只列出了一层目录，我们想连子目录一起List出来怎么办？

# []()ListMyDirWithSubDir

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. import java.io.*;
1.
1. public class ListMyDirWithSubDir {
1.
1. /**
1. * @param args
1. */
1. public void print(File f) {
1. if (f != null) {
1. if (f.isDirectory()) {
1. File[] fileArray = f.listFiles();
1. if (fileArray != null) {
1. for (int i = 0; i < fileArray.length; i++) {
1. print(fileArray[i]);
1. }
1. }
1. } else {
1. System.out.println(f);
1. }
1. }
1. }
1.
1. public static void main(String[] args) {
1. String fileName = "D:" + File.separator + "tomcat2";
1. File f = new File(fileName);
1. ListMyDirWithSubDir listDir = new ListMyDirWithSubDir();
1. listDir.print(f);
1.
1. }
1. }

package org.sky.io;

import java.io.*;
public class ListMyDirWithSubDir {

/**
* @param args

*/
public void print(File f) {

if (f != null) {
if (f.isDirectory()) {

File[] fileArray = f.listFiles();
if (fileArray != null) {

for (int i = 0; i < fileArray.length; i++) {
print(fileArray[i]);

}
}

} else {
System.out.println(f);

}
}

}
public static void main(String[] args) {

String fileName = "D:" + File.separator + "tomcat2";
File f = new File(fileName);

ListMyDirWithSubDir listDir = new ListMyDirWithSubDir();
listDir.print(f);

}
}

# []()InputStreamDemo

从外部读入一个文件

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. import java.io.File;
1. import java.io.FileInputStream;
1. import java.io.FileOutputStream;
1. import java.io.InputStream;
1. import java.io.OutputStream;
1.
1. public class InputStreamDemo {
1. public void readFile(String fileName) {
1. File srcFile = new File(fileName);
1. InputStream in = null;
1. try {
1. in = new FileInputStream(srcFile);
1. byte[] b = new byte[(int) srcFile.length()];
1. for (int i = 0; i < b.length; i++) {
1. b[i] = (byte) in.read();
1. }
1. System.out.println(new String(b));
1. } catch (Exception e) {
1. e.printStackTrace();
1. } finally {
1. try {
1. if (in != null) {
1. in.close();
1. in = null;
1. }
1. } catch (Exception e) {
1. }
1. }
1. }
1.
1. public static void main(String[] args) {
1. InputStreamDemo id = new InputStreamDemo();
1. String src = "D:" + File.separator + "hello.txt";
1. id.readFile(src);
1. }
1.
1. }

package org.sky.io;

import java.io.File;
import java.io.FileInputStream;

import java.io.FileOutputStream;
import java.io.InputStream;

import java.io.OutputStream;
public class InputStreamDemo {

public void readFile(String fileName) {
File srcFile = new File(fileName);

InputStream in = null;
try {

in = new FileInputStream(srcFile);
byte[] b = new byte[(int) srcFile.length()];

for (int i = 0; i < b.length; i++) {
b[i] = (byte) in.read();

}
System.out.println(new String(b));

} catch (Exception e) {
e.printStackTrace();

} finally {
try {

if (in != null) {
in.close();

in = null;
}

} catch (Exception e) {
}

}
}

public static void main(String[] args) {
InputStreamDemo id = new InputStreamDemo();

String src = "D:" + File.separator + "hello.txt";
id.readFile(src);

}
}

# []()OutputStreamDemo

讲完了InputStream来讲OutputStream，输出内容至外部的一个文件

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. import java.io.*;
1.
1. public class OutputStreamDemo {
1.
1. public void writeWithByte() {
1. String fileName = "D:" + File.separator + "hello.txt";
1. OutputStream out = null;
1. File f = new File(fileName);
1. try {
1. out = new FileOutputStream(f, true);
1. String str = " [Publicity ministry of ShangHai Municipal committee of CPC]";
1. byte[] b = str.getBytes();
1. out.write(b);
1. } catch (Exception e) {
1. e.printStackTrace();
1. } finally {
1. try {
1. if (out != null) {
1. out.close();
1. out = null;
1. }
1. } catch (Exception e) {
1. }
1. }
1. }
1.
1. public void writeWithByteArray() {
1. String fileName = "D:" + File.separator + "hello.txt";
1. OutputStream out = null;
1. File f = new File(fileName);
1. try {
1. out = new FileOutputStream(f, true);
1. String str = " [hello with byte yi ge ge xie]";
1. byte[] b = str.getBytes();
1. for (int i = 0; i < b.length; i++) {
1. out.write(b[i]);
1. }
1. } catch (Exception e) {
1. e.printStackTrace();
1. } finally {
1. try {
1. if (out != null) {
1. out.close();
1. out = null;
1. }
1. } catch (Exception e) {
1. }
1. }
1. }
1.
1. public static void main(String[] args) {
1. OutputStreamDemo od = new OutputStreamDemo();
1. od.writeWithByte();
1. od.writeWithByteArray();
1.
1. }
1.
1. }

package org.sky.io;

import java.io.*;
public class OutputStreamDemo {

public void writeWithByte() {
String fileName = "D:" + File.separator + "hello.txt";

OutputStream out = null;
File f = new File(fileName);

try {
out = new FileOutputStream(f, true);

String str = " [Publicity ministry of ShangHai Municipal committee of CPC]";
byte[] b = str.getBytes();

out.write(b);
} catch (Exception e) {

e.printStackTrace();
} finally {

try {
if (out != null) {

out.close();
out = null;

}
} catch (Exception e) {

}
}

}
public void writeWithByteArray() {

String fileName = "D:" + File.separator + "hello.txt";
OutputStream out = null;

File f = new File(fileName);
try {

out = new FileOutputStream(f, true);
String str = " [hello with byte yi ge ge xie]";

byte[] b = str.getBytes();
for (int i = 0; i < b.length; i++) {

out.write(b[i]);
}

} catch (Exception e) {
e.printStackTrace();

} finally {
try {

if (out != null) {
out.close();

out = null;
}

} catch (Exception e) {
}

}
}

public static void main(String[] args) {
OutputStreamDemo od = new OutputStreamDemo();

od.writeWithByte();
od.writeWithByteArray();

}
}
这个Demo里分别用了”writeWithByte“和 ”writeWithByteArray“两种方法，注意查看

# []()CopyFile

我们讲完了InputStream和OutputStream，我们就可以自己实现一个File Copy的功能了，来看

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. import java.io.*;
1.
1. public class CopyFile {
1.
1. public void copy(String src, String des) {
1. File srcFile = new File(src);
1. File desFile = new File(des);
1. InputStream in = null;
1. OutputStream out = null;
1. try {
1. in = new FileInputStream(srcFile);
1. out = new FileOutputStream(desFile);
1. byte[] b = new byte[(int) srcFile.length()];
1. for (int i = 0; i < b.length; i++) {
1. b[i] = (byte) in.read();
1. }
1. out.write(b);
1. System.out.println("copied [" + srcFile.getName() + "] with "
1. + srcFile.length());
1. } catch (Exception e) {
1. e.printStackTrace();
1. } finally {
1. try {
1. if (out != null) {
1. out.close();
1. out = null;
1. }
1. } catch (Exception e) {
1. }
1. try {
1. if (in != null) {
1. in.close();
1. in = null;
1. }
1. } catch (Exception e) {
1. }
1. }
1. }
1.
1. public static void main(String[] args) {
1. CopyFile cp = new CopyFile();
1. String src = "D:" + File.separator + "UltraEdit.zip";
1. String des = "D:" + File.separator + "UltraEdit_Copy.zip";
1. long sTime = System.currentTimeMillis();
1. cp.copy(src, des);
1. long eTime = System.currentTimeMillis();
1. System.out.println("Total spend: " + (eTime - sTime));
1. }
1.
1. }

package org.sky.io;

import java.io.*;
public class CopyFile {

public void copy(String src, String des) {
File srcFile = new File(src);

File desFile = new File(des);
InputStream in = null;

OutputStream out = null;
try {

in = new FileInputStream(srcFile);
out = new FileOutputStream(desFile);

byte[] b = new byte[(int) srcFile.length()];
for (int i = 0; i < b.length; i++) {

b[i] = (byte) in.read();
}

out.write(b);
System.out.println("copied [" + srcFile.getName() + "] with "

+ srcFile.length());
} catch (Exception e) {

e.printStackTrace();
} finally {

try {
if (out != null) {

out.close();
out = null;

}
} catch (Exception e) {

}
try {

if (in != null) {
in.close();

in = null;
}

} catch (Exception e) {
}

}
}

public static void main(String[] args) {
CopyFile cp = new CopyFile();

String src = "D:" + File.separator + "UltraEdit.zip";
String des = "D:" + File.separator + "UltraEdit_Copy.zip";

long sTime = System.currentTimeMillis();
cp.copy(src, des);

long eTime = System.currentTimeMillis();
System.out.println("Total spend: " + (eTime - sTime));

}
}
运行后显示：

![]()

来看我们被Copy的这个文件的大小：

![]()

也不大，怎么用了7秒多？

**原是我们没有使用Buffer这个东西**，即缓冲，性能会相差多大呢？来看

# []()BufferInputStreamDemo

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. import java.io.*;
1.
1. public class BufferInputStreamDemo {
1.
1. /**
1. * @param args
1. */
1. public void copy(String src, String des) {
1. File srcFile = new File(src);
1. File desFile = new File(des);
1. BufferedInputStream bin = null;
1. BufferedOutputStream bout = null;
1. try {
1. bin = new BufferedInputStream(new FileInputStream(srcFile));
1. bout = new BufferedOutputStream(new FileOutputStream(desFile));
1. byte[] b = new byte[1024];
1. while (bin.read(b) != -1) {
1. bout.write(b);
1. }
1. bout.flush();
1. System.out.println("copied [" + srcFile.getName() + "] with "
1. + srcFile.length());
1. } catch (Exception e) {
1. e.printStackTrace();
1. } finally {
1. try {
1. if (bout != null) {
1. bout.close();
1. bout = null;
1. }
1. } catch (Exception e) {
1. }
1. try {
1. if (bin != null) {
1. bin.close();
1. bin = null;
1. }
1. } catch (Exception e) {
1. }
1. }
1. }
1.
1. public static void main(String[] args) {
1. BufferInputStreamDemo bd = new BufferInputStreamDemo();
1. String src = "D:" + File.separator + "UltraEdit.zip";
1. String des = "D:" + File.separator + "UltraEdit_Copy.zip";
1. long sTime = System.currentTimeMillis();
1. bd.copy(src, des);
1. long eTime = System.currentTimeMillis();
1. System.out.println("Total spend: " + (eTime - sTime));
1.
1. }
1.
1. }

package org.sky.io;

import java.io.*;
public class BufferInputStreamDemo {

/**
* @param args

*/
public void copy(String src, String des) {

File srcFile = new File(src);
File desFile = new File(des);

BufferedInputStream bin = null;
BufferedOutputStream bout = null;

try {
bin = new BufferedInputStream(new FileInputStream(srcFile));

bout = new BufferedOutputStream(new FileOutputStream(desFile));
byte[] b = new byte[1024];

while (bin.read(b) != -1) {
bout.write(b);

}
bout.flush();

System.out.println("copied [" + srcFile.getName() + "] with "
+ srcFile.length());

} catch (Exception e) {
e.printStackTrace();

} finally {
try {

if (bout != null) {
bout.close();

bout = null;
}

} catch (Exception e) {
}

try {
if (bin != null) {

bin.close();
bin = null;

}
} catch (Exception e) {

}
}

}
public static void main(String[] args) {

BufferInputStreamDemo bd = new BufferInputStreamDemo();
String src = "D:" + File.separator + "UltraEdit.zip";

String des = "D:" + File.separator + "UltraEdit_Copy.zip";
long sTime = System.currentTimeMillis();

bd.copy(src, des);
long eTime = System.currentTimeMillis();

System.out.println("Total spend: " + (eTime - sTime));
}

}
我们Copy同样一个文件，用了多少时间呢？来看！

![]()

丫只用了14毫秒，CALL！！！

# []()ByteArrayDemo

来看看使用ByteArray输出文件吧

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. import java.io.*;
1.
1. public class ByteArrayDemo {
1.
1. /**
1. * @param args
1. */
1. public void testByteArray() {
1. String str = "HOLLYJESUS";
1. ByteArrayInputStream input = null;
1. ByteArrayOutputStream output = null;
1. try {
1. input = new ByteArrayInputStream(str.getBytes());
1. output = new ByteArrayOutputStream();
1. int temp = 0;
1. while ((temp = input.read()) != -1) {
1. char ch = (char) temp;
1. output.write(Character.toLowerCase(ch));
1. }
1. String outStr = output.toString();
1. input.close();
1. output.close();
1. System.out.println(outStr);
1. } catch (Exception e) {
1. e.printStackTrace();
1. } finally {
1. try {
1. if (output != null) {
1. output.close();
1. output = null;
1. }
1. } catch (Exception e) {
1. }
1. try {
1. if (input != null) {
1. input.close();
1. input = null;
1. }
1. } catch (Exception e) {
1. }
1. }
1. }
1.
1. public static void main(String[] args) {
1. ByteArrayDemo bd = new ByteArrayDemo();
1. bd.testByteArray();
1.
1. }
1.
1. }

package org.sky.io;

import java.io.*;
public class ByteArrayDemo {

/**
* @param args

*/
public void testByteArray() {

String str = "HOLLYJESUS";
ByteArrayInputStream input = null;

ByteArrayOutputStream output = null;
try {

input = new ByteArrayInputStream(str.getBytes());
output = new ByteArrayOutputStream();

int temp = 0;
while ((temp = input.read()) != -1) {

char ch = (char) temp;
output.write(Character.toLowerCase(ch));

}
String outStr = output.toString();

input.close();
output.close();

System.out.println(outStr);
} catch (Exception e) {

e.printStackTrace();
} finally {

try {
if (output != null) {

output.close();
output = null;

}
} catch (Exception e) {

}
try {

if (input != null) {
input.close();

input = null;
}

} catch (Exception e) {
}

}
}

public static void main(String[] args) {
ByteArrayDemo bd = new ByteArrayDemo();

bd.testByteArray();
}

}

# []()RandomAccess

有种输出流叫Random，你们还记得吗？学习时记得的，工作久了，HOHO，忘了，它到底有什么特殊的地方呢？来看：

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. import java.io.*;
1.
1. public class RandomAccess {
1. public void writeToFile() {
1. String fileName = "D:" + File.separator + "hello.txt";
1. RandomAccessFile randomIO = null;
1. try {
1.
1. File f = new File(fileName);
1. randomIO = new RandomAccessFile(f, "rw");
1. randomIO.writeBytes("asdsad");
1. randomIO.writeInt(12);
1. randomIO.writeBoolean(true);
1. randomIO.writeChar('A');
1. randomIO.writeFloat(1.21f);
1. randomIO.writeDouble(12.123);
1. } catch (Exception e) {
1. e.printStackTrace();
1. } finally {
1. try {
1. if (randomIO != null) {
1. randomIO.close();
1. randomIO = null;
1. }
1. } catch (Exception e) {
1. }
1. }
1. }
1.
1. public static void main(String[] args) {
1. RandomAccess randomA = new RandomAccess();
1. randomA.writeToFile();
1. }
1. }

package org.sky.io;

import java.io.*;
public class RandomAccess {

public void writeToFile() {
String fileName = "D:" + File.separator + "hello.txt";

RandomAccessFile randomIO = null;
try {

File f = new File(fileName);
randomIO = new RandomAccessFile(f, "rw");

randomIO.writeBytes("asdsad");
randomIO.writeInt(12);

randomIO.writeBoolean(true);
randomIO.writeChar('A');

randomIO.writeFloat(1.21f);
randomIO.writeDouble(12.123);

} catch (Exception e) {
e.printStackTrace();

} finally {
try {

if (randomIO != null) {
randomIO.close();

randomIO = null;
}

} catch (Exception e) {
}

}
}

public static void main(String[] args) {
RandomAccess randomA = new RandomAccess();

randomA.writeToFile();
}

}
它输出后的文件是怎么样的呢？

![]()

# []()PipeStream

这个流很特殊，我们在线程操作时，两个线程都在运行，这时通过发送一个指令让某个线程do something，我们在以前的jdk1.4中为了实现这样的功能，使用的就是这个PipeStream

先来看两个类，一个叫SendMessage，即发送一个指令。一个叫ReceiveMessage，用于接受指令。

## []()SendMessage

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. import java.io.*;
1.
1. public class SendMessage implements Runnable {
1. private PipedOutputStream out = null;
1.
1. public PipedOutputStream getOut() {
1. return this.out;
1. }
1.
1. public SendMessage() {
1. this.out = new PipedOutputStream();
1. }
1.
1. public void send() {
1.
1. String msg = "start";
1. try {
1. out.write(msg.getBytes());
1. } catch (Exception e) {
1. e.printStackTrace();
1. } finally {
1. try {
1. if (out != null) {
1. out.close();
1. out = null;
1. }
1. } catch (Exception e) {
1. }
1. }
1. }
1.
1. public void run() {
1. try {
1. System.out.println("waiting for signal...");
1. Thread.sleep(2000);
1. send();
1. } catch (Exception e) {
1. e.printStackTrace();
1. }
1. }
1. }

package org.sky.io;

import java.io.*;
public class SendMessage implements Runnable {

private PipedOutputStream out = null;
public PipedOutputStream getOut() {

return this.out;
}

public SendMessage() {
this.out = new PipedOutputStream();

}
public void send() {

String msg = "start";
try {

out.write(msg.getBytes());
} catch (Exception e) {

e.printStackTrace();
} finally {

try {
if (out != null) {

out.close();
out = null;

}
} catch (Exception e) {

}
}

}
public void run() {

try {
System.out.println("waiting for signal...");

Thread.sleep(2000);
send();

} catch (Exception e) {
e.printStackTrace();

}
}

}
ReceiveMessage

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. import java.io.*;
1.
1. public class ReceiveMessage implements Runnable {
1. private PipedInputStream input = null;
1.
1. public PipedInputStream getInput() {
1. return this.input;
1. }
1.
1. public ReceiveMessage() {
1. this.input = new PipedInputStream();
1. }
1.
1. private void receive() {
1.
1. byte[] b = new byte[1000];
1. int len = 0;
1. String msg = "";
1. try {
1. len = input.read(b);
1. msg = new String(b, 0, len);
1. if (msg.equals("start")) {
1. System.out
1. .println("received the start message, receive now can do something......");
1. Thread.interrupted();
1. }
1. } catch (Exception e) {
1. e.printStackTrace();
1. } finally {
1. try {
1. if (input != null) {
1. input.close();
1. input = null;
1. }
1. } catch (Exception e) {
1. }
1. }
1. }
1.
1. public void run() {
1. try {
1. receive();
1. } catch (Exception e) {
1. }
1. }
1. }

package org.sky.io;

import java.io.*;
public class ReceiveMessage implements Runnable {

private PipedInputStream input = null;
public PipedInputStream getInput() {

return this.input;
}

public ReceiveMessage() {
this.input = new PipedInputStream();

}
private void receive() {

byte[] b = new byte[1000];
int len = 0;

String msg = "";
try {

len = input.read(b);
msg = new String(b, 0, len);

if (msg.equals("start")) {
System.out

.println("received the start message, receive now can do something......");
Thread.interrupted();

}
} catch (Exception e) {

e.printStackTrace();
} finally {

try {
if (input != null) {

input.close();
input = null;

}
} catch (Exception e) {

}
}

}
public void run() {

try {
receive();

} catch (Exception e) {
}

}
}
如何使用这两个类呢？

## []()TestPipeStream

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. public class TestPipeStream {
1.
1. /**
1. * @param args
1. */
1. public static void main(String[] args) {
1. SendMessage send = new SendMessage();
1. ReceiveMessage receive = new ReceiveMessage();
1. try {
1. send.getOut().connect(receive.getInput());
1. Thread t1 = new Thread(send);
1. Thread t2 = new Thread(receive);
1. t1.start();
1. t2.start();
1.
1. } catch (Exception e) {
1. e.printStackTrace();
1. }
1.
1. }
1.
1. }

package org.sky.io;

public class TestPipeStream {
/**

* @param args
*/

public static void main(String[] args) {
SendMessage send = new SendMessage();

ReceiveMessage receive = new ReceiveMessage();
try {

send.getOut().connect(receive.getInput());
Thread t1 = new Thread(send);

Thread t2 = new Thread(receive);
t1.start();

t2.start();
} catch (Exception e) {

e.printStackTrace();
}

}
}
注意这边有一个send.getOut().connect(receive.getInput());

这个方法就把两个线程”connect“起来了。

# []()Serializable的IO操作

把一个类序列化到磁盘上，怎么做？

先来看我们要序列化的一个Java Bean

## []()Person

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. import java.io.Serializable;
1.
1. public class Person implements Serializable {
1.
1. private String name = "";
1. private String age = "";
1. private String personId = "";
1.
1. public String getName() {
1. return name;
1. }
1.
1. public void setName(String name) {
1. this.name = name;
1. }
1.
1. public String getAge() {
1. return age;
1. }
1.
1. public void setAge(String age) {
1. this.age = age;
1. }
1.
1. public String getPersonId() {
1. return personId;
1. }
1.
1. public void setPersonId(String personId) {
1. this.personId = personId;
1. }
1.
1. public String getCellPhoneNo() {
1. return cellPhoneNo;
1. }
1.
1. public void setCellPhoneNo(String cellPhoneNo) {
1. this.cellPhoneNo = cellPhoneNo;
1. }
1.
1. private String cellPhoneNo = "";
1. }

package org.sky.io;

import java.io.Serializable;
public class Person implements Serializable {

private String name = "";
private String age = "";

private String personId = "";
public String getName() {

return name;
}

public void setName(String name) {
this.name = name;

}
public String getAge() {

return age;
}

public void setAge(String age) {
this.age = age;

}
public String getPersonId() {

return personId;
}

public void setPersonId(String personId) {
this.personId = personId;

}
public String getCellPhoneNo() {

return cellPhoneNo;
}

public void setCellPhoneNo(String cellPhoneNo) {
this.cellPhoneNo = cellPhoneNo;

}
private String cellPhoneNo = "";

}
下面来看序列化的操作

## []()SerializablePersonToFile

**[java]** [view plain]( "view plain")[copy]( "copy")[print]( "print")[?]( "?")

1. package org.sky.io;
1.
1. import java.io.*;
1. import java.util.*;
1.
1. public class SerializablePersonToFile {
1.
1. /**
1. * @param args
1. */
1. private List<Person> initList() {
1. List<Person> userList = new ArrayList<Person>();
1. Person loginUser = new Person();
1. loginUser.setName("sam");
1. loginUser.setAge("30");
1. loginUser.setCellPhoneNo("13333333333");
1. loginUser.setPersonId("111111111111111111");
1. userList.add(loginUser);
1. loginUser = new Person();
1. loginUser.setName("tonny");
1. loginUser.setAge("31");
1. loginUser.setCellPhoneNo("14333333333");
1. loginUser.setPersonId("111111111111111111");
1. userList.add(loginUser);
1. loginUser = new Person();
1. loginUser.setName("jim");
1. loginUser.setAge("28");
1. loginUser.setCellPhoneNo("15333333333");
1. loginUser.setPersonId("111111111111111111");
1. userList.add(loginUser);
1. loginUser = new Person();
1. loginUser.setName("Simon");
1. loginUser.setAge("30");
1. loginUser.setCellPhoneNo("17333333333");
1. loginUser.setPersonId("111111111111111111");
1. userList.add(loginUser);
1. return userList;
1. }
1.
1. private void serializeFromFile() {
1. FileInputStream fs = null;
1. ObjectInputStream ois = null;
1. try {
1. fs = new FileInputStream("person.txt");
1. ois = new ObjectInputStream(fs);
1. List<Person> userList = (ArrayList<Person>) ois.readObject();
1. for (Person p : userList) {
1. System.out.println(p.getName() + " " + p.getAge() + " "
1. + p.getCellPhoneNo() + " " + p.getCellPhoneNo());
1. }
1. } catch (Exception ex) {
1. ex.printStackTrace();
1. } finally {
1. try {
1. if (ois != null) {
1. ois.close();
1. }
1. } catch (Exception e) {
1. }
1. try {
1. if (fs != null) {
1. fs.close();
1. }
1. } catch (Exception e) {
1. }
1. }
1. }
1.
1. private void serializeToFile() {
1. List<Person> userList = new ArrayList<Person>();
1. userList = initList();
1. FileOutputStream fs = null;
1. ObjectOutputStream os = null;
1. try {
1. fs = new FileOutputStream("person.txt");
1. os = new ObjectOutputStream(fs);
1. os.writeObject(userList);
1. } catch (Exception ex) {
1. ex.printStackTrace();
1. } finally {
1. try {
1. if (os != null) {
1. os.close();
1. }
1. } catch (Exception e) {
1. }
1. try {
1. if (fs != null) {
1. fs.close();
1. }
1. } catch (Exception e) {
1. }
1. }
1. }
1.
1. public static void main(String[] args) {
1. SerializablePersonToFile sf = new SerializablePersonToFile();
1. sf.serializeToFile();
1. sf.serializeFromFile();
1. }
1.
1. }

package org.sky.io;

import java.io.*;
import java.util.*;

public class SerializablePersonToFile {
/**

* @param args
*/

private List<Person> initList() {
List<Person> userList = new ArrayList<Person>();

Person loginUser = new Person();
loginUser.setName("sam");

loginUser.setAge("30");
loginUser.setCellPhoneNo("13333333333");

loginUser.setPersonId("111111111111111111");
userList.add(loginUser);

loginUser = new Person();
loginUser.setName("tonny");

loginUser.setAge("31");
loginUser.setCellPhoneNo("14333333333");

loginUser.setPersonId("111111111111111111");
userList.add(loginUser);

loginUser = new Person();
loginUser.setName("jim");

loginUser.setAge("28");
loginUser.setCellPhoneNo("15333333333");

loginUser.setPersonId("111111111111111111");
userList.add(loginUser);

loginUser = new Person();
loginUser.setName("Simon");

loginUser.setAge("30");
loginUser.setCellPhoneNo("17333333333");

loginUser.setPersonId("111111111111111111");
userList.add(loginUser);

return userList;
}

private void serializeFromFile() {
FileInputStream fs = null;

ObjectInputStream ois = null;
try {

fs = new FileInputStream("person.txt");
ois = new ObjectInputStream(fs);

List<Person> userList = (ArrayList<Person>) ois.readObject();
for (Person p : userList) {

System.out.println(p.getName() + " " + p.getAge() + " "
+ p.getCellPhoneNo() + " " + p.getCellPhoneNo());

}
} catch (Exception ex) {

ex.printStackTrace();
} finally {

try {
if (ois != null) {

ois.close();
}

} catch (Exception e) {
}

try {
if (fs != null) {

fs.close();
}

} catch (Exception e) {
}

}
}

private void serializeToFile() {
List<Person> userList = new ArrayList<Person>();

userList = initList();
FileOutputStream fs = null;

ObjectOutputStream os = null;
try {

fs = new FileOutputStream("person.txt");
os = new ObjectOutputStream(fs);

os.writeObject(userList);
} catch (Exception ex) {

ex.printStackTrace();
} finally {

try {
if (os != null) {

os.close();
}

} catch (Exception e) {
}

try {
if (fs != null) {

fs.close();
}

} catch (Exception e) {
}

}
}

public static void main(String[] args) {
SerializablePersonToFile sf = new SerializablePersonToFile();

sf.serializeToFile();
sf.serializeFromFile();

}
}
这边先把Person输出到Person.txt，再从Person.txt里反序列化出这个Person的Java Bean。

先讲这么些吧！

Java的流操作还有很多，这些是经常会被面试到的，很基础，因此经常被考到。

以前听一个读IT的同学说过，这些IO操作，就算没有Eclipse编辑器的话，用文本编辑器也应该能够写出来，你写不出只能代表你的基础太弱了。
分享到： []( "分享到新浪微博")[]( "分享到腾讯微博")

1. 上一篇：[think in java interview-高级开发人员面试宝典(七)](http://blog.csdn.net/lifetragedy/article/details/10305735)
顶11 踩1

查看评论[]()
15楼 [lifetragedy](http://blog.csdn.net/lifetragedy) 3小时前发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复u011487311：这边讲的是面试不是project or even not a product way，所以以功能为主！！！
一般工程中都是把这个函数返回的东西放到一个TreeTable！ 14楼 [LI597494570](http://blog.csdn.net/LI597494570) 3天前 10:10发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/LI597494570)今天 细看了下 发现 OutputStreamDemo 里面2个方法名和内容不一致撒 ；writeWithByteArray() { out.write(b[i]); } 和 writeWithByte() {() { out.write(b[]); } ，方法名应该互换下；顺便问下这两种方法的区别？ Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 3天前 11:08发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复LI597494570：这个留着是让读者自己感受的，因为我不可能在你背后手把手教你写！ 13楼 [wuming5920](http://blog.csdn.net/wm5920) 3天前 16:53发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/wm5920)一路走来 12楼 [zzjjiandan](http://blog.csdn.net/zzjjiandan) 3天前 14:25发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/zzjjiandan)加油，加油，加油，期待您出书啊，把面试宝典和架构师之路合成一本出书。 11楼 [袁先生](http://blog.csdn.net/u010582739) 4天前 00:18发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/u010582739)很有收获啊！谢谢！ 10楼 [starduliang](http://blog.csdn.net/starduliang) 4天前 19:02发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/starduliang)受益匪浅，原本还以为自己基础还不错，开了眼界，感谢，感谢 9楼 [谭cc来也](http://blog.csdn.net/u011487311) 5天前 12:56发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/u011487311)留着学习学习，我会经常关注你的！希望你能给我们初学者带来更多的学习机会！ 8楼 [qq467339640](http://blog.csdn.net/qq467339640) 5天前 09:29发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/qq467339640)博主，你的第一个代码输入中文有乱码的 Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 5天前 09:53发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复qq467339640：哦，第一个代码，你输入中文，有乱码，正常，你得“转码”，即你输入后的中文要显示必须new String(str,"UTF-8");，因为JAVA接受的流是ISO8859-1 Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 5天前 09:52发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复qq467339640：确认你的ECLISE的WORKSPACE的全局字符集是什么！！ Re: [qq467339640](http://blog.csdn.net/qq467339640) 4天前 12:53发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/qq467339640)回复lifetragedy：恩的确用了全局编码utf-8在run configrations里面改成gbk就没了 7楼 [hsun924](http://blog.csdn.net/hsun924) 5天前 09:15发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/hsun924)mark 6楼 [独行天下](http://blog.csdn.net/supersugar3126) 5天前 08:52发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/supersugar3126)持续关注中…… 5楼 [1986123](http://blog.csdn.net/u011836682) 5天前 01:59发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/u011836682)不错不错不错不错不错不错不错不错不错不错不错不错不错不错不错不错不错不错不错不错不错不错不错不错 4楼 [LI597494570](http://blog.csdn.net/LI597494570) 5天前 17:15发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/LI597494570)学习了 大神应该再详细点 什么时候用哪种文件读取方式什么的 加上场景 和你的分析，也让我们小白看看和你们的差别撒 Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 5天前 17:54发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复LI597494570：这个讲不没绝对的，这些是“拳招”，组合应用搞得是经验，所谓无招胜有招，碰到具体的CASE你可以问我这时用的是什么IO操作较好！ Re: [LI597494570](http://blog.csdn.net/LI597494570) 5天前 09:48发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/LI597494570)回复lifetragedy：我试了你InputFromConsole这个例子出错了
ERROR: JDWP Unable to get JNI 1.2 environment, jvm->GetEnv() return code = -2
JDWP exit error AGENT_ERROR_NO_JNI_ENV(183): [../../../src/share/back/util.c:820] Re: [lifetragedy](http://blog.csdn.net/lifetragedy) 5天前 09:52发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lifetragedy)回复LI597494570：把你机器里的JDK全删光，删干净了，注册表也清空了，重装一个JDK1.6吧 Re: [LI597494570](http://blog.csdn.net/LI597494570) 5天前 10:16发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/LI597494570)回复lifetragedy：据说是JDK1.6的Bug... 3楼 [lu0424liu](http://blog.csdn.net/lu0424liu) 6天前 13:06发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/lu0424liu)学习了！ 2楼 [鹳狸媛](http://blog.csdn.net/suannai0314) 6天前 10:32发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/suannai0314)您的文章已被推荐到CSDN首页，感谢您的分享。 1楼 [水哥709](http://blog.csdn.net/leon709) 6天前 09:38发表 [[回复]]( "回复") [[引用]]( "引用") [[举报]]( "举报") [![]()](http://blog.csdn.net/leon709)卖关子了，期待后续……

您还没有登录,请[[登录]]()或[[注册]](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Flifetragedy%2Farticle%2Fdetails%2F10363489)
* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场[]()[]()

[![TOP]()]( "回到顶部")
