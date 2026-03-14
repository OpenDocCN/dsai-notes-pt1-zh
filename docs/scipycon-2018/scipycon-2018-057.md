# 57：使用 Scikit-Learn 进行机器学习，第一部分 🧠

![](img/c04d678b99df1406ae3dbf232bf3d810_1.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_3.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_5.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_7.png)

在本节课中，我们将要学习机器学习的基本概念，并重点介绍 Scikit-Learn 库。我们将探讨什么是机器学习，何时以及为何在数据驱动的组织中使用它，并通过具体示例理解其工作流程。

![](img/c04d678b99df1406ae3dbf232bf3d810_9.png)

## 什么是机器学习？🤔

![](img/c04d678b99df1406ae3dbf232bf3d810_11.png)

机器学习，或称预测建模，是根据重复事件的历史观测数据来预测其结果的能力。

![](img/c04d678b99df1406ae3dbf232bf3d810_13.png)

我们必须使用过去感兴趣的观测数据或历史记录，并假设我们试图建模的行为在时间上是重复的，具有可预测的结构。我们无法对随机观测进行机器学习。

![](img/c04d678b99df1406ae3dbf232bf3d810_15.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_17.png)

我们使用统计工具和数学方法，从历史数据中提取某种统计摘要，并将该摘要转化为可执行的模型。可执行模型是一个可以在计算机上运行的程序。

![](img/c04d678b99df1406ae3dbf232bf3d810_19.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_21.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_23.png)

你可以将机器学习视为由专家编写的硬编码规则的替代方案。例如，如果你了解物理定律，可以编写某种模拟器，给定初始条件来预测后续发展。但如果你不了解所观察现象的内在规律，例如构建一个为用户推荐电影的系统，你无法建模用户的品味及其随时间的变化，除非你是心理学家。在这种情况下，你可以利用用户过去观看行为的历史记录，尝试找出电影之间的关系，并预测哪些电影适合哪些特定用户。

![](img/c04d678b99df1406ae3dbf232bf3d810_25.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_27.png)

在实践中，机器学习不一定是专家硬编码规则的替代方案，因为对于电影推荐系统，没有专家能为一百万不同人群提供推荐。雇佣专家来做这件事成本太高，不可行。机器学习适用于此类情况。

![](img/c04d678b99df1406ae3dbf232bf3d810_29.png)

另一个经验法则是，当重复事件的频率相当高时，机器学习可能有用。你可能至少有数千甚至数百万次重复观测。并且，犯个别错误的成本不能太高。如果单个错误代价高昂，例如涉及人命或数百万美元，你可能不希望使用机器学习，因为模型会犯错。例如，向某人推荐一部糟糕的电影并不是大问题，只要推荐系统总体上比不推荐要好。

## 一个具体示例：房地产估价 🏠

假设你经营一家房地产中介，拥有过去交易记录的数据库。

![](img/c04d678b99df1406ae3dbf232bf3d810_31.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_33.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_35.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_37.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_39.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_41.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_43.png)

你的数据库包含不同类型值的列。第一列是过去售出的住房类型，这是一个分类变量。第二列是房间数量，是一个整数。第三列是面积（平方米）。第四列是一个布尔标志，表示是否靠近公共交通。最后一列是交易价格。这些是历史数据。

![](img/c04d678b99df1406ae3dbf232bf3d810_45.png)

我们使用以下术语来描述此数据的结构：矩阵的行称为**样本**，或训练集的一部分。我们将用作模型输入的列称为**特征**。模型的输出，即我们试图预测的内容，称为**目标**。

![](img/c04d678b99df1406ae3dbf232bf3d810_47.png)

基于这些历史记录，我们试图找到目标变量与输入变量（即特征）之间的某种统计关系。我们将基于训练样本的统计分布来提取这种关系。

![](img/c04d678b99df1406ae3dbf232bf3d810_49.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_51.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_53.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_55.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_57.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_59.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_61.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_63.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_65.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_67.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_69.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_71.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_73.png)

当有新客户要求评估其房产时，你可以要求他们用与描述过去交易相同的特征来描述其房产。对于这些新房产，你还不知道价格，因为你尚未售出它们。但你可以使用在历史数据上训练的统计模型来估算一个合理的售价。你还可以要求模型给出置信区间。

![](img/c04d678b99df1406ae3dbf232bf3d810_75.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_77.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_79.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_81.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_83.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_85.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_87.png)

