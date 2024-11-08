# 数据类型
MySQL 支持以下数据类型组：

+ 数值
+ 字符串
+ 日期&时间
+ JSON

## 数值
### 整数
+ TINYINT：占用 1 字节
+ SMALLINT：占用 2 字节
+ MEDIUMINT：占用 3 字节
+ INT 或 INTEGER：占用 4 字节
+ BIGINT：占用 8 字节

### 浮点数
+ FLOAT：占用 4 字节，存储单精度浮点数
+ DOUBLE：占用 8 字节，存储双精度浮点数
+ DECIMAL 或 NUMERIC
    - 用于存储精确的数字，特别适合存储货币值
    - 可以指定总位数和小数位数，例如 DECIMAL(10, 2) 表示总共 10 位，其中 2 位是小数

## 字符串
### 字符类型
+ CHAR(n)：固定长度字符串，长度为 n，如果实际长度小于 n，则会用空格填充
+ VARCHAR(n)：可变长度字符串，最大长度为 n，只存储实际长度，不会填充空格

### 文本类型
+ TINYTEXT：最大长度为 255 字节的字符串
+ TEXT：最大长度为 65,535 字节的字符串
+ MEDIUMTEXT：最大长度为 16,777,215 字节的字符串
+ LONGTEXT：最大长度为 4,294,967,295 字节的字符串

### 二进制类型
+ BINARY(n)：固定长度的二进制字符串
+ VARBINARY(n)：可变长度的二进制字符串
+ TINYBLOB、BLOB、MEDIUMBLOB、LONGBLOB：用于存储二进制数据，分别对应不同的最大长度

## 日期和时间
+ DATE：存储日期，格式为 YYYY-MM-DD
+ DATETIME：存储日期和时间，格式为 YYYY-MM-DD HH:MM:SS
+ TIMESTAMP：存储时间戳，格式为 YYYY-MM-DD HH:MM:SS，范围是 1970-01-01 00:00:01 UTC 到 2038-01-19 03:14:07 UTC，它会根据时区自动调整
+ TIME：存储时间，格式为 HH:MM:SS
+ YEAR：存储年份，格式为 YYYY，范围是 1901 到 2155

# 表格操作
## 创建表
```sql
CREATE TABLE table_name (
    column1 datatype constraints,
    //constraints 是列的约束条件
    ...
);

CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    age INT,
    department VARCHAR(50),
    email VARCHAR(100) UNIQUE,
    hire_date DATE,
    salary DECIMAL(10, 2)
);
```

## 查看表
提供了表的详细结构信息，包括列的数据类型、是否可以为 NULL、默认值、键信息等

```sql
DESCRIBE table_name;
///===
DESC table_name;
```

## 修改表结构
操作部分有添加 ADD，删除 DROP，修改 MOTIFY

```sql
ALTER TABLE table_name 操作 COLUMN column datatype constraints;

//在 employees 表中添加了一个名为 phone_number 的列
ALTER TABLE employees ADD COLUMN phone_number VARCHAR(20);
```

## 删除表
使用 IF EXISTS 可以避免在表不存在时出现错误

```sql
DROP TABLE IF EXISTS table_name;

DROP TABLE IF EXISTS employees;
```

# 数据操作
## 插入数据
如果插入的数据与列的数据类型不匹配，MySQL会报错。

如果列设置了 NOT NULL 约束但未提供值，MySQL也会报错

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);

INSERT INTO employees (name, age, department)
VALUES ('John Doe', 30, 'Marketing');
```

## 查询数据
```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;//WHERE condition 是查询条件，用于过滤结果

//查询了 employees 表中所有属于市场部门的员工的 id、name 和 age
SELECT id, name, age
FROM employees
WHERE department = 'Marketing';
```

## 更新数据
```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;

//将 employees 表中名为 'John Doe' 的员工的年龄更新为 31
UPDATE employees
SET age = 31
WHERE name = 'John Doe';
```

## 删除数据
```sql
DELETE FROM table_name 
WHERE condition;

DELETE FROM employees
WHERE name = 'John Doe';
```

# 表达式
## 字面量
字面值是某种常量， 可以是字符串，数字，十六进制值，布尔值和 NULL

## 变量
变量以@字符开头

```sql
SET @name = 'Jane';

