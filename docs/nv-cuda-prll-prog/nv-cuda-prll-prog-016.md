# 016：CUDA中的函数 🚀

![](img/0cd3f371fd0cd709168919e0c2635342_1.png)

![](img/0cd3f371fd0cd709168919e0c2635342_3.png)

## 概述

在本节课中，我们将要学习CUDA中不同类型的函数。到目前为止，我们只见过一种使用 `__global__` 关键字定义的函数，我们称之为内核。但CUDA还提供了其他几种定义函数的方式。我们将详细探讨这些不同类型的函数，并通过多个示例来理解它们的设计理念和具体用法。如果时间允许，我们还将简要介绍CUDA的并行算法库——Thrust。

---

![](img/0cd3f371fd0cd709168919e0c2635342_5.png)

![](img/0cd3f371fd0cd709168919e0c2635342_7.png)

## 函数类型介绍

上一节我们介绍了CUDA编程的基本概念，本节中我们来看看CUDA中定义函数的几种不同方式。

在CUDA中，主要有三种方式声明函数，每种方式决定了函数在哪里被调用以及在哪里执行。

![](img/0cd3f371fd0cd709168919e0c2635342_9.png)

![](img/0cd3f371fd0cd709168919e0c2635342_11.png)

1.  **`__global__` 函数（内核）**
    *   使用 `__global__` 关键字声明。
    *   从主机（CPU）调用。
    *   在设备（GPU）上执行。
    *   必须返回 `void` 类型。

![](img/0cd3f371fd0cd709168919e0c2635342_13.png)

![](img/0cd3f371fd0cd709168919e0c2635342_14.png)

![](img/0cd3f371fd0cd709168919e0c2635342_16.png)

![](img/0cd3f371fd0cd709168919e0c2635342_18.png)

2.  **`__device__` 函数（设备函数）**
    *   使用 `__device__` 关键字声明。
    *   从设备（GPU）调用。
    *   在设备（GPU）上执行。
    *   通常由内核函数调用。

![](img/0cd3f371fd0cd709168919e0c2635342_20.png)

![](img/0cd3f371fd0cd709168919e0c2635342_22.png)

3.  **`__host__` 函数（主机函数）**
    *   使用 `__host__` 关键字声明。
    *   从主机（CPU）调用。
    *   在主机（CPU）上执行。
    *   这与普通的C++函数行为一致。`__host__` 关键字的主要用途是与 `__device__` 结合，定义可在主机和设备上执行的函数。

![](img/0cd3f371fd0cd709168919e0c2635342_24.png)

![](img/0cd3f371fd0cd709168919e0c2635342_26.png)

![](img/0cd3f371fd0cd709168919e0c2635342_28.png)

![](img/0cd3f371fd0cd709168919e0c2635342_30.png)

一个程序可以包含多种类型的函数，并且同一种函数可以被多次调用。

---

![](img/0cd3f371fd0cd709168919e0c2635342_32.png)

![](img/0cd3f371fd0cd709168919e0c2635342_34.png)

## 函数调用规则与示例

![](img/0cd3f371fd0cd709168919e0c2635342_36.png)

![](img/0cd3f371fd0cd709168919e0c2635342_38.png)

理解了基本类型后，我们通过一个具体的代码示例来深入理解这些函数之间的调用关系。

![](img/0cd3f371fd0cd709168919e0c2635342_40.png)

![](img/0cd3f371fd0cd709168919e0c2635342_42.png)

以下是示例代码的第一部分，展示了四种不同方式声明的函数：

![](img/0cd3f371fd0cd709168919e0c2635342_44.png)

![](img/0cd3f371fd0cd709168919e0c2635342_46.png)

![](img/0cd3f371fd0cd709168919e0c2635342_48.png)

