# 011：函数与数据操作

在本节课中，我们将学习Python中一个核心概念——函数。函数就像可以嵌入到更大程序中的小型计算机程序，用于执行特定任务。它们允许你对数据采取行动或与世界互动。让我们深入了解其含义。

## 函数是什么？

在上一节中，我们接触了`print_om_response`函数。函数允许你对数据执行计算或操作。

![](img/6d5471201dcecc01b0a2ce0a617989de_1.png)

你已经在本课程中遇到并使用过几个不同的Python函数。

第一个是`print`函数，你可以用它来向屏幕显示文本。

```python
print("Hello World")
```

![](img/6d5471201dcecc01b0a2ce0a617989de_3.png)

在之前的视频中，你看到了一个更复杂的函数`print_om_response`。这个函数接收一个字符串作为输入（例如一个提问），将其发送到互联网上的大型语言模型，然后将AI的响应显示在屏幕上。

![](img/6d5471201dcecc01b0a2ce0a617989de_5.png)

你还见过另一个函数`type`，它可以告诉你一个值或变量的数据类型，例如字符串、浮点数或整数。

```python
type(42.17)
```

## 函数的运作机制

本节中，我们将更深入地探讨函数的工作原理。

![](img/6d5471201dcecc01b0a2ce0a617989de_7.png)

Python中有一个名为`len`的函数，代表长度。如果你输入`len(“Hello World”)`，它会将字符串“Hello World”作为输入，并计算其长度，即字符串中包括标点和空格在内的字符数量。

```python
len("Hello World")
# 返回 12
```

这是另一个Python函数`round`的例子。它接收一个数字，例如`42.17`，并将其四舍五入到最接近的整数。

![](img/6d5471201dcecc01b0a2ce0a617989de_9.png)

```python
round(42.17)
# 返回 42
```

函数的一个常见用途是对数据执行计算并返回某个值。例如，`len`函数接收数据作为输入，计算字符数并返回答案。`round(42.17)`计算42.17四舍五入后的值并返回42。

有时函数用于执行一个动作，例如显示一段信息。`print(“Hello World”)`接收字符串“Hello World”并将其显示出来。

## 函数的组成部分

![](img/6d5471201dcecc01b0a2ce0a617989de_11.png)

让我们仔细看看当你调用`len(“Hello World”)`时具体发生了什么。

![](img/6d5471201dcecc01b0a2ce0a617989de_13.png)

*   `len`是函数的名称。
*   当你调用一个函数时，使用圆括号`()`向其提供数据。在这个例子中，字符串“Hello World”就是你提供给`len`函数的数据。
*   你提供给函数的数据通常被称为**参数**。
*   当函数返回一个数字（如12）时，我们说函数**返回**了一个结果。

`print`函数也是如此：`print`是函数名，圆括号内是函数的参数，即数据“Hello World”。

![](img/6d5471201dcecc01b0a2ce0a617989de_15.png)

![](img/6d5471201dcecc01b0a2ce0a617989de_16.png)

## 使用函数返回值

在之前的例子中，我们获取函数的结果并立即打印出来。

![](img/6d5471201dcecc01b0a2ce0a617989de_18.png)

```python
print(len("Hello World"))
```

![](img/6d5471201dcecc01b0a2ce0a617989de_20.png)

但你不必立即打印函数的结果。你可以定义一个变量来存储函数的返回值。

```python
string_length = len("Hello World")
print(string_length)
# 打印出 12
```

![](img/6d5471201dcecc01b0a2ce0a617989de_22.png)

在这段代码中，`string_length`是我们创建的新变量的名称。赋值运算符`=`将`len(“Hello World”)`返回的数字12赋给这个变量。这相当于将数字12放入标签为`string_length`的盒子中。之后打印`string_length`，就会输出数字12。

## 一个综合示例

![](img/6d5471201dcecc01b0a2ce0a617989de_24.png)

以下是一个更复杂的例子，展示了如何将变量和函数组合起来进行更复杂的计算。

```python
name = "Tommy"
potatoes = 4.75
prompt = f"Write a couplet about my friend {name} who has about {round(potatoes)} potatoes."
response = get_om_response(prompt)
print(response)
```

这里，`get_om_response`是一个新函数，它的功能与`print_om_response`类似，但它不是直接打印响应，而是调用ChatGPT获取响应，并将其作为一个值返回，然后我们将其存储在变量`response`中。

这段代码将创建提示语“为我的朋友Tommy写一首对句诗，他大约有5个土豆”，然后要求AI写一首对句诗。对于使用生成式AI编写软件的开发者来说，这种编程模式非常常见：拥有一些变量，构建提示语，调用大型语言模型获取响应，然后基于该响应进行其他操作。

![](img/6d5471201dcecc01b0a2ce0a617989de_26.png)

## 课程总结

本节课中，我们一起学习了Python的基础知识，包括数据类型、数学运算、变量、函数的使用，以及如何通过Python代码与大型语言模型交互。

恭喜你完成了这门简短课程！要真正学会并熟练使用Python，关键在于练习。请务必尝试Jupyter笔记本底部的额外练习题。

请记住，任何时候你都可以向聊天机器人寻求帮助或建议，它是解决问题非常有用的伙伴。

完成本课程后，希望你加入下一门课程，学习更强大的Python功能，例如如何让计算机重复执行相同操作，以及如何根据看到的数据让计算机采取不同的行动。

随着不断学习，你将能够使用Python和AI做出越来越多令人惊叹的事情。

再次祝贺你完成本课程，期待在下一门课程中见到你。😊