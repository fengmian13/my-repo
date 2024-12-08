# 如何写sql

## 基础语句

#### 建表

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    position VARCHAR(50),
    hire_date DATE
);
```

删表

```sql
DROP TABLE [IF EXISTS] table_name;
```

修改表

- **添加列**:

```sql
ALTER TABLE table_name
ADD column_name datatype [constraints];
```

- **修改列**:

```sql
ALTER TABLE table_name
MODIFY column_name new_datatype [new_constraints];
```

- **重命名列** (不同数据库可能有不同的语法):

```sql
-- MySQL 语法
ALTER TABLE table_name
CHANGE old_column_name new_column_name new_datatype [new_constraints];

-- PostgreSQL 语法
ALTER TABLE table_name
RENAME COLUMN old_column_name TO new_column_name;
```

- **删除列**:

```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```

- **重命名表**:

```sql
ALTER TABLE old_table_name
RENAME TO new_table_name;
```

#### 修改数据 (UPDATE)

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

### 删除数据 (DELETE)

```sql
DELETE FROM table_name
WHERE condition;
```

---

---

## SQL基础

### **select**

#### 使用 SELECT COLUMN, COLUMN 查询多个列

请编写 SQL 语句，从课程表 `courses` 中获取课程名称 `name` 和上课学生人数 `student_count` 的列。

表定义：courses（课程表）

| **列名**      | **类型** | **注释** |
| :------------ | :------- | :------- |
| id            | int      | 主键     |
| name          | varchar  | 课程名称 |
| student_count | int      | 学生总数 |
| created_at    | date     | 开课时间 |
| teacher_id    | int      | 讲师 id  |

```mysql
SELECT name , student_count from courses;
```



----

#### 使用 SELECT DISTINCT 查询唯一不同的值

请编写 SQL 语句，查询教师表 `teachers` 中的不重复的教师国籍（country）。
表定义: teachers (教师表)

| **列名** | **类型** | **注释** |
| :------- | :------- | :------- |
| id       | int      | 主键     |
| name     | varchar  | 讲师姓名 |
| email    | varchar  | 讲师邮箱 |
| age      | int      | 讲师年龄 |
| country  | varchar  | 讲师国籍 |

```mysql
SELECT distinct country from teachers
```

-----

#### 使用 SELECT WHERE 对行进行筛选过滤

请编写 SQL 语句，查询课程表 `courses` 中，学生人数超过 1000 的全部课程信息。

表定义：courses

| **列名**      | **类型** | **注释** |
| :------------ | :------- | :------- |
| id            | int      | 主键     |
| name          | varchar  | 课程名称 |
| student_count | int      | 学生总数 |
| created_at    | date     | 开课时间 |
| teacher_id    | int      | 讲师 id  |

```MySQL
SELECT * FROM courses where student_count > 1000;
```

----

### **insert**

#### 使用 INSERT INTO 在不指定列的情况下插入数据

请编写 SQL 语句，向课程表 `courses` 中插入一条新的课程记录，该记录内容如下：

| id   | name | student_count | created_at | teacher_id |
| :--- | :--- | :------------ | :--------- | :--------- |
| 14   | SQL  | 200           | 2021-02-25 | 1          |

表定义：courses

| 列名          | 类型    | 注释     |
| :------------ | :------ | :------- |
| id            | int     | 主键     |
| name          | varchar | 课程名称 |
| student_count | int     | 学生总数 |
| created_at    | date    | 开课时间 |
| teacher_id    | int     | 讲师 ID  |

```mysql
insert into `courses` value(14,"SQL",200,"2021-02-25",1);
#char varchar 等字符类型使用"?"双引号括起来
```

----

#### 使用 INSERT INTO 在指定的列中插入数据

语法如下：

```mysql
INSERT INTO `table_name`
(`column1`, `column2`, `column3`,...)
VALUES (value1, value2, value3,...);
```

请编写 SQL 语句，向教师表 `teachers` 插入一条新的教师记录，该记录指定列的字段内容如下：

| name   | email                                             | age  | country |
| :----- | :------------------------------------------------ | :--- | :------ |
| XiaoFu | [XiaoFu@lintcode.com](mailto:XiaoFu@lintcode.com) | 20   | CN      |

```mysql
insert into teachers (`name`,`email`,`age`,`country`)
value ("XiaoFu", "XiaoFu@lintcode.com", 20, "CN");
```

---

### update

#### 使用 UPDATE 更新数据

**语法**

```sql
UPDATE `table_name`
SET `column1`=value1,`column2`=value2,...
WHERE `some_column`=some_value;
```

请编写 SQL 语句，将课程表 `courses` 中人工智能课 (Artificial Intelligence) 的学生人数修改为 500 人。

表定义: courses

| **列名**      | **类型** | **注释** |
| :------------ | :------- | :------- |
| id            | int      | 主键     |
| name          | varchar  | 课程名称 |
| student_count | int      | 学生总数 |
| created_at    | date     | 开课时间 |
| teacher_id    | int      | 讲师 ID  |

```sql
update courses
set student_count = 500
where name = "Artificial Intelligence";
```

----

### delete

语法：

```sql
DELETE FROM table_name
WHERE condition;
```

假设我们有一个名为 `employees` 的表，并且我们想要删除所有职位为 'Manager' 的员工记录，我们可以这样写：

```sql
DELETE FROM employees
WHERE position = 'Manager';
```

-----



## SQL之**运算、筛选、排序**

#### 比较运算符

常用的比较运算符有 `=`（等于） 、`!=`（不等于）、 `<>`（不等于）、`<`（小于）、`<=`（小于等于）、`>`（大于）、`>=`（大于等于），其中 `!=` 和 `<>` 在特殊情况下用法是不同的。

**`!=` 和 `<>` 都是用于表示“不等于”的运算符** ：在几乎所有的情况下，`!=` 和 `<>` 的行为是相同的。但是，有一些特殊情况需要注意。

1. **NULL 值**：当与 `NULL` 比较时，`!=` 和 `<>` 的结果总是 `UNKNOWN`，这是因为 `NULL` 在 SQL 中代表未知值。因此，`column != NULL` 或 `column <> NULL` 都不会返回任何行，即使列中确实包含 `NULL` 值。要检查 `NULL` 值，应该使用 `IS NULL` 或 `IS NOT NULL`。

   ```
   -- 错误的方式
   SELECT * FROM table_name WHERE column != NULL;
   
   -- 正确的方式
   SELECT * FROM table_name WHERE column IS NOT NULL;
   ```

2. **三值逻辑**：由于 SQL 使用三值逻辑（TRUE, FALSE, UNKNOWN），当比较表达式的结果为 `UNKNOWN` 时，它不会被视为 `TRUE` 或 `FALSE`。这会影响到 `WHERE` 子句中的过滤条件。例如，如果你有一个 `WHERE` 条件是 `column1 != column2 OR column1 = column2`，如果 `column1` 或 `column2` 包含 `NULL`，那么这个条件可能会返回 `UNKNOWN`，导致该行不被选中。

---

#### AND 连接多条件

使用 SQL 中的逻辑运算符 AND 可以将 WHERE 子句中两个或两个以上的条件结合起来，其结果是满足 AND 连接的所有条件的数据。

**语法**

```sql
SELECT `column_name` 
FROM `table_name` 
WHERE condition1 AND condition2;
```

----

#### OR 连接多个条件

------

使用 SQL 中的逻辑运算符 OR 与 AND 关键字不同，OR 关键字，只要记录满足任意一个条件，就会被查询出来。

**语法**

```sql
SELECT `column_name` 
FROM `table_name` 
WHERE condition1 or condition2;
```

-----

####  NOT 过滤不满足条件的数据

------

使用 SQL 中的逻辑运算符 NOT 可以过滤掉 WHERE 子句中不满足条件的结果集。

**语法**

```sql
SELECT `column_name` 
FROM `table_name` 
WHERE NOT `condition`;
```

----

#### IN 查询多条件

**IN** 用法：

```sql
SELECT *
FROM `table_name`
WHERE `column_name` IN `value`;
```

---

#### NOT IN 排除

'NOT IN'，表示不在集合中的所有结果。

**NOT IN** 用法：

```sql
SELECT *
FROM `table_name`
WHERE `column_name` NOT IN value;
```

请编写 SQL 语句，查询课程表 `courses` 中所有教师 id `teacher_id` 不为 1 或 3 的所有课程，并返回满足查询条件的课程名称。

`courses` 表定义如下：

|     列名      |  类型   |   注释   |
| :-----------: | :-----: | :------: |
|      id       |   int   |   主键   |
|     name      | varchar | 课程名称 |
| student_count |   int   | 学生总数 |
|  created_at   |  date   | 开课时间 |
|  teacher_id   |   int   | 讲师 ID  |

```sql
select name from courses where teacher_id not in (1,3)
```

----

####  BETWEEN AND 查询两值间的数据范围

**BETWEEN AND** 会选取介于两个值之间的数据范围。这些值可以是数值、文本或者日期。

**BETWEEN AND** 用法：

```sql
SELECT *
FROM `table_name`
WHERE `column_name` BETWEEN `value` AND `value`;
```

---

####  使用 IS NULL 查询空数据

NULL 值代表遗漏的未知数据。默认的，表的列可以存放 NULL 值。

**IS NULL** 用法：

```sql
SELECT *
FROM `table_name`
WHERE `column_name` IS NULL;
```

请编写 SQL 语句，查询教师表 `teachers` 中，国籍为 'CN' 或 'JP' 且 `email` 信息不为空的所有教师信息。

表定义: teachers (教师表)

|  列名   |  类型   |   注释   |
| :-----: | :-----: | :------: |
|   id    |   int   |   主键   |
|  name   | varchar | 讲师姓名 |
|  email  | varchar | 讲师邮箱 |
|   age   |   int   | 讲师年龄 |
| country | varchar | 讲师国籍 |

```sql
select * from teachers WHERE country in ('CN','JP') and !(email is null);不用
select * from teachers WHERE country in ('CN','JP') and email is not null;
```

---

####  LIKE 模糊查询

SQL 支持两种主要的通配符：

- **%**：代表零个、一个或多个字符。
- **_**（下划线）：代表单个字符。

**LIKE** 用法：

```sql
SELECT *
FROM `table_name`
WHERE `column_name` LIKE  `value`;
```

`LIKE` 查询，尤其是以通配符开始的模式（如 `'%abc'`），可能会导致全表扫描，因为索引不能有效地利用前缀通配符。这种查询可能会影响性能，尤其是在大表上。

- 尽量避免以通配符开头的模式。
- 使用全文索引（如果数据库支持并且适合你的用例）。
- 对经常用于 `LIKE` 查询的列建立索引。
- 如果可能，限制结果集的大小，比如通过添加其他过滤条件或使用 `LIMIT` 子句。

----

#### ORDER BY排序查询

它可以按照一个或多个列来对结果集进行升序（ASC）或降序（DESC）排序。如果未指定排序顺序，默认是升序（ASC）。

**基本语法**

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC], ...;
```

