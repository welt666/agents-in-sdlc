---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的存储库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款用于智能体驱动开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此存储库、分支和 CI 管道都可以开箱即用。它专为同时指挥多个智能体的工作流而设计：每个智能体都在独立工作区中运行，让你可以并行处理工作并自动执行重复任务。安装好 Node.js 并准备好项目副本后，下一步是安装应用并连接该存储库。

本课将：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 存储库将项目添加到应用。
- 熟悉工作区，包括模板创建的待办事项。
- 通过快速聊天了解应用本身。

## 场景

团队正采用 AI 智能体处理不断增加的待办事项。Copilot app 提供统一的工作位置，可领取议题、运行智能体、审查更改并合并拉取请求。本课将帮助你完成安装和连接，并熟悉如何开始讨论项目。

> [!NOTE]
> 必须拥有符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任一付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须保持 **GitHub Copilot app** 策略启用。

## 安装并配置 GitHub Copilot app

要使用 GitHub Copilot app，第一步自然是安装它。应用支持 Windows、macOS 和 Linux。现在安装应用、完成身份验证并添加 Tailspin Toys 存储库。

1. 在浏览器中打开 [GitHub Copilot app 产品页面][download-app]。
2. 下载适用于当前平台的应用，并按照产品页面上的说明进行安装。
3. 安装完成后打开应用。
4. 选择 **Sign in to GitHub**，并按照提示完成身份验证。如果使用启用了数据驻留的 GitHub Enterprise Cloud，请选择 **Use GitHub Enterprise**，并在提示时输入 `*.ghe.com` 主机名。GitHub Enterprise Server 不受支持。
5. 如果引导流程提供存储库选择，请选择通过模板创建的 Tailspin Toys 存储库。也可以跳过存储库选择，在完成引导后再添加项目。
6. 按提示选择主题，然后完成引导。
7. 在侧边栏 **Sessions** 下查找存储库。如果已经列出，无需重复添加。
8. 如果尚未连接存储库，选择 **Sessions** 旁的 **+** 创建新会话，然后选择 **Add GitHub repository**。输入先决条件课程中记录的所有者和存储库名称。
9. 确认存储库显示在侧边栏中。在此处使用的布局中，`tailspin-toys` 等存储库文件夹会显示在 **Sessions** 下，每个存储库下方再列出各个会话。

## 熟悉工作区

连接项目后，花些时间熟悉工作区。应用在侧边栏中将功能组织为几个区域：

- **New**：开始与 Copilot 的新聊天会话。
- **Pull requests**：通过应用与 GitHub 的原生集成显示拉取请求。在这里可以浏览和筛选拉取请求、查看 CI 状态并审查 PR，无需离开应用。
- **Issues**：以同样的方式显示议题。在这里可以浏览和筛选议题，并从议题启动会话。
- **Automations**：按计划或按需运行的已保存智能体任务。适合管理待办事项、定期项目维护或其他重复工作。总结部分会将其作为后续探索方向，而非额外的研讨会练习。
- **Customize**：通过 MCP 服务器、插件、技能和其他组件为 Copilot app 添加功能。你将使用它配置 Playwright MCP。
- **Chats**：用于提问和集思广益的轻量对话，无需单独创建分支或工作区。开始第一个聊天后，此区域才会显示。如果未显示，请选择 **Sessions** 旁的 **+** 开始第一次聊天。
- **Sessions**：存放存储库文件夹及其智能体会话。选择 `tailspin-toys` 等存储库名称可打开存储库视图；选择其下的会话可返回该对话。每个会话都在独立工作区中运行，因此可以同时运行多个会话而不会发生更改冲突。添加星级评分时将启动第一个会话。

随着研讨会继续进行，你会逐步了解工作区。

> [!TIP]
> 有疑问就问 Copilot！如果不确定如何操作，或某件事是否可行，可以向 Copilot 提问，让它提供指导。

### 查找模板创建的待办事项

几乎每个项目都有待办事项，Tailspin Toys 也不例外。现在查看通过模板创建项目时生成的待办事项。

1. 在侧边栏 **Sessions** 下，选择 Tailspin Toys 存储库文件夹，而不是其下的某个会话。然后在存储库顶部导航中选择 **Issues**。
2. 按标题查找以下议题，不要假设它们的编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择议题阅读详情。每个议题也可以作为智能体会话的启动点。完成一项快速的首次更改后，你将从筛选功能议题启动会话。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，仅显示已添加到 Copilot app 的存储库中的项目。若要查看其他存储库中的工作项，请将它们添加到应用。

## 尝试快速聊天

熟悉应用的一种好方法是用它来了解*应用本身*。**快速聊天**正适合此用途：无需创建分支或工作树即可提问或集思广益，非常适合无需启动会话的一次性问题。

1. 在侧边栏中选择 **Chats** 旁的 **+**，开始新聊天。如果尚未显示 **Chats**，选择 **Sessions** 旁的 **+** 开始第一次聊天；之后 **Chats** 会出现。
2. 询问应用自身的会话工作方式：

   ```plaintext
   GitHub Copilot app 如何使用工作树？
   ```

3. 在对话视图中阅读回复。每个会话都在独立的 Git 工作树中运行，因此可以并行运行多个智能体，而不会造成更改冲突。你可以随时继续对话或开始新聊天。

## 总结和后续步骤

恭喜！你已安装 GitHub Copilot app、连接项目并探索工作区。你学习了如何：

- 安装应用并登录 GitHub。
- 从 GitHub 存储库添加项目。
- 熟悉工作区，并在 **Issues** 中找到模板创建的待办事项。
- 使用快速聊天提出一次性问题。

接下来，[启动第一个智能体会话][next-lesson]，并用它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
