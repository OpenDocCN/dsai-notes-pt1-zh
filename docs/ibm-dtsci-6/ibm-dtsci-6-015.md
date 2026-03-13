# 015：内置数据库函数

![](img/94957eef39b55bdf2096945069ff0e89_1.png)

在本节课中，我们将学习SQL中内置的数据库函数。这些函数允许我们直接在数据库内部对数据进行操作，从而减少需要从数据库检索的数据量，提高处理效率，并降低网络带宽的使用。

![](img/94957eef39b55bdf2096945069ff0e89_2.png)

---

![](img/94957eef39b55bdf2096945069ff0e89_4.png)

## 🏁 内置数据库函数简介

![](img/94957eef39b55bdf2096945069ff0e89_5.png)

虽然可以先从数据库获取数据，然后在应用程序或笔记本中执行操作，但大多数数据库都自带内置函数。

![](img/94957eef39b55bdf2096945069ff0e89_7.png)

![](img/94957eef39b55bdf2096945069ff0e89_8.png)

这些函数可以包含在SQL语句中，允许您直接在数据库内部对数据进行操作。

使用数据库函数可以显著减少需要从数据库检索的数据量，即减少网络流量和带宽使用。在处理大型数据集时，使用内置函数可能比先将数据检索到应用程序中再执行函数更快。

请注意，也可以在数据库中创建自己的函数，即用户定义函数，但这属于更高级的主题。

---

## 🐾 示例数据表：宠物救援表

![](img/94957eef39b55bdf2096945069ff0e89_10.png)

为了本课的示例，我们考虑一个宠物救援组织数据库中的“宠物救援”表。

![](img/94957eef39b55bdf2096945069ff0e89_11.png)

它记录了救援交易的详细信息，包含以下列：`ID`、`animal`、`quantity`、`cost`和`rescue_date`。为了本课的目的，我们已经填充了几行数据，如下所示。

![](img/94957eef39b55bdf2096945069ff0e89_13.png)

---

![](img/94957eef39b55bdf2096945069ff0e89_14.png)

![](img/94957eef39b55bdf2096945069ff0e89_15.png)

## 📈 什么是聚合函数（列函数）？

上一节我们介绍了内置函数的基本概念，本节中我们来看看聚合函数。

![](img/94957eef39b55bdf2096945069ff0e89_17.png)

![](img/94957eef39b55bdf2096945069ff0e89_18.png)

聚合函数接收一组相似的值（例如列中的所有值）作为输入，并返回单个值或`NULL`。

聚合函数的例子包括`SUM`、`MIN`、`MAX`、`AVG`等。

让我们基于宠物救援表看一些例子。

![](img/94957eef39b55bdf2096945069ff0e89_20.png)

![](img/94957eef39b55bdf2096945069ff0e89_21.png)

---

![](img/94957eef39b55bdf2096945069ff0e89_23.png)

![](img/94957eef39b55bdf2096945069ff0e89_24.png)

### 求和函数（SUM）

`SUM`函数用于将列中的所有值相加。要使用该函数，您需要在函数名后的括号内写入列名。

例如，要将`cost`列中的所有值相加：
```sql
SELECT SUM(cost) FROM PetRescue;
```

![](img/94957eef39b55bdf2096945069ff0e89_26.png)

当您使用聚合函数时，结果集中的列默认会得到一个数字作为列名。可以显式地为结果列命名。

![](img/94957eef39b55bdf2096945069ff0e89_27.png)

![](img/94957eef39b55bdf2096945069ff0e89_28.png)

![](img/94957eef39b55bdf2096945069ff0e89_29.png)

例如，假设我们想将上一个查询的输出列命名为`sum_of_cost`：
```sql
SELECT SUM(cost) AS sum_of_cost FROM PetRescue;
```
请注意此示例中`AS`的用法。

![](img/94957eef39b55bdf2096945069ff0e89_30.png)

![](img/94957eef39b55bdf2096945069ff0e89_31.png)

---

### 最小值与最大值函数（MIN, MAX）

![](img/94957eef39b55bdf2096945069ff0e89_33.png)

`MIN`函数，顾名思义，用于获取最低值。同样地，`MAX`函数用于获取最高值。

![](img/94957eef39b55bdf2096945069ff0e89_34.png)

![](img/94957eef39b55bdf2096945069ff0e89_35.png)

