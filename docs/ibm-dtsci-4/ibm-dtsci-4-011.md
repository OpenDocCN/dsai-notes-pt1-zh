# 011：对象与类 🧩

![](img/bb4d1704e49a91687dc23a7d4e7854f0_0.png)

在本节课中，我们将学习Python中的**对象**与**类**。我们将探讨什么是对象，如何创建自定义的类，以及如何使用类的方法来操作对象的数据。理解这些概念是掌握Python面向对象编程的基础。

---

## 🧠 什么是对象？

![](img/bb4d1704e49a91687dc23a7d4e7854f0_2.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_3.png)

Python中的每种数据类型（如整数、浮点数、字符串、列表、字典、布尔值）都是一个**对象**。每个对象都具备以下特征：

![](img/bb4d1704e49a91687dc23a7d4e7854f0_4.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_5.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_6.png)

*   **类型**：标识对象的种类。
*   **内部表示**：对象内部存储数据的方式。
*   **方法**：一组用于与对象数据交互的函数。

对象是特定类型的**实例**。例如，我们可以有多个整数对象，每个都是“整数”类型的实例。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_8.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_9.png)

### 对象实例示例

![](img/bb4d1704e49a91687dc23a7d4e7854f0_11.png)

每次创建一个整数，我们就在创建“整数”类型的一个实例，或者说一个整数对象。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_13.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_14.png)

```python
# 创建五个整数对象（实例）
num1 = 1
num2 = 2
num3 = 3
num4 = 4
num5 = 5
```

同样，每次创建一个列表，我们就在创建“列表”类型的一个实例，或者说一个列表对象。

```python
# 创建五个列表对象（实例）
list1 = [1, 2]
list2 = [3, 4, 5]
list3 = ['a', 'b']
list4 = []
list5 = [6, 7, 8, 9]
```

我们可以使用 `type()` 命令来查看对象的类型。

```python
print(type([1, 2, 3]))   # 输出：<class 'list'>
print(type(5))           # 输出：<class 'int'>
print(type("Hello"))     # 输出：<class 'str'>
print(type({'a': 1}))    # 输出：<class 'dict'>
```

![](img/bb4d1704e49a91687dc23a7d4e7854f0_16.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_17.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_18.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_19.png)

---

![](img/bb4d1704e49a91687dc23a7d4e7854f0_20.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_21.png)

## 🔧 类与方法

一个**类**（或类型）的**方法**是该类每个实例都提供的函数。方法是我们与对象交互的方式。

我们一直在使用方法。例如，列表的 `sort()` 方法可以改变对象内部的数据。

我们通过在对象名称后加一个点 `.` 和方法名（后跟括号）来调用方法。

### 方法操作示例

![](img/bb4d1704e49a91687dc23a7d4e7854f0_23.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_24.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_25.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_26.png)

考虑一个列表 `ratings`。`sort()` 方法会改变对象内部的数据（即列表元素的顺序）。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_27.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_28.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_29.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_30.png)

```python
ratings = [2, 5, 3, 1, 4]
ratings.sort()  # 调用 sort 方法
print(ratings)  # 输出：[1, 2, 3, 4, 5]
```

![](img/bb4d1704e49a91687dc23a7d4e7854f0_31.png)

我们还可以调用 `reverse()` 方法再次改变列表。

```python
ratings.reverse()  # 调用 reverse 方法
print(ratings)     # 输出：[5, 4, 3, 2, 1]
```

![](img/bb4d1704e49a91687dc23a7d4e7854f0_33.png)

在许多情况下，你无需了解类及其方法的内部工作原理，只需知道如何使用它们即可。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_34.png)

---

![](img/bb4d1704e49a91687dc23a7d4e7854f0_36.png)

## 🏗️ 创建自定义类

上一节我们了解了Python内置的类和方法。本节中，我们将学习如何创建自己的类。

在Python中，你可以创建自己的类型或类。一个类包含：
*   **数据属性**：定义类的数据。
*   **方法**：用于与数据交互的函数。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_38.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_39.png)