**注意事项**

- `ORDER BY` 必须是 `SELECT` 语句中的最后一个子句，除了 `LIMIT`、`OFFSET` 和 `FETCH` 等分页相关的子句。
- 如果你的查询涉及联合（`JOIN`），确保 `ORDER BY` 只作用于最终的结果集，而不会影响联结的操作。



编写 SQL 语句，查询教师表 `teachers` 中教师年龄 `age` 的唯一值，并将结果按照年龄 `age` 进行升序排序。
表定义: teachers (教师表)

| **列名** | **类型** | **注释** |
| :------- | :------- | :------- |
| id       | int      | 主键     |
| name     | varchar  | 讲师姓名 |
| email    | varchar  | 讲师邮箱 |
| age      | int      | 讲师年龄 |
| country  | varchar  | 讲师国籍 |

```sql
SELECT DISTINCT age FROM teachers order by age;
```

---

#### LIMIT限制查询

**用法：**

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition
ORDER BY column1 [ASC|DESC]
LIMIT number_of_rows OFFSET offset_value;
```

- `number_of_rows`：指定要返回的最大行数。
- `offset_value`：可选参数，指定从哪一行开始返回数据。默认是从第一行（偏移量为 0）开始。

请编写 SQL 语句，从教师表 `teachers` 中查询一条年龄最大的并且国籍为中国（对应的 `country` 值为 `CN`）的教师的信息。

表定义: teachers (教师表)

|  列名   |  类型   |   注释   |
| :-----: | :-----: | :------: |
|   id    |   int   |   主键   |
|  name   | varchar | 讲师姓名 |
|  email  | varchar | 讲师邮箱 |
|   age   |   int   | 讲师年龄 |
| country | varchar | 讲师国籍 |

```sql
select * from teachers where country = "CN" order by age desc limit 1;
```

-----

## 函数

### 算术函数

####  AVG() 函数求数值列的平均值

平均函数 AVG() 是平均数 AVERAGE 的缩写，它用于求数值列的平均值。它可以用来返回所有列的平均值，也可以用来返回特定列和行的平均值。
具体的计算过程为：其通过对表中行数计数并计算特定数值列的列值之和，求得该列的平均值。但是当参数 `column_name` 列中的数据均为空时，结果会返回 NULL。

**语法：**

```sql
SELECT AVG(`column_name`) 
FROM `table_name`;
```

NULL**的处理：**

当 `AVG()` 函数遇到包含 `NULL` 值的列时，它会自动忽略这些 `NULL` 值，并仅基于非 `NULL` 的值来计算平均值。这意味着 `NULL` 值不会影响最终的平均值计算。

**行为说明:**

- **忽略 `NULL`**：`AVG()` 函数会跳过任何 `NULL` 值，只考虑那些有实际数值的行。
- **空结果集**：如果所有行的该列值都是 `NULL` 或者没有任何行满足查询条件，那么 `AVG()` 函数将返回 `NULL`。
- **数据类型**：`AVG()` 通常用于数值类型的列（如 `INT`, `FLOAT`, `DECIMAL` 等）。如果尝试对非数值类型的列使用 `AVG()`，将会导致错误。

##### **处理 `NULL` 值**

如果你希望将 `NULL` 值视为零或某个特定的默认值，可以使用 `COALESCE()` 函数来替换 `NULL`。例如：

```sql
SELECT AVG(COALESCE(score, 0)) AS average_score
FROM students;
```

在这个例子中，`COALESCE(score, 0)` 会将 `NULL` 值替换为 `0`，然后 `AVG()` 函数将基于这些替换后的值计算平均值。

**例题：**

请编写 SQL 语句，查询教师表 `teachers` 中教师邮箱为 '@qq.com' 结尾的年龄的平均值，最后返回结果列名显示为 'average_teacher_age' 。

表定义: teachers (教师表)

|  列名   |  类型   |   注释   |
| :-----: | :-----: | :------: |
|   id    |   int   |   主键   |
|  name   | varchar | 讲师姓名 |
|  email  | varchar | 讲师邮箱 |
|   age   |   int   | 讲师年龄 |
| country | varchar | 讲师国籍 |

```sql
SELECT avg(age) as 'average_teacher_age' from teachers where email like '%@qq.com';
```

---

#### MAX() 函数返回指定列中的最大值

最大值函数 MAX() 用于返回指定列中的最大值。它只有一个参数 `column_name` ，表示指定的列名。但是当参数 `column_name` 列中的数据均为空时，结果会返回 NULL。

**语法：**

```sql
SELECT MAX(`column_name`) 
FROM `table_name`;
```

---

####  MIN () 函数返回指定列中的最小值

MIN() 函数的功能与 MAX() 正好相反，它用于返回指定列中的最小值。但与 MAX() 相同的是，它也只有一个参数 `column_name` ，表示指定的列名，且当参数 `column_name` 列中的数据均为空时，结果会返回 NULL。

**语法**

```sql
SELECT MIN(`column_name`) 
FROM `table_name`;
```

---

#### SUM() 函数统计数值列的总数

SUM() 函数用于统计数值列的总数并返回其值。它只有一个参数 `column_name` ，表示指定的列名，但是当参数 `column_name` 列中的数据均为空时，结果会返回 NULL。

**语法**

```sql
SELECT SUM(`column_name`) 
FROM `table_name`;
```

---

#### ROUND() 函数将数值四舍五入

`ROUND()` 函数用于把数值字段舍入为指定的小数位数。

**语法**

```sql
SELECT ROUND(`column_name`, `decimals`) 
FROM `table_name`;
```

其中：

- column_name 为要舍入的字段
- decimals 规定要返回的小数位数
- ROUND() 函数始终返回一个值。当 decimals 为正数时，column_name 四舍五入为 decimals 所指定的小数位数。当 decimals 为负数时，column_name 则按 decimals 所指定的在小数点的左边四舍五入。
- 特别的，如果 length 是负数且大于小数点前的数字个数，ROUND() 函数将返回 0

##### ROUND(X)

ROUND( X )：返回参数 X 四舍五入后的一个整数。

##### ROUND(X, D)

ROUND(X, D)： 返回参数 X 四舍五入且保留 D 位小数后的一个数字。如果 D 为 0，结果将没有小数点或小数部分。

编写 SQL 语句，查询教师表 `teachers` 中，20 岁（不包含 20 岁）以上教师的平均年龄，返回的字段为 avg_teacher_age ，结果保留四舍五入后的整数。

表定义：`teachers`

| **列名** | **类型** | **注释** |
| :------- | :------- | :------- |
| id       | int      | 主键     |
| name     | varchar  | 讲师姓名 |
| email    | varchar  | 讲师邮箱 |
| age      | int      | 讲师年龄 |
| country  | varchar  | 讲师国籍 |

```sql
select round(avg(age)) as avg_teacher_age from teachers where age >20;
```

----

####  NULL() 函数判断空值

#####  ISNULL( )

`ISNULL()` 函数用于判断字段是否为 NULL，它只有一个参数 `column_name` 为列名，根据`column_name` 列中的字段是否为 NULL 值返回 0 或 1。

**语法**

```sql
1SELECT ISNULL(`column_name`)
2FROM `table_name`;
```

其中：

- 如果 `column_name` 列中的某个字段是 NULL 则返回 1，不是则返回 0

#####  IFNULL()

`IFNULL()` 函数也用于判断字段是否为NULL，但是与 `ISNULL()` 不同的是它接收两个参数，第一个参数 `column_name` 为列名，第二个参数 `value` 相当于备用值。

**语法**

```sql
1SELECT IFNULL(`column_name`, `value`)
2FROM `table_name`;
```

其中：

- 如果 `column_name` 列中的某个字段是 NULL 则返回 value 值，不是则返回对应内容。
- `COALESCE(column_name, value)` 函数也用于判断字段是否为NULL，其用法和 `IFNULL()` 相同。

---

#### COUNT() 函数计数

用于计数，可利用其确定表中行的数目或者符合特定条件的行的数目。当COUNT() 中的参数不同时，其的用途也是有明显的不同的，主要可分为以下三种情况：COUNT(column_name) 、COUNT( * ) 和 COUNT(DISTINCT column_name) 。

##### COUNT( column_name )

COUNT(column_name) 函数会对指定列具有的行数进行计数，但是**会除去值为 NULL 的行**。该函数主要用于查看各列数据的数量情况，便于统计数据的缺失值。

假如出现某一列的数据全为 NULL 值的情况，
使用COUNT( column_name ) 函数对该列进行计数，会返回 0。

**语法：**

```sql
1SELECT COUNT(`column_name`) 
2FROM `table_name`;
```

#####  COUNT(*)

COUNT(*) 函数会对表中行的数目进行计数，**包括值为 NULL 所在行和重复项所在行**。该函数主要用于查看表中的记录数。

**语法：**

```sql
1SELECT COUNT(*) 
2FROM `table_name`;
```

❗ 注意： COUNT(column_name) 与 COUNT(*) 的区别

- COUNT(column_name) 中，如果 `column_name` 字段中的值为 NULL，则计数不会增加，而如果字段值为空字符串`""`，则字段值会加 1
- COUNT(*) 中，除非整个记录全为 NULL，则计数不会增加，如果存在某一个记录不为 NULL，或者为空字符串`""`，计数值都会加 1。正常来说，表都会有主键，而主键不为空，所以 COUNT(*) 在有主键的表中等同于 COUNT(PRIMARY_KEY)，即查询有多少条记录。

##### COUNT(DISTINCT column_name)

COUNT(DISTINCT column_name) 函数返回**指定列的不同值的数目**

**语法**

```sql
1SELECT COUNT(DISTINCT `column_name`) 
2FROM `table_name`;
```



编写 SQL 语句，统计教师表中年龄在 20 到 28 岁之间，且国籍为中国（对应的 `country` 值为 `CN`）或英国（对应的 `country` 值为 `UK`）的教师人数，最后返回统计值，结果列名显示为 `teacher_count` 。

表定义: teachers (教师表)

|  列名   |  类型   |   注释   |
| :-----: | :-----: | :------: |
|   id    |   int   |   主键   |
|  name   | varchar | 讲师姓名 |
|  email  | varchar | 讲师邮箱 |
|   age   |   int   | 讲师年龄 |
| country | varchar | 讲师国籍 |

```sql
select count(*) as teacher_count from teachers where country in ('CN','UK') and (age BETWEEN 20 and 28)
select count(*) as teacher_count from teachers where age BETWEEN 20 and 28and country in ('CN','UK');
```

---

### 时间函数

#### 获取当前的时间

当前时间的获取函数：

- `NOW()` 可以用来返回当前日期和时间 格式：YYYY-MM-DD hh:mm:ss
- `CURDATE()` 可以用来返回当前日期 格式：YYYY-MM-DD
- `CURTIME()` 可以用来返回当前时间 格式：hh:mm:ss

> 在使用 `NOW()` 和 `CURTIME()` 时，如果要精确的秒以后的时间的话，可以在（）中加数字，加多少，就表示精确到秒后多少位
> 比如 `NOW(3)` 就是精确到毫秒，表示为： `2021-03-31 15:27:20.645`

```sql
mysql> SELECT NOW() AS `current_datetime`,
    -> CURDATE() AS `current_date`,
    -> CURTIME() AS `current_time`;
