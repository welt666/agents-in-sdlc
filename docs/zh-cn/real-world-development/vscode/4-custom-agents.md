---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]让你可以创建专门针对开发工作流中特定任务或领域的 AI 助手。在存储库的 `.github/agents` 文件夹中通过 Markdown 文件定义智能体，可以为 Copilot 提供聚焦的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编写成可复用的智能体，例如执行 [WCAG][wcag] 合规检查的无障碍智能体、遵循安全编码实践的安全智能体，或保持测试模式一致的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后面是定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的人员，技能是工具**。

自定义智能体拥有自己的上下文窗口，并用于协调技能（甚至其他智能体）来完成工作。在本练习中，无障碍自定义智能体会根据无障碍指南审查并更新网站；执行过程中，它可以调用诸如拉取请求工作流或运行和管理测试等技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方式。与 AI 的其他用法一样，应在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

自定义智能体可以帮助你：

- 了解如何定义自定义智能体。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，当前暗色主题的文字与背景颜色对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍访问至关重要，因此你希望尽快实现此功能。接下来将使用自定义智能体生成相关功能。

## 审查无障碍自定义智能体

项目已经提供了无障碍自定义智能体。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后为无障碍工作从 `main` 创建新分支（这样可以将筛选 PR 与本练习分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续章节，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，该会话会遵循相应的指令和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会说明具体要求。

1. 如果 Chat 视图的智能体下拉列表中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表（默认显示当前智能体 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    为网站添加高对比度模式。应有一个可由用户控制的高对比度切换开关，并使用浏览器本地存储，在页面重新加载后保留设置。
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改建议并编辑项目文件。每项编辑都会以内嵌差异的形式显示文件路径和更改内容，供你审查。
6. 随着编辑应用，**Files changed** 指示器会更新，显示智能体已修改的文件。

> [!NOTE]
> 这个过程可能需要几分钟。Copilot 会实际修改存储库文件，工作过程中可能会编辑 Astro 组件、CSS 和相关测试。

下一练习将审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域的专用 AI 助手。使用自定义智能体可以将团队专长和规范编写为可复用配置，引导 Copilot 更有效地完成特定工作。

本课中，你了解了：

- 如何定义自定义智能体。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，审查实时更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
