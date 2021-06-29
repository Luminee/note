## 1. 安装 PHP

```sh
yum install epel-release  //首先安装 EPEL 源
yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm //安装 REMI 源
yum install yum-utils //安装 Yum 源管理工具(可忽略)
//安装 PHP7.3
yum install -y php73-php-fpm php73-php-cli php73-php-bcmath \
 php73-php-gd php73-php-json php73-php-mbstring php73-php-mcrypt \
 php73-php-mysqlnd php73-php-opcache php73-php-pdo php73-php-pecl-crypto \
 php73-php-pecl-mcrypt php73-php-pecl-geoip php73-php-recode \
 php73-php-snmp php73-php-soap php73-php-xmll  
systemctl enable php73-php-fpm //开机启动
systemctl restart php73-php-fpm //运行服务
```

## 2. 安装 Nginx

```sh
yum install nginx
systemctl enable nginx //开机启动
firewall-cmd --zone=public --permanent --add-service=http // 打开防火墙 
firewall-cmd --reload // 重启防火墙
```