+---------------------+--------------+--------------+
| current_datetime    | current_date | current_time |
+---------------------+--------------+--------------+
| 2021-03-25 16:16:30 | 2021-03-25   | 16:16:30     |
+---------------------+--------------+--------------+
1 row in set
```

**例题：**

请编写 SQL 语句，向记录表 `records` 中插入当前的日期。

表定义: records (记录表)

| 列名     | 类型 | 注释     |
| :------- | :--- | :------- |
| now_time | date | 现在时间 |

```sql
INSERT into records (now_time) values (now());
INSERT into records values (now());
```

----

####  DATE()、TIME() 函数提取日期和时间

 `DATE()`、`TIME()` 函数分别将 `'2021-03-25 16:16:30'` 这组数据中的日期于时间提取出来，并用 `date` 、`time` 作为结果集列名。

通过 `NOW()` 获得当前的时间与日期，再使用 `DATE()` 函数将其中的日期提取出来，具体的用法是这样子的 `DATE(NOW())`，我们在把他与 `CURDATE()` 比较一下。

```sql
mysql> SELECT DATE(NOW()),CURDATE();
+-------------+------------+
| DATE(NOW()) | CURDATE()  |
+-------------+------------+
| 2021-03-25  | 2021-03-25 |
+-------------+------------+
1 row in set
```

----

####  EXTRACT() 函数提取指定的时间信息

EXTRACT() 函数用于返回日期/时间的单独部分，如 `YEAR` (年)、`MONTH` (月)、`DAY` (日)、`HOUR` (小时)、`MINUTE` (分钟)、 `SECOND` (秒)。

**语法**

```sql
1SELECT EXTRACT(unit FROM date)
2FROM `table`
```

其中：

*table* 是表格名

*date* 参数是合法的日期表达式。

*unit* 参数是需要返回的时间部分，如 `YEAR` 、`MONTH` 、 `DAY` 、 `HOUR` 、`MINUTE` 、`SECOND` 等。

> 在一般情况下，`EXTRACT(unit FROM date)` 与 `unit()` 的结果相同。

使用 EXTRACT() 函数，从课程表 `courses` 中查询课程的名字和创建时间的小时数，并为 `created_at` 起别名为 `created_hour` 。

```bash
1mysql> SELECT `name`, EXTRACT(HOUR FROM `created_at`) AS `created_hour`
2FROM `courses`;
3+-------------------------+--------------+
4| name                    | created_hour |
5+-------------------------+--------------+
6| Advanced Algorithms     | 9            |
7| System Design           | 10           |
8| Django                  | 12           |
9| Web                     | 13           |
10| Big Data                | 16           |
11| Artificial Intelligence | 18           |
12| Java P6+                | 13           |
13| Data Analysis           | 13           |
14| Object Oriented Design  | 13           |
15| Dynamic Programming     | 20           |
16+-------------------------+--------------+
1710 row in set
```

请编写 SQL 语句，从课程表 `courses` 中查询所有课程的课程名称（ `name` ）和课程创建时间（ `created_at` ）的小时数，将提取小时数的列名起别名为 `created_hour`。

表定义：courses（课程表）

| **列名**      | **类型**     | **注释** |
| :------------ | :----------- | :------- |
| id            | int unsigned | 主键     |
| name          | varchar      | 课程名称 |
| student_count | int          | 学生总数 |
| created_at    | datetime     | 创建时间 |
| teacher_id    | int          | 讲师 id  |

```sql
select name , extract(HOUR from created_at) as created_hour from courses;
```

----

#### DATE_FORMAT() 格式化输出日期

##### DATE_FORMAT() 用法

我们在 SQL 中使用 `DATE_FORMAT()` 方法来格式化输出 date/time。
需要注意的是 `DATE_FORMAT()` 函数返回的是**字符串**格式。

**语法**

```sql
SELECT DATE_FORMAT(date,format);
```

其中

- `date` 一个有效日期。
- `format` 是 date/time 的输出格式。

```sql
SELECT DATE_FORMAT(`created_at`, '%Y %m') AS `DATE_FORMAT`
FROM `courses`;
```

其中 %m 表示月份，%d 表示日期，%Y 表示年份，%w 表示星期。

执行输出结果：

```bash
mysql> SELECT DATE_FORMAT(`created_at`, '%Y %m') AS `DATE_FORMAT`
    -> FROM `courses`;
