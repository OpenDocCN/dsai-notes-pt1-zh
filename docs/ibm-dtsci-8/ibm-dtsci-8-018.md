# 018：仪表盘概述

![](img/70b897a76afbfd341a3227b9efb5a2af_1.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_2.png)

在本节课中，我们将学习交互式数据应用如何帮助提升业务表现，并了解可用于构建此类应用的工具。

![](img/70b897a76afbfd341a3227b9efb5a2af_4.png)

## 概述

![](img/70b897a76afbfd341a3227b9efb5a2af_6.png)

仪表盘通过在一个集中的位置展示实时数据和合适的图表，使利益相关者能够轻松理解业务的运行状况、存在的问题以及需要改进的方面。这有助于企业做出明智的决策，从而提升业务表现。通常，最佳的仪表盘能够回答关键的商业问题。

![](img/70b897a76afbfd341a3227b9efb5a2af_7.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_8.png)

## 仪表盘的价值：一个案例

假设你被分配了一项任务：监控并报告美国国内航班的运营表现。

![](img/70b897a76afbfd341a3227b9efb5a2af_10.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_12.png)

以下是年度审查报告需要包含的项目：
*   2019年航班数量排名前十的航空公司。
*   2019年按月份划分的航班数量。
*   从加利福尼亚州出发，按距离组划分的前往其他州的旅客数量。

让我们看看呈现此类报告的两种方式。

### 报告类型一：传统表格报告

![](img/70b897a76afbfd341a3227b9efb5a2af_14.png)

第一种方式是通过表格呈现信息，并将从表格中得出的推论记录在案以供参考。

![](img/70b897a76afbfd341a3227b9efb5a2af_16.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_17.png)

### 报告类型二：交互式仪表盘报告

![](img/70b897a76afbfd341a3227b9efb5a2af_18.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_20.png)

第二种方式是以仪表盘格式呈现相同的报告。

![](img/70b897a76afbfd341a3227b9efb5a2af_22.png)

在仪表盘中，将鼠标悬停在每个图表上，可以在底部的旭日图中查看数据点的详细信息。你可以点击不同的数字，向下钻取层级，获取每个细分部分的详细信息。

![](img/70b897a76afbfd341a3227b9efb5a2af_24.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_25.png)

你能观察到两种方式在呈现结果上的差异吗？

![](img/70b897a76afbfd341a3227b9efb5a2af_27.png)

如果需要基于实时数据而非静态数据生成报告，情况又会如何？此外，使用表格和文档呈现结果耗时较长，视觉吸引力较低，且更难以理解。

![](img/70b897a76afbfd341a3227b9efb5a2af_29.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_31.png)

数据科学家需要能够围绕发现创建并传达一个故事，让利益相关者易于理解。考虑到这一点，仪表盘是理想的选择。

![](img/70b897a76afbfd341a3227b9efb5a2af_33.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_35.png)

## Python中的Web仪表盘工具

![](img/70b897a76afbfd341a3227b9efb5a2af_37.png)

接下来，我们来看看Python中可用的基于Web的仪表盘工具选项。

### Dash

**Dash** 是一个用于构建Web分析应用的Python框架。它构建在 **Flask**、**Plotly.js** 和 **React.js** 之上。Dash非常适合构建具有高度自定义用户界面的数据可视化应用。

![](img/70b897a76afbfd341a3227b9efb5a2af_39.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_40.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_41.png)

### Panel

![](img/70b897a76afbfd341a3227b9efb5a2af_42.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_43.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_44.png)

**Panel** 可以与来自 **Bokeh**、**Matplotlib**、**HoloViews** 以及许多其他Python绘图库的可视化图表协同工作，使它们能够单独或与控制它们的交互式小部件结合时即时查看。Panel在Jupyter Notebook中同样表现优异，可用于创建快速数据探索工具。它也可以用于独立部署的应用和仪表盘，让你可以根据需要在不同场景间轻松切换。

![](img/70b897a76afbfd341a3227b9efb5a2af_46.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_48.png)

### Voila

![](img/70b897a76afbfd341a3227b9efb5a2af_50.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_51.png)

**Voila** 可以将Jupyter Notebook转换为独立的Web应用程序。它可以与Jupyter Flex等独立的布局工具或Voila Butify等模板一起使用。

![](img/70b897a76afbfd341a3227b9efb5a2af_53.png)

### Streamlit

![](img/70b897a76afbfd341a3227b9efb5a2af_55.png)

**Streamlit** 可以轻松地将数据脚本转换为可共享的Web应用，其核心应用理念有三点：
*   拥抱Python脚本。
*   将小部件视为变量。
*   重用数据和计算。

### 其他工具

![](img/70b897a76afbfd341a3227b9efb5a2af_57.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_58.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_59.png)

以下是其他一些可用于构建仪表盘的工具：
*   **Bokeh**： 一个绘图库，也是一个部件和应用库。它充当图表和仪表盘的服务端。Panel就是构建在Bokeh之上的Web仪表盘工具之一。
*   **ipywidgets**： 提供了一系列与Jupyter兼容的小部件，以及一个受许多Python库支持的接口。但共享仪表盘需要一个像Voila这样的独立可部署服务器。
*   **Matplotlib**： 一个用于在Python中创建静态、动画和交互式可视化的综合库。
*   **Bottle**： 允许用户使用纯Python构建仪表盘。
*   **Flask**： 一个Python后端Web服务器，可用于构建任意网站，包括那些包含作为Flask仪表盘功能的Python图表的网站。

![](img/70b897a76afbfd341a3227b9efb5a2af_60.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_61.png)

你可以从源链接了解更多关于这些工具的信息。

![](img/70b897a76afbfd341a3227b9efb5a2af_63.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_65.png)

在本课程中，我们将重点学习 **Dash**。

![](img/70b897a76afbfd341a3227b9efb5a2af_67.png)

![](img/70b897a76afbfd341a3227b9efb5a2af_68.png)

## 总结

![](img/70b897a76afbfd341a3227b9efb5a2af_70.png)

本节课我们一起学习了仪表盘在提升业务洞察和决策效率方面的重要价值。我们通过一个航班数据报告的案例，对比了传统表格报告与交互式仪表盘报告的差异，明确了仪表盘在实时性、交互性和易理解性上的优势。最后，我们概述了Python生态中主流的Web仪表盘构建工具，如Dash、Panel、Voila和Streamlit等，并明确了本课程将重点深入讲解Dash框架。