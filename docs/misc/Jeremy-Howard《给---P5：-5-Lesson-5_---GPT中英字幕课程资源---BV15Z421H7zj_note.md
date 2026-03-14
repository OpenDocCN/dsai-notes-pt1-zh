# 课程5：从零构建线性模型与神经网络 🚢

![](img/82ab127558454f398f75c3d75e236e71_1.png)

![](img/82ab127558454f398f75c3d75e236e71_3.png)

![](img/82ab127558454f398f75c3d75e236e71_5.png)

![](img/82ab127558454f398f75c3d75e236e71_7.png)

![](img/82ab127558454f398f75c3d75e236e71_9.png)

![](img/82ab127558454f398f75c3d75e236e71_11.png)

在本节课中，我们将深入学习神经网络的工作原理。我们将回到表格数据，并尝试从零开始构建一个表格模型。具体来说，我们将从零构建几种不同类型的表格模型。

![](img/82ab127558454f398f75c3d75e236e71_13.png)

![](img/82ab127558454f398f75c3d75e236e71_15.png)

![](img/82ab127558454f398f75c3d75e236e71_17.png)

![](img/82ab127558454f398f75c3d75e236e71_19.png)

![](img/82ab127558454f398f75c3d75e236e71_21.png)

![](img/82ab127558454f398f75c3d75e236e71_23.png)

## 概述与数据准备

![](img/82ab127558454f398f75c3d75e236e71_25.png)

![](img/82ab127558454f398f75c3d75e236e71_27.png)

![](img/82ab127558454f398f75c3d75e236e71_29.png)

![](img/82ab127558454f398f75c3d75e236e71_31.png)

上一节我们介绍了使用Hugging Face Transformers训练NLP模型。本节中，我们将使用泰坦尼克号数据集，从零开始构建线性模型和神经网络。

![](img/82ab127558454f398f75c3d75e236e71_33.png)

![](img/82ab127558454f398f75c3d75e236e71_35.png)

首先，我们需要准备数据。泰坦尼克号数据集每一行代表一位乘客，包含其是否幸存、船舱等级、性别、年龄、兄弟姐妹数量、其他家庭成员数量、票价以及登船港口等信息。

![](img/82ab127558454f398f75c3d75e236e71_37.png)

![](img/82ab127558454f398f75c3d75e236e71_39.png)

![](img/82ab127558454f398f75c3d75e236e71_41.png)

### 环境设置与数据加载

![](img/82ab127558454f398f75c3d75e236e71_43.png)

![](img/82ab127558454f398f75c3d75e236e71_44.png)

![](img/82ab127558454f398f75c3d75e236e71_46.png)

以下是设置环境和加载数据的关键步骤：

![](img/82ab127558454f398f75c3d75e236e71_48.png)

![](img/82ab127558454f398f75c3d75e236e71_50.png)

*   **检查环境**：首先检查是否在Kaggle环境中，以便正确设置数据路径。
*   **安装必要库**：使用 `pip install kaggle` 安装Kaggle API。
*   **导入核心库**：导入PyTorch、NumPy和Pandas，它们是处理数据和构建模型的基础。
*   **读取数据**：使用Pandas的 `read_csv` 函数读取CSV文件，并查看数据的基本信息。

```python
import torch, numpy as np, pandas as pd
df = pd.read_csv(path/'train.csv')
df.head()
```

### 处理缺失值

![](img/82ab127558454f398f75c3d75e236e71_52.png)

在Excel版本中，我们直接删除了包含缺失值的行。但在Pandas中，处理缺失值更加灵活。一个简单有效的方法是用该列的众数（出现频率最高的值）填充缺失值。

![](img/82ab127558454f398f75c3d75e236e71_54.png)

![](img/82ab127558454f398f75c3d75e236e71_56.png)

```python
# 计算每列的缺失值数量
df.isna().sum()
# 获取每列的众数
modes = df.mode().iloc[0]
# 用众数填充缺失值
df.fillna(modes, inplace=True)
# 再次检查，确认无缺失值
df.isna().sum()
```

