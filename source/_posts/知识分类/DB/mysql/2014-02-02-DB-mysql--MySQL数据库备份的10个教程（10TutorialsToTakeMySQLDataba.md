---
layout: post
title: "MySQL数据库备份的10个教程（10 Tutorials To Take MySQL Databa"
categories: DB
tags: 
 - DB
 - mysql
--- 

# MySQL数据库备份的10个教程（10 Tutorials To Take MySQL Database Backup）

译文如下：

 
MySQL 是用于交互式网站开发的最为知名的开源数据库系统。如果你使用的 MySQL 数据库，你应当经常有规律地备份数据，以防数据丢失（译注：不管用什么类型的数据都得定期备份）。这里有10个自动或手动备份 MySQL 数据库的方法，应该有适合你的方法。

**1.  [Backing Up Using MySQLDump](http://www.sitepoint.com/backing-up-mysqldump/) **

****数据备份，可以使用 MySQL 自带的  MySQLDump 命令来完成。这篇文章给出了多种例子，包括把数据库备份成一个文件，备份到另外一个服务器，还有备份成一个gzip压缩文件。

**2. [MySQL Export: How to Backup Your MySQL Database?](http://www.php-mysql-tutorial.com/wikis/mysql-tutorials/using-php-to-backup-mysql-databases.aspx)**

录数据库，可以通过生成一个 dump 文件来备份数据库。这种方法的前提是，服务器上必须有 phpMyAdmin 工具。

**3. [Automatically Backup Mysql Database to Amazon S3](http://www.theblog.ca/mysql-email-backup)**

也可以使用Amazon  S3云存储服务来备份数据库。这篇文章中有一个自动脚本，它可以自动备份数据库，并转移至Amazon S3系统。

**4. [How to Backup MySQL Databases, Web Server Files to an FTP Server Automatically](http://www.cyberciti.biz/tips/how-to-backup-mysql-databases-web-server-files-to-a-ftp-server-automatically.html)**

如果你有自己的Web服务器或VPS，这里有一个简单方法：使用 FTP 或 NAS备份。首先你需要用 mysqldump 命令备份每个单独数据库，然后写一个脚本，用于 tar 打包，设置 cron ，并创建  FTP 备份。

[![MySQL数据库备份的10个教程]( "MySQL数据库备份的10个教程")](http://cdn2.jobbole.com/2012/03/mysql.jpg "MySQL数据库备份的10个教程")

 

**5. [How to E-Mail Yourself an Automatic Backup of Your MySQL Database Table with PHP](http://www.theblog.ca/mysql-email-backup)**

这个方法可以帮助你轻松备份特定的数据表，给你发送一封附有. sql 文件的邮件。 你可以创建一个特殊的邮箱l账号来接收备份文件。

**6. [How to Backup MySQL Database Using PHP](http://www.php-mysql-tutorial.com/wikis/mysql-tutorials/using-php-to-backup-mysql-databases.aspx)**

至少分三步：① 在 PHP 文件中执行数据库备份语句；② 在 system()函数中执行 mysqldump 命令；③ 用 phpMyAdmin 做备份

**7. [Backup Your Database Into an XML File By Using PHP](http://davidwalsh.name/backup-database-xml-php)**

这个方法使用一段PHP代码片段，以XML格式输出备数据库。虽然 XML 文件不是还原数据表的最便捷格式，但便于读取。

**8. [Backup MySQL Database Through SSH](http://www.blogthority.com/87/how-to-backup-mysql-database-without-phpmyadmin/)**

没有 phpMyAdmin 工具也可以备份数据库，SSH可用于备份较大的数据。必须在 cPanel 或 Plesk 控制面板中开启 shell 访问权，然后使用一个诸如 PuTTY 之类的工具远程登录服务器。

**9. [How to Backup MySQL Database Automatically (For Linux Users)](http://www.backuphowto.info/how-backup-mysql-database-automatically-linux-users)**

如果你是 Linux 用户，你可以用 cron 自动备份 MySQL 数据库。cron 是 Unix/Linux 系统下的一个定时执行工具。

**10. [Ubuntu Linux Backup MySQL Server Shell Script](http://www.cyberciti.biz/faq/ubuntu-linux-mysql-nas-ftp-backup-script/)**

如果你的VPS 操作系统是 Ubuntu 系统，那你可以把整个MySQL服务器数据库备份到FTP服务器中。
来源： <[http://blog.jobbole.com/14012/](http://blog.jobbole.com/14012/)> 原文：

MySQL is the most famous open source database management system for the development of interactive Websites. If you use MySQL Databases in your websites then you should always make backup of your data regularly to recover it in case of loss.

Here are 10 methods to automatically or manually backup MySQL databases. Check them out and pick the one which is best suited to you.
[![Backing Up MySQL Database]( "Backing Up MySQL Database")](http://smashinghub.com/wp-content/uploads/2011/08/Backing-Up-MySQL-Database.jpg)

Advertisement

**1.  [Backing Up Using MySQLDump](http://www.sitepoint.com/backing-up-mysqldump/)**
Backup of data can be made using mysqldump utility that comes with MySQL. Various examples are given using mysqldump, including the backing up your database to a file, another server, and even a compressed gzip file.

**2. [MySQL Export: How to Backup Your MySQL Database?](http://www.php-mysql-tutorial.com/wikis/mysql-tutorials/using-php-to-backup-mysql-databases.aspx)**

You can backup by making a dump file (export / backup) of a database used by your account. To do so you have to head over to phpMyAdmin tool in your cPanel.

**3. [Automatically Backup Mysql Database to Amazon S3](http://www.theblog.ca/mysql-email-backup)**

You can also use Amazon S3 to backup your mysql databases. An automated script is present here, which takes backup of a mysql database and then moves it to Amazon S3.

**4. [How to Backup MySQL Databases, Web Server Files to an FTP Server Automatically](http://www.cyberciti.biz/tips/how-to-backup-mysql-databases-web-server-files-to-a-ftp-server-automatically.html)**

It is an easy way to backup data for users who run their own web server and MySQL server on a dedicated box or VPS. The best thing when using FTP or NAS backup is the fact that your data is secure. First of all you have to backup every single database with mysqldump command, Automating tasks of backup with tar, Setup a cron job and create FTP backup script.

**5. [How to E-Mail Yourself an Automatic Backup of Your MySQL Database Table with PHP](http://www.theblog.ca/mysql-email-backup)**

It will deliver an e-mail to you with an .sql file attached, which lets you to back up particular tables easily. You can even create an e-mail account specifically to get these backup.

**6. [How to Backup MySQL Database Using PHP](http://www.php-mysql-tutorial.com/wikis/mysql-tutorials/using-php-to-backup-mysql-databases.aspx)**

Carry out a database backup query from PHP file. In order to restore the backup all you have to do is to run LOAD DATA INFILE query.

**7. [Backup Your Database Into an XML File By Using PHP](http://davidwalsh.name/backup-database-xml-php)**

It will display a PHP snippet that outputs your database as XML. XML is not one of the easiest format to restore a table but it is definitely easier to read.

**8. [Backup MySQL Database Through SSH](http://www.blogthority.com/87/how-to-backup-mysql-database-without-phpmyadmin/)**

SSH can be used to backup large MySQL data. You will have to enable shell access in your cPanel or Plesk control panel and utilize a tool like PuTTY to log into your server through SSH.

**9. [How to Backup MySQL Database Automatically (For Linux Users)](http://www.backuphowto.info/how-backup-mysql-database-automatically-linux-users)**

If you are a Linux user you can backup MySQL Database automatically by using cron. “cron” is a time-based scheduling utility in Unix/Linux OS.

**10. [Ubuntu Linux Backup MySQL Server Shell Script](http://www.cyberciti.biz/faq/ubuntu-linux-mysql-nas-ftp-backup-script/)**

You can backup all your MySQL server databases to your ftp server, if you have a dedicated VPS server with Ubuntu Linux.

You are welcome to share any more methods in comments below.
来源： <[http://smashinghub.com/10-tutorials-to-take-mysql-database-backup.htm](http://smashinghub.com/10-tutorials-to-take-mysql-database-backup.htm)> 
