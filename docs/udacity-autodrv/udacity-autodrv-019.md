# 019：05 实现矩阵类（第二部分）

在本节课中，我们将继续深入学习如何实现一个完整的矩阵类，这是构建自动驾驶系统中感知与控制模块的基础数学工具。

---

上一节我们介绍了矩阵类的基本框架，本节中我们来看看如何为这个类添加更多实用的功能。

## 核心功能实现

以下是矩阵类需要实现的核心运算方法。

### 矩阵加法

矩阵加法要求两个矩阵的维度必须相同。其运算规则是：将两个矩阵中对应位置的元素相加。

**公式**：`C[i][j] = A[i][j] + B[i][j]`

```python
def add(self, other_matrix):
    if self.rows != other_matrix.rows or self.cols != other_matrix.cols:
        raise ValueError(“矩阵维度不匹配，无法相加。”)
    result_data = [
        [self.data[i][j] + other_matrix.data[i][j] for j in range(self.cols)]
        for i in range(self.rows)
    ]
    return Matrix(result_data)
```

### 矩阵减法

矩阵减法与加法类似，同样要求维度一致，运算规则是对应元素相减。

**公式**：`C[i][j] = A[i][j] - B[i][j]`

```python
def subtract(self, other_matrix):
    if self.rows != other_matrix.rows or self.cols != other_matrix.cols:
        raise ValueError(“矩阵维度不匹配，无法相减。”)
    result_data = [
        [self.data[i][j] - other_matrix.data[i][j] for j in range(self.cols)]
        for i in range(self.rows)
    ]
    return Matrix(result_data)
```

### 矩阵乘法

矩阵乘法分为两种：标量乘法和矩阵乘法。标量乘法是将矩阵的每个元素乘以一个常数。矩阵乘法规则是：第一个矩阵的列数必须等于第二个矩阵的行数，结果矩阵的第i行第j列元素等于第一个矩阵第i行与第二个矩阵第j列的点积。

**标量乘法公式**：`B[i][j] = k * A[i][j]`

**矩阵乘法公式**：`C[i][j] = sum(A[i][k] * B[k][j]) for k in range(A.cols)`

```python
def multiply(self, other):
    # 标量乘法
    if isinstance(other, (int, float)):
        result_data = [[self.data[i][j] * other for j in range(self.cols)] for i in range(self.rows)]
        return Matrix(result_data)
    # 矩阵乘法
    elif isinstance(other, Matrix):
        if self.cols != other.rows:
            raise ValueError(“第一个矩阵的列数必须等于第二个矩阵的行数。”)
        result_data = [
            [
                sum(self.data[i][k] * other.data[k][j] for k in range(self.cols))
                for j in range(other.cols)
            ]
            for i in range(self.rows)
        ]
        return Matrix(result_data)
    else:
        raise TypeError(“参数类型必须是数值或Matrix对象。”)
```

### 矩阵转置

矩阵转置是将矩阵的行和列互换。对于一个m×n的矩阵，其转置是一个n×m的矩阵。

**公式**：`B[j][i] = A[i][j]`

```python
def transpose(self):
    result_data = [
        [self.data[j][i] for j in range(self.rows)]
        for i in range(self.cols)
    ]
    return Matrix(result_data)
```

---

## 代码的优雅性与复用性

实现这样一个结构清晰、功能完备的矩阵类非常有价值。整洁、美观且可复用的代码是优秀软件工程师的标志。对这种编码工作充满热情，能促使你写出真正出色的代码。

---

本节课中我们一起学习了如何为矩阵类实现加法、减法、乘法及转置等核心运算。掌握这些基础操作，是你在无人驾驶领域进行计算机视觉、感知与控制算法开发的坚实第一步。