# 006：标量

在本节课中，我们将要学习张量的一种基础类型——标量。我们将了解标量的理论定义、数学表示方法，并首次进行动手实践，在三个主流的Python张量库（NumPy、TensorFlow和PyTorch）中创建和操作标量。

## 概述

上一节我们介绍了张量的基本概念。本节中，我们来看看张量中最简单的一种类型：标量。标量是零维张量，仅包含一个单一的数值。理解标量是理解更高维张量（如向量和矩阵）的基础。

## 标量的理论

![](img/120a8201d2c85929261b6c1d09e3b47a_1.png)

![](img/120a8201d2c85929261b6c1d09e3b47a_3.png)

标量的特征在于其没有维度，它是一个单一的数值。在数学表示中，标量通常用小写斜体字母表示，例如 **x**。

与所有在机器学习或数值计算库中使用的张量一样，标量也具有数据类型。例如，它可以是整数（如 `int`）或32位浮点数（如 `float32`）。

![](img/120a8201d2c85929261b6c1d09e3b47a_5.png)

## 动手实践：创建标量

![](img/120a8201d2c85929261b6c1d09e3b47a_6.png)

![](img/120a8201d2c85929261b6c1d09e3b47a_7.png)

![](img/120a8201d2c85929261b6c1d09e3b47a_8.png)

![](img/120a8201d2c85929261b6c1d09e3b47a_9.png)

以下是创建和操作标量的实践步骤。我们将使用一个Jupyter笔记本进行交互式学习。

### 访问学习材料

![](img/120a8201d2c85929261b6c1d09e3b47a_11.png)

![](img/120a8201d2c85929261b6c1d09e3b47a_12.png)

首先，你需要访问包含所有代码示例的Jupyter笔记本。

1.  前往 GitHub 仓库：`github.com/jochroone/mfoundations`。
2.  在该仓库中，找到“机器学习基础”系列的第一个主题：“线性代数导论”。
3.  点击链接，在线打开对应的Jupyter笔记本，或者进入 `notebooks` 目录，找到 `Intro to linear algebra` 文件。

为了获得最佳的交互学习体验，建议在 Google Colab 中打开并运行该笔记本。

![](img/120a8201d2c85929261b6c1d09e3b47a_14.png)

![](img/120a8201d2c85929261b6c1d09e3b47a_15.png)

![](img/120a8201d2c85929261b6c1d09e3b47a_16.png)

![](img/120a8201d2c85929261b6c1d09e3b47a_17.png)

### 准备Colab环境

在Colab中打开笔记本后，建议执行以下操作以获得清晰的界面：

![](img/120a8201d2c85929261b6c1d09e3b47a_19.png)

1.  点击菜单栏中的“编辑”选项。
2.  选择“清除所有输出”。这将清除之前运行的所有代码结果，让你可以重新执行代码单元，获得干净、新鲜的输出。

![](img/120a8201d2c85929261b6c1d09e3b47a_21.png)

Jupyter笔记本混合了Markdown文本单元和代码单元。Markdown单元用于解释说明，代码单元包含可执行的Python代码。你可以通过按 `Shift + Enter` 或点击单元格左侧的“播放”按钮来执行代码。

### 使用基础Python创建标量

我们首先使用纯Python来创建一个零阶张量（即标量）。

```python
# 创建一个标量张量 x，赋值为 25
x = 25
print(x)
print(type(x))
```

执行上述代码，`x` 的值是25，其类型是Python的 `int`（整数）。如果我们创建另一个标量 `y = 3`，可以使用Python的加法运算符将它们相加：

![](img/120a8201d2c85929261b6c1d09e3b47a_23.png)

![](img/120a8201d2c85929261b6c1d09e3b47a_24.png)

```python
y = 3
py_sum = x + y
print(py_sum)
print(type(py_sum))
```

![](img/120a8201d2c85929261b6c1d09e3b47a_26.png)

结果是28，类型同样是 `int`。如果创建一个浮点数标量：

```python
z = 4.0
print(type(z))
result = z + y  # 浮点数与整数相加
print(result)
print(type(result))
```

当一个整数和一个浮点数进行运算时，结果会默认转换为浮点类型（`float`）。

