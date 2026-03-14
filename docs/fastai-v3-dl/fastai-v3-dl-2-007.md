# 深度学习基础到稳定扩散模型：7：反向传播 🔄

![](img/5149affbe02aaa27d202cb5dce5b7d13_1.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_3.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_5.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_7.png)

在本节课中，我们将要学习神经网络的核心训练算法——反向传播。我们将从一个简单的多层感知机（MLP）开始，逐步推导并实现其前向传播和反向传播过程，最终训练一个能够识别手写数字的模型。

## 概述 📋

反向传播是训练神经网络的关键算法，它通过计算损失函数相对于网络参数的梯度，并使用梯度下降法来更新这些参数。本节课我们将从零开始，手动实现一个多层感知机的反向传播，并理解其背后的数学原理——链式法则。

---

![](img/5149affbe02aaa27d202cb5dce5b7d13_9.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_11.png)

## 神经网络基础回顾 🧠

![](img/5149affbe02aaa27d202cb5dce5b7d13_13.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_15.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_17.png)

上一节我们介绍了张量操作，本节中我们来看看神经网络的基本构成。

![](img/5149affbe02aaa27d202cb5dce5b7d13_19.png)

一个最简单的线性模型可以表示为：`y = w * x + b`。其中，`x`是输入（例如一个像素值），`w`是权重，`b`是偏置，`y`是输出（例如该像素属于数字“3”的概率）。

![](img/5149affbe02aaa27d202cb5dce5b7d13_20.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_22.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_24.png)

然而，单个线性模型表达能力有限。为了拟合更复杂的函数，我们可以组合多个“修正线性单元”（ReLU）。ReLU函数定义为：`f(x) = max(0, x)`。通过将多个ReLU“线段”相加，我们可以逼近任意形状的曲线。

![](img/5149affbe02aaa27d202cb5dce5b7d13_26.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_28.png)

当输入是多个像素（例如784个）时，我们就需要处理多维空间中的“平面”。原理是相同的：通过矩阵乘法组合多个输入，然后应用ReLU非线性激活函数，再将结果通过另一个线性层映射到输出空间（例如10个数字类别）。

![](img/5149affbe02aaa27d202cb5dce5b7d13_30.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_32.png)

以下是构建一个简单多层感知机（MLP）所需的步骤：
1.  定义网络结构：输入层、隐藏层（带ReLU激活）、输出层。
2.  初始化权重和偏置。
3.  实现前向传播函数。
4.  定义损失函数（如均方误差MSE或交叉熵损失）。
5.  实现反向传播以计算梯度。
6.  使用梯度下降更新参数。

![](img/5149affbe02aaa27d202cb5dce5b7d13_34.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_36.png)

---

## 实现前向传播 ➡️

![](img/5149affbe02aaa27d202cb5dce5b7d13_38.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_40.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_42.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_44.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_46.png)

首先，我们定义网络参数并实现前向传播过程。

![](img/5149affbe02aaa27d202cb5dce5b7d13_48.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_50.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_52.png)

```python
import torch
import torch.nn.functional as F

# 定义网络尺寸
n, m, c = 50000, 784, 10  # 样本数， 像素数， 类别数
nh = 50  # 隐藏层神经元数量

# 初始化权重和偏置 (随机初始化权重，偏置初始为0)
w1 = torch.randn(m, nh) / m**0.5  # 第一层权重
b1 = torch.zeros(nh)              # 第一层偏置
w2 = torch.randn(nh, 1)           # 第二层权重 (简化版，单输出)
b2 = torch.zeros(1)               # 第二层偏置

# 线性层函数
def lin(x, w, b):
    return x @ w + b

![](img/5149affbe02aaa27d202cb5dce5b7d13_54.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_56.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_58.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_60.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_62.png)

# ReLU激活函数
def relu(x):
    return x.clamp_min(0.)

![](img/5149affbe02aaa27d202cb5dce5b7d13_64.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_66.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_68.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_70.png)

# 模型前向传播 (简化版，单输出)
def model(xb):
    l1 = lin(xb, w1, b1)  # 第一层线性变换
    l2 = relu(l1)         # ReLU激活
    l3 = lin(l2, w2, b2)  # 第二层线性变换
    return l3

![](img/5149affbe02aaa27d202cb5dce5b7d13_72.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_74.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_76.png)

# 计算均方误差损失 (MSE)
def mse(preds, targets):
    # 处理广播问题：确保preds是向量而非列向量
    diff = preds.squeeze() - targets.float()
    return (diff ** 2).mean()
```

