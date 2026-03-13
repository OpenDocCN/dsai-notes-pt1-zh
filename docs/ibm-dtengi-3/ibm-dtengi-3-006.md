# 006：Python样式指南与编码实践

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_0.png)

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_2.png)

在本节课中，我们将学习如何编写清晰、一致且易于维护的Python代码。我们将重点介绍PEP 8样式指南的核心原则、关键的编码惯例，并了解如何使用静态代码分析工具来确保代码质量。

---

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_4.png)

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_5.png)

## 🎯 编写可读代码的重要性

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_7.png)

当你编写代码时，需要确保团队成员能够轻松阅读和理解它。这项任务需要遵循一定的编码标准和惯例。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_8.png)

Python官方发布了一份名为 **Python增强提案第8号（PEP 8）** 的文档，它提供了使Python代码可读且格式一致的惯例和指南。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_10.png)

---

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_11.png)

## ✨ 提升代码可读性的关键指南

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_13.png)

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_15.png)

上一节我们介绍了PEP 8的重要性，本节中我们来看看提升代码可读性的具体指南。

### 缩进：使用空格而非制表符

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_17.png)

PEP 8建议使用空格而非制表符进行缩进。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_18.png)

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_19.png)

不同的文本编辑器和集成开发环境（IDE）对制表符所代表空格数的解释可能不同。例如，一个编辑器可能将制表符解释为三个空格，而另一个可能解释为四个。

使用制表符缩进可能导致代码格式不统一，从而引发格式错误。为了避免此类错误，你应该在缩进代码时使用一致数量的空格。

为了统一性，指南建议在代码的每个缩进层级使用**四个空格**。四个空格足以保证良好的可读性。

```python
# 正确示例：使用四个空格缩进
if condition:
    statement1
    statement2
```

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_21.png)

请注意，示例中的四个点是为了描绘四个空格而添加的。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_22.png)

### 使用空行分隔代码块

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_24.png)

PEP 8还建议使用空行来分隔代码中的函数和类。

空行有助于界定代码不同部分的开始和结束。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_26.png)

```python
# 不遵循PEP 8的示例：函数和类之间没有空行
def function1():
    pass
class UserClass:
    pass

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_28.png)

# 遵循PEP 8的示例：函数和类之间有空行
def function1():
    pass

class UserClass:
    pass
```

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_30.png)

### 在运算符和逗号周围使用空格

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_31.png)

为了提高代码可读性，应在运算符周围和逗号后使用空格。使用空格会使命令看起来更宽松、更清晰，从而提升命令的可读性。

以下是具体示例：

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_33.png)

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_34.png)

*   当你写 `A=B+C` 而没有任何空格时，可能会令人困惑。
*   然而，当你添加空格后，例如 `A = B + C`，可读性就提高了。

---

## 🔧 保持代码一致性与可管理性的编码惯例

上一节我们学习了提升可读性的格式指南，本节中我们来看看一些保持代码一致性和可管理性的编码惯例。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_36.png)

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_37.png)

### 将大块代码封装在函数中

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_38.png)

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_39.png)

一个关键的编码惯例是为包含较大代码块的功能创建独立的函数，然后从主程序中调用这些函数。

例如，在以下代码中，`if-else` 语法没有封装成函数，每次需要该功能时都必须重写。然而，如果你将功能写成函数 `function_1`，它就可以被轻松调用。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_41.png)

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_42.png)

```python
# 未使用函数，代码重复
if a > b:
    c = a
else:
    c = b

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_44.png)

# 使用函数，提高复用性
def get_max_value(x, y):
    if x > y:
        return x
    else:
        return y

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_45.png)

c = get_max_value(a, b)  # 调用函数
```

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_47.png)

这种方式提高了代码的执行速度，并以更方便的方式支持代码块的重用。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_49.png)

### 函数和文件的命名：小写字母加下划线

另一个编码惯例是使用小写字母和下划线来命名函数和文件。

Python本身使用这种命名约定，并且许多内置库和预定义函数都遵循这一通用命名惯例。因此，使用小写函数名（最好带下划线）是明智的，这能使函数名更具独特性。

例如：
*   不要写成：`CompSurfaceRadiation()`
*   应该写成：`comp_surface_radiation()`

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_51.png)

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_52.png)

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_53.png)

此规则的一个例外是Python包的命名，通常不鼓励使用下划线。例如，应写为 `mypackage` 而不是 `my_package`。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_54.png)

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_55.png)

### 类的命名：使用驼峰命名法

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_57.png)

使用驼峰命名法命名类也是一个编码惯例。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_59.png)

驼峰命名法（或大驼峰命名法）是编码社区广泛接受的类命名约定。它也有助于在代码中区分类和函数。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_61.png)

例如：
*   不要写成：`class la_sirrel_c`
*   最佳实践是：`class LaSirrelC`（L、S和C大写）

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_63.png)

### 常量的命名：全大写字母加下划线

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_64.png)

使用全大写字母并用下划线分隔单词的命名惯例，以保持一致性。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_66.png)

名称通常表明常量的用途，例如：`MAX_FILE_UPLOAD_SIZE`。

---

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_68.png)

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_69.png)

## 🔍 静态代码分析

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_70.png)

我们讨论了编码惯例和指南，软件开发人员通常使用静态代码分析来管理对这些样式指南的遵从性。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_72.png)

静态代码分析是一种在不执行代码的情况下，根据预定义的样式和标准来评估代码的方法。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_74.png)

静态分析有助于发现诸如编程错误、违反编码标准、未定义的值、语法违规和安全漏洞等问题。

你可以使用 **Pylint** 等库来检查你的Python代码是否符合PEP 8指南。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_76.png)

---

## 📝 总结

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_78.png)

本节课中我们一起学习了编写一致代码的重要性，它有助于所有团队成员轻松阅读和理解代码。

**PEP 8提升代码可读性的指南包括：**
*   使用四个空格进行缩进。
*   使用空行分隔函数和类。
*   在运算符周围和逗号后使用空格。

**保持代码一致性和可管理性的编码惯例包括：**
*   将大块代码封装在函数内部。
*   使用小写字母加下划线命名函数和文件。
*   使用驼峰命名法命名类。
*   使用全大写字母加下划线命名常量。

![](img/3229a2ab7bdbc020d7a161a3392cb8bf_80.png)

最后，我们了解了可以使用**静态代码分析方法**，在不执行代码的情况下，根据预定义的样式和标准来评估你的代码。