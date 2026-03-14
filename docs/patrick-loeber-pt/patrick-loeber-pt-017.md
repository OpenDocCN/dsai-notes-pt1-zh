# 017：保存与加载模型 💾

在本节课中，我们将学习如何在PyTorch中保存和加载训练好的模型。这是将模型部署到生产环境或中断后恢复训练的关键技能。

我们将介绍三种核心方法，并详细解释每种方法的适用场景和注意事项，特别是使用GPU时的关键点。

---

## 概述与核心方法

保存和加载模型主要涉及三个核心方法：
*   `torch.save`
*   `torch.load`
*   `model.load_state_dict`

`torch.save` 函数可以保存张量、模型或任何字典。它利用Python的pickle模块将对象序列化后保存，因此生成的文件不是人类可读的。

---

## 保存模型的两种方式

上一节我们介绍了核心方法，本节中我们来看看保存模型的具体选项。

### 方式一：保存整个模型（简易方法）

第一种方式是直接保存整个模型对象。

![](img/c885dfe548f252ed653964b1bd0374b3_1.png)

```python
# 保存整个模型
torch.save(model, ‘model.pth’)

# 加载整个模型
model = torch.load(‘model.pth’)
model.eval()
```

这种方式非常简便，但有一个缺点：序列化的数据与保存时使用的特定类以及确切的目录结构绑定。如果源代码结构发生变化，加载可能会失败。

![](img/c885dfe548f252ed653964b1bd0374b3_3.png)

### 方式二：仅保存模型参数（推荐方法）

![](img/c885dfe548f252ed653964b1bd0374b3_4.png)

![](img/c885dfe548f252ed653964b1bd0374b3_5.png)

![](img/c885dfe548f252ed653964b1bd0374b3_7.png)

如果我们的目标只是保存训练好的模型并在之后用于推理，那么仅保存模型参数就足够了。

![](img/c885dfe548f252ed653964b1bd0374b3_9.png)

```python
# 仅保存模型的状态字典（包含参数）
torch.save(model.state_dict(), ‘model_state_dict.pth’)

![](img/c885dfe548f252ed653964b1bd0374b3_11.png)

# 加载时，需要先创建模型结构，再加载参数
loaded_model = ModelClass() # 先实例化模型结构
loaded_model.load_state_dict(torch.load(‘model_state_dict.pth’))
loaded_model.eval()
```

这是PyTorch官方推荐的方式，因为它只保存核心参数，不依赖于原始类定义，更加灵活和稳定。

![](img/c885dfe548f252ed653964b1bd0374b3_13.png)

---

## 代码实践演示

让我们通过代码来具体看看这两种方式如何操作。

![](img/c885dfe548f252ed653964b1bd0374b3_15.png)

首先，我们定义一个简单的模型：

![](img/c885dfe548f252ed653964b1bd0374b3_16.png)

```python
import torch
import torch.nn as nn

![](img/c885dfe548f252ed653964b1bd0374b3_18.png)

class SimpleModel(nn.Module):
    def __init__(self, input_size):
        super(SimpleModel, self).__init__()
        self.linear = nn.Linear(input_size, 1)

    def forward(self, x):
        return self.linear(x)

# 创建模型实例
model = SimpleModel(6)
```

![](img/c885dfe548f252ed653964b1bd0374b3_20.png)

### 实践1：保存与加载整个模型

![](img/c885dfe548f252ed653964b1bd0374b3_22.png)

以下是使用“简易方法”的完整代码示例：

![](img/c885dfe548f252ed653964b1bd0374b3_24.png)

![](img/c885dfe548f252ed653964b1bd0374b3_25.png)

```python
# 保存整个模型
file_name = ‘model.pth’
torch.save(model, file_name)

# 加载整个模型
loaded_model = torch.load(file_name)
loaded_model.eval()

# 检查参数
for param in loaded_model.parameters():
    print(param)
```

![](img/c885dfe548f252ed653964b1bd0374b3_27.png)

![](img/c885dfe548f252ed653964b1bd0374b3_28.png)

![](img/c885dfe548f252ed653964b1bd0374b3_29.png)

运行后，你会看到一个名为 `model.pth` 的文件，其内容是序列化的二进制数据。

### 实践2：保存与加载状态字典

![](img/c885dfe548f252ed653964b1bd0374b3_31.png)

![](img/c885dfe548f252ed653964b1bd0374b3_32.png)

现在，让我们使用推荐的方法：

```python
# 保存模型的状态字典
state_dict_file = ‘model_state_dict.pth’
torch.save(model.state_dict(), state_dict_file)

# 加载模型的状态字典
loaded_model = SimpleModel(6) # 必须重新实例化模型结构
loaded_model.load_state_dict(torch.load(state_dict_file))
loaded_model.eval()

# 打印参数以验证
print(“Original model parameters:“)
for param in model.parameters():
    print(param)
print(“\nLoaded model parameters:“)
for param in loaded_model.parameters():
    print(param)
```

你会看到两个模型的参数完全相同，说明加载成功。你可以通过 `print(model.state_dict())` 查看状态字典的具体内容，它是一个包含权重和偏置张量的字典。

---

![](img/c885dfe548f252ed653964b1bd0374b3_34.png)

## 保存训练检查点

在模型训练过程中，我们经常需要保存检查点，以便从特定阶段恢复训练。这通常需要保存更多信息。

由于 `torch.save` 可以保存任何字典，我们可以轻松地创建一个包含多种信息的检查点。

