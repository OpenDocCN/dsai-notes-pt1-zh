# 19：数据预处理入门 🧠

![](img/f3f18789ed9809b85e76f612bee9ca6a_1.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_3.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_4.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_5.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_6.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_8.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_9.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_10.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_11.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_12.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_13.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_15.png)

在本节课中，我们将学习生成式AI中数据预处理的基础知识。数据预处理是将原始数据转换为适合模型训练的格式的关键步骤，它直接决定了AI模型的输出质量和可靠性。

![](img/f3f18789ed9809b85e76f612bee9ca6a_17.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_18.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_19.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_20.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_21.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_22.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_24.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_25.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_26.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_27.png)

大家好，我是安德鲁·布朗，欢迎来到免费的生成式AI训练营。今天和我一起的是基拉，她将帮助我们了解数据方面的知识。我必须诚实地说，我在数据方面真的很不擅长，所以我非常需要这次入门讲解。基拉非常慷慨地为我们分解了这些内容。在开始看幻灯片之前，我想请基拉介绍一下自己。

![](img/f3f18789ed9809b85e76f612bee9ca6a_29.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_30.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_31.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_32.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_33.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_34.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_35.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_36.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_37.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_39.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_40.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_41.png)

好的。大家好，我是基拉·多特森，我是Data B的创始人兼CEO，这是一家数据职业指导和咨询公司。我还在业余时间从事Kubernetes和数据合同方面的工作。感谢邀请我来到这里。现在，我们来开始吧。

![](img/f3f18789ed9809b85e76f612bee9ca6a_42.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_43.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_44.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_46.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_47.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_48.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_49.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_50.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_51.png)

## 为什么数据预处理对生成式AI至关重要 🎯

![](img/f3f18789ed9809b85e76f612bee9ca6a_53.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_54.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_55.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_56.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_57.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_58.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_59.png)

数据预处理对于生成式AI至关重要，但在当前AI领域却常常被忽视。虽然大型语言模型非常强大，但它们并非魔法，也不是无所不知的软件。这一点非常重要，我需要重复强调：模型输出的质量在很大程度上取决于数据是如何准备的。

![](img/f3f18789ed9809b85e76f612bee9ca6a_61.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_62.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_63.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_64.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_65.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_66.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_67.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_68.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_69.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_70.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_71.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_73.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_74.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_75.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_76.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_77.png)

如果模型存在偏见，那不是模型的问题，而是数据的问题。如果模型给出错误信息，那不是模型的问题，而是数据的问题。这与那句流行的口号“垃圾进，垃圾出”完全一致。

![](img/f3f18789ed9809b85e76f612bee9ca6a_79.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_80.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_81.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_82.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_83.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_84.png)

一个很好的例子是，假设一家客户服务公司正在实施自己的聊天GPT，并为其提供自己的客服对话记录。很多时候，这些记录充满了不一致之处，比如日期格式不同、称呼客户的方式不同、拼写错误，或者客服代表使用双语与不同地区或国家的客户交流。如果没有适当的处理或预处理，AI可能会生成不一致或不正确的响应。

![](img/f3f18789ed9809b85e76f612bee9ca6a_86.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_87.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_88.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_89.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_90.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_91.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_92.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_93.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_94.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_95.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_96.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_97.png)

## 课程议程 📋

![](img/f3f18789ed9809b85e76f612bee9ca6a_99.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_100.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_101.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_102.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_103.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_104.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_105.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_106.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_107.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_108.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_109.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_110.png)

接下来，我们将讨论为什么预处理很重要，以便大家更好地理解它的作用。我们还会谈到一些关于生成式AI的误解，以及一些知名公司中生成式AI出错的例子。然后，我们会讨论数据质量——我最喜欢的话题，接着是一些最佳实践。最后，我会给大家一些关键要点，供大家在继续参与训练营时牢记。

![](img/f3f18789ed9809b85e76f612bee9ca6a_112.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_113.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_114.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_115.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_116.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_117.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_118.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_119.png)

