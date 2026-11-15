---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解某些背景信息，就需要确保它能获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[说明文件][instruction-files]来指导代码生成。

本练习将学习如何：

- 通过存储库自定义说明和按路径限定范围的说明文件，将项目上下文、编码指南和文档标准提供给 Copilot。
- 在现有说明下生成筛选功能的第一个数据切片（publisher 辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 提交说明更新和筛选代码切片到 `main`，供下一课中的 Copilot 使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全遵循设置的标准，因为 Copilot 的输出具有非确定性。本练习旨在观察更新说明后行为的总体变化，而不是要求输出与示例逐字相同。

## 说明文件

### 场景

和其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过说明文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它能让你引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码约定、库以及注释风格，也可以为整个存储库或特定文件类型创建说明。

说明文件分为两类：

- `.github/copilot-instructions.md` 是一个说明文件，会随该存储库的**每个**请求发送给 Copilot。它应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md` 可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境对其他说明格式的支持情况各异。依赖特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目的自定义说明文件

入门项目中已包含一组说明文件。先探索现有文件，再进行更改，以便了解更改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指导。这些说明适用于在此存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。留意针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它指定适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他说明文件和现有项目文件的链接。通过这种方式，可以把较大的说明集拆分成更小、可复用的文件，并提供可参考的示例。

## 创建更改分支

遵循最佳实践，为更改创建分支。

1. 返回上一练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新会话。每个练习会在相关步骤说明所需模式和模型，因为两者会因练习而异。

为了观察自定义说明的影响，先在当前说明文件下发送提示。更新说明后，再次发送相同提示并比较结果。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，让 Copilot 可以应用文件更改。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时涉及辅助函数文件和测试。
6. 留意建议的辅助函数是一个经过类型标注的函数，第一个参数接收 `db` 客户端，并返回经过类型标注的 publisher 数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有相应要求，它也可能添加文档注释。这也没关系；本练习的重点是观察更新说明后行为的一致性是否提高。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来将编码标准写入存储库，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，应该在第 27 行附近。留意其中记录了项目编码标准，但尚未规定代码内文档，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局说明，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供说明。刚才添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 再次运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除当前对话。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意现在建议的文件是否以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议的函数是否现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

现在，你已引导 Copilot 遵循新的项目标准，并将其应用于真实代码，供下一课继续构建。

## 提交说明并推送分支

说明文件和存储库中的其他资源一样，使用相同的源代码管理流程。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改已保存：

   ```bash
   git status
   ```

3. 暂存并提交说明更新以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求并将其关联到待办事项中的文档标准议题。可以手动操作，也可以让 Copilot 使用已连接到项目的 GitHub 工具查找议题、创建用于关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已为工作区配置 GitHub 工具，因此无需设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示完成登录。后续练习会介绍这种连接（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中已选中 **Agent**。
3. 要求 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明其中既更新了说明，也添加了新的 publishers 辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 当 Copilot 提示 **Allow** 其代表你执行 GitHub 操作时，请先审查操作内容，再酌情选择 **Allow**。
6. PR 创建后，要求 Copilot 将其合并并把分支切回 main，使用以下提示：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 提示 **Allow** 其代表你执行 GitHub 操作和 shell 命令时，请先审查命令，再酌情选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和下一步

你已了解 Copilot 如何读取说明文件，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 如何根据**现有**说明生成代码。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次发送相同提示，观察建议的代码如何遵循新标准。
- 将说明更新和辅助函数基础提交到 `main`，供下一课使用。

接下来，[使用智能体模式][next-lesson]让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件和其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
