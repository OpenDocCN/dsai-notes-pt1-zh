#  004：为你的ML实验使用W&B

在本节课中，我们将学习一个名为Weights & Biases的平台。在后续课程中，我们将通过改变超参数进行大量机器学习模型实验。手动记录和管理这些实验既耗时又容易出错。本节课将教你如何像专业机器学习工程师一样，系统化、高效地进行实验。

## 课程回顾

上一节我们介绍了如何构建一个简单的神经网络来对五种花卉进行分类。我们构建了两个模型：一个线性模型和一个带有一个隐藏层的神经网络。我们发现，虽然神经网络的损失比线性模型低了一个数量级，但验证准确率提升有限，并且出现了过拟合的迹象。

我们讨论了几个可以调整的超参数，例如：
*   学习率
*   训练轮数
*   批量大小
*   隐藏层节点数
*   图像尺寸

在之前的实验中，我们通过手动复制代码、修改参数、并行运行多个Colab实例并手动记录结果的方式来尝试不同的超参数组合。这种方法效率低下、容易出错且难以扩展。

![](img/6809f64637cdb7d6cac1703eb5ceaa16_1.png)

## 引入Weights & Biases

![](img/6809f64637cdb7d6cac1703eb5ceaa16_3.png)

本节中，我们来看看如何利用Weights & Biases平台来优化我们的实验流程。Weights & Biases是一个用于跟踪、可视化和比较机器学习实验的平台。它可以帮助我们系统化地管理超参数、记录指标并比较不同实验的结果。

![](img/6809f64637cdb7d6cac1703eb5ceaa16_5.png)

![](img/6809f64637cdb7d6cac1703eb5ceaa16_6.png)

![](img/6809f64637cdb7d6cac1703eb5ceaa16_7.png)

使用W&B的主要优势包括：
*   **自动化记录**：自动记录超参数、损失、准确率等指标。
*   **可视化比较**：在统一的仪表板中可视化比较不同实验的结果。
*   **实验管理**：轻松管理、组织和复现大量实验。
*   **协作共享**：方便地与团队成员分享实验过程和结果。

## 核心概念与代码集成

![](img/6809f64637cdb7d6cac1703eb5ceaa16_9.png)

![](img/6809f64637cdb7d6cac1703eb5ceaa16_11.png)

要使用Weights & Biases，首先需要安装其Python库并进行初始化。

```python
# 安装 wandb 库
!pip install wandb -q

![](img/6809f64637cdb7d6cac1703eb5ceaa16_13.png)

![](img/6809f64637cdb7d6cac1703eb5ceaa16_15.png)

# 导入必要的库
import wandb
from wandb.keras import WandbCallback

![](img/6809f64637cdb7d6cac1703eb5ceaa16_17.png)

# 初始化一个W&B运行实例
# 这将创建一个项目来跟踪你的实验
wandb.init(project="flower-classification-p4", entity="your-username")
```

![](img/6809f64637cdb7d6cac1703eb5ceaa16_19.png)

![](img/6809f64637cdb7d6cac1703eb5ceaa16_20.png)

初始化后，你可以通过一个配置字典来定义本次实验的超参数。

![](img/6809f64637cdb7d6cac1703eb5ceaa16_22.png)

```python
# 定义超参数配置
config = wandb.config
config.learning_rate = 0.001
config.epochs = 10
config.batch_size = 32
config.hidden_layer_size = 128
config.image_size = 224
```

在模型训练过程中，只需添加`WandbCallback`回调函数，W&B就会自动记录每一轮的指标。

![](img/6809f64637cdb7d6cac1703eb5ceaa16_24.png)

![](img/6809f64637cdb7d6cac1703eb5ceaa16_25.png)

![](img/6809f64637cdb7d6cac1703eb5ceaa16_26.png)

![](img/6809f64637cdb7d6cac1703eb5ceaa16_27.png)

```python
# 在 model.fit 中添加 WandbCallback
history = model.fit(
    train_dataset,
    validation_data=val_dataset,
    epochs=config.epochs,
    callbacks=[WandbCallback()]  # 添加此行以自动记录
)
```

![](img/6809f64637cdb7d6cac1703eb5ceaa16_29.png)

![](img/6809f64637cdb7d6cac1703eb5ceaa16_30.png)

![](img/6809f64637cdb7d6cac1703eb5ceaa16_31.png)

训练结束后，你可以在Weights & Biases的网页仪表板中查看所有记录的指标图表，并轻松比较不同配置实验的结果。

## 系统化实验流程

![](img/6809f64637cdb7d6cac1703eb5ceaa16_33.png)

![](img/6809f64637cdb7d6cac1703eb5ceaa16_34.png)

以下是使用W&B进行超参数调优的系统化步骤：

![](img/6809f64637cdb7d6cac1703eb5ceaa16_36.png)

1.  **定义搜索空间**：确定你想要调整的超参数及其可能的值范围。
2.  **编写可配置的训练脚本**：确保你的训练代码能够从`wandb.config`中读取超参数。
3.  **使用W&B Sweep**：利用W&B的Sweep功能自动或半自动地运行多组超参数实验。
4.  **分析与比较**：在W&B仪表板中分析结果，找出表现最佳的超参数组合。

![](img/6809f64637cdb7d6cac1703eb5ceaa16_37.png)

![](img/6809f64637cdb7d6cac1703eb5ceaa16_39.png)

通过这种方式，你可以轻松运行数十甚至数百个实验，而无需手动管理每个实验的代码和结果。

![](img/6809f64637cdb7d6cac1703eb5ceaa16_41.png)

## 总结

![](img/6809f64637cdb7d6cac1703eb5ceaa16_43.png)

本节课中我们一起学习了如何利用Weights & Biases平台来专业地进行机器学习实验。我们了解了手动管理实验的弊端，并掌握了使用W&B来自动记录超参数、跟踪训练指标以及可视化比较不同实验结果的方法。通过集成简单的代码，我们能够将繁琐的实验管理过程系统化，从而更专注于模型和算法的改进。在接下来的课程中，我们将运用这个强大的工具来更高效地探索正则化、Dropout等改进我们花卉分类模型的方法。