然后，我们可以创建该类的**实例**（即对象）。

### 定义类的数据属性

类的数据属性定义了该类。让我们创建两个类：`Circle`（圆形）和 `Rectangle`（矩形）。

*   **对于圆形**：定义一个圆需要半径。我们还可以添加颜色属性，以便后续区分不同的实例。
    *   因此，`Circle` 类的数据属性是：`radius`（半径）和 `color`（颜色）。
*   **对于矩形**：定义一个矩形需要高度和宽度。同样，我们添加颜色属性。
    *   因此，`Rectangle` 类的数据属性是：`color`（颜色）、`height`（高度）和 `width`（宽度）。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_41.png)

### 类的代码结构

![](img/bb4d1704e49a91687dc23a7d4e7854f0_42.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_43.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_44.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_45.png)

要创建 `Circle` 类，需要包含类定义。`class` 关键字告诉Python你正在创建自己的类。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_46.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_47.png)

```python
class Circle(object):
    # 类定义体
    pass

class Rectangle(object):
    # 类定义体
    pass
```
*   `class` 后跟类名（如 `Circle`）。
*   括号中的 `object` 是该类的父类（在本课程中始终如此）。
*   `pass` 是一个占位符，表示暂时不写具体内容。

类只是一个蓝图。我们必须设置属性才能创建对象。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_49.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_50.png)

---

![](img/bb4d1704e49a91687dc23a7d4e7854f0_51.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_52.png)

## 🎨 创建类的实例（对象）

![](img/bb4d1704e49a91687dc23a7d4e7854f0_54.png)

我们可以创建 `Circle` 类型的对象实例。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_55.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_56.png)

```python
# 创建 Circle 类的实例
circle1 = Circle(radius=4, color='red')
circle2 = Circle(radius=2, color='green')

![](img/bb4d1704e49a91687dc23a7d4e7854f0_58.png)

# 创建 Rectangle 类的实例
rect1 = Rectangle(height=2, width=2, color='blue')
rect2 = Rectangle(height=1, width=3, color='yellow')
```

![](img/bb4d1704e49a91687dc23a7d4e7854f0_59.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_61.png)

现在，我们有了 `Circle` 类的不同对象，以及 `Rectangle` 类的不同对象。

---

![](img/bb4d1704e49a91687dc23a7d4e7854f0_63.png)

## ⚙️ 深入类定义：构造函数与 `self`

![](img/bb4d1704e49a91687dc23a7d4e7854f0_65.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_66.png)

让我们继续用Python构建 `Circle` 类。我们定义类，然后使用类构造函数 `__init__` 来初始化每个实例的数据属性（`radius` 和 `color`）。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_68.png)

```python
class Circle(object):
    def __init__(self, radius, color):
        self.radius = radius
        self.color = color
```

![](img/bb4d1704e49a91687dc23a7d4e7854f0_69.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_71.png)

*   `__init__` 函数是一个**构造函数**。它是一个特殊函数，告诉Python你正在创建一个新类。
*   `self` 参数指向**新创建的类实例**。
*   `radius` 和 `color` 参数用于初始化类实例的 `self.radius` 和 `self.color` 数据属性。

我们可以类似地定义 `Rectangle` 类。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_73.png)

```python
class Rectangle(object):
    def __init__(self, color, height, width):
        self.color = color
        self.height = height
        self.width = width
```

这次，类的数据属性是 `color`、`height` 和 `width`。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_75.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_76.png)

### 理解对象创建过程

![](img/bb4d1704e49a91687dc23a7d4e7854f0_78.png)

创建类之后，为了创建 `Circle` 类的对象，我们引入一个变量作为对象名称，并使用对象构造函数。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_80.png)

```python
# 创建 Circle 对象
my_circle = Circle(10, 'red')
```

![](img/bb4d1704e49a91687dc23a7d4e7854f0_81.png)

*   对象构造函数由类名和参数（即数据属性）组成。
*   当我们创建 `Circle` 对象时，我们像调用函数一样调用它。传递给 `Circle` 构造函数的参数用于初始化新创建的 `Circle` 实例的数据属性。

