---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエストの自動化を学びながら、Tailspin Toys の 2 つの変更を構築、検証、公開します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** は、ターミナルで GitHub Copilot をエージェント型のコーディング アシスタントとして利用できるようにします。グラフィカル エディターに切り替えずに作業を続けながら、コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。

このワークショップでは、Tailspin Toys の一連のワークフローに取り組みます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、プロジェクトを確認します。
2. 星評価の小さな変更を行い、ブラウザーで確認して、最初のプルリクエスト（PR）を手動で merge します。
3. フィルタリングの Issue から始め、Plan モードでアプローチを決め、Autopilot モードで構築して、Interactive モードでレビューします。
4. リポジトリの instruction を更新し、フィルタリングの作業に適用します。
5. 既存の `quality-checks` skill をカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol（MCP）サーバーを追加し、ブラウザーでフィルタリング機能を確認します。
7. 品質保証（QA）用のカスタム エージェントを作成し、要件、カバレッジ、検証結果をレビューします。
8. フィルタリングの変更全体をレビューし、フィルタリングの PR に Agent Merge を使います。
9. コンテキスト、モデル、共有、cloud agent への任意の委任に使う便利なスラッシュ コマンドを試します。

ワークショップを分かりやすく進めるため、PR は 2 つ作成します。星評価の変更と、instruction の更新、skill の更新、QA プロファイル、テストを含むフィルタリングの変更です。フィルタリングと品質確認のワークフローでは、同じ会話とブランチを使い、各ツールを学びながら作業を積み上げます。

## レッスン

| レッスン | トピック | 内容 |
| ------ | ----- | ---------------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、使い方を確認する |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR を merge する |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue から機能を計画し、Autopilot で構築して Interactive モードでレビューする |
| [4. Custom instructions で Copilot を導く][ex4] | コンテキスト | instruction を確認して更新し、フィルタリング機能に適用する |
| [5. quality-checks skill をカスタマイズして使う][ex5] | 繰り返し実行するチェック | 既存の skill を確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、フィルタリングの動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門的なプロファイルを作成して選び、検証結果をまとめる |
| [8. 機能 PR を作成して merge する][ex8] | レビューと merge | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュ コマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、さらに学ぶためのリソースを確認する |
| [任意: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイして Web サイトに接続する |

## 前提条件

ワークショップに参加する前に、次の条件を満たしていることを確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ラインの基本的な知識

> [!TIP]
> 有料プランを利用していない場合、認定を受けた学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始めます →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
