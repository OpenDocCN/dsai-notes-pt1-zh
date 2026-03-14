# 014：CUDA中的多种原子函数与屏障 🚀

![](img/b570460d95f5d51d21e4c35ea3a489a9_1.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_3.png)

在本节课中，我们将学习如何使用CUDA中的原子函数来解决数据竞争问题，并深入探讨屏障（Barrier）这一重要的同步机制。我们将从回顾单源最短路径算法中的数据竞争问题开始，然后学习如何使用原子操作（如 `atomicMin` 和 `atomicCAS`）来确保代码的正确性。接着，我们将了解如何利用 `atomicCAS` 实现锁（Lock）和单线程执行区（Single Section）。最后，我们将正式定义屏障，并介绍CUDA中不同级别的屏障同步。

---

![](img/b570460d95f5d51d21e4c35ea3a489a9_5.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_7.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_9.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_10.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_12.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_14.png)

## 回顾：单源最短路径算法中的数据竞争 🔄

![](img/b570460d95f5d51d21e4c35ea3a489a9_16.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_17.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_19.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_21.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_23.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_25.png)

在之前的课程中，我们学习了单源最短路径算法。我们注意到，在并行执行时，代码中存在数据竞争问题，这可能导致计算出错误的距离值。

![](img/b570460d95f5d51d21e4c35ea3a489a9_27.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_29.png)

问题出现在以下代码段中：
```c
if (alt_dist < dist[neighbor]) {
    dist[neighbor] = alt_dist;
}
```
当多个线程同时读取和更新同一个邻居节点（例如节点C）的距离时，可能会发生数据竞争。例如，线程A和线程B可能都读取到节点C的初始距离为无穷大，然后分别尝试将其更新为3和6。最后写入的值（可能是6）将覆盖掉更短的正确距离（3），从而导致错误。

![](img/b570460d95f5d51d21e4c35ea3a489a9_31.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_33.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_35.png)

上一节我们介绍了数据竞争的问题，本节中我们来看看如何使用原子操作来解决它。

![](img/b570460d95f5d51d21e4c35ea3a489a9_37.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_39.png)

---

![](img/b570460d95f5d51d21e4c35ea3a489a9_41.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_43.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_45.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_47.png)

## 使用原子操作解决数据竞争 ⚛️

![](img/b570460d95f5d51d21e4c35ea3a489a9_49.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_51.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_53.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_55.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_56.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_58.png)

为了消除数据竞争，我们需要确保“读取-比较-写入”这一系列操作是原子性的，即不可分割的。CUDA提供了 `atomicMin` 函数，它正是用于此目的。

我们可以将上述易出错的代码替换为：
```c
atomicMin(&dist[neighbor], alt_dist);
```
`atomicMin` 函数会原子性地比较 `dist[neighbor]` 的当前值与 `alt_dist` 的值，并将 `dist[neighbor]` 更新为两者中的较小值。这样，无论线程执行顺序如何，每个节点的距离都会被正确地更新为所有可能路径中的最小值。

![](img/b570460d95f5d51d21e4c35ea3a489a9_60.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_61.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_63.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_65.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_67.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_69.png)

---

## 深入原子比较与交换（atomicCAS） 🔧

![](img/b570460d95f5d51d21e4c35ea3a489a9_71.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_72.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_74.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_76.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_78.png)

`atomicCAS`（Compare And Swap）是一个功能强大的原子操作，其语法如下：
```c
int atomicCAS(int* address, int compare, int val);
```
它的工作流程是：
1.  原子性地读取 `address` 指针处的值。
2.  将该值与 `compare` 参数进行比较。
3.  如果相等，则将 `address` 处的值设置为 `val`。
4.  无论是否交换，函数都返回 `address` 处原来的旧值。

![](img/b570460d95f5d51d21e4c35ea3a489a9_80.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_82.png)

`atomicCAS` 有两个典型应用：
1.  **实现锁（Lock）**，用于保护临界区。
2.  **实现单线程执行区（Single Section）**，确保只有任意一个线程执行特定代码块。

![](img/b570460d95f5d51d21e4c35ea3a489a9_84.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_86.png)

---

![](img/b570460d95f5d51d21e4c35ea3a489a9_88.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_90.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_92.png)

### 使用 atomicCAS 实现锁 🔒

![](img/b570460d95f5d51d21e4c35ea3a489a9_94.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_96.png)

简单的 `atomicCAS` 调用本身并不能实现锁。我们需要将其与循环结合，构建一个自旋锁。

![](img/b570460d95f5d51d21e4c35ea3a489a9_98.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_100.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_102.png)