## 为什么预处理对生成式AI很重要？ 🤔

![](img/f3f18789ed9809b85e76f612bee9ca6a_121.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_122.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_123.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_124.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_125.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_126.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_127.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_128.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_129.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_130.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_131.png)

数据预处理是将原始数据转换为能够高效、有效地用于模型训练的格式的过程。其中最重要的部分是“原始数据”，因为数据在被任何人（无论是分析师还是数据工程师）使用之前，通常都处于原始状态。很多时候，这些数据会被转换，以便业务或使用它的客户能够理解。

![](img/f3f18789ed9809b85e76f612bee9ca6a_133.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_134.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_135.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_136.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_137.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_138.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_139.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_140.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_141.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_142.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_143.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_145.png)

你希望输入数据尽可能与你所用模型的要求保持一致，以确保模型的输出或响应有意义，并且对用户有价值。数据准备得越好，出现过拟合模型的可能性就越小。顺便说一下，过拟合是指模型对训练数据过于精细地调整，以至于在新的、未见过的数据上表现不佳。因此，你需要确保数据尽可能从一开始就接近高质量，因为随着你不断微调模型，成本也会越来越高。

![](img/f3f18789ed9809b85e76f612bee9ca6a_146.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_147.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_148.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_149.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_151.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_152.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_153.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_154.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_155.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_157.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_158.png)

## 预处理的好处 ✨

![](img/f3f18789ed9809b85e76f612bee9ca6a_160.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_161.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_162.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_163.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_164.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_165.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_167.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_168.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_169.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_170.png)

让我们谈谈预处理的一些好处。

![](img/f3f18789ed9809b85e76f612bee9ca6a_171.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_172.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_173.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_175.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_176.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_177.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_178.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_179.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_180.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_181.png)

1.  **提高模型准确性**：模型基本上是基于它在数据中发现的模式和关系来输出响应的。经过适当处理的数据将帮助模型更快、更容易地学习这些模式和关系，从而使响应更清晰、更容易理解。
2.  **更好的泛化能力**：干净、标准化的数据有助于构建能够很好地泛化到新的、未见过的数据的模型，从而提高模型的稳健性和可靠性。
3.  **最小化偏见**：通过有效处理缺失值等技术，确保模型训练不会产生偏见、扭曲，或者对给定的提示产生不恰当的响应。

![](img/f3f18789ed9809b85e76f612bee9ca6a_182.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_183.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_184.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_185.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_186.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_188.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_190.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_191.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_192.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_193.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_194.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_195.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_196.png)

这真的很有趣，因为我认为罗阿告诉我们，很多现有的模型都是在互联网上的所有数据上训练的，这有点令人不安，因为互联网是一个狂野的地方。我几乎想知道他们在幕后做了什么，才让这些东西不会吐出疯狂的内容。但说到“预处理”这个词，我理解在数据管道中有一系列步骤，我听说过预处理这个东西。它是一个很大的类别吗？还是像我们管道中一个非常具体的东西？它有多大或多小？

![](img/f3f18789ed9809b85e76f612bee9ca6a_198.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_199.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_200.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_201.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_202.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_203.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_204.png)

## 预处理的范畴 📦

![](img/f3f18789ed9809b85e76f612bee9ca6a_205.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_206.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_207.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_208.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_209.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_211.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_212.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_213.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_214.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_215.png)

这是一个很大的类别。它再次取决于数据的用例、数据的混乱程度以及数据类型。结构化数据的处理可能是最简单的，因为它一开始就有统一的格式。可能简单到只需要从数据中删除重复项，无论是文本数据还是其他数据，或者删除一些缺失的列或行。当然，当涉及到处理图像、JSON或其他非结构化数据时，如果存在一些损坏的JSON括号之类的问题，处理起来就复杂多了，尤其是对于工程师来说，找出代码中缺少一个逗号已经够难了，想象一下AI软件必须这样做，而它还没有接受过如何做的训练，那简直是盲人摸象。

