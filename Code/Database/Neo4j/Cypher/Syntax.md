### 1. Values and types 值和类型

- **Property types** 属性类型 (_return_, _use as parameters_, _store as properties_)
  - int, float, string, boolean, point, date, time
- **Structural types** 结构类型 (_return_)
  - node : (id, labels, map) 
  - relationship (id, type, map, id of start node, id of end node)
  - path 节点和关系交替序列
- **Composite types** 复合类型 (_return_, _use as parameters_)
  - list, (ordered collection)
  - map (key - value pairs)

### 2. Naming rules 命名规则

- 字符开头
- 可包含数字
- 下划线，或`$`开头
- 最长 65535
- 区分大小写

#### 3. Expressions 表达式

- 十进制 / 科学计数法 / 八进制 / 十六进制

- 字符串文字

  - `\t`,`\n`,`\r`,`\\`, e.g.

- 布尔

- 变量

- 属性 / 动态属性

- 参数

- 表达式列表

- 函数调用 / 聚合函数 / 路径

- 运算 (1 + 2, 3 < 4) / 判断 / 正则

- 子查询

- CASE 表达式

  - ```cypher
    CASE test
    	WHEN value THEN result
    	[WHEN ...]
        [ELSE default]
    END
    ```

  - ```cypher
    CASE
    	WHEN predicate THEN result
    	[WHEN ...]
        [ELSE default]
    END
    ```

#### 4. Variables 变量

> 变量只在相同query 部分可见，不会结转到后续查询中。
>
> 如果链接了多个查询部件，必须在 `WITH`子句中列出与一起使用的变量，以便结转到下一部分。

#### 5. Reserved keywords 保留字

- Clauses
  - `CALL`
  - `CREATE`,`DELETE`,`REMOVE`
  - `DETACH`
  - `EXISTS`
  - `FOREACH`,`LOAD`,`SET`
  - `MATCH`,`MERGE`
  - `OPTIONAL`
  - `RETURN`,`WITH`
  - `START`,`UNION`,`UNWIND`
- Subclauses
  - `LIMIT`,`ORDER`
  - `SKIP`,`YIELD`
  - `WHERE`
- Modifiers
  - `ASC`,`DESC`,`ASCENDING`,`DESCENDING`
  - `ASSERT`,`BY`,`CSV`,`ON`
- Expressions
  - `ALL`
  - `CASE`,`ELSE`,`END`,`THEN`,`WHEN`
- Operators
  - `AND`,`OR`,`XOR`
  - `AS`,`CONTAINS`,`DISTINCT`,`ENDS`
  - `IN`,`IS`,`NOT`,`STARTS`
- Schema
  - `CONSTRAINT`
  - `CREATE`,`DROP`
  - `EXISTS`,`INDEX`,`NODE`,`KEY`,`UNIQUE`
- Hints
  - `INDEX`,`JOIN`,`PERIODIC`
  - `COMMIT`,`SCAN`,`USING`
- Litrals
  - `false`,`null`,`true`

#### 6. Parameters 参数

