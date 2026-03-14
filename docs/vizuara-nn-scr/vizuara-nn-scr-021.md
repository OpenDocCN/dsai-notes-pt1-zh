#  021：Vizuara【中英⚡从零开始构建神经网络｜Building Neural Networks from Scratch】 p21 P21 Lecture 21 - Coding the entire neural network forward backward pass in Python [BV1iEHPzGEpa_p21]

![](img/90a1836f44f204a179305ed4c4dccf99_0.png)

## 🎼Yeah。In the previous lecture， we looked at how to assemble all the back propag building blocks on the whiteboard。

### 在上一节课中，我们学习了如何在白板上组装所有反向传播的构建模块。

## so we saw that there are three three classes which will create one for the layers。

### 因此，我们看到了有三个类，每个类创建一个用于层。

## one for the rail activation and one for the software。

### 一个用于激活函数，一个用于软件。

---

## 🎼Yeah。In the previous lecture， we looked at how to assemble all the back propag building blocks on the whiteboard。

![](img/90a1836f44f204a179305ed4c4dccf99_0.png)

### 在上一节课中，我们学习了如何在白板上组装所有反向传播的构建模块。

### **公式**：反向传播是神经网络训练过程中的关键步骤，它通过计算损失函数相对于网络参数的梯度来更新网络权重。

## so we saw that there are three three classes which will create one for the layers。

### 因此，我们看到了有三个类，每个类创建一个用于层。

### **代码**：
```python
class Layer:
    def __init__(self):
        pass

class Activation:
    def __init__(self):
        pass

class Software:
    def __init__(self):
        pass
```

## one for the rail activation and one for the software。

### 一个用于激活函数，一个用于软件。

### **代码**：
```python
class ActivationFunction:
    def activate(self, x):
        pass

class SoftwareComponent:
    def run(self):
        pass
```

---

## 🎼Yeah。In the previous lecture， we looked at how to assemble all the back propag building blocks on the whiteboard。

### 在上一节课中，我们学习了如何在白板上组装所有反向传播的构建模块。

## so we saw that there are three three classes which will create one for the layers。

### 因此，我们看到了有三个类，每个类创建一个用于层。

## one for the rail activation and one for the software。

### 一个用于激活函数，一个用于软件。

---

## 本节课中我们一起学习了yyy

在本节课中，我们学习了如何使用Python代码构建神经网络中的反向传播过程。我们介绍了三个主要的类：Layer、Activation和Software，以及它们在神经网络中的作用。通过这些知识，我们可以更好地理解神经网络的工作原理。