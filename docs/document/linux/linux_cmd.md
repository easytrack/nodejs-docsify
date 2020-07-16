# 开启端口

1、开启防火墙 
    systemctl start firewalld

2、开放指定端口
      firewall-cmd --zone=public --add-port=80/tcp --permanent
 命令含义：
--zone #作用域
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
--permanent  #永久生效，没有此参数重启后失效

3、重启防火墙
      firewall-cmd --reload

firewall-cmd --list-ports 查看开启的端口

```
# 查看防火墙，添加的端口也可以看到
firewall-cmd --list-all
```

```shell
# 安装firewalld
yum install firewalld firewall-config

systemctl start  firewalld # 启动
systemctl status firewalld # 或者 firewall-cmd --state 查看状态
systemctl disable firewalld # 停止
systemctl stop firewalld  # 禁用
```

[参考文档](https://www.cnblogs.com/tkzc2013/p/11319625.html)

