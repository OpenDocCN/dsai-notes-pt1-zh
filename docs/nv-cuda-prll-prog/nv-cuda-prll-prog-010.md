# 010：存储体冲突、纹理内存、常量内存及计算能力 🚀

![](img/fe6f14585acd6e9a70d02305c3769a23_1.png)

## 概述

在本节课中，我们将继续深入学习GPU内存体系结构。我们将详细探讨共享内存中的存储体冲突问题，并通过示例理解其成因和影响。接着，我们将介绍两种特殊的只读内存：纹理内存和常量内存，了解它们的特性、声明方式和使用场景。最后，我们将简要介绍CUDA计算能力的概念及其与GPU硬件家族的关系。

![](img/fe6f14585acd6e9a70d02305c3769a23_3.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_5.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_7.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_9.png)

---

## 存储体冲突详解

![](img/fe6f14585acd6e9a70d02305c3769a23_11.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_13.png)

上一节我们介绍了共享内存的基本概念和`__syncthreads()`同步原语。本节中，我们来看看共享内存内部的组织结构——存储体，以及由此引发的“存储体冲突”问题。

![](img/fe6f14585acd6e9a70d02305c3769a23_15.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_17.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_18.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_20.png)

共享内存被组织成32个存储体。对共享内存的访问由线程束（32个线程）执行。理解以下关键点至关重要：

![](img/fe6f14585acd6e9a70d02305c3769a23_22.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_24.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_26.png)

1.  对同一存储体的访问是顺序执行的。
2.  对同一存储体中**不同字**的访问会导致顺序化，从而引发存储体冲突。
3.  对同一存储体中**同一字**的访问不会导致顺序化，因此不会引发存储体冲突。
4.  连续的字存储在相邻的存储体中。

![](img/fe6f14585acd6e9a70d02305c3769a23_28.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_30.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_32.png)

以下是理解存储体访问模式的几个核心示例：

![](img/fe6f14585acd6e9a70d02305c3769a23_34.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_36.png)

### 无存储体冲突的访问模式

![](img/fe6f14585acd6e9a70d02305c3769a23_38.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_40.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_42.png)

当线程束中的每个线程访问连续且位于不同存储体中的字时，所有访问可以并行完成，没有冲突。

```cuda
__global__ void noBankConflict(float* output) {
    __shared__ float s_data[1024];
    int tid = threadIdx.x;
    // 每个线程访问不同存储体中的元素
    s_data[tid] = tid * 1.0f; // 乘以1仅为示意，无实际影响
    // ... 其他操作
}
```
在这个例子中，`thread0`访问`s_data[0]`（存储体0），`thread1`访问`s_data[1]`（存储体1），以此类推。所有访问并行发生。

![](img/fe6f14585acd6e9a70d02305c3769a23_44.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_46.png)

### 导致存储体冲突的访问模式

![](img/fe6f14585acd6e9a70d02305c3769a23_48.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_50.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_52.png)

当线程束中的多个线程访问同一存储体中的不同字时，这些访问会被顺序化，导致性能下降。

![](img/fe6f14585acd6e9a70d02305c3769a23_54.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_56.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_58.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_60.png)

```cuda
__global__ void bankConflict(float* output) {
    __shared__ float s_data[1024];
    int tid = threadIdx.x;
    // 导致冲突的访问模式：多个线程访问同一存储体
    s_data[tid * 32] = tid * 1.0f;
    // ... 其他操作
}
```
在这个例子中，`thread0`访问`s_data[0]`（存储体0），`thread1`访问`s_data[32]`（存储体0），`thread2`访问`s_data[64]`（存储体0）。由于它们访问的是存储体0中的不同字，这些请求会被顺序处理，形成存储体冲突。这被称为“多路存储体冲突”（例如，本例是32路冲突）。

![](img/fe6f14585acd6e9a70d02305c3769a23_62.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_64.png)

### 存储体冲突可视化

![](img/fe6f14585acd6e9a70d02305c3769a23_66.png)

为了更直观地理解，我们可以通过以下图示来分析几种访问模式：

![](img/fe6f14585acd6e9a70d02305c3769a23_68.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_70.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_72.png)

*   **场景A**：每个线程访问不同存储体中的唯一字。这是**无冲突**的理想情况。
*   **场景B**：两个线程（如thread0和thread16）访问同一存储体中的不同字。这会导致**2路存储体冲突**。
*   **场景C**：访问模式看似杂乱，但每个线程访问的存储体仍然是唯一的。这同样是**无冲突**的。
*   **场景D & E**：多个线程访问同一存储体中的**同一个字**。根据规则3，这属于特殊情况，访问可以广播给所有请求线程，因此**不会引发存储体冲突**。

![](img/fe6f14585acd6e9a70d02305c3769a23_74.png)

