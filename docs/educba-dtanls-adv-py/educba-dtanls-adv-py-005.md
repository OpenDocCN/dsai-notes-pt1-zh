# 005：列表方法与其他数据结构 🐍

![](img/a36ee59f55dfb174f458128a80f1437e_0.png)

![](img/a36ee59f55dfb174f458128a80f1437e_2.png)

![](img/a36ee59f55dfb174f458128a80f1437e_4.png)

![](img/a36ee59f55dfb174f458128a80f1437e_5.png)

![](img/a36ee59f55dfb174f458128a80f1437e_6.png)

![](img/a36ee59f55dfb174f458128a80f1437e_7.png)

![](img/a36ee59f55dfb174f458128a80f1437e_9.png)

在本节课中，我们将学习Python列表的更多实用方法，并快速了解元组、集合和字典这三种基本数据结构。掌握这些知识是进行高效数据分析的基础。

![](img/a36ee59f55dfb174f458128a80f1437e_10.png)

![](img/a36ee59f55dfb174f458128a80f1437e_12.png)

![](img/a36ee59f55dfb174f458128a80f1437e_13.png)

上一节我们介绍了列表的基本操作，本节中我们来看看列表的其他内置方法。

![](img/a36ee59f55dfb174f458128a80f1437e_15.png)

![](img/a36ee59f55dfb174f458128a80f1437e_16.png)

![](img/a36ee59f55dfb174f458128a80f1437e_17.png)

## 列表的其他方法

![](img/a36ee59f55dfb174f458128a80f1437e_19.png)

![](img/a36ee59f55dfb174f458128a80f1437e_20.png)

![](img/a36ee59f55dfb174f458128a80f1437e_22.png)

![](img/a36ee59f55dfb174f458128a80f1437e_23.png)

列表对象提供了多种方法来修改和查询数据。以下是几个关键方法：

![](img/a36ee59f55dfb174f458128a80f1437e_25.png)

![](img/a36ee59f55dfb174f458128a80f1437e_27.png)

![](img/a36ee59f55dfb174f458128a80f1437e_29.png)

*   **`insert` 方法**：在列表的指定索引位置插入一个新元素。其语法为 `list.insert(index, value)`。
    *   例如，`LST.insert(2, 15)` 会在索引2的位置插入数值15。

![](img/a36ee59f55dfb174f458128a80f1437e_31.png)

![](img/a36ee59f55dfb174f458128a80f1437e_32.png)

![](img/a36ee59f55dfb174f458128a80f1437e_33.png)

*   **`pop` 方法**：移除列表中的一个元素。默认移除并返回最后一个元素。其语法为 `list.pop()` 或 `list.pop(index)`。
    *   例如，`LST.pop()` 移除最后一个元素。
    *   例如，`LST.pop(-3)` 移除从末尾开始计数的第3个元素（即索引为 `len(LST)-3` 的元素）。

![](img/a36ee59f55dfb174f458128a80f1437e_34.png)

![](img/a36ee59f55dfb174f458128a80f1437e_36.png)

![](img/a36ee59f55dfb174f458128a80f1437e_38.png)

*   **`sort` 方法**：对列表本身进行升序排序。其语法为 `list.sort()`。

![](img/a36ee59f55dfb174f458128a80f1437e_40.png)

![](img/a36ee59f55dfb174f458128a80f1437e_41.png)

![](img/a36ee59f55dfb174f458128a80f1437e_42.png)

*   **`reverse` 方法**：将列表中的元素反向排列。其语法为 `list.reverse()`。

![](img/a36ee59f55dfb174f458128a80f1437e_44.png)

![](img/a36ee59f55dfb174f458128a80f1437e_45.png)

![](img/a36ee59f55dfb174f458128a80f1437e_47.png)

![](img/a36ee59f55dfb174f458128a80f1437e_48.png)

![](img/a36ee59f55dfb174f458128a80f1437e_50.png)

此外，Python还有一些内置函数可以直接作用于列表：

![](img/a36ee59f55dfb174f458128a80f1437e_52.png)

![](img/a36ee59f55dfb174f458128a80f1437e_53.png)

![](img/a36ee59f55dfb174f458128a80f1437e_55.png)

![](img/a36ee59f55dfb174f458128a80f1437e_56.png)

*   **`sum` 函数**：计算列表中所有数值元素的总和。其语法为 `sum(list)`。
*   **`len` 函数**：获取列表的长度（元素个数）。其语法为 `len(list)`。

![](img/a36ee59f55dfb174f458128a80f1437e_58.png)

---

![](img/a36ee59f55dfb174f458128a80f1437e_60.png)

![](img/a36ee59f55dfb174f458128a80f1437e_61.png)

![](img/a36ee59f55dfb174f458128a80f1437e_62.png)

![](img/a36ee59f55dfb174f458128a80f1437e_63.png)

![](img/a36ee59f55dfb174f458128a80f1437e_65.png)

![](img/a36ee59f55dfb174f458128a80f1437e_67.png)

## 列表推导式

![](img/a36ee59f55dfb174f458128a80f1437e_68.png)

列表推导式提供了一种简洁、高效的方式来创建新列表。其核心思想是对一个序列中的每个元素应用一个表达式或进行过滤。

![](img/a36ee59f55dfb174f458128a80f1437e_70.png)

其基本语法结构为：
```python
新列表 = [对元素的表达式 for 元素 in 旧列表 if 条件]
```

