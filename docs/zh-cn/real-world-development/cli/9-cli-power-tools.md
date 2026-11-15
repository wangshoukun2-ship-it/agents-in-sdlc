---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令提供高级功能、后台信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来了解其他实用命令。

本课将学习如何：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树以及委派任务给云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。接下来了解一些其他功能：管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会达到模型上下文窗口的上限。Copilot CLI 会在需要时自动压缩对话，也可以使用斜杠命令查看或手动压缩。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意模型、当前 token 用量，以及上下文在系统说明、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近容量上限时，Copilot CLI 会自动压缩上下文。若要自己选择压缩时机，请使用 `/compact`。如果要切换到无关任务并开始新对话，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发人员也有不同偏好。Copilot CLI 支持列出并选择模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型及其使用情况。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作并分享经验有助于提升所有人使用 AI 工具的能力。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 如果要共享此会话，请选择合适的目标，例如 `/share file`，导出本地 Markdown 文件。
3. 发送或发布之前，先审查导出内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选的。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心研讨会已完成。Copilot CLI 还提供适用于较大任务的命令：

- `/fleet` 可将彼此独立的子任务分配给子智能体并行执行。
- `/worktree` 可为独立任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot 云智能体，由它异步工作并可能创建拉取请求。

这些命令是可选的，因为它们可能会创建其他工作树或触发远程工作。试用前，请启动新的、范围明确的任务，并通过正常工作流审查结果。如果想深入了解异步智能体工作，可继续学习[云智能体研讨会][cloud-workshop]。

## 总结和下一步

使用 Copilot CLI 斜杠命令可以对其进行配置、共享会话并了解后台运行情况。本课中，你：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树以及委派任务给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令可供探索！接下来[回顾所学内容][next-lesson]，了解后续学习建议。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
