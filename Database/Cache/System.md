#### 1). %SYS

##### 1. Sys

- `SYS.Process` 类似 ` show processlist`  (ProcessQuery)

- `SYS_History` 

##### 2. Security

- `Security.Users` 系统用户表，保存用户名和密码
- `Security.SQLPrivileges` SQL权限分配
- `Security.SQLAdminPrivilegeSet` SQL特权
- `Security.Services` 服务管理
- `Security.Roles` 角色管理
- `Security.Resources` 资源管理
- `Security.Events` 系统事件 (审计用)
- `Security.Applications` 应用程序



#### 2). InformationSchema

##### 1. information_schema

- `SCHEMATA` 所有schema

- `TABLES` 所有表
- `COLUMNS` 表字段
- `INDEXES` 表索引
- `TABLE_CONSTRAINTS` 表约束(索引)
- `CHECK_CONSTRAINTS ` 检查约束
- `CONSTRAINT_TABLE_USAGE` 约束表
- `CONSTRAINT_COLUMN_USAGE` 约束字段
- `KEY_COLUMN_USAGE` 索引列
- `REFERENTIAL_CONSTRAINTS ` 引用约束
- `ROUTINES` 存储过程
- `PARAMETERS` 存储过程参数
- `TRIGGERS` 触发器

- `VIEWS` 视图定义
- `VIEW_COLUMN_USAGE` 使用列
- `VIEW_TABLE_USAGE` 使用表
- `STATEMENTS ` 执行历史
- `STATEMENT_LOCATIONS` 执行语句存储文件