# 人工智能—计算机视觉CV公开课（七月在线出品） - P16：时间序列实战：心电图疾病监测 📈

![](img/1cebb71d97b4a547eba57fece2a7f602_0.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_2.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_4.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_6.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_8.png)

在本节课中，我们将要学习时间序列分析的基础知识、特征提取方法、分类与回归模型，并通过一个心电图疾病识别的实战案例，掌握如何应用深度学习模型解决实际问题。

## 时间序列介绍 📅

时间序列数据在我们的日常生活中非常常见。这类数据的特点是按照时间维度有规律地进行采集和存储，最终形成一个数据序列，而不是单个数值点。

时间序列数据具有规律性，例如每分钟采集一次股票价格。如果数据是无规律采集的，则不能称为时间序列。下图展示了一个典型的按分钟级别采集的时间序列走势图，其数值会随时间推移而波动。

![](img/1cebb71d97b4a547eba57fece2a7f602_10.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_12.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_13.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_14.png)

## 时间序列的组成与分解 🔍

上一节我们介绍了时间序列的基本概念，本节中我们来看看时间序列通常由哪些部分组成。

一个时间序列通常可以分解为以下三项：
*   **趋势项**：描述序列长期的规律，可以是增加、减少或保持不变。
*   **季节项**：序列在特定周期（如一天、一周、一年）内呈现的规律性波动。
*   **残差项**：在剔除趋势和季节因素后，剩余的随机波动。

原始时间序列可以看作是这三项的组合。根据组合方式的不同，主要有两种模型：

**加法模型**：当季节和残差与趋势无关时，序列由三者相加得到。
`序列 = 趋势 + 季节 + 残差`

![](img/1cebb71d97b4a547eba57fece2a7f602_16.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_17.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_19.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_21.png)

**乘法模型**：当季节波动的幅度与趋势水平相关时，序列由三者相乘得到。
`序列 = 趋势 * 季节 * 残差`

![](img/1cebb71d97b4a547eba57fece2a7f602_23.png)

## 代码实践：构建与分解序列 💻

![](img/1cebb71d97b4a547eba57fece2a7f602_25.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_27.png)

理解了理论后，我们通过代码来实践如何构建和分解时间序列。

以下是构建加法模型时间序列的示例代码：
```python
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose

# 创建时间轴
time = np.arange(1, 51)
# 创建趋势项（线性增长）
trend = time * 2.75
# 创建季节项（正弦波动）
seasonal = 10 + np.sin(time * 10)
# 创建残差项（随机噪声）
residual = np.random.normal(size=50) * 10
# 组合成加法模型序列
additive_ts = trend + seasonal + residual
```

![](img/1cebb71d97b4a547eba57fece2a7f602_29.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_31.png)

接下来，我们可以使用 `seasonal_decompose` 函数对构建好的序列进行分解，直观地观察趋势、季节和残差项。

## 时间序列的特征提取 🛠️

![](img/1cebb71d97b4a547eba57fece2a7f602_33.png)

要对时间序列进行建模，特征提取是关键一步。我们可以从多个角度提取有价值的特征。

![](img/1cebb71d97b4a547eba57fece2a7f602_35.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_37.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_39.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_41.png)

以下是基于日期时间的特征提取方法：
我们可以从时间戳中提取出年、月、日、小时、分钟、星期几、是一年中的第几天等信息，这些特征常能反映周期性规律。

另一种重要的特征是**滞后特征**。它的核心思想是利用历史数据作为当前时刻的特征。
例如，`lag1` 表示用前一时刻的值作为当前特征，`lag2` 表示用前两个时刻的值，以此类推。在Pandas中，这可以通过 `shift()` 操作轻松实现。

![](img/1cebb71d97b4a547eba57fece2a7f602_43.png)

此外，我们还可以计算**滚动窗口特征**。
例如，计算窗口大小为7的滚动平均值，即用最近7个时刻的均值作为一个特征。这有助于平滑短期波动，捕捉中期趋势。与之相对的是**扩展窗口特征**，它计算从序列起点到当前时刻所有数据的累积统计值（如累积均值）。

![](img/1cebb71d97b4a547eba57fece2a7f602_45.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_47.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_48.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_50.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_52.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_53.png)

