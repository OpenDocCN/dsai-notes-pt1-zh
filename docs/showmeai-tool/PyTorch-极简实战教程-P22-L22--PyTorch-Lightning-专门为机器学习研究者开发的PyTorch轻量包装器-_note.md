# PyTorch 极简实战教程！P22：L22- PyTorch Lightning：为研究者设计的PyTorch轻量包装器 🚀

在本节课中，我们将学习 PyTorch Lightning。这是一个专门为机器学习研究者开发的 PyTorch 轻量级包装器，旨在通过减少样板代码来加速算法实现和模型改进。我们将通过将一个标准的 PyTorch 训练脚本转换为 Lightning 格式，来直观感受其工作原理和优势。

## 概述与优势 ✨

PyTorch Lightning 是一个开源框架，你可以在 GitHub 上找到它，其官方网站也提供了完善的文档。它抽象了许多繁琐的细节，让你可以更专注于模型和研究本身。

以下是使用 PyTorch Lightning 后，你不再需要手动处理的一些事项：
*   你无需手动设置模型为训练或评估模式（`model.train()` / `model.eval()`）。
*   你无需操心设备（CPU/GPU）的切换以及将模型和张量移动到对应设备。
*   你无需手动执行梯度清零、反向传播和优化器更新步骤（`optimizer.zero_grad()`, `loss.backward()`, `optimizer.step()`）。
*   你无需在推理时使用 `torch.no_grad()` 或 `detach()`。
*   作为额外奖励，它还集成了 TensorBoard 支持，并能打印有用的警告和机器学习提示来帮助你优化代码。

虽然我通常对过度抽象的框架持谨慎态度，但 PyTorch Lightning 确实值得推荐。不过，我仍然建议你先掌握 PyTorch 基础知识。如果你已经熟悉 PyTorch，那么可以尝试使用 Lightning 来提升开发效率。

## 环境准备与安装 ⚙️

![](img/8f1dbba751e44013b8cee2c7d55082c5_3.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_4.png)

我们将通过一个实例来学习：将我的 PyTorch 教程第13课（一个在 MNIST 数据集上进行数字分类的简单前馈神经网络）的代码转换为 PyTorch Lightning 格式。

![](img/8f1dbba751e44013b8cee2c7d55082c5_5.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_6.png)

首先，你需要安装 PyTorch Lightning。有两种常见方式：

![](img/8f1dbba751e44013b8cee2c7d55082c5_8.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_9.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_10.png)

使用 pip 安装：
```bash
pip install pytorch-lightning
```
使用 conda 安装：
```bash
conda install pytorch-lightning -c conda-forge
```

![](img/8f1dbba751e44013b8cee2c7d55082c5_12.png)

安装完成后，我们就可以开始编写代码了。

## 构建 Lightning 模型 🏗️

![](img/8f1dbba751e44013b8cee2c7d55082c5_14.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_15.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_16.png)

上一节我们介绍了 PyTorch Lightning 的优势，本节中我们来看看如何构建一个 Lightning 模型。核心是将原有的 `nn.Module` 模型类转换为继承自 `pl.LightningModule`。

![](img/8f1dbba751e44013b8cee2c7d55082c5_17.png)

首先，导入必要的库，包括新引入的 `pytorch_lightning`：

![](img/8f1dbba751e44013b8cee2c7d55082c5_19.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_20.png)

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
from torch.utils.data import DataLoader, random_split
from torchvision.datasets import MNIST
from torchvision import transforms
import pytorch_lightning as pl
```

接下来，定义我们的 Lightning 模型。我们复制原有模型的架构和超参数，但将父类改为 `pl.LightningModule`：

![](img/8f1dbba751e44013b8cee2c7d55082c5_22.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_23.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_24.png)

```python
class LitNeuralNet(pl.LightningModule):
    def __init__(self, input_size, hidden_size, num_classes):
        super(LitNeuralNet, self).__init__()
        self.l1 = nn.Linear(input_size, hidden_size)
        self.relu = nn.ReLU()
        self.l2 = nn.Linear(hidden_size, num_classes)

    def forward(self, x):
        out = self.l1(x)
        out = self.relu(out)
        out = self.l2(out)
        return out