![](img/82ab127558454f398f75c3d75e236e71_58.png)

![](img/82ab127558454f398f75c3d75e236e71_60.png)

![](img/82ab127558454f398f75c3d75e236e71_62.png)

![](img/82ab127558454f398f75c3d75e236e71_64.png)

### 数据探索与预处理

![](img/82ab127558454f398f75c3d75e236e71_66.png)

![](img/82ab127558454f398f75c3d75e236e71_68.png)

![](img/82ab127558454f398f75c3d75e236e71_69.png)

了解数据分布对建模至关重要。

![](img/82ab127558454f398f75c3d75e236e71_71.png)

![](img/82ab127558454f398f75c3d75e236e71_73.png)

![](img/82ab127558454f398f75c3d75e236e71_75.png)

*   **数值变量描述**：使用 `df.describe()` 快速查看数值变量的统计信息（如均值、标准差、分位数）。
*   **处理长尾分布**：像“票价”这样的列通常呈长尾分布（少数极大值）。对这类数据取对数可以使其分布更集中，有利于模型训练。公式为：`log1p = log(x + 1)`。
*   **分类变量处理**：模型无法直接处理“男性”、“S港口”这样的文本。我们需要创建虚拟变量（哑变量），为每个类别创建一个新的布尔列。

![](img/82ab127558454f398f75c3d75e236e71_77.png)

![](img/82ab127558454f398f75c3d75e236e71_79.png)

![](img/82ab127558454f398f75c3d75e236e71_81.png)

```python
# 对票价取对数
df['LogFare'] = np.log1p(df['Fare'])
# 为分类变量创建虚拟变量
df = pd.get_dummies(df, columns=['Sex', 'Pclass', 'Embarked'])
```

![](img/82ab127558454f398f75c3d75e236e71_83.png)

![](img/82ab127558454f398f75c3d75e236e71_85.png)

![](img/82ab127558454f398f75c3d75e236e71_86.png)

![](img/82ab127558454f398f75c3d75e236e71_88.png)

![](img/82ab127558454f398f75c3d75e236e71_90.png)

![](img/82ab127558454f398f75c3d75e236e71_92.png)

![](img/82ab127558454f398f75c3d75e236e71_94.png)

并非所有列都适合作为特征。例如，“姓名”列包含大量唯一值，直接创建虚拟变量会导致维度爆炸。但通过特征工程（如提取头衔、姓氏），可以从中挖掘有用信息。

![](img/82ab127558454f398f75c3d75e236e71_96.png)

![](img/82ab127558454f398f75c3d75e236e71_98.png)

## 从零构建线性模型

![](img/82ab127558454f398f75c3d75e236e71_100.png)

![](img/82ab127558454f398f75c3d75e236e71_102.png)

![](img/82ab127558454f398f75c3d75e236e71_104.png)

现在，我们将数据转换为PyTorch张量，并构建一个线性模型。

![](img/82ab127558454f398f75c3d75e236e71_106.png)

![](img/82ab127558454f398f75c3d75e236e71_108.png)

### 准备张量

![](img/82ab127558454f398f75c3d75e236e71_110.png)

首先，将因变量（是否幸存）和自变量（特征）转换为PyTorch张量。

![](img/82ab127558454f398f75c3d75e236e71_112.png)

![](img/82ab127558454f398f75c3d75e236e71_114.png)

```python
# 因变量
dep_var = 'Survived'
y = torch.tensor(df[dep_var].values, dtype=torch.float32).reshape(-1, 1)

# 自变量（特征）
indep_cols = ['Age', 'SibSp', 'Parch', 'LogFare'] + [c for c in df.columns if c.startswith(('Sex_', 'Pclass_', 'Embarked_'))]
x = torch.tensor(df[indep_cols].values, dtype=torch.float32)
```

