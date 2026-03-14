# 004：线性与逻辑回归代码重构 🛠️

在本节课中，我们将学习如何重构线性回归和逻辑回归的代码。通过识别并提取两个模型中的公共代码，我们将创建一个基类，从而使代码结构更清晰、更易于维护。

## 概述

在前两节中，我们分别实现了线性回归和逻辑回归模型。通过对比代码，我们发现这两个类有许多相似之处。本节我们将通过重构，提取公共逻辑到基类中，以减少代码重复并提升代码质量。

## 代码相似性分析

上一节我们介绍了线性回归和逻辑回归的独立实现。本节中我们来看看它们的代码结构。

![](img/deb40f827b6fb8d4c9fd1a3029854cea_1.png)

![](img/deb40f827b6fb8d4c9fd1a3029854cea_2.png)

![](img/deb40f827b6fb8d4c9fd1a3029854cea_3.png)

通过比较代码，可以发现两个类非常相似。

![](img/deb40f827b6fb8d4c9fd1a3029854cea_4.png)

以下是主要的相似点：
*   两个类拥有完全相同的 `__init__` 方法。
*   两个类拥有几乎相同的 `fit` 方法。
*   在两个类中，我们都初始化参数，并执行相同的梯度下降过程。

以下是核心的区别：
*   在线性回归中，我们使用线性模型计算预测值 `y_approx`。
*   在逻辑回归中，我们同样使用线性模型，但随后会应用 **sigmoid** 函数。

![](img/deb40f827b6fb8d4c9fd1a3029854cea_6.png)

![](img/deb40f827b6fb8d4c9fd1a3029854cea_8.png)

线性回归的预测模型为：
`y_approx = X · W + b`

逻辑回归的预测模型为：
`y_approx = σ(X · W + b)`

![](img/deb40f827b6fb8d4c9fd1a3029854cea_10.png)

其中，σ 代表 **sigmoid** 函数。

![](img/deb40f827b6fb8d4c9fd1a3029854cea_12.png)

这种差异同样体现在 `predict` 方法中。在线性回归中，我们直接应用线性模型；在逻辑回归中，我们先应用线性模型，再应用 **sigmoid** 函数，最后根据阈值（如0.5）判断输出为1或0。

![](img/deb40f827b6fb8d4c9fd1a3029854cea_14.png)

此外，我们还有一个辅助函数 `_sigmoid`。

![](img/deb40f827b6fb8d4c9fd1a3029854cea_16.png)

鉴于大量代码是相似的，接下来让我们开始重构。

![](img/deb40f827b6fb8d4c9fd1a3029854cea_18.png)

## 创建基类

![](img/deb40f827b6fb8d4c9fd1a3029854cea_20.png)

![](img/deb40f827b6fb8d4c9fd1a3029854cea_22.png)

首先，我们创建一个名为 `BaseRegression` 的基类，然后让 `LinearRegression` 和 `LogisticRegression` 类继承自这个基类。

```python
class BaseRegression:
    # 基类代码将放在这里

![](img/deb40f827b6fb8d4c9fd1a3029854cea_24.png)

![](img/deb40f827b6fb8d4c9fd1a3029854cea_25.png)

class LinearRegression(BaseRegression):
    # 线性回归类

class LogisticRegression(BaseRegression):
    # 逻辑回归类
```

## 提取公共方法

由于 `__init__` 方法在两个子类中完全相同，我们可以将其剪切并放入基类中。

![](img/deb40f827b6fb8d4c9fd1a3029854cea_27.png)

```python
class BaseRegression:
    def __init__(self, learning_rate=0.001, n_iters=1000):
        self.lr = learning_rate
        self.n_iters = n_iters
        self.weights = None
        self.bias = None
```

![](img/deb40f827b6fb8d4c9fd1a3029854cea_29.png)

接下来，我们处理 `fit` 方法。它的逻辑也几乎相同，因此我们也可以将其移到基类中。

![](img/deb40f827b6fb8d4c9fd1a3029854cea_31.png)

