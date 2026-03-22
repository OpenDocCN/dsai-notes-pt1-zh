# 035：NumPy数组占用的内存与性能对比 🧠⚡

![](img/c37408bbc5730d68ebadffe663143d5e_1.png)

![](img/c37408bbc5730d68ebadffe663143d5e_3.png)

![](img/c37408bbc5730d68ebadffe663143d5e_5.png)

在本节课中，我们将要学习NumPy数组与Python列表在内存占用和计算性能上的核心差异。我们将通过具体的代码示例来量化这些差异，帮助你理解为什么NumPy在数据科学领域如此重要。

## 概述

![](img/c37408bbc5730d68ebadffe663143d5e_7.png)

NumPy是Python中进行科学计算的基础库，其核心是`ndarray`对象。与Python内置的列表相比，NumPy数组在内存使用效率和计算速度上具有显著优势。本节我们将通过对比实验来直观展示这些优势。

![](img/c37408bbc5730d68ebadffe663143d5e_8.png)

![](img/c37408bbc5730d68ebadffe663143d5e_9.png)

![](img/c37408bbc5730d68ebadffe663143d5e_10.png)

![](img/c37408bbc5730d68ebadffe663143d5e_11.png)

![](img/c37408bbc5730d68ebadffe663143d5e_12.png)

![](img/c37408bbc5730d68ebadffe663143d5e_13.png)

## NumPy数组的内存占用

![](img/c37408bbc5730d68ebadffe663143d5e_15.png)

![](img/c37408bbc5730d68ebadffe663143d5e_17.png)

上一节我们介绍了NumPy数组的基本概念，本节中我们来看看其内存占用的具体表现。

![](img/c37408bbc5730d68ebadffe663143d5e_19.png)

![](img/c37408bbc5730d68ebadffe663143d5e_21.png)

NumPy数组不包含对Python对象的引用，而是直接存储数值本身。这意味着数组占用的内存就是其数据本身的大小，没有额外的开销。

![](img/c37408bbc5730d68ebadffe663143d5e_22.png)

以下是计算Python列表和NumPy数组内存占用的方法：

![](img/c37408bbc5730d68ebadffe663143d5e_24.png)

![](img/c37408bbc5730d68ebadffe663143d5e_25.png)

![](img/c37408bbc5730d68ebadffe663143d5e_26.png)

```python
import numpy as np
import sys

![](img/c37408bbc5730d68ebadffe663143d5e_28.png)

![](img/c37408bbc5730d68ebadffe663143d5e_29.png)

# 创建一个Python列表
py_list = [24, 1257, 57, 100, 200, 300]
print(f"Python列表: {py_list}")

![](img/c37408bbc5730d68ebadffe663143d5e_31.png)

![](img/c37408bbc5730d68ebadffe663143d5e_33.png)

# 计算Python列表的内存占用
size_of_list = sys.getsizeof(py_list)
print(f"Python列表的内存占用: {size_of_list} 字节")

![](img/c37408bbc5730d68ebadffe663143d5e_35.png)

![](img/c37408bbc5730d68ebadffe663143d5e_37.png)

![](img/c37408bbc5730d68ebadffe663143d5e_38.png)

# 将列表转换为NumPy数组
np_array = np.array(py_list)
print(f"NumPy数组: {np_array}")

![](img/c37408bbc5730d68ebadffe663143d5e_40.png)

![](img/c37408bbc5730d68ebadffe663143d5e_42.png)

# 计算NumPy数组的内存占用
size_of_np_array = np_array.nbytes
print(f"NumPy数组的内存占用: {size_of_np_array} 字节")
```

运行上述代码，你会发现即使对于少量元素，NumPy数组的内存占用也远小于Python列表。随着元素数量的增加，这种差异会变得更加显著。

![](img/c37408bbc5730d68ebadffe663143d5e_44.png)

![](img/c37408bbc5730d68ebadffe663143d5e_45.png)

![](img/c37408bbc5730d68ebadffe663143d5e_46.png)

![](img/c37408bbc5730d68ebadffe663143d5e_47.png)

![](img/c37408bbc5730d68ebadffe663143d5e_48.png)

## 性能时间对比

除了内存优势，NumPy在计算速度上也远超Python列表。这是因为NumPy的底层由C语言实现，并且支持向量化操作，避免了Python循环的开销。

