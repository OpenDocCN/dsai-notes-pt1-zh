# 117：28_实现支持向量机核模型 🧠

![](img/f443b8657d8dc12b6691807f9af07549_1.png)

![](img/f443b8657d8dc12b6691807f9af07549_2.png)

在本节课中，我们将学习如何使用Scikit-learn库中的核近似方法，将数据映射到高维空间，以便使用线性分类器处理非线性问题。我们将重点介绍`Nystroem`和`RBF Sampler`两种核近似方法。

![](img/f443b8657d8dc12b6691807f9af07549_4.png)

## 概述

![](img/f443b8657d8dc12b6691807f9af07549_5.png)

支持向量机在处理非线性可分数据时，通常需要使用核技巧将数据映射到高维空间。然而，直接计算核函数可能计算成本高昂。核近似方法提供了一种高效的替代方案，它通过采样和变换来近似核函数的效果，从而允许我们使用线性模型来解决非线性问题。

上一节我们介绍了核方法的基本概念，本节中我们来看看如何用代码实现两种具体的核近似方法。

![](img/f443b8657d8dc12b6691807f9af07549_7.png)

![](img/f443b8657d8dc12b6691807f9af07549_8.png)

![](img/f443b8657d8dc12b6691807f9af07549_9.png)

## 实现Nystroem核近似

![](img/f443b8657d8dc12b6691807f9af07549_10.png)

首先，我们需要导入所需的类。

![](img/f443b8657d8dc12b6691807f9af07549_12.png)

![](img/f443b8657d8dc12b6691807f9af07549_13.png)

```python
from sklearn.kernel_approximation import Nystroem
```

导入后，我们将创建该类的一个实例。在初始化时，我们需要指定核函数类型及其他超参数。

以下是创建Nystroem实例时需要考虑的关键参数：

![](img/f443b8657d8dc12b6691807f9af07549_15.png)

![](img/f443b8657d8dc12b6691807f9af07549_16.png)

*   **`kernel`**: 指定使用的核函数，例如径向基函数（RBF）或多项式核（polynomial）。你可以查阅文档了解其他可用的核函数。
*   **`gamma`**: 核函数的系数参数，其含义与SVC模型中的`gamma`参数相同。
*   **`n_components`**: 用于构建核近似的样本数量。这个值越小，采样的数据点越少，计算速度越快，但近似精度可能降低；值越大，则越接近使用整个数据集进行近似的效果，但计算时间会更长。

![](img/f443b8657d8dc12b6691807f9af07549_17.png)

![](img/f443b8657d8dc12b6691807f9af07549_18.png)

![](img/f443b8657d8dc12b6691807f9af07549_19.png)

创建实例后，我们需要在数据上拟合并应用这个变换器。

![](img/f443b8657d8dc12b6691807f9af07549_20.png)

```python
# 在训练集上拟合并变换
X_train_approx = nystroem.fit_transform(X_train)

![](img/f443b8657d8dc12b6691807f9af07549_22.png)

![](img/f443b8657d8dc12b6691807f9af07549_23.png)

# 使用从训练集学到的参数变换测试集
X_test_approx = nystroem.transform(X_test)
```

![](img/f443b8657d8dc12b6691807f9af07549_25.png)

这里，我们对训练集调用`fit_transform`方法，因为它需要根据训练数据学习映射规则并将其应用到训练数据本身。对于测试集，我们只调用`transform`方法，使用从训练集学习到的参数进行变换，这与标准化（StandardScaler）的处理逻辑一致。

![](img/f443b8657d8dc12b6691807f9af07549_26.png)

![](img/f443b8657d8dc12b6691807f9af07549_28.png)

最后，我们可以结合交叉验证来调整核函数的参数。在实践中，建议创建一个管道（Pipeline）。

![](img/f443b8657d8dc12b6691807f9af07549_30.png)

