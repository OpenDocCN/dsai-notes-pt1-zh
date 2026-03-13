# 007：条件判断与分支 🧠

在本节课中，我们将要学习Python中的**条件判断**与**分支**。这是编程中控制程序流程的基础，允许程序根据不同的情况执行不同的代码块。

## 概述 📋

条件判断通过比较操作来评估表达式，并根据结果是真（`True`）还是假（`False`）来决定程序的执行路径。我们将学习比较运算符、`if`、`else`和`elif`语句，以及逻辑运算符。

---

![](img/5a7969a1bfc03149ac719eb401f9ab8c_1.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_2.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_3.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_4.png)

## 比较运算符 🔍

![](img/5a7969a1bfc03149ac719eb401f9ab8c_5.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_6.png)

比较运算符用于比较两个值，并返回一个布尔值（`True`或`False`）。

以下是Python中常用的比较运算符：

*   **等于**：`==`
*   **不等于**：`!=`
*   **大于**：`>`
*   **小于**：`<`
*   **大于等于**：`>=`
*   **小于等于**：`<=`

### 等于运算符（`==`）

等于运算符（`==`）检查两个值是否相等。

![](img/5a7969a1bfc03149ac719eb401f9ab8c_8.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_9.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_10.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_11.png)

```python
a = 6
print(a == 7)  # 输出：False
print(a == 6)  # 输出：True
```

![](img/5a7969a1bfc03149ac719eb401f9ab8c_12.png)

### 大于运算符（`>`）

大于运算符（`>`）检查左侧操作数的值是否大于右侧操作数的值。

```python
i = 6
print(i > 5)  # 输出：True
```

如果我们将`i`的值设为2，结果将是`False`。

![](img/5a7969a1bfc03149ac719eb401f9ab8c_14.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_15.png)

### 大于等于运算符（`>=`）

![](img/5a7969a1bfc03149ac719eb401f9ab8c_16.png)

大于等于运算符（`>=`）检查左侧操作数的值是否大于或等于右侧操作数的值。

```python
i = 5
print(i >= 5)  # 输出：True
```

![](img/5a7969a1bfc03149ac719eb401f9ab8c_18.png)

### 小于运算符（`<`）

![](img/5a7969a1bfc03149ac719eb401f9ab8c_19.png)

小于运算符（`<`）检查左侧操作数的值是否小于右侧操作数的值。

```python
i = 2
print(i < 6)  # 输出：True
```

### 不等于运算符（`!=`）

不等于运算符（`!=`）检查两个操作数是否不相等。

```python
i = 2
print(i != 6)  # 输出：True
```

![](img/5a7969a1bfc03149ac719eb401f9ab8c_21.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_22.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_23.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_24.png)

### 字符串比较

![](img/5a7969a1bfc03149ac719eb401f9ab8c_25.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_26.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_27.png)

比较运算符也可以用于比较字符串。

```python
print(“ACDC” == “Michael Jackson”)  # 输出：False
print(“ACDC” != “Michael Jackson”)  # 输出：True
```

---

## 分支语句：`if` 🚪

上一节我们介绍了如何使用比较运算符得到布尔值。本节中我们来看看如何利用这些布尔值来控制程序流程，这称为**分支**。

![](img/5a7969a1bfc03149ac719eb401f9ab8c_29.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_30.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_31.png)

`if`语句就像一个上锁的房间。只有当条件为`True`时，程序才能“进入房间”执行预定义的任务。如果条件为`False`，程序会跳过这个任务。

![](img/5a7969a1bfc03149ac719eb401f9ab8c_32.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_33.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_34.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_35.png)

**语法**：
```python
if (条件表达式):
    # 如果条件为True，则执行此缩进块内的代码
```

![](img/5a7969a1bfc03149ac719eb401f9ab8c_36.png)

**示例**：模拟音乐会年龄检查
```python
age = 19
if (age >= 18):
    print(“你可以入场。”)
print(“继续流程。”)
```
当`age`为19时，条件为`True`，程序会打印“你可以入场。”，然后打印“继续流程。”。
当`age`为17时，条件为`False`，程序只会打印“继续流程。”。

---

## 分支语句：`else` 🔄

`else`语句与`if`语句配合使用，当`if`的条件为`False`时，程序会执行`else`块中的代码。

![](img/5a7969a1bfc03149ac719eb401f9ab8c_38.png)

**语法**：
```python
if (条件表达式):
    # 条件为True时执行
else:
    # 条件为False时执行
```

![](img/5a7969a1bfc03149ac719eb401f9ab8c_39.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_40.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_41.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_42.png)

