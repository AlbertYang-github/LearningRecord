# MySQL基础
---
## 数据库管理
- 查询所有的数据库
```
show databases;
```
MySQL中自带的几个数据库：
> information_schema： 元数据，基础数据<br/>
> mysql： 数据库配置，包含用户信息(用户名和密码，权限管理)<br/>
> performance_schema： 数据库软件的运行数据，日志信息，性能数据<br/>
> test： 测试数据库(空的)<br/>

- 创建数据库
```
-- 指定默认字符集
create database [数据库名] default character set utf8;
```
- 查看数据库的默认字符集
```
show create database [数据库名];
```
- 删除数据库
```
drop database [数据库名];
```
- 使用数据库
```
use [数据库名];
```

## 表管理
首先应该选择需要管理的表（use [数据库名];）
- 查看所有的表
```
show tables;
```
- 创建表
```
create table [表名] (
	[字段名](长度) [类型], 
	[字段名](长度) [类型], 
	[字段名](长度) [类型], 
	···
);
```
- 查看表结构
```
desc [表名];
```
- 删除表
```
drop table [表名];
```
- 修改表
 - 添加字段
 ```
 alter table [表名] add [字段名] [类型];
 ```

 - 删除字段
 ```
 alter table [表名] drop [字段名];
 ```

 - 修改字段类型
 ```
 alter table [表名] modify [字段名] [类型];
 ```

 - 修改字段名称
 ```
 -- 可以同时修改字段名称和字段类型
 alter table [表名] change [旧字段名] [新字段名] [类型];
 ```

 - 修改表名称
 ```
 alter table [旧表名] rename to [新表名];
 ```

## 数据管理
### 增删改数据
- 增加数据
```
-- 插入所有字段（必须按照表字段顺序插入）
insert into [表名] values([值1], [值2], ···);
-- 插入部分字段（按照指定字段顺序插入）
insert into [表名]([字段1], [字段2], ···) values([值1], [值2], ···); 
```

- 修改数据
```
-- 修改所有数据（会将给字段的所有记录的值修改）
update [表名] set [字段] = [值];
-- 带条件修改
update [表名] set [字段] = [值] where [字段] = [值];
-- 修改多个字段
update [表名] set [字段1] = [值1], [字段2] = [值2], ··· where [字段] = [值];
```

- 删除数据
```
-- 删除所有数据
delete from [表名];
-- 带条件删除
delete from [表名] where 字段 = 值;
-- 不能带条件删除，会删除数据和约束，删除的数据不能回滚
truncate table [表名];
```

### 查询数据
- 插叙所有列
```
select * from [表名];
```
- 查询指定列
```
select [字段1], [字段2],··· from [表名];
```
- 查询时添加常量列
```
select [字段1], [字段2],··· [常量列内容] as [别名] from [表名];
```
- 查询时合并列
```
select [字段1 + 字段2] as [别名] from [表名];
注意：合并列只能合并数值类型的字段
```

- 查询去除重复记录
```
select distinct [字段] from [表名];
```

- 条件查询
```
select * from [表名] where (条件);
```
> 逻辑条件： and(与) or(或)<br/>
> 比较条件： > < >= <= <>(不等于) between and(等价于>=且<=)<br/>
> 判空条件： where = is null;<br/>
> 判断空字符串： where [字段] = '';<br/>
- 模糊调节 (like)
%： 代表任意个字符(包括零个)
_： 代表一个字符

- 聚合函数
 - count 获取数量
 ```
 select count(*) from [表名] (where子句);
 ```
 - sum 求和
 ```
 select sum(字段 + 字段 + ...) from [表名] (where子句);
 select sum(字段) + sum(字段) + ... from [表名] (where子句);
 ```
 注意：如果某个字段中含有null，应该使用第二种写法，因为第一种写法是以元组为单位求和的，如果某个元组中含有null，就会忽略该条记录，但是可以写为:
 select sum(ifnull(字段,0) + ifnull(字段,0) + ...) from [表名] (where子句);
 - avg 平均数
 ```
 select avg(字段) from [表名];
 ```
 - max 最大值
 ```
 select max(字段) from [表名];
 ```
 - min 最小值
 ```
 select min(字段) from [表名];
 ```

- 分页查询
```
select * from [表名] limit [起始行] [需要查询的行数];
```
注意：起始行为0代表第一行

- 查询排序
语法：order by [字段] asc/desc<br/>
asc： 顺序（数值递增，字母a-z）<br/>
desc： 倒序（数值递减，字母z-a）<br/>
默认情况是按照插入顺序排序<br/>