---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。[说明文件][instruction-files]可提供此类指导，让 Copilot 不仅了解你希望它做什么，也了解你希望它如何完成。

本练习将：

- 了解项目专属上下文、编码指南和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不会运行前后对比提示词。Copilot 云智能体在 GitHub 议题上异步运行，因此难以实时并排展示效果。稍后审查云智能体创建的拉取请求时，你会看到说明文件的影响。

## 说明文件

### 场景

与其他优秀的开发团队一样，Tailspin Toys 也制定了开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过使用说明文件，可以确保 Copilot 获得正确信息，并按照这些实践要求完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。借助此功能，可以指定首选的编码约定、库，甚至希望添加的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型创建说明，为任务提供上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库提出请求时，都会将这个单一说明文件发送给 Copilot。此文件应包含项目级信息，即大多数发送给 Copilot Chat 或 CLI 的请求都会用到的上下文。内容可以包括技术栈、正在构建的内容概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建这些文件。例如，可以为 TypeScript 等语言，或创建 UI 组件、编写单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体都会使用存储库级说明文件，以及带有 `applyTo` 前置元数据的 `*.instructions.md` 文件。
>
> 此外，Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本研讨会的范围。不过，示例项目展示了具有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 限定为项目级指导，例如正在构建的内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，清晰地编写指导，并提供代码的正反示例。

创建说明文件没有唯一正确的方法，使用 AI 也是如此。通过试验，你会逐渐了解哪些方法适合自己的项目。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。探索本项目中的文件时，你可能会注意到，其中包含用于许多任务的说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的操作各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相应界面的课程会在相关处介绍具体操作。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义说明文件

花点时间阅读此存储库附带的说明文件：其中有一个核心文件 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目的简要说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。使用 Copilot 时，这些内容都适用。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。你会看到适用于 Astro 文件、Drizzle 数据层、测试等的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于确定说明适用的文件。这里，任何匹配 `**/*.test.ts`（例如 TypeScript 测试文件）的文件都会应用此说明。
5. 留意针对本项目单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。相关路径是相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求在代码中添加说明。接下来，你会添加 TSDoc 文档注释和文件头注释的规则。

## 添加新的存储库标准

接下来要编辑的内容只有一项：添加项目级规则，要求代码使用 TSDoc 文档注释并包含文件头注释。云智能体稍后处理你分配的议题时会遵循这项规则。

编辑前，先创建一个分支进行操作（云智能体会从议题目标分支读取说明，但仍然建议先整洁地提交你的更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，要求 Copilot 编写代码说明，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应靠近第 27 行）。留意此处对项目编码标准的说明，但还没有关于代码文档的规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，以要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局说明，也可以为特定语言、文件类型或任务创建 `*.instructions.md` 文件。存储库级文件适合存放项目范围的标准，例如刚刚添加的文档注释规则。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支上的说明文件。下一课将把议题分配给 Copilot，届时 Copilot 会从 `main` 分支创建新分支。因此，你的说明更改必须先合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。下一课分配议题后，云智能体就会读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支操作并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何使用本项目的说明文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。接下来的练习中，你会看到云智能体创建的拉取请求如何遵循这项标准。

下一步，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
