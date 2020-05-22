
```sql
initdb -E UTF-8 --locale=chinese-simplified_china.936   -D D:\app\pgsql\data -U postgres -W  windows
initdb -E UTF-8 --locale=zh_CN.UTF-8  -D /pgsql/data   linux
```



```sql
CREATE DATABASE ppmcloud
  WITH OWNER = easytrack
       ENCODING = 'UTF8'
       TABLESPACE = pg_default
       LC_COLLATE = 'Chinese (Simplified)_China.936'
       LC_CTYPE = 'Chinese (Simplified)_China.936'
       CONNECTION LIMIT = -1;
```

```sql
createdb -U easytrack -h 192.168.1.2 et_zy

dropdb -U easytrack -h 192.168.1.2 et_zy
```

备份所有schema的方式

```sql
pg_dump -h 192.168.1.13 -p 5432 -E UTF8 -U easytrack --compress 9 -b -F custom  -f D:\ppmcloud9.backup -d ppm8test
```

```sql
pg_restore -U easytrack -d ppmtest  D:\ppmcloud9.backup
```


备份所有schema的方式

```sql
pg_dump -U easytrack -h 192.168.1.2 skyworth > skyworth.sql
```

```sql
psql -U easytrack -h 192.168.1.2 et_zy < skyworth.sql
```



>
> C:\Users\elwin>"d:\app\pgsql\bin\pg_ctl" register -N pgsql -D "d:\app\pgsql\data"   --注册本机服务
>

------



## 1、windows下sql文件备份与恢复

(1)备份
cmd命令 进入备份导出的目录

```sql
pg_dump -U easytrack -p 5432 -h localhost easytrack>D:\sqlbak\easytrack.sql
```

(2)恢复
A 删除当前数据库

```sql
dropdb -U easytrack -p 5432 easytrack
```

B 创建数据库

```sql
createdb -U easytrack -p 5432 easytrack
```

C 还原数据库

```sql
psql -U easytrack -p 5432 easytrack<D:\sqlbak\easytrack.sql
```

## 2、windows下dmp文件备份与恢复
```sql
CREATE ROLE easytrack CREATEDB PASSWORD 'easytrack';
```

```sql
ALTER ROLE easytrack WITH SUPERUSER;
```

创建角色并给超级管理员权限

```sql
ALTER ROLE easytrack WITH SUPERUSER;
```

(1)备份

```sql
pg_dump -U easytrack -p 5432 -Fc -f D:\sqlbak\easytrack.dmp -h 192.168.1.13 ppm9demo
```

(2)恢复

A:删除当前数据库

```sql
dropdb -U easytrack -p 5432 easytrack
```

B:创建数据库

```sql
createdb -U easytrack -p 5432 easytrack
```

C:还原数据库

```sql
pg_restore -U easytrack -h 192.168.1.11 -p 65432 -d easytrack D:\sqlbak\easytrack.dmp
```



## 3、linux下sql文件备份与恢复
数据备份
执行/home/easytrack/EasyTrack目录下的backupdb.sh，备份数据文件到/home/easytrack/EasyTrack/backup目录下
数据恢复

1.  已 EasyTrack身份登录Linux操作系统； 
2.  连接 postgreSQL 数据库；（/home/easytrack/EasyTrack/pgsql/bin/psql -U easytrack -p 5432 easytrack） 
3.  删除现有数据库；（/home/easytrack/EasyTrack/pgsql/bin/dropdb -U easytrack -p 5432 easytrack） 
4.  创建新数据库；（/home/easytrack/EasyTrack/pgsql/bin/createdb -U easytrack -p 5432 easytrack） 
5.  恢复数据库。（/home/easytrack/EasyTrack/pgsql/bin/psql -U easytrack -p 5432 easytrack<easytrack.sql）



## 4、linux下dmp文件恢复
以 EasyTrack身份登录Linux操作系统
数据备份
在EasyTrack的系统管理中执行备份，备份数据文件到/home/easytrack/EasyTrack/backup目录
数据恢复

1.  已 EasyTrack身份登录Linux操作系统； 
2.  连接 postgreSQL 数据库；（/home/easytrack/EasyTrack/pgsql/bin/psql -U easytrack -p 5432 easytrack） 
3.  删除现有数据库；（/home/easytrack/EasyTrack/pgsql/bin/dropdb -U easytrack -p 5432 easytrack） 
4.  创建新数据库；（/home/easytrack/EasyTrack/pgsql/bin/createdb -U easytrack -p 5432 easytrack） 
5.  恢复数据库。（/home/easytrack/EasyTrack/pgsql/bin/pg_restore -U easytrack -p 5432 -d easytrack easytrack.dmp）



"pg_ctl" -D "D:\app\pgsql\data" -l logfile start
create user ysr superuser password '123456';



## 5、最常用

```sql
pg_dump -U easytrack -h 192.168.1.13 -p 5432 -Fc -f E:\backup\20191020.dmp ppm10
```

```sql
pg_restore -U easytrack -h 192.168.1.11 -p 54321 -d ppm10 E:\backup\20191020.dmp
```














