# 070：PyTorch中的自定义数据加载器

在本节课中，我们将学习如何在PyTorch中创建自定义的数据加载器。我们将通过一个具体的例子，使用MNIST数据集（以PNG图片文件形式存储）来演示如何从零开始构建一个`Dataset`类，并将其与`DataLoader`结合使用，以便在模型训练中高效地加载和批处理数据。

## 概述与数据准备

![](img/c68c8e32db85801c06d8ca2cb577037a_1.png)

首先，我们需要理解为什么需要自定义数据加载器。PyTorch内置的`DataLoader`和`Dataset`类为常见数据集提供了便利，但在实际研究中，我们的数据通常以自定义格式存储（例如，特定文件夹结构中的图片文件及其对应的CSV标签文件）。自定义数据加载器使我们能够灵活地处理这些数据。

![](img/c68c8e32db85801c06d8ca2cb577037a_3.png)

本节使用的数据是MNIST数据集的一个小型子集，它被分成了训练集、验证集和测试集三个部分，并分别存储在对应的文件夹中。每个文件夹内包含PNG格式的图片文件，同时有一个CSV文件记录了文件名及其对应的数字标签。

![](img/c68c8e32db85801c06d8ca2cb577037a_5.png)

以下是数据组织结构的示例：
*   `mnist_train/` 文件夹包含训练图片，`mnist_train.csv` 文件包含对应的标签。
*   `mnist_valid/` 和 `mnist_valid.csv` 对应验证集。
*   `mnist_test/` 和 `mnist_test.csv` 对应测试集。

这种结构代表了一种常见的数据存储方式，你可以根据自己的需求调整文件夹和文件的组织方式。

![](img/c68c8e32db85801c06d8ca2cb577037a_7.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_8.png)

## 数据探查

![](img/c68c8e32db85801c06d8ca2cb577037a_10.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_11.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_13.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_15.png)

在构建数据加载器之前，先探查数据是一个好习惯。这有助于我们了解数据的格式、维度和值范围。

我们可以使用Python的PIL库（Pillow）来加载和查看一张图片。

```python
from PIL import Image
import matplotlib.pyplot as plt

# 加载一张图片
img_path = './mnist_train/5132.png'
image = Image.open(img_path)

# 查看图片尺寸和模式
print(f"Image size: {image.size}") # 输出: (28, 28)
print(f"Image mode: {image.mode}") # 输出: L (灰度图)

# 将图片转换为数组并查看像素值范围
import numpy as np
img_array = np.array(image)
print(f"Pixel value range: [{img_array.min()}, {img_array.max()}]") # 输出: [0, 255] 或类似范围

# 显示图片（使用灰度色彩映射）
plt.imshow(img_array, cmap='gray')
plt.show()
```

通过探查，我们确认数据是28x28像素的灰度图，像素值在0到255之间。这些信息在后续的数据预处理中很重要。

![](img/c68c8e32db85801c06d8ca2cb577037a_17.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_19.png)

接下来，我们查看一下CSV文件的内容，以确认标签与文件的对应关系。

```python
import pandas as pd

# 加载训练集的CSV文件
train_df = pd.read_csv('./mnist_train.csv')
print(train_df.head())
```

![](img/c68c8e32db85801c06d8ca2cb577037a_21.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_23.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_25.png)

CSV文件通常包含`file_name`和`label`两列，分别对应图片文件名和数字标签。

![](img/c68c8e32db85801c06d8ca2cb577037a_27.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_28.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_30.png)

## 构建自定义Dataset类

![](img/c68c8e32db85801c06d8ca2cb577037a_32.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_33.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_34.png)

上一节我们探查了数据的基本情况，本节中我们来看看如何构建PyTorch的`Dataset`类。`Dataset`是一个抽象类，用于表示数据集。我们需要继承它并实现三个核心方法：`__init__`、`__getitem__`和`__len__`。

以下是自定义`Dataset`类的代码：

