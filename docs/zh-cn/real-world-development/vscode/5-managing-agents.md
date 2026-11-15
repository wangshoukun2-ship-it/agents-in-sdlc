---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于工作会实时在本机上执行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以内联审查每个差异、接受或拒绝单项更改，也可以通过后续提示引导对话、完善或扩展工作，全程无需离开编辑器。

本练习将学习如何：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间引导智能体，同时添加浅色模式和高对比度模式切换。

## 场景

[练习 4][previous-lesson]中的无障碍自定义智能体正在实现高对比度模式。工作进行中，设计团队提出了额外需求：为觉得默认主题太暗的用户添加浅色模式切换。无需从头开始新对话，而是引导当前会话扩展工作。

## 监控 Copilot 进度

Copilot Chat 视图会显示智能体采取的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中内联显示差异，并带有 **Keep** / **Undo** 控件。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容查看每次工具调用。你会看到文件读取、文件编辑以及需要运行的终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本会话中修改的完整文件集。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你信任的命令。

5. 等待智能体完成高对比度模式。此时应会看到总结更改内容的最终消息。

## 引导智能体添加浅色模式

高对比度模式实现后，在同一对话中扩展工作，添加浅色模式切换。继续使用当前聊天会话，智能体会保留先前工作的上下文，包括已修改的组件、采用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续工作，探索已完成的更改并提出浅色模式更新。
3. 像审查高对比度模式一样，逐项审查新增编辑。对每项差异使用 **Keep** 或 **Undo**。
4. 如果智能体的处理方式不符合预期（例如尝试以不喜欢的方式重构切换 UI），可以选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送澄清提示。

> [!TIP]
> 在正在进行的会话中引导智能体比从头开始更快。可以使用后续提示要求调整，例如“简化 CSS，改用 CSS 变量而不是重复选择器”；纠正方向，例如“不要添加第三个开关，将浅色模式整合到现有设置面板”；或扩展范围，例如“为两种模式切换添加键盘快捷键”。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查完整的已更改文件列表。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开若干已更改文件并查看差异。确认其中体现了自定义智能体提出的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和下一步

你使用 Copilot Chat 智能体模式监控并引导正在进行的会话：逐项审查文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

本课介绍了以下概念：

- 实时监控智能体工作。
- 接受或拒绝单项文件编辑。
- 通过后续提示引导正在进行的会话。

在[下一课][next-lesson]中，你将发布无障碍更改作为拉取请求，并审查本实验中本地自定义智能体所做的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
