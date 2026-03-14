# 008：GPU中的共享内存 🧠

![](img/2a3df8b485a890f6aeea02b32796c9af_1.png)

## 概述
在本节课中，我们将要学习CUDA编程中一个非常强大的特性：共享内存。我们将了解什么是共享内存、如何声明和使用共享内存变量，以及为什么它在某些场景下至关重要。我们还将通过示例来探讨使用共享内存时可能遇到的同步问题。

---

![](img/2a3df8b485a890f6aeea02b32796c9af_3.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_5.png)

## 共享内存简介

![](img/2a3df8b485a890f6aeea02b32796c9af_7.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_9.png)

上一节我们介绍了内存合并（Memory Coalescing）的概念，它对于提升GPU内存访问效率至关重要。本节中，我们来看看另一种可以显著提升性能的内存：共享内存。

共享内存是GPU上L1缓存的一部分，但它与CPU缓存有一个关键区别：**共享内存是可由程序员显式控制的**。在CPU上，数据如何进入L1、L2、L3缓存完全由硬件管理，程序员无法干预。而在GPU上，程序员可以指定哪些数据应该存放在共享内存中，这为我们优化程序提供了极大的灵活性。

共享内存主要有两个关键特性：
1.  **按线程块分配**：每个线程块（Thread Block）都拥有自己独立的一块共享内存。即使多个线程块被调度到同一个流式多处理器（SM）上执行，它们各自的共享内存也是隔离的。
2.  **线程块内共享**：在同一个线程块内的所有线程，可以看到并修改共享内存中的同一份数据副本。这使得共享内存成为线程块内线程间通信和协作的理想场所。

共享内存通常用于以下两种情况：
*   **数据重用**：当线程块内需要反复访问一小块数据时，将其加载到共享内存中可以极大降低访问延迟。
*   **线程协作**：当需要在线程块内的线程之间进行同步或协调时（例如，实现一个屏障或归约操作），共享内存可以作为共享的工作空间。

---

![](img/2a3df8b485a890f6aeea02b32796c9af_11.png)

## 如何声明共享内存

![](img/2a3df8b485a890f6aeea02b32796c9af_13.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_15.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_17.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_19.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_21.png)

在CUDA内核（Kernel）中声明共享内存变量非常简单，只需在变量声明前加上 `__shared__` 限定符即可。

![](img/2a3df8b485a890f6aeea02b32796c9af_23.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_25.png)

以下是声明共享内存数组和变量的语法：

![](img/2a3df8b485a890f6aeea02b32796c9af_27.png)

```cuda
// 在内核函数中声明一个共享内存数组
__shared__ float shared_array[1024];

![](img/2a3df8b485a890f6aeea02b32796c9af_29.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_31.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_32.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_34.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_36.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_37.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_39.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_41.png)

// 在内核函数中声明一个共享内存变量
__shared__ unsigned int shared_variable;
```

![](img/2a3df8b485a890f6aeea02b32796c9af_43.png)

**重要说明**：
*   这些声明必须写在CUDA内核函数内部。
*   如果声明时不加 `__shared__`，例如 `float local_array[1024];`，那么这个数组将是线程局部的（Thread-local），每个线程都有自己的副本，线程间无法直接访问彼此的数据。
*   加上 `__shared__` 后，该变量在线程块内是共享的。例如，一个包含1024个线程的线程块，所有线程都操作 `shared_array` 的同一份物理存储。

![](img/2a3df8b485a890f6aeea02b32796c9af_45.png)

---

![](img/2a3df8b485a890f6aeea02b32796c9af_47.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_49.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_51.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_53.png)

## 示例与分析：理解共享内存的并发访问

![](img/2a3df8b485a890f6aeea02b32796c9af_55.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_57.png)

为了深入理解共享内存的共享特性以及可能引发的并发问题，我们来看一个具体的代码示例。

假设我们启动一个内核，只使用一个线程块，该线程块包含1024个线程。内核代码如下：

![](img/2a3df8b485a890f6aeea02b32796c9af_59.png)

