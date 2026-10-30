---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "探索存储库说明、添加文档规范，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应确保 Copilot 能获得相关指导。[说明文件][instruction-files]不仅描述你想要什么代码，也描述代码应如何组织。现在你已经实现了筛选功能，接下来将查看 Copilot 使用的说明、添加文档规范，并将其应用到代码中。

本课将：

- 探索存储库说明和按路径限定的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码规范。
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 应保留现有格式和 lint 指南。

通过说明文件，你可以确保 Copilot 获得正确的信息，并按照这些实践执行任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。它能让你引导 Copilot 生成更符合需求的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望添加的注释类型。说明既可以适用于整个存储库，也可以针对特定类型的文件或任务。

说明文件有两种：

- `.github/copilot-instructions.md`：每次请求都会发送给 Copilot 的单个说明文件。它应包含适用于存储库大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：提供特定语言、文件类型或任务的指导。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。依赖某种格式前，请先查看[自定义说明支持参考][custom-instructions-support]。

## 探索此项目中的自定义说明文件

开始修改之前，先查看入门项目中已有的说明文件，了解当前指引。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解项目简介和编码指南。这些说明适用于在此存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有 Astro 文件、Drizzle 数据层、测试等说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它定义了适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他说明文件和项目文件。这种方式可以将大型说明拆分成可复用的小文件，并提供示例供 Copilot 参考。

## 更新说明文件以符合团队指南

现有文件是不错的起点，但仍有不足。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指南

Copilot CLI 会在对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指南。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录运行以下命令，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指南：

   ```plaintext
   我们刚刚更新了说明和代码指南。请根据新指南更新你生成的代码。
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你探索了 Copilot 如何从说明文件中获取上下文，并将新的规范应用到了功能中。具体来说，你：

- 探索了存储库说明和按路径限定的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码规范。
- 查看说明文件对代码的影响。

接下来，使用可复用的 `quality-checks` 技能[自定义并运行检查][next-lesson]，确保始终运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
