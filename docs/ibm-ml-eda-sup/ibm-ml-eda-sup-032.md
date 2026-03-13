# 032：特征工程实验解决方案（选修部分）第3部分 🧠

在本节课中，我们将学习两种高级特征工程技术：**类别内偏差特征**和**多项式特征**。我们将通过代码示例，一步步理解如何创建这些特征，并了解它们在机器学习工作流中的应用。

![](img/51916725d59d753def6378c75a4f0fc6_1.png)

## 概述

![](img/51916725d59d753def6378c75a4f0fc6_3.png)

![](img/51916725d59d753def6378c75a4f0fc6_4.png)

上一节我们介绍了基础的特征工程概念。本节中，我们将探讨更复杂的特征类型，这些特征能捕捉数据点在其所属类别内的相对位置信息，并学习如何使用Scikit-learn库自动生成多项式特征。

![](img/51916725d59d753def6378c75a4f0fc6_6.png)

![](img/51916725d59d753def6378c75a4f0fc6_7.png)

---

![](img/51916725d59d753def6378c75a4f0fc6_9.png)

## 类别内偏差特征

![](img/51916725d59d753def6378c75a4f0fc6_10.png)

![](img/51916725d59d753def6378c75a4f0fc6_11.png)

![](img/51916725d59d753def6378c75a4f0fc6_13.png)

本节我们将创建一个能捕捉特征值相对于其所属类别均值的偏差的特征。这种特征有助于理解数据点在其分组内的相对位置，例如，一栋房屋相对于其所在社区内其他房屋的“优秀”程度。

![](img/51916725d59d753def6378c75a4f0fc6_15.png)

以下是创建此类特征的核心步骤：

![](img/51916725d59d753def6378c75a4f0fc6_17.png)

![](img/51916725d59d753def6378c75a4f0fc6_18.png)

1.  **按类别分组**：使用Pandas的`groupby`函数，根据一个分类变量（如“社区”）对数据进行分组。
2.  **计算组内统计量**：对每个分组内的数值特征（如“整体质量”）计算其均值和标准差。
3.  **计算标准化偏差**：对于每一行数据，用其数值特征值减去所属组的均值，再除以所属组的标准差。公式如下：
    `偏差 = (特征值 - 组内均值) / 组内标准差`
4.  **生成新特征列**：将计算出的偏差作为一个新的特征列添加到原始数据框中。

![](img/51916725d59d753def6378c75a4f0fc6_20.png)

![](img/51916725d59d753def6378c75a4f0fc6_22.png)

![](img/51916725d59d753def6378c75a4f0fc6_24.png)

### 代码逐步解析

我们通过分解一个自定义函数来逐步理解这个过程。

![](img/51916725d59d753def6378c75a4f0fc6_26.png)

首先，我们有一个数据框`X4`，其中包含“社区”这个分类变量。

![](img/51916725d59d753def6378c75a4f0fc6_28.png)

```python
# 假设 X4 是我们的数据框，包含‘Neighborhood’和‘OverallQual’等列
import pandas as pd

# 按‘Neighborhood’分组，并计算‘OverallQual’的组内均值
group_means = X4.groupby('Neighborhood')['OverallQual'].transform('mean')

![](img/51916725d59d753def6378c75a4f0fc6_30.png)

![](img/51916725d59d753def6378c75a4f0fc6_31.png)

![](img/51916725d59d753def6378c75a4f0fc6_32.png)

![](img/51916725d59d753def6378c75a4f0fc6_34.png)

# 计算‘OverallQual’的组内标准差
group_stds = X4.groupby('Neighborhood')['OverallQual'].transform('std')

![](img/51916725d59d753def6378c75a4f0fc6_35.png)

![](img/51916725d59d753def6378c75a4f0fc6_37.png)

![](img/51916725d59d753def6378c75a4f0fc6_39.png)

# 计算每个‘OverallQual’值相对于其社区均值的偏差
deviation_feature = (X4['OverallQual'] - group_means) / group_stds

![](img/51916725d59d753def6378c75a4f0fc6_41.png)

# 将偏差作为一个新列添加到数据框中
X4['OverallQual_Neighborhood_Deviation'] = deviation_feature
```

![](img/51916725d59d753def6378c75a4f0fc6_42.png)

![](img/51916725d59d753def6378c75a4f0fc6_43.png)

`transform`方法的关键在于，它返回一个与原始数据框长度相同的序列，其中每个值是其所属组的统计量（如均值），从而便于进行逐元素计算。

![](img/51916725d59d753def6378c75a4f0fc6_45.png)

运行完整函数后，我们得到的新特征列可以这样解读：负值表示该房屋的“整体质量”低于其社区的平均水平，正值则表示高于平均水平。

![](img/51916725d59d753def6378c75a4f0fc6_47.png)

![](img/51916725d59d753def6378c75a4f0fc6_48.png)

---

![](img/51916725d59d753def6378c75a4f0fc6_50.png)

![](img/51916725d59d753def6378c75a4f0fc6_52.png)

## 多项式特征