+-------------+
| DATE_FORMAT |
+-------------+
| 2020 06     |
| 2020 07     |
| 2020 02     |
| 2020 04     |
| 2020 09     |
| 2018 05     |
| 2019 01     |
| 2019 07     |
| 2020 08     |
| 2018 08     |
+-------------+
10 rows in set (0.01 sec)
```



请编写 SQL 语句，查询 `courses` 表，查询课程创建时间，按照 ’yyyy-MM-dd HH:mm:ss’ 的格式返回结果，返回列名显示为 `DATE_FORMAT`。

表定义: courses（课程表）

|     列名      |     类型     |     注释     |
| :-----------: | :----------: | :----------: |
|      id       | int unsigned |     主键     |
|     name      |   varchar    |   课程名称   |
| student_count |     int      |   学生总数   |
|  created_at   |     date     | 课程创建时间 |
|  teacher_id   |     int      |   讲师 id    |

```sql
select DATE_FORMAT(`created_at`, '%Y-%m-%d %H:%i:%s') AS `DATE_FORMAT` from courses;
```

---

#### 例题：

##### 计算课程表中所有课程指定日期与开课日期的月数差

请编写 SQL 语句，查询 `courses` 表，计算 '2020-04-22' 与课程创建时间的月数差，返回列名显示为 MonthDiff。

表定义: courses（课程表）

|     列名      |     类型     |     注释     |
| :-----------: | :----------: | :----------: |
|      id       | int unsigned |     主键     |
|     name      |   varchar    |   课程名称   |
| student_count |     int      |   学生总数   |
|  created_at   |     date     | 课程创建时间 |
|  teacher_id   |     int      |   讲师 id    |

```sql
SELECT 
    TIMESTAMPDIFF(MONTH, created_at, '2020-04-22') AS MonthDiff
