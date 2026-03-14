# 002：CUDA程序流程与cudaMemcpy操作 🚀

## 概述
在本节课中，我们将学习CUDA程序的基本流程，特别是CPU与GPU之间如何通过`cudaMemcpy`函数进行数据传输。我们将通过具体的代码示例，理解如何为GPU分配内存、启动内核以及将结果复制回CPU。

![](img/79f6aa2eef992bc8530785c087c51019_1.png)

![](img/79f6aa2eef992bc8530785c087c51019_3.png)

![](img/79f6aa2eef992bc8530785c087c51019_5.png)

---

![](img/79f6aa2eef992bc8530785c087c51019_7.png)

![](img/79f6aa2eef992bc8530785c087c51019_9.png)

![](img/79f6aa2eef992bc8530785c087c51019_11.png)

![](img/79f6aa2eef992bc8530785c087c51019_12.png)

![](img/79f6aa2eef992bc8530785c087c51019_14.png)

## 回顾与引入
上一节我们介绍了如何编写和启动一个简单的CUDA内核，并使用多个线程并行打印数字的平方。本节中，我们来看看如何将计算结果存储到数组中，并将其从GPU传回CPU。

我们将从一个简单的C语言串行代码开始，目标是将其并行化。

```c
// 串行C代码示例
for (int i = 0; i < 100; i++) {
    a[i] = i * i;
}
```

以下是实现相同功能的CUDA并行代码。

![](img/79f6aa2eef992bc8530785c087c51019_16.png)

![](img/79f6aa2eef992bc8530785c087c51019_18.png)

```c
#include <stdio.h>

![](img/79f6aa2eef992bc8530785c087c51019_20.png)

__global__ void square(int *a) {
    int tid = threadIdx.x;
    a[tid] = tid * tid;
}

![](img/79f6aa2eef992bc8530785c087c51019_22.png)

int main() {
    int n = 100;
    int a[n];
    int *d_a;
    cudaMalloc((void**)&d_a, n * sizeof(int));
    square<<<1, n>>>(d_a);
    cudaMemcpy(a, d_a, n * sizeof(int), cudaMemcpyDeviceToHost);
    for (int i = 0; i < n; i++) {
        printf("%d ", a[i]);
    }
    cudaFree(d_a);
    return 0;
}
```

### 代码解析
以下是上述代码的关键步骤解析：

![](img/79f6aa2eef992bc8530785c087c51019_24.png)

![](img/79f6aa2eef992bc8530785c087c51019_26.png)

1.  **主机（CPU）内存分配**：在CPU上声明数组 `a`。
2.  **设备（GPU）内存分配**：使用 `cudaMalloc` 在GPU上分配内存，`d_a` 是一个位于CPU上的指针，但它指向GPU上的内存地址。
3.  **内核启动**：使用 `<<<1, n>>>` 语法启动 `square` 内核，其中 `n` 是线程数。每个线程计算其 `threadIdx.x` 的平方，并写入GPU数组 `d_a` 的对应位置。
4.  **数据回传**：使用 `cudaMemcpy` 将计算结果从GPU内存 (`d_a`) 复制回CPU内存 (`a`)。第四个参数 `cudaMemcpyDeviceToHost` 指明了传输方向。
5.  **内存释放**：使用 `cudaFree` 释放GPU上分配的内存。

![](img/79f6aa2eef992bc8530785c087c51019_28.png)

### 核心概念与注意事项
在深入更多示例前，我们需要理解几个核心概念：

![](img/79f6aa2eef992bc8530785c087c51019_30.png)

![](img/79f6aa2eef992bc8530785c087c51019_32.png)

![](img/79f6aa2eef992bc8530785c087c51019_34.png)

*   **CPU与GPU内存分离**：CPU和GPU拥有独立的内存空间（显存）。在离散GPU上，CPU无法直接访问GPU内存，反之亦然。因此，必须显式地进行内存分配和数据拷贝。
*   **`cudaMemcpy` 是阻塞操作**：`cudaMemcpy` 调用会阻塞CPU线程，直到所有先前启动的内核执行完毕且数据传输完成。因此，在这个例子中，我们不需要显式调用 `cudaDeviceSynchronize()`。
*   **PCIe总线瓶颈**：CPU与GPU之间的数据传输通过PCI Express总线进行，其带宽有限。频繁或大量数据传输可能成为程序性能瓶颈。因此，应确保在GPU上进行足够的计算，以抵消数据传输的开销。

![](img/79f6aa2eef992bc8530785c087c51019_36.png)