![](img/f3f18789ed9809b85e76f612bee9ca6a_217.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_218.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_219.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_220.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_221.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_222.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_223.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_224.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_225.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_226.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_227.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_228.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_230.png)

这就是为什么预处理如此重要，尤其是在将数据提供给人的场景下。比如，你提供给分析师的数据，他们可能不理解数据是如何处理的技术细节，也可能不理解他们收到的数据背后的上下文。这就是为什么预处理很重要，因为你试图使数据尽可能格式化，以满足最终用户的需求。

![](img/f3f18789ed9809b85e76f612bee9ca6a_231.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_232.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_233.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_235.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_236.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_237.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_238.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_239.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_240.png)

例如，在训练营中，我想处理一部电影的字幕，里面有所有的时间码，我最终把它们删除了，因为我只想要原始的语言文本。这是预处理吗？

![](img/f3f18789ed9809b85e76f612bee9ca6a_242.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_243.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_244.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_245.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_246.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_248.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_250.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_251.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_252.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_253.png)

是的，绝对是。这实际上属于数据质量类别，即标准化你的数据，确保其格式正确，没有时间码。

![](img/f3f18789ed9809b85e76f612bee9ca6a_255.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_256.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_257.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_258.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_259.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_260.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_261.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_262.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_263.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_265.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_266.png)

## 生成式AI的误解与真相 🧐

![](img/f3f18789ed9809b85e76f612bee9ca6a_268.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_269.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_270.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_271.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_272.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_273.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_274.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_275.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_276.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_277.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_278.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_279.png)

接下来，让我们谈谈一些关于生成式AI的误解。我会先列出一些神话，然后告诉你真相。

![](img/f3f18789ed9809b85e76f612bee9ca6a_281.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_282.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_283.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_284.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_285.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_286.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_287.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_288.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_289.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_290.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_291.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_292.png)

**误解一：LLM可以理解任何数据格式。**

![](img/f3f18789ed9809b85e76f612bee9ca6a_294.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_295.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_296.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_297.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_299.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_300.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_301.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_303.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_304.png)

不，它不能。例如，流媒体视频目前对于许多LLM来说并不流行，这只是刚刚开始出现的东西。我甚至无法想象未来可能会出现哪些我们尚未处理过的其他数据格式。很多时候，我们看到的只是文本和图像，或者已经预先存在的、被输入AI模型或LLM以进行解释并为我们输出信息的视频。

![](img/f3f18789ed9809b85e76f612bee9ca6a_305.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_306.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_307.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_309.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_310.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_311.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_312.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_313.png)

另一个重要点是，为模型提供格式正确的数据可以显著改善其模式匹配和响应准确性。一个很好的例子是JSON。JSON基本上是一种以文档化方式发送大量数据的方法，这些数据可以是某物的属性。假设你有一个混乱的JSON文档，缺少括号，字段周围的引号不一致，这对于普通人类来说都很难理解，当然对于模型来说也会非常困难，因为它也必须像人类一样解析、排序和理解信息。但如果你给它一个干净、格式正确的JSON文档，带有适当的空格、括号和引号，它将更容易消化这些信息，从而加快训练时间，并且对你来说成本效益更高，因为你没有使用那么多资源来解析混乱的数据。第三，它可能会减少幻觉的产生，或者减少模型反复思考“这个文档格式是否正确？这个文档和那个不一样，因为引号没了”的情况。模型会像我们一样感到困惑。

![](img/f3f18789ed9809b85e76f612bee9ca6a_314.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_315.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_316.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_317.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_318.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_319.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_320.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_322.png)

我认为，让人们相信LLM可以处理任何事情的原因之一，是这些AI驱动的助手。我认为它们正在做预处理。比如，如果你给它一个PDF，它可能会运行一个预处理步骤来提取信息或对其进行处理。当然，我不确定具体细节，因为我们并不真正了解内部情况，但有时确实有魔法发生。就像你提到的视频，我们知道ChatGPT的最新版本有视频功能，但它是如何处理视频流的呢？外面有技术，但我找不到任何开源的。所以，如果他们解决了这个问题，他们并没有与其他人分享。

