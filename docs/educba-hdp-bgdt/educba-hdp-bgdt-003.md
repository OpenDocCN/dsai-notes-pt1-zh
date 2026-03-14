# 003：MapReduce实战案例

在本节课中，我们将学习如何使用Hadoop MapReduce框架处理真实数据，并生成有价值的分析结果。我们将基于一个菲律宾人口普查数据集，完成七个具体的分析用例，从计算男女比例到识别潜在的童工问题。通过实践，你将掌握如何编写MapReduce程序来解决实际问题。

## 概述

上一节我们完成了数据格式的转换，将JSON文件转换为了纯文本格式。本节中，我们将基于这个纯文本文件，编写并执行一系列MapReduce程序，以提取关键的人口统计信息。每个用例都将对应一个独立的MapReduce作业。

## 用例一：计算男女比例

首先，我们来处理第一个用例：计算菲律宾的男女人口比例。我们的数据属于菲律宾，因此可以统计该国的男女人口数量。

以下是实现此功能的MapReduce程序核心逻辑：

![](img/b186c018503f12301905d2729c2456f6_1.png)

![](img/b186c018503f12301905d2729c2456f6_3.png)

![](img/b186c018503f12301905d2729c2456f6_5.png)

**Mapper类** 负责读取每一行数据，提取“性别”字段，并将其作为键输出，值为1。这样，所有“男性”记录会聚合在一起，所有“女性”记录也会聚合在一起。

```java
public class GenderMapper extends Mapper<LongWritable, Text, Text, IntWritable> {
    private final static IntWritable one = new IntWritable(1);
    private Text gender = new Text();

    public void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
        String line = value.toString();
        String[] fields = line.split(",");
        // 假设性别字段在索引3的位置
        String gen = fields[3].trim();
        gender.set(gen);
        context.write(gender, one);
    }
}
```

**Reducer类** 接收来自Mapper的键值对（如 `male, [1,1,1...]`），并对相同键的所有值进行求和，得到该性别的总人数。

![](img/b186c018503f12301905d2729c2456f6_7.png)

![](img/b186c018503f12301905d2729c2456f6_8.png)

![](img/b186c018503f12301905d2729c2456f6_9.png)

![](img/b186c018503f12301905d2729c2456f6_11.png)

```java
public class GenderReducer extends Reducer<Text, IntWritable, Text, IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
            sum += val.get();
        }
        result.set(sum);
        context.write(key, result);
    }
}
```

![](img/b186c018503f12301905d2729c2456f6_13.png)

程序执行后，我们得到了输出结果：女性人数为1061，男性人数为939。政府可以根据这个男女比例数据来制定国家发展战略。

## 用例二：统计45岁以上且离异/丧偶的女性人数

![](img/b186c018503f12301905d2729c2456f6_15.png)

接下来，我们处理第二个用例：统计年龄超过45岁且婚姻状况为“离异”或“丧偶”的女性人数。这个信息有助于政府为这类弱势群体提供支持，例如开设相关组织或非政府机构。

![](img/b186c018503f12301905d2729c2456f6_17.png)

![](img/b186c018503f12301905d2729c2456f6_19.png)

![](img/b186c018503f12301905d2729c2456f6_21.png)

以下是实现此功能的MapReduce程序过滤逻辑：

在Mapper中，我们需要应用多个过滤条件。首先检查性别是否为女性，然后检查年龄是否大于45，最后检查婚姻状态是否为“widowed”或“divorced”。

![](img/b186c018503f12301905d2729c2456f6_23.png)

```java
public void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
    String line = value.toString();
    String[] fields = line.split(",");
    String gender = fields[3].trim();
    String ageStr = fields[0].trim();
    String maritalStatus = fields[2].trim().toLowerCase();

    if ("female".equals(gender)) {
        try {
            int age = Integer.parseInt(ageStr);
            if (age > 45 && ("widowed".equals(maritalStatus) || "divorced".equals(maritalStatus))) {
                context.write(new Text("qualifying_female"), one);
            }
        } catch (NumberFormatException e) {
            // 处理年龄字段非数字的情况
        }
    }
}
```

**注意**：在将字符串年龄转换为整数时，我们使用了`trim()`方法去除可能存在的空格，避免了`NumberFormatException`异常。

![](img/b186c018503f12301905d2729c2456f6_25.png)

![](img/b186c018503f12301905d2729c2456f6_27.png)

程序运行后，输出结果为100。这意味着有100名女性符合条件，政府可以据此采取相应措施。

![](img/b186c018503f12301905d2729c2456f6_29.png)

## 用例三：计算总收入

第三个用例是计算数据集中所有人的总收入。政府可以用这个数据，根据本国的税收规则计算预期所得税，并与实际税收进行对比。

![](img/b186c018503f12301905d2729c2456f6_31.png)

![](img/b186c018503f12301905d2729c2456f6_33.png)

