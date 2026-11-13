---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你要求 Copilot 云智能体处理了三项任务，以改善用户体验并添加功能。云智能体虽然能够异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 Mission Control 中，可以查看所有创建了未关闭拉取请求的智能体任务，检查已执行的操作，甚至在会话进行中引导智能体。

本课将：

- 探索智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到正好可以同时添加浅色模式。由于已在更新网站样式并添加切换功能，因此添加该功能也很合理。现在，你希望引导智能体的工作，让它在实现高对比度模式的同时也添加浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 前往 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。这里应显示你分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作。这些操作可能包括：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和执行方式而变化。

4. 另外，请留意右侧显示的拉取请求（PR）面板。你可以在这里查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

现在你已查看正在进行的任务，可以在智能体实现高对比度模式时要求它添加浅色模式切换功能。

1. 选择有关添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![Mission Control 中的辅助功能会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行工作。看到类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面上的云智能体任务截图，其中突出显示了 steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 注意 Copilot 会确认收到提示词，并将其纳入工作流程。

## 等待 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务！在收到消息时，Copilot 正在执行的步骤完成后，它会将新要求纳入工作流程。

这同样需要几分钟。你可以利用这段时间回顾到目前为止学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。借助这个 Mission Control，你可以跟踪所有正在进行和已完成的任务、审查已完成的工作，甚至在任务执行过程中调整范围或提供更多指导。

你了解了：

- 如何使用 Mission Control 和智能体页面监控云智能体任务。
- 如何引导正在进行的会话，要求添加其他功能。

Copilot 正在完成辅助功能。接下来，开始[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
