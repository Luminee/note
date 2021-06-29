[toc]

## 1. SQL Query

1. 长链接

2. 查询缓存

3. 分析器 - 优化器 - 执行器

## 2. SQL Update

1. redo log

2. binlog

3. 两阶段提交

## 3. Transaction Isolation

1. Read Committed

2. Repeatable Read

3. MVCC

## 4/5. Index

1. Hash

2. B+Tree

3. 覆盖索引(避免回表)

4. 最左前缀

5. 索引下推

## 6. Locks

1. Flush tables with read lock(FTWRL)

2. Lock tables ... read/write

3. Metadata lock(MDL) [NOWAIT/WAIT n]

## 7. Row Locks

1. Two-phase locking - 2PL

2. Dead lock

3. Deadlock detect

## 8. MVCC

1. trx_id

2. current read : 更新数据是先读后写，只能读当前的值

## 9. Index and Unique Index

1. change buffer

a). Using change buffer: unique (x) index (v)

## 10. MySQL choose index

1. choose less rows

2. if 不需要回表

3. force index

## 11. Index string

1. all index and index(n)

2. index(n) will return to check

3. reverse column index, hash column index

## 12. 