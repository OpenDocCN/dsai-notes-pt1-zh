# 014：在Apache Airflow中将数据流水线表示为DAG的优势 🚀

![](img/f02780fed08073a7bad0a03e33513511_0.png)

![](img/f02780fed08073a7bad0a03e33513511_1.png)

在本节课中，我们将学习如何在Apache Airflow中使用有向无环图（DAG）来表示数据流水线。我们将探讨DAG的基本概念、其组成部分，以及将工作流定义为代码所带来的关键优势。

---

![](img/f02780fed08073a7bad0a03e33513511_3.png)

![](img/f02780fed08073a7bad0a03e33513511_4.png)

## 什么是DAG？ 📊

![](img/f02780fed08073a7bad0a03e33513511_5.png)

上一节我们介绍了课程概述，本节中我们来看看DAG的定义。DAG是一种特殊类型的图，称为有向无环图。

一个简单的图由**节点**和**边**组成。例如，圆圈代表节点，连接节点的线代表边。有向图在此基础上增加了方向性，每条边都有一个指定的方向，连接一个起始节点和另一个节点。最后，“无环”意味着图中没有循环，即不存在一系列有向边能够返回到链中的某个节点。

**公式**：`DAG = (V, E)`，其中`V`是节点集合，`E`是有向边集合，且不存在环。

![](img/f02780fed08073a7bad0a03e33513511_7.png)

![](img/f02780fed08073a7bad0a03e33513511_8.png)

![](img/f02780fed08073a7bad0a03e33513511_9.png)

---

![](img/f02780fed08073a7bad0a03e33513511_10.png)

## DAG的示例 🌳

![](img/f02780fed08073a7bad0a03e33513511_12.png)

![](img/f02780fed08073a7bad0a03e33513511_13.png)

以下是几个DAG的示例。

![](img/f02780fed08073a7bad0a03e33513511_14.png)

![](img/f02780fed08073a7bad0a03e33513511_16.png)

最简单的非平凡DAG包含一条有向边，它有一个单一的根节点连接到一个单一的终端节点。

另一个常见的DAG示例是树形结构，常用于表示家谱或目录结构。所有树都是DAG，但并非所有DAG都是树。例如，一个DAG可能拥有多个根节点，而树则不行。DAG不施加这些限制，因此单个节点可以有多个父节点，也可能存在多个没有父节点的节点。

---

![](img/f02780fed08073a7bad0a03e33513511_18.png)

![](img/f02780fed08073a7bad0a03e33513511_19.png)

![](img/f02780fed08073a7bad0a03e33513511_20.png)

## 在Airflow中使用DAG表示工作流 ⚙️

![](img/f02780fed08073a7bad0a03e33513511_21.png)

DAG在Apache Airflow中用于表示工作流或流水线。数据流水线中执行的每个任务都表示为DAG中的一个节点，而流水线中两个任务之间的每个依赖关系则表示为DAG中的一条有向边。换句话说，边定义了两个任务应该运行的顺序。因此，Airflow使用DAG来定义应该运行哪些任务以及它们应该以何种顺序运行。

DAG在一个Python脚本中定义，该脚本代表了DAG的结构。因此，任务及其依赖关系被定义为代码。同时，调度指令也在DAG脚本中作为代码指定。

---

![](img/f02780fed08073a7bad0a03e33513511_23.png)

## DAG中的任务与操作符 🔧

![](img/f02780fed08073a7bad0a03e33513511_24.png)

![](img/f02780fed08073a7bad0a03e33513511_25.png)

![](img/f02780fed08073a7bad0a03e33513511_26.png)

![](img/f02780fed08073a7bad0a03e33513511_27.png)

让我们更仔细地看看DAG中的节点或任务。就像DAG本身一样，在DAG中执行的每个任务也是用Python编写的。每个任务实现一个**操作符**。

以下是几种操作符的示例：
*   **PythonOperator**：用于部署一些Python代码。
*   **SQLOperator**：用于运行SQL查询。
*   **BashOperator**：用于运行bash命令。

操作符用于定义DAG中每个任务的功能。

**传感器**是一类特殊的操作符，用于等待某个时间或条件被满足。例如，您可以使用传感器每30秒检查一次文件是否存在，或者检查另一个DAG是否已完成运行。

![](img/f02780fed08073a7bad0a03e33513511_29.png)

![](img/f02780fed08073a7bad0a03e33513511_30.png)

还有其他许多类型的操作符，包括电子邮件和HTTP请求操作符。