SELECT @name;
+-------+
| @name |
+-------+
| Jane  |
+-------+
```

## 运算符
### 算术运算符
用于执行基本的数学计算

+ %：取模，返回除法的余数
+ DIV：整除，返回除法的整数结果

```sql
SELECT 10 + 5, 20 - 4, 6 * 7, 100 / 25, 29 % 4, 10 DIV 3;
```

### 逻辑运算符
+ AND（逻辑与）
+ OR（逻辑或）
+ NOT（逻辑非）

```sql
SELECT * FROM employees WHERE age > 30 AND department = 'Marketing';
SELECT * FROM employees WHERE age < 25 OR salary > 5000;
```

### 比较运算符
比较运算符用于比较两个值，并返回一个布尔值

+ >=<
+ >=  <=
+ <> 或 !=（不等于）
+ BETWEEN（在某个范围内）
+ IN（在某个列表中）
+ IS NULL（等于 NULL）
+ LIKE（模式匹配）
+ REGEXP 或 RLIKE（正则表达式匹配）

```sql
SELECT * FROM employees WHERE age > 30;
SELECT * FROM employees WHERE name = 'John Doe';
SELECT * FROM employees WHERE department IN ('Marketing', 'Sales');
```





### 位运算符
对整数的二进制表示进行操作

+ &（位与）
+ |（位或）
+ ^（位异或）：只有当两个相应的位不同时，结果才为1
+ ~（位非）
+ <<（左移位）：二进制向左移动指定的位数，相当于将数值乘以2的指定次幂
+ >>（右移位）：二进制向右移动指定的位数，相当于将数值除以2的指定次幂

## SELECT
### 常用语句
+ 选择所有列：`SELECT ***** FROM table_name;`
+ 使用别名：`SELECT column1 **AS** alias_name FROM table_name;`
+ 条件查询：`SELECT column1 FROM table_name **WHERE** condition;`
+ 排序：`SELECT column1, column2 FROM table_name **ORDER BY** column1 ASC|DESC;`，ASC 升序，DESC 降序
+ 限制数量：`SELECT * FROM employees **LIMIT** 5;`，最多返回 5 行
+ 分组：`SELECT column1, COUNT(*) FROM table_name **GROUP BY** column1;`
+ 过滤：`SELECT column1, COUNT(*) FROM table_name GROUP BY column1 **HAVING** COUNT(*) > value;`

### 分页查询
使用SELECT查询时，如果结果集数据量很大，比如几万行数据，放在一个页面显示的话数据量太大，不如分页显示，每次显示100条

要实现分页功能，实际上就是从结果集中显示第1~100条记录作为第1页，显示第101~200条记录作为第2页，以此类推

因此，分页实际上就是从结果集中“截取”出第M~N条记录。这个查询可以通过 `LIMIT <N-M> OFFSET <M>`子句实现



在MySQL中，`LIMIT 15 OFFSET 30`可以简写成`LIMIT 30, 15`



例子：把结果集分页，每页3条记录

```sql
-- 查询第1页:
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 0;

-- 查询第2页:
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 3;

-- 查询第3页:
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 6;
```

### 多表查询
SELECT查询不但可以从一张表查询数据，还可以从多张表同时查询数据。查询多张表的语法是：SELECT * FROM <表1> <表2>

这种一次查询两个表的数据，查询的结果也是一个二维表![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730282379861-d90b91e3-33f0-481b-a7e5-481650a240c9.png)

要使用 **表名.列名 **这样的方式来引用列和设置别名，这样就避免了结果集的列名重复问题

```sql
SELECT
    students.id sid,
    students.name,
    classes.id cid,
    classes.name cname
FROM students, classes;
```

SQL还允许给表设置一个别名

```sql
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c;
```

多表查询也是可以添加WHERE条件的

```sql
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c
WHERE s.gender = 'M' AND c.id = 1;
```

### JOIN 连接查询
连接查询是另一种类型的多表查询。连接查询对多个表进行JOIN运算，简单地说，就是先确定一个主表作为结果集，然后，把其他表的行有选择性地“连接”在主表结果集上

#### 内连接
INNER JOIN 返回两个表中匹配的行，只有当两个表中都有匹配的行时，结果中才会出现

```sql
SELECT column1, column2, ...
FROM table1
INNER JOIN table2
ON table1.column_name = table2.column_name;

// 返回了所有有订单的客户名称和订单 ID
SELECT customers.name, orders.order_id
FROM customers
INNER JOIN orders
ON customers.id = orders.customer_id;
```

#### 左连接
LEFT JOIN 返回左表（第一个表）的所有行，即使右表（第二个表）中没有匹配的行。如果右表中没有匹配的行，则结果中右表的部分将为 NULL

```sql
SELECT column1, column2, ...
FROM table1
LEFT JOIN table2
ON table1.column_name = table2.column_name;

