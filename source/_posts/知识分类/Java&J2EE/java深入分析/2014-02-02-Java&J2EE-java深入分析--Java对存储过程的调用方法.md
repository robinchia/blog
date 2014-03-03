---
layout: post
title: "Java对存储过程的调用方法"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - java深入分析
--- 

# Java对存储过程的调用方法

一:Java如何实现对存储过程的调用:
   A:不带输出参数的
   ---------------不带输出参数的----------------------------------
create procedure getsum
@n int =0<--此处为参数-->
as
declare @sum int<--定义变量-->
declare @i int
set @sum=0
set @i=0
while @i<=@n begin
set @sum=@sum+@i
set @i=@i+1
end
print 'the sum is '+ltrim(rtrim(str(@sum)))

--------------在SQL中执行:--------------------
   exec getsum 100

------------在JAVA中调用:---------------------
   JAVA可以调用   但是在JAVA程序却不能去显示该存储过程的结果 因为上面的存储
   过程的参数类型int 传递方式是in(按值)方式
   import java.sql.*;
public class ProcedureTest 
{
public static void main(String args[]) throws Exception
{
   //加载驱动
   DriverManager.registerDriver(new sun.jdbc.odbc.JdbcOdbcDriver());
   //获得连接
   Connection conn=DriverManager.getConnection("jdbc:odbc:mydata","sa","");

         //创建存储过程的对象
         CallableStatement c=conn.prepareCall("{call getsum(?)}");
        
         //给存储过程的参数设置值
         c.setInt(1,100);   //将第一个参数的值设置成100
        
         //执行存储过程
         c.execute();
         conn.close();
}
}
   
   B:带输出参数的
     1:返回int
         -------------------------带输出参数的----------------
alter procedure getsum
@n int =0,
@result int output
as
declare @sum int
declare @i int
set @sum=0
set @i=0
while @i<=@n begin
set @sum=@sum+@i
set @i=@i+1
end
set @result=@sum
   -------------------在查询分析器中执行------------
   declare @myResult int
exec getsum 100,@myResult output
print @myResult

 

   ------------在JAVA中调用---------------------
import java.sql.*;
public class ProcedureTest 
{
public static void main(String args[]) throws Exception
{
   //加载驱动
   DriverManager.registerDriver(new sun.jdbc.odbc.JdbcOdbcDriver());
   //获得连接
   Connection conn=DriverManager.getConnection("jdbc:odbc:mydata","sa","");

         //创建存储过程的对象
         CallableStatement c=conn.prepareCall("{call getsum(?,?)}");
        
         //给存储过程的第一个参数设置值
         c.setInt(1,100);
        
         //注册存储过程的第二个参数
         c.registerOutParameter(2,java.sql.Types.INTEGER);
        
         //执行存储过程
         c.execute();
        
         //得到存储过程的输出参数值
         System.out.println (c.getInt(2));
         conn.close();
}
}
     2:返回varchar
       ----------------存储过程带游标----------------
---在存储过程中带游标   使用游标不停的遍历orderid
create procedure CursorIntoProcedure
@pname varchar(8000) output
as
--定义游标
declare cur cursor for select orderid from orders
--定义一个变量来接收游标的值
declare @v varchar(5)
--打开游标
open cur
set @pname=''--给@pname初值
--提取游标的值
fetch next from cur into @v
while @@fetch_status=0
   begin

set @pname=@pname+';'+@v
   fetch next from cur into @v
end
print @pname
--关闭游标
close cur
--销毁游标
deallocate cur

 

   ------------执行存储过程--------------
exec CursorIntoProcedure ''

   --------------JAVA调用------------------
import java.sql.*;
public class ProcedureTest 
{
public static void main(String args[]) throws Exception
{
   //加载驱动
   DriverManager.registerDriver(new sun.jdbc.odbc.JdbcOdbcDriver());
   //获得连接
   Connection conn=DriverManager.getConnection("jdbc:odbc:mydata","sa","");
   CallableStatement c=conn.prepareCall("{call CursorIntoProcedure(?)}");
  
  
   c.registerOutParameter(1,java.sql.Types.VARCHAR);
  
   c.execute();
  
   System.out.println (c.getString(1));
   conn.close();
}
}
   C:删除数据的存储过程
     ------------------存储过程--------------------------

drop table 学生基本信息表
create table 学生基本信息表
(
StuID int primary key,
StuName varchar(10),
StuAddress varchar(20)
)
insert into   学生基本信息表 values(1,'三毛','wuhan')
insert into   学生基本信息表 values(2,'三毛','wuhan')
create table 学生成绩表
(
StuID int,
Chinese int,
PyhSics int
foreign key(StuID) references   学生基本信息表(StuID)
on delete cascade
on update cascade
) 
insert into   学生成绩表 values(1,99,100)
insert into   学生成绩表 values(2,99,100)

--创建存储过程 
create procedure delePro
@StuID int
as
delete from 学生基本信息表 where StuID=@StuID
--创建完毕
exec delePro 1   --执行存储过程
--创建存储过程
create procedure selePro
as
select * from 学生基本信息表
--创建完毕
exec selePro   --执行存储过程
     ------------------在JAVA中调用----------------
