# 011：知识图谱构建 – 第二部分 🧠

![](img/7479beb84ff34a83faf6d4c678d9097c_0.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_2.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_4.png)

在本节课中，我们将继续知识图谱的构建工作。上一节课我们根据构建计划，从CSV文件创建了领域图。现在，我们将处理Markdown文件，将其分块成词汇图，并将提取的实体整合到主题图中。

## 概述

![](img/7479beb84ff34a83faf6d4c678d9097c_6.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_8.png)

本节课我们将学习如何使用Neo4j GraphRAG库进行文本分块和实体提取，并了解实体解析的技术。这并非工作流中的智能体部分，整个过程将由工具和辅助函数完成。我们将定义两个核心工具：一个用于构建知识图谱，另一个用于关联主题节点和领域节点。

![](img/7479beb84ff34a83faf6d4c678d9097c_10.png)

## 环境设置与数据准备

首先，我们需要导入必要的库并完成环境设置。

![](img/7479beb84ff34a83faf6d4c678d9097c_12.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_13.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_15.png)

```python
# 导入所需库
import ...
```

确保OpenAI和Neo4j环境已准备就绪。由于Neo4j在加载本笔记本时是全新的，我们需要加载上一节课创建的产品节点。我们有一个辅助函数 `load_product_nodes` 来完成此任务。

![](img/7479beb84ff34a83faf6d4c678d9097c_17.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_18.png)

```python
# 加载产品节点
load_product_nodes()
```

接下来，我们需要加载工作流前序部分创建的初始状态，包括构建计划、已批准的文件、已批准的实体和事实类型。

```python
# 加载初始状态
construction_plan = load_construction_plan()
approved_files = load_approved_files()
approved_entities = load_approved_entities()
approved_fact_types = load_approved_fact_types()
```

![](img/7479beb84ff34a83faf6d4c678d9097c_20.png)

## 知识图谱构建管道

![](img/7479beb84ff34a83faf6d4c678d9097c_22.png)

现在，我们可以开始定义处理Markdown文件、进行分块和实体提取所需的所有函数。Neo4j GraphRAG库提供了一个便捷的 `SimpleKGPipeline` 类来完成这些处理。

### 了解SimpleKGPipeline接口

![](img/7479beb84ff34a83faf6d4c678d9097c_24.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_26.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_27.png)

`SimpleKGPipeline` 需要以下组件：
*   **LLM**：用于实体提取。
*   **Neo4j驱动**：用于将图写入数据库。
*   **嵌入模型**：用于计算文本块的向量嵌入。
*   **PDF加载器**：我们将使用自定义的Markdown加载器。
*   **文本分割器**：我们将使用自定义的分割器。
*   **模式**：基于上一节课提出的实体和事实类型。
*   **提示词**：指导LLM如何进行提取。

以下是创建管道实例的示例代码结构：

![](img/7479beb84ff34a83faf6d4c678d9097c_29.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_31.png)

```python
pipeline = SimpleKGPipeline(
    llm=llm_instance,
    driver=neo4j_driver,
    embedder=embedder_instance,
    pdf_loader=custom_markdown_loader,
    text_splitter=custom_text_splitter,
    schema=extraction_schema,
    prompt=custom_prompt
)
```

![](img/7479beb84ff34a83faf6d4c678d9097c_32.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_33.png)

### 端到端工作流

![](img/7479beb84ff34a83faf6d4c678d9097c_35.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_37.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_38.png)

让我们了解一下Neo4j KG构建器的端到端工作流，以帮助理解这些组件的作用：
1.  **文档加载**：使用自定义的Markdown加载器加载每个文档。
2.  **文本分块**：文本分割器组件执行分块。
3.  **嵌入计算**：为每个文本块计算向量嵌入。
4.  **实体与关系提取**：LLM根据实体和关系提取器的配置分析每个块。
5.  **图构建**：在内存中构建图。
6.  **图修剪（可选）**：使用图修剪器清理图。
7.  **图写入**：KG写入器组件将内存中的图保存到Neo4j。
8.  **实体解析**：实体解析器组件合并可能是同一实体的节点。

