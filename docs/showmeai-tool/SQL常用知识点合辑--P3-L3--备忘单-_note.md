# SQL常用知识点合辑——P3：L3- 备忘单 📝

![](img/1a3bebe5e3b6be5376d8331c9a6944e0_0.png)

![](img/1a3bebe5e3b6be5376d8331c9a6944e0_0.png)

在本节课中，我们将介绍一份完整的SQL备忘单和总结笔记。这份资料旨在帮助你快速回顾和查找SQL的核心概念与语法，无需死记硬背。

## 课程概述

上一节我们介绍了SQL的高级查询技巧。本节中，我们将提供一份结构化的备忘单，作为你学习和工作中的快速参考指南。

## 备忘单内容

以下是SQL核心操作的分类总结。

### 1. 数据查询（SELECT）

`SELECT`语句用于从数据库中检索数据。

*   **基本查询**：`SELECT column1, column2 FROM table_name;`
*   **查询所有列**：`SELECT * FROM table_name;`
*   **条件筛选（WHERE）**：`SELECT * FROM table_name WHERE condition;`
*   **结果去重（DISTINCT）**：`SELECT DISTINCT column FROM table_name;`
*   **结果排序（ORDER BY）**：`SELECT * FROM table_name ORDER BY column ASC|DESC;`
*   **结果数量限制（LIMIT）**：`SELECT * FROM table_name LIMIT number;`

### 2. 数据过滤（WHERE子句操作符）

`WHERE`子句支持多种操作符来定义过滤条件。

*   **比较操作符**：`=`, `<>` 或 `!=`, `>`, `<`, `>=`, `<=`
*   **范围筛选（BETWEEN）**：`SELECT * FROM table_name WHERE column BETWEEN value1 AND value2;`
*   **成员匹配（IN）**：`SELECT * FROM table_name WHERE column IN (value1, value2, ...);`
*   **模式匹配（LIKE）**：`SELECT * FROM table_name WHERE column LIKE ‘pattern’;`
*   **空值判断（IS NULL）**：`SELECT * FROM table_name WHERE column IS NULL;`

### 3. 数据操作（DML）

数据操作语言用于增删改数据。

*   **插入数据（INSERT）**：`INSERT INTO table_name (column1, column2) VALUES (value1, value2);`
*   **更新数据（UPDATE）**：`UPDATE table_name SET column1 = value1 WHERE condition;`
*   **删除数据（DELETE）**：`DELETE FROM table_name WHERE condition;`

### 4. 聚合函数

聚合函数对一组值执行计算并返回单个值。

*   **计数（COUNT）**：`SELECT COUNT(column) FROM table_name;`
*   **求和（SUM）**：`SELECT SUM(column) FROM table_name;`
*   **平均值（AVG）**：`SELECT AVG(column) FROM table_name;`
*   **最大值（MAX）**：`SELECT MAX(column) FROM table_name;`
*   **最小值（MIN）**：`SELECT MIN(column) FROM table_name;`

### 5. 数据分组（GROUP BY 与 HAVING）

`GROUP BY` 语句用于结合聚合函数，根据一个或多个列对结果集进行分组。`HAVING` 子句用于过滤分组后的结果。

*   **基本分组**：`SELECT column, COUNT(*) FROM table_name GROUP BY column;`
*   **过滤分组**：`SELECT column, COUNT(*) FROM table_name GROUP BY column HAVING COUNT(*) > 5;`

### 6. 表连接（JOIN）

![](img/1a3bebe5e3b6be5376d8331c9a6944e0_2.png)

`JOIN` 用于基于相关列合并两个或多个表中的行。

*   **内连接（INNER JOIN）**：`SELECT * FROM table1 INNER JOIN table2 ON table1.column = table2.column;`
*   **左连接（LEFT JOIN）**：`SELECT * FROM table1 LEFT JOIN table2 ON table1.column = table2.column;`
*   **右连接（RIGHT JOIN）**：`SELECT * FROM table1 RIGHT JOIN table2 ON table1.column = table2.column;`
*   **全外连接（FULL OUTER JOIN）**：`SELECT * FROM table1 FULL OUTER JOIN table2 ON table1.column = table2.column;`

## 课程总结

本节课中，我们一起学习了SQL的快速参考备忘单，涵盖了数据查询、过滤、操作、聚合、分组和表连接等核心语法。请将这份资料作为你的工具，在需要时进行查阅，以巩固和加速你的SQL应用。

![](img/1a3bebe5e3b6be5376d8331c9a6944e0_2.png)