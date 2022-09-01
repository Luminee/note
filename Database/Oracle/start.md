### Start

#### 1. Insert, Update, Delete

```sql
insert into tab1(col1, col2...) values(val1, val2...)
insert into tab1 values(val1, val2...)
insert into tab1 (select val1, val2... from tab2)

update tab1 set col1=a, col2=b... where ...

delete from tab1 where ...
drop table tab1
truncate table tab1
```

#### 2. Query

```sql
select [distinct] *, col1, col2... from tab1 where ...
select ename as "名字", sal*12 as "年薪" from tab1 where ...
# 别名不能用 单引号
select nvl(col1, 0) from tab1 // is_null(col1) ? 0 : col1
select ename||'年薪是'||sal*12 from tab1 # abc年薪是24000

# where
between 20 and 50 #[20,50]
like '%s%' # 带s 的， 0+
like '_s___' # 5个字符，且第二个是s
in (1,2,3) # 最多1000个值
is null || is not null
and, or
order by sal asc # desc, order by coo desc
max(), min, avg, sum, count
group by, having

#函数
ascii('a'), chr(65) #返回字符的十进制, 返回字符
concat('Hello', 'World') # HelloWorld
initcap('smith') # Smith
instr('oracle training', 'ra', 1, 2) #搜索字符位置
length() #字符串长度
lower(), upper() #字符全部小写，大写
rpad(), lpad() #左右添加字符
ltrim(), rtrim(), trim() #左右删除字符
substr() #截取字符
replace() #替换字符

# join
select a.*, b.* from a, b where a.id = b.a_id
select a.*, b.* from a inner join b on a.id = b.a_id
# left join, right join, full outer join
```

#### 3. Create

```sql
create user dev   # 用户名
  identified by "123" #密码
  default tablespace USERS; #表空间
```

