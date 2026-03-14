# 第二三四部分 13：使用生成式AI进行可视化 📊

## 概述
在本节课中，我们将学习如何利用生成式AI进行数据可视化。我们将重点介绍使用ChatGPT将原始数据转化为生动的图表和图形，从而揭示数据中的趋势、模式和故事。

---

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_1.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_2.png)

## 什么是数据可视化？🔍
上一节我们介绍了课程目标，本节中我们来看看数据可视化的核心概念。

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_4.png)

数据可视化是利用图表、图形等视觉元素对信息进行图形化表示。它提供了一种易于理解的方式来分析数据中的趋势和模式。在大数据时代，原始信息可能令人不知所措，数据可视化工具在将数据转化为可操作的见解方面发挥着关键作用。

**核心公式**：`数据可视化 = 图形化表示(原始数据)`

简而言之，数据可视化就是将枯燥的数字转化为生动的图像，让数据自己“讲故事”。

---

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_6.png)

## 探索性数据分析 (EDA) 🔬
理解了数据可视化的基本概念后，我们接下来探讨一个关键的应用场景：探索性数据分析。

想象自己是一名数据世界的侦探。探索性数据分析就像是一个放大镜，它帮助你深入挖掘数据，揭示其中的模式、关系和隐藏的故事。这是一个将数据线索转化为有价值见解的过程。

以下是探索性数据分析的技术定义：
*   **探索性数据分析** 是一个分析和总结数据的系统过程。
*   它涉及识别数据中的**模式**、**关系**、**异常**和**见解**。
*   它为后续的数据探索和决策制定提供了初步的“侦探工作”。

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_8.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_9.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_10.png)

---

## 实践示例：使用ChatGPT和Google Colab进行EDA 📈
理论部分已经介绍完毕，现在让我们通过一个实际的机器学习项目示例，看看如何将理论应用于实践。

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_12.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_13.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_14.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_15.png)

我们将使用ChatGPT生成代码，并在Google Colab环境中创建一个EDA可视化演示。

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_16.png)

**步骤简述**：
1.  向ChatGPT提供一个提示，要求其为示例机器学习项目生成数据可视化演示代码。
2.  将生成的代码复制到Google Colab中运行。
3.  观察并分析生成的图表。

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_18.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_19.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_20.png)

**向ChatGPT提供的提示示例**：
```text
生成一个引人注目的演示，展示数据可视化在示例机器学习项目中的力量。使用Google Colab创建EDA可视化，制作有洞察力的图表、图形和摘要，以揭示数据集中的模式、关系和关键见解，将原始数据转化为视觉上引人入胜的叙述。
```

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_21.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_22.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_23.png)

ChatGPT可能会生成类似以下的Python代码（以著名的Iris数据集为例）：

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_25.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_26.png)

```python
# 第二三四部分 示例代码片段：加载数据并创建散点图矩阵
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.datasets import load_iris

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_27.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_28.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_29.png)

# 第二三四部分 加载数据
iris = load_iris()
df = pd.DataFrame(iris.data, columns=iris.feature_names)
df['target'] = iris.target

# 第二三四部分 创建散点图矩阵
sns.pairplot(df, hue='target', palette='viridis')
plt.suptitle('Iris数据集特征关系散点图矩阵', y=1.02)
plt.show()
```

运行代码后，你将获得一系列可视化图表，例如：
*   **散点图矩阵**：展示不同特征两两之间的关系。
*   **箱线图**：显示每个特征的分布和异常值。
*   **相关性热力图**：用颜色直观表示特征之间的相关性强度。
*   **直方图**：展示单个特征的分布情况。

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_31.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_32.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_33.png)

通过观察这些可视化结果，你可以轻松地识别出：
*   不同类别（如鸢尾花品种）在特征上的差异。
*   哪些特征之间存在强相关性。
*   数据的整体分布情况。

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_34.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_35.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_36.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_37.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_38.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_39.png)

这比直接阅读原始数据表格要直观和高效得多。

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_40.png)

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_41.png)

---

![](img/cc65f1696fe6bd6d4c3293d39dcb4613_43.png)

## 总结
本节课中，我们一起学习了使用生成式AI进行数据可视化的方法。我们探讨了数据可视化的定义及其重要性，并深入了解了探索性数据分析的过程。通过结合ChatGPT和Google Colab的实践示例，我们掌握了将原始数据转化为具有洞察力的视觉图表的能力，使得数据探索变得更加动态和引人入胜。