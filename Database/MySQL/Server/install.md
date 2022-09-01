### 1. Install


```sh
// 下载
$ wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.27-linux-glibc2.12-x86_64.tar.gz

// 解压缩
$ tar -xvf mysql-5.7.27-linux-glibc2.12-x86_64.tar.gz
$ mv mysql-5.7.27-linux-glibc2.12-x86_64 /usr/local/mysql
```

```sh
// 创建mysql用户组和用户并修改权限
$ groupadd mysql
$ useradd -r -g mysql mysql

// 创建数据目录
$ mkdir -p /data/mysql
$ chown mysql:mysql -R /data/mysql  
```

> vim /etc/my.cnf

```
[mysqld]
bind-address=0.0.0.0
port=3306
user=mysql
basedir=/usr/local/mysql
datadir=/data/mysql
socket=/tmp/mysql.sock
log-error=/data/mysql/mysql.err
pid-file=/data/mysql/mysql.pid
max_connections=10000
max_user_connections=2000
wait_timeout=200
#character config
character_set_server=utf8mb4
symbolic-links=0
explicit_defaults_for_timestamp=true
lower_case_table_names=1
```

```sh
// 初始化数据库
$ /usr/local/mysql/bin/mysqld --defaults-file=/etc/my.cnf --basedir=/usr/local/mysql/ --datadir=/data/mysql/ --user=mysql --initialize

```

> 若报错

```
$ yum install libaio-devel.x86_64
```

```
// 查看密码
$ cat /data/mysql/mysql.err

// 启动服务
$ cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql
$ service mysql start

// 开机启动
$ chmod +x /etc/init.d/mysql
$ chkconfig --add mysql
```

> 如果 3 4 5 不为 on
```
$ chkconfig --list
$ chkconfig --level 345 mysql on
```

```
// 修改密码
$ mysql -u root -p
> SET PASSWORD = PASSWORD('123456');
> ALTER USER 'root'@'localhost' PASSWORD EXPIRE NEVER;
> FLUSH PRIVILEGES;

// 远程登录
> use mysql;
> update user set host = '%' where user = 'root';
> FLUSH PRIVILEGES;