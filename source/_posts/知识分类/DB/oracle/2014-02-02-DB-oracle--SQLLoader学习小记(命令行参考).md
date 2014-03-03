---
layout: post
title: "SQL Loader学习小记(命令行参考)"
categories: DB
tags: 
 - DB
 - oracle
--- 

# SQL Loader学习小记(命令行参考)

SQL Loader 命令行参考
   Command line Sample:
   SQLLDR CONTROL=sample.ctl, LOG=sample.log, BAD=baz.bad, DATA=etc.dat
   USERID=scott/tiger, ERRORS=999, LOAD=2000, DISCARD=toss.dsc,
   DISCARDMAX=5
   常用的keywords:
userid     -- orACLE username/password
control    -- control file name
log        -- log file name
bad        -- bad file name
data       -- data file name
discard    -- discard file name
discardmax -- number of discards to allow (Default all)
skip       -- number of logical records to skip (Default 0)
load       -- number of logical records to load (Default all)
errors     -- number of errors to allow (Default 50)(允许出错的记录数)
rows       -- number of rows in conventional path bind array or between direct path data saves
               (Default: Conventional path 64, Direct path all)(每次提交的记录数)
bindsize   -- size of conventional path bind array in bytes (Default 256000)
silent     -- suppress messages during run (header,feedback,errors,discards,partitions)(是否显示load信息)
direct     -- use direct path (Default FALSE)
parfile    -- parameter file: name of file that contains parameter specifications
parallel   -- do parallel load (Default FALSE)
file       -- file to allocate extents from
skip_unusable_indexes  -- disallow/allow unusable indexes or index partitions(Default FALSE)
skip_index_maintenance -- do not maintain indexes, mark affected indexes as unusable (Default FALSE)
commit_discontinued    -- commit loaded rows when load is discontinued (Default FALSE)
readsize           -- size of read buffer (Default 1048576)
external_table     -- use external table for load; NOT_USED, GENERATE_ONLY, EXECUTE (Default NOT_USED)
columnarrayrows    -- number of rows for direct path column array (Default 5000)
streamsize         -- size of direct path stream buffer in bytes (Default 256000)
multithreading     -- use multithreading in direct path
resumable          -- enable or disable resumable for current session (Default FALSE)
resumable_name     -- text string to help identify resumable statement
resumable_timeout  -- wait time (in seconds) for RESUMABLE (Default 7200)
date_cache         -- size (in entries) of date conversion cache (Default 1000)
    上述大部分的参数定义可以入在control file中的option里。
    执行结果返回代码
All rows loaded successfully      EX_SUCC
All or some rows rejected      EX_WARN
All or some rows discarded      EX_WARN
Discontinued load EX_WARN
Command-line or syntax errors      EX_FAIL
oracle errors nonrecoverable for SQL*Loader    EX_FAIL
Operating system errors (such as file open/close and malloc)  EX_FAIL
    For UNIX, the exit codes are as follows:
EX_SUCC 0
EX_FAIL 1
EX_WARN 2
EX_FTL  3

来源： <[http://www.wudongqi.com/article/540.htm](http://www.wudongqi.com/article/540.htm)>
 
