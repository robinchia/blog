---
layout: post
title: "linux 命令 字符集"
categories: linux
tags: 
 - linux
--- 

# linux 命令 字符集

 locale -a 查看本地字符集
 locale -m 查看所有支持的字符集
  echo $LANG

用export LANG=zh_CN.UTF-8这样只下次重起又要重设置

修改 /etc/sysconfig/i18n 文件，如 
LANG="en_US"，xwindow会显示英文界面， 
LANG="zh_CN.GB18030"，xwindow会显示中文界面。

编辑/etc/sysconfig/i18n这个文件，
LANG="zh_CN.GB18030"
SUPPORTED="zh_CN.GB18030:zh_CN:zh:en_US.UTF-8:en_US:en"
SYSFONT="latarcyrheb-sun16"
保存,重起.OK了
注:
I18N 是 internationalization 的缩写形式，意即在 i 和 n 之间有 18 个字母，本意是指软件的“国际化”.
I18N支持多种语言，但是同一时间只能是英文和一种选定的语言，例如英文+中文、英文+德文、英文+韩文等等；
原来的:
LANG="zh_CN.UTF-8"
SUPPORTED="zh_CN.UTF-8:zh_CN:zh"
SYSFONT="latarcyrheb-sun16"
来源： <[http://www.cnitblog.com/windone0109/archive/2008/04/28/42901.html](http://www.cnitblog.com/windone0109/archive/2008/04/28/42901.html)> 