![](img/5149affbe02aaa27d202cb5dce5b7d13_78.png)

我们使用验证集数据测试前向传播，并计算初始的MSE损失。由于权重是随机的，初始损失会很大。

![](img/5149affbe02aaa27d202cb5dce5b7d13_80.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_81.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_82.png)

---

## 理解梯度与链式法则 📈

![](img/5149affbe02aaa27d202cb5dce5b7d13_84.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_86.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_87.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_89.png)

为了优化网络，我们需要计算损失函数相对于每个权重（`w1`, `b1`, `w2`, `b2`）的梯度。梯度是一个多元函数的偏导数向量，它指向函数值增长最快的方向。

![](img/5149affbe02aaa27d202cb5dce5b7d13_91.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_93.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_94.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_96.png)

对于神经网络这种复合函数，我们使用**链式法则**来计算梯度。链式法则指出，对于函数 `z = f(y)` 和 `y = g(x)`，`z` 关于 `x` 的导数为：
`dz/dx = (dz/dy) * (dy/dx)`

![](img/5149affbe02aaa27d202cb5dce5b7d13_98.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_100.png)

在神经网络中，损失 `L` 是最终输出 `o` 的函数，`o` 又是前一层激活 `a` 的函数，依此类推。因此，计算 `dL/dw1` 需要将路径上所有中间导数相乘：
`dL/dw1 = (dL/do) * (do/da2) * (da2/dz2) * (dz2/da1) * (da1/dz1) * (dz1/dw1)`

![](img/5149affbe02aaa27d202cb5dce5b7d13_102.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_103.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_105.png)

这个过程从最终损失开始，向后逐层传播梯度，因此被称为**反向传播**。

![](img/5149affbe02aaa27d202cb5dce5b7d13_107.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_109.png)

一个直观的理解方式是想象一组联动的齿轮。第一个齿轮（输入 `x`）的转动会带动中间齿轮（激活 `a`），最终带动最后一个齿轮（损失 `L`）。链式法则计算的就是 `x` 转动对 `L` 产生的总影响，等于每一级齿轮传动比的乘积。

---

## 手动实现反向传播 ⚙️

![](img/5149affbe02aaa27d202cb5dce5b7d13_111.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_113.png)

我们将为每个操作（线性层、ReLU、MSE损失）实现其反向传播函数，计算并存储梯度。

```python
def mse_grad(preds, targets):
    # MSE损失 L = mean((preds - targets)^2)
    # dL/d(preds) = 2 * (preds - targets) / n
    diff = preds.squeeze() - targets.float()
    return 2. * diff.unsqueeze(-1) / preds.shape[0]  # 梯度形状需与preds匹配

![](img/5149affbe02aaa27d202cb5dce5b7d13_115.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_117.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_118.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_120.png)

def lin_grad(input, output, weight, bias, output_grad):
    # 线性层 output = input @ weight + bias
    # dL/d(input) = output_grad @ weight.T
    # dL/d(weight) = input.T @ output_grad
    # dL/d(bias) = sum(output_grad, dim=0)
    input_grad = output_grad @ weight.T
    weight_grad = input.T @ output_grad
    bias_grad = output_grad.sum(0)
    return input_grad, weight_grad, bias_grad

![](img/5149affbe02aaa27d202cb5dce5b7d13_122.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_124.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_126.png)

def relu_grad(input, output_grad):
    # ReLU层 output = max(0, input)
    # d(output)/d(input) = 1 if input > 0 else 0
    # dL/d(input) = output_grad * (input > 0)
    return output_grad * (input > 0).float()
```

![](img/5149affbe02aaa27d202cb5dce5b7d13_128.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_130.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_132.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_134.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_136.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_138.png)

然后，我们按照计算图的反向顺序调用这些函数：

![](img/5149affbe02aaa27d202cb5dce5b7d13_140.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_142.png)

```python
# 前向传播
l1 = lin(xb, w1, b1)
l2 = relu(l1)
l3 = lin(l2, w2, b2)
loss = mse(l3, yb)

![](img/5149affbe02aaa27d202cb5dce5b7d13_144.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_146.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_148.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_150.png)

# 反向传播
l3_grad = mse_grad(l3, yb)          # 计算损失层梯度
l2_grad, w2_grad, b2_grad = lin_grad(l2, l3, w2, b2, l3_grad) # 计算第二层梯度
l1_grad = relu_grad(l1, l2_grad)    # 计算ReLU层梯度
_, w1_grad, b1_grad = lin_grad(xb, l1, w1, b1, l1_grad) # 计算第一层梯度
```

