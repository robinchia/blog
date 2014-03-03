---
layout: post
title: "Oracle分割字符串返回数组"
categories: DB
tags: 
 - DB
 - oracle
 - 函数
--- 

# Oracle分割字符串返回数组

**[[Oracle]分割字符串返回数组](http://blog.csdn.net/believefym/article/details/3791122 " [Oracle]分割字符串返回数组")******

 

 

[view plain](http://blog.csdn.net/believefym/article/details/3791122 "view plain")[copy to clipboard](http://blog.csdn.net/believefym/article/details/3791122 "copy to clipboard")[print](http://blog.csdn.net/believefym/article/details/3791122 "print")[?](http://blog.csdn.net/believefym/article/details/3791122 "?")

1.  CREATE OR REPLACE TYPE mytable AS TABLE OF varchar2(100)  

2.  /  

[view plain](http://blog.csdn.net/believefym/article/details/3791122 "view plain")[copy to clipboard](http://blog.csdn.net/believefym/article/details/3791122 "copy to clipboard")[print](http://blog.csdn.net/believefym/article/details/3791122 "print")[?](http://blog.csdn.net/believefym/article/details/3791122 "?")

1.  CREATE OR REPLACE FUNCTION split  

2.     (src VARCHAR2, delimiter varchar2)  

3.    RETURN mytable IS  

4.    psrc VARCHAR2(500);  

5.    a mytable := mytable();  

6.    i NUMBER := 1;  --  

7.    j NUMBER := 1;  

8.  BEGIN  

9.    psrc := RTrim(LTrim(src, delimiter), delimiter);  

10.   LOOP  

11.     i := InStr(psrc, delimiter, j);  

12.     --Dbms_Output.put_line(i);  

13.     IF i>0 THEN  

14.       a.extend;  

15.       a(a.Count) := Trim(SubStr(psrc, j, i-j));  

16.       j := i+1;  

17.       --Dbms_Output.put_line(a(a.Count-1));  

18.     END IF;  

19.     EXIT WHEN i=0;  

20.   END LOOP;  

21.   IF j < Length(psrc) THEN  

22.     a.extend;  

23.     a(a.Count) := Trim(SubStr(psrc, j, Length(psrc)+1-j));  

24.   END IF;  

25.   RETURN a;  

26. END;  

27. /  

CREATE OR REPLACE FUNCTION split (src VARCHAR2, delimiter varchar2) RETURN mytable IS psrc VARCHAR2(500); a mytable := mytable(); i NUMBER := 1; -- j NUMBER := 1; BEGIN psrc := RTrim(LTrim(src, delimiter), delimiter); LOOP i := InStr(psrc, delimiter, j); --Dbms_Output.put_line(i); IF i>0 THEN a.extend; a(a.Count) := Trim(SubStr(psrc, j, i-j)); j := i+1; --Dbms_Output.put_line(a(a.Count-1)); END IF; EXIT WHEN i=0; END LOOP; IF j < Length(psrc) THEN a.extend; a(a.Count) := Trim(SubStr(psrc, j, Length(psrc)+1-j)); END IF; RETURN a; END; /

 

数组作为select in的查询条件

[view plain](http://blog.csdn.net/believefym/article/details/3791122 "view plain")[copy to clipboard](http://blog.csdn.net/believefym/article/details/3791122 "copy to clipboard")[print](http://blog.csdn.net/believefym/article/details/3791122 "print")[?](http://blog.csdn.net/believefym/article/details/3791122 "?")

1.  SELECT * FROM student WHERE id IN (SELECT * FROM TABLE(CAST(split('001,002', ',')AS mytable)));  

SELECT * FROM student WHERE id IN (SELECT * FROM TABLE(CAST(split('001,002', ',')AS mytable)));

 

[view plain](http://blog.csdn.net/believefym/article/details/3791122 "view plain")[copy to clipboard](http://blog.csdn.net/believefym/article/details/3791122 "copy to clipboard")[print](http://blog.csdn.net/believefym/article/details/3791122 "print")[?](http://blog.csdn.net/believefym/article/details/3791122 "?")

1.  SELECT * FROM student WHERE id IN  

2.  (  

3.  SELECT id FROM student WHERE id='001'  

4.  UNION  

5.  SELECT * FROM TABLE(CAST(split('001,002',',') AS mytable))  

6.  );   

SELECT * FROM student WHERE id IN ( SELECT id FROM student WHERE id='001' UNION SELECT * FROM TABLE(CAST(split('001,002',',') AS mytable)) );

 

 
