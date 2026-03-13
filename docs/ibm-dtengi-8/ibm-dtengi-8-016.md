# 016：使用Airflow构建DAG 🛠️

![](img/9b863525c968314fe4f9d5135bbb2e23_0.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_1.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_3.png)

在本节课中，我们将学习如何将Airflow管道编写为一个定义Airflow DAG对象的Python脚本。我们将了解DAG定义文件的关键组成部分，学习如何通过实例化操作符来创建任务，并设置任务之间的依赖关系。

---

## 概述

一个Apache Airflow DAG本质上是一个Python脚本。它由几个逻辑块构成，包括库导入、DAG参数定义、DAG实例化、任务定义以及任务依赖关系设置。理解这些组成部分是构建有效数据管道的基础。

![](img/9b863525c968314fe4f9d5135bbb2e23_5.png)

---

![](img/9b863525c968314fe4f9d5135bbb2e23_6.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_7.png)

## DAG定义文件的逻辑块

一个典型的Airflow DAG定义脚本包含以下五个逻辑块。我们将逐一进行介绍。

![](img/9b863525c968314fe4f9d5135bbb2e23_9.png)

### 1. Python库导入

![](img/9b863525c968314fe4f9d5135bbb2e23_10.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_11.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_12.png)

首先，我们需要导入构建DAG所需的Python库。以下是导入语句的示例：

```python
from airflow import DAG
from airflow.operators.bash_operator import BashOperator
from datetime import datetime
```

*   `DAG` 类用于定义工作流。
*   `BashOperator` 用于创建执行Bash命令的任务。
*   `datetime` 模块用于处理时间相关的参数。

![](img/9b863525c968314fe4f9d5135bbb2e23_14.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_15.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_16.png)

### 2. DAG参数定义

![](img/9b863525c968314fe4f9d5135bbb2e23_17.png)

接下来，我们定义一个Python字典来指定DAG的默认参数。这些参数将应用于DAG中的所有任务。

```python
default_args = {
    'owner': 'you',
    'start_date': datetime(2021, 7, 28),
    'retries': 1,
    'retry_delay': timedelta(minutes=5)
}
```

*   `owner`：DAG的所有者。
*   `start_date`：DAG开始运行的日期。
*   `retries`：任务失败时的重试次数。
*   `retry_delay`：重试之间的等待时间。

![](img/9b863525c968314fe4f9d5135bbb2e23_19.png)

### 3. DAG实例化

![](img/9b863525c968314fe4f9d5135bbb2e23_20.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_21.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_22.png)

在这一步，我们将工作流实例化为一个DAG对象。我们可以在此设置DAG的名称、描述、默认参数和调度计划。

![](img/9b863525c968314fe4f9d5135bbb2e23_23.png)

```python
dag = DAG(
    'simple_example',
    description='一个简单的示例DAG',
    default_args=default_args,
    schedule_interval=timedelta(seconds=5)
)
```

*   `'simple_example'`：DAG的唯一标识符。
*   `description`：对DAG功能的描述。
*   `default_args`：应用上面定义的默认参数字典。
*   `schedule_interval`：DAG运行的调度间隔，例如每5秒运行一次。

### 4. 任务定义

![](img/9b863525c968314fe4f9d5135bbb2e23_25.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_26.png)

任务定义是DAG的核心，每个任务都是DAG中的一个节点。我们通过实例化操作符来创建任务。

![](img/9b863525c968314fe4f9d5135bbb2e23_27.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_28.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_29.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_30.png)

以下是定义两个Bash任务的示例：

![](img/9b863525c968314fe4f9d5135bbb2e23_31.png)

```python
t1 = BashOperator(
    task_id='print_hello',
    bash_command='echo "Greetings, the date and time are:"',
    dag=dag
)

t2 = BashOperator(
    task_id='print_date',
    bash_command='date',
    dag=dag
)
```

![](img/9b863525c968314fe4f9d5135bbb2e23_33.png)

*   `task_id`：任务的唯一标识符。
*   `bash_command`：该任务要执行的Bash命令。
*   `dag=dag`：将此任务分配给上面创建的DAG实例。

![](img/9b863525c968314fe4f9d5135bbb2e23_34.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_35.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_36.png)

### 5. 任务依赖关系设置

![](img/9b863525c968314fe4f9d5135bbb2e23_38.png)

最后，我们需要指定任务之间的执行顺序，即定义任务管道。这决定了工作流的逻辑流程。

```python
t1 >> t2
```

*   `>>` 操作符表示“下游”或“之后运行”。这里表示 `t2` 在 `t1` 之后运行。也就是说，`print_hello` 任务成功完成后，`print_date` 任务才会开始执行。

![](img/9b863525c968314fe4f9d5135bbb2e23_40.png)

---

![](img/9b863525c968314fe4f9d5135bbb2e23_41.png)

## 总结

本节课中，我们一起学习了如何构建一个Airflow DAG。我们了解到：

1.  Airflow管道是一个实例化Airflow DAG对象的Python脚本。
2.  DAG定义文件的关键组成部分包括：库导入、DAG参数、DAG定义、任务定义和任务管道规范。
3.  可以通过在DAG定义中设置 `schedule_interval` 参数来让DAG按计划重复运行。
4.  任务是通过实例化从Apache Airflow操作符模块导入的操作符来创建的。

![](img/9b863525c968314fe4f9d5135bbb2e23_43.png)

![](img/9b863525c968314fe4f9d5135bbb2e23_44.png)

通过掌握这些基本概念和结构，你已经具备了创建简单但功能完整的Airflow数据管道的能力。