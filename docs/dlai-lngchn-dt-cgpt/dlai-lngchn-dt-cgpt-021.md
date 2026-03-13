# 021：3. 第二篇 - RAG 指标三元组(RAG Triad of metrics) 📊

![](img/6f87d1fda84232c6f03ab28d46ab447f_0.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_0.png)

在本节课中，我们将深入探讨如何评估RAG（检索增强生成）应用。我们将介绍RAG指标三元组（RAG Triad）这一核心概念，它包含三个主要评估维度：**答案相关性**、**上下文相关性**和**基于事实的答案无关性（事实性）**。这些是使用TruLens等可扩展反馈函数框架进行程序化评估的示例。我们还将学习如何为任何非结构化语料库合成生成评估数据集。

![](img/6f87d1fda84232c6f03ab28d46ab447f_2.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_4.png)

现在，我们将通过一个笔记本示例，使用TruLens来实践RAG三元组（答案相关性、上下文相关性和事实性），以理解如何检测模型幻觉。

![](img/6f87d1fda84232c6f03ab28d46ab447f_2.png)

## 环境设置与初始化

到这一步，假设你已经安装了TruLens和LlamaIndex。

![](img/6f87d1fda84232c6f03ab28d46ab447f_4.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_6.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_8.png)

第一步是设置OpenAI API密钥。该密钥将用于TruLens的评估步骤。以下是设置代码片段：

```python
import os
os.environ["OPENAI_API_KEY"] = "your-api-key-here"
```

![](img/6f87d1fda84232c6f03ab28d46ab447f_10.png)

现在我们已经设置好了OpenAI密钥。

![](img/6f87d1fda84232c6f03ab28d46ab447f_12.png)

## 构建LlamaIndex查询引擎

![](img/6f87d1fda84232c6f03ab28d46ab447f_14.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_16.png)

接下来，我们将快速回顾使用LlamaIndex构建查询引擎的过程。这部分内容在第一课中已有详细介绍，我们将在此基础上进行。

![](img/6f87d1fda84232c6f03ab28d46ab447f_18.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_19.png)

首先，我们需要设置一个TruLens对象，用于记录和评估。

```python
from trulens_eval import Tru

tru = Tru()
tru.reset_database() # 重置数据库，用于记录后续的提示、响应和评估结果
```

![](img/6f87d1fda84232c6f03ab28d46ab447f_21.png)

现在，让我们设置LlamaIndex的文档读取器。以下代码从指定目录读取一个PDF文档（例如，一篇由吴恩达撰写的关于AI职业发展的文章），并将其加载为文档对象。

![](img/6f87d1fda84232c6f03ab28d46ab447f_23.png)

```python
from llama_index.core import SimpleDirectoryReader

documents = SimpleDirectoryReader("./data").load_data()
```

![](img/6f87d1fda84232c6f03ab28d46ab447f_25.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_6.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_27.png)

下一步是将所有页面内容合并为一个大文档，而不是默认的每页一个文档。

![](img/6f87d1fda84232c6f03ab28d46ab447f_29.png)

```python
from llama_index.core.node_parser import SentenceSplitter

text_parser = SentenceSplitter()
text_chunks = []
doc_texts = [doc.text for doc in documents]
for text in doc_texts:
    chunks = text_parser.split_text(text)
    text_chunks.extend(chunks)
```

![](img/6f87d1fda84232c6f03ab28d46ab447f_8.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_31.png)

接着，我们设置句子索引。这里使用OpenAI的GPT-3.5 Turbo作为LLM，设置温度为0，用于RAG的生成步骤。嵌入模型设置为`bge-small` v1.5。

```python
from llama_index.core import VectorStoreIndex, ServiceContext
from llama_index.llms.openai import OpenAI
from llama_index.embeddings.huggingface import HuggingFaceEmbedding

llm = OpenAI(model="gpt-3.5-turbo", temperature=0)
embed_model = HuggingFaceEmbedding(model_name="BAAI/bge-small-en-v1.5")
service_context = ServiceContext.from_defaults(llm=llm, embed_model=embed_model)

![](img/6f87d1fda84232c6f03ab28d46ab447f_33.png)

index = VectorStoreIndex.from_documents(documents, service_context=service_context)
```

