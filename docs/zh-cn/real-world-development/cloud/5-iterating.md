---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在整个实验中，你与 GitHub Copilot 合作处理了多项任务，以改善用户体验并添加功能。你要求 Copilot 为代码添加文档、构建相关游戏功能供设计团队迭代，并使用辅助功能自定义智能体实现高对比度和浅色模式切换。现在来检查代码更改；如有需要，还可以向 Copilot 提供反馈，进一步改进其工作。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。因此，在为团队其他成员启用审查之前，先检查 GitHub Copilot 为文档、相关游戏功能和辅助功能所做的更改。

## 安全性与 GitHub Copilot 云智能体

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此必须设置一些安全限制，确保一切安全。这些限制包括：

- Copilot 只能读取存储库，并且**只能**写入用于处理代码的分支。
- 云智能体在 GitHub Actions 中运行，并创建一个独立的临时环境来工作。
- GitHub Actions 工作流需要先由人工批准才能运行。
- [默认情况下会限制对外部资源的访问](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），其中包含为代码添加的文档。你将使用 GitHub.com 上的标准 PR 界面完成此操作。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器调用。你也可以[为 Copilot 云智能体自定义或禁用防火墙][agent-firewall]。

1. 在 github.com 上返回存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会显示 **[WIP]**。此时请等待 Copilot 完成工作。这可能需要几分钟，你可以休息一会儿或回顾目前学到的内容。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包含新生成的 TSDoc 文档注释和其他文档。具体更改可能会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件头注释。这些要求来自你在练习 2 开始时查看的自定义说明文件。

6. 审查更新并确认内容无误后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到一些工作流正在等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流在拉取请求的检查部分排队运行。如果一切正常，单体 Astro 应用的项目检查就会通过。完成此过程可能需要几分钟。

## 要求 GitHub Copilot 更改

与 Copilot 合作并非单向过程。你可以像对待团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动另一个会话来处理相关要求。由于结果具有非确定性，我们无法提供固定的请求文本。以下是一些可以要求 Copilot 更新的内容：

- 在每个代码文件开头添加简短的文件头注释，说明该文件的用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论来请求修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上面的任一建议，也可以提出其他希望为代码库添加的文档要求。
2. 选择 **View Session**，观察 Copilot 执行修改。注意 Copilot 会启动新会话来完成更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成修改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡以审查更改。

如果还不满意，可以继续迭代。确认满意后，可以将 PR 从草稿状态改为准备好审查，然后合并到主分支。

![将 PR 标记为准备好审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来，查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认内容无误后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到一些工作流正在等待批准的提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 你应该会看到工作流在拉取请求的检查部分排队运行。如果一切正常，单体 Astro 应用的项目检查就会通过。完成此过程可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，可以通过迭代让 Copilot 云智能体继续更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件头注释和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明）。和之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会看到评论并启动新会话。

## 审查辅助功能

最后，审查使用辅助功能自定义智能体实现的辅助功能。此 PR 应该同时包含你在练习 3 中要求的高对比度模式，以及你在练习 4 的 Mission Control 中要求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会显示 **[WIP]**。此时请等待 Copilot 完成工作，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现时，特别留意：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 辅助功能属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认内容无误后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到一些工作流正在等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流在拉取请求的检查部分排队运行。如果一切正常，单体 Astro 应用的项目检查就会通过。完成此过程可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到该分支，手动测试辅助功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换，确认它们可以正常运行，并能在重新加载页面后保留设置。

请注意，辅助功能自定义智能体帮助 Copilot 根据辅助功能最佳实践实现这些功能。如果发现任何辅助功能问题或可改进之处，可以像之前审查 PR 时一样在评论中提及 **@copilot**，要求它进行更新。

## 可选练习 — 继续委派工作

云智能体最适合处理真实的待办事项，让你能将工作交给它并继续处理其他事情。可以通过在存储库中创建更多议题并分配给 Copilot，来培养这种工作习惯。以下是一些建议：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体环境中的所有练习。在这些课程中，你：

- **检查了存储库自带的自定义说明文件**，以了解这些说明对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**处理辅助功能，添加高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为 Mission Control**，监控并引导辅助功能会话。
- **审查并迭代云智能体的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

云智能体环境中的练习已完成。若想继续探索，其他环境的练习也可以扩展你刚才学到的内容：

- 🖥️ **[VS Code 环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 在终端中使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 桥接到云智能体，交付两项经过审查的更改。

你也可以在自己的存储库中尝试以下后续任务：

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
