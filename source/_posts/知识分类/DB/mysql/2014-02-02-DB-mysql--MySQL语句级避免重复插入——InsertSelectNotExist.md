---
layout: post
title: "MySQL 语句级避免重复插入—— Insert Select Not Exist"
categories: DB
tags: 
 - DB
 - mysql
--- 

# MySQL 语句级避免重复插入—— Insert Select Not Exist

想要插入一条数据，要避免重复插入，又不想折腾两回数据库连接操作，可以参考如下办法。![]()
Sql代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. INSERT INTO table(column1,column2,column3 ...columnN)  
1. SELECT value1,value2,value3 ...valueN  
1. FROM dual  
1. WHERE NOT EXISTS(  
1.       SELECT *  
1.       FROM table  
1.       WHERE value = ?  
1. );  

INSERT INTO table(column1,column2,column3 ...columnN)

SELECT value1,value2,value3 ...valueN
FROM dual

WHERE NOT EXISTS(
      SELECT *

      FROM table
      WHERE value = ?

);
**dual**是为了构建查询语句而存在的表,Oracle中很常见,配合**INSERT ... SELECT**构建成我们需要的表,并指定了数据项.
**EXISTS**通过这个判断是否存在的函数,就免去了我们做IF-ELSE的冗繁操作.![]()
例:
Sql代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. INSERT INTO content (  
1.     detail,  
1.     status,  
1.     beginTime,  
1.     endTime)   
1. SELECT  
1.     @detail,  
1.     1,  
1.     NULL,  
1.     NULL  
1. FROM DUAL  
1.     WHERE NOT EXISTS(  
1.         SELECT contentId   
1.         FROM content   
1.         WHERE detail=@detail);  

            INSERT INTO content (

                detail,
                status,

                beginTime,
                endTime)

            SELECT
                @detail,

                1,
                NULL,

                NULL
            FROM DUAL

             WHERE NOT EXISTS(
                 SELECT contentId

                 FROM content
                 WHERE detail=@detail);
**@detail**是要存入的内容，这里对内容进行了检索，如果要这么做，最好对该字段做唯一约束，或加索引。![]()
省掉了IF-ELSE，在iBatis配置一下就ok了，哈！![]()
还有个更坚决的办法——replace into：
Sql代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. replace into blacklist(userInfoId,uid)  
1. select userInfoId,uid from user_info u where uid in(  
1. 'u303565440','u303566922','u303515112','u303559738');  

replace into blacklist(userInfoId,uid)

select userInfoId,uid from user_info u where uid in(
'u303565440','u303566922','u303515112','u303559738');
