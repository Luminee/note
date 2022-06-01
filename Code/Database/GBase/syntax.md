[TOC]

### DDL

#### Database

**Create Database**

```sql
create database [if not exists] <db_name> [in dbs_name] [with [buffered] log | with log mode ansi] [nlscasesensitive | nlscase insensitive];

create database demo_db1;
```

**Rename Database**

```sql
rename database <old_db_name> to <name_db_name>;

rename database demo_db1 to demo_db2;
```

**Drop Database**

```sql
drop database [if exists] <db_name>;

drop database demo_db2;
```

#### Table

**Create Table**

```sql
create [standard | raw] table [if not exists] <table_name> (colname1 data_type1, colname2 data_type2, ...);

create table t_user1(f_userid int, f_username varchar(20));
```

**Rename Table**

```sql
rename table <old_table_name> to <new_table_name>;

rename table t_user1 to t_user2;
```

**Drop Table**

```sql
drop table [if exists] <table_name>;

drop table t_user2;
```

#### Column

**Add Column**

```sql
alter table <table_name | synonym_name> 
add (new_column_name1 data_type1 [before old_column_name1] [, new_column_name2 data_type2 [before old_column_name2], ...]);

alter table t_user add (f_nickname varchar(20));
```

**Rename Column**

```sql
rename column <table_name>.<old_column_name> to <new_column_name>;

rename column t_user.f_nickname to f_showname;
```

**Drop Column**

```sql
alter table <table_name | synonym_name> drop (old_column_name1[, old_column_name2, ...]);

alter table t_user drop (f_showname);
```

#### View

**Create View**

```sql
create view [if not exists] <view_name> as <query_define>;

create view vw_user as select * from t_user;
```

**Drop View**

```sql
drop view if exists <view_name>;

drop view vw_user;
```

#### Index

**Create Index**

```sql
create [unique | distinct | cluster] index [if not exists] <idx_name> 
on <table_name | synonym_name> (<column_name | func_name(column_name)> [asc | desc]);

create unique index idx_user on t_user (f_userid asc);
```

**Rename Index**

```sql
rename index <old_index_name> to <new_index_name>;

rename index idx_user to idx_user01;
```

**Drop Index**

```sql
drop index [if exists] <idx_name>;

drop index idx_user01;
```

#### Procedure

**Create Procedure**

```sql
create procedure [if not exists] <procedure_name>(param1 data_type1, param2 data_type2, ...)
<spl code>
end procedure;
------
create procedure up_city_add(cityid int, cityname varchar(20))

insert into t_city(f_cityid, f_cityname) values(cityid, cityname);
end procedure;
```

**Drop Procedure**

```sql
drop procedure [if exists] <procedure_name> [(data_type1, data_type2, ...)];

drop procedure up_city_add;
```

#### Function

**Create Function**

```sql
create function [if not exists] <function_name>(param1 data_type1, param2 data_type2, ...)
returning data_type1 [as var1] [, data_type2 [as var2], ...]
<spl code>
return var1[, var2, ...];
end function;
------
create function fn_get_ymd(dt date)
returning int as year, int as month, int as day

define y,m,d int;

let y = year(dt);
let m = month(dt);
let d = day(dt);

return y,m,d;
end function;
```

**Drop Function**

```sql
drop function [if exists] <function_name> [(data_type1, data_type2, ...)];

drop function fn_get_ymd;
```

#### Synonym

**Create Synonym**

```sql
create [public | private] synonym [if not exists] <synonym_name> for <table_name | view_name | sequence_name>;

create synonym syn_user for t_user;
```

**Drop Synonym**

```sql
drop synonym [if exists] <synonym_name>;

drop synonym syn_user;
```

#### Trigger

**Create Trigger**

```sql
create [or replace] tirgger <trigger_name> <insert | update [of column_name] | delete | select> on <target_table_name>
<before | after | for each row>
when <condition>
<action>
---------
create or replace trigger trg_sale_insert insert on t_sale
referencing new as new_item
for each row 
(
insert into t_log(f_message, f_operatedate) values(concat('insert:', new_item.f_productname), today)
);
```

**Drop Trigger**

```sql
drop trigger <trigger_name>;

drop trigger if exists trg_sale_insert;
```



### DML

#### Insert

```sql
insert into <table_name | view_name | synonym_name> [column_name1, column_name2, ...] values(value1, value2, ...);

insert into t_user1 values(1, 'gbasedbt');
insert into t_user2 select * from t_user1;
```

#### Update

```sql
update <table_name | view_name | synonym_name> set column_name1 = value1[, column_name2 = value2, ... ] [where condition];

update t_user set f_age = 20 where f_userid = 1;
```

#### Delete

```sql
delete from <table_name | view_name | synonym_name> [where condition];

delete from t_user where f_userid = 1;
```

#### Merge

```sql
merge into <target_table_name> as t
using <source_table_name | source_query> as s
on t.column_name1 = s.column_name2
when matched then update set t.col_1 = s.col_1, t.col_2 = s.col_2, ...
when not matched then insert (t.col_1, t.col_2, ...) values(s.col_1, s.col_2, ...);
-------
merge into t_user1 a
using t_user2 b
on a.f_userid = b.f_userid
when matched then update set a.f_age = b.f_age
when not matched then insert (a.f_userid, a.f_username, a.f_age) values(b.f_userid, b.f_username, b.f_age);
```



### DQL

#### Select

```sql
select [first n] <* | column_name1[, column_name2, ...]> from <table_name>;
select <column_name1[, column_name2, ...], aggr_func1(column_name_n1)[, aggr_func2(column_name_n2), ...]>
from <table_name>
group by column_name_n1[, column_name_n2, ...]
[order by column_name_m1 [asc | desc][, column_name_m2  [asc | desc], ...]];
------
select first 3 * from t_employee;
select f_deptid, max(f_salary) as f_salary from t_employee group by f_deptid order by f_salary desc;
```

```sql
select distinct age ...;
select unique age ...;
where column [!= | <>]
select top m, n * from ...;
select skip m first n * from ...;
select column as col ...;
```



#### Join

```sql
select * from <table_name> a, <table_name> b where a.f_column_name1 = b.f_column_name2;

select a.*, b.f_deptname as f_parentname from t_dept a, t_dept b where a.f_parentid = b.f_deptid;
```

```sql
selct table_name1.column_name1, table_name1.column_name2, ... ,
      table_name2.column_name1, table_name2.column_name2, ... 
from <table_name1> a
<inner | left outer | right outer | full outer > join <table_name2> b
on a.column_name1 = b.column_name2;
-------
select a.f_employeeid, a.f_employeename, b.f_deptname
from t_employee a
inner join t_dept b
on a.f_deptid = b.f_deptid;
```

#### Sub Query

```sql
where column_name1 in (select column_name2 from <table_name2>);
where column_name1 in <(val1, val2, ...)>;
where exists (select 1 from <table_name2> where table_name1.column_name1 = table_name2.column_name2)
-------
where f_deptid in (select f_deptid from t_dept);
where f_deptid in (3, 5);
where exists (select 1 from t_dept b where b.f_deptid = a.f_deptid);
```