FROM 
    courses;
```

---

##### 将课程创建日期均推迟一天

请编写 SQL 语句，查询 `courses` 表中课程的课程创建日期，并将课程创建日期均推迟一天，最后返回课程名称 `name` 及修改后的课程创建时间，修改后的课程创建时间命名为 `new_created` 。

表定义: courses（课程表）

|     列名      |     类型     |     注释     |
| :-----------: | :----------: | :----------: |
|      id       | int unsigned |     主键     |
|     name      |   varchar    |   课程名称   |
| student_count |     int      |   学生总数   |
|  created_at   |     date     | 课程创建时间 |
|  teacher_id   |     int      |   讲师 id    |

```sql
SELECT 
    name,
    DATE_ADD(created_at, INTERVAL 1 DAY) AS new_created
FROM 
    courses;
```

---

#####  将课程创建日期均提前一天

请编写 SQL 语句，查询 `courses` 表中课程的课程创建日期，将课程创建日期均提前一天，最后返回课程 `id` 、课程名称 `name` 及修改后的开课日期，修改后的课程创建日期命名为 `new_created` 。

表定义: courses（课程表）

|     列名      |     类型     |     注释     |
| :-----------: | :----------: | :----------: |
|      id       | int unsigned |     主键     |
|     name      |   varchar    |   课程名称   |
| student_count |     int      |   学生总数   |
|  created_at   |     date     | 课程创建时间 |
|  teacher_id   |     int      |   讲师 id    |

```sql
SELECT 
    id,
    name,
    DATE_SUB(created_at, INTERVAL 1 DAY) AS new_created
