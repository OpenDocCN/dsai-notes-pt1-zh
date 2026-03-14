# 20：使用AI编写后端API与前后端技术规范 📝

![](img/b1e2bb7066de98c5786115c0dc7edc75_1.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_2.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_4.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_5.png)

在本节课中，我们将学习如何从零开始，利用AI辅助工具（如Windsurf）来编写一个完整的后端API技术规范。我们将重点关注如何清晰地定义业务目标、技术需求、数据库模式以及API端点，为后续的代码生成打下坚实基础。

![](img/b1e2bb7066de98c5786115c0dc7edc75_7.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_9.png)

上一节我们介绍了生成式AI的基本概念，本节中我们来看看如何将其应用于实际的软件开发流程中。

![](img/b1e2bb7066de98c5786115c0dc7edc75_11.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_12.png)

## 概述：从零开始构建后端技术规范

本教程的目标是重建一个语言学习门户的后端。我们将使用Go语言和Gin框架，数据库采用SQLite。整个过程将完全从零开始，通过编写详细的技术规范来指导AI生成代码。

![](img/b1e2bb7066de98c5786115c0dc7edc75_14.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_16.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_18.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_20.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_22.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_24.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_26.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_28.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_30.png)

## 第一步：定义业务目标与技术需求

![](img/b1e2bb7066de98c5786115c0dc7edc75_32.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_33.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_35.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_37.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_39.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_40.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_42.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_44.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_46.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_48.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_50.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_52.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_54.png)

首先，我们需要明确项目的核心目标和技术栈选择。

![](img/b1e2bb7066de98c5786115c0dc7edc75_56.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_57.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_59.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_61.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_62.png)

**业务目标**：一所语言学习学校希望构建一个学习门户原型。该门户将充当以下三个角色：
1.  一个记录待学习词汇的“库存”。
2.  一个记录学习过程、提供纠错功能的“学习记录存储”。
3.  一个启动不同学习应用的“统一启动平台”。

![](img/b1e2bb7066de98c5786115c0dc7edc75_64.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_65.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_66.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_68.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_70.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_72.png)

**技术需求**：
*   **后端语言**：使用 **Go** 语言构建。
*   **部署方式**：使用 **Docker** 部署（可选，本教程暂不涉及）。
*   **数据库**：使用 **SQLite**。
*   **API框架**：使用 **Gin** 框架。
*   **数据格式**：API始终返回 **JSON** 格式数据。
*   **用户管理**：暂不实现身份验证和授权，所有操作视为单一用户。

![](img/b1e2bb7066de98c5786115c0dc7edc75_74.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_76.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_78.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_80.png)

## 第二步：设计数据库模式

清晰的数据库模式是API设计的基础。以下是本系统所需的表结构：

![](img/b1e2bb7066de98c5786115c0dc7edc75_82.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_84.png)

以下是核心数据表及其字段描述：

![](img/b1e2bb7066de98c5786115c0dc7edc75_86.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_88.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_90.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_92.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_93.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_95.png)

*   **words（单词表）**
    *   `id`：整数，主键。
    *   `japanese`：字符串，日语单词。
    *   `romaji`：字符串，罗马字拼写。
    *   `english`：字符串，英语释义。
    *   `parts`：JSON，词性信息。
*   **word_groups（单词-分组关联表）**
    *   `id`：整数，主键。
    *   `word_id`：整数，外键关联 `words.id`。
    *   `group_id`：整数，外键关联 `groups.id`。
    *   *（这是一个多对多关系的连接表）*
*   **groups（分组表）**
    *   `id`：整数，主键。
    *   `name`：字符串，分组名称。
*   **study_sessions（学习会话表）**
    *   `id`：整数，主键。
    *   `group_id`：整数，外键关联 `groups.id`。
    *   `created_at`：时间戳，创建时间。
*   **study_activities（学习活动表）**
    *   `id`：整数，主键。
    *   `name`：字符串，活动名称。
    *   `description`：字符串，活动描述。
    *   `thumbnail_url`：字符串，缩略图URL。
    *   `launch_url`：字符串，启动URL。
*   **word_review_items（单词复习记录表）**
    *   `id`：整数，主键。
    *   `study_session_id`：整数，外键关联 `study_sessions.id`。
    *   `word_id`：整数，外键关联 `words.id`。
    *   `correct`：布尔值，回答是否正确。
    *   `created_at`：时间戳，创建时间。

![](img/b1e2bb7066de98c5786115c0dc7edc75_97.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_99.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_101.png)

## 第三步：分析前端页面以确定API端点

![](img/b1e2bb7066de98c5786115c0dc7edc75_103.png)

为了设计出符合实际需求的API，我们需要分析前端每个页面的功能和所需数据。通过截图和描述每个页面，我们可以反向推导出后端需要提供哪些API端点。

![](img/b1e2bb7066de98c5786115c0dc7edc75_105.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_107.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_109.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_111.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_113.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_115.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_117.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_119.png)

以下是各页面的分析：

![](img/b1e2bb7066de98c5786115c0dc7edc75_121.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_123.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_125.png)