这里的两个新记录就是我们所说的**测试样本**，我们将对它们应用模型以进行预测。

![](img/c04d678b99df1406ae3dbf232bf3d810_89.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_91.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_93.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_95.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_97.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_99.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_101.png)

## 预测建模的一般数据流程 📊

![](img/c04d678b99df1406ae3dbf232bf3d810_103.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_105.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_107.png)

预测建模的一般数据流程始于原始历史记录。根据应用领域的不同，原始数据表示形式可能千差万别。例如，如果你想进行网站评论审核，原始数据可能是文本文档；对于图像分类，可能是手机摄像头拍摄的图像；对于工业应用，可能是特定传感器的读数；对于语音识别，可能是录音；对于数据库中的结构化交易，可能是带有列的Excel文件。

![](img/c04d678b99df1406ae3dbf232bf3d810_109.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_111.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_113.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_115.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_117.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_119.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_121.png)

原始数据表示形式非常特定于领域，可能不适合直接进行统计分析。原始数据格式可能不合适。同样重要的是，对于数据库中的每条记录，你还需要记录感兴趣的目标标签。例如，对于文本文档，可能是评论是否具有攻击性，或是否是垃圾邮件。对于图像，可能需要人工标注者标记图像的有趣内容。对于声音，需要转录感兴趣的部分。对于交易，可能是数据库中的一列，就像本例中我们记录了操作活动，并自然地拥有基于系统操作的目标列。

![](img/c04d678b99df1406ae3dbf232bf3d810_123.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_125.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_127.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_129.png)

第一步是从原始数据中提取某种数值表示，使其基本上能放入二维NumPy数组或数值矩阵中。关键步骤是这种转换是领域特定的。我们需要确保这些数字能捕捉原始数据的有趣部分。这种转换没有通用方法，非常依赖领域知识。作为数据科学家，你需要一些领域专业知识来进行这种特征提取。如今，对于声音和图像，存在一些由机器学习驱动的通用方法，如神经网络。但对于许多其他领域，你确实需要领域特定的专业知识。

![](img/c04d678b99df1406ae3dbf232bf3d810_131.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_133.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_135.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_137.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_139.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_141.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_143.png)

一旦你有了这些数值特征向量，就可以使用通用的机器学习算法，例如Scikit-Learn中提供的算法。将机器学习应用于特征向量和标签的计算结果将产生一个可执行模型，这是一个可以保存在硬盘上、转移到另一台机器或部署在手机上的程序。然后你可以忘记原始数据，只关注模型。模型将接收具有相同原始数据表示形式的新观测数据。你需要应用相同的特征转换、特征提取步骤。如果与之前的转换不一致，模型将输出无意义的预测。但如果你应用一致且可重复的转换，模型将能够预测预期的目标。

![](img/c04d678b99df1406ae3dbf232bf3d810_145.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_147.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_149.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_151.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_153.png)

所有这些操作，例如，如果你有一个小数据集，想进行线性回归以提取数据趋势，特别是如果它是自然以Excel电子表格表示的交易日志，也许你可以直接转换列，进行数据解析等操作，在新列中提取一些特征，并应用Excel中的线性回归例程来提取模型。如果你想将其产品化，使其能够作为程序部署，最好使用通用编程语言，如Python，以便可以将其打包为软件、运行测试、通过持续集成系统进行部署等。此外，Python能够扩展到更大的数据量，例如轻松处理内存中的GB级数据，而使用Excel可能会有问题。通常，如果你想在Python中实现，可以使用pandas库进行数据转换、提取、特征提取等操作，然后使用Scikit-Learn进行统计预测建模的数学部分。

![](img/c04d678b99df1406ae3dbf232bf3d810_155.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_157.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_159.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_161.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_163.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_165.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_167.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_169.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_171.png)

如果你的数据非常大，原始数据转换可能无法在单台机器的内存中完成。在这种情况下，你可能需要使用某种大数据系统，例如运行在Hadoop集群上的Hive，这基本上是一种在集群上运行分布式操作的SQL引擎。但有许多替代方案可以实现。通常，当你在此处提取特征向量时，即使原始数据可能非常大，你为机器学习提取的统计摘要（即特征向量）可能会小得多。如果是这种情况，那么你可以使用Scikit-Learn，通过将生成的特征向量加载到内存中并自然运行Scikit-Learn来构建模型。作为替代方案，你可以使用云上的分析数据库，例如Redshift或Google BigQuery等，并执行相同的操作。这是一种非常常见的模式。