![](img/f3f18789ed9809b85e76f612bee9ca6a_324.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_325.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_326.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_327.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_328.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_329.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_330.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_331.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_332.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_333.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_334.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_335.png)

**误解二：文本数据不需要预处理。**

![](img/f3f18789ed9809b85e76f612bee9ca6a_337.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_338.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_339.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_340.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_341.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_342.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_343.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_344.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_345.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_346.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_348.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_349.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_350.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_351.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_352.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_354.png)

我有一个很酷的例子。假设一个客户支持聊天机器人从数据库中提取一些信息，那只是一个文本字符串：“紧急！产品到货损坏。订购日期：2023年某月某日。订单号：XXXX。失望 😞😞😞。” 这对AI来说非常混乱。AI可能不知道如何分析情感，除非它连接了相关的代码库。对于AI来说，这可能只是一堆随机字符。它怎么知道“订购日期：XXXX”是一个日期，还是一堆数字和字符扔在一起？你真的不知道AI模型或AI背后的代码是如何解析这些东西的。相比之下，如果你看右边，你会看到一个更好的客户数据格式化方式，当然，你有字段，然后在另一侧有对应的值，这对模型来说更容易分解、理解和解释。

![](img/f3f18789ed9809b85e76f612bee9ca6a_355.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_356.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_358.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_359.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_360.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_361.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_362.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_363.png)

现实是：结构化、干净的文本数据能带来更可靠、更一致的生成式AI输出。模型不那么困惑，能更好地理解数据，因此有更高的机会返回有意义的内容，或者减少幻觉，因为它可能只是说“是的，这个人很高兴”，因为它不知道该怎么做，但也许AI被训练成不能告诉你它不知道，所以它必须回应点什么。

![](img/f3f18789ed9809b85e76f612bee9ca6a_365.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_366.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_367.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_368.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_369.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_370.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_371.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_373.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_374.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_375.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_376.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_377.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_378.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_379.png)

**误解三：我们可以直接把文档扔进RAG（检索增强生成）系统。**

![](img/f3f18789ed9809b85e76f612bee9ca6a_381.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_382.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_384.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_385.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_386.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_387.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_388.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_389.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_390.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_391.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_393.png)

很多人认为他们可以这样做，并且能完美工作，但你需要适当的预处理。假设你需要删除一些不相关的页眉，集中一些格式和内容结构，否则检索质量会大大受损。顺便说一下，如果你不知道RAG是什么，它基本上是将数据库连接到LLM，以便模型可以引用数据库中的任何专业信息。这是一种经济有效的方式，可以在不进行微调的情况下为模型提供更多数据，因为微调实际上相当昂贵。这也是使用客户特定信息定制模型的好方法。你可能会看到很多公司创建自己的内部聊天GPT，这样员工就可以输入公司数据并使用它，同时也能让模型保持最新状态。例如，如果你有2025年的HR政策，但你的模型是在2020年的数据上训练的，那么仅仅保留2020年的数据就不是一个好主意，尤其是HR部门需要它时，因为如果因旧信息而出错，可能会引发合规问题。你可以将更新的政策或信息放入向量数据库中，然后LLM现在就有了相关的信息可以提取并输出给用户。

![](img/f3f18789ed9809b85e76f612bee9ca6a_394.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_396.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_397.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_398.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_399.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_400.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_401.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_402.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_403.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_405.png)

再次强调，“垃圾进，垃圾出”的原则尤其相关。一个很好的例子是GitHub Copilot，它建议代码的能力不仅仅取决于它能够访问代码仓库，还取决于它所训练的数据必须是干净的、有良好文档记录的、格式正确的代码示例，以便理解如何帮助人们修复或构建代码、添加注释或修正间距等。Copilot作为一个完美的AI产品脱颖而出，其背后的数据使其成为今天的样子。

![](img/f3f18789ed9809b85e76f612bee9ca6a_407.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_408.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_409.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_410.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_411.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_412.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_413.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_414.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_415.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_416.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_417.png)