```cuda
__global__ void myKernel() {
    __shared__ int s;
    int i = threadIdx.x;

    if (i == 0) {
        s = 0;          // 语句A：线程0初始化s为0
    }
    if (i == 1) {
        s += 1;         // 语句B：线程1给s加1
    }
    if (i == 100) {
        s += 2;         // 语句C：线程100给s加2
    }
    if (i == 0) {
        printf("%d\n", s); // 语句D：线程0打印s的值
    }
}
```

![](img/2a3df8b485a890f6aeea02b32796c9af_61.png)

我们需要思考：**这个程序可能的输出结果是什么？**

### 关键概念回顾：线程束（Warp）与锁步执行
在分析之前，必须回忆GPU的执行模型：线程以32个为一组（称为一个Warp）进行调度。**同一个Warp内的线程是“锁步”（Lock-step）执行的**，即它们同时执行同一条指令（尽管可能因为分支而产生部分线程不活跃的情况）。

在我们的例子中：
*   线程0和线程1属于同一个Warp（Warp 0）。
*   线程100属于另一个Warp（Warp 3）。

![](img/2a3df8b485a890f6aeea02b32796c9af_63.png)

### 并发执行与可能的输出
由于不同Warp之间的执行顺序是不确定的，语句A、B、C、D可能以多种方式交织执行，导致最终 `s` 的值不确定。以下是几种可能的执行顺序及其结果：

![](img/2a3df8b485a890f6aeea02b32796c9af_65.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_67.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_69.png)

1.  **顺序执行 (A -> B -> C -> D)**：`s` 最终为 0 + 1 + 2 = 3。输出 **3**。
2.  **线程100先执行 (C -> A -> B -> D)**：线程100读取未初始化的 `s`（垃圾值），加2后写入。随后A将其覆盖为0，B加1。最终 `s` 为 1。输出 **1**。
3.  **A和C并发执行产生竞争**：线程0执行 `s=0` 和线程100执行 `s+=2` 同时发生。`s+=2` 操作在底层可能被分解为“读-改-写”三步。如果线程100读取了垃圾值，计算了垃圾值+2，但在写入前，线程0写入了0，线程1写入了1，最后线程100的写入覆盖了结果。最终 `s` 可能为 **垃圾值+2**。
4.  **另一种竞争情况**：类似情况3，但线程1的 `s+=1` 在线程100最终写入后才执行，最终 `s` 可能为 **垃圾值+3**。
5.  **B和C并发执行**：线程1的 `s+=1` 和线程100的 `s+=2` 并发执行，且 `s` 已被初始化为0。如果线程100的写入后发生，`s` 最终为 2。输出 **2**。

![](img/2a3df8b485a890f6aeea02b32796c9af_71.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_73.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_75.png)

**结论**：这个简单的程序由于存在数据竞争（Data Race），可能产生多种输出（如1, 2, 3, 垃圾值+2, 垃圾值+3），其行为是**非确定性的**。

这个例子清晰地展示了当多个线程不加协调地访问共享内存时会导致的问题。要获得确定性的、正确的结果，我们必须引入**同步机制**。

![](img/2a3df8b485a890f6aeea02b32796c9af_77.png)

---

![](img/2a3df8b485a890f6aeea02b32796c9af_79.png)

## 共享内存的典型使用模式

![](img/2a3df8b485a890f6aeea02b32796c9af_81.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_83.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_85.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_87.png)

让我们回到一个更实际的例子，看看共享内存如何被应用，以及同步为何必不可少。

**问题**：有一个1024x1024的矩阵 `M`。我们想对每个元素 `M[i][j]` 进行如下操作：`M[i][j] = M[i][j] + M[i][j+1]`（对最后一列不操作）。我们计划将每一行分配给一个线程块（共1024个线程块），每个线程块内的1024个线程处理该行的1024个元素。

![](img/2a3df8b485a890f6aeea02b32796c9af_89.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_91.png)

为了优化性能，我们决定将一行数据从全局内存加载到共享内存中进行计算。实现步骤可分为三步：

![](img/2a3df8b485a890f6aeea02b32796c9af_93.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_95.png)

1.  **声明并加载数据**：在线程块内声明一个共享内存数组，然后将全局内存中对应行的数据复制进来。
2.  **执行计算**：所有线程使用共享内存中的数据执行 `shared_data[j] = shared_data[j] + shared_data[j+1]`。
3.  **写回结果**：将计算后的共享内存数据写回全局内存的对应行。