//返回了所有客户名称和他们的订单 ID
//包括那些没有订单的客户（结果中订单 ID 将为 NULL）
SELECT customers.name, orders.order_id
FROM customers
LEFT JOIN orders
ON customers.id = orders.customer_id;
```

#### 右连接
RIGHT JOIN 返回右表（第二个表）的所有行，即使左表（第一个表）中没有匹配的行。如果左表中没有匹配的行，则结果中左表的部分将为 NULL

```sql
SELECT column1, column2, ...
FROM table1
RIGHT JOIN table2
ON table1.column_name = table2.column_name;

//返回了所有订单和它们的客户名称
//包括那些没有客户的订单（结果中客户名称将为 NULL）
SELECT customers.name, orders.order_id
FROM customers
RIGHT JOIN orders
ON customers.id = orders.customer_id;
```

#### 全连接
FULL JOIN 返回两个表中所有行，无论它们是否匹配。如果一个表中没有匹配的行，则结果中该表的部分将为 NULL

```sql
SELECT column1, column2, ...
FROM table1
FULL OUTER JOIN table2
ON table1.column_name = table2.column_name;

SELECT customers.name, orders.order_id
FROM customers
FULL OUTER JOIN orders
ON customers.id = orders.customer_id;
```

# 子查询
## 概念
又称为内嵌查询或内部查询，是嵌套在另一个查询中的 SQL 查询

子查询可以返回单个值、多个值或值的集合，这些值可以被用来与外部查询进行比较或作为外部查询的一部分

注意：

1. 性能：子查询可能会影响查询性能，尤其是在子查询返回大量数据时
2. 单值要求：如果子查询用在 = 运算符的右侧，那么子查询必须返回单个值
3. 多值操作符：如果子查询返回多个值，可以使用 IN、ANY、ALL 或 EXISTS 等操作符
4. 可读性：虽然子查询功能强大，但过度使用可能会降低 SQL 语句的可读性。在某些情况下，将子查询重写为 JOIN 或其他形式可能更清晰

## 标量子查询
标量子查询返回单个值，通常用在 SELECT 列表或 WHERE 子句中

```sql
-- 找出员工表中薪水最高的员工的薪水
SELECT * FROM employees 
WHERE salary = (SELECT MAX(salary) FROM employees);
-- 子查询(SELECT MAX(salary) FROM employees)返回表中最高的薪水值
-- 外部查询返回所有薪水等于这个最高薪水的员工。
```

## 行子查询
行子查询返回单个行的多个值，通常与 IN 运算符一起使用

```sql
-- 找出所有部门名称和该部门薪水最高的员工的薪水
SELECT * FROM employees
WHERE (department, salary) IN (
    SELECT department, MAX(salary)
    FROM employees
    GROUP BY department
);
```

## 表子查询
表子查询返回多行单列或多列的值，通常作为一个派生表使用

```sql
-- 找出所有部门的员工数量
SELECT department, COUNT(*) AS num_employees
FROM employees
GROUP BY department
HAVING num_employees > (SELECT COUNT(*) FROM employees WHERE department = 'Sales');
```

# 约束
用于定义列的数据完整性规则，确保数据库中的数据满足特定的要求。约束可以帮助你维护数据的准确性和一致性，并且可以强制实施业务规则

## PRIMARY KEY 主键约束
唯一标识表中的每条记录，每个表只能有一个主键，主键列不能包含 NULL 值

```sql
CREATE TABLE table_name (
  column1 datatype PRIMARY KEY,
  ...
);
```

## FOREIGN KEY 外键约束
用于建立两个表之间的关系，确保引用的数据的完整性，外键列指向另一个表的键

```sql
CREATE TABLE table_name (
  ...
  column1 datatype,
  FOREIGN KEY (column1) REFERENCES parent_table(parent_key),
  ...
);
```

## UNIQUE 唯一约束
确保列中的所有值都是唯一的，允许 NULL 值，但每个 NULL 值只能出现一次

```sql
CREATE TABLE table_name (
  column1 datatype UNIQUE,
  ...
);
```

## NOT NULL 非空约束
确保列中的值不能为 NULL

```sql
CREATE TABLE table_name (
  column1 datatype NOT NULL,
  ...
);
```

## CHECK 检查约束
确保列中的值满足特定的条件

```sql
CREATE TABLE table_name (
  column1 datatype CHECK (condition),
  ...
);
```

## DEFAULT 默认值约束
为列指定默认值

```sql
CREATE TABLE table_name (
  column1 datatype DEFAULT value,
  ...
);
```

## INDEX 索引约束
提高查询效率，但不强制数据完整性，可以用于单个列或多个列

```sql
CREATE TABLE table_name (
  ...
  column1 datatype,
  INDEX index_name (column1),
  ...
);
```

## FULLTEXT 全文索引约束
用于全文搜索，只能用于 CHAR、VARCHAR 或 TEXT 类型的列

```sql
CREATE TABLE table_name (
  ...
  column1 datatype,
  FULLTEXT (column1),
  ...
);
```

# 函数
## 字符串函数
+ CONCAT(str1, str2, ...)：连接两个或多个字符串
+ LENGTH(str）或 CHAR_LENGTH（str）：返回字符串的长度
+ OWER(str）：将字符串转换为小写
+ UPPER(str）：将字符串转换为大写
+ TRIM(str）：去除字符串前后的空格
+ REPLACE(str, search_str, replace_str)：替换字符串中的某些字符

```sql
SELECT CONCAT('Hello', ' ', 'World');  -- 输出：Hello World
SELECT LENGTH('Hello World');  -- 输出：11
SELECT LOWER('HELLO WORLD');  -- 输出：hello world
SELECT UPPER('hello world');  -- 输出：HELLO WORLD
SELECT TRIM('  Hello World  ');  -- 输出：Hello World
SELECT REPLACE('Hello World', 'World', 'MySQL');  -- 输出：Hello MySQL
```

## 数值函数
+ ABS(value)：返回数值的绝对值
+ CEILING(value) 或 CEIL(value)：向上取整
+ FLOOR(value)：向下取整
+ ROUND(value, decimal_places)：将数值四舍五入到指定的小数位数
+ POWER(value, power) 或 POW(value, power)：计算数值的幂

```sql
SELECT ABS(-10);  -- 输出：10
SELECT CEILING(9.2);  -- 输出：10
SELECT FLOOR(9.8);  -- 输出：9
SELECT ROUND(123.456, 2);  -- 输出：123.46
SELECT POWER(2, 3);  -- 输出：8
```

## 日期和时间函数
+ NOW（）：返回当前日期和时 间
+ CURDATE（）：返回当前日期
+ CURTIMEO：返回当前时间
+ DATE_ADD(date，INTERVALinterval）：给日期增加指定的时间间隔
+ DATEDIFF(date1, date2)：计算两个日期之间的天数差

```sql
SELECT NOW();  -- 输出：当前日期和时间
SELECT CURDATE();  -- 输出：当前日期
SELECT CURTIME();  -- 输出：当前时间
SELECT DATE_ADD('2024-01-01', INTERVAL 1 MONTH);  -- 输出：2024-02-01
SELECT DATEDIFF('2024-01-01', '2023-12-31');  -- 输出：1
```

## 聚合函数
是 SQL 中用于对一组值执行计算并返回单个值的特殊函数，这些函数通常与 GROUP BY 子句一起使用，以便对数据进行分组和汇总

### COUNT() 
返回指定列非 NULL 值的数量

```sql
SELECT COUNT(column_name) FROM table_name;