## 现实世界中的生成式AI失败案例 ⚠️

![](img/f3f18789ed9809b85e76f612bee9ca6a_418.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_420.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_422.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_423.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_424.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_425.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_426.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_427.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_428.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_429.png)

现在，让我们谈谈一些现实世界的例子，在这些例子中，数据预处理不足导致了生成式AI的失败。这些不仅仅是技术故障，它们还产生了真实的商业和社会影响。

![](img/f3f18789ed9809b85e76f612bee9ca6a_430.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_431.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_433.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_434.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_436.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_437.png)

**2014年亚马逊AI招聘工具**

![](img/f3f18789ed9809b85e76f612bee9ca6a_438.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_439.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_440.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_441.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_442.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_443.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_444.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_445.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_447.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_448.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_449.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_450.png)

2014年，亚马逊创建了一个AI招聘工具，旨在帮助筛选申请者的简历并对其进行评分。但它出现了一个大问题：它开始歧视女性。根本原因是该模型在一个数据集上进行了训练，该数据集包含了过去10年间男性提交的简历。训练数据没有经过预处理以去除性别标识或解决历史招聘偏见。如果简历中有“女子排球队”或“科技女性委员会”等关键词，它可能会被标记或视为不好的，仅仅因为训练数据中没有出现过“女性”这个关键词。该系统基本上从过去本身就有偏见的招聘决策中学习，并创造了一个自我强化的循环。这是在LLM出现之前（2014年），现在想象一下，如果人们只是把10年的简历扔进RAG系统或进行微调，说“去搞定它”，它可能也会犯同样严重的错误。

![](img/f3f18789ed9809b85e76f612bee9ca6a_452.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_453.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_454.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_455.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_456.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_457.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_458.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_459.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_460.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_461.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_462.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_463.png)

**2016年微软Tay聊天机器人**

![](img/f3f18789ed9809b85e76f612bee9ca6a_465.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_466.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_467.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_468.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_469.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_470.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_471.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_472.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_473.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_474.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_475.png)

2016年，微软有一个名为Tay的对话理解聊天机器人。它被设计用来从Twitter上的互动中学习词汇和句法，模仿青少女的语言，使用俚语等。但它发生了转变，因为它开始用极其不恰当的想法回复人们的帖子。原因是有人提到了4chan论坛（一个以恶搞和捣乱者闻名的论坛），并鼓励用户用种族主义、厌女症和反犹太主义的语言“毒害”这个机器人，这样机器人就能学会并开始发布类似的内容。它确实学会了那种语言并模仿了它。是的，没有对传入的Twitter数据进行预处理来过滤和验证，也没有清理那些原始的社交媒体数据。我敢肯定他们一开始有一些检查和过滤器，但他们可能没有预料到事情会发展到那一步。这也是为什么预处理需要跳出思维定式，并需要一个能够思考AI可能遇到的所有不同用例或边缘情况的多元化团队。

![](img/f3f18789ed9809b85e76f612bee9ca6a_477.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_478.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_479.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_480.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_481.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_482.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_483.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_484.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_485.png)

**Meta的Galactica AI**

![](img/f3f18789ed9809b85e76f612bee9ca6a_486.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_487.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_488.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_490.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_491.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_492.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_493.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_494.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_495.png)

Meta的Galactica AI理论上应该很棒。它是一个AI机器人，可以总结学术论文、解决数学问题、为维基百科生成文章、编写科学代码等。但这个模型产生了太多幻觉，主要是因为训练数据包含了未经证实的科学内容，并且缺乏关于数据来源（共享的科学文章）的标注。此外，还使用了同行评审和非同行评审的文章。因此，它在三天内就被下架了，因为它生成了一些令人信服但虚假的科学论文，在学术界引起了强烈反响。

![](img/f3f18789ed9809b85e76f612bee9ca6a_497.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_498.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_499.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_500.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_501.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_502.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_503.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_504.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_505.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_506.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_507.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_509.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_511.png)

## 数据质量 📊

