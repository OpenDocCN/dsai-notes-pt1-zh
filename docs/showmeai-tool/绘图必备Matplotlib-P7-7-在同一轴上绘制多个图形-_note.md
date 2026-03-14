# Matplotlib 绘图教程 P7：在同一坐标轴上绘制多个图形 📊

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_1.png)

在本节课中，我们将学习如何使用 Matplotlib 在同一坐标轴上绘制多个图形。我们将涵盖在同一坐标轴上叠加折线图、散点图和柱状图的方法，以及如何为图形添加标签、图例和突出显示特定数据点。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_3.png)

---

## 1. 基础：在同一坐标轴上绘制多个折线图

上一节我们介绍了如何创建图形和坐标轴。本节中，我们来看看如何在同一坐标轴上绘制多个图形。

首先，我们创建一个图形和一个坐标轴对象：

```python
fig, ax = plt.subplots()
plt.show()
```

接下来，我们可以在同一个坐标轴对象 `ax` 上多次调用绘图方法。例如，我们可以绘制一个抛物线和一个阻尼振荡曲线。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_5.png)

以下是绘制两个图形的步骤：

1.  定义数据。
2.  使用 `ax.plot()` 方法绘制第一个图形。
3.  使用 `ax.plot()` 方法绘制第二个图形。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_7.png)

```python
import numpy as np
import matplotlib.pyplot as plt

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_9.png)

# 创建数据
x = np.linspace(-10, 10, 100)
y_parabola = x**2
y_damped = np.exp(-0.1 * x) * np.sin(x)

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_11.png)

fig, ax = plt.subplots()
# 绘制第一个图形：抛物线
ax.plot(x, y_parabola)
# 绘制第二个图形：阻尼振荡
ax.plot(x, y_damped)
plt.show()
```

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_13.png)

运行上述代码，Matplotlib 会自动为两个图形分配不同的颜色。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_15.png)

---

## 2. 添加标签和图例

当在同一坐标轴上绘制多个图形时，添加标签和图例至关重要，以便区分不同的数据系列。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_17.png)

要为图形添加标签，只需在 `plot()` 方法中传入 `label` 参数。然后，调用 `ax.legend()` 方法来显示图例。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_19.png)

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_21.png)

以下是添加标签和图例的步骤：

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_23.png)

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_25.png)

1.  在 `plot()` 方法中设置 `label` 参数。
2.  调用 `ax.legend()` 方法。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_27.png)

```python
fig, ax = plt.subplots()
# 绘制图形并添加标签
ax.plot(x, y_parabola, label='Parabola (x^2)')
ax.plot(x, y_damped, label='Damped Oscillation')
# 显示图例
ax.legend()
plt.show()
```

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_29.png)

执行代码后，图例会显示在图形上，并自动匹配图形的颜色和线型。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_31.png)

---

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_33.png)

## 3. 应用：按类别着色的散点图

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_35.png)

现在，我们将这个技巧应用到真实数据中。我们将使用一个心脏病数据集，创建一个散点图来探索静息血压与胆固醇之间的关系，并根据患者是否患有心脏病为数据点着色。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_37.png)

假设我们有一个名为 `df` 的 DataFrame，其中包含 `trestbps`（静息血压）、`chol`（胆固醇）和 `target`（是否有心脏病，0表示无，1表示有）等列。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_39.png)

以下是按类别着色散点图的步骤：

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_41.png)

1.  创建图形和坐标轴。
2.  分别筛选出健康患者（`target == 0`）和心脏病患者（`target == 1`）的数据。
3.  为每个子集分别绘制散点图，并指定标签。
4.  添加图例。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_43.png)

```python
fig, ax = plt.subplots()

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_45.png)

# 绘制健康患者的数据
healthy = df[df['target'] == 0]
ax.scatter(healthy['trestbps'], healthy['chol'], label='Healthy')

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_47.png)

# 绘制心脏病患者的数据
disease = df[df['target'] == 1]
ax.scatter(disease['trestbps'], disease['chol'], label='Heart Disease')

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_49.png)

ax.set_xlabel('Resting Blood Pressure')
ax.set_ylabel('Cholesterol')
ax.legend()
plt.show()
```

Matplotlib 会自动为两个系列分配不同的颜色。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_51.png)

---

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_53.png)

## 4. 进阶：使用循环优化代码

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_54.png)

如果类别很多，重复编写绘图代码会显得冗余。我们可以通过循环遍历类别的唯一值来优化代码。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_56.png)