将 `self` 想象成一个包含对象所有数据属性的盒子会很有帮助。

### 访问与修改数据属性

![](img/bb4d1704e49a91687dc23a7d4e7854f0_83.png)

输入对象名称，后跟一个点 `.` 和数据属性名称，可以获取数据属性的值。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_84.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_85.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_86.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_87.png)

```python
print(my_circle.radius)  # 输出：10
print(my_circle.color)   # 输出：'red'
```

![](img/bb4d1704e49a91687dc23a7d4e7854f0_88.png)

在Python中，我们也可以直接设置或更改数据属性。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_90.png)

```python
my_circle.color = 'blue'  # 直接修改颜色属性
print(my_circle.color)    # 输出：'blue'
```

![](img/bb4d1704e49a91687dc23a7d4e7854f0_92.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_93.png)

通常，为了改变对象中的数据，我们在类中定义**方法**。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_95.png)

---

![](img/bb4d1704e49a91687dc23a7d4e7854f0_97.png)

## 🛠️ 为类添加方法

我们已经看到数据属性如何构成定义对象的数据。**方法**是用于交互和更改数据属性的函数。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_99.png)

假设我们想改变一个圆的大小，这涉及到更改 `radius` 属性。我们给 `Circle` 类添加一个 `add_radius` 方法。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_101.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_102.png)

```python
class Circle(object):
    def __init__(self, radius, color):
        self.radius = radius
        self.color = color

    def add_radius(self, r):
        self.radius = self.radius + r
```

![](img/bb4d1704e49a91687dc23a7d4e7854f0_104.png)

*   该方法是一个函数，需要 `self` 参数以及其他参数（这里是要添加的值 `r`）。
*   在方法体内，我们将 `r` 加到数据属性 `self.radius` 上。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_106.png)

### 调用方法

我们创建一个对象并调用 `add_radius` 方法。

```python
my_circle = Circle(2, 'red')  # 创建对象，radius=2, color='red'
print(my_circle.radius)       # 输出：2

![](img/bb4d1704e49a91687dc23a7d4e7854f0_108.png)

my_circle.add_radius(8)       # 调用方法，增加半径8
print(my_circle.radius)       # 输出：10
```

![](img/bb4d1704e49a91687dc23a7d4e7854f0_109.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_110.png)

*   我们通过添加点 `.` 后跟方法名和括号来调用方法。
*   调用方法时，我们**无需担心 `self` 参数**，Python会为我们处理。
*   在许多情况下，除了 `self`，方法定义中可能没有指定任何其他参数，因此调用函数时无需传递任何参数。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_112.png)

当调用 `add_radius` 方法时，它会通过更改 `radius` 数据属性的值来改变对象。

---

![](img/bb4d1704e49a91687dc23a7d4e7854f0_114.png)

## 📝 实践与总结

在实验环节，我们还可以为类的构造函数参数添加默认值，并创建诸如 `draw_circle` 这样的方法（具体实现请参见实验）。

我们可以使用 `dir()` 函数来获取与类相关的数据属性和方法列表。将感兴趣的对象作为参数传递，返回值是该对象的属性和方法列表。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_116.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_117.png)

![](img/bb4d1704e49a91687dc23a7d4e7854f0_118.png)

```python
print(dir(my_circle))
```

输出中，被下划线包围的属性（如 `__init__`）是内部使用的，通常无需担心。常规外观的属性才是你需要关注的，它们是对象的方法和数据属性。

![](img/bb4d1704e49a91687dc23a7d4e7854f0_120.png)

**本节课总结**：
我们一起学习了Python中**对象**与**类**的核心概念。我们了解到对象是类的实例，拥有类型、内部数据和方法。我们学会了如何定义自己的类（如 `Circle` 和 `Rectangle`），包括使用 `__init__` 构造函数初始化数据属性，以及如何为类添加方法来操作对象数据。最后，我们还了解了如何创建类的实例（对象），并访问或修改其属性。掌握这些是进行面向对象编程的重要基础。