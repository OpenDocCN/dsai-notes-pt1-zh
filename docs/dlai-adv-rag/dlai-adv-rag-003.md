# 003：RAG指标三元组 (RAG Triad) 📊

![](img/a2005aee8b790bf2f86eafe6547573fe_0.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_2.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_4.png)

在本节课中，我们将深入探讨如何评估RAG系统的核心概念。具体来说，我们将介绍**RAG Triad**，这是一个用于评估RAG执行上下文三个主要步骤的指标三元组。我们将学习如何利用反馈功能框架，对LLM应用程序进行编程式评估，并向您展示如何在给定任何非结构化语料库的情况下，综合生成评估数据集。

## 环境设置与回顾

![](img/a2005aee8b790bf2f86eafe6547573fe_6.png)

上一节我们介绍了RAG的基本概念，本节中我们来看看如何为评估做准备。首先，您需要完成一些环境设置。

![](img/a2005aee8b790bf2f86eafe6547573fe_8.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_10.png)

以下是设置步骤：
1.  设置OpenAI API密钥。该密钥将用于完成RAG步骤以及使用TruLens进行评估。
2.  导入必要的库，包括TruLens和LlamaIndex。

![](img/a2005aee8b790bf2f86eafe6547573fe_12.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_14.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_16.png)

```python
# 设置OpenAI API密钥
import os
os.environ["OPENAI_API_KEY"] = "your-api-key-here"

![](img/a2005aee8b790bf2f86eafe6547573fe_18.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_20.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_22.png)

# 导入TruLens类并设置Tru对象
from trulens_eval import Tru
tru = Tru()
tru.reset_database() # 重置数据库，用于记录应用程序的提示、响应和中间结果
```

![](img/a2005aee8b790bf2f86eafe6547573fe_23.png)

接下来，我们将快速回顾如何使用LlamaIndex构建查询引擎。我们将使用TruLens阅读器加载一个PDF文档（例如，Andrew Ng关于AI职业发展的文章），并将数据加载到文档对象中。

![](img/a2005aee8b790bf2f86eafe6547573fe_25.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_27.png)

```python
from trulens_eval.tru_llama import TruLlama
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader

# 使用TruLens阅读器加载文档
documents = SimpleDirectoryReader("./data").load_data()

![](img/a2005aee8b790bf2f86eafe6547573fe_29.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_31.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_33.png)

# 将所有内容合并为一个大文档，而不是默认的每页一个文档
from llama_index.core.node_parser import SimpleNodeParser
parser = SimpleNodeParser.from_defaults(chunk_size=512)
nodes = parser.get_nodes_from_documents(documents)
```

然后，我们利用LlamaIndex的实用程序设置句子窗口索引。

![](img/a2005aee8b790bf2f86eafe6547573fe_35.png)

```python
from llama_index.core import Settings
from llama_index.llms.openai import OpenAI
from llama_index.embeddings.huggingface import HuggingFaceEmbedding

# 配置LLM和嵌入模型
Settings.llm = OpenAI(temperature=0.1, model="gpt-3.5-turbo")
Settings.embed_model = HuggingFaceEmbedding(model_name="BAAI/bge-small-en-v1.5")

![](img/a2005aee8b790bf2f86eafe6547573fe_37.png)

# 创建索引和查询引擎
from llama_index.core import VectorStoreIndex
index = VectorStoreIndex(nodes)
query_engine = index.as_query_engine(similarity_top_k=2)
```

![](img/a2005aee8b790bf2f86eafe6547573fe_39.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_41.png)

现在，我们已经为基于句子窗口的RAG设置了查询引擎。让我们通过询问一个具体问题（例如“如何创建AI作品集”）来实际操作一下。这将返回一个包含LLM最终响应、检索到的上下文片段以及其他元数据的完整对象。

![](img/a2005aee8b790bf2f86eafe6547573fe_43.png)

```python
response = query_engine.query("如何创建AI作品集")
print(response)
```

![](img/a2005aee8b790bf2f86eafe6547573fe_45.png)

## 深入RAG Triad评估

![](img/a2005aee8b790bf2f86eafe6547573fe_47.png)

现在我们已经设置了基于句子窗口的RAG应用程序，我们可以使用RAG Triad来更深入地评估其响应，识别故障模式，并建立改进LLM应用程序的信心。

首先，我们进行一些内务处理。启动一个Streamlit仪表板，用于查看评估结果和运行实验。

```python
tru.run_dashboard() # 从笔记本内部启动仪表板
```

![](img/a2005aee8b790bf2f86eafe6547573fe_49.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_51.png)

我们初始化一个LLM（例如OpenAI GPT-3.5 Turbo）作为我们评估的默认提供者，该提供者将用于实现不同的反馈功能。

![](img/a2005aee8b790bf2f86eafe6547573fe_53.png)

