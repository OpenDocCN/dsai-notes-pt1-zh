# 031：特征工程实验解决方案（选修部分）第2部分 🛠️

![](img/4179310741c5b7580ad0055aefc8a9ca_1.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_2.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_3.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_4.png)

在本节课中，我们将继续学习特征工程的核心步骤。我们将重点探讨如何处理缺失值、分析变量关系、创建多项式特征和交互项，以及如何将分类变量转换为数值特征。这些步骤是构建有效机器学习模型前至关重要的数据预处理环节。

![](img/4179310741c5b7580ad0055aefc8a9ca_6.png)

## 识别与处理缺失值 🔍

![](img/4179310741c5b7580ad0055aefc8a9ca_8.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_9.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_10.png)

上一节我们介绍了特征工程的基本概念，本节中我们来看看如何具体处理数据中的缺失值。

![](img/4179310741c5b7580ad0055aefc8a9ca_12.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_13.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_14.png)

首先，我们使用 `data.isnull()` 函数来检查原始数据框中的缺失值。这个函数会为每个单元格返回 `True` 或 `False`，指示其是否为缺失值。

![](img/4179310741c5b7580ad0055aefc8a9ca_16.png)

```python
data.isnull()
```

![](img/4179310741c5b7580ad0055aefc8a9ca_17.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_19.png)

接着，我们使用 `.sum()` 方法对每一列的 `True` 值进行求和。由于 `True` 被计为1，`False` 被计为0，求和结果就是每列的缺失值数量。

![](img/4179310741c5b7580ad0055aefc8a9ca_21.png)

```python
data.isnull().sum()
```

![](img/4179310741c5b7580ad0055aefc8a9ca_23.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_24.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_26.png)

为了更清晰地查看哪些特征缺失值最多，我们可以对结果进行排序。列表底部的特征就是缺失值最多的。

![](img/4179310741c5b7580ad0055aefc8a9ca_28.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_29.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_30.png)

```python
data.isnull().sum().sort_values()
```

![](img/4179310741c5b7580ad0055aefc8a9ca_31.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_33.png)

考虑到特征数量较多，为了更清晰地演示后续步骤，我们将缩小数据集，只关注部分关键特征。

![](img/4179310741c5b7580ad0055aefc8a9ca_35.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_37.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_39.png)

以下是选取的特征列：
*   `LotArea`
*   `OverallQual`
*   `YearBuilt`
*   `GrLivArea`
*   `GarageCars`
*   `HouseStyle`
*   `Neighborhood`
*   `SalePrice`（目标变量）

![](img/4179310741c5b7580ad0055aefc8a9ca_41.png)

```python
df_small = df.loc[:, ['LotArea', 'OverallQual', 'YearBuilt', 'GrLivArea', 'GarageCars', 'HouseStyle', 'Neighborhood', 'SalePrice']]
```

我们可以使用 `.describe()` 和 `.info()` 方法来快速查看这个子数据集的统计信息和数据类型。

![](img/4179310741c5b7580ad0055aefc8a9ca_43.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_44.png)

```python
df_small.describe().T  # .T 表示转置，便于阅读
df_small.info()
```

![](img/4179310741c5b7580ad0055aefc8a9ca_46.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_47.png)

现在，我们来处理这个子数据集中的缺失值。目前只有一个缺失值，位于 `GarageCars` 列。

![](img/4179310741c5b7580ad0055aefc8a9ca_49.png)

处理缺失值有多种方法，例如用中位数、平均数或0填充。选择哪种方法需要根据数据的实际含义决定。对于 `GarageCars`（车库容量），用0填充可能表示“没有车库”，这在逻辑上是合理的。虽然用中位数（1）或平均数（约0.6）也是选项，但0.6不是整数，可能不太合适。这里我们选择用0填充。

```python
df_small_filled = df_small.fillna(0)
df_small_filled.isnull().sum()  # 再次检查，确认已无缺失值
```

## 探索特征与目标的关系 📊

![](img/4179310741c5b7580ad0055aefc8a9ca_51.png)

