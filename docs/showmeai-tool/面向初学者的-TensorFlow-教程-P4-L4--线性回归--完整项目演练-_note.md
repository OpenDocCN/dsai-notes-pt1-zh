# TensorFlow 教程 P4：L4- 线性回归完整项目演练 🚗

![](img/938d327f6471f60f92b4ed5bae0092f3_0.png)

在本教程中，我们将实现第一个真实世界的项目，处理一个回归问题。我们将学习如何加载和分析数据，进行数据预处理，并应用线性回归模型。之后，我们还会将模型扩展为深度神经网络，以更好地理解Keras层和激活函数的作用。

## 导入必要的库 📚

首先，我们需要导入项目所需的库。这些库包括用于数据处理的`pandas`，用于可视化的`matplotlib`，以及TensorFlow和Keras的核心模块。

```python
import warnings
warnings.filterwarnings('ignore')

import matplotlib.pyplot as plt
import pandas as pd

pd.set_option('display.max_columns', None)
pd.set_option('display.width', None)

import tensorflow as tf
from tensorflow import keras
from tensorflow.keras import layers
from tensorflow.keras.layers.experimental import preprocessing
```

## 加载与探索数据集 🔍

我们将使用经典的“Auto MPG”数据集，该数据集包含1983年各种汽车的特征，目标是预测每加仑燃油行驶的英里数（MPG）。

以下是加载和初步查看数据的步骤：

```python
url = 'http://archive.ics.uci.edu/ml/machine-learning-databases/auto-mpg/auto-mpg.data'
column_names = ['MPG', 'Cylinders', 'Displacement', 'Horsepower', 'Weight', 'Acceleration', 'Model Year', 'Origin']

raw_dataset = pd.read_csv(url, names=column_names, na_values='?', comment='\t', sep=' ', skipinitialspace=True)
dataset = raw_dataset.copy()
print(dataset.tail())
```

## 数据清洗与预处理 🧹

数据集中可能存在缺失值，并且“Origin”列是分类数据，我们需要对其进行处理。

以下是数据清洗的步骤：
*   首先，使用 `dropna()` 方法删除包含缺失值的行。
*   接着，将“Origin”列转换为独热编码（One-Hot Encoding），以便模型能够处理。

```python
dataset = dataset.dropna()
origin = dataset.pop('Origin')
dataset['USA'] = (origin == 1)*1
dataset['Europe'] = (origin == 2)*1
dataset['Japan'] = (origin == 3)*1
print(dataset.tail())
```

## 划分训练集与测试集 📊

我们需要将数据分为训练集和测试集，以评估模型的泛化能力。

```python
train_dataset = dataset.sample(frac=0.8, random_state=0)
test_dataset = dataset.drop(train_dataset.index)

print(f"训练集形状: {train_dataset.shape}")
print(f"测试集形状: {test_dataset.shape}")
```

我们可以使用 `describe()` 方法查看数据的基本统计信息。

```python
train_stats = train_dataset.describe()
train_stats.pop("MPG")
train_stats = train_stats.transpose()
print(train_stats)
```

## 分离特征与标签 🏷️

我们的目标是预测“MPG”，因此需要将其从特征中分离出来作为标签。

```python
train_labels = train_dataset.pop('MPG')
test_labels = test_dataset.pop('MPG')
```

## 数据可视化 📈

在建模前，可视化数据有助于理解特征与目标之间的关系。例如，我们可以绘制“马力”与“MPG”的散点图。

```python
def plot(feature, x=None, y=None):
    plt.figure(figsize=(10, 8))
    plt.scatter(train_dataset[feature], train_labels, label='Data')
    if x is not None:
        plt.plot(x, y, color='k', label='Predictions')
    plt.xlabel(feature)
    plt.ylabel('MPG')
    plt.legend()
    plt.show()

plot('Horsepower')
```

## 数据标准化 ⚖️

不同特征具有不同的量纲和范围（例如，“排量”的数值远大于“气缸数”），这可能导致模型训练困难。标准化是一个常见的预处理步骤，它将数据转换为均值为0、标准差为1的分布。

标准化公式为：`(x - mean) / std`

我们使用Keras的预处理层来实现：

```python
normalizer = preprocessing.Normalization()
normalizer.adapt(np.array(train_dataset))
print(normalizer.mean.numpy())

first = np.array(train_dataset[:1])
print('原始示例:', first)
print('标准化后:', normalizer(first).numpy())
```

