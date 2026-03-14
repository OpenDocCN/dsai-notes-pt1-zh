# 9：Matplotlib 剖析 📊

在本节课中，我们将学习 Matplotlib 库的基础知识。Matplotlib 是一个用于创建高质量图形的 Python 库，广泛应用于科学计算和数据可视化领域。我们将从基本概念开始，逐步深入到更高级的功能。

---

## 🏛️ 历史背景与设计哲学

Matplotlib 始于 2001 年，由 John Hunter 创建。他当时对 Matlab 感到不满，于是决定自己编写一个库。Matplotlib 的设计初衷是模仿 Matlab 的 API，以便科学家们能够轻松上手。它能够生成像素完美的图像，适用于学术出版物。

Matplotlib 的成功部分归功于其跨平台兼容性。它支持 Windows、Linux 和 Mac 操作系统，并且可以与多种 GUI 工具包（如 WX、GTK、TK、QT）协同工作。文本渲染也是 Matplotlib 的一个重要特性，确保图形在出版物中看起来完美无缺。

然而，Matplotlib 也携带了一些历史包袱，这意味着它有一些独特的特性。一旦你理解了这些特性，它们就会变得合情合理。本教程旨在帮助你理解这些特性。

---

## 📚 学习资源

Matplotlib 提供了丰富的文档资源，包括常见问题解答、示例和直接的 API 文档。其中，图库（Gallery）是最重要的资源之一。图库中包含了许多预制的示例，涵盖了 Matplotlib 的各种功能。你可以浏览这些图像，找到你需要的功能，然后查看生成该图像的源代码。

![](img/e4958744496fcbb23b7c948eb7b9c702_1.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_3.png)

此外，你还可以通过邮件列表、Stack Overflow 和 Gitter 等渠道获取帮助。Matplotlib 社区非常友好，欢迎任何问题。如果你有改进 Matplotlib 的想法，可以提交 Matplotlib 增强提案（MEP）。

![](img/e4958744496fcbb23b7c948eb7b9c702_5.png)

---

![](img/e4958744496fcbb23b7c948eb7b9c702_7.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_9.png)

## 🔧 后端与配置

![](img/e4958744496fcbb23b7c948eb7b9c702_11.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_13.png)

Matplotlib 使用不同的后端来适应不同的平台。后端是 Matplotlib 与不同 GUI 工具包交互的组件。大多数情况下，你不需要关心后端，但在报告问题时，可能需要提供后端信息。

![](img/e4958744496fcbb23b7c948eb7b9c702_15.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_17.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_19.png)

你可以通过以下代码查看 Matplotlib 的版本和当前使用的后端：

![](img/e4958744496fcbb23b7c948eb7b9c702_21.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_23.png)

```python
import matplotlib
print(matplotlib.__version__)
print(matplotlib.get_backend())
```

在 Jupyter Notebook 中，你可以使用 `%matplotlib notebook` 来启用交互式绘图。本教程将使用 `nbagg` 后端，以模拟编写 Python 脚本时的行为。

![](img/e4958744496fcbb23b7c948eb7b9c702_25.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_27.png)

---

## 🖼️ 图形结构：Figure 与 Axes

在 Matplotlib 中，图形的基本结构包括 Figure（图形窗口）和 Axes（坐标轴）。Figure 是顶层的容器，而 Axes 是实际绘制图形的区域。

![](img/e4958744496fcbb23b7c948eb7b9c702_29.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_31.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_33.png)

以下是创建 Figure 和 Axes 的基本步骤：

![](img/e4958744496fcbb23b7c948eb7b9c702_35.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_37.png)

```python
import matplotlib.pyplot as plt

![](img/e4958744496fcbb23b7c948eb7b9c702_39.png)

# 创建 Figure
fig = plt.figure()

# 创建 Axes
ax = fig.add_subplot(111)

![](img/e4958744496fcbb23b7c948eb7b9c702_41.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_43.png)

# 显示图形
plt.show()
```

![](img/e4958744496fcbb23b7c948eb7b9c702_45.png)