![](img/6f87d1fda84232c6f03ab28d46ab447f_35.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_10.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_37.png)

然后，我们设置句子窗口查询引擎，这将用于后续的高级RAG应用检索。

```python
from llama_index.core.indices.postprocessor import SentenceWindowNodePostprocessor

![](img/6f87d1fda84232c6f03ab28d46ab447f_39.png)

postprocessor = SentenceWindowNodePostprocessor.from_defaults(window_size=3)
query_engine = index.as_query_engine(node_postprocessors=[postprocessor])
```

![](img/6f87d1fda84232c6f03ab28d46ab447f_41.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_12.png)

现在，基于句子窗口的RAG查询引擎已经设置完毕。让我们通过提问来测试其效果。

![](img/6f87d1fda84232c6f03ab28d46ab447f_43.png)

```python
response = query_engine.query("你如何创建你的AI作品集？")
print(response)
```

![](img/6f87d1fda84232c6f03ab28d46ab447f_14.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_45.png)

这将返回一个响应对象，其中包含LLM的最终回答、检索到的上下文片段以及一些元数据。

![](img/6f87d1fda84232c6f03ab28d46ab447f_16.png)

让我们看看最终响应的样子。

![](img/6f87d1fda84232c6f03ab28d46ab447f_18.png)
![](img/6f87d1fda84232c6f03ab28d46ab447f_19.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_47.png)

可以看到，基于句子窗口的RAG对问题“如何创建你的AI作品集”产生了一个表面上相当不错的回答。稍后，我们将使用RAG三元组来评估此类回答，以建立信心并识别潜在的失败模式。

![](img/6f87d1fda84232c6f03ab28d46ab447f_49.png)

## 引入RAG三元组进行评估

![](img/6f87d1fda84232c6f03ab28d46ab447f_51.png)

上一节我们成功构建了一个RAG应用。本节中，我们将看看如何使用RAG三元组来评估它。

在开始之前，我们先进行一些清理工作。

![](img/6f87d1fda84232c6f03ab28d46ab447f_53.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_21.png)

第一步是运行以下代码片段，从笔记本内部启动TruLens的Streamlit仪表板，以便可视化评估结果。

```python
tru.run_dashboard()
```

![](img/6f87d1fda84232c6f03ab28d46ab447f_55.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_57.png)

我们初始化默认的评估提供者为OpenAI GPT-3.5 Turbo，它将用于实现不同的反馈函数（评估指标），如上下文相关性、答案相关性和事实性。

![](img/6f87d1fda84232c6f03ab28d46ab447f_59.png)

现在，让我们深入探讨RAG三元组的每一项评估。我们将在幻灯片讲解和笔记本代码之间切换，以提供完整的上下文。

![](img/6f87d1fda84232c6f03ab28d46ab447f_61.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_25.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_63.png)

### 1. 答案相关性 (Answer Relevance)

首先，我们讨论答案相关性。

![](img/6f87d1fda84232c6f03ab28d46ab447f_65.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_66.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_27.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_68.png)

答案相关性检查的是：**最终的响应是否与用户提出的查询相关**。

![](img/6f87d1fda84232c6f03ab28d46ab447f_70.png)

为了给你一个具体的例子：

![](img/6f87d1fda84232c6f03ab28d46ab447f_29.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_72.png)

假设用户问题是：“利他主义如何有益于构建职业生涯？”。RAG应用给出了一个回答。答案相关性评估会产生两个输出：
1.  **得分**：一个0到1之间的分数，表示相关性程度。例如，高度相关的答案可能得0.9分。
2.  **理由**：支持该得分的思考链或证据。例如，评估模型可能指出答案中的哪些部分直接回应了问题。

