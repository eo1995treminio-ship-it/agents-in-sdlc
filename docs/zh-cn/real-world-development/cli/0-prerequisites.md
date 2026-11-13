---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

开始 Copilot CLI 练习前，需要先完成环境准备。将先创建 Tailspin Toys 存储库的个人副本，再启动一个 [codespace][codespaces]。下一节练习会使用其中集成的终端来安装并运行 Copilot CLI。

## 设置实验存储库

你将在自己的 Tailspin Toys 项目副本中工作。现在从 [模板存储库][tailspin-template] 创建副本。新存储库包含实验所需的全部文件。

1. 在新的浏览器窗口中，转到 [Tailspin Toys 模板][tailspin-template]。
2. 选择 **Use this template**，再选择 **Create a new repository**，创建自己的存储库副本。
3. 如果你是在 GitHub 或 Microsoft 主办的活动中参加本研讨会，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。
4. 记下所创建的存储库路径（`organization-or-user-name/repository-name`），稍后会在研讨会中用到。

> [!NOTE]
> **积压工作已准备就绪**
>
> 通过模板创建存储库时，系统会自动创建一组 GitHub 议题作为待办事项。整个研讨会都会使用这些议题，无需自行创建。

请使用一份全新的研讨会模板副本。它包含存储库说明、应用代码、测试、`quality-checks` 技能和待办事项。如果使用旧副本，请向导师确认其中包含所需文件。

## 创建 codespace

接下来，将使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是一个基于云的开发环境，可以直接在浏览器中编写、运行和调试代码。它提供功能完整的 IDE，并支持多种编程语言、扩展和工具。

1. 转到刚创建的存储库。
2. 选择 **Code**。
3. 选择 **Codespaces** 选项卡，再选择 **Create codespace on main**。
4. 等待 Codespace 设置完成。模板会自动安装项目依赖项、Playwright Chromium 和本地数据库。
5. 如果系统询问 **Do you trust the authors of the files in this folder?**，请选择 **Trust Folder & Continue**。
6. 在存储库根目录中打开终端并启动应用：

   ```bash
   npm run dev
   ```

7. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**，确认 Tailspin Toys 网站可以加载。
8. 返回终端，按 <kbd>Ctrl</kbd>+<kbd>C</kbd> 停止开发服务器。

> [!NOTE]
[codespaces]: https://github.com/features/codespaces

## 总结

准备工作已完成。本课中，你：

- 通过模板创建了自己的 Tailspin Toys 项目副本。
- 创建了 Codespace，并确认项目已准备就绪。

## 下一步

接下来，在 Codespace 中[安装 GitHub Copilot CLI][next-lesson]，并使用 GitHub 账户完成身份验证。

## 资源

- [GitHub Codespaces 概览][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 快速入门][codespaces-quickstart]

[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[tailspin-template]: https://github.com/github-samples/tailspin-toys
