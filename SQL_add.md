[TOC]

# SQL

## 实体关系图ERD

[来源](https://mode.com/) (parch_and_posey)

![img](SQL.assets/screen-shot-2017-08-02-at-11.14.25-am.png)

- 可以通过 ["crow's foot" 表示法](http://www.vertabelo.com/blog/technical-articles/crow-s-foot-notation)来标记出一个表格里的列与另一个表格的列之间的关联，比如 region 表格里的 `id` 对应 sales_rep 表格里 `region_id`。
  - Relationships have two indicators. These are shown on both sides of the line.

    - The first one (often called **multiplicity**) refers to the *maximum* number of times that an instance of one entity can be associated with instances in the related entity. It can be **one** or **many**.

      ![Multiplicity of one in crow’s foot notation](SQL.assets/crows-foot-notation-multiplicity-of-one.png)

      ![Multiplicity of many in crow’s foot notation](SQL.assets/crows-foot-notation-multiplicity-of-many.png)

    - The second describes the *minimum* number of times one instance can be related to others. It can be **zero** or **one**, and accordingly describes the relationship as **optional** or **mandatory**.

      ![Mandatory relationship in crow’s foot notation](SQL.assets/crows-foot-notation-mandatory.png)

      ![Optional relationship in crow’s foot notation](SQL.assets/crows-foot-notation-optional.png)

- One-to-one

  ![img](SQL_add.assets/crows-foot-notation-one-to-one.png)

- One-to-many

  ![img](SQL_add.assets/crows-foot-notation-one-to-many.png)

- Many-to-many

  ![Many-to-many](SQL.assets/crows-foot-notation-many-to-many.png)

## 键

### 主键 (PK)

**主键**是特定表格的唯一列。在我们的每个表格中是第一列，并且这些列都叫做 **id**，但是并非必须都要这样。**通常，在大多数数据库中，主键是表格的第一列。**

### 外键 (FK)

**外键**是另一个表格中的主键。

## 数据库类型

- SQL数据库

  - 一些最受欢迎的数据库包括：:
  1. MySQL
    2. Access
    3. Oracle
    4. Microsoft SQL Server
    5. Postgres
  
  [这里](https://www.digitalocean.com/community/tutorials/sqlite-vs-mysql-vs-postgresql-a-comparison-of-relational-database-management-systems) 的文章比较了三种最常见的 SQL 类型：SQLite、PostgreSQL 和 MySQL。
  
- Non-SQL数据库

  - [MongoDB](https://classroom.udacity.com/courses/ud032)

## 数据库规范化

在创建数据库时，一定要思考下将如何存储数据。这称为**规范化**，是大多数 SQL 课程的一个重要组成部分。如果你负责设置新的数据库，则需要详细了解数据库**规范化**。

数据库规范化需要考虑以下三个要点：

1. 表格存储了逻辑分组的数据吗？
2. 我能在一个位置进行更改，而不是在多个表格中对同一信息作出更改吗？
3. 我能快速高效地访问和操纵数据吗？

[这篇文章](http://agiledata.org/essays/dataNormalization.html)详细讲解了上述内容。

## 格式

### 大写

你可能已经注意到，我们大写了 SELECT 和 FROM，而将表和列名称小写。这是一个常见的格式惯例。大写命令（SELECT、FROM），小写查询中的其他内容是常见做法。这使得查询更容易读取，这在编写更复杂的查询时更为重要。准备编写查询时，这是一个很好的习惯。

### 表和变量名中不需要空格

通常在列名中使用下划线，避免使用空格。 在 SQL 中使用空格有点麻烦。 在 Postgres 中，如果列或表名称中有空格，就需要使用双引号括住这些列/表名称（例如：FROM "Table Name"，而不是 FROM table_name）。在其他环境中，可能会使用方括号（例如：FROM [Table Name]）。

### 在查询中使用空格

SQL 查询忽略空格，因此可以根据需要在代码之间添加尽可能多的空格和空行，并且查询结果是相同的。我们来看下面这个查询

```
SELECT account_id FROM orders
```

等价于这个查询:

```
SELECT account_id
FROM orders
```

和这个查询（但是不要这样写，不符合规范，而且太丑了）:

```
SELECT              account_id

FROM               orders
```

### SQL 不区分大小写

如果你已经使用过其他语言编程，那么可能会熟悉编程语言，如果没有区分大小写键入正确的字符，那么会非常麻烦。 SQL 不区分大小写。 我们来看看下面的查询：

```
SELECT account_id
FROM orders
```

和这个相同：

```
select account_id
from orders
```

也和这个相同：

```
SeLeCt AcCoUnt_id
FrOm oRdErS
```

但是，我会再次提醒你遵循上面讲述的完全大写命令的惯例，而将其他代码片段小写。

### 分号

根据 SQL 环境，查询结尾可能需要一个执行的分号。 这个"要求"在其他环境中比较灵活。我们认为在每个语句的末尾添加一个分号是最好的做法，如果环境能够一次显示多个结果，那么这样做还可以一次运行多个命令。

最好的做法：

```
SELECT account_id
FROM orders;
```

因为，我们这里的环境不需要分号，你会看到没有分号的解决方案：

```
SELECT account_id
FROM orders
```

## 单表查询

| **语句** | **用法**                        | **其他详情**                               |
| :------- | :------------------------------ | :----------------------------------------- |
| SELECT   | SELECT **Col1**, **Col2**, ...  | 提供你想要的列                             |
| FROM     | FROM **Table**                  | 提供列存在的表                             |
| LIMIT    | LIMIT **10**                    | 限制返回的行数                             |
| ORDER BY | ORDER BY **Col**                | 根据列对表排序。与 **DESC** 一起使用。     |
| WHERE    | WHERE **Col > 5**               | 用于过滤结果的条件语句                     |
| LIKE     | WHERE **Col LIKE '%me%**        | 仅拉取文本中包含 'me' 的列                 |
| IN       | WHERE **Col IN ('Y', 'N')**     | 仅过滤包含 'Y' 或 'N' 列的行               |
| NOT      | WHERE **Col NOT IN ('Y', 'N')** | **NOT** 经常与 **LIKE** 和 **IN** 一起使用 |
| AND      | WHERE **Col1 > 5 AND Col2 < 3** | 过滤两个或多个条件必须为真的行             |
| OR       | WHERE **Col1 > 5 OR Col2 < 3**  | 过滤至少一个条件必须为真的行               |
| BETWEEN  | WHERE **Col BETWEEN 3 AND 5**   | 通常比使用 **AND** 的语法简单              |

## SQLJOIN

**JOIN** 存储的是表格，**ON** 是让**主键**等于**外键**。

### 别名

```sql
FROM tablename AS t1
JOIN tablename2 AS t2
```

- AS 可省略

### LEFTJOIN

- 国际通用，很少用RIGHT JOIN

- OUTER JOIN 罕见

- **LEFT JOIN ON**后可加**AND**替代**WHERE**语句(相当于在连接前使用WHERE语句，连接了一个只包含了AND筛选后的表)

  - 结果中包含321500和其他销售代表的数据

  ```SQL
  SELECT orders.*,
  			 accounts.*
  	FROM demo.orders
  	LEFT JOIN demo.accounts
  		ON orders.accounts_id = accounts.id
  	 AND accounts.sales_rep_id = 321500
  ```

  - 当数据库执行该查询时，它先执行连接和 **ON** 条件中的指令。将其看做构建新的结果集，然后使用 **WHERE** 条件来过滤该结果集。
  - 将此过滤器移到内连接的 **ON** 条件中将与使其保留在 **WHERE** 条件中产生的结果一样。

### [UNION 和 UNION ALL](http://www.sqlservertutorial.net/sql-server-basics/sql-server-union/)

### [CROSS JOIN](http://www.sqlservertutorial.net/sql-server-basics/sql-server-cross-join/)

### [SELF JOIN](http://www.sqlservertutorial.net/sql-server-basics/sql-server-self-join/)

## SQL 聚合

### NULL

- **NULL** 是一种数据类型，表示 SQL 中没有数据
  - 与零或者空格不同，它们表示不存在数据的单元格

- 在 **WHERE** 条件中表示 **NULL** 时，我们写成 **IS NULL** 或 **IS NOT NULL**。我们不使用 `=`，因为 **NULL** 在 SQL 中不属于值。但是它是数据的一个属性。
- 在执行 **LEFT JOIN** 或 **RIGHT JOIN** 时，**NULL** 经常会发生。
- **NULL** 也可能是因为数据库中缺失数据。

***COUNT 会返回包含非空值数据行的计数***

***SUM 会忽略NULL，其他聚合函数也是这样***

***AVG 忽略 NULL，NULL不会记入分子或分母。如果想将 NULL 当做零，则需要使用 SUM 和 COUNT 。但是，如果 NULL 值真的只是代表单元格的未知值，那么这么做可能不太合适***

### GROUP BY

- **GROUP BY** 可以用来在数据子集中聚合数据，多个条件用`,`分隔
- **SELECT** 语句中的任何一列如果不在聚合函数中，则必须在 **GROUP BY** 条件中。即必须都是***折叠（collapse）***后的数据
- **GROUP BY** 始终在 **WHERE** 和 **ORDER BY** 之间。
- **ORDER BY** 有点像电子表格软件中的 **SORT**。

### DISTINCT

- **仅返回特定列的唯一值**的函数。
- 在使用 **DISTINCT** 时，尤其是在聚合函数中使用时，会让查询速度有所减慢。

### HAVING

- 对聚合结果进行筛选
  - 想对通过聚合创建的查询中的元素执行 **WHERE** 条件，就需要使用 **HAVING**。
- 位置在**GROUP BY**之后
  - **WHERE**在**GROUP BY**之前
- **HAVING** 是过滤被聚合的查询的 “整洁”方式，但是通常采用**子查询**的方式来实现

## DATE函数

- **DATE_TRUNC**
- **DATE_PART**
- 

