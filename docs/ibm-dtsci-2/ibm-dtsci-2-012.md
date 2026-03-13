# 012：数据科学库 📚

在本节课中，我们将要学习数据科学领域中常用的核心库。库是函数和方法的集合，它们能让你无需自己编写代码即可执行多种操作。我们将重点介绍Python库、其他语言的库，并了解它们如何帮助数据科学家高效工作。

## 概述

我们将回顾几类数据科学库，包括用于科学计算的Python库、数据可视化库、高级机器学习与深度学习库，以及其他编程语言中的相关库。理解这些工具是构建数据科学项目的基础。

![](img/4cfa4585f4d5d19d0fff669cf412b206_1.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_2.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_3.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_4.png)

---

![](img/4cfa4585f4d5d19d0fff669cf412b206_5.png)

## Python科学计算库 🧮

![](img/4cfa4585f4d5d19d0fff669cf412b206_7.png)

上一节我们介绍了库的基本概念，本节中我们来看看Python中用于科学计算的核心库。

![](img/4cfa4585f4d5d19d0fff669cf412b206_9.png)

### Pandas

![](img/4cfa4585f4d5d19d0fff669cf412b206_10.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_11.png)

Pandas提供了数据结构和工具，用于高效地进行数据清洗、操作和分析。它提供了处理不同类型数据的工具。Pandas的主要工具是一个由列和行组成的二维表格。

这个表格被称为**数据框**，其设计旨在提供简便的索引，以便你处理数据。

![](img/4cfa4585f4d5d19d0fff669cf412b206_13.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_14.png)

**核心数据结构示例**：
```python
import pandas as pd
df = pd.DataFrame(data)
```

![](img/4cfa4585f4d5d19d0fff669cf412b206_15.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_16.png)

### NumPy

![](img/4cfa4585f4d5d19d0fff669cf412b206_18.png)

NumPy库基于数组，使你能对这些数组应用数学函数。Pandas实际上是构建在NumPy之上的。

![](img/4cfa4585f4d5d19d0fff669cf412b206_20.png)

**核心概念公式**：
- 数组操作：`np.array([1, 2, 3])`
- 数学运算：`np.sum(array)`

![](img/4cfa4585f4d5d19d0fff669cf412b206_21.png)

---

![](img/4cfa4585f4d5d19d0fff669cf412b206_23.png)

## Python数据可视化库 📊

![](img/4cfa4585f4d5d19d0fff669cf412b206_24.png)

数据可视化方法是与他人沟通和展示分析有意义结果的重要方式。这些库能让你创建图表、图形和地图。

![](img/4cfa4585f4d5d19d0fff669cf412b206_26.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_27.png)

以下是两个主要的Python可视化库：

*   **Matplotlib**：这是最著名的数据可视化库，非常适合制作图形和图表。这些图表也具有高度可定制性。
*   **Seaborn**：这是一个基于Matplotlib的高级可视化库。Seaborn可以轻松生成热图、时间序列图和小提琴图等。

![](img/4cfa4585f4d5d19d0fff669cf412b206_29.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_30.png)

---

## 机器学习与深度学习库 🤖

对于机器学习，Scikit-learn库包含了用于统计建模的工具，包括回归、分类、聚类等。它构建在NumPy、SciPy和Matplotlib之上，并且对于这种高级方法来说，入门相对简单。你定义模型并指定想要使用的参数类型。

对于深度学习：

*   **Keras**：使你能构建标准的深度学习模型。与Scikit-learn类似，其高级接口使你能快速简单地构建模型。它可以使用图形处理单元运行，但对于许多深度学习场景，可能需要更低层级的环境。
*   **TensorFlow**：这是一个用于大规模生产深度学习模型的低级框架。它为生产而设计，但对于实验可能显得笨重。
*   **PyTorch**：用于实验，使研究人员能轻松测试他们的想法。

---

## 其他语言的数据科学库 🌐

![](img/4cfa4585f4d5d19d0fff669cf412b206_32.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_33.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_34.png)

除了Python，其他编程语言也拥有强大的数据科学生态系统。

![](img/4cfa4585f4d5d19d0fff669cf412b206_35.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_36.png)

### Apache Spark

Apache Spark是一个通用集群计算框架，使你能使用计算集群处理数据。这意味着你可以使用多台计算机同时并行处理数据。Spark库具有与Pandas、NumPy和Scikit-learn类似的功能。

![](img/4cfa4585f4d5d19d0fff669cf412b206_38.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_39.png)

Apache Spark数据处理作业可以使用Python、R、Scala或SQL。

![](img/4cfa4585f4d5d19d0fff669cf412b206_40.png)

### Scala库

![](img/4cfa4585f4d5d19d0fff669cf412b206_42.png)

Scala主要用于数据工程，但有时也用于数据科学。以下是Spark的一些补充库：

*   **Vegas**：这是一个用于统计数据可视化的Scala库。使用Vegas，你可以处理数据文件以及Spark数据框。
*   **BigDL**：用于深度学习的库。

![](img/4cfa4585f4d5d19d0fff669cf412b206_44.png)

### R语言库

![](img/4cfa4585f4d5d19d0fff669cf412b206_45.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_46.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_47.png)

R语言内置了机器学习和数据可视化的功能，但也有几个补充库。

![](img/4cfa4585f4d5d19d0fff669cf412b206_48.png)

*   **ggplot2**：是R中一个流行的数据可视化库。
*   你也可以使用能够与Keras和TensorFlow接口的库。

R曾经是开源数据科学的事实标准，但现在正逐渐被Python取代。

![](img/4cfa4585f4d5d19d0fff669cf412b206_50.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_51.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_52.png)

---

![](img/4cfa4585f4d5d19d0fff669cf412b206_53.png)

## 总结

![](img/4cfa4585f4d5d19d0fff669cf412b206_55.png)

![](img/4cfa4585f4d5d19d0fff669cf412b206_56.png)

本节课中，我们一起学习了数据科学中的关键库。我们从Python的科学计算库Pandas和NumPy开始，然后探讨了用于可视化的Matplotlib和Seaborn。接着，我们介绍了机器学习的Scikit-learn以及深度学习的Keras、TensorFlow和PyTorch。最后，我们了解了其他语言中的工具，如Apache Spark、Scala的Vegas和BigDL，以及R语言的ggplot2。掌握这些库将为你进行有效的数据分析和建模奠定坚实的基础。