![](img/5149affbe02aaa27d202cb5dce5b7d13_152.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_153.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_154.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_155.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_157.png)

现在，`w1_grad`, `b1_grad`, `w2_grad`, `b2_grad` 就包含了损失相对于每个参数的梯度。我们可以用PyTorch的自动微分来验证我们手动计算的梯度是否正确。

---

![](img/5149affbe02aaa27d202cb5dce5b7d13_159.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_161.png)

## 模块化重构：创建Layer类 🧩

![](img/5149affbe02aaa27d202cb5dce5b7d13_163.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_164.png)

上面的代码比较冗长且重复。我们可以通过创建通用的 `Module` 类来重构，每个层（线性、ReLU、MSE）都继承自它。`Module` 类负责在正向传播时存储输入和输出，以便在反向传播时使用。

![](img/5149affbe02aaa27d202cb5dce5b7d13_166.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_168.png)

```python
class Module:
    def __call__(self, *args):
        self.args = args          # 存储输入参数
        self.out = self.forward(*args) # 计算并存储输出
        return self.out

    def forward(self): raise NotImplementedError
    def backward(self): raise NotImplementedError

![](img/5149affbe02aaa27d202cb5dce5b7d13_170.png)

class Relu(Module):
    def forward(self, inp):
        return inp.clamp_min(0.)
    def backward(self):
        inp, = self.args
        self.inp_grad = (self.out > 0).float() * self.out_grad

![](img/5149affbe02aaa27d202cb5dce5b7d13_172.png)

class Lin(Module):
    def __init__(self, w, b):
        self.w, self.b = w, b
    def forward(self, inp):
        return inp @ self.w + self.b
    def backward(self):
        inp, = self.args
        self.inp_grad = self.out_grad @ self.w.T
        self.w.grad = inp.T @ self.out_grad
        self.b.grad = self.out_grad.sum(0)

![](img/5149affbe02aaa27d202cb5dce5b7d13_174.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_176.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_178.png)

class Mse(Module):
    def forward(self, inp, targ):
        diff = inp.squeeze() - targ.float()
        return (diff ** 2).mean()
    def backward(self):
        inp, targ = self.args
        diff = inp.squeeze() - targ.float()
        self.inp_grad = (2. * diff.unsqueeze(-1)) / inp.shape[0]
```

![](img/5149affbe02aaa27d202cb5dce5b7d13_180.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_181.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_183.png)

然后，我们的模型可以简洁地定义为一系列层的组合，反向传播只需按逆序调用各层的 `.backward()` 方法。这种设计与PyTorch的 `nn.Module` 设计思想一致。

![](img/5149affbe02aaa27d202cb5dce5b7d13_185.png)

---

![](img/5149affbe02aaa27d202cb5dce5b7d13_187.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_189.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_191.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_193.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_195.png)

## 使用交叉熵损失改进模型 🎯

![](img/5149affbe02aaa27d202cb5dce5b7d13_197.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_199.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_201.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_203.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_205.png)

之前我们使用了均方误差（MSE）损失，但对于分类问题，交叉熵损失是更标准且效果更好的选择。交叉熵损失与Softmax激活函数通常结合使用。

![](img/5149affbe02aaa27d202cb5dce5b7d13_207.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_209.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_210.png)

Softmax函数将神经网络的原始输出（logits）转换为概率分布：
`softmax(x_i) = exp(x_i) / sum(exp(x_j)) for j in all classes`

![](img/5149affbe02aaa27d202cb5dce5b7d13_212.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_214.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_216.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_218.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_219.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_221.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_223.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_225.png)

为了避免数值计算不稳定（指数运算可能产生极大值），我们使用 **Log-Softmax Trick**：
`log_softmax(x_i) = x_i - log(sum(exp(x_j - max(x)))) - max(x)`
这样在计算对数时更加稳定。

![](img/5149affbe02aaa27d202cb5dce5b7d13_227.png)

对于分类问题，目标标签通常是整数形式（如“3”）。交叉熵损失可以高效地计算为：
`loss = -log(softmax_preds[target])` 的均值。

![](img/5149affbe02aaa27d202cb5dce5b7d13_229.png)

PyTorch中，`F.cross_entropy` 函数已经集成了 `log_softmax` 和 `negative log likelihood loss` 的计算。

![](img/5149affbe02aaa27d202cb5dce5b7d13_231.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_233.png)