FROM 
    courses;
```

---

## **约束与多表连结**

### 约束

#### 非空约束 NOT NULL

NOT NULL 约束强制列不接受 NULL 值，强制字段始终包含值，这意味着，如果不向字段添加值，就无法插入新纪录或者更新记录。

先通过一个例子感受一下 NOT NULL 的作用

下面的 SQL 强制 `ID` 列、 `LastName` 列以及 `FirstName` 列不接受 NULL 值：

```sql
CREATE TABLE `Persons` (
    `ID` int NOT NULL,
    `LastName` varchar(255) NOT NULL,
    `FirstName` varchar(255) NOT NULL,
    `Age` int
);
```

在一个已创建的表的 `Age` 字段中添加 NOT NULL 约束如下所示：

```sql
ALTER TABLE `Persons`
MODIFY `Age` int NOT NULL;
```

---

#### 唯一约束 UNIQUE

- UNIQUE 约束唯一标识数据库表中的每条记录
- UNIQUE 和主键约束均为列或列集合提供了唯一性的保证
- 主键约束会自动定义一个 UNIQUE 约束，或者说主键约束是一种特殊的 UNIQUE 约束。但是二者有明显的区别：每个表可以有多个 UNIQUE 约束，但只能有一个主键约束。

下面的 SQL 在 `Persons` 表创建时在 `P_Id` 列上创建 UNIQUE 约束：

```sql
CREATE TABLE `Persons`
(
`P_Id` int NOT NULL,
`LastName` varchar(255) NOT NULL,
`FirstName` varchar(255),
`Address` varchar(255),
`City` varchar(255),
UNIQUE (`P_Id`)
)
```

当表已被创建时，在 `P_Id` 列创建 UNIQUE 约束:

```sql
ALTER TABLE `Persons`
ADD UNIQUE（`P_Id`）
```

撤销 UNIQUE **约束**

```sql
ALTER TABLE `Persons`
DROP INDEX uc_PersonID
```

---

#### 主键约束 PRIMARY KEY

PRIMARY KEY 约束唯一标识数据库表中的每条记录 ，简单的说，PRIMARY KEY = UNIQUE + NOT NULL ,从技术的角度来看，PRIMARY KEY 和 UNIQUE 有很多相似之处。

- NOT NULL UNIQUE 可以将表的一列或多列定义为唯一性属性，而 PRIMARY KEY 设为多列时，仅能保证多列之和是唯一的，具体到某一列可能会重复。
- PRIMARY KEY 可以与外键配合，从而形成主从表的关系，而 NOT NULL UNIQUE 则做不到这一点

> 如：
>
> 表一：用户 `id` (主键)，用户名
>
> 表二: 银行卡号 `id` (主键)，用户 `id` (外键)
>
> 则表一为主表，表二为从表。

- 更大的区别在逻辑设计上。 PRIMARY KEY 一般在逻辑设计中用作记录标识，这也是设置 PRIMARY KEY 的本来用意，而 UNIQUE 只是为了保证域/域组的唯一性。

**CREATE TABLE 时 添加 PRIMARY KEY 约束:**

```sql
CREATE TABLE `Persons`
(
    `P_Id` int NOT NULL,
    `LastName` varchar(255) NOT NULL,
    `FirstName` varchar(255),
    `Address` varchar(255),
    `City` varchar(255),
    PRIMARY KEY (`P_Id`)
);
```

**ALTER TABLE 时添加主键约束:**

```sql
ALTER TABLE `Persons`
ADD PRIMARY KEY (`P_Id`)
```

**撤销 PRIMARY KEY**

```sql
ALTER TABLE `Persons`
    DROP PRIMARY KEY
```

---

### **多表连结**

联结是一种机制，用于在一条 SELECT 语句中关联多个表，返回一组输出。

#### 内连接 INNER JOIN

**内连接就是取两个表的交集，返回的结果就是连接的两张表中都满足条件的部分。**

**基本语法**

在对 INNER JOIN（内连接）的概念有基本的了解之后，我们再来学习一下它的基本语法。
基本语法有如下两种写法：

```sql
SELECT `table1`.`column1`, `table2`.`column2`...
FROM `table1`
INNER JOIN `table2`
ON `table1`.`common_field` = `table2`.`common_field`;
```

```sql
SELECT `table1`.`column1`, `table2`.`column2`...
FROM `table1`
JOIN `table2`
ON `table1`.`common_field` = `table2`.`common_field`;
```

**例题：**

请编写 SQL 语句，将课程表 `courses` 和教师表 `teachers` 进行内连接，查询 “Eastern Heretic” 老师所教的所有课程的课程名和课程编号 , 且结果列名分别以课程编号 `id` 、课程名称 `course_name` 和教师姓名 `teacher_name` 显示。

表定义 1 ：courses (课程表)

|     列名      |     类型     |     注释     |
| :-----------: | :----------: | :----------: |
|      id       | int unsigned |     主键     |
|     name      |   varchar    |   课程名称   |
| student_count |     int      |   学生总数   |
|  created_at   |     date     | 创建课程时间 |
|  teacher_id   |     int      |   讲师 id    |

表定义 2 : teachers (教师表)

|  列名   |  类型   |   注释   |
| :-----: | :-----: | :------: |
|   id    |   int   |   主键   |
|  name   | varchar | 讲师姓名 |
|  email  | varchar | 讲师邮箱 |
|   age   |   int   | 讲师年龄 |
| country | varchar | 讲师国籍 |

```sql
SELECT c.id, c.name as course_name, t.name as teacher_name
from courses c
inner join teachers t
on c.teacher_id = t.id
where t.name = 'Eastern Heretic';
```

-----

#### 外连接 OUTER JOIN

外连接查询可以分为以下三类：

- 左外连接
- 右外连接
- 全外连接

外连接数据查询语法如下：

```sql
SELECT column_name 1,column_name 2 ... column_name n
    FROM table1
        LEFT | RIGHT | FULL  (OUTER) JOIN table2
        ON CONDITION;
