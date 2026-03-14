# Python正则表达式课程 P1：一小时掌握Python正则表达式 🐍

![](img/da8ee5de7a086ab699eec08ed957b1f1_1.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_3.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_5.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_6.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_7.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_8.png)

在本课程中，我们将学习如何在Python中使用正则表达式。正则表达式是一种强大的工具，用于在文本中搜索和匹配特定模式。课程结束时，你将能够理解并编写用于提取电子邮件、日期等信息的正则表达式。

![](img/da8ee5de7a086ab699eec08ed957b1f1_10.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_12.png)

## 导入模块与基础用法

![](img/da8ee5de7a086ab699eec08ed957b1f1_14.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_16.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_18.png)

Python内置了`re`模块来处理正则表达式。首先需要导入该模块。

![](img/da8ee5de7a086ab699eec08ed957b1f1_20.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_21.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_23.png)

```python
import re
```

![](img/da8ee5de7a086ab699eec08ed957b1f1_25.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_27.png)

我们可以创建一个模式对象，并使用它在文本中查找匹配项。建议为模式使用原始字符串（以`r`开头），以避免转义字符带来的混淆。

![](img/da8ee5de7a086ab699eec08ed957b1f1_29.png)

```python
test_string = "123 abc ABC 456 abc"
pattern = re.compile(r'abc')
matches = pattern.finditer(test_string)

![](img/da8ee5de7a086ab699eec08ed957b1f1_31.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_33.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_34.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_35.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_36.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_37.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_39.png)

for match in matches:
    print(match)
```

![](img/da8ee5de7a086ab699eec08ed957b1f1_41.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_43.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_45.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_46.png)

运行上述代码会找到两个匹配对象，分别对应字符串中的`abc`。正则表达式默认是区分大小写的。

![](img/da8ee5de7a086ab699eec08ed957b1f1_48.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_50.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_51.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_53.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_55.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_56.png)

## 搜索匹配项的方法

![](img/da8ee5de7a086ab699eec08ed957b1f1_58.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_60.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_62.png)

`re`模块提供了几种不同的方法来搜索匹配项。

![](img/da8ee5de7a086ab699eec08ed957b1f1_63.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_65.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_66.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_68.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_69.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_71.png)

以下是四种主要方法：
*   `finditer()`: 返回一个包含所有匹配对象的迭代器。
*   `findall()`: 返回一个包含所有匹配字符串的列表。
*   `match()`: 仅在字符串**开头**匹配时返回一个匹配对象。
*   `search()`: 扫描整个字符串，返回**第一个**匹配对象。

![](img/da8ee5de7a086ab699eec08ed957b1f1_73.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_74.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_75.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_77.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_79.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_80.png)

其中，`finditer()`是功能最全面且常用的方法。

![](img/da8ee5de7a086ab699eec08ed957b1f1_82.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_84.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_86.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_87.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_89.png)

## 处理匹配对象

![](img/da8ee5de7a086ab699eec08ed957b1f1_91.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_93.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_94.png)

当我们使用`finditer()`等方法获得匹配对象后，可以从中提取详细信息。

![](img/da8ee5de7a086ab699eec08ed957b1f1_96.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_98.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_100.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_102.png)

以下是匹配对象的常用方法：
*   `.span()`: 返回匹配的（开始， 结束）索引位置元组。
*   `.start()`: 返回匹配的开始索引。
*   `.end()`: 返回匹配的结束索引。
*   `.group()`: 返回匹配的字符串。`group(0)`与之相同，分组后可用`group(1)`, `group(2)`等访问子组。

![](img/da8ee5de7a086ab699eec08ed957b1f1_104.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_105.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_106.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_108.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_109.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_110.png)

```python
for match in matches:
    print(f"匹配内容: {match.group()}, 位置: {match.span()}")
```

![](img/da8ee5de7a086ab699eec08ed957b1f1_112.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_113.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_115.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_117.png)

## 元字符与特殊序列

![](img/da8ee5de7a086ab699eec08ed957b1f1_118.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_120.png)

正则表达式包含一些具有特殊含义的字符，称为元字符。了解它们是编写模式的关键。

![](img/da8ee5de7a086ab699eec08ed957b1f1_122.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_123.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_124.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_126.png)

