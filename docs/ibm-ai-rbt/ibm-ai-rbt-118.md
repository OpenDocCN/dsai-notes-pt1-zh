# 118：集成Watson Assistant与Discovery 🤖➡️📚

![](img/c7ad889a0733d3b36efc86e0f5eea79d_1.png)

在本节课中，我们将要学习如何将Watson Assistant与Watson Discovery服务集成，从而构建一个能够回答更广泛、更具体问题的智能聊天机器人。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_3.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_4.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_5.png)

---

![](img/c7ad889a0733d3b36efc86e0f5eea79d_6.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_7.png)

Watson Discovery非常出色，但您可能想知道它与聊天机器人有何关联。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_9.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_10.png)

您可能知道，Watson Assistant允许我们解释和分类用户的输入，然后根据我们在对话中定义的规则，用一些预定义的响应进行回复。这种方法对于处理常见问题（即图表中的“短头”部分）非常有效。更具挑战性的是处理用户可能提出的“长尾”问题。如果我们的聊天机器人也能回答这些非常具体的问题，那将非常理想。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_12.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_13.png)

大多数企业都拥有现有的知识库，这些知识库通常可供客户和支持他们的客服人员使用。这些网页和/或PDF文档往往包含丰富的信息。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_15.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_16.png)

手动提取这些文档中所有可能的答案，并将它们准备为对话中成千上万种可能的响应，这是不可行的。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_18.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_19.png)

这正是Watson Discovery发挥作用的地方。通过连接Watson Assistant和Watson Discovery，我们可以让Watson Assistant处理我们预定义的查询，同时利用Watson Discovery的查询能力来处理长尾问题。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_20.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_22.png)

当无法响应用户时，Watson Assistant会将用户查询传递给Watson Discovery。Discovery能够很好地处理用自然语言表达的查询，并允许我们从已定义的知识库集合中检索相关的答案。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_24.png)

最后，Watson Assistant会将检索到的知识库答案传递给用户。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_25.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_26.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_28.png)

换句话说，通过集成Watson Assistant和Watson Discovery，我们将能够创建更智能、更有用的聊天机器人。Watson Assistant仍将处理与用户的整个交互，但会将长尾问题外包给Discovery处理。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_30.png)

好消息是，Watson Assistant和Watson Discovery可以轻松集成，无需任何编程。就像您为助手添加对话技能一样，您可以添加一个搜索技能，将您的聊天机器人与Discovery连接起来，从而轻松查询您在那里的集合。

坏消息是，这种便捷的集成仅适用于Plus或Premium计划的用户。如果您像大多数人在培训期间一样使用免费的Lite计划，您将无法添加搜索技能。不过，请不要担心，因为我们仍然可以通过API集成Watson Assistant和Watson Discovery。事实上，每个Watson服务都提供了一个API，可用于以编程方式访问该服务。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_32.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_33.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_34.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_35.png)

因此，我们可以直接从脚本或应用程序查询Watson Discovery集合。这一点，再加上Watson Assistant对话节点能够以编程方式调用API，将使两者能够进行通信。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_37.png)

从对话节点调用远程API的最简单方法实际上是使用云函数。云函数正如其名，它是一个可以执行某些代码并可以远程调用的函数。在我们的案例中，云函数将向Watson Discovery服务发出API请求并返回响应。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_39.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_40.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_42.png)

然后，Watson Assistant将利用该函数返回的值向用户发出最终响应。我喜欢这种方法，因为它非常灵活。您可能希望在某个时候将您的对话连接到不同的服务，了解如何集成不同的服务将会派上用场。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_44.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_45.png)

此外，您还将更加熟悉IBM Cloud Functions提供的无服务器计算模型。

在模块4中，您还将学习如何使用API集成Watson的语音转文本和文本转语音服务。

![](img/c7ad889a0733d3b36efc86e0f5eea79d_47.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_48.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_49.png)

---

![](img/c7ad889a0733d3b36efc86e0f5eea79d_51.png)

![](img/c7ad889a0733d3b36efc86e0f5eea79d_53.png)

本节课中，我们一起学习了如何通过API将Watson Assistant与Watson Discovery集成，以扩展聊天机器人的知识处理能力。我们了解了处理常见问题的“短头”与处理特定问题的“长尾”之间的区别，并掌握了利用云函数作为桥梁连接两个服务的基本原理。这种方法不仅增强了机器人的实用性，也为未来集成其他服务打下了基础。