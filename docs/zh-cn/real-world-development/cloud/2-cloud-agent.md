---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务。这可能是尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发人员专注于更重要的工作。

本课将使用 Copilot 云智能体探索：

- 如何自定义代码生成环境。
- 如何确保操作安全。
- 明确限定议题范围的重要性。
- 如何将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇开发网站的承包商没有留下完善的文档，事实上，文档完全缺失。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页显示相关游戏（即同类别的其他游戏），让用户可以继续浏览。他们暂时不需要完善的实现，只需要可用于 UX 验收测试的版本。这项工作目前不是阻塞项，另有优先级更高的议题。

这两项都是容易被搁置的任务，很适合分配给 Copilot 云智能体异步处理。开发人员可以转而处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。就像与开发人员协作一样，你可以通过多种方式分配工作，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始工作。会话进行期间或完成后，你都可以查看 Copilot 会话。解决方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 明确限定说明和任务范围的重要性

GitHub Copilot 并非魔法——你仍然需要思考。看似简单的操作也可能包含复杂细节，因此[应谨慎限定交给云智能体的任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、试验并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果你已完成自定义说明练习，就见过 `.github/copilot-instructions.md` 的作用；如果还没完成，现在可以快速查看。分配任务前，只读浏览存储库中已有的说明文件，以便稍后观察它们的影响。

在 GitHub 网页界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，重点关注 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将说明限定为适用于 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` 议题分配给云智能体时，观察生成的 PR 中的 TSDoc 注释、TypeScript 约定和文件头注释。这些内容来自上述说明文件；后续练习审查 PR 时还会再次提到。

## 设置 Copilot 云智能体开发环境

无论由谁编写代码，通常都需要特定环境和设置脚本，确保项目处于良好状态。将任务分配给 Copilot 时也是如此，它会像软件工程师一样执行工作。

云智能体使用 [GitHub Actions][github-actions] 环境。你可以通过创建[专用设置工作流][setup-workflow]自定义环境，将其配置在 `.github/workflows/copilot-setup-steps.yml` 中，并在智能体开始工作前运行，以便安装所需开发工具和依赖项。实验已预先配置此工作流，以便顺利完成课程。它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单个 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅授予步骤所需的最低权限。Copilot 会获得用于执行其操作的独立令牌。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已采用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 设置数据库：迁移并填充本地 SQLite 数据库。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几个关键点：

- 它仅包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 其中的 `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这便于测试工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档很重要，但多数项目的文档要么过时，要么完全缺失。这类技术债务常被忽略，会降低工作效率，也让维护代码库或帮助新开发人员加入团队变得更困难。Copilot 擅长生成文档，因此这是分配给云智能体的理想议题。它可以在后台生成所需文档，后续练习中再回来审查。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    我们组织要求在有助于理解行为的地方为函数和方法添加 TSDoc 文档注释。遗憾的是，近期更新没有遵循此规范。我们需要更新现有代码，在能够阐明行为的地方补充文档注释。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚生成的拉取请求（PR），标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定处理任务的最佳方案。

13. 审查列表及 Copilot 准备完成的任务。
14. 向下滚动拉取请求时间线，应能看到 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码并了解状态、暂停思考并决定合适方案，以及编写代码。

这可能需要几分钟。云智能体的主要目标之一是异步执行任务，让我们腾出时间处理其他工作。现在也利用这一功能，再给 Copilot 云智能体分配一项任务，然后转而为应用编写其他功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务时，它可以处理另一组。为设计团队添加相关游戏部分可能不会花很长时间，但这段时间仍可用于其他任务。现在将该任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    我们希望在每款游戏的详情页显示相关游戏，帮助用户发现更多游戏。设计团队希望探索 UX 并进行一些验收测试。要求如下：

   - 在 `src/lib/` 中添加数据访问辅助函数，返回与指定游戏类别相同的其他游戏，并排除该游戏自身
   - 在游戏详情页显示使用此辅助函数的“相关游戏”部分
   - 处理某款游戏没有相关游戏的情况
   - 为新辅助函数创建单元测试
   - 创建 PR 前确保所有测试通过
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！云智能体的工作方式类似软件工程师，无需主动持续监控，完成后再进行审查即可。现在转而创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体异步处理。Copilot 可用于解决技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

本课探索了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确限定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以转而创建并使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
