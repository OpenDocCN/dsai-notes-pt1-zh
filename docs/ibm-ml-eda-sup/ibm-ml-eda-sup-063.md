# 063：训练集和测试集分割实验（选修部分）第2部分 🔧

![](img/3d38103bacdc97251250451abc8c3395_1.png)

在本节课中，我们将深入探讨第三个问题，即如何对分类特征进行独热编码，并比较编码前后对模型结果的影响。我们将介绍独热编码的核心概念、实现方法以及需要注意的关键点。

![](img/3d38103bacdc97251250451abc8c3395_3.png)

---

## 概述

![](img/3d38103bacdc97251250451abc8c3395_5.png)

上一节我们讨论了数据预处理的基本步骤。本节中，我们将重点关注如何将分类变量转换为机器学习模型可以处理的数值形式，具体来说，就是使用独热编码技术。

![](img/3d38103bacdc97251250451abc8c3395_7.png)

![](img/3d38103bacdc97251250451abc8c3395_9.png)

![](img/3d38103bacdc97251250451abc8c3395_10.png)

我们将创建一个新的数据集，其中所有之前讨论过的分类特征都将被独热编码。然后，我们将拟合这个数据，并观察与不包含这些独热编码分类变量相比，结果会如何变化。

## 核心步骤与概念

![](img/3d38103bacdc97251250451abc8c3395_12.png)

### 1. 创建数据副本

![](img/3d38103bacdc97251250451abc8c3395_14.png)

首先，我们需要创建一个原始数据框的独立副本，以便在其上进行独热编码操作，而不会影响原始数据。

```python
import pandas as pd
from sklearn.preprocessing import OneHotEncoder

# 创建数据副本
data_OHC = data.copy()
```

### 2. 初始化独热编码器

![](img/3d38103bacdc97251250451abc8c3395_16.png)

![](img/3d38103bacdc97251250451abc8c3395_17.png)

![](img/3d38103bacdc97251250451abc8c3395_19.png)

我们将使用Scikit-learn的`OneHotEncoder`。与`pd.get_dummies`相比，它在处理可能扩展到数千列的大型数据集时，内存效率更高。

```python
# 初始化OneHotEncoder对象
ohe = OneHotEncoder()
```

![](img/3d38103bacdc97251250451abc8c3395_21.png)

![](img/3d38103bacdc97251250451abc8c3395_23.png)

**一个重要概念：** 在初始化编码器时，可以设置`drop='first'`参数。这对于希望保持模型可解释性的线性回归非常重要。

![](img/3d38103bacdc97251250451abc8c3395_25.png)

![](img/3d38103bacdc97251250451abc8c3395_26.png)

![](img/3d38103bacdc97251250451abc8c3395_27.png)

### 3. 理解“丢弃第一列”的重要性

![](img/3d38103bacdc97251250451abc8c3395_29.png)

![](img/3d38103bacdc97251250451abc8c3395_31.png)

假设我们有一个“是否临海”的列，经过独热编码后，会变成两列：“是临海”和“非临海”。

![](img/3d38103bacdc97251250451abc8c3395_33.png)

![](img/3d38103bacdc97251250451abc8c3395_34.png)

![](img/3d38103bacdc97251250451abc8c3395_35.png)

![](img/3d38103bacdc97251250451abc8c3395_37.png)

| 原始列 | 临海_是 | 临海_否 | 房价 |
| :--- | :---: | :---: | :---: |
| 是 | 1 | 0 | 5 |
| 否 | 0 | 1 | 4 |

![](img/3d38103bacdc97251250451abc8c3395_39.png)

![](img/3d38103bacdc97251250451abc8c3395_41.png)

![](img/3d38103bacdc97251250451abc8c3395_43.png)

![](img/3d38103bacdc97251250451abc8c3395_45.png)

如果不丢弃其中一列，这两列存在完全的多重共线性（当一列为1时，另一列必为0）。在线性回归模型 `y = β₀ + β₁X₁ + β₂X₂` 中，这会导致系数 `β₀`, `β₁`, `β₂` 有无限多种组合能拟合数据，从而**影响系数的可解释性**。

![](img/3d38103bacdc97251250451abc8c3395_46.png)

如果丢弃“非临海”列，模型变为 `y = β₀ + β₁X₁`。此时：
*   当 `X₁=0`（非临海），`y = β₀`。从数据知房价为4，所以 `β₀ = 4`。
*   当 `X₁=1`（临海），`y = β₀ + β₁ = 5`。代入 `β₀=4`，得 `β₁ = 1`。

