# 077：使用 `open()` 函数读取文件 📂

在本节课中，我们将学习如何使用 Python 内置的 `open()` 函数来创建文件对象，并从 TXT 文件中获取数据。

![](img/71a26e369d67235ac363be86e8f5be57_1.png)

![](img/71a26e369d67235ac363be86e8f5be57_2.png)

我们将使用 Python 的 `open()` 函数来获取一个文件对象。然后，我们可以对该对象应用方法来读取文件中的数据。例如，我们可以按如下方式打开文件 `example1.txt`。

![](img/71a26e369d67235ac363be86e8f5be57_3.png)

## 使用 `open()` 函数

我们使用 `open()` 函数。它的第一个参数是文件路径，由文件名和文件目录组成。第二个参数是模式，常用的值包括 `'r'` 表示读取，`'w'` 表示写入，`'a'` 表示追加。在本节中，我们将使用 `'r'` 模式进行读取。最终，我们会得到一个文件对象。

![](img/71a26e369d67235ac363be86e8f5be57_5.png)

```python
file1 = open('example1.txt', 'r')
```

现在，我们可以使用这个文件对象来获取文件的信息。我们可以使用数据属性 `.name` 来获取文件名，结果是一个包含文件名的字符串。我们还可以使用数据属性 `.mode` 来查看对象的模式，这里会显示 `'r'` 表示读取。

你应该始终使用方法 `.close()` 来关闭文件对象。但有时这可能会很繁琐，因此让我们使用 `with` 语句。

## 使用 `with` 语句

![](img/71a26e369d67235ac363be86e8f5be57_7.png)

![](img/71a26e369d67235ac363be86e8f5be57_8.png)

![](img/71a26e369d67235ac363be86e8f5be57_9.png)

使用 `with` 语句打开文件是更好的做法，因为它会自动关闭文件。

![](img/71a26e369d67235ac363be86e8f5be57_10.png)

![](img/71a26e369d67235ac363be86e8f5be57_11.png)

```python
with open('example1.txt', 'r') as file1:
    # 在此缩进块内执行操作
```

这段代码将运行缩进块内的所有内容，然后自动关闭文件。此代码读取文件 `example1.txt`。我们可以在缩进块内使用文件对象 `file1` 执行所有操作，在缩进块结束时，文件会自动关闭。

![](img/71a26e369d67235ac363be86e8f5be57_13.png)

![](img/71a26e369d67235ac363be86e8f5be57_14.png)

## 读取文件内容

方法 `.read()` 将文件的值作为一个字符串存储在变量中。

```python
with open('example1.txt', 'r') as file1:
    file_stuff = file1.read()
    print(file_stuff)
```

![](img/71a26e369d67235ac363be86e8f5be57_16.png)

![](img/71a26e369d67235ac363be86e8f5be57_17.png)

你可以打印文件内容。你可以检查文件是否已关闭，但你不能在缩进块外读取它。不过，你仍然可以在缩进块外打印文件内容。

![](img/71a26e369d67235ac363be86e8f5be57_18.png)

当我们检查原始字符串时，会看到 `\n`。这是 Python 用来表示换行的方式。

![](img/71a26e369d67235ac363be86e8f5be57_20.png)

## 逐行读取文件

![](img/71a26e369d67235ac363be86e8f5be57_22.png)

我们可以使用方法 `.readlines()` 将每一行输出为列表中的一个元素。第一行对应列表的第一个元素，第二行对应第二个元素，依此类推。

![](img/71a26e369d67235ac363be86e8f5be57_23.png)

```python
with open('example1.txt', 'r') as file1:
    file_stuff = file1.readlines()
    print(file_stuff)
```

![](img/71a26e369d67235ac363be86e8f5be57_25.png)

我们可以使用方法 `.readline()` 来读取文件的第一行。如果我们运行此命令，它会将第一行存储在变量 `file_stuff` 中，然后打印第一行。

![](img/71a26e369d67235ac363be86e8f5be57_26.png)

![](img/71a26e369d67235ac363be86e8f5be57_27.png)

```python
with open('example1.txt', 'r') as file1:
    file_stuff = file1.readline()
    print(file_stuff)
```

![](img/71a26e369d67235ac363be86e8f5be57_29.png)

我们可以两次使用 `.readline()` 方法。第一次调用时，它会将第一行保存在变量 `file_stuff` 中，然后打印第一行。第二次调用时，它会将第二行保存在变量 `file_stuff` 中，然后打印第二行。

![](img/71a26e369d67235ac363be86e8f5be57_30.png)

我们可以使用循环来单独打印每一行，如下所示。

![](img/71a26e369d67235ac363be86e8f5be57_32.png)

```python
with open('example1.txt', 'r') as file1:
    for line in file1:
        print(line)
```

![](img/71a26e369d67235ac363be86e8f5be57_33.png)

## 读取指定数量的字符

![](img/71a26e369d67235ac363be86e8f5be57_35.png)

![](img/71a26e369d67235ac363be86e8f5be57_36.png)

我们可以将字符串中的每个字符想象成一个网格。我们可以通过向 `.read()` 方法传递一个参数，来指定要从字符串中读取的字符数量。

![](img/71a26e369d67235ac363be86e8f5be57_38.png)

```python
with open('example1.txt', 'r') as file1:
    print(file1.read(4))
```

![](img/71a26e369d67235ac363be86e8f5be57_39.png)

当我们在 `.read()` 方法中使用参数 `4` 时，会打印出文件中的前四个字符。

每次调用该方法，我们都会在文本中前进。如果我们使用参数 `16` 调用该方法，会打印出前 16 个字符，然后是换行符。如果我们第二次调用该方法，会打印出接下来的五个字符。最后，如果我们最后一次调用该方法，参数为 `9`，则会打印出最后 9 个字符。

请查看实验部分，以获取更多关于方法和其他文件类型的示例。

![](img/71a26e369d67235ac363be86e8f5be57_41.png)

![](img/71a26e369d67235ac363be86e8f5be57_42.png)

---

![](img/71a26e369d67235ac363be86e8f5be57_43.png)

![](img/71a26e369d67235ac363be86e8f5be57_44.png)

![](img/71a26e369d67235ac363be86e8f5be57_45.png)

本节课中，我们一起学习了如何使用 Python 的 `open()` 函数读取文件，包括使用 `with` 语句自动管理文件、读取全部内容、逐行读取以及读取指定数量的字符。这些是处理文件数据的基础操作。