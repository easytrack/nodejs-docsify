## 1、备份
```sql
mysqldump -u easytrack -p dgd_test -P 3306 -h 192.168.1.12 > C:\Users\Administrator\Desktop\sqlbak\dgd_test.dump
```
## 2、还原
```sql
mysql -P 33306 -u 192.168.1.11@easytrack -p dgd_test < C:\Users\Administrator\Desktop\sqlbak\dgd_test.dump
```
