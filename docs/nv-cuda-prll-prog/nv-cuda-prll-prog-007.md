# 007：内存合并与AoS对SoA 🚀

![](img/df312d42a097bbad3d97c841f3ecfc46_0.png)

## 概述
在本节课中，我们将要学习GPU内存访问中一个至关重要的性能优化概念——**内存合并**。我们将探讨什么是内存合并，以及如何通过选择合适的数据结构布局（**数组结构体** 与 **结构体数组**）来最大化内存合并的效率，从而显著提升GPU程序的性能。

---

## 上一节回顾
在上一节中，我们介绍了内存层次结构、带宽以及**空间局部性**和**时间局部性**的概念。我们通过CPU上的例子看到，遵循内存的存储顺序（如行主序）进行访问可以带来显著的性能提升。

![](img/df312d42a097bbad3d97c841f3ecfc46_2.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_4.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_6.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_7.png)

本节中，我们将把焦点转移到GPU上，看看除了局部性之外，另一个对GPU内存性能影响巨大的因素——**内存合并**。

![](img/df312d42a097bbad3d97c841f3ecfc46_9.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_10.png)

---

![](img/df312d42a097bbad3d97c841f3ecfc46_12.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_14.png)

## 什么是内存合并？ 🤔

![](img/df312d42a097bbad3d97c841f3ecfc46_16.png)

内存合并是GPU硬件层面的一项优化技术。其核心定义如下：

**当一个线程束（32个线程）访问一个连续的内存块（例如，一个32字大小的块）时，这些零散的内存访问请求会被硬件“合并”成一个单一的、更宽的内存事务。**

![](img/df312d42a097bbad3d97c841f3ecfc46_18.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_20.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_22.png)

这意味着，原本可能需要32次独立内存访问的操作，现在可能只需要1次或几次合并后的访问即可完成。这极大地减少了内存延迟，提升了内存带宽的利用率。

![](img/df312d42a097bbad3d97c841f3ecfc46_24.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_26.png)

### 合并访问示例
考虑一个经典的初始化数组的内核：
```c
__global__ void initArray(int *A) {
    int tid = blockIdx.x * blockDim.x + threadIdx.x;
    A[tid] = 0; // 合并的内存访问
}
```
在这个例子中，线程0访问`A[0]`，线程1访问`A[1]`，...，线程31访问`A[31]`。由于这些元素在内存中是连续存储的，并且位于同一个内存块内，因此这32次访问会被硬件合并成一次高效的内存事务。

![](img/df312d42a097bbad3d97c841f3ecfc46_28.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_30.png)

### 非合并访问的代价
如果没有内存合并，每个线程的加载/存储指令都需要独立的内存周期。在最坏情况下，一个线程束可能需要32个内存周期，这将导致严重的性能瓶颈，使得GPU的计算能力无法充分发挥。

![](img/df312d42a097bbad3d97c841f3ecfc46_32.png)

---

![](img/df312d42a097bbad3d97c841f3ecfc46_34.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_36.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_38.png)

## 内存合并的内部实现 🛠️

![](img/df312d42a097bbad3d97c841f3ecfc46_40.png)

内存合并是由GPU硬件自动完成的，程序员无法直接控制其开关。其工作方式与线程束的执行模型紧密相关：
*   线程束以**锁步**方式执行指令。
*   当线程束执行一条内存指令（加载或存储）时，内存控制器会检查所有线程请求的内存地址。
*   如果这些地址落在一个对齐的、连续的内存块（例如32字节、64字节或128字节，具体取决于GPU架构和访问类型）内，硬件就会将这些请求合并为一个内存事务。

**关键要点**：内存合并是硬件的固有特性。程序员的责任是通过组织数据和访问模式来**促成**合并访问的发生。

![](img/df312d42a097bbad3d97c841f3ecfc46_42.png)

---

## 内存访问模式分析 📊

![](img/df312d42a097bbad3d97c841f3ecfc46_44.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_46.png)

根据线程束内线程访问内存的方式，我们可以将访问模式分为三类：

![](img/df312d42a097bbad3d97c841f3ecfc46_48.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_50.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_52.png)

以下是三种典型的内存访问模式图示：

![](img/df312d42a097bbad3d97c841f3ecfc46_54.png)

1.  **完全合并访问**：线程束中所有线程访问连续且位于同一内存块内的地址。这是最理想的情况。
    ![](img/df312d42a097bbad3d97c841f3ecfc46_2.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_56.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_58.png)

2.  **部分合并访问**：线程束中只有部分线程的访问地址是连续的，其他线程的访问地址分散在不同的内存块中。这会导致多个内存事务。
    ![](img/df312d42a097bbad3d97c841f3ecfc46_4.png)

3.  **完全非合并访问**：线程束中每个线程访问的地址都位于完全不同的、不连续的内存块中。这会导致最差的性能，需要最多（32次）的内存事务。
    ![](img/df312d42a097bbad3d97c841f3ecfc46_6.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_60.png)

---

![](img/df312d42a097bbad3d97c841f3ecfc46_62.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_64.png)

## 代码示例：步长访问与随机访问 💻

![](img/df312d42a097bbad3d97c841f3ecfc46_66.png)

让我们通过具体代码来理解不同的访问模式。

### 1. 完全合并访问
```c
// 经典示例：连续访问
A[tid] = value;
```

### 2. 完全非合并访问（通过大跨度步长实现）
```c
// 假设 chunk_size = 32 (或更大)
int start = tid * chunk_size;
int end = start + chunk_size;
for (int i = start; i < end; i++) {
    A[i] = value; // 每个线程访问完全独立的内存块
}
```
当`chunk_size`为32时，线程0访问元素0-31，线程1访问元素32-63...，彼此毫无重叠，导致完全非合并。

