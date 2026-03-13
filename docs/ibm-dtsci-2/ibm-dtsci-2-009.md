# 009：开源数据科学工具（下） 🛠️

![](img/7e6dadb269c711fe0ce3601b3798e7a9_1.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_2.png)

在本节课中，我们将继续学习开源数据科学工具。我们将重点介绍开发环境、开源数据集成、转换和可视化工具。这些工具是数据科学家日常工作的核心，掌握它们将帮助你更高效地进行数据分析和建模。



## 🖥️ 开发环境

上一节我们介绍了数据科学的基础工具，本节中我们来看看数据科学家常用的开发环境。开发环境是编写、测试和运行代码的平台，选择合适的开发环境能极大提升工作效率。



![](img/7e6dadb269c711fe0ce3601b3798e7a9_4.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_5.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_6.png)

### Jupyter Notebooks

![](img/7e6dadb269c711fe0ce3601b3798e7a9_7.png)

目前数据科学家最流行的开发环境之一是Jupyter。Jupyter最初是作为交互式Python编程工具出现的。它现在通过内核支持超过100种不同的编程语言。需要注意的是，Jupyter内核不应与操作系统内核混淆。Jupyter内核封装了不同编程语言的不同交互式解释器。

![](img/7e6dadb269c711fe0ce3601b3798e7a9_9.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_10.png)

Jupyter Notebooks的一个关键特性是能够将文档、代码、代码输出、Shell命令和可视化统一到单个文档中。



![](img/7e6dadb269c711fe0ce3601b3798e7a9_12.png)

### JupyterLab

![](img/7e6dadb269c711fe0ce3601b3798e7a9_13.png)

JupyterLab是Jupyter Notebooks的下一代产品，从长远来看，它将实际取代Jupyter Notebooks。JupyterLab中引入的架构变更使Jupyter更加现代化和模块化。

从用户的角度来看，JupyterLab引入的主要区别是能够打开不同类型的文件，包括Jupyter Notebooks、数据和终端。然后你可以在画布上排列这些文件。



### Apache Zeppelin

尽管Apache Zeppelin已完全重新实现，但它受到Jupyter Notebooks的启发，并提供类似的体验。一个关键区别是集成的绘图能力。在Jupyter Notebooks中，你需要使用外部库。在Apache Zeppelin中，绘图不需要编码。你还可以通过使用额外的库来扩展这些功能。



### RStudio

![](img/7e6dadb269c711fe0ce3601b3798e7a9_15.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_16.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_17.png)

RStudio是最古老的统计和数据科学开发环境之一，于2011年推出。它专门运行R和所有相关的R库。然而，Python开发也是可能的，因此R被紧密集成到这个工具中，以提供最佳的用户体验。

![](img/7e6dadb269c711fe0ce3601b3798e7a9_18.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_19.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_20.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_21.png)

RStudio将编程、执行、调试、远程数据访问、数据探索和可视化统一到一个工具中。



### Spyder

Spyder试图模仿RStudio的行为，将其功能引入Python世界。尽管Spyder没有RStudio相同水平的功能，但数据科学家确实将其视为Python世界中的一个替代方案。然而，在Python世界中，Jupyter的使用频率更高。

![](img/7e6dadb269c711fe0ce3601b3798e7a9_23.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_24.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_25.png)

下图展示了Spyder如何将代码、文档、可视化和其他组件集成到单个画布中。



![](img/7e6dadb269c711fe0ce3601b3798e7a9_26.png)

## 🚀 集群执行环境

![](img/7e6dadb269c711fe0ce3601b3798e7a9_28.png)

有时你的数据无法放入单台计算机的存储或主内存容量中。这就是集群执行环境发挥作用的地方。



![](img/7e6dadb269c711fe0ce3601b3798e7a9_29.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_30.png)

### Apache Spark

著名的集群计算框架Apache Spark是最活跃的Apache项目之一，被所有行业使用，包括许多财富500强公司。Apache Spark的关键特性是线性可扩展性。这意味着如果你将集群中的服务器数量翻倍，其性能也大致翻倍。



![](img/7e6dadb269c711fe0ce3601b3798e7a9_32.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_33.png)

### Apache Flink

![](img/7e6dadb269c711fe0ce3601b3798e7a9_34.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_35.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_36.png)

在Apache Spark开始获得市场份额后，Apache Flink被创建出来。Apache Spark和Apache Flink之间的关键区别在于，Apache Spark是一个批处理数据处理引擎，能够按文件处理大量数据。

另一方面，Apache Flink是一个流处理引擎，主要专注于处理实时数据流。尽管两个引擎都支持两种数据处理范式，但在大多数用例中，Apache Spark通常是首选。



![](img/7e6dadb269c711fe0ce3601b3798e7a9_38.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_39.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_40.png)

### Ray

数据科学执行环境的最新发展之一是Ray，它明确专注于大规模深度学习模型训练。



![](img/7e6dadb269c711fe0ce3601b3798e7a9_42.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_43.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_44.png)

## 📊 集成可视化工具

![](img/7e6dadb269c711fe0ce3601b3798e7a9_46.png)

让我们看看为数据科学家提供的完全集成和可视化的开源工具。使用这些工具不需要编程知识。这些工具支持最重要的任务，包括数据集成、转换、数据可视化和模型构建。

![](img/7e6dadb269c711fe0ce3601b3798e7a9_47.png)

以下是这类工具的主要特点：

![](img/7e6dadb269c711fe0ce3601b3798e7a9_49.png)

*   具有拖放功能的可视化用户界面
*   内置的可视化能力
*   可以通过R和Python编程进行扩展
*   具有与Apache Spark等工具的连接器



![](img/7e6dadb269c711fe0ce3601b3798e7a9_50.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_51.png)

### KNIME

KNIME于2004年在康斯坦茨大学诞生。正如你所见，KNIME具有拖放功能的可视化用户界面。它还具有内置的可视化能力。KNIME可以通过R和Python编程进行扩展，并具有与Apache Spark的连接器。



![](img/7e6dadb269c711fe0ce3601b3798e7a9_53.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_54.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_55.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_56.png)

### Orange

![](img/7e6dadb269c711fe0ce3601b3798e7a9_58.png)

这类工具的另一个例子是Orange。它比KNIME灵活性稍差，但更易于使用。



![](img/7e6dadb269c711fe0ce3601b3798e7a9_59.png)

## 📝 总结

![](img/7e6dadb269c711fe0ce3601b3798e7a9_61.png)

在本节课中，我们一起学习了数据科学中最常见的任务以及哪些开源工具与这些任务相关。我们介绍了多种开发环境，包括Jupyter、RStudio和Spyder，探讨了处理大数据的集群执行环境如Apache Spark和Apache Flink，并了解了无需编程的集成可视化工具如KNIME和Orange。

![](img/7e6dadb269c711fe0ce3601b3798e7a9_62.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_63.png)

![](img/7e6dadb269c711fe0ce3601b3798e7a9_64.png)

在下一个视频中，我们将描述一些你在数据科学经验中会遇到的老牌商业工具。让我们继续观看下一个视频以获取更多详细信息。