##基本的SQL语句
###数据库CURD
* 创建数据库
```
create database [db_name];
```
* 查看所有数据库
```
show databases;
```
* 查询数据库的定义
```
show create database [db_name];
```
* 删除数据库
```
drop database [db_name];
```
* 切换数据库
```
use [db_name];
```
* 查看当前使用的数据库
```
select database();
```

###表CURD
* 创建表
```
create table [table_name](
	字段1 类型（长度） 约束，
	字段2 类型（长度） 约束，
	字段3 类型（长度） 约束
);
```
注意：每个完整的字段(包括类型和约束)之间用逗号隔开,如果类型是字符串，就必须加长度，int默认为11。

* 约束
 * 主键约束(primary key, 自增的话后面加上auto_increment)
 * 唯一约束(unique)
 * 非空约束(not null)
* 查看表的信息
```
desc [table_name];
```
* 查看当前数据库中的所有表名
```
show tables;
```
* 删除表
```
drop table [table_name];
```
* 修改表
```
//添加一个字段
alter table [table_name] add 字段 类型(长度) 约束;
//删除一个字段
alter table [table_name] drop 字段;
//修改字段的类型以及约束
alter table [table_name] modify 字段 类型(长度) 约束;
//修改字段名
alter table [table_name] modify 旧字段名 新字段名 类型(长度) 约束;
```
###数据CURD
* 添加数据
```
insert into [table_name] (字段1,字段2,···) value (值1,值2,···);
//每个字段都添加数据 (可以不用写字段名)
insert into [table_name] value (值1,值2,···);
```
* 修改数据
```
update [table_name] set 字段=值,字段=值,···· (where 字段=值);
```
例如：
```	
//将所有员工薪水修改为5000元。
update employee set salary=5000;			
//将姓名为"张三"的员工薪水修改为3000元。
update employee set salary=3000 where name="张三";
```
* 删除数据
```
delete from [table_name] (where 字段=值);
```
例如：
```
//删除表中名称为"张三"的记录
delete from user where username="张三";
//删除表中所有记录
delete from user;
```
* 查询数据
```
//查询所有字段
select * from [table_name];
//查询指定字段
select 字段1,字段2,字段3 from [table_name];
//含有where子句的查询
select * from [table_name] where 字段=值;
//使用order by对查询结果进行排序(升序 ——> asc, 降序 ——> desc)
select * from [table_name] order by 字段 [asc/desc]
注意：其中字段可以是多个运算，比如 字段1 + 字段2
```

* 聚合函数
 * count 获取数量
 ```
 select count(*) from [table_name] (where子句);
 ```
 * sum 求和
 ```
 select sum(字段 + 字段 + ...) from [table_name] (where子句);
 select sum(字段) + sum(字段) + ... from [table_name] (where子句);
 ```
注意：如果某个字段中含有null，应该使用第二种写法，因为第一种写法是以元组为单位求和的，如果某个元组中含有null，就会忽略该条记录，但是可以写为:
select sum(ifnull(字段,0) + ifnull(字段,0) + ...) from [table_name] (where子句);
 * avg 平均数
 ```
 select avg(字段) from [table_name];
 ```
 * max 最大值
 ```
 select max(字段) from [table_name];
 ```
 * min 最小值
 ```
 select min(字段) from [table_name];
 ```