以下是使用 `atomicCAS` 在GPU上实现一个能正确处理线程束（Warp）内同步的锁的示例：
```c
// 假设 lock_var 初始化为 0
__shared__ int lock_var;

![](img/b570460d95f5d51d21e4c35ea3a489a9_104.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_106.png)

// 尝试获取锁
int old;
do {
    old = atomicCAS(&lock_var, 0, 1); // 尝试将锁从0设为1
} while (old != 0); // 如果old不是0，说明锁已被占用，继续循环

// 临界区代码
// ...

![](img/b570460d95f5d51d21e4c35ea3a489a9_108.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_110.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_112.png)

// 释放锁
lock_var = 0;
```
**重要说明**：这个基础实现在CPU或不同Warp的线程间可以工作，但在同一个Warp的线程间会导致死锁。因为Warp内的线程是同步执行的，一个线程获得锁后，其他线程会在循环中等待，而获得锁的线程也在等待循环中的其他线程，形成死锁。

![](img/b570460d95f5d51d21e4c35ea3a489a9_114.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_116.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_118.png)

为了解决Warp内死锁问题，需要结合条件判断和线程掩码，确保同一时刻只有一个Warp线程进入临界区，而其他线程被正确屏蔽。这通常涉及更复杂的逻辑。

![](img/b570460d95f5d51d21e4c35ea3a489a9_120.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_122.png)

---

### 使用 atomicCAS 实现单线程执行区 🎯

![](img/b570460d95f5d51d21e4c35ea3a489a9_124.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_126.png)

单线程执行区要求只有任意一个线程执行某段代码，其他线程无需等待，直接跳过。

![](img/b570460d95f5d51d21e4c35ea3a489a9_128.png)

使用 `atomicCAS` 可以轻松实现：
```c
// 假设 single_flag 初始化为 0
__shared__ int single_flag;

![](img/b570460d95f5d51d21e4c35ea3a489a9_130.png)

int old = atomicCAS(&single_flag, 0, 1); // 尝试将标志从0设为1
if (old == 0) {
    // 只有一个线程会成功进入此区域
    // 单线程执行区代码
    // ...
}
// 其他线程直接跳过，继续执行后续代码
```
在这个实现中，`single_flag` 在首次被成功后就不再是0，因此后续线程的 `atomicCAS` 操作都会失败，从而不会进入if块。线程执行完单线程区后，也无需重置 `single_flag`。

![](img/b570460d95f5d51d21e4c35ea3a489a9_132.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_134.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_136.png)

---

![](img/b570460d95f5d51d21e4c35ea3a489a9_138.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_140.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_142.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_144.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_146.png)

## 理解屏障（Barrier） 🚧

屏障是一种同步点，要求所有参与线程都必须到达该点后，才能有任何线程继续执行后续代码。

**类比**：想象一群朋友约定去踢足球。大家从各自家中（异步）出发，到一个集合点汇合（屏障）。只有所有人都到达集合点后，大家才一起前往足球场。

![](img/b570460d95f5d51d21e4c35ea3a489a9_148.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_150.png)

在CUDA中，存在不同级别的屏障：

![](img/b570460d95f5d51d21e4c35ea3a489a9_152.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_154.png)

1.  **隐式全局屏障**：内核（Kernel）的结束点本身就是一个隐式屏障，主机代码会等待所有GPU线程完成。
2.  **线程块内屏障**：使用 `__syncthreads()` 函数。它同步同一个线程块内的所有线程。
3.  **全局屏障**：从CUDA 9开始，支持 `__syncgrid()`（或类似机制，具体API需查阅文档），可以同步网格（Grid）中所有线程块内的线程。
4.  **线程束内屏障**：对于Warp内的线程，由于它们以锁步方式执行指令，因此本身就存在隐式的执行屏障，通常不需要显式同步。

![](img/b570460d95f5d51d21e4c35ea3a489a9_156.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_158.png)

---

![](img/b570460d95f5d51d21e4c35ea3a489a9_160.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_162.png)

## 总结 📚

本节课我们一起学习了以下核心内容：
*   **原子函数**：我们回顾了使用 `atomicMin` 解决单源最短路径算法中的数据竞争问题。
*   **`atomicCAS`**：深入学习了其原理，并探讨了如何用它来实现**锁**（解决临界区互斥访问）和**单线程执行区**。特别要注意在GPU上实现锁时需考虑Warp内线程的死锁问题。
*   **屏障**：我们正式定义了屏障作为同步点的概念，并介绍了CUDA中不同层次的同步机制，包括隐式内核屏障、`__syncthreads()` 线程块内屏障以及更新的全局屏障支持。

![](img/b570460d95f5d51d21e4c35ea3a489a9_164.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_166.png)

![](img/b570460d95f5d51d21e4c35ea3a489a9_168.png)

通过掌握原子操作和屏障，你可以编写出正确、高效且同步良好的CUDA并行程序，避免数据竞争和协调线程间的执行顺序。