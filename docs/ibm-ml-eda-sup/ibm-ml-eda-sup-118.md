# 118：使用支持向量机（SVM）进行数据探索与预处理 🍷

![](img/87518773a58ac627a5208dda23bdc623_1.png)

![](img/87518773a58ac627a5208dda23bdc623_3.png)

![](img/87518773a58ac627a5208dda23bdc623_4.png)

在本节课中，我们将学习如何使用Python中的支持向量机（SVM）。我们将使用一个葡萄酒质量数据集，该数据集包含了葡萄酒的多种化学属性（如酸度、糖分、pH值和酒精含量）以及一个从3到9的质量评分（分数越高越好），还有一个表示葡萄酒颜色的标签（红葡萄酒或白葡萄酒）。

![](img/87518773a58ac627a5208dda23bdc623_6.png)

![](img/87518773a58ac627a5208dda23bdc623_7.png)

## 1. 环境准备与数据导入

![](img/87518773a58ac627a5208dda23bdc623_9.png)

![](img/87518773a58ac627a5208dda23bdc623_10.png)

首先，我们需要确保工作目录正确，并导入必要的库。我们将使用Pandas来读取数据。

![](img/87518773a58ac627a5208dda23bdc623_12.png)

![](img/87518773a58ac627a5208dda23bdc623_13.png)

![](img/87518773a58ac627a5208dda23bdc623_14.png)

以下是导入数据并查看其结构的代码：

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.preprocessing import MinMaxScaler

![](img/87518773a58ac627a5208dda23bdc623_16.png)

![](img/87518773a58ac627a5208dda23bdc623_17.png)

![](img/87518773a58ac627a5208dda23bdc623_18.png)

# 导入数据
data = pd.read_csv('wine_quality.csv')
```

![](img/87518773a58ac627a5208dda23bdc623_20.png)

![](img/87518773a58ac627a5208dda23bdc623_22.png)

![](img/87518773a58ac627a5208dda23bdc623_23.png)

## 2. 创建目标变量

![](img/87518773a58ac627a5208dda23bdc623_25.png)

![](img/87518773a58ac627a5208dda23bdc623_27.png)

我们的目标是预测葡萄酒的颜色（红或白）。我们将创建一个二元目标变量 `y`，其中“红葡萄酒”标记为1，“白葡萄酒”标记为0。

![](img/87518773a58ac627a5208dda23bdc623_29.png)

![](img/87518773a58ac627a5208dda23bdc623_30.png)

![](img/87518773a58ac627a5208dda23bdc623_32.png)

![](img/87518773a58ac627a5208dda23bdc623_33.png)

以下是创建目标变量的步骤：

![](img/87518773a58ac627a5208dda23bdc623_35.png)

![](img/87518773a58ac627a5208dda23bdc623_36.png)

```python
# 创建目标变量 y
y = (data['color'] == 'red').astype(int)
```

## 3. 数据探索：配对图

![](img/87518773a58ac627a5208dda23bdc623_38.png)

为了理解数据特征之间的关系以及它们与目标变量的关系，我们将创建一个配对图。这有助于我们直观地看到红葡萄酒和白葡萄酒在特征空间中的分布情况。

![](img/87518773a58ac627a5208dda23bdc623_40.png)

![](img/87518773a58ac627a5208dda23bdc623_41.png)

![](img/87518773a58ac627a5208dda23bdc623_42.png)

![](img/87518773a58ac627a5208dda23bdc623_43.png)

以下是生成配对图的代码：

![](img/87518773a58ac627a5208dda23bdc623_45.png)

![](img/87518773a58ac627a5208dda23bdc623_46.png)

![](img/87518773a58ac627a5208dda23bdc623_48.png)

```python
# 创建配对图，按颜色区分
sns.pairplot(data, hue='color')
plt.show()
```

![](img/87518773a58ac627a5208dda23bdc623_50.png)

![](img/87518773a58ac627a5208dda23bdc623_51.png)

从配对图中，我们可以看到不同特征之间的散点关系以及每个特征的分布直方图。通过颜色区分，可以观察到红葡萄酒和白葡萄酒的数据点在某些特征组合下存在分离的趋势，这预示着我们可以构建一个有效的分类器。

![](img/87518773a58ac627a5208dda23bdc623_53.png)

## 4. 分析特征相关性

![](img/87518773a58ac627a5208dda23bdc623_55.png)

![](img/87518773a58ac627a5208dda23bdc623_56.png)

![](img/87518773a58ac627a5208dda23bdc623_58.png)

接下来，我们将分析每个特征与目标变量 `y` 的相关性，以找出哪些特征对预测颜色最重要。我们将计算相关性并绘制条形图。

![](img/87518773a58ac627a5208dda23bdc623_59.png)

![](img/87518773a58ac627a5208dda23bdc623_61.png)

![](img/87518773a58ac627a5208dda23bdc623_62.png)

以下是计算和绘制相关性的代码：

![](img/87518773a58ac627a5208dda23bdc623_64.png)

![](img/87518773a58ac627a5208dda23bdc623_65.png)

```python
# 选择特征列（排除目标列‘color’）
fields = data.columns[:-1]

