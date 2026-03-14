# 003：CUDA中的线程组织与线程块 🧵

![](img/68e31b967d69cfea77cc142db62cac46_0.png)

## 概述
在本节课中，我们将深入学习CUDA中的线程组织模型。我们将探讨线程网格、线程块和线程的层次结构，理解如何通过多维索引来唯一标识每个线程，并通过实例学习如何为一维和二维数据结构编写并行初始化代码。

---

![](img/68e31b967d69cfea77cc142db62cac46_2.png)

## 线程组织的层次结构

![](img/68e31b967d69cfea77cc142db62cac46_4.png)

在上一讲中，我们学习了典型的CUDA程序流程。本节中，我们来看看CUDA如何组织海量的线程来执行并行计算。

![](img/68e31b967d69cfea77cc142db62cac46_6.png)

![](img/68e31b967d69cfea77cc142db62cac46_8.png)

![](img/68e31b967d69cfea77cc142db62cac46_10.png)

当一个内核被启动时，它会以一个**线程网格**的形式执行。这个网格是一个三维数据结构，可以将其想象成一个**魔方**。整个魔方就是网格，而构成魔方的每一个小立方体就是一个**线程块**。

![](img/68e31b967d69cfea77cc142db62cac46_12.png)

*   **网格**：是内核启动的最高层次组织，由线程块构成。
*   **线程块**：是网格的组成部分，内部包含线程。
*   **线程**：是实际执行计算的最小单元，位于线程块内部。

网格和线程块都可以是一维、二维或三维的，这为处理不同维度的数据（如图像、矩阵、体积数据）提供了灵活性。

![](img/68e31b967d69cfea77cc142db62cac46_14.png)

![](img/68e31b967d69cfea77cc142db62cac46_16.png)

---

## 标识线程与线程块

![](img/68e31b967d69cfea77cc142db62cac46_18.png)

为了在并行计算中准确定位每个线程，我们需要一套坐标系统。

### 网格维度与块索引
网格的尺寸（即每个维度上有多少个线程块）可以通过内置变量访问：
*   `gridDim.x`：网格在x方向的尺寸（线程块数量）。
*   `gridDim.y`：网格在y方向的尺寸。
*   `gridDim.z`：网格在z方向的尺寸。

![](img/68e31b967d69cfea77cc142db62cac46_20.png)

网格中每个线程块的位置由块索引唯一确定：
*   `blockIdx.x`：当前线程块在网格x方向上的索引。
*   `blockIdx.y`：当前线程块在网格y方向上的索引。
*   `blockIdx.z`：当前线程块在网格z方向上的索引。

**索引范围**：如果`gridDim.x = 10`，那么`blockIdx.x`的取值范围是 `0` 到 `9`。

### 线程块维度与线程索引
每个线程块的尺寸（即每个维度上有多少个线程）可以通过内置变量访问：
*   `blockDim.x`：线程块在x方向的尺寸（线程数量）。
*   `blockDim.y`：线程块在y方向的尺寸。
*   `blockDim.z`：线程块在z方向的尺寸。

线程块中每个线程的位置由线程索引唯一确定：
*   `threadIdx.x`：当前线程在线程块x方向上的索引。
*   `threadIdx.y`：当前线程在线程块y方向上的索引。
*   `threadIdx.z`：当前线程在线程块z方向上的索引。

![](img/68e31b967d69cfea77cc142db62cac46_22.png)

![](img/68e31b967d69cfea77cc142db62cac46_24.png)

![](img/68e31b967d69cfea77cc142db62cac46_26.png)

**索引范围**：如果`blockDim.x = 5`，那么`threadIdx.x`的取值范围是 `0` 到 `4`。

![](img/68e31b967d69cfea77cc142db62cac46_28.png)

![](img/68e31b967d69cfea77cc142db62cac46_30.png)

![](img/68e31b967d69cfea77cc142db62cac46_32.png)

---

![](img/68e31b967d69cfea77cc142db62cac46_34.png)

![](img/68e31b967d69cfea77cc142db62cac46_36.png)

![](img/68e31b967d69cfea77cc142db62cac46_38.png)

![](img/68e31b967d69cfea77cc142db62cac46_40.png)

## 内核启动配置示例

以下代码展示了如何配置并启动一个多维的内核：

![](img/68e31b967d69cfea77cc142db62cac46_42.png)

```c
dim3 grid(2, 3, 4); // 定义一个2x3x4的网格（共24个线程块）
dim3 block(5, 6, 7); // 定义一个5x6x7的线程块（共210个线程）

![](img/68e31b967d69cfea77cc142db62cac46_44.png)

// 启动内核，共创建 24 * 210 = 5040 个线程
myKernel<<<grid, block>>>();
cudaDeviceSynchronize();
```

