## 1、 常用命令1



- ﻿查看oracle版本： `SELECT * from v$version;`
- 无口令登录：`sqlplus / as sysdba - 无用户/口令登陆`
- 有口令登录：`sqlplus lixupeng/lixupeng as sysdba`
- 查看数据库大小：

```sql
SELECT t.tablespace_name, round(SUM(bytes / (1024 * 1024)), 0) ts_size FROM dba_tablespaces t, dba_data_files d WHERE t.tablespace_name = d.tablespace_name 
GROUP BY t.tablespace_name;

SELECT * from  (select segment_name, MAX(BYTES/1024/1024) as memory_M from dba_segments where owner=(select user from dual) GROUP BY segment_name) ORDER BY memory_M desc;
```

- 查看表空间：`SELECT * FROM dba_tablespaces t;`

- 查看用户：`select * from dba_users;`

- 查看日志文件：`select * from v$logfile;`

- 查看数据文件：`SELECT * FROM dba_data_files t; 	select * from v$datafile;` 

- 修改用户和密码：`alter user $username identified by $pwd;`

- 解锁用户：`ALTER USER 用户名 ACCOUNT UNLOCK;`

- 设置用户不会被锁定：`ALTER PROFILE DEFAULT LIMIT FAILED_LOGIN_ATTEMPTS UNLIMITED;`

- 设置用户密码不过期：`ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;`

- 查看服务名：`select global_name from global_name;` 

- 修改服务名：`set ORACLE_SID=orcl`

- 删除用户：drop user XXXX cascade;

- 删除备份备份目录:`drop directory kmdata_exp ;`

- 新建备份目录：

  ```sql
  create or replace directory kmdata_exp as '/u01/app/oracle/backup ';
  ```
  


## 2、创建表空间

- 创建临时表空间：

  ```sql
  create temporary tablespace easytrack_temp 
  tempfile 'E:\Databases\oracleXE\app\oracle\oradata\XE\easytrack_temp.dbf' 
  size 50m 
  autoextend on 
  next 50m maxsize 20480m 
  extent management local; 
  ```

- 创建数据表空间

  ```sql
  create tablespace easytrack 
  logging 
  datafile 'E:\Databases\oracleXE\app\oracle\oradata\XE\easytrack.dbf' 
  size 50m 
  autoextend on 
  next 50m maxsize 20480m 
  extent management local; 
  ```

- 创建用户并指定表空间

  ```sql
  create user easytrack identified by easytrack 
  default tablespace easytrack 
  temporary tablespace easytrack_temp; 
  ```

- 给用户授予权限

  ```sql
  grant connect,resource,dba to easytrack; 
  GRANT CREATE USER,DROP USER,ALTER USER,CREATE ANY VIEW,DROP ANY VIEW,EXP_FULL_DATABASE,IMP_FULL_DATABASE,DBA,CONNECT,RESOURCE,CREATE SESSION TO easytrack; 
  ```
## 3、备份与还原

