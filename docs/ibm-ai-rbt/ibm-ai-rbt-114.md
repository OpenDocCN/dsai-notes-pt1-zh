# 114：将您的聊天机器人部署到Slack 🚀

在本节课中，我们将学习如何将已构建的Watson Assistant聊天机器人集成并部署到Slack工作区中。通过本次实践，您将掌握连接两个平台所需的关键配置步骤。

![](img/513a180df0c8f09dcde106c2738295ee_1.png)

---

## 概述：什么是Slack？🤔

![](img/513a180df0c8f09dcde106c2738295ee_3.png)

Slack是一款团队协作中常用的沟通工具。

![](img/513a180df0c8f09dcde106c2738295ee_5.png)

![](img/513a180df0c8f09dcde106c2738295ee_1.png)

通过在Slack上部署聊天机器人，您可以改善团队的工作流程，或与朋友和家人一起设置重要事件的提醒。

![](img/513a180df0c8f09dcde106c2738295ee_3.png)

本次实验的目标是将我们的聊天机器人与Slack进行集成。

![](img/513a180df0c8f09dcde106c2738295ee_5.png)

---

## 第一步：创建Slack工作区与应用 🏗️

![](img/513a180df0c8f09dcde106c2738295ee_7.png)

我们将从创建一个Slack应用开始。首先需要创建一个Slack工作区。访问Slack.com，输入您的电子邮件地址以开始创建。

![](img/513a180df0c8f09dcde106c2738295ee_8.png)

![](img/513a180df0c8f09dcde106c2738295ee_9.png)

为您的新工作区选择一个名称，并为项目和团队名称进行同样的操作。创建工作区后，我们将继续创建一个Slack应用，使其能够与我们的Watson Assistant聊天机器人通信。

![](img/513a180df0c8f09dcde106c2738295ee_10.png)

因此，我们的第二步是登录Slack工作区，然后访问Slack网站的“应用”部分。

![](img/513a180df0c8f09dcde106c2738295ee_11.png)

![](img/513a180df0c8f09dcde106c2738295ee_7.png)
![](img/513a180df0c8f09dcde106c2738295ee_8.png)
![](img/513a180df0c8f09dcde106c2738295ee_9.png)
![](img/513a180df0c8f09dcde106c2738295ee_10.png)
![](img/513a180df0c8f09dcde106c2738295ee_11.png)

![](img/513a180df0c8f09dcde106c2738295ee_13.png)

到达后，我们将创建一个新应用。

![](img/513a180df0c8f09dcde106c2738295ee_13.png)

![](img/513a180df0c8f09dcde106c2738295ee_15.png)

您需要为应用命名，并指定您希望部署此应用的工作区，然后创建应用。

![](img/513a180df0c8f09dcde106c2738295ee_15.png)

![](img/513a180df0c8f09dcde106c2738295ee_17.png)

---

![](img/513a180df0c8f09dcde106c2738295ee_18.png)

## 第二步：创建Slack机器人用户 🤖

现在我们有了一个Slack应用，我们需要在Slack网站的Slack API相关部分创建一个Slack机器人用户。在这里，我们将添加一个新的机器人用户。

![](img/513a180df0c8f09dcde106c2738295ee_20.png)

![](img/513a180df0c8f09dcde106c2738295ee_17.png)
![](img/513a180df0c8f09dcde106c2738295ee_18.png)

我们将确保机器人始终在线并准备好回答用户的问题，然后创建该用户。

![](img/513a180df0c8f09dcde106c2738295ee_22.png)

![](img/513a180df0c8f09dcde106c2738295ee_20.png)

![](img/513a180df0c8f09dcde106c2738295ee_23.png)

接下来，我们需要记录下我们的客户端ID、客户端密钥和验证令牌。在Watson Assistant内部集成Slack时，我们将需要这些凭证。

![](img/513a180df0c8f09dcde106c2738295ee_22.png)
![](img/513a180df0c8f09dcde106c2738295ee_23.png)

![](img/513a180df0c8f09dcde106c2738295ee_25.png)

![](img/513a180df0c8f09dcde106c2738295ee_26.png)

现在，我们需要在我们的工作区内安装Slack应用，并且还需要对其进行授权。

