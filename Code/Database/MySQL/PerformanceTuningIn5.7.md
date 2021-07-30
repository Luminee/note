## 1. 5.7 Default Settings

- `innodb_file_per_table`=ON
- `innodb_stats_on_metadata`=OFF
- `innodb_buffer_pool_instances`=8
- `query_cache_type`=0; `query_cache_size`=0

## 2. Four Important Variables

```ini
[mysqld]
innodb_buffer_pool_size=1G #(50%-70% of total RAM)
innodb_log_file_size=256M #(128M - 2)
innodb_flush_log_at_trx_commit=1
innodb_flush_method=O_DIRECT
```

