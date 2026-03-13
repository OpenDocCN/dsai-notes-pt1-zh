# 024：使用真实数据集

![](img/9798c5590d88ccb9513ec3fdc3b1341b_1.png)

在本节课中，我们将学习如何在实际工作中处理真实世界的数据集。我们将重点介绍CSV文件的特性、如何将其加载到数据库中，以及查询数据时需要注意的关键事项，例如列名的大小写、特殊字符处理等。

---

![](img/9798c5590d88ccb9513ec3fdc3b1341b_3.png)

## 📁 理解CSV文件格式

许多真实世界的数据集以`.CSV`文件的形式提供。这些是文本文件，其中的数据值通常由逗号分隔。在某些情况下，也可能使用其他分隔符，例如分号。

![](img/9798c5590d88ccb9513ec3fdc3b1341b_5.png)

---

## 🐕 示例数据集：Dogs.csv

![](img/9798c5590d88ccb9513ec3fdc3b1341b_7.png)

在本视频中，我们将使用一个名为`Dogs.csv`的虚构数据集作为示例。该数据集包含狗的名字和品种，我们将用它来说明一些概念，这些概念随后可以应用于真实数据集。

以下是`Dogs.csv`文件的部分内容示例：

![](img/9798c5590d88ccb9513ec3fdc3b1341b_9.png)

![](img/9798c5590d88ccb9513ec3fdc3b1341b_10.png)

| ID | Name of dog | Breed (dominant breed if not pure breed) |
|----|-------------|------------------------------------------|
| 1  | Wolfy       | German Shepherd                          |
| 2  | Fluffy      | Pomeranian                               |
| 3  | Huggy       | Labrador                                 |

![](img/9798c5590d88ccb9513ec3fdc3b1341b_12.png)

第一行通常包含属性标签，这些标签映射到数据库表中的列名。在本例中，第一行包含三个属性名称：`ID`、`Name of dog`和`Breed (dominant breed if not pure breed)`。

![](img/9798c5590d88ccb9513ec3fdc3b1341b_14.png)

---

## 🚀 加载CSV文件到数据库

![](img/9798c5590d88ccb9513ec3fdc3b1341b_16.png)

正如我们所见，CSV文件的第一行通常是包含属性名称的表头行。

![](img/9798c5590d88ccb9513ec3fdc3b1341b_17.png)

如果您使用数据库控制台中的可视化加载工具将数据加载到数据库中，请确保启用“首行为表头”选项。

这将把CSV文件第一行中的属性名称映射为数据库表中的列名，其余行则映射为表中的数据行。

![](img/9798c5590d88ccb9513ec3fdc3b1341b_19.png)

请注意，默认的列名可能并不总是对数据库或查询友好。如果出现这种情况，您可能需要在创建表之前编辑这些列名。

![](img/9798c5590d88ccb9513ec3fdc3b1341b_20.png)

![](img/9798c5590d88ccb9513ec3fdc3b1341b_21.png)

---

## 🔤 查询大小写混合的列名

现在，我们来讨论查询小写或大小写混合（即大写和小写组合）的列名。

![](img/9798c5590d88ccb9513ec3fdc3b1341b_23.png)

![](img/9798c5590d88ccb9513ec3fdc3b1341b_24.png)

假设我们使用CSV文件中的默认列名加载了`Dogs.csv`文件。如果我们尝试使用查询`SELECT id FROM dogs;`来检索`ID`列的内容，可能会收到错误提示，指出`id`无效。

这是因为数据库解析器默认假定列名为大写。然而，当我们把CSV文件加载到数据库时，`ID`列名是大小写混合的（即大写`I`和小写`D`）。

在这种情况下，要选择具有大小写混合名称的列中的数据，我们需要在双引号内指定正确大小写的列名，如下所示：

```sql
SELECT "ID" FROM dogs;
```

请确保在列名周围使用双引号，而不是单引号。

---

![](img/9798c5590d88ccb9513ec3fdc3b1341b_26.png)

## ⚠️ 查询包含空格和特殊字符的列名

![](img/9798c5590d88ccb9513ec3fdc3b1341b_27.png)

![](img/9798c5590d88ccb9513ec3fdc3b1341b_28.png)

如果列名包含空格，数据库默认可能会将它们映射为下划线。

![](img/9798c5590d88ccb9513ec3fdc3b1341b_30.png)

例如，在`Name of dog`列中，三个单词之间有空格。数据库可能会将其更改为`name_of_dog`。

![](img/9798c5590d88ccb9513ec3fdc3b1341b_32.png)

其他特殊字符，如括号或方括号，也可能被映射为下划线。

因此，在编写查询时，请确保在引号内使用正确的大小写格式，并将特殊字符替换为下划线，如下例所示：

![](img/9798c5590d88ccb9513ec3fdc3b1341b_34.png)

![](img/9798c5590d88ccb9513ec3fdc3b1341b_35.png)