![](img/c68c8e32db85801c06d8ca2cb577037a_36.png)

```python
import torch
from torch.utils.data import Dataset
from PIL import Image
import pandas as pd

![](img/c68c8e32db85801c06d8ca2cb577037a_38.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_39.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_40.png)

class MyDataset(Dataset):
    """自定义数据集类，用于加载图片和标签。"""

    def __init__(self, csv_path, img_dir, transform=None):
        """
        初始化数据集。
        参数:
            csv_path (str): CSV标签文件的路径。
            img_dir (str): 图片文件夹的路径。
            transform (callable, optional): 一个可选的图像变换函数。
        """
        # 读取CSV文件
        self.df = pd.read_csv(csv_path)
        # 存储图片目录路径
        self.img_dir = img_dir
        # 获取文件名列表
        self.image_names = self.df['file_name']
        # 获取标签列表
        self.y = self.df['label']
        # 存储图像变换函数
        self.transform = transform

    def __len__(self):
        """返回数据集中的样本总数。"""
        return self.y.shape[0]

    def __getitem__(self, idx):
        """根据索引idx加载并返回一个数据样本（图像和标签）。"""
        # 构建完整的图片路径
        img_path = f"{self.img_dir}/{self.image_names.iloc[idx]}"
        # 打开图片
        image = Image.open(img_path)

        # 如果提供了transform函数，则应用它（例如，转换为Tensor、归一化）
        if self.transform:
            image = self.transform(image)

        # 获取对应的标签
        label = self.y.iloc[idx]
        # 将标签转换为PyTorch Tensor（长整型）
        label = torch.tensor(label, dtype=torch.long)

        return image, label
```

**代码解析：**
*   `__init__`：构造函数。我们在这里加载CSV文件，并保存图片目录、文件名列表、标签列表以及可能的数据变换函数。
*   `__len__`：返回数据集的长度，这对于`DataLoader`确定何时完成一个epoch至关重要。
*   `__getitem__`：这是核心方法。它根据给定的索引`idx`，从硬盘加载对应的图片，应用变换，获取标签，并返回`(图像, 标签)`对。`DataLoader`会自动调用此方法来获取批量数据。

## 创建DataLoader并进行批处理

我们已经定义了如何加载单个数据样本，接下来需要`DataLoader`来帮我们组织小批量（mini-batch）数据，并提供打乱、并行加载等功能。

首先，我们需要定义一个简单的图像变换。这里我们使用`torchvision.transforms.ToTensor()`，它可以将PIL图像转换为PyTorch Tensor，并自动将像素值从[0, 255]范围归一化到[0.0, 1.0]范围。

```python
from torchvision import transforms

![](img/c68c8e32db85801c06d8ca2cb577037a_42.png)

# 定义图像变换：仅转换为Tensor（包含归一化）
transform = transforms.Compose([
    transforms.ToTensor()
])
```

现在，我们可以为训练集、验证集和测试集分别创建`Dataset`实例，然后用它们来初始化`DataLoader`。

![](img/c68c8e32db85801c06d8ca2cb577037a_44.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_45.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_47.png)

```python
from torch.utils.data import DataLoader

![](img/c68c8e32db85801c06d8ca2cb577037a_49.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_51.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_53.png)

# 创建Dataset实例
train_dataset = MyDataset(csv_path='./mnist_train.csv',
                          img_dir='./mnist_train',
                          transform=transform)

![](img/c68c8e32db85801c06d8ca2cb577037a_55.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_57.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_58.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_60.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_62.png)

valid_dataset = MyDataset(csv_path='./mnist_valid.csv',
                          img_dir='./mnist_valid',
                          transform=transform)

![](img/c68c8e32db85801c06d8ca2cb577037a_64.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_66.png)

test_dataset = MyDataset(csv_path='./mnist_test.csv',
                         img_dir='./mnist_test',
                         transform=transform)

![](img/c68c8e32db85801c06d8ca2cb577037a_68.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_69.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_71.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_73.png)

# 创建DataLoader实例
batch_size = 32
train_loader = DataLoader(dataset=train_dataset,
                          batch_size=batch_size,
                          shuffle=True,   # 训练时打乱数据
                          num_workers=0,  # 用于数据加载的子进程数，0表示仅用主进程
                          drop_last=True) # 丢弃最后一个不完整的batch

![](img/c68c8e32db85801c06d8ca2cb577037a_75.png)

valid_loader = DataLoader(dataset=valid_dataset,
                          batch_size=batch_size,
                          shuffle=False,
                          num_workers=0)

![](img/c68c8e32db85801c06d8ca2cb577037a_77.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_79.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_81.png)

test_loader = DataLoader(dataset=test_dataset,
                         batch_size=batch_size,
                         shuffle=False,
                         num_workers=0)
```

