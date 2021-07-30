## 1. Before

1. 一次只改变一个配置
2. 大多数设置可在运行时用 `SET GLOBAL` 配置，但最后记得写到配置文件中
3. 重启后配置仍不生效？确定文件和配置项位置是否正确(本页配置均在 `mysqld` 中)
4. 更改配置后启动失败，请确认单位是否正确
5. 不要重复配置，如果想保持追踪，请使用版本控制
6. 不要做简单运算，例如服务器提升2x 内存，那么所有值都 x2

## 2. Basic Settings

1. `innodb_buffer_pool_size` 这是 InnoDB 首要注意的配置，

   > 5-6GB (8GB RAM), 20-25GB (32GB RAM), 100-120GB (128GB RAM)

2. `innodb_log_file_size` **redo logs** 大小，from `512M` to `4G`
3. `max_connections` 如果经常有 `Too many connections` 错误，则需要调高该值(默认 **150**)。设置太高的值(如 **1000**) 如果真达到1000+ 连接，服务器会无响应，因此需设置合适的值。也可使用连接池或线程池。

## 3. InnoDB settings

1. `innodb_file_per_table` 如果是 `off` 则数据和索引都放在共享空间内，如果是 `on` (默认)则每个表有 `.idb` 和 `.frm` 文件
2. `innodb_flush_log_at_trx_commit` 

