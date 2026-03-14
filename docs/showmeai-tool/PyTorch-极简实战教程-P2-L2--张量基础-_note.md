# PyTorch 极简实战教程！P2：L2- 张量基础 🧮

在本节课中，我们将学习PyTorch的核心数据结构——张量（Tensor）。我们将学习如何创建张量、进行基本操作、重塑张量，以及如何在PyTorch张量和NumPy数组之间进行转换。

![](img/90c2a152a218100453276638b7fdbc56_1.png)

## 概述 📋

PyTorch中的所有操作都基于张量。张量可以理解为多维数组，它可以有1维（向量）、2维（矩阵）或更高维度。掌握张量的基本操作是使用PyTorch进行深度学习的第一步。

![](img/90c2a152a218100453276638b7fdbc56_3.png)

![](img/90c2a152a218100453276638b7fdbc56_4.png)

## 创建张量 🛠️

首先，我们需要导入PyTorch库。

![](img/90c2a152a218100453276638b7fdbc56_6.png)

```python
import torch
```

以下是几种创建张量的方法。

![](img/90c2a152a218100453276638b7fdbc56_8.png)

![](img/90c2a152a218100453276638b7fdbc56_9.png)

### 创建空张量

![](img/90c2a152a218100453276638b7fdbc56_10.png)

![](img/90c2a152a218100453276638b7fdbc56_11.png)

使用 `torch.empty()` 可以创建一个未初始化的张量。你需要指定张量的大小。

![](img/90c2a152a218100453276638b7fdbc56_13.png)

```python
# 创建一个标量（0维张量）
x = torch.empty(1)
print(x)

![](img/90c2a152a218100453276638b7fdbc56_15.png)

# 创建一个包含三个元素的1维向量
x = torch.empty(3)
print(x)

![](img/90c2a152a218100453276638b7fdbc56_17.png)

# 创建一个2x3的2维矩阵
x = torch.empty(2, 3)
print(x)

# 创建一个更高维度的张量，例如4维
x = torch.empty(2, 3, 4, 5)
```

![](img/90c2a152a218100453276638b7fdbc56_19.png)

### 创建随机值张量

![](img/90c2a152a218100453276638b7fdbc56_21.png)

使用 `torch.rand()` 可以创建一个填充了随机值的张量。

```python
# 创建一个2x2的随机张量
x = torch.rand(2, 2)
print(x)
```

![](img/90c2a152a218100453276638b7fdbc56_23.png)

![](img/90c2a152a218100453276638b7fdbc56_24.png)

![](img/90c2a152a218100453276638b7fdbc56_25.png)

### 创建特定值张量

你可以创建填充特定值的张量，例如全零或全一张量。

![](img/90c2a152a218100453276638b7fdbc56_27.png)

![](img/90c2a152a218100453276638b7fdbc56_28.png)

```python
# 创建一个2x2的全零张量
x = torch.zeros(2, 2)
print(x)

![](img/90c2a152a218100453276638b7fdbc56_29.png)

![](img/90c2a152a218100453276638b7fdbc56_30.png)

# 创建一个2x2的全一张量
x = torch.ones(2, 2)
print(x)
```

![](img/90c2a152a218100453276638b7fdbc56_32.png)

### 指定数据类型

![](img/90c2a152a218100453276638b7fdbc56_34.png)

![](img/90c2a152a218100453276638b7fdbc56_35.png)

创建张量时，可以指定其数据类型。

![](img/90c2a152a218100453276638b7fdbc56_37.png)

```python
# 创建一个全一张量，并指定数据类型为双精度浮点数
x = torch.ones(2, 2, dtype=torch.double)
print(x)
print(x.dtype)  # 查看数据类型
```

![](img/90c2a152a218100453276638b7fdbc56_39.png)

![](img/90c2a152a218100453276638b7fdbc56_40.png)

### 从Python列表创建

