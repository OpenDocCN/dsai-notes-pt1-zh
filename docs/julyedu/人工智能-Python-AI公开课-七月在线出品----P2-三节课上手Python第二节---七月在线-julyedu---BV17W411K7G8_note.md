# 🐍 人工智能—Python AI公开课（七月在线出品） - P2：三节课上手Python第二节

![](img/fe835b0a2aa000823ea87911b98d27ef_0.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_2.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_4.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_5.png)

在本节课中，我们将要学习Python编程中两个核心概念：**函数**与**面向对象编程**。它们是构建复杂程序、实现代码复用和结构化的基石。我们将通过具体的代码示例，帮助你理解如何创建和使用函数，以及如何运用面向对象的思想来设计程序。

![](img/fe835b0a2aa000823ea87911b98d27ef_7.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_9.png)

---

![](img/fe835b0a2aa000823ea87911b98d27ef_11.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_13.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_15.png)

## 🔄 复习与课程目标

![](img/fe835b0a2aa000823ea87911b98d27ef_17.png)

上一节我们介绍了Python基础、Anaconda环境配置、Jupyter Notebook的使用以及Python的基本数据类型。本节中，我们来看看本节课的两个核心目标。

![](img/fe835b0a2aa000823ea87911b98d27ef_19.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_20.png)

本节课的目标是承前启后：
1.  **了解函数的意义，掌握函数的创建及使用**，包括不定长参数。
2.  **理解面向对象的基本思想**，理解类和对象的关系，了解类的结构和成员，并能根据需求创建包含相应成员的类。

---

## 📦 第一部分：函数

![](img/fe835b0a2aa000823ea87911b98d27ef_22.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_24.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_25.png)

函数是编程中用于封装代码块、实现特定功能的基本单元。它接收输入（参数），经过处理，然后返回输出（结果）。

![](img/fe835b0a2aa000823ea87911b98d27ef_27.png)

### 函数的创建与结构

![](img/fe835b0a2aa000823ea87911b98d27ef_29.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_30.png)

在Python中，使用 `def` 关键字来定义函数。

![](img/fe835b0a2aa000823ea87911b98d27ef_32.png)

```python
def my_add(a, b):
    c = a + b
    return c
```

![](img/fe835b0a2aa000823ea87911b98d27ef_33.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_35.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_37.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_39.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_41.png)

*   `def`：定义函数的关键字。
*   `my_add`：函数名。
*   `(a, b)`：形参列表，定义函数接收的参数。
*   `:`：冒号表示函数头的结束。
*   `c = a + b` 和 `return c`：函数体，包含具体的执行逻辑和返回语句。

![](img/fe835b0a2aa000823ea87911b98d27ef_43.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_45.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_47.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_49.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_50.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_52.png)

**调用函数**时，需要传入实际的参数（实参）：
```python
result = my_add(4, 7)  # 实参为4和7
print(result)  # 输出：11
```

![](img/fe835b0a2aa000823ea87911b98d27ef_54.png)

**函数体**是执行具体操作的地方。**`return`语句**用于指定函数的返回值。如果没有`return`语句，函数默认返回 `None`。

`print()` 和 `return` 的区别在于：`print()` 是将内容输出到屏幕，而 `return` 是将结果返回给调用者。一个函数可以有多个 `print()`，但通常只有一个 `return` 值（或元组等复合结构）作为最终输出。

![](img/fe835b0a2aa000823ea87911b98d27ef_56.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_57.png)

### 实战：实现斐波那契数列函数

斐波那契数列的特点是：从第三项开始，每一项都等于前两项之和（例如：0, 1, 1, 2, 3, 5, 8...）。

![](img/fe835b0a2aa000823ea87911b98d27ef_59.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_61.png)

以下是实现该数列的函数：

```python
def fib(num):
    l1 = [0, 1]  # 初始化数列前两项
    for i in range(num - 2):  # 循环生成后续项
        l1.append(sum(l1[-2:]))  # 将最后两项的和追加到列表
    return l1

![](img/fe835b0a2aa000823ea87911b98d27ef_63.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_64.png)

# 调用函数，生成前8个斐波那契数
print(fib(8))  # 输出：[0, 1, 1, 2, 3, 5, 8, 13]
```

这个函数接收一个参数 `num`，返回一个长度为 `num` 的斐波那契数列列表。它演示了如何将过程化的循环逻辑封装在一个函数中，方便重复调用。

![](img/fe835b0a2aa000823ea87911b98d27ef_66.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_68.png)

### 不定长参数

有时我们希望函数能接受任意数量的参数。这时可以使用不定长参数 `*args`。

```python
def my_add(*args):
    return sum(args)

print(my_add(3, 5, 7))  # 输出：15
print(my_add(1, 2, 3, 4, 5))  # 输出：15
print(my_add())  # 输出：0
```

![](img/fe835b0a2aa000823ea87911b98d27ef_70.png)

`*args` 会将所有传入的位置参数打包成一个元组，在函数体内可以遍历或直接使用。

![](img/fe835b0a2aa000823ea87911b98d27ef_72.png)

---

## 🧠 第二部分：面向对象编程（OOP）

面向对象编程是一种将数据和操作数据的方法组合成“对象”的编程范式。Python中“一切皆对象”。

### 核心概念：类与对象

![](img/fe835b0a2aa000823ea87911b98d27ef_74.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_76.png)

