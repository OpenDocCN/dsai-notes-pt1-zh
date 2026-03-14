# 25：Triton中的多GPU编程框架Iris 🚀

![](img/6651019c42b2811f2ce4d85f264eba3a_1.png)

## 概述
在本节课中，我们将学习Iris，一个基于Triton构建的、用于简化多GPU编程的开源框架。我们将了解其设计原则、核心API、编程模型，并通过具体示例展示如何利用Iris实现计算与通信的重叠，从而提升多GPU程序的性能。

![](img/6651019c42b2811f2ce4d85f264eba3a_3.png)

---

## Iris框架简介 🎯

![](img/6651019c42b2811f2ce4d85f264eba3a_5.png)

Iris是一个开源的、基于Triton的框架，旨在为多GPU编程提供一流的开发体验。它仅用370行代码实现，提供了简洁的Pythonic接口，使得在Triton中进行细粒度的多GPU编程成为可能。

Iris的核心设计原则是保持Triton原有的可编程性，同时提供高性能的多GPU操作支持。它提供了与PyTorch和Triton风格相似的API，适用于主机端和设备端的抽象。

---

## 传统方法与Iris的对比 🔄

![](img/6651019c42b2811f2ce4d85f264eba3a_7.png)

上一节我们介绍了Iris的基本概念，本节中我们来看看它与传统多GPU编程方法的区别。

![](img/6651019c42b2811f2ce4d85f264eba3a_9.png)

传统的多GPU编程通常采用**批量同步编程模型**。以下是其典型流程：
1.  设置设备。
2.  启动计算内核。
3.  等待计算内核完全完成。
4.  启动通信内核。
5.  等待通信内核完全完成。
6.  启动后续工作。

![](img/6651019c42b2811f2ce4d85f264eba3a_10.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_12.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_13.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_15.png)

这种模型存在一些问题：它是CPU发起的控制路径，需要主机与设备端同步，并且由于其“走走停停”的特性，会带来很多效率低下的问题。

![](img/6651019c42b2811f2ce4d85f264eba3a_17.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_19.png)

相比之下，Iris采用了不同的世界观。在Iris中，代码看起来是这样的：
*   在一个内核中，你可以在工作组或线程块粒度上进行一些计算。
*   如果需要多GPU通信，在计算之后，只需进行简单的存储或加载操作。
    *   例如：进行计算 -> 存储到远程GPU -> 进行更多计算 -> 从远程GPU加载数据。
*   它遵循Triton已有的、以工作组为中心的自然编程模型，并将其扩展到支持多GPU编程。
*   它不需要任何中间缓冲区，也无需额外的同步开销，本质上就是一个可以编写此类模式的融合内核。

Iris旨在提供设备端原语，以实现上述类型的代码。

---

![](img/6651019c42b2811f2ce4d85f264eba3a_21.png)

## Iris的设计原则 🏗️

在构建Iris时，我们遵循了四个核心原则：

![](img/6651019c42b2811f2ce4d85f264eba3a_23.png)

1.  **简洁性与最小依赖**：确保API简洁、依赖最少，任何有Triton背景的人都能轻松上手。
2.  **清晰的抽象**：提供Pythonic的主机端API和Triton风格的设备端API，用于多GPU的加载、存储和原子操作。
3.  **对称堆实现**：完全在Python中实现，基于对称堆（Symmetric Heap）模型。我们提供了集合通信（如All-Gather、All-to-All）的示例，但框架的核心目标是提供设备端API，用户可以在其上构建集合通信。
4.  **可扩展性**：设计旨在支持扩展。目前支持单节点后端（通过XGMI），并计划未来支持多节点扩展。

Iris的可编程性是其构建时的根本焦点之一。

![](img/6651019c42b2811f2ce4d85f264eba3a_25.png)

---

## 主机端与设备端API 📚

![](img/6651019c42b2811f2ce4d85f264eba3a_27.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_29.png)

上一节我们了解了Iris的设计原则，本节中我们来看看其具体的API设计。

![](img/6651019c42b2811f2ce4d85f264eba3a_31.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_33.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_35.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_37.png)

