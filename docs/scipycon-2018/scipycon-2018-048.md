# 48：使用 SciPy 和 scikit-image 进行 Python 图像分析 🖼️

![](img/5f6eab589fce803a1ae2363679a6a5aa_1.png)

在本节课中，我们将学习如何使用 Python 的 SciPy 和 scikit-image 库进行图像分析。课程将从图像的基础表示开始，逐步深入到图像过滤、分割等高级主题，并通过解决实际问题来巩固所学知识。

![](img/5f6eab589fce803a1ae2363679a6a5aa_3.png)

![](img/5f6eab589fce803a1ae2363679a6a5aa_5.png)

## 图像表示：NumPy 数组 📊

图像在 scikit-image 中被表示为 NumPy 数组。我们使用通用的 ND 数组作为容器，这使得 scikit-image 能够与 Python 科学生态系统中的大多数其他包（如 scikit-learn、SciPy 等）互操作。

首先，我们导入必要的库并创建一个随机图像。

```python
import numpy as np
from matplotlib import pyplot as plt

# 创建一个 500x500 的随机图像
random_image = np.random.random((500, 500))
plt.imshow(random_image, cmap='gray')
plt.show()
```

灰度图像是一个二维矩阵。对于彩色图像，我们需要不同的层来表示不同的颜色，通常是红、绿、蓝（RGB）三个通道。

```python
from skimage import data

![](img/5f6eab589fce803a1ae2363679a6a5aa_7.png)

# 加载灰度图像
coins = data.coins()
print(f"类型: {type(coins)}")
print(f"数据类型: {coins.dtype}")
print(f"形状: {coins.shape}")

# 加载彩色图像
cat = data.chelsea()
print(f"彩色图像形状: {cat.shape}")  # 输出: (300, 451, 3)
```

因为图像只是 NumPy 数组，所以你可以像操作任何其他 NumPy 数组一样操作它们，例如切片、赋值或应用归约操作。

```python
# 在图像上绘制一个红色方块
cat[10:110, 10:110] = [255, 0, 0]  # 红色
plt.imshow(cat)
plt.show()
```

图像的值范围可以是 0 到 1（浮点数）或 0 到 255（8位整数）。scikit-image 内部通常使用浮点表示。

```python
# 浮点表示与整数表示
float_gradient = np.linspace(0, 1, 2500).reshape((50, 50))
int_gradient = np.linspace(0, 255, 2500, dtype=np.uint8).reshape((50, 50))

fig, axes = plt.subplots(1, 2)
axes[0].imshow(float_gradient, cmap='gray')
axes[0].set_title('浮点图像')
axes[1].imshow(int_gradient, cmap='gray')
axes[1].set_title('整数图像')
plt.show()
```

使用 `img_as_float` 函数可以确保图像值在 0 到 1 之间，这是典型的工作流程。

```python
from skimage import img_as_float

float_cat = img_as_float(cat)
print(f"转换后范围: [{float_cat.min():.2f}, {float_cat.max():.2f}]")
```

要从磁盘读取图像，可以使用 `io.imread` 函数。要处理多个图像，可以使用 `ImageCollection`，它支持延迟加载以节省内存。

```python
from skimage import io

# 读取单个图像
balloon = io.imread('balloon.jpg')
print(f"气球图像形状: {balloon.shape}")

# 使用 ImageCollection 读取多个图像
from skimage.io import ImageCollection
ic = ImageCollection('*.jpg')
print(f"找到 {len(ic)} 张图像")
```

## 图像过滤：平滑与边缘检测 🔍

上一节我们介绍了图像如何表示为数组，本节中我们来看看如何应用过滤器来处理这些图像。过滤的核心思想是定义一个“核”（kernel），将其在图像上滑动，并在每个位置计算核与对应图像区域的加权和。

首先，我们在一维信号上理解这个概念。

```python
# 创建一个阶跃信号并添加噪声
signal = np.zeros(100)
signal[50:] = 1
noisy_signal = signal + 0.1 * np.random.randn(100)

![](img/5f6eab589fce803a1ae2363679a6a5aa_9.png)

# 使用简单移动平均进行平滑
mean_kernel_3 = np.full(3, 1/3)
smoothed_3 = np.convolve(noisy_signal, mean_kernel_3, mode='valid')
```

