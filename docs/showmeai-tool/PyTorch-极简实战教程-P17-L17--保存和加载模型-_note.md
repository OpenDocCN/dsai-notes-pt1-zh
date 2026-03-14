# PyTorch 极简实战教程！P17：L17- 保存和加载模型 💾

在本节课中，我们将要学习如何在 PyTorch 中保存和加载训练好的模型。这是模型部署和继续训练的关键步骤。我们将介绍三种核心方法，并讨论在不同设备（CPU/GPU）间迁移模型时的注意事项。

## 概述 📋

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_1.png)

保存和加载模型是机器学习工作流中至关重要的一环。它允许我们复用训练成果、部署模型，或在训练中断后恢复训练。PyTorch 为此提供了灵活且强大的工具。

## 核心方法与概念

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_3.png)

你必须记住以下三种核心方法：
*   `torch.save()`: 用于保存模型或状态字典。
*   `torch.load()`: 用于加载保存的文件。
*   `model.load_state_dict()`: 将加载的状态字典加载到模型结构中。

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_5.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_6.png)

`torch.save()` 可以保存任何字典对象，这为我们保存模型参数、优化器状态等提供了便利。它使用 Python 的 pickle 模块进行序列化，生成的文件是二进制格式，不可直接阅读。

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_7.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_8.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_10.png)

## 方法一：保存整个模型（“懒惰”方法）

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_12.png)

第一种方法是直接保存整个模型对象。这是一种快速但不够灵活的方式。

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_14.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_16.png)

以下是具体步骤：
1.  **保存模型**：使用 `torch.save(model, file_path)`。
2.  **加载模型**：使用 `model = torch.load(file_path)`。
3.  **设置模式**：加载后，调用 `model.eval()` 将模型设置为评估模式。

**示例代码**：
```python
# 保存整个模型
torch.save(model, ‘model.pth’)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_18.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_19.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_21.png)

# 加载整个模型
model = torch.load(‘model.pth’)
model.eval()
```

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_22.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_24.png)

这种方法的缺点是序列化数据与保存时使用的特定类定义和目录结构紧密绑定。如果源代码结构发生变化，可能导致加载失败。

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_26.png)

## 方法二：保存状态字典（推荐方法）

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_28.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_29.png)

上一节我们介绍了保存整个模型的方法，本节中我们来看看更推荐的方式：仅保存模型的**状态字典**。状态字典是一个Python字典对象，它将每一层映射到其参数张量。

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_31.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_32.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_33.png)

以下是具体步骤：
1.  **保存状态字典**：使用 `torch.save(model.state_dict(), file_path)`。
2.  **加载状态字典**：
    *   首先，重新实例化模型结构。
    *   然后，使用 `model.load_state_dict(torch.load(file_path))` 加载参数。
3.  **设置模式**：调用 `model.eval()`。

**示例代码**：
```python
# 保存模型的状态字典（参数）
torch.save(model.state_dict(), ‘model_state_dict.pth’)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_35.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_36.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_37.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_38.png)

# 加载模型的状态字典
# 1. 重新创建模型结构
loaded_model = ModelClass(input_features=6)
# 2. 加载参数
loaded_model.load_state_dict(torch.load(‘model_state_dict.pth’))
# 3. 设置为评估模式
loaded_model.eval()
```

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_41.png)

这是首选的保存方式，因为它只保存模型参数，与模型类定义解耦，更加灵活和安全。你可以通过 `print(model.state_dict())` 查看状态字典的内容。

## 方法三：保存训练检查点

在训练过程中，我们经常需要保存检查点，以便从特定阶段恢复训练。这通常包括模型参数、优化器状态和当前的训练轮次等信息。

以下是创建一个检查点字典并保存的步骤：
1.  构建一个字典，包含你想保存的所有信息。
2.  使用 `torch.save()` 保存这个字典。

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_45.png)

**示例代码**：
```python
# 定义检查点内容
checkpoint = {
    ‘epoch’: 90,
    ‘model_state_dict’: model.state_dict(),
    ‘optimizer_state_dict’: optimizer.state_dict(),
    ‘loss’: loss,
    # … 可以保存其他任何信息
}

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_47.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_49.png)

# 保存检查点
torch.save(checkpoint, ‘checkpoint.pth’)
```

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_51.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_52.png)

加载检查点时，你需要按照保存时的结构重新初始化模型和优化器，然后加载对应的状态。

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_54.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_56.png)

**示例代码**：
```python
# 加载检查点
checkpoint = torch.load(‘checkpoint.pth’)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_58.png)

# 恢复模型
model = ModelClass(input_features=6)
model.load_state_dict(checkpoint[‘model_state_dict’])
model.eval() # 或 model.train()，取决于你是否要继续训练

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_60.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_61.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_62.png)

# 恢复优化器
optimizer = torch.optim.SGD(model.parameters(), lr=0) # 学习率会被覆盖
optimizer.load_state_dict(checkpoint[‘optimizer_state_dict’])

# 恢复其他信息
epoch = checkpoint[‘epoch’]
loss = checkpoint[‘loss’]
```

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_64.png)

## 跨设备加载的注意事项

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_66.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_67.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_68.png)

当你在不同设备（如GPU和CPU）之间保存和加载模型时，需要特别注意张量的位置。

以下是几种常见场景的处理方法：

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_70.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_71.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_72.png)

**在GPU上保存，在CPU上加载**
加载时需要使用 `map_location` 参数将张量映射到CPU。
```python
# 假设模型在GPU上训练并保存
model.load_state_dict(torch.load(‘model_gpu.pth’, map_location=torch.device(‘cpu’)))
```

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_74.png)

**在GPU上保存和加载**
加载后，需要将模型再次送到GPU设备上。
```python
device = torch.device(‘cuda’)
model = ModelClass(...)
model.load_state_dict(torch.load(‘model_gpu.pth’))
model.to(device) # 将模型参数转移到GPU
```

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_76.png)

**在CPU上保存，在GPU上加载**
加载时需要使用 `map_location` 将张量映射到GPU，然后再将整个模型送到GPU。
```python
device = torch.device(‘cuda:0’) # 指定GPU
model = ModelClass(...)
# 加载时映射到GPU，加载后送到GPU
model.load_state_dict(torch.load(‘model_cpu.pth’, map_location=device))
model.to(device)
```

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_78.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_79.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_80.png)

无论哪种情况，进行前向传播时，都需要确保输入数据也在同一个设备上。

## 总结 🎯

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_82.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_83.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_84.png)

本节课中我们一起学习了 PyTorch 中保存和加载模型的三种核心方法：
1.  **保存整个模型**：简单但不灵活，依赖于原始代码结构。
2.  **保存状态字典**（推荐）：只保存模型参数，灵活且安全。
3.  **保存训练检查点**：用于保存训练状态，便于恢复训练。

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_86.png)

![](img/d077e5bf8acaccfa2f2d408a7bb4ae8d_88.png)

我们还探讨了在不同计算设备（CPU/GPU）间迁移模型时，如何使用 `map_location` 参数正确处理张量位置。掌握这些技能，你将能够有效地管理模型的生命周期，为模型部署和持续训练打下坚实基础。