我想借此机会介绍**反馈函数**这个抽象概念。答案相关性就是反馈函数的一个具体例子。更一般地说，反馈函数在审查LLM应用的输入、输出和中间结果后，会给出一个0到1的分数。

让我们看看反馈函数的结构，以答案相关性为例：

![](img/6f87d1fda84232c6f03ab28d46ab447f_74.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_31.png)

1.  **提供者**：在这个例子中，我们使用OpenAI的LLM来实现反馈函数。请注意，反馈函数不一定非要用LLM，也可以用BERT模型或其他机制实现。
2.  **函数实现**：利用该提供者，我们实现一个具体的反馈函数，这里就是“相关性”函数。我们给它一个人类可读的名字（如“答案相关性”），这个名字会显示在评估仪表板上。
3.  **输入**：对于这个特定的反馈函数，它接收**用户查询**和RAG应用产生的**最终答案**作为输入。
4.  **输出**：给定用户问题和最终答案，这个反馈函数会使用LLM提供者（如OpenAI GPT-3.5）来计算答案的相关性得分，并提供支持该得分的理由。

![](img/6f87d1fda84232c6f03ab28d46ab447f_33.png)

现在，让我们回到笔记本，看看如何在代码中定义答案相关性反馈函数。

```python
from trulens_eval import Feedback, Select
from trulens_eval.feedback import Groundedness
from trulens_eval.feedback.provider.openai import OpenAI as TruLensOpenAI

# 初始化提供者
provider = TruLensOpenAI()

![](img/6f87d1fda84232c6f03ab28d46ab447f_76.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_78.png)

# 定义答案相关性反馈函数
f_answer_relevance = Feedback(provider.relevance_with_cot_reasoning, 
                             name="Answer Relevance"
                            ).on_input_output()
# `on_input_output()` 表示该函数接收输入（提示）和输出（响应）
```

![](img/6f87d1fda84232c6f03ab28d46ab447f_80.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_35.png)
![](img/6f87d1fda84232c6f03ab28d46ab447f_37.png)
![](img/6f87d1fda84232c6f03ab28d46ab447f_39.png)

在笔记本的后续部分，我们将看到如何将此反馈函数应用到一组记录上，获取每个答案的相关性评估分数及其理由。

![](img/6f87d1fda84232c6f03ab28d46ab447f_82.png)

### 2. 上下文相关性 (Context Relevance)

![](img/6f87d1fda84232c6f03ab28d46ab447f_84.png)

接下来我们将深入探讨的反馈函数是**上下文相关性**。

![](img/6f87d1fda84232c6f03ab28d46ab447f_86.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_41.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_88.png)

上下文相关性检查的是：**给定查询，检索过程的质量如何**。我们会查看从向量数据库中检索出的每一段上下文，并评估这段上下文与所提问题的相关程度。

![](img/6f87d1fda84232c6f03ab28d46ab447f_90.png)

让我们看一个简单的例子：

![](img/6f87d1fda84232c6f03ab28d46ab447f_92.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_43.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_94.png)

用户问题是：“利他主义如何有益于构建职业生涯？”。假设检索到两段上下文。在评估上下文相关性后，每段上下文都会得到一个0到1的分数。例如，第一段上下文得0.5分，第二段得0.7分（被认为更相关）。最终报告的**平均上下文相关性得分**是这些片段得分的平均值。

![](img/6f87d1fda84232c6f03ab28d46ab447f_96.png)

现在，让我们看看上下文相关性反馈函数的结构：

![](img/6f87d1fda84232c6f03ab28d46ab447f_98.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_45.png)

其结构与答案相关性类似，但关键区别在于**输入**。除了用户输入（提示）外，这个反馈函数还需要访问**检索到的上下文片段**（这是RAG应用执行过程中的中间结果）。它为每个检索到的上下文片段返回一个相关性分数，然后聚合（平均）所有片段的分数以得到最终分数。

![](img/6f87d1fda84232c6f03ab28d46ab447f_100.png)

