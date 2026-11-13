---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的专用 AI 助手。你可以通过存储库 `.github/agents` 文件夹中的 Markdown 文件定义智能体，为 Copilot 提供有针对性的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业经验整理为可复用的智能体，例如执行 [WCAG][wcag] 合规性检查的辅助功能智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在 `~/.copilot/agents` 中全局定义。每个文件都包含 YAML 前置元数据，至少要有 `name` 和 `description` 字段，后面则是定义智能体行为、专长和说明的 Markdown 提示词。

### 自定义智能体与智能体技能的区别

自定义智能体与[智能体技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是工作者，技能是工具**。

自定义智能体拥有自己的上下文窗口，并负责协调技能（甚至其他智能体）来完成工作。在本实验中，辅助功能自定义智能体会根据辅助功能指南审查并更新网站；执行过程中，它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他应用一样，应针对自己的环境和场景进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

使用自定义智能体时，你将了解：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的影响。近期的用户反馈指出，由于文字与背景颜色对比度不足，一些用户觉得当前的暗色主题难以阅读。为解决这个辅助功能问题，设计团队要求实现一种可由用户开启或关闭的高对比度模式。

辅助功能至关重要，因此你希望尽快完成这项功能。接下来将使用自定义智能体生成相关功能。

## 查看辅助功能自定义智能体

我们已经为你创建了辅助功能自定义智能体。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后查看辅助功能自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML 前置元数据。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` 前置元数据。

3. 接着浏览后续各节，了解其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 辅助功能最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误清单。

## 创建并分配议题

对于你的环境，Mission Control 是管理所有智能体的中心位置。你可以将任务分配给 Copilot 云智能体、监控任务，甚至在必要时调整和引导任务。现在，将创建高对比度模式的任务分配给 Copilot。

1. 转到存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![云智能体分配界面截图，其中自定义智能体和 Accessibility 已突出显示](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 就会在后台开始处理任务！

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是针对特定任务和领域定制的专用 AI 助手。借助自定义智能体，可以将团队专业知识和标准整理为可复用的智能体，帮助 Copilot 更有效地完成特定类型的工作。

你了解了：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

Copilot 正在后台实现高对比度模式。接下来，前往 Mission Control [监控和引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做准备][org-custom-agents]
- [为企业使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