![](img/c04d678b99df1406ae3dbf232bf3d810_173.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_175.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_177.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_179.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_181.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_183.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_185.png)

此外，如果你不想使用Python，也可以使用替代方案。例如，Spark可以同时处理数据预处理部分和一些机器学习功能。如今，你也可以使用Dask进行可扩展的数据预处理部分，使用Scikit-Learn进行机器学习部分。我们将在会议期间介绍一些相关内容。

![](img/c04d678b99df1406ae3dbf232bf3d810_187.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_189.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_191.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_193.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_195.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_197.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_199.png)

## 机器学习的典型应用实例 🌟

![](img/c04d678b99df1406ae3dbf232bf3d810_201.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_203.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_205.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_207.png)

以下是Scikit-Learn的一些典型真实应用案例，这些用户已公开交流了他们对Scikit-Learn的使用。

![](img/c04d678b99df1406ae3dbf232bf3d810_209.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_211.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_213.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_215.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_217.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_219.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_221.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_223.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_225.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_227.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_229.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_231.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_232.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_234.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_236.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_238.png)

*   **《纽约时报》**：有一个数据科学家团队，致力于建模新闻的传播性和读者的参与度。
*   **Airbnb**：大量使用机器学习进行欺诈检测或公寓定价建模。
*   **Spotify**：构建推荐系统，基于用户的收听行为创建个性化的播放列表。
*   **Etsy**：进行大量库存预测和时尚或其他商品的趋势检测。
*   **工业系统预测性维护**：对复杂工业系统的运行进行预测建模。目标是建立系统正常运行时的模型。一旦检测到漂移，就可以发现可能存在问题的迹象。如果任其发展而不干预，可能会导致代价高昂的事故。因此，你可以在事故发生前派出维护团队。这就是所谓的预测性维护。对于此类操作，你可以使用我们称为**异常检测**的机器学习算法家族。
*   **OK Cupid**：这是一个特殊的推荐系统案例，用于交友网站的性格匹配。

![](img/c04d678b99df1406ae3dbf232bf3d810_240.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_242.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_244.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_246.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_248.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_250.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_252.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_254.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_256.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_258.png)

还有许多其他应用，如欺诈检测、客户流失预测等，这些都是非常常见的应用。这些都是商业应用。但在SciPy会议上，听众可能更偏向科学领域。你可以想象，机器学习和Scikit-Learn也可用于帮助解决天文学、遗传学等领域的科学应用。

![](img/c04d678b99df1406ae3dbf232bf3d810_260.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_262.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_264.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_266.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_268.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_270.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_272.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_274.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_276.png)

## 关于 Scikit-Learn 📚

![](img/c04d678b99df1406ae3dbf232bf3d810_278.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_280.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_282.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_284.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_286.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_288.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_290.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_291.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_293.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_295.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_297.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_299.png)

Scikit-Learn 是一个开源的机器学习库。它用 Python 编写，因为它属于 SciPy 生态系统。它大量依赖 NumPy 进行数组和线性代数运算，依赖 SciPy 处理稀疏矩阵等数据结构以及优化算法。我们还大量使用 Cython 来加速一些计算密集型算法，这些算法无法自然地通过向量操作组合（如 NumPy 线性代数操作）来表达。只要进行 NumPy 数组操作（如向量矩阵乘法），NumPy 就非常快。但有时，对于决策树等算法，你需要使用嵌套的 for 循环。在这种情况下，NumPy 无法用于加速计算。此时，我们使用 Cython。

![](img/c04d678b99df1406ae3dbf232bf3d810_301.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_303.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_305.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_307.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_309.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_311.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_313.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_315.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_317.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_319.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_321.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_323.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_325.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_326.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_328.png)

Scikit-Learn 的一个非常有趣的主要特性，我认为也是它变得流行的原因，是我们在设计该库时付出了大量努力，以确保其 API 非常简单。公共 API 非常简单且非常统一，因此库中实现了许多机器学习算法。其中大多数都实现了两种方法：`fit` 和 `predict`，或 `fit` 和 `transform`。我们稍后会解释其含义。我认为，这正是 Scikit-Learn 非常有趣的原因。

![](img/c04d678b99df1406ae3dbf232bf3d810_330.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_332.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_334.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_336.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_338.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_340.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_342.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_344.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_346.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_348.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_350.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_352.png)