SELECT COUNT(*) FROM employees;  -- 返回员工表中的总人数
SELECT COUNT(email) FROM employees;  -- 返回有非 NULL 邮箱地址的员工数量
```

### SUM()
计算指定列的数值总和

```sql
SELECT SUM(column_name) FROM table_name;

SELECT SUM(salary) FROM employees;  -- 返回所有员工薪水的总和
```

### AVG()
计算指定列的平均值

```sql
SELECT AVG(column_name) FROM table_name;

SELECT AVG(salary) FROM employees;  -- 返回所有员工薪水的平均值
```

### MAX()
返回指定列的最大值

```sql
SELECT MAX(column_name) FROM table_name;

SELECT MAX(salary) FROM employees;  -- 返回薪水最高的员工薪水
```

### MIN()
返回指定列的最小值

```sql
SELECT MIN(column_name) FROM table_name;

SELECT MIN(salary) FROM employees;  -- 返回薪水最低的员工薪水
```

### GROUP BY
用于将结果集分成多个组，每个组内的行具有相同的列值。聚合函数通常与 GROUP BY 一起使用，以便对每个组执行计算

```sql
SELECT column_name, AGGREGATE_FUNCTION(column_name)
FROM table_name
GROUP BY column_name;

-- 按部门分组并计算每个部门的平均工资
SELECT department, AVG(salary) AS average_salary
FROM employees
GROUP BY department;  
```