### 主机端API
Iris扩展了PyTorch风格的张量创建接口，使其支持多GPU。例如，你可以使用与PyTorch相同的接口在多个GPU上创建张量：

```python
# 在所有GPU上创建一个张量
tensor = iris.full(...)
```

![](img/6651019c42b2811f2ce4d85f264eba3a_39.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_40.png)

它还支持其他PyTorch API，如 `zeros`、`arange`、`linspace` 等，只需扩展它们以支持多GPU变体即可。

### 设备端API
设备端API与Triton的加载/存储API非常相似。关键区别在于需要指定目标GPU的`rank`和堆基础地址`heap_bases`。

```python
# 从远程GPU（rank=1）加载数据
data = iris.load(pointer, remote_rank=1, heap_bases=...)
```

`heap_bases` 是一个关键概念，我们将在下一节详细解释。即使有这些额外的参数，API看起来仍然非常熟悉。

---

![](img/6651019c42b2811f2ce4d85f264eba3a_42.png)

## 核心：对称堆（Symmetric Heap）模型 💾

![](img/6651019c42b2811f2ce4d85f264eba3a_44.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_46.png)

Iris实现的核心是**对称堆**的概念，这是一种分区全局地址空间（PGAS）抽象。

![](img/6651019c42b2811f2ce4d85f264eba3a_48.png)

关键思想是：在所有GPU上创建一个对称堆。你需要跟踪两种偏移量：
1.  **堆基址偏移**：对于给定的进程ID，其虚拟地址空间的起始位置。
2.  **变量偏移**：特定变量在该堆内的位置。

所有Iris操作都基于这个堆进行。程序开始时，需要通过All-Gather操作在所有进程间共享堆基址偏移。一旦获得这些地址，它们就会被传递给Iris的API，并通过一个地址转换计算来确定远程内存的位置。

![](img/6651019c42b2811f2ce4d85f264eba3a_50.png)

地址转换函数是Iris后端库中最关键的函数，其核心是简单的指针运算：
1.  加载当前rank和目标rank的堆基址。
2.  计算指针相对于本地堆基址的偏移量：`offset = pointer - local_heap_base`。
3.  将偏移量加到目标堆基址上，得到转换后的指针：`translated_ptr = remote_heap_base + offset`。

理解了这个转换过程，就理解了Iris设备端后端的全部思想。

---

![](img/6651019c42b2811f2ce4d85f264eba3a_52.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_53.png)

## 性能验证与原子操作 ⚡

为了验证Iris实现的效率，我们进行了基准测试，测量GPU间的加载/存储带宽。

测试结果显示，无论是本地内存（HBM）访问还是跨XGMI的远程访问，Iris都能达到接近硬件峰值（约100%）的带宽。这证实了在Triton中实现此功能没有引入额外开销。通过检查生成的汇编代码，我们可以确保生成了预期的底层指令。

除了基本的加载/存储，多GPU编程还需要同步机制。Iris实现了一系列**原子操作**（如 `atomic_cas`, `atomic_add`, `atomic_max` 等）。

![](img/6651019c42b2811f2ce4d85f264eba3a_55.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_57.png)

原子操作对于实现生产者-消费者模式至关重要。例如，生产者完成数据写入后，通过一个带有`release`语义的原子操作设置标志位；消费者则通过一个带有`acquire`语义的原子操作轮询该标志位，确保在数据准备好后才进行读取。这保证了跨GPU的内存操作顺序和可见性。

![](img/6651019c42b2811f2ce4d85f264eba3a_59.png)

`scope`参数（如 `system`, `gpu`, `workgroup`）定义了同步的范围。对于跨GPU通信，通常使用 `scope=system`。

---

![](img/6651019c42b2811f2ce4d85f264eba3a_61.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_63.png)

## Hello World 示例：生产者-消费者模式 👋

![](img/6651019c42b2811f2ce4d85f264eba3a_65.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_66.png)

现在，我们通过一个具体的“Hello World”示例来展示Iris的核心编程模式。这是理解Iris多GPU同步的基础。

![](img/6651019c42b2811f2ce4d85f264eba3a_68.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_70.png)

以下是实现跨GPU生产者-消费者模式的关键步骤：