![](img/90c2a152a218100453276638b7fdbc56_42.png)

使用 `torch.tensor()` 可以从Python列表直接创建张量。

![](img/90c2a152a218100453276638b7fdbc56_44.png)

```python
# 从列表创建张量
x = torch.tensor([2.5, 0.1])
print(x)
```

![](img/90c2a152a218100453276638b7fdbc56_45.png)

![](img/90c2a152a218100453276638b7fdbc56_46.png)

上一节我们介绍了如何创建各种张量，本节中我们来看看如何获取张量的属性信息。

![](img/90c2a152a218100453276638b7fdbc56_48.png)

### 获取张量属性

![](img/90c2a152a218100453276638b7fdbc56_49.png)

![](img/90c2a152a218100453276638b7fdbc56_50.png)

你可以查看张量的大小和形状。

![](img/90c2a152a218100453276638b7fdbc56_52.png)

```python
x = torch.rand(2, 3)
print(x.size())  # 输出张量的大小，例如 torch.Size([2, 3])
```

![](img/90c2a152a218100453276638b7fdbc56_53.png)

![](img/90c2a152a218100453276638b7fdbc56_55.png)

## 张量的基本运算 ➕➖✖️➗

![](img/90c2a152a218100453276638b7fdbc56_57.png)

现在，让我们创建两个张量并尝试一些基本运算。

![](img/90c2a152a218100453276638b7fdbc56_59.png)

```python
# 创建两个2x2的随机张量
x = torch.rand(2, 2)
y = torch.rand(2, 2)
print(x)
print(y)
```

![](img/90c2a152a218100453276638b7fdbc56_60.png)

![](img/90c2a152a218100453276638b7fdbc56_61.png)

以下是我们可以执行的基本运算。

![](img/90c2a152a218100453276638b7fdbc56_63.png)

### 加法

![](img/90c2a152a218100453276638b7fdbc56_65.png)

```python
# 逐元素加法
z = x + y
print(z)

![](img/90c2a152a218100453276638b7fdbc56_67.png)

![](img/90c2a152a218100453276638b7fdbc56_68.png)

# 使用torch.add函数实现相同功能
z = torch.add(x, y)
print(z)
```

### 就地操作

![](img/90c2a152a218100453276638b7fdbc56_70.png)

在PyTorch中，任何带有下划线后缀的函数（如 `add_`）都会执行就地操作，直接修改原变量。

![](img/90c2a152a218100453276638b7fdbc56_71.png)

```python
# 将x加到y上，并修改y
y.add_(x)
print(y)  # y已被修改
```

![](img/90c2a152a218100453276638b7fdbc56_73.png)

![](img/90c2a152a218100453276638b7fdbc56_74.png)

### 减法

![](img/90c2a152a218100453276638b7fdbc56_75.png)

```python
# 逐元素减法
z = x - y
print(z)

![](img/90c2a152a218100453276638b7fdbc56_77.png)

# 使用torch.sub函数
z = torch.sub(x, y)
print(z)
```

![](img/90c2a152a218100453276638b7fdbc56_79.png)

### 乘法

![](img/90c2a152a218100453276638b7fdbc56_81.png)

```python
# 逐元素乘法
z = x * y
print(z)

![](img/90c2a152a218100453276638b7fdbc56_82.png)

![](img/90c2a152a218100453276638b7fdbc56_83.png)

# 使用torch.mul函数
z = torch.mul(x, y)
print(z)

![](img/90c2a152a218100453276638b7fdbc56_85.png)

# 就地乘法
y.mul_(x)
print(y)
```

![](img/90c2a152a218100453276638b7fdbc56_87.png)

### 除法

![](img/90c2a152a218100453276638b7fdbc56_89.png)

```python
# 逐元素除法
z = x / y
print(z)

![](img/90c2a152a218100453276638b7fdbc56_90.png)

# 使用torch.div函数
z = torch.div(x, y)
print(z)
```

