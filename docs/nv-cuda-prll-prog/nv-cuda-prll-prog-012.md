# 012：互斥锁 🔒

![](img/849f9634ffa9fef9cf255d58f370c34c_0.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_2.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_4.png)

## 概述
在本节课中，我们将学习如何使用控制流和数据流（即不使用原子操作或硬件屏障等特殊指令）来实现两个线程间的互斥锁。我们将分析几种不同的实现方法，探讨它们各自在互斥性、进展性和死锁方面的优缺点，并最终介绍经典的Peterson算法。

![](img/849f9634ffa9fef9cf255d58f370c34c_6.png)

---

![](img/849f9634ffa9fef9cf255d58f370c34c_8.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_10.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_12.png)

## 上节回顾
在上一节中，我们介绍了同步、互斥锁和临界区的概念。我们通过数据库的例子了解了数据竞争，分析了导致数据竞争的四个必要条件，并探讨了如何通过消除这些条件来避免数据竞争。课程最后，我们分析了一个用于计算城市间最短距离的GPU内核代码，并指出了其中因多个线程同时读写共享数组而可能引发的数据竞争问题。

![](img/849f9634ffa9fef9cf255d58f370c34c_14.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_16.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_17.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_19.png)

---

![](img/849f9634ffa9fef9cf255d58f370c34c_21.png)

## 同步的三种方式
实现同步通常有三种主要方式：
1.  **控制流 + 数据流**：仅使用程序逻辑（如循环、条件判断）和普通变量进行同步。
2.  **原子操作**：使用硬件支持的原子指令。
3.  **屏障**：使用硬件支持的同步屏障。

![](img/849f9634ffa9fef9cf255d58f370c34c_23.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_25.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_27.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_29.png)

其中，原子操作和屏障需要硬件支持，而控制流+数据流的方式则完全在软件层面实现。

![](img/849f9634ffa9fef9cf255d58f370c34c_31.png)

---

![](img/849f9634ffa9fef9cf255d58f370c34c_33.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_35.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_37.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_39.png)

## 任务：实现两线程互斥锁
我们的目标是：仅使用控制流和数据流（不使用原子操作、锁或屏障），为两个线程实现互斥锁机制。

![](img/849f9634ffa9fef9cf255d58f370c34c_41.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_43.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_45.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_47.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_49.png)

以下是几种尝试方案及其分析。

![](img/849f9634ffa9fef9cf255d58f370c34c_51.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_53.png)

### 方案一：简单标志法
我们首先尝试一个简单的方案，使用一个共享的布尔标志 `flag`。

![](img/849f9634ffa9fef9cf255d58f370c34c_55.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_57.png)

```c
// 线程 T1 的代码
while(!flag); // 等待
S1; // 临界区
flag = false;

// 线程 T2 的代码
while(flag); // 等待
S2; // 临界区
flag = true;
```
**假设**：`flag` 初始值为 `false`。

**分析**：
*   **互斥性**：❌ **不保证**。如果 `flag` 初始值为 `true`，两个线程的 `while` 循环条件会同时为假，导致它们同时进入临界区。
*   **进展性**：❌ **不保证**。执行顺序被固定：必须先执行 `S2`，然后才能执行 `S1`。如果 `T2` 不执行，`T1` 将永远等待。
*   **死锁**：✅ **不会发生**。因为 `flag` 的值非真即假，不会导致两个线程都卡在 `while` 循环中。

![](img/849f9634ffa9fef9cf255d58f370c34c_59.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_61.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_63.png)

**结论**：此方案存在严重缺陷，无法保证互斥性。

![](img/849f9634ffa9fef9cf255d58f370c34c_65.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_67.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_69.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_71.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_73.png)

---

![](img/849f9634ffa9fef9cf255d58f370c34c_75.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_77.png)

### 方案二：对称等待法
我们尝试修改方案一，让两个线程对称地等待对方。

![](img/849f9634ffa9fef9cf255d58f370c34c_79.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_81.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_83.png)

