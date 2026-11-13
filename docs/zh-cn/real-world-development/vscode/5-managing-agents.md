---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主工作——探索代码库、提出更改建议、编辑文件并运行命令。由于所有工作都在本机实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以在聊天中内联审查每个差异、接受或拒绝单项更改，并通过后续提示词引导对话，以完善或扩展工作，而无需离开编辑器。

本练习将：

- 实时监控辅助功能智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中引导智能体，要求它在高对比度模式之外添加浅色模式切换。

## 场景

你在[练习 4][previous-lesson]中使用的辅助功能自定义智能体正在实现高对比度模式。工作进行期间，设计团队又提出一项要求：希望为觉得默认主题太暗的用户添加浅色模式切换。你不必从头开始新对话，可以引导现有会话继续完成这项工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和建议的编辑。每项文件编辑都会在聊天中内联显示差异和 **Keep** / **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天记录，查看每次工具调用。你会看到文件读取、文件编辑，以及它请求运行的终端命令。
3. 智能体建议编辑文件时，差异会以内联方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看该会话中智能体修改的完整工作集。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你信任的命令。

5. 等待智能体完成高对比度模式。它应该会显示总结所做更改的最终消息。

## 引导智能体添加浅色模式

高对比度模式已经就绪，现在扩展同一个对话，添加浅色模式切换。由于继续使用相同的聊天会话，智能体会保留之前工作的所有上下文，包括涉及的组件、使用的样式方案和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示词：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续之前的工作，探索已进行的更改并提出添加浅色模式的方案。
3. 新编辑出现时，像处理高对比度模式一样逐一审查，并对每项差异使用 **Keep** 或 **Undo**。
4. 如果智能体的方向不符合预期，例如试图以你不喜欢的方式重构切换 UI，可以选择聊天输入框旁的 **Stop** 按钮停止它，然后发送澄清后的后续消息。

> [!TIP]
> 引导进行中的会话比重新开始更快。可以使用后续提示词要求调整（“简化 CSS——使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换项，把浅色模式集成到现有设置面板中”），或扩展范围（“还要为两个切换项添加键盘快捷方式”）。

## 审查工作集

提交工作之前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有更改文件。你应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并查看差异。确认其中体现了自定义智能体的辅助功能模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你已使用 Copilot Chat 智能体模式监控并引导正在进行的会话，查看每项文件编辑，接受或拒绝更改，并发送后续提示词扩展工作，同时保留上下文。

本课中，你了解了：

- 如何实时监控智能体工作。
- 如何接受或拒绝单项文件编辑。
- 如何通过后续提示词引导正在进行的会话。

在[下一课][next-lesson]中，你将通过拉取请求发布辅助功能更改，并审查本实验中本地自定义智能体生成的工作。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
