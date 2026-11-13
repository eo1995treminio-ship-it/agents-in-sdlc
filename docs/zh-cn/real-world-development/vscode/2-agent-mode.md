---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发者，工作流程通常包括：找到所有需要修改的文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……后续还有许多类似工作。

Copilot 智能体模式正好可以帮上忙。

Copilot 智能体模式可在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。接下来看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但它们针对两种不同的体验。智能体模式在 IDE 中执行任务，可快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 合并到代码库之前，如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望用户可以按类别和发行商筛选。上一练习已添加发行商辅助函数。现在，你将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改之前，先探索 Tailspin Toys 网站，了解它当前的功能。

这是一个以开发者为主题的桌游众筹平台，允许用户浏览游戏列表并查看游戏详情。网站是一个 Astro 应用，会在构建时将页面呈现为静态 HTML。页面通过 Drizzle ORM 在前置元数据中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则使用少量受限的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，便于启动单个 Astro 开发服务器。可以按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应该位于 `main` 分支。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新的终端窗口。
3. 创建并切换到用于筛选工作的分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果系统提示是否在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 关闭提示框。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost URL 时，系统会自动将你转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到托管 Web 服务器的 codespace 的私有隧道。

### 探索网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示桌游列表，包括标题、图像和描述。
- **游戏详情页**：选择游戏后，会打开显示更多信息的详情页，包括标题、描述、发行商和类别。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的新对话。模式和模型选择因练习而异；每个步骤会在相关处说明。

网站的初始实现已经可以正常工作，不过我们希望添加新功能来改进体验。先查看待办事项。通过模板设置项目时，系统已自动创建一组待办议题。现在让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会的作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此说明中列出了该模型。我们希望由此获得相对一致的代码建议和良好体验。不过，由于 LLM 具有概率性，你收到的建议可能与研讨会示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示词，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 审查生成的议题列表。

注意 Copilot 还会根据它认为对用户最有用的程度，为待办事项排定优先级。

## 查看说明文件

让智能体生成代码之前，先查看用于向 Copilot 提供上下文的说明文件。你将使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)说明文件，了解如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和辅助功能原则。
   - 指向不同文件类型专用说明文件的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径相对于说明文件所在位置。这样可以复用内容、将复杂说明拆分成更易管理的小文件，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要更新应用的三个部分：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 添加或更新筛选行为的测试。
- 更新游戏列表页面，添加筛选 UI。

此外，合并前还需要运行测试并确保它们通过。Copilot 智能体模式可以帮你完成这些任务！现在添加筛选功能。

1. 可以继续当前与 Copilot 的对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图智能体下拉菜单中仍选择了 **Agent**。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 作为模型。
5. 使用以下提示词，要求 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目，找到与目标功能相关的文件。你应该会看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 按钮，因此无需在它工作过程中选择这些按钮。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中要求用户确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 此过程中可能会出现停顿，甚至有测试暂时失败。这没有关系！Copilot 会来回调整代码并运行测试，直到完成任务且不再检测到错误。

   ![Copilot 智能体模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- 合并到代码库之前，如何审查 Copilot 智能体模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，然后创建该功能的拉取请求。

### 奖励探索练习——实现分页

随着游戏列表不断增加，网站需要支持分页。使用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可以考虑：

- 遵循现有最佳实践，包括使用已有的说明文件。
- 考虑如何实现分页，例如是否允许用户选择每页大小，还是使用固定大小。
- 编写提示词时，提供必要指导，确保 Copilot 按照期望实现功能。
- 可能需要与 GitHub Copilot 多次迭代，要求它修改并提供上下文。这是使用 Copilot 的正常工作流程！

## 资源

- [Copilot ask、edit 和 agent 模式：功能与适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
