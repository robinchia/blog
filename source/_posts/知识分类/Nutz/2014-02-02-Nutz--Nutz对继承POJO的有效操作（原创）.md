---
layout: post
title: "Nutz 对继承POJO的有效操作（原创）"
categories: Nutz
tags: 
 - Nutz
--- 

# Nutz 对继承POJO的有效操作（原创）

Nutz对POJO直接操作数据库是有效的，同时对继承类（子类）的操作，一定程序上是有效的。

如下：
建立POJO类testt：

      

import org.nutz.dao.entity.annotation.Column;
import org.nutz.dao.entity.annotation.Id;

import org.nutz.dao.entity.annotation.Table;
import org.nutz.dao.impl.NutDao;

@Table("venus_testt")
public class testt {

    @Id
    private int id;

    @Column("name")
    private String name;

    @Column
    private String test;

    
}
 

子类test2：
import org.nutz.dao.entity.annotation.Column;

public class test2 extends testt {
    @Column

    private String haha;
}

 
测试父类，建立表成功。

        Dao dao = ioc.get(Dao.class, "dao");
        dao.create(testt.class, false);

Table: venus_test

**字段信息**
Field Type Comment id int(32) name varchar(50) test varchar(50)
测试子类，同样也成功：

           Dao dao = ioc.get(Dao.class, "dao");       
         dao.create(test2.class, false);

Table: venus_test

**字段信息**
Field Type Comment id int(32) haha varchar(50) name varchar(50) test varchar(50)
如果将子类指定表名：

@Table("venus_t_test")
则同样成功：

![]()
 Table: venus_t_test

**字段信息**
Field Type Comment id int(32) haha varchar(50) name varchar(50) test varchar(50)

当需要对表进行精确控制时，可采用@ColDefine
如果，test2可修改如下：

import org.nutz.dao.entity.annotation.ColDefine;

import org.nutz.dao.entity.annotation.ColType;
import org.nutz.dao.entity.annotation.Column;

import org.nutz.dao.entity.annotation.Table;
@Table("venus_t_test")

public class test2 extends testt {
    @Column

    @ColDefine(type=ColType.VARCHAR,width=128,notNull=true)
    private String haha; }
 

测试后，结果如下：
![]()
**** 