在内核函数`myKernel`内部，我们可以使用上述变量。例如，以下条件只会在一个特定的线程（位于第一个线程块的第一个线程）中为真：

![](img/68e31b967d69cfea77cc142db62cac46_46.png)

![](img/68e31b967d69cfea77cc142db62cac46_48.png)

```c
if (blockIdx.x == 0 && blockIdx.y == 0 && blockIdx.z == 0 &&
    threadIdx.x == 0 && threadIdx.y == 0 && threadIdx.z == 0) {
    // 只有这个线程会执行这里的代码
}
```

---

![](img/68e31b967d69cfea77cc142db62cac46_50.png)

## 应用实例：计算全局唯一线程ID

![](img/68e31b967d69cfea77cc142db62cac46_52.png)

![](img/68e31b967d69cfea77cc142db62cac46_54.png)

![](img/68e31b967d69cfea77cc142db62cac46_56.png)

在实际编程中，我们经常需要为每个线程计算一个在整个网格范围内唯一的ID，以便处理像数组这样的线性内存。

### 情况一：单线程块，二维线程组织
假设我们有一个大小为 `N * M` 的一维数组，我们使用一个二维线程块（`N x M` 个线程）来初始化它。

![](img/68e31b967d69cfea77cc142db62cac46_58.png)

![](img/68e31b967d69cfea77cc142db62cac46_60.png)

![](img/68e31b967d69cfea77cc142db62cac46_61.png)

![](img/68e31b967d69cfea77cc142db62cac46_63.png)

![](img/68e31b967d69cfea77cc142db62cac46_65.png)

内核启动配置：
```c
#define N 5
#define M 6
dim3 block(N, M); // 5x6的线程块
myKernel<<<1, block>>>(matrix); // 网格中只有1个线程块
```

![](img/68e31b967d69cfea77cc142db62cac46_67.png)

在内核中，计算唯一ID的公式为：
```c
int i = threadIdx.x * blockDim.y + threadIdx.y;
matrix[i] = i; // 为每个位置赋予唯一值
```
**公式解释**：`threadIdx.x` 是行索引，`blockDim.y`（固定为6）是总列数，`threadIdx.y` 是列索引。该公式将二维索引映射到一维数组。

### 情况二：多线程块，一维线程组织
同样初始化一个 `N * M` 的数组，现在我们使用多个一维线程块。每个线程块包含 `M` 个线程，总共启动 `N` 个线程块。

内核启动配置：
```c
#define N 5
#define M 6
myKernel<<<N, M>>>(matrix); // N个块，每个块M个线程
```

![](img/68e31b967d69cfea77cc142db62cac46_69.png)

![](img/68e31b967d69cfea77cc142db62cac46_71.png)

![](img/68e31b967d69cfea77cc142db62cac46_73.png)

在内核中，计算唯一ID的公式变为：
```c
int i = blockIdx.x * blockDim.x + threadIdx.x;
matrix[i] = i;
```
**公式解释**：`blockIdx.x` 是线程块索引，`blockDim.x`（固定为6）是每个块的线程数，`threadIdx.x` 是块内线程索引。该公式结合了块索引和块内线程索引来生成全局唯一ID。

这两种方法都能成功地将数组初始化为 `[0, 1, 2, ..., 29]`，展示了组织线程的灵活性。

---

![](img/68e31b967d69cfea77cc142db62cac46_75.png)

![](img/68e31b967d69cfea77cc142db62cac46_77.png)

## 总结

本节课我们一起学习了CUDA中线程组织的核心概念：
1.  CUDA执行模型是分层的：**网格(Grid) -> 线程块(Block) -> 线程(Thread)**。
2.  网格和线程块都可以配置为**一维、二维或三维**结构，以适应不同维度的数据。
3.  使用 `blockIdx`、`threadIdx`、`gridDim`、`blockDim` 这些内置变量可以唯一标识每个线程并获取执行配置信息。
4.  关键在于**计算全局唯一的线程ID**，其公式取决于网格和线程块的维度配置。我们通过两个初始化数组的实例演示了如何推导和应用这个公式。

![](img/68e31b967d69cfea77cc142db62cac46_79.png)

![](img/68e31b967d69cfea77cc142db62cac46_81.png)

理解线程组织是编写高效CUDA并行程序的基础，它直接决定了任务如何被分解并映射到GPU的数千个核心上执行。