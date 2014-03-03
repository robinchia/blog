---
layout: post
title: "设计文档模板 - 我的文章 - 崔超的思想备忘录"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_设计类
--- 

# 设计文档模板 - 我的文章 - 崔超的思想备忘录

  [博客首页](http://blog.chinaunix.net/) [注册](http://blog.chinaunix.net/register.php) [建议与交流](http://bbs.chinaunix.net/forumdisplay.php?fid=51) [排行榜](http://blog.chinaunix.net/top/) [加入友情链接](http://blog.chinaunix.net/u/31756/)  ![]() [推荐](http://blog.chinaunix.net/u2/star.php?blogid=31756 "给此博客推荐值") [投诉](http://blog.chinaunix.net/u2/complaint.php?blogid=31756 "投诉此博客") 搜索：  [帮助](http://blog.chinaunix.net/help/)  **崔超的思想备忘录**

 思想有重量吗？    [cuichaox.cublog.cn](http://cuichaox.cublog.cn/) 
* [管理博客](http://control.cublog.cn/)
* [发表文章](http://control.cublog.cn/article_new.php)
* [留言](http://blog.chinaunix.net/u/31756/guestbook.html)
* [收藏夹](http://blog.chinaunix.net/u/31756/links.html)
* [博客圈](http://blog.chinaunix.net/u/31756/group.html)
* [音乐](http://blog.chinaunix.net/u/31756/music.html)
* [相册](http://blog.chinaunix.net/u/31756/photo.html)

* [· 软件图的艺术](http://blog.chinaunix.net/u/31756/photo_6788.html)
* [文章](http://blog.chinaunix.net/u/31756/article.html)

* [· 设计模式](http://blog.chinaunix.net/u/31756/article_48992.html)
* [首页](http://blog.chinaunix.net/u/31756/index.html)  **关于作者** ![]( "收起")   [![]()](http://blog.chinaunix.net/u/31756/up_user.gif) 只有原创和翻译 cuichaox@gmail.com
**我的分类** ![]( "收起") 
[![]()](http://blog.chinaunix.net/u/rss.php?id=31756)
  **设计文档模板**设计文档模板。 对今后思考“如何更好的编写设计文档"有一个的从出发点

xx系统,xx模块

设计文档v0.1

部门：xx

作者：崔超<[cuichao@boco.com.cn](mailto:cuichao@boco.com.cn)>

版权说明: ××拥有本文档的全部版权，没有经过明确的书面说明，任何人不能复制，转载本文档的所有内容.

可以把上面的内容放在一个好看的封面页上.

文档更新记录
版本
 
说明
 
完成日期
 
修改人 0.1
 
创建文档
 
 
崔超

# 1.概述

## 1.1.术语表

编号
 
缩写
 
全写
 
定义 1 2

## 1.3引用文档

编号
 
文档名称
 
作者
 
说明 1 2

## **1.4****文档说明**

本设计文档作为XX模块的设计文档，为编码的依据。也作为代码的说明，在代码开发过程中应该保持本文档的更新。

## 1.5项目背景

对项目的背景进行介绍

## **1.3****系统说明**

对整个系统的情况进行介绍。列举参考的文档

## **1.4****模块说明**

从功能角度模块进行简单的说明

**2.****工具和方法论**

对使用的设计方法，关键的概念和工具进行说明。比如：是否使用面向对象的方法，组件技术？是否使用到某种开发框架？最终使用哪种编码语言?

**3.****分析**

## **3.1.****本模块在整个系统中的地位**

最好使用结构图的形式并补充详细的说明。

## **3.2.****功能说明**

使用列表或描述的形式对功能进行详细的说明。对强调数据处理类型的模块，这里可以使用数据流图进行说明。对强调交互过程的模块，可以使用用例图进行说明。

**3.3.****非功能需求**

性能的要求，从异常中恢复的要求等非功能的要求.

**4.****概要设计**

**4.1****外部接口**

### 4.1.1接口要求

对外部接口进行精确的描述。如果更上层的文档已经有了这个模块的接口要求，对文档进行引用.

### 4.1.2界面设计

如果存在界面设计

### 4.1.3配置文件

如果这个模块有自己的配置文件

**4.2****模块划分**

使用软件结构图（结构化设计）。或使用包图，类图(面向对象设计)作为说明的主体。并且补充详细的文字说明.关键要说明每个子模块的（或类）的功能和职责。（对面向对象的设计，应该更多使用职责这样的词汇）。如果结构明显的呈现出层次，要对每个层次进行说明。

**5.****详细设计**

对每个模块，或者每一个类，进行详细的说明.说明内容包括(1)接口定义 (2)关键算法。可能使用到，类图，流程图，顺序图，交互图，等.如果是多线程程序，还可能堆同步模型进行说明。

**5.1****子模块****/****类****1**

... ...

**5.2****子模块****/****类****2**

... ...

**6.****测试考虑**

这节内容主要提供给测试人员使用，主要说明测试的重点。

**7.****总结**

开发完成后，自己有什么体会，对近一步的改进有什么好的想法，好好的写一下总结。对软件今后改进，扩展.对自己进步都有好处.

update 2007-3-16 修改错字  发表于： 2007-03-15，修改于： 2007-03-16 13:41 已浏览4413次，有评论1条 [推荐](http://blog.chinaunix.net/u2/star.php?blogid=31756&artid=259400 "推荐这篇文章") [投诉](http://blog.chinaunix.net/u2/complaint.php?blogid=31756&artid=259400 "投诉这篇文章")
  **网友评论**  **本站网友** 时间：2009-04-10 19:56:31 IP地址：59.108.199.★  **** 很好,很强大,很个性
  **发表评论** Copyright © 2001-2010 ChinaUnix.net All Rights Reserved

感谢所有关心和支持过ChinaUnix的朋友们
页面生成时间：0.014

[京ICP证041476号](http://www.miibeian.gov.cn/)
