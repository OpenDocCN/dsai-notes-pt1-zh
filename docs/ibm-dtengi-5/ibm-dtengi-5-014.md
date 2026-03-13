# 014：分组结果集

![](img/2b293320e2298e9630af6877d2fecd7f_0.png)

在本节课中，我们将学习从关系数据库表中检索数据的高级技巧，特别是如何对结果集进行排序和分组。课程结束时，你将能够解释如何从结果集中消除重复值，并描述如何进一步限制结果集。

![](img/2b293320e2298e9630af6877d2fecd7f_2.png)

---

![](img/2b293320e2298e9630af6877d2fecd7f_3.png)

![](img/2b293320e2298e9630af6877d2fecd7f_4.png)

![](img/2b293320e2298e9630af6877d2fecd7f_5.png)

有时，SELECT 语句的结果集可能包含重复值。以我们简化的图书馆数据库模型中的作者表为例，`country` 列列出了作者所属国家的两位字母代码。

如果我们仅选择 `country` 列，会得到所有国家的列表。例如：

![](img/2b293320e2298e9630af6877d2fecd7f_7.png)

![](img/2b293320e2298e9630af6877d2fecd7f_8.png)

![](img/2b293320e2298e9630af6877d2fecd7f_9.png)

```sql
SELECT country FROM author ORDER BY 1;
```

`ORDER BY` 子句会对结果集进行排序。

![](img/2b293320e2298e9630af6877d2fecd7f_11.png)

![](img/2b293320e2298e9630af6877d2fecd7f_12.png)

这个结果集列出了作者所属的国家，并按国家字母顺序排序。在本例中，结果集显示了20行，对应20位作者。但有些作者来自同一个国家，因此结果集中包含重复项。

然而，我们可能只需要一份作者来源国家的列表。在这种情况下，重复项没有意义。

![](img/2b293320e2298e9630af6877d2fecd7f_14.png)

![](img/2b293320e2298e9630af6877d2fecd7f_15.png)

![](img/2b293320e2298e9630af6877d2fecd7f_16.png)

![](img/2b293320e2298e9630af6877d2fecd7f_17.png)

为了消除重复项，我们使用关键字 **`DISTINCT`**。

![](img/2b293320e2298e9630af6877d2fecd7f_19.png)

```sql
SELECT DISTINCT country FROM author ORDER BY 1;
```

![](img/2b293320e2298e9630af6877d2fecd7f_21.png)

使用 `DISTINCT` 关键字可以将结果集减少到只有六行。

![](img/2b293320e2298e9630af6877d2fecd7f_23.png)

![](img/2b293320e2298e9630af6877d2fecd7f_25.png)

但如果我们还想知道有多少作者来自同一个国家呢？

![](img/2b293320e2298e9630af6877d2fecd7f_27.png)

现在我们知道20位作者来自六个不同的国家。但我们可能还想了解每个国家具体有多少位作者。

![](img/2b293320e2298e9630af6877d2fecd7f_29.png)

为了显示列出国家及其作者数量的结果集，我们在 SELECT 语句中添加 **`GROUP BY`** 子句。

![](img/2b293320e2298e9630af6877d2fecd7f_31.png)

![](img/2b293320e2298e9630af6877d2fecd7f_32.png)

```sql
SELECT country, COUNT(country) FROM author GROUP BY country;
```

`GROUP BY` 子句根据一个或多个列的匹配值将结果分组。在这个例子中，国家被分组，然后使用 `COUNT` 函数进行计数。

请注意结果集中第二列的列标题。数值 `2` 被显示为列名，因为该列名在表中并不直接存在。结果集中的第二列是由 `COUNT` 函数计算得出的。

![](img/2b293320e2298e9630af6877d2fecd7f_34.png)

![](img/2b293320e2298e9630af6877d2fecd7f_35.png)

![](img/2b293320e2298e9630af6877d2fecd7f_36.png)

我们可以为结果集中的列指定一个更有意义的名称，而不是使用默认的 `2`。

![](img/2b293320e2298e9630af6877d2fecd7f_37.png)

![](img/2b293320e2298e9630af6877d2fecd7f_38.png)

我们使用 **`AS`** 关键字来实现这一点。在这个例子中，我们将派生列名 `2` 改为 `count`。

![](img/2b293320e2298e9630af6877d2fecd7f_40.png)

```sql
SELECT country, COUNT(country) AS count FROM author GROUP BY country;
```

![](img/2b293320e2298e9630af6877d2fecd7f_42.png)

![](img/2b293320e2298e9630af6877d2fecd7f_43.png)

![](img/2b293320e2298e9630af6877d2fecd7f_44.png)

这有助于澄清结果集的含义。

现在我们已经得到了来自不同国家的作者数量，我们可以通过设置一些条件来进一步限制行数。例如，我们可以检查是否有超过四位作者来自同一个国家。

![](img/2b293320e2298e9630af6877d2fecd7f_46.png)

要为 `GROUP BY` 子句设置条件，我们使用关键字 **`HAVING`**。

![](img/2b293320e2298e9630af6877d2fecd7f_47.png)

![](img/2b293320e2298e9630af6877d2fecd7f_48.png)

`HAVING` 子句与 `GROUP BY` 子句结合使用。非常重要的一点是，`WHERE` 子句是针对整个结果集的，而 `HAVING` 子句仅与 `GROUP BY` 子句配合工作。

![](img/2b293320e2298e9630af6877d2fecd7f_50.png)

要检查是否有超过四位作者来自同一个国家，我们在 SELECT 语句中添加以下内容：

![](img/2b293320e2298e9630af6877d2fecd7f_51.png)

```sql
SELECT country, COUNT(country) AS count FROM author GROUP BY country HAVING COUNT(country) > 4;
```

![](img/2b293320e2298e9630af6877d2fecd7f_53.png)

只有拥有五位或更多作者的国家才会被列在结果集中。

![](img/2b293320e2298e9630af6877d2fecd7f_54.png)

![](img/2b293320e2298e9630af6877d2fecd7f_55.png)

在这个例子中，这些国家是中国（有六位作者）和印度（也有六位作者）。

![](img/2b293320e2298e9630af6877d2fecd7f_57.png)

![](img/2b293320e2298e9630af6877d2fecd7f_58.png)

---

![](img/2b293320e2298e9630af6877d2fecd7f_60.png)

![](img/2b293320e2298e9630af6877d2fecd7f_62.png)

在本节课中，我们一起学习了如何从结果集中消除重复值，以及如何使用 `GROUP BY` 和 `HAVING` 子句对结果进行分组和进一步限制。