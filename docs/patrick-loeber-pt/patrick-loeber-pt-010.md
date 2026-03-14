# 010：数据集变换（Dataset Transforms）🚀

在本节课中，我们将要学习PyTorch中一个非常实用的功能——数据集变换（Transforms）。我们将了解如何为自定义数据集添加变换支持，以及如何创建和使用自定义的变换类。

---

![](img/6d807e3e238aab5bca2f58cef0a9eb81_1.png)

## 概述

上一节我们介绍了如何创建自定义的`Dataset`类并使用`DataLoader`加载数据。本节中我们来看看如何为数据集应用变换。变换允许我们在数据被模型使用之前，对其进行一系列预处理操作，例如将数据转换为张量、归一化或数据增强。

PyTorch的`torchvision.transforms`模块提供了许多内置变换。同时，我们也可以轻松地编写自己的变换类。

![](img/6d807e3e238aab5bca2f58cef0a9eb81_3.png)

---

## PyTorch内置变换

![](img/6d807e3e238aab5bca2f58cef0a9eb81_5.png)

以下是PyTorch官方文档中提供的一些主要变换类别，你可以通过[此链接](https://pytorch.org/vision/stable/transforms.html)查看完整列表。

![](img/6d807e3e238aab5bca2f58cef0a9eb81_7.png)

*   **图像变换**：例如`RandomCrop`（随机裁剪）、`Grayscale`（灰度化）、`Pad`（填充）。
*   **张量变换**：例如`LinearTransformation`（线性变换）、`Normalize`（归一化）。
*   **转换变换**：例如`ToPILImage`（转换为PIL图像）、`ToTensor`（转换为张量）。
*   **通用变换**：例如`Lambda`（使用lambda函数）或编写自定义类。
*   **组合变换**：使用`transforms.Compose`将多个变换按顺序组合应用。

![](img/6d807e3e238aab5bca2f58cef0a9eb81_8.png)

---

## 扩展自定义数据集以支持变换

在上一教程中，我们实现了一个自定义的葡萄酒数据集。现在，让我们扩展这个类，使其支持`transform`参数。

![](img/6d807e3e238aab5bca2f58cef0a9eb81_10.png)

首先，我们需要修改`__init__`方法以接收一个可选的`transform`参数，并将其保存为实例变量。

```python
def __init__(self, transform=None):
    # ... 原有的数据加载代码 ...
    self.transform = transform
```

接着，在`__getitem__`方法中，我们需要在返回数据样本之前应用变换（如果提供了的话）。

![](img/6d807e3e238aab5bca2f58cef0a9eb81_12.png)

![](img/6d807e3e238aab5bca2f58cef0a9eb81_13.png)

![](img/6d807e3e238aab5bca2f58cef0a9eb81_14.png)

```python
def __getitem__(self, index):
    sample = self.features[index], self.labels[index]
    if self.transform:
        sample = self.transform(sample)
    return sample
```

这样，我们的自定义数据集就具备了应用变换的能力。

---

![](img/6d807e3e238aab5bca2f58cef0a9eb81_16.png)

![](img/6d807e3e238aab5bca2f58cef0a9eb81_18.png)

## 创建自定义变换类

![](img/6d807e3e238aab5bca2f58cef0a9eb81_20.png)

一个自定义变换类本质上是一个可调用对象。它需要实现`__call__`方法，该方法接收一个数据样本并返回变换后的样本。

### 示例1：转换为张量（ToTensor）

以下是我们创建的一个`ToTensor`变换类，它将NumPy数组转换为PyTorch张量。

```python
class ToTensor:
    def __call__(self, sample):
        inputs, targets = sample
        return torch.from_numpy(inputs), torch.from_numpy(targets)
```

![](img/6d807e3e238aab5bca2f58cef0a9eb81_22.png)

![](img/6d807e3e238aab5bca2f58cef0a9eb81_23.png)

![](img/6d807e3e238aab5bca2f58cef0a9eb81_24.png)

![](img/6d807e3e238aab5bca2f58cef0a9eb81_25.png)

现在，我们可以在创建数据集时使用这个变换。

```python
dataset = WineDataset(transform=ToTensor())
first_data = dataset[0]
features, labels = first_data
print(type(features))  # 输出: <class 'torch.Tensor'>
```

![](img/6d807e3e238aab5bca2f58cef0a9eb81_27.png)

### 示例2：乘法变换（MulTransform）

![](img/6d807e3e238aab5bca2f58cef0a9eb81_29.png)

我们还可以创建更复杂的变换，例如一个将特征值乘以指定因子的变换。

![](img/6d807e3e238aab5bca2f58cef0a9eb81_31.png)

```python
class MulTransform:
    def __init__(self, factor):
        self.factor = factor

    def __call__(self, sample):
        inputs, targets = sample
        inputs *= self.factor
        return inputs, targets
```

![](img/6d807e3e238aab5bca2f58cef0a9eb81_33.png)

---

![](img/6d807e3e238aab5bca2f58cef0a9eb81_35.png)

## 组合多个变换

![](img/6d807e3e238aab5bca2f58cef0a9eb81_37.png)

![](img/6d807e3e238aab5bca2f58cef0a9eb81_39.png)

在实际应用中，我们经常需要按顺序应用多个变换。PyTorch提供了`transforms.Compose`来简化这个过程。

以下是使用`Compose`组合`ToTensor`和`MulTransform`的示例。

![](img/6d807e3e238aab5bca2f58cef0a9eb81_41.png)

![](img/6d807e3e238aab5bca2f58cef0a9eb81_42.png)

```python
composed = torchvision.transforms.Compose([
    ToTensor(),
    MulTransform(factor=2)
])

![](img/6d807e3e238aab5bca2f58cef0a9eb81_44.png)

![](img/6d807e3e238aab5bca2f58cef0a9eb81_46.png)

dataset = WineDataset(transform=composed)
first_data = dataset[0]
features, labels = first_data
print(features)  # 输出: 所有特征值都已翻倍并转换为张量
```

![](img/6d807e3e238aab5bca2f58cef0a9eb81_48.png)

通过更改`MulTransform`的因子（例如改为4），我们可以验证变换是否按预期工作。

![](img/6d807e3e238aab5bca2f58cef0a9eb81_50.png)

---

![](img/6d807e3e238aab5bca2f58cef0a9eb81_52.png)

![](img/6d807e3e238aab5bca2f58cef0a9eb81_53.png)

## 总结

![](img/6d807e3e238aab5bca2f58cef0a9eb81_55.png)

本节课中我们一起学习了PyTorch数据集变换的核心概念。我们掌握了如何修改自定义`Dataset`类以支持变换参数，如何编写实现`__call__`方法的自定义变换类，以及如何使用`transforms.Compose`来顺序组合多个变换。

![](img/6d807e3e238aab5bca2f58cef0a9eb81_57.png)

变换是数据预处理流水线中至关重要的一环，它使得数据准备过程更加模块化和灵活。在处理图像等复杂数据时，合理使用内置和自定义变换能极大地提升模型的训练效果和泛化能力。建议你多多查阅官方文档，了解更多的内置变换及其用法。