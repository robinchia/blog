---
layout: post
title: "一个备份MySQL数据库的简单Shell脚本"
categories: DB
tags: 
 - DB
 - mysql
--- 

# 一个备份MySQL数据库的简单Shell脚本

本文翻译自 [iSystemAdmin](http://isystemadmin.com/) 的 《[A Simple Shell Script to Backup MySQL Database](http://isystemadmin.com/a-simple-shell-script-to-backup-mysql-database)》

Shell脚本是我们写不同类型命令的一种脚本，这些命令在这一个文件中就可以执行。我们也可以逐一敲入命令手动执行。如果我们要使用shell脚本就必须在一开始把这些命令写到一个文本文件中，以后就可以随意反复运行这些命令了。

我首先要在本文带给你的是完整脚本。后面会对该脚本做说明。我假定你已经知道**shell scripting、** **mysqldump和****crontab。**

适用操作系统：任何Linux或UNIX。

[![a-simple-shell-script-to-backup-mysql-database]( "a-simple-shell-script-to-backup-mysql-database")](http://cdn2.jobbole.com/2012/04/a-simple-shell-script-to-backup-mysql-database.jpg "a-simple-shell-script-to-backup-mysql-database")

**主脚本（用于备份mysql数据库）：**

该Shell脚本可以自动备份数据库。只要复制粘贴本脚本到文本编辑器中，输入数据库用户名、密码以及数据库名即可。我备份数据库使用的是**mysqlump** 命令。后面会对每行脚本命令进行说明。

**1. 分别建立目录“backup”和“oldbackup”**
1

2
 #mkdir /backup

#mkdir /oldbackup

**2. 现在使用你喜欢的编辑软件创建并编辑“backup.sh”**

这里我用的是 vi
1 # vi /backup/backup.sh

现在把以下几行命令输入到 backup.sh 文件中：

1

2
3

4
5

6
7

8
9 #!bin/bash

cd

/backup
echo

“You are In Backup Directory”

mv

backup*

/oldbackup
echo

“Old Databases are Moved to oldbackup folder”

Now=$(

date

+”%d-%m-%Y--%H:%M:%S”)
File=backup-$Now.sql

mysqldump –u user-name  –p ‘password’ database-name > $File
echo

“Your Database Backup Successfully Completed”

**脚本说明：**

切记，在第8行命令中，在mysqldump命令后要输入自己的数据库用户名、密码及数据库名。

执行该脚本，首先会进入 */backup* 目录，然后该脚本会把原有的旧数据库备份移动到 */oldbackup* 文件夹中，接着根据系统的日期及时间生成一个文件名，在最后 **mysqldump** 命令会生成一个“.sql”格式的数据库备份文件。

**3. 设置 backup.sh 脚本文件的可执行许可**
1 # chmod +x /backup/backup.sh

**4. 执行脚本**

1 #./backup.sh

脚本运行结束后会得到以下输入。

1

2
3

4
5 root@Server1:

/download

#./backup.sh

You areinDownload Directory
Old Backup DatabaseisMoved to oldbackup folder

database backup successful completed
root@Server1:

/download

#

注：首次执行该脚本会有一个“no such file”的提示信息，这是由于旧备份文件还不存在。只要再次执行该脚本就没有问题了，这个问题已经不存在了。

**5. 使用cron制订备份计划**

使用Cron可以定时执行该脚本，备份会自动完成。使用 **crontab** 命令编辑cron 执行的计划任务。
1 #crontab –e

只要在编辑器上加入下面这一行代码保存即可。

1 013* * * *

/backup/backup

.sh

本任务表示的是在每天下午1点钟把数据库备份到指定的文件夹。有关cron任务设置的详细内容可以查阅crontab手册。

对初学者而言，这是非常基础的脚本。希望你能举一反三写出更复杂的备份脚本。我们会努力提供更自动化的新脚本。请大家不吝赐教，我们会尽力解决你们的问题。感谢与我们相伴。
来源： <[http://blog.jobbole.com/18005/](http://blog.jobbole.com/18005/)> 

A Simple Shell Script to Backup MySQL DataBase
 

The Shell script is a script where we are writing different types of commands and executing those commands from a single file.  We can execute that command manually, by entering command one by one. But if we use shell script we have to write those commands into a text file for the first time and then we can run those commands as many times as required.

In this article first I will show you, Complete Script. Later on, you will get a description of that script.  I assumed that you know about **shell scripting**, **mysqldump** and **crontab**.

Operating System: Any Linux or UNIX.

**Main Script (for backup your [MySQL](http://resources.isystemadmin.com/mysql/ "MySQL Introduction") database):**

This shell script will make the backup process of a database automatic. Just copy and paste this script in a text editor, put database user name, password, and database name. I will use **mysqlump** command to backup the database. Later on you will get description of each line.
**1. Make a directory name “backup” and “oldbackup”**

1

2
 #mkdir /backup

#mkdir /oldbackup
**2. Now make file name “backup.sh” and edit with any editor you like**

I’m using vi here-

1 # vi /backup/backup.sh

Now write these lines into backup.sh file:

#!bin/bash

cd /backup
echo “You are In Backup Directory”

mv backup* /oldbackup
echo “Old Databases are Moved to oldbackup folder”

Now=$(date +”%d-%m-%Y--%H:%M:%S”)
File=backup-$Now.sql

mysqldump –u user-name  –p ‘password’ database-name > $File
echo “Your Database Backup Successfully Completed”

**Script Description:**

Remember, in line number 8: Put your Database username, Password, database name after mysqldump command.

When we run this script, first it goes to a */backup* directory. Then it will move old database backup files to */oldbackup* folder. Next it generates a file name from system date and time. And finally **mysqldump** command will generate a database backup file with “.sql” format
**3. Set permission to backup.sh file executable**

1 # chmod +x /backup/backup.sh
**4. Running the Script**

1 #./backup.sh

You will get following output after executing the script.
root@Server1:/download#./backup.sh

You are in Download Directory
Old Backup Database is Moved to oldbackup folder

database backup successful completed
root@Server1:/download#

NB: first time when you run this script you will get a message “no such file”, because you don’t have old backup file. No problem just runs again this script, problem will be solved.

**5. Schedule the Backup using cron**

Using Cron job you can execute this script in a certain time, and all works will be done automatically. Use **crontab** command to edit editing cron schedules.

#crontab –e

Just add below line in the editor and save it.
0 13 * * * * /backup/backup.sh

In this way every day 1PM your database will back up in your desired folder. Please visit crontab manuals for more details about setting the cron jobs.

This is a very basic script for the beginners. Hope you can use the same idea for more complex backup. We will try to come up with new scripts to automate further. Please ask any question you have. We will try our best to address your questions. Thanks for staying with us.

**No Related Posts**

###

Sifat is a veteran System Administrator who still loves to make his hand dirty with text consoles. Sifat has 14 years of Operations and Management experience in IT and telecommunication industries.He has proven record of IT planning, Policy Development, IT Process Management, Cost Control and successful leadership for effective and efficient IT organization. He is effective in reducing Capital and Operation Expense by H/W & Software consolidation and virtualization. Sifat is a certified ITIL and VMWare Professional.

[View all posts by Sifat →](http://isystemadmin.com/author/sifat)

来源： <[http://isystemadmin.com/a-simple-shell-script-to-backup-mysql-database](http://isystemadmin.com/a-simple-shell-script-to-backup-mysql-database)>
 
 