import java.sql.*;
public class ProcedureTest 
{
public static void main(String args[]) throws Exception
{
   //加载驱动
   DriverManager.registerDriver(new sun.jdbc.odbc.JdbcOdbcDriver());
   //获得连接
   Connection conn=DriverManager.getConnection("jdbc:odbc:mydata","sa","");

         //创建存储过程的对象
         CallableStatement c=conn.prepareCall("{call delePro(?)}");
        
         c.setInt(1,1);
        
         c.execute();
        
         c=conn.prepareCall("{call selePro}");
         ResultSet rs=c.executeQuery();
        
         while(rs.next())
         {
         String Stu=rs.getString("StuID");
         String name=rs.getString("StuName");
         String add=rs.getString("StuAddress");
         
         System.out.println ("学号:"+"     "+"姓名:"+"     "+"地址");
         System.out.println (Stu+"     "+name+"   "+add);
         }
         c.close();
}
}
   D:修改数据的存储过程
---------------------创建存储过程---------------------
   create procedure ModPro
@StuID int,
@StuName varchar(10)
as
update 学生基本信息表 set StuName=@StuName where StuID=@StuID

 

   -------------执行存储过程-------------------------
exec ModPro 2,'四毛'
   ---------------JAVA调用存储过程--------------------
import java.sql.*;
public class ProcedureTest 
{
public static void main(String args[]) throws Exception
{
   //加载驱动
   DriverManager.registerDriver(new sun.jdbc.odbc.JdbcOdbcDriver());
   //获得连接
   Connection conn=DriverManager.getConnection("jdbc:odbc:mydata","sa","");

         //创建存储过程的对象
         CallableStatement c=conn.prepareCall("{call ModPro(?,?)}");
        
         c.setInt(1,2);
         c.setString(2,"美女");
                
         c.execute();
        
         c=conn.prepareCall("{call selePro}");
         ResultSet rs=c.executeQuery();
        
         while(rs.next())
         {
         String Stu=rs.getString("StuID");
         String name=rs.getString("StuName");
         String add=rs.getString("StuAddress");
         
         System.out.println ("学号:"+"     "+"姓名:"+"     "+"地址");
         System.out.println (Stu+"     "+name+"   "+add);
         }
         c.close();
}
}
   E:查询数据的存储过程(模糊查询)
     -----------------存储过程---------------------
create procedure FindCusts
@cust varchar(10)
as
select customerid from orders where customerid 
like '%'+@cust+'%'
     ---------------执行---------------------------
execute FindCusts 'alfki'
   -------------在JAVA中调用--------------------------
import java.sql.*;
public class ProcedureTest 
{
public static void main(String args[]) throws Exception
{
   //加载驱动
   DriverManager.registerDriver(new sun.jdbc.odbc.JdbcOdbcDriver());
   //获得连接
   Connection conn=DriverManager.getConnection("jdbc:odbc:mydata","sa","");

         //创建存储过程的对象
         CallableStatement c=conn.prepareCall("{call FindCusts(?)}");
         c.setString(1,"Tom");
        
         ResultSet rs=c.executeQuery();
        
         while(rs.next())
         {
         String cust=rs.getString("customerid");        
         System.out.println (cust);
         }
         c.close();
}
}
   F:增加数据的存储过程

------------存储过程--------------------
create procedure InsertPro
@StuID int,
@StuName varchar(10),
@StuAddress varchar(20)
as
insert into 学生基本信息表 values(@StuID,@StuName,@StuAddress)

-----------调用存储过程---------------
exec InsertPro 5,'555','555'
-----------在JAVA中执行-------------
import java.sql.*;
public class ProcedureTest 
{
public static void main(String args[]) throws Exception
{
   //加载驱动
   DriverManager.registerDriver(new sun.jdbc.odbc.JdbcOdbcDriver());
   //获得连接
   Connection conn=DriverManager.getConnection("jdbc:odbc:mydata","sa","");

         //创建存储过程的对象
         CallableStatement c=conn.prepareCall("{call InsertPro(?,?,?)}");
         c.setInt(1,6);
         c.setString(2,"Liu");
         c.setString(3,"wuhan");
        
         c.execute();
        
         c=conn.prepareCall("{call selePro}");
         ResultSet rs=c.executeQuery();
        
         while(rs.next())
         {
         String stuid=rs.getString("StuID");        
         String name=rs.getString("StuName");        
         String address=rs.getString("StuAddress");        
         System.out.println (stuid+"   "+name+"   "+address);
         }
         c.close();
}
}

G:在JAVA中创建存储过程   并且在JAVA中直接调用
import java.sql.*;
public class ProcedureTest 
{
public static void main(String args[]) throws Exception
{
   //加载驱动
   DriverManager.registerDriver(new sun.jdbc.odbc.JdbcOdbcDriver());
   //获得连接
   Connection conn=DriverManager.getConnection("jdbc:odbc:mydata","sa","");
  
  
   Statement stmt=conn.createStatement();
   //在JAVA中创建存储过程
   stmt.executeUpdate("create procedure OOP as select * from 学生成绩表");
  
  
   CallableStatement c=conn.prepareCall("{call OOP}");
  
   ResultSet rs=c.executeQuery();
   while(rs.next())
   {
   String chinese=rs.getString("Chinese");
   
   System.out.println (chinese);
   }
   conn.close();
  
}
} 
来源： <[http://technicalsearch.iteye.com/blog/1433293](http://technicalsearch.iteye.com/blog/1433293)> 
