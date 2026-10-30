---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应先确认它能在浏览器中正常工作。你将连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，而不必亲自逐项操作，然后发布分支并创建 PR。

本练习将：

- 了解 Model Context Protocol（MCP）以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支，并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol（MCP）？

当智能体模式能够访问编辑器以外的资源时，它会更加强大。Copilot 通过 Model Context Protocol（MCP）实现这一点：这是智能体与外部工具和服务通信的一种标准方式。

![智能体模式内部工作原理示意图，展示其如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)让 AI 智能体能够与外部工具和服务通信。通过 MCP，AI 智能体可以实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理智能体与外部工具（例如现有 API 或 NPM 等本地工具包）的通信。每个 MCP 服务器都提供一组不同的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 智能体可以创建存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力。AI 智能体可以访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器提供不同工具和资源。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助社区发现并贡献生态系统中的项目。

> [!CAUTION]
> 出于安全考虑，请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应仔细审查其源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件配置了此 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板中预置了 `github` 条目，因此 Copilot 能在上一练习中读取议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 会通过 HTTP 连接，并在 Copilot 首次使用其工具时要求你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二台服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 启动它。`--headless` 标志会让 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面环境的 codespace 是必要的。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装了浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器：Copilot 首次需要某服务器提供的工具时，VS Code 会启动它，并要求你确认是否信任该服务器。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选的；如果跳过此步骤，VS Code 会在下一步 Copilot 首次需要服务器工具时启动它，并询问是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的真实浏览器。智能体可以打开页面、导航、应用筛选并读取结果，然后向你总结观察结果，无需你亲自操作应用或离开对话。这是快速确认功能符合预期的方式。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，智能体会根据结构化且带标签的元素（按钮、链接、列表项）进行推理，与辅助技术的工作方式相同。快速功能检查也因此兼顾了基础的无障碍检查。

服务器已连接且应用正在运行时，请 Copilot 测试刚实现的筛选功能：

```text
使用 Playwright MCP 服务器，在 http://localhost:4321 打开正在运行的应用并验证新的游戏筛选功能：

1. 打开游戏页面，并记录列出的游戏数量。
2. 应用类别筛选，并确认列表只显示该类别的游戏。
3. 清除筛选，然后应用发行商筛选，确认列表已更新为该发行商的游戏。
4. 同时应用类别和发行商筛选，确认结果同时符合两个条件。

报告每一步观察到的结果，并指出任何行为异常之处。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器并逐步操作，然后报告结果。请根据议题中的验收标准审查报告；如果发现问题，可继续提问，或要求它先修复代码再创建 PR。

> [!NOTE]
> 测试期间，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR）供团队审查了。首先发布 `filtering-vscode` 分支。

1. 打开 codespace 中的 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）等方式创建拉取请求。既然正在使用 GitHub Copilot，不妨让它代为创建 PR。它可以找到相关议题并将 PR 与议题关联。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   查找存储库中与按类别和发行商筛选有关的议题。为当前分支创建新的拉取请求，并将其关联到正确的议题。
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你通过 GitHub 身份验证，请按照提示操作。
5. 观察 Copilot 如何搜索议题、找到正确的议题并创建 PR。
6. 选择 Copilot 生成的链接审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol（MCP）以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 在发布前使用它通过浏览器手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

你已使用 MCP 服务器测试功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等聚焦任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