![](img/51916725d59d753def6378c75a4f0fc6_54.png)

![](img/51916725d59d753def6378c75a4f0fc6_55.png)

![](img/51916725d59d753def6378c75a4f0fc6_57.png)

上一节我们手动创建了多项式特征。本节中我们来看看如何使用Scikit-learn的`PolynomialFeatures`工具自动生成多项式特征和交互项。

![](img/51916725d59d753def6378c75a4f0fc6_59.png)

![](img/51916725d59d753def6378c75a4f0fc6_61.png)

多项式特征能够捕捉特征之间的非线性关系和交互作用。例如，房屋的“地块面积”和“整体质量”可能共同对价格产生复合影响。

![](img/51916725d59d753def6378c75a4f0fc6_63.png)

![](img/51916725d59d753def6378c75a4f0fc6_65.png)

以下是使用`PolynomialFeatures`的步骤：

1.  **导入并实例化**：从`sklearn.preprocessing`导入`PolynomialFeatures`并创建一个对象，指定所需的多项式次数。
2.  **选择特征并拟合**：选择要生成多项式特征的基础特征列，并对这些数据调用`fit`方法。
3.  **转换数据**：使用`transform`方法将原始特征转换为包含多项式项的新特征集。
4.  **理解输出**：新特征集包括原始特征、各特征的幂次项以及特征间的交互项。

![](img/51916725d59d753def6378c75a4f0fc6_67.png)

![](img/51916725d59d753def6378c75a4f0fc6_68.png)

### 代码示例与解析

![](img/51916725d59d753def6378c75a4f0fc6_70.png)

![](img/51916725d59d753def6378c75a4f0fc6_71.png)

![](img/51916725d59d753def6378c75a4f0fc6_72.png)

```python
from sklearn.preprocessing import PolynomialFeatures

# 1. 实例化多项式特征对象，指定次数为2
poly = PolynomialFeatures(degree=2)

![](img/51916725d59d753def6378c75a4f0fc6_74.png)

![](img/51916725d59d753def6378c75a4f0fc6_75.png)

# 2. 指定要使用的特征（例如‘LotArea’和‘OverallQual’），并拟合
features_to_poly = X5[['LotArea', 'OverallQual']]
poly.fit(features_to_poly)

![](img/51916725d59d753def6378c75a4f0fc6_76.png)

![](img/51916725d59d753def6378c75a4f0fc6_78.png)

# 3. 转换数据，生成多项式特征
X_poly = poly.transform(features_to_poly)

![](img/51916725d59d753def6378c75a4f0fc6_80.png)

![](img/51916725d59d753def6378c75a4f0fc6_81.png)

![](img/51916725d59d753def6378c75a4f0fc6_83.png)

# 4. 获取生成的特征名称以便理解
feature_names = poly.get_feature_names_out(input_features=['LotArea', 'OverallQual'])
print(feature_names)
```

运行上述代码，`feature_names`的输出可能类似于：
`[‘1’, ‘LotArea’, ‘OverallQual’, ‘LotArea^2’, ‘LotArea OverallQual’, ‘OverallQual^2’]`

![](img/51916725d59d753def6378c75a4f0fc6_85.png)

这表示我们从两个原始特征，生成了包含常数项1、原始特征、平方项和交互项在内的6个新特征。这极大地扩展了模型的表达能力。

![](img/51916725d59d753def6378c75a4f0fc6_86.png)

![](img/51916725d59d753def6378c75a4f0fc6_88.png)

---

![](img/51916725d59d753def6378c75a4f0fc6_90.png)

![](img/51916725d59d753def6378c75a4f0fc6_91.png)

![](img/51916725d59d753def6378c75a4f0fc6_93.png)

## 总结

![](img/51916725d59d753def6378c75a4f0fc6_95.png)

本节课我们一起学习了两种高级特征工程技术。

![](img/51916725d59d753def6378c75a4f0fc6_97.png)

![](img/51916725d59d753def6378c75a4f0fc6_99.png)

首先，我们掌握了如何创建**类别内偏差特征**，它通过计算一个数值特征相对于其分类组别均值的标准化偏差，来揭示数据点在组内的相对位置信息。

![](img/51916725d59d753def6378c75a4f0fc6_100.png)

![](img/51916725d59d753def6378c75a4f0fc6_101.png)

![](img/51916725d59d753def6378c75a4f0fc6_103.png)

接着，我们学习了使用Scikit-learn的`PolynomialFeatures`来高效生成**多项式特征**。这种方法能自动创建特征的幂次项和交互项，为线性模型引入非线性能力，是机器学习工作流中特征预处理的重要一环。

![](img/51916725d59d753def6378c75a4f0fc6_105.png)

![](img/51916725d59d753def6378c75a4f0fc6_106.png)

![](img/51916725d59d753def6378c75a4f0fc6_107.png)

通过结合这些技术，我们可以从现有数据中构建出更具信息量和预测能力的新特征，为后续的模型训练打下坚实基础。特征工程是提升模型性能的关键步骤，需要根据具体问题和数据特点进行创造性探索。