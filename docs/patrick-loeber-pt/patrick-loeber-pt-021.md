# 021：PyTorch Lightning教程 - 机器学习研究者的轻量级PyTorch封装 🚀

在本节课中，我们将学习PyTorch Lightning。这是一个旨在减少样板代码的轻量级PyTorch封装框架，它能让算法实现和模型优化过程变得更快。你无需再记忆PyTorch框架的所有繁琐细节，因为Lightning会为你处理这些。此外，它还能在你犯错时打印警告并提供有用的机器学习提示。

## 概述

![](img/9bfd815daaf2c68d52dc66079efb45cc_1.png)

PyTorch Lightning是一个开源项目，你可以在GitHub上找到它，其官方网站也提供了详尽的入门文档。使用Lightning后，你将不再需要操心以下细节：
*   设置模型为训练或评估模式。
*   为GPU支持处理设备，并将模型和张量推送到设备。
*   手动调用`zero_grad()`、`backward()`和优化器的`step()`。
*   使用`torch.no_grad()`或`detach()`。
*   作为额外福利，它还集成了TensorBoard支持，并能打印有用的提示。

上一节我们介绍了PyTorch Lightning的基本概念和优势，本节中我们来看看如何将一个标准的PyTorch项目转换为使用Lightning。

## 从PyTorch代码转换

我们将以我的PyTorch教程中的一个简单前馈神经网络为例，该网络应用于MNIST数据集进行数字分类。首先，你需要安装PyTorch Lightning。

![](img/9bfd815daaf2c68d52dc66079efb45cc_3.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_4.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_5.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_6.png)

以下是安装PyTorch Lightning的两种常见方式：
*   使用Pip：`pip install pytorch-lightning`
*   使用Conda：`conda install pytorch-lightning -c conda-forge`

![](img/9bfd815daaf2c68d52dc66079efb45cc_8.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_9.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_10.png)

安装完成后，我们开始转换代码。首先导入必要的库，并额外导入`pytorch_lightning`。

![](img/9bfd815daaf2c68d52dc66079efb45cc_12.png)

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
from torch.utils.data import DataLoader, random_split
from torchvision.datasets import MNIST
from torchvision import transforms
import pytorch_lightning as pl
```

![](img/9bfd815daaf2c68d52dc66079efb45cc_14.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_15.png)

## 定义Lightning模型

![](img/9bfd815daaf2c68d52dc66079efb45cc_16.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_17.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_18.png)

接下来，我们将原始的PyTorch模型转换为Lightning模型。关键变化是让模型类继承自`pl.LightningModule`，而不是`nn.Module`。

![](img/9bfd815daaf2c68d52dc66079efb45cc_20.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_21.png)

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

![](img/9bfd815daaf2c68d52dc66079efb45cc_23.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_24.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_25.png)

定义超参数：

![](img/9bfd815daaf2c68d52dc66079efb45cc_27.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_29.png)

```python
input_size = 784
hidden_size = 500
num_classes = 10
num_epochs = 2
batch_size = 100
learning_rate = 0.001
```

在Lightning模型中，我们需要实现几个特定的函数。以下是必须实现的核心函数：

![](img/9bfd815daaf2c68d52dc66079efb45cc_31.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_33.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_34.png)

*   **`configure_optimizers`**: 定义优化器。
*   **`training_step`**: 定义单步训练的逻辑。
*   **`train_dataloader`**: 提供训练数据加载器。

首先实现`configure_optimizers`函数，它只需返回我们配置好的优化器。

![](img/9bfd815daaf2c68d52dc66079efb45cc_36.png)

```python
    def configure_optimizers(self):
        return torch.optim.Adam(self.parameters(), lr=learning_rate)
```

![](img/9bfd815daaf2c68d52dc66079efb45cc_37.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_38.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_39.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_40.png)

接着实现`training_step`函数。这里我们执行原始训练循环中的核心操作，但无需手动处理梯度清零、反向传播和优化器更新。

```python
    def training_step(self, batch, batch_idx):
        images, labels = batch
        images = images.reshape(-1, 28*28)

        # 前向传播
        outputs = self(images)
        # 计算损失
        loss = F.cross_entropy(outputs, labels)

        # 返回一个包含损失的字典
        return {'loss': loss}
```

最后实现`train_dataloader`函数，用于设置训练数据集和数据加载器。

```python
    def train_dataloader(self):
        # MNIST数据集
        train_dataset = MNIST(root='./data',
                              train=True,
                              transform=transforms.ToTensor(),
                              download=True)
        train_loader = DataLoader(dataset=train_dataset,
                                  batch_size=batch_size,
                                  num_workers=4, # 使用4个工作进程加速数据加载
                                  shuffle=True)
        return train_loader
```

## 训练模型

现在，我们可以设置Lightning Trainer并开始训练。在开发阶段，一个有用的技巧是设置`fast_dev_run=True`，这将快速运行一个批次来测试模型是否能正常工作。

```python
if __name__ == '__main__':
    # 初始化模型
    model = LitNeuralNet(input_size, hidden_size, num_classes)
    # 设置Trainer
    trainer = pl.Trainer(fast_dev_run=True) # 快速开发运行
    # 开始训练
    trainer.fit(model)
