#  023：Vizuara【中英⚡从零开始构建神经网络｜Building Neural Networks from Scratch】 p23 P23 Lecture 23 - Learning Rate Decay in Neural Network Optimization

🎼大家好，欢迎来到神经网络从零开始的系列课程。今天，我们将探讨一个非常重要的主题——学习率衰减，以及它在神经网络优化中的重要性。

## 学习率衰减概述

学习率衰减是一种在训练过程中逐渐减小学习率的策略。其目的是防止模型在训练过程中过拟合，并提高模型的泛化能力。

### 学习率衰减的原理

学习率衰减的原理可以通过以下公式描述：

$$
 \text{new\_learning\_rate} = \text{initial\_learning\_rate} \times \text{decay\_rate}^{t} 
$$

其中，$ \text{new\_learning\_rate} $ 表示新的学习率，$ \text{initial\_learning\_rate} $ 表示初始学习率，$ \text{decay\_rate} $ 表示衰减率，$ t $ 表示训练轮数。

![](img/e159b44885737b126c086d84223eb3a2_0.png)

## 学习率衰减的方法

以下是几种常见的学习率衰减方法：

### 1. 线性衰减

线性衰减是指学习率以线性方式逐渐减小。其公式如下：

$$
 \text{new\_learning\_rate} = \text{initial\_learning\_rate} - \text{decay\_step} \times \text{decay\_rate} 
$$

其中，$ \text{decay\_step} $ 表示每经过多少轮训练衰减一次学习率。

### 2. 指数衰减

指数衰减是指学习率以指数方式逐渐减小。其公式如下：

$$
 \text{new\_learning\_rate} = \text{initial\_learning\_rate} \times \text{decay\_rate}^{t} 
$$

### 3. 余弦退火

余弦退火是指学习率以余弦方式逐渐减小。其公式如下：

$$
 \text{new\_learning\_rate} = \text{initial\_learning\_rate} \times \frac{1 + \cos(\pi \times t / \text{epochs})}{2} 
$$

其中，$ \text{epochs} $ 表示训练的总轮数。

## 总结

本节课中，我们学习了学习率衰减的概念、原理以及常见方法。学习率衰减是神经网络优化中的一种重要策略，可以帮助我们提高模型的泛化能力。希望这节课的内容对大家有所帮助。