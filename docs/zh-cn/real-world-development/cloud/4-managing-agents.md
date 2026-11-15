---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体执行了三个任务，旨在改进用户体验并添加功能。虽然云智能体被设计为异步自主工作，但监控这些任务仍很重要。

可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有包含未关闭拉取请求（PR）的智能体任务，了解已执行的操作，也可以引导正在进行的会话。

本课将学习如何：

- 使用智能体页面监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

分配高对比度模式任务后，团队意识到也可以同时添加浅色模式。既然正在更新网站样式并添加切换功能，一并实现浅色模式就很合理。现在需要引导智能体，确保它同时添加浅色模式和高对比度模式。

## 审查 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务状态。

1. 转到 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 留意主面板和左侧面板中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 根据 Copilot 的工作状态和所采用的方法，具体列出的步骤会有所不同。

4. 同时留意右侧显示的拉取请求（PR）面板。可在此查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

现在已了解活动任务，接下来要求 Copilot 在实现高对比度模式的同时添加浅色模式。

1. 选择表示高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成设置并开始工作。此时会出现类似下文的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示 Steer active session while Copilot is working 对话框的云智能体任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其加入当前工作流程。

## 让 Copilot 完成工作

和之前一样，Copilot 会继续处理更新后的任务！收到消息时，它会先完成当前步骤，然后将新要求加入工作流程。

这同样需要几分钟。此时可以暂停一下，回顾目前学到和探索的内容。

## 总结和下一步

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。通过任务控制中心，可以跟踪所有正在进行和已完成的任务、审查已执行的工作，也可以重定向正在进行的任务以调整范围或补充指导。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话以请求添加其他功能。

Copilot 正在完成无障碍功能。接下来将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