主要元字符包括：`.` `^` `$` `*` `+` `?` `{}` `[]` `|` `()` `\`

![](img/da8ee5de7a086ab699eec08ed957b1f1_128.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_130.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_132.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_133.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_135.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_136.png)

此外，还有一些以反斜杠`\`开头的特殊序列，用于匹配特定字符类别。

![](img/da8ee5de7a086ab699eec08ed957b1f1_138.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_140.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_142.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_143.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_145.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_146.png)

以下是常用的特殊序列：
*   `\d`: 匹配任何十进制数字，相当于`[0-9]`。
*   `\D`: 匹配任何非数字字符。
*   `\s`: 匹配任何空白字符（空格、制表符、换行符）。
*   `\S`: 匹配任何非空白字符。
*   `\w`: 匹配任何字母数字字符（单词字符），相当于`[a-zA-Z0-9_]`。
*   `\W`: 匹配任何非单词字符。
*   `\b`: 匹配单词的边界。

![](img/da8ee5de7a086ab699eec08ed957b1f1_148.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_149.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_150.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_151.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_153.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_154.png)

例如，`r'\d+'`会匹配一个或多个连续的数字。

![](img/da8ee5de7a086ab699eec08ed957b1f1_156.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_157.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_159.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_161.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_163.png)

## 使用集合与量词

![](img/da8ee5de7a086ab699eec08ed957b1f1_165.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_167.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_168.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_170.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_171.png)

集合用于定义一组我们想要匹配的字符，写在方括号`[]`内。量词则用于指定一个模式出现的次数。

![](img/da8ee5de7a086ab699eec08ed957b1f1_173.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_175.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_176.png)

在集合中，可以使用连字符`-`定义范围，例如`[a-z]`匹配所有小写字母。量词则附加在字符或组后面。

![](img/da8ee5de7a086ab699eec08ed957b1f1_178.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_180.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_181.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_183.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_184.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_185.png)

以下是主要的量词：
*   `*`: 匹配前面的模式0次或多次。
*   `+`: 匹配前面的模式1次或多次。
*   `?`: 匹配前面的模式0次或1次（使其可选）。
*   `{n}`: 匹配前面的模式恰好n次。
*   `{n,m}`: 匹配前面的模式n到m次。

![](img/da8ee5de7a086ab699eec08ed957b1f1_187.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_188.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_190.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_191.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_192.png)

例如，`r'[A-Za-z]+'`会匹配一个或多个连续的字母（不区分大小写）。

![](img/da8ee5de7a086ab699eec08ed957b1f1_194.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_195.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_197.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_198.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_199.png)

## 分组与条件匹配

![](img/da8ee5de7a086ab699eec08ed957b1f1_201.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_202.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_203.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_204.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_205.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_206.png)

分组使用圆括号`()`实现，它有两个主要用途：一是将多个字符视为一个整体以便应用量词，二是提取匹配文本的子部分。条件匹配使用竖线`|`表示“或”的关系。

![](img/da8ee5de7a086ab699eec08ed957b1f1_208.png)

例如，模式`r'(Mr|Mrs|Ms|Miss)\.?\s\w+'`可以匹配“Mr.”、“Mrs”、“Ms. Smith”或“Miss Jones”等称谓加姓氏的格式。圆括号创建了一个组，竖线`|`提供了多种可选前缀。

![](img/da8ee5de7a086ab699eec08ed957b1f1_210.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_211.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_213.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_214.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_216.png)

## 修改字符串：分割与替换

![](img/da8ee5de7a086ab699eec08ed957b1f1_217.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_219.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_220.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_222.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_223.png)

除了查找，`re`模块还可以用来修改字符串，主要通过`split()`和`sub()`方法实现。

![](img/da8ee5de7a086ab699eec08ed957b1f1_224.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_225.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_226.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_227.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_228.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_230.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_231.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_233.png)

`split()`方法在匹配模式的地方分割字符串，返回一个列表。`sub()`方法查找所有匹配的子串，并用指定的字符串替换它们。

![](img/da8ee5de7a086ab699eec08ed957b1f1_234.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_236.png)

```python
# 分割示例
text = "apple,banana,orange,grape"
pattern = re.compile(r',')
result = pattern.split(text)
print(result)  # 输出: ['apple', 'banana', 'orange', 'grape']

![](img/da8ee5de7a086ab699eec08ed957b1f1_237.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_239.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_241.png)

# 替换示例
text = "Hello world! The world is big."
pattern = re.compile(r'world')
new_text = pattern.sub('planet', text)
print(new_text)  # 输出: Hello planet! The planet is big.
```

![](img/da8ee5de7a086ab699eec08ed957b1f1_243.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_245.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_247.png)

在`sub()`方法中，可以使用反向引用（如`\1`, `\2`）来引用匹配组中的内容。

![](img/da8ee5de7a086ab699eec08ed957b1f1_249.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_251.png)

## 编译标志

![](img/da8ee5de7a086ab699eec08ed957b1f1_253.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_254.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_256.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_257.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_259.png)

在编译正则表达式模式时，可以传入标志来修改其行为。常见的标志可以组合使用（如`re.I | re.M`）。

![](img/da8ee5de7a086ab699eec08ed957b1f1_260.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_262.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_264.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_266.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_267.png)

以下是一些有用的编译标志：
*   `re.IGNORECASE` 或 `re.I`: 使匹配对大小写不敏感。
*   `re.MULTILINE` 或 `re.M`: 影响`^`和`$`的行为，使其匹配每行的开头和结尾。
*   `re.DOTALL` 或 `re.S`: 使点`.`匹配任何字符，包括换行符。

![](img/da8ee5de7a086ab699eec08ed957b1f1_268.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_269.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_271.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_273.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_274.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_276.png)

```python
pattern = re.compile(r'hello', re.IGNORECASE)
match = pattern.search('HELLO World')
print(match)  # 会找到匹配，因为忽略了大小写
```

![](img/da8ee5de7a086ab699eec08ed957b1f1_277.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_278.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_279.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_281.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_283.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_285.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_286.png)

## 总结

![](img/da8ee5de7a086ab699eec08ed957b1f1_287.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_288.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_290.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_291.png)

在本节课中，我们一起学习了Python正则表达式的基础知识。我们从导入`re`模块和基础匹配开始，逐步了解了搜索方法、匹配对象、元字符、特殊序列、集合、量词、分组、条件匹配以及如何使用`split()`和`sub()`修改字符串。最后，我们还简要介绍了编译标志的用途。

![](img/da8ee5de7a086ab699eec08ed957b1f1_293.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_294.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_296.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_298.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_299.png)

![](img/da8ee5de7a086ab699eec08ed957b1f1_301.png)

掌握这些核心概念后，你已经能够编写正则表达式来处理许多常见的文本匹配和提取任务。正则表达式是一门需要练习的技能，建议多尝试不同的模式来巩固理解。