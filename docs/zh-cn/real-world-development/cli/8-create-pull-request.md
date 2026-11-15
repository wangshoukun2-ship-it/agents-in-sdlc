---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一个分支中。现在一起审查这些更改并创建拉取请求。之前你手动合并了星级评分 PR；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体分别放到几个 PR 中。为简化本研讨会，我们让完整的筛选和质量工作流共用一个对话和分支，并将所有更改放进同一个 PR。

本课将学习如何：

- 了解 Agent Merge 及其如何自动化合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查完成后启用 Agent Merge 并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动化剩余 PR 工作，同时由开发人员控制合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复审查评论以及在需要时变基；随后，在存储库允许的情况下启用 GitHub 自动合并。

之前你都是自行选择 **Merge pull request**。Agent Merge 可以代为执行此操作，但启用前仍应先审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码编写完成后，一起审查代码、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成过程。一切检查成功后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的批准、分支保护、合并队列、存储库设置或权限不足等限制。如果操作被阻止，请阅读报告的原因，并在存储库允许的情况下完成经审查的手动合并。

## 总结和下一步

你已自动化开发流程中的多个部分，包括生成代码、测试和验证代码，以及管理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动化合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查完成后启用 Agent Merge，并确认 PR 已合并。

接下来[探索其他实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
