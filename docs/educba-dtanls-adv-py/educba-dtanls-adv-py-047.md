# 047：比较与逻辑运算符 🔍

![](img/e00075feafddefec491466aea5e9f39d_0.png)

![](img/e00075feafddefec491466aea5e9f39d_2.png)

![](img/e00075feafddefec491466aea5e9f39d_4.png)

在本节课中，我们将学习NumPy中的比较运算符和逻辑运算符。这些运算符允许我们对数组进行元素级别的比较和逻辑运算，是数据筛选和条件判断的基础。

---

![](img/e00075feafddefec491466aea5e9f39d_6.png)

![](img/e00075feafddefec491466aea5e9f39d_7.png)

## 比较运算符

![](img/e00075feafddefec491466aea5e9f39d_9.png)

在Python中，我们经常使用比较运算符。NumPy也支持这些运算符，并且可以对数组进行元素级别的比较。

![](img/e00075feafddefec491466aea5e9f39d_11.png)

首先，我们需要导入NumPy库。

![](img/e00075feafddefec491466aea5e9f39d_13.png)

```python
import numpy as np
```

![](img/e00075feafddefec491466aea5e9f39d_15.png)

接下来，我们创建两个数组 `a` 和 `b` 来进行比较。

![](img/e00075feafddefec491466aea5e9f39d_16.png)

![](img/e00075feafddefec491466aea5e9f39d_17.png)

```python
a = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9])
b = np.array([1, 5, 3, 5, 5, 2, 4, 9, 9])
```

![](img/e00075feafddefec491466aea5e9f39d_19.png)

现在，我们可以使用 `==` 运算符来比较这两个数组的对应元素是否相等。

![](img/e00075feafddefec491466aea5e9f39d_20.png)

```python
print(a == b)
```

![](img/e00075feafddefec491466aea5e9f39d_22.png)

执行上述代码会返回一个布尔值数组，其中 `True` 表示对应位置的元素相等，`False` 表示不相等。这个结果数组的数据类型是布尔型。

![](img/e00075feafddefec491466aea5e9f39d_24.png)

![](img/e00075feafddefec491466aea5e9f39d_25.png)

除了直接使用运算符，NumPy还提供了一个方法 `np.array_equal()` 来检查两个数组是否完全相等。

```python
print(np.array_equal(a, b))
```

![](img/e00075feafddefec491466aea5e9f39d_27.png)

![](img/e00075feafddefec491466aea5e9f39d_28.png)

如果两个数组的所有元素都相同，则返回 `True`，否则返回 `False`。

![](img/e00075feafddefec491466aea5e9f39d_29.png)

![](img/e00075feafddefec491466aea5e9f39d_30.png)

![](img/e00075feafddefec491466aea5e9f39d_32.png)

---

## 逻辑运算符

![](img/e00075feafddefec491466aea5e9f39d_34.png)

![](img/e00075feafddefec491466aea5e9f39d_35.png)

![](img/e00075feafddefec491466aea5e9f39d_36.png)

上一节我们介绍了比较运算符，本节中我们来看看逻辑运算符。逻辑运算符只能用于布尔数组。

![](img/e00075feafddefec491466aea5e9f39d_38.png)

首先，我们定义两个布尔数组。

![](img/e00075feafddefec491466aea5e9f39d_40.png)

![](img/e00075feafddefec491466aea5e9f39d_42.png)

```python
a_bool = np.array([1, 1, 0, 0], dtype=bool)  # 对应 True, True, False, False
b_bool = np.array([1, 0, 1, 0], dtype=bool)  # 对应 True, False, True, False
```

![](img/e00075feafddefec491466aea5e9f39d_44.png)

![](img/e00075feafddefec491466aea5e9f39d_46.png)

以下是NumPy中可用的逻辑运算符函数：

![](img/e00075feafddefec491466aea5e9f39d_48.png)

*   **`np.logical_and`**：逻辑与。当两个对应元素都为 `True` 时，结果为 `True`。
    ```python
    print(np.logical_and(a_bool, b_bool))
    ```

![](img/e00075feafddefec491466aea5e9f39d_50.png)

*   **`np.logical_or`**：逻辑或。当两个对应元素中至少有一个为 `True` 时，结果为 `True`。
    ```python
    print(np.logical_or(a_bool, b_bool))
    ```

![](img/e00075feafddefec491466aea5e9f39d_52.png)

![](img/e00075feafddefec491466aea5e9f39d_54.png)

*   **`np.logical_xor`**：逻辑异或。当两个对应元素的值不同时，结果为 `True`。
    ```python
    print(np.logical_xor(a_bool, b_bool))
    ```

![](img/e00075feafddefec491466aea5e9f39d_56.png)

*   **`np.logical_not`**：逻辑非。对数组中的每个布尔值取反。
    ```python
    print(np.logical_not(a_bool))
    ```

![](img/e00075feafddefec491466aea5e9f39d_58.png)

---

![](img/e00075feafddefec491466aea5e9f39d_60.png)

![](img/e00075feafddefec491466aea5e9f39d_61.png)

![](img/e00075feafddefec491466aea5e9f39d_62.png)

## 总结

![](img/e00075feafddefec491466aea5e9f39d_64.png)

![](img/e00075feafddefec491466aea5e9f39d_65.png)

本节课中我们一起学习了NumPy中的比较运算符和逻辑运算符。我们掌握了如何使用 `==` 等运算符进行元素级别的数组比较，以及如何使用 `np.logical_and`、`np.logical_or`、`np.logical_xor` 和 `np.logical_not` 函数对布尔数组进行逻辑运算。这些操作是进行数据筛选、条件判断和构建复杂数据操作逻辑的基础。在后续课程中，我们还将学习如何将这些运算符应用于不同形状的数组。