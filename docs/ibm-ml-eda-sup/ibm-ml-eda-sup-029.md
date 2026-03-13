# 029：Python中的常见变量转换 📊

![](img/49c047babdbbceaf36303d394a8f5aa1_1.png)

在本节课中，我们将学习如何根据特征类型，在Python中应用不同的变量转换方法。我们将回顾连续数值、无序分类数据以及有序分类数据的处理方式，并介绍相应的Python实现工具。



![](img/49c047babdbbceaf36303d394a8f5aa1_3.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_4.png)

## 回顾变量转换类型 🔄

上一节我们介绍了线性回归的例子，并看到了如何使用多项式转换和对数转换来确保必要的线性关系。本节中，我们将根据特征类型，系统性地总结所有可能应用的转换方法。



![](img/49c047babdbbceaf36303d394a8f5aa1_6.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_7.png)

## 连续数值数据的转换 📈

![](img/49c047babdbbceaf36303d394a8f5aa1_8.png)

对于连续的数值型数据，我们通常希望使用标准化或归一化转换。

![](img/49c047babdbbceaf36303d394a8f5aa1_10.png)

以下是三种常见的缩放方法：

![](img/49c047babdbbceaf36303d394a8f5aa1_12.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_13.png)

*   **标准化 (Standard Scaling)**：将数据转换为均值为0，标准差为1的分布。公式为：`z = (x - μ) / σ`
*   **最小-最大缩放 (Min-Max Scaling)**：将数据缩放到一个固定的范围，通常是[0, 1]。公式为：`X_scaled = (X - X_min) / (X_max - X_min)`
*   **鲁棒缩放 (Robust Scaling)**：使用中位数和四分位数进行缩放，对异常值不敏感。



在Python中，我们可以从`sklearn.preprocessing`模块导入相应的类来实现这些转换。

![](img/49c047babdbbceaf36303d394a8f5aa1_15.png)

```python
from sklearn.preprocessing import StandardScaler, MinMaxScaler, RobustScaler
```



![](img/49c047babdbbceaf36303d394a8f5aa1_16.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_17.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_18.png)

## 无序分类数据的转换 🏷️

![](img/49c047babdbbceaf36303d394a8f5aa1_19.png)

接下来，我们处理名义或无序的分类数据，例如“真/假”或“红/蓝/绿”。

![](img/49c047babdbbceaf36303d394a8f5aa1_21.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_22.png)

对于这类数据，如果只有两个类别，我们可以使用二进制编码；如果有多个不同的值，则需要使用独热编码。



![](img/49c047babdbbceaf36303d394a8f5aa1_24.png)

以下是一些需要了解的函数：

*   **`LabelEncoder`**：将类别标签编码为0到n_classes-1的整数。
*   **`OneHotEncoder`**：将分类特征转换为独热编码（0和1）的数组，每个类别生成一个新列。
*   **`pandas.get_dummies`**：Pandas库中的函数，同样可以将单个分类列转换为多个表示独热编码的列。



## 有序分类数据的转换 🔢

![](img/49c047babdbbceaf36303d394a8f5aa1_26.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_27.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_28.png)

最后，我们可以处理有序分类数据，这类数据存在某种顺序关系。

![](img/49c047babdbbceaf36303d394a8f5aa1_29.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_30.png)

我们可能希望用数字（如1，2，3）对其进行编码，其中3代表最高等级，1代表最低等级。



编码有序变量的一些有用函数包括：

![](img/49c047babdbbceaf36303d394a8f5aa1_32.png)

*   **`DictVectorizer`**：你需要为每个不同的值指定一个对应的数字。
*   **`OrdinalEncoder`**：你需要按正确顺序传入所有值的名称列表，以便编码器保持你希望转换后变量所呈现的顺序。



![](img/49c047babdbbceaf36303d394a8f5aa1_33.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_34.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_35.png)

## 本节总结 📝

本节课中，我们一起学习了特征工程和变量转换。

![](img/49c047babdbbceaf36303d394a8f5aa1_37.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_38.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_39.png)

我们首先回顾了线性回归的例子，了解了如何使用多项式转换和对数转换来确保线性关系。

接着，我们讨论了特征编码，学习了如何对无序分类数据使用独热编码，以及对有序分类数据使用序数编码，并将其转换为数值。

![](img/49c047babdbbceaf36303d394a8f5aa1_41.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_42.png)

最后，我们探讨了特征缩放，讨论了标准化和归一化的不同选项，并通过可视化理解了确保特征处于相同尺度的重要性。

![](img/49c047babdbbceaf36303d394a8f5aa1_44.png)

![](img/49c047babdbbceaf36303d394a8f5aa1_45.png)

现在，让我们将所学知识应用到一个Jupyter笔记本中。