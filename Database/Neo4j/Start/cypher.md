### Cypher - CQL

#### 1. Order

- `Create` 创建 - 创建节点，关系，属性
- `Match` 匹配 - 检索节点，关系，属性
- `Return` 返回 - 返回查询结果
- `Where` 哪里 - 条件过滤
- `Delete` 删除 - 删除节点，关系
- `Remove` 移除 - 删除节点和关系的属性
- `Order By` 排序 - 排序数据
- `Set` 设置 - 添加或更新标签

#### 2. Function

- `String` 字符串 - 处理字符串
  - `id(r)`,`type(r)`,`labels(n)`,`keys(n)`,`properties(n)`
- `Aggregation` 聚合 -  执行聚合操作
- `Relationship` 关系 - 获取关系细节

#### 3. Data Type

- `boolean` 布尔 true, false
- `byte` 8位整数
- `short` 16位整数
- `int` 32位整数
- `long` 64位整数
- `float` 32位浮点数
- `double` 64位浮点数
- `char` 16位字符
- `string` 字符串

#### 4. Path

- `(a)-->(b)` 步长为1的路径，a与b有直接关系
- `(a)-->()-->(b)` 步长为2
- `(a)-[*2]->(b)` 路径长度为2
- `(a)-[*3..5]->(b)` 最小3，最大5；
-  `[*..5], [*3..], [*]` 最大5；最小3；不限制长度
- `p = (a)-[*3..5]->(b)` 路径变量