![](img/3d38103bacdc97251250451abc8c3395_48.png)

这样，系数 `β₁=1` 就清晰地解释了“临海属性为房价增加了1个单位价值”，模型变得可解释。虽然丢弃一列可能不影响预测精度，但它是保证线性模型系数可解释性的最佳实践。本节课我们主要关注预测，因此暂不丢弃列。

![](img/3d38103bacdc97251250451abc8c3395_50.png)

![](img/3d38103bacdc97251250451abc8c3395_51.png)

![](img/3d38103bacdc97251250451abc8c3395_53.png)

![](img/3d38103bacdc97251250451abc8c3395_55.png)

### 4. 循环处理每个分类列

![](img/3d38103bacdc97251250451abc8c3395_57.png)

以下是处理每个分类列的核心循环逻辑。我们以“neighborhood”（街区）列为例进行分解说明。

![](img/3d38103bacdc97251250451abc8c3395_59.png)

![](img/3d38103bacdc97251250451abc8c3395_61.png)

![](img/3d38103bacdc97251250451abc8c3395_63.png)

![](img/3d38103bacdc97251250451abc8c3395_64.png)

```python
# 假设 cat_OHC_columns 是包含所有需要编码的分类列名的列表
for col in cat_OHC_columns:
    # 使用双括号确保提取的是二维DataFrame，而非一维Series
    col_data = data_OHC[[col]]
    
    # 对当前列进行拟合和转换，得到稀疏矩阵
    new_sparse = ohe.fit_transform(col_data)
    
    # 从副本中删除原始的分类列
    data_OHC = data_OHC.drop(col, axis=1)
    
    # 获取新生成的列名（例如：neighborhood_Bloomington）
    new_col_names = ohe.get_feature_names_out([col])
    
    # 将稀疏矩阵转换为密集数组，再转为DataFrame，并赋予新列名
    new_df = pd.DataFrame(new_sparse.toarray(), columns=new_col_names)
    
    # 将新的独热编码列连接到数据副本的右侧
    data_OHC = pd.concat([data_OHC, new_df], axis=1)
```

![](img/3d38103bacdc97251250451abc8c3395_66.png)

![](img/3d38103bacdc97251250451abc8c3395_68.png)

![](img/3d38103bacdc97251250451abc8c3395_70.png)

![](img/3d38103bacdc97251250451abc8c3395_72.png)

### 5. 关键技巧：使用双括号

![](img/3d38103bacdc97251250451abc8c3395_74.png)

一个常见错误是使用单括号提取列，这会得到一个一维的Pandas Series（形状为 `(n,)`）。许多Scikit-learn的转换器和模型要求输入是二维的（形状为 `(n, 1)` 或 `(n, m)`）。

![](img/3d38103bacdc97251250451abc8c3395_76.png)

![](img/3d38103bacdc97251250451abc8c3395_77.png)

```python
# 错误：一维数组
one_dim = data['neighborhood']  # 形状: (1379,)

![](img/3d38103bacdc97251250451abc8c3395_79.png)

# 正确：二维DataFrame
two_dim = data[['neighborhood']] # 形状: (1379, 1)
```
使用双括号 `[['column_name']]` 可以确保我们始终处理二维数据，避免后续出错。

![](img/3d38103bacdc97251250451abc8c3395_81.png)

![](img/3d38103bacdc97251250451abc8c3395_82.png)

![](img/3d38103bacdc97251250451abc8c3395_84.png)

### 6. 稀疏矩阵的优势

![](img/3d38103bacdc97251250451abc8c3395_86.png)

![](img/3d38103bacdc97251250451abc8c3395_87.png)

![](img/3d38103bacdc97251250451abc8c3395_88.png)

![](img/3d38103bacdc97251250451abc8c3395_89.png)

`OneHotEncoder` 默认输出稀疏矩阵。对于像“街区”这样有25个不同取值的列，独热编码会生成25个新列。但每行只有一个值是1，其余都是0。

![](img/3d38103bacdc97251250451abc8c3395_91.png)

![](img/3d38103bacdc97251250451abc8c3395_92.png)

稀疏矩阵只存储非零值的位置和数值，而不是一个庞大的、大部分是0的二维数组。这在处理自然语言处理等会产生成千上万列的任务时，能**极大地节省内存**。

![](img/3d38103bacdc97251250451abc8c3395_94.png)

![](img/3d38103bacdc97251250451abc8c3395_96.png)

![](img/3d38103bacdc97251250451abc8c3395_98.png)