```c
// 线程 T1 的代码
while(!flag);
S1;
flag = false;

![](img/849f9634ffa9fef9cf255d58f370c34c_85.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_87.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_89.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_91.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_93.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_95.png)

// 线程 T2 的代码
while(flag);
S2;
flag = true;
```
**假设**：`flag` 初始值为 `false`。`flag` 被声明为 `volatile`，确保读写直接作用于主存，避免缓存一致性问题。

![](img/849f9634ffa9fef9cf255d58f370c34c_97.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_99.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_101.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_103.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_104.png)

**分析**：
*   **互斥性**：✅ **保证**。`flag` 在同一时刻只能有一个值（`true` 或 `false`），因此只有一个线程能通过 `while` 循环。
*   **进展性**：❌ **不保证**。如果 `flag` 初始为 `true` 且 `T1` 永远不想进入临界区，那么想进入临界区的 `T2` 将永远在 `while(flag);` 处等待。
*   **死锁**：❌ **可能发生**。考虑以下执行顺序：
    1.  `T1` 执行 `flag = false;`（假设之前已执行完 `S1`）。
    2.  `T2` 执行 `flag = true;`。
    3.  `T1` 和 `T2` 几乎同时到达各自的 `while` 循环。
    此时，`flag` 的最终值为 `true`（后写入者生效）。`T1` 看到 `while(!true)` 为假，进入 `S1`。`T2` 看到 `while(true)` 为真，陷入无限等待。这虽然不是一个典型的“双方互相等待”的死锁，但导致了 `T2` 饥饿，从效果上看类似于死锁。更精确地说，在并发执行时，**可能**导致一个线程永远等待。

![](img/849f9634ffa9fef9cf255d58f370c34c_106.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_108.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_110.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_112.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_113.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_115.png)

**结论**：此方案保证了互斥性，但无法保证进展性，且在某些执行顺序下会导致线程饥饿/死锁。

![](img/849f9634ffa9fef9cf255d58f370c34c_117.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_119.png)

---

### 方案三：数组声明法 (Lock Version 1)
我们尝试实现通用的 `lock()` 和 `unlock()` 函数，使用一个标志数组 `flag[2]`。

![](img/849f9634ffa9fef9cf255d58f370c34c_121.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_123.png)

```c
volatile bool flag[2] = {false, false};

void lock(int tid) {
    int me = tid; // 0 或 1
    int other = 1 - me;
    flag[me] = true; // 声明自己希望进入临界区
    while(flag[other] == true); // 如果对方也希望进入，则等待
}

void unlock(int tid) {
    flag[tid] = false; // 声明自己离开临界区
}
```

**分析**：
*   **互斥性**：✅ **保证**。只有当对方线程的标志为 `false` 时，当前线程才能进入临界区。
*   **进展性**：✅ **保证**。即使一个线程不参与（其 `flag` 为 `false`），另一个线程也能顺利进入临界区。
*   **死锁**：❌ **可能发生**。考虑以下执行顺序：
    1.  `T0` 执行 `flag[0] = true;`
    2.  `T1` 执行 `flag[1] = true;`
    3.  现在 `flag[0]` 和 `flag[1]` 都为 `true`。
    4.  `T0` 执行 `while(flag[1] == true);` 陷入等待。
    5.  `T1` 执行 `while(flag[0] == true);` 陷入等待。
    双方都在等待对方将标志置为 `false`，形成死锁。

![](img/849f9634ffa9fef9cf255d58f370c34c_125.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_127.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_129.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_130.png)

**结论**：此方案保证了互斥性和进展性，但存在死锁风险。

![](img/849f9634ffa9fef9cf255d58f370c34c_132.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_134.png)

---

### 方案四：牺牲变量法 (Lock Version 2)
我们引入一个共享的 `victim` 变量来打破对称性，避免死锁。

![](img/849f9634ffa9fef9cf255d58f370c34c_136.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_138.png)