![](img/c37408bbc5730d68ebadffe663143d5e_50.png)

![](img/c37408bbc5730d68ebadffe663143d5e_51.png)

![](img/c37408bbc5730d68ebadffe663143d5e_52.png)

![](img/c37408bbc5730d68ebadffe663143d5e_53.png)

以下是进行时间对比的步骤：

![](img/c37408bbc5730d68ebadffe663143d5e_54.png)

![](img/c37408bbc5730d68ebadffe663143d5e_55.png)

![](img/c37408bbc5730d68ebadffe663143d5e_57.png)

1.  首先，我们需要导入必要的模块。
2.  然后，创建两个函数，分别使用Python循环和NumPy向量化操作执行相同的计算任务。
3.  最后，使用`time`模块测量并比较两个函数的执行时间。

![](img/c37408bbc5730d68ebadffe663143d5e_59.png)

以下是具体的代码实现：

![](img/c37408bbc5730d68ebadffe663143d5e_60.png)

![](img/c37408bbc5730d68ebadffe663143d5e_61.png)

![](img/c37408bbc5730d68ebadffe663143d5e_63.png)

```python
import numpy as np
import time

![](img/c37408bbc5730d68ebadffe663143d5e_65.png)

![](img/c37408bbc5730d68ebadffe663143d5e_66.png)

def calculate_time_python_list(size=1000000):
    """使用Python列表和循环进行计算"""
    x = list(range(size))
    y = list(range(size))
    z = []
    start_time = time.time()
    for i in range(len(x)):
        z.append(x[i] + y[i])
    end_time = time.time()
    return end_time - start_time

![](img/c37408bbc5730d68ebadffe663143d5e_68.png)

![](img/c37408bbc5730d68ebadffe663143d5e_69.png)

def calculate_time_numpy_array(size=1000000):
    """使用NumPy数组进行向量化计算"""
    x = np.arange(size)
    y = np.arange(size)
    start_time = time.time()
    z = x + y  # 向量化操作，无需循环
    end_time = time.time()
    return end_time - start_time

![](img/c37408bbc5730d68ebadffe663143d5e_71.png)

![](img/c37408bbc5730d68ebadffe663143d5e_73.png)

# 执行时间对比
py_time = calculate_time_python_list(1000000)
np_time = calculate_time_numpy_array(1000000)

![](img/c37408bbc5730d68ebadffe663143d5e_75.png)

![](img/c37408bbc5730d68ebadffe663143d5e_77.png)

print(f"Python列表计算耗时: {py_time:.4f} 秒")
print(f"NumPy数组计算耗时: {np_time:.4f} 秒")
print(f"NumPy比Python列表快 {py_time / np_time:.1f} 倍")
```

![](img/c37408bbc5730d68ebadffe663143d5e_79.png)

![](img/c37408bbc5730d68ebadffe663143d5e_81.png)

运行这段代码，你将看到NumPy的向量化操作比等价的Python循环快几个数量级。这种性能提升在处理大规模数据集时至关重要。

![](img/c37408bbc5730d68ebadffe663143d5e_83.png)

![](img/c37408bbc5730d68ebadffe663143d5e_85.png)

## 总结

![](img/c37408bbc5730d68ebadffe663143d5e_86.png)

本节课中我们一起学习了NumPy数组与Python列表的核心差异：

![](img/c37408bbc5730d68ebadffe663143d5e_88.png)

![](img/c37408bbc5730d68ebadffe663143d5e_89.png)

![](img/c37408bbc5730d68ebadffe663143d5e_90.png)

1.  **内存效率**：NumPy数组直接在连续的内存块中存储数据，内存占用远低于存储对象引用的Python列表。其大小可通过`numpy.ndarray.nbytes`属性获得。
2.  **计算性能**：NumPy利用预编译的C代码和向量化操作，在执行数学运算时比使用Python循环的列表快得多。

![](img/c37408bbc5730d68ebadffe663143d5e_92.png)

![](img/c37408bbc5730d68ebadffe663143d5e_94.png)

![](img/c37408bbc5730d68ebadffe663143d5e_96.png)

因此，在进行数值计算和数据分析时，应优先使用NumPy数组，以获得更高的内存利用率和更快的执行速度。