![](img/82ab127558454f398f75c3d75e236e71_116.png)

![](img/82ab127558454f398f75c3d75e236e71_118.png)

### 实现线性模型

![](img/82ab127558454f398f75c3d75e236e71_120.png)

线性模型的核心是每个特征乘以一个系数，然后求和。我们可以利用PyTorch的**广播机制**高效地实现矩阵与向量的元素级乘法。

```python
# 初始化随机系数
torch.manual_seed(42) # 设置随机种子保证结果可复现
coeffs = torch.rand(x.shape[1]) - 0.5
coeffs.shape
# 利用广播机制计算预测值：每个特征值乘以其系数
preds = (x * coeffs).sum(axis=1)
```

![](img/82ab127558454f398f75c3d75e236e71_122.png)

为了使不同尺度的特征对系数的影响相近，需要对特征进行**归一化**。一个简单的方法是除以每个特征的最大值。

```python
# 归一化特征
x_max = x.max(dim=0).values
x_norm = x / x_max
# 使用归一化后的特征计算预测
preds = (x_norm * coeffs).sum(axis=1)
```

![](img/82ab127558454f398f75c3d75e236e71_124.png)

![](img/82ab127558454f398f75c3d75e236e71_125.png)

### 训练模型：梯度下降

![](img/82ab127558454f398f75c3d75e236e71_127.png)

![](img/82ab127558454f398f75c3d75e236e71_129.png)

![](img/82ab127558454f398f75c3d75e236e71_131.png)

我们需要一个损失函数来衡量模型预测的好坏，然后使用梯度下降法优化系数。

![](img/82ab127558454f398f75c3d75e236e71_133.png)

![](img/82ab127558454f398f75c3d75e236e71_135.png)

![](img/82ab127558454f398f75c3d75e236e71_137.png)

1.  **定义损失函数**：这里使用平均绝对误差（MAE）。
2.  **启用梯度计算**：将系数张量的 `requires_grad` 属性设为 `True`。
3.  **计算梯度**：调用损失函数的 `.backward()` 方法，梯度会存储在系数的 `.grad` 属性中。
4.  **更新系数**：沿着梯度反方向，以学习率（lr）为步长更新系数：`coeffs -= lr * coeffs.grad`。

![](img/82ab127558454f398f75c3d75e236e71_139.png)

![](img/82ab127558454f398f75c3d75e236e71_141.png)

![](img/82ab127558454f398f75c3d75e236e71_143.png)

![](img/82ab127558454f398f75c3d75e236e71_145.png)

![](img/82ab127558454f398f75c3d75e236e71_147.png)

我们将这些步骤封装成函数，并循环多个周期（epoch）进行训练。

![](img/82ab127558454f398f75c3d75e236e71_148.png)

![](img/82ab127558454f398f75c3d75e236e71_150.png)

![](img/82ab127558454f398f75c3d75e236e71_152.png)

```python
def calc_preds(coeffs, x): return (x @ coeffs).view(-1, 1)
def calc_loss(coeffs, x, y): return torch.abs(calc_preds(coeffs, x) - y).mean()

![](img/82ab127558454f398f75c3d75e236e71_154.png)

![](img/82ab127558454f398f75c3d75e236e71_156.png)

![](img/82ab127558454f398f75c3d75e236e71_158.png)

![](img/82ab127558454f398f75c3d75e236e71_160.png)

![](img/82ab127558454f398f75c3d75e236e71_162.png)

def train_model(epochs=30, lr=0.01):
    torch.manual_seed(42)
    coeffs = (torch.rand(x.shape[1], 1) - 0.5).requires_grad_()
    for i in range(epochs): 
        loss = calc_loss(coeffs, x_norm, y)
        loss.backward()
        coeffs.data -= lr * coeffs.grad.data
        coeffs.grad.zero_()
        print(f"Epoch {i}: loss={loss.item():.4f}")
    return coeffs

![](img/82ab127558454f398f75c3d75e236e71_164.png)

![](img/82ab127558454f398f75c3d75e236e71_166.png)

![](img/82ab127558454f398f75c3d75e236e71_168.png)

![](img/82ab127558454f398f75c3d75e236e71_169.png)

![](img/82ab127558454f398f75c3d75e236e71_171.png)

coeffs = train_model()
```