NumPy 的 `convolve` 函数在边界处可能产生不理想的效果。SciPy 的 `ndimage.convolve` 提供了更多边界处理模式。

![](img/5f6eab589fce803a1ae2363679a6a5aa_11.png)

```python
from scipy import ndimage

![](img/5f6eab589fce803a1ae2363679a6a5aa_13.png)

# 使用 'reflect' 模式处理边界
smoothed_reflect = ndimage.convolve(noisy_signal, mean_kernel_3, mode='reflect')
```

![](img/5f6eab589fce803a1ae2363679a6a5aa_15.png)

除了均值过滤器，高斯过滤器是更优的平滑选择，它对中心像素赋予更高权重。

![](img/5f6eab589fce803a1ae2363679a6a5aa_17.png)

```python
# 创建一维高斯核
x = np.arange(9)
center = 4
gaussian_kernel_1d = np.exp(-0.5 * (x - center)**2)
gaussian_kernel_1d /= gaussian_kernel_1d.sum()  # 归一化
```

现在，我们将这些概念推广到二维图像。二维均值核是一个所有值均为 1/9 的 3x3 矩阵。

```python
# 二维均值过滤
from skimage import filters

pixelated_camera = data.camera()[::10, ::10]  # 下采样获取像素化图像
mean_kernel_2d = np.ones((3, 3)) / 9
filtered_mean = ndimage.convolve(pixelated_camera, mean_kernel_2d)

# 二维高斯过滤
filtered_gaussian = filters.gaussian(pixelated_camera, sigma=1)
```

边缘检测是另一个重要的过滤操作。我们可以使用差分过滤器来检测图像亮度的变化。

```python
# 垂直边缘检测核
vertical_kernel = np.array([[-1, 0, 1],
                            [-2, 0, 2],
                            [-1, 0, 1]])
vertical_edges = ndimage.convolve(pixelated_camera, vertical_kernel)

# Sobel 边缘检测器是更常用的方法
edges_sobel = filters.sobel(pixelated_camera)
```

中值过滤器是一种非线性过滤器，它在平滑的同时能更好地保留边缘。

![](img/5f6eab589fce803a1ae2363679a6a5aa_19.png)

![](img/5f6eab589fce803a1ae2363679a6a5aa_20.png)

```python
from skimage.filters import median
from skimage.morphology import disk

![](img/5f6eab589fce803a1ae2363679a6a5aa_22.png)

# 应用中值过滤器
filtered_median = median(pixelated_camera, disk(5))
```

## 图像分割：从图像中提取对象 ✂️

在学习了如何过滤图像后，本节我们将探讨如何分割图像，即识别并提取出感兴趣的区域。分割主要分为两类：有监督分割（需要人工输入）和无监督分割。

最简单的分割方法是阈值化，它根据像素强度将图像分为前景和背景。

```python
from skimage import data
text = data.page()

# 查看图像直方图以选择阈值
plt.hist(text.ravel(), bins=256)
plt.show()

# 应用简单阈值
threshold_value = 150
binary_image = text > threshold_value
plt.imshow(binary_image, cmap='gray')
plt.show()
```

scikit-image 提供了多种自动确定阈值的方法，如 Otsu 法。

![](img/5f6eab589fce803a1ae2363679a6a5aa_24.png)

![](img/5f6eab589fce803a1ae2363679a6a5aa_25.png)

![](img/5f6eab589fce803a1ae2363679a6a5aa_27.png)

```python
from skimage.filters import threshold_otsu

thresh_otsu = threshold_otsu(text)
binary_otsu = text > thresh_otsu
```

![](img/5f6eab589fce803a1ae2363679a6a5aa_29.png)

![](img/5f6eab589fce803a1ae2363679a6a5aa_31.png)

对于更复杂的场景，我们需要更高级的方法。活动轮廓（或“蛇”模型）是一种有监督分割方法，它从一个初始轮廓开始，使其收缩并贴合到目标边缘。

![](img/5f6eab589fce803a1ae2363679a6a5aa_33.png)

