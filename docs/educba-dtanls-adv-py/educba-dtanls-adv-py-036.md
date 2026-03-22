数据分析高级Python：P36：Python列表与NumPy数组性能对比 🚀

![](img/5b411cec3b5349ef62c69524f2488cff_1.png)

![](img/5b411cec3b5349ef62c69524f2488cff_2.png)

在本节课中，我们将学习如何通过一个简单的性能对比实验，直观地理解Python原生列表与NumPy数组在执行效率上的显著差异。我们将编写两个函数，分别使用列表和NumPy数组进行元素加法运算，并测量其执行时间。

![](img/5b411cec3b5349ef62c69524f2488cff_3.png)

---

### 概述

我们将创建两个函数：一个使用Python原生列表进行循环加法，另一个使用NumPy数组进行向量化加法。通过对比两者的执行时间，可以清晰地看到NumPy在数值计算上的高效性。

![](img/5b411cec3b5349ef62c69524f2488cff_5.png)

![](img/5b411cec3b5349ef62c69524f2488cff_6.png)

---

![](img/5b411cec3b5349ef62c69524f2488cff_7.png)

![](img/5b411cec3b5349ef62c69524f2488cff_8.png)

![](img/5b411cec3b5349ef62c69524f2488cff_9.png)

![](img/5b411cec3b5349ef62c69524f2488cff_10.png)

![](img/5b411cec3b5349ef62c69524f2488cff_11.png)

### Python列表版本实现

![](img/5b411cec3b5349ef62c69524f2488cff_12.png)

![](img/5b411cec3b5349ef62c69524f2488cff_13.png)

首先，我们来实现使用Python列表进行元素加法的函数。其核心思路是遍历两个列表，将对应位置的元素相加，并将结果存入一个新列表。

以下是该函数的具体步骤：

1.  记录操作开始前的时间 `t1`。
2.  创建两个包含1000个元素的列表 `x` 和 `y`。
3.  创建一个空列表 `z`，用于存放结果。
4.  使用 `for` 循环遍历索引，将 `x[i]` 与 `y[i]` 相加，结果追加到列表 `z` 中。
5.  记录操作结束后的时间 `t2`。
6.  函数返回时间差 `t2 - t1`，即整个操作所耗费的时间。

![](img/5b411cec3b5349ef62c69524f2488cff_15.png)

![](img/5b411cec3b5349ef62c69524f2488cff_16.png)

![](img/5b411cec3b5349ef62c69524f2488cff_17.png)

![](img/5b411cec3b5349ef62c69524f2488cff_18.png)

对应的代码逻辑如下：
```python
def calculate_time_python():
    import time
    t1 = time.time()
    x = list(range(1000))
    y = list(range(1000))
    z = []
    for i in range(len(x)):
        z.append(x[i] + y[i])
    t2 = time.time()
    return t2 - t1
```

![](img/5b411cec3b5349ef62c69524f2488cff_19.png)

![](img/5b411cec3b5349ef62c69524f2488cff_20.png)

![](img/5b411cec3b5349ef62c69524f2488cff_21.png)

---

![](img/5b411cec3b5349ef62c69524f2488cff_23.png)

### NumPy数组版本实现

![](img/5b411cec3b5349ef62c69524f2488cff_24.png)

![](img/5b411cec3b5349ef62c69524f2488cff_25.png)

![](img/5b411cec3b5349ef62c69524f2488cff_27.png)

上一节我们介绍了使用Python列表进行计算的笨拙方法。本节中，我们来看看如何使用NumPy数组以更高效、简洁的方式完成同样的任务。

![](img/5b411cec3b5349ef62c69524f2488cff_29.png)

![](img/5b411cec3b5349ef62c69524f2488cff_31.png)

以下是该函数的具体步骤：

1.  记录操作开始前的时间 `t1`。
2.  使用 `np.arange(1000)` 直接创建两个包含1000个元素的NumPy数组 `x` 和 `y`。这比先创建列表再转换更高效。
3.  利用NumPy的**向量化**操作，直接使用 `z = x + y` 完成整个数组的加法，无需显式循环。
4.  记录操作结束后的时间 `t2`。
5.  函数返回时间差 `t2 - t1`。