![](img/7479beb84ff34a83faf6d4c678d9097c_40.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_41.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_42.png)

## 定义自定义组件

![](img/7479beb84ff34a83faf6d4c678d9097c_44.png)

现在，我们可以开始定义需要传入管道的自定义函数。

### 自定义文本分割器

![](img/7479beb84ff34a83faf6d4c678d9097c_46.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_47.png)

首先，我们设置一个自定义文本分割器。根据我们Markdown文件的结构（以H1标题开头，用 `---` 分隔评论），我们将使用一个简单的正则表达式分割器。

![](img/7479beb84ff34a83faf6d4c678d9097c_49.png)

以下是扩展 `TextSplitter` 基类的实现：

![](img/7479beb84ff34a83faf6d4c678d9097c_50.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_51.png)

```python
class CustomTextSplitter(TextSplitter):
    def run(self, text, regex_pattern):
        # 使用正则表达式分割文本
        splits = re.split(regex_pattern, text)
        # 将分割结果转换为TextChunk对象列表
        text_chunks = [TextChunk(text=split, index=i) for i, split in enumerate(splits)]
        return text_chunks
```

### 自定义Markdown数据加载器

类似地，我们将创建一个自定义的数据加载器来加载Markdown文件。它将扩展 `DataLoaderBase` 类，并提取文档标题作为元数据。

![](img/7479beb84ff34a83faf6d4c678d9097c_53.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_55.png)

以下是 `MarkdownDataLoader` 的实现：

```python
class MarkdownDataLoader(DataLoaderBase):
    def extract_title(self, text):
        # 使用正则表达式提取第一个H1标题作为文档标题
        match = re.search(r'^# (.+)$', text, re.MULTILINE)
        return match.group(1) if match else "Untitled"

    def run(self, file_path):
        # 从文件系统加载Markdown文本
        with open(file_path, 'r') as f:
            markdown_text = f.read()
        # 提取标题
        title = self.extract_title(markdown_text)
        # 创建包含元数据和文本的DocumentInfo对象
        doc_info = DocumentInfo(metadata={'title': title})
        pdf_doc = PDFDocument(text=markdown_text)
        return doc_info, pdf_doc
```

![](img/7479beb84ff34a83faf6d4c678d9097c_57.png)

### 配置LLM、嵌入模型和驱动

我们需要为知识图谱构建管道设置LLM、嵌入模型和Neo4j驱动。我们将使用OpenAI。

```python
# 创建LLM和嵌入模型实例
llm = OpenAILanguageModel(api_key=openai_api_key)
embedder = OpenAIEmbedder(api_key=openai_api_key)
# 从GDB单例获取Neo4j驱动
driver = get_neo4j_driver()
```

![](img/7479beb84ff34a83faf6d4c678d9097c_59.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_60.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_61.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_62.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_63.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_64.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_65.png)

## 配置实体提取上下文

![](img/7479beb84ff34a83faf6d4c678d9097c_67.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_68.png)

接下来，我们转向配置知识图谱实体提取所需的上下文，从实体模式开始。

![](img/7479beb84ff34a83faf6d4c678d9097c_70.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_71.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_72.png)

### 定义实体模式

Neo4j GraphRAG包所需的实体模式包含几个部分：节点类型、关系类型和模式模式。

![](img/7479beb84ff34a83faf6d4c678d9097c_74.png)

```python
# 节点类型（来自已批准的实体）
node_types = approved_entities
# 关系类型（从已批准的事实类型中提取键）
relationship_types = list(approved_fact_types.keys())
# 模式模式（重新打包已批准的事实类型信息）
schema_patterns = []
for fact in approved_fact_types.values():
    pattern = (fact['subject_label'], fact['predicate_label'].upper(), fact['object_label'])
    schema_patterns.append(pattern)
# 完整的模式
kg_schema = {
    'node_types': node_types,
    'relationship_types': relationship_types,
    'patterns': schema_patterns,
    'strict_mode': True  # 仅使用定义的类型，不添加新内容
}
```

### 创建自定义提示词

