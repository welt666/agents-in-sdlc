---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保这些上下文可以访问。[说明文件][instruction-files]可以提供指导，让 Copilot 不仅理解你想让它做什么，也理解你希望它如何完成。

本练习将：

- 探索项目上下文、编码指南和文档规范如何通过存储库自定义说明及按路径限定的说明文件提供给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。

> [!NOTE]
> 与 VS Code 和 CLI 练习路径不同，本练习不会运行“更新前/更新后”的提示比较。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示影响。稍后审查云智能体创建的拉取请求时，你会看到说明文件的作用。

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

## 浏览此项目中的自定义说明文件

花些时间阅读存储库自带的说明文件：一个核心 `copilot-instructions.md` 文件，以及一组适用于各种任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指南，这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有 Astro 文件、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段设置了相对于存储库根目录的 glob，确定说明适用于哪些文件。这里会匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看此项目的单元测试创建说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。注意它指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可以拆分并复用较大的说明集，也可以为 Copilot 提供参考示例。（其中的路径相对于说明文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求添加代码文档。接下来会添加 TSDoc 注释和文件头注释规则。

## 添加新的存储库规范

接下来要进行的唯一编辑，是添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级头注释。云智能体稍后处理本练习路径中分配的议题时会读取这条规范。

编辑之前，先创建工作分支（云智能体会读取议题目标分支中的说明，但你仍应整洁地提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一条存储库编码规范，改善代码建议。

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

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。在下一练习中将议题分配给 Copilot 时，它会从 `main` 创建分支，因此说明更新必须先合并到 `main`，云智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后，云智能体处理下一练习分配的议题时就会读取这些说明。

> [!TIP]
> 如果希望在本次研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处采用分支，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你探索了 Copilot 如何从说明文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级规范。后续练习中审查云智能体创建的拉取请求时，就能看到这条规范的效果。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