![](img/90c2a152a218100453276638b7fdbc56_92.png)

![](img/90c2a152a218100453276638b7fdbc56_93.png)

## 张量的切片与索引 🔪

![](img/90c2a152a218100453276638b7fdbc56_95.png)

![](img/90c2a152a218100453276638b7fdbc56_96.png)

与NumPy数组类似，PyTorch张量也支持切片操作。

![](img/90c2a152a218100453276638b7fdbc56_98.png)

```python
# 创建一个5x3的张量
x = torch.rand(5, 3)
print(x)

# 获取所有行的第一列
print(x[:, 0])

# 获取第二行的所有列
print(x[1, :])

![](img/90c2a152a218100453276638b7fdbc56_100.png)

# 获取特定位置的元素（例如第1行第1列，注意索引从0开始）
print(x[1, 1])

![](img/90c2a152a218100453276638b7fdbc56_101.png)

![](img/90c2a152a218100453276638b7fdbc56_102.png)

# 如果张量只有一个元素，可以使用.item()获取其Python数值
scalar_tensor = torch.tensor([3.1415])
print(scalar_tensor.item())
```

![](img/90c2a152a218100453276638b7fdbc56_103.png)

![](img/90c2a152a218100453276638b7fdbc56_104.png)

## 重塑张量 🔄

![](img/90c2a152a218100453276638b7fdbc56_106.png)

![](img/90c2a152a218100453276638b7fdbc56_107.png)

使用 `.view()` 方法可以改变张量的形状，但元素总数必须保持不变。

![](img/90c2a152a218100453276638b7fdbc56_109.png)

```python
# 创建一个4x4的张量
x = torch.rand(4, 4)
print(x)
print(x.size())

![](img/90c2a152a218100453276638b7fdbc56_111.png)

![](img/90c2a152a218100453276638b7fdbc56_112.png)

# 重塑为一维向量（16个元素）
y = x.view(16)
print(y)
print(y.size())

![](img/90c2a152a218100453276638b7fdbc56_113.png)

# 重塑为2x8的矩阵
y = x.view(2, 8)
print(y)
print(y.size())

![](img/90c2a152a218100453276638b7fdbc56_115.png)

# 使用-1让PyTorch自动计算某一维度的大小
# 例如，将4x4重塑为2x?，PyTorch会自动计算出?为8
y = x.view(2, -1)
print(y)
print(y.size())
```

![](img/90c2a152a218100453276638b7fdbc56_117.png)

## 与NumPy的转换 🔄

![](img/90c2a152a218100453276638b7fdbc56_118.png)

![](img/90c2a152a218100453276638b7fdbc56_119.png)

PyTorch张量和NumPy数组可以方便地相互转换，但需要注意内存共享问题。

![](img/90c2a152a218100453276638b7fdbc56_121.png)

### 导入NumPy

![](img/90c2a152a218100453276638b7fdbc56_123.png)

```python
import numpy as np
```

![](img/90c2a152a218100453276638b7fdbc56_125.png)

### 张量转NumPy数组

![](img/90c2a152a218100453276638b7fdbc56_127.png)

![](img/90c2a152a218100453276638b7fdbc56_128.png)

```python
# 创建一个PyTorch张量
a = torch.ones(5)
print(a)

![](img/90c2a152a218100453276638b7fdbc56_130.png)

# 转换为NumPy数组
b = a.numpy()
print(b)
print(type(b))
```

![](img/90c2a152a218100453276638b7fdbc56_132.png)

**重要提示**：如果张量在CPU上（不在GPU上），转换后的NumPy数组与原始张量将共享同一块内存。修改其中一个会影响到另一个。

```python
# 修改张量
a.add_(1)
print(a)  # 张量已改变
print(b)  # NumPy数组也随之改变
```

![](img/90c2a152a218100453276638b7fdbc56_134.png)