![](img/87518773a58ac627a5208dda23bdc623_66.png)

![](img/87518773a58ac627a5208dda23bdc623_68.png)

![](img/87518773a58ac627a5208dda23bdc623_70.png)

![](img/87518773a58ac627a5208dda23bdc623_71.png)

# 计算每个特征与 y 的相关性
correlations = data[fields].corrwith(y).sort_values()

![](img/87518773a58ac627a5208dda23bdc623_73.png)

![](img/87518773a58ac627a5208dda23bdc623_75.png)

# 绘制相关性条形图
correlations.plot(kind='bar')
plt.title('Correlation with Wine Color')
plt.show()
```

![](img/87518773a58ac627a5208dda23bdc623_77.png)

![](img/87518773a58ac627a5208dda23bdc623_78.png)

![](img/87518773a58ac627a5208dda23bdc623_80.png)

相关性分析显示，“挥发性酸度”与目标呈最高正相关，而“总二氧化硫”呈最高负相关。

![](img/87518773a58ac627a5208dda23bdc623_82.png)

![](img/87518773a58ac627a5208dda23bdc623_83.png)

## 5. 选择关键特征并缩放数据

![](img/87518773a58ac627a5208dda23bdc623_84.png)

![](img/87518773a58ac627a5208dda23bdc623_85.png)

![](img/87518773a58ac627a5208dda23bdc623_87.png)

为了便于在二维空间中可视化SVM的决策边界，我们将选择与目标变量 `y` 绝对相关性最高的两个特征。同时，由于SVM对特征的尺度敏感，我们需要对数据进行缩放，确保所有特征在相同尺度上。

以下是选择特征并进行缩放的步骤：

![](img/87518773a58ac627a5208dda23bdc623_89.png)

```python
# 1. 获取与 y 的绝对相关性，并排序
abs_correlations = correlations.abs().sort_values()

![](img/87518773a58ac627a5208dda23bdc623_91.png)

![](img/87518773a58ac627a5208dda23bdc623_92.png)

![](img/87518773a58ac627a5208dda23bdc623_94.png)

![](img/87518773a58ac627a5208dda23bdc623_96.png)

# 2. 选择相关性最高的两个特征
top_two_fields = abs_correlations.index[-2:]

![](img/87518773a58ac627a5208dda23bdc623_98.png)

# 3. 使用这两个特征创建新的特征矩阵 X
X = data[top_two_fields]

![](img/87518773a58ac627a5208dda23bdc623_100.png)

![](img/87518773a58ac627a5208dda23bdc623_102.png)

# 4. 初始化缩放器并缩放数据
scaler = MinMaxScaler()
X_scaled = scaler.fit_transform(X)

![](img/87518773a58ac627a5208dda23bdc623_104.png)

![](img/87518773a58ac627a5208dda23bdc623_105.png)

![](img/87518773a58ac627a5208dda23bdc623_106.png)

![](img/87518773a58ac627a5208dda23bdc623_107.png)

# 5. 将缩放后的数组转换回DataFrame，便于后续处理
X_scaled_df = pd.DataFrame(X_scaled, columns=[f'{col}_scaled' for col in top_two_fields])
```

![](img/87518773a58ac627a5208dda23bdc623_109.png)

![](img/87518773a58ac627a5208dda23bdc623_110.png)

通过以上步骤，我们得到了一个仅包含两个关键特征且经过缩放的干净数据集 `X_scaled_df`，为下一节构建和可视化SVM模型做好了准备。

![](img/87518773a58ac627a5208dda23bdc623_112.png)

![](img/87518773a58ac627a5208dda23bdc623_113.png)

![](img/87518773a58ac627a5208dda23bdc623_115.png)

![](img/87518773a58ac627a5208dda23bdc623_117.png)

## 总结

![](img/87518773a58ac627a5208dda23bdc623_119.png)

![](img/87518773a58ac627a5208dda23bdc623_120.png)

![](img/87518773a58ac627a5208dda23bdc623_121.png)

本节课中，我们一起学习了支持向量机（SVM）应用前的数据准备流程。我们导入了葡萄酒数据集，创建了二元目标变量，并通过配对图和相关性分析探索了数据。为了简化可视化，我们选择了两个最具相关性的特征，并对其进行了标准化缩放，以确保模型性能不受特征量纲影响。下一节，我们将基于这个处理好的数据集，开始构建SVM模型并绘制其决策边界。