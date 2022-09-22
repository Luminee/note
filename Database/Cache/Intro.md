#### Cache

##### 1. Intro

1. Cache 以多维数组存储

2. 提供M语言进行类定义、方法定义
3. Oracle语系 (字段可用双引号、分页用 %VID、Schema.Table)
4. 与Oracle 不同: 可用Top
5. 不能跨 Namespace 查询
6. 不支持 `PARTITION BY` 语句



##### 2. 三种查询方式

1. 对象数据库访问

```c#
Obj = ##class(User.Title).%OpenId(Id)
```

2. 多维数组访问

```c#
Code = $Piece($Get(^CT("TTL",RowId)),"^",1)
```

3. SQL访问

```sql
&SQL(
	SELECT RowId, Code, Desc
	INTO :RowId, :Code, :Desc
	FROM User.Title Where RowId = :Id
)
```

> 用处：
>
> - SQL 语法用在SQL窗口
> - 对象访问用在M语言
> - 多维数组用在多维数组字段(类Json字段)、

##### 3. SQL语法

```sql
SELECT TOP 5 Name, %ID, %TABLENAME, %CLASSNAME, 'A' || 'B' as ab From Sample.Person
WHERE FOR SOME (children) AS c 
	(c.%Key = 'betty' AND c.%Value > 5 AND
     FOR SOME %ELEMENT(toppings) AS t
     	(t.%VALUE = 'Butterscotch' AND c.%KEY = t.%KEY))
    AND legalBrief %CONTAINS ('neighbor', 'tree', 'roof')
```

```sql
SELECT *, %VID FROM (
    SELECT TOP 5 Age FROM Sample.Person) As t
WHERE %VID > 3
```



##### 4. DDL

```sql
CREATE TABLE my_test (
	MyId INT PRIMARY KEY,
    LastName VARCHAR(20) CONSTRAINT unq_lname UNIQUE CONSTRAINT nonull_lname NOT NULL,
    FirstName VARCHAR(20) DEFAULT '***TBD***',
    DateStamp DATETIME DEFAULT CURRENT_TIMESTAMP
)
INSERT INTO my_test(MyId,LastName,FirstName) VALUES (1224,'Smith','John')
DROP TABLE my_test
```

> - #table  临时表
> - 不带schema 则schema = SQLUser

##### 5. Transaction

```sql
BEGIN TRANSACTION [name]
COMMIT [TRANSACTION | WORK]
ROLLBACK [TRANSACTION | WORK ] [name]
```

> [name] 可选，作为 savepoint
>
> COMMIT 提交当前事务
>
> ROLLBACK 回滚所有事务， ROLLBACK name 回滚至命名事务，如果事务不存在或已回滚，则回滚当前事务