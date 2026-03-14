# 058：One-Hot编码与多类别交叉熵代码示例 🔢

![](img/d7633ffbd2ad795f213c96187346d24b_1.png)

![](img/d7633ffbd2ad795f213c96187346d24b_3.png)

在本节中，我们将通过一个PyTorch代码示例，具体演示One-Hot编码和多类别交叉熵损失的计算过程。我们将从准备数据开始，逐步实现Softmax激活函数、预测类别，并最终计算损失值。

![](img/d7633ffbd2ad795f213c96187346d24b_5.png)

## 概述

我们将创建一个简单的示例，包含四个训练样本和三个类别。通过手动实现和调用PyTorch内置函数两种方式，计算交叉熵损失，并比较其结果。

---

![](img/d7633ffbd2ad795f213c96187346d24b_7.png)

![](img/d7633ffbd2ad795f213c96187346d24b_8.png)

### One-Hot编码实现

![](img/d7633ffbd2ad795f213c96187346d24b_10.png)

![](img/d7633ffbd2ad795f213c96187346d24b_11.png)

首先，我们实现一个One-Hot编码函数。虽然PyTorch的损失函数内部会自动处理One-Hot编码，但了解其原理是有益的。以下是该函数的代码：

![](img/d7633ffbd2ad795f213c96187346d24b_13.png)

```python
def one_hot_encoding(class_labels, num_classes):
    # 此函数使用scatter方法将类别标签转换为One-Hot编码矩阵
    # 初学者可直接使用此函数，无需深究其内部scatter的工作原理
    return torch.zeros(class_labels.size(0), num_classes).scatter_(1, class_labels.view(-1, 1), 1)
```

![](img/d7633ffbd2ad795f213c96187346d24b_15.png)

![](img/d7633ffbd2ad795f213c96187346d24b_17.png)

我们定义类别标签和类别总数，并应用该函数：

![](img/d7633ffbd2ad795f213c96187346d24b_19.png)

![](img/d7633ffbd2ad795f213c96187346d24b_21.png)

```python
class_labels = torch.tensor([0, 1, 2, 2])
num_classes = 3
y_onehot = one_hot_encoding(class_labels, num_classes)
```

![](img/d7633ffbd2ad795f213c96187346d24b_23.png)

得到的 `y_onehot` 是一个4x3的矩阵，每一行代表一个样本的One-Hot编码。例如，第一行 `[1, 0, 0]` 表示类别0。

![](img/d7633ffbd2ad795f213c96187346d24b_25.png)

![](img/d7633ffbd2ad795f213c96187346d24b_27.png)

---

![](img/d7633ffbd2ad795f213c96187346d24b_29.png)

### 准备网络输入与Softmax激活

![](img/d7633ffbd2ad795f213c96187346d24b_31.png)

接下来，我们定义一组模拟的网络输入值（logits）：

![](img/d7633ffbd2ad795f213c96187346d24b_33.png)

![](img/d7633ffbd2ad795f213c96187346d24b_34.png)

```python
Z = torch.tensor([[1.0, 2.0, 3.0],
                  [1.5, 2.5, 0.5],
                  [2.0, 1.0, 4.0],
                  [0.5, 2.5, 2.0]])
```

![](img/d7633ffbd2ad795f213c96187346d24b_36.png)

![](img/d7633ffbd2ad795f213c96187346d24b_38.png)

为了得到类别概率，我们需要对网络输入应用Softmax函数。Softmax的公式如下：

**公式：** `softmax(z_i) = exp(z_i) / Σ_j exp(z_j)`

![](img/d7633ffbd2ad795f213c96187346d24b_40.png)

我们手动实现Softmax函数如下：

![](img/d7633ffbd2ad795f213c96187346d24b_42.png)

![](img/d7633ffbd2ad795f213c96187346d24b_43.png)

```python
def softmax(z):
    # 减去最大值以提高数值稳定性
    exp_z = torch.exp(z - torch.max(z, dim=1, keepdim=True)[0])
    return exp_z / torch.sum(exp_z, dim=1, keepdim=True)

![](img/d7633ffbd2ad795f213c96187346d24b_45.png)

![](img/d7633ffbd2ad795f213c96187346d24b_47.png)

activations = softmax(Z)
```

![](img/d7633ffbd2ad795f213c96187346d24b_49.png)

![](img/d7633ffbd2ad795f213c96187346d24b_50.png)

![](img/d7633ffbd2ad795f213c96187346d24b_52.png)

计算后，`activations` 的每一行（对应一个样本）的概率之和为1。

![](img/d7633ffbd2ad795f213c96187346d24b_54.png)

![](img/d7633ffbd2ad795f213c96187346d24b_56.png)

---

![](img/d7633ffbd2ad795f213c96187346d24b_58.png)

### 获取预测类别

![](img/d7633ffbd2ad795f213c96187346d24b_60.png)

![](img/d7633ffbd2ad795f213c96187346d24b_61.png)

为了得到模型的预测类别，我们取概率最大的那个类别索引。这可以通过 `argmax` 函数实现：

![](img/d7633ffbd2ad795f213c96187346d24b_63.png)

![](img/d7633ffbd2ad795f213c96187346d24b_65.png)

```python
predictions = torch.argmax(activations, dim=1)
```

![](img/d7633ffbd2ad795f213c96187346d24b_67.png)

![](img/d7633ffbd2ad795f213c96187346d24b_69.png)