```cpp
// 一个既可在主机运行也可在设备运行的函数
__host__ __device__ void dh_func() {
    printf("I can run on both CPU and GPU\n");
}

![](img/0cd3f371fd0cd709168919e0c2635342_50.png)

// 一个设备函数
__device__ void d_func() {
    dh_func(); // 设备函数可以调用主机-设备函数
}

![](img/0cd3f371fd0cd709168919e0c2635342_52.png)

![](img/0cd3f371fd0cd709168919e0c2635342_54.png)

![](img/0cd3f371fd0cd709168919e0c2635342_56.png)

![](img/0cd3f371fd0cd709168919e0c2635342_58.png)

// 一个内核函数
__global__ void d_kernel() {
    dh_func(); // 内核可以调用主机-设备函数
    d_func();  // 内核可以调用设备函数
}

![](img/0cd3f371fd0cd709168919e0c2635342_60.png)

![](img/0cd3f371fd0cd709168919e0c2635342_62.png)

![](img/0cd3f371fd0cd709168919e0c2635342_64.png)

![](img/0cd3f371fd0cd709168919e0c2635342_66.png)

// 一个主机函数
__host__ void host_func() {
    dh_func(); // 主机函数可以调用主机-设备函数
}
```

![](img/0cd3f371fd0cd709168919e0c2635342_68.png)

![](img/0cd3f371fd0cd709168919e0c2635342_70.png)

以下是代码的第二部分，即主函数：

![](img/0cd3f371fd0cd709168919e0c2635342_72.png)

![](img/0cd3f371fd0cd709168919e0c2635342_74.png)

```cpp
int main() {
    // 从主机（CPU）调用内核
    d_kernel<<<1, 1>>>();
    cudaDeviceSynchronize();

    // 从主机调用主机函数
    host_func();

    // 从主机调用主机-设备函数
    dh_func();

    // 以下调用会导致编译错误，因为设备函数只能从设备调用
    // d_func();

    return 0;
}
```

![](img/0cd3f371fd0cd709168919e0c2635342_76.png)

![](img/0cd3f371fd0cd709168919e0c2635342_78.png)

基于以上代码，我们可以总结出函数调用的核心规则：

![](img/0cd3f371fd0cd709168919e0c2635342_80.png)

![](img/0cd3f371fd0cd709168919e0c2635342_82.png)

![](img/0cd3f371fd0cd709168919e0c2635342_84.png)

*   **`__device__` 函数**：只能被 `__global__` 内核或其他 `__device__` 函数调用。
*   **`__global__` 内核**：只能从主机（CPU）调用。
*   **`__host__ __device__` 函数**：可以被主机或设备代码调用，但其函数体内的代码必须是主机和设备都能执行的“交集”。

![](img/0cd3f371fd0cd709168919e0c2635342_86.png)

为了更清晰地展示，以下是可能的函数调用关系图：
*   `main` -> `d_kernel` (有效)
*   `main` -> `host_func` (有效)
*   `main` -> `dh_func` (有效)
*   `host_func` -> `dh_func` (有效)
*   `d_kernel` -> `dh_func` (有效)
*   `d_kernel` -> `d_func` (有效)
*   `d_func` -> `dh_func` (有效)

**重要限制**：`__host__ __device__` 函数内部不能调用纯 `__device__` 函数。因为 `__host__ __device__` 函数可能从CPU被调用，而 `__device__` 函数只能在GPU上执行，这会导致运行时错误。同理，其内部也不能使用 `threadIdx.x` 或 `__syncthreads()` 等GPU特有的变量或函数。

---

![](img/0cd3f371fd0cd709168919e0c2635342_88.png)

![](img/0cd3f371fd0cd709168919e0c2635342_90.png)

![](img/0cd3f371fd0cd709168919e0c2635342_92.png)

![](img/0cd3f371fd0cd709168919e0c2635342_94.png)

## 统一内存与函数

![](img/0cd3f371fd0cd709168919e0c2635342_96.png)

上一节我们探讨了函数调用的基本规则，本节中我们来看看函数如何与一种特殊的内存——统一内存进行交互。

![](img/0cd3f371fd0cd709168919e0c2635342_98.png)

![](img/0cd3f371fd0cd709168919e0c2635342_100.png)

![](img/0cd3f371fd0cd709168919e0c2635342_102.png)

统一内存（使用 `cudaMallocManaged` 分配）提供了一种在CPU和GPU之间自动同步的数据视图。这对于 `__host__ __device__` 函数特别有用，因为它们可能从两端访问同一数据。

![](img/0cd3f371fd0cd709168919e0c2635342_104.png)

![](img/0cd3f371fd0cd709168919e0c2635342_106.png)

请看以下示例：