*   **类**：是对具有相同属性和行为的一类事物的抽象描述，相当于蓝图或图纸。例如，“学生”是一个类。
*   **对象**：是类的一个具体实例，是根据类创建出来的实体。例如，名为“张三”的学生是一个对象。

关系：**类**是抽象的模板，**对象**是根据模板创建的具体实体。

### 类的创建与成员

![](img/fe835b0a2aa000823ea87911b98d27ef_78.png)

使用 `class` 关键字定义类。类包含**属性**（变量）和**方法**（函数）。

![](img/fe835b0a2aa000823ea87911b98d27ef_80.png)

以下是一个“学生类”的设计示例，要求能记录学生信息、考试成绩、毕业状态，并能统计整体学生情况。

```python
class Student:
    # 类属性：所有对象共享
    total = 0  # 学生总人数
    name_list = []  # 所有学生姓名列表
    graduated_count = 0  # 已毕业学生人数
    graduate_standard = 1000  # 毕业所需总分

    def __init__(self, name, age, gender):
        # 实例属性：每个对象独有的数据
        self._name = name  # 姓名（私有属性）
        self._age = age  # 年龄
        self._gender = gender  # 性别
        self._score = 0  # 个人总分，初始为0

        # 更新类属性
        Student.total += 1
        Student.name_list.append(name)

    # 实例方法：对象的行为
    def exam(self, score):
        """考试方法，成绩合格则累加分数"""
        if score < 60:
            return "Sorry, you didn't pass."
        else:
            self._score += score
            # 检查是否达到毕业标准
            if self._score >= Student.graduate_standard:
                Student.graduated_count += 1
            return f"Score added. Current total: {self._score}"

    def check_score(self):
        """查询个人当前总分"""
        return self._score

    # 类方法：与类相关，而非特定实例
    @classmethod
    def check_graduated(cls):
        """查询已毕业学生人数"""
        return cls.graduated_count

    @classmethod
    def check_all_students(cls):
        """查询所有学生姓名"""
        return cls.name_list
```

**代码解析：**

1.  **`__init__` 方法**：构造方法，在创建对象时自动调用，用于初始化对象的属性。`self` 代表对象实例本身。
2.  **实例属性**：如 `self._name`，以 `self.` 开头，属于每个对象独有的数据。属性名前加单下划线 `_` 是一种约定，暗示其为“私有”（非强制），建议不直接从外部访问。
3.  **类属性**：如 `total`，在类内部直接定义，属于类本身，所有对象共享。
4.  **实例方法**：第一个参数必须是 `self`，代表调用该方法的对象。如 `exam`, `check_score`。
5.  **类方法**：使用 `@classmethod` 装饰器，第一个参数必须是 `cls`，代表类本身。如 `check_graduated`, `check_all_students`。

### 使用类创建对象

```python
# 创建两个学生对象
david = Student("David", 33, "Male")
jack = Student("Jack", 23, "Male")

![](img/fe835b0a2aa000823ea87911b98d27ef_82.png)

# 调用实例方法：David参加考试
print(david.exam(80))  # Score added. Current total: 80
print(david.exam(90))  # Score added. Current total: 170

![](img/fe835b0a2aa000823ea87911b98d27ef_84.png)

# 查询David的分数
print(david.check_score())  # 170

# 调用类方法：查询整体情况
print(Student.check_all_students())  # ['David', 'Jack']
print(Student.check_graduated())  # 0 (尚未有人毕业)
```

通过这个例子，你可以看到：
*   `david` 和 `jack` 是两个独立的对象，拥有各自的分数。
*   类属性 `total` 和 `name_list` 记录了所有学生的整体信息。
*   当某个学生的分数达到 `graduate_standard` 时，类属性 `graduated_count` 会自动更新。

面向对象编程通过这种方式，将数据（属性）和操作数据的方法（方法）捆绑在一起，使代码结构更清晰，更易于管理和扩展。

---

## 📝 总结

本节课中我们一起学习了Python中两个至关重要的概念。

![](img/fe835b0a2aa000823ea87911b98d27ef_86.png)

首先，我们深入探讨了**函数**。我们学习了如何使用 `def` 关键字定义函数，理解了形参、实参、函数体和返回值的含义，并通过斐波那契数列的例子实践了函数的封装与调用。我们还简要介绍了不定长参数 `*args` 的用法。

![](img/fe835b0a2aa000823ea87911b98d27ef_88.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_90.png)

接着，我们进入了**面向对象编程**的世界。我们理解了“类”作为蓝图和“对象”作为实例的关系。通过设计一个功能完整的“学生类”，我们实践了如何定义类属性、实例属性、实例方法以及类方法。我们看到了 `self` 和 `cls` 参数的作用，并初步了解了封装的思想。

![](img/fe835b0a2aa000823ea87911b98d27ef_92.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_93.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_94.png)

掌握函数和面向对象是编写结构化、可复用Python代码的关键。建议你根据课堂示例和留下的作业（如“创建素数查找函数”、“设计公司类”）进行练习，这是巩固知识的最佳途径。

![](img/fe835b0a2aa000823ea87911b98d27ef_96.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_97.png)

![](img/fe835b0a2aa000823ea87911b98d27ef_98.png)

下节课，我们将学习用于科学计算的 `NumPy` 库和用于数据分析的 `Pandas` 库，进入数据处理的实际应用阶段。