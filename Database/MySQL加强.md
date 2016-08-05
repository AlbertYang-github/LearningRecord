# MySQL加强
---
## 数据约束
- **默认值** <br/>
写法：default [默认值] <br/>
作用：当用户不插入该字段值时就使用默认值 <br/>

- **非空** <br/>
写法：not null <br/>
作用：限制字段必须赋值 <br/>
注意： <br/>
1）非空字符必须赋值 <br/>
2）非空字符不能赋null

- **唯一** <br/>
写法：unique <br/>
作用：字段的值不能重复 <br/>
注意： <br/>
1）唯一字段可以插入null <br/>
2）唯一字段可以插入多个null

- **主键** <br/>
写法：primary key <br/>
作用：非空+唯一 <br/>
注意： <br/>
1）通常情况下，每张表都会设置一个主键字段，用于标记表中的每条记录的唯一性 <br/>
2）建议不要选择表的包含业务含义的字段作为主键，建议给每张表独立设计一个非业务含义的id字段

- **自增长** <br/>
写法：auto_increment <br/>
作用：自动增长，从0开始，zerofill零填充

- **外键** <br/>
作用：约束两个表的数据 <br/>
声明一个外键约束： <br/>
```
constraint [外键名称] foreign key ([外键]) references [参考表]([参考字段])
```
注意：<br/>
1）被约束的表称为副表，约束别人的表称为主表，外键设置在副表上 <br/>
2）主表的参考字段通用为主键 <br/>

## 外键用法示例
创建员工表emp
```
-- 创建员工表（在创建时声明外键，也可以在创建后声明，如下语法）
-- ALTER TABLE emp ADD CONSTRAINT emp_dept_fk FOREIGN KEY(deptId) REFERENCES dept(id);
-- emp_dept_fk：外键名称; deptId：外键; dept(id)：参考表(参考字段)
CREATE TABLE emp(
	id INT PRIMARY KEY,
	ename VARCHAR(10),
	deptId INT,
	CONSTRAINT emp_dept_fk FOREIGN KEY(deptId) REFERENCES dept(id)
);

-- 插入数据
INSERT INTO emp VALUES(1, '张三', 1);
INSERT INTO emp VALUES(2, '李四', 2);
INSERT INTO emp VALUES(3, '王五', 3);
```
创建部门表dept
```
CREATE TABLE dept(
	id INT PRIMARY KEY,
	dname VARCHAR(10)
);

-- 插入数据
INSERT INTO dept VALUES(1, '研发部');
INSERT INTO dept VALUES(2, '测试部');
INSERT INTO dept VALUES(3, '维护部');
```
员工表emp声明约束后，不符合约束条件的记录是无法插入的，例如：
```
INSERT INTO emp VALUES(4, '狗子', 4);
```

### 级联操作
** 当有了外键约束的时候，必须先修改或删除副表中的所有关联数据，才能修改或删除主表！但是，我们希望直接修改或删除主表数据，从而影响副表数据，可以使用级联操作实现。**
> 级联修改： ON UPDATE CASCADE <br/>
> 级联删除： ON DELETE CASCADE
```
-- 在创建表时添加级联操作
CREATE TABLE emp(
	id INT PRIMARY KEY,
	ename VARCHAR(10),
	deptId INT,
	CONSTRAINT emp_dept_fk FOREIGN KEY(deptId) REFERENCES dept(id) ON UPDATE CASCADE ON DELETE CASCADE
);
```

## 关联查询（多表查询）
查询员工姓名及所在部门 <br/>
- 内连接查询（使用别名），只有满足条件的结果才会显示(使用最频繁)
```
SELECT ename AS '姓名', dname AS '部门' FROM emp, dept WHERE emp.deptId = dept.id;
```
![](http://i.imgur.com/RsXRrcl.png)

- 内连接的另一种语法
```
SELECT ename AS '姓名', dname AS '部门' FROM emp INNER JOIN dept ON emp.deptId = dept.id;
```

- 左[外]连接查询： 使用左边表的数据去匹配右边表的数据，如果符合连接条件的结果则显示，如果不符合连接条件则显示null（注意：左外连接：左表的数据一定会完成显示）
```
SELECT ename AS '姓名', dname AS '部门' FROM emp LEFT OUTER JOIN dept ON emp.deptId = dept.id;
```
![](http://i.imgur.com/QroRB9x.png)

- 右[外]连接查询: 使用右边表的数据去匹配左边表的数据，如果符合连接条件的结果则显示，如果不符合连接条件则显示null（注意： 右外连接：右表的数据一定会完成显示）
```
-- 查询结果与上面的相同
SELECT ename AS '姓名', dname AS '部门' FROM dept RIGHT OUTER JOIN emp ON emp.deptId = dept.id;
```