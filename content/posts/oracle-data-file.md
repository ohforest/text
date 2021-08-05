---
title: "Oracle Data File"
date: 2021-08-03T11:54:52+08:00
draft: false
summary: "Oracle Data File"
categories:
  - "Oracle"
tags:
  - "data file"
---

在ORACLE中，数据库是由实例和物理存储结构组成的，物理存储结构是指存储在磁盘上的物理文件，包括数据文件(data file)、控制文件(control file)、重做日志(online redo log)、参数文件(spfile/pfile)、警告日志(alert log)、跟踪文件(trace file)等众多作用不同的文件组成

# Bigfile Tablespaces

使用32位来存储block号，大小为(2^32-1)*32K = 128T(Oracle最大支持block为32k)

## 创建

```
CREATE BIGFILE TABLESPACE bigtbs 
    DATAFILE '/u02/oracle/data/bigtbs01.dbf' SIZE 50G
```

```
ALERT BIGFILE TABLESPACE bigtbs 
    ADD DATAFILE '/u02/oracle/data/bigtbs01.dbf' SIZE 50G AUTOEXTEND ON NEXT 5G MAXSIZE 100G;
```

SIZE可以是kilobytes (K), megabytes (M), gigabytes (G), or terabytes (T).

如果创建表空间的类型是大文件表空间，则不需要指定BIGFILE参数。

是不是大文件表空间可通过DBA_TABLESPACES/USER_TABLESPACES/V$TABLESPACE表的BIGFILE字段查看

```
SELECT TABLESPACE_NAME, BIGFILE FROM DBA_TABLESPACES;

TABLESPACE_NAME             BIGFILE
SYSTEM                      NO
SYSAUX                      NO
UNDOTBS1                    NO
TEMP                        NO
USERS                       NO
TIEAF_CUS                   NO
TIEAF_TEMP                  NO
TIEAF_TEST1                 NO
TIEAF_SYS                   NO
CWR_ATTENDANCE_SERVICE      NO
TBS_TICWR                   NO
```

## 修改

```
ALERT DATABASE 
    DATAFILE '/u02/oracle/data/bigtbs01.dbf' AUTOEXTEND ON MAXSIZE UNLIMITED;
```

*MAXSIZE UNLIMITED*即为最大值

# Smallfile Tablespaces

使用22位来存储block号，大小为(2^22-1)*32K = 128G(Oracle最大支持block为32k)

## 创建

```
CREATE SMALLFILE TABLESPACE smalltbs 
    DATAFILE '/u02/oracle/data/smalltbs01.dbf' SIZE 10G
```

```
ALERT SMALLFILE TABLESPACE smalltbs 
    ADD DATAFILE '/u02/oracle/data/smalltbs01.dbf' SIZE 1G AUTOEXTEND ON NEXT 5M MAXSIZE 10G;
```

SIZE可以是kilobytes (K), megabytes (M), gigabytes (G), or terabytes (T).

## 修改

```
ALERT DATABASE 
    DATAFILE '/u02/oracle/data/smalltbs01.dbf' SIZE 1G AUTOEXTEND ON MAXSIZE UNLIMITED;
```

*MAXSIZE UNLIMITED*即为最大值

# 相关查询

block size多大？

```
select value from v$parameter where name='db_block_size';
```

data file概况

```
select * from dba_data_files;
```

data files最大值

```
select name, value from v$parameter where name like 'db_files';
```

# 参考链接

- [15 Managing Data Files and Temp Files](https://docs.oracle.com/cd/E11882_01/server.112/e25494/dfiles.htm#ADMIN012)

- [14 Managing Tablespaces](https://docs.oracle.com/cd/E11882_01/server.112/e25494/tspaces.htm#ADMIN13316)