![](img/82ab127558454f398f75c3d75e236e71_173.png)

![](img/82ab127558454f398f75c3d75e236e71_175.png)

![](img/82ab127558454f398f75c3d75e236e71_176.png)

![](img/82ab127558454f398f75c3d75e236e71_178.png)

![](img/82ab127558454f398f75c3d75e236e71_180.png)

### 模型评估与改进

![](img/82ab127558454f398f75c3d75e236e71_182.png)

![](img/82ab127558454f398f75c3d75e236e71_184.png)

训练完成后，我们可以查看每个特征的系数，并评估模型在验证集上的准确率。

![](img/82ab127558454f398f75c3d75e236e71_186.png)

![](img/82ab127558454f398f75c3d75e236e71_188.png)

![](img/82ab127558454f398f75c3d75e236e71_190.png)

![](img/82ab127558454f398f75c3d75e236e71_192.png)

![](img/82ab127558454f398f75c3d75e236e71_194.png)

*   **查看系数**：系数的大小和正负反映了特征与生存率的关系。
*   **计算准确率**：将预测概率大于0.5的视为预测生存，与真实标签比较。

![](img/82ab127558454f398f75c3d75e236e71_196.png)

![](img/82ab127558454f398f75c3d75e236e71_198.png)

![](img/82ab127558454f398f75c3d75e236e71_200.png)

![](img/82ab127558454f398f75c3d75e236e71_202.png)

```python
# 查看系数
for col, coeff in zip(indep_cols, coeffs[:,0]):
    print(f"{col:15} {coeff.item():.4f}")

![](img/82ab127558454f398f75c3d75e236e71_204.png)

![](img/82ab127558454f398f75c3d75e236e71_206.png)

![](img/82ab127558454f398f75c3d75e236e71_208.png)

# 计算准确率
preds = calc_preds(coeffs, x_norm)
acc = ((preds > 0.5).float() == y).float().mean()
print(f"Accuracy: {acc:.4f}")
```

对于二分类问题，线性模型的输出是任意实数。为了将其映射到[0,1]区间（表示概率），我们可以在最后应用 **Sigmoid函数**。这通常能使模型更容易训练。

![](img/82ab127558454f398f75c3d75e236e71_210.png)

![](img/82ab127558454f398f75c3d75e236e71_212.png)

![](img/82ab127558454f398f75c3d75e236e71_214.png)

```python
def calc_preds(coeffs, x): return torch.sigmoid(x @ coeffs).view(-1, 1)
# 重新训练模型，通常可以使用更大的学习率
coeffs = train_model(lr=2)
```

![](img/82ab127558454f398f75c3d75e236e71_216.png)

![](img/82ab127558454f398f75c3d75e236e71_218.png)

![](img/82ab127558454f398f75c3d75e236e71_220.png)

![](img/82ab127558454f398f75c3d75e236e71_222.png)

## 从零构建神经网络

![](img/82ab127558454f398f75c3d75e236e71_224.png)

![](img/82ab127558454f398f75c3d75e236e71_226.png)

![](img/82ab127558454f398f75c3d75e236e71_228.png)

线性模型是神经网络的基础。一个简单的神经网络（多层感知机）在线性层之间加入了非线性激活函数（如ReLU）。

### 单隐藏层神经网络

![](img/82ab127558454f398f75c3d75e236e71_230.png)

![](img/82ab127558454f398f75c3d75e236e71_232.png)

我们构建一个包含一个隐藏层的神经网络：
1.  第一层：输入特征 -> 隐藏层（例如20个神经元），使用ReLU激活函数。
2.  第二层：隐藏层 -> 输出层（1个神经元），使用Sigmoid函数输出概率。