```

在上述语句中，参数 column_name 表示所要查询的字段名字，来源于所连接的表 table1 和 table2，关键字 OUTER JOIN 表示表进行外连接，参数 CONDITION 表示进行匹配的条件。

##### 左外连接 LEFT JOIN

左外连接的结果包括 LEFT OUTER 子句中指定的左表的所有行，而不仅仅是连接列所匹配的行，这就意味着，左连接会返回左表中的所有记录，加上右表中匹配到的记录。

**语法：**

```sql
SELECT column_name 1,column_name 2 ... column_name n
    FROM table1
        LEFT JOIN table2
        ON CONDITION ;
```

##### 右外连接 RIGHT JOIN

外连接查询中的右外连接是指新关系中执行匹配条件时，以关键字 RIGHT JOIN 右边的表为参考表，如果右表的某行在左表中没有匹配行，左表就返回空值。

**语法：**

```sql
SELECT column_name 1,column_name 2 ... column_name n
    FROM table1
        RIGHT JOIN table2
        ON CONDITION ;
```

##### 全外连接 FULL (OUTER) JOIN

FULL OUTER JOIN 关键字只要左表（table1）和右表（table2）其中一个表中存在匹配，则返回行。FULL OUTER JOIN 关键字结合了 LEFT JOIN 和 RIGHT JOIN 的结果。

**由于mysql不支持，所以可以使用UNION连接左右连接来实现。**

**语法：**

```sql
SELECT column_name 1,column_name 2 ... column_name n
    FROM table1
        LEFT JOIN table2 ON CONDITION 
UNION
SELECT column_name 1,column_name 2 ... column_name n
    FROM table1
        RIGHT JOIN table2 ON CONDITION ;
```

**例题：**

请编写 SQL 语句，将教师表 `teachers` 和课程表 `courses` 进行左连接，查询来自中国（讲师国籍 `country` ='CN' ）的教师名称以及所教课程名称，结果列名请分别以课程名称 `course_name` ，教师名称 `teacher_name` 显示。

表定义 1 ：courses (课程表)

|     列名      |     类型     |     注释     |
| :-----------: | :----------: | :----------: |
|      id       | int unsigned |     主键     |
|     name      |   varchar    |   课程名称   |
| student_count |     int      |   学生总数   |
|  created_at   |     date     | 创建课程时间 |
|  teacher_id   |     int      |   讲师 id    |

表定义 2 : teachers (教师表)

|  列名   |     类型     |   注释   |
| :-----: | :----------: | :------: |
|   id    | int unsigned |   主键   |
|  name   |   varchar    | 讲师姓名 |
|  email  |   varchar    | 讲师邮箱 |
|   age   |     int      | 讲师年龄 |
| country |   varchar    | 讲师国籍 |

```sql
select c.name as course_name, t.name as teacher_name
from teachers t
left join courses c
on c.teacher_id = t.id
where t.country ='CN';
```

请编写 SQL 语句，将课程表 `courses` 和教师表 `teachers` 进行全外连接，查询所有课程名称以及与其相互对应的教师名称和教师国籍，结果列名请分别以课程名称 `course_name` 、教师名称 `teacher_name` 、教师国籍 `teacher_country` 显示。

表定义 1 ：courses (课程表)

|     列名      |     类型     |     注释     |
| :-----------: | :----------: | :----------: |
|      id       | int unsigned |     主键     |
|     name      |   varchar    |   课程名称   |
| student_count |     int      |   学生总数   |
|  created_at   |     date     | 创建课程时间 |
|  teacher_id   |     int      |   讲师 id    |

表定义 2 : teachers (教师表)

|  列名   |     类型     |   注释   |
| :-----: | :----------: | :------: |
|   id    | int unsigned |   主键   |
|  name   |   varchar    | 讲师姓名 |
|  email  |   varchar    | 讲师邮箱 |
|   age   |     int      | 讲师年龄 |
| country |   varchar    | 讲师国籍 |

```sql
SELECT 
    c.name AS course_name,
    t.name AS teacher_name,
    t.country AS teacher_country
FROM 
    courses c
LEFT JOIN 
    teachers t ON c.teacher_id = t.id

UNION

SELECT 
    c.name AS course_name,
    t.name AS teacher_name,
    t.country AS teacher_country
FROM 
    courses c
RIGHT JOIN 
    teachers t ON c.teacher_id = t.id;
```

----

### 分组查询与子查询

#### 分组查询 GROUP BY 子句

**语法：**

```sql
SELECT `column_name`, aggregate_function(`column_name`)
FROM `table_name`
WHERE `column_name` operator value
GROUP BY `column_name`;
```

##### GROUP BY 单表实例

**可以看到我们教师表中的教师来自不同的国家，现需要统计不同国家教师的人数，并将结果按照不同国籍教师人数从小到大排列，请编写相应的 SQL 语句实现。**

使用 SQL 中子查询的方式如下：

```sql
SELECT `country`, COUNT(`country`) AS `teacher_count`
FROM `teachers`
GROUP BY `country`
ORDER BY `teacher_count`, `country`;
```

##### GROUP BY 多表实例

课程表的每节课程都有对应的一个教师负责授课，而每一个教师对应多门课程，现需要统计每个教师教授课程的学生总数，请编写相应的 SQL 语句实现。

使用 SQL 中子查询的方式如下：

```sql
SELECT T.name AS `teacher_name`, IFNULL(SUM(C.student_count), 0) AS `student_count`
FROM `courses` C
	RIGHT JOIN `teachers` T ON C.teacher_id = T.id