```sql
SELECT "ID", "name_of_dog", "breed__dominant_breed_if_not_pure_breed_" FROM dogs;
```

![](img/9798c5590d88ccb9513ec3fdc3b1341b_36.png)

请注意双引号内单词之间的下划线分隔符。同时，注意`breed`和`dominant`之间的双下划线，如示例所示。最后，查询末尾`breed`单词后的尾随下划线也很重要，它用于替代原列名中的右括号。

---

## 🐍 在Jupyter Notebooks中使用引号

![](img/9798c5590d88ccb9513ec3fdc3b1341b_38.png)

![](img/9798c5590d88ccb9513ec3fdc3b1341b_39.png)

在Jupyter Notebooks中使用引号时，您可能会先将查询分配给Python变量，然后在笔记中执行它们。

![](img/9798c5590d88ccb9513ec3fdc3b1341b_40.png)

![](img/9798c5590d88ccb9513ec3fdc3b1341b_41.png)

在这种情况下，如果您的查询包含双引号（例如，用于指定大小写混合的列名），您可以通过对Python变量使用单引号来包裹整个SQL查询，而对列名使用双引号来区分引号。

例如：
```python
selectQuery = 'SELECT "ID" FROM dogs;'
```

现在，如果您需要在查询中指定单引号（例如，在WHERE子句中指定一个值），该怎么办？在这种情况下，您可以使用反斜杠作为转义字符，如下所示：

```python
selectQuery = 'SELECT * FROM dogs WHERE "name_of_dog" = \'Huggy\';'
```

---

## 📝 处理长查询以提高可读性

如果您有非常长的查询，例如连接查询或嵌套查询，将查询拆分为多行以提高可读性可能会很有用。

在Python Notebooks中，您可以使用反斜杠字符表示延续到下一行，如下例所示：

![](img/9798c5590d88ccb9513ec3fdc3b1341b_43.png)

![](img/9798c5590d88ccb9513ec3fdc3b1341b_44.png)

```python
%sql SELECT "ID", "name_of_dog" \
FROM dogs \
WHERE "name_of_dog" = 'Huggy';
```

![](img/9798c5590d88ccb9513ec3fdc3b1341b_46.png)

![](img/9798c5590d88ccb9513ec3fdc3b1341b_47.png)

此时，花点时间回顾一下这些特殊字符会很有帮助。

请记住，如果您在Python Notebook中将查询拆分为多行而没有使用反斜杠，可能会收到错误。

![](img/9798c5590d88ccb9513ec3fdc3b1341b_49.png)

---

![](img/9798c5590d88ccb9513ec3fdc3b1341b_51.png)

## ✨ 使用SQL Magic命令

在Jupyter Notebooks中使用SQL Magic时，可以在单元格的第一行使用`%%sql`。这意味着单元格的其余内容将由SQL Magic解释。

例如：
```sql
%%sql
SELECT "ID", "name_of_dog"
FROM dogs
WHERE "name_of_dog" = 'Huggy';
```

再次请注意示例中显示的特殊字符。当使用`%%sql`时，每行末尾不需要反斜杠。

---

## 🔍 限制检索的行数

![](img/9798c5590d88ccb9513ec3fdc3b1341b_53.png)

![](img/9798c5590d88ccb9513ec3fdc3b1341b_54.png)

![](img/9798c5590d88ccb9513ec3fdc3b1341b_55.png)

此时您可能会问，如何限制检索的行数？这是一个很好的问题，因为一个表可能包含数千甚至数百万行，而您可能只想查看一些样本数据，或者只看几行以了解表中包含的数据类型。

您可能想直接使用`SELECT * FROM table_name;`来检索结果到Pandas DataFrame，然后对其使用`.head()`函数。但这样做可能会导致查询运行很长时间。

![](img/9798c5590d88ccb9513ec3fdc3b1341b_57.png)

![](img/9798c5590d88ccb9513ec3fdc3b1341b_58.png)

相反，您可以使用`LIMIT`子句来限制结果集。例如，使用以下查询仅检索名为`census_data`的表中的前三行：

```sql
SELECT * FROM census_data LIMIT 3;
```

![](img/9798c5590d88ccb9513ec3fdc3b1341b_60.png)

---

![](img/9798c5590d88ccb9513ec3fdc3b1341b_61.png)

![](img/9798c5590d88ccb9513ec3fdc3b1341b_62.png)

## 🎯 总结

![](img/9798c5590d88ccb9513ec3fdc3b1341b_64.png)

在本节课中，我们一起探讨了处理真实世界数据集时的一些注意事项和技巧。我们学习了CSV文件的结构、如何正确加载数据、处理大小写混合及包含特殊字符的列名、在Jupyter Notebooks中编写查询的注意事项，以及如何使用`LIMIT`子句高效地预览数据。掌握这些基础知识将帮助您更自信地处理实际数据分析任务。