```cpp
__host__ __device__ void func(int *counter) {
    (*counter)++;
    printf("Counter: %d\n", *counter);
}

![](img/0cd3f371fd0cd709168919e0c2635342_108.png)

![](img/0cd3f371fd0cd709168919e0c2635342_110.png)

__global__ void print_k(int *counter) {
    func(counter); // 在GPU上调用func
}

int main() {
    int *counter;
    // 分配统一内存
    cudaMallocManaged(&counter, sizeof(int));
    *counter = 0; // 在CPU上初始化

    printf("Main: %d\n", *counter); // 输出: 0

    print_k<<<1, 1>>>(counter); // 启动内核
    cudaDeviceSynchronize();     // 等待GPU完成

    func(counter); // 在CPU上再次调用func
    printf("Main: %d\n", *counter); // 输出: 2

    cudaFree(counter);
    return 0;
}
```

![](img/0cd3f371fd0cd709168919e0c2635342_112.png)

**程序输出**：
```
Main: 0
Counter: 1
Counter: 2
Main: 2
```

![](img/0cd3f371fd0cd709168919e0c2635342_114.png)

![](img/0cd3f371fd0cd709168919e0c2635342_116.png)

**解释**：
1.  计数器在CPU上初始化为0。
2.  内核 `print_k` 在GPU上执行 `func`，将计数器增加到1并打印。
3.  由于是统一内存，GPU的修改对CPU可见。
4.  CPU再次调用 `func`，此时看到的值是1，将其增加到2并打印。
5.  最终在 `main` 中打印的值为2。

这个例子展示了统一内存如何简化CPU和GPU之间的数据共享。如果使用普通的 `cudaMalloc`（仅在GPU分配）或 `malloc`（仅在CPU分配），并尝试从另一端访问，则会导致段错误或读取垃圾值。

![](img/0cd3f371fd0cd709168919e0c2635342_118.png)

![](img/0cd3f371fd0cd709168919e0c2635342_120.png)

![](img/0cd3f371fd0cd709168919e0c2635342_122.png)

---

![](img/0cd3f371fd0cd709168919e0c2635342_124.png)

![](img/0cd3f371fd0cd709168919e0c2635342_126.png)

![](img/0cd3f371fd0cd709168919e0c2635342_127.png)

![](img/0cd3f371fd0cd709168919e0c2635342_129.png)

## 实践：数组增量示例

![](img/0cd3f371fd0cd709168919e0c2635342_131.png)

![](img/0cd3f371fd0cd709168919e0c2635342_133.png)

理论需要结合实践，本节我们通过一个具体的编程任务来巩固所学知识：编写一个既能被CPU串行执行，也能被GPU并行执行的数组元素自增函数。

![](img/0cd3f371fd0cd709168919e0c2635342_135.png)

**目标**：实现一个函数，对数组的所有元素执行 `arr[i]++` 操作。

**初始方案（CPU串行，GPU单线程串行）**：
```cpp
__host__ __device__ void func(int *arr, int n) {
    for (int i = 0; i < n; ++i) {
        arr[i]++;
    }
}

![](img/0cd3f371fd0cd709168919e0c2635342_137.png)

![](img/0cd3f371fd0cd709168919e0c2635342_139.png)

![](img/0cd3f371fd0cd709168919e0c2635342_141.png)

int main() {
    int n = 1024;
    int *h_arr = new int[n];
    int *d_arr;
    cudaMalloc(&d_arr, n * sizeof(int));

    // 初始化数组...

    // CPU端串行执行
    func(h_arr, n);

    // GPU端串行执行（仅使用1个线程）
    func<<<1, 1>>>(d_arr, n);
    // ...
}
```
此方案在GPU上也是串行的，效率低下。

**优化方案（GPU并行）**：
为了利用GPU的并行能力，我们修改调用方式，让每个GPU线程处理一个数组元素。
```cpp
__host__ __device__ void func(int *elem) {
    (*elem)++; // 只操作单个元素
}

![](img/0cd3f371fd0cd709168919e0c2635342_143.png)

![](img/0cd3f371fd0cd709168919e0c2635342_145.png)

![](img/0cd3f371fd0cd709168919e0c2635342_147.png)

__global__ void d_parallel(int *arr, int n) {
    int idx = threadIdx.x + blockIdx.x * blockDim.x;
    if (idx < n) {
        func(&arr[idx]); // 每个线程调用func处理一个元素
    }
}

![](img/0cd3f371fd0cd709168919e0c2635342_149.png)

![](img/0cd3f371fd0cd709168919e0c2635342_151.png)

![](img/0cd3f371fd0cd709168919e0c2635342_153.png)

int main() {
    // ... 分配和初始化内存
    // CPU端仍串行
    for (int i = 0; i < n; ++i) func(&h_arr[i]);

    // GPU端并行
    int threadsPerBlock = 256;
    int blocks = (n + threadsPerBlock - 1) / threadsPerBlock;
    d_parallel<<<blocks, threadsPerBlock>>>(d_arr, n);
    // ...
}
```
现在GPU执行是并行的，但CPU端仍需显式循环。

