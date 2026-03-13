# 106：K最近邻算法实践（第一部分）📊

![](img/d87d0b9b639520ef6954b0ebfea015b0_0.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_2.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_3.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_4.png)

在本节课中，我们将学习如何应用K最近邻算法解决一个实际问题：预测客户流失。我们将使用一个包含客户电话账户数据的真实数据集，其中混合了数值型、分类型和有序型变量。课程的核心是学习如何对数据进行预处理，以便将其输入到K最近邻模型中。

![](img/d87d0b9b639520ef6954b0ebfea015b0_5.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_6.png)

## 概述

![](img/d87d0b9b639520ef6954b0ebfea015b0_8.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_9.png)

我们将从一个客户流失数据集开始。这个数据集包含了客户的长期数据使用量、月收入等特征。我们的目标是预测客户是否会流失。由于数据包含多种类型的变量，我们需要先进行数据加载和预处理，然后才能应用K最近邻算法进行预测。

![](img/d87d0b9b639520ef6954b0ebfea015b0_11.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_12.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_13.png)

完成本实验后，你应该能够理解如何预处理各种类型的变量以应用K最近邻算法，并学会如何选择K值以及评估模型性能。

![](img/d87d0b9b639520ef6954b0ebfea015b0_15.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_16.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_17.png)

## 第一步：导入与初步探索数据

![](img/d87d0b9b639520ef6954b0ebfea015b0_19.png)

首先，我们需要导入数据并对其进行初步检查。我们将导入名为 `churn_data` 的数据集，并移除一些不需要的列。

![](img/d87d0b9b639520ef6954b0ebfea015b0_21.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_22.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_23.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_24.png)

以下是导入数据并查看基本信息的代码：

![](img/d87d0b9b639520ef6954b0ebfea015b0_26.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_27.png)

```python
import pandas as pd

![](img/d87d0b9b639520ef6954b0ebfea015b0_28.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_29.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_30.png)

# 假设数据已加载到变量 churn_data 中
df = churn_data.drop(columns=['不需要的列1', '不需要的列2'])
print(df.head())
print(df.describe().round(2))
```

![](img/d87d0b9b639520ef6954b0ebfea015b0_32.png)

运行上述代码后，我们可以看到数据的基本统计信息。例如，“months”列表示客户成为会员的月数，其平均值约为32，范围从1到72，标准差较大。其他数值型特征如“gigabytes_per_month”和“monthly_payment”也有较大的标准差。“satisfaction”是1到5的评分，“churn”是目标变量，取值为0或1，其中约27%的客户流失了。

![](img/d87d0b9b639520ef6954b0ebfea015b0_33.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_34.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_35.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_36.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_37.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_39.png)

接下来，我们查看非数值型（对象类型）特征的描述：

![](img/d87d0b9b639520ef6954b0ebfea015b0_40.png)

```python
print(df.describe(include=['object']))
```

![](img/d87d0b9b639520ef6954b0ebfea015b0_42.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_43.png)

输出显示了每个分类特征的唯一值数量、最常见的值及其出现频率。我们可以看到许多特征是二元的（只有两个值），而“offer”特征有六个唯一值，其中最常见的是“none”。

![](img/d87d0b9b639520ef6954b0ebfea015b0_44.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_45.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_46.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_47.png)

## 第二步：识别变量类型

![](img/d87d0b9b639520ef6954b0ebfea015b0_49.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_50.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_51.png)

为了应用机器学习模型，我们需要识别数据集中每种变量的类型：二元变量、分类变量（无序）、有序分类变量和数值变量。非数值型特征都需要进行编码。

![](img/d87d0b9b639520ef6954b0ebfea015b0_53.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_55.png)

以下是识别变量类型的步骤：

首先，我们查看每个列的唯一值数量：

![](img/d87d0b9b639520ef6954b0ebfea015b0_57.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_58.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_59.png)

```python
unique_counts = df.nunique()
print(unique_counts)
```

![](img/d87d0b9b639520ef6954b0ebfea015b0_60.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_61.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_62.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_63.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_64.png)

然后，我们筛选出只有两个唯一值的列，这些就是二元变量：

![](img/d87d0b9b639520ef6954b0ebfea015b0_66.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_67.png)

```python
binary_vars = unique_counts[unique_counts == 2].index.tolist()
print("二元变量：", binary_vars)
```

![](img/d87d0b9b639520ef6954b0ebfea015b0_68.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_69.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_71.png)

接着，我们找出分类变量（这里定义为唯一值数量大于2且小于等于6的列）：

