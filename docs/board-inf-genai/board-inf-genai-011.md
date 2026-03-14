#  011：零样本与少样本学习 🧠

![](img/4d8636a8a41300111fb8db2f0dd757c8_0.png)

在本节课中，我们将学习两种高级提示工程技术：零样本学习和少样本学习。这两种技术能显著影响AI理解、生成和回应的方式，使其输出更准确、更有用。

## 零样本学习 🚀

上一节我们介绍了提示工程的基础知识，本节中我们来看看零样本学习。想象一下，你要求某人完成一项他们从未做过的任务，且不提供任何先例。你只给出一个提示，AI则完全依赖其预训练的知识来生成回应。

例如，你想将一条电影评论分类为正面或负面。

**提示示例：**
```
classify this movie review as positive or negative.
😊 I couldn't wait to see this film in the end.
```

这里我们没有提供任何例子，但AI利用其对情感分析的理解，判断出这是一条负面评论。零样本提示适用于模型能力范围内的直接任务，其构建方法很简单：清晰描述你的需求。

以下是构建有效零样本提示的要点：

*   **明确任务**：直接说明你想要什么。
*   **指定格式与限制**：包括输出格式和任何约束条件。

**提示示例对比：**
*   **效果不佳**：`help with this text`
*   **效果更好**：`rewrite this paragraph to make it more engaging for a teenage audience while maintaining all the key information`

## 少样本学习 📚

现在，如果你希望AI遵循特定的模式或风格，该怎么办？这就是少样本学习的用武之地。这种方法不完全依赖预训练，而是在要求AI生成回应前，先向其展示任务示例。

可以这样理解：如果你要教某人区分正式与非正式语言，你可能会先给出一些例子。

**少样本提示示例：**
```
classify these sentences as formal or informal.

First one: I am pleased to inform you that your application has been accepted. (This is a formal sentence.)

Hey, just letting you know you got the stuff. (This is an informal sentence.)

Now classify: We regret to inform you that your submission was not selected.
```

通过看到这些例子，AI学会了其中的模式并能正确应用。少样本学习在以下情况特别有用：

*   需要AI遵循特定模式或格式。
*   希望展示特定风格或语调。
*   任务本身模糊，没有例子难以界定。
*   需要在多次请求中获得一致的输出。

你提供的示例会建立一个模式，AI将尝试延续这个模式。这些示例的质量和清晰度会显著影响结果。即使只有几个例子，也能极大地改善AI的表现。

## 总结与展望 🔮

本节课中，我们一起学习了零样本与少样本学习。零样本学习让AI直接根据指令完成任务，而少样本学习则通过提供示例来引导AI遵循特定模式。

![](img/4d8636a8a41300111fb8db2f0dd757c8_2.png)

现在我们已经掌握了这两种技术，接下来就可以探索更具结构性的方法，例如思维链提示。这些方法能帮助分解复杂的推理过程，让AI变得更“聪明”。我们下节课再见。