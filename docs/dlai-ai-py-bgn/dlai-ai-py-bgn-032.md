# 032：安装Python包

在本节课中，我们将学习如何在Python环境中安装外部包。到目前为止，本网站上的所有Jupyter笔记本都已预装了运行所需的所有包。但如果你在自己的计算机或其他环境中使用Jupyter笔记本，可能需要自行安装一些包。本节将演示下载和安装包的过程，特别是用于解析HTML网页的包，以及DeepLearning.AI提供的辅助工具包。

## 安装Beautiful Soup包

有多种方式可以安装Python包。这里我们将介绍如何在Jupyter笔记本中直接安装。本节中，我们需要使用一个名为Beautiful Soup（简称bs4）的包来解析HTML网页。

以下是安装bs4包的方法：在代码单元格中输入 `!pip install bs4` 并运行。感叹号 `!` 允许我们在笔记本中执行系统命令。`pip` 是Python的包管理工具。

```python
!pip install bs4
```

运行此命令后，计算机会在线查找bs4包，然后下载并安装它。安装成功后，你将看到类似“Successfully installed bs4-0.0.2”的提示。

> **注**：bs4代表Beautiful Soup版本4。其名称来源于《爱丽丝梦游仙境》中的一首诗。这是一个用于解析HTML网页的Python包。

![](img/c16acff38a28f3e66666813711a7832b_1.png)

bs4是一个相对较小的包，安装只需几秒钟。有些包可能需要几分钟才能安装完成。

## 导入并使用已安装的包

安装包后，你就可以导入该包或其包含的函数了。

接下来，我们从刚安装的`bs4`包中导入`BeautifulSoup`函数。同时，为了完成今天的示例，我们还需要导入其他几个模块：`requests`用于下载网页，以及一些辅助函数和用于显示HTML的`IPython`组件。

![](img/c16acff38a28f3e66666813711a7832b_3.png)

```python
from bs4 import BeautifulSoup
import requests
from IPython.display import display, HTML
```

## 从网页抓取并解析文本

上一节我们介绍了如何安装和导入包，本节中我们来看看如何使用bs4从网页中提取纯文本。

![](img/c16acff38a28f3e66666813711a7832b_5.png)

首先，你需要获取网页内容。这里有一个URL，指向一份通讯文章。

![](img/c16acff38a28f3e66666813711a7832b_6.png)

```python
url = "https://example.com/the-batch-letter"  # 示例URL
response = requests.get(url)
```

![](img/c16acff38a28f3e66666813711a7832b_8.png)

`requests.get(url)` 会向该URL发送请求并获取响应。然后，我们可以使用BeautifulSoup来解析响应的HTML内容，并提取其中的段落文本。

![](img/c16acff38a28f3e66666813711a7832b_9.png)

![](img/c16acff38a28f3e66666813711a7832b_10.png)

以下是提取网页中所有段落文本并将其合并的代码：

![](img/c16acff38a28f3e66666813711a7832b_12.png)

![](img/c16acff38a28f3e66666813711a7832b_13.png)

```python
# 使用BeautifulSoup解析HTML
soup = BeautifulSoup(response.content, 'html.parser')
# 找到所有的段落（<p>标签）
paragraphs = soup.find_all('p')
# 将所有段落的文本合并成一个字符串
combined_text = ' '.join([para.get_text() for para in paragraphs])
print(combined_text)
```

![](img/c16acff38a28f3e66666813711a7832b_15.png)

运行这段代码后，你将得到网页中主要文章的纯文本内容。这个文本字符串可以直接作为提示词输入给大型语言模型进行处理，例如让它生成摘要或要点。

> **关键点**：你不必完全理解上述代码的每个细节。如果你需要编写类似功能的代码，可以直接向AI助手求助。本节的核心是掌握安装包（`!pip install <package_name>`）和导入使用的基本流程。

![](img/c16acff38a28f3e66666813711a7832b_17.png)

## 安装AI辅助工具包

让我们再看一个安装包的例子。DeepLearning.AI团队提供了一个名为`ai_setup`的包，它包含了本课程及之前课程中使用过的关键辅助函数。

安装`ai_setup`包的方法与之前相同：

![](img/c16acff38a28f3e66666813711a7832b_19.png)

![](img/c16acff38a28f3e66666813711a7832b_20.png)

```python
!pip install ai_setup
```

安装完成后，你就可以导入并使用其中的函数了，例如之前用过的`get_completion`函数。

![](img/c16acff38a28f3e66666813711a7832b_22.png)

```python
from ai_setup import get_completion

prompt = "为什么编程语言叫Python？"
response = get_completion(prompt)
print(response)
```

![](img/c16acff38a28f3e66666813711a7832b_24.png)

运行上述代码，你会得到一个有趣的答案（它实际上灵感来源于一部名为《蒙提·派森的飞行马戏团》的喜剧节目）。

> **注意**：在本网站上，`ai_setup`可以直接使用。若要在你自己的计算机上使用`ai_setup`，后续课程还会介绍一个额外的配置步骤。

`get_completion`函数通过API（应用程序编程接口）访问互联网上的大型语言模型服务（例如OpenAI）来获取答案。API是一种非常强大的工具，它允许你的代码在获得许可的情况下访问他人的计算机或服务来获取功能。

## 总结

本节课中我们一起学习了如何在Python环境中安装外部包。我们通过安装`bs4`包来解析HTML网页，并安装了`ai_setup`包来使用预定义的AI辅助函数。核心操作是使用 `!pip install <package_name>` 命令进行安装，然后通过 `import` 语句将包引入代码中。掌握包的安装和管理是扩展Python功能、利用丰富生态系统的关键一步。

在下一节视频中，我们将深入探讨什么是API以及如何在自己的代码中使用它们，这将帮助你快速构建更强大的程序。