![](img/7479beb84ff34a83faf6d4c678d9097c_76.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_77.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_79.png)

我们将创建一个自定义提示词，它会注入每个文本块的内容、模式以及文件级别的上下文。

![](img/7479beb84ff34a83faf6d4c678d9097c_81.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_83.png)

首先，定义一个辅助函数来获取文件上下文（例如文件标题和简介）。

![](img/7479beb84ff34a83faf6d4c678d9097c_85.png)

```python
def get_file_context(file_text):
    # 获取文件的前几行作为上下文
    lines = file_text.split('\n')[:5]
    return '\n'.join(lines)
```

![](img/7479beb84ff34a83faf6d4c678d9097c_87.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_88.png)

然后，定义提示词模板。它包含LLM的角色和目标、设计说明、输出格式规范、模式定义以及文件上下文。

```python
def create_contextualized_prompt(file_context):
    prompt_template = """
    角色：你是一个知识提取专家。
    目标：从提供的文本块中提取实体和关系，并遵循给定的模式。
    设计说明：仅提取模式中定义的实体和关系类型。为每个节点生成唯一ID。
    输出格式：以JSON格式输出，包含节点列表（含id、标签和属性）和关系列表。
    模式定义：
    {schema_definition}
    文件上下文：
    {file_context}
    文本块：
    {chunk_text}
    """
    # 在实际使用中，schema_definition和chunk_text会被动态注入
    return prompt_template
```

## 构建并运行知识图谱构建器

配置好所有上下文和辅助函数后，我们可以创建知识图谱构建器并运行它。

### 创建知识图谱构建器辅助函数

由于我们要为每个文件创建一个知识图谱构建器（以便拥有基于文件内容的专门提示词），我们定义一个辅助函数。

```python
def make_kg_builder(file_path):
    # 获取文件级别的上下文
    file_context = get_file_context_from_path(file_path)
    # 创建上下文化的提示词
    contextualized_prompt = create_contextualized_prompt(file_context)
    # 创建SimpleKGPipeline实例
    pipeline = SimpleKGPipeline(
        llm=llm,
        driver=driver,
        embedder=embedder,
        pdf_loader=MarkdownDataLoader(),
        text_splitter=CustomTextSplitter(regex_pattern=r'\n---\n'),
        schema=kg_schema,
        prompt=contextualized_prompt
    )
    return pipeline
```

![](img/7479beb84ff34a83faf6d4c678d9097c_90.png)

### 处理所有Markdown文件

![](img/7479beb84ff34a83faf6d4c678d9097c_92.png)

现在，我们可以遍历所有已批准的文件，为每个文件创建构建器并运行处理。

![](img/7479beb84ff34a83faf6d4c678d9097c_94.png)

```python
for file_info in approved_files:
    file_path = file_info['path']
    print(f"正在处理文件: {file_path}")
    # 创建知识图谱构建器
    kg_builder = make_kg_builder(file_path)
    # 运行构建器
    kg_builder.run(file_path)
```

![](img/7479beb84ff34a83faf6d4c678d9097c_96.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_98.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_100.png)

此循环完成后，我们将得到一个完整的词汇图（包含相互连接且连接到文档节点的文本块）和一个主题图（包含所有提取的实体及其关系）。

![](img/7479beb84ff34a83faf6d4c678d9097c_102.png)

## 关联主题图与领域图

![](img/7479beb84ff34a83faf6d4c678d9097c_104.png)

现在我们已经处理了所有文件，拥有了词汇图、主题图和领域图。然而，图谱还不完整，因为主题图和领域图没有连接。领域图是从CSV文件创建的，而主题图是从Markdown文件中提取数据创建的。

下一步是将我们从Markdown文件中提取的实体（主题图）与领域图连接起来。我们将定义一些工具来完成这个任务。

![](img/7479beb84ff34a83faf6d4c678d9097c_106.png)

### 实体解析策略

对于主题图中的每种实体类型，我们将设计一种策略来与领域图中的正确节点关联。例如，主题图中具有产品名称的产品应与领域图中的产品关联。