```c
volatile int victim;

![](img/849f9634ffa9fef9cf255d58f370c34c_140.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_142.png)

void lock(int tid) {
    int me = tid;
    victim = me; // 主动“牺牲”自己，让对方优先
    while(victim == me); // 如果自己仍然是“牺牲者”，则等待
}

![](img/849f9634ffa9fef9cf255d58f370c34c_144.png)

void unlock(int tid) {
    // 解锁操作简单，无需修改 victim
    // victim 的值会在下一次 lock 竞争时被覆盖
}
```

**分析**：
*   **互斥性**：✅ **保证**。`victim` 只能为 0 或 1。最后执行 `victim = me;` 的线程会成为真正的“牺牲者”，在其 `while` 循环中等待。
*   **进展性**：❌ **不保证**。如果只有一个线程（如 `T0`）调用 `lock()`，它会执行 `victim = 0;`，然后发现 `victim == 0` 为真，从而在 `while` 循环中无限等待，即使没有竞争者。
*   **死锁**：✅ **不会发生**。`victim` 的最终值会确定一个唯一的等待线程，另一个线程总能进入临界区。

![](img/849f9634ffa9fef9cf255d58f370c34c_146.png)

**结论**：此方案保证了互斥性且无死锁，但无法保证进展性（单个线程无法独立工作）。

---

### 最终方案：Peterson算法 (Lock Version 3)
Peterson算法巧妙地结合了**声明意向** (`flag[]`) 和**礼貌谦让** (`victim`) 两种机制。

![](img/849f9634ffa9fef9cf255d58f370c34c_148.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_150.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_152.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_154.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_156.png)

```c
volatile bool flag[2] = {false, false};
volatile int victim;

![](img/849f9634ffa9fef9cf255d58f370c34c_158.png)

void lock(int tid) {
    int me = tid;
    int other = 1 - me;
    flag[me] = true; // 步骤1：声明自己希望进入
    victim = me;      // 步骤2：主动让对方优先
    // 步骤3：等待条件：当对方想进入，并且自己是被礼让者时，继续等待
    while(flag[other] == true && victim == me);
}

void unlock(int tid) {
    flag[tid] = false; // 离开临界区，取消声明
}
```

**分析**：
*   **互斥性**：✅ **保证**。进入临界区的条件是：`flag[other] == false`（对方不想进）**或** `victim == other`（对方是牺牲者）。两个线程不可能同时使对方的条件为假。
*   **进展性**：✅ **保证**。如果只有一个线程想进入（`flag[other] == false`），它将立即通过 `while` 循环。即使两个线程都想进入，`victim` 变量也能确保其中之一（最后设置 `victim` 的线程）会等待，另一个则进入。
*   **死锁**：✅ **不会发生**。`victim` 变量确保了至少有一个线程能通过 `while` 循环。不会出现双方无限等待的情况。

**核心思想**：
1.  `flag[me] = true`：大声宣布“我想进去”。
2.  `victim = me`：礼貌地说“您先请”。
3.  `while(...)`：等待的条件是“您想进，并且您还让我先请”，那我就继续等。否则（您不想进，或者该我进了），我就进去。

![](img/849f9634ffa9fef9cf255d58f370c34c_160.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_162.png)

---

## 总结
本节课我们一起学习了如何仅用控制流和数据流实现两线程互斥锁。我们经历了多个方案的迭代：
1.  **简单标志法**：互斥性和进展性均无法保证。
2.  **对称等待法**：保证互斥性，但无进展性，可能饥饿。
3.  **数组声明法 (V1)**：保证互斥性和进展性，但存在死锁风险。
4.  **牺牲变量法 (V2)**：保证互斥性且无死锁，但无进展性。
5.  **Peterson算法 (V3)**：完美地同时保证了**互斥性**、**进展性**和**无死锁**。

![](img/849f9634ffa9fef9cf255d58f370c34c_164.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_166.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_168.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_170.png)

![](img/849f9634ffa9fef9cf255d58f370c34c_172.png)

Peterson算法是一个经典的软件互斥解决方案，它优雅地展示了如何通过共享变量和程序逻辑来解决复杂的同步问题。需要注意的是，它只适用于**两个线程**的场景。在下一节课中，我们将探讨如何将互斥锁的概念扩展到**多个线程**。