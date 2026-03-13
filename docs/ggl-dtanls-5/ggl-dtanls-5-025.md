# 025：子查询 🧩

![](img/8ebcd6625c7453bd321aad7eb2d58ad5_0.png)

在本节课中，我们将学习SQL中的子查询。子查询是一种嵌套在更大查询内部的SQL查询，它允许你将不同的逻辑片段组合在一起，使查询更高效、更易读。

## 什么是子查询？🤔

上一节我们介绍了基础的SQL查询，本节中我们来看看子查询。子查询很像俄罗斯套娃。你的主查询可以包含一个子查询，而这个子查询内部又可以包含另一个子查询，层层嵌套。但将它们组合在一起时，就构成了一个完整的查询。

包含子查询的语句可以称为**外部查询**或**外部选择**。这使得子查询成为**内部查询**或**内部选择**。内部查询首先执行，其结果被传递给外部查询使用。

子查询可能看起来有些复杂，因为涉及多层结构。但请记住，最内层的查询最先执行，这有助于你安排子查询的执行顺序。

## 子查询的常见用法 📝

子查询可以嵌套在各种其他查询中，通常出现在`FROM`或`WHERE`子句中。以下是几种常见的子查询用法。

### 在SELECT语句中使用子查询

我们从一个使用共享单车数据的例子开始。假设我们想比较某个站点的可用单车数量与所有站点的平均可用单车数量。

首先，我们构建外部查询，选择站点ID和可用单车数量。

```sql
SELECT station_id, bikes_available
```

然后，我们将计算平均可用单车数量的查询作为子查询嵌入。

```sql
SELECT station_id, bikes_available,
       (SELECT AVG(bikes_available) FROM `bigquery-public-data.london_bicycles.cycle_stations`) AS avg_bikes_available
FROM `bigquery-public-data.london_bicycles.cycle_stations`;
```

运行此查询后，我们得到一个表格，其中包含各站点的可用单车数量以及所有站点的平均可用单车数量。

### 在FROM语句中使用子查询

接下来，我们尝试在`FROM`语句中使用子查询。我们可以计算每个站点随时间开始的骑行次数。

我们以外部查询开始，选择站点ID、名称和骑行次数。

```sql
SELECT station_id, name, number_of_rides
```

在`FROM`子句中，我们嵌入一个子查询来计算每个起始站点的骑行次数。

```sql
SELECT s.station_id, s.name, ride_counts.number_of_rides
FROM `bigquery-public-data.london_bicycles.cycle_stations` AS s
JOIN (
    SELECT start_station_id, COUNT(*) AS number_of_rides
    FROM `bigquery-public-data.london_bicycles.cycle_hire`
    GROUP BY start_station_id
) AS ride_counts
ON s.station_id = ride_counts.start_station_id
ORDER BY number_of_rides DESC;
```

运行此查询后，我们得到每个站点开始的骑行次数列表。

### 在WHERE语句中使用子查询

最后，我们看看如何在`WHERE`语句中使用子查询。共享单车公司有两种用户：订阅用户和一次性客户。假设我们想要订阅用户使用过的站点列表。

我们以外部查询开始，从数据集中选择站点ID和名称。

```sql
SELECT station_id, name
FROM `bigquery-public-data.london_bicycles.cycle_stations`
```

这次，我们在`WHERE`子句中使用`IN`操作符来指定多个值，并嵌入一个子查询。

```sql
SELECT station_id, name
FROM `bigquery-public-data.london_bicycles.cycle_stations`
WHERE station_id IN (
    SELECT DISTINCT start_station_id
    FROM `bigquery-public-data.london_bicycles.cycle_hire`
    WHERE user_type = 'Subscriber'
);
```

值得注意的是，你可以在子查询中使用比较操作符，甚至是多行操作符如`IN`、`ANY`或`ALL`。在这个例子中，我们使用等号来指定只获取订阅用户的数据。

运行此查询后，我们得到符合条件（即订阅用户使用过）的站点ID和名称。

## 总结与练习建议 🎯

本节课中我们一起学习了SQL子查询。子查询虽然具有挑战性，需要考虑多层逻辑，并且在练习时可能会遇到错误，但这完全正常。面对挑战意味着你在成长。

建议你花时间练习这个新概念。在接下来的内容中，你将有机会使用子查询来聚合数据，或者继续进行每周的挑战。你将运用所学知识，如使用VLOOKUP、不同的JOIN类型以及子查询，来完成即将到来的评估。

如果你觉得这些内容复杂，可以在继续前进前花点时间复习这些视频。完成挑战后，我们将一起开始下一个重要的学习冒险。

![](img/8ebcd6625c7453bd321aad7eb2d58ad5_2.png)

![](img/8ebcd6625c7453bd321aad7eb2d58ad5_3.png)

祝你学习愉快！🚀