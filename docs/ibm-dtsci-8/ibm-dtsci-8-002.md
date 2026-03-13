# 002：Matplotlib入门

![](img/152b6e411a354a7b52bcf07a5f13d533_0.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_2.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_4.png)

在本节课中，我们将开始学习Matplotlib。我们将了解Matplotlib的历史背景及其核心架构，并通过实例理解其不同层次的工作原理。

![](img/152b6e411a354a7b52bcf07a5f13d533_6.png)

## 🏛️ Matplotlib的历史与起源

![](img/152b6e411a354a7b52bcf07a5f13d533_8.png)

Matplotlib是Python中最广泛使用的数据可视化库之一，即使不是最流行的，也位居前列。

![](img/152b6e411a354a7b52bcf07a5f13d533_9.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_10.png)

它由约翰·亨特创建。约翰·亨特是一位神经生物学家，当时他所在的研究团队正在分析脑皮层电图信号。

![](img/152b6e411a354a7b52bcf07a5f13d533_12.png)

团队当时使用一款专有软件进行分析。然而，他们只有一个软件许可证，因此需要轮流使用。

![](img/152b6e411a354a7b52bcf07a5f13d533_14.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_15.png)

为了克服这个限制，约翰着手开发一个基于MATLAB的替代版本，以便他和他的团队成员都能使用，并且可以被其他研究者扩展。

因此，Matplotlib最初是作为一个脑皮层电图可视化工具而开发的。与MATLAB类似，Matplotlib配备了一个脚本接口，用于通过Pyplot快速、轻松地生成图形。

![](img/152b6e411a354a7b52bcf07a5f13d533_17.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_18.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_19.png)

## 🧱 Matplotlib的三层架构

![](img/152b6e411a354a7b52bcf07a5f13d533_20.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_22.png)

Matplotlib的架构由三个主要层次构成。

![](img/152b6e411a354a7b52bcf07a5f13d533_24.png)

*   **后端层**
*   **艺术家层**：大部分繁重工作在此层完成。当编写Web应用服务器、UI应用程序或需要与其他开发者共享的脚本时，通常使用此编程范式。
*   **脚本层**：这是适合日常使用的层次，被视为一个更轻量的脚本接口，用于简化常见任务，并快速、轻松地生成图形和图表。

![](img/152b6e411a354a7b52bcf07a5f13d533_26.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_27.png)

接下来，让我们更详细地了解每一层。

![](img/152b6e411a354a7b52bcf07a5f13d533_28.png)

### 后端层

![](img/152b6e411a354a7b52bcf07a5f13d533_30.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_31.png)

后端层包含三个内置的抽象接口类。

![](img/152b6e411a354a7b52bcf07a5f13d533_33.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_35.png)

以下是后端层的核心组件：
*   **图形画布**：定义并包含了绘制图形的区域。
*   **渲染器**：渲染器类的实例知道如何在图形画布上绘制。
*   **事件**：处理用户输入，如键盘敲击和鼠标点击。

![](img/152b6e411a354a7b52bcf07a5f13d533_37.png)

### 艺术家层

![](img/152b6e411a354a7b52bcf07a5f13d533_39.png)

艺术家层由一个主要对象构成，即**艺术家**。艺术家是知道如何获取渲染器并使用它在画布上“着墨”的对象。你在Matplotlib图形中看到的一切都是一个艺术家实例。标题、线条、刻度标签、图像等等，都对应着独立的艺术家。

![](img/152b6e411a354a7b52bcf07a5f13d533_41.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_42.png)

艺术家对象有两种类型。

以下是艺术家对象的两种类型：
*   **基本类型**：例如线条、矩形、圆形或文本。
*   **复合类型**：例如图形或坐标轴。

![](img/152b6e411a354a7b52bcf07a5f13d533_44.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_45.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_46.png)

顶层的Matplotlib对象是**图形艺术家**，它包含并管理给定图形中的所有元素。最重要的复合艺术家是**坐标轴**，因为大多数Matplotlib API的绘图方法都在此定义，包括创建和操作刻度、轴线、网格或绘图背景的方法。

![](img/152b6e411a354a7b52bcf07a5f13d533_47.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_49.png)

需要注意的是，每个复合艺术家可以包含其他复合艺术家以及基本艺术家。例如，一个图形艺术家可以包含一个坐标轴艺术家，以及矩形或文本艺术家。

![](img/152b6e411a354a7b52bcf07a5f13d533_51.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_52.png)

现在，让我们使用艺术家层，看看如何用它生成一个图形。

我们将尝试使用艺术家层生成一个包含10,000个随机数的直方图。