```

运行上述代码，如果一切正常，你将看到训练开始，并且Lightning可能会给出一些优化建议，例如提示数据加载器的`num_workers`数量可能成为瓶颈。

要执行完整训练，只需在创建Trainer时指定最大训练轮数（epochs）。

```python
    trainer = pl.Trainer(max_epochs=num_epochs)
```

![](img/9bfd815daaf2c68d52dc66079efb45cc_42.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_44.png)

## 添加验证步骤

![](img/9bfd815daaf2c68d52dc66079efb45cc_46.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_47.png)

为了进行模型评估，我们需要添加验证循环。这与训练步骤类似。

首先，实现`validation_step`函数。

![](img/9bfd815daaf2c68d52dc66079efb45cc_49.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_50.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_51.png)

```python
    def validation_step(self, batch, batch_idx):
        images, labels = batch
        images = images.reshape(-1, 28*28)

        outputs = self(images)
        loss = F.cross_entropy(outputs, labels)

        # 返回验证损失
        return {'val_loss': loss}
```

通常，我们还需要在每个验证周期（epoch）结束后计算平均损失，这可以通过实现`validation_epoch_end`函数来完成。

![](img/9bfd815daaf2c68d52dc66079efb45cc_53.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_54.png)

```python
    def validation_epoch_end(self, outputs):
        # outputs 是所有validation_step返回值的列表
        avg_loss = torch.stack([x['val_loss'] for x in outputs]).mean()
        # 返回一个包含平均损失的字典
        return {'avg_val_loss': avg_loss}
```

接着，提供验证数据加载器。注意，验证数据通常不应打乱（shuffle）。

```python
    def val_dataloader(self):
        # 使用MNIST测试集作为验证集（仅为示例，理想情况应单独划分）
        val_dataset = MNIST(root='./data',
                            train=False,
                            transform=transforms.ToTensor())
        val_loader = DataLoader(dataset=val_dataset,
                                batch_size=batch_size,
                                num_workers=4,
                                shuffle=False) # 验证集不打乱
        return val_loader
```

![](img/9bfd815daaf2c68d52dc66079efb45cc_56.png)

现在，当你再次运行训练时，Trainer会自动执行验证循环。如果你在验证数据加载器中错误地设置了`shuffle=True`，Lightning会给出警告提示，帮助你遵循最佳实践。

## 高级功能与便利性

![](img/9bfd815daaf2c68d52dc66079efb45cc_58.png)

PyTorch Lightning 让许多高级功能的启用变得非常简单，只需在创建 `Trainer` 时传递相应参数即可。

以下是一些例子：
*   **使用GPU**：`trainer = pl.Trainer(gpus=1)`
*   **使用多GPU**：`trainer = pl.Trainer(gpus=2)` 或 `gpus=4`
*   **使用16位精度训练**：`trainer = pl.Trainer(precision=16)`
*   **梯度裁剪**：`trainer = pl.Trainer(gradient_clip_val=0.5)`
*   **自动寻找学习率**：`trainer = pl.Trainer(auto_lr_find=True)`
*   **确保结果可复现**：`trainer = pl.Trainer(deterministic=True)`

## TensorBoard集成

![](img/9bfd815daaf2c68d52dc66079efb45cc_60.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_61.png)

PyTorch Lightning 自动集成了 TensorBoard 日志记录。要记录训练和验证损失到 TensorBoard，我们可以在 `training_step` 和 `validation_epoch_end` 返回的字典中添加额外的键。

![](img/9bfd815daaf2c68d52dc66079efb45cc_63.png)

修改 `training_step`：

```python
    def training_step(self, batch, batch_idx):
        ...
        # 返回的字典中包含用于TensorBoard的日志
        return {'loss': loss, 'log': {'train_loss': loss}}
```

修改 `validation_epoch_end`：

```python
    def validation_epoch_end(self, outputs):
        avg_loss = torch.stack([x['val_loss'] for x in outputs]).mean()
        # 返回的字典中包含用于TensorBoard的日志
        return {'avg_val_loss': avg_loss, 'log': {'avg_val_loss': avg_loss}}
```

训练完成后，日志会自动保存在 `lightning_logs` 目录中。你可以使用以下命令启动 TensorBoard 来可视化训练过程：

![](img/9bfd815daaf2c68d52dc66079efb45cc_65.png)

```bash
tensorboard --logdir lightning_logs
```

## 总结

![](img/9bfd815daaf2c68d52dc66079efb45cc_67.png)

![](img/9bfd815daaf2c68d52dc66079efb45cc_68.png)

本节课中我们一起学习了 PyTorch Lightning 的核心用法。我们看到了如何将一个标准的 PyTorch 模型转换为 Lightning 模块，这主要通过继承 `pl.LightningModule` 并实现 `configure_optimizers`、`training_step`、`train_dataloader` 等特定方法来完成。Lightning 帮助我们省去了大量样板代码，例如手动管理设备、训练循环、梯度操作等，并提供了诸如自动 TensorBoard 集成、有用的开发提示、以及轻松启用 GPU 训练、混合精度等高级功能。这极大地简化了研究人员的开发流程，使其能更专注于模型算法本身。