---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续工作流中使用了 GitHub Copilot CLI。你：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到已创建的筛选议题。
- 添加星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式中确定方案、在 Autopilot 模式中实现，然后在 Interactive 模式中审查。
- 使用自定义说明指导智能体，然后自定义现有 `quality-checks` 技能并运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol（MCP）服务器，在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖范围、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于上下文、模型、共享和可选云端委派的斜杠命令。

## 已交付的内容

本研讨会包含两个 PR 里程碑，它们分别从更新后的 `main` 分支创建：

1. **星级评分：**在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流：**实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为了简化研讨会，这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自使用浏览器检查，以及通过 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 会将这些结果与需求和测试覆盖率审查结合起来，再供 PR 使用。

新增测试应解决真实缺口；如果 QA 检查后无需新增测试，也可能是正确结果。授权合并前，请审查代码和证据，并在更改后更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了说明、自定义了技能、创建了 QA 配置并配置 MCP 服务器。可以在多个会话中复用这些自定义项，并根据团队需求调整。说明定义标准，技能描述可重复执行的任务，自定义智能体定义专家角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 参与专注的小规模更改，使用 **Autopilot** 执行范围明确的任务。日常修改可选择速度更快的模型，复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建什么、为什么构建以及如何构建，会显著影响输出结果。

## 更多可探索的功能

你已完成核心工作流。还可以了解一些 Copilot CLI 功能：

- `/review`：要求代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获取另一种看法。
- `/fleet`：并行协调彼此独立的子任务。
- `/worktree`：隔离一项单独任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方法是持续实践！可以用它编写生产代码、兴趣项目，或构思已久但尚未动手的小应用。与团队分享经验，也向团队成员学习。最后，继续探索文档。

如果想继续从终端扩展 Tailspin Toys，可以学习可选的 [Foundry Backer Concierge 系列][foundry]。还可以对比其他环境，探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

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