![](img/5f6eab589fce803a1ae2363679a6a5aa_35.png)

![](img/5f6eab589fce803a1ae2363679a6a5aa_37.png)

```python
from skimage.segmentation import active_contour
from skimage.draw import circle_perimeter

# 准备图像和初始轮廓
astronaut = data.astronaut()
gray_astronaut = rgb2gray(astronaut)
s = np.linspace(0, 2*np.pi, 200)
init = np.array([100 + 100*np.sin(s), 100 + 100*np.cos(s)]).T  # 圆形初始轮廓

# 运行活动轮廓算法
snake = active_contour(gray_astronaut, init, alpha=0.1, beta=1.0)
```

随机游走器是另一种有监督算法，它根据用户提供的种子点标签来分割图像。

```python
from skimage.segmentation import random_walker

# 创建标签数组：1 为面部，2 为背景
labels = np.zeros(gray_astronaut.shape, dtype=np.uint8)
# ... 在 labels 数组中设置种子点 ...

# 运行随机游走器
segmentation = random_walker(gray_astronaut, labels, beta=400)
```

对于无监督分割，SLIC 算法使用 K-means 将图像分割成外观相似的超像素区域。

```python
from skimage.segmentation import slic
from skimage.color import label2rgb

![](img/5f6eab589fce803a1ae2363679a6a5aa_39.png)

# 应用 SLIC
segments_slic = slic(astronaut, n_segments=100, compactness=10)
segments_color = label2rgb(segments_slic, astronaut, kind='avg')
```

![](img/5f6eab589fce803a1ae2363679a6a5aa_41.png)

![](img/5f6eab589fce803a1ae2363679a6a5aa_43.png)

## 实战挑战：解决 Stack Overflow 上的真实问题 🎯

现在，我们将应用所学知识来解决一些来自 Stack Overflow 的真实图像分析问题。

**挑战 1：药片识别**
目标是从背景中分割出药片并计算其面积。一个可行的策略是：均衡化图像以增强对比度，使用 Canny 边缘检测器找到边缘，然后使用 RANSAC 拟合一个圆形模型。

**挑战 2：硬币计数**
目标是从一张照片中自动计数硬币数量。可以尝试：将图像转换为灰度图，应用阈值化得到二值图像，进行形态学操作（如闭运算）以填充孔洞和分离轻微粘连的物体，最后标记并计数连通区域。

**挑战 3：蛇形线端点检测**
目标是在一幅包含多条弯曲、交织的线条图像中，找出每条线的起点和终点。关键思路是：一个端点像素是线条的一部分，但其八邻域中只有一个邻居像素也属于线条。这可以通过卷积运算来高效计算每个像素的“活跃邻居”数量。

**挑战 4：蓝色 M&M 糖果计数**
目标是从一堆 M&M 糖果中数出蓝色糖果的数量。策略可能包括：将图像从 RGB 转换到 HSV 或 Lab 颜色空间以便更好地分离颜色，根据蓝色范围创建掩膜，使用分水岭分割来分离相互接触的糖果，最后对分割后的区域进行计数和颜色分析。

**挑战 5：复杂相变边界分割**
目标是从一张噪声大、对比度低、边界模糊的相变实验图像中，分割出“雪花”状物体的边界。这非常具有挑战性。可能需要结合使用高级去噪算法（如非局部均值去噪）、自适应阈值化以及基于边缘或区域的主动轮廓模型，并需要仔细调整参数。

## 总结 📝

![](img/5f6eab589fce803a1ae2363679a6a5aa_45.png)

本节课中我们一起学习了使用 Python 进行图像分析的核心流程。我们从理解图像作为 NumPy 数组的基本表示开始，掌握了应用各种线性与非线性过滤器进行图像平滑和边缘检测。接着，我们深入探讨了图像分割的世界，区分了有监督与无监督方法，并实践了阈值化、活动轮廓、随机游走器等关键算法。最后，我们通过解决一系列来自真实世界的 Stack Overflow 挑战，将理论知识应用于实践。希望本教程为你开启了使用 scikit-image 进行图像处理的大门，并鼓励你探索其丰富的文档和示例库以解决更多问题。