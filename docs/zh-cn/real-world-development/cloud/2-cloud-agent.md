---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

很多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发人员专注于更重要的工作。

本练习将学习如何：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务。最初受雇开发网站的承包商没有留下完善的文档，事实上，项目中完全没有文档。团队希望首先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现功能。他们希望每个游戏详情页都显示相关游戏，也就是同一类别中的其他作品。暂时不需要完善的实现，只需要可用于 UX 验收测试的功能。这个任务目前阻碍了工作，但其他议题的优先级更高。

这些任务很容易被不断推迟，适合交给 Copilot 云智能体异步处理。开发人员可以继续其他工作，之后再回来审查 Copilot 的成果并确认符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以在后台执行任务，方式类似于人工开发人员。与人工协作一样，可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在会话进行中或完成后查看其过程。解决方案准备好审查时，Copilot 会在拉取请求中提及你。

## 编写范围明确的说明很重要

GitHub Copilot 没有魔法，不能替代思考。即使看似简单的操作，也可能包含复杂环节，因此请注意如何为 Copilot 云智能体[界定任务范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段工作、学习、试验并不断调整。加入生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何指导 Copilot。如果已完成自定义说明的实践练习，就已看过 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。分配任务前，先只读查看存储库中已有的说明文件，之后便能观察它们的影响。

在 GitHub Web 界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将这些说明限定用于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述说明文件。后续练习审查 PR 时还会再次提到。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要准备特定环境并运行一些设置脚本，确保环境就绪。将任务分配给 Copilot 时也一样，它会像软件工程师一样处理任务。

云智能体使用 [GitHub Actions][github-actions] 执行工作。你可以创建一个[专用设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 中，让智能体开始工作前先运行，从而获得所需开发工具和依赖项。为便于实验，此处已预先配置好工作流。它会为单体 Astro 应用准备 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，从而测试工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档很重要，但大多数项目的信息要么已过时，要么完全缺失。这类技术债务常被忽略，会降低效率，也会增加维护代码库或帮助新开发人员加入团队的难度。Copilot 很擅长编写文档，因此适合将这类议题分配给云智能体。它会在后台生成所需文档，之后可在后续练习中审查结果。

1. 在新的浏览器选项卡中转到 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中暂时没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定处理任务的方案。

13. 审查列表及其计划完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到最新状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并决定合适方案，以及编写代码。

这可能需要几分钟。云智能体的主要目标之一是异步执行任务，让我们有时间处理其他工作。接下来会利用这一点：先把另一个任务分配给云智能体，再编写代码为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你可以处理一组任务，而它同时处理另一组。为设计团队添加相关游戏部分可能用不了很久，但这些时间仍可用于其他任务。现在将它分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会看到议题中的第一条评论旁出现 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！云智能体的工作方式类似软件工程师，因此无需一直监控它；等任务完成后再审查即可。现在可以开始了解如何创建和使用自定义智能体。

## 总结和下一步

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理。云智能体可用于解决技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

本课介绍了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

云智能体正在后台处理任务。接下来将了解如何创建并使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