![](img/0cd3f371fd0cd709168919e0c2635342_155.png)

![](img/0cd3f371fd0cd709168919e0c2635342_157.png)

![](img/0cd3f371fd0cd709168919e0c2635342_158.png)

![](img/0cd3f371fd0cd709168919e0c2635342_160.png)

![](img/0cd3f371fd0cd709168919e0c2635342_162.png)

**最终方案（统一的函数接口）**：
我们可以让 `func` 函数自身根据调用者决定行为，提供更统一的接口。
```cpp
__host__ __device__ void func(int *arr, int n) {
    // 当从GPU并行调用时，n=1，每个线程增加一个元素
    // 当从CPU串行调用时，n=数组长度，循环增加所有元素
    for (int i = 0; i < n; ++i) {
        arr[i]++;
    }
}

![](img/0cd3f371fd0cd709168919e0c2635342_164.png)

![](img/0cd3f371fd0cd709168919e0c2635342_166.png)

![](img/0cd3f371fd0cd709168919e0c2635342_168.png)

![](img/0cd3f371fd0cd709168919e0c2635342_170.png)

![](img/0cd3f371fd0cd709168919e0c2635342_172.png)

__global__ void d_parallel_unified(int *arr, int n) {
    int idx = threadIdx.x + blockIdx.x * blockDim.x;
    if (idx < n) {
        // 每个线程告诉func只增加它负责的那个元素
        func(&arr[idx], 1);
    }
}

int main() {
    // ... 分配和初始化内存
    // CPU端调用
    func(h_arr, n); // n是数组长度，内部循环

    // GPU端调用
    d_parallel_unified<<<blocks, threadsPerBlock>>>(d_arr, n);
    // ...
}
```
这个设计让 `func` 函数更加灵活，能适应不同的执行环境。

---

## 扩展知识：Thrust库简介 ⚡

在课程的最后，我们简要介绍一下CUDA的Thrust库。Thrust是一个类似于C++标准模板库的并行算法库，它提供了丰富的数据结构（如向量）和算法（如排序、归约、前缀和）。

**Thrust的特点**：
*   **高层抽象**：程序员无需关心内核启动、线程管理等底层细节。
*   **可移植性**：相同的Thrust代码可以在CPU（使用STL后端）或GPU（使用CUDA后端）上运行，编译器根据设备选择实现。
*   **高性能**：库内部经过优化，能有效利用GPU的并行计算能力。

**简单示例（归约求和）**：
使用Thrust进行数组求和比手动编写归约内核简单得多：
```cpp
#include <thrust/reduce.h>
#include <thrust/device_vector.h>
// ...
thrust::device_vector<int> d_vec(n);
// ... 填充数据
int sum = thrust::reduce(d_vec.begin(), d_vec.end(), 0, thrust::plus<int>());
```
Thrust库包含大量此类实用算法，能极大提升CUDA程序的开发效率。

![](img/0cd3f371fd0cd709168919e0c2635342_174.png)

![](img/0cd3f371fd0cd709168919e0c2635342_176.png)

---

## 总结

![](img/0cd3f371fd0cd709168919e0c2635342_178.png)

![](img/0cd3f371fd0cd709168919e0c2635342_180.png)

本节课中我们一起学习了CUDA中不同类型的函数及其调用规则。我们深入探讨了 `__global__`、`__device__` 和 `__host__` 函数的定义、调用限制以及它们与统一内存的协作。通过数组增量等示例，我们实践了如何编写既能用于CPU串行执行也能用于GPU并行执行的灵活函数。最后，我们简要了解了能简化并行编程的Thrust库。掌握这些函数类型是构建高效、复杂CUDA应用程序的基础。