[参考文献](https://www.cnblogs.com/promise-x/p/7477360.html)

### 准备工作

导入导出前准备，新服务器或者客户的数据库服务器，需要准备：
1，建立表空间，最好就是我们产品的默认表空间，easytrack，我们自身的空库以后就，且只能使用easytrack表空间
2，各种运维oracle数据库的优化配置与必要的安全备份策略，字符集AL32UTF8等

查看管理理员目录(可以先查看逻辑目录是否存在，已经存在则不用新建）
select * from dba_directories;
create directory mydumpfile as 'C:\mydumpfile';

建立逻辑目录：（3号机逻辑路径easytrack目录  'D:\';   dpdata2  'D:\share';）





### 导入一

表空间要求相同，且用户名相同，同一台服务器上测试时，会报错，因为数据库已经存在了，把源库删除，即可测试，但是要注意备份源库，别把数据搞丢了

```sql
impdp adm_liaoguohua/adm_liaoguohua schemas=adm_liaoguohua directory= dpdata2 dumpfile=adm_liaoguohua.dmp logfile=impdp.log;
```



### 导入二

表空间要求相同，用户名不同，同一台服务器就可以测试

```sql
impdp adm_cs/adm_cs schemas=adm_cs remap_schema=adm_liaoguohua:adm_cs directory=dpdata2 dumpfile=adm_liaoguohua.dmp logfile=impdp.log;
```

备份和恢复，表空间不同，且用户名也不同(目前各项目推荐用这种)
将表空间TBS01、TBS02、TBS03导入到表空间A_TBS，将用户B的数据导入到A，并生成新的oid防止冲突（不报错的话，transform=oid:n 不要加）；

```sql
impdp A/passwd remap_tablespace=TBS01:A_TBS,TBS02:A_TBS,TBS03:A_TBS remap_schema=B:A FULL=Y transform=oid:n 
directory=data_dir dumpfile=expdp.dmp logfile=impdp.log
```



##  9、错误解决

### 导出报错及解决方案

导出：

```sh
expdp user/pwd@orcl schemas=user1 directory=directory_name dumpfile=xxxx.dmp logfile=xxx.log

例如expdp hisense_lixupeng/hisense_lixupeng@orcl schemas=hisense_lxp directory=dpdata2 dumpfile=hisense_zy.dmp logfile=hisense_zy.log;

最终导出的是 hisense_lxp库

/* 说明：user/pwd@orcl --->用户名/密码@数据库实例
        directory="逻辑目录"
		schemas=数据实例，最终要导出的库
        dumpfile="需要导入/导出的dmp文件全称"
        logfile="日志文件"
        FULL=y;  导出 不用full，导入用full
*/
[oracle@oracle-rac01 backup]$ expdp parfile=expdp.par 

Export: Release 11.2.0.4.0 - Production on Mon Sep 4 09:20:59 2017

Copyright (c) 1982, 2011, Oracle and/or its affiliates. All rights reserved.

Connected to: Oracle Database 11g Enterprise Edition Release 11.2.0.4.0 - 64bit Production
With the Partitioning, Real Application Clusters, Automatic Storage Management, OLAP,
Data Mining and Real Application Testing options
ORA-39002: invalid operation
ORA-39070: Unable to open the log file.
ORA-29283: invalid file operation
ORA-06512: at "SYS.UTL_FILE", line 536
ORA-29283: invalid file operation
```

```sql
解决：重新创建备份目录
1.1先删除原先创建的备份目录
SQL> drop directory kmdata_exp ;

1.2 重新创建新的备份目录
SQL> create or replace directory kmdata_exp as '/u01/app/oracle/backup ';
Directory created.
SQL> grant read,write on directory kmdata_exp to public ;

1.3 修改备份目录的属主和属组
[root@oracle-rac01 ~]# mkdir /u01/app/backup/ 
[root@oracle-rac01 ~]# chown -R oracle:oinstall /u01/app/oracle/backup/
```

版权声明：本文为CSDN博主「东城绝神」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/m0_37814112/article/details/78119212


​	
##  10、备份与还原的例子
```sql
SELECT * from  (select segment_name, MAX(BYTES/1024/1024) as memory_M from dba_segments where owner=(select user from dual) GROUP BY segment_name) ORDER BY memory_M desc;

expdp user/pwd@orcl schemas=HISENSE_LXP directory=DATA_PUMP_DIR dumpfile=heise.dmp logfile=heise.log
expdp hisense_lxp/hisense_lxp@XE schemas=HISENSE_LXP directory=DATA_PUMP_DIR dumpfile=heise.dmp logfile=heise.log

CREATE temporary tablespace ETTPM_TEMP tempfile '/u01/app/oracle/oradata/XE/ETTPM_TEMP.DBF' SIZE 100M autoextend ON next 50M;
CREATE tablespace ETTPM datafile '/u01/app/oracle/oradata/XE/ETTPM.DBF' SIZE 100M
autoextend ON next 50M;

CREATE temporary tablespace EASYTRACK_TEMP tempfile '/u01/app/oracle/oradata/XE/EASYTRACK_TEMP.DBF' SIZE 100M autoextend ON next 50M;
CREATE tablespace EASYTRACK datafile '/u01/app/oracle/oradata/XE/EASYTRACK.DBF' SIZE 100M
autoextend ON next 50M;

--drop user hisense_lxp cascade;
--drop tablespace xxxx;
--drop temporary tablespace xxxx;

CREATE temporary tablespace hisense_temp tempfile '/u01/app/oracle/oradata/XE/hisense_temp.DBF' SIZE 100M autoextend ON next 50M;
CREATE tablespace hisense datafile '/u01/app/oracle/oradata/XE/hisense.DBF' SIZE 100M
autoextend ON next 50M;

create user hisense_lxp identified by hisense_lxp default tablespace hisense temporary tablespace hisense_temp;

grant dba to hisense_lxp;


impdp hisense_lxp/hisense_lxp remap_tablespace=ETPPM:hisense remap_schema=etppm:hisense_lxp FULL=Y directory=DATA_PUMP_DIR dumpfile=backup191031V1.5.dmp logfile=backup191031V1.5.log;




===========example=========
expdp etppm/etppm2019##@pm  schemas=etppm  dumpfile=/oracle/soft/expdp20190827.dmp

impdp tcltest/tcltest schemas=tcltest remap_schema=etppm:tcltest directory=dpdata2 dumpfile=expdp20190827.dmp logfile=impdp.log;

===========example  海信新建表空间=========
drop user XXXX cascade;  /* 删除用户 */ 
drop tablespace xxxx;  /* 删除无任何数据对象的表空间*/
create temporary tablespace hisense_temp  tempfile 'E:\app\oracle11g\oradata\orcl\hisense_temp.DBF' size 100M autoextend on next 50M;/*创建临时表空间 */ 
create tablespace hisense datafile 'E:\app\oracle11g\oradata\orcl\hisense.DBF' size 100M autoextend on next 50M;
create user hisense_lxp identified by hisense_lxp default tablespace hisense temporary tablespace hisense_temp;/*创建用户并指定表空间 */ 
grant dba to hisense_lxp; /*赋予权限 */ 
impdp hisense_lxp/hisense_lxp remap_tablespace=ETPPM:hisense, remap_schema=etppm:hisense_lxp FULL=Y  directory=dpdata2 dumpfile=ET10_v1.0_oracle11g_62054.DMP
==========================================

drop user tcl_wxy cahisense_lxpscade;
create user tcl_wxy identified by tcl_wxy default tablespace etppm temporary tablespace etppm_temp; 
grant connect,resource,dba to tcl_wxy;

expdp tcl20190920/tcl20190920@orcl schemas=tcl20190920 directory=dpdata2 dumpfile=tclbug0924.dmp logfile=tclbug0924.log;
impdp tcl_wxy/tcl_wxy remap_schema=tcl20190920:tcl_wxy directory=dpdata2 dumpfile=tclbug0924.dmp logfile=tclbug0924.log;

==========TCL================================
create temporary tablespace TCL_temp tempfile 'D:\oracledata\TCL_temp.dbf' size 100M autoextend on next 50M;/*创建临时表空间 */ 
create tablespace TCL_tablespace datafile 'D:\oracledata\TCL_data.dbf' size 100M autoextend on next 50M;
create user TCL identified by TCL default tablespace TCL_tablespace temporary tablespace TCL_temp ;/*创建用户并指定表空间 */ 
grant dba to TCL; /*赋予权限 */ 

impdp TCL/TCL remap_schema=etppm:TCL directory=dpdata2 dumpfile=TCL9.dmp logfile=TCL9.log;
expdp TCL/TCL@orcl schemas=TCL directory=dpdata2 dumpfile=TCL.dmp logfile=TCL.log;


------------lixupeng---------

select * from dba_directories;
create directory mydumpfile as 'C:\mydumpfile';

expdp SYSTEM/easytrack@XE schemas=hisense directory=mydumpfile dumpfile=test.dmp logfile=test.log;



```