![](img/82ab127558454f398f75c3d75e236e71_234.png)

```python
def init_coeffs(n_hidden=20):
    # 第一层系数矩阵：输入特征数 x 隐藏层神经元数
    l1 = (torch.rand(x.shape[1], n_hidden)-0.5)/n_hidden
    # 第二层系数矩阵：隐藏层神经元数 x 1
    l2 = (torch.rand(n_hidden, 1)-0.3)
    # 常数项（偏置）
    const = torch.rand(1)[0]
    return l1.requires_grad_(), l2.requires_grad_(), const.requires_grad_()

def calc_preds(coeffs, x):
    l1, l2, const = coeffs
    # 第一层线性变换 + ReLU激活
    res = torch.relu(x @ l1)
    # 第二层线性变换 + 常数项 + Sigmoid激活
    res = torch.sigmoid(res @ l2 + const)
    return res

![](img/82ab127558454f398f75c3d75e236e71_236.png)

def update_coeffs(coeffs, lr):
    for layer in coeffs:
        layer.data -= lr * layer.grad.data
        layer.grad.zero_()

![](img/82ab127558454f398f75c3d75e236e71_238.png)

![](img/82ab127558454f398f75c3d75e236e71_240.png)

![](img/82ab127558454f398f75c3d75e236e71_242.png)

![](img/82ab127558454f398f75c3d75e236e71_244.png)

# 训练过程与线性模型类似，但需要更新所有层的系数
```

![](img/82ab127558454f398f75c3d75e236e71_246.png)

![](img/82ab127558454f398f75c3d75e236e71_248.png)

### 深度神经网络

![](img/82ab127558454f398f75c3d75e236e71_250.png)

![](img/82ab127558454f398f75c3d75e236e71_252.png)

我们可以轻松地将网络扩展到多个隐藏层，只需初始化更多层的系数，并在前向传播中依次进行线性变换和ReLU激活（最后一层除外）。

![](img/82ab127558454f398f75c3d75e236e71_254.png)

![](img/82ab127558454f398f75c3d75e236e71_256.png)

![](img/82ab127558454f398f75c3d75e236e71_258.png)

![](img/82ab127558454f398f75c3d75e236e71_260.png)

![](img/82ab127558454f398f75c3d75e236e71_262.png)

![](img/82ab127558454f398f75c3d75e236e71_263.png)

![](img/82ab127558454f398f75c3d75e236e71_265.png)

```python
def init_coeffs(hidden_layers=[10, 10]):
    sz = [x.shape[1]] + hidden_layers + [1]
    layers = [((torch.rand(sz[i], sz[i+1])-0.5)/sz[i+1]).requires_grad_() for i in range(len(sz)-1)]
    consts = [torch.rand(1)[0].requires_grad_() for _ in range(len(sz)-1)]
    return layers, consts

![](img/82ab127558454f398f75c3d75e236e71_267.png)

def calc_preds(coeffs, x):
    layers, consts = coeffs
    res = x
    for i, (l, c) in enumerate(zip(layers, consts)):
        res = res @ l + c
        if i != len(layers)-1: # 如果不是最后一层，使用ReLU激活
            res = torch.relu(res)
        else: # 最后一层使用Sigmoid激活
            res = torch.sigmoid(res)
    return res
```

![](img/82ab127558454f398f75c3d75e236e71_269.png)

![](img/82ab127558454f398f75c3d75e236e71_270.png)

![](img/82ab127558454f398f75c3d75e236e71_272.png)

对于泰坦尼克号这样的小型、特征简单的数据集，深度神经网络可能不会比线性模型有显著提升。特征工程（如从姓名中提取头衔）有时更为关键。

![](img/82ab127558454f398f75c3d75e236e71_274.png)

![](img/82ab127558454f398f75c3d75e236e71_276.png)

