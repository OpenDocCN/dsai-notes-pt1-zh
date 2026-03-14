#  25：PyTorch 与 NumPy 交互 🧮

在本节课中，我们将要学习如何在 PyTorch 和 NumPy 之间进行数据转换。这是深度学习工作流中一个非常常见的步骤，因为数据可能最初以 NumPy 数组的形式存在，而我们需要将其转换为 PyTorch 张量以利用其深度学习功能。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_1.png)

![](img/3d79ac863a4bbc04d06057c6f6d2006b_2.png)

![](img/3d79ac863a4bbc04d06057c6f6d2006b_3.png)

---

上一节我们介绍了张量的索引操作，本节中我们来看看如何将 PyTorch 张量与另一个强大的数值计算库 NumPy 进行交互。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_5.png)

NumPy 是一个非常流行的 Python 科学计算库。实际上，安装 PyTorch 时通常也会安装 NumPy。由于这种紧密的关系，PyTorch 内置了与 NumPy 交互的功能。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_6.png)

以下是两种主要的转换场景：
*   **从 NumPy 数组到 PyTorch 张量**：使用 `torch.from_numpy()` 方法。
*   **从 PyTorch 张量到 NumPy 数组**：在张量上调用 `.numpy()` 方法。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_8.png)

![](img/3d79ac863a4bbc04d06057c6f6d2006b_10.png)

---

![](img/3d79ac863a4bbc04d06057c6f6d2006b_12.png)

### 从 NumPy 数组到 PyTorch 张量

![](img/3d79ac863a4bbc04d06057c6f6d2006b_14.png)

![](img/3d79ac863a4bbc04d06057c6f6d2006b_16.png)

让我们通过代码来实践第一种转换。

首先，我们需要导入必要的库。

```python
import torch
import numpy as np
```

接下来，我们创建一个 NumPy 数组。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_18.png)

```python
array = np.arange(1, 9)  # 创建一个包含 1 到 8 的数组
```

![](img/3d79ac863a4bbc04d06057c6f6d2006b_20.png)

现在，我们使用 `torch.from_numpy()` 将这个数组转换为 PyTorch 张量。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_21.png)

![](img/3d79ac863a4bbc04d06057c6f6d2006b_23.png)

```python
tensor = torch.from_numpy(array)
```

![](img/3d79ac863a4bbc04d06057c6f6d2006b_25.png)

我们可以打印出两者来查看结果。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_27.png)

```python
print(f"NumPy 数组: {array}")
print(f"PyTorch 张量: {tensor}")
```

![](img/3d79ac863a4bbc04d06057c6f6d2006b_29.png)

![](img/3d79ac863a4bbc04d06057c6f6d2006b_30.png)

你会注意到，转换后的张量数据类型是 `torch.float64`。这是因为 NumPy 的默认数据类型是 `float64`，而 `torch.from_numpy()` 会保留原始数组的数据类型。这与 PyTorch 默认创建 `float32` 类型张量的习惯不同，需要特别注意。

如果你想在转换时指定数据类型，可以这样做：

![](img/3d79ac863a4bbc04d06057c6f6d2006b_32.png)

```python
tensor = torch.from_numpy(array).type(torch.float32)
```

![](img/3d79ac863a4bbc04d06057c6f6d2006b_34.png)

![](img/3d79ac863a4bbc04d06057c6f6d2006b_36.png)

**重要提示**：通过 `torch.from_numpy()` 创建的张量与原始 NumPy 数组共享内存吗？让我们测试一下。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_38.png)

```python
# 修改原始数组
array = array + 1
print(f"修改后的数组: {array}")
print(f"对应的张量: {tensor}")
```

![](img/3d79ac863a4bbc04d06057c6f6d2006b_40.png)

你会发现，修改原始数组后，从它创建的张量**并没有**随之改变。这意味着转换过程创建了一个新的张量对象，两者不共享内存。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_42.png)

![](img/3d79ac863a4bbc04d06057c6f6d2006b_44.png)

---

![](img/3d79ac863a4bbc04d06057c6f6d2006b_46.png)

![](img/3d79ac863a4bbc04d06057c6f6d2006b_47.png)

### 从 PyTorch 张量到 NumPy 数组

![](img/3d79ac863a4bbc04d06057c6f6d2006b_49.png)

现在，让我们看看如何进行反向转换。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_51.png)

首先，创建一个 PyTorch 张量。

```python
tensor = torch.ones(7)  # 创建一个包含7个1的张量
```

![](img/3d79ac863a4bbc04d06057c6f6d2006b_53.png)

![](img/3d79ac863a4bbc04d06057c6f6d2006b_54.png)

然后，使用 `.numpy()` 方法将其转换为 NumPy 数组。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_56.png)

```python
numpy_tensor = tensor.numpy()
print(f"PyTorch 张量: {tensor}")
print(f"NumPy 数组: {numpy_tensor}")
```

转换后的 NumPy 数组会反映原始张量的数据类型。由于我们创建的张量默认是 `float32`，所以转换后的数组也是 `float32` 类型。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_58.png)

![](img/3d79ac863a4bbc04d06057c6f6d2006b_60.png)

同样，我们来测试数据是否共享内存。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_62.png)

```python
# 修改原始张量
tensor = tensor + 1
print(f"修改后的张量: {tensor}")
print(f"对应的NumPy数组: {numpy_tensor}")
```

![](img/3d79ac863a4bbc04d06057c6f6d2006b_64.png)

![](img/3d79ac863a4bbc04d06057c6f6d2006b_66.png)

修改张量后，之前转换得到的 NumPy 数组保持不变。这再次确认了转换会创建新的数据副本，两者不共享内存。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_68.png)

---

### 核心要点与注意事项

![](img/3d79ac863a4bbc04d06057c6f6d2006b_70.png)

![](img/3d79ac863a4bbc04d06057c6f6d2006b_72.png)

在进行 PyTorch 与 NumPy 的转换时，有以下几个关键点需要牢记：

![](img/3d79ac863a4bbc04d06057c6f6d2006b_74.png)

1.  **数据类型差异**：NumPy 默认使用 `float64`，而 PyTorch 默认使用 `float32`。在转换时要注意数据类型可能引发计算错误，必要时可以使用 `.type()` 方法进行转换。
2.  **内存不共享**：使用 `torch.from_numpy()` 和 `.numpy()` 进行转换时，会创建新的对象，修改源数据不会影响已转换的数据。
3.  **应用场景**：这种转换能力非常实用，它允许你在 NumPy 的生态（如数据加载和预处理）和 PyTorch 的生态（如模型训练）之间无缝切换。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_75.png)

如果你想深入了解，官方文档的 “PyTorch 与 NumPy” 部分提供了更多高级用例和细节。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_77.png)

---

本节课中我们一起学习了 PyTorch 张量与 NumPy 数组之间的相互转换。我们掌握了使用 `torch.from_numpy()` 将 NumPy 数组转为张量，以及使用 `.numpy()` 方法将张量转回 NumPy 数组。同时，我们明确了转换过程中数据类型的影响以及内存不共享的特性。掌握这些是构建高效深度学习工作流的基础。

![](img/3d79ac863a4bbc04d06057c6f6d2006b_79.png)

接下来，在下一节视频中，我们将探讨机器学习中一个至关重要的概念：可复现性。