Figure 可以包含多个 Axes，每个 Axes 可以独立绘制图形。你可以通过 `add_subplot` 方法添加多个子图。

---

## 📈 基本绘图：plot 与 scatter

Matplotlib 提供了多种绘图函数，其中最常用的是 `plot` 和 `scatter`。`plot` 用于绘制线图，而 `scatter` 用于绘制散点图。

以下是使用 `plot` 和 `scatter` 的示例：

```python
import numpy as np
import matplotlib.pyplot as plt

# 创建数据
x = np.linspace(0, 10, 100)
y = np.sin(x)

![](img/e4958744496fcbb23b7c948eb7b9c702_47.png)

# 创建 Figure 和 Axes
fig, ax = plt.subplots()

# 绘制线图
ax.plot(x, y, color='blue', linewidth=2)

# 绘制散点图
ax.scatter(x, y, color='red', marker='o')

![](img/e4958744496fcbb23b7c948eb7b9c702_49.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_51.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_53.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_55.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_56.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_57.png)

# 设置坐标轴范围
ax.set_xlim(0, 10)
ax.set_ylim(-1, 1)

![](img/e4958744496fcbb23b7c948eb7b9c702_59.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_61.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_63.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_65.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_67.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_69.png)

# 显示图形
plt.show()
```

![](img/e4958744496fcbb23b7c948eb7b9c702_71.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_72.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_74.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_76.png)

需要注意的是，`scatter` 函数的主要用途是根据数据点的大小或颜色进行可视化，而不仅仅是绘制不带线的标记。如果你只需要绘制不带线的标记，可以使用 `plot` 函数并指定 `linestyle='none'`。

![](img/e4958744496fcbb23b7c948eb7b9c702_78.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_79.png)

---

![](img/e4958744496fcbb23b7c948eb7b9c702_81.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_83.png)

## 🧩 多子图布局

![](img/e4958744496fcbb23b7c948eb7b9c702_85.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_87.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_88.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_90.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_92.png)

Matplotlib 支持在单个 Figure 中创建多个子图。你可以使用 `subplots` 函数轻松创建网格布局的子图。

![](img/e4958744496fcbb23b7c948eb7b9c702_94.png)

以下是创建多个子图的示例：

![](img/e4958744496fcbb23b7c948eb7b9c702_96.png)

```python
import numpy as np
import matplotlib.pyplot as plt

![](img/e4958744496fcbb23b7c948eb7b9c702_98.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_100.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_102.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_104.png)

# 创建 2x2 的子图网格
fig, axes = plt.subplots(2, 2)

# 在每个子图中绘制不同的图形
axes[0, 0].plot(np.random.rand(10))
axes[0, 0].set_title('Subplot 1')

axes[0, 1].scatter(np.random.rand(10), np.random.rand(10))
axes[0, 1].set_title('Subplot 2')

axes[1, 0].bar(range(5), np.random.rand(5))
axes[1, 0].set_title('Subplot 3')

![](img/e4958744496fcbb23b7c948eb7b9c702_106.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_107.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_109.png)

axes[1, 1].hist(np.random.randn(100), bins=20)
axes[1, 1].set_title('Subplot 4')

![](img/e4958744496fcbb23b7c948eb7b9c702_111.png)

# 调整布局
plt.tight_layout()
plt.show()
```

通过 `subplots` 函数，你可以轻松创建复杂的图形布局。

![](img/e4958744496fcbb23b7c948eb7b9c702_113.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_115.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_117.png)

---

## 🎨 颜色、标记和线型

![](img/e4958744496fcbb23b7c948eb7b9c702_119.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_121.png)

在 Matplotlib 中，你可以通过多种方式指定颜色、标记和线型。颜色可以通过名称、十六进制值或 RGB 元组来指定。标记和线型也有多种预定义选项。

![](img/e4958744496fcbb23b7c948eb7b9c702_123.png)

以下是颜色、标记和线型的示例：

