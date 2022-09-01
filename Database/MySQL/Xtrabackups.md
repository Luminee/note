## 1. 全量备份

```shell
$ innobackupex --defaults-file=/etc/my.cnf 
--slave-info --user=root --password="123456" 
--backup /data/backups/
```

## 2. 增量备份

```shell

#增量备份1

$ innobackupex --defaults-file=/etc/my.cnf 

--slave-info --user=root --password="123456" 

--incremental /data/backups/ 

--incremental-basedir=/data/backups/2021-02-07_11-33-15

#增量备份2

$ innobackupex --defaults-file=/etc/my.cnf 

--slave-info --user=root --password="123456" 

--incremental /data/backups/ 

--incremental-basedir=/data/backups/2021-02-08_15-23-55

```

## 3. 恢复

### 1. 合并log

```shell

#处理全量log

$ innobackupex --apply-log --redo-only 

/data/backups/2021-02-07_11-33-15/

#合并增量1 到全量log

$ innobackupex --apply-log --redo-only 

--incremental /data/backups/2021-02-07_11-33-15/ 

--incremental-dir /data/backups/2021-02-08_15-23-55

#合并增量2 到全量log

$ innobackupex --apply-log --redo-only 

--incremental /data/backups/2021-02-07_11-33-15/ 

--incremental-dir /data/backups/2021-02-08_15-25-57/

```
### 2. 恢复

```shell

$ service mysql stop

$ rm -rf /usr/local/mysql-5.7.33/data/*

#复制数据文件

$ innobackupex --defaults-file=/etc/my.cnf 

--copy-back /data/backups/2021-02-07_11-33-15

#文件权限

$ chown -R mysql:mysql /usr/local/mysql-5.7.33/data/ 

&& chmod -R 755 /usr/local/mysql-5.7.33/data/

$ service mysql start

```

### 3. binlog 恢复

```shell

$ cat /data/backups/2021-02-18_13-50-04/xtrabackup_binlog_info

$ /usr/local/mysql-5.7.33/bin/mysqlbinlog -vv 

--start-position=154 

/usr/local/mysql-5.7.33/mysql-bin.000003 > recover.sql

$ mysql -uroot -p -N -e "reset master;"

$ mysql -uroot -p < recover.sql

```