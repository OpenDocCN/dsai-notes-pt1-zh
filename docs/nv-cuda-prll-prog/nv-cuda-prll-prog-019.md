# 019：CUDA中的多维数组 🚀

![](img/d54cfa444db3dbe398e015bafa7cae42_1.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_3.png)

在本节课中，我们将学习如何在CUDA中声明和使用多维数组。我们将通过三个具体的应用实例来深入理解这一概念：矩阵平方、二维最小值算法和模板计算。通过本课，你将学会如何更直观地处理多维数据，避免复杂的索引计算，并编写更易读的GPU代码。

---

![](img/d54cfa444db3dbe398e015bafa7cae42_5.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_7.png)

## 概述 📋

在之前的课程中，我们使用一维数组来表示二维矩阵，并通过公式 `i * matrix_size + j` 来访问元素。本节课，我们将学习如何直接在CUDA中声明和访问二维（甚至更高维）数组，从而使代码更简洁、更易维护。

---

## 矩阵平方应用 🔲

上一节我们回顾了使用一维数组进行矩阵平方计算的方法。本节中，我们来看看如何使用真正的二维数组来实现相同的功能。

在之前的实现中，我们使用一维数组并通过公式计算索引来模拟二维访问。现在，我们将直接在GPU上声明二维数组。

### CPU版本（使用一维数组）

```c
void cpu_square(float* matrix, float* result, int matrix_size) {
    for (int i = 0; i < matrix_size; i++) {
        for (int j = 0; j < matrix_size; j++) {
            for (int k = 0; k < matrix_size; k++) {
                result[i * matrix_size + j] += matrix[i * matrix_size + k] * matrix[k * matrix_size + j];
            }
        }
    }
}
```

### GPU版本（使用二维数组）

![](img/d54cfa444db3dbe398e015bafa7cae42_9.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_11.png)

以下是使用CUDA二维数组实现矩阵平方的关键步骤：

![](img/d54cfa444db3dbe398e015bafa7cae42_13.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_15.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_17.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_19.png)

1.  **在GPU上声明二维数组：**
    ```c
    __device__ float d_A[N][N];
    __device__ float d_B[N][N];
    ```

2.  **主机端代码配置：**
    ```c
    dim3 blocksPerGrid(1, 1);
    dim3 threadsPerBlock(N, N);
    // 使用 cudaMemcpyToSymbol 将数据从主机复制到设备
    cudaMemcpyToSymbol(d_A, A, size);
    cudaMemcpyToSymbol(d_B, B, size);
    // 启动核函数
    square_kernel<<<blocksPerGrid, threadsPerBlock>>>(N);
    // 将结果复制回主机
    cudaMemcpyFromSymbol(C, d_B, size);
    ```

![](img/d54cfa444db3dbe398e015bafa7cae42_21.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_23.png)

3.  **核函数实现：**
    ```c
    __global__ void square_kernel(int n) {
        int i = threadIdx.y;
        int j = threadIdx.x;
        if (i < n && j < n) {
            float sum = 0.0f;
            for (int k = 0; k < n; k++) {
                sum += d_A[i][k] * d_A[k][j];
            }
            d_B[i][j] = sum;
        }
    }
    ```

**核心优势：** 我们不再需要复杂的索引计算 `i * n + j`，可以直接使用 `d_A[i][j]` 这种直观的语法访问元素，代码可读性大大增强。

---

## 二维最小值算法 📉

![](img/d54cfa444db3dbe398e015bafa7cae42_25.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_27.png)

上一节我们介绍了矩阵操作，本节中我们来看看一个图像处理中常见的算法——二维最小值算法。

![](img/d54cfa444db3dbe398e015bafa7cae42_29.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_31.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_33.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_35.png)

该算法扫描输入矩阵中每个2x2的窗口，找出窗口内的最小值，并放入输出矩阵的对应位置。这类似于卷积操作。

### 算法定义

对于输出矩阵 `O` 中的元素 `O[i][j]`，其值为输入矩阵 `A` 中以下四个元素的最小值：
`A[i-1][j]`, `A[i-1][j+1]`, `A[i][j]`, `A[i][j+1]`

![](img/d54cfa444db3dbe398e015bafa7cae42_37.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_39.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_41.png)

边界情况（第一行和最后一列）保持不变。

![](img/d54cfa444db3dbe398e015bafa7cae42_43.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_45.png)

### CPU实现

![](img/d54cfa444db3dbe398e015bafa7cae42_47.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_49.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_51.png)

```c
void compute(float grid[N][N], float prev_grid[N][N]) {
    for (int i = 1; i < N; i++) { // 忽略第一行
        for (int j = 0; j < N-1; j++) { // 忽略最后一列
            float min_val = fminf(prev_grid[i-1][j], prev_grid[i-1][j+1]);
            min_val = fminf(min_val, prev_grid[i][j]);
            min_val = fminf(min_val, prev_grid[i][j+1]);
            grid[i][j] = min_val;
        }
    }
}
```

![](img/d54cfa444db3dbe398e015bafa7cae42_53.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_55.png)

### GPU实现

以下是GPU核函数的关键部分：

