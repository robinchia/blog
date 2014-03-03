---
layout: post
title: "用BCV备份Oracle数据库"
categories: DB
tags: 
 - DB
 - oracle
 - oracle-
--- 

# 用BCV备份Oracle数据库

用BCV备份Oracle数据库
BCV（业务连续卷）是Business Continuance Volume的简写，EMC的产品TimeFinder的理念。可用于海量数据库的备份和测试库的建立等。
  对海量数据进行备份的挑战之一就是对产品库系统的影响。如果采用传统方法，其备份周期，数据库文件处于热备份的时间会很长，备份期间会严重影响系统性能。
  而BCV备份采用磁盘同步，磁盘分割更底层的方式，使数据库热备份在几分钟就可以完成。
  采用BCV备份的步骤如下
  1）Resynch BCVs with Production
  2）Put Production Databases in Hot Backup Mode
  3）Split BCV
  4）Take Production Databases out of Hot Backup Mode
  分割完毕后的BCVs，可以将数据备份到磁带上，再与Production同步。也可以挂载在其他主机上进行恢复，建立测试数据库。
  公司中多个TB级的数据库备份的第2到第4步，都在15分钟以内完成。