*   **仪表盘 (`/dashboard`)**
    *   **目的**：展示学习概览。
    *   **组件**：最近学习会话、学习进度、快速统计（成功率、会话总数、活跃分组数、学习连续天数）、”开始学习“按钮。
    *   **所需API端点**：`GET /api/dashboard` (可能需要聚合多个数据)。
*   **学习活动列表 (`/study-activities`)**
    *   **目的**：展示所有可用的学习活动。
    *   **组件**：学习活动卡片（包含缩略图、名称、启动按钮、查看详情按钮）。
    *   **所需API端点**：`GET /api/study-activities`。
*   **学习活动详情 (`/study-activities/:id`)**
    *   **目的**：展示特定学习活动的详情和历史会话。
    *   **组件**：活动名称、描述、缩略图、启动按钮、历史会话列表（ID、活动名、分组名、开始时间、结束时间、复习项目数量）。
    *   **所需API端点**：`GET /api/study-activities/:id`, `GET /api/study-sessions?activity_id=:id`。
*   **学习活动启动页 (`/study-activities/:id/launch`)**
    *   **目的**：启动一个学习活动。
    *   **组件**：启动表单（选择分组的下拉菜单）、启动按钮。
    *   **所需API端点**：`POST /api/study-sessions` (请求体需包含 `group_id` 和 `study_activity_id`)。
*   **单词列表 (`/words`)**
    *   **目的**：展示数据库中的所有单词。
    *   **组件**：分页单词列表（列：日语、罗马字、英语、正确次数、错误次数）。
    *   **所需API端点**：`GET /api/words` (支持分页)。
*   **单词详情 (`/words/:id`)**
    *   **目的**：展示特定单词的详细信息。
    *   **组件**：日语、罗马字、英语、学习统计（正确/错误次数）、所属分组标签。
    *   **所需API端点**：`GET /api/words/:id`。
*   **分组列表 (`/word-groups`)**
    *   **目的**：展示所有单词分组。
    *   **组件**：分页分组列表（列：分组名、单词数量）。
    *   **所需API端点**：`GET /api/word-groups` (支持分页)。
*   **分组详情 (`/word-groups/:id`)**
    *   **目的**：展示特定分组的详情。
    *   **组件**：分组名、分组统计（总单词数）、该分组下的单词列表、关联的学习会话列表。
    *   **所需API端点**：`GET /api/word-groups/:id`, `GET /api/words?group_id=:id`, `GET /api/study-sessions?group_id=:id`。
*   **学习会话列表 (`/study-sessions`)**
    *   **目的**：展示所有学习会话记录。
    *   **组件**：分页会话列表（列：ID、活动名、分组名、开始时间、结束时间、复习项目数量）。
    *   **所需API端点**：`GET /api/study-sessions` (支持分页和过滤)。
*   **学习会话详情 (`/study-sessions/:id`)**
    *   **目的**：展示特定学习会话的详情。
    *   **组件**：会话详情（活动名、分组名、开始时间、结束时间、复习项目数量）、本次会话复习的单词列表。
    *   **所需API端点**：`GET /api/study-sessions/:id`, `GET /api/word-review-items?session_id=:id`。
*   **设置页面 (`/settings`)**
    *   **目的**：进行系统配置。
    *   **组件**：主题选择（亮色/暗色/系统）、重置历史按钮、重载种子数据按钮。
    *   **所需API端点**：`DELETE /api/study-sessions` (重置历史), `POST /api/seed` (重载数据)。

![](img/b1e2bb7066de98c5786115c0dc7edc75_127.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_129.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_130.png)

**额外的核心API**：
*   `POST /api/words/:id/review`：提交一个单词的复习结果。请求体需包含 `study_session_id` 和 `correct` 参数。

![](img/b1e2bb7066de98c5786115c0dc7edc75_132.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_134.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_136.png)

## 第四步：整理与提交技术规范

将上述所有内容整理到两个文档中：`backend-technical-spec.md` (包含业务目标、技术需求、数据库模式、API端点列表) 和 `frontend-technical-spec.md` (包含每个页面的详细分析)。使用Git工具（如SmartGit/GitX）将这两个文档以及相关的页面截图提交到版本库中。

![](img/b1e2bb7066de98c5786115c0dc7edc75_138.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_140.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_142.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_143.png)

至此，我们已经拥有了一份详尽的技术规范，可以用于指导AI生成初始代码，或在团队中作为开发蓝图。

![](img/b1e2bb7066de98c5786115c0dc7edc75_145.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_146.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_148.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_150.png)

## 总结

![](img/b1e2bb7066de98c5786115c0dc7edc75_152.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_154.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_155.png)

![](img/b1e2bb7066de98c5786115c0dc7edc75_157.png)

本节课中我们一起学习了如何系统化地编写一个后端项目的技术规范。我们从定义清晰的**业务目标**和**技术栈**出发，设计了详细的**数据库模式**，并通过分析**前端页面**的功能反向推导出所有必需的**API端点**。这个过程强调了文档先行的重要性，一份好的技术规范是高效利用AI辅助编程或进行团队协作的基础。在下一节中，我们将利用这份规范，使用AI工具来生成可运行的后端Go代码。