![](img/f02780fed08073a7bad0a03e33513511_31.png)

![](img/f02780fed08073a7bad0a03e33513511_32.png)

---

## DAG定义文件的结构 📝

![](img/f02780fed08073a7bad0a03e33513511_34.png)

![](img/f02780fed08073a7bad0a03e33513511_35.png)

一个Apache Airflow DAG是一个Python脚本，由以下逻辑块组成：

以下是DAG定义脚本的主要组成部分：
1.  **库导入**：导入所需的Python库。
2.  **DAG参数**：指定DAG的默认参数。
3.  **DAG定义**：实例化DAG对象。
4.  **任务定义**：定义DAG中的各个任务（节点）。
5.  **任务流水线**：指定任务之间的依赖关系（边）。

让我们简要地看一个例子。

**代码示例**：
```python
# 1. 库导入
from airflow import DAG
from airflow.operators.bash_operator import BashOperator
from datetime import datetime

# 2. DAG参数
default_args = {
    'owner': 'airflow',
    'start_date': datetime(2023, 10, 27),
}

![](img/f02780fed08073a7bad0a03e33513511_37.png)

# 3. DAG定义
dag = DAG('example_dag', default_args=default_args, schedule_interval='@daily')

# 4. 任务定义
task1 = BashOperator(
    task_id='print_date',
    bash_command='date',
    dag=dag,
)

task2 = BashOperator(
    task_id='sleep',
    bash_command='sleep 5',
    dag=dag,
)

![](img/f02780fed08073a7bad0a03e33513511_39.png)

# 5. 任务流水线
task2.set_upstream(task1)  # task2 依赖于 task1
```

![](img/f02780fed08073a7bad0a03e33513511_40.png)

![](img/f02780fed08073a7bad0a03e33513511_41.png)

![](img/f02780fed08073a7bad0a03e33513511_42.png)

![](img/f02780fed08073a7bad0a03e33513511_43.png)

---

![](img/f02780fed08073a7bad0a03e33513511_44.png)

## Airflow调度器 🗓️

您的新DAG已经创建，但尚未部署。为此，**Airflow调度器**被设计为在Airflow生产环境中作为持久服务运行。

Apache Airflow调度器可用于在多个工作节点上部署您的工作流。它遵循您在DAG中指定的任务和依赖关系。一旦启动Airflow调度器实例，您的DAG将根据您在每个DAG中作为代码指定的开始日期开始运行。之后，调度器根据您指定的调度间隔触发每个后续的DAG运行。

![](img/f02780fed08073a7bad0a03e33513511_46.png)

![](img/f02780fed08073a7bad0a03e33513511_47.png)

---

![](img/f02780fed08073a7bad0a03e33513511_48.png)

![](img/f02780fed08073a7bad0a03e33513511_49.png)

![](img/f02780fed08073a7bad0a03e33513511_50.png)

## 将工作流定义为代码的优势 ✨

![](img/f02780fed08073a7bad0a03e33513511_51.png)

![](img/f02780fed08073a7bad0a03e33513511_52.png)

![](img/f02780fed08073a7bad0a03e33513511_53.png)

Apache Airflow将数据流水线表示为DAG的方法的一个关键优势在于它们被表达为**代码**。

当工作流被定义为代码时，它们变得：
*   **更易于维护**：开发者可以通过阅读代码来明确理解已指定的内容。
*   **可版本控制**：代码修订可以轻松地通过如Git这样的版本控制系统进行跟踪。
*   **可协作**：开发团队可以轻松地在整个工作流的代码开发和维护上进行协作。
*   **可测试**：任何修订都可以通过单元测试来确保代码仍按预期工作。

---

![](img/f02780fed08073a7bad0a03e33513511_55.png)

## 总结 🎯

![](img/f02780fed08073a7bad0a03e33513511_56.png)

![](img/f02780fed08073a7bad0a03e33513511_57.png)

![](img/f02780fed08073a7bad0a03e33513511_58.png)

本节课中我们一起学习了：
*   在Apache Airflow中，DAG是定义为Python代码的工作流。
*   **任务**是DAG中的节点，通过实现Airflow内置的**操作符**来创建。
*   **流水线**被指定为任务之间的依赖关系，这些是DAG中节点之间的有向边。
*   **Airflow调度器**负责调度和部署您的DAG。
*   最后，Apache Airflow将数据流水线表示为DAG的方法的关键优势在于它们被表达为代码，这使得您的数据流水线更易于维护、测试和协作。