在这个用例中，我们只需要关注“收入”字段。由于总收入可能很大，我们使用`LongWritable`类型来存储。

![](img/b186c018503f12301905d2729c2456f6_35.png)

**Mapper类** 提取收入字段，并将其作为值输出。键可以设为空或一个固定标签。

![](img/b186c018503f12301905d2729c2456f6_37.png)

```java
public class IncomeMapper extends Mapper<LongWritable, Text, Text, LongWritable> {
    public void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
        String line = value.toString();
        String[] fields = line.split(",");
        // 假设收入字段在索引5的位置
        String incomeStr = fields[5].trim();
        try {
            long income = Long.parseLong(incomeStr);
            context.write(new Text("total_income"), new LongWritable(income));
        } catch (NumberFormatException e) {
            // 处理异常
        }
    }
}
```

**Reducer类** 将所有收入值相加。

![](img/b186c018503f12301905d2729c2456f6_39.png)

![](img/b186c018503f12301905d2729c2456f6_41.png)

```java
public class IncomeReducer extends Reducer<Text, LongWritable, Text, LongWritable> {
    public void reduce(Text key, Iterable<LongWritable> values, Context context) throws IOException, InterruptedException {
        long sum = 0;
        for (LongWritable val : values) {
            sum += val.get();
        }
        context.write(key, new LongWritable(sum));
    }
}
```

![](img/b186c018503f12301905d2729c2456f6_43.png)

最终输出总收入的数值，政府可以基于此进行税收估算。

![](img/b186c018503f12301905d2729c2456f6_45.png)

## 用例四：预测下一年可能新增的纳税人

第四个用例是预测政府下一年可以期待多少新增所得税。这通过统计刚满18岁且有收入（即可能开始工作并纳税）的人数来实现。

Mapper的逻辑是筛选出年龄等于18岁且收入不为0的记录。

```java
if (age == 18 && income > 0) {
    context.write(new Text("new_taxpayers"), one);
}
```

![](img/b186c018503f12301905d2729c2456f6_47.png)

程序输出结果为27，意味着预计有27人将在下一年成为新的纳税人。政府可以根据这个数字调整税收预期。

![](img/b186c018503f12301905d2729c2456f6_49.png)

![](img/b186c018503f12301905d2729c2456f6_51.png)

![](img/b186c018503f12301905d2729c2456f6_52.png)

![](img/b186c018503f12301905d2729c2456f6_54.png)

## 用例五：统计各教育水平人数

![](img/b186c018503f12301905d2729c2456f6_56.png)

第五个用例是分析人口的教育构成。我们通过按“教育”字段分组并计数来实现。

Mapper将教育水平作为键输出。

```java
String education = fields[1].trim();
context.write(new Text(education), one);
```

Reducer对每个教育水平进行计数。输出结果显示了各个教育类别的人数，例如“10th grade: 81”、“Doctorate: 15”等。政府可以据此了解国家的教育状况。

## 用例六：统计非菲律宾籍人口

![](img/b186c018503f12301905d2729c2456f6_58.png)

第六个用例是统计非菲律宾籍的人口数量，这涉及“出生国”字段。政府可以借此管理签证和移民配额。

Mapper提取出生国字段作为键。

![](img/b186c018503f12301905d2729c2456f6_60.png)

![](img/b186c018503f12301905d2729c2456f6_62.png)

```java
String countryOfBirth = fields[7].trim(); // 假设是第7个字段
context.write(new Text(countryOfBirth), one);
```

输出结果显示了许多其他国家及其对应人数，其中仅有8人来自菲律宾本土。这些数据对移民政策制定有参考价值。

![](img/b186c018503f12301905d2729c2456f6_64.png)

## 用例七：识别潜在童工（18岁以下有收入者）

最后一个用例是识别可能违反童工法的案例，即年龄小于18岁但有收入记录的人。

Mapper的过滤条件为年龄小于18且收入大于0。

![](img/b186c018503f12301905d2729c2456f6_66.png)

```java
if (age < 18 && income > 0) {
    context.write(new Text("child_labor"), one);
}
```

程序输出结果为535，表示有535名未成年人有收入记录。政府可以利用这些信息对雇佣童工的行为进行调查和处理。

![](img/b186c018503f12301905d2729c2456f6_68.png)

## 总结

![](img/b186c018503f12301905d2729c2456f6_70.png)

本节课中，我们一起学习了如何使用Hadoop MapReduce处理人口普查数据，并完成了七个实际分析用例。我们从计算基本的男女比例开始，逐步深入到更复杂的分析，如识别弱势群体、预测税收、分析教育背景和移民状况，以及检测潜在的违法行为。通过这些实践，我们掌握了如何将原始数据转化为对政府决策有价值的信息。在接下来的课程中，我们将使用Pig和Hive这些更高级的工具来实现相同的分析，以比较不同大数据处理技术的优劣。