![](img/d87d0b9b639520ef6954b0ebfea015b0_73.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_74.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_76.png)

```python
categorical_vars = unique_counts[(unique_counts > 2) & (unique_counts <= 6)].index.tolist()
print("分类变量：", categorical_vars)
```

![](img/d87d0b9b639520ef6954b0ebfea015b0_78.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_79.png)

为了更清楚地查看这些分类变量的具体取值，我们可以运行一个循环：

![](img/d87d0b9b639520ef6954b0ebfea015b0_81.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_83.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_85.png)

```python
for var in categorical_vars:
    print(f"{var}: {df[var].unique()}")
```

![](img/d87d0b9b639520ef6954b0ebfea015b0_87.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_88.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_89.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_90.png)

现在，我们需要指定哪些变量是有序的。在本数据集中，“contract”和“satisfaction”是有序的。“contract”的值（如“month-to-month”, “one year”, “two years”）存在顺序关系，但差值不一定相等。“satisfaction”的1到5分也明显有序。此外，虽然“months”本质上是数值型，但我们也可以将其视为有序分类变量进行处理。

```python
ordinal_vars = ['contract', 'satisfaction', 'months']
```

![](img/d87d0b9b639520ef6954b0ebfea015b0_92.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_93.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_94.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_95.png)

最后，数值变量就是剩下的列：

![](img/d87d0b9b639520ef6954b0ebfea015b0_96.png)

```python
all_columns = set(df.columns)
numeric_vars = list(all_columns - set(ordinal_vars) - set(categorical_vars) - set(binary_vars))
print("数值变量：", numeric_vars)
```

![](img/d87d0b9b639520ef6954b0ebfea015b0_98.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_99.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_100.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_101.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_103.png)

在我们的数据中，数值变量是“monthly_payment”和“gigabytes_per_month”。

![](img/d87d0b9b639520ef6954b0ebfea015b0_105.png)

我们可以为这些数值变量绘制直方图以观察其分布：

![](img/d87d0b9b639520ef6954b0ebfea015b0_107.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_108.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_109.png)

```python
import matplotlib.pyplot as plt
df[numeric_vars].hist(bins=30, figsize=(10, 5))
plt.show()
```

![](img/d87d0b9b639520ef6954b0ebfea015b0_110.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_111.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_113.png)

从直方图可以看出，“gigabytes_per_month”严重右偏，而“monthly_payment”大致呈正态分布，略有左偏。

![](img/d87d0b9b639520ef6954b0ebfea015b0_115.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_116.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_117.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_118.png)

## 第三步：处理有序变量“months”

![](img/d87d0b9b639520ef6954b0ebfea015b0_120.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_121.png)

对于“months”这个变量，我们使用分箱将其从连续数值转换为有序分类。这里我们使用`pandas.cut`函数将其分为5个等宽的区间。

![](img/d87d0b9b639520ef6954b0ebfea015b0_123.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_125.png)

```python
df['months'] = pd.cut(df['months'], bins=5)
print(df['months'].value_counts().sort_index())
```

![](img/d87d0b9b639520ef6954b0ebfea015b0_127.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_128.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_129.png)

`pd.cut`函数默认会生成表示区间的标签。你也可以通过`labels`参数自定义标签名称，但需要确保标签数量与分箱数量一致。

![](img/d87d0b9b639520ef6954b0ebfea015b0_131.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_132.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_134.png)

```python
# 示例：自定义分箱标签
# custom_labels = ['Very Low', 'Low', 'Medium', 'High', 'Very High']
# df['months'] = pd.cut(df['months'], bins=5, labels=custom_labels)
```

![](img/d87d0b9b639520ef6954b0ebfea015b0_136.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_137.png)

## 总结

![](img/d87d0b9b639520ef6954b0ebfea015b0_138.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_139.png)

在本节课的第一部分，我们一起完成了以下工作：
1.  **导入并探索了客户流失数据集**，了解了数据的基本结构和统计信息。
2.  **系统性地识别了变量类型**，包括二元变量、无序分类变量、有序分类变量和数值变量。
3.  **对有序变量“months”进行了分箱处理**，将其转换为有序分类特征，为后续的编码做准备。

![](img/d87d0b9b639520ef6954b0ebfea015b0_141.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_142.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_143.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_144.png)

![](img/d87d0b9b639520ef6954b0ebfea015b0_145.png)

在下一节中，我们将学习如何对所有非数值型特征进行编码（例如，将二元变量转换为0/1，对分类变量进行独热编码），并对所有特征进行标准化缩放，以便最终将其输入到K最近邻模型中进行训练和预测。