在我们的例子中，预测结果与真实标签 `[0, 1, 0, 2]` 对比，会发现第三个样本预测错误（预测为0，真实是2）。

---

![](img/d7633ffbd2ad795f213c96187346d24b_71.png)

![](img/d7633ffbd2ad795f213c96187346d24b_72.png)

### 计算交叉熵损失

![](img/d7633ffbd2ad795f213c96187346d24b_74.png)

![](img/d7633ffbd2ad795f213c96187346d24b_76.png)

交叉熵损失的公式为：

![](img/d7633ffbd2ad795f213c96187346d24b_78.png)

**公式：** `Loss = - Σ (y_true * log(y_pred))`

![](img/d7633ffbd2ad795f213c96187346d24b_80.png)

其中求和包括所有样本和所有类别。由于One-Hot编码中 `y_true` 仅在真实类别处为1，其余为0，因此这个求和实际上只计算了每个样本在真实类别上的预测概率的负对数。

我们首先手动计算每个样本的损失：

![](img/d7633ffbd2ad795f213c96187346d24b_82.png)

![](img/d7633ffbd2ad795f213c96187346d24b_84.png)

```python
loss_per_sample = -torch.sum(y_onehot * torch.log(activations), dim=1)
```

![](img/d7633ffbd2ad795f213c96187346d24b_86.png)

![](img/d7633ffbd2ad795f213c96187346d24b_87.png)

得到的 `loss_per_sample` 是每个样本的损失值。整个批次的损失可以取平均值：

![](img/d7633ffbd2ad795f213c96187346d24b_89.png)

![](img/d7633ffbd2ad795f213c96187346d24b_91.png)

```python
total_loss_manual = torch.mean(loss_per_sample)
```

![](img/d7633ffbd2ad795f213c96187346d24b_93.png)

![](img/d7633ffbd2ad795f213c96187346d24b_95.png)

---

![](img/d7633ffbd2ad795f213c96187346d24b_96.png)

![](img/d7633ffbd2ad795f213c96187346d24b_98.png)

### 使用PyTorch内置损失函数

![](img/d7633ffbd2ad795f213c96187346d24b_100.png)

上一节我们手动计算了损失，本节中我们来看看如何使用PyTorch内置的高效且数值稳定的函数来完成相同任务。

![](img/d7633ffbd2ad795f213c96187346d24b_102.png)

PyTorch提供了 `nn.CrossEntropyLoss`。**重要提示**：该函数期望接收的是**原始的网络输入（logits）**，而不是经过Softmax后的概率。它会在内部组合Softmax和交叉熵计算。

![](img/d7633ffbd2ad795f213c96187346d24b_104.png)

以下是使用示例：

![](img/d7633ffbd2ad795f213c96187346d24b_106.png)

```python
import torch.nn as nn

# 方法1：使用 nn.NLLLoss (需要手动log-softmax输入)
log_softmax_activations = torch.log(activations)
criterion_nll = nn.NLLLoss(reduction='none')
loss_nll = criterion_nll(log_softmax_activations, class_labels)

![](img/d7633ffbd2ad795f213c96187346d24b_108.png)

# 方法2：使用 nn.CrossEntropyLoss (推荐，输入为logits)
criterion_ce = nn.CrossEntropyLoss(reduction='none')
loss_ce = criterion_ce(Z, class_labels)

![](img/d7633ffbd2ad795f213c96187346d24b_109.png)

![](img/d7633ffbd2ad795f213c96187346d24b_110.png)

![](img/d7633ffbd2ad795f213c96187346d24b_112.png)

# 验证结果一致性
print(torch.allclose(loss_per_sample, loss_nll))  # 应返回 True
print(torch.allclose(loss_per_sample, loss_ce))   # 应返回 True
```

![](img/d7633ffbd2ad795f213c96187346d24b_114.png)

![](img/d7633ffbd2ad795f213c96187346d24b_115.png)

默认情况下，`reduction` 参数是 `‘mean‘`，会对批次损失求平均。设置为 `‘none‘` 可以获取每个样本的损失，以便与我们手动计算的结果对比。

![](img/d7633ffbd2ad795f213c96187346d24b_117.png)

![](img/d7633ffbd2ad795f213c96187346d24b_118.png)

![](img/d7633ffbd2ad795f213c96187346d24b_120.png)

**核心建议**：在实践中，始终使用 `nn.CrossEntropyLoss` 并直接传入网络输出（logits），因为其数值稳定性更好，有助于模型训练。

![](img/d7633ffbd2ad795f213c96187346d24b_122.png)

![](img/d7633ffbd2ad795f213c96187346d24b_124.png)

---

### 总结

本节课中我们一起学习了：
1.  **One-Hot编码**：将类别标签转换为二进制矩阵表示。
2.  **Softmax函数**：将网络输入转换为概率分布。
3.  **交叉熵损失计算**：衡量模型预测概率分布与真实分布之间的差异。
4.  **PyTorch实现**：使用手动计算和内置的 `nn.CrossEntropyLoss` 函数两种方式计算损失，并强调使用内置函数是更优选择。

![](img/d7633ffbd2ad795f213c96187346d24b_126.png)

![](img/d7633ffbd2ad795f213c96187346d24b_128.png)

通过本示例，你应该对多分类问题中的损失计算有了直观的理解，并掌握了在PyTorch中正确使用损失函数的方法。在接下来的视频中，我们将探讨如何为这类模型计算梯度。