![](img/79f6aa2eef992bc8530785c087c51019_38.png)

---

## CPU/GPU内存访问示例
为了加深对内存分离的理解，我们来看几个例子。

![](img/79f6aa2eef992bc8530785c087c51019_40.png)

### 示例1：错误的全局变量访问
以下代码尝试在内核中访问一个在CPU上定义的全局变量字符串指针，这将导致编译错误。

![](img/79f6aa2eef992bc8530785c087c51019_42.png)

![](img/79f6aa2eef992bc8530785c087c51019_44.png)

![](img/79f6aa2eef992bc8530785c087c51019_45.png)

```c
char *msg = "Hello World"; // 在CPU内存中

![](img/79f6aa2eef992bc8530785c087c51019_47.png)

![](img/79f6aa2eef992bc8530785c087c51019_49.png)

__global__ void hello() {
    printf("%s\n", msg); // 错误！GPU内核无法直接访问CPU内存
}

![](img/79f6aa2eef992bc8530785c087c51019_51.png)

int main() {
    hello<<<1, 32>>>();
    cudaDeviceSynchronize();
    return 0;
}
```
**关键点**：在离散GPU上，定义在主机（CPU）上的变量不能被设备（GPU）代码直接访问。

![](img/79f6aa2eef992bc8530785c087c51019_53.png)

![](img/79f6aa2eef992bc8530785c087c51019_55.png)

![](img/79f6aa2eef992bc8530785c087c51019_57.png)

![](img/79f6aa2eef992bc8530785c087c51019_59.png)

### 示例2：使用预处理器宏
如果使用预处理器宏 `#define`，代码则可以正常工作。

![](img/79f6aa2eef992bc8530785c087c51019_61.png)

```c
#define MSG "Hello World" // 预处理器指令，在编译前进行文本替换

![](img/79f6aa2eef992bc8530785c087c51019_63.png)

![](img/79f6aa2eef992bc8530785c087c51019_65.png)

__global__ void hello() {
    printf("%s\n", MSG); // 正确！编译前MSG已被替换为字符串字面量
}

int main() {
    hello<<<1, 32>>>();
    cudaDeviceSynchronize();
    return 0;
}
```
**关键点**：`#define` 是预处理器指令，它在编译之前将 `MSG` 直接替换为 `"Hello World"` 字符串字面量。对于内核来说，它只是在打印一个常量字符串，而非访问CPU内存地址。

![](img/79f6aa2eef992bc8530785c087c51019_67.png)

![](img/79f6aa2eef992bc8530785c087c51019_69.png)

---

## 典型的CUDA程序流程 📊
一个标准的CUDA程序通常遵循以下步骤，我们将其与所需的CUDA API对应起来：

1.  **从文件系统加载数据到CPU内存**：使用标准C/C++文件操作（如`fread`）。
2.  **将数据从CPU内存传输到GPU内存**：
    *   使用 `cudaMalloc` 在GPU上分配内存。
    *   使用 `cudaMemcpy(..., cudaMemcpyHostToDevice)` 将数据从主机复制到设备。
3.  **在GPU上执行内核**：使用 `kernel_name<<<grid_size, block_size>>>(parameters)` 启动内核。
4.  **将结果从GPU内存传输回CPU内存**：使用 `cudaMemcpy(..., cudaMemcpyDeviceToHost)`。
5.  **在CPU上使用或输出结果**：使用 `printf` 打印或 `fwrite` 写回文件。

**重要提示**：程序员有责任维护CPU和GPU上数据的一致性。通常，建议使用不同的变量名来区分它们（例如，`h_a` 表示主机数组，`d_a` 表示设备数组），以避免错误传递指针。

![](img/79f6aa2eef992bc8530785c087c51019_71.png)

---

## 实践练习：字符串操作与边界问题
让我们通过一个操作字符串的练习，来学习如何正确处理GPU内存和线程索引。

![](img/79f6aa2eef992bc8530785c087c51019_73.png)

### 任务描述
我们有一个字符串 `"GdkknVnqkc"`，每个字符在字母表中都是前一个字符（例如，‘H’的前一个字符是‘G’）。我们想用GPU线程并行地将每个字符递增，最终得到 `"HelloWorld"`。

### 有问题的初始代码
以下代码存在一个常见的边界错误。

