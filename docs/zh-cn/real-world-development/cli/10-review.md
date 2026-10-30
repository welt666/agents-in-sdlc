---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你通过一套连续的 Tailspin Toys 工作流使用了 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到已准备好的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题出发，在 Plan 模式下确定方案，在 Autopilot 模式下构建，并在 Interactive 模式下审查。
- 使用自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol（MCP）服务器，并在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义智能体，以评估需求、覆盖率、技能结果和浏览器证据。
- 审查了完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 你交付的内容

本研讨会有两个 PR 里程碑，分别从更新后的 `main` 创建独立分支：

1. **星级评分：** 在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流：** 实现筛选、更新说明并将其应用到功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为了简化研讨会，这些工作合并在同一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 进行浏览器探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 综合这些结果，并在创建 PR 前审查需求和测试覆盖率。

新增测试应解决实际覆盖缺口；如果 QA 检查后确认无需添加测试，也可能是合理结果。授权合并前，请审查代码和证据；如果代码更改影响了已有证据，请重新验证。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了说明、自定义技能、创建 QA 配置并设置 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明定义规范，技能描述可重复任务，自定义智能体定义专用角色，MCP 服务器则连接外部工具。请审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先思考方案，使用 **Interactive** 参与聚焦更改的过程，对范围明确的任务使用 **Autopilot**。日常修改可选择速度更快的模型，复杂任务则选用能力更强的模型。

上下文仍与基础设施同样重要。清晰说明要构建什么、为什么构建以及应如何构建，会显著影响输出结果。

## 深入探索

你已完成核心工作流。还有一些值得了解的 CLI 功能：

- 使用 `/review` 请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种思路。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

要熟练使用任何工具，最好的办法就是持续实践！将它用于生产代码、个人项目，或者用于实现一个想了多年的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想在终端中扩展 Tailspin Toys，可继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