![](img/5b411cec3b5349ef62c69524f2488cff_33.png)

![](img/5b411cec3b5349ef62c69524f2488cff_34.png)

![](img/5b411cec3b5349ef62c69524f2488cff_35.png)

![](img/5b411cec3b5349ef62c69524f2488cff_36.png)

对应的代码逻辑如下：
```python
def calculate_time_numpy():
    import numpy as np
    import time
    t1 = time.time()
    x = np.arange(1000)
    y = np.arange(1000)
    z = x + y  # 向量化加法，无需循环
    t2 = time.time()
    return t2 - t1
```

![](img/5b411cec3b5349ef62c69524f2488cff_37.png)

---

### 执行与结果对比

![](img/5b411cec3b5349ef62c69524f2488cff_39.png)

![](img/5b411cec3b5349ef62c69524f2488cff_40.png)

定义好两个函数后，我们可以调用它们并打印执行时间。由于单次执行时间可能太短（尤其是NumPy版本），为了更清晰地观察差异，我们可以将测试放在一个循环中多次运行。

![](img/5b411cec3b5349ef62c69524f2488cff_41.png)

![](img/5b411cec3b5349ef62c69524f2488cff_42.png)

![](img/5b411cec3b5349ef62c69524f2488cff_43.png)

![](img/5b411cec3b5349ef62c69524f2488cff_44.png)

![](img/5b411cec3b5349ef62c69524f2488cff_45.png)

以下是执行对比的示例代码：
```python
# 单次调用对比
print("NumPy 时间:", calculate_time_numpy())
print("Python 列表时间:", calculate_time_python())

![](img/5b411cec3b5349ef62c69524f2488cff_47.png)

print("\n--- 循环测试10次 ---")
# 循环测试，取平均值更可靠
for i in range(10):
    t_numpy = calculate_time_numpy()
    t_python = calculate_time_python()
    print(f"第{i+1}次 - NumPy: {t_numpy:.6f}秒, Python列表: {t_python:.6f}秒")
```

![](img/5b411cec3b5349ef62c69524f2488cff_49.png)

运行上述代码，你会发现 `calculate_time_numpy` 函数的执行时间几乎总是远小于 `calculate_time_python` 函数。即使将循环次数增加到1000次，NumPy版本的速度优势依然非常明显。

![](img/5b411cec3b5349ef62c69524f2488cff_50.png)

![](img/5b411cec3b5349ef62c69524f2488cff_51.png)

![](img/5b411cec3b5349ef62c69524f2488cff_52.png)

---

### 总结

![](img/5b411cec3b5349ef62c69524f2488cff_54.png)

![](img/5b411cec3b5349ef62c69524f2488cff_55.png)

本节课中我们一起学习了如何通过实际代码对比Python列表与NumPy数组的性能。实验清晰地表明：

![](img/5b411cec3b5349ef62c69524f2488cff_56.png)

![](img/5b411cec3b5349ef62c69524f2488cff_57.png)

![](img/5b411cec3b5349ef62c69524f2488cff_58.png)

1.  **时间效率**：NumPy数组利用**向量化**计算和底层C语言实现，在执行数值运算时比使用Python循环操作列表要快几个数量级。
2.  **代码简洁性**：NumPy语法更简洁（如 `x + y`），避免了冗长的循环代码，提高了开发效率。
3.  **存储效率**：NumPy数组在内存中连续存储同类型数据，比Python列表（存储对象引用）更节省空间。

![](img/5b411cec3b5349ef62c69524f2488cff_60.png)

![](img/5b411cec3b5349ef62c69524f2488cff_62.png)

![](img/5b411cec3b5349ef62c69524f2488cff_63.png)

正是由于在**时间**和**空间**上的双重高效性，NumPy成为了科学计算和数据分析领域不可或缺的核心工具。