**优化优先级**：如果一个程序同时存在未合并的全局内存访问和存储体冲突，应优先优化全局内存访问，使其合并。因为全局内存访问延迟（400-800周期）远高于共享内存。在全局内存访问优化好后，再着手解决存储体冲突问题。

![](img/fe6f14585acd6e9a70d02305c3769a23_76.png)

---

![](img/fe6f14585acd6e9a70d02305c3769a23_78.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_80.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_82.png)

## 纹理内存

![](img/fe6f14585acd6e9a70d02305c3769a23_84.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_86.png)

纹理内存是一种全局的、只读的内存空间，针对具有空间局部性的访问模式（如图像处理中的2D邻域访问）进行了优化。

![](img/fe6f14585acd6e9a70d02305c3769a23_88.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_90.png)

### 声明与绑定

![](img/fe6f14585acd6e9a70d02305c3769a23_92.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_94.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_96.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_98.png)

使用纹理内存需要以下步骤：

![](img/fe6f14585acd6e9a70d02305c3769a23_100.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_102.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_104.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_106.png)

1.  **声明纹理引用**：使用`texture`类型声明一个变量。
    ```cuda
    texture<float, 2, cudaReadModeElementType> texRef;
    ```
    其中，`float`是数据类型，`2`表示是2D纹理，`cudaReadModeElementType`表示以元素形式读取。

![](img/fe6f14585acd6e9a70d02305c3769a23_108.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_110.png)

2.  **绑定到设备内存**：将纹理引用绑定到已分配的CUDA数组。
    ```cuda
    cudaArray* cuArray;
    // ... 分配并初始化cuArray (例如使用cudaMallocArray, cudaMemcpyToArray)
    cudaBindTextureToArray(texRef, cuArray);
    ```

![](img/fe6f14585acd6e9a70d02305c3769a23_112.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_113.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_115.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_117.png)

### 在核函数中访问

![](img/fe6f14585acd6e9a70d02305c3769a23_119.png)

在核函数内部，使用`tex2D()`函数来读取纹理内存。
```cuda
float value = tex2D(texRef, x, y);
```

![](img/fe6f14585acd6e9a70d02305c3769a23_121.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_123.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_125.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_126.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_128.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_130.png)

### 应用示例：图像旋转

![](img/fe6f14585acd6e9a70d02305c3769a23_132.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_134.png)

以下是一个简化的核函数，演示如何使用纹理内存实现图像旋转（或仿射变换）：
```cuda
__global__ void transformKernel(float* output, int width, int height, float theta) {
    unsigned int x = blockIdx.x * blockDim.x + threadIdx.x;
    unsigned int y = blockIdx.y * blockDim.y + threadIdx.y;

    if (x < width && y < height) {
        // 计算旋转后的坐标 (u, v)
        float u = x * cosf(theta) - y * sinf(theta);
        float v = x * sinf(theta) + y * cosf(theta);

        // 从纹理内存中读取旋转后坐标处的像素值
        // 注意：tex2D会自动处理边界和插值（如果设置）
        float pixel = tex2D(texRef, u + 0.5f, v + 0.5f);

        // 写入输出数组（全局内存）
        output[y * width + x] = pixel;
    }
}
```
在这个例子中，输入图像被绑定到纹理内存`texRef`。每个线程计算其对应输出像素`(x, y)`在输入图像中对应的源位置`(u, v)`，然后通过`tex2D`高效地读取该位置的值。纹理内存硬件会对`(u, v)`附近的像素进行缓存，非常适合这种访问模式。

![](img/fe6f14585acd6e9a70d02305c3769a23_136.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_138.png)

**重要提示**：纹理内存是只读的。计算结果需要写入到全局内存（如`output`数组）中。

![](img/fe6f14585acd6e9a70d02305c3769a23_140.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_142.png)

---

![](img/fe6f14585acd6e9a70d02305c3769a23_144.png)

## 常量内存

常量内存是另一种全局只读内存，大小固定（通常为64KB）。它用于存储需要被所有线程频繁访问的常量数据。常量内存通过缓存实现高速访问。

### 声明与初始化

![](img/fe6f14585acd6e9a70d02305c3769a23_146.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_148.png)

1.  **声明常量变量**：使用`__constant__`限定符在全局作用域声明。
    ```cuda
    __constant__ int constData[256];
    ```

![](img/fe6f14585acd6e9a70d02305c3769a23_150.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_152.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_154.png)

2.  **从主机初始化**：常量内存只能从主机（CPU）代码进行初始化，使用`cudaMemcpyToSymbol`函数。
    ```cuda
    int h_data = 10;
    cudaMemcpyToSymbol(constData, &h_data, sizeof(int));
    ```

![](img/fe6f14585acd6e9a70d02305c3769a23_156.png)

### 在核函数中访问