```c
__global__ void min2d_kernel(int n) {
    int i = blockIdx.y * blockDim.y + threadIdx.y;
    int j = blockIdx.x * blockDim.x + threadIdx.x;

    // 处理边界，避免第一行和最后一列
    if (i > 0 && j < n-1) {
        float min_val = fminf(d_prev_grid[i-1][j], d_prev_grid[i-1][j+1]);
        min_val = fminf(min_val, d_prev_grid[i][j]);
        min_val = fminf(min_val, d_prev_grid[i][j+1]);
        d_grid[i][j] = min_val;
    } else if (i == 0 || j == n-1) {
        // 边界直接复制
        d_grid[i][j] = d_prev_grid[i][j];
    }
}
```

![](img/d54cfa444db3dbe398e015bafa7cae42_57.png)

**实现要点：** 每个GPU线程负责计算一个输出元素。我们使用二维线程块来自然映射到矩阵的`(i, j)`坐标，并通过条件判断优雅地处理了边界情况。

![](img/d54cfa444db3dbe398e015bafa7cae42_59.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_60.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_62.png)

---

## 模板计算 🧮

上一节我们学习了空间上的邻域操作，本节中我们来看看一个涉及多次迭代的模板计算。

模板计算中，每个单元格的新值由其上方两个邻居的当前值决定，公式为：
`A[i][j] += A[i-1][j] + A[i-1][j+1]`
该计算需要迭代多次（例如10次），且每次迭代后，新值成为下一次迭代的输入。

![](img/d54cfa444db3dbe398e015bafa7cae42_64.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_66.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_68.png)

### 基础版本（使用额外空间）

![](img/d54cfa444db3dbe398e015bafa7cae42_70.png)

最简单的实现是使用两个独立的数组，一个用于读取（上一轮结果），一个用于写入（本轮结果）。

**GPU核函数示例：**
```c
__global__ void stencil_kernel(int n) {
    int i = blockIdx.y * blockDim.y + threadIdx.y;
    int j = blockIdx.x * blockDim.x + threadIdx.x;
    if (i > 0 && j < n-1) {
        d_grid_new[i][j] = d_grid_old[i][j] + d_grid_old[i-1][j] + d_grid_old[i-1][j+1];
    }
}
// 每次迭代后，需要将 d_grid_new 的数据复制到 d_grid_old
```

### 进阶挑战：原地计算

![](img/d54cfa444db3dbe398e015bafa7cae42_72.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_74.png)

现在，我们增加一个限制：**不能使用额外的数组**，必须在原输入矩阵上就地更新数据。

![](img/d54cfa444db3dbe398e015bafa7cae42_76.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_78.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_80.png)

这就带来了数据竞争问题：当线程正在读取`A[i-1][j]`和`A[i-1][j+1]`来计算`A[i][j]`的新值时，另一个负责计算`A[i-1][j]`的线程可能正在写入它。

![](img/d54cfa444db3dbe398e015bafa7cae42_82.png)

#### 解决方案：按行顺序执行

为了消除数据竞争，我们可以将并行度限制在单行内。即，每次只计算一行：
1.  从最后一行开始计算（因为它没有下方的依赖）。
2.  等待该行所有线程计算完成后，再开始计算上一行。
3.  依此类推，直到第一行。

![](img/d54cfa444db3dbe398e015bafa7cae42_84.png)

**主机端代码逻辑：**
```c
for (int iter = 0; iter < 10; iter++) {
    for (int row = N-1; row >= 1; row--) { // 从底向上
        // 1. 将当前行数据复制到设备
        // 2. 启动核函数，只计算这一行
        dim3 threadsPerBlock(N, 1); // 一行N个线程
        stencil_inplace_kernel<<<1, threadsPerBlock>>>(N, row);
        // 3. 将计算后的行复制回主机
    }
}
```

**核函数实现：**
```c
__global__ void stencil_inplace_kernel(int n, int current_row) {
    int j = threadIdx.x; // 列索引
    int i = current_row; // 行索引由主机传入
    if (j < n-1) { // 忽略最后一列
        d_grid[i][j] += d_grid[i-1][j] + d_grid[i-1][j+1];
    }
}
```

![](img/d54cfa444db3dbe398e015bafa7cae42_86.png)

**性能权衡：** 这种方法保证了正确性，但牺牲了并行度（从并行处理整个矩阵变为每次只并行处理一行）。另一种可能的方案是使用原子操作，但需要精心设计以避免将并行计算完全序列化。这可以作为课后思考题。

---

## 总结 🎯

![](img/d54cfa444db3dbe398e015bafa7cae42_88.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_90.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_92.png)

本节课我们一起学习了CUDA中多维数组的核心用法：

1.  **声明与访问**： 使用 `__device__ float arr[N][M]` 在GPU上声明二维数组，并可直接用 `arr[i][j]` 语法访问，无需手动计算线性索引。
2.  **三个应用实例**：
    *   **矩阵平方**：展示了用二维数组简化代码逻辑。
    *   **二维最小值算法**：演示了如何将图像处理类算法映射到二维线程网格，并处理边界条件。
    *   **模板计算**：深入探讨了迭代计算中的依赖问题，并给出了通过限制执行顺序来实现原地计算的方案。
3.  **核心思想**： 利用CUDA的二维/三维线程组织来自然匹配多维数据结构，使算法更清晰，并注意处理边界条件和迭代计算中的数据竞争问题。

![](img/d54cfa444db3dbe398e015bafa7cae42_94.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_96.png)

![](img/d54cfa444db3dbe398e015bafa7cae42_98.png)

通过掌握这些技巧，你可以更高效、更直观地在GPU上处理科学计算、图像处理等领域的多维数据问题。