![](img/513a180df0c8f09dcde106c2738295ee_25.png)
![](img/513a180df0c8f09dcde106c2738295ee_26.png)

![](img/513a180df0c8f09dcde106c2738295ee_28.png)

保存将提供的OAuth访问令牌和机器人用户OAuth访问令牌。

![](img/513a180df0c8f09dcde106c2738295ee_28.png)

---

![](img/513a180df0c8f09dcde106c2738295ee_30.png)

## 第三步：在Watson Assistant中添加Slack集成 🔗

上一节我们准备好了Slack应用的所有凭证，本节中我们来看看如何在Watson Assistant中完成集成配置。

![](img/513a180df0c8f09dcde106c2738295ee_32.png)

登录您的IBM Cloud账户并启动您的Watson Assistant实例。选择我们的“Student Advisor”聊天机器人。

![](img/513a180df0c8f09dcde106c2738295ee_30.png)

![](img/513a180df0c8f09dcde106c2738295ee_34.png)

在“集成”部分，我们将添加一个集成。

![](img/513a180df0c8f09dcde106c2738295ee_32.png)

![](img/513a180df0c8f09dcde106c2738295ee_36.png)

在这里，我们将像在上一个实验中所做的那样选择一个服务，但这次我们当然选择Slack。

![](img/513a180df0c8f09dcde106c2738295ee_37.png)

![](img/513a180df0c8f09dcde106c2738295ee_34.png)

在这里，我们将粘贴我们的一些凭证，即验证令牌、OAuth访问令牌和机器人用户OAuth访问令牌。这允许Watson Assistant连接到Slack。

![](img/513a180df0c8f09dcde106c2738295ee_36.png)
![](img/513a180df0c8f09dcde106c2738295ee_37.png)

![](img/513a180df0c8f09dcde106c2738295ee_39.png)

我们还将能够为我们的Slackbot生成一个请求URL以供订阅。我们需要记下它，因为我们需要它回到Slack中配置我们的Slackbot。我们需要启用传入Webhook。

![](img/513a180df0c8f09dcde106c2738295ee_40.png)

![](img/513a180df0c8f09dcde106c2738295ee_41.png)

![](img/513a180df0c8f09dcde106c2738295ee_39.png)
![](img/513a180df0c8f09dcde106c2738295ee_40.png)
![](img/513a180df0c8f09dcde106c2738295ee_41.png)

---

## 第四步：启用事件并连接URL ⚙️

接下来，我们启用事件并将Slack连接到从Watson Assistant收到的请求URL。每当Slack中发生选定的事件时，对此URL的HTTP调用将通知Watson Assistant，以便它可以做出适当的响应。

以下是需要启用的事件列表：
*   **`message.im`**：这将启用直接消息来触发聊天机器人的响应。
*   **`app_mention`**：这将启用提及（@机器人）来触发聊天机器人的响应。

![](img/513a180df0c8f09dcde106c2738295ee_43.png)
![](img/513a180df0c8f09dcde106c2738295ee_44.png)
![](img/513a180df0c8f09dcde106c2738295ee_45.png)
![](img/513a180df0c8f09dcde106c2738295ee_46.png)
![](img/513a180df0c8f09dcde106c2738295ee_47.png)

![](img/513a180df0c8f09dcde106c2738295ee_43.png)

![](img/513a180df0c8f09dcde106c2738295ee_44.png)

---

![](img/513a180df0c8f09dcde106c2738295ee_45.png)

## 总结与下一步 🎯

![](img/513a180df0c8f09dcde106c2738295ee_46.png)

![](img/513a180df0c8f09dcde106c2738295ee_47.png)

本节课中，我们一起学习了将Watson Assistant聊天机器人部署到Slack的完整流程。我们完成了从创建Slack工作区和应用、配置机器人用户、获取关键凭证，到在Watson Assistant中设置Slack集成并配置事件订阅的所有关键步骤。

现在您已经了解了所有步骤，接下来就轮到您在下一个实验中进行实际操作了。祝您好运！

![](img/513a180df0c8f09dcde106c2738295ee_49.png)

![](img/513a180df0c8f09dcde106c2738295ee_49.png)