GROUP BY T.id;
```

请编写 SQL 语句，查询教师表 `teachers`，统计不同年龄教师的人数，返回的结果按照以年龄列 `age` 从大到小排列，所统计的人数的列名显示为 `age_count`。

**例题：**

表定义: teachers (教师表)

| 列名    | 类型         | 注释     |
| :------ | :----------- | :------- |
| id      | int unsigned | 主键     |
| name    | varchar      | 讲师姓名 |
| email   | varchar      | 讲师邮箱 |
| age     | int          | 讲师年龄 |
| country | varchar      | 讲师国籍 |

```sql
select age ,count(age) as age_count 
from teachers 
group by age 
order by age desc
```

请编写 SQL 语句，查询教师表 `teachers` 和课程表 `courses`,统计每个老师教授课程的数量，并将结果按课程数量降序排列，课程数量相同时按教师姓名升序排列，返回老师姓名和课程数量，分别命名为 `teacher_name` 和 `course_count`。

表定义：teachers（教师表）

| 列名    | 类型         | 注释     |
| :------ | :----------- | :------- |
| id      | int unsigned | 主键     |
| name    | varchar      | 讲师姓名 |
| email   | varchar      | 讲师邮箱 |
| age     | int          | 讲师年龄 |
| country | varchar      | 讲师国籍 |

表定义：courses（课程表）

| 列名          | 类型         | 注释         |
| :------------ | :----------- | :----------- |
| id            | int unsigned | 主键         |
| name          | varchar      | 课程名称     |
| student_count | int          | 学生总数     |
| created_at    | datetime     | 课程创建时间 |
| teacher_id    | int unsigned | 讲师 id      |

```sql
select t.name as teacher_name,count(c.name) as course_count
from teachers t
left join courses c
on t.id = c.teacher_id
group by t.id
order by course_count desc, teacher_name asc;
```

----

#### HAVING 子句

我们在使用 WHERE 条件子句时会发现其不能与聚合函数联合使用，为解决这一点，SQL 中提供了 HAVING 子句。在使用时， HAVING 子句经常与 GROUP BY 联合使用，**HAVING 子句就是对分组统计函数进行过滤的子句**。HAVING 子句对于 GROUP BY 子句设置条件的方式其实与 WHERE 子句与 SELECT 的方式类似，语法也相近，但 WHERE 子句搜索条件是在分组操作之前，而 HAVING 则是在之后。

**语法：**

```sql
SELECT   `column_name`, aggregate_function(`column_name`) 
FROM     `table_name` 
WHERE    `column_name` operator value 
GROUP BY `column_name` 
HAVING   aggregate_function(`column_name`) operator value;
```

```sql
SELECT `T`.`name` AS `name`, IFNULL(SUM(`C`.`student_count`),0) AS `student_count`
FROM `courses` `C` RIGHT JOIN `teachers` `T`
ON `C`.`teacher_id` = `T`.`id`
GROUP BY `T`.`id`
HAVING `student_count` < 3000
ORDER BY `student_count`, `name`;
```

**例题：**

请编写 SQL 语句，从 `teachers` 表中，筛选出同一国家的教师平均年龄大于所有教师平均年龄的 **国家**，并获取这些国家的所有教师信息。

表定义: teachers (教师表)

| 列名    | 类型         | 注释     |
| :------ | :----------- | :------- |
| id      | int unsigned | 主键     |
| name    | varchar      | 讲师姓名 |
| email   | varchar      | 讲师邮箱 |
| age     | int          | 讲师年龄 |
| country | varchar      | 讲师国籍 |

```sql
select * from teachers
    WHERE country in (
        select country from teachers GROUP BY country
            HAVING AVG(age) > (SELECT AVG(age) FROM teachers)
    )
```

---

### SELECT 语句中的子查询

**当一个查询是另一个查询的条件时**，称之为子查询。

语法：

```sql
SELECT `column_name(s)`
FROM `table_name`
WHERE `column_name` OPERATOR (
    SELECT `column_name(s)`
    FROM `table_name`
);
```

**例题：**

请编写 SQL 语句，查询 `courses` 表和 `teachers` 表，查询 'Big Data' 课程对应的老师姓名。

表定义: teachers (教师表)

|  列名   |     类型     |   注释   |
| :-----: | :----------: | :------: |
|   id    | int unsigned |   主键   |
|  name   |   varchar    | 讲师姓名 |
|  email  |   varchar    | 讲师邮箱 |
|   age   |     int      | 讲师年龄 |
| country |   varchar    | 讲师国籍 |

表定义: courses（课程表）

|     列名      |     类型     |     注释     |
| :-----------: | :----------: | :----------: |
|      id       | int unsigned |     主键     |
|     name      |   varchar    |   课程名称   |
| student_count |     int      |   学生总数   |
|  created_at   |     date     | 课程创建时间 |
|  teacher_id   |     int      |   讲师 id    |

```sql
select name 
    from teachers
        where id in (
            select teacher_id 
                from courses 
                where name = 'Big Data'
        )
        
select t.name from teachers t 
inner join courses c 
on t.id=c.teacher_id
where c.name='Big Data';
```

---

### INSERT 语句中的子查询

子查询不仅仅可以嵌套在 SELECT 语句中用于构造父查询的条件，也可以嵌套在 INSERT 语句中用于插入数据。

```sql
INSERT INTO `table_name`
	SELECT `colnum_name(s)`
	FROM `table_name`
	[ WHERE VALUE OPERATOR ]
```

-----

### UPDATE 语句中的子查询

```sql
UPDATE `table_name` 
SET `column_name` = `new_value`
WHERE `column_name` OPERATOR 
   (SELECT `column_name`
   FROM `table_name`
   [WHERE] )
```

