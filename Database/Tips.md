1. 数据库 `id`, `bigint unsigned` 自增 1

2. 业务上具有唯一特性的字段，即使是多个字段的组合，也必须建成唯一索引

3. 超过三个表禁止 `join`。需要 `join` 的字段，数据类型必须绝对一致, 关联字段需要索引

4. MySQL 并不是跳过 `offset` 行，而是取 `offset` + `N` 行，然后返回放弃前 `offset` 行，返回 `N` 行，那当 `offset` 特别大的时候，效率就非常的低下

5. 至少要达到 `range` 级别，要求是 `ref` 级别，如果可以是 `consts` 最好

    a. `consts` 单表中最多只有一个匹配行（主键或者唯一索引），在优化阶段即可读取到数据。

    b. `ref` 指的是使用普通的索引（normal index）。

    c. `range` 对索引进行范围检索。

6. 建组合索引的时候，区分度最高的在最左边, 区分度的公式是 `COUNT(DISTINCT col) / COUNT(*)`

7. 使用 `ISNULL()` 来判断是否为 `NULL` 值。`NULL` 与任何值的直接比较都为 `NULL`。

8. 永远小表驱动大表是最优的选择方式

    a. 表A（小表），表B（大表）

    b. `select * from A where cc in (select cc from B)` [效率低，用到了A表上cc列的索引]

    c. `select * from A where exists(select cc from B where cc=A.cc)` [效率高，用到了B表上cc列的索引]

    d. `select * from B where cc in (select cc from A)` [效率高，用到了B表上cc列的索引]

    e. `select * from B where exists(select cc from A where cc=B.cc)` [效率低，用到了A表上cc列的索引]

    f. 无论那个表大，用 `not exists` 都比 `not in` 要快

9. 单张表索引数量建议控制在5个以内, 组合索引最左前缀原则

10. 指定字段能有效利用索引覆盖

11. 负向查询 `!=`, `<>` 不能命中索引, 前导模糊查询 `like %keyword` 不能使用索引

12. 控制 in 的值在 100 以内

13. 尽量选择区分度高的列作为索引，区分度的公式是 `COUNT(DISTINCT col) / COUNT(*)`

14. 与 SQL 标准不同的地方在于InnoDB 存储引擎在 **REPEATABLE-READ(可重读)** 事务隔离级别下使用的是Next-Key Lock 锁算法，因此可以避免幻读的产生，这与其他数据库系统(如 SQL Server)是不同的

> 1. Adjacency List 邻接表
> 2. Path Enumerations 路径枚举 (维护 更新上层路径)
> 3. Nested Sets 嵌套集合 (左右节点，维护困难)
> 4. Closure Table 闭合表 (需要大表维护各节点关系)
> 5. 前序遍历