![](img/df312d42a097bbad3d97c841f3ecfc46_68.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_70.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_72.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_74.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_76.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_78.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_79.png)

### 3. 部分合并访问
```c
// 假设 chunk_size = 2
int start = tid * chunk_size;
int end = start + chunk_size;
for (int i = start; i < end; i++) {
    A[i] = value;
}
```
当`chunk_size`为2时，前16个线程（tid 0-15）访问的元素（如0-1, 2-3, ..., 30-31）可能位于同一个或两个内存块内，从而产生部分合并。这通常需要2次内存事务。

### 4. 随机访问
```c
// 索引来自另一个输入数组，访问模式不可预测
int index = input[tid];
A[index] = value;
```
这种模式在图形算法（如BFS、SSSP）中很常见，其合并效率完全取决于`input`数组中的数据分布，通常是低效的。

---

## 数据结构布局：AoS vs SoA 🏗️

数据在内存中的组织方式（数据结构布局）对内存合并有决定性影响。主要分为两种：

![](img/df312d42a097bbad3d97c841f3ecfc46_81.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_83.png)

### 数组结构体
**数组结构体** 是CPU编程中常见的形式。它将一个对象的所有属性打包在一起，然后创建该对象的数组。
```c
// AoS (Array of Structures)
struct Student {
    int roll_no;
    double marks;
    char name[50];
};
Student students[1000]; // 数组的每个元素都是一个完整的Student结构体
```
*   **CPU优势**：利于**空间局部性**。当CPU线程访问`students[i].roll_no`时，很可能紧接着访问`students[i].marks`，后者可能已被缓存。
*   **GPU劣势**：当GPU线程束访问不同学生的`roll_no`时，访问地址的跨度是`sizeof(Student)`，这会导致**跨步访问**，严重阻碍内存合并。

![](img/df312d42a097bbad3d97c841f3ecfc46_85.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_87.png)

### 结构体数组
**结构体数组** 是GPU优化中推荐的形式。它将所有对象的同一属性分别存储在独立的数组中。
```c
// SoA (Structure of Arrays)
struct StudentData {
    int roll_no[1000];
    double marks[1000];
    char name[1000][50];
};
StudentData students; // 一个结构体，其成员是多个数组
```
*   **GPU优势**：利于**内存合并**。当GPU线程束访问不同学生的`roll_no`时（即访问`roll_no[0]`, `roll_no[1]`, ...），这些地址是连续的，可以实现完美的合并访问。
*   **CPU劣势**：可能破坏空间局部性，因为访问一个学生的所有属性需要从三个不同的数组位置读取。

![](img/df312d42a097bbad3d97c841f3ecfc46_89.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_91.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_93.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_95.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_97.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_99.png)

---

![](img/df312d42a097bbad3d97c841f3ecfc46_101.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_103.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_105.png)

## GPU上的AoS与SoA实现对比 ⚖️

![](img/df312d42a097bbad3d97c841f3ecfc46_107.png)

以下是两种布局在GPU内核中访问方式的对比：

### AoS 访问方式（性能较差）
```c
// 内核函数
__global__ void processAoS(Student *d_students) {
    int tid = blockIdx.x * blockDim.x + threadIdx.x;
    // 跨步访问，不利于合并
    d_students[tid].roll_no = tid;
    d_students[tid].marks = tid * 10.0;
}
// 内存分配与拷贝
Student *d_students;
cudaMalloc(&d_students, N * sizeof(Student));
cudaMemcpy(d_students, h_students, N * sizeof(Student), cudaMemcpyHostToDevice);
```

![](img/df312d42a097bbad3d97c841f3ecfc46_109.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_111.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_113.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_115.png)

### SoA 访问方式（性能更优）
```c
// 内核函数
__global__ void processSoA(int *roll_no, double *marks) {
    int tid = blockIdx.x * blockDim.x + threadIdx.x;
    // 连续访问，利于合并
    roll_no[tid] = tid;
    marks[tid] = tid * 10.0;
}
// 内存分配与拷贝（需要为每个数组成员单独分配）
int *d_roll_no;
double *d_marks;
cudaMalloc(&d_roll_no, N * sizeof(int));
cudaMalloc(&d_marks, N * sizeof(double));
cudaMemcpy(d_roll_no, h_roll_no, N * sizeof(int), cudaMemcpyHostToDevice);
cudaMemcpy(d_marks, h_marks, N * sizeof(double), cudaMemcpyHostToDevice);
```
**性能对比**：在实际测试中，使用SoA布局的内核执行时间可能仅为使用AoS布局内核的1/3，这巨大的性能提升主要归功于高效的内存合并。

![](img/df312d42a097bbad3d97c841f3ecfc46_117.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_119.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_121.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_122.png)

---

![](img/df312d42a097bbad3d97c841f3ecfc46_124.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_126.png)

## 总结
本节课中我们一起学习了GPU并行编程中两个核心的优化概念：
1.  **内存合并**：GPU硬件将线程束内连续的、对齐的内存访问合并为单个宽内存事务的机制，是获得高内存带宽的关键。
2.  **AoS与SoA**：数据结构布局的选择直接影响内存合并的效率。
    *   **AoS** 在CPU上利用局部性有优势，但在GPU上会导致跨步访问，破坏合并。
    *   **SoA** 是GPU编程的推荐模式，它能确保对同一属性的访问是连续的，从而实现完美的内存合并，带来显著的性能提升。

![](img/df312d42a097bbad3d97c841f3ecfc46_128.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_130.png)

![](img/df312d42a097bbad3d97c841f3ecfc46_132.png)

作为程序员，在设计GPU程序时，应当时刻考虑数据的访问模式，并优先采用SoA布局来最大化内存合并的效益。