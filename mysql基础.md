---
title: mysql的DDL与DCL与DML与DQL
date: 2024/5/24 23:45:21
tag: [mysql]
categories: mysql
description: mysql最基本的指令
top_img: https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240524233722.png
cover: https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240524234051.png
---

[TOC]



## 小把戏：

### 1.查看系统是否区分大小写

~~~
 show variables like 'lower_case_table_names';
~~~

1. **`lower_case_table_names = 0`**
   - 表名在存储和比较时都是区分大小写的。
2. **`lower_case_table_names = 1`**
   - 表名在存储和比较时都会被转换为小写。这意味着，如果你创建了一个名为 `MyTable` 的表，MySQL 会将其存储为 `mytable`，并且在引用它时也必须使用小写。
   - 这个设置通常在跨平台环境中使用，以确保表名的一致性，特别是当数据库在 Unix/Linux 和 Windows 之间迁移时。
3. **`lower_case_table_names = 2`**
   - 表名在存储时保持原样，但在比较时会被转换为小写。在 MySQL 5.6.3 及之后的版本中已被弃用，因为它可能导致混淆和不可预测的行为。

## 真常用：

### DDL（数据定义）

- 对库或者表进行操作的语句

##### 创建数据库：

~~~sql
create database db01;
~~~

##### 查看数据库(DQL)

~~~sql
show databases;
~~~

##### 查看建库语句

~~~sql
show create database db01;
~~~

##### 创建数据库的时候添加属性

```sql
create database db02 charset utf8;
```

##### 删除数据库

~~~sql
drop database db02;
~~~

##### 修改定义库

~~~sql
alter database db01 charset utf8;
~~~

##### 创建表(范例1)

~~~sql
create table student1(
sid int,
sname varchar(20),
sage tinyint,
sgender enum('m','f'),
comtime datetime
);
~~~

##### 创建表(范例2)

~~~
create table student2(
sid int comment '学号',
sname varchar(20) not null comment '学生姓名',
sage tinyint unsigned not null comment '年龄',
sgender enum('m','f') not null default 'm' comment '学生性别',
cometime datetime not null comment '入学时间'
)charset utf8 engine innodb;
~~~

##### 查看建表语句

```sql
show create table student2;
```

##### 查看表

~~~sql
show tables;
~~~

##### 查看表中列的定义信息（推荐）

~~~sql
desc student2;
~~~

##### 删除表

~~~sql
drop table student2;
~~~

##### 修改表名

~~~sql
alter table student rename stu;
~~~

##### 添加列（或多行列）和列数据类型的定义

~~~
alter table stu add age int;
alter table stu add test varchar(20),add qq int;
~~~

##### 指定位置进行添加列

~~~
# 表首
alter table stu add classid varchar(20) first;
# 指定列
alter table stu add phone2 int after sid;
~~~

##### 删除指定的列及定义

~~~
alter table stu drop qq;
~~~

##### 修改列及定义（列属性）

~~~
alter table stu modify sid varchar(20);
~~~

##### 修改列及定义（列名及属性)

~~~
alter table stu change phone telphone int();
~~~

### DCL（数据控制）

- DCL是针对权限进行控制

1. 授权

```sql
grant all on *.* to root@'192.168.175.%' identified by '123456';
# 授予root@'192.168.175.%'用户所有权限(非超级管理员)
grant all on *.* to root@'192.168.175.%' identified by '123456' with grant option;
# 授权一个超级管路员
with
max_queries_per_hour：一个用户每小时可发出的查询数量
max_updates_per_hour：一个用户每小时可发出的更新数量
max_connections_per_hour：一个用户每小时可连接到服务器的次数
max_user_connections：允许同时连接数量
```

2. 收回权限

```sql
revoke select on *.* from root@'192.168.175.%';
# 收回select权限
show grants for root@'192.168.175.%';
# 查看权限
```

3. with权限列举

```sql
ALTER USER 'root'@'192.168.175.%' WITH MAX_QUERIES_PER_HOUR 1000 MAX_UPDATES_PER_HOUR 500;
```

这将限制`'root'@'192.168.175.%'`用户每小时最多执行1000个查询和500个更新操作。

### DML(数据操作)

- 操作表中的数据

##### 更新数据

1. 规范update修改

~~~sql
update student set sgender='f' where sid=1;
~~~

2. 如果非要**全表修改**

~~~sql
update student set sgender='f' where 1=1;
~~~

##### 删除数据

~~~
delete from student where sid=3;
~~~

##### 删除表

```sql
truncate table student;
```

以下是关于`TRUNCATE TABLE`的一些关键点：

1. **速度**：通常，`TRUNCATE TABLE`比`DELETE`命令更快，因为它不记录任何单独的行删除操作。
2. **触发器**：`TRUNCATE TABLE`不会触发与行删除相关的DELETE触发器。
3. **权限**：执行`TRUNCATE TABLE`通常需要比执行`DELETE`更高的权限。
4. **锁定**：`TRUNCATE TABLE`通常会锁定表以防止在删除过程中进行其他操作。这可能会导致其他尝试访问该表的操作失败或被阻塞。

### DQL（数据查询）

- select：基础用法

  SELECT DISTINCT 字段1,字段2... FROM 表名

  WHERE 条件

  GROUP BY field 【列名】

  HAVING 筛选

  ORDER BY field

  LIMIT 限制条数

  ```sql
  mysql -uroot -p123 < world.sql
  # 常用用法
  select countrycode,district from city;
  # 常用用法
  select countrycode from city;
  # 查询单列
  select countrycode,district from city limit 2;
  select id,countrycode,district from city limit 2,2;
  # 行级查询
  select name,population from city where countrycode='CHN';
  # 条件查询
  select name,population from city where countrycode='CHN' and district='heilongjiang';
  # 多条件查询
  select name,population,countrycode from city where countrycode like '%H%' limit 10;
  # 模糊查询
  select id,name,population,countrycode from city order by countrycode limit 10;
  # 排序查询（顺序）
  select id,name,population,countrycode from city order by countrycode desc limit 10;
  # 排序查询（倒序 desc 正序 asc）
  select * from city where population>=1410000;
  # 范围查询(>,<,>=,<=,<>)
  select * from city where countrycode='CHN' or countrycode='USA';
  # 范围查询OR语句
  select * from city where countrycode in ('CHN','USA');
  # 范围查询IN语句
  select country.name,city.name,city.population,country.code from city,country where city.countrycode=country.code and city.population < 100;
  # 多表查询
  ```