处理完缺失值后，下一步是探索特征之间的关系，以及它们与目标变量（`SalePrice`）的关系。

![](img/4179310741c5b7580ad0055aefc8a9ca_53.png)

我们将使用 Seaborn 库的 `pairplot` 函数来绘制特征对的散点图矩阵。

![](img/4179310741c5b7580ad0055aefc8a9ca_54.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_55.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_56.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_57.png)

```python
import seaborn as sns
sns.pairplot(df_small_filled)
```

![](img/4179310741c5b7580ad0055aefc8a9ca_59.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_60.png)

通过观察这些图表，我们需要思考三个问题：
1.  目标变量 `SalePrice` 的分布情况如何？
2.  各个特征与 `SalePrice` 的关系如何？
3.  不同特征之间是否存在强相关性（多重共线性）？

![](img/4179310741c5b7580ad0055aefc8a9ca_62.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_63.png)

以下是观察结果：
*   目标变量分布：从右下角的直方图可以看出，`SalePrice` 呈右偏分布，这意味着我们可能需要对它进行对数转换。
*   特征与目标关系：例如，第二列最底行的图（`OverallQual` vs `SalePrice`）显示两者存在明显的曲线关系，而非严格的线性关系。这表明我们可能需要为 `OverallQual` 添加多项式特征。
*   特征间关系：目前没有发现特征之间存在非常强的线性关系，多重共线性问题不严重。

![](img/4179310741c5b7580ad0055aefc8a9ca_64.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_65.png)

## 创建多项式特征与交互项 ➕✖️

![](img/4179310741c5b7580ad0055aefc8a9ca_67.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_68.png)

基于之前的观察，我们发现 `OverallQual` 和 `GrLivArea` 与 `SalePrice` 可能存在非线性关系。因此，我们可以为它们创建平方项（二阶多项式特征）。

![](img/4179310741c5b7580ad0055aefc8a9ca_70.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_71.png)

首先，将特征 `X` 和目标 `y` 分离。

![](img/4179310741c5b7580ad0055aefc8a9ca_72.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_74.png)

```python
X = df_small_filled.drop('SalePrice', axis=1)
y = df_small_filled['SalePrice']
```

![](img/4179310741c5b7580ad0055aefc8a9ca_76.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_77.png)

然后，创建原始数据的副本，并添加新的多项式特征列。

![](img/4179310741c5b7580ad0055aefc8a9ca_79.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_81.png)

```python
X_poly = X.copy()
X_poly['OQ2'] = X_poly['OverallQual'] ** 2  # OverallQual 的平方
X_poly['GLA2'] = X_poly['GrLivArea'] ** 2   # GrLivArea 的平方
```

![](img/4179310741c5b7580ad0055aefc8a9ca_83.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_85.png)

接下来，考虑创建交互项。交互项捕捉的是一个特征对目标的影响如何依赖于另一个特征的值。

![](img/4179310741c5b7580ad0055aefc8a9ca_87.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_89.png)

例如，房屋的`OverallQual`（整体质量）对售价的影响，可能因`YearBuilt`（建造年份）不同而不同。较新年份的高质量房屋可能会有更高的溢价。为了捕捉这种相互作用，我们可以创建 `OverallQual` 和 `YearBuilt` 的乘积作为新特征。

![](img/4179310741c5b7580ad0055aefc8a9ca_90.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_92.png)

```python
X_poly['OQ_x_Year'] = X_poly['OverallQual'] * X_poly['YearBuilt']
```

![](img/4179310741c5b7580ad0055aefc8a9ca_94.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_95.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_97.png)

另一个可能的交互项是“质量密度”，例如 `OverallQual` 除以 `LotArea`（占地面积），这可以理解为每平方英尺的质量，可能也是一个有预测力的指标。

![](img/4179310741c5b7580ad0055aefc8a9ca_98.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_99.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_101.png)

```python
X_poly['OQ_per_Lot'] = X_poly['OverallQual'] / X_poly['LotArea']
```

![](img/4179310741c5b7580ad0055aefc8a9ca_103.png)

## 编码分类变量 🔠