```python
# 查看稀疏矩阵（节省内存，但不易读）
print(new_sparse)
# 输出类似: (0, 4) 1.0  (1, 24) 1.0 ... 表示在第0行第4列是1，第1行第24列是1...

![](img/3d38103bacdc97251250451abc8c3395_100.png)

![](img/3d38103bacdc97251250451abc8c3395_102.png)

![](img/3d38103bacdc97251250451abc8c3395_104.png)

# 转换为密集数组（易读，但消耗内存）
print(new_sparse.toarray())
# 输出一个完整的 1379行 x 25列 的数组，大部分是0
```

![](img/3d38103bacdc97251250451abc8c3395_105.png)

![](img/3d38103bacdc97251250451abc8c3395_107.png)

![](img/3d38103bacdc97251250451abc8c3395_109.png)

![](img/3d38103bacdc97251250451abc8c3395_110.png)

### 7. 处理原始数据

![](img/3d38103bacdc97251250451abc8c3395_112.png)

![](img/3d38103bacdc97251250451abc8c3395_114.png)

![](img/3d38103bacdc97251250451abc8c3395_115.png)

在对副本进行编码的同时，我们需要从**原始数据集**中删除所有字符串类型的分类列，因为机器学习模型无法直接处理字符串。

![](img/3d38103bacdc97251250451abc8c3395_117.png)

![](img/3d38103bacdc97251250451abc8c3395_119.png)

![](img/3d38103bacdc97251250451abc8c3395_121.png)

```python
# 删除原始数据中的所有对象类型（字符串）列
data = data.drop(cat_OHC_columns, axis=1)
```
原始数据列数从80减少到37，因为我们删除了43个分类列。

![](img/3d38103bacdc97251250451abc8c3395_123.png)

![](img/3d38103bacdc97251250451abc8c3395_124.png)

![](img/3d38103bacdc97251250451abc8c3395_125.png)

![](img/3d38103bacdc97251250451abc8c3395_127.png)

### 8. 验证结果

![](img/3d38103bacdc97251250451abc8c3395_129.png)

![](img/3d38103bacdc97251250451abc8c3395_131.png)

![](img/3d38103bacdc97251250451abc8c3395_132.png)

最后，我们验证独热编码是否成功添加了预期的列数。

![](img/3d38103bacdc97251250451abc8c3395_134.png)

![](img/3d38103bacdc97251250451abc8c3395_136.png)

![](img/3d38103bacdc97251250451abc8c3395_138.png)

```python
# 计算新增的列数
new_column_count = data_OHC.shape[1] - data.shape[1]
print(f"新增了 {new_column_count} 个独热编码列。")
# 输出: 新增了 215 个独热编码列。
```
这与我们之前的预测（215个新列）相符。

![](img/3d38103bacdc97251250451abc8c3395_140.png)

![](img/3d38103bacdc97251250451abc8c3395_142.png)

---

![](img/3d38103bacdc97251250451abc8c3395_144.png)

![](img/3d38103bacdc97251250451abc8c3395_145.png)

## 总结

![](img/3d38103bacdc97251250451abc8c3395_146.png)

![](img/3d38103bacdc97251250451abc8c3395_148.png)

本节课我们一起深入学习了独热编码的实践应用。

![](img/3d38103bacdc97251250451abc8c3395_150.png)

![](img/3d38103bacdc97251250451abc8c3395_151.png)

![](img/3d38103bacdc97251250451abc8c3395_153.png)

![](img/3d38103bacdc97251250451abc8c3395_155.png)

我们首先创建了数据副本以避免污染原始数据，然后使用Scikit-learn的`OneHotEncoder`进行编码。我们探讨了“丢弃第一列”对于线性回归模型可解释性的重要性，并学习了使用**双括号**确保数据二维性的关键技巧。此外，我们还了解了**稀疏矩阵**在节省内存方面的优势，以及如何将编码后的新列整合回数据框中。

![](img/3d38103bacdc97251250451abc8c3395_157.png)

最后，我们对比了编码前后数据集的变化，验证了操作的正确性。这些技能是构建有效机器学习管道的重要组成部分。

![](img/3d38103bacdc97251250451abc8c3395_159.png)

![](img/3d38103bacdc97251250451abc8c3395_160.png)

![](img/3d38103bacdc97251250451abc8c3395_161.png)

![](img/3d38103bacdc97251250451abc8c3395_162.png)

![](img/3d38103bacdc97251250451abc8c3395_164.png)

![](img/3d38103bacdc97251250451abc8c3395_165.png)

在下一节课中，我们将进入训练集和测试集分割的部分。