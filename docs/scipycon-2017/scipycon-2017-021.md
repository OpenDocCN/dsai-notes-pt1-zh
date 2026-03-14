# 21：使用 HoloViews 与 Bokeh 进行交互式数据可视化 🎨

在本节课中，我们将学习如何使用 HoloViews 和 Bokeh 这两个强大的 Python 库来创建交互式数据可视化应用。HoloViews 提供了一个高级、语义化的接口，让你专注于数据本身的意义，而不是繁琐的绘图细节。Bokeh 则负责将这些语义对象渲染成美观、可交互的图表。我们将从基础概念开始，逐步构建一个复杂的交互式仪表板。

---

## 环境设置与介绍 🛠️

首先，我们需要确保开发环境已正确设置。本教程基于一个特定的 Git 代码库和 Conda 环境。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_1.png)

以下是设置步骤：

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_3.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_5.png)

*   克隆指定的 Git 代码库。
*   使用提供的 YAML 文件创建 Conda 环境：`conda env create -f environment.yml`。
*   进入环境后，更新到最新版本的库（例如 `taulivis 1.8.1`）。
*   进入 `notebooks` 目录，使用命令 `jupyter notebook --NotebookApp.iopub_data_rate_limit=1.0e10` 启动 Jupyter Notebook，该参数用于提高数据传输限制，以便处理大型数据集。
*   打开 `00_welcome.ipynb` 笔记本文件开始学习。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_7.png)

如果在设置过程中遇到问题，可以通过课程提供的 Slack 频道寻求帮助。

---

## 理解 HoloViews 核心：元素 📦

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_9.png)

上一节我们完成了环境设置，本节中我们来看看 HoloViews 的核心抽象——元素。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_11.png)

元素是数据的容器，它们代表你的数据并拥有丰富的视觉表示，但其设计初衷是让你关注数据的语义信息（如“距离”、“高度”），而非视觉样式（如线条粗细、颜色）。你可以将元素视为带有可视化能力的“数据对象”。

HoloViews 提供了多种元素类型，例如 `Curve`（曲线）、`Scatter`（散点图）、`Area`（面积图）和 `Image`（图像）。你可以在 HoloViews 官网的参考图库中查看所有可用的元素类型。

### 创建第一个元素

让我们从一个简单的 `Curve` 元素开始。

```python
import holoviews as hv
hv.extension('bokeh') # 指定使用 Bokeh 进行渲染

# 创建数据
x_values = [1, 2, 3, 4, 5]
y_values = [1, 4, 9, 16, 25]

# 创建 Curve 元素
simple_curve = hv.Curve((x_values, y_values))
simple_curve
```

运行上述代码，你将看到一个用 Bokeh 渲染的抛物线图。你可以使用 Bokeh 的工具进行缩放、平移等交互操作。

**元素对象**不仅具有可视化表示，还有文本表示。打印 `simple_curve` 对象可以看到其结构。通过 `.data` 属性可以访问元素内部的数据，对于上面的列表数据，它通常会被转换为 Pandas DataFrame。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_13.png)

### 探索不同元素类型

以下是几种相似但视觉表现不同的元素：

*   `hv.Curve((x, y))`：绘制连接数据点的曲线。
*   `hv.Area((x, y))`：绘制曲线下方的填充区域。
*   `hv.Scatter((x, y))`：绘制独立的散点。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_15.png)

你可以尝试将上面代码中的 `hv.Curve` 替换为 `hv.Area` 或 `hv.Scatter`，观察其变化。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_17.png)

### 为数据添加语义信息

目前我们的数据只是简单的数字列表。我们可以为其添加维度标签，以说明数据的实际意义。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_19.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_21.png)

```python
# 添加关键维度 (kdims) 和值维度 (vdims) 标签
trajectory_curve = hv.Curve((x_values, y_values), kdims='distance', vdims='height')
trajectory_curve
```

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_22.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_24.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_26.png)

现在，图表的坐标轴标签变成了“distance”和“height”。`kdims` 通常对应自变量（如X轴），`vdims` 对应因变量（如Y轴）。这些维度信息被封装在丰富的 `Dimension` 对象中。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_28.png)

### 元素间的转换

由于元素关注数据语义，因此在数据结构相似的元素间可以轻松转换。

```python
# 将 Curve 转换为 Scatter
trajectory_scatter = hv.Scatter(trajectory_curve)
trajectory_scatter
```

转换后，新的 `Scatter` 元素会保留原 `Curve` 元素的所有维度标签信息。

---

## 使用不同数据格式 📊

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_30.png)

上一节我们使用列表创建了元素，本节中我们来看看 HoloViews 如何支持 NumPy 数组和 Pandas DataFrame。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_32.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_34.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_36.png)

### 使用 NumPy 数组

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_38.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_40.png)

对于二维数组，对应的元素是 `Image`。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_42.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_44.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_46.png)

```python
import numpy as np

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_48.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_50.png)

# 创建一个二维数组（例如，正弦余弦组合的图案）
x = np.linspace(0, 10, 200)
y = np.linspace(0, 10, 200)
X, Y = np.meshgrid(x, y)
array_data = np.sin(X) * np.cos(Y)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_52.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_54.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_56.png)

# 创建 Image 元素
simple_image = hv.Image(array_data)
simple_image
```

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_58.png)

`Image` 元素将数组值映射为颜色，直观展示数据结构。你可以尝试修改数组生成函数（例如 `np.sin(X)**2 * np.cos(Y)`），创建不同的图案。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_60.png)

