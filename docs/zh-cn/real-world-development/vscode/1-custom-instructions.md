---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保这些上下文可以访问。提供上下文的方式有很多，其中重要的一种是[说明文件][instruction-files]，它可以为代码生成提供指导。

本练习将：

- 探索项目上下文、编码指南和文档规范如何通过存储库自定义说明及按路径限定的说明文件提供给 Copilot。
- 在现有说明生效时，生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。
- 使用相同提示重新生成代码，观察新规范带来的影响。
- 将说明更新和筛选功能基础提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不完全遵循你设置的规范。Copilot 具有概率性；本练习的重点是观察更新说明后行为的整体变化趋势，而不是让输出逐字一致。

## 说明文件

### 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 代码中应使用 TSDoc 文档注释。
- 每个文件开头都应添加注释块，说明文件用途。

通过说明文件，你可以确保 Copilot 获得正确的信息，并按照这些实践执行任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以指定编码约定、库，以及希望添加的注释类型。说明既可以适用于整个存储库，也可以针对特定类型的文件或任务。

说明文件有两种：

- `.github/copilot-instructions.md`：每次请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定语言（如 TypeScript 或 Astro）、文件类型或任务提供指南。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时会使用存储库级说明，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本练习范围，但入门项目中的示例展示了常见做法：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目说明、结构和全局编码规范。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持说明清晰，并提供代码应如何编写或不应如何编写的示例。

创建说明文件或使用 AI 都没有唯一正确的方法。通过实践找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的说明文件。浏览项目时，你会发现其中包含多种任务的说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。各产品界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本研讨会会在相关课程中说明具体入口。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览项目中的自定义说明文件

花些时间阅读存储库自带的说明文件：一个核心 `copilot-instructions.md` 文件，以及一组适用于各种任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指南，这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有 Astro 文件、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段设置了相对于存储库根目录的 glob，确定说明适用于哪些文件。这里会匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看此项目的单元测试创建说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。注意它指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以拆分并复用较大的说明集，也可以为 Copilot 提供参考示例。（其中的路径相对于说明文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求添加代码文档。接下来会添加 TSDoc 注释和文件头注释规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建分支。

1. 返回上一练习使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会说明具体要求。

为了观察自定义说明的影响，先在当前说明文件生效时发送提示。稍后更新说明后，再次发送相同提示并比较结果。

1. 关闭之前练习遗留的编辑器选项卡，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，让 Copilot 可以应用文件更改。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级头注释。

> [!CAUTION]
> Copilot 具有概率性，因此它也可能在没有明确要求时添加文档注释。如果出现这种情况也没关系，本练习的重点仍是观察更新说明后行为是否更一致。

## 添加新的存储库规范

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项存储库编码规范，改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意其中记录了项目编码规范，但尚未要求添加代码文档，这就是生成的辅助函数没有文档注释的原因。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - 每个导出的函数都应包含 TSDoc 注释，说明其用途、参数和返回值。
   - 在导入语句或任何代码之前，添加注释块说明文件用途。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件可以放在存储库级 `.github/copilot-instructions.md` 中，提供全局指导；也可以放在 `*.instructions.md` 文件中，针对特定语言、文件类型或任务。刚添加的文档注释规则属于项目级规范，因此适合放在存储库级说明文件中。

## 重新发送提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除对话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的名称和 ID。应用文件更改。
   ```

4. 注意生成的文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 注意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商的 ID 和名称列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为发行商对象数组。
    */
   ```

你刚刚引导 Copilot 遵循新的项目规范，并将其应用到真实代码中；下一练习会以此为基础继续开发。

## 提交说明并推送分支

说明文件和其他项目资源一样，应通过源代码管理进行维护。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更改（例如测试），也一并添加。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求，并关联待办事项中的文档规范议题。你可以手动操作，不过也可以让 Copilot 使用已连接到项目的 GitHub 工具查找议题、创建关闭该议题的 PR，并合并它。

> [!NOTE]
> 这是首次让 Copilot 代表你在 GitHub 上操作。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub，请按照提示操作。后续课程会介绍这种连接（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 请 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，说明其中既有说明更新，也有新的发行商辅助函数基础：

   ```plaintext
   查找与更新说明文件有关的议题。从当前分支创建 PR，说明该 PR 会关闭此议题，并指出我们还添加了后续筛选工作所需的发行商辅助函数基础。
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果 Copilot 提示允许它代表你执行 GitHub 操作，请审查操作并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 将其合并到 `main`，并将本地分支切回 `main`：

   ```plaintext
   将 PR 合并到 main。然后在本地切回 main，并拉取最新代码以保持同步。
   ```

7. 如果 Copilot 提示允许它执行 GitHub 操作和 shell 命令，请审查操作并酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你探索了 Copilot 如何从说明文件中获取上下文，然后使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，并观察现有说明下 Copilot 的输出。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。
- 再次发送相同提示，观察生成的代码如何遵循新规范。
- 将说明更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，在[智能体模式][next-lesson]中运用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