```python
from trulens_eval import Feedback, Select
from trulens_eval.feedback import Groundedness
from trulens_eval.feedback.provider.openai import OpenAI as OpenAIProvider

![](img/a2005aee8b790bf2f86eafe6547573fe_55.png)

# 初始化反馈提供者
provider = OpenAIProvider(model_engine="gpt-3.5-turbo")
```

![](img/a2005aee8b790bf2f86eafe6547573fe_57.png)

接下来，让我们更深入地了解RAG Triad的每一项评估。我们将在幻灯片和笔记本之间来回切换，以便为您提供完整的上下文。

![](img/a2005aee8b790bf2f86eafe6547573fe_59.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_61.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_63.png)

### 1. 答案相关性 (Answer Relevance)

![](img/a2005aee8b790bf2f86eafe6547573fe_65.png)

答案相关性检查最终响应是否与用户提出的查询相关。

![](img/a2005aee8b790bf2f86eafe6547573fe_67.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_69.png)

以下是答案相关性反馈函数的结构示例：
*   **提供者 (Provider)**： 我们使用来自OpenAI的LLM来实现反馈功能。请注意，反馈功能不一定非要使用LLM，也可以使用BERT模型等其他机制。
*   **功能实现**： 我们实现一个名为“答案相关性”的反馈功能。它接收用户输入（查询）和应用程序的最终输出（响应）作为输入。
*   **输出**： 给定用户问题和RAG的最终答案，此反馈功能将利用LLM提供者在0到1的范围内给出一个分数，并提供支持该分数的思想链推理。

![](img/a2005aee8b790bf2f86eafe6547573fe_71.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_73.png)

现在让我们在代码中定义答案相关性反馈函数。

![](img/a2005aee8b790bf2f86eafe6547573fe_75.png)

```python
# 定义答案相关性反馈函数
f_answer_relevance = Feedback(provider.relevance_with_cot_reasoning,
                              name="Answer Relevance"
                             ).on_input_output()
# `on_input_output()` 表示该函数将应用于用户输入和最终输出
```

### 2. 上下文相关性 (Context Relevance)

![](img/a2005aee8b790bf2f86eafe6547573fe_77.png)

上下文相关性检查给定查询的检索过程的好坏。它评估从向量数据库中检索到的每段文本与所提出问题的相关程度。

上下文相关性反馈函数的结构与答案相关性类似，但输入不同：
*   **输入**： 除了用户输入，该函数还接收一个指向检索上下文的指针，这是执行RAG应用程序的中间结果。
*   **输出**： 它为每个检索到的上下文片段返回一个分数，评估该上下文对于查询的相关性，然后对所有片段的分数进行平均以获得最终分数。

在代码中定义上下文相关性反馈函数。

```python
# 定义上下文相关性反馈函数
f_context_relevance = Feedback(provider.context_relevance_with_cot_reasoning,
                               name="Context Relevance"
                              ).on_input().on(Select.Record.app.combine_documents_chain._call.args.inputs.input_documents[:].page_content)
# 此函数应用于用户输入和检索到的上下文
```

![](img/a2005aee8b790bf2f86eafe6547573fe_79.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_81.png)

### 3. 事实基础性 (Groundedness)

![](img/a2005aee8b790bf2f86eafe6547573fe_83.png)

事实基础性检查最终答案中的陈述是否得到检索到的上下文的支持，用于检测幻觉。

![](img/a2005aee8b790bf2f86eafe6547573fe_85.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_87.png)

基础性反馈函数的结构与上下文相关性非常相似：
*   **输入**： 访问RAG应用程序中检索到的上下文集合以及RAG的最终输出。
*   **输出**： 最终响应中的每个句子都会获得一个基础性分数，这些分数被汇总平均以产生完整的最终基础性分数。

![](img/a2005aee8b790bf2f86eafe6547573fe_89.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_91.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_93.png)

用于设置基础性反馈功能的代码片段。

![](img/a2005aee8b790bf2f86eafe6547573fe_95.png)

```python
# 定义基础性反馈函数
grounded = provider.groundedness_measure_with_cot_reasoning
f_groundedness = Feedback(grounded,
                          name="Groundedness"
                         ).on(Select.Record.app.combine_documents_chain._call.args.inputs.input_documents[:].page_content
                             ).on_output()
# 此函数应用于检索到的上下文和最终输出
```

![](img/a2005aee8b790bf2f86eafe6547573fe_97.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_99.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_101.png)

## 评估工作流程与数据集生成

现在我们已经设置了所有三个反馈功能（答案相关性、上下文相关性和事实基础性），我们需要一个评估集来运行应用程序和评估。

![](img/a2005aee8b790bf2f86eafe6547573fe_103.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_105.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_106.png)

我们将遵循以下工作流程来评估和迭代改进LLM应用程序：
1.  从基本LlamaIndex RAG开始，并使用TruLens RAG Triad对其进行评估。
2.  关注与上下文相关性相关的故障模式。
3.  使用高级RAG技术（如LlamaIndex句子窗口RAG）迭代改进基本RAG。
4.  使用TruLens RAG Triad重新评估新的高级RAG，重点关注在上下文相关性等方面是否看到了具体改进。

