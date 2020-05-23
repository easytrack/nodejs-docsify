# v2ray 配置

------

##  一 、开通服务器

1. 购买阿里云轻量应用服务器

[阿里轻应用服务器](https://www.aliyun.com/product/swas?spm=5176.12825654.eofdhaal5.35.20bd2c4aKhpzfU)

## 二、卸载阿里云盾

1. [登陆阿里云并卸载云盾](https://swas.console.aliyun.com/?spm=5176.161059.835498.2.7c097fdaO9vlV4#/server/43ba82d115c04e3480fc1fa43433a76d/cn-hongkong/dashboard)

```shell
卸载阿里云盾
远程连接到阿里云云服务器或者轻量应用服务器后，执行以下代码卸载阿里云盾：

wget http://update.aegis.aliyun.com/download/uninstall.sh
chmod +x uninstall.sh
./uninstall.sh
wget http://update.aegis.aliyun.com/download/quartz_uninstall.sh
chmod +x quartz_uninstall.sh
./quartz_uninstall.sh
删除阿里云盾文件残留
卸载阿里云盾后，执行如下代码删除阿里云盾文件残留：

pkill aliyun-service
rm -fr /etc/init.d/agentwatch /usr/sbin/aliyun-service
rm -rf /usr/local/aegis*

卸载云监控 Java 版
/usr/local/cloudmonitor/wrapper/bin/cloudmonitor.sh stop
/usr/local/cloudmonitor/wrapper/bin/cloudmonitor.sh remove
rm -rf /usr/local/cloudmonitor


屏蔽阿里云盾IP
最后就是屏蔽阿里云盾的IP：

iptables -I INPUT -s 140.205.201.0/28 -j DROP
iptables -I INPUT -s 140.205.201.16/29 -j DROP
iptables -I INPUT -s 140.205.201.32/28 -j DROP
iptables -I INPUT -s 140.205.225.192/29 -j DROP
iptables -I INPUT -s 140.205.225.200/30 -j DROP
iptables -I INPUT -s 140.205.225.184/29 -j DROP
iptables -I INPUT -s 140.205.225.183/32 -j DROP
iptables -I INPUT -s 140.205.225.206/32 -j DROP
iptables -I INPUT -s 140.205.225.205/32 -j DROP
iptables -I INPUT -s 140.205.225.195/32 -j DROP
iptables -I INPUT -s 140.205.225.204/32 -j DROP

iptables-save > /etc/sysconfig/iptables
iptables --list-rules

iptables-save




检查阿里云盾是否卸载干净
最后检查下自己服务器上的阿里云盾是否卸载干净了，主要就是看进程里有没有阿里云盾的相关进程了（AliYunDun、aliyun-service和AliYunDunUpdate），
可以通过ps -aux | grep -E 'aliyun|AliYunDun'来检查，如果没有相关进程则说明阿里云盾已经卸载干净了。


```



## 三、安装工具/下载可执行文件

1. 安装wget

```shell
yum -y install wget    ##ContOS Yum安装wget
```

2. 安装curl

```shell
yum update -y && yum install curl -y            ##Centos 系统安装 Curl 方法
```

3. 安装/更新方式（h2和ws版本已合并）Vmess+websocket+TLS+Nginx+Website

```shell
wget -N --no-check-certificate -q -O install.sh "https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install.sh" && chmod +x install.sh && bash install.sh
```

4. 安装加速包

``` shell
————————————内核管理————————————
 1. 安装 BBR原版内核 - 5.4.14/5.5.5/5.6.11
 2. 安装 BBRplus版内核 - 4.14.129
 3. 安装 Lotserver(锐速)内核 - 多种
 4. 安装 xanmod版内核 - 5.5.1/5.5.8
 5. 安装 BBR2测试版内核 - 5.4.0
 6. 安装 Zen版内核 - 5.5.2/5.5.10
 7. 安装 BBRplus新版内核 - 4.14.173
————————————加速管理————————————
 11. 使用BBR+FQ加速
 12. 使用BBR+CAKE加速 
 13. 使用BBRplus+FQ版加速
 14. 使用Lotserver(锐速)加速
 15. 使用BBR2+FQ加速
 16. 使用BBR2+CAKE加速
 17. 使用BBR2+FQ+ECN加速
 18. 使用BBR2+CAKE+ECN加速 
————————————杂项管理————————————
 21. 卸载全部加速
 22. 系统配置优化
 23. 退出脚本
————————————————————————————————

安装步骤：
1、选择数字：2
2、选择数字：13
```



## 四、安装docker

1. 卸载旧版本

   ``` shell
   sudo yum remove docker \
                     docker-client \
                     docker-client-latest \
                     docker-common \
                     docker-latest \
                     docker-latest-logrotate \
                     docker-logrotate \
                     docker-engine
   ```

2. 安装docker

   ```shell
   sudo yum install -y yum-utils \
     device-mapper-persistent-data \
     lvm2
     
   sudo yum-config-manager \
     --add-repo \
     https://download.docker.com/linux/centos/docker-ce.repo
     
   sudo yum install docker-ce docker-ce-cli containerd.io
   
   systemctl enable docker
   
   systemctl enable docker.service
   ```

3. 下载compose

   ```shell
   #运行以下命令以下载 Docker Compose 的当前稳定版本
   curl -L https://github.com/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
   
   #将可执行权限应用于二进制文件：
   chmod +x /usr/local/bin/docker-compose
   ```

4. docker常用命令

   > docker-compose up 启动
   > docker-compose up -d 后台启动
   > docker-compose down 卸载
   > docker-compose restart 重启
   > docker-compose stop 停止容器
   > docker-compose start 启动日期
   >
   > docker ps 查看已经启动的容器
   >
   > docker stats 查看资源消耗

## 五、配置证书

1. 新建文件夹存放证书

   > mkdir docker
   >
   > #上传文件v2ray-nginx-tls.zip至docker目录下，[文件路径](http://www.baidu.com)
   >
   > #安装socat
   >
   > curl  https://get.acme.sh | sh
   > yum -y install socat
   >
   > #生成证书：
   >
   > $ sudo ~/.acme.sh/acme.sh --issue -d jeffrey.easytrack.gq --standalone -k ec-256
   >
   > #安装证书到 /etc/v2ray 中：
   >
   > sudo ~/.acme.sh/acme.sh --installcert -d jeffrey.easytrack.gq --fullchainpath /etc/v2ray/v2ray.crt --keypath /etc/v2ray/v2ray.key --ecc
   >
   > #复制证书至docker/v2ray-nginx-tls/nginx-conf/data目录下
   >
   > cp /etc/v2ray/ /
   >
   > #吊销证书：
   >
   > ~/.acme.sh/acme.sh --revoke -d ml.easytarck.ml --ecc

2. v2ray-conf/config.json

   > 可修改唯一id，vpn配置代理时需用到
   >
   > "id":"6845c6f3-6f75-4cba-929c-21c068406f16"

3. conf.d/v2ray.conf

   ```shell
   server {
   		listen 443 ssl http2;
   			ssl_certificate       /data/v2ray.crt;
   			ssl_certificate_key   /data/v2ray.key;
   			ssl_protocols         TLSv1.2 TLSv1.3;
   			ssl_ciphers           TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+ECDSA+AES128:EECDH+aRSA+AES128:RSA+AES128:EECDH+ECDSA+AES256:EECDH+aRSA+AES256:RSA+AES256:EECDH+ECDSA+3DES:EECDH+aRSA+3DES:RSA+3DES:!MD5;
   	set $mydomain jeffrey.easytrack.gq;
   	
   	server_name $mydomain;
   		index index.html index.htm;
   		root  /usr/share/nginx/html/3DCEList;
   		error_page 400 = /400.html;
   	location /v2ray/
   		{
   		proxy_redirect off;
   		proxy_pass http://my_v2ray:6666;
   		proxy_http_version 1.1;
   		proxy_set_header Upgrade $http_upgrade;
   		proxy_set_header Connection "upgrade";
   		proxy_set_header Host $http_host;
   		}
   }
   server {
       	set $mydomain jeffrey.easytrack.gq;
   	listen 80;
   	server_name $mydomain;
   	return 301 https://jeffrey.easytrack.gq$request_uri;
   }
   
   配置步骤：
   1、修改对应域名jeffrey.easytrack.gq
   2、location /v2ray/此目标对应vpn配置路径
   ```

## 六、启动docker

```shell
 docker-compose up /
 docker-compose up -d
```



## 七、证书

```sh
证书生成：
TLS 是证书认证机制，所以使用 TLS 需要证书，证书也有免费付费的，同样的这里使用免费证书，证书认证机构为 Let's 
Encrypt。
证书的生成有许多方法，这里使用的是比较简单的方法：使用 acme.sh 脚本生成，本部分说明部分内容参考于acme.sh
README。

证书有两种，一种是 ECC 证书（内置公钥是 ECDSA 公钥），一种是 RSA 证书（内置 RSA 公钥）。简单来说，同等长
度 ECC 比 RSA
 更安全,也就是说在具有同样安全性的情况下，ECC 的密钥长度比 RSA 短得多（加密解密会更快）。但问题是 ECC 的兼
容性会差一些，
 Android 4.x 以下和 Windows XP 不支持。只要您的设备不是非常老的老古董，建议使用 ECC 证书。

以下将给出这两类证书的生成方法，请大家根据自身的情况自行选择其中一种证书类型。
证书生成只需在服务器上操作。

安装 acme.sh
执行以下命令，acme.sh 会安装到 ~/.acme.sh 目录下。

$ curl  https://get.acme.sh | sh


安装成功后执行 source ~/.bashrc 以确保脚本所设置的命令别名生效。

如果安装报错，那么可能是因为系统缺少 acme.sh 所需要的依赖项，acme.sh 的依赖项主要是 netcat(nc)，我们通过以下命令来安装这些
依赖项，然后重新安装一遍 acme.sh:

$ yum -y install socat


使用 acme.sh 生成证书
#证书生成
执行以下命令生成证书：

以下的命令会临时监听 80 端口，请确保执行该命令前 80 端口没有使用

$ sudo ~/.acme.sh/acme.sh --issue -d ml.easytrack.ml --standalone -k ec-256

-k 表示密钥长度，后面的值可以是 ec-256 、ec-384、2048、3072、4096、8192，带有 ec 表示生成的是 ECC 证书，没有则是 RSA 证书。
在安全性上 256 位的 ECC 证书等同于 3072 位的 RSA 证书。

#证书更新
由于 Let's Encrypt 的证书有效期只有 3 个月，因此需要 90 天至少要更新一次证书，acme.sh 脚本会每 60 天自动更新证书。也可以手动更新。
手动更新 ECC 证书，执行：
$ sudo ~/.acme.sh/acme.sh --renew -d p1.iriscy.ml --force --ecc
如果是 RSA 证书则执行：
$ sudo ~/.acme.sh/acme.sh --renew -d p1.iriscy.ml --force
由于本例中将证书生成到 /etc/v2ray/ 文件夹，更新证书之后还得把新证书生成到 /etc/v2ray。


安装证书和密钥
#ECC 证书
将证书和密钥安装到 /etc/v2ray 中：
$ sudo ~/.acme.sh/acme.sh --installcert -d ml.easytrack.ml --fullchainpath /etc/v2ray/v2ray.crt --keypath /etc/v2ray/v2ray.key --ecc

#RSA 证书
$ sudo ~/.acme.sh/acme.sh --installcert -d p1.iriscy.ml --fullchainpath /etc/v2ray/v2ray.crt --keypath /etc/v2ray/v2ray.key
注意：无论什么情况，密钥(即上面的v2ray.key)都不能泄漏，如果你不幸泄漏了密钥，可以使用 acme.sh 将原证书吊销，再生成新的证书，吊销方法请
自行参考 acme.sh 的手册
~/.acme.sh/acme.sh --revoke -d ml.easytarck.ml --ecc
```



## 八、参考文献

[开始配置](https://www.v2rayssr.com/v2raywsh2.html)