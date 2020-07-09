## 1、备份
```sql
mysqldump -u easytrack -p dgd_test -P 3306 -h 192.168.1.12 > C:\Users\Administrator\Desktop\sqlbak\dgd_test.dump
```
## 2、还原
```sql
mysql -P 33306 -u 192.168.1.11@easytrack -p dgd_test < C:\Users\Administrator\Desktop\sqlbak\dgd_test.dump
```



## 3、例子：

备份：

```
mysqldump -P 3306 -h 192.168.1.12 -u easytrack -p ppm10> /home/dump/ppm10_20_7_9.dump
```

还原

```
mysql -P 3306 -u easytrack -p ppm10_lxp< /home/dump/ppm10_20_7_9.dump
```



## 4、授权

easytarck用户可以在任何ip下对任何数据库进行任何操作

```mysql
grant all privileges on *.*  yto "easytarck"@"%";
```