首先，我们从后端导入图形画布，并将图形艺术家附加到它上面。请注意，`Agg`代表反图形几何库，这是一个能生成高质量图像的高性能库。

![](img/152b6e411a354a7b52bcf07a5f13d533_54.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_55.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_56.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_57.png)

```python
from matplotlib.backends.backend_agg import FigureCanvasAgg as FigureCanvas
from matplotlib.figure import Figure
```

![](img/152b6e411a354a7b52bcf07a5f13d533_58.png)

然后，我们导入NumPy库来生成随机数。

![](img/152b6e411a354a7b52bcf07a5f13d533_60.png)

```python
import numpy as np
```

![](img/152b6e411a354a7b52bcf07a5f13d533_62.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_63.png)

接下来，我们创建一个坐标轴艺术家。坐标轴艺术家会自动添加到图形容器的坐标轴列表中。

![](img/152b6e411a354a7b52bcf07a5f13d533_65.png)

```python
fig = Figure()
canvas = FigureCanvas(fig)
ax = fig.add_subplot(111)
```

![](img/152b6e411a354a7b52bcf07a5f13d533_67.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_68.png)

请注意，这里的`111`遵循MATLAB的惯例，它创建一个一行一列的网格，并使用该网格中的第一个单元格作为新坐标轴的位置。

![](img/152b6e411a354a7b52bcf07a5f13d533_69.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_70.png)

然后，我们调用坐标轴的`hist`方法来生成直方图。`hist`方法为每个直方图条创建一系列矩形艺术家，并将它们添加到坐标轴容器中。这里的`100`表示创建100个数据箱。

![](img/152b6e411a354a7b52bcf07a5f13d533_72.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_73.png)

```python
x = np.random.randn(10000)
ax.hist(x, 100)
```

![](img/152b6e411a354a7b52bcf07a5f13d533_75.png)

最后，我们为图形添加标题装饰并保存它。

![](img/152b6e411a354a7b52bcf07a5f13d533_76.png)

```python
ax.set_title('Normal distribution with $\mu=0, \sigma=1$')
fig.savefig('matplotlib_histogram.png')
```

![](img/152b6e411a354a7b52bcf07a5f13d533_78.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_79.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_80.png)

这就是生成的直方图。以上就是我们如何使用艺术家层来生成图形。

![](img/152b6e411a354a7b52bcf07a5f13d533_82.png)

### 脚本层

![](img/152b6e411a354a7b52bcf07a5f13d533_84.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_85.png)

脚本层是为非专业程序员的科学家们开发的。我相信，基于我们刚刚创建的直方图代码，你会同意我的看法：艺术家层的语法较为繁琐，因为它面向开发者，而不是那些目标是对某些数据进行快速探索性分析的个人。

Matplotlib的脚本层本质上是Matplotlib的Pyplot接口，它自动化了定义画布、定义图形艺术家实例以及连接它们的过程。

![](img/152b6e411a354a7b52bcf07a5f13d533_87.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_88.png)

让我们看看之前使用艺术家层生成10,000个随机数直方图的代码，现在使用脚本层会是什么样子。

首先，我们导入Pyplot接口。你可以看到，所有与创建直方图、其他艺术家对象以及操作它们相关的方法，无论是`hist`方法还是显示图形，都是Pyplot接口的一部分。

![](img/152b6e411a354a7b52bcf07a5f13d533_90.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_91.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_92.png)

```python
import matplotlib.pyplot as plt
import numpy as np

![](img/152b6e411a354a7b52bcf07a5f13d533_94.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_95.png)

x = np.random.randn(10000)
plt.hist(x, 100)
plt.title('Normal distribution with $\mu=0, \sigma=1$')
plt.show()
```

## 📚 延伸阅读

![](img/152b6e411a354a7b52bcf07a5f13d533_97.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_98.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_99.png)

如果你有兴趣了解更多关于Matplotlib历史及其架构的信息，这个链接将带你到由Matplotlib创建者亲自撰写的一章内容。这绝对是一篇值得推荐的阅读材料。

## 🎯 课程总结

![](img/152b6e411a354a7b52bcf07a5f13d533_101.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_102.png)

![](img/152b6e411a354a7b52bcf07a5f13d533_104.png)

在本节课中，我们一起学习了Matplotlib的起源、其三层架构以及各层的基本概念。我们了解到，艺术家层提供了底层的精细控制，而脚本层则通过Pyplot接口提供了更简洁、更易用的方式来进行日常数据可视化。理解这些层次将帮助我们更有效地使用Matplotlib来创建各种图表。