![](img/2a3df8b485a890f6aeea02b32796c9af_97.png)

### 这个流程中存在两个关键的同步问题：

![](img/2a3df8b485a890f6aeea02b32796c9af_99.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_101.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_103.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_105.png)

**问题一：步骤间的屏障**
*   在步骤1和步骤2之间，必须确保**线程块内所有线程**都已完成数据加载，才能开始计算。否则，某些线程可能还在加载数据，而另一些线程已经开始使用未完全加载的数据进行计算。
*   同理，在步骤2和步骤3之间，必须确保**所有线程**都已完成计算，才能将结果写回全局内存。

![](img/2a3df8b485a890f6aeea02b32796c9af_107.png)

**问题二：计算步骤中的数据竞争**
*   在步骤2中，线程 `j` 需要读取 `shared_data[j]` 和 `shared_data[j+1]`。而线程 `j+1` 同时也在读取 `shared_data[j+1]` 和 `shared_data[j+2]`。如果操作是就地更新（in-place update），并且没有同步，就可能发生我们前面例子中类似的竞争条件，导致结果错误。

![](img/2a3df8b485a890f6aeea02b32796c9af_109.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_111.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_113.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_115.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_117.png)

为了解决这些问题，CUDA提供了 `__syncthreads()` 函数。它是一个**线程块级别的屏障**，调用该函数会阻塞线程块内的所有线程，直到每个线程都到达这个调用点，然后所有线程才能继续执行后续指令。

![](img/2a3df8b485a890f6aeea02b32796c9af_119.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_121.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_123.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_125.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_127.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_129.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_131.png)

一个正确的、使用了同步的伪代码结构如下：
```cuda
__global__ void matrixKernel(float* M) {
    __shared__ float row_data[1024];
    int tid = threadIdx.x;
    int row_start = blockIdx.x * 1024;

    // 步骤1：协作加载数据
    row_data[tid] = M[row_start + tid];
    __syncthreads(); // 等待所有线程加载完毕

    // 步骤2：执行计算（注意处理边界）
    if (tid < 1023) { // 最后一列不计算
        float temp = row_data[tid] + row_data[tid + 1];
        __syncthreads(); // 确保所有读取操作完成后再写入
        row_data[tid] = temp;
    }
    __syncthreads(); // 等待所有计算完成

    // 步骤3：写回结果
    M[row_start + tid] = row_data[tid];
}
```
> **注意**：实际处理此类相邻元素依赖的计算时，模式可能更复杂（例如使用双缓冲），但此示例展示了同步的基本用法。

![](img/2a3df8b485a890f6aeea02b32796c9af_133.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_135.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_137.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_139.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_141.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_143.png)

---

![](img/2a3df8b485a890f6aeea02b32796c9af_145.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_147.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_148.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_149.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_151.png)

## 总结

![](img/2a3df8b485a890f6aeea02b32796c9af_153.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_155.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_157.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_159.png)

本节课中我们一起学习了CUDA中共享内存的核心知识：

![](img/2a3df8b485a890f6aeea02b32796c9af_161.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_163.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_165.png)

1.  **共享内存的本质**：它是GPU上可由程序员控制的高速、片上内存，以线程块为单位进行分配和管理。
2.  **声明方法**：使用 `__shared__` 限定符在内核中声明变量或数组。
3.  **核心价值**：用于**数据重用**以降低延迟，以及实现**线程块内的线程间协作与通信**。
4.  **关键挑战**：并发访问共享内存会引入**数据竞争**，导致非确定性和错误的结果。
5.  **解决方案**：必须使用 `__syncthreads()` 等同步原语来协调线程块内线程的执行顺序，确保在关键操作点（如数据加载后、计算后）所有线程达成一致。

![](img/2a3df8b485a890f6aeea02b32796c9af_167.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_169.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_171.png)

![](img/2a3df8b485a890f6aeea02b32796c9af_173.png)

理解并正确使用共享内存与同步，是编写高效、正确CUDA并行程序的关键一步。在下一节课中，我们将更详细地探讨 `__syncthreads()` 以及其他同步机制。