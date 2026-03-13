# 038：数据精炼工具

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_1.png)

在本节课中，我们将学习如何使用IBM Data Refinery这一数据精炼工具，来高效地清洗、转换和准备数据，从而为后续的数据分析和机器学习建模扫清障碍。

数据科学家通常需要花费大量时间执行数据清洗、整理和准备等基础任务。这些任务常常成为阻碍他们进入更有趣的数据集分析或机器学习模型构建阶段的瓶颈。这是因为数据集通常并非立即可用的格式，需要先经过清洗和精炼才能被数据科学家使用。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_3.png)

IBM Data Refinery正是为了解决这一问题而设计的，它简化了数据精炼及其工作流的任务。它提供了一个自助式的数据准备环境，让您可以快速分析、清洗和准备数据集。Data Refinery可在公共云、私有云和桌面端的Watson Studio中使用。

接下来，我们将通过一个具体场景，来了解Data Refinery的实际操作。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_5.png)

## 🛒 场景介绍：寻找最佳折扣

在本场景中，我们将使用Data Refinery来分析随时间变化的折扣数据，从而找出最佳交易。之后，我们还会将这一分析过程自动化，使其能够定期运行。

在开始分析之前，数据科学家首先查看了数据分布，并注意到“I”列存在数据缺失。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_7.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_8.png)

## 🔍 第一步：探索与清洗“offer”列

她首先对“offer”列进行了可视化。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_10.png)

她注意到该列包含了关于折扣的宝贵信息。许多字段包含百分比信息，有些则引用了之前的价格，表明当前有新的降价优惠。因此，她决定从“offer”列中提取信息。

以下是她的操作步骤：

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_7.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_8.png)

1.  **使用条件列操作**：她使用了条件列操作来创建一个新列。
    ![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_12.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_10.png)

2.  **筛选有效数据**：接下来，她使用筛选操作来剔除那些不包含答案或无法提取折扣信息的交易记录。
    ![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_18.png)

3.  **提取折扣值**：为了提取折扣信息，她使用了“替换子字符串”操作，并提供了一个用于从“offer”字段中提取折扣的模式。
    ![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_20.png)

4.  **转换数据格式**：在将折扣值转换为十进制数之后，她可以直观地看到所有可用的折扣。
    ![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_22.png)

## 📅 第二步：处理日期数据

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_12.png)

现在，她需要找出提供最佳折扣的月份。为此，她可视化了“date updated”列。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_14.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_16.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_24.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_18.png)

她注意到日期字段有多种格式：有些使用短横线（-），有些使用斜杠（/），还有些月份是以文本形式表示的。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_26.png)
![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_28.png)
![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_30.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_20.png)

她希望Data Refinery能够标准化这些数据并提取出月份。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_22.png)

以下是她的操作步骤：

1.  **转换日期格式**：她使用“转换列”操作，将列转换为日期格式，并选择“年月日（YMD）”模式。
    ![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_34.png)
    ![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_36.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_24.png)

2.  **提取月份**：接着，她提取出月份，并创建了一个名为“discount_month”的派生列。
    ![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_38.png)

至此，数据已经包含了所有提供销售的品牌、产品以及优惠可用的月份。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_26.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_40.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_28.png)

## 🏷️ 第三步：筛选心仪品牌

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_30.png)

这位数据科学家只对她偏好的品牌感兴趣。长期以来，她建立了一个偏好品牌列表，并已将该数据导入到她的项目中。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_32.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_42.png)
![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_43.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_34.png)

Data Refinery提供了多种关系型操作，例如左连接、内连接、右连接、全连接、半连接和反连接。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_36.png)

为了确保数据只包含她偏好的品牌，她使用了**半连接**操作。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_38.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_47.png)
![](img/4d1c7dfa0b92f7f0c50ec9a_48.png)

这个操作将品牌范围缩小，以匹配她的偏好。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_40.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_50.png)
![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_51.png)

然后，她选择了用于连接的键和结果字段。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_42.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_53.png)
![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_55.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_43.png)

可视化结果现在确认了品牌符合她的偏好，并且她需要执行一些聚合操作来找出最佳交易。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_45.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_57.png)
![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_58.png)

## 📊 第四步：数据聚合与分析

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_47.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_48.png)

多个因素决定了一笔交易是否划算。她感兴趣的是最佳优惠以及折扣活动的持续时间。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_50.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_60.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_51.png)

聚合销售数据将有助于理解交易详情。她按“品牌”和“折扣月份”对列进行分组。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_53.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_62.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_55.png)

并计算了最大折扣。最后，她对结果进行了排序。

现在，Data Refinery按品牌偏好和优惠可用时长显示了最佳交易。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_57.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_58.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_64.png)

## ⚙️ 第五步：执行完整分析与自动化

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_60.png)

最后一步是在完整数据集上执行分析。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_66.png)
![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_67.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_62.png)

她运行了完整分析，并可以监控其完成状态。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_69.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_64.png)

现在是时候将分析自动化，使其能够定期运行了，因为数据库中的数据会随时间增长。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_71.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_66.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_67.png)

她使用了一个个性化运行时环境来匹配更大的数据量。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_69.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_73.png)
![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_75.png)

并为自动化设置了计划。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_71.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_77.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_73.png)

这个每小时运行的计划会从数据库中读取更新后的数据。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_75.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_79.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_77.png)

并写入到目标表中。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_79.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_81.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_81.png)

## ✅ 总结

在本节课中，我们一起学习了如何使用IBM Data Refinery工具。通过一系列简单的操作和转换，Data Refinery帮助数据科学家从原始数据中发掘出有价值的交易信息，并完成了大部分繁重的工作。我们了解了如何：
1.  探索和清洗数据列。
2.  标准化和提取日期信息。
3.  使用关系型操作（如半连接）筛选数据。
4.  对数据进行聚合分析以得出结论。
5.  将整个分析流程自动化，实现定期运行。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_83.png)

感谢观看。

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_84.png)

![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_83.png)
![](img/4d1c7dfa0b92f7f0c5aaa4742b50ec9a_84.png)