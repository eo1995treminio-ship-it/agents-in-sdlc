---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を行い、プルリクエストを作成します。その間、別の作業を続けられます。

各演習では、まず cloud agent が従う custom instruction を追加し、GitHub Issue を割り当てて実装を任せます。エージェントの進め方をカスタム エージェントで調整し、agents ダッシュボードからセッションの進捗を確認して指示を出します。最後に、エージェントのプルリクエストをレビューし、結果を改善します。

## 演習

| 演習 | トピック | 内容 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. Custom instructions][ex1] | コンテキスト | cloud agent が従う custom instruction を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | GitHub Issue を Copilot cloud agent に割り当てる |
| [3. Custom agents][ex3] | 特化したエージェント | カスタム エージェントを確認して使用する |
| [4. Managing agents][ex4] | 監視 | エージェント セッションを監視して方向付ける |
| [5. Iterating][ex5] | レビュー | PR をレビューし、Copilot の作業を反復して改善し、次の手順を選ぶ |

## 前提条件

ワークショップに参加する前に、次の条件を満たしていることを確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっている

> [!TIP]
> 有料プランを利用していない場合、認定を受けた学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は Copilot Free 以外のすべてのプラン（**Copilot Student、Pro、Pro+、Business、Enterprise**）で利用できます。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## はじめる

**[演習 0: 前提条件から始めます →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
