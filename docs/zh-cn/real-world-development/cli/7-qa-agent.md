---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建一个 QA 自定义智能体，结合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到自定义智能体中，定义清晰的 QA 流程。

本课将学习如何：

- 探索自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，一致地审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并提供可复用报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明智能体的用途、说明和可用工具。本研讨会将把 QA 角色定义在 `.github/agents/qa.agent.md` 中，并在 Copilot CLI 中选择该角色。

你已使用的各项自定义功能各有用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 如何利用这些能力评估需求并报告发现。QA 配置不会取代它们，也不需要另开对话。

## 创建 QA 配置

打开功能 PR 之前，让 Copilot 创建可复用的 QA 配置。该配置将定义 QA 执行的检查及其必须遵循的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 要求默认智能体创建自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先审查其配置，确认 Copilot 捕获了预期 QA 流程和边界。这可以避免你只想验证功能时，智能体却以不完整或过宽的范围更改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必需项，`name` 为可选项，但添加名称可为智能体提供清晰的显示名称。
3. 阅读配置说明，确认 QA 会从需求开始、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持性证据，在更改实现代码前征求同意，并且不会提交更改或创建拉取请求。
5. 如果生成的配置遗漏上述职责或边界，要求默认智能体修改后再继续。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一筛选对话，然后选择 QA 智能体，以便继续使用现有议题和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会显示在可用智能体列表中，但仍可以使用上述命令启用。

2. 使用以下提示要求 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作。
4. 完成后阅读它提供的报告。

## 总结和下一步

你已添加可复用的专家角色并审查其工作。本课中，你：

- 了解了自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 智能体，根据筛选议题审查其发现。

现在已有实现、技能更新、QA 配置、测试和验证报告可供审查。接下来[将这些内容汇总到功能 PR 中并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
