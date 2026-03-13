# 033：可视化模型评估

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_0.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_2.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_4.png)

在本节课中，我们将学习如何使用可视化工具来评估回归模型。我们将介绍三种核心的可视化方法：回归图、残差图和分布图。这些图表能帮助我们直观地理解模型的预测效果、误差分布以及模型与数据的关系。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_5.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_7.png)

---

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_9.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_11.png)

## 🔍 回归图

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_13.png)

上一节我们介绍了模型评估的基本概念，本节中我们来看看第一种可视化工具——回归图。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_15.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_16.png)

回归图是估计两个变量之间关系、相关性强弱以及关系方向（正相关或负相关）的良好工具。在回归图中，横轴代表自变量，纵轴代表因变量。图中的每个点代表一个不同的数据点，而拟合线则代表模型的预测值。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_18.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_19.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_20.png)

有多种方法可以绘制回归图，其中一种简单的方法是使用 Seaborn 库中的 `regplot` 函数。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_22.png)

以下是绘制回归图的步骤：

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_24.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_25.png)

1.  首先导入 seaborn 库。
2.  使用 `regplot` 函数。
    *   参数 `x` 是包含自变量（特征）的列名。
    *   参数 `y` 是包含因变量（目标）的列名。
    *   参数 `data` 是数据框的名称。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_27.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_28.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_30.png)

示例代码如下：
```python
import seaborn as sns
sns.regplot(x='independent_variable', y='dependent_variable', data=df)
```

---

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_32.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_33.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_34.png)

## 📉 残差图

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_36.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_38.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_39.png)

了解了如何通过回归图观察整体关系后，我们接下来深入分析模型的误差，这就需要用到残差图。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_41.png)

残差图展示了实际值与预测值之间的误差。我们通过用预测值减去实际目标值来得到残差。然后，我们将残差值绘制在纵轴上，自变量作为横轴。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_43.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_45.png)

观察残差图可以为我们提供关于数据的洞察。我们期望看到的结果是：残差均值为0，围绕X轴均匀分布，且方差相似，没有明显的曲线模式。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_47.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_48.png)

以下是几种典型的残差图模式：

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_50.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_52.png)

*   **理想的线性模式**：残差随机、均匀地分布在零点线上下，表明线性模型是合适的。
*   **存在曲率的模式**：残差误差的值随X变化。例如，在某些区域所有残差都为正，在另一些区域则为负，且误差较大。这表明残差不是随机分布的，线性假设可能不正确，暗示了非线性函数的存在。
*   **异方差模式**：残差的方差随X的增加而增加。这表明我们的模型可能不正确。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_54.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_56.png)

我们可以使用 seaborn 来创建残差图。首先导入 seaborn，然后使用 `residplot` 函数。第一个参数是自变量（特征）序列，第二个参数是因变量（目标）序列。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_58.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_59.png)

示例代码如下：
```python
import seaborn as sns
sns.residplot(x=feature_series, y=target_series)
```

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_60.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_62.png)

---

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_63.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_65.png)

## 📊 分布图

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_67.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_69.png)

在分析了单变量的误差后，对于具有多个自变量的模型，我们可以使用分布图来更全面地比较预测值与实际值。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_71.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_73.png)

分布图用于比较预测值与实际值的分布。这些图对于可视化具有多个自变量或特征的模型极为有用。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_75.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_77.png)

让我们看一个简化的例子。我们检查纵轴，然后计算并绘制预测值大约等于1、2、3等值的点的数量。接着，我们对目标值重复这个过程。由于目标和预测值是连续值，而直方图适用于离散值，因此 pandas 会将它们转换为分布。纵轴经过缩放，使得分布下的面积等于一。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_79.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_80.png)

这是一个使用分布图的例子。因变量或特征是价格。模型得出的拟合值用蓝色表示，实际值用红色表示。通过比较，我们可以看出预测值在哪些区间更准确或更不准确。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_82.png)

以下是创建分布图的代码。实际值作为参数传入。因为我们想要的是分布图而不是直方图，所以需要将 `hist` 参数设置为 `False`。可以设置颜色和标签。预测值用于第二个图的绘制，其余参数相应设置。

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_84.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_85.png)

示例代码如下：
```python
import seaborn as sns
# 绘制实际值分布
sns.distplot(actual_values, hist=False, color="red", label="Actual Values")
# 绘制预测值分布
sns.distplot(predicted_values, hist=False, color="blue", label="Predicted Values")
plt.legend()
```

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_86.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_88.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_90.png)

---

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_92.png)

## ✅ 总结

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_93.png)

![](img/83f050db4d028bd6c49bcc7e7faa7ae6_95.png)

本节课中我们一起学习了三种用于模型评估的可视化方法。**回归图**帮助我们直观理解变量间的整体关系；**残差图**让我们深入分析预测误差的模式，判断线性假设是否成立；**分布图**则能有效比较在多特征模型下预测值与实际值的整体分布情况。掌握这些可视化工具，将极大地提升我们诊断和解释回归模型的能力。