![](img/f3f18789ed9809b85e76f612bee9ca6a_513.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_514.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_516.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_517.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_519.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_520.png)

接下来，我们将讨论数据质量，这非常重要。无论你的数据是用于AI、数据分析还是任何其他职位，你都应该始终对你使用的数据集进行评估，因为数据总是相关的。高质量的数据是成功AI计划的基础。

![](img/f3f18789ed9809b85e76f612bee9ca6a_522.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_523.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_524.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_525.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_526.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_527.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_528.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_529.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_530.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_531.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_532.png)

让我们分解一些数据质量的组成部分：

![](img/f3f18789ed9809b85e76f612bee9ca6a_534.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_535.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_536.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_537.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_538.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_539.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_540.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_541.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_542.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_543.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_544.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_545.png)

1.  **准确性**：数据正确反映现实世界的状况。例如，假设你正在开发一个餐厅菜单聊天机器人（不确定这是否是最佳用例）。如果菜单价格基于2020年的训练数据，而现在是2025年，如果有人查看菜单价格并期望更便宜的价格，但实际价格因通货膨胀而上涨，他们会非常失望。输出基于训练数据是准确的，但基于当前价格则不准确。
2.  **完整性**：所有必要的数据点都存在，尤其是与你要解决的问题相关的数据点。例如，假设你有一个医学知识聊天机器人，它连接到一个RAG或向量数据库，包含了许多关于药物的信息（化学成分、用途），但没有包含关于药物相互作用（如服用频率或副作用）的信息。如果有人用它来学习如何服药，而它没有显示任何关于如何服用的信息，可能会导致潜在的法律诉讼。
3.  **一致性**：标准化你的数据，确保数据在不同数据集之间不矛盾。再次回到客户服务AI机器人的例子，假设它用于一家零售公司，帮助提供产品信息。在一个数据集中，产品被引用为“夹克”（可能是库存数据集），但在营销团队的数据中，它被宣传为“毛衣”。如果一个人访问网站并问“最酷的夹克是什么”，AI不确定应该说“这是夹克”还是“这是毛衣”，因为它被标记为两者。它可能产生幻觉，说“这里没有夹克”，或者说“这是夹克毛衣”之类的话。这是一个非常温和的例子，想象一下如果这是在医疗保健领域，医生需要查看处方列表，而某些信息缺失，他们正在处理危及生命的状况。
4.  **及时性**：确保你的数据是最新的和相关的。例如，一个法律合规AI助手使用训练数据，但数据基于2023年之前的政策法规。你需要确保连接到RAG以持续获取最新的法规。还要确保数据是相关的。如果你有一个HR助手，但它包含IT安全协议的信息，这就不相关了，因为它只涉及HR。现在，模型可能会对HR政策和IT政策感到困惑，因为它们可能有相同的缩写但需要完全不同的东西。

![](img/f3f18789ed9809b85e76f612bee9ca6a_547.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_548.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_549.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_550.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_551.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_552.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_554.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_556.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_557.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_559.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_560.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_561.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_562.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_563.png)

所有这些数据质量检查或组成部分在验证数据集时都至关重要。

![](img/f3f18789ed9809b85e76f612bee9ca6a_565.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_566.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_567.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_568.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_569.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_570.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_571.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_572.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_573.png)

## 数据准备最佳实践 🛠️

![](img/f3f18789ed9809b85e76f612bee9ca6a_574.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_575.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_576.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_578.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_579.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_580.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_581.png)

现在，让我们深入了解一些数据准备的最佳实践。这些是为生成式AI应用准备数据的基本步骤。可以把它想象成一个食谱，每一步都建立在前一步的基础上，以创建一个强大可靠的AI系统。

![](img/f3f18789ed9809b85e76f612bee9ca6a_583.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_584.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_585.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_586.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_587.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_588.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_589.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_590.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_591.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_592.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_593.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_594.png)

**1. 数据清洗**

![](img/f3f18789ed9809b85e76f612bee9ca6a_596.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_597.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_598.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_599.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_600.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_601.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_602.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_603.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_604.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_605.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_606.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_607.png)

