---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 一起完成了多项任务，以改进用户体验并添加功能：为代码添加文档、为设计团队实现相关游戏功能，以及实现包括高对比度和浅色模式切换在内的无障碍功能。现在来检查代码更改；如有需要，还可以向 Copilot 提供反馈，改进其工作成果。

### 场景

如前所述，加入生成式 AI 不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来先审查 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议，再为团队其他成员启用审查。

## 安全和 GitHub Copilot 云智能体

由于 Copilot 云智能体会异步且不受监督地执行任务，因此它受到一些安全限制，以确保操作安全：

- Copilot 只能读取存储库，并且只对用于其代码工作的分支拥有写入权限。
- 云智能体在 GitHub Actions 中运行，并创建独立、临时的工作环境。
- 所有 GitHub Actions 工作流都必须由人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，其中包括 MCP 服务器。

## 审查生成的文档

先通过 GitHub.com 上的标准 PR 界面，查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），即为代码添加文档的 PR。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 在 github.com 上返回存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或者描述更具体的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以休息一下或回顾迄今为止学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新生成的 TSDoc 文档注释和其他文档。具体更改可能会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自你在练习 2 开始时查看的自定义说明文件。

6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应在拉取请求检查区域中排队运行。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 进行更改

与 Copilot 协作并非单向过程。你可以像在拉取请求或代码行内评论中提及团队成员一样提及 Copilot。Copilot 会读取这些评论并启动新会话来处理请求。由于结果具有非确定性，无法规定确切的提示文字。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加一条评论，请求修改生成的文档，并像提及用户一样标记 **@copilot**。可以使用上述建议，也可以提出其他希望在代码库中看到的文档要求。
2. 选择 **View Session**，观察 Copilot 执行修改。留意 Copilot 如何启动新会话来进行更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。确认满意后，可将 PR 从草稿转换为就绪状态，再合并到主分支。

![将 PR 转为可供审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或者描述更具体的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流应在 PR 检查区域中排队运行。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能！和之前一样，可以迭代改进云智能体的工作。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明）。和之前一样，可在 **Conversation** 选项卡中添加评论来提出请求；Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心要求添加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或者描述更具体的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现时，尤其要留意：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应在 PR 检查区域中排队运行。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换，确认功能符合预期且在重新加载页面后仍然生效。

留意无障碍自定义智能体如何帮助 Copilot 根据无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派工作

云智能体最适合处理真实的待办事项，让你能转而处理其他工作。继续练习这种工作方式：在存储库中创建更多议题并分配给 Copilot。可以尝试以下建议：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体体验路径。在这些课程中，你：

- **检查了存储库提供的自定义说明文件**，稍后可在云智能体的输出中看到它们的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**，实现高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为任务控制中心**，在无障碍会话执行期间监控并引导它。
- **审查并迭代改进云智能体创建的拉取请求**，使用 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成云智能体体验路径。如果想继续探索，其他体验路径可补充本次实践的内容：

- 🖥️ **[VS Code 体验路径](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 体验路径](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，还可以尝试：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