## 构建单变量线性回归模型 🧱

为了简化理解，我们先使用一个特征（例如“马力”）来构建线性回归模型。线性回归的公式可以表示为：`y = wx + b`

在Keras中，一个带有单个输出单元且没有激活函数的`Dense`层就构成了一个线性回归模型。

以下是构建和编译模型的步骤：

```python
horsepower = np.array(train_dataset['Horsepower'])
horsepower_normalizer = preprocessing.Normalization(input_shape=[1,])
horsepower_normalizer.adapt(horsepower)

horsepower_model = tf.keras.Sequential([
    horsepower_normalizer,
    layers.Dense(units=1)
])

horsepower_model.summary()
```

定义损失函数和优化器。对于回归问题，常用均方误差（MSE）或平均绝对误差（MAE）。

```python
horsepower_model.compile(
    optimizer=tf.optimizers.Adam(learning_rate=0.1),
    loss='mean_absolute_error')
```

## 训练与评估模型 🏋️

现在，我们可以用数据来训练这个单特征模型。

```python
history = horsepower_model.fit(
    train_dataset['Horsepower'],
    train_labels,
    epochs=100,
    verbose=1,
    validation_split = 0.2)

plot('Horsepower', x, y)
```

训练完成后，我们在测试集上评估模型性能。

```python
test_results = {}
test_results['horsepower_model'] = horsepower_model.evaluate(
    test_dataset['Horsepower'],
    test_labels, verbose=0)
print(f"测试集MAE: {test_results['horsepower_model']}")
```

## 使用单变量模型进行预测 🔮

我们可以用训练好的模型对新数据进行预测，并将预测结果可视化。

```python
x = tf.linspace(0.0, 250, 251)
y = horsepower_model.predict(x)
plot('Horsepower', x, y)
```

## 构建多变量线性回归模型 🌐

上一节我们介绍了如何使用单个特征进行线性回归。本节中，我们来看看如何使用所有特征来构建一个更强大的多变量线性回归模型。方法几乎相同，只是输入变成了所有特征。

```python
linear_model = tf.keras.Sequential([
    normalizer,
    layers.Dense(units=1)
])

linear_model.compile(
    optimizer=tf.optimizers.Adam(learning_rate=0.1),
    loss='mean_absolute_error')

history = linear_model.fit(
    train_dataset,
    train_labels,
    epochs=100,
    verbose=1,
    validation_split = 0.2)

test_results['linear_model'] = linear_model.evaluate(
    test_dataset, test_labels, verbose=0)
print(f"多变量模型测试集MAE: {test_results['linear_model']}")
```

## 构建深度神经网络（DNN） 🧠

线性模型能力有限。为了捕捉特征间更复杂的非线性关系，我们可以构建一个深度神经网络。这通过在模型中添加多个带有激活函数（如ReLU）的隐藏层来实现。

以下是构建一个简单DNN的步骤：

```python
dnn_model = tf.keras.Sequential([
    normalizer,
    layers.Dense(64, activation='relu'),
    layers.Dense(64, activation='relu'),
    layers.Dense(1)
])

dnn_model.compile(
    optimizer=tf.optimizers.Adam(learning_rate=0.001),
    loss='mean_absolute_error')

dnn_model.summary()
```

训练这个DNN模型。

```python
history = dnn_model.fit(
    train_dataset,
    train_labels,
    epochs=100,
    verbose=1,
    validation_split = 0.2)
```

评估DNN模型，并与线性模型进行比较。

```python
test_results['dnn_model'] = dnn_model.evaluate(test_dataset, test_labels, verbose=0)
print(f"DNN模型测试集MAE: {test_results['dnn_model']}")
print(pd.DataFrame(test_results, index=['Mean Absolute Error [MPG]']).T)
```

## 总结 📝

在本教程中，我们一起完成了一个完整的回归项目演练。我们学习了：
1.  如何使用`pandas`加载和清洗CSV格式的数据集。
2.  如何通过标准化对数据进行预处理。
3.  如何构建、编译和训练一个单变量及多变量的**线性回归模型**（`y = wx + b`）。
4.  如何将简单的线性模型扩展为**深度神经网络**，并理解了激活函数（如ReLU）在引入非线性能力中的关键作用。
5.  整个过程涵盖了模型训练、评估和预测的基本工作流。

通过这个项目，你应该对使用TensorFlow/Keras解决实际问题有了一个扎实的初步认识。