```python
import matplotlib.pyplot as plt

![](img/e4958744496fcbb23b7c948eb7b9c702_125.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_127.png)

# 创建数据
x = [1, 2, 3, 4, 5]
y = [1, 4, 9, 16, 25]

![](img/e4958744496fcbb23b7c948eb7b9c702_129.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_131.png)

# 绘制带有不同颜色、标记和线型的图形
plt.plot(x, y, color='red', marker='o', linestyle='--', linewidth=2, label='Data')
plt.xlabel('X Axis')
plt.ylabel('Y Axis')
plt.title('Sample Plot')
plt.legend()
plt.show()
```

你还可以使用简写形式来指定颜色、标记和线型。例如，`'ro--'` 表示红色、圆形标记和虚线。

---

## 🌈 色彩映射

![](img/e4958744496fcbb23b7c948eb7b9c702_133.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_135.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_137.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_139.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_141.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_142.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_144.png)

色彩映射（Colormap）用于将数据值映射到颜色。Matplotlib 提供了多种预定义的色彩映射，包括顺序色彩映射、发散色彩映射和分类色彩映射。

![](img/e4958744496fcbb23b7c948eb7b9c702_145.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_147.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_149.png)

以下是使用色彩映射的示例：

![](img/e4958744496fcbb23b7c948eb7b9c702_151.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_153.png)

```python
import numpy as np
import matplotlib.pyplot as plt

# 创建数据
data = np.random.rand(10, 10)

![](img/e4958744496fcbb23b7c948eb7b9c702_155.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_157.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_159.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_161.png)

# 绘制热图
plt.imshow(data, cmap='viridis')
plt.colorbar()
plt.show()
```

![](img/e4958744496fcbb23b7c948eb7b9c702_163.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_165.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_167.png)

在选择色彩映射时，应注意其适用性。例如，`jet` 色彩映射可能导致误导性的可视化结果，而 `viridis` 色彩映射在感知上更加均匀。

---

![](img/e4958744496fcbb23b7c948eb7b9c702_169.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_171.png)

## 📝 数学文本与 LaTeX

![](img/e4958744496fcbb23b7c948eb7b9c702_173.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_175.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_177.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_179.png)

Matplotlib 支持使用 LaTeX 语法渲染数学文本。你可以通过 `rcParams` 配置 LaTeX 支持，并在文本中使用数学表达式。

![](img/e4958744496fcbb23b7c948eb7b9c702_181.png)

以下是使用数学文本的示例：

```python
import matplotlib.pyplot as plt

# 启用 LaTeX 渲染
plt.rcParams['text.usetex'] = True

![](img/e4958744496fcbb23b7c948eb7b9c702_183.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_184.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_185.png)

# 绘制图形并添加数学文本
plt.plot([1, 2, 3], [1, 4, 9])
plt.xlabel(r'$x$')
plt.ylabel(r'$y = x^2$')
plt.title(r'$\sigma_i = 15$')
plt.show()
```

如果未安装 LaTeX，Matplotlib 会使用内置的数学文本渲染器。

---

![](img/e4958744496fcbb23b7c948eb7b9c702_187.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_189.png)

## 🧱 填充与阴影

![](img/e4958744496fcbb23b7c948eb7b9c702_191.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_193.png)

Matplotlib 提供了填充和阴影功能，用于增强图形的可视化效果。你可以使用 `fill_between` 函数填充区域，或使用阴影样式区分不同的数据系列。

以下是填充和阴影的示例：

![](img/e4958744496fcbb23b7c948eb7b9c702_195.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_197.png)

```python
import numpy as np
import matplotlib.pyplot as plt

# 创建数据
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)

# 填充区域
plt.fill_between(x, y1, y2, color='gray', alpha=0.5, label='Fill')
plt.plot(x, y1, label='Sin')
plt.plot(x, y2, label='Cos')
plt.legend()
plt.show()
```

![](img/e4958744496fcbb23b7c948eb7b9c702_199.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_201.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_203.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_205.png)

---

![](img/e4958744496fcbb23b7c948eb7b9c702_207.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_209.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_211.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_213.png)

## 🔄 属性循环

