数据分析高级Python：构建与优化：p34：列表内存占用分析 🧠

![](img/79b39d43f9db93d45f816ae94aaf856c_1.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_2.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_3.png)

在本节课中，我们将学习如何分析Python列表在内存中的占用情况。我们将通过计算列表本身及其元素的内存大小，来理解Python列表的内存结构。

![](img/79b39d43f9db93d45f816ae94aaf856c_5.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_6.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_7.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_9.png)

---

![](img/79b39d43f9db93d45f816ae94aaf856c_11.png)

上一节我们介绍了内存分析的基本概念，本节中我们来看看如何具体计算一个列表的内存占用。

首先，我们创建一个包含三个整数的列表，并将其存储在一个变量中。

![](img/79b39d43f9db93d45f816ae94aaf856c_13.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_14.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_15.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_16.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_17.png)

```python
lst = [10, 20, 30]
```

![](img/79b39d43f9db93d45f816ae94aaf856c_19.png)

接下来，我们使用`sys`模块中的`getsizeof`函数来获取这个列表对象本身的内存大小。

![](img/79b39d43f9db93d45f816ae94aaf856c_21.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_22.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_23.png)

```python
import sys
size_of_list = sys.getsizeof(lst)
print("Size of list:", size_of_list)
```

执行上述代码，输出结果为88。这意味着存储这个列表结构本身需要88字节。

![](img/79b39d43f9db93d45f816ae94aaf856c_25.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_26.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_27.png)

现在产生一个问题：这个88字节是列表的完整大小吗？它是否包含了列表中三个元素的实际数据？为了理解这一点，我们先创建一个空列表并查看其大小。

```python
empty_lst = []
size_of_empty_list = sys.getsizeof(empty_lst)
print("Size of empty list:", size_of_empty_list)
```

![](img/79b39d43f9db93d45f816ae94aaf856c_29.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_30.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_31.png)

执行代码，输出结果为64。这表明一个空列表本身就有64字节的固定开销。

![](img/79b39d43f9db93d45f816ae94aaf856c_33.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_35.png)

通过计算差值，我们可以得出每个列表元素引用所占用的内存。
*   列表总大小：88字节
*   空列表大小：64字节
*   三个元素引用总大小：24字节 (88 - 64)
*   每个元素引用大小：8字节 (24 / 3)

![](img/79b39d43f9db93d45f816ae94aaf856c_36.png)

然而，在Python中，列表存储的并不是元素值本身，而是指向这些值（对象）的**引用**。我们可以验证这一点。

![](img/79b39d43f9db93d45f816ae94aaf856c_38.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_39.png)

```python
print(type(lst[0]))
```

![](img/79b39d43f9db93d45f816ae94aaf856c_40.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_41.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_43.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_44.png)

输出结果为`<class 'int'>`，证实`lst[0]`是一个指向整数对象的引用。

因此，之前计算的8字节是存储这个“引用”或“指针”的成本，而不是整数对象本身的大小。要获取整数对象实际占用的内存，我们需要测量该元素本身。

![](img/79b39d43f9db93d45f816ae94aaf856c_46.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_47.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_48.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_49.png)

```python
size_of_each_element = sys.getsizeof(lst[0])
print("Size of each element (object):", size_of_each_element)
```

![](img/79b39d43f9db93d45f816ae94aaf856c_51.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_53.png)

执行代码，输出结果为28。这意味着每个整数对象本身占用28字节。

![](img/79b39d43f9db93d45f816ae94aaf856c_55.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_56.png)

现在，我们可以计算这个列表在内存中的**总消耗**。它由两部分组成：
1.  列表结构本身的开销（包含引用数组）。
2.  所有元素对象本身的大小。

![](img/79b39d43f9db93d45f816ae94aaf856c_58.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_59.png)

以下是计算总内存占用的公式：

![](img/79b39d43f9db93d45f816ae94aaf856c_61.png)

```python
total_size = size_of_list + (len(lst) * size_of_each_element)
print("Total memory consumed by list:", total_size)
```

![](img/79b39d43f9db93d45f816ae94aaf856c_63.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_64.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_65.png)

对于我们的例子，总大小为 `88 + (3 * 28) = 172` 字节。

![](img/79b39d43f9db93d45f816ae94aaf856c_67.png)

---

![](img/79b39d43f9db93d45f816ae94aaf856c_69.png)

本节课中我们一起学习了Python列表的内存占用分析。我们了解到：
1.  列表对象有固定的内存开销（例如64字节）。
2.  列表中的每个元素占用额外的8字节来存储指向实际对象的引用。
3.  元素对象（如整数）本身有独立的内存占用。
4.  列表的总内存消耗是**列表结构大小**与**所有元素对象大小之和**的总和。

![](img/79b39d43f9db93d45f816ae94aaf856c_70.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_71.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_72.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_73.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_74.png)

![](img/79b39d43f9db93d45f816ae94aaf856c_75.png)

理解这一点对于优化使用大量列表的数据分析程序至关重要。在下一节，我们将把这种分析方法应用到NumPy数组上，并进行对比。