基类 `fit` 方法的核心是梯度下降循环。唯一的不同在于计算预测值 `y_predicted` 的方式：一次需要简单的线性模型，另一次需要线性模型加 **sigmoid** 函数。

为了解决这个差异，我们创建一个名为 `_approximation` 的辅助方法。它接收数据 `X`、权重 `W` 和偏置 `b` 作为参数。在基类中，我们将其定义为抽象方法，抛出一个 `NotImplementedError`，强制子类去实现它。

![](img/deb40f827b6fb8d4c9fd1a3029854cea_33.png)

![](img/deb40f827b6fb8d4c9fd1a3029854cea_34.png)

```python
class BaseRegression:
    # ... 其他代码 ...
    def _approximation(self, X, W, b):
        raise NotImplementedError
```

![](img/deb40f827b6fb8d4c9fd1a3029854cea_36.png)

## 实现子类特定逻辑

![](img/deb40f827b6fb8d4c9fd1a3029854cea_38.png)

![](img/deb40f827b6fb8d4c9fd1a3029854cea_40.png)

现在，在 `LinearRegression` 类中，我们实现 `_approximation` 方法。这里就是简单的线性模型。

```python
class LinearRegression(BaseRegression):
    def _approximation(self, X, W, b):
        return np.dot(X, W) + b
```

![](img/deb40f827b6fb8d4c9fd1a3029854cea_42.png)

同样地，我们从 `LogisticRegression` 类中移除原有的 `fit` 方法逻辑，并实现其特有的 `_approximation` 方法。这里需要先计算线性模型，再应用 **sigmoid** 函数。

```python
class LogisticRegression(BaseRegression):
    def _approximation(self, X, W, b):
        linear_model = np.dot(X, W) + b
        return self._sigmoid(linear_model)
```

## 重构预测方法

![](img/deb40f827b6fb8d4c9fd1a3029854cea_44.png)

`predict` 方法也略有不同。我们在基类中定义一个公共的 `predict` 接口，并在其中调用一个需要子类实现的辅助方法 `_predict`。

```python
class BaseRegression:
    # ... 其他代码 ...
    def _predict(self, X, W, b):
        raise NotImplementedError

    def predict(self, X):
        return self._predict(X, self.weights, self.bias)
```

在 `LinearRegression` 类中，我们实现 `_predict` 方法，即直接返回线性模型的结果。

![](img/deb40f827b6fb8d4c9fd1a3029854cea_46.png)

```python
class LinearRegression(BaseRegression):
    # ... 其他代码 ...
    def _predict(self, X, W, b):
        return np.dot(X, W) + b
```

在 `LogisticRegression` 类中，我们同样实现 `_predict` 方法：计算线性模型，应用 **sigmoid** 函数，并根据阈值返回0或1的分类结果。

```python
class LogisticRegression(BaseRegression):
    # ... 其他代码 ...
    def _predict(self, X, W, b):
        linear_model = np.dot(X, W) + b
        y_predicted = self._sigmoid(linear_model)
        y_predicted_cls = [1 if i > 0.5 else 0 for i in y_predicted]
        return y_predicted_cls
```

## 最终成果

![](img/deb40f827b6fb8d4c9fd1a3029854cea_48.png)

现在，我们可以将重构后的 `LinearRegression` 和 `LogisticRegression` 类放在同一个文件中。最终，我们用大约60行Python代码实现了两个清晰的回归模型，代码结构得到了显著优化。

## 总结

![](img/deb40f827b6fb8d4c9fd1a3029854cea_50.png)

![](img/deb40f827b6fb8d4c9fd1a3029854cea_51.png)

本节课中，我们一起学习了如何通过重构来优化代码。我们识别了线性回归和逻辑回归中的公共模式，创建了一个 `BaseRegression` 基类来存放共享的初始化、训练流程和预测接口。通过定义抽象的 `_approximation` 和 `_predict` 方法，我们允许子类灵活地实现其特有的核心逻辑。这种设计使代码更简洁、更易于扩展和维护。