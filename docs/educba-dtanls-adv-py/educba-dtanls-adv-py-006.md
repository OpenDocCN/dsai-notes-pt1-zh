# 006：构建自定义类

![](img/25dda7d56f1773d8f2190fc7f575ca7e_0.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_2.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_4.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_6.png)

在本节课中，我们将学习如何构建自定义类。类是创建自定义数据结构的核心工具，它允许我们将数据（属性）和操作数据的方法（函数）封装在一起。我们将通过创建一个物联网设备管理类来理解其基本概念和用法。

![](img/25dda7d56f1773d8f2190fc7f575ca7e_8.png)

## 概述

![](img/25dda7d56f1773d8f2190fc7f575ca7e_10.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_12.png)

上一节我们介绍了Python的基础数据结构。本节中，我们来看看如何创建自己的数据结构——类。我们将学习如何定义类、初始化对象、添加属性和方法，以及使用类属性来跟踪对象数量。

![](img/25dda7d56f1773d8f2190fc7f575ca7e_14.png)

## 创建类与 `__init__` 方法

![](img/25dda7d56f1773d8f2190fc7f575ca7e_16.png)

要创建自定义类，需要使用 `class` 关键字。`__init__` 方法是一个特殊方法，每当创建该类的一个新对象（实例）时，它会被自动调用。它的主要作用是对新创建的对象进行初始化。

以下是创建一个名为 `IoTDevices` 的类的语法：

```python
class IoTDevices:
    def __init__(self):
        print("一个 IoTDevices 类的对象已被创建。")
```

![](img/25dda7d56f1773d8f2190fc7f575ca7e_18.png)

当创建对象时，`__init__` 方法会自动执行：

![](img/25dda7d56f1773d8f2190fc7f575ca7e_19.png)

```python
rasppy = IoTDevices()
# 输出：一个 IoTDevices 类的对象已被创建。
```

![](img/25dda7d56f1773d8f2190fc7f575ca7e_21.png)

## 为对象添加属性

在 `__init__` 方法中，我们可以接收参数并将其赋值给对象的属性。属性是绑定到特定对象上的变量，可以通过 `self.属性名` 来访问和存储。

以下是定义带有属性的 `__init__` 方法的示例：

![](img/25dda7d56f1773d8f2190fc7f575ca7e_23.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_25.png)

```python
class IoTDevices:
    def __init__(self, name, device_type, cost):
        self.name = name
        self.type = device_type
        self.cost = cost
        print(f"一个 {self.name} 设备对象已被创建。")
```

![](img/25dda7d56f1773d8f2190fc7f575ca7e_27.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_29.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_31.png)

创建对象并访问其属性：

![](img/25dda7d56f1773d8f2190fc7f575ca7e_33.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_35.png)

```python
device1 = IoTDevices("Raspberry Pi", "控制器", 3500)
print(f"设备名称: {device1.name}")
print(f"设备成本: {device1.cost}")
```

![](img/25dda7d56f1773d8f2190fc7f575ca7e_36.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_37.png)

## 为类添加方法

![](img/25dda7d56f1773d8f2190fc7f575ca7e_39.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_40.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_41.png)

方法是定义在类内部的函数，用于描述对象可以执行的操作。方法的第一个参数通常是 `self`，它代表调用该方法的对象实例。

![](img/25dda7d56f1773d8f2190fc7f575ca7e_43.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_45.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_47.png)

以下是为 `IoTDevices` 类添加一个 `print_details` 方法的示例：

![](img/25dda7d56f1773d8f2190fc7f575ca7e_49.png)

```python
class IoTDevices:
    def __init__(self, name, device_type, cost):
        self.name = name
        self.type = device_type
        self.cost = cost

    def print_details(self):
        print(f"设备名称是 {self.name}，类型是 {self.type}，成本是 {self.cost}。")
```

![](img/25dda7d56f1773d8f2190fc7f575ca7e_51.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_53.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_54.png)

使用方法：

![](img/25dda7d56f1773d8f2190fc7f575ca7e_56.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_58.png)

```python
device1 = IoTDevices("Raspberry Pi", "控制器", 3500)
device2 = IoTDevices("数字万用表", "仪器", 500)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_60.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_61.png)

device1.print_details()
device2.print_details()
```

![](img/25dda7d56f1773d8f2190fc7f575ca7e_63.png)

## 使用类属性与静态方法

![](img/25dda7d56f1773d8f2190fc7f575ca7e_65.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_66.png)

类属性是属于类本身的变量，被所有实例共享。静态方法使用 `@staticmethod` 装饰器定义，它不依赖于任何特定的对象实例，可以通过类名直接调用。

![](img/25dda7d56f1773d8f2190fc7f575ca7e_68.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_69.png)

以下是使用类属性和静态方法来跟踪创建了多少个设备的示例：

![](img/25dda7d56f1773d8f2190fc7f575ca7e_71.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_72.png)

```python
class IoTDevices:
    # 类属性，用于计数
    __count = 0  # 双下划线开头表示私有变量

    def __init__(self, name, device_type, cost):
        self.name = name
        self.type = device_type
        self.cost = cost
        IoTDevices.__count += 1  # 每创建一个对象，计数加1

    def print_details(self):
        print(f"设备名称是 {self.name}，类型是 {self.type}，成本是 {self.cost}。")

    @staticmethod
    def get_count():
        """静态方法，返回已创建的对象数量"""
        return IoTDevices.__count
```

![](img/25dda7d56f1773d8f2190fc7f575ca7e_74.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_75.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_77.png)

使用静态方法获取计数：

![](img/25dda7d56f1773d8f2190fc7f575ca7e_79.png)

```python
device1 = IoTDevices("Raspberry Pi", "控制器", 3500)
print(f"当前设备数量: {IoTDevices.get_count()}")

![](img/25dda7d56f1773d8f2190fc7f575ca7e_81.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_83.png)

device2 = IoTDevices("数字万用表", "仪器", 500)
print(f"当前设备数量: {device2.get_count()}")  # 也可以通过对象调用
```

## 总结

![](img/25dda7d56f1773d8f2190fc7f575ca7e_85.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_86.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_87.png)

![](img/25dda7d56f1773d8f2190fc7f575ca7e_88.png)

本节课中我们一起学习了Python中类的构建。我们掌握了如何通过 `class` 关键字定义类，使用 `__init__` 方法初始化对象属性，以及如何为类添加实例方法和静态方法。我们还了解了类属性的概念，并学会了使用私有变量和静态方法来管理类级别的数据。通过创建 `IoTDevices` 这个自定义类，你现在已经能够构建属于自己的数据结构来组织和处理数据了。