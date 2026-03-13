# 061：4.SC-Laurence_L3_v04.zh

![](img/5289e8520d74fc4c3d52bc47334a8937_0.png)

## 概述 📋

![](img/5289e8520d74fc4c3d52bc47334a8937_2.png)

![](img/5289e8520d74fc4c3d52bc47334a8937_4.png)

![](img/5289e8520d74fc4c3d52bc47334a8937_6.png)

在本节课中，我们将学习如何利用大型语言模型（LLM）作为编程助手，来处理和改进现有代码。我们将探索多个实际场景，包括代码优化、简化、测试用例生成、效率提升以及调试。通过这些练习，你将了解如何有效地向LLM提问，以获得有价值的编程见解和解决方案。

![](img/5289e8520d74fc4c3d52bc47334a8937_8.png)

![](img/5289e8520d74fc4c3d52bc47334a8937_10.png)

---

![](img/5289e8520d74fc4c3d52bc47334a8937_12.png)

![](img/5289e8520d74fc4c3d52bc47334a8937_14.png)

## 环境设置与准备 ⚙️

![](img/5289e8520d74fc4c3d52bc47334a8937_16.png)

![](img/5289e8520d74fc4c3d52bc47334a8937_18.png)

在开始之前，我们需要设置好编程环境。这包括导入必要的库、配置API密钥以及初始化模型。

![](img/5289e8520d74fc4c3d52bc47334a8937_20.png)

![](img/5289e8520d74fc4c3d52bc47334a8937_22.png)

首先，我们需要从工具中获取API密钥。我们将使用Google的生成式AI库。

![](img/5289e8520d74fc4c3d52bc47334a8937_24.png)

```python
import google.generativeai as palm
```

接下来，配置API密钥。

```python
palm.configure(api_key='YOUR_API_KEY')
```

![](img/5289e8520d74fc4c3d52bc47334a8937_26.png)

然后，我们需要遍历可用的模型，并选择一个支持文本生成的模型。在本例中，我们选择 `text-bison-001` 模型。

![](img/5289e8520d74fc4c3d52bc47334a8937_28.png)

```python
models = [m for m in palm.list_models() if 'generateText' in m.supported_generation_methods]
model = models[0].name
```

最后，我们设置一个包装函数来调用文本生成功能，并覆盖默认参数（如温度）以获得更确定的输出。

```python
from google.api_core import retry

@retry.Retry()
def generate_text(prompt, **kwargs):
    return palm.generate_text(prompt=prompt, model=model, **kwargs)

# 设置温度参数为0，以获得更确定的输出
response = generate_text(prompt, temperature=0.0)
```

现在，我们已经准备好开始探索LLM在编程中的各种应用。

---

## 改进现有代码 ✨

上一节我们设置了环境，本节中我们来看看如何利用LLM改进现有的代码。这对于学习新语言或优化现有代码风格特别有用。

以下是请求LLM改进代码的步骤：

1.  **定义提示模板**：创建一个包含代码和请求的提示。
2.  **请求改进**：将提示发送给LLM，请求其提供改进建议。
3.  **分析结果**：查看LLM提供的改进方案和解释。

例如，我们有一段遍历数组并打印元素的Python代码，但写法可能不够“Pythonic”。

![](img/5289e8520d74fc4c3d52bc47334a8937_30.png)

![](img/5289e8520d74fc4c3d52bc47334a8937_32.png)

```python
# 原始代码示例
def print_array(array):
    for i in range(len(array)):
        print(array[i])
```

我们可以向LLM提问：“这段Python代码是好的做法吗？如果不是，请改进它并详细解释。”

![](img/5289e8520d74fc4c3d52bc47334a8937_34.png)

LLM可能会建议使用更简洁的列表推导式或 `enumerate` 函数，并解释为什么这些方法更好。

![](img/5289e8520d74fc4c3d52bc47334a8937_36.png)

```python
# LLM可能建议的改进代码
def print_array(array):
    for element in array:
        print(element)
    # 或者使用列表推导式: [print(element) for element in array]
```

通过这种方式，我们不仅能获得更好的代码，还能学习到更地道的编程实践。

---

![](img/5289e8520d74fc4c3d52bc47334a8937_38.png)

## 探索多种解决方案 🔍

除了获得单一改进方案，我们还可以请求LLM探索解决问题的多种方法，并比较它们的优劣。

我们可以修改提示，要求LLM“探索解决这个问题的多种方法，并解释每一种”。

LLM可能会返回多种方案，例如：

*   **列表推导式**：简洁，但复杂时可能难以阅读。
*   **`enumerate` 函数**：易于阅读，但需要额外的变量。
*   **`map` 函数**：灵活，但需要自定义格式化函数。

通过比较这些方法，我们可以根据具体需求选择最合适的解决方案，并加深对语言特性的理解。

---

## 简化复杂代码 🧹

有时，我们编写的代码虽然功能正确，但结构复杂，不易维护。LLM可以帮助我们简化代码逻辑。

![](img/5289e8520d74fc4c3d52bc47334a8937_40.png)

例如，我们有一个手动创建节点的链表实现，代码较为冗长。

![](img/5289e8520d74fc4c3d52bc47334a8937_42.png)

```python
# 复杂的链表初始化代码
node1 = Node("Monday")
node2 = Node("Tuesday")
node3 = Node("Wednesday")
list1.head_val = node1
node1.next_val = node2
node2.next_val = node3
```