![](img/c885dfe548f252ed653964b1bd0374b3_36.png)

以下是创建和加载训练检查点的示例：

![](img/c885dfe548f252ed653964b1bd0374b3_38.png)

```python
# 假设我们有一些训练组件
learning_rate = 0.001
optimizer = torch.optim.SGD(model.parameters(), lr=learning_rate)
epoch = 90

![](img/c885dfe548f252ed653964b1bd0374b3_40.png)

# 创建一个检查点字典
checkpoint = {
    ‘epoch’: epoch,
    ‘model_state_dict’: model.state_dict(),
    ‘optimizer_state_dict’: optimizer.state_dict(),
}

![](img/c885dfe548f252ed653964b1bd0374b3_41.png)

![](img/c885dfe548f252ed653964b1bd0374b3_43.png)

# 保存检查点
torch.save(checkpoint, ‘checkpoint.pth’)

![](img/c885dfe548f252ed653964b1bd0374b3_45.png)

# --- 加载检查点 ---
loaded_checkpoint = torch.load(‘checkpoint.pth’)

![](img/c885dfe548f252ed653964b1bd0374b3_47.png)

# 提取信息
epoch = loaded_checkpoint[‘epoch’]

# 重新初始化模型和优化器（结构必须一致）
loaded_model = SimpleModel(6)
loaded_optimizer = torch.optim.SGD(loaded_model.parameters(), lr=0) # 学习率会被覆盖

# 加载状态
loaded_model.load_state_dict(loaded_checkpoint[‘model_state_dict’])
loaded_optimizer.load_state_dict(loaded_checkpoint[‘optimizer_state_dict’])

![](img/c885dfe548f252ed653964b1bd0374b3_49.png)

![](img/c885dfe548f252ed653964b1bd0374b3_50.png)

# 验证优化器状态（例如学习率）已正确加载
print(loaded_optimizer.state_dict())
```

![](img/c885dfe548f252ed653964b1bd0374b3_51.png)

通过这种方式，你可以完美地从第90个epoch恢复模型和优化器的状态，继续训练。

---

![](img/c885dfe548f252ed653964b1bd0374b3_53.png)

## GPU与CPU设备处理

当使用GPU进行训练时，保存和加载模型需要注意设备映射问题。

![](img/c885dfe548f252ed653964b1bd0374b3_55.png)

![](img/c885dfe548f252ed653964b1bd0374b3_56.png)

以下是几种常见场景的处理方法：

![](img/c885dfe548f252ed653964b1bd0374b3_57.png)

**1. 在GPU上保存，在CPU上加载**

```python
# 保存（在GPU上）
device = torch.device(‘cuda’)
model.to(device)
torch.save(model.state_dict(), ‘model_gpu.pth’)

# 加载（到CPU上）
device = torch.device(‘cpu’)
loaded_model = SimpleModel(6)
loaded_model.load_state_dict(torch.load(‘model_gpu.pth’, map_location=device))
```

![](img/c885dfe548f252ed653964b1bd0374b3_59.png)

![](img/c885dfe548f252ed653964b1bd0374b3_60.png)

**2. 在GPU上保存，在GPU上加载**

![](img/c885dfe548f252ed653964b1bd0374b3_61.png)

```python
# 保存（在GPU上）
device = torch.device(‘cuda’)
model.to(device)
torch.save(model.state_dict(), ‘model_gpu.pth’)

# 加载（到GPU上）
loaded_model = SimpleModel(6)
loaded_model.load_state_dict(torch.load(‘model_gpu.pth’))
loaded_model.to(device) # 将模型送到GPU
```

**3. 在CPU上保存，在GPU上加载**

```python
# 保存（在CPU上，默认）
torch.save(model.state_dict(), ‘model_cpu.pth’)

![](img/c885dfe548f252ed653964b1bd0374b3_63.png)

# 加载（到GPU上）
device = torch.device(‘cuda’)
loaded_model = SimpleModel(6)
# 指定map_location，将存储的张量映射到GPU
loaded_model.load_state_dict(torch.load(‘model_cpu.pth’, map_location=‘cuda:0’))
loaded_model.to(device) # 将模型送到GPU
```

![](img/c885dfe548f252ed653964b1bd0374b3_64.png)

请注意，当使用GPU进行推理时，别忘了将输入数据也发送到GPU设备上：`data = data.to(device)`。

---

## 总结

![](img/c885dfe548f252ed653964b1bd0374b3_66.png)

![](img/c885dfe548f252ed653964b1bd0374b3_67.png)

![](img/c885dfe548f252ed653964b1bd0374b3_68.png)

本节课中我们一起学习了PyTorch中保存和加载模型的核心知识。

![](img/c885dfe548f252ed653964b1bd0374b3_70.png)

我们掌握了三种关键方法：`torch.save`、`torch.load` 和 `model.load_state_dict`。我们比较了保存整个模型和仅保存模型状态字典两种方式的优缺点，并明确了推荐使用后者的原因。我们还学习了如何创建包含模型、优化器和训练进度信息的完整检查点，这对于长时间训练任务至关重要。最后，我们详细探讨了在不同设备（CPU/GPU）之间迁移模型时需要注意的 `map_location` 参数设置。

![](img/c885dfe548f252ed653964b1bd0374b3_72.png)

记住推荐的工作流程：使用 `torch.save(model.state_dict(), PATH)` 保存，使用 `model.load_state_dict(torch.load(PATH))` 加载，并在加载后调用 `model.eval()` 将模型设置为评估模式。