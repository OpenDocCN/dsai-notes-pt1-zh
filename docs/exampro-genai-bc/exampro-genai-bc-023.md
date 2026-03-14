# 23：后端API测试代码 🧪

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_1.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_3.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_5.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_7.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_9.png)

在本节课中，我们将学习如何为已实现的后端API编写和运行测试代码，以确保所有端点都能按预期工作。我们将使用Ruby的RSpec框架来编写可读性强的测试，并配置测试环境来隔离开发数据。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_11.png)

## 代码提交与环境准备

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_13.png)

上一节我们完成了后端API的实现，本节我们首先需要提交代码并准备测试环境。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_15.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_17.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_18.png)

在继续之前，我忘记提交代码了。现在需要完成这个步骤。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_20.png)

我将代码暂存并提交，提交信息为“implement back end code”。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_22.png)

同时，我生成了一个`.gitignore`文件来排除不必要的文件。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_24.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_26.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_28.png)

现在代码已提交，我们可以开始下一步。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_30.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_32.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_34.png)

## 测试策略选择

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_36.png)

我们的下一个任务是确保所有API端点都能正常工作，并返回预期的JSON响应或状态码（如200、404、500等）。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_38.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_40.png)

我们需要找到一种简单的方法来编写测试代码。虽然可以使用Go语言自带的测试框架，但Go是静态类型语言，通常不需要像动态语言那样多的测试代码。

另一种更开发者友好的方式是使用像Postman或VSCode扩展（如Thunder Client）这样的工具。但Postman现在是付费的，不易访问。我更倾向于使用Ruby的RSpec框架，因为它编写的测试代码非常清晰易读。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_42.png)

以下是使用RSpec测试API的一个示例结构，它清晰地描述了期望的响应：

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_44.png)

```ruby
describe ‘GET /groups’ do
  it ‘returns a 200 status code’ do
    # 测试逻辑
  end
end
```

这种可读性正是我们想要的。我们可以用Ruby为所有端点编写测试，即使后端是Go语言，这些测试也能通用。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_46.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_48.png)

## 编写与运行RSpec测试

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_50.png)

我让AI助手使用RSpec为所有API端点生成测试代码。它创建了一个`spec`目录，里面包含了针对`groups`、`words`、`study_sessions`等端点的测试文件。

测试代码生成后，我们需要运行它。首先，需要在系统上安装Ruby。我使用RVM（Ruby版本管理器）来安装最新版本的Ruby（3.4.1）和Bundler。

安装完成后，在项目根目录运行`bundle install`安装依赖，然后尝试运行`rspec`命令。

然而，一开始遇到了问题，比如找不到`spec_helper`文件，或者路径错误。通过调整`require`语句为`require_relative`，并确保`spec_helper.rb`文件正确加载了必要的配置和辅助模块，我们解决了这些问题。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_52.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_54.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_56.png)

## 配置测试数据库

运行测试时，一个关键问题是测试依赖于数据库中的特定数据。例如，测试期望存在ID为1的单词，但开发数据库中可能没有。

为了解决这个问题，我们配置了一个独立的测试数据库。我们创建了一个环境变量`DB_PATH`来控制Go后端连接哪个数据库文件（例如`words.test.db`）。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_58.png)

我们还编写了一个SQL脚本（`test_data.sql`）来为测试数据库插入保证存在的特定数据。同时，更新了RSpec的`spec_helper.rb`文件，确保在运行测试前设置正确的环境变量，从而让Go应用连接到测试数据库。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_60.png)

我们利用项目中已有的`magefile.go`中的命令（如`mage testdb`）来初始化和填充测试数据库，这比直接运行SQL脚本更集成化。

## 调试与修复测试

配置好测试数据库后，我们开始逐个运行测试套件，并修复遇到的问题。

1.  **Words端点**：最初因为缺少数据而失败。在确保测试数据库中有ID为1的数据后，测试通过。
2.  **Groups端点**：在数据就绪后也顺利通过。
3.  **Study Activities端点**：遇到了几个问题：
    *   **数据缺失**：测试数据库中没有插入学习活动数据。我们通过更新初始化脚本来解决。
    *   **时间格式**：Go服务返回的时间字段格式与RSpec期望的不匹配。我们更新了Go代码中的JSON标签，以正确格式化时间字段。
    *   **HTTP状态码**：某些POST请求期望返回`200`，但实际返回了`201`。我们根据API设计规范调整了测试的期望值。
4.  **Study Sessions端点**：遇到了与Study Activities类似的时间格式问题，以及查询参数与请求体处理的混淆。在修正了处理程序逻辑后得以解决。
5.  **Dashboard端点**：这是最复杂的端点，涉及多表查询。主要问题是测试期望的响应结构（例如`words_learned`字段位于根级别）与实际API返回的结构不匹配。我们根据实际的API实现修正了测试断言。
6.  **Reset端点**：该端点用于清除学习历史。它的测试依赖于调用reset后再检查dashboard的状态，因此需要确保dashboard端点本身工作正常。

在修复过程中，我们采取了一次只专注修复一个失败测试的策略，这比同时处理所有问题更有效。我们反复执行了“停止Go服务 -> 重建测试数据库 -> 启动Go服务 -> 运行特定测试”的循环。

## 最终测试状态与总结

经过一系列调试和修复，我们成功让大部分测试套件（Words, Groups, Study Activities, Study Sessions）全部通过。Dashboard和Reset端点的测试由于涉及复杂的数据状态变化，在批量运行时可能因数据不一致而偶发失败，但当单独运行时表现正确。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_62.png)

本节课中我们一起学习了：
1.  为Go后端API编写RSpec测试代码的策略。
2.  如何安装和配置Ruby、RVM及RSpec环境。
3.  创建和使用独立的测试数据库来保证测试数据的一致性。
4.  利用项目已有的`mage`命令来管理数据库初始化。
5.  逐步调试和修复测试中遇到的各种问题，包括数据准备、API响应格式和HTTP状态码。

虽然通过AI助手生成和修正测试代码提高了效率，但整个过程也表明，对于复杂项目，完全依赖AI一次性生成所有测试并让其自行修复是低效且容易出错的。更好的实践是：**分模块实现功能，随后立即为每个模块编写和运行测试，这样能拥有更多上下文和控制权，从而更早发现和解决问题**。

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_64.png)

![](img/ff2a5afc2665f02a7881fc99fdfb9f4c_66.png)

最终，我们获得了一个具备基本测试覆盖的后端代码库，为后续的前端开发或进一步的功能迭代打下了基础。然而，在将代码投入生产环境前，仍需进行更全面和彻底的手动测试与代码审查。