#  037：Python深度学习库入门 - TensorFlow, Keras, PyTorch

![](img/231309c9ca8e0688d4963d7084b89159_0.png)

![](img/231309c9ca8e0688d4963d7084b89159_0.png)

在本节课中，我们将讨论Python中三个最强大的深度学习框架：TensorFlow、Keras和PyTorch。我们将了解它们的重要性，并通过构建一个简单的神经网络来学习它们的基本用法。课程结束时，你将对这些框架有一个清晰的认识。

## 什么是神经网络？🧠

首先，让我们讨论什么是神经网络。

从本质上讲，神经网络被称为**通用函数逼近器**。这是因为，一个具有单隐藏层和有限数量节点的神经网络，可以在给定的输入范围内逼近任何连续函数。

一个更简单的理解方式是，将神经网络看作是一堆层的组合。你可能见过类似下图的表示，它通过输入层、一个或多个隐藏层以及最终的输出层来描述一个神经网络。

![](img/231309c9ca8e0688d4963d7084b89159_2.png)

在这个神经网络中，输入层有三个节点，意味着你可以输入三个值。它有两个隐藏层，以及一个输出节点，最终输出一个数值。

这些神经网络有时也被称为**大型复合函数**。例如，上图所示的神经网络可以用如下函数表示：

**output = σ( W₂ * σ( W₁ * x + b₁ ) + b₂ )**

其中：
*   **W** 代表权重。
*   **b** 代表偏置。
*   **σ** 代表激活函数。

在这个神经网络中，如果没有激活函数，输出将只是输入的线性组合，即 **y = W₂ * (W₁ * x + b₁) + b₂**，这仍然是一个线性函数。激活函数的引入使得网络能够学习和表示复杂的非线性关系。

## 深度学习框架简介 🛠️

上一节我们介绍了神经网络的基本概念，本节中我们来看看为什么需要专门的框架来实现它们。

手动实现神经网络的所有计算（如前向传播、反向传播和梯度更新）非常繁琐且容易出错。这就是深度学习框架的用武之地。它们提供了高效、模块化的工具来构建和训练神经网络。

以下是三个最流行的Python深度学习框架：

*   **TensorFlow**：由Google开发，是一个用于高性能数值计算的开源库，特别专注于机器学习和深度学习。它提供了灵活性和强大的生产部署能力。
*   **Keras**：最初是一个独立的高级神经网络API，现在已紧密集成到TensorFlow中，作为 `tf.keras`。它以用户友好、模块化和可扩展性著称，能够快速原型设计。
*   **PyTorch**：由Facebook的AI研究实验室开发，以其动态计算图和“Python化”的设计哲学而闻名，深受学术研究界的喜爱。

## 实践：使用Keras构建神经网络 💻

了解了基本框架后，让我们动手实践。本节我们将使用Keras构建一个用于二元分类任务的简单神经网络。

我们将使用 `scikit-learn` 来导入数据。以下是构建模型的步骤：

1.  **导入必要的库**：包括 `tensorflow` 和 `sklearn` 的相关模块。
2.  **准备数据**：使用 `sklearn` 生成或加载一个简单的数据集，并将其分为训练集和测试集。
3.  **构建模型**：使用 `tf.keras.Sequential` 顺序模型，添加输入层、隐藏层和输出层。
4.  **编译模型**：指定损失函数、优化器和评估指标。
5.  **训练模型**：在训练数据上拟合模型。
6.  **评估模型**：在测试数据上评估模型的性能。

```python
# 示例代码框架
import tensorflow as tf
from tensorflow import keras
from sklearn.model_selection import train_test_split
from sklearn.datasets import make_classification

# 1. 生成模拟数据
X, y = make_classification(n_samples=1000, n_features=20, random_state=42)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 2. 构建模型
model = keras.Sequential([
    keras.layers.Dense(64, activation='relu', input_shape=(20,)), # 隐藏层
    keras.layers.Dense(1, activation='sigmoid') # 输出层
])

# 3. 编译模型
model.compile(optimizer='adam',
              loss='binary_crossentropy',
              metrics=['accuracy'])

# 4. 训练模型
model.fit(X_train, y_train, epochs=10, batch_size=32, validation_split=0.1)

# 5. 评估模型
test_loss, test_acc = model.evaluate(X_test, y_test)
print(f'测试准确率：{test_acc}')
```

