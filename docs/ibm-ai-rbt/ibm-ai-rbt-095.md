# 095：单元测试 🧪

![](img/f26e00fe22eaa7567f06dcf816c0547a_0.png)

在本节课中，我们将学习单元测试的概念、流程以及如何构建和执行单元测试。单元测试是验证代码单元是否按预期工作的重要方法，有助于确保代码质量。

![](img/f26e00fe22eaa7567f06dcf816c0547a_2.png)

![](img/f26e00fe22eaa7567f06dcf816c0547a_3.png)

![](img/f26e00fe22eaa7567f06dcf816c0547a_4.png)

## 概述

单元测试是一种验证代码单元是否按设计运行的方法。一个“单元”是应用程序中较小且可测试的部分。通过单元测试，开发者可以在代码开发早期发现并修复问题。

## 什么是单元测试？

![](img/f26e00fe22eaa7567f06dcf816c0547a_6.png)

单元测试用于验证代码单元是否按设计运行。一个“单元”是应用程序中较小且可测试的部分。

以下是一个单元示例，它包含两个函数：`square` 和 `doubler`，位于 `my_module.py` 文件中。

![](img/f26e00fe22eaa7567f06dcf816c0547a_8.png)

`square` 函数的代码为：
```python
def square(number):
    return number ** 2
```

![](img/f26e00fe22eaa7567f06dcf816c0547a_10.png)

`doubler` 函数的代码为：
```python
def doubler(number):
    return number * 2
```

## 单元测试流程

![](img/f26e00fe22eaa7567f06dcf816c0547a_12.png)

![](img/f26e00fe22eaa7567f06dcf816c0547a_13.png)

为了开发单元测试，你将使用 `unittest` 库。这是一个已安装的 Python 模块，提供了一个包含测试功能的框架。

让我们简要回顾从单元测试到发布到生产代码库的端到端测试流程。

![](img/f26e00fe22eaa7567f06dcf816c0547a_15.png)

![](img/f26e00fe22eaa7567f06dcf816c0547a_17.png)

在代码开发过程中，你将测试每个单元。测试分为两个阶段进行。

![](img/f26e00fe22eaa7567f06dcf816c0547a_19.png)

### 第一阶段：本地系统测试

![](img/f26e00fe22eaa7567f06dcf816c0547a_21.png)

在第一阶段，你将在本地系统上测试单元。如果测试失败，你需要确定失败原因并修复问题。然后，你将再次测试该单元。

![](img/f26e00fe22eaa7567f06dcf816c0547a_23.png)

### 第二阶段：服务器环境测试

![](img/f26e00fe22eaa7567f06dcf816c0547a_25.png)

单元测试通过后，你需要在服务器环境中测试该单元，例如持续集成/持续交付（CI/CD）测试服务器。

![](img/f26e00fe22eaa7567f06dcf816c0547a_27.png)

如果单元未通过服务器测试，你将收到失败详情。

![](img/f26e00fe22eaa7567f06dcf816c0547a_29.png)

你需要确定并修复问题。

![](img/f26e00fe22eaa7567f06dcf816c0547a_31.png)

一旦单元通过服务器测试，该单元将被集成到最终代码库中。

![](img/f26e00fe22eaa7567f06dcf816c0547a_33.png)

## 如何构建单元测试

![](img/f26e00fe22eaa7567f06dcf816c0547a_34.png)

在概述了单元测试流程后，让我们回顾一些测试函数，以了解如何构建单元测试。

注意单元和单元测试的代码，观察单元文件名为 `my_module.py`。

![](img/f26e00fe22eaa7567f06dcf816c0547a_36.png)

![](img/f26e00fe22eaa7567f06dcf816c0547a_37.png)

![](img/f26e00fe22eaa7567f06dcf816c0547a_38.png)

单元测试文件在名称前或后附加了“test”。这是一个良好的命名约定，因为它有助于清晰区分单元文件和单元测试文件。

![](img/f26e00fe22eaa7567f06dcf816c0547a_39.png)

让我们看看创建单元测试文件的步骤。

![](img/f26e00fe22eaa7567f06dcf816c0547a_41.png)

### 步骤一：导入 unittest 库

第一步是导入 `unittest` Python 库，输入：
```python
import unittest
```

### 步骤二：导入要测试的函数

接下来，导入要测试的函数。例如，要从 `my_module` 单元导入 `square` 和 `doubler` 函数进行单元测试，输入：
```python
from my_module import square, doubler
```

![](img/f26e00fe22eaa7567f06dcf816c0547a_43.png)

