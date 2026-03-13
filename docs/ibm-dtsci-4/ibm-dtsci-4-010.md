# 010：异常处理 🛡️

![](img/449f802bb9671e6cfe8ac9782ecaa704_0.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_1.png)

在本节课中，我们将要学习Python中的异常处理。异常处理是编写健壮程序的关键技术，它允许程序在遇到错误时优雅地恢复，而不是直接崩溃。我们将解释异常处理的概念，演示其使用方法，并理解其基本原理。

![](img/449f802bb9671e6cfe8ac9782ecaa704_3.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_4.png)

## 什么是异常处理？ 🤔

你是否曾经在应该输入文本的输入框中误输入了数字？大多数人都有过这样的经历，无论是出于错误还是测试程序。但你是否知道为什么程序会显示错误信息，而不是完成并终止程序？为了让错误信息出现，后台触发了一个事件。这个事件被激活是因为程序试图对输入的姓名进行计算，并发现输入包含数字而非字母。通过将这段代码包裹在异常处理器中，程序知道如何处理这类错误，并能输出错误信息以继续执行程序。这只是请求用户输入时可能发生的众多错误之一。

上一节我们介绍了异常处理的基本概念，本节中我们来看看异常处理是如何工作的。

## Try-Except语句 🧪

我们将首先探索`try-except`语句。这种类型的语句会首先尝试执行`try`块中的代码。但如果发生错误，它会跳出并开始搜索匹配该错误的异常。一旦找到正确的异常来处理错误，它就会执行那行代码。

![](img/449f802bb9671e6cfe8ac9782ecaa704_6.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_7.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_8.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_9.png)

以下是`try-except`语句的基本结构：

![](img/449f802bb9671e6cfe8ac9782ecaa704_10.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_11.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_12.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_13.png)

```python
try:
    # 尝试执行的代码
    result = 10 / 0
except ZeroDivisionError:
    # 处理特定异常
    print("不能除以零！")
```

![](img/449f802bb9671e6cfe8ac9782ecaa704_14.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_15.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_16.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_17.png)

## 处理特定异常 🎯

例如，假设你正在编写一个打开并写入文件的程序。程序启动后，由于数据无法读取而发生了错误。因为这个错误，程序跳过了`try`语句下的代码行，直接转到`except`行。由于这个错误属于IO错误的范畴，它向控制台打印了“无法打开或读取文件中的数据”。

编写简单程序时，有时我们只用一个`except`语句就能应付。但如果发生了未被IO错误捕获的另一个错误怎么办？如果发生这种情况，我们需要为这个`except`语句添加另一个。

以下是处理多个异常的例子：

```python
try:
    file = open('myfile.txt', 'r')
    content = file.read()
    number = int(content)
except FileNotFoundError:
    print("文件未找到。")
except ValueError:
    print("文件内容无法转换为整数。")
```

![](img/449f802bb9671e6cfe8ac9782ecaa704_19.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_20.png)

## 避免捕获所有异常 ⚠️

![](img/449f802bb9671e6cfe8ac9782ecaa704_21.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_22.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_23.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_24.png)

对于这个`except`语句，你会注意到没有指定要捕获的错误类型。虽然这看起来是一个合理的步骤，让程序捕获所有错误而不终止，但这并不是最佳实践。

![](img/449f802bb9671e6cfe8ac9782ecaa704_25.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_26.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_27.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_28.png)

例如，也许我们的小程序只是一个超过一千行代码的大型程序的一部分。我们的任务是调试程序，因为它不断抛出错误，给用户造成干扰。在调查程序时，你发现这个错误不断出现。因为这个错误没有详细信息，你最终花费了数小时试图定位和修复错误。

![](img/449f802bb9671e6cfe8ac9782ecaa704_29.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_30.png)

## 使用Else和Finally语句 ✅

到目前为止，在我们的程序中，我们已经定义了如果发生错误应该打印出错误信息，但我们没有收到任何程序执行成功的通知。这就是我们可以添加`else`语句来给我们那个通知的地方。

通过添加这个`else`语句，它将向控制台提供一个通知，表明文件写入成功。

```python
try:
    file = open('data.txt', 'w')
    file.write("Hello, World!")
except IOError:
    print("写入文件时发生错误。")
else:
    print("文件写入成功！")
```

现在我们已经定义了如果程序正确执行或发生错误时会发生什么，对于这个例子，还有最后一个语句要添加。

![](img/449f802bb9671e6cfe8ac9782ecaa704_32.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_33.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_34.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_35.png)

## 清理资源：Finally语句 🧹

![](img/449f802bb9671e6cfe8ac9782ecaa704_36.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_37.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_38.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_39.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_40.png)

我们正在打开一个文件，我们需要做的最后一件事是关闭文件。通过添加一个`finally`语句，它将告诉程序无论最终结果如何都要关闭文件，并向控制台打印“文件现已关闭”。

![](img/449f802bb9671e6cfe8ac9782ecaa704_41.png)

```python
try:
    file = open('data.txt', 'w')
    file.write("Hello, World!")
except IOError:
    print("写入文件时发生错误。")
else:
    print("文件写入成功！")
finally:
    file.close()
    print("文件现已关闭。")
```

## 总结 📝

![](img/449f802bb9671e6cfe8ac9782ecaa704_43.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_44.png)

![](img/449f802bb9671e6cfe8ac9782ecaa704_45.png)

本节课中我们一起学习了如何编写`try-except`语句，为什么在创建异常时始终定义错误很重要，以及如何添加`else`和`finally`语句。异常处理是确保程序稳定性和用户体验的重要组成部分。通过合理使用异常处理，我们可以使程序更加健壮和可靠。