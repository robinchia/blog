---
layout: post
title: "MySQL 运维笔记（一）—— 终止高负载SQL"
categories: DB
tags: 
 - DB
 - mysql
--- 

# MySQL 运维笔记（一）—— 终止高负载SQL

数据库表体积大了，负载高了，难免一个sql出去耗时延长。半个月前，一个凌晨定时任务跑了8小时，突然手足无措。最后找DBA协助，直接干掉了这个sql进程。
其实，这并不复杂。
首先，找出占用CPU时间过长的SQL
Sql代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. show processlist;  

show processlist;

![]()
 假定最后一条sql处于Query状态，且Time时间过长，就锁定它的ID，直接干掉即可。

 

然后，杀死进程：

Sql代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. kill QUERY  4487855;  

kill QUERY  4487855;

 这就大功告成了！

 

 参考

KILL [CONNECTION | QUERY] thread_id
 