![](img/6651019c42b2811f2ce4d85f264eba3a_72.png)

**1. 初始化**
```python
import torch.distributed as dist
dist.init_process_group(...) # 使用torch.distributed初始化
iris_instance = iris.Iris(...) # 构造Iris实例，创建对称堆

![](img/6651019c42b2811f2ce4d85f264eba3a_74.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_75.png)

# 从对称堆中分配张量（返回的是普通的PyTorch张量）
input_tensor = iris.full(...)
flag_tensor = iris.zeros(...)
```
**注意**：对通过Iris API分配的、用于远程访问的张量进行操作时，应尽量使用原地操作，以避免PyTorch重新分配内存，导致张量离开对称堆。

![](img/6651019c42b2811f2ce4d85f264eba3a_77.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_79.png)

**2. 启动内核**
```python
# Rank 0 启动生产者内核
if dist.get_rank() == 0:
    producer_kernel[grid](input_tensor, flag_tensor, ...)
# Rank 1 启动消费者内核
else:
    consumer_kernel[grid](input_tensor, flag_tensor, ...)
```

![](img/6651019c42b2811f2ce4d85f264eba3a_81.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_83.png)

**3. 生产者内核（Rank 0）**
```python
@triton.jit
def producer_kernel(input_ptr, flag_ptr, ...):
    # ... 进行一些计算，准备数据 ...
    # 1. 将数据存储到远程GPU（Rank 1）
    iris.store(data_to_send, remote_rank=1, heap_bases=...)
    # 2. 使用 release 语义原子操作设置标志位，通知消费者数据就绪
    iris.atomic_xchg(flag_ptr, value=1, remote_rank=1, semantics="release", scope="system")
```
`release`语义确保之前的存储操作在设置标志位之前对消费者可见。

**4. 消费者内核（Rank 1）**
```python
@triton.jit
def consumer_kernel(input_ptr, flag_ptr, ...):
    # 1. 使用 acquire 语义原子操作轮询标志位
    while iris.atomic_cas(flag_ptr, expected=0, desired=0, remote_rank=0, semantics="acquire", scope="system") == 0:
        pass # 等待
    # 2. 标志位变为1后，从远程GPU（Rank 0）加载数据
    received_data = iris.load(input_ptr, remote_rank=0, heap_bases=...)
    # ... 消费数据 ...
```
`acquire`语义确保在读取到标志位变化后，才加载生产者写入的数据。

![](img/6651019c42b2811f2ce4d85f264eba3a_85.png)

这个“释放-获取”原子操作对，是跨GPU实现正确同步的核心模式。

![](img/6651019c42b2811f2ce4d85f264eba3a_87.png)

---

## 计算-通信重叠模式 🧩

![](img/6651019c42b2811f2ce4d85f264eba3a_89.png)

仅仅实现功能是不够的，要获得最佳性能，必须实现**计算与通信的重叠**。Iris允许你快速探索多种重叠模式。我们以 **GEMM + All-Scatter** 为例进行说明。

![](img/6651019c42b2811f2ce4d85f264eba3a_91.png)

以下是几种不同的实现模式：

![](img/6651019c42b2811f2ce4d85f264eba3a_93.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_95.png)

**1. 非融合的批量同步模式**
*   **描述**：先启动一个GEMM内核使用所有SM进行计算，计算完成后通过屏障同步，再启动一个All-Scatter内核进行通信。
*   **特点**：类似于传统RCCL方式。实现简单，但存在“走走停停”的缺点，GPU利用率有间隙。
*   **性能**：有时能超越通用库（如PyTorch `torch.matmul` + RCCL），因为Iris允许为特定形状编写定制化内核。

![](img/6651019c42b2811f2ce4d85f264eba3a_97.png)

**2. 非融合的生产者-消费者模式**
*   **描述**：将GPU的SM划分为两部分。一部分SM运行生产者内核（GEMM），另一部分SM运行消费者内核（通信）。两者并发执行在不同的CUDA流上。生产者每完成一个数据块（Tile）就释放一个锁，消费者获取锁后立即进行通信。
*   **特点**：实现了计算与通信的流水线重叠。关键挑战在于如何最优地在计算和通信之间划分SM资源。
*   **性能**：可获得显著的性能提升（例如2.5倍加速比）。