![](img/7479beb84ff34a83faf6d4c678d9097c_108.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_109.png)

为了实现这一点，我们需要做以下几件事：
1.  查找主题图中所有唯一的实体标签。
2.  查找领域图中所有唯一的节点标签。
3.  尝试关联这两组标签之间的属性键，以确定如何匹配实体。

### 查找主题图中的唯一实体标签

![](img/7479beb84ff34a83faf6d4c678d9097c_111.png)

首先，让我们查看主题图，看看Neo4j GraphRAG库处理后节点是什么样子。结果节点将带有一个额外的标签 `__entity__label` 来标识它们。

![](img/7479beb84ff34a83faf6d4c678d9097c_112.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_113.png)

我们可以运行一个Cypher查询来查找不同的实体标签。

![](img/7479beb84ff34a83faf6d4c678d9097c_115.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_116.png)

```cypher
MATCH (n)
WHERE any(label IN labels(n) WHERE label CONTAINS '__entity__')
UNWIND labels(n) AS entity_label
RETURN DISTINCT entity_label
```

![](img/7479beb84ff34a83faf6d4c678d9097c_118.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_120.png)

为了过滤掉下划线开头的内部标签（如 `__entity__` 和 `__kg_builder__`），我们可以优化查询：

![](img/7479beb84ff34a83faf6d4c678d9097c_122.png)

```cypher
MATCH (n)
WHERE any(label IN labels(n) WHERE label CONTAINS '__entity__')
UNWIND labels(n) AS entity_label
WHERE NOT entity_label STARTS WITH '_'
RETURN DISTINCT entity_label
```

我们将这个查询包装成一个辅助函数 `get_unique_entity_labels`。

![](img/7479beb84ff34a83faf6d4c678d9097c_124.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_125.png)

### 查找实体和领域节点的属性键

![](img/7479beb84ff34a83faf6d4c678d9097c_127.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_129.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_130.png)

接下来，对于每个唯一的实体标签，我们还需要查找这些标签拥有的唯一属性键。

![](img/7479beb84ff34a83faf6d4c678d9097c_132.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_133.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_135.png)

以下是查找主题图中某个实体标签唯一键的辅助函数：

![](img/7479beb84ff34a83faf6d4c678d9097c_137.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_138.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_139.png)

```python
def get_unique_entity_keys(entity_label):
    query = """
    MATCH (n:`%s`)
    WHERE any(label IN labels(n) WHERE label CONTAINS '__entity__')
    UNWIND keys(n) AS key
    RETURN COLLECT(DISTINCT key) AS unique_keys
    """ % entity_label
    result = driver.run(query)
    return result.data()[0]['unique_keys']
```

![](img/7479beb84ff34a83faf6d4c678d9097c_141.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_142.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_144.png)

类似地，我们定义一个函数来查找领域图中某个标签的唯一属性键：

```python
def get_unique_domain_keys(domain_label):
    query = """
    MATCH (n:`%s`)
    WHERE NOT any(label IN labels(n) WHERE label CONTAINS '__entity__')
    UNWIND keys(n) AS key
    RETURN COLLECT(DISTINCT key) AS unique_keys
    """ % domain_label
    result = driver.run(query)
    return result.data()[0]['unique_keys']
```

### 规范化属性键并关联键值对

![](img/7479beb84ff34a83faf6d4c678d9097c_146.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_147.png)

为了便于比较，我们定义一个辅助函数来规范化属性键（例如，小写、去除空格、移除标签前缀）。

```python
def normalize_key(label, key):
    key = key.lower().strip()
    # 如果键以标签前缀开头，则移除它
    prefix = label.lower() + ' '
    if key.startswith(prefix):
        key = key[len(prefix):]
    return key
```

然后，我们定义一个函数来关联给定标签的键。它使用 `rapidfuzz` 库计算文本相似度得分，并将高度相关的键配对。