在核函数中，可以像读取普通全局变量一样读取常量内存。
```cuda
__global__ void myKernel(int* result) {
    int tid = threadIdx.x + blockIdx.x * blockDim.x;
    result[tid] = constData[0] * tid; // 所有线程读取同一个常量值
}
```

![](img/fe6f14585acd6e9a70d02305c3769a23_158.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_159.png)

### 简单示例

以下代码展示了常量内存的完整使用流程：
```cuda
// 1. 声明常量内存变量
__constant__ unsigned int myConst;

__global__ void constantKernel(unsigned int* data) {
    // 3. 在核函数中访问常量内存
    int tid = threadIdx.x;
    data[tid] = myConst; // 所有线程都将自己的输出设置为常量值
}

![](img/fe6f14585acd6e9a70d02305c3769a23_161.png)

int main() {
    unsigned int h_val = 10;
    unsigned int* d_data;
    cudaMalloc(&d_data, 32 * sizeof(unsigned int));

    // 2. 从主机复制数据到常量内存
    cudaMemcpyToSymbol(myConst, &h_val, sizeof(unsigned int));

    constantKernel<<<1, 32>>>(d_data);
    // ... 同步并打印d_data，所有32个元素值都为10
    cudaFree(d_data);
    return 0;
}
```

![](img/fe6f14585acd6e9a70d02305c3769a23_163.png)

---

![](img/fe6f14585acd6e9a70d02305c3769a23_165.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_167.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_169.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_171.png)

## CUDA计算能力

计算能力（Compute Capability）是一个由主版本号和次版本号组成的版本号（例如`8.6`），它代表了GPU硬件的架构和功能集。

![](img/fe6f14585acd6e9a70d02305c3769a23_173.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_175.png)

*   **硬件特性标识**：不同的计算能力版本支持不同的硬件特性（如双精度原子操作、动态并行、张量核心等）。
*   **编译指定**：在使用`nvcc`编译时，可以通过`-arch=sm_xx`标志指定目标计算能力（例如`-arch=sm_86`），以确保编译器使用该版本支持的指令和优化。
*   **与CUDA版本区别**：CUDA版本（如CUDA 11.0）是软件工具包的版本。计算能力是GPU硬件的属性。高版本的CUDA工具包支持更多计算能力的设备。

### 主要计算能力版本与特性

| 计算能力 | 架构家族 | 新增关键特性示例 |
| :--- | :--- | :--- |
| 1.x | Tesla | 基础CUDA支持 |
| 2.x | Fermi | 线程束内投票、共享内存原子操作 |
| 3.x | Kepler | 统一内存、动态并行性（从3.5开始） |
| 5.x, 6.x | Maxwell, Pascal | 改进的功耗效率、统一内存增强 |
| 7.x | Volta, Turing | 张量核心（AI加速）、独立的线程调度 |
| 8.x | Ampere | 硬件支持的异步拷贝、第三代张量核心 |
| 9.x | Hopper (最新) | 第四代张量核心、Transformer引擎 |

![](img/fe6f14585acd6e9a70d02305c3769a23_177.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_179.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_181.png)

**注意**：计算能力4.x被跳过。计算能力10.x（Lovelace）和11.x（Blackwell）对应更新的架构，部分仍在开发或发布初期。

![](img/fe6f14585acd6e9a70d02305c3769a23_183.png)

选择编译目标时，需要根据程序使用的特性和目标部署平台的GPU来确定合适的计算能力版本。

![](img/fe6f14585acd6e9a70d02305c3769a23_185.png)

![](img/fe6f14585acd6e9a70d02305c3769a23_187.png)

---

## 总结

![](img/fe6f14585acd6e9a70d02305c3769a23_189.png)

本节课中我们一起学习了CUDA内存体系的几个高级主题。

![](img/fe6f14585acd6e9a70d02305c3769a23_191.png)

我们深入分析了**共享内存的存储体冲突**，明白了其产生条件是线程束内多个线程访问同一存储体中的不同字，并通过代码示例学会了如何识别和避免这类冲突。

接着，我们探讨了两种特殊的只读内存：**纹理内存**和**常量内存**。纹理内存针对空间局部性访问优化，非常适合图像处理等算法；常量内存则通过缓存为所有线程提供对常量数据的高速访问。我们掌握了它们的声明、初始化和在核函数中访问的方法。

最后，我们介绍了**CUDA计算能力**的概念，理解了它是标识GPU硬件功能和架构的版本号，并知道了如何在编译时指定目标计算能力。

![](img/fe6f14585acd6e9a70d02305c3769a23_193.png)

至此，我们已经完成了对CUDA内存子系统主要组成部分的学习。从全局内存、共享内存、寄存器到纹理和常量内存，这些知识将帮助我们更好地优化CUDA程序，充分发挥GPU的并行计算潜力。