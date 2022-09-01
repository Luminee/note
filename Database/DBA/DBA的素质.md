## 环境

1. 软件环境

    a. 操作系统: Linux or Windows, 内核, CPU, RAM, IO, 带宽

    b. 数据库: 部署目录, 可执行文件, 数据文件, log, 配置文件, 启动和停止, 数据库版本

    c. 引擎: 默认引擎, 引擎特点, 现用引擎

    d. 同步方式: binlog, GTID, 异步, 半同步, 单线程 多线程同步

    e. 存储过程, 事件

    f. 关键配置: 

 ```ini

 innodb_buffer_pool_size

 #对innodb生效，对性能影响非常大，一般可以设置内存的50~80%

 

 key_buffer_size

 #对Myisam生效，建议修改成innodb

 innodb_flush_log_at_trx_commit

 #innodb的redo日志刷新方式，对innodb的影响会很大，一般设置为2log

 -bin

 #是否开始binlog，如果没有开启，一定要开启

 sync_binlog

 #刷binlog的方式，一般设置为0，如果对数据需要强一致的，可以将sync_binlog设置为大于1的数，兼顾安全和性能

 innodb_file_per_table

 #采用独立表空间，建议都设置成独立表空间，不然后面磁盘空间满了，删除表空间也无法释放，必须做数据迁移

 lower_case_table_names

 #表名区分大小写

 character_set_server

 #字符集在迁移、数据库变更、数据导入等都是必须要注意的，不然数据乱码了就会很麻烦

 max_connections

 #最大连接数不能设置太大，要计算一下session内存*max_connections + 固定内存 < 总内存-2G（这2G用来做系统内存，留给系统的内存可以再设大一点）

 transaction_isolation

 #设置隔离级别，默认是Repeatable Read，如果是binlog是row模式，也经常设置为Read Committed级别

 ```

2. 硬件环境

    a. QPS, TPS, CPU核数, 内存大小, 硬盘, 压测数据

    b. sysbench, mysqlslap, fio 工具

3. 运行状态

    a. 数据量, 单表数据量, 负载(CPU, IO, 系统负载)

    b. 慢查询, SQL 延迟, 锁状况, 脏页

    c. 访问模型(读写均衡, 高并发, pt-mysql-summary, innotop, orzdba 工具)

## 数据安全

1. 权限: 复杂密码， 禁止 % 登录, 限制业务账号权限, 并限制登录机器

2. 数据一致性: 周期使用 pt-table-checksum 检查主从一致

3. 数据安全: 备份(全量备份和binlog增量备份, 备份数据安全, 确保备份数据可用)

## 常规操作

1. 操作: 启动, 变更, 索引, 配置修改, 备份, 迁移, 切换

2. 常见故障, 极端故障, 定期演习

## 架构

1. 了解数据库架构

2. 了解数据库结构

3. 了解业务

## 操作经验

1. 修改删除前先备份, 更改要有回退方案, 批量操作间添加sleep, alter 需谨慎

2. 变更前先测试, 禁止删除大量 binlog, 变更操作要写详细步骤

3. 确认执行前要检查, 如果情况可能变坏则停止

4. 连接数满先修改内存变量，而不是重启

 ```sh
gdb -p pid -ex "set max_connections=1000" -batch
#pid是mysqld的对应的pid
 ```