```

![](img/8f1dbba751e44013b8cee2c7d55082c5_26.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_28.png)

`__init__` 和 `forward` 函数与标准 PyTorch 模型完全一致。Lightning 的魔力来自于我们需要额外实现的几个特定方法。

## 配置优化器与训练步骤 ⚡

![](img/8f1dbba751e44013b8cee2c7d55082c5_30.png)

现在，我们需要告诉 Lightning 如何训练我们的模型。这通过实现 `configure_optimizers` 和 `training_step` 方法来完成。

![](img/8f1dbba751e44013b8cee2c7d55082c5_32.png)

`configure_optimizers` 方法非常简单，只需返回你想要的优化器即可：

```python
    def configure_optimizers(self):
        # 假设学习率定义在模型的超参数中，例如 self.hparams.lr
        optimizer = torch.optim.Adam(self.parameters(), lr=1e-3)
        return optimizer
```

![](img/8f1dbba751e44013b8cee2c7d55082c5_34.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_35.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_36.png)

`training_step` 方法定义了单个批次的训练逻辑。Lightning 会自动处理循环、梯度清零和优化器更新：

![](img/8f1dbba751e44013b8cee2c7d55082c5_37.png)

```python
    def training_step(self, batch, batch_idx):
        # 1. 解包数据
        images, labels = batch
        images = images.reshape(-1, 28*28)

        # 2. 前向传播
        outputs = self(images)

        # 3. 计算损失
        loss = F.cross_entropy(outputs, labels)

        # 4. 记录日志（可选，用于TensorBoard等）
        self.log(‘train_loss‘, loss)
        return loss
```

注意，我们不再需要手动将数据移动到设备（如 `.to(device)`），Lightning 会自动处理。`self.log` 方法用于记录指标，方便后续监控。

## 准备数据加载器 📊

模型和训练逻辑定义好后，我们需要提供数据。这是通过实现 `train_dataloader` 方法（以及可选的 `val_dataloader`, `test_dataloader`）来完成的。

以下是 `train_dataloader` 的实现：

```python
    def train_dataloader(self):
        # 定义训练数据集
        train_dataset = MNIST(root=‘./data‘,
                              train=True,
                              transform=transforms.ToTensor(),
                              download=True)
        # 创建并返回数据加载器
        train_loader = DataLoader(dataset=train_dataset,
                                  batch_size=100,
                                  shuffle=True,
                                  num_workers=4) # Lightning提示我们增加workers以加速
        return train_loader
```

至此，一个具备基本训练功能的 Lightning 模型就构建完成了。

## 初始化训练器并开始训练 🚂

上一节我们完成了模型和数据加载器的定义，本节中我们来看看如何启动训练。这是通过创建 `Trainer` 对象并调用其 `fit` 方法实现的。

![](img/8f1dbba751e44013b8cee2c7d55082c5_39.png)

首先，在脚本的主程序中，我们初始化模型和训练器：

![](img/8f1dbba751e44013b8cee2c7d55082c5_41.png)

```python
if __name__ == ‘__main__‘:
    # 超参数
    input_size = 784
    hidden_size = 500
    num_classes = 10

    # 初始化模型
    model = LitNeuralNet(input_size, hidden_size, num_classes)

    # 初始化训练器
    trainer = pl.Trainer(max_epochs=5, fast_dev_run=False)
    # fast_dev_run=True 可用于快速调试，只跑一个batch

    # 开始训练！
    trainer.fit(model)
```

![](img/8f1dbba751e44013b8cee2c7d55082c5_43.png)

运行这段代码，你将看到一个清晰的进度条，显示训练轮次和损失值。Lightning 自动处理了设备分配、训练循环和日志记录。

![](img/8f1dbba751e44013b8cee2c7d55082c5_45.png)

## 添加验证与测试循环 🔍

![](img/8f1dbba751e44013b8cee2c7d55082c5_46.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_47.png)

一个完整的流程通常包含验证和测试。在 Lightning 中，添加它们非常简单，只需实现对应的方法。

以下是添加验证循环的步骤：

![](img/8f1dbba751e44013b8cee2c7d55082c5_49.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_50.png)

首先，实现 `val_dataloader` 方法来提供验证数据：

```python
    def val_dataloader(self):
        # 通常你会从完整数据集中划分出一部分作为验证集
        # 此处为示例，我们使用MNIST测试集作为验证集
        val_dataset = MNIST(root=‘./data‘,
                            train=False,
                            transform=transforms.ToTensor(),
                            download=True)
        val_loader = DataLoader(dataset=val_dataset,
                                batch_size=100,
                                shuffle=False, # 验证集不应打乱
                                num_workers=4)
        return val_loader