```c
__global__ void decode(char *a, int len) {
    int tid = threadIdx.x;
    if (tid < len) {
        a[tid]++; // 所有线程都对字符进行加一操作
    }
}

int main() {
    char cpu_a[] = "GdkknVnqkc";
    int len = strlen(cpu_a);
    char *gpu_a;
    cudaMalloc((void**)&gpu_a, (len + 1) * sizeof(char)); // 分配len+1字节，包含'\0'
    cudaMemcpy(gpu_a, cpu_a, (len + 1) * sizeof(char), cudaMemcpyHostToDevice);
    decode<<<1, len + 1>>>(gpu_a, len + 1); // 启动了 len+1 个线程
    cudaMemcpy(cpu_a, gpu_a, (len + 1) * sizeof(char), cudaMemcpyDeviceToHost);
    printf("%s\n", cpu_a);
    cudaFree(gpu_a);
    return 0;
}
```
**问题分析**：字符串 `"GdkknVnqkc"` 长度为10，但C风格字符串以 `\0` 结尾，所以 `cpu_a` 实际占用11字节。代码分配了11字节，并启动了11个线程。第11个线程（`tid=10`）操作的是字符串终止符 `\0`，将其递增为 `\1`，导致字符串失去终止符。后续 `printf` 可能打印乱码或导致段错误。

![](img/79f6aa2eef992bc8530785c087c51019_75.png)

### 正确的代码
正确的做法是只对有效字符进行操作，不改变终止符。

![](img/79f6aa2eef992bc8530785c087c51019_77.png)

![](img/79f6aa2eef992bc8530785c087c51019_79.png)

```c
decode<<<1, len>>>(gpu_a, len); // 只启动 len 个线程，不处理终止符
```
**关键点**：在并行处理数组（尤其是字符串）时，必须仔细管理线程索引与数据边界，确保不会意外修改或访问越界。

![](img/79f6aa2eef992bc8530785c087c51019_81.png)

![](img/79f6aa2eef992bc8530785c087c51019_83.png)

---

![](img/79f6aa2eef992bc8530785c087c51019_85.png)

![](img/79f6aa2eef992bc8530785c087c51019_87.png)

## 线程组织与限制 ⚙️
最后，我们简要探讨一下CUDA的线程组织。目前我们只使用了 `<<<1, n>>>` 中的第二个参数，它定义了一个**线程块（Thread Block）**中的线程数量。

*   **线程块大小限制**：每个线程块的最大线程数通常是1024（取决于GPU架构）。这意味着，使用 `<<<1, n>>>` 这种格式，`n` 不能超过1024。
*   **如何启动更多线程**：为了启动超过1024个线程（例如，处理8000个元素的数组），我们需要使用第一个参数来指定**网格（Grid）**中线程块的数量。这是下节课的重点。

例如，以下调用试图启动8000个线程，但可能会失败，因为单个线程块无法容纳8000个线程。
```c
kernel<<<1, 8000>>>(); // 可能失败，超出线程块最大限制
```
我们将学习如何使用网格和线程块来灵活地组织大量线程。

![](img/79f6aa2eef992bc8530785c087c51019_89.png)

![](img/79f6aa2eef992bc8530785c087c51019_91.png)

---

![](img/79f6aa2eef992bc8530785c087c51019_93.png)

![](img/79f6aa2eef992bc8530785c087c51019_95.png)

![](img/79f6aa2eef992bc8530785c087c51019_97.png)

![](img/79f6aa2eef992bc8530785c087c51019_99.png)

## 总结
本节课中我们一起学习了：

![](img/79f6aa2eef992bc8530785c087c51019_101.png)

![](img/79f6aa2eef992bc8530785c087c51019_103.png)

![](img/79f6aa2eef992bc8530785c087c51019_105.png)

1.  **CUDA程序基本流程**：包括主机与设备内存分配、数据传输和内核执行。
2.  **`cudaMemcpy` 函数**：用于在CPU和GPU之间复制数据，方向由 `cudaMemcpyHostToDevice` 和 `cudaMemcpyDeviceToHost` 控制。
3.  **CPU与GPU内存分离**：这是CUDA编程的核心概念，必须为需在GPU上处理的数据在设备端创建副本。
4.  **数据边界管理**：在线程化代码中，必须谨慎处理数组索引，防止越界访问。
5.  **线程组织初步**：了解了线程块的概念及其线程数量限制，为学习网格化线程组织打下基础。

![](img/79f6aa2eef992bc8530785c087c51019_107.png)

![](img/79f6aa2eef992bc8530785c087c51019_109.png)

![](img/79f6aa2eef992bc8530785c087c51019_111.png)

掌握这些基础是编写正确、高效CUDA程序的关键。下一节，我们将深入探讨CUDA的线程层次结构（网格和线程块），以支持大规模并行计算。