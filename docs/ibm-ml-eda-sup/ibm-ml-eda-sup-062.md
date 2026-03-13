# 062：训练集与测试集分割实验（第一部分）📊

![](img/956a2e074cf4f6a0c302ede4f0be3678_1.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_3.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_4.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_6.png)

在本节课中，我们将学习如何使用训练集与测试集分割来评估线性回归模型。我们将使用爱荷华州埃姆斯市的房价数据集进行实践。

---

![](img/956a2e074cf4f6a0c302ede4f0be3678_8.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_9.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_10.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_11.png)

## 概述

![](img/956a2e074cf4f6a0c302ede4f0be3678_13.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_15.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_16.png)

我们将从导入和探索数据集开始，了解其基本结构。接着，我们会分析数据集中不同数据类型的分布。最后，我们将重点探讨对分类变量进行独热编码时可能产生的特征数量变化，为后续的实际编码操作做准备。

![](img/956a2e074cf4f6a0c302ede4f0be3678_18.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_20.png)

---

![](img/956a2e074cf4f6a0c302ede4f0be3678_22.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_24.png)

## 导入与数据探索

首先，我们需要导入必要的库并加载数据集。

![](img/956a2e074cf4f6a0c302ede4f0be3678_26.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_27.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_28.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_29.png)

以下是导入库和设置文件路径的代码：

![](img/956a2e074cf4f6a0c302ede4f0be3678_31.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_33.png)

```python
import os
import pandas as pd
import numpy as np

![](img/956a2e074cf4f6a0c302ede4f0be3678_35.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_37.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_39.png)

# 设置数据路径
data_path = ['data']
file_path = os.path.join(*data_path, 'Ames_Housing_Sales.csv')
```

![](img/956a2e074cf4f6a0c302ede4f0be3678_41.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_43.png)

我们使用 `os.path.join` 来构建跨操作系统的文件路径。然后，使用 pandas 读取 CSV 文件。

![](img/956a2e074cf4f6a0c302ede4f0be3678_45.png)

```python
data = pd.read_csv(file_path)
```

![](img/956a2e074cf4f6a0c302ede4f0be3678_47.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_48.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_50.png)

加载数据后，我们首先查看数据集的形状。

![](img/956a2e074cf4f6a0c302ede4f0be3678_52.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_54.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_55.png)

```python
print(data.shape)
```

![](img/956a2e074cf4f6a0c302ede4f0be3678_57.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_58.png)

输出应为 `(1379, 80)`，表示有1379行数据和80列。其中79列是特征，1列是预测目标（房价）。

![](img/956a2e074cf4f6a0c302ede4f0be3678_60.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_61.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_62.png)

---

## 分析数据类型

![](img/956a2e074cf4f6a0c302ede4f0be3678_64.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_65.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_66.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_67.png)

上一节我们查看了数据整体结构，本节中我们来看看数据的具体类型。

![](img/956a2e074cf4f6a0c302ede4f0be3678_69.png)

数据集中包含三种数据类型：整数（int）、浮点数（float）和对象（object，通常是字符串）。我们需要统计每种类型的数量。

![](img/956a2e074cf4f6a0c302ede4f0be3678_71.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_73.png)

以下是检查数据类型分布的代码：

![](img/956a2e074cf4f6a0c302ede4f0be3678_74.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_75.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_77.png)

```python
# 获取每列的数据类型并计数
type_counts = data.dtypes.value_counts()
print(type_counts)
```

![](img/956a2e074cf4f6a0c302ede4f0be3678_78.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_80.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_82.png)

这段代码会输出类似 `object: X, float64: Y, int64: Z` 的结果，显示了数据集中每种类型的列数。

![](img/956a2e074cf4f6a0c302ede4f0be3678_84.png)

---

![](img/956a2e074cf4f6a0c302ede4f0be3678_86.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_88.png)

## 评估独热编码的影响

![](img/956a2e074cf4f6a0c302ede4f0be3678_90.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_91.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_92.png)

在数据预处理中，一个关键步骤是对分类变量进行编码。无序分类变量通常需要进行独热编码。

![](img/956a2e074cf4f6a0c302ede4f0be3678_94.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_96.png)

然而，独热编码会显著增加特征的数量，并可能引入高度相关的特征。我们的目标是：如果对所有对象类型的特征进行独热编码，总特征数会增加多少。

![](img/956a2e074cf4f6a0c302ede4f0be3678_97.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_99.png)

首先，我们需要识别出所有对象类型的列。

![](img/956a2e074cf4f6a0c302ede4f0be3678_100.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_102.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_104.png)

```python
# 创建一个布尔掩码，标识对象类型的列
object_mask = data.dtypes == np.object
# 获取对象类型列的名称
object_columns = data.columns[object_mask]
```

接下来，对于每个对象类型的列，我们计算其唯一值的数量。独热编码后，每个唯一值会变成一个新列（但我们会丢弃原始列），因此新增列数为 `n_unique - 1`。

![](img/956a2e074cf4f6a0c302ede4f0be3678_106.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_107.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_108.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_109.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_110.png)

以下是计算潜在新增特征数量的步骤：

![](img/956a2e074cf4f6a0c302ede4f0be3678_112.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_114.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_115.png)

```python
# 计算每个对象列的唯一值数量
n_unique_ohc = data[object_columns].apply(lambda x: x.nunique())

![](img/956a2e074cf4f6a0c302ede4f0be3678_117.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_118.png)

# 排序以便查看
n_unique_ohc.sort_values(ascending=False, inplace=True)

![](img/956a2e074cf4f6a0c302ede4f0be3678_120.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_121.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_122.png)

# 减去1，因为独热编码后会丢弃原始列
n_unique_ohc -= 1

# 计算总共将新增多少列
total_ohc_columns = n_unique_ohc.sum()
print(f"独热编码将新增 {total_ohc_columns} 个特征。")
```

![](img/956a2e074cf4f6a0c302ede4f0be3678_124.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_125.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_126.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_128.png)

假设原始有80列（含目标变量），执行上述编码后，总特征数将变为 `80 + total_ohc_columns`。

![](img/956a2e074cf4f6a0c302ede4f0be3678_130.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_131.png)

---

![](img/956a2e074cf4f6a0c302ede4f0be3678_133.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_134.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_136.png)

## 总结

![](img/956a2e074cf4f6a0c302ede4f0be3678_138.png)

本节课中我们一起学习了：
1.  如何导入数据集并检查其形状。
2.  如何分析数据集中不同数据类型的分布。
3.  如何评估对分类变量进行独热编码可能带来的特征数量增长。

![](img/956a2e074cf4f6a0c302ede4f0be3678_140.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_141.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_142.png)

![](img/956a2e074cf4f6a0c302ede4f0be3678_143.png)

在下一部分，我们将开始使用 Scikit-learn 的 `OneHotEncoder` 实际执行独热编码，并继续进行训练集与测试集的分割实验。