![](img/6651019c42b2811f2ce4d85f264eba3a_99.png)

**3. 融合的顺序执行模式**
*   **描述**：编写单个内核。每个线程块先完成自己的GEMM计算，将结果保存在寄存器中，然后立即执行All-Scatter操作将结果发送出去。
*   **特点**：避免了将中间结果写回全局内存（HBM），数据直接在寄存器间通信。实现代码简洁。
*   **挑战**：可能增加寄存器压力，并导致“尾部效应”——即某些线程块先完成所有工作而闲置，造成GPU利用率下降。
*   **性能**：结果因问题规模和实现细节而异，有时很好，有时则不理想。

**4. 融合的工作组专业化模式**
*   **描述**：启动单个内核。在内核开头，根据`blockIdx`决定线程块的角色：一部分线程块作为“生产者”专用于计算，另一部分作为“消费者”专用于通信。它们通过原子锁在同一个内核内进行同步。
*   **特点**：兼具融合内核的简洁性和生产者-消费者模式的并发性。是团队最喜欢的模式之一。
*   **性能**：通常能获得比批量同步模式更好的性能（例如高达60%的加速比），但同样需要仔细权衡计算与通信资源的分配。

**关于资源分配的思考**：确定计算和通信之间的最佳分区是一个开放问题。可能的方法包括：基于分析的编译器模型、运行时工作队列（动态分配任务），或者针对特定已知形状进行微基准测试和网格搜索。

Iris的目标不是提供单一的“最佳”模式，而是提供底层API，赋能开发者快速探索和实现所有这些模式及其变体，从而找到针对特定问题的最佳解决方案。

---

## 未来展望与社区共建 🌟

![](img/6651019c42b2811f2ce4d85f264eba3a_101.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_103.png)

Iris不仅关注单节点扩展，也从设计之初就考虑了多节点扩展。我们计划引入`world`内存域，并继续提供低级的RMA/RDMA API。同时，我们也在构建更高级的集合通信示例，作为最佳实践供用户参考。

![](img/6651019c42b2811f2ce4d85f264eba3a_105.png)

我们正在以下方面持续努力：
*   **库开发**：改进文档、测试、CI，并实现多节点扩展。
*   **应用集成**：非常希望与推理框架（如vLLM）集成，探索端到端的工作流。
*   **丰富示例**：我们已经实现了FlashAttention、All-Gather+GEMM等示例，并欢迎社区贡献更多领域的应用（如生物信息学）。

![](img/6651019c42b2811f2ce4d85f264eba3a_107.png)

**Iris是为谁而建？**
我们最关心的是那些参加此类讲座的“GPU编程高手”、为兴趣而编码的爱好者以及进行前沿研究的研究人员。Iris旨在赋能你们去实现那些新颖、有趣、处于技术前沿的想法。

**加入我们！**
Iris将始终保持开源。我们希望通过GitHub的Issues、Pull Requests和Discussions与社区共同驱动项目发展。如果你有任何想法、问题，或者想实现某个内核但需要指导，我们都非常乐意合作。我们甚至录制了教学视频，并愿意应大家的要求制作更多深入讲解的内容。

![](img/6651019c42b2811f2ce4d85f264eba3a_109.png)

---

## 总结 🎓

![](img/6651019c42b2811f2ce4d85f264eba3a_111.png)

本节课中，我们一起学习了多GPU编程框架Iris。我们从其设计理念和与传统方法的对比入手，深入探讨了其核心的对称堆模型和简洁的API设计。通过“Hello World”示例，我们掌握了跨GPU同步的核心模式。最后，我们分析了多种实现计算-通信重叠的模式，并看到了Iris在赋能快速性能探索方面的潜力。

![](img/6651019c42b2811f2ce4d85f264eba3a_113.png)

![](img/6651019c42b2811f2ce4d85f264eba3a_115.png)

Iris的核心价值在于，它通过一组精巧的低级API，将Triton优雅的单GPU编程体验扩展到了多GPU领域，让开发者能够以熟悉的范式，轻松探索高性能并发计算的广阔空间。