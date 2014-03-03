---
layout: post
title: "删除oracle某用户所用表"
categories: DB
tags: 
 - DB
 - oracle
--- 

# 删除oracle某用户所用表

命令：drop user username CASCADE;

 
select 'drop table '||table_name||';' from user_tables;

 
分享这些经验只是为了给 曾经犯过 错误的同学一个 挽救的方法

    第一个问题:drop table,切记没有purge掉

    恢复的方法是:

    flashback table tableName to before drop [ rename to newTableName]

    解释:红字是关键字,[] 代表可选项

    第二个问题:delete from tableName

    恢复方法是:

    先查询scn(system change number)号:

    select dbms_flashback.get_system_change_number from dual;

    --需要用户有dbms_flashback.get_system_change_number 权限;

    根据查询的scn号,一点点往下减少,尝试那个号符合我们删除数据的时间点

    select * from tableName as of scn 13102359953430;

    --笔者的13102359953430 是尝试出来的,通过sql查询的值为13102359953436,我一点一点尝试 才知道是这个值
来源： <[http://oracle.chinaitlab.com/optimize/909867.html](http://oracle.chinaitlab.com/optimize/909867.html)> 

 
