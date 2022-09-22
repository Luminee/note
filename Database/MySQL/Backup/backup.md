### 备份和恢复

#### 1. 备份

##### 1. 备份策略

1. 完全备份 (每次都将所有数据备份)
2. 差异备份(第一次完全备份，之后备份与第一次备份之间的数据)
3. 增量备份(第一次完全备份，之后备份与上一次备份之间的数据)

##### 2. 备份方式

1. 物理冷备 (关闭数据库，直接打包数据文件)
2. 备份工具 (mysqldump 常用的逻辑备份工具)
3. 二进制日志 (增量备份)

#### 2. 完全备份

##### 1. 物理备份和恢复

> 停机打包数据文件，速度快，简单

```sh
# 停机备份
systemctl stop mysqld
yum -y install xz
cd /usr/local/mysql

tar Jcvf /opt/mysql_all_$(date +%F).tar.xz data/

# 停机恢复
cd /usr/local/mysql
tar Jxvf /opt/mysql_all_2022-09-20.tar.xz -C ./

# 启动服务
systemctl start mysqld.service
```



##### 2. mysqldump 备份和恢复

> 常用的逻辑备份工具

```sh
# 备份一个或多个库
mysqldump -u root -p pwd --database db1 [db2] > /opt/backup.sql
# 备份全部库
mysqldump -u root -p [pwd] --all-databases > /opt/backup.sql
# 备份指定库的部分表 /?/
mysqldump -u root -p [pwd] db1 [tab1] [tab2] > /opt/backup.sql

# 恢复数据库
mysql -u root -p < /opt/backup.sql
# 恢复数据表
mysql -u root -p db1 < /opt/backup.sql
```



#### 3. 增量备份与恢复

> 通过 MySQL 二进制日志方式备份

```ini
# vim /etc/my.cnf
[mysqld]
log-bin=mysql-bin
# STATEMENT (基于SQL语句)
# ROW (基于行)
# MIXED (混合模式)
binlog_format=ROW
server-id=1
```

```sh
#查看二进制文件
mysqlbinlog --no-defaults --base64-output=decode-rows -v /opt/mysql-bin.000007
```


```sh
# 生成新的二进制日志文件
mysqladmin -u root -p flush-logs
# 按文件恢复数据
mysqlbinlog --no-defaults /usr/lcal/mysql/data/mysql-bin.000007 | mysql -u root -p
# 基于位置点恢复
mysqlbinlog --no-defaults --stop-position='22233' mysql-bin.000008 | mysql -u root -p
# --start-position --stop-datetime --start-datetime
```

