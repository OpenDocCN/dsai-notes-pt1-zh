# 006：更新与删除语句 📝

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_0.png)

在本节课中，我们将学习如何在关系数据库表中修改和删除数据。课程结束后，你将能够识别UPDATE语句和DELETE语句的语法，并理解WHERE子句在这些语句中的重要性。

---

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_2.png)

## 更新数据：UPDATE语句

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_3.png)

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_4.png)

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_5.png)

创建表并填充数据后，可以使用UPDATE语句修改表中的数据。UPDATE语句是数据操作语言（DML）语句之一，用于读取和修改数据。

基于作者实体示例，我们使用实体名称“author”和实体属性作为表的列创建了表。随后向作者表添加行以填充数据。一段时间后，你可能需要修改表中的数据。要修改作者表中的数据，我们使用UPDATE语句。

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_7.png)

UPDATE语句的语法如下：

```sql
UPDATE table_name
SET column_name = value
WHERE condition;
```

在这个语句中，`table_name` 标识目标表，`column_name` 标识要更改的列，`value` 是要设置的新值，`WHERE condition` 指定哪些行需要更新。

### 示例：更新作者姓名

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_9.png)

假设需要将作者ID为A2的作者姓名从“Raav Ahuja”更改为“Lakshmi Kata”。以下是操作步骤：

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_10.png)

首先，查看更新前的所有行：

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_12.png)

```sql
SELECT * FROM author;
```

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_13.png)

然后，执行UPDATE语句：

```sql
UPDATE author
SET last_name = 'Kata', first_name = 'Lakshmi'
WHERE author_id = 'A2';
```

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_15.png)

更新后，再次查看所有行以确认更改：

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_16.png)

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_17.png)

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_18.png)

```sql
SELECT * FROM author;
```

你将看到第二行的姓名已从“Raav Ahuja”更改为“Lakshmi Kata”。

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_20.png)

**重要提示**：如果不指定WHERE子句，表中的所有行都将被更新。例如，省略WHERE子句会导致所有作者姓名都被更改为“Lakshmi Kata”。

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_21.png)

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_22.png)

---

## 删除数据：DELETE语句

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_24.png)

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_25.png)

有时可能需要从表中删除一行或多行数据。DELETE语句用于删除行，它也是数据操作语言（DML）语句，用于读取和修改数据。

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_27.png)

DELETE语句的语法如下：

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_28.png)

```sql
DELETE FROM table_name
WHERE condition;
```

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_30.png)

要删除的行由WHERE条件指定。

### 示例：删除特定作者

基于作者实体示例，假设需要删除作者ID为A2和A3的行。以下是操作步骤：

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_32.png)

执行DELETE语句：

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_33.png)

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_34.png)

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_35.png)

```sql
DELETE FROM author
WHERE author_id IN ('A2', 'A3');
```

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_36.png)

**重要提示**：如果不指定WHERE子句，表中的所有行都将被删除。

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_38.png)

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_39.png)

---

## 总结

本节课中，我们一起学习了UPDATE语句和DELETE语句的语法及其应用。UPDATE语句用于修改现有数据，而DELETE语句用于删除数据。在这两个语句中，WHERE子句都至关重要，因为它指定了要更新或删除的具体行，避免了对整个表的意外操作。

![](img/57a2e45ac98e7fe687d8d2f76a1bd6aa_41.png)

通过掌握这些语句，你将能够有效地管理和维护数据库中的数据。