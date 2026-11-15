---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发人员的工作流程往往包括查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件并再次修改……事情可能没完没了。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式能够在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，再执行必要更新、运行测试等任务，并自动修复发现的问题。接下来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但它们针对不同的使用体验。智能体模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习如何：

- 让 Copilot 智能体模式探索项目、查找相关文件并协调多处更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户可以按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改之前，先探索 Tailspin Toys 网站，了解它目前的功能。

该网站是一个以开发者为主题的桌游众筹平台，可显示游戏列表和详情。网站是单体 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能则使用少量范围明确的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。可按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，当前应位于 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下 URL 的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，选择终端中的 localhost URL 会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到托管 Web 服务器的 codespace 的私有隧道。

### 探索网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示桌游列表，包括名称、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示游戏名称、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新会话。每个练习会在相关步骤说明所需模式和模型，因为两者会因练习而异。

网站的初始实现已可正常运行，但我们希望添加新功能。先查看待办事项。使用模板创建项目时，系统已自动创建一组待办议题；现在让 GitHub Copilot 帮你查看这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未推荐任何特定模型。构建本研讨会时使用的是 Claude Sonnet 4.5，因此将其列入说明，希望你能获得相对一致的代码建议。但由于 LLM 具有概率性，你收到的建议可能与研讨会中的示例不同，这是完全正常且可预期的。

> [!NOTE]
> 由于 LLM 的概率特性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题待办列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 审查生成的议题列表。

留意 Copilot 甚至会根据对用户的价值帮你排列议题优先级。

## 查看说明文件

启动智能体生成代码前，先查看将用于提供上下文的说明文件。接下来使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)说明文件，其中介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中介绍的功能开发指南，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用说明的链接，包括：
     - Astro。
     - Tailwind CSS。
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置。这样可以复用说明、拆分复杂指导，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三项更新：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，添加筛选 UI。

此外，合并前必须运行并通过测试。Copilot 智能体模式可以帮你完成这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选择 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，要求 Copilot 根据待办列表中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。它应会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。过程中编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 选项，因此无需在工作进行期间选择。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 处理中可能会暂停，甚至发现某些测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且未检测到错误。

   ![包含完整 Copilot 智能体模式会话的 Chat 截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站浏览器，探索新功能。
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和下一步

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你已了解如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 让 Copilot 智能体模式探索项目、查找相关文件并协调多处更改。
- 在合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

现在使用 [Playwright MCP 服务器测试功能][next-lesson]，然后创建拉取请求。

### 加分探索练习 — 实现分页

游戏列表不断增长后，需要添加分页。运用本练习学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用已有的说明文件。
- 考虑分页方式：允许用户选择页面大小，还是使用固定页面大小。
- 编写提示时，提供足够指导，让 Copilot 按预期实现功能。
- 可能需要与 GitHub Copilot 多次迭代，提出修改要求并补充上下文。这是正常的协作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