以下是使用循环绘图的步骤：

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_58.png)

1.  获取分类列的唯一值。
2.  循环遍历每个唯一值。
3.  在每次迭代中，筛选出对应类别的数据并进行绘制。
4.  可以使用字典将类别值映射为更具描述性的标签。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_60.png)

```python
fig, ax = plt.subplots()

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_62.png)

# 标签映射字典
label_map = {0: 'Healthy', 1: 'Heart Disease'}

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_64.png)

# 循环遍历每个唯一的类别值
for target_value in df['target'].unique():
    subset = df[df['target'] == target_value]
    ax.scatter(subset['trestbps'], subset['chol'], label=label_map[target_value])

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_66.png)

ax.set_xlabel('Resting Blood Pressure')
ax.set_ylabel('Cholesterol')
ax.legend()
plt.show()
```

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_68.png)

这种方法使代码更简洁，易于扩展。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_69.png)

---

## 5. 突出显示特定数据点

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_71.png)

有时我们需要在图表中突出显示某个特定的数据点。这可以通过在该点上叠加一个不同颜色或大小的新散点图来实现。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_73.png)

以下是突出显示单个数据点的步骤：

1.  首先绘制所有数据点（例如设为灰色）。
2.  然后，单独绘制你想要突出的那个数据点（例如设为红色并增大点的大小）。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_75.png)

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_76.png)

```python
fig, ax = plt.subplots()

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_78.png)

# 绘制所有点为浅灰色
ax.scatter(df['trestbps'], df['chol'], c='lightgrey', label='All Data')

# 突出显示第一个数据点（索引为0）
highlight_point = df.iloc[0]
ax.scatter(highlight_point['trestbps'], highlight_point['chol'], c='red', s=100, label='Highlighted Point')

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_80.png)

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_81.png)

ax.set_xlabel('Resting Blood Pressure')
ax.set_ylabel('Cholesterol')
ax.legend()
plt.show()
```

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_83.png)

参数 `s` 控制点的大小，`c` 控制点的颜色。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_85.png)

---

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_86.png)

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_88.png)

## 6. 组合不同类型的图表

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_90.png)

最后，我们可以在同一坐标轴上组合不同类型的图表，例如将折线图和柱状图结合在一起。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_92.png)

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_94.png)

假设我们想展示不同年龄组的平均胆固醇水平（折线图）和平均静息血压（柱状图）。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_96.png)

以下是组合图表的步骤：

1.  准备分组聚合数据。
2.  使用 `ax.plot()` 绘制折线图。
3.  使用 `ax.bar()` 绘制柱状图。
4.  为两个系列添加标签和图例。

```python
# 假设已按年龄分组并计算了均值
age_grouped = df.groupby('age').mean()

fig, ax = plt.subplots()

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_98.png)

# 绘制折线图：年龄 vs 平均胆固醇
ax.plot(age_grouped.index, age_grouped['chol'], label='Avg Cholesterol', marker='o')

# 绘制柱状图：年龄 vs 平均静息血压
ax.bar(age_grouped.index, age_grouped['trestbps'], label='Avg Resting BP', alpha=0.5, width=0.8)

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_100.png)

ax.set_xlabel('Age')
ax.set_ylabel('Value')
ax.legend()
plt.show()
```

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_102.png)

`alpha` 参数控制柱状图的透明度，`width` 参数控制柱子的宽度，这有助于在组合图表时提高可读性。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_104.png)

---

## 总结

本节课中我们一起学习了如何在 Matplotlib 的同一坐标轴上绘制多个图形。我们掌握了以下核心技能：

*   **多次调用绘图方法**：通过在同一坐标轴对象上多次调用如 `plot()`、`scatter()`、`bar()` 等方法，可以叠加图形。
*   **添加标签和图例**：使用 `label` 参数和 `ax.legend()` 方法来区分不同的数据系列。
*   **按类别着色**：通过筛选数据子集并分别绘制，可以实现按类别对散点图进行着色。
*   **使用循环优化**：遍历类别唯一值可以简化代码，便于处理多类别数据。
*   **突出显示数据点**：通过叠加绘制特定点，可以轻松实现数据点的突出显示。
*   **组合图表类型**：可以在同一坐标轴上自由组合折线图、柱状图等不同类型的图表，以呈现更丰富的信息。

![](img/ba4f2bfdbb4eb6c9346c88ace128100c_106.png)

掌握这些技巧，你将能够创建信息丰富且美观的复合图表。