## 实践：使用PyTorch构建神经网络 ⚡

现在，我们来看看如何使用PyTorch完成相同的任务。PyTorch提供了更底层的控制，其动态图特性使得调试更加直观。

以下是使用PyTorch构建神经网络的关键步骤：

1.  **定义网络结构**：创建一个继承自 `torch.nn.Module` 的类，在 `__init__` 中定义层，在 `forward` 方法中定义前向传播逻辑。
2.  **准备数据**：将数据转换为PyTorch张量，并可能封装进 `DataLoader`。
3.  **定义损失函数和优化器**：例如，使用二元交叉熵损失和Adam优化器。
4.  **训练循环**：手动编写循环，在每个周期（epoch）中执行前向传播、计算损失、反向传播和参数更新。
5.  **模型评估**。

```python
# 示例代码框架
import torch
import torch.nn as nn
import torch.optim as optim
from sklearn.model_selection import train_test_split
from sklearn.datasets import make_classification

# 1. 生成数据并转换为张量
X, y = make_classification(n_samples=1000, n_features=20, random_state=42)
X = torch.tensor(X, dtype=torch.float32)
y = torch.tensor(y, dtype=torch.float32).view(-1, 1)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 2. 定义模型
class SimpleNN(nn.Module):
    def __init__(self):
        super(SimpleNN, self).__init__()
        self.layer1 = nn.Linear(20, 64)
        self.layer2 = nn.Linear(64, 1)
        self.relu = nn.ReLU()
        self.sigmoid = nn.Sigmoid()

    def forward(self, x):
        x = self.relu(self.layer1(x))
        x = self.sigmoid(self.layer2(x))
        return x

model = SimpleNN()

# 3. 定义损失函数和优化器
criterion = nn.BCELoss()
optimizer = optim.Adam(model.parameters(), lr=0.001)

# 4. 训练循环
epochs = 10
for epoch in range(epochs):
    model.train()
    optimizer.zero_grad() # 清零梯度
    outputs = model(X_train)
    loss = criterion(outputs, y_train)
    loss.backward() # 反向传播
    optimizer.step() # 更新参数
    print(f'Epoch [{epoch+1}/{epochs}], Loss: {loss.item():.4f}')

# 5. 评估（示例）
model.eval()
with torch.no_grad():
    predictions = model(X_test)
    predicted_classes = (predictions > 0.5).float()
    accuracy = (predicted_classes == y_test).float().mean()
    print(f'测试准确率：{accuracy.item():.4f}')
```

## 如何选择框架？🤔

我们已经用Keras和PyTorch分别实现了简单的神经网络。那么在实际项目中该如何选择呢？

以下是一些简单的指导原则：

*   **选择Keras (tf.keras) 如果**：你是一名初学者，希望快速入门和构建原型。你的项目侧重于快速实验和部署，并且你习惯使用TensorFlow生态系统。
*   **选择PyTorch 如果**：你正在进行学术研究，需要灵活的模型结构和动态计算图以便于调试。你更喜欢Python化的、直观的编程风格。
*   **选择TensorFlow (底层API) 如果**：你需要对模型进行极其精细的控制，或者项目对生产环境下的性能和部署有非常高的要求。

实际上，两者都非常强大，并且功能在逐渐趋同。对于大多数初学者和常见任务，从 `tf.keras` 开始是一个极佳的选择。

## 总结 📚

本节课中我们一起学习了Python深度学习的核心工具。

我们首先了解了神经网络作为通用函数逼近器的基本概念。接着，我们介绍了三大主流框架：TensorFlow、Keras和PyTorch。通过动手实践，我们分别使用 **Keras** 和 **PyTorch** 构建并训练了一个简单的二元分类神经网络。最后，我们讨论了如何根据项目需求和个人偏好来选择合适的框架。

![](img/231309c9ca8e0688d4963d7084b89159_2.png)

掌握这些框架是进入深度学习领域的重要一步。建议你继续在更复杂的数据集和网络结构上进行练习，以加深理解。