![](img/4179310741c5b7580ad0055aefc8a9ca_105.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_106.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_108.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_110.png)

许多机器学习模型要求输入是数值。因此，我们需要将分类变量（如`HouseStyle`、`Neighborhood`）转换为数值形式。最常用的方法是**独热编码**。

![](img/4179310741c5b7580ad0055aefc8a9ca_112.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_113.png)

首先，我们检查 `HouseStyle` 这一列中各个取值的分布，确保每个类别都有足够的样本量。

![](img/4179310741c5b7580ad0055aefc8a9ca_115.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_116.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_117.png)

```python
X['HouseStyle'].value_counts()
```

![](img/4179310741c5b7580ad0055aefc8a9ca_119.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_120.png)

如果类别分布合理，我们可以使用 `pd.get_dummies()` 函数进行独热编码。它会为原始列的每一个唯一值创建一个新的二进制（0或1）列。

![](img/4179310741c5b7580ad0055aefc8a9ca_122.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_123.png)

```python
house_style_dummies = pd.get_dummies(X['HouseStyle'], prefix='HS')
# 可以将这些新的虚拟变量列合并回原特征矩阵
X_encoded = pd.concat([X, house_style_dummies], axis=1)
```

![](img/4179310741c5b7580ad0055aefc8a9ca_124.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_125.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_127.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_128.png)

实际上，你可以直接对整个数据框的**所有对象类型列**应用 `pd.get_dummies()`。

![](img/4179310741c5b7580ad0055aefc8a9ca_130.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_132.png)

```python
X_encoded_all = pd.get_dummies(X)
```

![](img/4179310741c5b7580ad0055aefc8a9ca_134.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_136.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_137.png)

对于某些类别，如果某些取值出现的频率非常低（例如少于8次），直接为其创建单独的虚拟变量可能意义不大，且容易导致过拟合。更好的做法是将这些稀有类别归为一类，例如“其他”。

![](img/4179310741c5b7580ad0055aefc8a9ca_138.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_140.png)

我们以 `Neighborhood` 列为例：

![](img/4179310741c5b7580ad0055aefc8a9ca_142.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_143.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_144.png)

```python
# 1. 查看各街区频数
neigh_counts = X['Neighborhood'].value_counts()
# 2. 找出频数小于8的街区名称
low_freq_neighs = neigh_counts[neigh_counts < 8].index.tolist()
# 3. 将这些稀有类别替换为“Other”
X['Neighborhood_processed'] = X['Neighborhood'].replace(low_freq_neighs, 'Other')
# 4. 检查处理后的分布
X['Neighborhood_processed'].value_counts()
```

![](img/4179310741c5b7580ad0055aefc8a9ca_146.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_148.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_150.png)

处理完成后，再对 `Neighborhood_processed` 列进行独热编码。

![](img/4179310741c5b7580ad0055aefc8a9ca_152.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_154.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_155.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_157.png)

## 总结 📝

![](img/4179310741c5b7580ad0055aefc8a9ca_159.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_160.png)

本节课中我们一起学习了特征工程中几个关键的实践步骤：
1.  **处理缺失值**：使用 `.isnull().sum()` 识别缺失值，并根据数据含义选择中位数、均值或特定值（如0）进行填充。
2.  **数据探索**：利用 `pairplot` 可视化分析特征分布、特征与目标的关系以及特征间的相关性，为后续的转换提供依据。
3.  **创建衍生特征**：
    *   **多项式特征**：通过平方等方式处理非线性关系。
    *   **交互项**：通过特征相乘来捕捉特征间的相互作用。
4.  **编码分类变量**：使用**独热编码**将分类变量转换为模型可用的数值格式，并对稀有类别进行归并处理。

![](img/4179310741c5b7580ad0055aefc8a9ca_162.png)

![](img/4179310741c5b7580ad0055aefc8a9ca_163.png)

这些步骤是数据预处理的核心，通常占据了机器学习项目80%的工作量。扎实的特征工程能为模型性能打下坚实基础。在接下来的课程中，我们将学习如何将这些处理好的数据输入到模型中进行训练。