最后，我们还提供了一些额外的工具，例如模型评估（用于评估模型的预测质量）、模型选择（用于自动选择模型的超参数，我们将在今天下午介绍），以及如何构建模型集成。例如，像决策树这样的模型，单独训练时效果不是很好，但如果你训练一个大的集成模型，它们的准确性会高得多。

![](img/c04d678b99df1406ae3dbf232bf3d810_354.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_356.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_358.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_360.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_362.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_364.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_366.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_368.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_370.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_372.png)

## Scikit-Learn 的基本工作流程 ⚙️

![](img/c04d678b99df1406ae3dbf232bf3d810_374.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_376.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_378.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_380.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_382.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_384.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_386.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_388.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_390.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_392.png)

如果我们采用之前介绍的预测建模数据流程并将其垂直放置，我们从训练数据开始。这里的特征提取已经完成。因此，训练数据是一个具有两个维度的 NumPy 数组：记录数和特征数。我们还有一个用于每个记录的训练标签向量。这也是一个 NumPy 数组。

![](img/c04d678b99df1406ae3dbf232bf3d810_394.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_396.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_398.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_399.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_401.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_403.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_405.png)

我们将输入称为 `X`，输出称为 `y`。`X` 是一个二维的 NumPy 数组，`y` 是一个一维的 NumPy 标签/目标值数组。

![](img/c04d678b99df1406ae3dbf232bf3d810_407.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_409.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_411.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_413.png)

要在 Scikit-Learn 中构建模型，我们只需从 Scikit-Learn 的某个包中导入一个类，通过提供一些超参数来实例化该类，然后在训练数据上拟合模型。完成此操作后，模型将被就地修改，我们可以对新数据调用 `predict` 方法。

![](img/c04d678b99df1406ae3dbf232bf3d810_415.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_416.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_418.png)

这个新数据具有与训练集相同数量的列，因此它也是一个二维的 NumPy 数组。这次它将返回对 `y` 的估计。如果你可以访问测试集的真实标签，则可以计算模型的某些性能指标，以查看你的模型是否能够泛化到新数据。我们也可以在训练集 `X_train` 和 `y_train` 上计算准确率分数，但通常这不太有趣，因为我们希望拥有一个能够对未来训练时无法访问的数据做出良好预测的模型。

![](img/c04d678b99df1406ae3dbf232bf3d810_420.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_422.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_424.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_426.png)

正如我之前所说，Scikit-Learn 中的所有模型都具有非常统一的 API。这是一个非常简单的 Python 程序，使用不同的模型执行完全相同的操作。这次我们使用来自 `sklearn.svm` 的支持向量机。我们创建模型，调用 `fit`，进行一些预测，然后计算一些性能指标。这里我们使用 F1 分数而不是准确率。这只是另一种分类指标。

因为模型具有 `fit` 和 `predict` 方法，Scikit-Learn 中所有用于分类的模型都具有相同的 `fit` 和 `predict` 方法。因此，你可以用 Scikit-Learn 中的另一个分类器（例如逻辑回归）替换该模型。只有前两行需要更改，其余的训练、预测和评估部分都相同。

![](img/c04d678b99df1406ae3dbf232bf3d810_428.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_430.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_432.png)

如果你想尝试比线性分类器更复杂的东西，可以使用随机森林，同样，只有前两行会改变。

![](img/c04d678b99df1406ae3dbf232bf3d810_434.png)

## 为什么有这么多机器学习模型？🤷

![](img/c04d678b99df1406ae3dbf232bf3d810_436.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_438.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_440.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_442.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_444.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_446.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_448.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_450.png)

我们拥有如此多机器学习模型的原因是，大多数模型都对数据的分布以及标签与输入样本的关联方式做出了强有力的统计假设。

![](img/c04d678b99df1406ae3dbf232bf3d810_452.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_454.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_456.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_458.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_460.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_462.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_464.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_466.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_468.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_470.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_472.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_474.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_476.png)

这里我有三个分类问题。目标是根据二维空间中的位置预测一个点是蓝色还是红色。输入特征是二维平面上的 x 和 y 轴两个连续变量。

![](img/c04d678b99df1406ae3dbf232bf3d810_478.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_480.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_482.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_484.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_486.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_488.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_490.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_492.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_494.png)

第一个数据集，你有两组点，一组嵌套在另一组中，像两个半月形。第二个数据集，蓝点在中间，红点遍布四周，加上一些噪声。最后一个数据集是两个斑点，蓝色在左边，红色在右边，但也有噪声，所以它们有些重叠。