![](img/82ab127558454f398f75c3d75e236e71_278.png)

![](img/82ab127558454f398f75c3d75e236e71_280.png)

![](img/82ab127558454f398f75c3d75e236e71_282.png)

![](img/82ab127558454f398f75c3d75e236e71_284.png)

## 使用FastAI框架

![](img/82ab127558454f398f75c3d75e236e71_286.png)

![](img/82ab127558454f398f75c3d75e236e71_288.png)

![](img/82ab127558454f398f75c3d75e236e71_289.png)

从零构建有助于理解原理，但在实践中使用框架（如FastAI）可以极大提升效率，它自动化处理了数据预处理、模型构建和训练中的许多繁琐细节。

![](img/82ab127558454f398f75c3d75e236e71_291.png)

![](img/82ab127558454f398f75c3d75e236e71_292.png)

![](img/82ab127558454f398f75c3d75e236e71_294.png)

### 简化流程

![](img/82ab127558454f398f75c3d75e236e71_296.png)

使用FastAI，只需几行代码即可完成数据加载、预处理和模型训练。

![](img/82ab127558454f398f75c3d75e236e71_298.png)

![](img/82ab127558454f398f75c3d75e236e71_300.png)

![](img/82ab127558454f398f75c3d75e236e71_302.png)

![](img/82ab127558454f398f75c3d75e236e71_304.png)

![](img/82ab127558454f398f75c3d75e236e71_306.png)

```python
from fastai.tabular.all import *
# 定义预处理步骤
procs = [Categorify, FillMissing, Normalize]
# 定义分类变量和连续变量
cat_names = ['Pclass', 'Sex', 'Embarked']
cont_names = ['Age', 'SibSp', 'Parch', 'LogFare']
y_names = 'Survived'
# 创建数据加载器
dls = TabularDataLoaders.from_df(df, path, procs=procs, cat_names=cat_names, cont_names=cont_names, y_names=y_names, y_block=CategoryBlock(), splits=splits)
# 创建学习器并寻找合适的学习率
learn = tabular_learner(dls, layers=[10,10])
learn.lr_find()
# 训练模型
learn.fit(10, lr=0.03)
```

![](img/82ab127558454f398f75c3d75e236e71_308.png)

![](img/82ab127558454f398f75c3d75e236e71_310.png)

![](img/82ab127558454f398f75c3d75e236e71_312.png)

![](img/82ab127558454f398f75c3d75e236e71_314.png)

![](img/82ab127558454f398f75c3d75e236e71_316.png)

### 模型集成

![](img/82ab127558454f398f75c3d75e236e71_318.png)

![](img/82ab127558454f398f75c3d75e236e71_319.png)

集成多个模型是提升性能的简单有效方法。我们可以训练多个相同结构的模型，然后对它们的预测结果取平均。

![](img/82ab127558454f398f75c3d75e236e71_321.png)

![](img/82ab127558454f398f75c3d75e236e71_323.png)

![](img/82ab127558454f398f75c3d75e236e71_325.png)

```python
def ensemble():
    learn = tabular_learner(dls, layers=[10,10])
    with learn.no_logging(): learn.fit(10, lr=0.03)
    return learn.get_preds(dl=test_dl)[0]

![](img/82ab127558454f398f75c3d75e236e71_327.png)

![](img/82ab127558454f398f75c3d75e236e71_329.png)

![](img/82ab127558454f398f75c3d75e236e71_331.png)

preds = torch.stack([ensemble() for _ in range(5)])
avg_preds = preds.mean(dim=0)
```

![](img/82ab127558454f398f75c3d75e236e71_333.png)

## 决策树与随机森林基础

![](img/82ab127558454f398f75c3d75e236e71_335.png)

![](img/82ab127558454f398f75c3d75e236e71_337.png)

![](img/82ab127558454f398f75c3d75e236e71_339.png)

![](img/82ab127558454f398f75c3d75e236e71_341.png)