```python
from sklearn.pipeline import Pipeline
from sklearn.model_selection import GridSearchCV
from sklearn.linear_model import SGDClassifier

# 创建包含核近似和线性分类器的管道
pipeline = Pipeline([
    ('kernel_approx', Nystroem()),
    ('linear_clf', SGDClassifier())
])

![](img/f443b8657d8dc12b6691807f9af07549_32.png)

![](img/f443b8657d8dc12b6691807f9af07549_33.png)

![](img/f443b8657d8dc12b6691807f9af07549_34.png)

# 定义参数网格进行搜索
param_grid = {
    'kernel_approx__kernel': ['rbf', 'poly'],
    'kernel_approx__gamma': [0.01, 0.1, 1],
    'kernel_approx__n_components': [50, 100, 200]
}

![](img/f443b8657d8dc12b6691807f9af07549_35.png)

grid_search = GridSearchCV(pipeline, param_grid, cv=5)
grid_search.fit(X_train, y_train)
```

通过网格搜索，我们可以为变换器（核近似）和管道内的线性分类器寻找最优的超参数组合，并在验证集上评估最佳性能。

## 实现RBF采样器核近似

![](img/f443b8657d8dc12b6691807f9af07549_37.png)

![](img/f443b8657d8dc12b6691807f9af07549_38.png)

![](img/f443b8657d8dc12b6691807f9af07549_39.png)

另一种核近似方法是RBF采样器。它的使用流程与Nystroem类似。

![](img/f443b8657d8dc12b6691807f9af07549_40.png)

![](img/f443b8657d8dc12b6691807f9af07549_41.png)

![](img/f443b8657d8dc12b6691807f9af07549_42.png)

首先，导入`RBFSampler`类。

![](img/f443b8657d8dc12b6691807f9af07549_44.png)

```python
from sklearn.kernel_approximation import RBFSampler
```

![](img/f443b8657d8dc12b6691807f9af07549_45.png)

接着，创建`RBFSampler`类的实例。对于RBF采样器，`kernel`参数固定为RBF核。

![](img/f443b8657d8dc12b6691807f9af07549_47.png)

![](img/f443b8657d8dc12b6691807f9af07549_48.png)

以下是RBFSampler的主要参数：

![](img/f443b8657d8dc12b6691807f9af07549_50.png)

![](img/f443b8657d8dc12b6691807f9af07549_51.png)

*   **`gamma`**: RBF核的系数。
*   **`n_components`**: 近似特征的维度数量，作用与Nystroem中的`n_components`类似。

![](img/f443b8657d8dc12b6691807f9af07549_53.png)

![](img/f443b8657d8dc12b6691807f9af07549_54.png)

然后，同样在数据上进行拟合和变换。

```python
rbf_sampler = RBFSampler(gamma=1, n_components=100)
X_train_approx = rbf_sampler.fit_transform(X_train)
X_test_approx = rbf_sampler.transform(X_test)
```

![](img/f443b8657d8dc12b6691807f9af07549_56.png)

![](img/f443b8657d8dc12b6691807f9af07549_57.png)

与Nystroem方法一样，我们也可以使用交叉验证和管道来优化`gamma`和`n_components`等参数，并将RBF采样器与一个线性分类器结合使用。

## 总结

![](img/f443b8657d8dc12b6691807f9af07549_59.png)

![](img/f443b8657d8dc12b6691807f9af07549_60.png)

![](img/f443b8657d8dc12b6691807f9af07549_61.png)

本节课中我们一起学习了两种实现支持向量机核模型的近似方法：`Nystroem`和`RBF Sampler`。这两种方法的核心思想都是通过高效的采样和线性变换，将原始数据隐式地映射到高维特征空间，从而使得线性分类器能够处理非线性决策边界。关键步骤包括导入类、初始化实例、在训练集上拟合变换、在测试集上应用变换，以及通过构建管道和交叉验证来优化超参数。掌握这些方法能让你在处理大规模非线性数据时，在计算效率和模型性能之间取得更好的平衡。