```python
def log_softmax(x):
    # 使用log-sum-exp技巧实现稳定的log_softmax
    m = x.max(-1, keepdim=True).values
    exp_x = (x - m).exp()
    log_sum_exp = exp_x.sum(-1, keepdim=True).log()
    return x - m - log_sum_exp

def cross_entropy_loss(preds, targets):
    # preds: [batch_size, n_classes]
    # targets: [batch_size] (整数标签)
    log_probs = log_softmax(preds)
    # 使用高级索引获取每个目标类别对应的对数概率
    nll = -log_probs[range(targets.shape[0]), targets].mean()
    return nll
```

![](img/5149affbe02aaa27d202cb5dce5b7d13_235.png)

---

![](img/5149affbe02aaa27d202cb5dce5b7d13_237.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_239.png)

## 实现训练循环 🔁

![](img/5149affbe02aaa27d202cb5dce5b7d13_241.png)

现在，我们将所有部分组合起来，实现一个完整的训练循环。我们使用小批量梯度下降（Mini-batch SGD）。

以下是训练循环的关键步骤：
1.  将数据划分为小批量（batch）。
2.  对每个批量执行前向传播，计算预测和损失。
3.  执行反向传播，计算梯度。
4.  根据梯度和学习率更新模型参数。
5.  重复多个周期（epoch）。

![](img/5149affbe02aaa27d202cb5dce5b7d13_243.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_245.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_247.png)

```python
lr = 0.1  # 学习率
epochs = 20
bs = 64   # 批量大小

![](img/5149affbe02aaa27d202cb5dce5b7d13_249.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_251.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_253.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_255.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_257.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_259.png)

for epoch in range(epochs):
    for i in range(0, n, bs):
        # 1. 获取小批量数据
        xb, yb = x_train[i:i+bs], y_train[i:i+bs]

        # 2. 前向传播 (使用改进后的模型，输出10个类别)
        # ... (假设 model() 现在输出 [batch_size, 10])
        preds = model(xb)
        loss = cross_entropy_loss(preds, yb)

        # 3. 反向传播
        loss.backward()  # 如果使用我们的Module类，需要手动调用各层.backward()

        # 4. 更新参数 (梯度下降)
        with torch.no_grad():
            for p in [w1, b1, w2, b2]:
                p -= lr * p.grad
                p.grad.zero_()  # 清零梯度，为下一轮准备

    # 每个epoch后可以计算并打印训练集准确率
    train_acc = (model(x_train).argmax(1) == y_train).float().mean()
    print(f"Epoch {epoch}, Loss: {loss.item():.4f}, Acc: {train_acc:.4f}")
```

![](img/5149affbe02aaa27d202cb5dce5b7d13_261.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_263.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_264.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_265.png)

经过几个周期的训练，我们的手写数字识别模型在训练集上的准确率可以显著提升。

![](img/5149affbe02aaa27d202cb5dce5b7d13_267.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_269.png)

---

![](img/5149affbe02aaa27d202cb5dce5b7d13_271.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_272.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_273.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_275.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_277.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_279.png)

## 总结 🎓

![](img/5149affbe02aaa27d202cb5dce5b7d13_281.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_283.png)

本节课中我们一起学习了神经网络训练的核心——反向传播算法。

![](img/5149affbe02aaa27d202cb5dce5b7d13_285.png)

我们从一个简单的多层感知机出发，回顾了神经网络的基础。然后，我们深入探讨了梯度的概念和链式法则，这是理解反向传播的数学基础。接着，我们手动实现了线性层、ReLU激活层和损失层的反向传播计算。

![](img/5149affbe02aaa27d202cb5dce5b7d13_287.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_289.png)

为了使代码更清晰和可复用，我们引入了模块化设计，创建了通用的 `Module` 类以及具体的层类。我们还改进了损失函数，用更适合分类任务的交叉熵损失替代了均方误差损失，并解释了其实现细节和数值稳定技巧（Log-Softmax Trick）。

![](img/5149affbe02aaa27d202cb5dce5b7d13_291.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_293.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_295.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_296.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_298.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_300.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_301.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_303.png)

最后，我们将所有组件整合到一个完整的训练循环中，使用小批量梯度下降成功训练了一个手写数字识别模型。

![](img/5149affbe02aaa27d202cb5dce5b7d13_305.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_306.png)

![](img/5149affbe02aaa27d202cb5dce5b7d13_308.png)

通过本节课的学习，你应该对神经网络的内部运作机制，特别是梯度如何从损失函数向后传播并用于更新权重，有了扎实的理解。这是理解更复杂深度学习模型的重要基石。