![](img/82ab127558454f398f75c3d75e236e71_342.png)

![](img/82ab127558454f398f75c3d75e236e71_344.png)

最后，我们简要介绍了另一种强大的表格数据建模方法：决策树。其核心思想是寻找最佳的**二元分割**，将数据分成两组，使得每组内部的目标变量尽可能相似。

![](img/82ab127558454f398f75c3d75e236e71_346.png)

![](img/82ab127558454f398f75c3d75e236e71_348.png)

![](img/82ab127558454f398f75c3d75e236e71_350.png)

### 寻找最佳分割点

![](img/82ab127558454f398f75c3d75e236e71_352.png)

![](img/82ab127558454f398f75c3d75e236e71_354.png)

![](img/82ab127558454f398f75c3d75e236e71_356.png)

我们可以定义一个简单的分数来衡量分割的好坏：`分数 = 左组样本数 * 左组标准差 + 右组样本数 * 右组标准差`。分数越低，说明两组内部越纯净，分割越好。

![](img/82ab127558454f398f75c3d75e236e71_358.png)

![](img/82ab127558454f398f75c3d75e236e71_360.png)

![](img/82ab127558454f398f75c3d75e236e71_362.png)

![](img/82ab127558454f398f75c3d75e236e71_364.png)

对于每个特征（无论是连续还是分类），我们都可以尝试所有可能的分割点，找到分数最低的那个。

![](img/82ab127558454f398f75c3d75e236e71_366.png)

![](img/82ab127558454f398f75c3d75e236e71_368.png)

```python
def _side_score(side, y):
    tot = side.sum()
    if tot <= 1: return 0
    return y[side].std() * tot

![](img/82ab127558454f398f75c3d75e236e71_370.png)

![](img/82ab127558454f398f75c3d75e236e71_372.png)

![](img/82ab127558454f398f75c3d75e236e71_374.png)

![](img/82ab127558454f398f75c3d75e236e71_376.png)

def score(col, y, split):
    lhs = col <= split
    return (_side_score(lhs, y) + _side_score(~lhs, y)) / len(y)

![](img/82ab127558454f398f75c3d75e236e71_378.png)

# 对于连续特征“年龄”，尝试所有唯一值作为分割点
scores = torch.tensor([score(df['Age'], y, split) for split in df['Age'].unique()])
best_idx = scores.argmin()
best_split = df['Age'].unique()[best_idx]
print(f"Best split for Age: {best_split}, Score: {scores[best_idx]:.4f}")
```

![](img/82ab127558454f398f75c3d75e236e71_380.png)

![](img/82ab127558454f398f75c3d75e236e71_381.png)

![](img/82ab127558454f398f75c3d75e236e71_383.png)

![](img/82ab127558454f398f75c3d75e236e71_385.png)

![](img/82ab127558454f398f75c3d75e236e71_387.png)

![](img/82ab127558454f398f75c3d75e236e71_389.png)

通过在所有特征中寻找最佳分割，我们发现按“性别”分割是一个简单而有效的基线模型（1R规则）。在下一课中，我们将学习如何递归地应用这一过程来构建完整的决策树和随机森林。

![](img/82ab127558454f398f75c3d75e236e71_391.png)

![](img/82ab127558454f398f75c3d75e236e71_393.png)

![](img/82ab127558454f398f75c3d75e236e71_395.png)

![](img/82ab127558454f398f75c3d75e236e71_397.png)

## 总结

![](img/82ab127558454f398f75c3d75e236e71_398.png)

![](img/82ab127558454f398f75c3d75e236e71_400.png)

本节课中我们一起学习了如何从零开始构建和训练线性模型与神经网络来处理表格数据。我们涵盖了数据预处理、模型实现、梯度下降训练以及模型评估的全过程。同时，我们也看到了使用FastAI这样的高级框架如何简化工作流程，并初步探索了决策树的基础。理解这些底层原理是有效使用高级工具和进行模型调试的关键。