# 015：内置数据库函数

![](img/28564982fe25b3b2fe488956586ca822_1.png)

![](img/28564982fe25b3b2fe488956586ca822_2.png)

![](img/28564982fe25b3b2fe488956586ca822_4.png)

![](img/28564982fe25b3b2fe488956586ca822_5.png)

![](img/28564982fe25b3b2fe488956586ca822_7.png)

在本节课中，我们将学习SQL数据库中内置的函数。这些函数允许我们直接在数据库内对数据进行操作，从而减少从数据库检索的数据量，提高处理效率。

## 🧮 聚合函数（列函数）

![](img/28564982fe25b3b2fe488956586ca822_9.png)

![](img/28564982fe25b3b2fe488956586ca822_10.png)

![](img/28564982fe25b3b2fe488956586ca822_12.png)

![](img/28564982fe25b3b2fe488956586ca822_13.png)

上一节我们介绍了内置函数的基本概念。本节中，我们来看看聚合函数。聚合函数接收一组相似的值（例如一列中的所有值）作为输入，并返回单个值或NULL。

![](img/28564982fe25b3b2fe488956586ca822_14.png)

![](img/28564982fe25b3b2fe488956586ca822_16.png)

![](img/28564982fe25b3b2fe488956586ca822_17.png)

以下是常见的聚合函数示例：

![](img/28564982fe25b3b2fe488956586ca822_19.png)

![](img/28564982fe25b3b2fe488956586ca822_21.png)

![](img/28564982fe25b3b2fe488956586ca822_22.png)

![](img/28564982fe25b3b2fe488956586ca822_24.png)

*   **SUM**：用于计算一列中所有值的总和。
    *   公式：`SUM(column_name)`
    *   示例：`SELECT SUM(cost) FROM PetRescue;`
*   **MIN**：用于获取一列中的最小值。
    *   公式：`MIN(column_name)`
    *   示例：`SELECT MIN(quantity) FROM PetRescue WHERE animal = 'Dog';`
*   **MAX**：用于获取一列中的最大值。
    *   公式：`MAX(column_name)`
    *   示例：`SELECT MAX(quantity) FROM PetRescue;`
*   **AVG**：用于返回一列的平均值。
    *   公式：`AVG(column_name)`
    *   示例：`SELECT AVG(cost) FROM PetRescue;`

![](img/28564982fe25b3b2fe488956586ca822_25.png)

![](img/28564982fe25b3b2fe488956586ca822_27.png)

> 注意：使用聚合函数时，默认情况下结果集中的列会被赋予一个编号。可以使用`AS`关键字为结果列显式命名，例如：`SELECT SUM(cost) AS sum_of_cost FROM PetRescue;`。

![](img/28564982fe25b3b2fe488956586ca822_28.png)

![](img/28564982fe25b3b2fe488956586ca822_29.png)

![](img/28564982fe25b3b2fe488956586ca822_30.png)

![](img/28564982fe25b3b2fe488956586ca822_31.png)

![](img/28564982fe25b3b2fe488956586ca822_32.png)

聚合函数也可以应用于数据的子集，而不仅仅是整个列。例如，可以结合`WHERE`子句使用。

![](img/28564982fe25b3b2fe488956586ca822_34.png)

![](img/28564982fe25b3b2fe488956586ca822_35.png)

![](img/28564982fe25b3b2fe488956586ca822_36.png)

![](img/28564982fe25b3b2fe488956586ca822_38.png)

## 🔢 标量与字符串函数

![](img/28564982fe25b3b2fe488956586ca822_39.png)

![](img/28564982fe25b3b2fe488956586ca822_40.png)

![](img/28564982fe25b3b2fe488956586ca822_42.png)

![](img/28564982fe25b3b2fe488956586ca822_43.png)

![](img/28564982fe25b3b2fe488956586ca822_44.png)

了解了聚合函数后，我们来看看标量函数。标量函数对单个值执行操作，例如对数值进行四舍五入。

![](img/28564982fe25b3b2fe488956586ca822_46.png)

![](img/28564982fe25b3b2fe488956586ca822_47.png)

![](img/28564982fe25b3b2fe488956586ca822_48.png)

以下是一些标量函数，特别是字符串函数的示例：

![](img/28564982fe25b3b2fe488956586ca822_50.png)

*   **ROUND**：对数值进行四舍五入。
    *   公式：`ROUND(column_name)`
    *   示例：`SELECT ROUND(cost) FROM PetRescue;`
*   **LENGTH**：返回字符串的长度。
    *   公式：`LENGTH(column_name)`
    *   示例：`SELECT LENGTH(animal) FROM PetRescue;`
*   **UPPER / UCASE**：将字符串转换为大写。
    *   公式：`UPPER(column_name)` 或 `UCASE(column_name)`
    *   示例：`SELECT UPPER(animal) FROM PetRescue;`
*   **LOWER / LCASE**：将字符串转换为小写。
    *   公式：`LOWER(column_name)` 或 `LCASE(column_name)`
    *   示例：`SELECT LOWER(animal) FROM PetRescue;`

![](img/28564982fe25b3b2fe488956586ca822_52.png)

![](img/28564982fe25b3b2fe488956586ca822_53.png)

![](img/28564982fe25b3b2fe488956586ca822_55.png)

![](img/28564982fe25b3b2fe488956586ca822_56.png)

标量函数同样可以在`WHERE`子句中使用。这在不确定表中数据存储为大写、小写还是混合格式时，用于匹配条件非常有用。例如：`SELECT * FROM PetRescue WHERE LOWER(animal) = 'cat';`。

![](img/28564982fe25b3b2fe488956586ca822_57.png)

![](img/28564982fe25b3b2fe488956586ca822_59.png)

![](img/28564982fe25b3b2fe488956586ca822_60.png)

![](img/28564982fe25b3b2fe488956586ca822_61.png)

此外，还可以让一个函数对另一个函数的输出进行操作，例如：`SELECT DISTINCT UPPER(animal) FROM PetRescue;`。

![](img/28564982fe25b3b2fe488956586ca822_63.png)

![](img/28564982fe25b3b2fe488956586ca822_64.png)

![](img/28564982fe25b3b2fe488956586ca822_65.png)

## 📝 总结

![](img/28564982fe25b3b2fe488956586ca822_66.png)

![](img/28564982fe25b3b2fe488956586ca822_68.png)

![](img/28564982fe25b3b2fe488956586ca822_69.png)

![](img/28564982fe25b3b2fe488956586ca822_70.png)

本节课中，我们一起学习了SQL的内置函数。我们首先介绍了**聚合函数**，如`SUM`、`MIN`、`MAX`和`AVG`，它们用于对列数据进行汇总计算。接着，我们探讨了**标量函数**和**字符串函数**，如`ROUND`、`LENGTH`、`UPPER`和`LOWER`，它们用于对单个数据值进行操作和格式化。掌握这些函数能帮助我们在数据库层面高效地处理和转换数据。