```python
from rapidfuzz import fuzz

def correlate_keys(label, entity_keys, domain_keys, similarity_threshold=0.9):
    correlated_keys = []
    for e_key in entity_keys:
        for d_key in domain_keys:
            norm_e_key = normalize_key(label, e_key)
            norm_d_key = normalize_key(label, d_key)
            # 计算相似度得分 (0-100)，并转换为0-1的范围
            similarity = fuzz.ratio(norm_e_key, norm_d_key) / 100.0
            if similarity > similarity_threshold:
                correlated_keys.append((e_key, d_key, similarity))
    # 按相似度排序
    correlated_keys.sort(key=lambda x: x[2], reverse=True)
    return correlated_keys
```

![](img/7479beb84ff34a83faf6d4c678d9097c_149.png)

### 使用Jaro-Winkler距离进行实体解析

![](img/7479beb84ff34a83faf6d4c678d9097c_151.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_152.png)

最后，我们使用Cypher的 `apoc.text.jaroWinklerDistance` 函数，基于相似属性值来执行实体解析。该函数计算两个字符串之间的编辑距离，得分在0到1之间（0表示完全匹配）。

![](img/7479beb84ff34a83faf6d4c678d9097c_154.png)

以下是一个Cypher查询示例，它查找产品实体中 `name` 和 `product_name` 属性值高度匹配的节点对，并创建 `CORRESPONDS_TO` 关系。

![](img/7479beb84ff34a83faf6d4c678d9097c_156.png)

```cypher
MATCH (entity:Product)
WHERE any(label IN labels(entity) WHERE label CONTAINS '__entity__')
MATCH (domain:Product)
WHERE NOT any(label IN labels(domain) WHERE label CONTAINS '__entity__')
WITH entity, domain,
     apoc.text.jaroWinklerDistance(entity.name, domain.product_name) AS score
WHERE score < 0.1
MERGE (entity)-[r:CORRESPONDS_TO]->(domain)
ON CREATE SET r.created_at = timestamp()
ON MATCH SET r.updated_at = timestamp()
RETURN count(r) AS relationships_created
```

我们将这个查询包装成一个函数 `resolve_entities`。

![](img/7479beb84ff34a83faf6d4c678d9097c_158.png)

### 连接所有实体

![](img/7479beb84ff34a83faf6d4c678d9097c_159.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_160.png)

为了完整性，我们遍历所有唯一的实体标签，尝试将主题节点与相应的领域节点关联起来。

![](img/7479beb84ff34a83faf6d4c678d9097c_162.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_164.png)

```python
unique_entity_labels = get_unique_entity_labels()
for label in unique_entity_labels:
    entity_keys = get_unique_entity_keys(label)
    domain_keys = get_unique_domain_keys(label)
    key_pairs = correlate_keys(label, entity_keys, domain_keys)
    if key_pairs:
        # 使用最相关的键对进行解析
        best_entity_key, best_domain_key, _ = key_pairs[0]
        resolve_entities(label, best_entity_key, best_domain_key, threshold=0.1)
```

![](img/7479beb84ff34a83faf6d4c678d9097c_166.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_167.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_168.png)

## 总结

![](img/7479beb84ff34a83faf6d4c678d9097c_170.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_172.png)

在本节课中，我们一起完成了知识图谱构建的第二部分。我们学习了：

![](img/7479beb84ff34a83faf6d4c678d9097c_174.png)

![](img/7479beb84ff34a83faf6d4c678d9097c_175.png)

1.  **配置知识图谱构建管道**：使用Neo4j GraphRAG库的 `SimpleKGPipeline`，并为其定制了Markdown加载器、文本分割器、提取模式和提示词。
2.  **处理非结构化文本**：遍历Markdown文件，进行分块、嵌入计算和实体/关系提取，从而构建了词汇图和主题图。
3.  **执行实体解析**：通过比较主题图和领域图中实体的属性键和属性值，使用字符串相似度算法（如Jaro-Winkler距离）将两个图中的相同实体关联起来，创建了 `CORRESPONDS_TO` 关系。

![](img/7479beb84ff34a83faf6d4c678d9097c_177.png)

最终，我们成功地将从CSV文件构建的领域图、从Markdown文件构建的词汇图和主题图连接起来，形成了一个完整且互联的知识图谱。