![](img/f26e00fe22eaa7567f06dcf816c0547a_44.png)

### 步骤三：构建单元测试类

然后，构建单元测试类，以便从单个类对象调用单元测试。例如，创建一个名为 `TestMyModule` 的类，输入：
```python
class TestMyModule(unittest.TestCase):
```

![](img/f26e00fe22eaa7567f06dcf816c0547a_46.png)

注意类名如何以“Test”为前缀，后跟单元名称。在示例中，这是一个良好的命名约定，将“test”作为类名前缀，有助于区分单元类和单元测试类。

![](img/f26e00fe22eaa7567f06dcf816c0547a_48.png)

接下来，让该类继承 `unittest` 库的 `TestCase` 类。例如，`TestCase` 是 `unittest` 库的测试用例类。继承该类允许你利用 `TestCase` 类中的现有方法。

![](img/f26e00fe22eaa7567f06dcf816c0547a_50.png)

![](img/f26e00fe22eaa7567f06dcf816c0547a_51.png)

### 步骤四：创建测试函数

![](img/f26e00fe22eaa7567f06dcf816c0547a_53.png)

然后在单元测试类中创建与每个需要测试的函数相对应的函数。例如，在 `TestMyModule` 类中，两个函数 `test_square` 和 `test_doubler` 对应于 `my_module` 单元中的 `square` 和 `doubler` 函数。

注意，请确保在单元测试模块中，函数名以“test”开头，因为只有以“test”开头的函数才会运行。

![](img/f26e00fe22eaa7567f06dcf816c0547a_55.png)

### 步骤五：创建测试用例

![](img/f26e00fe22eaa7567f06dcf816c0547a_57.png)

最后，你可以创建测试用例。

![](img/f26e00fe22eaa7567f06dcf816c0547a_59.png)

创建测试用例时，添加一个或多个断言方法，以确保满足单元测试条件。

![](img/f26e00fe22eaa7567f06dcf816c0547a_61.png)

一个断言函数是 `assertEqual`。注意，在代码中，该方法已添加到 `TestCase` 类中。

![](img/f26e00fe22eaa7567f06dcf816c0547a_63.png)

`assertEqual` 函数比较两个值或实体，并判断它们是否相等。

![](img/f26e00fe22eaa7567f06dcf816c0547a_65.png)

该方法用于检查函数是否返回正确的值。

![](img/f26e00fe22eaa7567f06dcf816c0547a_66.png)

`assertEqual` 函数接受的参数之一是实际值。对于实际值，你将调用要测试的函数。

![](img/f26e00fe22eaa7567f06dcf816c0547a_68.png)

![](img/f26e00fe22eaa7567f06dcf816c0547a_70.png)

第二个参数是期望值，你将在其中添加函数预期返回的内容。

在示例中，第一个测试是针对 `square` 函数，使用数字 2。如果函数正确执行，应返回值 4。

![](img/f26e00fe22eaa7567f06dcf816c0547a_72.png)

作为测试的一部分，首先评估函数。然后比较两个值，看它们是否相等。

根据比较的输出，测试通过或失败。

## 执行与输出

运行测试文件后，会生成输出。输出显示测试结果以及一些额外细节。例如，如果输出显示在 0 秒内运行了两个测试，“OK”表示测试通过，两个函数已正确实现。

![](img/f26e00fe22eaa7567f06dcf816c0547a_74.png)

但如果函数未正确实现会发生什么？

![](img/f26e00fe22eaa7567f06dcf816c0547a_76.png)

考虑 `square` 函数，你编写了计算数字立方而非平方的代码。函数失败，并生成输出。

让我们回顾一个失败单元测试的示例输出。输出清晰地显示单元测试失败。例如，输出显示“FAILED test_square (__main__.TestMyModule)”。你还可以查看单元测试失败的函数。例如，“test_square self.assertEqual(square(2), 4)” 表明 `square` 函数失败。“AssertionError: 8 != 4” 表示值不匹配。详细的输出使你能在实际部署解决方案之前纠正错误。

## 总结

在本节课中，我们一起学习了：
*   单元测试是一种验证代码单元是否按设计运行的方法。
*   在代码开发过程中，每个单元都会被测试。单元测试分为两个阶段进行。
*   一旦单元通过服务器测试，它就会被合并到最终代码库中。
*   确保测试文件在名称前或后附加“test”，以清晰区分它们与模块文件。
*   你可以使用不同的测试函数来构建单元测试。
*   `assertEqual` 函数是一种常用的断言方法，用于比较两个值。
*   你可以查看单元测试输出，并确定测试是通过还是失败。