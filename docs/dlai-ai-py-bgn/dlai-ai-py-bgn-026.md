# 026：将代码块转化为可重用函数 🧩

在本节课中，我们将要学习Python中一个非常重要的概念：**函数**。在前面的课程中，你可能已经注意到，我们经常需要重复执行某些操作，比如打开、读取和关闭文件，或者定义变量并生成提示词。虽然`for`循环可以帮助我们连续多次执行相同操作，但它要求操作必须一个接一个地连续进行。而函数则提供了更灵活的方式来创建可重用的代码块，让你可以在程序的任何地方随时调用它们。

## 什么是函数？ 🤔

函数是一组可重复使用的命令，用于帮助你完成特定的任务。你已经使用过许多内置函数，例如：
*   `print()` 函数用于输出信息。
*   `len()` 函数用于返回对象的长度。

函数是现代编程语言的核心概念。在本节中，你将学习如何创建自己的函数，并了解它们如何帮助你避免重复编写代码。

## 为什么需要自定义函数？ 💡

让我们通过一个例子来理解。之前，我们写过这样的代码块来打开、读取并打印文件内容：

```python
f = open("Tokyo.txt")
journal = f.read()
f.close()
print(journal)
```

如果我想对另一个城市（如巴黎）的文件做同样的事情，就必须重复这些代码，并将文件名改为`Paris.txt`。为了避免这种重复，我们可以定义一个函数。

## 如何定义一个函数？ 🛠️

我们可以定义一个名为 `print_journal` 的函数，如下所示：

```python
def print_journal(file):
    f = open(file)
    journal = f.read()
    f.close()
    print(journal)
```

现在，你可以通过调用这个函数来加载和打印不同城市的文件：

```python
print_journal("Sydney.txt")
print_journal("Paris.txt")
```

![](img/f41419f36defbe1559688350c0d3f313_1.png)

让我们详细解析这段代码：
*   `def` 是一个特殊的关键字，用于告诉Python你要定义一个新函数。
*   `print_journal` 是函数的名称。
*   括号内的 `file` 是函数的**参数**。这意味着当你调用这个函数时，需要为它提供一个文件名。
*   冒号 `:` 后面是一个缩进的代码块。每次调用 `print_journal` 函数时，都会执行这四行代码。

![](img/f41419f36defbe1559688350c0d3f313_3.png)

## 函数的返回值 📤

上面的函数直接打印了内容。但有时，我们可能希望函数加载文件内容后，将其返回给我们，以便进行其他操作（比如计算长度），而不是直接打印。

我们可以定义另一个函数 `read_journal`：

```python
def read_journal(file):
    f = open(file)
    journal = f.read()
    f.close()
    return journal
```

这个函数与 `print_journal` 的主要区别在于：
1.  它不再使用 `print(journal)`。
2.  它使用了 `return journal` 语句。`return` 命令的作用是让函数将 `journal` 变量的值“返回”给调用者。

现在，我们可以这样使用它：

```python
journal_tokyo = read_journal("Tokyo.txt")
print(journal_tokyo)
print(len(journal_tokyo))
```

`journal_tokyo` 变量将保存函数返回的文章内容，然后我们可以打印它或计算其长度。

## 另一个例子：温度转换 🌡️

![](img/f41419f36defbe1559688350c0d3f313_5.png)

![](img/f41419f36defbe1559688350c0d3f313_6.png)

![](img/f41419f36defbe1559688350c0d3f313_7.png)

让我们看一个更通用的例子：将华氏度转换为摄氏度。没有函数时，每次转换都需要写一遍公式：

```python
fahrenheit = 72
celsius = (fahrenheit - 32) * 5 / 9
print(celsius)
```

如果要转换多个温度，代码会变得重复。使用函数可以简化这个过程。

首先，定义一个打印结果的函数：

```python
def fahrenheit_to_celsius(fahrenheit):
    celsius = (fahrenheit - 32) * 5 / 9
    print(celsius)

![](img/f41419f36defbe1559688350c0d3f313_9.png)

![](img/f41419f36defbe1559688350c0d3f313_10.png)

![](img/f41419f36defbe1559688350c0d3f313_11.png)

![](img/f41419f36defbe1559688350c0d3f313_12.png)

fahrenheit_to_celsius(72)
fahrenheit_to_celsius(68)
```

其次，定义一个返回结果的函数，以便将转换后的值保存下来进行后续操作：

```python
def fahrenheit_to_celsius(fahrenheit):
    celsius = (fahrenheit - 32) * 5 / 9
    return celsius

![](img/f41419f36defbe1559688350c0d3f313_14.png)

temp_f = 45
temp_c = fahrenheit_to_celsius(temp_f)
print(f"温度是 {temp_c} 摄氏度")
print(type(temp_c))
```

## 动手练习 ✍️

![](img/f41419f36defbe1559688350c0d3f313_16.png)

现在，你已经了解了函数的基本用法。可以尝试修改上面的温度转换代码：
*   创建一个将摄氏度转换为华氏度的函数。
*   创建一个将英尺转换为米的函数。

![](img/f41419f36defbe1559688350c0d3f313_18.png)

## 总结 📝

本节课中我们一起学习了Python函数的核心概念：
*   **函数** 是通过 `def` 关键字定义的可重用代码块。
*   **参数** 是传递给函数的数据，在函数名后的括号内指定。
*   **返回值** 使用 `return` 语句将函数内部计算的结果传递出去，这样调用者就可以使用这个值。
*   使用函数可以**避免代码重复**，让程序更简洁、更易维护。

![](img/f41419f36defbe1559688350c0d3f313_20.png)

函数是编程的基石，掌握它将极大地提升你编写高效、清晰代码的能力。在下一节课中，我们将综合运用目前所学的所有知识，来规划一次完美的环球旅行。