# 27：FlashAttention 4 工作原理详解

在本教程中，我们将深入探讨FlashAttention 4的工作原理。FlashAttention是Transformer模型中注意力计算的核心优化内核，其第四代版本针对NVIDIA Blackwell架构（B200/SM10.0 GPU）进行了重大优化，实现了PetaFLOPS级别的性能。我们将从FlashAttention的发展历程讲起，逐步解析FlashAttention 4的关键创新点，包括其五级流水线设计、更智能的SoftMax缩放策略以及软件模拟指数计算等核心技术。

---

## FlashAttention 发展回顾

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_1.png)

上一节我们介绍了本教程的概述，本节中我们来看看FlashAttention系列的发展历程，理解其核心思想的演进。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_3.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_5.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_6.png)

### FlashAttention 1：在线SoftMax与分块计算

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_8.png)

FlashAttention 1的核心贡献是将**分块计算**策略应用于注意力计算，并引入了**在线SoftMax算法**以解决数值稳定性问题。

标准的注意力计算涉及一个序列长度乘以序列长度的大型矩阵（QK^T），随后应用SoftMax，再进行矩阵乘法（与V）。硬件原生实现会创建这个大矩阵，导致巨大的内存开销。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_10.png)

FlashAttention 1的关键在于将计算分解为多个小块（Tile）进行处理。它采用在线SoftMax算法，在计算过程中动态减去当前遇到的最大值，以保持数值稳定，并在后续步骤中对之前的输出进行重新缩放。其核心公式如下：

**在线SoftMax公式**：
对于每个新的输入块，更新运行最大值 `m_new = max(m_old, max(current_tile))`，并相应地缩放之前的累加和与输出。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_12.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_14.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_15.png)

### FlashAttention 2：查询并行与性能提升

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_17.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_19.png)

FlashAttention 2的主要优化是改变了计算的组织方式，采用了**查询并行**策略。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_21.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_22.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_24.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_26.png)

在FlashAttention 2及之后的版本中，每个线程块（或线程束组）负责处理一个查询块（Query Tile），但需要扫描所有的键（Key）和值（Value）块。这种结构使得输入输出与线程块/线程束的映射关系更加清晰，从而在Ampere架构上实现了更高的计算利用率。

### FlashAttention 3：异步性与线程束专业化

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_28.png)

FlashAttention 3为Hopper架构（如H100）引入了两项关键技术：**异步性**和**线程束专业化**。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_30.png)

*   **异步性**：允许在Tensor Core执行矩阵乘法或Tensor Memory Accelerator加载数据的同时，执行其他计算，从而更好地隐藏延迟。
*   **线程束专业化**：在一个线程块内，将线程束分为不同的角色。例如，一些线程束专门负责从全局内存加载数据到共享内存（生产者），而另一些线程束专门负责计算（消费者）。这得益于Hopper架构的动态寄存器分配功能，允许为不同角色的线程束分配不同数量的寄存器。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_32.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_34.png)

---

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_36.png)

## FlashAttention 4 核心创新

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_38.png)

上一节我们回顾了FlashAttention系列的基础，本节中我们来看看针对Blackwell架构的FlashAttention 4带来了哪些关键性的革新。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_40.png)

### 更复杂的五级流水线与线程束角色

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_42.png)

FlashAttention 4将异步流水线设计发挥到极致，引入了**五个专门的线程束角色**，构成了一个复杂的生产者-消费者管道。

以下是流水线中的主要角色：
1.  **加载线程束**：负责将输入数据（Q, K, V）从全局内存加载到共享内存。
2.  **矩阵乘线程束**：负责执行Q*K^T和Attention权重*V的矩阵乘法。在Blackwell上，单个线程束即可驱动Tensor Core进行高效计算。
3.  **SoftMax线程束**：负责对注意力分数应用SoftMax操作，并判断是否需要触发重新缩放。
4.  **校正线程束**：当SoftMax线程束判定需要时，负责对之前的中间结果进行重新缩放校正。
5.  **存储线程束**：负责将最终的计算结果写回全局内存。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_44.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_46.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_47.png)

这种设计需要程序员显式地使用内存屏障进行同步，以管理不同阶段对共享缓冲区的访问，避免数据竞争。为了隐藏延迟，每个线程块通常会同时处理两个查询块，使得流水线更加饱满。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_49.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_51.png)

### 更智能的SoftMax缩放策略

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_53.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_55.png)

FlashAttention 4优化了在线SoftMax算法中的重新缩放逻辑。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_57.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_59.png)

在FlashAttention 1-3中，每当遇到新的最大值时，都需要对之前的所有相关结果进行重新缩放，以确保数值稳定。FlashAttention 4引入了一个**重新缩放阈值**。只有当新最大值与旧最大值的差异足够大，以至于可能影响数值精度时，才触发重新缩放操作。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_61.png)

**代码逻辑示意**：
```cpp
float scale = exp2f(old_max - new_max);
if (scale < rescale_threshold) {
    // 触发重新缩放
    trigger_rescale();
}
```
这种方法可以显著减少重新缩放的次数（根据Hot Chips演示，可达10倍），从而减少了线程束间的协调开销。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_63.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_64.png)

### 软件模拟指数计算

在SoftMax中，指数计算是一个关键但非矩阵乘法的操作，通常由专用的特殊函数单元执行。在Blackwell架构上，SFU可能成为瓶颈。

FlashAttention 4的解决方案是：在部分迭代中，使用**CUDA核心进行软件模拟的指数计算**，以分担SFU的压力。它采用一个在[0,1]区间内精度很高的三次多项式来近似指数函数的小数部分。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_66.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_68.png)

**软件指数近似代码片段（概念性）**：
```ptx
// 使用CUDA核心的FMA指令进行多项式计算
// 计算 exp2f(x) 的近似值，其中x已被规范到[0,1]
```
该实现与硬件指令在BF16精度下是比特位一致的。内核会根据当前迭代的负载情况，动态决定是否启用软件模拟，通常在计算密集阶段使用，而在负载较轻的末尾阶段则切换回硬件指令。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_70.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_71.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_72.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_73.png)

---

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_75.png)

## 性能对比与总结

本节课中我们一起学习了FlashAttention 4的核心机制。作为总结，我们来看一下它的性能表现及其意义。

根据公开数据，在Blackwell B200上，FlashAttention 4的性能大约是前代FlashAttention 3在A100上的两倍，比原始FlashAttention快15倍。其计算利用率达到约30%，处于业界先进水平。与NVIDIA官方cuDNN库中的注意力实现相比，FlashAttention 4约有5%-20%的性能优势。

FlashAttention 4目前仅支持前向传播和BF16精度，反向传播和更低精度的支持仍在开发中。它支持分页键值张量（Paged KV Tensors）和注意力同步（Attention Sync）等特性，使其更适合长上下文和大型模型训练场景。

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_77.png)

![](img/5cb94cecfa0019d6bff03e9744a0c0ad_79.png)

FlashAttention 4的出现也反映了GPU高性能编程的一个趋势：为了极致压榨硬件性能（尤其是Tensor Core），编程模型正变得更加复杂和专业化。程序员需要更精细地控制流水线、线程束角色和异步操作。这推动了像CUTLASS、Triton等基于Tile的领域特定语言的发展，它们试图在编程便利性和性能之间找到新的平衡点。