你会发现，答案相关性反馈函数只使用了原始输入（提示）和最终输出（响应）。而上下文相关性反馈函数则使用了用户输入和中间结果（检索到的上下文集）。这体现了反馈函数的强大之处：它可以通过评估LLM应用的输入、输出和中间结果来全面评估其质量。

![](img/6f87d1fda84232c6f03ab28d46ab447f_102.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_47.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_104.png)

现在，我们可以在代码中定义上下文相关性反馈函数。

![](img/6f87d1fda84232c6f03ab28d46ab447f_106.png)

```python
# 定义上下文相关性反馈函数
f_context_relevance = Feedback(provider.context_relevance_with_cot_reasoning,
                               name="Context Relevance"
                              ).on_input().on(Select.Record.app.combine_documents_chain._call.args.inputs.input_documents[:].page_content)
# `on_input()` 表示接收用户输入
# `on(...)` 指定了从应用记录中提取检索上下文的位置
```

![](img/6f87d1fda84232c6f03ab28d46ab447f_108.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_49.png)
![](img/6f87d1fda84232c6f03ab28d46ab447f_51.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_110.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_111.png)

这里还有一个变体，除了报告每个上下文的得分，还可以增强**思考链推理**，让评估的LLM不仅提供分数，还提供解释。这可以通过使用`context_relevance_with_cot_reasoning`方法实现。

![](img/6f87d1fda84232c6f03ab28d46ab447f_113.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_115.png)

例如，对于用户提示“利他主义如何有益于构建职业生涯？”和一段检索到的上下文，评估函数可能给出0.7分，并附上得此分的理由。

![](img/6f87d1fda84232c6f03ab28d46ab447f_117.png)

### 3. 事实性/基于事实的答案无关性 (Groundedness/Faithfulness)

![](img/6f87d1fda84232c6f03ab28d46ab447f_119.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_121.png)

RAG三元组的第三个指标是**事实性**（有时称为忠实性）。

![](img/6f87d1fda84232c6f03ab28d46ab447f_122.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_124.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_53.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_126.png)

事实性检查的是：**最终答案中的陈述在多大程度上得到了检索到的上下文的支持**。其目标是识别模型可能产生的“幻觉”——即答案中那些看似合理但缺乏检索依据的内容。

![](img/6f87d1fda84232c6f03ab28d46ab447f_128.png)

其反馈函数的定义在结构上与上下文相关性非常相似。

![](img/6f87d1fda84232c6f03ab28d46ab447f_130.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_132.png)

```python
# 定义事实性反馈函数（带思考链理由）
f_groundedness = Feedback(provider.groundedness_measure_with_cot_reasoning,
                          name="Groundedness"
                         ).on(Select.Record.app.combine_documents_chain._call.args.inputs.input_documents[:].page_content
                             ).on_output()
# 它接收检索到的上下文和最终输出作为输入
```

![](img/6f87d1fda84232c6f03ab28d46ab447f_133.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_55.png)
![](img/6f87d1fda84232c6f03ab28d46ab447f_57.png)
![](img/6f87d1fda84232c6f03ab28d46ab447f_59.png)
![](img/6f87d1fda84232c6f03ab28d46ab447f_61.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_135.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_137.png)

该函数为最终答案中的每一句话评估一个事实性分数，然后聚合平均，产生对完整回答的最终事实性得分。它访问的上下文与上下文相关性函数相同。

![](img/6f87d1fda84232c6f03ab28d46ab447f_138.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_140.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_63.png)

## 评估工作流程与迭代

![](img/6f87d1fda84232c6f03ab28d46ab447f_142.png)

现在我们已经设置好了所有三个反馈函数（答案相关性、上下文相关性、事实性）。我们还需要一个评估数据集来运行应用并查看其表现，以便在有机会时进行迭代和改进。

让我们看看评估和改进LLM应用的工作流程：

![](img/6f87d1fda84232c6f03ab28d46ab447f_144.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_65.png)
![](img/6f87d1fda84232c6f03ab28d46ab447f_66.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_146.png)

