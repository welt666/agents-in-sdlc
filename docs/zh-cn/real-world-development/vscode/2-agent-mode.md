---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行多项操作，例如运行测试。开发人员通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件并再次修改……类似工作还有很多。

这正是 Copilot 智能体模式发挥作用的地方。

Copilot 智能体模式专为在 IDE 中更自主地工作而设计。它的行为类似开发人员：先探索现有项目结构，执行必要的更新，运行测试等任务，并自动修复发现的问题。现在来看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于不同的体验。智能体模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍：

- Copilot 智能体模式如何探索项目、识别相关文件并协调修改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 合并到代码库前，如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望用户能够按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

进行更改前，先探索 Tailspin Toys 网站，了解它目前的功能。

该网站是一个采用开发者主题的桌游众筹平台，可列出游戏并显示其详情。它是一个单独的 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过范围较小、使用标准 DOM API 的 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应已切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 为筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下 URL 的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 codespace 的私有隧道，网站服务器正在其中运行。

### 浏览网站

网站运行后，可以探索以下主要功能：

- **主页**：显示桌游列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、描述、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会说明具体要求。

网站的初始实现已可正常使用，但我们希望添加新功能。先查看待办事项。从模板设置项目时，系统已自动创建一组议题；请 GitHub Copilot 列出这些事项。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此将其列入说明，希望代码建议尽量保持一致。不过，LLM 具有概率性，你可能会收到与示例不同的建议。这完全正常，也在预期之中。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问 GitHub 存储库中的待办事项：

   ```plaintext
   请显示我 GitHub 存储库中的待办事项，并根据对用户最有用的程度帮助我确定优先级。
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 审查生成的议题列表。

注意 Copilot 如何根据它认为对用户最有用的程度为事项排序。

## 审查说明文件

开始让智能体生成代码前，先查看将用于提供上下文的说明文件。本练习将使用 [用户界面（UI）说明](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中记录了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向各文件类型说明的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径相对于说明文件所在位置，因此可以复用说明、将复杂内容拆分为较小部分，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要三项独立的应用更新：

- 在数据层（`src/lib/`）中添加或完善其余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前必须运行并通过测试。Copilot 智能体模式可以为你完成这些任务。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   请根据待办事项中相关 GitHub 议题的要求，更新网站以支持按发行商和类别筛选。上一练习已经添加发行商辅助函数，因此请保留并酌情完善该工作，同时完成其余数据层、UI 和测试。结束前确保所有测试均通过。服务器已经在运行，无需再次启动。
   ```

6. 观察 Copilot 如何先探索项目并找到相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库规模，这可能需要几分钟。过程中编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示相应按钮，因此无需在工作进行时立即选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 工作过程中可能会暂停，甚至出现测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直至任务完成且未发现错误。

   ![已完成的 Copilot 智能体模式聊天会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行网站的浏览器，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了新功能。你了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调修改。
- 合并到代码库前，如何审查 Copilot 智能体模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索：实现分页

随着游戏列表增加，网站需要支持分页。运用本练习学到的技能，请 Copilot 更新网站以实现分页。考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 决定分页实现方式：允许用户选择每页条目数，还是使用固定值。
- 编写提示时，为 Copilot 提供实现所需的指导。
- 可能需要与 GitHub Copilot 迭代，要求更改并补充上下文。这是正常的协作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