和 `Curve` 一样，你也可以为 `Image` 添加维度标签。`Image` 有两个 `kdims`（用于索引数组的行和列）和一个 `vdim`（数组值本身）。

```python
labeled_image = hv.Image(array_data, kdims=['longitude', 'latitude'])
labeled_image
```

### 使用 Pandas DataFrame

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_62.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_64.png)

当数据已存在于 DataFrame 中时，可以直接将其传入元素，并通过列名指定维度。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_66.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_68.png)

```python
import pandas as pd

# 假设 `economics_df` 是一个包含‘year’， ‘growth’， ‘unemp’等列的DataFrame
# 选取美国的数据
us_data = economics_df[economics_df['country'] == 'United States']

# 绘制 GDP 增长曲线
gdp_growth_curve = hv.Curve(us_data, kdims='year', vdims='growth')
gdp_growth_curve
```

通过更改 `vdims` 参数，可以轻松绘制不同的指标，例如失业率：

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_70.png)

```python
unemp_curve = hv.Curve(us_data, kdims='year', vdims='unemp')
unemp_curve
```

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_72.png)

### 完善维度标签

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_74.png)

为了生成更专业的图表，我们需要更详细的轴标签和单位信息。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_76.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_78.png)

```python
# 为维度设置更友好的标签和单位
gdp_growth_labeled = gdp_growth_curve.redim.label(growth='GDP Growth').redim.unit(growth='%')
gdp_growth_labeled
```

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_80.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_82.png)

`redim.label` 方法用于修改维度的显示名称，`redim.unit` 用于添加单位。这些信息会体现在最终的图表轴上。

---

## 组合与布局元素 🧩

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_84.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_86.png)

在数据分析中，我们经常需要并排比较多个图表。HoloViews 使用 `+` 和 `*` 运算符来组合元素。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_88.png)

### 使用 Layout（`+` 运算符）

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_90.png)

`+` 运算符将元素排列在**不同的子图**中，创建一个布局。

```python
# 创建同一个数据的不同视图
curve_view = hv.Curve(trajectory_curve)
scatter_view = hv.Scatter(trajectory_curve)
area_view = hv.Area(trajectory_curve)
spikes_view = hv.Spikes(trajectory_curve)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_92.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_94.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_96.png)

# 使用 + 创建布局
layout = curve_view + scatter_view + area_view + spikes_view
layout
```

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_98.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_100.png)

默认情况下，共享相同维度的子图会**链接其交互**（如缩放、平移），这是一个有用的特性。布局对象可以通过属性或字典方式访问其子元素（例如 `layout.Curve_I` 或 `layout[‘Curve’, ‘I’]`）。

### 为元素添加组和标签

为了更好地组织布局中的元素，可以为每个元素设置 `group` 和 `label`。

```python
# 为元素设置组和标签
cannonball = trajectory_curve.relabel(group='Trajectory', label='Cannonball')
filled = hv.Area(trajectory_curve).relabel(group='Trajectory', label='Filled')

labeled_layout = cannonball + filled
labeled_layout
```

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_102.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_104.png)

设置后，布局中每个子图的标题会显示这些信息，并且在访问子图时也可以使用这些标签（例如 `labeled_layout.Trajectory.Filled`）。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_106.png)

### 使用 Overlay（`*` 运算符）

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_108.png)

`*` 运算符将元素叠加在**同一个坐标系**中，创建一个覆盖层。

```python
# 在同一坐标系中叠加曲线和钉状图
overlay = trajectory_curve * hv.Spikes(trajectory_curve)
overlay
```

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_110.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_112.png)

覆盖层适用于需要同时显示多个数据序列的场景，并且会自动处理颜色循环和图例。和布局一样，覆盖层也支持通过组和标签进行索引。

### 克隆元素以创建变体

`clone` 方法可以基于现有元素创建一个新实例，并允许修改其属性（如数据或标签），而不影响原元素。

```python
# 克隆一个元素并修改其标签
tennis_ball = cannonball.clone(label='Tennisball')
comparison_overlay = cannonball * tennis_ball
comparison_overlay
```

你可以尝试使用 `clone` 方法修改数据（例如将 y 值减半以模拟“手抛球”轨迹），并将其添加到覆盖层中进行比较。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_114.png)

---

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_116.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_118.png)

## 总结 📝

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_120.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_122.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_124.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_126.png)

本节课我们一起学习了使用 HoloViews 和 Bokeh 进行交互式数据可视化的基础。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_128.png)

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_130.png)

我们首先了解了 **HoloViews 元素** 作为语义化数据容器的核心概念，学会了如何创建 `Curve`、`Scatter`、`Image` 等基本元素，并为它们添加有意义的维度标签和单位。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_132.png)

接着，我们探索了如何将 **NumPy 数组** 和 **Pandas DataFrame** 这两种常见数据格式无缝转换为可视化元素。

最后，我们掌握了组合元素的强大技巧：使用 `+` 运算符创建并排的 **布局**，以及使用 `*` 运算符创建叠加的 **覆盖层**。我们还学习了通过 `relabel` 和 `clone` 方法来更好地组织和区分不同的数据视图。

![](img/e8f896ce5e8eb9365dbe25b3a1671f94_134.png)

通过这些基础构建模块，你已经可以开始创建结构清晰、富有表现力的静态和交互式图表。在接下来的课程中，我们将深入探讨如何定制图表样式、处理更大规模的数据集以及构建完整的交互式应用程序。