这就像在烹饪前整理你的食材。首先，你可能想删除重复项，就像你不会想在布朗尼食谱中加两次糖和香料一样，你也不希望有任何相关的客户信息或任何类型的政策信息出现重复。删除重复项也可以在你放入数据时节省空间。其次，处理缺失数据，这类似于决定当你缺少一种食材时该怎么办。你是想用其他东西替代它，还是完全跳过它？这再次取决于数据点，你根据它来决定应该使用哪种技术。第三，去除噪声。可以把它想象成在制作食谱时过滤掉你不想要的调味料或食材残渣。如果你在做布朗尼，你可能不想要大蒜粉，那听起来不会做出最好的风味。最后，处理异常值。这些是可能扰乱整个系统的极端值。例如，如果你处理的是天气数据，假设你得到一个随机数据点显示1000华氏度，而你的典型范围是25到100度，你可能会把它扔掉，因为那是异常值。

![](img/f3f18789ed9809b85e76f612bee9ca6a_609.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_610.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_611.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_612.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_614.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_615.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_616.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_618.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_619.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_620.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_621.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_622.png)

**2. 数据预处理**

![](img/f3f18789ed9809b85e76f612bee9ca6a_624.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_625.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_626.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_627.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_628.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_629.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_630.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_631.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_632.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_633.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_634.png)

这是将干净数据转换为AI可以理解的格式的地方。你需要对分类值进行编码。分类值基本上是我们理解为短语的任何类型的标签或属性，但AI将所有东西都理解为数字。例如，如果你有“热”、“温”和“冷”作为某些东西的标签，你会想把它们转换成数字。你还需要缩放数值数据，以确保所有测量值具有可比性。可以想象一下，将不同测量系统（如美制、公制、英制）的食谱转换成一个标准的公制测量。我们还会讨论将数据分割成训练集、验证集和测试集，这对于确保AI能够很好地泛化到新情况至关重要。

![](img/f3f18789ed9809b85e76f612bee9ca6a_635.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_637.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_638.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_640.png)

**3. 特征工程**

![](img/f3f18789ed9809b85e76f612bee9ca6a_641.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_642.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_643.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_644.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_645.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_646.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_647.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_648.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_649.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_650.png)

这是魔法发生的地方。你从现有数据中创建新的、有意义的特征。例如，与其使用原始温度数据，你可能会创建诸如“24小时内温度变化”或“与季节平均值的偏差”等特征。这一切都基于你试图解决的业务结果以及存在的数据点。更详细地说，特征是一个可测量的变量或属性，或者是数据点的特征，用作机器学习算法的输入。例如，房价数据集可能包含以下特征：卧室数量、平方英尺、位置和房产年龄。

![](img/f3f18789ed9809b85e76f612bee9ca6a_652.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_653.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_654.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_655.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_656.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_657.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_658.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_659.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_660.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_661.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_662.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_664.png)

**4. 数据标注**

![](img/f3f18789ed9809b85e76f612bee9ca6a_665.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_666.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_667.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_668.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_669.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_670.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_671.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_673.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_674.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_675.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_676.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_677.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_678.png)

这是为数据添加上下文，就像为食谱添加烹饪说明一样。你告诉AI每一块数据的含义以及应该如何解释。这对于当你回顾数据集并试图理解为什么将其分类或标记，并将其与其他数据放在特定批次中也很有用。

![](img/f3f18789ed9809b85e76f612bee9ca6a_680.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_681.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_682.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_683.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_684.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_685.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_686.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_687.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_688.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_689.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_690.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_691.png)

**5. 数据隐私**

![](img/f3f18789ed9809b85e76f612bee9ca6a_693.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_694.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_695.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_696.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_697.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_698.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_699.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_700.png)

这同样至关重要。你需要确保在保护数据或敏感信息的同时，仍能保持数据用于AI训练的效用。

