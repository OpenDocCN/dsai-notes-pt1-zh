# 015：基本张量算术（哈达玛积） 🧮

在本节课中，我们将通过NumPy、TensorFlow和PyTorch的代码演示，学习基本的张量算术运算，包括哈达玛积（Hadamard product）。

上一节我们介绍了张量的基本概念和创建方法。本节中我们来看看如何对张量进行基础的算术运算。

## 张量与标量的运算

![](img/fa59c0c24e93ceb64684279dec417ad4_1.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_2.png)

最基本的张量算术运算之一是与标量值进行加法或乘法。该运算会将标量应用到张量的所有元素上，并保持张量的形状不变。

![](img/fa59c0c24e93ceb64684279dec417ad4_3.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_4.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_5.png)

例如，我们有一个NumPy矩阵张量 `X`，其形状和值如下：

![](img/fa59c0c24e93ceb64684279dec417ad4_7.png)

```python
import numpy as np
X = np.array([[25, 2], [5, 26], [3, 30]])
```

我们可以将整个张量乘以2，所有元素都会乘以2。

![](img/fa59c0c24e93ceb64684279dec417ad4_9.png)

```python
X * 2
# 输出: array([[50,  4],
#              [10, 52],
#              [ 6, 60]])
```

我们也可以将整个张量加上2，所有元素都会加上2。

![](img/fa59c0c24e93ceb64684279dec417ad4_11.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_12.png)

```python
X + 2
# 输出: array([[27,  4],
#              [ 7, 28],
#              [ 5, 32]])
```

![](img/fa59c0c24e93ceb64684279dec417ad4_14.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_15.png)

我们甚至可以应用混合运算，标准的运算顺序同样适用。例如，`X * 2 + 2` 会先进行乘法，再进行加法。

```python
X * 2 + 2
# 输出: array([[52,  6],
#              [12, 54],
#              [ 8, 62]])
```

可以验证，每个元素的计算结果都符合预期。张量的形状完全保持不变，输入是3行2列，输出也是3行2列。

![](img/fa59c0c24e93ceb64684279dec417ad4_17.png)

在PyTorch中，我们可以用与NumPy完全相同的方式进行这些标量乘法和加法运算。

```python
import torch
X_torch = torch.tensor([[25, 2], [5, 26], [3, 30]])
X_torch * 2 + 2
# 输出: tensor([[52,  6],
#               [12, 54],
#               [ 8, 62]])
```

![](img/fa59c0c24e93ceb64684279dec417ad4_19.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_20.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_21.png)

Python运算符被重载了。当我们使用乘号（`*`）或加号（`+`）这些Python中用于算术运算的标准符号对PyTorch张量进行操作时，实际上是在后台调用了`torch.mul()`或`torch.add()`方法。这一切在幕后自动完成，无需担心。

![](img/fa59c0c24e93ceb64684279dec417ad4_23.png)

以下是显式使用方法的示例，会得到完全相同的结果。

![](img/fa59c0c24e93ceb64684279dec417ad4_24.png)

```python
torch.mul(X_torch, 2)
torch.add(torch.mul(X_torch, 2), 2)
```

TensorFlow的情况也类似。你可以使用标准的 `* 2 + 2` 运算，因为当操作对象是TensorFlow矩阵时，Python运算符同样被重载了。这等价于调用 `tf.multiply()` 和 `tf.add()` 方法，使用相同的方法会得到相同的结果。

![](img/fa59c0c24e93ceb64684279dec417ad4_26.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_27.png)

```python
import tensorflow as tf
X_tf = tf.constant([[25, 2], [5, 26], [3, 30]])
X_tf * 2 + 2
# 输出: <tf.Tensor: shape=(3, 2), dtype=int32, numpy=
#       array([[52,  6],
#              [12, 54],
#              [ 8, 62]], dtype=int32)>
```

![](img/fa59c0c24e93ceb64684279dec417ad4_28.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_29.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_30.png)

