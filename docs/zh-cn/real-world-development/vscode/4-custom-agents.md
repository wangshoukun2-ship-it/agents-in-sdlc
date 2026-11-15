---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。你可以通过存储库 `.github/agents` 文件夹中的 Markdown 文件定义智能体，为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业知识编写成可复用的智能体，例如执行 [WCAG][wcag] 合规审查的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后续 Markdown 提示则定义智能体的行为、专业知识和说明。

### 自定义智能体与智能体技能的区别

自定义智能体与[智能体技能][agent-skills-concept]之间有一定重叠。两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的角色，技能是它可使用的工具。**

自定义智能体有自己的上下文窗口，并用于协调技能（甚至其他智能体）完成工作。在本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；它可以在工作中调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，应在自己的环境和场景中测试并迭代改进。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题的文字与背景颜色对比度不足，导致部分用户难以阅读。为解决此无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此需要尽快实现此功能。接下来将使用自定义智能体生成所需功能。

本练习将学习如何：

- 查看现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

项目已经提供了无障碍自定义智能体。先查看它的内容，了解它将如何指导 Copilot。

返回 codespace，打开终端，并从 `main` 创建新分支来实现无障碍功能（这样可以将第 3 课的筛选 PR 分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 继续浏览后续部分，了解其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，会话将遵循该智能体的说明和工具设置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新会话。每个练习会在相关步骤说明所需模式和模型，因为两者会因练习而异。

1. 如果 Chat 视图的智能体下拉菜单中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单（其中显示当前智能体，默认为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并编辑项目文件。每项编辑都会以内联方式显示在聊天中，并包含文件路径和差异供你审查。
6. 编辑落地后，**Files changed** 指示器会更新，显示智能体已修改的文件集。

> [!NOTE]
> 这可能需要几分钟。Copilot 会对代码库进行实际更改，可能会修改 Astro 组件、CSS 和相关测试。

下一课将审查并引导这项正在进行的工作。

## 总结和下一步

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。借助自定义智能体，可以将团队专业知识和标准编写成可复用智能体，从而指导 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 定义自定义智能体。
- 在 Copilot Chat 智能体模式中使用自定义智能体。

接下来[监控并引导智能体的工作][next-lesson]，审查正在产生的更改，并在同一会话中添加浅色模式切换。

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