### 使用PyTorch创建标量

![](img/120a8201d2c85929261b6c1d09e3b47a_28.png)

![](img/120a8201d2c85929261b6c1d09e3b47a_29.png)

PyTorch是一个用于自动微分（后续微积分主题会深入介绍）的流行Python库。PyTorch张量的设计非常“Python化”，行为类似NumPy数组，但其优势在于可以方便地利用GPU进行并行计算，这对于深度学习训练至关重要。

![](img/120a8201d2c85929261b6c1d09e3b47a_31.png)

以下是PyTorch张量类型的文档链接，可供参考：[PyTorch Tensor Types](https://pytorch.org/docs/stable/tensors.html)。

现在，让我们在代码中导入PyTorch并创建标量。Colab默认已安装PyTorch。

```python
import torch

# 创建一个PyTorch标量张量，值为25
x_pt = torch.tensor(25)
print(x_pt)
print(x_pt.shape)
```

这里我们没有显式指定数据类型，PyTorch会自动推断。打印其形状（`shape`），会得到一个空元组 `torch.Size([])`，这证实了标量没有维度。

### 使用TensorFlow创建标量

![](img/120a8201d2c85929261b6c1d09e3b47a_33.png)

![](img/120a8201d2c85929261b6c1d09e3b47a_34.png)

TensorFlow是另一个主流的自动微分库。在TensorFlow中，张量通常通过包装器创建。最常见的类型是 `Variable`、`Constant`、`Placeholder` 和 `SparseTensor`，其中 `Variable` 最为常用。

以下是TensorFlow张量类型的文档链接：[TensorFlow DTypes](https://www.tensorflow.org/api_docs/python/tf/dtypes)。

让我们导入TensorFlow并创建标量：

```python
import tensorflow as tf

# 创建一个TensorFlow变量（标量张量），显式指定为64位整数
x_tf = tf.Variable(25, dtype=tf.int64)
print(x_tf)
print(x_tf.shape)
```

与PyTorch相比，TensorFlow的打印输出可能看起来更复杂一些，这是其底层C++架构带来的特点。查看其形状，同样是 `TensorShape([])`，表示零维。

我们可以创建另一个标量并进行加法运算：

```python
y_tf = tf.Variable(3, dtype=tf.int64)
# 两种加法操作是等价的
sum_tf_1 = tf.add(x_tf, y_tf)
sum_tf_2 = x_tf + y_tf
print(sum_tf_1)
print(sum_tf_2)
```

Python的 `+` 运算符在TensorFlow张量上已被重载，其底层调用的就是 `tf.add` 方法。

![](img/120a8201d2c85929261b6c1d09e3b47a_36.png)

![](img/120a8201d2c85929261b6c1d09e3b47a_37.png)

此外，可以轻松地将TensorFlow张量转换为NumPy数组：

![](img/120a8201d2c85929261b6c1d09e3b47a_39.png)

```python
import numpy as np
sum_np = sum_tf_1.numpy()
print(sum_np)
print(type(sum_np))
```

最后，我们创建一个浮点数类型的标量：

```python
float_tf = tf.Variable(25, dtype=tf.float16)
print(float_tf)
print(float_tf.shape)
```

即使指定为浮点类型，它仍然是一个没有维度的标量。

### 库的选择说明

值得注意的是，PyTorch和TensorFlow都提供了训练机器学习模型所需的所有功能，包括自动微分和GPU加速。两者各有优势：
*   **PyTorch**：更“Python化”，代码直观，调试方便，使用体验流畅。
*   **TensorFlow**：社区更庞大，生态系统成熟，在生产环境部署、多服务器训练和移动端服务方面有丰富的配套工具。

在本系列教程中，出于易用性和教学清晰度的考虑，后续示例可能会更倾向于使用PyTorch。

## 总结

![](img/120a8201d2c85929261b6c1d09e3b47a_41.png)

本节课中我们一起学习了标量张量。我们明确了标量是零维的单一数值，并掌握了在基础Python、PyTorch和TensorFlow中创建和操作标量的方法。我们还初步了解了PyTorch和TensorFlow这两个重要库的特点。在下一节中，我们将学习如何创建向量（一维张量）以及在这些库中进行基本的向量运算。