**参数解释：**
*   `batch_size`：每个小批量包含的样本数。
*   `shuffle`：是否在每个epoch开始时打乱数据。通常训练集需要打乱，验证集和测试集则不需要。
*   `num_workers`：用于数据加载的子进程数量。大于0时可以实现并行数据加载，从而让GPU无需等待下一批数据准备完毕，提升训练效率。对于MNIST这类极小的数据集，有时设为0以避免文件句柄过多的问题。
*   `drop_last`：当数据集样本数不能被`batch_size`整除时，是否丢弃最后一个不完整的小批量。有时丢弃它可以避免一个非常小的批次对模型更新产生噪声影响。

![](img/c68c8e32db85801c06d8ca2cb577037a_83.png)

现在，我们可以迭代`DataLoader`来查看批量数据。

![](img/c68c8e32db85801c06d8ca2cb577037a_85.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_86.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_87.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_89.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_91.png)

```python
# 模拟训练循环，查看一个epoch的数据
for epoch in range(2): # 2个epoch
    print(f"\nEpoch {epoch+1}")
    for batch_idx, (features, labels) in enumerate(train_loader):
        # 在这里，features会被自动转移到GPU（如果设置了device）
        # 进行前向传播、计算损失、反向传播等步骤...
        print(f"  Batch {batch_idx}: Features shape = {features.shape}, Labels shape = {labels.shape}")
        # 例如，输出: Features shape = torch.Size([32, 1, 28, 28])
        # 只查看前几个批次
        if batch_idx == 2:
            break
```

![](img/c68c8e32db85801c06d8ca2cb577037a_93.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_95.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_97.png)

输出中，`features`的形状是`[batch_size, channels, height, width]`，对于MNIST就是`[32, 1, 28, 28]`。这正是卷积神经网络期望的输入格式。如果使用全连接网络，我们可以在模型的第一层使用`torch.nn.Flatten()`将其展平为`[32, 784]`。

![](img/c68c8e32db85801c06d8ca2cb577037a_99.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_100.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_102.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_104.png)

## 在完整模型训练中使用自定义DataLoader

![](img/c68c8e32db85801c06d8ca2cb577037a_106.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_107.png)

前面我们构建了数据加载的组件，本节我们将把这些组件集成到一个完整的模型训练流程中。为了保持代码的整洁和可复用性，通常会将数据集定义、数据加载器创建、模型训练和评估等函数模块化，放在单独的`.py`文件中。

假设我们有以下辅助文件结构：
*   `dataset.py`：包含`MyDataset`类的定义。
*   `dataloader_utils.py`：包含`get_dataloaders`函数，用于创建并返回训练、验证、测试的`DataLoader`。
*   `train.py`：包含模型训练循环的函数。
*   `eval.py`：包含计算准确率等评估函数。
*   `model.py`：包含神经网络模型的定义。

![](img/c68c8e32db85801c06d8ca2cb577037a_109.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_111.png)

在主训练脚本或Jupyter Notebook中，代码会变得非常简洁：

![](img/c68c8e32db85801c06d8ca2cb577037a_113.png)