![](img/c04d678b99df1406ae3dbf232bf3d810_496.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_498.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_500.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_502.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_503.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_505.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_507.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_509.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_511.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_513.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_515.png)

在第一列中，我们有一个称为 K 近邻分类的机器学习算法。你可以看到它能够找到一个统计决策函数，该函数找到两个区域之间的边界。在这里，它能够识别中心是蓝色，外部是红色。在那种情况下，是某种直线。这个模型是某种可以通过计算到新数据点的距离来泛化的数据库。但它没有对数据的结构做出任何强有力的假设。在大多数情况下，它能够找到大致良好的分离。但你可以看到决策边界非常嘈杂。这可能是个问题。

第二列是一个线性模型。你可以看到分隔蓝色区域和红色区域的边界是二维空间中的一条直线。特别是在这种情况下，假设存在一条直线可以分隔蓝色和红色是完全错误的。因此，在这种情况下，预测质量非常差。然而，在这里这是正确的假设，因为这是分隔蓝色和红色的最佳方式。因此，这个模型是最好的。而且它训练速度最快，因为线性模型通常训练速度非常快。因此，这是一个快速尝试的良好基线。

![](img/c04d678b99df1406ae3dbf232bf3d810_517.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_519.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_521.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_523.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_525.png)

还有支持向量机与 RBF 核，它基本上是 K 近邻的光滑版本。你可以这样理解它。在这种情况下，你可以看到决策边界噪音小得多。因此在所有情况下它可能更好。但在高维数据中，情况可能并非总是如此。

![](img/c04d678b99df1406ae3dbf232bf3d810_527.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_529.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_531.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_533.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_535.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_537.png)

你还有此列中的决策树，以及旁边的随机森林，它基本上是决策树的集成。你可以看到它在大多数情况下都很好。还有许多其他算法，我不会进一步介绍。

![](img/c04d678b99df1406ae3dbf232bf3d810_539.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_541.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_543.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_545.png)

这里的主要观点是，根据你可以对数据做出的假设，有时线性模型将是你能得到的最佳模型。其他时候，你确实需要某种非线性模型，如决策树或具有非线性核的支持向量机。否则，你将得到糟糕的预测。我们将在笔记本中看到如何评估这些质量。

![](img/c04d678b99df1406ae3dbf232bf3d810_547.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_549.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_551.png)

背景中的颜色基本上是模型的预测。如果你在训练集中未观察到的位置取一个新点，模型将预测其类别。如果是深红色，则非常确信它是红色位置。如果是深蓝色，则非常确信它是蓝色位置。如果是浅白色，则不是非常确信。例如，它会给出红色概率为 0.6。你可以使用此置信度来决定在置信度不够时不进行预测。有时，根据应用，这可能是个好主意，你宁愿不给出答案，也不愿给出错误答案。

![](img/c04d678b99df1406ae3dbf232bf3d810_553.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_555.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_557.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_559.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_561.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_563.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_565.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_567.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_569.png)

## Scikit-Learn 的模型与文档 📖

![](img/c04d678b99df1406ae3dbf232bf3d810_571.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_573.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_575.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_577.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_579.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_581.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_583.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_585.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_587.png)

如果你想了解 Scikit-Learn 中为分类、回归、聚类、降维、模型选择和预处理实现的所有模型，我们将在今天的教程中解释这些含义。但也有广泛的在线文档。我认为这是 Scikit-Learn 项目成功的另一个原因，从一开始，我们就决定记录所有内容。如果文档没有更新，我们会拒绝拉取请求。我认为这也成为了深入了解机器学习算法工作原理、在哪些情况下有效以及在哪些情况下容易失败的非常好的资源。因此，请随时使用它。大多数时候，如果你在 Google 上搜索“scikit-learn something”，你会找到相关文档。

![](img/c04d678b99df1406ae3dbf232bf3d810_589.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_591.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_593.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_595.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_597.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_599.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_601.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_603.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_605.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_607.png)

## 在数据驱动组织中使用预测模型 🏢

![](img/c04d678b99df1406ae3dbf232bf3d810_609.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_611.png)

现在让我们退一步，思考在哪里使用预测模型。假设你正在运营某种数据驱动的组织。

![](img/c04d678b99df1406ae3dbf232bf3d810_613.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_615.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_617.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_619.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_621.png)