无论你使用哪种方式进行这些标量运算，结果都是一致的。当然，同样的规则也适用于减法、除法等其他运算。

![](img/fa59c0c24e93ceb64684279dec417ad4_32.png)

## 哈达玛积（元素级乘积）🔗

![](img/fa59c0c24e93ceb64684279dec417ad4_34.png)

比标量运算更有趣一点的是**哈达玛积**，也称为**元素级乘积**。如果两个张量具有相同的形状，我们通常可以按元素进行运算，这也是这些库中的默认行为。

![](img/fa59c0c24e93ceb64684279dec417ad4_35.png)

**请注意**：这不是矩阵乘法，矩阵乘法我们将在稍后介绍。哈达玛积是元素级乘积。

其数学符号是一个带圆圈的点（⊙），表示哈达玛积。例如，对矩阵 `X` 和矩阵 `A` 应用哈达玛积：**X ⊙ A**。

让我们再次看看矩阵 `X`：

```python
X = np.array([[25, 2], [5, 26], [3, 30]])
```

![](img/fa59c0c24e93ceb64684279dec417ad4_37.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_38.png)

我们可以创建矩阵 `A`，例如通过对 `X` 的每个元素加2得到：

![](img/fa59c0c24e93ceb64684279dec417ad4_39.png)

```python
A = X + 2
# A = array([[27,  4],
#            [ 7, 28],
#            [ 5, 32]])
```

![](img/fa59c0c24e93ceb64684279dec417ad4_41.png)

现在，由于 `A` 和 `X` 形状相同，我们可以将它们相加。相加操作会按元素进行：25 + 27 = 52， 2 + 4 = 6，以此类推，贯穿整个3x2矩阵。

![](img/fa59c0c24e93ceb64684279dec417ad4_43.png)

```python
X + A
# 输出: array([[52,  6],
#              [12, 54],
#              [ 8, 62]])
```

当我们使用星号（`*`）这个典型的乘法运算符时，我们执行的就是哈达玛积。我们对 `A` 和 `X` 进行这个操作，它是元素级的：25 * 27 = 675， 2 * 4 = 8，等等。

![](img/fa59c0c24e93ceb64684279dec417ad4_45.png)

```python
X * A  # 哈达玛积
# 输出: array([[675,   8],
#              [ 35, 728],
#              [ 15, 960]])
```

在PyTorch和TensorFlow中，操作完全相同。我们可以在PyTorch中以与NumPy相同的方式创建矩阵 `A`，然后对其执行相同的元素级加法和哈达玛积运算。

![](img/fa59c0c24e93ceb64684279dec417ad4_47.png)

**PyTorch示例：**
```python
A_torch = X_torch + 2
X_torch + A_torch  # 元素级加法
X_torch * A_torch  # 哈达玛积
```

**TensorFlow示例：**
```python
A_tf = X_tf + 2
X_tf + A_tf  # 元素级加法
X_tf * A_tf  # 哈达玛积
```

![](img/fa59c0c24e93ceb64684279dec417ad4_49.png)

以上就是一些最基本的算术运算。

![](img/fa59c0c24e93ceb64684279dec417ad4_50.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_51.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_53.png)

## 总结 📝

![](img/fa59c0c24e93ceb64684279dec417ad4_55.png)

本节课中我们一起学习了基本的张量算术运算。我们首先了解了如何对张量进行标量运算（加、减、乘、除），这些运算会广播到张量的每个元素。接着，我们重点学习了哈达玛积，即两个形状相同的张量之间的元素级乘积，这在深度学习中非常常见。我们通过NumPy、PyTorch和TensorFlow的代码示例实践了这些操作，并注意到这些库通过运算符重载提供了直观的语法。

![](img/fa59c0c24e93ceb64684279dec417ad4_57.png)

![](img/fa59c0c24e93ceb64684279dec417ad4_58.png)

下一节，我们将学习另一种常见的张量操作：归约。