# 027：为多个城市创建详细行程 🗺️

在本节课中，我们将综合运用之前学到的所有知识，利用Python和AI来规划一次环游世界的假期行程。我们将学习如何遍历多个文件，为每个城市提取关键信息，并利用大型语言模型生成详细的每日活动安排。

## 概述

上一节我们学习了如何读写文件以及调用AI模型。本节中，我们将把这些技能结合起来，构建一个更复杂的工作流：为一个包含多个城市的旅行计划，自动生成每个城市的详细行程，包括推荐的餐厅和特色菜肴。

## 读取行程数据

![](img/50ba23651961fb2281c83afab37bdd15_1.png)

首先，我们需要导入必要的辅助函数，并定义一个专门用于读取CSV文件的函数。

```python
def read_csv_file(filename):
    file = open(filename, 'r')
    data = file.read()
    file.close()
    return data
```

![](img/50ba23651961fb2281c83afab37bdd15_3.png)

![](img/50ba23651961fb2281c83afab37bdd15_4.png)

运行此函数后，我们可以读取存储行程的CSV文件。

```python
itinerary = read_csv_file('itinerary.csv')
print(itinerary)
```

![](img/50ba23651961fb2281c83afab37bdd15_6.png)

行程数据包含了我们计划访问的各个城市、国家、抵达和离开日期。

## 为单个城市生成行程

![](img/50ba23651961fb2281c83afab37bdd15_8.png)

![](img/50ba23651961fb2281c83afab37bdd15_9.png)

我们以悉尼为例，演示如何为一个城市生成详细行程。首先，我们需要加载悉尼的餐厅数据，这些数据已保存在一个CSV文件中。

```python
sydney_restaurants = read_csv_file('Sydney.csv')
```

接下来，我们从总行程中提取悉尼的具体信息，并构建一个提示词（prompt）来调用大型语言模型。

![](img/50ba23651961fb2281c83afab37bdd15_11.png)

```python
city = itinerary[6]['city']
country = itinerary[6]['country']
arrival = itinerary[6]['arrival']
departure = itinerary[6]['departure']
restaurants = sydney_restaurants

![](img/50ba23651961fb2281c83afab37bdd15_13.png)

prompt = f"""
请为{city}, {country}的旅行创建一份详细行程。
访问时间：从{arrival}到{departure}。
请规划每日活动，并指定早餐、午餐和晚餐的时间。
请考虑访问以下餐厅，并尝试其特色菜：
{restaurants}
"""
response = get_completion(prompt)
print(response)
```

![](img/50ba23651961fb2281c83afab37bdd15_15.png)

运行以上代码，AI模型将生成一份包含具体时间安排和餐厅推荐的悉尼详细行程。

## 为所有城市生成行程

现在，我们来看看如何将此过程自动化，为行程中的每一个城市生成详细安排。

以下是实现这一目标的核心步骤：

1.  创建一个空字典来存储所有城市的详细行程。
2.  遍历行程列表中的每一个目的地。
3.  为每个城市加载对应的餐厅CSV文件。
4.  为每个城市构建提示词并调用AI模型。
5.  将生成的行程保存到字典中。

![](img/50ba23651961fb2281c83afab37bdd15_17.png)

![](img/50ba23651961fb2281c83afab37bdd15_18.png)

```python
detailed_itineraries = {}

for stop in itinerary:
    city = stop['city']
    country = stop['country']
    arrival = stop['arrival']
    departure = stop['departure']
    
    # 加载该城市的餐厅文件
    restaurant_file = f"{city}.csv"
    restaurants = read_csv_file(restaurant_file)
    
    print(f"正在为{city}创建详细行程...")
    
    # 构建提示词
    prompt = f"""
    请为{city}, {country}的旅行创建一份详细行程。
    访问时间：从{arrival}到{departure}。
    请规划每日活动，并指定早餐、午餐和晚餐的时间。
    请考虑访问以下餐厅，并尝试其特色菜：
    {restaurants}
    """
    
    # 获取AI响应并存储
    response = get_completion(prompt)
    detailed_itineraries[city] = response
```

运行此循环后，`detailed_itineraries` 字典将包含所有城市的完整行程。例如，要查看东京的行程，可以执行：

![](img/50ba23651961fb2281c83afab37bdd15_20.png)

![](img/50ba23651961fb2281c83afab37bdd15_21.png)

```python
print(detailed_itineraries['Tokyo'])
```

![](img/50ba23651961fb2281c83afab37bdd15_22.png)

## 总结

本节课中我们一起学习了如何将文件操作、数据提取和AI模型调用整合到一个自动化工作流中。我们实现了：
*   读取包含多个城市信息的CSV行程文件。
*   为每个城市加载独立的餐厅数据文件。
*   利用大型语言模型为每个城市生成包含具体时间点和餐厅推荐的个性化详细行程。
*   将所有结果组织并存储在一个数据结构中，便于查阅。

通过这个项目，你可以看到，即使是初学者，也能利用现有的AI工具和Python基础，构建出功能强大的实用程序。我鼓励你运行并修改这段代码，例如调整提示词，告诉AI你是一个“早起者”或“晚起者”，以生成更符合你作息的行程。请务必查看不同城市的行程建议。

记住，如果对代码有任何疑问，你可以随时向AI助手提问。

恭喜你完成本课程！你已经学会了如何在Python中处理文本和CSV文件，以及如何利用大型语言模型高效地完成许多任务。几年前，要实现本课程所展示的内容可能非常困难，甚至对于经验丰富的程序员来说也是如此。但得益于AI语言模型的普及，现在的初学者程序员能够构建出几年前世界上最优秀的程序员都可能难以完成的应用程序。

这正是一个学习编程和使用AI的激动人心的时代。可能实现的事情范围正在迅速扩大，有许多事情是初学者程序员可以做到，而世界上还没有人尝试过的。因此，你有大量的机会编写代码来帮助自己、你的朋友以及工作和家庭。

在看过编写代码所能实现的一些功能之后，我希望你能继续探索如何将其应用于你自己的数据和兴趣领域。在本系列简短课程的第四门也是最后一门课程中，你将学习如何访问这些预先编写好的Python程序（称为包或库）。这将让你能够使用大量非常强大的函数，并且通常只需几行代码就能完成真正强大的事情。最后一门课程中的示例将会是最有趣的，我期待在那里见到你。😊