**示例**：为不同年龄提供不同音乐会选项
```python
age = 17
if (age >= 18):
    print(“你可以进入ADC音乐会。”)
else:
    print(“你可以去 Meatloaf 音乐会。”)
print(“继续流程。”)
```
当`age`为17时，`if`条件为`False`，程序执行`else`块，打印“你可以去 Meatloaf 音乐会。”。
当`age`为19时，`if`条件为`True`，程序执行`if`块，打印“你可以进入ADC音乐会。”，并跳过`else`块。

![](img/5a7969a1bfc03149ac719eb401f9ab8c_43.png)

---

## 分支语句：`elif`（else if）🎯

`elif`语句（`else if`的缩写）允许我们在前面的条件为`False`时，检查额外的条件。

**语法**：
```python
if (条件1):
    # 条件1为True时执行
elif (条件2):
    # 条件1为False 且 条件2为True时执行
else:
    # 以上条件都为False时执行
```

![](img/5a7969a1bfc03149ac719eb401f9ab8c_45.png)

**示例**：为18岁观众提供特别选项
```python
age = 18
if (age > 18):
    print(“你可以进入ADC音乐会。”)
elif (age == 18):
    print(“你可以去看 Pink Floyd 音乐会。”)
else:
    print(“你可以去 Meatloaf 音乐会。”)
print(“继续流程。”)
```
当`age`为18时，第一个条件（`age > 18`）为`False`，程序检查`elif`条件（`age == 18`）为`True`，因此打印“你可以去看 Pink Floyd 音乐会。”。

![](img/5a7969a1bfc03149ac719eb401f9ab8c_46.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_47.png)

---

## 逻辑运算符 🧩

逻辑运算符用于组合多个布尔表达式，形成更复杂的条件。

### 逻辑非（`not`）

`not`运算符对布尔值取反。如果输入是`True`，结果为`False`；如果输入是`False`，结果为`True`。

![](img/5a7969a1bfc03149ac719eb401f9ab8c_49.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_50.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_51.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_52.png)

```python
print(not(True))   # 输出：False
print(not(False))  # 输出：True
```

![](img/5a7969a1bfc03149ac719eb401f9ab8c_53.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_54.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_55.png)

### 逻辑或（`or`）

![](img/5a7969a1bfc03149ac719eb401f9ab8c_57.png)

`or`运算符接受两个布尔值，只要其中**至少有一个**为`True`，结果就为`True`；只有两者都为`False`时，结果才为`False`。

| A     | B     | A `or` B |
| :---- | :---- | :------- |
| True  | True  | True     |
| True  | False | True     |
| False | True  | True     |
| False | False | False    |

**示例**：检查专辑年份是否为70年代或90年代
```python
album_year = 1990
if (album_year < 1980) or (album_year > 1989):
    print(“这张专辑制作于70年代或90年代。”)
else:
    print(“这张专辑制作于80年代。”)
```
对于`album_year = 1990`，第二个条件（`1990 > 1989`）为`True`，因此整个`or`表达式为`True`，会执行`if`块内的打印语句。

### 逻辑与（`and`）

`and`运算符接受两个布尔值，只有**两者都为**`True`时，结果才为`True`；否则结果为`False`。

![](img/5a7969a1bfc03149ac719eb401f9ab8c_59.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_60.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_61.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_62.png)

| A     | B     | A `and` B |
| :---- | :---- | :-------- |
| True  | True  | True      |
| True  | False | False     |
| False | True  | False     |
| False | False | False     |

![](img/5a7969a1bfc03149ac719eb401f9ab8c_63.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_64.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_65.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_66.png)

**示例**：精确检查专辑年份是否为80年代
```python
album_year = 1983
if (album_year > 1979) and (album_year < 1990):
    print(“这张专辑制作于80年代。”)
```
对于`album_year = 1983`，两个条件（`1983 > 1979` 和 `1983 < 1990`）都为`True`，因此整个`and`表达式为`True`，会执行`if`块内的打印语句。

![](img/5a7969a1bfc03149ac719eb401f9ab8c_67.png)

---

## 总结 🎓

本节课中我们一起学习了Python中控制程序流程的核心概念：
1.  **比较运算符**（`==`, `!=`, `>`, `<`, `>=`, `<=`）用于评估条件并产生布尔值。
2.  **`if`语句**用于在条件为`True`时执行特定代码块。
3.  **`else`语句**与`if`配对，在`if`条件为`False`时提供替代执行路径。
4.  **`elif`语句**允许在`if`条件不满足时，顺序检查多个条件。
5.  **逻辑运算符**（`not`, `and`, `or`）用于组合多个布尔条件，构建更复杂的逻辑判断。

![](img/5a7969a1bfc03149ac719eb401f9ab8c_69.png)

![](img/5a7969a1bfc03149ac719eb401f9ab8c_70.png)

掌握条件判断和分支是编写智能、动态程序的基础，它让你的代码能够根据不同的输入和数据做出决策。