![](img/f3f18789ed9809b85e76f612bee9ca6a_702.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_703.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_704.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_705.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_706.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_707.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_708.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_709.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_710.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_711.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_712.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_713.png)

在接下来的课程中，我们将使用一个真实世界的例子——构建一个AI日语口语教练，来深入探讨这些步骤。它将帮助人们完善他们的日语发音。基拉，你的日语好吗？

![](img/f3f18789ed9809b85e76f612bee9ca6a_715.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_716.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_717.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_718.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_719.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_720.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_721.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_722.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_723.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_724.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_725.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_726.png)

不好。我唯一能说的是“Watashi wa”（我是），那是因为看了《海贼王》。你呢？

![](img/f3f18789ed9809b85e76f612bee9ca6a_728.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_730.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_731.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_732.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_733.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_734.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_735.png)

我知道的多一点，但考虑到我花在学日语上的时间和金钱，我应该进步得更快一些，不过没关系。

![](img/f3f18789ed9809b85e76f612bee9ca6a_736.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_737.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_738.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_739.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_740.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_742.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_743.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_744.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_745.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_746.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_747.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_748.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_750.png)

你总有一天会学会的，没关系。我也在非常缓慢地学习法语，所以我懂。

![](img/f3f18789ed9809b85e76f612bee9ca6a_752.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_753.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_754.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_755.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_756.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_757.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_758.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_759.png)

## 数据清洗实践示例：日语口语教练 🗣️

![](img/f3f18789ed9809b85e76f612bee9ca6a_760.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_761.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_762.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_763.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_765.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_766.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_767.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_768.png)

接下来，让我们在我们的日语口语教练应用背景下谈谈数据清洗。

![](img/f3f18789ed9809b85e76f612bee9ca6a_770.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_771.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_772.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_773.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_774.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_775.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_776.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_777.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_778.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_779.png)

**1. 删除重复项**

![](img/f3f18789ed9809b85e76f612bee9ca6a_781.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_782.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_783.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_784.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_785.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_786.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_787.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_788.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_789.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_790.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_791.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_792.png)

假设你有同一个学生对同一短语的多次录音，并且所有文件都命名为“10月12日的音频文件”。你有三种处理方式：
*   你可以按最新的音频时间戳过滤掉所有录音。如果你不想保存状态（即不想查看这个人发音随时间改善的情况），这可能会有帮助，也许你只是处于应用的MVP阶段。
*   创建唯一的文件名。我总是支持这一点，尤其是从人类的角度来看，它有助于人们理解。例如，你可以使用学生ID、短语和时间戳，以便更好地查询特定的音频文件。
*   保留录音日志以供未来故障排除和分析。这非常重要，也符合我的DevOps背景。有时会出现错误，你需要查看日志来理解导致AI模型错误的确切原因。你甚至可以使用这些信息在下一次需要微调或更新数据验证步骤时使用。

![](img/f3f18789ed9809b85e76f612bee9ca6a_794.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_795.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_796.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_797.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_798.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_799.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_800.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_801.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_802.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_803.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_804.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_805.png)

**2. 处理缺失数据**

![](img/f3f18789ed9809b85e76f612bee9ca6a_807.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_809.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_810.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_811.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_812.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_813.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_814.png)

![](img/f3f18789ed9809b85e76f612bee9ca6a_815.png)

假设你正在收集学生的多个数据点，可能是学生ID、短语、原生音频（单词的正确发音）、学生音频（学生在特定时间发音的版本）和难度级别。但你注意到有些音频缺失了某些数据，可能一个缺失了难度级别，另一个缺失了音频。以下是一些处理方法：
*   如果缺少音频，你必须要求重新录制。
*   如果缺少难度级别，你可以使用其他学生的平均难度。例如，如果95%的学生发现某个特定短语非常难，那么可以安全地假设那个人可能也这么认为。
*   如果缺少某些值（例如学生ID），你可能需要过滤掉该记录，尤其是如果你的应用是基于个人的。如果我登录，我只想看到我的信息。如果我有音频但没有学生ID，你不能安全地假设它属于我，所以最好