![](img/94957eef39b55bdf2096945069ff0e89_36.png)

例如，要获取单次交易中任何动物救援的最大数量：
```sql
SELECT MAX(quantity) FROM PetRescue;
```

聚合函数也可以应用于数据的子集，而不是整个列。

![](img/94957eef39b55bdf2096945069ff0e89_38.png)

例如，要获取`ID`列中关于狗的最小值：
```sql
SELECT MIN(ID) FROM PetRescue WHERE animal = 'Dog';
```

![](img/94957eef39b55bdf2096945069ff0e89_39.png)

![](img/94957eef39b55bdf2096945069ff0e89_40.png)

---

### 平均值函数（AVG）

![](img/94957eef39b55bdf2096945069ff0e89_42.png)

![](img/94957eef39b55bdf2096945069ff0e89_43.png)

![](img/94957eef39b55bdf2096945069ff0e89_44.png)

`AVG`函数用于返回平均值或均值。

例如，要指定`cost`的平均值：
```sql
SELECT AVG(cost) FROM PetRescue;
```

![](img/94957eef39b55bdf2096945069ff0e89_46.png)

![](img/94957eef39b55bdf2096945069ff0e89_47.png)

请注意，我们可以在列之间执行数学运算，然后对结果应用聚合函数。

![](img/94957eef39b55bdf2096945069ff0e89_48.png)

例如，计算每只狗的平均成本：
```sql
SELECT AVG(cost / quantity) FROM PetRescue WHERE animal = 'Dog';
```
在这种情况下，成本是针对多个单位的，因此我们首先将成本除以救援的数量。

---

## 🔢 标量函数与字符串函数

![](img/94957eef39b55bdf2096945069ff0e89_50.png)

上一节我们介绍了聚合函数，本节中我们来看看标量函数。

![](img/94957eef39b55bdf2096945069ff0e89_52.png)

标量函数对单个值执行操作。

![](img/94957eef39b55bdf2096945069ff0e89_53.png)

例如，将`cost`列中的每个值四舍五入到最接近的整数：
```sql
SELECT ROUND(cost) FROM PetRescue;
```

有一类称为字符串函数的标量函数，可用于对字符串（即`CHAR`和`VARCHAR`值）进行操作。

![](img/94957eef39b55bdf2096945069ff0e89_55.png)

以下是字符串函数的例子。

![](img/94957eef39b55bdf2096945069ff0e89_56.png)

![](img/94957eef39b55bdf2096945069ff0e89_57.png)

例如，检索`animal`列中每个值的长度：
```sql
SELECT LENGTH(animal) FROM PetRescue;
```

`UPPER`和`LOWER`函数可用于返回字符串的大写或小写值。

![](img/94957eef39b55bdf2096945069ff0e89_59.png)

![](img/94957eef39b55bdf2096945069ff0e89_60.png)

![](img/94957eef39b55bdf2096945069ff0e89_61.png)

例如，检索大写的`animal`值：
```sql
SELECT UPPER(animal) FROM PetRescue;
```

标量函数可以在`WHERE`子句中使用。

例如，获取`animal`列为“猫”的小写值：
```sql
SELECT * FROM PetRescue WHERE LOWER(animal) = 'cat';
```
如果您不确定表中的值是以大写、小写还是混合大小写存储的，这种类型的语句对于在`WHERE`子句中匹配值非常有用。

![](img/94957eef39b55bdf2096945069ff0e89_63.png)

您还可以让一个函数对另一个函数的输出进行操作。

![](img/94957eef39b55bdf2096945069ff0e89_64.png)

![](img/94957eef39b55bdf2096945069ff0e89_65.png)

![](img/94957eef39b55bdf2096945069ff0e89_66.png)

例如，获取`animal`列的唯一值并转换为大写：
```sql
SELECT DISTINCT UPPER(animal) FROM PetRescue;
```

---

![](img/94957eef39b55bdf2096945069ff0e89_68.png)

## ✅ 总结

![](img/94957eef39b55bdf2096945069ff0e89_69.png)

![](img/94957eef39b55bdf2096945069ff0e89_70.png)

在本节课中，我们一起学习了一些内置的SQL聚合函数，如`SUM`、`MIN`、`MAX`和`AVG`。我们还学习了标量函数和字符串函数，如`ROUND`、`LOWER`和`UPPER`。掌握这些函数将帮助您更高效地在数据库层面处理和分析数据。