### NumPy数组转张量

![](img/90c2a152a218100453276638b7fdbc56_136.png)

```python
# 创建一个NumPy数组
a = np.ones(5)
print(a)

# 转换为PyTorch张量
b = torch.from_numpy(a)
print(b)
```

![](img/90c2a152a218100453276638b7fdbc56_138.png)

![](img/90c2a152a218100453276638b7fdbc56_139.png)

同样，它们也共享内存。

![](img/90c2a152a218100453276638b7fdbc56_140.png)

![](img/90c2a152a218100453276638b7fdbc56_141.png)

```python
# 修改NumPy数组
a += 1
print(a)  # NumPy数组已改变
print(b)  # 张量也随之改变
```

![](img/90c2a152a218100453276638b7fdbc56_142.png)

## GPU支持（可选） 🚀

![](img/90c2a152a218100453276638b7fdbc56_144.png)

![](img/90c2a152a218100453276638b7fdbc56_145.png)

如果你的计算机配备了NVIDIA GPU并安装了CUDA工具包，你可以将张量移动到GPU上进行加速计算。

![](img/90c2a152a218100453276638b7fdbc56_147.png)

```python
# 检查CUDA是否可用
print(torch.cuda.is_available())

![](img/90c2a152a218100453276638b7fdbc56_148.png)

![](img/90c2a152a218100453276638b7fdbc56_149.png)

# 如果可用，指定设备
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print(device)

![](img/90c2a152a218100453276638b7fdbc56_151.png)

# 在GPU上创建张量
if torch.cuda.is_available():
    x = torch.ones(5, device=device)  # 直接在GPU上创建
    # 或者将CPU上的张量移动到GPU
    y = torch.ones(5)
    y = y.to(device)
    # 在GPU上执行运算
    z = x + y
    print(z)
    # 注意：GPU上的张量不能直接转换为NumPy数组
    # 需要先移回CPU
    z = z.to("cpu")
    print(z.numpy())
```

![](img/90c2a152a218100453276638b7fdbc56_152.png)

## 自动求导梯度 ⚙️

![](img/90c2a152a218100453276638b7fdbc56_154.png)

在深度学习中，我们经常需要计算梯度以优化模型参数。PyTorch通过设置 `requires_grad=True` 来跟踪张量的所有操作，以便后续进行自动求导。

![](img/90c2a152a218100453276638b7fdbc56_156.png)

```python
# 创建一个需要计算梯度的张量
x = torch.ones(5, requires_grad=True)
print(x)
```

![](img/90c2a152a218100453276638b7fdbc56_158.png)

![](img/90c2a152a218100453276638b7fdbc56_159.png)

我们将在后续关于自动求导的教程中详细讨论这一重要特性。

## 总结 🎯

![](img/90c2a152a218100453276638b7fdbc56_161.png)

本节课中我们一起学习了PyTorch张量的基础知识：
1.  **创建张量**：学习了使用 `torch.empty()`, `torch.rand()`, `torch.zeros()`, `torch.ones()` 以及从列表创建张量的方法。
2.  **基本运算**：掌握了张量的逐元素加法、减法、乘法、除法以及就地操作。
3.  **切片与索引**：学会了如何访问和修改张量中的特定元素。
4.  **重塑张量**：使用 `.view()` 方法改变张量的形状。
5.  **与NumPy互转**：理解了如何在PyTorch张量和NumPy数组之间进行转换，并注意到了内存共享的问题。
6.  **GPU支持**：了解了如何利用GPU加速计算（如果可用）。
7.  **梯度计算**：初步认识了 `requires_grad` 参数，它为后续的自动求导和模型优化奠定了基础。

![](img/90c2a152a218100453276638b7fdbc56_162.png)

![](img/90c2a152a218100453276638b7fdbc56_163.png)

张量是PyTorch的基石，熟练掌握这些操作是构建和训练深度学习模型的关键第一步。