![](img/a36ee59f55dfb174f458128a80f1437e_71.png)

![](img/a36ee59f55dfb174f458128a80f1437e_72.png)

![](img/a36ee59f55dfb174f458128a80f1437e_73.png)

![](img/a36ee59f55dfb174f458128a80f1437e_74.png)

![](img/a36ee59f55dfb174f458128a80f1437e_75.png)

![](img/a36ee59f55dfb174f458128a80f1437e_76.png)

![](img/a36ee59f55dfb174f458128a80f1437e_77.png)

![](img/a36ee59f55dfb174f458128a80f1437e_78.png)

例如，将一个列表中的每个元素求平方：
```python
SQ_list = [element**2 for element in LST]
```

![](img/a36ee59f55dfb174f458128a80f1437e_80.png)

![](img/a36ee59f55dfb174f458128a80f1437e_81.png)

![](img/a36ee59f55dfb174f458128a80f1437e_82.png)

列表推导式在数据分析中非常实用，例如进行单位转换。假设有一个摄氏温度列表，需要将其转换为华氏温度：
```python
temp_celsius = [20, 22, 23, 24, 25, 26, 27, 25]
temp_fahrenheit = [t * 9/5 + 32 for t in temp_celsius]
```
这个例子展示了如何通过一行代码完成整个列表的批量计算。

![](img/a36ee59f55dfb174f458128a80f1437e_83.png)

![](img/a36ee59f55dfb174f458128a80f1437e_85.png)

![](img/a36ee59f55dfb174f458128a80f1437e_87.png)

![](img/a36ee59f55dfb174f458128a80f1437e_89.png)

---

![](img/a36ee59f55dfb174f458128a80f1437e_90.png)

![](img/a36ee59f55dfb174f458128a80f1437e_91.png)

![](img/a36ee59f55dfb174f458128a80f1437e_93.png)

![](img/a36ee59f55dfb174f458128a80f1437e_95.png)

## 元组、集合与字典简介

![](img/a36ee59f55dfb174f458128a80f1437e_97.png)

![](img/a36ee59f55dfb174f458128a80f1437e_98.png)

![](img/a36ee59f55dfb174f458128a80f1437e_99.png)

了解列表后，我们快速浏览一下Python中其他几种重要的数据结构。

![](img/a36ee59f55dfb174f458128a80f1437e_100.png)

![](img/a36ee59f55dfb174f458128a80f1437e_102.png)

![](img/a36ee59f55dfb174f458128a80f1437e_103.png)

### 元组

![](img/a36ee59f55dfb174f458128a80f1437e_104.png)

![](img/a36ee59f55dfb174f458128a80f1437e_105.png)

![](img/a36ee59f55dfb174f458128a80f1437e_107.png)

![](img/a36ee59f55dfb174f458128a80f1437e_109.png)

![](img/a36ee59f55dfb174f458128a80f1437e_111.png)

元组使用圆括号 `()` 定义，例如 `T = (5, 2, 8, 4)`。元组与列表的主要区别在于**元组是不可变的**，创建后不能修改其元素。尝试执行 `T[0] = 10` 会导致错误。元组常用的方法只有 `count()` 和 `index()`。

### 集合

![](img/a36ee59f55dfb174f458128a80f1437e_113.png)

![](img/a36ee59f55dfb174f458128a80f1437e_114.png)

![](img/a36ee59f55dfb174f458128a80f1437e_115.png)

集合使用花括号 `{}` 定义，例如 `S = {1, 2, 3, 5, 5, 5}`。集合的核心特性是**元素唯一且无序**。打印集合 `S` 会得到 `{1, 2, 3, 5}`，重复的5被自动去重。由于无序，不能通过索引访问集合元素。集合常用于成员检测、去重以及数学运算（如并集、交集）。

![](img/a36ee59f55dfb174f458128a80f1437e_116.png)

![](img/a36ee59f55dfb174f458128a80f1437e_117.png)

![](img/a36ee59f55dfb174f458128a80f1437e_119.png)

### 字典

![](img/a36ee59f55dfb174f458128a80f1437e_121.png)

![](img/a36ee59f55dfb174f458128a80f1437e_123.png)

![](img/a36ee59f55dfb174f458128a80f1437e_125.png)

![](img/a36ee59f55dfb174f458128a80f1437e_127.png)

字典使用花括号 `{}` 定义，但其元素是 **`键: 值`** 对，例如 `D = {25: ‘Roy‘, 50: ‘Noop‘, 80: ‘Re‘}`。字典的键相当于自定义的索引，可以通过键来快速访问对应的值，例如 `D[25]` 会返回 `‘Roy‘`。字典在存储和查询具有映射关系的数据时非常高效。

![](img/a36ee59f55dfb174f458128a80f1437e_129.png)

![](img/a36ee59f55dfb174f458128a80f1437e_131.png)

![](img/a36ee59f55dfb174f458128a80f1437e_132.png)

---

![](img/a36ee59f55dfb174f458128a80f1437e_134.png)

![](img/a36ee59f55dfb174f458128a80f1437e_135.png)

![](img/a36ee59f55dfb174f458128a80f1437e_136.png)

本节课中我们一起学习了Python列表的进阶方法，掌握了强大的列表推导式技巧，并快速了解了元组、集合和字典这三种基本数据结构的特点与用途。这些是构建复杂数据处理流程的基石，在后续学习NumPy、Pandas等数据分析库时将会频繁用到。