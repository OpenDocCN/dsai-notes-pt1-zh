# 142：Boosting与Stacking实验（第一部分）📊

在本节课中，我们将学习如何使用Python进行Boosting和Stacking建模。我们将使用智能手机人类活动识别数据库，这个数据集在之前的逻辑回归实验中已经使用过。我们将快速回顾数据集，并进行数据准备，为后续的建模工作打下基础。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_1.png)

## 1. 导入必要的库与数据 📥

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_3.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_5.png)

首先，我们需要导入进行数据分析和建模所必需的Python库。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_7.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_8.png)

以下是导入库的代码：

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import LabelEncoder
from sklearn.model_selection import train_test_split
```

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_10.png)

接着，我们使用Pandas读取数据集文件。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_12.png)

```python
data = pd.read_csv('your_dataset.csv')
```

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_13.png)

查看数据形状，可以看到数据集包含10299行和562列。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_15.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_16.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_17.png)

```python
print(data.shape)
```

## 2. 探索数据特征 🔍

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_19.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_20.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_21.png)

上一节我们导入了数据，本节中我们来看看数据的类型和基本特征。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_22.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_24.png)

首先，检查各列的数据类型分布。

```python
print(data.dtypes.value_counts())
```

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_26.png)

结果显示，561列是浮点数（float），1列是对象（object）类型。回忆之前的逻辑回归实验，这个对象类型的列就是我们的目标变量（活动类型），后续需要将其转换为整型以便在Scikit-learn模型中使用。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_28.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_29.png)

然后，我们需要确认特征数据是否已经经过标准化处理。树模型虽然对数据缩放不敏感，但为了后续Stacking步骤中可能用到的其他模型，我们仍需检查。

我们通过检查每个浮点数列的最大值和最小值是否为1和-1来确认。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_31.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_33.png)

以下是检查数据是否已缩放的步骤代码：

```python
# 1. 创建布尔序列，标识哪些列是浮点型
is_float = data.dtypes == ‘float’

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_35.png)

# 2. 选取所有浮点数列，并计算每列的最大值
max_vals = data.loc[:, is_float].max()

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_37.png)

# 3. 检查所有最大值是否都等于1
all_max_one = (max_vals == 1).all()

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_39.png)

# 4. 同样检查最小值是否都等于-1
min_vals = data.loc[:, is_float].min()
all_min_neg_one = (min_vals == -1).all()

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_41.png)

print(all_max_one, all_min_neg_one)
```

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_43.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_44.png)

运行代码后，如果两个结果都为True，则证明数据已经标准化到[-1, 1]的范围内。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_46.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_47.png)

**核心概念**：数据标准化通常将数据缩放到特定范围（如[-1, 1]或[0, 1]），公式为：
`x_scaled = (x - min) / (max - min)`
但本数据集已预先处理。

> **提示**：如果在理解某段代码时感到困惑，可以随时暂停，将代码分解成小块单独运行，观察每一步的输出结果，这是学习编程的有效方法。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_49.png)

## 3. 编码目标变量与划分数据集 🏷️

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_50.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_51.png)

在确认数据特征后，我们需要对目标变量进行预处理，并将其划分为训练集和测试集。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_53.png)

首先，使用标签编码器（LabelEncoder）将字符串类型的活动类别转换为整数。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_55.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_57.png)

```python
# 初始化标签编码器
le = LabelEncoder()

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_59.png)

# 对‘activity’列进行拟合和转换，并替换原数据
data[‘activity’] = le.fit_transform(data[‘activity’])

# 查看编码后的类别对应关系
print(le.classes_)
```

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_61.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_62.png)

`le.classes_` 数组按顺序对应编码后的整数值（如0，1，2…），在后续分析混淆矩阵时，可以用它来查找具体类别。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_63.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_65.png)

接下来，决定是否需要进行分层抽样来划分数据集。分层抽样能确保训练集和测试集中各类别的比例与原始数据集一致，这在类别不平衡时尤为重要。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_67.png)

我们先检查目标变量的分布是否平衡。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_69.png)

```python
print(data[‘activity’].value_counts())
```

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_71.png)

观察计数，如果各个类别的样本数量大致相当，则可以使用常规的`train_test_split`。如果发现明显不平衡，则应使用`stratify`参数。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_73.png)

本数据集分布较为均衡，因此我们采用常规划分。同时请注意，Boosting算法是顺序执行的，计算可能较慢。如果数据集很大，可以考虑减少训练集规模以加快实验速度。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_74.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_75.png)

以下是划分数据集的代码：

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_77.png)

```python
# 定义特征列（排除目标列‘activity’）
feature_columns = [x for x in data.columns if x != ‘activity’]

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_78.png)

# 设置特征（X）和目标（y）
X = data[feature_columns]
y = data[‘activity’]

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_80.png)

# 划分训练集和测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_82.png)

# 查看划分后的形状
print(X_train.shape, X_test.shape)
```

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_84.png)

我们将70%的数据用于训练，30%用于测试。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_86.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_87.png)

## 总结 📝

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_89.png)

本节课中我们一起学习了Boosting与Stacking实验的数据准备阶段。我们完成了以下工作：
1.  导入必要的库并加载数据集。
2.  探索了数据特征，确认数据已标准化。
3.  使用`LabelEncoder`对目标变量进行整数编码。
4.  检查了类别分布，并将数据集划分为训练集和测试集。

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_90.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_91.png)

![](img/cccfcad3f6a6d2852b5e37be1c2feccd_93.png)

数据预处理是机器学习工作流中至关重要的一步，良好的数据准备能为模型训练奠定坚实基础。在下一节课中，我们将正式使用梯度提升分类器（Gradient Boosting Classifier）开始建模。