你将收集的数据流的输入是你的应用程序的用户活动（如果是移动应用程序、桌面应用程序或连接到网络的工作站应用程序）。有时你没有用户，但在物联网应用中，你有一个传感器网络。但在所有这些情况下，基本上每个客户端组件都会发出一个事件流，你可以使用队列将其收集到分布式事件日志中。这将确保即使用户的某台机器出现故障，你也能记录所有事件。并使该事件流可分类到应用程序中的不同服务。

![](img/c04d678b99df1406ae3dbf232bf3d810_623.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_625.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_627.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_629.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_631.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_633.png)

通常情况下，如果你从客户端应用程序收集的事件吞吐量很高，有时你需要做出非常快速的决策。因此，你将拥有某种流处理系统，该系统也是分布式的，以实现冗余和可扩展性。它将非常快速地进行转换，并将结果放入为 Web 用户应用程序 Web 界面或移动应用程序提供服务的某种数据库中。这是一种在线事务处理系统或在线数据库。它响应用户刷新页面的查询。例如，如果你考虑 Twitter 应用程序，如果某个用户使用手机发推文，你会将推文发送到此系统以确保不会丢失它。然后处理它，例如查找所有关注者的列表，将推文存储在所有这些用户时间线的数据库中。如果任何关注者刷新页面，我们会查询此数据库并看到与最近发布的所有其他推文一起收集的新推文。这通常需要不到一秒或最多几秒钟。

![](img/c04d678b99df1406ae3dbf232bf3d810_635.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_637.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_639.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_641.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_643.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_645.png)

然后，你通常拥有某种长时间批处理系统，能够对收集的事件历史执行操作。例如，第一步是将数据存储在某种结构化的分布式存储中。它可以是结构化存储，也可以只是 HDFS 分布式文件系统。然后，在其之上，你拥有一个分布式批处理系统，可以聚合计算整个系统历史部分的统计数据，并输出某种统计摘要。这些操作的结果可以存储在结构化数据库中，以便进行快速分析查询。如果你的组织中有业务分析师，你可能希望输出某种仪表板来运营公司，并查看世界不同地区的销售趋势如何演变。

![](img/c04d678b99df1406ae3dbf232bf3d810_647.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_649.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_651.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_653.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_655.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_657.png)

但你也可以使用此分析数据库或直接使用此分布式批处理系统来提取特征以构建预测模型。预测模型也可以为业务分析师提供某种预测。但它们也可用于向应用程序提供推荐，例如，对于 Twitter，你可以突出显示推荐的推文，例如最重要的推文的统计摘要。你还可以将预测模型的副本嵌入流处理系统中，用于自动审核，例如垃圾邮件过滤。这样你就不会存储垃圾邮件推文，从一开始就拒绝它们。

![](img/c04d678b99df1406ae3dbf232bf3d810_659.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_661.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_663.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_665.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_667.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_669.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_671.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_673.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_675.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_677.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_679.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_681.png)

为了实现这种架构，这是你在许多组织中都会看到的模式。有标准的开源系统来完成每个步骤。例如，对于分布式队列系统，你可以使用 Kafka。对于流处理系统，你可以使用 Apache Storm、Spark Streaming 或 Apache Flink。对于存储结果并非常快速响应查询的数据库，你可以使用 HBase 或 Cassandra 等。对于长期存储的分布式系统，你可以使用 HDFS 和 Hadoop 或 Amazon S3 上的存储。你可以使用 MapReduce Hadoop 作为批处理系统，Redshift 作为分析数据库，Scikit-Learn 用于机器学习等。但你也可以使用其他组合，例如 Spark 可以做很多事情，因此非常流行。你也可以从 Python 中使用它。

![](img/c04d678b99df1406ae3dbf232bf3d810_683.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_685.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_687.png)

## 总结 📝

![](img/c04d678b99df1406ae3dbf232bf3d810_689.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_691.png)

![](img/c04d678b99df1406ae3dbf232bf3d810_693.png)

本节课中，我们一起学习了机器学习的基本概念，了解了预测建模的核心思想及其在数据驱动组织中的应用场景。我们通过房地产估价的例子，具体理解了样本、特征和目标等关键术语。我们还探讨了预测建模的一般数据流程，从原始数据到特征提取，再到模型训练和部署。最后，我们介绍了强大的 Scikit-Learn 库，了解了其设计哲学、统一 API 以及在实际业务和科学领域的广泛应用。这为我们后续动手实践打下了坚实的理论基础。