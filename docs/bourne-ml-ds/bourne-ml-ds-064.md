# 64：数据可视化基础 🎨

![](img/cd9829c86d2fe630c1a325ed55b1fbb7_0.png)

在本节课中，我们将学习数据可视化的核心概念与实践方法。数据可视化是将数据转化为直观图表的过程，其关键在于准确、清晰且有意义的呈现，而不仅仅是制作“好看的”图形。我们将通过一个医院患者数据的案例，学习如何使用可视化工具来帮助理解数据并支持预测分析。

---

上一节我们介绍了数据可视化的基本目标与重要性。本节中，我们来看看实现数据可视化的核心工具与库。

在Python中，`matplotlib` 是一个广泛使用的绘图库。以下是导入该库并设置基本样式的基本代码：

```python
import matplotlib.pyplot as plt
# 设置全局样式，例如使用‘seaborn’风格使图表更美观
plt.style.use('seaborn')
```

---

以下是进行有效数据可视化时需要遵循的几个核心原则：

1.  **准确性**：图表必须真实反映数据，避免扭曲事实。
2.  **清晰性**：图表应易于理解，标签、标题和图例必须明确。
3.  **简洁性**：避免不必要的装饰，突出核心信息。
4.  **一致性**：在同一系列图表中使用统一的配色与样式。

---

掌握了基本原则后，我们进入实践环节。假设我们有一份医院的患者数据集，目标是分析其中心脏疾病的相关指标。

首先，我们需要加载和探索数据。通常，我们会使用 `pandas` 库来处理数据：

![](img/cd9829c86d2fe630c1a325ed55b1fbb7_2.png)

![](img/cd9829c86d2fe630c1a325ed55b1fbb7_3.png)

```python
import pandas as pd
# 加载数据集
data = pd.read_csv('patient_records.csv')
# 查看数据前几行和基本信息
print(data.head())
print(data.info())
```

---

数据准备完成后，便可以开始创建可视化图表。根据客户要求，我们需要创建以蓝色为主的图表。

例如，要绘制患者年龄的分布直方图，可以使用以下代码：

```python
# 绘制年龄分布直方图，并指定蓝色
plt.figure(figsize=(10, 6))
plt.hist(data['age'], bins=15, color='blue', edgecolor='black')
plt.title('患者年龄分布')
plt.xlabel('年龄')
plt.ylabel('人数')
plt.grid(True)
plt.show()
```

---

除了直方图，散点图常用于展示两个变量之间的关系。例如，我们可以分析胆固醇水平与最大心率之间的关系：

```python
# 绘制散点图
plt.figure(figsize=(10, 6))
plt.scatter(data['chol'], data['thalach'], color='blue', alpha=0.5)
plt.title('胆固醇水平 vs 最大心率')
plt.xlabel('胆固醇')
plt.ylabel('最大心率')
plt.grid(True)
plt.show()
```

![](img/cd9829c86d2fe630c1a325ed55b1fbb7_5.png)

---

![](img/cd9829c86d2fe630c1a325ed55b1fbb7_7.png)

在本节课中，我们一起学习了数据可视化的目的、核心原则以及使用 `matplotlib` 库创建基本图表（如直方图和散点图）的实践方法。记住，优秀的数据可视化是连接原始数据与有效洞察的桥梁，它要求我们在追求视觉美观的同时，必须保证表达的准确与清晰。