```

然后，实现 `validation_step` 方法定义验证逻辑（通常与 `training_step` 类似，但不进行反向传播）：

```python
    def validation_step(self, batch, batch_idx):
        images, labels = batch
        images = images.reshape(-1, 28*28)
        outputs = self(images)
        loss = F.cross_entropy(outputs, labels)
        self.log(‘val_loss‘, loss)
        return loss
```

![](img/8f1dbba751e44013b8cee2c7d55082c5_52.png)

你还可以实现 `validation_epoch_end` 方法，在每个验证周期结束后计算平均损失等指标：

```python
    def validation_epoch_end(self, validation_step_outputs):
        # validation_step_outputs 是所有validation_step返回值的列表
        avg_loss = torch.stack(validation_step_outputs).mean()
        self.log(‘avg_val_loss‘, avg_loss)
```

![](img/8f1dbba751e44013b8cee2c7d55082c5_54.png)

类似地，你可以添加 `test_dataloader` 和 `test_step` 来定义测试循环。完成后，训练器会自动在每轮训练后运行验证，并在训练结束后使用 `trainer.test()` 运行测试。

## 使用高级功能与TensorBoard集成 📈

![](img/8f1dbba751e44013b8cee2c7d55082c5_56.png)

PyTorch Lightning 的 `Trainer` 提供了大量高级功能，只需简单设置参数即可启用。

以下是一些有用的训练器参数示例：

![](img/8f1dbba751e44013b8cee2c7d55082c5_58.png)

*   **GPU训练**：`trainer = pl.Trainer(gpus=1)` 使用1块GPU。
*   **多GPU/TPU训练**：`gpus=2` 或 `tpu_cores=8`，轻松扩展。
*   **16位混合精度训练**：`precision=16`，可以加速训练并减少显存占用。
*   **梯度裁剪**：`gradient_clip_val=0.5`，防止梯度爆炸。
*   **自动学习率查找**：`auto_lr_find=True`，Trainer 会尝试寻找一个较好的初始学习率。
*   **确定性训练**：`deterministic=True`，使结果可复现。

此外，Lightning 内置了 TensorBoard 日志记录。我们在 `training_step` 和 `validation_epoch_end` 中使用的 `self.log` 方法会自动将指标记录到 `lightning_logs` 目录中。

你可以通过以下命令启动 TensorBoard 来可视化训练过程：

```bash
tensorboard --logdir=./lightning_logs
```

然后在浏览器中打开提供的地址，即可查看损失曲线等指标图表。

![](img/8f1dbba751e44013b8cee2c7d55082c5_60.png)

## 总结 🎯

本节课中我们一起学习了 PyTorch Lightning 的核心用法。我们了解到，Lightning 通过将模型 (`LightningModule`) 与训练逻辑 (`Trainer`) 分离，极大地减少了样板代码。我们实践了如何：
1.  将 `nn.Module` 转换为 `LightningModule`。
2.  实现 `configure_optimizers`、`training_step` 来定义训练过程。
3.  实现 `train_dataloader`、`val_dataloader` 来提供数据。
4.  使用 `Trainer` 对象来灵活、高效地管理训练流程。
5.  利用内置的日志和高级功能（如多GPU训练、自动精度）来提升开发体验。

![](img/8f1dbba751e44013b8cee2c7d55082c5_62.png)

![](img/8f1dbba751e44013b8cee2c7d55082c5_63.png)

PyTorch Lightning 让你能更快速地进行实验和迭代，同时保持代码的整洁和可读性。如果你经常进行模型研究，它无疑是一个强大的工具。