![](img/5289e8520d74fc4c3d52bc47334a8937_44.png)

![](img/5289e8520d74fc4c3d52bc47334a8937_46.png)

我们可以请求LLM：“请简化这段Python链表代码，并详细解释你的修改。”

![](img/5289e8520d74fc4c3d52bc47334a8937_48.png)

LLM可能会建议添加辅助函数来自动化节点的创建和链接过程，从而使代码更清晰、更易于管理。

![](img/5289e8520d74fc4c3d52bc47334a8937_50.png)

![](img/5289e8520d74fc4c3d52bc47334a8937_52.png)

```python
# 简化后的代码可能包含辅助函数
def create_linked_list(values):
    head = None
    current = None
    for value in values:
        new_node = Node(value)
        if not head:
            head = new_node
            current = head
        else:
            current.next_val = new_node
            current = new_node
    return head
```

这样，代码不仅更简洁，也更容易扩展和维护。

![](img/5289e8520d74fc4c3d52bc47334a8937_54.png)

---

![](img/5289e8520d74fc4c3d52bc47334a8937_56.png)

## 生成单元测试 🧪

编写单元测试是确保代码质量的关键步骤。LLM可以根据现有代码自动生成测试用例。

我们可以向LLM提供代码，并请求：“请为这段Python代码生成测试用例，并详细解释这些测试的目的。”

例如，为我们之前简化的链表代码生成测试。

```python
# LLM可能生成的测试用例示例
import unittest

class TestLinkedList(unittest.TestCase):
    def test_creation(self):
        ll = create_linked_list([1, 2, 3])
        self.assertIsNotNone(ll)
        self.assertEqual(ll.data, 1)
        # ... 更多断言

    def test_insertion(self):
        # 测试插入节点的逻辑
        pass

    def test_deletion(self):
        # 测试删除节点的逻辑
        pass
```

生成的测试用例可以覆盖各种边界情况，甚至可能启发我们为代码添加之前未考虑的功能（如删除节点），从而提高代码的健壮性。

---

## 提升代码效率 ⚡

LLM可以分析代码的算法效率，并提出优化建议。这对于处理大数据或性能关键的应用尤其重要。

假设我们有一个使用递归实现的二叉搜索函数。

```python
def binary_search(arr, x, low, high):
    if high >= low:
        mid = (high + low) // 2
        if arr[mid] == x:
            return mid
        elif arr[mid] > x:
            return binary_search(arr, x, low, mid - 1)
        else:
            return binary_search(arr, x, mid + 1, high)
    else:
        return -1
```

我们可以询问LLM：“请使这段代码更高效，并解释你的修改。”

LLM可能会指出递归的内存开销，并建议使用迭代方法，或者直接利用Python内置的高效模块（如 `bisect`）。

```python
import bisect

def binary_search_efficient(arr, x):
    i = bisect.bisect_left(arr, x)
    if i != len(arr) and arr[i] == x:
        return i
    return -1
```

**注意**：在使用LLM的建议时，必须仔细验证其正确性，因为模型有时会产生“幻觉”，提供看似合理但不准确的信息。

![](img/5289e8520d74fc4c3d52bc47334a8937_58.png)

![](img/5289e8520d74fc4c3d52bc47334a8937_60.png)

---

## 调试与错误识别 🐛

LLM可以帮助识别代码中潜在的错误，特别是那些不会立即导致崩溃的逻辑错误。

![](img/5289e8520d74fc4c3d52bc47334a8937_62.png)

例如，我们有一段双向链表的代码，其中打印函数在节点为 `None` 时可能引发错误。

![](img/5289e8520d74fc4c3d52bc47334a8937_64.png)

```python
def print_list(self):
    current = self.head
    while current:
        print(current.data)
        current = current.next
        # 错误：当current为None时，尝试访问current.next会导致错误
```

我们可以请求LLM：“请帮我调试这段代码，详细解释你发现的问题以及为什么它是一个bug。”

LLM可能会识别出在循环中未正确检查 `current` 是否为 `None` 就访问其属性，并建议添加条件判断。

```python
def print_list_fixed(self):
    current = self.head
    while current and current.next:  # 添加更严格的检查
        print(current.data)
        current = current.next
```

通过这种方式，LLM可以作为一个中立的代码审查者，帮助我们捕捉自己可能忽略的细节。

---

![](img/5289e8520d74fc4c3d52bc47334a8937_66.png)

## 总结 🎯

![](img/5289e8520d74fc4c3d52bc47334a8937_68.png)

本节课中，我们一起学习了如何将大型语言模型作为强大的编程助手。我们探讨了多个应用场景：

*   **改进代码风格**：获得更地道、更高效的代码实现。
*   **探索多种方案**：比较不同解决方法的优缺点。
*   **简化复杂逻辑**：使代码更清晰、更易于维护。
*   **生成单元测试**：自动创建测试用例以提高代码质量。
*   **提升运行效率**：优化算法和数据结构。
*   **辅助调试**：识别和修复潜在的错误。

![](img/5289e8520d74fc4c3d52bc47334a8937_70.png)

重要的是，虽然LLM能提供宝贵的建议和灵感，但我们始终需要谨慎验证其输出，并结合自己的判断来使用这些建议。希望你能将这些技巧应用到自己的编程实践中，不断提升开发效率和质量。