## 时间序列的分类与回归模型 🤖

![](img/1cebb71d97b4a547eba57fece2a7f602_55.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_57.png)

提取特征后，我们就可以构建模型了。时间序列任务主要分为分类和回归两大类。

![](img/1cebb71d97b4a547eba57fece2a7f602_58.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_60.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_62.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_63.png)

**时间序列分类**：目标是识别一个序列所属的类别（如心电图是否异常）。常用方法有：
1.  基于距离的方法（如KNN），使用DTW（动态时间规整）等算法计算序列相似度。
2.  提取手工特征（如上节所述），然后使用传统机器学习模型（如随机森林）进行分类。
3.  使用端到端的深度学习模型，如一维卷积神经网络（1D CNN），自动学习特征并分类。

![](img/1cebb71d97b4a547eba57fece2a7f602_65.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_67.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_69.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_70.png)

**时间序列回归/预测**：目标是预测序列未来的值。常用方法有：
1.  简单方法，如使用历史数据的滑动平均值作为预测值。
2.  经典统计模型，如ARIMA、Prophet。
3.  深度学习模型，如RNN、LSTM、DeepAR等，它们能捕捉复杂的时序依赖关系。

![](img/1cebb71d97b4a547eba57fece2a7f602_72.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_73.png)

例如，我们可以使用GluonTS库中的DeepAR模型进行时间序列预测，该模型会输出预测值及相应的置信区间。

![](img/1cebb71d97b4a547eba57fece2a7f602_75.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_76.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_78.png)

## 实战案例：心电图疾病识别 ❤️

![](img/1cebb71d97b4a547eba57fece2a7f602_80.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_82.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_83.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_85.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_87.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_88.png)

现在，我们将应用所学知识，解决一个实际问题：心电图疾病识别。心电图信号是典型的时间序列数据。

![](img/1cebb71d97b4a547eba57fece2a7f602_90.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_92.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_94.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_96.png)

我们的目标是构建一个分类模型，根据一段心电图信号判断其对应的疾病类别。数据集中包含5个类别，且存在类别不均衡的问题。

![](img/1cebb71d97b4a547eba57fece2a7f602_98.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_100.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_101.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_102.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_104.png)

以下是解决此问题的核心步骤：
1.  **数据预处理**：读取数据，进行下采样以缓解类别不均衡，并将数据集划分为训练集和验证集。
2.  **数据格式化**：将数据转换为适合卷积网络输入的三维格式 `(样本数, 序列长度, 1)`。
3.  **构建1D CNN模型**：我们搭建一个包含三个一维卷积层、一个池化层和三个全连接层的网络。模型输入为187维的心电信号序列。
    ```python
    model = Sequential([
        Conv1D(filters=32, kernel_size=3, activation='relu', input_shape=(187, 1)),
        Conv1D(filters=32, kernel_size=3, activation='relu'),
        Conv1D(filters=32, kernel_size=3, activation='relu'),
        GlobalAveragePooling1D(),
        Dense(64, activation='relu'),
        Dense(32, activation='relu'),
        Dense(5, activation='softmax') # 5个类别
    ])
    ```
4.  **模型训练与评估**：使用分类交叉熵损失函数训练模型。经过少量轮次的训练，模型在验证集上即可达到很高的准确率。通过混淆矩阵可以进一步观察模型在各个类别上的分类效果。

![](img/1cebb71d97b4a547eba57fece2a7f602_106.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_108.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_110.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_112.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_113.png)

## 总结 📝

![](img/1cebb71d97b4a547eba57fece2a7f602_115.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_117.png)

![](img/1cebb71d97b4a547eba57fece2a7f602_118.png)

本节课中我们一起学习了时间序列分析的全流程。我们从时间序列的定义和组成（趋势、季节、残差）入手，学习了如何通过代码构建和分解序列。接着，探讨了关键的特征提取技术，包括日期特征、滞后特征和滚动窗口特征。然后，我们介绍了用于时间序列分类和回归的各类模型。最后，通过心电图疾病识别的实战案例，完整演示了如何使用一维卷积神经网络对时间序列数据进行分类建模。希望本教程能帮助你入门时间序列分析，并将其应用于更多实际场景。