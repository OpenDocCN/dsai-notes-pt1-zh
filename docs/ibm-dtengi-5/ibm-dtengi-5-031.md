# 031：外部连接 🔗

在本节课中，我们将要学习SQL中的外部连接。外部连接是连接操作的一种，它允许我们不仅获取两个表中匹配的行，还能获取那些在另一个表中没有匹配项的行。这对于数据分析中需要查看完整数据集，包括缺失关联数据的情况非常有用。

## 概述

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_1.png)

与内连接不同，外部连接会返回连接列中具有匹配值的行，同时也会返回表之间没有匹配的行。SQL提供了三种类型的外部连接：左外部连接、右外部连接和全外部连接。

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_3.png)

## 左外部连接

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_4.png)

上一节我们介绍了外部连接的基本概念，本节中我们来看看左外部连接的具体机制。

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_6.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_7.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_8.png)

在左外部连接中，连接谓词左侧第一个表的所有行都会被包含在结果中，而只有连接谓词右侧第二个表中匹配的行会被包含。

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_1.png)

左连接会匹配左表的所有行，并将信息与右表中符合查询指定条件的行相结合。

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_10.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_11.png)

以下是左外部连接的基本语法示例：

```sql
SELECT borrower.borrower_id, borrower.last_name, borrower.country, loan.loan_date
FROM borrower
LEFT JOIN loan ON borrower.borrower_id = loan.borrower_id;
```

在这个例子中，`borrower`表是SELECT语句FROM子句中指定的第一个表，因此它是左表，而`loan`表是右表。`borrower`表位于连接操作符的左侧，因此将选择`borrower`表的所有行，并根据查询中指定的条件（本例中是`borrower_id`列）与`loan`表的内容相结合。

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_13.png)

左连接从`borrower`表中选择每个`borrower_id`，并显示`loan`表中的`loan_date`。结果集显示了`borrower`表中的每个`borrower_id`以及该借款人的贷款日期。最后三行没有贷款日期，因此`borrower_id`和`loan_date`显示为NULL值。

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_14.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_15.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_16.png)

## 右外部连接

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_17.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_18.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_19.png)

了解了左连接后，我们接下来探讨右外部连接。

在右外部连接中，连接谓词右侧第二个表的所有行都会被包含，而只有连接谓词左侧第一个表中匹配的行会被包含。

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_21.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_22.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_6.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_23.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_24.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_25.png)

右连接会匹配右表的所有行，并将信息与左表中符合查询指定条件的行相结合。

以下是右外部连接的基本语法示例：

```sql
SELECT loan.borrower_id, loan.loan_date, borrower.last_name, borrower.country
FROM borrower
RIGHT JOIN loan ON borrower.borrower_id = loan.borrower_id;
```

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_27.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_28.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_29.png)

在这个例子中，`borrower`表是左表，`loan`表是右表。在FROM子句中，`loan`表位于连接操作符的右侧，因此将选择`loan`表的所有行，并根据查询中指定的条件与`borrower`表的内容相结合。

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_30.png)

对于右连接，将从`loan`表中选择`borrower_id`和`loan_date`列，并从`borrower`表中选择`borrower_id`、`last_name`和`country`列，条件是`loan`表中的`borrower_id`与`borrower`表中的`borrower_id`匹配。

结果集显示了`loan`表中的每个`borrower_id`以及该借款人的贷款日期，前提是该`borrower_id`在`borrower`表中也存在。对于最后一行，`borrower`表中没有匹配的行，因此`borrower_id`、`last_name`和`country`显示为NULL值。这可能表明图书馆存在一个问题：有一本书借给了一个未知的人。

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_32.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_33.png)

## 全外部连接

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_34.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_35.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_36.png)

最后，我们来学习功能最全面的全外部连接。

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_38.png)

全外部连接返回右表和左表的所有行。因此，全连接可能返回一个非常大的结果集。

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_39.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_40.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_10.png)

全连接的结果集是符合查询指定条件的两个表的所有行，加上右表中所有不匹配的行。

以下是全外部连接的基本语法：

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_42.png)

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_43.png)

```sql
SELECT borrower.borrower_id, borrower.last_name, borrower.country, loan.borrower_id, loan.loan_date
FROM borrower
FULL JOIN loan ON borrower.borrower_id = loan.borrower_id;
```

对于全连接，您选择`borrower`表的所有行和`loan`表的所有行。

结果集显示了`borrower`表中列出的所有八条记录以及`loan`表中的相应数据。再次地，有三行返回NULL值，因为借款人Peters、Lee和Wang从未借过书。最后一行返回`loan`表中的`borrower_id`和`loan_date`值，但从`borrower`表返回NULL值。在这种情况下，`borrower`表中没有匹配项，这本书的借款人是未知的。

## 总结

本节课中我们一起学习了SQL中外部连接的三种类型。

*   **左外部连接**：返回左表的所有行，以及右表中与左表匹配的行。同时返回左表中在右表没有匹配项的所有行。
*   **右外部连接**：返回右表的所有行，以及左表中与右表匹配的行。同时返回右表中在左表没有匹配项的所有行。
*   **全外部连接**：返回两个表中所有匹配的行，以及两个表中所有没有匹配项的行。

![](img/d7ddf8cf4b0b0bef6c2cb6738bd3931d_45.png)

掌握这些连接类型，可以帮助您根据不同的数据分析需求，灵活地组合和查询数据库中的表。