1.  **从基础RAG开始**：我们使用之前课程中介绍的基础LlamaIndex RAG，并用TruLens的RAG三元组对其进行评估。
2.  **识别失败模式**：我们可能会关注与**上下文大小**相关的失败模式。
3.  **迭代**：我们使用高级RAG技术（如LlamaIndex的句子窗口RAG）迭代这个基础RAG。
4.  **重新评估**：我们用TruLens的RAG三元组重新评估这个新的高级RAG，重点关注问题是否在特定指标上得到改进。

![](img/6f87d1fda84232c6f03ab28d46ab447f_68.png)
![](img/6f87d1fda84232c6f03ab28d46ab447f_70.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_148.png)

我们之所以关注上下文相关性，是因为一个常见的失败模式是上下文太小。一旦增加到某个程度，你可能会看到上下文相关性的改进。此外，当上下文相关性提高时，事实性通常也会提高，因为在生成步骤中，LLM有足够的相关上下文来产生总结。如果上下文不足，LLM倾向于利用其内部知识来填补空白，这会导致事实性丧失。

![](img/6f87d1fda84232c6f03ab28d46ab447f_150.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_152.png)

最后，我们可以尝试不同的窗口大小，以找出哪种窗口大小能产生最佳的评估指标。记住，如果窗口太小，可能没有足够的相关上下文来获得好的上下文相关性和事实性得分；如果窗口太大，不相关的上下文可能会渗入最终响应，导致答案相关性或事实性得分不高。

![](img/6f87d1fda84232c6f03ab28d46ab447f_154.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_72.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_156.png)

## 反馈函数的其他实现方式

![](img/6f87d1fda84232c6f03ab28d46ab447f_158.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_160.png)

我们已经介绍了三个使用LLM评估实现的反馈函数示例。我想指出，反馈函数可以以不同的方式实现。

![](img/6f87d1fda84232c6f03ab28d46ab447f_162.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_74.png)

*   **真实数据**：从业者通常从收集真实数据（Ground Truth）开始，这虽然成本高，但是一个良好的起点。
*   **人工评估**：人们也利用人类进行评估，这很有帮助，但在实践中难以扩展。
*   **LLM评估**：研究显示，LLM评估与人类评估的一致性大约在80%左右，这表明LLM评估可以与人类评估相媲美，并提供了程序化扩展评估的方法。

![](img/6f87d1fda84232c6f03ab28d46ab447f_164.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_76.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_166.png)

除了LLM评估，反馈函数还可以实现传统的NLP指标，如ROUGE和BLEU分数。

![](img/6f87d1fda84232c6f03ab28d46ab447f_168.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_78.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_170.png)

但它们有一个弱点：它们非常注重语法，寻找两个文本片段中单词的重叠。例如，“bank”一词在“河岸”和“银行”的语境中可能被视为相似，而LLM评估能更好地利用上下文进行更有意义的评估。

![](img/6f87d1fda84232c6f03ab28d46ab447f_172.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_80.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_174.png)

TruLens提供了更广泛的评估集，以确保你构建的应用程序是**诚实、无害且有益**的。我们鼓励你在完成课程并构建自己的LLM应用时尝试它们。

![](img/6f87d1fda84232c6f03ab28d46ab447f_176.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_178.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_82.png)
![](img/6f87d1fda84232c6f03ab28d46ab447f_84.png)

## 执行评估并查看结果

现在我们已经设置了所有反馈函数，可以设置一个记录器对象来开始记录和评估。

```python
from trulens_eval import TruLlama

![](img/6f87d1fda84232c6f03ab28d46ab447f_180.png)

# 创建 TruLlama 记录器，集成 TruLens 与 LlamaIndex
tru_recorder = TruLlama(
    query_engine,
    app_id="Sentence Window RAG",
    feedbacks=[f_answer_relevance, f_context_relevance, f_groundedness]
)
```

![](img/6f87d1fda84232c6f03ab28d46ab447f_182.png)

![](img/6f87d1fda84232c6f03ab28d46ab447f_86.png)
![](img/6f87d1fda84232