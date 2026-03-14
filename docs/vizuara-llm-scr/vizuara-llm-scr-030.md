# 30：加载OpenAI GPT-2预训练权重

![](img/53915cdb9afe31a7e9d0015987ec20ce_0.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_2.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_3.png)

## 概述
在本节课中，我们将学习如何将OpenAI公开发布的GPT-2预训练权重加载到我们之前亲手构建的GPT模型架构中。这将使我们之前训练出的、生成文本不连贯的模型，能够生成有意义的句子。本节课是我们之前所有辛勤工作的成果展示。

## 下载与准备权重文件

![](img/53915cdb9afe31a7e9d0015987ec20ce_5.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_7.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_8.png)

上一节我们介绍了如何构建GPT模型架构，本节中我们来看看如何为其注入强大的“知识”——预训练权重。

首先，我们需要下载OpenAI GPT-2的权重文件。这些文件最初由TensorFlow保存，而我们的代码基于PyTorch，因此需要安装TensorFlow来读取这些文件。同时，我们安装`tqdm`库来跟踪下载进度。

以下是需要安装的库及其版本要求：
```python
# 安装必要的库
# TensorFlow >= 2.15
# tqdm >= 4.66
```

下载过程通过一个自定义函数`download_and_load_gpt2`完成。该函数主要执行两个任务：
1.  从指定URL（如Kaggle）下载包含模型权重的七个核心文件。
2.  调用辅助函数`load_gpt2_params_from_tf_checkpoint`，从下载的文件中提取参数，并将其整理成一个结构化的参数字典。

![](img/53915cdb9afe31a7e9d0015987ec20ce_10.png)

## 理解下载的文件与参数结构

在深入代码之前，理解下载的文件和最终参数字典的结构至关重要。

以下是下载的七个核心文件及其作用：
*   **`checkpoint`**：指向存储模型权重的主要文件路径（通常是`model.ckpt`）。
*   **`encoder.json`**：词汇表文件，包含所有50257个token及其对应的ID。
*   **`vocab.bpe`**：字节对编码（BPE）合并规则列表，用于子词分词。
*   **`hparams.json`**：模型超参数设置文件，包含词汇表大小、上下文长度、嵌入维度等关键配置。

`load_gpt2_params_from_tf_checkpoint`函数的核心目标是构建一个名为`params`的字典，该字典包含以下五个关键部分，对应GPT模型架构中的可训练参数：

1.  **`wte`** (Word Token Embeddings)：词嵌入矩阵，形状为 `[50257, 768]`。
2.  **`wpe`** (Word Position Embeddings)：位置嵌入矩阵，形状为 `[1024, 768]`。
3.  **`blocks`**：包含所有Transformer块参数的嵌套字典。每个块内又包含：
    *   **注意力层** (`attn`): 查询、键、值融合矩阵的权重和偏置。
    *   **前馈神经网络** (`mlp`): 全连接层和投影层的权重和偏置。
    *   **层归一化** (`ln_1`, `ln_2`): 缩放和偏移参数。
4.  **`g`** (Final Norm Scale)：最终层归一化的缩放参数。
5.  **`b`** (Final Norm Shift)：最终层归一化的偏移参数。

这个参数字典的结构完全映射了我们自建的GPT模型类中的每一个可训练组件。

![](img/53915cdb9afe31a7e9d0015987ec20ce_12.png)

## 配置模型与加载权重

![](img/53915cdb9afe31a7e9d0015987ec20ce_14.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_16.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_17.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_18.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_19.png)

理解了参数结构后，我们需要调整模型配置以匹配GPT-2的设置，然后将下载的权重加载到我们的模型实例中。

![](img/53915cdb9afe31a7e9d0015987ec20ce_20.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_21.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_23.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_24.png)

首先，更新模型配置。我们之前使用的配置与GPT-2基本一致，但有两个关键区别需要修正：
1.  将上下文长度从256改为GPT-2使用的1024。
2.  将注意力机制中查询、键、值的偏置项启用（设为`True`），以匹配GPT-2的原始设置。

代码示例如下：
```python
# 更新模型配置以匹配GPT-2
new_config = {**GPT_CONFIG_124M, ‘context_length’: 1024, ‘qkv_bias’: True}
model = GPT(new_config)
```

![](img/53915cdb9afe31a7e9d0015987ec20ce_26.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_27.png)

接下来，执行核心的权重加载函数`load_weights_into_gpt`。这个过程是逐层、逐参数地将`params`字典中的值赋值给我们模型中的对应层。

![](img/53915cdb9afe31a7e9d0015987ec20ce_29.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_30.png)

以下是加载过程中的关键步骤：
*   **注意力层**：从`params[‘blocks’][…][‘attn’][‘c_attn’][‘w’]`获取融合的QKV权重矩阵，将其分割后分别赋值给模型注意力层的`query`、`key`、`value`权重。偏置项同理。
*   **前馈神经网络**：分别加载全连接层(`c_fc`)和投影层(`c_proj`)的权重和偏置。
*   **层归一化**：加载每个Transformer块内两个层归一化层(`ln_1`, `ln_2`)以及最终层归一化的缩放(`g`)和偏移(`b`)参数。
*   **权重绑定**：GPT-2使用了权重绑定技术，即输出层的权重与输入词嵌入层(`wte`)共享。因此，我们不需要为输出层单独加载权重。

通过一个自定义的`assign`函数确保加载的权重形状与模型层期望的形状完全匹配，否则会报错。

## 测试加载后的模型

最激动人心的环节是测试我们加载了GPT-2权重的自建模型。我们将使用相同的输入和生成函数，观察输出文本的质量变化。

我们使用输入“every effort moves you”，并设置生成参数（如`max_new_tokens=25`, `temperature=1.5`, `top_k=50`）进行文本生成。

以下是测试结果的对比：
*   **加载权重前**：模型生成的文本不连贯，没有逻辑意义。
*   **加载GPT-2权重后**：模型生成了如“every effort moves you toward finding an ideal new way to practice something, what makes us want to be on top of that.”这样语义连贯、语法正确的句子。

这证明了我们成功地将预训练知识注入了模型。现在，我们可以像研究者一样自由探索：
*   调整生成参数（如`temperature`, `top_k`），观察输出随机性和质量的变化。
*   修改模型架构（如Transformer块的数量、注意力头数），进行实验。
*   更改优化器设置，进行微调实验。

![](img/53915cdb9afe31a7e9d0015987ec20ce_32.png)

![](img/53915cdb9afe31a7e9d0015987ec20ce_34.png)

## 总结
本节课中，我们一起完成了从下载OpenAI GPT-2预训练权重到将其成功加载到我们自建GPT模型的全过程。我们深入理解了权重文件的结构、参数字典的组织方式，以及权重与模型层之间的映射关系。最终，我们验证了加载权重后的模型能够生成高质量的文本，这标志着我们不仅理解了GPT的理论架构，也掌握了让其“工作”起来的实践能力。这为我们后续进行模型微调和特定应用开发奠定了坚实的基础。在接下来的课程中，我们将以此为基础，探索大语言模型的微调技术。