```python
# 导入模块和辅助函数
import torch
from dataloader_utils import get_dataloaders
from model import MultilayerPerceptron
from train import train_model
from eval import compute_accuracy

![](img/c68c8e32db85801c06d8ca2cb577037a_115.png)

# 设置超参数和随机种子以确保可复现性
RANDOM_SEED = 123
BATCH_SIZE = 32
NUM_EPOCHS = 10
torch.manual_seed(RANDOM_SEED)

![](img/c68c8e32db85801c06d8ca2cb577037a_117.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_119.png)

# 获取数据加载器
train_loader, valid_loader, test_loader = get_dataloaders(batch_size=BATCH_SIZE)

![](img/c68c8e32db85801c06d8ca2cb577037a_121.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_122.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_123.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_125.png)

# 初始化模型、损失函数和优化器
model = MultilayerPerceptron(input_size=784, hidden_size=50, output_size=10)
loss_fn = torch.nn.CrossEntropyLoss()
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)

![](img/c68c8e32db85801c06d8ca2cb577037a_127.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_129.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_130.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_132.png)

# 训练模型
train_losses, valid_losses, train_accs, valid_accs = train_model(
    model=model,
    train_loader=train_loader,
    valid_loader=valid_loader,
    loss_fn=loss_fn,
    optimizer=optimizer,
    num_epochs=NUM_EPOCHS
)

![](img/c68c8e32db85801c06d8ca2cb577037a_134.png)

# 在测试集上评估最终模型性能
test_acc = compute_accuracy(model, test_loader)
print(f"\nFinal Test Accuracy: {test_acc:.2f}%")
```

![](img/c68c8e32db85801c06d8ca2cb577037a_136.png)

其中，一个简化的多层感知机模型`MultilayerPerceptron`可以这样定义：

![](img/c68c8e32db85801c06d8ca2cb577037a_138.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_140.png)

```python
# model.py
import torch.nn as nn

![](img/c68c8e32db85801c06d8ca2cb577037a_142.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_144.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_146.png)

class MultilayerPerceptron(nn.Module):
    def __init__(self, input_size, hidden_size, output_size):
        super().__init__()
        self.layers = nn.Sequential(
            nn.Flatten(), # 将 [batch, 1, 28, 28] 展平为 [batch, 784]
            nn.Linear(input_size, hidden_size),
            nn.Sigmoid(), # 激活函数
            nn.Linear(hidden_size, output_size)
            # 注意：这里没有Softmax，因为CrossEntropyLoss内部包含了它
        )

    def forward(self, x):
        logits = self.layers(x)
        return logits
```

![](img/c68c8e32db85801c06d8ca2cb577037a_148.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_150.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_151.png)

通过这种方式，我们成功地将自定义数据加载流程整合到了端到端的深度学习训练管道中。模块化的设计使得代码易于维护、调试和在不同项目间复用。

![](img/c68c8e32db85801c06d8ca2cb577037a_153.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_155.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_157.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_158.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_159.png)

## 总结

![](img/c68c8e32db85801c06d8ca2cb577037a_161.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_163.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_164.png)

![](img/c68c8e32db85801c06d8ca2cb577037a_166.png)

本节课中我们一起学习了如何在PyTorch中创建和使用自定义数据加载器。我们从探查自定义格式的数据（PNG图片和CSV标签）开始，然后逐步构建了继承自`torch.utils.data.Dataset`的自定义类`MyDataset`，实现了数据加载的核心逻辑。接着，我们使用`torch.utils.data.DataLoader`来创建能够高效批处理、打乱和并行加载数据的迭代器。最后，我们将这些组件模块化，并集成到一个完整的模型训练示例中，演示了其在真实训练流程中的应用。

![](img/c68c8e32db85801c06d8ca2cb577037a_168.png)

掌握自定义数据加载是处理真实世界研究数据的关键一步，它提供了处理任意数据格式的灵活性。在接下来的课程中，我们将探讨如何通过数据增强（变换）来正则化模型，以防止过拟合。