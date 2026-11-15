---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就需要确保它能获取这些上下文。[说明文件][instruction-files]用于提供这些指导，让 Copilot 不仅知道要做什么，也了解应如何完成。

本练习将学习如何：

- 通过存储库自定义说明和按路径限定范围的说明文件，将项目上下文、编码指南和文档标准提供给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 体验路径不同，本体验不会在此处运行“前后对比”提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示效果。稍后审查云智能体创建的拉取请求时，可以看到说明文件的影响。

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

- `.github/copilot-instructions.md` 是一个说明文件，会随该存储库的**每个**请求发送给 Copilot。它应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的说明文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

全面讨论如何创建说明文件超出了本实验的范围，但示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保说明清晰，并提供代码示例说明推荐或不推荐的写法。

说明文件并没有唯一正确的写法，AI 的使用方式也是如此。通过试验，逐渐找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套可靠的说明文件。查看本项目中的文件时，会发现其中涵盖了多种任务，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的操作方式各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前体验路径会在相关处介绍具体操作。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收录了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses

## 探索本项目的自定义说明文件

花些时间查看此存储库附带的说明文件：一个核心 `copilot-instructions.md` 和一组用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的嵌套 **GitHub Actions Workflows** 指导。这些说明适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览。留意其中关于 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它指定相对于存储库根目录的 glob，用于确定适用文件。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 留意针对本项目单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。留意它指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。通过这种方式，可以把较大的说明集拆分成更小、可复用的文件，并提供可参考的示例。（其中的路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但目前尚未要求添加代码内文档。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来只需编辑一处：添加项目级规则，要求以 TSDoc 文档注释和文件级注释头的形式在代码中编写文档。云智能体会在本体验后续部分处理分配给它的议题时使用此规则。

编辑前先创建一个分支。（云智能体会从议题目标分支读取说明，但仍建议在分支上清晰提交更改。）

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。在下一课中，将议题分配给 Copilot 时，它会从 `main` 分支创建分支。因此，说明更改必须先合并到 `main`，云智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一课中，云智能体处理分配给它的议题时就会读取这些说明。

> [!TIP]
> 如果想在本研讨会中直接使用 `main`，可以跳过分支操作并直接提交到 `main`。此处创建分支是为了模拟真实项目中的处理方式。

## 总结和下一步

你已了解 Copilot 如何读取本项目的说明文件，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。接下来的练习中，可以在云智能体创建的拉取请求中看到该标准的应用。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
