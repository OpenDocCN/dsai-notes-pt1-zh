# 014：结果集分组 📊

![](img/b3b9c8dd03b77b36b21ee3debec16021_0.png)

在本节课中，我们将学习从关系数据库表中检索数据的高级技巧，特别是如何对结果集进行排序和分组。课程结束时，你将能够解释如何从结果集中消除重复值，并描述如何进一步限制结果集。

![](img/b3b9c8dd03b77b36b21ee3debec16021_2.png)

---

![](img/b3b9c8dd03b77b36b21ee3debec16021_3.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_4.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_5.png)

## 消除结果集中的重复值

有时，`SELECT` 语句的结果集可能包含重复值。以我们简化的图书馆数据库模型中的作者表为例，`country` 列列出了作者所属国家的两位字母代码。

![](img/b3b9c8dd03b77b36b21ee3debec16021_7.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_8.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_9.png)

如果我们仅选择 `country` 列，会得到所有国家的列表。例如：

```sql
SELECT country FROM author ORDER BY 1;
```

![](img/b3b9c8dd03b77b36b21ee3debec16021_11.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_12.png)

`ORDER BY` 子句对结果集进行排序。这个结果集列出了作者所属的国家，并按国家字母顺序排序。在本例中，结果集显示20行，对应20位作者。但由于部分作者来自同一国家，结果集中存在重复值。

然而，我们可能只需要一份作者来源国家的列表。在这种情况下，重复值没有意义。

![](img/b3b9c8dd03b77b36b21ee3debec16021_14.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_15.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_16.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_17.png)

为了消除重复，我们使用关键字 `DISTINCT`：

![](img/b3b9c8dd03b77b36b21ee3debec16021_19.png)

```sql
SELECT DISTINCT country FROM author ORDER BY 1;
```

![](img/b3b9c8dd03b77b36b21ee3debec16021_21.png)

使用 `DISTINCT` 关键字将结果集减少到仅六行。现在我们知道了20位作者来自六个不同的国家。

![](img/b3b9c8dd03b77b36b21ee3debec16021_23.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_25.png)

---

![](img/b3b9c8dd03b77b36b21ee3debec16021_27.png)

## 对结果集进行分组统计

![](img/b3b9c8dd03b77b36b21ee3debec16021_29.png)

但我们可能还想知道有多少作者来自同一个国家。为了显示列出国家及其作者数量的结果集，我们在 `SELECT` 语句中添加 `GROUP BY` 子句。

![](img/b3b9c8dd03b77b36b21ee3debec16021_31.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_32.png)

`GROUP BY` 子句根据一个或多个列的匹配值将结果分组。在本例中，国家被分组，然后使用 `COUNT` 函数进行计数。

```sql
SELECT country, COUNT(country) FROM author GROUP BY country;
```

请注意结果集中第二列的列标题。数值 `2` 显示为列名，因为该列名在表中并非直接可用。结果集中的第二列是由 `COUNT` 函数计算得出的。

![](img/b3b9c8dd03b77b36b21ee3debec16021_34.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_35.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_36.png)

我们可以为结果集分配一个列名，而不是使用默认的列名 `2`。这通过使用 `AS` 关键字实现：

![](img/b3b9c8dd03b77b36b21ee3debec16021_37.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_38.png)

```sql
SELECT country, COUNT(country) AS count FROM author GROUP BY country;
```

![](img/b3b9c8dd03b77b36b21ee3debec16021_40.png)

在这个例子中，我们将派生列名 `2` 改为 `count`，使用 `AS count` 关键字。这有助于澄清结果集的意义。

![](img/b3b9c8dd03b77b36b21ee3debec16021_42.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_43.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_44.png)

---

## 使用 HAVING 子句进一步限制分组结果

![](img/b3b9c8dd03b77b36b21ee3debec16021_46.png)

现在我们有了来自不同国家的作者数量，我们可以通过设置一些条件来进一步限制行数。例如，我们可以检查是否有超过四位作者来自同一个国家。

![](img/b3b9c8dd03b77b36b21ee3debec16021_47.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_48.png)

要对 `GROUP BY` 子句设置条件，我们使用关键字 `HAVING`。`HAVING` 子句与 `GROUP BY` 子句结合使用。需要特别注意，`WHERE` 子句适用于整个结果集，而 `HAVING` 子句仅与 `GROUP BY` 子句配合工作。

![](img/b3b9c8dd03b77b36b21ee3debec16021_50.png)

为了检查是否有超过四位作者来自同一个国家，我们在 `SELECT` 语句中添加以下内容：

![](img/b3b9c8dd03b77b36b21ee3debec16021_51.png)

```sql
SELECT country, COUNT(country) AS count FROM author GROUP BY country HAVING COUNT(country) > 4;
```

![](img/b3b9c8dd03b77b36b21ee3debec16021_53.png)

只有拥有五位或更多作者的国家才会被列在结果集中。在本例中，这些国家是中国（有六位作者）和印度（也有六位作者）。

![](img/b3b9c8dd03b77b36b21ee3debec16021_54.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_55.png)

---

![](img/b3b9c8dd03b77b36b21ee3debec16021_57.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_58.png)

## 总结

![](img/b3b9c8dd03b77b36b21ee3debec16021_60.png)

![](img/b3b9c8dd03b77b36b21ee3debec16021_62.png)

本节课中，我们一起学习了如何从结果集中消除重复值，以及如何对结果集进行分组和进一步限制。我们介绍了 `DISTINCT` 关键字用于去重，`GROUP BY` 子句用于分组统计，以及 `HAVING` 子句用于对分组结果设置条件。掌握这些技巧将帮助你更有效地从数据库中检索和分析数据。