![](img/a2005aee8b790bf2f86eafe6547573fe_108.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_110.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_112.png)

我们关注上下文相关性的原因，是由于上下文太小经常会导致失败模式。一旦将上下文增加到某个点，您可能会看到上下文相关性的改进。此外，当上下文相关性上升时，我们可能会看到事实基础性方面的改进，因为LLM有足够的相关上下文来生成摘要。

![](img/a2005aee8b790bf2f86eafe6547573fe_114.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_116.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_118.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_120.png)

现在，让我们加载一些预设的评估问题。

![](img/a2005aee8b790bf2f86eafe6547573fe_122.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_124.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_126.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_128.png)

```python
# 加载评估问题
eval_questions = []
with open('eval_questions.txt', 'r') as file:
    for line in file:
        # 移除可能的换行符并过滤空行
        item = line.strip()
        if item:
            eval_questions.append(item)
print(eval_questions)
```

![](img/a2005aee8b790bf2f86eafe6547573fe_130.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_132.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_134.png)

## 运行评估与结果分析

![](img/a2005aee8b790bf2f86eafe6547573fe_136.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_138.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_140.png)

现在我们已经完成了所有设置，可以执行评估了。我们将对评估问题列表中的每个问题运行句子窗口引擎，然后使用TruLens记录器针对RAG Triad运行每条记录。

![](img/a2005aee8b790bf2f86eafe6547573fe_142.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_144.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_146.png)

```python
from trulens_eval.tru_llama import TruLlama
from trulens_eval import FeedbackMode

![](img/a2005aee8b790bf2f86eafe6547573fe_148.png)

# 创建TruLlama记录器对象
tru_recorder = TruLlama(query_engine,
                        app_id='App1',
                        feedbacks=[f_answer_relevance, f_context_relevance, f_groundedness])

![](img/a2005aee8b790bf2f86eafe6547573fe_150.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_152.png)

# 对每个评估问题运行评估
for question in eval_questions:
    with tru_recorder as recording:
        response = query_engine.query(question)
        print(question)
        print(response)
```

评估完成后，我们可以获取记录和反馈，查看日志。

![](img/a2005aee8b790bf2f86eafe6547573fe_154.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_156.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_158.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_160.png)

```python
# 获取记录
records, feedback = tru.get_records_and_feedback(app_ids=["App1"])
print(records[['input', 'output'] + feedback.columns] .head())
```

![](img/a2005aee8b790bf2f86eafe6547573fe_162.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_164.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_166.png)

我们还可以在排行榜中获得一个聚合视图，该视图聚合所有单独的记录并生成平均值。

![](img/a2005aee8b790bf2f86eafe6547573fe_168.png)

```python
# 获取聚合视图（排行榜）
tru.get_leaderboard(app_ids=["App1"])
```

![](img/a2005aee8b790bf2f86eafe6547573fe_170.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_172.png)

TruLens还提供了一个本地简化的应用程序仪表板，您可以使用它来检查正在构建的应用程序，查看评估结果，并深入到记录级别视图中。

![](img/a2005aee8b790bf2f86eafe6547573fe_174.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_176.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_178.png)

```python
# 运行仪表板以进行可视化检查
tru.run_dashboard()
```

![](img/a2005aee8b790bf2f86eafe6547573fe_180.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_182.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_184.png)

在仪表板中，您可以：
*   查看应用程序性能的聚合视图（平均延迟、总成本、各指标平均分）。
*   选择单个应用程序，查看每条记录的详细评估视图（用户输入、响应、各分数）。
*   点击某一行，深入查看答案相关性、上下文相关性和事实基础性的详细评估结果，包括LLM给出分数的思想链原因。

![](img/a2005aee8b790bf2f86eafe6547573fe_186.png)

通过分析得分较低的案例（例如基础性分数为0.5），我们可以识别故障模式。例如，最终答案中的某些陈述可能在检索到的上下文中没有支持证据，从而导致低分。这些洞察可以指导我们后续对RAG应用程序进行迭代和改进。

## 总结

![](img/a2005aee8b790bf2f86eafe6547573fe_188.png)

![](img/a2005aee8b790bf2f86eafe6547573fe_190.png)

在本节课中，我们一起学习了RAG指标三元组（RAG Triad）的核心概念：**答案相关性**、**上下文相关性**和**事实基础性**。我们了解了如何将这些评估指标实现为可编程的反馈函数，并利用TruLens框架对一个实际的句子窗口RAG应用程序进行评估。通过设置评估集、运行评估和分析仪表板结果，我们掌握了识别RAG系统故障模式、建立性能基准并指导其迭代改进的基本方法。在下一课中，我们将介绍更高级的RAG技术，并展示如何利用这些评估方法来优化应用程序性能。