![](img/e4958744496fcbb23b7c948eb7b9c702_215.png)

属性循环允许你在绘制多个图形时自动循环不同的属性（如颜色、线型、标记）。你可以通过 `cycler` 对象自定义属性循环。

以下是属性循环的示例：

```python
import matplotlib.pyplot as plt
from cycler import cycler

![](img/e4958744496fcbb23b7c948eb7b9c702_217.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_219.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_221.png)

# 自定义属性循环
custom_cycler = cycler(color=['r', 'g', 'b']) * cycler(linestyle=['-', '--', ':'])
plt.rcParams['axes.prop_cycle'] = custom_cycler

![](img/e4958744496fcbb23b7c948eb7b9c702_223.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_225.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_227.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_229.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_231.png)

# 绘制多个图形
for i in range(5):
    plt.plot([i, i+1, i+2], label=f'Line {i+1}')

![](img/e4958744496fcbb23b7c948eb7b9c702_233.png)

plt.legend()
plt.show()
```

---

## 🎭 艺术家对象

在 Matplotlib 中，所有可见的元素都是艺术家对象。艺术家对象分为容器类型（如 Figure、Axes）和原始类型（如 Line2D、Rectangle）。你可以直接操作艺术家对象来自定义图形。

以下是操作艺术家对象的示例：

```python
import matplotlib.pyplot as plt

![](img/e4958744496fcbb23b7c948eb7b9c702_235.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_237.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_239.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_241.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_243.png)

# 创建图形
fig, ax = plt.subplots()
line, = ax.plot([1, 2, 3], [1, 4, 9])

![](img/e4958744496fcbb23b7c948eb7b9c702_245.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_247.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_249.png)

# 修改线条属性
line.set_color('red')
line.set_linewidth(3)
line.set_linestyle('--')

![](img/e4958744496fcbb23b7c948eb7b9c702_251.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_252.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_254.png)

plt.show()
```

![](img/e4958744496fcbb23b7c948eb7b9c702_256.png)

---

![](img/e4958744496fcbb23b7c948eb7b9c702_258.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_260.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_262.png)

## 🗺️ 扩展工具包

![](img/e4958744496fcbb23b7c948eb7b9c702_264.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_266.png)

Matplotlib 提供了一些扩展工具包，用于处理特定的可视化需求。例如：
- **Basemap**：用于绘制地图（已弃用，推荐使用 Cartopy）。
- **mplot3d**：用于绘制 3D 图形。
- **AxesGrid1**：用于创建复杂的图形布局。

![](img/e4958744496fcbb23b7c948eb7b9c702_268.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_270.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_272.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_274.png)

以下是使用 mplot3d 绘制 3D 图形的示例：

![](img/e4958744496fcbb23b7c948eb7b9c702_276.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_278.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_280.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_282.png)

```python
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import numpy as np

# 创建 3D 图形
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

# 生成数据
x = np.linspace(-5, 5, 100)
y = np.linspace(-5, 5, 100)
x, y = np.meshgrid(x, y)
z = np.sin(np.sqrt(x**2 + y**2))

# 绘制曲面
ax.plot_surface(x, y, z, cmap='viridis')
plt.show()
```

---

![](img/e4958744496fcbb23b7c948eb7b9c702_284.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_286.png)

## 📦 总结

![](img/e4958744496fcbb23b7c948eb7b9c702_288.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_290.png)

在本节课中，我们一起学习了 Matplotlib 的基础知识。我们从 Matplotlib 的历史背景和设计哲学开始，逐步介绍了图形结构、基本绘图函数、多子图布局、颜色与样式、色彩映射、数学文本、填充与阴影、属性循环、艺术家对象以及扩展工具包。通过本教程，你应该能够使用 Matplotlib 创建高质量的科学图形，并进一步探索其高级功能。

![](img/e4958744496fcbb23b7c948eb7b9c702_292.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_294.png)

![](img/e4958744496fcbb23b7c948eb7b9c702_296.png)

希望本教程对你有所帮助，祝你在数据可视化的道路上越走越远！